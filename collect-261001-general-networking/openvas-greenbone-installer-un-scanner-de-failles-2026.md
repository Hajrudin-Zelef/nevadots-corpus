Selon le classement Wiz publié en 2026, OpenVAS s’impose comme l’outil de gestion des vulnérabilités le plus utilisé dans les infrastructures open source en Europe, porté par un feed de sécurité mis à jour quotidiennement et couvrant plus de 160 000 tests de détection. Avec la directive NIS2 qui impose désormais des scans de vulnérabilités réguliers à des dizaines de milliers d’entités européennes, la question n’est plus de savoir s’il faut scanner son réseau, mais avec quel outil et à quelle fréquence. Ce tutoriel détaille l’installation complète de Greenbone Community Edition (OpenVAS) sur Debian et Ubuntu, du premier scan jusqu’à l’automatisation des rapports, avec la version 25.0.6 publiée le 6 août 2026.

## Qu’est-ce qu’OpenVAS et pourquoi Greenbone domine la gestion de vulnérabilités en 2026

OpenVAS (Open Vulnerability Assessment Scanner) est le moteur de scan au cœur de Greenbone Community Edition, une suite open source de gestion des vulnérabilités. Concrètement, l’outil interroge chaque machine de votre réseau avec des milliers de tests appelés NVT (Network Vulnerability Tests), compare les résultats à des bases de données de failles connues (CVE), puis génère un rapport classé par gravité selon le score CVSS. Ce n’est pas un simple scanner de ports comme peut l’être Nmap : OpenVAS va chercher les versions de logiciels exposées, teste les mauvaises configurations, et signale les correctifs manquants.

La branche actuelle, OPENVAS SCAN 25.0, est classée en statut de cycle de vie « Mature » par Greenbone, avec un patch 25.0.6 daté du 6 août 2026 et un manuel officiel pour cette même branche 25.0 recensé sur Greenbone.net en août 2026. Ce numéro de suite ne doit pas être confondu avec la version du composant scanner lui-même : sur le dépôt GitHub officiel, la dernière release taguée du moteur OpenVAS Scanner reste la v23.50.24, publiée en août 2026, un numéro également repris par la fiche Wikipedia du projet dans son infobox mise à jour le 31 août 2026 — les deux schémas de version (produit Greenbone vs composant scanner) coexistent et peuvent prêter à confusion lors d’un audit de conformité. Une branche antérieure, OPENVAS SCAN 24.10, reste maintenue avec un patch 24.10.11 publié le 19 février 2026, pour les environnements qui n’ont pas encore migré. Côté appliance, Greenbone a lancé OPENVAS OS 2.0 le 1er juillet 2026, avec un patch 2.0.1 daté du 30 juillet 2026, une version « New » pensée pour les déploiements clé en main en VM ou sur matériel dédié.

Ce qui distingue Greenbone des scanners commerciaux comme Nessus ou Qualys, c’est le modèle open source : le moteur de scan et l’interface web (GSA, Greenbone Security Assistant) sont librement installables, seul l’accès au feed « Enterprise » le plus complet est payant. Le feed communautaire, mis à jour chaque jour, reste largement suffisant pour la majorité des PME, collectivités et laboratoires de test. C’est cette gratuité couplée à une couverture large qui explique pourquoi l’outil revient si souvent dans les benchmarks 2026, y compris pour des cas d’usage sur des hyperviseurs comme Proxmox VE 8.0+ ou Huawei FusionCompute 8.0.

Pour resituer OpenVAS dans une stratégie de sécurité complète, il complète des outils déjà couverts sur ce site comme Nmap pour la découverte réseau, Trivy pour les images de conteneurs ou Wazuh pour la corrélation d’événements. Là où Nmap répond à « quels ports sont ouverts » et Trivy à « quelles failles dans mes images Docker », OpenVAS répond à « quelles vulnérabilités exploitables tournent sur mes serveurs et postes de travail, avec quel niveau de gravité ».

## Comprendre les familles de tests NVT et les profils de scan

Avant de lancer le moindre scan, il faut comprendre comment OpenVAS organise ses tests. Les NVT sont regroupés par familles : détection de services, applications web, bases de données, systèmes d’exploitation, équipements réseau, protocoles industriels (SCADA/ICS), et bien d’autres. Chaque famille peut être activée ou désactivée indépendamment dans une configuration de scan personnalisée, ce qui permet d’adapter précisément la portée à votre environnement plutôt que de lancer systématiquement l’intégralité des 160 000 tests du feed.

Greenbone fournit plusieurs profils de scan préconfigurés, accessibles dans Configuration → Scan Configs. Le profil `Full and fast` couvre l’essentiel des familles pertinentes tout en évitant les tests les plus lents ou les plus intrusifs. Le profil `Full and very deep` active davantage de tests, y compris certains considérés comme potentiellement perturbateurs pour les services fragiles, et rallonge la durée du scan de façon significative. Le profil `Discovery`, plus léger, se limite à l’identification des services et versions sans chercher activement à exploiter une faille, ce qui en fait un bon point de départ pour cartographier un réseau inconnu avant d’aller plus loin.

Pour un environnement de production sensible, la meilleure pratique consiste à créer une configuration de scan sur mesure : partez du profil `Full and fast`, dupliquez-le, puis désactivez manuellement les familles de tests non pertinentes pour votre parc (par exemple les tests SCADA si vous n’avez aucun automate industriel, ou les tests spécifiques à des CMS que vous n’utilisez pas). Ce réglage réduit à la fois la durée du scan et le risque de faux positifs liés à des tests hors sujet.

## Prérequis avant d’installer OpenVAS / Greenbone Community Edition

Avant de démarrer, vérifiez que votre environnement correspond aux exigences minimales. Greenbone Community Edition est gourmand en ressources car il combine une base PostgreSQL, un moteur de scan multithread et un feed de plusieurs gigaoctets à synchroniser au premier lancement. Pour les puristes qui préfèrent une installation depuis les sources plutôt que Docker, un script d’installation Debian 12 maintenu sur GitHub (publié en février 2024 et mis à jour en octobre 2025) exige un système Debian 12 propre et entièrement à jour, et expose ensuite l’interface web à l’adresse https://

| Composant | Minimum | Recommandé (PME/collectivité) | 
|---|---|---|
| Système d’exploitation | Debian 12 / Ubuntu 22.04 LTS | Debian 13 / Ubuntu 24.04 LTS | 
| RAM | 4 Go | 16 Go ou plus | 
| CPU | 2 vCPU | 4 vCPU ou plus | 
| Stockage | 20 Go | 100 Go SSD (feed + rapports) | 
| Docker Engine | 24.x | 27.x ou supérieur | 
| Docker Compose | v2.20 | v2.30 ou supérieur | 
| Bande passante Internet | 10 Mbps | 50 Mbps (synchro feed quotidienne) | 

Vous aurez aussi besoin d’un accès root ou sudo sur la machine cible, d’une adresse IP fixe ou d’un nom de domaine interne pour accéder à l’interface web, et idéalement d’un second réseau isolé (VLAN de scan) si vous comptez auditer des systèmes de production. Ne lancez jamais un premier scan complet directement sur un réseau de production sans avoir prévenu les équipes : un scan agressif peut faire planter des équipements réseau ou des IoT fragiles.

Enfin, gardez à l’esprit un point de conformité : au sein de l’Union européenne, scanner un système sans autorisation explicite du propriétaire peut relever de l’article 323-1 du Code pénal français (accès frauduleux à un système de traitement automatisé de données). Ce tutoriel s’adresse à des scans réalisés sur votre propre infrastructure ou dans le cadre d’un mandat écrit.

Si vous testez d’abord sur une machine virtuelle avant un déploiement en production, prévoyez un instantané (snapshot) juste avant l’installation. Docker isole bien les services entre eux, mais une mauvaise manipulation du fichier `docker-compose.yml` ou une coupure réseau pendant la synchronisation initiale du feed peut laisser la base de données dans un état incohérent qu’il est plus rapide de restaurer que de déboguer.

## Étape 1 : préparer le serveur Debian ou Ubuntu

Commencez par mettre à jour le système et installer les paquets de base nécessaires à Docker et à la gestion du pare-feu.

```
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl ca-certificates gnupg lsb-release ufw
# Vérifier l'espace disque disponible (minimum 20 Go recommandé)
df -h /
# Vérifier la RAM disponible
free -h
```
Configurez ensuite un utilisateur dédié plutôt que de tout exécuter en root, par principe de moindre privilège :

```
sudo adduser gvmadmin
sudo usermod -aG sudo gvmadmin
su - gvmadmin
```
## Étape 2 : installer Docker Engine et Docker Compose

Greenbone Community Edition se déploie officiellement via Docker Compose depuis 2023, ce qui simplifie énormément la gestion des multiples services (scanner, base de données, feed, interface web). Un guide comparatif publié par Secrails en mai 2026 confirme d’ailleurs que les Greenbone Community Containers restent la voie d’installation la plus propre, alors que les installations via apt sur Ubuntu 22.04 continuent de se heurter à des conflits de dépendances, Kali Linux demeurant de son côté l’option bare-metal la moins friction. Ajoutez le dépôt officiel Docker :

```
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
# Ajouter votre utilisateur au groupe docker
sudo usermod -aG docker $USER
newgrp docker
# Vérifier l'installation
docker --version
docker compose version
```
Sortie attendue à ce stade :

```
Docker version 27.3.1, build ce12230
Docker Compose version v2.30.3
```
## Étape 3 : télécharger et lancer Greenbone Community Container

Greenbone publie un dépôt Docker Compose officiel qui orchestre l’ensemble des microservices : gvmd (le démon de gestion), le scanner OpenVAS, la base PostgreSQL, le service de mise à jour du feed NVT et l’interface GSA.

```
mkdir -p ~/greenbone-ce && cd ~/greenbone-ce
curl -f -L https://greenbone.github.io/docs/latest/_static/docker-compose-22.4.yml -o docker-compose.yml
curl -f -L https://greenbone.github.io/docs/latest/_static/gvm.conf -o gvm.conf
# Lancer les conteneurs en arrière-plan
docker compose up -d
# Vérifier que tous les services tournent
docker compose ps
```
Le premier démarrage déclenche automatiquement la synchronisation du feed de vulnérabilités : plus de 160 000 tests NVT, les données CERT, les correspondances CVE et les scripts CVSS. Cette synchronisation initiale peut prendre entre 30 minutes et plusieurs heures selon votre bande passante. Suivez la progression avec :

`docker compose logs -f pg-gvm gvmd`
## Étape 4 : récupérer le mot de passe administrateur généré

À la création du conteneur, Greenbone génère automatiquement un compte `admin` avec un mot de passe aléatoire, visible dans les logs du conteneur `gvmd` :

```
docker compose logs gvmd | grep -i "user created"
# Exemple de sortie :
# gvmd: User created with password 'K7x-mP2q-Rv9t-Zb4w'
```
Changez immédiatement ce mot de passe une fois connecté à l’interface web. Vous pouvez aussi le réinitialiser en ligne de commande si les logs ne l’affichent plus :

`docker compose exec gvmd gvmd --user=admin --new-password='VotreMotDePasseFort123!'`
## Étape 5 : accéder à l’interface Greenbone Security Assistant (GSA)

Ouvrez un navigateur et rendez-vous sur `https://IP_DU_SERVEUR:9392`. Le certificat TLS auto-signé par défaut déclenchera un avertissement de sécurité, normal pour un premier accès local. Connectez-vous avec le compte `admin` et le mot de passe récupéré à l’étape précédente.

Dans le tableau de bord, vérifiez trois indicateurs avant de lancer le moindre scan : le statut du feed NVT (menu Administration → Feed Status), qui doit afficher « Current », la date de dernière synchronisation, et le nombre de tests chargés (autour de 160 000 en cas de feed communautaire complet).

## Étape 6 : configurer un premier scan cible

Créez d’abord une cible (Target) dans le menu Configuration → Targets. Renseignez une plage IP restreinte pour ce premier test, par exemple un seul serveur non critique :

- Nom : `Test-Serveur-Web-01`
- Hosts : `192.168.1.50`
- Méthode de découverte de port : `OpenVAS Default`
- Alive Test : `ICMP, TCP-ACK Service Ping, ARP Ping`

Rendez-vous ensuite dans Scans → Tasks, cliquez sur « New Task », associez la cible créée et sélectionnez une configuration de scan. Pour un premier passage, privilégiez le profil `Full and fast`, qui équilibre couverture et durée sans saturer la cible.

## Étape 7 : automatiser les scans via l’API GMP (Greenbone Management Protocol)

Pour intégrer OpenVAS dans un pipeline d’automatisation ou un cron, utilisez le protocole GMP, disponible en version 22.8 et 22.7 dans la documentation Greenbone mise à jour le 12 août 2026. Le client Python `python-gvm` reste la méthode la plus fiable pour scripter des scans récurrents.

```
pip install python-gvm
python3 - <<'EOF'
from gvm.connections import TLSConnection
from gvm.protocols.gmp import Gmp
from gvm.transforms import EtreeCheckCommandTransform
connection = TLSConnection(hostname="192.168.1.10", port=9390)
transform = EtreeCheckCommandTransform()
with Gmp(connection=connection, transform=transform) as gmp:
    gmp.authenticate("admin", "VotreMotDePasseFort123!")
    version = gmp.get_version()
    print(version)
    # Lancer une tâche existante par son UUID
    gmp.start_task("11111111-2222-3333-4444-555555555555")
EOF
```
Planifiez ensuite un cron hebdomadaire qui déclenche ce script chaque lundi à 2h du matin, hors heures de bureau, pour limiter l'impact sur la production :

```
crontab -e
# Ajouter la ligne suivante :
0 2 * * 1 /usr/bin/python3 /home/gvmadmin/scripts/scan_hebdo.py >> /var/log/openvas_scan.log 2>&1
```
## Étape 8 : lire et prioriser un rapport de vulnérabilités

Une fois le scan terminé, Greenbone classe chaque résultat selon le score CVSS v3.1, calculé via la même méthodologie que le calculateur officiel du FIRST. Ne traitez pas toutes les failles de la même façon : concentrez-vous d'abord sur les scores critiques et sur les vulnérabilités exploitées activement.

| Score CVSS | Niveau | Délai de correction recommandé | 
|---|---|---|
| 9.0 – 10.0 | Critique | 24 à 72 heures | 
| 7.0 – 8.9 | Élevé | 7 jours | 
| 4.0 – 6.9 | Moyen | 30 jours | 
| 0.1 – 3.9 | Faible | 90 jours ou cycle de patch normal | 

Croisez systématiquement les identifiants CVE remontés avec la base officielle du CVE Program pour confirmer la description exacte de la faille et vérifier si un correctif existe déjà côté éditeur.

## Étape 9 : exporter les rapports en PDF et CSV pour l'audit NIS2

Dans le menu Scans → Reports, sélectionnez un rapport terminé puis « Export » pour choisir un format. Pour un dossier de conformité NIS2 ou un rapport à destination d'un RSSI, le format PDF avec le modèle « Executive Summary » reste le plus lisible. Pour une intégration dans un tableau de bord ou un outil tiers, exportez en CSV ou en XML :

```
# Via l'API GMP, exporter un rapport au format CSV
gmp.get_report(report_id="abc-123", filter_string="apply_overrides=0 min_qod=70", format_id="c1645568-627a-11e3-a660-406186ea4fc5")
```
Conservez ces rapports au moins 12 mois dans un stockage séparé du serveur Greenbone lui-même, idéalement chiffré et avec un accès restreint. En cas d'audit NIS2 ou de contrôle de la CNIL suite à un incident, la capacité à démontrer un historique continu de scans et de remédiation pèse souvent plus lourd que le résultat d'un seul rapport isolé. Beaucoup d'organisations découvrent trop tard, au moment de l'audit, qu'elles n'ont conservé aucune trace des scans réalisés l'année précédente.

## Étape 10 : configurer des scans authentifiés (credentialed scans)

Un scan non authentifié ne voit que la surface exposée depuis le réseau. Pour détecter les failles internes (versions de paquets installés, correctifs Windows manquants, mauvaises configurations locales), configurez des identifiants dans Configuration → Credentials, puis associez-les à votre cible.

- Pour Linux : créez un couple SSH clé publique/privée dédié au scan, avec un compte à droits limités plutôt que root
- Pour Windows : utilisez un compte de service avec accès WMI/SMB, jamais un compte administrateur de domaine
- Stockez ces identifiants uniquement dans le coffre-fort intégré de Greenbone, jamais en clair dans un script

Les scans authentifiés remontent en moyenne deux à trois fois plus de vulnérabilités que les scans réseau seuls, car ils inspectent directement l'état du système plutôt que de deviner ses failles depuis l'extérieur. C'est cette profondeur qui distingue un vrai programme de gestion des vulnérabilités d'un simple scan de surface, et c'est aussi le réglage que les auditeurs NIS2 vérifient en priorité lorsqu'ils examinent la méthodologie de scan d'une entité essentielle ou importante.

## Étape 11 : intégrer OpenVAS avec Proxmox VE et les environnements virtualisés

Le classement Wiz 2026 souligne la prise en charge d'OpenVAS pour les infrastructures virtualisées type Proxmox VE 8.0 et supérieur, ou Huawei FusionCompute 8.0. Dans ce contexte, créez une cible groupée par VLAN de gestion plutôt que de scanner chaque hyperviseur individuellement, et excluez les interfaces de migration à chaud (souvent non chiffrées et sensibles aux interruptions) de la portée du scan.

```
# Exemple de plage ciblée pour un cluster Proxmox (3 nœuds)
10.10.20.11-10.10.20.13
# Exclure l'interface de migration à chaud (souvent sur un VLAN dédié)
# via le champ "Exclude Hosts" du formulaire Target
```
## Étape 12 : mettre à jour vers OPENVAS SCAN 25.0.6 ou migrer depuis 24.10

Si votre installation tourne encore sur la branche 24.10.x, planifiez une migration vers 25.0.6 lors d'une fenêtre de maintenance. Notez que certains dépôts d'installation communautaires sur GitHub annoncent depuis août 2025 la prise en charge d'une mise à niveau automatique (auto upgrade), une option à évaluer avec prudence en production avant de vous y fier entièrement. Sauvegardez d'abord la base PostgreSQL et le volume de configuration :

```
# Sauvegarde avant migration
docker compose exec pg-gvm pg_dumpall -U gvmd > backup_gvmd_$(date +%F).sql
# Mise à jour de l'image et redémarrage
docker compose pull
docker compose up -d
# Vérifier la version après migration
docker compose exec gvmd gvmd --version
```
Consultez la documentation officielle de Greenbone TechDoc avant toute migration en production, car chaque montée de version majeure peut modifier le schéma de base de données.

## Étape 13 : durcir l'accès à l'interface web GSA

L'interface GSA donne accès à l'ensemble des identifiants de scan, des rapports de vulnérabilités et des cibles de votre réseau : c'est une cible de choix si elle reste exposée sans protection supplémentaire. Trois réglages simples réduisent fortement le risque. D'abord, limitez l'accès au port 9392 par pare-feu à une liste d'IP autorisées plutôt que de l'ouvrir largement, même en interne.

```
# Restreindre l'accès au port GSA à un sous-réseau d'administration
sudo ufw allow from 192.168.10.0/24 to any port 9392 proto tcp
sudo ufw deny 9392/tcp
```
Ensuite, remplacez le certificat TLS auto-signé par un certificat émis par une autorité interne ou par Let's Encrypt si l'interface est exposée derrière un reverse proxy comme Nginx ou Traefik. Enfin, désactivez le compte `admin` par défaut une fois un second compte administrateur nominatif créé, et activez la journalisation des connexions pour repérer toute tentative d'accès suspecte. Ces trois réglages combinés transforment une interface d'administration ouverte en un point d'accès correctement cloisonné, conforme aux attentes d'un audit NIS2 sur le contrôle des accès.

## Quelle fréquence de scan choisir selon votre secteur d'activité

La fréquence idéale dépend directement de la criticité des systèmes exposés et du niveau de réglementation applicable. Une collectivité soumise à NIS2 n'a pas les mêmes obligations qu'un hébergeur SecNumCloud ou qu'une PME sans exigence sectorielle particulière, mais dans tous les cas, un scan trimestriel isolé ne suffit plus face au rythme de publication des nouvelles CVE.

| Profil d'organisation | Fréquence recommandée | Portée | 
|---|---|---|
| Entité essentielle NIS2 (santé, énergie, transport) | Hebdomadaire ou continue | Tous les actifs exposés + scan authentifié mensuel | 
| Collectivité territoriale | Hebdomadaire | Serveurs exposés Internet, mensuel pour le parc interne | 
| PME sans obligation réglementaire | Mensuelle | Serveurs de production, sites web publics | 
| Hébergeur / infogéreur | Continue (à chaque changement) | Ensemble du parc client, déclenché par la CI/CD | 
| Environnement de test / laboratoire | À la demande | Selon les campagnes de test | 

Dans la pratique, la meilleure approche combine un scan léger et fréquent (hebdomadaire, orienté détection de nouvelles failles critiques) avec un scan complet et authentifié mensuel qui couvre l'intégralité du parc, y compris les correctifs internes. Cette cadence à deux vitesses évite de saturer les équipes tout en gardant une visibilité proche du temps réel sur les vulnérabilités critiques nouvellement publiées.

## 5 erreurs fréquentes lors de l'installation d'OpenVAS

Ces pièges reviennent le plus souvent dans les forums communautaires et les retours d'administrateurs qui déploient Greenbone pour la première fois.

- **Lancer un scan avant la fin de la synchronisation du feed.** Le premier scan sur un feed incomplet donne des résultats faussement rassurants, car de nombreux NVT ne sont pas encore chargés.
- **Scanner sans autorisation écrite.** Même sur son propre réseau d'entreprise, l'absence de mandat clair peut poser problème en cas d'incident pendant le scan.
- **Sous-dimensionner la RAM.** En dessous de 4 Go, le conteneur PostgreSQL et le moteur de scan se disputent la mémoire, provoquant des scans qui échouent à 0 % (un problème régulièrement signalé sur le forum communautaire Greenbone en août 2026, notamment sur Kali Linux).
- **Ignorer les faux positifs.** Un scan non authentifié peut signaler une faille sur un service dont la bannière de version a été modifiée volontairement (hardening), sans que la faille existe réellement. Vérifiez toujours manuellement les résultats critiques.
- **Oublier de renouveler le certificat TLS de l'interface GSA.** Le certificat auto-signé par défaut expire au bout de 12 mois et bloque l'accès à l'interface sans message d'erreur explicite dans certains navigateurs.

## Dépannage : 8 problèmes courants et leurs solutions

Voici les incidents les plus signalés lors du déploiement de Greenbone Community Edition, avec la commande ou le réglage qui permet généralement de les résoudre.

| Symptôme | Cause probable | Solution | 
|---|---|---|
| Scan bloqué à 0 % | Feed NVT incomplet ou RAM insuffisante | Vérifier Administration → Feed Status et augmenter la RAM à 8 Go minimum | 
| Interface GSA inaccessible sur le port 9392 | Pare-feu UFW bloque le port | `sudo ufw allow 9392/tcp` | 
| Erreur « Feed not synced » | Synchronisation initiale en cours ou échouée | `docker compose logs -f nvt-feed-sync` pour suivre la progression | 
| Mot de passe admin perdu | Logs de création déjà purgés | Réinitialiser via `gvmd --user=admin --new-password=` | 
| Scan authentifié échoue en SSH | Clé publique non déployée sur la cible | Copier la clé avec `ssh-copy-id` vers le compte de scan dédié | 
| Conteneur pg-gvm redémarre en boucle | Volume de données corrompu | Restaurer depuis la dernière sauvegarde `pg_dumpall` | 
| Rapport vide malgré un scan « terminé » | Filtre de qualité de détection (QoD) trop strict | Réduire `min_qod` dans le filtre du rapport, ex. de 70 à 30 | 
| API GMP refuse la connexion Python | Port 9390 non exposé ou certificat TLS invalide | Vérifier `docker compose ps` et le mapping de port dans`docker-compose.yml` | 
| Synchronisation du feed très lente | Bande passante limitée ou blocage proxy sortant | Vérifier la connectivité vers les serveurs Greenbone et augmenter le délai d'expiration du service | 
| Interface GSA lente à charger les rapports volumineux | Base PostgreSQL non indexée ou trop de résultats affichés | Filtrer par niveau de gravité minimum et paginer les résultats plutôt que tout afficher | 

## Conseils avancés pour une gestion de vulnérabilités mature

Une fois l'installation stabilisée, plusieurs pratiques permettent de passer d'un simple scanner ponctuel à un vrai programme de gestion des vulnérabilités, aligné avec les attentes de la directive NIS2 pour les entités essentielles et importantes.

Segmentez vos cibles par criticité métier plutôt que par simple plage IP : un groupe « Serveurs exposés Internet » scanné quotidiennement, un groupe « Postes de travail internes » scanné hebdomadairement, un groupe « OT/IoT » scanné mensuellement avec un profil léger pour éviter les interruptions de service. Ce découpage évite de noyer les équipes sous des rapports ingérables et concentre l'attention là où le risque est le plus élevé.

Couplez OpenVAS à un SIEM comme Wazuh pour corréler automatiquement une vulnérabilité détectée avec une tentative d'exploitation observée dans les logs, plutôt que de traiter les deux flux séparément. Beaucoup d'équipes découvrent une faille critique via un scan, sans jamais vérifier si elle a déjà été exploitée dans les semaines précédentes faute de corrélation.

Mettez en place un tableau de bord de suivi des délais de remédiation (mean time to remediate), avec un objectif réaliste : 72 heures pour le critique, 7 jours pour l'élevé. Un scanner qui tourne sans suivi de correction produit des rapports que personne ne lit après la troisième semaine.

Enfin, ne vous fiez jamais à un seul outil. OpenVAS excelle sur la détection réseau et système, mais reste complémentaire d'un scanner de code statique, d'un outil de test d'intrusion applicatif et d'une veille CERT active, comme celle publiée par le CERT-FR de l'ANSSI.

Pensez aussi à gérer les faux positifs de façon structurée plutôt qu'au cas par cas. Greenbone permet de créer des « Overrides », des règles qui reclassent automatiquement un résultat donné (par exemple une faille sur un service dont vous savez qu'il est déjà corrigé par un correctif compensatoire) sans supprimer l'information du rapport brut. Documentez systématiquement la justification de chaque override avec une date de réexamen, sans quoi ces exceptions s'accumulent et finissent par masquer de vraies régressions lors d'une mise à jour ultérieure.

Sur le plan organisationnel, évitez de confier la lecture des rapports à une seule personne. Un tableau de répartition simple, par exemple par équipe applicative ou par périmètre technique, accélère la remédiation et évite le goulot d'étranglement classique où un seul administrateur croule sous des centaines d'alertes chaque semaine sans pouvoir toutes les traiter à temps.

## Projet complet : script d'automatisation hebdomadaire avec alerte email

Voici un exemple de projet fonctionnel qui lance un scan complet chaque semaine, exporte le rapport en PDF et envoie une alerte email si des failles critiques sont détectées. Adaptez les identifiants et l'UUID de tâche à votre environnement.

```
#!/usr/bin/env python3
import smtplib
import time
from email.mime.text import MIMEText
from gvm.connections import TLSConnection
from gvm.protocols.gmp import Gmp
from gvm.transforms import EtreeCheckCommandTransform
TASK_ID = "11111111-2222-3333-4444-555555555555"
GVM_HOST = "192.168.1.10"
ADMIN_USER = "admin"
ADMIN_PASS = "VotreMotDePasseFort123!"
ALERT_EMAIL = "[email protected]"
def envoyer_alerte(nb_critiques):
    msg = MIMEText(f"{nb_critiques} vulnérabilité(s) critique(s) détectée(s) lors du scan hebdomadaire OpenVAS.")
    msg["Subject"] = "[ALERTE] Scan OpenVAS - Vulnérabilités critiques"
    msg["From"] = "[email protected]"
    msg["To"] = ALERT_EMAIL
    with smtplib.SMTP("localhost") as s:
        s.send_message(msg)
def main():
    connection = TLSConnection(hostname=GVM_HOST, port=9390)
    transform = EtreeCheckCommandTransform()
    with Gmp(connection=connection, transform=transform) as gmp:
        gmp.authenticate(ADMIN_USER, ADMIN_PASS)
        gmp.start_task(TASK_ID)
        # Attendre la fin du scan (vérification toutes les 5 minutes)
        while True:
            status = gmp.get_task(TASK_ID).find(".//status").text
            if status == "Done":
                break
            time.sleep(300)
        report = gmp.get_reports(filter_string=f"task_id={TASK_ID}")
        nb_critiques = report.xpath("count(//result[threat='High'])")
        if int(nb_critiques) > 0:
            envoyer_alerte(int(nb_critiques))
if __name__ == "__main__":
    main()
```
Planifiez ce script avec le cron déjà configuré à l'étape 7, et conservez un historique des rapports dans un dossier daté pour constituer votre dossier de conformité NIS2 ou de certification ISO 27001.

## OpenVAS face aux autres scanners de vulnérabilités en 2026

Pour situer Greenbone Community Edition, voici un comparatif rapide avec les alternatives les plus citées par les équipes sécurité européennes.

| Outil | Modèle | Tests de vulnérabilités | Cas d'usage privilégié | 
|---|---|---|---|
| OpenVAS / Greenbone CE | Open source, gratuit | plus de 160 000 (feed communautaire) | PME, collectivités, labos, conformité NIS2 | 
| Nessus Essentials | Gratuit jusqu'à 16 IP | environ 90 000 | Petits environnements, formation | 
| Qualys VMDR | SaaS commercial | propriétaire, mis à jour en continu | Grands groupes, cloud multi-comptes | 
| Trivy | Open source, gratuit | bases CVE pour conteneurs/IaC | Pipelines CI/CD, images Docker | 

Le choix dépend surtout du périmètre : OpenVAS reste la meilleure option gratuite pour un scan réseau et système large, tandis que Trivy se concentre sur les conteneurs et Qualys cible les environnements cloud à grande échelle avec un budget dédié.

## Coût réel : OpenVAS gratuit face aux solutions commerciales

Le principal argument en faveur d'OpenVAS reste financier. Là où un abonnement Qualys VMDR ou Tenable Nessus Professional se facture par nombre d'actifs scannés, souvent plusieurs milliers d'euros par an dès quelques centaines de machines, Greenbone Community Edition ne coûte rien en licence. Le coût réel se déplace vers l'infrastructure (un serveur dédié avec suffisamment de RAM) et le temps d'administration nécessaire pour maintenir l'outil, lire les rapports et suivre la remédiation.

Pour une collectivité ou une PME de moins de 200 postes, ce compromis est presque toujours favorable à OpenVAS : le coût d'un serveur virtuel dédié (souvent inférieur à 50 euros par mois chez un hébergeur européen) reste très inférieur à une licence commerciale équivalente. Pour un grand groupe avec plusieurs milliers d'actifs répartis sur de multiples clouds, l'équation change : le temps d'administration et le manque d'automatisation avancée (tableau de bord multi-tenant, intégration SSO poussée, support commercial avec SLA) peuvent justifier le passage à une solution payante comme Greenbone Enterprise ou un concurrent commercial.

Un point souvent négligé dans ce calcul : le coût d'une absence de scan est presque toujours supérieur au coût d'un outil, gratuit ou payant. Les rapports d'incident analysés année après année montrent régulièrement que la majorité des compromissions exploitent des vulnérabilités connues depuis plusieurs mois, jamais corrigées faute de visibilité. Un outil gratuit correctement configuré et suivi vaut largement mieux qu'un outil payant acheté puis laissé à l'abandon après la démonstration initiale.

## Foire aux questions sur OpenVAS et Greenbone

### OpenVAS est-il vraiment gratuit ?

Oui, le moteur de scan, l'interface GSA et le feed communautaire de tests de vulnérabilités sont entièrement gratuits sous licence open source. Seul le feed « Enterprise », avec une couverture élargie et un support commercial, est payant et vendu par Greenbone AG.

### Combien de temps dure un scan complet avec OpenVAS ?

Cela dépend du nombre de cibles, du profil choisi et de la présence de scans authentifiés. Un scan « Full and fast » sur un seul serveur prend généralement entre 20 minutes et 2 heures. Sur un sous-réseau de 50 machines, comptez plusieurs heures.

### OpenVAS peut-il faire planter un serveur pendant un scan ?

Certains tests actifs, notamment sur des équipements réseau ou IoT fragiles, peuvent provoquer des instabilités. C'est pourquoi il est recommandé d'utiliser des profils de scan moins agressifs sur les systèmes critiques et de toujours tester d'abord hors production.

### La directive NIS2 impose-t-elle explicitement OpenVAS ?

Non, la directive NIS2 n'impose aucun outil précis, mais elle exige une gestion des risques incluant l'identification régulière des vulnérabilités. OpenVAS répond à cette exigence sans coût de licence, ce qui explique son adoption croissante chez les entités essentielles et importantes de taille modeste.

### Quelle est la différence entre OpenVAS et Greenbone Community Edition ?

OpenVAS désigne historiquement le moteur de scan lui-même. Greenbone Community Edition est la suite complète qui inclut ce moteur, la base de données, l'interface web GSA et le service de synchronisation du feed. Dans l'usage courant, les deux noms sont souvent utilisés l'un pour l'autre.

### Peut-on utiliser OpenVAS sur un cluster Proxmox en production ?

Oui, à condition d'exclure les interfaces de migration à chaud de la portée du scan et de privilégier un scan authentifié plutôt qu'un scan réseau agressif, afin de limiter le risque d'interruption sur les nœuds actifs.

### Faut-il un serveur dédié pour faire tourner Greenbone Community Edition ?

Ce n'est pas obligatoire pour un petit environnement de test, mais fortement recommandé en production. Le scanner consomme beaucoup de CPU et de bande passante pendant les scans actifs, ce qui peut ralentir d'autres services hébergés sur la même machine.

### Comment savoir si mon feed OpenVAS est à jour ?

Rendez-vous dans Administration → Feed Status sur l'interface GSA. Le statut doit afficher « Current » avec une date de synchronisation datant de moins de 24 heures, puisque le feed communautaire est mis à jour quotidiennement.

### Peut-on scanner un site web ou une API avec OpenVAS ?

OpenVAS détecte les vulnérabilités classiques d'un serveur web (versions obsolètes, mauvaises configurations TLS, en-têtes de sécurité manquants) mais reste limité pour l'analyse applicative en profondeur d'une API REST ou d'une logique métier spécifique. Pour ce périmètre, un scanner applicatif dédié (DAST) ou un test d'intrusion manuel complète utilement les résultats d'OpenVAS.

### Que faire si un scan OpenVAS remonte trop de faux positifs ?

Passez d'abord à un scan authentifié plutôt qu'un scan réseau seul : la majorité des faux positifs viennent d'une détection de version approximative basée sur une bannière de service, que le scan authentifié corrige en lisant directement l'état réel du système. Utilisez ensuite les Overrides pour documenter les exceptions confirmées, plutôt que d'ignorer silencieusement les résultats douteux.

Pour aller plus loin dans la sécurisation de votre infrastructure après ce scan initial, pensez à croiser vos résultats avec les recommandations de l'ENISA, à consulter la documentation officielle sur openvas.org et le site de l'éditeur Greenbone, puis à documenter chaque correctif appliqué : c'est ce dossier de preuve qui sera demandé lors d'un audit NIS2 ou d'une certification ISO 27001.