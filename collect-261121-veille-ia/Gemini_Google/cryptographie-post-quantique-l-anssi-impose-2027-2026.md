---
id: collect-261121-veille-ia/gemini-google/cryptographie-post-quantique-l-anssi-impose-2027-2026
title: "Cryptographie post-quantique : l'ANSSI impose 2027 [2026]"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-06-16"]
keywords: ["post-quantique", "anssi", "cryptographie", "lagence", "securite", "quantum", "dollars", "lanssi"]
source: https://tech-insider.org/fr/cryptographie-post-quantique-anssi-2027/
source_anchor: ""
source_lines: [1, 178]
section: "Gemini_Google"
---
L’Agence nationale de la sécurité des systèmes d’information (ANSSI) a franchi un cap que peu de régulateurs osaient encore : elle ne certifiera plus les produits de sécurité dépourvus de **cryptographie post-quantique**. L’annonce, faite à la mi-juin 2026 lors de la conférence France Quantum 2026 à Station F (Paris), transforme une recommandation prudente en obligation de marché. À partir de 2027, un produit qui ne résiste pas à un futur **ordinateur quantique** perdra son sésame le plus précieux : la qualification ANSSI, indispensable pour vendre à l’État et aux opérateurs d’infrastructures critiques.

Derrière le calendrier – arrêt des certifications en 2027, achats exclusivement « quantum-safe » d’ici 2030 – se joue une bascule industrielle majeure que plusieurs cabinets d’analystes commencent à chiffrer, avec des fourchettes qui varient selon la méthodologie mais convergent toutes sur une croissance à deux chiffres. Selon Juniper Research, dont le rapport de janvier 2026 a été révisé à la hausse, les investissements mondiaux dans la cryptographie post-quantique devraient désormais atteindre 13,3 milliards de dollars sur la seule année 2026 ; MarketsandMarkets l’évaluait pour sa part à 0,42 milliard de dollars en octobre 2025, avec un taux de croissance annuel de 46,2 % projeté jusqu’en 2030, tandis que Kaiso Research le situait à 470,2 millions de dollars en juin 2025, en route vers 26,15 milliards de dollars d’ici 2035. Les éditeurs français et européens de VPN, de HSM, de cartes à puce, de messageries chiffrées ou de solutions cloud souveraines doivent désormais réécrire leur cryptographie avant que le tampon de l’agence ne devienne inaccessible. Cet article décortique la décision, ses échéances, les algorithmes concernés et l’onde de choc qu’elle provoque déjà sur le marché européen de la **certification ANSSI**.

## Ce que l’ANSSI a réellement annoncé à France Quantum 2026

Le 16 juin 2026, sur la scène de la conférence France Quantum 2026, Samih Souissi, chef de cabinet de l’ANSSI, a confirmé une orientation que l’agence préparait depuis des années : à compter de 2027, l’ANSSI cessera progressivement de délivrer ses certifications et qualifications aux produits de sécurité qui n’intègrent pas de **chiffrement post-quantique**. Le message aux entreprises est sans ambiguïté : d’ici 2030, elles ne devraient plus acheter que des solutions résistantes au calcul quantique.

La portée est stratégique. La qualification ANSSI – la déclinaison française la plus exigeante de la certification – conditionne l’accès au marché des administrations et des opérateurs critiques. Perdre son éligibilité, c’est perdre l’accès à l’un des plus grands marchés technologiques publics d’Europe. Selon les précisions relayées par la presse spécialisée, l’agence recommande fortement des mécanismes *hybrides* (algorithme classique + algorithme post-quantique) pour toute solution censée protéger des données au-delà de 2030, cette exigence s’étendant aussi bien à l’échange de clés qu’aux signatures numériques. Le média français IT Social et le site spécialisé The Quantum Insider ont tous deux confirmé l’échéance de 2027.

Fait notable, l’ANSSI ne part pas d’une page blanche. L’agence avait publié dès 2022 un premier avis sur la transition vers la **cryptographie post-quantique**, suivi fin 2023 d’une position plus ferme annonçant qu’elle cesserait de délivrer certains labels de sécurité longue durée sans PQC. L’annonce de juin 2026 ne fait que fixer une date à un engagement déjà pris.

## Pourquoi 2027 ? La menace « récolter maintenant, déchiffrer plus tard »

Aucun **ordinateur quantique** capable de casser le chiffrement RSA ou les courbes elliptiques n’existe aujourd’hui. Pourquoi, alors, agir dès 2027 ? La réponse tient en un acronyme : HNDL, pour *Harvest Now, Decrypt Later* – « récolter maintenant, déchiffrer plus tard ». Des acteurs étatiques collectent dès aujourd’hui des flux chiffrés (communications diplomatiques, dossiers médicaux, secrets industriels) dans l’espoir de les déchiffrer le jour où une machine quantique suffisamment puissante – le fameux « Q-Day » – verra le jour.

Le raisonnement s’appuie sur l’inégalité formulée par le cryptographe Michele Mosca : si la durée pendant laquelle une donnée doit rester secrète (X), additionnée au temps nécessaire pour migrer son infrastructure (Y), dépasse le délai avant l’arrivée d’un ordinateur quantique cryptographiquement pertinent (Z), alors il est déjà trop tard. Pour des données de santé ou de défense qui doivent rester confidentielles trente à cinquante ans, la fenêtre de risque est *déjà* ouverte. L’algorithme de Shor, connu depuis 1994, garantit qu’une machine quantique mature réduirait à néant la sécurité de RSA et de l’ECC. La cryptographie résistante au quantique n’est donc pas une assurance contre un risque hypothétique de 2040 : c’est une réponse à une captation qui a commencé. Les calendriers de migration publiés en mai 2026 traduisent déjà cette urgence en obligation concrète : RSA-2048 doit être abandonné pour tout nouveau système d’ici 2030, avec une transition complète visée pour 2035.

L’ANSSI n’est pas isolée dans ce raisonnement. Le calendrier de 2027 converge, presque à la date près, avec le « procurement gate » de la NSA américaine (CNSA 2.0), qui impose que toute nouvelle acquisition destinée aux systèmes de sécurité nationale prenne en charge les algorithmes post-quantiques à compter du 1er janvier 2027. Deux des autorités de certification les plus exigeantes au monde ont convergé indépendamment sur la même année.

## Le calendrier de la transition post-quantique

La décision française s’inscrit dans une chronologie mondiale qui s’accélère depuis la standardisation des premiers algorithmes par le NIST – la consultation publique sur le rapport NIST IR 8547, consacré à la transition post-quantique, s’est d’ailleurs close en janvier 2025 après environ deux mois de retours de la communauté technique. Le tableau ci-dessous retrace les jalons qui mènent de l’appel à candidatures de 2016 à l’objectif européen de transition complète en 2035.

| Date | Jalon | Acteur | 
|---|---|---|
| 2016 | Lancement de l’appel à candidatures pour standardiser la PQC | NIST (États-Unis) | 
| 2022 | Premier avis sur la transition post-quantique | ANSSI | 
| Avril 2024 | Recommandation pour une feuille de route coordonnée | Commission européenne | 
| 13 août 2024 | Finalisation des standards FIPS 203, 204 et 205 | NIST | 
| Mai 2025 | « Agreed Cryptographic Mechanisms v2 » (usage hybride imposé) | ECCG (UE) | 
| Juin 2025 | Premier livrable de la feuille de route PQC | Groupe de coopération NIS | 
| 16 juin 2026 | Annonce de l’arrêt des certifications sans PQC | ANSSI | 
| 1er janv. 2027 | Obligation CNSA 2.0 pour les nouveaux achats + début de l’arrêt ANSSI | NSA / ANSSI | 
| 31 déc. 2030 | Systèmes à haut risque migrés ; achats « quantum-safe » uniquement | UE / ANSSI | 
| 31 déc. 2035 | Transition complète, autant que possible | UE | 

Ce séquençage révèle une logique : la standardisation d’abord (2024), les feuilles de route ensuite (2025-2026), puis les obligations contraignantes (2027-2030). L’ANSSI se positionne à la charnière, au moment où la théorie devient contrainte réglementaire.

## Qu’est-ce que la cryptographie post-quantique ?

La **cryptographie post-quantique** désigne des algorithmes exécutés sur des ordinateurs classiques mais conçus pour résister à un attaquant équipé d’une machine quantique. Contrairement à la cryptographie quantique (QKD), qui exige du matériel photonique dédié, la PQC est un remplacement logiciel : de nouveaux algorithmes mathématiques, fondés notamment sur les réseaux euclidiens (*lattices*) ou les fonctions de hachage, viennent se substituer à RSA et à l’ECC.

En août 2024, le NIST a finalisé les trois premiers standards, socle de toute la transition mondiale. Publié en mars 2025, le rapport NIST IR 8545 documente précisément le statut de la « quatrième phase » (Fourth Round) du concours de standardisation : il y confirme la sélection de HQC comme mécanisme d’encapsulation de clés de secours, fondé sur une famille mathématique différente pour ne pas mettre tous les œufs dans le même panier, et dresse la liste des cinq algorithmes désormais considérés comme prioritaires : ML-KEM, ML-DSA, SLH-DSA, FN-DSA et HQC. La dynamique ne s’est pas arrêtée là : en mai 2026, le rapport NIST IR 8610 a fait état de quatre schémas de signature supplémentaires ayant franchi la deuxième phase d’évaluation, preuve que le chantier de standardisation reste actif au-delà des quatre familles déjà figées. Le tableau suivant récapitule les briques standardisées et ce qu’elles remplacent.

| Standard | Nom (ex-candidat) | Fonction | Remplace | 
|---|---|---|---|
| FIPS 203 | ML-KEM (ex-CRYSTALS-Kyber) | Établissement de clés (KEM) | RSA, ECDH | 
| FIPS 204 | ML-DSA (ex-CRYSTALS-Dilithium) | Signature numérique | RSA, ECDSA | 
| FIPS 205 | SLH-DSA (ex-SPHINCS+) | Signature fondée sur le hachage | RSA, ECDSA | 
| FIPS 206 (en préparation) | FN-DSA (ex-Falcon) | Signature compacte | RSA, ECDSA | 
| Sélection 2025 | HQC | KEM de secours (code correcteur) | Complément de ML-KEM | 

Ces algorithmes sont désormais accessibles aux développeurs, et les adoptions concrètes se multiplient bien au-delà du seul monde de la sécurité. OpenSSL 3.5, publié en avril 2025, intègre nativement ML-KEM, ML-DSA et SLH-DSA, tandis que des navigateurs et CDN majeurs déploient déjà l’échange de clés hybride X25519MLKEM768 en TLS 1.3 ; Oracle a rejoint le mouvement en intégrant ce même échange de clés hybride post-quantique en TLS 1.3 à Java 27, disponible depuis septembre 2026. Le phénomène déborde même sur la blockchain : Algorand a fait passer ses comptes post-quantiques natifs en production sur son réseau principal dès août 2026 avec la version 5.0.0, sa feuille de route promettant en outre la prise en charge de la signature Falcon-1024 dès la publication du troisième trimestre 2026. Le NIST continue d’ailleurs d’affiner le mode d’emploi : le projet de norme SP 800-133 Rev. 3, publié en avril 2026, met à jour les règles de génération de clés pour ces quatre algorithmes FIPS post-quantiques, signe que la spécification technique elle-même est encore en train de se stabiliser. Vous pouvez le vérifier en quelques commandes.

```
# OpenSSL 3.5+ intègre nativement ML-KEM, ML-DSA et SLH-DSA
openssl list -kem-algorithms | grep -i mlkem
# Négocier un échange de clés hybride (X25519 + ML-KEM-768) en TLS 1.3
openssl s_client -connect exemple.fr:443 -groups X25519MLKEM768
# Générer une paire de clés de signature post-quantique (ML-DSA-65)
openssl genpkey -algorithm ML-DSA-65 -out cle_pq.pem
```
### Le pari de la cryptographie hybride

L’ANSSI ne demande pas de jeter la cryptographie classique : elle exige de la *combiner* avec la PQC. Cette approche hybride protège contre deux risques opposés. D’un côté, une faille encore inconnue dans un algorithme post-quantique récent ; de l’autre, l’arrivée de l’ordinateur quantique. En chaînant X25519 et ML-KEM, un attaquant devrait casser les deux mécanismes pour réussir. Le document « Agreed Cryptographic Mechanisms v2 » de l’ECCG (mai 2025) va jusqu’à préciser que les mécanismes fondés sur les réseaux LWE et MLWE ne devraient pas être utilisés seuls. L’agence étend cette prudence aux signatures, jugées plus récentes et moins éprouvées que l’établissement de clés. Le NIST a d’ailleurs illustré cette même prudence en avril 2026 : le projet de norme SP 800-230 a introduit de nouveaux jeux de paramètres SLH-DSA taillés pour des cas de signature à usage limité, une façon de donner plus de marge de manœuvre aux implémenteurs les plus exposés.

## La qualification ANSSI, sésame du marché public français

Pour comprendre la brutalité potentielle de la mesure, il faut saisir ce qu’est la **certification ANSSI**. L’agence délivre plusieurs niveaux d’assurance : la Certification de Sécurité de Premier Niveau (CSPN), les Critères Communs (CC), et surtout la qualification (élémentaire, standard, renforcée), qui est une recommandation officielle d’usage par l’État. Sans qualification, un HSM, une passerelle VPN ou une solution de messagerie chiffrée est de facto exclu des appels d’offres des ministères et des opérateurs d’importance vitale (OIV).

Or un cycle de qualification dure généralement de 12 à 18 mois. Un produit qui entre en évaluation à la mi-2026 dispose donc d’une fenêtre serrée pour décrocher son label avant que la porte ne se referme en 2027. Les éditeurs qui n’ont pas encore intégré ces algorithmes post-quantiques à leur feuille de route se retrouvent, mécaniquement, en retard d’un cycle. Cette contrainte temporelle transforme l’annonce de juin 2026 en signal d’alarme : ce n’est pas 2030 qu’il faut regarder, mais l’année qui vient.

La contrainte s’articule par ailleurs avec la directive NIS2, dont la transposition française élargit considérablement le périmètre des entités « essentielles » et « importantes » tenues à un haut niveau de sécurité. Pour ces milliers d’organisations, la conformité passera de plus en plus par des briques technologiques elles-mêmes qualifiées – et donc, demain, post-quantiques.

## Une convergence internationale : NSA, UE, NCSC

La France n’invente pas un calendrier isolé. Les grandes autorités cyber occidentales alignent leurs échéances autour d’un même horizon 2030-2035, avec des jalons intermédiaires en 2027-2028. Le tableau ci-dessous compare les feuilles de route des principaux régulateurs.

| Autorité | Jalon intermédiaire | Cible finale | Portée | 
|---|---|---|---|
| ANSSI (France) | Arrêt des certifications sans PQC dès 2027 | Achats « quantum-safe » d’ici 2030 | Produits qualifiés / État & OIV | 
| NSA (États-Unis, CNSA 2.0) | Prise en charge exigée au 1er janv. 2027 | Usage exclusif d’ici 2030-2033 | Systèmes de sécurité nationale | 
| Union européenne | Feuilles de route nationales fin 2026 | Haut risque en 2030, complet en 2035 | Infrastructures critiques | 
| NCSC (Royaume-Uni) | Inventaire et priorisation d’ici 2028 | Migration complète d’ici 2035 | Secteurs critiques & grandes entreprises | 

Cette convergence n’est pas fortuite, et les hyperscalers l’ont déjà anticipée : Google Cloud a publié en août 2026 une feuille de route confirmant que ses points de terminaison API proposent désormais un échange de clés quantum-safe, avec une compatibilité PQC complète visée pour 2029. Elle crée un effet de cliquet : dès lors que la France, les États-Unis et les principaux fournisseurs cloud exigent ou proposent la PQC à partir de 2027, aucun grand éditeur international ne peut se permettre de proposer deux gammes de produits. La cryptographie résistante au quantique devient, de fait, le nouveau standard mondial par défaut.

## L’Europe et sa feuille de route coordonnée

L’annonce française prolonge un mouvement continental. En avril 2024, la Commission européenne a publié une recommandation pour une feuille de route coordonnée de transition vers la PQC. Le Groupe de coopération NIS en a livré la première déclinaison opérationnelle en juin 2025, cosignée par l’ANSSI et disponible sur le portail de l’agence.

Cette feuille de route fixe trois échéances : fin 2026 pour les premières stratégies nationales et la sensibilisation ; fin 2030 pour la migration des cas d’usage à haut risque (eau, énergie, santé, finance, transports) ; fin 2035 pour une transition complète, dans la mesure du possible. Le message de l’ANSSI aligne donc la contrainte de certification française sur la cible européenne de 2030 pour les systèmes les plus sensibles.

Le défi reste immense. Une évaluation de l’ENISA portant sur plus de 1 350 organisations dans les 27 États membres a conclu que la plupart des acteurs européens demeurent largement impréparés. Plus frappant encore : une étude commandée par l’ANSSI en mai 2025 a montré qu’aucune des organisations sondées ne disposait alors d’un plan de transition post-quantique formalisé. Entre l’ambition réglementaire et la réalité opérationnelle, le fossé est considérable – et c’est précisément pour le combler que l’agence a choisi de brandir l’arme de la certification. Les ressources de l’ENISA sur la cryptographie offrent un point de départ aux RSSI qui cherchent à structurer leur plan.

## Impact sur le marché : éditeurs, intégrateurs, secteurs critiques

L’onde de choc se propage à toute la chaîne de valeur, et les grands équipementiers comme les investisseurs l’ont déjà intégrée : Cisco a annoncé en septembre 2026 qu’il déploierait des communications quantum-safe sur la quasi-totalité de son portefeuille de cœur de réseau d’ici décembre 2026, tandis qu’une nouvelle génération d’éditeurs spécialisés émerge, à l’image de Quantum Secure Encryption Corp, qui a lancé dès mars 2026 sa plateforme de migration PQC pour entreprises, QPA v2. Le britannique PQShield, spécialiste pur-player de la cryptographie post-quantique, a porté son financement cumulé à 63 millions de dollars, dont une levée Série B de 37 millions de dollars bouclée en avril 2026 selon Sig.ai – signe que les fonds parient sur une demande européenne durable, portée par des échéances comme celle de l’ANSSI. Pour les éditeurs français de sécurité – fabricants de HSM, de cartes à puce, de VPN souverains, de solutions de signature électronique – l’échéance 2027 impose un chantier de réécriture cryptographique immédiat. Ceux qui ont anticipé, souvent des acteurs historiques rompus aux exigences de l’ANSSI, disposent d’une avance décisive. Les autres risquent l’exclusion pure et simple du marché public, sanction commerciale plus redoutable qu’aucune amende.

La décision s’inscrit dans une stratégie nationale de long terme, mais aussi dans une course mondiale aux financements. La France avait lancé en 2021 un plan quantique doté de 1,8 milliard d’euros, et la souveraineté cryptographique en constitue un pilier ; selon SNS Insider (décembre 2025), plus de 55 pays ont désormais alloué collectivement plus de 4 milliards de dollars à des initiatives post-quantiques, preuve que Paris n’est qu’une capitale parmi beaucoup d’autres à transformer la PQC en priorité budgétaire. En liant certification et post-quantique, l’ANSSI transforme un investissement de recherche en levier industriel : elle crée une demande captive pour les technologies qu’elle a contribué à financer.

### Les secteurs en première ligne

Tous les secteurs ne sont pas logés à la même enseigne. La banque et l’assurance, qui manipulent des données financières à longue durée de vie et des signatures engageantes, figurent en tête des priorités – d’autant que les régulateurs financiers (dans le sillage de DORA) scrutent désormais la résilience opérationnelle. La santé, avec des dossiers patients devant rester confidentiels des décennies, est tout aussi exposée. Viennent ensuite l’énergie, les télécommunications et les transports, colonne vertébrale des infrastructures critiques visées par la feuille de route européenne. Pour ces filières, le chiffrement post-quantique n’est plus une option de veille technologique : c’est une échéance de conformité.

## Le défi de la migration : crypto-agilité et inventaire

Migrer vers la PQC ne se résume pas à changer une bibliothèque. La plupart des organisations ignorent où, exactement, leur chiffrement est utilisé : certificats TLS, tunnels VPN, signatures de code, clés SSH, secrets applicatifs enfouis dans du code hérité. La première étape, unanimement recommandée, est l’inventaire cryptographique – cartographier chaque usage avant de le remplacer. C’est le préalable à la « crypto-agilité », cette capacité à changer d’algorithme sans réécrire toute l’application – un concept que le NIST a lui-même formalisé en juin 2026 dans la mise à jour CSWP 39upd1, consacrée aux bonnes pratiques de crypto-agilité.

Les organisations les plus matures adoptent une approche progressive : d’abord les systèmes exposés à Internet et les données à longue durée de vie, puis les systèmes internes. Le chiffrement hybride sert de pont, permettant de déployer la PQC sans casser la compatibilité avec les systèmes classiques. Concrètement, un audit peut commencer par recenser les algorithmes en usage sur un parc de serveurs.

```
# Recenser les algorithmes de clés publiques d'un parc de certificats
for host in $(cat serveurs.txt); do
  echo "== $host =="
  echo | openssl s_client -connect "$host":443 2>/dev/null \
    | openssl x509 -noout -text \
    | grep -E "Public Key Algorithm|Signature Algorithm"
done
# Repérer les clés RSA/ECDSA à migrer en priorité (longue durée de vie)
grep -rlE "BEGIN (RSA|EC) PRIVATE KEY" /etc/ssl /home 2>/dev/null
```
Ce type d’inventaire, trivial en apparence, révèle souvent des centaines de dépendances cryptographiques oubliées. C’est là que se joue le vrai coût de la transition : non dans les algorithmes eux-mêmes, publics et gratuits, mais dans l’archéologie logicielle nécessaire pour les déployer partout. Une estimation conjointe NSA, NIST et ENISA publiée en juin 2026 chiffre d’ailleurs ce déséquilibre : les dépenses de services liées à la migration post-quantique (audit, intégration, accompagnement) représenteraient de 8 à 12 fois le montant des licences logicielles PQC elles-mêmes.

## Contexte : une France sous forte pression cyber en 2026

L’annonce de l’ANSSI ne surgit pas dans un ciel serein. L’année 2026 s’est ouverte sur une vague d’incidents qui a placé la France en tête des pays européens les plus attaqués, comme nous l’avons documenté dans notre analyse de la France, deuxième pays le plus piraté. La cyberattaque contre l’ANTS, avec ses 11,7 millions de comptes exposés, ou encore l’exploitation de failles critiques comme la vulnérabilité Oracle PeopleSoft (CVSS 9,8), rappellent que la menace « classique » n’a pas attendu l’ordinateur quantique pour frapper.

Ce contexte donne du poids à la décision. En durcissant les exigences de certification, l’ANSSI ne se contente pas d’anticiper une menace future : elle relève le niveau de sécurité de tout l’écosystème face à des attaques qui, elles, sont bien présentes. La **cryptographie post-quantique** devient l’un des chantiers d’une doctrine plus large de durcissement, aux côtés du renseignement sur les menaces et de l’hygiène cyber promue par des outils comme CrowdSec ou l’auto-hébergement de coffres de mots de passe via Vaultwarden.

## Souveraineté : le vrai enjeu derrière la technique

Au-delà des algorithmes, Samih Souissi a inscrit la décision dans un cadre plus large mêlant gouvernance, planification industrielle, réglementation et souveraineté. Ce vocabulaire n’est pas anodin : il révèle que la transition post-quantique est autant une politique industrielle qu’une mesure de sécurité. En fixant le tempo par la certification, l’État français conserve la maîtrise du calendrier plutôt que de le subir de fournisseurs extra-européens.

Cette dimension souveraine explique aussi la préférence pour l’hybridation et la volonté de conserver la possibilité de certifier des algorithmes alternatifs (comme FrodoKEM), au-delà des seuls choix du NIST. L’Europe ne veut pas dépendre d’un unique jeu d’algorithmes validé outre-Atlantique. La **certification ANSSI** devient ainsi un instrument de politique cryptographique : un moyen d’imposer des standards tout en préservant une marge d’autonomie technologique.

## Cinq prédictions pour la transition post-quantique

- **Une ruée sur les cycles de qualification en 2026.** Les éditeurs vont saturer les laboratoires d’évaluation agréés pour décrocher leur label avant la fermeture de 2027, allongeant mécaniquement les délais.
- **La crypto-agilité deviendra un critère d’achat.** Au-delà de « supporte-t-il la PQC ? », les acheteurs publics exigeront la capacité à changer d’algorithme sans remplacer le matériel – un argument commercial différenciant dès 2027.
- **L’hybride s’imposera comme norme de fait.** Comme pour X25519MLKEM768 en TLS, les déploiements combineront systématiquement classique et post-quantique jusqu’à ce que la confiance dans les algorithmes récents soit consolidée.
- **Un effet d’entraînement sur le privé.** Bien que ciblant les produits qualifiés, la mesure poussera banques, assureurs et santé à exiger la PQC de leurs fournisseurs, bien avant toute obligation légale directe.
- **D’autres régulateurs européens suivront.** L’Allemagne (BSI) et d’autres agences nationales devraient durcir leurs propres schémas de certification d’ici 2027-2028, sous l’impulsion de la feuille de route coordonnée.

## FAQ : cryptographie post-quantique et certification ANSSI

### Qu’a exactement annoncé l’ANSSI en juin 2026 ?

L’agence a annoncé qu’à partir de 2027, elle cesserait progressivement de certifier et de qualifier les produits de sécurité qui n’intègrent pas de cryptographie post-quantique, et a appelé les entreprises à n’acheter que des solutions « quantum-safe » d’ici 2030. L’annonce a été faite le 16 juin 2026 lors de la conférence France Quantum 2026 à Paris.

### La cryptographie post-quantique est-elle différente de la cryptographie quantique ?

Oui. La cryptographie post-quantique repose sur des algorithmes classiques (logiciels) résistants aux attaques quantiques, déployables sur les ordinateurs actuels. La cryptographie quantique, comme la distribution quantique de clés (QKD), nécessite du matériel photonique spécialisé. L’obligation de l’ANSSI porte sur la première.

### Quels algorithmes faut-il adopter ?

Les standards NIST finalisés en 2024 servent de référence : ML-KEM (FIPS 203) pour l’établissement de clés, ML-DSA (FIPS 204) et SLH-DSA (FIPS 205) pour les signatures. L’ANSSI recommande de les utiliser en mode hybride, combinés à un algorithme classique éprouvé.

### Un ordinateur quantique peut-il déjà casser mon chiffrement ?

Pas aujourd’hui. Aucune machine quantique capable de casser RSA ou l’ECC n’existe encore. Le risque immédiat vient de la stratégie « récolter maintenant, déchiffrer plus tard » : des données interceptées aujourd’hui pourraient être déchiffrées demain. C’est pourquoi les données à longue durée de vie doivent migrer sans attendre.

### Mon entreprise est-elle concernée si elle ne vend pas à l’État ?

Directement, la mesure vise les produits soumis à la certification ANSSI. Indirectement, elle façonnera l’ensemble du marché : les fournisseurs alignant leurs gammes sur les exigences publiques, la PQC deviendra le standard proposé à tous les clients, y compris privés.

### Par où commencer une migration post-quantique ?

Par l’inventaire cryptographique : cartographier où et comment le chiffrement est utilisé (TLS, VPN, signatures, clés SSH). Ensuite, prioriser les systèmes exposés et les données sensibles à longue durée de vie, puis déployer des mécanismes hybrides pour assurer la compatibilité pendant la transition.

### Quel est le lien avec la feuille de route européenne ?

La décision française s’aligne sur la feuille de route coordonnée de l’UE, qui vise la migration des systèmes à haut risque d’ici fin 2030 et une transition complète d’ici 2035. L’ANSSI utilise la certification comme levier national pour atteindre ces objectifs communs.

### Related Coverage : pour aller plus loin

*Sources et références : NIST – First Post-Quantum Encryption Standards, NIST Post-Quantum Cryptography Project, Commission européenne – Coordinated Implementation Roadmap, ANSSI / Groupe NIS, IT Social, The Quantum Insider. Article publié le 17 juin 2026.*
