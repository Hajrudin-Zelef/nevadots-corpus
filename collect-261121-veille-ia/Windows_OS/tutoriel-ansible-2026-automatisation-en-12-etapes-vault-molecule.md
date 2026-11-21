---
id: collect-261121-veille-ia/windows-os/tutoriel-ansible-2026-automatisation-en-12-etapes-vault-molecule
title: "Tutoriel Ansible 2026 : Automatisation en 12 Étapes [Vault + Molecule]"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["ansible", "version", "tutoriel", "serveurs", "python", "septembre", "vault", "controle"]
source: https://tech-insider.org/fr/ansible-tutoriel-automatisation-serveurs-vault-molecule-2026/
source_anchor: ""
source_lines: [1, 1081]
section: "Windows_OS"
---
Ansible s’impose comme l’outil d’automatisation IT le plus recherché en 2026 : plus de 63 000 étoiles sur GitHub, une adoption massive dans les entreprises européennes, et une branche 2.20 qui était encore désignée comme Current-Latest sur la page de statut du Ansible Community Package en août 2026 (Ansible Documentation) — avant que l’Ansible Forum n’annonce, le 8 septembre 2026, la sortie d’ansible-core 2.21.4 et du community package 14.4.0, nouvelle ligne de publication de référence du projet. Ce tutoriel vous guide pas à pas dans la maîtrise d’Ansible, depuis l’installation jusqu’au déploiement d’une infrastructure multi-serveurs complète. Vous apprendrez à écrire des playbooks, organiser vos rôles, gérer les secrets avec Ansible Vault, et automatiser le provisionnement de serveurs en production. À la fin de ce guide, vous disposerez d’un projet fonctionnel déployant une stack LAMP sécurisée sur plusieurs serveurs.

*Dernière mise à jour : 8 septembre 2026 – ansible-core 2.21.4 et community package 14.4.0 (annoncés le 8 septembre 2026 par l’Ansible Forum), qui succèdent à la branche 2.20 restée Current-Latest jusqu’en août 2026 selon la documentation Ansible*

## Prérequis : Versions et Environnement Nécessaires

Avant de commencer ce tutoriel Ansible, assurez-vous de disposer de l’environnement suivant. Chaque composant a été testé avec les versions indiquées pour garantir la compatibilité de l’ensemble du projet.

| Composant | Version requise | Vérification | 
|---|---|---|
| Python | 3.10 ou supérieur | `python3 --version` | 
| pip | 23.0 ou supérieur | `pip3 --version` | 
| Ansible | 13.5.0 (installé à l’étape 1) | `ansible --version` | 
| Ansible Core | 2.20.4 | `ansible-core --version` | 
| SSH | OpenSSH 8.0+ | `ssh -V` | 
| Système hôte (nœud de contrôle) | Ubuntu 22.04+ / Debian 12+ / macOS | `lsb_release -a` | 
| Serveurs cibles | 2 VMs ou serveurs (Ubuntu/Debian) | Accès SSH root ou sudo | 

**Configuration matérielle minimale** : le nœud de contrôle Ansible nécessite seulement 1 Go de RAM et 1 vCPU. Les serveurs cibles doivent disposer d’au moins 512 Mo de RAM et de Python 3 installé. Pour ce tutoriel, nous utiliserons deux serveurs cibles : un serveur web et un serveur de base de données.

**Note importante** : Ansible fonctionne sans agent – il se connecte aux serveurs cibles via SSH. Aucune installation n’est requise sur les machines distantes, contrairement à Puppet ou Chef qui nécessitent un agent dédié. C’est l’un des avantages majeurs d’Ansible, particulièrement apprécié dans les environnements cloud européens soumis aux contraintes de souveraineté numérique.

## Étape 1 – Installer Ansible sur le Nœud de Contrôle

La première étape consiste à installer Ansible sur votre machine de contrôle. Ansible 13.5.0, publié le 25 mars 2026, reste la base stable utilisée dans ce tutoriel, mais le paysage des versions a continué d’évoluer depuis : ansible-core 2.20 était encore listé comme la version Current-Latest par la documentation officielle Ansible en août 2026, avant que l’Ansible Forum n’annonce, le 8 septembre 2026, la sortie d’ansible-core 2.21.4, qui prolonge désormais la ligne de publication communautaire au-delà de la branche 2.20. Red Hat continue par ailleurs de maintenir les branches précédentes pour les projets qui ne sont pas encore passés à la dernière version. L’installation via pip est la méthode recommandée car elle garantit l’accès à la version la plus récente, indépendamment des dépôts de votre distribution Linux.

```
# Créer un environnement virtuel Python dédié
python3 -m venv ~/ansible-env
source ~/ansible-env/bin/activate
# Installer Ansible 13 via pip
pip install ansible==13.5.0
# Vérifier l'installation
ansible --version
# ansible [core 2.20.4]
#   config file = None
#   configured module search path = ['~/.ansible/plugins/modules']
#   ansible python module location = ~/ansible-env/lib/python3.12/site-packages/ansible
#   executable location = ~/ansible-env/bin/ansible
#   python version = 3.12.x
# Vérifier la connectivité locale
ansible localhost -m ping
# localhost | SUCCESS => {
#     "changed": false,
#     "ping": "pong"
# }
```
**Alternative sur Ubuntu/Debian** : vous pouvez aussi utiliser le PPA officiel d’Ansible, mais la version disponible peut être en retard par rapport à pip. Pour les distributions Red Hat (RHEL, CentOS, Fedora), la plateforme Red Hat Ansible Automation Platform a continué d’évoluer tout au long de 2026 : Red Hat a expédié AAP 2.7 dès le 8 juin 2026, avant sa disponibilité générale officielle le 16 juin 2026 (Red Hat Developer), marquant la génération actuelle de la plateforme. La release de maintenance 2.7.20260824, publiée le 24 août 2026, a livré pas moins de 25 correctifs de sécurité, 16 améliorations de performance et d’exploitation, et 31 corrections de bugs selon la documentation Red Hat. Les branches précédentes restent activement maintenues : AAP 2.6 a reçu le 4 août 2026 un correctif regroupant 35 correctifs CVE ainsi que 10 améliorations de performance et de fonctionnalités (Red Hat Docs). Cette plateforme offre un support entreprise complet dans les environnements d’exécution, bien au-delà de ce que propose le simple PPA.

L’utilisation d’un environnement virtuel Python est fortement recommandée pour isoler Ansible et ses dépendances du système. Cela évite les conflits de paquets et permet de gérer facilement plusieurs versions d’Ansible sur la même machine – un point crucial dans les environnements de CI/CD où différents projets peuvent nécessiter des versions différentes.

## Étape 2 – Configurer l’Inventaire des Serveurs

L’inventaire Ansible définit les serveurs cibles et leur organisation en groupes. C’est le fichier de référence qui indique à Ansible quelles machines gérer. Vous pouvez utiliser le format INI (simple) ou YAML (structuré). Pour ce tutoriel, nous utiliserons le format YAML qui offre une meilleure lisibilité et prend en charge les structures imbriquées.

Créez un répertoire de projet et le fichier d’inventaire :

```
# Créer la structure du projet
mkdir -p ~/ansible-projet/{inventaire,playbooks,roles,group_vars,host_vars,fichiers,templates}
cd ~/ansible-projet
# Créer le fichier d'inventaire YAML
cat > inventaire/production.yml << 'EOF'
---
all:
  children:
    serveurs_web:
      hosts:
        web01:
          ansible_host: 192.168.1.10
          ansible_user: deploy
          http_port: 80
        web02:
          ansible_host: 192.168.1.11
          ansible_user: deploy
          http_port: 80
    serveurs_bdd:
      hosts:
        db01:
          ansible_host: 192.168.1.20
          ansible_user: deploy
          mysql_port: 3306
    serveurs_monitoring:
      hosts:
        monitor01:
          ansible_host: 192.168.1.30
          ansible_user: deploy
    production:
      children:
        serveurs_web:
        serveurs_bdd:
        serveurs_monitoring:
EOF
# Vérifier l'inventaire
ansible-inventory -i inventaire/production.yml --list
# Résultat attendu : liste JSON de tous les hôtes et groupes
# Tester la connectivité vers tous les serveurs
ansible all -i inventaire/production.yml -m ping
# web01 | SUCCESS => {"changed": false, "ping": "pong"}
# web02 | SUCCESS => {"changed": false, "ping": "pong"}
# db01 | SUCCESS => {"changed": false, "ping": "pong"}
# monitor01 | SUCCESS => {"changed": false, "ping": "pong"}
```
**Inventaire dynamique** : pour les environnements cloud (AWS, Azure, GCP), Ansible propose des plugins d’inventaire dynamique qui interrogent automatiquement l’API du fournisseur pour découvrir les instances. Par exemple, le plugin `amazon.aws.aws_ec2` génère l’inventaire à partir des tags EC2. Cette approche est indispensable dans les architectures auto-scalantes où les serveurs sont créés et détruits fréquemment.

Les variables d’hôte (`ansible_host`, `ansible_user`) et les variables personnalisées (`http_port`, `mysql_port`) permettent d’adapter le comportement des playbooks à chaque serveur sans modifier le code. C’est un principe fondamental d’Ansible : séparer les données de la logique d’automatisation.

## Étape 3 – Écrire le Fichier de Configuration ansible.cfg

Le fichier `ansible.cfg` centralise la configuration d’Ansible pour votre projet. Sans ce fichier, Ansible utilise les paramètres par défaut, ce qui peut entraîner des comportements inattendus, notamment la vérification des clés SSH qui bloque l’exécution sur de nouveaux serveurs. Un fichier de configuration bien pensé accélère considérablement le développement et le déploiement.

```
# Créer ansible.cfg à la racine du projet
cat > ~/ansible-projet/ansible.cfg << 'EOF'
[defaults]
# Chemin vers l'inventaire par défaut
inventory = inventaire/production.yml
# Utilisateur distant par défaut
remote_user = deploy
# Désactiver la vérification des clés SSH (dev uniquement)
host_key_checking = False
# Chemin vers les rôles
roles_path = roles
# Parallélisme : nombre de serveurs gérés simultanément
forks = 10
# Format de sortie coloré et lisible
stdout_callback = yaml
callbacks_enabled = timer, profile_tasks
# Répertoire pour les faits mis en cache
fact_caching = jsonfile
fact_caching_connection = /tmp/ansible_facts_cache
fact_caching_timeout = 3600
# Timeout de connexion SSH (secondes)
timeout = 30
# Réessayer les connexions échouées
retries = 3
[privilege_escalation]
# Utiliser sudo pour les tâches nécessitant des privilèges root
become = True
become_method = sudo
become_user = root
become_ask_pass = False
[ssh_connection]
# Optimisation SSH : réutiliser les connexions
pipelining = True
ssh_args = -o ControlMaster=auto -o ControlPersist=60s -o PreferredAuthentications=publickey
EOF
```
Le paramètre `pipelining = True` est une optimisation cruciale : il réduit le nombre d'opérations SSH nécessaires pour exécuter un module, ce qui peut accélérer l'exécution des playbooks de 30 à 50 %. Le paramètre `forks = 10` permet de gérer 10 serveurs simultanément au lieu de 5 par défaut, ce qui est particulièrement utile pour les déploiements à grande échelle.

**Sécurité** : en production, désactivez `host_key_checking = False` et utilisez plutôt un fichier `known_hosts` géré. Cette option est uniquement recommandée pour les environnements de développement et de test. De même, le paramètre `become_ask_pass = False` suppose que votre utilisateur `deploy` dispose de sudo sans mot de passe via le fichier sudoers – configurez cela sur chaque serveur cible avant de lancer vos playbooks.

## Étape 4 – Créer un Premier Playbook Ansible

Les playbooks sont le cœur d'Ansible : ils définissent les tâches à exécuter sur les serveurs cibles dans un format YAML déclaratif. Un playbook bien structuré est idempotent – il peut être exécuté plusieurs fois sans effet indésirable. Cette propriété est essentielle pour garantir la fiabilité des déploiements automatisés. Commençons par un playbook qui configure la sécurité de base d'un serveur.

```
# playbooks/securite-base.yml
---
- name: Configuration de sécurité de base des serveurs
  hosts: all
  become: true
  vars:
    utilisateur_deploy: deploy
    paquets_securite:
      - ufw
      - fail2ban
      - unattended-upgrades
      - logwatch
    ports_autorises:
      - "22"
      - "80"
      - "443"
  tasks:
    - name: Mettre à jour le cache APT et les paquets
      ansible.builtin.apt:
        update_cache: true
        upgrade: safe
        cache_valid_time: 3600
    - name: Installer les paquets de sécurité
      ansible.builtin.apt:
        name: "{{ paquets_securite }}"
        state: present
    - name: Créer l'utilisateur de déploiement
      ansible.builtin.user:
        name: "{{ utilisateur_deploy }}"
        groups: sudo
        shell: /bin/bash
        create_home: true
        state: present
    - name: Configurer l'authentification SSH par clé uniquement
      ansible.builtin.lineinfile:
        path: /etc/ssh/sshd_config
        regexp: "{{ item.regexp }}"
        line: "{{ item.line }}"
        state: present
        validate: sshd -t -f %s
      loop:
        - { regexp: '^#?PasswordAuthentication', line: 'PasswordAuthentication no' }
        - { regexp: '^#?PermitRootLogin', line: 'PermitRootLogin no' }
        - { regexp: '^#?PubkeyAuthentication', line: 'PubkeyAuthentication yes' }
      notify: Redémarrer SSH
    - name: Configurer le pare-feu UFW
      community.general.ufw:
        rule: allow
        port: "{{ item }}"
        proto: tcp
      loop: "{{ ports_autorises }}"
    - name: Activer UFW avec politique par défaut deny
      community.general.ufw:
        state: enabled
        policy: deny
        direction: incoming
    - name: Configurer Fail2Ban pour SSH
      ansible.builtin.copy:
        dest: /etc/fail2ban/jail.local
        content: |
          [sshd]
          enabled = true
          port = ssh
          filter = sshd
          logpath = /var/log/auth.log
          maxretry = 3
          bantime = 3600
          findtime = 600
        mode: '0644'
      notify: Redémarrer Fail2Ban
    - name: Activer les mises à jour automatiques de sécurité
      ansible.builtin.copy:
        dest: /etc/apt/apt.conf.d/20auto-upgrades
        content: |
          APT::Periodic::Update-Package-Lists "1";
          APT::Periodic::Unattended-Upgrade "1";
          APT::Periodic::AutocleanInterval "7";
        mode: '0644'
  handlers:
    - name: Redémarrer SSH
      ansible.builtin.service:
        name: sshd
        state: restarted
    - name: Redémarrer Fail2Ban
      ansible.builtin.service:
        name: fail2ban
        state: restarted
```
Exécutez ce playbook avec la commande suivante :

```
# Lancer le playbook en mode vérification (dry-run)
ansible-playbook playbooks/securite-base.yml --check --diff
# Exécuter réellement le playbook
ansible-playbook playbooks/securite-base.yml
# Résultat attendu :
# PLAY [Configuration de sécurité de base des serveurs] ************************
# 
# TASK [Gathering Facts] ********************************************************
# ok: [web01]
# ok: [web02]
# ok: [db01]
# ok: [monitor01]
# 
# TASK [Mettre à jour le cache APT et les paquets] ******************************
# changed: [web01]
# changed: [web02]
# changed: [db01]
# changed: [monitor01]
# 
# TASK [Installer les paquets de sécurité] **************************************
# changed: [web01]
# changed: [web02]
# changed: [db01]
# changed: [monitor01]
# 
# PLAY RECAP ********************************************************************
# web01    : ok=8  changed=6  unreachable=0  failed=0  skipped=0
# web02    : ok=8  changed=6  unreachable=0  failed=0  skipped=0
# db01     : ok=8  changed=6  unreachable=0  failed=0  skipped=0
# monitor01: ok=8  changed=6  unreachable=0  failed=0  skipped=0
```
Les **handlers** sont des tâches spéciales qui ne s'exécutent que lorsqu'elles sont notifiées par une tâche qui a effectué un changement. Dans notre exemple, SSH ne sera redémarré que si la configuration a réellement été modifiée. Cette mécanique évite les redémarrages inutiles et garantit la stabilité du système pendant les déploiements.

## Étape 5 – Organiser les Variables avec group_vars et host_vars

La gestion des variables est un aspect fondamental de tout projet Ansible professionnel. Au lieu de coder les valeurs en dur dans les playbooks, Ansible propose une hiérarchie de variables qui permet d'adapter le comportement à chaque groupe de serveurs ou à chaque hôte individuel. Les répertoires `group_vars` et `host_vars` suivent une convention de nommage stricte qui correspond aux groupes et aux hôtes définis dans l'inventaire.

```
# Variables partagées par tous les serveurs
cat > group_vars/all.yml << 'EOF'
---
# Configuration commune à tous les serveurs
ntp_serveur: "fr.pool.ntp.org"
timezone: "Europe/Paris"
locale: "fr_FR.UTF-8"
# Utilisateur de déploiement
utilisateur_deploy: deploy
cle_ssh_deploy: "ssh-ed25519 AAAA... deploy@control"
# Paquets communs à installer sur tous les serveurs
paquets_communs:
  - curl
  - wget
  - vim
  - htop
  - git
  - net-tools
  - python3-pip
# Configuration des logs
log_retention_jours: 30
rsyslog_serveur: "monitor01"
EOF
# Variables spécifiques aux serveurs web
cat > group_vars/serveurs_web.yml << 'EOF'
---
# Configuration Apache/Nginx
serveur_web: nginx
nginx_worker_processes: auto
nginx_worker_connections: 1024
nginx_keepalive_timeout: 65
# Configuration PHP
php_version: "8.3"
php_memory_limit: "256M"
php_upload_max_filesize: "64M"
php_max_execution_time: 30
# Domaines virtuels
vhosts:
  - nom: "app.exemple.fr"
    racine: "/var/www/app"
    ssl: true
  - nom: "api.exemple.fr"
    racine: "/var/www/api"
    ssl: true
EOF
# Variables spécifiques aux serveurs de base de données
cat > group_vars/serveurs_bdd.yml << 'EOF'
---
# Configuration MySQL/MariaDB
mysql_root_password: "{{ vault_mysql_root_password }}"
mysql_port: 3306
mysql_bind_address: "0.0.0.0"
mysql_max_connections: 150
mysql_innodb_buffer_pool_size: "1G"
# Bases de données à créer
mysql_bases:
  - nom: app_production
    encoding: utf8mb4
    collation: utf8mb4_unicode_ci
  - nom: app_staging
    encoding: utf8mb4
    collation: utf8mb4_unicode_ci
# Utilisateurs MySQL
mysql_utilisateurs:
  - nom: app_user
    mot_de_passe: "{{ vault_app_db_password }}"
    privileges: "app_production.*:ALL"
    hote: "192.168.1.%"
EOF
# Variables spécifiques à un hôte
cat > host_vars/web01.yml << 'EOF'
---
# web01 est le serveur principal
serveur_principal: true
nginx_worker_connections: 2048
EOF
```
La hiérarchie de priorité des variables Ansible suit un ordre précis : les variables en ligne de commande (`-e`) ont la priorité la plus haute, suivies des variables de tâche, puis des variables de bloc, de rôle, de playbook, de `host_vars`, de `group_vars`, et enfin de l'inventaire. Comprendre cette hiérarchie est essentiel pour éviter les comportements inattendus dans les projets complexes.

Notez l'utilisation de `{{ vault_mysql_root_password }}` dans les variables de base de données. Ces valeurs sensibles sont stockées de manière chiffrée avec Ansible Vault, que nous configurerons à l'étape suivante. Ne stockez jamais de mots de passe en clair dans vos fichiers de variables – c'est une règle de sécurité fondamentale que trop de projets négligent.

## Étape 6 – Sécuriser les Secrets avec Ansible Vault

Ansible Vault permet de chiffrer les données sensibles (mots de passe, clés API, certificats) directement dans votre dépôt Git. Les fichiers chiffrés sont déchiffrés automatiquement lors de l'exécution des playbooks, sans exposer les secrets en clair. Cette fonctionnalité est indispensable pour les équipes qui versionnent leur infrastructure as code tout en respectant les bonnes pratiques de sécurité.

```
# Créer un fichier de secrets chiffré
ansible-vault create group_vars/serveurs_bdd/vault.yml
# Entrez le mot de passe de chiffrement quand demandé
# Contenu du fichier vault.yml (édité dans votre éditeur par défaut)
---
vault_mysql_root_password: "MonMotDePasseSecurise2026!"
vault_app_db_password: "AppUserP@ssw0rd2026"
vault_backup_encryption_key: "cle-de-chiffrement-backup-32chars"
# Chiffrer un fichier existant
ansible-vault encrypt fichiers/certificat-ssl.pem
# Éditer un fichier chiffré
ansible-vault edit group_vars/serveurs_bdd/vault.yml
# Voir le contenu déchiffré sans modifier
ansible-vault view group_vars/serveurs_bdd/vault.yml
# Exécuter un playbook avec Vault
ansible-playbook playbooks/deploiement.yml --ask-vault-pass
# Ou utiliser un fichier de mot de passe (pour CI/CD)
echo "MonMotDePasseVault" > ~/.vault_pass
chmod 600 ~/.vault_pass
ansible-playbook playbooks/deploiement.yml --vault-password-file ~/.vault_pass
# Chiffrer une seule variable (inline)
ansible-vault encrypt_string 'MotDePasseSecret' --name 'ma_variable_secrete'
# Résultat :
# ma_variable_secrete: !vault |
#   $ANSIBLE_VAULT;1.1;AES256
#   62313365396662343062653031623164...
```
**Bonne pratique** : utilisez des identifiants Vault multiples pour séparer les secrets par environnement. Par exemple, un identifiant `dev` pour les secrets de développement et `prod` pour la production. Ajoutez la ligne `vault_password_file = ~/.vault_pass` dans votre `ansible.cfg` pour éviter de taper le mot de passe à chaque exécution. N'oubliez pas d'ajouter `.vault_pass` à votre `.gitignore` pour ne jamais committer le fichier de mot de passe en clair.

Dans les pipelines CI/CD comme GitHub Actions, stockez le mot de passe Vault en tant que secret de l'environnement et injectez-le au moment de l'exécution. Cette approche garantit que les secrets ne sont jamais exposés dans les logs de build ni dans l'historique Git.

## Étape 7 – Structurer le Code avec les Rôles Ansible

Les rôles Ansible permettent d'organiser le code en composants réutilisables et modulaires. Un rôle encapsule les tâches, les variables, les templates, les fichiers et les handlers liés à une fonctionnalité spécifique. Cette structure favorise la réutilisation du code entre projets et simplifie la maintenance à long terme. Ansible Galaxy, le dépôt communautaire, propose des milliers de rôles prêts à l'emploi – à noter que la feuille de route du projet a fixé le feature freeze de la collection 14.0 à mars 2026 (Ansible Community Documentation), un jalon à surveiller si vos rôles dépendent de modules récents.

```
# Créer la structure d'un rôle avec ansible-galaxy
ansible-galaxy role init roles/serveur_web
# Structure générée :
# roles/serveur_web/
# ├── defaults/
# │   └── main.yml       # Variables par défaut (priorité la plus basse)
# ├── files/
# │   └── ...            # Fichiers statiques à copier
# ├── handlers/
# │   └── main.yml       # Handlers (redémarrages de services, etc.)
# ├── meta/
# │   └── main.yml       # Métadonnées et dépendances du rôle
# ├── tasks/
# │   └── main.yml       # Tâches principales
# ├── templates/
# │   └── ...            # Templates Jinja2
# ├── tests/
# │   └── ...            # Tests du rôle
# └── vars/
#     └── main.yml       # Variables du rôle (priorité haute)
# Installer un rôle depuis Galaxy
ansible-galaxy role install geerlingguy.docker
# Installer des collections depuis Galaxy
ansible-galaxy collection install community.general
ansible-galaxy collection install ansible.posix
```
Voici le contenu du rôle `serveur_web` qui installe et configure Nginx avec PHP :

```
# roles/serveur_web/tasks/main.yml
---
- name: Installer Nginx
  ansible.builtin.apt:
    name: nginx
    state: present
  notify: Démarrer Nginx
- name: Installer PHP et ses extensions
  ansible.builtin.apt:
    name:
      - "php{{ php_version }}-fpm"
      - "php{{ php_version }}-mysql"
      - "php{{ php_version }}-curl"
      - "php{{ php_version }}-mbstring"
      - "php{{ php_version }}-xml"
      - "php{{ php_version }}-zip"
    state: present
- name: Configurer les virtual hosts Nginx
  ansible.builtin.template:
    src: vhost.conf.j2
    dest: "/etc/nginx/sites-available/{{ item.nom }}.conf"
    mode: '0644'
  loop: "{{ vhosts }}"
  notify: Recharger Nginx
- name: Activer les virtual hosts
  ansible.builtin.file:
    src: "/etc/nginx/sites-available/{{ item.nom }}.conf"
    dest: "/etc/nginx/sites-enabled/{{ item.nom }}.conf"
    state: link
  loop: "{{ vhosts }}"
  notify: Recharger Nginx
- name: Supprimer le site par défaut
  ansible.builtin.file:
    path: /etc/nginx/sites-enabled/default
    state: absent
  notify: Recharger Nginx
- name: Créer les répertoires des applications
  ansible.builtin.file:
    path: "{{ item.racine }}"
    state: directory
    owner: www-data
    group: www-data
    mode: '0755'
  loop: "{{ vhosts }}"
- name: Configurer PHP-FPM
  ansible.builtin.template:
    src: php-fpm-pool.conf.j2
    dest: "/etc/php/{{ php_version }}/fpm/pool.d/www.conf"
    mode: '0644'
  notify: Redémarrer PHP-FPM
# roles/serveur_web/handlers/main.yml
---
- name: Démarrer Nginx
  ansible.builtin.service:
    name: nginx
    state: started
    enabled: true
- name: Recharger Nginx
  ansible.builtin.service:
    name: nginx
    state: reloaded
- name: Redémarrer PHP-FPM
  ansible.builtin.service:
    name: "php{{ php_version }}-fpm"
    state: restarted
# roles/serveur_web/templates/vhost.conf.j2
server {
    listen {{ http_port | default(80) }};
    server_name {{ item.nom }};
    root {{ item.racine }};
    index index.php index.html;
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
    location ~ \.php$ {
        fastcgi_pass unix:/run/php/php{{ php_version }}-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }
    location ~ /\.ht {
        deny all;
    }
    access_log /var/log/nginx/{{ item.nom }}_access.log;
    error_log /var/log/nginx/{{ item.nom }}_error.log;
}
```
Les templates Jinja2 (`.j2`) permettent de générer des fichiers de configuration dynamiques en utilisant les variables Ansible. Cette approche garantit que chaque serveur reçoit une configuration adaptée à son rôle et à ses spécificités, tout en maintenant un code source unique.

## Étape 8 – Déployer une Stack LAMP Complète

Nous allons maintenant assembler les rôles et les playbooks pour déployer une stack LAMP (Linux, Apache/Nginx, MySQL, PHP) complète sur notre infrastructure. Ce playbook principal orchestre l'ensemble du déploiement en appliquant les rôles appropriés à chaque groupe de serveurs. C'est l'étape qui transforme votre code Ansible en infrastructure réelle.

```
# playbooks/deploiement-complet.yml
---
- name: Phase 1 – Sécurité et configuration de base
  hosts: all
  become: true
  roles:
    - securite-base
  tags: [securite]
- name: Phase 2 – Déployer les serveurs web
  hosts: serveurs_web
  become: true
  roles:
    - serveur_web
  tags: [web]
  
  post_tasks:
    - name: Déployer la page de test
      ansible.builtin.copy:
        dest: "{{ vhosts[0].racine }}/index.php"
        content: |
          Serveur {{ inventory_hostname }} opérationnel";
          echo "
```
PHP " . phpversion() . "

";
          echo "
Serveur : " . gethostname() . "

";
          echo "
Date : " . date('Y-m-d H:i:s') . "

";
          phpinfo();
        owner: www-data
        group: www-data
        mode: '0644'
    - name: Vérifier que Nginx répond
      ansible.builtin.uri:
        url: "http://localhost"
        status_code: 200
      register: resultat_test
      retries: 3
      delay: 5
      until: resultat_test.status == 200
- name: Phase 3 – Déployer les serveurs de base de données
  hosts: serveurs_bdd
  become: true
  
  tasks:
    - name: Installer MySQL Server
      ansible.builtin.apt:
        name:
          - mysql-server
          - python3-pymysql
        state: present
    - name: Démarrer MySQL
      ansible.builtin.service:
        name: mysql
        state: started
        enabled: true
    - name: Configurer le mot de passe root MySQL
      community.mysql.mysql_user:
        name: root
        password: "{{ mysql_root_password }}"
        login_unix_socket: /var/run/mysqld/mysqld.sock
        state: present
    - name: Créer les bases de données
      community.mysql.mysql_db:
        name: "{{ item.nom }}"
        encoding: "{{ item.encoding }}"
        collation: "{{ item.collation }}"
        state: present
        login_user: root
        login_password: "{{ mysql_root_password }}"
      loop: "{{ mysql_bases }}"
    - name: Créer les utilisateurs MySQL
      community.mysql.mysql_user:
        name: "{{ item.nom }}"
        password: "{{ item.mot_de_passe }}"
        priv: "{{ item.privileges }}"
        host: "{{ item.hote }}"
        state: present
        login_user: root
        login_password: "{{ mysql_root_password }}"
      loop: "{{ mysql_utilisateurs }}"
# Exécuter le déploiement complet
# ansible-playbook playbooks/deploiement-complet.yml
# Exécuter uniquement la partie web
# ansible-playbook playbooks/deploiement-complet.yml --tags web
# Exécuter sur un seul serveur
# ansible-playbook playbooks/deploiement-complet.yml --limit web01
L'utilisation des **tags** permet d'exécuter sélectivement des parties du playbook, ce qui est particulièrement utile pendant le développement ou pour les mises à jour ciblées. L'option `--limit` restreint l'exécution à un serveur spécifique, idéal pour les déploiements progressifs (canary deployments) où vous testez d'abord sur un seul serveur avant de déployer sur l'ensemble de la flotte.

## Étape 9 – Gérer les Conditions, Boucles et Blocs

Ansible offre des structures de contrôle puissantes qui permettent d'adapter le comportement des playbooks en fonction du contexte d'exécution. Les conditions (`when`), les boucles (`loop`) et les blocs (`block/rescue/always`) rendent vos playbooks flexibles et robustes face aux situations variées que vous rencontrerez en production.

```
# playbooks/conditions-avancees.yml
---
- name: Démonstration des structures de contrôle avancées
  hosts: all
  become: true
  tasks:
    # Condition simple : exécuter selon la distribution
    - name: Installer les paquets (Debian/Ubuntu)
      ansible.builtin.apt:
        name: "{{ paquets_communs }}"
        state: present
      when: ansible_os_family == "Debian"
    - name: Installer les paquets (RedHat/CentOS)
      ansible.builtin.dnf:
        name: "{{ paquets_communs }}"
        state: present
      when: ansible_os_family == "RedHat"
    # Condition avec variable d'inventaire
    - name: Configurer Nginx comme serveur principal
      ansible.builtin.template:
        src: nginx-principal.conf.j2
        dest: /etc/nginx/conf.d/upstream.conf
      when: serveur_principal | default(false)
    # Boucle avec index et condition
    - name: Créer les utilisateurs avec des rôles différents
      ansible.builtin.user:
        name: "{{ item.nom }}"
        groups: "{{ item.groupes }}"
        shell: "{{ item.shell | default('/bin/bash') }}"
        state: present
      loop:
        - { nom: "dev01", groupes: "developers", shell: "/bin/zsh" }
        - { nom: "ops01", groupes: "sudo,docker" }
        - { nom: "staging", groupes: "www-data" }
      when: item.nom != "staging" or environnement == "staging"
    # Bloc avec gestion d'erreur (try/catch/finally)
    - name: Déployer l'application avec rollback
      block:
        - name: Sauvegarder la version actuelle
          ansible.builtin.copy:
            src: /var/www/app/
            dest: /var/www/app_backup/
            remote_src: true
        - name: Déployer la nouvelle version
          ansible.builtin.git:
            repo: "https://github.com/monprojet/app.git"
            dest: /var/www/app
            version: "{{ version_app | default('main') }}"
            force: true
        - name: Installer les dépendances
          ansible.builtin.command:
            cmd: composer install --no-dev --optimize-autoloader
            chdir: /var/www/app
      rescue:
        - name: Rollback – Restaurer la version précédente
          ansible.builtin.copy:
            src: /var/www/app_backup/
            dest: /var/www/app/
            remote_src: true
        - name: Notifier l'échec du déploiement
          ansible.builtin.debug:
            msg: "ERREUR : Déploiement échoué sur {{ inventory_hostname }}. Rollback effectué."
      always:
        - name: Nettoyer le répertoire de backup
          ansible.builtin.file:
            path: /var/www/app_backup
            state: absent
        - name: Vérifier que l'application répond
          ansible.builtin.uri:
            url: "http://localhost/health"
            status_code: 200
          register: sante_app
          ignore_errors: true
        - name: Afficher le statut de l'application
          ansible.builtin.debug:
            msg: "Application {{ 'opérationnelle' if sante_app.status == 200 else 'EN ERREUR' }}"
```
Le pattern `block/rescue/always` est l'équivalent Ansible du try/catch/finally. Il est indispensable pour les déploiements en production où un rollback automatique doit être déclenché en cas d'échec. Ce mécanisme est nettement plus fiable que la gestion manuelle des erreurs et constitue une bonne pratique que toute équipe DevOps devrait adopter.

## Étape 10 – Créer un Pipeline CI/CD avec Ansible

L'intégration d'Ansible dans un pipeline CI/CD automatise entièrement le cycle de déploiement, de la validation du code jusqu'à la mise en production. Cette étape transforme vos playbooks en workflows automatisés déclenchés par des événements Git. Voici comment intégrer Ansible avec GitHub Actions pour un déploiement continu.

```
# .github/workflows/deploy.yml
name: Déploiement Ansible
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
jobs:
  lint:
    name: Validation du code Ansible
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Installer les dépendances
        run: |
          python3 -m pip install ansible==13.5.0 ansible-lint yamllint
      - name: Valider la syntaxe YAML
        run: yamllint -d relaxed .
      - name: Linter Ansible
        run: ansible-lint playbooks/ roles/
      - name: Vérifier la syntaxe des playbooks
        run: |
          for playbook in playbooks/*.yml; do
            ansible-playbook "$playbook" --syntax-check
          done
  deploy-staging:
    name: Déployer en staging
    needs: lint
    runs-on: ubuntu-latest
    if: github.event_name == 'push'
    environment: staging
    steps:
      - uses: actions/checkout@v4
      - name: Configurer SSH
        run: |
          mkdir -p ~/.ssh
          echo "${{ secrets.SSH_PRIVATE_KEY }}" > ~/.ssh/id_ed25519
          chmod 600 ~/.ssh/id_ed25519
          ssh-keyscan -H ${{ secrets.STAGING_HOST }} >> ~/.ssh/known_hosts
      - name: Installer Ansible
        run: pip install ansible==13.5.0
      - name: Créer le fichier Vault
        run: echo "${{ secrets.VAULT_PASSWORD }}" > ~/.vault_pass
      - name: Déployer sur staging
        run: |
          ansible-playbook playbooks/deploiement-complet.yml \
            -i inventaire/staging.yml \
            --vault-password-file ~/.vault_pass \
            --extra-vars "environnement=staging version_app=${{ github.sha }}"
      - name: Test de fumée
        run: |
          curl -sf https://staging.exemple.fr/health || exit 1
  deploy-production:
    name: Déployer en production
    needs: deploy-staging
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    environment: production
    steps:
      - uses: actions/checkout@v4
      - name: Déployer en production (canary)
        run: |
          ansible-playbook playbooks/deploiement-complet.yml \
            -i inventaire/production.yml \
            --vault-password-file ~/.vault_pass \
            --limit web01 \
            --extra-vars "environnement=production"
      - name: Valider le canary
        run: curl -sf https://app.exemple.fr/health
      - name: Déployer sur tous les serveurs
        run: |
          ansible-playbook playbooks/deploiement-complet.yml \
            -i inventaire/production.yml \
            --vault-password-file ~/.vault_pass \
            --extra-vars "environnement=production"
```
Ce pipeline implémente un déploiement canary automatisé : la nouvelle version est d'abord déployée sur un seul serveur (`web01`), validée par un test de santé, puis propagée à l'ensemble de l'infrastructure. En cas d'échec du test canary, le déploiement s'arrête automatiquement, protégeant les utilisateurs d'une version défectueuse. C'est une stratégie éprouvée utilisée par les plus grandes entreprises technologiques.

## Étape 11 – Tester les Playbooks avec Molecule

Molecule est le framework de test officiel pour les rôles Ansible. Il permet de valider que vos rôles fonctionnent correctement dans un environnement isolé (conteneur Docker ou VM) avant de les déployer en production. Tester son infrastructure as code est aussi important que tester son code applicatif – une configuration défectueuse peut provoquer des pannes catastrophiques.

```
# Installer Molecule avec le driver Docker
pip install molecule molecule-docker
# Initialiser Molecule dans un rôle existant
cd roles/serveur_web
molecule init scenario --driver-name docker
# molecule/default/molecule.yml
---
dependency:
  name: galaxy
driver:
  name: docker
platforms:
  - name: ubuntu-test
    image: ubuntu:22.04
    pre_build_image: true
    command: /sbin/init
    privileged: true
    volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup:ro
provisioner:
  name: ansible
  inventory:
    host_vars:
      ubuntu-test:
        php_version: "8.3"
        http_port: 80
        vhosts:
          - nom: "test.local"
            racine: "/var/www/test"
            ssl: false
verifier:
  name: ansible
# molecule/default/converge.yml
---
- name: Converge
  hosts: all
  become: true
  roles:
    - role: serveur_web
# molecule/default/verify.yml
---
- name: Verify
  hosts: all
  become: true
  tasks:
    - name: Vérifier que Nginx est installé
      ansible.builtin.package:
        name: nginx
        state: present
      check_mode: true
      register: nginx_check
      failed_when: nginx_check.changed
    - name: Vérifier que Nginx écoute sur le port 80
      ansible.builtin.wait_for:
        port: 80
        timeout: 10
    - name: Vérifier que le répertoire web existe
      ansible.builtin.stat:
        path: /var/www/test
      register: web_dir
      failed_when: not web_dir.stat.exists
# Exécuter les tests
molecule test
# Résultat attendu :
# --> Test matrix
# └── default
#     ├── dependency
#     ├── lint
#     ├── cleanup
#     ├── destroy
#     ├── syntax
#     ├── create
#     ├── prepare
#     ├── converge
#     ├── idempotence
#     ├── verify
#     └── destroy
# 
# PLAY RECAP ********
# ubuntu-test : ok=12  changed=0  unreachable=0  failed=0
```
Le test d'idempotence est particulièrement important : Molecule exécute le playbook une deuxième fois et vérifie qu'aucune tâche ne produit de changement. Si une tâche indique `changed` lors de la seconde exécution, cela signifie que le playbook n'est pas idempotent – un défaut qui peut entraîner des comportements imprévisibles lors de réexécutions en production.

## Étape 12 – Optimiser les Performances Ansible

À mesure que votre infrastructure grandit, les performances d'Ansible deviennent un enjeu critique. Un playbook qui prend 5 minutes sur 10 serveurs peut nécessiter 50 minutes sur 100 serveurs si les optimisations ne sont pas en place. Voici les techniques essentielles pour accélérer vos déploiements.

| Optimisation | Impact | Configuration | 
|---|---|---|
| Pipelining SSH | Réduction de 30-50 % du temps SSH | `pipelining = True` | 
| Fact caching (JSON) | Élimination du gathering sur les exécutions suivantes | `fact_caching = jsonfile` | 
| Forks (parallélisme) | Gestion de N serveurs simultanés | `forks = 20` | 
| Stratégie free | Chaque serveur avance indépendamment | `strategy: free` | 
| Mitogen (plugin) | Accélération de 1,25x à 7x | Plugin tiers, installation séparée | 
| Async tasks | Exécution en arrière-plan pour les tâches longues | `async: 300, poll: 5` | 
| gather_facts: false | Suppression de la collecte de facts inutile | Directive dans le play | 

```
# Playbook optimisé pour les déploiements à grande échelle
---
- name: Déploiement optimisé
  hosts: serveurs_web
  become: true
  strategy: free          # Chaque serveur avance à son rythme
  gather_facts: false     # Pas de gathering si les facts ne sont pas nécessaires
  serial: "30%"           # Déployer par lots de 30% des serveurs
  pre_tasks:
    - name: Collecter uniquement les facts réseau (plus rapide)
      ansible.builtin.setup:
        gather_subset:
          - network
          - hardware
  tasks:
    - name: Télécharger l'artefact (tâche longue en async)
      ansible.builtin.get_url:
        url: "https://releases.exemple.fr/app-{{ version_app }}.tar.gz"
        dest: /tmp/app.tar.gz
      async: 300
      poll: 0
      register: download_async
    - name: Pendant ce temps, préparer le répertoire
      ansible.builtin.file:
        path: /var/www/app-{{ version_app }}
        state: directory
        owner: www-data
        mode: '0755'
    - name: Attendre la fin du téléchargement
      ansible.builtin.async_status:
        jid: "{{ download_async.ansible_job_id }}"
      register: job_result
      until: job_result.finished
      retries: 30
      delay: 10
    - name: Extraire l'application
      ansible.builtin.unarchive:
        src: /tmp/app.tar.gz
        dest: /var/www/app-{{ version_app }}
        remote_src: true
    - name: Basculer le lien symbolique
      ansible.builtin.file:
        src: /var/www/app-{{ version_app }}
        dest: /var/www/app
        state: link
        force: true
      notify: Recharger Nginx
```
La directive `serial: "30%"` est cruciale pour les déploiements sans interruption : elle limite le déploiement simultané à 30 % de la flotte, garantissant que 70 % des serveurs restent disponibles à tout moment. Combinée avec la stratégie `free`, chaque serveur progresse à son rythme sans attendre les plus lents du lot.

Le pattern `async/poll` permet de lancer des tâches longues (téléchargements, compilations) en arrière-plan et de continuer avec d'autres tâches en parallèle. Cela peut diviser le temps total d'exécution par deux ou plus pour les playbooks qui incluent des opérations I/O intensives.

## 5 Pièges Courants à Éviter avec Ansible

Même les administrateurs systèmes expérimentés tombent dans ces pièges classiques d'Ansible. Connaître ces erreurs vous évitera des heures de débogage et des incidents en production.

**Piège n°1 : Oublier l'idempotence.** Utiliser le module `command` ou `shell` au lieu des modules Ansible dédiés rend vos playbooks non-idempotents. Par exemple, `command: apt install nginx` s'exécute à chaque fois, tandis que `ansible.builtin.apt: name=nginx state=present` ne fait rien si Nginx est déjà installé. Préférez toujours les modules spécialisés.

**Piège n°2 : Stocker des secrets en clair dans Git.** C'est la faille de sécurité la plus courante dans les projets Ansible. Utilisez systématiquement Ansible Vault pour chiffrer les mots de passe, clés API et certificats. Un audit de sécurité révèle souvent des fichiers `group_vars` contenant des mots de passe en clair – une pratique qui peut coûter cher en cas de fuite du dépôt.

**Piège n°3 : Négliger la gestion des erreurs.** Un playbook sans blocs `rescue` peut laisser un serveur dans un état incohérent en cas d'échec à mi-parcours. Utilisez toujours `block/rescue/always` pour les opérations critiques, et `--check` pour valider les changements avant de les appliquer.

**Piège n°4 : Utiliser `become: true` globalement.** Donner les privilèges root à toutes les tâches est une violation du principe du moindre privilège. Appliquez `become: true` uniquement aux tâches qui le nécessitent (installation de paquets, modification de fichiers système). Les tâches de copie de fichiers ou de configuration d'applications ne nécessitent généralement pas root.

**Piège n°5 : Ignorer les tags et les limites.** Exécuter un playbook entier pour une petite modification est risqué et lent. Utilisez les tags (`--tags`) pour cibler des sections spécifiques et `--limit` pour restreindre l'exécution à certains serveurs. En production, commencez toujours par un déploiement limité avant d'étendre à l'ensemble de l'infrastructure.

## Dépannage : 8+ Problèmes Fréquents et Solutions

Voici les problèmes les plus fréquemment rencontrés lors de l'utilisation d'Ansible, avec leurs solutions testées et validées.

| Problème | Cause | Solution | 
|---|---|---|
| `UNREACHABLE!` – Host unreachable | Clé SSH non configurée ou mauvais utilisateur | Vérifier `ssh -i ~/.ssh/id_ed25519 user@host` . Ajouter`ansible_ssh_private_key_file` dans l'inventaire. | 
| `Permission denied (publickey)` | L'utilisateur n'a pas de clé autorisée sur le serveur cible | Copier la clé : `ssh-copy-id user@host` . Vérifier les permissions de`~/.ssh/authorized_keys` (600). | 
| `Missing sudo password` | `become: true` sans accès sudo sans mot de passe | Ajouter `deploy ALL=(ALL) NOPASSWD:ALL` dans`/etc/sudoers.d/deploy` ou utiliser`--ask-become-pass` . | 
| `Module not found` | Collection manquante dans l'environnement | Installer la collection : `ansible-galaxy collection install community.general` . | 
| `Vault password not provided` | Fichier Vault chiffré sans mot de passe | Ajouter `--ask-vault-pass` ou configurer`vault_password_file` dans`ansible.cfg` . | 
| `changed` à chaque exécution | Module non-idempotent (shell/command) | Utiliser les modules dédiés ou ajouter `creates:` /`removes:` pour vérifier l'état. | 
| Playbook très lent | Gathering de facts complet sur chaque exécution | Activer `fact_caching` , utiliser`gather_subset` , ou`gather_facts: false` . | 
| `Jinja2 template error` | Variable non définie dans le contexte du template | Utiliser le filtre `{{ variable \| default('valeur') }}` pour définir des valeurs par défaut. | 
| `YAML syntax error` | Indentation incorrecte ou caractères spéciaux | Utiliser `yamllint` et`ansible-lint` . Les chaînes avec`:` doivent être entre guillemets. | 
| `could not find or access` un fichier template | Chemin relatif incorrect vers le template | Les templates doivent être dans `roles/NOM/templates/` ou spécifier le chemin absolu. | 

**Commandes de débogage essentielles** : utilisez `-vvv` (triple verbose) pour voir les connexions SSH détaillées, `-vvvv` pour le débogage complet incluant les scripts envoyés aux serveurs, et `--step` pour exécuter les tâches une par une avec confirmation. La commande `ansible-playbook --list-tasks` affiche toutes les tâches sans les exécuter, utile pour vérifier la logique avant un déploiement.

## Astuces Avancées pour Ansible en Production

Ces techniques avancées vous permettront d'exploiter pleinement la puissance d'Ansible dans des environnements de production exigeants. Elles sont utilisées par les équipes DevOps les plus performantes pour gérer des centaines, voire des milliers de serveurs.

**Callback plugins pour l'observabilité.** Les plugins de callback envoient les résultats d'exécution vers des systèmes externes. Le plugin `community.general.slack` notifie un canal Slack à chaque déploiement, tandis que `ara` (ARA Records Ansible) stocke l'historique complet dans une base de données consultable via une interface web. Configurez-les dans `ansible.cfg` avec `callbacks_enabled = ara_default, slack`.

**Dynamic includes vs static imports.** La différence entre `include_tasks` (dynamique) et `import_tasks` (statique) est subtile mais importante. Les imports sont résolus au parsing du playbook, ce qui permet l'utilisation de tags et de `--list-tasks`. Les includes sont résolus à l'exécution, ce qui permet l'utilisation de boucles et de conditions dynamiques. Utilisez `import` par défaut et `include` uniquement quand vous avez besoin de comportement dynamique.

**Custom modules en Python.** Quand aucun module existant ne correspond à votre besoin, vous pouvez créer un module personnalisé en Python. Placez-le dans `library/` à la racine du projet ou dans `roles/NOM/library/`. Le module reçoit les arguments en JSON et retourne un résultat structuré. Cette approche est particulièrement utile pour interagir avec des API internes ou des systèmes propriétaires.

**Ansible Navigator.** Ansible Navigator (`ansible-navigator`) est le successeur de `ansible-playbook` en ligne de commande. Il offre une interface interactive avec des vues détaillées pour chaque tâche, le support natif des execution environments (conteneurs), et une meilleure gestion des collections. Pour les utilisateurs de la plateforme Red Hat Ansible Automation Platform, il remplace progressivement les outils CLI traditionnels – d'autant plus que le patch 2.6.20260325, livré le 25 mars 2026, fait évoluer l'écosystème avec Automation Controller 4.7.19, Automation Hub 4.11.7 et Event-Driven Ansible 1.2.7.

## Structure Complète du Projet

Voici la structure finale du projet Ansible complet que nous avons construit au fil de ce tutoriel. Cette organisation suit les bonnes pratiques recommandées par la documentation officielle d'Ansible et peut être versionnée dans un dépôt Git.

```
ansible-projet/
├── ansible.cfg                          # Configuration Ansible du projet
├── inventaire/
│   ├── production.yml                   # Inventaire de production
│   └── staging.yml                      # Inventaire de staging
├── group_vars/
│   ├── all.yml                          # Variables communes
│   ├── serveurs_web.yml                 # Variables des serveurs web
│   └── serveurs_bdd/
│       ├── vars.yml                     # Variables non-sensibles
│       └── vault.yml                    # Secrets chiffrés (Vault)
├── host_vars/
│   └── web01.yml                        # Variables spécifiques à web01
├── playbooks/
│   ├── securite-base.yml                # Hardening de sécurité
│   ├── deploiement-complet.yml          # Déploiement full-stack
│   └── conditions-avancees.yml          # Structures de contrôle
├── roles/
│   ├── securite-base/
│   │   ├── tasks/main.yml
│   │   ├── handlers/main.yml
│   │   └── templates/
│   └── serveur_web/
│       ├── defaults/main.yml
│       ├── tasks/main.yml
│       ├── handlers/main.yml
│       ├── templates/
│       │   ├── vhost.conf.j2
│       │   └── php-fpm-pool.conf.j2
│       └── molecule/
│           └── default/
│               ├── molecule.yml
│               ├── converge.yml
│               └── verify.yml
├── fichiers/                            # Fichiers statiques
├── templates/                           # Templates globaux
├── .github/
│   └── workflows/
│       └── deploy.yml                   # Pipeline CI/CD
├── requirements.yml                     # Collections Galaxy requises
├── .gitignore                           # Exclure .vault_pass, *.retry
└── README.md
```
Créez un fichier `requirements.yml` pour documenter les collections et rôles Galaxy nécessaires à votre projet. Cette pratique garantit que tout développeur peut reproduire l'environnement avec un simple `ansible-galaxy install -r requirements.yml`. Incluez les versions exactes pour assurer la reproductibilité des déploiements.

### Couverture associée

## FAQ – Questions Fréquentes sur Ansible

### Ansible est-il gratuit ?

Oui, Ansible (le projet open source) est entièrement gratuit et distribué sous licence GPL v3. La version communautaire disponible via pip ou les gestionnaires de paquets Linux inclut toutes les fonctionnalités essentielles : playbooks, rôles, Vault, inventaires dynamiques et modules. Red Hat propose la plateforme Ansible Automation Platform (AAP) en version payante, qui ajoute une interface web, le contrôle d'accès par rôle, la gestion centralisée des inventaires et le support entreprise – la lignée 2.5 a par exemple reçu le correctif 2.5-22 dès le 1er mars 2025, avant qu'AAP 2.6 ne soit diffusé le 8 octobre 2025 avec des bundles x86_64 de 3,74 Go et 2,96 Go. Pour la majorité des cas d'usage, la version communautaire est amplement suffisante.

### Quelle est la différence entre Ansible et Terraform ?

Ansible et Terraform servent des objectifs complémentaires. Terraform excelle dans le provisionnement d'infrastructure (créer des VMs, des réseaux, des buckets S3), tandis qu'Ansible excelle dans la configuration de ces machines une fois créées (installer des logiciels, déployer des applications, gérer des utilisateurs). En pratique, de nombreuses équipes utilisent les deux ensemble : Terraform crée l'infrastructure, puis Ansible la configure. Ansible peut aussi provisionner de l'infrastructure cloud, mais sa force réside dans la gestion de configuration, là où Terraform gère mieux l'état déclaratif des ressources cloud.

### Ansible fonctionne-t-il sous Windows ?

Ansible ne peut pas s'exécuter nativement comme nœud de contrôle sous Windows. Le nœud de contrôle doit être sous Linux ou macOS. En revanche, Ansible peut gérer des machines Windows comme cibles, en utilisant le protocole WinRM (Windows Remote Management) au lieu de SSH. Les modules Windows dédiés (`win_package`, `win_service`, `win_feature`) permettent de gérer la quasi-totalité des fonctionnalités Windows. Sous Windows 10/11, vous pouvez utiliser WSL 2 (Windows Subsystem for Linux) pour installer Ansible dans un environnement Linux.

### Combien de serveurs Ansible peut-il gérer simultanément ?

La limite dépend principalement de la puissance du nœud de contrôle et de la bande passante réseau. Le paramètre `forks` (par défaut 5) définit le nombre de connexions SSH parallèles. En pratique, un nœud de contrôle avec 4 vCPU et 8 Go de RAM peut gérer efficacement 50 à 100 connexions simultanées avec `forks = 50`. Pour les déploiements massifs (1 000+ serveurs), utilisez la stratégie `serial` pour déployer par lots et le plugin Mitogen pour réduire la consommation de ressources. Red Hat AAP propose des architectures multi-contrôleurs pour les très grandes infrastructures – la release 2.7.20260603 de juin 2026 y associe un Automation Controller 4.8 et un Automation Hub 4.12.0, tandis qu'une image d'exécution allégée de seulement 10,98 Mo facilite le déploiement sur des nœuds de contrôle contraints en ressources.

### Comment déboguer un playbook Ansible qui échoue ?

Commencez par augmenter la verbosité avec `-v` (jusqu'à `-vvvv`). Utilisez `--check --diff` pour voir les changements sans les appliquer. Le module `debug` permet d'afficher la valeur des variables à l'exécution : `ansible.builtin.debug: var=ma_variable`. Pour isoler un problème, utilisez `--start-at-task="Nom de la tâche"` pour reprendre l'exécution à une tâche spécifique. L'outil `ansible-lint` détecte les erreurs de style et les anti-patterns avant l'exécution. Enfin, `ansible-playbook --list-tasks` affiche l'ordre d'exécution sans rien lancer.

### Comment gérer plusieurs environnements (dev, staging, production) ?

La meilleure approche est de maintenir des fichiers d'inventaire séparés pour chaque environnement (`inventaire/dev.yml`, `inventaire/staging.yml`, `inventaire/production.yml`) avec des `group_vars` spécifiques. Utilisez `-i inventaire/staging.yml` pour cibler un environnement. Les variables d'environnement (ports, URLs, credentials) sont définies dans les `group_vars` de chaque inventaire, tandis que les playbooks et rôles restent identiques. Cette séparation garantit que le même code Ansible est testé en staging avant d'être déployé en production.

### Faut-il utiliser Ansible ou Docker pour le déploiement ?

Ansible et Docker ne s'excluent pas mutuellement. Docker containerise les applications, tandis qu'Ansible orchestre l'infrastructure qui héberge ces conteneurs. En pratique, Ansible est souvent utilisé pour installer Docker sur les serveurs, configurer le réseau et le stockage, déployer les conteneurs avec `docker compose`, et gérer les mises à jour. Pour les architectures basées sur Kubernetes, Ansible peut provisionner le cluster et déployer les manifestes, bien que Helm soit souvent préféré pour cette dernière tâche.

## Conclusion et Prochaines Étapes

Ce tutoriel vous a guidé à travers les 12 étapes essentielles pour maîtriser Ansible en 2026, de l'installation à l'optimisation des performances en production. Vous disposez maintenant d'un projet complet incluant un inventaire structuré, des playbooks sécurisés avec Vault, des rôles modulaires testés avec Molecule, et un pipeline CI/CD automatisé avec GitHub Actions.

La version actuelle d'Ansible (13.5.0 avec Core 2.20.4, publiée le 25 mars 2026) offre un écosystème mature avec des milliers de collections disponibles sur Ansible Galaxy. Le projet open source reste l'un des plus actifs sur GitHub, avec une communauté qui contribue activement aux modules et aux plugins.

Pour aller plus loin, explorez les collections spécialisées pour votre fournisseur cloud (AWS, Azure, GCP), intégrez Ansible Navigator pour une expérience CLI améliorée, et envisagez Ansible Automation Platform pour les déploiements d'entreprise nécessitant une gouvernance centralisée. L'automatisation de l'infrastructure n'est plus un luxe en 2026 – c'est une nécessité pour toute équipe qui veut livrer rapidement et en toute confiance.
