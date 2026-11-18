Suricata vient de passer un cap important : depuis le 9 juillet 2026, la branche 8.0.x est la seule activement maintenue par l’Open Information Security Foundation (OISF), la branche 7.x étant officiellement en fin de vie (EOL) depuis le 7 juillet 2026 avec l’ultime correctif 7.0.17. Le rythme de publication est resté soutenu depuis : après la 8.0.6 de juillet, l’OISF a livré la 8.0.7 en septembre 2026, confirmant un calendrier de correctifs régulier sur la nouvelle branche. Pour les administrateurs réseau, RSSI et développeurs qui gèrent l’infrastructure d’une PME ou d’un service public en France, ce basculement n’est pas un détail technique : sans mise à jour, les serveurs restent exposés sans les correctifs de sécurité les plus récents. Ce tutoriel vous montre, étape par étape, comment déployer Suricata 8.0.7, la dernière version stable disponible en septembre 2026, comme système de détection et de prévention d’intrusion (IDS/IPS) sur un serveur Debian ou Ubuntu, configurer des règles de détection pertinentes, et l’intégrer dans une chaîne de supervision réseau complète.

Suricata est un moteur open source d’inspection profonde de paquets (DPI), capable d’analyser le trafic HTTP, TLS, DNS et bien d’autres protocoles en temps réel pour repérer des signatures d’attaque, des comportements suspects ou des flux non conformes à une politique de sécurité. Contrairement à un pare-feu classique qui filtre sur des règles statiques de ports et d’adresses, Suricata inspecte le contenu des paquets et peut donc détecter une exploitation de vulnérabilité, un malware en communication avec son serveur de commande, ou une exfiltration de données. Il tourne aussi bien en mode IDS (détection passive, alertes) qu’en mode IPS (blocage actif en ligne), et sert de moteur de détection dans des distributions de sécurité populaires comme Security Onion, dont la version 3.2.0 est sortie fin juillet 2026.

## Pourquoi déployer Suricata 8.0.6 en 2026

La fin de vie de Suricata 7 change la donne pour toute organisation qui exploite encore cette branche. Selon la politique EOL publiée par l’OISF, les versions 7.0.x sont en fin de vie depuis le 7 juillet 2026 avec la publication de l’ultime correctif 7.0.17, après une série de mises à jour régulières qui a vu se succéder la 7.0.13 en novembre 2025, la 7.0.14 en janvier 2026, la 7.0.15 en mars puis la 7.0.16 en mai 2026 ; les versions 6.0.x sont quant à elles en fin de vie depuis août 2024. Concrètement, cela signifie qu’aucun correctif de sécurité ne sera plus publié pour ces branches, ce qui expose les moteurs de détection eux-mêmes à des vulnérabilités non patchées, un comble pour un outil censé protéger le réseau.

Suricata 8.0.6, publié le 9 juillet 2026 en même temps que le correctif 7.0.17 de la branche sortante, a rapidement été suivi par la 8.0.7 en septembre 2026, confirmant le rythme de publication engagé par l’OISF depuis le lancement de la branche 8.0.x avec la 8.0.3 en janvier 2026, puis la 8.0.4 en mars et la 8.0.5 en mai. La 8.0.7 est aujourd’hui le choix recommandé pour un déploiement neuf. L’écosystème a suivi rapidement : le paquet Debian `suricata 1:8.0.6-1` a été accepté dans *unstable* et *stable-backports* le 10 juillet 2026, ce qui permet une installation propre sur Debian 12 et Debian 13 sans avoir besoin de compiler depuis les sources, en attendant la mise à jour du paquet vers la 8.0.7. Security Onion, la distribution de supervision réseau la plus utilisée dans les SOC (Security Operations Center) en Europe, a également suivi le mouvement : la version 3.1.0 sortie le 28 mai 2026 embarque Suricata 8.0.5, et la 3.2.0 de fin juillet 2026 introduit en plus des fonctionnalités d’IA agentique et un support étendu des règles Sigma pour la corrélation d’événements.

Pour une entreprise soumise à la directive NIS2 ou au Cyber Resilience Act, disposer d’un IDS/IPS à jour et documenté fait partie des exigences de base en matière de détection d’incident. Un moteur de détection open source, gratuit et audité par une fondation indépendante, reste l’option la plus accessible pour les PME qui n’ont pas le budget d’une solution commerciale de type NDR (Network Detection and Response).

Security Onion 3.2.0 illustre également une tendance de fond de l’écosystème en 2026 : l’ajout de capacités d’IA agentique en complément de la détection par signatures, avec un modèle Gemma proposé comme option hébergée pour assister l’analyse d’alertes. Cela ne remplace pas Suricata, mais montre que le moteur de détection par signatures reste la brique de base sur laquelle viennent désormais se greffer des couches de corrélation plus intelligentes. Pour une PME ou une collectivité qui déploie son premier IDS/IPS, l’essentiel reste néanmoins de maîtriser Suricata lui-même avant d’envisager ces couches additionnelles.

## Prérequis : matériel, logiciels et versions

Avant de commencer l’installation, vérifiez que votre environnement correspond à ces prérequis. Ce tutoriel a été rédigé et testé sur Debian 13 (Trixie), mais fonctionne également sur Debian 12 (Bookworm) avec les backports, ainsi que sur Ubuntu 24.04 LTS et 25.10.

| Composant | Version minimale recommandée | Remarque | 
|---|---|---|
| Système d’exploitation | Debian 12/13 ou Ubuntu 24.04+ | Autres distributions compatibles via compilation source | 
| Suricata | 8.0.6 (juillet 2026) | Suricata 7.x est EOL depuis le 7 juillet 2026 | 
| RAM | 4 Go minimum, 8 Go recommandé | Dépend du débit réseau à inspecter | 
| CPU | 4 cœurs minimum | Suricata exploite le multi-threading natif | 
| Espace disque | 50 Go minimum | Pour les logs, PCAP et règles de détection | 
| Accès réseau | Interface en mode promiscuous ou port mirroring (SPAN) | Nécessaire pour capturer tout le trafic à inspecter | 
| Droits | Accès root ou sudo | Requis pour l’installation de paquets et la configuration réseau | 

Vous aurez également besoin d’un accès à Internet pour télécharger les règles de détection (les “rulesets”) depuis des sources comme Emerging Threats Open ou Abuse.ch, et d’une machine de test isolée si vous voulez d’abord valider votre configuration avant un déploiement en production. Si vous n’avez pas encore de labo de test, notre tutoriel pour installer Kali Linux 2026.2 dans VirtualBox peut servir de base pour générer du trafic d’attaque contrôlé et valider vos règles Suricata.

## Étape 1 : Préparer le système et identifier l’interface réseau

Commencez par mettre à jour votre système et identifier l’interface réseau que Suricata va surveiller. Cette interface doit voir passer tout le trafic à inspecter, ce qui implique généralement un port mirroring (SPAN) sur votre switch si vous inspectez du trafic autre que celui de la machine elle-même.

```
sudo apt update && sudo apt upgrade -y
ip a
# repérez le nom de votre interface, par exemple eth0 ou ens18
sudo ethtool -k eth0 | grep -i offload
```
Désactivez les fonctions d’offload matériel (segmentation offload, generic receive offload) sur l’interface de capture, car elles peuvent fragmenter la vue que Suricata a du trafic réel et provoquer des faux négatifs.

`sudo ethtool -K eth0 tso off gso off gro off lro off`
## Étape 2 : Installer Suricata 8.0.6 depuis le dépôt officiel

Sur Debian, le paquet `suricata 1:8.0.6-1` est disponible dans *unstable* et via *stable-backports* depuis le 10 juillet 2026. Sur Debian 12 stable, ajoutez le dépôt backports pour obtenir la version à jour plutôt que l’ancienne version packagée dans la branche stable.

```
echo "deb http://deb.debian.org/debian bookworm-backports main" | sudo tee /etc/apt/sources.list.d/backports.list
sudo apt update
sudo apt install -y suricata -t bookworm-backports
suricata --build-info | grep "This is Suricata"
```
Sur Ubuntu, un PPA maintenu par l’OISF distribue également les paquets 8.0.x à jour :

```
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt update
sudo apt install -y suricata
suricata -V
```
La commande `suricata -V` doit retourner au minimum `8.0.6` ; la dernière version stable publiée par l’OISF est la 8.0.7, sortie en septembre 2026, et il est recommandé d’y migrer dès qu’elle est disponible dans les dépôts backports ou le PPA. Si une version antérieure s’affiche, vérifiez que le dépôt backports ou le PPA est bien prioritaire dans votre configuration APT avant de continuer.

## Étape 3 : Comprendre l’arborescence de configuration

L’installation crée plusieurs répertoires clés qu’il faut connaître avant de toucher à quoi que ce soit :

- `/etc/suricata/suricata.yaml` : fichier de configuration principal
- `/etc/suricata/rules/` : dossier des règles de détection
- `/var/log/suricata/` : logs, alertes et fichiers`eve.json`
- `/var/lib/suricata/` : données runtime, dont la base de données GeoIP si activée

Le fichier `eve.json` est le cœur du système : c’est un flux structuré au format JSON qui contient toutes les alertes, les métadonnées de flux, les logs DNS, TLS et HTTP. C’est ce fichier que vous brancherez plus tard sur un SIEM comme Wazuh ou Graylog pour la corrélation et la visualisation des événements.

## Étape 4 : Configurer l’interface de capture dans suricata.yaml

Ouvrez le fichier de configuration principal et modifiez la section `af-packet` pour pointer vers votre interface réseau réelle.

`sudo nano /etc/suricata/suricata.yaml`
Repérez la section `af-packet` et adaptez-la :

```
af-packet:
  - interface: eth0
    threads: auto
    cluster-id: 99
    cluster-type: cluster_flow
    defrag: yes
    use-mmap: yes
    tpacket-v3: yes
```
L’option `cluster-type: cluster_flow` permet à Suricata de répartir la charge entre plusieurs threads tout en gardant les paquets d’un même flux TCP sur le même thread, ce qui est essentiel pour une inspection cohérente. Modifiez également la variable `HOME_NET` en haut du fichier pour qu’elle corresponde à votre plage IP interne, sans quoi de nombreuses règles ne se déclencheront pas correctement.

```
vars:
  address-groups:
    HOME_NET: "[192.168.1.0/24,10.0.0.0/8]"
    EXTERNAL_NET: "!$HOME_NET"
```
## Étape 5 : Choisir entre mode IDS et mode IPS

Suricata peut fonctionner en mode passif (IDS, détection uniquement avec alertes) ou en mode actif en ligne (IPS, capable de bloquer le trafic malveillant). Pour un premier déploiement, commencez toujours en mode IDS afin d’observer le comportement des règles sans risquer de bloquer du trafic légitime.

| Mode | Fonctionnement | Cas d’usage recommandé | 
|---|---|---|
| IDS (af-packet) | Capture passive via mirroring, alertes uniquement | Phase de test, tuning des règles, environnements sensibles | 
| IPS (NFQUEUE) | Inline avec iptables/nftables, blocage actif | Production après validation, passerelle réseau | 
| IPS (af-packet en ligne) | Bridge transparent entre deux interfaces | Déploiement en coupure sur un lien dédié | 

Pour activer le mode IPS via NFQUEUE sur Linux, redirigez le trafic vers Suricata avec nftables avant de lancer le moteur en mode `-q` :

```
sudo nft add rule inet filter forward queue num 0
sudo suricata -c /etc/suricata/suricata.yaml -q 0
```
## Étape 6 : Mettre à jour et gérer les règles de détection avec suricata-update

Suricata ne détecte rien sans règles. L’outil officiel `suricata-update`, installé automatiquement avec le paquet 8.0.6, gère le téléchargement et la mise à jour des rulesets depuis plusieurs sources communautaires.

```
sudo suricata-update
sudo suricata-update list-sources
```
Par défaut, la source Emerging Threats Open est activée. Vous pouvez en ajouter d’autres, comme les règles ET Pro (payantes) ou les flux communautaires spécialisés dans les malwares connus :

```
sudo suricata-update enable-source et/open
sudo suricata-update enable-source oisf/trafficid
sudo suricata-update
```
Planifiez une tâche cron pour que ces règles se mettent à jour automatiquement chaque jour, ce qui est indispensable pour rester protégé contre les menaces les plus récentes :

`echo "0 4 * * * root suricata-update && systemctl restart suricata" | sudo tee /etc/cron.d/suricata-update`
## Comprendre la syntaxe d’une règle Suricata

Avant d’aller plus loin, il est utile de comprendre comment une règle Suricata est structurée, ne serait-ce que pour interpréter correctement les alertes que vous allez recevoir. Une règle se compose de trois blocs : une action, un en-tête réseau et des options entre parenthèses.

`alert tcp $EXTERNAL_NET any -> $HOME_NET 22 (msg:"ET SCAN Potential SSH Scan"; flags:S; threshold:type both, track by_src, count 5, seconds 60; classtype:attempted-recon; sid:2001219; rev:20;)`
L’action `alert` indique que Suricata doit générer une alerte sans bloquer le paquet (contrairement à `drop`, réservé au mode IPS). Vient ensuite le protocole (`tcp`), puis l’adresse et le port source, une direction (`->` pour un sens unique, `<>` pour bidirectionnel), et enfin l’adresse et le port de destination. Le bloc d’options contient le message affiché dans les logs (`msg`), les conditions de correspondance, la classification de la menace, et surtout le `sid`, un identifiant unique qui permet de retrouver, désactiver ou surveiller cette règle précise dans vos outils de gestion.

Le mot-clé `threshold` mérite une attention particulière : il évite de recevoir une alerte à chaque paquet suspect individuel et regroupe les événements similaires sur une fenêtre de temps donnée, ici cinq tentatives en 60 secondes avant de déclencher l’alerte. C’est ce type de réglage qui distingue une configuration Suricata bien réglée d’une installation par défaut qui noie l’équipe sécurité sous des milliers d’alertes redondantes.

## Écrire une règle de détection personnalisée

Les rulesets communautaires comme Emerging Threats Open couvrent l’essentiel, mais chaque environnement a ses propres besoins. Créez un fichier dédié à vos règles maison plutôt que de modifier les fichiers téléchargés automatiquement, qui seraient écrasés à la prochaine mise à jour.

`sudo nano /etc/suricata/rules/local.rules`
Voici un exemple de règle personnalisée qui détecte une tentative de connexion à un panneau d’administration sensible depuis l’extérieur du réseau, un cas fréquent pour protéger un service interne exposé par erreur :

`alert http $EXTERNAL_NET any -> $HOME_NET any (msg:"LOCAL Acces suspect au panneau admin depuis Internet"; flow:established,to_server; http.uri; content:"/wp-admin"; classtype:web-application-attack; sid:9000001; rev:1;)`
Ajoutez ensuite ce fichier à la liste des règles chargées dans `suricata.yaml`, sous la section `rule-files`, puis rechargez la configuration :

```
rule-files:
  - suricata.rules
  - local.rules
```
```
sudo suricata -T -c /etc/suricata/suricata.yaml -v
sudo systemctl reload suricata
```
Numérotez toujours vos SID personnalisés au-delà de 9 000 000, une plage réservée par convention aux règles locales, afin d’éviter tout conflit avec les identifiants utilisés par les rulesets publics.

## Étape 7 : Valider la configuration avant le premier lancement

Ne lancez jamais Suricata en production sans avoir vérifié que la configuration et les règles sont syntaxiquement valides. Le mode `-T` (test) permet de le faire sans démarrer réellement le service.

`sudo suricata -T -c /etc/suricata/suricata.yaml -v`
Un message `Configuration provided was successfully loaded` confirme que tout est prêt. Si une erreur apparaît, elle indique généralement le numéro de ligne du fichier YAML ou de la règle fautive, ce qui facilite grandement le débogage.

## Étape 8 : Démarrer le service et vérifier son fonctionnement

```
sudo systemctl enable suricata
sudo systemctl start suricata
sudo systemctl status suricata
```
Vérifiez que le processus capture bien du trafic en consultant les statistiques internes, mises à jour toutes les 8 secondes par défaut dans le fichier `stats.log` :

`sudo tail -f /var/log/suricata/stats.log | grep -E "capture.kernel_packets|decoder.pkts"`
Exemple de sortie attendue si la capture fonctionne correctement :

```
capture.kernel_packets       | Total                     | 128453
decoder.pkts                 | Total                     | 128453
decoder.bytes                | Total                     | 98234112
flow.total                   | Total                     | 3021
```
Un compteur `capture.kernel_drops` élevé signale que Suricata ne parvient pas à traiter tout le trafic en temps réel, généralement parce que le nombre de threads est insuffisant ou que le CPU sature. C’est le signal qu’il faut revoir le dimensionnement matériel ou ajuster les paramètres de capture.

## Étape 9 : Déclencher et lire une alerte de test

Pour vérifier que la chaîne de détection fonctionne de bout en bout, utilisez la règle de test officielle intégrée à Emerging Threats, qui se déclenche sur une requête HTTP contenant une chaîne spécifique inoffensive.

`curl http://testmynids.org/uid/index.html`
Consultez ensuite le fichier d’alertes au format lisible :

`sudo tail -20 /var/log/suricata/fast.log`
Vous devriez voir une ligne du type :

`08/30/2026-11:42:03.128341 [**] [1:2100498:7] GPL ATTACK_RESPONSE id check returned root [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 192.168.1.50:51422 -> 217.160.0.1:80`
Si cette ligne apparaît, votre pipeline de détection est opérationnel de bout en bout : capture réseau, moteur de règles et journalisation fonctionnent ensemble.

## Optimiser les performances : threads, mémoire et capture

Sur un lien réseau chargé, la configuration par défaut de Suricata montre vite ses limites. Le paramètre `threads: auto` dans la section af-packet laisse Suricata déterminer lui-même le nombre de threads de capture en fonction des cœurs disponibles, mais sur un serveur partagé avec d’autres services, il est souvent préférable de fixer une valeur explicite pour ne pas monopoliser tout le CPU.

```
af-packet:
  - interface: eth0
    threads: 6
    ring-size: 200000
    block-size: 1048576
```
Le paramètre `ring-size` définit le nombre de paquets que le noyau peut mettre en mémoire tampon avant que Suricata ne les traite : une valeur trop basse provoque des pertes de paquets sous forte charge, une valeur trop haute consomme de la RAM inutilement. Sur un serveur avec 8 Go de RAM dédiés à Suricata, une valeur entre 100 000 et 200 000 constitue un bon point de départ, à ajuster ensuite en observant le compteur `capture.kernel_drops` dans les statistiques.

Pensez également à ajuster la limite de mémoire allouée au flow tracking, qui garde en mémoire l’état de chaque connexion réseau active. Sur un réseau avec beaucoup de connexions simultanées (proxy, passerelle NAT), cette limite peut être atteinte rapidement et provoquer l’abandon prématuré du suivi de certains flux.

```
flow:
  memcap: 512mb
  hash-size: 65536
  prealloc: 10000
```
Après chaque changement de configuration, relancez le test de validation puis surveillez les statistiques pendant au moins une heure de trafic représentatif avant de considérer le réglage comme définitif. Un dimensionnement correct dès le départ évite des heures de diagnostic plus tard, lorsque le trafic augmente en production.

## Étape 10 : Exploiter le format eve.json pour l’analyse structurée

Le fichier `fast.log` est utile pour un test rapide, mais en production, tout doit passer par `eve.json`, qui structure chaque événement en JSON exploitable par un outil d’analyse. Vérifiez que le module `eve-log` est actif dans `suricata.yaml` :

```
outputs:
  - eve-log:
      enabled: yes
      filetype: regular
      filename: eve.json
      types:
        - alert
        - dns
        - tls
        - http
        - flow
        - ssh
```
Vous pouvez interroger ce flux directement avec `jq` pour un premier niveau d’analyse sans outil externe :

`sudo tail -f /var/log/suricata/eve.json | jq 'select(.event_type=="alert") | {time:.timestamp, src:.src_ip, dst:.dest_ip, sig:.alert.signature}'`
## Étape 11 : Centraliser les alertes dans un SIEM

Un IDS/IPS isolé, dont personne ne consulte les logs, ne sert à rien en pratique. L’étape suivante consiste à envoyer le flux `eve.json` vers un outil de centralisation et de corrélation. Deux options open source dominent l’écosystème francophone : Wazuh, qui combine SIEM et EDR, et Graylog, plus orienté centralisation de logs pure.

Si vous utilisez déjà Wazuh (voir notre tutoriel Wazuh SIEM Open Source), ajoutez simplement Suricata comme source de log dans le fichier `ossec.conf` de l’agent :

```
<localfile>
  <log_format>json</log_format>
  <location>/var/log/suricata/eve.json</location>
</localfile>
```
Si vous préférez Graylog pour la centralisation, notre guide pour centraliser ses logs de sécurité avec Graylog 7.1 détaille la configuration d’un input GELF ou Syslog compatible avec la sortie JSON de Suricata.

## Étape 12 : Compléter la défense avec un pare-feu et un scanner de vulnérabilités

Suricata détecte et peut bloquer du trafic malveillant, mais il ne remplace pas un pare-feu périmétrique ni un scanner de vulnérabilités. Une architecture de sécurité réseau cohérente combine généralement les trois. Si vous n’avez pas encore de pare-feu open source en place, notre tutoriel pour installer OPNsense couvre le filtrage périmétrique, qui peut d’ailleurs intégrer nativement Suricata comme moteur IDS/IPS via son propre plugin.

Pour identifier les vulnérabilités présentes sur vos serveurs avant qu’un attaquant ne les exploite, complétez votre chaîne avec un scanner comme celui présenté dans notre guide pour configurer OpenVAS/Greenbone. Et pour cartographier votre réseau et identifier les services exposés avant même de déployer Suricata, notre tutoriel Nmap reste la première étape logique de tout audit réseau.

## Étape 13 : Renforcer contre le brute-force et automatiser le blocage

Suricata excelle dans la détection de trafic malveillant complexe, mais pour bloquer automatiquement les tentatives de brute-force sur SSH ou vos services exposés, un outil complémentaire comme CrowdSec ou Fail2ban reste plus adapté et plus léger. Notre tutoriel CrowdSec montre comment bloquer les attaques répétées grâce à une base de réputation communautaire, en complément direct des alertes générées par Suricata.

## Suricata et le RGPD : ce qu’il faut savoir avant de capturer du trafic

Déployer un IDS/IPS qui inspecte le trafic réseau, y compris les métadonnées HTTP et DNS de vos utilisateurs, soulève des questions de conformité au RGPD qu’il ne faut pas négliger en France. Une adresse IP est considérée comme une donnée à caractère personnel par la CNIL, et les journaux générés par Suricata contiennent systématiquement des adresses IP source et destination, parfois associées à des noms de domaine ou des chemins d’URL révélateurs de l’activité d’un utilisateur.

Concrètement, cela implique plusieurs précautions pour une entreprise française. Informez vos salariés, via la charte informatique ou le règlement intérieur, qu’une supervision réseau est en place à des fins de sécurité, conformément au principe de transparence du RGPD. Limitez la durée de conservation des logs à ce qui est strictement nécessaire à la détection et à l’investigation d’incident, en général quelques mois, et documentez cette durée dans votre registre de traitement. Restreignez l’accès aux journaux Suricata et à votre SIEM aux seules personnes habilitées, avec une journalisation des accès lorsque c’est possible. Enfin, si votre organisation dépend du secteur public ou traite des données de santé, vérifiez que votre architecture de supervision réseau s’inscrit bien dans le cadre plus large de vos obligations SecNumCloud ou NIS2, qui imposent déjà une capacité de détection documentée.

## Automatiser le déploiement avec Ansible

Pour une organisation qui gère plusieurs sondes Suricata sur différents sites ou segments réseau, installer et configurer chaque serveur manuellement devient vite ingérable. Un playbook Ansible simple permet de standardiser l’installation, le déploiement des règles locales et le redémarrage du service sur l’ensemble du parc en une seule commande.

```
---
- name: Déployer Suricata 8.0.6 sur les sondes réseau
  hosts: sondes_suricata
  become: true
  tasks:
    - name: Installer Suricata depuis les backports
      apt:
        name: suricata
        state: present
        default_release: bookworm-backports
        update_cache: true
    - name: Copier la configuration HOME_NET personnalisée
      template:
        src: templates/suricata.yaml.j2
        dest: /etc/suricata/suricata.yaml
      notify: redémarrer suricata
    - name: Copier les règles locales
      copy:
        src: files/local.rules
        dest: /etc/suricata/rules/local.rules
      notify: redémarrer suricata
    - name: Mettre à jour les règles publiques
      command: suricata-update
  handlers:
    - name: redémarrer suricata
      systemd:
        name: suricata
        state: restarted
```
Ce playbook peut être exécuté via une intégration continue (GitLab CI, GitHub Actions) à chaque modification du dépôt Git contenant vos règles locales, ce qui garantit que toutes vos sondes appliquent la même politique de détection en quelques minutes, sans intervention manuelle serveur par serveur.

## Erreurs fréquentes à éviter lors du déploiement

Plusieurs pièges reviennent systématiquement lors des premiers déploiements de Suricata, y compris chez des équipes techniques expérimentées.

- **Oublier de définir HOME_NET correctement.** Une valeur par défaut ou trop large fait que de nombreuses règles orientées “trafic sortant” ou “trafic interne” ne se déclenchent jamais, donnant une fausse impression de calme.
- **Laisser les fonctions d’offload réseau activées.** TSO, GSO et GRO peuvent réassembler ou fragmenter les paquets avant que Suricata ne les voie, provoquant des détections manquées difficiles à diagnostiquer.
- **Activer trop de règles d’un coup sans phase de tuning.** Un ruleset complet en mode IPS sans période d’observation en IDS génère un volume de faux positifs qui décourage rapidement les équipes et peut bloquer du trafic métier légitime.
- **Ne jamais mettre à jour les règles.** Un cron mal configuré ou oublié laisse le moteur fonctionner avec des signatures vieilles de plusieurs mois, inefficaces contre les menaces récentes.
- **Sous-dimensionner le CPU par rapport au débit réel.** Un lien à 1 Gbit/s saturé génère des kernel_drops massifs sur un serveur à 2 cœurs, rendant la détection partielle et peu fiable.
- **Confondre mode IDS et mode IPS dès le départ.** Démarrer directement en blocage actif sans phase de validation peut interrompre des flux métier critiques, notamment lors de faux positifs sur du trafic chiffré ou des API internes.
- **Ignorer la rotation des logs.** Le fichier eve.json peut grossir de plusieurs gigaoctets par jour sur un réseau chargé ; sans logrotate configuré, le disque se remplit et le service peut planter.

## Dépannage : 8 problèmes courants et leurs solutions

| Symptôme | Cause probable | Solution | 
|---|---|---|
| Le service ne démarre pas | Erreur de syntaxe dans suricata.yaml | Lancer `suricata -T -c /etc/suricata/suricata.yaml -v` pour localiser l’erreur | 
| Aucune alerte générée | HOME_NET mal configuré ou interface incorrecte | Vérifier la variable HOME_NET et confirmer l’interface avec `ip a` | 
| capture.kernel_drops élevé | CPU insuffisant ou offload réseau actif | Augmenter les threads af-packet et désactiver TSO/GSO/GRO | 
| eve.json ne grossit pas | Module eve-log désactivé dans la config | Vérifier `enabled: yes` sous la section eve-log | 
| suricata-update échoue | Pas d’accès Internet sortant ou proxy non configuré | Tester la connectivité et définir la variable HTTP_PROXY si nécessaire | 
| Trop de faux positifs | Règles génériques activées sans contexte réseau | Désactiver les signatures non pertinentes via un fichier disable.conf | 
| IPS bloque du trafic légitime | Règle en mode “drop” mal calibrée pour l’environnement | Repasser la règle en mode “alert” le temps de l’analyser | 
| Service qui plante après quelques heures | Disque saturé par les logs non tournés | Configurer logrotate sur /var/log/suricata/ et limiter la rétention | 

## Astuces avancées pour aller plus loin

Une fois le déploiement de base stabilisé, plusieurs réglages permettent d’affiner la détection et de réduire la charge opérationnelle. Activez d’abord l’inspection TLS avec extraction de certificats pour repérer les certificats auto-signés suspects ou les domaines nouvellement enregistrés utilisés dans des campagnes de phishing, une technique de plus en plus courante en 2026 face au chiffrement quasi systématique du trafic web.

Exploitez également le module `file-store` de Suricata pour extraire automatiquement les fichiers transitant sur le réseau (exécutables, documents Office, archives) et les soumettre à un antivirus ou à un bac à sable d’analyse. Cette fonctionnalité, combinée à des règles de détection de type YARA importées via `suricata-update`, transforme Suricata en un outil de détection de malware réseau à part entière, et pas seulement en signature matching classique.

Pour les environnements à fort débit, envisagez PF_RING ou DPDK comme méthode de capture alternative à af-packet standard : ces bibliothèques contournent une partie de la pile réseau du noyau Linux et réduisent significativement les pertes de paquets sur des liens à plusieurs Gbit/s. Enfin, si votre organisation dispose de plusieurs sites, pensez à uniformiser vos règles de détection via un dépôt Git central, ce qui facilite l’audit de conformité NIS2 et garantit une politique de détection cohérente sur l’ensemble du parc.

## Projet complet : IDS/IPS Suricata avec supervision Wazuh

Voici l’architecture complète d’un déploiement fonctionnel, du capteur réseau à l’alerte visible dans un tableau de bord, telle qu’elle peut être mise en place sur un serveur unique pour une PME.

1. Un serveur Debian 13 avec une interface dédiée en mode SPAN/mirroring recevant une copie du trafic du switch principal
2. Suricata 8.0.6 installé et configuré en mode IDS, avec HOME_NET défini sur la plage interne réelle
3. Le ruleset Emerging Threats Open, mis à jour quotidiennement via cron et suricata-update
4. La sortie eve.json envoyée en local vers un agent Wazuh qui transmet les événements au serveur central
5. Un tableau de bord Wazuh affichant les alertes Suricata classées par sévérité, IP source et signature
6. Une règle d’alerte critique déclenchant une notification par e-mail ou webhook vers un canal de messagerie interne dès qu’une signature de sévérité 1 (haute priorité) est détectée

Ce montage, réalisable en une demi-journée pour une équipe technique déjà familière avec Linux, constitue une base de détection réseau crédible pour toute structure qui doit démontrer une capacité de détection d’incident dans le cadre de ses obligations réglementaires.

## Cas d’usage concret : repérer une exfiltration de données

Pour illustrer l’intérêt pratique de tout ce déploiement, prenons un scénario courant en entreprise : un poste de travail compromis par un malware tente d’envoyer des documents internes vers un serveur externe via une requête HTTPS chiffrée, un comportement qu’un pare-feu classique laisserait passer sans broncher puisque le port 443 est légitimement ouvert vers l’extérieur.

Suricata, grâce à l’inspection TLS activée sur le module `tls-log`, peut extraire le nom de domaine du certificat présenté lors de la négociation TLS (SNI), même sans déchiffrer le contenu du trafic. Si ce domaine ne correspond à aucun service métier connu, ou s’il vient d’être enregistré depuis quelques jours seulement, cela déclenche une alerte de type “flux TLS suspect vers domaine récemment enregistré”, souvent activée par les règles Emerging Threats spécialisées dans la détection de commande et contrôle (C2). Combiné à un volume de données sortant anormalement élevé sur une courte période, détectable via les compteurs de flux Suricata, ce signal permet à une équipe sécurité d’isoler le poste concerné avant qu’une fuite de données ne devienne massive.

Ce type de détection illustre pourquoi un IDS/IPS reste pertinent même dans un monde où l’essentiel du trafic est chiffré : il ne s’agit pas de lire le contenu des communications, mais d’analyser les métadonnées, les motifs de comportement et les indicateurs de compromission connus, une approche que ni un pare-feu ni un antivirus de poste ne couvrent seuls.

## Suricata face à ses alternatives : Snort et Zeek

Suricata n’est pas le seul moteur IDS/IPS open source disponible. Snort, développé historiquement par Sourcefire puis racheté par Cisco, reste très utilisé mais avec un moteur mono-thread historiquement moins performant sur des débits élevés, même si Snort 3 a rattrapé une partie de ce retard. Zeek, de son côté, n’est pas à proprement parler un IDS basé sur signatures, mais un framework d’analyse de trafic orienté scripting, souvent déployé en complément de Suricata plutôt qu’en remplacement, comme le fait justement Security Onion qui embarque les deux moteurs simultanément (Suricata 8.0.5 et Zeek 8.0.8 dans la version 3.1.0 de mai 2026, en attendant une future mise à jour vers le moteur Suricata 8.0.7 publié par l’OISF en septembre 2026).

| Outil | Type de détection | Performance multi-cœur | Cas d’usage typique | 
|---|---|---|---|
| Suricata 8.0.6 | Signatures + DPI + extraction de fichiers | Native, multi-threadée | IDS/IPS généraliste, forte inspection applicative | 
| Snort 3 | Signatures | Améliorée mais historiquement plus limitée | Environnements Cisco, règles Talos | 
| Zeek 8.0.8 | Analyse comportementale scriptée | Native | Investigation forensique, complément à Suricata | 

## Foire aux questions

**Suricata est-il gratuit et open source ?**

Oui. Suricata est distribué sous licence GPLv2 par l’Open Information Security Foundation, une fondation à but non lucratif. Aucune licence n’est requise pour l’utiliser en production, y compris en entreprise.

**Faut-il utiliser Suricata en mode IDS ou en mode IPS ?**

Commencez systématiquement en mode IDS (détection passive) pendant plusieurs semaines pour observer le comportement des règles sur votre trafic réel, avant d’envisager un passage en mode IPS actif sur des flux non critiques.

**Que se passe-t-il si je reste sur Suricata 7 après juillet 2026 ?**

Vous ne recevrez plus aucun correctif de sécurité pour le moteur lui-même, ce qui expose potentiellement votre infrastructure de détection à des vulnérabilités non corrigées. La migration vers la branche 8.0.x est fortement recommandée par l’OISF.

**Suricata peut-il remplacer un pare-feu ?**

Non. Suricata complète un pare-feu comme OPNsense en apportant une inspection de contenu applicative, mais ne gère pas nativement le NAT, le routage ou les politiques de filtrage de base qu’assure un pare-feu périmétrique.

**Combien de RAM et de CPU faut-il pour un déploiement en production ?**

Pour un lien à quelques centaines de Mbit/s, 4 cœurs et 8 Go de RAM suffisent généralement. Au-delà de 1 Gbit/s soutenu, prévoyez davantage de cœurs et envisagez des méthodes de capture accélérées comme PF_RING ou DPDK.

**Quelle est la différence entre Suricata et Security Onion ?**

Suricata est un moteur de détection unique. Security Onion est une distribution Linux complète qui intègre Suricata, Zeek, Elasticsearch et un tableau de bord de visualisation, prête à l’emploi pour un SOC.

**Les règles Emerging Threats Open suffisent-elles ?**

Pour une PME, le ruleset gratuit ET Open couvre déjà une large partie des menaces courantes. Les organisations avec des exigences de détection plus poussées peuvent envisager les règles ET Pro, payantes, mises à jour plus fréquemment.

**Comment tester que Suricata fonctionne sans attendre une vraie attaque ?**

Utilisez la règle de test officielle en effectuant une requête vers `testmynids.org/uid/index.html`, qui déclenche une alerte inoffensive prévue à cet effet, puis vérifiez son apparition dans les logs.