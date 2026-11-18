# Guide Ansible — Automatisation système en production

> **Public** : administrateurs systèmes, chefs de service systèmes & énergies, équipes d'exploitation.
> **Angle** : pratique, production, sans agent, idempotent.
> **Versions couvertes** : Ansible Core 2.15 → 2.19 (Debian 12/13, Ubuntu 22.04/24.04).
> **Avertissement de version** : la syntaxe `ansible.builtin.*` (FQCN) est recommandée depuis Ansible 2.10+ ;
> `gather_facts` remplace l'ancien `gather_subset: all` implicite ; le plugin d'inventaire Proxmox
> s'appelle `community.general.proxmox` et nécessite la collection `community.general`.
> Vérifiez toujours la version cible avec `ansible --version` avant d'appliquer un playbook.

---

## 1. Pourquoi Ansible et où il se positionne

Ansible est un moteur d'automatisation **sans agent** (*agentless*) : le nœud de contrôle pilote les
machines gérées en SSH (Linux/Unix) ou WinRM (Windows). Aucun démon à installer, aucun port
supplémentaire à ouvrir sur les cibles.

| Outil | Agent | Langage | Idempotent | Usage typique |
|---|---|---|---|---|
| Ansible | Non (SSH) | YAML + Jinja2 | Oui (modules) | Config management, déploiements, orchestration |
| Puppet | Oui | DSL Puppet | Oui | Grands parcs, modèle pull |
| Chef | Oui | Ruby | Oui | Idem, culture dev |
| SaltStack | Oui (minion) | YAML + Python | Oui | Temps réel, event-driven |
| Terraform | Non | HCL | Oui | Provisionnement infra (complémentaire, pas concurrent) |

Ansible excelle quand on veut **déclarer l'état désiré** : « ce paquet doit être installé »,
« ce fichier doit contenir ceci », « ce service doit tourner ». On le relance sans crainte :
c'est l'**idempotence**.

## 2. Concepts fondamentaux

- **Control node** : la machine (ou le conteneur) où Ansible est installé et d'où partent les exécutions.
  Exigences : Python 3.10+, SSH client, Ansible Core.
- **Managed nodes** : les machines gérées. Exigences minimales : un serveur SSH et Python 3
  (`/usr/bin/python3`). Pas d'agent Ansible.
- **Inventory** : la liste des nœuds gérés, leurs groupes et leurs variables.
- **Module** : l'unité d'action (ex. `ansible.builtin.apt`, `ansible.builtin.copy`).
  Chaque module est **idempotent par conception** : il ne change l'état que si nécessaire.
- **Task** : un appel à un module avec des paramètres, exécuté sur un ensemble d'hôtes.
- **Play** : un ensemble de tâches appliquées à un groupe d'hôtes.
- **Playbook** : un fichier YAML contenant un ou plusieurs plays. C'est le livrable versionné en Git.
- **Idempotence** : propriété selon laquelle exécuter N fois une tâche produit le même état final
  qu'une seule exécution. C'est ce qui rend Ansible sûr en production.

```text
Control node ──SSH──> managed node 1 (web-01)
             ──SSH──> managed node 2 (web-02)
             ──SSH──> managed node 3 (db-01)
```

## 3. Architecture et fonctionnement interne

1. Ansible lit l'inventaire et construit la liste des hôtes cibles.
2. Pour chaque tâche, il génère un petit script Python (le *module*) et le transfère via SFTP/SCP.
3. Le script s'exécute sur le nœud géré avec les privilèges demandés (`become`).
4. Le résultat revient en JSON ; Ansible décide `changed` / `ok` / `failed`.
5. Les **handlers** notifiés s'exécutent en fin de play, une seule fois.

Points clés pour un sysadmin :

- Le transfert utilise SFTP par défaut (`scp_if_ssh` dans les vieilles versions).
- Le **pipelining** SSH (`pipelining = True` dans `ansible.cfg`) évite le transfert de fichier
  et accélère fortement les exécutions, mais exige `requiretty` désactivé côté sudo.
- Les **facts** sont collectés une fois par hôte (sauf `gather_facts: false`).

## 4. Installation sur Debian / Ubuntu via APT

```bash
# Debian 12/13 et Ubuntu 22.04/24.04 : paquet officiel (version parfois en retard)
sudo apt update
sudo apt install -y ansible

# Vérification
ansible --version
ansible-community --version 2>/dev/null || true
```

Le paquet Debian/Ubuntu fournit `ansible-core` + une sélection de collections.
Pour la dernière version stable, préférez **pipx** (section suivante).

Pré-requis Python sur les nœuds gérés (Debian/Ubuntu récents l'ont déjà) :

```bash
# À exécuter sur chaque nœud géré si Python manque
sudo apt update && sudo apt install -y python3
```

## 5. Installation via pipx (méthode recommandée)

`pipx` isole chaque outil Python dans son propre environnement virtuel : pas de conflit
avec le Python système, mises à jour propres.

```bash
# 1. Installer pipx
sudo apt update
sudo apt install -y pipx python3-venv
pipx ensurepath
# Rouvrir le shell ou : source ~/.bashrc

# 2. Installer Ansible
pipx install --include-deps ansible

# 3. Vérifier
ansible --version
which ansible   # ~/.local/bin/ansible

# 4. Mettre à jour plus tard
pipx upgrade ansible
```

Avantages en production : version figée par environnement, `pipx list` pour l'audit,
rollback simple (`pipx install ansible==9.5.1`).

## 6. Vérification de l'installation et versions

```bash
ansible --version
# ansible [core 2.17.3]
#   config file = /etc/ansible/ansible.cfg
#   python version = 3.12.x ...

# Lister les collections installées
ansible-galaxy collection list

# Lister les modules d'une collection
ansible-doc -l | grep -i apt

# Aide détaillée d'un module (à lire avant usage en prod)
ansible-doc ansible.builtin.apt

# Tester la connectivité vers l'inventaire
ansible all -i inventaire.ini -m ansible.builtin.ping
```

> **Avertissement de version** : `ansible` (paquet communautaire) ≠ `ansible-core`.
> Le premier embarque des dizaines de collections ; le second est le moteur seul.
> En production, partez d'`ansible-core` et n'ajoutez que les collections nécessaires.

## 7. Configuration : ansible.cfg complet et commenté

Ansible lit la configuration dans cet ordre : `ANSIBLE_CONFIG` (variable d'env) >
`./ansible.cfg` (répertoire courant) > `~/.ansible.cfg` > `/etc/ansible/ansible.cfg`.

```ini
# ~/projets/ansible/ansible.cfg — à poser à la racine du projet
[defaults]
# Inventaire par défaut du projet
inventory = ./inventaires/production.ini
# Rôles et collections versionnés avec le projet
roles_path = ./roles:./roles_tiers
collections_path = ./collections
# Utilisateur SSH par défaut (surchargé par l'inventaire si besoin)
remote_user = deploy
# Clé privée par défaut
private_key_file = ~/.ssh/id_ed25519_ansible
# Parallélisme : nombre de forks simultanés (défaut 5, trop faible)
forks = 30
# Pipelining : accélère SSH, exige sudo sans requiretty
pipelining = True
# Ne pas vérifier les clés d'hôtes inconnus en labo UNIQUEMENT
# En production : host_key_checking = True + ssh_known_hosts
host_key_checking = True
# Timeout SSH en secondes
timeout = 30
# Callback lisible (yaml) au lieu du pavé JSON une-ligne
stdout_callback = yaml
# Journaliser chaque exécution (supervision)
log_path = ./logs/ansible.log
# Désactiver le warning sur les collections (bruit en CI)
deprecation_warnings = False
# Interdire les plugins non signés ? (durcissement)
# allow_unsafe_lookups = False

[privilege_escalation]
# become: sudo par défaut, mot de passe demandé si become_ask_pass
become = True
become_method = sudo
become_user = root
become_ask_pass = False

[ssh_connection]
# Multiplexage SSH : réutilise les connexions (gros gain de vitesse)
ssh_args = -o ControlMaster=auto -o ControlPersist=60s -o ServerAliveInterval=30
# Nombre de tentatives avant échec
retries = 3

[colors]
# Couleurs lisibles même sur fond clair
changed = yellow
```

Checklist de revue d'un `ansible.cfg` en production :

- [ ] `host_key_checking = True` (sauf bootstrap initial documenté)
- [ ] `forks` adapté au parc (20–50 pour 200 nœuds, voir section 50)
- [ ] `log_path` défini et le dossier `logs/` exclu de Git via `.gitignore`
- [ ] `private_key_file` pointe vers une clé dédiée à Ansible, pas votre clé perso
- [ ] `stdout_callback = yaml` pour des logs exploitables

## 8. Arborescence projet recommandée

```text
ansible-projet/
├── ansible.cfg
├── inventaires/
│   ├── production.ini
│   ├── staging.ini
│   └── dynamique/
│       ├── proxmox.yml          # plugin d'inventaire Proxmox
│       └── aws_ec2.yml          # plugin d'inventaire AWS
├── group_vars/
│   ├── all.yml                  # variables globales
│   ├── webservers.yml
│   └── dbservers.yml
├── host_vars/
│   └── db-01.yml
├── roles/
│   └── mon_role/
│       ├── tasks/main.yml
│       ├── handlers/main.yml
│       ├── templates/
│       ├── files/
│       ├── vars/main.yml
│       ├── defaults/main.yml
│       └── meta/main.yml
├── playbooks/
│   ├── site.yml
│   ├── webservers.yml
│   └── durcissement_ssh.yml
├── collections/
│   └── requirements.yml
├── logs/                        # .gitignore
├── .gitignore
└── README.md
```

## 9. Inventaire statique au format INI

```ini
# inventaires/production.ini
[webservers]
web-01 ansible_host=192.168.10.11
web-02 ansible_host=192.168.10.12

[dbservers]
db-01 ansible_host=192.168.10.21 ansible_user=dba

[proxmox]
pve-01 ansible_host=192.168.10.2
pve-02 ansible_host=192.168.10.3

# Groupe de groupes
[production:children]
webservers
dbservers
proxmox

# Variables de groupe
[production:vars]
ansible_ssh_private_key_file=~/.ssh/id_ed25519_ansible
ntp_server=192.168.10.1
```

Vérifier l'inventaire :

```bash
ansible-inventory -i inventaires/production.ini --list
ansible-inventory -i inventaires/production.ini --graph
```

## 10. Inventaire statique au format YAML

Plus lisible dès que les variables se multiplient. Équivalent exact du INI précédent :

```yaml
# inventaires/production.yml
all:
  children:
    production:
      children:
        webservers:
          hosts:
            web-01:
              ansible_host: 192.168.10.11
            web-02:
              ansible_host: 192.168.10.12
        dbservers:
          hosts:
            db-01:
              ansible_host: 192.168.10.21
              ansible_user: dba
        proxmox:
          hosts:
            pve-01:
              ansible_host: 192.168.10.2
            pve-02:
              ansible_host: 192.168.10.3
      vars:
        ansible_ssh_private_key_file: ~/.ssh/id_ed25519_ansible
        ntp_server: 192.168.10.1
```

## 11. Groupes, groupes enfants et variables d'inventaire

Variables de connexion les plus utiles (à mettre dans l'inventaire ou `group_vars`) :

| Variable | Rôle |
|---|---|
| `ansible_host` | IP ou FQDN réel (le nom d'inventaire peut être logique) |
| `ansible_user` | Utilisateur SSH |
| `ansible_port` | Port SSH (défaut 22) |
| `ansible_ssh_private_key_file` | Clé privée dédiée |
| `ansible_become` / `ansible_become_user` | Élévation de privilèges par hôte/groupe |
| `ansible_python_interpreter` | Chemin Python cible (`/usr/bin/python3`) |
| `ansible_ssh_common_args` | Arguments SSH supplémentaires (ProxyJump…) |

Exemple bastion (hôtes derrière un jump host) :

```ini
[isoles]
srv-dmz-01 ansible_host=10.0.5.11

[isoles:vars]
ansible_ssh_common_args=-o ProxyJump=deploy@bastion.example.lan
```

## 12. Bonnes pratiques d'inventaire statique

- [ ] Un fichier par environnement (`production.ini`, `staging.ini`) — jamais de mélange.
- [ ] Les secrets **jamais** dans l'inventaire : utilisez `host_vars/` + Vault (section 46).
- [ ] Noms d'hôtes logiques (`web-01`) + `ansible_host` pour l'IP : l'IP peut changer, le rôle reste.
- [ ] Versionnez l'inventaire en Git, sauf les fichiers contenant des données sensibles.
- [ ] Validez après chaque modification : `ansible-inventory --graph` puis un `ping` ciblé.

## 13. Inventaire dynamique : plugin AWS EC2

L'inventaire dynamique interroge l'API du fournisseur à chaque exécution : les hôtes
apparaissent/disparaissent sans édition manuelle.

```yaml
# inventaires/dynamique/aws_ec2.yml
plugin: amazon.aws.aws_ec2
regions:
  - eu-west-3
# Ne prendre que les instances démarrées
filters:
  instance-state-name: running
# Construire des groupes à partir des tags
keyed_groups:
  - key: tags.Role
    prefix: role
    separator: ""
  - key: tags.Environment
    prefix: env
    separator: ""
# Variables d'hôte depuis les métadonnées AWS
hostvars_prefix: aws_
compose:
  ansible_host: public_ip_address | default(private_ip_address)
```

```bash
pipx inject ansible --include-apps boto3 botocore   # dépendances AWS
ansible-inventory -i inventaires/dynamique/aws_ec2.yml --graph
```

## 14. Inventaire dynamique : plugin Proxmox

Indispensable quand le parc virtualisé bouge souvent. Nécessite la collection
`community.general` et le module Python `proxmoxer`.

```yaml
# inventaires/dynamique/proxmox.yml
plugin: community.general.proxmox
url: https://pve-01.example.lan:8006
user: ansible@pve
password: "{{ vault_pve_api_password }}"   # Vault, jamais en clair !
validate_certs: false   # true en production avec un certificat valide
# Grouper par statut, template, type de nœud...
keyed_groups:
  - key: proxmox_status
    prefix: pve_status
  - key: proxmox_type
    prefix: pve_type
compose:
  ansible_host: proxmox_ipconfig0 | regex_replace('.*ip=([^,]+).*', '\\1')
```

```bash
ansible-galaxy collection install community.general
pipx inject ansible proxmoxer requests
ansible-inventory -i inventaires/dynamique/proxmox.yml --list | head -50
```

> **Avertissement** : `validate_certs: false` n'est acceptable qu'en labo. En production,
> déployez un certificat Let's Encrypt ou interne sur Proxmox et passez à `true`.

## 15. Inventaire dynamique : script personnalisé

Quand aucun plugin n'existe (CMDB maison, NetBox sans plugin…), un script exécutable
qui imprime du JSON `--list` fait l'affaire. Exemple minimal :

```python
#!/usr/bin/env python3
"""inventaires/dynamique/cmdb.py — inventaire depuis la CMDB interne."""
import json, sys

def main():
    inventory = {
        "webservers": {
            "hosts": ["web-01", "web-02"],
            "vars": {"http_port": 80},
        },
        "_meta": {
            "hostvars": {
                "web-01": {"ansible_host": "192.168.10.11"},
                "web-02": {"ansible_host": "192.168.10.12"},
            }
        },
    }
    if "--list" in sys.argv:
        print(json.dumps(inventory))
    elif "--host" in sys.argv:
        print(json.dumps({}))

if __name__ == "__main__":
    main()
```

```bash
chmod +x inventaires/dynamique/cmdb.py
ansible-inventory -i inventaires/dynamique/cmdb.py --graph
```

## 16. Première commande ad-hoc

Les commandes ad-hoc exécutent **un module** sans playbook — parfait pour l'exploitation
quotidienne et les vérifications rapides.

```bash
# Ping Ansible (pas ICMP) sur tout le parc
ansible all -m ansible.builtin.ping

# Temps de disponibilité des serveurs web
ansible webservers -m ansible.builtin.command -a "uptime"

# Espace disque avec sudo
ansible all -b -m ansible.builtin.command -a "df -h /"

# Redémarrer un service sur les DB
ansible dbservers -b -m ansible.builtin.service -a "name=postgresql state=restarted"
```

Options ad-hoc à connaître : `-b` (become), `-K` (demande le mot de passe sudo),
`-f 20` (forks), `-l web-01` (limiter à un hôte), `--check` (dry-run).

## 17. Dix commandes ad-hoc utiles au quotidien

```bash
# 1. Qui est connecté ?
ansible all -m ansible.builtin.command -a "who"

# 2. Version d'OS de tout le parc
ansible all -m ansible.builtin.command -a "cat /etc/os-release" | grep -E "SUCCESS|PRETTY"

# 3. Copier un fichier vers tous les hôtes
ansible all -m ansible.builtin.copy -a "src=/tmp/motd dest=/etc/motd mode=0644" -b

# 4. Créer un utilisateur d'astreinte partout
ansible all -b -m ansible.builtin.user -a "name=astreinte groups=sudo shell=/bin/bash"

# 5. Lancer une mise à jour sèche (check mode)
ansible all -b -m ansible.builtin.apt -a "upgrade=dist update_cache=yes" --check --diff

# 6. Vérifier qu'un port écoute
ansible webservers -m ansible.builtin.wait_for -a "port=80 timeout=5"

# 7. Collecter les facts d'un hôte et filtrer
ansible web-01 -m ansible.builtin.setup -a "filter=ansible_distribution*"

# 8. Exécuter une tâche en arrière-plan longue (async)
ansible all -b -m ansible.builtin.command -a "apt-get upgrade -y" -B 3600 -P 60

# 9. Tuer un processus bloqué par nom
ansible all -b -m ansible.builtin.command -a "pkill -f mon_processus_hs" --check

# 10. Tester la résolution DNS interne
ansible all -m ansible.builtin.command -a "getent hosts nas-01.example.lan"
```

## 18. Anatomie d'un playbook

```yaml
---
# playbooks/exemple_anatomie.yml
- name: Exemple d'anatomie d'un play           # 1. Nom du play (lisible dans les logs)
  hosts: webservers                            # 2. Cible : groupe d'inventaire
  become: true                                 # 3. Élévation de privilèges
  gather_facts: true                           # 4. Collecte des facts (défaut true)

  vars:                                        # 5. Variables du play
    http_port: 80

  pre_tasks:                                   # 6. Avant les rôles/tâches
    - name: Vérifier la connectivité
      ansible.builtin.ping:

  roles:                                       # 7. Rôles (optionnel)
    - role: mon_role_nginx

  tasks:                                       # 8. Tâches dans l'ordre
    - name: Installer nginx
      ansible.builtin.apt:
        name: nginx
        state: present
      notify: Redémarrer nginx                 # 9. Déclenche le handler si changed

  handlers:                                    # 10. Exécutés en fin de play, une fois
    - name: Redémarrer nginx
      ansible.builtin.service:
        name: nginx
        state: restarted

  post_tasks:                                  # 11. Après les handlers
    - name: Vérifier que le port écoute
      ansible.builtin.wait_for:
        port: "{{ http_port }}"
```

## 19. Playbook complet commenté : socle serveur Debian

```yaml
---
- name: Socle commun tous serveurs Debian/Ubuntu
  hosts: all
  become: true
  gather_facts: true

  vars:
    packages_socle:
      - vim
      - htop
      - curl
      - net-tools
      - unattended-upgrades

  tasks:
    - name: Mettre à jour le cache APT (Debian/Ubuntu uniquement)
      ansible.builtin.apt:
        update_cache: true
        cache_valid_time: 3600
      when: ansible_facts['os_family'] == "Debian"

    - name: Installer les paquets du socle
      ansible.builtin.apt:
        name: "{{ packages_socle }}"
        state: present

    - name: Déployer le MOTD d'entreprise
      ansible.builtin.copy:
        src: ../files/motd
        dest: /etc/motd
        owner: root
        group: root
        mode: "0644"

    - name: Activer les mises à jour automatiques de sécurité
      ansible.builtin.service:
        name: unattended-upgrades
        state: started
        enabled: true
```

Exécution :

```bash
ansible-playbook playbooks/socle.yml --check --diff   # dry-run d'abord !
ansible-playbook playbooks/socle.yml -l staging       # staging avant prod
ansible-playbook playbooks/socle.yml                  # production
```

## 20. Module apt : gérer les paquets Debian/Ubuntu

```yaml
- name: Mettre à jour le cache si vieux de plus d'une heure
  ansible.builtin.apt:
    update_cache: true
    cache_valid_time: 3600

- name: Installer une liste de paquets
  ansible.builtin.apt:
    name:
      - nginx
      - certbot
      - python3-certbot-nginx
    state: present

- name: Installer une version précise (épinglage)
  ansible.builtin.apt:
    name: postgresql=15.6-1.pgdg120+1
    state: present
    allow_downgrade: true

- name: Mise à jour de sécurité uniquement (équivalent unattended)
  ansible.builtin.apt:
    upgrade: dist
    update_cache: true

- name: Supprimer un paquet et purger sa configuration
  ansible.builtin.apt:
    name: apache2
    state: absent
    purge: true
    autoremove: true
```

> **Avertissement** : `upgrade: dist` sur un parc de production se fait en `serial: 1`
> ou par vagues (section 50), jamais en aveugle sur tous les nœuds à la fois.

## 21. Module copy : déployer des fichiers statiques

```yaml
- name: Déployer un fichier de configuration statique
  ansible.builtin.copy:
    src: files/ntp.conf            # relatif au playbook (ou au rôle : files/)
    dest: /etc/ntp.conf
    owner: root
    group: root
    mode: "0644"
    backup: true                   # garde une copie de l'ancien fichier
  notify: Redémarrer ntp

- name: Déployer un contenu inline (petits fichiers)
  ansible.builtin.copy:
    content: |
      # Géré par Ansible — ne pas modifier à la main
      127.0.0.1 localhost
    dest: /etc/hosts.d/ansible
    mode: "0644"

- name: Copier un répertoire entier
  ansible.builtin.copy:
    src: files/skel/
    dest: /etc/skel/
    mode: "0644"
```

`backup: true` crée `/etc/ntp.conf.12345~` : indispensable avant d'écraser un fichier
critique en production.

## 22. Module template : fichiers générés avec Jinja2

```yaml
- name: Générer la configuration nginx depuis un template
  ansible.builtin.template:
    src: nginx.conf.j2             # dans templates/ du rôle ou du playbook
    dest: /etc/nginx/nginx.conf
    owner: root
    group: root
    mode: "0644"
    backup: true
    validate: "nginx -t -c %s"     # valide AVANT d'écraser : sécurité maximale
  notify: Recharger nginx
```

Le paramètre `validate` est une **bonne pratique production** : `%s` est remplacé par le
fichier temporaire ; si la validation échoue, le fichier destination n'est pas touché.

## 23. Module file : fichiers, liens, répertoires, permissions

```yaml
- name: Créer un répertoire applicatif
  ansible.builtin.file:
    path: /opt/monapp
    state: directory
    owner: monapp
    group: monapp
    mode: "0750"

- name: Créer un lien symbolique
  ansible.builtin.file:
    src: /opt/monapp/releases/v2
    dest: /opt/monapp/current
    state: link

- name: Supprimer un fichier obsolète
  ansible.builtin.file:
    path: /etc/cron.d/vieux_job
    state: absent

- name: Toucher un fichier (créer s'il n'existe pas)
  ansible.builtin.file:
    path: /var/log/monapp/app.log
    state: touch
    mode: "0640"

- name: Appliquer récursivement des permissions
  ansible.builtin.file:
    path: /srv/data
    owner: www-data
    group: www-data
    mode: "0750"
    recurse: true
```

## 24. Modules service et systemd

```yaml
- name: S'assurer que nginx tourne et démarre au boot
  ansible.builtin.service:
    name: nginx
    state: started
    enabled: true

- name: Recharger (sans couper les connexions)
  ansible.builtin.systemd:
    name: nginx
    state: reloaded

- name: Redémarrer avec daemon-reload après changement d'unité
  ansible.builtin.systemd:
    name: monapp
    state: restarted
    daemon_reload: true

- name: Masquer un service indésirable
  ansible.builtin.systemd:
    name: telnet
    state: stopped
    enabled: false
    masked: true
```

`ansible.builtin.service` est agnostique (sysv/systemd) ; `ansible.builtin.systemd`
offre `daemon_reload`, `masked`, `scope=user`.

## 25. Modules user et group

```yaml
- name: Créer le groupe applicatif
  ansible.builtin.group:
    name: monapp
    gid: 2001
    state: present

- name: Créer un utilisateur de service (sans shell, sans home)
  ansible.builtin.user:
    name: monapp
    group: monapp
    uid: 2001
    shell: /usr/sbin/nologin
    create_home: false
    system: true

- name: Créer un admin avec clé SSH
  ansible.builtin.user:
    name: alice
    groups: sudo
    append: true                  # ne pas écraser les groupes existants !
    shell: /bin/bash
    password: "{{ vault_alice_password_hash }}"  # hash, jamais en clair

- name: Déployer la clé publique SSH d'alice
  ansible.posix.authorized_key:
    user: alice
    key: "ssh-ed25519 AAAA... alice@poste"
    state: present
```

> **Sécurité** : `password` attend un **hash** (ex. `$y$j9T$...`), généré par
> `mkpasswd --method=yescrypt`. Ne jamais mettre un mot de passe en clair,
> même dans Vault : préférez toujours le hash.

## 26. Module cron

```yaml
- name: Sauvegarde quotidienne à 2h du matin
  ansible.builtin.cron:
    name: "sauvegarde quotidienne"
    user: root
    minute: "0"
    hour: "2"
    job: "/usr/local/bin/backup.sh >> /var/log/backup.log 2>&1"

- name: Job avec variables d'environnement
  ansible.builtin.cron:
    name: "nettoyage tmp"
    user: monapp
    minute: "30"
    hour: "3"
    weekday: "0"
    job: "/opt/monapp/bin/cleanup.sh"
    env:
      - name: PATH
        value: "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

- name: Supprimer un job obsolète
  ansible.builtin.cron:
    name: "ancien job"
    state: absent
```

Chaque entrée est identifiée par `name` : Ansible la retrouve et la met à jour de façon
idempotente au lieu de dupliquer la ligne.

## 27. Modules ufw et firewalld

```yaml
# Debian/Ubuntu : UFW
- name: Activer UFW avec politique restrictive
  community.general.ufw:
    state: enabled
    policy: deny
    direction: incoming

- name: Autoriser SSH (limité contre le brute-force)
  community.general.ufw:
    rule: limit
    port: "22"
    proto: tcp

- name: Autoriser HTTP/HTTPS
  community.general.ufw:
    rule: allow
    port: "80,443"
    proto: tcp

- name: Autoriser depuis le réseau d'administration uniquement
  community.general.ufw:
    rule: allow
    port: "5666"          # NRPE / supervision
    src: 192.168.20.0/24
```

```yaml
# RHEL-like : firewalld
- name: Ouvrir HTTPS en permanent
  ansible.posix.firewalld:
    service: https
    permanent: true
    immediate: true
    state: enabled
```

## 28. Modules lineinfile et blockinfile

Pour modifier **une ligne** dans un fichier existant sans template complet :

```yaml
- name: S'assurer que le forwarding IP est activé
  ansible.builtin.lineinfile:
    path: /etc/sysctl.conf
    regexp: '^#?net.ipv4.ip_forward\s*='
    line: 'net.ipv4.ip_forward=1'
    backup: true
  notify: Appliquer sysctl

- name: Forcer PermitRootLogin à no dans sshd_config
  ansible.builtin.lineinfile:
    path: /etc/ssh/sshd_config
    regexp: '^#?PermitRootLogin'
    line: 'PermitRootLogin no'
    validate: 'sshd -t -f %s'
  notify: Redémarrer sshd

- name: Insérer un bloc géré par Ansible
  ansible.builtin.blockinfile:
    path: /etc/hosts
    marker: "# {mark} BLOC ANSIBLE"
    block: |
      192.168.10.50 nas-01
      192.168.10.51 nas-02
```

> **Bonne pratique** : si vous gérez **tout** le fichier, préférez `template`.
> `lineinfile` est réservé aux ajustements chirurgicaux sur des fichiers
> partiellement gérés par le système.

## 29. Modules get_url et unarchive

```yaml
- name: Télécharger un binaire avec vérification de checksum
  ansible.builtin.get_url:
    url: https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz
    dest: /tmp/node_exporter.tar.gz
    checksum: sha256:81a8c4294a7a7a8b8e0c...   # checksum officiel
    mode: "0644"

- name: Extraire l'archive (idempotent grâce à creates)
  ansible.builtin.unarchive:
    src: /tmp/node_exporter.tar.gz
    dest: /opt/
    remote_src: true
    creates: /opt/node_exporter-1.8.2.linux-amd64/node_exporter
```

`télécharger + vérifier le checksum + extraire` : le trio de base pour déployer
un binaire tiers sans paquet officiel.

## 30. Module git : déployer du code

```yaml
- name: Cloner le dépôt applicatif sur une branche précise
  ansible.builtin.git:
    repo: "git@git.example.lan:equipe/monapp.git"
    dest: /opt/monapp
    version: main
    key_file: /root/.ssh/id_ed25519_deploy   # clé de déploiement lecture seule
    accept_hostkey: true
    force: false                              # ne pas écraser les modifs locales

- name: Mettre à jour vers un tag précis puis notifier
  ansible.builtin.git:
    repo: https://git.example.lan/equipe/monapp.git
    dest: /opt/monapp
    version: v2.4.1
  notify: Redémarrer monapp
  register: git_result
```

Le module retourne `changed: true` uniquement si le dépôt a réellement bougé :
le handler ne redémarre l'application qu'en cas de nouveau code.

## 31. Modules command, shell, raw : à utiliser en dernier recours

```yaml
- name: Commande simple sans shell (préféré quand on doit exécuter)
  ansible.builtin.command:
    cmd: /usr/local/bin/outil --check
  register: resultat
  changed_when: false          # lecture seule : jamais "changed"
  failed_when: resultat.rc not in [0, 2]

- name: Shell uniquement si pipes/redirections indispensables
  ansible.builtin.shell:
    cmd: "cat /var/log/app/*.log | grep -c ERROR || true"
  register: nb_erreurs
  changed_when: false

- name: Raw sur un équipement sans Python (switch, vieux système)
  ansible.builtin.raw: "show version"
```

**Règle d'or** : si un module existe, utilisez-le. `shell`/`command` ne sont pas
idempotents par nature : encadrez-les toujours de `changed_when` / `failed_when`
/ `creates` / `removes`.

## 32. Modules debug, set_fact, assert

```yaml
- name: Afficher une variable pendant le développement
  ansible.builtin.debug:
    msg: "L'hôte {{ inventory_hostname }} a l'IP {{ ansible_host }}"

- name: Afficher toute la structure d'une variable
  ansible.builtin.debug:
    var: ansible_facts['network']

- name: Mémoriser un calcul pour la suite du play
  ansible.builtin.set_fact:
    backup_dir: "/srv/backup/{{ inventory_hostname }}"

- name: Valider un prérequis avant de continuer
  ansible.builtin.assert:
    that:
      - ansible_facts['distribution'] in ['Debian', 'Ubuntu']
      - ansible_facts['distribution_major_version'] | int >= 11
    fail_msg: "Ce playbook exige Debian 11+ ou Ubuntu 22.04+"
    success_msg: "OS compatible"
```

`assert` en début de playbook évite d'appliquer une configuration à un OS non testé :
c'est un garde-fou production simple et efficace.

## 33. Variables : types, définitions, portée

```yaml
# group_vars/all.yml — types de variables
nom_app: monapp                                   # chaîne
http_port: 8080                                  # entier
tls_active: true                                 # booléen
serveurs_dns:                                    # liste
  - 192.168.10.1
  - 192.168.10.2
config_reseau:                                   # dictionnaire
  interface: eth0
  mtu: 1500
liste_mixte:                                     # liste de dictionnaires
  - { nom: web-01, role: frontal }
  - { nom: db-01, role: donnees }
```

Où définir les variables (de la plus générale à la plus spécifique) :

| Emplacement | Portée | Usage |
|---|---|---|
| `group_vars/all.yml` | Tout l'inventaire | Valeurs par défaut globales |
| `group_vars/<groupe>.yml` | Un groupe | Spécificités par rôle serveur |
| `host_vars/<hote>.yml` | Un hôte | Exceptions individuelles |
| `vars:` du play | Le play | Paramétrage du playbook |
| `vars:` de la tâche | La tâche | Cas ultra-local |
| `-e` ligne de commande | L'exécution | Surcharges ponctuelles (priorité max) |

## 34. Ordre de précédence des variables (21 niveaux)

Du **moins** prioritaire au **plus** prioritaire (mémorisez les 5 qui comptent
vraiment au quotidien : ils sont marqués ★) :

| # | Niveau | Exemple |
|---|---|---|
| 1 | Arguments de ligne de commande `--extra-vars` ★ | `-e "http_port=8080"` |
| 2 | `set_facts` / variables enregistrées | `register: resultat` |
| 3 | Variables du play (`vars:`) | bloc `vars` du play |
| 4 | Variables de tâche (`vars:` sur la tâche) | bloc `vars` de la tâche |
| 5 | Variables de bloc | `vars` sur un `block` |
| 6 | Variables de rôle (`vars/main.yml`) | `roles/x/vars/main.yml` |
| 7 | Variables d'hôte (`host_vars/`) ★ | `host_vars/db-01.yml` |
| 8 | Variables de groupe (`group_vars/`) ★ | `group_vars/webservers.yml` |
| 9 | Facts de l'hôte (`ansible_facts`) | `ansible_facts['os_family']` |
| 10 | Inventaire : `host_vars` inline | `web-01 ansible_host=...` |
| 11 | Inventaire : `group_vars` inline | `[groupe:vars]` |
| 12 | `defaults/main.yml` du rôle ★ | valeurs par défaut surchargeables |
| 13 | Paramètres de connexion | `ansible_user`, `ansible_port`… |

> Les niveaux 14 à 21 concernent des cas avancés (vars plugins, `include_vars`
> dynamiques, variables de `import_role`…). En pratique : **defaults < groupe <
> hôte < extra-vars**. Le reste se consulte dans `ansible-doc` au besoin.

Règle pratique : mettez les valeurs par défaut dans `defaults/`, les valeurs
d'environnement dans `group_vars/`, les exceptions dans `host_vars/`, et les
surcharges ponctuelles en `-e`.

## 35. Facts Ansible : connaître le système cible

Les facts sont collectés automatiquement (`gather_facts: true`) au début du play.
Exemples de facts indispensables :

```yaml
- name: Utiliser les facts pour adapter la configuration
  ansible.builtin.template:
    src: app.conf.j2
    dest: /etc/monapp/app.conf
  vars:
    memoire_totale: "{{ ansible_facts['memtotal_mb'] }}"
    nb_cpu: "{{ ansible_facts['processor_vcpus'] }}"
```

| Fact | Contenu |
|---|---|
| `ansible_facts['distribution']` | `Debian`, `Ubuntu`… |
| `ansible_facts['distribution_major_version']` | `12`, `22`… |
| `ansible_facts['os_family']` | `Debian`, `RedHat`… |
| `ansible_facts['default_ipv4']['address']` | IP principale |
| `ansible_facts['memtotal_mb']` | RAM totale en Mo |
| `ansible_facts['processor_vcpus']` | Nombre de vCPU |
| `ansible_facts['mounts']` | Points de montage |
| `ansible_facts['hostname']` | Nom court de la machine |

Désactiver la collecte pour gagner du temps quand elle est inutile :

```yaml
- hosts: all
  gather_facts: false
  tasks:
    - name: Redémarrage simple sans facts
      ansible.builtin.reboot:
```

## 36. Variables magiques : inventory_hostname, groups, hostvars

```yaml
- name: Générer la liste de tous les serveurs web
  ansible.builtin.template:
    src: upstreams.conf.j2
    dest: /etc/nginx/upstreams.conf
```

```jinja2
{# templates/upstreams.conf.j2 — utilise les variables magiques #}
upstream backend {
{% for hote in groups['webservers'] %}
    server {{ hostvars[hote]['ansible_host'] }}:8080;
{% endfor %}
}
```

| Variable magique | Contenu |
|---|---|
| `inventory_hostname` | Nom de l'hôte courant dans l'inventaire |
| `inventory_hostname_short` | Nom court (avant le premier point) |
| `groups` | Dictionnaire groupe → liste d'hôtes |
| `group_names` | Groupes auxquels appartient l'hôte courant |
| `hostvars` | Variables de **tous** les hôtes |
| `play_hosts` / `ansible_play_hosts` | Hôtes du play en cours |
| `ansible_check_mode` | `true` si `--check` actif |

## 37. Handlers : réagir aux changements, une seule fois

```yaml
tasks:
  - name: Déployer nginx.conf
    ansible.builtin.template:
      src: nginx.conf.j2
      dest: /etc/nginx/nginx.conf
      validate: "nginx -t -c %s"
    notify: Recharger nginx

  - name: Déployer le vhost
    ansible.builtin.template:
      src: vhost.conf.j2
      dest: /etc/nginx/sites-enabled/app.conf
      validate: "nginx -t -c %s"
    notify: Recharger nginx

handlers:
  - name: Recharger nginx
    ansible.builtin.service:
      name: nginx
      state: reloaded
```

Même si les deux tâches changent quelque chose, **le handler ne s'exécute qu'une fois**
en fin de play. Règles :

- Un handler se déclenche uniquement si la tâche notificatrice est `changed`.
- `notify` peut appeler plusieurs handlers (liste) ou utiliser `listen` pour regrouper.
- Forcer l'exécution immédiate : `meta: flush_handlers`.

```yaml
- name: Forcer les handlers notifiés jusqu'ici
  ansible.builtin.meta: flush_handlers
```

## 38. Jinja2 : les bases du templating

Jinja2 est le moteur de templates d'Ansible. Trois délimiteurs :

| Délimiteur | Usage |
|---|---|
| `{{ ... }}` | Afficher une variable / expression |
| `{% ... %}` | Instruction (boucle, condition) |
| `{# ... #}` | Commentaire (n'apparaît pas dans le rendu) |

```jinja2
# {{ ansible_managed }} — en-tête standard : fichier géré par Ansible, ne pas éditer
serveur={{ inventory_hostname }}
port={{ http_port }}
tls={{ tls_active | ternary('on', 'off') }}
```

Bonnes pratiques :

- Quotez toujours les expressions qui commencent par `{{` en YAML :
  `port: "{{ http_port }}"` (sinon erreur de parsing YAML).
- `{{ ansible_managed }}` en en-tête de chaque template : traçabilité.
- Testez un template avec `--check --diff` avant application.

## 39. Jinja2 : boucles et conditions

```jinja2
{# Boucle simple #}
{% for dns in serveurs_dns %}
nameserver {{ dns }}
{% endfor %}

{# Boucle avec index et condition #}
{% for hote in groups['dbservers'] %}
{% if hostvars[hote]['ansible_host'] is defined %}
# {{ hote }} ({{ loop.index }}/{{ loop.length }})
db_host={{ hostvars[hote]['ansible_host'] }}
{% endif %}
{% endfor %}

{# Conditions multiples #}
{% if tls_active and cert_path is defined %}
ssl_certificate {{ cert_path }};
{% elif tls_active %}
# ATTENTION : TLS demandé mais aucun certificat défini
{% else %}
# TLS désactivé
{% endif %}
```

## 40. Jinja2 : les filtres les plus utiles

| Filtre | Exemple | Résultat |
|---|---|---|
| `default` | `{{ var \| default('vide') }}` | Valeur ou `vide` |
| `ternary` | `{{ ok \| ternary('oui','non') }}` | `oui` / `non` |
| `upper` / `lower` | `{{ nom \| upper }}` | `MONAPP` |
| `join` | `{{ liste \| join(',') }}` | `a,b,c` |
| `split` | `{{ 'a:b' \| split(':') }}` | `['a','b']` |
| `length` | `{{ liste \| length }}` | `3` |
| `sort` / `unique` | `{{ liste \| unique \| sort }}` | triée, dédupliquée |
| `dict2items` | `{{ dico \| dict2items }}` | liste de `{key, value}` |
| `to_json` / `to_yaml` | `{{ var \| to_json }}` | sérialisation |
| `password_hash` | `{{ mdp \| password_hash('yescrypt') }}` | hash yescrypt |
| `ipaddr` | `{{ '192.168.1.5/24' \| ipaddr('network') }}` | `192.168.1.0/24` |
| `b64encode` / `b64decode` | `{{ secret \| b64encode }}` | base64 |
| `regex_replace` | `{{ v \| regex_replace('^v','') }}` | substitution |

Exemple combiné :

```jinja2
serveurs_autorises={{ ips_admin | unique | sort | join(',') }}
motd_hash={{ motd_admin | password_hash('yescrypt') }}
```

## 41. Template complet : vhost nginx avec Jinja2

```jinja2
# {{ ansible_managed }}
# Rôle nginx — vhost {{ nom_app }}

upstream {{ nom_app }}_backend {
{% for hote in groups['webservers'] %}
    server {{ hostvars[hote]['ansible_host'] }}:{{ port_backend }} max_fails=3 fail_timeout=30s;
{% endfor %}
}

server {
    listen {{ http_port }}{% if tls_active %} ssl{% endif %};
    server_name {{ nom_domaine }};

{% if tls_active %}
    ssl_certificate {{ cert_path }};
    ssl_certificate_key {{ cle_path }};
    ssl_protocols TLSv1.2 TLSv1.3;
{% endif %}

    access_log /var/log/nginx/{{ nom_app }}_access.log;
    error_log  /var/log/nginx/{{ nom_app }}_error.log;

    location / {
        proxy_pass http://{{ nom_app }}_backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

## 42. Rôles : l'arborescence standard

Un rôle regroupe tâches, handlers, templates, fichiers et variables par fonction.
C'est l'unité de réutilisation d'Ansible.

```text
roles/nginx/
├── tasks/
│   ├── main.yml          # point d'entrée : inclut les autres
│   ├── install.yml
│   └── configure.yml
├── handlers/
│   └── main.yml
├── templates/
│   ├── nginx.conf.j2
│   └── vhost.conf.j2
├── files/
│   └── dhparam.pem
├── vars/
│   └── main.yml          # variables du rôle (forte précédence)
├── defaults/
│   └── main.yml          # valeurs par défaut (faible précédence ★)
├── meta/
│   └── main.yml          # dépendances, plateformes supportées
└── README.md
```

Créer le squelette :

```bash
ansible-galaxy role init roles/nginx
```

`tasks/main.yml` typique :

```yaml
---
- name: Inclure l'installation
  ansible.builtin.include_tasks: install.yml

- name: Inclure la configuration
  ansible.builtin.include_tasks: configure.yml
```

## 43. Rôle complet : exemple `durcissement_ssh`

```yaml
# roles/durcissement_ssh/defaults/main.yml
---
ssh_port: 22
ssh_permit_root_login: "no"
ssh_password_authentication: "no"
ssh_max_auth_tries: 3
```

```yaml
# roles/durcissement_ssh/tasks/main.yml
---
- name: Déployer sshd_config durci
  ansible.builtin.template:
    src: sshd_config.j2
    dest: /etc/ssh/sshd_config
    owner: root
    group: root
    mode: "0600"
    backup: true
    validate: "sshd -t -f %s"
  notify: Redémarrer sshd

- name: S'assurer que sshd tourne
  ansible.builtin.service:
    name: ssh
    state: started
    enabled: true
```

```yaml
# roles/durcissement_ssh/handlers/main.yml
---
- name: Redémarrer sshd
  ansible.builtin.service:
    name: ssh
    state: restarted
```

```jinja2
{# roles/durcissement_ssh/templates/sshd_config.j2 #}
# {{ ansible_managed }}
Port {{ ssh_port }}
PermitRootLogin {{ ssh_permit_root_login }}
PasswordAuthentication {{ ssh_password_authentication }}
ChallengeResponseAuthentication no
MaxAuthTries {{ ssh_max_auth_tries }}
X11Forwarding no
AllowAgentForwarding no
```

Utilisation :

```yaml
- hosts: all
  become: true
  roles:
    - role: durcissement_ssh
      vars:
        ssh_port: 2222
```

## 44. Ansible Galaxy : partager et réutiliser des rôles

```bash
# Initialiser un rôle partageable
ansible-galaxy role init mon_role --init-path roles/

# Installer un rôle depuis Galaxy
ansible-galaxy role install geerlingguy.nginx

# Figer les versions dans requirements.yml (recommandé)
cat > roles/requirements.yml <<'EOF'
---
roles:
  - name: geerlingguy.nginx
    version: 3.2.0
    src: https://galaxy.ansible.com
EOF
ansible-galaxy role install -r roles/requirements.yml
```

> **Prudence production** : un rôle Galaxy est du code tiers exécuté en root.
> Épinglez la version, lisez le code (`roles/geerlingguy.nginx/tasks/`), et
> testez en staging avant la production.

## 45. Collections : le packaging moderne

Les collections regroupent modules, plugins, rôles et playbooks par domaine
(`community.general`, `amazon.aws`, `ansible.posix`…).

```yaml
# collections/requirements.yml
---
collections:
  - name: community.general
    version: ">=9.0.0"
  - name: amazon.aws
    version: ">=8.0.0"
  - name: ansible.posix
    version: ">=1.5.0"
```

```bash
ansible-galaxy collection install -r collections/requirements.yml
ansible-galaxy collection list
```

Dans les playbooks, utilisez les **FQCN** (noms pleinement qualifiés) :

```yaml
# Bien : explicite, pas d'ambiguïté
- ansible.posix.firewalld:
    service: https
    state: enabled

# Éviter : dépend de la résolution implicite
- firewalld:
    service: https
```

## 46. Ansible Vault : créer un coffre-fort

Vault chiffre les variables sensibles (mots de passe, clés API, certificats).
Le contenu chiffré reste versionnable en Git en toute sécurité.

```bash
# Créer un fichier chiffré
ansible-vault create group_vars/all/vault.yml

# Le fichier contient ensuite (éditable uniquement avec le mot de passe) :
# vault_pve_api_password: "SUPER_SECRET_A_CHANGER"
# vault_smtp_password: "AUTRE_SECRET"

# Chiffrer un fichier existant
ansible-vault encrypt host_vars/db-01/secrets.yml

# Déchiffrer (rare, à éviter : préférez edit)
ansible-vault decrypt /tmp/fichier.yml

# Changer le mot de passe du coffre
ansible-vault rekey group_vars/all/vault.yml
```

Fichier de mot de passe pour l'automatisation (CI/AWX) :

```bash
# ~/.vault_pass : chmod 600, hors Git !
echo "mot-de-passe-du-coffre" > ~/.vault_pass
chmod 600 ~/.vault_pass
# ansible.cfg :
# [defaults]
# vault_password_file = ~/.vault_pass
```

> **Sécurité** : le fichier `~/.vault_pass` a les droits `600`, n'est jamais
> commité, et idéalement est fourni par un gestionnaire de secrets (HashiCorp
> Vault, Bitwarden, variable d'environnement CI masquée).

## 47. Vault : chiffrer, éditer, utiliser au quotidien

```bash
# Éditer un coffre existant
ansible-vault edit group_vars/all/vault.yml

# Voir sans éditer
ansible-vault view group_vars/all/vault.yml

# Chiffrer une seule valeur (pratique pour une variable isolée)
ansible-vault encrypt_string 'mon_secret' --name 'api_token'
# Résultat à coller dans un fichier vars :
# api_token: !vault |
#   $ANSIBLE_VAULT;1.1;AES256
#   3362663138623839...

# Exécuter un playbook avec le coffre
ansible-playbook site.yml --ask-vault-pass
ansible-playbook site.yml --vault-password-file ~/.vault_pass
```

Utilisation dans un playbook :

```yaml
- name: Créer l'utilisateur de sauvegarde
  ansible.builtin.user:
    name: backup
    password: "{{ vault_backup_password_hash }}"  # hash yescrypt chiffré par Vault
```

## 48. Vault : bonnes pratiques et erreurs à éviter

- [ ] **Un coffre par environnement** : `vault_prod.yml` / `vault_staging.yml` avec des
      mots de passe différents (`--vault-id prod@~/.vault_pass_prod`).
- [ ] Ne chiffrez que les secrets, pas tout le fichier : le diff Git reste lisible.
- [ ] Nommez les variables `vault_*` pour repérer d'un coup d'œil ce qui est sensible.
- [ ] Ne commitez **jamais** `~/.vault_pass`, même par accident : ajoutez-le au `.gitignore`
      global et vérifiez avec `git log --all --full-history -- "*vault_pass*"`.
- [ ] Rotation : `ansible-vault rekey` à chaque départ d'un membre de l'équipe.
- [ ] En cas de fuite du mot de passe Vault, considérez **tous** les secrets compromis.

```bash
# Multi-vault-id : prod et staging séparés
ansible-playbook site.yml \
  --vault-id prod@~/.vault_pass_prod \
  --vault-id staging@~/.vault_pass_staging
```

## 49. Stratégies d'exécution

La stratégie définit **comment** les tâches sont réparties sur les hôtes.

```yaml
- hosts: webservers
  strategy: linear        # défaut : tâche N finie partout avant tâche N+1
  tasks: [...]

- hosts: webservers
  strategy: free          # chaque hôte avance à son rythme (plus rapide, logs mélangés)

- hosts: webservers
  strategy: host_pinned   # comme free mais un hôte reste sur le même worker
```

| Stratégie | Cas d'usage |
|---|---|
| `linear` (défaut) | Déploiements ordonnés, la plupart des cas |
| `free` | Tâches longues indépendantes (mises à jour, sauvegardes) |
| `host_pinned` | `free` + affinité hôte/worker (gros parcs) |
| `debug` | Pas à pas interactif (débogage) |

## 50. Parallélisme : forks, serial, throttle

```ini
# ansible.cfg — forks globaux
[defaults]
forks = 30
```

```yaml
# serial : vagues successives (rolling update)
- hosts: webservers
  serial: 2                 # 2 hôtes à la fois
  # serial: "25%"            # ou en pourcentage
  # serial: [1, 5, 10]       # 1 puis 5 puis 10 (montée en charge prudente)
  tasks:
    - name: Déployer la nouvelle version
      ...

# throttle : limite globale même avec beaucoup de forks
- name: Appel API limité à 3 simultanés
  ansible.builtin.uri:
    url: https://api.example.lan/deploy
  throttle: 3

# batch par groupe avec max_fail_percentage
- hosts: all
  serial: "20%"
  max_fail_percentage: 10   # stop si plus de 10 % d'échecs dans la vague
  tasks: [...]
```

Repères de dimensionnement :

| Taille du parc | forks conseillé | serial conseillé |
|---|---|---|
| < 20 hôtes | 5–10 | inutile |
| 20–100 hôtes | 20–30 | `25%` pour les déploiements |
| 100–500 hôtes | 50–100 | `[10, "25%", "50%"]` |
| 500+ hôtes | 100+ + `host_pinned` | par vagues + `max_fail_percentage` |

Surveillez la charge du control node : chaque fork = un processus SSH + Python.

## 51. Gestion des erreurs : failed_when et changed_when

```yaml
- name: Vérifier la santé de l'application
  ansible.builtin.uri:
    url: http://localhost:8080/health
    status_code: 200
  register: sante
  # Échec seulement si le service ne répond pas après 3 essais
  retries: 3
  delay: 10
  until: sante.status == 200

- name: Commande dont le code 2 est normal
  ansible.builtin.command: /usr/bin/monoutil --verif
  register: verif
  failed_when: verif.rc not in [0, 2]
  changed_when: "'MODIFIE' in verif.stdout"

- name: Lecture seule, jamais marquée changed
  ansible.builtin.command: cat /etc/os-release
  changed_when: false
```

Par défaut, `failed_when` = code retour non nul. Surchargez-le dès qu'un outil
utilise des codes de sortie non standard.

## 52. block / rescue / always : le try/catch d'Ansible

```yaml
- name: Déploiement avec rollback
  block:
    - name: Arrêter l'application
      ansible.builtin.service:
        name: monapp
        state: stopped

    - name: Déployer la nouvelle version
      ansible.builtin.unarchive:
        src: /srv/releases/monapp-v2.5.tar.gz
        dest: /opt/monapp
        remote_src: true

  rescue:
    - name: Restaurer la version précédente
      ansible.builtin.unarchive:
        src: /srv/releases/monapp-v2.4.tar.gz
        dest: /opt/monapp
        remote_src: true

    - name: Alerter l'équipe
      ansible.builtin.debug:
        msg: "ROLLBACK effectué sur {{ inventory_hostname }}"

  always:
    - name: Redémarrer l'application dans tous les cas
      ansible.builtin.service:
        name: monapp
        state: started
```

`block` = tentative, `rescue` = exécuté **uniquement en cas d'échec**,
`always` = exécuté **dans tous les cas** (nettoyage, redémarrage).

## 53. Check mode (--check) et diff (--diff)

```bash
# Dry-run : montre ce qui CHANGERAIT sans rien modifier
ansible-playbook site.yml --check

# Dry-run + diff détaillé des fichiers
ansible-playbook site.yml --check --diff

# Forcer l'exécution réelle d'une tâche normalement ignorée en check
# (ex. : créer un répertoire temporaire nécessaire au diff)
- name: Préparer le répertoire de travail
  ansible.builtin.file:
    path: /tmp/work
    state: directory
  check_mode: false

# Forcer le dry-run d'une tâche dangereuse même sans --check
- name: Ne jamais exécuter sans --check
  ansible.builtin.command: /sbin/reboot
  check_mode: true   # tâche simulée même en exécution réelle : à manier avec soin
```

> **Limite du check mode** : les modules `command`/`shell` ne peuvent pas prédire
> leur effet ; Ansible les **saute** en `--check` sauf `check_mode: false`.
> Un `--check` vert ne garantit donc pas une exécution réelle verte.

## 54. Tags : exécuter une partie du playbook

```yaml
- name: Installer nginx
  ansible.builtin.apt:
    name: nginx
    state: present
  tags: [install, nginx]

- name: Configurer nginx
  ansible.builtin.template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
  tags: [config, nginx]
  notify: Recharger nginx

- name: Toujours exécuté (prérequis)
  ansible.builtin.apt:
    update_cache: true
    cache_valid_time: 3600
  tags: [always]
```

```bash
ansible-playbook site.yml --tags "config"        # que la configuration
ansible-playbook site.yml --tags "nginx"         # tout nginx
ansible-playbook site.yml --skip-tags "install"  # tout sauf l'installation
ansible-playbook site.yml --list-tags            # inventorier les tags
```

Convention d'équipe : `install`, `config`, `service`, `securite`, `always`, `never`
(pour les tâches destructrices type `reboot` à déclencher explicitement).

## 55. Délégation : delegate_to, run_once, local_action

```yaml
- name: Exécuter sur le control node (ex. : appel API central)
  ansible.builtin.uri:
    url: https://cmdb.example.lan/api/maint/{{ inventory_hostname }}
    method: POST
  delegate_to: localhost
  run_once: true        # une seule fois pour tout le play, pas par hôte

- name: Sauvegarder la base depuis le serveur de backup
  ansible.builtin.command: /usr/local/bin/dump.sh {{ inventory_hostname }}
  delegate_to: backup-01
  # la tâche tourne sur backup-01, mais UNE FOIS PAR HÔTE du play

- name: Ajouter temporairement au load balancer
  ansible.builtin.command: /usr/local/bin/lb_drain.sh {{ inventory_hostname }}
  delegate_to: lb-01
```

`delegate_to: localhost` + `run_once: true` : le motif standard pour les actions
centralisées (DNS, CMDB, tickets, supervision) dans un play multi-hôtes.

## 56. Boucles : loop et filtres associés

```yaml
- name: Installer plusieurs paquets
  ansible.builtin.apt:
    name: "{{ item }}"
    state: present
  loop:
    - nginx
    - certbot
    - htop

- name: Créer plusieurs utilisateurs
  ansible.builtin.user:
    name: "{{ item.nom }}"
    groups: "{{ item.groupes }}"
    shell: /bin/bash
  loop:
    - { nom: alice, groupes: sudo }
    - { nom: bob, groupes: docker }

- name: Déployer plusieurs templates
  ansible.builtin.template:
    src: "{{ item.src }}"
    dest: "{{ item.dest }}"
    mode: "0644"
  loop:
    - { src: app.conf.j2, dest: /etc/monapp/app.conf }
    - { src: log.conf.j2, dest: /etc/monapp/log.conf }

# Boucle sur un dictionnaire
- name: Configurer les vhosts
  ansible.builtin.template:
    src: vhost.conf.j2
    dest: "/etc/nginx/sites-enabled/{{ item.key }}.conf"
  loop: "{{ vhosts | dict2items }}"
```

Anciennes syntaxes `with_items`, `with_dict` : fonctionnelles mais dépréciées
dans la documentation — préférez `loop` + filtres Jinja2.

## 57. Conditions : when

```yaml
- name: Installer le paquet Debian uniquement sur Debian/Ubuntu
  ansible.builtin.apt:
    name: nftables
    state: present
  when: ansible_facts['os_family'] == "Debian"

- name: Redémarrer si le noyau a changé
  ansible.builtin.reboot:
  when:
    - kernel_update is changed
    - autoriser_reboot | default(false)

- name: Tâche réservée à la production
  ansible.builtin.debug:
    msg: "Je tourne en PROD"
  when: "'production' in group_names"

# when sur un block entier
- name: Bloc conditionnel
  when: installer_monitoring | default(true)
  block:
    - name: Installer node_exporter
      ...
```

`when` accepte une liste (ET logique). Pour OU logique : `when: a or b`.
Attention : `when: ma_variable` échoue si la variable n'existe pas —
utilisez `| default(...)` ou `is defined`.

## 58. become : l'élévation de privilèges

```yaml
# Au niveau du play (recommandé : homogène)
- hosts: all
  become: true
  become_user: root
  become_method: sudo
  tasks: [...]

# Ponctuellement sur une tâche
- name: Lire un fichier root en tant que simple utilisateur sudo
  ansible.builtin.command: cat /root/secret.txt
  become: true
  changed_when: false

# Devenir un autre utilisateur que root
- name: Exécuter en tant que postgres
  ansible.builtin.command: psql -c "SELECT 1"
  become: true
  become_user: postgres
  changed_when: false
```

Côté sudoers sur les cibles (`/etc/sudoers.d/ansible`) :

```text
deploy ALL=(ALL) NOPASSWD: ALL
```

> **Sécurité** : `NOPASSWD` est pratique mais large. En environnement sensible,
> restreignez les commandes autorisées et utilisez `become_ask_pass: true`
> avec `--ask-become-pass` pour les interventions manuelles.

## 59. SSH : clés, multiplexage, bastion, durcissement

```bash
# 1. Clé dédiée à Ansible (jamais la clé personnelle)
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_ansible -C "ansible@control-node"

# 2. Déployer la clé sur le parc (une fois, avec mot de passe)
ssh-copy-id -i ~/.ssh/id_ed25519_ansible deploy@web-01

# 3. ssh_config du control node (~/.ssh/config)
Host *.example.lan
    User deploy
    IdentityFile ~/.ssh/id_ed25519_ansible
    ServerAliveInterval 30
```

Bastion dans l'inventaire (déjà vu section 11) ou global :

```ini
[all:vars]
ansible_ssh_common_args=-o ProxyJump=deploy@bastion.example.lan -o StrictHostKeyChecking=yes
```

Checklist SSH production :

- [ ] Clé ed25519 dédiée, `chmod 600`
- [ ] `ControlPersist` activé (section 7) pour la vitesse
- [ ] `host_key_checking = True` + `~/.ssh/known_hosts` rempli (via `ssh-keyscan` versionné)
- [ ] Port SSH non standard + `fail2ban` côté cibles (voir cas pratique 2)

## 60. Performance : pipelining, facts, stratégies

| Levier | Gain typique | Risque / condition |
|---|---|---|
| `pipelining = True` | ÷2 à ÷5 sur le temps | sudo sans `requiretty` |
| `ControlPersist` (section 7) | Connexions réutilisées | Aucun |
| `gather_facts: false` | −5 à 15 s par hôte | Uniquement si facts inutiles |
| `forks` adapté (section 50) | Linéaire jusqu'à saturation | Charge du control node |
| `strategy: free` | Hôtes rapides non bloqués | Logs entrelacés |
| `serial` bien dimensionné | Rolling updates sûrs | Plus lent par conception |
| Facts en cache (`fact_caching`) | Playbooks répétés | Redis ou JSON file |

Cache des facts pour les exécutions répétées :

```ini
[defaults]
fact_caching = jsonfile
fact_caching_connection = /tmp/ansible_facts_cache
fact_caching_timeout = 86400
```

## 61. Supervision : logs et callbacks

```ini
# ansible.cfg
[defaults]
log_path = ./logs/ansible.log
stdout_callback = yaml
# En cas d'échec : résumé dense des hôtes en échec
# (plugin community.general.log_plays ou callback splunk à configurer)
```

Plugins de callback utiles :

| Callback | Usage |
|---|---|
| `yaml` | Sortie lisible (défaut conseillé) |
| `dense` | Résumé compact en fin d'exécution |
| `timer` / `profile_tasks` | Temps par tâche (optimisation) |
| `logstash` / `splunk` | Envoi vers la supervision centralisée |
| `mail` | Notification email en fin de play |

```bash
# Activer le profiling ponctuellement
ANSIBLE_CALLBACKS_ENABLED=ansible.posix.profile_tasks ansible-playbook site.yml

# Journaliser en JSON pour ingestion (ELK/Loki)
ANSIBLE_STDOUT_CALLBACK=json ansible-playbook site.yml > run.json
```

Conservez les logs d'exécution 90 jours minimum : c'est la preuve d'audit
« qui a changé quoi, quand, sur quel serveur ».

## 62. ARA : enregistrer et visualiser les exécutions

[ARA](https://ara.recordsansible.org/) (Ansible Run Analysis) enregistre chaque
exécution dans une base SQLite/PostgreSQL avec interface web.

```bash
pipx inject ansible "ara[server]"

# ansible.cfg
[defaults]
callbacks_enabled = ara_default

# Lancer le serveur web ARA
ara-manage runserver 0.0.0.0:8000
```

Bénéfices : historique des `changed`, comparaison avant/après, recherche par hôte
ou par tâche, API REST pour la supervision. Alternative légère : le callback
`community.general.log_plays` qui écrit un fichier par hôte.

## 63. Sauvegarder les playbooks avec Git

```bash
cd ~/projets/ansible
git init
cat > .gitignore <<'EOF'
# Secrets et fichiers locaux
.vault_pass*
*vault_pass*
logs/
*.retry
collections/
roles_tiers/
# Inventaires sensibles éventuels
inventaires/*secret*
EOF
git add .
git commit -m "Socle Ansible initial"
```

Workflow d'équipe recommandé :

1. Une branche par changement (`git checkout -b feature/durcissement-ssh`).
2. `--check --diff` en local, puis revue de code (merge request).
3. `main` = ce qui tourne en production ; taggez les versions (`v2026.09.1`).
4. En cas d'incident : `git log --oneline -20` puis `git diff v2026.09.0..HEAD`
   pour identifier le changement fautif, rollback par `git revert`.

> **Règle absolue** : aucun secret en clair dans Git, même « temporairement ».
> L'historique Git n'oublie jamais (`git log -p` les retrouvera).

## 64. AWX / Ansible Controller : tour d'horizon

AWX (open source) et Ansible Controller (Red Hat, ex-Tower) ajoutent une
**interface web + API + ordonnanceur** au-dessus d'Ansible.

| Fonctionnalité | Apport vs CLI |
|---|---|
| Inventaires dynamiques | Synchronisés depuis cloud/CMDB |
| Credentials | Secrets chiffrés en base, jamais sur disque |
| Job templates | Playbooks paramétrables en un clic |
| Ordonnancement | Cron intégré avec historique |
| RBAC | Qui peut lancer quoi, sur quel inventaire |
| Workflows | Enchaîner plusieurs job templates |
| Notifications | Slack/email/webhook en fin de job |

Quand passer à AWX :

- [ ] Plusieurs opérateurs lancent des playbooks (besoin de traçabilité).
- [ ] Les exécutions doivent être planifiées (patching mensuel).
- [ ] Délégation à des non-experts (helpdesk relance un playbook via template).

Installation rapide (Docker Compose, labo) :

```bash
git clone https://github.com/ansible/awx.git
cd awx
# Suivre docs/awx-operator pour Kubernetes en production
```

> **Avertissement** : AWX en production se déploie via l'opérateur Kubernetes
> officiel, pas via un compose artisanal. Prévoyez PostgreSQL managé et des
> backups de la base AWX (c'est là que vivent les credentials).

## 65. Cas pratique 1 : déployer un serveur web complet (commenté)

Objectif : sur `web-01`, installer nginx + PHP-FPM, déployer un vhost, ouvrir le
pare-feu, vérifier le service. Idempotent et rejouable.

```yaml
---
- name: Déployer la stack web LEMP
  hosts: webservers
  become: true
  gather_facts: true

  vars:
    nom_domaine: intranet.example.lan
    php_version: "8.3"

  pre_tasks:
    # Garde-fou : on ne déploie que sur Debian/Ubuntu testés
    - name: Vérifier l'OS supporté
      ansible.builtin.assert:
        that:
          - ansible_facts['os_family'] == "Debian"
        fail_msg: "OS non supporté : {{ ansible_facts['distribution'] }}"

  tasks:
    # --- Installation ---
    - name: Installer nginx et PHP-FPM
      ansible.builtin.apt:
        name:
          - nginx
          - "php{{ php_version }}-fpm"
          - "php{{ php_version }}-mysql"
        state: present
        update_cache: true
        cache_valid_time: 3600
      tags: [install]

    # --- Configuration nginx ---
    - name: Déployer le vhost
      ansible.builtin.template:
        src: templates/vhost_lemp.conf.j2
        dest: /etc/nginx/sites-available/intranet.conf
        mode: "0644"
        validate: "nginx -t -c %s"   # jamais de nginx cassé en prod
      tags: [config]
      notify: Recharger nginx

    - name: Activer le site
      ansible.builtin.file:
        src: /etc/nginx/sites-available/intranet.conf
        dest: /etc/nginx/sites-enabled/intranet.conf
        state: link
      notify: Recharger nginx

    - name: Désactiver le site par défaut
      ansible.builtin.file:
        path: /etc/nginx/sites-enabled/default
        state: absent
      notify: Recharger nginx

    # --- Racine web ---
    - name: Créer la racine web
      ansible.builtin.file:
        path: /var/www/intranet
        state: directory
        owner: www-data
        group: www-data
        mode: "0755"

    - name: Déployer la page d'accueil
      ansible.builtin.template:
        src: templates/index.php.j2
        dest: /var/www/intranet/index.php
        owner: www-data
        group: www-data
        mode: "0644"

    # --- Pare-feu ---
    - name: Autoriser HTTP/HTTPS via UFW
      community.general.ufw:
        rule: allow
        port: "80,443"
        proto: tcp
      tags: [firewall]

    # --- Services ---
    - name: S'assurer que nginx et PHP-FPM tournent
      ansible.builtin.service:
        name: "{{ item }}"
        state: started
        enabled: true
      loop: [nginx, "php{{ php_version }}-fpm"]
      tags: [service]

  handlers:
    - name: Recharger nginx
      ansible.builtin.service:
        name: nginx
        state: reloaded

  post_tasks:
    # --- Vérification finale ---
    - name: Vérifier que le site répond 200
      ansible.builtin.uri:
        url: "http://localhost/"
        status_code: 200
      register: check_site
      retries: 3
      delay: 5
      until: check_site.status == 200
```

```bash
ansible-playbook playbooks/deployer_web.yml --check --diff -l web-01
ansible-playbook playbooks/deployer_web.yml -l web-01
curl -I http://web-01/
```

## 66. Cas pratique 2 : durcir SSH sur 50 serveurs

Objectif : appliquer le rôle `durcissement_ssh` (section 43) à tout le parc,
par vagues, avec rollback manuel documenté.

```yaml
---
- name: Durcissement SSH du parc
  hosts: all
  become: true
  serial: 5                    # 5 serveurs à la fois : on garde l'accès en cas de pépin
  max_fail_percentage: 20      # stop si > 20 % d'échecs dans une vague

  pre_tasks:
    - name: Sauvegarder sshd_config actuel
      ansible.builtin.copy:
        src: /etc/ssh/sshd_config
        dest: /root/sshd_config.avant_ansible
        remote_src: true
        mode: "0600"

  roles:
    - role: durcissement_ssh
      vars:
        ssh_port: 2222
        ssh_permit_root_login: "no"

  post_tasks:
    - name: Vérifier que le nouveau port répond (avant de couper l'ancien accès)
      ansible.builtin.wait_for:
        host: "{{ ansible_host }}"
        port: 2222
        timeout: 10
      delegate_to: localhost
      become: false
```

Points de vigilance :

- [ ] Testez d'abord sur **1 serveur de staging** avec `--check --diff`.
- [ ] Gardez une session SSH ouverte sur chaque vague pendant l'application.
- [ ] La sauvegarde `/root/sshd_config.avant_ansible` permet un rollback :
      `ansible all -b -m ansible.builtin.command -a "cp /root/sshd_config.avant_ansible /etc/ssh/sshd_config && systemctl restart ssh"`.
- [ ] Mettez à jour l'inventaire (`ansible_port=2222`) **après** validation.

## 67. Cas pratique 3 : gérer les utilisateurs sur un parc

Objectif : créer les comptes d'équipe depuis une variable centrale, déployer les
clés SSH, supprimer les comptes obsolètes.

```yaml
# group_vars/all.yml
utilisateurs_equipe:
  - nom: alice
    groupes: [sudo, docker]
    cle_ssh: "ssh-ed25519 AAAA... alice@poste"
    etat: present
  - nom: bob
    groupes: [docker]
    cle_ssh: "ssh-ed25519 BBBB... bob@poste"
    etat: present
  - nom: charlie          # départ : compte à supprimer
    etat: absent
```

```yaml
---
- name: Gérer les comptes utilisateurs du parc
  hosts: all
  become: true

  tasks:
    - name: Créer / supprimer les comptes
      ansible.builtin.user:
        name: "{{ item.nom }}"
        groups: "{{ item.groupes | default([]) }}"
        append: true
        shell: /bin/bash
        state: "{{ item.etat }}"
        remove: "{{ item.etat == 'absent' }}"   # supprime aussi le home si absent
      loop: "{{ utilisateurs_equipe }}"
      loop_control:
        label: "{{ item.nom }}"

    - name: Déployer les clés SSH (comptes présents uniquement)
      ansible.posix.authorized_key:
        user: "{{ item.nom }}"
        key: "{{ item.cle_ssh }}"
        state: present
      loop: "{{ utilisateurs_equipe }}"
      when: item.etat == 'present' and item.cle_ssh is defined
      loop_control:
        label: "{{ item.nom }}"
```

`loop_control.label` rend la sortie lisible (affiche juste le nom au lieu de tout
le dictionnaire). `remove: true` sur un compte `absent` nettoie le home :
à n'utiliser qu'après validation avec le management.

## 68. Cas pratique 4 : campagne de mises à jour planifiée

Objectif : patching mensuel des serveurs Debian/Ubuntu, par vagues, avec reboot
conditionnel et fenêtre de maintenance.

```yaml
---
- name: Patching mensuel
  hosts: all
  become: true
  serial: "25%"
  max_fail_percentage: 10

  tasks:
    - name: Mise à jour des paquets (dist-upgrade)
      ansible.builtin.apt:
        upgrade: dist
        update_cache: true
        cache_valid_time: 3600
      register: resultat_apt

    - name: Vérifier si un reboot est requis
      ansible.builtin.stat:
        path: /var/run/reboot-required
      register: reboot_requis

    - name: Rebooter si nécessaire (fenêtre de maintenance)
      ansible.builtin.reboot:
        reboot_timeout: 600
        msg: "Reboot planifié après patching Ansible"
      when:
        - reboot_requis.stat.exists
        - autoriser_reboot | default(false)

    - name: Attendre le retour du serveur
      ansible.builtin.wait_for_connection:
        timeout: 300
      when: reboot_requis.stat.exists and (autoriser_reboot | default(false))
```

```bash
# Fenêtre de maintenance : on autorise explicitement le reboot
ansible-playbook playbooks/patching.yml -e "autoriser_reboot=true" -l staging
```

## 69. Dépannage : méthodologie générale

1. **Reproduire en ciblant un seul hôte** : `-l hote-en-panne --check --diff`.
2. **Monter la verbosité** : `-v` (résultats), `-vv` (fichiers transférés),
   `-vvv` (connexions SSH), `-vvvv` (debug connexion brute).
3. **Lire le message d'erreur réel** : Ansible affiche souvent la cause
   (`FAILED! => {"msg": "..."}`) — ne la survolez pas.
4. **Isoler** : `ansible hote -m ansible.builtin.ping` puis le module seul en ad-hoc.
5. **Vérifier les classiques** : YAML valide ? Variables définies ? Droits `become` ?
6. **Consulter** : `ansible-doc <module>`, logs `log_path`, ARA (section 62).

```bash
# Valider la syntaxe YAML de tous les playbooks
for f in playbooks/*.yml; do python3 -c "import yaml,sys; yaml.safe_load(open('$f'))" && echo "OK $f"; done

# Vérifier un playbook sans l'exécuter (syntax check)
ansible-playbook playbooks/site.yml --syntax-check
```

## 70. Dépannage : cas concret « UNREACHABLE » SSH

Symptôme :

```text
fatal: [web-01]: UNREACHABLE! => {"changed": false, "msg": "Failed to connect to the host via ssh: ..."}
```

Checklist de résolution :

```bash
# 1. Le SSH manuel fonctionne-t-il avec la même clé ?
ssh -i ~/.ssh/id_ed25519_ansible deploy@web-01

# 2. Ansible utilise-t-il la bonne clé et le bon utilisateur ?
ansible web-01 -m ansible.builtin.ping -vvvv 2>&1 | grep -E "SSH|identity|user"

# 3. L'hôte est-il bien dans l'inventaire avec la bonne IP ?
ansible-inventory --host web-01

# 4. Le port est-il ouvert ?
ansible web-01 -m ansible.builtin.wait_for -a "port=22 timeout=5"  # via un autre chemin
```

Causes fréquentes : clé non déployée, `ansible_port` oublié après changement de
port, `known_hosts` corrompu (`ssh-keygen -R web-01`), pare-feu, bastion mal configuré.

## 71. Dépannage : cas concret « become / sudo »

Symptôme :

```text
fatal: [db-01]: FAILED! => {"msg": "Missing sudo password"}
```

ou :

```text
"msg": "sudo: a password is required"
```

Résolutions :

```bash
# L'utilisateur a-t-il le droit sudo NOPASSWD ?
ansible db-01 -b -m ansible.builtin.command -a "sudo -n true && echo OK"

# Si mot de passe requis : demander interactivement
ansible-playbook site.yml --ask-become-pass   # ou -K

# Vérifier la config sudoers côté cible
ansible db-01 -m ansible.builtin.command -a "sudo -l"
```

Autre piège : `pipelining = True` + `requiretty` activé dans sudoers →
désactivez `requiretty` (`Defaults !requiretty` dans `/etc/sudoers.d/ansible`)
ou repassez `pipelining = False`.

## 72. Dépannage : cas concret « template / Jinja2 »

Symptôme :

```text
fatal: [web-01]: FAILED! => {"msg": "AnsibleUndefinedVariable: 'dict object' has no attribute 'cle'"}
```

Résolutions :

```bash
# 1. Voir la valeur réelle de la variable
ansible web-01 -m ansible.builtin.debug -a "var=ma_variable"

# 2. Tester le rendu du template en dry-run avec diff
ansible-playbook site.yml --check --diff -l web-01 --tags config

# 3. Rendre le template tolérant
# {{ ma_variable | default('valeur_par_defaut') }}
# {% if ma_variable is defined %}...{% endif %}
```

Pièges fréquents : variable définie dans `host_vars` d'un autre hôte, faute de
frappe (`http_port` vs `http_ports`), filtre appliqué à `None`, accolades
littérales dans le fichier destination (échappez avec `{{ '{{' }}`).

## 73. Dépannage : cas concret « YAML »

Symptôme : `ERROR! We were unable to read either as JSON nor YAML`.

```yaml
# FAUX : tabulation (YAML interdit les tabulations)
- name: Test
\tansible.builtin.ping:

# FAUX : ":" sans espace dans une valeur
- name: Test
  ansible.builtin.debug:
    msg: "erreur:ici"        # OK car quoté ; sans quotes ce serait une erreur

# FAUX : expression Jinja2 non quotée en début de valeur
  port: {{ http_port }}       # ERREUR
  port: "{{ http_port }}"     # OK

# FAUX : liste mal indentée
  loop:
  - a
   - b                        # indentation incohérente

# JUSTE
  loop:
    - a
    - b
```

Outils : `yamllint` (`pipx install yamllint`), `--syntax-check`, et l'option
`ansible.builtin.debug: var=` pour inspecter.

## 74. Dépannage : cas concret « handler jamais déclenché »

Symptôme : le fichier est déployé mais le service n'est pas rechargé.

Causes et remèdes :

```yaml
# 1. Le notify ne correspond pas EXACTEMENT au nom du handler
notify: Recharger nginx        # doit matcher "name: Recharger nginx"

# 2. La tâche n'est pas "changed" (fichier identique) : normal !
#    Forcer si besoin :
- name: Forcer le rechargement
  ansible.builtin.command: nginx -s reload
  changed_when: true

# 3. Handler défini dans le mauvais play (les handlers sont par play)
# 4. Utiliser listen pour regrouper plusieurs notifies
handlers:
  - name: Recharger le frontal web
    listen: recharger_frontaux
    ansible.builtin.service:
      name: nginx
      state: reloaded
# notify: recharger_frontaux  (insensible au renommage du name)
```

## 75. Dépannage : playbook lent — diagnostic

```bash
# Identifier les tâches lentes
ANSIBLE_CALLBACKS_ENABLED=ansible.posix.profile_tasks ansible-playbook site.yml 2>&1 | tail -20

# Causes typiques et remèdes :
# - gather_facts sur des centaines d'hôtes  -> fact_caching (section 60)
# - apt update_cache à chaque tâche          -> cache_valid_time: 3600
# - forks = 5 par défaut                     -> forks = 30 dans ansible.cfg
# - pas de pipelining                        -> pipelining = True
# - git clone complet à chaque run           -> le module git est idempotent, vérifier version figée
# - handlers redémarrant en boucle           -> un seul notify, handlers en fin de play
```

## 76. Sauvegarde et restauration d'un nœud via Ansible

Exemple : sauvegarder `/etc` et la liste des paquets avant une opération risquée.

```yaml
---
- name: Snapshot pré-intervention
  hosts: all
  become: true
  gather_facts: false

  vars:
    dossier_sauvegarde: /srv/snapshots/{{ inventory_hostname }}

  tasks:
    - name: Créer le dossier de snapshot (sur le control node)
      ansible.builtin.file:
        path: "{{ dossier_sauvegarde }}"
        state: directory
        mode: "0700"
      delegate_to: localhost
      become: false

    - name: Archiver /etc
      community.general.archive:
        path: /etc
        dest: /tmp/etc_backup.tar.gz
        format: gz

    - name: Récupérer l'archive
      ansible.builtin.fetch:
        src: /tmp/etc_backup.tar.gz
        dest: "{{ dossier_sauvegarde }}/etc_backup.tar.gz"
        flat: true

    - name: Sauvegarder la liste des paquets
      ansible.builtin.command: dpkg --get-selections
      register: paquets
      changed_when: false

    - name: Écrire la liste des paquets
      ansible.builtin.copy:
        content: "{{ paquets.stdout }}"
        dest: "{{ dossier_sauvegarde }}/paquets.txt"
        mode: "0600"
      delegate_to: localhost
      become: false
```

Restauration paquets : `dpkg --set-selections < paquets.txt && apt-get dselect-upgrade`.

## 77. Les 12 erreurs classiques (et comment les éviter)

| # | Erreur | Symptôme | Remède |
|---|---|---|---|
| 1 | Secrets en clair dans Git | Fuite de mots de passe | Vault (sections 46–48) + `.gitignore` |
| 2 | `shell` au lieu d'un module | Non idempotent, `changed` à chaque run | Utiliser le module dédié + `changed_when` |
| 3 | `notify` qui ne matche pas le handler | Service jamais rechargé | Noms identiques ou `listen` (section 74) |
| 4 | `gather_facts: true` inutile | Playbook lent | `gather_facts: false` ou cache (section 60) |
| 5 | Pas de `--check --diff` avant prod | Changement destructeur surprise | Dry-run systématique + staging d'abord |
| 6 | `serial` oublié sur rolling update | Tout le parc redémarre en même temps | `serial: "25%"` + `max_fail_percentage` |
| 7 | Variables non quotées `{{ }}` | Erreur de parsing YAML | Toujours quoter : `"{{ var }}"` |
| 8 | `become` sans sudoers adapté | `Missing sudo password` | Tester `sudo -n true`, configurer sudoers |
| 9 | Inventaire prod/staging mélangé | Patch appliqué au mauvais env | Un fichier par environnement (section 12) |
| 10 | `ignore_errors: true` abusif | Échecs masqués, état inconnu | `block/rescue` ciblé, jamais global |
| 11 | Pas de `validate` sur les templates critiques | `sshd_config` invalide → lockout | `validate: "sshd -t -f %s"` (sections 22, 28) |
| 12 | `latest` / `upgrade: yes` sans contrôle | Version surprise en production | Épingler les versions, upgrades planifiés |

## 78. Bonnes pratiques production — checklist

- [ ] Chaque playbook commence par `--check --diff` puis passe par le staging.
- [ ] Handlers pour les restarts, jamais de `service restarted` direct dans les tâches
      (évite les redémarrages en boucle à chaque run).
- [ ] `validate` sur tout template critique (`sshd_config`, `nginx.conf`, `sudoers`).
- [ ] Idempotence vérifiée : deux runs successifs → le second affiche `changed=0`.
- [ ] Tags cohérents (`install`, `config`, `service`, `always`, `never`).
- [ ] `serial` + `max_fail_percentage` sur tout changement à impact (reboot, upgrade).
- [ ] Logs conservés 90 jours (`log_path` + rotation logrotate).
- [ ] Code relu (merge request) avant merge sur `main`.
- [ ] Versions épinglées : collections (`requirements.yml`), paquets critiques.
- [ ] `assert` en prérequis OS au début des playbooks sensibles.
- [ ] Pas de `shell`/`command` sans `changed_when`/`failed_when` explicites.
- [ ] Documentation : `README.md` par rôle, en-tête `ansible_managed` dans les templates.

## 79. Sécurité — checklist durcissement

- [ ] Vault pour tous les secrets ; mots de passe Vault distincts prod/staging.
- [ ] Fichier `~/.vault_pass` en `chmod 600`, hors Git, rotation au départ d'un équipier.
- [ ] Clé SSH dédiée à Ansible (`id_ed25519_ansible`), jamais la clé personnelle.
- [ ] `host_key_checking = True` en production.
- [ ] Utilisateur `deploy` avec sudo restreint si possible (pas `NOPASSWD: ALL`
      sans réflexion — voir section 58).
- [ ] `no_log: true` sur les tâches manipulant des secrets en clair transitoire :

```yaml
- name: Appel API avec token (ne pas logger)
  ansible.builtin.uri:
    url: https://api.example.lan/secret
    headers:
      Authorization: "Bearer {{ vault_api_token }}"
  no_log: true
```

- [ ] Audit régulier : `git log -p --all -S "password"` pour traquer les secrets
      ayant fuité dans l'historique.
- [ ] AWX/Controller : credentials en base chiffrée, RBAC, pas de Vault partagé
      par messagerie.

## 80. Mise à jour d'Ansible

```bash
# Via pipx (recommandé)
pipx upgrade ansible
ansible --version

# Épingler une version précise (reproductibilité)
pipx install --force ansible==10.7.0

# Via APT (Debian/Ubuntu)
sudo apt update && sudo apt install --only-upgrade ansible

# Après mise à jour : vérifier les dépréciations
ansible-playbook playbooks/site.yml --check 2>&1 | grep -i deprecat
```

Points d'attention entre versions majeures :

- Lire le **porting guide** (`docs.ansible.com` → Porting Guides) : suppressions
  de modules, renommages (`ansible.builtin.` stabilisé depuis la 2.10).
- Tester en staging avec `--check --diff` avant de basculer le control node de prod.
- Les collections évoluent indépendamment : `ansible-galaxy collection install -r
  collections/requirements.yml --upgrade`.

## 81. Pense-bête de poche

```bash
# --- Inventaire ---
ansible-inventory -i prod.ini --graph
ansible-inventory -i prod.ini --list | python3 -m json.tool | less

# --- Ad-hoc ---
ansible all -m ansible.builtin.ping
ansible web -b -m ansible.builtin.service -a "name=nginx state=reloaded"
ansible all -b -m ansible.builtin.apt -a "upgrade=dist update_cache=yes" --check --diff

# --- Playbooks ---
ansible-playbook site.yml --syntax-check
ansible-playbook site.yml --check --diff
ansible-playbook site.yml -l staging --tags config
ansible-playbook site.yml --vault-password-file ~/.vault_pass -f 30
ansible-playbook site.yml --step          # confirmation tâche par tâche
ansible-playbook site.yml --start-at-task "Déployer le vhost"

# --- Vault ---
ansible-vault create group_vars/all/vault.yml
ansible-vault edit group_vars/all/vault.yml
ansible-vault encrypt_string 'secret' --name 'token'

# --- Debug ---
ansible-playbook site.yml -vvv -l hote-en-panne
ansible hote -m ansible.builtin.setup -a "filter=ansible_distribution*"
ansible-doc ansible.builtin.template

# --- Galaxy ---
ansible-galaxy collection install -r collections/requirements.yml
ansible-galaxy role install -r roles/requirements.yml
```

## 82. Glossaire

| Terme | Définition |
|---|---|
| **Ad-hoc** | Commande Ansible unique, sans playbook |
| **Become** | Élévation de privilèges (sudo, su…) |
| **Callback** | Plugin qui traite la sortie / les événements d'exécution |
| **Check mode** | Dry-run (`--check`) : simule sans modifier |
| **Collection** | Paquet distribuable : modules, plugins, rôles |
| **Control node** | Machine où Ansible est installé et lancé |
| **Diff** | Affichage des différences (`--diff`) avant/après |
| **Facts** | Informations collectées sur les nœuds gérés |
| **Forks** | Processus parallèles d'exécution (défaut : 5) |
| **FQCN** | Fully Qualified Collection Name (`ansible.builtin.apt`) |
| **Galaxy** | Hub communautaire de rôles et collections |
| **Gather facts** | Phase de collecte des facts en début de play |
| **Handler** | Tâche exécutée une fois en fin de play si notifiée |
| **Host vars** | Variables spécifiques à un hôte |
| **Group vars** | Variables spécifiques à un groupe |
| **Idempotence** | Rejouer sans effet de bord si l'état est déjà correct |
| **Inventory** | Inventaire : hôtes, groupes, variables de connexion |
| **Jinja2** | Moteur de templates (`{{ }}`, `{% %}`) |
| **Managed node** | Machine gérée via SSH/WinRM, sans agent |
| **Module** | Unité d'action idempotente |
| **Notify** | Déclenchement d'un handler depuis une tâche |
| **Play** | Ensemble de tâches appliquées à des hôtes |
| **Playbook** | Fichier YAML contenant un ou plusieurs plays |
| **Role** | Regroupement réutilisable : tasks, handlers, templates… |
| **Serial** | Exécution par vagues successives d'hôtes |
| **Tags** | Étiquettes pour exécuter un sous-ensemble de tâches |
| **Task** | Appel d'un module avec ses paramètres |
| **Throttle** | Limite de concurrence sur une tâche |
| **Vault** | Chiffrement des variables sensibles |
| **AWX / Controller** | Interface web, ordonnanceur et RBAC au-dessus d'Ansible |

## 83. Quiz — 10 questions

1. Quelle est la différence entre `ansible` et `ansible-core` ?
2. Pourquoi Ansible est-il qualifié d'« agentless » et quel protocole utilise-t-il
   par défaut sur Linux ?
3. Citez les 4 niveaux de précédence des variables à connaître par cœur, du moins
   au plus prioritaire.
4. Que se passe-t-il si deux tâches différentes notifient le même handler et que
   les deux sont `changed` ?
5. À quoi sert le paramètre `validate` du module `template`, et sur quel fichier
   critique l'utiliseriez-vous en priorité ?
6. Expliquez la différence entre `serial: "25%"` et `throttle: 3`.
7. Votre playbook échoue avec `AnsibleUndefinedVariable`. Citez 3 vérifications
   dans l'ordre.
8. Pourquoi faut-il éviter `ignore_errors: true` sur un play entier, et que
   proposer à la place ?
9. Où stocker le mot de passe Vault pour une exécution automatisée (CI), et avec
   quels droits ?
10. Votre second run d'un playbook affiche encore des `changed`. Est-ce normal ?
    Que devez-vous vérifier ?

## 84. Quiz — réponses

1. `ansible-core` est le moteur seul (playbooks, modules `ansible.builtin`) ;
   `ansible` (paquet communautaire) ajoute des dizaines de collections. En
   production, partez du core et n'ajoutez que le nécessaire.
2. Aucun agent à installer sur les cibles : Ansible pousse des scripts Python via
   **SSH** (SFTP/SCP) et récupère le résultat en JSON. Sur Windows : WinRM.
3. `defaults/` du rôle < `group_vars/` < `host_vars/` < `--extra-vars` (`-e`).
4. Le handler ne s'exécute **qu'une seule fois** en fin de play, quel que soit le
   nombre de notifications.
5. `validate` exécute une commande (ex. `sshd -t -f %s`, `nginx -t -c %s`) sur le
   fichier généré **avant** de remplacer la destination : si la validation échoue,
   le fichier en production n'est pas touché. Priorité : `sshd_config` (un fichier
   invalide = lockout SSH).
6. `serial` découpe les **hôtes** en vagues successives (rolling update) ;
   `throttle` limite le nombre d'exécutions **simultanées d'une tâche** même avec
   beaucoup de forks (ex. : ne pas saturer une API).
7. (a) `ansible -m debug -a "var=ma_variable"` pour voir la valeur réelle ;
   (b) vérifier l'orthographe et la portée (host_vars vs group_vars) ;
   (c) ajouter `| default(...)` ou `is defined` si la variable est optionnelle.
8. Parce qu'il masque tous les échecs, y compris les vrais problèmes, et laisse le
   parc dans un état inconnu. À la place : `block/rescue/always` ciblé sur les
   tâches à risque, avec alerte explicite dans le `rescue`.
9. Dans un fichier dédié (ex. `~/.vault_pass`) en `chmod 600`, hors Git, de
   préférence fourni par un gestionnaire de secrets ou une variable CI masquée —
   jamais dans le dépôt ni en clair dans un script.
10. Non : un playbook idempotent doit afficher `changed=0` au second run (hors
    tâches explicitement non idempotentes comme `command` sans `changed_when`).
    Vérifiez les tâches `changed` : utilisez un module dédié, ajoutez
    `changed_when`/`creates`, ou corrigez le template qui génère un diff à chaque fois.

## 85. Pour aller plus loin

- **Documentation officielle** : `docs.ansible.com` — le porting guide à chaque
  montée de version, et `ansible-doc` en local (plus rapide que le web).
- **Molecule** : tester vos rôles dans des conteneurs (`pipx install molecule
  molecule-plugins[docker]`) avant de toucher au staging.
- **Ansible Lint** : `pipx install ansible-lint` puis `ansible-lint playbooks/` —
  intégrez-le en CI pour uniformiser le style d'équipe.
- **AWX** : quand plusieurs opérateurs et de la planification entrent en jeu
  (section 64).
- **Terraform + Ansible** : Terraform provisionne (VM, réseau), Ansible configure —
  le duo standard, pas des concurrents.
- **Communauté** : Galaxy pour les rôles/collections, forum `forum.ansible.com`.
- **Prochaine étape concrète** : prenez le cas pratique 1 (section 65), adaptez-le
  à votre intranet, versionnez-le en Git, et planifiez le patching (section 68)
  via un cron sur le control node ou un job AWX.

---

*Guide rédigé pour un usage production — sysadmin. Versions : Ansible Core 2.15 → 2.19,
Debian 12/13, Ubuntu 22.04/24.04. Relisez le porting guide officiel à chaque montée
de version majeure.*
