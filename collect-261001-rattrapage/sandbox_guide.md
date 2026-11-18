# Guide complet du sandboxing sous Linux

**Isolation, confinement et sécurité pratique pour sysadmin**
*Public : Zelef, chef de service systèmes & énergies — approche opérationnelle, exemples testés sur Debian/Ubuntu.*

> Ce guide couvre les mécanismes d'isolation Linux (namespaces, cgroups v2, chroot), les outils de confinement (Firejail, systemd, AppArmor, SELinux, Docker) et les cas d'usage concrets : ouvrir un PDF suspect, durcir un navigateur, compiler du code tiers, isoler une application non fiable. Chaque section contient des exemples de code prêts à l'emploi.

---

## 1. Pourquoi sandboxer ? Le problème de départ

Un système Linux classique repose sur la confiance : tout processus lancé par un utilisateur hérite de ses droits et peut, en théorie, lire ses fichiers, ouvrir des sockets réseau, ou exploiter une faille du noyau. Le sandboxing inverse cette logique : **on part du principe que le programme est hostile ou compromis**, et on lui retire tout ce dont il n'a pas strictement besoin.

Cas concrets rencontrés en production :

- Un PDF reçu par mail exploite une faille du lecteur PDF → sans sandbox, l'attaquant lit `~/.ssh/id_rsa`.
- Un paquet `.deb` téléchargé hors dépôt exécute un script `postinst` malveillant → sans confinement, il installe un service persistant.
- Un outil de compilation tiers (`npm install`, `pip install`, `make`) exécute du code arbitraire pendant le build.
- Un navigateur web exécute du JavaScript hostile en permanence.

Le sandbox ne supprime pas les vulnérabilités : il **réduit l'impact** de leur exploitation. C'est la différence entre "la machine est compromise" et "un processus isolé a planté".

## 2. Modèle de menace : à quoi se préparer

Avant de choisir un outil, définissez ce que vous craignez :

| Menace | Exemple | Contre-mesure principale |
|---|---|---|
| Lecture de données sensibles | Malware lisant `~/.ssh/`, `~/Documents` | Namespaces `mnt`, `ProtectHome` |
| Exfiltration réseau | Reverse shell vers l'attaquant | Namespace `net`, `RestrictAddressFamilies` |
| Persistance | Installation d'un cron, d'un service systemd | Filesystem read-only, `ProtectSystem=strict` |
| Élévation de privilèges | Exploitation setuid, `sudo` | `NoNewPrivileges`, user namespace |
| Épuisement de ressources | Fork bomb, remplissage disque | cgroups (pids, memory, cpu) |
| Évasion du sandbox | Exploitation d'une faille noyau | Seccomp, AppArmor, noyau à jour |

**Règle d'or :** un sandbox n'est jamais parfait. On empile les couches (défense en profondeur) : namespace + seccomp + AppArmor + cgroups + filesystem read-only.

## 3. Les namespaces Linux : la brique de base

Un namespace est une vue isolée d'une ressource système. Deux processus dans des namespaces différents voient des ressources différentes. Le noyau Linux en propose 8 :

| Namespace | Flag `clone()` | Isole | Depuis le noyau |
|---|---|---|---|
| PID | `CLONE_NEWPID` | Les identifiants de processus | 2.6.24 |
| NET | `CLONE_NEWNET` | Interfaces, routes, iptables, ports | 2.6.24 |
| MNT | `CLONE_NEWNS` | Les points de montage (arborescence `/`) | 2.4.19 |
| UTS | `CLONE_NEWUTS` | Hostname et nom de domaine NIS | 2.6.19 |
| IPC | `CLONE_NEWIPC` | Mémoire partagée, sémaphores, files de messages | 2.6.19 |
| USER | `CLONE_NEWUSER` | Les UID/GID (root mappé vers un UID non privilégié) | 3.8 |
| CGROUP | `CLONE_NEWCGROUP` | La vue des cgroups | 4.6 |
| TIME | `CLONE_NEWTIME` | L'horloge (monotonic, boottime) | 5.6 |

Vérifier les namespaces d'un processus :

```bash
# Lister les namespaces du processus 1 (init)
ls -l /proc/1/ns/

# Lister les namespaces du shell courant
lsns

# Version lisible : quel processus est dans quel namespace
lsns -t net,mnt,pid,user
```

Exemple de sortie `lsns` :

```
        NS TYPE   NPROCS   PID USER COMMAND
4026531836 pid       187     1 root /sbin/init
4026531837 user      187     1 root /sbin/init
4026531840 mnt       186     1 root /sbin/init
4026531992 net         2  3456 toto /usr/bin/firefox
```

Ici, le processus 3456 (Firefox) est dans un namespace réseau dédié (`4026531992`) : il ne voit pas les interfaces de l'hôte.

## 4. Namespace PID : isoler la vue des processus

Dans un namespace PID, le premier processus devient le PID 1 de ce namespace, et `ps` ne montre que les processus du namespace.

```bash
# Créer un shell dans de nouveaux namespaces pid + mnt + uts
sudo unshare --pid --mount --uts --fork --mount-proc /bin/bash

# Dans ce shell :
ps aux        # ne montre que les processus du namespace
hostname test # le hostname de l'hôte n'est pas modifié
```

Points importants :

- `--mount-proc` est nécessaire pour remonter un `/proc` cohérent avec le nouveau namespace PID.
- Le PID 1 du namespace a un rôle spécial : il adopte les processus orphelins. Si votre PID 1 meurt, tout le namespace meurt.
- `unshare` sans `--fork` ne fonctionne pas pour `pid` : le namespace PID ne s'applique qu'aux processus enfants.

## 5. Namespace NET : isoler le réseau

Un namespace réseau vierge ne contient que l'interface `lo` (éteinte). On peut y créer des paires `veth` pour le relier à l'hôte.

```bash
# Créer un namespace réseau nommé "sandbox"
sudo ip netns add sandbox

# Le lister
ip netns list

# Exécuter une commande dedans
sudo ip netns exec sandbox ip addr
# -> seulement lo, DOWN

# Activer lo et tester
sudo ip netns exec sandbox ip link set lo up
sudo ip netns exec sandbox ping -c 2 127.0.0.1

# Relier à l'hôte avec une paire veth
sudo ip link add veth-host type veth peer name veth-sb
sudo ip link set veth-sb netns sandbox
sudo ip addr add 10.200.0.1/24 dev veth-host
sudo ip link set veth-host up
sudo ip netns exec sandbox ip addr add 10.200.0.2/24 dev veth-sb
sudo ip netns exec sandbox ip link set veth-sb up
sudo ip netns exec sandbox ip route add default via 10.200.0.1

# Tester la connectivité
sudo ip netns exec sandbox ping -c 2 10.200.0.1

# Nettoyer
sudo ip netns del sandbox
sudo ip link del veth-host
```

C'est exactement ce que font Docker et Firejail sous le capot pour isoler le réseau d'un conteneur.

## 6. Namespace MNT : isoler le système de fichiers

Le namespace `mnt` permet de monter/démonter des systèmes de fichiers sans affecter l'hôte. Combiné à `pivot_root` ou `chroot`, c'est la base des conteneurs.

```bash
sudo unshare --mount --fork /bin/bash

# Dans le shell isolé :
mount -t tmpfs tmpfs /tmp     # /tmp de l'hôte masqué, sans l'affecter
mount -o remount,ro /          # racine en lecture seule (dans ce namespace)
mount | grep " / "             # vérifier
exit                           # l'hôte n'a rien vu
```

Attention : sans précaution, les montages se propagent (montages "shared" par défaut avec systemd). Pour un vrai sandbox, passez les montages en `private` :

```bash
mount --make-rprivate /
```

## 7. Namespace USER : le faux root

Le namespace `user` est le plus puissant : il permet à un utilisateur non privilégié d'avoir l'UID 0 **à l'intérieur** du namespace, sans aucun privilège sur l'hôte.

```bash
# Créer un user namespace : uid 0 dedans = votre uid dehors
unshare --user --map-root-user /bin/bash

id -u      # affiche 0 !
touch /test_root  # échoue : pas de droit réel sur l'hôte
```

```bash
# Mapping manuel : uid 0-999 dedans -> 100000-100999 dehors
unshare --user --map-users=0:100000:1000 --map-groups=0:100000:1000 /bin/bash
```

C'est ce mécanisme qui permet à Docker "rootless" et à `podman` de fonctionner sans daemon root. Limite : certaines distributions désactivent les user namespaces non privilégiés (`kernel.unprivileged_userns_clone=0` sur Debian par défaut à 1, heureusement).

Vérifier :

```bash
sysctl kernel.unprivileged_userns_clone
# 1 = autorisé (nécessaire pour Firejail, podman, bubblewrap)
```

## 8. cgroups v2 : limiter les ressources

Les namespaces isolent la **vue**, les cgroups limitent les **ressources** : CPU, mémoire, nombre de processus, I/O disque.

```bash
# Vérifier que cgroups v2 est actif
mount | grep cgroup2
# cgroup2 on /sys/fs/cgroup type cgroup2 (...)

# Créer un cgroup pour un programme suspect
sudo mkdir /sys/fs/cgroup/sandbox.d/

# Limites : 512 Mo de RAM, 50% d'un CPU, 100 processus max
echo 536870912      | sudo tee /sys/fs/cgroup/sandbox.d/memory.max
echo 50000           | sudo tee /sys/fs/cgroup/sandbox.d/cpu.max
# (format cpu.max : quota_us period_us ; 50000 100000 = 50%)
echo 100             | sudo tee /sys/fs/cgroup/sandbox.d/pids.max

# Lancer un programme dans ce cgroup
sudo systemd-run --unit=sandbox-test --scope \
  -p MemoryMax=512M -p CPUQuota=50% -p TasksMax=100 \
  /usr/bin/programme-suspect --option

# Surveiller
systemd-cgtop
cat /sys/fs/cgroup/sandbox.d/memory.current
cat /sys/fs/cgroup/sandbox.d/pids.current
```

Avec systemd, c'est encore plus simple : les directives `MemoryMax=`, `CPUQuota=`, `TasksMax=` dans une unité font exactement cela (voir section 44).

## 9. chroot vs vrai sandbox : la différence fondamentale

`chroot` ne change que la racine apparente du système de fichiers. **Ce n'est pas un mécanisme de sécurité** :

| Propriété | chroot | Vrai sandbox (namespaces) |
|---|---|---|
| Isole le filesystem | Partiellement (racine redéfinie) | Oui (mnt ns + pivot_root) |
| Isole les processus | Non (`ps` voit tout) | Oui (pid ns) |
| Isole le réseau | Non | Oui (net ns) |
| Isole les UID | Non | Oui (user ns) |
| Évasion possible | Oui, triviale si root (voir §15) | Difficile (nécessite faille noyau) |
| Limite les ressources | Non | Oui (cgroups) |
| Filtre les appels système | Non | Oui (seccomp) |

Un processus root dans un chroot peut s'en échapper en quelques lignes de C (double chroot + `chdir`). Retenez : **chroot = commodité d'empaquetage et de build, jamais une barrière de sécurité à lui seul**.

## 10. Seccomp : filtrer les appels système

Seccomp-bpf permet de filtrer les appels système qu'un processus peut faire. C'est le dernier rempart : même si un attaquant contrôle le processus, il ne peut pas appeler `mount()`, `ptrace()`, `reboot()`, etc.

```bash
# Voir le filtre seccomp d'un processus (1 = actif)
grep Seccomp /proc/1/status
# Seccomp: 2  (2 = SECCOMP_MODE_FILTER)

# systemd peut appliquer un filtre par défaut à un service :
# SystemCallFilter=@system-service
# SystemCallArchitectures=native
```

Docker applique un profil seccomp par défaut qui bloque ~44 appels système dangereux (`mount`, `ptrace`, `reboot`, `swapon`, `kexec_load`, ...). Firejail fait de même avec `seccomp`. C'est invisible à l'usage, mais redoutablement efficace.

## 11. Capabilities : découper le pouvoir de root

Le "root" monolithique est découpé en ~40 capabilities. Un sandbox retire celles qui sont inutiles :

```bash
# Voir les capabilities d'un processus
grep Cap /proc/1/status
capsh --decode=00000000a80425fb

# Lister les capabilities d'un binaire fichier
getcap /usr/bin/ping
# /usr/bin/ping cap_net_raw=ep
```

Les plus dangereuses à retirer en priorité dans un sandbox : `CAP_SYS_ADMIN` (équivaut presque à root), `CAP_SYS_PTRACE`, `CAP_NET_ADMIN`, `CAP_SYS_MODULE`, `CAP_DAC_OVERRIDE`.

```bash
# Lancer un programme sans aucune capability (nobody-like)
sudo -u nobody /usr/bin/programme

# Avec capsh : garder uniquement cap_net_bind_service
sudo capsh --drop=all --addamb=cap_net_bind_service \
  --user=nobody -- -c "/usr/bin/mon-serveur"
```

Docker : `--cap-drop=ALL --cap-add=NET_BIND_SERVICE`. Systemd : `CapabilityBoundingSet=` et `AmbientCapabilities=` (voir §46).

## 12. Cartographie des outils : qui fait quoi

| Outil | Namespaces | Seccomp | AppArmor/SELinux | cgroups | Cas d'usage typique |
|---|---|---|---|---|---|
| chroot | Non | Non | Non | Non | Build, rescue |
| Firejail | Oui | Oui | Oui (optionnel) | Non | Applis desktop non fiables |
| systemd (directives) | Oui | Oui | Oui (optionnel) | Oui | Services système |
| bubblewrap (`bwrap`) | Oui | Oui | Non | Non | Brique de base (Flatpak) |
| Docker | Oui | Oui | Oui | Oui | Applis serveur conteneurisées |
| AppArmor seul | Non | Non | Oui | Non | Confinement par profil |
| QEMU/KVM | N/A (VM) | N/A | Oui (sVirt) | Oui | Isolation forte (noyau séparé) |

**Lecture :** pour un poste de travail (PDF suspect, navigateur), Firejail est le meilleur rapport simplicité/efficacité. Pour un service, les directives systemd suffisent souvent. Pour une isolation forte (code totalement hostile), une VM KVM reste la référence.

---

# PARTIE II — CHROOT : COMPRENDRE ET UTILISER SANS ILLUSION

## 13. Créer un chroot Debian minimal avec debootstrap

`debootstrap` construit une arborescence Debian dans un répertoire. C'est l'outil standard.

```bash
# Installation
sudo apt update && sudo apt install -y debootstrap schroot

# Créer un chroot Debian stable (bookworm) dans /srv/chroot/build
sudo mkdir -p /srv/chroot/build
sudo debootstrap --variant=minbase bookworm /srv/chroot/build \
  http://deb.debian.org/debian/

# Variante Ubuntu
sudo debootstrap --variant=minbase noble /srv/chroot/ubuntu-noble \
  http://archive.ubuntu.com/ubuntu/
```

Options utiles :

```bash
# Inclure des paquets dès la création
sudo debootstrap --variant=minbase \
  --include=build-essential,git,ca-certificates \
  bookworm /srv/chroot/build http://deb.debian.org/debian/

# Architecture différente (nécessite qemu-user-static)
sudo apt install -y qemu-user-static
sudo debootstrap --foreign --arch=arm64 bookworm /srv/chroot/arm64 \
  http://deb.debian.org/debian/
sudo chroot /srv/chroot/arm64 /debootstrap/debootstrap --second-stage
```

Taille indicative : un `minbase` bookworm pèse ~250 Mo.

## 14. Entrer dans le chroot et le configurer

```bash
# Monter les pseudo-filesystems indispensables
sudo mount --bind /dev  /srv/chroot/build/dev
sudo mount --bind /dev/pts /srv/chroot/build/dev/pts
sudo mount -t proc proc /srv/chroot/build/proc
sudo mount -t sysfs sys /srv/chroot/build/sys

# Copier la résolution DNS (sinon pas de réseau sortant en noms)
sudo cp /etc/resolv.conf /srv/chroot/build/etc/resolv.conf

# Entrer
sudo chroot /srv/chroot/build /bin/bash

# Dans le chroot : configuration minimale
apt update
apt install -y locales
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen && locale-gen
apt install -y build-essential
exit
```

Script d'aide `/usr/local/bin/chroot-enter` :

```bash
#!/bin/bash
# Usage : chroot-enter /srv/chroot/build [commande...]
CHROOT="$1"; shift
for m in dev dev/pts proc sys; do
  mountpoint -q "$CHROOT/$m" || {
    case $m in
      dev)     mount --bind /dev "$CHROOT/dev" ;;
      dev/pts) mount --bind /dev/pts "$CHROOT/dev/pts" ;;
      proc)    mount -t proc proc "$CHROOT/proc" ;;
      sys)     mount -t sysfs sys "$CHROOT/sys" ;;
    esac
  }
done
[ -f "$CHROOT/etc/resolv.conf" ] || cp /etc/resolv.conf "$CHROOT/etc/resolv.conf"
exec chroot "$CHROOT" "$@"
```

## 15. Pourquoi chroot n'est PAS une sécurité : la démonstration

Un processus **root** dans un chroot peut s'échapper. Voici le principe (ne l'exécutez que dans un labo) :

```c
/* Évasion chroot classique : le "double chroot" ne suffit pas seul ;
   la méthode fiable passe par open() d'un descripteur hors chroot
   avant le chroot, puis fchdir(). */
#include <unistd.h>
#include <fcntl.h>
#include <stdio.h>
int main(void) {
    int fd = open("/", O_RDONLY);   /* descripteur vers la VRAIE racine */
    chroot("/srv/chroot/build");    /* on "entre" dans le chroot */
    fchdir(fd);                     /* ... et on en ressort aussitôt */
    chroot(".");                    /* on redéfinit la racine ici */
    execl("/bin/bash", "bash", NULL);
    return 0;
}
```

Conclusion opérationnelle :

1. Ne lancez **jamais** de programme non fiable en root dans un chroot en croyant être protégé.
2. Si vous devez utiliser chroot, combinez-le avec : utilisateur dédié (` nobody`), `NoNewPrivileges`, montages read-only, et idéalement des namespaces par-dessus (`unshare --map-root-user`).
3. Pour une vraie isolation, préférez `systemd-nspawn` (voir §16) ou `bwrap`.

## 16. systemd-nspawn : le chroot qui devient un conteneur

`systemd-nspawn` est le chaînon manquant entre chroot et Docker : il ajoute namespaces (pid, net, mnt, uts, ipc), cgroups et un vrai PID 1.

```bash
# Lancer le chroot comme un conteneur léger
sudo systemd-nspawn -D /srv/chroot/build

# Avec réseau privé (veth vers l'hôte)
sudo systemd-nspawn -D /srv/chroot/build --network-veth

# Sans privilèges : user namespace
sudo systemd-nspawn -D /srv/chroot/build --private-users=pick

# Lancer une commande unique et quitter
sudo systemd-nspawn -D /srv/chroot/build --as-pid2 \
  /usr/bin/make -C /src

# Lier un répertoire de l'hôte en lecture seule
sudo systemd-nspawn -D /srv/chroot/build \
  --bind-ro=/home/zelef/sources:/src \
  /usr/bin/make -C /src
```

Fichier machine persistant `/etc/systemd/nspawn/build.nspawn` :

```ini
[Exec]
Boot=no
PrivateUsers=yes

[Files]
BindReadOnly=/home/zelef/sources:/src
TemporaryFileSystem=/:ro
Bind=/srv/chroot/build/var/tmp

[Network]
Private=yes
VirtualEthernet=yes
```

Puis `sudo systemd-nspawn -M build /usr/bin/make -C /src`. Les logs partent dans le journal : `journalctl -M build`.

## 17. Cas pratique : compiler du code tiers dans un chroot jetable

Scénario : vous devez compiler un projet téléchargé depuis un dépôt obscur. Le `Makefile` pourrait contenir des commandes malveillantes.

```bash
#!/bin/bash
# build-isole.sh — compile dans un nspawn jetable
set -euo pipefail
SRC="$1"                       # répertoire des sources (hôte)
CHROOT=/srv/chroot/build
MACHINE="build-$$"

# 1. Copie des sources dans un répertoire dédié (pas de bind direct)
WORK=$(mktemp -d)
cp -a "$SRC"/. "$WORK"/
chmod -R a-w "$WORK"           # sources en lecture seule

# 2. Compilation confinée : pas de réseau, user namespace, / en ro
sudo systemd-nspawn --machine="$MACHINE" -D "$CHROOT" \
  --private-users=pick \
  --network-veth --private-network \
  --bind-ro="$WORK:/src" \
  --tmpfs=/tmp --tmpfs=/build \
  --as-pid2 \
  bash -c 'cp -a /src/. /build/ && cd /build && ./configure && make -j"$(nproc)"'

# 3. Récupération des artefacts (on ne fait confiance qu'aux binaires, pas au build)
mkdir -p ./artefacts
sudo cp -a "$WORK"/artefacts/. ./artefacts/ 2>/dev/null || true
rm -rf "$WORK"
echo "OK — vérifiez les artefacts avant usage."
```

Points de vigilance : `--private-network` coupe le réseau (un build qui télécharge des dépendances échouera — c'est voulu) ; les artefacts restent suspects jusqu'à inspection (`strings`, antivirus, test en sandbox).

## 18. Mettre à jour et maintenir un chroot

```bash
# Mise à jour depuis l'hôte sans entrer dedans
sudo chroot /srv/chroot/build apt update
sudo chroot /srv/chroot/build apt -y upgrade
sudo chroot /srv/chroot/build apt -y autoremove --purge
sudo chroot /srv/chroot/build apt clean

# Taille et contenu
sudo du -sh /srv/chroot/build
dpkg-query --admindir=/srv/chroot/build/var/lib/dpkg -W | wc -l

# Supprimer proprement un chroot
sudo umount -R /srv/chroot/build 2>/dev/null
sudo rm -rf /srv/chroot/build
```

Automatisez la mise à jour hebdomadaire avec un timer systemd (voir §44 pour le sandboxing du service lui-même) :

```ini
# /etc/systemd/system/chroot-update.service
[Unit]
Description=Mise à jour du chroot de build

[Service]
Type=oneshot
ExecStart=/usr/bin/chroot /srv/chroot/build /usr/bin/apt-get update
ExecStart=/usr/bin/chroot /srv/chroot/build /usr/bin/apt-get -y upgrade
```

## 19. schroot : gérer plusieurs chroots proprement

`schroot` gère sessions, utilisateurs autorisés et snapshots LVM.

```ini
# /etc/schroot/chroot.d/build.conf
[build]
description=Chroot de compilation Debian
type=directory
directory=/srv/chroot/build
users=zelef
groups=sbuild
root-groups=root
personality=linux
preserve-environment=true
```

```bash
# Entrer comme utilisateur normal (pas root !)
schroot -c build -u zelef

# Session nommée pour un build long
schroot -b -c build -n session-build-1
schroot -r -c session-build-1 -u zelef -- make -C /src
schroot -e -c session-build-1
```

## 20. Limites du chroot : tableau récapitulatif opérationnel

| Risque | chroot seul | chroot + user non-root | nspawn |
|---|---|---|---|
| Lecture `/etc/shadow` hôte | Possible (si monté) | Possible si permissions | Non (fs isolé) |
| Évasion par root | Triviale | N/A (pas root) | Très difficile |
| Accès réseau hôte | Total | Total | Filtrable |
| `ps` voit l'hôte | Oui | Oui | Non |
| Fork bomb | Affecte l'hôte | Affecte l'hôte | Contenue (cgroups) |
| Persistance via cron hôte | Possible | Possible | Non |

**Décision :** chroot = environnement de build reproductible ; nspawn = exécution isolée ; ni l'un ni l'autre ne remplace un audit du code exécuté.

## 21. Checklist chroot avant usage en production

- [ ] Le chroot est-il à jour (`apt upgrade` récent) ?
- [ ] Les montages `/dev`, `/proc`, `/sys` sont-ils démontés après usage ?
- [ ] Aucun secret de l'hôte n'est bind-monté (clés SSH, tokens) ?
- [ ] Le processus s'exécute-t-il en non-root à l'intérieur ?
- [ ] Le réseau est-il nécessaire ? Si non, coupez-le (nspawn `--private-network`).
- [ ] Les artefacts produits sont-ils vérifiés avant réutilisation ?
- [ ] Le chroot est-il reconstruit régulièrement depuis une base saine ?

## 22. Erreurs classiques avec chroot

**Erreur 1 — `chroot: failed to run command '/bin/bash': No such file or directory`**
Cause : le chroot est vide ou l'architecture ne correspond pas (binaire ARM sur hôte x86 sans qemu). Vérifiez `ls /srv/chroot/build/bin/bash` et `file`.

**Erreur 2 — `apt update` échoue : "Temporary failure resolving ..."**
Cause : `/etc/resolv.conf` absent dans le chroot. Copiez celui de l'hôte (voir §14).

**Erreur 3 — Oublier de démonter avant `rm -rf`**
`rm -rf` sur un chroot dont `/dev` est bind-monté peut endommager l'hôte. Toujours `umount -R` d'abord, ou utilisez `schroot -e`.

**Erreur 4 — Croire que `chroot` + mot de passe = sécurité**
Le chroot n'authentifie rien et ne confine rien. Voir §15.

---

# PARTIE III — FIREJAIL : LE SANDBOX DU POSTE DE TRAVAIL

## 23. Firejail : principe et installation

Firejail est un sandbox SUID qui utilise les namespaces Linux, seccomp-bpf et les capabilities pour confiner n'importe quel programme, sans configuration préalable. C'est l'outil le plus rentable pour un poste de travail.

```bash
# Debian / Ubuntu
sudo apt update && sudo apt install -y firejail firejail-profiles

# Vérifier la version (>= 0.9.70 recommandé)
firejail --version

# Vérifier que les user namespaces sont autorisés
sysctl kernel.unprivileged_userns_clone
```

Test immédiat, sans config :

```bash
# Firefox confiné avec le profil par défaut
firejail firefox

# Voir ce que le sandbox bloque
firejail --noprofile --seccomp --private-tmp bash
```

Firejail s'appuie sur `kernel.unprivileged_userns_clone=1` (défaut sur Debian/Ubuntu). Si votre noyau le désactive, Firejail bascule sur son binaire SUID — moins propre, à éviter si possible.

## 24. Profils existants : l'inventaire

Firejail livre plus de 1000 profils dans `/etc/firejail/` :

```bash
ls /etc/firejail/*.profile | wc -l
ls /etc/firejail/ | grep -E "firefox|chrom|thunderbird|evince|okular|vlc|libreoffice"
```

Profils les plus utiles au quotidien :

| Profil | Programme | Protections notables |
|---|---|---|
| `firefox.profile` | Firefox | `private-bin`, `seccomp`, `noroot`, netfilter |
| `chromium.profile` | Chromium | idem + `disable-mnt` |
| `thunderbird.profile` | Thunderbird | `private-tmp`, restriction `~/` |
| `evince.profile` | Lecteur PDF GNOME | `private-bin`, pas de réseau (`net none`) |
| `okular.profile` | Lecteur PDF KDE | idem |
| `vlc.profile` | VLC | `nosound` optionnel, `seccomp` |
| `libreoffice.profile` | LibreOffice | `private-tmp`, `dbus-user none` |
| `wget.profile` / `curl.profile` | Téléchargements | `private-bin`, écriture limitée |

Consulter un profil :

```bash
cat /etc/firejail/firefox.profile
```

## 25. Utilisation quotidienne : les commandes essentielles

```bash
# Lancer avec le profil par défaut (détection automatique)
firejail firefox

# Lancer SANS profil (sandbox générique)
firejail --noprofile evince doc.pdf

# Lancer avec un profil personnalisé
firejail --profile=/home/zelef/.config/firejail/navigateur.profile firefox

# Confinement maximal ad hoc : pas de réseau, /home masqué
firejail --noprofile --net=none --private=/tmp/test --seccomp \
  --caps.drop=all --noroot evince suspect.pdf

# Lister les sandboxes actifs
firejail --list
# 1234:toto:firejail firefox:/usr/bin/firefox

# Voir l'arbre des processus d'un sandbox
firejail --tree

# Arrêter un sandbox
firejail --shutdown=1234

# Top des sandboxes (ressources)
firejail --top
```

Intégration au bureau : Firejail peut remplacer les lanceurs via `firecfg` :

```bash
# Crée des liens symboliques /usr/local/bin/firefox -> /usr/bin/firejail
sudo firecfg
# Désormais, cliquer sur l'icône Firefox lance firejail automatiquement
```

Vérifiez ensuite : `which firefox` doit pointer vers `/usr/local/bin/firefox`.

## 26. Anatomie d'un profil Firejail

Un profil est un fichier texte avec une directive par ligne. Exemple commenté :

```ini
# ~/.config/firejail/navigateur.profile
include firefox.local

# --- Filesystem ---
private-dev                 # /dev minimal (pas d'accès disques bruts)
private-tmp                 # /tmp isolé par namespace
private-bin firefox,sh,bash # seuls ces binaires visibles dans PATH
private-etc passwd,group,resolv.conf,hosts
whitelist ${HOME}/Téléchargements
whitelist ${HOME}/.mozilla
# tout le reste de ${HOME} est masqué (blacklist implicite via whitelist)

# --- Réseau ---
netfilter                   # applique les règles iptables du profil
# net none                  # décommentez pour couper tout réseau

# --- Noyau / syscalls ---
seccomp                     # filtre seccomp par défaut
caps.drop all               # retire toutes les capabilities
noroot                      # même root dedans = nobody dehors
nonewprivs                  # NoNewPrivileges (bloque setuid/sudo)
protocol unix,inet,inet6    # familles de sockets autorisées

# --- Divers ---
nodbus                      # coupe D-Bus (ou dbus-user none)
notv                        # pas d'accès TV/DVB
nosound                     # pas d'audio (à adapter)
name navigateur-durci
hostname sandbox-navigateur
```

Ordre de priorité : la ligne de commande écrase le profil utilisateur (`~/.config/firejail/`), qui écrase le profil système (`/etc/firejail/`).

## 27. Créer un profil : navigateur durci pas à pas

Objectif : un profil Firefox pour la navigation à risque (liens reçus par mail, sites douteux).

```bash
mkdir -p ~/.config/firejail
cp /etc/firejail/firefox.profile ~/.config/firejail/firefox.profile
```

Ajoutez à la fin de `~/.config/firejail/firefox.profile` :

```ini
# --- Durcissement navigation à risque ---
private-cache               # cache isolé, jeté à la fermeture
#whitelist ${HOME}/Téléchargements
# -> commentez whitelist pour interdire tout téléchargement persistant
read-only ${HOME}
tmpfs ${HOME}/.cache
noexec /tmp
noexec ${HOME}
seccomp.block-secondary     # bloque les ABI 32 bits (réduit la surface)
restrict-namespaces         # interdit de créer de nouveaux namespaces
memory-deny-write-execute   # bloque les pages W+X (anti-exploit JIT limité)
```

Testez :

```bash
firejail --profile=~/.config/firejail/firefox.profile firefox
# Dans Firefox : about:support -> vérifier le fonctionnement
# Tenter un téléchargement -> doit échouer ou aller en RAM
```

**Limite connue :** `memory-deny-write-execute` peut casser le JIT JavaScript de Firefox sur certains sites (baisse de perf ou plantage d'onglet). Si c'est gênant, retirez la ligne pour la navigation courante et gardez-la pour le profil "à risque".

## 28. Créer un profil : lecteur PDF pour documents suspects

Scénario : ouvrir un PDF d'origine inconnue. Le lecteur n'a besoin ni de réseau, ni de vos fichiers.

```ini
# ~/.config/firejail/pdf-suspect.profile
# Usage : firejail --profile=pdf-suspect.profile evince /tmp/suspect.pdf

include disable-common.inc
include disable-devel.inc
include disable-programs.inc

caps.drop all
net none                    # AUCUN réseau : un PDF n'en a pas besoin
nonewprivs
noroot
seccomp
protocol unix               # sockets unix locales uniquement (X11/Wayland)
private-dev
private-tmp
private-bin evince,sh
private-etc passwd,group,fonts
dbus-user none
dbus-system none

# Le PDF est copié dans un répertoire dédié, seul visible
whitelist /tmp/pdfs-suspects
read-only /tmp/pdfs-suspects
```

Procédure d'ouverture d'un PDF suspect :

```bash
mkdir -p /tmp/pdfs-suspects
cp ~/Téléchargements/suspect.pdf /tmp/pdfs-suspects/
firejail --profile=~/.config/firejail/pdf-suspect.profile \
  evince /tmp/pdfs-suspects/suspect.pdf
# Après lecture : rm /tmp/pdfs-suspects/suspect.pdf
```

Même si le PDF exploite Evince, l'attaquant n'a ni réseau pour exfiltrer, ni accès à vos fichiers, ni `ptrace`, ni `mount`.

## 29. Créer un profil : Thunderbird

```ini
# ~/.config/firejail/thunderbird-durci.profile
include thunderbird.profile

# Durcissement supplémentaire
seccomp.block-secondary
restrict-namespaces
# Thunderbird a besoin du réseau (IMAP/SMTP) : on ne coupe pas net,
# mais on restreint les protocoles
protocol unix,inet,inet6,netlink
private-cache
```

Pièces jointes : configurez Thunderbird pour enregistrer les pièces jointes dans `/tmp/pdfs-suspects` (ou `~/Téléchargements`), puis ouvrez-les avec le profil PDF ci-dessus. Ne jamais ouvrir une pièce jointe directement depuis le client mail sans sandbox.

## 30. Application non fiable : patron générique

Pour n'importe quel binaire douteux, ce patron couvre 90 % des besoins :

```bash
firejail --noprofile \
  --private \                    # home temporaire vide
  --net=none \                   # pas de réseau (retirez si nécessaire)
  --seccomp \                    # filtre appels système
  --caps.drop=all \              # aucune capability
  --noroot \                     # pas de root même factice
  --nonewprivs \                 # bloque setuid
  --private-dev \                # /dev minimal
  --private-tmp \                # /tmp isolé
  --read-only=/ \                # système en lecture seule...
  --tmpfs=/tmp --tmpfs=/var/tmp \ # ...sauf tmpfs explicites
  --hostname=sandbox \
  /chemin/vers/binaire --args
```

Variante avec accès réseau contrôlé mais sans accès à votre home :

```bash
firejail --noprofile \
  --private=/tmp/faux-home \
  --dns=9.9.9.9 \
  --seccomp --caps.drop=all --noroot \
  /chemin/vers/binaire
```

## 31. Firejail et X11 : le point faible à connaître

Sous X11, **tout client X peut espionner le clavier et capturer l'écran** (keylogging, screenshots). Firejail propose :

```ini
x11 xephyr     # serveur X imbriqué : l'appli ne voit que sa fenêtre
# ou
x11 xpra       # similaire, avec persistance
# ou (radical)
x11 none       # pas d'accès X du tout (applis console uniquement)
```

```bash
sudo apt install -y xephyr
firejail --x11=xephyr firefox
```

Sous Wayland, l'isolation est native : chaque application ne voit que ses propres fenêtres. **Si vous utilisez Firejail pour des applis graphiques sensibles, préférez une session Wayland.**

## 32. Supervision : voir ce que fait un sandbox

```bash
# Processus et namespaces
firejail --list
firejail --tree

# Événements d'audit du sandbox (tentatives bloquées)
firejail --audit firefox
# Exemple de sortie : syscall bloqué, accès fichier refusé

# Journal en temps réel d'un sandbox nommé
firejail --name=test --noprofile bash
# dans un autre terminal :
firejail --join=test
```

Activer la journalisation des violations seccomp :

```bash
# Les processus tués par seccomp apparaissent dans le journal noyau
sudo dmesg | grep -i seccomp
sudo journalctl -k | grep -i "audit.*seccomp"
```

## 33. Dépannage Firejail : cas concrets

**Cas 1 — L'application ne démarre pas : écran noir / segfault immédiat**
Cause fréquente : `private-bin` oublie un binaire nécessaire, ou `seccomp` bloque un appel légitime.
Diagnostic :

```bash
# Lancer sans seccomp pour isoler la cause
firejail --profile=votre.profile --ignore=seccomp programme
# Si ça marche : le filtre seccomp est en cause -> ajoutez
# seccomp.keep=mmap,mmap2,openat,... (approche par allowlist ciblée)
# Lancer avec strace dans le sandbox pour voir l'appel bloqué
firejail --noprofile --seccomp strace -f -e trace=%network programme
```

**Cas 2 — "cannot create /run/firejail/..." ou erreur SUID**
Le binaire firejail doit être SUID ou les user namespaces activés :

```bash
ls -l /usr/bin/firejail
sysctl kernel.unprivileged_userns_clone   # doit être 1
```

**Cas 3 — Pas de son dans le navigateur sandboxé**
`nosound` ou `private-dev` bloque l'accès à PulseAudio/PipeWire. Retirez `nosound` et ajoutez le socket :

```ini
# pour PipeWire/PulseAudio
whitelist ${RUNUSER}/pulse/native
whitelist ${RUNUSER}/pipewire-0
```

**Cas 4 — Téléchargements invisibles**
Avec `private=` ou `whitelist`, le répertoire `~/Téléchargements` de l'hôte n'est pas le même que dans le sandbox. Utilisez `--whitelist=~/Téléchargements` explicite ou copiez les fichiers après fermeture.

## 34. Erreurs classiques avec Firejail

1. **Lancer en root** : `sudo firejail firefox` casse l'isolation (le sandbox hérite des privilèges). Toujours lancer en utilisateur normal.
2. **Empiler les profils sans tester** : un profil trop strict casse l'appli, l'utilisateur le désactive entièrement. Durcissez progressivement.
3. **Oublier `firecfg` après mise à jour** : relancez `sudo firecfg` après chaque mise à jour de firejail pour régénérer les liens.
4. **Croire que `--private` protège les données** : `--private` donne un home *vide*, il ne chiffre rien. Les fichiers restent accessibles hors sandbox.
5. **Négliger X11** : sans `x11 xephyr` (ou Wayland), un keylogger sandboxé lit quand même le clavier sous X11.

## 35. Checklist Firejail pour un poste

- [ ] `firejail` et `firejail-profiles` installés, version ≥ 0.9.70
- [ ] `kernel.unprivileged_userns_clone = 1`
- [ ] `sudo firecfg` exécuté (lanceurs intégrés)
- [ ] Profils personnalisés pour : navigateur, lecteur PDF, client mail
- [ ] Procédure "PDF suspect" connue et testée
- [ ] Session Wayland préférée (sinon `x11 xephyr` pour les applis à risque)
- [ ] Test mensuel : `firejail --list` et revue des profils après mises à jour

---

# PARTIE IV — SANDBOXING SYSTEMD : CONFINER LES SERVICES

## 36. Pourquoi sandboxer avec systemd ?

Sur un serveur, la majorité des processus sont des services systemd. Plutôt qu'ajouter un outil tiers, systemd intègre nativement : namespaces, seccomp, capabilities, cgroups, montages read-only. Avantage décisif : **zéro dépendance supplémentaire**, configuration déclarative, audit via `systemd-analyze security`.

```bash
# Vérifier la version (>= 247 pour un bon support, >= 252 idéal)
systemctl --version

# Voir le niveau de confinement actuel d'un service
systemd-analyze security nginx.service
```

## 37. ProtectSystem : verrouiller le système de fichiers

| Valeur | Effet |
|---|---|
| `no` (défaut) | Aucune protection |
| `yes` / `true` | `/usr` et `/boot` en lecture seule, `/etc` en lecture seule |
| `full` | Comme `yes` + `/var`, `/srv`, `/home` en lecture seule |
| `strict` | Tout le filesystem en lecture seule + `/proc`, `/sys`, `/dev` masqués |

```ini
[Service]
# Service web typique : binaires et config en lecture seule
ProtectSystem=strict
ReadWritePaths=/var/lib/monapp /var/log/monapp
# /tmp et /var/tmp privés (namespaces mnt)
PrivateTmp=true
```

`ReadWritePaths=` perce des exceptions dans le read-only. `ReadOnlyPaths=` fait l'inverse (rend un chemin read-only avec `ProtectSystem=no`).

## 38. ProtectHome : protéger les données utilisateurs

| Valeur | Effet |
|---|---|
| `no` (défaut) | Accès normal |
| `read-only` | `/home`, `/root`, `/run/user` en lecture seule |
| `tmpfs` | Masqués par des tmpfs vides (invisibles) |
| `yes` | Comme `tmpfs` (alias historique) |

```ini
[Service]
# Un service n'a JAMAIS besoin de lire /home
ProtectHome=tmpfs
# Exception ciblée si nécessaire :
# ReadWritePaths=/home/monapp/data
```

**Réflexe :** sauf besoin explicite, mettez `ProtectHome=tmpfs` sur tous vos services. C'est la directive au meilleur ratio protection/effort.

## 39. PrivateTmp, PrivateDevices, PrivateNetwork

```ini
[Service]
# /tmp et /var/tmp privés au service (invisibles des autres processus)
PrivateTmp=true

# /dev minimal : null, zero, random, urandom, full, tty, ptmx...
# Bloque l'accès aux disques (/dev/sda), au hardware brut
PrivateDevices=true

# Namespace réseau vide (que lo). Pour un agent qui n'a pas besoin du réseau.
PrivateNetwork=true

# Combinaison typique pour un worker de traitement local
PrivateTmp=true
PrivateDevices=true
PrivateNetwork=true
```

Attention : `PrivateNetwork=true` casse la résolution DNS et toute connexion. Pour un service qui doit sortir mais pas écouter en local, préférez `RestrictAddressFamilies` (§43).

## 40. NoNewPrivileges : interdire l'élévation

```ini
[Service]
# Le processus et ses enfants ne pourront JAMAIS gagner de privilèges
# (setuid, setgid, capabilities fichier, sudo... deviennent inopérants)
NoNewPrivileges=true
```

C'est la directive la plus importante après `ProtectSystem`/`ProtectHome`. Elle neutralise toute une classe d'exploitations (binaire setuid vulnérable, `sudo` mal configuré invoqué depuis le service). **Effet de bord :** les programmes qui ont *légitimement* besoin de setuid (ex. `ping` avec capabilities fichier) cesseront de fonctionner — prévoyez des capabilities explicites (§46).

## 41. ProtectKernelTunables, ProtectKernelModules, ProtectControlGroups

```ini
[Service]
# /proc/sys, /sys en lecture seule : impossible de modifier les paramètres noyau
ProtectKernelTunables=true

# Interdit le chargement/déchargement de modules noyau
ProtectKernelModules=true

# /sys/fs/cgroup en lecture seule : le service ne peut pas modifier ses limites
# ni s'échapper de son cgroup
ProtectControlGroups=true

# Masque /proc/kallsyms, /proc/kcore... (adresses noyau utiles aux exploits)
ProtectKernelLogs=true
```

Ces quatre directives sont sans risque pour 95 % des services applicatifs (web, bases de données, workers). Mettez-les par défaut.

## 42. SystemCallFilter : seccomp pour les services

```ini
[Service]
# Allowlist : le service ne peut appeler que les syscalls du groupe @system-service
SystemCallFilter=@system-service
# Bloque aussi les architectures 32 bits (x32, i386) : réduit la surface
SystemCallArchitectures=native
# Action en cas de violation : tuer le processus (défaut) ou errno
SystemCallErrorNumber=EPERM
```

Groupes prédéfinis utiles (`man systemd.exec` pour la liste complète) :

| Groupe | Contenu indicatif |
|---|---|
| `@system-service` | Base pour un service système standard |
| `@basic-io` | Lecture/écriture fichiers, ioctl de base |
| `@network-io` | Sockets, connect, send/recv |
| `@file-system` | open, stat, mkdir... |
| `@privileged` | mount, sethostname... (à éviter) |
| `@resources` | nice, setrlimit... |
| `@debug` | ptrace, process_vm_readv (à éviter) |

Approche pragmatique : commencez par `@system-service`, testez, et resserrez avec `~@mount @debug` (syntaxe d'exclusion) si besoin :

```ini
SystemCallFilter=@system-service @network-io
SystemCallFilter=~@privileged ~@debug ~@resources
```

**Piège :** un filtre trop strict tue le service au démarrage (souvent pendant l'init, ex. un appel `name_to_handle_at` inhabituel). Diagnostic : `journalctl -u monservice` montre `code=killed, status=31/SYS` (31 = SIGSYS, le signal seccomp).

## 43. RestrictAddressFamilies : contrôler le réseau au niveau socket

```ini
[Service]
# Le service ne peut créer que des sockets TCP/UDP/Unix
RestrictAddressFamilies=AF_UNIX AF_INET AF_INET6
# Version radicale : aucun socket du tout
# RestrictAddressFamilies=no
```

Exemples par profil :

```ini
# Serveur web : TCP + sockets unix (php-fpm, etc.)
RestrictAddressFamilies=AF_UNIX AF_INET AF_INET6

# Worker local sans réseau
RestrictAddressFamilies=AF_UNIX

# Agent SNMP qui a besoin de raw sockets : ajoutez AF_PACKET avec prudence
# RestrictAddressFamilies=AF_UNIX AF_INET AF_PACKET
```

Combiné à `PrivateNetwork=true`, c'est redondant ; combiné à un réseau normal, c'est un filet anti-exfiltration par canaux exotiques (ex. `AF_PACKET` pour sniffer).

## 44. Limiter les ressources : cgroups via systemd

```ini
[Service]
# Mémoire : le service est tué (OOM) au-delà de 1 Go
MemoryMax=1G
# Variante souple : pression avant kill
# MemoryHigh=800M

# CPU : 50% d'un cœur max
CPUQuota=50%

# Nombre de tâches (processus+threads) : anti fork-bomb
TasksMax=100

# I/O disque : 10 Mo/s en lecture, 5 Mo/s en écriture
IOReadBandwidthMax=/dev/sda 10M
IOWriteBandwidthMax=/dev/sda 5M
```

Vérification :

```bash
systemctl show monservice.service -p MemoryMax,CPUQuota,TasksMax
systemd-cgtop
```

## 45. RestrictNamespaces, RestrictRealtime, MemoryDenyWriteExecute

```ini
[Service]
# Interdit au service de créer des namespaces (anti-évasion vers user ns)
RestrictNamespaces=yes
# Variante fine : autorise seulement user et mnt
# RestrictNamespaces=user mnt

# Interdit le temps réel (anti DoS CPU)
RestrictRealtime=true

# Interdit les pages mémoire à la fois inscriptibles et exécutables (W^X)
# Bloque une partie des exploits (shellcode), peut casser les JIT (Java, Node)
MemoryDenyWriteExecute=true

# Interdit les namespaces user spécifiquement (durcit encore)
RestrictNamespaces=~user
```

## 46. Capabilities : le minimum vital

```ini
[Service]
# Le service ne garde AUCUNE capability...
CapabilityBoundingSet=
# ... sauf celles listées ici (exemple : binder un port < 1024)
AmbientCapabilities=CAP_NET_BIND_SERVICE
# Alternative moderne au setuid root pour les ports privilégiés
```

Tableau de décision :

| Besoin | Directive |
|---|---|
| Écouter sur le port 80/443 | `AmbientCapabilities=CAP_NET_BIND_SERVICE` (+ `authbind` en alternative) |
| Envoyer des pings | `CAP_NET_RAW` |
| Changer l'heure | `CAP_SYS_TIME` (rare, préférez chrony) |
| Rien de spécial | `CapabilityBoundingSet=` vide |

## 47. Exemple complet : unité durcie pour une application web

```ini
# /etc/systemd/system/monapp.service
[Unit]
Description=Mon application web interne
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
User=monapp
Group=monapp
WorkingDirectory=/opt/monapp
ExecStart=/opt/monapp/bin/monapp --config /etc/monapp/config.yaml
Restart=on-failure
RestartSec=5

# --- Filesystem ---
ProtectSystem=strict
ProtectHome=tmpfs
PrivateTmp=true
PrivateDevices=true
ReadWritePaths=/var/lib/monapp /var/log/monapp
ProtectKernelTunables=true
ProtectKernelModules=true
ProtectControlGroups=true

# --- Privilèges ---
NoNewPrivileges=true
CapabilityBoundingSet=
RestrictNamespaces=yes

# --- Syscalls ---
SystemCallFilter=@system-service
SystemCallFilter=~@privileged ~@debug
SystemCallArchitectures=native

# --- Réseau ---
RestrictAddressFamilies=AF_UNIX AF_INET AF_INET6

# --- Ressources ---
MemoryMax=1G
CPUQuota=100%
TasksMax=200

# --- Divers ---
RestrictRealtime=true
LockPersonality=true
UMask=0027

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now monapp.service
systemd-analyze security monapp.service   # viser <= 3.0 (EXPOSED)
```

## 48. systemd-analyze security : lire le score

```bash
systemd-analyze security monapp.service
```

Sortie typique :

```
  NAME                                        DESCRIPTION
✗ PrivateNetwork=                             Service has access to the host's network
✗ User=/DynamicUser=                          Service runs as root user
✗ CapabilityBoundingSet=~CAP_SYS_ADMIN        Service may have elevated capabilities
✓ NoNewPrivileges=                           Service cannot gain new privileges
...
→ Overall exposure level for monapp.service: 7.5 UNSAFE 🛑
```

Échelle :

| Score | Niveau | Interprétation |
|---|---|---|
| 0 – 1.5 | `SAFE` | Très bien confiné |
| 1.6 – 3.0 | `EXPOSED` | Correct, revoir les ✗ restants |
| 3.1 – 8.0 | `UNSAFE` | Confinement insuffisant |
| 8.1 – 10 | `DANGEROUS` | Aucune protection (défaut) |

**Méthode :** partez du score, corrigez les ✗ un par un en testant le service à chaque étape. Un service qui ne démarre plus après un changement = directive trop stricte → assouplissez *cette* directive uniquement.

```bash
# Comparer avant/après
systemd-analyze security nginx.service > /tmp/avant.txt
# ... modifiez l'unité ...
systemd-analyze security nginx.service > /tmp/apres.txt
diff /tmp/avant.txt /tmp/apres.txt
```

## 49. Cas concret : durcir nginx pas à pas

État initial : `nginx.service` fourni par Debian, score ~9.6 DANGEROUS (tourne en root pour le master).

```bash
# 1. Créer un drop-in (ne jamais modifier le fichier du paquet)
sudo systemctl edit nginx.service
```

```ini
# /etc/systemd/system/nginx.service.d/durcissement.conf
[Service]
# nginx master a besoin de root pour binder le 443 et lire les certs :
# on garde User=root mais on verrouille le reste
ProtectSystem=strict
ReadWritePaths=/var/lib/nginx /var/log/nginx /run
ProtectHome=tmpfs
PrivateTmp=true
PrivateDevices=true
NoNewPrivileges=true
ProtectKernelTunables=true
ProtectKernelModules=true
ProtectControlGroups=true
RestrictAddressFamilies=AF_UNIX AF_INET AF_INET6 AF_NETLINK
SystemCallFilter=@system-service
SystemCallFilter=~@privileged
SystemCallArchitectures=native
RestrictNamespaces=yes
RestrictRealtime=true
MemoryDenyWriteExecute=true
```

```bash
sudo systemctl daemon-reload
sudo systemctl restart nginx.service
systemd-analyze security nginx.service
# Objectif : passer de 9.6 DANGEROUS à ~2.5 EXPOSED
curl -sI https://localhost | head -3   # vérifier que ça sert toujours
```

Note : `AF_NETLINK` est nécessaire à nginx pour la résolution et les logs. Si `MemoryDenyWriteExecute` casse un module tiers (ex. ModSecurity avec JIT PCRE), retirez-le.

## 50. Cas concret : service utilisateur (user service) sandboxé

Les services `--user` bénéficient des mêmes directives :

```ini
# ~/.config/systemd/user/syncthing-perso.service
[Unit]
Description=Syncthing personnel confiné

[Service]
ExecStart=/usr/bin/syncthing -no-browser
Restart=on-failure
ProtectSystem=strict
ProtectHome=read-only
ReadWritePaths=%h/Sync %h/.config/syncthing
PrivateTmp=true
NoNewPrivileges=true
SystemCallFilter=@system-service
RestrictAddressFamilies=AF_UNIX AF_INET AF_INET6
MemoryMax=512M
TasksMax=100

[Install]
WantedBy=default.target
```

```bash
systemctl --user daemon-reload
systemctl --user enable --now syncthing-perso.service
systemd-analyze security --user syncthing-perso.service
```

---

# PARTIE V — APPARMOR : CONFINEMENT PAR PROFIL (DEBIAN/UBUNTU)

## 51. AppArmor : principe

AppArmor est un système de **contrôle d'accès obligatoire (MAC)** : chaque programme confiné suit un profil qui liste ce qu'il a le droit de faire (fichiers, réseau, capabilities). Tout le reste est refusé — même si les permissions Unix l'autoriseraient. C'est le MAC par défaut sur Debian/Ubuntu (contrairement à SELinux, défaut sur RHEL).

```bash
# Vérifier qu'AppArmor est actif
sudo aa-status
# apparmor module is loaded.
# 45 profiles are loaded.

# Profils chargés et leur mode
sudo aa-status | head -30
```

Deux modes :

| Mode | Comportement |
|---|---|
| `enforce` | Les violations sont **bloquées** et journalisées |
| `complain` | Les violations sont **journalisées uniquement** (apprentissage) |

## 52. Lire un profil AppArmor

Les profils sont dans `/etc/apparmor.d/` :

```bash
ls /etc/apparmor.d/
cat /etc/apparmor.d/usr.sbin.cupsd | head -60
```

Anatomie :

```apparmor
# /etc/apparmor.d/usr.bin.monapp (exemple simplifié)
#include <tunables/global>

/usr/bin/monapp {
  #include <abstractions/base>      # règles de base (libc, /proc, etc.)
  #include <abstractions/nameservice> # DNS : /etc/resolv.conf, nss

  capability net_bind_service,       # capabilities autorisées
  capability sys_nice,

  network inet tcp,                  # réseau autorisé
  network inet6 tcp,

  /usr/bin/monapp mr,                # m = mmap exécutable, r = lecture
  /etc/monapp/** r,                  # lecture config
  /var/lib/monapp/** rwk,            # rwx + k (verrouillage)
  /var/log/monapp/** w,              # écriture logs
  /tmp/** rw,

  deny /home/** r,                   # refus explicite (loggé)
}
```

Syntaxe des droits fichiers : `r` (lecture), `w` (écriture), `x` (exécution), `m` (mmap PROT_EXEC), `k` (lock), `l` (lien). Les règles s'accumulent (union).

## 53. Passer un profil en complain / enforce

```bash
# Passer en mode apprentissage (ne bloque plus, loggue)
sudo aa-complain /usr/bin/monapp

# Revenir en enforce
sudo aa-enforce /usr/bin/monapp

# Désactiver totalement un profil
sudo aa-disable /usr/bin/monapp

# Recharger tous les profils
sudo systemctl reload apparmor
```

Cycle de travail recommandé pour un nouveau profil : `complain` → exercer le programme normalement → générer le profil → `enforce` → surveiller les logs.

## 54. aa-genprof : générer un profil par apprentissage

```bash
# 1. Lancer l'assistant (terminal 1)
sudo aa-genprof /usr/bin/monapp

# 2. Dans un autre terminal, utilisez le programme normalement :
#    ouvrez des fichiers, cliquez partout, testez toutes les fonctions
monapp

# 3. Revenez au terminal 1, appuyez sur "S" (Scan) :
#    aa-genprof propose chaque accès détecté : (A)llow, (D)eny, (G)lob...
# 4. "F" (Finish) quand vous avez tout couvert, puis "S" (Save)
```

Bonnes pratiques pendant l'apprentissage :

- Exercez **tous** les chemins : ouverture, sauvegarde, impression, préférences, mise à jour.
- Faites-le sur une machine de test, pas en production.
- Relisez le profil généré avant de l'enforcer : `aa-genprof` a tendance à autoriser large (`/tmp/** rw`).

## 55. aa-logprof : affiner depuis les logs

Quand un profil `enforce` bloque quelque chose de légitime, les refus sont dans les logs :

```bash
# Voir les refus AppArmor
sudo dmesg | grep -i apparmor | grep DENIED
sudo journalctl -k | grep "apparmor.*DENIED"
# Exemple :
# audit(1234.567:89): apparmor="DENIED" operation="open"
#   profile="/usr/bin/monapp" name="/etc/monapp/extra.conf" comm="monapp"
```

```bash
# Assistant interactif : propose d'ajouter les accès refusés au profil
sudo aa-logprof
```

## 56. Exemple : profil AppArmor pour un lecteur PDF

```apparmor
# /etc/apparmor.d/usr.bin.evince-sandbox
#include <tunables/global>

/usr/bin/evince {
  #include <abstractions/base>
  #include <abstractions/fonts>
  #include <abstractions/gnome>

  capability dac_override,

  network none,          # aucun réseau (redondant avec Firejail, défense en profondeur)

  /usr/bin/evince mr,
  /usr/share/evince/** r,
  /usr/share/poppler/** r,
  /etc/fonts/** r,
  /usr/share/fonts/** r,

  # Le répertoire des PDF suspects : lecture seule
  /tmp/pdfs-suspects/** r,

  # Cache et config GNOME minimaux
  owner @{HOME}/.cache/evince/** rw,
  owner @{HOME}/.config/evince/** rw,

  # Tout le reste du home : refusé
  deny owner @{HOME}/** rwklx,
}
```

```bash
sudo apparmor_parser -r /etc/apparmor.d/usr.bin.evince-sandbox
sudo aa-enforce /usr/bin/evince
```

## 57. AppArmor et systemd : combiner les deux

Les directives systemd (`ProtectSystem`, etc.) et AppArmor se cumulent : si l'un bloque, l'accès est refusé. Pour un service critique, utilisez les deux :

```ini
[Service]
# systemd confine les namespaces/syscalls...
ProtectSystem=strict
ProtectHome=tmpfs
NoNewPrivileges=true
SystemCallFilter=@system-service
# ...et AppArmor confine les chemins fins
AppArmorProfile=monapp-confine
```

```bash
# Vérifier que le profil AppArmor est bien appliqué au service
sudo aa-status | grep monapp
cat /proc/$(pidof monapp)/attr/current
# -> monapp-confine (enforce)
```

## 58. Dépannage AppArmor : cas concrets

**Cas 1 — Le programme ne démarre plus après `aa-enforce`, aucun message clair**
Regardez les DENIED :

```bash
sudo journalctl -k --since "5 min ago" | grep DENIED
```

Souvent : une bibliothèque dans un chemin non prévu (`/opt/...`), ou une abstraction manquante (`#include <abstractions/X>`).

**Cas 2 — `aa-genprof` ne détecte rien**
Le programme tourne peut-être déjà sous un autre profil, ou les logs audit sont désactivés. Vérifiez `sudo aa-status` et relancez le programme après `aa-complain`.

**Cas 3 — Conflit avec un profil existant du paquet**
Debian livre des profils pour cupsd, dhclient, etc. Si vous écrivez le vôtre pour le même binaire, le paquet peut l'écraser à la mise à jour. Placez vos surcharges dans `/etc/apparmor.d/local/usr.bin.monapp` (inclus via `#include <local/...>` si le profil le prévoit).

**Cas 4 — Performances**
AppArmor a un coût négligeable (< 1 %). Si un doute : `sudo sysctl -w kernel.apparmor_restrict_unprivileged_userns=1` n'a rien à voir avec les perfs, c'est un durcissement.

---

# PARTIE VI — SELINUX : TOUR D'HORIZON (CULTURE GÉNÉRALE)

## 59. SELinux : notions essentielles

SELinux est le MAC historique (NSA, défaut sur RHEL/Fedora). Plus puissant et plus complexe qu'AppArmor : il étiquette **chaque objet** (fichier, processus, port) avec un **contexte**, et n'autorise que les interactions prévues par la politique.

```bash
# Sur un système RHEL/Fedora :
getenforce        # Enforcing | Permissive | Disabled
sestatus
```

Modes :

| Mode | Effet |
|---|---|
| Enforcing | Bloque + journalise |
| Permissive | Journalise seulement (apprentissage) |
| Disabled | Désactivé (nécessite reboot) |

## 60. Contextes SELinux : lire les étiquettes

Format : `utilisateur:rôle:type:niveau` — ex. `system_u:system_r:httpd_t:s0`.

```bash
# Voir le contexte des fichiers
ls -Z /var/www/html/
# system_u:object_r:httpd_sys_content_t:s0 index.html

# Voir le contexte des processus
ps -eZ | grep httpd
# system_u:system_r:httpd_t:s0 1234 ? /usr/sbin/httpd

# Voir le contexte des ports
semanage port -l | grep http
```

Règle d'or : un processus `httpd_t` ne peut lire que les fichiers étiquetés `httpd_sys_content_t`. Un fichier créé ailleurs garde son ancien type → accès refusé même avec `chmod 777`. D'où 90 % des "bugs" SELinux : **mauvais contexte après déplacement de fichiers**.

## 61. SELinux au quotidien : les 5 commandes qui sauvent

```bash
# 1. Le fichier a le mauvais type ? Restaurez le type par défaut du chemin
restorecon -Rv /var/www/html/

# 2. Changer durablement le type d'un chemin personnalisé
semanage fcontext -a -t httpd_sys_content_t "/srv/web(/.*)?"
restorecon -Rv /srv/web

# 3. Activer/désactiver une booléenne (ex. : httpd peut envoyer des mails)
setsebool -P httpd_can_sendmail on
getsebool -a | grep httpd

# 4. Comprendre un refus : lire l'audit
ausearch -m avc -ts recent | audit2why
# audit2why explique : "il manque la booléenne X" ou "contexte incorrect"

# 5. Générer un module de politique pour autoriser un refus légitime
ausearch -m avc -ts recent | audit2allow -M monmodule
semodule -i monmodule.pp
```

**Avertissement :** `audit2allow` génère des autorisations parfois trop larges. Relisez le `.te` avant `semodule -i`. Et ne désactivez jamais SELinux pour "résoudre" un problème : passez en Permissive, corrigez, repassez en Enforcing.

## 62. AppArmor vs SELinux : que choisir ?

| Critère | AppArmor | SELinux |
|---|---|---|
| Défaut sur | Debian, Ubuntu, openSUSE | RHEL, Fedora, CentOS |
| Modèle | Profils par programme (chemins) | Politique globale (types/étiquettes) |
| Courbe d'apprentissage | Modérée | Raide |
| Finesse | Bonne (fichiers, réseau, caps) | Excellente (tout objet) |
| Outils d'aide | aa-genprof, aa-logprof | audit2allow, setroubleshoot |

**Pour Zelef (Debian/Ubuntu) :** investissez sur AppArmor. Connaissez SELinux pour la culture et les environnements RHEL croisés en entreprise.

## 63. Checklist MAC

- [ ] `aa-status` : profils en enforce pour les services exposés (web, mail, DNS, impression)
- [ ] Nouveau binaire sensible → `aa-genprof` en recette avant production
- [ ] `aa-logprof` passé après chaque mise à jour majeure d'une appli confinée
- [ ] Sur RHEL : `getenforce` = Enforcing, `restorecon` connu par l'équipe
- [ ] Les refus MAC remontent dans la supervision (voir §73)

## 64. Erreurs classiques MAC

1. **Désactiver AppArmor/SELinux "pour que ça marche"** : on perd le filet sans corriger la cause. Toujours diagnostiquer le DENIED/AVC.
2. **`audit2allow` aveugle** : autorise parfois `dontaudit` massifs. Relire le `.te`.
3. **Profils AppArmor écrasés par les mises à jour** : personnalisations dans `/etc/apparmor.d/local/`, jamais dans le fichier du paquet.
4. **Oublier `restorecon` après `mv`** : `mv` conserve le contexte d'origine, `cp` applique celui de la destination. En cas de doute : `restorecon`.

---

# PARTIE VII — DOCKER COMME SANDBOX : LIMITES ET DURCISSEMENT

## 65. Docker isole, mais n'est pas une VM : les limites

Un conteneur partage le **noyau de l'hôte**. Conséquences :

| Menace | Conteneur Docker par défaut | VM KVM |
|---|---|---|
| Faille applicative | Contenue (namespaces) | Contenue |
| Faille noyau | **Compromet l'hôte** | Contenue (noyau séparé) |
| `--privileged` | Équivaut à root sur l'hôte | Sans objet |
| Montage `/var/run/docker.sock` | Contrôle total de l'hôte | Sans objet |
| Évasion par capabilities | Possible si mal configuré | Non |

**Règle :** ne faites jamais tourner du code *activement hostile* (malware à analyser) dans Docker sur un hôte de production. Pour l'analyse de malware : VM KVM isolée, snapshots, pas de réseau ou réseau simulé (INetSim).

## 66. Durcir un conteneur : les options indispensables

```bash
docker run -d --name monapp-durci \
  --read-only \                    # filesystem racine en lecture seule
  --tmpfs /tmp --tmpfs /run \      # tmpfs inscriptibles explicites
  --cap-drop=ALL \                 # retire TOUTES les capabilities...
  --cap-add=NET_BIND_SERVICE \     # ...sauf celle(s) nécessaire(s)
  --security-opt no-new-privileges \ # bloque setuid/setgid
  --pids-limit 100 \               # anti fork-bomb (cgroup pids)
  --memory 512m --memory-swap 512m \ # limite RAM (swap inclus)
  --cpus 1.0 \                     # limite CPU
  --network mon-reseau-isole \     # réseau dédié, pas le bridge par défaut
  --user 10000:10000 \            # ne PAS tourner en root dans le conteneur
  --restart on-failure:3 \
  monimage:1.2.3
```

Checklist de revue d'un `docker run` / compose :

- [ ] Pas de `--privileged`
- [ ] Pas de montage de `/var/run/docker.sock` (sauf cas justifié et audité)
- [ ] `--read-only` + tmpfs ciblés
- [ ] `--cap-drop=ALL` + `--cap-add` minimal
- [ ] `--user` non-root (ou `USER` dans le Dockerfile)
- [ ] Limites mémoire/CPU/pids
- [ ] Image épinglée par digest, pas `:latest`
- [ ] Réseau dédié, ports publiés minimaux

## 67. Seccomp et AppArmor sous Docker

Docker applique par défaut :

1. **Un profil seccomp** (`/usr/share/docker/seccomp/default.json`) qui bloque ~44 syscalls : `mount`, `ptrace`, `reboot`, `swapon`, `kexec_load`, `open_by_handle_at`, `init_module`, etc.
2. **Un profil AppArmor** `docker-default` (sur Ubuntu/Debian si AppArmor actif).

```bash
# Voir le profil AppArmor d'un conteneur
docker inspect monapp-durci --format '{{.AppArmorProfile}}'
# docker-default

# Désactiver seccomp (À NE FAIRE QU'EN DEBUG)
docker run --security-opt seccomp=unconfined ...

# Utiliser un profil seccomp personnalisé
docker run --security-opt seccomp=/opt/seccomp/monapp.json ...
```

Vérifier depuis l'hôte :

```bash
grep Seccomp /proc/$(docker inspect -f '{{.State.Pid}}' monapp-durci)/status
# Seccomp: 2  -> filtre actif
```

## 68. Dockerfile : construire des images sandbox-friendly

```dockerfile
# 1. Base minimale et épinglée par digest
FROM debian:bookworm-slim@sha256:<digest>

# 2. Un seul RUN pour limiter les couches, nettoyage à la fin
RUN apt-get update && apt-get install -y --no-install-recommends \
      ca-certificates tini \
 && rm -rf /var/lib/apt/lists/*

# 3. Utilisateur non-root DÉDIÉ (uid fixe pour les volumes)
RUN groupadd -r app --gid 10000 && useradd -r -g app --uid 10000 app
USER 10000:10000
WORKDIR /app

# 4. Copie avec bons propriétaires, pas de secrets dans l'image
COPY --chown=10000:10000 ./bin/monapp /app/monapp

# 5. init léger pour réaper les zombies (PID 1)
ENTRYPOINT ["/usr/bin/tini", "--"]
CMD ["/app/monapp"]

# 6. Healthcheck
HEALTHCHECK --interval=30s --timeout=3s CMD ["/app/monapp", "health"]
```

Interdits dans un Dockerfile sérieux :

- `ADD http://...` (utilisez `curl` + vérification de checksum)
- Secrets en `ENV` ou en couche (utilisez BuildKit `--secret`)
- `:latest` en production
- `RUN chmod 777` / `USER root` final

## 69. docker-compose durci : exemple complet

```yaml
# compose.durci.yaml
services:
  web:
    image: "monregistry/monapp:1.2.3@sha256:<digest>"
    read_only: true
    tmpfs:
      - /tmp:noexec,nosuid,size=100m
      - /run:noexec,nosuid,size=50m
    cap_drop:
      - ALL
    cap_add:
      - NET_BIND_SERVICE
    security_opt:
      - no-new-privileges:true
    user: "10000:10000"
    pids_limit: 100
    mem_limit: 512m
    memswap_limit: 512m
    cpus: 1.0
    networks:
      - front
    ports:
      - "8080:8080"   # n'exposez que le nécessaire
    volumes:
      - app-data:/var/lib/monapp:rw
      - ./config.yaml:/etc/monapp/config.yaml:ro
    restart: on-failure:3
    healthcheck:
      test: ["CMD", "/app/monapp", "health"]
      interval: 30s
      timeout: 3s
      retries: 3

networks:
  front:
    driver: bridge
    internal: false   # true si pas d'accès Internet nécessaire

volumes:
  app-data:
```

```bash
docker compose -f compose.durci.yaml up -d
docker compose -f compose.durci.yaml exec web id   # uid=10000
```

## 70. Rootless Docker / Podman : ne plus tourner en root

Le daemon Docker classique tourne en **root** : une faille du daemon = root sur l'hôte. Deux alternatives :

**Docker rootless :**

```bash
# Installation (utilisateur normal, pas de sudo)
dockerd-rootless-setuptool.sh install
systemctl --user enable --now docker

# Vérifier : le daemon tourne avec votre UID
ps aux | grep dockerd | grep -v grep
```

**Podman (recommandé sur serveurs, daemonless) :**

```bash
sudo apt install -y podman
podman run --rm -it --userns=auto debian:bookworm-slim bash
# Chaque conteneur mappe ses UID vers une plage non privilégiée de l'hôte
# (/etc/subuid, /etc/subgid)
```

Avec les user namespaces, "root" dans le conteneur = UID banal sur l'hôte. Même en cas d'évasion du conteneur, l'attaquant n'a aucun privilège.

## 71. Analyser une image avant de l'exécuter

```bash
# Lister les couches et leur taille
docker history monimage:1.2.3

# Scanner les vulnérabilités (Trivy)
trivy image monimage:1.2.3

# Inspecter sans exécuter : extraire le filesystem
docker export $(docker create monimage:1.2.3) | tar -t | less
mkdir /tmp/img && tar -xf <(docker export $(docker create monimage:1.2.3)) -C /tmp/img
grep -r "password\|secret\|BEGIN PRIVATE KEY" /tmp/img/etc /tmp/img/app 2>/dev/null | head

# Vérifier l'utilisateur par défaut
docker inspect monimage:1.2.3 --format '{{.Config.User}}'
# vide = root ! -> prévoyez --user au run
```

## 72. Cas pratique : exécuter un outil non fiable dans Docker

Scénario : un utilitaire de conversion téléchargé hors dépôt, image inconnue.

```bash
#!/bin/bash
# run-douteux.sh — exécute un conteneur "jetable" très confiné
set -euo pipefail
IMAGE="$1"; shift
WORK=$(mktemp -d)
cp -a ./entree/. "$WORK"/

docker run --rm -i \
  --read-only --tmpfs /tmp:noexec,nosuid \
  --cap-drop=ALL --security-opt no-new-privileges \
  --network none \                 # pas de réseau du tout
  --user 10000:10000 \
  --pids-limit 50 --memory 256m --memory-swap 256m \
  --volume "$WORK":/work:rw \
  --workdir /work \
  "$IMAGE" "$@"

echo "--- Résultat dans $WORK/sortie (à inspecter avant usage) ---"
ls -l "$WORK"
```

`--network none` + `--read-only` + `--user` + `--cap-drop=ALL` : l'outil peut au pire corrompre `/work`, qui est un répertoire temporaire.

## 73. Supervision des conteneurs : logs d'audit

```bash
# Logs d'un conteneur
docker logs --since 1h monapp-durci
docker logs -f --tail 100 monapp-durci

# Événements Docker (création, mort, OOM...)
docker events --since 1h --filter event=die --filter event=oom

# Un conteneur tué par seccomp : status 31/SYS dans les events
docker events --filter event=die | grep -i "31"

# Refus AppArmor liés à Docker
sudo journalctl -k | grep "apparmor.*DENIED.*docker"

# Centraliser : driver json-file + logrotate, ou syslog/journald
# /etc/docker/daemon.json
{
  "log-driver": "journald",
  "log-opts": { "tag": "docker/{{.Name}}" }
}
```

Alertes à mettre en place (voir §80) : conteneur en boucle de restart, OOM kills répétés, refus seccomp/AppArmor, image `:latest` déployée.

## 74. Erreurs classiques avec Docker

1. **`--privileged` "pour que ça marche"** : donne accès aux devices de l'hôte, désactive seccomp/AppArmor. Cherchez l'option précise (`--device`, `--cap-add`) au lieu du marteau.
2. **Monter `docker.sock`** dans un conteneur (Portainer mal configuré, CI) : le conteneur peut lancer des conteneurs privilégiés → contrôle de l'hôte.
3. **Tourner en root dans le conteneur** par défaut : combinez `USER` dans le Dockerfile et `--user` au run.
4. **`:latest` en production** : déploiements non reproductibles, mises à jour surprises. Épinglez `image:tag@sha256:digest`.
5. **Secrets dans les variables d'environnement** : visibles via `docker inspect`. Utilisez des fichiers (`_FILE`) ou un gestionnaire de secrets.
6. **Aucune limite de ressources** : un conteneur qui fuit = OOM de l'hôte. Toujours `mem_limit` + `pids_limit`.

---

# PARTIE VIII — BONNES PRATIQUES ET DÉFENSE EN PROFONDEUR

## 75. Principe du moindre privilège : méthode d'application

Pour chaque programme/service, posez-vous ces 6 questions et traduisez en directives :

| Question | Si non | Traduction technique |
|---|---|---|
| A-t-il besoin de tout le filesystem ? | Non | `ProtectSystem=strict` / `--read-only` |
| A-t-il besoin de `/home` ? | Non | `ProtectHome=tmpfs` |
| A-t-il besoin du réseau ? | Non | `PrivateNetwork=true` / `--network none` |
| A-t-il besoin de root/capabilities ? | Non | `NoNewPrivileges=true`, `CapabilityBoundingSet=` vide |
| A-t-il besoin de tous les syscalls ? | Non | `SystemCallFilter=@system-service` |
| A-t-il besoin de ressources illimitées ? | Non | `MemoryMax=`, `TasksMax=`, `CPUQuota=` |

**Méthode itérative :** appliquez tout, testez, et ne relâchez que ce qui casse un usage légitime — en documentant pourquoi dans un commentaire de l'unité.

## 76. Défense en profondeur : empiler les couches

Aucune couche n'est infaillible ; leur combinaison rend l'exploitation exponentiellement plus difficile :

```
┌─────────────────────────────────────────────┐
│  Couche 7 : Supervision (logs, alertes)     │  <- détecte l'échec des autres
├─────────────────────────────────────────────┤
│  Couche 6 : MAC (AppArmor/SELinux)          │  <- confine par profil
├─────────────────────────────────────────────┤
│  Couche 5 : Seccomp (filtre syscalls)       │  <- bloque mount, ptrace...
├─────────────────────────────────────────────┤
│  Couche 4 : Capabilities (retrait)          │  <- pas de CAP_SYS_ADMIN
├─────────────────────────────────────────────┤
│  Couche 3 : Namespaces (pid, net, mnt...)   │  <- vue isolée
├─────────────────────────────────────────────┤
│  Couche 2 : cgroups (mémoire, CPU, pids)    │  <- pas d'épuisement
├─────────────────────────────────────────────┤
│  Couche 1 : Utilisateur dédié, FS read-only │  <- base saine
└─────────────────────────────────────────────┘
        Noyau à jour + mises à jour auto
```

Exemple concret (service web) : `User=monapp` + `ProtectSystem=strict` + `NoNewPrivileges` + `SystemCallFilter` + profil AppArmor + `MemoryMax` + alertes sur les DENIED. Un attaquant doit enchaîner : exploit appli → évasion namespace → contournement seccomp → faille noyau → évasion AppArmor, le tout en silence.

## 77. Durcissement de l'hôte : prérequis au sandboxing

Un sandbox sur un hôte pourri ne sert à rien :

```bash
# 1. Mises à jour automatiques de sécurité
sudo apt install -y unattended-upgrades
sudo dpkg-reconfigure -plow unattended-upgrades

# 2. Noyau : user namespaces OK, mais restreindre si inutile
sysctl kernel.unprivileged_userns_clone   # 1 nécessaire pour Firejail/podman

# 3. ptrace : restreindre aux processus du même utilisateur
# /etc/sysctl.d/10-ptrace.conf
kernel.yama.ptrace_scope = 1

# 4. kexec / hibernation : désactiver si inutile (anti-persistance firmware-like)
# /etc/sysctl.d/10-kexec.conf
kernel.kexec_load_disabled = 1

# 5. dmesg restreint aux root (cache les adresses noyau)
kernel.dmesg_restrict = 1

# 6. Appliquer
sudo sysctl --system
```

```bash
# 7. Audit du système : lynis (rapport)
sudo apt install -y lynis
sudo lynis audit system --quick | grep -E "Warning|Suggestion" | head -20
```

## 78. Mises à jour des sandboxes eux-mêmes

- **Firejail** : `sudo firecfg` après chaque mise à jour (régénère les liens).
- **Profils AppArmor** : `aa-logprof` après mise à jour majeure d'une appli confinée.
- **Images Docker** : rebuild régulier, scan Trivy en CI, politique "pas d'image > 90 jours".
- **Chroots de build** : reconstruction mensuelle depuis `debootstrap` frais.
- **Profils Firejail personnalisés** : revue trimestrielle (un `private-bin` peut manquer un binaire ajouté par une mise à jour).

---

# PARTIE IX — CAS D'USAGE COMMENTÉS

## 79. Cas 1 : ouvrir un PDF suspect reçu par mail (procédure complète)

```bash
#!/bin/bash
# pdf-suspect.sh — procédure d'ouverture sécurisée d'un PDF douteux
set -euo pipefail
PDF="$1"
QUARANTAINE=/tmp/pdfs-suspects
mkdir -p "$QUARANTAINE"

# 1. Copie en quarantaine (on ne touche pas à l'original)
cp -- "$PDF" "$QUARANTAINE/in.pdf"
chmod 400 "$QUARANTAINE/in.pdf"

# 2. Analyse statique rapide (sans l'ouvrir)
pdfinfo "$QUARANTAINE/in.pdf" | head -20        # métadonnées
strings "$QUARANTAINE/in.pdf" | grep -iE "javascript|/JS|/Launch|/EmbeddedFile" | head
# Si /JS ou /Launch présent : méfiance renforcée

# 3. Ouverture sandboxée : pas de réseau, home masqué, seccomp
firejail --profile="$HOME/.config/firejail/pdf-suspect.profile" \
  evince "$QUARANTAINE/in.pdf"

# 4. Nettoyage
shred -u "$QUARANTAINE/in.pdf"
echo "Terminé. Si le document est légitime, demandez une version saine à l'expéditeur."
```

Alternative sans Firejail (pur systemd-run) :

```bash
systemd-run --user --scope -p PrivateTmp=yes -p ProtectSystem=strict \
  -p ProtectHome=tmpfs -p SystemCallFilter=@system-service \
  -p RestrictAddressFamilies=AF_UNIX -p NoNewPrivileges=yes \
  /usr/bin/evince /tmp/pdfs-suspects/in.pdf
```

## 80. Cas 2 : navigateur durci pour sites à risque

Procédure pour l'équipe : un profil Firefox "quarantaine" lancé via un script :

```bash
#!/bin/bash
# navigateur-quarantaine.sh
# Usage : navigateur-quarantaine.sh https://site-douteux.example/
URL="${1:-about:blank}"
firejail \
  --profile="$HOME/.config/firejail/firefox.profile" \
  --private-cache \
  --name=quarantaine \
  firefox --no-remote --new-instance "$URL"
```

Règles d'usage à afficher aux utilisateurs :

1. Jamais de mot de passe / session bancaire dans le navigateur quarantaine.
2. Téléchargements désactivés (ou vers tmpfs jeté à la fermeture).
3. Fermer le sandbox après usage (`firejail --shutdown=quarantaine`).
4. En cas de comportement bizarre (popups, ralentissement) : tuer le sandbox, signaler au support.

## 81. Cas 3 : tester un script d'installation douteux (`curl | bash`)

Le classique `curl https://site/install.sh | sudo bash` est une roulette russe. Procédure :

```bash
# 1. Télécharger SANS exécuter
curl -fsSL https://site.example/install.sh -o /tmp/install.sh
chmod 400 /tmp/install.sh

# 2. Lire le script (au moins en diagonale : curl, wget, chmod +x, /etc, systemd ?)
less /tmp/install.sh
grep -nE "curl|wget|chmod \+x|/etc/(cron|systemd)|useradd|passwd|ssh" /tmp/install.sh

# 3. Exécution à blanc dans un conteneur jetable (jamais sur l'hôte)
docker run --rm -it --read-only --cap-drop=ALL --network none \
  --user 10000:10000 -v /tmp/install.sh:/tmp/install.sh:ro \
  debian:bookworm-slim bash -c 'bash -n /tmp/install.sh && echo "syntaxe OK"'
# bash -n = parse seul, n'exécute rien

# 4. Si le script semble sain ET nécessaire : exécution supervisée
#    dans une VM de test d'abord, avec snapshot avant/après (diff du filesystem)
```

**Règle d'équipe :** aucun `curl|bash` en production sans relecture par un pair et test en VM.

## 82. Cas 4 : pièce jointe Thunderbird → extraction sandboxée

```bash
#!/bin/bash
# pj-sandbox.sh — ouvre une pièce jointe dans le bon sandbox selon son type
set -euo pipefail
PJ="$1"
MIME=$(file -b --mime-type "$PJ")

case "$MIME" in
  application/pdf)
    firejail --profile="$HOME/.config/firejail/pdf-suspect.profile" evince "$PJ" ;;
  application/zip|application/x-tar)
    # Lister d'abord, sans extraire
    firejail --noprofile --net=none --private-tmp 7z l "$PJ" | less ;;
  application/vnd.openxmlformats*|application/msword)
    firejail --profile=/etc/firejail/libreoffice.profile libreoffice "$PJ" ;;
  *)
    echo "Type $MIME : ouverture refusée par politique. Demandez au support." ;;
esac
```

## 83. Cas 5 : compilation d'un projet GitHub inconnu (récapitulatif outillé)

Combine les §17 et §72 : pipeline "clone → build → artefact" jamais exécuté sur l'hôte.

```bash
#!/bin/bash
# build-tiers.sh <url-git> <branche>
set -euo pipefail
URL="$1"; BRANCHE="${2:-main}"
WORK=$(mktemp -d); trap 'rm -rf "$WORK"' EXIT

# 1. Clone SANS exécuter de hooks
git clone --depth 1 --branch "$BRANCHE" --config core.hooksPath=/dev/null "$URL" "$WORK/src"

# 2. Inspection : que va exécuter le build ?
grep -rE "curl|wget|pip install|npm install" "$WORK/src/Makefile" "$WORK/src/configure" 2>/dev/null | head

# 3. Build dans nspawn sans réseau (voir §17)
sudo systemd-nspawn -D /srv/chroot/build --private-users=pick \
  --private-network --bind-ro="$WORK/src:/src" --tmpfs=/build --as-pid2 \
  bash -c 'cp -a /src/. /build/ && cd /build && (./configure && make -j$(nproc))'

# 4. Les binaires restent suspects : testez-les DANS le sandbox avant déploiement
echo "Artefacts dans $WORK (non nettoyé pour inspection)"
```

---

# PARTIE X — SUPERVISION ET LOGS D'AUDIT

## 84. Centraliser les signaux de sandboxing

Sources à collecter :

| Source | Ce qu'elle révèle | Commande / chemin |
|---|---|---|
| Journal noyau (audit) | Refus AppArmor, kills seccomp | `journalctl -k \| grep -E "apparmor.*DENIED\|seccomp"` |
| `auditd` | AVC SELinux, appels surveillés | `/var/log/audit/audit.log` |
| Journal systemd | Services tués (SIGSYS=31) | `journalctl -u <service>` + `status=31/SYS` |
| Docker events | OOM, die, restart loops | `docker events` |
| Firejail | `--audit`, `--tree` | `firejail --list` |

```bash
# Installer auditd pour une traçabilité fine (serveurs sensibles)
sudo apt install -y auditd audispd-plugins
sudo systemctl enable --now auditd

# Surveiller les exécutions de binaires sensibles
sudo auditctl -w /usr/bin/docker -p x -k conteneurs
sudo auditctl -w /etc/apparmor.d/ -p wa -k apparmor-conf

# Rechercher
sudo ausearch -k conteneurs -ts today
```

## 85. Alertes : ce qui doit réveiller l'astreinte

| Alerte | Seuil indicatif | Action |
|---|---|---|
| Refus AppArmor sur service prod | > 5 / heure | `aa-logprof`, vérifier mise à jour |
| Processus tué par seccomp (SIGSYS) | ≥ 1 | Filtre trop strict OU exploitation tentée |
| OOM kill conteneur | > 3 / jour | Fuite mémoire ou `MemoryMax` trop bas |
| Restart loop Docker | > 5 restarts / 10 min | Config cassée, rollback image |
| Nouveau profil AppArmor en complain | rappel 7 j | Repasser en enforce après apprentissage |
| Image Docker > 90 jours | hebdo | Rebuild + scan |

Exemple d'alerte simple avec un timer systemd :

```bash
#!/bin/bash
# /usr/local/bin/check-sandbox-alerts.sh
SEUIL=5
N=$(journalctl -k --since "1 hour ago" | grep -c "apparmor.*DENIED")
[ "$N" -gt "$SEUIL" ] && echo "ALERTE: $N refus AppArmor sur $(hostname) la dernière heure" \
  | mail -s "[sandbox] refus AppArmor" astreinte@example.com
```

## 86. Tableau de bord minimal (sans usine à gaz)

```bash
# État des sandboxes en une page (à mettre en cron hebdo / rapport)
{
  echo "=== $(date) — $(hostname) ==="
  echo "--- AppArmor ---"; sudo aa-status | grep -E "profiles are in|profiles are loaded"
  echo "--- Firejail ---"; firejail --list | wc -l | xargs echo "sandboxes actifs:"
  echo "--- Services systemd les plus exposés ---"
  for s in $(systemctl list-units --type=service --state=running --no-legend | awk '{print $1}' | head -20); do
    score=$(systemd-analyze security "$s" 2>/dev/null | grep "Overall exposure" | awk '{print $(NF-1)}')
    echo "$score $s"
  done | sort -n | tail -5
  echo "--- Docker : conteneurs privileged/root ---"
  docker ps -q | xargs -r docker inspect --format '{{.Name}} user={{.Config.User}} privileged={{.HostConfig.Privileged}}'
} > /var/log/sandbox-report.txt
```

---

# PARTIE XI — DÉPANNAGE GÉNÉRAL

## 87. Matrice de diagnostic : "ça ne marche plus depuis le confinement"

| Symptôme | Cause probable | Vérification | Correctif |
|---|---|---|---|
| Service tué au démarrage, `status=31/SYS` | `SystemCallFilter` trop strict | `journalctl -u X` | Élargir le filtre ou `SystemCallErrorNumber=EPERM` pour diagnostiquer |
| `Permission denied` sur un fichier légitime | `ProtectSystem=strict` / AppArmor | `journalctl -k \| grep DENIED` | `ReadWritePaths=` / règle AppArmor |
| Plus de DNS / réseau | `PrivateNetwork` / `net none` | `ip a` dans le contexte | Retirer ou ajuster `RestrictAddressFamilies` |
| L'appli graphique ne se lance pas | `private-bin` incomplet, D-Bus coupé | Lancer sans profil, comparer | Ajouter binaires/abstractions |
| Écriture impossible dans `/tmp` | `PrivateTmp` + `read-only` | `touch /tmp/test` | Ajouter `--tmpfs /tmp` ciblé |
| Conteneur sans son/vidéo | devices masqués | `ls /dev` dans le conteneur | `--device /dev/snd` ciblé |
| Build qui échoue (dépendances) | Pas de réseau dans le sandbox | logs du build | Autoriser le réseau pendant le build, couper à l'exécution |
| `sudo` ne marche plus dans le service | `NoNewPrivileges=true` | `sudo -n true` → échec | Ne pas utiliser sudo dans le service (revoir l'archi) |

## 88. Procédure de diagnostic en 5 minutes

```bash
# 1. Le service est-il tué par le confinement ?
systemctl status monservice.service
journalctl -u monservice.service --since "10 min ago" | tail -30

# 2. SIGSYS (31) = seccomp. DENIED = AppArmor. OOM = cgroup mémoire.
journalctl -k --since "10 min ago" | grep -iE "seccomp|apparmor.*denied|oom-killer" | tail -20

# 3. Bissection : désactiver UNE directive à la fois via drop-in
sudo systemctl edit monservice.service   # commenter la directive suspecte
sudo systemctl daemon-reload && sudo systemctl restart monservice.service

# 4. Tracer les appels bloqués (dernier recours, en recette)
sudo strace -f -e trace=%network,%process -p $(pidof monservice) 2>&1 | tail -40

# 5. Valider le correctif avec le score
systemd-analyze security monservice.service | grep "Overall exposure"
```

## 89. 10+ erreurs classiques (récapitulatif)

1. **Confondre chroot et sandbox** : chroot seul n'arrête ni root, ni le réseau, ni `ps`. Voir §15.
2. **Lancer Firejail en root** (`sudo firejail ...`) : hérite des privilèges, isolation illusoire.
3. **`--privileged` Docker "temporaire"** qui devient permanent : auditez les `docker run` avec un script.
4. **Désactiver AppArmor/SELinux au lieu de diagnostiquer** : on supprime l'alarme incendie au lieu d'éteindre le feu.
5. **Profils trop stricts, jamais testés** : l'utilisateur contourne tout. Durcir progressivement.
6. **Oublier les user namespaces** (`kernel.unprivileged_userns_clone=0`) : Firejail/podman cassés après un durcissement noyau trop agressif.
7. **`ProtectSystem=strict` sans `ReadWritePaths`** : le service ne peut plus écrire ses logs/pidfiles → crash silencieux.
8. **Aucune limite de ressources** : fork bomb ou fuite mémoire = DoS de l'hôte. Toujours `TasksMax` + `MemoryMax`.
9. **Secrets dans l'image Docker / le profil** : `docker inspect` et `/etc/firejail` sont lisibles. Secrets = fichiers montés ou gestionnaire dédié.
10. **Ne pas mettre à jour les sandboxes** : vieux Firejail, image de 2 ans, chroot non patché = sandbox vulnérable lui-même.
11. **X11 sans `xephyr`** : keylogger sandboxé sous X11 = clavier espionné quand même. Wayland ou `x11 xephyr`.
12. **Faire confiance aux artefacts d'un build sandboxé** : le sandbox protège l'hôte, pas la qualité du binaire. Scanner avant déploiement.

## 90. Checklist de mise en production d'un service confiné

- [ ] Unité systemd avec au minimum : `ProtectSystem=strict`, `ProtectHome=tmpfs`, `PrivateTmp=true`, `NoNewPrivileges=true`
- [ ] `systemd-analyze security` ≤ 3.0 (EXPOSED) ou justification écrite des ✗ restants
- [ ] Utilisateur dédié (`User=`), jamais root sauf besoin justifié (ports < 1024 → `AmbientCapabilities`)
- [ ] Limites : `MemoryMax`, `TasksMax`, `CPUQuota` dimensionnées
- [ ] Profil AppArmor en `enforce` si le service est exposé au réseau
- [ ] Logs d'audit collectés (journald/auditd) et alertes configurées (§85)
- [ ] Procédure de rollback testée (ancienne image / ancien binaire)
- [ ] Test d'intrusion léger : que se passe-t-il si le service est compromis ? (lecture `/etc/shadow` ? réseau ? persistance ?)
- [ ] Documentation : pourquoi chaque exception (`ReadWritePaths`, `cap_add`) existe

---

# PARTIE XII — PENSE-BÊTE DE POCHE

## 91. Pense-bête : une page à imprimer

```
=== SANDBOX LINUX — PENSE-BÊTE ===
NAMESPACES
  lsns -t net,mnt,pid,user          # qui est isolé ?
  sudo unshare --pid --mount --uts --fork --mount-proc bash
  sudo ip netns add sb && sudo ip netns exec sb ip a
  unshare --user --map-root-user bash   # faux root

CGROUPS v2
  systemd-run --scope -p MemoryMax=512M -p CPUQuota=50% -p TasksMax=100 CMD

FIREJAIL
  firejail firefox                   # profil auto
  firejail --noprofile --net=none --seccomp --caps.drop=all --noroot CMD
  firejail --list | --tree | --top   # supervision
  firejail --shutdown=<pid|name>
  sudo firecfg                       # intégration bureau (après chaque MAJ)

SYSTEMD (dans [Service])
  ProtectSystem=strict | ProtectHome=tmpfs | PrivateTmp=true
  PrivateDevices=true | NoNewPrivileges=true
  ProtectKernelTunables=true | ProtectKernelModules=true | ProtectControlGroups=true
  SystemCallFilter=@system-service | SystemCallArchitectures=native
  RestrictAddressFamilies=AF_UNIX AF_INET AF_INET6
  RestrictNamespaces=yes | RestrictRealtime=true | MemoryDenyWriteExecute=true
  CapabilityBoundingSet=  (+ AmbientCapabilities=CAP_NET_BIND_SERVICE si besoin)
  MemoryMax=1G | CPUQuota=50% | TasksMax=100
  systemd-analyze security SERVICE   # viser ≤ 3.0

APPARMOR
  sudo aa-status | aa-complain BIN | aa-enforce BIN
  sudo aa-genprof BIN   # apprentissage (utiliser l'appli à fond)
  sudo aa-logprof       # intégrer les refus légitimes
  sudo journalctl -k | grep "apparmor.*DENIED"

DOCKER DURCI
  docker run --read-only --tmpfs /tmp --cap-drop=ALL --cap-add=NET_BIND_SERVICE \
    --security-opt no-new-privileges --network none --user 10000:10000 \
    --pids-limit 100 --memory 512m IMAGE
  INTERDIT: --privileged, docker.sock monté, :latest en prod, root par défaut

DIAGNOSTIC EXPRESS
  status=31/SYS  -> seccomp trop strict (SystemCallFilter)
  DENIED         -> AppArmor (aa-logprof)
  oom-killer     -> MemoryMax trop bas ou fuite
  Permission denied fichier -> ProtectSystem / ReadWritePaths
```

## 92. Glossaire

| Terme | Définition |
|---|---|
| **Namespace** | Vue isolée d'une ressource noyau (processus, réseau, montages, UID...) |
| **cgroup** | Groupe de contrôle : limite et mesure les ressources (CPU, RAM, pids, I/O) |
| **chroot** | Redéfinit la racine `/` apparente d'un processus (pas une sécurité) |
| **Seccomp-bpf** | Filtre les appels système autorisés pour un processus |
| **Capability** | Morceau du pouvoir root (ex. `CAP_NET_BIND_SERVICE`) attribuable séparément |
| **MAC** | Mandatory Access Control : politique obligatoire (AppArmor, SELinux), au-dessus des permissions Unix (DAC) |
| **DAC** | Discretionary Access Control : les classiques `rwx` Unix |
| **Profil AppArmor** | Liste des accès autorisés pour un programme donné |
| **Contexte SELinux** | Étiquette `user:role:type:level` collée sur chaque objet |
| **User namespace** | Namespace qui mappe root interne vers un UID non privilégié externe |
| **pivot_root** | Change la racine d'un namespace de montage (base des conteneurs) |
| **Bubblewrap (bwrap)** | Outil minimal de sandbox par namespaces, brique de Flatpak |
| **nspawn** | `systemd-nspawn` : chroot amélioré avec namespaces + cgroups |
| **SIGSYS (31)** | Signal envoyé quand seccomp tue un processus |
| **W^X** | Write XOR Execute : une page mémoire ne peut être à la fois inscriptible et exécutable |
| **Fork bomb** | Programme qui se réplique jusqu'à épuisement des processus (`:(){ :|:& };:`) |
| **Évasion (escape)** | Sortie du sandbox par l'attaquant (faille noyau, mauvaise config) |
| **Défense en profondeur** | Empilement de couches de sécurité indépendantes |
| **Moindre privilège** | Ne donner que les droits strictement nécessaires |
| **Drop-in systemd** | Fichier `/etc/systemd/system/X.d/*.conf` qui surcharge une unité sans la modifier |
| **tmpfs** | Système de fichiers en RAM, vide au montage, jeté au démontage |

## 93. Quiz : 10 questions pour valider

**Q1.** Un processus root s'échappe-t-il facilement d'un chroot classique ?
**Q2.** Quelle directive systemd rend `/usr` et `/boot` (entre autres) en lecture seule pour un service ?
**Q3.** Que signifie un service tué avec `status=31/SYS` dans `journalctl` ?
**Q4.** Citez 3 namespaces Linux et ce qu'ils isolent.
**Q5.** Pourquoi faut-il éviter `sudo firejail firefox` ?
**Q6.** Quelle option Docker donne au conteneur un accès quasi total à l'hôte et doit être bannie ?
**Q7.** AppArmor : quelle est la différence entre les modes `complain` et `enforce` ?
**Q8.** Quel fichier SELinux restaure-t-on avec `restorecon` après avoir déplacé un fichier web ?
**Q9.** Que protège `NoNewPrivileges=true` ?
**Q10.** Sous X11, pourquoi un programme sandboxé avec Firejail peut-il quand même espionner le clavier, et quelle est la parade ?

<details>
<summary><b>Réponses</b></summary>

**R1.** Oui, trivialement (ex. descripteur ouvert sur `/` avant `chroot` + `fchdir`, voir §15). Le chroot n'est pas une barrière de sécurité contre root.
**R2.** `ProtectSystem=strict` (ou `yes`/`full` selon le niveau). Avec `ReadWritePaths=` pour les exceptions.
**R3.** Le processus a été tué par seccomp (SIGSYS = signal 31) : `SystemCallFilter` bloque un appel système que le programme tente d'utiliser.
**R4.** Exemples : `pid` (vue des processus), `net` (interfaces, routes, ports), `mnt` (arborescence des montages), `user` (mapping UID/GID), `uts` (hostname), `ipc` (mémoire partagée).
**R5.** Parce que le sandbox hérite alors des privilèges root : l'isolation (user namespace, `noroot`) devient illusoire et une évasion donne root sur l'hôte.
**R6.** `--privileged` : désactive seccomp/AppArmor, donne accès aux devices de l'hôte. À remplacer par `--device` / `--cap-add` ciblés.
**R7.** `complain` : journalise les violations sans bloquer (mode apprentissage). `enforce` : bloque et journalise.
**R8.** Le **contexte SELinux** (type) du fichier : `mv` conserve l'ancien type, d'où des refus même avec les bonnes permissions Unix. `restorecon` réapplique le type prévu par la politique.
**R9.** Contre toute élévation de privilèges du processus et de ses enfants : setuid/setgid, capabilities fichier et `sudo` deviennent inopérants.
**R10.** Parce que le protocole X11 permet à tout client de lire les événements clavier/souris et de capturer l'écran. Parades : `firejail --x11=xephyr` (serveur X imbriqué) ou passer à Wayland (isolation native par fenêtre).
</details>

## 94. Pour aller plus loin

**Documentation officielle :**
- `man systemd.exec` — LA référence des directives de sandboxing (très complète)
- `man firejail-profile` — toutes les directives de profils Firejail
- `man apparmor.d` — syntaxe des profils AppArmor
- Namespaces : `man 7 namespaces`, `man 1 unshare`, `man 8 ip-netns`
- cgroups v2 : `Documentation/admin-guide/cgroup-v2.rst` du noyau

**Outils à explorer ensuite :**
- **Bubblewrap (`bwrap`)** : la brique minimale (`flatpak` repose dessus) — idéal pour scripter ses propres sandboxes
- **gVisor (Google)** : sandbox userspace qui intercepte les syscalls (isolation proche d'une VM pour des conteneurs)
- **Kata Containers** : conteneurs dans des micro-VM (compromis Docker/KVM)
- **Qubes OS** : poste de travail où chaque appli tourne dans sa VM (paranoïa assumée)
- **Trivy / Grype** : scan de vulnérabilités des images

**Lectures :**
- "Linux Containers in 500 lines of Go" — comprendre les namespaces en les codant
- La documentation seccomp-bpf (`man 2 seccomp`)
- CIS Benchmarks (Debian, Docker) : checklists de durcissement auditables

**Exercices pratiques :**
1. Prenez un de vos services et faites passer son score `systemd-analyze security` sous 3.0.
2. Écrivez un profil AppArmor en `complain` pour un outil interne, passez en `enforce` après une semaine.
3. Montez un pipeline "PDF suspect" (§79) et testez-le avec un PDF EICAR-like inoffensif.
4. Auditez vos `docker run` / compose avec la checklist §66.

## 95. Modèle de fiche service confiné (à remplir par l'équipe)

```markdown
# Fiche confinement — <nom du service>
- Responsable :
- Date de mise en confinement :
- Score systemd-analyze security : ___ (objectif ≤ 3.0)
- Utilisateur dédié : ___
- Directives appliquées : ProtectSystem=___ ProtectHome=___ ...
- Exceptions justifiées :
  - ReadWritePaths=___ parce que ___
  - cap_add ___ parce que ___
- Profil AppArmor : ___ (enforce/complain)
- Limites ressources : MemoryMax=___ TasksMax=___ CPUQuota=___
- Alertes configurées : ___
- Test d'intrusion léger (date + résultat) : ___
- Prochaine revue : ___
```

## 96. Anti-sèche sysctl durcissement (rappel §77)

```bash
# /etc/sysctl.d/10-durcissement.conf — appliquer avec sysctl --system
kernel.yama.ptrace_scope = 1          # ptrace restreint
kernel.kexec_load_disabled = 1        # pas de kexec
kernel.dmesg_restrict = 1             # dmesg pour root uniquement
kernel.unprivileged_bpf_disabled = 2  # eBPF non privilégié désactivé
net.core.bpf_jit_harden = 2          # durcit le JIT eBPF
kernel.perf_event_paranoid = 3        # perf restreint
# kernel.unprivileged_userns_clone = 1  # GARDER à 1 si Firejail/podman utilisés !
```

## 97. Ce que ce guide ne couvre pas (périmètre honnête)

- L'analyse de malware avancée (sandbox dynamique type Cuckoo, INetSim) : autre métier.
- Le durcissement du noyau par compilation (options `CONFIG_*`, grsecurity).
- La sécurité des orchestrateurs (Kubernetes : PodSecurity, NetworkPolicies, OPA/Gatekeeper).
- Le chiffrement (LUKS, TLS) : complémentaire, pas remplacé par le sandboxing.
- La réponse à incident (forensique) : le sandbox limite l'impact, il ne remplace pas un plan de réponse.

## 98. Historique des révisions du guide

| Date | Version | Changement |
|---|---|---|
| 2026-09-26 | 1.0 | Création : 8 parties, 98 sections, quiz, pense-bête |

## 99. Licence et réutilisation interne

Ce guide est un document de travail interne pour l'équipe systèmes. Les scripts et configurations sont fournis "tels quels" : **testez toujours en recette avant production**. Les exemples avec `sudo`, montages et conteneurs modifient le système : ne les exécutez pas aveuglément sur un serveur de production.

## 100. Le mot de la fin : la discipline bat l'outil

Le meilleur sandbox ne vaut rien sans discipline d'usage : profils testés, mises à jour suivies, alertes lues, exceptions documentées. Commencez petit — un service systemd durci, un profil Firejail pour le PDF — mesurez avec `systemd-analyze security`, et étendez. Dans six mois, chaque nouveau service de l'équipe naîtra confiné par défaut : c'est ça, le vrai objectif.

---

*Fin du guide — bon confinement.*
