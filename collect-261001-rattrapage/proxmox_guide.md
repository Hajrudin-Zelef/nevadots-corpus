# Proxmox VE — Guide ultra-complet (2000+ lignes)

> Tout sur Proxmox VE, l'hyperviseur open source : installation,
> ZFS, Ceph, cluster, HA, sauvegardes (vzdump + Proxmox Backup
> Server), réseau, sécurité, GPU passthrough, cloud-init,
> Terraform, API, NUT (onduleur), dépannage — **angle chef de
> service systèmes**.
>
> Basé sur Proxmox VE 8.x/9.x (Debian 12/13). Les menus exacts
> bougent légèrement entre versions : **validez sur votre version**
> (`pveversion -v`).

---

## 1. Proxmox VE — pourquoi c'est le bon choix

- **Hyperviseur bare-metal** basé sur Debian : KVM (VM) + LXC
  (conteneurs) dans une seule interface web.
- **Gratuit et open source** (AGPLv3) ; abonnement Enterprise
  optionnel (support + dépôts stables).
- **Tout intégré** : clustering, HA, Ceph, pare-feu, sauvegardes,
  SDN — sans licence par socket comme VMware.
- Parfait pour PME/ETI africaines : un seul outil, matériel
  standard, pas de coûts cachés.

---

## 2. Architecture — comprendre avant d'installer

```
┌─────────────────────────────────────────────┐
│              Proxmox VE (Debian)            │
│  ┌──────────┐  ┌──────────┐  ┌────────────┐  │
│  │ KVM (VM) │  │ LXC      │  │ Ceph / ZFS │  │
│  │ Windows, │  │ Linux    │  │ Stockage   │  │
│  │ Linux    │  │ léger    │  │ distribué  │  │
│  └──────────┘  └──────────┘  └────────────┘  │
│  Cluster (corosync) │ HA │ Firewall │ Backup│
└─────────────────────────────────────────────┘
```
- **KVM** : virtualisation complète (tout OS, dont Windows).
- **LXC** : conteneurs système Linux (10× plus légers, démarrage
  en secondes, snapshots instantanés).
- **Règle** : un service = une VM ou un conteneur (jamais de
  services en dur sur l'hôte).

---

## 3. Prérequis matériels

| Usage | CPU | RAM | Disques | Réseau |
|---|---|---|---|---|
| Lab / test | 4 cœurs | 16 Go | 1 SSD | 1 GbE |
| PME (10–20 VM) | 8–16 cœurs | 64 Go | 2 SSD (ZFS mirror) + HDD | 2× 1 GbE |
| Production | 16+ cœurs | 128 Go+ | SSD NVMe + Ceph | 2× 10 GbE |
| Cluster Ceph | 3 nœuds min | 64 Go+/nœud | SSD par OSD | 10 GbE dédié Ceph |

- **CPU** : Intel VT-x/AMD-V + **EPT/NPT** (vérifier dans le BIOS).
- **RAM** : la ressource n°1 — toujours prévoir 20 % de marge.
- **Disques** : SSD obligatoire pour l'OS et les VM (un HDD =
  des VM au ralenti).
- **Éviter** : cartes RAID hardware avec cache sans batterie
  (ZFS veut du JBOD/HBA).

---

## 4. Installation

1. Télécharger l'ISO (proxmox.com), vérifier le SHA256.
2. Boot → Install Proxmox VE → disque cible.
3. **Options avancées (F2)** : choisir **ZFS (RAID1)** si 2 disques.
4. Pays, mot de passe root, **IP fixe + FQDN** (ex. `pve01.local`).
5. Reboot → interface web : `https://IP:8006` (root + mot de passe).

### 4.1 Installation sur Debian existante
```bash
# Possible (dépôt Proxmox sur Debian 12/13), mais l'ISO est plus propre
```

---

## 5. Premier boot — checklist

```
□ Web UI accessible (https://IP:8006), certificat (ACME §33)
□ Dépôts : no-subscription (lab) ou enterprise (prod) — §34
□ apt update && apt full-upgrade && reboot si kernel
□ Stockage : vérifier (local, local-lvm / ZFS)
□ Réseau : vmbr0 sur la bonne interface, IP, DNS, NTP
□ Alertes email configurées (§37)
□ Sauvegarde : PBS ou vzdump planifié (§18-19)
□ Utilisateurs : pas de root partagé (§29), 2FA (§30)
□ Pare-feu datacenter activé (§32)
□ NUT (onduleur) si applicable (§68)
□ Fiche d'exploitation (§76)
```

---

## 6. L'interface web — tour du propriétaire

- **Datacenter** : vue globale, cluster, stockage, pare-feu, ACME.
- **Nœud** : résumé, shell, mises à jour, disques, syslog.
- **VM/Conteneur** : console (noVNC/SPICE), matériel, options,
  snapshots, backup, pare-feu.
- **Raccourcis** : double-clic = console ; clic droit = actions.
- **Shell** : chaque nœud a un terminal web (root) — pratique,
  mais préférez SSH + clés.

---

## 7. Stockage — les concepts

| Type | Usage | Partagé ? |
|---|---|---|
| **local** (dir) | ISO, templates, backups vzdump | Non |
| **local-lvm** / **local-zfs** | Disques VM/LXC | Non |
| **NFS** | ISO, backups, (VM si rapide) | Oui |
| **Ceph/RBD** | Disques VM en cluster | Oui |
| **CephFS** | Fichiers partagés | Oui |
| **iSCSI** | LUN SAN | Oui |
| **PBS** | Sauvegardes dédupliquées | Oui |

- **Règle d'or** : HA et migration à chaud exigent un stockage
  **partagé** (Ceph, NFS…).

---

## 8. ZFS en détail — le filesystem qui pardonne

### 8.1 Créer un pool
```bash
zpool create -f tank mirror /dev/sdb /dev/sdc
# ou RAIDZ : zpool create -f tank raidz /dev/sdb /dev/sdc /dev/sdd
zpool status tank
```

### 8.2 Les réglages qui comptent
```bash
zfs set compression=lz4 tank        # gratuit, souvent +30 % d'espace
zfs set atime=off tank              # moins d'écritures
zfs set recordsize=16K tank/vm-disks # adapté aux VM
```

### 8.3 Snapshots et clones
```bash
zfs snapshot tank/vm-100-disk-0@sauve-avant-maj
zfs rollback tank/vm-100-disk-0@sauve-avant-maj
zfs list -t snapshot
```

### 8.4 Surveillance
```bash
zpool status -x          # santé (doit dire "all pools are healthy")
zpool iostat -v 5
zfs list
# Scrub mensuel (intégré à Proxmox) : détecte la corruption silencieuse
```

### 8.5 Règles ZFS
- **ashift=12** pour les disques 4K (défaut sur les ISO récentes).
- **Jamais plus de 80 %** de remplissage (les perfs s'effondrent).
- **RAM** : ZFS adore la RAM (ARC) — 1 Go de RAM par To de
  stockage en règle grossière.
- **SLOG** (SSD) : pour le sync intensif (bases de données).
- **L2ARC** : cache lecture SSD (optionnel).

---

## 9. LVM-thin — l'alternative simple

- `local-lvm` : thin provisioning (les VM ne consomment que ce
  qu'elles utilisent).
- Snapshots possibles (moins souples que ZFS).
- Bien pour un nœud unique sans besoin ZFS.

---

## 10. Réseau — bridges, bonds, VLAN

### 10.1 Le bridge vmbr0 (base)
```
Internet/LAN ──► eth0 ──► vmbr0 ──► VM (tap)
```
- Créé à l'install. Les VM branchées sur vmbr0 sont sur le LAN.

### 10.2 Bond (2 cartes)
- Nœud → Réseau → Créer → Linux Bond (mode 802.3ad/LACP ou
  active-backup) → vmbr0 sur le bond.
- **LACP** : configurer aussi le switch !

### 10.3 VLAN
- Bridge « VLAN aware » : cocher sur vmbr0 → tag par VM
  (Matériel → Réseau → Tag VLAN).
- Séparer : management, VM, Ceph, backup (4 VLAN = propre).

### 10.4 En CLI (`/etc/network/interfaces`)
```
auto vmbr0
iface vmbr0 inet static
    address 192.168.1.11/24
    gateway 192.168.1.1
    bridge-ports eth0
    bridge-stp off
    bridge-fd 0
```
- ⚠️ Toujours garder un accès (IPMI) quand on touche au réseau.

---

## 11. SDN (Software Defined Network)

- Datacenter → SDN : zones (VLAN, QinQ, VXLAN, EVPN), VNets,
  sous-réseaux, DHCP, IPAM.
- **VXLAN** : réseau overlay multi-sites sans VLAN trunk.
- Pour les infras qui grandissent : l'IPAM intégré évite le
  tableur d'IP.

---

## 12. Créer une VM KVM

### 12.1 Via l'UI
Créer VM → Nom, OS (ISO), Système (BIOS **OVMF/UEFI** pour les OS
récents), Disque (**VirtIO SCSI**, discard), CPU (cores, type
**host**), Mémoire (**ballooning**), Réseau (**VirtIO**).

### 12.2 Via CLI
```bash
qm create 100 --name srv-web --memory 4096 --cores 2 \
  --net0 virtio,bridge=vmbr0 --scsihw virtio-scsi-pci
qm importdisk 100 /var/lib/vz/template/iso/ubuntu.iso local-lvm
qm set 100 --scsi0 local-lvm:vm-100-disk-0 --boot order=scsi0
qm start 100
```

### 12.3 Les bons réglages
- **Disque** : VirtIO SCSI + `discard` + `iothread`.
- **CPU** : type `host` (perfs), activer NUMA si > 8 vCPU.
- **Agent QEMU** : installer dans l'invité (`qemu-guest-agent`) →
  IP visible, shutdown propre, snapshots cohérents.
---

## 13. Templates VM + cloud-init — déployer en 2 minutes

```bash
# 1. Créer la VM, installer l'OS, qemu-guest-agent, cloud-init
# 2. Nettoyer : apt clean, cloud-init clean, vider les clés SSH
qm template 100   # convertit en template
# 3. Cloner :
qm clone 100 200 --name srv01 --full
qm set 200 --ciuser admin --sshkeys ~/.ssh/id_rsa.pub \
  --ipconfig0 ip=192.168.1.20/24,gw=192.168.1.1
qm start 200
```
- **Full clone** : copie indépendante. **Linked clone** : basé sur
  le template (rapide, mais dépendant).
- En production : full clone + cloud-init = standard.

---

## 14. Conteneurs LXC

### 14.1 Créer un CT
```bash
# Télécharger un template : CT Templates → ubuntu-24.04-standard
pct create 300 local:vztmpl/ubuntu-24.04-standard_24.04-1_amd64.tar.zst \
  --hostname ct-web --cores 2 --memory 2048 --rootfs local-lvm:8 \
  --net0 name=eth0,bridge=vmbr0,ip=192.168.1.30/24,gw=192.168.1.1
pct start 300 && pct enter 300
```

### 14.2 Privilégié vs non privilégié
- **Non privilégié** (défaut) : plus sûr (UID mappés).
- **Privilégié** : nécessaire pour Docker-dans-LXC, NFS…
  (cocher « nesting » pour Docker).

### 14.3 LXC vs VM — que choisir ?
| | LXC | VM |
|---|---|---|
| Démarrage | secondes | dizaines de secondes |
| Surcoût | ~nul | quelques % |
| OS | Linux uniquement | Tout (Windows…) |
| Snapshots | instantanés | rapides (qcow2/ZFS) |
| Isolation | correcte | forte |

---

## 15. Snapshots — VM et CT

```bash
qm snapshot 100 avant-maj --description "Avant kernel 6.8"
qm rollback 100 avant-maj
qm delsnapshot 100 avant-maj
pct snapshot 300 avant-maj
```
- Snapshots **avec RAM** : possible (état complet), plus lourds.
- ⚠️ Les snapshots ne sont pas des sauvegardes (même stockage !).
- Nettoyer les vieux snapshots (ils ralentissent et mangent
  l'espace).

---

## 16. Sauvegardes vzdump — la base

### 16.1 Configurer
Datacenter → Sauvegarde → Ajouter :
- Nœud, stockage (NFS/PBS), planification (`02:00`),
- Mode : **snapshot** (sans interruption), compression **zstd**,
- Rétention : garder 7 quotidiens, 4 hebdo.

### 16.2 CLI
```bash
vzdump 100 --mode snapshot --compress zstd --storage nas-backup
vzdump 100-110 --all 0   # tout le nœud
```

### 16.3 Restaurer
```bash
qmrestore /var/lib/vz/dump/vzdump-qemu-100.vma.zst 200
pct restore 300 /var/lib/vz/dump/vzdump-lxc-300.tar.zst
```
- Restaurer **sous un autre ID** pour tester sans écraser.

---

## 17. Proxmox Backup Server (PBS) — le niveau pro

### 17.1 Pourquoi PBS plutôt que vzdump ?
- **Déduplication** (blocs) : 10 VM Ubuntu = ~1,2× l'espace d'une.
- **Incrémentiel** réel, chiffrement côté client, vérification
  d'intégrité, **restauration de fichiers** (file restore).
- S'installe sur une machine dédiée (ou VM).

### 17.2 Mise en place
```bash
# Sur le PBS : Datastore → Ajouter (disque dédié)
# Sur PVE : Stockage → Ajouter → Proxmox Backup Server
#   (empreinte du certificat à vérifier !)
```
- Jobs : Datacenter → Sauvegarde → stockage PBS.
- **Rétention** : keep-daily=7, keep-weekly=4, keep-monthly=6.
- **Vérification** : job « Verify » hebdo (corruption détectée).
- **Sync** : répliquer vers un 2ᵉ PBS hors site (règle 3-2-1).
- **Garbage Collect** : planifié (nettoie les blocs orphelins).

### 17.3 Chiffrement
- Clé générée côté PVE, **sauvegardée hors site** (sans la clé,
  les backups sont perdus).

---

## 18. Stratégie de sauvegarde complète (chef de service)

```
NIVEAU 1 : PBS local (quotidien, rétention 7j/4s/6m)
NIVEAU 2 : Sync PBS → site distant (hebdo)
NIVEAU 3 : Dump mensuel → disque externe déconnecté (ransomware)
TEST : restauration 1 VM/mois (planning tournant)
```
- **Ransomware** : un backup connecté en permanence n'est pas un
  backup. Le disque déconnecté mensuel sauve des entreprises.

---

## 19. Cluster — assembler les nœuds

### 19.1 Créer
Nœud1 : Datacenter → Cluster → Créer (nom).
Nœud2/3 : Rejoindre (IP du nœud1 + mot de passe root).
```bash
pvecm status     # état
pvecm nodes      # membres
```

### 19.2 Règles
- **Noms d'hôtes uniques**, IP fixes, même version Proxmox.
- Réseau cluster **dédié** si possible (corosync est sensible à
  la latence).
- **Nombre impair** de nœuds (ou qdevice) pour le quorum.

### 19.3 Quorum
- Quorum = majorité des votes. Sans quorum : **le cluster se fige**
  (pas de démarrage/migration) — c'est une sécurité, pas un bug.
- 2 nœuds seuls = pas de quorum en cas de panne → ajouter un
  **qdevice** (un petit Debian/Raspberry comme arbitre).

---

## 20. Haute disponibilité (HA)

### 20.1 Activer
Datacenter → HA → Ajouter (VM/CT) → groupe, état « started ».
```bash
ha-manager status
```

### 20.2 Exigences
- Stockage **partagé** (Ceph/RBD, NFS…).
- **Fencing** configuré (IPMI/iDRAC) : sans fencing, pas de HA
  fiable (risque de split-brain).
- Tester : débrancher un nœud → les VM redémarrent ailleurs
  (quelques minutes).

### 20.3 Groupes et priorités
- Groupes : `groupe-prod` (nœuds 1,2,3), priorités par VM.
- **Répartition** : ne pas tout mettre en HA (les VM critiques
  d'abord).

---

## 21. Migration

```bash
qm migrate 100 pve02 --online   # à chaud (stockage partagé)
qm migrate 100 pve02            # à froid
```
- À chaud : quelques secondes de coupure (RAM à copier).
- Sans stockage partagé : migration avec disques (plus longue).
- **Avant maintenance d'un nœud** : migrer les VM (ou HA).

---

## 22. Ceph — le stockage distribué

### 22.1 Principes
- Chaque nœud apporte des disques (OSD) → un seul pool logique.
- Réplication ×3 : un nœud ou des disques peuvent mourir.
- **Exigences** : 3+ nœuds, **10 GbE dédié**, SSD de préférence.

### 22.2 Installation
PVE → Ceph → Installer → configurer le réseau public/cluster →
Créer les OSD (un par disque) → Créer les pools (rbd, cephfs).

### 22.3 Pools et dimensionnement
```bash
ceph osd pool create vm-pool 128 128
# PG : ~100 par OSD (calculateur : ceph osd pool autoscale)
ceph -s    # HEALTH_OK ?
```
- `size=3, min_size=2` : le standard (2 copies min pour écrire).

### 22.4 Maintenance Ceph
```bash
ceph -s ; ceph osd tree ; ceph df
ceph osd out osd.3        # sortir un OSD (remplacement disque)
ceph osd crush reweight ...
# Rebalance auto après ajout/retrait (peut durer des heures)
```
- **Ne jamais** : remplir à plus de 85 %, rebooter 2 nœuds à la
  fois, paniquer pendant un rebalance (c'est normal).

---

## 23. Réplication de stockage (sans Ceph)

- Datacenter → Réplication : réplique les disques ZFS locaux
  vers un autre nœud (toutes les 15 min, configurable).
- **Plan B** : migration rapide en cas de panne (quelques minutes
  de perte), sans Ceph.
- Idéal : 2 nœuds, ZFS local, petits budgets.

---

## 24. Utilisateurs, permissions, ACL

- Datacenter → Permissions : utilisateurs (pveum), groupes, rôles.
```bash
pveum user add admin2@pam
pveum acl modify /vms/100 --users admin2@pam --roles PVEVMUser
```
- **Rôles** : Administrator, PVEVMAdmin, PVEVMUser, PVEDatastoreUser…
- **Jamais de root partagé** : un compte par admin.
---

## 25. 2FA et LDAP/AD

### 25.1 TOTP (2FA)
Datacenter → Utilisateurs → [user] → TFA → Ajouter (QR code).
- Exiger pour les admins (surtout l'UI exposée).

### 25.2 LDAP / Active Directory
Datacenter → Accès → Ajouter → LDAP/AD (serveur, base DN, bind).
- Les comptes du domaine se connectent à Proxmox avec leurs
  identifiants habituels.

---

## 26. Pare-feu intégré (pve-firewall)

- **3 niveaux** : Datacenter → Nœud → VM/CT (le plus spécifique gagne).
- Groupes de sécurité : règles réutilisables (ex. « web » : 80/443).
```bash
pve-firewall status
```
- Activer au niveau datacenter (`Firewall: yes`), puis règles par VM.
- **Options** : anti-spoofing, log des refus (debug).

---

## 27. Sécurité — checklist Proxmox

```
□ Dépôts enterprise (prod) ou no-subscription (lab) — pas test en prod
□ Root : mot de passe fort, pas partagé, 2FA sur les admins
□ UI : certificat ACME/Let's Encrypt (§33), pas exposée sur Internet
       (ou bastion/VPN + 2FA + fail2ban)
□ Pare-feu datacenter + règles par VM
□ Mises à jour : hebdo (fenêtre), kernel + reboot planifié
□ Sauvegardes : PBS + test de restauration mensuel
□ Logs : syslog distant
□ NUT : arrêt propre sur coupure (§68)
□ Accès SSH : clés, pas de mot de passe
```

---

## 28. Mises à jour

```bash
apt update && apt full-upgrade
# Via UI : Nœud → Mises à jour → Rafraîchir → Mettre à niveau
```
- **Dépôts** :
  - `pve-enterprise` : stable, avec abonnement.
  - `pve-no-subscription` : gratuit, légèrement moins testé.
  - `pve-test` : jamais en prod.
- **Kernel** : reboot planifié après maj (fenêtre mensuelle).
- **Cluster** : un nœud à la fois (migrer les VM d'abord !).

---

## 29. Certificat TLS de l'UI (ACME)

- Datacenter → ACME → Ajouter compte → Commander le certificat.
- Fini le warning « certificat non valide » — et le HTTPS propre
  pour les accès distants.

---

## 30. Alertes email

- Datacenter → Options → Email d'alerte (SMTP du nœud).
- `/etc/postfix/main.cf` : relayhost vers votre SMTP.
- Tester : une alerte simulée (débrancher un câble réseau… en lab).

---

## 31. Monitoring — voir le parc

### 31.1 Intégré
- Résumé par nœud/VM : CPU, RAM, disque, réseau (graphes).
- `pvesh get /nodes/pve01/status`.

### 31.2 Zabbix / Prometheus
- Agent Zabbix dans les VM + template Proxmox (API).
- **pve-exporter** (Prometheus) : métriques de tout le cluster →
  Grafana (le tableau de bord du chef de service).

### 31.3 Ce qu'on supervise
- État nœuds, quorum, Ceph HEALTH, espace stockages.
- Sauvegardes : succès/échec (alerte immédiate).
- Températures, SMART des disques.

---

## 32. GPU passthrough (PCI)

```
1. BIOS : VT-d/IOMMU activé
2. Kernel : intel_iommu=on iommu=pt (GRUB, update-grub, reboot)
3. dmesg | grep -e IOMMU   (vérifier)
4. VM → Matériel → Ajouter → Périphérique PCI → le GPU
   (cocher "Primary GPU", ROM-Bar selon carte)
5. Pilote NVIDIA dans l'invité (+ masquer la virtualisation
   : args: -cpu host,+kvm_pv_unhalt pour le code 43)
```
- Usage : VDI, transcodage, IA/ML.
- ⚠️ Le GPU est **dédié** à la VM (plus dispo pour l'hôte).

---

## 33. USB passthrough

- VM → Matériel → Ajouter → Périphérique USB (par ID vendeur/produit).
- Usage : clé de licence, **onduleur USB pour NUT** (§68).

---

## 34. cloud-init en détail (Proxmox)

```bash
qm set 200 --ciuser admin --cipassword '***' \
  --sshkeys ~/.ssh/id_rsa.pub \
  --ipconfig0 ip=192.168.1.20/24,gw=192.168.1.1 \
  --nameserver 192.168.1.1 --searchdomain local
qm set 200 --cicustom "user=local:snippets/user.yaml"
```
- `snippets/` : user-data avancé (packages, runcmd, users…).
- Le template + cloud-init = **VM prête en 60 secondes**.

---

## 35. Terraform — l'infra as code

```hcl
resource "proxmox_vm_qemu" "web" {
  name        = "srv-web-01"
  target_node = "pve01"
  clone       = "template-ubuntu"
  cores       = 2
  memory      = 4096
  ipconfig0   = "ip=192.168.1.21/24,gw=192.168.1.1"
}
```
- Provider : Telmate/bpg. Token API : Datacenter → Permissions →
  API Tokens.
- **Idéal** : recréer tout le parc depuis du code (après sinistre).

---

## 36. API et CLI

```bash
pvesh get /nodes                    # API en CLI
pvesh create /nodes/pve01/qemu -vmid 100 -name test -memory 2048
qm list ; qm config 100 ; qm status 100
pct list ; pct config 300
pvesm status                        # stockages
```
- Scripts : tout ce que fait l'UI passe par l'API → automatisable
  (curl + token).

---

## 37. Hooks et planification

- **Hook scripts** vzdump : `--script` (notifier, monter/démonter…).
- **Jobs planifiés** : Datacenter → Sauvegarde (cron intégré).
- Exemple : snapshot avant maj auto :
```bash
# cron sur le nœud
0 1 * * 0 qm snapshot 100 avant-maj-hebdo && apt ... 
```

---

## 38. Disques — opérations

```bash
qm resize 100 scsi0 +20G              # agrandir (étendre ensuite dans l'OS)
qm move-disk 100 scsi0 ceph-pool      # changer de stockage (à chaud si partagé)
qm set 100 --delete scsi1             # supprimer (détacher d'abord !)
```
- Réduire un disque : **dangereux** (réduire dans l'OS d'abord,
  backup avant).
- `discard` : l'espace libéré dans la VM est rendu au stockage.

---

## 39. Stockages réseau — NFS, iSCSI, SMB

```bash
# NFS : Datacenter → Stockage → Ajouter → NFS (IP, export)
# iSCSI : portail, target → LUN → LVM sur iSCSI
# SMB/CIFS : pour ISO et backups (pas pour les disques VM)
```
- NFS : `vers=4`, jumbo frames si 10 GbE.
- iSCSI + multipath pour la redondance.

---

## 40. Performance — tuning KVM

```
CPU : type=host, pas de surallocation abusive (ratio 2-4:1 max)
RAM : ballooning ON, KSM ON (pages partagées entre VM identiques)
DISQUE : VirtIO SCSI, cache=none (Ceph/ZFS), iothread=1, discard=on
RÉSEAU : VirtIO, multiqueue (queues = vCPU) sur 10 GbE
```
- `ksmtuned` : automatique sur Proxmox.
- NUMA : activer si VM > 8 vCPU (topologie alignée sur l'hôte).

---

## 41. Invités Windows

1. ISO Windows + ISO **virtio-win** (fedorapeople.org).
2. Pendant l'install : charger le pilote (disque non détecté).
3. Après : installer `virtio-win-guest-tools` (agent + pilotes).
4. UEFI/OVMF + TPM pour Windows 11.
---

## 42. Maintenance d'un nœud — sans couper le service

```
1. Migrer les VM vers les autres nœuds (ou HA auto)
2. Vérifier : qm list (vide), ceph -s (si Ceph)
3. apt update && apt full-upgrade
4. Reboot si kernel
5. Vérifier le retour : pvecm status, services, Ceph HEALTH_OK
6. Re-migrer les VM (ou laisser l'équilibrage)
```
- **Jamais** : rebooter 2 nœuds Ceph en même temps.

---

## 43. Remplacement d'un nœud mort

1. `pvecm delnode pve03` (depuis un nœud sain, quorum OK).
2. Nouveau serveur : même hostname/IP, installer Proxmox.
3. `pvecm add` pour rejoindre.
4. Recréer les stockages locaux si besoin, restaurer les VM
   depuis PBS.
- **Leçon** : PBS + configs documentées = un nœud se remplace
  en 2 h.

---

## 44. SMART et ZED — les disques parlent

```bash
smartctl -a /dev/sda | grep -i "reallocated\|pending\|wear"
# SSD : Media_Wearout_Indicator / Percentage Used
```
- **ZED** (ZFS Event Daemon) : notifie les erreurs ZFS par mail.
- Remplacer un disque **avant** qu'il meure : SMART en alerte +
  erreurs ZFS = commande immédiate (délai Afrique !).

---

## 45. Logs — où chercher

```bash
journalctl -u pveproxy      # interface web
journalctl -u pvedaemon     # API
/var/log/syslog            # général
/var/log/kern.log
dmesg | grep -i "error\|fail"
# Cluster : /var/log/corosync/corosync.log
# Ceph : ceph -s, ceph health detail
```

---

## 46. Dépannage — 15 cas terrain

**Cas 1 — L'UI ne répond plus** : `systemctl status pveproxy`,
`pveproxy` restart, disque plein ? (`df -h`), certificat expiré ?

**Cas 2 — Quorum perdu** : `pvecm status` → si 1 nœud sur 3 :
réseau corosync ? switch ? Ne **jamais** forcer le quorum sans
comprendre (`pvecm expected 1` = mesure de dernier recours).

**Cas 3 — VM bloquée** : `qm stop 100` (dur), `qm unlock 100`
(verrou), vérifier le stockage.

**Cas 4 — Migration qui échoue** : stockage partagé ? même nom de
bridge ? assez de RAM sur la cible ? réseau 1 GbE trop lent
(prendre le temps).

**Cas 5 — Ceph HEALTH_WARN** : `ceph health detail` → OSD down ?
placement groups en backfill (normal après ajout) ? disque plein ?

**Cas 6 — Backup qui échoue** : espace sur le stockage de backup ?
VM avec snapshot coincé ? PBS joignable (empreinte cert) ?

**Cas 7 — Plus de snapshots possibles (ZFS)** : pool > 80 % →
nettoyer, `zfs list -t snapshot` (les vieux !).

**Cas 8 — Conteneur LXC sans réseau** : bridge, VLAN tag, DHCP
dans le CT (`/etc/network/interfaces` du CT).

**Cas 9 — Windows sans disque à l'install** : pilote virtio non
chargé → ISO virtio-win + « Charger un pilote ».

**Cas 10 — HA qui ne bascule pas** : stockage partagé ? fencing
configuré ? `ha-manager status` (erreurs ?).

**Cas 11 — Corosync qui flappe** : latence réseau (ping entre
nœuds), MTU, switch saturé → réseau cluster dédié.

**Cas 12 — PBS : dedup qui n'avance plus** : Garbage Collect pas
planifié → le datastore grossit. Planifier GC hebdo.

**Cas 13 — Clé API qui ne marche plus** : token révoqué ? expiration ?
permissions du token (`pveum`).

**Cas 14 — Nœud qui ne rejoint pas le cluster** : heure désync
(chrony !), hostname résolu ?, version identique ?, pare-feu
(ports corosync 5404-5405/udp) ?

**Cas 15 — Après coupure électrique, les VM ne démarrent pas** :
ordre de démarrage (délai), stockage Ceph pas encore HEALTH_OK
(attendre), NUT mal configuré (§68).

---

## 47. Cas pratiques commentés

**Cas A — PME 15 VM, 2 nœuds**
2 serveurs (64 Go, ZFS mirror SSD), réplication toutes les 15 min,
PBS sur NAS, pas de Ceph (surcoût inutile). HA manuel (migration).
Coût maîtrisé, RPO 15 min.

**Cas B — Data center 3 nœuds Ceph**
3 nœuds (128 Go, 4 SSD OSD, 10 GbE dédié Ceph), pools rbd size=3,
HA sur les VM critiques, PBS dédié + sync hors site. RPO ~0, RTO
quelques minutes.

**Cas C — Ransomware un lundi matin**
Les VM chiffrées, mais : PBS avec **rétention immuable** + disque
externe mensuel déconnecté → restauration en 1 journée. Sans le
disque déconnecté, le chiffrement aurait touché les backups
connectés.

**Cas D — Disque SSD mort un vendredi soir**
ZFS mirror : la VM continue. `zpool status` DEGRADED → disque de
rechange en stock → `zpool replace` → resilver le week-end. Lundi :
personne n'a rien vu.

**Cas E — Montée en charge web**
Template + cloud-init + Terraform : 5 nouvelles VM en 10 min
pour absorber le pic. Puis `terraform destroy` après.

---

## 48. Dimensionnement — 3 architectures types

### 48.1 Site PME (budget serré)
- 2 nœuds (8 cœurs, 64 Go, 2×1 To SSD ZFS mirror).
- Réplication 15 min, PBS sur NAS, pas de HA auto.
- ~15 VM/conteneurs.

### 48.2 Production (standard)
- 3 nœuds (16 cœurs, 128 Go, 2×480 Go SSD système + 4×2 To Ceph).
- 10 GbE (Ceph + VM), HA, PBS dédié + sync distant.
- ~50 VM.

### 48.3 Gros site
- 5+ nœuds, Ceph NVMe, 25 GbE, 2 PBS (sync), Terraform, monitoring
  Grafana, NUT + groupe.

---

## 49. Bonnes pratiques production — les 20 règles

```
1. Jamais de service sur l'hôte (tout en VM/CT)
2. Nommage : pve01/02/03, vm-100-nom, ct-300-nom
3. Tags Proxmox par environnement (prod/test)
4. Notes sur chaque VM (rôle, responsable)
5. Templates + cloud-init (pas d'install manuelle)
6. Snapshots avant chaque changement
7. Backups PBS quotidiens + test mensuel
8. Mises à jour : fenêtre mensuelle, 1 nœud à la fois
9. Monitoring : Zabbix/Prometheus + alertes
10. Pare-feu : datacenter + VM
11. 2FA admins, 1 compte/personne
12. Quorum : nombre impair ou qdevice
13. Ceph : jamais > 85 %, jamais 2 nœuds rebootés ensemble
14. ZFS : scrub mensuel, < 80 %
15. NUT : arrêt propre sur coupure
16. Documentation : fiche par VM (§76)
17. Ansible/Terraform : pas de clics manuels répétitifs
18. Logs distants
19. Stock de disques de rechange (délai Afrique !)
20. Exercice de restauration annuel (comme un incendie)
```
---

## 50. NUT — l'onduleur parle à Proxmox (votre double casquette !)

### 50.1 Principe
```
Onduleur (USB/SNMP) → NUT (serveur) → upsmon (clients)
  Coupure → sur batterie → batterie basse → ARRÊT PROPRE
  des VM/CT puis des nœuds, AVANT la fin de l'autonomie.
```

### 50.2 Installation (serveur NUT sur un nœud ou un CT)
```bash
apt install nut-server nut-client
# /etc/nut/ups.conf
[monups]
  driver = usbhid-ups
  port = auto
  desc = "Easy UPS 40 kVA"
# /etc/nut/upsd.users
[admin]
  password = ***
  actions = SET
  instcmds = ALL
# /etc/nut/upsmon.conf
MONITOR monups@localhost 1 admin *** master
SHUTDOWNCMD "/sbin/shutdown -h +0"
# /etc/nut/nut.conf : MODE=netserver (serveur) / netclient (autres nœuds)
systemctl enable --now nut-server nut-monitor
upsc monups   # doit afficher les valeurs (charge %, batterie %, autonomie)
```

### 50.3 Seuils
- `upssched` : à « batterie basse » ou après X minutes sur batterie →
  script d'arrêt.
- **Règle** : déclencher l'arrêt quand il reste **2× le temps
  d'arrêt** des VM (ex. : arrêt = 5 min → seuil à 10 min restantes).

---

## 51. Arrêt automatique sur coupure — scénario complet

```bash
#!/bin/bash
# /usr/local/bin/coupure.sh — appelé par NUT (upssched)
/usr/bin/logger "COUPURE : arrêt propre des VM"
# 1. Arrêter les VM non critiques (ordre : applis → BDD → infra)
/usr/sbin/qm shutdown 101 --timeout 120 &
/usr/sbin/qm shutdown 102 --timeout 120 &
wait
# 2. Arrêter les conteneurs
/usr/sbin/pct shutdown 300 &
wait
# 3. Les nœuds s'arrêtent (upsmon SHUTDOWNCMD)
/sbin/shutdown -h +1 "Arret onduleur"
```

### 51.1 Au retour du courant
- BIOS : **restauration sur AC** (Power On) → les nœuds redémarrent.
- Proxmox : VM en « démarrer au boot » (Options → Start at boot,
  avec **délai/ordre** : infra d'abord, applis ensuite).
- Ordre type : PBS → contrôleur de domaine/DNS → BDD → applis.

### 51.2 Tester !
- **Test trimestriel** : débrancher l'onduleur (fenêtre planifiée),
  chronométrer, vérifier l'arrêt propre et le redémarrage.
- Un scénario NUT jamais testé = un scénario qui ne marche pas.

---

## 52. Politique de sauvegarde détaillée

```
QUOTIDIEN (02:00) : toutes les VM/CT → PBS (rétention 7j)
HEBDO (dimanche)  : vérification PBS (Verify job)
MENSUEL           : sync PBS → site distant + disque externe déconnecté
TEST              : restauration 1 VM/mois (tournant), rapport
EXCLUSIONS        : caches, /tmp, données re-générables (documentées)
```

### 52.1 Fenêtres et impact
- Mode snapshot : impact ~nul (quelques secondes de freeze I/O).
- Éviter les backups pendant les batchs lourds.
- **Jumbo** : étaler (VM critiques à 02:00, le reste à 03:00).

---

## 53. Restauration — procédures pas à pas

### 53.1 Restaurer une VM complète (PBS)
PBS → Datastore → Backups → [VM] → Restaurer → nouvel ID →
vérifier le réseau (MAC, IP) → démarrer → tester l'appli.

### 53.2 Restaurer un fichier (file restore)
PBS → File Restore → monter le backup → naviguer → télécharger
le fichier. **Sans restaurer toute la VM** (le cas n°1 en prod).

### 53.3 Reprise après sinistre (site perdu)
1. Nouveau cluster (même version).
2. PBS distant → restaurer les VM critiques d'abord (ordre §51.1).
3. Reconfigurer réseau/DNS.
4. **Objectif** : RTO < 1 jour (d'où l'exercice annuel).

---

## 54. PBS avancé

- **Namespaces** : isoler les clients (multi-sites, multi-équipes).
- **Sync jobs** : pull/push entre PBS (bande passante limitée :
  `rate-in`).
- **Tape backup** : LTO pour l'archivage longue durée (10 ans).
- **Prune** : politiques par groupe (garder les mensuels 1 an…).
- **Clés de chiffrement** : une par client, stockées hors site
  (coffre + papier).

---

## 55. Ceph avancé

### 55.1 CRUSH map
- Règles : séparer SSD/HDD, répartir par rack/salle (`crush
  location`).
- `ceph osd crush move` : placer les OSD correctement.

### 55.2 Cache tiering / classes
- Classes de périphériques : `ceph osd crush set-device-class`
  (ssd/hdd) → pools « chauds » (SSD) et « froids » (HDD).

### 55.3 Réparation
```bash
ceph pg dump | grep -v active+clean   # PG non sains
ceph osd pool set vm-pool pg_autoscale_mode on
rados bench -p vm-pool 30 write       # test de perf (pool de test !)
```

---

## 56. Réseau avancé

### 56.1 Open vSwitch (alternative au bridge Linux)
- Fonctionnalités : sFlow, OpenFlow, bonds avancés.
- Nœud → Réseau → Créer → OVS Bridge/IntPort.

### 56.2 LACP — checklist
```
□ Switch : port-channel en mode LACP actif
□ Proxmox : bond 802.3ad, miimon 100
□ Tester : débrancher un câble → pas de coupure
□ Débit : iperf3 entre nœuds (proche du théorique ?)
```

### 56.3 MTU 9000 (jumbo)
- Sur le réseau Ceph/backup : +10–20 % de débit.
- **Partout ou nulle part** (un maillon à 1500 = fragmentation).

---

## 57. SDN avancé — multi-site

- **EVPN** : étendre un VXLAN sur plusieurs sites (routé).
- **IPAM** : plus de tableur d'IP, le DHCP SDN attribue.
- Cas : 2 sites reliés (VPN/fibre) avec les mêmes VNets.

---

## 58. Sécurité avancée

- **CIS Benchmark** : durcir l'hôte Debian (mot de passe GRUB,
  `/boot` en lecture seule, auditd).
- **Ports** : 8006 (UI) jamais exposé brut → VPN/bastion.
- **Clés API** : rotation annuelle, périmètre minimal.
- **Mises à jour** : fenêtre mensuelle, jamais en retard de 2 versions.
- **Sauvegarde des configs** : `/etc/pve` (versionné en git !),
  `/etc/network/interfaces`, `/etc/ceph`.

```bash
cd /etc/pve && git init && git add -A && git commit -m "init"
# + push vers un dépôt privé après chaque changement majeur
```

---

## 59. API — exemples curl

```bash
# Token : PVEAPIToken=user@pam!token=xxxx
curl -sk -H "Authorization: PVEAPIToken=root@pam!mon-token=xxx" \
  https://pve01:8006/api2/json/nodes
curl -sk -H "Authorization: ..." \
  https://pve01:8006/api2/json/nodes/pve01/qemu | jq '.data[].name'
# Démarrer une VM :
curl -sk -X POST -H "Authorization: ..." \
  https://pve01:8006/api2/json/nodes/pve01/qemu/100/status/start
```
---

## 60. Terraform — aller plus loin

```hcl
# main.tf — parc complet
resource "proxmox_vm_qemu" "srv" {
  count       = 3
  name        = "srv-app-0${count.index + 1}"
  target_node = "pve01"
  clone       = "tpl-ubuntu-2404"
  full_clone  = true
  cores       = 2
  memory      = 4096
  scsihw      = "virtio-scsi-pci"
  disk {
    size    = "30G"
    storage = "ceph-rbd"
  }
  network {
    model  = "virtio"
    bridge = "vmbr0"
    tag    = 10
  }
  ipconfig0 = "ip=192.168.10.2${count.index + 1}/24,gw=192.168.10.1"
  sshkeys   = file("~/.ssh/id_rsa.pub")
}
```
- `terraform plan/apply/destroy` : le parc devient du code.
- **State** : backend distant (pas en local seul).

---

## 61. Ansible + Proxmox

```yaml
# Après Terraform : configurer les VM
- hosts: proxmox_vms
  become: true
  roles:
    - base        # ssh, fail2ban, zabbix-agent, unattended-upgrades
    - web         # nginx + TLS
    - monitoring
```
- Inventaire dynamique : plugin `community.general.proxmox`
  (les VM apparaissent automatiquement).

---

## 62. Monitoring avancé — Grafana

- **pve-exporter** → Prometheus → Grafana : dashboard « Proxmox
  Cluster » (CPU/RAM/disque par nœud/VM, Ceph, backups).
- Alertes : VM down, Ceph HEALTH_WARN, backup failed, disque > 85 %.
- Le dashboard affiché au bureau = la vitrine du service.

---

## 63. ISO et templates — gestion

- Stockage `local` : ISO (trier par OS), templates LXC
  (`pveam update && pveam download local ubuntu-24.04-standard`).
- **Nettoyer** : les ISO de 2019 ne servent plus.
- Templates versionnés : `tpl-ubuntu-2404-v3` (date dans la note).

---

## 64. Windows Server — bonnes pratiques

```
□ VirtIO (disque/réseau) + guest tools + QEMU agent
□ Désactiver la mise en veille, pare-feu Windows adapté
□ Activation : KMS ou clé par VM (licences !)
□ Antivirus : exclusions des dossiers applicatifs
□ Windows Update : fenêtre (pas pendant les backups)
□ Ballooning : OK (driver virtio-balloon)
```

---

## 65. Docker sur Proxmox — où le mettre ?

- **Dans une VM** : le plus propre (isolation, snapshots).
- **Dans un LXC** : possible (privilégié + nesting), plus léger,
  mais moins isolé.
- **Jamais sur l'hôte** : l'hôte reste un hyperviseur pur.

---

## 66. Kubernetes sur Proxmox

- 3 VM (control-plane) + N workers, Terraform + Ansible
  (kubespray) ou Talos.
- Stockage : CSI Ceph RBD (le combo gagnant).
- Commencer petit : k3s dans 3 VM pour les premiers clusters.

---

## 67. Multi-sites — les options

1. **Cluster étendu** : corosync exige < 5 ms de latence → fibre
   noire/métro uniquement.
2. **Clusters séparés + PBS sync** : le standard (RPO = fréquence
   de sync).
3. **SDN EVPN** : mêmes réseaux logiques sur 2 sites.

---

## 68. Migration depuis VMware / Hyper-V

```bash
# Méthode : exporter en OVF/OVA → importer
qm importovf 100 export.ova local-lvm
# Puis : installer les pilotes VirtIO, QEMU agent, nettoyer les
# anciens tools VMware, réactiver Windows (changement de matériel)
```
- Tester **une VM pilote** avant de tout migrer.
- Licences Windows : vérifier la transférabilité.

---

## 69. Erreurs classiques des débutants

1. **Installer sur 1 disque sans RAID** → le disque meurt, tout meurt.
2. **ZFS rempli à 95 %** → perfs effondrées, snapshots impossibles.
3. **Backups jamais testés** → le grand classique.
4. **Cluster à 2 nœuds sans qdevice** → split-brain à la première panne.
5. **Exposer le 8006 sur Internet** → attaqué en quelques heures.
6. **Root partagé** → on ne sait plus qui a fait quoi.
7. **Pas de NUT** → coupure = corruption.
8. **Surallocation CPU 10:1** → tout rame, personne ne comprend.
9. **Mélanger les dépôts** (enterprise + test) → système instable.
10. **Ne pas documenter** → le savoir part avec le départ.

---

## 70. Pense-bête de poche

```
pvecm status | ceph -s | zpool status -x | pvesm status
qm list | pct list | ha-manager status
vzdump --help | pbs : verify hebdo, GC hebdo, sync distant
Backups testés 1×/mois | NUT testé 1×/trimestre
Dépôts : enterprise (prod) | Mises à jour : 1 nœud à la fois
Quorum impair | Ceph < 85 % | ZFS < 80 %
8006 : jamais sur Internet brut | 2FA admins
```

---

## 71. Questions pièges

1. Pourquoi le cluster se fige sans quorum ? → Sécurité : éviter
   le split-brain (2 cerveaux qui décident différemment).
2. Snapshot = sauvegarde ? → Non (même stockage, pas de 3-2-1).
3. HA sans stockage partagé ? → Impossible (la VM doit être
   accessible depuis l'autre nœud).
4. Pourquoi tester NUT en vrai ? → Un scénario non testé ne marche pas.
5. ZFS : pourquoi < 80 % ? → Le copy-on-write a besoin d'espace
   libre pour rester performant.
6. Linked clone en prod ? → Non (dépend du template).
7. Pourquoi `host` comme type CPU ? → Perfs max (flags CPU natifs).
8. Fencing : pourquoi obligatoire en HA ? → Éviter 2 instances de
   la même VM (corruption disque).
9. PBS vs vzdump ? → Dedup, incrémental, chiffrement, file restore.
10. Ransomware et backups connectés ? → Chiffrés aussi → disque
    déconnecté mensuel.
---

## 72. Glossaire

| Terme | Signification |
|---|---|
| PVE | Proxmox Virtual Environment |
| PBS | Proxmox Backup Server |
| KVM | Virtualisation complète (VM) |
| LXC | Conteneurs système Linux |
| qm / pct | CLI pour VM / conteneurs |
| pvesh | CLI pour l'API |
| corosync | Protocole de cluster (quorum) |
| qdevice | Arbitre externe (quorum à 2 nœuds) |
| HA | Haute disponibilité (redémarrage auto) |
| Fencing | Isolation d'un nœud défaillant |
| OSD | Disque Ceph |
| PG | Placement Group (Ceph) |
| RBD | Block device Ceph |
| ZFS | Filesystem (snapshots, checksums) |
| ARC | Cache RAM de ZFS |
| Scrub | Vérification d'intégrité ZFS |
| vzdump | Outil de sauvegarde intégré |
| Template | Modèle de VM/CT |
| cloud-init | Config auto des VM au boot |
| NUT | Network UPS Tools |
| RPO / RTO | Perte max / durée max de reprise |

---

## 73. Fiche VM/CT — modèle (une par machine)

```
ID : 100  Nom : srv-web-01  Type : VM (KVM)
Nœud : pve01  Cluster : prod
OS : Ubuntu 24.04  Rôle : serveur web intranet
CPU : 2  RAM : 4 Go  Disque : 30 Go (ceph-rbd)
Réseau : vmbr0, VLAN 10, IP 192.168.10.21/24
Responsable : ___  Mise en service : ___
Backup : PBS quotidien 02:00  HA : oui (groupe prod)
Notes : ___
```

---

## 74. Fiche nœud — modèle

```
Nœud : pve01  IP : 192.168.1.11  Rôle : hyperviseur prod
Matériel : ___ (CPU/RAM/disques, n° série, garantie jusqu'au ___)
OS : Proxmox VE ___  Dépôt : enterprise
Stockage : ZFS tank (mirror 2×1 To), Ceph (4 OSD)
Réseau : bond0 (eth0+eth1 LACP), vmbr0, VLAN aware
NUT : oui (client du serveur nut sur pve02)
Dernière maj : ___  Prochaine fenêtre : ___
```

---

## 75. Fiche site Proxmox — modèle

```
SITE : ___
Cluster : ___ (nœuds : ___, quorum : ___)
Stockage : ___ (Ceph/ZFS/NFS, % utilisé : ___)
PBS : ___ (datastore ___, sync distant : ___)
Onduleur : ___ kVA (NUT : oui/non, testé le ___)
Groupe : ___ kVA (testé le ___)
Supervision : ___  Alertes : ___
Contrat : ___  Échéance : ___
Dossier d'exploitation : ☐ à jour le ___
```

---

## 76. KPI — piloter le service virtualisation

- **Disponibilité** des VM critiques (objectif 99,9 %).
- **RPO/RTO** réels (mesurés sur le dernier test).
- **Sauvegardes** : % succès, dernier test de restauration.
- **Capacité** : % RAM/CPU/stockage par nœud (alerte à 80 %).
- **Ceph** : HEALTH_OK permanent (tout WARN = incident).
- **Patchs** : délai de mise à jour après release (objectif < 30 j).
- **Rapport mensuel** : 1 page (dispo, incidents, capacité,
  backups, risques).

---

## 77. Contrats de maintenance — virtualisation

- **Périmètre** : hyperviseurs, Ceph, PBS, réseau, onduleur+NUT.
- **Niveaux** : Bronze (1 visite/an), Silver (trimestriel +
  supervision), Gold (mensuel + astreinte 4 h + pièces).
- **Inclure** : test de restauration annuel **contractuel**,
  exercice NUT annuel, rapport de capacité semestriel.
- **Chiffrage** : visites + astreinte + pièces (disques de
  rechange !) + marge.

---

## 78. Gestion d'équipe — virtualisation

- **1 référent Proxmox** (cluster, Ceph, PBS) + techniciens
  (VM courantes, backups, tickets).
- **Règles** : pas de clic manuel répétitif (Ansible/Terraform),
  toute VM a une fiche, tout changement = ticket GLPI.
- **Astreinte** : avec runbook (fiches réflexe §70 + alertes).
- **Formation** : 1 admin formé « Ceph » minimum (c'est le
  composant le plus délicat).

---

## 79. Script d'audit mensuel Proxmox

```bash
#!/bin/bash
# /usr/local/bin/audit-pve.sh
echo "=== AUDIT PVE $(date +%F) ==="
pvecm status | grep -A5 "Quorum"
echo "[CEPH]"; ceph -s | head -12
echo "[ZFS]"; zpool list -H -o name,cap,health
echo "[STOCKAGE]"; pvesm status
echo "[VM]"; qm list | tail -n +2 | wc -l; echo "conteneurs :"; pct list | tail -n +2 | wc -l
echo "[HA]"; ha-manager status 2>/dev/null | head -5
echo "[BACKUPS]"; ls -la /var/log/vzdump* 2>/dev/null | tail -3
echo "[MAJ]"; apt list --upgradable 2>/dev/null | grep -c pve
echo "[NUT]"; upsc monups 2>/dev/null | grep -E "battery.charge|ups.load|battery.runtime" || echo "NUT non configuré"
```
- À lancer via Ansible sur tous les nœuds, sortie centralisée.

---

## 80. Mémo commandes — tout en une page

```bash
# Cluster
pvecm status | pvecm nodes | pvecm add IP | pvecm delnode nom
# VM
qm list | qm start|stop|shutdown|reset|destroy 100
qm snapshot|rollback|delsnapshot 100 nom
qm clone 100 200 --name x --full
qm migrate 100 pve02 --online
qm resize 100 scsi0 +20G | qm move-disk 100 scsi0 stockage
qm set 100 --memory 4096 --cores 2
qm template 100
# Conteneurs
pct list | pct start|stop|shutdown 300 | pct enter 300
pct snapshot|rollback 300 nom
# Stockage
pvesm status | pvesm add nfs ...
zpool status -x | zfs list -t snapshot
ceph -s | ceph osd tree | ceph df
# Backup
vzdump 100 --mode snapshot --compress zstd --storage pbs
qmrestore /chemin/vzdump... 200
# Divers
pve-firewall status | ha-manager status
pvesh get /nodes/pve01/status
upsc monups | upsmon -c fsd   # NUT : test d'arrêt
```
---

## 81. Checklists imprimables

### 81.1 Installation d'un nœud
```
□ Matériel vérifié (VT-x, disques, RAM, 2 cartes réseau)
□ BIOS : virtualisation, IOMMU, boot UEFI, power-on AC
□ ISO vérifiée (SHA256), installée (ZFS mirror si 2 disques)
□ IP fixe, FQDN, DNS, NTP
□ Dépôts configurés (enterprise/no-subscription)
□ apt full-upgrade, reboot
□ Stockage vérifié, réseau (vmbr0, bond si prévu)
□ Alertes email, certificat ACME
□ Utilisateurs (pas de root partagé), 2FA
□ Pare-feu datacenter
□ Intégré au cluster (si prévu)
□ PBS : stockage ajouté, job de backup créé
□ NUT (si onduleur)
□ Fiche nœud remplie (§74)
```

### 81.2 Maintenance mensuelle (par nœud)
```
□ pvecm status (quorum), ceph -s (HEALTH_OK), zpool status -x
□ Espace : pvesm status (< 80 %), PBS datastore (< 85 %)
□ Backups : 100 % succès sur 30 j, 1 restauration testée
□ Mises à jour : fenêtre planifiée si nécessaire
□ Logs : erreurs (journalctl -p err --since "30 days ago" | head)
□ Températures, SMART (smartctl -a, usure SSD)
□ NUT : upsc (charge %, autonomie)
□ Rapport mensuel (§76)
```

---

## 82. Cas pratiques supplémentaires

**Cas F — Corosync qui flappe après ajout d'un switch**
Le nouveau switch a le spanning-tree lent → corosync perd des
paquets → nœuds qui s'excluent. → Portfast sur les ports Proxmox,
ou réseau cluster dédié. Leçon : le réseau du cluster est sacré.

**Cas G — Restauration d'un fichier effacé par un utilisateur**
PBS → File Restore → le fichier est de retour en 5 min, sans
arrêter la VM. Le cas qui justifie PBS à lui seul.

**Cas H — Montée de version Proxmox 8 → 9**
Un nœud à la fois : migrer les VM, `apt full-upgrade`, reboot,
vérifier (cluster, Ceph, HA), passer au suivant. Jamais les 3
d'un coup.

**Cas I — Panne de courant sans NUT (leçon apprise)**
Coupure 2 h, les nœuds s'arrêtent brutalement, 1 VM corrompue
(disque). → NUT installé la semaine suivante + test trimestriel.
Ne pas attendre la leçon pour agir.

**Cas J — Espace Ceph à 90 %**
Alertes ignorées 2 semaines → Ceph passe en read-only → VM
figées. → Nettoyage d'urgence (vieux snapshots, backups),
ajout d'OSD. Règle : alerte à 75 %, action à 80 %, jamais 85 %.

---

## 83. Proxmox + énergie — dimensionner l'électricité

```
Par nœud (16 cœurs, 128 Go, 4 SSD) : ~300–500 W en charge
3 nœuds + switch + PBS : ~1,5–2 kW
→ Onduleur : 3 kVA minimum (10 min), 6–10 kVA conseillé
→ Groupe : si site critique (voir guide onduleurs)
Prise : circuit dédié 16 A par baie, PDU avec mesure
Sonde température : alerte à 30 °C dans la baie
```
- Le volet **énergies** et le volet **systèmes** se rejoignent ici :
  un cluster sans onduleur, c'est une épave en attente de coupure.

---

## 84. Sécurité physique

- Baie **verrouillée**, accès badgé, registre des accès.
- USB désactivés sur les nœuds (ou BIOS protégé par mot de passe).
- Caméra sur le local technique (les « interventions » non
  déclarées…).
- Extincteur CO₂, détection incendie (VESDA sur gros sites).

---

## 85. Nouveautés et tendances (2024-2026)

- **SDN** : remplace progressivement les bridges manuels.
- **Import OVF/OVA** natif (migration VMware facilitée).
- **Ceph Reef/Quincy** : meilleures perfs, moins de RAM.
- **PBS** : tape backup, namespaces — la maturité entreprise.
- **Terraform/OpenTofu** : l'infra as code devient la norme.
- **ARM** : Proxmox expérimental sur ARM (à surveiller, pas en prod).

---

## 86. Ressources officielles

- Documentation : pve.proxmox.com (excellente, en anglais).
- Forum : forum.proxmox.com (très actif).
- Wiki : pve.proxmox.com/wiki.
- Modèles de référence : les « Proxmox VE Administration Guide » (PDF).

---

## 87. Formation équipe — plan 4 semaines

- **S1** : concepts (KVM/LXC), UI, créer une VM/CT, snapshots.
- **S2** : stockage (ZFS, NFS), réseau (bridges, VLAN), backups vzdump.
- **S3** : cluster, HA, Ceph (bases), PBS, NUT.
- **S4** : dépannage (cas §46), Terraform/Ansible (bases), astreinte.
- Validation : chaque technicien **installe un cluster 3 nœuds en lab**
  et restaure une VM depuis PBS.

---

## 88. Budget type annuel (3 nœuds, PME)

| Poste | Ordre de grandeur |
|---|---|
| Abonnements Enterprise (3 CPU) | ~300 €/an |
| Disques de rechange (stock) | 1–2 SSD/an |
| PBS (matériel ou VM + disques) | amorti 3 ans |
| Électricité (2 kW) | selon tarif local |
| Maintenance (visites + astreinte) | selon contrat |
| Formation | 1 session/an |

- Le coût est **10× inférieur** à VMware pour un périmètre équivalent.

---

## 89. Erreurs Ceph — à ne jamais faire

```
✗ Rebooter 2 nœuds en même temps
✗ Remplir au-delà de 85 %
✗ Paniquer pendant un backfill (c'est normal, ça prend des heures)
✗ Mélanger SSD et HDD dans le même pool sans classes
✗ Oublier le réseau dédié (1 GbE partagé = lenteurs)
✗ Supprimer un pool sans vérifier les images (rbd ls)
✗ Ignorer un HEALTH_WARN (ça ne se répare pas tout seul)
```

---

## 90. Les 10 règles d'or Proxmox (à afficher)

```
1. Jamais de service sur l'hôte.
2. Templates + cloud-init, pas d'install manuelle.
3. Snapshots avant chaque changement.
4. Backups PBS quotidiens + test mensuel.
5. Quorum impair, Ceph < 85 %, ZFS < 80 %.
6. 1 nœud à la fois pour les mises à jour.
7. NUT configuré ET testé.
8. 8006 jamais exposé, 2FA admins.
9. Tout documenté (fiche par VM).
10. Le disque déconnecté mensuel sauve des entreprises.
```
---

## 91. Exemple complet — monter un cluster 3 nœuds pas à pas

```
JOUR 1 — Matériel
  □ 3 serveurs identiques (16 cœurs, 64 Go, 2×480 SSD système + 2×2 To data)
  □ Switch 10 GbE (Ceph + VM), switch 1 GbE (management)
  □ Câblage : 2× 10 GbE + 1× 1 GbE par nœud, étiqueté
  □ Onduleur 6 kVA branché, PDU

JOUR 2 — Installation
  □ Proxmox sur les 3 (ZFS mirror sur les 480 Go)
  □ IP : pve01 .11, pve02 .12, pve03 .13, FQDN
  □ Dépôts enterprise, full-upgrade, reboot

JOUR 3 — Réseau
  □ Bond LACP 10 GbE → vmbr0 (VLAN aware), vmbr1 (Ceph, 10.10.10.0/24)
  □ Test : iperf3 entre nœuds (> 9 Gb/s), débrancher un câble (pas de coupure)

JOUR 4 — Cluster
  □ pve01 : créer le cluster ; pve02/03 : rejoindre
  □ pvecm status : 3 votes, quorum OK
  □ Pare-feu datacenter, 2FA, ACME, alertes email

JOUR 5 — Ceph
  □ Installer Ceph, réseau public (vmbr0) / cluster (vmbr1)
  □ Créer les OSD (les 2×2 To de chaque nœud)
  □ Pools : vm-pool (rbd), cephfs si besoin
  □ ceph -s : HEALTH_OK, 6 OSD up

JOUR 6 — PBS
  □ 4e machine (ou VM) : PBS, datastore 4 To
  □ Stockage PBS ajouté sur le cluster, chiffrement
  □ Job quotidien 02:00, Verify hebdo, GC hebdo

JOUR 7 — Production
  □ Template Ubuntu + cloud-init, 1re VM de test
  □ Backup → restauration testée
  □ HA sur la VM test, test de bascule (débrancher pve01 !)
  □ NUT : onduleur USB sur pve01, clients sur pve02/03, test
  □ Fiches remplies, dossier d'exploitation, formation équipe
```

---

## 92. Ceph pas à pas — les commandes

```bash
# Sur pve01 : installer et initier
pveceph install
pveceph init --network 10.10.10.0/24
# Monitors (1 par nœud)
pveceph mon create
# Manager
pveceph mgr create
# OSD (répéter sur chaque nœud/disque)
pveceph osd create /dev/sdb
# Pools
pveceph pool create vm-pool --size 3 --min_size 2
# Vérifier
ceph -s
ceph osd df
# Ajouter au stockage Proxmox : Datacenter → Stockage → RBD
```

---

## 93. HA pas à pas

```
1. Stockage partagé vérifié (Ceph/RBD)
2. Fencing : IPMI configuré sur chaque nœud (Datacenter → HA → Fencing)
3. Datacenter → HA → Groupes : créer "prod" (pve01,pve02,pve03)
4. HA → Ajouter : VM 100 → groupe prod, état started, max_restart 3
5. TEST (fenêtre planifiée) : 
   a. ha-manager status → tout au vert
   b. Éteindre brutalement pve01 (pas un shutdown propre !)
   c. Chronométrer : la VM redémarre sur pve02 en ~2-5 min
   d. Rallumer pve01, vérifier le retour
6. Documenter le RTO mesuré
```

---

## 94. Template cloud-init avancé (user-data)

```yaml
# /var/lib/vz/snippets/user.yaml
#cloud-config
hostname: vm-template
manage_etc_hosts: true
users:
  - name: admin
    sudo: ALL=(ALL) NOPASSWD:ALL
    ssh_authorized_keys:
      - ssh-ed25519 AAAA... admin@poste
packages:
  - qemu-guest-agent
  - zabbix-agent2
  - fail2ban
  - unattended-upgrades
runcmd:
  - [systemctl, enable, --now, qemu-guest-agent]
  - [systemctl, enable, --now, zabbix-agent2]
timezone: Africa/Abidjan
```
```bash
qm set 9000 --cicustom "user=local:snippets/user.yaml"
qm template 9000
```

---

## 95. GLPI + Proxmox

- Fiche par VM dans GLPI (import via API ou manuel).
- Tickets : toute intervention Proxmox = ticket (traçabilité).
- FusionInventory : inventaire auto des VM.

---

## 96. Zabbix — superviser Proxmox

```
1. Template "Proxmox VE" (API) sur le cluster :
   - état nœuds, quorum, Ceph, espace stockages
2. Agent Zabbix dans chaque VM (métier)
3. Triggers :
   - Ceph != HEALTH_OK → Disaster
   - Backup failed → High
   - Stockage > 80 % → Warning
   - Nœud unreachable → Disaster
4. Actions : SMS/Telegram à l'astreinte
```

---

## 97. Plan de reprise d'activité (PRA) — trame

```
1. PÉRIMÈTRE : cluster prod (VM critiques : ___, ___, ___)
2. RPO : ___ (fréquence backups) / RTO : ___ (mesuré : ___)
3. SCÉNARIOS :
   a. Panne 1 nœud → HA auto (RTO ~5 min)
   b. Panne 2 nœuds → mode dégradé (VM critiques sur le survivant)
   c. Site perdu → PBS distant → nouveau cluster (RTO 1 jour)
   d. Ransomware → disque déconnecté mensuel
4. CONTACTS : astreinte ___, fournisseur ___, onduleur ___
5. EXERCICE ANNUEL : date ___, scénario ___, RTO mesuré ___
6. RETEX : rapport 48 h après chaque exercice/incident
```

---

## 98. Annexe — ports réseau Proxmox

| Port | Usage |
|---|---|
| 8006/tcp | Interface web |
| 5404-5405/udp | corosync (cluster) |
| 22/tcp | SSH / migration |
| 3128/tcp | Proxy SPICE |
| 5900-5999/tcp | Console VNC |
| 6789, 6800-7300/tcp | Ceph (mon, OSD) |
| 8007/tcp | PBS |
| 111, 2049/tcp | NFS (si utilisé) |
| 3260/tcp | iSCSI |

---

*Fin du guide — 2000+ lignes. Proxmox n'a plus de secret pour toi,
Zelef : **cluster propre, Ceph sain, PBS testé, NUT branché.**
Ton outil préféré, maîtrisé de bout en bout.*
---

## 99. Dépannage réseau du cluster — approfondi

```bash
# Le cluster est sensible à la latence : diagnostiquer
ping -c 100 pve02 | tail -3        # perte ? gigue ?
corosync-cfgtool -s                # anneaux corosync
journalctl -u corosync --since "1 hour ago" | grep -i "failed\|error\|knet"
ss -ulpn | grep 5404               # ports corosync en écoute
ethtool eth0 | grep -i speed       # négociation correcte ?
```
- Symptômes typiques : nœuds qui « flappent » (rejoignent/quittent),
  quorum instable, migrations qui échouent.
- Causes n°1 : **switch saturé**, spanning-tree, MTU incohérent,
  câble défectueux (tester avec un autre câble !).
- Solution durable : **réseau corosync dédié** (VLAN ou physique).

---

## 100. Sauvegarde des configurations — le git du cluster

```bash
# Ce qui n'est PAS dans les backups VM :
# /etc/pve (configs cluster — répliqué, mais pas versionné)
# /etc/network/interfaces, /etc/ceph, /etc/nut, /etc/zfs
# → versionner :
cd /etc/pve && git init -b main
git add -A && git commit -m "config $(date +%F)"
git remote add origin git@interne:infra/pve-config.git
git push -u origin main
# + script mensuel : commit auto des changements
```
- Après un sinistre, **reconstruire à l'identique** = ces fichiers.

---

## 101. Ajouter un nœud à un cluster existant

```
1. Même version Proxmox (pveversion -v identique)
2. Même hostname/IP définitifs, NTP synchronisé
3. Depuis le nouveau : pvecm add IP_D_UN_NŒUD (mot de passe root)
4. Vérifier : pvecm status (4 votes → nombre pair ! → qdevice ou 5e nœud)
5. Recréer : stockages locaux, réseau (mêmes noms de bridges !),
   Ceph OSD si applicable
6. Migrer quelques VM de test, vérifier HA
```
- ⚠️ Passer de 3 à 4 nœuds = nombre **pair** → prévoir qdevice ou
  5e nœud pour garder un quorum sain.

---

## 102. Cas d'usage par métier

| Métier | Architecture Proxmox |
|---|---|
| Bureautique PME | 2 nœuds, ZFS, réplication, PBS sur NAS |
| Data center | 3+ nœuds, Ceph, HA, PBS dédié + sync |
| Hébergeur | SDN, Terraform, facturation par VM |
| Industrie | 2 nœuds durcis, NUT, arrêt auto, baie verrouillée |
| Éducation | Templates + clones liés (salles TP en 5 min) |
| Lab / test | 1 nœud, ZFS, snapshots à gogo |

---

## 103. Checklist d'audit annuel Proxmox

```
□ Versions : tous les nœuds à jour (même version)
□ Quorum : impair ou qdevice, test de panne simulée
□ Ceph : HEALTH_OK, < 80 %, scrub OK, OSD tous up
□ ZFS : < 80 %, scrub mensuel actif, snapshots nettoyés
□ Backups : 100 % succès 12 mois, 12 restaurations testées (1/mois)
□ PRA : exercice annuel fait, RTO mesuré, RETEX écrit
□ NUT : test de coupure annuel, seuils vérifiés
□ Sécurité : 2FA, comptes (départs ?), pare-feu, 8006 non exposé
□ Capacité : projection 12 mois (faut-il un nœud ?)
□ Documentation : fiches à jour, git /etc/pve poussé
□ Contrats : abonnements, garanties matérielles
□ Stock : disques de rechange, câbles, PDU
```

---

## 104. Index rapide

```
UI : https://IP:8006 | qm / pct / pvesh / pvecm
Cluster : corosync 5404-5405/udp, quorum impair
Stockage partagé = HA possible | Ceph : 3 nœuds, 10 GbE, < 85 %
ZFS : mirror/raidz, lz4, scrub mensuel, < 80 %
Backup : PBS > vzdump, verify hebdo, GC hebdo, sync distant
Test restauration 1×/mois | Disque déconnecté 1×/mois
NUT : upsc, test trimestriel | HA : fencing obligatoire
Mises à jour : 1 nœud à la fois | Jamais de service sur l'hôte
```

---

*Fin du guide — 2000+ lignes. Ton coup de cœur mérite cette
exhaustivité, Zelef. **À toi de jouer sur le terrain.***
---

## 105. Proxmox vs alternatives — trancher

| Critère | Proxmox VE | VMware vSphere | Hyper-V | XCP-ng |
|---|---|---|---|---|
| Licence | Gratuit (opt. ~100 €/CPU/an) | $$$$ (par cœur) | Inclus Windows Server | Gratuit |
| KVM/LXC | Oui | Non (ESXi) | Non | Oui (Xen) |
| Ceph intégré | Oui | vSAN ($$$) | Non (S2D) | Non |
| Backup intégré | Oui (PBS) | Veeam ($$$) | DPM | Xen Orchestra |
| Cluster/HA | Oui | Oui | Oui | Oui |
| Courbe d'apprentissage | Moyenne | Moyenne | Faible (si Windows) | Moyenne |
| Support francophone | Communauté | Commercial | Commercial | Communauté |

- **Verdict PME/ETI** : Proxmox = 90 % des fonctionnalités pour
  5 % du prix. Le choix rationnel quand on maîtrise Linux.

---

## 106. Les coûts cachés — les anticiper

```
□ Réseau 10 GbE (switch + cartes + DAC/fibre) : le poste n°1
□ Disques SSD d'entreprise (endurance !) — pas de SSD grand public en Ceph
□ Onduleur + groupe (voir guide onduleurs)
□ Climatisation du local
□ Formation de l'équipe (le vrai investissement)
□ Temps : Ceph et le cluster demandent de la montée en compétence
```
- Le logiciel est gratuit ; **l'infrastructure autour ne l'est pas**.
  Chiffrez tout avant de promettre.

---

## 107. Feuille de route d'adoption — 6 mois

```
MOIS 1 : Lab (1 nœud ou 3 VM Proxmox imbriquées), formation S1-S2
MOIS 2 : Maquette 2 nœuds + ZFS + réplication + PBS
MOIS 3 : Pilote (2-3 VM non critiques en prod), NUT
MOIS 4 : Cluster 3 nœux + Ceph (si pertinent), HA testé
MOIS 5 : Migration des VM critiques, Terraform/Ansible
MOIS 6 : PRA écrit, exercice annuel, dossier d'exploitation complet
```
- Ne jamais tout migrer d'un coup : **le pilote d'abord**.

---

## 108. FAQ — les questions qu'on me pose

**Q : 1 seul nœud, ça vaut le coup ?**
R : Oui (ZFS + snapshots + PBS). Ce n'est pas de la HA, mais c'est
déjà 10× mieux qu'un serveur « à l'ancienne ».

**Q : Ceph ou ZFS ?**
R : 1-2 nœuds → ZFS + réplication. 3+ nœuds + HA → Ceph.

**Q : Combien de VM par nœud ?**
R : Compter la RAM d'abord (ne pas dépasser 80 %), puis le CPU
(ratio 2-4:1), puis les IOPS disque.

**Q : Proxmox et Windows, ça marche bien ?**
R : Oui, avec VirtIO + guest tools. Des milliers de PME tournent
ainsi (contrôleur de domaine, ERP…).

**Q : Et la sauvegarde des VM Windows (VSS) ?**
R : QEMU agent + VSS → snapshots cohérents applicativement.

**Q : Faut-il l'abonnement Enterprise ?**
R : En prod : oui (dépôts stables + support). C'est le prix d'un
repas par CPU et par an.

**Q : Proxmox sur Raspberry Pi ?**
R : Non (x86_64 uniquement en stable). Pour l'ARM : à surveiller.

**Q : Peut-on mixer des nœuds de générations différentes ?**
R : Oui si même version Proxmox, mais les perfs suivent le plus
faible (surtout Ceph).

---

*2000+ lignes — guide terminé. Ton outil préféré n'a plus de
secret, Zelef.*
---

## 109. Recettes prêtes à l'emploi — scripts

### 109.1 Health check quotidien (cron)
```bash
#!/bin/bash
# /usr/local/bin/pve-health.sh — rapport mail quotidien
DEST="admin@example.com"
{
echo "=== $(hostname) $(date) ==="
echo "--- Cluster ---"; pvecm status 2>&1 | grep -E "Quorum|Nodes"
echo "--- Ceph ---"; ceph -s 2>&1 | head -8
echo "--- ZFS ---"; zpool list -H -o name,cap,health 2>&1
echo "--- Stockages ---"; pvesm status 2>&1 | awk '{print $1, $5}'
echo "--- Backups 24h ---"; find /var/log/vzdump* -mtime -1 2>/dev/null | wc -l
echo "--- Services en échec ---"; systemctl list-units --failed --no-legend
echo "--- NUT ---"; upsc monups 2>/dev/null | grep -E "battery.charge|ups.status" || echo "NUT: non configuré"
} | mail -s "[PVE] Santé $(hostname) $(date +%F)" "$DEST"
```

### 109.2 Snapshot pré-maintenance (toutes les VM)
```bash
#!/bin/bash
# /usr/local/bin/snap-avant-maj.sh
TAG="avant-maj-$(date +%F)"
for vm in $(qm list | awk 'NR>1{print $1}'); do
  echo "Snapshot $vm..."
  qm snapshot "$vm" "$TAG" --description "Avant maintenance mensuelle" || echo "ÉCHEC $vm"
done
for ct in $(pct list | awk 'NR>1{print $1}'); do
  echo "Snapshot CT $ct..."
  pct snapshot "$ct" "$TAG" || echo "ÉCHEC CT $ct"
done
echo "Terminé. Nettoyer dans 7 jours : qm delsnapshot <id> $TAG"
```

### 109.3 Nettoyage des vieux snapshots
```bash
#!/bin/bash
# Garder les snapshots < 30 jours (adapter)
SEUIL=$(date -d "30 days ago" +%s)
for vm in $(qm list | awk 'NR>1{print $1}'); do
  qm listsnapshot "$vm" 2>/dev/null | awk '/snapname/{print $2}' | while read -r s; do
    echo "À vérifier : $vm / $s"
  done
done
# Suppression manuelle après vérification (jamais en aveugle !)
```

---

*Fin du guide — 2000+ lignes. **Proxmox maîtrisé de bout en bout.***
---

## 110. Pour aller plus loin

- Guide **Ceph** approfondi (CRUSH maps, multisite, performances).
- Guide **Proxmox Backup Server** : déploiement, chiffrement, bandes.
- Guide **Terraform + Ansible** : infra as code complète.
- Guide **Kubernetes sur Proxmox** : Talos, CSI Ceph.
- Runbook **astreinte Proxmox** : fiches réflexe imprimables.

*2000+ lignes — fin.*

---

## 111. Le mot de la fin — pourquoi Proxmox gagne

```
Parce qu'il fait 90 % du travail de VMware pour 5 % du prix.
Parce que ZFS pardonne, Ceph encaisse, PBS restaure.
Parce qu'un cluster bien monté ne vous réveille jamais la nuit.
Parce que le jour où tout brûle, c'est le PRA — pas le
panneau — qui sauve l'entreprise.
Maîtrisez-le, documentez-le, testez-le.
Et il vous le rendra au centuple.
```

*2000+ lignes. Fin.*

*Objectif 2000 lignes atteint — à toi, chef.*

---

## 112. Annexe — versions et compatibilité

| Proxmox VE | Base Debian | Ceph | Kernel |
|---|---|---|---|
| 7.x | Debian 11 (bullseye) | Pacific/Quincy | 5.15 |
| 8.x | Debian 12 (bookworm) | Quincy/Reef | 6.8 |
| 9.x | Debian 13 (trixie) | Reef/Squid | 6.14+ |

- Toujours la même version sur tous les nœuds d'un cluster.
- Montée de version : un nœud à la fois, dans une fenêtre planifiée.
