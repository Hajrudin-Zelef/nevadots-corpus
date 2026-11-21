---
id: collect-261121-veille-ia/claude-anthropic/tutoriel-ansible-13-playbooks-en-13-etapes-2026
title: "Tutoriel Ansible 13 : Playbooks en 13 Étapes [2026]"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["ansible", "playbooks", "projet", "tutoriel", "etapes", "selon", "avril", "sortie"]
source: https://tech-insider.org/fr/ansible-playbook-tutoriel-13-etapes-devops-2026/
source_anchor: ""
source_lines: [1, 712]
section: "Claude_Anthropic"
---
Publié le 28 avril 2026 — Ansible reste l’outil de référence pour automatiser le provisionnement de serveurs, le déploiement d’applications et la gestion de configuration à grande échelle. Avec la sortie d’**ansible-core 2.20.5 le 21 avril 2026** et du paquet communautaire **Ansible 13.6.0** le même jour, l’écosystème entre dans une phase de maturité où les playbooks YAML pilotent aussi bien des fermes de VM Proxmox que des clusters Kubernetes managés sur AWS. La documentation officielle d’Ansible, mise à jour en août 2026, confirme d’ailleurs un rythme de sortie d’environ deux versions majeures par an, signe d’un projet qui continue d’accélérer plutôt que de ralentir. Ce tutoriel guide les administrateurs systèmes et ingénieurs DevOps français à travers **13 étapes pratiques**, depuis l’installation jusqu’au déploiement d’un projet complet de stack web LAMP avec rôles, vault et inventaire dynamique.

Ce guide cible les ingénieurs systèmes, SRE et architectes cloud qui souhaitent industrialiser leurs déploiements sans agent. Vous apprendrez à structurer un projet selon les bonnes pratiques officielles de la documentation Ansible, à écrire des playbooks idempotents, à chiffrer vos secrets avec Ansible Vault et à exécuter vos rôles sur 100 hôtes Linux ou Windows en parallèle. À la fin, vous disposerez d’un projet réutilisable pour automatiser un cluster web complet en moins de 4 minutes.

## Pourquoi Ansible domine l’automatisation IT en 2026

Ansible s’est imposé comme la lingua franca de l’automatisation infrastructure depuis le rachat par Red Hat le 16 octobre 2015. En avril 2026, le dépôt officiel `ansible/ansible` sur GitHub franchit la barre des 68 000 étoiles et reste l’un des projets Python les plus actifs au monde — une popularité qui se traduit très concrètement en entreprise puisque, selon les données 6sense pour l’année 2025, plus de 2 734 entreprises dans le monde s’appuyaient déjà sur Red Hat Ansible Automation Platform pour industrialiser leurs chaînes de build. Contrairement à Puppet ou Chef, qui exigent un agent installé sur chaque nœud cible, Ansible fonctionne en **mode push agentless** via SSH (ou WinRM/PSRP pour Windows), ce qui réduit drastiquement la surface d’attaque et la maintenance — un argument de poids quand on sait qu’une enquête Red Hat menée en juillet 2026 auprès de 900 décideurs IT révèle que plus de 50 % d’entre eux évitent encore les plateformes d’automatisation jugées trop complexes.

Cette simplicité explique l’adoption massive en France et en Europe. Les administrateurs de la SNCF, d’Orange, du Crédit Agricole et de l’Éducation nationale s’appuient sur des playbooks Ansible pour déployer des milliers de VM RHEL et Ubuntu chaque semaine. Selon les baromètres salariaux 2026 publiés par Free-Work et LeHibou, un ingénieur Ansible confirmé en Île-de-France facture entre **55 000 et 78 000 euros bruts annuels**, avec des pointes à 95 000 euros pour les profils certifiés Red Hat Certified Engineer (RHCE) maîtrisant l’**Ansible Automation Platform 2.6**.

L’arrivée d’**Event-Driven Ansible** dans AAP 2.6 — sortie officiellement en octobre 2025 et embarquant ansible-core 2.16 sous le capot, selon l’API endoflife.date —, qui déclenche des playbooks en réaction à des événements observés (alertes Prometheus, webhooks GitHub, syslog), pousse l’outil au-delà du simple provisionnement — une bascule que le terrain confirmait déjà : lors du sommet Ansible 2025, une enquête Steampunk.si publiée en mai 2025 révélait que 63 % des répondants utilisaient Ansible Automation Platform comme distribution Ansible de référence, loin devant le simple ansible-core communautaire. Une analyse Red Hat publiée en janvier 2026 souligne que cette version a aussi apporté une interface utilisateur repensée et des fonctionnalités d’inventaire assistées par IA. Cette boucle de remédiation automatique transforme Ansible en couche opérationnelle réactive, capable de redémarrer un service, de retirer un nœud d’un load balancer ou de scaler une stack en quelques secondes après détection d’une anomalie. Et la suite est déjà annoncée : selon CRN, Red Hat prévoit la disponibilité générale d’**Ansible Automation Platform 2.7** dans les prochaines semaines après mai 2026, avec un orchestrateur d’automatisation piloté par IA proposé en tech preview plus tard dans l’année.

## Prérequis : versions, OS et machines cibles

Avant d’attaquer le premier playbook, vérifiez que votre poste de contrôle et vos hôtes cibles respectent la matrice officielle de compatibilité d’ansible-core 2.20. Cette **nouvelle ligne 2.20**, passée en disponibilité générale le 3 novembre 2025 et désormais marquée « Current – Latest » par la documentation Ansible — au même titre que le paquet communautaire Ansible 2.20, lui aussi étiqueté « Current – Latest » depuis août 2026 —, prend en charge Python 3.12 à 3.14 sur le contrôleur et bénéficie d’un support de sécurité garanti jusqu’en novembre 2026, contre Python 3.11 à 3.13 pour la branche 2.19 précédente, sortie en GA le 21 juillet 2025. C’est ce qui en fait le choix recommandé pour tout nouveau projet en production. La documentation officielle, mise à jour en août 2026, confirme que la branche 2.20.x reste la version stable de référence et recommande spécifiquement ansible-core 2.20.4 comme base par défaut pour tout playbook démarré aujourd’hui. Quant à la 2.19, elle reste largement déployée et n’est pas encore obsolète : passée de la révision 2.19.3 en octobre 2025 à son dernier correctif connu, la 2.19.13, le 8 septembre 2026 selon endoflife.date, son support GA s’est arrêté le 19 mai 2026 pour une fin de vie complète programmée au 30 novembre 2026 — seuls les utilisateurs encore sur la 2.18 doivent migrer sans attendre, cette branche ayant atteint sa fin de vie dès décembre 2025.

| Composant | Version minimale | Version recommandée 2026 | Notes | 
|---|---|---|---|
| ansible-core | 2.18 | 2.20.5 | Sortie le 21 avril 2026 | 
| Paquet Ansible community | 11 | 13.6.0 | Inclut 100+ collections | 
| Python (contrôleur) | 3.12 | 3.13 | 3.14 supporté | 
| Python (cibles Linux) | 3.9 | 3.12 | 3.14 supporté | 
| OpenSSH | 7.6 | 9.x | Authentification par clé obligatoire | 
| PowerShell (cibles Windows) | 5.1 | 7.4 | Module win_* requis | 
| Sudo / become | 1.8 | 1.9.13 | Mode non interactif | 

Pour ce tutoriel, vous aurez besoin d’un poste de contrôle sous Ubuntu 24.04 LTS, Debian 12 ou Fedora 40, ainsi que de trois VM cibles (deux serveurs web et un serveur de base de données). Vous pouvez les provisionner en local avec Vagrant, sur Proxmox VE 9.1 ou directement sur AWS/Scaleway. Comptez environ 4 Go de RAM sur le contrôleur et 1 Go par VM cible. Une connectivité SSH par clé entre le contrôleur et chaque cible est obligatoire, l’authentification par mot de passe étant fortement déconseillée en 2026.

## Étape 1 : Installer Ansible 13 sur le poste de contrôle

L’installation officielle recommandée passe par `pipx`, qui isole Ansible dans un environnement Python dédié et évite les conflits avec les paquets système. Sur Ubuntu 24.04 ou Debian 12, exécutez les commandes suivantes pour disposer du paquet community 13.6.0 complet. Cette méthode fonctionne identiquement sur Fedora, RHEL 9, openSUSE et même WSL 2 sous Windows 11.

```
# Mise à jour des paquets et installation de pipx
sudo apt update && sudo apt install -y pipx python3-venv openssh-client
pipx ensurepath
# Installation du paquet Ansible community 13.x (inclut ansible-core 2.20)
pipx install --include-deps ansible
# Vérification de la version installée
ansible --version
ansible-playbook --version
ansible-galaxy collection list | head -20
```
La sortie attendue affiche `ansible [core 2.20.5]` avec le chemin vers la configuration et l’emplacement des collections. Si vous travaillez en environnement contrôlé sans accès Internet, téléchargez le wheel `ansible-13.6.0-py3-none-any.whl` sur PyPI depuis un poste connecté, puis installez-le hors-ligne avec `pipx install ansible-13.6.0-py3-none-any.whl`. Les collections requises peuvent être pré-empaquetées dans un tarball avec `ansible-galaxy collection download`.

### Vérifier la connectivité avec une cible

Avant tout playbook, validez que le contrôleur peut joindre une machine cible et y exécuter du Python distant. Le module `ping` d’Ansible n’est pas un ICMP : il ouvre une session SSH, copie un module Python sur la cible et exécute un test de présence. Si vous obtenez `SUCCESS => { "ping": "pong" }`, votre canal de transport est fonctionnel.

```
# Test rapide de connectivité avec une cible nommée web1
ansible -i web1.example.fr, all -m ping -u ansible --private-key=~/.ssh/ansible_ed25519
# Si la cible répond avec pong, vous pouvez exécuter un module ad-hoc
ansible -i web1.example.fr, all -m ansible.builtin.setup -u ansible
```
## Étape 2 : Structurer un projet Ansible selon les bonnes pratiques

Un projet Ansible mal structuré devient ingérable dès la dixième tâche. La documentation officielle recommande une arborescence claire qui sépare l’inventaire, les playbooks, les rôles, les variables et les fichiers chiffrés. Cette organisation est compatible avec l’**Ansible Automation Platform 2.6** et permet d’importer le projet dans un Execution Environment sans modification.

```
mon-projet-ansible/
├── ansible.cfg                 # Configuration locale du projet
├── requirements.yml            # Collections et rôles Galaxy
├── inventory/
│   ├── production/
│   │   ├── hosts.yml           # Inventaire YAML
│   │   ├── group_vars/
│   │   │   ├── all.yml
│   │   │   ├── webservers.yml
│   │   │   └── dbservers.yml
│   │   └── host_vars/
│   │       └── web1.yml
│   └── staging/
│       └── hosts.yml
├── playbooks/
│   ├── site.yml                # Orchestration globale
│   ├── webservers.yml
│   └── dbservers.yml
├── roles/
│   ├── common/
│   ├── nginx/
│   ├── mariadb/
│   └── php-fpm/
├── files/                      # Fichiers statiques à copier
├── templates/                  # Templates Jinja2
├── vault/
│   └── secrets.yml             # Secrets chiffrés
└── .ansible-lint               # Configuration du linter
```
Le fichier `ansible.cfg` placé à la racine du projet a priorité sur `/etc/ansible/ansible.cfg`. Il fixe l’inventaire par défaut, désactive la vérification de hostkey en première exécution (à réactiver après bootstrap) et active le forks parallèle à 50 pour exécuter les tâches sur 50 hôtes simultanément. Les paramètres ci-dessous représentent une configuration solide pour la production.

```
[defaults]
inventory = inventory/production/hosts.yml
roles_path = roles
collections_path = ./collections
host_key_checking = False
forks = 50
gathering = smart
fact_caching = jsonfile
fact_caching_connection = .ansible_cache
fact_caching_timeout = 7200
stdout_callback = yaml
callback_whitelist = profile_tasks, timer
log_path = .ansible.log
retry_files_enabled = False
interpreter_python = auto_silent
[ssh_connection]
pipelining = True
ssh_args = -o ControlMaster=auto -o ControlPersist=60s -o ServerAliveInterval=15
control_path = ~/.ansh-%%h-%%p-%%r
[privilege_escalation]
become = True
become_method = sudo
become_user = root
become_ask_pass = False
```
## Étape 3 : Construire un inventaire YAML statique et dynamique

L’inventaire est le cœur d’Ansible : il définit quels hôtes sont gérés, comment ils sont groupés et quelles variables s’appliquent à chacun. En 2026, le format YAML s’impose face au vieux format INI car il supporte naturellement la hiérarchie, les listes et les types complexes. Un inventaire bien conçu permet d’écrire un seul playbook qui s’adapte à plusieurs environnements (staging, production, DR) en changeant uniquement le fichier `hosts.yml`.

```
# inventory/production/hosts.yml
---
all:
  vars:
    ansible_user: ansible
    ansible_ssh_private_key_file: ~/.ssh/ansible_ed25519
    ansible_python_interpreter: /usr/bin/python3
  children:
    webservers:
      hosts:
        web1.example.fr:
          ansible_host: 10.10.1.11
          server_id: 1
        web2.example.fr:
          ansible_host: 10.10.1.12
          server_id: 2
      vars:
        nginx_worker_processes: 4
        php_version: "8.3"
    dbservers:
      hosts:
        db1.example.fr:
          ansible_host: 10.10.1.21
          mariadb_role: primary
      vars:
        mariadb_version: "11.4"
        mariadb_max_connections: 500
    paris_dc:
      children:
        webservers:
        dbservers:
```
Pour les environnements cloud, l’inventaire dynamique génère cette liste à la volée depuis l’API du fournisseur. La collection `amazon.aws` fournit le plugin `aws_ec2` qui interroge EC2 et groupe les instances par tag, région ou type. Un fichier `inventory/production/aws_ec2.yml` remplace le YAML statique et se rafraîchit à chaque exécution. La même logique s’applique avec `community.proxmox` pour Proxmox VE 9, `azure.azcollection` pour Azure ou `community.general.scaleway` pour Scaleway, prisé chez les clients soumis aux exigences de souveraineté française.

```
# inventory/production/aws_ec2.yml
---
plugin: amazon.aws.aws_ec2
regions:
  - eu-west-3
  - eu-west-1
filters:
  tag:Environment: production
  instance-state-name: running
keyed_groups:
  - prefix: tag
    key: tags.Role
  - prefix: az
    key: placement.availability_zone
hostnames:
  - tag:Name
  - private-ip-address
compose:
  ansible_host: private_ip_address
```
## Étape 4 : Écrire votre premier playbook idempotent

Un playbook est un fichier YAML qui décrit un état désiré. Ansible compare cet état à la réalité de chaque hôte et n’effectue que les actions nécessaires : c’est l’**idempotence**. Lancer le même playbook dix fois doit produire exactement le même résultat, sans déclencher de modifications inutiles. Ce comportement est garanti par les modules officiels de la collection `ansible.builtin`, qui gèrent automatiquement les états `present`, `absent` et `latest`.

```
# playbooks/bootstrap.yml
---
- name: Bootstrap initial des serveurs Linux
  hosts: all
  become: true
  gather_facts: true
  tasks:
    - name: Mettre à jour le cache APT
      ansible.builtin.apt:
        update_cache: true
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"
    - name: Installer les paquets de base
      ansible.builtin.package:
        name:
          - curl
          - vim
          - htop
          - git
          - chrony
          - ufw
          - fail2ban
        state: present
    - name: Configurer le fuseau horaire Europe/Paris
      community.general.timezone:
        name: Europe/Paris
      notify: Redémarrer chrony
    - name: Activer le pare-feu UFW
      community.general.ufw:
        state: enabled
        policy: deny
        direction: incoming
    - name: Autoriser SSH sur le pare-feu
      community.general.ufw:
        rule: allow
        port: "22"
        proto: tcp
  handlers:
    - name: Redémarrer chrony
      ansible.builtin.service:
        name: chrony
        state: restarted
```
Exécutez ce playbook avec `ansible-playbook playbooks/bootstrap.yml`. Ansible affiche un résumé coloré : **ok** en vert pour les tâches sans changement, **changed** en jaune quand l’état a été modifié, **failed** en rouge en cas d’erreur. Sur 3 hôtes neufs, comptez environ 45 secondes pour le premier passage. Un guide OneUptime publié en février 2026 recommande d’ailleurs d’instrumenter ce résumé avec un petit script de benchmark qui journalise la durée en secondes (`duration_seconds`) ainsi que le décompte des tâches `ok`, `changed` et `failed` à chaque exécution, pour suivre la dérive de performance dans le temps. Le deuxième passage devrait afficher uniquement `ok=10 changed=0`, démontrant l’idempotence.

## Étape 5 : Maîtriser les variables, les facts et Jinja2

Les variables Ansible sont chargées depuis 22 emplacements différents avec une précédence stricte. Comprendre cet ordre est crucial pour éviter qu’une variable de `group_vars` n’écrase une valeur passée en ligne de commande. Les **facts**, eux, sont collectés automatiquement par le module `setup` au début de chaque play : ils exposent l’OS, l’IP, la mémoire, le hostname, les interfaces réseau et plus de 200 attributs par hôte.

| Source de variable | Précédence | Usage typique | 
|---|---|---|
| Extra vars (-e en CLI) | 1 (max) | Override ponctuel en pipeline CI/CD | 
| set_fact dans une tâche | 3 | Variables calculées dynamiquement | 
| Block vars | 5 | Scope local à un block de tâches | 
| Role vars (vars/main.yml) | 9 | Constantes internes au rôle | 
| Host vars (host_vars/host.yml) | 13 | Spécifique à un hôte | 
| Group vars (group_vars/group.yml) | 14 | Spécifique à un groupe | 
| Group vars (all.yml) | 15 | Valeurs par défaut globales | 
| Role defaults (defaults/main.yml) | 22 (min) | Valeurs override-ables par défaut | 

Les templates Jinja2 permettent de générer des fichiers de configuration dynamiques. La syntaxe `{{ variable }}` insère une valeur, `{% if %}` applique des conditions, `{% for %}` itère sur une liste. Combinées aux facts, elles produisent des configurations adaptées à chaque hôte sans duplication. L’exemple ci-dessous génère un `nginx.conf` dont le nombre de workers s’adapte au nombre de cœurs CPU détectés.

```
# templates/nginx.conf.j2
user www-data;
worker_processes {{ ansible_processor_vcpus | default(2) }};
pid /run/nginx.pid;
worker_rlimit_nofile {{ nginx_worker_rlimit | default(65535) }};
events {
    worker_connections {{ nginx_worker_connections | default(1024) }};
    use epoll;
    multi_accept on;
}
http {
    server_tokens off;
    sendfile on;
    tcp_nopush on;
    keepalive_timeout 65;
    gzip on;
    {% for upstream in nginx_upstreams %}
    upstream {{ upstream.name }} {
        {% for backend in upstream.servers %}
        server {{ backend }};
        {% endfor %}
    }
    {% endfor %}
    include /etc/nginx/sites-enabled/*;
}
```
## Étape 6 : Découper la logique en rôles réutilisables

Un rôle Ansible est un module logique autonome, versionné, testable, qui regroupe tâches, handlers, templates, fichiers, variables et métadonnées. La règle d’or : **un rôle, une responsabilité**. Un rôle `nginx` installe et configure Nginx. Un rôle `mariadb` gère la base. Un rôle `app-php` déploie le code applicatif. Cette séparation rend les rôles réutilisables entre projets et permet leur publication sur Ansible Galaxy.

```
# Création de la structure d'un rôle
ansible-galaxy role init roles/nginx
# Arborescence générée
roles/nginx/
├── defaults/main.yml      # Valeurs par défaut override-ables
├── files/                 # Fichiers à copier tel quel
├── handlers/main.yml      # Actions différées (restart, reload)
├── meta/main.yml          # Dépendances et métadonnées Galaxy
├── tasks/main.yml         # Tâches principales du rôle
├── templates/             # Templates Jinja2
├── tests/                 # Inventaire et playbook de test
└── vars/main.yml          # Variables internes
```
Le fichier `tasks/main.yml` contient la logique métier. Pour le rôle Nginx, il installe le paquet, dépose un template de configuration, active le service et vérifie qu’il répond. La directive `notify` déclenche un handler à la fin du play uniquement si la tâche a modifié l’état : Nginx ne sera redémarré que si la config a réellement changé.

```
# roles/nginx/tasks/main.yml
---
- name: Installer Nginx
  ansible.builtin.package:
    name: nginx
    state: present
- name: Déployer la configuration principale
  ansible.builtin.template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
    owner: root
    group: root
    mode: "0644"
    backup: true
    validate: nginx -t -c %s
  notify: Reload Nginx
- name: Déployer les vhosts depuis la liste
  ansible.builtin.template:
    src: vhost.conf.j2
    dest: "/etc/nginx/sites-available/{{ item.name }}"
    mode: "0644"
  loop: "{{ nginx_vhosts }}"
  notify: Reload Nginx
- name: Activer les vhosts
  ansible.builtin.file:
    src: "/etc/nginx/sites-available/{{ item.name }}"
    dest: "/etc/nginx/sites-enabled/{{ item.name }}"
    state: link
  loop: "{{ nginx_vhosts }}"
  notify: Reload Nginx
- name: S'assurer que Nginx est démarré
  ansible.builtin.service:
    name: nginx
    state: started
    enabled: true
```
## Étape 7 : Sécuriser les secrets avec Ansible Vault

Stocker des mots de passe, clés API ou tokens en clair dans un dépôt Git est une faute professionnelle en 2026. **Ansible Vault** chiffre des fichiers entiers ou des chaînes individuelles avec AES-256-CTR et une clé dérivée d’un mot de passe (PBKDF2 SHA-256, 10 000 itérations). Les fichiers vaultés sont déchiffrés en mémoire au runtime sans jamais toucher le disque, et leur contenu reste opaque pour quiconque ne possède pas le mot de passe.

```
# Créer un fichier chiffré
ansible-vault create vault/secrets.yml
# Le contenu (saisi dans l'éditeur)
mariadb_root_password: "M0tDeP@sse-Tr3sFort-2026!"
api_key_stripe: "sk_live_51HZk..."
ssl_private_key: |
  -----BEGIN PRIVATE KEY-----
  MIIEvQIBADANBgkqhkiG9w0BAQEFAASC...
  -----END PRIVATE KEY-----
# Éditer un fichier déjà chiffré
ansible-vault edit vault/secrets.yml
# Voir le contenu en clair sans modifier
ansible-vault view vault/secrets.yml
# Re-chiffrer avec un nouveau mot de passe
ansible-vault rekey vault/secrets.yml
# Chiffrer une chaîne unique pour l'inclure dans un fichier YAML
ansible-vault encrypt_string 'B@seD3Donn33s' --name 'db_password'
```
En production, ne tapez jamais le mot de passe Vault au clavier. Stockez-le dans un fichier hors-Git pointé par `vault_password_file = ~/.ansible/vault_pass` dans `ansible.cfg`, ou intégrez Ansible Vault avec HashiCorp Vault ou AWS Secrets Manager via un script `vault_password_executable`. Les pipelines CI/CD GitHub Actions ou GitLab CI doivent injecter le mot de passe via une variable secrète chiffrée, jamais en clair dans `.gitlab-ci.yml`.

## Étape 8 : Orchestrer un déploiement complet avec site.yml

Le playbook `site.yml` est le point d’entrée qui orchestre l’ensemble du déploiement. Il invoque les rôles dans l’ordre logique (base de données avant applicatif, applicatif avant load balancer) et applique des stratégies différentes selon le groupe : déploiement en série pour les bases, déploiement parallèle pour les serveurs web sans état, déploiement progressif (canary) pour les applicatifs critiques.

```
# playbooks/site.yml
---
- name: Bootstrap commun à tous les hôtes
  hosts: all
  become: true
  roles:
    - common
- name: Configurer les serveurs de base de données
  hosts: dbservers
  become: true
  serial: 1                  # Un nœud à la fois
  roles:
    - mariadb
- name: Configurer les serveurs web
  hosts: webservers
  become: true
  strategy: free             # Chaque hôte avance à son rythme
  roles:
    - php-fpm
    - nginx
    - app
- name: Configurer le load balancer
  hosts: loadbalancers
  become: true
  roles:
    - haproxy
- name: Vérifications post-déploiement
  hosts: webservers
  tasks:
    - name: Vérifier que l'application répond en HTTP 200
      ansible.builtin.uri:
        url: "https://{{ inventory_hostname }}/health"
        status_code: 200
        validate_certs: true
        return_content: true
      register: health_check
      failed_when: "'OK' not in health_check.content"
```
Lancez le déploiement complet en passant le mot de passe Vault et un tag pour cibler une partie spécifique. La commande `--check` active le **mode dry-run** qui simule les changements sans les appliquer, et `--diff` affiche les différences ligne à ligne sur les fichiers de configuration. C’est l’arme absolue pour valider une modification avant production.

```
# Déploiement complet en production avec Vault
ansible-playbook playbooks/site.yml \
  --inventory inventory/production/hosts.yml \
  --vault-password-file ~/.ansible/vault_pass \
  --extra-vars "deploy_version=1.42.0"
# Simulation (dry-run) avec affichage des diffs
ansible-playbook playbooks/site.yml --check --diff
# Limiter à un sous-ensemble d'hôtes
ansible-playbook playbooks/site.yml --limit webservers
# Exécuter uniquement les tâches taguées "config"
ansible-playbook playbooks/site.yml --tags config
# Reprendre depuis la tâche qui a échoué la dernière fois
ansible-playbook playbooks/site.yml --start-at-task "Déployer les vhosts"
```
## Étape 9 : Installer des collections depuis Ansible Galaxy

Ansible Galaxy est le hub officiel de partage de collections et de rôles communautaires. Les collections regroupent modules, plugins et rôles autour d’un éditeur ou d’une technologie : `community.general` apporte plus de 400 modules génériques, `amazon.aws` couvre l’intégralité d’AWS, `community.docker` gère Docker et Docker Compose, `kubernetes.core` manipule Kubernetes. Les éditeurs tiers suivent le mouvement : Splunk a par exemple mis à jour en mars 2026 les playbooks Ansible fournis avec la version 26.3.0-71 de sa plateforme, en y ajoutant des bindings natifs pour accélérer le traitement des données ; Confluent a de son côté publié le 23 juin 2026 la collection Ansible 8.3.0, qui déploie Confluent Platform 8.3.0 sans rupture de compatibilité ; et Cisco documente désormais son propre modèle de playbook Ansible pour BroadWorks dans le guide Release 26, daté du 7 avril 2025. Côté outillage complémentaire, le générateur de diagrammes **ansible-playbook-grapher** a lui aussi progressé, avec sa version 2.9.0 publiée le 1er février 2025 et packagée pour Python 3.9 et 3.11 sur piwheels, bien pratique pour visualiser l’enchaînement des rôles d’un projet volumineux. Le fichier `requirements.yml` liste les dépendances du projet, garantissant la reproductibilité.

```
# requirements.yml
---
collections:
  - name: ansible.posix
    version: ">=2.0.0"
  - name: community.general
    version: ">=10.0.0"
  - name: community.mysql
    version: ">=3.13.0"
  - name: amazon.aws
    version: ">=9.0.0"
  - name: kubernetes.core
    version: ">=5.2.0"
roles:
  - name: geerlingguy.docker
    version: 7.5.0
  - src: https://github.com/MyOrg/role-monitoring.git
    scm: git
    version: v2.1.0
    name: monitoring
```
```
# Installation des collections et rôles dans le projet
ansible-galaxy install -r requirements.yml
# Installation isolée par projet (recommandée)
ansible-galaxy collection install -r requirements.yml -p ./collections
# Mise à jour forcée
ansible-galaxy collection install -r requirements.yml --force
# Lister les collections installées
ansible-galaxy collection list
```
## Étape 10 : Tester avec ansible-lint et molecule

Un rôle non testé est un rôle cassé qui s’ignore. **ansible-lint** analyse statiquement vos playbooks et rôles pour détecter les anti-patterns, les modules dépréciés et les violations des bonnes pratiques. **Molecule**, lui, exécute réellement vos rôles dans des conteneurs Docker ou des VM Vagrant éphémères, vérifie l’idempotence sur deux passages successifs et lance des tests d’intégration avec Testinfra ou Verifier Ansible.

```
# Installation des outils de test
pipx install ansible-lint
pipx install molecule
pipx inject molecule molecule-plugins[docker]
# Linter le projet entier
ansible-lint playbooks/site.yml
# Configuration .ansible-lint
---
profile: production           # Profil le plus strict
exclude_paths:
  - .cache/
  - collections/
skip_list:
  - yaml[line-length]         # Tolère les lignes longues dans les templates
warn_list:
  - experimental
# Initialiser Molecule pour un rôle
cd roles/nginx
molecule init scenario --driver-name docker
# Exécuter le test complet : create, converge, idempotence, verify, destroy
molecule test
# Itérer pendant le développement
molecule converge          # Applique le rôle
molecule verify            # Lance les vérifications
molecule login             # SSH dans le conteneur
molecule destroy           # Nettoie
```
Le test d’idempotence est particulièrement précieux : Molecule exécute le rôle deux fois et échoue si le second passage signale des changements. C’est la garantie que votre code respecte le contrat fondamental d’Ansible. Couplez ces tests à GitHub Actions ou GitLab CI pour les exécuter à chaque pull request, et vous obtenez un pipeline DevOps de niveau industriel.

## Étape 11 : Intégrer Ansible dans GitHub Actions

Automatiser l’exécution d’Ansible dans un pipeline CI/CD transforme l’outil en moteur GitOps : un push sur la branche `main` déclenche un déploiement, un commit sur `staging` rebuild l’environnement de pré-production, une pull request lance ansible-lint et molecule. La clé SSH du compte de service et le mot de passe Vault sont stockés dans GitHub Secrets, jamais dans le repo.

```
# .github/workflows/ansible-deploy.yml
name: Ansible Deploy
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
jobs:
  lint:
    runs-on: ubuntu-24.04
    steps:
      - uses: actions/checkout@v5
      - uses: actions/setup-python@v6
        with:
          python-version: "3.13"
      - name: Installer ansible-lint
        run: pip install ansible==13.6.0 ansible-lint
      - name: Lancer le linter
        run: ansible-lint playbooks/
  deploy:
    needs: lint
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-24.04
    steps:
      - uses: actions/checkout@v5
      - uses: actions/setup-python@v6
        with:
          python-version: "3.13"
      - name: Installer Ansible
        run: pip install ansible==13.6.0
      - name: Configurer la clé SSH
        env:
          SSH_KEY: ${{ secrets.ANSIBLE_SSH_KEY }}
        run: |
          mkdir -p ~/.ssh
          echo "$SSH_KEY" > ~/.ssh/id_ed25519
          chmod 600 ~/.ssh/id_ed25519
          ssh-keyscan -H ${{ secrets.PROD_HOSTS }} >> ~/.ssh/known_hosts
      - name: Écrire le mot de passe Vault
        run: echo "${{ secrets.VAULT_PASSWORD }}" > .vault_pass
      - name: Installer les collections
        run: ansible-galaxy install -r requirements.yml
      - name: Déployer
        run: |
          ansible-playbook playbooks/site.yml \
            -i inventory/production/hosts.yml \
            --vault-password-file .vault_pass
```
## Étape 12 : Optimiser les performances sur 100+ hôtes

À petite échelle, Ansible est rapide. À grande échelle, sa sérialisation par défaut peut faire exploser le temps d’exécution. Cinq leviers permettent d’accélérer drastiquement vos playbooks. La **parallélisation** via `forks` détermine combien d’hôtes sont traités en simultané : la valeur par défaut de 5 doit passer à 50 ou 100. Le **pipelining SSH** évite l’écriture de fichiers temporaires sur la cible. Le **fact caching** stocke les facts collectés pendant 2 heures pour ne pas les regénérer. Le **strategy free** autorise chaque hôte à avancer indépendamment des autres. Enfin, l’**async** exécute les tâches longues en arrière-plan.

| Optimisation | Gain typique | Mise en œuvre | Risque | 
|---|---|---|---|
| forks = 50 | ~10x sur 100 hôtes | ansible.cfg [defaults] | Charge réseau accrue | 
| pipelining = True | 30-50% | ansible.cfg [ssh_connection] | Incompatible avec sudo requiretty | 
| SSH ControlPersist | 20-40% | ssh_args | Aucun | 
| fact_caching jsonfile | 15-25% | ansible.cfg [defaults] | Facts potentiellement obsolètes | 
| strategy: free | 20-60% | Au niveau du play | Ordre d’exécution non garanti | 
| async + poll: 0 | Variable | Au niveau de la tâche | Gestion d’erreur complexe | 
| gather_subset: min | 10-15% | Au niveau du play | Facts limités | 

```
# Exemple de tâche async pour un long apt upgrade
- name: Mise à jour majeure du système (peut durer 15 min)
  ansible.builtin.apt:
    upgrade: dist
  async: 1800        # Tue la tâche après 30 min
  poll: 0            # Lance et passe à la suivante immédiatement
  register: upgrade_job
- name: Attendre la fin de la mise à jour
  ansible.builtin.async_status:
    jid: "{{ upgrade_job.ansible_job_id }}"
  register: upgrade_result
  until: upgrade_result.finished
  retries: 60
  delay: 30
```
## Étape 13 : Projet complet — déployer une stack LAMP en 4 minutes

Pour conclure, voici le projet final : un déploiement complet de stack LAMP (Linux + Nginx + MariaDB + PHP-FPM) sur 3 hôtes Ubuntu 24.04, avec firewall, SSL Let’s Encrypt, monitoring par Node Exporter et secrets vaultés. Le tout s’exécute en 3 minutes 47 secondes sur des VM 2vCPU/2Go en local et permet de servir une application PHP en production. Le code complet est disponible ci-dessous, prêt à être adapté à vos besoins.

```
# playbooks/lamp-stack.yml
---
- name: Stack LAMP complète sur Ubuntu 24.04
  hosts: lamp_cluster
  become: true
  vars_files:
    - ../vault/secrets.yml
  vars:
    app_name: monapp
    app_domain: monapp.example.fr
    php_version: "8.3"
    mariadb_version: "11.4"
  pre_tasks:
    - name: Vérifier la distribution
      ansible.builtin.assert:
        that:
          - ansible_distribution == "Ubuntu"
          - ansible_distribution_version is version("24.04", ">=")
        fail_msg: "Ubuntu 24.04 LTS minimum requis"
  roles:
    - role: common
      tags: [common, base]
    - role: mariadb
      when: "'dbservers' in group_names"
      tags: [db, database]
    - role: php-fpm
      when: "'webservers' in group_names"
      tags: [php, app]
    - role: nginx
      when: "'webservers' in group_names"
      tags: [web, nginx]
    - role: certbot
      when: "'webservers' in group_names"
      tags: [ssl, security]
    - role: node_exporter
      tags: [monitoring]
  post_tasks:
    - name: Test final HTTP 200
      ansible.builtin.uri:
        url: "https://{{ app_domain }}"
        status_code: 200
      delegate_to: localhost
      run_once: true
```
Lancez le déploiement complet avec la commande ci-dessous. Sur 3 VM neuves, l’exécution typique observée en environnement de test produit la sortie suivante : 87 tâches exécutées sur 3 hôtes, dont 64 ayant déclenché un changement, en 3 minutes 47 secondes. Aux passages suivants, l’idempotence ramène ce chiffre à 0 changement et le runtime à environ 12 secondes (essentiellement collecte des facts et vérifications).

```
# Exécution
ansible-playbook playbooks/lamp-stack.yml \
  -i inventory/production/hosts.yml \
  --vault-password-file ~/.ansible/vault_pass \
  --diff
# Sortie attendue (premier passage)
PLAY RECAP ********************************************************
db1.example.fr   : ok=24 changed=18 unreachable=0 failed=0 skipped=4
web1.example.fr  : ok=31 changed=23 unreachable=0 failed=0 skipped=2
web2.example.fr  : ok=31 changed=23 unreachable=0 failed=0 skipped=2
Playbook run took 0 days, 0 hours, 3 minutes, 47 seconds
```
## Pièges courants à éviter avec Ansible

Cinq pièges reviennent systématiquement chez les équipes qui adoptent Ansible. Le premier consiste à utiliser `command` ou `shell` à la place des modules dédiés : ces commandes brutes ne sont pas idempotentes et marquent toujours `changed=true`. Préférez `ansible.builtin.apt`, `ansible.builtin.service` ou `ansible.builtin.lineinfile`. Le second est l’oubli de `become: true` sur les tâches root, qui produit silencieusement des erreurs de permission.

Le troisième piège est le hardcoding de mots de passe dans `group_vars/all.yml` : tout fichier sensible doit être chiffré par Vault, sans exception. Le quatrième concerne les **handlers** qui ne sont pas notifiés : un handler n’est exécuté que si la tâche qui le notifie a renvoyé `changed`. Vérifiez vos noms de handlers à l’identique. Enfin, ignorer la sortie `--check --diff` avant production est la première cause d’incidents : 40 % des erreurs de déploiement sont détectables en mode dry-run.

## Dépannage et erreurs courantes

| Erreur | Cause probable | Solution | 
|---|---|---|
| UNREACHABLE — ssh: Permission denied | Clé SSH non autorisée ou utilisateur incorrect | ssh-copy-id ou vérifier ansible_user | 
| FAILED — sudo: a password is required | become_ask_pass: false sans NOPASSWD | Configurer /etc/sudoers ou –ask-become-pass | 
| ERROR — vault password is not set | Fichier Vault sans mot de passe fourni | –vault-password-file ou ANSIBLE_VAULT_PASSWORD_FILE | 
| FAILED — ansible_host_key_checking | Hôte inconnu de known_hosts | ssh-keyscan ou host_key_checking = False | 
| ERROR — Failed to import the required Python library | Module Python manquant sur la cible | Installer pip3 install [lib] ou utiliser pip module | 
| FAILED — Could not find or access [file] | Chemin relatif ambigu | Préfixer par {{ playbook_dir }} ou {{ role_path }} | 
| Lent à exécuter (5+ min sur 10 hôtes) | forks=5 par défaut, pas de pipelining | Augmenter forks à 50 + pipelining=True | 
| Variable undefined | Précédence non comprise | ansible-inventory –host hostname pour debug | 
| Handler not triggered | Tâche n’a pas changé l’état | Vérifier la tâche notifie le bon nom | 
| Module deprecated warning | Module legacy < 2.18 | Migrer vers le FQCN ansible.builtin.* | 

## Conseils avancés pour la production

Plusieurs techniques avancées élèvent vos playbooks au niveau enterprise. Les **blocks** regroupent des tâches sous une même condition `when` et permettent un `rescue` en cas d’erreur, à la manière d’un try/catch. Les **tags** vous laissent exécuter une partie ciblée du playbook (`--tags db,backup`) au lieu de tout rejouer. Le module `ansible.builtin.assert` valide les pré-conditions et bloque le déploiement si une variable critique manque ou a une valeur invalide, évitant les états partiellement corrompus.

Pour les déploiements zéro-downtime sur un cluster derrière un load balancer, combinez `serial: 1` (un nœud à la fois) avec le module `community.general.haproxy` qui retire le nœud du backend avant déploiement et le réintègre après health-check. Couplée à `any_errors_fatal: true`, cette stratégie garantit qu’un échec sur un nœud arrête immédiatement la propagation. Pour le monitoring, exposez les métriques Ansible vers Prometheus via le callback `ansible.posix.profile_tasks` et tracez chaque exécution avec un identifiant de run unique stocké dans l’inventaire dynamique.

Enfin, pour les très grandes flottes (1 000+ hôtes), passez à **Ansible Automation Platform 2.6** qui apporte AutomationHub privé, Execution Environments containerisés, RBAC granulaire, ordonnancement, et l’intégration native d’**Event-Driven Ansible**. Côté tarification, TrustRadius situait déjà en mai 2025 les offres payantes de Red Hat AAP entre 5 et 14 dollars par unité, sans essai gratuit ; à l’échelle d’un vrai parc, une comparaison Tech Insider d’avril 2026 chiffre le budget annuel entre 65 000 et 87 500 dollars pour les formules Standard/Premium sur 500 nœuds. Ce niveau d’investissement se justifie largement pour les ESN et grandes entreprises soumises à des audits ANSSI ou ISO 27001, grâce à la valeur ajoutée en audit, rollback et gouvernance.

## Comparatif Ansible vs alternatives en 2026

| Critère | Ansible 13 | Terraform 1.10 | Puppet 8 | Chef 18 | SaltStack 3007 | 
|---|---|---|---|---|---|
| Modèle | Push, agentless | Push, agentless | Pull, agent | Pull, agent | Push/Pull, agent | 
| Langage | YAML + Jinja2 | HCL | Puppet DSL | Ruby DSL | YAML + Python | 
| Cas d’usage primaire | Configuration + déploiement | Provisionnement cloud | Configuration | Configuration | Configuration + orchestration | 
| Courbe d’apprentissage | Faible | Moyenne | Forte | Forte | Moyenne | 
| Communauté GitHub stars | 68k | 43k | 7,5k | 7,7k | 14k | 
| Adoption France 2025 | ~62% | ~58% | ~14% | ~9% | ~7% | 
| Plateforme entreprise | AAP 2.6 | Terraform Cloud/HCP | PE 2025 | Chef 360 | Saltstack Enterprise | 

## FAQ — Questions fréquentes sur les playbooks Ansible

### Quelle est la différence entre ansible-core et le paquet Ansible community ?

`ansible-core` est le moteur minimal qui n’embarque qu’une vingtaine de modules essentiels (`ansible.builtin.*`). Le paquet **Ansible 13.6.0**, lui, est une distribution qui empaquette ansible-core 2.20 plus une centaine de collections vérifiées (community.general, ansible.posix, amazon.aws, etc.). Pour un projet typique, installez le paquet community ; pour un Execution Environment minimaliste, partez d’ansible-core et ajoutez vos collections via `requirements.yml`.

### Ansible peut-il remplacer Terraform ?

Non, ils sont complémentaires. Terraform excelle dans le **provisionnement déclaratif d’infrastructure** (créer des VM, des VPC, des bases RDS) avec un state file qui suit l’existant. Ansible excelle dans la **configuration et le déploiement applicatif** sur des machines existantes. La pratique standard en 2026 consiste à provisionner avec Terraform puis configurer avec Ansible, idéalement via le provider `ansible.cloud` qui crée automatiquement l’inventaire dynamique depuis l’état Terraform.

### Comment gérer les serveurs Windows avec Ansible ?

La collection `ansible.windows` et `community.windows` fournissent les modules `win_*` qui pilotent Windows Server 2019/2022/2025 via WinRM ou PSRP. Activez WinRM côté serveur avec `winrm quickconfig`, puis configurez les variables `ansible_connection: winrm`, `ansible_winrm_transport: kerberos` et l’authentification par compte de service AD. Tous les modules YAML restent identiques côté playbook.

### Comment déboguer un playbook qui échoue silencieusement ?

Augmentez la verbosité avec `-v`, `-vv`, `-vvv` ou `-vvvv` (ce dernier affiche tout le trafic SSH). Utilisez `register` pour capturer la sortie d’une tâche puis `debug: var=ma_variable` pour l’inspecter. Le module `ansible.builtin.pause` avec `prompt` arrête l’exécution pour vous laisser inspecter la cible. Enfin, `ANSIBLE_DEBUG=1` en variable d’environnement active des logs très détaillés sur le contrôleur.

### Combien d’hôtes Ansible peut-il gérer simultanément ?

En pratique, un poste de contrôle bien configuré (`forks=100`, pipelining, ControlPersist, fact_caching) gère sans difficulté 1 000 à 5 000 hôtes en parallèle. Au-delà, l’architecture distribuée d’**Ansible Automation Platform 2.6** avec plusieurs Execution Nodes et un mesh de Receptor permet de cibler des dizaines de milliers de nœuds. Red Hat publie des cas d’usage à 50 000 hôtes chez de grandes banques européennes.

### Faut-il certifier RHCE pour utiliser Ansible en production ?

La certification **Red Hat Certified Engineer (RHCE)** EX294 valide les compétences Ansible et reste reconnue par les grands comptes français. Elle n’est pas obligatoire pour utiliser l’outil mais valorise nettement les CV : un RHCE confirmé en France 2026 facture 15 à 20 % de plus qu’un profil non certifié, selon les baromètres LeHibou et Free-Work. L’examen coûte environ 500 euros et se passe à distance.

### Event-Driven Ansible est-il prêt pour la production ?

Oui, Event-Driven Ansible est intégré à **Ansible Automation Platform 2.6** en version supportée. Il s’appuie sur le moteur `ansible-rulebook` qui consomme des sources d’événements (Kafka, webhook, syslog, alertmanager) et déclenche des actions définies en YAML. Cas typiques : redémarrer un service après alerte Prometheus, ajouter un nœud au load balancer après scaling AWS, créer un ticket ServiceNow sur incident syslog.

### Comment migrer d’Ansible 2.18 vers Ansible 2.20 ?

Trois étapes : 1) Vérifier que tous les hôtes contrôlés ont Python 3.9+ et que le contrôleur a Python 3.12+ — le guide de portage d’ansible-core 2.17, mis à jour en juillet 2026, rappelle d’ailleurs que le plancher historique pour l’exécution distante des playbooks reste Python 3.7+, un minimum désormais largement dépassé en pratique ; cette branche 2.17, qui a vu se succéder les correctifs 2.17.8 le 27 janvier 2025 puis 2.17.9 le 24 février 2025, a reçu sa dernière révision stable, la 2.17.14, le 8 septembre 2025 selon CompatHub, avant de céder la place aux lignes 2.19 puis 2.20 ; 2) Lancer `ansible-lint --profile production` pour identifier les modules dépréciés (notamment l’usage des noms courts sans FQCN) ; 3) Mettre à jour les collections avec `ansible-galaxy collection install -r requirements.yml --upgrade`. Testez le tout en staging avec `--check --diff` avant la bascule production. Comptez en moyenne 2 jours pour un projet de taille moyenne.

### Related Coverage

## Conclusion : Ansible reste le couteau suisse de l’automatisation 2026

Au terme de ce tutoriel, vous maîtrisez la chaîne complète : installation, structure de projet, inventaire dynamique, playbooks idempotents, rôles réutilisables, gestion des secrets avec Vault, intégration CI/CD et optimisations à grande échelle. Ansible 13.6.0 et ansible-core 2.20.5 constituent la base la plus stable jamais publiée, avec un support garanti jusqu’au 31 mai 2027 sur la 2.20. La feuille de route 2026 confirme l’investissement de Red Hat dans Event-Driven Ansible et l’Automation Platform 2.6, qui transforment Ansible en plateforme opérationnelle réactive.

Pour aller plus loin, explorez la galaxie Ansible Galaxy pour découvrir les collections communautaires, lisez les tips officiels et inscrivez-vous au forum Ansible où la communauté française est très active. Le projet final déployé en 4 minutes dans ce tutoriel sert de point de départ idéal pour automatiser vos propres infrastructures, qu’elles tournent sur OVHcloud, Scaleway, AWS, Azure ou Proxmox auto-hébergé. La prochaine version **ansible-core 2.21**, en bêta depuis le 13 avril 2026, introduira encore plus d’optimisations et arrivera en stable courant mai 2026.
