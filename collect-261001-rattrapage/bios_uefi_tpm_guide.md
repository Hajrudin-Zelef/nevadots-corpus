# BIOS / UEFI — Secure Boot — TPM 2.0
## Guide technique ultra-complet pour l'entreprise

> **Public :** administrateurs systèmes, techniciens de parc, responsables d'infrastructure.
> **Objectif :** comprendre, configurer, sécuriser et dépanner le firmware des postes et serveurs Windows en entreprise.
> **Prérequis :** bases d'administration Windows (PowerShell, Active Directory), notions de partitionnement.
>
> ⚠️ **Avertissement :** les menus et noms d'options varient selon le constructeur (Dell, HP, Lenovo...) et la version du firmware. Les chemins indiqués sont des exemples représentatifs ; adaptez-les à votre modèle exact. Les manipulations du firmware (flash, Clear TPM, Clear CMOS) comportent des risques : lisez les précautions avant d'agir.

---

## Sommaire

1. [Le BIOS Legacy : l'héritage du PC](#1-le-bios-legacy--lh%C3%A9ritage-du-pc)
2. [L'UEFI : l'architecture moderne](#2-luefi--larchitecture-moderne)
3. [BIOS vs UEFI : comparatif détaillé](#3-bios-vs-uefi--comparatif-d%C3%A9taill%C3%A9)
4. [CSM (Compatibility Support Module)](#4-csm-compatibility-support-module)
5. [MBR : structure et fonctionnement](#5-mbr--structure-et-fonctionnement)
6. [GPT : structure et fonctionnement](#6-gpt--structure-et-fonctionnement)
7. [MBR vs GPT : limites et comparatif](#7-mbr-vs-gpt--limites-et-comparatif)
8. [Conversion MBR → GPT : mbr2gpt](#8-conversion-mbr--gpt--mbr2gpt)
9. [Cas d'usage : quand utiliser MBR ou GPT](#9-cas-dusage--quand-utiliser-mbr-ou-gpt)
10. [Séquence de boot UEFI pas à pas](#10-s%C3%A9quence-de-boot-uefi-pas-%C3%A0-pas)
11. [ESP (EFI System Partition)](#11-esp-efi-system-partition)
12. [UEFI Boot Manager et NVRAM](#12-uefi-boot-manager-et-nvram)
13. [BCD (Boot Configuration Data)](#13-bcd-boot-configuration-data)
14. [bcdedit : le couteau suisse du boot Windows](#14-bcdedit--le-couteau-suisse-du-boot-windows)
15. [bcdboot : reconstruire les fichiers de démarrage](#15-bcdboot--reconstruire-les-fichiers-de-d%C3%A9marrage)
16. [Secure Boot : principe général](#16-secure-boot--principe-g%C3%A9n%C3%A9ral)
17. [La chaîne de confiance du démarrage](#17-la-cha%C3%AEne-de-confiance-du-d%C3%A9marrage)
18. [Les clés Secure Boot : PK](#18-les-cl%C3%A9s-secure-boot--pk-platform-key)
19. [Les clés Secure Boot : KEK](#19-les-cl%C3%A9s-secure-boot--kek-key-exchange-key)
20. [Les clés Secure Boot : db (base de signatures autorisées)](#20-les-cl%C3%A9s-secure-boot--db-base-de-signatures-autoris%C3%A9es)
21. [Les clés Secure Boot : dbx (base de révocation)](#21-les-cl%C3%A9s-secure-boot--dbx-base-de-r%C3%A9vocation)
22. [Mode Standard vs mode Custom](#22-mode-standard-vs-mode-custom)
23. [Activer Secure Boot : procédure](#23-activer-secure-boot--proc%C3%A9dure)
24. [Vérifier Secure Boot : Confirm-SecureBootUEFI et msinfo32](#24-v%C3%A9rifier-secure-boot--confirm-securebootuefi-et-msinfo32)
25. [Mise à jour des clés et certificats Secure Boot](#25-mise-%C3%A0-jour-des-cl%C3%A9s-et-certificats-secure-boot)
26. [Secure Boot et Linux en entreprise](#26-secure-boot-et-linux-en-entreprise)
27. [Dépannage Secure Boot : boot bloqué](#27-d%C3%A9pannage-secure-boot--boot-bloqu%C3%A9)
28. [TPM 2.0 : principe et racine de confiance matérielle](#28-tpm-20--principe-et-racine-de-confiance-mat%C3%A9rielle)
29. [TPM 1.2 vs TPM 2.0](#29-tpm-12-vs-tpm-20)
30. [TPM discret vs TPM firmware (fTPM / Intel PTT)](#30-tpm-discret-vs-tpm-firmware-ftpm--intel-ptt)
31. [Activer le TPM dans le firmware](#31-activer-le-tpm-dans-le-firmware)
32. [Vérifier le TPM : Get-TPM et tpm.msc](#32-v%C3%A9rifier-le-tpm--get-tpm-et-tpmsc)
33. [Clear TPM : quand, pourquoi, précautions](#33-clear-tpm--quand-pourquoi-pr%C3%A9cautions)
34. [Provisioning et attestation du TPM](#34-provisioning-et-attestation-du-tpm)
35. [PCR : Platform Configuration Registers](#35-pcr--platform-configuration-registers)
36. [BitLocker et TPM : les protecteurs](#36-bitlocker-et-tpm--les-protecteurs)
37. [Pourquoi une MAJ du BIOS déclenche la récupération BitLocker](#37-pourquoi-une-maj-du-bios-d%C3%A9clenche-la-r%C3%A9cup%C3%A9ration-bitlocker)
38. [Gestion des clés de récupération BitLocker (AD / Entra ID)](#38-gestion-des-cl%C3%A9s-de-r%C3%A9cup%C3%A9ration-bitlocker-ad--entra-id)
39. [Windows 11 : exigences TPM 2.0 et Secure Boot](#39-windows-11--exigences-tpm-20-et-secure-boot)
40. [Inventaire du parc : script PowerShell de conformité](#40-inventaire-du-parc--script-powershell-de-conformit%C3%A9)
41. [Ordre de boot : configuration et bonnes pratiques](#41-ordre-de-boot--configuration-et-bonnes-pratiques)
42. [Virtualisation matérielle : VT-x / AMD-V et VT-d / AMD-Vi](#42-virtualisation-mat%C3%A9rielle--vt-x--amd-v-et-vt-d--amd-vi)
43. [Mot de passe BIOS/UEFI (admin / supervisor)](#43-mot-de-passe-biosuefi-admin--supervisor)
44. [Désactivation des périphériques : USB boot et ports](#44-d%C3%A9sactivation-des-p%C3%A9riph%C3%A9riques--usb-boot-et-ports)
45. [Wake-on-LAN (WoL)](#45-wake-on-lan-wol)
46. [PXE boot : principe](#46-pxe-boot--principe)
47. [WDS : installation du rôle](#47-wds--installation-du-r%C3%B4le)
48. [WDS : configuration initiale](#48-wds--configuration-initiale)
49. [WDS : images de démarrage et d'installation](#49-wds--images-de-d%C3%A9marrage-et-dinstallation)
50. [Dépannage PXE / WDS](#50-d%C3%A9pannage-pxe--wds)
51. [iPXE et boot réseau avancé](#51-ipxe-et-boot-r%C3%A9seau-avanc%C3%A9)
52. [Mises à jour du firmware : précautions générales](#52-mises-%C3%A0-jour-du-firmware--pr%C3%A9cautions-g%C3%A9n%C3%A9rales)
53. [Mise à jour BIOS/UEFI : Dell](#53-mise-%C3%A0-jour-biosuefi--dell)
54. [Mise à jour BIOS/UEFI : HP](#54-mise-%C3%A0-jour-biosuefi--hp)
55. [Mise à jour BIOS/UEFI : Lenovo](#55-mise-%C3%A0-jour-biosuefi--lenovo)
56. [Mise à jour du firmware TPM](#56-mise-%C3%A0-jour-du-firmware-tpm)
57. [Attaques : les bootkits](#57-attaques--les-bootkits)
58. [Attaques : evil maid et accès physique](#58-attaques--evil-maid-et-acc%C3%A8s-physique)
59. [Durcissement d'un poste : la check-list sécurité](#59-durcissement-dun-poste--la-check-list-s%C3%A9curit%C3%A9)
60. [Bonnes pratiques de gestion de parc](#60-bonnes-pratiques-de-gestion-de-parc)
61. [UEFI en datacenter : les serveurs](#61-uefi-en-datacenter--les-serveurs)
62. [Redfish et BMC : administrer le firmware à distance](#62-redfish-et-bmc--administrer-le-firmware-%C3%A0-distance)
63. [Secure Boot sur Hyper-V : les VM de génération 2](#63-secure-boot-sur-hyper-v--les-vm-de-g%C3%A9n%C3%A9ration-2)
64. [Cas pratique 1 : TPM non détecté par Windows](#64-cas-pratique-1--tpm-non-d%C3%A9tect%C3%A9-par-windows)
65. [Cas pratique 2 : Secure Boot bloque le démarrage après un changement matériel](#65-cas-pratique-2--secure-boot-bloque-le-d%C3%A9marrage-apr%C3%A8s-un-changement-mat%C3%A9riel)
66. [Cas pratique 3 : boucle de démarrage (boot loop) après une mise à jour](#66-cas-pratique-3--boucle-de-d%C3%A9marrage-boot-loop-apr%C3%A8s-une-mise-%C3%A0-jour)
67. [Cas pratique 4 : BCD corrompu — écran bleu 0xc000000f](#67-cas-pratique-4--bcd-corrompu--%C3%A9cran-bleu-0xc000000f)
68. [Cas pratique 5 : PXE-E53 / PXE-M0F — le client ne trouve pas le serveur de boot](#68-cas-pratique-5--pxe-e53--pxe-m0f--le-client-ne-trouve-pas-le-serveur-de-boot)
69. [Cas pratique 6 : mot de passe BIOS oublié](#69-cas-pratique-6--mot-de-passe-bios-oubli%C3%A9)
70. [Cas pratique 7 : BitLocker demande la clé de récupération à chaque démarrage](#70-cas-pratique-7--bitlocker-demande-la-cl%C3%A9-de-r%C3%A9cup%C3%A9ration-%C3%A0-chaque-d%C3%A9marrage)
71. [Cas pratique 8 : Clear CMOS — quand et comment](#71-cas-pratique-8--clear-cmos--quand-et-comment)
72. [Cas pratique 9 : Windows 11 refuse de s'installer — « ce PC ne peut pas exécuter Windows 11 »](#72-cas-pratique-9--windows-11-refuse-de-sinstaller----ce-pc-ne-peut-pas-ex%C3%A9cuter-windows-11---)
73. [Cas pratique 10 : disque GPT vu comme « non alloué » après un passage en mode Legacy](#73-cas-pratique-10--disque-gpt-vu-comme--non-allou%C3%A9---apr%C3%A8s-un-passage-en-mode-legacy)
74. [Cas pratique 11 : Secure Boot bloque une distribution Linux](#74-cas-pratique-11--secure-boot-bloque-une-distribution-linux)
75. [Cas pratique 12 : le TPM se désactive après une mise à jour du BIOS](#75-cas-pratique-12--le-tpm-se-d%C3%A9sactive-apr%C3%A8s-une-mise-%C3%A0-jour-du-bios)
76. [Cas pratique 13 : WDS ne répond pas aux clients d'un autre sous-réseau (VLAN)](#76-cas-pratique-13--wds-ne-r%C3%A9pond-pas-aux-clients-dun-autre-sous-r%C3%A9seau-vlan)
77. [Cas pratique 14 : écran noir après flash du BIOS — procédure de récupération](#77-cas-pratique-14--%C3%A9cran-noir-apr%C3%A8s-flash-du-bios--proc%C3%A9dure-de-r%C3%A9cup%C3%A9ration)
78. [Cas pratique 15 : double boot Windows/Linux cassé après une mise à jour Windows](#78-cas-pratique-15--double-boot-windowslinux-cass%C3%A9-apr%C3%A8s-une-mise-%C3%A0-jour-windows)
79. [Erreurs classiques : les 18 pièges à éviter](#79-erreurs-classiques--les-18-pi%C3%A8ges-%C3%A0-%C3%A9viter)
80. [Checklist : configuration d'un poste neuf en entreprise](#80-checklist--configuration-dun-poste-neuf-en-entreprise)
81. [Pense-bête de poche](#81-pense-b%C3%AAte-de-poche)
82. [Glossaire](#82-glossaire)
83. [Quiz : 10 questions pour valider](#83-quiz--10-questions-pour-valider)
84. [Pour aller plus loin](#84-pour-aller-plus-loin)

---

## 1. Le BIOS Legacy : l'héritage du PC

Le **BIOS** (*Basic Input/Output System*) est le firmware historique des PC, né avec l'IBM PC en 1981. C'est un programme stocké dans une puce mémoire flash de la carte mère, exécuté en premier à la mise sous tension.

### Ce que fait le BIOS au démarrage

1. **POST** (*Power-On Self-Test*) : test rapide du processeur, de la mémoire vive, du clavier, des disques.
2. **Initialisation matérielle** : configuration basique des périphériques via des interruptions 16 bits (INT 13h pour les disques, INT 10h pour l'affichage...).
3. **Recherche d'un périphérique bootable** : lecture du **MBR** (secteur 0) du premier disque trouvé dans l'ordre de boot.
4. **Transfert du contrôle** : chargement en mémoire du code du MBR (446 octets de code exécutable) puis exécution.

### Les limites historiques du BIOS

| Limite | Détail |
|---|---|
| Mode 16 bits réel | Le BIOS s'exécute en mode réel 16 bits du processeur, avec 1 Mo de mémoire adressable. |
| Disques ≤ 2 To | Adressage des secteurs sur 32 bits (LBA 32 bits) → 2^32 secteurs × 512 octets ≈ 2,2 To. |
| 4 partitions primaires | Structure MBR limitée (voir section 5). |
| Pas de réseau natif | Aucune pile réseau : impossible de booter en HTTP, de vérifier une signature, etc. |
| Lenteur | Initialisation séquentielle, pas de parallélisme, POST parfois long. |
| Sécurité quasi nulle | Aucune vérification d'intégrité du chargeur : un bootkit peut s'installer dans le MBR sans être détecté. |
| Interface texte | Configuration via une interface clavier en mode texte, navigation rudimentaire. |

### Pourquoi c'est important de le connaître encore

- Des millions de machines en entreprise tournent encore en mode **Legacy/CSM**.
- Les outils d'imagerie anciens, certains logiciels de clonage et vieux OS (Windows 7 sans UEFI, DOS) exigent le mode Legacy.
- Comprendre le BIOS éclaire *pourquoi* l'UEFI existe : chaque fonctionnalité UEFI répond à une limite du BIOS.

> 💡 **En résumé :** le BIOS est un firmware 16 bits, simple, rapide à comprendre, mais limité (2 To, pas de sécurité, pas de réseau). Il a régné de 1981 à ~2012.

---

## 2. L'UEFI : l'architecture moderne

L'**UEFI** (*Unified Extensible Firmware Interface*) est le successeur du BIOS, standardisé par l'**UEFI Forum**. Intel l'a initié (sous le nom EFI pour Itanium, fin des années 1990) ; il s'est généralisé sur PC à partir de ~2012.

### Différences d'architecture fondamentales

| Aspect | BIOS | UEFI |
|---|---|---|
| Mode processeur | 16 bits réel | 32 ou 64 bits protégé |
| Mémoire adressable | 1 Mo | Toute la RAM |
| Langage des modules | Assembleur 16 bits | C (drivers UEFI compilés) |
| Pilotes | Intégrés au firmware, figés | **Drivers UEFI** chargeables (système de fichiers, réseau...) |
| Interface | Texte, clavier | Graphique possible, souris, police TrueType |
| Partitionnement | MBR uniquement | **GPT** (et MBR via CSM) |
| Taille disque bootable | ≤ 2 To | **> 9 Zo** (Zettaoctets) en théorie |
| Réseau | Non | Pile **TCP/IP** intégrée (HTTP boot possible) |
| Sécurité | Aucune | **Secure Boot**, TPM mesuré |
| Configuration stockée | CMOS (pile) | **NVRAM** (variables UEFI persistantes) |
| Extensibilité | Quasi nulle | Applications UEFI (shell, diagnostics, utilitaires) |

### Les composants d'un firmware UEFI

```
┌─────────────────────────────────────────────────┐
│  SEC (Security)        Phase d'initialisation   │
│  PEI (Pre-EFI Init)    Mémoire, CPU de base     │
│  DXE (Driver Exec.)    Pilotes, périphériques   │
│  BDS (Boot Dev Select) Choix du périphérique    │
│  TSL (Transient Sys.)  Chargeurs, OS            │
│  RT (Runtime)          Services après boot OS   │
└─────────────────────────────────────────────────┘
```

1. **SEC** : le processeur sort du reset, initialise un début d'environnement (cache-as-RAM).
2. **PEI** : initialise la mémoire vive permanente (RAM), découvre le chipset.
3. **DXE** : charge les pilotes UEFI (contrôleur SATA/NVMe, USB, réseau, affichage GOP...). C'est ici que le TPM est généralement interrogé et que Secure Boot vérifie les signatures.
4. **BDS** : le **Boot Manager** lit les variables de boot en NVRAM, affiche éventuellement un menu, puis lance l'application EFI choisie (ex. `\EFI\Microsoft\Boot\bootmgfw.efi`).
5. **TSL** : l'OS démarre ; il peut encore appeler les services de boot UEFI.
6. **RT** : une fois l'OS lancé (`ExitBootServices`), seuls les **services d'exécution** restent disponibles (horloge temps réel, variables NVRAM, reset/shutdown, capsule de mise à jour firmware).

### Les services UEFI en pratique

- **Boot Services** : allocation mémoire, protocoles (Block I/O, Simple File System, Graphics Output...). Disponibles jusqu'au démarrage de l'OS.
- **Runtime Services** : `GetVariable` / `SetVariable` (lecture/écriture NVRAM), `GetTime`, `ResetSystem`, `UpdateCapsule` (mise à jour firmware depuis l'OS !). C'est grâce à `UpdateCapsule` que Windows Update peut proposer des mises à jour de firmware.

> 💡 **En résumé :** l'UEFI est un mini-système d'exploitation pré-boot : pilotes en C, réseau, sécurité cryptographique, variables persistantes. Tout ce que le BIOS ne savait pas faire.

---

## 3. BIOS vs UEFI : comparatif détaillé

Tableau de synthèse pour décider du mode à utiliser sur le parc :

| Critère | BIOS (Legacy) | UEFI natif |
|---|---|---|
| Année d'introduction | 1981 | ~2006 (généralisé ~2012) |
| Architecture CPU au boot | 16 bits réel | 64 bits (long mode) |
| Disque système max | 2 To | 9,4 Zo (limite GPT) |
| Table de partitions | MBR | GPT |
| Nombre de partitions | 4 primaires (ou 3 + étendue) | 128 par défaut |
| Vitesse de démarrage | Lente (POST séquentiel) | Rapide (Fast Boot, init parallèle) |
| Secure Boot | ❌ Impossible | ✅ Natif |
| TPM mesuré au boot | ❌ | ✅ (Measured Boot) |
| Boot réseau | PXE via ROM optionnelle | PXE + **HTTP Boot** natif |
| Shell pré-boot | ❌ | ✅ (UEFI Shell en option) |
| Interface de config | Texte | Graphique / souris |
| Variables persistantes | CMOS 256 octets | NVRAM (plusieurs Mo) |
| Windows 11 | ❌ Incompatible | ✅ Requis |
| BitLocker + TPM 2.0 | ⚠️ Partiel (TPM 1.2) | ✅ Plein support |
| Support constructeurs | Abandonné (CSM retiré depuis ~2020) | Standard actuel |

### Recommandation d'entreprise (2026)

- **Tout poste neuf : UEFI natif, CSM désactivé, Secure Boot activé, TPM 2.0 activé.** C'est le prérequis Windows 11 et la base du durcissement.
- **Parc existant :** planifier la migration Legacy → UEFI (via `mbr2gpt`, section 8) avant toute montée en Windows 11.
- **Exceptions :** vieux logiciels industriels, DOS, Windows 7 32 bits, certains outils de clonage propriétaires → maintenir quelques machines en Legacy/CSM, isolées et documentées.

---

## 4. CSM (Compatibility Support Module)

Le **CSM** est un module de compatibilité intégré à certains firmwares UEFI : il **émule un BIOS Legacy** pour démarrer des OS et périphériques qui ne connaissent pas l'UEFI.

### Comment ça marche

1. Le firmware démarre en mode UEFI.
2. Si le périphérique de boot n'a pas de chargeur EFI valide (pas d'ESP, pas de `\EFI\BOOT\bootx64.efi`), le CSM prend le relais.
3. Il fournit les interruptions 16 bits (INT 13h...) et exécute le MBR comme un vrai BIOS.

### Options typiques dans le setup

| Option | Signification |
|---|---|
| `UEFI` / `UEFI only` | Pas de CSM, boot UEFI natif uniquement. **Recommandé.** |
| `Legacy` / `Legacy only` / `CSM` | Émulation BIOS, boot MBR uniquement. |
| `UEFI + Legacy` / `Both` | Essaie UEFI puis Legacy. Pratique en transition, à éviter à terme. |
| `Legacy ROM` / `Option ROM` | Active les ROMs d'extension 16 bits (vieilles cartes réseau pour PXE Legacy, cartes RAID...). |

### Pourquoi désactiver le CSM

1. **Sécurité :** le mode Legacy contourne Secure Boot. Un attaquant qui active le CSM peut booter un OS non signé.
2. **Windows 11 :** exige le mode UEFI natif (le CSM doit être désactivé).
3. **Démarrage :** le CSM ralentit le POST (initialisation des ROMs 16 bits).
4. **Avenir :** Intel et AMD ont annoncé la fin du CSM (Intel : plus de CSM sur les plateformes récentes ; les constructeurs le retirent de leurs firmwares depuis ~2020-2022). Sur du matériel récent, l'option n'existe tout simplement plus.

### Piège classique

> Un disque GPT n'est **pas bootable** en mode Legacy/CSM (sauf cas exotiques avec partition BIOS boot). Inversement, un disque MBR avec installation Windows Legacy ne boote pas en UEFI natif. **Le mode du firmware et le style de partition du disque système doivent correspondre.** C'est la cause n°1 des « plus de boot » après un changement de réglage.

Vérifier le mode de démarrage actuel de Windows :

```powershell
# Méthode 1 : variable d'environnement système
# Si le dossier existe, Windows a démarré en UEFI
Test-Path "$env:SystemRoot\Panther"

# Méthode 2 : fiable — interroger le firmware via WMI
$bios = Get-CimInstance -ClassName Win32_BIOS
$bios | Select-Object Manufacturer, Name, SMBIOSBIOSVersion, ReleaseDate

# Méthode 3 : msinfo32 → "Mode BIOS" = UEFI ou Hérité
msinfo32
```

```powershell
# Méthode 4 (la plus directe) : le chemin du chargeur
# En UEFI : \EFI\Microsoft\Boot\bootmgfw.efi — En Legacy : \Windows\system32\winload.exe
bcdedit /enum '{current}' | Select-String 'path'
```

---

## 5. MBR : structure et fonctionnement

Le **MBR** (*Master Boot Record*) occupe le **secteur 0** (512 octets) du disque.

### Layout des 512 octets

| Offset | Taille | Contenu |
|---|---|---|
| 0x000 | 446 octets | **Code de démarrage** (bootstrap) exécuté par le BIOS |
| 0x1BE | 16 octets | Entrée de partition n°1 |
| 0x1CE | 16 octets | Entrée de partition n°2 |
| 0x1DE | 16 octets | Entrée de partition n°3 |
| 0x1EE | 16 octets | Entrée de partition n°4 |
| 0x1FE | 2 octets | **Signature** `55 AA` (indique un MBR valide) |

### Une entrée de partition (16 octets)

| Offset | Taille | Champ |
|---|---|---|
| 0x00 | 1 | Indicateur de boot (`0x80` = active/bootable) |
| 0x01 | 3 | Début CHS (obsolète) |
| 0x04 | 1 | **Type de partition** (0x07 = NTFS, 0x0B/0x0C = FAT32, 0x83 = Linux...) |
| 0x05 | 3 | Fin CHS (obsolète) |
| 0x08 | 4 | LBA de début (32 bits) |
| 0x0C | 4 | Nombre de secteurs (32 bits) |

### Conséquences directes

- **4 entrées × 16 octets = 4 partitions primaires maximum.** Pour en avoir plus, l'une d'elles devient une partition **étendue** contenant des partitions **logiques** (chaînage EBR).
- **Adressage 32 bits** : 2^32 secteurs × 512 o = **2 To** (2,2 To exactement : 2 199 023 255 552 octets). Au-delà, l'espace est inutilisable.
- **Une seule copie** : pas de redondance. Si le secteur 0 est corrompu → disque illisible (récupérable avec des outils comme TestDisk).

Inspecter le style de partition en PowerShell :

```powershell
Get-Disk | Select-Object Number, FriendlyName, PartitionStyle, Size,
    @{n='SizeGB';e={[math]::Round($_.Size/1GB,1)}}
```

---

## 6. GPT : structure et fonctionnement

Le **GPT** (*GUID Partition Table*), défini dans la spécification UEFI, remplace le MBR.

### Layout du disque GPT

```
LBA 0    : Protective MBR (faux MBR : 1 partition type 0xEE couvrant tout le disque)
LBA 1    : En-tête GPT primaire (signature "EFI PART", CRC32, LBA de la sauvegarde)
LBA 2-33 : Table des entrées de partitions primaire (128 entrées × 128 octets)
LBA 34.. : Partitions de données
   ...    : Table des entrées secondaire (copie de secours)
   fin-1  : En-tête GPT secondaire (copie de secours)
```

### Points clés

| Aspect | Détail |
|---|---|
| **Protective MBR** | Protège le disque des vieux outils MBR : une partition fictive de type `0xEE` couvre tout le disque pour éviter tout écrasement accidentel. |
| **128 partitions** | 128 entrées par défaut (extensible), chacune identifiée par un **GUID unique**. |
| **Types par GUID** | Chaque partition a un GUID de type : ESP (`C12A7328-F81F-11D2-BA4B-00A0C93EC93B`), Microsoft Basic Data, Microsoft Reserved (MSR), Linux filesystem, etc. |
| **Noms** | Chaque partition peut porter un nom UTF-16 (36 caractères). |
| **Redondance** | En-tête + table dupliqués en fin de disque → **auto-réparation** si le primaire est corrompu. |
| **CRC32** | Intégrité de l'en-tête et de la table vérifiée à chaque lecture. |
| **Taille max** | LBA sur 64 bits → 9,4 Zo (Zettaoctets). En pratique, limité par l'OS et le système de fichiers. |

### Les partitions typiques d'un Windows en UEFI/GPT

| N° | Type (GUID) | Taille | Rôle |
|---|---|---|---|
| 1 | ESP | 100–260 Mo | **EFI System Partition** (FAT32) : chargeurs de boot |
| 2 | MSR | 16 Mo | Microsoft Reserved : zone de travail pour conversions |
| 3 | Basic Data | le reste − 500 Mo | `C:` Windows (NTFS) |
| 4 | Recovery | ~500 Mo–1 Go | WinRE (environnement de récupération) |

Voir le détail :

```powershell
Get-Disk -Number 0 | Get-Partition |
    Select-Object PartitionNumber, DriveLetter, Size,
    @{n='SizeGB';e={[math]::Round($_.Size/1GB,2)}},
    Type, GptType
```

---

## 7. MBR vs GPT : limites et comparatif

| Critère | MBR | GPT |
|---|---|---|
| Âge | 1983 | 2000s (UEFI) |
| Taille max disque | **2 To** | **9,4 Zo** |
| Partitions primaires | **4** (ou 3 + étendue) | **128** |
| Redondance | Aucune (1 secteur) | En-tête + table dupliqués |
| Contrôle d'intégrité | Aucun | CRC32 |
| Nommage des partitions | Non | Oui (UTF-16) |
| Boot UEFI natif | ❌ Non | ✅ Oui |
| Boot BIOS Legacy | ✅ Oui | ❌ Non (sauf CSM + cas spéciaux) |
| Windows 11 (disque système) | ❌ Incompatible | ✅ Requis |
| Outils de réparation | TestDisk, bootrec | bcdboot, bcdedit, diskpart |

### Règle d'or

> **Disque système Windows moderne = GPT + UEFI.** Le MBR ne se justifie plus que pour : vieux OS, disques de données sur très vieilles machines, clés USB de compatibilité universelle (et encore, l'exFAT/GPT est lisible partout aujourd'hui).

---

## 8. Conversion MBR → GPT : mbr2gpt

`mbr2gpt.exe` (présent depuis Windows 10 1703) convertit un disque système MBR en GPT **sans perte de données** et **sans réinstallation**, puis reconfigure le boot en UEFI.

### Préconditions (toutes obligatoires)

- [ ] Windows 10 version 1703+ / Windows 11.
- [ ] Le disque est le **disque système** (celui qui contient Windows).
- [ ] **3 partitions maximum** sur le disque MBR (le GPT a besoin de place pour l'ESP + MSR).
- [ ] Pas de partition étendue/logique compliquée (l'outil gère les cas simples).
- [ ] **BitLocker suspendu ou désactivé** sur le volume (sinon la conversion échoue ou déclenche une récupération).
- [ ] Sauvegarde à jour (principe de précaution, même si l'outil est fiable).

### Procédure complète

```powershell
# 1. Vérifier l'état actuel (depuis Windows, en administrateur)
Get-Disk -Number 0 | Select-Object PartitionStyle   # doit afficher MBR

# 2. Suspendre BitLocker si actif (indispensable)
Suspend-BitLocker -MountPoint "C:" -RebootCount 1
# ou : manage-bde -protectors -disable C:

# 3. VALIDER la conversion (ne modifie rien)
mbr2gpt /validate /disk:0 /allowFullOS

# 4. CONVERTIR (depuis Windows ; redémarrage non nécessaire pour la conversion elle-même)
mbr2gpt /convert /disk:0 /allowFullOS
```

Sortie typique en cas de succès :

```
MBR2GPT: Attempting to convert disk 0
MBR2GPT: Retrieving layout of disk
MBR2GPT: Validating layout, disk sector size is: 512 bytes
MBR2GPT: Trying to shrink the system partition
MBR2GPT: Trying to shrink the OS partition
MBR2GPT: Creating the EFI system partition
MBR2GPT: Installing the new boot files
MBR2GPT: Performing the layout conversion
MBR2GPT: Migrating default boot entry
MBR2GPT: Adding recovery boot entry
MBR2GPT: Fixing drive letter mapping
MBR2GPT: Conversion completed successfully
MBR2GPT: Before the new system can boot properly you need to switch the
firmware to boot to UEFI mode!
```

### 5. Basculer le firmware en UEFI (après la conversion)

1. Redémarrer, entrer dans le setup (F2 / DEL / F10 selon constructeur).
2. Passer le mode de boot de **Legacy** à **UEFI** (désactiver le CSM).
3. Vérifier que **Secure Boot** est activable/activé.
4. Sauvegarder et redémarrer.

```powershell
# 6. Vérification post-migration
Get-Disk -Number 0 | Select-Object PartitionStyle   # doit afficher GPT
Confirm-SecureBootUEFI                              # idéalement True
```

### En cas d'échec de validation

| Message | Cause probable | Solution |
|---|---|---|
| `Disk layout validation failed` | > 3 partitions, ou partition étendue | Supprimer/fusionner les partitions superflues (après sauvegarde). |
| `Cannot find OS partition` | Partition Windows non reconnue | Vérifier avec `diskpart` → `list volume`. |
| Échec avec BitLocker | Chiffrement actif | `Suspend-BitLocker` puis réessayer. |
| `ValidateLayout: Too many MBR partitions` | 4 partitions primaires | Supprimer une partition (souvent la partition de récupération d'usine). |

> ⚠️ **mbr2gpt ne convertit pas dans l'autre sens.** Pour GPT → MBR, il faut sauvegarder, recréer la table (diskpart `clean` + `convert mbr`) et réinstaller : opération destructive.

---

## 9. Cas d'usage : quand utiliser MBR ou GPT

| Situation | Choix recommandé |
|---|---|
| Poste Windows 10/11 neuf | **GPT + UEFI** (obligatoire pour Win11) |
| Migration vers Windows 11 | Convertir en GPT via `mbr2gpt` |
| Disque de données > 2 To | **GPT** (le MBR ne verra que 2 To) |
| Disque de données ≤ 2 To sur machine récente | **GPT** (redondance, CRC, nommage) |
| Vieux PC en BIOS Legacy (Windows 7, DOS, industriel) | **MBR** (le seul bootable en Legacy) |
| Clé USB multi-usage (boot + stockage) | **GPT** + FAT32 si UEFI only ; **MBR** + FAT32 si compatibilité Legacy requise |
| Serveur / NAS | **GPT** systématiquement |

---

## 10. Séquence de boot UEFI pas à pas

Chronologie complète, de l'appui sur le bouton à l'écran de connexion Windows :

```
① Mise sous tension
   └─ Le CPU sort du reset, exécute le firmware (SEC → PEI)
② Initialisation matérielle (DXE)
   ├─ Pilotes UEFI : NVMe/SATA, USB, GOP (affichage), réseau
   ├─ Mesure TPM (Measured Boot) : chaque composant est "mesuré" (hash)
   │   dans les PCR du TPM
   └─ Secure Boot : vérification des signatures des pilotes/chargeurs
③ BDS — Boot Device Selection
   ├─ Lecture des variables Boot#### en NVRAM
   ├─ Application de BootOrder
   └─ (Optionnel) menu de boot F12 / timeout
④ Exécution du chargeur : \EFI\Microsoft\Boot\bootmgfw.efi (depuis l'ESP)
   └─ Secure Boot vérifie sa signature Microsoft
⑤ bootmgfw.efi lit le BCD
   ├─ Affiche le menu (multi-boot éventuel)
   └─ Sélectionne l'entrée OS
⑥ Chargement de winload.efi (+ signature vérifiée)
   ├─ winload.efi vérifie à son tour les composants critiques :
   │   noyau ntoskrnl.exe, HAL, drivers de démarrage (ELAM)
   └─ ExitBootServices : fin des services de boot UEFI
⑦ Noyau Windows : initialisation, chargement des drivers, session
⑧ Écran de connexion
```

### Measured Boot vs Secure Boot : ne pas confondre

| | Secure Boot | Measured Boot (TPM) |
|---|---|---|
| Rôle | **Bloquer** ce qui n'est pas signé | **Enregistrer** (mesurer) ce qui a démarré |
| Action en cas de problème | Refuse de booter | Boote quand même, mais les PCR changent |
| Utilisé par | Vérification de signature | BitLocker (scelle la clé sur les PCR), attestation |

---

## 11. ESP (EFI System Partition)

L'**ESP** est la partition FAT32 qui contient les chargeurs de démarrage UEFI.

### Caractéristiques

| Propriété | Valeur |
|---|---|
| Système de fichiers | **FAT32** (obligatoire par la spec UEFI) |
| GUID de type | `C12A7328-F81F-11D2-BA4B-00A0C93EC93B` |
| Taille Windows | 100 Mo (disques ≤ 16 Go : 100 Mo ; en pratique 100–260 Mo) |
| Lettre de lecteur | **Aucune** par défaut (montage manuel possible) |
| Contenu Windows | `\EFI\Microsoft\Boot\bootmgfw.efi`, `\EFI\Boot\bootx64.efi` (fallback), polices, BCD |

### Arborescence typique

```
ESP (FAT32)
└── EFI
    ├── Boot
    │   └── bootx64.efi          ← chargeur de secours (chemin "removable media")
    ├── Microsoft
    │   └── Boot
    │       ├── bootmgfw.efi     ← Windows Boot Manager
    │       ├── bootmgr.efi
    │       ├── BCD              ← base de configuration de boot
    │       ├── memtest.efi
    │       └── Fonts\ / Resources\
    └── Ubuntu                  ← exemple multi-boot
        └── grubx64.efi / shimx64.efi
```

### Manipuler l'ESP (lecture seule recommandée)

```powershell
# Assigner une lettre à l'ESP pour l'inspecter (administrateur)
# 1. Identifier l'ESP
Get-Partition | Where-Object { $_.GptType -eq '{c12a7328-f81f-11d2-ba4b-00a0c93ec93b}' } |
    Select-Object DiskNumber, PartitionNumber, Size

# 2. Lui assigner une lettre temporaire via diskpart
diskpart
#   list disk
#   select disk 0
#   list partition
#   select partition 1        (l'ESP)
#   assign letter=S
#   exit

# 3. Explorer
Get-ChildItem S:\EFI -Recurse | Select-Object FullName, Length

# 4. Retirer la lettre après inspection (hygiène)
#   diskpart → select partition 1 → remove letter=S
```

> ⚠️ **Ne jamais formater l'ESP** sauf à savoir la reconstruire avec `bcdboot` (section 15). Ne jamais y stocker de fichiers personnels.

---

## 12. UEFI Boot Manager et NVRAM

Le **Boot Manager UEFI** est le menu/logiciel du firmware qui choisit *quoi* démarrer. Ses choix sont stockés dans des **variables NVRAM** :

| Variable | Rôle |
|---|---|
| `Boot0001`, `Boot0002`... | Une entrée de boot = chemin vers un `.efi` + description + options |
| `BootOrder` | Ordre de priorité (ex. `0001,0003,0002`) |
| `BootNext` | Boot unique au prochain redémarrage (puis effacé) |
| `Timeout` | Délai d'affichage du menu |
| `BootCurrent` | Entrée utilisée pour le boot en cours |

### Lire/écrire les entrées (Windows)

```powershell
# Lister les entrées de boot du firmware (vue Windows via BCD)
bcdedit /enum firmware

# Exemple de sortie (abrégée) :
# Firmware Boot Manager
# ---------------------
# identifier              {fwbootmgr}
# displayorder            {bootmgr}
#                         {6a5f2e1a-...}   ← entrée "ubuntu" par ex.
# timeout                 2
```

```powershell
# Changer l'ordre de boot du firmware (exemple : remettre Windows en premier)
bcdedit /set '{fwbootmgr}' displayorder '{bootmgr}' /addfirst

# Définir le timeout du menu firmware
bcdedit /set '{fwbootmgr}' timeout 5

# Créer une entrée pour booter une fois sur un périphérique (BootNext) :
# (souvent plus simple via la touche F12 du constructeur au démarrage)
```

### Côté Linux (pour info, en multi-boot)

```bash
efibootmgr -v        # lister les entrées avec chemins complets
efibootmgr -o 0001,0003,0002   # changer l'ordre
efibootmgr -n 0003   # booter une fois sur 0003 (BootNext)
```

### Pourquoi c'est utile en entreprise

- Après un clonage ou un changement de disque, des **entrées fantômes** peuvent subsister → nettoyage via `bcdedit /delete` ou le setup.
- En déploiement (MDT/SCCM), on peut forcer l'ordre de boot par script.
- Un malware/bootkit peut ajouter une entrée : `bcdedit /enum firmware` fait partie de l'audit.

---

## 13. BCD (Boot Configuration Data)

Le **BCD** remplace le `boot.ini` depuis Windows Vista. C'est une base de registre (ruche) stockée dans :
- **UEFI :** `S:\EFI\Microsoft\Boot\BCD` (sur l'ESP, sans extension),
- **Legacy :** `C:\Boot\BCD`.

### Objets principaux

| Objet (GUID) | Rôle |
|---|---|
| `{bootmgr}` | **Windows Boot Manager** : comportement du menu (timeout, affichage, ordre) |
| `{current}` / `{default}` | L'entrée OS démarrée par défaut |
| `{fwbootmgr}` | Le Boot Manager du **firmware** (variables NVRAM) |
| `{memdiag}` | Diagnostic mémoire Windows |
| GUIDs divers | Entrées secondaires (WinRE, autres OS, options de démarrage) |

### Paramètres courants d'une entrée OS

| Paramètre | Signification |
|---|---|
| `device` / `osdevice` | Partition contenant le chargeur / l'OS (`partition=C:`) |
| `path` | `\Windows\system32\winload.efi` (UEFI) ou `winload.exe` (Legacy) |
| `systemroot` | `\Windows` |
| `nx` | DEP : `OptIn` (défaut) / `OptOut` / `AlwaysOn` / `AlwaysOff` |
| `pae` | Physical Address Extension |
| `testsigning` | `Yes` = accepte les drivers en mode test (à éviter en prod !) |
| `nointegritychecks` | Désactive la vérification de signature des drivers (danger) |
| `recoveryenabled` | Active WinRE en cas d'échec |
| `bootlog` | Journal `ntbtlog.txt` |

```powershell
# Vue d'ensemble
bcdedit /enum

# Détail de l'entrée courante
bcdedit /enum '{current}'

# Vue firmware (NVRAM)
bcdedit /enum firmware

# Exporter le BCD avant toute modification (réflexe !)
bcdedit /export C:\BCD-backup-2026-09-27
```

---

## 14. bcdedit : le couteau suisse du boot Windows

`bcdedit.exe` (console administrateur obligatoire) lit et modifie le BCD. **Toujours exporter avant de modifier.**

### Commandes de lecture

```powershell
bcdedit                          # équivalent de /enum ACTIVE
bcdedit /enum ALL                 # tout : firmware + bootmgr + OS + WinRE
bcdedit /enum '{current}' /v      # détail verbeux (GUIDs complets)
bcdedit /v                        # affiche les GUIDs au lieu des alias
```

### Commandes d'écriture courantes

```powershell
# --- Menu de démarrage ---
# Timeout du menu (secondes)
bcdedit /timeout 10

# Entrée par défaut
bcdedit /default '{current}'

# Ordre d'affichage des OS
bcdedit /displayorder '{current}' '{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}'

# --- Options de démarrage avancées ---
# Activer le menu F8 hérité (utile en dépannage, désactivé par défaut depuis Win8)
bcdedit /set '{current}' bootmenupolicy Legacy
# Revenir au comportement moderne :
bcdedit /set '{current}' bootmenupolicy Standard

# Démarrage en mode sans échec minimal (puis reboot)
bcdedit /set '{current}' safeboot Minimal
# Annuler :
bcdedit /deletevalue '{current}' safeboot

# Activer le journal de démarrage (C:\Windows\ntbtlog.txt)
bcdedit /set '{current}' bootlog Yes

# Désactiver la signature des pilotes (DÉPANNAGE UNIQUEMENT, jamais en prod)
bcdedit /set '{current}' testsigning Yes
# ⚠️ Laisse un filigrane "Test Mode" et désactive une protection clé.

# --- Gestion des entrées ---
# Copier l'entrée courante pour créer une entrée de test
bcdedit /copy '{current}' /d "Windows 11 (Test)"

# Supprimer une entrée (utiliser le GUID affiché par /v)
bcdedit /delete '{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}'

# Réparer l'entrée : repointer vers la bonne partition
bcdedit /set '{current}' device partition=C:
bcdedit /set '{current}' osdevice partition=C:
```

### Sauvegarde / restauration

```powershell
# Export (à faire AVANT toute modification)
bcdedit /export C:\BCD-backup-avant-modif

# Import (depuis WinRE ou un OS fonctionnel)
bcdedit /import C:\BCD-backup-avant-modif
```

> 💡 **Réflexe d'entreprise :** avant toute intervention sur le BCD (changement de disque, clonage, multi-boot), exportez le BCD et notez le `diskpart → list volume`. 30 secondes qui évitent des heures de dépannage.

---

## 15. bcdboot : reconstruire les fichiers de démarrage

`bcdboot.exe` **recrée** les fichiers de boot (chargeurs + BCD) à partir d'une installation Windows existante. C'est l'outil n°1 quand « BOOTMGR is missing » / `0xc000000f` / ESP effacée.

### Cas d'usage

| Symptôme | Commande |
|---|---|
| ESP formatée ou corrompue | `bcdboot C:\Windows /s S: /f UEFI` |
| BCD supprimé/corrompu | `bcdboot C:\Windows` (recrée tout) |
| Nouveau disque cloné qui ne boote pas | `bcdboot` + vérification `bcdedit` |
| Ajouter Windows au menu après install Linux | `bcdboot C:\Windows` (recrée l'entrée `{bootmgr}`) |

### Procédure depuis WinRE (environnement de récupération)

```
1. Démarrer sur une clé USB d'installation Windows → "Réparer l'ordinateur"
   → Dépannage → Invite de commandes.
2. Identifier les lettres (elles peuvent changer en WinRE !) :
     diskpart → list volume → noter C: (Windows) et l'ESP (FAT32, ~100 Mo)
3. Assigner une lettre à l'ESP si besoin :
     select volume 2   (l'ESP)
     assign letter=S
     exit
4. Reconstruire :
     bcdboot C:\Windows /s S: /f UEFI
     → "Boot files successfully created."
5. Vérifier :
     bcdedit /store S:\EFI\Microsoft\Boot\BCD /enum
6. Retirer la lettre (optionnel) puis redémarrer.
```

### Syntaxe détaillée

```powershell
# Forme complète
bcdboot C:\Windows /s S: /f UEFI /l fr-FR
#        ^source     ^cible ^firmware ^langue du menu
# /f ALL    : copie les fichiers pour UEFI ET BIOS (clé USB universelle)
# /l fr-FR  : langue ; par défaut celle de l'OS
```

### bcdboot vs bootrec (Legacy)

| Outil | Firmware | Rôle |
|---|---|---|
| `bcdboot` | UEFI (et BIOS) | Recrée chargeurs + BCD depuis `C:\Windows` |
| `bootrec /fixmbr` | BIOS Legacy | Réécrit le code MBR (pas la table !) |
| `bootrec /fixboot` | BIOS Legacy | Réécrit le secteur de boot de la partition |
| `bootrec /rebuildbcd` | BIOS Legacy | Reconstruit le BCD en scannant les installations |

> ⚠️ Sur un système UEFI/GPT, `bootrec /fixmbr` est inutile voire contre-productif : utilisez `bcdboot`.

---

## 16. Secure Boot : principe général

**Secure Boot** est une fonctionnalité UEFI (chapitre 27 de la spec) qui garantit que **seuls des logiciels signés par une autorité de confiance** s'exécutent pendant le démarrage : firmware → chargeur → noyau → pilotes critiques.

### L'idée en une phrase

> Chaque maillon de la chaîne de démarrage **vérifie la signature cryptographique** du maillon suivant avant de lui passer la main. Un chargeur modifié ou inconnu = démarrage refusé.

### Ce que Secure Boot protège

- ✅ Chargeurs de boot (bootmgfw.efi, shim Linux signé...)
- ✅ Noyau de l'OS et HAL
- ✅ Pilotes de démarrage critiques (via ELAM — *Early Launch Anti-Malware*)
- ✅ Pilotes en mode noyau (sur Windows 64 bits : signature obligatoire de toute façon)

### Ce que Secure Boot ne protège PAS

- ❌ L'OS une fois démarré (c'est le rôle de l'antivirus, de HVCI/VBS...)
- ❌ Les attaques avec accès physique + mot de passe firmware inconnu (voir evil maid, section 58)
- ❌ Les vulnérabilités dans un chargeur **légitimement signé** (ex. failles dans des bootloaders révoqués ensuite via dbx)
- ❌ Le vol de données sur disque non chiffré (Secure Boot ≠ chiffrement → coupler avec BitLocker)

### Prérequis

1. Firmware en mode **UEFI natif** (CSM désactivé).
2. Disque système en **GPT**.
3. Clés Secure Boot provisionnées (mode Standard : clés Microsoft d'usine).

---

## 17. La chaîne de confiance du démarrage

```
                    ┌──────────────┐
                    │  Firmware   │── vérifié par la racine matérielle
                    │  (signé OEM)│    (Boot Guard Intel / PSB AMD, si actif)
                    └──────┬───────┘
                           │ Secure Boot vérifie la signature
                    ┌──────▼───────┐
                    │ bootmgfw.efi │── signé Microsoft (clé dans db)
                    │ (Boot Mgr)   │
                    └──────┬───────┘
                           │ vérifie la signature
                    ┌──────▼───────┐
                    │ winload.efi  │── signé Microsoft
                    └──────┬───────┘
                           │ vérifie les signatures (CI = Code Integrity)
              ┌────────────▼────────────┐
              │ ntoskrnl.exe, HAL,      │── signés Microsoft
              │ drivers boot (ELAM)     │
              └─────────────────────────┘
```

### Maillons et signatures (Windows)

| Maillon | Fichier | Signé par | Vérifié par |
|---|---|---|---|
| Boot Manager | `bootmgfw.efi` | Microsoft Windows Production PCA | Firmware (clé MS dans `db`) |
| Chargeur OS | `winload.efi` | Microsoft | bootmgfw.efi |
| Noyau | `ntoskrnl.exe` | Microsoft | winload.efi (Code Integrity) |
| Pilotes kernel | `*.sys` | Microsoft / WHQL | Code Integrity du noyau |

### ELAM (Early Launch Anti-Malware)

Windows charge en premier un pilote anti-malware spécial (**ELAM**) : c'est lui qui valide les autres pilotes de démarrage *avant* qu'ils ne s'exécutent. Votre solution EDR/antivirus d'entreprise fournit ce pilote. Si l'ELAM est absent ou non signé → Secure Boot peut bloquer.

---

## 18. Les clés Secure Boot : PK (Platform Key)

La **PK** est la clé maîtresse : **elle contrôle la plateforme**. Celui qui détient la clé privée de la PK est le « propriétaire » du Secure Boot de la machine.

| Propriété | Détail |
|---|---|
| Rôle | Signer les mises à jour de la **KEK** (et donc indirectement tout le reste) |
| Nombre | **Une seule** par machine |
| Détenteur (mode Standard) | Le **constructeur** (Dell, HP, Lenovo...) |
| Mode Setup | Si aucune PK n'est installée, la machine est en **Setup Mode** : n'importe qui peut écrire les clés (état d'usine avant provisioning) |
| Mode User | PK installée → **User Mode** : les clés sont protégées, modifiables uniquement avec signature PK |

### Opérations liées à la PK

| Action | Effet |
|---|---|
| Installer une PK | Passe en User Mode, verrouille la configuration |
| **Effacer la PK** (`Clear Secure Boot Keys`) | Repasse en **Setup Mode** → Secure Boot **désactivé** de fait |
| Remplacer la PK | Possible en mode Custom (signature avec l'ancienne PK requise) |

> ⚠️ **Ne jamais effacer les clés Secure Boot** (« Clear Secure Boot Keys ») sans raison valable : cela désactive Secure Boot jusqu'à re-provisioning complet. En entreprise, c'est une action à tracer.

---

## 19. Les clés Secure Boot : KEK (Key Exchange Key)

La **KEK** fait le lien entre la PK (propriétaire plateforme) et les bases de signatures (db/dbx).

| Propriété | Détail |
|---|---|
| Rôle | Signer les mises à jour de **`db`** et **`dbx`** |
| Nombre | **Plusieurs** possibles |
| Détenteurs typiques (mode Standard) | **Microsoft** (`Microsoft Corporation KEK CA 2011`) + le **constructeur** (KEK OEM) |
| Mise à jour | Via Windows Update / capsule firmware, signée par la PK |

### Hiérarchie complète

```
PK (constructeur)
 └─ signe les mises à jour de la KEK
     KEK (Microsoft + OEM)
      └─ signe les mises à jour de db et dbx
          db  : chargeurs autorisés  (ex. "Microsoft Windows Production PCA 2011",
                                      "Microsoft Corporation UEFI CA 2011")
          dbx : chargeurs révoqués   (hashes interdits)
```

Concrètement : quand Microsoft révoque un chargeur vulnérable, il publie une mise à jour de la **dbx** signée par sa KEK → le firmware l'accepte car il fait confiance à cette KEK.

---

## 20. Les clés Secure Boot : db (base de signatures autorisées)

La **`db`** (*authorized signature database*) contient les certificats et hashes **autorisés** à démarrer.

### Contenu typique en mode Standard

| Entrée | Usage |
|---|---|
| `Microsoft Windows Production PCA 2011` | Signe `bootmgfw.efi`, `winload.efi` (Windows 8 → 11) |
| `Microsoft Corporation UEFI CA 2011` | Signe le **shim** Linux (Canonical, Red Hat, SUSE...) |
| `Microsoft Option ROM UEFI CA` | Signe les ROMs d'extension (cartes réseau, RAID) |
| Certificats OEM | Chargeurs/outils du constructeur (diagnostics Dell/HP/Lenovo) |

### Comment le firmware vérifie

1. Le firmware calcule le hash du binaire `.efi` à exécuter.
2. Il vérifie la **signature** du binaire avec les certificats de `db`.
3. Si la signature est valide **et** que le hash n'est pas dans `dbx` → exécution autorisée.
4. Sinon → **refus de démarrage** (écran rouge / message « Secure Boot Violation »).

---

## 21. Les clés Secure Boot : dbx (base de révocation)

La **`dbx`** (*forbidden signature database*) contient les certificats et hashes **interdits**, même s'ils sont signés par une clé de `db`.

### À quoi ça sert

Révoquer des chargeurs **légitimement signés mais vulnérables**. Exemple historique : des failles dans le chargeur GRUB ou dans des utilitaires de boot signés Microsoft ont permis de contourner Secure Boot → Microsoft a publié leurs hashes en `dbx` via Windows Update.

### Points d'attention en entreprise

- Les mises à jour `dbx` arrivent via **Windows Update** (KB spécifiques) ou les **mises à jour firmware** du constructeur.
- Une `dbx` obsolète = des chargeurs vulnérables toujours acceptés → **vérifier que les mises à jour `dbx` sont bien appliquées sur le parc** (surtout après les correctifs majeurs).
- ⚠️ **Effet de bord connu :** une mise à jour `dbx` trop agressive peut bloquer un **double-boot Linux** dont le shim a été révoqué → prévoir une procédure (section 74).

### L'échéance des certificats 2026

Les certificats Secure Boot Microsoft de 2011 (`...2011`) arrivent à expiration : Microsoft a publié de nouveaux certificats (`...2023`) et pousse leur déploiement via Windows Update et les firmwares OEM. **En entreprise :**

- [ ] Vérifier que les postes reçoivent les mises à jour de clés (Windows Update actif ou WSUS avec la catégorie).
- [ ] Sur les images master (MDT/SCCM), intégrer un firmware récent incluant les nouvelles clés.
- [ ] Tester le déploiement sur un lot pilote avant généralisation (un raté = parc qui ne boote plus).

---

## 22. Mode Standard vs mode Custom

| | Mode Standard | Mode Custom |
|---|---|---|
| Clés | Clés d'usine (Microsoft + OEM) | Modifiables par l'administrateur |
| Usage | **99 % des entreprises** | Cas spéciaux : OS maison, chargeurs auto-signés, R&D |
| Sécurité | Maximale par défaut | Dépend de la rigueur de l'admin |
| Gestion | Automatique (Windows Update) | Manuelle (import/export de clés) |

### Quand utiliser le mode Custom

- Déployer un **chargeur auto-signé** (outil de déploiement maison, noyau Linux custom) sans passer par le shim Microsoft.
- **Enrôler sa propre PK** d'entreprise (modèle « je suis le propriétaire de la plateforme », très avancé, rarement justifié).
- Signer des binaires EFI internes avec une CA d'entreprise ajoutée à `db`.

### Procédure type (mode Custom)

```
1. Passer le firmware en "Custom Mode".
2. Exporter/sauvegarder les clés d'usine (fichiers .auth / .esl selon firmware).
3. Enrôler la clé : importer le certificat (.cer/.der) dans db.
   (Certains firmwares exigent un fichier signé .auth ; d'autres acceptent
   l'import direct depuis une clé USB en FAT32.)
4. Signer le binaire EFI avec sbsign (Linux) ou signtool (Windows).
5. Tester le boot, puis verrouiller (repasser en User Mode si PK posée).
```

> 💡 **Recommandation :** restez en **mode Standard** sauf besoin métier démontré. Le mode Custom multiplie les risques d'erreur (machine qui ne boote plus après un import raté) et la charge de gestion des clés.

---

## 23. Activer Secure Boot : procédure

### Préconditions

- [ ] Firmware en **UEFI natif** (CSM **désactivé**).
- [ ] Disque système en **GPT**.
- [ ] OS compatible : Windows 8+ (64 bits), Windows 11 (requis), Linux avec shim signé.

### Procédure générique

```
1. Redémarrer → entrer dans le setup UEFI (F2 / DEL / F10 / F12 selon marque).
2. Aller dans l'onglet Boot ou Security.
3. Secure Boot → Enabled.
   - Si l'option est grisée : vérifier que le mode est UEFI (pas Legacy/CSM),
     et que les clés sont provisionnées (Secure Boot Mode = Standard,
     état ≠ "Setup Mode").
4. Si "Setup Mode" : choisir "Provision Factory Keys" / "Restore Factory Keys"
   (selon firmware) pour installer les clés d'usine.
5. Sauvegarder (F10) et redémarrer.
6. Vérifier dans Windows (section suivante).
```

### Cas particulier : activer Secure Boot sur un Windows installé en Legacy

Impossible directement. Ordre des opérations :

```
1. mbr2gpt /convert (section 8) — BitLocker suspendu !
2. Basculer le firmware en UEFI (CSM off).
3. Activer Secure Boot.
4. Vérifier : Confirm-SecureBootUEFI → True.
```

### Déploiement en masse

- **Dell :** Dell Command Configure (`cctk --secureboot=enable`), ou profils BIOS via SCCM/Intune.
- **HP :** HP BIOS Configuration Utility (`BiosConfigUtility.exe`), ou HP Manageability Kit.
- **Lenovo :** ThinkPad Setup Settings Capture/Deploy, ou WMI (`Lenovo_SetBiosSetting`).
- **Intune :** profils de configuration (Endpoint Security) pour exiger Secure Boot.

---

## 24. Vérifier Secure Boot : Confirm-SecureBootUEFI et msinfo32

```powershell
# Méthode 1 : cmdlet officiel (retourne True / False)
Confirm-SecureBootUEFI

# Méthode 2 : msinfo32 → ligne "État du démarrage sécurisé"
msinfo32

# Méthode 3 : registre (utile en inventaire, sans cmdlet)
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\SecureBoot\State' |
    Select-Object UEFISecureBootEnabled
# 1 = activé, 0 = désactivé

# Méthode 4 : System Information via CIM
Get-CimInstance -ClassName Win32_ComputerSystem |
    Select-Object Name, Domain, Manufacturer, Model
```

### Interpréter les résultats

| `Confirm-SecureBootUEFI` | Registre | Signification |
|---|---|---|
| `True` | 1 | Secure Boot actif ✅ |
| `False` | 0 | Secure Boot désactivé ou firmware en Legacy ⚠️ |
| Erreur « cmdlet not supported » | — | Machine en **Legacy BIOS** ou firmware sans Secure Boot ❌ |

### Script de vérification rapide (une machine)

```powershell
#requires -RunAsAdministrator
$sb = Confirm-SecureBootUEFI
$tpm = Get-Tpm
[PSCustomObject]@{
    ComputerName      = $env:COMPUTERNAME
    SecureBoot        = $sb
    TpmPresent        = $tpm.TpmPresent
    TpmReady          = $tpm.TpmReady
    Conforme_Win11    = ($sb -and $tpm.TpmPresent -and $tpm.TpmReady)
} | Format-List
```

---

## 25. Mise à jour des clés et certificats Secure Boot

### Pourquoi mettre à jour

- **Révocations (dbx)** : bloquer les chargeurs vulnérables (patchs de sécurité).
- **Nouveaux certificats (db)** : remplacer les certificats expirés (échéance 2026 des certificats 2011).
- **Nouveaux OS/chargeurs** : ajouter de nouvelles autorités (rare en Standard).

### Canaux de distribution

| Canal | Ce qu'il met à jour |
|---|---|
| **Windows Update** | `dbx` (KB de révocation), nouveaux certificats `db`/`KEK` Microsoft |
| **Mise à jour firmware OEM** | Clés OEM, `dbx` embarquée, nouveaux certificats |
| **Manuel (mode Custom)** | Import depuis clé USB dans le setup |

### Vérifier l'état des mises à jour dbx

```powershell
# Voir les KB de mise à jour dbx installées
Get-HotFix | Where-Object { $_.HotFixID -match 'KB5012170|KB5025885|KB5036212' } |
    Select-Object HotFixID, InstalledOn, Description

# Journal des mises à jour Secure Boot (observateur d'événements)
# → Journaux Windows → Système, source "SecureBoot" / ID liés au firmware
Get-WinEvent -LogName System -MaxEvents 50 |
    Where-Object { $_.ProviderName -match 'SecureBoot|Kernel-Boot' } |
    Select-Object TimeCreated, Id, LevelDisplayName, Message |
    Format-Table -AutoSize -Wrap
```

### Plan d'action entreprise (renouvellement des certificats)

```
1. INVENTAIRE : lister les modèles du parc et leurs versions de firmware
   (script section 40 + Get-CimInstance Win32_BIOS).
2. PILOTE : appliquer firmware + KB sur 5-10 machines représentatives,
   vérifier Confirm-SecureBootUEFI et le boot (y compris double-boot).
3. DÉPLOIEMENT : vagues de 25 %, avec point de contrôle BitLocker
   (suspendre BitLocker avant flash, section 37).
4. CONTRÔLE : re-vérifier Secure Boot + dbx à jour sur 100 % du parc.
5. DOCUMENTATION : noter les versions de firmware validées par modèle.
```

> ⚠️ **Ne jamais bloquer les mises à jour dbx** par « prudence » : une dbx obsolète laisse passer des chargeurs vulnérables connus. Le risque d'une dbx à jour (rare blocage d'un vieux chargeur) est très inférieur au risque inverse.

---

## 26. Secure Boot et Linux en entreprise

### Le mécanisme : le shim

La plupart des distributions « Secure Boot compatibles » (Ubuntu, Red Hat/RHEL, SUSE, Debian depuis Bullseye...) utilisent un **shim** :

```
Firmware → shimx64.efi (signé Microsoft, clé "UEFI CA 2011" dans db)
           └─ vérifie grubx64.efi avec la clé de la distribution (MOK)
              └─ GRUB vérifie le noyau signé
```

**MOK** (*Machine Owner Key*) : base de clés gérée par l'utilisateur/la distribution, distincte de `db`. Permet d'ajouter ses propres clés (ex. modules DKMS VirtualBox/NVIDIA auto-signés) via `mokutil`.

### Commandes utiles (Linux)

```bash
# Vérifier Secure Boot
mokutil --sb-state
# ou
bootctl status | grep -i secure

# Lister les clés MOK enrôlées
mokutil --list-enrolled

# Enrôler une nouvelle clé MOK (demande un mot de passe, validé au reboot)
mokutil --import ma-cle.der
```

### En entreprise

- **Postes Linux :** choisir une distribution avec shim signé ; documenter l'enrôlement MOK pour les modules propriétaires.
- **Double-boot :** Windows Update peut mettre à jour `dbx` et révoquer un vieux shim → prévoir la procédure de mise à jour du Linux (section 78).
- **Serveurs Linux :** Secure Boot activable sur la plupart des serveurs récents ; vérifier la compatibilité de la distribution avant.

---

## 27. Dépannage Secure Boot : boot bloqué

### Symptômes typiques

| Message / écran | Cause probable |
|---|---|
| `Secure Boot Violation — Invalid signature detected` | Chargeur non signé ou signature inconnue |
| Écran rouge du firmware (certains OEM) | Échec de vérification |
| Boot direct sur le setup / « No bootable device » | Entrée de boot pointant vers un `.efi` refusé |
| Windows démarre, puis écran bleu `0xc0000428` | Pilote non signé bloqué par Code Integrity |

### Arbre de décision

```
Secure Boot bloque le démarrage
│
├─► C'est un OS/outil que JE veux booter (Linux, utilitaire, WinPE maison) ?
│   ├─ OUI → 1) Vérifier qu'il existe une version signée (shim, WinPE signé).
│   │         2) Sinon : signer le binaire (mode Custom) OU désactiver
│   │            temporairement Secure Boot (tracer l'action !).
│   └─ NON (c'est Windows qui ne boote plus) → continuer
│
├─► Changement récent ? (MAJ firmware, dbx, nouveau disque, clonage)
│   ├─ OUI → Restaurer l'état antérieur / réparer le boot (bcdboot, §15).
│   └─ NON → continuer
│
├─► Vérifier l'intégrité : Secure Boot est-il en "Deployed Mode" avec les
│   clés d'usine ? (setup → Secure Boot → Key Management)
│   ├─ Clés effacées / Setup Mode → restaurer les clés d'usine.
│   └─ Clés OK → continuer
│
└─► Piste malware/bootkit ? (chargeur modifié)
    → Analyser depuis un support sain, comparer les hashes des .efi
      avec une machine de référence.
```

### Commandes de diagnostic

```powershell
# Depuis WinRE : vérifier que les fichiers de boot existent et sont signés
# (comparer avec une machine saine)
dir S:\EFI\Microsoft\Boot\bootmgfw.efi
dir S:\EFI\Boot\bootx64.efi

# Vérifier l'état Secure Boot vu par Windows (si Windows démarre encore)
Confirm-SecureBootUEFI
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\SecureBoot\State'
```

> 🔒 **Règle d'or :** désactiver Secure Boot pour « faire passer » un problème, c'est comme enlever la porte parce que la serrure coince. Autorisé en dépannage, **toujours temporaire**, toujours tracé, toujours réactivé après.

---

## 28. TPM 2.0 : principe et racine de confiance matérielle

Le **TPM** (*Trusted Platform Module*) est un composant cryptographique matériel (puce dédiée ou firmware) qui fournit une **racine de confiance** : un point d'ancrage inviolable pour la sécurité du système.

### Ce que fait un TPM

| Fonction | Détail |
|---|---|
| **Génération de clés** | Crée des clés RSA/ECC **à l'intérieur** de la puce ; la clé privée ne sort jamais. |
| **Stockage protégé** | Clés, certificats, secrets scellés dans la mémoire interne. |
| **Chiffrement / signature** | Opérations RSA, ECC, SHA-1/SHA-256, HMAC réalisées par la puce. |
| **Mesures d'intégrité (PCR)** | Enregistre les hashes des composants de démarrage (voir section 35). |
| **Scellage (sealing)** | Chiffre des données liées à un état PCR : elles ne se déchiffrent que si le boot est identique. |
| **Générateur aléatoire** | Source d'aléa matérielle (TRNG) pour les clés. |
| **Compteurs monotones** | Compteurs anti-rejeu (anti-rollback). |

### Hiérarchie des clés TPM 2.0

```
Endorsement Key (EK)      ← clé RSA/ECC unique, gravée à la fabrication,
                            certifiée par le fabricant. Identité du TPM.
   └─ Storage Root Key (SRK)
         └─ Clés applicatives : BitLocker, Windows Hello, certificats,
            clés d'attestation, Credential Guard...
```

### À quoi sert concrètement le TPM en entreprise

- **BitLocker** : la clé de chiffrement du disque est scellée au TPM (+ PCR) → le disque est illisible sur une autre machine.
- **Windows Hello for Business** : clés d'authentification protégées par le TPM.
- **Measured Boot / attestation** : prouver à distance qu'une machine a démarré sainement (scénarios Zero Trust, Conditional Access).
- **Credential Guard / HVCI** : isolation des secrets basée sur la virtualisation + TPM.
- **VPN / Wi-Fi EAP-TLS** : certificats machine stockés dans le TPM.

> 💡 **En résumé :** le TPM est le coffre-fort matériel du PC. Sans lui, BitLocker n'est qu'un cadenas logiciel ; avec lui, la clé de chiffrement est liée au matériel *et* à l'intégrité du démarrage.

---

## 29. TPM 1.2 vs TPM 2.0

| Critère | TPM 1.2 | TPM 2.0 |
|---|---|---|
| Standard | TCG, 2011 | TCG, 2014 (ISO/IEC 11889) |
| Algorithmes | RSA, SHA-1 **imposés** | **Agile** : RSA, ECC, SHA-256, etc. (négociables) |
| Hiérarchies de clés | 1 (Storage) | 3 (Endorsement, Storage, Platform) |
| PCR | 24, SHA-1 uniquement | 24, **banques SHA-1 + SHA-256** |
| Autorisation | HMAC simple | Politiques d'autorisation riches |
| Support Windows | Jusqu'à Windows 10 | **Requis pour Windows 11** |
| Support constructeurs | Abandonné | Standard depuis ~2018 |

### Migration 1.2 → 2.0

Certains TPM (notamment Infineon sur des Dell/Lenovo 2016-2018) permettent une **conversion firmware 1.2 ↔ 2.0** via un utilitaire du constructeur. Points d'attention :

```
1. La conversion EFFACE le TPM (toutes les clés perdues).
2. Suspendre/désactiver BitLocker AVANT (sinon : demande de clé de récupération).
3. Sauvegarder les clés de récupération BitLocker (AD/Entra ID, §38).
4. Après conversion : réactiver, re-provisionner (Initialize-Tpm), réactiver BitLocker.
```

> ⚠️ Sur du matériel récent, le TPM est nativement 2.0 : rien à convertir. La migration 1.2→2.0 ne concerne que le parc 2016-2019.

---

## 30. TPM discret vs TPM firmware (fTPM / Intel PTT)

| | TPM discret (dTPM) | TPM firmware (fTPM / Intel PTT) |
|---|---|---|
| Implémentation | **Puce dédiée** soudée (ex. Infineon, STMicro) | Code s'exécutant dans le **firmware** (AMD fTPM / Intel PTT) |
| Sécurité | Isolation matérielle maximale | Isolation via extensions CPU (moins « dur » qu'une puce) |
| Coût | + composant | Gratuit (inclus au CPU/chipset) |
| Activation | Souvent activé par défaut (pro) | Parfois **désactivé par défaut** → à activer dans le setup |
| Vulnérabilités connues | Failles Infineon RSA (2017, CVE-2017-15361) → MAJ firmware | Failles de contournement sur certaines plateformes (faible impact pratique) |
| Cas d'usage | Postes sensibles, serveurs | **Standard sur le parc bureautique** |

### Identifier le type de TPM

```powershell
Get-Tpm | Select-Object TpmPresent, TpmReady, ManufacturerId, ManufacturerVersion

# ManufacturerId (hex → ASCII) :
#  0x49465800 = "IFX" (Infineon, souvent discret)
#  0x414D4400 = "AMD" (souvent fTPM)
#  0x494E5443 = "INTC" (Intel PTT)
```

```powershell
# Décoder le ManufacturerId en lettres
$tpm = Get-Tpm
$bytes = [BitConverter]::GetBytes($tpm.ManufacturerId)
[System.Text.Encoding]::ASCII.GetString($bytes).Trim([char]0)
```

### Recommandation

- **Parc standard :** le fTPM/PTT suffit (c'est ce que Microsoft valide pour Windows 11).
- **Postes sensibles** (direction, finance, R&D) : préférer le TPM discret si l'option existe.
- Dans tous les cas : **mettre à jour le firmware du TPM** (section 56).

---

## 31. Activer le TPM dans le firmware

### Chemins typiques par constructeur

| Constructeur | Chemin dans le setup | Nom de l'option |
|---|---|---|
| **Dell** | Security → TPM 2.0 Security | `TPM On`, `TPM 2.0 Security On` |
| **HP** | Security → TPM Embedded Security | `TPM Device` = Available, `TPM State` = On |
| **Lenovo** | Security → Security Chip | `Security Chip` = Enabled (Discrete / Intel PTT / AMD fTPM) |

### Procédure générique

```
1. Entrer dans le setup UEFI.
2. Security → activer le TPM (On / Enabled / Available).
3. Vérifier la version : TPM 2.0 (si choix 1.2/2.0 → 2.0).
4. Sauvegarder et redémarrer.
5. Dans Windows : Get-Tpm → TpmPresent = True, TpmReady = True.
   Si TpmReady = False : initialiser (Initialize-Tpm ou tpm.msc → "Préparer le TPM").
```

### Activation à distance / en masse

```powershell
# Dell : Dell Command Configure
cctk --tpm=on
cctk --tpmactivation=activate

# Lenovo : via WMI (exemple)
(Get-WmiObject -Namespace root\wmi -Class Lenovo_SetBiosSetting).SetBiosSetting("SecurityChip,Active")

# HP : HP BIOS Configuration Utility (fichier de config)
# Security/TPM Device=Available ; Security/TPM State=On
```

> ⚠️ **L'activation du TPM peut nécessiter un redémarrage + validation physique** (appuyer sur une touche pour confirmer l'activation : protection anti-malware). Prévoyez-le dans le planning de déploiement : pas de « tout à distance sans toucher les machines ».

---

## 32. Vérifier le TPM : Get-TPM et tpm.msc

### Get-TPM : la méthode scriptable

```powershell
Get-Tpm | Format-List *
```

Sortie typique d'un TPM sain :

```
TpmPresent                : True
TpmReady                  : True
TpmEnabled                : True
TpmActivated              : True
TpmOwned                  : True
RestartPending            : False
ManufacturerId            : 1229346816
ManufacturerIdTxt         : IFX
ManufacturerVersion       : 7.85.4555.0
ManagedAuthLevel          : Full
OwnerAuth                 :
OwnerClearDisabled        : False
AutoProvisioning          : Enabled
LockedOut                 : False
LockoutHealTime           : 2 hours
LockoutCount              : 0
LockoutMax                : 32
SelfTest                  : {}
```

### Interprétation des champs clés

| Champ | Signification | Valeur attendue |
|---|---|---|
| `TpmPresent` | Puce détectée par Windows | `True` |
| `TpmReady` | Prêt à l'emploi (provisionné) | `True` |
| `TpmEnabled` / `TpmActivated` | Activé dans le firmware | `True` |
| `TpmOwned` | Propriétaire défini (provisionné) | `True` |
| `AutoProvisioning` | Windows peut provisionner seul | `Enabled` |
| `LockedOut` | Verrouillage anti-bruteforce actif | `False` (normal) |
| `RestartPending` | Un redémarrage est requis | `False` (sinon : rebooter) |

### tpm.msc : la console graphique

```
Win + R → tpm.msc
```

Affiche : fabricant, version de la spécification (2.0), état (« Le TPM est prêt à être utilisé »), et les actions : **Préparer le TPM**, **Effacer le TPM**, **Activer/Désactiver**.

### Diagnostic rapide en une ligne

```powershell
$t = Get-Tpm
if ($t.TpmPresent -and $t.TpmReady) { "TPM OK" }
elseif ($t.TpmPresent) { "TPM présent mais non prêt → Initialize-Tpm puis reboot" }
else { "TPM ABSENT → vérifier le firmware (Security → TPM On)" }
```

---

## 33. Clear TPM : quand, pourquoi, précautions

**Effacer le TPM** réinitialise la puce à l'état d'usine : toutes les clés, tous les secrets scellés sont **détruits définitivement**.

### Quand c'est nécessaire

| Situation | Pourquoi |
|---|---|
| **Cession / reconditionnement** d'un poste | Supprimer les clés de l'ancien utilisateur (BitLocker, Hello, certificats) |
| **Changement de propriétaire** (sortie d'un collaborateur sensible) | Hygiène cryptographique |
| **TPM corrompu / incohérent** | Dernier recours après échec de provisioning |
| **Recyclage vers un autre usage** (poste → serveur de test) | Repartir sur une base saine |

### Quand c'est INTERDIT (sans précautions)

| Situation | Risque |
|---|---|
| BitLocker actif avec protecteur TPM | **Disque illisible** → clé de récupération obligatoire |
| Windows Hello for Business déployé | L'utilisateur devra ré-enrôler ses identifiants |
| Certificats machine dans le TPM (EAP-TLS, VPN) | Perte des certificats → réémission PKI |

### Procédure sécurisée

```powershell
# 1. VÉRIFIER BitLocker (si actif → ne PAS continuer sans la clé de récupération)
manage-bde -status C:

# 2. Sauvegarder la clé de récupération (elle doit être dans l'AD/Entra ID)
manage-bde -protectors -get C: -type RecoveryPassword

# 3. Suspendre BitLocker (le temps de l'opération)
Suspend-BitLocker -MountPoint "C:" -RebootCount 1

# 4. Effacer le TPM (demande un redémarrage + validation physique)
Clear-Tpm
# ou via tpm.msc → "Effacer le TPM" → redémarrer → confirmer (F12 ou touche indiquée)

# 5. Après redémarrage : re-provisionner
Initialize-Tpm
# (Windows le fait aussi automatiquement si AutoProvisioning = Enabled)

# 6. Réactiver BitLocker
Resume-BitLocker -MountPoint "C:"
```

> 🔴 **Règle absolue :** jamais de `Clear-Tpm` sans avoir **vérifié et sauvegardé la clé de récupération BitLocker**. C'est l'erreur qui transforme un poste en presse-papier.

---

## 34. Provisioning et attestation du TPM

### Le provisioning (prise de possession)

Le **provisioning** prépare le TPM : création de la hiérarchie de stockage (SRK), définition du propriétaire. Sur Windows moderne, c'est **automatique** :

```powershell
# Vérifier l'auto-provisioning
Get-Tpm | Select-Object AutoProvisioning, TpmReady, TpmOwned

# Forcer le provisioning manuel (si TpmReady = False)
Initialize-Tpm
# → redémarrage souvent nécessaire

# Désactiver l'auto-provisioning (durcissement avancé, rare)
Disable-TpmAutoProvisioning
# Réactiver :
Enable-TpmAutoProvisioning
```

### L'attestation : prouver l'intégrité à distance

L'**attestation** permet à un serveur de vérifier *à distance* qu'une machine a démarré sainement :

```
1. Au boot, chaque composant est "mesuré" (hash SHA-256) dans les PCR.
2. Le TPM signe les valeurs PCR avec une clé d'attestation (AIK).
3. Le serveur de vérification compare les mesures à des valeurs de référence.
4. Conforme → accès accordé (ex. Conditional Access, accès VPN Zero Trust).
   Non conforme → accès refusé / quarantaine.
```

### En entreprise : où on la rencontre

| Solution | Usage de l'attestation |
|---|---|
| **Microsoft Entra Conditional Access** | Conformité de l'appareil (Intune + attestation) |
| **Windows Autopilot** | Vérification matérielle à l'enrôlement |
| **Solutions Zero Trust / NAC** | Preuve d'intégrité avant accès réseau |
| **Credential Guard** | Isolation basée sur l'intégrité mesurée |

---

## 35. PCR : Platform Configuration Registers

Les **PCR** sont des registres du TPM qui accumulent les **mesures** (hashes) des composants de démarrage. On ne les écrit pas : on les **étend** (`PCR_new = Hash(PCR_old || mesure)`), ce qui rend toute falsification détectable.

### Les 24 PCR et leur usage (TPM 2.0, profil PC Client)

| PCR | Mesuré par | Contenu typique |
|---|---|---|
| **0** | Firmware (SRTM) | Code du firmware / BIOS |
| **1** | Firmware | Configuration du firmware |
| **2** | Firmware | Code des ROMs d'extension (cartes) |
| **3** | Firmware | Données des ROMs d'extension |
| **4** | Chargeur (MBR/GPT) | **MBR / gestionnaire de boot** (bootmgfw.efi) |
| **5** | Chargeur | Configuration du gestionnaire (BCD) |
| **6** | — | Données spécifiques plateforme |
| **7** | Firmware | **État Secure Boot** (clés, mode) |
| **8–15** | OS | Composants OS (winload, noyau...) — usage OS |
| **16–23** | — | Usage libre / débogage |

### Les PCR critiques pour BitLocker

Par défaut, BitLocker scelle la clé sur les PCR **0, 2, 4, 11** (profil Windows standard) :

```powershell
# Voir le profil PCR utilisé par BitLocker
manage-bde -protectors -get C: -type TPM
# Affiche : "PCR Validation Profile: 0, 2, 4, 11"
```

| PCR | Pourquoi il compte pour BitLocker |
|---|---|
| 0 | **Firmware** → toute MAJ BIOS change PCR 0 → récupération BitLocker ! |
| 2 | ROMs d'extension → ajout d'une carte = changement |
| 4 | Chargeur de boot → modification du boot = changement |
| 7 | Secure Boot (non inclus par défaut, mais recommandé en durcissement) |
| 11 | Code du gestionnaire de démarrage |

### Lire les PCR (diagnostic)

```powershell
# Via tpm.msc → pas d'affichage direct des PCR.
# Via PowerShell (nécessite le module TPM) : pas de cmdlet natif simple.
# Méthode : journal des mesures dans l'observateur d'événements
Get-WinEvent -LogName "Microsoft-Windows-TPM/Operational" -MaxEvents 20 |
    Select-Object TimeCreated, Id, Message | Format-Table -Wrap
```

> 💡 **Lien direct avec la section 37 :** c'est parce que BitLocker scelle sa clé sur **PCR 0 (firmware)** que toute mise à jour du BIOS invalide le scellé et déclenche la demande de clé de récupération.

---

## 36. BitLocker et TPM : les protecteurs

BitLocker chiffre le volume ; les **protecteurs** (*key protectors*) contrôlent *comment* la clé de chiffrement (FVEK) est déverrouillée au démarrage.

### Les protecteurs principaux

| Protecteur | Commande | Sécurité | Ergonomie |
|---|---|---|---|
| **TPM seul** | `Enable-BitLocker C: -TpmProtector` | Bonne (lié au matériel + PCR) | ✅ Transparent (pas de saisie) |
| **TPM + PIN** | `Add-BitLockerKeyProtector C: -TpmAndPinProtector` | **Excellente** (double facteur) | ⚠️ PIN à chaque démarrage |
| **TPM + clé USB** | `-TpmAndStartupKeyProtector` | Excellente | ⚠️ Clé USB requise |
| **Mot de passe** | `-PasswordProtector` | Moyenne (attaques dico) | Saisie à chaque démarrage |
| **Clé de récupération** | `-RecoveryPasswordProtector` | Secours (48 chiffres) | À conserver hors machine |

### Recommandations par profil

| Profil de poste | Protecteur recommandé |
|---|---|
| Bureautique standard | **TPM seul** (+ clé de récupération séquestrée) |
| Portable / nomade | **TPM + PIN** (6+ chiffres, anti-vol) |
| Direction / finance / sensible | **TPM + PIN** (+ interdiction du mode veille simple → hibernation) |
| Serveur en datacenter | TPM seul (redémarrage sans intervention) |

### Commandes essentielles

```powershell
# Activer BitLocker (TPM seul) avec clé de récupération
Enable-BitLocker -MountPoint "C:" -TpmProtector -RecoveryPasswordProtector

# Ajouter un PIN (TPM+PIN) — demande le PIN de manière interactive
Add-BitLockerKeyProtector -MountPoint "C:" -TpmAndPinProtector

# Lister les protecteurs
(Get-BitLockerVolume -MountPoint "C:").KeyProtector |
    Select-Object KeyProtectorType, KeyProtectorId

# Sauvegarder la clé de récupération dans l'AD
Backup-BitLockerKeyProtector -MountPoint "C:" -KeyProtectorId '{GUID-de-la-recovery}'

# État du chiffrement
Get-BitLockerVolume -MountPoint "C:" |
    Select-Object MountPoint, VolumeStatus, EncryptionPercentage, KeyProtector
```

### Choisir l'algorithme de chiffrement (GPO)

```
Stratégie : Configuration ordinateur → Modèles d'administration →
            Composants Windows → BitLocker →
            "Choisir la méthode de chiffrement de lecteur"
Recommandé : XTS-AES 256 bits (disques fixes et OS, Windows 10 1511+).
```

---

## 37. Pourquoi une MAJ du BIOS déclenche la récupération BitLocker

### Le mécanisme (rappel PCR)

```
1. BitLocker scelle la clé (FVEK) : "ne te déchiffre que si
   PCR[0,2,4,11] == valeurs enregistrées au moment du chiffrement".
2. Mise à jour du BIOS → le code du firmware change
   → au prochain boot, PCR 0 = NOUVELLE valeur.
3. Le TPM refuse de desceller : les PCR ne correspondent plus.
4. BitLocker bascule en mode récupération → demande la clé à 48 chiffres.
```

### Ce n'est PAS un bug, c'est une protection

Si un attaquant remplaçait votre BIOS par une version piégée, **vous voudriez** que BitLocker refuse de déchiffrer silencieusement. La récupération est le comportement de sécurité attendu.

### Procédure de MAJ BIOS sur poste chiffré (sans se faire piéger)

```powershell
# AVANT le flash :
# 1. Vérifier que la clé de récupération est séquestrée (AD/Entra ID)
manage-bde -protectors -get C: -type RecoveryPassword

# 2. Suspendre BitLocker (1 redémarrage suffit pour un flash)
Suspend-BitLocker -MountPoint "C:" -RebootCount 1
# → le scellé PCR est temporairement contourné, pas le chiffrement :
#    les données restent chiffrées, seul le contrôle d'intégrité est levé.

# 3. Flasher le BIOS (section 52-55).

# 4. APRÈS le flash : BitLocker re-scelle automatiquement sur les
#    nouvelles valeurs PCR au redémarrage suivant.
#    Vérifier :
Get-BitLockerVolume -MountPoint "C:" | Select-Object VolumeStatus
# VolumeStatus doit revenir à "FullyEncrypted" (pas "EncryptionSuspended").
```

### Si la récupération se déclenche quand même

```
1. Récupérer la clé à 48 chiffres : AD (Attribut msFVE-RecoveryPassword),
   Entra ID (fiche appareil), ou copie papier/USB de l'utilisateur.
2. La saisir à l'invite bleue BitLocker.
3. Une fois dans Windows : Suspend-BitLocker puis Resume-BitLocker
   pour re-sceller sur les nouveaux PCR.
```

> 🔴 **Erreur classique n°1 en entreprise :** flasher 200 BIOS un vendredi soir sans suspendre BitLocker et sans vérifier le séquestre des clés → 200 utilisateurs bloqués lundi matin. **Toujours** : séquestre vérifié + suspension avant flash.

---

## 38. Gestion des clés de récupération BitLocker (AD / Entra ID)

Sans séquestre centralisé, une clé de récupération perdue = données perdues. Trois modèles :

### Modèle 1 : Active Directory local (GPO)

```
GPO : Configuration ordinateur → Stratégies → Modèles d'administration →
      Composants Windows → BitLocker → Lecteurs du système d'exploitation →
      "Choisir comment les lecteurs protégés par BitLocker peuvent être récupérés"
  ✅ Stocker les informations de récupération dans AD DS
  ✅ Exiger la sauvegarde AD avant activation (bloque si échec)
```

Attributs AD utilisés : `msFVE-RecoveryPassword`, `msFVE-KeyPackage`, `msFVE-VolumeGuid` (sur l'objet ordinateur).

```powershell
# Retrouver la clé de récupération d'un poste dans l'AD
Get-ADObject -Filter "objectClass -eq 'msFVE-RecoveryInformation'" `
    -SearchBase (Get-ADComputer PC-0427).DistinguishedName `
    -Properties msFVE-RecoveryPassword |
    Select-Object Name, msFVE-RecoveryPassword
```

### Modèle 2 : Microsoft Entra ID (Intune / AAD Join)

- La clé est sauvegardée automatiquement si la stratégie Intune l'exige.
- Consultation : **Centre d'administration Entra → Appareils → [appareil] → Clés de récupération**.
- En PowerShell (module Microsoft.Graph) :

```powershell
# Lister les clés BitLocker d'un appareil Entra
Connect-MgGraph -Scopes BitLockerKey.Read.All
$device = Get-MgDevice -Filter "displayName eq 'PC-0427'"
Get-MgInformationProtectionBitlockerRecoveryKey -Filter "deviceId eq '$($device.DeviceId)'" |
    Select-Object Id, CreatedDateTime
# Puis récupérer la clé elle-même :
Get-MgInformationProtectionBitlockerRecoveryKey -BitlockerRecoveryKeyId '<id>' |
    Select-Object -ExpandProperty Key
```

### Modèle 3 : fichier / papier (petites structures)

```powershell
# Exporter la clé dans un fichier (à stocker CHIFFRÉ, hors du poste)
(Get-BitLockerVolume -MountPoint "C:").KeyProtector |
    Where-Object KeyProtectorType -eq RecoveryPassword |
    Select-Object KeyProtectorId, RecoveryPassword |
    Export-Csv C:\sequestre\cles-bitlocker.csv -NoTypeInformation
```

> 🔒 **La clé de récupération est aussi sensible que les données** : stockage chiffré, accès restreint (groupe dédié), journalisation des consultations.

---

## 39. Windows 11 : exigences TPM 2.0 et Secure Boot

### Exigences matérielles officielles (disque système)

| Exigence | Détail |
|---|---|
| **TPM 2.0** | Activé et provisionné (fTPM/PTT accepté) |
| **Secure Boot** | Compatible et **activé** |
| **UEFI** | Mode natif (CSM désactivé) |
| **GPT** | Style de partition du disque système |
| CPU / RAM / stockage | Selon liste Microsoft (CPU 1 GHz+ 2 cœurs 64 bits, 4 Go RAM, 64 Go disque) |

### Vérifier la compatibilité (méthodes officielles)

```powershell
# Méthode 1 : les 3 piliers en une fois
[PSCustomObject]@{
    SecureBoot = Confirm-SecureBootUEFI
    TPM_Present = (Get-Tpm).TpmPresent
    TPM_Ready   = (Get-Tpm).TpmReady
    GPT_System  = ((Get-Disk | Where-Object IsSystem).PartitionStyle -eq 'GPT')
    UEFI_Boot   = (Test-Path "$env:SystemRoot\Panther\setupact.log") # indice, voir §4
}

# Méthode 2 : PC Health Check (outil graphique Microsoft, à déployer si besoin)

# Méthode 3 : registre — état de l'évaluation Windows 11
Get-ItemProperty 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\TargetVersionUpgradeExperienceIndicators\NI23H2' |
    Select-Object UpgEx, UpgExU
# UpgEx = "Green" → compatible ; "Red" → bloqué (voir les raisons dans les valeurs)
```

### Chemin de migration type (parc Windows 10 → 11)

```
Pour chaque poste :
1. Inventaire (§40) → classer : OK / TPM à activer / Legacy à convertir / incompatible.
2. Activer TPM + Secure Boot dans le firmware (vague pilote d'abord).
3. Si Legacy : mbr2gpt /convert (§8) + bascule UEFI.
4. Vérifier les 4 piliers (script ci-dessus).
5. Déployer Windows 11 (MDT/SCCM/Intune, image avec firmware à jour).
6. Post-installation : re-vérifier BitLocker + séquestre des clés.
```

> ⚠️ **Les contournements** (installation de Windows 11 sans TPM via clés de registre `BypassTPMCheck`) existent mais sont **à proscrire en entreprise** : pas de support Microsoft, pas de mises à jour garanties, faille de sécurité assumée. Un poste non conforme = un poste à remplacer, pas à bricoler.

---

## 40. Inventaire du parc : script PowerShell de conformité

Script complet : interroge les machines via CIM/WinRM et produit un CSV de conformité Windows 11 + sécurité firmware.

```powershell
#requires -Version 5.1
<#
.SYNOPSIS
    Inventaire firmware/sécurité du parc : TPM, Secure Boot, GPT/UEFI, BitLocker, BIOS.
.EXAMPLE
    .\Invoke-FirmwareInventory.ps1 -ComputerNames (Get-Content .\parc.txt) |
        Export-Csv .\inventaire-firmware.csv -NoTypeInformation -Encoding UTF8
#>
[CmdletBinding()]
param(
    [Parameter(Mandatory)][string[]]$ComputerNames,
    [PSCredential]$Credential
)

$scriptBlock = {
    $tpm = $null; $sb = $null
    try { $tpm = Get-Tpm -ErrorAction Stop } catch { }
    try { $sb  = Confirm-SecureBootUEFI -ErrorAction Stop } catch { }

    $sysDisk = Get-Disk | Where-Object IsSystem | Select-Object -First 1
    $bde     = Get-BitLockerVolume -MountPoint ($env:SystemDrive) -ErrorAction SilentlyContinue
    $bios    = Get-CimInstance -ClassName Win32_BIOS
    $cs      = Get-CimInstance -ClassName Win32_ComputerSystem

    [PSCustomObject]@{
        ComputerName   = $env:COMPUTERNAME
        Manufacturer   = $cs.Manufacturer
        Model          = $cs.Model
        BiosVersion    = $bios.SMBIOSBIOSVersion
        BiosDate       = $bios.ReleaseDate
        TpmPresent     = $tpm.TpmPresent
        TpmReady       = $tpm.TpmReady
        TpmSpecVersion = $tpm.TpmPresent ? '2.0 (à confirmer via tpm.msc)' : 'N/A'
        SecureBoot     = $sb
        PartitionStyle = $sysDisk.PartitionStyle
        BitLocker      = $bde.VolumeStatus
        Win11_Ready    = ($sb -eq $true -and $tpm.TpmReady -and
                          $sysDisk.PartitionStyle -eq 'GPT')
    }
}

foreach ($computer in $ComputerNames) {
    try {
        $params = @{ ComputerName = $computer; ScriptBlock = $scriptBlock;
                     ErrorAction = 'Stop' }
        if ($Credential) { $params.Credential = $Credential }
        Invoke-Command @params
    } catch {
        [PSCustomObject]@{
            ComputerName = $computer; Manufacturer = 'INJOIGNABLE'
            Model = $_.Exception.Message; BiosVersion = ''; BiosDate = $null
            TpmPresent = $null; TpmReady = $null; TpmSpecVersion = ''
            SecureBoot = $null; PartitionStyle = ''; BitLocker = ''
            Win11_Ready = $false
        }
    }
}
```

### Exploiter le CSV

```powershell
$inv = Import-Csv .\inventaire-firmware.csv

# Synthèse
$inv | Group-Object { $_.Win11_Ready } | Select-Object Name, Count

# Postes à traiter : TPM absent ou non prêt
$inv | Where-Object { $_.TpmReady -ne 'True' } |
    Select-Object ComputerName, Manufacturer, Model, TpmPresent, TpmReady |
    Export-Csv .\a-traiter-tpm.csv -NoTypeInformation

# Postes Legacy (non GPT)
$inv | Where-Object { $_.PartitionStyle -ne 'GPT' } |
    Select-Object ComputerName, Model

# BIOS vieux de plus de 2 ans (candidats à la MAJ firmware)
$inv | Where-Object { $_.BiosDate -and ([datetime]$_.BiosDate) -lt (Get-Date).AddYears(-2) } |
    Select-Object ComputerName, Model, BiosVersion, BiosDate
```

> 💡 **Prérequis réseau :** WinRM activé sur les postes (`Enable-PSRemoting`), compte admin du domaine, pare-feu ouvert (TCP 5985). Pour un parc Intune-only (sans AD), préférez les rapports de conformité Intune / Endpoint Analytics.

---

## 41. Ordre de boot : configuration et bonnes pratiques

L'**ordre de boot** (*Boot Order / Boot Priority*) définit dans quel ordre le firmware essaie les périphériques.

### Ordre recommandé en entreprise

```
1. Disque dur / SSD interne (Windows Boot Manager)
2. Réseau (IPv4 PXE)          ← si déploiement réseau utilisé
3. USB                         ← ou désactivé (voir §44)
4. CD/DVD (si présent)
```

### Pourquoi mettre le disque en premier

- **Sécurité :** un attaquant avec une clé USB bootable ne peut pas démarrer dessus si l'USB est après le disque **et** protégé par mot de passe firmware.
- **Vitesse :** pas de timeout PXE/USB à chaque démarrage.
- **Fiabilité :** pas de boot accidentel sur une clé oubliée.

### Modifier l'ordre

```
Setup UEFI → Boot → Boot Priority Order → monter "Windows Boot Manager"
en premier (touches + / - ou F5/F6 selon firmware) → F10.
```

```powershell
# Via Windows : forcer Windows Boot Manager en premier dans l'ordre firmware
bcdedit /set '{fwbootmgr}' displayorder '{bootmgr}' /addfirst

# Boot unique sur le réseau au prochain redémarrage (utile pour réimager) :
# → le plus simple reste la touche F12 (menu de boot) au démarrage.
```

### Piège : l'ordre ne suffit pas

L'ordre de boot **n'est pas une sécurité** à lui seul : sans mot de passe firmware, n'importe qui change l'ordre en 30 secondes. **Ordre de boot + mot de passe admin UEFI + Secure Boot = le trio de base** (sections 43, 16).

---

## 42. Virtualisation matérielle : VT-x / AMD-V et VT-d / AMD-Vi

### Les deux couches à activer

| Technologie | Nom Intel | Nom AMD | Rôle |
|---|---|---|---|
| Virtualisation CPU | **VT-x** | **AMD-V** (SVM) | Permet à l'hyperviseur de s'exécuter (Hyper-V, VirtualBox, VMware...) |
| Virtualisation E/S | **VT-d** | **AMD-Vi** (IOMMU) | Isolation DMA : un périphérique ne peut pas lire la mémoire d'une autre VM (requis pour Credential Guard, VBS) |

### Où les activer

```
Setup UEFI → Advanced → CPU Configuration (ou Virtualization) :
  Intel Virtualization Technology → Enabled
  VT-d → Enabled
AMD : SVM Mode → Enabled ; IOMMU → Enabled
```

### Vérifier dans Windows

```powershell
# Méthode 1 : informations système
systeminfo | Select-String 'Hyper-V'

# Méthode 2 : CIM (détail)
Get-CimInstance -ClassName Win32_Processor |
    Select-Object Name, VirtualizationFirmwareEnabled, SecondLevelAddressTranslationExtensions

# Méthode 3 : le gestionnaire des tâches → onglet Performances → CPU
#   "Virtualisation : Activé"

# Méthode 4 : vérifier que l'hyperviseur tourne
Get-CimInstance -ClassName Win32_ComputerSystem |
    Select-Object HypervisorPresent
```

### Cas d'usage en entreprise

- **Hyper-V / WSL2 / Docker Desktop** : exigent VT-x/AMD-V.
- **Credential Guard / HVCI (VBS)** : exigent VT-x + VT-d (et UEFI + Secure Boot).
- **Postes développeurs** : activer systématiquement dans l'image master.

> ⚠️ Sur certains BIOS, VT-d est caché dans un sous-menu « Chipset » ou « Security ». Et sur les portables d'entrée de gamme anciens, l'option peut ne pas exister (VT-x présent mais non exposé).

---

## 43. Mot de passe BIOS/UEFI (admin / supervisor)

Le **mot de passe administrateur du firmware** verrouille l'accès au setup et (selon les modèles) le changement de l'ordre de boot.

### Les 3 niveaux (selon constructeurs)

| Niveau | Nom courant | Protège |
|---|---|---|
| **Admin / Supervisor** | `Admin Password`, `Supervisor Password` | L'accès au setup UEFI lui-même |
| **System / User** | `System Password`, `User Password` | Le **démarrage** de la machine (demandé avant le boot) |
| **HDD** | `HDD Password` | Le **disque** (chiffrement ATA, lié au disque, suit le disque) |

### Recommandations

- [ ] **Toujours** définir un mot de passe **admin** sur le parc (déploiement via outils constructeur).
- [ ] Mot de passe **système** (au boot) : uniquement sur postes sensibles — il bloque aussi le Wake-on-LAN et les redémarrages à distance non supervisés.
- [ ] Stocker les mots de passe dans un **coffre** (Keepass d'équipe, solution PAM) : un mot de passe admin perdu = procédure constructeur lourde (section 69).
- [ ] Ne **jamais** utiliser le même mot de passe admin sur tout le parc *sans* plan de rotation ; à défaut, au minimum un mot de passe par site/service.

### Déploiement en masse

```powershell
# Dell (Dell Command Configure) — définit le mot de passe admin
cctk --setuppwd=<nouveau> --valsetuppwd=<ancien si existant>

# HP (BiosConfigUtility) — via fichier config :
#   Setup Password
#       <nouveau encodé>

# Lenovo — via WMI :
(Get-WmiObject -Namespace root\wmi -Class Lenovo_SetBiosPassword).SetBiosPassword(
    "pap,$null,<nouveau>,ascii,us")   # pap = password admin present
```

> 🔴 **Mot de passe BIOS oublié ?** Voir le cas pratique 6 (section 69) : procédures par constructeur, jamais de « master password » universel fiable.

---

## 44. Désactivation des périphériques : USB boot et ports

### L'objectif

Empêcher le démarrage sur un support externe (clé USB, disque USB) et limiter l'exfiltration : c'est la contre-mesure directe à l'**evil maid** de base (section 58).

### Options typiques du setup

| Option | Effet | Recommandation |
|---|---|---|
| `USB Boot` / `Boot from USB` | Autorise/interdit le boot USB | **Disabled** (sauf exceptions documentées) |
| `USB Ports` (avant/arrière) | Désactive physiquement des ports | Désactiver les ports avant sur postes exposés |
| `Thunderbolt Boot` / `Pre-boot DMA` | Boot et DMA via Thunderbolt | **Disabled** sauf besoin (risque DMA — voir VT-d §42) |
| `SD Card Boot`, `CD/DVD Boot` | Boot sur ces supports | Disabled si inutilisés |
| `PXE Boot` | Boot réseau | Enabled uniquement si WDS/déploiement utilisé |

### En complément (couche Windows)

Le verrouillage firmware ne suffit pas : un OS démarré normalement voit toujours les clés USB. Compléter avec :

```powershell
# Restreindre le stockage USB amovible via stratégie (exemple registre)
# HKLM\SYSTEM\CurrentControlSet\Services\USBSTOR → Start = 4 (désactivé)
Set-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Services\USBSTOR' -Name Start -Value 4

# Via GPO (recommandé) : Modèles d'administration → Système →
# Accès au stockage amovible → "Disques amovibles : refuser l'accès en écriture"
```

> ⚠️ **Attention au support :** si vous désactivez totalement l'USB boot, prévoyez une procédure d'exception (mot de passe admin + réactivation temporaire tracée) pour les réinstallations sur site.

---

## 45. Wake-on-LAN (WoL)

Le **WoL** permet d'allumer une machine éteinte (mais branchée) en lui envoyant un **paquet magique** (*magic packet*) sur le réseau.

### Prérequis (les 3 couches)

```
1. FIRMWARE : Wake-on-LAN → Enabled (souvent dans Power Management).
   ⚠️ Sur portable : ne fonctionne généralement que sur secteur + parfois
   uniquement en Ethernet (pas en Wi-Fi, ou "Wake on WLAN" séparé).
2. WINDOWS : carte réseau → Propriétés → Gestion de l'alimentation →
   ✅ "Autoriser cet appareil à sortir l'ordinateur du mode veille"
   ✅ "Autoriser uniquement un paquet magique à sortir l'ordinateur du mode veille"
   + désactiver le "démarrage rapide" (fast startup) qui met en hybride.
3. RÉSEAU : le paquet magique est en broadcast → ne traverse pas les routeurs.
   Prévoir un relais par sous-réseau (agent, switch avec directed broadcast,
   ou outil de déploiement type SCCM).
```

### Le paquet magique

```
FF FF FF FF FF FF  + 16 × adresse MAC de la cible
(ex. pour AA:BB:CC:DD:EE:FF : 6 octets FF puis 16 répétitions de la MAC)
Envoyé en UDP, ports 7 ou 9, en broadcast.
```

### Envoyer un WoL en PowerShell

```powershell
function Send-WakeOnLan {
    param([Parameter(Mandatory)][string]$MacAddress)
    $mac = ($MacAddress -replace '[^0-9A-Fa-f]','')
    $packet = [byte[]]::new(102)
    for ($i = 0; $i -lt 6; $i++) { $packet[$i] = 0xFF }
    $macBytes = for ($i = 0; $i -lt 12; $i += 2) {
        [Convert]::ToByte($mac.Substring($i,2), 16) }
    for ($i = 0; $i -lt 16; $i++) {
        [Array]::Copy($macBytes, 0, $packet, 6 + $i*6, 6) }
    $udp = New-Object Net.Sockets.UdpClient
    $udp.Connect([Net.IPAddress]::Broadcast, 9)
    $udp.Send($packet, $packet.Length) | Out-Null
    $udp.Close()
    "Paquet magique envoyé à $MacAddress"
}

Send-WakeOnLan -MacAddress 'AA-BB-CC-DD-EE-FF'
```

### Vérifier la config côté Windows

```powershell
Get-NetAdapter | Select-Object Name, MacAddress, Status
# Puis, pour chaque carte :
Get-NetAdapterPowerManagement -Name 'Ethernet' |
    Select-Object Name, WakeOnMagicPacket, WakeOnPattern, DeviceSleepOnDisconnect
```

### En entreprise

- **Inventaire nocturne / patching :** allumer les postes à 3h, patcher, éteindre (GPO + tâche planifiée).
- **Limites :** ne traverse pas les VLANs sans relais ; incompatible avec le mot de passe système au boot (§43) ; sur les portables, préférer la sortie de veille moderne.

---

## 46. PXE boot : principe

Le **PXE** (*Preboot Execution Environment*) permet à une machine **sans OS** de démarrer sur une image fournie par le réseau. C'est le fondement du déploiement automatisé (WDS, SCCM, MDT).

### Séquence PXE (simplifiée)

```
① Client : firmware avec ROM PXE → DHCP Discover (avec options PXE)
② Serveur DHCP : répond avec IP + option 66 (serveur TFTP) / 67 (fichier boot)
   └─ ou : le serveur WDS répond lui-même (DHCP option 60 = "PXEClient")
③ Client : télécharge le chargeur via TFTP (wdsmgfw.efi en UEFI, wdsnbp.com en Legacy)
④ Chargeur : affiche le menu WDS (F12 pour boot réseau)
⑤ Téléchargement de l'image de démarrage (boot.wim) via TFTP/multicast
⑥ WinPE démarre → assistant d'installation / déploiement
```

### Composants réseau requis

| Composant | Rôle |
|---|---|
| **DHCP** | Attribue l'IP au client PXE |
| **TFTP** | Transfère le chargeur initial (petit, UDP) |
| **Serveur de déploiement** | WDS / SCCM : fournit les images |
| **DNS/AD** (WDS) | WDS s'intègre à l'AD en mode natif |

### Options DHCP critiques

| Option | Nom | Valeur |
|---|---|---|
| 60 | Class ID | `PXEClient` (posée par le serveur WDS lui-même) |
| 66 | Boot Server Host Name | Nom/IP du serveur TFTP |
| 67 | Bootfile Name | `boot\x64\wdsmgfw.efi` (UEFI) ou `boot\x64\wdsnbp.com` (Legacy) |

> ⚠️ **Ne pas poser les options 66/67 sur le DHCP si le WDS est sur le même réseau** : le WDS répond directement via l'option 60. Les options 66/67 ne servent que si DHCP et WDS sont séparés (et dans ce cas, ne pas cocher « ne pas écouter le port 67 » par erreur — voir §50).

### UEFI vs Legacy en PXE

| | Legacy PXE | UEFI PXE |
|---|---|---|
| Fichier de boot | `wdsnbp.com` | `wdsmgfw.efi` |
| Protocole | TFTP | TFTP (WDS classique) |
| Secure Boot | N/A | Le chargeur PXE doit être signé |

---

## 47. WDS : installation du rôle

**WDS** (*Windows Deployment Services*) est le rôle Windows Server qui fournit le boot PXE et la distribution d'images.

### Prérequis serveur

- [ ] Windows Server (2016/2019/2022/2025).
- [ ] **AD DS** (recommandé ; le mode autonome existe mais limité).
- [ ] **DHCP** (sur le serveur ou ailleurs — voir les options §46).
- [ ] **DNS**.
- [ ] Volume **NTFS** dédié pour le dépôt d'images (pas le volume système).

### Installation (PowerShell)

```powershell
# Installer le rôle WDS + outils d'administration
Install-WindowsFeature WDS -IncludeManagementTools

# Vérifier
Get-WindowsFeature WDS | Select-Object Name, Installed
```

### Initialisation du serveur

```powershell
# Initialiser WDS (mode intégré AD, dépôt sur E:\RemoteInstall)
wdsutil /Initialize-Server /Server:WDS-01 /RemInst:"E:\RemoteInstall"

# Mode autonome (sans AD) :
wdsutil /Initialize-Server /Server:WDS-01 /RemInst:"E:\RemoteInstall" /Standalone
```

### Structure du dépôt

```
E:\RemoteInstall
├── Boot
│   └── x64
│       ├── wdsmgfw.efi      ← chargeur PXE UEFI
│       ├── wdsnbp.com       ← chargeur PXE Legacy
│       └── boot.wim         ← image de démarrage (WinPE)
├── Images
│   └── install.wim          ← images d'installation
├── MGMT, Tmp, ...
└── WdsClientUnattend        ← fichiers de réponse (unattend.xml)
```

---

## 48. WDS : configuration initiale

### Assistant / paramètres essentiels (console `wdsmgmt.msc`)

```
1. Clic droit sur le serveur → Configurer le serveur.
2. Emplacement RemoteInstall : E:\RemoteInstall (volume NTFS non système).
3. Réponse PXE :
     ○ Ne répondre à aucun client  (désactivé)
     ○ Répondre uniquement aux clients connus
     ● Répondre à tous les clients  ← labo
   Avec "Exiger l'approbation de l'administrateur" en production
   (les machines inconnues attendent une validation).
4. DHCP :
   - Si DHCP sur le même serveur : ☑ "Ne pas écouter sur le port 67"
     ET ☑ "Configurer l'option DHCP 60 sur PXEClient".
   - Si DHCP ailleurs : ne rien cocher ici, configurer les options
     66/67 sur le serveur DHCP (ou utiliser un redirecteur).
```

### Équivalent en ligne de commande

```powershell
# Ne pas écouter le port 67 (DHCP cohabitant) + option 60
wdsutil /Set-Server /Server:WDS-01 /UseDHCPPorts:No /DHCPOption60:Yes

# Politique de réponse : répondre à tous (labo)
wdsutil /Set-Server /Server:WDS-01 /AnswerClients:All

# En production : uniquement les clients connus + approbation
wdsutil /Set-Server /Server:WDS-01 /AnswerClients:Known
wdsutil /Set-Server /Server:WDS-01 /NewMachinePolicy:AdminApproval

# Démarrer le service
Start-Service WDSServer
Set-Service WDSServer -StartupType Automatic
```

### Fichiers de réponse (automatisation)

```
- WdsClientUnattend.xml : automatise la phase PXE (langue, identifiants,
  choix de l'image) → déposé dans RemoteInstall\WdsClientUnattend.
- Unattend.xml d'image : automatise l'installation Windows elle-même
  (créé avec Windows System Image Manager, SIM).
```

---

## 49. WDS : images de démarrage et d'installation

### Les deux types d'images

| Image | Fichier | Rôle |
|---|---|---|
| **Image de démarrage** (*boot image*) | `boot.wim` | WinPE : l'environnement minimal qui démarre en PXE et lance l'installation |
| **Image d'installation** (*install image*) | `install.wim` | Le Windows à installer (avec vos customisations) |

### Ajouter les images

```powershell
# 1. Image de démarrage : depuis l'ISO Windows (sources\boot.wim)
wdsutil /Add-Image /Server:WDS-01 /ImageFile:"D:\sources\boot.wim" /ImageType:Boot

# 2. Image d'installation : depuis l'ISO (sources\install.wim)
wdsutil /Add-Image /Server:WDS-01 /ImageFile:"D:\sources\install.wim" /ImageType:Install /ImageGroup:"Windows 11"

# Lister les images
wdsutil /Get-Image /Server:WDS-01 /ImageType:Boot
wdsutil /Get-Image /Server:WDS-01 /ImageType:Install /ImageGroup:"Windows 11"
```

### Créer une image de capture (pour cloner un poste master)

```
1. Préparer le poste master (applications, réglages).
2. sysprep : C:\Windows\System32\Sysprep\sysprep.exe /oobe /generalize /shutdown
3. Créer une image de capture dans WDS (à partir de boot.wim) :
   console WDS → Images de démarrage → clic droit sur boot.wim →
   "Créer une image de capture" → la déployer sur le WDS.
4. PXE-booter le poste master → choisir l'image de capture →
   capturer vers install.wim sur le serveur.
```

### Pilotes dans l'image de démarrage (indispensable !)

Sans le pilote réseau/NVMe dans `boot.wim`, le WinPE ne voit ni le réseau ni le disque :

```powershell
# Ajouter un groupe de pilotes au serveur
wdsutil /Add-DriverPackage /Server:WDS-01 /DriverFile:"E:\Drivers\NIC\net.inf"

# Injecter dans l'image de démarrage
wdsutil /Add-ImageDriverPackage /Server:WDS-01 /Image:"Microsoft Windows Setup (x64)" `
    /ImageType:Boot /DriverGroup:"WinPE x64"
```

> 💡 **En 2026 :** pensez aussi à **MDT** (Microsoft Deployment Toolkit, gratuit) au-dessus de WDS pour les séquences de tâches, ou **SCCM/MECM** pour les grands parcs. WDS seul = déploiement basique ; MDT/SCCM = déploiement industrialisé.

---

## 50. Dépannage PXE / WDS

### Tableau des erreurs PXE courantes

| Code / message | Cause probable | Solution |
|---|---|---|
| `PXE-E53: No boot filename received` | Pas de réponse du serveur WDS / options DHCP | Vérifier que WDSServer tourne, options 66/67 ou 60, pare-feu (ports 67/68/69/4011) |
| `PXE-M0F: Exiting PXE ROM` | Aucun serveur PXE trouvé | Câble/VLAN, DHCP fonctionnel ?, WDS démarré ? |
| `PXE-E51: No DHCP or proxyDHCP offers` | Pas de DHCP | Serveur DHCP joignable ? Relais DHCP (ip helper) sur le routeur inter-VLAN ? |
| `PXE-E32: TFTP open timeout` | TFTP bloqué | Pare-feu UDP 69, service WDS, fichier de boot présent |
| `0xc000000f` après chargement | BCD de l'image corrompu | Régénérer l'image de démarrage |
| F12 sans effet / boot direct disque | Ordre de boot, ou touche différente (F12 Dell/HP, F8/F12 Lenovo) | Menu de boot au démarrage |
| WinPE sans réseau | Pilote NIC absent de boot.wim | Injecter le pilote (§49) |
| WinPE ne voit pas le disque | Pilote NVMe/RAID absent | Injecter le pilote ; vérifier mode SATA (AHCI vs RAID) |

### Check-list de diagnostic (dans l'ordre)

```powershell
# Côté serveur WDS
Get-Service WDSServer | Select-Object Status, StartType
# → Running + Automatic ?

# Le port TFTP écoute-t-il ?
Get-NetTCPConnection -LocalPort 69 -ErrorAction SilentlyContinue
# (TFTP = UDP : vérifier avec netstat -ano | findstr ":69")

# Journal WDS
Get-WinEvent -LogName 'Microsoft-Windows-Deployment-Services-Diagnostics/Operational' `
    -MaxEvents 30 | Select-Object TimeCreated, Id, Message | Format-Table -Wrap

# Côté client : tester le DHCP
# (depuis un poste du même VLAN)
ipconfig /all   # passerelle, DHCP visibles ?
```

### Le cas du DHCP sur le même serveur (piège n°1)

```
Symptômes : PXE-E53 ou pas de réponse PXE, alors que le DHCP fonctionne.
Cause : le service DHCP et WDS se disputent le port UDP 67.
Solution : dans les propriétés WDS → onglet DHCP :
  ☑ "Ne pas écouter sur les ports DHCP"
  ☑ "Configurer l'option DHCP 60 sur PXEClient"
En CLI :
  wdsutil /Set-Server /UseDHCPPorts:No /DHCPOption60:Yes
```

### Inter-VLAN (voir aussi cas pratique 13, §76)

```
Le broadcast DHCP ne traverse pas les routeurs.
→ Configurer un relais DHCP (ip helper-address) sur l'interface du VLAN
  client, pointant vers le serveur DHCP ET vers le serveur WDS
  (ou utiliser l'option DHCP 60/66/67 + redirecteur PXE).
```

---

## 51. iPXE et boot réseau avancé

**iPXE** est un firmware PXE open source avancé : il ajoute **HTTP**, les scripts, et le boot depuis des URL — là où le PXE classique est limité à TFTP.

### Pourquoi iPXE en entreprise/datacenter

| PXE classique | iPXE |
|---|---|
| TFTP uniquement (lent, UDP) | **HTTP** (rapide, TCP, cacheable) |
| Pas de script | Scripts de boot conditionnels |
| Menu WDS uniquement | Menus dynamiques, boot multi-OS |
| Pas de HTTPS | **HTTPS**, SAN boot (iSCSI) |

### Exemple de script iPXE

```ipxe
#!ipxe
# Menu de boot réseau d'entreprise
set server http://deploy.lan.lan

:start
menu Boot reseau - ${hostname}
item --gap --             -------------------------
item win11               Installer Windows 11 (WDS)
item winpe               WinPE de depannage
item diag                Diagnostics memoire (memtest)
item --gap --             -------------------------
item shell               Shell iPXE
item reboot              Redemarrer
choose option || goto start

:win11
kernel ${server}/wdsboot.cgi/arch=x64 ||
chain ${server}/menu.ipxe || goto start

:winpe
kernel ${server}/winpe/wimboot
initrd ${server}/winpe/boot.wim boot.wim
boot

:diag
kernel ${server}/memtest86.bin
boot

:shell
shell

:reboot
reboot
```

### Chaînage depuis le PXE existant

```
1. Le client PXE classique télécharge undionly.kpxe / ipxe.efi via TFTP.
2. iPXE prend la main → DHCP → télécharge le script via HTTP.
3. Le script propose le menu → charge les images via HTTP.
```

> 💡 **En datacenter :** iPXE + HTTP est le standard pour provisionner des serveurs (avec Foreman, MAAS, ou NetBox + scripts maison). Sur les serveurs récents, l'**HTTP Boot UEFI natif** (sans iPXE) fait la même chose directement depuis le firmware.

---

## 52. Mises à jour du firmware : précautions générales

### Règle d'or

> 🔴 **Ne jamais interrompre un flash du BIOS/UEFI.** Une coupure pendant l'écriture = carte mère potentiellement inutilisable (sauf fonction de récupération, §77). C'est l'opération la plus risquée sur un poste.

### Check-list avant tout flash

- [ ] **Secteur branché** + batterie chargée à > 50 % (portable). Idéalement : onduleur (lien avec votre métier énergie !).
- [ ] **BitLocker suspendu** (`Suspend-BitLocker -RebootCount 1`) si chiffré (§37).
- [ ] **Sauvegarde** des données critiques (principe).
- [ ] **Bonne version** : vérifier le modèle exact (référence constructeur, pas « ça ressemble »).
- [ ] **Lire le changelog** : la MAJ corrige-t-elle quelque chose qui vous concerne ? (Si tout fonctionne et qu'aucune faille critique : on peut planifier plutôt que subir.)
- [ ] **Ne pas flasher pendant un orage** / sur une prise douteuse.
- [ ] Fermer les applications, désactiver temporairement l'antivirus si le constructeur le recommande.
- [ ] Noter la **version actuelle** (pour rollback éventuel).

### Méthodes de flash (général)

| Méthode | Description | Quand l'utiliser |
|---|---|---|
| **Depuis Windows** (utilitaire OEM) | `.exe` du constructeur | Parc standard, flash simple |
| **Depuis le setup UEFI** | Clé USB FAT32 + fichier `.bin`/`.cap` | Le plus sûr (pas d'OS), serveurs |
| **Capsule UEFI** (Windows Update) | MAJ proposée par WU | Parc géré, avec validation pilote |
| **À distance** (outils OEM) | Dell Command Update, HP Image Assistant... | Déploiement de masse |

### Après le flash

```
1. Le PC redémarre (parfois 2-3 fois, avec écran noir prolongé : NORMAL).
2. Entrer dans le setup : vérifier la nouvelle version.
3. ⚠️ Les réglages peuvent être réinitialisés → re-vérifier :
   ordre de boot, Secure Boot, TPM, virtualisation, mot de passe.
4. BitLocker : vérifier qu'il est "FullyEncrypted" (re-scellé).
5. Tester un redémarrage complet + une mise en veille/reprise.
```

---

## 53. Mise à jour BIOS/UEFI : Dell

### Méthodes

| Méthode | Outil | Usage |
|---|---|---|
| Exécutable Windows | `Latitude_5x40_1.2.3.exe` téléchargé du support Dell | Un poste, manuel |
| **Dell Command Update** | `dcu-cli.exe` | Parc : scan + déploiement planifié |
| Depuis le BIOS | F12 → BIOS Flash Update (fichier sur USB) | Sans OS, le plus sûr |
| Via SCCM/Intune | Dell Command Configure + packages | Masse |

### Dell Command Update en CLI (déploiement)

```powershell
# Installer silencieusement (téléchargé depuis dell.com/support)
Start-Process .\Dell-Command-Update-Application.msi -ArgumentList '/qn' -Wait

# Scanner les mises à jour disponibles
& 'C:\Program Files\Dell\CommandUpdate\dcu-cli.exe' /scan

# Appliquer BIOS + firmware (silencieux, avec reboot auto)
& 'C:\Program Files\Dell\CommandUpdate\dcu-cli.exe' /applyUpdates -silent -reboot=enable

# Rapport
& 'C:\Program Files\Dell\CommandUpdate\dcu-cli.exe' /report
```

### BIOS Flash Update (F12, sans OS)

```
1. Télécharger le .exe du BIOS → l'exécuter UNE fois sur un PC Windows :
   il propose "créer un support de flash" OU extraire le fichier .rcv/.hdr.
   (Astuce : beaucoup de .exe Dell s'extraient avec /s /e=C:\temp\bios)
2. Copier le fichier sur une clé USB FAT32.
3. Sur la cible : F12 → BIOS Flash Update → sélectionner le fichier.
4. Ne pas interrompre. Le PC redémarre seul.
```

### BIOSConnect / SupportAssist (récupération)

Les Dell récents ont une **récupération BIOS automatique** : si le flash échoue, le système restaure depuis une copie de secours (voir §77).

---

## 54. Mise à jour BIOS/UEFI : HP

### Méthodes

| Méthode | Outil | Usage |
|---|---|---|
| Exécutable Windows | `spXXXXX.exe` (SoftPaq) | Un poste |
| **HP Image Assistant** / HP Manageability | Analyse du parc | Masse |
| Depuis le BIOS | F10 → Update BIOS (HP Sure Start) | Sans OS |
| Via USB recovery | Touche `Win + B` au démarrage | **Récupération** après flash raté |

### Exemple SoftPaq en silencieux

```powershell
# Télécharger le SoftPaq BIOS depuis support.hp.com (ex. sp123456.exe)
# Installation silencieuse avec flash :
Start-Process .\sp123456.exe -ArgumentList '/s /f' -Wait
# /s = silencieux, /f = forcer le flash (selon SoftPaq, vérifier la doc)
```

### HP Sure Start (protection)

Les HP Pro/Elite récents intègrent **Sure Start** : le BIOS est vérifié cryptographiquement à chaque démarrage, avec **copie dorée de restauration automatique**. En cas de corruption → restauration sans intervention. C'est une excellente protection contre les flashs ratés et les attaques firmware.

### Récupération d'urgence HP

```
Écran noir après flash :
1. Éteindre (10 s sur le bouton).
2. Maintenir Win + B (ou Win + V sur certains modèles).
3. Brancher le secteur TOUT en maintenant les touches.
4. Allumer : l'utilitaire de récupération BIOS HP démarre
   (chercher un BIOS sur USB ou en ligne).
```

---

## 55. Mise à jour BIOS/UEFI : Lenovo

### Méthodes

| Méthode | Outil | Usage |
|---|---|---|
| Exécutable Windows | `bios_update_utility.exe` | Un poste |
| **Lenovo System Update** | `tvsu.exe` | Parc ThinkPad/ThinkCentre |
| Depuis le BIOS | F12 / Enter → BIOS Update via USB | Sans OS |
| Via SCCM | Lenovo Patch / ThinkVantage | Masse |

### Lenovo System Update en CLI

```powershell
# Scanner
& 'C:\Program Files (x86)\Lenovo\System Update\tvsu.exe' /CM -search A -action INSTALL `
    -repository C:\temp\lsu -noreboot

# Installer les critiques (dont BIOS) sans interaction
& 'C:\Program Files (x86)\Lenovo\System Update\tvsu.exe' /CM -search C -action INSTALL -noreboot
# Codes : C = critiques, R = recommandées, A = toutes
```

### ThinkShield / self-healing

Les ThinkPad récents (ThinkShield) incluent une vérification d'intégrité du firmware avec restauration. Comme chez HP/Dell, la tendance est au **firmware auto-réparant** : vérifiez que cette option est activée dans le setup (Security → Secure Rollback Prevention, etc.).

### Particularité Lenovo : le mot de passe supervisor

Sans le mot de passe supervisor, **impossible de flasher** le BIOS sur beaucoup de ThinkPad. Prévoyez-le dans la procédure de masse (déverrouillage via WMI avec le mot de passe, §43).

---

## 56. Mise à jour du firmware TPM

Le TPM lui-même a un firmware, avec des failles publiées (ex. Infineon RSA 2017, CVE-2017-15361). Le mettre à jour, c'est patcher le coffre-fort.

### Quand mettre à jour

- [ ] Bulletin de sécurité du fabricant du TPM (Infineon, STMicro) ou du constructeur.
- [ ] Outil de détection : `Get-Tpm | Select-Object ManufacturerVersion` → comparer avec la version corrigée publiée.

### Procédure type (exemple Dell/Infineon)

```powershell
# 1. Séquestre BitLocker VÉRIFIÉ + suspension (la MAJ TPM = Clear implicite !)
Suspend-BitLocker -MountPoint "C:" -RebootCount 2
manage-bde -protectors -get C: -type RecoveryPassword

# 2. Lancer l'utilitaire du constructeur (ex. Dell TPM 2.0 Firmware Update)
Start-Process .\TPM_Firmware_Update.exe -ArgumentList '/s' -Wait

# 3. Le PC redémarre, met à jour le TPM (ne pas interrompre !),
#    puis Windows re-provisionne automatiquement (AutoProvisioning).

# 4. Vérifications
Get-Tpm | Select-Object TpmPresent, TpmReady, ManufacturerVersion
Get-BitLockerVolume -MountPoint "C:" | Select-Object VolumeStatus
# → FullyEncrypted, pas "suspendu"
```

> 🔴 **Une MAJ du firmware TPM efface le TPM** (comme un Clear). Sans la clé de récupération BitLocker sous la main, c'est la perte des données. C'est l'opération la plus piégeuse du chapitre : elle ressemble à une simple MAJ, elle agit comme un Clear.

---

## 57. Attaques : les bootkits

Un **bootkit** est un malware qui s'installe **avant l'OS**, dans la chaîne de démarrage : MBR, chargeur, firmware UEFI, ou ROM d'extension. Il est invisible pour l'antivirus classique (qui démarre après lui).

### Vecteurs d'infection

| Vecteur | Cible | Contre-mesure |
|---|---|---|
| MBR infecté (ex. familles historiques) | Secteur 0 du disque | UEFI + Secure Boot (le MBR n'est plus exécuté) |
| Chargeur EFI remplacé | `\EFI\...\bootmgfw.efi` | **Secure Boot** (signature vérifiée) |
| Pilote malveillant au boot | Drivers de démarrage | Secure Boot + ELAM + HVCI |
| **Firmware UEFI infecté** | Image du BIOS elle-même | Secure Boot + **Boot Guard** (Intel) / PSB (AMD) + MAJ firmware |
| ROM d'extension (carte réseau, GPU) | Option ROM | Secure Boot vérifie aussi les ROMs (si activé) |

### Exemples notables (culture sécurité)

- **BlackLotus (2023)** : contournement de Secure Boot via une faille dans un chargeur signé (CVE-2023-24932) → Microsoft a dû révoquer via dbx + patch du boot. Démonstration que Secure Boot n'est pas invincible : il faut **tenir la dbx à jour** (§21).
- **CosmicStrand / MoonBounce** : implants dans le firmware UEFI (persistants même après formatage du disque !).
- **Bootkits MBR historiques** (TDL4, Olmasco) : rendus obsolètes par l'UEFI.

### Détection

```
- Secure Boot activé + dbx à jour = la base (bloque 95 % des cas).
- Comparer les hashes des fichiers EFI avec une machine de référence saine.
- Outils EDR avec protection firmware (certains EDR d'entreprise scannent
  l'image UEFI).
- Flash d'un firmware officiel connu-sain en cas de doute sérieux
  (un reflash écrase un implant firmware dans la plupart des cas).
```

> 💡 **Message clé pour la direction :** un bootkit firmware survit au formatage et à la réinstallation. La prévention (Secure Boot, MAJ firmware, contrôle physique) coûte 100× moins cher que la remédiation.

---

## 58. Attaques : evil maid et accès physique

L'**evil maid** (« la femme de chambre malveillante ») : un attaquant avec un **accès physique temporaire** au poste (hôtel, bureau partagé, maintenance) compromet la machine en quelques minutes.

### Scénarios

| Scénario | Déroulé | Contre-mesure |
|---|---|---|
| Clé USB bootable | Boot sur Linux live → copie/altération du disque | Mot de passe UEFI + USB boot désactivé + **BitLocker** |
| Modification du chargeur | Remplacement de `bootmgfw.efi` par une version piégée | **Secure Boot** (signature refusée) |
| Reset du mot de passe Windows | Utilitaire offline sur la base SAM | BitLocker (disque illisible hors OS) |
| Keylogger matériel | Dongle USB entre clavier et PC | Inspection physique, ports verrouillés |
| Attaque DMA (Thunderbolt/PCIe) | Lecture mémoire via DMA avant verrouillage | VT-d + **Kernel DMA Protection**, désactiver le pre-boot DMA |
| Cold boot | Refroidir la RAM pour en extraire les clés | BitLocker + extinction complète (pas de veille) sur postes sensibles |

### La défense en profondeur du poste

```
1. Chiffrement : BitLocker TPM+PIN (le disque seul ne vaut rien).
2. Firmware : mot de passe admin, USB boot désactivé, Secure Boot.
3. Physique : câble antivol, ports condamnés si besoin, BIOS verrouillé.
4. Comportement : extinction (pas veille) pour les postes sensibles ;
   méfiance sur les postes laissés sans surveillance en déplacement.
5. Détection : BitLocker en récupération "sans raison" = signal d'alerte
   (quelqu'un a touché au firmware ou au boot !).
```

> 🔴 **Point d'attention manager :** une demande de clé de récupération BitLocker « spontanée » sur un portable de direction n'est pas qu'un incident support : c'est potentiellement un **indicateur de compromission physique**. Procédure : vérifier la cause (MAJ ? changement matériel ?) avant de simplement débloquer.

---

## 59. Durcissement d'un poste : la check-list sécurité

### Niveau 1 — Base (tout le parc)

- [ ] UEFI natif, CSM désactivé
- [ ] Secure Boot activé (vérifié : `Confirm-SecureBootUEFI` = True)
- [ ] TPM 2.0 activé et provisionné (`Get-Tpm` → TpmReady)
- [ ] Disque système GPT
- [ ] BitLocker activé, clé séquestrée (AD/Entra ID)
- [ ] Mot de passe admin UEFI défini (coffre d'équipe)
- [ ] USB boot désactivé (sauf exceptions tracées)
- [ ] Ordre de boot : disque en premier
- [ ] Firmware à jour (moins de 12 mois)

### Niveau 2 — Renforcé (portables, nomades)

- [ ] Tout le niveau 1, plus :
- [ ] BitLocker **TPM + PIN** (6 chiffres minimum)
- [ ] Mot de passe système au boot (si compatible avec le WoL/patching)
- [ ] Kernel DMA Protection activé (vérifier : `msinfo32` → « Protection DMA du noyau »)
- [ ] HVCI / Memory Integrity activé (Sécurité Windows → Isolation du noyau)
- [ ] Extinction exigée (désactiver la veille simple via GPO sur profils sensibles)

### Niveau 3 — Sensible (direction, finance, admin)

- [ ] Tout le niveau 2, plus :
- [ ] TPM discret si disponible
- [ ] Ports USB avant désactivés dans le firmware
- [ ] Windows Hello for Business (clés dans le TPM)
- [ ] Credential Guard activé
- [ ] Inventaire firmware mensuel (script §40)

### Vérification automatisée (conformité)

```powershell
#requires -RunAsAdministrator
function Test-PostureSecurite {
    $tpm = Get-Tpm
    $bde = Get-BitLockerVolume -MountPoint $env:SystemDrive
    $sb  = $false; try { $sb = Confirm-SecureBootUEFI } catch {}
    $dma = (Get-CimInstance -ClassName Win32_DeviceGuard |
        Select-Object -ExpandProperty KernelDmaProtectionStatus -ErrorAction SilentlyContinue)
    [PSCustomObject]@{
        SecureBoot      = $sb
        TPM_Pret        = $tpm.TpmReady
        BitLocker       = $bde.VolumeStatus
        Protecteur_TPM  = ($bde.KeyProtector.KeyProtectorType -contains 'Tpm')
        DMA_Kernel      = $dma
        Score           = (($sb) + ($tpm.TpmReady) + ($bde.VolumeStatus -eq 'FullyEncrypted'))
    }
}
Test-PostureSecurite | Format-List
```

---

## 60. Bonnes pratiques de gestion de parc

### Standardisation

- [ ] **2–3 modèles** de postes maximum par génération (réduit les images, les pilotes, les procédures firmware).
- [ ] **Image master** unique par modèle (avec firmware de référence inclus dans la doc).
- [ ] **Profils BIOS standardisés** : exporter la config d'un poste modèle (Dell `cctk --backup`, HP `BiosConfigUtility /get`, Lenovo `wmi`) et la déployer.

### Cycle de vie du firmware

```
Trimestriel : vérifier les bulletins de sécurité firmware des constructeurs.
Semestriel  : vague de MAJ firmware (lot pilote → 25 % → 100 %),
              toujours avec BitLocker suspendu + séquestre vérifié.
Annuel      : revue des réglages (nouveautés : Kernel DMA, Pluton, etc.).
```

### Documentation

- [ ] Matrice **modèle → version BIOS validée → réglages** (wiki interne).
- [ ] Procédure **mot de passe BIOS oublié** par constructeur (§69), avec contacts support et preuves d'achat.
- [ ] Registre des **exceptions** (postes en Legacy, Secure Boot désactivé...) : qui, pourquoi, jusqu'à quand. Une exception non documentée = une faille.

### Supervision

```powershell
# Exemple : remonter les postes avec Secure Boot désactivé (via le CSV d'inventaire §40)
Import-Csv .\inventaire-firmware.csv |
    Where-Object { $_.SecureBoot -ne 'True' } |
    Select-Object ComputerName, Model, BiosVersion
# → à traiter ou à justifier (exception documentée).
```

---

## 61. UEFI en datacenter : les serveurs

### Particularités des serveurs

| Aspect | Serveur vs poste |
|---|---|
| Firmware | UEFI avec **Redfish/BMC** (administration à distance) |
| Boot | Souvent **réseau** (iPXE/HTTP Boot) ou SAN |
| Secure Boot | Supporté (à activer, avec les clés de l'OS serveur) |
| TPM | TPM 2.0 sur carte mère serveur (pour BitLocker, attestation) |
| Mises à jour | Via le BMC (montage d'ISO virtuel) ou outils OEM (iDRAC, iLO, XCC) |

### Ordre de boot type d'un hyperviseur

```
1. Disque local / RAID (hyperviseur)
2. Réseau (iPXE) — pour re-provisionnement
3. (Désactiver l'USB boot)
```

### Bonnes pratiques

- [ ] **Mot de passe du BIOS serveur** défini (distinct du BMC !).
- [ ] **Secure Boot activé** (vérifier la compatibilité de l'hyperviseur : ESXi, Hyper-V, Proxmox le supportent).
- [ ] **Boot réseau sécurisé** : iPXE avec HTTPS, pas de TFTP en clair sur un réseau non fiable.
- [ ] **Firmware à jour** : BIOS + BMC + cartes (planifié, jamais « à l'arrache »).
- [ ] **TPM activé** pour le chiffrement des VM / vTPM.

---

## 62. Redfish et BMC : administrer le firmware à distance

Le **BMC** (*Baseboard Management Controller* : iDRAC Dell, iLO HP, XCC Lenovo, IMM...) permet d'administrer le serveur **hors OS**, via **Redfish** (API REST standard).

### Cas d'usage firmware

- Monter une ISO à distance (flash du BIOS sans se déplacer).
- Lire/modifier les réglages UEFI (ordre de boot, Secure Boot).
- Forcer un boot PXE au prochain redémarrage.
- Inventorier les versions de firmware du parc serveurs.

### Exemple Redfish (PowerShell)

```powershell
# 1. Lire les réglages BIOS actuels
$bmc = 'https://idrac-01.lan.lan'
$cred = Get-Credential  # compte BMC
Invoke-RestMethod -Uri "$bmc/redfish/v1/Systems/System.Embedded.1/Bios" `
    -Credential $cred -SkipCertificateCheck |
    Select-Object -ExpandProperty Attributes |
    Select-Object BootMode, SecureBoot, TpmSecurity

# 2. Forcer un boot PXE au prochain redémarrage
$body = @{ Boot = @{ BootSourceOverrideTarget = 'Pxe';
                     BootSourceOverrideEnabled = 'Once' } } |
        ConvertTo-Json
Invoke-RestMethod -Uri "$bmc/redfish/v1/Systems/System.Embedded.1" `
    -Method Patch -Body $body -ContentType 'application/json' `
    -Credential $cred -SkipCertificateCheck

# 3. Redémarrer
$reset = @{ ResetType = 'ForceRestart' } | ConvertTo-Json
Invoke-RestMethod -Uri "$bmc/redfish/v1/Systems/System.Embedded.1/Actions/ComputerSystem.Reset" `
    -Method Post -Body $reset -ContentType 'application/json' `
    -Credential $cred -SkipCertificateCheck
```

> 🔒 **Sécurité BMC :** changer les mots de passe par défaut (root/calvin...), dédier un VLAN d'administration, HTTPS uniquement, désactiver les vieux protocoles (Telnet !). Un BMC compromis = contrôle total du serveur, **en dessous de l'OS**.

---

## 63. Secure Boot sur Hyper-V : les VM de génération 2

Hyper-V propose deux générations de VM :

| | Génération 1 | Génération 2 |
|---|---|---|
| Firmware | BIOS émulé | **UEFI émulé** |
| Disque système | MBR (IDE) | **GPT** (SCSI) |
| **Secure Boot** | ❌ | ✅ (modèles de stratégie) |
| PXE | Legacy | UEFI |
| Usage | Vieux OS | **Tout OS moderne** |

### Activer Secure Boot sur une VM gen2

```powershell
# À la création
New-VM -Name 'SRV-WEB-01' -Generation 2 -MemoryStartupBytes 4GB `
    -NewVHDPath 'D:\VMs\SRV-WEB-01.vhdx' -NewVHDSizeBytes 80GB

# Sur une VM existante (éteinte)
Set-VMFirmware -VMName 'SRV-WEB-01' -EnableSecureBoot On

# Choisir le modèle (template) de clés
Set-VMFirmware -VMName 'SRV-WEB-01' -SecureBootTemplate 'MicrosoftWindows'
# Modèles disponibles :
#   MicrosoftWindows      → Windows uniquement
#   MicrosoftUEFICertificateAuthority → Windows + Linux (shim)
#   OpenSourceShieldedVM  → distributions open source
```

### vTPM (TPM virtuel)

```powershell
# Ajouter un vTPM à une VM gen2 (prérequis : hôte avec TPM ou HGS pour les shielded VM)
# 1. Chiffrer la VM avec un protecteur par certificat (key protector)
$cert = New-SelfSignedCertificate -Subject "ShieldedVM"
$kp = New-HgsKeyProtector -Owner $cert -AllowUntrustedRoot
Set-VMKeyProtector -VMName 'SRV-WEB-01' -KeyProtector $kp.RawData

# 2. Activer le vTPM
Enable-VMTPM -VMName 'SRV-WEB-01'

# 3. Vérifier
Get-VMSecurity -VMName 'SRV-WEB-01' |
    Select-Object VMName, TpmEnabled, Shielded
```

> 💡 **Cas d'usage :** BitLocker **dans** la VM (vTPM), Windows 11 en VM (exige vTPM + Secure Boot), Shielded VM en datacenter.

---

## 64. Cas pratique 1 : TPM non détecté par Windows

### Contexte

Un poste neuf (ou après une MAJ BIOS) : `Get-Tpm` retourne `TpmPresent = False`, `tpm.msc` affiche « TPM introuvable ». BitLocker refuse de s'activer.

### Diagnostic pas à pas

```powershell
# Étape 1 : confirmer l'absence
Get-Tpm | Select-Object TpmPresent, TpmReady
# → False / False

# Étape 2 : le firmware voit-il la puce ? (pas de cmdlet direct :
# c'est le setup UEFI qui tranche — mais on peut déjà vérifier le pilote)
Get-PnpDevice -Class 'SecurityDevices' -ErrorAction SilentlyContinue |
    Select-Object FriendlyName, Status
# → vide = Windows ne voit aucun périphérique TPM du tout
```

### Résolution

```
Étape 3 : redémarrer → setup UEFI → Security :
  - TPM / Security Chip : est-il sur "Off" ou "Hidden" ?
    → passer sur On / Enabled.
  - Version : choisir TPM 2.0 (pas 1.2).
  - Sur AMD : "AMD fTPM switch" → AMD CPU fTPM.
    Sur Intel : "PTT" → Enabled.
  - Sauvegarder (F10), redémarrer.

Étape 4 : dans Windows
  Get-Tpm → TpmPresent = True ?
  Si TpmReady = False → Initialize-Tpm (ou tpm.msc → "Préparer le TPM"),
  puis redémarrer.

Étape 5 : si toujours absent
  - Re-flasher le BIOS (un flash corrompu peut masquer le TPM).
  - Clear CMOS (§71) en dernier recours.
  - Vérifier la compatibilité : certains CPU très anciens n'ont ni
    fTPM ni PTT et la carte mère n'a pas de header TPM → pas de TPM
    possible → poste à remplacer (non conforme Windows 11).
```

### Points de vigilance

- Sur **HP**, l'option s'appelle parfois « TPM Embedded Security » avec *deux* réglages : `TPM Device` (Available/Hidden) **et** `TPM State` (On/Off). Il faut les deux.
- Sur **Lenovo**, après un Clear CMOS, le Security Chip peut repasser en « Inactive » : le réactiver + `Initialize-Tpm`.

---

## 65. Cas pratique 2 : Secure Boot bloque le démarrage après un changement matériel

### Contexte

Après l'ajout d'une carte réseau 10 Gb/s (ou d'un GPU) sur une station, le PC affiche au démarrage : `Secure Boot Violation — Invalid signature detected`, puis retourne au setup.

### Analyse

```
Le firmware, avec Secure Boot activé, vérifie aussi la signature des
ROMs d'extension (Option ROM) des cartes PCIe.
→ La ROM de cette carte n'est pas signée par une clé de db
  (carte exotique, vieux firmware de carte, ou contrefaçon).
→ Secure Boot refuse d'exécuter la ROM → bloque le boot.
```

### Résolution (par ordre de préférence)

```
Option A (recommandée) : mettre à jour le firmware de LA CARTE
  → les firmwares récents des cartes pro sont signés (clé "Microsoft
    Option ROM UEFI CA" dans db). Télécharger le firmware chez le
    fabricant de la carte, flasher, retester.

Option B : désactiver le boot sur cette carte
  → setup UEFI → désactiver l'Option ROM de ce slot PCIe
    (si la carte ne sert pas au boot : ex. GPU de calcul, carte
    d'acquisition). Le système démarrera, la carte sera vue par l'OS.

Option C (temporaire, tracée) : désactiver Secure Boot
  → uniquement pour valider que c'est bien la cause, puis réactiver
    après application de l'option A ou B.

Option D (avancée) : mode Custom + enrôlement de la clé du fabricant
  → si le fabricant fournit son certificat, l'importer en db.
```

### Vérification

```powershell
# Après résolution
Confirm-SecureBootUEFI   # → True (on n'a PAS laissé Secure Boot désactivé)
# Tester 2-3 redémarrages à froid + un arrêt prolongé.
```

> 💡 **Leçon :** intégrer la vérification « Secure Boot + nouvelle carte » dans la procédure d'ajout matériel. Un pilote Windows signé ne suffit pas : c'est la **ROM de la carte** qui est vérifiée au boot.

---

## 66. Cas pratique 3 : boucle de démarrage (boot loop) après une mise à jour

### Contexte

Après une mise à jour Windows (ou un flash BIOS), le PC redémarre en boucle : logo constructeur → écran bleu fugace → reboot. Impossible d'atteindre Windows.

### Diagnostic

```
1. Interrompre la boucle : forcer 3 extinctions pendant le démarrage
   (bouton 10 s) → au 3e, Windows lance WinRE automatiquement.
   (Alternative : clé USB d'installation → "Réparer l'ordinateur".)
2. En WinRE : Dépannage → Options avancées → voir les options :
   - "Désinstaller les mises à jour" (si MAJ Windows en cause).
   - "Restauration du système" (si point de restauration).
   - "Invite de commandes" (diagnostic manuel).
```

### Résolution en invite WinRE

```powershell
# Identifier les lettres (en WinRE, C: n'est pas toujours Windows !)
diskpart
#   list volume   → noter la lettre du volume Windows (ex. D:) et de l'ESP
#   exit

# Cas A : BCD endommagé par la MAJ
bcdboot D:\Windows /s S: /f UEFI
#   → "Boot files successfully created."

# Cas B : pilote fautif (écran bleu au boot)
#   Démarrer en mode sans échec :
bcdedit /store S:\EFI\Microsoft\Boot\BCD /set '{default}' safeboot Minimal
#   → reboot, désinstaller le pilote/MAJ, puis :
bcdedit /store S:\EFI\Microsoft\Boot\BCD /deletevalue '{default}' safeboot

# Cas C : MAJ Windows fautive → la désinstaller
dism /image:D:\ /get-packages | findstr "Package_for"
dism /image:D:\ /remove-package /packagename:Package_for_RollupFix~...

# Cas D : restauration système
rstrui.exe
```

### Si c'est le flash BIOS qui a causé la boucle

```
→ Le firmware a peut-être réinitialisé des réglages (SATA RAID→AHCI,
  Secure Boot, ordre de boot). Vérifier dans le setup :
  - Mode SATA identique à l'installation d'origine (sinon BSOD INACCESSIBLE_BOOT_DEVICE).
  - Ordre de boot correct.
→ BitLocker peut demander la clé (PCR 0 changé) : l'avoir sous la main (§37).
```

---

## 67. Cas pratique 4 : BCD corrompu — écran bleu 0xc000000f

### Contexte

Message au démarrage :

```
Recovery — Your PC needs to be repaired
The Boot Configuration Data for your PC is missing or contains errors.
File: \EFI\Microsoft\Boot\BCD
Error code: 0xc000000f
```

Causes : coupure pendant une MAJ, clonage raté, ESP effacée, disque remplacé.

### Résolution complète (WinRE)

```powershell
# 1. Démarrer sur clé USB Windows → Réparer → Invite de commandes.

# 2. Identifier les volumes
diskpart
#   list disk        → repérer le disque système
#   select disk 0
#   list partition   → repérer l'ESP (FAT32, ~100 Mo) et Windows (NTFS)
#   select partition 1   (l'ESP)
#   assign letter=S
#   list volume      → noter la lettre de Windows (ex. C:)
#   exit

# 3. Reconstruire le boot
bcdboot C:\Windows /s S: /f UEFI /l fr-FR

# 4. Vérifier
bcdedit /store S:\EFI\Microsoft\Boot\BCD /enum
#   → doit afficher Windows Boot Manager + une entrée Windows.

# 5. Nettoyer et redémarrer
#   diskpart → select partition 1 → remove letter=S → exit
#   → "Continuer" (redémarrer sur le disque).
```

### Si bcdboot échoue (« Failure when attempting to copy boot files »)

```
Cause n°1 : mauvaise lettre Windows (en WinRE, vérifier avec dir C:\Windows).
Cause n°2 : ESP non en FAT32 ou sans lettre → reformater PROPREMENT :
  diskpart → select partition 1 → format fs=fat32 quick → assign letter=S
  (⚠️ ne formater QUE si on est sûr que c'est l'ESP !)
Cause n°3 : disque en lecture seule → attributes disk clear readonly.
```

---

## 68. Cas pratique 5 : PXE-E53 / PXE-M0F — le client ne trouve pas le serveur de boot

### Contexte

Un poste en boot PXE (F12 → Network) affiche `PXE-E53: No boot filename received` puis `PXE-M0F: Exiting Intel PXE ROM`. Le déploiement est bloqué.

### Arbre de diagnostic (dans l'ordre, du plus probable)

```
① Le client a-t-il une IP ?
   → Non (ou 169.254.x.x) : problème DHCP de base.
     Vérifier : câble, port switch (VLAN correct ?), serveur DHCP joignable,
     relais DHCP (ip helper) si autre sous-réseau.
② Le client a une IP mais pas de "boot filename" ?
   → Le serveur WDS ne répond pas :
     a) Service WDSServer démarré ? (Get-Service WDSServer)
     b) Pare-feu serveur : UDP 67/68 (DHCP), UDP 69 (TFTP), UDP 4011 (PXE) ?
        → Test rapide : désactiver temporairement le pare-feu pour valider.
     c) DHCP et WDS sur le même serveur ? → options "Ne pas écouter le
        port 67" + "DHCP option 60" cochées ? (§50)
     d) Stratégie de réponse WDS : "Répondre aux clients connus" alors que
        la machine est inconnue ? → passer en "Répondre à tous" ou
        pré-créer le compte ordinateur (wdsutil /Add-Device).
③ TFTP timeout (PXE-E32) après l'IP ?
   → Le fichier de boot est-il présent ?
     E:\RemoteInstall\Boot\x64\wdsmgfw.efi (UEFI)
     → Régénérer : redémarrer le service WDS, vérifier les droits NTFS.
④ Le client est en UEFI mais le DHCP pousse wdsnbp.com (Legacy) ?
   → Corriger l'option 67 : boot\x64\wdsmgfw.efi pour l'UEFI.
```

### Commandes serveur

```powershell
# État du service et des ports
Get-Service WDSServer
netstat -ano | Select-String ':69|:4011'

# Journaux WDS (les 20 dernières erreurs)
Get-WinEvent -LogName 'Microsoft-Windows-Deployment-Services-Diagnostics/Operational' `
    -MaxEvents 20 | Where-Object LevelDisplayName -eq 'Erreur' |
    Select-Object TimeCreated, Id, Message | Format-List

# Tester le TFTP depuis un poste (client TFTP Windows à activer)
tftp <ip-wds> GET boot\x64\wdsmgfw.efi C:\temp\test.efi
# → si ça échoue : pare-feu ou service.
```

---

## 69. Cas pratique 6 : mot de passe BIOS oublié

### Contexte

Personne ne connaît le mot de passe admin du firmware. Impossible de changer l'ordre de boot ou d'activer le TPM. (Situation fréquente sur du matériel récupéré ou après le départ d'un technicien.)

### Ce qui NE marche PAS (ou plus)

| Méthode | Statut |
|---|---|
| « Master password » universel trouvé sur internet | ❌ Arnaque ou obsolète ; les algos par constructeur changent |
| Enlever la pile CMOS | ❌ Inefficace sur 99 % des machines modernes (mot de passe en NVRAM/flash, pas en CMOS) |
| Court-circuiter la puce au hasard | ❌ Dangereux, peut détruire la carte mère |

### Procédures par constructeur (voies officielles)

```
DELL :
  - Code à 8 caractères affiché après 3 échecs → le support Dell peut
    fournir un code de déverrouillage (preuve d'achat requise).
  - Outil : Dell Command Configure avec l'ancien mot de passe
    (si connu d'un coffre) : cctk --setuppwd= --valsetuppwd=<ancien>.

HP :
  - Après 3 échecs : code "System Disabled" → utilitaire HP BIOS
    Configuration Utility ou support HP (preuve d'achat).
  - Sur ProBook/EliteBook : la procédure SMC.bin (fichier de déblocage
    généré par HP) via clé USB.

LENOVO (ThinkPad) :
  - Mot de passe supervisor : AUCUNE procédure officielle de contournement.
    Carte mère à remplacer (c'est volontaire : sécurité).
  - Mot de passe "power-on" (au boot) : parfois réinitialisable par
    déconnexion batterie + CMOS (selon modèle), mais pas le supervisor.

GÉNÉRIQUE (tour/CM) :
  - Jumper "Clear CMOS" / "Password Clear" sur la carte mère
    (voir manuel ; souvent noté CLR_CMOS, JBAT1, ou PSWD).
  - Retrait pile + débrancher 15 min (uniquement vieilles cartes).
```

### Prévention (leçon managériale)

```
→ TOUS les mots de passe firmware dans le coffre d'équipe, dès la
  réception du matériel.
→ Procédure écrite de transmission lors des départs.
→ Ne jamais laisser un prestataire poser un mot de passe sans le consigner.
```

---

## 70. Cas pratique 7 : BitLocker demande la clé de récupération à chaque démarrage

### Contexte

Après un changement (ou sans cause apparente), Windows demande la clé de récupération à **chaque** démarrage. Le scellé TPM ne fonctionne plus.

### Causes possibles

| Cause | Indice |
|---|---|
| Firmware mis à jour sans re-scellé | PCR 0 changé, `Suspend/Resume` non fait |
| Périphérique USB branché au boot | Certains BIOS mesurent les périphériques (PCR) |
| Option firmware modifiée | Secure Boot on/off, CSM, ordre de boot → PCR 1/7 |
| TPM en erreur / re-provisionné | `Get-Tpm` → vérifier l'état |
| Protecteur TPM supprimé | `manage-bde -protectors -get C:` → plus de protecteur TPM |

### Résolution

```powershell
# 1. Débloquer avec la clé de récupération (une fois).
# 2. Une fois dans Windows, diagnostiquer :
Get-Tpm | Select-Object TpmPresent, TpmReady
manage-bde -protectors -get C:
manage-bde -status C:

# 3. Si le protecteur TPM a disparu → le recréer :
#    (nécessite la clé de récupération sous la main)
manage-bde -protectors -add C: -tpm

# 4. Forcer le re-scellé sur les PCR actuels :
Suspend-BitLocker -MountPoint "C:" -RebootCount 0
Resume-BitLocker -MountPoint "C:"

# 5. Redémarrer 2 fois et vérifier : plus de demande ?
# 6. Si le coupable est un périphérique USB : le débrancher au boot
#    ou figer la config (ne plus le brancher).
```

### Durcissement du profil PCR (optionnel, avancé)

```powershell
# Restreindre les PCR surveillés (ex. retirer le PCR 2 sensible aux cartes)
# Via GPO : "Configurer le profil de validation de plateforme TPM"
# ou en CLI (à manier avec précaution) :
manage-bde -protectors -add C: -tpm -pcr 0,4,11
# ⚠️ Réduire les PCR = réduire la sécurité. Documenter le choix.
```

---

## 71. Cas pratique 8 : Clear CMOS — quand et comment

### Contexte

Le **Clear CMOS** réinitialise les réglages du firmware aux valeurs d'usine (pas le mot de passe supervisor sur les machines modernes, pas le firmware lui-même).

### Quand l'utiliser

| Situation | Clear CMOS utile ? |
|---|---|
| Réglages incohérents après des bidouilles (overclocking, ordre de boot exotique) | ✅ Oui |
| PC qui ne démarre plus après un mauvais réglage (fréquence RAM...) | ✅ Oui |
| Mot de passe **utilisateur** (power-on) oublié sur vieille machine | ⚠️ Parfois |
| Mot de passe **supervisor/admin** oublié (machine moderne) | ❌ Non (voir §69) |
| Firmware corrompu (flash raté) | ❌ Non (voir §77) |
| TPM non détecté après MAJ | ⚠️ En dernier recours |

### Méthodes (par ordre de préférence)

```
Méthode 1 — Jumper (tour / carte mère) :
  1. Éteindre, DÉBRANCHER le cordon d'alimentation.
  2. Localiser le jumper CLR_CMOS / JBAT1 / CLRTC (voir manuel carte mère).
  3. Déplacer le cavalier sur la position Clear 10 secondes,
     puis le remettre en position normale.
  4. Rebrancher, démarrer, reconfigurer le setup.

Méthode 2 — Pile (si pas de jumper accessible) :
  1. Éteindre, débrancher, retirer la pile CR2032 15 minutes
     (appuyer sur le bouton power 10 s pour vider les condensateurs).
  2. Remettre la pile, rebrancher, démarrer.

Méthode 3 — Bouton (portables / cartes récentes) :
  Certains portables ont un trou "reset" (trombone 10 s) ou
  une combinaison (bouton power 30 s sans batterie ni secteur).

Méthode 4 — Depuis le setup :
  "Load Optimized Defaults" / "Restore Defaults" = Clear CMOS logiciel
  (ne touche pas aux mots de passe).
```

### Après un Clear CMOS : reconfiguration obligatoire

```powershell
# Le Clear CMOS remet TOUT par défaut. Repasser en revue :
#  [ ] Mode UEFI (pas Legacy)
#  [ ] Secure Boot → Enabled
#  [ ] TPM → On
#  [ ] Ordre de boot → disque en premier
#  [ ] Virtualisation → Enabled (si utilisée)
#  [ ] Mot de passe admin → REPOSER (il a pu être effacé !)
#  [ ] Date/heure → vérifier (la pile retirée = horloge perdue)
#
# Puis :
Confirm-SecureBootUEFI
Get-Tpm | Select-Object TpmPresent, TpmReady
# Si BitLocker demande la clé : normal (PCR changés) → la saisir,
# puis Suspend/Resume pour re-sceller (§37).
```

---

## 72. Cas pratique 9 : Windows 11 refuse de s'installer — « ce PC ne peut pas exécuter Windows 11 »

### Contexte

L'assistant d'installation ou `setup.exe` bloque : « Ce PC ne répond pas à la configuration système minimale requise ». Pourtant le PC semble correct.

### Diagnostic précis (ne pas deviner !)

```powershell
# 1. Les 4 piliers
Confirm-SecureBootUEFI                        # doit être True
(Get-Tpm).TpmReady                            # doit être True
(Get-Disk | Where-Object IsSystem).PartitionStyle  # doit être GPT

# 2. Détail du blocage (registre d'évaluation)
Get-ItemProperty 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\TargetVersionUpgradeExperienceIndicators\NI23H2' |
    Format-List UpgEx, UpgExU, RedReason
# RedReason indique la cause exacte (ex. "Tpm", "SecureBoot", "Cpu", "Ram", "Storage")

# 3. Log du setup
Get-Content "$env:SystemRoot\Panther\setupact.log" -Tail 50 |
    Select-String -Pattern 'Compat|TPM|SecureBoot|UEFI' -CaseSensitive:$false
```

### Résolutions par cause

```
Cause = TPM (le plus fréquent) :
  → Setup UEFI → activer le TPM / fTPM / PTT (§31).
  → Parfois nommé "Security Device Support" (ASUS) ou "Trusted Computing".

Cause = SecureBoot :
  → Passer en UEFI natif (CSM OFF), activer Secure Boot (§23).
  → Si le disque est en MBR : mbr2gpt d'abord (§8).

Cause = Disque MBR :
  → mbr2gpt /convert /disk:0 /allowFullOS (§8).

Cause = CPU non supporté :
  → Vérifier la liste Microsoft (PC trop ancien) → remplacement.
  → ⚠️ Pas de contournement registre en entreprise.

Cause = "Tout est OK mais ça bloque quand même" :
  → Mettre à jour le BIOS (vieux firmware = TPM mal exposé).
  → Vérifier que le TPM est en 2.0 (pas 1.2).
  → Lancer l'installation depuis une clé USB créée avec l'outil
    officiel (Media Creation Tool), pas une ISO bricolée.
```

---

## 73. Cas pratique 10 : disque GPT vu comme « non alloué » après un passage en mode Legacy

### Contexte

Après avoir basculé le firmware en mode Legacy/CSM « pour tester », le disque système GPT n'apparaît plus comme bootable ; dans un outil de partitionnement, il semble vide ou « non alloué ». Panique.

### Analyse

```
Rappel (§4) : un BIOS Legacy ne comprend PAS le GPT.
→ Il lit le Protective MBR (qui dit "disque plein, ne touche pas"),
  ne trouve pas de MBR bootable valide → "No bootable device".
→ Certains vieux outils affichent alors le disque comme "non alloué"
  ou proposent de l'initialiser. C'EST UN AFFICHAGE, pas une perte.
```

### Résolution (ne rien formater !)

```
1. NE PAS cliquer sur "Initialiser le disque" (cela ÉCRASERAIT le GPT).
2. Revenir dans le setup UEFI → repasser en mode UEFI (CSM OFF).
3. Le disque GPT réapparaît, Windows redémarre normalement.
4. Si Windows ne redémarre pas : vérifier l'ordre de boot
   (Windows Boot Manager en premier), puis bcdboot si besoin (§15).
```

### Vérification de l'intégrité (si un outil a touché au disque)

```powershell
# Depuis Windows (une fois rebooté en UEFI) :
Get-Disk -Number 0 | Select-Object PartitionStyle, OperationalStatus
Get-Partition -DiskNumber 0 | Select-Object PartitionNumber, Type, Size

# Si une partition a été endommagée : TestDisk (outil libre) peut
# reconstruire la table GPT grâce à sa copie de secours en fin de disque.
```

> 🔴 **Réflexe :** devant un disque « non alloué » après un changement de mode firmware, la cause est à 99 % le **mode firmware**, pas le disque. On ne formate jamais avant d'avoir remis le bon mode.

---

## 74. Cas pratique 11 : Secure Boot bloque une distribution Linux

### Contexte

Installation d'Ubuntu/Debian sur un poste avec Secure Boot activé : le live USB ne démarre pas, ou après installation, `shim` est rejeté.

### Diagnostic

```
1. La distribution a-t-elle un shim signé Microsoft ?
   ✅ Ubuntu, Fedora, RHEL, SUSE, Debian (Bullseye+) : oui.
   ❌ Arch (de base), Gentoo, Kali (selon version), customs : non.
2. Le live USB a-t-il été créé en mode UEFI ? (Rufus : "GPT / UEFI",
   pas "MBR / BIOS".)
3. Message exact : "Secure Boot Violation" → signature refusée ;
   "no bootable device" → clé non bootable en UEFI.
```

### Résolutions

```
Option A : utiliser une distribution avec shim signé (recommandé).
Option B : créer la clé USB en UEFI/GPT (souvent la vraie cause).
Option C : enrôler une clé MOK pour les modules propriétaires
  (ex. pilote NVIDIA/VirtualBox) :
    mokutil --import /var/lib/shim-signed/mok/MOK.der
  → redémarrer → l'écran bleu MOK Manager demande le mot de passe
    défini → Enroll MOK → le module est accepté.
Option D (temporaire) : désactiver Secure Boot pour l'installation,
  puis le réactiver après avoir installé le shim signé.
```

### Le cas de la dbx qui révoque un vieux shim

```
Après une mise à jour dbx Windows, un Linux en double-boot ne démarre
plus (shim révoqué car vulnérable).
→ Mettre à jour le Linux (le shim récent est signé avec un certificat
  non révoqué) : sudo apt update && sudo apt install shim-signed grub-efi.
→ Ne PAS bloquer les mises à jour dbx pour "protéger" le double-boot :
  c'est le shim qu'il faut mettre à jour.
```

---

## 75. Cas pratique 12 : le TPM se désactive après une mise à jour du BIOS

### Contexte

Après un flash du BIOS, `Get-Tpm` indique `TpmPresent = False` ou `TpmReady = False`. BitLocker menace de demander la clé.

### Pourquoi ça arrive

```
- Le nouveau firmware a réinitialisé les réglages → TPM repassé sur Off.
- Le firmware a changé le mode (Discrete ↔ fTPM) → Windows voit
  un "nouveau" TPM.
- La MAJ incluait une MAJ du firmware TPM → équivalent d'un Clear.
```

### Résolution

```powershell
# 1. D'abord : sécuriser BitLocker (ne pas rebooter en boucle sans clé !)
manage-bde -protectors -get C: -type RecoveryPassword
Suspend-BitLocker -MountPoint "C:" -RebootCount 1

# 2. Setup UEFI → Security → réactiver le TPM (même mode qu'avant : fTPM/PTT
#    ou Discrete — changer de mode = nouveau TPM = re-provisioning).

# 3. Dans Windows :
Get-Tpm | Select-Object TpmPresent, TpmReady
# Si TpmReady = False :
Initialize-Tpm
# → redémarrer si demandé (RestartPending).

# 4. Re-scellé BitLocker :
Resume-BitLocker -MountPoint "C:"
Get-BitLockerVolume -MountPoint "C:" | Select-Object VolumeStatus
```

### Prévention

```
→ Toujours noter les réglages TPM AVANT un flash (photo du setup ou
  export cctk/BiosConfigUtility).
→ Après tout flash : check-list de re-vérification (§52) incluant le TPM.
```

---

## 76. Cas pratique 13 : WDS ne répond pas aux clients d'un autre sous-réseau (VLAN)

### Contexte

Le PXE fonctionne sur le VLAN du serveur WDS, mais les clients d'un autre VLAN obtiennent `PXE-E51: No DHCP offers` ou `PXE-M0F`.

### Analyse

```
Le PXE démarre par un broadcast DHCP (Discover).
→ Les broadcasts ne traversent pas les routeurs.
→ Sans relais, le client n'a ni IP ni "boot filename".
```

### Résolution : relais DHCP (ip helper)

```
Sur le routeur / switch L3, sur l'interface du VLAN client :

Cisco IOS :
  interface Vlan20
    ip helper-address 10.1.0.10    ← serveur DHCP
    ip helper-address 10.1.0.20    ← serveur WDS (si différent du DHCP)

HP/Aruba (similaire), MikroTik :
  /ip dhcp-relay add interface=vlan20 dhcp-server=10.1.0.10,10.1.0.20

⚠️ Si DHCP et WDS sont sur la MÊME machine : un seul helper suffit,
   mais le WDS doit avoir l'option "Ne pas écouter le port 67" + option 60
   (§50), sinon conflit DHCP/WDS.
```

### Alternative : options DHCP 66/67 sur le DHCP du site distant

```
Si un DHCP local existe sur le VLAN distant :
  option 66 = IP du serveur WDS
  option 67 = boot\x64\wdsmgfw.efi   (UEFI)
             boot\x64\wdsnbp.com     (Legacy)
→ Le client obtient IP + serveur de boot en une seule réponse DHCP.
```

### Vérification

```powershell
# Depuis un client du VLAN distant (en OS) : le DHCP répond-il ?
ipconfig /all   # serveur DHCP visible ?

# Côté WDS : voit-on la requête arriver ?
Get-WinEvent -LogName 'Microsoft-Windows-Deployment-Services-Diagnostics/Operational' `
    -MaxEvents 10 | Select-Object TimeCreated, Message | Format-Table -Wrap
```

---

## 77. Cas pratique 14 : écran noir après flash du BIOS — procédure de récupération

### Contexte

Pendant ou après un flash : écran noir, ventilateurs qui tournent, aucun affichage, parfois des bips ou des LED qui clignotent. **Ne pas paniquer, ne pas multiplier les tentatives.**

### Premiers gestes (5 minutes)

```
1. Attendre 10 minutes : certains flashs font plusieurs redémarrages
   avec de longs écrans noirs. NORMAL.
2. Forcer l'extinction (10 s), débrancher 2 minutes, rebrancher, rallumer.
3. Tester un écran externe (portable) / une autre sortie vidéo (tour).
4. Écouter les bips, noter les codes LED (voir manuel constructeur).
```

### Récupération par constructeur

```
DELL — BIOS Recovery :
  1. Éteindre. Maintenir CTRL + ÉCHAP, brancher le secteur (toujours
     en maintenant), puis allumer.
  2. L'écran "BIOS Recovery" propose de restaurer depuis le disque
     ou une clé USB (fichier .rcv/.hdr du support Dell).

HP — Sure Start / Win+B :
  1. Éteindre. Maintenir WIN + B, brancher le secteur, allumer
     en maintenant 5-10 s.
  2. L'utilitaire HP BIOS Update cherche un BIOS (USB ou en ligne).
  3. Sur les modèles Sure Start récents : la restauration est AUTOMATIQUE
     (copie dorée) → le PC redémarre seul après quelques minutes.

LENOVO — Emergency Reset + USB :
  1. Trou "reset" (trombone) 10 s, ou bouton Novo.
  2. Sur ThinkPad : clé USB avec le BIOS renommé selon la doc du modèle,
     Fn+R au démarrage (selon génération — vérifier le manuel !).

GÉNÉRIQUE (carte mère tour) :
  - Fonction "BIOS Flashback" (ASUS, MSI, Gigabyte...) : clé USB avec
    le fichier renommé (ex. MSI.ROM) sur le port dédié, bouton Flashback
    5 s, PC éteint mais branché. La LED clignote pendant le flash.
  - Double BIOS (Gigabyte DualBIOS) : bascule automatique ou par jumper.
```

### Si rien ne fonctionne

```
→ Carte mère à remplacer (ou reprogrammation de la puce en atelier
  avec un programmateur SPI type CH341A — opération de laboratoire).
→ C'est pour cela qu'on ne flashe JAMAIS sans onduleur/secteur stable
  et sans raison valable (§52).
```

---

## 78. Cas pratique 15 : double boot Windows/Linux cassé après une mise à jour Windows

### Contexte

Après une mise à jour majeure de Windows, le menu GRUB a disparu : le PC démarre directement sur Windows. Ou l'inverse : GRUB s'affiche mais Windows ne démarre plus.

### Analyse

```
Windows a réécrit l'entrée de boot par défaut / restauré son chargeur
comme premier dans BootOrder, ou régénéré le BCD.
→ Les fichiers Linux (shim, GRUB) sont toujours sur l'ESP : rien n'est
  perdu, c'est l'ORDRE qui a changé.
```

### Résolution A : GRUB a disparu (boot direct Windows)

```powershell
# Depuis Windows (admin) : remettre le chargeur Linux en premier
# 1. Voir les entrées firmware
bcdedit /enum firmware

# 2. Identifier l'entrée Linux (ex. {xxxxxxxx-...} "ubuntu")
# 3. La passer en premier :
bcdedit /set '{fwbootmgr}' displayorder '{GUID-ubuntu}' /addfirst

# Alternative : au démarrage, touche F12 (menu de boot) → choisir ubuntu
# → puis dans Linux : sudo efibootmgr -o XXXX,YYYY (remettre l'ordre).
```

### Résolution B : GRUB s'affiche mais Windows ne boote plus

```bash
# Depuis Linux : réparer l'entrée Windows
sudo os-prober          # doit détecter Windows
sudo update-grub        # régénère grub.cfg avec Windows
# Si os-prober ne voit rien : vérifier que l'ESP est montée et que
# /EFI/Microsoft/Boot/bootmgfw.efi existe toujours.
```

```powershell
# Depuis Windows (si GRUB est le problème, pas Windows) : restaurer
# le chargeur Windows comme défaut sans effacer GRUB :
bcdboot C:\Windows /s S: /f UEFI
bcdedit /set '{fwbootmgr}' displayorder '{bootmgr}' /addfirst
```

### Prévention

```
→ Après chaque mise à jour majeure Windows sur un double-boot :
  vérifier l'ordre de boot (bcdedit /enum firmware).
→ Garder une clé USB de réparation (boot-repair ou WinPE) à portée.
→ En entreprise : éviter le double-boot sur les postes de production ;
  préférer une VM ou un poste dédié.
```

---

## 79. Erreurs classiques : les 18 pièges à éviter

| N° | Erreur | Conséquence | Bonne pratique |
|---|---|---|---|
| 1 | Flasher le BIOS sans suspendre BitLocker | Clé de récupération demandée (panique) | `Suspend-BitLocker -RebootCount 1` avant |
| 2 | `Clear-Tpm` sans séquestre de la clé BitLocker | **Données perdues** | Vérifier + sauvegarder la clé avant |
| 3 | Effacer les clés Secure Boot « pour tester » | Secure Boot désactivé, machine en Setup Mode | Ne jamais effacer sans plan de re-provisioning |
| 4 | Passer en Legacy/CSM « parce que ça bootait avant » | Windows 11 impossible, Secure Boot perdu | Rester en UEFI ; convertir avec mbr2gpt |
| 5 | Formater l'ESP « pour faire du propre » | PC non bootable | ESP intouchable ; `bcdboot` si besoin |
| 6 | Initialiser un disque GPT en MBR dans le gestionnaire de disques | Perte de la table GPT | Vérifier le style AVANT toute action |
| 7 | Désactiver Secure Boot définitivement pour un outil | Porte ouverte aux bootkits | Désactivation temporaire, tracée, réactivée |
| 8 | Mot de passe BIOS posé sans le consigner | Procédure constructeur lourde (§69) | Coffre d'équipe dès la réception |
| 9 | MAJ firmware TPM sans précaution | Équivaut à un Clear TPM → BitLocker bloqué | Même protocole que Clear TPM (§56) |
| 10 | Options DHCP 66/67 + WDS sur le même réseau | Conflit, PXE-E53 | Option 60 du WDS, pas 66/67 (§50) |
| 11 | Oublier les pilotes NIC/NVMe dans boot.wim | WinPE sans réseau/disque | Injecter les pilotes (§49) |
| 12 | `bcdedit` sans export préalable | BCD cassé sans filet | `bcdedit /export` systématique |
| 13 | Laisser `testsigning Yes` après un dépannage | Pilotes non signés acceptés durablement | Le désactiver + vérifier le filigrane |
| 14 | Contourner les exigences Windows 11 (BypassTPMCheck) | Parc non supporté, non patché | Remplacer le matériel non conforme |
| 15 | WoL qui « ne marche pas » | Démarrage rapide Windows activé | Désactiver le fast startup + config carte (§45) |
| 16 | Clé USB d'install créée en MBR pour un poste UEFI | Setup qui ne démarre pas en UEFI | Rufus en GPT/UEFI, ou Media Creation Tool |
| 17 | Ne pas re-vérifier les réglages après un flash | TPM/Secure Boot/VT-x retombés par défaut | Check-list post-flash (§52) |
| 18 | Stocker la clé BitLocker uniquement sur le poste | Clé perdue avec le poste | Séquestre AD/Entra ID systématique (§38) |

---

## 80. Checklist : configuration d'un poste neuf en entreprise

### À la réception (atelier)

- [ ] Vérifier le modèle, le numéro de série, la garantie (enregistrer dans l'inventaire)
- [ ] Flasher le dernier BIOS **validé** (matrice §60)
- [ ] Restaurer les réglages par défaut, puis appliquer le **profil standard** :
  - [ ] Mode UEFI natif (CSM désactivé)
  - [ ] Secure Boot → Enabled (Standard)
  - [ ] TPM 2.0 → On, provisionné (`Get-Tpm` → TpmReady)
  - [ ] Virtualisation → Enabled (VT-x/AMD-V + VT-d/AMD-Vi)
  - [ ] Ordre de boot → disque en premier
  - [ ] USB boot → Disabled (sauf exception)
  - [ ] PXE → selon politique (Enabled si déploiement réseau)
  - [ ] WoL → selon politique
  - [ ] Mot de passe admin UEFI → défini, consigné au coffre
- [ ] Vérifier le disque en GPT (ou convertir si image Legacy)

### Déploiement de l'OS

- [ ] Déployer l'image master (WDS/MDT/SCCM/Intune)
- [ ] Vérifier : `Confirm-SecureBootUEFI` = True, `Get-Tpm` OK
- [ ] Activer BitLocker (TPM ou TPM+PIN selon profil), **vérifier le séquestre**
- [ ] Jonction au domaine / Entra ID, GPO/Intune appliquées
- [ ] Inventaire : ajouter la machine au CSV (§40)

### Avant remise à l'utilisateur

- [ ] Test de redémarrage à froid + sortie de veille
- [ ] Vérifier qu'aucun mot de passe ne bloque l'utilisateur (PIN BitLocker communiqué si TPM+PIN)
- [ ] Fiche de remise : consigner le modèle, le n° de série, la clé BitLocker séquestrée

---

## 81. Pense-bête de poche

### Les 10 commandes à connaître par cœur

```powershell
Confirm-SecureBootUEFI                                  # Secure Boot ? True/False
Get-Tpm | Select-Object TpmPresent,TpmReady             # TPM OK ?
Get-Disk | Select-Object Number,PartitionStyle,Size     # GPT ou MBR ?
bcdedit /enum                                           # Config de boot
bcdedit /export C:\BCD-backup                          # Sauvegarder le BCD
bcdboot C:\Windows /s S: /f UEFI                        # Réparer le boot UEFI
mbr2gpt /validate /disk:0 /allowFullOS                  # Tester la conversion
mbr2gpt /convert /disk:0 /allowFullOS                   # Convertir en GPT
manage-bde -status C:                                   # État BitLocker
Suspend-BitLocker -MountPoint "C:" -RebootCount 1       # Avant flash BIOS
```

### Touches d'accès au firmware (indicatif)

| Constructeur | Setup | Menu de boot |
|---|---|---|
| Dell | F2 | F12 |
| HP | F10 (ou Échap puis F10) | F9 (ou Échap puis F9) |
| Lenovo ThinkPad | Entrée puis F1 (ou F2) | F12 |
| ASUS | DEL ou F2 | F8 |
| MSI | DEL | F11 |
| Gigabyte | DEL | F12 |

### Les 5 réflexes

```
1. AVANT de toucher au BCD/firmware : exporter/sauvegarder.
2. AVANT un flash : BitLocker suspendu + séquestre vérifié + secteur.
3. DEVANT un disque "non alloué" : vérifier le MODE FIRMWARE d'abord.
4. DEVANT une demande BitLocker "spontanée" : chercher la CAUSE (MAJ ? intrusion ?).
5. Secure Boot désactivé = TEMPORAIRE + TRACÉ + RÉACTIVÉ.
```

### Numéros utiles à noter (par poste)

```
Modèle : _______________  N° série : _______________
Version BIOS : _______________  Date : _______________
TPM : [ ] discret [ ] fTPM/PTT   Version : _______________
Secure Boot : [ ] ON   Clé BitLocker séquestrée : [ ] AD [ ] Entra [ ] fichier
Mot de passe admin UEFI consigné au coffre : [ ] oui
```

---

## 82. Glossaire

| Terme | Définition |
|---|---|
| **ACPI** | Interface de gestion d'énergie/matériel entre firmware et OS. |
| **AHCI** | Mode du contrôleur SATA (vs IDE/RAID) ; à figer avant l'install. |
| **Attestation** | Preuve cryptographique à distance de l'intégrité du démarrage (TPM). |
| **BCD** | Boot Configuration Data : base de config du démarrage Windows. |
| **BMC** | Contrôleur de gestion de la carte mère (iDRAC, iLO...) : administration hors OS. |
| **Boot Guard** | Technologie Intel : vérification matérielle du firmware avant exécution. |
| **Bootkit** | Malware s'exécutant avant l'OS, dans la chaîne de démarrage. |
| **bcdboot** | Outil de reconstruction des fichiers de démarrage Windows. |
| **bcdedit** | Outil de lecture/écriture du BCD. |
| **CSM** | Compatibility Support Module : émulation BIOS dans un firmware UEFI. |
| **db** | Base des signatures autorisées par Secure Boot. |
| **dbx** | Base des signatures révoquées par Secure Boot. |
| **DMA** | Accès direct à la mémoire par un périphérique (risque si non isolé). |
| **ELAM** | Early Launch Anti-Malware : pilote antivirus chargé en premier au boot. |
| **ESP** | EFI System Partition : partition FAT32 contenant les chargeurs UEFI. |
| **Evil maid** | Attaque avec accès physique temporaire au poste. |
| **Fast Boot** | Initialisation firmware accélérée (saute certains tests). |
| **fTPM** | TPM implémenté en firmware (AMD) ; équivalent Intel : PTT. |
| **GPT** | GUID Partition Table : table de partitions moderne (UEFI). |
| **HVCI** | Hypervisor-protected Code Integrity (Memory Integrity) : protection du noyau. |
| **iPXE** | Firmware PXE avancé open source (HTTP, scripts). |
| **KEK** | Key Exchange Key : signe les mises à jour de db/dbx. |
| **Kernel DMA Protection** | Protection du noyau contre les attaques DMA (Thunderbolt). |
| **MBR** | Master Boot Record : secteur 0 + table de partitions historique. |
| **mbr2gpt** | Outil Microsoft de conversion MBR→GPT sans perte. |
| **Measured Boot** | Enregistrement des mesures de boot dans les PCR du TPM. |
| **MOK** | Machine Owner Key : clés Secure Boot gérées par l'utilisateur (Linux). |
| **MSR** | Partition Microsoft Reserved (16 Mo) sur disque GPT. |
| **NVRAM** | Mémoire non volatile du firmware (variables UEFI, ordre de boot). |
| **Option ROM** | Firmware d'une carte d'extension (réseau, RAID...), vérifié par Secure Boot. |
| **PCR** | Platform Configuration Registers : registres de mesures du TPM. |
| **PK** | Platform Key : clé maîtresse de Secure Boot (propriétaire plateforme). |
| **POST** | Power-On Self-Test : tests matériels au démarrage. |
| **Protective MBR** | Faux MBR protégeant un disque GPT des vieux outils. |
| **PTT** | Intel Platform Trust Technology : TPM firmware d'Intel. |
| **PXE** | Preboot Execution Environment : boot réseau (DHCP+TFTP). |
| **Redfish** | API REST standard d'administration des serveurs (via BMC). |
| **Scellage (sealing)** | Chiffrement de données liées à un état PCR du TPM. |
| **Secure Boot** | Vérification des signatures à chaque maillon du démarrage (UEFI). |
| **Setup Mode** | État sans PK : les clés Secure Boot sont modifiables librement. |
| **Shim** | Petit chargeur Linux signé Microsoft, premier maillon Secure Boot sous Linux. |
| **SMBIOS** | Tables d'inventaire matériel exposées par le firmware (dmidecode). |
| **TPM** | Trusted Platform Module : composant cryptographique, racine de confiance. |
| **UEFI** | Unified Extensible Firmware Interface : firmware moderne remplaçant le BIOS. |
| **VBS** | Virtualization-Based Security : sécurité basée sur l'hyperviseur. |
| **vTPM** | TPM virtuel (VM Hyper-V gen2). |
| **WDS** | Windows Deployment Services : déploiement réseau d'images Windows. |
| **WinPE** | Windows Preinstallation Environment : mini-Windows de déploiement/dépannage. |
| **WinRE** | Windows Recovery Environment : environnement de récupération. |
| **WoL** | Wake-on-LAN : allumage à distance par paquet magique. |

---

## 83. Quiz : 10 questions pour valider

**Q1.** Un disque MBR peut adresser au maximum : a) 128 partitions / b) 4 partitions primaires / c) 2 To par partition ?
> **R1.** **b) 4 partitions primaires** (et le disque entier est limité à ~2 To par l'adressage 32 bits). La limite « 2 To » concerne le disque, pas la partition.

**Q2.** Pourquoi une mise à jour du BIOS déclenche-t-elle la récupération BitLocker ?
> **R2.** BitLocker scelle la clé de chiffrement sur les **PCR du TPM**, dont **PCR 0 = mesure du firmware**. Le nouveau BIOS change cette mesure → le TPM refuse de desceller → BitLocker demande la clé de récupération. Prévention : `Suspend-BitLocker -RebootCount 1` avant le flash.

**Q3.** Citez les 4 bases de clés de Secure Boot et le rôle de chacune.
> **R3.** **PK** (Platform Key : clé maîtresse, signe les MAJ de KEK, définit le propriétaire) ; **KEK** (signe les MAJ de db/dbx) ; **db** (signatures autorisées à booter) ; **dbx** (signatures révoquées/interdites).

**Q4.** Quelle commande convertit un disque système MBR en GPT sans perte de données, et quelle est la condition n°1 avant de la lancer ?
> **R4.** `mbr2gpt /convert /disk:0 /allowFullOS` (après `/validate`). Condition n°1 : **suspendre BitLocker** (et vérifier le séquestre de la clé de récupération), puis basculer le firmware en UEFI après conversion.

**Q5.** `Confirm-SecureBootUEFI` retourne une erreur « cmdlet not supported ». Deux causes possibles ?
> **R5.** La machine démarre en **mode Legacy/BIOS** (pas d'UEFI), ou le firmware ne supporte pas Secure Boot (matériel trop ancien). Dans les deux cas : non conforme Windows 11.

**Q6.** Quelle est la différence entre un TPM discret et un fTPM / Intel PTT ?
> **R6.** Le TPM **discret** est une puce dédiée soudée (isolation matérielle maximale) ; le **fTPM/PTT** est implémenté en logiciel dans le firmware/CPU (gratuit, standard, suffisant pour Windows 11). Les deux sont des TPM 2.0 valides.

**Q7.** Un poste affiche `PXE-E53: No boot filename received`. Citez 3 vérifications dans l'ordre.
> **R7.** 1) Le service **WDSServer** est-il démarré ? 2) Le **pare-feu** laisse-t-il passer UDP 67/68/69/4011 ? 3) Si DHCP et WDS cohabitent : options **« Ne pas écouter le port 67 » + option DHCP 60** activées ? (Puis : stratégie de réponse, fichier `wdsmgfw.efi` présent.)

**Q8.** Pourquoi ne faut-il JAMAIS faire un `Clear-Tpm` « pour voir » sur un poste chiffré ?
> **R8.** Le Clear TPM **détruit définitivement** toutes les clés du TPM, y compris celle qui descelle BitLocker. Sans la **clé de récupération à 48 chiffres** (séquestrée), les données sont perdues. Règle : séquestre vérifié d'abord, Clear ensuite.

**Q9.** Secure Boot est-il une protection contre le vol de données en cas de vol du PC ? Justifiez.
> **R9.** **Non, pas seul.** Secure Boot garantit l'intégrité du démarrage (pas de chargeur pirate), mais **ne chiffre rien** : le disque reste lisible si on le monte ailleurs. Contre le vol : **BitLocker** (chiffrement) + TPM + PIN.

**Q10.** Citez 3 réglages firmware à vérifier après chaque mise à jour du BIOS, et pourquoi.
> **R10.** **Secure Boot** (peut être retombé sur Disabled/Setup), **TPM** (peut être désactivé → BitLocker/Windows 11 en panne), **ordre de boot et mode UEFI/CSM** (un retour en Legacy rend le disque GPT non bootable). Bonus : virtualisation, mot de passe admin.

---

## 84. Pour aller plus loin

### Documentation officielle

- **UEFI Forum** — Spécification UEFI (chapitre 27 : Secure Boot) : uefi.org/specifications
- **TCG** — Spécifications TPM 2.0 (Trusted Computing Group) : trustedcomputinggroup.org
- **Microsoft Learn** — Secure Boot, TPM, BitLocker, WDS : learn.microsoft.com
- **NIST SP 800-147** — Protection du BIOS ; **NIST SP 800-155** — Intégrité du firmware

### Outils à maîtriser

| Outil | Usage |
|---|---|
| `Dell Command Configure` / `Update` | Gestion BIOS Dell en masse |
| `HP BIOS Configuration Utility` / `Image Assistant` | Gestion BIOS HP en masse |
| `Lenovo System Update` / Thin Installer | Gestion BIOS Lenovo en masse |
| **MDT** (Microsoft Deployment Toolkit) | Industrialiser le déploiement au-dessus de WDS |
| **Rufus** | Créer des clés USB UEFI/GPT propres |
| **TestDisk** | Reconstruire une table GPT endommagée |
| `mokutil` / `efibootmgr` | Gérer Secure Boot et les entrées UEFI sous Linux |

### Sujets connexes à creuser

- **Microsoft Pluton** : le processeur de sécurité intégré aux CPU récents (au-delà du TPM).
- **DRTM** (Dynamic Root of Trust for Measurement) : Intel TXT / AMD SKINIT — mesurer *après* le firmware.
- **Supply chain firmware** : attaques de la chaîne d'approvisionnement (firmwares pré-infectés).
- **Confidential Computing** : AMD SEV-SNP, Intel TDX — chiffrer la mémoire des VM dans le cloud.
- **Passkeys / FIDO2** : le TPM comme ancrage des clés d'authentification sans mot de passe.

### Feuille de route suggérée pour votre parc (6 mois)

```
Mois 1-2 : inventaire complet (§40), matrice modèles/firmwares, coffre
           mots de passe, séquestre BitLocker vérifié à 100 %.
Mois 3    : vague pilote MAJ firmware + activation TPM/Secure Boot (10 %).
Mois 4    : généralisation par vagues, conversion mbr2gpt des Legacy.
Mois 5    : durcissement (profils BIOS standard, USB boot, check-list §59),
           migration Windows 11 des postes conformes.
Mois 6    : audit final, documentation, plan de maintien (trimestriel).
```

---

*Fin du guide — BIOS / UEFI / Secure Boot / TPM 2.0 en entreprise.*
*Document de travail : adaptez les chemins et options à vos modèles exacts, et testez toute procédure sur un lot pilote avant généralisation.*
