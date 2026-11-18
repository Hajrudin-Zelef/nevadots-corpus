# Guide GLPI ultra-complet — Ticketing, gestion de parc et pilotage SAV

> **Version du guide :** 2026 — GLPI 10.x (10.0.x LTS)
> **Public :** chef de service systèmes & énergies, techniciens SAV, administrateurs
> **Objectif :** installer, configurer, sécuriser, exploiter et maintenir GLPI comme cœur du
> ticketing et de la gestion de parc d'une entreprise de maintenance (focus : parc copieurs,
> consommables, contrats, planning techniciens).
> **Langue du corpus :** français (pensé pour un RAG personnel ; captures et libellés GLPI 10.x FR).

---

## Sommaire

1. [À quoi sert GLPI (et pourquoi c'est le cœur de votre organisation)](#1-à-quoi-sert-glpi-et-pourquoi-cest-le-cœur-de-votre-organisation)
2. [Concepts fondamentaux : ITSM, CMDB, entités, profils](#2-concepts-fondamentaux--itsm-cmdb-entités-profils)
3. [Architecture technique de GLPI 10.x](#3-architecture-technique-de-glpi-10x)
4. [Prérequis serveur détaillés (Debian/Ubuntu)](#4-prérequis-serveur-détaillés-debianubuntu)
5. [Installation pas à pas — pile LAMP (Apache + PHP 8 + MariaDB)](#5-installation-pas-à-pas--pile-lamp-apache--php-8--mariadb)
6. [Installation pas à pas — variante nginx + PHP-FPM](#6-installation-pas-à-pas--variante-nginx--php-fpm)
7. [Assistant d'installation web : écran par écran](#7-assistant-dinstallation-web--écran-par-écran)
8. [Configuration post-installation : vérifications critiques](#8-configuration-post-installation--vérifications-critiques)
9. [Tâches planifiées (cron) : le cœur battant de GLPI](#9-tâches-planifiées-cron--le-cœur-battant-de-glpi)
10. [Entités : l'arborescence multi-sites](#10-entités--larborescence-multi-sites)
11. [Profils et habilitations : qui fait quoi](#11-profils-et-habilitations--qui-fait-quoi)
12. [Utilisateurs : création, import LDAP/AD, règles](#12-utilisateurs--création-import-ldapad-règles)
13. [Groupes : équipes, escalades, astreintes](#13-groupes--équipes-escalades-astreintes)
14. [Notifications par courriel : configuration complète](#14-notifications-par-courriel--configuration-complète)
15. [Le ticket : cycle de vie complet](#15-le-ticket--cycle-de-vie-complet)
16. [Catégories de tickets : la taxonomie du SAV](#16-catégories-de-tickets--la-taxonomie-du-sav)
17. [Urgence, impact, priorité : la matrice](#17-urgence-impact-priorité--la-matrice)
18. [Gabarits de tickets et champs obligatoires](#18-gabarits-de-tickets-et-champs-obligatoires)
19. [Tickets récurrents : la maintenance planifiée](#19-tickets-récurrents--la-maintenance-planifiée)
20. [Actions automatiques et règles métier sur tickets](#20-actions-automatiques-et-règles-métier-sur-tickets)
21. [SLA : niveaux de service, escalades, pénalités](#21-sla--niveaux-de-service-escalades-pénalités)
22. [Suivis, tâches, solutions : bien documenter](#22-suivis-tâches-solutions--bien-documenter)
23. [Sondages de satisfaction](#23-sondages-de-satisfaction)
24. [Base de connaissances : rédiger des articles utiles](#24-base-de-connaissances--rédiger-des-articles-utiles)
25. [Le parc : vision d'ensemble (CMDB)](#25-le-parc--vision-densemble-cmdb)
26. [Ordinateurs : fiches, composants, liaisons](#26-ordinateurs--fiches-composants-liaisons)
27. [Moniteurs, imprimantes et périphériques](#27-moniteurs-imprimantes-et-périphériques)
28. [Fiches copieurs : le métier (marque, modèle, série, compteur, contrat)](#28-fiches-copieurs--le-métier-marque-modèle-série-compteur-contrat)
29. [Périphériques réseau : switchs, routeurs, bornes Wi-Fi, onduleurs](#29-périphériques-réseau--switchs-routeurs-bornes-wi-fi-onduleurs)
30. [Cartouches et consommables : gérer le stock de toner](#30-cartouches-et-consommables--gérer-le-stock-de-toner)
31. [Logiciels, licences et versions](#31-logiciels-licences-et-versions)
32. [Réservations de matériel](#32-réservations-de-matériel)
33. [FusionInventory : présentation et architecture](#33-fusioninventory--présentation-et-architecture)
34. [Installation de l'agent FusionInventory](#34-installation-de-lagent-fusioninventory)
35. [Inventaire automatique : réseau et SNMP](#35-inventaire-automatique--réseau-et-snmp)
36. [Règles d'import et de déduplication](#36-règles-dimport-et-de-déduplication)
37. [Gestion des contrats et des fournisseurs](#37-gestion-des-contrats-et-des-fournisseurs)
38. [Budgets et lignes budgétaires](#38-budgets-et-lignes-budgétaires)
39. [Projets et tâches : piloter les chantiers](#39-projets-et-tâches--piloter-les-chantiers)
40. [Rapports et statistiques : le tableau de bord du chef de service](#40-rapports-et-statistiques--le-tableau-de-bord-du-chef-de-service)
41. [Recherche, filtres et vues enregistrées](#41-recherche-filtres-et-vues-enregistrées)
42. [Sécurité — comptes et mots de passe](#42-sécurité--comptes-et-mots-de-passe)
43. [Sécurité — durcissement des profils](#43-sécurité--durcissement-des-profils)
44. [Sécurité — HTTPS/TLS de bout en bout](#44-sécurité--httpstls-de-bout-en-bout)
45. [Sécurité — durcissement serveur et GLPI](#45-sécurité--durcissement-serveur-et-glpi)
46. [Supervision de GLPI : ce qu'il faut surveiller](#46-supervision-de-glpi--ce-quil-faut-surveiller)
47. [Sauvegarde : stratégie complète (fichiers + base)](#47-sauvegarde--stratégie-complète-fichiers--base)
48. [Sauvegarde automatisée : scripts prêts à l'emploi](#48-sauvegarde-automatisée--scripts-prêts-à-lemploi)
49. [Restauration : procédure testée pas à pas](#49-restauration--procédure-testée-pas-à-pas)
50. [Mise à jour de GLPI : méthode sans casse](#50-mise-à-jour-de-glpi--méthode-sans-casse)
51. [Plugins : lesquels installer, lesquels éviter](#51-plugins--lesquels-installer-lesquels-éviter)
52. [API REST : authentification et premiers appels](#52-api-rest--authentification-et-premiers-appels)
53. [API REST : exemples concrets (tickets, parc, compteurs)](#53-api-rest--exemples-concrets-tickets-parc-compteurs)
54. [Dépannage — page blanche / erreur 500](#54-dépannage--page-blanche--erreur-500)
55. [Dépannage — le cron ne tourne pas](#55-dépannage--le-cron-ne-tourne-pas)
56. [Dépannage — les courriels ne partent pas](#56-dépannage--les-courriels-ne-partent-pas)
57. [Dépannage — 7 autres cas concrets](#57-dépannage--7-autres-cas-concrets)
58. [10 erreurs classiques (et comment les éviter)](#58-10-erreurs-classiques-et-comment-les-éviter)
59. [Cas pratique 1 — organiser le SAV d'une entreprise de maintenance copieurs](#59-cas-pratique-1--organiser-le-sav-dune-entreprise-de-maintenance-copieurs)
60. [Cas pratique 2 — tickets d'intervention copieurs de A à Z](#60-cas-pratique-2--tickets-dintervention-copieurs-de-a-à-z)
61. [Cas pratique 3 — planning des techniciens](#61-cas-pratique-3--planning-des-techniciens)
62. [Cas pratique 4 — stock pièces et consommables](#62-cas-pratique-4--stock-pièces-et-consommables)
63. [Cas pratique 5 — migration depuis Excel/ancien outil](#63-cas-pratique-5--migration-depuis-excelancien-outil)
64. [Cas pratique 6 — revue hebdomadaire du chef de service](#64-cas-pratique-6--revue-hebdomadaire-du-chef-de-service)
65. [Cas pratique 7 — intégrer les guides existants dans la base de connaissances](#65-cas-pratique-7--intégrer-les-guides-existants-dans-la-base-de-connaissances)
66. [Pense-bête de poche (une page)](#66-pense-bête-de-poche-une-page)
67. [Glossaire](#67-glossaire)
68. [Quiz — 10 questions + réponses](#68-quiz--10-questions--réponses)
69. [Pour aller plus loin](#69-pour-aller-plus-loin)
70. [Checklist de mise en production](#70-checklist-de-mise-en-production)
71. [Annexe A — paramètres PHP recommandés](#71-annexe-a--paramètres-php-recommandés)
72. [Annexe B — configuration Apache de référence](#72-annexe-b--configuration-apache-de-référence)
73. [Annexe C — configuration nginx de référence](#73-annexe-c--configuration-nginx-de-référence)
74. [Annexe D — modèle de gabarit « Intervention copieur »](#74-annexe-d--modèle-de-gabarit--intervention-copieur-)
75. [Annexe E — modèle de rapport hebdomadaire SAV](#75-annexe-e--modèle-de-rapport-hebdomadaire-sav)

---

# PARTIE I — CONCEPTS ET ARCHITECTURE

## 1. À quoi sert GLPI (et pourquoi c'est le cœur de votre organisation)

GLPI (Gestionnaire Libre de Parc Informatique) est une application web open source
(licence GPL) qui remplit **deux fonctions majeures** :

1. **Helpdesk / ITSM** : réception, qualification, assignation, suivi et clôture des
   tickets d'incidents et de demandes. C'est le guichet unique entre les utilisateurs
   (ou les clients, dans le cas d'une société de maintenance) et les techniciens.
2. **Gestion de parc / CMDB** : inventaire exhaustif du matériel (ordinateurs, imprimantes,
   copieurs, équipements réseau, onduleurs...), des logiciels, des licences, des contrats,
   des consommables et des budgets.

Pour un **chef de service systèmes & énergies** qui dirige aussi la maintenance des
copieurs, GLPI devient :

- le **registre unique** de chaque copieur client : marque, modèle, numéro de série,
  compteur, contrat de maintenance, historique des interventions ;
- le **canal officiel** des demandes d'intervention : fini les appels « au technicien
  que je connais » qui ne laissent aucune trace ;
- le **tableau de bord** du service : tickets ouverts par technicien, respect des SLA,
  taux de résolution au premier passage, consommation de toner par client ;
- la **mémoire** du service : base de connaissances alimentée par les retours terrain
  (codes erreur copieurs, procédures de maintenance préventive).

> **Règle d'or :** si ce n'est pas dans GLPI, ça n'existe pas. Chaque intervention,
> chaque compteur relevé, chaque cartouche posée doit laisser une trace.

**Ce que GLPI n'est pas :** ce n'est pas un outil de supervision temps réel
(remplacé par Zabbix, Nagios...), ni un ERP comptable, ni un GMAO pure
(même s'il s'en rapproche beaucoup avec les tickets récurrents et les projets).

---

## 2. Concepts fondamentaux : ITSM, CMDB, entités, profils

### ITSM (IT Service Management)

L'ITSM désigne l'ensemble des bonnes pratiques de gestion des services informatiques,
structurées notamment par le référentiel **ITIL**. GLPI implémente les processus ITIL
essentiels :

| Processus ITIL | Équivalent GLPI |
|---|---|
| Gestion des incidents | Tickets (statut, urgence, impact, priorité) |
| Gestion des demandes | Tickets de type « Demande » |
| Gestion des problèmes | Tickets liés, base de connaissances |
| Gestion des changements | Projets + tickets (plugin Change possible) |
| Gestion de la configuration (CMDB) | Parc : matériels, logiciels, liaisons |
| Gestion des niveaux de service | SLA (temps de prise en compte / résolution) |
| Gestion des connaissances | Base de connaissances |

### CMDB (Configuration Management Database)

La CMDB de GLPI, c'est l'onglet **Parc** : chaque élément (ordinateur, imprimante,
copieur, switch...) est un **élément de configuration (CI)** avec ses caractéristiques,
ses liaisons (branché à, installé sur, géré par) et son historique. L'intérêt : quand un
copieur tombe en panne, le technicien voit d'un coup d'œil le modèle, le contrat, les
tickets passés et les consommables compatibles.

### Les trois piliers du modèle GLPI

1. **Entités** : découpage organisationnel (société → sites → services). Tout objet
   GLPI appartient à une entité (ou est « récursif » sur plusieurs).
2. **Profils** : ensembles d'habilitations (ce qu'un utilisateur a le droit de voir/faire).
3. **Utilisateurs** : rattachés à une ou plusieurs entités avec un ou plusieurs profils.

> **Analogie métier :** l'entité, c'est l'agence ou le site client ; le profil, c'est la
> fiche de poste (technicien, superviseur, simple déclarant) ; l'utilisateur, c'est la personne.

---

## 3. Architecture technique de GLPI 10.x

GLPI 10.x est une application **PHP 8** adossée à **MariaDB/MySQL**, servie par
**Apache** ou **nginx**. Schéma simplifié :

```
Navigateur ──HTTPS──▶ Apache/nginx ──▶ PHP-FPM / mod_php ──▶ GLPI (/var/www/glpi)
                                                        │
                                                        ▼
                                              MariaDB (base glpidb)
                                                        │
Agent FusionInventory ──HTTPS──▶ GLPI ◀── cron système ──┘
        (inventaire)              (tâches planifiées : mail, SLA, alertes)
```

### Répertoires importants (installation standard)

| Répertoire | Contenu |
|---|---|
| `/var/www/glpi` (ou `/var/www/html/glpi`) | Code applicatif |
| `/var/www/glpi/config` | `config_db.php` (accès BDD) — **à protéger** |
| `/var/www/glpi/files` | Documents joints, dumps, exports — **hors web si possible** |
| `/var/www/glpi/marketplace` | Plugins installés via le marketplace |
| `/var/www/glpi/plugins` | Plugins manuels (legacy) |
| `/var/lib/glpi` (recommandé) | Données (`files/`, `config/`) déplacées hors du DocumentRoot |

> **Bonne pratique GLPI 10 :** déplacer `config/` et `files/` hors de la racine web
> (variables `GLPI_CONFIG_DIR` et `GLPI_VAR_DIR`). Voir section 45.

### Composants et flux

- **Tâches planifiées (cron)** : envoi des notifications, calcul des SLA, purge,
  remontée des collecteurs mail, inventaire. Sans cron, GLPI est « aveugle ».
- **Collecteurs mail** : GLPI lit une boîte (IMAP) et transforme chaque courriel en ticket.
- **API REST** : `/apirest.php` — création de tickets et relevés de compteurs depuis
  des scripts externes.
- **Agent FusionInventory** : remonte l'inventaire (matériel, logiciels, SNMP réseau).

---

## 4. Prérequis serveur détaillés (Debian/Ubuntu)

**Cible :** Debian 12 (Bookworm) ou Ubuntu 22.04/24.04 LTS, GLPI **10.0.x**.

### Matériel minimal / recommandé

| Profil | vCPU | RAM | Disque |
|---|---|---|---|
| Test / < 50 tickets/mois | 2 | 4 Go | 40 Go |
| Production PME (5-15 techniciens) | 4 | 8 Go | 100 Go SSD |
| Production + inventaire SNMP large | 4-8 | 16 Go | 200 Go SSD |

### Extensions PHP obligatoires (GLPI 10.x)

```
mysqli, curl, gd, intl, ldap, apcu, xml, mbstring, zip, bz2,
openssl, json, session, simplexml, domxml (dom), fileinfo,
sodium (recommandé), exif (recommandé), zend-opcache (recommandé)
```

### Base de données

- **MariaDB ≥ 10.6** (recommandé) ou MySQL ≥ 8.0.
- Jeu de caractères : `utf8mb4`, collation `utf8mb4_unicode_ci`.
- Privilèges : un utilisateur dédié `glpi` avec droits complets **sur la seule base** `glpidb`.

### Réseau et système

- Nom DNS interne (ex. `glpi.entreprise.lan`) + certificat TLS (section 44).
- NTP synchronisé (les SLA et les horodatages en dépendent !).
- Accès SMTP sortant (relais interne ou fournisseur) pour les notifications.
- Accès IMAP si collecteur mail utilisé.
- Pare-feu : ouvrir 443 (et 80 pour la redirection) depuis le LAN ; restreindre
  l'administration (SSH) aux postes d'exploitation.

> **Avertissement de version :** GLPI 10.x exige **PHP 8.1 minimum**
> (8.2/8.3 supportés selon la sous-version). PHP 7.4 n'est **plus** supporté.
> Vérifiez toujours la matrice de compatibilité de la version exacte téléchargée.

---

## 5. Installation pas à pas — pile LAMP (Apache + PHP 8 + MariaDB)

Procédure complète sur **Debian 12**. Adaptez les noms de paquets pour Ubuntu
(`php8.2-*` / `php8.3-*` selon la version distribuée).

### Étape 1 — mise à jour du système

```bash
sudo apt update && sudo apt full-upgrade -y
sudo apt install -y curl wget unzip ca-certificates gnupg lsb-release
```

### Étape 2 — installation d'Apache et MariaDB

```bash
sudo apt install -y apache2 mariadb-server
sudo systemctl enable --now apache2 mariadb
sudo mysql_secure_installation   # définir le mot de passe root, réponses Y
```

### Étape 3 — installation de PHP 8 et des extensions

```bash
sudo apt install -y php php-mysql php-curl php-gd php-intl php-ldap \
  php-apcu php-xml php-mbstring php-zip php-bz2 php-sodium php-exif \
  php-opcache libapache2-mod-php
php -v        # vérifier : PHP 8.2 ou 8.3
php -m | grep -E 'mysqli|curl|gd|intl|ldap|apcu'
```

### Étape 4 — création de la base de données

```bash
sudo mysql -u root -p <<'EOF'
CREATE DATABASE glpidb CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'glpi'@'localhost' IDENTIFIED BY 'REMPLACER_PAR_UN_MOT_DE_PASSE_FORT';
GRANT ALL PRIVILEGES ON glpidb.* TO 'glpi'@'localhost';
FLUSH PRIVILEGES;
EOF
```

> **Ne jamais** utiliser l'utilisateur `root` de MariaDB dans GLPI.
> Générez le mot de passe avec `openssl rand -base64 24` et conservez-le dans
> un coffre (KeePass, Vault) — jamais en clair dans un ticket ou un courriel.

### Étape 5 — téléchargement et déploiement de GLPI

```bash
cd /tmp
GLPI_VERSION="10.0.18"   # adapter à la dernière 10.0.x disponible
wget "https://github.com/glpi-project/glpi/releases/download/${GLPI_VERSION}/glpi-${GLPI_VERSION}.tgz"
tar xzf "glpi-${GLPI_VERSION}.tgz"
sudo mv glpi /var/www/
sudo chown -R www-data:www-data /var/www/glpi
sudo find /var/www/glpi -type d -exec chmod 755 {} \;
sudo find /var/www/glpi -type f -exec chmod 644 {} \;
```

### Étape 6 — virtual host Apache

Créer `/etc/apache2/sites-available/glpi.conf` (contenu complet en annexe B),
puis :

```bash
sudo a2enmod rewrite headers ssl
sudo a2ensite glpi
sudo a2dissite 000-default
sudo apache2ctl configtest
sudo systemctl reload apache2
```

### Étape 7 — paramètres PHP (fichier `/etc/php/8.2/apache2/php.ini` ou `8.3`)

```ini
memory_limit = 256M
max_execution_time = 120
max_input_vars = 5000
upload_max_filesize = 32M
post_max_size = 40M
session.cookie_httponly = 1
date.timezone = Europe/Paris   ; adapter
opcache.enable = 1
```

Puis `sudo systemctl restart apache2`.

### Étape 8 — lancer l'assistant web

Ouvrez `https://glpi.entreprise.lan/` (ou `http://<ip>/glpi`) et suivez la
[section 7](#7-assistant-dinstallation-web--écran-par-écran).

---

## 6. Installation pas à pas — variante nginx + PHP-FPM

Si vous préférez nginx (performances, reverse proxy existant) :

```bash
sudo apt install -y nginx php-fpm php-mysql php-curl php-gd php-intl \
  php-ldap php-apcu php-xml php-mbstring php-zip php-bz2 mariadb-server
```

### Pool PHP-FPM dédié : `/etc/php/8.2/fpm/pool.d/glpi.conf`

```ini
[glpi]
user = www-data
group = www-data
listen = /run/php/glpi.sock
listen.owner = www-data
listen.group = www-data
pm = dynamic
pm.max_children = 20
pm.start_servers = 4
pm.min_spare_servers = 2
pm.max_spare_servers = 6
php_admin_value[memory_limit] = 256M
php_admin_value[upload_max_filesize] = 32M
php_admin_value[post_max_size] = 40M
```

### Serveur nginx : `/etc/nginx/sites-available/glpi` (extrait, complet en annexe C)

```nginx
server {
    listen 443 ssl;
    server_name glpi.entreprise.lan;
    root /var/www/glpi/public;

    ssl_certificate     /etc/ssl/certs/glpi.crt;
    ssl_certificate_key /etc/ssl/private/glpi.key;

    location / {
        try_files $uri /index.php$is_args$args;
    }
    location ~ ^/index\.php(/|$) {
        fastcgi_pass unix:/run/php/glpi.sock;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
    # Bloquer l'accès direct aux dossiers sensibles
    location ~ ^/(config|files|inc|install|tests)/ { deny all; }
}
```

> **Note GLPI 10 :** le DocumentRoot recommandé est `/var/www/glpi/public`
> (le routeur frontal `public/index.php`). Ne pointez plus la racine sur
> `/var/www/glpi` directement comme en 9.x.

```bash
sudo ln -s /etc/nginx/sites-available/glpi /etc/nginx/sites-enabled/
sudo nginx -t && sudo systemctl reload nginx
sudo systemctl restart php8.2-fpm
```

---

## 7. Assistant d'installation web : écran par écran

1. **Sélection de la langue** → Français.
2. **Licence** → accepter (GPL v3+).
3. **Installation / Mise à jour** → « Installation ».
4. **Vérification de l'environnement** : tous les voyants doivent être verts.
   - Orange « recommandé » (ex. `exif`, `sodium`) : acceptable mais à corriger.
   - Rouge « obligatoire » : bloquant — installez l'extension manquante puis rechargez.
5. **Connexion à la base** : serveur `localhost`, utilisateur `glpi`, mot de passe
   choisi à l'étape 4, base `glpidb` (l'assistant peut la créer si l'utilisateur a
   les droits ; ici elle existe déjà).
6. **Initialisation** : création des tables (quelques minutes).
7. **Collecte de données (télémétrie)** : répondez selon votre politique interne.
   En entreprise, on choisit généralement « Ne pas envoyer ».
8. **Comptes par défaut créés** — ⚠️ **à sécuriser immédiatement** (section 8) :
   - `glpi` / `glpi` (super-admin)
   - `tech` / `tech` (technicien)
   - `normal` / `normal` (post-only)
   - `post-only` / `postonly` (déclarant)

### Checklist immédiate après l'assistant

- [ ] Changer les 4 mots de passe par défaut (ou désactiver les comptes inutiles).
- [ ] Supprimer le dossier `/var/www/glpi/install` (ou le renommer `install.bak`).
- [ ] Vérifier `Administration > Journaux` : aucune erreur d'installation.
- [ ] Configurer le cron système (section 9) **avant** toute utilisation réelle.

---

## 8. Configuration post-installation : vérifications critiques

### 8.1 Supprimer / sécuriser l'installeur

```bash
sudo mv /var/www/glpi/install /var/www/glpi/install.bak_$(date +%F)
sudo chown -R www-data:www-data /var/www/glpi
```

### 8.2 Comptes par défaut

`Administration > Utilisateurs` : pour chaque compte par défaut,
définissez un mot de passe fort (20+ caractères) ou désactivez-le
(« Actif : Non »). Ne gardez actif que ce dont vous avez besoin.

### 8.3 Identité de l'application

`Configuration > Générale` :

| Paramètre | Valeur conseillée |
|---|---|
| Nom | `GLPI — SAV Entreprise` |
| URL de l'application | `https://glpi.entreprise.lan` (utilisée dans les courriels !) |
| Fuseau horaire | celui du site principal |
| Langue par défaut | Français |

> **Piège classique :** une URL d'application fausse = des liens morts dans tous
> les courriels de notification. À vérifier en premier quand « les liens ne marchent pas ».

### 8.4 Dossiers de données hors racine web (recommandé)

```bash
sudo mkdir -p /var/lib/glpi/{config,files}
sudo cp -a /var/www/glpi/config/* /var/lib/glpi/config/
sudo cp -a /var/www/glpi/files/* /var/lib/glpi/files/
sudo chown -R www-data:www-data /var/lib/glpi
```

Dans le virtual host Apache (ou `.htaccess`), définir :

```apache
SetEnv GLPI_CONFIG_DIR /var/lib/glpi/config
SetEnv GLPI_VAR_DIR /var/lib/glpi/files
```

(nginx : `fastcgi_param GLPI_CONFIG_DIR /var/lib/glpi/config;` etc.)

### 8.5 Test d'envoi de courriel

`Configuration > Notifications > Configuration des notifications` → renseigner le
SMTP, puis « Envoyer un courriel de test ». Si rien n'arrive : section 56.

---

## 9. Tâches planifiées (cron) : le cœur battant de GLPI

Sans cron, GLPI n'envoie **aucun** courriel, ne calcule **aucun** SLA, ne purge rien
et ne relève pas les boîtes mail. C'est la cause n°1 des « GLPI ne fait rien tout seul ».

### 9.1 Cron système (mode CLI, recommandé)

```bash
# /etc/cron.d/glpi — toutes les minutes, en www-data
* * * * * www-data /usr/bin/php /var/www/glpi/front/cron.php
```

Vérification :

```bash
sudo -u www-data php /var/www/glpi/front/cron.php
# sortie attendue : exécution silencieuse, code 0
```

### 9.2 Mode pseudo-cron (si pas d'accès au cron système)

`Configuration > Actions automatiques` → « Exécuter en mode CLI » : Non.
GLPI exécute alors les tâches lors des visites de pages. **Déconseillé en production**
(irrégulier, dépend du trafic).

### 9.3 Actions automatiques essentielles à vérifier

`Configuration > Actions automatiques` :

| Action | Fréquence conseillée | Rôle |
|---|---|---|
| `queuednotification` | 1 min | Envoi des courriels en file |
| `mailcollector` | 5 min | Relève IMAP → tickets |
| `sla` | 15 min | Calculs et escalades SLA |
| `purgeticket` | 1 jour | Clôture/purge selon règles |
| `alertnotclosed` | 1 jour | Alerte tickets non clos |
| `createinquest` | 1 heure | Envoi des sondages |
| `sendpasswordexpires` | 1 jour | Expiration mots de passe |

> Chacune a un **journal d'exécution** (dernière exécution, durée, erreurs).
> En dépannage, c'est le premier écran à ouvrir (section 55).

### 9.4 Supervision du cron (exemple Nagios/Zabbix)

```bash
#!/bin/bash
# check_glpi_cron.sh — alerte si le cron GLPI n'a pas tourné depuis 10 min
STAMP_FILE="/var/lib/glpi/files/_cron/last_run"
# alternative : interroger la BDD
AGE=$(mysql -N -s -e "SELECT TIMESTAMPDIFF(MINUTE, MAX(lastrun), NOW())
  FROM glpidb.glpi_crontasks;" 2>/dev/null)
[ "${AGE:-999}" -gt 10 ] && echo "CRITICAL: cron GLPI inactif depuis ${AGE} min" && exit 2
echo "OK: cron GLPI actif (dernière exécution il y a ${AGE} min)"
```

---

# PARTIE II — ORGANISATION : ENTITÉS, PROFILS, UTILISATEURS

## 10. Entités : l'arborescence multi-sites

L'entité est le découpage organisationnel. Tout objet GLPI (ticket, matériel,
contrat...) appartient à une entité. Les entités forment un **arbre**.

### Exemple pour une société de maintenance multi-sites

```
Entité racine : ACME Maintenance
├── Agence Nord
│   ├── Atelier
│   └── Clients rattachés (sous-entités par client important)
├── Agence Sud
└── Siège (services internes : informatique, comptabilité)
```

Ou, variante « par client » si chaque client est une entité :

```
ACME Maintenance
├── Client — Mairie de Villeurbanne
├── Client — Clinique Saint-Roch
└── Interne
```

### Bonnes pratiques

- **Ne multipliez pas les entités** : 5 à 15 suffisent pour une PME. Trop d'entités
  = administration cauchemardesque.
- Les objets « mutualisés » (contrats-cadres, modèles de documents) se mettent en
  **récursif** depuis l'entité racine pour être visibles partout.
- Un technicien itinérant reçoit son profil en **récursif** sur l'entité racine :
  il voit tous les sites.
- Les clients externes (portail déclarant) sont cantonnés à **leur** entité :
  ils ne voient que leurs tickets et leurs matériels.

### Création

`Administration > Entités` → `+` : nom, entité parente, adresse, responsable,
délégations (qui peut administrer cette entité). Pensez à renseigner l'adresse :
elle sert sur les documents et les notifications.

---

## 11. Profils et habilitations : qui fait quoi

Un **profil** = un jeu d'habilitations. GLPI 10 fournit des profils prédéfinis,
à dupliquer et ajuster (ne modifiez jamais les profils natifs directement :
dupliquez-les d'abord).

### Matrice des profils types pour un SAV

| Capacité | Post-only (déclarant) | Tech | Superviseur | Admin |
|---|---|---|---|---|
| Créer un ticket | ✅ | ✅ | ✅ | ✅ |
| Voir ses propres tickets | ✅ | ✅ | ✅ | ✅ |
| Voir tous les tickets de l'entité | ❌ | ✅ | ✅ | ✅ |
| S'assigner / assigner un ticket | ❌ | ✅ (soi) | ✅ | ✅ |
| Changer statut, priorité, SLA | ❌ | ✅ | ✅ | ✅ |
| Résoudre / clore | ❌ | ✅ | ✅ | ✅ |
| Gérer le parc (créer/modifier) | ❌ | lecture | ✅ | ✅ |
| Gérer contrats/budgets | ❌ | ❌ | ✅ | ✅ |
| Base de connaissances (publier) | ❌ | ✅ | ✅ | ✅ |
| Configuration / utilisateurs | ❌ | ❌ | partiel | ✅ |

### Profils à créer pour le métier

1. **Technicien SAV** (dupliqué de Tech) : + gestion des imprimantes/copieurs,
   + réservation de pièces, − suppression définitive.
2. **Superviseur / Chef de service** : tout le Tech + contrats, budgets, rapports,
   + réassignation inter-équipes, + validation des solutions.
3. **Déclarant client** (dupliqué de Post-only) : création de tickets avec gabarit
   « Intervention copieur », vision de ses matériels (ses copieurs !).
4. **Magasinier** : gestion du stock (cartouches, pièces détachées) sans accès tickets.

### Principe du moindre privilège

> Donnez à chacun le minimum nécessaire. Un technicien n'a pas besoin de voir les
> budgets ; un déclarant client ne doit jamais voir les tickets d'un autre client.
> Revoyez les profils **une fois par an** (départs, changements de poste).

---

## 12. Utilisateurs : création, import LDAP/AD, règles

### Création manuelle

`Administration > Utilisateurs` → `+` : identifiant, nom, courriel, entité(s) +
profil(s). Activez « Actif ». Le courriel est **indispensable** (notifications).

### Import depuis Active Directory / LDAP (recommandé)

`Configuration > Authentification > Annuaires LDAP` → `+` :

| Champ | Exemple |
|---|---|
| Nom | `AD-Entreprise` |
| Serveur | `ad.entreprise.lan` |
| Port | `389` (ou `636` LDAPS) |
| Base DN | `DC=entreprise,DC=lan` |
| DN de connexion | `CN=svc_glpi,OU=Services,DC=entreprise,DC=lan` |
| Filtre utilisateurs | `(&(objectClass=user)(!(userAccountControl:514)))` |
| Champ identifiant | `sAMAccountName` |
| Champ courriel | `mail` |

Puis `Administration > Utilisateurs` → « Import LDAP » : import de masse ou
**import à la première connexion** (l'utilisateur est créé automatiquement quand
il se connecte avec ses identifiants AD — zéro administration).

> **Sécurité :** préférez LDAPS (636) avec certificat valide. Le compte de
> connexion LDAP doit avoir le minimum de droits (lecture seule sur l'annuaire).

### Règles d'affectation automatique

`Administration > Règles > Règles d'affectation d'habilitations` : ex.
« si membre du groupe AD `SAV-Techniciens` → profil Technicien SAV récursif sur
l'entité racine ». L'arrivée d'un nouveau technicien = l'ajouter au groupe AD,
rien à faire dans GLPI.

---

## 13. Groupes : équipes, escalades, attrib
utions

Les **groupes** servent à assigner des tickets à une équipe plutôt qu'à une personne,
et à gérer les escalades.

### Groupes types d'un SAV

| Groupe | Rôle |
|---|---|
| `SAV — Hotline N1` | Qualification, diagnostic à distance |
| `SAV — Techniciens terrain` | Interventions sur site |
| `SAV — Atelier` | Réparations en atelier |
| `SAV — Superviseurs` | Escalade, arbitrage |
| `Informatique interne` | Parc interne de l'entreprise |
| `Astreinte` | Hors heures ouvrées |

`Assistance > Groupes` (ou `Administration > Groupes`) : créez le groupe,
ajoutez-y les utilisateurs, définissez s'il peut être **assigné** à un ticket
et/ou **notifié**.

### Bonnes pratiques

- Assignez d'abord au **groupe**, le superviseur redistribue aux techniciens :
  évite les tickets « orphelins » quand quelqu'un est absent.
- Un ticket non pris en compte sous X minutes (SLA) → **escalade automatique**
  vers le groupe `Superviseurs` (section 21).
- Le groupe `Astreinte` ne reçoit que les tickets d'urgence maximale hors horaires.

---

## 14. Notifications par courriel : configuration complète

### 14.1 Configuration SMTP

`Configuration > Notifications > Configuration des notifications` :

| Paramètre | Valeur type |
|---|---|
| Méthode d'envoi | SMTP |
| Hôte SMTP | `smtp.entreprise.lan` ou relais du FAI |
| Port | `587` (STARTTLS) ou `465` (SSL) |
| Chiffrement | STARTTLS |
| Authentification | Oui — compte dédié `glpi@entreprise.lan` |
| Expéditeur | `glpi@entreprise.lan` / nom « SAV — GLPI » |

> Utilisez un **compte dédié** à GLPI, jamais une boîte personnelle.
> Testez avec le bouton « Envoyer un courriel de test » (section 8.5).

### 14.2 Modèles de notification

`Configuration > Notifications > Modèles de notifications` : chaque événement
(nouveau ticket, nouveau suivi, résolution...) possède un modèle en français,
modifiable. Variables utiles :

```
##ticket.id##            → n° du ticket
##ticket.title##          → titre
##ticket.content##        → description
##ticket.url##            → lien direct (dépend de l'URL appli, section 8.3 !)
##ticket.status##         → statut
##user.name##             → destinataire
```

### 14.3 Qui reçoit quoi (matrice minimale)

| Événement | Déclarant | Technicien assigné | Groupe assigné | Superviseur |
|---|---|---|---|---|
| Nouveau ticket | ✅ | ✅ | ✅ | — |
| Nouveau suivi | ✅ | ✅ | — | — |
| Prise en compte | ✅ | — | — | — |
| Solution proposée | ✅ | — | — | — |
| Ticket résolu | ✅ | ✅ | — | — |
| Dépassement SLA (alerte) | — | ✅ | ✅ | ✅ |

### 14.4 File d'attente et collecteur

- Les courriels partent via la file `queuednotification` traitée par le cron
  (délai normal : < 2 minutes).
- Le **collecteur** (`Configuration > Collecteurs`) lit une boîte IMAP et crée
  des tickets depuis les courriels reçus — idéal pour une adresse `sav@entreprise.lan`.

---

# PARTIE III — TICKETS : LE CŒUR DU SAV

## 15. Le ticket : cycle de vie complet

### Les statuts (GLPI 10)

```
NOUVEAU ──▶ EN COURS (Attribué) ──▶ EN COURS (Planifié) ──▶ RÉSOLU ──▶ CLOS
   │               │                       │
   │               ▼                       ▼
   └─────▶ EN ATTENTE ◀────────────────────┘
```

| Statut | Signification | Qui l'utilise |
|---|---|---|
| **Nouveau** | Ticket créé, non qualifié | Déclarant, collecteur mail |
| **En cours (Attribué)** | Assigné à technicien/groupe | Superviseur, technicien |
| **En cours (Planifié)** | Intervention planifiée (date) | Technicien |
| **En attente** | Bloqué (pièce, client, fournisseur) | Technicien (+ motif obligatoire) |
| **Résolu** | Solution proposée, en attente validation | Technicien |
| **Clos** | Validé par le déclarant ou auto-clôture | Système / déclarant |

### Règles de gestion conseillées

1. **Tout ticket naît « Nouveau »** — la hotline le qualifie sous 30 min ouvrées.
2. **« En attente » exige un motif** (liste déroulante : attente pièce, attente
   client, attente fournisseur) + date de relance prévue.
3. **« Résolu » ≠ « Clos »** : le déclarant valide la solution (ou clôture
   automatique après 15 jours sans réponse — paramétrable).
4. Un ticket **ne retourne jamais** à « Nouveau » une fois traité (traçabilité).

### Champs clés d'un ticket

Titre, description, catégorie, urgence/impact/priorité, SLA, demandeur,
observateur(s), assigné à (technicien/groupe/fournisseur), éléments associés
(le copieur concerné !), statut, solution, coût, durée.

---

## 16. Catégories de tickets : la taxonomie du SAV

`Assistance > Catégories de tickets` : arborescence **hiérarchique**. Une bonne
taxonomie = des statistiques exploitables. Exemple pour la maintenance copieurs :

```
Panne matérielle
├── Bourrage papier
├── Qualité d'impression (traces, pâleur, taches)
├── Code erreur affiché
├── Bac / alimentation papier
└── Scanner / chargeur
Consommables
├── Toner à remplacer
├── Kit de maintenance (fusion, tambour)
└── Papier
Demande
├── Installation / déménagement
├── Formation utilisateur
├── Relevé de compteur
└── Devis / commande
Réseau & logiciels
├── Imprimante hors ligne
├── Pilote / file d'impression
└── Scan vers dossier / e-mail
Maintenance préventive
└── Visite planifiée
```

### Règles d'or

- **2 à 3 niveaux maximum** : au-delà, les techniciens choisissent « Autre ».
- Chaque catégorie peut avoir son **gabarit** (section 18) et son **groupe
  assigné par défaut** (règle métier, section 20).
- La catégorie est **obligatoire** à la création (gabarit) : pas de ticket
  « sans catégorie » = pas de statistique fausse.
- Revoyez la taxonomie **une fois par an** avec les techniciens.

---

## 17. Urgence, impact, priorité : la matrice

GLPI calcule la **priorité** à partir de l'**urgence** (ressenti du demandeur)
et de l'**impact** (étendue réelle), selon une matrice paramétrable
(`Configuration > Générale > Assistance`).

### Matrice par défaut (à adapter)

| Urgence \ Impact | 1-Mineur | 2-Moyen | 3-Majeur | 4-Critique |
|---|---|---|---|---|
| **1-Basse** | 1-Très basse | 1-Très basse | 2-Basse | 2-Basse |
| **2-Moyenne** | 2-Basse | 2-Basse | 3-Moyenne | 3-Moyenne |
| **3-Haute** | 3-Moyenne | 3-Moyenne | 4-Haute | 4-Haute |
| **4-Très haute** | 4-Haute | 4-Haute | 5-Très haute | 5-Très haute |

### Définitions métier (exemple SAV copieurs)

| Impact | Définition |
|---|---|
| 4-Critique | Tout un site client à l'arrêt (ex. unique copieur d'une mairie) |
| 3-Majeur | Service entier perturbé (ex. copieur du service comptabilité en clôture) |
| 2-Moyen | Quelques utilisateurs gênés |
| 1-Mineur | Gêne légère, contournement possible |

> **Conseil :** laissez le déclarant fixer l'**urgence**, mais faites valider
> l'**impact** par la hotline. La priorité affichée doit refléter la réalité
> opérationnelle, pas l'émotion du moment.

---

## 18. Gabarits de tickets et champs obligatoires

Les **gabarits** (`Configuration > Gabarits > Gabarits de tickets`) définissent,
**par catégorie**, quels champs sont visibles, obligatoires ou pré-remplis.
C'est l'outil n°1 de la qualité des tickets.

### Gabarit « Intervention copieur » (modèle complet en annexe D)

| Champ | Réglage | Pourquoi |
|---|---|---|
| Catégorie | Pré-remplie, verrouillée | Évite les erreurs |
| Éléments associés | **Obligatoire** | Le copieur concerné, toujours |
| Urgence | Visible, obligatoire | — |
| Type | Pré-rempli « Incident » | — |
| Compteur actuel | Champ personnalisé, obligatoire | Facturation / suivi |
| Code erreur affiché | Champ personnalisé, facultatif | Diagnostic |
| Description | Obligatoire, aide à la saisie | « Décrivez les symptômes... » |

### Champs personnalisés

`Configuration > Champs personnalisés` (plugin ou natif selon version) :
ajoutez « Compteur N&B », « Compteur couleur », « Code erreur », « N° de contrat ».
Ces champs deviennent interrogeables dans les rapports et l'API.

### Aides à la saisie

Chaque champ peut afficher un texte d'aide (« Ex. : bourrage bac 2, code C-0202 »).
Rédigez-les avec les mots **des utilisateurs**, pas le jargon technique.

---

## 19. Tickets récurrents : la maintenance planifiée

`Assistance > Tickets récurrents` : des tickets générés automatiquement selon un
calendrier — l'équivalent GMAO de GLPI. **Parfait pour la maintenance préventive
des copieurs** (le métier !).

### Exemples de tickets récurrents

| Ticket | Périodicité | Assigné à |
|---|---|---|
| Relevé compteurs copieurs — Client Mairie | Mensuel, le 1er | Technicien référent |
| Nettoyage + contrôle copieurs — Clinique | Trimestriel | Groupe Techniciens terrain |
| Remplacement kit de fusion (préventif) | Selon compteur (> 200 000 pages) | Atelier |
| Vérification onduleurs salle serveurs | Semestriel | Groupe Énergies |
| Test groupe électrogène | Mensuel | Groupe Énergies |

### Configuration

1. Créez un **modèle de ticket** (titre, catégorie « Maintenance préventive »,
   gabarit, éléments associés si fixes).
2. `Tickets récurrents` → `+` : choisissez le modèle, la périodicité
   (quotidien, hebdo, mensuel, annuel), la date de début, la **création anticipée**
   (ex. créer le ticket 7 jours avant pour préparer la tournée).
3. Le cron génère les tickets automatiquement.

> **Astuce chef de service :** la liste des tickets récurrents = votre
> **plan de maintenance préventive** officiel. Présentez-la en revue de direction.

---

## 20. Actions automatiques et règles métier sur tickets

### Règles d'assignation (`Administration > Règles > Règles métier`)

Exemples qui font gagner un temps fou :

| Condition | Action |
|---|---|
| Catégorie = « Bourrage papier » | Assigner au groupe `Hotline N1` |
| Catégorie commence par « Panne matérielle » + entité = Client X | Assigner au technicien référent du client |
| Urgence = Très haute | Assigner au groupe `Superviseurs` + priorité max |
| Demandeur = direction@client.lan | Ajouter observateur = chef de service |
| Titre contient « onduleur » | Catégorie = Énergies / Onduleurs |

### Actions automatiques (déjà vues section 9)

Complétez avec des règles de **clôture automatique** : ticket « Résolu » sans
réponse du déclarant depuis 15 jours → « Clos » (avec notification).

> **Prudence :** testez chaque règle sur une entité de test avant production.
> Une règle mal écrite peut réassigner des centaines de tickets en boucle.

---

## 21. SLA : niveaux de service, escalades, pénalités

### Concepts

- **SLA (Service Level Agreement)** : engagement contractuel (ex. prise en compte
  sous 4 h ouvrées, résolution sous 2 jours ouvrés).
- **TTO (Time To Own)** : délai de **prise en compte** (statut → En cours).
- **TTR (Time To Resolve)** : délai de **résolution** (statut → Résolu).
- **Calendrier** : les délais se calculent en **heures ouvrées** (calendrier
  8h-18h lun-ven, hors jours fériés).

### Configuration pas à pas

1. `Configuration > Calendriers` : créez « Heures ouvrées SAV » (lun–ven 8h–18h),
   ajoutez les jours fériés (plages d'exclusion).
2. `Assistance > SLA` → `+` : nom (« SLA Standard Copieurs »), TTO = 4 h,
   TTR = 48 h, calendrier associé.
3. `Assistance > Niveaux de SLA` : définissez les **niveaux d'escalade** :
   - Niveau 1 : à 50 % du TTO → rappel au technicien ;
   - Niveau 2 : TTO dépassé → escalade au superviseur + notification ;
   - Niveau 3 : TTR dépassé → alerte chef de service.
4. Affectez le SLA aux tickets via **règles métier** (ex. tout ticket de l'entité
   « Client Mairie » → SLA « Collectivités ») ou manuellement.

### Pénalités et pilotage

GLPI calcule automatiquement les **dépassements**. En revue hebdomadaire
(section 64), sortez : % de tickets dans les SLA par technicien, par client,
par catégorie. Si un contrat prévoit des pénalités financières, ces chiffres
sont votre **preuve contractuelle**.

> **Point de vigilance :** le statut « En attente » **suspend** le compteur SLA
> (selon paramétrage). Formez les techniciens : un ticket en attente sans motif
> valable = un SLA artificiellement « tenu » = statistiques faussées.

---

## 22. Suivis, tâches, solutions : bien documenter

### Suivis

Chaque intervention s'écrit en **suivi** (public = visible du déclarant ;
privé = interne équipe). Règles :

- Un suivi **à chaque étape** : diagnostic, action, résultat.
- Style factuel : « Remplacement du kit de fusion (réf. FK-8350). Compteur :
  212 458 pages. Test OK, 20 copies de contrôle. »
- Joignez la **photo** du compteur ou du défaut (pièce jointe du ticket).

### Tâches

Pour découper une intervention (planification, durée, technicien) :
`Tâches` dans le ticket → « Planifier » (date, durée prévue) → le technicien
la retrouve dans son **planning** (`Assistance > Planning`).

### Solutions

À la résolution : choisissez un **type de solution** (liste : « Remplacement
pièce », « Nettoyage », « Paramétrage », « Formation », « Non reproduit »...),
décrivez la solution, liez éventuellement un **article de la base de
connaissances**. Le déclarant peut **approuver ou refuser** (avec motif).

> **Capitalisation :** une solution bien rédigée + article KB lié = la prochaine
> panne identique se résout en 10 minutes par un N1 au lieu d'un déplacement.

---

## 23. Sondages de satisfaction

`Assistance > Sondages` : questionnaire envoyé automatiquement à la résolution
(paramétrable : délai, cible). Questions types :

1. Note globale de l'intervention (1–5).
2. Délai d'intervention satisfaisant ? (Oui/Non)
3. Technicien courtois et compétent ? (1–5)
4. Commentaire libre.

**Exploitation :** taux de réponse, note moyenne par technicien/client.
Une note < 3 déclenche une **alerte au superviseur** (notification sur événement
« sondage négatif »). Présentez la synthèse en revue mensuelle : c'est l'indicateur
« qualité perçue » qui complète les SLA.

---

## 24. Base de connaissances : rédiger des articles utiles

`Assistance > Base de connaissances` : le wiki interne du SAV. **C'est ici que
les guides existants de l'entreprise prennent vie** (voir cas pratique 7,
section 65).

### Structure d'un bon article

```
Titre : [Marque] [Modèle] — Symptôme / Procédure
Ex. : « Kyocera TASKalfa 4054ci — Bourrage récurrent bac 2 (code J-0511) »

1. Symptômes
2. Causes probables (par ordre de fréquence)
3. Diagnostic pas à pas
4. Résolution / procédure
5. Pièces et références (toner, kits)
6. Prévention
7. Liens (manuel constructeur, article lié)
```

### Règles éditoriales

- **Un article = un problème = une solution.** Pas de pavé fourre-tout.
- Catégories miroir des catégories de tickets (section 16) : l'article se
  retrouve facilement depuis le ticket.
- **Cible** : rédigez pour le technicien N1 qui ne connaît pas la machine.
- Chaque article a un **responsable de mise à jour** et une **date de révision**
  (revue annuelle).
- Marquez les articles « FAQ publique » pour les déclarants (ex. « Changer le
  toner vous-même ») vs « Interne » (procédures SAV).

### Workflow de publication

Technicien rédige (brouillon) → Superviseur relit/valide → Publication.
Un article validé peut être **lié aux tickets** (solution type) et proposé
automatiquement lors de la création d'un ticket de même catégorie.

---

# PARTIE IV — LE PARC (CMDB)

## 25. Le parc : vision d'ensemble (CMDB)

`Parc` : l'inventaire. Chaque **type d'objet** a sa fiche : ordinateurs, moniteurs,
imprimantes, matériels réseau, téléphones, onduleurs... et tout objet personnalisé.

### Principes

- **Un objet = une fiche**, avec : état (neuf, en service, en panne, au rebut),
  type, marque, modèle, n° de série, n° d'inventaire, entité, lieu, utilisateur,
  groupe responsable, fournisseur, contrat(s), dates (achat, mise en service,
  garantie), valeur, composants, liaisons, documents, historique, tickets liés.
- Les **liaisons** font la CMDB : « ce PC est branché sur ce switch, utilise cette
  imprimante, appartient à cet utilisateur ».
- L'**historique** de chaque fiche est automatique : qui a modifié quoi, quand.

### États et cycle de vie du matériel

```
Commandé → Livré → En service ──▶ En maintenance ──▶ En service
                            └──▶ Au rebut / Sorti (don, reprise)
```

> **Discipline :** aucun matériel n'entre ou ne sort sans fiche GLPI à jour.
> Le technicien qui installe un copieur crée/met à jour la fiche **le jour même**.

---

## 26. Ordinateurs : fiches, composants, liaisons

Fiche ordinateur : système d'exploitation, processeur, RAM, disques, carte réseau
(adresses MAC/IP), logiciels installés, périphériques liés (moniteur, imprimante),
utilisateur, dernière modification.

**Alimentation automatique :** l'agent FusionInventory (sections 33-36) remonte
tout cela sans saisie manuelle. La saisie manuelle ne concerne que les champs
« gestion » (utilisateur, lieu, inventaire, garantie).

### Champs critiques à renseigner (même avec inventaire auto)

| Champ | Pourquoi |
|---|---|
| N° d'inventaire interne | Étiquette physique ↔ fiche GLPI |
| Utilisateur / Groupe | Responsabilité |
| Lieu (bâtiment, bureau) | Retrouver la machine |
| Statut | En service / en panne / stock |
| Date de fin de garantie | Alertes automatiques (section 37) |

---

## 27. Moniteurs, imprimantes et périphériques

Même logique : marque, modèle, série, liaison (« connecté à » tel ordinateur),
consommables compatibles (pour les imprimantes : **quelles cartouches ?** —
lien direct vers le stock, section 30).

**Cas des imprimantes réseau :** renseignez l'adresse IP, la file d'impression,
le pilote. L'agent FusionInventory peut les découvrir en SNMP (section 35) :
compteur de pages remonté automatiquement — **données de facturation sans relevé
manuel**.

---

## 28. Fiches copieurs : le métier (marque, modèle, série, compteur, contrat)

C'est **le cœur du métier** : chaque copieur client est une fiche « Imprimante »
(enrichie) ou un type d'objet dédié. Voici la fiche type à normaliser pour tout
le parc :

### Fiche copieur normalisée

| Rubrique | Champs |
|---|---|
| **Identification** | Marque (Kyocera, Canon, Ricoh...), modèle (TASKalfa 4054ci), n° de série, n° d'inventaire, adresse MAC, IP |
| **Localisation** | Client (entité), site, bâtiment, étage/bureau, contact sur place |
| **Compteurs** | N&B, couleur, total, date du dernier relevé (champs personnalisés) |
| **Contrat** | Contrat de maintenance lié (section 37), type (forfait, coût/copie), date de fin, SLA applicable |
| **Consommables** | Toners compatibles (liens vers stock), kit de fusion, tambour — avec seuils d'alerte |
| **Historique** | Tous les tickets liés, interventions, pièces remplacées |
| **Documents** | Contrat signé (PDF), PV d'installation, manuel |

### Relevés de compteurs : processus

1. **Mensuel** (ticket récurrent, section 19) ou **automatique** (SNMP, section 35).
2. Saisie dans les champs personnalisés de la fiche + suivi dans le ticket.
3. **Alerte** si écart anormal (ex. +50 % vs mois précédent = fuite ? erreur de
   saisie ? à contrôler).
4. Les compteurs alimentent la **facturation au coût/copie** (export CSV pour
   la comptabilité).

> **Lien avec le guide Kyocera** : les codes d'accès maintenance (ex. 10871087),
> les U-codes et les C-codes documentés dans le guide copieurs doivent exister
> en articles de la base de connaissances (section 24) et être liés aux fiches
> copieurs concernées.

---

## 29. Périphériques réseau : switchs, routeurs, bornes Wi-Fi, onduleurs

- **Matériels réseau** (`Parc > Matériels réseau`) : switchs, routeurs, pare-feux.
  Renseignez les ports, VLAN, firmware, liaisons (« ce switch alimente ces PC »).
  Découverte SNMP via FusionInventory (section 35).
- **Onduleurs** : GLPI n'a pas de type natif « onduleur » en 10.x — utilisez
  `Parc > Autres` ou créez un type via le plugin **Champs personnalisés** /
  **Objects**. Champs métier : puissance (kVA), autonomie, date des batteries,
  prochaine maintenance (lien avec le guide onduleurs !).
- **Liaisons** : branchez chaque équipement critique sur son onduleur dans la
  CMDB (« alimenté par »). En cas de coupure, vous savez **exactement** ce qui
  est protégé ou non.

---

## 30. Cartouches et consommables : gérer le stock de toner

`Parc > Cartouches` (modèles) + `Parc > Consommables` : le **magasin** de GLPI.

### Organisation du stock

1. Créez les **modèles de cartouches** : référence constructeur (ex. TK-8345K),
   couleur, capacité (pages), seuil d'alerte.
2. Liez chaque modèle aux **imprimantes/copieurs compatibles**.
3. **Entrées** : à chaque livraison fournisseur (bon de livraison → quantité).
4. **Sorties** : le technicien « donne » une cartouche depuis le ticket ou la
   fiche imprimante → le stock décrémente, le coût s'impute au client/contrat.

### Alertes et réassort

- Seuil d'alerte par modèle (ex. < 3 toners noirs TK-8345) → notification au
  magasinier + au chef de service.
- **Rapport mensuel** : consommation par client, par modèle → ajustement des
  commandes et détection des anomalies (un client qui consomme 3× plus = fuite ?
  usage détourné ?).
- Inventaire physique **trimestriel** : comptage réel vs GLPI, régularisation
  avec motif (écart justifié dans l'historique).

> **Le saviez-vous ?** Lier les cartouches aux tickets d'intervention permet de
> calculer le **coût réel par client** (pièces + consommables + main-d'œuvre) —
> indispensable pour renégocier les contrats déficitaires.

---

## 31. Logiciels, licences et versions

`Parc > Logiciels` : catalogue (nom, éditeur, versions), puis **licences**
(type : OEM, volume, SaaS ; nombre de postes ; dates d'expiration ; clé).

- L'agent FusionInventory remonte les logiciels installés → rapprochement
  automatique avec les licences → **alertes de sous/sur-licence**.
- Dates d'expiration (antivirus, certificats, abonnements SaaS) : notifications
  à J-60 / J-30 / J-7.
- Dictionnaire des logiciels (`Configuration > Dictionnaires`) : fusionne
  « Acrobat Reader », « Adobe Acrobat Reader DC »... en une seule entrée propre.

---

## 32. Réservations de matériel

`Assistance > Réservations` : prêt de matériel (vidéoprojecteur, PC portable de
dépannage, copieur de remplacement). Le déclarant réserve sur un créneau, le
technicien valide, le retour est tracé. Fini le « qui a pris le portable n°12 ? ».

> **Usage SAV :** gérez ainsi votre **parc de prêt** (copieurs de remplacement
> pendant une réparation longue) : chaque prêt = réservation liée au ticket
> d'intervention.

---

# PARTIE V — INVENTAIRE AUTOMATIQUE

## 33. FusionInventory : présentation et architecture

**FusionInventory** = le plugin GLPI + l'**agent** installé sur les postes (ou
serveur dédié pour le SNMP). Il remonte : matériel, logiciels, réseau, imprimantes.

```
Poste Windows/Linux ──agent──▶ https://glpi/plugins/fusioninventory/
Serveur de découverte ──SNMP──▶ switchs, imprimantes, copieurs ──▶ GLPI
```

> **Note :** le plugin historique « FusionInventory for GLPI » évolue ; en
> GLPI 10.x, l'inventaire natif (`Administration > Inventaire`, agent GLPI)
> reprend l'essentiel. Vérifiez la compatibilité exacte plugin ↔ version GLPI
> **avant** d'installer. Les principes ci-dessous restent valables.

---

## 34. Installation de l'agent FusionInventory

### Windows (GPO ou script)

```powershell
# Installation silencieuse de l'agent GLPI/FusionInventory
msiexec /i "glpi-agent-1.x.msi" /qn `
  SERVER="https://glpi.entreprise.lan" `
  RUNNOW=1
```

Fichier de config `C:\Program Files\GLPI-Agent\etc\glpi-agent.cfg` :

```ini
server = https://glpi.entreprise.lan
tag = AGENCE-NORD
delaytime = 24
```

### Linux (Debian/Ubuntu)

```bash
sudo apt install -y glpi-agent   # ou fusioninventory-agent selon dépôt
sudo tee /etc/glpi-agent/conf.d/serveur.cfg <<'EOF'
server = https://glpi.entreprise.lan
tag = SIEGE
EOF
sudo systemctl enable --now glpi-agent
```

### Vérification côté GLPI

`Administration > Inventaire > Agents` : l'agent apparaît après son premier
contact. Forcez un inventaire : `glpi-agent --debug` côté poste, ou tâche
« Forcer l'inventaire » côté serveur.

---

## 35. Inventaire automatique : réseau et SNMP

### Découverte réseau

`Plugins > FusionInventory > Découverte réseau` : définissez des plages IP
(ex. `192.168.10.0/24`), une tâche planifiée lance le scan via l'agent.
Résultat : liste des équipements répondant (ping/ARP), à importer ou ignorer.

### Inventaire SNMP (imprimantes, copieurs, switchs)

1. `Configuration > Authentification SNMP` : créez la communauté
   (`public` → **changez-la !**, ou SNMPv3 avec authPriv).
2. Tâche « Inventaire réseau » : plage IP + authentification SNMP.
3. L'agent interroge chaque équipement : modèle, série, **compteurs de pages**,
   niveaux de toner, firmware.

> **Jackpot métier :** les compteurs des copieurs réseau remontent **sans
> déplacement**. Programmez l'inventaire SNMP la nuit du 30 de chaque mois →
> relevés de facturation automatiques (export API, section 53).

### Bonnes pratiques SNMP

- SNMPv3 (auth + chiffrement) dès que l'équipement le supporte.
- Ne scannez que les plages utiles, la nuit, pour ne pas saturer le réseau.
- Mettez les équipements inventoriés en **verrou** sur les champs critiques
  (n° de série) pour éviter les écrasements.

---

## 36. Règles d'import et de déduplication

`Administration > Inventaire > Règles` : GLPI décide si un inventaire remonté
= **nouvel objet** ou **mise à jour** d'un existant. Critères par type :

| Type | Critère de rapprochement conseillé |
|---|---|
| Ordinateur | N° de série + UUID |
| Imprimante/Copieur | N° de série, puis adresse MAC |
| Matériel réseau | N° de série, puis IP |

- **Verrous** (`Verrous` sur la fiche) : empêchent l'agent d'écraser les champs
  saisis manuellement (lieu, utilisateur, inventaire).
- **Dictionnaires** : normalisent les marques/modèles remontés
  (« HEWLETT-PACKARD » → « HP »).
- Purgez les doublons via `Administration > Maintenance > Vérification` puis
  fusion manuelle si besoin.

> **Erreur classique :** réinstaller un poste sans conserver l'UUID → doublon.
> Procédure : avant réinstallation, noter l'UUID ; après, forcer le rapprochement
> sur le n° de série.

---

# PARTIE VI — CONTRATS, BUDGETS, PROJETS

## 37. Gestion des contrats et des fournisseurs

`Gestion > Contrats` + `Gestion > Fournisseurs` : le suivi contractuel.

### Fiche contrat

| Champ | Exemple |
|---|---|
| Nom | `Maintenance copieurs — Mairie (2026)` |
| Fournisseur | (si sous-traité) ou « Interne » |
| Type | Forfait / Coût par copie / Garantie |
| Dates | Début, fin, **préavis de reconduction** |
| Coût | Annuel, modalités |
| SLA lié | « Collectivités » (section 21) |
| Éléments couverts | Les 12 copieurs (liaison) |
| Documents | Contrat signé (PDF) |
| Alertes | Fin de contrat à J-90 / J-30 |

### Notifications critiques

- **Fin de garantie** des matériels (J-30) → renouvellement ou remplacement.
- **Fin de contrat** (J-90/J-30) → renégociation ; un contrat oublié reconduit
  tacitement = de l'argent perdu.
- **Seuils de consommables** liés au contrat (inclus ou facturés ?).

### Fournisseurs

Fiche par fournisseur : contacts, délais constatés, tickets liés
(« assigné au fournisseur » quand la panne relève de la garantie constructeur).
Le **taux de respect des délais** par fournisseur = argument de renégociation.

---

## 38. Budgets et lignes budgétaires

`Gestion > Budgets` : enveloppes annuelles (ex. « Budget SAV 2026 — 180 k€ »),
découpées en **lignes** (pièces détachées, consommables, sous-traitance,
renouvellement parc).

- Chaque **coût de ticket** (pièces + main-d'œuvre saisie) s'impute sur une ligne.
- Chaque **achat** (fiche matériel : valeur d'achat) aussi.
- Tableau de bord : consommé vs prévu, par ligne, avec alertes à 80 % / 100 %.

> **Rituel du chef de service :** le 5 de chaque mois, 15 minutes sur le budget
> GLPI avant la revue de direction. Aucune surprise en fin d'année.

---

## 39. Projets et tâches : piloter les chantiers

`Outils > Projets` : pour les chantiers (pas les incidents) — ex. « Déploiement
de 20 copieurs — Groupe Scolaire », « Migration du serveur GLPI », « Refonte du
plan de maintenance préventive ».

- **Tâches** : découpage, responsables, dates, dépendances, avancement %.
- **Tickets liés** : les incidents survenus pendant le projet restent tracés.
- **Coûts** : temps passé × taux horaire → coût réel du projet.
- Diagramme de Gantt intégré pour la présentation en réunion.

**Distinction nette à faire respecter :** incident (ticket) vs projet.
« Le copieur est en panne » = ticket. « Remplacer les 12 copieurs du client » =
projet (avec tickets d'installation rattachés).

---

# PARTIE VII — PILOTAGE : RAPPORTS, RECHERCHE, SUPERVISION

## 40. Rapports et statistiques : le tableau de bord du chef de service

`Assistance > Statistiques` + `Outils > Rapports` : le pilotage chiffré.

### Les 10 indicateurs à suivre chaque semaine

| # | Indicateur | Où le trouver |
|---|---|---|
| 1 | Tickets ouverts / résolus / en retard | Statistiques globales |
| 2 | % SLA tenus (TTO/TTR) par client | Statistiques par SLA |
| 3 | Ancienneté du stock (tickets > 15 jours) | Recherche : statut + date |
| 4 | Charge par technicien (tickets en cours) | Statistiques par technicien |
| 5 | Top 10 catégories (panne la plus fréquente) | Statistiques par catégorie |
| 6 | Top 10 matériels (copieur le plus en panne) | Statistiques par élément |
| 7 | Consommation toner par client | Rapport cartouches |
| 8 | Coût SAV par client / contrat | Rapports + budgets |
| 9 | Note satisfaction moyenne | Sondages |
| 10 | Tickets récurrents en retard (préventif) | Recherche tickets récurrents |

### Tableaux de bord personnalisés

`Outils > Tableaux de bord` : assemblez vos indicateurs en widgets
(nombre de tickets, graphiques, listes). Créez un dashboard **« Pilotage SAV »**
affiché par défaut aux superviseurs, et un **« Mon activité »** pour les techniciens.

### Export

Toutes les listes s'exportent en **CSV/PDF** (icône en bas de liste) →
réutilisation dans le rapport mensuel de direction (modèle en annexe E).

---

## 41. Recherche, filtres et vues enregistrées

La **recherche avancée** (`Assistance > Tickets` → « Rechercher ») est d'une
puissance redoutable : critères multiples (ET/OU), tri, colonnes choisies.

### Recherches à enregistrer (marque-pages)

- « Mes tickets en cours » (assigné à = moi, statut ≠ résolu/clos).
- « Tickets en attente depuis > 7 jours » (relance).
- « Copieurs — compteurs non relevés ce mois ».
- « Toners sous le seuil d'alerte ».
- « Contrats se terminant dans < 90 jours ».
- « Tickets sans catégorie / sans élément associé » (contrôle qualité).

`Outils > Notes` : pensez à documenter chaque recherche enregistrée
(à quoi elle sert, qui l'utilise).

> **Contrôle qualité hebdo (15 min) :** lancez « tickets sans élément associé »
> et « tickets en attente sans date de relance ». Zéro résultat = saisie
> disciplinée. Sinon, rappel aux techniciens concernés.

---

# PARTIE VIII — SÉCURITÉ

## 42. Sécurité — comptes et mots de passe

La sécurité de GLPI = la sécurité de **toutes les données du SAV** (coordonnées
clients, contrats, mots de passe parfois notés dans les tickets...).

### Politique des mots de passe

`Configuration > Générale > Sécurité` :

| Paramètre | Valeur conseillée |
|---|---|
| Longueur minimale | 12 caractères |
| Complexité | Majuscules + minuscules + chiffres + spéciaux |
| Durée de validité | 180 jours (comptes locaux) |
| Historique | 5 derniers interdits |
| Verrouillage | 5 échecs → blocage 15 min |
| Expiration | Notification à J-7 (action `sendpasswordexpires`) |

### Règles

- **Comptes nominatifs uniquement** : jamais de compte partagé « sav » ou « admin ».
  Chaque action est tracée par utilisateur (audit).
- Privilégiez l'**authentification AD/LDAP** (section 12) : un départ = désactivation
  dans l'AD = accès GLPI coupé automatiquement.
- **Double authentification (2FA)** : GLPI 10.x supporte le TOTP natif
  (`Préférences > Sécurité`) — imposez-la aux profils Admin et Superviseur.
- Revoyez les comptes **trimestriellement** : désactivez les inactifs depuis 90 jours
  (recherche : dernière connexion).
- **Jamais de mot de passe en clair** dans un ticket, un suivi ou la base de
  connaissances. Utilisez un coffre (KeePass/Vault) et ne stockez que la référence.

---

## 43. Sécurité — durcissement des profils

Reprenez la matrice de la section 11 et appliquez le **moindre privilège** :

- [ ] Le profil déclarant ne voit **que** sa propre entité (pas de récursivité).
- [ ] Les techniciens ne peuvent pas **supprimer** (seulement mettre au rebut).
- [ ] Seuls Admin/Superviseur gèrent **utilisateurs, profils, entités, règles**.
- [ ] L'accès à `Configuration` est réservé à l'Admin.
- [ ] Les **fournisseurs externes** (s'ils ont un accès) : profil dédié, entité
      limitée, pas d'accès aux budgets ni aux autres clients.
- [ ] Désactivez les **comptes par défaut** inutilisés (`tech`, `normal`,
      `post-only` si vous avez créé vos propres profils).

### Audit

`Administration > Journaux` : historique de **toutes** les modifications
(qui, quoi, quand). En cas d'incident, c'est votre piste d'audit.
Conservez les journaux ≥ 1 an (purge paramétrable, attention aux obligations
contractuelles/RGPD).

---

## 44. Sécurité — HTTPS/TLS de bout en bout

GLPI **ne doit jamais** circuler en HTTP clair en production (mots de passe,
données clients).

### Certificat

- En interne : certificat d'une **CA interne** (ou Let's Encrypt si exposé).
- Déposez cert + clé : `/etc/ssl/certs/glpi.crt`, `/etc/ssl/private/glpi.key`
  (permissions `640`, propriétaire `root:ssl-cert`, www-data dans le groupe).

### Redirection HTTP → HTTPS (Apache)

```apache
<VirtualHost *:80>
    ServerName glpi.entreprise.lan
    Redirect permanent / https://glpi.entreprise.lan/
</VirtualHost>
```

### En-têtes de sécurité (Apache, vhost 443)

```apache
Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"
Header always set X-Content-Type-Options "nosniff"
Header always set X-Frame-Options "SAMEORIGIN"
Header always set Referrer-Policy "strict-origin-when-cross-origin"
Header always set Content-Security-Policy "default-src 'self' https:; img-src 'self' data:;"
```

### Côté GLPI

- `Configuration > Générale` : URL de l'application en `https://...`.
- `php.ini` : `session.cookie_secure = 1`, `session.cookie_httponly = 1`,
  `session.cookie_samesite = "Lax"`.
- Testez avec un scanner (testssl.sh en interne) après chaque changement.

---

## 45. Sécurité — durcissement serveur et GLPI

### Système

- Mises à jour automatiques de sécurité (`unattended-upgrades` sur Debian/Ubuntu).
- Pare-feu : 443 ouvert vers le LAN, SSH restreint, **pas d'exposition directe
  sur Internet** sans WAF/VPN (préférez un VPN pour l'accès distant des techniciens).
- Sauvegardes chiffrées et **testées** (partie IX).

### GLPI applicatif

- Dossiers `config/` et `files/` **hors racine web** (section 8.4).
- `install/` supprimé (section 8.1).
- Droits : fichiers `644`, dossiers `755`, propriétaire `www-data` — jamais de `777`.
- ` marketplace` : n'installez que des plugins **compatibles avec votre version
  exacte** et maintenus (section 51).
- Désactivez l'affichage des erreurs PHP en production :
  `display_errors = Off`, `log_errors = On` (les stack traces en page blanche
  révèlent des chemins et versions — voir section 54).

### Fichiers sensibles à surveiller

| Fichier | Risque si exposé |
|---|---|
| `config/config_db.php` | Identifiants MariaDB |
| `files/_log/*.log` | Chemins, erreurs, parfois données |
| Dumps de sauvegarde dans `files/` | Base complète ! → stocker hors web |

---

# PARTIE IX — SUPERVISION ET SAUVEGARDE

## 46. Supervision de GLPI : ce qu'il faut surveiller

GLPI supervise le parc ; **qui supervise GLPI ?** Votre Zabbix/Nagios/Prometheus.

### Points de contrôle

| Sonde | Seuil d'alerte | Comment |
|---|---|---|
| HTTPS `https://glpi.entreprise.lan` | code ≠ 200 | check_http |
| Cron GLPI (dernière exécution) | > 10 min | script section 9.4 |
| File de notifications `queuednotification` | > 50 en attente depuis > 30 min | requête SQL |
| Espace disque `/var/lib/glpi` | > 80 % | les pièces jointes grossissent vite |
| MariaDB joignable + réplication (si) | — | check_mysql |
| Validité certificat TLS | < 30 jours | check_http --ssl |
| Temps de réponse page d'accueil | > 5 s | indicateur de saturation |

### Requêtes SQL utiles pour la supervision

```sql
-- Notifications en file d'attente (anormal si ancien)
SELECT COUNT(*) AS en_attente,
       TIMESTAMPDIFF(MINUTE, MIN(date_creation), NOW()) AS plus_ancien_min
FROM glpidb.glpi_queuednotifications;

-- Tâches cron en erreur
SELECT name, lastrun, lastcode FROM glpidb.glpi_crontasks
WHERE state != 1 OR lastcode != 0;

-- Tickets critiques ouverts sans assigné
SELECT COUNT(*) FROM glpidb.glpi_tickets
WHERE status NOT IN (5,6) AND priority >= 4
  AND (users_id_recipient = 0);
```

> Les statuts GLPI : 1=Nouveau, 2=En cours (attribué), 3=En cours (planifié),
> 4=En attente, 5=Résolu, 6=Clos. Vérifiez dans votre version
> (`glpi_configs` / interface).

---

## 47. Sauvegarde : stratégie complète (fichiers + base)

**Règle 3-2-1** : 3 copies, 2 supports différents, 1 hors site.

### Ce qu'il faut sauvegarder

| Quoi | Où | Fréquence |
|---|---|---|
| Base `glpidb` (dump) | `mysqldump` | **Quotidienne** |
| `/var/lib/glpi/files` (pièces jointes, documents) | rsync/tar | Quotidienne (incrémentale) |
| `/var/lib/glpi/config` + vhost Apache/nginx | tar | À chaque changement |
| Code `/var/www/glpi` | tar versionné | À chaque mise à jour |

### Rétention type

- Quotidiennes : 14 jours en local.
- Hebdomadaires : 8 semaines sur NAS.
- Mensuelles : 12 mois hors site (chiffrées).

> **RGPD :** les sauvegardes contiennent des données personnelles (noms,
> courriels). Chiffrez-les (`gpg`/`openssl`) et limitez l'accès au support
> de sauvegarde.

---

## 48. Sauvegarde automatisée : scripts prêts à l'emploi

### Script `/usr/local/bin/backup-glpi.sh`

```bash
#!/bin/bash
set -euo pipefail
DEST="/srv/backup/glpi"
DATE=$(date +%F)
RETENTION_LOCAL=14
DB="glpidb"; DBUSER="glpi"; DBPASS="REMPLACER"  # ou fichier ~/.my.cnf 600 !

mkdir -p "$DEST"
# 1. Dump base
mysqldump --single-transaction --routines --triggers \
  -u"$DBUSER" -p"$DBPASS" "$DB" | gzip > "$DEST/glpidb-$DATE.sql.gz"
# 2. Fichiers applicatifs
tar czf "$DEST/glpi-files-$DATE.tar.gz" -C / var/lib/glpi
# 3. Config web + code (léger)
tar czf "$DEST/glpi-conf-$DATE.tar.gz" /etc/apache2/sites-available/glpi.conf
# 4. Rotation locale
find "$DEST" -name 'glpidb-*.sql.gz' -mtime +$RETENTION_LOCAL -delete
find "$DEST" -name 'glpi-files-*.tar.gz' -mtime +$RETENTION_LOCAL -delete
# 5. Copie vers NAS (exemple)
rsync -a --delete "$DEST/" nas:/backups/glpi/ || echo "WARN: rsync NAS échoué"
echo "OK backup GLPI $DATE"
```

```bash
sudo chmod 700 /usr/local/bin/backup-glpi.sh
# cron root : tous les jours à 2h
echo "0 2 * * * root /usr/local/bin/backup-glpi.sh >> /var/log/backup-glpi.log 2>&1" \
  | sudo tee /etc/cron.d/backup-glpi
```

### Variante sans mot de passe en clair

`/root/.my.cnf` (permissions `600`) :

```ini
[client]
user = glpi
password = MOT_DE_PASSE_FORT
host = localhost
```

puis `mysqldump --defaults-extra-file=/root/.my.cnf ...` (plus sûr que `-p`).

---

##  49. Restauration : procédure testée pas à pas

**Une sauvegarde non testée = pas de sauvegarde.** Testez la restauration
**trimestriellement** sur une machine isolée.

### Procédure

```bash
# 1. Stopper le cron GLPI et le web (éviter les écritures)
sudo systemctl stop apache2   # ou nginx

# 2. Restaurer la base
zcat /srv/backup/glpi/glpidb-2026-09-20.sql.gz \
  | mysql -u root -p glpidb_restauree   # d'abord sur une base de test !

# 3. Restaurer les fichiers
sudo tar xzf /srv/backup/glpi/glpi-files-2026-09-20.tar.gz -C /
sudo chown -R www-data:www-data /var/lib/glpi

# 4. Redémarrer et vérifier
sudo systemctl start apache2
# → page de connexion OK, connexion admin OK, ticket récent présent,
#    pièce jointe téléchargeable, cron relancé
```

### Checklist de validation post-restauration

- [ ] Connexion avec un compte admin.
- [ ] Un ticket créé la veille de la sauvegarde est présent avec ses suivis.
- [ ] Une pièce jointe s'ouvre.
- [ ] Les compteurs copieurs affichent les dernières valeurs.
- [ ] Le cron tourne (`front/cron.php` manuel OK).
- [ ] Envoi d'un courriel de test OK.

> **Exercice annuel :** restauration complète sur VM vierge, chronométrée.
> Objectif : RTO (durée max d'interruption) et RPO (perte de données max)
> **écrits et validés** par la direction.

---

# PARTIE X — MISE À JOUR, PLUGINS, API

## 50. Mise à jour de GLPI : méthode sans casse

### Règle d'or

**Jamais de mise à jour sans sauvegarde complète + testée** (partie IX),
et **jamais directement en production** : d'abord sur une **copie de test**.

### Procédure (ex. 10.0.x → 10.0.y, ou 10.x → 10.(x+1))

```bash
# 1. Sauvegarde complète (section 48) — VÉRIFIER le dump
# 2. Cloner en environnement de test (VM ou dossier + base _test)
# 3. Sur le test : déployer la nouvelle version
cd /tmp && wget https://github.com/glpi-project/glpi/releases/download/10.0.XX/glpi-10.0.XX.tgz
tar xzf glpi-10.0.XX.tgz
sudo rsync -a --delete glpi/ /var/www/glpi/ \
  --exclude=config --exclude=files --exclude=marketplace
sudo chown -R www-data:www-data /var/www/glpi
# 4. Lancer la mise à jour : https://glpi-test/ → assistant « Mise à jour »
#    ou en CLI : sudo -u www-data php /var/www/glpi/bin/console db:update --no-interaction
# 5. Mettre à jour les PLUGINS vers leurs versions compatibles 10.x
# 6. Tests : connexion, ticket complet, recherche, impression PDF, API, cron
# 7. Si OK : reproduire en production (fenêtre de maintenance annoncée)
```

### Points de vigilance

- Vérifiez la **matrice de compatibilité des plugins** avant chaque montée
  de version (un plugin incompatible = page blanche, section 54).
- Les montées **majeures** (ex. 9.5 → 10) exigent souvent PHP plus récent :
  mettez à jour PHP **avant** GLPI.
- Après mise à jour : videz le cache (`Administration > Maintenance > Vider les
  caches`) et relancez le cron manuellement une fois.

---

## 51. Plugins : lesquels installer, lesquels éviter

Installation : `Configuration > Plugins` (marketplace intégré en 10.x) ou dépôt
manuel dans `marketplace/`.

### Plugins utiles pour un SAV (à valider vs votre 10.0.x exacte)

| Plugin | Usage |
|---|---|
| **Champs personnalisés** (Fields) | Compteurs copieurs, codes erreur sur tickets/fiches |
| **Comportements** (Behaviors) | Champs obligatoires, formats |
| **Impression PDF** | Tickets/fiches en PDF pour les clients |
| **Outils d'inventaire / Agent** | Selon version, inventaire natif |
| **Satisfaction avancée** | Sondages enrichis |
| **Tag** | Étiquettes libres (ex. « sous-garantie ») |
| **Carbon** (thème sombre) | Confort techniciens |

### Règles

1. **Un plugin = un besoin réel.** Chaque plugin = surface d'attaque + risque
   à chaque mise à jour.
2. Vérifiez : compatible 10.x ? maintenu (commit < 6 mois) ? avis communauté ?
3. Testez sur l'environnement de **test** d'abord.
4. Avant chaque mise à jour GLPI : mettez à jour les plugins **en premier**.
5. Désinstallez (pas seulement désactivez) les plugins abandonnés.

---

## 52. API REST : authentification et premiers appels

L'API REST (`https://glpi.entreprise.lan/apirest.php`) permet de créer des
tickets, remonter des compteurs, interroger le parc depuis des scripts.

### Activation

1. `Configuration > Générale > API` : activer l'API REST.
2. Créez un utilisateur dédié `svc-api` (profil restreint, ex. Technicien
   sur les entités utiles) — **jamais** le compte admin.
3. `Préférences` de `svc-api` : générer un **token API** (et un token d'app
   si besoin).

### Session

```bash
GLPI="https://glpi.entreprise.lan/apirest.php"
APP_TOKEN="TOKEN_APPLICATION"      # optionnel mais recommandé
USER_TOKEN="TOKEN_UTILISATEUR_svc-api"

# 1. Initier la session
SESSION=$(curl -s -X GET "$GLPI/initSession?user_token=$USER_TOKEN" \
  -H "App-Token: $APP_TOKEN" | python3 -c "import sys,json;print(json.load(sys.stdin)['session_token'])")

# 2. Appel authentifié (ex. : mon profil)
curl -s -H "Session-Token: $SESSION" -H "App-Token: $APP_TOKEN" \
  "$GLPI/getMyProfiles" | python3 -m json.tool

# 3. Tuer la session
curl -s -X GET -H "Session-Token: $SESSION" -H "App-Token: $APP_TOKEN" \
  "$GLPI/killSession"
```

> **Sécurité :** tokens = mots de passe. Stockez-les en coffre, rotation annuelle,
> profil API au moindre privilège, HTTPS obligatoire.

---

## 53. API REST : exemples concrets (tickets, parc, compteurs)

### Créer un ticket d'intervention copieur

```bash
curl -s -X POST -H "Session-Token: $SESSION" -H "App-Token: $APP_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "input": {
      "name": "Bourrage récurrent bac 2 — Mairie, bureau 204",
      "content": "Code J-0511 affiché. Compteur N&B : 212458.",
      "itilcategories_id": 12,
      "urgency": 3, "impact": 2,
      "type": 1,
      "_users_id_requester": 45,
      "entities_id": 3
    }
  }' "$GLPI/Ticket/"
```

### Rechercher les copieurs d'un client

```bash
curl -s -G -H "Session-Token: $SESSION" -H "App-Token: $APP_TOKEN" \
  --data-urlencode "criteria[0][field]=80" \
  --data-urlencode "criteria[0][searchtype]=equals" \
  --data-urlencode "criteria[0][value]=3" \
  "$GLPI/search/Printer" | python3 -m json.tool
```

### Mettre à jour les compteurs (champs personnalisés)

```bash
# Exemple : compteur N&B = champ personnalisé id 101 sur l'imprimante id 27
curl -s -X PUT -H "Session-Token: $SESSION" -H "App-Token: $APP_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"input": {"id": 27, "plugin_fields_compteurnb": 213102}}' \
  "$GLPI/Printer/27"
```

### Script Python : relevé mensuel automatique (SNMP → GLPI)

```python
#!/usr/bin/env python3
"""Relève les compteurs via SNMP et les pousse dans GLPI (API REST)."""
import requests
COPIERS = [  # (ip, communauté, id_fiche_glpi)
    ("192.168.10.21", "COMMUNAUTE_RO", 27),
    ("192.168.10.22", "COMMUNAUTE_RO", 31),
]
# 1. snmpget iso.3.6.1.2.1.43.10.2.1.4.1.1  (prtMarkerLifeCount)
#    via subprocess/pysnmp -> compteur total
# 2. session API (cf. section 52)
# 3. PUT /Printer/<id> avec les champs personnalisés compteurs
# 4. log + alerte si écart > 30 % vs mois précédent
print("Squelette : complétez avec pysnmp + vos IDs de champs personnalisés.")
```

> Ce script, lancé par cron le 1er du mois, **supprime la tournée de relevés
> manuels** sur tout le parc réseau. Vérifiez toujours un échantillon à la main
> les 3 premiers mois.

---

# PARTIE XI — DÉPANNAGE

## 54. Dépannage — page blanche / erreur 500

**Symptôme :** page blanche ou « HTTP 500 » après mise à jour, installation
de plugin, ou modification PHP.

### Diagnostic express

```bash
# 1. Logs applicatifs GLPI (le plus parlant)
tail -n 100 /var/lib/glpi/files/_log/php-errors.log
tail -n 100 /var/lib/glpi/files/_log/error.log
# 2. Log Apache / PHP-FPM
sudo tail -n 100 /var/log/apache2/glpi_error.log
sudo tail -n 100 /var/log/php8.2-fpm.log
# 3. Tester la syntaxe PHP du fichier suspect
php -l /var/www/glpi/marketplace/monplugin/hook.php
```

### Causes fréquentes et remèdes

| Cause | Indice dans les logs | Remède |
|---|---|---|
| Plugin incompatible | `Fatal error ... marketplace/xxx` | Renommer le dossier du plugin en `.off` |
| Extension PHP manquante | `Call to undefined function` | `apt install php-xxx` + restart |
| `memory_limit` trop bas | `Allowed memory size exhausted` | passer à 256M/512M |
| Droits fichiers | `Permission denied ... files/` | `chown -R www-data:www-data` |
| `display_errors=Off` | page blanche sans log web | lire les logs fichiers ci-dessus |
| Cache corrompu après MAJ | erreurs diverses | `Administration > Maintenance > Vider les caches` (ou `rm -rf files/_cache/*`) |

### Mode debug ciblé

`Configuration > Générale > Mode débogage` : à activer **temporairement** pour
reproduire l'erreur avec un compte de test — jamais en production ouverte.

---

## 55. Dépannage — le cron ne tourne pas

**Symptômes :** pas de courriels, SLA figés, collecteur muet, tickets récurrents
non créés.

### Checklist

1. Le cron système existe-t-il ?
   ```bash
   cat /etc/cron.d/glpi
   sudo grep CRON /var/log/syslog | grep -i glpi | tail
   ```
2. Test manuel :
   ```bash
   sudo -u www-data php /var/www/glpi/front/cron.php; echo "code=$?"
   ```
   - code ≠ 0 → lire l'erreur affichée.
   - silencieux mais rien ne change → voir 3.
3. `Configuration > Actions automatiques` : chaque action a-t-elle une
   **dernière exécution récente** ? Une action en « erreur » bloque parfois les
   suivantes → consultez son journal, corrigez, puis « Exécuter ».
4. Conflit de **verrou** : `SELECT * FROM glpi_crontasks WHERE state=2;`
   (2 = en cours) — un cron tué brutalement laisse un verrou ; repassez à 1.
5. PHP CLI vs PHP web : vérifiez que le **CLI** a les bonnes extensions
   (`php -m`) et le bon `php.ini` (`php --ini`).

> **Piège :** deux crons (système + pseudo-cron web) qui se marchent dessus.
> Choisissez **un seul** mode (section 9).

---

## 56. Dépannage — les courriels ne partent pas

### Checklist ordonnée

1. **File d'attente** : `Configuration > Notifications > File d'attente des
   notifications` — des courriels en attente ? Leur âge ? Un message d'erreur
   SMTP s'affiche par ligne (authentification, connexion refusée...).
2. **Cron** : `queuednotification` tourne-t-il ? (section 55)
3. **Paramètres SMTP** : hôte, port, chiffrement, identifiants — refaites le
   **courriel de test** (section 8.5).
4. **Réseau** : depuis le serveur,
   ```bash
   nc -zv smtp.entreprise.lan 587
   openssl s_client -connect smtp.entreprise.lan:587 -starttls smtp
   ```
5. **Notifications activées ?** `Configuration > Notifications > Notifications` :
   l'événement est-il actif **pour ce profil/entité** ? Un modèle désactivé =
   silence total.
6. **Anti-spam** : vérifiez les spams du destinataire + SPF/DKIM du domaine
   d'envoi (`glpi@entreprise.lan` doit être autorisé à envoyer).
7. **Collecteur (sens inverse)** : si les mails **entrants** ne créent pas de
   tickets → tester la connexion IMAP dans le collecteur, vérifier
   `mailcollector` dans les actions automatiques.

---

## 57. Dépannage — 7 autres cas concrets

### Cas 1 — « Impossible de se connecter » après import LDAP

- Vérifiez le DN de connexion et le mot de passe du compte de service
  (test : `ldapsearch -x -D "CN=..." -W -h ad.entreprise.lan -b "DC=..."`).
- Champ identifiant : `sAMAccountName` (AD) vs `uid` (OpenLDAP).
- L'utilisateur existe-t-il dans la base LDAP ciblée par le filtre ?

### Cas 2 — Lenteurs générales

- Activez OPcache (déjà en annexe A) ; `memory_limit` à 256M mini.
- Purgez les vieux journaux (`Administration > Maintenance > Purge`).
- Vérifiez la taille de `glpi_queuednotifications` et `glpi_logs` (tables
  géantes = requêtes lentes) ; optimisez : `mysqlcheck -o glpidb`.
- Trop d'onglets/acteurs sur les tickets ? Limitez les notifications.

### Cas 3 — Pièces jointes impossibles à téléverser

- `upload_max_filesize` / `post_max_size` (php.ini) vs taille du fichier.
- Droits d'écriture sur `files/` (`www-data`).
- Quota disque du volume.

### Cas 4 — PDF illisibles / caractères bizarres

- Vérifiez `utf8mb4` sur la base **et** la connexion (`config_db.php`).
- Après import d'un dump en latin1 : convertissez
  (`ALTER TABLE ... CONVERT TO CHARACTER SET utf8mb4`).

### Cas 5 — Sessions qui expirent sans arrêt

- `session.gc_maxlifetime` trop court ; espace `/var/lib/php/sessions` plein.
- Plusieurs onglets + bascule HTTP/HTTPS (cookie `secure`) → restez en HTTPS.

### Cas 6 — Doublons d'inventaire après réinstallation de postes

- Voir section 36 : verrous + règles de rapprochement sur n° de série.
- Fusion manuelle des doublons, puis verrouillage.

### Cas 7 — « Vous n'avez pas les droits » alors que tout semble bon

- Profil **non récursif** sur la bonne entité ? L'utilisateur a-t-il le profil
  sur **l'entité du ticket** ?
- Videz le cache des droits : déconnexion/reconnexion.
- Vérifiez les **règles d'affectation** qui auraient écrasé le profil.

---

## 58. 10 erreurs classiques (et comment les éviter)

| # | Erreur | Conséquence | Prévention |
|---|---|---|---|
| 1 | Mots de passe par défaut conservés | Compromission triviale | Section 8.2 — changer dès l'install |
| 2 | Pas de cron configuré | Aucun mail, SLA morts | Section 9 — cron dès le jour 1 |
| 3 | Dossier `install/` oublié | Réinstallation / fuite d'infos | Section 8.1 |
| 4 | URL appli fausse (http/ip) | Liens morts dans les mails | Section 8.3 |
| 5 | Tickets sans élément associé | Stats inutilisables | Gabarit obligatoire (section 18) |
| 6 | Catégories en vrac (« Autre » à 60 %) | Pilotage impossible | Taxonomie sobre + revue annuelle (16) |
| 7 | SLA calculés en heures calendaires | Engagements intenables la nuit | Calendrier heures ouvrées (21) |
| 8 | Sauvegarde jamais testée | Découverte du désastre le jour J | Test trimestriel (49) |
| 9 | Plugins installés à l'aveugle | Page blanche à la MAJ | Règles de la section 51 |
| 10 | Mots de passe notés dans les tickets | Fuite de credentials | Coffre dédié, règle d'équipe (42) |

---

# PARTIE XII — CAS PRATIQUES

## 59. Cas pratique 1 — organiser le SAV d'une entreprise de maintenance copieurs

**Contexte :** 8 techniciens, 2 agences, 350 copieurs chez 120 clients, contrats
au coût/copie et au forfait.

### Semaine 1 — fondations

1. Installer GLPI (partie I), sécuriser (partie VIII), configurer le cron.
2. Créer l'arborescence d'entités : racine → 2 agences → 5 gros clients en
   sous-entités (les autres restent au niveau agence avec un champ « client »).
3. Créer les profils : Technicien SAV, Superviseur, Déclarant client, Magasinier.
4. Importer les utilisateurs depuis l'AD ; règles d'affectation automatiques.

### Semaine 2 — référentiels

5. Catégories de tickets (section 16) + gabarit « Intervention copieur » (annexe D).
6. Créer les fiches des 350 copieurs : import CSV
   (`Administration > Maintenance > Import`) — colonnes : marque, modèle, série,
   client, compteur, contrat. **Vérifier un échantillon de 20 fiches à la main.**
7. Créer les contrats et les lier aux copieurs ; renseigner les SLA.
8. Créer les modèles de cartouches + stock initial (comptage physique).

### Semaine 3 — processus

9. Règles d'assignation automatiques (section 20).
10. Tickets récurrents : relevés de compteurs mensuels, visites préventives
    trimestrielles.
11. Modèles de notification relus et testés ; collecteur `sav@entreprise.lan`.
12. Formation : 2 h techniciens (créer/traiter un ticket), 1 h déclarants clients.

### Semaine 4 — pilote puis généralisation

13. Pilote sur 1 agence / 10 clients pendant 2 semaines.
14. Revue : catégories utilisées ? champs remplis ? SLA réalistes ?
15. Ajustements, puis généralisation. **Date butoir** : l'ancien système
    (cahier, Excel) s'arrête — pas de double saisie au-delà d'un mois.

---

## 60. Cas pratique 2 — tickets d'intervention copieurs de A à Z

**Scénario :** la mairie (gros client) signale un bourrage récurrent sur son
Kyocera TASKalfa 4054ci (bureau 204).

1. **Création** : la secrétaire appelle la hotline → ticket créé avec le gabarit
   (catégorie « Bourrage papier », élément associé = la fiche du copieur,
   compteur N&B relevé au téléphone, code affiché J-0511).
2. **Qualification (N1, < 30 min)** : la hotline consulte la base de connaissances
   → article « J-0511 bac 2 » → guide la secrétaire (retirer le papier, vérifier
   les galets). Échec → assignation au groupe `Techniciens terrain`, priorité
   calculée (urgence haute, impact moyen → priorité haute).
3. **Planification** : le superviseur planifie une tâche le lendemain 9h,
   technicien référent du client.
4. **Intervention** : sur site, diagnostic = galets d'entraînement usés.
   Remplacement (pièce sortie du stock → décrément + coût imputé au contrat).
   Suivi rédigé avec photo du compteur. Article KB lié.
5. **Résolution** : type de solution « Remplacement pièce », description complète.
   Notification à la déclarante.
6. **Validation** : la secrétaire approuve (tout fonctionne) → ticket **Clos**.
   Sondage envoyé → note 5/5.
7. **Capitalisation** : le superviseur vérifie que l'article KB est à jour
   (référence du kit galets ajoutée).

**Ce que le chef de service voit :** temps de prise en compte, respect du SLA,
coût de l'intervention, pièce consommée, satisfaction — tout est tracé.

---

## 61. Cas pratique 3 — planning des techniciens

### Outils

- `Assistance > Planning` : vue semaine par technicien/groupe des **tâches
  planifiées** des tickets + tickets récurrents.
- Chaque intervention planifiée = une **tâche** dans le ticket (date, durée,
  technicien).

### Rituel hebdomadaire (vendredi 16h, 30 min)

1. Ouvrir le planning de la semaine suivante.
2. Placer les tickets récurrents (préventif) en priorité — ce sont des
   engagements contractuels.
3. Répartir les tickets « En cours (Planifié) » selon : compétences (habilitations
   constructeur), secteur géographique (tournées optimisées), charge (équilibrage).
4. Identifier les tickets « En attente » avec relance dépassée → action.
5. Publier le planning (impression PDF ou partage d'écran en réunion).

### Règles d'équipe

- Aucune intervention sans **tâche planifiée** (sauf urgence critique).
- Le technicien pointe le **temps réel** (début/fin) dans la tâche → fiabilité
  des coûts et des SLA.
- Astreinte : un seul technicien d'astreinte, groupe `Astreinte`, téléphone
  dédié, tickets urgents uniquement.

---

## 62. Cas pratique 4 — stock pièces et consommables

### Organisation du magasin

- **Zones** : consommables courants (toners), pièces d'usure (kits, tambours),
  pièces détachées (cartes, moteurs), parc de prêt.
- Chaque référence = modèle de cartouche/consommable dans GLPI avec **seuil
  d'alerte** et **fournisseur habituel**.

### Processus

| Étape | Action GLPI |
|---|---|
| Réception livraison | Entrée de stock (+ bon de livraison en pièce jointe) |
| Intervention | Sortie depuis le ticket (quantité, imputation client/contrat) |
| Seuil atteint | Alerte auto → commande (demande d'achat = ticket interne) |
| Inventaire trimestriel | Comptage physique → régularisation motivée |
| Pièce défectueuse | Retour fournisseur (suivi dans le ticket d'origine) |

### Indicateurs

- **Taux de rupture** : nombre d'interventions retardées faute de pièce
  (objectif : < 2 %).
- **Rotation** : références dormantes depuis > 12 mois → déstockage.
- **Coût pièces par client** : renégociation des contrats déficitaires.

---

## 63. Cas pratique 5 — migration depuis Excel/ancien outil

### Méthode en 5 étapes

1. **Inventaire des données** : lister les fichiers (parc.xls, tickets.mdb...),
   colonnes, qualité (doublons ? séries manquantes ?).
2. **Nettoyage** : normaliser marques/modèles, dédupliquer, compléter les
   n° de série (un passage atelier peut être nécessaire).
3. **Correspondance** : mapper chaque colonne vers un champ GLPI
   (tableau de correspondance écrit et validé).
4. **Import** : `Administration > Maintenance > Import` (CSV) par lots de
   200 lignes, **vérification d'un échantillon après chaque lot**.
5. **Historique** : les vieux tickets se résument en 1 suivi « Historique repris
   de l'ancien système » sur la fiche du matériel — pas d'import ligne à ligne
   (trop coûteux, peu de valeur).

### Pièges

- Importer des doublons → prévoir la passe de déduplication (section 36).
- Vouloir tout importer → ne migrez que l'**actif** (matériels en service,
  contrats en cours, tickets ouverts + 12 mois d'historique).
- Oublier de **former** avant l'import → les utilisateurs remplissent mal
  dès le premier jour.

---

## 64. Cas pratique 6 — revue hebdomadaire du chef de service

**Rituel : lundi 9h, 45 minutes, avec les superviseurs.** Support : dashboard
« Pilotage SAV » (section 40) + rapport PDF.

### Ordre du jour type

1. **File active** (5 min) : tickets ouverts par ancienneté — tout ticket
   > 15 jours doit avoir une explication et un plan.
2. **SLA** (10 min) : % TTO/TTR tenus par client et par technicien ; chaque
   dépassement → cause + action corrective.
3. **Qualité** (10 min) : sondages < 3/5 → analyse ; tickets rouverts
   (résolu → nouveau) → problème de diagnostic ?
4. **Préventif** (5 min) : tickets récurrents en retard, compteurs non relevés.
5. **Stock & contrats** (10 min) : alertes stock, contrats à J-90, budgets à 80 %.
6. **Actions** (5 min) : 3 actions max, un responsable, une échéance —
   notées dans un ticket interne ou un projet GLPI.

> **Modèle de rapport** en annexe E. La régularité du rituel vaut plus que sa
> sophistication : c'est lui qui fait vivre GLPI.

---

## 65. Cas pratique 7 — intégrer les guides existants dans la base de connaissances

L'entreprise possède déjà des guides (copieurs Kyocera, onduleurs, Debian/Ubuntu,
Proxmox, langages...) : en faire des **articles KB exploitables par les
techniciens**.

### Méthode de découpage

Un guide de 1600 lignes ≠ un article KB. Découpez en **articles ciblés** :

| Guide source | Articles KB à créer (exemples) |
|---|---|
| Guide copieurs Kyocera | « U000 — imprimer le rapport », « C6000 — rupture de chauffe : diagnostic », « Remplacer le kit de fusion », « Codes d'accès maintenance par famille » |
| Guide onduleurs | « Dimensionner un onduleur 20 kVA », « Remplacer les batteries : procédure », « Alarmes courantes et conduite à tenir » |
| Guide Proxmox | « Redémarrer proprement un nœud », « Restaurer une VM depuis PBS » |
| Guide Debian/Ubuntu | « Durcir SSH : checklist », « Étendre un volume LVM » |

### Règles d'intégration

1. **Un article = un problème/une procédure** (section 24).
2. Catégorie KB = catégorie de ticket correspondante.
3. Ajoutez en bas de chaque article : « Source : Guide <nom>, section X —
   mise à jour le <date> ».
4. Liez les articles aux **fiches matériels** concernées et aux **gabarits**
   de tickets (suggestion automatique).
5. Nommez un **référent par famille d'articles** (ex. le spécialiste Kyocera
   relit les articles copieurs chaque année).

---

# PARTIE XIII — RÉFÉRENCES RAPIDES

## 66. Pense-bête de poche (une page)

**À imprimer et afficher à l'atelier.**

```
┌─ GLPI — PENSE-BÊTE ─────────────────────────────────────┐
│                                                          │
│ CRÉER UN TICKET : Assistance > Tickets > +                │
│   → catégorie, copieur (élément associé !), compteur,    │
│     code erreur, description factuelle                    │
│                                                          │
│ CYCLE : Nouveau → En cours → Résolu → Clos               │
│   En attente = TOUJOURS un motif + date de relance       │
│                                                          │
│ UN SUIVI À CHAQUE ÉTAPE (public si info client)          │
│ PHOTO du compteur / du défaut en pièce jointe            │
│                                                          │
│ SORTIE DE PIÈCE : depuis le ticket (stock décrémenté)    │
│                                                          │
│ EN ATTENTE > 7 JOURS → relancer / escalader              │
│ SLA : le compteur tourne en heures ouvrées               │
│                                                          │
│ BASE DE CONNAISSANCES : chercher AVANT d'appeler         │
│   un collègue / de se déplacer                          │
│                                                          │
│ MOTS DE PASSE : jamais dans GLPI → coffre dédié         │
│                                                          │
│ EN PANNE ? → prévenir le superviseur + noter l'heure    │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

---

## 67. Glossaire

| Terme | Définition |
|---|---|
| **API REST** | Interface permettant à des scripts d'échanger avec GLPI (tickets, parc...) |
| **CMDB** | Base de données de configuration : l'inventaire et ses liaisons (le « Parc ») |
| **Collecteur** | Boîte mail lue par GLPI qui transforme les courriels en tickets |
| **Cron** | Tâche planifiée système qui fait tourner les actions automatiques de GLPI |
| **Déclarant (post-only)** | Utilisateur qui crée/suit ses tickets, sans voir le reste |
| **Entité** | Découpage organisationnel (société, agence, client) |
| **FusionInventory** | Plugin + agent d'inventaire automatique (matériel, réseau, SNMP) |
| **Gabarit** | Modèle de ticket définissant champs visibles/obligatoires par catégorie |
| **Impact** | Étendue réelle d'un incident (1-mineur → 4-critique) |
| **ITIL** | Référentiel de bonnes pratiques ITSM |
| **ITSM** | Gestion des services informatiques |
| **Priorité** | Calculée depuis urgence × impact (matrice paramétrable) |
| **Profil** | Ensemble d'habilitations (technicien, superviseur, admin...) |
| **SLA** | Engagement de niveau de service (délais de prise en compte/résolution) |
| **SNMP** | Protocole d'interrogation des équipements réseau (compteurs, toner...) |
| **Suivi** | Message horodaté dans un ticket (public ou privé) |
| **Ticket récurrent** | Ticket généré automatiquement (maintenance préventive) |
| **TTO / TTR** | Délai de prise en compte / de résolution |
| **Urgence** | Criticité ressentie par le demandeur |
| **2FA / TOTP** | Double authentification par code temporaire |

---

## 68. Quiz — 10 questions + réponses

**Q1. Que se passe-t-il si le cron système de GLPI ne tourne pas ?**
R : Aucun courriel ne part (file `queuednotification` bloquée), les SLA ne sont
pas calculés, le collecteur mail ne relève rien, les tickets récurrents ne sont
pas créés. C'est la cause n°1 des « GLPI ne fait rien tout seul ».

**Q2. Quelle est la différence entre les statuts « Résolu » et « Clos » ?**
R : « Résolu » = le technicien propose une solution, en attente de validation du
déclarant. « Clos » = le déclarant a validé (ou clôture automatique après le délai
paramétré). On ne clôt jamais sans validation.

**Q3. Pourquoi le statut « En attente » exige-t-il toujours un motif et une
date de relance ?**
R : Parce qu'il suspend le compteur SLA : sans motif, les statistiques de délais
sont faussées et le ticket peut rester bloqué indéfiniment sans que personne ne
le relance.

**Q4. Citez 4 champs indispensables d'une fiche copieur.**
R : Marque + modèle, numéro de série, compteurs (N&B/couleur + date de relevé),
contrat de maintenance lié. (Plus : localisation client, consommables compatibles.)

**Q5. Comment la priorité d'un ticket est-elle déterminée ?**
R : Par la matrice urgence × impact (paramétrable). L'urgence est fixée par le
demandeur, l'impact validé par la hotline ; la priorité affichée reflète la
réalité opérationnelle.

**Q6. À quoi servent les tickets récurrents ? Donnez deux exemples métier.**
R : À générer automatiquement les tickets de maintenance planifiée. Ex. : relevé
mensuel des compteurs copieurs ; visite préventive trimestrielle.

**Q7. Pourquoi faut-il lier chaque sortie de cartouche à un ticket ?**
R : Pour décrémenter le stock en temps réel, imputer le coût au bon client/contrat,
et calculer la consommation (donc le coût réel) par client — base des
renégociations de contrats.

**Q8. Quelles sont les 3 sauvegardes indispensables de GLPI et leur fréquence ?**
R : Le dump de la base (`mysqldump`, quotidien), les fichiers
(`/var/lib/glpi/files`, quotidien), la configuration (à chaque changement).
Le tout selon la règle 3-2-1, avec test de restauration trimestriel.

**Q9. Un plugin provoque une page blanche après installation. Que faire ?**
R : Consulter les logs (`files/_log/php-errors.log`, log Apache), puis désactiver
le plugin en renommant son dossier (`xxx` → `xxx.off`). Ne jamais installer un
plugin sans vérifier sa compatibilité avec la version exacte de GLPI.

**Q10. Pourquoi ne doit-on jamais noter un mot de passe dans un ticket ?**
R : Les tickets sont visibles par de nombreuses personnes, exportables,
sauvegardés et journalisés : c'est une fuite de credential garantie. Les secrets
vont dans un coffre dédié (KeePass/Vault), jamais dans GLPI.

---

## 69. Pour aller plus loin

### Documentation officielle

- Documentation GLPI 10 (utilisateur + installation) — à consulter pour chaque
  version exacte : les écrans évoluent entre sous-versions.
- Dépôt GitHub `glpi-project/glpi` : notes de version (lisez-les **avant**
  chaque mise à jour), matrice de compatibilité PHP/plugins.

### Approfondir l'ITSM

- Référentiel **ITIL 4** (gestion des incidents, des problèmes, des changements).
- **RGPD** : registre des traitements pour les données clients hébergées dans GLPI.

### Côté technique

- **MariaDB** : réplication pour la haute disponibilité ; `mysqltuner` pour
  le dimensionnement.
- **Supervision** : intégrer les sondes de la section 46 à votre Zabbix/Nagios.
- **API** : automatiser les relevés de compteurs (section 53) puis la
  facturation au coût/copie.
- **SSO** : SAML/OIDC si votre infrastructure l'exige (au-delà du LDAP).

### Côté métier

- Croiser ce guide avec : le **guide copieurs** (codes, maintenance),
  le **guide onduleurs** (énergies), le **guide Proxmox** (hébergement),
  le **guide Debian/Ubuntu** (serveur) — tous transposables en articles
  de la base de connaissances (section 65).
- Rejoindre la **communauté GLPI** (forum, salon de discussion) : les retours
  d'expérience SAV y sont nombreux.

---

## 70. Checklist de mise en production

- [ ] Installation LAMP/nginx + GLPI 10.x OK, assistant terminé.
- [ ] Comptes par défaut sécurisés, dossier `install/` supprimé.
- [ ] URL d'application en HTTPS renseignée.
- [ ] Cron système actif et vérifié (actions automatiques à jour).
- [ ] SMTP testé, modèles de notification relus.
- [ ] Entités, profils, groupes créés ; utilisateurs importés (LDAP).
- [ ] Catégories de tickets + gabarits (champs obligatoires).
- [ ] SLA + calendriers + niveaux d'escalade configurés.
- [ ] Fiches parc importées (échantillon vérifié), contrats liés.
- [ ] Stock cartouches/consommables initialisé (comptage physique).
- [ ] Tickets récurrents (relevés, préventif) programmés.
- [ ] Sauvegarde automatisée + **test de restauration** effectué.
- [ ] Supervision (sondes section 46) en place.
- [ ] Formation techniciens (2 h) et déclarants (1 h) réalisée.
- [ ] Pense-bête affiché à l'atelier.
- [ ] Date d'arrêt de l'ancien système fixée et communiquée.

---

# ANNEXES

## 71. Annexe A — paramètres PHP recommandés

`/etc/php/8.2/apache2/php.ini` (et `cli/php.ini` pour le cron) :

```ini
[PHP]
memory_limit = 256M
max_execution_time = 120
max_input_time = 120
max_input_vars = 5000
upload_max_filesize = 32M
post_max_size = 40M
date.timezone = Europe/Paris
display_errors = Off
log_errors = On
error_log = /var/log/php_errors.log
session.cookie_httponly = 1
session.cookie_secure = 1
session.cookie_samesite = "Lax"
session.gc_maxlifetime = 7200

[opcache]
opcache.enable = 1
opcache.memory_consumption = 128
opcache.interned_strings_buffer = 16
opcache.max_accelerated_files = 10000
opcache.validate_timestamps = 1
opcache.revalidate_freq = 60

[APCu]
apc.enabled = 1
apc.shm_size = 64M
```

---

## 72. Annexe B — configuration Apache de référence

`/etc/apache2/sites-available/glpi.conf` :

```apache
<VirtualHost *:80>
    ServerName glpi.entreprise.lan
    Redirect permanent / https://glpi.entreprise.lan/
</VirtualHost>

<VirtualHost *:443>
    ServerName glpi.entreprise.lan
    DocumentRoot /var/www/glpi/public

    SSLEngine on
    SSLCertificateFile    /etc/ssl/certs/glpi.crt
    SSLCertificateKeyFile /etc/ssl/private/glpi.key

    SetEnv GLPI_CONFIG_DIR /var/lib/glpi/config
    SetEnv GLPI_VAR_DIR /var/lib/glpi/files

    <Directory /var/www/glpi/public>
        Require all granted
        AllowOverride All
        FallbackResource /index.php
    </Directory>

    <Directory /var/www/glpi>
        Require all denied
    </Directory>
    <Directory /var/www/glpi/public>
        Require all granted
    </Directory>

    Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"
    Header always set X-Content-Type-Options "nosniff"
    Header always set X-Frame-Options "SAMEORIGIN"
    Header always set Referrer-Policy "strict-origin-when-cross-origin"

    ErrorLog  ${APACHE_LOG_DIR}/glpi_error.log
    CustomLog ${APACHE_LOG_DIR}/glpi_access.log combined
</VirtualHost>
```

---

## 73. Annexe C — configuration nginx de référence

`/etc/nginx/sites-available/glpi` (version complète) :

```nginx
server {
    listen 80;
    server_name glpi.entreprise.lan;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name glpi.entreprise.lan;
    root /var/www/glpi/public;
    index index.php;

    ssl_certificate     /etc/ssl/certs/glpi.crt;
    ssl_certificate_key /etc/ssl/private/glpi.key;
    ssl_protocols TLSv1.2 TLSv1.3;

    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-Frame-Options "SAMEORIGIN" always;

    client_max_body_size 40M;

    location / {
        try_files $uri /index.php$is_args$args;
    }

    location ~ ^/index\.php(/|$) {
        fastcgi_pass unix:/run/php/glpi.sock;
        fastcgi_split_path_info ^(.+\.php)(/.*)$;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
        fastcgi_param GLPI_CONFIG_DIR /var/lib/glpi/config;
        fastcgi_param GLPI_VAR_DIR /var/lib/glpi/files;
    }

    location ~ ^/(config|files|inc|install|tests|marketplace)/ { deny all; }
    location ~ /\. { deny all; }
}
```

---

## 74. Annexe D — modèle de gabarit « Intervention copieur »

À reproduire dans `Configuration > Gabarits > Gabarits de tickets` :

```
Gabarit : INTERVENTION COPIEUR
Catégorie par défaut : Panne matérielle > Code erreur affiché (modifiable)
Type : Incident (verrouillé)

Champs :
  [OBLIGATOIRE] Éléments associés  → aide : « Sélectionnez LE copieur concerné »
  [OBLIGATOIRE] Urgence            → aide : « Très haute = client totalement bloqué »
  [OBLIGATOIRE] Compteur N&B       → champ personnalisé (nombre)
  [OBLIGATOIRE] Compteur couleur   → champ personnalisé (nombre)
  [FACULTATIF ] Code erreur affiché → champ personnalisé (texte, ex. C-0202, J-0511)
  [OBLIGATOIRE] Titre              → aide : « Ex. : Bourrage récurrent bac 2 »
  [OBLIGATOIRE] Description        → aide : « Symptômes, fréquence, depuis quand,
                                      ce qui a déjà été tenté »
  [MASQUÉ     ] Impact             → fixé par la hotline à la qualification
  [PRÉ-REMPLI ] Source             → « Téléphone » / « Collecteur mail »

Notifications : déclarant + technicien assigné + groupe assigné
SLA suggéré   : selon l'entité (règle métier)
```

---

## 75. Annexe E — modèle de rapport hebdomadaire SAV

```markdown
# Rapport SAV — semaine 39 (22 → 26 sept. 2026)

## 1. Activité
- Tickets créés : 47 | résolus : 44 | en cours : 18 (dont 3 > 15 j)
- Répartition : pannes 28, consommables 9, demandes 7, préventif 3

## 2. SLA
- TTO tenu : 96 % (objectif 95 %) | TTR tenu : 91 % (objectif 90 %)
- Dépassements : #4821 (attente pièce), #4833 (client injoignable) → actions

## 3. Qualité
- Satisfaction : 4,6/5 (12 réponses) — 1 note 2/5 (#4815, rappel technicien fait)
- Tickets rouverts : 2

## 4. Parc & préventif
- Compteurs relevés : 118/120 (2 relances)
- Visites préventives réalisées : 3/3

## 5. Stock & contrats
- Alertes : toner TK-8345K sous seuil (commande passée)
- Contrats à J-90 : Clinique Saint-Roch (renégociation en cours)

## 6. Budget
- Pièces : 3 240 € (cumul 41 k€ / 60 k€) | Consommables : 1 120 €

## 7. Actions
- [ ] Relancer fournisseur pièces (M. Diarra, avant vendredi)
- [ ] Former N1 au diagnostic J-0511 (article KB n°42)
```

---

*Fin du guide — GLPI 10.x. Document vivant : revoyez-le à chaque mise à jour
majeure et après chaque revue annuelle des processus SAV.*
