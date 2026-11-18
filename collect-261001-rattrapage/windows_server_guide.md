# Windows Server en entreprise — Guide technique ultra-complet

> **Public :** Zelef, chef de service systèmes & énergies — administrateurs, techniciens senior.
> **Versions couvertes :** Windows Server 2019, 2022, 2025 (LTSC). Versions 2025 signalées quand le comportement diffère.
> **Ton :** direct, dense, pratique. Commandes PowerShell testables, tableaux de référence, checklists.
> **Avertissement :** les mots de passe et secrets dans ce guide sont des exemples fictifs (`P@ssw0rd-Exemple-2026!`). Remplace-les systématiquement.

---

## Table des matières

1. Éditions et canaux de distribution
2. Standard vs Datacenter : le tableau qui décide
3. Checklist de choix d'édition (10 critères)
4. Installation pas à pas (Desktop Experience)
5. Installation Server Core pas à pas
6. Post-installation : la checklist des 20 minutes
7. Server Core vs Desktop Experience : différences concrètes
8. sconfig : le couteau suisse de Server Core
9. Administration à distance : RSAT, Windows Admin Center, WinRM
10. Quand choisir Core, quand choisir Desktop
11. Rôles et fonctionnalités : concepts
12. Installation via Server Manager
13. Installation via PowerShell (Install-WindowsFeature)
14. Les 15 rôles courants et quand les installer
15. Hyper-V : architecture et prérequis
16. Commutateurs virtuels : externe, interne, privé
17. VM génération 1 vs génération 2
18. Création d'une VM pas à pas (PowerShell)
19. Mémoire dynamique, processeurs virtuels, disques
20. Checkpoints : standard vs production
21. Réplication Hyper-V (Hyper-V Replica)
22. Cluster de basculement + CSV
23. Bonnes pratiques Hyper-V (20 règles d'or)
24. WSUS : architecture et prérequis
25. Installation et configuration initiale de WSUS
26. Groupes d'ordinateurs et ciblage côté client
27. Approbations : manuelles et règles automatiques
28. Maintenance : Server Cleanup Wizard et scripts
29. Dépannage WSUS (clients qui ne remontent pas, reset base)
30. Partages réseau : SMB, création, bonnes pratiques
31. Permissions NTFS vs permissions de partage : la combinaison
32. Cas pratiques de permissions (tableau de vérité)
33. DFS-N : espaces de noms
34. DFS-R : réplication
35. FSRM : quotas et filtrage de fichiers
36. Déduplication des données
37. Dépannage d'accès aux fichiers (méthode en 7 étapes)
38. Serveur d'impression : rôle et architecture
39. Déploiement d'imprimantes et copieurs via GPO
40. Pilotes Type 3 vs Type 4, Print Management
41. Files d'attente, spooler : redémarrage et migration
42. Dépannage d'impression (lien métier copieurs)
43. RDS : les 4 rôles (Broker, Session Host, Passerelle, Accès Web)
44. Déploiement RDS pas à pas
45. Licences CAL RDS : par utilisateur vs par appareil
46. Dépannage RDS
47. AD CS / PKI : concepts (AC racine, subordonnée, modèles)
48. Installation d'une AC d'entreprise pas à pas
49. Modèles de certificats et auto-enrollment via GPO
50. Renouvellement et révocation (CRL, OCSP)
51. Dépannage PKI
52. Sauvegarde : Windows Server Backup
53. Planifications, bare metal, restauration
54. Stratégie 3-2-1 et rotation des supports
55. Restauration : scénarios pas à pas
56. Veeam et alternatives : quand passer au niveau supérieur
57. Licences : KMS vs ADBA
58. CAL utilisateur vs appareil, inventaire
59. Activation : slmgr, DISM, dépannage
60. Durcissement de base (hardening)
61. Pare-feu Windows Defender : règles et profils
62. Microsoft Defender Antivirus sur serveur
63. LAPS et gestion des mots de passe locaux
64. Audit, journaux et supervision minimale
65. Cas pratiques commentés (1 à 5)
66. Cas pratiques commentés (6 à 10)
67. Cas pratiques commentés (11 à 15)
68. Cas pratiques commentés (16 à 18)
69. Erreurs classiques (1 à 5)
70. Erreurs classiques (6 à 10)
71. Erreurs classiques (11 à 15)
72. Erreurs classiques (16 à 18)
73. Checklist d'exploitation quotidienne
74. Checklist hebdomadaire
75. Checklist mensuelle
76. Checklist trimestrielle / annuelle
77. Checklist de mise en production d'un nouveau serveur
78. Pense-bête de poche : PowerShell
79. Pense-bête de poche : commandes et raccourcis
80. Pense-bête de poche : ports réseau essentiels
81. Glossaire (A–M)
82. Glossaire (N–Z)
83. Quiz : 10 questions
84. Quiz : réponses commentées
85. Pour aller plus loin : 20 ressources et chantiers

---

## 1. Éditions et canaux de distribution

Windows Server existe en deux canaux :

| Canal | Versions | Support | Usage |
|---|---|---|---|
| **LTSC** (Long-Term Servicing Channel) | 2019, 2022, 2025 | 10 ans (5 mainstream + 5 étendu) | Production, charges stables, serveurs d'infrastructure |
| **Canaux annuels** (anciennement SAC) | ex. 23H2, 25H2 | 24 mois | Conteneurs, dev/test, environnements agiles |

**Règle d'or en entreprise : toujours LTSC pour la production.** Les versions annuelles servent aux conteneurs et aux bancs d'essai. Windows Server 2025 est la LTSC actuelle ; 2022 reste le choix le plus éprouvé en 2026 ; 2019 entre en support étendu (correctifs de sécurité uniquement).

Éditions principales (LTSC) :

| Édition | Cible | Différence clé |
|---|---|---|
| **Standard** | PME, peu de virtualisation | 2 VM (OSE) incluses par licence |
| **Datacenter** | Virtualisation intensive | VM illimitées |
| **Essentials** | ≤ 25 utilisateurs / 50 appareils | Rôles limités, pas de virtualisation (2019 = dernière vraie Essentials) |

> Note 2025 : l'édition Essentials n'existe plus en 2025. Les petites structures prennent Standard.

---

## 2. Standard vs Datacenter : le tableau qui décide

| Critère | Standard | Datacenter |
|---|---|---|
| VM (OSE) par licence | **2** | **Illimitées** |
| Conteneurs Hyper-V illimités | Non | Oui |
| Storage Spaces Direct (S2D) | Non | Oui |
| Réplica de stockage (Storage Replica) | Limité (1 partenariat, 1 volume ≤ 2 To) | Illimité |
| Réseau SDN complet (Network Controller) | Non | Oui |
| Machines virtuelles protégées (Shielded VM) | Non (hôte uniquement) | Oui |
| Prix (ordre de grandeur) | ~1 000 € / 16 cœurs | ~6 000 € / 16 cœurs |

**Calcul de rentabilité :** si un hôte Hyper-V fait tourner plus de ~6 VM en production durable, Datacenter est généralement moins cher que d'empiler des licences Standard (chaque licence Standard = 2 VM ; il faut re-licencier par tranches de 2 VM).

```powershell
# Vérifier l'édition installée
Get-ComputerInfo | Select-Object WindowsProductName, WindowsEditionId, WindowsVersion

# Changer d'édition (montée en gamme Standard -> Datacenter, sans réinstallation)
DISM /Online /Set-Edition:ServerDatacenter /ProductKey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX /AcceptEula
```

> La montée en gamme Standard → Datacenter fonctionne. La descente Datacenter → Standard est **impossible** sans réinstallation.

---

## 3. Checklist de choix d'édition (10 critères)

1. [ ] Nombre de VM prévues par hôte sur 5 ans → > 6 = Datacenter
2. [ ] Storage Spaces Direct prévu ? → Datacenter obligatoire
3. [ ] Réplication de stockage multi-sites ? → Datacenter (au-delà des limites Standard)
4. [ ] SDN / Network Controller ? → Datacenter
5. [ ] Budget licences sur 5 ans (comparer 3× Standard vs 1× Datacenter)
6. [ ] Modèle de licence : 16 cœurs minimum par serveur, par tranches de 2/16 cœurs
7. [ ] CAL : prévoir CAL Utilisateur ou Appareil pour chaque accès (voir §58)
8. [ ] Canal LTSC confirmé (pas de version annuelle en prod)
9. [ ] Compatibilité applicative : l'éditeur supporte-t-il 2025 ? (sinon 2022)
10. [ ] Stratégie de sortie : 2019 → planifier la migration (support étendu = sécurité seule)

---

## 4. Installation pas à pas (Desktop Experience)

1. Démarrer sur l'ISO (ou clé USB bootable via Rufus / Media Creation).
2. Langue, heure/devise, clavier → **Suivant** → **Installer maintenant**.
3. Choisir l'édition **avec Expérience de bureau** (Desktop Experience) si tu veux une GUI.
4. Type d'installation : **Personnalisé** (jamais de mise à niveau sur un serveur de prod).
5. Partitionnement : supprimer/recréer si disque neuf. Laisser 100+ Go pour le système.
6. Mot de passe Administrateur local (fictif ici) : `Adm1n-L0cal-Exemple!` → **Terminer**.
7. Premier boot : `Ctrl+Alt+Suppr`, connexion Administrateur.
8. Server Manager se lance → passer à la checklist §6.

```powershell
# En une fois après le premier boot (à adapter) :
Rename-Computer -NewName "SRV-FICHIER-01" -Restart
```

> Astuce : note le nommage normalisé dès l'installation : `SRV-<ROLE>-<NN>` (ex. `SRV-HV-01`, `SRV-AD-01`, `SRV-PRINT-01`). Un nommage propre évite 50 % des confusions d'exploitation.

---

## 5. Installation Server Core pas à pas

Même procédure qu'au §4, mais choisir l'édition **sans** mention "Expérience de bureau" (ex. `Windows Server 2025 Standard` tout court).

Au premier démarrage : une fenêtre `cmd` + `sconfig` (voir §8). Pas de menu Démarrer, pas d'Explorateur.

```powershell
# Depuis la console Core, ouvrir PowerShell :
powershell

# Vérifier qu'on est bien en Core :
Get-ComputerInfo | Select-Object WindowsInstallationType
# Résultat attendu : Server Core
```

**Pièges fréquents à l'installation :**
- Oublier le mot de passe Administrateur → pas de récupération simple, réinstaller.
- Choisir la mauvaise édition (Core vs Desktop) → réinstallation obligatoire (pas de bascule GUI ↔ Core depuis 2016).
- Pilotes RAID/NVMe manquants → charger via **Charger un pilote** pendant l'installation.

---

## 6. Post-installation : la checklist des 20 minutes

À faire sur **chaque** serveur, dans l'ordre :

```powershell
# 1. Nom + domaine/groupe de travail
Rename-Computer -NewName "SRV-ROLE-01"

# 2. IP statique (jamais de DHCP sur un serveur d'infra)
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress "192.168.10.11" `
  -PrefixLength 24 -DefaultGateway "192.168.10.1"
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses "192.168.10.10","192.168.10.12"

# 3. Fuseau horaire + NTP
Set-TimeZone -Id "Romance Standard Time"   # France métropolitaine
w32tm /config /manualpeerlist:"192.168.10.10" /syncfromflags:manual /reliable:yes /update

# 4. Mises à jour
Install-Module PSWindowsUpdate -Force   # une fois
Get-WindowsUpdate -Install -AcceptAll -AutoReboot

# 5. Activer l'administration à distance
Enable-PSRemoting -Force
Set-NetFirewallRule -DisplayGroup "Gestion à distance de Windows" -Enabled True

# 6. Renommer + joindre au domaine (redémarrage requis)
Add-Computer -DomainName "entreprise.lan" -Credential (Get-Credential) -Restart
```

Checklist manuelle :
- [ ] IP statique + DNS corrects, passerelle OK (`Test-NetConnection 8.8.8.8`)
- [ ] Nom conforme à la convention
- [ ] Fuseau horaire + heure synchronisée (`w32tm /query /status`)
- [ ] Mises à jour installées
- [ ] Pare-feu actif avec profil Domaine
- [ ] RDP activé si besoin (voir §9)
- [ ] Antivirus / Defender à jour
- [ ] Sauvegarde planifiée (voir §52)
- [ ] Serveur documenté (CMDB/GLPI : rôle, IP, garanties)

---

## 7. Server Core vs Desktop Experience : différences concrètes

| Point | Server Core | Desktop Experience |
|---|---|---|
| Interface | Console texte + sconfig | GUI complète |
| Empreinte disque | ~6–9 Go | ~12–20 Go |
| Surface d'attaque | Réduite (~40 % de correctifs en moins) | Complète |
| Redémarrages (patchs) | Moins fréquents | Plus fréquents |
| Outils d'admin | PowerShell, sconfig, RSAT, WAC | + Server Manager, MMC locales |
| Cas d'usage | Hyper-V, AD, DNS, DHCP, fichiers | RDS Session Host, applis à GUI, transition |

**Depuis Windows Server 2016 : pas de bascule** entre Core et Desktop sans réinstallation. Le choix est définitif à l'installation.

---

## 8. sconfig : le couteau suisse de Server Core

Tape `sconfig` dans la console. Menu numéroté :

| N° | Action |
|---|---|
| 1 | Joindre au domaine / groupe de travail |
| 2 | Changer le nom de l'ordinateur |
| 3 | Ajouter un compte administrateur local |
| 4 | Configurer l'administration à distance (WinRM) |
| 5 | Paramètres Windows Update (Auto/Manuel/Désactivé) |
| 6 | Télécharger et installer les mises à jour |
| 7 | Activer/désactiver le Bureau à distance |
| 8 | Configurer la carte réseau (IP statique/DHCP, DNS) |
| 9 | Date et heure |
| 10 | Télémétrie |
| 11 | Paramètres Windows Update avancés |
| 13 | Redémarrer / arrêter |

Équivalents PowerShell directs (plus rapides en série) :

```powershell
# Tout ce que fait sconfig, en PowerShell :
sconfig                                  # lancer le menu
# ou en direct :
Set-NetIPInterface -InterfaceAlias "Ethernet" -Dhcp Disabled   # passer en statique via New-NetIPAddress
Enable-PSRemoting -Force                 # = option 4
cscript C:\Windows\System32\slmgr.vbs    # activation (option 11/12 selon version)
shutdown /r /t 0                         # = option 13
```

---

## 9. Administration à distance : RSAT, Windows Admin Center, WinRM

**Option A — RSAT (Remote Server Administration Tools)**, sur un poste Windows 10/11 :

```powershell
# Lister et installer RSAT (Windows 10 1809+ / 11 : fonctionnalités à la demande)
Get-WindowsCapability -Online | Where-Object Name -like 'Rsat*' | Select-Object Name, State
Add-WindowsCapability -Online -Name Rsat.ActiveDirectory.DS-LDS.Tools~~~~0.0.1.0
Add-WindowsCapability -Online -Name Rsat.DHCP.Tools~~~~0.0.1.0
Add-WindowsCapability -Online -Name Rsat.DNS.Tools~~~~0.0.1.0
Add-WindowsCapability -Online -Name Rsat.FileServices.Tools~~~~0.0.1.0
Add-WindowsCapability -Online -Name Rsat.Hyper-V.Tools~~~~0.0.1.0
```

**Option B — Windows Admin Center (WAC)** : portail web gratuit, idéal pour Server Core.

```powershell
# Installation silencieuse de WAC sur une machine d'administration
msiexec /i WindowsAdminCenter.msi /qn /L*v log.txt SME_PORT=443 SSL_CERTIFICATE_OPTION=generate
```

**Option C — WinRM / PowerShell Remoting** (la base de tout) :

```powershell
# Sur le serveur cible (une fois) :
Enable-PSRemoting -Force

# Depuis ta station :
Enter-PSSession -ComputerName SRV-HV-01 -Credential (Get-Credential)
Invoke-Command -ComputerName SRV-HV-01,SRV-HV-02 -ScriptBlock { Get-Service spooler }
```

> En 2025, privilégie **Windows Admin Center + PowerShell Remoting**. Le RDP permanent sur chaque serveur est un anti-pattern (sessions oubliées, consommation mémoire).

---

## 10. Quand choisir Core, quand choisir Desktop

**Choisir Server Core :**
- Hôtes Hyper-V (recommandation Microsoft officielle)
- Contrôleurs de domaine, DNS, DHCP
- Serveurs de fichiers (avec admin via WAC/RSAT)
- WSUS, serveurs d'impression

**Choisir Desktop Experience :**
- Hôtes de session RDS (les utilisateurs ont besoin d'une GUI complète)
- Applications métier qui exigent une interface graphique
- Serveur de transition / équipe junior en montée en compétence
- Outils fournisseurs sans équivalent ligne de commande (ex. certaines consoles de sauvegarde)

**Règle pratique :** par défaut Core ; Desktop uniquement sur justification écrite.

---

## 11. Rôles et fonctionnalités : concepts

- **Rôle** : une fonction serveur majeure (AD DS, Hyper-V, DNS, Serveur de fichiers, RDS...). Un rôle peut dépendre d'autres rôles.
- **Fonctionnalité** : un composant auxiliaire (.NET Framework, Windows Server Backup, SNMP, Telnet Client...).
- **Service de rôle** : sous-composant d'un rôle (ex. pour RDS : Broker, Session Host, Passerelle, Accès Web).

```powershell
# Lister tout ce qui est disponible
Get-WindowsFeature | Where-Object InstallState -eq 'Available' | Select-Object Name, DisplayName

# Lister ce qui est installé
Get-WindowsFeature | Where-Object Installed | Format-Table Name, DisplayName -AutoSize
```

---

## 12. Installation via Server Manager

1. **Gérer → Ajouter des rôles et fonctionnalités**.
2. Installation basée sur un rôle ou une fonctionnalité → serveur local ou distant.
3. Cocher le(s) rôle(s) → accepter les fonctionnalités dépendantes.
4. Options par rôle (ex. Hyper-V : créer le commutateur virtuel).
5. **Redémarrage automatique si nécessaire** : à cocher avec prudence en production.
6. Vérifier dans **Notifications** (drapeau) que l'installation a réussi.

> Server Manager est pratique pour découvrir, mais **tout déploiement reproductible passe par PowerShell** (§13) : script versionné, rejouable, auditable.

---

## 13. Installation via PowerShell (Install-WindowsFeature)

```powershell
# Installer un rôle simple
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart

# Installer plusieurs rôles/fonctionnalités d'un coup
Install-WindowsFeature -Name AD-Domain-Services, DNS, DHCP -IncludeManagementTools

# Avec les outils d'administration, sans redémarrage forcé
Install-WindowsFeature -Name File-Services, FS-FileServer -IncludeManagementTools

# Depuis un support d'installation (source alternative, utile en Core sans internet)
Install-WindowsFeature -Name Net-Framework-Core -Source D:\sources\sxs

# Désinstaller proprement
Uninstall-WindowsFeature -Name Telnet-Client -Restart:$false

# Vérifier + journal
Get-WindowsFeature -Name Hyper-V | Select-Object Installed, InstallState
```

**Noms de rôles à connaître par cœur :**

| Nom PowerShell | Rôle |
|---|---|
| `AD-Domain-Services` | AD DS |
| `DNS` | Serveur DNS |
| `DHCP` | Serveur DHCP |
| `Hyper-V` | Hyper-V |
| `Failover-Clustering` | Clustering de basculement |
| `FS-FileServer` | Serveur de fichiers |
| `FS-DFS-Namespace` / `FS-DFS-Replication` | DFS-N / DFS-R |
| `FS-Data-Deduplication` | Déduplication |
| `FS-Resource-Manager` | FSRM |
| `Print-Services`, `Print-Server` | Serveur d'impression |
| `Web-Server` | IIS |
| `Windows-Server-Backup` | Sauvegarde |
| `UpdateServices` | WSUS |
| `AD-Certificate` | AD CS (PKI) |
| `RDS-RD-Server` (+ sous-rôles) | RDS |

---

## 14. Les 15 rôles courants et quand les installer

| # | Rôle | Quand l'installer |
|---|---|---|
| 1 | AD DS | Contrôleur de domaine (toujours ≥ 2 par domaine) |
| 2 | DNS | Avec AD DS, ou DNS dédié |
| 3 | DHCP | Distribution d'adresses (redondance : basculement DHCP) |
| 4 | Hyper-V | Virtualisation |
| 5 | Clustering de basculement | Haute dispo Hyper-V / fichiers |
| 6 | Serveur de fichiers | Partages SMB, DFS |
| 7 | Déduplication | Volumes de fichiers avec doublons (jamais sur CSV ni disques système) |
| 8 | FSRM | Quotas, filtrage (bloquer .mp3, .exe...) |
| 9 | Serveur d'impression | Centraliser copieurs/imprimantes (§38) |
| 10 | WSUS | Patch management Windows |
| 11 | AD CS | PKI interne (certificats 802.1X, LDAPS, RDP...) |
| 12 | RDS | Bureaux / applis à distance |
| 13 | IIS | Applis web internes |
| 14 | Windows Server Backup | Sauvegarde locale d'appoint |
| 15 | WDS/MDT (Services de déploiement) | Déploiement d'images Windows (postes) |

---

## 15. Hyper-V : architecture et prérequis

Hyper-V est un hyperviseur **de type 1** (bare metal) : il s'installe sous Windows, qui devient lui-même une VM privilégiée (partition parente).

**Prérequis matériels :**
- CPU 64 bits avec **SLAT** (EPT/NPT) — tout CPU serveur depuis ~2012
- **Virtualisation matérielle activée dans le BIOS/UEFI** (Intel VT-x / AMD-V)
- DEP/NX activé
- RAM : 4 Go minimum pour l'hôte, bien plus en pratique

```powershell
# Vérifier la compatibilité AVANT d'installer
systeminfo | Select-String "Hyper-V"

# Détail via WMI
Get-CimInstance Win32_Processor | Select-Object Name, SecondLevelAddressTranslationExtensions,
  VirtualizationFirmwareEnabled, VMMonitorModeExtensions
# VirtualizationFirmwareEnabled doit être True (= activé dans le BIOS)
```

**Installation :**

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart
# Le redémarrage est OBLIGATOIRE (l'hyperviseur se charge au boot)
```

> Sur Windows Server 2025, Hyper-V prend en charge les GPU partitionnés (GPU-P) améliorés et les VM confidentielles de manière plus large. Les concepts ci-dessous restent identiques.

---

## 16. Commutateurs virtuels : externe, interne, privé

| Type | La VM parle au réseau physique ? | L'hôte parle aux VM ? | Usage typique |
|---|---|---|---|
| **Externe** | Oui (via une carte physique liée) | Oui (si "autoriser le partage") | VM de production |
| **Interne** | Non | Oui | Lab, management isolé |
| **Privé** | Non | Non | Lab totalement isolé, malware |

```powershell
# Lister les commutateurs
Get-VMSwitch | Format-Table Name, SwitchType, NetAdapterInterfaceDescription -AutoSize

# Créer un commutateur externe (lié à la carte physique "Ethernet")
New-VMSwitch -Name "vSwitch-Prod" -NetAdapterName "Ethernet" -AllowManagementOS $true

# Créer un commutateur interne
New-VMSwitch -Name "vSwitch-Lab" -SwitchType Internal

# Créer un commutateur privé
New-VMSwitch -Name "vSwitch-Isole" -SwitchType Private

# Identifier la carte physique à lier (si plusieurs)
Get-NetAdapter | Where-Object Status -eq 'Up' | Format-Table Name, InterfaceDescription, LinkSpeed
```

**Bonnes pratiques réseau Hyper-V :**
- Dédier au moins 1 carte physique (ou 1 team) au trafic VM, 1 au management hôte.
- **Ne jamais** mettre le trafic de cluster/CSV et le trafic VM sur le même vSwitch sans QoS.
- Activer le **trunk VLAN** si les VM sont sur plusieurs VLAN :

```powershell
# Port trunk sur une VM (VLAN 10-20 autorisés)
Set-VMNetworkAdapterVlan -VMName "SRV-WEB-01" -Trunk -AllowedVlanIdList "10-20" -NativeVlanId 10
# Port access simple
Set-VMNetworkAdapterVlan -VMName "SRV-AD-01" -Access -VlanId 10
```

---

## 17. VM génération 1 vs génération 2

| Critère | Génération 1 | Génération 2 |
|---|---|---|
| Firmware | BIOS | **UEFI** (Secure Boot) |
| Démarrage | IDE, PXE legacy | SCSI virtuel, PXE UEFI |
| OS supportés | Anciens (2008 R2, Linux legacy...) | 2012+ / 2016+, Linux récents |
| Secure Boot | Non | Oui |
| vTPM | Non | Oui (VM protégées) |
| Performances disque | Moindres (IDE émulé) | Meilleures (SCSI paravirtualisé) |

**Règle : toujours Génération 2** sauf besoin d'un OS antédiluvien. Le choix est **définitif** à la création.

---

## 18. Création d'une VM pas à pas (PowerShell)

```powershell
# 1. Créer la VM (génération 2, 4 Go RAM, nouveau VHDX de 100 Go)
New-VM -Name "SRV-AD-02" -MemoryStartupBytes 4GB -Generation 2 `
  -NewVHDPath "D:\VMs\SRV-AD-02\disque.vhdx" -NewVHDSizeBytes 100GB `
  -SwitchName "vSwitch-Prod" -Path "D:\VMs\SRV-AD-02"

# 2. CPU virtuels
Set-VMProcessor -VMName "SRV-AD-02" -Count 4

# 3. Désactiver le Secure Boot si l'OS ne le supporte pas (rare)
Set-VMFirmware -VMName "SRV-AD-02" -EnableSecureBoot Off

# 4. Monter l'ISO et démarrer
Add-VMDvdDrive -VMName "SRV-AD-02" -Path "D:\ISO\fr_windows_server_2022.iso"
Set-VMFirmware -VMName "SRV-AD-02" -FirstBootDevice (Get-VMDvdDrive -VMName "SRV-AD-02")
Start-VM -Name "SRV-AD-02"

# 5. Après l'install OS : démonter l'ISO
Set-VMFirmware -VMName "SRV-AD-02" -FirstBootDevice (Get-VMHardDiskDrive -VMName "SRV-AD-02")
Remove-VMDvdDrive -VMName "SRV-AD-02" -ControllerNumber 0 -ControllerLocation 1

# 6. Installer les services d'intégration (inclus depuis 2016, via Windows Update)
Get-VMIntegrationService -VMName "SRV-AD-02"
```

> Stocker les VM sur un volume dédié (jamais sur C:). Convention : `D:\VMs\<NomVM>\`.

---

## 19. Mémoire dynamique, processeurs virtuels, disques

**Mémoire dynamique :**

```powershell
# Activer la mémoire dynamique (min 512 Mo, max 8 Go, démarrage 2 Go)
Set-VMMemory -VMName "SRV-WEB-01" -DynamicMemoryEnabled $true `
  -MinimumBytes 512MB -StartupBytes 2GB -MaximumBytes 8GB -Priority 50

# Mémoire statique (préférable pour SQL, AD, charges critiques)
Set-VMMemory -VMName "SRV-SQL-01" -DynamicMemoryEnabled $false -StartupBytes 16GB
```

| Charge | Mémoire dynamique ? |
|---|---|
| Web, fichiers, utilitaires | Oui |
| AD DS, DNS | Plutôt statique (petites VM, coût nul) |
| SQL Server, Exchange | **Non** (statique + verrouillée) |
| RDS Session Host | Statique |

**Disques :**

| Type VHDX | Avantage | Inconvénient |
|---|---|---|
| Dynamique (expansible) | Gain de place | Fragmentation, légère perte perf |
| Taille fixe | Meilleures perfs | Espace réservé |
| Différenciant | Lab, VDI | Chaîne parent/enfant fragile |

```powershell
# Convertir un disque dynamique en fixe (VM éteinte)
Convert-VHD -Path "D:\VMs\SRV-SQL-01\disque.vhdx" -DestinationPath "D:\VMs\SRV-SQL-01\disque-fixe.vhdx" -VHDType Fixed

# Étendre un disque (VM allumée possible si pas de snapshot)
Resize-VHD -Path "D:\VMs\SRV-FICHIER-01\data.vhdx" -SizeBytes 500GB
# Puis étendre la partition DANS la VM : Gestion des disques ou
# Resize-Partition -DriveLetter D -Size (Get-PartitionSupportedSize -DriveLetter D).SizeMax
```

---

## 20. Checkpoints : standard vs production

| | Checkpoint standard | Checkpoint de production |
|---|---|---|
| Technique | État mémoire complet | VSS dans la VM (cohérent applicatif) |
| Usage | Lab, tests | **Production** (sauvegarde applicative) |
| Restauration | Retour état exact | Restauration "au redémarrage" |

```powershell
# Passer en checkpoints de production (défaut recommandé en prod)
Set-VM -Name "SRV-SQL-01" -CheckpointType Production
Set-VM -Name "SRV-SQL-01" -AutomaticCheckpointsEnabled $false

# Créer / lister / restaurer / supprimer
Checkpoint-VM -Name "SRV-WEB-01" -SnapshotName "Avant-MAJ-2026-09"
Get-VMSnapshot -VMName "SRV-WEB-01"
Restore-VMSnapshot -Name "Avant-MAJ-2026-09" -VMName "SRV-WEB-01" -Confirm:$false
Remove-VMSnapshot -VMName "SRV-WEB-01" -Name "Avant-MAJ-2026-09"
```

> **Un checkpoint n'est PAS une sauvegarde.** Chaîne AVHDX fragile, perte de perfs, risque de saturation disque. Durée de vie max conseillée : **quelques jours**, jamais des mois. En production, préférer la vraie sauvegarde (§52).

---

## 21. Réplication Hyper-V (Hyper-V Replica)

Réplication asynchrone d'une VM vers un second hôte (même site ou distant), sans stockage partagé. RPO 5 minutes (30 s / 15 min configurables).

```powershell
# --- Sur le site de REPLICA (destination) ---
# Autoriser la réplication entrante (Kerberos = domaine, Certificat = hors domaine)
Set-VMReplicationServer -ReplicationEnabled $true -AllowedAuthenticationType Kerberos `
  -KerberosPort 8080 -DefaultStorageLocation "E:\Replica"

# Ouvrir le pare-feu
Enable-NetFirewallRule -DisplayName "Réplication Hyper-V (TCP 8080 entrant)"

# --- Sur le site PRINCIPAL (source) ---
# Activer la réplication pour une VM
Enable-VMReplication -VMName "SRV-FICHIER-01" -ReplicaServerName "SRV-HV-DR-01" `
  -ReplicaServerPort 8080 -AuthenticationType Kerberos `
  -ReplicaVhdLocation "E:\Replica" -VSSSnapshotFrequencyHour 4

# Démarrer la réplication initiale (réseau ou support externe)
Start-VMInitialReplication -VMName "SRV-FICHIER-01"

# Suivi
Get-VMReplication -VMName "SRV-FICHIER-01" | Format-List *
Measure-VMReplication -VMName "SRV-FICHIER-01"   # statistiques (nécessite le compteur activé)

# Basculement planifié (maintenance) / non planifié (sinistre)
Start-VMFailover -VMName "SRV-FICHIER-01"                    # planifié : sur la source
Start-VMFailover -VMName "SRV-FICHIER-01" -AsTest            # test sans impacter la prod
```

**Limites :** pas de haute disponibilité automatique (bascule manuelle), RPO ≥ 30 s, ne remplace pas un cluster pour la continuité locale.

---

## 22. Cluster de basculement + CSV

Architecture : 2+ nœuds Hyper-V + stockage partagé (SAN, Storage Spaces Direct, ou SMB 3.0 Scale-Out) + réseau dédié.

```powershell
# 1. Installer le rôle sur chaque nœud
Install-WindowsFeature -Name Failover-Clustering -IncludeManagementTools

# 2. Valider la configuration (OBLIGATOIRE avant création)
Test-Cluster -Node SRV-HV-01,SRV-HV-02 -Include "Inventory","Network","Storage","System Configuration"
# Lire le rapport : C:\Windows\Cluster\Reports\Validation Report.html

# 3. Créer le cluster
New-Cluster -Name CLUSTER-HV -Node SRV-HV-01,SRV-HV-02 -StaticAddress 192.168.10.50

# 4. Ajouter le stockage et le passer en CSV (Cluster Shared Volume)
Get-ClusterAvailableDisk | Add-ClusterDisk
Add-ClusterSharedVolume -Name "Cluster Disk 1"   # devient C:\ClusterStorage\Volume1

# 5. Rendre des VM hautement disponibles
Add-ClusterVirtualMachineRole -VMName "SRV-AD-01"
Add-ClusterVirtualMachineRole -VMName "SRV-FICHIER-01"

# 6. Quorum : témoin de partage de fichiers (recommandé pour 2 nœuds)
Set-ClusterQuorum -FileShareWitness "\\SRV-FICHIER-01\Quorum"
```

**Réseaux du cluster :** séparer (1) management, (2) trafic VM, (3) cluster/heartbeat, (4) CSV/redirection. Le heartbeat sur le même lien que les VM = risque de bascules intempestives.

```powershell
# Vérifier l'état
Get-ClusterNode | Format-Table Name, State -AutoSize
Get-ClusterResource | Where-Object ResourceType -eq "Virtual Machine" | Format-Table Name, State, OwnerNode
```

---

## 23. Bonnes pratiques Hyper-V (20 règles d'or)

1. Hôtes en **Server Core**, jamais de Desktop Experience sur un hôte.
2. **Aucun autre rôle** sur l'hôte (pas d'AD, pas de fichiers, pas d'IIS).
3. Volume dédié pour les VM, jamais sur C:.
4. Antivirus : **exclure** les dossiers VM, extensions `.vhd/.vhdx/.avhd/.avhdx`, processus `vmms.exe`/`vmwp.exe`.
5. Checkpoints : durée de vie < 7 jours, jamais comme sauvegarde.
6. Mémoire dynamique sauf charges critiques (SQL).
7. vCPU : commencer petit (2-4), ratio max ~8:1 vCPU/pCPU en prod généraliste.
8. **Secure Boot** activé sur les Gen2 (sauf incompatibilité prouvée).
9. Services d'intégration à jour (via Windows Update dans la VM).
10. Arrêt : configurer l'**action d'arrêt automatique** (Enregistrer l'état ou Arrêter).
11. Réseau : séparer management / VM / cluster / CSV.
12. Teaming (SET) sur 2022/2025 : `New-VMSwitch -Name vSw -NetAdapterName "Team1" -EnableEmbeddedTeaming $true`.
13. Jumbo frames uniquement si toute la chaîne les supporte (sinon perfs dégradées).
14. Réplication Hyper-V ≠ cluster : RPO 5 min, bascule manuelle.
15. Ne pas faire de snapshots sur les **contrôleurs de domaine virtualisés** (risque USN rollback — préférer la réplication AD native).
16. Time sync : désactiver l'intégration "Synchronisation de l'heure" sur les DC (le PDC fait foi via NTP).
17. Dédup : **jamais** sur un CSV ni sur des VHDX de VM en cours d'exécution.
18. Sauvegarder l'hôte (config) ET les VM (données).
19. Documenter : nom VM = rôle, notes Hyper-V remplies (`Set-VM -Notes "..."`).
20. Tester la restauration. Une sauvegarde non testée = pas de sauvegarde.

---

## 24. WSUS : architecture et prérequis

WSUS = référentiel local des mises à jour Microsoft. Les clients se synchronisent dessus au lieu d'aller sur Internet.

**Prérequis :**
- IIS (installé automatiquement avec le rôle)
- Base : **WID** (Windows Internal Database, incluse) ou SQL Server
- Stockage : 100 Go minimum (métadonnées + contenu selon produits/classifications)
- RAM : 4 Go minimum, 8 Go conseillés (le pool IIS WSUS consomme)

```powershell
# Installer WSUS (base WID)
Install-WindowsFeature -Name UpdateServices -IncludeManagementTools
# Puis lancer la configuration post-installation :
& 'C:\Program Files\Update Services\Tools\wsusutil.exe' postinstall CONTENT_DIR=D:\WSUS
```

---

## 25. Installation et configuration initiale de WSUS

Après `wsusutil postinstall` :

1. Ouvrir la console **Windows Server Update Services**.
2. Assistant de configuration : amont = **Microsoft Update** (ou serveur WSUS parent en aval/réplica).
3. Choisir les **produits** (ex. Windows Server 2022, Windows 11, Office) — ne cocher que l'utile.
4. Choisir les **classifications** : Critical Updates, Security Updates, Definition Updates, Updates, Upgrades (avec parcimonie).
5. Planifier la **synchronisation** (ex. 1×/jour à 3h).
6. Première synchro : longue (plusieurs heures la première fois).

```powershell
# Forcer une synchronisation
$wsus = Get-WsusServer -Name "localhost" -PortNumber 8530
$wsus.GetSubscription().StartSynchronization()

# Vérifier l'état de la synchro
$wsus.GetSubscription().GetLastSynchronizationInfo()
```

> Ports : **8530 (HTTP)** / 8531 (HTTPS) par défaut depuis WSUS 2012. Les vieux guides parlent du port 80 : à oublier.

---

## 26. Groupes d'ordinateurs et ciblage côté client

**Côté serveur :** créer des groupes (ex. `Serveurs-Production`, `Serveurs-Test`, `Postes-Pilotes`, `Postes-Standard`).

**Côté client :** ciblage via **GPO** (recommandé) :

```
Configuration ordinateur → Stratégies → Modèles d'administration →
Composants Windows → Windows Update → Gérer les mises à jour proposées par WSUS
  • Spécifier l'emplacement du service : http://srv-wsus-01:8530
  • Activer le ciblage côté client → nom du groupe : Serveurs-Test
```

```powershell
# Vérifier côté client que la GPO s'applique
gpresult /r | Select-String "WSUS|ciblage" -Context 2
Get-ItemProperty "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate" -Name WUServer, TargetGroup -ErrorAction SilentlyContinue

# Forcer le client à se signaler au WSUS
$updateSession = New-Object -ComObject Microsoft.Update.Session
wuauclt /reportnow /detectnow   # legacy, toujours utile
UsoClient.exe StartScan         # Windows 10/11 / Server 2019+
```

> Le client apparaît dans la console WSUS sous **Ordinateurs → Tous les ordinateurs** après son premier rapport (délai : jusqu'à ~24 h ou forcer comme ci-dessus).

---

## 27. Approbations : manuelles et règles automatiques

**Manuelle :** clic droit sur la mise à jour → Approuver → choisir groupes → OK.

**Règles d'approbation automatique** (recommandées) :

1. Console WSUS → **Options → Approbations automatiques**.
2. Nouvelle règle : ex. *"Critiques + Sécurité → Serveurs-Test"*.
3. Deuxième règle : *"Critiques + Sécurité → Serveurs-Production, délai 7 jours"* (échéance différée).

```powershell
# Approuver via PowerShell : toutes les mises à jour de sécurité non approuvées pour un groupe
$wsus = Get-WsusServer -Name "localhost" -PortNumber 8530
$groupe = $wsus.GetComputerTargetGroups() | Where-Object Name -eq "Serveurs-Production"
$updates = $wsus.GetUpdates() | Where-Object {
  $_.Classification.Title -match "Security" -and
  ($_.GetUpdateApprovalActions($groupe) | Where-Object { $_.Action -eq 'Install' }).Count -eq 0 -and
  -not $_.IsDeclined
}
$updates | ForEach-Object { $_.Approve("Install", $groupe) }
```

**Cycle de patch recommandé :** Test (J+2) → Pilotes (J+5) → Production (J+9), avec fenêtre de maintenance et validation applicative entre chaque.

---

## 28. Maintenance : Server Cleanup Wizard et scripts

Sans maintenance, la base WID gonfle (10+ Go) et la console devient inutilisable.

**Assistant de nettoyage du serveur** (Options → Assistant Nettoyage du serveur), mensuel :
- Mises à jour expirées, remplacées, refusées
- Fichiers non nécessaires
- Ordinateurs inactifs (30+ jours)

```powershell
# Nettoyage automatisé mensuel (tâche planifiée)
$wsus = Get-WsusServer -Name "localhost" -PortNumber 8530
$cleanup = $wsus.GetCleanupManager()
$cleanup.PerformCleanup(
  [Microsoft.UpdateServices.Administration.CleanupScope]::new(
    $true,  # CleanUnneededContentFiles
    $true,  # CleanExpiredUpdates
    $true,  # CleanSupersededUpdates
    $true,  # CleanExpiredComputers
    $true,  # DeclineExpiredUpdates
    $true   # DeclineSupersededUpdates
  )
)

# Réindexer la base WID (quand la console rame)
sqlcmd -S \\.\pipe\MICROSOFT##WID\tsql\query -E -Q "USE SUSDB; EXEC sp_MSforeachtable 'DBCC DBREINDEX (''?'')'"
```

> Si la console WSUS met > 30 s à s'ouvrir : base à réindexer + pool IIS à augmenter (private memory limit du `WsusPool` → 4 Go+).

---

## 29. Dépannage WSUS (clients qui ne remontent pas, reset base)

**Client qui ne remonte pas — méthode en 6 étapes :**

```powershell
# 1. Le client vise-t-il le bon WSUS ?
Get-ItemProperty "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate" -Name WUServer

# 2. Connectivité
Test-NetConnection srv-wsus-01 -Port 8530

# 3. Réinitialiser l'agent Windows Update (le classique qui marche)
Stop-Service wuauserv, bits, cryptsvc -Force
Remove-Item "$env:windir\SoftwareDistribution\*" -Recurse -Force
Remove-Item "$env:windir\System32\catroot2\*" -Recurse -Force   # prudent : arrêter cryptsvc avant
Start-Service wuauserv, bits, cryptsvc
wuauclt /resetauthorization /detectnow

# 4. Doublon de SusClientId (image clonée sans sysprep !)
Remove-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate" -Name SusClientId, SusClientIdValidation -ErrorAction SilentlyContinue

# 5. Forcer le rapport
UsoClient.exe StartScan; Start-Sleep 60; wuauclt /reportnow

# 6. Logs
Get-WindowsUpdateLog   # génère WindowsUpdate.log lisible sur le bureau
```

**Reset complet de la base WSUS** (quand SUSDB est corrompue) :

```powershell
# Option nucléaire : réinitialiser le contenu + resynchroniser
& 'C:\Program Files\Update Services\Tools\wsusutil.exe' reset
# Puis forcer une synchro complète. Les approbations sont conservées (base), seul le contenu est re-téléchargé.
```

**Erreur 0x8024401c / timeout :** souvent le pool IIS `WsusPool` en mémoire insuffisante → augmenter la limite mémoire privée à 4–8 Go et recycler.

---

---

## 30. Partages réseau : SMB, création, bonnes pratiques

```powershell
# Créer un partage SMB simple
New-SmbShare -Name "Commun" -Path "D:\Partages\Commun" -FullAccess "ENTREPRISE\Utilisateurs du domaine"

# Partage avec accès restreint + description
New-SmbShare -Name "Compta" -Path "D:\Partages\Compta" -ChangeAccess "ENTREPRISE\GRP-Compta" `
  -Description "Partage comptabilité - accès restreint"

# Partage masqué (avec $) pour l'administration
New-SmbShare -Name "Admin$" -Path "D:\Partages\Admin" -FullAccess "ENTREPRISE\Admins du domaine"

# Lister / modifier / supprimer
Get-SmbShare | Format-Table Name, Path, Description -AutoSize
Set-SmbShare -Name "Commun" -Description "Nouvelle description" -Force
Remove-SmbShare -Name "AncienPartage" -Force
```

**Bonnes pratiques :**
- Permissions de partage : **Tout le monde = Contrôle total**, et gérer le fin via **NTFS** (§31). (L'exception : partages sensibles où on restreint aussi au niveau partage.)
- Activer **l'énumération basée sur l'accès (ABE)** : l'utilisateur ne voit que ce à quoi il a accès.
- Volume dédié, quotas FSRM (§35), antivirus avec exclusions réfléchies.
- Désactiver SMBv1 partout (voir §60) :

```powershell
# Vérifier / désactiver SMBv1
Get-WindowsFeature FS-SMB1
Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol -NoRestart
Get-SmbServerConfiguration | Select-Object EnableSMB1Protocol, EnableSMB2Protocol
```

---

## 31. Permissions NTFS vs permissions de partage : la combinaison

**Règle fondamentale : l'accès effectif = l'intersection (la plus restrictive) des deux.**

| | Partage : Contrôle total | Partage : Lecture |
|---|---|---|
| **NTFS : Contrôle total** | Contrôle total | Lecture |
| **NTFS : Modification** | Modification | Lecture |
| **NTFS : Lecture** | Lecture | Lecture |

**Méthode recommandée :**
1. Partage : `Tout le monde` (ou `Utilisateurs authentifiés`) = **Contrôle total**.
2. NTFS : permissions fines par groupe (Lecture / Modification / Contrôle total).
3. Groupes AD par rôle : `GRP-Partage-Compta-L` (lecture), `GRP-Partage-Compta-M` (modification).

```powershell
# Poser des permissions NTFS propres (héritage désactivé, groupes uniquement)
$path = "D:\Partages\Compta"
$acl = Get-Acl $path
$acl.SetAccessRuleProtection($true, $false)  # désactive l'héritage, ne copie pas les règles existantes
$acl.Access | ForEach-Object { $acl.RemoveAccessRule($_) } | Out-Null
$acl.AddAccessRule((New-Object System.Security.AccessControl.FileSystemAccessRule(
  "ENTREPRISE\GRP-Compta-M", "Modify", "ContainerInherit,ObjectInherit", "None", "Allow")))
$acl.AddAccessRule((New-Object System.Security.AccessControl.FileSystemAccessRule(
  "ENTREPRISE\GRP-Compta-L", "ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow")))
$acl.AddAccessRule((New-Object System.Security.AccessControl.FileSystemAccessRule(
  "AUTORITE NT\Système", "FullControl", "ContainerInherit,ObjectInherit", "None", "Allow")))
$acl.AddAccessRule((New-Object System.Security.AccessControl.FileSystemAccessRule(
  "ENTREPRISE\Admins du domaine", "FullControl", "ContainerInherit,ObjectInherit", "None", "Allow")))
Set-Acl -Path $path -AclObject $acl

# Auditer les permissions effectives d'un utilisateur
Get-Acl $path | Select-Object -ExpandProperty Access | Format-Table IdentityReference, FileSystemRights, AccessControlType
```

> **Jamais** de permissions à un utilisateur nommé directement : toujours via des groupes AD. Le jour du départ d'un salarié, on retire le groupe, pas 47 ACL.

---

## 32. Cas pratiques de permissions (tableau de vérité)

| Besoin | Partage | NTFS |
|---|---|---|
| Dossier lecture seule pour tous | Tout le monde = CT | Utilisateurs du domaine = Lecture |
| Écriture pour un service, lecture pour les autres | Tout le monde = CT | GRP-Service = Modification ; Utilisateurs = Lecture |
| Dossier confidentiel RH | GRP-RH = CT (partage restreint) | GRP-RH = Modification ; héritage coupé |
| Dépôt sans suppression (writ-only log) | Tout le monde = CT | Créateur propriétaire = Contrôle total ; autres = Écriture seule (droits spéciaux) |
| Homedirs `\\srv\users\%username%` | Utilisateurs authentifiés = CT | Chaque dossier : utilisateur = Modification, héritage coupé |

```powershell
# Créer des dossiers personnels en masse (à partir d'une liste d'utilisateurs)
$users = Get-ADUser -Filter * -SearchBase "OU=Utilisateurs,DC=entreprise,DC=lan" | Select-Object -ExpandProperty SamAccountName
foreach ($u in $users) {
    $dir = "D:\Partages\Users\$u"
    if (-not (Test-Path $dir)) { New-Item -ItemType Directory -Path $dir | Out-Null }
    $acl = Get-Acl $dir
    $acl.SetAccessRuleProtection($true, $false)
    $acl.AddAccessRule((New-Object System.Security.AccessControl.FileSystemAccessRule(
      "ENTREPRISE\$u", "Modify", "ContainerInherit,ObjectInherit", "None", "Allow")))
    Set-Acl -Path $dir -AclObject $acl
}
New-SmbShare -Name "Users" -Path "D:\Partages\Users" -FullAccess "ENTREPRISE\Utilisateurs authentifiés"
```

---

## 33. DFS-N : espaces de noms

DFS-N = un chemin logique unique (`\\entreprise.lan\data\compta`) qui pointe vers un ou plusieurs partages physiques, avec bascule.

```powershell
# Installer
Install-WindowsFeature -Name FS-DFS-Namespace -IncludeManagementTools

# Créer un espace de noms de domaine
New-DfsnRoot -Path "\\entreprise.lan\data" -TargetPath "\\SRV-FICHIER-01\data" -Type DomainV2

# Ajouter un dossier + cibles (2e cible = haute dispo / site distant)
New-DfsnFolder -Path "\\entreprise.lan\data\compta" -TargetPath "\\SRV-FICHIER-01\compta"
New-DfsnFolderTarget -Path "\\entreprise.lan\data\compta" -TargetPath "\\SRV-FICHIER-02\compta"

# Lister
Get-DfsnRoot | Format-Table Path, State
Get-DfsnFolder -Path "\\entreprise.lan\data\*" | Format-Table Path, State
```

**Pourquoi DFS-N même avec un seul serveur :** abstraction. Le jour où tu migres `SRV-FICHIER-01` → `SRV-FICHIER-03`, les utilisateurs gardent `\\entreprise.lan\data\...` : zéro changement de lecteurs mappés, zéro GPO à retoucher.

---

## 34. DFS-R : réplication

DFS-R réplique le contenu entre membres d'un groupe de réplication (RDC = ne transfère que les blocs modifiés).

```powershell
# Installer
Install-WindowsFeature -Name FS-DFS-Replication -IncludeManagementTools

# Créer un groupe de réplication (ex. entre 2 serveurs de fichiers)
New-DfsReplicationGroup -GroupName "RG-Data"
Add-DfsrMember -GroupName "RG-Data" -ComputerName "SRV-FICHIER-01","SRV-FICHIER-02"
New-DfsReplicatedFolder -GroupName "RG-Data" -FolderName "Compta" -DfsnPath "\\entreprise.lan\data\compta"
Set-DfsrMembership -GroupName "RG-Data" -FolderName "Compta" -ComputerName "SRV-FICHIER-01" `
  -ContentPath "D:\Partages\Compta" -PrimaryMember $true -StagingPathQuotaInMB 16384
Set-DfsrMembership -GroupName "RG-Data" -FolderName "Compta" -ComputerName "SRV-FICHIER-02" `
  -ContentPath "D:\Partages\Compta" -StagingPathQuotaInMB 16384

# Suivi
Get-DfsrState -ComputerName SRV-FICHIER-01 | Format-Table FolderName, State -AutoSize
dfsrdiag backlog /rgname:"RG-Data" /rfname:"Compta" /smem:SRV-FICHIER-01 /rmem:SRV-FICHIER-02
```

**Limites à connaître :**
- Fichiers **ouverts/verrouillés** : conflit = le "perdant" va dans `DfsrPrivate\ConflictAndDeleted` (quota par défaut 660 Mo — à augmenter).
- **Ne pas** répliquer des bases de données ouvertes (SQL, etc.) ni des VHDX de VM.
- Taille max de fichier : ça passe, mais la réplication initiale d'un gros volume prend des heures — pré-répliquer via disque externe (`robocopy` + pré-amorçage).

---

## 35. FSRM : quotas et filtrage de fichiers

```powershell
Install-WindowsFeature -Name FS-Resource-Manager -IncludeManagementTools

# --- QUOTAS ---
# Modèle : 10 Go avec seuil d'alerte à 85 %
New-FsrmQuotaTemplate -Name "Standard 10 Go" -Size 10GB
# Appliquer à un dossier
New-FsrmQuota -Path "D:\Partages\Users" -Template "Standard 10 Go"

# Quota dur (bloque) vs souple (alerte seule) : le template "Standard" est dur par défaut.
# Notification e-mail à 90 % :
# (via la console FSRM → Modèles de quota → Seuils, ou en XML d'action)

# --- FILTRAGE DE FICHIERS (file screening) ---
# Bloquer les fichiers multimédias et exécutables sur un partage de travail
New-FsrmFileScreenTemplate -Name "Blocage Multimédia+Exe" -IncludeGroup "Fichiers audio et vidéo","Fichiers exécutables"
New-FsrmFileScreen -Path "D:\Partages\Commun" -Template "Blocage Multimédia+Exe" -Active:$true

# Groupes de fichiers utiles prédéfinis : "Fichiers audio et vidéo", "Fichiers image",
# "Fichiers exécutables", "Fichiers compressés", "Fichiers de sauvegarde"...

# Rapport d'usage
New-FsrmStorageReport -Name "Rapport Quotas" -Namespace @("D:\Partages") -ReportType @("QuotaUsage")
```

**Cas d'usage n°1 en entreprise :** le file screening anti-ransomware — bloquer les extensions exotiques (`*.locked`, `*.crypt`, etc.) en alerte passive pour détecter une propagation.

---

## 36. Déduplication des données

Gain typique : 30–70 % sur des partages bureautiques (beaucoup de doublons : mêmes pièces jointes, mêmes ISO, mêmes installateurs).

```powershell
# Installer + activer sur un volume (JAMAIS sur le volume système, JAMAIS sur un CSV)
Install-WindowsFeature -Name FS-Data-Deduplication
Enable-DedupVolume -Volume "D:" -UsageType Default

# Planifier l'optimisation (par défaut : tâche planifiée, à ajuster)
Set-DedupSchedule -Name "BackgroundOptimization" -Start (Get-Date "2026-01-01 02:00") -DurationHours 6 -Days Monday,Tuesday,Wednesday,Thursday,Friday

# Forcer une optimisation + mesurer le gain
Start-DedupJob -Volume "D:" -Type Optimization -Priority High
Get-DedupStatus -Volume "D:" | Format-List Volume, SavingsRate, SavedSpace, OptimizedFilesCount

# Exclure des extensions (bases, chiffrés)
Set-DedupVolume -Volume "D:" -ExcludeFileType @("edb","vhdx","bak")
```

| À faire | À ne pas faire |
|---|---|
| Volumes de fichiers partagés, VDI | Volume système (C:) |
| Sauvegardes sur disque (avec prudence) | CSV de cluster Hyper-V |
| | VHDX de VM en cours d'exécution |

---

## 37. Dépannage d'accès aux fichiers (méthode en 7 étapes)

Face à "Accès refusé" sur `\\srv\partage` :

```powershell
# 1. Le partage existe-t-il ? Le serveur répond-il ?
Test-NetConnection SRV-FICHIER-01 -Port 445
Get-SmbShare -CimSession SRV-FICHIER-01 | Where-Object Name -eq "Compta"

# 2. Permissions de PARTAGE
Get-SmbShareAccess -Name "Compta" -CimSession SRV-FICHIER-01

# 3. Permissions NTFS + héritage
(Get-Acl "\\SRV-FICHIER-01\Compta").Access | Format-Table IdentityReference, FileSystemRights, IsInherited

# 4. L'utilisateur est-il bien dans le groupe ? (jeton d'accès)
whoami /groups                          # sur le poste de l'utilisateur
# Attention : appartenance ajoutée APRÈS l'ouverture de session = déconnexion/reconnexion requise !

# 5. Verrous de fichiers ouverts
Get-SmbOpenFile -CimSession SRV-FICHIER-01 | Where-Object Path -like "*Compta*"

# 6. Journal côté serveur (échecs d'audit si activés)
Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4656,4658} -MaxEvents 20

# 7. Accès effectif (onglet Sécurité → Avancé → Accès effectif, ou icacls)
icacls "D:\Partages\Compta"
```

**Top 3 des causes réelles :** (1) groupe ajouté mais session non rouverte, (2) héritage NTFS coupé par un sous-dossier, (3) partage restreint + NTFS large (ou l'inverse).

---

---

## 38. Serveur d'impression : rôle et architecture

Le serveur d'impression centralise : files d'attente, pilotes, déploiement GPO, supervision. Un seul point d'administration au lieu de 40 imprimantes installées en local sur les postes.

```powershell
# Installer le rôle
Install-WindowsFeature -Name Print-Services -IncludeManagementTools
# Services de rôle utiles :
Install-WindowsFeature -Name Print-Server, Print-Management   # Print-Management = console dédiée
```

**Architecture :** poste client → file d'attente sur le serveur (`\\SRV-PRINT-01\Copieur-Compta`) → spouleur serveur → imprimante/copieur réseau (port TCP/IP Standard, port 9100 ou LPR).

**LIEN MÉTIER COPIEURS (Zelef) :** le serveur d'impression est l'interface entre ton parc copieurs (Kyocera, etc.) et les utilisateurs. Bonnes pratiques spécifiques copieurs :
- 1 file d'attente par **fonction** (ex. `Copieur-Accueil-NB`, `Copieur-Accueil-Couleur`) plutôt que par machine, pour mutualiser.
- Pilote **PCL** pour la bureautique courante, **PostScript** si PAO/impression graphique, **KX Driver** (Kyocera) en version validée — jamais la dernière version le jour de sa sortie.
- Port TCP/IP Standard avec **SNMP activé** (communauté `public` en lecture) pour remonter les états (toner bas, bourrage) — croiser avec la supervision (voir guide onduleurs/supervision, Zabbix).

---

## 39. Déploiement d'imprimantes et copieurs via GPO

```powershell
# Publier une imprimante dans l'annuaire (pour recherche)
Set-Printer -Name "Copieur-Compta" -Published $true

# Lister les imprimantes du serveur
Get-Printer -ComputerName SRV-PRINT-01 | Format-Table Name, DriverName, PortName, Published -AutoSize
```

**Méthode GPO (recommandée) :**

1. Console **Gestion de l'impression** → clic droit sur l'imprimante → **Déployer avec la stratégie de groupe**.
2. Choisir : **par utilisateur** (suit l'utilisateur) ou **par ordinateur** (suit le poste — idéal pour un copieur d'étage).
3. Lier la GPO à l'OU des utilisateurs/postes concernés.
4. Au prochain `gpupdate`, l'imprimante apparaît.

**En PowerShell (alternative, script d'ouverture de session) :**

```powershell
# Ajouter une imprimante réseau partagée sur un poste (script GPO)
Add-Printer -ConnectionName "\\SRV-PRINT-01\Copieur-Accueil"
# Définir par défaut
(Get-WmiObject -Class Win32_Printer -Filter "Name='\\\\SRV-PRINT-01\\Copieur-Accueil'").SetDefaultPrinter()
```

> Depuis les correctifs **PrintNightmare** (2021), le déploiement de pilotes Type 3 exige des droits administrateur côté client sauf configuration spécifique (Point and Print Restrictions). Préférer les pilotes **Type 4** quand disponibles, ou signer/valider le pipeline (voir §40).

---

## 40. Pilotes Type 3 vs Type 4, Print Management

| | Type 3 (v3) | Type 4 (v4) |
|---|---|---|
| Modèle | Pilote fabricant complet | Basé sur la classe (inbox) + extensions |
| Installation client | Télécharge le pilote depuis le serveur | Utilise le pilote local du poste |
| PrintNightmare | Surface d'attaque (exécution côté serveur) | Réduite |
| Fonctionnalités avancées | Complètes (finisher, codes...) | Parfois limitées |
| Recommandation | Si besoin métier (copieurs avec codes départementaux) | Par défaut si possible |

```powershell
# Voir les pilotes installés et leur type
Get-PrinterDriver -ComputerName SRV-PRINT-01 | Format-Table Name, PrinterEnvironment, MajorVersion -AutoSize
# MajorVersion 3 = Type 3, 4 = Type 4

# Isoler un pilote problématique (mode isolé)
Set-PrinterDriver -Name "Kyocera ECOSYS MXXXX" -PrinterEnvironment "Windows x64" # (via registre/GUI pour l'isolation)
# En pratique : Gestion de l'impression → Pilotes → Propriétés → Isolation : Isolé/Partagé
```

**Règle copieurs :** les fonctions avancées (codes comptables, boîtes personnelles, finisher agrafage) exigent souvent le pilote **Type 3 du fabricant**. Dans ce cas : serveur d'impression dédié, pilotes validés, **jamais** de pilote téléchargé au hasard.

---

## 41. Files d'attente, spooler : redémarrage et migration

```powershell
# Redémarrer le spouleur (LE réflexe quand tout est bloqué)
Restart-Service -Name Spooler -Force

# Purger une file bloquée (document corrompu)
Get-PrintJob -PrinterName "Copieur-Compta" -ComputerName SRV-PRINT-01
Remove-PrintJob -PrinterName "Copieur-Compta" -ID 42 -ComputerName SRV-PRINT-01
# Purge totale d'urgence :
Stop-Service Spooler -Force
Remove-Item "C:\Windows\System32\spool\PRINTERS\*" -Force
Start-Service Spooler

# Lister les travaux en erreur
Get-PrintJob -ComputerName SRV-PRINT-01 | Where-Object JobStatus -like "*Error*"

# MIGRATION vers un nouveau serveur (l'outil officiel)
# Sur l'ancien :
Printbrm -s \\SRV-PRINT-OLD -b -f D:\Migration\printers.printerExport
# Sur le nouveau :
Printbrm -s \\SRV-PRINT-NEW -r -f D:\Migration\printers.printerExport
```

> `Printbrm` migre files, pilotes, ports, formulaires. Tester sur 2 imprimantes avant de tout basculer. Prévoir le changement de nom dans les GPO de déploiement.

---

## 42. Dépannage d'impression (lien métier copieurs)

| Symptôme | Cause probable | Action |
|---|---|---|
| Rien ne s'imprime, file bloquée | Document corrompu / pilote | Purger la file (§41), redémarrer spouleur |
| "Pilote non disponible" sur postes | Type 3 + restrictions Point and Print | GPO Point and Print : serveurs approuvés ; ou passer en Type 4 |
| Impression lente (gros PDF) | Rendu côté client désactivé | Activer "Rendre les travaux sur les ordinateurs clients" (propriétés imprimante → Partage) |
| Le copieur n'est pas joint | Port TCP/IP en erreur | `Test-NetConnection 192.168.20.50 -Port 9100` ; vérifier SNMP |
| Codes départementaux non demandés | Pilote générique au lieu du KX Driver | Installer le pilote fabricant Type 3 validé |
| Bourrages non remontés | SNMP désactivé sur le port | Activer SNMP sur le port TCP/IP Standard |
| File "hors connexion" | Copieur éteint / IP changée (DHCP !) | **IP fixe ou réservation DHCP** sur tous les copieurs |

```powershell
# Diagnostic express d'un serveur d'impression
Get-Service Spooler | Select-Object Name, Status
Get-Printer -ComputerName SRV-PRINT-01 | Where-Object PrinterStatus -ne "Normal" |
  Format-Table Name, PrinterStatus, PortName -AutoSize
# Tester le port d'un copieur
Test-NetConnection -ComputerName 192.168.20.50 -Port 9100
```

---

## 43. RDS : les 4 rôles (Broker, Session Host, Passerelle, Accès Web)

| Rôle | Fonction |
|---|---|
| **RD Connection Broker** | Aiguille les connexions, gère les collections, reconnexion de session |
| **RD Session Host** | Héberge les sessions utilisateurs (le "serveur TSE") |
| **RD Gateway** | Proxy HTTPS (443) pour accès externes sans VPN |
| **RD Web Access** | Portail web listant les applis/bureaux publiés (RemoteApp) |

Topologies :
- **PME** : tout sur 1–2 serveurs (Broker + Web + Passerelle sur l'un, Session Hosts sur l'autre).
- **Entreprise** : Broker en HA (SQL), ferme de Session Hosts, Passerelle en DMZ.

```powershell
# Installer les 4 rôles (déploiement rapide, 1 serveur)
Install-WindowsFeature -Name RDS-RD-Server -IncludeManagementTools
# Puis : Server Manager → Services Bureau à distance → déploiement basé sur les scénarios
```

---

## 44. Déploiement RDS pas à pas

**Via Server Manager (recommandé pour la topologie) :**

1. Server Manager → **Services Bureau à distance** → **Vue d'ensemble** → déploiement standard.
2. Ajouter les serveurs aux 4 rôles.
3. Créer une **collection de sessions** : nom, Session Hosts membres, groupes d'utilisateurs autorisés.
4. Publier des **RemoteApp** (ex. l'ERP) ou le **bureau complet**.
5. Configurer le **certificat** (public si passerelle exposée, voir §48 PKI en interne).
6. Tester avec un compte utilisateur standard.

**En PowerShell :**

```powershell
# Créer la collection + publier une RemoteApp
New-RDSessionCollection -CollectionName "Bureautique" -SessionHost "SRV-RDS-01.entreprise.lan","SRV-RDS-02.entreprise.lan" `
  -ConnectionBroker "SRV-RDS-CB-01.entreprise.lan"
New-RDRemoteApp -CollectionName "Bureautique" -DisplayName "ERP Gestion" `
  -FilePath "C:\Program Files\ERP\erp.exe" -Alias "ERP"
Set-RDSessionCollectionConfiguration -CollectionName "Bureautique" -UserGroup "ENTREPRISE\GRP-RDS-Users"

# Équilibrage : pondération des hôtes
Set-RDSessionHost -SessionHost "SRV-RDS-01.entreprise.lan" -NewConnectionAllowed Yes
```

> Les hôtes de session RDS sont le **cas n°1 du Desktop Experience** (§10) : les utilisateurs ont besoin d'une GUI complète.

---

## 45. Licences CAL RDS : par utilisateur vs par appareil

En plus des CAL Windows Server (§58), RDS exige des **CAL RDS** :

| | Par utilisateur | Par appareil |
|---|---|---|
| Compte | Chaque **utilisateur** nommé | Chaque **appareil** |
| Idéal | Utilisateurs multi-appareils (PC + tablette + maison) | Postes partagés (3×8, ateliers, bornes) |
| Suivi | Attribution permanente, révocable après 90 j | Attribution à l'appareil |

```powershell
# Installer le rôle de gestion des licences
Install-WindowsFeature -Name RDS-Licensing -IncludeManagementTools

# Activer le serveur de licences + installer les CAL (via l'assistant ou)
# Configurer le mode de licence sur la collection :
Set-RDLicenseConfiguration -LicenseServer "SRV-RDS-LIC-01.entreprise.lan" -Mode PerUser -Force

# Vérifier l'état des licences
Get-RDLicenseConfiguration
```

> **Délai de grâce : 120 jours.** Passé ce délai sans serveur de licences configuré, les connexions sont refusées. C'est la panne RDS n°1.

---

## 46. Dépannage RDS

| Symptôme | Cause probable | Action |
|---|---|---|
| "Aucun serveur de licences disponible" | Délai de grâce expiré | Configurer le serveur de licences (§45) |
| Écran noir à la connexion | Session bloquée / GPO | Fermer la session depuis le Gestionnaire : `logoff <id> /server:SRV-RDS-01` |
| Lenteurs générales | Surcharge hôte | `Get-RDUserSession` : compter les sessions ; vérifier RAM/CPU |
| RemoteApp ne se lance pas | Chemin ou droits | Vérifier le FilePath publié + groupe autorisé |
| Passerelle inaccessible de l'extérieur | Certificat / 443 | Certificat valide (nom externe), NAT 443 → passerelle |
| Imprimantes locales non remontées | Redirection désactivée | GPO : autoriser la redirection d'imprimantes ; pilote Easy Print |

```powershell
# Sessions actives et déconnexion
Get-RDUserSession -ConnectionBroker "SRV-RDS-CB-01.entreprise.lan"
Invoke-RDUserLogoff -HostServer "SRV-RDS-01.entreprise.lan" -UnifiedSessionID 3 -Force

# Voir qui consomme quoi sur un hôte
quser /server:SRV-RDS-01
Get-Process -ComputerName SRV-RDS-01 | Sort-Object WS -Descending | Select-Object -First 10 Name, WS
```

---

## 47. AD CS / PKI : concepts (AC racine, subordonnée, modèles)

**Hiérarchie type en entreprise :**
- **AC racine autonome (offline)** : allumée 1×/an pour signer la subordonnée, sinon éteinte et coffrée. Durée de vie 15–20 ans.
- **AC subordonnée d'entreprise (online)** : intégrée à l'AD, émet les certificats au quotidien. Durée de vie 5–10 ans.

**Briques :**
- **Modèle de certificat** : définit qui peut demander quoi (ex. "Utilisateur", "Ordinateur", "Serveur Web").
- **Auto-enrollment** : distribution automatique via GPO, sans intervention.
- **CRL** (liste de révocation) : publiée sur HTTP/LDAP, consultée par les clients.
- **OCSP** : alternative en ligne à la CRL.

```powershell
# Installer AD CS (sur le futur serveur d'AC subordonnée membre du domaine)
Install-WindowsFeature -Name AD-Certificate -IncludeManagementTools
# Services de rôle : Certification Authority, (optionnel) Online Responder, Web Enrollment
Install-WindowsFeature -Name ADCS-Cert-Authority, ADCS-Online-Cert, ADCS-Web-Enrollment
```

---

## 48. Installation d'une AC d'entreprise pas à pas

**Étape A — AC racine autonome (hors domaine, VM isolée) :**

```powershell
# Sur la future racine (WORKGROUP, jamais jointe au domaine)
Install-WindowsFeature -Name AD-Certificate -IncludeManagementTools
Install-AdcsCertificationAuthority -CAType StandaloneRootCA `
  -CryptoProviderName "RSA#Microsoft Software Key Storage Provider" `
  -KeyLength 4096 -HashAlgorithmName SHA512 -ValidityPeriod Years -ValidityPeriodUnits 20 `
  -CACommonName "ENTREPRISE-Root-CA" -Force
```

**Étape B — AC subordonnée d'entreprise (membre du domaine) :**

```powershell
Install-WindowsFeature -Name AD-Certificate -IncludeManagementTools
# 1. Générer la demande :
Install-AdcsCertificationAuthority -CAType EnterpriseSubordinateCA -CACommonName "ENTREPRISE-Issuing-CA-01" `
  -KeyLength 4096 -HashAlgorithmName SHA384 -ValidityPeriod Years -ValidityPeriodUnits 10 `
  -OutputCertRequestFile "C:\req-issuing.req" -Force
# 2. Signer la demande sur la RACINE (console certsrv ou certreq)
# 3. Installer le certificat signé :
Install-AdcsCertificationAuthority -CAType EnterpriseSubordinateCA -CertificateFile "C:\issuing-signé.p7b" -Force
# 4. Éteindre et archiver la racine (export clé privée sur support chiffré, coffre-fort)
```

**Publier la CRL et AIA en HTTP** (indispensable : les clients non-joints au domaine doivent y accéder) :

```powershell
# Sur l'AC : ajouter un point de distribution HTTP
certutil -setreg CA\CRLPublicationURLs "1:C:\Windows\System32\CertSrv\CertEnroll\%%3%%8%%9.crl\n2:http://pki.entreprise.lan/crld/%%3%%8%%9.crl"
certutil -setreg CA\CACertPublicationURLs "1:C:\Windows\System32\CertSrv\CertEnroll\%%1_%%3%%4.crt\n2:http://pki.entreprise.lan/crld/%%1_%%3%%4.crt"
Restart-Service certsvc
# Publier une CRL fraîche
certutil -crl
```

---

## 49. Modèles de certificats et auto-enrollment via GPO

```powershell
# Dupliquer un modèle (ex. "Utilisateur" -> "Utilisateur-Entreprise" avec auto-enrollment)
# Via la console : certsrv.msc → Modèles de certificats → Gérer → Dupliquer "Utilisateur"
# Onglet Sécurité : ajouter le groupe + cocher Inscrire + Inscription automatique
# Onglet Général : cocher "Publier le certificat dans Active Directory" si besoin

# Publier le modèle sur l'AC :
Add-CATemplate -Name "Utilisateur-Entreprise" -Force
Get-CATemplate | Format-Table Name -AutoSize
```

**GPO d'auto-enrollment :**

```
Configuration ordinateur → Stratégies → Paramètres Windows → Paramètres de sécurité →
Stratégies de clés publiques → Client des services de certificats - Inscription automatique
  • Modèle de configuration : Activé
  • Cocher : Renouveler les certificats expirés, mettre à jour les certificats...
```

**Cas d'usage internes typiques :**
| Besoin | Modèle | Auto-enrollment |
|---|---|---|
| LDAPS (AD chiffré) | Kerberos Authentication / Domain Controller | Oui, sur les DC |
| 802.1X Wi-Fi/filaire | Ordinateur + Utilisateur | Oui |
| RDP chiffré | Ordinateur | Oui |
| Serveurs web internes (IIS, WAC) | Serveur Web | Manuel ou auto |
| Signature e-mail (S/MIME) | Utilisateur | Oui |

---

## 50. Renouvellement et révocation (CRL, OCSP)

**Renouvellement :** avec l'auto-enrollment (§49), les certificats se renouvellent seuls avant expiration. Sans auto-enrollment : demande manuelle via `certlm.msc` / `certmgr.msc` ou le portail `http://srv-ca/certsrv`.

**Révocation :**

```powershell
# Révoquer un certificat (ex. départ d'un salarié, clé compromise)
certutil -revoke <NuméroDeSérie> 1   # 1 = keyCompromise ; 0 = unspecified
# Publier immédiatement la CRL
certutil -crl

# Vérifier qu'un certificat est révoqué
certutil -verify -urlfetch C:\temp\cert.cer
```

**Codes de révocation :** 0 = non spécifié, 1 = clé compromise, 3 = affiliation changée, 5 = cessation d'activité.

**OCSP (Online Responder)** : pour les gros parcs, installer le répondeur en ligne (service de rôle `ADCS-Online-Cert`) afin d'éviter le téléchargement de CRL volumineuses.

> **Point de vigilance :** si la CRL expire ou est injoignable, les clients **rejettent** les certificats (hard-fail par défaut sur beaucoup d'applis). Surveiller la date d'expiration de la CRL comme un certificat.

---

## 51. Dépannage PKI

| Symptôme | Cause probable | Action |
|---|---|---|
| Auto-enrollment ne distribue rien | Modèle non publié / droits insuffisants | `Add-CATemplate` ; onglet Sécurité du modèle : groupe = Inscrire + Inscription auto |
| "Le serveur RPC n'est pas disponible" | DCOM bloqué | Pare-feu : autoriser DCOM/RPC entre client et AC |
| Certificat rejeté : révocation inconnue | CRL injoignable/expirée | `certutil -urlfetch -verify` ; republier la CRL ; vérifier le HTTP |
| La demande reste "en attente" | Approbation manuelle requise sur le modèle | Console AC → Demandes en attente → Délivrer ; ou passer le modèle en auto |
| `certreq` échoue | Modèle mal orthographié | `certutil -template` pour lister les noms exacts |
| LDAPS ne fonctionne pas | Certificat DC sans le bon modèle | Modèle "Domain Controller Authentication" + redémarrer le DC |

```powershell
# Diagnostic express PKI
certutil -ping -config "SRV-CA-01\ENTREPRISE-Issuing-CA-01"   # l'AC répond-elle ?
certutil -cainfo                                              # infos de l'AC locale
Get-ChildItem Cert:\LocalMachine\My | Format-Table Subject, NotAfter, Issuer  # certificats machine
```

---

---

## 52. Sauvegarde : Windows Server Backup

Windows Server Backup (WSB) = sauvegarde locale correcte pour petits parcs ou appoint. Pas de déduplication avancée, pas de console centrale native.

```powershell
Install-WindowsFeature -Name Windows-Server-Backup -IncludeManagementTools

# Planifier une sauvegarde complète quotidienne à 22h vers un disque dédié
wbadmin enable backup -addtarget:E: -schedule:22:00 -allCritical -systemState -vssFull -quiet

# Sauvegarde bare metal (récupération complète sur matériel différent)
wbadmin start backup -backupTarget:E: -allCritical -systemState -bareMetalRecovery -quiet

# Sauvegarde immédiate d'un volume
wbadmin start backup -backupTarget:\\SRV-NAS-01\sav$ -include:D: -quiet

# Lister les sauvegardes disponibles
wbadmin get versions
wbadmin get disks
```

**Bonnes pratiques WSB :**
- Disque **dédié** (jamais le volume sauvegardé), de préférence **hors site** en rotation.
- `-vssFull` tronque les journaux applicatifs (SQL/Exchange) ; `-vssCopy` ne tronque pas.
- Tester la restauration (voir §55). Toujours.

---

## 53. Planifications, bare metal, restauration

**Planification type :**

| Quoi | Fréquence | Destination |
|---|---|---|
| État système + volumes critiques | Quotidien 22h | Disque USB en rotation (3 disques) |
| Bare Metal Recovery | Hebdo (dimanche) | NAS + disque externe |
| VM Hyper-V (hôte) | Quotidien | Stockage dédié |

```powershell
# Restaurer un fichier précis
wbadmin start recovery -version:09/26/2026-22:00 -itemType:File `
  -items:D:\Partages\Compta\budget.xlsx -recoveryTarget:D:\Restauration -quiet

# Restaurer un volume complet
wbadmin start recovery -version:09/26/2026-22:00 -itemType:Volume `
  -items:D: -recoveryTarget:D: -quiet

# État système (restauration AD sur un DC : voir mode DSRM)
wbadmin start systemstaterecovery -version:09/26/2026-22:00 -quiet

# Bare metal : démarrer sur le support d'installation → Réparer → Restauration d'image système
```

> **Restauration d'un contrôleur de domaine :** démarrer en **mode DSRM** (F8), restaurer l'état système, puis choisir restauration **autoritative** (`ntdsutil`) ou non selon le scénario. En pratique : si un autre DC sain existe, préférer **reconstruire** le DC plutôt que restaurer.

---

## 54. Stratégie 3-2-1 et rotation des supports

**3-2-1 :** 3 copies des données, sur 2 supports différents, dont 1 hors site.

| Support | Rôle | Rotation |
|---|---|---|
| Disque interne/dédié | Sauvegarde quotidienne rapide | Permanent |
| Disques USB chiffrés (×3) | Rotation hebdomadaire | LUN → MAR → MER, 1 hors site |
| NAS / cloud | Copie distante | Quotidien/hebdo |

```powershell
# Chiffrer le disque de sauvegarde (BitLocker) — obligatoire si le disque sort du site
Enable-BitLocker -MountPoint "E:" -PasswordProtector   # mot de passe FICTIF à la demande
# Ou via manage-bde :
manage-bde -on E: -pw
```

**Règle d'or :** une sauvegarde dont on n'a jamais testé la restauration est une **hypothèse**, pas une sauvegarde.

---

## 55. Restauration : scénarios pas à pas

**Scénario A — fichier supprimé par un utilisateur :**
1. Clic droit sur le dossier parent → **Propriétés → Versions précédentes** (clichés instantanés VSS à activer, voir ci-dessous).
2. Ou `wbadmin start recovery` (§53).

```powershell
# Activer les clichés instantanés (VSS) sur un volume de partages : 2×/jour
vssadmin add shadowstorage /for=D: /on=D: /maxsize=10%
# Planifier via tâche planifiée : wmic shadowcopy call create Volume='D:\'
schtasks /create /tn "VSS D midi" /tr "wmic shadowcopy call create Volume='D:\'" /sc daily /st 12:00 /ru SYSTEM
schtasks /create /tn "VSS D soir" /tr "wmic shadowcopy call create Volume='D:\'" /sc daily /st 18:00 /ru SYSTEM
```

**Scénario B — serveur HS, matériel OK :** restauration bare metal depuis le support d'installation.

**Scénario C — ransomware :** ne PAS restaurer sur l'infrastructure compromise. Isoler, reconstruire sur sain, restaurer depuis la copie **hors site** (la seule dont on est sûr qu'elle n'est pas chiffrée), changer tous les mots de passe.

---

## 56. Veeam et alternatives : quand passer au niveau supérieur

WSB suffit pour : 1–3 serveurs, budget nul, besoins simples.

**Passer à Veeam Backup & Replication (ou équivalent) quand :**
- Parc > 5 serveurs / dizaines de VM
- Besoin de réplication, de SureBackup (test auto des restaurations), de console centrale
- RPO/RTO contractuels (PRA/PCA)
- Sauvegarde applicative fine (SQL, AD, Exchange)

**Alternatives :** Nakivo, Altaro (PME), Bacula/Bareos (open source, voir guide Debian/Ubuntu), Azure Backup (cloud).

> Même avec Veeam : garder la règle 3-2-1, chiffrer, tester les restaurations, documenter le PRA.

---

## 57. Licences : KMS vs ADBA

Deux modes d'activation en volume pour Windows Server (et Windows 10/11) :

| | **KMS** (Key Management Service) | **ADBA** (Active Directory-Based Activation) |
|---|---|---|
| Principe | Un hôte KMS active les clients (port 1688) | L'activation est stockée dans l'AD |
| Seuil d'activation | 25 postes / 5 serveurs | Aucun seuil |
| Renouvellement | Tous les 180 jours (contact KMS) | Persistant (tant que joint au domaine) |
| Hors domaine | Fonctionne | Ne fonctionne pas |
| Idéal | Parc mixte, workgroup | Parc 100 % joint au domaine (le cas courant) |

```powershell
# --- KMS : installer l'hôte ---
slmgr.vbs /ipk XXXXX-XXXXX-XXXXX-XXXXX-XXXXX   # clé hôte KMS (fournie par Microsoft/contrat)
slmgr.vbs /ato                                 # activer l'hôte en ligne
# Publier dans le DNS : automatique (enregistrement _vlmcs._tcp)
# Forcer la publication :
slmgr.vbs /cdns

# --- Côté client : pointer vers le KMS ---
slmgr.vbs /skms srv-kms-01.entreprise.lan:1688
slmgr.vbs /ato
slmgr.vbs /dlv    # détail de licence

# --- ADBA : une seule fois dans la forêt ---
# Sur un DC : installer la clé via l'outil d'activation en volume (VAMT) ou :
slmgr.vbs /ipk <clé ADBA>   # sur le serveur qui héberge le service (souvent le DC)
# Les clients joints au domaine s'activent automatiquement, sans seuil.
```

> Depuis Windows Server 2012 R2, **ADBA est le choix par défaut** pour un parc joint au domaine. KMS reste utile pour les DMZ/workgroup.

---

## 58. CAL utilisateur vs appareil, inventaire

**CAL Windows Server** (Client Access License) : chaque utilisateur **ou** appareil accédant au serveur doit être couvert.

| | Par utilisateur | Par appareil |
|---|---|---|
| Compte | Chaque **personne** | Chaque **terminal** |
| Idéal | 1 personne, N appareils (nomades) | N personnes, 1 poste (3×8, ateliers) |
| Mixable | **Non** : un seul mode par... en pratique on choisit le mode majoritaire et on documente |

**Rôles exigeant des CAL supplémentaires :** RDS (§45), parfois SQL Server.

```powershell
# Inventaire : lister les serveurs et leurs licences (base d'un audit)
Get-ADComputer -Filter { OperatingSystem -like "Windows Server*" } -Property OperatingSystem |
  Group-Object OperatingSystem | Format-Table Name, Count -AutoSize

# Vérifier l'état d'activation de tout le parc (via CIM)
$serveurs = Get-ADComputer -Filter { OperatingSystem -like "Windows Server*" } | Select-Object -ExpandProperty Name
Invoke-Command -ComputerName $serveurs -ScriptBlock {
  (Get-CimInstance SoftwareLicensingProduct | Where-Object { $_.PartialProductKey }).LicenseStatus
} | Group-Object | Format-Table Name, Count
# LicenseStatus : 1 = Licensed (activé)
```

**Documenter :** un tableau `Serveur | Édition | Clé (coffre) | Mode activation | CAL affectées`. Un contrôle de conformité sans inventaire = amende assurée.

---

## 59. Activation : slmgr, DISM, dépannage

```powershell
# Les commandes slmgr.vbs à connaître
slmgr.vbs /dli          # résumé licence
slmgr.vbs /dlv          # détail (canal : Retail/OEM/Volume:GVLK)
slmgr.vbs /xpr          # date d'expiration (KMS : 180 jours renouvelables)
slmgr.vbs /ipk <clé>    # installer une clé
slmgr.vbs /ato          # activer en ligne
slmgr.vbs /upk          # désinstaller la clé
slmgr.vbs /skms <srv>   # définir le KMS

# Activation par téléphone (serveur isolé) :
slmgr.vbs /dti          # affiche l'ID d'installation à dicter

# Changer d'édition avec DISM (voir §2)
DISM /Online /Get-TargetEditions
```

| Erreur | Sens | Action |
|---|---|---|
| `0xC004F074` | KMS introuvable | DNS `_vlmcs._tcp` ? Port 1688 ? `slmgr /skms` correct ? |
| `0xC004C008` | Clé déjà utilisée (quota MAK) | Demander une extension de quota / passer en KMS |
| `0x80072EE7` | Pas de réseau / DNS | Vérifier connectivité vers Microsoft ou KMS |
| Délai de grâce expiré | Non activé | Activer sous 30 jours (notifications puis restrictions) |

---

## 60. Durcissement de base (hardening)

**Les 15 premiers gestes sur tout serveur :**

```powershell
# 1. Désactiver SMBv1 (rançongiciels type WannaCry)
Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol -NoRestart

# 2. Désactiver les protocoles obsolètes (LLMNR, NetBIOS si possible)
# GPO : Configuration ordinateur → Modèles d'administration → Réseau → Client DNS → Désactiver la résolution multidiffusion (LLMNR)
Set-ItemProperty "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\DNSClient" -Name EnableMulticast -Value 0 -Type DWord -Force

# 3. Renommer le compte Administrateur intégré (GPO)
Rename-LocalUser -Name "Administrator" -NewName "AdminLocal-XYZ"

# 4. Désactiver le compte Invité
Disable-LocalUser -Name "Guest"

# 5. Verrouillage de compte (GPO : Stratégie de sécurité locale)
net accounts /lockoutthreshold:5 /lockoutduration:30 /lockoutwindow:30

# 6. Mot de passe : longueur mini 14 (GPO)
net accounts /minpwlen:14

# 7. Désactiver les services inutiles
$servicesInutiles = @("XblGameSave","XboxGipSvc","Print Spooler")  # adapter ! spooler à garder sur SRV-PRINT
foreach ($s in $servicesInutiles) { Set-Service -Name $s -StartupType Disabled -ErrorAction SilentlyContinue }

# 8. RDP : NLA obligatoire + groupe restreint
Set-ItemProperty "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name UserAuthentication -Value 1

# 9. Désactiver le partage administratif C$ si non nécessaire (avec prudence)
# HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters → AutoShareWks=0 (postes) / AutoShareServer=0 (serveurs)

# 10. UAC au maximum (serveurs avec Desktop)
Set-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name ConsentPromptBehaviorAdmin -Value 2
```

**Suites :** appliquer un **baseline** (Microsoft Security Compliance Toolkit : GPO "MS Security Guide"), auditer avec **PingCastle** (AD) et **Purview/Secure Score**.

> Avertissement version : les chemins de registre GPO évoluent peu, mais vérifie les noms de stratégies sur 2025 (quelques renommages, ex. "Windows Defender" → "Microsoft Defender").

---

## 61. Pare-feu Windows Defender : règles et profils

3 profils : **Domaine** (le plus permissif, réseau AD), **Privé**, **Public** (le plus restrictif).

```powershell
# État des profils
Get-NetFirewallProfile | Format-Table Name, Enabled, DefaultInboundAction -AutoSize

# Créer une règle : autoriser SMB entrant depuis le LAN uniquement
New-NetFirewallRule -DisplayName "SMB - LAN uniquement" -Direction Inbound `
  -Protocol TCP -LocalPort 445 -RemoteAddress 192.168.10.0/24 -Action Allow -Profile Domain

# Autoriser WinRM (admin à distance)
Enable-NetFirewallRule -DisplayName "Windows Remote Management (HTTP-In)"

# Bloquer un exécutable sortant (ex. télémétrie d'une appli)
New-NetFirewallRule -DisplayName "Blocage appli X" -Direction Outbound `
  -Program "C:\Program Files\X\x.exe" -Action Block

# Auditer : règles actives, triées
Get-NetFirewallRule -Enabled True -Direction Inbound |
  Get-NetFirewallPortFilter | Format-Table Protocol, LocalPort -AutoSize

# Exporter / importer la configuration (pour déploiement)
netsh advfirewall export "D:\Securite\parefeu.wfw"
netsh advfirewall import "D:\Securite\parefeu.wfw"
```

**Règle d'or :** `DefaultInboundAction = Block` sur les profils Public/Privé, n'ouvrir que le nécessaire, **scoper par IP source** (jamais "tout Internet" sauf besoin prouvé comme la passerelle RDS en 443).

---

## 62. Microsoft Defender Antivirus sur serveur

```powershell
# État
Get-MpComputerStatus | Select-Object AntivirusEnabled, AntispywareEnabled, AMEngineVersion, AntivirusSignatureLastUpdated

# Mise à jour des signatures
Update-MpSignature

# Scan rapide / complet
Start-MpScan -ScanType QuickScan

# Exclusions OBLIGATOIRES par rôle (exemples) :
# Hyper-V : dossiers VM + vmms.exe, vmwp.exe
Add-MpPreference -ExclusionPath "D:\VMs"
Add-MpPreference -ExclusionProcess "C:\Windows\System32\vmms.exe"
# AD/DNS : NTDS, SYSVOL
Add-MpPreference -ExclusionPath "C:\Windows\NTDS", "C:\Windows\SYSVOL"
# SQL : .mdf/.ldf/.bak + dossier data
Add-MpPreference -ExclusionExtension "mdf","ldf","bak"

# Protection en temps réel (ne JAMAIS désactiver durablement)
Set-MpPreference -DisableRealtimeMonitoring $false

# Voir les menaces détectées
Get-MpThreatDetection | Format-Table ThreatID, Resources, InitialDetectionTime -AutoSize
```

> Sur Server Core, Defender se pilote en PowerShell ou via GPO/WAC. La désinstallation de Defender au profit d'un tiers se fait proprement (pas de "désactivation sauvage").

---

## 63. LAPS et gestion des mots de passe locaux

**Problème :** même mot de passe Administrateur local sur 50 serveurs = 1 compromission = tout le parc.

**LAPS (Local Administrator Password Solution)** : mot de passe unique, aléatoire, rotatif, stocké dans l'AD.

```powershell
# Installer LAPS (fonctionnalité + GPO côté admin)
# 1. Étendre le schéma AD (une fois, sur le DC) :
Update-AdmPwdADSchema
# 2. Déléguer le droit d'écrire le mot de passe aux ordinateurs (sur l'OU) :
Set-AdmPwdComputerSelfPermission -OrgUnit "OU=Serveurs,DC=entreprise,DC=lan"
# 3. Déléguer la lecture aux admins :
Set-AdmPwdReadPasswordPermission -OrgUnit "OU=Serveurs,DC=entreprise,DC=lan" -AllowedPrincipals "ENTREPRISE\GRP-Admins-LAPS"
# 4. GPO : Modèles d'administration → LAPS → activer + complexité + durée (30 jours)

# Lire le mot de passe d'un serveur :
Get-AdmPwdPassword -ComputerName SRV-FICHIER-01

# Forcer la rotation :
Reset-AdmPwdPassword -ComputerName SRV-FICHIER-01
```

> **Windows LAPS** (intégré depuis 2023, natif sur 2025) remplace le LAPS legacy : GPO `Configuration ordinateur → Modèles d'administration → Système → LAPS`, stockage dans l'AD ou Entra ID, chiffrement possible. En 2025, utiliser **Windows LAPS natif**.

---

## 64. Audit, journaux et supervision minimale

```powershell
# Activer l'audit des échecs d'ouverture de session + accès objets (GPO recommandé, ici en local)
auditpol /set /subcategory:"Logon" /success:enable /failure:enable
auditpol /set /subcategory:"File System" /failure:enable

# Taille des journaux (éviter l'écrasement en 2 jours)
Limit-EventLog -LogName Security -MaximumSize 1GB -OverflowAction OverwriteAsNeeded

# Recherches utiles
Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4625} -MaxEvents 20 |  # échecs de logon
  Format-Table TimeCreated, @{N='Compte';E={$_.Properties[5].Value}}, @{N='IP';E={$_.Properties[19].Value}} -AutoSize
Get-WinEvent -FilterHashtable @{LogName='System'; ID=1074,1076} -MaxEvents 10  # arrêts/redémarrages

# Transférer vers un collecteur (WEF - Windows Event Forwarding, gratuit)
# Sur le collecteur :
wecutil qc /q
# Abonnement : journaux Security/System des serveurs → collecteur central
```

**Supervision minimale viable :** ping + ports (445, 3389, 8530...) + espace disque + journaux critiques → Zabbix (voir `zabbix_guide.md`), avec remontée SNMP ou agent.

---

---

## 65. Cas pratiques commentés (1 à 5)

### Cas n°1 — Migrer un serveur de fichiers 2019 → 2022 sans coupure visible
**Contexte :** `SRV-FICHIER-01` (2019) saturé, nouveau `SRV-FICHIER-03` (2022).
1. Installer le rôle Fichiers + DFS-N/DFS-R sur le nouveau.
2. Créer le groupe de réplication DFS-R, nouveau serveur en membre (pas primaire).
3. Laisser la réplication initiale se faire (surveiller le backlog, §34).
4. Ajouter `\\SRV-FICHIER-03\partage` comme **cible DFS-N** (désactivée d'abord).
5. Basculer : activer la nouvelle cible, désactiver l'ancienne. Les utilisateurs via `\\entreprise.lan\data\...` ne voient rien.
6. Après 1 semaine de validation : décommissionner l'ancien.
*Leçon : DFS-N (§33) rend les migrations de serveurs de fichiers transparentes.*

### Cas n°2 — Le WSUS ne propose plus rien depuis 3 mois
**Diagnostic :** console WSUS → Synchronisations : dernière réussie il y a 92 jours. **Cause :** le disque D: plein à 100 % (contenu WSUS + logs IIS). **Actions :** libérer de l'espace, `wsusutil reset`, relancer la synchro, mettre en place le nettoyage mensuel (§28) + alerte disque à 85 % dans Zabbix. *Leçon : superviser l'espace disque du WSUS comme un serveur de production.*

### Cas n°3 — Un copieur n'imprime plus après changement de switch
**Diagnostic :** file "hors connexion". `Test-NetConnection 192.168.20.50 -Port 9100` → échec. **Cause :** le copieur était en DHCP, le nouveau switch a un autre VLAN par défaut → le copieur a changé d'IP. **Actions :** IP fixe sur le copieur (ou réservation DHCP), corriger le port TCP/IP sur le serveur d'impression, documenter les IP des copieurs. *Leçon : IP fixe ou réservation DHCP sur TOUS les périphériques d'impression (§42).*

### Cas n°4 — Ransomware : que faire à 3h du matin ?
1. **Isoler** : débrancher le réseau des serveurs touchés (pas d'arrêt brutal si possible — la mémoire contient des preuves).
2. **Ne pas payer**, ne pas "nettoyer" avant d'avoir des copies des logs.
3. Identifier le patient zéro (journaux 4624/4625, EDR).
4. Reconstruire sur infrastructure **saine** (nouveaux disques/VM).
5. Restaurer depuis la copie **hors site** 3-2-1 (§54).
6. Changer **tous** les mots de passe (admin, services, utilisateurs).
7. Post-mortem : comment est-il entré ? (RDP exposé ? phishing ? SMBv1 ?)
*Leçon : le PRA se prépare avant, pas pendant.*

### Cas n°5 — Ajouter un hôte Hyper-V au cluster un dimanche
1. Mêmes patchs que les nœuds existants (sinon `Test-Cluster` échoue).
2. Mêmes vSwitch (noms identiques !), mêmes réseaux/VLAN.
3. `Add-ClusterNode -Name SRV-HV-03 -Cluster CLUSTER-HV`.
4. Vérifier le quorum et migrer une VM de test dessus.
*Leçon : l'homogénéité des nœuds (noms de vSwitch, patchs) est la condition n°1 d'un cluster sain.*

---

## 66. Cas pratiques commentés (6 à 10)

### Cas n°6 — Déployer 200 postes avec une imprimante par défaut selon l'étage
GPO par OU/étage : déploiement **par ordinateur** (§39) du copieur d'étage + script `SetDefaultPrinter`. Filtrage WMI ou groupes de sécurité par étage. Tester avec `gpupdate /force` + redémarrage sur 2 postes pilotes.

### Cas n°7 — Mettre en place la PKI pour le Wi-Fi d'entreprise (802.1X)
1. AC subordonnée (§48), modèles "Ordinateur" et "Utilisateur" avec auto-enrollment (§49).
2. GPO auto-enrollment sur OU Postes et OU Utilisateurs.
3. NPS (serveur de stratégies réseau) + certificats serveur.
4. SSID WPA2/3-Enterprise, validation du certificat serveur côté client.
*Leçon : sans PKI interne, pas de 802.1X sérieux.*

### Cas n°8 — Le serveur RDS refuse les connexions le lundi matin
**Message :** "Aucun serveur de licences...". **Cause :** délai de grâce de 120 jours expiré pendant les vacances (§45). **Action immédiate :** installer/configurer le rôle RDS-Licensing + CAL. **Préventif :** alerte à J+100 dans l'agenda d'exploitation.

### Cas n°9 — Espace disque critique sur le volume des VM
1. Identifier : `Get-VM | Get-VMHardDiskDrive` + tailles VHDX ; checkpoints oubliés (`Get-VMSnapshot`).
2. Supprimer/fusionner les vieux checkpoints (hors heures de pointe — la fusion consomme des I/O).
3. Compacter les VHDX dynamiques (`Optimize-VHD -Mode Full`, VM éteinte).
4. Déplacer une VM vers un autre volume (`Move-VMStorage`).
*Leçon : alerte disque à 80 % sur les volumes Hyper-V, pas à 95 %.*

### Cas n°10 — Restaurer un fichier écrasé par erreur il y a 2 jours
1. Versions précédentes (VSS §55) : clic droit → restaurer la version d'il y a 2 jours.
2. Si VSS non activé : `wbadmin start recovery` depuis la sauvegarde de la veille.
3. Vérifier l'intégrité avec l'utilisateur avant de clore.
*Leçon : activer VSS 2×/jour sur tous les volumes de partages — ça sauve plus de temps que n'importe quel outil.*

---

## 67. Cas pratiques commentés (11 à 15)

### Cas n°11 — Sécuriser le RDP exposé pour 3 administrateurs nomades
Ne **jamais** exposer le 3389 sur Internet. Solution : passerelle RDS (§43) ou VPN (WireGuard, voir guide Debian/Ubuntu) + RDP interne + NLA + comptes nominatifs + LAPS (§63) + audit 4625. Alternative moderne : Windows Admin Center publié via reverse proxy avec MFA.

### Cas n°12 — Migrer DHCP vers un nouveau serveur sans perdre les baux
1. `Export-DhcpServer -File C:\dhcp.xml -Leases` sur l'ancien.
2. `Import-DhcpServer -File C:\dhcp.xml -Leases -BackupPath C:\dhcp-backup` sur le nouveau.
3. Autoriser le nouveau dans l'AD, désautoriser l'ancien, vérifier les étendues et les réservations (copieurs ! §42).

### Cas n°13 — Un disque du pool Storage Spaces passe en "retired"
1. `Get-PhysicalDisk | Where-Object HealthStatus -ne "Healthy"` → identifier.
2. Ajouter le disque de remplacement au pool, `Repair-VirtualDisk`.
3. Retirer l'ancien (`Remove-PhysicalDisk`) après resync complète.
4. **Ne jamais** retirer un disque avant la fin de la réparation.

### Cas n°14 — Déployer LAPS sur 80 serveurs existants
1. Étendre le schéma (§63), délégations sur les OU.
2. GPO Windows LAPS (2025 : natif) liée aux OU Serveurs.
3. Vérifier : `Get-LapsADPassword -Identity SRV-X-01` sur un échantillon.
4. Révoquer les anciens mots de passe locaux partagés.

### Cas n°15 — Préparer l'audit de conformité licences
1. Inventaire (§58) : serveurs, éditions, VM par hôte.
2. Calculer : licences Standard/Datacenter requises vs achetées.
3. Vérifier les CAL (utilisateurs/appareils) vs effectif.
4. Documenter KMS/ADBA et les clés dans le coffre.
*Leçon : l'audit se prépare en continu, pas la veille du contrôle.*

---

## 68. Cas pratiques commentés (16 à 18)

### Cas n°16 — Panne du PDC : l'heure dérive sur tout le domaine
**Symptômes :** échecs Kerberos aléatoires ("l'horloge n'est pas synchronisée"). **Cause :** le PDC émulateur pointait vers une source NTP morte. **Action :** `w32tm /config /manualpeerlist:"0.fr.pool.ntp.org,1.fr.pool.ntp.org" /syncfromflags:manual /reliable:yes /update` sur le PDC, `w32tm /resync` sur les membres. **Préventif :** superviser le décalage NTP (Zabbix : `system.localtime` vs référence).

### Cas n°17 — Un service métier ne démarre plus après patch Tuesday
1. Identifier le patch (historique : `Get-HotFix | Sort-Object InstalledOn -Descending`).
2. Si régression avérée : `wusa /uninstall /kb:XXXXXXX` (avec fenêtre de maintenance).
3. Bloquer le KB sur WSUS (refuser l'approbation, §27) en attendant le correctif.
4. Tester les patchs sur le groupe `Serveurs-Test` avant la prod (toujours).

### Cas n°18 — Dimensionner un hôte Hyper-V pour 12 VM
Méthode : inventorier les besoins (vCPU/RAM/disque par VM), appliquer les ratios (§23 : ~8:1 vCPU/pCPU max, RAM = somme + 20 % pour l'hôte), prévoir la croissance 3 ans (+30 %). Exemple : 12 VM × (2 vCPU, 4 Go) → 24 vCPU → 8 cœurs physiques (ratio 3:1, confortable) ; RAM : 48 Go + 8 Go hôte + marge → 64 Go ; stockage : 12 × 100 Go + marge → 2 To utiles en RAID/miroir. Licence : Datacenter (12 VM > 6, §2).

---

## 69. Erreurs classiques (1 à 5)

1. **Cloner une VM sans sysprep** → doublons de SID/SusClientId : WSUS voit 1 client, l'AD se mélange. Toujours sysprep ou déployer depuis un template propre.
2. **Mettre des permissions NTFS à des utilisateurs nommés** au lieu de groupes → ingérable au premier départ. Groupes AD systématiques (§31).
3. **Oublier le délai de grâce RDS de 120 jours** → plus personne ne se connecte un lundi matin (§45).
4. **Un checkpoint Hyper-V vieux de 6 mois** considéré comme "sauvegarde" → chaîne AVHDX corrompue un jour = VM perdue (§20).
5. **IP en DHCP sur un serveur, un copieur, une imprimante** → l'IP change, tout casse. Statique ou réservation, sans exception (§6, §42).

---

## 70. Erreurs classiques (6 à 10)

6. **Sauvegarder sans jamais tester la restauration** → le jour J, la sauvegarde est vide/corrompue/illisible (§54).
7. **Ouvrir le RDP (3389) sur Internet** → brute-force en quelques heures. Passerelle/VPN uniquement (§60).
8. **Désactiver le pare-feu Windows "pour que ça marche"** → ouvrir le port précis au lieu de tout ouvrir (§61).
9. **Installer des rôles applicatifs sur un contrôleur de domaine** (fichiers, impression, IIS...) → DC = AD/DNS/DHCP, rien d'autre.
10. **Ne pas documenter** (mots de passe dans un tableur, pas d'inventaire) → le jour où l'admin part, c'est le black-out. GLPI + coffre à mots de passe (§6).

---

## 71. Erreurs classiques (11 à 15)

11. **Même mot de passe admin local partout** → LAPS (§63), sans débat.
12. **WSUS sans maintenance** → base de 15 Go, console inutilisable, clients non patchés (§28).
13. **Approuver toutes les mises à jour pour tout le monde d'un coup** → un patch foireux = tout le parc HS. Anneaux Test → Pilotes → Prod (§27).
14. **Désactiver SMBv1... sur le papier seulement** : vérifier avec `Get-SmbServerConfiguration`, car certains vieux copieurs/scanners exigent encore SMBv1 → isoler ces équipements sur un VLAN dédié plutôt que réactiver SMBv1 partout.
15. **Oublier de sauvegarder la configuration** (GPO, DHCP, DNS, certificats AC) → on sauvegarde les données mais pas la conf, et la reconstruction prend 3 jours. Exporter régulièrement (`Backup-GPO -All`, `Export-DhcpServer`, sauvegarde de l'AC via `certutil -backup`).

---

## 72. Erreurs classiques (16 à 18)

16. **Stocker les VM sur C:** → le système sature, l'hôte plante. Volume dédié (§23).
17. **Ne pas séparer les réseaux du cluster** (heartbeat + VM + CSV sur le même lien) → bascules fantômes en pleine journée (§22).
18. **Faire confiance à la corbeille AD par défaut** : activer la **corbeille AD** (`Enable-ADOptionalFeature -Identity "CN=Recycle Bin Feature,CN=Optional Features,CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=entreprise,DC=lan" -Scope ForestOrConfigurationSet -Target "entreprise.lan"`) pour restaurer un objet supprimé sans sueur froide.

---

---

## 73. Checklist d'exploitation quotidienne

- [ ] Sauvegardes de la nuit : statut OK (console Veeam/WSB, e-mail de rapport)
- [ ] Espace disque : aucun volume > 85 % (Zabbix ou script)
- [ ] Journaux critiques : erreurs System/Application dernières 24 h
- [ ] Réplication : Hyper-V Replica / DFS-R backlog nominal
- [ ] Cluster : tous les nœuds `Up`, ressources `Online`
- [ ] Antivirus : signatures à jour, aucune menace non traitée
- [ ] Files d'impression : aucune file en erreur massive (spooler OK)

```powershell
# Rapport quotidien express (à mettre en tâche planifiée + envoi e-mail)
$rapport = @()
$rapport += "=== Disques > 80% ==="
$rapport += Get-CimInstance Win32_LogicalDisk -Filter "DriveType=3" |
  Where-Object { ($_.FreeSpace / $_.Size) -lt 0.2 } |
  ForEach-Object { "$($_.DeviceID) libre : $([math]::Round($_.FreeSpace/1GB,1)) Go" }
$rapport += "=== Erreurs System 24h ==="
$rapport += (Get-WinEvent -FilterHashtable @{LogName='System'; Level=1,2; StartTime=(Get-Date).AddDays(-1)} -MaxEvents 20 |
  ForEach-Object { "$($_.TimeCreated) [$($_.Id)] $($_.ProviderName)" })
$rapport | Out-File "D:\Rapports\quotidien-$(Get-Date -Format yyyy-MM-dd).txt"
```

---

## 74. Checklist hebdomadaire

- [ ] Vérifier les sauvegardes hors site / rotation des disques USB
- [ ] WSUS : nouvelles mises à jour critiques à approuver (anneau Test)
- [ ] VSS : clichés bien créés sur les volumes de partages
- [ ] Redémarrage des serveurs non critiques si patchs en attente
- [ ] Revoir les comptes verrouillés (4625) : attaque ou utilisateur étourdi ?
- [ ] Nettoyer les vieux checkpoints Hyper-V (> 7 jours)
- [ ] Vérifier les files d'impression + niveaux de toner (croiser avec la supervision copieurs)

---

## 75. Checklist mensuelle

- [ ] Patch Tuesday : déployer Test → Pilotes → Production (§27)
- [ ] Server Cleanup Wizard WSUS (§28)
- [ ] Réindexer la base WSUS si la console rame
- [ ] Vérifier les certificats expirant dans < 90 jours (PKI + serveurs web)
- [ ] Revoir les quotas FSRM et l'espace des partages
- [ ] Test de restauration : 1 fichier + 1 VM (alterner les serveurs)
- [ ] Revoir les droits NTFS des partages sensibles (qui a accès à la Compta ?)
- [ ] Mettre à jour la documentation (GLPI) : nouveaux serveurs, changements

```powershell
# Certificats expirant dans moins de 90 jours (machine locale)
Get-ChildItem Cert:\LocalMachine\My |
  Where-Object { $_.NotAfter -lt (Get-Date).AddDays(90) } |
  Format-Table Subject, NotAfter -AutoSize

# Idem sur l'AC : certificats émis expirant bientôt
certutil -view -restrict "NotAfter<$( (Get-Date).AddDays(90).ToString('MM/dd/yyyy') )" -out "CommonName,NotAfter"
```

---

## 76. Checklist trimestrielle / annuelle

**Trimestrielle :**
- [ ] Test de restauration **complète** (bare metal d'un serveur non critique)
- [ ] Revoir les GPO : lesquelles sont encore utiles ? (désactiver les mortes)
- [ ] Audit des comptes à privilèges (Domain Admins : qui ? pourquoi ?)
- [ ] Vérifier le quorum du cluster et le témoin
- [ ] Rotation des mots de passe de service critiques

**Annuelle :**
- [ ] Inventaire licences complet (§58) — avant le renouvellement des contrats
- [ ] Révision du PRA/PCA : le document est-il encore vrai ?
- [ ] Renouvellement du certificat de l'AC subordonnée si échéance < 2 ans
- [ ] Vérifier la CRL : publication OK, expiration lointaine (§50)
- [ ] Plan de remplacement matériel (garanties, fin de vie)
- [ ] Formation / habilitation de l'équipe sur les procédures

---

## 77. Checklist de mise en production d'un nouveau serveur

- [ ] Installation : édition LTSC, Core sauf justification (§7, §10)
- [ ] Nom conforme (`SRV-<ROLE>-<NN>`), IP statique, DNS, passerelle (§6)
- [ ] Domaine joint, OU correcte, GPO appliquées (`gpresult /r`)
- [ ] Fuseau horaire + NTP synchronisé
- [ ] Rôles installés **via script PowerShell versionné**
- [ ] Pare-feu : profil Domaine, règles minimales scopées (§61)
- [ ] Durcissement de base appliqué (§60), SMBv1 désactivé
- [ ] LAPS / Windows LAPS actif (§63)
- [ ] Mises à jour : WSUS ciblé sur le bon groupe (§26)
- [ ] Sauvegarde planifiée + testée (§52)
- [ ] Supervision : ajouté à Zabbix (ping, disque, services)
- [ ] Documentation GLPI : rôle, IP, garanties, mots de passe au coffre
- [ ] Recette : redémarrage propre, bascule testée si cluster

---

## 78. Pense-bête de poche : PowerShell

```powershell
# --- Système ---
Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion
Rename-Computer -NewName "SRV-X-01" -Restart
Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 5
systeminfo | Select-String "Durée"

# --- Réseau ---
Get-NetIPAddress -AddressFamily IPv4 | Where-Object PrefixOrigin -ne "WellKnown"
Test-NetConnection 192.168.10.1 -Port 445
Resolve-DnsName srv-ad-01.entreprise.lan
Get-NetFirewallProfile | Format-Table Name, Enabled

# --- Services & processus ---
Get-Service | Where-Object Status -eq "Running"
Restart-Service Spooler -Force
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5

# --- Disques ---
Get-PSDrive -PSProvider FileSystem
Get-Volume | Format-Table DriveLetter, FileSystem, SizeRemaining, Size -AutoSize
Clear-RecycleBin -Force   # (avec prudence)

# --- Utilisateurs & AD ---
Get-ADUser -Filter * -SearchBase "OU=Utilisateurs,DC=entreprise,DC=lan" | Measure-Object
Get-ADComputer -Filter { Enabled -eq $false } | Disable-ADAccount  # déjà désactivés : à déplacer/supprimer
Unlock-ADAccount -Identity "j.dupont"

# --- Journaux ---
Get-EventLog -LogName System -Newest 20 -EntryType Error
Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4625} -MaxEvents 10

# --- À distance ---
Enter-PSSession -ComputerName SRV-HV-01
Invoke-Command -ComputerName (Get-Content serveurs.txt) -ScriptBlock { Get-Service Spooler }
```

---

## 79. Pense-bête de poche : commandes et raccourcis

| Commande | Effet |
|---|---|
| `sconfig` | Menu de configuration (Server Core) |
| `slmgr.vbs /dlv` | Détail de la licence |
| `wbadmin get versions` | Sauvegardes disponibles |
| `dfsrdiag backlog ...` | Retard de réplication DFS-R |
| `certutil -ping -config ...` | L'AC répond-elle ? |
| `nltest /dclist:entreprise.lan` | Lister les DC du domaine |
| `dcdiag /v` | Diagnostic complet d'un DC |
| `repadmin /replsummary` | Résumé de la réplication AD |
| `gpresult /r` | GPO appliquées |
| `gpupdate /force` | Forcer l'application des GPO |
| `whoami /groups` | Groupes de l'utilisateur courant |
| `quser /server:SRV-RDS-01` | Sessions sur un hôte RDS |
| `vssadmin list shadows` | Clichés instantanés |
| `w32tm /query /status` | État de la synchro NTP |

---

## 80. Pense-bête de poche : ports réseau essentiels

| Port | Service |
|---|---|
| 53 TCP/UDP | DNS |
| 67/68 UDP | DHCP |
| 88 TCP/UDP | Kerberos (AD) |
| 123 UDP | NTP |
| 135 TCP | RPC (endpoint mapper) |
| 139 TCP | NetBIOS Session |
| 389 TCP/UDP | LDAP |
| 443 TCP | HTTPS (WAC, RD Gateway, WSUS SSL) |
| 445 TCP | SMB |
| 464 TCP/UDP | Kerberos password change |
| 636 TCP | LDAPS |
| 1433 TCP | SQL Server (défaut) |
| 3389 TCP | RDP |
| 5985 TCP | WinRM HTTP |
| 5986 TCP | WinRM HTTPS |
| 8530/8531 TCP | WSUS HTTP/HTTPS |
| 9100 TCP | Impression JetDirect (copieurs) |
| 1688 TCP | KMS |
| 8080 TCP | Réplication Hyper-V (Kerberos) |

---

## 81. Glossaire (A–M)

- **ABE** (Access-Based Enumeration) : l'utilisateur ne voit que les dossiers auxquels il a accès.
- **ADBA** : activation des licences via Active Directory, sans seuil.
- **CAL** : licence d'accès client (par utilisateur ou par appareil).
- **CSV** (Cluster Shared Volume) : volume partagé entre nœuds d'un cluster.
- **DC** (Domain Controller) : contrôleur de domaine Active Directory.
- **Dedup** : déduplication, élimine les blocs de données en double.
- **DFS-N / DFS-R** : espaces de noms distribués / réplication de fichiers.
- **DSRM** : mode de restauration des services d'annuaire (restauration DC).
- **FSRM** : gestionnaire de ressources de fichiers (quotas, filtrage).
- **GPO** : stratégie de groupe Active Directory.
- **Hyper-V Replica** : réplication asynchrone de VM entre hôtes.
- **KMS** : service de gestion des clés d'activation en volume.
- **LAPS** : mots de passe d'admin local uniques et rotatifs.
- **LTSC** : canal de support long (10 ans), la référence en production.

---

## 82. Glossaire (N–Z)

- **NTP** : protocole de synchronisation de l'heure (vital pour Kerberos/AD).
- **OCSP** : vérification en ligne du statut de révocation d'un certificat.
- **OSE** (Operating System Environment) : instance de système d'exploitation couverte par une licence.
- **PKI** : infrastructure à clés publiques (AC, certificats, CRL).
- **Quorum** : mécanisme de vote qui évite le split-brain dans un cluster.
- **RDS** : services Bureau à distance (ex-TSE).
- **RPO / RTO** : perte de données max acceptable / durée max d'interruption acceptable.
- **RSAT** : outils d'administration à distance (sur poste Windows).
- **SMB** : protocole de partage de fichiers Windows.
- **Spooler** : service gérant les files d'impression.
- **VSS** : clichés instantanés (versions précédentes).
- **WAC** (Windows Admin Center) : console web d'administration des serveurs.
- **WID** : base de données interne Windows (utilisée par WSUS).
- **WSB** : Windows Server Backup.
- **WSUS** : service de distribution des mises à jour Microsoft.

---

## 83. Quiz : 10 questions

1. Un hôte Hyper-V fait tourner 10 VM de production. Quelle édition de Windows Server est la plus économique, et pourquoi ?
2. Quelle est la différence fondamentale entre un checkpoint standard et un checkpoint de production ?
3. Un utilisateur a "Accès refusé" sur un partage alors qu'il est dans le bon groupe AD depuis ce matin. Quelle est la cause la plus probable ?
4. Pourquoi faut-il combiner DFS-N avec des chemins UNC directs vers les serveurs ?
5. Quelle est la durée du délai de grâce RDS sans serveur de licences, et que se passe-t-il après ?
6. Dans une PKI à deux niveaux, quel est le rôle de l'AC racine et combien de temps doit-elle rester allumée ?
7. Que signifie 3-2-1 et quelle erreur commet-on le plus souvent avec les sauvegardes ?
8. KMS ou ADBA : lequel choisir pour un parc 100 % joint au domaine, et pourquoi ?
9. Un client WSUS n'apparaît pas dans la console. Citez 3 vérifications dans l'ordre.
10. Pourquoi ne faut-il jamais exposer le port 3389 (RDP) directement sur Internet ?

---

## 84. Quiz : réponses commentées

1. **Datacenter.** Chaque licence Standard ne couvre que 2 VM ; pour 10 VM il faudrait 5 licences Standard, plus cher qu'une Datacenter (VM illimitées). Seuil de rentabilité ≈ 6 VM (§2).
2. Le checkpoint **standard** fige l'état mémoire exact (restauration à l'identique, plutôt lab) ; le checkpoint de **production** utilise VSS dans la VM pour une image cohérente au niveau applicatif (adapté à la prod). Ni l'un ni l'autre n'est une sauvegarde (§20).
3. **Le jeton d'accès** : l'appartenance au groupe a été ajoutée après l'ouverture de session. Il faut fermer/rouvrir la session (ou `klist purge` + reverrouillage) pour que le nouveau groupe soit pris en compte (§37).
4. Il ne faut **pas** utiliser de chemins directs : DFS-N fournit un chemin logique stable (`\\entreprise.lan\data\...`) qui survit aux migrations de serveurs. Les UNC directs (`\\SRV-01\...`) cassent à chaque migration (§33).
5. **120 jours.** Après, les connexions RDS sont refusées ("aucun serveur de licences disponible"). C'est la panne RDS la plus classique (§45).
6. L'AC **racine** signe uniquement l'AC subordonnée (1×/an ou à l'émission), puis est **éteinte et mise au coffre**. Elle ne doit jamais être en ligne en permanence : c'est l'AC subordonnée d'entreprise qui émet au quotidien (§47-48).
7. **3 copies, 2 supports différents, 1 hors site.** L'erreur la plus courante : ne jamais tester la restauration — une sauvegarde non testée est une hypothèse (§54).
8. **ADBA** : pas de seuil d'activation (KMS exige 25 postes/5 serveurs), activation persistante tant que la machine est jointe au domaine, rien à maintenir (§57).
9. (1) La GPO pointe-t-elle vers `http://srv-wsus:8530` ? (registre `WUServer`) ; (2) connectivité `Test-NetConnection -Port 8530` ; (3) `SusClientId` en doublon (clone sans sysprep) puis `wuauclt /resetauthorization /detectnow` et `UsoClient StartScan` (§29).
10. Le RDP exposé subit un **brute-force permanent** (bots) ; une faille ou un mot de passe faible = porte d'entrée (rançongiciels). Passer par VPN ou passerelle RDS en 443, avec NLA et comptes nominatifs (§60-61).

---

## 85. Pour aller plus loin : 20 ressources et chantiers

**Chantiers à planifier :**
1. Migrer les derniers 2019 vers 2022/2025 (support étendu = sécurité seule).
2. Déployer Windows LAPS natif sur tout le parc (§63).
3. Passer les hôtes Hyper-V en Server Core si ce n'est pas fait (§10).
4. Mettre en place la PKI à deux niveaux si absente (§47-48).
5. Tester une restauration bare metal par trimestre (§76).
6. Documenter le PRA et le faire valider par la direction.
7. Isoler les équipements exigeant SMBv1 sur un VLAN dédié (§60).
8. Remplacer les UNC directs par DFS-N partout (§33).
9. Auditer l'AD avec PingCastle (gratuit).
10. Chiffrer les disques de sauvegarde en rotation (BitLocker, §54).

**Ressources :**
11. Microsoft Learn — documentation Windows Server (référence officielle, à jour par version).
12. Microsoft Security Compliance Toolkit — baselines GPO de durcissement.
13. PingCastle — audit de la sécurité Active Directory.
14. Les guides compagnons : `onduleurs_ups_guide.md` (coupures → arrêt propre des serveurs : NUT), `proxmox_guide.md`, `zabbix_guide.md`, `debian_ubuntu_guide.md`, `kyocera_copieurs_guide.md` (métier copieurs ↔ serveur d'impression §38-42).
15. Veeam — guides de dimensionnement et best practices (même sans licence, la doc est pédagogique).
16. NIST SP 800-53 / CIS Benchmarks Windows Server — référentiels de durcissement.
17. TechNet Gallery / GitHub — scripts PowerShell communautaires (à relire avant usage).
18. Wireshark — comprendre SMB/Kerberos/LDAP quand le dépannage coince.
19. Laboratoire : 1 hôte Hyper-V + 3 VM (DC, fichiers, WSUS) pour tester chaque section de ce guide sans risque.
20. Ce guide lui-même : le relire une fois par an, cocher ce qui est appliqué, planifier le reste.

---

*Fin du guide — Windows Server en entreprise. Document de travail : adapte les noms de serveurs, IP et domaines à ton infrastructure réelle avant toute exécution.*
