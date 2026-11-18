# Sécurité datacenter — HSM, Root of Trust, FPGA, sécurité physique, gestion des clés

> Guide technique en français — rédigé le 27/09/2026 pour Zelef, chef de service systèmes & énergies.
> Angle : chiffres, BOMs, dimensionnements, lien énergie (conso, refroidissement, alimentation).
> Références produits **vérifiées le 27/09/2026** par recherche web. Prix = ordres de grandeur, **à vérifier**
> sur devis. Référence introuvable = **« non trouvée au 27/09/2026 »**. Aucune invention.
> Complète le guide BIOS/UEFI/TPM existant de Zelef (focus datacenter/serveurs, sans dupliquer).

---

## Sommaire

- **Partie 1 — HSM** (sections 1 à 48) : principe, usages, form factors, fiches produits vérifiées,
  comparatifs, placement réseau, dimensionnement, sauvegardes, cérémonie, quorum MofN, HA, prix, TCO.
- **Partie 2 — Root of Trust** (sections 49 à 70) : TPM 2.0 serveur, secure boot, measured boot,
  attestation, BMC sécurisés, supply chain.
- **Partie 3 — FPGA** (sections 71 à 90) : principe, cartes AMD Alveo / Intel Agilex vérifiées,
  comparatif FPGA vs GPU vs ASIC, cas d'usage, grille de décision.
- **Partie 4 — Sécurité physique** (sections 91 à 104) : contrôle d'accès, cages, vidéo,
  intrusion, visiteurs, destruction de médias.
- **Partie 5 — KMS / gestion des clés** (sections 105 à 120) : cycle de vie, rotation, rôles,
  HSM↔KMS, Vault, EJBCA, sauvegardes.
- **Partie 6 — Conformité** (sections 121 à 128) : ISO 27001, PCI DSS, RGPD, eIDAS (généraliste,
  pas de conseil juridique).
- **Partie 7 — Checklists pratiques** (sections 129 à 138) : mise en service serveur, BMC, HSM,
  cérémonie, runbooks incident.
- **Partie 8 — Lien énergie** (sections 139 à 146) : consos, onduleurs, refroidissement,
  continuité des services de clés.
- **Partie 9 — À venir, annonces vérifiées au 27/09/2026** (sections 147 à 157) : PQC NIST,
  confidential computing.
- **Glossaire** (40 termes), **Quiz** (10 questions + réponses), **Pièges terrain** (25).

---

# PARTIE 1 — HSM : LE COFFRE-FORT DES CLÉS

## 1. Principe : pourquoi un HSM plutôt qu'un fichier de clés

Un HSM (Hardware Security Module) est un boîtier ou une carte dédiée qui **génère, stocke et utilise
des clés cryptographiques sans jamais les laisser sortir en clair**. La clé privée d'une autorité de
certification, la clé maîtresse qui chiffre une base de données, la clé de signature d'un firmware :
si elle est dans un fichier sur disque, quiconque lit le disque (ou la sauvegarde, ou le snapshot
VM) la possède. Dans un HSM, la clé naît à l'intérieur du composant sécurisé, y reste, et seules
des **opérations** (signer, déchiffrer) sont exposées via une API. Le matériel est conçu pour
**s'auto-détruire logiquement** (effacement des clés) en cas d'ouverture ou d'attaque physique.

Règle de base : **toute clé dont la compromission coûte plus cher que le HSM doit être dans un HSM.**
Clé racine d'AC, clé de chiffrement des sauvegardes, clé de signature de code, clé DNSSEC KSK :
oui. Clé TLS d'un serveur web interne renouvelée tous les 90 jours : discutable, un KMS logiciel
suffit souvent.

## 2. Génération de clés : TRNG et entropie matérielle

Un HSM embarque un **générateur d'aléa vrai (TRNG)** basé sur un phénomène physique (bruit
thermique, oscillateurs en anneau). Les clés générées dans le HSM ne dépendent donc pas de
l'entropie de l'OS hôte — point critique sur les VM cloud au démarrage, historiquement pauvres en
entropie. Les HSM sérieux combinent une source de bruit physique et un DRBG conforme
**NIST SP 800-90A** (ex. CTR-DRBG). Exemple vérifié le 27/09/2026 : les Thales Luna annoncent un
générateur « conçu pour être conforme AIS 20/31 (DRG.4) avec source de bruit matérielle + CTR-DRBG
NIST 800-90A » (product brief Luna Network HSM, cpl.thalesgroup.com).

En pratique : générer la clé **dans** le HSM (et non l'y importer) est la méthode recommandée pour
les clés racines. L'import existe (key wrapping, enveloppe chiffrée) pour la migration, mais chaque
import est une fenêtre de risque.

## 3. Tamper resistance : ce que le boîtier fait quand on l'attaque

La résistance à l'effraction (tamper resistance) repose sur plusieurs couches :

- **Capteurs** : interrupteurs d'ouverture du capot, capteurs de température (attaque au froid pour
  figer la RAM), capteurs de tension/fréquence (glitch), parfois capteurs de rayonnement.
- **Mémoire volatile protégée par batterie** : les clés vivent en SRAM alimentée en permanence.
  Coupure d'alimentation anormale ou déclenchement d'un capteur → **effacement immédiat**
  (zeroization).
- **Revêtement** : résine époxy ou mesh conducteur autour du composant : toute tentative de
  perçage/attaque par sonde coupe des pistes et déclenche l'effacement.
- **Blindage** : contre les attaques par canaux auxiliaires (analyse de consommation, émissions
  électromagnétiques).

C'est exactement ce qui justifie la certification **FIPS 140-2/140-3 niveau 3 ou 4** (voir
section 5) : le niveau 3 exige une réponse active à l'effraction.

## 4. Tamper evidence : scellés et traçabilité

Complément de la résistance : la **preuve** d'effraction. Les HSM réseau sont livrés avec des
scellés inviolables sur les vis et les jointures du châssis. À la réception, on photographie et on
consigne l'état des scellés dans le procès-verbal d'installation. Un scellé dégradé par la chaleur
ou le temps n'est pas forcément une compromission, mais c'est un **drapeau rouge** à documenter
avec le vendeur. Les journaux d'audit internes du HSM (tamper-evident logs, parfois chaînés par
hash comme sur le YubiHSM 2) complètent le dispositif : toute tentative laisse une trace.

## 5. FIPS 140-2 / 140-3 : les 4 niveaux, ce qu'ils garantissent vraiment

FIPS 140 est la certification de référence (NIST, reconnue internationalement). Ce qu'il faut en
retenir, sans jargon inutile :

| Niveau | Exigence clé | Usage typique |
|---|---|---|
| 1 | Algorithmes validés, pas d'exigence physique particulière | Logiciel chiffrant (ex. module validé dans un OS) |
| 2 | Scellés inviolables, contrôle d'accès par rôles | Clés USB chiffrées, tokens |
| 3 | **Détection + réponse active à l'effraction**, effacement des clés, authentification forte des opérateurs | **HSM réseau/PCIe** (c'est le niveau visé par Luna, Utimaco, nShield) |
| 4 | Protection contre les attaques physiques poussées (analyse environnementale large, canaux auxiliaires) | HSM haut de gamme, militaire |

Points honnêtes :

- FIPS 140-2 est en **fin de vie** : les nouvelles validations se font en **FIPS 140-3**
  (ISO/IEC 19790:2021). Un HSM « FIPS 140-2 niveau 3 » reste valable, mais vérifiez la date de la
  validation sur le site du NIST (CMVP).
- FIPS valide une **version précise** de firmware : mettre à jour le firmware peut exiger une
  re-validation ou passer par un mode « non validé » temporaire. À planifier.
- La validation porte sur le **module cryptographique**, pas sur tout le produit ni sur votre
  déploiement.

## 6. Common Criteria EAL : le complément européen

Common Criteria (ISO/IEC 15408) évalue le produit contre un **profil de protection** avec des
niveaux d'assurance EAL1 à EAL7. Pour les HSM, on croise typiquement **EAL4+**. Vérifié le
27/09/2026 : Utimaco annonce ses HSM généralistes « FIPS 140-2 niveau 3 (niveau 4 en cours) »
et une certification **Common Criteria** de son HSM par le CCN espagnol (communiqué Utimaco,
septembre 2026, hsm.utimaco.com). En pratique PME/datacenter : FIPS 140-2/3 L3 suffit dans 95 %
des appels d'offres ; Common Criteria devient exigé sur les marchés publics sensibles et
l'administration (ex. exigences ANSSI/CPM en France — à vérifier au cas par cas).

## 7. PCI HSM, eIDAS : certifications sectorielles

- **PCI HSM** (anciennement PCI PTS HSM) : obligatoire si le HSM manipule des clés liées aux
  cartes bancaires (PIN, clés de chiffrement de données carte). Thales propose une gamme
  **Luna Payment HSM** dédiée ; Utimaco a **Atalla** (voir section 25).
- **eIDAS** (UE) : pour la signature électronique qualifiée et l'horodatage qualifié, le
  dispositif de création de signature doit être **QSCD** (Qualified Signature Creation Device) —
  en pratique un HSM certifié Common Criteria. Si vous visez la signature qualifiée, le choix du
  HSM est contraint par cette exigence.

## 8. Usage 1 — PKI et autorités de certification

C'est l'usage historique n°1. La **clé privée de l'AC racine** (root CA) est générée dans le HSM
lors d'une cérémonie (voir section 42), n'en sort jamais, et ne sert qu'à signer les certificats
des AC intermédiaires — quelques signatures par an. Les AC intermédiaires/émettrices peuvent
aussi être sur HSM (souvent une partition du même boîtier, voir section 38). Dimensionnement :
quelques opérations par jour → le plus petit modèle suffit. Outils : **EJBCA** (open source,
voir section 114) ou Microsoft AD CS branché en PKCS#11 sur le HSM. Pour une PME, un **YubiHSM 2**
(~650 €, voir section 29) suffit pour une AC interne ; pour une AC publique ou qualifiée eIDAS,
appliance réseau FIPS 140-2 L3 / Common Criteria obligatoire.

## 9. Usage 2 — Chiffrement bases de données et disques (TDE)

Transparent Data Encryption (Oracle TDE, SQL Server, PostgreSQL via pgcrypto ou chiffrement au
niveau stockage) : la clé maîtresse (master key) qui protège les DEK est stockée dans le HSM, les
opérations de déchiffrement des DEK passent par PKCS#11. Intérêt : même avec les fichiers de base
et les sauvegardes, sans le HSM les données sont inexploitables. Volume : quelques opérations par
minute (rotation, démarrage d'instance) → faible. Point de vigilance : **la disponibilité du HSM
conditionne le démarrage des bases** — prévoir le cluster HA (section 45) et la procédure de
redémarrage après coupure électrique (section 145).

## 10. Usage 3 — Signature électronique et code signing

Signer des documents (factures, contrats), des binaires, des images de firmware, des conteneurs.
Le HSM garantit que la clé de signature ne peut pas être copiée par un développeur ou exfiltrée
par un malware du poste de build. Cas datacenter : signature des **firmwares/updates internes**,
signature des images de déploiement (PXE, golden images). Volume : de quelques signatures/jour
(code) à des milliers/heure (facturation électronique à grande échelle) — dimensionner en
conséquence (section 36). Pour le code signing « moderne » (Sigstore/Fulcio), le HSM reste
pertinent pour la clé racine ; les certificats éphémères sont gérés par l'infrastructure.

## 11. Usage 4 — Horodatage qualifié (RFC 3161)

Une autorité d'horodatage (TSA) signe des jetons d'horodatage prouvant l'existence d'un document
à un instant T. La clé de la TSA vit dans le HSM, et l'horloge doit être fiable (NTP sécurisé,
idéalement NTS). Usage PME : horodatage des journaux d'audit, des sauvegardes, des signatures.
Volume : potentiellement élevé (un jeton par événement) → prévoir le débit. Produits : la plupart
des TSA logicielles (ex. OpenTSA) se branchent en PKCS#11 sur n'importe quel HSM.

## 12. Usage 5 — DNSSEC

La **KSK** (Key Signing Key) d'une zone DNSSEC est typiquement générée et conservée dans un HSM ;
la ZSK (Zone Signing Key) peut tourner plus souvent, parfois en logiciel. Pour un hébergeur ou
une entreprise gérant ses zones, c'est un usage à faible volume mais à forte criticité (la KSK
compromise = usurpation de tout le domaine). Outils : BIND 9 / Knot DNS avec PKCS#11, ou
PowerDNS. Bonne nouvelle : un petit HSM suffit.

## 13. Usage 6 — Paiement : PIN, DUKPT, PCI HSM

Le monde du paiement a ses propres HSM (« payment HSM ») avec des jeux de commandes spécifiques
(vérification de PIN, génération de CVV, DUKPT pour les TPE). **Ne pas utiliser un HSM généraliste
pour du paiement** si la conformité PCI l'exige : il faut un HSM certifié **PCI HSM**. Acteurs
vérifiés le 27/09/2026 : **Thales Luna Payment HSM**, **Utimaco Atalla** (dont la version cloud
**Azure Payment HSM v2**, annoncée le 17/09/2026 avec Marvell LiquidSecurity — public preview
Western US et Western Europe). Débit typique : jusqu'à ~2 000 vérifications PIN/seconde sur un
Luna Payment HSM (product brief Thales).

## 14. Usage 7 — 5G, blockchain, IoT

- **5G** : les HSM gèrent les clés d'authentification des abonnés (Milenage, TUAK — supportés par
  les Luna, voir product brief) et les certificats des fonctions réseau.
- **Blockchain/custody** : génération et stockage des clés de portefeuilles (BIP32/SLIP10
  supportés par Luna), signature de transactions. Les dépositaires d'actifs numériques utilisent
  des HSM en quorum MofN.
- **IoT/industrie** : AC dédiée aux certificats d'équipements (provisioning en usine), signature
  des firmwares OTA. Volume potentiellement massif à l'émission → dimensionner.

## 15. Form factors : panorama (réseau, PCIe, USB, cloud)

| Format | Principe | Latence typique | Partage | Idéal pour |
|---|---|---|---|---|
| **Appliance réseau** | Boîtier 1U/2U, accès via LAN chiffrée | ~1-5 ms (réseau) | Oui, multi-apps/partitions | Datacenter, PKI centrale, mutualisation |
| **Carte PCIe** | Carte dans le serveur, accès local | ~µs-ms (bus local) | Non (dédiée au serveur) | Serveur applicatif à fort débit, latence critique |
| **USB/nano** | Clé USB interne au serveur | ~ms (USB) | Via logiciel (yubihsm-connector) | AC racine offline, PME, budget serré |
| **Cloud HSM** | HSM dédié chez le provider, accès réseau | ~ms (selon région) | Oui (cluster) | Charges cloud-natives, sans investissement CAPEX |

Le choix se fait sur trois axes : **débit/latence**, **mutualisation**, **modèle économique**
(CAPEX vs OPEX). Les sections 16 à 19 détaillent.

## 16. Appliance réseau : architecture et cas d'usage

L'appliance (ex. Thales Luna Network HSM, Utimaco SecurityServer en version LAN, Entrust nShield
Connect) est un serveur durci 19" avec le module crypto inviolable, des alimentations
redondantes, et une pile réseau minimale. Les applications se connectent via un **client**
(PKCS#11, JCE, CAPI/CNG) sur un **canal chiffré et mutuellement authentifié** (NTLS chez Thales).
Le HSM vit dans un **VLAN dédié**, derrière un firewall qui n'autorise que les clients déclarés
(voir section 35). Avantages : mutualisation entre applications via **partitions** isolées
cryptographiquement (jusqu'à 20 sur un Luna Payment HSM), haute disponibilité par clustering,
administration centralisée. Inconvénients : prix d'entrée le plus élevé, latence réseau.

## 17. Carte PCIe : architecture et cas d'usage

La carte (ex. Luna PCIe HSM A700/A750/A790 ou S700/S750/S790, Utimaco Se en version PCIe) s'enfiche
dans un slot du serveur applicatif. L'application y accède **en local**, latence minimale, pas de
dépendance réseau. Idéal pour : serveur TLS à très fort débit (terminaison SSL avec clé protégée),
serveur de signature à haute cadence, appliance de sécurité embarquant sa crypto. Limites :
**un serveur = un HSM** (pas de mutualisation simple), la HA exige une carte par nœud, et la
carte consomme un slot PCIe + ~14-18 W (chiffres Luna PCIe vérifiés : 14 W typique, 18 W max).
En virtualisation, prévoir le **PCI passthrough** (IOMMU/VT-d) vers la VM dédiée.

## 18. USB/nano : architecture et cas d'usage

Le **YubiHSM 2** (vérifié le 27/09/2026) : format nano USB-A (12 × 13 × 3,1 mm, 1 g), 20 mA
moyens / 30 mA max, RSA 2048/3072/4096, ECC P-256/P-384/P-521/Curve25519, AES, SHA, HMAC,
interfaces PKCS#11 / Microsoft CNG (KSP) / SDK natif C et Python. Il se branche sur un **port USB
interne** du serveur et y reste. Un connecteur réseau (yubihsm-connector) permet de le partager
entre serveurs via sessions mutuellement authentifiées. Cas d'usage roi : **clé d'AC racine
offline** (la clé ne sort jamais, le boîtier tient dans un coffre), PKI interne PME, AD CS.
Limites : débit modeste (dizaines d'opérations/s, pas des milliers), pas de clustering natif —
on duplique en générant la même clé sur 2 modules via cérémonie, ou on accepte le point unique
de défaillance pour une racine offline (acceptable : elle ne sert que quelques fois par an).

## 19. Cloud HSM : principe et limites de confiance

Le provider vous alloue un **HSM physique dédié** (single-tenant) dans son datacenter ;
vous en êtes le seul client (Crypto Officer), le provider n'a pas accès à vos clés. Références
vérifiées le 27/09/2026 :

| Service | Prix indicatif vérifié | Certification | Notes |
|---|---|---|---|
| **AWS CloudHSM** | ~1,45–1,50 $/h/HSM | FIPS 140-2 L3 | PKCS#11, JCE, OpenSSL ; clusters multi-AZ |
| **Azure Dedicated HSM** | **4,85 $/h/HSM** (page tarifs Azure) | FIPS 140-2 L3 | Régions limitées ; basé sur Thales Luna |
| **Azure Managed HSM** | ~3,20 $/h (indicatif, à vérifier) | FIPS 140-2 L3 | Géré, multi-tenant logique |
| **GCP Cloud HSM** (via Cloud KMS) | ~1 $/mois/clé + requêtes (indicatif) | FIPS 140-2 L3 | Intégré à Cloud KMS |

Limites honnêtes : vous **faites confiance au provider** pour l'intégrité physique du datacenter
et l'absence de backdoor d'administration ; la latence dépend de la région ; le coût mensuel
d'un cluster HA (2-3 HSM) tourne autour de **2 200–3 500 $/mois** pour AWS CloudHSM — à comparer
au TCO d'une appliance achetée (section 48). Pour une PME sans datacenter, c'est souvent le
meilleur rapport sécurité/effort.

## 20. Thales Luna Network HSM — fiche vérifiée le 27/09/2026

(Source : product briefs Thales / cpl.thalesgroup.com ; chiffres du brief « BBv37 avril 2024 »
et du brief T-Series.)

- **Formats** : appliance 19" 1U (482,6 × 533,4 × 43,8 mm), 12,7 kg, **double alimentation
  hot-swap**.
- **Électrique** : 100-240 V, 50-60 Hz ; **110 W max, 84 W typique** ; dissipation
  376 BTU/h max. Plage 0-35 °C en fonctionnement.
- **Fiabilité** : MTBF **171 308 h** (~19,5 ans).
- **Modèles** : deux gammes (A : authentification par mot de passe ; S : authentification
  multifacteur PED), 3 niveaux de performance chacune. Ex. T-Series : T-2000 (1 400 tps
  RSA-2048, 3 000 tps ECC P-256) → T-5000 (14 000 tps RSA-2048, 16 000 tps ECC P-256).
- **Partitions** : jusqu'à 20 partitions isolées cryptographiquement (modèle paiement),
  politiques de partitions flexibles.
- **Crypto** : RSA, DSA, DH, ECC (ECDSA/ECDH/Ed25519/ECIES, courbes NIST/Brainpool),
  AES/AES-GCM, 3DES, SHA-1/2/3, SM2/SM3/SM4, BIP32/SLIP10, Milenage/TUAK (5G).
- **RNG** : source de bruit matérielle + CTR-DRBG NIST 800-90A, conception visant AIS 20/31.
- **PQC** : **Luna PQC Functionality Module** mentionné dans la documentation récente
  (mécanismes post-quantiques dans le HSM — voir section 152).
- **APIs** : PKCS#11, JCA/JCE, Microsoft CAPI/CNG, OpenSSL, REST d'administration ;
  Functionality Modules (code custom dans l'enclave).
- **Certifications visées** : FIPS 140-2 niveau 3 (historique de la gamme ; vérifier la
  validation exacte du modèle/firmware sur le site NIST CMVP — **à vérifier** au moment
  de l'achat).

## 21. Thales Luna PCIe HSM — fiche vérifiée le 27/09/2026

(Source : product brief Luna PCIe HSM, « BBv37 avril 2024 ».)

- **Format** : carte PCIe **low profile**, PCIe 2.0, 69,6 × 167 × 18,7 mm.
- **Électrique** : **18 W max, 14 W typique** — à prévoir dans le budget thermique du serveur.
- **Fiabilité** : MTBF **997 508 h** (~114 ans — valeur constructeur, indicateur, pas promesse).
- **Modèles** : A700/A750/A790 (mot de passe) et S700/S750/S790 (PED multifacteur).
  Performances : **RSA-2048 : 1 000 / 5 000 / 10 000 tps** ; **ECC P-256 : 2 000 / 10 000 /
  22 000 tps** ; **AES-GCM : 2 000 / 10 000 / 17 000 tps** selon modèle.
- **Mémoire** : 4 / 32 / 64 Mo selon modèle (nombre de clés/objets stockables).
- **Cas d'usage** : serveur applicatif dédié, appliance de sécurité embarquée, PKI locale
  à fort débit.

## 22. Thales Luna USB HSM et DPoD (cloud)

- **Luna USB HSM** : format portable pour **stockage offline** des clés (sauvegarde,
  transport sécurisé, racine d'AC hors-ligne). Petit volume, usage ponctuel.
- **DPoD (Data Protection on Demand)** : plateforme cloud de Thales — HSM à la demande
  sans acheter de matériel, marketplace de services (Luna Cloud HSM, gestion de clés).
  Modèle OPEX, à comparer avec AWS CloudHSM/Azure (section 19). Tarifs : **à vérifier**
  sur devis.

## 23. Utimaco SecurityServer Se Gen2 — fiche vérifiée le 27/09/2026

(Source : datasheet constructeur ; communiqué Utimaco septembre 2026.)

- **Formats** : **carte PCIe** ou **appliance réseau (LAN)** — même logiciel, deux
  emballages.
- **Crypto** : RSA, DSA, ECDSA (courbes NIST et Brainpool), DH/ECDH, AES, 3DES, MAC/CMAC/
  HMAC, SHA-1/SHA-2/**SHA-3** (depuis firmware 4.10), RIPEMD, DRNG déterministe.
- **Authentification** : **« n out of m »** (quorum MofN natif), séparation des tâches,
  smartcard pour l'authentification forte des opérateurs.
- **Administration** : à distance extensive, SNMP, mises à jour de firmware à distance,
  simulateur logiciel pour les tests d'intégration.
- **Standards** : PKCS#11, Microsoft CSP/CNG, JCE.
- **Certifications** : **FIPS 140-2 niveau 3** (niveau 4 annoncé « en cours » — à vérifier),
  **PCI PTS HSM V3**, certification **Common Criteria** (CCN espagnol, septembre 2026).

## 24. Utimaco u.trust General Purpose HSM — la gamme conteneurisée (vérifié)

(Source : communiqué Utimaco, septembre 2026 — vérifié le 27/09/2026.)

- **Architecture conteneurisée** inspirée du cloud : jusqu'à **31 tenants/conteneurs**,
  plusieurs partitions PKCS#11 par conteneur — séparation applicative fine.
- **Modèles** : Se100, Se2k, Se5k, Se15k, Se40k (montée en charge par licence/field upgrade),
  stockage interne ou externe des clés.
- **Crypto-agile** : support des algorithmes **post-quantiques sans changement de matériel**
  (voir section 152 : package **Quantum Protect**).
- **Packages solutions** : 5G Protect, Blockchain Protect, Double Key Encryption,
  Quantum Protect.
- **Disponible** : en appliance LAN, carte PCIe, ou **as-a-service**.

## 25. Utimaco Atalla AT1000 — le HSM paiement (à vérifier)

La gamme **Atalla** est la référence historique du paiement. Vérifié le 27/09/2026 :
**Azure Payment HSM v2** combine la technologie **Marvell LiquidSecurity**, le module
**Utimaco Atalla Payments** et Azure — public preview depuis le **17/09/2026**
(Western US, Western Europe), ciblant PCI sans gérer de HSM physiques (communiqués Marvell,
Microsoft, Utimaco). Pour un datacenter on-prem avec besoin paiement : Atalla AT1000 en
appliance — caractéristiques détaillées **non trouvées au 27/09/2026**, demander le datasheet
au vendeur.

## 26. Entrust nShield — panorama (détails à vérifier)

Entrust (ex-nCipher) **nShield** est le troisième grand acteur historique avec Thales et
Utimaco : gammes **nShield Connect** (réseau), **nShield Edge** (USB), **nShield 5c**
(PCIe). Points différenciants connus : architecture **Security World** (clés partagées et
protégées entre plusieurs HSM via cartes d'opérateurs), outil **nShield Monitor**.
Fiche technique détaillée et tarifs : **non trouvés au 27/09/2026** (constructeur sur devis)
— **à vérifier** avant tout comparatif d'achat.

## 27. Futurex — panorama (détails à vérifier)

**Futurex** (Texas, USA) : HSM généralistes et de paiement (séries Vectra, Excrypt),
positionnés sur la performance et le marché américain. Intégration PKCS#11/JCE classique,
clustering. Fiche détaillée : **non trouvée au 27/09/2026** — **à vérifier** si un appel
d'offres les inclut.

## 28. Marvell LiquidSecurity — le HSM paiement cloud (vérifié)

**Marvell LiquidSecurity** est la brique matérielle derrière **Azure Payment HSM v2**
(voir section 25) : des HSM conçus pour le cloud, exposés en service managé. Tendance 2026 :
le HSM « cloud-scale » opéré par l'hyperscaler, le client gardant la souveraineté des clés.
À suivre pour les workloads paiement qui refusent le CAPEX.

## 29. YubiHSM 2 — fiche vérifiée le 27/09/2026 + prix

(Sources : Yubico via revendeurs — prix constatés septembre 2026 ; specs constructeur.)

| Caractéristique | Valeur vérifiée |
|---|---|
| Format | Nano **USB-A**, 12 × 13 × 3,1 mm, **1 g** — tient dans un port USB interne |
| Alimentation | **20 mA moyen / 30 mA max** (négligeable) |
| Algos | RSA 2048/3072/4096, ECC P-256/P-384/P-521/Curve25519, **Ed25519**, AES 128/192/256, SHA-1/256/384/512, HMAC |
| Interfaces | PKCS#11, Microsoft CNG (KSP), SDK natif C/Python, yubihsm-connector (partage réseau) |
| Audit | Journal chaîné par hash (**tamper-evident**) |
| Version FIPS | **YubiHSM 2 FIPS** : FIPS 140-2 (niveau global 2, physique niveau 3) |
| Prix constatés | **~650 €** version standard ; **~949 €** version FIPS (revendeur EU, 2026) — **à vérifier** au jour de l'achat |
| PQC | Firmware 2.4+ : PQC **en test pilote**, pas de support complet (suivi pqca, 09/2026) |

C'est le **meilleur rapport sécurité/prix** pour : AC racine offline, AD CS en PME,
signature de code d'équipe, labo. Ce n'est PAS un remplacement d'appliance réseau pour des
milliers d'opérations/seconde.

## 30. Tableau comparatif général des HSM

| Critère | Thales Luna Network | Thales Luna PCIe | Utimaco Se Gen2 / u.trust GP | YubiHSM 2 | Cloud HSM (AWS/Azure) |
|---|---|---|---|---|---|
| Format | Appliance 1U | Carte PCIe LP | Appliance ou PCIe | Nano USB | Service managé |
| Partage réseau | Oui (partitions) | Non | Oui (31 conteneurs u.trust) | Via connector | Oui (cluster) |
| Débit RSA-2048 | 1 400–14 000 tps | 1 000–10 000 tps | Gamme Se100→Se40k (à vérifier) | Dizaines de tps | ~10 000 ops/s/HSM (indicatif) |
| FIPS | 140-2 L3 (gamme) | 140-2 L3 | 140-2 L3 (L4 en cours) | 140-2 (version FIPS) | 140-2 L3 |
| Conso | 84 W typ. | 14 W typ. | À vérifier (~50-100 W appliance) | 0,1 W | 0 (chez le provider) |
| Prix d'entrée | ~15–30 k€ (indicatif) | ~5–12 k€ (indicatif) | ~10–25 k€ (indicatif) | 650–949 € | ~1 100 €/mois/HSM (AWS) |
| Idéal pour | Datacenter mutualisé | Serveur dédié haut débit | Datacenter, PQC-ready | PME, racine offline | Cloud-native, OPEX |

Prix matériels : **ordres de grandeur constatés sur le marché, à vérifier sur devis** — les
constructeurs ne publient pas de tarifs publics.

## 31. Tableau des performances chiffrées (tps = transactions/s)

Chiffres **constructeur vérifiés** (briefs avril 2024) — maximums soutenus en conditions de
test, pas des garanties SLA :

| Produit / modèle | RSA-2048 | RSA-4096 | ECC P-256 | AES-GCM |
|---|---|---|---|---|
| Luna Network T-2000 | 1 400 | 350 | 3 000 | — |
| Luna Network T-5000 | 14 000 | 3 500 | 16 000 | — |
| Luna PCIe A700/S700 | 1 000 | — | 2 000 | 2 000 |
| Luna PCIe A750/S750 | 5 000 | — | 10 000 | 10 000 |
| Luna PCIe A790/S790 | 10 000 | — | 22 000 | 17 000 |
| Luna Payment HSM | — | — | — | 2 000 vérif. PIN/s |

Règle d'ingénieur : **diviser par 2** les chiffres constructeur pour le dimensionnement
(marge réseau, latence applicative, pics), et **ne jamais dimensionner au pic théorique**.

## 32. Tableau des certifications par produit (vérifié le 27/09/2026)

| Produit | FIPS 140-2/3 | Common Criteria | PCI HSM | eIDAS/QSCD |
|---|---|---|---|---|
| Thales Luna (gammes) | L3 (vérifié, gamme) | Selon modèle (à vérifier) | Gamme Payment dédiée | Selon modèle (à vérifier) |
| Utimaco Se/u.trust GP | L3, L4 en cours (vérifié 09/2026) | Oui — CCN Espagne 09/2026 (vérifié) | PCI PTS HSM V3 (vérifié) | Selon modèle (à vérifier) |
| YubiHSM 2 FIPS | 140-2 (global L2, physique L3) | Non trouvé | Non | Non |
| AWS CloudHSM | 140-2 L3 | — | Via configuration | Non |

Toujours vérifier la **version exacte firmware/matériel** sur la liste CMVP du NIST au moment
de l'achat : une certification « de gamme » ne couvre pas automatiquement votre modèle.

## 33. Où placer le HSM : schéma datacenter

```
                    ┌─────────────────────────────────────────────────┐
                    │              DATACENTER / SALLE SÉCURISÉE       │
                    │                                                 │
  Internet / WAN ──▶│  ┌─────────┐   ┌──────────┐   ┌──────────────┐   │
                    │  │ Firewall│──▶│  VLAN    │──▶│  HSM réseau  │   │
                    │  │ périm.  │   │  MGMT    │   │  (cluster    │   │
                    │  └─────────┘   │  SÉCURISÉ│   │   2 nœuds)   │   │
                    │                │ 10.99.0.0│   └──────┬───────┘   │
                    │                │  /24     │          │ PKCS#11   │
                    │                └──────────┘          │ (TLS mutuel)
                    │       ┌──────────────┐              │           │
                    │       │ VLAN SERVEURS│◀─────────────┘           │
                    │       │ applicatifs  │   seuls les clients     │
                    │       │ (PKI, AD CS, │   déclarés (IP+cert)    │
                    │       │  Vault, TSA) │                         │
                    │       └──────────────┘                         │
                    │                                                 │
                    │  ┌──────────────┐   ┌──────────────────────┐    │
                    │  │ Coffre-fort  │   │ Baie 42U verrouillée │    │
                    │  │ YubiHSM racine│  │ alim. secourue (UPS) │    │
                    │  │ (AC offline) │   │ double PDU A+B       │    │
                    │  └──────────────┘   └──────────────────────┘    │
                    └─────────────────────────────────────────────────┘
```

Principes : le HSM **n'est jamais sur le VLAN utilisateurs**, jamais exposé sur Internet,
jamais sur le même VLAN que les serveurs web. Un **VLAN crypto dédié** (/24 suffit), firewall
avec règles explicites par IP cliente + authentification mutuelle TLS/PKCS#11. La racine
offline (YubiHSM ou Luna USB) dort dans un **coffre**, pas dans un serveur allumé.

## 34. Où placer le HSM : schéma PME (budget serré)

```
  ┌─ Serveur PKI/AD CS (ou VM dédiée) ─────────────────────┐
  │  - Windows Server + AD CS  OU  EJBCA sur Linux          │
  │  - YubiHSM 2 sur port USB INTERNE (invisible de l'ext.) │
  │  - yubihsm-connector si partage entre 2 serveurs        │
  │  - Sauvegarde : 2e YubiHSM (même clé, cérémonie) au coffre│
  └────────────────────────────────────────────────────────┘
  Coût : ~1 300 € (2 × YubiHSM) + serveur existant.
  Quand passer à l'appliance : > 100 signatures/heure, besoin HA,
  audit externe, ou clés de plus de 3 applications.
```

## 35. Zone réseau du HSM : VLAN dédié, firewall

Checklist réseau minimale, quel que soit le format :

- [ ] VLAN ou sous-réseau **dédié** au HSM (pas de routage vers Internet).
- [ ] Firewall stateful : n'autoriser que les **IP des clients PKCS#11** + le poste
      d'administration, sur les **ports documentés** du constructeur uniquement.
- [ ] **TLS mutuel** (NTLS ou équivalent) entre clients et HSM : certificats clients
      générés à l'initialisation, stockés hors du HSM si besoin.
- [ ] Pas de DHCP sur ce VLAN : IP statiques, documentation à jour.
- [ ] Supervision : SNMP/syslog du HSM vers le SIEM, alertes sur échecs d'authentification
      (tentatives = incident potentiel).
- [ ] Le poste d'administration du HSM est une **machine dédiée**, durcie, sans Internet,
      accès physique contrôlé.

## 36. Dimensionnement : méthode de calcul ops/s

La question « quel modèle de HSM ? » se répond en 4 étapes :

1. **Inventorier les opérations** : pour chaque application, type d'opération (signature RSA,
   déchiffrement, génération), taille de clé, fréquence moyenne et **pic**.
2. **Convertir en ops/s au pic** : ex. une AC qui émet 3 600 certificats/heure en pic de
   renouvellement = 1 signature/s. Un serveur de signature de factures à 36 000 factures/heure
   = 10 signatures/s. Une TSA à 1 M de jetons/jour = ~12/s en moyenne, ~50/s en pic.
3. **Appliquer le facteur de sécurité ×2** minimum (pics, croissance 3 ans, overhead réseau).
4. **Choisir le modèle** dont le débit constructeur ÷ 2 couvre le besoin (voir section 31).

Note : les opérations **symétriques** (AES) sont 10 à 100× moins coûteuses que l'asymétrique ;
le dimensionnement se fait presque toujours sur **RSA/ECDSA**. Et préférez **ECDSA P-256**
à RSA-2048 pour les nouveaux usages : ~2× plus rapide à sécurité équivalente (chiffres Luna :
2 000 vs 1 000 tps sur A700).

## 37. Dimensionnement : 3 exemples chiffrés

**Exemple A — PME, PKI interne (AD CS + EJBCA), 500 utilisateurs.**
Émission : ~2 000 certificats/an + renouvellements. Soit < 1 signature/heure en moyenne,
pic à ~20 signatures/heure lors d'un renouvellement massif. **Besoin : < 1 op/s.**
→ **YubiHSM 2** largement suffisant. Coût : ~650 €.

**Exemple B — ETI, signature de documents + TDE + Vault, 3 applications.**
Signature : 50 000 documents/jour = ~0,6/s moyen, pic 5/s. TDE : négligeable. Vault :
unseal et rotation, ~10 ops/s en pic. **Besoin : ~20 ops/s avec marge.**
→ **Luna Network T-2000** (1 400 tps RSA-2048 ÷ 2 = 700 ops/s utiles) ou **Utimaco Se2k**,
largement couverts, avec 3 partitions (une par usage). Coût indicatif : 15–25 k€.

**Exemple C — Prestataire, TSA + signature de masse, 10 M opérations/jour.**
10 M/jour = ~116/s moyen, pic ×4 = ~460/s. **Besoin : ~1 000 ops/s avec marge.**
→ **Luna Network T-5000** (14 000 tps ÷ 2 = 7 000 utiles) ou **2× T-2000 en cluster HA**.
Coût indicatif : 25–50 k€ + redondance.

## 38. Partitions et multi-tenancy

Une **partition** est un HSM logique isolé dans le HSM physique : clés, politiques et
administrateurs séparés, comme si c'était un HSM indépendant. Intérêts :

- **Séparation des usages** : partition « AC racine », partition « TDE », partition
  « signature » — une compromission d'un mot de passe opérateur n'expose qu'une partition.
- **Multi-entités** : héberger les clés de plusieurs clients/entités sur un seul boîtier
  (modèle MSP/hébergeur).
- **Séparation des rôles** : chaque partition a ses propres Crypto Officers.

Limites : les partitions **partagent le débit** du HSM physique et, selon les modèles, un
même firmware. Pour une séparation forte (ex. racine offline vs production), préférez deux
boîtiers physiques. Chez Utimaco u.trust, le découpage va jusqu'à **31 conteneurs**
(voir section 24).

## 39. APIs : PKCS#11, le socle universel

**PKCS#11** (Cryptoki) est l'API standard d'accès aux HSM : ouvrir une session, lister les
objets, signer, déchiffrer, générer. 95 % des logiciels (EJBCA, OpenSSL via engine/provider,
Java via SunPKCS11, BIND, OpenTSA) parlent PKCS#11. Points pratiques :

- Le HSM est vu comme un **slot** contenant un **token** ; chaque partition = un slot.
- Les clés peuvent être marquées **non extractibles** (CKA_EXTRACTABLE=false) : le
  minimum vital pour une clé racine.
- Les **sessions** et les **PIN** (User PIN, SO PIN) : le SO (Security Officer) administre,
  l'utilisateur normal utilise. Ne jamais utiliser le SO au quotidien.
- Tester avec `pkcs11-tool` (OpenSC) avant d'intégrer : `pkcs11-tool --module
  /usr/lib/libCryptoki2.so -L` liste les slots.

## 40. APIs : CNG/CAPI, JCE, OpenSSL engine, REST

- **Microsoft CNG (KSP) / CAPI** : pour AD CS, IIS, les applis Windows. Le YubiHSM 2
  fournit un KSP ; Thales/Utimaco fournissent leurs CSP/KSP.
- **Java JCE** : via le provider **SunPKCS11** (fichier de config pointant sur la lib
  PKCS#11 du HSM) ou les providers natifs (ex. Luna JCP). Indispensable pour EJBCA,
  les applis Java.
- **OpenSSL engine/provider** : permet à nginx, Apache, stunnel d'utiliser la clé du HSM
  sans la copier. En OpenSSL 3.x, le modèle **provider** remplace les engines.
- **REST d'administration** : les appliances modernes exposent une API REST pour le
  provisionnement et le monitoring (pas pour les opérations crypto sensibles, qui restent
  en PKCS#11). Utile pour l'automatisation (Terraform/Ansible — avec parcimonie et
  revue, voir piège n°7).

## 41. Sauvegarde de clés : principes

Un HSM protège les clés contre le vol, **pas contre la perte** : panne matérielle, incendie,
erreur d'effacement. Sans sauvegarde, la clé est perdue à jamais — et avec elle, toutes les
données chiffrées. Les méthodes :

1. **HSM de sauvegarde dédié** (ex. Luna Backup HSM, ou un 2e boîtier) : réplication
   chiffrée de partition à partition, clés jamais en clair hors HSM. La méthode reine.
2. **Sauvegarde chiffrée** : export des clés **wrappées** (chiffrées par une KEK) vers un
   support externe, stocké en coffre. La KEK elle-même est partagée en quorum MofN
   (section 43).
3. **Duplication à la génération** : générer la clé simultanément sur 2 HSM pendant la
   cérémonie (pratique courante avec 2 YubiHSM pour une racine).

Règle : **tester la restauration** au moins une fois par an (voir section 138). Une sauvegarde
jamais testée n'est pas une sauvegarde.

## 42. Cérémonie de génération de clés (key ceremony)

La cérémonie est la procédure **formelle, documentée et auditée** de génération d'une clé
critique (typiquement la racine d'AC). Déroulé type :

1. **Préparation** : script écrit et relu, salle sécurisée, HSM initialisé et vérifié
   (scellés, firmware), participants convoqués (voir quorum).
2. **Participants** : au minimum 2-3 personnes de confiance + un **témoin/auditeur**
   (interne ou externe). Chacun a un rôle écrit.
3. **Génération** : la clé est générée **dans** le HSM (TRNG interne), paramètres
   vérifiés à l'écran (algorithme, taille — ex. RSA-4096 ou ECDSA P-384 pour une racine).
4. **Sauvegarde** : duplication vers le HSM de backup, ou partage MofN (section 43).
5. **Procès-verbal** : qui, quand, où, numéros de série, empreintes de clés publiques,
   signatures des participants. Ce PV est un **document d'audit** (eIDAS, WebTrust).

Durée : 1 à 3 heures pour une racine. Coût : essentiellement du temps humain — mais c'est
le moment le plus critique de la vie de la PKI. **Ne jamais générer une racine « vite fait »
sur un poste admin.**

## 43. Quorum M of N : le principe

Le **quorum MofN** (ex. 3-of-5) : la clé (ou le secret permettant de la restaurer) est
découpée en N parts (smartcards, PED keys), et **M parts** sont nécessaires pour la
reconstituer. Personne seul ne peut restaurer la clé ; il faut la collusion de M personnes.
En pratique :

- Les parts sont remises à des **custodians** nommés (direction, RSSI, admin senior...),
  stockées en **lieux séparés** (coffres différents, sites différents).
- M est typiquement la **majorité** (2-of-3, 3-of-5) : compromis entre sécurité et
  disponibilité (si un custodian est injoignable).
- Chez Thales, le **PED** (PIN Entry Device) gère les clés de partition sur smartcards ;
  chez Utimaco, le **« n out of m »** est natif avec smartcards d'opérateurs.

Piège classique : mettre les N parts dans le **même coffre** « pour simplifier ». Le quorum
ne vaut que par la séparation physique et organisationnelle.

## 44. Custodians, PED, smartcards : qui porte quoi

| Rôle / objet | Fonction | Bonne pratique |
|---|---|---|
| **Crypto Officer (CO)** | Administre une partition au quotidien | 2 CO minimum, jamais le même que l'auditeur |
| **Custodian** | Détient une part du quorum MofN | Personne de confiance, hors équipe d'exploitation si possible |
| **PED / smartcard** | Support physique des parts et de l'authentification forte | Cartes numérotées, inventaire annuel |
| **Auditeur/témoin** | Assiste aux cérémonies, vérifie les PV | Indépendant de l'exploitation |
| **SO (Security Officer)** | Initialise le HSM (niveau boîtier) | Utilisé uniquement à l'initialisation |

Règle d'or : **séparation des tâches** — celui qui génère ne sauvegarde pas seul, celui qui
exploite ne détient pas le quorum seul.

## 45. HA, clustering, reprise après sinistre

Un HSM est un **point critique** : s'il tombe, les signatures/chiffrements s'arrêtent. Les
mécanismes :

- **Cluster HA** : 2+ HSM réseau en **réplication synchrone** (ex. Luna HA : un groupe,
  un membre actif, les autres en miroir ; bascule automatique côté client). Prévoir 2
  boîtiers minimum en production.
- **Client multi-HSM** : le client PKCS#11 connaît les 2 membres et bascule
  automatiquement.
- **Site distant** : un 3e HSM (ou le backup) sur le site de secours pour le PRA,
  réplication asynchrone ou manuelle.
- **RPO/RTO** : pour des clés (petit volume), RPO ≈ 0 envisageable en synchrone ;
  RTO = temps de bascule client (secondes à minutes). À tester (section 138).

Attention : la HA protège de la **panne**, pas de la **compromission logique** (une clé
compromise est répliquée compromise). D'où la sauvegarde offline déconnectée.

## 46. Durée de vie : batterie, MTBF, fin de vie

- **Batterie** : les clés en SRAM sont alimentées par une pile interne (durée typique
  5-10 ans selon modèles). Une batterie faible = alerte à traiter **avant** la panne :
  à son épuisement, le HSM s'efface (comportement de sécurité normal). **Contrôler
  l'état batterie dans la supervision** et planifier le remplacement (intervention
  constructeur, souvent avec cérémonie).
- **MTBF** : 171 308 h (Luna Network) / 997 508 h (Luna PCIe) — valeurs constructeur
  vérifiées, utiles pour comparer, pas des garanties.
- **Fin de vie** : à la mise au rebut, **zeroization** documentée + destruction physique
  si le modèle le permet (section 100). Ne jamais revendre un HSM sans effacement certifié.

## 47. Prix indicatifs : tableau (ordres de grandeur, à vérifier sur devis)

| Produit | Prix d'achat indicatif | Récurrent annuel indicatif |
|---|---|---|
| YubiHSM 2 (standard / FIPS) | 650 € / 949 € (constaté 2026) | 0 € |
| Luna PCIe HSM (A700→A790) | ~5–12 k€ | Support ~15-20 %/an |
| Luna Network HSM (T-2000→T-5000) | ~15–35 k€ | Support ~15-20 %/an |
| Utimaco Se Gen2 / u.trust (Se2k→Se15k) | ~10–30 k€ | Support ~15-20 %/an |
| AWS CloudHSM (1 HSM) | 0 € CAPEX | ~13 k$/an (~1,45 $/h) |
| AWS CloudHSM (cluster 2 HSM, 2 AZ) | 0 € CAPEX | ~26 k$/an |
| Azure Dedicated HSM | 0 € CAPEX | ~42 k$/an (4,85 $/h — tarif public vérifié) |

Tous les prix matériels sont **à vérifier** : les constructeurs vendent via partenaires,
avec des remises selon volumes et support. Le support annuel (15-20 %) inclut firmware et
remplacement — **ne pas le négliger** dans le budget.

## 48. TCO : calcul exemple sur 5 ans

**Scénario** : ETI, 1 appliance réseau + 1 backup, support 18 %/an.

| Poste | Année 1 | Années 2-5 (par an) | Total 5 ans |
|---|---|---|---|
| 2× appliance (25 k€ pièce, indicatif) | 50 000 € | — | 50 000 € |
| Support 18 % | 9 000 € | 9 000 € | 45 000 € |
| Installation + cérémonie (presta) | 8 000 € | — | 8 000 € |
| Formation (2 pers.) | 4 000 € | — | 4 000 € |
| Électricité (2× 84 W × 24h × 365 × 0,20 €/kWh) | ~300 € | ~300 € | ~1 500 € |
| **Total** | **~71 300 €** | **~9 300 €** | **~108 500 €** |

Comparaison : le même service en **AWS CloudHSM** (2 HSM) ≈ 26 k$/an ≈ **130 k$ sur 5 ans**,
sans CAPEX ni gestion matérielle. Le **seuil de rentabilité** de l'achat se situe vers
**3-4 ans** d'utilisation continue — en-deçà, le cloud HSM gagne ; au-delà, l'appliance
gagne. Pour un usage < 2 ans ou très variable : cloud HSM sans hésiter.

# PARTIE 2 — ROOT OF TRUST : LA CONFIANCE DEPUIS LE SILICIUM

## 49. Root of Trust : définition et pourquoi c'est la base

Le **Root of Trust (RoT)** est le composant (matériel, firmware ou les deux) en qui le système
a **implicitement confiance** pour démarrer la chaîne de vérification. Tout le reste —
bootloader, OS, applications — est vérifié **à partir de lui**. S'il est compromis, tout
l'édifice s'effondre : un rootkit dans le firmware survit à la réinstallation de l'OS, au
changement de disque, à tout. En datacenter, le RoT concerne **chaque serveur** : des centaines
de machines avec BMC, BIOS/UEFI, cartes réseau à firmware — autant de surfaces d'attaque.

On distingue classiquement :
- **RTM (Root of Trust for Measurement)** : mesure chaque étape du démarrage (hash dans le TPM).
- **RTS (Root of Trust for Storage)** : protège les clés/secrets (TPM, HSM).
- **RTR (Root of Trust for Reporting)** : signe les attestations (clé EK/AK du TPM).

## 50. Chaîne de confiance : du silicium à l'OS

```
  [Silicium / Boot ROM immuable]
        │ vérifie (signature)
        ▼
  [Bootloader / firmware initial] ──mesure──▶ [TPM PCR]
        │ vérifie
        ▼
  [UEFI / BIOS] ──mesure──▶ [TPM PCR]
        │ vérifie
        ▼
  [Bootloader OS (shim, GRUB)] ──mesure──▶ [TPM PCR]
        │ vérifie
        ▼
  [Noyau OS] ──mesure──▶ [TPM PCR]
        │
        ▼
  [Applications / conteneurs]
```

Deux opérations à chaque étape : **vérifier** (la signature est-elle valide ? → secure boot,
qui **bloque** si non) et **mesurer** (quel est le hash de ce qui a démarré ? → measured boot,
qui **enregistre** sans bloquer). La vérification sans mesure = on sait que c'est signé, pas
**quoi exactement** a démarré. La mesure sans vérification = on enregistre tout, y compris le
malware. Il faut les deux.

## 51. Complément au guide BIOS/UEFI/TPM de Zelef : ce qu'on ne redit pas

Zelef dispose déjà d'un guide **BIOS/UEFI/TPM** (bases : activer le TPM, secure boot sur
poste, BitLocker, etc.). Cette partie **ne duplique pas** : elle couvre ce qui est
**spécifique datacenter/serveurs** — TPM sur serveurs (dTPM/fTPM/DICE), measured boot et
attestation à distance en production, BMC sécurisés (le point aveugle n°1), supply chain et
pièces contrefaites. Pour les fondamentaux (qu'est-ce qu'un PCR, comment activer le TPM dans
l'UEFI), se reporter au guide existant.

## 52. TPM 2.0 sur serveur : dTPM, fTPM, DICE

Sur serveur, trois implémentations :

| Type | Principe | Avantages | Limites |
|---|---|---|---|
| **dTPM** (discret) | Puce dédiée (ex. Infineon OPTIGA) sur la carte mère | Isolation physique maximale, certifié Common Criteria | Coût, une puce par serveur |
| **fTPM** (firmware) | TPM émulé dans le firmware (AMD fTPM, Intel PTT) | Coût nul, déjà présent | Partage le TEE du CPU ; vulnérabilités historiques (ex. failles AMD fTPM — patcher) |
| **DICE** | Identité dérivée à chaque boot (chaîne de certificats) | Idéal pour BMC/périphériques sans TPM complet | Écosystème plus jeune |

En datacenter : exiger le **dTPM 2.0** sur les serveurs critiques (PKI, HSM-adjacents,
hyperviseurs), accepter le fTPM sur le parc standard **à condition de maintenir les
microcodes/firmwares à jour**. Vérifier la présence : `tpm2_pcrread` sous Linux, ou via
l'inventaire Redfish du BMC.

## 53. PCR : les registres de mesure, en pratique

Les **PCR** (Platform Configuration Registers) sont 24 registres du TPM où s'accumulent les
mesures (hash étendu : PCR_new = hash(PCR_old || mesure)). Répartition standard :

- **PCR 0-7** : firmware/UEFI (code, configuration, secure boot).
- **PCR 8-15** : OS (bootloader, noyau, initrd).
- **PCR 16-23** : usage libre (applications, conteneurs).

Usage datacenter n°1 : **sceller un secret au PCR** (seal) — ex. la clé de déchiffrement du
disque (LUKS) n'est utilisable que si les PCR 0-7 correspondent au firmware attendu. Si le
firmware est modifié (rootkit UEFI), les PCR changent, le secret reste scellé. Usage n°2 :
**politiques d'accès** — n'autoriser le déverrouillage que sur un état mesuré connu.

## 54. Secure Boot UEFI : focus serveurs datacenter

Sur serveur, le Secure Boot UEFI vérifie la signature de chaque binaire de boot (shim →
GRUB → noyau). Spécificités datacenter :

- **Clés** : utiliser le **mode custom** avec vos propres clés (PK/KEK/db) plutôt que les
  clés Microsoft par défaut si vous voulez contrôler ce qui boote — indispensable pour des
  images maison signées.
- **PXE/iPXE** : le boot réseau doit lui aussi être signé ; un serveur qui netboot une image
  non signée contourne tout le secure boot local.
- **Cartes d'extension** : les **Option ROM** (cartes réseau, contrôleurs RAID) sont
  vérifiées par le secure boot UEFI — d'où l'importance de firmwares signés par le vendeur
  (voir section 62).
- **Inventaire** : auditer régulièrement l'état Secure Boot du parc via Redfish/BMC
  (un serveur avec Secure Boot désactivé « pour dépanner » et jamais réactivé = classique).

## 55. Measured Boot : la différence qui compte

Le measured boot **n'empêche pas** de démarrer un composant non signé : il **enregistre**
son hash dans les PCR. Intérêt : **détection** plutôt que prévention. En production :

- Les mesures sont collectées par un **agent** (ex. Keylime, ou l'infrastructure
  d'attestation du cloud) et comparées à des **valeurs de référence** (golden measurements).
- Tout écart = alerte (firmware modifié, bootloader remplacé).
- Combiné au secure boot : le secure boot **bloque** le connu-mauvais (non signé), le
  measured boot **détecte** le signé-mais-modifié ou le downgrade.

Sans infrastructure de collecte et de comparaison, le measured boot ne sert à rien : c'est
un investissement **système**, pas une case à cocher.

## 56. Attestation à distance : le protocole

L'attestation à distance permet à un **vérificateur** (serveur central) de s'assurer qu'une
machine distante a démarré dans un état sain, **sans lui faire confiance a priori** :

1. Le vérificateur envoie un **nonce** (défi aléatoire anti-rejeu).
2. Le TPM de la machine signe (**quote**) les PCR + le nonce avec sa clé d'attestation (AK).
3. Le vérificateur contrôle la signature, la fraîcheur du nonce, et **compare les PCR aux
   valeurs de référence**.
4. Décision : sain → la machine rejoint le cluster / reçoit ses secrets ; non sain →
   quarantaine.

C'est le mécanisme derrière l'**admission des nœuds** dans les clusters sensibles
(Kubernetes, hyperviseurs) et la **distribution de secrets** (un secret n'est délivré qu'à
une machine attestée — voir Trustee/Keylime, section 156).

## 57. Attestation : qui vérifie quoi (le vérificateur)

Le vérificateur (verifier) a besoin de trois choses :

- **La chaîne de confiance du TPM** : le certificat de la clé EK, signé par le fabricant
  (Infineon, STMicro...), pour prouver que le TPM est authentique.
- **Les valeurs de référence** : hashes attendus du firmware, bootloader, noyau — à
  maintenir à **chaque mise à jour** (sinon faux positifs en cascade après un patch).
- **Une politique** : quels PCR sont critiques, quelle tolérance (ex. PCR 8-15 variables
  selon le noyau, PCR 0-7 stricts).

Solutions : **Keylime** (open source, CNCF), les services d'attestation des clouds,
**Intel Trust Authority** (service managé). En 2026, l'attestation devient aussi la brique
de base du **confidential computing** (voir sections 154-156).

## 58. Intel Boot Guard / AMD Platform Secure Boot (à vérifier)

Les constructeurs CPU proposent des racines de confiance matérielles au tout premier stade :

- **Intel Boot Guard** : vérifie la signature du firmware (BIOS/UEFI) dès le reset, avec une
  clé programmée en usine (fuses). En mode « Verified Boot », un firmware non signé ne
  démarre pas.
- **AMD Platform Secure Boot (PSB)** : équivalent AMD, la clé est programmée chez l'OEM et
  lie le CPU à la plateforme.

Ces mécanismes **complètent** le TPM (ils sont en amont). Détails d'activation par
plateforme et par OEM (HPE, Dell, Supermicro) : **à vérifier** dans la documentation du
serveur — c'est souvent une option à commander ou à activer en usine, pas un réglage
rétroactif.

## 59. BMC : le talon d'Achille des serveurs

Le **BMC** (Baseboard Management Controller : iLO chez HPE, iDRAC chez Dell, XCC chez
Lenovo, OpenBMC sur les plateformes ouvertes) est un **ordinateur dans l'ordinateur** :
SoC ARM séparé, son propre OS (Linux), sa propre pile réseau, accès **total** au serveur
(allumage/extinction, console KVM, montage d'images ISO, lecture des capteurs, et souvent
accès au bus système). Un BMC compromis = contrôle total persistant, **invisible depuis
l'OS** (l'OS ne voit pas le BMC). C'est la cible n°1 des attaquants sur un datacenter, et
le point le plus négligé : mots de passe par défaut jamais changés, firmwares jamais
patchés, BMC exposés sur le réseau de production.

## 60. Durcissement BMC : checklist technique

- [ ] **Mot de passe** : changer le défaut à la réception (chaque BMC un mot de passe
      unique, stocké au coffre-fort de mots de passe). Désactiver les comptes inutiles.
- [ ] **Réseau dédié** : le BMC sur un **VLAN management** séparé, jamais sur le VLAN
      de production, jamais exposé sur Internet. Firewall : seuls les postes d'admin.
- [ ] **Firmware** : mettre à jour à la réception puis suivre les bulletins (les failles
      iLO/iDRAC sont régulières et critiques).
- [ ] **Services** : désactiver Telnet/HTTP non chiffré, IPMI 1.5 (authentification
      faible), ne garder que HTTPS/Redfish + SSH si besoin.
- [ ] **IPMI** : si utilisé, imposer **IPMI 2.0 + chiffrement**, mots de passe longs ;
      le hash RAKP d'IPMI se craque offline — un BMC sur réseau non fiable = compromis.
- [ ] **Certificats** : remplacer le certificat auto-signé par un certificat de votre PKI
      interne (qui vit idéalement sur HSM — boucle bouclée).
- [ ] **Journaux** : syslog du BMC vers le SIEM ; alerter sur les connexions anormales.
- [ ] **2FA** : activer l'authentification multifacteur sur l'interface web quand
      disponible.

## 61. OpenBMC, Redfish, DMTF : les standards

- **Redfish** (DMTF) : API REST standard de gestion des serveurs — remplace progressivement
  IPMI. Exigez-le sur les nouveaux serveurs : requêtes HTTPS + JSON, inventaire, mise à
  jour firmware, gestion des comptes.
- **OpenBMC** : stack BMC open source (Linux Foundation) adoptée par les hyperscalers et
  des ODM (Supermicro, etc.). Avantage : **auditabilité** du code du BMC, mises à jour
  maîtrisées. Si vous achetez des serveurs ODM, demandez OpenBMC.
- **DMTF SPDM** (Security Protocol and Data Model) : attestation des **composants**
  (cartes réseau, disques, GPU) — le serveur vérifie l'identité et le firmware de ses
  périphériques. À exiger sur les achats 2026+ pour les composants critiques.

## 62. Mises à jour firmware signées

Tout firmware (UEFI/BMC, cartes réseau, disques, FPGA, alimentations !) doit être **signé
par le vendeur** et **vérifié avant application**. Procédure :

- Télécharger uniquement depuis le site officiel du constructeur (vérifier le hash/TLS).
- Ne jamais appliquer un firmware « trouvé sur un forum ».
- Maintenir un **registre des versions** par serveur (inventaire automatisé via Redfish).
- Tester sur un serveur pilote avant déploiement massif.
- Après mise à jour : **re-mesurer** (les PCR changent → mettre à jour les valeurs de
  référence d'attestation, sinon tout le parc passe en alerte).

## 63. Supply chain : pièces contrefaites, le risque réel

Le risque supply chain n'est pas théorique : disques, barrettes mémoire, transceivers,
alimentations et même cartes mères **contrefaites ou reconditionnées vendues comme neuves**
circulent sur le marché gris. Conséquences : firmwares modifiés, portes dérobées, fiabilité
dégradée, et **invalidation des certifications**. Les vecteurs : achats hors circuit
officiel pour « faire des économies », pièces de rechange d'occasion, sous-traitants peu
regardants.

## 64. Supply chain : SBOM, attestations constructeur

Contre-mesures 2026 :

- **Acheter en circuit officiel** (constructeur ou distributeur agréé) pour tout composant
  critique ; conserver factures et numéros de série.
- **SBOM** (Software Bill of Materials) : exiger du vendeur la liste des composants
  logiciels/firmwares — c'est une exigence croissante (EO 14028 aux US, CRA en UE).
- **Attestation SPDM** (section 61) : vérification cryptographique des composants à
  l'allumage.
- **Scellés et traçabilité** : à la réception, vérifier l'intégrité des emballages et la
  concordance des numéros de série (voir section 65).

## 65. Réception serveur : contrôle à la livraison

- [ ] Emballage intact, scellés constructeur présents.
- [ ] Numéros de série (châssis, carte mère, disques, alimentations) = bon de livraison.
- [ ] Premier allumage **en zone de quarantaine** (pas directement en production).
- [ ] Mise à jour UEFI/BMC vers les versions validées, Secure Boot activé, TPM provisionné.
- [ ] Mots de passe BMC changés, inventaire Redfish initial capturé (référence).
- [ ] Photo des étiquettes et archivage du PV de réception.

## 66. Serveurs d'occasion/reconditionnés : précautions

Le reconditionné fait sens économiquement, mais impose un **reconditionnement de confiance** :

- Effacement crypto des disques (section 101) ou remplacement pur et simple.
- **Reflash complet** des firmwares (UEFI, BMC) depuis les images officielles.
- Réinitialisation d'usine du TPM (`tpm2_clear`) et du BMC.
- Vérification SPDM/attestation des composants si disponible.
- Ne jamais acheter de **HSM d'occasion sans effacement certifié** par un professionnel
  (voir le marché du reconditionné — exiger le PV de reset et tester avant production).

## 67. Cas terrain : BMC compromis (retour d'expérience type)

Scénario vécu dans l'industrie (synthèse de cas publics) : un parc de serveurs avec BMC en
**DHCP sur le VLAN de production**, mots de passe **par défaut** (constructeur connus),
firmware vieux de 3 ans. Un attaquant ayant un pied sur le réseau scanne le port 623 (IPMI),
se connecte en admin, monte une **image ISO** via le KVM virtuel et redémarre les serveurs
dessus — prise de contrôle totale, **persistante** (le BMC survit à la réinstallation),
**invisible** (aucun agent OS ne voit le BMC). Coût de remédiation : reflash de tout le
parc, changement de tous les mots de passe, segmentation réseau — plusieurs semaines.
Leçon : le BMC est un **serveur à part entière** avec son cycle de patch, pas un « détail
hardware ».

## 68. Tableau : mécanismes RoT par couche

| Couche | Mécanisme | Ce qu'il garantit | Outil de vérification |
|---|---|---|---|
| Silicium | Boot Guard / PSB (à vérifier) | Le 1er firmware est signé | Fuses usine, doc OEM |
| Firmware UEFI | Secure Boot | Seuls les binaires signés bootent | Setup UEFI, Redfish |
| Firmware UEFI | Measured Boot | Le boot est enregistré (PCR 0-7) | `tpm2_pcrread`, Keylime |
| OS | Secure/measured boot (shim/GRUB) | Noyau signé et mesuré (PCR 8-15) | Attestation distante |
| BMC | Firmware signé, mots de passe | Le BMC n'est pas compromis | Redfish, audit firmware |
| Composants | SPDM | Cartes/disques authentiques | Attestation SPDM |
| Clés | TPM seal, HSM | Secrets liés à l'état mesuré | Politiques TPM |

## 69. Ce que le TPM ne protège pas (limites honnêtes)

- Un TPM ne protège pas contre un **firmware signé mais vulnérable** (il mesure, il ne
  juge pas la qualité du code).
- Il ne protège pas la **mémoire vive** en cours d'exécution (attaques DMA, cold boot —
  d'où le chiffrement mémoire des TEE, voir partie 9).
- Un **fTPM** partage le silicium avec le reste : des vulnérabilités ont existé, patcher
  est obligatoire.
- Sans **infrastructure d'attestation**, les mesures restent des nombres que personne ne
  vérifie.
- Le TPM ne remplace pas le **HSM** : il protège les secrets d'**une** machine, pas les
  clés d'une organisation.

## 70. Lien avec le HSM : sceller des clés au TPM

Architecture type : la **clé maîtresse** vit dans le HSM (datacenter) ; chaque serveur en
dérive une clé locale **scellée à ses PCR** via son TPM. Au boot, le serveur demande au
HSM/KMS sa clé enveloppe, le TPM la descelle uniquement si les PCR sont conformes, et la
clé n'existe en clair qu'en RAM. Compromis élégant : révocation centrale possible (via le
HSM), vol du disque inutile (clé scellée), et pas de clé maîtresse qui traîne sur les
serveurs. C'est le pattern derrière le **sealed storage** des OS modernes et l'auto-unseal
de Vault (section 113).

# PARTIE 3 — FPGA : LE MATÉRIEL REPROGRAMMABLE

## 71. FPGA : le principe en 5 minutes

Un **FPGA** (Field-Programmable Gate Array) est une puce dont la logique est **reconfigurable
après fabrication** : au lieu d'exécuter des instructions (CPU/GPU), on y **câble un circuit
dédié** à l'application. Résultat : parallélisme massif, latence déterministe (cycles
d'horloge comptés, pas d'OS ni d'interruptions), et efficacité énergétique par opération.
Le prix : un développement **matériel** (plus long, plus cher en ingénierie) et des volumes
limités. Devise : « le FPGA fait en silicium ce que le CPU fait en logiciel, et l'ASIC fait
en dur ».

## 72. LUT, CLB, DSP, BRAM : le vocabulaire minimal

- **LUT** (Look-Up Table) : la brique de base — une petite table qui implémente n'importe
  quelle fonction logique à N entrées. La taille d'un FPGA se mesure en **kLUT/M LUT**.
- **CLB** (Configurable Logic Block) : regroupement de LUT + bascules.
- **DSP slice** : multiplieurs-accumulateurs câblés en dur — critiques pour le traitement
  du signal et l'IA (ex. 10 848 DSP sur l'Alveo V80).
- **BRAM/UltraRAM** : mémoire embarquée rapide à côté de la logique (ex. 673 Mb sur V80).
- **Transceivers** : SerDes haute vitesse vers l'extérieur (Ethernet, PCIe) — ex. 112 Gb/s
  PAM4 sur Agilex 7.
- **Bitstream** : le fichier de configuration qui « programme » le FPGA (à protéger : c'est
  de la propriété intellectuelle, et un bitstream modifié = porte dérobée matérielle —
  **le chiffrer et l'authentifier**, les FPGA modernes le supportent).

## 73. Le flot de développement : HDL, synthèse, place & route, bitstream

1. **Description** : en VHDL/Verilog/SystemVerilog (ou HLS, voir section 74).
2. **Synthèse** : traduction en netlist de portes.
3. **Place & route** : placement sur le silicium — l'étape longue (minutes à heures).
4. **Génération du bitstream** : avec chiffrement/authentification si activés.
5. **Validation** : simulation, puis test sur carte.

Cycle typique : **semaines à mois** pour un design sérieux, contre des heures pour du
logiciel. D'où la règle : on ne met sur FPGA que ce qui est **stable** (le protocole ne
change pas tous les mois) et **critique** (latence, débit, conso).

## 74. HLS : programmer en C/C++

Le **HLS** (High-Level Synthesis) permet de décrire l'accélérateur en C/C++ (ou OpenCL)
au lieu de VHDL/Verilog. Outils : **AMD Vitis HLS**, **Intel HLS Compiler**. Gain :
développement 3-5× plus rapide, accessible aux développeurs logiciel. Limite : le résultat
est moins optimal qu'un design RTL manuel, et il faut quand même **penser matériel**
(pipeline, parallélisme, mémoire). Pour un datacenter qui débute : HLS pour le prototype,
RTL pour la production si la perf l'exige.

## 75. AMD Alveo V80 — fiche vérifiée le 27/09/2026

(Sources : product brief AMD via mouser.com/amd.com ; prix via wccftech — vérifiés le
27/09/2026.)

- **Puce** : AMD Versal HBM **XCV80** (7 nm), 2,6 M LUT, **10 848 DSP slices**,
  673 Mb de mémoire embarquée (132 Mb BRAM + 541 Mb UltraRAM).
- **Mémoire** : **32 Go HBM2e** (2 stacks), **820 Go/s** de bande passante — le point fort
  de la carte (workloads mémoire-intensifs).
- **Réseau** : 4× **QSFP56** (2× 100G par port), 3× moteurs crypto 400G (bulk crypto, IPsec,
  MACsec), MAC Ethernet jusqu'à 600G.
- **Host** : PCIe **Gen4 x16** ou 2× Gen5 x8 ; extension MCIO en PCIe Gen5.
- **Format/conso** : FH 3/4 longueur, double slot, **190 W TDP**, refroidissement passif
  (flux d'air du serveur requis).
- **SKU** : A-V80-P64G-PQ-G. **Prix public constaté : 9 495 $** (MSRP).
- **Cibles** : HPC, data analytics, sécurité réseau (NGFW), fintech, calcul haute
  performance gourmand en mémoire.

## 76. AMD Alveo U55C, U250, U280 : le reste de la gamme (contexte)

- **Alveo U55C** : prédécesseur direct du V80 (HBM aussi) — le V80 annonce ~2× la bande
  passante mémoire et la densité logique, 4× la bande passante réseau (brief AMD).
- **Alveo U250/U280** : cartes à base d'UltraScale+, visées réseau/stockage/IA — encore
  très présentes en occasion/reconditionné, intéressantes pour débuter à moindre coût.
- Gamme complète et prix : **à vérifier** chez les distributeurs (Mouser, etc.) au moment
  de l'achat — les prix varient fortement selon les volumes.

## 77. Intel/Altera Agilex 7 — fiche vérifiée le 27/09/2026

(Sources : datasheet « Agilex 7 FPGAs and SoCs Device Overview » 2025 via mouser.com ;
articles NetworkWorld, The Register ; documentation Altera FPGA AI Suite — vérifiés le
27/09/2026. Note : l'activité FPGA d'Intel opère sous la marque **Altera**.)

- **Architecture** : chiplet (EMIB), gravure fine (famille issue des Stratix/Arria).
- **Connectivité** : **PCIe 5.0**, **CXL**, transceivers jusqu'à **116 Gb/s** (tuile F),
  le plus rapide du marché FPGA au lancement.
- **Séries** : F-Series (logique + transceivers), I-Series (optimisée), M-Series
  (avec HBM2e).
- **Blocs durs** : contrôleurs mémoire, DSP câblés (hard IP) — performance sans
  consommer la logique programmable.
- **Cas d'usage** : SmartNIC, data centers, télécoms, finance — workloads réseau et
  bande passante.

## 78. Intel SmartNIC N6000 / N6001-PL — fiche vérifiée le 27/09/2026

(Source : documentation Altera FPGA AI Suite, github.com/altera-fpga — vérifié le
27/09/2026.)

- **N6000** : plateforme de développement d'accélération basée sur **Agilex** (issue des
  IPU « Oak Springs Canyon » : 2× 100 Gb/s d'accélération d'infrastructure).
- **N6001-PL** : variante SmartNIC **sans contrôleur Ethernet** (le pipeline réseau est
  dans le FPGA), disponible via partenaires ODM.
- **FPGA AI Suite** : stack Intel/Altera pour l'**inférence IA sur FPGA** (avec
  OpenVINO), supportant les cartes Agilex 7 PCIe via **Open FPGA Stack (OFS)**.
- Positionnement : **DPU/SmartNIC** — décharger le CPU du traitement réseau/sécurité/
  stockage (voir section 81).

## 79. Autres acteurs : Achronix, Microchip, Lattice (panorama)

- **Achronix** (Speedster7t) : FPGA haute performance avec NoC 2D, GDDR6 — challenger sur
  le réseau/IA.
- **Microchip** (PolarFire) : FPGA **basse consommation**, flash-based (configuration
  non volatile, démarrage instantané, bonne tenue aux radiations) — intéressant pour
  l'embarqué et les équipements réseau sobres.
- **Lattice** : petits FPGA basse consommation (contrôle, glue logic) — pas des
  accélérateurs datacenter, mais présents dans les BMC et cartes de management.
- Détails produits 2026 : **à vérifier** au cas par cas — le marché FPGA bouge vite et
  les gammes se renouvellent.

## 80. FPGA vs GPU vs ASIC : le tableau de décision

| Critère | FPGA | GPU | ASIC |
|---|---|---|---|
| Latence | **Déterministe, très basse** (ns-µs) | Variable (µs-ms, batching) | Optimale |
| Débit | Très élevé sur pipeline fixe | Très élevé (parallélisme massif) | Maximal |
| Flexibilité | **Reprogrammable** | Totale (logiciel) | Nulle (figé) |
| Développement | Long (RTL/HLS), cher | Rapide (CUDA, frameworks) | Très long, très cher (NRE) |
| Conso/op | **Excellente** sur workload fixe | Bonne | Optimale |
| Coût unitaire | Moyen (~10 k€/carte) | Moyen-élevé | Faible en volume, NRE énorme |
| Volumes | 1 à 10 000 | 1 à millions | > 100 000 pour rentabiliser |
| Cas roi | Réseau, trading, crypto, protocoles | IA training/inférence, HPC | Minage, grand volume figé |

Règle : **GPU** si le workload évolue vite (IA) ; **FPGA** si le workload est stable et
la latence/déterminisme critique (réseau, trading) ; **ASIC** si les volumes justifient
le NRE.

## 81. Cas d'usage 1 — Réseau : packet processing, SmartNIC/DPU

Le cas d'usage datacenter n°1 du FPGA : **décharger le CPU**. Jusqu'à ~80 % des cycles CPU
peuvent partir en overhead I/O sur des workloads cloud-natifs (chiffre avancé par Intel
pour justifier ses IPU). Le FPGA/SmartNIC prend en charge : switching, routage, firewall,
**IPsec/MACsec** (moteurs 400G câblés sur l'Alveo V80), encapsulation (VxLAN, GENEVE),
équilibrage de charge. Résultat : les cycles CPU retournent aux applications — c'est un
**gain capacitaire** mesurable en nombre de serveurs évités. Pour Zelef : un parc qui
chiffre beaucoup (IPsec site-à-site massif) est un candidat naturel.

## 82. Cas d'usage 2 — Crypto : TLS, IPsec/MACsec, PQC

- **TLS** : terminaison à haut débit avec clés protégées (le FPGA peut dialoguer avec un
  HSM pour les opérations de clé privée, ou embarquer ses propres clés dans du stockage
  sécurisé).
- **IPsec/MACsec** : les moteurs câblés (3× 400G sur V80) traitent le chiffrement
  ligne-rate sans charger le CPU.
- **PQC** : les algorithmes post-quantiques (ML-KEM, ML-DSA) sont **plus lourds** que
  RSA/ECDH (clés et signatures plus grosses, plus de calculs) — le FPGA est un candidat
  naturel pour les accélérer, et sa **reprogrammabilité** est précieuse pendant la
  transition (les algos peuvent encore évoluer — cf. affaire HAWK, section 153).

## 83. Cas d'usage 3 — Trading haute fréquence

En finance, la latence se mesure en **nanosecondes** et se monnaye. Les FPGA traitent les
flux de marché (décodage des protocoles de bourse, prise de décision, émission d'ordres)
**entièrement en matériel**, sans traverser un OS. C'est le domaine historique du FPGA,
avec des designs propriétaires ultra-optimisés. Hors sujet pour la plupart des
datacenters d'entreprise, mais c'est la démonstration la plus pure de l'avantage FPGA :
**déterminisme absolu**.

## 84. Cas d'usage 4 — IA et inférence

Le FPGA accélère l'**inférence** (pas l'entraînement) sur des modèles **figés et
quantifiés** (INT8) : latence basse et déterministe, excellent perf/watt. Outils : Vitis AI
(AMD), FPGA AI Suite + OpenVINO (Intel/Altera). Limites honnêtes : l'écosystème logiciel
est **très en retard** sur le GPU (CUDA), les gros LLM ne tiennent pas sur FPGA, et chaque
nouveau modèle demande un nouveau design. Pertinent pour : vision industrielle, NLP
embarqué, scoring temps réel à latence garantie. Pour les LLM : restez sur GPU (voir le
guide IA de Zelef).

## 85. Cas d'usage 5 — Stockage et compression

Compression/décompression (GZIP, ZSTD), déduplication, chiffrement au vol, calcul
d'empreintes — le FPGA excelle dans ces pipelines réguliers à haut débit. Cas datacenter :
**appliances de sauvegarde** (accélération de la déduplication), **stockage NVMe-oF**
(offload du protocole), **bases de données** (filtrage/compression près des données).
Souvent le ROI le plus rapide : le FPGA se paie en **débit disque/licences évités**.

## 86. Consommation et refroidissement : les chiffres

| Équipement | TDP / conso typique | Refroidissement | Remarque |
|---|---|---|---|
| Alveo V80 | **190 W TDP** (vérifié) | Passif (flux d'air serveur requis) | Prévoir le slot double + airflow |
| Alveo U55C (gén. précédente) | ~150-225 W (à vérifier) | Passif/actif selon SKU | — |
| SmartNIC FPGA type N6000 | ~75-150 W (à vérifier) | Passif | Vérifier le SKU exact |
| GPU H100 (comparaison) | 700 W | Actif/liquide | 3-4× un FPGA |
| Serveur 2U standard | 300-800 W | Ventilateurs | — |

Point énergie (lien avec le guide onduleurs de Zelef) : un serveur avec 2 FPGA = **+300 à
400 W** à prévoir dans le dimensionnement de la baie, de l'onduleur et de la clim. Le FPGA
ne « consomme peu » qu'en **perf/watt** sur son workload — en absolu, c'est une charge
thermique sérieuse à évacuer (airflow avant-arrière, pas d'obstruction).

## 87. Quand ça vaut le coup : grille de décision

| Question | Si oui → | Si non → |
|---|---|---|
| Le workload est-il **stable** (pas de changement mensuel) ? | FPGA envisageable | Restez CPU/GPU |
| La **latence déterministe** est-elle critique ? | FPGA fort candidat | GPU/CPU suffisent |
| Le débit dépasse-t-il ce qu'un CPU fait (~10-40 Gb/s chiffrés/cœur) ? | FPGA | CPU |
| Avez-vous (ou pouvez-vous acheter) de l'**expertise RTL/HLS** ? | FPGA | GPU (écosystème logiciel) |
| Le volume justifie-t-il 10 k€/carte + développement ? | FPGA | Cloud / CPU |
| Le workload est-il de l'IA générative / LLM ? | — | **GPU**, pas FPGA |

Score : 4+ « oui » → étudier le FPGA sérieusement ; 2-3 → prototype ; moins → oubliez.

## 88. Coûts : prix et TCO d'une carte FPGA

- **Carte** : ~9 500 $ (Alveo V80 MSRP vérifié) ; 3 000-10 000 € selon gamme et
  distributeur (**à vérifier** au jour de l'achat).
- **Serveur hôte** : prévoir un serveur avec slots PCIe x16 libres, airflow adapté,
  alimentation redondante dimensionnée (+190 W/carte).
- **Licences outils** : Vivado/Quartus — les éditions pour ces cartes sont souvent
  incluses ou en abonnement (**à vérifier**).
- **Développement** : le vrai coût — **3 à 12 mois-ingénieur** pour un design sérieux,
  en interne ou via un bureau d'études (50-150 k€). C'est ce poste qui fait ou défait le ROI.
- **TCO 3 ans** (1 carte + serveur + dev. amorti) : de l'ordre de **80-200 k€** — à
  comparer au nombre de serveurs CPU évités ou à la valeur de la latence gagnée.

## 89. Outils : Vivado, Quartus, open source

- **AMD Vivado / Vitis** : flot historique Xilinx, supporte les Alveo (exemple de design
  AVED sur GitHub pour le V80 — vérifié).
- **Intel Quartus Prime** : flot Altera pour Agilex.
- **Open source** : Yosys/nextpnr pour les petits FPGA (Lattice surtout) — pas pour les
  grosses cartes datacenter en production.
- **Simulation** : cocotb (Python), Verilator — indispensables avant de flasher.
- Conseil : figer les **versions d'outils** par projet (un design validé sous Vivado
  2023.2 n'est pas garanti sous 2024.1 sans re-validation).

## 90. FPGA en production : drivers, monitoring, mise à jour bitstream

- **Drivers** : XRT (AMD) / OFS (Intel) — à intégrer dans l'image OS standard, version
  épinglée.
- **Monitoring** : température du FPGA, taux d'utilisation, erreurs ECC mémoire —
  remonter au SIEM/supervision comme tout équipement.
- **Mise à jour du bitstream** : procédure **signée et versionnée** (comme un firmware) ;
  prévoir le **rollback** (garder l'ancien bitstream valide). Un bitstream corrompu =
  carte inutilisable jusqu'au reflash physique.
- **Sécurité du bitstream** : activer le **chiffrement + authentification** du bitstream
  (clé dans le FPGA) — sinon la propriété intellectuelle et l'intégrité du design sont
  exposées à quiconque lit la flash de la carte.

# PARTIE 4 — SÉCURITÉ PHYSIQUE DU DATACENTER

## 91. Défense en profondeur physique : les 4 cercles

```
  ┌──────────────────────────────────────────────────────────────┐
  │ CERCLE 1 — Périmètre : clôture, portail, éclairage, rondes    │
  │ ┌──────────────────────────────────────────────────────────┐ │
  │ │ CERCLE 2 — Bâtiment : sas d'entrée, accueil, badges       │ │
  │ │ ┌──────────────────────────────────────────────────────┐ │ │
  │ │ │ CERCLE 3 — Salle : contrôle d'accès biométrique,      │ │ │
  │ │ │ vidéo, anti-tailgating                                │ │ │
  │ │ │ ┌──────────────────────────────────────────────────┐ │ │ │
  │ │ │ │ CERCLE 4 — Rack : serrure, cage client, scellés   │ │ │ │
  │ │ │ │ HSM + coffre des sauvegardes                     │ │ │ │
  │ │ │ └──────────────────────────────────────────────────┘ │ │ │
  │ │ └──────────────────────────────────────────────────────┘ │ │
  │ └──────────────────────────────────────────────────────────┘ │
  └──────────────────────────────────────────────────────────────┘
```

Chaque cercle **retarde** et **détecte**. Aucun n'est infaillible seul : la sécurité physique
est un **empilement de délais** (le temps de franchir > le temps d'intervention).

## 92. Périmètre : bâtiment, clôture, sas

- **Clôture** : 2 m minimum, anti-escalade, avec détection (câble périmétrique ou vidéo).
- **Éclairage** : périmètre éclairé la nuit (dissuasion + efficacité des caméras).
- **Portail** : contrôle d'accès véhicules, barrières, registre des immatriculations.
- **Sas d'entrée** : double porte à interverrouillage (la 2e ne s'ouvre que si la 1re est
  fermée) — anti-tailgating dès l'entrée du bâtiment.
- Pour une PME sans datacenter propre : ces exigences se **délèguent à l'hébergeur**
  (demander ses certifications et son rapport d'audit — voir section 126).

## 93. Contrôle d'accès : badges, biométrie, anti-passback

- **Badges** : technologie **MIFARE DESFire** ou équivalent (ne pas utiliser les vieux
  MIFARE Classic, clonables en minutes). Un badge = une personne, jamais de badge
  « générique ».
- **Biométrie** : empreinte ou veine du doigt en **second facteur** pour les salles
  critiques (jamais seule : on ne « révoque » pas un doigt).
- **Anti-passback** : le système refuse une 2e entrée sans sortie enregistrée — tue le
  prêt de badge.
- **Zonage** : chaque badge n'ouvre que les zones nécessaires à sa fonction (principe du
  moindre privilège physique).
- **Révocation** : procédure de désactivation **immédiate** en cas de départ/perte
  (SLA < 1 h).

## 94. Cages, racks, serrures : le cloisonnement

- **Cages grillagées** : dans une salle mutualisée, chaque client a sa cage fermée à clé —
  vos HSM et serveurs critiques n'y sont accessibles qu'à vos habilités.
- **Racks verrouillés** : serrures à clé ou à code **par rack**, pas une clé unique pour
  toute la salle.
- **Scellés** : sur les serveurs sensibles (HSM, serveurs PKI), scellés numérotés dont
  l'état est contrôlé à chaque intervention.
- **Baies aveugles** : panneaux obturateurs sur les U vides (sécurité + airflow —
  double bénéfice, voir section 142).

## 95. Vidéosurveillance : caméras, enregistrement, durées

- **Couverture** : entrées/sorties, couloirs, allées de baies, coffre-fort, zone de
  réception — **pas de zones aveugles** sur les chemins critiques.
- **Qualité** : résolution permettant l'identification (pas juste « une silhouette »),
  infrarouge ou éclairage pour la nuit.
- **Enregistrement** : **90 jours minimum** de rétention pour les zones critiques
  (standard d'audit courant), stockage **protégé** (local sécurisé + réplication).
- **Supervision** : affichage en temps réel à l'accueil/sécurité + détection de
  mouvement avec alertes hors heures ouvrées.
- **Conformité** : affichage réglementaire (information des personnes filmées), registre
  des accès aux enregistrements — pas de conseil juridique ici, mais à cadrer avec le
  DPO/juriste (voir section 128).

## 96. Détection intrusion : alarmes, capteurs

- **Alarme volumétrique** : détecteurs de mouvement dans les salles hors présence
  permanente.
- **Contacts d'ouverture** : portes, baies sensibles, coffre.
- **Report** : vers un centre de télésurveillance ou l'astreinte, avec **levée de doute**
  vidéo.
- **Tests** : déclenchement testé **trimestriellement** (une alarme jamais testée est une
  décoration).
- **Couplage** : intrusion + coupure électrique simultanées = scénario d'attaque à
  intégrer au plan de réponse (voir section 136).

## 97. Gestion des visiteurs : procédure complète

1. **Pré-enregistrement** : visite annoncée, motif, zones autorisées, accompagnateur désigné.
2. **Accueil** : pièce d'identité vérifiée, badge **visiteur** distinct (couleur
   différente), registre signé (papier ou électronique, conservé 1 an minimum).
3. **Escorte permanente** : aucun visiteur seul en salle technique — **sans exception**,
   y compris les prestataires connus.
4. **Restitution** : badge rendu, sortie enregistrée, vérification qu'il ne reste rien
   (outils, documents).
5. **Prestataires récurrents** (maintenance clim, électriciens) : même procédure, avec en
   plus une **charte de confidentialité** signée et une sensibilisation aux zones
   interdites (photos interdites en salle).

## 98. Politique anti-tailgating et escortes

Le **tailgating** (suivre quelqu'un qui badge) est le vecteur n°1 d'intrusion physique.
Contre-mesures : sas à interverrouillage, tourniquets pleine hauteur aux entrées
critiques, **culture d'entreprise** (« je ne tiens pas la porte, c'est la procédure » —
à faire valider par la direction pour que personne ne se sente malpoli), et
**rappels réguliers**. Un test d'intrusion physique annuel (auditeur qui tente d'entrer)
est le meilleur indicateur.

## 99. Personnel : habilitations, séparation des tâches

- **Habilitation** : l'accès aux salles critiques est nominatif, revu **semestriellement**
  (les droits s'accumulent avec les changements de poste — revue obligatoire).
- **Séparation** : celui qui administre les HSM n'est pas celui qui contrôle les accès
  physiques ; l'auditeur n'est pas l'exploitant (même logique qu'en section 44).
- **Départs** : checklist de sortie incluant la révocation des accès physiques **le jour
  même**.
- **Sensibilisation** : phishing, tailgating, photos en salle — 1 session/an minimum,
  traçée.

## 100. Destruction de médias : NIST 800-88

La norme **NIST SP 800-88** définit 3 niveaux de sanitization :

| Niveau | Méthode | Quand l'utiliser |
|---|---|---|
| **Clear** | Écrasement logique (1+ passes) | Réutilisation interne, disque sain |
| **Purge** | Commandes firmware (Secure Erase, crypto-erase), démagnétisation | Sortie du parc, disques défectueux mais lisibles |
| **Destroy** | Broyage, désintégration, incinération | Données top critiques, disques HS, fin de vie certifiée |

Procédure : **inventaire** (n° de série), **méthode tracée** par support, **PV de
destruction** signé, prestataire certifié pour le broyage (avec attestation). Les
**bandes de sauvegarde** suivent le même régime — une bande « jetée » lisible = fuite de
données.

## 101. Disques chiffrés (SED) et crypto-erase

Les disques **SED** (Self-Encrypting Drives, norme TCG Opal) chiffrent en permanence avec
une clé interne : la mise au rebut devient un **crypto-erase** (destruction de la clé =
données irrécupérables en millisecondes). Avantages : instantané, pas d'usure, fonctionne
même sur disque partiellement défectueux. Conditions : **activer le chiffrement dès le
déploiement** (un SED non activé = disque clair), gérer les mots de passe/clés (via le
contrôleur ou un KMIP/KMS — voir partie 5), et vérifier la conformité du firmware (des
failles ont existé sur des implémentations SED — patcher).

## 102. Déclassement de matériel : procédure

Checklist de sortie de parc pour tout serveur/équipement :

- [ ] Sauvegarde des données utiles (si réutilisation prévue ailleurs).
- [ ] **Effacement** selon NIST 800-88 (section 100) — niveau adapté à la sensibilité.
- [ ] Retrait et destruction séparée des **modules TPM/HSM** le cas échéant.
- [ ] Retrait des étiquettes d'inventaire sensibles, des configurations (BMC reset).
- [ ] PV de déclassement archivé (n° de série, méthode, date, opérateur).
- [ ] Si revente/don : uniquement après Purge minimum + contrôle.

## 103. Sécurité électrique et incendie : le lien énergie

La sécurité physique inclut l'énergie (métier de Zelef — voir partie 8 en détail) :

- **Incendie** : détection très précoce (aspiration), extinction **gaz** (inerte, sans
  eau) en salle serveurs, compartimentage coupe-feu, exercice d'évacuation annuel.
- **Dégât des eaux** : détection de fuite sous plancher technique, pas de tuyauterie
  d'eau au-dessus des baies.
- **Électrique** : double alimentation des baies critiques, onduleurs **secourus**
  (section 140), parafoudres, terre < 5 Ω.
- **Clim** : redondance N+1 minimum, sondes de température avec alertes, délestage
  automatique en cas de surchauffe.

## 104. Tableau : équipements physiques et budgets indicatifs

| Équipement | Ordre de grandeur | Remarque |
|---|---|---|
| Contrôle d'accès (lecteurs + logiciel, 5 portes) | 5–15 k€ | MIFARE DESFire, anti-passback |
| Biométrie (1 point, second facteur) | 1–3 k€ | En complément du badge |
| Vidéosurveillance (8 caméras + NVR 90 j) | 5–12 k€ | Rétention = coût de stockage |
| Alarme intrusion + télésurveillance | 2–5 k€ + abonnement | Tests trimestriels |
| Cage grillagée (10 m²) | 3–8 k€ | En salle mutualisée |
| Coffre-fort ignifuge (clés, HSM backup) | 1–4 k€ | Classe selon valeur protégée |
| Destruction (broyage, par passage) | 500–2 000 € | Prestataire certifié + PV |

Budgets **indicatifs France/EU 2026, à vérifier** sur devis — mais l'ordre de grandeur
compte : la sécurité physique d'une petite salle se chiffre en **dizaines de k€**, pas en
centaines.

# PARTIE 5 — GESTION DES CLÉS / KMS

## 105. Gestion des clés : le cycle de vie complet

Une clé n'est pas un fichier qu'on pose quelque part : c'est un **actif avec un cycle de
vie** :

```
 Génération (HSM/TRNG) → Distribution → Utilisation → Rotation → Archivage → Destruction
        │                     │              │            │            │            │
   cérémonie MofN        canal chiffré   journalisée   planifiée   chiffrée    certifiée
   (section 42)          (jamais clair)  (section 120) (section 108)(durée légale)(section 100)
```

Chaque transition est **documentée** : qui a fait quoi, quand, avec quelle autorisation.
Une clé sans cycle de vie géré, c'est une clé dont on ne connaît ni l'âge, ni les copies,
ni le moment où elle fuira.

## 106. Hiérarchie : KEK, DEK, clés maîtres

- **DEK** (Data Encryption Key) : chiffre les données. Nombreuses, à vie courte, stockées
  **chiffrées**.
- **KEK** (Key Encryption Key) : chiffre les DEK. Peu nombreuses, à vie longue, stockées
  dans le **HSM/KMS**, jamais en clair hors de lui.
- **Clé maîtresse / root key** : au sommet de la hiérarchie, générée en cérémonie,
  sauvegardée en MofN. Elle ne chiffre jamais de données directement.

Principe : **compromettre une DEK n'expose qu'un jeu de données** ; compromettre la KEK
expose tout ce qu'elle protège ; compromettre la maîtresse = tout reconstruire. D'où la
protection croissante à chaque niveau.

## 107. Envelope encryption : le pattern central

Le pattern utilisé par AWS KMS, GCP, Vault :

1. Le KMS génère une **DEK** (clé de données) via le HSM.
2. La DEK est **chiffrée par la KEK** → on obtient la DEK « enveloppée ».
3. Les données sont chiffrées avec la DEK **en clair en mémoire uniquement**.
4. On stocke : données chiffrées + DEK enveloppée. La DEK en clair est **oubliée**.

Pour déchiffrer : le KMS désenveloppe la DEK (opération HSM), déchiffre les données.
Avantages : la KEK ne voyage jamais, les DEK peuvent être nombreuses et rotées souvent,
le volume chiffré par une seule clé reste limité. **C'est le pattern à implémenter par
défaut** pour tout chiffrement applicatif maison.

## 108. Rotation : politiques et automatisation

La rotation limite l'exposition d'une clé dans le temps :

| Type de clé | Rotation recommandée | Notes |
|---|---|---|
| Clé TLS serveur | 90 jours (ou moins, ACME) | Automatisable à 100 % |
| DEK applicatives | 1 an ou par volume (ex. tous les 2^32 chiffrements) | Re-chiffrement ou double enveloppe |
| KEK | 2-3 ans | Opération planifiée, pas d'urgence |
| Clé d'AC intermédiaire | 3-5 ans | Planifiée avec la cérémonie |
| Clé d'AC racine | 10-20 ans (ou jamais, avec rollover) | Événement majeur, cérémonie complète |
| Clé de chiffrement de sauvegarde | 1-2 ans | Attention aux sauvegardes anciennes |

Règle : **automatiser** tout ce qui est automatisable (TLS, DEK), **planifier et
documenter** le reste. Une rotation manuelle « quand on y pense » n'arrive jamais.

## 109. Durées de vie recommandées par type de clé (détail)

Complément du tableau 108 — les justifications :

- **TLS 90 jours** : compromis entre charge opérationnelle et fenêtre d'exposition ;
  avec ACME/Let's Encrypt interne, le coût marginal est nul.
- **DEK 1 an** : limite le volume de données sous une même clé (principe de
  compartimentation) ; le re-chiffrement peut être progressif (lazy re-encryption).
- **KEK 2-3 ans** : la rotation d'une KEK = re-envelopper les DEK, opération lourde
  mais planifiable ; au-delà de 3 ans, le risque d'exposition silencieuse augmente.
- **Racine 10-20 ans** : sa rotation invalide toute la chaîne — on ne la fait que si
  compromise ou par obsolescence d'algorithme (ex. migration PQC, voir partie 9).

## 110. Séparation des rôles : qui peut quoi

Matrice type (à adapter) :

| Rôle | Générer | Utiliser | Sauvegarder | Détruire | Auditer |
|---|---|---|---|---|---|
| Crypto Officer | ✅ (cérémonie) | ✅ | ✅ | ❌ | ❌ |
| Opérateur applicatif | ❌ | ✅ (via API) | ❌ | ❌ | ❌ |
| Custodian (MofN) | ❌ | ❌ | ✅ (sa part) | ❌ | ❌ |
| Auditeur | ❌ | ❌ | ❌ | ❌ | ✅ |
| Admin système | ❌ | ❌ | ❌ | ❌ | ✅ (logs) |

Aucune personne ne cumule **génération + utilisation + audit**. Les cumuls sont
documentés comme exceptions temporaires, avec date de fin.

## 111. Intégration HSM↔KMS : les 3 architectures

1. **KMS logiciel + HSM en backend** : le KMS (ex. Vault) stocke sa clé maîtresse dans le
   HSM (seal/unseal), les opérations courantes restent logicielles. Bon compromis
   coût/sécurité — le plus courant en PME/ETI.
2. **HSM direct** : les applications appellent le HSM en PKCS#11, pas de KMS
   intermédiaire. Simple, mais pas de gestion de cycle de vie (rotation, politiques) —
   à réserver aux usages simples (PKI, TSA).
3. **KMS cloud + Cloud HSM** : le KMS du provider (AWS KMS, Azure Key Vault Managed HSM)
   adossé à des HSM dédiés. Idéal cloud-native ; la souveraineté dépend du modèle
   (BYOK/HYOK, section 116).

## 112. HashiCorp Vault : panorama

**Vault** est le KMS logiciel de référence (open source + entreprise) : stockage de
secrets, PKI intégrée, chiffrement as-a-service (Transit), identités dynamiques
(credentials à usage unique pour bases de données). Points clés pour ce guide :

- Le **seal/unseal** : Vault démarre « scellé », sa clé maîtresse est découpée en parts
  (Shamir) ou confiée à un HSM/cloud-KMS (**auto-unseal**).
- Le backend de stockage doit être **chiffré et sauvegardé** (Raft intégré ou Consul).
- La **PKI intégrée** peut émettre des certificats internes avec l'AC racine sur HSM.

## 113. Vault : auto-unseal avec HSM

Sans auto-unseal, chaque redémarrage de Vault exige que des humains apportent les parts
Shamir — **incompatible avec une infrastructure qui redémarre seule** (panne électrique,
voir partie 8). L'**auto-unseal** confie le descellement à un HSM (via PKCS#11) ou à un
KMS cloud : au démarrage, Vault demande au HSM de déchiffrer sa clé maîtresse, **sans
intervention humaine**. Conditions : le HSM doit être **disponible avant Vault**
(ordre de démarrage documenté), sur **alimentation secourue** (section 140), et l'accès
réseau HSM↔Vault sécurisé (VLAN crypto, section 35). C'est le chaînon qui rend la
continuité électrique critique pour la crypto (voir section 141).

## 114. EJBCA : la PKI open source

**EJBCA** (PrimeKey/Cybertrust) est l'AC logicielle open source de référence : émission de
certificats X.509, OCSP, CMP/SCEP pour l'enrôlement, profils de certificats. Branchée en
PKCS#11 sur n'importe quel HSM du marché, elle couvre : PKI interne d'entreprise, AC pour
IoT/industrie, certificats serveurs. Alternative : **AD CS** (Microsoft) pour les
environnements Windows — le choix dépend de l'écosystème, pas de la sécurité (les deux
s'adossent au HSM).

## 115. Sauvegardes chiffrées des clés

Les sauvegardes du KMS/HSM suivent les mêmes règles que les clés elles-mêmes :

- **Chiffrées** (par une KEK dédiée, stockée en MofN), jamais en clair.
- **Testées** : restauration annuelle sur un environnement isolé (section 138).
- **Géographiquement séparées** : une copie sur le site principal (coffre), une sur le
  site distant — le vol/incendie d'un site ne doit pas tout emporter.
- **Rétention** : alignée sur la durée de vie des données chiffrées (une sauvegarde
  chiffrée avec une clé détruite = données perdues — coordonner avec la section 109).
- **Journalisées** : chaque export/import de clé est un événement d'audit (section 120).

## 116. BYOK et HYOK : apporter ses clés au cloud

- **BYOK** (Bring Your Own Key) : vous générez la clé dans **votre** HSM on-prem, vous
  l'importez (wrappée) dans le KMS cloud. Vous gardez la **génération**, le provider
  gère l'**usage**. Révocation possible en supprimant la clé (avec les conséquences :
  données illisibles).
- **HYOK** (Hold Your Own Key) : la clé **ne quitte jamais** votre HSM ; le cloud
  appelle votre HSM pour chaque opération (ex. Double Key Encryption chez Utimaco/
  Microsoft). Sécurité maximale, **latence** et **dépendance réseau** à assumer.
- Choix : BYOK pour la conformité standard, HYOK pour les données les plus sensibles
  ou les exigences de souveraineté strictes.

## 117. Tableau comparatif KMS

| Solution | Modèle | HSM backend | Idéal pour | Coût indicatif |
|---|---|---|---|---|
| HashiCorp Vault (OSS) | Logiciel on-prem | Oui (PKCS#11, auto-unseal) | ETI/datacenter, contrôle total | 0 € + infra + expertise |
| HashiCorp Vault (Enterprise) | Logiciel on-prem | Oui + HSM as-a-service | Multi-sites, support | Licence (à vérifier) |
| AWS KMS | Cloud managé | Oui (CloudHSM backend en option) | Workloads AWS | ~1 $/mois/clé + requêtes |
| Azure Key Vault / Managed HSM | Cloud managé | Oui (Managed HSM = dédié) | Workloads Azure | Voir section 19 |
| GCP Cloud KMS (+ Cloud HSM) | Cloud managé | Oui (niveau HSM) | Workloads GCP | ~1 $/mois/version + requêtes |
| EJBCA | Logiciel on-prem (PKI) | Oui (PKCS#11) | AC interne, IoT | 0 € (OSS) + support en option |

## 118. Cas chiffré PME : BOM KMS + HSM complète

**Besoin** : PME 80 personnes, PKI interne (Wi-Fi EAP-TLS, VPN), chiffrement des
sauvegardes, signature des documents RH.

| Poste | Choix | Coût indicatif |
|---|---|---|
| HSM racine | 2× YubiHSM 2 (1 prod + 1 backup coffre) | ~1 300 € |
| Serveur PKI | VM existante + EJBCA (OSS) | 0 € |
| KMS applicatif | Vault OSS sur 3 VM (Raft) | 0 € + infra existante |
| Auto-unseal | YubiHSM 2 n°3 dédié (ou Shamir manuel si redémarrages rares) | ~650 € |
| Coffre-fort | Ignifuge, pour backup HSM + PV cérémonie | ~1 500 € |
| Prestation cérémonie + formation | 3 jours | ~5 000 € |
| **Total** | | **~8 500 €** |

C'est le **ticket d'entrée réaliste** d'une crypto bien gérée en PME. Le poste le plus
sous-estimé : le **temps humain** (cérémonie, revues, tests de restauration).

## 119. Erreurs classiques de gestion des clés

1. Clé maîtresse **dans un fichier** sur le serveur applicatif (« en attendant le HSM »).
2. **Même clé** pour chiffrer et signer, ou pour la prod et les tests.
3. Sauvegarde des clés **en clair** « parce que c'est plus simple à restaurer ».
4. Rotation **jamais** faite (« on n'a jamais eu le temps »).
5. Quorum MofN dont toutes les parts sont **dans le même coffre**.
6. Vault en **Shamir manuel** sur une infra qui redémarre seule après coupure.
7. Clés **codées en dur** dans le code ou les playbooks Ansible (secrets → Vault/Ansible
   Vault chiffré, jamais en clair dans git).
8. **Anciennes clés jamais détruites** : elles traînent dans les sauvegardes pendant
   des années.
9. Pas d'**inventaire** : personne ne sait combien de clés existent ni où.
10. Certificats **expirés** en production parce que le renouvellement était manuel.

## 120. Journaux d'audit des opérations de clés

Tout KMS/HSM sérieux journalise : **qui** (identité), **quoi** (génération, utilisation,
export, destruction), **quand** (horodatage fiable — voir section 11), **résultat**
(succès/échec). Exigences :

- Journaux **protégés en écriture** (append-only, WORM ou SIEM distant) — un attaquant
  qui efface ses traces dans les logs a gagné.
- **Alertes** : échecs d'authentification répétés, export de clé (événement rare =
  alerte immédiate), utilisation hors heures ouvrées.
- **Rétention** : 1 an minimum en ligne, archivage selon les obligations (voir
  section 127).
- **Revue** : les logs que personne ne lit ne servent à rien — revue hebdo automatisée
  + revue mensuelle humaine.

# PARTIE 6 — CONFORMITÉ : L'ESSENTIEL SANS JARGON JURIDIQUE

> Avertissement : cette partie est **généraliste et informative**. Ce n'est pas un conseil
> juridique. Pour une certification, un audit ou un contentieux, faites valider par un
> juriste/auditeur qualifié.

## 121. ISO 27001:2022 : l'essentiel en bref

L'**ISO/IEC 27001** est la norme de référence pour un **SMSI** (système de management de la
sécurité de l'information). Principe : on identifie les risques, on applique des mesures
(l'Annexe A : 93 contrôles), on **améliore en continu** (roue de Deming). La certification
par un organisme accrédité est un **argument commercial** fort (exigée dans de nombreux
appels d'offres) et un **cadre** qui force la rigueur. Pour un datacenter/PME : viser la
certification est un projet de **6 à 18 mois** ; en attendant, appliquer les contrôles
pertinents (dont ceux de la section 122) apporte déjà 80 % de la valeur.

## 122. Annexe A : les contrôles crypto (A.10 et associés)

Les contrôles directement liés à ce guide (ISO 27001:2022, Annexe A) :

- **A.10 — Cryptographie** : politique d'utilisation de la cryptographie, **gestion des
  clés** sur tout leur cycle de vie (c'est exactement la partie 5 de ce guide).
- **A.8.10** : suppression des informations (cf. NIST 800-88, section 100).
- **A.8.13** : sauvegarde des informations (cf. section 115).
- **A.5.33** : protection des procédures d'exploitation — dont les **cérémonies**
  (section 42).
- **A.8.15** : journalisation (cf. section 120), **A.8.16** : surveillance.
- **A.5.14** : contact avec les autorités / groupes spécialisés (utile pour remonter
  les vulnérabilités firmware).
- **A.8.9** : gestion de la configuration (inventaire firmware, section 62).

En audit, l'auditeur demandera : la **politique crypto écrite**, l'**inventaire des clés**,
les **PV de cérémonie**, les **preuves de rotation** et les **journaux**. Ce guide fournit
le fond ; il reste à l'écrire dans vos documents.

## 123. PCI DSS v4 : si vous touchez au paiement

Si vous stockez, traitez ou transmettez des données de cartes bancaires, **PCI DSS**
s'applique — et ses exigences crypto sont strictes : chiffrement des données carte au
repos et en transit, **gestion des clés documentée** (génération, distribution,
stockage, rotation au moins annuelle, destruction), et pour les opérations PIN : HSM
certifié **PCI HSM** (voir sections 13, 25, 28). La v4.x insiste sur l'approche
personnalisée et la validation continue. Point pratique : la conformité PCI est
**périmétrique** — isolez le périmètre carte du reste (segmentation réseau prouvée)
pour réduire le coût d'audit.

## 124. RGPD et chiffrement : l'article 32

Le RGPD n'impose pas le chiffrement nommément, mais son **article 32** exige des mesures
« appropriées » dont le chiffrement des données personnelles est l'exemple cité. En
pratique : chiffrement des données personnelles **au repos** (disques, bases, sauvegardes)
et **en transit** (TLS), avec gestion des clés sérieuse (partie 5). Bénéfice concret :
des données personnelles **chiffrées avec des clés non compromises** peuvent éviter la
notification de violation (considérant 83) — c'est un argument ROI direct pour le HSM.

## 125. eIDAS : signature et horodatage

Le règlement **eIDAS** (UE) encadre la signature électronique et les services de confiance.
Niveaux : simple, avancée, **qualifiée** (la plus forte, équivalent manuscrit). Pour le
qualifié : certificat qualifié + **QSCD** (dispositif de création de signature qualifié —
typiquement un HSM certifié, voir section 7) + prestataire de services de confiance
qualifié. Si vos clients exigent du « qualifié », le choix du HSM et du prestataire est
contraint — à cadrer **avant** l'achat du matériel.

## 126. Exigences clients : répondre aux questionnaires

Les grands clients envoient des **questionnaires de sécurité** (type SIG, CAIQ). Les
questions crypto reviennent toujours : « où sont stockées les clés ? » (→ HSM, partie 1),
« comment sont-elles sauvegardées ? » (→ section 41), « quelle rotation ? » (→ section
108), « qui y a accès ? » (→ section 110), « quels journaux ? » (→ section 120).
**Préparer un dossier standard** avec ces réponses (sans divulguer de secrets
opérationnels) fait gagner des semaines par appel d'offres. Les certifications (ISO 27001,
rapports d'audit de l'hébergeur) répondent à 50 % des questions d'un coup.

## 127. Journaux et audit : ce qu'il faut tracer

Synthèse des exigences de traçabilité de ce guide :

| Source | Événements à tracer | Rétention indicative |
|---|---|---|
| HSM | Génération, utilisation, export, destruction de clés ; échecs d'auth | 1-3 ans |
| KMS (Vault...) | Unseal, lecture de secrets, émissions de certificats | 1-3 ans |
| BMC/serveurs | Connexions, montages ISO, mises à jour firmware | 1 an |
| Contrôle d'accès physique | Entrées/sorties salles critiques | 1 an |
| Vidéosurveillance | Enregistrements zones critiques | 90 jours (section 95) |
| Cérémonies | PV signés (papier + numérique) | Durée de vie de la clé + 5 ans |

Rétentions **indicatives** — à aligner sur vos obligations contractuelles et légales
(avec le juriste, voir l'avertissement en tête de partie).

## 128. Politiques à écrire : la liste minimale

Sans ces documents, l'audit échoue quel que soit le matériel :

1. **Politique de cryptographie** : algos autorisés/interdits, tailles de clés minimales
   (ex. RSA ≥ 2048, interdiction de DES/3DES sauf legacy documenté).
2. **Politique de gestion des clés** : cycle de vie, rotation, MofN, rôles (partie 5).
3. **Procédure de cérémonie** : script pas-à-pas (section 42).
4. **Politique de contrôle d'accès physique** : zonage, visiteurs, habilitations
   (partie 4).
5. **Procédure de destruction des médias** : NIST 800-88 adaptée (section 100).
6. **Plan de réponse aux incidents** : dont les runbooks crypto (sections 136-137).
7. **Politique de mise à jour des firmwares** : registre, tests, rollback (section 62).

Modèles : partir des exemples de l'**ANSSI** (guides d'hygiène) et des templates ISO 27001
— puis les **adapter** à votre réalité, jamais du copier-coller pur.

# PARTIE 7 — CHECKLISTS PRATIQUES : LA MISE EN SERVICE SÉCURISÉE

## 129. Checklist : réception et mise en service d'un serveur

**À la réception** (voir aussi section 65) :

- [ ] Emballage/scellés intacts, n° de série = bon de livraison.
- [ ] Inventaire physique : CPU, RAM, disques, cartes, alimentations.
- [ ] Premier boot en **zone de quarantaine** (VLAN isolé, pas de production).

**Firmware & plateforme** :

- [ ] UEFI à la version validée, **Secure Boot activé** (clés propres si images maison).
- [ ] **TPM 2.0** présent et provisionné (`tpm2_pcrread` OK, EK certifié).
- [ ] Boot Guard / PSB activé si disponible (**à vérifier** selon OEM).
- [ ] Option ROM des cartes : versions notées, Secure Boot les vérifie.

**BMC** (détail section 130) :

- [ ] Firmware BMC à jour, mot de passe par défaut **changé** (unique par serveur).
- [ ] BMC sur **VLAN management** dédié, IP statique, DNS correct.
- [ ] Services inutiles désactivés (Telnet, HTTP, IPMI 1.5).
- [ ] Certificat du BMC remplacé par un certificat de la PKI interne.

**OS & durcissement** :

- [ ] Installation depuis une **image signée/vérifiée** (hash contrôlé).
- [ ] Chiffrement disque (LUKS/BitLocker) avec clé **scellée au TPM** (PCR 0-7).
- [ ] Comptes par défaut supprimés/renommés, SSH par clés uniquement, root désactivé.
- [ ] Agent de supervision + syslog vers SIEM, NTP (NTS si possible).
- [ ] Pare-feu local : uniquement les ports nécessaires.

**Documentation** :

- [ ] Fiche serveur : n° de série, versions firmware, empreintes de clés, VLAN, rôle.
- [ ] PV de mise en service signé et archivé.

## 130. Checklist : durcissement BMC (iLO/iDRAC/OpenBMC)

- [ ] Mot de passe admin changé (20+ caractères, unique), comptes inutiles désactivés.
- [ ] **2FA** activée sur l'interface web (si supportée).
- [ ] Réseau : VLAN management dédié, pas de routage vers Internet/production.
- [ ] Firewall : seules les IP des postes d'admin autorisées.
- [ ] Protocoles : HTTPS + Redfish uniquement ; SSH si besoin justifié ; **IPMI 2.0
      chiffré** seulement si indispensable (jamais 1.5).
- [ ] Certificat TLS remplacé (PKI interne).
- [ ] SNMP : v3 uniquement, community par défaut supprimée.
- [ ] Syslog vers SIEM, alertes sur connexions.
- [ ] Firmware BMC suivi : abonnement aux bulletins de sécurité du constructeur,
      patch sous 30 jours pour les critiques.
- [ ] Montage d'images ISO : désactivé par défaut, activé ponctuellement avec traçabilité.

## 131. Checklist : firmware et secure boot

- [ ] Registre des versions firmware par serveur (automatisé via Redfish, revu
      mensuellement).
- [ ] Secure Boot **activé et vérifié** sur 100 % du parc (audit trimestriel — un
      serveur « en panne de secure boot » est un incident).
- [ ] Clés Secure Boot : inventaire des PK/KEK/db ; rotation planifiée si clés maison.
- [ ] Valeurs de référence **measured boot** à jour après chaque mise à jour firmware.
- [ ] Procédure de mise à jour : test sur pilote → déploiement par vagues → rollback
      documenté.
- [ ] Firmwares téléchargés **uniquement** depuis le constructeur (hash vérifié).

## 132. Checklist : mise en service d'un HSM

- [ ] Réception : scellés intacts, PV avec photos.
- [ ] Installation en baie verrouillée, **double alimentation** sur PDU A+B secourues
      (voir partie 8).
- [ ] Réseau : VLAN crypto dédié, IP statiques, firewall (section 35).
- [ ] Initialisation : SO PIN généré et stocké en MofN, jamais réutilisé ensuite.
- [ ] Partitions créées selon le plan (une par usage), CO nommés par partition.
- [ ] **Cérémonie** de génération des clés critiques (section 133), PV signé.
- [ ] Sauvegarde : réplication vers le HSM backup, test de restauration.
- [ ] Supervision : SNMP/syslog vers SIEM, alertes batterie/température/échecs d'auth.
- [ ] Documentation : schéma réseau, mots de passe au coffre, procédure d'urgence.
- [ ] Firmware à jour, validation FIPS du modèle **vérifiée** sur la liste CMVP.

## 133. Checklist : cérémonie de génération de clés

**Avant** :

- [ ] Script de cérémonie écrit, relu par 2 personnes, version figée.
- [ ] Participants convoqués : officiants (quorum), témoin/auditeur, scribe.
- [ ] Salle sécurisée réservée, HSM vérifié (scellés, firmware, initialisé).
- [ ] Supports de sauvegarde prêts (HSM backup, smartcards PED numérotées).

**Pendant** :

- [ ] Identité des participants vérifiée, feuille de présence signée.
- [ ] Génération **dans** le HSM (jamais d'import pour une racine), paramètres lus à
      voix haute et vérifiés (algo, taille, usage, dates).
- [ ] Empreinte de la clé publique relevée et notée par 2 personnes indépendamment.
- [ ] Sauvegarde immédiate (duplication ou MofN), vérification de la sauvegarde.
- [ ] Zéro document sensible laissé sur table ; photos interdites sauf procédure.

**Après** :

- [ ] PV rédigé sous 48 h, signé par tous les participants.
- [ ] PV archivé (2 copies, lieux séparés), n° de série et empreintes dans l'inventaire.
- [ ] Smartcards remises aux custodians contre signature.

## 134. Checklist : mise en service d'une carte FPGA

- [ ] Serveur hôte : slots PCIe x16 libres, **airflow** vérifié (carte passive !),
      alimentation dimensionnée (+190 W par V80).
- [ ] Drivers XRT/OFS installés, version épinglée dans l'image OS.
- [ ] **Bitstream signé** : vérification de la signature avant flash, version notée.
- [ ] Chiffrement/authentification du bitstream activé sur la carte.
- [ ] Tests : design de validation du constructeur OK, température en charge < seuils.
- [ ] Monitoring : température, utilisation, erreurs ECC → supervision.
- [ ] Procédure de mise à jour du bitstream écrite, avec **rollback**.
- [ ] Bitstream source versionné (git + hash), accès restreint (propriété
      intellectuelle).

## 135. Checklist : baie datacenter sécurisée

- [ ] Baie **verrouillée** (serrure par baie, pas de clé unique), panneaux obturateurs.
- [ ] Double alimentation : PDU A + PDU B sur **circuits secourus différents**
      (voir section 141).
- [ ] Câblage : étiqueté, chemins séparés courant fort/courant faible.
- [ ] Sondes température/humidité en haut et bas de baie, alertes.
- [ ] Extinction : détection incendie de la salle, consignes affichées.
- [ ] Accès : seuls les habilités, registre des interventions.
- [ ] Plan de baie à jour (U par U), affiché + version numérique.

## 136. Runbook : incident « HSM indisponible »

**Symptômes** : les applications remontent des erreurs PKCS#11, les signatures s'arrêtent.

1. **Qualifier** (5 min) : un seul HSM ou tout le cluster ? Panne réseau (VLAN crypto)
   ou panne boîtier ? Consulter la supervision (SNMP/syslog).
2. **Basculer** : si cluster HA, vérifier que le client a basculé sur le 2e membre ;
   sinon bascule manuelle documentée.
3. **Isoler** : ne pas redémarrer « pour voir » avant d'avoir les logs (un redémarrage
   peut effacer des traces, voire déclencher une zeroization si la batterie est en cause).
4. **Si panne matérielle** : appliquer la procédure constructeur (remplacement, avec
   support sous contrat — d'où l'importance du support 15-20 %, section 47).
5. **Restaurer** : recharger les clés depuis le HSM backup (procédure testée, section 138).
6. **Post-mortem** : cause racine, RTO réel vs objectif, actions correctives.
7. **Ne jamais** : restaurer une clé depuis une sauvegarde non vérifiée, ni réutiliser
   un HSM dont l'intégrité physique est douteuse sans expertise.

## 137. Runbook : compromission suspectée d'une clé

1. **Confiner** : révoquer les certificats associés (CRL/OCSP), désactiver la clé dans
   le HSM/KMS (pas la détruire tout de suite — besoin forensique).
2. **Évaluer** : périmètre (quelles données sous cette clé ? quelle période ?),
   vecteur probable (fuite de sauvegarde ? accès abusif ? malware ?).
3. **Régénérer** : nouvelle clé (cérémonie si racine), **re-chiffrer** les données
   exposées avec la nouvelle clé.
4. **Notifier** : selon les obligations (clients, autorités — avec le juriste ; voir
   l'avertissement partie 6).
5. **Post-mortem** : comment la clé a-t-elle fuité ? Corriger la cause (souvent
   organisationnelle, pas technique — voir les erreurs de la section 119).

## 138. Plan de test DR crypto : le test annuel

Une fois par an minimum, sur un **environnement isolé** :

- [ ] Restauration d'une partition HSM depuis la sauvegarde → clés utilisables ?
- [ ] Restauration du quorum MofN : réunir M custodians, reconstituer, vérifier.
- [ ] Démarrage de Vault en auto-unseal après **coupure électrique simulée** (ordre
      de démarrage HSM → Vault → applications).
- [ ] Révocation + réémission d'un certificat (la chaîne CRL/OCSP fonctionne ?).
- [ ] Déchiffrement d'une sauvegarde ancienne avec la clé archivée (la clé existe
      toujours ?).
- [ ] Chronométrer le **RTO réel** et comparer à l'objectif.
- [ ] PV de test archivé, écarts corrigés sous 90 jours.

Un DR crypto jamais testé est une fiction — et c'est le test qui révèle les
sauvegardes pourries, les mots de passe perdus et les procédures obsolètes.

# PARTIE 8 — LIEN ÉNERGIE : QUAND LA CRYPTO DÉPEND DU COURANT

> Le credo de Zelef (chef de service systèmes & énergies) : la sécurité cryptographique
> est un **service électrique** comme un autre. Un HSM sans courant, c'est une PKI à
> l'arrêt. Cette partie chiffre le lien.

## 139. Consommations : le tableau HSM/FPGA/serveurs

| Équipement | Puissance typique | /an (24/7, 0,20 €/kWh) | Commentaire |
|---|---|---|---|
| YubiHSM 2 | **0,1 W** (20 mA sous 5 V) | ~0,20 € | Négligeable |
| Luna PCIe HSM | **14 W** typ. / 18 W max (vérifié) | ~25 € | Dans le serveur hôte |
| Luna Network HSM | **84 W** typ. / 110 W max (vérifié) | ~150 € | + clim (~1:1) |
| Utimaco appliance (indicatif) | ~50-100 W (à vérifier) | ~90-175 € | Selon modèle |
| Serveur PKI/Vault (1U/2U) | 150-350 W | ~260-610 € | Selon charge |
| Carte FPGA Alveo V80 | **190 W** TDP (vérifié) | ~330 € | + airflow serveur |
| Serveur 2U + 2 FPGA | 700-1 100 W | ~1 230-1 930 € | Prévoir la baie en conséquence |
| Baie 42U mixte (exemple) | 5-10 kW | 8 760-17 500 € | Voir section 146 |

Constat : **le HSM lui-même consomme peu** (84 W = une ampoule LED industrielle), mais
**son indisponibilité coûte cher** : c'est un équipement à faible conso et **forte
criticité** — la catégorie qui justifie le plus la redondance électrique.

## 140. Règle d'or : jamais sur onduleur non secouru

Formulation brutale, à afficher dans le local technique :

> **Aucun équipement cryptographique critique (HSM, serveur PKI/AC, Vault, TSA, serveur
> NTP sécurisé) ne doit être branché sur un circuit non secouru par onduleur.**

Pourquoi c'est vital :

- Une coupure franche = **arrêt des signatures et des déchiffrements** (bases TDE qui ne
  redémarrent pas, Vault qui reste scellé, VPN qui ne renégocient plus).
- Pire : des **coupures répétées / micro-coupures** stressent les alimentations et les
  batteries internes des HSM (la batterie SRAM est une sécurité, pas un onduleur).
- Le redémarrage n'est **pas automatique** partout : Vault en Shamir manuel exige des
  humains (section 113), la remise en service d'un HSM après incident exige une
  procédure.

En pratique : circuit **ondulé + groupe électrogène** (autonomie onduleur ≥ temps de
démarrage du groupe + marge, typiquement 15-30 min), **test mensuel** du basculement
(voir le guide onduleurs de Zelef pour les procédures).

## 141. Continuité des services de clés : architecture électrique

```
  ┌─────────┐   ┌──────────┐   ┌───────────────┐   ┌────────────────────┐
  │ Arrivée │──▶│ TGBT +   │──▶│ Onduleur(s)   │──▶│ PDU A (secourue)  │
  │ EDF     │   │ inverseur│   │ N+1, 15-30 min│   └────────┬───────────┘
  └─────────┘   │ de source│   └───────────────┘            │
                │          │   ┌───────────────┐   ┌────────┴───────────┐
                │          │──▶│ Groupe électro│──▶│ PDU B (secourue)  │
                └──────────┘   │ (démarrage    │   └────────┬───────────┘
                               │  < 1 min)      │            │
                               └───────────────┘            ▼
                                              ┌────────────────────────┐
                                              │ HSM 1 → PDU A          │
                                              │ HSM 2 → PDU B          │
                                              │ (jamais les 2 sur la  │
                                              │  même PDU !)           │
                                              │ Serveur Vault → A+B    │
                                              │ Serveur PKI → A+B      │
                                              └────────────────────────┘
```

Règles :

- Les **2 HSM d'un cluster HA sur 2 PDU différentes** (idéalement 2 onduleurs ou 2
  chaînes) : la panne d'une chaîne n'emporte qu'un membre.
- Les serveurs Vault/PKI en **double alimentation** (A+B).
- **Ordre de redémarrage** après coupure longue, documenté et testé : réseau → HSM →
  KMS/Vault (auto-unseal) → bases/applications. Chaque étape **vérifiée** avant la
  suivante (section 138).
- Le **coffre-fort** contenant le HSM backup n'a pas besoin d'être secouru — mais son
  **accès** (contrôle d'accès, éclairage) doit l'être pour une intervention de nuit.

## 142. Refroidissement : HSM et FPGA en baie

- **HSM appliance** (84-110 W) : charge thermique modeste, mais en **1U dense** — ne pas
  le coincer entre deux serveurs à 800 W sans airflow. Plage constructeur : **0-35 °C**
  (Luna, vérifié) — en salle à 27 °C ça passe, en local technique à 38 °C l'été, non.
- **FPGA** (190 W, refroidissement **passif**) : critique — la carte compte sur le flux
  d'air du serveur (avant → arrière, ~20-30 CFM). **Ne jamais** laisser un slot adjacent
  obstrué, ne jamais faire tourner le serveur capot ouvert en production (l'airflow est
  calculé capot fermé).
- **Baie** : panneaux obturateurs sur les U vides (évite le recyclage d'air chaud),
  brosses passe-câbles, sondes haut/bas de baie avec alertes à 30 °C / 32 °C.
- **Lien sécu** : une surchauffe qui coupe les serveurs = même effet qu'une coupure
  électrique sur les services de clés. La clim est un **organe de sécurité**.

## 143. Dimensionnement électrique : méthode

Pour la zone « crypto » d'une baie (exemple) :

1. **Inventaire** : 2× HSM réseau (2× 110 W max) + 1 serveur Vault (350 W) + 1 serveur
   PKI (300 W) + switch management (50 W) = **~920 W**.
2. **Marge** : ×1,5 (pics, vieillissement, évolutions) → **~1 400 W**.
3. **Onduleur** : la zone crypto doit tenir **30 min** sur batteries → ~700 Wh utiles
   (avec rendement 0,9 : ~780 Wh de batteries).
4. **Disjoncteurs** : 1 400 W / 230 V ≈ 6,1 A → disjoncteur **10 A** par PDU (courbe C),
   câbles 1,5 mm² mini (2,5 mm² recommandé).
5. **Clim** : ~1 400 W thermiques à évacuer pour cette zone (+ les serveurs voisins).

Documenter ce calcul dans le **dossier électrique de la baie** — c'est aussi une pièce
d'audit (disponibilité = contrôle ISO 27001 A.8.x).

## 144. Monitoring énergie des équipements de sécurité

Superviser, comme la température :

- **Puissance instantanée** par PDU (les PDU metered le font nativement) — dérive
  anormale = alerte (ventilateur HS, composant en surcharge).
- **État onduleur** : charge %, autonomie restante, état batteries, dernier test.
- **Température** : HSM (via SNMP), FPGA (via driver), baie (sondes).
- **Batterie interne HSM** : état remonté en SNMP — alerte **préventive** 6 mois avant
  fin de vie estimée.
- **Corrélation** : pic de conso + alerte température + échec d'opération crypto =
  incident unique à traiter comme tel (pas 3 tickets séparés).

## 145. Groupe électrogène et HSM : séquence de redémarrage

Après une coupure longue (groupe électrogène en service) :

1. **Stabiliser** : attendre la tension/fréquence stables du groupe (2-5 min), vérifier
   l'onduleur en mode normal (plus sur batteries).
2. **Réseau** : switches, firewall du VLAN crypto.
3. **HSM** : allumage, vérification des scellés/journaux (une coupure brutale peut
   générer des alertes tamper — les **lire**, pas les ignorer), vérification des
   partitions.
4. **KMS** : Vault en auto-unseal (vérifier le descellement), sinon procédure Shamir
   avec les custodians.
5. **Applications** : bases TDE, PKI, TSA — dans l'ordre des dépendances.
6. **Vérifications** : une signature test, un déchiffrement test, les CRL/OCSP à jour.
7. **PV** : durées, écarts, anomalies — archivé.

Temps typique : **30-60 min** pour une zone crypto complète — à intégrer au RTO du PRA.

## 146. Cas chiffré : baie crypto complète (élec + froid)

**Contenu** : 2× HSM réseau, 2× serveurs (Vault + PKI), 1 switch, 1 FPGA de test.

| Poste | Valeur |
|---|---|
| Puissance max installée | ~1 600 W (avec marge ×1,5) |
| Onduleur dédié ou quote-part | 3 kVA N+1 (partagé avec la baie) |
| Autonomie batteries | 30 min |
| Clim (quote-part) | ~2 kW froid |
| Coût élec annuel (0,20 €/kWh) | ~2 800 € |
| Coût complet annuel (élec + clim + maintenance onduleur) | ~5 000-7 000 € |

À comparer au **coût d'une heure d'arrêt** des services de clés (signatures bloquées,
bases inaccessibles, VPN down) : pour la plupart des organisations, **quelques heures
d'arrêt coûtent plus cher que l'année d'électricité secourue**. C'est l'argument à
présenter à la direction.

# PARTIE 9 — À VENIR : ANNONCES VÉRIFIÉES AU 27/09/2026

> Règle de cette partie : uniquement des faits **sourcés et vérifiés le 27/09/2026**.
> Le reste est marqué « à vérifier » ou « non trouvé au 27/09/2026 ».

## 147. PQC : l'état des standards NIST au 27/09/2026 (vérifié)

Le NIST a finalisé ses trois premiers standards post-quantiques le **13 août 2024**
(vérifié) :

| Standard | Algorithme | Usage | Statut au 27/09/2026 |
|---|---|---|---|
| **FIPS 203** | **ML-KEM** (ex-Kyber) | Échange de clés / encapsulation | Finalisé, prêt pour la production |
| **FIPS 204** | **ML-DSA** (ex-Dilithium) | Signatures usage général | Finalisé, prêt pour la production |
| **FIPS 205** | **SLH-DSA** (ex-Sphincs+) | Signatures à base de hash | Finalisé, prêt pour la production |

Conséquence pratique : **on peut déjà déployer** ML-KEM/ML-DSA dans les nouveaux
systèmes (avec crypto-agilité, voir section 152). L'argument « attendons les standards »
n'est plus valable depuis août 2024. Menace active : **harvest now, decrypt later** —
les adversaires enregistrent le trafic chiffré aujourd'hui pour le déchiffrer quand un
ordinateur quantique cryptographiquement pertinent existera (fenêtre crédible estimée
**2029-2032** par les acteurs industrie, à prendre comme ordre de grandeur).

## 148. FIPS 206 (HQC) : en attente (à vérifier)

Le NIST a sélectionné **HQC** comme KEM de secours (basé sur les codes correcteurs,
famille différente des réseaux euclidiens — diversification bienvenue). Le standard
**FIPS 206** est **en cours de finalisation, attendue 2026-2027** (suivi communautaire
pqca, septembre 2026) — **à vérifier** sur le site du NIST. En pratique : ne pas
attendre HQC pour démarrer ; ML-KEM est le standard de production aujourd'hui.

## 149. CNSA 2.0 : le calendrier qui s'impose (vérifié)

La **NSA** impose via **CNSA 2.0** (Commercial National Security Algorithm Suite 2.0,
v2.1 de décembre 2024) la migration PQC des systèmes de sécurité nationale US — avec des
effets de bord sur tous les fournisseurs (vérifié, septembre 2026) :

- **1er janvier 2027** : les **nouvelles acquisitions** de systèmes de sécurité nationale
  doivent supporter CNSA 2.0 par défaut (CNSSP 15).
- **31 décembre 2030** : les équipements ne supportant pas CNSA 2.0 doivent être
  **retirés**.
- **31 décembre 2031** : usage **exclusif** des algorithmes CNSA 2.0.
- Algorithmes retenus : **ML-KEM, ML-DSA** (SLH-DSA **non approuvé** pour ces systèmes),
  AES-256, SHA-384/512.

Même hors marché US : les équipementiers alignent leurs roadmaps sur CNSA 2.0 — c'est le
calendrier qui tire l'industrie.

## 150. EO 14412 et OMB M-26-15 : les jalons US (vérifié)

- **Executive Order 14412** (22 juin 2026, « Securing the Nation Against Advanced
  Cryptographic Attacks ») : les agences fédérales doivent migrer leurs systèmes les plus
  sensibles vers PQC au **31/12/2030**, et vers l'authentification post-quantique au
  **31/12/2031** ; pilote de migration terminé au **31/12/2027**.
- **OMB M-26-15** : plan de migration en **5 phases de 2026 à 2035**, plans de migration
  des agences à remettre en **octobre 2026**.
- **NIST IR 8547** : calendrier de retrait des algos classiques — RSA/ECDSA/EdDSA et
  Diffie-Hellman à 112 bits de sécurité **dépréciés après 2030, interdits en 2035**.

Pour un datacenter européen : pas d'obligation directe, mais les **exigences clients**
et les **fournisseurs** suivront ces calendriers — anticiper 2027-2028 comme fenêtre
d'alignement (recommandation ENISA en ce sens).

## 151. TLS hybride X25519MLKEM768 : déjà en production (vérifié)

Le mode **hybride** combine l'échange classique (X25519) et post-quantique (ML-KEM-768) :
si l'un des deux est cassé, l'autre protège encore. Vérifié septembre 2026 : supporté en
production par **Chrome, Firefox, Cloudflare, AWS, Windows Schannel**. Concrètement : une
partie du trafic TLS mondial est **déjà** résistante au harvest-now-decrypt-later côté
échange de clés. Reste classique : **l'authentification** (certificats) — la migration des
signatures (ML-DSA) est le chantier 2027-2030. Action : vérifier que vos terminaisons TLS
négocient bien les groupes hybrides, et planifier la PKI PQC.

## 152. PQC dans les HSM : Quantum Protect, Luna PQC FM (vérifié)

Les HSM suivent (vérifié le 27/09/2026, suivi pqca/wg-readiness-tracking) :

- **Utimaco u.trust GP HSM** : package **Quantum Protect**, statut **« Ready »** —
  supporte **ML-KEM, ML-DSA, XMSS, LMS, HSS**, avec un **simulateur gratuit** pour tester
  la migration. Argument fort : support PQC **sans changement de matériel**
  (crypto-agilité matérielle).
- **Thales Luna** : **PQC Functionality Module** documenté (mécanismes post-quantiques
  dans le HSM).
- **YubiHSM 2** (firmware 2.4+) : PQC **en test pilote**, support complet non prévu —
  pour une racine PQC future, prévoir un autre matériel.

Leçon : à l'achat d'un HSM en 2026, exiger le **support PQC logiciel** (au minimum par
mise à jour firmware) — un HSM incapable de ML-KEM/ML-DSA sera obsolète avant sa fin de
vie mécanique.

## 153. L'affaire HAWK : juillet 2026 (vérifié)

En **juillet 2026**, une équipe incluant un modèle d'IA d'Anthropic a trouvé une
**faiblesse réelle** dans **HAWK**, un schéma de signature à base de réseaux encore en
évaluation au NIST. L'équipe HAWK l'a **retiré** de la compétition. Aucun système en
production ne l'utilisait — rien n'a cassé. La leçon pour ce guide : **« post-quantique »
ne veut pas dire « définitivement sûr »**. D'où l'exigence de **crypto-agilité** : la
capacité à changer d'algorithme sans ré-architecturer (c'est le critère n°1 d'un achat
HSM/KMS en 2026, devant le choix de l'algorithme lui-même).

## 154. Confidential computing : Intel TDX, AMD SEV-SNP (vérifié)

Le **confidential computing** chiffre la **mémoire vive** des VM : ni l'hyperviseur, ni
l'opérateur cloud, ni un accès physique à la RAM ne voient les données en clair
(vérifié, documentation 2026) :

- **Intel TDX** (Trust Domain Extensions) : VM confidentielles sur Xeon Scalable
  (4e gén. et suivantes).
- **AMD SEV-SNP** (Secure Encrypted Virtualization – Secure Nested Paging) : sur EPYC
  (Milan/Genoa et suivants), avec protection d'intégrité et attestation.
- **Arm CCA** (Confidential Compute Architecture, Armv9) : Realm isolés gérés par le
  RMM — edge et serveurs Arm (vérifié).
- Usage : **attestation à distance** (section 56) + distribution de secrets (le secret
  n'est délivré qu'à une VM attestée — projet **Trustee**/Confidential Containers).

Pour un datacenter : TDX/SEV-SNP protègent les workloads sensibles même contre
l'administrateur de l'hyperviseur — complément du chiffrement disque et du HSM.

## 155. DDRop : l'attaque de septembre 2026 (vérifié)

Le **14 septembre 2026**, des chercheurs (KU Leuven, ETH Zurich, Durham, Google) ont
divulgué **DDRop** : avec un interposeur DDR5 à **moins de 200 $**, ils ont cassé les
garanties d'intégrité mémoire d'**Intel TDX, Intel Scalable SGX et AMD SEV-SNP** — en
faisant « disparaître » des écritures mémoire (le processeur relit des données chiffrées
périmées sans s'en apercevoir). Ils ont pu lire la mémoire d'une VM victime et **forger
la mesure d'attestation**. La leçon : le chiffrement mémoire **sans fraîcheur garantie**
a des limites face à un attaquant **physique** équipé. Pour un datacenter on-prem avec
accès physique contrôlé (partie 4), le risque est contenu ; pour du cloud public, c'est
un argument pour la **défense en profondeur** (contrôle d'accès + attestation + HSM),
jamais une technologie seule.

## 156. NVIDIA H100 CC, Arm CCA, Red Hat CoCo (vérifié)

- **NVIDIA H100 Confidential Computing** : mode CC du GPU — VRAM chiffrée
  (AES-256-GCM), transferts CPU-GPU chiffrés (Confidential CUDA), attestation du GPU.
  Surcoût : **~2-5 %** de débit (ordre de grandeur constaté). S'intègre avec TDX/SEV-SNP
  pour une chaîne CPU+GPU attestée (vérifié, 2026).
- **Red Hat OpenShift** : support **Confidential GPU** en Technology Preview dans
  « OpenShift sandboxed containers 1.12 » (avril 2026, vérifié) — inférence de modèles
  propriétaires sur infra non fiable, clés délivrées après double attestation CPU+GPU.
- **Arm CCA** : overhead **1-5 %** annoncé, intéressant pour l'edge industriel
  (voir section 154).

## 157. Ce qui n'est pas encore là : la liste honnête

Au 27/09/2026, **non trouvé / non mature** :

- **FIPS 206 (HQC)** finalisé — en cours (section 148).
- Migration **complète** des PKI vers ML-DSA (les certificats PQC restent rares en
  production ; le hybride domine).
- Support PQC **natif et complet** dans tous les HSM d'entrée de gamme (YubiHSM 2 :
  pilote uniquement).
- Attestation **standardisée et interopérable** entre tous les TEE (les formats
  divergent encore ; les frameworks comme Veraison/Trustee progressent).
- Ordinateur quantique cryptographiquement pertinent — **aucun au 27/09/2026**
  (les estimations crédibles parlent de 2029-2032 au plus tôt, avec une large
  incertitude).

Ne pas acheter sur des promesses : exiger des **références déployées** et des
**validations FIPS/Common Criteria** datées.

# PARTIE 10 — APPROFONDISSEMENTS HSM : L'INTÉGRATION AU QUOTIDIEN

## 158. Intégrer un HSM à Microsoft AD CS (pas-à-pas)

1. Installer le **KSP/CSP** du HSM sur le serveur AD CS (YubiHSM KSP, Luna CSP...).
2. Lors de l'installation du rôle AD CS, choisir « utiliser un module de stockage de
   clés existant » et sélectionner le provider du HSM.
3. Générer la clé **dans** le HSM (ne jamais générer en logiciel puis importer pour
   une AC — sauf migration documentée).
4. Vérifier : `certutil -store` doit montrer la clé avec le provider HSM ; un export
   doit **échouer** (clé non exportable).
5. Sauvegarde : dupliquer la clé vers le 2e HSM (cérémonie, section 133).
6. Tester l'émission et la révocation avant de mettre en production.
7. Documenter le provider, le slot et la procédure de bascule.

Piège : AD CS en **cluster** ou après migration de serveur — le KSP doit être installé
et la clé présente sur chaque nœud ; tester le failover.

## 159. OpenSSL provider + HSM : exemple nginx

Avec OpenSSL 3.x, le modèle **provider** (remplaçant les engines) permet à nginx de
faire les handshakes TLS avec la clé privée dans le HSM :

```
# openssl.cnf (extrait)
[provider_sect]
pkcs11 = pkcs11-provider

# nginx : la clé est référencée par URI PKCS#11, jamais copiée
ssl_certificate     /etc/nginx/certs/server.crt;
ssl_certificate_key "pkcs11:token=web;object=tls-key;type=private";
```

Points de vigilance : le PIN du token doit être fourni au démarrage (fichier protégé
ou variable d'environnement via un helper), la latence handshake augmente (~ms), et il
faut **cacher les sessions** (session tickets/resumption) pour ne pas saturer le HSM.
Dimensionner : chaque handshake RSA-2048 = 1 op asymétrique — à 1 000 handshakes/s,
prévoir ~500 tps utiles (voir section 36).

## 160. EJBCA + HSM : les points de vigilance

- Le **CryptoToken PKCS#11** d'EJBCA pointe vers la lib du HSM ; tester avec
  `p11tool` avant de configurer.
- Séparer les **tokens** : un token/slot par CA (racine, intermédiaire) — jamais le
  même slot pour la racine offline et l'émettrice en ligne.
- Les **workers** (signature OCSP, CMP) utilisent le même HSM : dimensionner le débit
  cumulé.
- Sauvegarde EJBCA (base de données) **et** HSM : restaurer l'un sans l'autre ne sert à
  rien — tester la restauration complète (section 138).
- Durcissement : EJBCA sur OS durci, accès admin via client cert, pas d'Internet.

## 161. Monter une TSA avec OpenTSA + HSM

**OpenTSA** (openssl ts) + clé dans le HSM via provider PKCS#11 :

1. Générer la clé TSA dans le HSM, émettre le certificat TSA (EKU = timeStamping,
   **critique**, un seul usage).
2. Configurer `openssl ts -reply` avec `-signer` pointant vers le provider.
3. Source de temps : **NTS** (Network Time Security) vers des serveurs de confiance,
   avec monitoring du décalage (une TSA dont l'horloge dérive émet des jetons
   contestables).
4. Dimensionner : 1 jeton = 1 signature — à 100 jetons/s, un petit HSM suffit ; à
   10 000/s, prévoir le modèle adapté.
5. Publier la politique de la TSA (précision, exactitude) — exigé pour le qualifié
   eIDAS.

## 162. DNSSEC avec HSM : BIND 9 / Knot

- La **KSK** dans le HSM (PKCS#11), la **ZSK** en logiciel avec rotation fréquente
  (90 jours) ou aussi en HSM selon la politique.
- BIND 9 : `dnssec-keyfromlabel` pour générer la clé **dans** le HSM, configuration
  `key-directory` + moteur PKCS#11.
- **Rollover KSK** : procédure RFC 6781 (double signature) — planifier des semaines à
  l'avance (propagations DNS, TTL).
- Sauvegarde : la KSK perdue = impossible de signer la zone — backup HSM obligatoire.
- Volume : quelques signatures par jour (signatures de RRset à chaque changement de
  zone) — le plus petit HSM suffit.

## 163. Cluster Luna HA : principes de configuration

Le mode **HA** de Thales (principe général, détails dans la doc constructeur) :

1. Créer un **groupe HA** : un membre primaire, un ou plusieurs secondaires.
2. Les objets (clés) sont **répliqués** chiffrés entre membres ; les clients voient un
   slot virtuel unique.
3. Le client bascule automatiquement en cas de panne d'un membre (timeout
   configurable).
4. **Ne pas confondre** : la HA protège de la panne, pas de la compromission logique
   ni de la perte de tous les membres (d'où le backup offline, section 41).
5. Tester la bascule **réellement** (débrancher un membre) avant la mise en production
   — une HA jamais testée est une supposition.

## 164. Négocier l'achat d'un HSM : retours terrain

- Les prix publics n'existent pas : tout se fait **sur devis** via partenaires. Demander
  **3 devis** (Thales, Utimaco, Entrust) avec la **même** fiche de besoin (débit,
  partitions, certifications, support).
- Négocier le **support** (15-20 %/an) : durée d'engagement, SLA de remplacement
  (J+1 vs 4 h), inclus ou non des mises à jour firmware majeures.
- Demander une **maquette** (POC) : les constructeurs prêtent du matériel 30-60 jours
  pour valider l'intégration (PKCS#11 avec vos applis).
- Vérifier la **fin de commercialisation** du modèle (pas d'achat sur une gamme en
  fin de vie sans remise massive et stock de pièces).
- Faire chiffrer la **prestation d'installation + cérémonie** : souvent 5-10 k€, à
  intégrer au budget dès le départ (section 48).

## 165. TCO comparé : 3 scénarios sur 5 ans (détaillé)

Hypothèses : besoin ~500 ops/s, 3 partitions, HA (2 nœuds).

| Poste (5 ans) | A : 2× appliance achetée | B : AWS CloudHSM (2 HSM) | C : 4× YubiHSM 2 + Vault |
|---|---|---|---|
| CAPEX matériel | ~50 000 € | 0 € | ~2 600 € |
| Support/licences 5 ans | ~45 000 € | 0 € | 0 € |
| Abonnement cloud 5 ans | 0 € | ~130 000 $ | 0 € |
| Prestation install. | ~8 000 € | ~3 000 € | ~5 000 € |
| Électricité 5 ans | ~1 500 € | 0 € | ~100 € |
| Temps humain (estimé) | ~30 k€ | ~15 k€ | ~35 k€ |
| **Total ordre de grandeur** | **~135 k€** | **~145 k$** | **~43 k€** |

Lecture : le scénario C (YubiHSM) ne convient que si le débit réel est faible
(dizaines d'ops/s) et sans besoin de clustering natif — sinon c'est A ou B. Entre A et
B, le **seuil** est à ~3-4 ans (section 48) ; B gagne en flexibilité et en absence de
gestion matérielle. Tous les chiffres sont **indicatifs, à vérifier** sur devis.

## 166. Migrer une PKI existante vers le HSM

1. **Inventaire** : où sont les clés aujourd'hui ? (fichiers, keystores Java, Windows...)
   — c'est souvent la découverte la plus pénible.
2. **Nouvelle hiérarchie** : générer la nouvelle racine **dans** le HSM (cérémonie).
   Ne pas « importer » l'ancienne racine sauf contrainte absolue (et dans ce cas,
   cérémonie d'import traçée, puis destruction des copies logicielles avec PV).
3. **Coexistence** : faire signer une intermédiaire par la nouvelle racine, migrer les
   émettrices par vagues, maintenir l'ancienne chaîne le temps de l'expiration.
4. **Nettoyage** : détruire les anciennes clés logicielles (PV, section 100), révoquer
   les anciens certificats en fin de vie.
5. **Durée typique** : 3-6 mois pour une PKI d'entreprise — prévoir la communication
   (les applications doivent basculer de chaîne).

## 167. FAQ HSM (10 questions)

1. **Un HSM ralentit-il les applications ?** La latence réseau (~1-5 ms) ne se sent que
   sur des opérations à très haute fréquence ; le cache et la résumption TLS absorbent
   le reste. Mesurer avant d'optimiser.
2. **Faut-il un HSM pour Let's Encrypt ?** Non — les certificats publics à 90 jours se
   gèrent en logiciel ; le HSM sert aux clés **longues et critiques** (racines, KEK).
3. **HSM + virtualisation ?** Appliance réseau : aucun problème (le HSM est distant).
   Carte PCIe : PCI passthrough vers une VM dédiée.
4. **Que se passe-t-il si on perd le PIN SO ?** Selon les modèles : réinitialisation
   d'usine (= perte des clés) ou procédure constructeur. D'où le MofN (section 43).
5. **Peut-on virtualiser un HSM ?** Non — c'est du matériel. On virtualise l'**accès**
   (partitions, clients multiples).
6. **HSM et sauvegardes Veeam ?** Les clés ne sont pas dans les VM sauvegardées si
   elles sont dans le HSM — c'est le but. Sauvegarder **séparément** (section 41).
7. **Durée de vie d'un HSM ?** 7-10 ans typiques (batterie, support constructeur).
   Planifier le renouvellement à 5 ans.
8. **HSM d'occasion ?** Possible via reconditionneurs sérieux **avec PV de reset** ;
   jamais sans (section 66). Budget divisé par 2-3, mais support à vérifier.
9. **Un seul HSM suffit-il ?** Pour une racine offline : oui (avec backup au coffre).
   Pour de la production : **non** — cluster de 2 minimum.
10. **Le quantique casse-t-il mon HSM ?** Pas le boîtier : les algorithmes. D'où la
    crypto-agilité et le PQC (partie 9). Le HSM protège les clés **quel que soit**
    l'algorithme.

# PARTIE 11 — APPROFONDISSEMENTS SERVEURS / ROOT OF TRUST

## 168. Inventaire Redfish : script et automatisation

Redfish (HTTPS/JSON) permet d'inventorier firmware, Secure Boot et TPM sans agent :

```
# Exemple de principe (à adapter, identifiants en coffre-fort)
curl -sk -u admin:$PASS https://bmc-serveur01/redfish/v1/Systems/1 \
  | jq '.BiosVersion, .SecureBoot'
curl -sk -u admin:$PASS https://bmc-serveur01/redfish/v1/UpdateService/FirmwareInventory
```

Automatisation : un script (Ansible/Python) qui parcourt les BMC, compare les versions à
la **baseline validée**, et alerte sur les écarts. Fréquence : hebdomadaire. C'est la
base du registre de firmware (section 62) et de l'audit Secure Boot (section 131).
Stocker les identifiants BMC dans un **coffre** (Vault), jamais dans le script.

## 169. Keylime : déployer l'attestation en pratique

**Keylime** (open source, CNCF) : l'agent sur chaque serveur envoie les mesures (quotes
TPM) au vérificateur, qui compare aux valeurs de référence et peut **révoquer** l'accès
(ex. retirer un nœud du cluster). Déploiement type :

1. Installer le vérificateur + registrar sur une machine dédiée (VLAN management).
2. Enregistrer les EK des serveurs (prouver l'authenticité des TPM).
3. Capturer les **mesures de référence** sur un serveur sain (golden).
4. Déployer l'agent, brancher la décision à l'orchestrateur (ex. : un nœud non attesté
   ne reçoit pas ses secrets / est exclu du scheduling).
5. **Processus** : à chaque mise à jour firmware/noyau, re-capturer les références —
   sinon alerte massive (le piège n°1 de l'attestation).

## 170. Sceller LUKS au TPM : pas-à-pas (principe)

1. TPM provisionné, Secure Boot activé (sinon le scellement ne veut rien dire).
2. Enroller la clé LUKS dans le TPM liée aux PCR 0+7 (firmware + secure boot) :
   `systemd-cryptenroll --tpm2-device=auto --tpm2-pcrs=0+7 /dev/sdX`.
3. Tester : reboot → déverrouillage **sans mot de passe** si les PCR sont conformes.
4. Tester le cas d'échec : modifier un réglage UEFI → le déverrouillage doit **échouer**
   (puis déblocage manuel avec la passphrase de secours).
5. Conserver la **passphrase de secours** au coffre (c'est elle qui sauve en cas de
   changement de carte mère).

Limite : protège contre le **vol du disque**, pas contre un attaquant avec accès au
serveur allumé (la clé est en RAM).

## 171. Durcir GRUB et le boot Linux

- Mot de passe GRUB (empêche l'édition des paramètres au boot — le contournement
  `init=/bin/bash`).
- `shim` + GRUB + noyau **signés** (Secure Boot), modules noyau signés
  (`CONFIG_MODULE_SIG_FORCE`).
- Paramètres noyau durcis : `slab_nomerge`, `pti=on`, `vsyscall=none`, IOMMU activé
  (`intel_iommu=on` / `amd_iommu=on` — protège aussi contre le DMA malveillant, en lien
  avec les attaques type DDRop).
- `/boot` en lecture seule si possible, vérification périodique des hashes.

## 172. BMC : centraliser la gestion d'un parc

Au-delà de 10 serveurs, gérer les BMC un par un ne passe plus à l'échelle :

- **Inventaire centralisé** : CMDB avec IP BMC, versions, mots de passe (coffre).
- **Mises à jour groupées** : via les outils constructeur (HPE OneView, Dell OME,
  Supermicro SSM) ou Redfish scripté — par vagues, avec validation.
- **Comptes** : un compte d'administration central (LDAP/AD) + un compte local de
  secours par BMC (mot de passe unique au coffre).
- **Supervision** : tous les syslog BMC vers le SIEM, tableau de bord des versions.
- **Règle** : aucun BMC accessible sans passer par le **bastion** du VLAN management.

## 173. Microcodes CPU : pourquoi et comment les maintenir

Les **microcodes** corrigent des bugs CPU, dont des failles de sécurité (Spectre/Meltdown
et leurs descendants). Ils se chargent au boot (via le firmware UEFI ou l'OS). En
datacenter :

- Suivre les bulletins Intel/AMD, appliquer via mise à jour UEFI **ou** package OS
  (`intel-microcode` / `amd64-microcode` sous Debian/Ubuntu).
- Après mise à jour : **re-mesurer** (les PCR peuvent changer) et re-tester les perfs
  (certains mitigations coûtent des % de performance — à mesurer, pas à subir).
- Un parc avec des microcodes de 3 ans = des failles connues exploitables localement.

## 174. Cas chiffré : sécuriser 50 serveurs (BOM + planning)

**Périmètre** : 50 serveurs 2U, dont 5 critiques (PKI, Vault, hyperviseurs).

| Poste | Détail | Budget indicatif |
|---|---|---|
| dTPM 2.0 (5 serveurs critiques) | Modules/options OEM | ~500 € |
| Temps : durcissement BMC (50) | 1 h/serveur | ~2 semaines-homme |
| Temps : Secure Boot + TPM + LUKS | 2 h/serveur | ~3 semaines-homme |
| Keylime (vérificateur + agents) | 1 semaine-homme + infra | ~5 000 € |
| Formation équipe (3 pers.) | 2 jours | ~4 000 € |
| Prestation d'audit initial | 5 jours | ~7 500 € |
| **Total** | | **~25-35 k€ + temps interne** |

**Planning** : semaine 1-2 audit et baseline ; 3-6 durcissement par vagues de 10 ;
7 Keylime ; 8 tests d'intrusion et recettes. Le poste « temps interne » est le vrai coût
— le prévoir dans la charge de l'équipe.

## 175. FAQ RoT (8 questions)

1. **fTPM ou dTPM ?** dTPM sur les serveurs critiques, fTPM acceptable ailleurs **si**
   patché. Le dTPM coûte quelques dizaines d'euros — ne pas lésiner sur les critiques.
2. **Secure Boot casse-t-il le dual-boot / les modules DKMS ?** Les modules non signés
   sont bloqués : signer ses modules (MOK) ou utiliser des noyaux/modules signés.
3. **L'attestation remplace-t-elle l'antivirus ?** Non — elle vérifie l'**intégrité du
   boot**, pas le comportement à l'exécution. Complémentaire.
4. **TPM + cloud public ?** Les vTPM existent (attestation par l'hyperviseur) — modèle
   de confiance différent (on fait confiance au provider, voir partie 9).
5. **Que faire d'un serveur sans TPM ?** Le prévoir au renouvellement ; en attendant,
   durcissement OS + chiffrement avec passphrase au coffre + surveillance renforcée.
6. **BMC compromis = serveur à jeter ?** Non : reflash complet du BMC depuis l'image
   officielle, changement de tous les secrets, vérification des firmwares — mais avec
   une procédure écrite et un contrôle.
7. **SPDM est-il déployé ?** En 2026 : sur les plateformes récentes et les composants
   neufs — **à vérifier** à l'achat (c'est une case du cahier des charges).
8. **Le measured boot ralentit-il le démarrage ?** De quelques secondes (hash des
   composants) — négligeable sur un serveur.

# PARTIE 12 — APPROFONDISSEMENTS FPGA : CHIFFRER LE ROI

## 176. Étude ROI : offload IPsec sur FPGA (calcul complet)

**Contexte** : 40 Gb/s d'IPsec site-à-site à traiter, actuellement sur CPU (8 cœurs
dédiés sur 4 serveurs).

| Poste | CPU seul | Avec FPGA (2× cartes) |
|---|---|---|
| Cœurs CPU mobilisés | 32 cœurs (~4 serveurs partiellement) | ~2 cœurs (contrôle) |
| Cartes FPGA | 0 € | ~19 000 $ (2× V80 MSRP — à vérifier) |
| Serveurs évités / réalloués | — | ~2 serveurs (~16 000 €) |
| Énergie : CPU 32 cœurs (~400 W) vs FPGA (380 W + 30 W contrôle) | ~400 W | ~410 W |
| Latence IPsec | ~50-200 µs (variable) | ~5-20 µs (déterministe) |
| Développement/intégration | 0 € | ~30-60 k€ (presta ou interne) |

**Verdict** : à débit égal, le FPGA ne fait pas forcément économiser l'énergie ici —
il **libère des cœurs** et **garantit la latence**. Le ROI se calcule en **serveurs
évités** et en **SLA réseau**, pas en watts. Si les 32 cœurs sont réalloués à du
revenu (VM clients), le FPGA se paie en **12-18 mois**. Sinon, rester sur CPU.

## 177. SmartNIC vs DPU vs FPGA brut : quoi choisir

| Solution | Exemples | Contrôle | Effort | Idéal pour |
|---|---|---|---|---|
| **FPGA brut** (Alveo V80) | Carte PCIe + votre design | Total (RTL/HLS) | Élevé (mois) | Protocole propriétaire, latence extrême |
| **SmartNIC FPGA** | N6001-PL, cartes ODM | Moyen (firmware + partie programmable) | Moyen | Offload réseau standard (OVS, IPsec) |
| **DPU/ASIC** | NVIDIA BlueField, Intel Mount Evans | Faible (SDK) | Faible (semaines) | Cas standards, volumes |

Règle : **DPU** si votre besoin est standard (c'est du produit), **SmartNIC FPGA** si
vous avez besoin de logique custom modérée, **FPGA brut** si le différenciateur est
dans le silicium que vous dessinez. Ne pas payer le prix du FPGA brut pour un usage
DPU.

## 178. Sécurité du bitstream en profondeur

Le bitstream est un **actif critique** : il contient votre propriété intellectuelle et
définit le comportement matériel.

- **Chiffrement** : les FPGA modernes (Versal, Agilex) chiffrent le bitstream stocké en
  flash (AES), déchiffré à l'intérieur de la puce avec une clé **en eFuses/BBRAM**.
- **Authentification** : signature (RSA/ECDSA) vérifiée avant configuration — un
  bitstream modifié ne charge pas.
- **Chaîne d'approvisionnement** : générer les clés de bitstream en interne, les
  stocker en MofN (même discipline que les clés crypto, partie 5).
- **Mise à jour** : canal sécurisé (TLS mutuel), version vérifiée, rollback possible.
- **Menace** : un attaquant qui remplace le bitstream d'une carte réseau FPGA contrôle
  le trafic — traiter les cartes FPGA comme des **équipements de sécurité**, pas comme
  des périphériques banals.

## 179. Tester un FPGA avant achat : la méthode

1. **Définir le benchmark** : votre workload réel (pas un benchmark synthétique du
   vendeur) — débit, latence p99, conso mesurée à la prise.
2. **POC 30 jours** : les distributeurs/constructeurs prêtent des cartes ; exiger le
   kit de développement et un exemple proche de votre cas.
3. **Mesurer** : latence (hardware timestamping), débit soutenu 24 h (stabilité
   thermique), erreurs.
4. **Chiffrer le TCO** : carte + serveur + développement + formation (section 88).
5. **Décider** sur les chiffres, pas sur la démo — et documenter la décision
   (si le FPGA ne passe pas le POC, c'est une économie, pas un échec).

## 180. FPGA et PQC : préparer la transition

Les algorithmes PQC (ML-KEM, ML-DSA) ont des **clés et signatures plus grosses** et des
profils de calcul différents de RSA/ECC — les accélérateurs actuels devront évoluer :

- Le FPGA est **idéal** pour cette transition : reprogrammable quand les paramètres
  changent (cf. affaire HAWK, section 153), alors qu'un ASIC crypto serait figé.
- Prévoir dans les designs crypto-FPGA une **marge de ressources** (LUT/BRAM) pour les
  futurs algos PQC.
- Les moteurs câblés (ex. 400G crypto du V80) gèrent le **symétrique** (AES) — non
  impacté par le quantique en pratique (AES-256 résiste) ; c'est l'**asymétrique**
  qui migrera.

## 181. Refroidissement : dimensionner l'airflow (calcul)

Une carte passive de 190 W (V80) dans un serveur 2U :

- Air nécessaire (ordre de grandeur) : P = ṁ × Cp × ΔT → pour ΔT = 15 °C,
  ṁ ≈ 190 / (1005 × 15) ≈ 0,0126 kg/s ≈ **~25 CFM** à travers la carte.
- Vérifier que le serveur fournit ce débit **au niveau du slot** (courbes constructeur,
  pas de valeur « moyenne châssis »).
- Température d'entrée d'air ≤ 30 °C recommandée (au-delà, les ventilateurs montent en
  régime = bruit + conso + usure).
- **Ne jamais** obstruer le slot adjacent, ne jamais laisser de câble devant la carte.
- En baie : 4 serveurs × 2 FPGA = **1,5 kW** de plus à évacuer — à intégrer au bilan
  thermique de la baie (section 142).

## 182. Comparatif perf/watt chiffré (tableau indicatif)

Ordres de grandeur pour du **chiffrement AES-GCM à haut débit** (à mesurer sur votre
workload — les chiffres varient fortement) :

| Plateforme | Débit AES-GCM typique | Puissance | Efficacité indicative |
|---|---|---|---|
| CPU (AES-NI, 1 cœur) | ~5-10 Gb/s | ~15 W/cœur | ~0,5 Gb/s/W |
| CPU (multi-cœurs, 16) | ~80-150 Gb/s | ~250 W | ~0,5 Gb/s/W |
| FPGA (moteurs câblés) | ~400 Gb/s (3× 400G sur V80) | ~190 W (carte) | ~2 Gb/s/W |
| ASIC (moteur dédié) | Ligne-rate | ~10-30 W | > 10 Gb/s/W |

Le FPGA gagne **~4×** en efficacité sur le CPU pour le chiffrement en ligne — c'est là
que l'argument énergie est le plus fort. Pour du calcul irrégulier, l'écart se réduit.

## 183. FAQ FPGA (8 questions)

1. **Faut-il savoir coder en VHDL ?** Pour un POC : non (HLS + exemples). Pour de la
   production optimisée : oui, ou un prestataire.
2. **Un FPGA remplace-t-il un GPU pour l'IA ?** Non pour l'entraînement et les LLM ;
   oui pour de l'inférence **figée** à latence garantie (vision, scoring).
3. **Durée de vie d'une carte ?** 5-7 ans (support outils, disponibilité) ; le design
   est portable vers la génération suivante avec effort.
4. **Le FPGA consomme-t-il moins qu'un GPU ?** En **perf/watt sur workload fixe** :
   souvent oui. En absolu : 190 W reste 190 W à évacuer.
5. **Peut-on louer du FPGA ?** Oui (instances cloud F1/FPGA chez les hyperscalers) —
   bien pour le POC, à chiffrer vs achat au-delà de 6 mois.
6. **Bitstream et sécurité ?** Voir section 178 — à traiter comme un firmware critique.
7. **FPGA + virtualisation ?** PCI passthrough vers une VM dédiée ; le partage fin
   (vFPGA) reste un sujet de recherche/édition entreprise.
8. **Quel est le premier pas ?** Une carte d'occasion U250 (~1-2 k€) + Vivado + un
   tutoriel packet-processing : 1 mois pour savoir si votre équipe accroche.

# PARTIE 13 — APPROFONDISSEMENTS KMS / PHYSIQUE : LA MÉTHODE

## 184. Modèle de menaces : l'atelier en 2 heures

Avant d'acheter, **modéliser** : réunir 4-6 personnes (infra, sécu, métier, énergie)
et répondre par écrit :

1. **Actifs** : quelles clés ? quelles données ? (inventaire, section 119 n°9)
2. **Adversaires** : voleur opportuniste ? concurrent ? étatique ? insider ?
   (le niveau d'adversaire dimensionne tout — un HSM FIPS L3 contre un voleur de
   disques, c'est cohérent ; contre un étatique, c'est une couche parmi d'autres)
3. **Vecteurs** : vol physique, accès réseau, insider, fournisseur, panne ?
4. **Impacts** : chiffrer en euros l'heure d'arrêt et la fuite (section 146).
5. **Mesures existantes** : que couvre-t-on déjà ?
6. **Écarts** : la liste d'achats qui en découle — **priorisée par risque**, pas par
   catalogue vendeur.

Livrable : 2 pages. Refaire **annuellement** ou à chaque changement majeur. C'est
l'exercice qui évite d'acheter un HSM à 30 k€ quand le vrai risque est le mot de passe
BMC par défaut (section 67).

## 185. Vault : politiques et durcissement (exemples)

Principes (HashiCorp Vault, transposables) :

```
# Politique : une appli ne lit que ses secrets
path "secret/data/appli-paiement/*" {
  capabilities = ["read"]
}
# Pas de list sur la racine, pas d'accès aux secrets des autres applis
```

Durcissement :

- **Auto-unseal** sur HSM (section 113), jamais de Shamir manuel en production
  automatisée.
- Backend **Raft** chiffré, snapshots **chiffrés** et testés.
- **Audit device** activé (fichier + syslog), non désactivable par les opérateurs.
- Authentification : **AppRole** ou OIDC pour les applis, jamais de token root qui
  traîne (le token root sert à l'initialisation puis est **révoqué**).
- Rotation des **credentials dynamiques** (bases de données) : durée de vie courte
  (heures), pas de mots de passe statiques.

## 186. Rotation automatisée des DEK : pattern

```
1. Générer DEK_n+1 dans le KMS/HSM
2. Re-envelopper : chiffrer DEK_n+1 avec la KEK → stocker
3. Double lecture : l'appli accepte DEK_n (déchiffrement ancien) et chiffre en DEK_n+1
4. Migration : re-chiffrer progressivement (ou au fil de l'eau / lazy)
5. Retirer DEK_n de la lecture après la fenêtre de migration
6. Archiver DEK_n (chiffrée) pour les anciennes sauvegardes, puis détruire à échéance
```

Automatisable à 90 % (cron + API KMS). La fenêtre de double lecture (étape 3) est le
point critique : trop courte = données illisibles, trop longue = exposition prolongée.
La documenter dans la politique de rotation (section 108).

## 187. Sauvegarde 3-2-1 appliquée aux clés

La règle **3-2-1** (3 copies, 2 supports différents, 1 hors site) adaptée aux clés :

- **Copie 1** : HSM de production (opérationnelle).
- **Copie 2** : HSM backup sur site (réplication chiffrée, testée).
- **Copie 3** : sauvegarde chiffrée **hors site** (coffre distant ou HSM du site de
  secours) — la KEK de cette sauvegarde en **MofN** (section 43).
- **Jamais** : de copie en clair, même « temporaire », même « chez le prestataire de
  confiance ».

Tester la restauration de la copie 3 **une fois par an** (c'est elle qui sauve en cas
de sinistre majeur — section 138).

## 188. Plan de câblage de la zone crypto

- **Courant fort** : PDU A et B sur chemins séparés, étiquetage par couleur
  (ex. rouge = A, bleu = B), jamais de multiprise sauvage.
- **Courant faible** : VLAN crypto sur switch dédié ou ports dédiés, jarretières
  étiquetées, longueur adaptée (pas de boucles de 3 m pour 30 cm).
- **Séparation** : goulottes séparées fort/faible, croisements à 90°.
- **Documentation** : plan de brassage à jour, chaque câble numéroté aux deux bouts.
- **Sécurité** : aucun câble du VLAN crypto ne sort de la baie sans passer par le
  firewall documenté (section 35) ; les ports non utilisés du switch sont
  **désactivés**.

## 189. Journalisation : architecture SIEM pour la crypto

```
[HSM] ──syslog/TLS──┐
[Vault] ──audit──┐    │
[BMC] ──syslog──┤    ├─▶ [Collecteur] ─▶ [SIEM] ─▶ alertes + tableaux de bord
[Firewall] ─────┘    │         │
[Badgeuse] ──────────┘         └─▶ [Stockage WORM / immuable]
```

Règles :

- Transport **chiffré** (TLS) et **horodatage fiable** (NTP/NTS sur tous les
  équipements — sans temps fiable, la corrélation est impossible).
- Stockage **immuable** (WORM) pour les journaux d'audit : un attaquant ne doit pas
  pouvoir effacer ses traces.
- **Alertes** : échec d'auth HSM ×5, export de clé, accès physique hors heures,
  dérive d'horloge > 1 s.
- **Revue** : quotidienne automatisée (règles), hebdomadaire humaine (échantillon).

## 190. Exercice de crise : scénario « clé compromise »

Déroulé d'un exercice annuel (2 h, table ronde + technique) :

1. **Injection** : « le SIEM remonte un export de la KEK sauvegardes à 3 h du matin
   depuis le poste d'un admin en congés ».
2. **Décisions** : qui est convoqué ? (cellule de crise : RSSI, infra, direction,
   juriste) — dans quel ordre ?
3. **Actions** : confinement (section 137), périmètre, communication interne/externe.
4. **Technique** : révoquer, régénérer, re-chiffrer — **chronométrer** chaque étape.
5. **Débrief** : ce qui a coincé (mot de passe du coffre introuvable ? quorum
   injoignable un dimanche ?) → plan d'action.

C'est l'exercice le plus rentable de l'année : il révèle les failles
**organisationnelles** que la technique ne voit pas.

## 191. FAQ KMS (8 questions)

1. **Vault remplace-t-il un HSM ?** Non — il le **complète** (section 111). Vault sans
   HSM = clé maîtresse logicielle = point faible.
2. **Combien de Vault ?** 1 cluster (3 nœuds Raft) suffit pour une ETI ; séparer par
   environnement (prod/preprod) si les exigences divergent.
3. **Où stocker les secrets des conteneurs ?** Via l'agent Vault / CSI driver, jamais
   en variables d'environnement en clair ni dans les images.
4. **BYOK ou HYOK ?** (section 116) — BYOK par défaut, HYOK pour le top critique.
5. **Faut-il chiffrer les sauvegardes Vault ?** Oui, et tester leur restauration
   (section 138) — une sauvegarde Vault contient tous les secrets.
6. **Que faire des anciens secrets ?** Rotation puis **révocation** : un secret
   « ancien mais toujours valide » est une bombe à retardement.
7. **KMS cloud ou on-prem ?** Cloud si les workloads sont cloud (latence, intégration) ;
   on-prem si souveraineté ou workloads locaux — souvent les deux (hybride).
8. **Le post-quantique change-t-il le KMS ?** Le KMS doit devenir **crypto-agile**
   (section 153) : stocker l'identifiant d'algorithme avec chaque clé, prévoir la
   migration ML-KEM/ML-DSA.

# GLOSSAIRE (40 TERMES)

1. **HSM (Hardware Security Module)** — Boîtier/carte inviolable qui génère, stocke et
   utilise des clés crypto sans les exposer en clair. Cœur de la partie 1.
2. **KMS (Key Management System)** — Logiciel/service gérant le cycle de vie des clés
   (ex. HashiCorp Vault, AWS KMS). Partie 5.
3. **Root of Trust (RoT)** — Composant implicitement fiable qui amorce la chaîne de
   confiance (silicium, TPM, firmware). Partie 2.
4. **TPM 2.0 (Trusted Platform Module)** — Puce/firmware cryptographique d'une machine :
   mesures (PCR), scellement, attestation. Norme TCG.
5. **PCR (Platform Configuration Registers)** — 24 registres du TPM accumulant les
   hashes des composants démarrés.
6. **Secure Boot** — Vérification des signatures à chaque étape du boot ; bloque si non
   signé.
7. **Measured Boot** — Enregistrement (hash) de chaque étape du boot dans le TPM ;
   détecte sans bloquer.
8. **Attestation (à distance)** — Preuve cryptographique qu'une machine a démarré dans
   un état sain, vérifiée par un tiers.
9. **BMC (Baseboard Management Controller)** — Contrôleur de management d'un serveur
   (iLO, iDRAC, OpenBMC) : ordinateur dans l'ordinateur, à durcir en priorité.
10. **PKCS#11 (Cryptoki)** — API standard d'accès aux HSM/tokens (slots, sessions, objets).
11. **FIPS 140-2 / 140-3** — Certification NIST des modules crypto ; le niveau 3 exige
    une réponse active à l'effraction.
12. **Common Criteria (CC)** — Certification internationale (ISO 15408) par niveaux EAL.
13. **eIDAS** — Règlement UE sur l'identification électronique et les services de
    confiance (signature, horodatage).
14. **QSCD** — Qualified Signature Creation Device : dispositif de création de signature
    qualifiée eIDAS (typiquement un HSM certifié).
15. **AC (Autorité de Certification)** — Émet et révoque les certificats ; sa clé racine
    vit dans un HSM.
16. **CRL / OCSP** — Liste de révocation / protocole de vérification en ligne des
    certificats.
17. **TSA (Time-Stamping Authority)** — Autorité d'horodatage (RFC 3161) ; clé dans HSM,
    horloge fiable requise.
18. **DNSSEC** — Extension sécurisant le DNS par signatures ; la KSK est typiquement en
    HSM.
19. **KEK / DEK** — Key Encryption Key (chiffre les clés) / Data Encryption Key (chiffre
    les données). Hiérarchie de clés.
20. **Envelope encryption** — Pattern : DEK chiffrée par la KEK, données chiffrées par
    la DEK.
21. **MofN (M of N)** — Quorum : M parts sur N nécessaires pour reconstituer un secret.
22. **Cérémonie (key ceremony)** — Procédure formelle et auditée de génération d'une clé
    critique.
23. **Zeroization** — Effacement immédiat des clés en cas d'attaque détectée.
24. **TRNG / DRBG** — Générateur d'aléa vrai (physique) / déterministe (NIST 800-90A).
25. **FPGA** — Circuit logique reprogrammable après fabrication ; parallélisme et
    latence déterministe.
26. **Bitstream** — Fichier de configuration d'un FPGA ; à chiffrer et signer.
27. **LUT / DSP slice** — Briques de base d'un FPGA : table logique / multiplieur câblé.
28. **HLS (High-Level Synthesis)** — Compilation de C/C++ vers circuit FPGA.
29. **SmartNIC / DPU** — Carte réseau programmable déchargeant le CPU (réseau, sécu,
    stockage).
30. **ASIC** — Circuit figé à la fabrication ; optimal en volume, nul en flexibilité.
31. **PQC (Post-Quantum Cryptography)** — Crypto résistante à l'ordinateur quantique
    (ML-KEM, ML-DSA, SLH-DSA).
32. **ML-KEM / ML-DSA / SLH-DSA** — Standards NIST FIPS 203/204/205 (échange de clés,
    signatures, signatures hash).
33. **CNSA 2.0** — Suite d'algorithmes imposée par la NSA aux systèmes US (calendrier
    2027-2031).
34. **Confidential computing** — Chiffrement de la mémoire des VM (Intel TDX, AMD
    SEV-SNP, Arm CCA, NVIDIA H100 CC).
35. **TEE (Trusted Execution Environment)** — Environnement d'exécution isolé matériellement.
36. **NIST 800-88** — Norme de sanitization des médias (Clear / Purge / Destroy).
37. **SED (Self-Encrypting Drive)** — Disque à chiffrement intégré (TCG Opal) ;
    mise au rebut par crypto-erase.
38. **SBOM** — Software Bill of Materials : inventaire des composants logiciels d'un
    produit.
39. **SPDM** — Protocole DMTF d'attestation des composants matériels (cartes, disques).
40. **Redfish** — API REST standard (DMTF) de gestion des serveurs ; remplace IPMI.

# QUIZ : 10 QUESTIONS + RÉPONSES

**Q1. Pourquoi une clé privée ne doit-elle jamais exister en clair hors d'un HSM ?**
R : Parce que toute copie en clair (fichier, sauvegarde, snapshot VM) est une
compromission potentielle : quiconque lit le support possède la clé. Le HSM garantit
que la clé naît, vit et meurt à l'intérieur du périmètre inviolable — seules les
opérations (signer, déchiffrer) sortent.

**Q2. Quelle différence entre secure boot et measured boot ?**
R : Le secure boot **vérifie** la signature et **bloque** si non signé ; le measured
boot **enregistre** le hash dans le TPM sans bloquer. Il faut les deux : le premier
empêche le connu-mauvais, le second permet de **détecter** (via attestation) ce qui a
vraiment démarré.

**Q3. Pourquoi le BMC est-il le talon d'Achille d'un serveur ?**
R : C'est un ordinateur indépendant (SoC, OS, réseau) avec accès total au serveur
(allumage, KVM, ISO), **invisible depuis l'OS**. Compromis (mot de passe par défaut,
firmware non patché), il donne un contrôle persistant que ni la réinstallation ni
l'antivirus ne voient.

**Q4. Dans quel cas choisir un FPGA plutôt qu'un GPU ?**
R : Quand le workload est **stable** (pas de changement fréquent), que la **latence
déterministe** est critique (réseau, trading, protocoles), et que l'écosystème logiciel
du GPU n'apporte rien. Pour l'IA générative et tout ce qui évolue vite : GPU.

**Q5. Qu'est-ce qu'un quorum MofN et pourquoi ne pas mettre toutes les parts dans le
même coffre ?**
R : La clé est découpée en N parts dont M sont nécessaires pour la reconstituer :
personne seul ne peut la restaurer. Si toutes les parts sont au même endroit, un seul
vol/accès les récupère toutes — le quorum ne vaut que par la **séparation physique et
organisationnelle**.

**Q6. Pourquoi l'auto-unseal de Vault est-il lié à l'alimentation électrique ?**
R : Sans auto-unseal, Vault redémarre scellé et exige des humains (Shamir). Avec
auto-unseal sur HSM, le descellement est automatique **si le HSM est disponible avant
Vault**. Après une coupure, l'ordre de redémarrage (HSM → Vault → applis) et
l'alimentation secourue du HSM conditionnent la reprise — d'où la règle « jamais sur
onduleur non secouru ».

**Q7. Que signifie « harvest now, decrypt later » et que faire dès 2026 ?**
R : Les adversaires enregistrent le trafic chiffré aujourd'hui pour le déchiffrer avec
un futur ordinateur quantique. Action : déployer le **TLS hybride** (X25519MLKEM768,
déjà supporté par les navigateurs et clouds), inventorier la crypto, exiger la
**crypto-agilité** des HSM/KMS, planifier la migration ML-DSA des signatures.

**Q8. Quelle est la méthode de destruction d'un disque contenant des données critiques
en fin de vie ?**
R : Selon NIST 800-88, niveau **Destroy** (broyage/désintégration) pour le top
critique, ou **Purge** (crypto-erase sur SED) avec **PV de destruction** tracé par
numéro de série. Un disque « jeté » lisible = fuite de données.

**Q9. Pourquoi tester la restauration des sauvegardes de clés une fois par an ?**
R : Parce qu'une sauvegarde jamais testée n'est pas une sauvegarde : mots de passe
perdus, KEK détruites, procédures obsolètes, quorum injoignable ne se révèlent qu'au
test. Le test DR crypto (section 138) mesure aussi le **RTO réel**.

**Q10. Un HSM protège-t-il contre la perte des clés (panne, incendie) ?**
R : Non — il protège contre le **vol et la copie**, pas contre la **perte**. D'où la
sauvegarde chiffrée (HSM backup, export wrappé en MofN), géographiquement séparée et
testée. Sans sauvegarde, une panne = clés perdues = données chiffrées irrécupérables.

# PIÈGES TERRAIN : 25 ERREURS À NE PAS COMMETTRE

1. **Générer la racine d'AC « vite fait » sur un poste admin** — sans cérémonie, sans
   témoin, clé exportable. Refaire une cérémonie après coup ne répare pas les copies.
2. **Importer une clé logicielle dans le HSM** « pour aller plus vite » — la clé a
   existé en clair, elle est potentiellement compromise dès le départ.
3. **Un seul HSM en production** — pas de HA : la première panne arrête les signatures.
4. **HSM sur le VLAN de production** — accessible à tout le réseau interne au lieu
   d'un VLAN crypto filtré.
5. **PIN SO utilisé au quotidien** — le compte d'initialisation devient le compte
   d'exploitation ; le compromettre = tout compromettre.
6. **Batterie du HSM ignorée** — elle s'épuise, le HSM s'efface un jour sans prévenir.
   Superviser et planifier le remplacement.
7. **Automatiser l'admin HSM sans revue** — un playbook Ansible avec les PIN en clair
   dans git annule tout le bénéfice du HSM.
8. **Secure Boot désactivé « pour dépanner » et jamais réactivé** — l'audit trimestriel
   (section 131) existe pour ça.
9. **BMC en DHCP sur le VLAN de production, mot de passe par défaut** — le scénario
   de la section 67, vu et revu.
10. **IPMI 1.5 laissé activé** — hashes craquables offline ; passer en 2.0 chiffré ou
    désactiver.
11. **Firmware téléchargé hors site constructeur** — « trouvé sur un forum » =
    porte dérobée potentielle.
12. **Mise à jour firmware sans re-mesure** — les PCR changent, toute l'attestation
    passe en alerte, et on finit par ignorer les alertes (pire).
13. **Acheter du FPGA pour un workload qui change tous les mois** — le design ne sera
    jamais fini ; c'était un job pour GPU/CPU.
14. **Carte FPGA passive sans vérifier l'airflow** — surchauffe, throttling, panne en
    pleine charge estivale.
15. **Bitstream non signé** — n'importe qui peut reprogrammer la carte réseau.
16. **Photos en salle serveur** — un tableau blanc, un écran, une étiquette : fuite
    d'information. Interdire et faire respecter.
17. **Badge prêté « pour 5 minutes »** — l'anti-passback et la culture (section 98)
    existent pour ça.
18. **Disques « effacés » par simple formatage** — formatage ≠ effacement ; appliquer
    NIST 800-88.
19. **SED acheté mais chiffrement jamais activé** — un SED non activé est un disque
    clair. Vérifier à la mise en service.
20. **Clé codée en dur dans le code** — elle finira dans git, dans une image Docker,
    dans une sauvegarde. Vault/secret manager, toujours.
21. **Vault en Shamir manuel sur infra à redémarrage automatique** — après chaque
    coupure, quelqu'un doit se lever à 3 h du matin. Auto-unseal (section 113).
22. **Rotation « quand on aura le temps »** — c'est jamais. Automatiser (section 186).
23. **Sauvegarde des clés en clair « temporairement »** — le temporaire dure des années
    et finit dans le cloud.
24. **HSM et serveurs critiques sur onduleur non secouru** — la règle d'or (section
    140) : une coupure = PKI à l'arrêt + Vault scellé.
25. **Acheter sur promesse PQC/quantique** — exiger des standards finalisés (FIPS
    203/204/205), des validations datées, des références déployées. Le reste est du
    marketing (section 157).

# CONCLUSION : LA CHECKLIST DU CHEF DE SERVICE

Si vous ne retenez que dix actions :

1. **Inventorier** les clés et les firmwares (on ne sécurise pas l'inconnu).
2. **Mettre les clés critiques en HSM** — même un YubiHSM 2 à 650 € change tout pour
   une PME.
3. **Cérémonie + MofN** pour chaque clé racine, PV archivé.
4. **Sauvegarder et tester** la restauration chaque année.
5. **Durcir les BMC** : mots de passe uniques, VLAN dédié, firmware à jour.
6. **Activer Secure Boot + TPM** sur 100 % du parc serveurs, auditer trimestriellement.
7. **Segmenter** : VLAN crypto pour les HSM, jamais d'Internet, TLS mutuel.
8. **Secourir électriquement** toute la chaîne crypto (HSM, Vault, PKI) et tester le
   redémarrage après coupure.
9. **Préparer le PQC** : TLS hybride maintenant, crypto-agilité exigée à chaque achat.
10. **Écrire les politiques** : sans documents, l'audit échoue quel que soit le matériel.

> *Guide rédigé le 27/09/2026. Références produits vérifiées par recherche web le
> 27/09/2026 ; prix et détails non sourcés marqués « à vérifier ». Relire avant tout
> achat : les gammes, les firmwares et les validations évoluent vite.*

# PARTIE 14 — DOSSIERS D'ACHAT, GOUVERNANCE ET MODÈLES DE DOCUMENTS

## 192. Cahier des charges HSM : template

Un RFP HSM sérieux contient :

1. **Contexte** : organisation, usages (PKI, TDE, TSA...), volumes (ops/s moyens et
   pics — voir section 36).
2. **Exigences fonctionnelles** : form factor, nombre de partitions/tenants, APIs
   (PKCS#11 obligatoire, JCE/CNG selon écosystème), clustering HA, sauvegarde.
3. **Exigences de sécurité** : FIPS 140-2/3 niveau 3 minimum (référence CMVP exigée),
   Common Criteria si marchés publics, support **PQC logiciel** (ML-KEM/ML-DSA).
4. **Exigences d'exploitation** : SNMP/syslog, API REST d'admin, MTBF, batterie
   remplaçable, support J+1 ou 4 h.
5. **Exigences électriques** : conso max, plage de température, double alimentation.
6. **Prestations** : installation, cérémonie, formation, POC 30 jours.
7. **Prix** : matériel, support annuel (5 ans), prestations — **détaillés par ligne**.
8. **Références** : 3 clients comparables déployés.

Envoyer le **même** RFP aux 3 finalistes (Thales, Utimaco, Entrust) pour comparer à
périmètre égal.

## 193. Grille de notation fournisseurs (exemple)

| Critère | Poids | Thales | Utimaco | Entrust |
|---|---|---|---|---|
| Conformité au besoin fonctionnel | 25 % | /20 | /20 | /20 |
| Certifications (FIPS, CC, PCI) | 15 % | | | |
| PQC / crypto-agilité | 10 % | | | |
| Performance mesurée en POC | 15 % | | | |
| TCO 5 ans | 15 % | | | |
| Support et SLA | 10 % | | | |
| Références clients | 10 % | | | |
| **Total pondéré** | 100 % | | | |

Noter **pendant** le POC, pas après ; faire noter par 2 personnes indépendantes ;
archiver la grille (pièce d'audit d'achat).

## 194. Planning projet PKI/HSM type (12 semaines)

| Semaines | Phase | Livrable |
|---|---|---|
| 1-2 | Cadrage : modèle de menaces, RFP | Dossier de cadrage |
| 3-5 | POC : 2 finalistes, tests d'intégration | Grille de notation, choix |
| 6-7 | Commande, réception, installation physique | PV de réception |
| 8 | Configuration : partitions, réseau, supervision | Dossier d'exploitation |
| 9 | Cérémonie de génération des clés | PV de cérémonie |
| 10 | Intégration applicative (AD CS/EJBCA, Vault) | Recette fonctionnelle |
| 11 | Formation équipe + runbooks | Attestations, runbooks |
| 12 | Test DR + bascule en production | PV de test, GO |

Prévoir **+4 semaines** de marge (délais fournisseurs, indisponibilités) — un projet
crypto ne se fait jamais « entre deux urgences ».

## 195. Gouvernance : RACI de la fonction crypto

| Activité | RSSI | Admin HSM | Exploitation | Direction | Auditeur |
|---|---|---|---|---|---|
| Politique crypto | A | C | C | R | I |
| Cérémonie | A | R | I | C | Témoin |
| Exploitation quotidienne | I | R | C | I | I |
| Rotation planifiée | A | R | C | I | Vérifie |
| Test DR annuel | A | R | R | I | Vérifie |
| Achat/renouvellement | C | C | I | A/R | I |

R = Responsible, A = Accountable, C = Consulté, I = Informé. L'adapter, l'afficher, le
revoir à chaque changement d'équipe. Le cumul R+A sur une seule personne pour la
cérémonie est **interdit**.

## 196. Former l'équipe : plan minimal

- **Admins HSM** (2 pers. min.) : formation constructeur (3-5 jours) + cérémonie
  assistée + habilitation.
- **Exploitants** : runbooks (sections 136-137), exercice de crise annuel
  (section 190).
- **Direction** : sensibilisation 2 h (risques, coûts d'arrêt, responsabilités) —
  c'est elle qui signe les budgets et les PV de cérémonie.
- **Recyclage** : 1 jour/an (nouveautés firmware, retours d'incidents, évolutions PQC).
- **Documentation** : tout est écrit (runbooks, schémas, mots de passe au coffre) —
  l'équipe doit survivre au départ d'un membre clé (**bus factor** ≥ 2 sur chaque
  compétence critique).

## 197. Veille sécurité : s'abonner aux bonnes sources

- **Bulletins constructeurs** : Thales, Utimaco, Entrust, HPE, Dell, Supermicro, AMD,
  Intel — alertes firmware critiques sous 30 jours (section 62).
- **ANSSI** : bulletins, guides d'hygiène (référence FR).
- **NIST** : évolutions FIPS 140-3, PQC (csrc.nist.gov).
- **Communautés** : listes Keylime, EJBCA, Vault (changements, failles).
- **Rituel** : revue mensuelle de 1 h (quoi de neuf ? quoi appliquer ?), décisions
  tracées. La veille non ritualisée n'existe pas.

## 198. Modèle de PV de cérémonie (template)

```
PROCÈS-VERBAL DE CÉRÉMONIE DE GÉNÉRATION DE CLÉS
Date/heure : ............  Lieu : ............
Objet : génération de la clé ............ (usage : ............)
Script version : ............

Participants :
- Officiant 1 : ............ (rôle : ............)
- Officiant 2 : ............ (rôle : ............)
- Témoin/auditeur : ............
- Scribe : ............

Matériel : HSM ............ (S/N : ............), firmware ............,
scellés vérifiés : OK / NOK (photos jointes).

Opérations :
1. ............ (heure : ....)
2. ............ (heure : ....)

Clé générée : algorithme ............, taille ............
Empreinte clé publique (SHA-256) : ............
  (relevée indépendamment par : ............ et ............)

Sauvegarde : méthode ............, vérifiée : OK / NOK
Supports remis : ............ (n°, custodians, signatures)

Incidents / écarts au script : ............

Signatures : ............ ............ ............
```

## 199. Modèle de fiche serveur (template)

```
FICHE SERVEUR — mise en service sécurisée
Nom : ............  S/N châssis : ............  S/N CM : ............
Baie/U : ............  VLAN : ............  IP BMC : ............

Firmware : UEFI ............, BMC ............, NIC ............, disques ............
Secure Boot : activé / clés ............
TPM : présent (dTPM/fTPM), EK vérifié : oui/non
Boot Guard/PSB : ............

BMC : mot de passe changé le ............ (coffre réf. ............),
      services : HTTPS/Redfish OK, Telnet/HTTP/IPMI1.5 désactivés : oui/non
      certificat remplacé : oui/non

OS : image ............ (hash ............), LUKS scellé TPM : oui/non
Comptes : ............

Date de mise en service : ............  Opérateur : ............
Prochaine revue firmware : ............
```

## 200. Modèle de registre firmware (template)

| Serveur | UEFI | BMC | NIC | Disques | FPGA/bitstream | Baseline ? | Écart | Action |
|---|---|---|---|---|---|---|---|---|
| srv-pki-01 | 2.14 | 7.02 | 21.5 | 4.01 | — | ✅ | — | — |
| srv-vault-02 | 2.14 | 6.98 | 21.5 | 4.01 | — | ❌ | BMC 6.98 < 7.02 | Planifier MAJ |
| srv-gpu-01 | 2.14 | 7.02 | 21.5 | 4.01 | bitstream v3.2 signé | ✅ | — | — |

Revu **mensuellement**, écarts traités sous 30 jours (critiques : sous 7 jours).
Source : inventaire Redfish automatisé (section 168).

## 201. Revue annuelle de la fonction crypto

Ordre du jour (1 journée) :

1. Bilan incidents de l'année (tentatives, pannes, alertes).
2. Résultats du **test DR** (section 138) et du test d'intrusion physique.
3. État des clés : inventaire, rotations faites/restantes, expirations à venir.
4. Firmwares : parc à jour ? écarts ?
5. Veille : nouveautés (PQC, failles, EOL produits) — impacts ?
6. Budget N+1 : renouvellements, extensions, formations.
7. Mise à jour des politiques et du modèle de menaces (section 184).

Compte-rendu à la direction : 2 pages, chiffrées (coûts, risques résiduels).

# PARTIE 15 — CAS D'ÉCOLE COMPLETS : DE LA THÉORIE AU DEVIS

## 202. Cas 1 : datacenter régional, ETI 400 personnes (BOM complète)

**Contexte** : ETI industrielle, 1 datacenter 20 baies, PKI interne (2 000 utilisateurs),
TDE sur 3 bases, Vault pour 40 applis, TSA pour les journaux, site de secours à 30 km.

**BOM sécurité/crypto** :

| Poste | Choix | Qté | Coût indicatif |
|---|---|---|---|
| HSM réseau (prod + HA) | Appliance FIPS 140-2 L3, ~5 000 tps | 2 | ~50 000 € |
| HSM backup (site secours) | Même gamme, modèle inférieur | 1 | ~18 000 € |
| YubiHSM 2 (racine offline) | Standard + FIPS | 2 | ~1 600 € |
| Serveurs PKI/Vault/TSA | 2U durcis, dTPM | 3 | ~24 000 € |
| Coffre-fort ignifuge | Clés, PV, backup | 1 | ~2 500 € |
| Contrôle d'accès (3 portes) | Badges DESFire + biométrie salle | 1 lot | ~12 000 € |
| Vidéosurveillance | 8 caméras + NVR 90 j | 1 lot | ~9 000 € |
| Câblage zone crypto + PDU metered | — | 1 lot | ~4 000 € |
| Prestations (install, cérémonie, formation) | — | 1 lot | ~18 000 € |
| Support HSM 5 ans (18 %/an) | — | — | ~61 000 € |
| **Total 5 ans** | | | **~200 000 €** |

Soit **~40 k€/an** — à comparer au coût d'une fuite de données industrielles ou d'une
journée d'arrêt de production (souvent supérieur). Le découpage en phases (année 1 :
HSM + PKI ; année 2 : site secours) lisse le budget.

## 203. Cas 2 : PME industrielle, 60 personnes, sans datacenter

**Contexte** : pas de salle serveur (hébergé chez un prestataire), besoin : PKI interne
(Wi-Fi, VPN), chiffrement des sauvegardes, signature des plans/contrats.

**Solution** : pas d'appliance — **2× YubiHSM 2** (650 € pièce) + EJBCA sur VM hébergée
+ Vault OSS pour les secrets + sauvegardes chiffrées (envelope encryption, DEK
enveloppées par une KEK du YubiHSM). Cérémonie simplifiée (2 officiants + témoin).
**Budget total : ~8-12 k€** (voir le détail section 118). Le point dur n'est pas le
matériel mais la **discipline** : cérémonie, sauvegardes testées, rotations. Quand
l'activité croît (> 100 signatures/heure, audit externe) : migrer vers l'appliance
(section 166).

## 204. Cas 3 : hébergeur / MSP, mutualisation

**Contexte** : hébergeur avec 30 clients, besoin de séparer les clés par client.

**Solution** : 2× appliance réseau en HA avec **partitions par client** (jusqu'à 20 par
boîtier chez Thales, 31 conteneurs chez Utimaco u.trust — sections 20, 24). Chaque
client a son CO et ses politiques ; facturation au client (le HSM devient un **centre
de profit**). Points de vigilance : le **partage du débit** (dimensionner le boîtier
pour la somme des pics), la **réversibilité** (export wrappé des clés si un client
part), et le **contrat** (qui est responsable en cas de compromission d'une partition
voisine ? — à cadrer juridiquement).

## 205. Dimensionnement électrique complet d'une salle crypto

Reprenons le cas 1 (section 202), zone crypto d'une baie :

| Équipement | Puissance max |
|---|---|
| 2× HSM réseau | 2× 110 W = 220 W |
| 3× serveurs 2U (PKI, Vault, TSA) | 3× 350 W = 1 050 W |
| 1× switch management | 60 W |
| 1× KVM/écran (ponctuel) | 50 W |
| **Sous-total** | **1 380 W** |
| Marge ×1,5 | **2 070 W** |

**Traduction** : 2 PDU 16 A (A+B), 1 disjoncteur 10 A par PDU, onduleur : quote-part
3 kVA avec 30 min d'autonomie (~1 kWh utile pour cette zone), clim : ~2,5 kW froid.
**Test** : coupure simulée semestrielle avec chronométrage de la séquence de
redémarrage (section 145). Coût annuel de la continuité électrique de cette zone :
~3 500 € (élec + maintenance onduleur) — dérisoire face au risque.

## 206. Planning PRA crypto (reprise après sinistre)

| Étape | Action | RTO cible | Responsable |
|---|---|---|---|
| 0 | Déclenchement : sinistre site principal | — | Direction |
| 1 | Mise en route site secours (élec, réseau) | 1 h | Infra |
| 2 | Allumage HSM backup, vérification partitions | 30 min | Admin HSM |
| 3 | Restauration KMS/Vault (auto-unseal ou MofN) | 30 min | Admin HSM |
| 4 | Restauration AD CS/EJBCA + CRL/OCSP | 1 h | Admin PKI |
| 5 | Restauration applis (TDE : déchiffrement OK ?) | 2 h | Exploitation |
| 6 | Tests : signature, déchiffrement, certificats | 30 min | RSSI |
| 7 | Communication clients | — | Direction |
| **Total** | | **~5 h 30** | |

Testé **une fois par an** en conditions réelles (section 138) — le RTO réel est
toujours supérieur au RTO papier la première fois.

## 207. Leçons apprises : ce que les incidents enseignent

Synthèse des retours d'expérience de l'industrie (pas d'invention : des patterns) :

1. **La panne arrive toujours pendant les congés** — d'où l'astreinte formée, les
   runbooks écrits pour un lecteur fatigué, et les mots de passe au coffre (pas « dans
   la tête de Karim »).
2. **C'est l'électrique qui tue la crypto** — 80 % des indisponibilités de HSM en PME
   viennent de coupures, pas d'attaques. La partie 8 n'est pas un bonus.
3. **La compromission vient de l'intérieur** — insider ou prestataire : d'où la
   séparation des rôles, les journaux, le MofN.
4. **Le firmware oublié** — BMC non patché depuis 3 ans : la faille était publique.
   Le registre (section 200) et la veille (section 197) sont des mesures de sécurité
   à part entière.
5. **La sauvegarde non testée** — découverte le jour du sinistre. Tester, tester,
   tester.
6. **Le projet crypto sous-budgété en temps humain** — le matériel est 30 % du coût,
   l'humain 70 % (installation, cérémonies, tests, formation, revues). Le dire à la
   direction **avant**, pas après.

# ANNEXES

## A. Index des sections

**Partie 1 — HSM (1-48)** : 1 principe, 2 TRNG, 3 tamper resistance, 4 tamper evidence,
5 FIPS 140, 6 Common Criteria, 7 PCI HSM/eIDAS, 8 PKI, 9 TDE, 10 signature, 11 horodatage,
12 DNSSEC, 13 paiement, 14 5G/blockchain/IoT, 15 form factors, 16 appliance réseau,
17 carte PCIe, 18 USB/nano, 19 cloud HSM, 20 Thales Luna Network, 21 Thales Luna PCIe,
22 Luna USB/DPoD, 23 Utimaco Se Gen2, 24 Utimaco u.trust, 25 Atalla, 26 Entrust nShield,
27 Futurex, 28 Marvell LiquidSecurity, 29 YubiHSM 2, 30 comparatif général,
31 performances, 32 certifications, 33 schéma datacenter, 34 schéma PME, 35 VLAN/firewall,
36 dimensionnement méthode, 37 exemples chiffrés, 38 partitions, 39 PKCS#11, 40 autres APIs,
41 sauvegardes, 42 cérémonie, 43 MofN, 44 custodians, 45 HA, 46 batterie/MTBF, 47 prix,
48 TCO.
**Partie 2 — RoT (49-70)** : 49 définition, 50 chaîne de confiance, 51 lien guide Zelef,
52 TPM serveur, 53 PCR, 54 secure boot serveurs, 55 measured boot, 56 attestation protocole,
57 vérificateur, 58 Boot Guard/PSB, 59 BMC talon d'Achille, 60 durcissement BMC,
61 OpenBMC/Redfish/SPDM, 62 firmware signés, 63 contrefaçons, 64 SBOM, 65 réception,
66 reconditionné, 67 cas BMC compromis, 68 tableau RoT, 69 limites TPM, 70 TPM+HSM.
**Partie 3 — FPGA (71-90)** : 71 principe, 72 vocabulaire, 73 flot dev, 74 HLS, 75 Alveo V80,
76 autres Alveo, 77 Agilex 7, 78 SmartNIC N6000, 79 autres acteurs, 80 FPGA vs GPU vs ASIC,
81 réseau, 82 crypto, 83 trading, 84 IA, 85 stockage, 86 conso/refroidissement,
87 grille de décision, 88 coûts, 89 outils, 90 production.
**Partie 4 — Physique (91-104)** : 91 4 cercles, 92 périmètre, 93 contrôle d'accès,
94 cages/racks, 95 vidéo, 96 intrusion, 97 visiteurs, 98 anti-tailgating, 99 personnel,
100 NIST 800-88, 101 SED, 102 déclassement, 103 élec/incendie, 104 budgets.
**Partie 5 — KMS (105-120)** : 105 cycle de vie, 106 hiérarchie, 107 envelope encryption,
108 rotation, 109 durées de vie, 110 rôles, 111 HSM↔KMS, 112 Vault, 113 auto-unseal,
114 EJBCA, 115 sauvegardes chiffrées, 116 BYOK/HYOK, 117 comparatif KMS, 118 BOM PME,
119 erreurs, 120 journaux.
**Partie 6 — Conformité (121-128)** : 121 ISO 27001, 122 contrôles crypto, 123 PCI DSS,
124 RGPD, 125 eIDAS, 126 questionnaires clients, 127 journaux/audit, 128 politiques.
**Partie 7 — Checklists (129-138)** : 129 serveur, 130 BMC, 131 firmware/secure boot,
132 HSM, 133 cérémonie, 134 FPGA, 135 baie, 136 runbook HSM down, 137 runbook clé
compromise, 138 test DR.
**Partie 8 — Énergie (139-146)** : 139 consos, 140 règle d'or onduleur, 141 architecture
élec, 142 refroidissement, 143 dimensionnement, 144 monitoring, 145 séquence groupe,
146 cas baie crypto.
**Partie 9 — À venir (147-157)** : 147 NIST PQC, 148 FIPS 206, 149 CNSA 2.0, 150 EO/M-26-15,
151 TLS hybride, 152 PQC dans HSM, 153 HAWK, 154 TDX/SEV-SNP, 155 DDRop, 156 H100/CCA/CoCo,
157 pas encore là.
**Partie 10 — Approfondissements HSM (158-167)** : 158 AD CS, 159 OpenSSL/nginx,
160 EJBCA, 161 OpenTSA, 162 DNSSEC/BIND, 163 Luna HA, 164 négociation, 165 TCO 3 scénarios,
166 migration PKI, 167 FAQ HSM.
**Partie 11 — Approfondissements RoT (168-175)** : 168 Redfish, 169 Keylime, 170 LUKS+TPM,
171 GRUB, 172 parc BMC, 173 microcodes, 174 cas 50 serveurs, 175 FAQ RoT.
**Partie 12 — Approfondissements FPGA (176-183)** : 176 ROI IPsec, 177 SmartNIC/DPU,
178 bitstream, 179 POC, 180 FPGA+PQC, 181 airflow, 182 perf/watt, 183 FAQ FPGA.
**Partie 13 — Approfondissements KMS/physique (184-191)** : 184 modèle de menaces,
185 Vault durci, 186 rotation auto, 187 3-2-1 clés, 188 câblage, 189 SIEM, 190 exercice
de crise, 191 FAQ KMS.
**Partie 14 — Achat/gouvernance (192-201)** : 192 RFP, 193 grille notation, 194 planning,
195 RACI, 196 formation, 197 veille, 198 PV cérémonie, 199 fiche serveur, 200 registre
firmware, 201 revue annuelle.
**Partie 15 — Cas d'école (202-207)** : 202 ETI, 203 PME, 204 hébergeur, 205 dimensionnement
élec, 206 PRA, 207 leçons.
**Partie 16 — Sauvegardes/réseau/PRA (208-215)** : 208 chiffrement sauvegardes, 209
ransomware, 210 segmentation, 211 bastion, 212 supervision corrélée, 213 certificats à
grande échelle, 214 site secours, 215 classeur d'exploitation.
**Partie 17 — Feuille de route (216-223)** : 216 2026-2027, 217 2028-2029, 218 2030+,
219 maturité, 220 KPIs, 221 budget, 222 argumentaire, 223 lundi matin.
**Partie 18 — Attaques (224-233)** : 224 HSM, 225 rootkits UEFI, 226 BMC, 227 réseau,
228 physique, 229 supply chain, 230 HNDL, 231 sauvegardes, 232 ingénierie sociale,
233 synthèse.
**Partie 19 — Pour aller plus loin (234-240)** : 234 labo, 235 outils open source,
236 livres et références, 237 formations et certifications, 238 communautés, 239 maquette
POC, 240 mot de la fin.

## B. Table des tableaux et schémas

Tableaux : FIPS 140 (5), form factors HSM (15), cloud HSM prix (19), YubiHSM 2 (29),
comparatif HSM (30), performances tps (31), certifications (32), custodians (44),
prix HSM (47), TCO 5 ans (48), TPM serveur (52), mécanismes RoT (68), FPGA vs GPU vs
ASIC (80), conso FPGA (86), grille décision FPGA (87), équipements physiques (104),
rotation clés (108), matrice rôles (110), KMS (117), journaux (127), consos énergie (139),
ROI IPsec (176), SmartNIC/DPU/FPGA (177), perf/watt (182), grille notation (193),
registre firmware (200).
Schémas ASCII : placement HSM datacenter (33), PME (34), chaîne de confiance (50),
4 cercles physiques (91), architecture électrique (141), SIEM crypto (189), cycle de
vie des clés (105), rotation DEK (186).

## C. Sources et références (vérifiées le 27/09/2026)

- Thales Luna Network/PCIe HSM — product briefs (cpl.thalesgroup.com, via revendeurs ;
  chiffres : 84 W typ./110 W max, MTBF 171 308 h ; PCIe : 14 W typ./18 W max, MTBF
  997 508 h ; perfs RSA/ECC/AES par modèle ; Luna PQC Functionality Module).
- Utimaco — communiqué septembre 2026 (u.trust GP HSM : 31 conteneurs, Se100→Se40k,
  FIPS 140-2 L3 / L4 en cours, PCI PTS HSM V3, Common Criteria CCN Espagne, Quantum
  Protect) ; datasheet SecurityServer Se Gen2 (PCIe/LAN, n-of-m, SHA-3).
- Marvell + Microsoft + Utimaco — Azure Payment HSM v2, public preview 17/09/2026
  (Western US, Western Europe), Marvell LiquidSecurity + Atalla.
- pqca/wg-readiness-tracking (GitHub, 09/2026) — PQC HSM : Utimaco Ready (ML-KEM,
  ML-DSA, XMSS, LMS, HSS), YubiHSM 2 v2.4+ pilote uniquement.
- Yubico YubiHSM 2 — specs (nano USB-A 12×13×3,1 mm, 1 g, 20/30 mA, RSA/ECC/AES/Ed25519,
  PKCS#11/CNG) ; prix constatés 2026 : ~650 € standard, ~949 € FIPS (revendeurs EU).
- AMD Alveo V80 — product brief (Versal XCV80, 2,6 M LUT, 10 848 DSP, 32 Go HBM2e
  820 Go/s, 4× QSFP56, PCIe Gen4 x16/Gen5 x8, 190 W TDP, SKU A-V80-P64G-PQ-G) ;
  prix MSRP constaté 9 495 $.
- Intel/Altera Agilex 7 — device overview 2025 (PCIe 5.0, CXL, 116 Gb/s, chiplet EMIB) ;
  SmartNIC N6000/N6001-PL ; FPGA AI Suite + OpenVINO (altera-fpga.github.io).
- NIST — FIPS 203/204/205 finalisés 13/08/2024 ; FIPS 206 (HQC) en cours (attendu
  2026-2027, à vérifier) ; IR 8547 (retrait algos classiques 2030-2035).
- NSA CNSA 2.0 v2.1 (12/2024) — jalons 01/2027, 12/2030, 12/2031 ; CNSSP 15.
- EO 14412 (22/06/2026), OMB M-26-15 (5 phases 2026-2035), EO 14144 (TLS 1.3 PQC).
- TLS hybride X25519MLKEM768 — supporté Chrome/Firefox/Cloudflare/AWS/Schannel (2026).
- Affaire HAWK — retrait juillet 2026 après découverte d'une faiblesse (Anthropic).
- DDRop — divulgation 14/09/2026 (KU Leuven, ETH Zurich, Durham, Google) : interposeur
  DDR5 < 200 $ contre l'intégrité mémoire TDX/SGX/SEV-SNP.
- Confidential computing — Intel TDX, AMD SEV-SNP, Arm CCA (Armv9), NVIDIA H100 CC
  (VRAM AES-256-GCM, ~2-5 % overhead), Red Hat OpenShift sandboxed containers 1.12
  (04/2026, Tech Preview GPU confidentiel), Trustee/Confidential Containers.
- AWS CloudHSM — ~1,45-1,50 $/h, FIPS 140-2 L3 ; Azure Dedicated HSM — 4,85 $/h
  (page tarifs Azure).
- Non trouvés au 27/09/2026 (détails produit/tarifs) : Entrust nShield, Futurex,
  Atalla AT1000 (détails), prix publics Thales/Utimaco (sur devis uniquement).

## D. Historique du document

- **v1.0 — 27/09/2026** : création. 207 sections, glossaire 40 termes, quiz 10 Q/R,
  25 pièges terrain. Recherche web préalable le 27/09/2026 ; mentions « vérifié le
  27/09/2026 » / « à vérifier » / « non trouvée au 27/09/2026 ».
- Prochaine revue conseillée : **T1 2027** (finalisation FIPS 206, échéance CNSA 2.0
  du 01/01/2027, évolutions PQC des HSM).

# PARTIE 16 — SAUVEGARDES, RÉSEAU CRYPTO ET PRA ÉTENDU

## 208. Chiffrement des sauvegardes : l'architecture complète

Les sauvegardes contiennent tout — elles méritent le même niveau que la production :

```
[Données prod] ──chiffrées (DEK)──▶ [Sauvegarde chiffrée]
                                         │
[DEK] ──enveloppée (KEK du HSM)──▶ [stockée avec la sauvegarde]
                                         │
[KEK] ──dans le HSM──▶ [backup HSM] ──MofN──▶ [coffre]
```

Règles :

- Le logiciel de sauvegarde (Veeam, etc.) chiffre avec une clé dont la **KEK est dans
  le HSM/KMS** — jamais de mot de passe « dans la doc du logiciel ».
- **Air gap** : une copie offline (bande, disque déconnecté, bucket immuable) contre
  le ransomware — le chiffrement seul ne suffit pas si l'attaquant a les clés.
- **Immuabilité** : verrouillage d'objet (object lock) sur la copie distante : même
  avec les credentials, on ne peut pas effacer avant l'échéance.
- **Test** : restauration complète annuelle, incluant le **déchiffrement** (la clé
  existe-t-elle encore ? — voir section 138).
- **Rétention vs clés** : une sauvegarde de 7 ans exige que la clé vive 7 ans —
  coordonner avec la politique de rotation (section 108).

## 209. Ransomware et clés : le scénario à préparer

Un ransomware moderne fait deux choses : **chiffre** vos données et **vole** vos
sauvegardes si elles sont accessibles. La défense crypto :

1. **Clés hors d'atteinte** : le ransomware qui compromet l'OS n'atteint pas les clés
   du HSM (périmètre inviolable) — mais il peut **utiliser** le HSM via les applis
   compromises : d'où la surveillance des opérations anormales (section 120).
2. **Sauvegardes immuables + offline** : la seule vraie parade (section 208).
3. **Ne pas payer** : la clé de déchiffrement de l'attaquant n'est pas garantie, et
   payer finance l'écosystème — position à faire valider par la direction **avant**
   la crise.
4. **Exercice** : simuler un ransomware un vendredi soir — qui décide quoi, avec
   quelles clés, dans quel ordre (voir section 190).

## 210. Segmentation réseau de la zone crypto (détaillée)

Au-delà du VLAN dédié (section 35), l'architecture cible :

```
[Internet] ─▶ [FW périmétrique] ─▶ [DMZ] ─▶ [FW interne] ─▶ [VLAN prod]
                                                              │
[Postes admin] ─▶ [Bastion] ─▶ [FW mgmt] ─▶ [VLAN management]  │
                                              (BMC, Redfish)  │
[VLAN crypto] ◀── [FW crypto] ◀── autorisé depuis : VLAN prod (clients PKCS#11
   (HSM, Vault,   (règles par IP+port,        déclarés uniquement) + bastion
    PKI, TSA)      TLS mutuel, IDS)            (admin)
```

- **Firewall dédié** (physique ou virtuel) pour la zone crypto — pas de simples ACL
  sur le switch.
- **IDS/IPS** en écoute sur le trafic vers le HSM (détection d'anomalies).
- **Aucune route** entre VLAN crypto et Internet/DMZ — même via le firewall.
- **Bastion** : seul point d'administration, MFA obligatoire, sessions enregistrées.
- **NAC** (802.1X) sur les ports du VLAN crypto : un équipement non autorisé branché
  physiquement n'obtient pas d'IP.

## 211. Bastion d'administration : le poste fortifié

Le bastion est la **porte d'entrée unique** vers la zone crypto et le VLAN management :

- OS durci, **pas d'Internet**, pas de mail, pas de navigation.
- Accès par **MFA** (carte à puce / YubiKey + PIN).
- **Enregistrement des sessions** (audit trail) — qui a fait quoi sur le HSM.
- Comptes nominatifs, pas de compte partagé « admin ».
- Mises à jour prioritaires, antivirus/EDR, chiffrement disque.
- **2 bastions** (ou un accès de secours documenté) : le bastion est un SPOF
  assumé — prévoir la procédure si le bastion tombe.

## 212. Supervision avancée : corréler la crypto et l'infra

Aller au-delà des alertes unitaires (section 144) avec des **règles de corrélation** :

| Corrélation | Signification probable | Action |
|---|---|---|
| Échecs auth HSM + connexion BMC inhabituelle | Attaque en cours | Isoler, incident P1 |
| Pic d'opérations HSM ×10 + process inconnu | Ransomware utilisant le HSM | Couper l'accès client, incident P1 |
| Dérive horloge > 1 s + TSA active | Jetons contestables | Resync NTS, vérifier les jetons émis |
| Température baie > 32 °C + charge onduleur > 90 % | Risque coupure thermique | Délestage, intervention |
| Alerte intrusion physique + coupure réseau | Attaque coordonnée | Cellule de crise |

Ces règles se construisent avec le temps — commencer par les 5 ci-dessus, affiner
trimestriellement.

## 213. Gestion des certificats à grande échelle

Au-delà de ~100 certificats, la gestion manuelle ne passe plus :

- **Inventaire automatisé** : scanner le parc (ports 443, PKI, magasins), détecter les
  expirations à J-30/J-7.
- **ACME interne** : EJBCA ou Vault PKI en mode ACME pour le renouvellement
  automatique (90 jours, section 108).
- **Chaînes courtes** : intermédiaire dédiée par usage (serveurs, Wi-Fi, VPN) pour
  limiter l'impact d'une révocation.
- **CT logs** : publier les certificats publics en Certificate Transparency
  (détection d'émission frauduleuse).
- **Urgence** : procédure de **révocation massive** testée (compromission
  d'intermédiaire) — CRL/OCSP à jour en < 1 h.

## 214. PRA étendu : le site de secours crypto

Le site de secours n'est pas une simple copie des serveurs :

- **HSM du site distant** : synchronisé (réplication) ou restauré depuis la sauvegarde
  chiffrée — **testé** (section 138).
- **Clés DNS** : la KSK DNSSEC doit exister sur les deux sites (signer depuis le
  secours en cas de bascule).
- **Bastion de secours** : administrer le site distant sans dépendre du site principal.
- **Exercice** : bascule **réelle** une fois par an (pas juste « on a lu la procédure »).
- **Coûts** : le site secours crypto (HSM + 1 serveur) ≈ 25-30 k€ — à comparer au
  coût d'une indisponibilité totale (section 146).

## 215. Documentation d'exploitation : le classeur crypto

Le « classeur » (numérique, chiffré, sauvegardé) que tout nouvel admin doit pouvoir
utiliser :

1. Schéma réseau de la zone crypto (à jour).
2. Inventaire : HSM (S/N, firmware, partitions), clés (usage, dates, custodians).
3. Procédures : démarrage/arrêt, bascule HA, restauration, rotation.
4. Runbooks incidents (sections 136-137, 190).
5. PV : cérémonies, tests DR, revues.
6. Contacts : support constructeur (n° contrat), custodians, astreinte.
7. Journal des changements (qui a modifié quoi, quand).

**Règle** : si l'admin principal est injoignable, un second doit pouvoir tout faire
avec ce classeur + le coffre. Le tester : « exercice du bus » annuel.

# PARTIE 17 — FEUILLE DE ROUTE 2026-2030 ET SYNTHÈSE DÉCISIONNELLE

## 216. Feuille de route : 2026-2027 (fondations)

| Trimestre | Actions | Budget indicatif |
|---|---|---|
| T4 2026 | Inventaire clés + firmwares ; durcissement BMC ; Secure Boot/TPM sur critiques | Temps interne |
| T1 2027 | RFP HSM, POC, achat ; cahier des charges PQC (exiger ML-KEM/ML-DSA logiciels) | 50-80 k€ |
| T2 2027 | Installation, cérémonie racine, intégration AD CS/EJBCA + Vault auto-unseal | Prestations ~15 k€ |
| T3 2027 | VLAN crypto + bastion + SIEM ; premier test DR ; TLS hybride activé où dispo | Temps interne |
| T4 2027 | Revue annuelle ; exercice de crise ; formation équipe | ~5 k€ |

Jalon externe : **01/01/2027** — CNSA 2.0 pour les nouvelles acquisitions US (section
149) : vos fournisseurs s'alignent, profitez-en pour exiger le support PQC.

## 217. Feuille de route : 2028-2029 (montée en puissance)

- **PQC** : pilotes ML-KEM pour l'échange (VPN, TLS internes), évaluation ML-DSA pour
  les signatures ; suivre FIPS 206 (HQC).
- **Attestation** : déployer Keylime sur le parc critique, brancher à l'orchestrateur.
- **Site secours** : HSM distant opérationnel, bascule testée.
- **FPGA** : si le besoin réseau est avéré, POC SmartNIC/offload (partie 3).
- **Conformité** : viser ISO 27001 si exigée par les clients (12-18 mois de projet).
- **Budget** : ~30-50 k€/an (support, extensions, prestations).

## 218. Feuille de route : 2030+ (post-quantique opérationnel)

- **2030** : dépréciation des algos classiques à 112 bits (NIST IR 8547) — vos
  systèmes doivent être **hybrides ou PQC** avant cette date.
- **2030-2031** : migration des signatures vers ML-DSA (le chantier le plus long :
  PKI, code signing, documents).
- **2031** : CNSA 2.0 exclusif côté US — vérifier la conformité de vos équipements.
- **2035** : interdiction des algos vulnérables (IR 8547) — horizon de fin de
  migration.
- **Principe** : ne jamais faire de « big bang » crypto — des **vagues** par usage,
  avec rollback possible, sur 4-5 ans.

## 219. Matrice de maturité : où en êtes-vous ?

| Niveau | Clés | Serveurs | Physique | Énergie |
|---|---|---|---|---|
| 1 — Initial | Clés en fichiers | BMC par défaut | Porte fermée à clé | 1 onduleur |
| 2 — Géré | KMS logiciel, inventaire | Secure Boot/TPM | Badges, vidéo | Onduleur testé |
| 3 — Défini | **HSM**, cérémonies, MofN | Attestation, Redfish | Cages, visiteurs, 800-88 | A+B secourus, PRA testé |
| 4 — Maîtrisé | PQC/hybride, rotation auto | Keylime, SPDM | Tests d'intrusion | Corrélation SIEM |

Objectif réaliste pour une ETI : **niveau 3 en 18 mois**, niveau 4 en 3 ans. Se
positionner honnêtement avant de budgéter.

## 220. KPIs : piloter la fonction crypto

- **% de clés critiques en HSM** (objectif : 100 %).
- **% du parc avec Secure Boot + TPM actifs** (objectif : 100 % serveurs).
- **Délai moyen de rotation** des DEK (objectif : < politique).
- **RTO réel du test DR** vs objectif (écart < 20 %).
- **% de firmwares à jour** (objectif : > 95 %, critiques < 7 jours).
- **Nombre d'alertes HSM non traitées** (objectif : 0 > 24 h).
- **Temps moyen de détection** d'une anomalie physique (test d'intrusion annuel).

Revue **trimestrielle** avec la direction : 1 page, tendances, écarts, décisions.

## 221. Budget pluriannuel type (ETI, ordre de grandeur)

| Année | Investissement | Fonctionnement | Total |
|---|---|---|---|
| N (fondations) | 80-120 k€ (HSM, travaux, presta) | 20 k€ | ~100-140 k€ |
| N+1 | 20-30 k€ (site secours, extensions) | 25 k€ (support) | ~45-55 k€ |
| N+2 | 10-20 k€ | 25 k€ | ~35-45 k€ |
| N+3 | Renouvellement partiel | 25 k€ | ~35-45 k€ |

Soit **~250 k€ sur 4 ans** pour une ETI — dont ~40 % de temps humain interne valorisé.
À mettre en regard : le coût moyen d'une violation de données (plusieurs millions
selon les études IBM — ordre de grandeur public) et d'une journée d'arrêt de production.

## 222. Argumentaire direction : 5 phrases qui font signer

1. « Nos clés critiques sont aujourd'hui dans des fichiers : quiconque lit un disque
   les possède. »
2. « Un HSM coûte 25 k€ ; une journée d'arrêt de la PKI coûte [X] k€ — faites le ratio. »
3. « 80 % des pannes crypto viennent de l'électrique, pas des hackers : le plan couvre
   les deux. »
4. « Le quantique ne cassera pas nos systèmes en 2027, mais nos clients nous demanderont
   notre plan PQC dès 2027 — ayons-le. »
5. « Sans documents (politiques, PV, tests), le prochain audit client échoue quel que
   soit le matériel acheté. »

Adapter [X] avec vos chiffres (section 146) — un argument chiffré bat un argument
technique devant une direction.

## 223. Que faire lundi matin : les 5 premières actions

1. **Lister** les 10 clés les plus critiques et où elles sont (fichier ? HSM ? tête
   de quelqu'un ?).
2. **Vérifier** 5 BMC pris au hasard : mot de passe par défaut ? firmware à jour ?
   VLAN dédié ?
3. **Contrôler** que les sauvegardes de clés existent **et** qu'on sait les restaurer
   (demander la date du dernier test).
4. **Vérifier** que les équipements crypto sont sur circuits secourus (suivre les
   câbles, pas les plans).
5. **Planifier** la réunion de cadrage (modèle de menaces, section 184) avec la
   direction — 2 h, ordre du jour écrit.

Ces 5 actions ne coûtent rien, prennent une semaine, et révèlent 90 % des problèmes.
Le reste du guide sert à les corriger durablement.

---

> **Fin du guide.** 240 sections, glossaire 40 termes, quiz 10 Q/R, 25 pièges terrain,
> 19 parties + annexes A-F + 5 fiches réflexes. Rédigé le 27/09/2026 — références produits vérifiées par
> recherche web le 27/09/2026, prix et détails non sourcés marqués « à vérifier »,
> références introuvables marquées « non trouvée au 27/09/2026 ». Aucune invention.
> Revue conseillée : T1 2027.

# PARTIE 18 — DICTIONNAIRE DES ATTAQUES ET CONTRE-MESURES

> Pourquoi cette partie : on ne défend bien que ce qu'on comprend. Chaque attaque est
> décrite avec sa contre-mesure dans ce guide. Faits publics établis ; détails
> techniques pointus marqués « à vérifier ».

## 224. Attaques contre les HSM : canaux auxiliaires et glitch

- **Analyse de consommation / électromagnétique** : déduire la clé en observant la
  consommation ou les émissions pendant les opérations crypto. Contre-mesure : les HSM
  certifiés FIPS 140-2 L3/L4 implémentent des contre-mesures (masquage, blindage) —
  d'où l'importance de la certification (section 5), pas d'un « boîtier maison ».
- **Glitch (tension/horloge)** : perturber la puce pour sauter une vérification.
  Contre-mesure : capteurs de tension/fréquence avec zeroization (section 3).
- **Attaque au froid (cold boot adaptée)** : refroidir la SRAM pour prolonger la
  rémanence après coupure. Contre-mesure : capteur de température + effacement.
- **Extraction par le logiciel** : la voie la plus fréquente en pratique — pas le
  boîtier, mais une **API mal configurée** (clé marquée extractible, PIN faible).
  Contre-mesure : clés non extractibles, PIN forts, moindre privilège (sections 39, 44).

## 225. Attaques contre le firmware : rootkits UEFI

Cas publics établis : **LoJax** (2018, premier rootkit UEFI observé in the wild),
**TrickBoot** (2020, module UEFI), **CosmicStrand** (2022), **BlackLotus** (2023,
contournement du Secure Boot via binaire signé révoqué tardivement). Le pattern :

1. L'attaquant obtient un accès admin/OS.
2. Il écrit dans la flash SPI (variable UEFI, Option ROM, ou bootloader).
3. Le malware **survit** à la réinstallation de l'OS et au changement de disque.

Contre-mesures (ce guide) : Secure Boot + measured boot + attestation (55-57), Boot
Guard/PSB (58), mises à jour firmware (62), scellement TPM (53). Et surtout : **détecter**
— un rootkit UEFI ne se voit pas depuis l'OS, seulement par mesure/attestation.

## 226. Attaques contre les BMC : l'historique parle

Les BMC ont connu des vagues de failles critiques (exécution de code à distance,
contournement d'authentification) chez tous les constructeurs — les bulletins iLO/iDRAC/
XCC en témoignent chaque année. Le pattern d'exploitation :

1. Scan du réseau à la recherche des ports BMC (623/UDP IPMI, 443/HTTPS).
2. Connexion avec identifiants par défaut ou exploitation d'une CVE non patchée.
3. Montage d'une image ISO via KVM virtuel → boot sur l'image → contrôle total.
4. **Persistance** : le BMC n'est pas réinstallé avec l'OS.

Contre-mesures : checklist section 60 (VLAN dédié, mots de passe uniques, patch < 30
jours, IPMI 1.5 désactivé). En 2026, un BMC exposé sur Internet est compromis en
**quelques heures** (les botnets scannent en permanence) — ne jamais l'exposer.

## 227. Attaques réseau contre la zone crypto

- **Interception** : si le canal client↔HSM n'est pas en TLS mutuel, un attaquant
  réseau peut observer ou rejouer. Contre-mesure : NTLS/TLS mutuel (section 35).
- **Déni de service** : inonder le HSM de requêtes — il ne « casse » pas, mais les
  applis légitimes sont ralenties. Contre-mesure : firewall avec rate-limiting, HA.
- **Usurpation de client** : se faire passer pour un serveur applicatif autorisé.
  Contre-mesure : authentification mutuelle par certificats (pas seulement par IP).
- **Attaque de l'homme du milieu sur l'administration** : intercepter la session
  d'admin du HSM. Contre-mesure : poste d'admin dédié, sur le VLAN crypto uniquement,
  pas de Wi-Fi.

## 228. Attaques physiques : evil maid, cold boot, DMA

- **Evil maid** : accès physique bref (femme/homme de ménage, hôtel) pour modifier le
  bootloader. Contre-mesure : Secure Boot + scellement TPM (le boot modifié ne
  déverrouille plus le disque), contrôle d'accès physique (partie 4).
- **Cold boot** : refroidir la RAM, la transplanter, lire les clés. Contre-mesure :
  chiffrement mémoire (confidential computing, partie 9), extinction complète plutôt
  que mise en veille pour les machines sensibles.
- **DMA (Thunderbolt/PCIe)** : un périphérique malveillant lit la RAM via DMA.
  Contre-mesure : IOMMU activé (section 171), ports DMA désactivés sur les machines
  sensibles, contrôle physique des ports.
- **Vol de disque** : sans chiffrement, lecture directe. Contre-mesure : LUKS/BitLocker
  + clé scellée au TPM (section 170).

## 229. Attaques supply chain : cas publics et parades

Cas publics établis (patterns) : composants réseau avec firmwares modifiés découverts à
la réception, disques reconditionnés vendus comme neufs avec des firmwares anciens et
vulnérables, interceptions de colis. Parades (sections 63-66) : circuit officiel,
vérification des scellés et numéros de série, SBOM, attestation SPDM, premier boot en
quarantaine, reflash systématique du reconditionné. Le coût de la parade (quelques
heures par serveur) est négligeable face au coût d'un composant compromis en production.

## 230. Harvest now, decrypt later : le dossier complet

L'attaque la plus **stratégique** de la décennie :

1. **Aujourd'hui** : l'adversaire enregistre le trafic chiffré (TLS, VPN, archives) —
   coût du stockage dérisoire.
2. **Demain (2029-2032+, incertain)** : un ordinateur quantique cryptographiquement
   pertinent casse RSA/ECDH (Shor) et déchiffre rétroactivement.
3. **Cible** : les données à **longue durée de confidentialité** (secrets industriels,
   données de santé, communications sensibles, clés à longue vie).

Défense **dès 2026** : TLS hybride X25519MLKEM768 (section 151), inventaire crypto et
priorisation par durée de confidentialité, crypto-agilité des HSM/KMS (section 152),
ne pas attendre FIPS 206. Ce n'est pas de la paranoïa : c'est de la **gestion du risque
à horizon 10 ans**, comme on provisionne une garantie décennale.

## 231. Attaques contre les sauvegardes

- **Vol de bandes/disques** : transport non sécurisé, prestataire peu regardant.
  Contre-mesure : chiffrement (section 208) + transport scellé + traçabilité.
- **Ransomware sur les sauvegardes en ligne** : chiffrement des dépôts accessibles.
  Contre-mesure : immuabilité + air gap (section 209).
- **Sauvegarde non chiffrée « temporaire »** : devient permanente. Contre-mesure :
  chiffrement **par défaut**, pas en option.
- **Restauration avec une clé détruite** : la sauvegarde est illisible. Contre-mesure :
  coordonner rétention et cycle de vie des clés (sections 109, 208), tester.

## 232. Ingénierie sociale sur les custodians

Le MofN (section 43) repose sur des **humains** : ils sont la cible.

- **Prétexting** : « je suis du support Thales, j'ai besoin de votre carte pour une
  mise à jour urgente ». Contre-mesure : procédure écrite — **aucune opération sur
  convocation non planifiée**, vérification par un second canal.
- **Corruption** : acheter un custodian. Contre-mesure : le quorum (il faut corrompre
  M personnes), rotation des custodians, séparation avec l'exploitation.
- **Perte/vol de carte** : carte égarée = part potentiellement compromise.
  Contre-mesure : déclaration immédiate, révocation de la part, régénération du quorum.

La sécurité du MofN est **organisationnelle** avant d'être technique — d'où la
formation (section 196) et l'exercice de crise (section 190).

## 233. Tableau de synthèse : menaces → contre-mesures → sections

| Menace | Contre-mesure principale | Sections |
|---|---|---|
| Vol/copie de clé | HSM, clés non extractibles | 1-4, 39 |
| Perte de clé | Sauvegarde chiffrée + MofN, tests | 41-43, 138 |
| Rootkit UEFI | Secure Boot + measured + attestation | 54-57 |
| BMC compromis | Durcissement, VLAN, patch | 59-60 |
| Interception réseau crypto | VLAN dédié, TLS mutuel | 35, 210 |
| Evil maid / vol disque | TPM seal, chiffrement disque | 53, 170 |
| DMA / cold boot | IOMMU, chiffrement mémoire | 171, 154 |
| Supply chain | Circuit officiel, SPDM, quarantaine | 63-66 |
| Harvest now/decrypt later | TLS hybride, PQC, crypto-agilité | 147-153 |
| Ransomware | Immuabilité, air gap | 208-209 |
| Insider | Séparation des rôles, journaux, MofN | 44, 110, 120 |
| Coupure électrique | Onduleur secouru, séquence de redémarrage | 140-141, 145 |
| Surchauffe | Monitoring, airflow, clim N+1 | 142, 144 |
| Ingénierie sociale | Procédures, formation, quorum | 190, 196, 232 |
| Attaque physique TEE (DDRop) | Défense en profondeur, contrôle d'accès | 91-99, 155 |

> Ce tableau est la **carte de visite** du guide : chaque menace a sa contre-mesure,
> chaque contre-mesure sa section détaillée. L'utiliser en revue de direction pour
> montrer la couverture — et les trous à combler en priorité.

# PARTIE 19 — POUR ALLER PLUS LOIN : LABO, RESSOURCES, COMMUNAUTÉS

## 234. Monter un labo crypto à moins de 2 000 €

Le meilleur moyen d'apprendre : manipuler. Configuration labo recommandée :

| Poste | Choix | Coût indicatif |
|---|---|---|
| HSM | 1× YubiHSM 2 | ~650 € |
| Serveur | Mini-PC x86 d'occasion (TPM 2.0 vérifié) | ~300 € |
| OS | Debian/Ubuntu LTS | 0 € |
| PKI | EJBCA (container) | 0 € |
| KMS | Vault OSS (3 nœuds en VM sur le mini-PC) | 0 € |
| TSA | OpenTSA | 0 € |
| Attestation | Keylime (vérificateur + 1 agent) | 0 € |
| **Total** | | **~1 000 €** |

Parcours : générer une AC racine sur le YubiHSM (cérémonie simulée avec 2 collègues),
émettre des certificats, les révoquer, tester le backup/restauration, casser
volontairement (restaurer sans la sauvegarde — constater la perte), puis tout
documenter. Ce labo vaut mieux qu'une formation théorique.

## 235. Outils open source à maîtriser

- **EJBCA** — PKI complète (émission, OCSP, CMP/SCEP). Le standard open source.
- **HashiCorp Vault** — secrets, PKI, Transit encryption, auto-unseal.
- **Keylime** — attestation TPM à distance (CNCF).
- **OpenTSA / openssl ts** — horodatage RFC 3161.
- **SoftHSM** — HSM logiciel pour les tests (jamais en production pour des clés
  critiques — pas de tamper resistance).
- **TPM2-Tools / tpm2-pkcs11** — manipuler le TPM sous Linux.
- **YubiHSM SDK** (yubihsm-shell, libyubihsm) — C/Python, open source.
- **Wireshark + pkcs11** — comprendre les échanges (en labo uniquement).
- **Caliptra / OpenTitan** — designs open source de RoT (pour les curieux du silicium ;
  maturité variable — à vérifier).

## 236. Livres et références de fond

- **NIST SP 800-57** (parties 1-3) — la référence sur la gestion des clés (gratuit,
  csrc.nist.gov).
- **NIST SP 800-88** — sanitization des médias (gratuit).
- **NIST SP 800-147/147B/193** — protection des firmwares BIOS/plateforme (gratuit).
- **ANSSI** — guides d'hygiène, recommandations sur l'administration sécurisée
  (gratuit, cyber.gouv.fr).
- **ENISA** — rapports sur la transition PQC (gratuit).
- « Bulletproof SSL and TLS » (Ivan Ristić) — pour la partie TLS/PKI opérationnelle.
- Documentation constructeurs : les **product briefs** et **guides d'administration**
  (Thales, Utimaco, Yubico) — gratuits, indispensables avant achat.

## 237. Formations et certifications utiles

- **Formations constructeurs** : Thales/Utimaco (administration HSM, 3-5 jours) —
  inclure dans le devis d'achat.
- **CISSP / CCSP** — culture générale sécurité (pas spécifiques crypto, mais
  reconnues).
- **ISO 27001 Lead Implementer/Auditor** — pour porter la conformité (partie 6).
- **Formations ANSSI / CyberEdu** — selon disponibilité (à vérifier).
- **En interne** : le parcours labo (section 234) + l'exercice de crise annuel
  (section 190) valent autant qu'une certification pour l'opérationnel.

## 238. Communautés et veille

- **Listes de diffusion** : Keylime, EJBCA, Vault, OpenTitan — annonces et failles.
- **pqca (Post-Quantum Cryptography Alliance)** — suivi de la migration PQC, dont le
  tracking HSM cité dans ce guide (github.com/pqca).
- **Confidential Computing Consortium** (Linux Foundation) — TEE, attestation.
- **ANSSI / CERT-FR** — alertes et bulletins (abonnement gratuit).
- **Meetups locaux** (clubs SSI, OWASP chapters) — les retours terrain s'y échangent
  mieux que dans les webinars vendeurs.
- **Rituel d'équipe** : 1 h/mois de veille partagée (section 197) — chacun apporte une
  nouveauté, on décide quoi en faire.

## 239. Maquette POC : le protocole en 30 jours

Pour valider un HSM avant achat (à exiger du vendeur, section 164) :

**Semaine 1 — Prise en main** : installation, initialisation, création d'une partition
de test, génération d'une clé, signature test via PKCS#11.
**Semaine 2 — Intégration** : brancher **votre** application réelle (EJBCA, AD CS,
nginx, Vault) — c'est là que les incompatibilités sortent.
**Semaine 3 — Charge** : test de débit (atteint-on le besoin dimensionné ?), test de
bascule HA (débrancher un nœud), test de sauvegarde/restauration.
**Semaine 4 — Décision** : grille de notation (section 193), TCO affiné, GO/NOGO
documenté.

Règle : un POC qui « se passe bien » sans avoir testé la panne n'a rien prouvé. Casser
volontairement fait partie du protocole.

## 240. Mot de la fin : la sécurité est un sport d'équipe

Ce guide fait 240 sections, des dizaines de tableaux et de checklists — mais la
sécurité d'un datacenter ne tient pas dans un document. Elle tient dans :

- **2-3 personnes compétentes** qui se relaient (jamais une seule),
- des **procédures écrites** qu'on suit même un dimanche à 3 h du matin,
- des **tests réguliers** qui prouvent que ça marche (pas qui le supposent),
- une **direction** qui a compris que la sécurité coûte moins cher que l'incident,
- et l'**humilité** de relire ce guide dans un an — parce que les menaces, les
  produits et les standards auront bougé.

Zelef, chef de service systèmes & énergies : vous avez déjà le réflexe le plus rare —
penser **énergie et sécurité ensemble**. Un HSM sans courant, un FPGA sans airflow,
une PKI sans onduleur : la sécurité cryptographique est un système **électromécanique**
autant que mathématique. C'est cette vision qui fait la différence entre une crypto
« sur le papier » et une crypto qui tient la coupure de courant, la canicule et le
lundi matin.

> *Bon courage — et que vos clés restent à l'abri, vos firmwares signés, et vos
> onduleurs chargés.*

# FICHES RÉFLEXES — À IMPRIMER ET AFFICHER

## Fiche R1 — HSM en panne (à afficher près de la baie)

```
1. NE PAS redémarrer avant d'avoir les logs (SNMP/syslog).
2. Un seul membre ou tout le cluster ? → supervision.
3. Bascule client sur le 2e membre (procédure §136).
4. Si matériel : appeler le support (n° contrat : ............).
5. Restaurer depuis le HSM backup (procédure testée §138).
6. JAMAIS restaurer depuis une sauvegarde non vérifiée.
ASTREINTE : ............   SUPPORT : ............
```

## Fiche R2 — Coupure électrique, séquence de redémarrage

```
1. Stabiliser le groupe (2-5 min), onduleur en mode normal.
2. Réseau : switches, firewall VLAN crypto.
3. HSM : allumage → vérifier partitions + lire les alertes tamper.
4. Vault/KMS : vérifier l'auto-unseal (sinon Shamir + custodians).
5. Applis : bases TDE, PKI, TSA — dans l'ordre des dépendances.
6. Tests : 1 signature + 1 déchiffrement + CRL/OCSP.
7. PV : durées, écarts, anomalies.
ORDRE INVERSÉ À L'ARRÊT : applis → KMS → HSM → réseau.
```

## Fiche R3 — Clé compromise (premières 30 minutes)

```
1. RÉVOQUER les certificats (CRL/OCSP) — pas de panique, de la méthode.
2. DÉSACTIVER la clé dans le HSM/KMS (ne pas la détruire : forensique).
3. PÉRIMÈTRE : quelles données, quelle période ?
4. RÉGÉNÉRER + RE-CHIFFRER les données exposées.
5. NOTIFIER (direction, juriste, clients selon obligations).
6. POST-MORTEM sous 72 h : comment a-t-elle fuité ?
CELLULE DE CRISE : ............   JURISTE : ............
```

## Fiche R4 — Réception d'un serveur (quarantaine)

```
1. Scellés + S/N = bon de livraison. Photo.
2. Boot en VLAN de quarantaine — PAS en production.
3. UEFI à jour, Secure Boot ON, TPM provisionné.
4. BMC : firmware à jour, mot de passe par défaut CHANGÉ (unique).
5. BMC sur VLAN management, services inutiles OFF, certificat remplacé.
6. Image OS vérifiée (hash), LUKS scellé TPM, SSH par clés.
7. Fiche serveur remplie (§199), PV archivé.
```

## Fiche R5 — Les 5 interdits (à afficher dans le local)

```
⛔ Clé privée en clair hors HSM — JAMAIS, même « temporairement ».
⛔ Mot de passe BMC par défaut — changé à la réception, unique par serveur.
⛔ Équipement crypto sur circuit non secouru — JAMAIS.
⛔ Sauvegarde de clés non testée — test annuel obligatoire.
⛔ Photos en salle serveur — INTERDIT.
En cas de doute : relire le runbook AVANT d'agir.
```

## E. Checklist de relecture avant tout achat (à cocher avec le vendeur)

- [ ] La **référence exacte** (modèle + version firmware) figure sur la liste CMVP du
      NIST pour la certification annoncée — capture d'écran archivée.
- [ ] Le devis détaille : matériel, **support annuel** (taux %, durée, SLA), prestations
      (installation, cérémonie, formation), délais.
- [ ] Le support PQC **logiciel** (ML-KEM/ML-DSA) est confirmé par écrit — pas de
      promesse verbale.
- [ ] Un **POC de 30 jours** est prévu au contrat (protocole §239).
- [ ] La **fin de commercialisation** du modèle est connue (> 3 ans idéalement).
- [ ] Les **consommations** (W) et la plage de température sont compatibles avec vos
      baies et votre clim.
- [ ] Le **circuit de commande** est officiel (distributeur agréé) — facture et
      numéros de série traçables.
- [ ] La **reprise de l'ancien matériel** (si renouvellement) inclut la zeroization
      avec PV.
- [ ] Le **budget temps interne** est chiffré et validé (installation, formation,
      tests, revues) — voir §207 point 6.
- [ ] La **direction** a signé le dossier (besoin, budget, risques résiduels).

> *Cocher ces 10 cases prend une heure et évite 90 % des déconvenues d'achat. Bon courage.*

## F. Journal de maintenance du guide (à remplir par l'équipe)

| Date | Révision | Objet | Par |
|---|---|---|---|
| 27/09/2026 | v1.0 | Création (240 sections, 19 parties, annexes A-F) | Leo |
| _à compléter_ | v1.1 | Revue T1 2027 : FIPS 206, CNSA 2.0 (01/01/2027), évolutions PQC HSM | _équipe_ |
| _à compléter_ | v1.2 | Revue T3 2027 : retour du premier test DR réel, leçons apprises | _équipe_ |
| _à compléter_ | v2.0 | Revue 2028 : état de la migration hybride/PQC, nouveaux produits | _équipe_ |

> *Un guide non maintenu devient faux en 18 mois. Ce journal est le rappel.*
