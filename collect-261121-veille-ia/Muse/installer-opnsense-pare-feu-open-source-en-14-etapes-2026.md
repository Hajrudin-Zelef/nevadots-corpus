---
id: collect-261121-veille-ia/muse/installer-opnsense-pare-feu-open-source-en-14-etapes-2026
title: "Installer OPNsense : Pare-feu Open Source en 14 Étapes [2026]"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["opnsense", "pare-feu", "open", "source", "jour", "pfsense", "projet", "version"]
source: https://tech-insider.org/fr/installer-opnsense-pare-feu-open-source-2026/
source_anchor: ""
source_lines: [1, 347]
section: "Muse"
---
OPNsense vient de passer un cap. La version 26.7.3 “Xenial Xenops” est sortie le 27 août 2026, à peine un jour avant la publication de ce guide, et elle embarque FreeBSD 15.1, PHP 8.5 et un moteur de règles de pare-feu entièrement réécrit en MVC. Pour les administrateurs réseau qui cherchent une alternative open source crédible aux pare-feux propriétaires (Fortinet, Sophos, Cisco) ou à pfSense, c’est le bon moment pour s’y mettre. Ce tutoriel couvre l’installation complète, du téléchargement de l’ISO jusqu’à la mise en place du VPN WireGuard, de la détection d’intrusion Suricata et de la haute disponibilité CARP, avec chaque commande et chaque piège documenté.

OPNsense équipe aujourd’hui des dizaines de milliers de box faites maison, de PME et même certaines collectivités qui refusent la dépendance à un éditeur unique. Le projet, porté par l’entreprise néerlandaise Deciso, publie des mises à jour à un rythme largement supérieur à celui de pfSense CE, dont la dernière version communautaire (2.8.1) date de septembre 2025. Voici comment monter votre propre pare-feu OPNsense en partant de zéro, avec un niveau de détail suffisant pour éviter les erreurs qui bloquent la moitié des installations débutantes.

## Qu’est-ce qu’OPNsense et pourquoi l’adopter en 2026

OPNsense est un système d’exploitation de pare-feu et de routage open source, basé sur FreeBSD, né en 2015 d’une scission (fork) du projet pfSense. Les développeurs à l’origine du fork reprochaient à pfSense un manque de transparence dans le développement et une gouvernance trop centralisée autour de Netgate. Onze ans plus tard, OPNsense s’est imposé comme un projet à part entière avec sa propre base de code, son interface web et un calendrier de sortie indépendant remarquablement régulier : la série 25.1 (dotée d’une option d’extinction dans l’installateur) est sortie le 29 janvier 2025, suivie de 25.7 “Visionary Viper” le 23 juillet 2025 puis de 26.1 “Witty Woodpecker” le 28 janvier 2026, selon la documentation officielle du projet, soit une nouvelle branche majeure quasiment tous les six mois.

La branche communautaire (Community Edition, CE) actuelle est la série 26.7 “Xenial Xenops”, sortie le 15 juillet 2026, stabilisée par une première mise à jour 26.7.1 dès le 21 juillet 2026 puis portée à la version 26.7.3 le 27 août 2026 (la dernière en date selon Wikipedia). Elle apporte une base FreeBSD 15.1, l’intégration d’OpenVPN 2.7, un environnement PHP 8.5 pour l’interface web, un moteur de règles de pare-feu MVC désormais activé par défaut, ainsi qu’un support IPv6 amélioré. Pour rappel, la branche précédente 26.1 “Witty Woodpecker” avait elle-même reçu pas moins de onze mises à jour mineures jusqu’à la 26.1.11 du 1er juillet 2026, d’après le calendrier de versions tenu par endoflife.date, ce qui illustre le rythme de correctifs très soutenu du projet. Deciso maintient en parallèle une édition Business (BE), dont la version 26.4.2 a été publiée le 14 août 2026, destinée aux entreprises qui veulent un support contractuel et un rythme de mise à jour plus stable.

Contrairement à pfSense, qui distingue une édition Community (CE) gratuite et une édition Plus payante réservée aux appliances Netgate, OPNsense garde l’intégralité de son code sous licence open source de type BSD, y compris pour l’édition Business. La différence entre CE et BE tient surtout au calendrier de publication et au niveau de support, pas à des fonctionnalités verrouillées derrière un paywall. C’est un argument de poids pour les administrateurs qui veulent auditer leur pare-feu de bout en bout.

## Prérequis matériels et logiciels

Avant de se lancer, il faut choisir la bonne machine. OPNsense tourne sur toute architecture x86-64, mais les besoins réels varient énormément selon les fonctionnalités activées. La documentation officielle distingue plusieurs profils de dimensionnement selon le débit visé et les modules utilisés.

| Usage | CPU | RAM | Stockage | Débit indicatif | 
|---|---|---|---|---|
| Pare-feu basique uniquement | 1 GHz mono/dual-core 64 bits | 2 Go min. / 4 Go recommandé | 4-8 Go (image nano) | 11-150 Mbps | 
| Pare-feu + DNS + DHCP | 1 GHz dual-core | 4 Go min. / 8 Go recommandé | 16-40 Go SSD | 151-350 Mbps | 
| Pare-feu + Suricata IDS/IPS | 1,5 GHz multi-core (Intel J6412 conseillé) | 8 Go min. / 16 Go recommandé | 40-120 Go SSD | 350-750+ Mbps | 
| Toutes fonctions (VPN + IDS + HA + monitoring) | Quad-core 1,6 GHz+ | 16 Go min. / 32 Go recommandé | 120 Go SSD NVMe | 750 Mbps et plus | 

Pour une utilisation domestique ou en petite entreprise avec une connexion fibre à 500 Mbps ou 1 Gbps, un mini-PC équipé d’un processeur Intel de la gamme N100/N305 ou d’un ancien SFF (Small Form Factor) avec 8 Go de RAM et deux cartes réseau Intel (pilotes igb/em, réputés pour leur stabilité sous FreeBSD) constitue un excellent point de départ. Évitez les cartes réseau Realtek premier prix : les pilotes FreeBSD associés sont historiquement moins bien supportés et provoquent des pertes de paquets sous forte charge.

- Machine x86-64 avec au minimum 2 interfaces réseau (une pour le WAN, une pour le LAN)
- 4 Go de RAM minimum, 8 Go recommandé si vous activez Suricata
- 16 à 40 Go de stockage SSD (évitez les clés USB en usage permanent, leur endurance en écriture est trop faible)
- Une clé USB de 8 à 16 Go pour créer le support d’installation
- Un poste de travail avec Rufus (Windows) ou balenaEtcher (Windows/macOS/Linux) pour graver l’image
- Un accès physique ou une console série pour l’installation initiale
- Cartes réseau Intel de préférence, pour la compatibilité des pilotes FreeBSD

## Étape 1 : télécharger l’image ISO d’OPNsense

Rendez-vous sur la page de téléchargement officielle du projet et sélectionnez l’image DVD amd64. Pour la série 26.7, le fichier porte le nom `OPNsense-26.7-dvd-amd64.iso.bz2`. Point important souvent oublié : l’image est compressée en bzip2, il faut donc la décompresser avant de la graver sur la clé USB. Une erreur classique consiste à écrire directement le fichier `.bz2` sur la clé, ce qui produit un support non amorçable. Notez aussi qu’une fois décompressée, l’image ISO occupe environ 3 Go sur la clé USB, mais le système de fichiers se redimensionne automatiquement pour utiliser tout l’espace disponible dès le premier démarrage sur le disque de destination, une précision apportée par le README des miroirs de téléchargement d’OPNsense depuis août 2025.

```
# Sous Linux, décompression de l'image téléchargée
bunzip2 OPNsense-26.7-dvd-amd64.iso.bz2
# Vérification de l'intégrité avec la somme de contrôle SHA256 publiée sur opnsense.org
sha256sum OPNsense-26.7-dvd-amd64.iso
```
Vérifiez systématiquement la somme de contrôle SHA256 fournie sur le site officiel avant de graver l’image. Cette étape, souvent négligée, permet d’éviter d’installer un firmware corrompu ou modifié, particulièrement critique pour un équipement qui filtre tout le trafic réseau de votre organisation.

## Étape 2 : créer une clé USB bootable avec Rufus ou Etcher

Sous Windows, ouvrez Rufus, sélectionnez votre clé USB dans le menu déroulant “Périphérique”, puis cliquez sur “Sélection” pour pointer vers le fichier ISO décompressé. Laissez le schéma de partition sur GPT si votre machine cible démarre en UEFI (le cas le plus fréquent en 2026), ou MBR pour un BIOS Legacy. Lancez l’écriture et patientez : l’opération prend entre 3 et 8 minutes selon la vitesse de la clé.

Sous macOS ou Linux, balenaEtcher offre une interface identique en trois clics : sélection de l’image, sélection du disque cible, écriture. Etcher a l’avantage de vérifier automatiquement l’écriture après la gravure, ce qui élimine une source d’échec fréquente à l’étape suivante.

## Étape 3 : démarrer l’installateur et effectuer l’installation

Branchez la clé USB sur la machine cible, allumez-la et entrez dans le menu de démarrage (Boot Menu), généralement accessible avec F11, F12 ou Échap selon le fabricant de la carte mère. Sélectionnez la clé USB comme périphérique de démarrage.

OPNsense démarre dans un environnement live et affiche un menu console. Connectez-vous avec l’identifiant `installer` ou lancez directement le programme d’installation depuis le menu. L’assistant vous demande ensuite :

- Le clavier à utiliser (choisissez “French” pour un clavier AZERTY)
- Le type d’installation : “Install (ZFS)” est recommandé pour bénéficier des snapshots et de la protection contre la corruption de données, “Install (UFS)” reste plus léger pour du matériel très modeste
- Le disque de destination
- Le mot de passe du compte `root` , à choisir robuste puisqu’il protège l’accès administrateur complet du pare-feu

Une fois l’installation terminée, l’assistant propose de redémarrer. Retirez la clé USB avant le redémarrage pour éviter de rebooter sur l’installateur en boucle, une erreur d’inattention très courante.

## Étape 4 : assigner les interfaces réseau WAN et LAN

Au premier démarrage, la console affiche le menu principal d’OPNsense avec une série d’options numérotées. Choisissez l’option “Assign interfaces” (assigner les interfaces). Le système liste les interfaces détectées avec leur nom de pilote FreeBSD, par exemple `igb0`, `igb1` ou `em0`, `em1`.

C’est l’étape la plus sensible de toute l’installation. Le système vous demande de désigner physiquement l’interface WAN (connectée à votre box ou modem) puis l’interface LAN (connectée à votre commutateur interne). Le moyen le plus fiable de ne pas se tromper consiste à débrancher tous les câbles réseau sauf un, observer quelle interface devient active dans la console, puis répéter l’opération pour chaque câble.

```
# Dans le menu console OPNsense
# Option 1 : Assign interfaces
# Le système demande :
Enter the WAN interface name or 'a' for auto-detection: igb0
Enter the LAN interface name or 'a' for auto-detection: igb1
Enter the Optional interface 1 name or 'a' for auto-detection (or nothing if finished): [Entrée]
Do you want to proceed? [y/n]: y
```
## Étape 5 : configurer l’adresse IP du LAN et accéder à l’interface web

Toujours dans le menu console, sélectionnez l’option de configuration de l’interface LAN. Attribuez une adresse IPv4 statique, par exemple `192.168.1.1/24`, et activez le serveur DHCP si vous voulez que les postes du réseau interne reçoivent automatiquement une adresse IP dès cette étape.

Connectez ensuite un ordinateur au port LAN du pare-feu (directement ou via un commutateur), laissez-le obtenir une adresse par DHCP, puis ouvrez un navigateur et accédez à `https://192.168.1.1`. Le certificat étant auto-signé, votre navigateur affiche un avertissement de sécurité : c’est normal à ce stade, acceptez l’exception. Connectez-vous avec l’identifiant `root` et le mot de passe défini lors de l’installation.

## Étape 6 : suivre l’assistant de configuration initiale

Dès la première connexion, OPNsense lance un assistant de configuration en plusieurs écrans. Il vous demande le nom d’hôte, le domaine, les serveurs DNS à utiliser (ou de laisser le résolveur Unbound intégré s’en charger), le fuseau horaire et un serveur NTP. Vient ensuite le type de connexion WAN : IP statique, DHCP (le plus courant chez les box grand public en mode bridge) ou PPPoE (fréquent chez certains FAI xDSL en France).

L’assistant propose enfin de changer le mot de passe root et de recharger la configuration. À l’issue de ces écrans, le pare-feu applique déjà une politique par défaut sensée : le trafic sortant du LAN vers le WAN est autorisé, le trafic entrant depuis le WAN vers le LAN est bloqué.

## Étape 7 : installer les extensions (plugins) essentielles

OPNsense repose sur une architecture modulaire : l’installation de base reste volontairement légère et les fonctionnalités avancées s’ajoutent via des plugins téléchargés depuis le dépôt officiel. Rendez-vous dans le menu Système → Firmware → Plugins.

| Plugin | Fonction | Recommandé pour | 
|---|---|---|
| os-wireguard | VPN WireGuard, site-à-site ou nomade | Tous les usages, performance élevée | 
| os-ids (Suricata) | Détection et prévention d’intrusion réseau | Entreprises, réseaux exposés | 
| os-acme-client | Certificats TLS automatiques via Let’s Encrypt | Interfaces web exposées publiquement | 
| os-theme-cicada / os-theme-rebellion | Thèmes d’interface alternatifs | Confort visuel, aucun impact fonctionnel | 
| os-netflow / Insight | Statistiques de trafic détaillées par hôte et protocole | Supervision de bande passante | 
| os-nut | Gestion d’onduleur (UPS) réseau | Salles serveurs, arrêt propre sur coupure secteur | 

Pour installer un plugin, tapez son nom dans le champ de recherche, cliquez sur l’icône “+” en face du résultat, puis patientez quelques secondes pendant le téléchargement et l’installation. Certains plugins comme os-wireguard ajoutent immédiatement un nouveau menu dans l’interface, sans nécessiter de redémarrage du pare-feu.

## Étape 8 : configurer les règles de pare-feu et le NAT

La version 26.7 généralise le nouveau moteur de règles MVC (Modèle-Vue-Contrôleur), qui restructure l’affichage des règles sans changer leur logique. Rendez-vous dans Firewall → Rules → LAN pour consulter la règle par défaut autorisant tout le trafic sortant du réseau local.

Pour ouvrir un service hébergé en interne vers l’extérieur (un serveur web, un accès VPN), il faut créer une redirection de port sous Firewall → NAT → Port Forward. OPNsense génère alors automatiquement la règle de pare-feu correspondante sur l’interface WAN, sauf si vous décochez l’option “Filter rule association”.

```
# Exemple : redirection du port 443 (HTTPS) vers un serveur web interne
# Firewall > NAT > Port Forward > Add
Interface: WAN
Protocol: TCP
Destination: WAN address
Destination port range: HTTPS (443)
Redirect target IP: 192.168.1.50
Redirect target port: HTTPS (443)
Description: Reverse proxy interne - serveur web
```
Pour le NAT sortant, le mode par défaut “Automatic outbound NAT” convient à la majorité des installations. Si vous avez besoin d’un contrôle plus fin (par exemple pour forcer certaines machines à sortir par une adresse IP publique dédiée sur une connexion multi-WAN), basculez sur le mode “Hybrid” ou “Manual” dans Firewall → NAT → Outbound.

## Étape 9 : mettre en place le DHCP et le résolveur DNS Unbound

Sous Services → DHCPv4 → LAN, activez le serveur DHCP et définissez une plage d’adresses, par exemple de `192.168.1.100` à `192.168.1.200`, en laissant les 99 premières adresses disponibles pour des attributions statiques (imprimantes, serveurs, équipements réseau). Renseignez la passerelle et le serveur DNS avec l’adresse du pare-feu lui-même (`192.168.1.1`), qui fait office de résolveur.

OPNsense embarque Unbound comme résolveur DNS récursif par défaut, accessible sous Services → Unbound DNS → General. Le mode récursif interroge directement les serveurs racine DNS sans dépendre d’un fournisseur tiers, ce qui améliore la confidentialité. Cochez “Register DHCP leases in the DNS Resolver” pour que les machines du réseau local soient résolubles par leur nom d’hôte.

## Étape 10 : configurer un VPN avec WireGuard

Après avoir installé le plugin os-wireguard, une nouvelle section apparaît sous VPN → WireGuard. Commencez par créer une “Local” instance : un nom, un port d’écoute (le port UDP 51820 par défaut) et une plage d’adresses IP virtuelles pour le tunnel, par exemple `10.10.10.0/24`.

```
# Génération d'une paire de clés WireGuard côté client (Linux/macOS)
wg genkey | tee client_private.key | wg pubkey > client_public.key
# Exemple de configuration client WireGuard à importer sur l'appareil distant
[Interface]
PrivateKey = <client_private.key>
Address = 10.10.10.2/24
DNS = 192.168.1.1
[Peer]
PublicKey = <clé_publique_du_serveur_OPNsense>
Endpoint = votre-ip-publique-ou-ddns:51820
AllowedIPs = 192.168.1.0/24, 10.10.10.0/24
PersistentKeepalive = 25
```
Ajoutez chaque appareil distant comme “Peer” dans l’interface OPNsense, en collant sa clé publique. N’oubliez pas de créer une règle de pare-feu sur l’interface WAN autorisant le trafic UDP entrant sur le port 51820, sans quoi le tunnel ne s’établira jamais, et une règle sur l’interface WireGuard elle-même pour autoriser le trafic du tunnel vers le LAN.

## Étape 11 : activer la détection d’intrusion avec Suricata

Le plugin os-ids intègre le moteur Suricata directement dans OPNsense. Rendez-vous dans Services → Intrusion Detection → Administration. Deux modes sont disponibles : IDS (détection passive, journalisation uniquement) et IPS (prévention active, blocage en ligne du trafic malveillant). Le mode IPS exige davantage de ressources CPU car chaque paquet est inspecté avant d’être transmis.

Sous l’onglet “Download”, activez au moins un jeu de règles gratuit comme “ET Open” (Emerging Threats Open), puis lancez une mise à jour manuelle des règles. Sous “Policy”, associez les interfaces à surveiller (typiquement WAN, et LAN si vous redoutez une compromission interne). Activez ensuite le service et laissez-le tourner 24 à 48 heures en mode IDS avant de basculer en mode IPS, le temps d’identifier les faux positifs propres à votre trafic.

- Commencez toujours en mode détection (IDS) avant de passer en blocage (IPS)
- Désactivez les catégories de règles non pertinentes (par exemple les règles orientées jeux vidéo si votre réseau est un environnement professionnel strict) pour réduire la charge CPU
- Surveillez l’onglet “Alerts” quotidiennement la première semaine pour ajuster les seuils

## Étape 12 : mettre en place la haute disponibilité avec CARP

Pour les environnements qui ne tolèrent pas de coupure, OPNsense prend en charge le protocole CARP (Common Address Redundancy Protocol) permettant de faire fonctionner deux pare-feux en cluster actif/passif. Le nœud principal (MASTER) porte une adresse IP virtuelle partagée ; en cas de panne, le nœud secondaire (BACKUP) reprend automatiquement cette adresse.

La configuration se fait sous Interfaces → Virtual IPs pour créer les adresses CARP, puis sous System → High Availability → Settings pour synchroniser la configuration (XMLRPC Sync) et l’état des connexions (pfsync) entre les deux nœuds via un lien réseau dédié. Cette synchronisation garantit qu’une bascule ne coupe pas les connexions déjà établies, un point critique pour les connexions VPN de longue durée.

## Étape 13 : mises à jour, sauvegarde et restauration de la configuration

Toute la configuration d’OPNsense (règles de pare-feu, VPN, DHCP, plugins installés) est stockée dans un fichier unique, `config.xml`. Sous System → Configuration → Backups, téléchargez régulièrement ce fichier sur un support externe, hors du pare-feu lui-même. En cas de panne matérielle, une réinstallation suivie d’un import de ce fichier restaure l’intégralité de la configuration en quelques minutes.

```
# Vérification manuelle des mises à jour disponibles en ligne de commande
opnsense-update -c
# Application des mises à jour (équivalent à System > Firmware > Updates dans l'UI)
opnsense-update -bkr
```
Sous System → Firmware → Updates, OPNsense vérifie automatiquement la disponibilité de nouvelles versions et affiche un changelog avant application. Compte tenu du rythme de sortie soutenu du projet (trois versions mineures pour la seule branche 26.7 entre juillet et août 2026, un rythme déjà observé sur la branche 25.1 dont la dernière mineure, la 25.1.12, était sortie le 21 juillet 2025 selon endoflife.date), planifiez une vérification hebdomadaire au minimum, et systématiquement après toute annonce de faille de sécurité sur le forum officiel.

## Étape 14 : sécuriser l’accès administrateur avec la double authentification

Un pare-feu constitue une cible de choix : compromettre le compte root d’OPNsense revient à prendre le contrôle de tout le filtrage réseau d’une organisation. Avant de mettre l’équipement en production, il est fortement recommandé d’activer la double authentification (2FA) sur l’interface web, en plus d’un mot de passe robuste.

Sous System → Access → Servers, ajoutez un serveur d’authentification local avec un second facteur de type TOTP (Time-based One-Time Password), compatible avec des applications comme Google Authenticator, Aegis ou une YubiKey configurée en mode OTP. Une fois le serveur créé, associez-le au compte administrateur sous System → Access → Users, puis scannez le QR code généré avec votre application d’authentification.

- Créez un compte administrateur secondaire nommé (par exemple `admin-nom` ) plutôt que de n’utiliser que le compte`root` générique, pour garder une traçabilité des actions dans les journaux
- Restreignez l’accès à l’interface web à des adresses IP sources précises sous Firewall → Rules → LAN, en particulier si l’administration s’effectue aussi via VPN
- Changez le port HTTPS par défaut (443) de l’interface de gestion sous System → Settings → Administration si celle-ci reste exposée sur une interface autre que le LAN de confiance
- Désactivez l’accès administrateur en HTTP non chiffré, qui reste parfois actif par erreur après une mise à niveau

Ce durcissement prend une dizaine de minutes et élimine l’essentiel du risque lié au vol ou à la réutilisation d’un mot de passe, un vecteur d’attaque encore responsable d’une part importante des compromissions d’équipements réseau signalées par le CERT-FR ces dernières années.

## Multi-WAN et répartition de charge entre deux connexions Internet

Pour les petites entreprises qui ne peuvent pas se permettre une coupure Internet totale, OPNsense permet de connecter deux liens WAN distincts (par exemple une fibre et un lien 4G/5G de secours) et de répartir automatiquement la charge ou de basculer sur le second lien en cas de panne du premier.

La configuration se fait sous System → Gateways, où chaque connexion WAN reçoit une passerelle distincte surveillée par un test de latence (dpinger) vers une cible fiable, par exemple un résolveur DNS public. Sous System → Gateways → Group, créez ensuite un groupe de passerelles définissant l’ordre de priorité (tier 1, tier 2) : en fonctionnement normal, le trafic emprunte la passerelle de priorité la plus haute, et bascule automatiquement sur la passerelle de secours si les tests de latence ou de perte de paquets dépassent les seuils configurés.

```
# Exemple de configuration d'un groupe de passerelles (System > Gateways > Group)
Group Name: WAN_FAILOVER
Gateway: WAN_FIBRE   Tier: 1   (priorité principale)
Gateway: WAN_4G       Tier: 2   (bascule de secours)
Trigger Level: Member Down
# Règle de pare-feu utilisant ce groupe comme passerelle
Firewall > Rules > LAN
Action: Pass, Gateway: WAN_FAILOVER
```
Ce mécanisme de bascule (failover) reste transparent pour les utilisateurs du réseau local : seules les connexions actives au moment de la coupure sont interrompues, les nouvelles connexions empruntant automatiquement le lien de secours en quelques secondes. Pour une répartition de charge plutôt qu’un simple basculement, il suffit d’attribuer le même niveau de priorité (tier) aux deux passerelles : OPNsense distribue alors les nouvelles connexions entre les deux liens selon un mécanisme de round-robin pondéré.

## OPNsense vs pfSense : quelles différences concrètes en 2026

La question revient systématiquement chez les administrateurs qui migrent d’une solution vers l’autre. Les deux projets partagent une origine commune (OPNsense est un fork de pfSense datant de 2015) mais ont divergé sur la gouvernance, la licence et le rythme de publication.

| Critère | OPNsense | pfSense | 
|---|---|---|
| Éditeur | Deciso B.V. (Pays-Bas) | Netgate (États-Unis) | 
| Dernière version gratuite | 26.7.3 (27 août 2026) | pfSense CE 2.8.1 (septembre 2025) | 
| Édition commerciale | Business Edition 26.4.2, même base de code open source | pfSense Plus, fonctions réservées aux appliances Netgate | 
| Licence | Open source BSD sur l’ensemble du code | CE open source, Plus partiellement propriétaire | 
| Base système | FreeBSD 15.1 | FreeBSD (version antérieure) | 
| Rythme de publication | Plusieurs versions mineures par an | Une à deux versions majeures par an | 

Pour un déploiement neuf sans contrainte de compatibilité avec du matériel Netgate existant, OPNsense présente l’avantage d’un code entièrement ouvert et d’un cycle de correctifs plus rapide. pfSense conserve un écosystème d’appliances certifiées plus large et reste pertinent pour les organisations déjà investies dans du matériel Netgate.

## Édition Business, appliances et coûts de support

Deciso commercialise des appliances matérielles préinstallées et des contrats de support autour de l’édition Business (26.4.2 au 14 août 2026). Les tarifs précis dépendent du modèle d’appliance et du niveau de support choisi, et sont publiés directement sur le site de Deciso plutôt que dans un catalogue tarifaire fixe. Pour une utilisation personnelle ou associative, la Community Edition installée sur du matériel générique reste totalement gratuite et couvre l’immense majorité des besoins, y compris Suricata, WireGuard et la haute disponibilité.

## Cinq pièges fréquents à éviter

La majorité des échecs d’installation ou des pannes réseau après mise en production d’OPNsense proviennent d’une poignée d’erreurs récurrentes, largement documentées sur les forums de la communauté.

1. **Inversion des interfaces WAN et LAN** : brancher le câble du fournisseur d’accès sur l’interface configurée en LAN expose immédiatement le réseau interne à Internet sans filtrage. Vérifiez toujours physiquement chaque câble avant l’assignation finale.
2. **Verrouillage total après une règle de pare-feu trop stricte** : supprimer par erreur la règle autorisant l’accès HTTPS depuis le LAN vers l’interface de gestion coupe l’accès à l’interface web. La console série ou un accès physique au clavier reste alors le seul recours pour rétablir une règle d’urgence.
3. **Conflit de serveurs DHCP** : laisser le DHCP de la box internet actif en parallèle de celui d’OPNsense provoque des attributions d’adresses IP incohérentes et des pertes de connectivité intermittentes. Désactivez systématiquement le DHCP de la box en amont.
4. **Décharge matérielle (offloading) incompatible avec certaines cartes réseau** : certains contrôleurs Realtek ou des cartes réseau virtuelles mal configurées provoquent des instabilités si les fonctions de décharge de checksum (checksum offloading) ou TSO restent actives. Désactivez-les sous Interfaces → Settings en cas de symptômes de latence ou de paquets perdus.
5. **Interception TLS pour l’IDS sans PKI maîtrisée** : tenter de déchiffrer le trafic HTTPS pour l’inspection Suricata sans déployer correctement un certificat d’autorité de confiance sur tous les postes clients génère des erreurs de certificat en cascade côté utilisateurs. Cette fonctionnalité doit rester réservée aux environnements d’entreprise avec un service informatique capable de gérer un magasin de certificats centralisé.

## Résultats attendus après une installation réussie

Une fois toutes les étapes complétées, le tableau de bord (Dashboard) de l’interface web affiche l’état de santé du système : charge CPU, utilisation mémoire, état des interfaces réseau, et un flux des dernières entrées de journal. Voici un exemple représentatif de ce que vous devez observer sur une installation fonctionnelle.

```
État général attendu après installation :
Interfaces:
  WAN (igb0)   [UP]   IP: 203.0.113.45/24    Trafic entrant/sortant actif
  LAN (igb1)   [UP]   IP: 192.168.1.1/24     4 baux DHCP actifs
Services:
  unbound (DNS resolver)     [running]
  dhcpd (LAN)                [running]
  wireguard                  [running]  1 instance, 2 peers connectés
  suricata (IDS mode)        [running]  Interface: WAN
Firewall:
  Règles actives: 12
  Connexions suivies (states): 340/500000
  Dernière mise à jour firmware: 26.7.3 (27 août 2026)
```
Si votre tableau de bord ressemble à cet exemple, avec des interfaces “UP”, des services actifs sans erreur rouge et un nombre de connexions suivies cohérent avec votre trafic réel, l’installation est opérationnelle et prête pour un usage en production.

## Dépannage : 8 problèmes courants et leurs solutions

Voici les incidents les plus fréquemment rapportés par la communauté OPNsense, avec la cause probable et la correction associée.

1. **Impossible d’accéder à l’interface web après configuration** : vérifiez que le poste client est bien sur le même sous-réseau que le LAN, videz le cache DNS local, et confirmez qu’aucune règle de pare-feu ne bloque le port 443 sur l’interface LAN elle-même (Firewall → Rules → LAN).
2. **Aucune connexion Internet après l’installation** : contrôlez le type de WAN configuré (DHCP, statique, PPPoE) correspond bien à celui exigé par votre fournisseur d’accès, et vérifiez sous Interfaces → Overview que l’interface WAN a bien obtenu une adresse IP publique.
3. **Le tunnel WireGuard ne s’établit pas** : confirmez que le port UDP 51820 est ouvert sur le pare-feu WAN, que les clés publiques sont correctement échangées entre pairs, et que l’horloge système du serveur et du client ne dérive pas de plus de quelques minutes.
4. **Suricata consomme un CPU anormalement élevé** : réduisez le nombre de catégories de règles actives, passez en mode IDS plutôt qu’IPS si le matériel est limité, et vérifiez que le nombre de threads alloués correspond au nombre de cœurs physiques disponibles.
5. **Bascule CARP intempestive entre les deux nœuds** : ce symptôme provient généralement d’un lien de synchronisation pfsync saturé ou instable ; dédiez une interface réseau distincte, idéalement à 1 Gbps minimum, uniquement pour ce trafic de synchronisation.
6. **Échec de mise à jour du firmware** : videz l’espace disque disponible (une mise à jour nécessite généralement 2 à 4 Go d’espace libre), vérifiez la connectivité vers les dépôts OPNsense, puis relancez via`opnsense-update -c` pour un diagnostic en ligne de commande.
7. **Certificat Let’s Encrypt (ACME) qui ne se renouvelle pas** : vérifiez que le port 80 reste accessible depuis Internet pour la validation HTTP-01, ou basculez vers une validation DNS-01 si le port 80 est bloqué par ailleurs.
8. **Débit réseau anormalement bas malgré un matériel dimensionné** : désactivez le mode IPS de Suricata pour tester, vérifiez les statistiques d’interruptions (interrupts) au niveau du système, et confirmez que les cartes réseau utilisent bien un pilote FreeBSD natif (igb, em, ix) plutôt qu’un pilote générique.

## Conseils avancés pour aller plus loin

Une fois l’installation de base stabilisée, plusieurs ajustements permettent de tirer davantage parti d’OPNsense en environnement exigeant. Le plugin os-netflow, couplé à un outil de visualisation comme Insight, donne une vision granulaire de la consommation de bande passante par machine et par protocole, utile pour détecter une exfiltration de données ou un poste compromis participant à un botnet.

Pour la segmentation réseau, la création de VLAN directement dans OPNsense (Interfaces → Other Types → VLAN) permet d’isoler un réseau invité, un réseau IoT et un réseau de production sur une seule carte réseau physique reliée à un commutateur administrable compatible 802.1Q. Chaque VLAN reçoit ensuite son propre jeu de règles de pare-feu, empêchant par exemple une caméra IP compromise de communiquer avec un poste de travail du réseau de production.

Enfin, pour les environnements virtualisés (Proxmox, KVM, VMware ESXi), privilégiez les pilotes VirtIO pour les interfaces réseau plutôt que l’émulation d’une carte Intel générique : le gain de performance est significatif, à condition de vérifier au préalable que le noyau FreeBSD embarqué dans votre version d’OPNsense supporte correctement le pilote VirtIO de votre hyperviseur, ce qui est le cas pour toutes les versions de la série 26.x.

## Projet complet : pare-feu domestique sécurisé avec VPN et IDS

Pour synthétiser l’ensemble du tutoriel, voici l’architecture complète d’un projet fonctionnel que vous pouvez reproduire de bout en bout sur un mini-PC à deux cartes réseau.

1. Installation d’OPNsense 26.7.3 sur un mini-PC équipé de 8 Go de RAM, 120 Go de SSD et deux ports Ethernet Intel
2. Interface WAN en DHCP côté box opérateur placée en mode bridge, interface LAN en `192.168.1.1/24` avec DHCP activé
3. Résolveur Unbound configuré en mode récursif avec enregistrement automatique des baux DHCP
4. VLAN dédié pour les objets connectés (`192.168.20.0/24` ), isolé du réseau principal par des règles de pare-feu strictes
5. Plugin os-wireguard installé, tunnel VPN nomade actif sur le port UDP 51820 pour l’accès distant sécurisé
6. Plugin os-ids avec Suricata en mode IDS sur l’interface WAN, jeu de règles ET Open activé
7. Plugin os-acme-client configuré pour renouveler automatiquement un certificat Let’s Encrypt sur l’interface de gestion
8. Sauvegarde automatique hebdomadaire du fichier `config.xml` vers un stockage externe

```
# Résumé des règles de pare-feu essentielles du projet complet
# Sur l'interface WAN
Allow UDP 51820 (WireGuard) from any to WAN address
# Sur l'interface WireGuard
Allow TCP/UDP from 10.10.10.0/24 to 192.168.1.0/24
# Sur le VLAN IoT
Block from 192.168.20.0/24 to 192.168.1.0/24
Allow from 192.168.20.0/24 to WAN (Internet uniquement)
# Sur le LAN principal
Allow LAN net to any (règle par défaut)
```
Ce projet couvre la quasi-totalité des besoins d’un particulier exigeant ou d’une très petite structure : accès distant sécurisé, isolation des objets connectés, détection d’intrusion et chiffrement TLS automatisé, le tout sur du matériel accessible et sans licence à payer.

### Foire aux questions

**OPNsense est-il gratuit ?**

Oui, la Community Edition est entièrement gratuite et open source. Seule l’édition Business, destinée aux entreprises souhaitant un support contractuel avec Deciso, implique un coût.

**Quelle est la différence entre OPNsense et pfSense ?**

OPNsense est un fork de pfSense créé en 2015. Les deux projets partagent une origine commune mais divergent sur la gouvernance, la licence (OPNsense reste intégralement open source) et le rythme de publication, plus soutenu chez OPNsense.

**Combien de RAM faut-il pour faire tourner OPNsense correctement ?**

4 Go suffisent pour un usage basique (pare-feu, DHCP, DNS). Comptez 8 Go minimum si vous activez Suricata en IDS/IPS, et 16 Go ou plus pour un usage combinant VPN, IDS et haute disponibilité.

**Peut-on installer OPNsense sur une machine virtuelle ?**

Oui, OPNsense fonctionne sans problème sous Proxmox, KVM ou VMware ESXi, à condition d’utiliser des pilotes réseau VirtIO pour de meilleures performances plutôt que l’émulation d’une carte réseau générique.

**OPNsense supporte-t-il IPv6 ?**

Oui, et le support IPv6 a été spécifiquement amélioré dans la série 26.7 sortie en juillet 2026, notamment pour la délégation de préfixe (DHCPv6-PD) reçue depuis certains fournisseurs d’accès.

**Faut-il des cartes réseau spécifiques ?**

OPNsense fonctionne avec la majorité des cartes réseau x86-64 reconnues par FreeBSD, mais les modèles Intel utilisant les pilotes igb, em ou ix offrent la meilleure stabilité et les meilleures performances, en particulier sous forte charge.

**Comment migrer une configuration pfSense existante vers OPNsense ?**

Il n’existe pas d’outil de migration automatique officiel entre les deux projets en raison de leur divergence de code depuis 2015. La reconstruction manuelle des règles de pare-feu et des services à partir d’une exportation de configuration reste la méthode recommandée par la communauté.

**À quelle fréquence faut-il mettre à jour OPNsense ?**

Le projet publie des versions mineures plusieurs fois par an (trois rien que pour la branche 26.7 entre juillet et août 2026). Une vérification hebdomadaire des mises à jour, et une application rapide après toute annonce de faille de sécurité sur le forum officiel, est recommandée.

### Related Coverage

Pour approfondir les sujets liés à la protection des infrastructures réseau, consultez notre dossier complet sur les menaces cybersécurité en 2026.

Sources externes : site officiel OPNsense, documentation de dimensionnement matériel, historique du projet sur Wikipedia, calendrier de support des versions, moteur de détection d’intrusion Suricata, protocole VPN WireGuard, et le CERT-FR de l’ANSSI pour les alertes de sécurité françaises.
