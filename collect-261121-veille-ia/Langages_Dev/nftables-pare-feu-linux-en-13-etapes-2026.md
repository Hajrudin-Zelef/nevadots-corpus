---
id: collect-261121-veille-ia/langages-dev/nftables-pare-feu-linux-en-13-etapes-2026
title: "Nftables : Pare-feu Linux en 13 Étapes [2026]"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["nftables", "linux", "iptables", "reseau", "noyau", "moteur", "depuis", "etapes"]
source: https://tech-insider.org/fr/nftables-pare-feu-linux-2026/
source_anchor: ""
source_lines: [1, 384]
section: "Langages_Dev"
---
Un serveur Linux exposé sur Internet reçoit des tentatives de connexion non sollicitées en quelques minutes à peine après sa mise en ligne. Sans filtrage réseau correctement configuré, chaque port ouvert devient une porte d’entrée potentielle. C’est là qu’intervient nftables, le sous-système de filtrage de paquets qui a progressivement remplacé iptables dans le noyau Linux. Debian, Ubuntu, Fedora et Rocky Linux s’appuient désormais sur ce moteur en interne, même quand l’administrateur continue de taper des commandes iptables par habitude. Ce tutoriel montre comment configurer nftables depuis zéro, protéger un serveur SSH contre le brute-force, mettre en place du NAT, journaliser les paquets rejetés, puis migrer proprement depuis une ancienne configuration iptables. Treize étapes, un projet complet livré en fin d’article, et une liste de pièges qui font perdre des heures aux administrateurs pressés.

## Pourquoi nftables remplace iptables sur les serveurs Linux

iptables a longtemps été la référence pour filtrer le trafic réseau sous Linux. Le projet Netfilter, qui maintient ces outils, a introduit nftables pour corriger des limites structurelles devenues gênantes avec le temps : quatre outils séparés (iptables, ip6tables, arptables, ebtables), une syntaxe verbeuse, et des performances qui se dégradent avec des milliers de règles. nftables unifie tout cela dans un seul outil, `nft`, capable de gérer IPv4, IPv6, ARP et le pontage réseau avec une seule syntaxe cohérente.

Les distributions majeures ont basculé nftables comme moteur par défaut depuis plusieurs années déjà. Les commandes iptables classiques passent désormais par une couche de compatibilité, `iptables-nft`, qui traduit les anciennes règles vers le format nftables en coulisses. Autrement dit, même un administrateur qui n’a jamais tapé une commande `nft` utilise probablement déjà ce moteur sans le savoir. Le CERT-FR, l’organisme public français qui publie des bulletins de vulnérabilités quotidiens, rappelle régulièrement que le filtrage réseau reste une des premières lignes de défense contre l’exploitation de services exposés, notamment quand un correctif n’est pas encore disponible pour une faille annoncée.

L’ANSSI a également mis à jour sa politique open source en 2026, avec quatre axes qui incluent la publication de logiciels de cybersécurité sous licence libre et l’utilisation de solutions open source dans les infrastructures publiques. nftables, disponible dans le noyau Linux lui-même, coche cette case : pas de licence à acheter, pas de boîte noire, et un code audité par la communauté Netfilter depuis des années.

Au-delà de l’aspect réglementaire, l’argument technique reste le plus convaincant pour un administrateur qui gère plusieurs dizaines de serveurs. iptables devait dupliquer chaque règle IPv4 dans une commande ip6tables séparée pour couvrir IPv6, doublant mécaniquement le travail et le risque d’incohérence entre les deux jeux de règles. nftables résout ce problème avec la famille `inet`, qui applique une seule règle aux deux protocoles simultanément. Sur un parc de serveurs où IPv6 devient progressivement la norme chez les hébergeurs européens, cette unification change concrètement le temps passé à maintenir une configuration réseau cohérente.

## Prérequis : versions, distributions et compétences nécessaires

Avant de commencer, vérifiez que votre environnement correspond aux bases suivantes. nftables fonctionne sur toutes les distributions Linux modernes, mais les commandes d’installation et les chemins de fichiers varient légèrement d’une famille à l’autre.

| Élément | Version ou configuration recommandée | Remarque | 
|---|---|---|
| Distribution | Debian 12/13, Ubuntu 22.04 LTS et supérieur, Fedora récente, Rocky Linux 9 et supérieur | nftables est le moteur par défaut sur ces versions | 
| Noyau Linux | Série 5.x ou 6.x | Le support nftables complet nécessite un noyau relativement récent | 
| Paquet | nftables (dépôts officiels de chaque distribution) | Toujours installer depuis les dépôts, jamais depuis une source tierce non vérifiée | 
| Accès | Droits root ou sudo | Indispensable pour modifier les règles réseau | 
| Accès physique ou console | Recommandé pour un serveur distant | Une mauvaise règle peut couper l’accès SSH | 
| Connaissances | Bases de TCP/IP, ports, protocoles | Comprendre ce qu’on filtre évite les erreurs de configuration | 

Un point mérite d’être souligné avant de toucher à quoi que ce soit : sur un serveur distant accessible uniquement par SSH, une règle mal écrite peut couper la connexion et vous laisser dehors. Gardez toujours une session console ouverte (via l’hébergeur, une VM locale ou un accès KVM) pendant les premiers tests, et prévoyez un mécanisme de rollback automatique. Ce tutoriel montre comment faire les deux.

## Étape 1 : Vérifier le noyau et la compatibilité nftables

Commencez par confirmer que votre système prend en charge nftables nativement. La plupart des noyaux récents l’intègrent déjà, mais mieux vaut vérifier avant d’installer quoi que ce soit.

```
uname -r
grep -i nf_tables /boot/config-$(uname -r) 2>/dev/null
lsmod | grep nf_tables
```
Si la commande `lsmod` ne remonte rien, ce n’est pas forcément un problème : le module se charge automatiquement au premier usage de `nft`. Vérifiez aussi si iptables fonctionne déjà en mode legacy ou en mode nft sur votre machine, car les deux backends ne doivent jamais coexister sur les mêmes règles au risque de créer des conflits difficiles à diagnostiquer.

```
update-alternatives --display iptables 2>/dev/null
iptables --version
```
Une sortie mentionnant `nf_tables` à côté du numéro de version confirme que vous êtes déjà sur le bon backend. Sur Debian et Ubuntu récents, c’est le comportement par défaut depuis longtemps.

## Étape 2 : Installer nftables sur Debian, Ubuntu, Fedora et Rocky Linux

L’installation change peu d’une distribution à l’autre, mais quelques détails comptent, notamment la désactivation propre d’un ancien service iptables qui pourrait entrer en conflit.

```
# Debian / Ubuntu
sudo apt update
sudo apt install nftables -y
sudo systemctl enable --now nftables
# Fedora / RHEL / Rocky Linux
sudo dnf install nftables -y
sudo systemctl enable --now nftables
# Arch Linux
sudo pacman -S nftables
sudo systemctl enable --now nftables
```
Sur les systèmes qui utilisaient auparavant firewalld ou ufw comme frontend, désactivez-les avant de démarrer nftables directement, sous peine de voir deux gestionnaires de règles se marcher dessus. Un conflit entre firewalld et nftables actif en parallèle est l’une des causes les plus fréquentes de comportements imprévisibles signalées sur les forums d’administration système.

```
sudo systemctl disable --now firewalld
sudo systemctl disable --now ufw
```
Vérifiez ensuite que le service tourne et que le fichier de configuration principal existe déjà, généralement vide ou avec une politique par défaut minimale.

```
sudo systemctl status nftables
cat /etc/nftables.conf
```
## Étape 3 : Comprendre les tables, chaînes, règles et priorités

nftables organise le filtrage en trois niveaux hiérarchiques, et comprendre cette structure évite de nombreuses erreurs par la suite. Une table regroupe des chaînes par famille d’adresses (ip, ip6, inet pour les deux à la fois, arp, bridge). Une chaîne contient une liste ordonnée de règles et s’accroche à un point d’ancrage du noyau comme `input`, `output` ou `forward`. Une règle définit une condition de correspondance suivie d’une action, appelée verdict : accept, drop, reject, ou un renvoi vers une autre chaîne.

Contrairement à iptables qui imposait des tables figées (filter, nat, mangle), nftables laisse l’administrateur nommer ses propres tables et organiser sa logique comme il l’entend. Cette flexibilité est puissante mais demande de la rigueur : sans convention de nommage claire, une configuration complexe devient vite illisible pour la personne qui la reprendra six mois plus tard.

- **Table** : conteneur logique, par exemple`inet filtre` pour IPv4 et IPv6 combinés
- **Chaîne de base** : rattachée à un hook noyau (input, output, forward, prerouting, postrouting)
- **Chaîne régulière** : appelée depuis une autre chaîne via`jump` ou`goto` , utile pour modulariser
- **Priorité** : détermine l’ordre d’exécution quand plusieurs chaînes de base partagent le même hook
- **Politique** : verdict par défaut appliqué si aucune règle ne correspond (accept ou drop)

## Étape 4 : Créer une politique de base par défaut

La règle d’or en sécurité réseau reste la même depuis des décennies : tout refuser par défaut, puis autoriser explicitement ce qui est nécessaire. Créez une table et une chaîne d’entrée avec une politique drop.

```
sudo nft add table inet filtre
sudo nft add chain inet filtre entree { type filter hook input priority 0 \; policy drop \; }
sudo nft add chain inet filtre sortie { type filter hook output priority 0 \; policy accept \; }
sudo nft add chain inet filtre transit { type filter hook forward priority 0 \; policy drop \; }
```
À ce stade, si vous appliquez cette politique sans autoriser au préalable les connexions établies et le trafic de loopback, vous risquez de couper votre propre session SSH immédiatement. Ajoutez donc systématiquement ces deux règles en premier, avant toute politique restrictive.

```
sudo nft add rule inet filtre entree iif lo accept
sudo nft add rule inet filtre entree ct state established,related accept
sudo nft add rule inet filtre entree ct state invalid drop
```
## Étape 5 : Autoriser SSH, HTTP et HTTPS en entrée

Une fois la base posée, ouvrez uniquement les ports que votre serveur doit réellement exposer. Sur un serveur web classique, cela se limite souvent à trois ports.

```
sudo nft add rule inet filtre entree tcp dport 22 accept
sudo nft add rule inet filtre entree tcp dport { 80, 443 } accept
sudo nft add rule inet filtre entree ip protocol icmp accept
sudo nft add rule inet filtre entree ip6 nexthdr icmpv6 accept
```
Restreindre le port SSH à une plage d’adresses IP connues réduit encore davantage la surface d’attaque. Si votre équipe travaille depuis une IP fixe ou un VPN d’entreprise, verrouillez l’accès dès maintenant plutôt que d’ouvrir SSH au monde entier.

`sudo nft add rule inet filtre entree ip saddr 203.0.113.0/24 tcp dport 22 accept`
## Étape 6 : Limiter le débit et contrer le brute-force SSH

SSH exposé sur Internet reçoit des tentatives de connexion automatisées en continu, avec des scanners qui testent des milliers de combinaisons de mots de passe par heure. nftables propose un mécanisme de limitation de débit natif, sans dépendre d’un outil externe, même si le combiner avec Fail2ban ou CrowdSec reste recommandé pour une défense en profondeur.

```
sudo nft add rule inet filtre entree tcp dport 22 ct state new limit rate 4/minute accept
sudo nft add rule inet filtre entree tcp dport 22 ct state new log prefix "SSH-DROP: " drop
```
Cette paire de règles autorise au maximum quatre nouvelles connexions SSH par minute et journalise, avant de rejeter, toute tentative supplémentaire. Un utilisateur légitime qui tape mal son mot de passe deux ou trois fois ne sera jamais bloqué. Un script de brute-force qui tente des centaines de combinaisons par minute se heurte immédiatement au mur.

## Étape 7 : Configurer le NAT et le masquerading pour un routeur Linux

Si votre machine Linux fait office de routeur ou de passerelle pour un réseau local, nftables gère aussi la traduction d’adresses réseau (NAT). Cela concerne typiquement les box maison sous Linux, les hyperviseurs avec des VM en réseau privé, ou les serveurs qui partagent une connexion Internet.

```
sudo nft add table ip nat
sudo nft add chain ip nat postrouting { type nat hook postrouting priority 100 \; }
sudo nft add rule ip nat postrouting oif "eth0" masquerade
echo 'net.ipv4.ip_forward=1' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```
Sans activer `ip_forward` au niveau du noyau, aucune règle de NAT ne fonctionnera, même parfaitement écrite. C’est l’une des causes d’échec les plus fréquentes rencontrées par les administrateurs qui configurent leur premier routeur Linux.

## Étape 8 : Journaliser les paquets rejetés pour l’analyse forensique

Un pare-feu qui bloque silencieusement ne raconte aucune histoire. Journaliser les paquets rejetés permet de repérer des tentatives de scan, des attaques ciblées, ou simplement une mauvaise configuration côté client qui envoie du trafic inattendu.

`sudo nft add rule inet filtre entree log prefix "NFT-DROP: " flags all counter drop`
Placez cette règle en toute dernière position de la chaîne, juste avant la fin implicite. Les logs atterrissent dans `journalctl -k` ou `/var/log/kern.log` selon votre configuration syslog. Sur un serveur à fort trafic, limitez le volume de logs avec `limit rate` pour éviter de saturer le disque.

`sudo journalctl -k -f | grep NFT-DROP`
Exemple de sortie typique après une tentative de scan de port :

```
kernel: NFT-DROP: IN=eth0 OUT= MAC=... SRC=198.51.100.42 DST=203.0.113.10
LEN=40 TOS=0x00 PREC=0x00 TTL=48 ID=54321 PROTO=TCP SPT=51422 DPT=3389
WINDOW=1024 RES=0x00 SYN URGP=0
```
Cette ligne montre une tentative de connexion sur le port 3389 (RDP) depuis une adresse externe, un port que ce serveur Linux n’expose jamais. C’est exactement le type de bruit de fond qu’on observe sur n’importe quelle IP publique.

## Étape 9 : Simplifier les règles avec les sets et les verdict maps

Quand le nombre de règles grandit, les gérer une par une devient vite ingérable. nftables introduit les sets (ensembles) et les verdict maps, deux structures qui remplacent des dizaines de règles individuelles par une seule ligne dynamique.

```
# Un set nommé de ports autorisés, modifiable sans réécrire la règle
sudo nft add set inet filtre ports_autorises { type inet_service \; }
sudo nft add element inet filtre ports_autorises { 22, 80, 443, 8443 }
sudo nft add rule inet filtre entree tcp dport @ports_autorises accept
# Un set d'IP bannies, alimenté automatiquement par un script ou Fail2ban
sudo nft add set inet filtre ip_bannies { type ipv4_addr \; flags timeout \; }
sudo nft add rule inet filtre entree ip saddr @ip_bannies drop
```
Le second exemple, avec le flag `timeout`, permet d’ajouter dynamiquement une IP bannie pour une durée limitée sans jamais toucher aux règles elles-mêmes. C’est exactement le mécanisme qu’utilisent les outils comme CrowdSec ou Fail2ban en coulisses pour interagir avec nftables.

## Étape 10 : Rendre la configuration persistante avec systemd

Toutes les commandes tapées jusqu’ici disparaissent au redémarrage si elles ne sont pas sauvegardées dans un fichier de configuration. nftables charge automatiquement `/etc/nftables.conf` au démarrage via le service systemd activé à l’étape 2.

```
# Exporter la configuration active vers le fichier persistant
sudo nft list ruleset > /etc/nftables.conf
# Vérifier la syntaxe avant de redémarrer le service
sudo nft -c -f /etc/nftables.conf
# Recharger sans redémarrer le serveur
sudo systemctl restart nftables
```
L’option `-c` (check) valide la syntaxe du fichier sans l’appliquer. Prenez l’habitude de toujours passer par cette vérification avant un redémarrage du service : une erreur de syntaxe dans `/etc/nftables.conf` peut empêcher le service de démarrer, laissant potentiellement le serveur sans aucun filtrage actif.

## Étape 11 : Migrer depuis iptables avec la couche de compatibilité nft

Si vous gérez déjà un serveur avec des règles iptables historiques, il existe un chemin de migration qui évite de tout réécrire à la main. L’utilitaire `iptables-translate`, fourni avec le paquet nftables, convertit une règle iptables ligne par ligne vers sa syntaxe nft équivalente.

```
# Convertir une règle iptables existante
iptables-translate -A INPUT -p tcp --dport 22 -j ACCEPT
# Convertir un fichier iptables-save complet
iptables-restore-translate -f /etc/iptables/rules.v4
```
La traduction automatique fonctionne bien pour les règles simples, mais elle ne réorganise pas la logique en tables et sets optimisés propres à nftables. Pour une migration de production, considérez le résultat comme un premier jet à nettoyer, pas comme une configuration finale prête à déployer telle quelle.

## Étape 12 : Tester le pare-feu depuis l’extérieur avec nmap

Une configuration qui semble correcte en local peut se comporter différemment vue depuis Internet. Testez systématiquement depuis une machine externe, jamais uniquement depuis le serveur lui-même.

```
# Depuis une machine externe (pas le serveur lui-même)
nmap -Pn -p 22,80,443,3389,8080 203.0.113.10
```
Exemple de sortie attendue sur un serveur correctement filtré :

```
PORT     STATE    SERVICE
22/tcp   open     ssh
80/tcp   open     http
443/tcp  open     https
3389/tcp filtered ms-wbt-server
8080/tcp filtered http-proxy
Nmap done: 1 IP address (1 host up) scanned in 4.21 seconds
```
Les ports explicitement autorisés apparaissent en `open`, tandis que les ports non listés dans vos règles remontent en `filtered`, ce qui signifie que les paquets sont abandonnés sans réponse plutôt que rejetés activement. C’est le comportement attendu d’une politique drop bien configurée : un attaquant qui scanne votre serveur ne peut même pas distinguer un port fermé d’un port inexistant.

## Étape 13 : Superviser et auditer vos règles en continu

Une configuration figée le jour de sa création devient obsolète avec le temps : nouveaux services déployés, ports oubliés qui restent ouverts, règles temporaires jamais retirées. Prenez l’habitude d’auditer périodiquement l’état réel du pare-feu.

```
# Lister toutes les règles actives avec compteurs de paquets
sudo nft list ruleset -a
# Voir uniquement les compteurs pour repérer les règles jamais déclenchées
sudo nft -s list table inet filtre
```
Une règle avec un compteur à zéro après plusieurs semaines mérite d’être questionnée : sert-elle encore à quelque chose, ou peut-elle être supprimée pour alléger la configuration ? Des outils de supervision comme Wazuh peuvent aussi centraliser ces logs nftables pour une analyse de sécurité à l’échelle de tout un parc de serveurs.

## Projet complet : fichier nftables.conf prêt pour la production

Voici l’assemblage complet des règles vues dans ce tutoriel, prêt à copier dans `/etc/nftables.conf` sur un serveur web type LAMP ou LEMP avec accès SSH restreint.

```
#!/usr/sbin/nft -f
flush ruleset
table inet filtre {
    set ports_autorises {
        type inet_service
        elements = { 22, 80, 443 }
    }
    set ip_bannies {
        type ipv4_addr
        flags timeout
    }
    chain entree {
        type filter hook input priority 0; policy drop;
        iif lo accept
        ct state established,related accept
        ct state invalid drop
        ip saddr @ip_bannies drop
        ip protocol icmp accept
        ip6 nexthdr icmpv6 accept
        tcp dport 22 ct state new limit rate 4/minute accept
        tcp dport 22 ct state new log prefix "SSH-DROP: " drop
        tcp dport @ports_autorises accept
        log prefix "NFT-DROP: " flags all counter drop
    }
    chain sortie {
        type filter hook output priority 0; policy accept;
    }
    chain transit {
        type filter hook forward priority 0; policy drop;
    }
}
```
Appliquez-le avec `sudo nft -c -f /etc/nftables.conf` pour valider la syntaxe, puis `sudo systemctl restart nftables`. Ce socle couvre déjà l’essentiel : politique restrictive par défaut, protection SSH contre le brute-force, gestion d’une liste noire dynamique, et journalisation des paquets rejetés. Adaptez les ports, la plage d’IP SSH et les seuils de limitation à votre contexte réel avant tout déploiement en production.

## Erreurs fréquentes à éviter avec nftables

Certaines erreurs reviennent systématiquement chez les administrateurs qui découvrent nftables, souvent parce qu’ils transposent des réflexes iptables qui ne s’appliquent plus de la même façon.

- **Appliquer une politique drop sans autoriser lo et les connexions établies au préalable** , ce qui coupe immédiatement l’accès SSH en cours et exige une intervention physique ou via console pour récupérer l’accès.
- **Laisser coexister iptables legacy et nftables sur les mêmes chaînes** , provoquant des comportements incohérents où une règle semble ignorée alors qu’elle est simplement écrasée par l’autre backend.
- **Oublier d’activer ip_forward avant de configurer du NAT** , ce qui fait échouer silencieusement toute règle de masquerade sans message d’erreur explicite.
- **Ne jamais sauvegarder la configuration dans /etc/nftables.conf** après avoir testé des règles en ligne de commande, perdant tout le travail au prochain redémarrage.
- **Placer la règle de journalisation avant les règles d’acceptation** plutôt qu’à la fin de la chaîne, ce qui génère des volumes de logs énormes pour du trafic pourtant légitime.
- **Ouvrir SSH au monde entier par réflexe** plutôt que de restreindre l’accès à une plage d’IP connue ou un VPN, alors que la limitation de débit seule ne suffit pas contre des botnets distribués sur des milliers d’IP différentes.

## Dépannage : problèmes courants et leurs solutions

Voici les situations les plus fréquemment rencontrées lors de la configuration de nftables, avec la cause probable et la solution associée.

- **Le service nftables refuse de démarrer après modification du fichier de configuration.** Lancez`nft -c -f /etc/nftables.conf` pour identifier la ligne fautive avant de relancer le service, la plupart du temps une virgule ou une accolade mal fermée.
- **La connexion SSH se coupe immédiatement après application des règles.** Gardez toujours une session console active en parallèle, ou programmez un rollback automatique avec`at now + 5 minutes` qui restaure l’ancienne configuration si vous ne confirmez pas manuellement.
- **Les règles semblent ignorées malgré une configuration correcte.** Vérifiez qu’aucun ancien service iptables, ufw ou firewalld ne tourne encore en parallèle avec`systemctl status ufw firewalld` .
- **Le NAT ne fonctionne pas malgré une règle masquerade correcte.** Confirmez que`net.ipv4.ip_forward` vaut bien 1 avec`sysctl net.ipv4.ip_forward` , sinon aucun paquet ne transitera entre interfaces.
- **Les logs n’apparaissent jamais dans journalctl.** Vérifiez que le service systemd-journald tourne correctement et que la règle log est bien positionnée avant le verdict final drop, pas après.
- **Le compteur d’une règle reste à zéro alors que le trafic correspondant existe.** Une règle placée trop bas dans la chaîne peut être court-circuitée par une règle antérieure qui matche déjà le paquet, notamment la règle established,related.
- **Erreur “Could not process rule: No such file or directory” lors d’un ajout de règle.** La table ou la chaîne référencée n’existe pas encore, vérifiez l’orthographe exacte avec`nft list tables` .
- **Les performances se dégradent avec plusieurs milliers de règles individuelles.** Remplacez les règles répétitives par des sets nommés, qui utilisent des structures de données optimisées côté noyau au lieu d’une évaluation séquentielle.
- **Après une mise à jour de la distribution, les anciennes règles iptables ne sont plus prises en compte.** Certaines migrations de version basculent automatiquement le backend vers nft, exécutez une conversion avec iptables-restore-translate pour récupérer vos anciennes règles.

## Astuces avancées pour les administrateurs expérimentés

Une fois les bases maîtrisées, plusieurs fonctionnalités permettent d’aller plus loin sur des serveurs à fort trafic ou des infrastructures complexes.

Les flowtables accélèrent le transit de paquets déjà établis en les faisant passer par un chemin de traitement raccourci dans le noyau, particulièrement utile sur un routeur qui encaisse un débit important entre plusieurs interfaces. Elles évitent de repasser par l’intégralité de la chaîne de règles pour chaque paquet d’une connexion déjà validée.

```
sudo nft add flowtable inet filtre acceleration { hook ingress priority 0 \; devices = { eth0, eth1 } \; }
sudo nft add rule inet filtre transit ct state established flow add @acceleration
```
L’intégration avec CrowdSec ou Fail2ban permet d’automatiser complètement le bannissement dynamique : ces outils analysent les logs en continu et injectent directement des adresses IP malveillantes dans un set nftables avec timeout, sans intervention manuelle. C’est la combinaison recommandée pour un serveur exposé qui reçoit un volume élevé de tentatives d’intrusion automatisées.

Pour les infrastructures multi-serveurs, centraliser les logs nftables vers un outil comme Wazuh ou Graylog permet de corréler des tentatives d’attaque réparties sur plusieurs machines et de détecter des campagnes de scan coordonnées qui passeraient inaperçues serveur par serveur.

## nftables et Docker : éviter les conflits de règles

Docker manipule directement les règles de filtrage réseau pour exposer les ports de ses conteneurs, et cette manipulation entre régulièrement en collision avec une configuration nftables écrite à la main. Le démon Docker crée ses propres chaînes et injecte des règles d’acceptation qui peuvent contourner silencieusement une politique drop pourtant censée tout bloquer par défaut. Un administrateur qui publie un conteneur avec `-p 5432:5432` peut découvrir, parfois des semaines plus tard, que ce port PostgreSQL est accessible depuis Internet malgré une chaîne d’entrée nftables strictement restrictive.

Ce comportement s’explique par la façon dont Docker gère le NAT et le transit de paquets pour ses réseaux de conteneurs : les paquets destinés à un port publié passent par la chaîne forward et par des règles NAT propres à Docker, souvent insérées avec une priorité qui les place avant les règles de filtrage écrites manuellement. La documentation Docker recommande d’ailleurs explicitement de ne pas gérer ces chaînes générées automatiquement.

```
# Inspecter les chaînes injectées par Docker
sudo nft list chain ip docker docker
sudo nft list chain inet filtre transit
# Restreindre l'exposition d'un conteneur à une IP précise plutôt qu'à toutes les interfaces
docker run -p 203.0.113.10:5432:5432 postgres
```
La solution la plus fiable reste de limiter les ports publiés à une adresse IP spécifique plutôt qu’à `0.0.0.0`, et de vérifier systématiquement avec `nmap` depuis l’extérieur après chaque déploiement de conteneur, exactement comme à l’étape 12 de ce tutoriel. Sur un serveur qui héberge à la fois des services système et des conteneurs Docker, considérez ces deux mondes comme deux couches de filtrage distinctes qu’il faut auditer séparément.

## nftables sur les hébergeurs cloud européens : OVHcloud, Scaleway, Hetzner

La majorité des serveurs Linux déployés en France et en Europe tournent aujourd’hui chez des hébergeurs comme OVHcloud, Scaleway ou Hetzner, qui proposent tous un pare-feu réseau en amont, au niveau de l’infrastructure plutôt que du système d’exploitation. Ce filtrage périphérique ne dispense pas d’une configuration nftables locale : les deux couches se complètent et répondent à des besoins différents.

Le pare-feu réseau de l’hébergeur bloque le trafic avant même qu’il n’atteigne la carte réseau virtuelle du serveur, ce qui protège contre des volumes d’attaque qu’un pare-feu local ne pourrait pas encaisser seul, comme certaines tentatives de saturation réseau. nftables, de son côté, offre une granularité que les consoles d’hébergeur ne permettent généralement pas : limitation de débit par connexion, journalisation détaillée, sets dynamiques alimentés par des outils comme CrowdSec, ou logique conditionnelle complexe basée sur l’état des connexions.

Une pratique de défense en profondeur consiste à dupliquer les règles essentielles aux deux niveaux : bloquer un port sensible à la fois dans la console de l’hébergeur et dans `/etc/nftables.conf`. Si une des deux couches est mal configurée ou temporairement désactivée pendant une opération de maintenance, l’autre continue de protéger le serveur. Cette redondance coûte quelques minutes de configuration supplémentaires mais évite qu’une simple erreur de manipulation sur une console web n’expose brutalement un service critique.

## nftables vs ufw vs firewalld : quel outil choisir en 2026

nftables reste le moteur sous-jacent dans les trois cas, mais le choix de l’interface change radicalement l’expérience d’administration selon le contexte d’usage.

| Critère | nftables (natif) | ufw | firewalld | 
|---|---|---|---|
| Distribution typique | Toutes, usage direct | Ubuntu, Debian | Fedora, RHEL, Rocky Linux | 
| Courbe d’apprentissage | Élevée | Faible | Moyenne | 
| Granularité des règles | Totale | Limitée | Bonne, via zones | 
| Gestion dynamique (runtime) | Oui, sans coupure | Rechargement complet | Oui, via zones actives | 
| Cas d’usage recommandé | Serveurs de production, routeurs, règles complexes | Postes de travail, petits serveurs perso | Environnements d’entreprise avec zones réseau multiples | 

Pour un serveur de production avec des besoins de filtrage précis, le passage direct par nftables offre le contrôle le plus fin. Pour un poste de travail personnel ou un petit VPS où la simplicité prime, ufw reste un choix pertinent puisqu’il génère lui-même des règles nftables optimisées en coulisses. firewalld convient particulièrement aux environnements d’entreprise qui séparent le trafic par zones de confiance (interne, DMZ, externe) sans vouloir écrire de règles nftables brutes.

La documentation officielle du wiki nftables et le site du projet Netfilter restent les références techniques les plus fiables pour approfondir la syntaxe et suivre les évolutions du projet. Pour les administrateurs sous Ubuntu qui préfèrent une approche progressive, la documentation officielle Ubuntu sur les pare-feu détaille aussi bien ufw que l’usage direct de nftables.

Sur le plan réglementaire, le site de l’ANSSI publie des recommandations de durcissement système régulièrement mises à jour, et le CERT-FR diffuse des bulletins de vulnérabilités qui aident à prioriser quels services filtrer en urgence sur un serveur exposé.

## Questions fréquentes

**nftables remplace-t-il complètement iptables en 2026 ?**

Sur la majorité des distributions modernes, oui : iptables reste disponible en façade pour des raisons de compatibilité, mais les commandes passent par la couche iptables-nft qui traduit tout vers le moteur nftables en interne. Écrire directement en syntaxe nft reste toutefois recommandé pour profiter des sets, verdict maps et flowtables.

**Peut-on utiliser ufw et nftables en même temps sur le même serveur ?**

Non, il faut choisir l’un ou l’autre. ufw génère lui-même des règles nftables en coulisses, donc faire tourner les deux en parallèle avec des règles manuelles crée des conflits et des comportements imprévisibles.

**Comment revenir en arrière si une règle nftables bloque tout l’accès réseau ?**

Depuis une console physique ou un accès KVM, exécutez `nft flush ruleset` pour vider toutes les règles actives, puis redémarrez avec une configuration corrigée. C’est pourquoi garder un accès console de secours est indispensable avant tout test en production.

**nftables suffit-il seul pour sécuriser un serveur, ou faut-il d’autres outils ?**

Un pare-feu filtre le trafic réseau mais ne remplace pas les autres couches de sécurité : mises à jour régulières, authentification par clé SSH plutôt que mot de passe, supervision des logs, et éventuellement un outil de détection d’intrusion comme Suricata pour analyser le contenu du trafic autorisé.

**Quelle est la différence entre reject et drop dans une règle nftables ?**

Drop abandonne silencieusement le paquet sans réponse, ce qui rend le port indistinguable d’un port inexistant pour un attaquant. Reject renvoie un message d’erreur explicite (par exemple ICMP port unreachable), plus poli pour du trafic légitime mais qui révèle davantage d’informations à un scanner.

**Comment tester une configuration nftables sans risquer de couper l’accès SSH ?**

Utilisez toujours `nft -c -f` pour valider la syntaxe avant application, testez depuis une VM locale non critique en premier, et programmez un job de rollback automatique via `at` ou `cron` qui restaure l’ancienne configuration après quelques minutes si vous ne confirmez pas manuellement que tout fonctionne.

**Les sets nftables ralentissent-ils les performances par rapport à des règles individuelles ?**

C’est l’inverse : les sets utilisent des structures de données optimisées (tables de hachage ou arbres) évaluées en temps quasi constant, alors qu’une longue liste de règles individuelles s’évalue séquentiellement. Sur un serveur avec des centaines d’IP à filtrer, les sets sont nettement plus rapides.

**Faut-il désactiver le pare-feu de l’hébergeur si nftables est déjà configuré sur le serveur ?**

Non, les deux couches sont complémentaires plutôt que redondantes. Le pare-feu réseau de l’hébergeur filtre le trafic avant qu’il n’atteigne le serveur, ce qui protège contre des volumes de trafic qu’une configuration locale ne pourrait pas absorber seule, tandis que nftables offre une granularité plus fine, propre à chaque service et à chaque conteneur hébergé sur la machine.

**Pourquoi un port publié par Docker reste-t-il accessible malgré une politique nftables restrictive ?**

Docker insère ses propres chaînes et règles NAT pour exposer les ports de ses conteneurs, souvent avec une priorité qui les place avant les règles écrites manuellement dans la chaîne forward. Publier un conteneur en le liant à une adresse IP précise plutôt qu’à toutes les interfaces, puis vérifier avec nmap depuis l’extérieur, reste la méthode la plus fiable pour éviter les mauvaises surprises.
