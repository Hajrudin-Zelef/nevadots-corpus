---
id: collect-261121-veille-ia/windows-os/gophish-simuler-une-campagne-de-phishing-en-13-etapes
title: "GoPhish : Simuler une Campagne de Phishing en 13 Étapes"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["gophish", "phishing", "campagne", "simuler", "bien", "open", "source", "mesurer"]
source: https://tech-insider.org/fr/gophish-simuler-campagne-phishing-2026/
source_anchor: ""
source_lines: [1, 403]
section: "Windows_OS"
---
Simuler une campagne de phishing en interne n’est plus réservé aux grandes banques ou aux ministères. Avec GoPhish, un framework open source écrit en Go, n’importe quelle PME ou DSI peut monter en quelques heures un exercice de sensibilisation réaliste, mesurer qui clique, qui saisit ses identifiants et qui signale l’email suspect. Le contexte y pousse : dans son panorama de la cybermenace 2025 publié le 13 mai 2026, l’ANSSI note que les attaquants s’appuient de plus en plus sur des services commerciaux et open source détournés pour cibler les entités françaises, souvent via l’ingénierie sociale. Ce tutoriel détaille, étape par étape, l’installation de GoPhish sur un serveur Linux, la configuration d’une campagne complète et les pièges à éviter, dans un cadre strictement légal et autorisé.

**Avertissement** : cet article s’adresse aux RSSI, administrateurs systèmes et équipes de sensibilisation qui testent leur propre organisation avec une autorisation écrite de la direction. Utiliser GoPhish contre des cibles qui n’ont pas donné leur accord constitue une infraction pénale en France (article 323-1 du Code pénal). Ne déployez jamais cet outil hors d’un périmètre autorisé.

## Pourquoi simuler des campagnes de phishing en 2026

Le phishing reste le premier vecteur d’intrusion en entreprise, et il change de nature. Les attaquants automatisent désormais la rédaction de leurs emails avec des générateurs de texte, ce qui réduit les fautes d’orthographe qui trahissaient autrefois les tentatives frauduleuses. Résultat : les employés distinguent de moins en moins bien un email légitime d’un email piégé. Une campagne de simulation GoPhish permet de mesurer ce risque concrètement, avec des chiffres exploitables plutôt qu’une simple formation théorique.

L’intérêt de GoPhish tient à trois choses. D’abord, c’est un outil entièrement gratuit et open source, disponible sur GitHub, sans licence à négocier. Ensuite, il tourne aussi bien sur une petite VM Linux que sur un serveur dédié, avec une empreinte système minimale puisqu’il est écrit en Go et distribué sous forme de binaire unique. Enfin, son modèle de données (profils d’envoi, modèles d’email, pages d’atterrissage, groupes de cibles, campagnes) reproduit fidèlement le déroulé d’une vraie attaque de phishing, ce qui rend les résultats pédagogiques pour les collaborateurs testés.

Ce tutoriel couvre l’installation depuis zéro, la sécurisation du service (TLS, pare-feu, utilisateur dédié, systemd), la création d’une campagne complète avec profil SMTP, modèle d’email et page d’atterrissage, l’analyse des résultats, ainsi que le cadre juridique français à respecter avant tout lancement.

Il ne s’agit pas d’un exercice ponctuel à cocher une fois par an pour satisfaire un audit. Les équipes qui obtiennent les meilleurs résultats traitent la simulation de phishing comme un cycle continu : mesurer, former, remesurer. Le rapport 2025 de l’ANSSI souligne d’ailleurs que l’ingénierie sociale reste le point d’entrée privilégié des attaquants, précisément parce qu’elle contourne les protections techniques classiques comme les pare-feux ou les antivirus. Un firewall bien configuré ne sert à rien si un comptable saisit ses identifiants sur une fausse page de connexion. C’est cette faille humaine, difficile à corriger par des correctifs logiciels, que GoPhish permet de quantifier puis de réduire progressivement.

## Comment fonctionne l’architecture de GoPhish

Avant de se lancer dans l’installation, il est utile de comprendre comment GoPhish organise ses composants, car cela évite ensuite bien des erreurs de configuration. Le binaire unique embarque en réalité deux serveurs HTTP distincts qui tournent dans le même processus mais écoutent sur des ports différents.

Le premier, le serveur d’administration, expose l’interface web depuis laquelle vous pilotez tout : création des profils SMTP, des modèles d’email, des pages d’atterrissage, des groupes de cibles et des campagnes. Cette interface communique avec une API REST interne, la même que vous pouvez interroger directement en HTTP si vous souhaitez automatiser la création de campagnes depuis un script externe plutôt que de tout faire à la souris.

Le second, le serveur de phishing, est celui qui reçoit les visites des cibles lorsqu’elles cliquent sur le lien contenu dans l’email. C’est lui qui génère l’identifiant unique par destinataire (visible dans l’URL sous forme de paramètre), qui sert la page d’atterrissage correspondante, et qui enregistre chaque étape du parcours de la cible dans la base de données. Les deux serveurs partagent la même base SQLite par défaut (`gophish.db`), ce qui explique pourquoi un problème de permissions sur ce fichier peut faire planter l’ensemble de l’application, pas seulement une partie.

Cette séparation en deux serveurs a une conséquence pratique directe pour le déploiement : vous pouvez exposer publiquement uniquement le serveur de phishing tout en gardant le serveur d’administration strictement privé, ce qui réduit considérablement la surface d’attaque de votre propre outil de test.

## Prérequis techniques et légaux

Avant de commencer, réunissez les éléments suivants. Ils conditionnent la réussite de l’installation et surtout la légalité de la campagne.

- Un serveur Linux (Debian 12, Ubuntu 22.04/24.04 ou équivalent) avec au moins 1 vCPU, 1 Go de RAM et 10 Go de disque
- Un accès root ou sudo sur ce serveur
- GoPhish en version 0.12.1, la dernière publiée sur le dépôt officiel GitHub
- Un relais SMTP autorisé à envoyer des emails pour votre domaine de test (serveur interne, ou service tiers configuré avec SPF/DKIM)
- Un nom de domaine ou sous-domaine dédié à la simulation (jamais le domaine de production de l’entreprise)
- Un certificat TLS valide pour l’interface d’administration (Let’s Encrypt convient)
- Une autorisation écrite signée par la direction générale et le DPO/RSSI, précisant le périmètre, la durée et les destinataires de la campagne
- Une liste de cibles au format CSV avec au minimum une colonne email

Sur le plan juridique, le guide RGPD de la CNIL sur la sécurité des données personnelles rappelle que toute collecte de données comportementales sur les employés (qui a cliqué, qui a saisi ses identifiants) doit respecter les principes de proportionnalité, de minimisation et de transparence. Concrètement, informez les salariés en amont qu’une campagne de sensibilisation incluant d’éventuelles simulations de phishing peut avoir lieu, consultez le CSE si nécessaire, et ne réutilisez jamais les résultats individuels à des fins disciplinaires. Consultez le guide pratique RGPD de la CNIL avant de lancer votre première campagne.

En pratique, la plupart des DSI structurent leur autorisation autour de quatre points fixes : le périmètre exact des personnes testées (par service, par site, ou l’ensemble de l’organisation), la durée de la campagne, le type de scénario simulé (facture frauduleuse, fausse alerte RH, fausse mise à jour de mot de passe), et la finalité déclarée de la collecte de données, qui doit rester la formation et jamais l’évaluation individuelle des salariés. Ce document signé sert aussi de preuve en cas de question du CSE ou d’un salarié qui découvrirait, après coup, qu’il a été ciblé par un test.

## Étape 1 : préparer le serveur Linux

Commencez par mettre à jour le système et installer les paquets de base nécessaires au téléchargement et à la décompression du binaire GoPhish.

```
sudo apt update && sudo apt upgrade -y
sudo apt install -y wget unzip curl ufw
```
Créez ensuite un utilisateur système dédié. Faire tourner GoPhish sous root est une mauvaise pratique courante : en cas de faille dans l’application (et GoPhish en a connu plusieurs, voir la section pièges plus bas), un compromis limité à un utilisateur sans privilèges réduit fortement l’impact.

```
sudo useradd -r -m -d /opt/gophish -s /usr/sbin/nologin gophish
sudo mkdir -p /opt/gophish
sudo chown gophish:gophish /opt/gophish
```
## Étape 2 : télécharger et installer GoPhish 0.12.1

Rendez-vous sur la page des releases GitHub de GoPhish pour vérifier le nom exact de l’archive correspondant à votre architecture, puis téléchargez-la sur le serveur.

```
cd /opt/gophish
sudo -u gophish wget https://github.com/gophish/gophish/releases/download/v0.12.1/gophish-v0.12.1-linux-64bit.zip
sudo -u gophish unzip gophish-v0.12.1-linux-64bit.zip
sudo -u gophish chmod +x gophish
```
L’archive contient le binaire gophish, un fichier config.json, un dossier static (assets de l’interface admin) et un dossier db contenant les migrations SQLite. Ne renommez pas ces dossiers : le binaire s’attend à les trouver au même niveau que lui.

## Étape 3 : comprendre et adapter config.json

Le fichier config.json définit deux serveurs distincts : le serveur d’administration (interface web de pilotage) et le serveur de phishing (celui qui sert les pages d’atterrissage aux cibles). Par défaut, l’admin écoute sur le port 3333 en HTTPS, et le serveur de phishing sur le port 80 en HTTP.

```
{
  "admin_server": {
    "listen_url": "127.0.0.1:3333",
    "use_tls": true,
    "cert_path": "gophish_admin.crt",
    "key_path": "gophish_admin.key"
  },
  "phish_server": {
    "listen_url": "0.0.0.0:80",
    "use_tls": false,
    "cert_path": "",
    "key_path": ""
  },
  "db_name": "sqlite3",
  "db_path": "gophish.db",
  "migrations_prefix": "db/db_",
  "contact_address": "",
  "logging": {
    "filename": "",
    "level": ""
  }
}
```
Un point de sécurité important : limitez admin_server.listen_url à 127.0.0.1:3333 plutôt qu’à 0.0.0.0:3333. L’interface d’administration ne doit jamais être exposée directement sur Internet ; passez par un tunnel SSH ou un VPN pour vous y connecter. Seul le serveur de phishing, qui sert les pages piégées aux cibles, a besoin d’être accessible publiquement.

## Étape 4 : premier lancement et récupération du mot de passe admin

Lancez GoPhish une première fois en mode manuel pour vérifier que tout démarre correctement avant de créer le service systemd.

```
cd /opt/gophish
sudo -u gophish ./gophish
```
À la première exécution, la console affiche le mot de passe généré automatiquement pour le compte admin. Copiez-le immédiatement : il ne sera plus jamais réaffiché en clair dans les logs. Si vous le perdez, il faudra réinitialiser le mot de passe via la base SQLite ou recréer la base.

## Étape 5 : créer le service systemd

Pour que GoPhish démarre automatiquement au boot et redémarre en cas de crash, créez une unité systemd dédiée.

```
sudo tee /etc/systemd/system/gophish.service > /dev/null << 'EOF'
[Unit]
Description=Gophish - plateforme de simulation de phishing
After=network.target
[Service]
Type=simple
User=gophish
Group=gophish
WorkingDirectory=/opt/gophish
ExecStart=/opt/gophish/gophish
Restart=on-failure
RestartSec=5
[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload
sudo systemctl enable --now gophish
sudo systemctl status gophish
```
Un piège fréquent ici : si WorkingDirectory ne pointe pas exactement vers le dossier contenant config.json et gophish.db, le service échoue au démarrage avec une erreur de type « unable to open database file ». Vérifiez toujours ce chemin en premier en cas d'échec.

## Étape 6 : configurer le pare-feu

N'ouvrez que ce qui est strictement nécessaire. Le port 80 doit être accessible depuis Internet pour que les cibles atteignent la page piégée ; le port 3333 ne doit être accessible que depuis votre poste ou via VPN.

```
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow from VOTRE_IP_ADMIN to any port 3333
sudo ufw enable
```
Si vous préférez ne pas exposer le port 3333 du tout, une tunnellisation SSH est plus sûre : ssh -L 3333:127.0.0.1:3333 utilisateur@serveur, puis ouvrez https://127.0.0.1:3333 dans votre navigateur local.

## Étape 7 : configurer le profil d'envoi SMTP

Connectez-vous à l'interface d'administration, puis rendez-vous dans le menu Sending Profiles pour créer un nouveau profil. C'est ce profil qui définit comment GoPhish enverra les emails de la campagne.

| Champ | Exemple de valeur | Remarque | 
|---|---|---|
| Name | SMTP interne test | Nom interne, non visible par les cibles | 
| From | Support IT <[email protected]> | Utilisez un domaine dédié, jamais le domaine réel de l'entreprise | 
| Host | smtp.simulation-interne.fr:587 | Relais SMTP autorisé, avec SPF/DKIM configurés | 
| Username / Password | Identifiants du compte SMTP | À stocker dans un coffre-fort de secrets, pas en clair | 
| Use TLS | Activé | Obligatoire si le relais impose STARTTLS | 

Utilisez systématiquement le bouton Send Test Email avant de continuer. C'est le moyen le plus rapide de détecter un problème d'authentification SMTP ou un blocage par un filtre antispam avant de le découvrir en pleine campagne.

## Étape 8 : créer le modèle d'email

Dans le menu Email Templates, créez un nouveau modèle. GoPhish permet d'utiliser des variables qui seront remplacées automatiquement par les données de chaque cible : {{.FirstName}}, {{.LastName}}, {{.Email}}. Cette personnalisation augmente le réalisme du test, exactement comme le ferait un attaquant qui a récupéré un annuaire d'entreprise.

```
Objet : Action requise : mise à jour de votre mot de passe avant le {{.Date}}
Bonjour {{.FirstName}},
Notre politique de sécurité impose un renouvellement de mot de passe.
Merci de vous connecter avant expiration de votre compte :
{{.URL}}
Cordialement,
Le support informatique
```
La variable {{.URL}} est essentielle : c'est elle qui génère le lien de tracking unique par destinataire, celui qui permettra à GoPhish de savoir précisément qui a cliqué. Ne la remplacez jamais par une URL statique, sous peine de perdre toute la granularité des résultats.

## Étape 9 : créer la page d'atterrissage (landing page)

Dans Landing Pages, deux options s'offrent à vous : écrire le HTML à la main, ou utiliser la fonction Import Site qui clone automatiquement une page existante (par exemple votre propre portail d'authentification interne) pour maximiser le réalisme du test.

Activez l'option Capture Submitted Data si vous voulez enregistrer ce que les cibles saisissent dans le formulaire, et Capture Passwords si le formulaire contient un champ mot de passe. Soyez conscient que ces données deviennent immédiatement sensibles au sens du RGPD : elles doivent être stockées de façon sécurisée et supprimées après l'exploitation pédagogique de la campagne.

Ajoutez systématiquement une page de redirection post-soumission qui explique clairement à l'utilisateur qu'il vient de participer à un exercice de sensibilisation, avec des conseils pour repérer un vrai email de phishing la prochaine fois. C'est ce qui transforme un piège en moment de formation plutôt qu'en simple sanction déguisée.

## Étape 10 : importer la liste des cibles (Users & Groups)

Dans Users & Groups, créez un nouveau groupe et importez votre fichier CSV. Le format attendu est simple : une colonne Email obligatoire, et des colonnes First Name, Last Name, Position facultatives mais utiles pour la personnalisation.

```
First Name,Last Name,Email,Position
Camille,Girard,[email protected],Responsable Comptabilité
Karim,Benali,[email protected],Technicien Support
Léa,Fontaine,[email protected],Chargée RH
```
Ne dépassez pas la portée définie dans votre autorisation écrite. Si l'accord signé couvre le service comptabilité, n'importez pas l'ensemble de l'annuaire : chaque extension de périmètre doit être validée séparément avec la direction et le DPO.

## Étape 11 : lancer la campagne

Dans Campaigns, créez une nouvelle campagne en assemblant les briques précédentes : nom, modèle d'email, page d'atterrissage, URL du serveur de phishing, profil d'envoi SMTP, et groupe(s) de cibles. Vous pouvez soit lancer immédiatement, soit programmer une date et heure de démarrage.

| Paramètre | Valeur recommandée | 
|---|---|
| Name | Nom explicite incluant la date, ex. « Sensibilisation T3-2026-Compta » | 
| URL | Domaine dédié à la simulation, jamais un sous-domaine du site officiel | 
| Launch Date | Hors période de forte charge (éviter fin de mois pour la compta, par exemple) | 
| Send Emails By | Étalé sur plusieurs heures plutôt qu'un envoi groupé instantané, pour éviter de déclencher les filtres antispam | 

Vérifiez une dernière fois chaque élément avant de cliquer sur Launch Campaign : une fois lancée, une campagne ne peut pas être interrompue proprement sans laisser certains emails déjà partis en file d'attente SMTP.

## Étape 12 : lire et interpréter les résultats

La page de résultats de chaque campagne affiche, pour chaque destinataire, une chronologie d'événements. GoPhish distingue quatre statuts principaux, chacun révélant un niveau de vigilance différent chez l'employé testé.

| Statut | Déclencheur technique | Ce que cela révèle | 
|---|---|---|
| Email Sent | L'email a été remis au serveur SMTP | Aucune information sur le comportement de la cible | 
| Email Opened | Chargement du pixel de tracking intégré | La cible a ouvert l'email (dépend du client de messagerie) | 
| Clicked Link | Clic sur le lien de tracking unique | La cible a cliqué sans vérifier la légitimité du lien | 
| Submitted Data | Soumission du formulaire sur la landing page | La cible a saisi des identifiants ou des données sensibles | 
| Email Reported | Signalement via le bouton de report ou transfert à la boîte dédiée | La cible a identifié la tentative et suivi la bonne procédure | 

Exportez les résultats au format CSV pour construire vos indicateurs de sensibilisation : taux de clic, taux de soumission de données, et surtout taux de signalement, qui est l'indicateur le plus révélateur de la maturité sécurité d'une équipe. Un taux de signalement en hausse d'une campagne à l'autre est souvent plus significatif qu'un taux de clic en baisse.

## Étape 13 : sécuriser durablement l'installation

Une fois la première campagne terminée, prenez le temps de durcir l'installation avant tout usage récurrent.

- Placez un certificat TLS valide (Let's Encrypt) sur le serveur de phishing si vous voulez tester la réaction des employés face à un cadenas HTTPS trompeur
- Limitez l'accès à l'interface admin par IP ou VPN, jamais en accès public direct
- Changez le mot de passe admin par défaut immédiatement après le premier login
- Sauvegardez régulièrement le fichier gophish.db, qui contient l'historique complet des campagnes
- Purgez les données sensibles (identifiants saisis) une fois l'exploitation pédagogique terminée, conformément au principe de minimisation RGPD

## Les 5 pièges les plus fréquents

**1. Lancer une campagne sans autorisation écrite.** C'est la faute la plus grave. Même en interne, sans document signé par la direction et le DPO, vous vous exposez personnellement à des poursuites pour intrusion dans un système de traitement automatisé de données. Un simple accord verbal du manager d'équipe ne suffit pas : le document doit être signé par une personne habilitée à engager la responsabilité de l'organisation, et conservé aussi longtemps que la politique de sécurité de l'entreprise l'exige.

**2. Exposer l'interface admin sur Internet.** Le port 3333 ouvert en 0.0.0.0 est régulièrement scanné par des robots. Combiné à une vulnérabilité comme CVE-2025-70963 (voir section suivante), cela peut exposer la clé API de vos comptes administrateurs à n'importe quel script s'exécutant dans le navigateur. Un attaquant qui met la main sur cette clé API obtient un accès équivalent à celui d'un administrateur légitime, avec la possibilité de créer ses propres campagnes ou de récupérer l'historique complet des tests déjà menés dans l'organisation.

**3. Envoyer tous les emails d'un coup.** Un envoi massif instantané déclenche presque systématiquement les filtres antispam et les systèmes de détection d'anomalies de la messagerie de l'entreprise, ce qui fausse totalement les résultats de la campagne. Un pic soudain de connexions sortantes vers un même relais SMTP est aussi un signal classique que surveillent les solutions de détection d'anomalies réseau, ce qui peut déclencher une alerte interne inutile pendant votre propre exercice.

**4. Oublier de configurer SPF/DKIM sur le domaine de simulation.** Sans ces enregistrements DNS, les emails de test finiront en spam avant même d'atteindre la boîte de réception, rendant le test inutile. Pire, un domaine sans SPF/DKIM correctement configuré peut voir sa réputation dégradée auprès des grands fournisseurs de messagerie, ce qui compliquera l'envoi de futures campagnes légitimes depuis la même infrastructure.

**5. Réutiliser les résultats individuels à des fins disciplinaires.** Sanctionner nommément un employé qui a cliqué détruit la confiance nécessaire à toute démarche de sensibilisation future et pose un problème direct de conformité RGPD sur la finalité du traitement. Une fois qu'un salarié apprend qu'un test de phishing a servi de base à un avertissement, l'ensemble de l'équipe se met sur la défensive et les campagnes suivantes perdent toute valeur pédagogique, les employés cherchant alors à deviner les tests plutôt qu'à développer un réflexe de vigilance sincère.

## Vulnérabilités connues de GoPhish à surveiller

Comme tout logiciel, GoPhish a fait l'objet de plusieurs avis de sécurité qu'il faut connaître avant de le déployer en production.

| Référence | Type | Impact | Statut | 
|---|---|---|---|
| CVE-2026-39904 | Déni de service | Un utilisateur authentifié peut épuiser la mémoire du serveur via une pièce jointe Office piégée dans un modèle d'email | Aucun correctif officiel signalé à la date de l'avis ; limitez la taille des pièces jointes et l'accès à la création de modèles | 
| CVE-2025-70963 | Contrôle d'accès incorrect | La clé API de chaque utilisateur est exposée en clair dans le HTML/JS du tableau de bord admin, exploitable via un script malveillant dans le navigateur | Limitez strictement l'accès à l'interface admin, tournez les clés API régulièrement | 
| CVE-2020-24710 / 24708 / 24709 | Cross-Site Scripting stocké | Injection de script via des champs de modèle ou la fonction d'imitation | Corrigé dans les versions ultérieures à 0.10.1 | 
| CVE-2020-24713 | Expiration de session insuffisante | Le cookie de session restait utilisable après déconnexion | Corrigé | 

Ces avis renforcent un principe simple : ne jamais exposer l'interface d'administration de GoPhish sur Internet, restreindre les droits de création de modèles aux seuls administrateurs de confiance, et surveiller le dépôt GitHub pour toute mise à jour au-delà de la version 0.12.1. Ce dernier point mérite une vigilance particulière : contrairement à des outils commerciaux qui poussent des correctifs automatiques, GoPhish étant un projet communautaire, c'est à chaque administrateur de vérifier périodiquement l'existence d'une nouvelle version et de l'appliquer manuellement.

Une bonne pratique complémentaire consiste à isoler entièrement le serveur GoPhish dans un réseau dédié, séparé du reste de l'infrastructure de production, y compris du contrôleur de domaine ou des serveurs de fichiers. Ainsi, même dans le pire scénario où une des vulnérabilités listées ci-dessus serait exploitée par un tiers malveillant, l'impact resterait cantonné à l'outil de test lui-même plutôt que de se propager vers les systèmes critiques de l'entreprise.

## Dépannage : 8 problèmes courants et leurs solutions

**Le service systemd échoue au démarrage avec « unable to open database file ».** Vérifiez que WorkingDirectory dans l'unité systemd correspond exactement au dossier contenant gophish.db et config.json. Un chemin relatif mal résolu est la cause la plus fréquente.

**Impossible d'accéder à l'interface admin sur le port 3333.** Si vous avez limité listen_url à 127.0.0.1, ouvrez un tunnel SSH local avant de tenter la connexion depuis votre navigateur.

**Le navigateur affiche une alerte de certificat invalide.** Le certificat par défaut généré par GoPhish est auto-signé. Remplacez cert_path et key_path par un certificat Let's Encrypt valide, ou acceptez l'exception uniquement en environnement de test isolé.

**Le port 80 est déjà utilisé par un autre service (Apache, Nginx).** Modifiez phish_server.listen_url vers un autre port, par exemple 0.0.0.0:8080, et redirigez le trafic via un reverse proxy, ou arrêtez le service en conflit.

**Les emails de test n'arrivent jamais.** Vérifiez les enregistrements SPF, DKIM et DMARC du domaine d'expédition, testez l'envoi via Send Test Email dans le profil SMTP, et consultez les logs du relais pour un rejet ou une mise en quarantaine.

**Le taux de clic affiché semble anormalement élevé.** Certains scanners antivirus d'entreprise ou passerelles de sécurité email cliquent automatiquement sur les liens pour les analyser (URL rewriting). Filtrez ces IP connues dans vos statistiques finales pour ne pas fausser l'analyse.

**L'import CSV des cibles échoue silencieusement.** Vérifiez l'encodage du fichier (UTF-8 sans BOM) et l'orthographe exacte des en-têtes de colonnes attendues par GoPhish (Email est le seul champ strictement obligatoire).

**La page d'atterrissage importée via « Import Site » affiche des ressources cassées.** L'import clone le HTML mais pas toujours les chemins relatifs vers les images et scripts. Passez les URL des ressources en absolu manuellement après l'import.

**Le mot de passe admin initial est perdu.** Arrêtez le service, sauvegardez gophish.db, puis réinitialisez le mot de passe via la ligne de commande officielle documentée dans le guide utilisateur, ou supprimez la base pour repartir d'un état vierge si aucune campagne n'a encore été lancée. Dans tous les cas, effectuez d'abord une copie du fichier de base de données : une manipulation malheureuse à ce stade peut effacer l'historique complet de vos campagnes précédentes, y compris les résultats déjà exploités pour vos rapports internes.

**Les statistiques de la campagne restent bloquées à zéro plusieurs heures après le lancement.** Vérifiez d'abord que le job d'envoi n'est pas resté en file d'attente côté relais SMTP en consultant ses propres logs. Si les emails sont bien partis mais qu'aucun clic n'est enregistré, assurez-vous que le lien de tracking généré par GoPhish pointe vers une adresse IP ou un nom de domaine réellement joignable depuis l'extérieur, et non vers une adresse interne au réseau du serveur.

## Astuces avancées pour des campagnes plus réalistes

Segmentez vos campagnes par métier plutôt que d'envoyer le même email à toute l'entreprise. Un email de fausse facture fournisseur sera plus efficace auprès de la comptabilité, tandis qu'une fausse alerte RH fonctionnera mieux auprès des équipes support. Cette segmentation reproduit fidèlement la manière dont un attaquant cible réellement une organisation après une phase de reconnaissance, où il adapte son scénario au métier de la victime plutôt que d'envoyer un message générique à toute l'entreprise.

Programmez des campagnes récurrentes espacées de plusieurs mois plutôt qu'un exercice unique annuel. La répétition régulière ancre mieux les réflexes que des sessions ponctuelles, même très élaborées. Un rythme trimestriel avec des scénarios différents à chaque fois (fausse facture, fausse alerte de sécurité, fausse notification de livraison) évite également l'effet d'usure où les employés reconnaissent le même modèle d'un test à l'autre et faussent artificiellement les statistiques de vigilance.

Couplez GoPhish à un webhook vers votre plateforme de tickets pour déclencher automatiquement un module de micro-formation dès qu'un utilisateur soumet des données sur la page piégée, plutôt que d'attendre un débriefing collectif en fin de campagne. L'API REST de GoPhish permet d'interroger l'état d'une campagne en continu, ce qui rend possible ce type d'automatisation sans intervention manuelle de l'équipe sécurité à chaque événement.

Variez les vecteurs : email classique, lien raccourci, QR code imprimé sur une fausse note de service. Les attaquants de 2026 ne se limitent plus au seul email, et une campagne de sensibilisation complète devrait refléter cette diversification, même si GoPhish reste centré sur le vecteur email et landing page.

Documentez chaque campagne dans un registre interne : date, scénario utilisé, périmètre, taux de clic, taux de soumission, taux de signalement. Ce registre devient un historique précieux pour justifier des investissements en formation auprès de la direction, et il constitue également une preuve de diligence en cas de contrôle ou d'incident réel de phishing qui toucherait l'entreprise entre deux campagnes.

Enfin, formez un petit groupe de « champions sécurité » dans chaque service, chargés de relayer les bonnes pratiques et de répondre aux questions de leurs collègues après une campagne. Cette approche par les pairs a généralement plus d'impact qu'une communication descendante unique venant de la DSI, car elle normalise la vigilance comme un comportement d'équipe plutôt qu'une contrainte imposée d'en haut.

## Mesurer le retour sur investissement d'un programme de sensibilisation

Justifier le temps passé à administrer GoPhish auprès de la direction demande de traduire les résultats bruts en indicateurs compréhensibles pour un comité de direction qui ne lit pas de logs. Trois métriques suffisent généralement à construire ce dossier.

La première est l'évolution du taux de clic sur plusieurs campagnes consécutives. Une baisse progressive, même modeste, démontre que l'effort de sensibilisation porte ses fruits et justifie la poursuite du programme plutôt que son abandon au bout d'un ou deux exercices.

La deuxième est le taux de signalement, souvent plus parlant que le taux de clic lui-même. Un employé qui clique par réflexe mais signale ensuite l'email suspect a compris le message de fond, même s'il n'a pas eu le bon réflexe au premier contact. Suivre ce taux dans le temps permet de mesurer la maturité réelle de l'organisation plutôt que sa seule capacité à éviter un piège ponctuel.

La troisième est le délai moyen entre l'envoi de l'email et le premier signalement. Plus ce délai se raccourcit d'une campagne à l'autre, plus cela indique que les employés développent un réflexe de vérification rapide, ce qui réduit la fenêtre d'exposition en cas d'attaque réelle où chaque minute compte pour l'équipe sécurité qui doit bloquer la propagation.

## Projet complet : script de déploiement automatisé

Voici un script bash qui regroupe l'ensemble des étapes d'installation vues plus haut, à adapter à votre environnement avant exécution.

```
#!/bin/bash
set -e
VERSION="v0.12.1"
ARCHIVE="gophish-${VERSION}-linux-64bit.zip"
# 1. Mise à jour système
apt update && apt upgrade -y
apt install -y wget unzip curl ufw
# 2. Création de l'utilisateur dédié
useradd -r -m -d /opt/gophish -s /usr/sbin/nologin gophish || true
mkdir -p /opt/gophish
chown gophish:gophish /opt/gophish
# 3. Téléchargement et installation
cd /opt/gophish
sudo -u gophish wget "https://github.com/gophish/gophish/releases/download/${VERSION}/${ARCHIVE}"
sudo -u gophish unzip -o "${ARCHIVE}"
sudo -u gophish chmod +x gophish
# 4. Service systemd
cat > /etc/systemd/system/gophish.service << 'EOF'
[Unit]
Description=Gophish - plateforme de simulation de phishing
After=network.target
[Service]
Type=simple
User=gophish
Group=gophish
WorkingDirectory=/opt/gophish
ExecStart=/opt/gophish/gophish
Restart=on-failure
RestartSec=5
[Install]
WantedBy=multi-user.target
EOF
systemctl daemon-reload
systemctl enable --now gophish
# 5. Pare-feu minimal
ufw allow 22/tcp
ufw allow 80/tcp
ufw --force enable
echo "Installation terminée. Consultez 'journalctl -u gophish' pour récupérer le mot de passe admin initial."
```
Exemple de sortie attendue après lancement du service, à récupérer via journalctl -u gophish -n 50 :

```
time="2026-09-07T09:12:03Z" level=info msg="Please login with the username admin and the password XjK9-mPz2-Qw7Y"
time="2026-09-07T09:12:03Z" level=info msg="Starting admin server at https://127.0.0.1:3333"
time="2026-09-07T09:12:03Z" level=info msg="Starting phishing server at http://0.0.0.0:80"
```
## Comparer GoPhish aux alternatives

GoPhish n'est pas le seul outil de simulation de phishing sur le marché. Voici comment il se positionne face aux principales alternatives, open source comme commerciales.

| Outil | Licence | Interface | Cas d'usage typique | 
|---|---|---|---|
| GoPhish | Open source (MIT) | Web légère, API REST | Équipes techniques internes, budget limité | 
| KnowBe4 | Commercial SaaS | Web complète avec reporting avancé | Grandes organisations avec budget dédié à la sensibilisation | 
| Proofpoint Security Awareness | Commercial SaaS | Intégrée à la suite email Proofpoint | Entreprises déjà clientes Proofpoint pour la sécurité email | 
| King Phisher | Open source (BSD) | Application desktop | Pentesteurs cherchant des scénarios plus complexes | 

Pour une équipe technique en interne qui veut garder le contrôle total sur ses données et son infrastructure sans payer de licence, GoPhish reste le choix le plus rationnel. Les solutions commerciales apportent en échange des bibliothèques de modèles prêts à l'emploi, un reporting automatisé plus poussé, et un support client, ce qui compense leur coût pour les organisations qui manquent de temps interne à consacrer à la maintenance de l'outil.

## Foire aux questions

### GoPhish est-il légal à utiliser en France ?

Oui, à condition de disposer d'une autorisation écrite de la direction et du DPO, de limiter la campagne au périmètre autorisé, et d'informer les employés en amont qu'une démarche de sensibilisation incluant potentiellement des simulations peut avoir lieu, conformément aux principes du RGPD.

### Faut-il un serveur dédié pour faire tourner GoPhish ?

Non, une petite VM avec 1 vCPU et 1 Go de RAM suffit pour la plupart des campagnes de taille PME. GoPhish, écrit en Go, a une empreinte mémoire très réduite comparée à des solutions Java ou Python équivalentes.

### Peut-on utiliser GoPhish avec Gmail ou Outlook comme relais SMTP ?

Techniquement oui via un mot de passe d'application, mais ce n'est pas recommandé pour une campagne réelle : les fournisseurs grand public limitent le volume d'envoi et suspendent rapidement les comptes qui envoient des emails ressemblant à du phishing. Utilisez un relais SMTP dédié à la simulation.

### Les résultats de GoPhish sont-ils fiables à 100 % ?

Non. Le tracking d'ouverture dépend du chargement d'une image, que beaucoup de clients de messagerie bloquent par défaut, ce qui sous-estime souvent le taux d'ouverture réel. Le taux de clic et de soumission de données reste en revanche fiable, car il repose sur une action explicite de l'utilisateur.

### Que faire des identifiants captés pendant une campagne ?

Ne les stockez jamais en clair au-delà de la durée nécessaire à l'analyse pédagogique. Supprimez-les de la base GoPhish une fois le débriefing effectué, et ne les communiquez jamais individuellement en dehors d'un cadre confidentiel encadré par le RSSI et le DPO.

### GoPhish peut-il remplacer une formation complète à la cybersécurité ?

Non, c'est un complément, pas un substitut. Une simulation révèle un niveau de vigilance à un instant T, mais ne transmet pas les connaissances nécessaires pour progresser. Associez toujours vos campagnes à un module de formation et à un débriefing collectif.

### Existe-t-il une version cloud hébergée de GoPhish ?

Non, GoPhish est distribué uniquement en auto-hébergement via son binaire téléchargeable sur GitHub. Il n'existe pas d'offre SaaS officielle maintenue par le projet lui-même.

### Comment savoir si mon organisation a besoin de ce type de test ?

Si votre entreprise a déjà subi une compromission par email, gère des données sensibles, ou entre dans le périmètre d'obligations réglementaires comme la directive NIS2, une campagne de simulation régulière fait partie des mesures de base recommandées par l'ANSSI pour réduire le risque d'ingénierie sociale.
