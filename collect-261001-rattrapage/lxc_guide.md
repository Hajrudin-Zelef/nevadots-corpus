# LXC & LXD — Guide ultra-complet

> **Public :** administrateurs systèmes, chefs de service, utilisateurs Proxmox VE.
> **Objectif :** maîtriser les conteneurs Linux de A à Z — installation, réseau, stockage,
> sécurité, production, supervision, sauvegarde, dépannage — avec un lien permanent
> vers Proxmox VE (l'outil préféré de Zelef).
> **Convention :** les commandes préfixées `#` s'exécutent en root sur l'hôte,
> celles préfixées `$` en utilisateur simple. À l'intérieur d'un conteneur, on note `[ct] #`.

---

## 0. Comment utiliser ce guide

- Lisez les sections 1 à 10 pour démarrer en une heure.
- Les sections 11 à 50 couvrent le quotidien et l'exploitation.
- Les sections 51 à 74 traitent production, sécurité et Proxmox.
- Les sections 75 à 86 sont la pratique : cas concrets, erreurs, dépannage, quiz.
- Le **pense-bête de poche** (section 83) est imprimable pour le bureau.

---

## 1. Introduction — Pourquoi LXC ?

LXC (*Linux Containers*) est la technologie de conteneurisation native du noyau Linux :
des processus isolés grâce aux **namespaces** (pid, net, mnt, uts, ipc, user, cgroup)
et limités en ressources par les **cgroups**. Pas d'hyperviseur, pas de noyau invité :
le conteneur partage le noyau de l'hôte.

**Ce que ça change concrètement :**

| Aspect | Machine virtuelle | Conteneur LXC |
|---|---|---|
| Démarrage | 30 s à 2 min | < 2 secondes |
| Surcoût mémoire | Noyau invité complet (~200-500 Mo) | Quelques Mo |
| Densité | Dizaines par hôte | Centaines par hôte |
| Isolation | Totale (noyau séparé) | Forte (noyau partagé) |
| Cas d'usage | OS hétérogènes, noyaux spécifiques | Services Linux, densité, rapidité |

**En une phrase :** là où une VM virtualise du matériel, LXC virtualise un système
d'exploitation — c'est plus léger, plus rapide, et largement suffisant pour la
majorité des services Linux en production.

---

## 2. LXC vs LXD vs Docker vs Proxmox — Le grand comparatif

| Critère | LXC (pur) | LXD | Docker / Podman | Proxmox VE (LXC) |
|---|---|---|---|---|
| Nature | Bibliothèque + outils bas niveau | Gestionnaire de conteneurs système | Conteneurs applicatifs | Hyperviseur + conteneurs |
| Unité gérée | Conteneur brut | Conteneur système complet | Image applicative | CT (via `pct`) + VM (via `qm`) |
| Réseau intégré | Non (manuel) | Oui (bridges, OVN) | Oui (bridge docker0) | Oui (vmbr0, SDN) |
| Stockage intégré | Non | Oui (pools ZFS/btrfs/LVM/dir) | Volumes/drivers | Oui (ZFS, LVM, Ceph…) |
| Snapshots | Manuel (selon FS) | Natifs, planifiables | Layers d'image | Natifs (selon stockage) |
| Clustering | Non | Oui (natif) | Swarm/K8s (externe) | Oui (corosync) |
| Idéal pour | Embarqué, cas spécifiques | Infra système, VPS-like | Microservices, CI/CD | Datacenter complet |

**Retenez :** LXC = le moteur, LXD = la voiture complète construite autour.
Proxmox VE utilise LXC en interne via sa propre couche (`pct`) — voir sections 71-74.

---

## 3. Que choisir : LXC, LXD, Docker ou VM ?

**Arbre de décision :**

```
Besoin d'un autre noyau / OS non-Linux (Windows, BSD) ?
└── OUI → Machine virtuelle (KVM / Proxmox VE)
└── NON (Linux uniquement)
    ├── Un seul processus applicatif, image portable, CI/CD ?
    │   └── OUI → Docker / Podman
    └── NON → Un "mini-serveur" complet (systemd, ssh, plusieurs services) ?
        ├── Gestion centralisée type datacenter ? → Proxmox VE (LXC via pct)
        └── Hôte Debian/Ubuntu simple, automatisation ? → LXD
```

**Règle pratique de Zelef :**

- **Proxmox VE** = la plateforme de production (cluster, HA, backups intégrés).
- **LXD** = parfait sur un hôte isolé, un labo, un serveur dédié sans Proxmox,
  ou quand on veut l'API REST moderne et le clustering léger sans tout Proxmox.
- **Docker** = à l'intérieur d'un conteneur LXC si besoin (nesting, section 51),
  ou directement sur l'hôte pour des applis 12-factor.

---

## 4. Prérequis système et noyau

**Matériel :**

- CPU 64 bits (x86_64 ou ARM64), 2 cœurs minimum, 4 recommandés.
- RAM : 4 Go minimum, 8 Go+ recommandés (les conteneurs sont légers, mais ZFS adore la RAM).
- Disque : 20 Go minimum ; SSD fortement recommandé.
- Virtualisation matérielle **non requise** (pas d'hyperviseur).

**Noyau :** Linux ≥ 5.4 recommandé (cgroups v2 bien supportés à partir de 5.x ;
Ubuntu 22.04/24.04 et Debian 11/12/13 conviennent).

**Vérifications préalables :**

```bash
# Version du noyau
uname -r

# Architecture
uname -m

# cgroups v2 actif ? (doit afficher "cgroup2fs")
stat -fc %T /sys/fs/cgroup

# Vérifier le support LXC du noyau (si lxc installé)
lxc-checkconfig
```

**Sortie attendue de `lxc-checkconfig` :** la plupart des lignes en vert `enabled`.
Les lignes `missing` sur `Cgroup v1 ...` sont normales sur un système cgroup v2.

**Espace disque pour les images :** prévoir ~500 Mo par image de base
(Ubuntu ~ 200 Mo, Debian ~ 150 Mo téléchargés, plus une fois décompressés).

---

## 5. Installation sur Ubuntu — via snap (méthode recommandée)

Sur Ubuntu, LXD est distribué et maintenu via **snap** (version toujours à jour).

```bash
# Mise à jour du système
sudo apt update && sudo apt upgrade -y

# Installation de LXD
sudo snap install lxd

# Vérification
lxd --version
lxc --version
```

**Ajouter votre utilisateur au groupe `lxd`** (évite `sudo` à chaque commande) :

```bash
sudo usermod -aG lxd "$USER"
# Puis DÉCONNEXION / RECONNEXION (ou newgrp lxd) pour prendre en compte le groupe
newgrp lxd
```

> ⚠️ **Sécurité :** un membre du groupe `lxd` équivaut à root sur l'hôte
> (un conteneur privilégié peut monter le disque hôte). Ne mettez dans ce groupe
> que des administrateurs de confiance. Voir section 46.

**Canal snap :** par défaut `latest/stable`. Pour une version LTS figée :

```bash
sudo snap refresh lxd --channel=5.21/stable   # exemple : branche LTS 5.21
```

---

## 6. Installation sur Debian — dépôts et alternatives

Debian propose LXD dans ses dépôts (version parfois en retard d'une release) :

```bash
sudo apt update && sudo apt upgrade -y

# Option A : paquet Debian (simple, version du dépôt)
sudo apt install -y lxd lxd-client

# Vérification
lxd --version
```

**Option B : snap sur Debian** (pour la dernière version, comme Ubuntu) :

```bash
sudo apt install -y snapd
sudo snap install core
sudo snap install lxd
```

**Comparatif :**

| Méthode | Avantages | Inconvénients |
|---|---|---|
| `apt install lxd` | Intégré au système, mises à jour avec APT | Version en retard (ex. 5.x sur Debian 12) |
| `snap install lxd` | Toujours à jour, canaux LTS | Dépendance snapd, chemins `/snap` |

**Recommandation :** en production, préférez le **canal LTS du snap**
(stabilité + correctifs de sécurité), ou le paquet APT si votre politique
interdit snapd.

---

## 7. Installation de LXC pur (sans LXD) — pour comprendre les fondations

LXD repose sur LXC. Installer LXC seul est utile pour comprendre, ou pour des
usages embarqués/minimaux.

```bash
# Debian / Ubuntu
sudo apt install -y lxc lxc-templates uidmap

# Vérifier la configuration noyau
sudo lxc-checkconfig

# Créer un conteneur Debian (template download)
sudo lxc-create -t download -n c1 -- --dist debian --release bookworm --arch amd64

# Démarrer / attacher / arrêter
sudo lxc-start -n c1
sudo lxc-attach -n c1
sudo lxc-stop -n c1

# Lister
sudo lxc-ls --fancy
```

**Quand utiliser LXC pur ?**

- Systèmes très contraints, scripts sur mesure.
- Comprendre ce que LXD automatise (réseau, stockage, images).
- **En pratique :** préférez LXD pour tout usage serveur standard.

---

## 8. `lxd init` — Initialisation interactive

```bash
sudo lxd init
```

**Déroulé typique (réponses recommandées entre crochets) :**

```
Would you like to use LXD clustering? (yes/no) [default=no]: no
Do you want to configure a new storage pool? (yes/no) [default=yes]: yes
Name of the new storage pool [default=default]: default
Name of the storage backend to use (dir, lvm, zfs, btrfs) [default=zfs]: zfs
Create a new ZFS pool? (yes/no) [default=yes]: yes
Would you like to use an existing empty block device? (yes/no) [default=no]: no
Size in GiB of the new loop device (1GiB minimum) [default=30GiB]: 50GiB
Would you like to connect to a MAAS server? (yes/no) [default=no]: no
Would you like to create a new local network bridge? (yes/no) [default=yes]: yes
What should the new bridge be called? [default=lxdbr0]: lxdbr0
What IPv4 address should be used? (CIDR subnet notation, "auto" or "none") [default=auto]: 10.10.10.1/24
Would you like LXD to NAT IPv4 traffic on this bridge? [default=yes]: yes
What IPv6 address should be used? (CIDR subnet notation, "auto" or "none") [default=auto]: none
Would you like the LXD server to be available over the network? (yes/no) [default=no]: no
Would you like stale cached images to be updated automatically? (yes/no) [default=yes]: yes
Would you like a YAML "lxd init" preseed to be printed? (yes/no) [default=no]: no
```

**Points d'attention :**

- **Backend `zfs`** : le meilleur choix (snapshots instantanés, copies COW).
  En loop device c'est parfait pour tester ; en production, utilisez un vrai
  disque ou une partition (section 34).
- **`lxdbr0`** : bridge NATé privé. Les conteneurs obtiennent des IP via DHCP
  interne (dnsmasq géré par LXD).
- **Accès réseau au daemon** : laissez `no` sauf besoin distant explicite
  (dans ce cas, TLS + mot de passe, section 69).

---

## 9. `lxd init` — Mode preseed (reproductible, scriptable)

Pour automatiser (Ansible, cloud-init, documentation d'exploitation) :

```bash
cat <<'EOF' > /tmp/lxd-preseed.yaml
config:
  core.https_address: '[::]:8443'
  core.trust_password: "ChangeMe_TresLong_MotDePasse_2026"
networks:
- name: lxdbr0
  type: bridge
  config:
    ipv4.address: 10.10.10.1/24
    ipv4.nat: "true"
    ipv6.address: none
storage_pools:
- name: default
  driver: zfs
  config:
    size: 50GiB
profiles:
- name: default
  devices:
    eth0:
      name: eth0
      network: lxdbr0
      type: nic
    root:
      path: /
      pool: default
      type: disk
cluster: null
EOF

cat /tmp/lxd-preseed.yaml | sudo lxd init --preseed
```

> 🔐 Ne versionnez **jamais** un preseed contenant `trust_password` dans Git.
> Utilisez Ansible Vault ou un coffre de secrets, puis supprimez le fichier.

**Vérification post-init :**

```bash
lxc network list
lxc storage list
lxc profile show default
```

---

## 10. Première vérification de santé

```bash
# Le daemon répond ?
lxc list

# Réseau : le bridge existe avec son IP
ip addr show lxdbr0

# Stockage : le pool est monté
lxc storage info default
zfs list 2>/dev/null | head

# Version et extensions
lxc info | head -30
```

**Checklist de santé initiale :**

- [ ] `lxc list` ne renvoie pas d'erreur de connexion au socket
- [ ] `lxdbr0` existe avec l'IP configurée
- [ ] Le pool `default` apparaît dans `lxc storage list`
- [ ] Le profil `default` attache `eth0` à `lxdbr0` et `root` au pool
- [ ] `lxc image list` fonctionne (même vide au début)

Si `lxc list` échoue avec une erreur de permission : vérifiez l'appartenance
au groupe `lxd` (section 5) ou utilisez `sudo`.

---

## 11. Images — Remotes et liste

LXD télécharge des images depuis des **remotes** (serveurs d'images).

```bash
# Lister les remotes configurées
lxc remote list

# Remotes par défaut :
#   ubuntu:         images Ubuntu officielles (Canonical)
#   ubuntu-daily:   builds quotidiens Ubuntu
#   images:         images communautaires linuxcontainers.org
#   local:          (défini par défaut) votre serveur local
```

```bash
# Lister les images Ubuntu 24.04 disponibles
lxc image list ubuntu: 24.04

# Chercher Debian
lxc image list images: debian

# Détails d'une image
lxc image info ubuntu:24.04
```

**Types d'images :**

| Type | Description | Usage |
|---|---|---|
| `container` | Image de conteneur système | `lxc launch` |
| `virtual-machine` | Image de VM (LXD gère aussi des VM !) | `lxc launch --vm` |

> 💡 Oui, **LXD gère aussi des machines virtuelles** (`--vm`) via QEMU.
> Ce guide se concentre sur les conteneurs, mais sachez que l'option existe
> pour les cas nécessitant un noyau séparé.

---

## 12. Images — Alias, architectures et cycle de vie

```bash
# Alias pratiques (noms courts)
lxc launch ubuntu:24.04 c-web1        # alias "24.04"
lxc launch ubuntu:22.04 c-web2
lxc launch images:debian/12 c-db1
lxc launch images:alpine/3.20 c-tiny1

# Architecture explicite (utile sur ARM)
lxc launch ubuntu:24.04 c-arm1 --target-arch aarch64
```

**Gestion locale des images :**

```bash
# Lister les images en cache local
lxc image list

# Copier une image d'un remote vers le local avec un alias
lxc image copy ubuntu:24.04 local: --alias ubuntu-2404-base

# Renommer / aliaser
lxc image alias create ubuntu-2404-base ubuntu-lts

# Supprimer une image locale
lxc image delete ubuntu-2404-base

# Forcer le rafraîchissement
lxc image refresh ubuntu:24.04
```

**Expiration du cache :** `images.remote_cache_expiry` (défaut 10 jours) et
`images.auto_update_interval`. Les images inutilisées sont nettoyées
automatiquement — ne vous inquiétez pas de voir le cache grossir.

---

## 13. `lxc launch` — Votre premier conteneur

```bash
# Lancement simple (profil default : réseau lxdbr0 + disque root)
lxc launch ubuntu:24.04 c1

# Suivre le démarrage
lxc list
```

**Sortie de `lxc list` :**

```
+------+---------+---------------------+-----------------------------------------------+-----------+-----------+
| NAME |  STATE  |        IPV4         |                     IPV6                      |   TYPE    | SNAPSHOTS |
+------+---------+---------------------+-----------------------------------------------+-----------+-----------+
| c1   | RUNNING | 10.10.10.52 (eth0)  | fd42::... (eth0)                              | CONTAINER | 0         |
+------+---------+---------------------+-----------------------------------------------+-----------+-----------+
```

**Options utiles au lancement :**

```bash
# Avec un nom d'hôte et des profils spécifiques
lxc launch ubuntu:24.04 web1 --profile default --profile web

# Avec des limites directes (config)
lxc launch ubuntu:24.04 db1 -c limits.cpu=2 -c limits.memory=4GiB

# Avec une config réseau spécifique
lxc launch ubuntu:24.04 c2 -d eth0,ipv4.address=10.10.10.50

# En mode "frozen" (créé mais pas démarré)
lxc init ubuntu:24.04 c3
lxc start c3
```

**Différence `launch` vs `init` :** `launch` = `init` + `start`.
Utilisez `init` quand vous voulez configurer avant le premier démarrage.

---

## 14. Cycle de vie — start, stop, restart, delete, freeze

```bash
lxc start c1                 # démarrer
lxc stop c1                  # arrêt propre (SIGPWR → systemd → timeout puis SIGKILL)
lxc stop c1 --force          # arrêt brutal immédiat
lxc restart c1               # redémarrage
lxc restart c1 --force       # redémarrage brutal
lxc freeze c1                # suspendre (gel des processus, mémoire conservée)
lxc unfreeze c1              # dégeler
lxc delete c1                # supprimer (le conteneur doit être arrêté)
lxc delete c1 --force        # supprimer même si en cours d'exécution
```

**Comportement de `lxc stop` :** LXD envoie un signal d'arrêt propre et attend
`boot.host_shutdown_timeout` (défaut 30s) avant de forcer. Pour des bases de
données, augmentez ce timeout :

```bash
lxc config set c-db1 boot.host_shutdown_timeout 120
```

**Suppression en masse :**

```bash
# Supprimer tous les conteneurs arrêtés (attention !)
lxc list -c n -f csv | xargs -r -n1 lxc delete --force
```

---

## 15. `lxc exec` — Exécuter des commandes dans le conteneur

```bash
# Commande simple (non interactive)
lxc exec c1 -- ls -la /root

# Avec variables d'environnement
lxc exec c1 --env MODE=prod -- ./deploy.sh

# Mode interactif : shell
lxc exec c1 -- bash

# En tant qu'utilisateur non-root
lxc exec c1 --user 1000 --group 1000 -- whoami
lxc exec c1 -- su - deploy -c "whoami"
```

**Subtilités :**

- Le `--` sépare les options `lxc` de la commande à exécuter (obligatoire
  si la commande commence par `-`).
- Sans TTY alloué, utilisez `--mode non-interactive` pour forcer (utile
  dans les scripts/Ansible).
- `lxc exec` passe par l'API LXD : fonctionne même si le réseau du
  conteneur est cassé (gros avantage pour le dépannage !).

**Équivalent Proxmox :** `pct exec <vmid> -- <commande>` (section 71).

---

## 16. `lxc shell` — Accès shell simplifié

```bash
# Ouvre un shell (bash si dispo, sinon sh)
lxc shell c1

# Équivaut à :
lxc exec c1 -- bash
```

**Choisir le shell :**

```bash
lxc exec c1 -- sh      # minimaliste, toujours présent
lxc exec c1 -- bash    # confort
lxc exec c1 -- zsh     # si installé dans le conteneur
```

**Astuce :** définissez un alias shell pour vos conteneurs fréquents :

```bash
# ~/.bashrc
alias c1sh='lxc exec c1 -- bash'
alias c1root='lxc exec c1 -- su -'
```

---

## 17. `lxc info` / `lxc list` — Inspection

```bash
# Vue d'ensemble tabulaire
lxc list

# Colonnes personnalisées
lxc list -c n,s,4,P,profiles
# n=name s=state 4=ipv4 P=pid profiles=profils

# Format CSV (pour scripts)
lxc list -f csv -c n,s

# Format JSON (pour automatisation)
lxc list -f json | jq '.[].name'

# Détails d'un conteneur
lxc info c1

# Détails + état étendu (processus, mémoire...)
lxc info c1 --show-log
```

**Filtrer :**

```bash
lxc list status=running          # seulement les démarrés
lxc list "web.*"                 # par motif de nom
lxc list -c n --fast             # rapide (sans résolution IP)
```

---

## 18. Fichiers — `lxc file push/pull/edit`

```bash
# Envoyer un fichier vers le conteneur
lxc file push ./nginx.conf c1/etc/nginx/nginx.conf

# Avec propriétaire et permissions
lxc file push --uid 0 --gid 0 --mode 0644 ./app.conf c1/etc/app/app.conf

# Récupérer un fichier depuis le conteneur
lxc file pull c1/etc/nginx/nginx.conf ./nginx-c1.conf

# Récursif (dossier)
lxc file push -r ./site/ c1/var/www/html/ --create-dirs
lxc file pull -r c1/var/log/nginx/ ./logs-c1/

# Éditer directement (ouvre $EDITOR)
lxc file edit c1/etc/hosts

# Créer un dossier
lxc file push --create-dirs /dev/null c1/opt/app/placeholder
```

**Cas d'usage typiques :**

- Déployer une config sans SSH ni Ansible pour un petit parc.
- Récupérer des logs d'un conteneur au réseau cassé.
- **Équivalent Proxmox :** `pct push` / `pct pull` (section 71).

---

## 19. Configuration — `lxc config`

Toute la configuration d'un conteneur est une paire clé/valeur :

```bash
# Voir toute la config (fusionnée : profils + locale)
lxc config show c1 --expanded

# Voir seulement la config locale (sans l'héritage des profils)
lxc config show c1

# Définir une valeur
lxc config set c1 limits.memory 2GiB

# Supprimer une valeur (retour à l'héritage)
lxc config unset c1 limits.memory

# Éditer en YAML dans $EDITOR
lxc config edit c1

# Métadonnées
lxc config set c1 user.commentaire "Serveur web production"
lxc config get c1 user.commentaire
```

**Clés les plus utilisées :**

| Clé | Exemple | Effet |
|---|---|---|
| `limits.cpu` | `2` ou `0-3` | CPUs alloués |
| `limits.memory` | `4GiB` | RAM max |
| `limits.memory.swap` | `false` | Interdire le swap |
| `boot.autostart` | `true` | Démarrage auto avec l'hôte |
| `boot.autostart.delay` | `30` | Délai avant démarrage (s) |
| `boot.autostart.priority` | `10` | Ordre de démarrage |
| `security.nesting` | `true` | Autoriser Docker dans LXC |
| `security.privileged` | `true` | Conteneur privilégié ⚠️ |
| `snapshots.schedule` | `@daily` | Snapshots automatiques |
| `snapshots.expiry` | `7d` | Expiration des snapshots |

---

## 20. Profils — Le concept

Un **profil** est un ensemble réutilisable de configuration (devices + config)
appliqué à plusieurs conteneurs. C'est l'équivalent des "rôles" Ansible ou des
templates Proxmox.

```
┌──────────┐   ┌──────────� |   ┌──────────┐
│ profil   │   │ profil   │   │ profil   │
│ default  │ + │ web      │ + │ limits-m │ = config finale de c1
└──────────┘   └──────────┘   └──────────┘
     (réseau + disque)   (nginx, ports)   (2 CPU / 4 Go)
```

**Ordre d'application :** les profils s'appliquent dans l'ordre de la liste ;
la **config locale** du conteneur a toujours le dernier mot.

```bash
# Voir les profils d'un conteneur
lxc config show c1 | grep -A5 profiles

# Changer l'ordre / la liste des profils
lxc profile assign c1 default,web,limits-m
```

---

## 21. Profils — Créer et éditer

```bash
# Créer un profil vide
lxc profile create web

# Éditer (ouvre $EDITOR avec un squelette YAML)
lxc profile edit web
```

**Exemple : profil `web` complet :**

```yaml
config:
  limits.cpu: "2"
  limits.memory: 2GiB
  boot.autostart: "true"
  user.role: webserver
description: Profil serveurs web mutualises
devices:
  eth0:
    name: eth0
    network: lxdbr0
    type: nic
  root:
    path: /
    pool: default
    type: disk
    size: 20GiB
  http:
    connect: tcp:127.0.0.1:80
    listen: tcp:0.0.0.0:8080
    type: proxy
name: web
```

**Autres commandes :**

```bash
lxc profile list
lxc profile show web
lxc profile rename web web-prod
lxc profile copy web web-staging
lxc profile delete web-staging
```

---

## 22. Profils — Appliquer, exemples concrets

```bash
# Appliquer au lancement
lxc launch ubuntu:24.04 web1 --profile default --profile web

# Appliquer à un conteneur existant (à chaud pour la plupart des clés)
lxc profile add c1 web
lxc profile remove c1 web

# Remplacer toute la liste
lxc profile assign c1 default,db
```

**Bibliothèque de profils suggérée pour une équipe :**

| Profil | Contenu | Usage |
|---|---|---|
| `default` | réseau + disque de base | tout conteneur |
| `limits-s/m/l/xl` | CPU/RAM calibrés | gabarits de taille |
| `web` | limites + proxy 80/443 | serveurs web |
| `db` | RAM élevée, I/O prioritaires, shutdown long | bases de données |
| `docker-host` | `security.nesting=true`, disque 50 Go | hôtes Docker |
| `no-ipv6` | `ipv6.address=none` | réseau IPv4 only |

> ✅ **Bonne pratique :** tout ce qui est répété 3 fois devient un profil.
> Versionnez vos profils (`lxc profile show web > profiles/web.yaml` dans Git).

---

## 23. Réseau — Concepts (bridge, macvlan, routed, physical)

| Type | Description | IP du conteneur | Cas d'usage |
|---|---|---|---|
| `bridged` | Attaché à un bridge Linux | DHCP du bridge ou statique | Standard, recommandé |
| `macvlan` | Interface virtuelle sur l'interface physique | DHCP du LAN | IP LAN directe |
| `routed` | Routage via l'hôte (pas de bridge) | Statique, routée | Environnements sans bridge |
| `physical` | Interface physique dédiée au conteneur | Selon réseau | SR-IOV, cas avancés |
| `ovn` | Réseau virtuel OVN | Géré par OVN | Overlay multi-hôtes |
| `proxy` | Redirection de ports (pas un vrai NIC) | — | Exposer un port |

**Recommandation :** `bridged` sur `lxdbr0` (ou votre bridge) dans 90 % des cas.
`macvlan` quand le conteneur doit apparaître comme une machine du LAN
(attention : l'hôte ne peut pas joindre le conteneur en macvlan — limitation
du noyau, voir section 26).

---

## 24. Réseau — Le bridge `lxdbr0` par défaut

Créé par `lxd init`, c'est un bridge Linux privé avec NAT :

```bash
# Détails
lxc network show lxdbr0

# Voir les baux DHCP attribués
lxc network show lxdbr0 | grep -A20 "Used by"

# Leases DHCP (via dnsmasq géré par LXD)
cat /var/snap/lxd/common/lxd/networks/lxdbr0/dnsmasq.leases 2>/dev/null
# ou sur install APT :
cat /var/lib/lxd/networks/lxdbr0/dnsmasq.leases 2>/dev/null
```

**Ce que fait LXD pour vous :**

- Crée le bridge et lui assigne l'IP passerelle (ex. `10.10.10.1/24`).
- Lance `dnsmasq` : DHCP + DNS local (résolution des noms de conteneurs !).
- Configure le NAT (iptables/nftables) vers l'extérieur.

**Test DNS intégré :**

```bash
lxc exec c1 -- ping -c2 c2   # résout le nom d'un autre conteneur du bridge !
```

---

## 25. Réseau — Créer son propre bridge

Pour séparer les environnements (prod / dev / dmz) :

```bash
# Bridge isolé sans NAT (réseau privé pur)
lxc network create dmz0 \
  ipv4.address=192.168.50.1/24 \
  ipv4.nat=false \
  ipv6.address=none

# Bridge avec NAT et DHCP custom
lxc network create prod0 \
  ipv4.address=10.20.0.1/24 \
  ipv4.nat=true \
  ipv4.dhcp.ranges=10.20.0.100-10.20.0.200 \
  ipv6.address=none

# Attacher un conteneur
lxc network attach prod0 c1 eth1
# ou via device :
lxc config device add c1 eth1 nic network=prod0 name=eth1
```

**Options utiles :**

```bash
lxc network set prod0 dns.domain prod.lan
lxc network set prod0 dns.mode managed
lxc network set prod0 ipv4.firewall true
```

---

## 26. Réseau — macvlan (IP directe sur le LAN)

Le conteneur obtient une IP du **LAN physique** (comme une VM bridgée Proxmox
sur vmbr0) :

```bash
# Créer un profil macvlan
lxc profile create lan
lxc profile device add lan eth0 nic \
  nictype=macvlan \
  parent=enp0s3 \
  name=eth0

# Lancer avec ce profil
lxc launch ubuntu:24.04 srv1 --profile lan
```

> ⚠️ **Limitation noyau :** en macvlan, l'**hôte ne peut pas communiquer**
> avec ses propres conteneurs macvlan (le parent filtre). Les conteneurs se
> voient entre eux, le LAN les voit, mais pas l'hôte. Si vous avez besoin
> d'hôte ↔ conteneur, préférez un bridge avec une interface physique attachée.

**Vérifier le parent :** `parent=` doit être l'interface physique réelle
(`ip link show` pour lister).

---

## 27. Réseau — VLAN (tagged 802.1Q)

```bash
# 1. Créer l'interface VLAN sur l'hôte (exemple VLAN 20 sur enp0s3)
sudo ip link add link enp0s3 name enp0s3.20 type vlan id 20
sudo ip link set enp0s3.20 up

# 2. Rendre persistant via netplan (/etc/netplan/60-vlan.yaml)
```

```yaml
network:
  version: 2
  ethernets:
    enp0s3: {}
  vlans:
    enp0s3.20:
      id: 20
      link: enp0s3
```

```bash
# 3. Bridge LXD sur le VLAN
lxc network create vlan20 \
  --type=bridge \
  bridge.external_interfaces=enp0s3.20 \
  ipv4.address=none \
  ipv6.address=none

# 4. Attacher
lxc config device add c1 eth1 nic network=vlan20 name=eth1
```

Le conteneur reçoit alors une IP du DHCP du VLAN 20 (celui de votre infra).

---

## 28. Réseau — IP statiques et DHCP

**IP statique gérée par LXD (recommandé) :** LXD réserve l'IP dans dnsmasq,
le conteneur reste en DHCP mais reçoit toujours la même IP :

```bash
# À chaud (redémarrage du conteneur requis pour prise en compte DHCP)
lxc config device set c1 eth0 ipv4.address 10.10.10.50
lxc restart c1
```

**IP statique dans le conteneur (netplan) :** possible mais LXD ne la connaît
pas (le `lxc list` n'affichera pas l'IP). Préférez la méthode LXD.

**Réservations DHCP visibles :**

```bash
lxc network show lxdbr0 | grep -B2 -A2 "10.10.10.50"
```

**Désactiver le DHCP sur un réseau :**

```bash
lxc network set lxdbr0 ipv4.dhcp false
```

---

## 29. Réseau — NAT et port forwarding (device proxy)

Pour exposer un service d'un conteneur vers l'hôte ou le LAN **sans macvlan** :

```bash
# Rediriger le port 8080 de l'hôte vers le port 80 du conteneur
lxc config device add c1 http proxy \
  listen=tcp:0.0.0.0:8080 \
  connect=tcp:127.0.0.1:80

# HTTPS
lxc config device add c1 https proxy \
  listen=tcp:0.0.0.0:8443 \
  connect=tcp:127.0.0.1:443

# Restreindre l'écoute à localhost (admin seulement)
lxc config device add c1 ssh-admin proxy \
  listen=tcp:127.0.0.1:2222 \
  connect=tcp:127.0.0.1:22

# Lister / supprimer
lxc config device list c1
lxc config device remove c1 http
```

**Fonctionnement :** un processus proxy tourne sur l'hôte et relaie le trafic.
Simple et efficace pour quelques ports ; pour du vrai reverse-proxy multi-sites,
voir le cas pratique section 78.

---

## 30. Réseau — IPv6

Par défaut `lxd init` propose `auto` (ULA `fd42:...`) ou `none`.

```bash
# Activer l'IPv6 ULA sur lxdbr0
lxc network set lxdbr0 ipv6.address auto
lxc network set lxdbr0 ipv6.nat true

# IPv6 routé public (si votre hébergeur route un /64 vers l'hôte)
lxc network set prod0 ipv6.address 2001:db8:abcd:10::1/64
lxc network set prod0 ipv6.nat false

# Désactiver IPv6 sur un conteneur
lxc config device set c1 eth0 ipv6.address none
```

**Vérification dans le conteneur :**

```bash
lxc exec c1 -- ip -6 addr show eth0
lxc exec c1 -- ping -c2 ipv6.google.com
```

> 📝 En entreprise, documentez votre plan d'adressage IPv6 comme l'IPv4.
> L'ULA (`fd00::/8`) convient pour l'interne sans exposition publique.

---

## 31. Réseau — Fan networking (overlay simple)

Le **fan** est un overlay VXLAN simplifié : chaque hôte obtient un sous-réseau
d'un grand /8, les conteneurs communiquent entre hôtes sans configuration réseau
complexe.

```bash
# Sur chaque hôte du "fan", avec un sous-réseau unique par hôte
lxc network create fan0 --type=bridge \
  bridge.mode=fan \
  fan.overlay_subnet=240.0.0.0/8 \
  fan.underlay_subnet=10.0.0.0/16
```

**Cas d'usage :** maquette multi-hôtes rapide, labo. Pour de la production
multi-hôtes sérieuse, préférez **OVN** (routage distribué, voir section 69).

---

## 32. Stockage — Concepts (pools, volumes)

```
Storage pool "default" (driver: zfs)
├── volume c1          (disque root du conteneur c1)
├── volume c2
├── volume custom/data (volume custom attaché à c1 en /data)
└── snapshot c1/snap0  (snapshot ZFS natif)
```

```bash
# Lister les pools
lxc storage list

# Infos détaillées (espace utilisé/disponible)
lxc storage info default

# Lister les volumes d'un pool
lxc storage volume list default
```

**Règle d'or :** un conteneur = un volume `root`. Les données applicatives
volumineuses (bases, uploads) méritent des **volumes custom** séparés
(sauvegarde et migration indépendantes).

---

## 33. Stockage — Pool `dir` (simple, sans dépendance)

Le driver `dir` stocke les conteneurs comme de simples dossiers. Aucune
fonctionnalité avancée (pas de snapshots instantanés, copies lentes).

```bash
lxc storage create dirpool dir source=/var/lib/lxd-disk
```

| ✅ Avantages | ❌ Inconvénients |
|---|---|
| Fonctionne partout, zéro prérequis | Pas de snapshots natifs |
| Facile à inspecter / rsync | Copies complètes (lentes) |
| | Pas de quotas efficaces |

**Usage :** dépannage, tout petit labo, ou quand ZFS/btrfs sont impossibles.
**En production : préférez ZFS.**

---

## 34. Stockage — Pool ZFS (recommandé)

```bash
# Sur un disque dédié (PRODUCTION) — efface le disque !
lxc storage create zfspool zfs source=/dev/sdb

# Sur une partition existante
lxc storage create zfspool zfs source=/dev/sdb1

# En fichier loop (TEST uniquement — performances limitées)
lxc storage create zfspool zfs size=50GiB

# Migrer le pool par défaut vers un disque réel après coup
lxc storage create prod-zfs zfs source=/dev/sdb
lxc storage set default source /dev/sdb   # selon contexte
```

**Pourquoi ZFS :**

- Snapshots et clones **instantanés** (COW).
- Copies de conteneurs quasi gratuites (`lxc copy`).
- Compression native (`compression=on` — souvent `lz4` par défaut).
- `zfs list`, `zfs snapshot` accessibles directement sur l'hôte.

**Réglages utiles :**

```bash
# Compression (si non active)
sudo zfs set compression=lz4 default/containers

# Voir l'espace réel
sudo zfs list -o name,used,avail,compressratio
```

---

## 35. Stockage — Pool btrfs

Alternative COW quand ZFS n'est pas disponible (licence, noyau) :

```bash
lxc storage create btrfspool btrfs source=/dev/sdc
```

| ✅ | ❌ |
|---|---|
| Snapshots natifs, subvolumes | Moins mature que ZFS pour cet usage |
| Intégré au noyau | Fragmentation sur charges DB intenses |
| | Outils moins riches |

**Note :** btrfs convient bien pour des conteneurs légers et du dev.
Pour des bases de données en production, ZFS reste le choix sûr.

---

## 36. Stockage — Pool LVM

```bash
# Sur un groupe de volumes existant
sudo vgcreate vg-lxd /dev/sdd
lxc storage create lvmpool lvm source=vg-lxd
lxc storage set lvmpool lvm.thinpool_name=LXDThinPool
```

| ✅ | ❌ |
|---|---|
| Snapshots possibles (thin) | Snapshots moins efficaces que COW |
| Standard entreprise bien connu | Configuration plus manuelle |

**Usage :** parcs déjà standardisés sur LVM, ou contraintes de politique interne.

---

## 37. Volumes — Créer, attacher, détacher

```bash
# Créer un volume custom de 100 Go
lxc storage volume create default data-db --type=custom size=100GiB

# L'attacher à un conteneur (monté en /var/lib/postgresql)
lxc config device add c-db1 pgdata disk \
  pool=default \
  source=data-db \
  path=/var/lib/postgresql

# Détacher (le volume survit, les données sont conservées)
lxc config device remove c-db1 pgdata

# Le réattacher à un autre conteneur (migration de données !)
lxc config device add c-db2 pgdata disk \
  pool=default source=data-db path=/var/lib/postgresql

# Supprimer le volume (définitif)
lxc storage volume delete default data-db
```

**Bonnes pratiques :**

- Un volume par rôle de données (`pgdata`, `uploads`, `backups`).
- Nommez explicitement : `data-<service>-<env>`.
- Documentez les attachements (`lxc config device list`).

---

## 38. Snapshots — Créer, lister, restaurer

```bash
# Snapshot manuel
lxc snapshot c1
lxc snapshot c1 avant-maj           # avec un nom explicite

# Lister
lxc info c1 | grep -A20 Snapshots
lxc query /1.0/instances/c1/snapshots | jq

# Restaurer (état du snapshot ; le conteneur est arrêté brièvement)
lxc restore c1 avant-maj

# Restaurer SANS état (stateful=false par défaut ; les snapshots stateful
# capturent aussi la RAM — utile avant une manip risquée)
lxc snapshot c1 --stateful
lxc restore c1 snap-stateful

# Supprimer
lxc delete c1/avant-maj
```

> ⚠️ `lxc restore` **écrase** l'état actuel. Faites un snapshot "avant-restore"
> si le moindre doute subsiste.

---

## 39. Snapshots — Planification automatique

```bash
# Un snapshot par jour à 3h, conservés 7 jours
lxc config set c1 snapshots.schedule "0 3 * * *"
lxc config set c1 snapshots.schedule.stopped true   # même si arrêté
lxc config set c1 snapshots.expiry 7d
lxc config set c1 snapshots.pattern "auto-%Y%m%d-%H%M"

# Appliquer via un profil à tout un parc
lxc profile set backup-daily snapshots.schedule "0 3 * * *"
lxc profile set backup-daily snapshots.expiry 7d
```

**Stratégie type :**

| Fréquence | Rétention | Usage |
|---|---|---|
| `@hourly` | 24h | conteneurs critiques, pré-change |
| `@daily` | 7-14j | standard production |
| `@weekly` | 4-8 sem | conteneurs stables |

Les snapshots ne remplacent **pas** une sauvegarde externalisée (section 59) :
ils protègent des erreurs humaines, pas de la perte du disque hôte.

---

## 40. Publier un conteneur en image (golden image)

Transformez un conteneur configuré en **modèle réutilisable** :

```bash
# 1. Préparer le conteneur modèle (nettoyage)
lxc exec c-web-template -- apt clean
lxc exec c-web-template -- rm -rf /tmp/* /var/tmp/*
lxc exec c-web-template -- cloud-init clean 2>/dev/null

# 2. Arrêter
lxc stop c-web-template

# 3. Publier
lxc publish c-web-template --alias web-golden-v1

# 4. Utiliser
lxc launch web-golden-v1 web-prod-01
lxc launch web-golden-v1 web-prod-02

# Lister vos images publiées
lxc image list
```

**Workflow d'équipe :** `template` → `publish --alias web-golden-v<N>` →
déploiements. Versionnez les alias, documentez les changements dans la
description (`--prop description="..."`).

---

## 41. Limites de ressources — CPU

```bash
# Nombre de vCPU
lxc config set c1 limits.cpu 2

# Épinglage sur des cœurs précis (isolation NUMA / charges sensibles)
lxc config set c1 limits.cpu 0-3
lxc config set c1 limits.cpu 0,2,4,6

# Priorité relative (shares, défaut 1024)
lxc config set c1 limits.cpu.priority 512

# Temps CPU dur (microsecondes par période) — plafond strict
lxc config set c1 limits.cpu.allowance 50%
```

**Vérification dans le conteneur :**

```bash
lxc exec c1 -- nproc
lxc exec c1 -- cat /sys/fs/cgroup/cpu.max 2>/dev/null
```

> 📝 Avec cgroups v2, le conteneur voit ses limites via `/sys/fs/cgroup`.
> `nproc` reflète correctement les CPUs alloués sur les images récentes.

---

## 42. Limites de ressources — RAM

```bash
# Limite mémoire (dur)
lxc config set c1 limits.memory 4GiB

# Interdire le swap pour le conteneur (recommandé pour les DB)
lxc config set c1 limits.memory.swap false

# Pourcentage de la RAM hôte
lxc config set c1 limits.memory 25%
```

**Dimensionnement :** prévoyez toujours une marge hôte (~10-20 % pour le
système + ZFS ARC si applicable). La somme des limites peut dépasser la RAM
physique (overcommit), mais surveillez la pression mémoire réelle
(`lxc info c1` → Memory).

---

## 43. Limites de ressources — I/O disque

```bash
# Débit max (lecture/écriture)
lxc config set c1 limits.disk.priority 5
lxc config device set c1 root limits.read 100MB
lxc config device set c1 root limits.write 50MB

# IOPS max
lxc config device set c1 root limits.max 1000
```

**Cas d'usage :** empêcher un conteneur de monopoliser le disque (build qui
sature, logs qui s'emballent). Particulièrement utile sur pool partagé.

**Vérifier :**

```bash
lxc config device show c1 root
```

---

## 44. Limites — Processus, fichiers ouverts, mémoire verrouillée

```bash
# Nombre max de processus/threads
lxc config set c1 limits.processes 512

# Fichiers ouverts (nofile)
lxc config set c1 limits.kernel.nofile 65536

# Mémoire verrouillée (memlock, pour certaines DB)
lxc config set c1 limits.kernel.memlock unlimited

# Nombre max de conteneurs imbriqués / clés
lxc config set c1 limits.kernel.nproc 4096
```

**Exemple concret — PostgreSQL :**

```bash
lxc config set c-db1 limits.kernel.nofile 100000
lxc config set c-db1 limits.memory 8GiB
lxc config set c-db1 limits.memory.swap false
```

---

## 45. Limites — Ce que voit le conteneur (cgroups v2)

Sur les systèmes récents, les limites sont **visibles et opposables** dans
le conteneur :

```bash
lxc exec c1 -- cat /sys/fs/cgroup/memory.max
lxc exec c1 -- cat /sys/fs/cgroup/cpu.max
lxc exec c1 -- cat /sys/fs/cgroup/pids.max
```

**Pourquoi c'est important :** la JVM, Go, ou `systemd` adaptent leur
comportement (heap, threads) à la mémoire/CPU **vue**. Des limites correctes
= des applis qui s'auto-dimensionnent bien.

**Piège :** une image très ancienne peut ne pas exposer `/sys/fs/cgroup`
correctement → préférez des images récentes (Ubuntu 22.04+/Debian 12+).

---

## 46. Conteneurs privilégiés vs non privilégiés — Sécurité

|  | Non privilégié (défaut LXD) | Privilégié |
|---|---|---|
| UID 0 du conteneur | Mappé sur un UID non-root de l'hôte (ex. 1000000) | = UID 0 réel de l'hôte |
| Évasion → impact | Limitée par le user namespace | **Root complet sur l'hôte** |
| Cas d'usage | 99 % des cas | Pilotes/noyau spécifiques, rarement justifié |

```bash
# Vérifier le mode
lxc config get c1 security.privileged   # vide/false = non privilégié

# (DÉCONSEILLÉ) passer en privilégié
lxc config set c1 security.privileged true
```

**Mapping des UIDs (idmap) :**

```bash
# Voir la plage allouée à un conteneur
lxc config get c1 volatile.idmap.next
cat /etc/subuid /etc/subgid   # plages de l'hôte
```

> 🛡️ **Règle absolue :** restez en **non privilégié**. Si un logiciel exige
> le mode privilégié, cherchez d'abord les capabilities fines
> (`security.syscalls.*`, devices spécifiques) avant de céder.

---

## 47. Sécurité — Bonnes pratiques d'isolation

**Checklist par conteneur :**

- [ ] Non privilégié (`security.privileged=false`)
- [ ] Nesting désactivé sauf besoin (`security.nesting=false` par défaut)
- [ ] Limites CPU/RAM/processus définies (anti-DoS du voisin)
- [ ] Pas de device `disk` vers l'hôte sauf besoin métier documenté
- [ ] Réseau : le conteneur n'a que les interfaces nécessaires
- [ ] `boot.autostart` cohérent (évite les démarrages surprises)
- [ ] Mises à jour automatiques ou planifiées (section 62)

**Isolation réseau :** un bridge par zone de confiance (section 25).
Un conteneur compromis sur `dmz0` ne doit pas voir `prod0`.

---

## 48. Sécurité — AppArmor et seccomp

LXD applique par défaut des profils **AppArmor** et un filtre **seccomp**
qui bloquent les appels système dangereux dans le conteneur.

```bash
# Voir le profil AppArmor appliqué
lxc config get c1 raw.apparmor

# Surcharger (exemple : autoriser un syscall pour une appli exotique)
lxc config set c1 raw.apparmor |
# via edit :
lxc config edit c1
```

```yaml
config:
  raw.apparmor: |
    mount options=(rw, bind),
```

**Seccomp :**

```bash
# Mode par défaut (filtre strict)
lxc config get c1 security.syscalls.deny_default
# Personnaliser la liste
lxc config set c1 security.syscalls.allow "personality sched_setattr"
```

> ⚠️ Ne touchez à AppArmor/seccomp que si une application légitime est
> bloquée (logs `audit`/`dmesg` sur l'hôte). Documentez chaque exception.

---

## 49. Sécurité — idmap et UID/GID

Le **user namespace** mappe les UIDs du conteneur vers une plage haute
de l'hôte :

```
Conteneur : root (0)  →  Hôte : 1000000
Conteneur : www-data (33) → Hôte : 1000033
```

**Partager un dossier hôte avec les bons droits :**

```bash
# Le dossier hôte doit appartenir à la plage mappée
sudo chown -R 1000000:1000000 /srv/partage-c1

lxc config device add c1 partage disk \
  source=/srv/partage-c1 \
  path=/mnt/partage
```

**Trouver la plage d'un conteneur :**

```bash
lxc config get c1 volatile.idmap.next
# ou
grep -A2 "c1" /var/snap/lxd/common/lxd/containers/c1/rootfs 2>/dev/null
cat /proc/$(lxc info c1 | awk '/^PID/{print $2}')/uid_map
```

**idmap custom (faire correspondre un UID précis) :**

```bash
lxc config set c1 raw.idmap "uid 1000 1000
gid 1000 1000"
```

---

## 50. Sécurité — Durcir un conteneur (checklist opérationnelle)

```bash
# 1. Base : non privilégié, pas de nesting inutile
lxc config set c1 security.privileged false
lxc config set c1 security.nesting false

# 2. Interdire les montages sensibles (déjà restreint par défaut en non-privilégié)
# 3. Limiter les syscalls réseau bruts si inutile
lxc config set c1 security.syscalls.deny_compat true

# 4. Lecture seule du rootfs quand possible (applis stateless)
#    → monter les zones d'écriture en volumes séparés
lxc config device add c1 app-data disk pool=default source=appdata path=/data

# 5. Dans le conteneur : SSH durci, pas de root distant
lxc exec c1 -- sed -i 's/^#*PermitRootLogin.*/PermitRootLogin no/' /etc/ssh/sshd_config
```

**Checklist finale :**

- [ ] `security.privileged=false`, `security.nesting=false` (sauf besoin)
- [ ] Limites de ressources posées
- [ ] SSH : clés uniquement, root interdit
- [ ] Pare-feu dans le conteneur (ufw/nftables) ou au bridge
- [ ] Mises à jour auto configurées
- [ ] Logs centralisés (section 67)
- [ ] Snapshot pré-change + sauvegarde externalisée

---

## 51. Nesting — Docker dans LXC : le principe

Faire tourner **Docker à l'intérieur d'un conteneur LXC** : très pratique
(un "hôte Docker" léger et jetable), mais ça demande d'assouplir l'isolation.

```bash
# Configuration minimale requise
lxc config set c-docker security.nesting true

# Recommandé : autoriser les appels système nécessaires
lxc config set c-docker security.syscalls.intercept.mknod true
lxc config set c-docker security.syscalls.intercept.setxattr true

lxc restart c-docker
```

**Puis dans le conteneur :**

```bash
lxc exec c-docker -- bash
[ct] # apt update && apt install -y docker.io
[ct] # systemctl enable --now docker
[ct] # docker run hello-world
```

---

## 52. Nesting — Configuration requise détaillée

**Profil `docker-host` prêt à l'emploi :**

```yaml
config:
  security.nesting: "true"
  security.syscalls.intercept.mknod: "true"
  security.syscalls.intercept.setxattr: "true"
  linux.kernel_modules: ip_tables,ip6_tables,overlay,br_netfilter
  limits.memory: 8GiB
  boot.autostart: "true"
description: Hote Docker dans LXC
devices:
  eth0: {name: eth0, network: lxdbr0, type: nic}
  root: {path: /, pool: default, type: disk, size: 60GiB}
name: docker-host
```

```bash
lxc profile create docker-host
cat profil-docker-host.yaml | lxc profile edit docker-host
lxc launch ubuntu:24.04 docker1 --profile default --profile docker-host
```

**Points d'attention :**

- Préférez le driver de stockage **overlay2** de Docker (défaut) ; évitez
  `vfs` (lent) et le double COW ZFS-sur-ZFS (désactivez la compression
  redondante si besoin).
- **Ne pas** empiler : Docker dans LXC dans Docker = problèmes garantis.
- Sur Proxmox, la même option existe : `features: nesting=1` (section 71).
- Alternative moderne : **Podman rootless** dans le conteneur (moins d'exigences).

---

## 53. Devices — Disques (disk device)

```bash
# Dossier de l'hôte monté dans le conteneur
lxc config device add c1 data disk source=/srv/data-c1 path=/data

# Options de montage
lxc config device add c1 data disk \
  source=/srv/data-c1 \
  path=/data \
  readonly=true \
  shift=true

# Volume custom du pool (recommandé vs dossier hôte)
lxc storage volume create default vol-data size=50GiB
lxc config device add c1 data disk pool=default source=vol-data path=/data

# Limiter la taille du rootfs
lxc config device set c1 root size 30GiB

# Voir les devices
lxc config device list c1
lxc config device show c1 data
```

**`shift=true` :** LXD décale les UIDs à la volée pour que les fichiers
apparaissent avec les bons propriétaires des deux côtés. Pratique pour les
dossiers partagés hôte ↔ conteneur.

---

## 54. Devices — GPU

```bash
# Passer TOUS les GPU au conteneur
lxc config device add c1 gpu gpu

# Passer un GPU précis (NVIDIA, par PCI)
lxc config device add c1 gpu0 gpu pci=0000:01:00.0 id=0

# Vérifier dans le conteneur
lxc exec c1 -- nvidia-smi
lxc exec c1 -- ls /dev/dri
```

**Prérequis :** pilotes installés sur l'**hôte** (le conteneur utilise le
device via le noyau hôte). Pour NVIDIA, le toolkit container fonctionne
aussi bien dans LXC que dans Docker.

**Cas d'usage :** transcodage vidéo (Jellyfin/Plex), inférence IA légère,
build avec accélération.

---

## 55. Devices — USB

```bash
# Lister les USB de l'hôte
lsusb

# Passer une clé USB précise (par IDs vendeur/produit)
lxc config device add c1 usbkey usb vendorid=0951 productid=1666

# Passer un port USB entier
lxc config device add c1 usbport usb bus=1 device=4

# Dongle série (ex. Zigbee / onduleur en USB)
lxc config device add c1 zigbee unix-char path=/dev/ttyUSB0 source=/dev/ttyUSB0
```

**Exemple concret — supervision d'onduleur (lien avec le guide onduleurs) :**

```bash
# L'onduleur est branché en USB sur l'hôte, NUT tourne dans le conteneur
lxc config device add c-nut ups usb vendorid=051d productid=0002
lxc exec c-nut -- apt install -y nut
```

> ⚠️ Un device USB passé au conteneur disparaît de l'hôte. Documentez
> quel conteneur détient quel périphérique physique.

---

## 56. Devices — Proxy (redirection de ports) — approfondi

Déjà vu en section 29, voici les modes avancés :

```bash
# Mode NAT (défaut) : processus proxy sur l'hôte
lxc config device add c1 web proxy \
  listen=tcp:0.0.0.0:80 connect=tcp:127.0.0.1:80 nat=true

# Proxy UDP (ex. WireGuard, DNS)
lxc config device add c1 wg proxy \
  listen=udp:0.0.0.0:51820 connect=udp:127.0.0.1:51820

# Proxy vers un socket Unix
lxc config device add c1 dockersock proxy \
  listen=unix:/var/host-docker.sock connect=unix:/var/run/docker.sock \
  uid=1000 gid=1000 mode=0660

# Voir les connexions actives
lxc config device show c1 web
```

**Limites :** un proxy par port ; pour des dizaines de services, préférez
un reverse-proxy (section 78) ou macvlan.

---

## 57. Devices — NIC avancés

```bash
# Adresse MAC fixée (utile pour DHCP externe / filtrage)
lxc config device set c1 eth0 hwaddr 00:16:3e:aa:bb:cc

# MTU personnalisé
lxc config device set c1 eth0 mtu 9000

# File d'attente / offloading
lxc config device set c1 eth0 mtu 1500

# Plusieurs interfaces
lxc config device add c1 eth1 nic network=dmz0 name=eth1
lxc config device add c1 eth2 nic network=prod0 name=eth2

# Limiter le débit réseau du conteneur
lxc config device set c1 eth0 limits.ingress 100Mbit
lxc config device set c1 eth0 limits.egress 100Mbit
```

**Ordre des interfaces :** `eth0`, `eth1`… dans l'ordre d'ajout. Pour un
routage prévisible, fixez les noms (`name=wan`, `name=lan`) plutôt que de
compter sur l'ordre.

---

## 58. Backups — Export / Import (méthode native)

```bash
# Exporter un conteneur (arrêté de préférence) vers un fichier
lxc stop c1
lxc export c1 /backups/c1-2026-09-26.tar.gz

# Avec les snapshots inclus
lxc export c1 /backups/c1-full.tar.gz --instance-only=false

# Compressé et optimisé (backend ZFS : rapide)
lxc export c1 /backups/c1.tar.gz --optimized-storage

# Importer sur le même hôte ou un autre
lxc import /backups/c1-2026-09-26.tar.gz
lxc import /backups/c1-2026-09-26.tar.gz c1-restore
```

**Contenu de l'export :** rootfs + config + snapshots (si inclus).
**Ne contient pas :** les volumes custom attachés séparément → exportez-les
aussi (`lxc storage volume export`).

---

## 59. Backups — Stratégie snapshots + rsync / Borg

**Architecture recommandée :**

```
Conteneur (prod)
   │  snapshot ZFS @daily (récupération rapide locale)
   ▼
Export hebdo → /backups/ (fichiers .tar.gz)
   │  Borg vers NAS distant (dédupliqué, chiffré)
   ▼
Règle 3-2-1 : 3 copies, 2 supports, 1 hors site
```

```bash
# Export des volumes custom aussi
lxc storage volume export default vol-data /backups/vol-data.tar.gz
```

**Avec Borg (déduplication + chiffrement) :**

```bash
# Init du dépôt (une fois)
borg init --encryption=repokey /mnt/nas/borg-lxd

# Sauvegarde des exports
borg create /mnt/nas/borg-lxd::lxd-{now:%Y-%m-%d} /backups/*.tar.gz

# Rétention
borg prune /mnt/nas/borg-lxd --keep-daily=7 --keep-weekly=4 --keep-monthly=6
```

---

## 60. Backups — Script de sauvegarde automatisée

`/usr/local/bin/lxd-backup.sh` :

```bash
#!/bin/bash
# Sauvegarde LXD : export de tous les conteneurs + volumes, puis Borg.
set -euo pipefail

BACKUP_DIR="/backups/lxd"
DATE=$(date +%Y-%m-%d)
RETENTION_DAYS=14
BORG_REPO="/mnt/nas/borg-lxd"

mkdir -p "$BACKUP_DIR/$DATE"

echo "[1/4] Export des conteneurs..."
for ct in $(lxc list -f csv -c n status=running); do
    echo "  -> $ct"
    lxc export "$ct" "$BACKUP_DIR/$DATE/${ct}.tar.gz" --quiet
done

echo "[2/4] Export des volumes custom..."
for vol in $(lxc storage volume list default -f csv -c n | grep -v '^$'); do
    # on ignore les volumes root des instances (type container)
    if lxc storage volume show default "$vol" | grep -q "type: custom"; then
        echo "  -> $vol"
        lxc storage volume export default "$vol" \
            "$BACKUP_DIR/$DATE/vol-${vol}.tar.gz" --quiet
    fi
done

echo "[3/4] Envoi vers Borg..."
borg create --stats "$BORG_REPO::lxd-$DATE" "$BACKUP_DIR/$DATE"

echo "[4/4] Nettoyage local (+$RETENTION_DAYS jours)..."
find "$BACKUP_DIR" -maxdepth 1 -type d -mtime +$RETENTION_DAYS -exec rm -rf {} +
borg prune "$BORG_REPO" --keep-daily=7 --keep-weekly=4 --keep-monthly=6

echo "Sauvegarde terminee : $DATE"
```

```bash
chmod +x /usr/local/bin/lxd-backup.sh
# Cron : tous les jours à 2h
(crontab -l 2>/dev/null; echo "0 2 * * * /usr/local/bin/lxd-backup.sh >> /var/log/lxd-backup.log 2>&1") | crontab -
```

> 🔐 Stockez la passphrase Borg hors du script (variable d'environnement
> `BORG_PASSPHRASE` via un fichier protégé `chmod 600` sourcé par le cron).

---

## 61. Backups — Restauration complète (procédure)

**Scénario :** le conteneur `c-db1` est corrompu, on restaure la sauvegarde
de la veille.

```bash
# 1. Extraire l'archive Borg si besoin
borg extract /mnt/nas/borg-lxd::lxd-2026-09-25

# 2. Supprimer (ou renommer) le conteneur cassé
lxc stop c-db1 --force
lxc rename c-db1 c-db1-hs
# ou : lxc delete c-db1 --force

# 3. Réimporter
lxc import /backups/lxd/2026-09-25/c-db1.tar.gz

# 4. Restaurer le volume de données
lxc storage volume delete default pgdata --force  # si corrompu aussi
lxc storage volume import default /backups/lxd/2026-09-25/vol-pgdata.tar.gz pgdata

# 5. Démarrer et vérifier
lxc start c-db1
lxc exec c-db1 -- systemctl status postgresql
lxc exec c-db1 -- psql -U postgres -c "SELECT 1;"
```

**Testez votre restauration** au moins une fois par trimestre sur un
conteneur jetable (`c-db1-test`). Une sauvegarde non testée n'est pas une
sauvegarde.

---

## 62. Mises à jour — Hôte et conteneurs

**Hôte :**

```bash
# LXD via snap : mises à jour automatiques (canal suivi)
sudo snap refresh lxd

# Système hôte
sudo apt update && sudo apt upgrade -y
```

**Conteneurs — manuel :**

```bash
# Tous les conteneurs Ubuntu/Debian d'un coup
for ct in $(lxc list -f csv -c n status=running); do
    echo "=== $ct ==="
    lxc exec "$ct" -- apt update
    lxc exec "$ct" --env DEBIAN_FRONTEND=noninteractive -- \
        apt upgrade -y
done
```

**Conteneurs — automatique (`unattended-upgrades`) :**

```bash
# Dans chaque conteneur (ou via le template / profil cloud-init)
lxc exec c1 -- apt install -y unattended-upgrades
lxc exec c1 -- dpkg-reconfigure -plow unattended-upgrades
```

**Stratégie :** sécurité en auto (`unattended-upgrades`), montées de version
majeures planifiées avec snapshot préalable (section 38).

---

## 63. Mises à jour — Stratégie rolling (zéro coupure)

Pour un service en plusieurs instances (ex. 3 webs derrière un reverse-proxy) :

```
1. Snapshot de web1                    (lxc snapshot web1 avant-maj)
2. MAJ web1, redémarrage, tests        (lxc exec web1 -- apt upgrade -y)
3. Si OK → web2, puis web3
4. Si KO → lxc restore web1 avant-maj  (retour en < 1 min)
```

**Automatisation Ansible (extrait) :**

```yaml
- hosts: lxd_hosts
  serial: 1   # un conteneur à la fois
  tasks:
    - name: Snapshot avant MAJ
      command: lxc snapshot {{ inventory_hostname }} avant-maj-{{ ansible_date_time.date }}
    - name: Mise à jour
      command: lxc exec {{ inventory_hostname }} -- apt upgrade -y
      environment:
        DEBIAN_FRONTEND: noninteractive
```

---

## 64. Supervision — Métriques natives LXD

```bash
# État temps réel d'un conteneur
lxc info c1
# → CPU, mémoire, disque, réseau, processus, load

# Métriques au format OpenMetrics (pour Prometheus !)
curl --unix-socket /var/snap/lxd/common/lxd/unix.socket \
  lxd/1.0/metrics | head -50

# Avec authentification par certificat si HTTPS distant
```

**Métriques exposées :** `lxd_cpu_*`, `lxd_memory_*`, `lxd_disk_*`,
`lxd_network_*`, `lxd_procs_*` par instance. C'est la source la plus
fiable : elle vient des cgroups, pas d'un agent dans le conteneur.

---

## 65. Supervision — Prometheus + Grafana

**`prometheus.yml` (scrape du socket LXD via un exporter ou HTTPS) :**

```yaml
scrape_configs:
  - job_name: 'lxd'
    static_configs:
      - targets: ['hote-lxd1:8443']
    scheme: https
    tls_config:
      cert_file: /etc/prometheus/lxd-client.crt
      key_file: /etc/prometheus/lxd-client.key
      insecure_skip_verify: true
    metrics_path: /1.0/metrics
```

**Alertes utiles (`alerts.yml`) :**

```yaml
groups:
  - name: lxd
    rules:
      - alert: ConteneurArrete
        expr: lxd_instance_state == 0
        for: 5m
        labels: {severity: critical}
      - alert: MemoireConteneurHaute
        expr: lxd_memory_MemAvailable_bytes / lxd_memory_MemTotal_bytes < 0.1
        for: 10m
        labels: {severity: warning}
```

**Dashboards Grafana :** un panel par conteneur (CPU, RAM, I/O, réseau),
un panel d'ensemble (top 10 consommateurs).

---

## 66. Supervision — Zabbix / Netdata

**Zabbix :** agent dans chaque conteneur (ou supervision SNMP de l'hôte +
`lxc list` via UserParameter) :

```bash
# Sur l'hôte : UserParameter pour Zabbix
# /etc/zabbix/zabbix_agentd.d/lxd.conf
UserParameter=lxd.containers.running,lxc list -f csv -c s | grep -c RUNNING
UserParameter=lxd.container.discovery,lxc list -f json | jq -c '{data:[.[]|{ "{#CT}": .name }]}'
```

**Netdata :** le plugin cgroups détecte automatiquement les conteneurs
LXC/LXD et affiche leurs métriques sans configuration. Idéal pour un
diagnostic rapide :

```bash
# Installer Netdata sur l'hôte (conteneurs détectés auto)
bash <(curl -Ss https://get.netdata.cloud/kickstart.sh)
```

---

## 67. Supervision — Logs et alertes

```bash
# Logs du daemon LXD
lxc monitor --type=logging --pretty

# Logs d'un conteneur (console)
lxc info c1 --show-log

# Logs dans le conteneur (systemd)
lxc exec c1 -- journalctl -p err -b

# Événements cycle de vie (démarrages, arrêts, snapshots)
lxc monitor --type=lifecycle --pretty
```

**Centralisation :** envoyez les logs vers rsyslog/Loki/ELK :

```bash
# Exemple : forwarder journald du conteneur vers l'hôte
lxc exec c1 -- journalctl -f -o json | /usr/local/bin/ship-to-loki.sh
```

**Alertes minimales à mettre en place :**

- [ ] Un conteneur attendu est arrêté > 5 min
- [ ] Espace disque pool > 85 %
- [ ] Échec de sauvegarde (cron)
- [ ] Snapshot automatique en échec

---

## 68. Production — Checklist de mise en production

Avant de déclarer un conteneur "prod", validez :

**Configuration :**

- [ ] Profil(s) appliqué(s), config versionnée dans Git
- [ ] Limites CPU/RAM/I/O/processus définies
- [ ] `boot.autostart=true` + priorité/délai cohérents
- [ ] Réseau : bridge dédié, IP statique réservée, DNS OK
- [ ] Stockage : volume custom pour les données, taille rootfs limitée

**Sécurité :**

- [ ] Non privilégié, nesting désactivé (sauf besoin documenté)
- [ ] SSH clés uniquement, root interdit, fail2ban si exposé
- [ ] Mises à jour de sécurité automatiques
- [ ] Pare-feu (hôte et/ou conteneur)

**Exploitation :**

- [ ] Snapshots planifiés + expiration
- [ ] Sauvegarde externalisée testée (restauration OK)
- [ ] Supervision : métriques + alertes (arrêt, disque, mémoire)
- [ ] Logs centralisés
- [ ] Documentation : rôle, contacts, procédure de redémarrage
- [ ] Snapshot "mise-en-prod" pris le jour J

---

## 69. Production — Clustering LXD (haute disponibilité)

LXD peut former un **cluster** : plusieurs hôtes gérés comme un seul,
avec répartition des conteneurs.

```bash
# Sur le 1er nœud : activer le clustering à l'init
sudo lxd init
# Would you like to use LXD clustering? yes
# → définir un trust password

# Sur les autres nœuds : joindre
sudo lxd init
# Would you like to use LXD clustering? yes
# → join existing cluster, adresse du 1er nœud + trust password

# Voir les membres
lxc cluster list

# Lancer un conteneur sur un nœud précis
lxc launch ubuntu:24.04 c1 --target hote2

# Migrer à chaud (avec CRIU si configuré, sinon à froid)
lxc move c1 --target hote3
lxc stop c1 && lxc move c1 --target hote3 && lxc start c1
```

**Prérequis réseau :** les nœuds doivent partager le stockage (ou utiliser
des volumes répliqués) et un réseau de cluster dédié. Pour du stockage
partagé : **Ceph** (via `lxc storage create ... ceph`).

**Limites honnêtes :** le clustering LXD répartit la gestion, mais la
**haute disponibilité automatique** (redémarrage sur panne) reste moins
intégrée que le HA de Proxmox VE. Évaluez selon votre SLA.

---

## 70. Production — Quotas et facturation interne

```bash
# Quota disque par projet (LXD "projects")
lxc project create equipe-reseau
lxc project set equipe-reseau limits.containers 10
lxc project set equipe-reseau limits.memory 32GiB
lxc project set equipe-reseau limits.cpu 16
lxc project set equipe-reseau limits.disk 200GiB

# Basculer sur le projet et créer dedans
lxc project switch equipe-reseau
lxc launch ubuntu:24.04 c1   # créé dans le projet, soumis aux quotas
lxc project switch default
```

**Facturation interne (chargeback) :** exportez `lxc list -f json` +
métriques Prometheus, calculez CPU/RAM/disque par projet/équipe, et
produisez un rapport mensuel. Les `user.*` keys permettent de taguer
(`user.cost-center=SRV-042`).

---

## 71. Proxmox VE — LXC sous Proxmox : `pct`

Proxmox VE intègre LXC nativement. Chaque conteneur a un **VMID** numérique.

| Action | LXD | Proxmox VE |
|---|---|---|
| Créer | `lxc launch ubuntu:24.04 c1` | `pct create 100 local:vztmpl/ubuntu-24.04.tar.zst ...` |
| Démarrer/arrêter | `lxc start/stop c1` | `pct start/stop 100` |
| Shell | `lxc exec c1 -- bash` | `pct enter 100` / `pct exec 100 -- bash` |
| Fichiers | `lxc file push/pull` | `pct push/pull 100 ...` |
| Config | `lxc config set` | `pct set 100 --memory 4096` |
| Snapshot | `lxc snapshot` | `pct snapshot 100 snap0` |
| Backup | `lxc export` | `vzdump 100` (intégré !) |
| Liste | `lxc list` | `pct list` |
| Supprimer | `lxc delete` | `pct destroy 100` |

**Créer un conteneur sous Proxmox :**

```bash
# 1. Télécharger un template (interface ou CLI)
pveam update
pveam download local ubuntu-24.04-standard_24.04-2_amd64.tar.zst

# 2. Créer le CT
pct create 100 local:vztmpl/ubuntu-24.04-standard_24.04-2_amd64.tar.zst \
  --hostname web1 \
  --cores 2 --memory 4096 --swap 0 \
  --rootfs local-lvm:20 \
  --net0 name=eth0,bridge=vmbr0,ip=dhcp \
  --unprivileged 1 \
  --features nesting=1 \
  --onboot 1 \
  --password 'mot-de-passe-temporaire'
```

---

## 72. Proxmox VE — Différences LXD vs `pct`

| Aspect | LXD | Proxmox `pct` |
|---|---|---|
| Nommage | Nom libre (`web1`) | VMID numérique (100) |
| Images | Remotes dynamiques (`ubuntu:`) | Templates `pveam` (téléchargés) |
| Profils | Natif, puissant | Pas d'équivalent (templates + cloud-init partiel) |
| Réseau | `lxdbr0`, OVN, fan | `vmbr0`, SDN (EVPN/QinQ), firewall intégré |
| Stockage | Pools LXD | Stockages Proxmox (ZFS, LVM, Ceph, NFS…) |
| Snapshots | Planifiables nativement | Natifs (selon stockage), planifiés via backup |
| Backup | Manuel (`export`) / script | **`vzdump` intégré + planifié** |
| Clustering/HA | Cluster LXD | Cluster corosync + **HA automatique** |
| API | REST moderne | API REST Proxmox (bien outillée : Terraform…) |
| Nesting | `security.nesting=true` | `features: nesting=1` |
| Privilégié | `security.privileged` | `--unprivileged 0` (déconseillé idem) |

**En résumé :** LXD = excellent sur hôte standalone ; Proxmox = plateforme
datacenter complète (HA, backups planifiés, firewall, SDN).

---

## 73. Proxmox VE — Migrer entre LXD et Proxmox

**De LXD vers Proxmox :**

```bash
# 1. Sur l'hôte LXD : exporter
lxc stop c1
lxc export c1 /tmp/c1.tar.gz

# 2. Transférer vers Proxmox
scp /tmp/c1.tar.gz root@proxmox:/tmp/

# 3. Sur Proxmox : extraire le rootfs
mkdir -p /tmp/c1-rootfs
tar -xzf /tmp/c1.tar.gz -C /tmp/c1-rootfs
# Le rootfs se trouve dans ./rootfs/ de l'archive

# 4. Créer un CT avec ce rootfs (méthode manuelle)
pct create 100 local:vztmpl/ubuntu-24.04-standard_24.04-2_amd64.tar.zst \
  --rootfs local-lvm:20 --hostname c1-migre ...
# Puis remplacer le contenu du volume par le rootfs exporté
```

> 📝 Il n'existe pas de bouton "import LXD" dans Proxmox : la migration
> passe par l'export du rootfs + recréation. Pour des applis conteneurisées
> proprement (Docker, Ansible), **re-déployer** est souvent plus propre que
> migrer.

**De Proxmox vers LXD :** `vzdump` → extraire le `.tar.zst` → `lxc import`
via un rootfs re-packagé, ou `lxc publish` après recréation manuelle.

---

## 74. Proxmox VE — Bonnes pratiques templates

- **Templates standardisés :** un template par OS majeur, mis à jour
  mensuellement (`pveam download` + patch + `vzdump` en template).
- **Cloud-init :** utilisez-le pour hostname, clés SSH, IP (`--ciuser`,
  `--sshkey`, `--ipconfig0`) → conteneurs reproductibles.
- **Unprivileged par défaut** (`--unprivileged 1`), nesting seulement si besoin.
- **Backups `vzdump` planifiés** (datacenter → backup → ajouter) : c'est
  l'avantage n°1 de Proxmox sur LXD standalone.
- **Séparez OS et données** : disque système sur `local-lvm`, données sur
  volume dédié (monté via `pct set 100 -mp0 ...`).

```bash
# Exemple : création reproductible avec cloud-init
pct create 101 local:vztmpl/debian-12-standard_12.7-1_amd64.tar.zst \
  --hostname db1 --cores 4 --memory 8192 \
  --rootfs ceph-vm:30 \
  --net0 name=eth0,bridge=vmbr0,ip=10.0.0.21/24,gw=10.0.0.1 \
  --ciuser admin --sshkey ~/.ssh/id_ed25519.pub \
  --unprivileged 1 --onboot 1 --startup order=2,up=30
```

---

## 75. Cas pratique 1 — Conteneur web (nginx + PHP)

**Objectif :** un serveur web isolé, exposé sur le port 8080 de l'hôte.

```bash
# 1. Création avec le profil web (section 21)
lxc launch ubuntu:24.04 web1 --profile default --profile web

# 2. Installation
lxc exec web1 -- apt update
lxc exec web1 -- apt install -y nginx php-fpm php-mysql

# 3. Déployer le site
lxc file push -r ./monsite/ web1/var/www/html/ --create-dirs
lxc exec web1 -- chown -R www-data:www-data /var/www/html

# 4. Config nginx (via file edit ou push)
lxc file push ./web1-nginx.conf web1/etc/nginx/sites-available/monsite
lxc exec web1 -- ln -sf /etc/nginx/sites-available/monsite /etc/nginx/sites-enabled/
lxc exec web1 -- nginx -t && systemctl reload nginx

# 5. Exposer le port (si pas déjà dans le profil)
lxc config device add web1 http proxy listen=tcp:0.0.0.0:8080 connect=tcp:127.0.0.1:80

# 6. Snapshot de mise en service
lxc snapshot web1 mise-en-service
```

**Vérification :** `curl http://hote:8080` → page du site.
**Supervision :** alerte si le port 8080 ne répond plus (section 65).

---

## 76. Cas pratique 2 — Conteneur de build / CI

**Objectif :** un runner de build jetable, reconstruit à chaque pipeline.

```bash
# 1. Profil build (fort CPU, disque large, éphémère)
lxc profile create build
lxc profile set build limits.cpu 8
lxc profile set build limits.memory 16GiB
lxc profile device add build root disk path=/ pool=default size=100GiB

# 2. Lancement depuis la golden image (section 40)
lxc launch build-golden-v3 build-$CI_PIPELINE_ID --profile default --profile build

# 3. Build
lxc exec build-$CI_PIPELINE_ID -- su - builder -c "cd /src && make -j8"

# 4. Récupérer les artefacts
lxc file pull -r build-$CI_PIPELINE_ID/home/builder/dist/ ./artefacts/

# 5. Destruction (zéro trace)
lxc delete build-$CI_PIPELINE_ID --force
```

**Avantages vs Docker :** environnement système complet (systemd, multi-services),
isolation forte, démarrage en 2 s, coût nul de reconstruction.

---

## 77. Cas pratique 3 — Isolation d'un service (PostgreSQL)

**Objectif :** PostgreSQL dédié, données sur volume séparé, sauvegardé.

```bash
# 1. Volume de données
lxc storage volume create default pgdata-prod size=200GiB

# 2. Conteneur avec profil db
lxc launch ubuntu:24.04 pg1 --profile default --profile db

# 3. Attacher le volume
lxc config device add pg1 pgdata disk pool=default source=pgdata-prod \
  path=/var/lib/postgresql

# 4. Installer et configurer
lxc exec pg1 -- apt update && lxc exec pg1 -- apt install -y postgresql-16
lxc exec pg1 -- systemctl enable --now postgresql

# 5. Durcissement : n'écouter que sur le bridge interne
lxc exec pg1 -- bash -c "echo \"listen_addresses = '10.10.10.60'\" >> /etc/postgresql/16/main/postgresql.conf"
lxc config device set pg1 eth0 ipv4.address 10.10.10.60
lxc restart pg1

# 6. Snapshots + backup du volume
lxc config set pg1 snapshots.schedule "@daily"
lxc config set pg1 snapshots.expiry 7d
```

**Restauration :** volume `pgdata-prod` réattachable à un nouveau conteneur
en cas de problème OS (données intactes).

---

## 78. Cas pratique 4 — Reverse proxy multi-sites

**Objectif :** un conteneur nginx qui route vers N conteneurs web internes.

```
Internet → hote:80/443 → [ct reverse-proxy] → web1 (10.10.10.11)
                                          → web2 (10.10.10.12)
                                          → api  (10.10.10.13)
```

```bash
# 1. Conteneur proxy
lxc launch ubuntu:24.04 rproxy --profile default
lxc exec rproxy -- apt install -y nginx certbot python3-certbot-nginx

# 2. Exposer 80/443 de l'hôte vers le conteneur
lxc config device add rproxy http proxy listen=tcp:0.0.0.0:80 connect=tcp:127.0.0.1:80
lxc config device add rproxy https proxy listen=tcp:0.0.0.0:443 connect=tcp:127.0.0.1:443

# 3. Config nginx (exemple : deux vhosts)
lxc file push ./rproxy-sites.conf rproxy/etc/nginx/conf.d/sites.conf
```

```nginx
# /etc/nginx/conf.d/sites.conf dans rproxy
upstream web1 { server 10.10.10.11:80; }
upstream web2 { server 10.10.10.12:80; }

server {
    listen 80;
    server_name site1.exemple.fr;
    location / { proxy_pass http://web1; proxy_set_header Host $host; }
}
server {
    listen 80;
    server_name site2.exemple.fr;
    location / { proxy_pass http://web2; proxy_set_header Host $host; }
}
```

```bash
lxc exec rproxy -- nginx -t && lxc exec rproxy -- systemctl reload nginx

# 4. TLS avec Let's Encrypt (le challenge HTTP passe par le proxy)
lxc exec rproxy -- certbot --nginx -d site1.exemple.fr -d site2.exemple.fr
```

---

## 79. Cas pratique 5 — Environnement de dev par développeur

**Objectif :** chaque dev a son conteneur identique, recréable en 1 commande.

```bash
# 1. Golden image "dev" (outils communs préinstallés)
lxc launch ubuntu:24.04 dev-template
lxc exec dev-template -- apt install -y git build-essential python3-venv \
  nodejs npm docker.io zsh tmux htop
# ... dotfiles, config git globale ...
lxc stop dev-template
lxc publish dev-template --alias dev-golden-v1

# 2. Création pour un nouveau dev (1 commande)
lxc launch dev-golden-v1 dev-alice --profile default
lxc config device add dev-alice home disk \
  source=/home/alice source=/dev/null 2>/dev/null || true
# Mieux : volume persistant par dev
lxc storage volume create default dev-alice-home size=50GiB
lxc config device add dev-alice home disk pool=default \
  source=dev-alice-home path=/home/dev

# 3. Le dev s'y connecte en SSH ou via lxc exec
lxc exec dev-alice -- su - dev
```

**Reset du poste dev :** `lxc delete dev-alice --force` + relance →
environnement neuf en 10 secondes, home préservé sur le volume.

---

## 80. Erreurs classiques — 12 pièges et leurs solutions

### Erreur 1 : `Error: Get "http://unix/1.0": dial unix ... permission denied`

**Cause :** l'utilisateur n'est pas dans le groupe `lxd`.
**Solution :**

```bash
sudo usermod -aG lxd "$USER"
newgrp lxd   # ou reconnexion complète
```

### Erreur 2 : Le conteneur démarre mais n'a pas d'IP

**Causes possibles :** dnsmasq du bridge en conflit, DHCP désactivé, image sans
client DHCP.

```bash
# Vérifier dnsmasq
lxc network show lxdbr0 | grep ipv4.dhcp
# Voir les baux
sudo cat /var/snap/lxd/common/lxd/networks/lxdbr0/dnsmasq.leases
# Redémarrer le réseau LXD
lxc network set lxdbr0 ipv4.dhcp false && lxc network set lxdbr0 ipv4.dhcp true
lxc restart c1
```

### Erreur 3 : `Failed to run: forklimits: ...` / limites non appliquées

**Cause :** clé mal orthographiée (`limits.memroy`) — LXD ne valide pas tout.
**Solution :** `lxc config show c1 --expanded` et relire chaque clé.

### Erreur 4 : `lxc exec` se fige / pas de sortie

**Cause :** pas de TTY dans un script.
**Solution :** `lxc exec c1 --mode non-interactive -- <cmd>`.

### Erreur 5 : Plus de place sur le pool (`no space left on device`)

```bash
# Diagnostic
lxc storage info default
sudo zfs list -o name,used,avail | head -20
# Nettoyage : vieux snapshots, images inutilisées
lxc image list   # repérer les inutilisées
lxc query /1.0/instances/c1/snapshots -X DELETE  # au cas par cas
```

### Erreur 6 : Conteneur en macvlan injoignable depuis l'hôte

**Normal !** Limitation du noyau (section 26). Solutions : ajouter une
interface sur le bridge, ou passer en `bridged` + proxy.

### Erreur 7 : `security.nesting` oublié → Docker ne démarre pas dans LXC

Symptôme : `docker run` échoue avec des erreurs de montage/cgroups.
**Solution :** section 51-52 (`security.nesting=true` + interceptions).

### Erreur 8 : Droits incohérents sur un dossier partagé hôte ↔ conteneur

**Cause :** oubli du décalage UID (section 49).
**Solution :** `chown 1000000:1000000` côté hôte, ou `shift=true` sur le device.

### Erreur 9 : Snapshot `restore` qui écrase le travail en cours

**Réflexe :** toujours `lxc snapshot c1 avant-restore` avant un restore.
Le restore est destructif pour l'état actuel.

### Erreur 10 : Mise à jour snap qui redémarre les conteneurs

Le refresh snap de LXD peut impacter le daemon. **Solution :**

```bash
sudo snap set lxd refresh.hold="$(date -d '+7 days' +%Y-%m-%dT%H:%M:%S%:z)"
# Planifier les refreshs en fenêtre de maintenance
sudo snap refresh lxd   # manuel, en heure creuse
```

### Erreur 11 : `lxc copy` inter-hôtes très lent

**Cause :** copie sans optimisation entre backends différents.
**Solution :** `--optimized-storage`, ou mieux : même backend ZFS des deux
côtés + `lxc copy --refresh` pour les deltas.

### Erreur 12 : Oubli du `boot.autostart` → services morts après reboot hôte

```bash
# Appliquer à tout le parc prod d'un coup
for ct in web1 web2 pg1 rproxy; do
  lxc config set $ct boot.autostart true
  lxc config set $ct boot.autostart.priority 10
done
```

---

## 81. Dépannage — Méthode en 7 étapes

```
1. DÉFINIR    → Quel est le symptôme exact ? Depuis quand ? Qu'est-ce qui a changé ?
2. PÉRIMÈTRE  → Un conteneur ou tous ? Réseau ? Stockage ? Daemon ?
3. OBSERVER   → lxc list, lxc info, lxc monitor, journalctl -u snap.lxd.daemon
4. ISOLER     → Reproduire sur un conteneur test minimal
5. HYPOTHÈSE  → Une cause à la fois, la plus simple d'abord
6. CORRIGER   → Snapshot avant toute manip destructive !
7. DOCUMENTER → Noter la cause et la solution (ce guide, section 80)
```

**Commandes de premier réflexe :**

```bash
lxc list                              # état global
lxc info c1 --show-log                # log console du conteneur
lxc monitor --type=logging --pretty   # logs temps réel du daemon
sudo journalctl -u snap.lxd.daemon -n 100   # daemon (snap)
sudo journalctl -u lxd -n 100              # daemon (apt)
lxc network show lxdbr0               # état réseau
lxc storage info default              # état stockage
```

---

## 82. Cas de dépannage concrets

### Cas A : Un conteneur ne démarre plus après MAJ de l'hôte

```bash
# 1. Message d'erreur exact
lxc start c1
# Error: Failed to run: /snap/lxd/.../lxd forklimits ...

# 2. Souvent : module noyau ou AppArmor après reboot
sudo dmesg | grep -i apparmor | tail -20
sudo systemctl restart snap.lxd.daemon

# 3. Si le rootfs semble corrompu : monter le volume à la main (ZFS)
sudo zfs list | grep c1
sudo mkdir -p /mnt/rescue && sudo mount -t zfs default/containers/c1 /mnt/rescue
# → récupérer les données, puis recréer le conteneur
```

### Cas B : Lenteurs disque soudaines sur tous les conteneurs

```bash
# 1. Le pool est-il plein à > 80 % ? (ZFS s'effondre quand il est plein)
sudo zpool list -o name,size,alloc,free,cap

# 2. Scrub en cours ?
sudo zpool status | grep -A2 scrub

# 3. Un conteneur sature l'I/O ?
lxc info c1 | grep -A5 Disk
# → appliquer limits.read/write (section 43) au fautif

# 4. Santé des disques
sudo smartctl -a /dev/sdb | grep -i "reallocated\|pending"
```

### Cas C : Perte réseau d'un seul conteneur

```bash
# 1. Le veth existe-t-il côté hôte ?
ip link show | grep -i c1

# 2. Le conteneur voit-il son interface ?
lxc exec c1 -- ip addr show eth0

# 3. DHCP : le conteneur demande-t-il ?
lxc exec c1 -- dhclient -v eth0   # ou systemctl restart systemd-networkd

# 4. Règles firewall de l'hôte qui bloquent le forward ?
sudo nft list ruleset | grep -i forward
sudo iptables -L FORWARD -n -v | head

# 5. Solution rapide : recréer le device réseau
lxc config device remove c1 eth0
lxc config device add c1 eth0 nic network=lxdbr0 name=eth0
lxc restart c1
```

### Cas D : `lxc exec` impossible (conteneur gelé)

```bash
# Le daemon répond-il encore ?
lxc list --fast

# Forcer l'arrêt puis redémarrer
lxc stop c1 --force
lxc start c1

# Si le daemon lui-même est bloqué :
sudo systemctl restart snap.lxd.daemon
# Les conteneurs SURVIVENT au redémarrage du daemon (ils sont indépendants)
```

> 💡 Point clé : **le daemon LXD n'est pas dans le chemin d'exécution** des
> conteneurs. Le redémarrer ne tue pas les conteneurs — rassurant en prod.

---

## 83. Pense-bête de poche — Cheat sheet

```
=== CYCLE DE VIE ===
lxc launch ubuntu:24.04 c1            créer + démarrer
lxc init ubuntu:24.04 c1              créer sans démarrer
lxc start|stop|restart|freeze c1      contrôler
lxc delete c1 [--force]               supprimer
lxc list [-c n,s,4]                   lister

=== ACCÈS ===
lxc exec c1 -- bash                   shell root
lxc exec c1 --user 1000 -- whoami     en tant qu'autre user
lxc shell c1                          shell rapide
lxc file push f c1/path               envoyer fichier
lxc file pull c1/path f               récupérer fichier
lxc file edit c1/etc/hosts            éditer

=== CONFIG ===
lxc config show c1 --expanded         config complète
lxc config set c1 limits.memory 4GiB  définir
lxc config unset c1 limits.memory     hériter du profil
lxc config edit c1                    éditer YAML

=== PROFILS ===
lxc profile create|edit|list|show p   gérer
lxc profile add c1 p                  appliquer
lxc launch img c1 -p p1 -p p2         appliquer au lancement

=== RÉSEAU ===
lxc network list|show lxdbr0          réseaux
lxc network create dmz0 ipv4.address=192.168.50.1/24 ipv4.nat=false
lxc config device set c1 eth0 ipv4.address 10.10.10.50
lxc config device add c1 http proxy listen=tcp:0.0.0.0:8080 connect=tcp:127.0.0.1:80

=== STOCKAGE ===
lxc storage list|info default         pools
lxc storage volume create default v1 size=50GiB
lxc config device add c1 d disk pool=default source=v1 path=/data

=== SNAPSHOTS ===
lxc snapshot c1 [nom]                 créer
lxc restore c1 nom                    restaurer (destructif !)
lxc delete c1/nom                     supprimer
lxc publish c1 --alias golden-v1      publier en image

=== LIMITES ===
lxc config set c1 limits.cpu 2
lxc config set c1 limits.memory 4GiB
lxc config set c1 limits.processes 512

=== SÉCURITÉ ===
lxc config set c1 security.nesting true      # Docker dans LXC
lxc config get c1 security.privileged        # doit être false

=== BACKUP ===
lxc export c1 /backups/c1.tar.gz
lxc import /backups/c1.tar.gz [nouveau-nom]

=== INFOS ===
lxc info c1                           détails + métriques
lxc info c1 --show-log                log console
lxc monitor --pretty                  événements temps réel

=== PROXMOX (équivalents) ===
pct create|start|stop|destroy          cycle de vie
pct exec|enter <vmid>                 shell
pct push|pull <vmid>                  fichiers
pct snapshot <vmid> <nom>             snapshot
vzdump <vmid>                         backup intégré
```

---

## 84. Glossaire

| Terme | Définition |
|---|---|
| **LXC** | Technologie noyau de conteneurs (namespaces + cgroups) + outils bas niveau |
| **LXD** | Gestionnaire de conteneurs système construit sur LXC (daemon + CLI + API) |
| **Namespace** | Mécanisme noyau d'isolation (pid, net, mnt, uts, ipc, user, cgroup) |
| **Cgroup** | Mécanisme noyau de limitation/comptage des ressources (CPU, RAM, I/O) |
| **Image** | Modèle de système de fichiers pour créer des conteneurs |
| **Remote** | Serveur d'images (`ubuntu:`, `images:`, `local:`) |
| **Profil** | Ensemble réutilisable de config/devices appliqué aux conteneurs |
| **Pool** | Réserve de stockage gérée par LXD (dir, zfs, btrfs, lvm, ceph) |
| **Volume** | Unité de stockage dans un pool (rootfs ou volume custom) |
| **Snapshot** | Photographie instantanée d'un conteneur (restauration rapide) |
| **Device** | Ressource attachée : `disk`, `nic`, `proxy`, `gpu`, `usb`, `unix-char`… |
| **Bridge** | Commutateur virtuel Linux reliant conteneurs et/ou physique |
| **macvlan** | NIC virtuelle exposant le conteneur directement sur le LAN |
| **Nesting** | Exécuter des conteneurs dans un conteneur (ex. Docker dans LXC) |
| **Privilégié** | Conteneur dont root = root hôte (à éviter absolument) |
| **idmap** | Correspondance UID/GID conteneur ↔ hôte (user namespace) |
| **AppArmor** | MAC : confine les programmes par profils (complète LXD) |
| **seccomp** | Filtre les appels système autorisés dans le conteneur |
| **COW** | Copy-On-Write : les snapshots/clones ne dupliquent que les différences |
| **OVN** | Réseau virtuel distribué (overlay multi-hôtes pour LXD) |
| **CRIU** | Migration à chaud (checkpoint/restore) des processus |
| **pct** | CLI Proxmox pour les conteneurs LXC |
| **vzdump** | Outil de backup intégré de Proxmox VE |
| **cloud-init** | Standard de configuration initiale (hostname, SSH, réseau…) |

---

## 85. Quiz — 10 questions + réponses

**Q1. Quelle est la différence fondamentale entre LXC et LXD ?**
> R : LXC est la technologie bas niveau (bibliothèque + outils) ; LXD est le
> gestionnaire complet (daemon, API REST, réseau, stockage, images, clustering)
> construit au-dessus de LXC.

**Q2. Pourquoi un conteneur LXC démarre-t-il en moins de 2 secondes ?**
> R : Il partage le noyau de l'hôte : pas de BIOS, pas de bootloader, pas de
> noyau à charger — seul l'init du conteneur (systemd) démarre.

**Q3. Que signifie `lxc launch` par rapport à `lxc init` ?**
> R : `launch` = `init` (créer) + `start` (démarrer). `init` seul permet de
> configurer le conteneur avant son premier démarrage.

**Q4. Un membre du groupe `lxd` peut-il devenir root sur l'hôte ?**
> R : Oui. Via un conteneur privilégié ou un montage du disque hôte, c'est
> une équivalence root. Ne jamais y mettre d'utilisateur non-admin.

**Q5. Pourquoi préférer ZFS comme backend de stockage ?**
> R : Snapshots/clones instantanés (COW), copies quasi gratuites, compression
> native, quotas — idéal pour l'exploitation intensive de conteneurs.

**Q6. Quelle commande expose le port 80 d'un conteneur sur le port 8080 de l'hôte ?**
> R : `lxc config device add c1 http proxy listen=tcp:0.0.0.0:8080 connect=tcp:127.0.0.1:80`

**Q7. Que faut-il activer pour faire tourner Docker dans un conteneur LXC ?**
> R : `security.nesting=true` (et idéalement les interceptions de syscalls
> `mknod`/`setxattr`), puis redémarrer le conteneur.

**Q8. Un snapshot LXD remplace-t-il une sauvegarde externalisée ?**
> R : Non. Il protège des erreurs humaines et permet un rollback rapide, mais
> il vit sur le même disque/hôte : il ne protège ni du crash disque ni du site.

**Q9. Quelle est la limitation connue du macvlan concernant l'hôte ?**
> R : L'hôte ne peut pas communiquer avec ses propres conteneurs macvlan
> (filtrage au niveau du parent par le noyau).

**Q10. Citez trois équivalences entre LXD et Proxmox `pct`.**
> R : `lxc exec` ↔ `pct exec`/`pct enter` ; `lxc file push/pull` ↔
> `pct push/pull` ; `lxc snapshot` ↔ `pct snapshot` ; `lxc export` ↔ `vzdump`.

---

## 86. Pour aller plus loin

**Documentation officielle :**

- LXD — https://documentation.ubuntu.com/lxd/ (référence complète, API REST)
- LinuxContainers.org — https://linuxcontainers.org/ (LXC, images `images:`)
- Proxmox VE Wiki — https://pve.proxmox.com/wiki/ (LXC, `pct`, `vzdump`)

**Sujets d'approfondissement :**

1. **API REST LXD** : tout ce que fait la CLI passe par l'API — scriptez en
   Python/Go (`lxc query` pour explorer).
2. **OVN** : réseaux overlay multi-hôtes avec routage distribué et ACLs.
3. **Projets LXD** : multi-tenant, quotas, RBAC fin.
4. **Terraform provider LXD/Proxmox** : infra as code pour vos conteneurs.
5. **Ansible** : module `community.general.lxd_container`, inventaire dynamique.
6. **Ceph + LXD/Proxmox** : stockage distribué pour clusters.
7. **Confidential computing / Kata** : quand le noyau partagé ne suffit plus.

**Routines d'équipe suggérées :**

- Revue mensuelle : images à jour ? snapshots qui s'accumulent ? quotas OK ?
- Test trimestriel de restauration de sauvegarde (section 61).
- Mise à jour des golden images après chaque patch Tuesday / release Ubuntu.
- Ce guide est vivant : ajoutez-y vos propres erreurs (section 80) au fil de l'eau.

---

*Fin du guide — bon courage avec vos conteneurs !* 🚀
