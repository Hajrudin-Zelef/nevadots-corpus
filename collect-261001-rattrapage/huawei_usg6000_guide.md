# Guide ULTRA-COMPLET — Huawei USG6000 (Firewall UTM)

> **Public :** Zelef, chef de service systèmes & énergies — ton terrain, concret, direct.
> **Périmètre :** série classique **USG6000** (USG6305 → USG6680), logiciel **V500R005C20 / V600R007** (à vérifier sur la fiche du modèle exact).
> **Sources vérifiées (sept. 2026) :** fiche technique Huawei USG6000 Series NGFW Overview, specs matérielles par modèle (switch-router-olt.com reprenant les datasheets Huawei), guide « Quick Configuration Guide (With New Web UI) » basé sur USG6000E V600R007C00, white paper technique HUAWEI Secospace USG6000 Series.
> **Règle d'or :** toute valeur marquée « à vérifier sur la fiche du modèle exact » doit être confirmée avant dimensionnement ou achat. Les mots de passe des exemples sont fictifs.
> **Il existe déjà** un guide général `huawei_usg_guide.md` (684 lignes) : celui-ci va beaucoup plus loin — configs complètes, dépannage terrain approfondi, UTM avec impact perf honnête, HA, durcissement.

---

## Sommaire rapide

| Bloc | Sections | Contenu |
|------|----------|---------|
| A | 1–10 | Hardware : modèles, ports, LED, alimentation, console, capacités |
| B | 11–18 | Initialisation : premier démarrage, mot de passe, management, upgrade |
| C | 19–28 | Zones de sécurité |
| D | 29–44 | Politiques de sécurité (interzone) |
| E | 45–56 | NAT |
| F | 57–72 | VPN : IPSec, SSL VPN, GRE, L2TP |
| G | 73–88 | UTM : AV, IPS, URL, applicatif, anti-spam + impact perf |
| H | 89–96 | Authentification : locale, AD/LDAP, RADIUS |
| I | 97–106 | Haute disponibilité (VGMP/HRP) |
| J | 107–114 | Logs, rapports, supervision |
| K | 115–124 | Sauvegarde, firmware, licences |
| L | 125–132 | Maintenance préventive |
| M | 133–142 | Durcissement |
| N | 143–160 | Dépannage terrain (18 cas) |
| O | 161–168 | Cas pratiques complets commentés |
| P | 169–176 | Erreurs classiques à ne pas commettre |
| Q | 177–185 | Pense-bête de poche, glossaire, quiz, pour aller plus loin |

**Comment utiliser ce guide :**
- Cherche un sujet avec `Ctrl+F` sur le numéro de section.
- Tous les blocs ` ``` ` marqués `huawei` sont des commandes CLI VRP à coller en vue système (`system-view`).
- Les chemins web correspondent à la « New Web UI » (V600R007) ; sur V500, les menus peuvent différer légèrement.
- ⚠️ = point qui casse en prod si on l'oublie. 🔧 = commande de diagnostic. 📋 = checklist.

---

# BLOC A — HARDWARE : LA SÉRIE USG6000

## 1. La série USG6000 en une page

La série **Huawei USG6000** est la gamme de firewalls **Next-Generation (NGFW/UTM)** historique de Huawei : pare-feu stateful + VPN + UTM (antivirus, IPS, filtrage URL, contrôle applicatif, anti-spam, Anti-DDoS) dans un seul boîtier, piloté par le système **VRP** (le même que les routeurs/switchs Huawei — si tu connais `system-view`, tu es chez toi).

Trois générations coexistent :
- **USG6000 classique** (celle de ce guide) : V500R005C20 et versions antérieures.
- **USG6000E** : successeur logiciel/matériel, V600R006/V600R007, « New Web UI ».
- **USG6000F / HiSecEngine** : génération « AI firewall », la plus récente.

En pratique terrain, 90 % des commandes CLI de ce guide fonctionnent à l'identique sur USG6000 et USG6000E. Quand ça diverge, c'est signalé.

## 2. Les modèles de la série (débits constructeur vérifiés)

Débits firewall constructeur (paquets UDP 1518 octets, valeurs marketing — **divise par 2 à 3 pour estimer le débit UTM réel**, voir section 73) :

| Modèle | Format | Débit firewall | Ports en standard | Usage typique |
|--------|--------|----------------|-------------------|---------------|
| USG6305 / 6305-W | Desktop | 500 Mbit/s | 4 × GE | Très petite agence, Wi-Fi intégré (-W) |
| USG6310S / -W / -WL-OVS | Desktop | 1 Gbit/s | 8 × GE | Petite agence |
| USG6320 | Desktop | 2 Gbit/s | 8 × GE + slot extension | Agence moyenne |
| USG6330 | 1U | 1 Gbit/s | 4 × GE + 2 × combo | Petite PME (rack) |
| USG6350 | 1U | 2 Gbit/s | 4 × GE + 2 × combo | PME |
| USG6360 | 1U | 3 Gbit/s | 4 × GE + 2 × combo | PME |
| USG6370 | 1U | 4 Gbit/s | 8 × GE + 4 × SFP | PME / petite industrie |
| USG6380 | 1U | 6 Gbit/s | 8 × GE + 4 × SFP | PME exigeante |
| USG6390 | 1U | 8 Gbit/s | 8 × GE + 4 × SFP | ETI, petit datacenter |
| USG6620 | 1U | 12 Gbit/s | 8 × GE + 4 × SFP | ETI, campus |
| USG6630 | 1U | 16 Gbit/s | 8 × GE + 4 × SFP | Campus, pré-datacenter |
| USG6650 | 3U | 20 Gbit/s | 2 × 10GE + 8 × GE + 8 × SFP | Datacenter |
| USG6660 | 3U | 25 Gbit/s | 2 × 10GE + 8 × GE + 8 × SFP | Datacenter |
| USG6670 | 3U | 35 Gbit/s | 4 × 10GE + 16 × GE + 8 × SFP | Gros datacenter |
| USG6680 | 3U | 40 Gbit/s | 4 × 10GE + 16 × GE + 8 × SFP | Très gros datacenter |

Notes terrain :
- La gamme couvre **500 Mbit/s → 40 Gbit/s** en identification applicative ; la capacité **IPS + AV monte jusqu'à ~15 Gbit/s** sur les gros modèles (valeur constructeur, conditions de test idéales).
- Les modèles 6650→6680 acceptent des **cartes d'extension WSIC** (8 × GE RJ45, 2 × 10GE SFP+ + 8 × GE, 8 × GE SFP, bypass 4 × GE) et des **cartes XSIC** sur 6660.
- ⚠️ Les débits ci-dessus sont des **débits firewall purs**. Avec UTM activé (IPS+AV+URL), compte **30 à 50 %** de ces valeurs selon le mix de trafic. Dimensionne TOUJOURS sur le débit UTM, pas sur le débit firewall (section 73).

## 3. Ports physiques : ce que tu branches où

Sur un USG6000 typique (exemple USG6680, à adapter au modèle exact) :
- **Port console** : 1 × RJ45 (+ 1 × Mini-USB sur les gros modèles — un seul utilisable à la fois). Réglages : **9600-8-N-1**, câble console RJ45/DB9 ou USB-série.
- **Port de management hors-bande** : 1 × RJ45 (souvent `GE0/0/0` en interface de management dédiée).
- **Ports de service** : GE électriques RJ45 10/100/1000, SFP (fibre 1G), SFP+ (10G) selon modèle.
- **USB 2.0** : 1 à 2 ports — servent à l'**upgrade firmware**, l'import/export de config, les mises à jour de signatures UTM via clé USB.
- **Slots WSIC/XSIC** : cartes d'extension (modèles 3U).
- **Disques durs SAS 2,5"** (300/600/1200 Go, optionnels) : **RAID1** possible avec 2 disques identiques, **hot-swap** — servent au **stockage local des logs et rapports**. Sans disque, les logs partent en syslog ou se perdent au reboot (buffer limité).

## 4. LED et boutons : lecture rapide en baie

| LED | État | Signification |
|-----|------|---------------|
| PWR | Vert fixe | Alimentation OK |
| PWR | Éteint | Pas d'alimentation / module HS |
| SYS | Vert clignotant lent | Démarrage en cours |
| SYS | Vert fixe | Système démarré, OK |
| SYS | Rouge | Alarme système (voir logs) |
| ACT (par port) | Vert clignotant | Trafic sur le port |
| LINK (par port) | Vert fixe | Lien établi |
| HA | Vert | État HA normal (selon modèle) |

🔧 En cas de doute : `display device` et `display alarm` donnent l'état matériel vu par le système (section 144).

## 5. Alimentation : AC, DC, redondance

- **Modèles desktop/1U** : alimentation **interne ou externe** selon modèle, **non redondante** en général — prévois un onduleur (voir ton guide onduleurs) et, pour les sites critiques, un second USG en HA plutôt qu'une alim redondante.
- **Modèles 3U (6650→6680)** : **1+1 redondance**, modules **hot-swap**, versions **AC** (100–240 V) et **DC** (-48 V/–60 V télécom) disponibles.
- USG6680-AC : jusqu'à 700 W par module ; USG6680-DC : 350 W (valeurs à vérifier sur la fiche du modèle exact).
- ⚠️ **Ne mélange jamais** un module AC et un module DC dans le même châssis. En redondance 1+1, les deux modules doivent être **identiques**.
- Sens du flux d'air : **aspiration avant/côté gauche, extraction côté droit** (vu de l'arrière) — ne plaque pas le flanc droit contre un mur en baie.

## 6. Console et premier accès : la méthode qui marche toujours

1. Branche le câble console, ouvre PuTTY/secureCRT : **9600 bauds, 8 bits, pas de parité, 1 stop, pas de contrôle de flux**.
2. Allume le boîtier, attends le prompt `Username:` / `Login authentication`.
3. Identifiants initiaux : **à vérifier sur la fiche du modèle exact et le « HUAWEI Security Products Default Usernames and Passwords »** — historiquement `admin` / `Admin@123` sur beaucoup de modèles, mais les versions récentes (V600R007C20+) **n'ont plus d'admin par défaut** : la première connexion web impose de **créer un compte administrateur**.
4. ⚠️ Le système **force le changement du mot de passe par défaut** à la première connexion — prévois le nouveau mot de passe AVANT d'être devant la baie (politique de complexité : 8+ caractères, majuscules/minuscules/chiffres/caractères spéciaux selon version).

## 7. Capacités : sessions, politiques, VPN (ordres de grandeur)

Valeurs indicatives (varient selon modèle et version — **à vérifier sur la fiche du modèle exact**) :

| Capacité | Petit modèle (63xx) | Moyen (66x0) | Gros (6670/6680) |
|----------|---------------------|--------------|------------------|
| Sessions concurrentes | 500 000 – 1 M | 2 – 4 M | 6 – 10 M+ |
| Nouvelles sessions/s | 30 000 – 60 000 | 150 000 – 300 000 | 500 000+ |
| Politiques de sécurité | 5 000 – 10 000 | 20 000 – 50 000 | 50 000+ |
| Tunnels IPSec | 1 000 – 2 000 | 5 000 – 10 000 | 10 000 – 20 000 |
| Utilisateurs SSL VPN simultanés | 100 – 500 | 1 000 – 2 000 | 2 000 – 4 000+ |
| Firewalls virtuels (vsys) | 4 – 16 | 64 – 128 | 256+ |

🔧 Commandes pour voir TES limites réelles : `display firewall session table verbose` (compteur), `display license` (limites logicielles), `display memory` / `display cpu`.

## 8. Versions logicielles : V500 vs V600, quoi choisir

- **V500R005C20** (et antérieures) : la version historique des USG6000 classiques. Stable, très documentée, CLI complète.
- **V600R006 / V600R007** : versions des USG6000E (et portées sur certains USG6000). Apportent la **New Web UI**, des perfs UTM améliorées, de nouvelles signatures.
- ⚠️ **Avant tout upgrade** : vérifie la **matrice de compatibilité** (le firmware doit correspondre EXACTEMENT au modèle), sauvegarde la config (section 115), et prévois un **rollback** (section 117). Un upgrade qui échoue à 2h du matin sans backup, c'est une nuit blanche.
- Pour connaître ta version : `display version` (CLI) ou System > System Information (web).

## 9. USG6000 vs USG6000E vs USG6000F : faut-il migrer ?

| Critère | USG6000 | USG6000E | USG6000F (HiSecEngine) |
|---------|---------|----------|----------------------|
| Statut | Mature / fin de vente progressive | Génération courante | Dernière génération « AI » |
| Version | V500R005 | V600R006/007 | V600R007+ / R024 |
| Web UI | Classique | Nouvelle | Nouvelle |
| Perfs UTM | Bonnes | Meilleures (moteur optimisé) | Les meilleures (accélération IA) |
| Ton besoin | Parc existant, ça tourne | Renouvellement 2024-2026 | Nouveau projet exigeant |

Conseil terrain : **on ne migre pas un firewall qui tourne bien** juste pour la nouveauté. On migre quand : fin de support, besoin de débit UTM supérieur, ou fonctionnalités manquantes (ex. TLS 1.3 inspection poussée). Et on teste TOUJOURS la nouvelle version en maquette avant.

## 10. Positionnement : où mettre l'USG6000 dans ton architecture

Scénarios classiques :
1. **Passerelle Internet PME/ETI** : entre le routeur FAI et le LAN — NAT + politiques + UTM. Le cas n°1, traité en détail section 161.
2. **Firewall de datacenter** : devant les serveurs (zone DMZ), politiques fines par application.
3. **Concentrateur VPN** : IPSec site-à-site vers les agences + SSL VPN pour les nomades (sections 57-72).
4. **Coupe-feu inter-VLAN** : entre zones de sensibilité différentes (prod/bureautique/invités/industriel).
5. **Paire HA actif/passif** : deux USG6000 en hot standby pour les sites qui ne doivent pas tomber (sections 97-106).

Règle d'architecture : **un firewall = un point de passage obligé**. Si un flux peut contourner l'USG (ex. une box 4G branchée en douce), ta politique ne vaut rien. Vérifie le câblage AVANT la config.

---
---

# BLOC B — INITIALISATION : PREMIER DÉMARRAGE

## 11. Checklist de déballage et d'installation physique

📋 Avant de brancher quoi que ce soit :
- [ ] Carton : boîtier, cordons d'alimentation, câble console, kit de rails/oreilles rack, documentation.
- [ ] Vérifier la référence du modèle sur l'étiquette = celle du bon de commande.
- [ ] Rack : 1U ou 3U libres, rails montés, **dégagement avant/arrière** pour l'air (section 5).
- [ ] Alimentation : prise(s) ondulée(s) disponible(s), tension compatible (100-240 V AC ou -48 V DC).
- [ ] Câblage : prévoir les jarretières (cuivre/fibre selon ports), étiqueter CHAQUE câble aux deux bouts.
- [ ] Console : PC portable + câble console + PuTTY configuré en 9600-8-N-1.
- [ ] Noter le **numéro de série** (étiquette + `display esn`) — indispensable pour les licences et le support.
- [ ] Photo de la baie avant/après : ça sauve des heures en dépannage à distance.

## 12. Premier démarrage : séquence et timings

1. Branche l'alimentation, interrupteur ON.
2. Le boot prend **2 à 5 minutes** (plus long avec disques durs / gros modèles).
3. Sur la console, tu vois le décompte mémoire, le chargement du VRP, puis le prompt de login.
4. ⚠️ **Ne coupe JAMAIS l'alimentation pendant le boot ou un upgrade** — corruption du flash possible.
5. Si le boot boucle ou échoue : vérifie l'alimentation, puis boot sur le firmware de secours (section 117).

## 13. Mot de passe initial et création du compte admin

```huawei
# Connexion console, puis :
system-view
# Changer le mot de passe admin existant :
[USG] aaa
[USG-aaa] local-user admin password
        # Saisir le nouveau mot de passe (fictif ici) deux fois
[USG-aaa] local-user admin service-type web telnet ssh terminal
[USG-aaa] local-user admin privilege level 15
[USG-aaa] quit
# Créer un compte nominatif (bonne pratique, voir section 133) :
[USG-aaa] local-user chef-reseau password
[USG-aaa] local-user chef-reseau service-type ssh web
[USG-aaa] local-user chef-reseau privilege level 15
[USG-aaa] quit
save
```

⚠️ Sur V600R007C20+, la **première connexion web** impose la création d'un administrateur (pas de compte par défaut). Le compte créé a le rôle d'administrateur système mais **ne peut pas** être l'administrateur de système virtuel `manager-user@@vsys-name`.

## 14. Configuration de l'interface de management

```huawei
system-view
# Interface de management dédiée (exemple GE0/0/0, adresse fictive) :
[USG] interface GigabitEthernet 0/0/0
[USG-GigabitEthernet0/0/0] ip address 192.168.0.1 24
[USG-GigabitEthernet0/0/0] service-manage enable
[USG-GigabitEthernet0/0/0] service-manage https permit
[USG-GigabitEthernet0/0/0] service-manage ssh permit
[USG-GigabitEthernet0/0/0] service-manage ping permit
[USG-GigabitEthernet0/0/0] quit
# Mettre l'interface de management dans une zone dédiée ou trust :
[USG] firewall zone name MGMT
[USG-zone-mgmt] set priority 90
[USG-zone-mgmt] add interface GigabitEthernet 0/0/0
[USG-zone-mgmt] quit
save
```

Bonnes pratiques :
- L'interface de management a par défaut **192.168.0.1/24** sur beaucoup de modèles (à vérifier) — change-la ou isole-la.
- **HTTPS uniquement** pour le web (désactive HTTP, section 133).
- Ne mets JAMAIS l'interface de management dans la zone **untrust**.
- 📋 Web : System > Network > Interface, puis activer les services de management par interface.

## 15. Mise à l'heure : NTP (obligatoire avant les logs)

Sans heure correcte, tes logs sont inexploitables et les certificats VPN échouent.

```huawei
system-view
[USG] ntp-service unicast-server 192.0.2.10   # IP fictive : ton serveur NTP interne
[USG] clock timezone Paris add 01:00:00
[USG] clock daylight-saving-time Europe repeating 02:00 2026-03-29 03:00 2026-10-25 02:00
# Vérification :
[USG] display clock
[USG] display ntp-service status
save
```

🔧 Si le NTP ne synchronise pas : vérifie que la **politique local→untrust autorise UDP 123** (la zone local, c'est le firewall lui-même — section 19) et que le NAT ne casse pas la source.

## 16. Upgrade initiale du firmware : procédure sans stress

📋 Procédure recommandée :
1. `display version` → noter la version actuelle.
2. Télécharger le firmware **exact** du modèle sur support.huawei.com (compte requis).
3. Vérifier le **checksum** (MD5/SHA) du fichier téléchargé.
4. Sauvegarder la config : `save` puis exporter (`tftp`/`ftp`/USB, section 115).
5. Transférer le firmware : via web (System > Upgrade), ou CLI :
```huawei
# Exemple via FTP (adresses fictives) :
[USG] ftp 192.0.2.50
# login, puis :
get usg6000-firmware.cc
quit
[USG] startup system-software usg6000-firmware.cc
[USG] display startup          # vérifier que le nouveau firmware est bien "next startup"
[USG] reboot
```
6. Après reboot : `display version`, tester le trafic, vérifier les licences (`display license`).
7. ⚠️ Garde l'**ancien firmware en secours** : `startup system-software` ne supprime pas l'ancien — en cas de problème, redémarre sur l'ancienne version (section 117).

## 17. Activation des licences (de base)

```huawei
[USG] display license          # voir les licences actives
[USG] display esn              # numéro de série pour le portail licence
```

- Les licences UTM (AV, IPS, URL, anti-spam) s'activent avec un fichier `.dat` via web (System > License) ou CLI.
- Sans licence UTM valide, les profils UTM **ne protègent pas** (signatures non mises à jour) — le firewall continue de filtrer, mais sans inspection de contenu.
- 📋 Note la **date d'expiration** de chaque licence dans ton planning de maintenance (section 122).

## 18. Sauvegarde de la config « sortie de carton »

Avant TOUTE config métier, sauvegarde l'état initial :
```huawei
[USG] save                     # sauvegarde dans la mémoire flash
[USG] display current-configuration  # pour export vers fichier
```
Exporte aussi vers un serveur TFTP/FTP ou une clé USB (section 115). Nomme le fichier `USG6000_<site>_initiale_<date>.cfg`. Ce fichier, c'est ton **point de retour** si la config métier part en vrille.

---
---

# BLOC C — ZONES DE SÉCURITÉ

## 19. Le concept : pourquoi des zones et pas juste des interfaces

Sur un USG, **on ne filtre pas entre interfaces, on filtre entre ZONES**. Une zone = un ensemble d'interfaces partageant le même niveau de confiance. Avantages :
- La politique suit la **logique métier** (LAN → Internet), pas le câblage.
- Si tu changes de port physique, tu déplaces l'interface de zone — les politiques ne bougent pas.
- Le firewall lui-même est une zone (**local**) : on contrôle qui peut l'administrer, le pinger, lui envoyer du NTP/DNS.

## 20. Les 4 zones prédéfinies (priorités par défaut)

| Zone | Priorité | Rôle | Interfaces typiques |
|------|----------|------|---------------------|
| **local** | 100 | Le firewall lui-même | (implicite, pas d'interface à ajouter) |
| **trust** | 85 | Réseau de confiance | LAN bureautique |
| **dmz** | 50 | Zone démilitarisée | Serveurs exposés |
| **untrust** | 5 | Non fiable | Internet / WAN FAI |

⚠️ Points critiques :
- Une interface ne peut appartenir qu'à **UNE SEULE** zone.
- Une interface **sans zone** ne fait transiter **aucun** trafic (sauf management hors-bande selon config).
- La priorité sert aux **politiques par défaut** et à certaines fonctions (ex. : par défaut, le trafic d'une zone de priorité haute vers basse peut être autorisé selon la politique par défaut configurée — **vérifie toujours ta politique par défaut**, section 29).
- La zone **local** ne contient aucune interface : c'est le trafic **à destination ou en provenance du firewall lui-même** (SSH, HTTPS d'admin, NTP, DNS proxy, VPN terminé sur le boîtier).

## 21. Créer une zone personnalisée (ex. : GUEST, IOT, PROD)

```huawei
system-view
[USG] firewall zone name GUEST
[USG-zone-guest] set priority 20        # entre untrust (5) et dmz (50)
[USG-zone-guest] add interface GigabitEthernet 1/0/5
[USG-zone-guest] quit
[USG] firewall zone name IOT
[USG-zone-iot] set priority 30
[USG-zone-iot] add interface GigabitEthernet 1/0/6
[USG-zone-iot] quit
# Vérification :
[USG] display zone
[USG] display zone name GUEST
save
```

Conseil de nommage : **MAJUSCULES, explicites, sans espaces** (GUEST, IOT, PROD, BACKUP, VIDEO). Évite les accents.

## 22. Construire une DMZ propre

Schéma type :
```
Internet ── [untrust: GE1/0/1] ── USG ── [dmz: GE1/0/2] ── Serveurs (172.16.1.0/24)
                                   └──── [trust: GE1/0/3] ── LAN (192.168.10.0/24)
```

```huawei
system-view
[USG] interface GigabitEthernet 1/0/2
[USG-GigabitEthernet1/0/2] ip address 172.16.1.1 24
[USG-GigabitEthernet1/0/2] quit
[USG] firewall zone dmz
[USG-zone-dmz] set priority 50
[USG-zone-dmz] add interface GigabitEthernet 1/0/2
[USG-zone-dmz] quit
save
```

📋 Checklist DMZ :
- [ ] Les serveurs DMZ ont des IP **fixes** (ou DHCP avec réservation).
- [ ] Politique **untrust→dmz** : uniquement les ports nécessaires (80/443 vers le reverse proxy, jamais d'admin).
- [ ] Politique **dmz→trust** : **DENY par défaut** — un serveur DMZ compromis ne doit pas rebondir vers le LAN.
- [ ] Politique **dmz→untrust** : limitée (mises à jour, DNS, NTP via proxy si possible).
- [ ] Les serveurs DMZ **ne résolvent pas** le DNS interne directement (split-DNS ou forwarder dédié).

## 23. Zone local : protéger le firewall lui-même

La zone local, c'est **toi qui décides qui touche au boîtier** :

```huawei
system-view
[USG] security-policy
[USG-policy-security] rule name local-mgmt-ssh
[USG-policy-security-rule-local-mgmt-ssh] source-zone trust
[USG-policy-security-rule-local-mgmt-ssh] destination-zone local
[USG-policy-security-rule-local-mgmt-ssh] source-address 192.168.10.0 24
[USG-policy-security-rule-local-mgmt-ssh] service ssh https
[USG-policy-security-rule-local-mgmt-ssh] action permit
[USG-policy-security-rule-local-mgmt-ssh] quit
[USG-policy-security] rule name local-deny-untrust
[USG-policy-security-rule-local-deny-untrust] source-zone untrust
[USG-policy-security-rule-local-deny-untrust] destination-zone local
[USG-policy-security-rule-local-deny-untrust] action deny
[USG-policy-security-rule-local-deny-untrust] quit
[USG-policy-security] quit
save
```

⚠️ **Piège classique** : une politique trop restrictive vers local **coupe ton accès SSH/HTTPS** — tu te retrouves à devoir aller en console en baie. Toujours tester une nouvelle règle local depuis une session existante, et garder la console à portée.

## 24. Voir et diagnostiquer les zones

```huawei
[USG] display zone                          # toutes les zones et leurs interfaces
[USG] display zone name trust               # détail d'une zone
[USG] display firewall zone statistic       # stats par zone (selon version)
[USG] display interface brief               # état des interfaces
```

🔧 « Mon interface est UP mais rien ne passe » → dans 50 % des cas, **l'interface n'est dans aucune zone**. `display zone` en 10 secondes tranche.

## 25. Priorités de zones : à quoi ça sert vraiment

- La priorité (0–100) exprime la **confiance** : local (100) > trust (85) > dmz (50) > untrust (5).
- Elle intervient dans : l'**ordre d'évaluation implicite**, certaines fonctions de **détection d'attaque** (seuils par zone), et la **lisibilité** de la politique.
- ⚠️ La priorité **ne remplace pas** une politique explicite : avec une politique par défaut en `deny`, même trust→untrust est bloqué sans règle. Ne compte jamais sur la priorité pour « autoriser par défaut ».

## 26. Cas particulier : plusieurs interfaces dans la même zone

```huawei
[USG] firewall zone trust
[USG-zone-trust] add interface GigabitEthernet 1/0/3
[USG-zone-trust] add interface GigabitEthernet 1/0/4
[USG-zone-trust] add interface Vlanif 10
[USG-zone-trust] quit
```

- Le trafic **intra-zone** (entre deux interfaces de la même zone) : par défaut, il **passe** sans politique interzone sur beaucoup de versions — vérifie avec `display firewall interzone` / teste. Si tu veux le filtrer (ex. isoler deux départements), crée des zones distinctes ou active le filtrage intra-zone.
- Utile pour : agréger plusieurs liens LAN, ou mettre un port de **bypass/miroir**.

## 27. Sous-interfaces et VLAN : zones par VLAN

```huawei
system-view
[USG] interface GigabitEthernet 1/0/3.10
[USG-GigabitEthernet1/0/3.10] vlan-type dot1q 10
[USG-GigabitEthernet1/0/3.10] ip address 192.168.10.1 24
[USG-GigabitEthernet1/0/3.10] quit
[USG] interface GigabitEthernet 1/0/3.20
[USG-GigabitEthernet1/0/3.20] vlan-type dot1q 20
[USG-GigabitEthernet1/0/3.20] ip address 192.168.20.1 24
[USG-GigabitEthernet1/0/3.20] quit
[USG] firewall zone trust
[USG-zone-trust] add interface GigabitEthernet 1/0/3.10
[USG-zone-trust] quit
[USG] firewall zone name GUEST
[USG-zone-guest] set priority 20
[USG-zone-guest] add interface GigabitEthernet 1/0/3.20
[USG-zone-guest] quit
save
```

⚠️ La sous-interface **hérite** de la zone ? **Non** : chaque sous-interface doit être ajoutée explicitement à une zone. Oubli classique après création d'un VLAN.

## 28. Web : gérer les zones en interface graphique

Chemin (New Web UI) : **Network > Zone**. On y voit les zones, leurs priorités, les interfaces membres. Bouton « Add » pour créer une zone, glisser-déposer les interfaces.
Avantage du web : **vue d'ensemble immédiate** — en CLI, `display zone` reste le plus rapide pour vérifier.

---
---

# BLOC D — POLITIQUES DE SÉCURITÉ (INTERZONE POLICY)

## 29. Principes : comment l'USG évalue une politique

1. Le firewall reçoit un paquet, détermine **zone source** (interface d'entrée) et **zone destination** (routage).
2. Il parcourt les règles **dans l'ordre** (de haut en bas) pour ce couple de zones.
3. **Première règle qui matche → appliquée** (permit/deny). Les suivantes sont ignorées.
4. Si **aucune règle ne matche** → **action par défaut** (configurable : `permit` ou `deny`).

```huawei
system-view
[USG] security-policy
[USG-policy-security] default action deny      # RECOMMANDÉ : tout ce qui n'est pas autorisé est bloqué
[USG-policy-security] quit
save
```

⚠️ **Le choix le plus important de tout le firewall.** `default action permit` = passoire (pratique en migration, mortel en prod). `default action deny` = il faut TOUT autoriser explicitement, mais tu sais exactement ce qui passe. **En prod : deny.**

## 30. Anatomie d'une règle : les 8 critères

```huawei
[USG-policy-security] rule name LAN-vers-Internet
[USG-policy-security-rule-LAN-vers-Internet] source-zone trust          # 1. zone source
[USG-policy-security-rule-LAN-vers-Internet] destination-zone untrust  # 2. zone destination
[USG-policy-security-rule-LAN-vers-Internet] source-address 192.168.10.0 24   # 3. adresses sources
[USG-policy-security-rule-LAN-vers-Internet] destination-address any   # 4. adresses destination
[USG-policy-security-rule-LAN-vers-Internet] service http https dns    # 5. services (ports)
[USG-policy-security-rule-LAN-vers-Internet] application any           # 6. application (UTM)
[USG-policy-security-rule-LAN-vers-Internet] user any                 # 7. utilisateur
[USG-policy-security-rule-LAN-vers-Internet] time-range any            # 8. plage horaire
[USG-policy-security-rule-LAN-vers-Internet] action permit             # action
[USG-policy-security-rule-LAN-vers-Internet] quit
```

Critères non renseignés = `any` (tout). **Plus la règle est précise, plus elle est sûre** — mais plus elle est longue à maintenir. L'équilibre, c'est le métier.

## 31. L'ordre des règles : le piège n°1

```
❌ MAUVAIS ORDRE :
  rule 1 : deny  trust→untrust  service p2p        ← jamais atteinte...
  rule 2 : permit trust→untrust service any        ← ...car celle-ci matche d'abord !

✅ BON ORDRE :
  rule 1 : deny  trust→untrust  service p2p        ← spécifique d'abord
  rule 2 : permit trust→untrust service http https ← général ensuite
```

**Règle d'or : du plus spécifique au plus général.** En CLI, insère une règle à la bonne position :
```huawei
[USG-policy-security] rule name bloquer-p2p
# ... critères ...
[USG-policy-security] move rule bloquer-p2p before LAN-vers-Internet
```

🔧 Voir l'ordre réel : `display security-policy rule all` (l'ordre d'affichage = l'ordre d'évaluation).

## 32. Objets d'adresse : ne jamais mettre d'IP en dur dans les règles

```huawei
system-view
[USG] ip address-set serveurs-dmz type object
[USG-object-group-address-serveurs-dmz] address 0 172.16.1.10 32
[USG-object-group-address-serveurs-dmz] address 1 172.16.1.11 32
[USG-object-group-address-serveurs-dmz] address 2 172.16.1.12 32
[USG-object-group-address-serveurs-dmz] quit
[USG] ip address-set lan-bureautique type range
[USG-object-group-address-lan-bureautique] address 0 range 192.168.10.1 192.168.10.254
[USG-object-group-address-lan-bureautique] quit
# Utilisation dans une règle :
[USG] security-policy
[USG-policy-security] rule name DMZ-vers-LAN-deny
[USG-policy-security-rule-DMZ-vers-LAN-deny] source-zone dmz
[USG-policy-security-rule-DMZ-vers-LAN-deny] destination-zone trust
[USG-policy-security-rule-DMZ-vers-LAN-deny] source-address address-set serveurs-dmz
[USG-policy-security-rule-DMZ-vers-LAN-deny] action deny
[USG-policy-security-rule-DMZ-vers-LAN-deny] quit
[USG-policy-security] quit
save
```

Avantage : quand une IP de serveur change, tu modifies **l'objet**, pas les 12 règles qui l'utilisent.

## 33. Services personnalisés (ports non standards)

```huawei
system-view
[USG] ip service-set appli-metier
[USG-object-group-service-appli-metier] service 0 protocol tcp destination-port 8443
[USG-object-group-service-appli-metier] service 1 protocol tcp destination-port 9443
[USG-object-group-service-appli-metier] service 2 protocol udp destination-port 1194
[USG-object-group-service-appli-metier] quit
save
```

Services prédéfinis utiles : `http`, `https`, `dns`, `ftp`, `smtp`, `pop3`, `ssh`, `telnet`, `ping` (icmp), `snmp`. Pour une appli métier sur port exotique, crée ton service-set — ça rend la règle **lisible** (« appli-metier » parle mieux que « tcp/8443 »).

## 34. Plages horaires : restreindre l'accès dans le temps

```huawei
system-view
[USG] time-range heures-bureau
[USG-time-range-heures-bureau] period-range 08:00:00 to 18:00:00 working-day
[USG-time-range-heures-bureau] quit
[USG] security-policy
[USG-policy-security] rule name guest-horaire
[USG-policy-security-rule-guest-horaire] source-zone GUEST
[USG-policy-security-rule-guest-horaire] destination-zone untrust
[USG-policy-security-rule-guest-horaire] time-range heures-bureau
[USG-policy-security-rule-guest-horaire] action permit
[USG-policy-security-rule-guest-horaire] quit
[USG-policy-security] quit
save
```

Cas d'usage : Wi-Fi invités coupé la nuit, accès VPN nomades limités aux horaires de travail, maintenance le week-end.

## 35. Règle LAN → Internet complète et commentée

```huawei
system-view
[USG] security-policy
# --- 1. DNS sortant (indispensable, souvent oublié) ---
[USG-policy-security] rule name LAN-dns-out
[USG-policy-security-rule-LAN-dns-out] source-zone trust
[USG-policy-security-rule-LAN-dns-out] destination-zone untrust
[USG-policy-security-rule-LAN-dns-out] source-address 192.168.10.0 24
[USG-policy-security-rule-LAN-dns-out] service dns
[USG-policy-security-rule-LAN-dns-out] action permit
[USG-policy-security-rule-LAN-dns-out] quit
# --- 2. Web + UTM (antivirus/IPS/URL, voir bloc G) ---
[USG-policy-security] rule name LAN-web-out
[USG-policy-security-rule-LAN-web-out] source-zone trust
[USG-policy-security-rule-LAN-web-out] destination-zone untrust
[USG-policy-security-rule-LAN-web-out] source-address 192.168.10.0 24
[USG-policy-security-rule-LAN-web-out] service http https
[USG-policy-security-rule-LAN-web-out] profile av-profile-lan          # antivirus
[USG-policy-security-rule-LAN-web-out] profile ips-profile-lan         # IPS
[USG-policy-security-rule-LAN-web-out] profile url-profile-lan        # filtrage URL
[USG-policy-security-rule-LAN-web-out] action permit
[USG-policy-security-rule-LAN-web-out] quit
# --- 3. Le reste, au cas par cas (exemple : NTP) ---
[USG-policy-security] rule name LAN-ntp-out
[USG-policy-security-rule-LAN-ntp-out] source-zone trust
[USG-policy-security-rule-LAN-ntp-out] source-zone trust
[USG-policy-security-rule-LAN-ntp-out] destination-zone untrust
[USG-policy-security-rule-LAN-ntp-out] service ntp
[USG-policy-security-rule-LAN-ntp-out] action permit
[USG-policy-security-rule-LAN-ntp-out] quit
[USG-policy-security] quit
save
```

💡 **Ordre recommandé** dans la politique : 1) règles de management (local), 2) deny spécifiques (P2P, geo-block), 3) règles métier par utilisateur/groupe, 4) règles générales (web, DNS), 5) default deny. Revois l'ordre avec `display security-policy rule all` après chaque ajout.

## 36. Compteurs de règles : savoir ce qui sert vraiment

```huawei
[USG] display security-policy rule all          # colonne "hit" / compteurs
[USG] display security-policy statistics rule LAN-web-out   # détail par règle (selon version)
# Remettre les compteurs à zéro pour une mesure propre :
[USG] reset security-policy statistics
```

- Une règle à **0 hit depuis 90 jours** = à désactiver puis supprimer (section 128).
- Une règle `deny` avec **beaucoup de hits** = soit une attaque, soit un flux légitime mal configuré → **enquêter** avant de se réjouir.
- 📋 Relever les compteurs **avant** toute modification : c'est ta preuve que « ça marchait avant ».

## 37. Politique intra-zone : filtrer à l'intérieur d'une même zone

Par défaut, le trafic entre deux interfaces de la **même zone** passe sans politique interzone (comportement à vérifier selon version). Pour l'isoler (ex. : deux départements sur deux ports en trust), la méthode propre :

```huawei
system-view
# Méthode recommandée : deux zones distinctes + politique explicite entre elles.
# (Le filtrage intra-zone direct dépend de la version — syntaxe à vérifier avec ?)
[USG] firewall zone name COMPTA
[USG-zone-compta] set priority 84
[USG-zone-compta] add interface GigabitEthernet 1/0/4
[USG-zone-compta] quit
[USG] security-policy
[USG-policy-security] rule name trust-compta-limite
[USG-policy-security-rule-trust-compta-limite] source-zone trust
[USG-policy-security-rule-trust-compta-limite] destination-zone COMPTA
[USG-policy-security-rule-trust-compta-limite] service ping
[USG-policy-security-rule-trust-compta-limite] action permit
[USG-policy-security-rule-trust-compta-limite] quit
[USG-policy-security] quit
save
```

💡 Si tu as besoin de filtrer, c'est que ce ne sont pas le même niveau de confiance → **deux zones**, pas du bricolage.

## 38. Filtrage géographique dans les politiques

```huawei
[USG] ip address-set pays-bloques type object
# Renseigner avec des plages par pays (via un feed ou manuellement)
[USG] security-policy
[USG-policy-security] rule name geo-block-dmz
[USG-policy-security-rule-geo-block-dmz] source-zone untrust
[USG-policy-security-rule-geo-block-dmz] destination-zone dmz
[USG-policy-security-rule-geo-block-dmz] source-address address-set pays-bloques
[USG-policy-security-rule-geo-block-dmz] action deny
[USG-policy-security-rule-geo-block-dmz] quit
[USG-policy-security] move rule geo-block-dmz before untrust-web-dmz
[USG-policy-security] quit
save
```

⚠️ Les IP des **CDN/SaaS** sont mondiales : un geo-block trop large casse les mises à jour et les services cloud. **Toujours** tester après activation (section 136).

## 39. ICMP : ping et traceroute à travers l'USG

```huawei
[USG] security-policy
[USG-policy-security] rule name ping-lan-wan
[USG-policy-security-rule-ping-lan-wan] source-zone trust
[USG-policy-security-rule-ping-lan-wan] destination-zone untrust
[USG-policy-security-rule-ping-lan-wan] service ping tracert
[USG-policy-security-rule-ping-lan-wan] action permit
[USG-policy-security-rule-ping-lan-wan] quit
# Autoriser le firewall à répondre au ping depuis le LAN (diagnostic) :
[USG-policy-security] rule name ping-vers-local
[USG-policy-security-rule-ping-vers-local] source-zone trust
[USG-policy-security-rule-ping-vers-local] destination-zone local
[USG-policy-security-rule-ping-vers-local] service ping
[USG-policy-security-rule-ping-vers-local] action permit
[USG-policy-security-rule-ping-vers-local] quit
[USG-policy-security] quit
save
```

⚠️ **Jamais** de ping untrust→local en prod (cartographie du réseau par les attaquants). Le ping sortant, oui ; le ping entrant vers le firewall, non.

## 40. Templates de politiques : industrialiser les multi-sites

Quand tu gères 5 sites avec le même socle :
1. Construis une **politique de référence** sur le site pilote (sections 35-39).
2. Exporte-la (`display current-configuration | include security-policy`).
3. Paramètre les variables (IP LAN, IP WAN, noms) dans un **script**.
4. Rejoue sur chaque site en adaptant les variables.

📋 Le template vit dans ton **gestionnaire de versions** (Git), pas dans un coin de disque. Chaque déploiement = un commit. C'est comme ça qu'on évite les « mais sur le site B on avait mis quoi déjà ? ».

## 41. Documenter la politique : l'audit sans douleur

Pour chaque règle, renseigne la **description** :
```huawei
[USG] security-policy
[USG-policy-security] rule name LAN-web-out
[USG-policy-security-rule-LAN-web-out] description "Acces web sortant bureautique - demande DSI-2026-042 - revue 2026-12"
[USG-policy-security-rule-LAN-web-out] quit
[USG-policy-security] quit
save
```

📋 Règle d'or : **pas de règle sans description** (qui l'a demandée, pourquoi, quand la revoir). Un audit avec des descriptions = 1 jour ; sans = 1 semaine.

## 42. Politique et QoS : marquer le trafic (DSCP)

```huawei
# Marquer la VoIP en EF pour que les équipements en aval la priorisent :
[USG] traffic-policy
[USG-policy-traffic] rule name marquer-voip
[USG-policy-traffic-rule-marquer-voip] source-zone trust
[USG-policy-traffic-rule-marquer-voip] destination-zone untrust
[USG-policy-traffic-rule-marquer-voip] service sip rtp
[USG-policy-traffic-rule-marquer-voip] action dscp ef
[USG-policy-traffic-rule-marquer-voip] quit
[USG-policy-traffic] quit
save
```

💡 Le marquage ne sert que si les équipements **en aval** (routeur FAI, switchs) le respectent — coordonne avec ton opérateur (souvent, le FAI réécrit ou ignore le DSCP sur l'accès Internet).

## 43. Règles de management : SNMP, syslog, NTP vers/depuis local

Complément de la section 23 — le socle « le firewall parle au SI » :
```huawei
[USG] security-policy
# Le firewall envoie ses logs (local→trust) :
[USG-policy-security] rule name local-syslog-out
[USG-policy-security-rule-local-syslog-out] source-zone local
[USG-policy-security-rule-local-syslog-out] destination-zone trust
[USG-policy-security-rule-local-syslog-out] destination-address 192.168.10.100 32
[USG-policy-security-rule-local-syslog-out] service syslog
[USG-policy-security-rule-local-syslog-out] action permit
[USG-policy-security-rule-local-syslog-out] quit
# Le firewall interroge le NTP (local→untrust) :
[USG-policy-security] rule name local-ntp-out
[USG-policy-security-rule-local-ntp-out] source-zone local
[USG-policy-security-rule-local-ntp-out] destination-zone untrust
[USG-policy-security-rule-local-ntp-out] service ntp
[USG-policy-security-rule-local-ntp-out] action permit
[USG-policy-security-rule-local-ntp-out] quit
[USG-policy-security] quit
save
```

⚠️ Avec `default action deny`, **ces règles sont obligatoires** : sans elles, pas de NTP (→ horloge fausse → logs et VPN en vrac) et pas de syslog (→ aveugle).

## 44. Politique par défaut : le filet, pas la stratégie

```huawei
[USG] security-policy
[USG-policy-security] default action deny   # base globale restrictive
[USG-policy-security] quit
save
```

💡 La politique par défaut n'est qu'un **filet** : des règles explicites pour chaque flux légitime, `deny` pour tout le reste. Certaines versions permettent d'affiner l'action par défaut **par couple de zones** (syntaxe à vérifier avec `?`) — utile pour être strict sur untrust→* tout en restant souple sur local→trust (management).

---

---

# BLOC E — NAT : TRADUCTION D'ADRESSES

## 45. Les 3 types de NAT sur USG (et quand les utiliser)

| Type | Sens | Cas d'usage |
|------|------|-------------|
| **NAT de source** (SNAT) | privé → public | LAN vers Internet (le cas n°1) |
| **NAT de destination** (DNAT / server-map) | public → privé | Publier un serveur (web, mail) |
| **NAT bidirectionnel** | les deux | DMZ avec IP publiques dédiées |

Principe VRP : le NAT se configure dans la vue **`nat-policy`** (pas dans security-policy). Le NAT s'applique **avant** la politique de sécurité pour le trafic entrant, **après** pour le sortant — l'ordre compte pour le debug (section 143).

## 46. Easy IP : le NAT de source simple (90 % des cas)

« Easy IP » = le firewall utilise **l'IP de l'interface de sortie** comme adresse source NATée. Idéal quand le FAI donne une seule IP (ou un petit bloc DHCP/PPPoE).

```huawei
system-view
[USG] nat-policy
[USG-policy-nat] rule name nat-lan-internet
[USG-policy-nat-rule-nat-lan-internet] source-zone trust
[USG-policy-nat-rule-nat-lan-internet] destination-zone untrust
[USG-policy-nat-rule-nat-lan-internet] source-address 192.168.10.0 24
[USG-policy-nat-rule-nat-lan-internet] action nat easy-ip
[USG-policy-nat-rule-nat-lan-internet] quit
[USG-policy-nat] quit
save
```

🔧 Vérifier : `display nat-policy rule all`, `display firewall session table` (voir les adresses traduites), `display nat session`.

## 47. NAT avec pool d'adresses (plusieurs IP publiques)

Quand le FAI fournit un **bloc d'IP publiques** (ex. /29 = 6 IP utilisables) :

```huawei
system-view
[USG] nat address-group pool-fai
[USG-address-group-pool-fai] section 0 203.0.113.10 203.0.113.14   # IP fictives
[USG-address-group-pool-fai] quit
[USG] nat-policy
[USG-policy-nat] rule name nat-lan-pool
[USG-policy-nat-rule-nat-lan-pool] source-zone trust
[USG-policy-nat-rule-nat-lan-pool] destination-zone untrust
[USG-policy-nat-rule-nat-lan-pool] source-address 192.168.10.0 24
[USG-policy-nat-rule-nat-lan-pool] action nat address-group pool-fai
[USG-policy-nat-rule-nat-lan-pool] quit
[USG-policy-nat] quit
save
```

Modes du pool (à vérifier selon version) :
- **PAT** (par défaut) : plusieurs privés partagent les IP du pool avec des ports différents — le plus courant.
- **1:1** : une IP privée = une IP publique (réserver pour les serveurs).
- **No-PAT** : traduction d'adresse sans traduction de port (consomme une IP publique par session).

## 48. NAT de destination : publier un serveur web (port mapping)

Scénario : serveur web interne `172.16.1.10:80`, IP publique `203.0.113.20`, port 80.

```huawei
system-view
[USG] nat-policy
[USG-policy-nat] rule name dnat-web
[USG-policy-nat-rule-dnat-web] source-zone untrust
[USG-policy-nat-rule-dnat-web] destination-zone dmz
[USG-policy-nat-rule-dnat-web] destination-address 203.0.113.20 32
[USG-policy-nat-rule-dnat-web] service http
[USG-policy-nat-rule-dnat-web] action nat static
[USG-policy-nat-rule-dnat-web] static-to 172.16.1.10
[USG-policy-nat-rule-dnat-web] quit
[USG-policy-nat] quit
# Politique de sécurité associée (OBLIGATOIRE) :
[USG] security-policy
[USG-policy-security] rule name untrust-web-dmz
[USG-policy-security-rule-untrust-web-dmz] source-zone untrust
[USG-policy-security-rule-untrust-web-dmz] destination-zone dmz
[USG-policy-security-rule-untrust-web-dmz] destination-address 172.16.1.10 32
[USG-policy-security-rule-untrust-web-dmz] service http https
[USG-policy-security-rule-untrust-web-dmz] action permit
[USG-policy-security-rule-untrust-web-dmz] quit
[USG-policy-security] quit
save
```

⚠️ **Erreur classique** : configurer le NAT mais oublier la **politique de sécurité** (ou la mettre avec la mauvaise adresse destination — après DNAT, la politique voit **l'IP privée**). Si « le NAT est configuré mais ça ne passe pas », c'est 90 % du temps la politique.

## 49. Port mapping avancé : un port public vers un port privé différent

```huawei
[USG] nat-policy
[USG-policy-nat] rule name dnat-rdp
[USG-policy-nat-rule-dnat-rdp] source-zone untrust
[USG-policy-nat-rule-dnat-rdp] destination-zone trust
[USG-policy-nat-rule-dnat-rdp] destination-address 203.0.113.20 32
[USG-policy-nat-rule-dnat-rdp] service 0 protocol tcp destination-port 13389
[USG-policy-nat-rule-dnat-rdp] action nat static
[USG-policy-nat-rule-dnat-rdp] static-to 192.168.10.50 port 3389
[USG-policy-nat-rule-dnat-rdp] quit
[USG-policy-nat] quit
```

⚠️ **Sécurité** : exposer du RDP/SSH sur Internet = cible n°1 des bots. Si vraiment nécessaire : port non standard + **restriction par IP source** + authentification forte + envisager le **SSL VPN** à la place (section 63).

## 50. NAT bidirectionnel (Two-way NAT)

Pour une DMZ dont les serveurs doivent **sortir avec leur propre IP publique** ET être **joints depuis Internet sur cette même IP** :

```huawei
system-view
[USG] nat-policy
[USG-policy-nat] rule name nat-bidir-serveur1
[USG-policy-nat-rule-nat-bidir-serveur1] source-zone dmz
[USG-policy-nat-rule-nat-bidir-serveur1] destination-zone untrust
[USG-policy-nat-rule-nat-bidir-serveur1] source-address 172.16.1.10 32
[USG-policy-nat-rule-nat-bidir-serveur1] action nat static bidirectional
[USG-policy-nat-rule-nat-bidir-serveur1] static-to 203.0.113.21
[USG-policy-nat-rule-nat-bidir-serveur1] quit
[USG-policy-nat] quit
save
```

Le `bidirectional` crée automatiquement la traduction inverse. Vérifie avec `display nat server-map`.

## 51. NAT et hairpin : accéder au serveur par son IP publique depuis le LAN

Problème : depuis le LAN, `http://203.0.113.20` ne répond pas (le paquet sort en NAT puis revient — l'USG le droppe souvent).

Solutions :
1. **Split-DNS** : en interne, le nom DNS résout vers `172.16.1.10` (recommandé, le plus propre).
2. **NAT hairpin** (si pas de DNS interne) : règle NAT trust→dmz qui traduit la destination :

```huawei
[USG] nat-policy
[USG-policy-nat] rule name hairpin-web
[USG-policy-nat-rule-hairpin-web] source-zone trust
[USG-policy-nat-rule-hairpin-web] destination-zone dmz
[USG-policy-nat-rule-hairpin-web] destination-address 203.0.113.20 32
[USG-policy-nat-rule-hairpin-web] service http
[USG-policy-nat-rule-hairpin-web] action nat static
[USG-policy-nat-rule-hairpin-web] static-to 172.16.1.10
[USG-policy-nat-rule-hairpin-web] quit
[USG-policy-nat] quit
# + politique trust→dmz correspondante (même logique que la règle untrust→dmz, section 48)
save
```

## 52. NAT pour le VPN : exemption NAT (nat no-nat)

⚠️ **Le trafic VPN ne doit PAS être NATé.** Quand une règle NAT easy-ip couvre aussi le trafic vers le tunnel, le VPN casse. Solution : règle d'exemption **avant** la règle NAT :

```huawei
[USG] nat-policy
[USG-policy-nat] rule name no-nat-vpn
[USG-policy-nat-rule-no-nat-vpn] source-zone trust
[USG-policy-nat-rule-no-nat-vpn] destination-zone untrust
[USG-policy-nat-rule-no-nat-vpn] source-address 192.168.10.0 24
[USG-policy-nat-rule-no-nat-vpn] destination-address 192.168.20.0 24   # réseau distant du VPN
[USG-policy-nat-rule-no-nat-vpn] action no-nat
[USG-policy-nat-rule-no-nat-vpn] quit
[USG-policy-nat] move rule no-nat-vpn before nat-lan-internet
[USG-policy-nat] quit
save
```

**Ordre : exemption d'abord, NAT ensuite.** C'est l'erreur n°1 des VPN qui « montent mais ne passent pas de trafic » (section 147).

## 53. ALG : quand le NAT casse les protocoles (FTP, SIP, PPTP)

FTP/SIP/H.323 embarquent des IP **dans la charge utile** — le NAT d'en-tête ne suffit pas. L'USG a des **ALG** (Application Level Gateways) :

```huawei
system-view
[USG] firewall alg ftp enable
[USG] firewall alg sip enable
[USG] firewall alg pptp enable
# Voir l'état :
[USG] display firewall alg
save
```

🔧 FTP qui liste mais ne télécharge pas en mode actif/passif → 90 % ALG ou politique sur les ports de données. SIP qui s'enregistre mais pas d'audio → ALG SIP + ouvrir RTP (UDP 10000-20000 typique, à vérifier).

## 54. Diagnostiquer le NAT : les commandes qui tranchent

```huawei
[USG] display nat-policy rule all                    # règles NAT dans l'ordre
[USG] display nat-policy statistics rule nat-lan-internet   # compteurs par règle
[USG] display firewall session table source inside 192.168.10.25  # voir la traduction live
[USG] display nat address-group pool-fai             # état du pool (adresses utilisées)
[USG] display nat server-map                         # mappings statiques (DNAT)
[USG] display firewall session table verbose         # détail : NAT avant/après
```

📋 Checklist « le NAT ne marche pas » :
- [ ] La règle NAT est-elle **avant** toute règle qui matcherait aussi ?
- [ ] Les **zones** source/destination de la règle NAT sont-elles correctes ?
- [ ] La **politique de sécurité** autorise-t-elle le flux (avec les bonnes adresses post-NAT) ?
- [ ] L'interface de sortie a-t-elle une **route par défaut** ?
- [ ] Pas de **no-nat** qui court-circuite par erreur ?

## 55. NAT64 / IPv6 : le mot rapide

L'USG6000 supporte le NAT IPv6/IPv4 (NAT64, IVI) pour la transition. En pratique PME/industrie en 2026 : **rarement utilisé** — la plupart des déploiements restent en IPv4 + NAT44. Si ton FAI te pousse de l'IPv6 natif, configure le firewall en **dual-stack** avec des politiques IPv6 dédiées plutôt qu'un NAT64 complexe, sauf besoin d'interconnexion spécifique.

## 56. Web : configurer le NAT en interface graphique

Chemin (New Web UI) : **Policy > NAT Policy**. Assistant de création : choisir le type (Source NAT / Destination NAT / Bidirectional), les zones, les objets d'adresse, le mode (Easy IP / Address Pool / Static).
💡 Le web est pratique pour **visualiser l'ordre des règles** (glisser-déposer pour réordonner) — en CLI, `move rule` fait pareil.

---
---

# BLOC F — VPN

## 57. Vue d'ensemble : quel VPN pour quel besoin

| Besoin | Technologie | Section |
|--------|-------------|---------|
| Relier deux sites fixes | **IPSec site-à-site** | 58–62 |
| Nomades / télétravail | **SSL VPN** | 63–67 |
| Tunnel simple sans chiffrement (ou sous IPSec) | **GRE** | 68–69 |
| Vieux clients Windows natifs | **L2TP over IPSec** | 70 |
| Tout le trafic chiffré + routage dynamique | **GRE over IPSec / IPSec + OSPF** | 71 |

Règle : **IPSec** pour le site-à-site (robuste, standard), **SSL VPN** pour les utilisateurs (simple, traverse les NAT/proxys).

## 58. IPSec site-à-site : architecture et prérequis

```
Site A (USG-A)                                    Site B (USG-B)
LAN 192.168.10.0/24                               LAN 192.168.20.0/24
  │                                                 │
  │ GE1/0/1 : 203.0.113.1 (publique)                │ GE1/0/1 : 198.51.100.1 (publique)
  └────────────── tunnel IPSec ─────────────────────┘
```

📋 Prérequis AVANT de configurer :
- [ ] Les deux USG se **pingent** sur leurs IP publiques (politique local : autoriser ICMP/UDP 500/4500).
- [ ] **NTP synchronisé** des deux côtés (les certificats/PSK y sont sensibles).
- [ ] Les plages LAN des deux sites **ne se chevauchent pas**.
- [ ] **Exemption NAT** prévue (section 52).
- [ ] Ports : **UDP 500** (IKE), **UDP 4500** (NAT-T), **protocole 50** (ESP) ouverts vers la zone local.

## 59. IPSec site-à-site : configuration complète (côté A)

```huawei
system-view
# ===== 1. Proposition IKE (phase 1) =====
[USG-A] ike proposal 10
[USG-A-ike-proposal-10] encryption-algorithm aes-256
[USG-A-ike-proposal-10] authentication-algorithm sha2-256
[USG-A-ike-proposal-10] authentication-method pre-share
[USG-A-ike-proposal-10] dh group14
[USG-A-ike-proposal-10] quit
# ===== 2. Peer IKE : le correspondant =====
[USG-A] ike peer peer-site-b
[USG-A-ike-peer-peer-site-b] exchange-mode main
[USG-A-ike-peer-peer-site-b] pre-shared-key TestOnly123!    # MOT DE PASSE FICTIF - à changer
[USG-A-ike-peer-peer-site-b] ike-proposal 10
[USG-A-ike-peer-peer-site-b] remote-address 198.51.100.1
[USG-A-ike-peer-peer-site-b] local-address 203.0.113.1
[USG-A-ike-peer-peer-site-b] quit
# ===== 3. Proposition IPSec (phase 2) : transform-set =====
[USG-A] ipsec proposal trans-site-b
[USG-A-ipsec-proposal-trans-site-b] encapsulation-mode tunnel
[USG-A-ipsec-proposal-trans-site-b] transform esp
[USG-A-ipsec-proposal-trans-site-b] esp encryption-algorithm aes-256
[USG-A-ipsec-proposal-trans-site-b] esp authentication-algorithm sha2-256
[USG-A-ipsec-proposal-trans-site-b] quit
# ===== 4. ACL : quel trafic va dans le tunnel =====
[USG-A] acl 3001
[USG-A-acl-adv-3001] rule permit ip source 192.168.10.0 0.0.0.255 destination 192.168.20.0 0.0.0.255
[USG-A-acl-adv-3001] quit
# ===== 5. Politique IPSec : on assemble =====
[USG-A] ipsec policy policy-site-b 10 isakmp
[USG-A-ipsec-policy-isakmp-policy-site-b-10] security acl 3001
[USG-A-ipsec-policy-isakmp-policy-site-b-10] ike-peer peer-site-b
[USG-A-ipsec-policy-isakmp-policy-site-b-10] proposal trans-site-b
[USG-A-ipsec-policy-isakmp-policy-site-b-10] quit
# ===== 6. Application sur l'interface WAN =====
[USG-A] interface GigabitEthernet 1/0/1
[USG-A-GigabitEthernet1/0/1] ipsec policy policy-site-b
[USG-A-GigabitEthernet1/0/1] quit
save
```

Côté B : **miroir** (inverser les IP et les réseaux dans l'ACL). La PSK doit être **identique** des deux côtés.

## 60. IPSec : politiques de sécurité pour le tunnel

Le trafic VPN traverse les zones — il faut l'autoriser :

```huawei
# Côté A :
[USG-A] security-policy
# Trafic LAN local vers LAN distant (via le tunnel) :
[USG-A-policy-security] rule name vpn-a-vers-b
[USG-A-policy-security-rule-vpn-a-vers-b] source-zone trust
[USG-A-policy-security-rule-vpn-a-vers-b] destination-zone untrust
[USG-A-policy-security-rule-vpn-a-vers-b] source-address 192.168.10.0 24
[USG-A-policy-security-rule-vpn-a-vers-b] destination-address 192.168.20.0 24
[USG-A-policy-security-rule-vpn-a-vers-b] action permit
[USG-A-policy-security-rule-vpn-a-vers-b] quit
# Le firewall lui-même doit accepter IKE/ESP (zone local) :
[USG-A-policy-security] rule name vpn-ike-local
[USG-A-policy-security-rule-vpn-ike-local] source-zone untrust
[USG-A-policy-security-rule-vpn-ike-local] destination-zone local
[USG-A-policy-security-rule-vpn-ike-local] source-address 198.51.100.1 32
[USG-A-policy-security-rule-vpn-ike-local] service ike esp nat-t
[USG-A-policy-security-rule-vpn-ike-local] action permit
[USG-A-policy-security-rule-vpn-ike-local] quit
[USG-A-policy-security] quit
# Exemption NAT (section 52) - CRITIQUE :
[USG-A] nat-policy
[USG-A-policy-nat] rule name no-nat-vpn-b
[USG-A-policy-nat-rule-no-nat-vpn-b] source-zone trust
[USG-A-policy-nat-rule-no-nat-vpn-b] destination-zone untrust
[USG-A-policy-nat-rule-no-nat-vpn-b] source-address 192.168.10.0 24
[USG-A-policy-nat-rule-no-nat-vpn-b] destination-address 192.168.20.0 24
[USG-A-policy-nat-rule-no-nat-vpn-b] action no-nat
[USG-A-policy-nat-rule-no-nat-vpn-b] quit
[USG-A-policy-nat] quit
save
```

## 61. IPSec : vérification et diagnostic

```huawei
[USG-A] display ike sa                       # Phase 1 : doit afficher RD/ST (ready)
[USG-A] display ipsec sa                     # Phase 2 : SPI, algorithmes, compteurs
[USG-A] display ike proposal
[USG-A] display ipsec policy
# Debug ciblé (à couper après usage !) :
[USG-A] debugging ike packet                 # échanges IKE en live
[USG-A] debugging ipsec packet               # paquets ESP
[USG-A] undo debugging all                   # TOUJOURS couper après
```

Lecture de `display ike sa` :
- **État RD** (ready) en phase 1 + SA IPSec active = tunnel OK.
- Phase 1 **absente** → problème réseau/PSK/proposal IKE (voir section 147).
- Phase 1 OK mais **pas de phase 2** → ACL ou transform-set incohérents entre les deux côtés.

## 62. IPSec : durcir la config (PFS, DPD, lifetimes)

```huawei
system-view
[USG-A] ike peer peer-site-b
[USG-A-ike-peer-peer-site-b] dpd type periodic   # Dead Peer Detection : détecte un pair mort
[USG-A-ike-peer-peer-site-b] quit
[USG-A] ipsec proposal trans-site-b
[USG-A-ipsec-proposal-trans-site-b] pfs dh-group14   # Perfect Forward Secrecy
[USG-A-ipsec-proposal-trans-site-b] quit
[USG-A] ike sa-lifetime 86400                # durée de vie phase 1 (défaut souvent 86400s)
[USG-A] ipsec sa-lifetime time-based 3600    # durée de vie phase 2
save
```

- **DPD** : indispensable — sans lui, un tunnel « à moitié mort » ne se reconstruit pas tout seul.
- **PFS** : recommandé (compromis perf/sécurité acceptable sur USG6000).
- ⚠️ Les **lifetimes doivent être cohérents** des deux côtés (pas forcément identiques, mais compatibles — le plus petit l'emporte lors de la renégociation).

## 63. SSL VPN : principe et cas d'usage

Le SSL VPN permet aux **utilisateurs nomades** d'accéder au réseau interne via un simple navigateur ou un client léger (SecoClient), en HTTPS (port 443) — ça traverse **tous les NAT et proxys** d'hôtel/aéroport.

Modes d'accès :
- **Web proxy** : accès web aux applis internes via le portail (pas de client).
- **Port forwarding** : redirection de ports TCP (ex. RDP, SSH) via applet.
- **Network extension** : client VPN complet (tunnel réseau, comme un IPSec client).
- **File sharing** : accès aux partages de fichiers via le portail.

Licences : le nombre d'utilisateurs SSL VPN simultanés dépend du modèle **et de la licence** (100 par défaut sur beaucoup de modèles, extensible — à vérifier).

## 64. SSL VPN : configuration complète (portail + network extension)

```huawei
system-view
# ===== 1. Activer le SSL VPN et créer la passerelle =====
[USG] sslvpn gateway sslgw
[USG-sslvpn-gateway-sslgw] ip address 203.0.113.1 port 443
[USG-sslvpn-gateway-sslgw] quit
# ===== 2. Domaine d'authentification (base locale pour commencer) =====
[USG] sslvpn domain domaine-vpn
[USG-sslvpn-domain-domaine-vpn] quit
# ===== 3. Politique d'accès : qui accède à quoi =====
[USG] sslvpn policy group politique-nomades
[USG-sslvpn-policy-group-politique-nomades] resource ip 192.168.10.0 24   # réseau accessible
[USG-sslvpn-policy-group-politique-nomades] quit
# ===== 4. Utilisateur local de test (fictif) =====
[USG] aaa
[USG-aaa] local-user vpn-user1 password
[USG-aaa] local-user vpn-user1 service-type sslvpn
[USG-aaa] local-user vpn-user1 privilege level 3
[USG-aaa] quit
# ===== 5. Lier la passerelle au domaine =====
[USG] sslvpn gateway sslgw
[USG-sslvpn-gateway-sslgw] domain domaine-vpn
[USG-sslvpn-gateway-sslgw] quit
# ===== 6. Politique de sécurité : autoriser le 443 vers local =====
[USG] security-policy
[USG-policy-security] rule name sslvpn-443-in
[USG-policy-security-rule-sslvpn-443-in] source-zone untrust
[USG-policy-security-rule-sslvpn-443-in] destination-zone local
[USG-policy-security-rule-sslvpn-443-in] service https
[USG-policy-security-rule-sslvpn-443-in] action permit
[USG-policy-security] quit
save
```

⚠️ Les commandes SSL VPN **varient selon les versions** (V500 vs V600, la syntaxe `sslvpn gateway` / `virtual-gateway` a évolué) — **valide chaque commande sur ta version** avec `?` avant de l'appliquer en prod. Le web (VPN > SSL VPN) est souvent plus fiable que le CLI pour cette fonction.

## 65. SSL VPN : vérifications côté utilisateur et admin

Côté utilisateur : navigateur → `https://203.0.113.1` → portail → login → accès aux ressources ou lancement du client Network Extension.

Côté admin :
```huawei
[USG] display sslvpn gateway sslgw              # état de la passerelle
[USG] display sslvpn online-user                 # utilisateurs connectés (selon version)
[USG] display sslvpn session                    # sessions actives
```

🔧 « La page du portail ne s'ouvre pas » → checklist section 150. « Connecté mais pas d'accès réseau » → politique de sécurité pour le **trafic issu du tunnel** (les utilisateurs VPN arrivent souvent dans une zone dédiée ou trust — autoriser explicitement).

## 66. SSL VPN : certificats (ne pas rester en auto-signé)

Par défaut, le portail utilise un **certificat auto-signé** → alerte navigateur à chaque connexion, les utilisateurs s'habituent à cliquer « accepter le risque » (mauvaise hygiène).

Procédure :
1. Générer une CSR ou importer un certificat d'une **CA reconnue** (Let's Encrypt via ton infra, ou CA d'entreprise).
2. Web : System > Certificate Management > Import.
3. Lier le certificat à la passerelle SSL VPN.
4. 📋 Renouveler **avant expiration** — un portail VPN avec certificat expiré un lundi 8h, c'est 50 appels au support.

## 67. SSL VPN : bonnes pratiques de sécurité

- **Double authentification** : mot de passe + certificat client ou OTP (selon version/licence).
- **Contrôle de conformité** du poste (antivirus à jour) si la fonction est disponible.
- **Split tunneling** : ne router dans le VPN que le trafic interne (perf + vie privée) — ou **full tunneling** si la politique impose l'inspection de tout le trafic (à décider, pas par défaut).
- **Timeout de session** agressif (15–30 min d'inactivité).
- **Journaliser** les connexions (qui, quand, depuis quelle IP).

## 68. GRE : tunnel simple entre deux USG

GRE = encapsulation sans chiffrement. Utile pour transporter des protocoles non-IP ou du multicast (ex. OSPF entre sites), **souvent protégé par IPSec par-dessus** (GRE over IPSec).

```huawei
system-view
# Côté A :
[USG-A] interface Tunnel 0
[USG-A-Tunnel0] ip address 10.255.0.1 30
[USG-A-Tunnel0] tunnel-protocol gre
[USG-A-Tunnel0] source 203.0.113.1
[USG-A-Tunnel0] destination 198.51.100.1
[USG-A-Tunnel0] quit
[USG-A] firewall zone untrust
[USG-A-zone-untrust] add interface Tunnel 0
[USG-A-zone-untrust] quit
# Route vers le LAN distant via le tunnel :
[USG-A] ip route-static 192.168.20.0 24 Tunnel 0
save
```

⚠️ GRE seul = **trafic en clair** sur Internet. En prod, toujours GRE **over IPSec** (appliquer une `ipsec policy` sur l'interface Tunnel ou utiliser un profil IPSec).

## 69. GRE over IPSec : le combo routage + chiffrement

```huawei
# Sur l'interface Tunnel existante, ajouter la protection IPSec :
[USG-A] interface Tunnel 0
[USG-A-Tunnel0] ipsec policy policy-gre
[USG-A-Tunnel0] quit
# Avec une ACL qui matche le trafic GRE (protocole 47) :
[USG-A] acl 3002
[USG-A-acl-adv-3002] rule permit gre source 203.0.113.1 0 destination 198.51.100.1 0
[USG-A-acl-adv-3002] quit
```

Avantage : tu peux faire passer de l'**OSPF** dans le tunnel → routage dynamique entre sites, bascule automatique. Voir section 71.

## 70. L2TP over IPSec : pour les clients Windows natifs

```huawei
system-view
# Serveur L2TP :
[USG] l2tp enable
[USG] l2tp-group 1
[USG-l2tp-1] tunnel password Fictif123!          # mot de passe tunnel (fictif)
[USG-l2tp-1] allow l2tp virtual-template 1 remote client1
[USG-l2tp-1] quit
[USG] interface Virtual-Template 1
[USG-Virtual-Template1] ip address 192.168.99.1 24
[USG-Virtual-Template1] ppp authentication-mode chap
[USG-Virtual-Template1] quit
# Pool d'adresses pour les clients :
[USG] ip pool pool-l2tp 192.168.99.10 192.168.99.100
# Utilisateur :
[USG] aaa
[USG-aaa] local-user client1 password
[USG-aaa] local-user client1 service-type ppp
[USG-aaa] quit
save
```

Ensuite, protéger avec IPSec (politique IPSec sur l'interface WAN avec ACL UDP 1701). Côté Windows : créer une connexion VPN L2TP/IPSec avec clé pré-partagée. ⚠️ L2TP seul (sans IPSec) = **mots de passe en clair** — jamais en prod.

## 71. Routage dynamique dans le VPN (OSPF sur GRE/IPSec)

```huawei
# Côté A (après avoir monté le Tunnel 0, section 68) :
[USG-A] ospf 1
[USG-A-ospf-1] area 0
[USG-A-ospf-1-area-0.0.0.0] network 192.168.10.0 0.0.0.255
[USG-A-ospf-1-area-0.0.0.0] network 10.255.0.0 0.0.0.3
[USG-A-ospf-1-area-0.0.0.0] quit
[USG-A-ospf-1] quit
# Sécuriser OSPF (authentification MD5) :
[USG-A] interface Tunnel 0
[USG-A-Tunnel0] ospf authentication-mode md5 1 FictifOspf123
[USG-A-Tunnel0] quit
save
```

Vérif : `display ospf peer`, `display ip routing-table`. Si le tunnel tombe, OSPF reroute via la route de secours (si tu en as une).

## 72. VPN : tableau de diagnostic express

| Symptôme | `display ike sa` | Cause probable | Section |
|----------|------------------|----------------|---------|
| Rien ne monte | vide | Réseau / politique local / NAT | 147 |
| Phase 1 absente | vide | PSK / proposal IKE / ports | 147 |
| Phase 1 OK, pas de phase 2 | RD mais pas d'IPSec SA | ACL / transform-set | 147 |
| Tunnel OK, pas de trafic | SA actives | Politique / NAT / routage | 147 |
| Tunnel instable | SA qui flappent | DPD / lifetimes / double NAT | 148 |

---
---

# BLOC G — UTM : INSPECTION DE CONTENU (AV, IPS, URL, APPLICATIF, ANTI-SPAM)

## 73. L'UTM et la performance : la vérité honnête

**C'est LA section à lire avant d'activer quoi que ce soit.**

- Le **débit firewall** (section 2) mesure du simple filtrage stateful sur des paquets UDP calibrés.
- Dès que tu actives **AV + IPS + URL filtering**, le firewall doit **reassembler les flux TCP, décoder les protocoles (HTTP, SMTP, FTP...), matcher des dizaines de milliers de signatures** — c'est 3 à 10× plus coûteux en CPU.
- Ordres de grandeur constatés (à vérifier sur la fiche du modèle exact, conditions réelles) :
  - **Firewall seul** : 100 % du débit nominal.
  - **+ IPS** : ~60–70 % du débit nominal.
  - **+ IPS + AV** : ~40–60 % (le constructeur annonce jusqu'à ~15 Gbit/s IPS+AV sur les gros modèles, en conditions de test).
  - **+ IPS + AV + URL + SSL inspection** : ~25–40 %.

**Règles de décision :**
1. **Dimensionne sur le débit UTM**, pas sur le débit firewall. Si ton lien fait 1 Gbit/s et que tu veux IPS+AV partout, prends un modèle dont le débit IPS+AV ≥ 1 Gbit/s avec de la marge.
2. **N'active pas tout, partout** : applique les profils UTM uniquement sur les règles qui en ont besoin (LAN→Internet oui ; backup→NAS non).
3. **L'inspection SSL/TLS** est la plus coûteuse (débit dédié souvent 2–5× inférieur au débit IPS). Ne l'active que sur les flux à risque, avec consentement/cadre légal (inspection = lecture du contenu chiffré).
4. 🔧 Surveille : `display cpu`, `display memory`, et les compteurs de **sessions dropped par manque de ressources**.

## 74. Architecture UTM sur USG : profils + politiques

Logique en 2 temps :
1. **Créer un profil** (ex. `av-profile-lan`) : quoi détecter, quelle action.
2. **Attacher le profil à une règle de politique** (section 35) : sur quel flux l'appliquer.

```
Politique LAN→Internet (permit)
   ├── profile AV      → scanne les fichiers téléchargés
   ├── profile IPS     → bloque les exploits connus
   ├── profile URL     → filtre les catégories de sites
   └── profile APP     → contrôle les applications (P2P, réseaux sociaux...)
```

Avantage : **un même profil réutilisable** sur plusieurs règles ; modification centralisée.

## 75. Antivirus : configuration du profil

```huawei
system-view
[USG] profile type av name av-profile-lan
[USG-profile-av-av-profile-lan] exception application any   # exceptions éventuelles
# Action en cas de virus détecté : alert / block (recommandé : block)
[USG-profile-av-av-profile-lan] quit
# Attacher à la règle (déjà vu section 35) :
[USG] security-policy
[USG-policy-security] rule name LAN-web-out
[USG-policy-security-rule-LAN-web-out] profile av-profile-lan
[USG-policy-security-rule-LAN-web-out] quit
[USG-policy-security] quit
save
```

Protocoles scannés typiques : HTTP, FTP, SMTP, POP3, IMAP (à vérifier selon version/licence).
⚠️ **Limites** : l'AV réseau ne remplace pas l'antivirus des postes (il ne voit pas le chiffré sans inspection SSL, ni les menaces zero-day). C'est une **couche**, pas une solution.

## 76. Mise à jour des signatures AV/IPS (vital)

Des signatures vieilles de 6 mois = une passoire qui a l'air de protéger.

```huawei
[USG] display av update info          # version des signatures AV (selon version)
[USG] display ips signature version   # version des signatures IPS
```

- **Automatique** : planifier la mise à jour quotidienne (web : System > Update Center, ou CLI selon version). Nécessite un accès Internet (ou un serveur de mise à jour local).
- **Manuelle** : téléchargement sur support.huawei.com → import via web ou clé USB.
- 📋 Checklist : [ ] MAJ auto activée [ ] dernière MAJ < 7 jours [ ] alerte si échec de MAJ (syslog).
- 🔧 Après une MAJ de signatures, **surveille les faux positifs** (une signature trop agressive peut bloquer une appli métier — voir section 153).

## 77. IPS : choisir le bon niveau de protection

L'IPS détecte les **exploits** (tentatives d'intrusion, scans, shellcodes), pas les virus.

```huawei
system-view
[USG] profile type ips name ips-profile-lan
# Assigner un niveau : les profils prédéfinis vont de "léger" à "strict"
# (noms exacts à vérifier selon version : p.ex. default, strict)
[USG-profile-ips-ips-profile-lan] quit
[USG] security-policy
[USG-policy-security] rule name LAN-web-out
[USG-policy-security-rule-LAN-web-out] profile ips-profile-lan
[USG-policy-security-rule-LAN-web-out] quit
[USG-policy-security] quit
save
```

Stratégie terrain :
- **LAN→Internet** : IPS en mode **block** sur les signatures critiques/hautes, **alert** sur les moyennes (pour éviter les faux positifs bloquants au début).
- **untrust→DMZ** : IPS en **block** strict — c'est ta vitrine, elle est scannée en permanence.
- **Phase pilote** : 1 à 2 semaines en **alert only**, analyse des logs, puis bascule en block. Ça évite le « l'IPS a coupé la prod » du lundi matin.

## 78. Filtrage URL : catégories et listes

Base de données : **130+ catégories, 500M+ d'URL** (cloud Huawei, mise à jour auto).

```huawei
system-view
[USG] profile type url-filter name url-profile-lan
# Exemple de logique : bloquer les catégories à risque, alerter le reste
[USG-profile-url-filter-url-profile-lan] quit
[USG] security-policy
[USG-policy-security] rule name LAN-web-out
[USG-policy-security-rule-LAN-web-out] profile url-profile-lan
[USG-policy-security-rule-LAN-web-out] quit
[USG-policy-security] quit
save
```

Catégories à bloquer en entreprise (à adapter à ta charte informatique) :
- **Bloquer** : malware, phishing, pornographie, jeux d'argent, P2P/torrents, proxys d'anonymisation.
- **Limiter** (quota temps ou alerte) : réseaux sociaux, streaming vidéo, shopping.
- **Listes blanches/noires personnalisées** : toujours prévoir une whitelist pour les faux positifs (le site d'un fournisseur classé « parking de domaine » par erreur, ça arrive).

⚠️ HTTPS : sans **inspection SSL**, le filtrage URL ne voit que le **nom de domaine** (via SNI), pas l'URL complète. `https://malin.example.com/virus.exe` sera vu comme `malin.example.com`.

## 79. Contrôle applicatif : bloquer par application, pas par port

6000+ applications reconnues (signatures + comportement, pas les ports).

```huawei
system-view
[USG] profile type app name app-profile-lan
[USG-profile-app-app-profile-lan] quit
[USG] security-policy
[USG-policy-security] rule name LAN-web-out
[USG-policy-security-rule-LAN-web-out] profile app-profile-lan
[USG-policy-security-rule-LAN-web-out] quit
# Ou directement dans la règle (filtrage par application, sans profil) :
[USG-policy-security] rule name bloquer-p2p-strict
[USG-policy-security-rule-bloquer-p2p-strict] source-zone trust
[USG-policy-security-rule-bloquer-p2p-strict] destination-zone untrust
[USG-policy-security-rule-bloquer-p2p-strict] application p2p
[USG-policy-security-rule-bloquer-p2p-strict] action deny
[USG-policy-security-rule-bloquer-p2p-strict] quit
[USG-policy-security] quit
save
```

Cas d'usage : bloquer **BitTorrent, Tor, applications de contournement**, limiter **YouTube/Netflix** en bande passante (avec la gestion de bande passante, section 80), autoriser **WhatsApp** mais pas les appels vidéo, etc.

## 80. Gestion de bande passante par application/utilisateur

```huawei
system-view
# Limiter le streaming à 50 Mbit/s au total sur la règle LAN→Internet :
[USG] traffic-policy
[USG-policy-traffic] profile bandwidth-profile-streaming
[USG-traffic-profile-bandwidth-profile-streaming] bandwidth maximum 50000  # kbit/s
[USG-traffic-profile-bandwidth-profile-streaming] quit
[USG-policy-traffic] rule name limiter-streaming
[USG-policy-traffic-rule-limiter-streaming] source-zone trust
[USG-policy-traffic-rule-limiter-streaming] destination-zone untrust
[USG-policy-traffic-rule-limiter-streaming] application streaming-media
[USG-policy-traffic-rule-limiter-streaming] action bandwidth profile bandwidth-profile-streaming
[USG-policy-traffic-rule-limiter-streaming] quit
[USG-policy-traffic] quit
save
```

💡 **Garantir** la bande passante de la VoIP/appli métier (minimum garanti) est souvent plus utile que de limiter le reste. Les deux se combinent.

## 81. Anti-spam : protéger la messagerie

L'anti-spam USG filtre les mails entrants (SMTP) : RBL (listes noires), analyse de contenu, anti-phishing.

```huawei
system-view
[USG] profile type antispam name antispam-profile-mail
[USG-profile-antispam-antispam-profile-mail] quit
[USG] security-policy
[USG-policy-security] rule name mail-entrant
[USG-policy-security-rule-mail-entrant] source-zone untrust
[USG-policy-security-rule-mail-entrant] destination-zone dmz
[USG-policy-security-rule-mail-entrant] destination-address 172.16.1.25 32  # serveur mail
[USG-policy-security-rule-mail-entrant] service smtp
[USG-policy-security-rule-mail-entrant] profile antispam-profile-mail
[USG-policy-security-rule-mail-entrant] action permit
[USG-policy-security-rule-mail-entrant] quit
[USG-policy-security] quit
save
```

⚠️ L'anti-spam du firewall est un **complément**, pas un remplaçant d'une vraie passerelle mail (qui gère aussi l'anti-phishing avancé, le sandboxing de pièces jointes, la continuité). Si ton flux mail est critique, envisage une solution dédiée.

## 82. Inspection SSL/TLS : puissance et précautions

```huawei
# Principe : l'USG se place en "man-in-the-middle" légitime avec un certificat CA interne
# déployé sur les postes (via GPO/AD).
# Activer l'inspection sur une règle (syntaxe à vérifier selon version) :
[USG] security-policy
[USG-policy-security] rule name LAN-web-inspecte
[USG-policy-security-rule-LAN-web-inspecte] source-zone trust
[USG-policy-security-rule-LAN-web-inspecte] destination-zone untrust
[USG-policy-security-rule-LAN-web-inspecte] service https
[USG-policy-security-rule-LAN-web-inspecte] profile av-profile-lan
[USG-policy-security-rule-LAN-web-inspecte] action permit
# + activer le déchiffrement SSL dans les paramètres de la règle / du profil
[USG-policy-security-rule-LAN-web-inspecte] quit
[USG-policy-security] quit
save
```

⚠️ **Triple avertissement :**
1. **Légal** : en France/Europe, l'inspection du trafic chiffré des salariés exige **information des représentants du personnel / charte informatique**, et des **exclusions** (banque, santé...). Valide avec ta direction/juriste.
2. **Technique** : déploie la **CA interne sur tous les postes** (sinon alertes certificat partout) ; certaines applis (bancaires, avec certificate pinning) **cassent** — prévois une liste d'exclusion.
3. **Performance** : c'est la fonction la plus gourmande (section 73). Ne l'active que là où c'est justifié.

## 83. Filtrage de fichiers : bloquer les types dangereux

```huawei
system-view
[USG] profile type file-filter name file-profile-lan
# Bloquer : .exe, .bat, .ps1, .vbs, .js en téléchargement web/mail (selon version)
[USG-profile-file-filter-file-profile-lan] quit
[USG] security-policy
[USG-policy-security] rule name LAN-web-out
[USG-policy-security-rule-LAN-web-out] profile file-profile-lan
[USG-policy-security-rule-LAN-web-out] quit
[USG-policy-security] quit
save
```

💡 Moins violent que l'AV seul : le file-filter bloque **par extension/type**, même sans signature. Parfait contre les `.exe` « cadeaux » des campagnes de phishing.

## 84. Politique de contenu : assembler les profils par zone (matrice recommandée)

| Flux | AV | IPS | URL | APP | File-filter | Inspection SSL |
|------|----|-----|-----|-----|-------------|----------------|
| trust→untrust (LAN) | ✅ block | ✅ block/alert | ✅ | ✅ | ✅ | ⚠️ ciblé |
| GUEST→untrust | ✅ block | ✅ block | ✅ strict | ✅ strict | ✅ | ❌ |
| untrust→dmz | ✅ block | ✅ block strict | ❌ | ❌ | ✅ | ❌ |
| dmz→untrust | ❌ | ✅ alert | ❌ | ❌ | ❌ | ❌ |
| trust→dmz | ❌ | ✅ alert | ❌ | ❌ | ❌ | ❌ |
| VPN→trust | ✅ block | ✅ block | ❌ | ❌ | ❌ | ❌ |

Légende : ✅ recommandé, ⚠️ selon contexte légal/technique, ❌ inutile (coût perf sans bénéfice).

## 85. Tester l'UTM : le fichier EICAR et les sites de test

- **EICAR** : télécharge le fichier test `eicar.com` (inoffensif, détecté par tous les AV) depuis le LAN → l'USG doit le **bloquer** et logger. Si ça passe, ton profil AV n'est pas actif sur la règle.
- **URL test** : des sites de test constructeur/standards (ex. pages de test de filtrage) pour valider le blocage par catégorie.
- **IPS** : déclencheurs de test (scan de ports depuis l'extérieur vers la DMZ) → vérifier les logs IPS.
- 📋 Après chaque changement de profil UTM : **re-teste EICAR**. 2 minutes, zéro risque, 100 % utile.

## 86. UTM et faux positifs : la procédure

1. L'utilisateur signale un blocage légitime → récupérer **l'heure, l'URL/IP, l'utilisateur**.
2. Logs UTM → identifier le **profil et la signature** en cause.
3. Décision : **exception ciblée** (whitelist d'URL, exclusion de signature) — jamais « désactiver le profil ».
4. Documenter l'exception (qui, quoi, pourquoi, date de revue).
5. 📋 **Revue trimestrielle** des exceptions : une whitelist oubliée, c'est une porte ouverte.

## 87. Signatures : mise à jour hors-ligne (site isolé)

Si l'USG n'a pas d'accès Internet :
1. Télécharger les packs de signatures sur support.huawei.com (depuis un poste connecté).
2. Les copier sur **clé USB**.
3. Web : System > Update > Local Update, ou CLI selon version.
4. Vérifier les versions (`display av update info`).
5. 📋 Planifier : quelqu'un doit le faire **tous les mois** — mets-le dans le planning de maintenance (section 126).

## 88. Web : superviser l'UTM en un coup d'œil

Chemin (New Web UI) : **Monitor > Security Protection** (ou Threat/UTM selon version) : top des menaces bloquées, top des URL bloquées, état des signatures, utilisation CPU par moteur.
💡 En routine hebdo, 5 minutes sur cet écran valent mieux qu'un rapport mensuel que personne ne lit.

---
---

# BLOC H — AUTHENTIFICATION DES UTILISATEURS

## 89. Pourquoi authentifier : la politique par utilisateur

Sans authentification, tes règles voient des **IP**. Avec, elles voient des **personnes** : « le service compta accède à l'appli métier, les stagiaires non » — même en DHCP, même en Wi-Fi.

Modes supportés : **base locale**, **RADIUS**, **LDAP/AD**, **certificats**, **SAML/OTP** (selon version/licence).

## 90. Base locale : pour les petits sites et le secours

```huawei
system-view
[USG] aaa
[USG-aaa] local-user alice password
[USG-aaa] local-user alice service-type sslvpn web
[USG-aaa] local-user alice privilege level 3
[USG-aaa] local-user alice access-limit 2      # 2 sessions simultanées max
[USG-aaa] quit
# Groupe local :
[USG] user-group comptabilite
[USG-user-group-comptabilite] quit
[USG] aaa
[USG-aaa] local-user alice user-group comptabilite
[USG-aaa] quit
save
```

⚠️ La base locale, c'est pour **< 20 utilisateurs** ou le **compte de secours**. Au-delà : AD/LDAP/RADIUS (sinon tu passes ta vie à gérer les mots de passe).

## 91. AD/LDAP : brancher l'annuaire d'entreprise

```huawei
system-view
[USG] ldap-server template template-ad
[USG-ldap-template-ad] ldap-server authentication 192.168.10.5 389   # IP fictive : ton AD
[USG-ldap-template-ad] ldap-server bind-dn cn=svc-firewall,ou=services,dc=entreprise,dc=local
[USG-ldap-template-ad] ldap-server bind-password Fictif123!
[USG-ldap-template-ad] ldap-server search-dn ou=utilisateurs,dc=entreprise,dc=local
[USG-ldap-template-ad] quit
[USG] aaa
[USG-aaa] authentication-scheme auth-ad
[USG-aaa-authentication-scheme-auth-ad] authentication-mode ldap
[USG-aaa-authentication-scheme-auth-ad] quit
[USG-aaa] domain domaine-entreprise
[USG-aaa-domain-domaine-entreprise] authentication-scheme auth-ad
[USG-aaa-domain-domaine-entreprise] ldap-server template-ad
[USG-aaa-domain-domaine-entreprise] quit
[USG-aaa] quit
save
```

🔧 Tester : `test-aaa ldap-template template-ad user alice password` (syntaxe à vérifier selon version) ou via le bouton « Test » du web. **Toujours tester AVANT** de basculer les utilisateurs dessus.

## 92. RADIUS : l'alternative standard

```huawei
system-view
[USG] radius-server template template-radius
[USG-radius-template-radius] radius-server authentication 192.168.10.6 1812
[USG-radius-template-radius] radius-server shared-key FictifRadius123!
[USG-radius-template-radius] quit
[USG] aaa
[USG-aaa] authentication-scheme auth-radius
[USG-aaa-authentication-scheme-auth-radius] authentication-mode radius
[USG-aaa-authentication-scheme-auth-radius] quit
[USG-aaa] domain domaine-entreprise
[USG-aaa-domain-domaine-entreprise] authentication-scheme auth-radius
[USG-aaa-domain-domaine-entreprise] radius-server template-radius
[USG-aaa-domain-domaine-entreprise] quit
[USG-aaa] quit
save
```

💡 RADIUS = le choix naturel si tu as déjà un **NPS/FreeRADIUS** (souvent couplé à l'authentification Wi-Fi/802.1X — même annuaire, même logique).

## 93. Politique par utilisateur/groupe : l'exemple qui parle

```huawei
[USG] security-policy
[USG-policy-security] rule name compta-appli-metier
[USG-policy-security-rule-compta-appli-metier] source-zone trust
[USG-policy-security-rule-compta-appli-metier] destination-zone dmz
[USG-policy-security-rule-compta-appli-metier] user-group comptabilite
[USG-policy-security-rule-compta-appli-metier] destination-address 172.16.1.30 32
[USG-policy-security-rule-compta-appli-metier] service https
[USG-policy-security-rule-compta-appli-metier] action permit
[USG-policy-security-rule-compta-appli-metier] quit
# Les autres utilisateurs : règle générale SANS critère user, placée APRÈS :
[USG-policy-security] rule name lan-dmz-general
[USG-policy-security-rule-lan-dmz-general] source-zone trust
[USG-policy-security-rule-lan-dmz-general] destination-zone dmz
[USG-policy-security-rule-lan-dmz-general] service ping
[USG-policy-security-rule-lan-dmz-general] action permit
[USG-policy-security-rule-lan-dmz-general] quit
[USG-policy-security] quit
save
```

⚠️ Pour que le critère `user` fonctionne, l'utilisateur doit être **authentifié** (portail, SSL VPN, 802.1X...) — sinon la règle ne matche jamais. Prévois une **politique d'authentification** (qui doit s'authentifier, quand).

## 94. Portail captif : authentifier les utilisateurs du LAN

```huawei
system-view
[USG] web-auth-server template portail-interne port 8080
[USG] authentication-policy
[USG-policy-auth] rule name auth-lan
[USG-policy-auth-rule-auth-lan] source-zone trust
[USG-policy-auth-rule-auth-lan] destination-zone untrust
[USG-policy-auth-rule-auth-lan] source-address 192.168.10.0 24
[USG-policy-auth-rule-auth-lan] action auth
[USG-policy-auth-rule-auth-lan] quit
[USG-policy-auth] quit
save
```

L'utilisateur ouvre son navigateur → portail → login → ses flux sont ensuite associés à son identité. (Syntaxe exacte à vérifier selon version — le web **Policy > Authentication Policy** est plus direct.)

## 95. Verrouillage et politique de mot de passe (comptes locaux)

```huawei
system-view
[USG] aaa
[USG-aaa] local-user password-policy
# Complexité, longueur min, historique, durée de vie (paramètres selon version)
[USG-aaa] quit
save
```

Voir aussi section 133 (durcissement). Règle : **les comptes locaux d'admin** suivent la même politique que les utilisateurs — pas de `Admin123` qui traîne.

## 96. Diagnostic d'authentification

```huawei
[USG] display aaa online-fail-record       # échecs de connexion récents
[USG] display local-user                   # comptes locaux
[USG] display ldap-server template template-ad   # état du template LDAP
```

🔧 « Personne ne s'authentifie depuis ce matin » → vérifier dans l'ordre : **connectivité vers l'AD** (ping/LDAP port 389), **compte de bind** (mot de passe expiré ?), **politique** (quelqu'un a touché aux règles ?), **heure** (désynchro NTP = échec Kerberos/LDAP).

---
---

# BLOC I — HAUTE DISPONIBILITÉ (HOT STANDBY)

## 97. Principe : VGMP + HRP, en clair

- **HRP** (Huawei Redundancy Protocol) : synchronise **sessions, tables NAT, tables de routage, infos VPN** entre les deux USG.
- **VGMP** (VRRP Group Management Protocol) : gère le **basculement** — regroupe les VRRP des interfaces ; si un lien ou un boîtier tombe, tout bascule d'un coup (pas interface par interface).
- Mode **actif/passif** (hot standby) : un seul traite le trafic, l'autre attend **synchronisé**. Bascule en **quelques secondes**, sessions conservées (pas de coupure TCP).

## 98. Prérequis matériels et réseau

📋 Checklist :
- [ ] **2 USG identiques** : même modèle, même version logicielle, mêmes cartes d'extension.
- [ ] **Licences équivalentes** des deux côtés (sinon, après bascule, l'UTM ne protège plus).
- [ ] **Lien heartbeat dédié** : 1 interface (ou 2 pour la redondance) reliée en direct entre les deux USG — **jamais** via un switch partagé avec le trafic.
- [ ] Les interfaces de même nom des deux USG branchées sur les **mêmes réseaux** (même VLAN/switch).
- [ ] Câblage symétrique : GE1/0/1 du A et GE1/0/1 du B sur le même switch WAN, etc.

## 99. Configuration HA complète (les deux nœuds)

```huawei
# ===== Sur USG-A (futur actif) =====
system-view
[USG-A] hrp enable
[USG-A] hrp interface GigabitEthernet 1/0/7 remote 10.0.7.2   # heartbeat vers le pair
[USG-A] interface GigabitEthernet 1/0/7
[USG-A-GigabitEthernet1/0/7] ip address 10.0.7.1 30
[USG-A-GigabitEthernet1/0/7] quit
# VRRP sur chaque interface de production (exemple WAN) :
[USG-A] interface GigabitEthernet 1/0/1
[USG-A-GigabitEthernet1/0/1] vrrp vrid 1 virtual-ip 203.0.113.1
[USG-A-GigabitEthernet1/0/1] vrrp vrid 1 priority 120
[USG-A-GigabitEthernet1/0/1] quit
# ... répéter sur LAN, DMZ ...
# Groupe VGMP :
[USG-A] vrrp group 1
[USG-A-vrrp-group-1] add interface GigabitEthernet 1/0/1 vrid 1
[USG-A-vrrp-group-1] add interface GigabitEthernet 1/0/3 vrid 2
[USG-A-vrrp-group-1] quit
[USG-A] hrp track vrrp group 1
save
```

```huawei
# ===== Sur USG-B (futur passif) : MIROIR avec priorités plus basses =====
system-view
[USG-B] hrp enable
[USG-B] hrp interface GigabitEthernet 1/0/7 remote 10.0.7.1
[USG-B] interface GigabitEthernet 1/0/7
[USG-B-GigabitEthernet1/0/7] ip address 10.0.7.2 30
[USG-B-GigabitEthernet1/0/7] quit
[USG-B] interface GigabitEthernet 1/0/1
[USG-B-GigabitEthernet1/0/1] vrrp vrid 1 virtual-ip 203.0.113.1
[USG-B-GigabitEthernet1/0/1] vrrp vrid 1 priority 100
[USG-B-GigabitEthernet1/0/1] quit
# ... même VGMP ...
[USG-B] vrrp group 1
[USG-B-vrrp-group-1] add interface GigabitEthernet 1/0/1 vrid 1
[USG-B-vrrp-group-1] add interface GigabitEthernet 1/0/3 vrid 2
[USG-B-vrrp-group-1] quit
[USG-B] hrp track vrrp group 1
save
```

⚠️ La syntaxe HRP/VGMP **a évolué selon les versions** — valide chaque commande avec `?` sur ta version. Le principe (heartbeat + VRRP par interface + groupe VGMP) est constant.

## 100. Vérifier que la HA est saine

```huawei
[USG-A] display hrp state verbose     # rôle : active / standby, état du peer
[USG-A] display hrp statistics       # paquets heartbeat, erreurs
[USG-A] display vrrp                 # état des VRID
[USG-A] display hrp sync-status      # état de la synchronisation de config/sessions
```

État sain : A = **active**, B = **standby**, heartbeat OK, sync OK, compteurs d'erreurs à 0.

## 101. Tester le basculement (en heures creuses !)

📋 Procédure de test :
1. Prévenir (même en heures creuses, quelqu'un doit savoir).
2. Ouvrir un **ping continu** et une **session TCP** (ex. SSH) à travers le firewall.
3. Sur l'actif : `hrp switch` (bascule manuelle) ou débrancher le lien WAN.
4. Observer : le ping perd **1 à 3 paquets**, la session SSH **survit** (grâce à la sync HRP).
5. `display hrp state verbose` sur les deux nœuds → les rôles ont permuté.
6. Rebasculer si besoin (`hrp switch` sur le nouveau passif, ou préemption selon config).
7. 📋 **Tester 2 fois par an.** Une HA jamais testée = une HA qui ne marche pas le jour J.

## 102. Session asymétrique en HA : le problème et le remède

**Symptôme** : en temps normal ça marche, mais après un basculement (ou avec un routage asymétrique), des sessions tombent.

**Cause** : le paquet retour arrive sur le nœud qui n'a pas la session (ou la session n'a pas été synchronisée à temps).

**Remèdes** :
- Vérifier que **HRP synchronise bien** (`display hrp sync-status`) — configurer la **synchro automatique** (`hrp auto-sync` / `hrp sync` selon version).
- Vérifier le **câblage symétrique** (section 98) : si le retour passe par un autre chemin physique, c'est mort.
- Éviter les **routes asymétriques** en amont (HSRP/VRRP des routeurs autour).
- 🔧 `display firewall session table` comparé sur les deux nœuds : les tables doivent être **quasi identiques**.

## 103. HA et VPN : ce qui est synchronisé (et ce qui ne l'est pas)

- **Synchronisé** : sessions IPSec/SSL VPN, SA, compteurs — le tunnel **survit** au basculement (renégociation évitée dans la plupart des cas).
- ⚠️ **Non synchronisé** : les **certificats** (à installer sur les deux nœuds), les **licences** (une par nœud), certains états applicatifs.
- 📋 Après tout changement de certificat ou de licence : **le refaire sur le second nœud** et vérifier.

## 104. HA et upgrade firmware : sans coupure (ou presque)

1. Upgrader le **passif** d'abord, le rebooter, vérifier qu'il revient en standby sain.
2. Basculer (`hrp switch`) → l'ancien actif devient passif.
3. Upgrader l'ex-actif, le rebooter.
4. Vérifier la paire, rebasculer si besoin.
5. ⚠️ Les deux nœuds doivent finir sur la **même version** — une paire en versions différentes, c'est un comportement **non supporté** à terme.

## 105. Split-brain : quand les deux se croient actifs

**Cause** : lien heartbeat coupé → chaque nœud pense que l'autre est mort → les deux deviennent actifs → **conflit d'IP virtuelles**, chaos réseau.

**Prévention** :
- **Double lien heartbeat** (deux interfaces, chemins physiques différents).
- Le heartbeat ne doit **jamais** transiter par un équipement unique partagé.
- 🔧 Détection : `display hrp state verbose` affiche **active** sur les deux → couper le lien de production du « faux actif » en urgence, réparer le heartbeat, resynchroniser.

## 106. Web : supervision de la HA

Chemin (New Web UI) : **Monitor > HA** ou System > HA : état des nœuds, historique des bascules, état du heartbeat.
📋 En routine : vérifier **1 fois par semaine** que le standby est bien standby et synchronisé. Ça prend 30 secondes.

---
---

# BLOC J — LOGS, RAPPORTS, SUPERVISION

## 107. Le log center : où vont les logs

L'USG génère : logs de **politique** (permit/deny), logs **UTM** (AV/IPS/URL), logs **système**, logs **VPN**, logs d'**attaque**.

Destinations possibles :
- **Buffer mémoire** (limité, perdu au reboot).
- **Disque dur local** (si installé — recommandé pour garder l'historique).
- **Serveur syslog** externe (recommandé en prod).
- **eLog** (serveur de logs Huawei, voir section 108).

```huawei
system-view
[USG] info-center enable
[USG] info-center logbuffer size 1024
[USG] info-center syslog level informational
save
```

## 108. eLog : le serveur de logs Huawei

**eLog** = le collecteur/analyseur de logs de Huawei (gratuit dans sa version de base, à vérifier selon version). Il reçoit les logs des USG, les indexe, propose des **tableaux de bord** et des **recherches**.

Alternative : n'importe quel **syslog** (rsyslog, Graylog, Splunk...) reçoit les logs USG en standard. eLog apporte les **dashboards pré-câblés** pour firewall Huawei.

📋 Choix terrain : si tu as 1–2 USG, un syslog classique + Grafana suffit. Si tu as un parc Huawei (firewalls + NCE), eLog/Secomea... (vérifier le nom exact selon génération) centralise mieux.

## 109. Envoyer les logs vers un syslog externe

```huawei
system-view
[USG] info-center loghost 192.168.10.100 facility local7   # IP fictive : ton syslog
[USG] info-center loghost 192.168.10.100 port 514
[USG] info-center source default channel loghost log level informational
save
```

🔧 Tester : générer un log (`ping` bloqué par une règle avec logging) et vérifier sa réception côté serveur. **Sans test, tu découvriras la panne de logs le jour où tu en auras besoin.**

## 110. Logging par règle : quoi logger (et quoi ne pas logger)

```huawei
[USG] security-policy
[USG-policy-security] rule name LAN-web-out
# ... critères ...
[USG-policy-security-rule-LAN-web-out] action permit
# Activer le log :
[USG-policy-security-rule-LAN-web-out] counting enable
[USG-policy-security-rule-LAN-web-out] log enable      # syntaxe à vérifier selon version
[USG-policy-security-rule-LAN-web-out] quit
[USG-policy-security] quit
save
```

Stratégie :
- **Logger** : les `deny` (qui essaie de passer ?), les règles sensibles (DMZ, VPN, admin).
- **Ne PAS logger en détail** : les `permit` à fort volume (LAN→Internet web) — sinon tu noies le syslog. Utilise les **compteurs** (`counting`) + **échantillonnage** ou logs agrégés.
- ⚠️ Un syslog qui reçoit 10 000 logs/seconde = un syslog qui **perd** des logs. Dose.

## 111. Rapports intégrés : ce que l'USG sait faire seul

Avec disque dur : **rapports de trafic, top applications, top utilisateurs, top menaces** — générables depuis le web (Monitor > Reports).
- Planifier un **rapport hebdo PDF** envoyé par mail (selon version).
- 📋 Le rapport hebdo, c'est ton **radar** : une appli inconnue dans le top 10 = enquête immédiate.

## 112. SNMP : supervision dans Zabbix/Centreon/Prometheus

```huawei
system-view
[USG] snmp-agent
[USG] snmp-agent sys-info version v3
[USG] snmp-agent local-user snmp-admin
[USG] snmp-agent local-user snmp-admin password   # mot de passe fictif à saisir
[USG] snmp-agent local-user snmp-admin privilege level 3
[USG] snmp-agent target-host trap address udp-domain 192.168.10.100 params securityname snmp-admin v3
# Autoriser SNMP vers la zone local :
[USG] security-policy
[USG-policy-security] rule name snmp-local
[USG-policy-security-rule-snmp-local] source-zone trust
[USG-policy-security-rule-snmp-local] destination-zone local
[USG-policy-security-rule-snmp-local] source-address 192.168.10.100 32
[USG-policy-security-rule-snmp-local] service snmp
[USG-policy-security-rule-snmp-local] action permit
[USG-policy-security-rule-snmp-local] quit
[USG-policy-security] quit
save
```

Superviser en priorité : **CPU, mémoire, sessions actives, état HA, état des interfaces, état des tunnels VPN, expiration des licences, état des disques**. MIB Huawei : `1.3.6.1.4.1.2011` (l'OID du template USG6620 vu en recherche : `.1.3.6.1.4.1.2011.6.133.55`).

## 113. Traps SNMP : être prévenu au lieu de constater

Configurer les **traps** pour : bascule HA, interface down, CPU > 80 %, mémoire > 85 %, tunnel VPN down, échec de MAJ de signatures, disque en erreur.

```huawei
[USG] snmp-agent trap enable
# Activer les traps par module (syntaxe à vérifier selon version) :
[USG] snmp-agent trap enable feature-name hrp
[USG] snmp-agent trap enable feature-name ipsec
save
```

📋 **Tester chaque trap** après config (provoquer l'événement en maquette ou en heures creuses). Un trap jamais testé = une alerte qui n'arrivera jamais.

## 114. Sondes applicatives : vérifier que « ça marche vraiment »

Au-delà du ping :
- **NQA/SLA** : l'USG peut sonder périodiquement (ping, TCP connect, HTTP GET) vers des cibles critiques (serveur DMZ, DNS, passerelle FAI) et **déclencher des actions** (bascule de route, trap).
- Exemple : si le ping vers 8.8.8.8 via le lien principal échoue 3 fois → bascule sur le lien de secours (voir section 161, double WAN).

```huawei
system-view
[USG] nqa test-instance admin ping-fai
[USG-nqa-admin-ping-fai] test-type icmp
[USG-nqa-admin-ping-fai] destination-address ipv4 8.8.8.8
[USG-nqa-admin-ping-fai] frequency 30
[USG-nqa-admin-ping-fai] probe-count 3
[USG-nqa-admin-ping-fai] start now
[USG-nqa-admin-ping-fai] quit
[USG] display nqa results test-instance admin ping-fai
save
```

---
---

# BLOC K — SAUVEGARDE, FIRMWARE, LICENCES

## 115. Sauvegarde de configuration : les 3 méthodes

**Méthode 1 — CLI + TFTP** (la plus rapide) :
```huawei
[USG] save
[USG] tftp 192.168.10.50 put vrpcfg.cfg USG6000_siteA_20260927.cfg
```

**Méthode 2 — Web** : System > Configuration File Management > Export.

**Méthode 3 — USB** : brancher la clé, web ou CLI :
```huawei
[USG] copy vrpcfg.cfg usb0:/USG6000_siteA_20260927.cfg
```

📋 Nommer : `USG6000_<site>_<date>_<raison>.cfg` (ex. `USG6000_Siege_20260927_avant-MAJ.cfg`). Garder **au moins 3 générations** + la version « sortie de carton » (section 18).

## 116. Restauration : revenir en arrière proprement

```huawei
# Depuis un fichier sur TFTP :
[USG] tftp 192.168.10.50 get USG6000_Siege_20260927_avant-MAJ.cfg vrpcfg.cfg
[USG] startup saved-configuration USG6000_Siege_20260927_avant-MAJ.cfg
[USG] reboot
# Ou appliquer à chaud (DANGEREUX : peut couper l'accès) :
[USG] configuration replace file USG6000_Siege_20260927_avant-MAJ.cfg   # selon version
```

⚠️ La restauration à chaud peut **couper ta session** et le trafic. Préfère le reboot planifié, avec **console à portée** et **quelqu'un sur site** (ou en astreinte joignable).

## 117. Firmware : procédure complète + rollback

**Procédure d'upgrade** (détail de la section 16) :
1. Vérifier compatibilité modèle/version (matrice Huawei).
2. Sauvegarder config (section 115) + noter `display version` et `display license`.
3. Transférer le firmware (FTP/TFTP/web/USB), vérifier le checksum.
4. `startup system-software <fichier>` → `display startup` → `reboot`.
5. Vérifier : version, config intacte, trafic, VPN, HA, licences.

**Rollback** :
```huawei
[USG] display startup              # voir les firmwares disponibles
[USG] startup system-software <ancien-fichier>   # repointer vers l'ancienne version
[USG] reboot
```

💡 L'USG garde généralement **l'ancien firmware** disponible — ne le supprime pas « pour faire de la place » avant d'avoir validé le nouveau en prod pendant **au moins une semaine**.

## 118. Restauration d'urgence : mot de passe perdu

Si plus aucun accès admin :
1. Accès **console physique** obligatoire.
2. Au boot, interrompre pour entrer dans le **BootROM/BootLoad** (touche indiquée à l'écran, souvent `Ctrl+B`).
3. Option **« Clear password » / skip config** (intitulé exact à vérifier selon version) → reboot **sans charger la config**.
4. Reconfigurer le mot de passe, puis **recharger la config sauvegardée** (d'où l'importance de la section 115).
5. ⚠️ Cette procédure **efface la config courante** si mal exécutée — ne la faire qu'avec une sauvegarde valide sous la main.

## 119. Licences UTM : gestion et renouvellement

```huawei
[USG] display license               # état des licences
[USG] display license resource      # ressources couvertes (selon version)
```

- Les licences UTM (AV, IPS, URL, anti-spam) sont **annuelles** : sans renouvellement, les **signatures ne se mettent plus à jour** (la protection se dégrade, le firewall continue de filtrer).
- 📋 **Calendrier** : noter chaque expiration à **J-60** (commande), **J-30** (relance), **J-7** (alerte). Un renouvellement oublié = 3 mois de signatures obsolètes avant de s'en rendre compte.
- Activation : fichier `.dat` via web (System > License > Import) — lier au **numéro de série** (ESN) du boîtier.
- En HA : **une licence par nœud** (section 103).

## 120. Fichiers de signatures : sauvegarde et restauration

Les packs de signatures (AV/IPS/URL) se téléchargent depuis le centre de mise à jour. En cas de restauration :
1. Restaurer la config (section 116).
2. Vérifier les versions de signatures (`display av update info`).
3. Si obsolètes : relancer une MAJ (section 76) — la config référence les **profils**, les signatures se rechargent.

## 121. Inventaire : ce qu'il faut archiver pour chaque USG

📋 Fiche par équipement (dans ton GLPI/gestion de parc) :
- [ ] Modèle, numéro de série (ESN), version logicielle.
- [ ] Plan d'adressage (interfaces, zones, VLAN).
- [ ] Schéma réseau (où il est branché, quoi autour).
- [ ] Config sauvegardée (3 générations).
- [ ] Licences : type, expiration.
- [ ] Mots de passe dans le **coffre** (jamais dans la fiche en clair).
- [ ] Contacts support + contrat de maintenance.
- [ ] Historique des interventions.

## 122. Planning annuel type (récapitulatif, détail section 126)

| Fréquence | Action |
|-----------|--------|
| Quotidien | Coup d'œil supervision (CPU, sessions, VPN) — automatisé |
| Hebdo | Rapport UTM, état HA, échecs d'authentification |
| Mensuel | MAJ signatures (si pas auto), revue des logs d'attaque, backup config |
| Trimestriel | Revue des politiques (règles inutilisées), revue des exceptions UTM, test de restauration |
| Semestriel | Test de bascule HA, revue des licences (J-60) |
| Annuel | Upgrade firmware (si besoin), audit de durcissement, test PRA |

## 123. Upgrade en production : la checklist du jour J

📋 Jour J :
- [ ] Fenêtre de maintenance **validée** et **communiquée**.
- [ ] Sauvegarde config **vérifiée** (le fichier s'ouvre, il est complet).
- [ ] Firmware **vérifié** (checksum OK, bonne version pour le modèle).
- [ ] Console **branchée** et testée.
- [ ] Collègue **prévenu** (binôme en cas de pépin).
- [ ] Plan de rollback **écrit** (pas « dans la tête »).
- [ ] Après reboot : trafic, VPN, HA, licences, supervision — **tout vert** avant de clore.

## 124. Ce qu'il ne faut JAMAIS faire avec un firmware

- ❌ Upgrader **sans sauvegarde**.
- ❌ Upgrader **les deux nœuds HA en même temps**.
- ❌ Couper l'alimentation pendant l'écriture flash.
- ❌ Installer un firmware d'un **autre modèle** (même proche).
- ❌ Upgrader un vendredi 17h sans astreinte.
- ❌ Supprimer l'ancien firmware avant **une semaine** de validation.

---
---

# BLOC L — MAINTENANCE PRÉVENTIVE

## 125. Philosophie : le firewall qu'on oublie est celui qui tombe

Un USG bien configuré tourne des mois sans broncher — c'est exactement pour ça qu'on l'oublie, et qu'on découvre la panne (disque plein, licence expirée, ventilateur mort) **le jour où on a besoin des logs**. La maintenance préventive, c'est **rendre l'oubli impossible** : checks automatisés + rituels calendaires.

## 126. Plan de maintenance détaillé

| Fréquence | Tâches | Commandes/outils |
|-----------|--------|------------------|
| **Quotidien** (auto) | CPU, mémoire, sessions, état VPN/HA, traps | SNMP + supervision |
| **Hebdomadaire** | Rapport UTM, échecs auth, état standby HA | Web Monitor, `display hrp state` |
| **Mensuel** | Backup config, MAJ signatures (si manuel), espace disque, revue logs d'attaque | Sections 115, 76, `display disk` |
| **Trimestriel** | Règles inutilisées, exceptions UTM, test restauration (maquette), poussière/ventilation | `display security-policy`, section 116 |
| **Semestriel** | Test bascule HA, licences J-60, firmware (évaluer) | Section 101, `display license` |
| **Annuel** | Audit durcissement, test PRA complet, remplacement préventif (ventilos 4 ans, disques 3-5 ans) | Section 133+ |

## 127. Checklist mensuelle (imprimable)

📋 Maintenance mensuelle USG6000 — Site : _______ Date : _______
- [ ] `display cpu` / `display memory` — valeurs normales (< 70 % en pointe) ?
- [ ] `display firewall session table` — pas d'anomalie (sessions fantômes) ?
- [ ] `display version` — toujours la version attendue ?
- [ ] Signatures AV/IPS/URL à jour (< 7 jours) ?
- [ ] `display license` — aucune expiration < 90 jours ?
- [ ] HA : `display hrp state verbose` — standby synchronisé ?
- [ ] VPN : `display ike sa` — tunnels attendus montés ?
- [ ] Disque : espace libre > 20 % ?
- [ ] Backup config effectué et **vérifié** ?
- [ ] Logs : pas d'attaque massive non traitée ?
- [ ] Baie : voyants verts, pas de bruit anormal, température OK ?

## 128. Nettoyage des politiques : la règle des 90 jours

```huawei
[USG] display security-policy rule all   # repérer les règles à 0 hit
```

- Toute règle avec **0 hit depuis 90 jours** = candidate à la **désactivation** (pas suppression immédiate : désactiver 30 jours, puis supprimer si personne ne pleure).
- 📋 Documenter chaque suppression (date, raison, demandeur). Une règle « inutile » qui servait au PRA annuel, ça fait désordre de la réécrire en urgence.

## 129. Santé matérielle : ventilateurs, alimentations, disques

```huawei
[USG] display device            # état des modules
[USG] display fan               # ventilateurs (selon version)
[USG] display power             # alimentations (selon version)
[USG] display disk              # disques et espace (selon version)
[USG] display temperature       # températures (selon version)
[USG] display alarm              # alarmes actives
```

Seuils d'alerte terrain : ventilateur en défaut = **remplacement sous 48h** (un seul ventilo en moins = surchauffe en été) ; disque en erreur = **remplacement + resync RAID1** ; alim redondante en défaut = **commander le spare immédiatement** (tu n'as plus de redondance).

## 130. Dépoussiérage et environnement

- **Trimestriel** : dépoussiérage des façades/filtres (air comprimé sec, boîtier **éteint** si ouverture nécessaire — sinon par l'extérieur).
- **Température de baie** : 20–25 °C idéal, alarme au-delà de 35 °C.
- ⚠️ **Ne jamais** poser de carton/dossier devant les grilles d'aération. Vu en vrai. Plusieurs fois.

## 131. Revue des logs d'attaque : le rituel hebdo

1. Ouvrir le rapport hebdo (section 111) ou les logs UTM.
2. Chercher : **nouveaux top talkers**, **pics d'attaques**, **pays inhabituels**, **tentatives sur les comptes admin**.
3. Toute anomalie → **ticket/enquête** (pas « on verra la semaine prochaine »).
4. 📋 Tenir un **journal des incidents** (même les petits) — c'est lui qui justifie les budgets sécu.

## 132. Plan de Reprise d'Activité (PRA) : le firewall dedans

- Le PRA inclut : **config sauvegardée hors site**, **firmware de référence** archivé, **licences** (fichiers .dat), **procédure de reconstruction** (ce guide, sections 11–18), **boîtier de secours** (si contrat) ou **délai fournisseur**.
- 📋 **Tester 1 fois par an** : reconstruire un USG de maquette depuis la sauvegarde. Un backup jamais restauré = un espoir, pas un plan.

---
---

# BLOC M — DURCISSEMENT

## 133. Durcir l'accès management

```huawei
system-view
# 1. HTTPS uniquement (désactiver HTTP) :
[USG] web-manager enable port 8443            # port non standard (optionnel)
# Désactiver le HTTP clair via la config des services de management par interface
# 2. SSH uniquement (désactiver Telnet) :
[USG] undo telnet server enable
[USG] ssh server enable
[USG] ssh server port 2222                    # port non standard (optionnel, à documenter !)
# 3. Restreindre les sources d'administration :
[USG] acl 2000
[USG-acl-basic-2000] rule permit source 192.168.10.0 0.0.0.255   # VLAN admin uniquement
[USG-acl-basic-2000] quit
[USG] ssh server acl 2000
# 4. Comptes nominatifs (pas de compte générique partagé) - voir section 13
# 5. Timeout de session :
[USG] idle-timeout 10                         # minutes (console/web selon version)
save
```

📋 Règles d'or : **pas de Telnet**, **pas de HTTP**, **pas d'admin depuis Internet** (ou via VPN uniquement), **comptes nominatifs**, **double authentification** si possible.

## 134. Politique de mot de passe

- Longueur **≥ 12 caractères**, complexité (4 familles), **historique** (pas de réutilisation des 5 derniers), **durée de vie** 90 jours pour les comptes à privilèges.
- ⚠️ **Pas de mot de passe dans les scripts en clair**, pas de post-it, pas de « mot de passe d'équipe » partagé par 8 personnes.
- Comptes de **secours** : 1 compte « break-glass » local, mot de passe **sous enveloppe scellée** dans le coffre, **testé 1 fois par an**.

## 135. Protection Anti-DDoS / anti-attaque

L'USG6000 intègre des protections contre : **SYN flood, UDP flood, ICMP flood, scans, attaques par zone**.

```huawei
system-view
# Activer la détection d'attaque par zone (exemple untrust) :
[USG] firewall defend syn-flood enable zone untrust
[USG] firewall defend udp-flood enable zone untrust
# Seuils (à ajuster selon ton trafic normal - mesurer AVANT d'activer le blocage) :
[USG] firewall defend syn-flood threshold 10000 zone untrust   # paquets/s (exemple)
# Liste noire automatique :
[USG] firewall blacklist enable
save
```

⚠️ **Méthodologie** : 1) mesurer le trafic normal (pics), 2) activer en **détection seule**, 3) régler les seuils **au-dessus** des pics légitimes, 4) passer en **blocage**. Un seuil trop bas = **auto-DDoS** (tu bloques tes propres utilisateurs).

## 136. Filtrage géographique (si pertinent)

Si ton activité n'a aucune raison de recevoir du trafic de certains pays : blocage par **région/pays** dans les politiques (untrust→dmz, untrust→local).
⚠️ À manier avec prudence : CDN et services cloud ont des IP dans le monde entier — un blocage pays trop large **casse** des services légitimes (mises à jour, SaaS). Tester avant.

## 137. Désactiver les services inutiles

```huawei
# Inventaire des services actifs :
[USG] display tcp status
[USG] display udp status      # (selon version)
# Désactiver ce qui ne sert pas : telnet (133), ftp server, snmp v1/v2c si v3 en place...
[USG] undo ftp server enable
save
```

📋 Principe : **tout service actif est une surface d'attaque**. Si tu ne sais pas à quoi sert un port en écoute, cherche — puis coupe.

## 138. Journalisation des actions d'administration

- Activer les **logs de configuration** (qui a tapé quoi, quand) — `info-center` + envoi syslog.
- En HA/pilotage centralisé, les changements doivent passer par **demande → validation → application → vérification** (même à deux, même « vite fait »).
- 📋 Le « vite fait » non tracé, c'est la cause n°1 des « mais qui a touché à ça ?! ».

## 139. Sécurité des sauvegardes

- Les fichiers de config contiennent des **hashes de mots de passe** et des **PSK** : les stocker **chiffrés** (dossier chiffré, coffre), **jamais** sur un partage ouvert ni par mail.
- Droits d'accès restreints, **journal des accès** aux sauvegardes.

## 140. Mises à jour de sécurité : veille

- S'abonner aux **bulletins de sécurité Huawei** (alertes CVE).
- Criticité **critique** sur un firewall exposé = **patch sous 30 jours** (ou mesure compensatoire immédiate : règle de blocage, désactivation du service vulnérable).
- 📋 Noter chaque CVE applicable dans le journal (section 131) avec la décision (patché / compensé / non concerné + pourquoi).

## 141. Segmentation interne : ne pas s'arrêter au périmètre

Le firewall périmétrique ne protège pas contre : un poste infecté qui attaque le LAN, un prestataire branché en salle serveur, un IoT vérolé.
→ **Zones internes** (section 21 : IOT, GUEST, PROD), **politiques inter-VLAN** sur l'USG ou les switchs, **802.1X** sur les accès filaires sensibles. La défense en profondeur, pas en coquille d'œuf.

## 142. Audit de durcissement : grille annuelle

📋 Grille d'audit (1 fois/an) :
- [ ] Comptes : nominatifs ? anciens comptes supprimés ? break-glass testé ?
- [ ] Mots de passe : politique appliquée ? pas de défaut restant ?
- [ ] Accès : Telnet/HTTP désactivés ? sources restreintes ? pas d'admin depuis untrust ?
- [ ] Politiques : default deny ? règles 0-hit nettoyées ? DMZ→trust en deny ?
- [ ] UTM : signatures à jour ? profils actifs sur les bonnes règles ? exceptions revues ?
- [ ] VPN : PSK robustes ? certificats valides ? comptes VPN obsolètes supprimés ?
- [ ] HA : testée ? licences à jour des deux côtés ?
- [ ] Logs : syslog reçu ? rétention conforme ? rapports lus ?
- [ ] Firmware : version supportée ? CVE en cours traitées ?
- [ ] Physique : baie verrouillée ? console non branchée en permanence ?

---
---

# BLOC N — DÉPANNAGE TERRAIN (18 CAS)

> Méthode générale : **1)** qualifier (quoi, qui, quand, depuis quand), **2)** reproduire à la demande, **3)** observer (`display`, `debugging` ciblé), **4)** hypothèse unique, **5)** correction, **6)** vérification + traçabilité. Ne change jamais deux choses à la fois.

## 143. Méthode : l'ordre de traversée d'un paquet (à connaître par cœur)

```
Entrée interface → Zone source → [Session existante ? oui → forward]
                                   non → NAT destination (DNAT)
                                       → Routage (zone destination)
                                       → Politique de sécurité (1ère règle qui matche)
                                       → NAT source (SNAT)
                                       → UTM (profils de la règle)
                                       → Sortie interface
```

🔧 Quand « ça ne passe pas », remonte cette chaîne **dans l'ordre** : c'est presque toujours l'une de ces étapes. Les commandes reines : `display firewall session table`, `display security-policy`, `display nat-policy`, `display ip routing-table`, `debugging packet-filter`.

## 144. Cas 1 : « Rien ne passe » après une modification

- **Symptômes** : tout le trafic coupé (ou tout un sens) juste après un changement.
- **Diagnostic** : `display configuration` / comparer avec la sauvegarde ; `display security-policy rule all` (ordre), `display zone` (interface toujours dans sa zone ?).
- **Causes fréquentes** : `default action deny` activé sans règles de rattrapage ; interface **retirée de sa zone** par erreur ; `save` oublié puis reboot (l'ancienne config est revenue — ou l'inverse).
- **Solution** : restaurer la sauvegarde (section 116) ou corriger la modification. **Toujours** faire `display` AVANT/APRÈS chaque changement.

## 145. Cas 2 : une politique bloque un flux légitime

- **Symptômes** : une appli ne marche plus (ou n'a jamais marché) à travers l'USG.
- **Diagnostic** :
```huawei
[USG] display firewall session table source inside 192.168.10.25 destination outside 203.0.113.50
# Pas de session ? → bloqué avant création. Session en deny ?
[USG] display security-policy rule all   # quelle règle matche ? (ordre !)
# Test ciblé : autoriser temporairement en LOG pour voir :
# puis debugging :
[USG] debugging packet-filter ip source 192.168.10.25   # syntaxe à vérifier selon version
[USG] undo debugging all
```
- **Causes fréquentes** : règle trop restrictive (port oublié), **mauvais ordre** (un deny général avant le permit), objet d'adresse pas à jour, **plage horaire** active, critère `user` sans authentification.
- **Solution** : corriger la règle (ou son ordre avec `move rule`), re-tester, **retirer le debugging**.

## 146. Cas 3 : le retour ne passe pas (routage asymétrique / session)

- **Symptômes** : le SYN part, le SYN-ACK ne revient pas (ou l'inverse) ; applis qui « se connectent puis figent ».
- **Diagnostic** : `display firewall session table` → session **incomplète** (un seul sens) ; vérifier les **routes retour** (`display ip routing-table` des deux côtés).
- **Causes** : route de retour qui **ne repasse pas par l'USG** (le firewall ne voit qu'un sens → droppe, normal pour un stateful) ; **deux chemins** (lien principal + secours) avec routage asymétrique.
- **Solution** : corriger le routage pour la **symétrie** ; en dernier recours, ajuster les timeouts/options de session (à éviter — masque le problème).

## 147. Cas 4 : le VPN IPSec ne monte pas (phase 1)

- **Symptômes** : `display ike sa` vide.
- **Diagnostic par étapes** :
  1. `ping` entre les IP publiques → si KO : réseau ou politique local (ICMP).
  2. Politique **untrust→local** : UDP 500, UDP 4500, protocole ESP autorisés ?
  3. `debugging ike packet` → regarder où ça bloque : pas de réponse du tout (réseau/ACL) ou **payload mismatch** (propositions IKE différentes).
  4. Vérifier : **PSK identique** (la faute de frappe n°1 — la re-saisir des deux côtés), **proposals** compatibles, **remote-address** correcte, **heure** synchronisée.
- **Solution** : corriger le paramètre divergent, `reset ike sa` / `reset ipsec sa` pour forcer la renégociation proprement.

## 148. Cas 5 : phase 1 OK, phase 2 KO (« le tunnel monte mais rien ne passe »)

- **Symptômes** : `display ike sa` = RD, mais `display ipsec sa` vide ou sans trafic.
- **Diagnostic** : comparer les **ACL** des deux côtés (miroir exact ?), les **transform-set** (algorithmes compatibles ?), le **PFS** (activé d'un seul côté = échec).
- **Autres causes** : **NAT** qui attrape le trafic VPN (exemption `no-nat` manquante ou mal ordonnée — section 52) ; **politique de sécurité** qui bloque le trafic clair dans le tunnel.
- **Solution** : aligner ACL/transform-set/PFS, placer l'exemption NAT **avant** la règle NAT, autoriser le trafic dans la politique.

## 149. Cas 6 : tunnel IPSec instable (flapping)

- **Symptômes** : le tunnel tombe et remonte toutes les X minutes/heures.
- **Diagnostic** : corréler les coupures avec les **lifetimes** (renégociation qui échoue), vérifier le **DPD** (actif ?), regarder les logs des deux côtés à l'heure exacte.
- **Causes fréquentes** : DPD non configuré (pair « fantôme ») ; **double NAT** devant un site (NAT-T mal géré) ; lifetimes incompatibles ; **instabilité du lien** (perte de paquets — IKE n'aime pas).
- **Solution** : activer le DPD (section 62), aligner les lifetimes, stabiliser le lien (QoS, changement de FAI si récurrent).

## 150. Cas 7 : le portail SSL VPN ne s'ouvre pas

- **Symptômes** : `https://<ip-publique>` ne répond pas / timeout.
- **Diagnostic** :
  1. La **passerelle SSL VPN** est-elle démarrée ? (`display sslvpn gateway`)
  2. Politique **untrust→local** : port 443 (ou port configuré) autorisé ?
  3. Le port est-il en **conflit** (le web d'admin utilise aussi le 443 ? → changer l'un des deux) ?
  4. Test depuis l'intérieur (bypass la politique untrust) pour isoler.
- **Solution** : démarrer la passerelle, corriger la politique, changer le port (ex. 4433) et **documenter**.

## 151. Cas 8 : SSL VPN connecté mais aucun accès réseau

- **Symptômes** : login OK, portail OK, mais les ressources internes injoignables.
- **Diagnostic** : depuis quelle **zone** arrive le trafic VPN (souvent une zone dédiée ou trust) ? La **politique** autorise-t-elle zone-VPN→trust/dmz ? Le client a-t-il reçu une **IP du pool** (conflit avec le LAN ?) ? Les **routes** côté client (split tunneling : les routes sont-elles poussées ?).
- **Solution** : créer les politiques manquantes, vérifier le pool d'adresses (pas de chevauchement), vérifier le routage client.

## 152. Cas 9 : débit effondré depuis l'activation de l'UTM

- **Symptômes** : tout marchait, depuis l'activation AV/IPS c'est lent (ou ça timeout).
- **Diagnostic** : `display cpu` (un cœur à 100 % ?), `display memory`, comparer **avant/après** désactivation temporaire d'un profil (en heures creuses, avec accord).
- **Causes** : modèle **sous-dimensionné** pour l'UTM (section 73 — le cas classique) ; profil appliqué sur des règles à **gros volume** (backup, vidéosurveillance) ; **inspection SSL** sur tout le trafic.
- **Solution** : restreindre les profils aux règles utiles (matrice section 84), exclure les flux volumineux non à risque, **upgrader de modèle** si structurel (c'est un dimensionnement, pas un bug).

## 153. Cas 10 : l'IPS/AV bloque une application métier (faux positif)

- **Symptômes** : une appli interne/outil métier bloqué depuis une MAJ de signatures.
- **Diagnostic** : logs UTM → **signature ID** en cause ; confirmer que c'est bien un faux positif (l'appli est légitime, le comportement est normal).
- **Solution** : **exception ciblée** (désactiver la signature pour cette source/destination, ou whitelist), **jamais** désactiver tout le profil. Documenter (section 86), **revue trimestrielle**. Remonter le faux positif à Huawei si récurrent.

## 154. Cas 11 : session asymétrique en HA après bascule

- **Symptômes** : après bascule HA, certaines connexions tombent (surtout les longues : SSH, RDP, visio).
- **Diagnostic** : comparer `display firewall session table` sur les deux nœuds **avant** bascule (tables quasi identiques ?) ; `display hrp sync-status`.
- **Causes** : **synchro HRP** incomplète ou désactivée ; câblage **asymétrique** ; bascule pendant un pic (synchro en retard).
- **Solution** : activer/vérifier la synchro auto (section 102), corriger le câblage, re-tester la bascule (section 101).

## 155. Cas 12 : « Internet coupé » — le NAT ne traduit plus

- **Symptômes** : plus aucun accès Internet depuis le LAN, le reste (local, DMZ) OK.
- **Diagnostic** : `display nat-policy statistics` (compteurs à 0 ?), l'**IP de l'interface WAN** a-t-elle changé (DHCP/PPPoE du FAI ?) — avec Easy IP c'est transparent, mais vérifier ; le **pool NAT** est-il épuisé (`display nat address-group`) ?
- **Causes fréquentes** : FAI qui a changé d'IP et **route par défaut** tombée ; pool NAT **saturé** (trop petit pour le nombre de sessions) ; règle NAT **désactivée** par erreur.
- **Solution** : vérifier IP/route WAN, agrandir le pool ou passer en PAT, réactiver la règle.

## 156. Cas 13 : DNS qui ne résout plus à travers le firewall

- **Symptômes** : « Internet ne marche pas » mais les IP passent (ping 8.8.8.8 OK, google.com KO).
- **Diagnostic** : `nslookup` depuis un poste → timeout ? Politique : le **DNS (UDP/TCP 53)** est-il autorisé trust→untrust (section 35, règle 1 — l'oubli classique) ?
- **Solution** : ajouter la règle DNS. 💡 **Toujours** tester un accès Internet **par IP et par nom** pour distinguer « réseau » de « DNS ».

## 157. Cas 14 : FTP/SIP qui « marche à moitié »

- **Symptômes** : FTP : connexion OK, `ls` OK, transfert KO. SIP : enregistrement OK, pas d'audio.
- **Diagnostic** : ALG activés ? (`display firewall alg`) ; politique : les **ports de données** (FTP passif : plage du serveur ; SIP/RTP : UDP 10000-20000) sont-ils autorisés ?
- **Solution** : activer l'ALG (section 53), ouvrir la plage de ports de données (restreinte aux IP des serveurs), préférer le **mode passif** FTP.

## 158. Cas 15 : boucle / tempête après un mauvais câblage

- **Symptômes** : CPU à 100 %, réseau saturé, tout rame — juste après un brassage.
- **Diagnostic** : `display cpu`, `display interface` (compteurs d'erreurs qui explosent), vérifier physiquement : **deux câbles** entre les mêmes switchs ? Un câble en **boucle** sur le même équipement ?
- **Solution** : débrancher le câble fautif, vérifier le spanning-tree des switchs. 📋 **Étiqueter les câbles** (section 11) — ça évite 80 % de ces incidents.

## 159. Cas 16 : authentification AD qui échoue en masse

- **Symptômes** : plus personne ne s'authentifie (VPN, portail).
- **Diagnostic** : `ping` vers l'AD, port **389/636** ouvert ? **Compte de bind** : mot de passe expiré (le classique !) ? **Heure** : désynchro > 5 min = échec Kerberos. `display aaa online-fail-record`.
- **Solution** : renouveler le mot de passe du compte de service (avec **rappel calendaire** avant la prochaine expiration), resynchroniser NTP, basculer temporairement sur la **base locale de secours** si besoin.

## 160. Cas 17 : logs qui n'arrivent plus au syslog

- **Symptômes** : le serveur syslog ne reçoit plus rien (découvert pendant une enquête, évidemment).
- **Diagnostic** : `ping` vers le syslog depuis l'USG ; politique **local→untrust/trust** : UDP 514 autorisé ? Le **disque** du syslog est-il plein ? La config `info-center loghost` est-elle toujours là (`display current-configuration | include loghost`) ?
- **Solution** : corriger réseau/politique/espace disque. 📋 **Superviser la réception** des logs (alerte si 0 log reçu en 15 min) — sinon tu ne le sauras jamais.

## 161. Cas 18 : lenteurs intermittentes (le pire)

- **Symptômes** : « parfois c'est lent », non reproductible à la demande.
- **Méthode** : ne pas bricoler — **mesurer d'abord** :
  1. Historique supervision : CPU/mémoire/sessions aux heures des plaintes.
  2. `display firewall session table` en pointe : **saturation** (table pleine ?) ?
  3. Corrélations : backup à 14h ? MAJ signatures ? Scan antivirus des postes ?
  4. Tester **en bypass** (si possible) pour isoler le firewall du reste.
- **Causes typiques** : saturation de sessions (un poste vérolé qui spamme), lien FAI saturé (pas le firewall !), UTM en pointe, **boucle partielle**.
- **Solution** : traiter la cause mesurée, pas le symptôme. Documenter.

## 162. Kit de survie : les 10 commandes à connaître par cœur

```huawei
display version                          # version logicielle
display current-configuration            # config complète
display ip routing-table                 # routage
display zone                             # zones et interfaces
display security-policy rule all         # politiques dans l'ordre
display nat-policy rule all              # NAT dans l'ordre
display firewall session table           # sessions actives
display cpu / display memory             # ressources
display ike sa / display ipsec sa        # VPN
display hrp state verbose                # HA
display logbuffer                        # logs récents
display alarm                            # alarmes matérielles
debugging packet-filter ... / undo debugging all   # trace ciblée (à couper !)
```

---
---

# BLOC O — CAS PRATIQUES COMPLETS COMMENTÉS

## 163. Cas pratique 1 : PME 80 postes, 1 lien fibre — configuration complète

**Contexte** : siège PME, 80 postes (192.168.10.0/24), 1 serveur web vitrine + 1 serveur mail en DMZ (172.16.1.0/24), lien fibre 1 Gbit/s (IP publique fixe 203.0.113.1), 10 nomades en SSL VPN. Modèle : **USG6380** (6 Gbit/s firewall — dimensionné pour ~1 Gbit/s UTM, voir section 73).

```huawei
system-view
# ===== Interfaces et zones =====
[USG] interface GigabitEthernet 1/0/1
[USG-GigabitEthernet1/0/1] description LIEN-FIBRE-FAI
[USG-GigabitEthernet1/0/1] ip address 203.0.113.1 30
[USG-GigabitEthernet1/0/1] quit
[USG] interface GigabitEthernet 1/0/2
[USG-GigabitEthernet1/0/2] description DMZ-SERVERS
[USG-GigabitEthernet1/0/2] ip address 172.16.1.1 24
[USG-GigabitEthernet1/0/2] quit
[USG] interface GigabitEthernet 1/0/3
[USG-GigabitEthernet1/0/3] description LAN-BUREAUTIQUE
[USG-GigabitEthernet1/0/3] ip address 192.168.10.1 24
[USG-GigabitEthernet1/0/3] quit
[USG] firewall zone untrust
[USG-zone-untrust] set priority 5
[USG-zone-untrust] add interface GigabitEthernet 1/0/1
[USG-zone-untrust] quit
[USG] firewall zone dmz
[USG-zone-dmz] set priority 50
[USG-zone-dmz] add interface GigabitEthernet 1/0/2
[USG-zone-dmz] quit
[USG] firewall zone trust
[USG-zone-trust] set priority 85
[USG-zone-trust] add interface GigabitEthernet 1/0/3
[USG-zone-trust] quit
# ===== Routage =====
[USG] ip route-static 0.0.0.0 0.0.0.0 203.0.113.2    # passerelle FAI (fictive)
# ===== NAT source (Easy IP) =====
[USG] nat-policy
[USG-policy-nat] rule name nat-lan
[USG-policy-nat-rule-nat-lan] source-zone trust
[USG-policy-nat-rule-nat-lan] destination-zone untrust
[USG-policy-nat-rule-nat-lan] source-address 192.168.10.0 24
[USG-policy-nat-rule-nat-lan] action nat easy-ip
[USG-policy-nat-rule-nat-lan] quit
[USG-policy-nat] quit
# ===== NAT destination (serveurs publiés) =====
[USG] nat-policy
[USG-policy-nat] rule name dnat-web
[USG-policy-nat-rule-dnat-web] source-zone untrust
[USG-policy-nat-rule-dnat-web] destination-zone dmz
[USG-policy-nat-rule-dnat-web] destination-address 203.0.113.10 32
[USG-policy-nat-rule-dnat-web] service http
[USG-policy-nat-rule-dnat-web] action nat static
[USG-policy-nat-rule-dnat-web] static-to 172.16.1.10
[USG-policy-nat-rule-dnat-web] quit
[USG-policy-nat] rule name dnat-mail
[USG-policy-nat-rule-dnat-mail] source-zone untrust
[USG-policy-nat-rule-dnat-mail] destination-zone dmz
[USG-policy-nat-rule-dnat-mail] destination-address 203.0.113.11 32
[USG-policy-nat-rule-dnat-mail] service smtp
[USG-policy-nat-rule-dnat-mail] action nat static
[USG-policy-nat-rule-dnat-mail] static-to 172.16.1.25
[USG-policy-nat-rule-dnat-mail] quit
[USG-policy-nat] quit
# ===== Politiques (extraits, voir sections 35-40 pour le détail) =====
[USG] security-policy
[USG-policy-security] default action deny
[USG-policy-security] quit
# ... ajouter les règles LAN→Internet, untrust→DMZ, protections local (sections 35-40) ...
save
```

📋 **Recette de mise en service** : ping LAN→firewall, DNS, web, mail entrant/sortant, EICAR (section 85), test depuis un mobile en 4G (vue « extérieure »), backup config.

## 164. Cas pratique 2 : site industriel avec 2 liens WAN (principal + secours 4G)

**Contexte** : site de production, coupure Internet = arrêt de la supervision. Lien principal fibre + **secours 4G** (clé USB/routeur 4G).

```huawei
system-view
# Deux interfaces WAN dans untrust :
[USG] interface GigabitEthernet 1/0/1
[USG-GigabitEthernet1/0/1] description WAN-FIBRE-PRINCIPAL
[USG-GigabitEthernet1/0/1] ip address 203.0.113.1 30
[USG-GigabitEthernet1/0/1] quit
[USG] interface GigabitEthernet 1/0/4
[USG-GigabitEthernet1/0/4] description WAN-4G-SECOURS
[USG-GigabitEthernet1/0/4] dhcp select interface      # IP via DHCP du routeur 4G
[USG-GigabitEthernet1/0/4] quit
[USG] firewall zone untrust
[USG-zone-untrust] add interface GigabitEthernet 1/0/1
[USG-zone-untrust] add interface GigabitEthernet 1/0/4
[USG-zone-untrust] quit
# Routes avec préférences : principal = 60 (défaut), secours = 100 :
[USG] ip route-static 0.0.0.0 0.0.0.0 203.0.113.2
[USG] ip route-static 0.0.0.0 0.0.0.0 GigabitEthernet 1/0/4 preference 100
# NAT Easy IP sur les deux sorties (egress-interface) :
[USG] nat-policy
[USG-policy-nat] rule name nat-wan1
[USG-policy-nat-rule-nat-wan1] source-zone trust
[USG-policy-nat-rule-nat-wan1] destination-zone untrust
[USG-policy-nat-rule-nat-wan1] egress-interface GigabitEthernet 1/0/1
[USG-policy-nat-rule-nat-wan1] source-address 192.168.10.0 24
[USG-policy-nat-rule-nat-wan1] action nat easy-ip
[USG-policy-nat-rule-nat-wan1] quit
[USG-policy-nat] rule name nat-wan2
[USG-policy-nat-rule-nat-wan2] source-zone trust
[USG-policy-nat-rule-nat-wan2] destination-zone untrust
[USG-policy-nat-rule-nat-wan2] egress-interface GigabitEthernet 1/0/4
[USG-policy-nat-rule-nat-wan2] source-address 192.168.10.0 24
[USG-policy-nat-rule-nat-wan2] action nat easy-ip
[USG-policy-nat-rule-nat-wan2] quit
[USG-policy-nat] quit
save
```

💡 Pour une **bascule intelligente** (pas seulement « si l'interface tombe »), couple avec **NQA** (section 114) : si le ping via la fibre échoue, la route est retirée → bascule sur la 4G même si le lien physique reste UP (cas du FAI « UP mais noir »).

## 165. Cas pratique 3 : interconnexion de 3 sites en IPSec (hub-and-spoke)

**Contexte** : siège (hub) + 2 agences (spokes). Chaque agence monte un tunnel vers le siège ; le trafic inter-agences passe par le hub.

- Sur chaque spoke : config IPSec standard vers le hub (section 59).
- Sur le hub : **2 jeux** de config (peer/proposal/policy/ACL) — un par spoke.
- ⚠️ **Pas de NAT** entre les réseaux des tunnels (exemptions `no-nat` sur chaque nœud).
- Routage : routes statiques vers les LAN distants via les interfaces Tunnel (ou OSPF, section 71).
- 📋 **Documenter** : tableau des PSK (dans le coffre !), IP publiques, LAN, n° de politique — un hub-and-spoke mal documenté devient ingérable à 5 sites.

## 166. Cas pratique 4 : Wi-Fi invités isolé avec portail

**Contexte** : invités + prestataires, accès Internet uniquement, **aucun accès** au LAN/DMZ.

```huawei
system-view
[USG] interface GigabitEthernet 1/0/5.100
[USG-GigabitEthernet1/0/5.100] vlan-type dot1q 100
[USG-GigabitEthernet1/0/5.100] ip address 192.168.100.1 24
[USG-GigabitEthernet1/0/5.100] quit
[USG] firewall zone name GUEST
[USG-zone-guest] set priority 20
[USG-zone-guest] add interface GigabitEthernet 1/0/5.100
[USG-zone-guest] quit
# DHCP pour les invités :
[USG] dhcp enable
[USG] interface GigabitEthernet 1/0/5.100
[USG-GigabitEthernet1/0/5.100] dhcp select interface
[USG-GigabitEthernet1/0/5.100] dhcp server ip-range 192.168.100.10 192.168.100.200
[USG-GigabitEthernet1/0/5.100] dhcp server gateway-list 192.168.100.1
[USG-GigabitEthernet1/0/5.100] dhcp server dns-list 8.8.8.8
[USG-GigabitEthernet1/0/5.100] quit
[USG] security-policy
# GUEST→Internet : web + DNS + UTM stricte :
[USG-policy-security] rule name guest-internet
[USG-policy-security-rule-guest-internet] source-zone GUEST
[USG-policy-security-rule-guest-internet] destination-zone untrust
[USG-policy-security-rule-guest-internet] service http https dns
[USG-policy-security-rule-guest-internet] profile url-profile-guest
[USG-policy-security-rule-guest-internet] profile av-profile-lan
[USG-policy-security-rule-guest-internet] action permit
[USG-policy-security-rule-guest-internet] quit
# GUEST→trust/dmz : DENY explicite (avec default deny c'est redondant, mais explicite = lisible) :
[USG-policy-security] rule name guest-deny-interne
[USG-policy-security-rule-guest-deny-interne] source-zone GUEST
[USG-policy-security-rule-guest-deny-interne] destination-zone trust
[USG-policy-security-rule-guest-deny-interne] action deny
[USG-policy-security-rule-guest-deny-interne] quit
[USG-policy-security] quit
# NAT pour les invités :
[USG] nat-policy
[USG-policy-nat] rule name nat-guest
[USG-policy-nat-rule-nat-guest] source-zone GUEST
[USG-policy-nat-rule-nat-guest] destination-zone untrust
[USG-policy-nat-rule-nat-guest] source-address 192.168.100.0 24
[USG-policy-nat-rule-nat-guest] action nat easy-ip
[USG-policy-nat-rule-nat-guest] quit
[USG-policy-nat] quit
save
```

## 167. Cas pratique 5 : paire HA pour un site critique (synthèse)

Reprendre : sections 97–106 pour la config, 101 pour le test, 104 pour l'upgrade.
📋 **Spécificités à ne pas oublier** : licences **identiques** des deux côtés, certificats SSL VPN **installés des deux côtés**, supervision des **deux** nœuds (pas seulement l'actif), **test de bascule semestriel** au planning.

## 168. Cas pratique 6 : migration d'un ancien firewall vers l'USG6000

1. **Inventaire** de l'existant : règles, NAT, VPN, routes — tout, même les règles « temporaires » de 2019.
2. **Traduction** : chaque règle → couple de zones + critères USG. C'est le moment de **nettoyer** (règles 0-hit = on ne migre pas).
3. **Maquette** : monter l'USG en labo, rejouer les flux critiques.
4. **Bascule** : en fenêtre de maintenance, avec **rollback** (rebrancher l'ancien = 5 minutes si le câblage est prêt).
5. **Recette** : checklist section 163 + tests utilisateurs pilotes.
6. 📋 Garder l'ancien firewall **1 mois** sous la main (éteint, config sauvegardée) avant recyclage.

---
---

# BLOC P — ERREURS CLASSIQUES À NE PAS COMMETTRE

## 169. Erreur 1 : `default action permit` oublié en prod

Mettre `permit` « pour les tests » et oublier de repasser en `deny`. **Conséquence** : firewall passoire pendant des mois. **Prévention** : ne JAMAIS mettre permit en prod, même temporairement — à la place, ajouter une règle `permit` large **avec une date de fin** (time-range) et une alerte.

## 170. Erreur 2 : l'interface sans zone

Interface UP, IP configurée, mais **jamais ajoutée** à une zone → aucun trafic. **Réflexe** : `display zone` dès que « l'interface est UP mais rien ne passe » (section 24).

## 171. Erreur 3 : le NAT sans la politique (ou l'inverse)

DNAT configuré mais pas de règle security-policy → le paquet traduit est **droppé**. **Réflexe** : tout DNAT s'accompagne **toujours** d'une règle de politique (section 48).

## 172. Erreur 4 : l'exemption NAT oubliée pour le VPN

Le NAT easy-ip attrape aussi le trafic du tunnel → VPN « monté mais muet ». **Réflexe** : `no-nat` **avant** la règle NAT, pour chaque réseau distant (section 52).

## 173. Erreur 5 : la PSK avec une faute de frappe

Deux côtés « identiques » sauf un caractère → phase 1 impossible, logs cryptiques. **Prévention** : copier-coller depuis le coffre, **jamais** de saisie manuelle, re-saisir des deux côtés au moindre doute (section 147).

## 174. Erreur 6 : UTM activée partout, modèle sous-dimensionné

Activer AV+IPS+URL+SSL-inspection sur toutes les règles d'un USG6330 avec 1 Gbit/s de trafic → **effondrement**. **Prévention** : dimensionner sur le **débit UTM** (section 73), appliquer les profils **de façon ciblée** (matrice section 84).

## 175. Erreur 7 : upgrade sans sauvegarde ni rollback

« Ça va bien se passer » → ça ne se passe pas bien → pas de backup → nuit blanche. **Prévention** : checklist section 123, **à chaque fois**, sans exception.

## 176. Erreur 8 : HA jamais testée

Paire HA installée il y a 2 ans, jamais basculée → le jour de la panne, le standby a une **config obsolète** ou un **heartbeat mort**. **Prévention** : test semestriel au planning (section 101), supervision du standby (section 106).

## 177. Erreur 9 : les logs qui partent nulle part

Syslog configuré une fois, serveur changé depuis, **personne ne s'en rend compte** → enquête impossible. **Prévention** : supervision de la réception des logs (section 160), test après chaque changement.

## 178. Erreur 10 : le compte admin partagé « équipe-réseau »

Un compte pour 5 personnes → impossible de savoir **qui** a fait quoi, mot de passe qui ne change jamais. **Prévention** : comptes **nominatifs** (section 13), traçabilité (section 138).

## 179. Erreur 11 : la règle « temporaire » de 2019

`permit any any` ajoutée « pour dépanner vite fait » → toujours là 7 ans après. **Prévention** : règle des 90 jours (section 128), time-range avec expiration pour les dépannages.

## 180. Erreur 12 : oublier la zone local

On protège trust/untrust/dmz mais on laisse **untrust→local** ouvert → le firewall lui-même est attaquable (SSH, web). **Prévention** : politiques local explicites dès l'initialisation (section 23).

## 181. Erreur 13 : le certificat SSL VPN expiré

Portail avec certificat expiré un lundi matin → 50 tickets. **Prévention** : supervision de l'expiration (section 112), renouvellement à J-30 (section 66).

## 182. Erreur 14 : la licence UTM expirée en silence

Signatures non mises à jour depuis 4 mois, **personne ne le voit** → protection théorique. **Prévention** : alertes J-60/J-30/J-7 (section 119), contrôle mensuel (section 127).

## 183. Erreur 15 : le firmware d'un autre modèle

« USG6630 et USG6650, c'est presque pareil » → flashé le mauvais firmware → boîtier en rideau. **Prévention** : vérifier la **référence exacte** sur l'étiquette + `display version`, jamais « à peu près » (section 124).

## 184. Erreur 16 : couper l'accès en durcissant la zone local

Nouvelle règle local trop stricte appliquée **à distance** → plus de SSH → déplacement en baie. **Prévention** : tester depuis une session existante, garder la **console à portée**, prévoir une règle de secours (section 23).

## 185. Erreur 17 : le DNS oublié dans les politiques

Tout est autorisé sauf... le DNS → « Internet ne marche pas » alors que le réseau est OK. **Prévention** : la règle DNS sortante fait partie du **socle** (section 35), tester par IP **et** par nom (section 156).

---
---

# BLOC Q — PENSE-BÊTE, GLOSSAIRE, QUIZ, POUR ALLER PLUS LOIN

## 186. Pense-bête de poche : une page à imprimer

```
┌─ USG6000 PENSE-BÊTE ─────────────────────────────────┐
│ CONSOLE : 9600-8-N-1                                 │
│ ZONES   : local(100) trust(85) dmz(50) untrust(5)     │
│ ORDRE PAQUET : DNAT → routage → politique → SNAT → UTM│
│ POLITIQUE : 1ère règle qui matche gagne ; défaut=deny │
│ NAT : exemption no-nat AVANT easy-ip (VPN !)         │
│ VPN : ike sa (ph1) → ipsec sa (ph2) ; DPD activé     │
│ UTM : dimensionner sur débit UTM, pas firewall       │
│ HA : display hrp state verbose ; tester 2×/an        │
│ SAUVEGARDE : save + export ; 3 générations           │
│ DEBUG : debugging ... puis TOUJOURS undo debugging all│
│ URGENCE : console + sauvegarde + rollback écrit      │
└──────────────────────────────────────────────────────┘
```

## 187. Les 15 commandes du quotidien

| # | Commande | Usage |
|---|----------|-------|
| 1 | `display version` | Version logicielle |
| 2 | `display current-configuration` | Config complète |
| 3 | `display zone` | Zones ↔ interfaces |
| 4 | `display security-policy rule all` | Politiques (ordre réel) |
| 5 | `display nat-policy rule all` | NAT (ordre réel) |
| 6 | `display firewall session table` | Sessions live |
| 7 | `display ip routing-table` | Routage |
| 8 | `display cpu` / `display memory` | Ressources |
| 9 | `display ike sa` / `display ipsec sa` | VPN |
| 10 | `display hrp state verbose` | HA |
| 11 | `display license` | Licences |
| 12 | `display logbuffer` | Logs récents |
| 13 | `display interface brief` | État interfaces |
| 14 | `display alarm` | Alarmes matérielles |
| 15 | `save` | Sauvegarder (le plus important) |

## 188. Glossaire

| Terme | Définition |
|-------|------------|
| **ALG** | Application Level Gateway — module qui comprend un protocole (FTP, SIP) pour le faire passer à travers le NAT. |
| **ASPF** | Application Specific Packet Filter — inspection applicative stateful (ex. FTP multi-canaux). |
| **DNAT** | Destination NAT — traduit l'adresse de destination (publier un serveur). |
| **DPD** | Dead Peer Detection — détecte qu'un pair VPN ne répond plus pour reconstruire le tunnel. |
| **Easy IP** | NAT de source utilisant l'IP de l'interface de sortie. |
| **EICAR** | Fichier test standard inoffensif détecté par tous les antivirus. |
| **ESP** | Encapsulating Security Payload — protocole 50, transporte les données chiffrées IPSec. |
| **HRP** | Huawei Redundancy Protocol — synchronisation d'état entre 2 USG en HA. |
| **IKE** | Internet Key Exchange — négocie les clés IPSec (phase 1 : IKE SA, phase 2 : IPSec SA). |
| **IPS** | Intrusion Prevention System — bloque les exploits connus par signatures. |
| **NAT-T** | NAT Traversal — encapsule ESP dans UDP 4500 pour traverser un NAT. |
| **PFS** | Perfect Forward Secrecy — renouvelle les clés à chaque phase 2 (compromis sécu/perf). |
| **PSK** | Pre-Shared Key — secret partagé pour l'authentification IKE. |
| **Server-map** | Table des mappings statiques (DNAT) de l'USG. |
| **SNAT** | Source NAT — traduit l'adresse source (accès Internet du LAN). |
| **UTM** | Unified Threat Management — AV+IPS+URL+applicatif+anti-spam dans un seul boîtier. |
| **VGMP** | VRRP Group Management Protocol — bascule groupée des interfaces en HA. |
| **VRP** | Versatile Routing Platform — système d'exploitation Huawei (CLI `system-view`). |
| **Vsys** | Virtual System — découpage d'un USG en firewalls virtuels indépendants. |

## 189. Quiz : 10 questions pour valider (réponses en fin de section)

**Q1.** Quelles sont les 4 zones prédéfinies de l'USG6000 et leurs priorités par défaut ?
**Q2.** Une règle `deny` placée APRÈS un `permit any any` pour le même flux sera-t-elle appliquée ? Pourquoi ?
**Q3.** Pourquoi faut-il une règle d'exemption NAT (`no-nat`) pour le trafic VPN, et où la placer ?
**Q4.** `display ike sa` montre une SA en état RD mais `display ipsec sa` est vide : où est le problème ?
**Q5.** Citez 3 raisons pour lesquelles un débit peut s'effondrer après activation de l'UTM.
**Q6.** En HA, que synchronise HRP et que NE synchronise-t-il PAS ?
**Q7.** Un utilisateur se plaint qu'un site légitime est bloqué par l'IPS après une MAJ de signatures : quelle est la bonne réaction ?
**Q8.** Pourquoi l'inspection SSL/TLS exige-t-elle des précautions juridiques et techniques ?
**Q9.** Quelle est la première chose à vérifier quand « l'interface est UP mais rien ne passe » ?
**Q10.** Citez les 5 étapes d'un upgrade firmware sans stress.

<details>
<summary><b>Réponses</b> (cliquer pour déplier)</summary>

**R1.** local (100) — le firewall lui-même ; trust (85) — réseau de confiance ; dmz (50) — serveurs exposés ; untrust (5) — Internet/non fiable. (Section 20.)
**R2.** Non : c'est la **première règle qui matche** qui s'applique (évaluation de haut en bas). Le `permit any` placé avant intercepte tout. Il faut ordonner du plus spécifique au plus général. (Section 31.)
**R3.** Parce que la règle NAT easy-ip traduirait aussi les adresses du trafic destiné au tunnel, ce qui casse l'IPSec. L'exemption `no-nat` se place **avant** la règle NAT. (Section 52.)
**R4.** Phase 1 OK, phase 2 KO : ACL (miroir), transform-set ou PFS incohérents entre les deux côtés — ou NAT/politique qui bloque le trafic clair. (Sections 61, 148.)
**R5.** (1) Modèle sous-dimensionné pour l'UTM (débit UTM ≪ débit firewall), (2) profils appliqués sur des flux volumineux non à risque, (3) inspection SSL sur tout le trafic. (Sections 73, 152.)
**R6.** HRP synchronise : sessions, tables NAT, tables de routage, SA VPN. Il NE synchronise PAS : certificats, licences, certains états applicatifs — à refaire sur chaque nœud. (Section 103.)
**R7.** Identifier la signature en cause dans les logs, créer une **exception ciblée** (whitelist/exclusion de signature), documenter, revue trimestrielle — jamais désactiver tout le profil. (Sections 86, 153.)
**R8.** Juridique : information des salariés/représentants du personnel, exclusions (banque, santé). Technique : déployer la CA interne sur les postes, gérer le certificate pinning, coût CPU très élevé. (Section 82.)
**R9.** `display zone` : l'interface est-elle bien **membre d'une zone** ? Une interface sans zone ne fait passer aucun trafic. (Section 24.)
**R10.** (1) Sauvegarde config vérifiée, (2) firmware exact + checksum, (3) console branchée + fenêtre de maintenance, (4) upgrade (passif d'abord en HA), (5) vérification complète + garder l'ancien firmware 1 semaine + plan de rollback écrit. (Sections 117, 123.)

</details>

## 190. Pour aller plus loin : les chantiers d'après

1. **Firewalls virtuels (vsys)** : découper un USG6680 en plusieurs firewalls logiques indépendants (multi-clients, séparation prod/hors-prod) — à étudier quand un seul boîtier doit servir plusieurs entités.
2. **Pilotage centralisé** : NCE / SecoManager pour gérer un parc d'USG (politiques centralisées, logs, conformité) — indispensable au-delà de 5–10 boîtiers.
3. **SD-WAN sécurisé** : les gammes récentes (USG6000F) intègrent le SD-WAN chiffré — piste pour remplacer des MPLS coûteux entre sites.
4. **Automatisation** : l'USG expose une **API REST** (selon version) + NETCONF — scriptage des audits de config, détection de dérive (comparaison périodique avec la config de référence).
5. **Threat intelligence** : abonner l'USG aux flux de threat intel (listes d'IP malveillantes dynamiques) en complément des signatures.
6. **Zero Trust** : coupler l'USG (segmentation, authentification) avec du 802.1X/NAC sur les switchs pour une vraie architecture Zero Trust d'entreprise.
7. **Maquette permanente** : garder un USG de labo (ou USG6000V virtualisé) pour tester **chaque** changement avant la prod — le meilleur investissement sécurité qui soit.

## 191. Sources et références

- Fiche technique « Huawei USG6000 Series NGFW Overview » (débits par modèle : 500 Mbit/s → 40 Gbit/s, IPS+AV jusqu'à ~15 Gbit/s, 6000+ applications, base URL 130+ catégories / 500M+ URL).
- « HUAWEI USG6000, USG6000E, USG9500, and NGFW Module Quick Configuration Guide (With New Web UI) » — basé sur USG6000E V600R007C00 (management GE0/0/0, 192.168.0.1/24 par défaut, premier login web avec création d'administrateur sur V600R007C20+).
- White paper technique « HUAWEI Secospace USG6000 Series » (4 zones par défaut : Trust, Untrust, DMZ, Local ; disques SAS RAID1 hot-swap pour les logs).
- Spécifications matérielles par modèle (ports, alimentations AC/DC redondantes 1+1 sur 3U, console RJ45/Mini-USB).
- Documentation CLI VRP : exemples `security-policy`, `nat-policy` (easy-ip), zones, IKE/IPSec issus des guides de configuration Huawei.

> ⚠️ **Note de l'auteur** : les valeurs constructeur citées datent de septembre 2026. Avant tout achat ou dimensionnement, **vérifie sur la fiche du modèle exact** (les gammes E et F ont leurs propres fiches). Les commandes CLI sont syntaxiquement plausibles pour VRP V500/V600 mais **certaines variantes existent selon la version** — les points marqués « à vérifier selon version » doivent être validés avec `?` en maquette avant la prod. Aucun mot de passe réel ne figure dans ce guide : tous les secrets des exemples sont fictifs.

---

*Fin du guide — Huawei USG6000. Bon courage en baie, et que tes bascules HA soient toujours en heures creuses.* 🔧
