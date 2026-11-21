---
id: collect-261121-veille-ia/windows-os/wireshark-4-6-analyser-le-trafic-reseau-en-13-etapes
title: "Wireshark 4.6 : Analyser le Trafic Réseau en 13 Étapes"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["wireshark", "reseau", "trafic", "sous", "etapes", "sans", "apres", "capture"]
source: https://tech-insider.org/fr/wireshark-analyser-trafic-reseau-2026/
source_anchor: ""
source_lines: [1, 292]
section: "Windows_OS"
---
Un pare-feu bloque ce qu’il reconnaît. Un antivirus détecte ce qu’il a déjà catalogué. Mais quand un serveur rame sans raison apparente, qu’une application refuse de se connecter ou qu’un poste envoie des paquets vers une adresse inconnue à 3h du matin, un seul outil permet de voir ce qui circule vraiment sur le câble : Wireshark. Sorti en version 4.6.8 stable cet été, l’analyseur de paquets open source reste, vingt-huit ans après ses débuts sous le nom d’Ethereal, la référence pour quiconque doit diagnostiquer un réseau ou enquêter sur un incident. Ce tutoriel détaille l’installation, la capture, le filtrage et l’analyse de trafic en 13 étapes concrètes, avec des exemples de commandes, un projet complet de détection de brute-force SSH, et les pièges qui font perdre des heures aux débutants.

## Pourquoi apprendre Wireshark en 2026

Le 1er septembre 2026, le CERT-FR (rattaché à l’ANSSI) publiait encore cinq avis de sécurité en une seule journée, portant sur SPIP, Kaspersky Endpoint Security Windows et JFrog Artifactory, entre autres. La veille, le portail cyberveille.esante.gouv.fr recensait des failles critiques activement exploitées sur PaperCut (CVSS 9,1) et Gitea (CVSS 9,8), ainsi que des vulnérabilités notées 9,8 sur Redis et WatchGuard. Ce rythme n’est pas une anomalie : c’est le quotidien de la cybersécurité en France depuis le lancement de la stratégie nationale 2026-2030, qui pousse les entreprises et les administrations à renforcer la surveillance de leurs infrastructures exposées, notamment via le portail de signalement 17Cyber.gouv.fr.

Un scanner de vulnérabilités comme OpenVAS/Greenbone ou un IDS comme Suricata repèrent des signatures connues. Wireshark, lui, montre le trafic brut, sans filtre ni interprétation automatique. C’est justement ce qui en fait un complément indispensable : quand une alerte tombe, c’est souvent une capture Wireshark qui permet de confirmer si l’incident est réel, d’identifier la machine compromise, ou de comprendre pourquoi une application tierce échoue silencieusement. Les administrateurs réseau, les étudiants en cybersécurité, les développeurs qui débuggent une API et les analystes en réponse à incident l’utilisent tous, pour des raisons différentes mais avec les mêmes commandes de base.

## Qu’est-ce que Wireshark et à quoi ça sert vraiment

Wireshark est un analyseur de protocoles réseau open source sous licence GPLv2, disponible sur Windows, macOS, Linux et BSD. Il capture le trafic qui transite par une interface réseau, puis le décode paquet par paquet jusqu’au dernier octet, pour des centaines de protocoles (Ethernet, IP, TCP, UDP, HTTP, DNS, TLS, SMB, et bien d’autres). Le projet, fondé par Gerald Combs en 1998 sous le nom d’Ethereal, a été renommé Wireshark en 2006 après un différend avec un ancien employeur sur la marque. Il tourne aujourd’hui sous la Wireshark Foundation et reste maintenu activement, avec une branche stable (4.6.8) et une ancienne branche de maintenance (4.4.18) toujours supportée pour les environnements qui ne peuvent pas migrer immédiatement.

La documentation officielle est claire sur ce que l’outil n’est pas : “Wireshark isn’t an intrusion detection system. It will not warn you when someone does strange things on your network that he/she isn’t allowed to do.” (Wireshark n’est pas un système de détection d’intrusion. Il ne vous avertira pas si quelqu’un fait des choses anormales sur votre réseau sans y être autorisé), précise le guide utilisateur officiel. Le rôle de Wireshark commence après l’alerte, pas avant. Un forum communautaire officiel résume la nuance en réponse à la question de savoir si l’outil peut faire office d’IDS : “It would come into play once IDS alarms have been raised and network logs have been preserved to investigate the occurrence in more detail” (il entre en jeu une fois que des alertes IDS ont été déclenchées et que les journaux réseau ont été conservés pour approfondir l’investigation), selon une réponse publiée sur le forum d’assistance Wireshark. En clair : Suricata ou Wazuh sonnent l’alarme, Wireshark sert à comprendre ce qui s’est réellement passé.

Les usages concrets couvrent quatre familles : le diagnostic réseau classique (latence, pertes de paquets, mauvaise configuration DNS ou DHCP), le débogage applicatif (une API qui timeout, une requête HTTP mal formée, un certificat TLS invalide), la formation et la pédagogie (comprendre comment fonctionne réellement un handshake TCP ou une résolution DNS), et l’investigation de sécurité (confirmer une exfiltration de données, analyser un fichier PCAP fourni par un client lors d’un test d’intrusion, ou documenter une preuve pour un rapport d’incident).

## Prérequis : versions, matériel et droits nécessaires

Avant de commencer, vérifiez que votre environnement correspond aux versions actuelles. Wireshark évolue vite côté sécurité : les anciennes versions 3.x ne reçoivent plus de correctifs et embarquent des bibliothèques de décodage obsolètes, potentiellement vulnérables.

| Composant | Version recommandée (sept. 2026) | Rôle | Remarque | 
|---|---|---|---|
| Wireshark | 4.6.8 (stable) ou 4.4.18 (maintenance) | Interface graphique et moteur de décodage | Évitez toute version antérieure à 4.4 | 
| Npcap | 1.88 | Pilote de capture pour Windows | Remplace WinPcap, abandonné depuis 2013 | 
| libpcap | Fournie par la distribution Linux/macOS | Capture bas niveau sur Unix | Installée via le paquet système, pas de téléchargement séparé | 
| TShark | Identique à Wireshark | Version ligne de commande | Installée avec le paquet principal | 
| Espace disque | 10 Go minimum recommandés | Stockage des captures | Un lien 1 Gbit/s saturé génère plusieurs Go/minute | 
| Droits système | Administrateur (Windows) ou groupe “wireshark” (Linux) | Accès à l’interface réseau en mode promiscuous | Ne jamais lancer Wireshark en root en permanence | 

Sur le plan matériel, un PC de bureau standard suffit largement : Wireshark n’a pas besoin de GPU ni de RAM excessive pour du trafic courant. La contrainte vient surtout du stockage et du CPU quand on analyse des captures de plusieurs gigaoctets avec des filtres complexes. Côté réseau, capturer sur un switch nécessite soit un accès physique à un port mirroring (SPAN), soit une capture directe sur la machine cible : sur un réseau commuté moderne, vous ne voyez par défaut que votre propre trafic, pas celui du voisin.

Dernier prérequis, non technique celui-là : l’autorisation. Capturer du trafic sur un réseau qui ne vous appartient pas, ou sans le consentement des personnes concernées, expose à des poursuites au titre de l’article 226-15 du Code pénal (atteinte au secret des correspondances) et du RGPD si des données personnelles sont interceptées. Ce tutoriel part du principe que vous travaillez sur votre propre réseau, un labo isolé, ou dans le cadre d’un mandat de test d’intrusion écrit.

## Étapes 1 à 3 : installer Wireshark 4.6 et configurer la capture

### Étape 1 : télécharger et installer Wireshark

Téléchargez toujours Wireshark depuis le site officiel, jamais depuis un site tiers ou un lien raccourci : un faux installeur de Wireshark est un vecteur classique de malware, précisément parce que l’outil inspire confiance. Sur Windows, l’installeur .exe propose d’office l’installation de Npcap. Sur macOS, privilégiez le paquet .dmg officiel ou `brew install wireshark` si vous utilisez Homebrew. Sur Debian, Ubuntu et dérivés, la commande est directe :

```
sudo apt update
sudo apt install wireshark tshark
# Sur Fedora / RHEL / Rocky Linux
sudo dnf install wireshark wireshark-cli
# Sur Arch Linux
sudo pacman -S wireshark-qt wireshark-cli
```
Sur Debian et Ubuntu, l’installateur demande si les utilisateurs non root doivent pouvoir capturer des paquets. Répondez “Oui” uniquement si vous comprenez l’implication : cela crée le groupe `wireshark` avec des droits élevés sur l’interface de capture. Vous configurerez ce point précisément à l’étape suivante.

### Étape 2 : installer Npcap ou configurer libpcap sans lancer Wireshark en root

Sous Windows, Npcap 1.88 s’installe généralement en même temps que Wireshark. Si ce n’est pas le cas, téléchargez-le séparément depuis npcap.com, le projet maintenu par l’équipe Nmap qui a repris le flambeau de WinPcap. Cochez l’option “Support raw 802.11 traffic” uniquement si vous comptez analyser du Wi-Fi en mode moniteur, et laissez l’option “Restrict Npcap driver’s access to Administrators only” décochée pour permettre à un utilisateur standard membre du bon groupe de capturer sans élévation permanente.

Sous Linux, la bonne pratique consiste à ajouter votre utilisateur au groupe `wireshark` plutôt que de lancer l’application avec `sudo` à chaque fois, ce qui exposerait l’intégralité de l’interface graphique (et ses bibliothèques de décodage) avec les droits root :

```
sudo usermod -aG wireshark $USER
# Déconnexion/reconnexion nécessaire pour appliquer le groupe
# Vérifier les permissions sur dumpcap
getcap /usr/bin/dumpcap
# Résultat attendu : cap_net_admin,cap_net_raw+eip
```
Si `getcap` ne renvoie rien, les capacités n’ont pas été correctement attribuées lors de l’installation. Corrigez-le manuellement avec `sudo setcap cap_net_raw,cap_net_admin=eip /usr/bin/dumpcap`. Cette approche limite l’élévation de privilèges au seul binaire `dumpcap`, celui qui capture réellement les paquets, pendant que l’interface graphique tourne avec vos droits normaux.

### Étape 3 : choisir la bonne interface réseau

Au lancement, l’écran d’accueil de Wireshark liste toutes les interfaces disponibles avec un mini-graphique d’activité en temps réel : Ethernet, Wi-Fi, interfaces virtuelles VPN, boucle locale (loopback), et parfois des interfaces Bluetooth ou USB. Sur un poste connecté en filaire, choisissez l’interface Ethernet active (celle qui affiche du trafic). Sur un serveur avec plusieurs cartes réseau, identifiez la bonne interface via `ip addr` (Linux) ou `ipconfig` (Windows) avant de lancer la capture, pour éviter de scruter une interface inactive pendant dix minutes sans rien voir passer.

## Étapes 4 et 5 : première capture et prise en main de l’interface

### Étape 4 : lancer votre première capture de paquets

Double-cliquez sur l’interface choisie, ou sélectionnez-la puis cliquez sur l’icône en forme de requin bleu. Le trafic commence à défiler immédiatement. Pour un premier test, ouvrez un navigateur et visitez un site quelconque : vous verrez apparaître des requêtes DNS, un handshake TCP, puis du trafic TLS. Cliquez sur le carré rouge pour arrêter la capture une fois que vous avez quelques centaines de paquets, largement suffisant pour s’exercer.

Sauvegardez immédiatement cette première capture via Fichier > Enregistrer sous, au format .pcapng (le format natif de Wireshark, qui conserve les métadonnées et les commentaires, contrairement à l’ancien .pcap). Prenez l’habitude de nommer vos fichiers avec la date et le contexte, par exemple `2026-09-01_test-dns.pcapng`, cela évite de se perdre dans un dossier rempli de fichiers nommés `capture1`, `capture2`.

### Étape 5 : comprendre l’interface en trois volets

L’écran principal se divise en trois zones empilées. En haut, la liste des paquets (Packet List) affiche une ligne par paquet avec le numéro, l’horodatage, les adresses source et destination, le protocole détecté et une description résumée. Au milieu, les détails du paquet (Packet Details) montrent la structure en couches : trame Ethernet, en-tête IP, en-tête TCP ou UDP, puis le protocole applicatif, chaque couche pouvant être dépliée pour voir chaque champ individuellement. En bas, les octets bruts (Packet Bytes) affichent l’hexadécimal et sa représentation ASCII, avec surbrillance synchronisée : cliquez sur un champ dans les détails, et l’octet correspondant s’illumine en bas.

Les couleurs de fond dans la liste de paquets ne sont pas décoratives : le vert clair signale généralement du TCP standard, le bleu du DNS, le noir avec du rouge des paquets marqués comme erreurs ou retransmissions par le moteur d’analyse d’expert (Expert Info). Ces règles de coloration, personnalisables via Affichage > Règles de coloration, permettent de repérer un problème d’un coup d’œil avant même de lire le détail.

## Étape 6 : appliquer un filtre de capture (BPF)

Sur un serveur ou un lien saturé, capturer tout le trafic sans filtre génère des fichiers énormes et noie l’information utile. Les filtres de capture s’appliquent avant l’enregistrement, au niveau du pilote (BPF, Berkeley Packet Filter), et rejettent les paquets non voulus avant même qu’ils touchent le disque. Ils se saisissent dans le champ “Capture Filter” de l’écran de démarrage, avec une syntaxe différente de celle des filtres d’affichage.

```
# Ne capturer que le trafic HTTP et HTTPS
port 80 or port 443
# Ne capturer que le trafic vers ou depuis une machine précise
host 192.168.1.50
# Exclure tout le trafic SSH pour ne pas polluer une capture de debug distant
not port 22
# Ne capturer que le trafic d'un sous-réseau donné
net 10.0.0.0/24
# Combiner : trafic HTTP uniquement depuis un hôte donné
host 192.168.1.50 and port 80
```
Un filtre de capture mal écrit ne provoque pas d’erreur visible : Wireshark capture simplement zéro paquet, ou tout le trafic si la syntaxe est invalide et silencieusement ignorée selon la plateforme. Testez toujours un filtre de capture sur quelques secondes avant de lancer un enregistrement de plusieurs heures.

## Étape 7 : maîtriser les filtres d’affichage

Les filtres d’affichage, eux, s’appliquent après la capture, sur des paquets déjà enregistrés. Ils utilisent une syntaxe propre à Wireshark, bien plus riche que celle des filtres de capture, et se saisissent dans la barre verte en haut de la fenêtre principale (elle devient rouge si la syntaxe est invalide). C’est l’outil que vous utiliserez 90 % du temps.

```
# Isoler tout le trafic HTTP
http
# Isoler les requêtes DNS uniquement (pas les réponses)
dns.flags.response == 0
# Trouver les paquets marqués comme problématiques par Wireshark
tcp.analysis.retransmission or tcp.analysis.duplicate_ack
# Filtrer par adresse IP source
ip.src == 10.0.0.15
# Filtrer les échecs de connexion TCP (paquets RST)
tcp.flags.reset == 1
# Repérer un scan de ports classique (paquets SYN sans réponse)
tcp.flags.syn == 1 and tcp.flags.ack == 0
# Filtrer le trafic TLS et afficher la version négociée
tls.handshake.type == 1
```
Exemple de sortie attendue après avoir tapé `tcp.analysis.retransmission` sur une capture avec des soucis réseau : la liste se réduit d’un coup à une poignée de lignes surlignées en noir, chacune indiquant “[TCP Retransmission]” dans la colonne Info, avec le numéro de séquence dupliqué visible dans les détails du paquet. C’est exactement ce type de filtre qui permet de prouver, en quelques secondes, qu’un problème de lenteur applicative vient du réseau et non du code.

Astuce pour les débutants : clic droit sur n’importe quel champ dans le volet des détails, puis “Apply as Filter” > “Selected”, génère automatiquement la syntaxe correcte. C’est la méthode la plus rapide pour apprendre le langage des filtres sans mémoriser la documentation.

## Étape 8 : suivre un flux TCP, HTTP ou TLS

Un flux réseau se compose de dizaines, parfois de milliers de paquets individuels. Reconstituer une conversation complète paquet par paquet serait ingérable. La fonction “Follow Stream” (clic droit sur un paquet > Follow > TCP Stream, ou HTTP Stream, ou TLS Stream) réassemble automatiquement tous les paquets d’une même connexion et les affiche comme une conversation lisible, avec le trafic client en rouge et le trafic serveur en bleu.

Pour du trafic HTTP en clair, cette vue affiche directement les en-têtes de requête, le corps de la réponse, les cookies, tout en texte lisible. Pour du trafic chiffré en TLS, le contenu applicatif reste illisible par défaut : Wireshark voit les paquets, mais pas leur contenu déchiffré. Pour déboguer une application qui utilise HTTPS, la méthode standard consiste à exporter la clé de session TLS depuis le navigateur ou l’application, via la variable d’environnement `SSLKEYLOGFILE`, puis à la charger dans Wireshark.

```
# Sous Linux/macOS, avant de lancer le navigateur
export SSLKEYLOGFILE=~/tls-keys.log
firefox &
# Sous Windows PowerShell
$env:SSLKEYLOGFILE = "C:\Users\vous\tls-keys.log"
# Puis dans Wireshark :
# Édition > Préférences > Protocols > TLS
# Champ "(Pre)-Master-Secret log filename" -> pointer vers tls-keys.log
```
Une fois ce fichier chargé, Wireshark déchiffre le trafic TLS à la volée pour les sessions capturées après la création du fichier de clés, ce qui permet de voir le contenu HTTP réel derrière le HTTPS, sans jamais casser le chiffrement lui-même : la technique fonctionne uniquement parce que vous détenez déjà la clé côté client, légitimement.

## Étape 9 : lire les statistiques réseau

Le menu Statistiques regroupe des vues agrégées qui évitent de parcourir manuellement des milliers de lignes. Trois d’entre elles sont incontournables au quotidien. “Protocol Hierarchy” affiche la répartition du trafic capturé par protocole, en pourcentage et en nombre de paquets : utile pour repérer en un coup d’œil qu’un poste envoie 40 % de son trafic en DNS, ce qui est anormal et évoque potentiellement de l’exfiltration par tunneling DNS.

“Conversations” liste chaque paire source/destination avec le volume échangé, la durée et le débit, triable par colonne : la méthode la plus rapide pour identifier quelle machine consomme le plus de bande passante sur un lien saturé. “Expert Information” agrège automatiquement tous les paquets que le moteur de Wireshark juge suspects ou problématiques (retransmissions, fenêtres TCP à zéro, checksums invalides), classés par sévérité (Chat, Note, Warning, Error), ce qui donne un point de départ immédiat sur une capture inconnue de plusieurs milliers de paquets.

Le graphique “IO Graph” (Statistiques > IO Graph) trace le débit dans le temps et permet de superposer plusieurs filtres sur la même courbe, par exemple le trafic total contre le seul trafic d’erreur, pour visualiser instantanément à quel moment précis un incident a commencé.

## Étape 10 : repérer les anomalies et signes d’intrusion

Wireshark ne génère pas d’alerte automatique, mais certains motifs de trafic sont des signaux classiques une fois qu’on sait où regarder. Le tableau suivant liste les cas les plus fréquents rencontrés en analyse post-incident.

| Signe observé | Filtre d’affichage utile | Interprétation possible | 
|---|---|---|
| Rafale de paquets SYN vers des ports variés depuis une même source | `tcp.flags.syn==1 and tcp.flags.ack==0` | Scan de ports (type Nmap) | 
| Réponses ARP multiples pour une même IP avec des adresses MAC différentes | `arp.duplicate-address-detected` | ARP spoofing / attaque de l’homme du milieu | 
| Volume anormal de requêtes DNS TXT ou de sous-domaines aléatoires longs | `dns.qry.type==16` | Tunneling DNS / exfiltration de données | 
| Multiples tentatives d’authentification SSH depuis une même IP en quelques secondes | `tcp.port==22 and tcp.flags.syn==1` | Brute-force SSH | 
| Connexions sortantes régulières vers une IP externe à intervalles fixes | `ip.addr==X.X.X.X` + tri par temps | Balise de commande et contrôle (C2 beaconing) | 
| Certificat TLS auto-signé ou domaine récemment enregistré | `tls.handshake.type==11` | Serveur de phishing ou infrastructure malveillante | 

Aucun de ces motifs n’est une preuve à lui seul : un pic de requêtes SSH peut être un script légitime de supervision, un certificat auto-signé peut appartenir à un service interne. L’intérêt de Wireshark est de fournir la preuve matérielle qui confirme ou infirme une alerte déjà remontée par un IDS comme Suricata, un SIEM comme Wazuh, ou un outil de blocage comportemental comme CrowdSec.

## Étapes 11 à 13 : automatiser avec TShark, exporter et respecter le cadre légal

### Étape 11 : automatiser l’analyse avec TShark

TShark est la version ligne de commande de Wireshark, installée avec le même paquet. Elle partage le même moteur de décodage et la même syntaxe de filtres, ce qui la rend idéale pour scripter des captures sur un serveur sans interface graphique, ou pour intégrer une extraction de données dans un pipeline d’automatisation.

```
# Capturer 500 paquets sur l'interface eth0 et sauvegarder
tshark -i eth0 -c 500 -w capture.pcapng
# Lire un fichier existant et n'afficher que le trafic HTTP
tshark -r capture.pcapng -Y "http"
# Extraire uniquement les adresses IP source et destination, format CSV
tshark -r capture.pcapng -T fields -e ip.src -e ip.dst -E separator=,
# Capturer en continu avec rotation de fichiers (utile en production)
tshark -i eth0 -b filesize:100000 -b files:10 -w /var/log/pcap/rotation.pcapng
```
La rotation de fichiers (`-b filesize:100000 -b files:10`) évite de saturer le disque sur un serveur en capture permanente : ici, dix fichiers de 100 Mo maximum, les plus anciens étant écrasés automatiquement. C’est la configuration de base pour un poste de capture longue durée destiné à l’investigation a posteriori.

### Étape 12 : exporter et documenter une capture

Pour un rapport d’incident ou une preuve destinée à un client, exportez toujours au format .pcapng plutôt qu’en copie d’écran : le fichier brut permet à un tiers de rejouer votre analyse et de vérifier vos conclusions. Fichier > Exporter des objets permet aussi d’extraire directement les fichiers transférés en clair (images, documents, exécutables) détectés dans une capture HTTP ou SMB, une fonction régulièrement utilisée en réponse à incident pour récupérer un malware capturé en transit.

Ajoutez systématiquement un commentaire de capture (Édition > Commentaire de paquet, ou Statistiques > Résumé de capture) précisant le contexte : date, machine, objectif de la capture, autorisation associée. Un fichier .pcapng sans contexte perd toute valeur probante six mois plus tard.

### Étape 13 : respecter le cadre légal en France

Trois règles simples évitent les ennuis. D’abord, ne capturez jamais de trafic sur un réseau qui ne vous appartient pas sans autorisation écrite, qu’il s’agisse d’un client, d’un employeur ou d’un réseau public. Ensuite, si la capture peut contenir des données personnelles (identifiants, contenus de messages, cookies de session), traitez le fichier .pcapng comme une donnée sensible au sens du RGPD : chiffrement au repos, durée de conservation limitée, accès restreint. Enfin, sur un poste professionnel, informez généralement les utilisateurs concernés ou appuyez-vous sur la politique de sécurité de l’entreprise, en lien avec le délégué à la protection des données. Ces principes rejoignent ceux déjà détaillés dans les guides consacrés au scan réseau avec Nmap ou à la mise en conformité NIS2.

## Projet complet : détecter un brute-force SSH de bout en bout

Ce projet reproduit un scénario réaliste sur un labo isolé (deux machines virtuelles sur un réseau privé, jamais sur une infrastructure de production). L’objectif : capturer une tentative de brute-force SSH, la confirmer dans Wireshark, puis l’automatiser via TShark pour une détection reproductible.

Sur la machine cible, lancez la capture ciblée sur le port 22 :

`tshark -i eth0 -f "tcp port 22" -w ssh-bruteforce.pcapng`
Depuis la machine “attaquante” du labo, générez du trafic de test avec un outil comme Hydra ou un simple script de connexions SSH répétées (uniquement dans ce labo isolé, jamais contre une cible tierce). Après quelques dizaines de tentatives, arrêtez la capture et ouvrez le fichier dans Wireshark. Appliquez le filtre d’affichage suivant pour isoler les tentatives de connexion :

`tcp.port==22 and tcp.flags.syn==1 and tcp.flags.ack==0`
Résultat attendu : une succession de paquets SYN depuis la même adresse IP source, espacés de quelques centaines de millisecondes à peine, chacun suivi d’un échange TCP complet puis d’une clôture rapide (paquets FIN ou RST) une fois l’authentification SSH refusée. Un utilisateur humain qui se trompe de mot de passe génère au maximum trois à cinq tentatives par minute. Un script en génère des dizaines par seconde : la différence de fréquence est le signal le plus fiable, bien plus que le contenu chiffré lui-même, invisible côté SSH.

Pour automatiser ce comptage sans ouvrir l’interface graphique à chaque fois, un script TShark suffit :

```
tshark -r ssh-bruteforce.pcapng \
  -Y "tcp.port==22 and tcp.flags.syn==1 and tcp.flags.ack==0" \
  -T fields -e ip.src -e frame.time_relative \
  | awk '{print $1}' | sort | uniq -c | sort -rn
# Exemple de sortie :
#   47 192.168.56.20
#    2 192.168.56.30
```
Une adresse avec 47 tentatives SYN en quelques secondes, contre 2 pour une autre (probablement un test légitime), confirme sans ambiguïté la nature de l’attaque. Dans un contexte réel, ce script s’intègre à un cron ou à un pipeline de supervision qui alerte au-delà d’un seuil défini, par exemple 10 tentatives par minute et par IP, en complément d’un blocage automatique via Fail2ban.

## 5 erreurs fréquentes à éviter

- **Capturer sans filtre sur un lien saturé.** Une capture de dix minutes sans filtre sur un serveur à fort trafic peut générer plusieurs gigaoctets et rendre l’analyse dans l’interface graphique quasi impossible. Filtrez toujours à la capture, pas seulement à l’affichage.
- **Lancer Wireshark en root en permanence sous Linux.** C’est le raccourci le plus tentant et le plus risqué : toute faille dans le moteur de décodage (et il y en a eu, historiquement) s’exécute alors avec les droits root. Configurez le groupe`wireshark` une bonne fois pour toutes.
- **Confondre filtre de capture et filtre d’affichage.** Taper une syntaxe de filtre d’affichage (comme`tcp.port==80` ) dans le champ de capture, ou l’inverse, provoque une erreur ou un filtre silencieusement ignoré. Les deux langages ne sont pas interchangeables.
- **Ignorer les colonnes d’heure relative.** Par défaut, Wireshark affiche l’heure depuis le début de la capture, pas l’heure absolue. Pour corréler avec des logs serveur, basculez sur Affichage > Format d’heure > Date et heure du jour.
- **Analyser du trafic TLS sans exporter les clés de session au préalable.** Il est impossible de déchiffrer une capture TLS après coup sans avoir configuré`SSLKEYLOGFILE` avant la capture : pensez-y en amont, pas après avoir fermé le navigateur.

## Dépannage : 8 problèmes courants et leurs solutions

| Problème | Cause probable | Solution | 
|---|---|---|
| “You don’t have permission to capture” au démarrage | Droits insuffisants sur l’interface ou le pilote de capture | Ajouter l’utilisateur au groupe wireshark (Linux) ou réinstaller Npcap avec les droits par défaut (Windows) | 
| Aucune interface ne s’affiche dans la liste | Npcap ou libpcap non installé, ou service arrêté | Réinstaller Npcap, ou vérifier `systemctl status NetworkManager` sur Linux | 
| La capture reste vide malgré du trafic actif | Mauvaise interface sélectionnée, ou filtre de capture trop restrictif | Vérifier avec `ip addr` quelle interface est active, retirer temporairement le filtre | 
| Filtre d’affichage en rouge, refusé | Erreur de syntaxe (opérateur, nom de champ) | Utiliser l’autocomplétion intégrée ou “Apply as Filter” depuis un paquet existant | 
| Impossible de voir le trafic d’une autre machine sur le switch | Réseau commuté moderne : chaque port ne voit que son propre trafic | Configurer un port mirroring (SPAN) sur le switch, ou capturer directement sur la machine cible | 
| Trafic TLS toujours chiffré malgré SSLKEYLOGFILE configuré | Variable définie après le lancement de l’application, ou mauvais chemin de fichier | Redémarrer l’application après avoir exporté la variable, vérifier le chemin dans Préférences > TLS | 
| Wireshark se fige ou consomme toute la RAM sur une grosse capture | Fichier de plusieurs Go ouvert directement dans l’interface graphique | Découper le fichier avec `editcap -c 100000 gros.pcapng partie.pcapng` ou filtrer via TShark en amont | 
| Les noms de domaine ne s’affichent pas, seulement des IP | Résolution de noms désactivée pour limiter les requêtes DNS parasites | Activer temporairement Affichage > Résolution de noms, en sachant que cela génère du trafic DNS supplémentaire | 

## Astuces avancées pour aller plus loin

Une fois les bases maîtrisées, plusieurs fonctions accélèrent nettement le travail d’analyse au quotidien. Les colonnes personnalisées (clic droit sur un champ > “Apply as Column”) permettent d’afficher directement dans la liste principale une information précise, par exemple le code de statut HTTP ou la durée de round-trip TCP, sans avoir à déplier les détails de chaque paquet. Les profils de configuration (Édition > Profils de configuration) autorisent de basculer instantanément entre un jeu de colonnes et de filtres adapté au débogage web et un autre adapté à l’analyse d’intrusion, sans reconfigurer l’interface à chaque fois.

Pour le trafic USB, Wireshark peut capturer directement les échanges d’un périphérique via le module USBPcap (Windows) ou usbmon (Linux), utile pour analyser un keylogger matériel suspect ou déboguer un firmware. Pour le Wi-Fi, le mode moniteur (nécessitant une carte compatible et, sous Linux, l’outil `airmon-ng`) permet de capturer les trames 802.11 brutes, y compris les paquets de gestion, avant même l’association à un point d’accès.

Enfin, l’outil `editcap`, fourni avec Wireshark, permet de découper, fusionner ou anonymiser des captures en ligne de commande, une étape souvent nécessaire avant de partager un fichier .pcapng avec un tiers dans le cadre d’un support technique, sans exposer d’adresses IP ou de données internes sensibles.

## Wireshark face aux autres outils d’analyse réseau

| Outil | Type | Interface | Cas d’usage principal | 
|---|---|---|---|
| Wireshark | Analyseur de paquets | Graphique (GTK/Qt) | Analyse détaillée manuelle, décodage protocolaire | 
| TShark | Analyseur de paquets | Ligne de commande | Automatisation, capture sur serveur distant | 
| tcpdump | Capteur de paquets | Ligne de commande | Capture rapide et légère, sans décodage avancé | 
| Suricata | IDS/IPS | Ligne de commande + règles | Détection automatique de signatures et blocage temps réel | 
| Nmap | Scanner réseau | Ligne de commande | Découverte d’hôtes et de ports, pas d’analyse de flux | 

Ces outils ne sont pas concurrents mais complémentaires. Sur un poste de travail, Nmap découvre ce qui existe sur le réseau, Wireshark montre ce qui y circule réellement, et Suricata ou Wazuh alertent en continu pendant que personne ne regarde l’écran. La combinaison des trois couvre l’essentiel des besoins d’un administrateur système ou d’un analyste SOC débutant, sans nécessiter de budget logiciel : les cinq outils du tableau sont open source et gratuits.

## Questions fréquentes

### Wireshark est-il légal à utiliser en France ?

Oui, le logiciel lui-même est parfaitement légal et téléchargeable librement. Ce qui peut poser problème, c’est l’usage : capturer du trafic sur un réseau qui ne vous appartient pas, sans autorisation, expose à des poursuites au titre de l’atteinte au secret des correspondances (article 226-15 du Code pénal). Sur votre propre réseau domestique ou professionnel, avec l’accord de l’employeur le cas échéant, aucun problème.

### Wireshark peut-il déchiffrer n’importe quel trafic HTTPS ?

Non. Wireshark ne casse jamais le chiffrement TLS. Il peut afficher le contenu déchiffré uniquement si vous fournissez la clé de session, généralement via la variable `SSLKEYLOGFILE` exportée avant la capture depuis une machine où vous avez déjà un accès légitime. Sans cette clé, le trafic TLS reste illisible, ce qui est le comportement attendu et voulu.

### Quelle est la différence entre Wireshark et Suricata ?

Wireshark est un outil d’analyse manuelle : vous ouvrez une capture et vous cherchez vous-même les anomalies. Suricata est un système de détection et de prévention d’intrusion (IDS/IPS) qui tourne en continu et déclenche des alertes automatiques selon des règles prédéfinies. En pratique, Suricata alerte, Wireshark permet d’investiguer en détail ce qui a déclenché l’alerte.

### Faut-il des connaissances réseau avancées pour démarrer ?

Des bases sur le modèle TCP/IP (adresses IP, ports, notion de paquet) suffisent pour commencer à utiliser l’interface et les filtres simples. La compréhension fine des handshakes TCP, des en-têtes TLS ou des mécanismes DNS s’acquiert progressivement, en pratiquant directement sur des captures réelles plutôt qu’en lisant uniquement de la théorie.

### Pourquoi ma capture ne montre-t-elle pas le trafic d’un autre appareil sur mon réseau Wi-Fi ?

Sur un réseau moderne, filaire ou Wi-Fi, chaque appareil ne reçoit par défaut que son propre trafic. Pour voir le trafic d’un autre appareil, il faut soit un accès physique à un port mirroring sur le switch, soit une carte Wi-Fi compatible avec le mode moniteur, soit une capture directe sur l’appareil cible avec son autorisation.

### Wireshark ralentit-il la machine ou le réseau pendant une capture ?

L’impact reste minime sur un poste ou un serveur avec des ressources normales : la capture consomme surtout du CPU pour le décodage en temps réel si l’interface graphique reste ouverte, et de l’espace disque pour l’enregistrement. Sur un lien très chargé, il est préférable d’utiliser TShark plutôt que l’interface graphique, et de filtrer à la capture pour limiter la charge.

### Quelle est la différence entre le format .pcap et .pcapng ?

Le format .pcap est l’ancien format historique, plus simple mais limité : il ne conserve pas de métadonnées comme les commentaires ou les informations d’interface multiples. Le format .pcapng, adopté par défaut depuis plusieurs versions de Wireshark, permet de stocker plusieurs interfaces dans un seul fichier, d’ajouter des commentaires et de mieux gérer les captures de grande taille. Sauf contrainte de compatibilité avec un outil tiers ancien, utilisez toujours .pcapng.

### Peut-on utiliser Wireshark sur un Raspberry Pi ou un NAS ?

TShark s’installe sans difficulté sur un Raspberry Pi ou un NAS sous Linux, pour de la capture légère en ligne de commande. L’interface graphique complète de Wireshark fonctionne aussi mais reste plus confortable sur un poste avec davantage de RAM, surtout pour l’analyse de captures volumineuses. Une approche courante consiste à capturer sur le petit appareil avec TShark, puis à transférer le fichier .pcapng vers un poste plus puissant pour l’analyse détaillée dans l’interface graphique.
