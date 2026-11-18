# Windows 11 en entreprise — Guide technique ultra-complet

**Déploiement, sécurité, dépannage — Windows 11 23H2 / 24H2**
**Public : administrateurs systèmes, chefs de service systèmes & énergies**
**Dernière révision : septembre 2026**

> ⚠️ **Avertissement de cadrage** : ce guide cible les éditions **Pro et Entreprise** de Windows 11 (builds 22631+ pour 23H2, 26100+ pour 24H2). Certaines stratégies et fonctionnalités (ex. : AppLocker avancé, Credential Guard via stratégie de groupe native, Windows Autopilot) exigent **Entreprise** ou un abonnement Microsoft 365 E3/E5. Les commandes PowerShell de ce guide ont été rédigées pour PowerShell 5.1 (intégré) sauf mention contraire. Les mots de passe et clés cités sont fictifs : ne jamais saisir de secrets réels dans un fichier Markdown.

---

## Table des matières

1. [Vue d'ensemble et positionnement de Windows 11](#1-vue-densemble-et-positionnement-de-windows-11)
2. [Exigences matérielles : TPM 2.0, Secure Boot, CPU, RAM, stockage](#2-exigences-matérielles--tpm-20-secure-boot-cpu-ram-stockage)
3. [Vérifier la compatibilité : PC Health Check, Get-TPM, scripts d'inventaire](#3-vérifier-la-compatibilité--pc-health-check-get-tpm-scripts-dinventaire)
4. [Éditions Windows 11 : Pro vs Entreprise, canaux et cycle de vie](#4-éditions-windows-11--pro-vs-entreprise-canaux-et-cycle-de-vie)
5. [Obtenir les médias : ISO, MCT, VLSC, canaux de mise à jour](#5-obtenir-les-médias--iso-mct-vlsc-canaux-de-mise-à-jour)
6. [Installation propre : clé USB bootable, étapes, pilotes](#6-installation-propre--clé-usb-bootable-étapes-pilotes)
7. [Mise à niveau depuis Windows 10 : chemins, outils, pièges](#7-mise-à-niveau-depuis-windows-10--chemins-outils-pièges)
8. [Installation automatisée : unattend.xml, clés de produit, setupconfig](#8-installation-automatisée--unattendxml-clés-de-produit-setupconfig)
9. [Sysprep : généralisation, audit mode, capture d'image](#9-sysprep--généralisation-audit-mode-capture-dimage)
10. [MDT : installation, deployment share, task sequences](#10-mdt--installation-deployment-share-task-sequences)
11. [MDT : Lite Touch, pilotes, règles CustomSettings.ini](#11-mdt--lite-touch-pilotes-règles-customsettingsini)
12. [Autopilot + Intune : principe, profils, inscription](#12-autopilot--intune--principe-profils-inscription)
13. [Autopilot Reset, Fresh Start et scénarios de réaffectation](#13-autopilot-reset-fresh-start-et-scénarios-de-réaffectation)
14. [Jointure au domaine vs Entra join vs hybride](#14-jointure-au-domaine-vs-entra-join-vs-hybride)
15. [BitLocker : fondamentaux, TPM seul vs TPM+PIN, XTS-AES](#15-bitlocker--fondamentaux-tpm-seul-vs-tpmpin-xts-aes)
16. [BitLocker : clés de récupération (AD, fichier, impression, Entra)](#16-bitlocker--clés-de-récupération-ad-fichier-impression-entra)
17. [BitLocker : stratégies GPO, MBAM / BitLocker Management, déploiement silencieux](#17-bitlocker--stratégies-gpo-mbam--bitlocker-management-déploiement-silencieux)
18. [BitLocker : dépannage — écran de récupération, boucles, causes fréquentes](#18-bitlocker--dépannage--écran-de-récupération-boucles-causes-fréquentes)
19. [Profils utilisateurs : locaux, itinérants, obligatoires](#19-profils-utilisateurs--locaux-itinérants-obligatoires)
20. [FSLogix : principe, conteneurs de profils, cas d'usage](#20-fslogix--principe-conteneurs-de-profils-cas-dusage)
21. [Migration de profils : USMT (ScanState / LoadState)](#21-migration-de-profils--usmt-scanstate--loadstate)
22. [GPO Windows 11 : menu Démarrer, barre des tâches, disposition](#22-gpo-windows-11--menu-démarrer-barre-des-tâches-disposition)
23. [GPO Windows 11 : désactiver Copilot, Widgets, recommandations](#23-gpo-windows-11--désactiver-copilot-widgets-recommandations)
24. [GPO Windows 11 : bonnes pratiques, héritage, filtrage, dépannage](#24-gpo-windows-11--bonnes-pratiques-héritage-filtrage-dépannage)
25. [Windows Update for Business : anneaux de déploiement](#25-windows-update-for-business--anneaux-de-déploiement)
26. [WUfB : stratégies (CSP, GPO, Intune), délais, pause, qualité vs fonctionnalités](#26-wufb--stratégies-csp-gpo-intune-délais-pause-qualité-vs-fonctionnalités)
27. [WSUS vs Windows Update for Business : comparatif et coexistence](#27-wsus-vs-windows-update-for-business--comparatif-et-coexistence)
28. [Déploiement d'applications : winget en entreprise](#28-déploiement-dapplications--winget-en-entreprise)
29. [Microsoft Store en entreprise, applications privées, packaging](#29-microsoft-store-en-entreprise-applications-privées-packaging)
30. [Sécurité : Microsoft Defender Antivirus — configuration entreprise](#30-sécurité--microsoft-defender-antivirus--configuration-entreprise)
31. [Sécurité : SmartScreen, protection réseau, contrôle des applications](#31-sécurité--smartscreen-protection-réseau-contrôle-des-applications)
32. [Sécurité : Credential Guard, LSA Protection, HVCI / Memory Integrity](#32-sécurité--credential-guard-lsa-protection-hvci--memory-integrity)
33. [Sécurité : durcissement — baselines Microsoft, ASR, Attack Surface Reduction](#33-sécurité--durcissement--baselines-microsoft-asr-attack-surface-reduction)
34. [LAPS Windows (nouveau) : principe, GPO, récupération](#34-laps-windows-nouveau--principe-gpo-récupération)
35. [Pare-feu Windows Defender : profils, règles, GPO, journalisation](#35-pare-feu-windows-defender--profils-règles-gpo-journalisation)
36. [Réseau : Wi-Fi d'entreprise 802.1X (WPA2/WPA3-Enterprise)](#36-réseau--wi-fi-dentreprise-8021x-wpa2wpa3-enterprise)
37. [Réseau : VPN — Always On VPN (intro), profils, dépannage](#37-réseau--vpn--always-on-vpn-intro-profils-dépannage)
38. [Réseau : dépannage — pile TCP/IP, DNS, proxy, certificats](#38-réseau--dépannage--pile-tcpip-dns-proxy-certificats)
39. [WinRE : environnement de récupération, outils, personnalisation](#39-winre--environnement-de-récupération-outils-personnalisation)
40. [Points de restauration, sauvegardes système, récupération](#40-points-de-restauration-sauvegardes-système-récupération)
41. [Dépannage : méthodologie et boîte à outils de l'admin](#41-dépannage--méthodologie-et-boîte-à-outils-de-ladmin)
42. [Cas pratique 1 : le PC ne démarre plus (écran noir, BOOTMGR, BCD)](#42-cas-pratique-1--le-pc-ne-démarre-plus-écran-noir-bootmgr-bcd)
43. [Cas pratique 2 : boucle de réparation automatique / WinRE en boucle](#43-cas-pratique-2--boucle-de-réparation-automatique--winre-en-boucle)
44. [Cas pratique 3 : BitLocker demande la clé de récupération en boucle](#44-cas-pratique-3--bitlocker-demande-la-clé-de-récupération-en-boucle)
45. [Cas pratique 4 : mise à jour Windows bloquée / échec répété](#45-cas-pratique-4--mise-à-jour-windows-bloquée--échec-répété)
46. [Cas pratique 5 : mise à niveau 23H2 → 24H2 qui échoue](#46-cas-pratique-5--mise-à-niveau-23h2--24h2-qui-échoue)
47. [Cas pratique 6 : pilote défectueux — écran bleu, rollback, signature](#47-cas-pratique-6--pilote-défectueux--écran-bleu-rollback-signature)
48. [Cas pratique 7 : performances dégradées — disque, mémoire, processus](#48-cas-pratique-7--performances-dégradées--disque-mémoire-processus)
49. [Cas pratique 8 : profil utilisateur corrompu / temporaire](#49-cas-pratique-8--profil-utilisateur-corrompu--temporaire)
50. [Cas pratique 9 : impossible de joindre le domaine / relation d'approbation](#50-cas-pratique-9--impossible-de-joindre-le-domaine--relation-dapprobation)
51. [Cas pratique 10 : GPO qui ne s'applique pas](#51-cas-pratique-10--gpo-qui-ne-sapplique-pas)
52. [Cas pratique 11 : Wi-Fi 802.1X — échec d'authentification](#52-cas-pratique-11--wi-fi-8021x--échec-dauthentification)
53. [Cas pratique 12 : VPN qui ne se connecte pas](#53-cas-pratique-12--vpn-qui-ne-se-connecte-pas)
54. [Cas pratique 13 : applications qui ne se lancent plus / Store corrompu](#54-cas-pratique-13--applications-qui-ne-se-lancent-plus--store-corrompu)
55. [Cas pratique 14 : espace disque saturé après mise à niveau](#55-cas-pratique-14--espace-disque-saturé-après-mise-à-niveau)
56. [Cas pratique 15 : Windows Hello / biométrie ne fonctionne plus](#56-cas-pratique-15--windows-hello--biométrie-ne-fonctionne-plus)
57. [Cas pratique 16 : imprimante réseau / partage SMB inaccessible](#57-cas-pratique-16--imprimante-réseau--partage-smb-inaccessible)
58. [Fin de vie de Windows 10 (14/10/2025) : stratégie de migration](#58-fin-de-vie-de-windows-10-14102025--stratégie-de-migration)
59. [Inventaire du parc : PowerShell, GLPI, Intune, rapports](#59-inventaire-du-parc--powershell-glpi-intune-rapports)
60. [Supervision et journaux : Event Viewer, journaux clés, collecte](#60-supervision-et-journaux--event-viewer-journaux-clés-collecte)
61. [Scripts utiles : boîte à outils PowerShell de l'admin](#61-scripts-utiles--boîte-à-outils-powershell-de-ladmin)
62. [Sécurité avancée : Secure Boot, measured boot, attestation](#62-sécurité-avancée--secure-boot-measured-boot-attestation)
63. [Chiffrement avancé : BitLocker réseau (Network Unlock), DRA](#63-chiffrement-avancé--bitlocker-réseau-network-unlock-dra)
64. [Conformité : stratégies de conformité Intune, base de référence](#64-conformité--stratégies-de-conformité-intune-base-de-référence)
65. [Kiosque et postes partagés : Assigned Access, invité](#65-kiosque-et-postes-partagés--assigned-access-invité)
66. [Impression en entreprise : Universal Print, Point and Print](#66-impression-en-entreprise--universal-print-point-and-print)
67. [Langues, régions, claviers : déploiement multilingue](#67-langues-régions-claviers--déploiement-multilingue)
68. [Télémétrie et confidentialité : niveaux, GPO, registre](#68-télémétrie-et-confidentialité--niveaux-gpo-registre)
69. [Microsoft 365 Apps : déploiement, canaux, licences](#69-microsoft-365-apps--déploiement-canaux-licences)
70. [Navigateurs en entreprise : Edge géré, stratégies](#70-navigateurs-en-entreprise--edge-géré-stratégies)
71. [Sauvegarde et PRA poste de travail](#71-sauvegarde-et-pra-poste-de-travail)
72. [Gestion de l'énergie : stratégies, veille moderne, batteries](#72-gestion-de-lénergie--stratégies-veille-moderne-batteries)
73. [Erreurs classiques (20) : ce qu'il ne faut pas faire](#73-erreurs-classiques-20--ce-quil-ne-faut-pas-faire)
74. [Checklist : déploiement d'un poste de A à Z](#74-checklist--déploiement-dun-poste-de-a-à-z)
75. [Checklist : audit de sécurité d'un poste Windows 11](#75-checklist--audit-de-sécurité-dun-poste-windows-11)
76. [Pense-bête de poche : commandes et raccourcis essentiels](#76-pense-bête-de-poche--commandes-et-raccourcis-essentiels)
77. [Glossaire](#77-glossaire)
78. [Quiz : 10 questions + réponses commentées](#78-quiz--10-questions--réponses-commentées)
79. [Pour aller plus loin : documentation, outils, communautés](#79-pour-aller-plus-loin--documentation-outils-communautés)
80. [Annexe A : tableau des builds et versions](#80-annexe-a--tableau-des-builds-et-versions)
81. [Annexe B : chemins de clés de registre les plus utiles](#81-annexe-b--chemins-de-clés-de-registre-les-plus-utiles)
82. [Annexe C : modèle de plan de migration Windows 10 → 11](#82-annexe-c--modèle-de-plan-de-migration-windows-10--11)

---

## 1. Vue d'ensemble et positionnement de Windows 11

Windows 11 est la version cliente actuelle de Windows pour l'entreprise. Par rapport à Windows 10, les changements qui impactent le plus l'administration :

| Domaine | Windows 10 | Windows 11 | Impact admin |
|---|---|---|---|
| Exigences matérielles | Souples | TPM 2.0 + Secure Boot + CPU listé obligatoires | Inventaire parc indispensable avant migration |
| Menu Démarrer / barre des tâches | Personnalisable (tuiles) | Centré, non déplaçable, tuiles supprimées | Repackaging des layouts, GPO à revoir |
| Mises à jour | 2 feature updates/an (puis 1) | 1 mise à jour annuelle de fonctionnalités | Anneaux WUfB simplifiés |
| Cycle de vie Entreprise | 30 mois (H2) | 36 mois | Fenêtre de migration plus large |
| Sécurité par défaut | Opt-in | HVCI, Secure Boot, TPM exigés | Durcissement « gratuit » mais compat à tester |
| Store | Consumer | Bridge Entreprise (privé) | Nouveau packaging à prévoir |
| Copilot / Widgets / Teams Chat | Absents | Intégrés | GPO de désactivation en entreprise |

**Règle d'or** : ne jamais déployer Windows 11 comme « Windows 10 avec un nouveau thème ». C'est un changement de socle matériel et de modèle de gestion (cloud-first via Intune/Autopilot) qui mérite un projet dédié.

---

## 2. Exigences matérielles : TPM 2.0, Secure Boot, CPU, RAM, stockage

### 2.1 Tableau des exigences officielles

| Composant | Exigence minimale | Recommandé en entreprise |
|---|---|---|
| Processeur | 1 GHz+, 2 cœurs+, 64 bits, **dans la liste Microsoft** | Intel 8e gén+ / AMD Ryzen 2000+ / équivalent |
| RAM | 4 Go | 8 Go (bureautique), 16 Go (dev/CAO, multitâche lourd) |
| Stockage | 64 Go | 256 Go SSD NVMe (128 Go = trop juste avec profils + cache) |
| Firmware | UEFI, **Secure Boot capable** | Secure Boot activé |
| TPM | **TPM 2.0** (pas 1.2) | TPM 2.0 activé dans le BIOS/UEFI |
| Carte graphique | DirectX 12 / WDDM 2.0 | Intégrée suffisante en bureautique |
| Écran | 9"+, 720p | — |
| Internet | Requis pour l'OOBE (édition Famille) et les MàJ | — |

> ⚠️ **Point critique** : un PC avec TPM 1.2, un CPU 7e génération Intel ou un BIOS Legacy **ne passera pas** la vérification officielle, même si Windows 11 « pourrait » tourner. Les contournements de registre (`AllowUpgradesWithUnsupportedTPMOrCPU`) existent mais **ne sont pas supportés** : pas de mises à jour garanties, pas de support Microsoft. En entreprise : à proscrire.

### 2.2 TPM 2.0 — comprendre ce que c'est

Le TPM (Trusted Platform Module) est une puce (ou firmware : Intel PTT, AMD fTPM) qui :

- stocke les clés de chiffrement BitLocker sans les exposer à l'OS ;
- mesure l'intégrité du démarrage (measured boot) ;
- sert d'ancre pour Windows Hello, Credential Guard, l'attestation.

```powershell
# État du TPM : la commande de référence
Get-Tpm

# Sortie attendue sur un poste compatible :
# TpmPresent                : True
# TpmReady                  : True
# TpmEnabled                : True
# TpmActivated              : True
# ManagedAuthLevel          : Full
# OwnerAuth                 : (vide si géré par l'OS)
# AutoProvisioning          : Enabled
# LockedOut                 : False
```

```powershell
# Version du TPM (doit être 2.0)
(Get-CimInstance -Namespace root/cimv2/security/microsofttpm -ClassName Win32_Tpm).SpecVersion
# Exemple de sortie : 2.0, 0, 1.16
```

```powershell
# TPM via WMI (utile en inventaire à distance)
Get-WmiObject -Namespace "root\CIMV2\Security\MicrosoftTpm" -Class Win32_Tpm |
    Select-Object SpecVersion, IsEnabled, IsActivated, IsOwned
```

### 2.3 Secure Boot — vérification

```powershell
# État de Secure Boot
Confirm-SecureBootUEFI
# True = activé. Erreur "privilège non détenu" = BIOS Legacy ou Secure Boot non supporté.

# Via le registre (inventaire) :
Get-ItemProperty "HKLM:\SYSTEM\CurrentControlSet\Control\SecureBoot\State" -Name UEFISecureBootEnabled
```

### 2.4 CPU supportés — où vérifier

Microsoft publie des listes par génération (Intel, AMD, Qualcomm). En pratique :

- **Intel** : 8e génération (Coffee Lake) et supérieures. Quelques 7e gén spécifiques (ex. i7-7820HQ) sont listés mais c'est l'exception.
- **AMD** : Ryzen 2000 (Zen+) et supérieures, EPYC/Threadripper récents.
- **Qualcomm** : Snapdragon 850 et supérieures.

```powershell
# Identifier le CPU pour croiser avec la liste
Get-CimInstance Win32_Processor | Select-Object Name, Manufacturer, MaxClockSpeed
```

### 2.5 RAM et stockage — dimensionnement entreprise

| Usage | RAM | Stockage | Justification |
|---|---|---|---|
| Bureautique légère (web, Office) | 8 Go | 256 Go SSD | Marge pour MàJ (Windows.old, WinSxS) |
| Bureautique standard + visio | 16 Go | 256–512 Go SSD | Teams/Zoom + Edge consomment |
| Développement / CAO | 16–32 Go | 512 Go–1 To NVMe | Conteneurs, VMs locales |
| Postes partagés / kiosque | 8 Go | 256 Go SSD | Profils multiples |

> 💡 **Le 128 Go est un piège** : entre Windows (~30 Go), WinSxS, les points de restauration, le cache des MàJ et un profil utilisateur, on sature en moins d'un an. Standardisez sur 256 Go minimum.

---

## 3. Vérifier la compatibilité : PC Health Check, Get-TPM, scripts d'inventaire

### 3.1 PC Health Check (poste par poste)

L'outil officiel « Contrôle d'intégrité du PC » (`PC Health Check`) donne un verdict binaire. Utile pour un parc de quelques dizaines de postes, insuffisant à l'échelle.

### 3.2 Script d'inventaire de compatibilité (le vrai outil à l'échelle)

À exécuter via GPO (script de démarrage), SCCM/MECM, Intune (script PowerShell) ou en remote :

```powershell
<#
.SYNOPSIS
    Audit de compatibilité Windows 11 — à exécuter en administrateur.
    Exporte un CSV par machine vers un partage réseau.
#>
$ErrorActionPreference = 'SilentlyContinue'
$report = [ordered]@{}

$report.Hostname   = $env:COMPUTERNAME
$report.OS         = (Get-CimInstance Win32_OperatingSystem).Caption
$report.Build      = (Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion").CurrentBuild

# CPU
$cpu = Get-CimInstance Win32_Processor
$report.CPU = $cpu.Name.Trim()

# RAM (Go)
$report.RAM_GB = [math]::Round((Get-CimInstance Win32_ComputerSystem).TotalPhysicalMemory / 1GB, 1)

# Disque système (Go libres / taille)
$sys = Get-CimInstance Win32_LogicalDisk -Filter "DeviceID='C:'"
$report.DiskFree_GB  = [math]::Round($sys.FreeSpace / 1GB, 1)
$report.DiskSize_GB  = [math]::Round($sys.Size / 1GB, 1)

# TPM
$tpm = Get-Tpm
$report.TPM_Present = $tpm.TpmPresent
$report.TPM_Ready   = $tpm.TpmReady
$report.TPM_Enabled = $tpm.TpmEnabled
try {
    $spec = (Get-CimInstance -Namespace root/cimv2/security/microsofttpm -ClassName Win32_Tpm).SpecVersion
    $report.TPM_Version = ($spec -split ',')[0].Trim()
} catch { $report.TPM_Version = 'Inconnu' }

# Secure Boot
try { $report.SecureBoot = Confirm-SecureBootUEFI }
catch { $report.SecureBoot = 'Non supporté / BIOS Legacy' }

# Type de firmware
$report.Firmware = if (Test-Path "HKLM:\SYSTEM\CurrentControlSet\Control\SecureBoot\State") { 'UEFI' } else { 'Legacy probable' }

# Verdict
$report.Compatible = ($report.TPM_Version -eq '2.0' -and $report.SecureBoot -eq $true -and $report.RAM_GB -ge 4 -and $report.DiskSize_GB -ge 64)

$out = "\\SRV-FICHIERS\Inventaire$\Win11Compat_$($env:COMPUTERNAME).csv"
[pscustomobject]$report | Export-Csv -Path $out -NoTypeInformation -Encoding UTF8
Write-Output "Rapport écrit : $out — Compatible = $($report.Compatible)"
```

> ⚠️ Adaptez le chemin UNC `\\SRV-FICHIERS\Inventaire$` à votre infrastructure et donnez aux comptes machines le droit d'écriture sur le partage.

### 3.3 Inventaire à distance (WinRM)

```powershell
# Lister les postes d'une OU et auditer la compatibilité à distance
$computers = Get-ADComputer -Filter * -SearchBase "OU=Postes,DC=entreprise,DC=local" |
             Select-Object -ExpandProperty Name

Invoke-Command -ComputerName $computers -ScriptBlock {
    [pscustomobject]@{
        Hostname    = $env:COMPUTERNAME
        TPM         = (Get-Tpm).TpmPresent
        TPMVersion  = ((Get-CimInstance -Namespace root/cimv2/security/microsofttpm -ClassName Win32_Tpm).SpecVersion -split ',')[0]
        SecureBoot  = try { Confirm-SecureBootUEFI } catch { 'N/A' }
        RAM_GB      = [math]::Round((Get-CimInstance Win32_ComputerSystem).TotalPhysicalMemory / 1GB, 1)
    }
} | Export-Csv "C:\Admin\CompatWin11.csv" -NoTypeInformation -Encoding UTF8
```

### 3.4 Cas particulier : activer le TPM / passer en UEFI

| Situation | Action | Risque |
|---|---|---|
| TPM désactivé dans l'UEFI (Intel PTT / AMD fTPM off) | Activer dans le BIOS, souvent via outil constructeur (ex. Dell Command, Lenovo Thin Installer) | Nul |
| Disque en MBR / BIOS Legacy | Convertir avec `MBR2GPT.exe` avant migration | Sauvegarder avant ; échec possible si partitions non standard |
| TPM 1.2 uniquement | Remplacement ou contournement non supporté | À traiter comme poste non migrable |

```powershell
# Conversion MBR -> GPT sans perte de données (depuis Windows 10)
# 1. Valider d'abord :
MBR2GPT.EXE /validate /allowFullOS
# 2. Convertir :
MBR2GPT.EXE /convert /allowFullOS
# Redémarrer, passer le firmware en UEFI, activer Secure Boot.
```

---

## 4. Éditions Windows 11 : Pro vs Entreprise, canaux et cycle de vie

### 4.1 Comparatif Pro / Entreprise (ce qui change vraiment)

| Fonctionnalité | Pro | Entreprise |
|---|---|---|
| Domaine AD / Entra join | ✅ | ✅ |
| BitLocker | ✅ | ✅ |
| GPO (gpedit) | ✅ | ✅ |
| Hyper-V, Sandbox | ✅ | ✅ |
| Windows Update for Business | ✅ | ✅ |
| AppLocker (applocker CSP complet) | Partiel | ✅ |
| Credential Guard (stratégie native) | ❌ (registre possible) | ✅ |
| DirectAccess | ❌ | ✅ (déprécié — préférer Always On VPN) |
| App-V / UE-V | ❌ | ✅ (dépréciés — voir FSLogix) |
| Activation | Clé retail/OEM/MAK/KMS | KMS, Active Directory-Based Activation, abonnement M365 |
| Cycle de vie (feature update) | 24 mois | 36 mois |

**Recommandation** : standardisez sur **Entreprise** dès que le parc dépasse ~50 postes ou que la conformité l'exige (36 mois de support = 1 an de marge de migration en plus). Le surcoût se justifie par AppLocker/Credential Guard natifs et le cycle allongé.

### 4.2 Canaux de mise à jour (canaux de maintenance)

| Canal | Cible | Fréquence feature updates | Support |
|---|---|---|---|
| General Availability Channel | Tous | 1/an (H2, ex. 24H2) | Pro 24 mois / Entreprise 36 mois |
| Windows Insider (Canary/Dev/Beta/Release Preview) | Tests uniquement | Continu | Aucun support prod |

> ⛔ **Ne jamais** inscrire un poste de production au programme Insider. Utilisez un petit lot de pilotes/testeurs sur le canal **Release Preview** pour valider les MàJ en avance.

### 4.3 Cycle de vie — dates clés à connaître

| Version | Build | Fin de support Pro | Fin de support Entreprise |
|---|---|---|---|
| 21H2 | 22000 | 10/10/2023 | 08/10/2024 |
| 22H2 | 22621 | 08/10/2024 | 14/10/2025 |
| 23H2 | 22631 | 11/11/2025 | 10/11/2026 |
| 24H2 | 26100 | 13/10/2026 | 12/10/2027 |
| 25H2 | 26200 | (à confirmer) | (à confirmer) |

> 📌 **Lecture** : au 27/09/2026, la 23H2 Pro est déjà hors support et la 23H2 Entreprise se termine le 10/11/2026. **La cible de déploiement en 2026 est 24H2** (ou 25H2 si validée). Vérifiez toujours les dates sur le site officiel du cycle de vie Microsoft, elles peuvent évoluer.

```powershell
# Version installée sur un poste
Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion, OsBuildNumber
# ou via le registre :
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion" |
    Select-Object ProductName, DisplayVersion, CurrentBuild, UBR
```

---

## 5. Obtenir les médias : ISO, MCT, VLSC, canaux de mise à jour

| Source | Usage | Remarque |
|---|---|---|
| Media Creation Tool (MCT) | Clé USB / ISO grand public | Éditions Famille/Pro ; pas de VL |
| Page de téléchargement Microsoft (ISO direct) | ISO Pro | Simple, sans VL |
| VLSC (Volume Licensing Service Center) | ISO Entreprise VL | **La source officielle en entreprise** |
| Microsoft 365 admin center | ISO Entreprise (abonnement) | Si licences via M365 |
| Visual Studio Subscriptions | ISO toutes éditions | Pour lab/test |
| Windows Update / WUfB | Mise à niveau | Pas d'ISO, déploiement réseau |

```powershell
# Vérifier l'intégrité d'une ISO téléchargée (SHA-256 publié par Microsoft)
Get-FileHash C:\ISO\Win11_24H2_French_x64.iso -Algorithm SHA256
```

> 💡 **Montez toujours vos images de référence depuis l'ISO VLSC du mois en cours** (ISO « mise à jour » mensuelle) plutôt que de patcher une vieille ISO : vous gagnez le temps d'installation des MàJ cumulatives.

### Éditions contenues dans l'ISO : vérifier l'index

```powershell
# Lister les éditions dans install.wim / install.esd
dism /Get-WimInfo /WimFile:E:\sources\install.wim
# ou :
Get-WindowsImage -ImagePath E:\sources\install.wim
```

---

## 6. Installation propre : clé USB bootable, étapes, pilotes

### 6.1 Créer la clé USB (méthode supportée)

1. Media Creation Tool → « Créer un support d'installation ».
2. Ou en ligne de commande avec `diskpart` + copie des fichiers (clé en FAT32 pour boot UEFI ; si `install.wim` > 4 Go, splitter) :

```powershell
# Préparer la clé (ATTENTION : remplacez N par le bon numéro de disque !)
diskpart
list disk
select disk N
clean
convert gpt
create partition primary
format fs=fat32 quick label="WIN11"
assign
exit
```

```powershell
# Si install.wim dépasse 4 Go (limite FAT32), le scinder :
dism /Split-Image /ImageFile:C:\ISO\sources\install.wim /SWMFile:E:\sources\install.swm /FileSize:3800
```

Alternative moderne : **Rufus** (utile pour forcer certains paramètres en lab, mais en entreprise préférez la méthode Microsoft + unattend.xml).

### 6.2 Étapes d'installation propre

1. Boot UEFI sur la clé (désactiver temporairement Secure Boot n'est **pas** nécessaire avec un média officiel signé).
2. Langue → « Installer maintenant ».
3. Clé de produit : saisir ou « Je n'ai pas de clé » (l'activation se fera après, via KMS/ADBA/Entra).
4. Choisir l'édition correspondant à votre licence.
5. Type : **Personnalisé** → supprimer/recréer les partitions (sauf cas de double boot).
6. Laisser l'installation se dérouler, puis OOBE.

### 6.3 Pilotes : ordre d'installation recommandé

1. Chipset (Intel Chipset Device Software / AMD Chipset Drivers)
2. Stockage (NVMe / RAID / RST)
3. Réseau (Ethernet puis Wi-Fi)
4. Graphiques (Intel/AMD/NVIDIA — version **DCH**)
5. Audio
6. Périphériques (lecteur d'empreinte, etc.)
7. Utilitaires constructeur (Dell Command Update, Lenovo System Update…)

```powershell
# Exporter les pilotes d'un poste de référence (pour MDT, section 11)
Export-WindowsDriver -Online -Destination C:\Drivers\Export-Ref

# Installer un pilote en ligne de commande
pnputil /add-driver C:\Drivers\*.inf /subdirs /install
```

---

## 7. Mise à niveau depuis Windows 10 : chemins, outils, pièges

### 7.1 Chemins de mise à niveau supportés

| Source | Cible | Méthode |
|---|---|---|
| Windows 10 22H2 Pro/Entreprise | Windows 11 24H2 même édition | Windows Update, ISO (setup.exe), WUfB, MDT (refresh), Intune (feature update) |
| Windows 10 Famille | Windows 11 Pro | Changement de clé + MàJ |

> ⚠️ **Pas de mise à niveau** 32 bits → 64 bits, ni d'une langue vers une autre sans réinstallation. Et Windows 10 doit être en 22H2 avec les dernières MàJ cumulatives.

### 7.2 Lancement via setup.exe (ISO montée)

```powershell
# Mise à niveau silencieuse conservant fichiers et applications
E:\setup.exe /auto upgrade /quiet /noreboot /showoobe none /compat ignorewarning

# Options utiles :
# /auto upgrade            : tout conserver
# /quiet                  : sans interface
# /noreboot               : ne pas redémarrer (planifier le reboot)
# /dynamicupdate enable   : télécharger les dernières MàJ pendant l'install
# /compat ignorewarning   : ignorer les avertissements (pas les erreurs bloquantes)
```

### 7.3 Pièges classiques de la mise à niveau

| Piège | Symptôme | Parade |
|---|---|---|
| Espace disque insuffisant | Échec à ~30 %, code 0x80070070 | 20 Go libres minimum ; nettoyer avec cleanmgr |
| Pilote incompatible | Rollback, 0xC1900101 | Mettre à jour BIOS/pilotes avant ; débrancher les périphériques USB non essentiels |
| Antivirus tiers | Blocage setup | Désinstaller (pas seulement désactiver) avant |
| Chiffrement tiers | Échec | Suspendre/déchiffrer avant |
| Applications incompatibles | Avertissement compat | Tester sur lot pilote ; utiliser Upgrade Readiness |
| TPM désactivé | « Ce PC ne peut pas exécuter Windows 11 » | Activer Intel PTT / AMD fTPM dans l'UEFI |

### 7.4 Journaux de mise à niveau (où chercher quand ça échoue)

| Fichier | Rôle |
|---|---|
| `C:\$Windows.~BT\Sources\Panther\setupact.log` | Journal principal du setup |
| `C:\$Windows.~BT\Sources\Panther\setuperr.log` | Erreurs uniquement |
| `C:\$Windows.~BT\Sources\Rollback\setupact.log` | En cas de rollback |
| `C:\Windows\Logs\MoSetup\BlueBox.log` | Lancement via Windows Update |
| `setupdiag.exe` (outil Microsoft) | Diagnostic automatisé : `SetupDiag.exe /Output:C:\Logs\result.xml` |

```powershell
# Télécharger et lancer SetupDiag (nécessite .NET)
# https://aka.ms/SetupDiag
.\SetupDiag.exe /Output:C:\Admin\SetupDiag_Results.log
```

---

## 8. Installation automatisée : unattend.xml, clés de produit, setupconfig

### 8.1 Principe des passes d'installation

Le fichier `unattend.xml` pilote le setup via des « passes » : `windowsPE`, `offlineServicing`, `specialize`, `oobeSystem`, etc. L'outil graphique officiel est le **Windows System Image Manager (WSIM)**, inclus dans l'ADK.

### 8.2 Exemple unattend.xml minimal (lab — à adapter)

```xml
<?xml version="1.0" encoding="utf-8"?>
<unattend xmlns="urn:schemas-microsoft-com:unattend">
  <settings pass="windowsPE">
    <component name="Microsoft-Windows-International-Core-WinPE"
               processorArchitecture="amd64"
               publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
      <SetupUILanguage><UILanguage>fr-FR</UILanguage></SetupUILanguage>
      <InputLocale>040c:0000040c</InputLocale>
      <SystemLocale>fr-FR</SystemLocale>
      <UILanguage>fr-FR</UILanguage>
      <UserLocale>fr-FR</UserLocale>
    </component>
    <component name="Microsoft-Windows-Setup"
               processorArchitecture="amd64"
               publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
      <UserData>
        <ProductKey>
          <!-- Clé générique d'installation (GVLK) — N'ACTIVE PAS, sert au setup -->
          <Key>XXXXX-XXXXX-XXXXX-XXXXX-XXXXX</Key>
          <WillShowUI>Never</WillShowUI>
        </ProductKey>
        <AcceptEula>true</AcceptEula>
      </UserData>
    </component>
  </settings>
  <settings pass="oobeSystem">
    <component name="Microsoft-Windows-Shell-Setup"
               processorArchitecture="amd64"
               publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
      <OOBE>
        <HideEULAPage>true</HideEULAPage>
        <HideOEMRegistrationScreen>true</HideOEMRegistrationScreen>
        <HideOnlineAccountScreens>true</HideOnlineAccountScreens>
        <HideWirelessSetupInOOBE>true</HideWirelessSetupInOOBE>
        <ProtectYourPC>3</ProtectYourPC>
      </OOBE>
      <UserAccounts>
        <LocalAccounts>
          <LocalAccount wcm:action="add">
            <Name>tech</Name>
            <Group>Administrators</Group>
            <Password><Value>U2VjcmV0UGFzc3dvcmQxMjM=</Value><PlainText>false</PlainText></Password>
          </LocalAccount>
        </LocalAccounts>
      </UserAccounts>
    </component>
  </settings>
</unattend>
```

> ⚠️ Le mot de passe ci-dessus est un exemple encodé en base64 (`U2VjcmV0UGFzc3dvcmQxMjM=` = « SecretPassword123 » fictif). **Ne commitez jamais un unattend.xml contenant un vrai mot de passe** dans un dépôt : préférez créer le compte admin via une task sequence MDT ou une stratégie LAPS (section 34).

### 8.3 Clés de produit : les 4 modèles

| Modèle | Usage | Remarque |
|---|---|---|
| OEM | PC livrés avec Windows | Liée au firmware (OA3), réinstallation auto |
| Retail | Boîte / Microsoft Store | Transférable |
| MAK (Multiple Activation Key) | Volume, activation en ligne | Nombre d'activations limité, à réserver aux postes isolés |
| KMS / GVLK | Volume, activation locale | **Standard entreprise** : serveur KMS ou ADBA |

```powershell
# Installer une clé générique de volume (GVLK) — exemple fictif de format
slmgr.vbs /ipk XXXXX-XXXXX-XXXXX-XXXXX-XXXXX
# Pointer vers le KMS interne
slmgr.vbs /skms kms.entreprise.local:1688
slmgr.vbs /ato
# Vérifier
slmgr.vbs /dlv
```

```powershell
# Activation basée sur Active Directory (ADBA) — côté serveur, une fois :
# 1. Installer le rôle "Services d'activation en volume" sur un DC/serveur
# 2. slmgr.vbs /ipk <clé hôte KMS> puis /ato
# Côté client : rien à faire, l'activation est automatique à la jointure du domaine.
```

### 8.4 setupconfig.ini (alternative légère à unattend.xml)

Pour une mise à niveau pilotée sans XML complet :

```ini
[SetupConfig]
ShowOobe=None
Telemetry=Disable
DynamicUpdate=Enable
Compat=IgnoreWarning
PostOobe=C:\Admin\post-install.cmd
```

À placer à côté de `setup.exe` ou dans `C:\Users\Default\AppData\Local\Microsoft\Windows\WSUS\SetupConfig.ini`.

---

## 9. Sysprep : généralisation, audit mode, capture d'image

### 9.1 Les 3 modes de Sysprep

| Mode | Commande | Usage |
|---|---|---|
| OOBE + Generalize | `sysprep /generalize /oobe /shutdown` | **Image maître** : nettoie les SID, prépare la capture |
| Audit | `sysprep /audit /reboot` | Personnalisation en mode audit (compte Administrator intégré) |
| OOBE simple | `sysprep /oobe /shutdown` | Sans généralisation (même machine) |

```powershell
# La commande de référence avant capture :
C:\Windows\System32\Sysprep\Sysprep.exe /generalize /oobe /shutdown /unattend:C:\Admin\unattend.xml
```

> ⛔ **Limites impératives** :
> - Sysprep **échoue** si des applications du Store ont été mises à jour pour un utilisateur (erreur `0x80073cf2`). Désinstaller ou ne pas lancer le Store avant sysprep.
> - Maximum **3** réarmements (`slmgr /rearm`) — au-delà, sysprep refuse.
> - Ne jamais sysprepper une machine déjà en production ou jointe au domaine pour en faire une image (problèmes de SID/relations d'approbation).

### 9.2 Mode audit pas à pas

1. Sur le poste de référence, à l'écran OOBE : **Ctrl+Shift+F3** → redémarre en mode audit (compte Administrator).
2. Installer applications, pilotes, personnaliser le profil par défaut si besoin.
3. Nettoyer : vider les journaux, supprimer les fichiers temporaires.
4. `sysprep /generalize /oobe /shutdown`.
5. Capturer (section suivante).

```powershell
# En mode audit : copier un profil personnalisé vers le profil par défaut
# (via unattend.xml, passe specialize) :
# <CopyProfile>true</CopyProfile>
# ATTENTION : CopyProfile est déprécié dans ses effets sur le menu Démarrer
# moderne — préférez les GPO de personnalisation (section 22).
```

### 9.3 Capture de l'image avec DISM

Depuis WinPE (clé bootable) ou un autre OS :

```powershell
# Capturer C: vers un WIM
dism /Capture-Image /ImageFile:D:\Images\Win11-Ref.wim /CaptureDir:C:\ /Name:"Win11 24H2 Ref" /Compress:max /CheckIntegrity /Verify

# Vérifier le contenu
dism /Get-WimInfo /WimFile:D:\Images\Win11-Ref.wim

# Appliquer l'image sur un poste cible (depuis WinPE)
diskpart /s C:\Admin\partitions-uefi.txt
dism /Apply-Image /ImageFile:D:\Images\Win11-Ref.wim /Index:1 /ApplyDir:C:\
bcdboot C:\Windows
```

Fichier `partitions-uefi.txt` type (GPT/UEFI) :

```
select disk 0
clean
convert gpt
create partition efi size=100
format quick fs=fat32 label="System"
assign letter="S"
create partition msr size=16
create partition primary
format quick fs=ntfs label="Windows"
assign letter="C"
exit
```

---

## 10. MDT : installation, deployment share, task sequences

### 10.1 Prérequis et installation

| Composant | Version / remarque |
|---|---|
| Windows ADK | Version correspondant à Windows 11 (ex. ADK pour 24H2) |
| WinPE Add-on pour l'ADK | **Obligatoire** (boot images) |
| MDT | Dernière build (8456+) |
| Serveur | Windows Server 2019/2022/2025, partage SMB |

Ordre d'installation : **ADK → WinPE Add-on → MDT**. Puis console « Deployment Workbench ».

```powershell
# Partage de déploiement : créer le dossier et le partager
New-Item -Path D:\DeploymentShare -ItemType Directory
New-SmbShare -Name "DeploymentShare$" -Path D:\DeploymentShare -FullAccess "Administrateurs"
# (Affiner les ACL : lecture pour "Utilisateurs authentifiés", écriture pour le compte MDT)
```

### 10.2 Créer le deployment share

1. Deployment Workbench → clic droit « Deployment Shares » → New.
2. Chemin : `D:\DeploymentShare`, nom de partage `DeploymentShare$`.
3. Options : cocher les plateformes x64, désactiver x86 (sauf besoin legacy).
4. Le partage génère les images WinPE (`LiteTouchPE_x64.wim`) dans `Boot\`.

```powershell
# Mettre à jour le deployment share après chaque modification (régénère WinPE)
# Via la console : clic droit → "Update Deployment Share" → "Completely regenerate"
```

### 10.3 Importer l'OS

Deployment Workbench → Operating Systems → Import :

- **Full set of source files** : depuis l'ISO VLSC extraite (recommandé).
- **Custom image file** : votre WIM capturé (section 9).
- **WIM existant** du deployment share.

### 10.4 Task sequence « Standard Client »

Clic droit Task Sequences → New → **Standard Client Task Sequence** :

1. ID : `WIN11-24H2`, nom : « Windows 11 24H2 Entreprise ».
2. OS : sélectionner l'image importée.
3. Clé produit : ne rien saisir (activation via KMS/ADBA après) ou GVLK.
4. Compte admin local : définir un mot de passe **fictif/temporaire** (sera écrasé par LAPS ensuite).
5. Étapes par défaut : validation, formatage, application de l'image, pilotes, Windows Update, applications.

Structure d'une task sequence (onglet Task Sequence) :

```
Initialization
  └─ Gather local only
Validation
  └─ Validate (BIOS/UEFI, mémoire...)
State Capture (refresh uniquement)
Preinstall
  ├─ New Computer only → Format and Partition Disk (UEFI)
  └─ Apply Operating System
Postinstall
  ├─ Inject Drivers
  ├─ Apply Windows PE / Apply Network Settings
  └─ Apply Local GPO Package (optionnel)
State Restore
  ├─ Windows Update (Pre-Application / Post-Application)
  ├─ Install Applications
  └─ Tattoo (écrit les infos de déploiement dans le registre)
```

---

## 11. MDT : Lite Touch, pilotes, règles CustomSettings.ini

### 11.1 Boot Lite Touch

1. Copier `D:\DeploymentShare\Boot\LiteTouchPE_x64.wim` vers votre serveur WDS, ou créer une clé USB bootable (`LiteTouchPE_x64.iso`).
2. Au boot : WinPE → assistant Lite Touch → choix de la task sequence → identifiants (`DOMAIN\svc-mdt`) → déploiement.

Avec WDS : ajouter l'image de démarrage, configurer DHCP (options 66/67) ou IP Helper vers le serveur WDS.

### 11.2 Gestion des pilotes (par modèle)

Bonne pratique : **un dossier par modèle**, sélection par WMI :

```
Out-of-Box Drivers
 └─ Win11-x64
     ├─ Dell-Latitude-5440
     ├─ HP-EliteBook-840-G10
     └─ Lenovo-ThinkPad-T14-G4
```

Dans la task sequence, étape « Inject Drivers » → « Install all drivers from the selection profile » avec un **profil de sélection** par modèle, ou « Install only matching drivers from the selection profile ».

```powershell
# Importer des pilotes en masse dans MDT (à exécuter sur le serveur)
$drivers = "D:\DriversSCCM\Dell-Latitude-5440\Win11"
Import-MDTDriver -Path "DS001:\Out-of-Box Drivers\Win11-x64\Dell-Latitude-5440" -SourcePath $drivers -ImportDuplicates $false
```

```powershell
# Identifier le modèle exact (pour nommer le dossier)
Get-CimInstance Win32_ComputerSystem | Select-Object Manufacturer, Model
```

> ⚠️ **Ne jamais** injecter « tous les pilotes pour tout le monde » : conflits, BSOD, task sequences interminables. Un profil de sélection par modèle + requête WMI, c'est la règle.

### 11.3 CustomSettings.ini — le fichier qui automatise tout

`D:\DeploymentShare\Control\CustomSettings.ini` :

```ini
[Settings]
Priority=Model, Default
Properties=MyCustomProperty

[Dell Inc.-Latitude 5440]
DriverGroup001=Dell-Latitude-5440

[HP-EliteBook 840 G10]
DriverGroup001=HP-EliteBook-840-G10

[Default]
OSInstall=Y
SkipCapture=YES
SkipAdminPassword=YES
SkipProductKey=YES
SkipComputerBackup=YES
SkipBitLocker=NO
SkipLocaleSelection=YES
SkipTimeZone=YES
SkipApplications=NO
SkipSummary=NO
SkipFinalSummary=YES
KeyboardLocale=fr-FR
UserLocale=fr-FR
UILanguage=fr-FR
TimeZoneName=Romance Standard Time
; Romance Standard Time = fuseau Paris (UTC+1, gère l'heure d'été)
JoinDomain=entreprise.local
DomainAdmin=svc-join-domain
DomainAdminDomain=ENTREPRISE
; Le mot de passe du compte de jointure va dans Bootstrap.ini (chiffré si possible)
MachineObjectOU=OU=Postes Win11,OU=Ordinateurs,DC=entreprise,DC=local
SLShare=\\SRV-MDT\Logs$
EventService=http://SRV-MDT:9800
```

`Bootstrap.ini` (`D:\DeploymentShare\Control\Bootstrap.ini`) :

```ini
[Settings]
Priority=Default

[Default]
DeployRoot=\\SRV-MDT\DeploymentShare$
UserDomain=ENTREPRISE
UserID=svc-mdt
UserPassword=MotDePasseFictif_A_Remplacer
SkipBDDWelcome=YES
KeyboardLocale=fr-FR
```

> 🔐 `svc-mdt` et `svc-join-domain` : comptes de service dédiés, mots de passe complexes, **jamais** de compte admin du domaine. Le compte de jointure doit avoir uniquement le droit « Joindre des ordinateurs au domaine » délégué sur l'OU cible.

### 11.4 Applications dans MDT

Applications → New Application → « Application with source files » ou « Application bundle » :

```powershell
# Exemple : installation silencieuse 7-Zip (commande dans l'application MDT)
# Working directory : .\Applications\7-Zip
# Command line : 7z2408-x64.exe /S

# Exemple : winget en task sequence (section 28)
# Command line : powershell.exe -ExecutionPolicy Bypass -File Install-Apps.ps1
```

### 11.5 Journaux MDT (dépannage)

| Emplacement | Contenu |
|---|---|
| `C:\MININT\SMSOSD\OSDLOGS\BDD.log` | Journal agrégé (le plus utile) |
| `C:\MININT\SMSOSD\OSDLOGS\SMSTS.log` | Task sequence engine |
| `\\SRV-MDT\Logs$` (SLShare) | Copie réseau en temps réel |
| `X:\MININT\...` (WinPE) | Pendant la phase WinPE |

---

## 12. Autopilot + Intune : principe, profils, inscription

### 12.1 Principe (ce qui change par rapport à MDT)

| MDT / image | Autopilot |
|---|---|
| Image maître à maintenir | **Aucune image** : l'OEM livre Windows 11, Autopilot transforme |
| Réseau local, PXE | Cloud, l'utilisateur déballe et allume |
| Jointure AD classique | Entra join (+ hybride possible) |
| Applications via task sequence | Intune (applications, stratégies, conformité) |

Flux Autopilot :

```
1. Le fournisseur enregistre le hardware hash dans Autopilot (ou import CSV manuel)
2. L'appareil est affecté à un profil Autopilot (groupe Entra)
3. L'utilisateur allume → OOBE → connexion Entra ID
4. Intune pousse : stratégies, certificats, applications, conformité
5. L'utilisateur arrive sur un poste configuré (User-Driven ou Self-Deploying)
```

### 12.2 Enregistrer un appareil (import manuel)

```powershell
# Sur le poste (en OOBE : Shift+F10 pour ouvrir une invite)
# Récupérer le script officiel :
Install-Script -Name Get-WindowsAutopilotInfo -Force
# Générer le CSV :
Get-WindowsAutopilotInfo.ps1 -OutputFile C:\Temp\autopilot.csv
# Importer le CSV dans Intune → Appareils → Inscription → Windows Autopilot
```

Format CSV attendu : `Device Serial Number, Windows Product ID, Hardware Hash, Group Tag, Assigned User`.

### 12.3 Profils de déploiement Autopilot (réglages clés)

Dans Intune → Appareils → Inscription → Profils de déploiement :

| Paramètre | Recommandé entreprise |
|---|---|
| Mode de déploiement | Piloté par l'utilisateur (standard) |
| Joindre à | Microsoft Entra ID (pur cloud) ou Hybride |
| Type de compte | Standard (jamais admin local pour l'utilisateur) |
| Langue / clavier | fr-FR |
| Nom d'appareil | Modèle : `PC-%SERIAL%` ou `ENT-%RAND:6%` |
| Masquer les écrans de confidentialité | Oui |
| Masquer le changement de compte | Oui |

### 12.4 Page d'état d'inscription (ESP)

L'ESP bloque l'accès au bureau tant que les éléments critiques ne sont pas installés :

- Applications « bloquantes » (ex. : Defender config, VPN, Office) ;
- Stratégies de sécurité ;
- Certificats.

> 💡 Ne mettez en bloquant **que** le strict nécessaire : chaque application bloquante allonge l'OOBE et chaque échec bloque l'utilisateur. Le reste s'installe en arrière-plan après l'ouverture de session.

### 12.5 Groupes dynamiques Entra (affectation automatique)

```powershell
# Exemple de règle d'appartenance dynamique pour les appareils Autopilot :
# (device.devicePhysicalIDs -any (_ -contains "[ZTDId]"))
# Tous les appareils Autopilot → groupe "GRP-Autopilot-All"

# Par profil (Group Tag) :
# (device.devicePhysicalIDs -any (_ -eq "[OrderID]:Compta"))
```

---

## 13. Autopilot Reset, Fresh Start et scénarios de réaffectation

### 13.1 Comparatif des méthodes de réinitialisation

| Méthode | Conserve l'inscription Entra/Autopilot | Conserve les données | Usage |
|---|---|---|---|
| Autopilot Reset | ✅ (réinscription auto) | ❌ | Réaffectation à un nouvel utilisateur |
| Réinitialiser ce PC (paramètres) | ❌ | Au choix | Poste hors gestion |
| Fresh Start (Intune) | ❌ (réinscription) | ❌ | Repartir de zéro, supprime les apps préinstallées |
| Reimage MDT | ❌ | ❌ | Retour au socle MDT |

### 13.2 Déclencher un Autopilot Reset

```powershell
# Méthode 1 : depuis Intune (Appareils → ... → Autopilot Reset)
# Méthode 2 : en local (admin)
systemreset.exe --factoryreset
# puis choisir "Autopilot Reset" si proposé

# Méthode 3 : à distance via Intune (action groupée sur N appareils)
```

Processus détaillé :

```
1. L'appareil reçoit la commande (Intune ou locale)
2. Redémarrage → suppression des applications, paramètres, données utilisateurs
3. L'inscription Entra ID et l'enregistrement Autopilot sont CONSERVÉS
4. OOBE Autopilot → nouvel utilisateur se connecte
5. Intune réapplique profils + applications
Durée typique : 15 à 30 minutes.
```

### 13.3 Scénario de réaffectation (procédure type)

1. Sauvegarder les données de l'ancien utilisateur (OneDrive Known Folder Move = automatique).
2. Révoquer les sessions (Entra → Révoquer les sessions) si départ sensible.
3. Autopilot Reset via Intune.
4. Affecter le nouvel utilisateur dans le profil Autopilot.
5. Le nouvel utilisateur allume et se connecte.

---

## 14. Jointure au domaine vs Entra join vs hybride

### 14.1 Tableau de décision

| Critère | AD joint (classique) | Entra join (pur cloud) | Hybride (Hybrid Entra join) |
|---|---|---|---|
| Prérequis réseau | Ligne vers les DC | Internet | Les deux |
| Gestion | GPO | Intune | GPO + Intune (co-management) |
| SSO applications cloud | Via AD FS / Seamless SSO | Natif (PRT) | Natif après sync |
| Ressources locales (partages, impression) | Natif (Kerberos) | Via VPN/Cloud (Kerberos Cloud Trust) | Natif |
| Idéal pour | Parc sédentaire existant | Nomades, nouveaux parcs, filiales sans DC | Transition, contraintes locales fortes |

**Recommandation 2026** : pour un nouveau déploiement, visez **Entra join + Intune**. L'hybride est un état transitoire : il cumule les complexités des deux mondes (double inscription, délais de sync, dépannage plus difficile).

### 14.2 Jointure au domaine (classique)

```powershell
# Joindre le domaine en PowerShell
Add-Computer -DomainName "entreprise.local" -OUPath "OU=Postes Win11,DC=entreprise,DC=local" -Credential (Get-Credential) -Restart

# Vérifier
Get-CimInstance Win32_ComputerSystem | Select-Object PartOfDomain, Domain

# Quitter le domaine (avant réaffectation)
Remove-Computer -UnjoinDomainCredential (Get-Credential) -Restart
```

### 14.3 Entra join (manuel, hors Autopilot)

Paramètres → Comptes → Accès professionnel ou scolaire → Se connecter → « Joindre cet appareil à Microsoft Entra ID ».

```powershell
# Vérifier l'état d'inscription Entra
dsregcmd /status
# Lignes clés :
#   AzureAdJoined : YES
#   EnterpriseJoined : NO
#   DomainJoined : NO (pur cloud) / YES (hybride)
```

### 14.4 Hybride : points de vigilance

1. **Microsoft Entra Connect** doit synchroniser les objets ordinateurs.
2. Le SCP (Service Connection Point) dans AD indique au poste où s'enregistrer.
3. Délai : l'enregistrement hybride se fait au redémarrage suivant la sync (jusqu'à 30 min).
4. Dépannage : `dsregcmd /status`, journaux `Applications and Services Logs\Microsoft\Windows\User Device Registration`.

```powershell
# Forcer une tentative d'enregistrement (dépannage)
dsregcmd /join /debug
# Quitter proprement avant de recommencer
dsregcmd /leave
```

---

## 15. BitLocker : fondamentaux, TPM seul vs TPM+PIN, XTS-AES

### 15.1 Ce que BitLocker chiffre (et ce qu'il ne chiffre pas)

- ✅ Volume OS, volumes de données, disques amovibles (BitLocker To Go).
- ❌ Ne protège pas contre un OS compromis en cours d'exécution : c'est une protection **au repos** (vol/perte) et **au démarrage** (intégrité via TPM).

### 15.2 Protecteurs : TPM seul vs TPM+PIN vs clé USB

| Protecteur | Sécurité | Ergonomie | Recommandé pour |
|---|---|---|---|
| TPM seul | Bonne (lie le déchiffrement à l'intégrité du boot) | Transparente | Postes fixes en entreprise, standard |
| TPM + PIN | Forte (PIN = 2e facteur, anti-DMA/cold boot) | PIN à chaque démarrage | Portables sensibles, dirigeants, nomades |
| TPM + clé USB | Forte | Clé à brancher | Cas spécifiques (serveurs, postes sans clavier) |
| Mot de passe seul (sans TPM) | Moyenne | Mot de passe | Postes sans TPM (rare sous Win11) |

```powershell
# État BitLocker d'un poste
Get-BitLockerVolume | Select-Object MountPoint, VolumeStatus, ProtectionStatus, EncryptionMethod

# Détail des protecteurs
(Get-BitLockerVolume -MountPoint C:).KeyProtector | Select-Object KeyProtectorType, KeyProtectorId
```

### 15.3 Algorithmes : XTS-AES 128 vs 256

| Algorithme | Vitesse | Sécurité | Recommandation |
|---|---|---|---|
| XTS-AES 128 | + rapide | Suffisante (standard) | **Défaut recommandé** |
| XTS-AES 256 | ~10-20 % plus lent | Marge théorique | Données très sensibles, exigences réglementaires |

> 💡 La différence pratique entre 128 et 256 bits est négligeable face aux autres risques. Standardisez sur **XTS-AES 128** sauf contrainte de conformité explicite. Le paramètre se règle par GPO avant le chiffrement (un changement après = déchiffrement/rechiffrement).

### 15.4 Activer BitLocker en ligne de commande

```powershell
# Chiffrement simple : TPM seul (le plus courant)
Enable-BitLocker -MountPoint "C:" -TpmProtector -UsedSpaceOnly -SkipHardwareTest

# TPM + PIN (le PIN est demandé de façon sécurisée)
Enable-BitLocker -MountPoint "C:" -TpmAndPinProtector -UsedSpaceOnly -SkipHardwareTest
# Puis définir le PIN :
# (Add-BitLockerKeyProtector -MountPoint "C:" -Pin (Read-Host -AsSecureString "PIN") -TpmAndPinProtector)

# Options :
# -UsedSpaceOnly        : ne chiffre que l'espace utilisé (rapide, suffisant sur disque neuf)
# -SkipHardwareTest     : pas de redémarrage de test (à n'utiliser qu'en déploiement maîtrisé)

# Vérifier la progression
Get-BitLockerVolume -MountPoint C: | Select-Object VolumeStatus, EncryptionPercentage
```

```powershell
# Suspendre temporairement (ex. : avant mise à jour du BIOS !)
Suspend-BitLocker -MountPoint "C:" -RebootCount 1

# Reprendre
Resume-BitLocker -MountPoint "C:"
```

> ⚠️ **Avant toute mise à jour du BIOS/UEFI ou changement matériel** : suspendez BitLocker (1 redémarrage). Sinon, le TPM détecte un changement d'intégrité → **écran de récupération** au prochain boot (section 18).

---

(Bloc 1/6 — sections 1 à 15)

---

## 16. BitLocker : clés de récupération (AD, fichier, impression, Entra)

### 16.1 Les 4 destinations de la clé de récupération (48 chiffres)

| Destination | Avantage | Inconvénient |
|---|---|---|
| **Active Directory** (attribut msFVE) | Centralisé, lié à l'objet ordinateur, ACL possibles | Nécessite le schéma étendu + GPO |
| **Microsoft Entra ID** | Visible dans Intune / myapps | Postes Entra join uniquement |
| **Fichier** (`C:\...`, clé USB) | Simple | À protéger (chiffrer le partage, ACL) |
| **Impression** | Hors-ligne | Papier = à coffre-fort |

**En entreprise : imposez AD et/ou Entra par GPO, jamais « au choix de l'utilisateur ».**

### 16.2 Sauvegarder la clé dans AD (prérequis)

1. Étendre le schéma AD (une fois par forêt) : les classes `msFVE-RecoveryInformation` existent depuis Server 2008 — vérifiez avec :

```powershell
# Vérifier que le schéma contient les attributs BitLocker
Get-ADObject -SearchBase (Get-ADRootDSE).schemaNamingContext -Filter { name -like "ms-FVE-*" } |
    Select-Object Name | Format-Table -AutoSize
```

2. GPO : `Configuration ordinateur\Stratégies\Modèles d'administration\Composants Windows\BitLocker\Lecteurs du système d'exploitation` → **« Choisir comment les lecteurs chiffrés par BitLocker peuvent être récupérés »** → cocher « Enregistrer les informations de récupération BitLocker dans les services de domaine Active Directory ».

3. Déléguer le droit d'écriture sur les objets ordinateurs (le poste écrit lui-même sa clé à l'activation).

### 16.3 Récupérer une clé depuis AD

```powershell
# Trouver la clé de récupération d'un poste (depuis une session admin du domaine)
$computer = "PC-COMPTA-042"
Get-ADObject -Filter "objectClass -eq 'msFVE-RecoveryInformation'" `
    -SearchBase (Get-ADComputer $computer).DistinguishedName `
    -Properties msFVE-RecoveryPassword, whenCreated |
    Sort-Object whenCreated -Descending |
    Select-Object -First 1 msFVE-RecoveryPassword, whenCreated
```

### 16.4 Sauvegarder / exporter manuellement

```powershell
# Sauvegarder les protecteurs vers AD (si la GPO ne l'a pas fait)
Backup-BitLockerKeyProtector -MountPoint "C:" -KeyProtectorId "{ID-du-protecteur}"

# Lister les ID de protecteurs
(Get-BitLockerVolume -MountPoint C:).KeyProtector

# Exporter la clé de récupération dans un fichier (à stocker de façon sécurisée !)
(Get-BitLockerVolume -MountPoint C:).KeyProtector |
    Where-Object { $_.KeyProtectorType -eq 'RecoveryPassword' } |
    Select-Object -ExpandProperty RecoveryPassword |
    Out-File "\\SRV-FICHIERS\BitLocker$\PC-042-recovery.txt"
```

### 16.5 Clé dans Entra ID / Intune

- Intune → Appareils → sélectionner l'appareil → **Clés de récupération** → afficher/copier.
- L'utilisateur peut aussi retrouver sa clé sur `myaccount.microsoft.com` (si la stratégie l'autorise).

> 🔐 **Procédure d'urgence** : quand un utilisateur appelle avec l'écran bleu de récupération, demandez-lui l'**ID de clé** affiché (8 premiers caractères), retrouvez la clé correspondante dans AD/Intune, dictez les 48 chiffres par blocs de 6. Ne jamais envoyer une clé de récupération par e-mail non chiffré.

---

## 17. BitLocker : stratégies GPO, MBAM / BitLocker Management, déploiement silencieux

### 17.1 GPO BitLocker — les réglages à standardiser

Emplacement : `Configuration ordinateur\Stratégies\Modèles d'administration\Composants Windows\BitLocker\`

| Stratégie | Réglage recommandé | Pourquoi |
|---|---|---|
| Choisir la méthode de chiffrement (lecteurs OS) | XTS-AES 128 | Standard, rapide (section 15) |
| Exiger un chiffrement de l'espace utilisé uniquement | Activé | Déploiement rapide sur disques neufs |
| Choisir comment les lecteurs peuvent être récupérés | Sauvegarde AD obligatoire | Pas de clé = pas de chiffrement |
| Configurer l'utilisation de mots de passe (lecteurs OS) | Selon politique PIN | — |
| Refuser l'écriture sur lecteurs amovibles non chiffrés | Activé (optionnel) | DLP basique via BitLocker To Go |
| Activer BitLocker sans TPM compatible | Selon parc | À éviter sous Win11 (TPM requis de toute façon) |

### 17.2 Déploiement silencieux via GPO / script de démarrage

```powershell
<#
.SYNOPSIS
    Script de démarrage GPO : chiffre C: si ce n'est pas déjà fait,
    avec sauvegarde de la clé dans AD (via la GPO de récupération).
#>
$vol = Get-BitLockerVolume -MountPoint "C:"
if ($vol.VolumeStatus -eq 'FullyDecrypted') {
    # Vérifier que le TPM est prêt avant de chiffrer
    $tpm = Get-Tpm
    if ($tpm.TpmReady) {
        Enable-BitLocker -MountPoint "C:" -TpmProtector -UsedSpaceOnly -SkipHardwareTest
        # La GPO "Choisir comment..." se charge de la sauvegarde AD
        Write-EventLog -LogName Application -Source "BitLockerDeploy" `
            -EventId 1001 -EntryType Information `
            -Message "Chiffrement BitLocker démarré sur $env:COMPUTERNAME"
    } else {
        Write-EventLog -LogName Application -Source "BitLockerDeploy" `
            -EventId 1002 -EntryType Warning `
            -Message "TPM non prêt sur $env:COMPUTERNAME — chiffrement reporté"
    }
}
```

> ⚠️ Créez la source `BitLockerDeploy` une fois (`New-EventLog -LogName Application -Source BitLockerDeploy`) ou le script échouera sur `Write-EventLog`.

### 17.3 MBAM → BitLocker Management (MECM / Intune)

- **MBAM** (Microsoft BitLocker Administration and Monitoring) : historique, lié à MDT/SCCM. **Fin de support** — ne plus déployer.
- **BitLocker Management dans MECM** (à partir de la version 2002) : le successeur. Stratégies de chiffrement + portail de récupération en libre-service + rapports de conformité.
- **Intune** : stratégies de chiffrement de disque (Endpoint Security → Disk encryption), clés visibles dans le portail, conformité.

| Fonction | GPO seule | MECM BitLocker Mgmt | Intune |
|---|---|---|---|
| Imposer le chiffrement | ✅ | ✅ | ✅ |
| Stocker les clés | AD | Base MECM (+ AD) | Entra ID |
| Portail libre-service | ❌ | ✅ | Partiel (myaccount) |
| Rapports de conformité | Scripts maison | ✅ natif | ✅ natif |

### 17.4 Conformité : qui n'est pas chiffré ?

```powershell
# Rapport rapide : postes du domaine sans BitLocker actif
$computers = Get-ADComputer -Filter * -SearchBase "OU=Postes Win11,DC=entreprise,DC=local" |
             Select-Object -ExpandProperty Name
Invoke-Command -ComputerName $computers -ErrorAction SilentlyContinue -ScriptBlock {
    $v = Get-BitLockerVolume -MountPoint "C:" -ErrorAction SilentlyContinue
    [pscustomobject]@{
        Hostname         = $env:COMPUTERNAME
        ProtectionStatus = $v.ProtectionStatus   # On = chiffré et protégé
        VolumeStatus     = $v.VolumeStatus
    }
} | Where-Object { $_.ProtectionStatus -ne 'On' } |
  Export-Csv "C:\Admin\BitLocker_NonConformes.csv" -NoTypeInformation -Encoding UTF8
```

---

## 18. BitLocker : dépannage — écran de récupération, boucles, causes fréquentes

### 18.1 L'écran de récupération : que faire (procédure standard)

```
1. Noter l'ID DE CLÉ affiché (ex. : 4A2B1C3D) — il identifie QUELLE clé utiliser
2. Retrouver la clé dans AD / Intune / fichier (section 16)
3. Saisir les 48 chiffres (tirets automatiques)
4. Le poste démarre → NE PAS s'arrêter là :
5. Suspend-BitLocker -MountPoint "C:" -RebootCount 1
6. Redémarrer → si ça redemande la clé, chercher la CAUSE (ci-dessous)
7. Une fois stable : Resume-BitLocker
```

### 18.2 Causes fréquentes de demande de clé (par ordre de fréquence)

| Cause | Explication | Prévention |
|---|---|---|
| Mise à jour du BIOS/UEFI | Le TPM mesure un firmware différent | `Suspend-BitLocker` avant toute MàJ BIOS |
| Changement matériel (carte mère, disque) | Nouvelle mesure PCR | Suspendre avant intervention |
| Modification de l'ordre de boot / Secure Boot désactivé | PCR 7 modifié | Ne pas toucher au boot sans suspendre |
| Docking station / périphérique USB au boot | Modification de la chaîne de boot | Débrancher, ou configurer le BIOS |
| Mise à jour Windows majeure | Rare, mais possible | — |
| TPM effacé (clear TPM) | Les clés scellées sont perdues | Ne jamais effacer le TPM sans avoir la clé de récupération ! |

### 18.3 Boucle de récupération (demande la clé à chaque démarrage)

```powershell
# 1. Démarrer avec la clé de récupération, ouvrir PowerShell admin
# 2. Vérifier les protecteurs
Get-BitLockerVolume -MountPoint C: | Select-Object -ExpandProperty KeyProtector

# 3. Le cas classique : le protecteur TPM est "cassé" → le recréer
# D'abord suspendre, puis supprimer et recréer le protecteur TPM :
Suspend-BitLocker -MountPoint "C:" -RebootCount 0
$vol = Get-BitLockerVolume -MountPoint "C:"
$tpmProt = $vol.KeyProtector | Where-Object { $_.KeyProtectorType -eq 'Tpm' }
if ($tpmProt) {
    Remove-BitLockerKeyProtector -MountPoint "C:" -KeyProtectorId $tpmProt.KeyProtectorId
}
Add-BitLockerKeyProtector -MountPoint "C:" -TpmProtector
Resume-BitLocker -MountPoint "C:"
```

### 18.4 Réparer le démarrage chiffré depuis WinRE

```powershell
# Dans WinRE (invite de commandes) : déverrouiller le volume
manage-bde -unlock C: -RecoveryPassword 111111-222222-333333-444444-555555-666666-777777-888888
# Puis réparer le BCD :
bootrec /fixmbr
bootrec /fixboot   # peut échouer en UEFI → utiliser bcdboot
bootrec /rebuildbcd
bcdboot C:\Windows /s S: /f UEFI
```

### 18.5 TPM verrouillé (lockout)

Après trop de tentatives de PIN erronées, le TPM se verrouille (anti-bruteforce). Il se déverrouille seul après un délai (jusqu'à 24 h selon la configuration), ou via la clé de récupération.

```powershell
# Voir l'état de verrouillage
Get-Tpm | Select-Object LockedOut, LockoutCount, LockoutTimeRemaining
```

---

## 19. Profils utilisateurs : locaux, itinérants, obligatoires

### 19.1 Les 3 types de profils

| Type | Stockage | Usage moderne |
|---|---|---|
| Local | `C:\Users\<nom>` | Standard (avec OneDrive KFM pour la « mobilité ») |
| Itinérant (roaming) | Copié depuis/vers un partage à l'ouverture/fermeture | **Déconseillé** : lent, corruptible, incompatible avec les apps modernes |
| Obligatoire (mandatory) | `NTUSER.MAN` en lecture seule | Kiosques, salles de formation |

**Doctrine 2026** : profil local + **OneDrive Known Folder Move** (Bureau/Documents/Images synchronisés) + **FSLogix** pour les environnements virtualisés. Les profils itinérants classiques sont une dette technique.

### 19.2 Anatomie d'un profil

```
C:\Users\jdupont\
 ├─ NTUSER.DAT            ← ruche de registre de l'utilisateur
 ├─ AppData\Local         ← données locales (non itinérantes)
 ├─ AppData\LocalLow
 ├─ AppData\Roaming       ← données "itinérantes"
 ├─ Desktop / Documents / Downloads / ...
```

```powershell
# Lister les profils présents sur un poste
Get-CimInstance Win32_UserProfile |
    Where-Object { -not $_.Special } |
    Select-Object LocalPath, Loaded, LastUseTime, @{N='SizeGB';E={
        [math]::Round((Get-ChildItem $_.LocalPath -Recurse -Force -ErrorAction SilentlyContinue |
            Measure-Object Length -Sum).Sum / 1GB, 2)
    }} | Sort-Object SizeGB -Descending
```

### 19.3 Supprimer proprement un profil

```powershell
# Méthode propre (via WMI — nettoie aussi le registre)
$profile = Get-CimInstance Win32_UserProfile -Filter "LocalPath='C:\\Users\\ancien.user'"
Remove-CimInstance -InputObject $profile

# ⛔ Ne JAMAIS supprimer juste le dossier C:\Users\... sans nettoyer
# HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList :
# → profil "fantôme" / temporaire au prochain logon (voir cas pratique 8).
```

### 19.4 Profil obligatoire (mandatory) — pour kiosques

```powershell
# 1. Personnaliser un profil modèle en mode audit
# 2. Renommer NTUSER.DAT en NTUSER.MAN dans le profil copié
Rename-Item "D:\Profils\kiosque\NTUSER.DAT" "NTUSER.MAN"
# 3. Le copier sur un partage, le référencer dans l'AD (onglet Profil de l'utilisateur)
```

---

## 20. FSLogix : principe, conteneurs de profils, cas d'usage

### 20.1 Pourquoi FSLogix

FSLogix encapsule le profil utilisateur dans un **conteneur VHDX** monté à l'ouverture de session. Le profil « suit » l'utilisateur en quelques secondes, sans copie réseau.

**Cas d'usage** : Azure Virtual Desktop, RDS/Terminal Server, postes partagés, environnements non persistants. Sur poste physique fixe : inutile (profil local + OneDrive suffit).

### 20.2 Architecture

```
Ouverture de session
   → FSLogix monte \\SRV-FSLOGIX\Profils$\%username%_%sid%\Profile_%username%.vhdx
   → le VHDX apparaît comme C:\Users\<nom>
   → à la fermeture : démontage, le VHDX reste sur le partage
```

### 20.3 Installation et configuration (résumé)

1. Installer l'agent FSLogix sur l'image/RDS.
2. Configurer via GPO (modèles ADMX FSLogix) ou registre :

```powershell
# Configuration minimale via registre (équivalent GPO)
$reg = "HKLM:\SOFTWARE\FSLogix\Profiles"
New-Item -Path $reg -Force | Out-Null
Set-ItemProperty -Path $reg -Name "Enabled" -Value 1 -Type DWord
Set-ItemProperty -Path $reg -Name "VHDLocations" -Value "\\SRV-FSLOGIX\Profils$" -Type MultiString
Set-ItemProperty -Path $reg -Name "SizeInMBs" -Value 30000 -Type DWord        # 30 Go
Set-ItemProperty -Path $reg -Name "IsDynamic" -Value 1 -Type DWord           # VHDX dynamique
Set-ItemProperty -Path $reg -Name "VolumeType" -Value "vhdx" -Type String
Set-ItemProperty -Path $reg -Name "FlipFlopProfileDirectoryName" -Value 1 -Type DWord
```

3. Droits sur le partage : les utilisateurs doivent pouvoir **créer** leur dossier (Creator Owner : contrôle total sur sous-dossiers).

### 20.4 Bonnes pratiques

| Point | Recommandation |
|---|---|
| Taille | 30 Go par défaut ; surveiller la croissance |
| Redirections | Exclure les caches lourds (Teams, navigateurs) via `redirections.xml` |
| Haute dispo | Partage sur stockage redondé (Storage Spaces Direct, Azure Files) |
| Office | Utiliser le **conteneur Office** séparé si besoin de cache partagé |
| Antivirus | Exclure les VHDX de l'analyse temps réel sur le serveur de fichiers |

---

## 21. Migration de profils : USMT (ScanState / LoadState)

### 21.1 USMT — quand l'utiliser

USMT (User State Migration Tool, inclus dans l'ADK) migre profils et paramètres lors d'un **remplacement de poste** ou d'une **réinstallation**. Pour une simple mise à niveau Windows 10 → 11 sur le même poste : inutile (le setup conserve tout).

### 21.2 ScanState (capture) et LoadState (restauration)

```powershell
# 1. Capture sur l'ancien poste (depuis WinPE ou session admin)
# /i: fichiers de règles, /o: écrase le magasin existant
C:\USMT\amd64\scanstate.exe \\SRV-MIGR\USMT$\%COMPUTERNAME% /i:C:\USMT\amd64\MigApp.xml /i:C:\USMT\amd64\MigDocs.xml /o /c /v:5 /l:C:\Logs\scanstate.log

# Options clés :
# /ue:DOMAINE\* /ui:DOMAINE\jdupont  : exclure tous sauf jdupont
# /c                                 : continuer malgré les erreurs
# /v:5                               : verbosité
# /encrypt /key:xxx                  : chiffrer le magasin (recommandé sur partage réseau)

# 2. Restauration sur le nouveau poste
C:\USMT\amd64\loadstate.exe \\SRV-MIGR\USMT$\ANCIEN-PC /i:C:\USMT\amd64\MigApp.xml /i:C:\USMT\amd64\MigDocs.xml /c /v:5 /l:C:\Logs\loadstate.log
```

### 21.3 Fichiers de règles personnalisés

```xml
<!-- MigCustom.xml : inclure un dossier métier, exclure les caches -->
<migration urlid="http://www.microsoft.com/migration/1.0/migxmlext/custom">
  <component type="Documents" context="User">
    <displayName>Dossier métier</displayName>
    <role role="Data">
      <rules>
        <include>
          <objectSet><pattern type="File">%CSIDL_LOCAL_APPDATA%\MonAppMetier\* [*]</pattern></objectSet>
        </include>
        <exclude>
          <objectSet><pattern type="File">%CSIDL_LOCAL_APPDATA%\*\Cache\* [*]</pattern></objectSet>
        </exclude>
      </rules>
    </role>
  </component>
</migration>
```

### 21.4 Alternative moderne

Pour les postes Entra join : **OneDrive Known Folder Move** + réinstallation des applications via Intune/winget remplace USMT dans 90 % des cas, sans infrastructure de magasin.

---

## 22. GPO Windows 11 : menu Démarrer, barre des tâches, disposition

### 22.1 Verrouiller la disposition du menu Démarrer

Windows 11 utilise un fichier JSON de disposition (`LayoutModification.json`), plus le XML historique :

```powershell
# Exporter la disposition d'un poste modèle
Export-StartLayout -Path C:\Admin\StartLayout.json
```

GPO : `Configuration utilisateur\Stratégies\Modèles d'administration\Menu Démarrer et barre des tâches` → **« Configurer la disposition de l'écran de démarrage »** → chemin du JSON (partage réseau en lecture seule).

Exemple de `StartLayout.json` minimal :

```json
{
  "pinnedList": [
    { "desktopAppLink": "%ALLUSERSPROFILE%\\Microsoft\\Windows\\Start Menu\\Programs\\Microsoft Edge.lnk" },
    { "desktopAppLink": "%APPDATA%\\Microsoft\\Windows\\Start Menu\\Programs\\Word.lnk" },
    { "packagedAppId": "Microsoft.WindowsCalculator_8wekyb3d8bbwe!App" }
  ]
}
```

### 22.2 Barre des tâches : épingles et alignement

| Réglage | GPO / Registre |
|---|---|
| Alignement (gauche vs centré) | `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced\TaskbarAl` (0 = gauche, 1 = centré) |
| Épingles imposées | Même JSON de disposition (section `taskbar` via stratégies MDM, ou GPO « Configurer la barre des tâches ») |
| Empêcher l'utilisateur de modifier | `LockTaskbar` / stratégies « Verrouiller la barre des tâches » |

```powershell
# Forcer l'alignement à gauche pour tous les nouveaux profils (image de référence)
$reg = "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced"
# Via le profil par défaut (Default User) :
reg load HKU\DefaultUser "C:\Users\Default\NTUSER.DAT"
reg add "HKU\DefaultUser\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v TaskbarAl /t REG_DWORD /d 0 /f
reg unload HKU\DefaultUser
```

### 22.3 Menu Démarrer : recommandations entreprise

- Épingler : Edge/Chrome géré, Office, portail d'entreprise, outil de tickets.
- **Retirer** : jeux, applications grand public, suggestions.
- Ne pas verrouiller à 100 % : laissez une zone libre pour l'appropriation (moins de tickets au support).

---

## 23. GPO Windows 11 : désactiver Copilot, Widgets, recommandations

### 23.1 Désactiver Windows Copilot (entreprise)

> ⚠️ Selon la build (23H2 vs 24H2) et la région, Copilot peut apparaître comme application ou comme bouton de barre des tâches. Appliquez les deux verrous.

| Méthode | Chemin |
|---|---|
| GPO | `Configuration ordinateur\Modèles d'administration\Composants Windows\Windows Copilot` → **« Désactiver Windows Copilot »** : Activé |
| Registre | `HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsCopilot\TurnOffWindowsCopilot` = 1 (DWORD) |
| Intune | Catalogue de paramètres → Windows AI / Copilot |

```powershell
# Via registre (déploiement scripté)
New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsCopilot" -Force | Out-Null
Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsCopilot" `
    -Name "TurnOffWindowsCopilot" -Value 1 -Type DWord
```

### 23.2 Désactiver les Widgets et le fil d'actualités

```powershell
# Widgets : GPO "Autoriser les widgets" → Désactivé
# (Modèles d'administration\Composants Windows\Widgets)
New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\Dsh" -Force | Out-Null
Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Dsh" `
    -Name "AllowNewsAndInterests" -Value 0 -Type DWord

# Bouton Widgets de la barre des tâches (par défaut utilisateur) :
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v TaskbarDa /t REG_DWORD /d 0 /f
```

### 23.3 Recommandations, suggestions, publicité dans le Démarrer

| Réglage | GPO |
|---|---|
| Désactiver les recommandations du Démarrer | `Menu Démarrer et barre des tâches` → « Supprimer la liste des recommandations » |
| Désactiver les suggestions d'applications | « Désactiver les suggestions dans le menu Démarrer » |
| Désactiver le contenu recommandé dans l'Explorateur | Stratégies de l'Explorateur de fichiers |
| Spotlight / arrière-plans « à la une » | Personnalisation → empêcher le changement |

```powershell
# Regroupé en script (extrait) :
$policies = @{
    "HKLM:\SOFTWARE\Policies\Microsoft\Windows\CloudContent" = @{
        "DisableWindowsSpotlightFeatures" = 1
        "DisableTailoredExperiencesWithDiagnosticData" = 1
    }
    "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Explorer" = @{
        "HideRecommendedSection" = 1
    }
}
foreach ($path in $policies.Keys) {
    New-Item -Path $path -Force | Out-Null
    foreach ($name in $policies[$path].Keys) {
        Set-ItemProperty -Path $path -Name $name -Value $policies[$path][$name] -Type DWord
    }
}
```

---

## 24. GPO Windows 11 : bonnes pratiques, héritage, filtrage, dépannage

### 24.1 Structurer ses OU et ses GPO

```
DC=entreprise,DC=local
 └─ OU=Postes de travail
     ├─ OU=Win11-Bureautique
     ├─ OU=Win11-Nomades
     ├─ OU=Win11-Kiosques
     └─ OU=Serveurs (séparé !)
```

Règles :

1. **Une GPO = un objectif** (« GPO-BitLocker-OS », « GPO-StartLayout », …) : on désactive sans tout casser.
2. Nommer avec préfixe : `GPO-[Cible]-[Fonction]` (ex. `GPO-Win11-Defender-ASR`).
3. Lier au plus près de la cible, éviter le filtrage de sécurité complexe.
4. Toujours tester sur une OU pilote avant de lier au niveau supérieur.

### 24.2 Filtrage WMI (ex. : cibler Windows 11 uniquement)

```wql
-- Filtre WMI "Windows 11 uniquement" :
SELECT * FROM Win32_OperatingSystem WHERE Version LIKE "10.0.22%" OR Version LIKE "10.0.26%"
-- (Windows 11 = version 10.0.22000+, builds 22621/22631 = 22H2/23H2, 26100 = 24H2)
```

> ⚠️ Les filtres WMI ralentissent l'ouverture de session s'ils sont nombreux ou mal écrits. Préférez les OU dédiées quand c'est possible.

### 24.3 Dépannage : la GPO ne s'applique pas (voir aussi cas pratique 10)

```powershell
# 1. Forcer l'actualisation
gpupdate /force

# 2. Voir les GPO appliquées (résumé)
gpresult /r

# 3. Rapport HTML détaillé (à ouvrir dans Edge)
gpresult /h C:\Admin\gpresult.html

# 4. Simuler pour un autre utilisateur / poste (console GPMC)
# GPMC → "Résolution des problèmes de stratégie de groupe" → Assistant

# 5. Vérifier la réplication SYSVOL entre DC
dcdiag /test:frssysvol
repadmin /syncall /AdeP
```

---

## 25. Windows Update for Business : anneaux de déploiement

### 25.1 Le modèle des anneaux (rings)

```
Anneau 0 — Pilotes/IT      : 0 jour de délai   (5-10 postes, l'équipe IT)
Anneau 1 — Testeurs        : 7 jours           (5 % du parc, volontaires)
Anneau 2 — Production large: 14-21 jours       (le reste du parc)
Anneau 3 — Critiques       : 30 jours + pause  (postes sensibles : caisses, supervision)
```

> 💡 Les **mises à jour de qualité** (mensuelles, Patch Tuesday) et les **mises à jour de fonctionnalités** (annuelles) ont des délais séparés. Les MàJ de qualité : délai court (7-14 j). Les MàJ de fonctionnalités : délai long (60-120 j) pour laisser mûrir.

### 25.2 Définir les anneaux (Intune)

Intune → Appareils → Mises à jour Windows → **Anneaux de mise à jour** :

| Paramètre | Anneau Testeurs | Anneau Production |
|---|---|---|
| MàJ qualité — délai | 7 jours | 14 jours |
| MàJ fonctionnalités — délai | 30 jours | 90 jours |
| Canal de maintenance | General Availability | General Availability |
| Pilotes | Autoriser | Bloquer (déployer via constructeur) |
| Désinstaller auto si problème | Oui | Oui |

### 25.3 Définir les anneaux (GPO — AD joint)

`Configuration ordinateur\Modèles d'administration\Composants Windows\Windows Update\Gérer les mises à jour proposées par Windows Update` :

- « Sélectionner quand les mises à jour qualité sont reçues » : délai en jours.
- « Sélectionner quand les builds d'évaluation et les mises à jour de fonctionnalités sont reçus » : délai en jours.
- « Mettre en pause les mises à jour » : en cas d'incident majeur.

```powershell
# Équivalent registre (à n'utiliser qu'en scripté, préférez la GPO) :
$wu = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU"
New-Item -Path $wu -Force | Out-Null
Set-ItemProperty -Path $wu -Name "DeferFeatureUpdates" -Value 1 -Type DWord
Set-ItemProperty -Path $wu -Name "DeferFeatureUpdatesPeriodInDays" -Value 90 -Type DWord
Set-ItemProperty -Path $wu -Name "DeferQualityUpdates" -Value 1 -Type DWord
Set-ItemProperty -Path $wu -Name "DeferQualityUpdatesPeriodInDays" -Value 14 -Type DWord
```

---

## 26. WUfB : stratégies (CSP, GPO, Intune), délais, pause, qualité vs fonctionnalités

### 26.1 Les trois types de mises à jour

| Type | Contenu | Fréquence | Délai conseillé |
|---|---|---|---|
| Qualité (quality) | Correctifs sécurité + bugs cumulatifs | Mensuel (2e mardi) | 7-14 j |
| Fonctionnalités (feature) | Nouvelle version annuelle (24H2…) | 1/an | 60-120 j |
| Pilotes / firmware | Via Windows Update (optionnel) | Continu | Bloquer en prod, canal constructeur |

### 26.2 Mettre en pause (gestion d'incident)

```powershell
# Pause via registre (équivalent GPO "Mettre en pause") :
$wu = "HKLM:\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings"
Set-ItemProperty -Path $wu -Name "PauseFeatureUpdatesStartTime" -Value (Get-Date -Format "yyyy-MM-dd") -Type String
Set-ItemProperty -Path $wu -Name "PauseQualityUpdatesStartTime"  -Value (Get-Date -Format "yyyy-MM-dd") -Type String
# Reprendre : supprimer ces valeurs ou via Paramètres → Windows Update.
```

Procédure d'incident « MàJ qui casse » :

```
1. Mettre en pause l'anneau Production (GPO/Intune) — 7 jours
2. Identifier la KB fautive (journaux, retours testeurs)
3. Tester le correctif / la désinstallation sur l'anneau pilote
4. Si nécessaire : wusa /uninstall /kb:XXXXXXX (section 45)
5. Lever la pause par anneau, en cascade
```

### 26.3 Pilotes via Windows Update : oui ou non ?

- **En entreprise : NON** par défaut. Les pilotes doivent venir du constructeur (Dell Command Update, Lenovo System Update, HP Image Assistant) validés par l'IT.
- GPO : « Ne pas inclure les pilotes avec les mises à jour Windows » → Activé.
- Exception : périphériques exotiques sans canal constructeur.

### 26.4 Heures d'activité et redémarrages

```powershell
# Forcer les heures d'activité 8h-19h (les MàJ ne redémarrent pas dedans)
$wu = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate"
Set-ItemProperty -Path $wu -Name "SetActiveHours" -Value 1 -Type DWord
Set-ItemProperty -Path $wu -Name "ActiveHoursStart" -Value 8 -Type DWord
Set-ItemProperty -Path $wu -Name "ActiveHoursEnd" -Value 19 -Type DWord
# Notification de redémarrage :
Set-ItemProperty -Path $wu -Name "AutoRestartRequiredNotificationDismissal" -Value 2 -Type DWord
```

---

## 27. WSUS vs Windows Update for Business : comparatif et coexistence

### 27.1 Comparatif

| Critère | WSUS | WUfB |
|---|---|---|
| Infrastructure | Serveur(s) WSUS, stockage local (To) | Aucune (cloud Microsoft) |
| Bande passante | Optimisée en LAN (1 téléchargement) | Delivery Optimization (P2P LAN) |
| Contrôle fin (approuver KB par KB) | ✅ | ❌ (délais uniquement) |
| Postes nomades / télétravail | Compliqué (VPN) | ✅ natif |
| Coût | Licences serveur + stockage + admin | Inclus (Intune/GPO) |
| Rapports | Console WSUS (limitée) | Update Compliance / Intune |
| Avenir | En maintenance, **déprécié à terme** | **Stratégie Microsoft** |

**Verdict 2026** : pour un parc Windows 11 moderne, **WUfB + Delivery Optimization** est la cible. WSUS ne se justifie plus que pour des environnements isolés (sans Internet) ou des contraintes réglementaires strictes.

### 27.2 Coexistence / migration WSUS → WUfB

```
1. Inventorier les GPO WSUS existantes (WUServer, TargetGroup...)
2. Créer les anneaux WUfB en parallèle sur une OU pilote
3. Basculer l'OU pilote : supprimer le pointage WSUS (GPO "Spécifier l'emplacement...")
   → les postes repassent sur Windows Update avec les délais WUfB
4. Valider 1 cycle mensuel complet
5. Généraliser par vagues, puis décommissionner WSUS
```

```powershell
# Vérifier vers quoi pointe un poste (WSUS ou WU public)
Get-ItemProperty "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate" -Name WUServer -ErrorAction SilentlyContinue
# Si vide → Windows Update public / WUfB (selon les autres stratégies)
```

### 27.3 Delivery Optimization (le remplaçant du stockage WSUS)

```powershell
# Limiter le P2P au LAN (pas de pairs Internet)
$do = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\DeliveryOptimization"
New-Item -Path $do -Force | Out-Null
Set-ItemProperty -Path $do -Name "DODownloadMode" -Value 1 -Type DWord
# 0=Bypass, 1=LAN (groupe), 2=Groupe, 3=Internet, 99=Simple, 100=Bypass

# Cache local : taille max (Go)
Set-ItemProperty -Path $do -Name "DOMaxCacheSize" -Value 20 -Type DWord
```

---

## 28. Déploiement d'applications : winget en entreprise

### 28.1 Winget — l'essentiel

`winget` (Gestionnaire de package Windows) est intégré à Windows 11. Il installe depuis des manifests (dépôt community + Microsoft Store).

```powershell
# Rechercher / installer
winget search "7-Zip"
winget install --id 7zip.7zip --silent --accept-package-agreements --accept-source-agreements

# Lister et mettre à jour
winget list
winget upgrade --all --silent --accept-package-agreements
```

### 28.2 Script de post-installation type (MDT / Intune / GPO)

```powershell
<#
.SYNOPSIS
    Installe le socle applicatif via winget. À exécuter en SYSTEM ou admin.
    Idempotent : ne réinstalle pas ce qui est déjà présent.
#>
$ErrorActionPreference = 'Continue'
$apps = @(
    "7zip.7zip",
    "VideoLAN.VLC",
    "Adobe.Acrobat.Reader.64-bit",
    "Mozilla.Firefox",
    "Notepad++.Notepad++"
)

# winget en contexte SYSTEM : forcer le chemin et les accords
$winget = (Get-ChildItem "$env:ProgramFiles\WindowsApps\Microsoft.DesktopAppInstaller_*" |
           Sort-Object LastWriteTime -Descending | Select-Object -First 1).FullName + "\winget.exe"

foreach ($id in $apps) {
    if (-not (& $winget list --id $id --exact 2>$null | Select-String $id)) {
        Write-Output "Installation de $id ..."
        & $winget install --id $id --exact --silent `
            --accept-package-agreements --accept-source-agreements `
            --disable-interactivity
    } else {
        Write-Output "$id déjà installé."
    }
}
```

> ⚠️ **Limites de winget en entreprise** : le dépôt community n'est pas « validé entreprise » (un manifest peut changer). Pour un socle critique : **miroir interne** ou packages validés (Intune Win32, MDT). Winget = excellent pour le socle bureautique standard, pas pour les applications métier critiques sans validation.

### 28.3 Stratégies winget (désactiver / restreindre)

```powershell
# Désactiver winget (postes ultra-sensibles) via stratégie :
$pol = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\AppInstaller"
New-Item -Path $pol -Force | Out-Null
Set-ItemProperty -Path $pol -Name "EnableAppInstaller" -Value 0 -Type DWord

# Restreindre aux sources approuvées : n'autoriser que le dépôt interne
# (via "AllowedSources" / paramètres d'entreprise — voir doc winget Enterprise)
```

### 28.4 Mettre à jour en masse (maintenance mensuelle)

```powershell
# Via Intune : script PowerShell planifié, ou via GPO (tâche planifiée) :
$action = New-ScheduledTaskAction -Execute "powershell.exe" `
    -Argument "-NoProfile -File C:\Admin\winget-upgrade.ps1"
$trigger = New-ScheduledTaskTrigger -Weekly -DaysOfWeek Sunday -At "03:00"
Register-ScheduledTask -TaskName "Winget-Upgrade-Hebdo" -Action $action `
    -Trigger $trigger -User "SYSTEM" -RunLevel Highest
```

---

## 29. Microsoft Store en entreprise, applications privées, packaging

### 29.1 Le Store en entreprise : état des lieux

- Le **Microsoft Store for Business** historique est **fermé**. Remplacé par l'intégration Intune du nouveau Microsoft Store + **dépôt privé**.
- Via Intune : Applications → ajouter une « application du Microsoft Store (nouveau) » → recherche dans le catalogue → affectation aux groupes.

### 29.2 Bloquer ou autoriser le Store (GPO)

| Objectif | Réglage |
|---|---|
| Bloquer le Store grand public | `Désactiver l'application Store` (Composants Windows\Store) |
| Autoriser mais sans applications personnelles | Intune : n'affecter que les apps approuvées + AppLocker |
| Dépôt privé | Intune → Store → applications privées de l'entreprise |

```powershell
# Désactiver le Store via registre (kiosques, postes sensibles)
New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\WindowsStore" -Force | Out-Null
Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\WindowsStore" `
    -Name "RemoveWindowsStore" -Value 1 -Type DWord
```

### 29.3 Packaging : les formats

| Format | Usage | Outil |
|---|---|---|
| **Win32** (.exe/.msi + script) | Standard entreprise | IntuneWinAppUtil → `.intunewin` |
| MSIX | Moderne, sandboxé | MSIX Packaging Tool |
| APPX | Legacy Store | — |
| MSI | Toujours valable | Orca / éditeurs MSI |

```powershell
# Créer un package Intune (.intunewin) depuis un dossier source
.\IntuneWinAppUtil.exe -c C:\Packages\MonApp\Source -s setup.exe -o C:\Packages\MonApp\Output
# Commandes d'install/désinstall à déclarer dans Intune :
#   Install : setup.exe /S
#   Uninstall : "%ProgramFiles%\MonApp\uninstall.exe" /S
# Règle de détection : fichier / registre / version
```

### 29.4 Règles de packaging (checklist)

- [ ] Installation **silencieuse** (aucune interaction)
- [ ] Fonctionne en contexte **SYSTEM** (Intune)
- [ ] Code de retour géré (0 = succès ; 3010 = reboot requis)
- [ ] Règle de **détection** fiable (pas seulement « le dossier existe »)
- [ ] Commande de **désinstallation** testée
- [ ] Testé sur 23H2 **et** 24H2

---

## 30. Sécurité : Microsoft Defender Antivirus — configuration entreprise

### 30.1 Vérifier l'état

```powershell
# État global
Get-MpComputerStatus | Select-Object AntivirusEnabled, RealTimeProtectionEnabled,
    AntispywareEnabled, AMServiceEnabled, AntivirusSignatureLastUpdated,
    QuickScanAge, FullScanAge

# Préférences configurées
Get-MpPreference | Select-Object -ExpandProperty ExclusionPath
```

### 30.2 Durcissement via PowerShell (ou GPO / Intune)

```powershell
# Activer la protection cloud + niveau élevé (recommandé)
Set-MpPreference -MAPSReporting Advanced
Set-MpPreference -SubmitSamplesConsent SendAllSamples

# Protection contre les altérations (Tamper Protection) — via Intune de préférence
Set-MpPreference -DisableTamperProtection $false

# Analyses planifiées : rapide quotidienne, complète hebdo
Set-MpPreference -ScanScheduleDay 0 -ScanScheduleTime "02:00"   # 0 = tous les jours
# Analyse complète le dimanche :
# (via GPO : "Spécifier le jour de la semaine pour l'analyse complète")

# Exclusions : avec parcimonie, documentées, revues trimestriellement
Add-MpPreference -ExclusionPath "D:\BasesDeDonnees"
Add-MpPreference -ExclusionProcess "C:\Apps\Metier\app.exe"
```

### 30.3 Règles de réduction de la surface d'attaque (ASR) — activation

```powershell
# Exemple : bloquer le contenu exécutable d'un client de messagerie (règle audit puis block)
# GUIDs des règles ASR : voir documentation Microsoft (liste officielle)
$asrRules = @(
    "BE9BA2D9-53EA-4CDC-84E5-9B1EEEE46550",  # Bloquer exécutable depuis client mail
    "D4F940AB-401B-4EFC-AADC-ADB37D02A12E",  # Bloquer exécutable depuis Adobe Reader
    "3B576869-A4EC-4529-8536-B80A7769E899"   # Bloquer Office créant du contenu exécutable
)
foreach ($guid in $asrRules) {
    Add-MpPreference -AttackSurfaceReductionRules_Ids $guid -AttackSurfaceReductionRules_Actions Enabled
}
# Actions : Disabled=0, Block=1, Audit=2, Warn=6
# TOUJOURS commencer en Audit (2), analyser, puis passer en Block (1).
```

### 30.4 Journaux Defender utiles

| Journal | Emplacement |
|---|---|
| Opérationnel | `Applications and Services Logs\Microsoft\Windows\Windows Defender\Operational` |
| Détections | Même journal, ID 1116 (malware détecté), 1117 (action) |
| Ligne de commande | `Get-MpThreat`, `Get-MpThreatDetection` |

```powershell
# Historique des menaces
Get-MpThreat | Select-Object ThreatName, SeverityID, InitialDetectionTime, Resources
```

---

(Bloc 2/6 — sections 16 à 30)

---

## 31. Sécurité : SmartScreen, protection réseau, contrôle des applications

### 31.1 Microsoft Defender SmartScreen

SmartScreen filtre les URL et les fichiers (réputation) dans Edge et l'Explorateur.

| Réglage | GPO |
|---|---|
| Activer SmartScreen pour Edge | `Microsoft Edge\SmartScreen` → Activé |
| Activer pour l'Explorateur | `Composants Windows\Explorateur Windows` → « Configurer Windows Defender SmartScreen » |
| Empêcher le contournement | « Empêcher le contournement des avertissements SmartScreen » |

```powershell
# État via registre
Get-ItemProperty "HKLM:\SOFTWARE\Policies\Microsoft\Windows\System" -Name "EnableSmartScreen" -ErrorAction SilentlyContinue
# 1 = activé (stratégie), 2 = désactivé
```

### 31.2 Contrôle des applications : les 3 niveaux

| Technologie | Granularité | Complexité | Recommandé |
|---|---|---|---|
| **Smart App Control** | Réputation IA (binaire) | Faible (on/off) | Postes standard récents (édition Famille/Pro) |
| **AppLocker** | Règles par éditeur/chemin/hash | Moyenne | Entreprise : standard historique |
| **Windows Defender Application Control (WDAC)** | Stratégies signées, noyau | Élevée | Environnements critiques |

**En entreprise : AppLocker** reste le meilleur compromis (GPO natives, audit possible).

```powershell
# AppLocker : exporter la stratégie en XML (audit)
Get-AppLockerPolicy -Effective -Xml > C:\Admin\applocker-effectif.xml

# Tester une règle
Test-AppLockerPolicy -XmlPolicy C:\Admin\applocker.xml -Path C:\Outils\outil.exe -User "ENTREPRISE\jdupont"
```

Politique AppLocker minimale viable :

1. Règles par défaut (tout le monde peut exécuter dans `C:\Windows` et `C:\Program Files`).
2. Règles **Éditeur** pour les applications métier (plutôt que chemin/hash, plus maintenables).
3. Mode **Audit** 2-4 semaines → analyser le journal → passer en **Appliquer**.

---

## 32. Sécurité : Credential Guard, LSA Protection, HVCI / Memory Integrity

### 32.1 Vue d'ensemble (virtualization-based security)

| Fonctionnalité | Protège contre | Prérequis |
|---|---|---|
| **Credential Guard** | Vol d'identifiants en mémoire (Mimikatz & co) | Entreprise, VBS, UEFI |
| **LSA Protection (PPL)** | Injection de code dans LSASS | Registre ou stratégie |
| **HVCI / Memory Integrity** | Pilotes/code non signés dans le noyau | CPU avec VBS, Secure Boot |

### 32.2 Activer Credential Guard (Entreprise)

GPO : `Configuration ordinateur\Modèles d'administration\Système\Device Guard` → **« Activer la sécurité basée sur la virtualisation »** :

- Sélectionner le niveau de sécurité de plateforme : **Démarrage sécurisé** (ou + DMA).
- Credential Guard : **Activé avec verrouillage UEFI**.

```powershell
# Vérifier que VBS / Credential Guard sont actifs
Get-CimInstance -ClassName Win32_DeviceGuard -Namespace root\Microsoft\Windows\DeviceGuard |
    Select-Object VirtualizationBasedSecurityStatus, SecurityServicesConfigured, SecurityServicesRunning
# SecurityServicesRunning = 1 (Credential Guard) / 2 (HVCI)

# Alternative lisible :
(Get-ComputerInfo).DeviceGuardSecurityServicesConfigured
```

> ⚠️ Credential Guard **casse** l'authentification NTLMv1 et certains fournisseurs d'identifiants tiers. Tester sur l'anneau pilote avant généralisation. Wi-Fi 802.1X avec certificats machine : OK.

### 32.3 LSA Protection

```powershell
# Activer LSA Protection (PPL pour LSASS)
$lsa = "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa"
Set-ItemProperty -Path $lsa -Name "RunAsPPL" -Value 1 -Type DWord
# Redémarrage requis. Vérifier dans le journal :
# Applications and Services Logs\Microsoft\Windows\CodeIntegrity\Operational, ID 3065/3066
```

### 32.4 HVCI / Intégrité de la mémoire

Paramètres → Confidentialité et sécurité → Sécurité Windows → Isolation du noyau → **Intégrité de la mémoire** : Activé (défaut sur installations récentes de Windows 11).

```powershell
# Forcer par stratégie (recommandé en entreprise)
$dg = "HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard"
New-Item -Path "$dg\Scenarios\HypervisorEnforcedCodeIntegrity" -Force | Out-Null
Set-ItemProperty -Path "$dg\Scenarios\HypervisorEnforcedCodeIntegrity" -Name "Enabled" -Value 1 -Type DWord
```

> ⚠️ HVCI peut bloquer des **pilotes anciens non signés** (imprimantes, dongles, logiciels industriels). Symptôme : périphérique avec point d'exclamation + ID d'événement 3089. Solution : pilote à jour signé, ou exclusion ciblée (documentée).

---

## 33. Sécurité : durcissement — baselines Microsoft, ASR, Attack Surface Reduction

### 33.1 Security Baselines Microsoft

Microsoft publie des **baselines** (GPO packagées) pour chaque version de Windows : ~200 réglages prêts (Defender, Edge, Credential Guard, SMB, etc.).

Procédure :

```
1. Télécharger la "Security Compliance Toolkit" correspondant à la version (ex. Windows 11 24H2)
2. Importer les GPO via les scripts fournis (Baseline-LocalInstall.ps1 pour tester en local)
3. Comparer avec Policy Analyzer avant/après
4. Adapter : TOUTE baseline se personnalise (ex. : exceptions métier)
```

```powershell
# Appliquer la baseline en local (lab/test uniquement)
.\Baseline-LocalInstall.ps1 -Win11
# Puis vérifier avec Policy Analyzer (même toolkit)
```

### 33.2 Points de durcissement prioritaires (top 15)

| # | Réglage | Effet |
|---|---|---|
| 1 | Désactiver SMBv1 | Protocole obsolète, vecteur WannaCry |
| 2 | Désactiver LLMNR / NetBIOS | Anti-spoofing local |
| 3 | Signer SMB obligatoire | Anti-relay |
| 4 | UAC au maximum | Élévation systématique |
| 5 | Désactiver AutoRun/AutoPlay | Anti-clé USB |
| 6 | Règles ASR en Block (après audit) | Anti-ransomware/phishing |
| 7 | Bloquer les macros Office non signées | Vecteur n°1 |
| 8 | PowerShell Constrained Language (WDAC) | Anti-living-off-the-land |
| 9 | Désactiver PowerShell v2 | Moteur obsolète |
| 10 | LAPS Windows actif | Mots de passe admin locaux uniques |
| 11 | Credential Guard | Anti-vol d'identifiants |
| 12 | HVCI | Anti-pilotes malveillants |
| 13 | BitLocker imposé | Vol/perte |
| 14 | Pare-feu : bloquer entrant par défaut | Réduction de surface |
| 15 | Compte Administrator intégré désactivé | Moins de cible |

```powershell
# Désactiver SMBv1 (si encore présent)
Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol -NoRestart
Get-SmbServerConfiguration | Select-Object EnableSMB1Protocol, EnableSMB2Protocol

# Désactiver PowerShell v2
Disable-WindowsOptionalFeature -Online -FeatureName MicrosoftWindowsPowerShellV2Root -NoRestart

# Désactiver LLMNR + NetBIOS (via registre, déployé par GPO)
$tcp = "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\DNSClient"
New-Item -Path $tcp -Force | Out-Null
Set-ItemProperty -Path $tcp -Name "EnableMulticast" -Value 0 -Type DWord  # LLMNR off
```

### 33.3 Suivi du durcissement : Microsoft Secure Score

- **Microsoft Secure Score** (portail Defender) : note + actions recommandées pour l'organisation.
- **Exposure Score** : exposition des appareils.
- À suivre mensuellement en comité sécurité.

---

## 34. LAPS Windows (nouveau) : principe, GPO, récupération

### 34.1 Windows LAPS vs l'ancien LAPS (différences clés)

| Point | Ancien LAPS (outil) | Windows LAPS (intégré) |
|---|---|---|
| Installation | MSI à déployer | **Natif** depuis avril 2023 (MàJ cumulative) |
| Stockage | AD uniquement | **AD ou Entra ID** |
| Chiffrement du mot de passe | Non | Oui (optionnel, AD) |
| Historique des mots de passe | Non | Oui |
| Gestion DSRM | Non | Oui |

> ✅ **Windows LAPS est le standard** : l'ancien LAPS est déprécié. Si l'ancien est encore déployé, migrez (les stratégies sont distinctes, pas de conflit si on migre proprement par OU).

### 34.2 Configurer Windows LAPS (GPO)

`Configuration ordinateur\Modèles d'administration\Système\LAPS` :

| Stratégie | Réglage recommandé |
|---|---|
| Configurer le répertoire de sauvegarde | **Active Directory** (ou Entra ID si cloud) |
| Nom du compte administrateur à gérer | `Administrateur` (ou compte dédié `adm-local`) |
| Longueur du mot de passe | 20+ caractères |
| Âge maximal du mot de passe | 30 jours |
| Activer le chiffrement du mot de passe | Oui (nécessite le schéma AD à jour 2023+) |

```powershell
# Équivalent : forcer la rotation immédiate (test)
Reset-LapsPassword  # (module LAPS PowerShell si installé)

# Vérifier l'état LAPS sur un poste
Get-LapsAADPassword -DeviceId (Get-DeviceId) -AsPlainText  # Entra : via Graph/portail
```

### 34.3 Récupérer le mot de passe (AD)

```powershell
# Depuis un poste avec les outils RSAT / module LAPS :
Get-LapsADPassword -Identity "PC-COMPTA-042" -AsPlainText

# Délégation : donner au helpdesk le droit "Lire le mot de passe LAPS"
# (autorisation étendue ms-LAPS-Password sur l'OU des postes)
```

### 34.4 Procédure helpdesk « mot de passe admin local »

```
1. Vérifier l'identité du demandeur + le motif (ticket obligatoire)
2. Get-LapsADPassword -Identity <poste> -AsPlainText
3. Communiquer le mot de passe (jamais par e-mail), usage unique
4. Forcer la rotation après usage : le mot de passe expire automatiquement
   au prochain cycle (ou forcer via stratégie "expiration après utilisation")
```

> 🔐 Le compte admin local géré par LAPS **ne doit jamais** servir au quotidien : uniquement dépannage hors réseau / hors domaine. Toute utilisation = ticket + rotation.

---

## 35. Pare-feu Windows Defender : profils, règles, GPO, journalisation

### 35.1 Les 3 profils

| Profil | Quand | Politique par défaut recommandée |
|---|---|---|
| Domaine | Connecté au domaine AD | Entrant : bloquer (sauf exceptions) |
| Privé | Réseau de confiance (domicile) | Entrant : bloquer |
| Public | Wi-Fi public, inconnu | Entrant : bloquer strict |

### 35.2 Gérer par GPO (recommandé) plutôt qu'en local

`Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres de sécurité\Pare-feu Windows Defender avec fonctions avancées de sécurité`.

Bonnes pratiques :

- Définir les règles **une fois** en GPO, interdire la fusion avec les règles locales sur les postes sensibles (`Appliquer les règles locales : Non`).
- Nommer les règles : `[APP]-[Sens]-[Port]` (ex. `Zabbix-Agent-In-10050`).
- Documenter chaque exception dans un registre des flux.

```powershell
# Créer une règle en PowerShell (pour script / image de référence)
New-NetFirewallRule -DisplayName "Zabbix-Agent-In-10050" `
    -Direction Inbound -Protocol TCP -LocalPort 10050 `
    -RemoteAddress "10.0.50.0/24" -Action Allow -Profile Domain

# Lister les règles actives
Get-NetFirewallRule -Enabled True -Direction Inbound |
    Select-Object DisplayName, Profile, Action | Sort-Object DisplayName

# Règle de test temporaire (à supprimer après)
New-NetFirewallRule -DisplayName "TEMP-Debug" -Direction Inbound -Action Allow -Profile Any
```

### 35.3 Journalisation (indispensable en dépannage réseau)

```powershell
# Activer le log des paquets rejetés (par profil)
Set-NetFirewallProfile -Profile Domain, Private, Public -LogBlocked True -LogMaxSizeKilobytes 16384
# Fichier : C:\Windows\System32\LogFiles\Firewall\pfirewall.log

# Lire les rejets récents
Get-Content C:\Windows\System32\LogFiles\Firewall\pfirewall.log -Tail 20
```

> 💡 **Réflexe dépannage** : « l'application ne répond plus sur le réseau » → 1) `Test-NetConnection`, 2) `pfirewall.log`, 3) règle GPO. Dans 50 % des cas, c'est le pare-feu.

---

## 36. Réseau : Wi-Fi d'entreprise 802.1X (WPA2/WPA3-Enterprise)

### 36.1 Architecture

```
Client Windows 11
   → Point d'accès (authenticator)
   → Serveur RADIUS (NPS / ISE / ClearPass)
   → Active Directory / Entra (annuaire)
   Méthode EAP : PEAP-MSCHAPv2 (identifiants) ou EAP-TLS (certificats, recommandé)
```

| Méthode EAP | Sécurité | Contrainte |
|---|---|---|
| PEAP + MSCHAPv2 | Correcte | Mots de passe (phishing possible) |
| **EAP-TLS** | **Forte** | PKI : certificat machine et/ou utilisateur à déployer |

**Recommandation** : EAP-TLS avec certificats **machine** (auto-enrollment AD CS ou Intune/SCEP) : l'utilisateur n'a rien à saisir, le poste s'authentifie avant l'ouverture de session (utile pour les GPO au démarrage).

### 36.2 Déployer le profil Wi-Fi par GPO

`Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres de sécurité\Stratégies de réseau sans fil` → créer un profil :

- SSID : `ENTREPRISE` (ne pas diffuser le SSID invité ici)
- Sécurité : WPA2-Entreprise (ou WPA3-Entreprise si le parc AP le supporte)
- EAP : Microsoft Smart Card ou EAP-TLS
- Cocher « Authentification ordinateur » pour le pré-logon

### 36.3 Déployer par Intune

Intune → Appareils → Profils de configuration → **Wi-Fi** :

- Type : Entreprise
- EAP : EAP-TLS, certificat client SCEP
- Serveurs RADIUS de confiance (noms des certificats du NPS)

### 36.4 Commandes utiles côté client

```powershell
# Profils Wi-Fi mémorisés
netsh wlan show profiles

# Détail d'un profil (dont la méthode EAP)
netsh wlan show profile name="ENTREPRISE" key=clear

# Exporter / importer un profil (déploiement manuel / dépannage)
netsh wlan export profile name="ENTREPRISE" folder="C:\Admin" key=clear
netsh wlan add profile filename="C:\Admin\Wi-Fi-ENTREPRISE.xml"

# Voir l'état de la connexion et le chiffrement négocié
netsh wlan show interfaces
```

---

## 37. Réseau : VPN — Always On VPN (intro), profils, dépannage

### 37.1 Always On VPN : principe

Successeur de DirectAccess (déprécié) : VPN **IKEv2/SSTP** natif Windows, déployé par **profil XML via Intune/GPO**, avec deux tunnels :

| Tunnel | Usage | Authentification |
|---|---|---|
| **Device Tunnel** | Connectivité machine avant logon (GPO, Intune) | Certificat machine |
| **User Tunnel** | Accès utilisateur | Certificat utilisateur ou EAP |

### 37.2 Infrastructure minimale

- Serveur **RRAS** (Windows Server) avec IKEv2, ou solution tierce compatible ;
- **PKI** (AD CS) : certificats machine + serveur (SAN = nom public) ;
- **NPS** pour l'authentification RADIUS (optionnel selon l'architecture) ;
- Publication : le serveur VPN doit être joignable depuis Internet (443/500/4500).

### 37.3 Profil XML (extrait simplifié)

```xml
<VPNProfile>
  <NativeProfile>
    <Servers>vpn.entreprise.example</Servers>
    <NativeProtocolType>IKEv2</NativeProtocolType>
    <Authentication>
      <MachineMethod>Certificate</MachineMethod>
    </Authentication>
    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>
  </NativeProfile>
  <AlwaysOn>true</AlwaysOn>
  <DnsSuffix>entreprise.local</DnsSuffix>
  <TrustedNetworkDetection>entreprise.local</TrustedNetworkDetection>
</VPNProfile>
```

```powershell
# Appliquer un profil VPN en PowerShell (test / dépannage)
Add-VpnConnection -Name "ENTREPRISE-AO" -ServerAddress "vpn.entreprise.example" `
    -TunnelType Ikev2 -AuthenticationMethod MachineCertificate `
    -SplitTunneling -DnsSuffix "entreprise.local" -AllUserConnection

# État
Get-VpnConnection -Name "ENTREPRISE-AO" | Select-Object Name, ConnectionStatus, TunnelType
```

> ⚠️ **Split tunnel vs full tunnel** : le split tunnel (seul le trafic d'entreprise passe dans le VPN) économise la bande passante mais expose la navigation Internet du poste. Choix à valider avec la politique de sécurité (souvent : full tunnel pour les postes sensibles).

---

## 38. Réseau : dépannage — pile TCP/IP, DNS, proxy, certificats

### 38.1 La séquence de dépannage réseau (dans l'ordre)

```powershell
# 1. L'interface a-t-elle une adresse ?
Get-NetIPAddress -AddressFamily IPv4 | Where-Object { $_.InterfaceAlias -notlike "*Loopback*" }

# 2. La passerelle répond-elle ?
Test-NetConnection 10.0.0.254 -InformationLevel Detailed  # (adresse fictive d'exemple)

# 3. Le DNS résout-il ?
Resolve-DnsName intranet.entreprise.local -Server 10.0.0.10
nslookup intranet.entreprise.local

# 4. Le port du service est-il ouvert ?
Test-NetConnection srv-fichiers.entreprise.local -Port 445

# 5. Traceroute
tracert -d 8.8.8.8
```

### 38.2 Réinitialiser la pile réseau (quand tout est « bizarre »)

```powershell
# Dans l'ordre, puis redémarrer :
netsh winsock reset
netsh int ip reset
ipconfig /release
ipconfig /renew
ipconfig /flushdns
# Redémarrage obligatoire après winsock reset.
```

### 38.3 Proxy (cause fréquente en entreprise)

```powershell
# Voir la config proxy (WinHTTP = services, WinINET = utilisateur)
netsh winhttp show proxy
Get-ItemProperty "HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings" |
    Select-Object ProxyEnable, ProxyServer, AutoConfigURL

# Forcer l'alignement WinHTTP sur la config IE (utile pour Windows Update / Intune)
netsh winhttp import proxy source=ie
```

### 38.4 Certificats (802.1X, VPN, Wi-Fi, sites internes)

```powershell
# Certificats machine / utilisateur
Get-ChildItem Cert:\LocalMachine\My | Select-Object Subject, NotAfter, Thumbprint
Get-ChildItem Cert:\CurrentUser\My  | Select-Object Subject, NotAfter, Thumbprint

# Tester la chaîne de confiance d'un certificat
# (PowerShell 7+ : Test-Certificate ; sinon : certutil)
certutil -verify -urlfetch C:\Admin\cert.cer
```

---

## 39. WinRE : environnement de récupération, outils, personnalisation

### 39.1 Accéder à WinRE

| Méthode | Comment |
|---|---|
| Échecs de démarrage répétés (2-3) | Automatique |
| Depuis Windows | Paramètres → Récupération → Démarrage avancé → Redémarrer |
| Ligne de commande | `shutdown /r /o /f /t 0` |
| Écran d'ouverture de session | Maj + Redémarrer |

### 39.2 Outils disponibles dans WinRE

- **Invite de commandes** : diskpart, bcdboot, bootrec, manage-bde, chkdsk, sfc, dism (offline).
- **Désinstaller les mises à jour** : rollback d'une KB fautive sans démarrer Windows.
- **Restauration du système** : points de restauration.
- **Réparation du démarrage** : automatique (efficacité variable, voir cas pratique 1).
- **Réinitialiser ce PC**.

```powershell
# Vérifier que WinRE est activé et où se trouve son image
reagentc /info
# Sortie : Windows RE status: Enabled, emplacement \\?\GLOBALROOT\device\harddisk0\partition4\Recovery\WindowsRE

# Activer / désactiver
reagentc /enable
reagentc /disable
```

### 39.3 Commandes de réparation depuis WinRE (antisèche)

```powershell
# Reconstruire le BCD (UEFI)
diskpart
list vol
# identifier la partition EFI (FAT32, ~100 Mo) → lettre S:, et Windows → C:
exit
bcdboot C:\Windows /s S: /f UEFI

# Vérifier le disque
chkdsk C: /f /r

# Réparer les fichiers système hors ligne
sfc /scannow /offbootdir=C:\ /offwindir=C:\Windows

# Réparer l'image hors ligne (source : ISO montée ou WIM)
dism /Image:C:\ /Cleanup-Image /RestoreHealth /Source:E:\sources\install.wim:1
```

### 39.4 Personnaliser WinRE (avancé)

On peut injecter des pilotes/outils dans `winre.wim` (ex. : pilote RAID pour voir les disques) :

```powershell
# Monter, injecter, démonter (à faire sur une machine de lab)
dism /Mount-Wim /WimFile:C:\WinRE\winre.wim /Index:1 /MountDir:C:\Mount
dism /Image:C:\Mount /Add-Driver /Driver:C:\Drivers\RAID /Recurse
dism /Unmount-Wim /MountDir:C:\Mount /Commit
reagentc /setreimage /path C:\WinRE /target C:\Windows
```

---

## 40. Points de restauration, sauvegardes système, récupération

### 40.1 Points de restauration

```powershell
# Activer la protection système sur C:
Enable-ComputerRestore -Drive "C:\"

# Créer un point manuel (avant une intervention risquée !)
Checkpoint-Computer -Description "Avant MàJ pilote GPU" -RestorePointType "MODIFY_SETTINGS"

# Lister les points
Get-ComputerRestorePoint | Select-Object SequenceNumber, Description, CreationTime

# Restaurer (nécessite un redémarrage)
Restore-Computer -RestorePoint 12
```

> ⚠️ Les points de restauration ne protègent **ni les fichiers utilisateurs** ni les volumes chiffrés en cours de chiffrement. C'est un filet anti-bêtise système, pas une sauvegarde.

### 40.2 Sauvegarde image système (héritage, toujours présent)

```powershell
# Sauvegarde complète vers un disque externe (outil historique wbadmin)
wbadmin start backup -backupTarget:E: -include:C: -allCritical -quiet
```

### 40.3 Stratégie réaliste « récupération poste » en entreprise

| Scénario | Solution |
|---|---|
| Fichiers utilisateur | OneDrive KFM (continu) |
| Régression système (pilote, KB) | Point de restauration + désinstallation KB |
| Poste HS / volé / réaffecté | **Re-déploiement** (MDT/Autopilot) + OneDrive, pas de « réparation héroïque » |
| Besoin de preuve / forensique | Image disque bit-à-bit avant toute manipulation |

> 💡 **Doctrine** : en entreprise, un poste est **jetable**, les données ne le sont pas. Investissez dans OneDrive/KFM et le redéploiement rapide plutôt que dans des sauvegardes images de postes.

---

## 41. Dépannage : méthodologie et boîte à outils de l'admin

### 41.1 Méthode en 6 étapes

```
1. CADRER      : qui, quoi, quand, depuis quand, quoi de changé ?
2. REPRODUIRE  : le problème est-il reproductible ? Sur un autre poste / autre utilisateur ?
3. ISOLER      : dichotomie — réseau ? profil ? GPO ? pilote ? MàJ récente ?
4. JOURNAUX    : Event Viewer, journaux applicatifs, logs (toujours AVANT de toucher)
5. CORRIGER    : une seule modification à la fois, documentée
6. VÉRIFIER    : le problème a disparu ET rien d'autre n'a cassé
```

### 41.2 Boîte à outils (à avoir sur sa clé / son poste)

| Outil | Usage |
|---|---|
| Sysinternals Suite (Process Explorer, Autoruns, TCPView…) | Diagnostic avancé |
| SetupDiag | Échecs de mise à niveau |
| MDT / WinPE bootable | Réparation hors ligne |
| `winget` + scripts | Réinstallation rapide d'apps |
| RSAT | AD, GPO, DNS à distance |
| Wireshark | Captures réseau (avec accord) |
| CrystalDiskInfo | Santé SMART des disques |
| Carnet de bord | Noter chaque intervention (ticket GLPI !) |

### 41.3 Les journaux à connaître par cœur

| Journal | Chemin Event Viewer |
|---|---|
| Système | Journaux Windows → Système |
| Application | Journaux Windows → Application |
| Démarrage/arrêt | Système, ID 6005/6006/6008/6009, 1074 |
| Écrans bleus | Système, ID 1001 (BugCheck) |
| Windows Update | `Applications and Services Logs\Microsoft\Windows\WindowsUpdateClient\Operational` |
| GPO | `...\Microsoft\Windows\GroupPolicy\Operational` |
| BitLocker | `...\Microsoft\Windows\BitLocker\BitLocker-Operational` |
| Defender | `...\Microsoft\Windows\Windows Defender\Operational` |
| Inscription Entra | `...\Microsoft\Windows\User Device Registration\Admin` |

```powershell
# Extraire les erreurs récentes d'un journal (ex. : 7 derniers jours, niveau erreur)
Get-WinEvent -FilterHashtable @{
    LogName   = 'System'
    Level     = 2          # 2=Erreur, 3=Avertissement
    StartTime = (Get-Date).AddDays(-7)
} | Select-Object TimeCreated, Id, ProviderName,
    @{N='Message';E={ $_.Message.Substring(0, [Math]::Min(120, $_.Message.Length)) }} |
  Format-Table -AutoSize
```

---

## 42. Cas pratique 1 : le PC ne démarre plus (écran noir, BOOTMGR, BCD)

**Symptômes** : « Boot Device Not Found », écran noir avec curseur, ou boucle sur le logo.

**Diagnostic** :

```
1. Le BIOS/UEFI voit-il le disque ? (setup du firmware)
   → Non : problème matériel (câble, disque HS, contrôleur) — voir SMART
   → Oui : problème de démarrage logiciel → WinRE
2. Démarrer sur WinRE (clé USB ou démarrage avancé)
3. diskpart → list vol : la partition Windows est-elle visible et saine ?
```

**Réparation** (dans WinRE, invite de commandes) :

```powershell
# Identifier les lettres (en WinRE, C: n'est pas toujours Windows !)
diskpart
list vol
exit

# Cas UEFI : reconstruire le chargeur
bcdboot C:\Windows /s S: /f UEFI
# (S: = partition EFI montée au préalable via diskpart → assign letter=S)

# Cas Legacy/BIOS :
bootrec /fixmbr
bootrec /fixboot
bootrec /rebuildbcd

# Si le disque a des erreurs :
chkdsk C: /f /r
```

**Si BitLocker est actif** : déverrouiller d'abord avec `manage-bde -unlock` (section 18.4), sinon `chkdsk`/`bcdboot` travaillent sur du chiffré.

**Vérification** : 2 redémarrages complets (froid + chaud). Si le disque montre des secteurs réalloués (CrystalDiskInfo) → **remplacer le disque**, ne pas « réparer ».

---

## 43. Cas pratique 2 : boucle de réparation automatique / WinRE en boucle

**Symptômes** : « Réparation automatique » → « Votre PC n'a pas démarré correctement » → redémarrage → rebelote.

**Diagnostic** :

```
1. Dans WinRE → Options avancées → Invite de commandes
2. Lire le journal de la réparation :
   notepad C:\Windows\System32\LogFiles\Srt\SrtTrail.txt
   → la dernière section indique souvent la cause (pilote, KB, disque)
3. Vérifier les MàJ récentes : une KB installée juste avant ?
```

**Réparation** :

```powershell
# Option A : désinstaller la dernière MàJ qualité depuis WinRE
# WinRE → Dépannage → Désinstaller les mises à jour → "Désinstaller la dernière mise à jour qualité"

# Option B : en ligne de commande (identifier le package)
dism /Image:C:\ /Get-Packages /Format:Table | findstr "Installé"
dism /Image:C:\ /Remove-Package /PackageName:Package_for_RollupFix~31bf3856ad364e35~amd64~~26100.XXXX

# Option C : restauration système
# WinRE → Dépannage → Restauration du système → choisir un point antérieur

# Option D : désactiver la réparation auto pour voir le vrai BSOD
bcdedit /set {default} recoveryenabled No
# → au prochain boot, l'écran bleu affiche le code d'erreur au lieu de boucler
```

**Vérification** : réactiver la réparation après résolution (`recoveryenabled Yes`), puis 48 h d'observation.

---

## 44. Cas pratique 3 : BitLocker demande la clé de récupération en boucle

**Symptômes** : à chaque démarrage, l'écran bleu BitLocker réclame la clé à 48 chiffres.

**Diagnostic** : voir section 18 — causes typiques : MàJ BIOS sans suspension, changement de périphérique USB au boot, Secure Boot modifié, protecteur TPM corrompu.

**Résolution pas à pas** :

```powershell
# 1. Démarrer avec la clé (procédure section 18.1)
# 2. En session admin :
Suspend-BitLocker -MountPoint "C:" -RebootCount 1
Restart-Computer

# 3. Si ça redemande : recréer le protecteur TPM (section 18.3)
$vol = Get-BitLockerVolume -MountPoint "C:"
$tpmProt = $vol.KeyProtector | Where-Object { $_.KeyProtectorType -eq 'Tpm' }
Remove-BitLockerKeyProtector -MountPoint "C:" -KeyProtectorId $tpmProt.KeyProtectorId
Add-BitLockerKeyProtector -MountPoint "C:" -TpmProtector
Resume-BitLocker -MountPoint "C:"

# 4. Vérifier que la NOUVELLE clé de récupération est bien sauvegardée en AD !
Backup-BitLockerKeyProtector -MountPoint "C:" -KeyProtectorId (
    (Get-BitLockerVolume -MountPoint "C:").KeyProtector |
    Where-Object { $_.KeyProtectorType -eq 'RecoveryPassword' } |
    Select-Object -First 1 -ExpandProperty KeyProtectorId
)
```

**Prévention** : procédure écrite « toute MàJ BIOS = Suspend-BitLocker avant », affichée dans l'atelier.

---

## 45. Cas pratique 4 : mise à jour Windows bloquée / échec répété

**Symptômes** : Windows Update tourne en boucle, erreur 0x800700xx, ou « échec de l'installation » à chaque Patch Tuesday.

**Diagnostic** :

```powershell
# 1. Code d'erreur exact
Get-WinEvent -FilterHashtable @{
    LogName='System'; ProviderName='Microsoft-Windows-WindowsUpdateClient'; Level=2; StartTime=(Get-Date).AddDays(-3)
} | Select-Object TimeCreated, Id, Message | Format-List

# 2. Espace disque (cause n°1 !)
Get-PSDrive C | Select-Object Free, Used

# Codes fréquents :
# 0x80070070 / 0x80070002 : espace disque / fichiers corrompus
# 0x80240034             : téléchargement interrompu
# 0x800F0922             : partition EFI / de récupération trop petite
# 0x80070005             : droits d'accès
```

**Réparation** (dans l'ordre) :

```powershell
# Étape 1 : utilitaire de résolution des problèmes (Paramètres → Dépannage)
# Étape 2 : réinitialiser les composants Windows Update
net stop wuauserv
net stop cryptSvc
net stop bits
net stop msiserver
ren C:\Windows\SoftwareDistribution SoftwareDistribution.old
ren C:\Windows\System32\catroot2 catroot2.old
net start wuauserv
net start cryptSvc
net start bits
net start msiserver

# Étape 3 : réparer l'image puis relancer
dism /Online /Cleanup-Image /RestoreHealth
sfc /scannow

# Étape 4 : installer la KB manuellement (catalogue Microsoft Update)
# Télécharger le .msu correspondant puis :
wusa.exe C:\Admin\windows11.0-kb50xxxxx-x64.msu /quiet /norestart
```

**Désinstaller une KB fautive** :

```powershell
wusa /uninstall /kb:5034123 /quiet /norestart
# Puis mettre en pause les MàJ le temps de l'analyse (section 26.2)
```

**Vérification** : `Paramètres → Windows Update → Historique des mises à jour` = « Installation réussie », puis un cycle complet le mois suivant.

---

(Bloc 3/6 — sections 31 à 45)

---

## 46. Cas pratique 5 : mise à niveau 23H2 → 24H2 qui échoue

**Symptômes** : la feature update échoue à un pourcentage fixe (ex. 71 %), rollback vers 23H2.

**Diagnostic** :

```powershell
# 1. Lancer SetupDiag juste après l'échec (section 7.4)
.\SetupDiag.exe /Output:C:\Admin\SetupDiag.log
# Chercher "Error" / "Last Phase" (ex. : Safe OS, First Boot)

# 2. Lire setuperr.log
Get-Content "C:\`$Windows.~BT\Sources\Panther\setuperr.log" -Tail 30

# 3. Vérifier l'espace et les pilotes
# Erreur 0xC1900101-0x30018 = pilote ; 0xC1900208 = application incompatible
```

**Résolution** :

```
1. Mettre à jour le BIOS/UEFI et les pilotes chipset/stockage (constructeur)
2. Débrancher TOUT les périphériques USB non essentiels (dongles, disques externes)
3. Désinstaller l'antivirus tiers (temporairement)
4. Suspendre BitLocker : Suspend-BitLocker -MountPoint "C:" -RebootCount 1
5. Relancer via l'ISO : setup.exe /auto upgrade /dynamicupdate enable
6. Si échec persistant : installation propre + USMT/OneDrive (section 21)
   plutôt que de s'acharner sur une 3e tentative
```

**Vérification** : `winver` = 24H2 (build 26100+), applications métier testées, BitLocker réactivé (`Resume-BitLocker` + vérifier `ProtectionStatus`).

---

## 47. Cas pratique 6 : pilote défectueux — écran bleu, rollback, signature

**Symptômes** : BSOD après MàJ d'un pilote (souvent GPU, Wi-Fi, stockage), code `DRIVER_IRQL_NOT_LESS_OR_EQUAL`, `INACCESSIBLE_BOOT_DEVICE`.

**Diagnostic** :

```powershell
# 1. Identifier le pilote fautif (fichier .sys dans le BSOD ou le dump)
# Analyser le minidump avec WinDbg ou BlueScreenView (NirSoft)
# Emplacement : C:\Windows\Minidump\*.dmp

# 2. Voir l'historique des pilotes
Get-WinEvent -FilterHashtable @{LogName='System'; Id=20001,20003; StartTime=(Get-Date).AddDays(-7)} |
    Select-Object TimeCreated, Message | Format-List
```

**Résolution** :

```powershell
# Option A : restaurer le pilote précédent (si Windows démarre, même en mode sans échec)
# Gestionnaire de périphériques → Propriétés → Pilote → "Restaurer le pilote"
# En PowerShell (via pnputil) :
pnputil /enum-drivers | Select-String "oem" -Context 2

# Option B : désinstaller le pilote fautif depuis WinRE
# Identifier l'oemXX.inf du pilote, puis :
dism /Image:C:\ /Remove-Driver /Driver:oem12.inf

# Option C : démarrer en mode sans échec (pilotes minimaux)
bcdedit /set {default} safeboot minimal
# (puis `bcdedit /deletevalue {default} safeboot` pour revenir)

# Option D : bloquer la réinstallation auto du pilote fautif par Windows Update
# GPO : "Ne pas inclure les pilotes avec les mises à jour Windows" (section 26.3)
```

**Signature des pilotes** : Windows 11 exige des pilotes **signés**. Désactiver l'obligation de signature (`bcdedit /set nointegritychecks on`) = **à proscrire** en production (casse HVCI et Secure Boot).

---

## 48. Cas pratique 7 : performances dégradées — disque, mémoire, processus

**Symptômes** : lenteurs générales, ventilateur à fond, disque à 100 %.

**Diagnostic** (dans l'ordre) :

```powershell
# 1. Qui consomme ? (top 10 CPU / mémoire)
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10 Name, CPU, WorkingSet
Get-Process | Sort-Object WorkingSet64 -Descending | Select-Object -First 10 Name,
    @{N='RAM(Mo)';E={[math]::Round($_.WorkingSet64/1MB)}}

# 2. Disque : file d'attente et temps de réponse
Get-Counter '\PhysicalDisk(_Total)\% Disk Time', '\PhysicalDisk(_Total)\Avg. Disk Queue Length'

# 3. Démarrage : qu'est-ce qui se lance ?
Get-CimInstance Win32_StartupCommand | Select-Object Name, Command, Location

# 4. Santé du disque (SMART) — via CrystalDiskInfo ou :
Get-PhysicalDisk | Select-Object FriendlyName, MediaType, HealthStatus, OperationalStatus
```

**Résolutions typiques** :

| Cause | Action |
|---|---|
| Antivirus tiers + Defender en conflit | Un seul antivirus actif ; désinstaller l'autre |
| Indexation / SysMain sur HDD | Passer au SSD (la vraie solution) |
| Trop d'apps au démarrage | Désactiver via Gestionnaire des tâches → Démarrage |
| Mémoire saturée (8 Go + Teams + Edge) | Ajouter de la RAM ou limiter les onglets/processus |
| Disque système plein à >90 % | Nettoyage (cas pratique 14) |
| Profil utilisateur énorme | Nettoyer / recréer le profil |

```powershell
# Désactiver proprement SysMain (ex-Superfetch) si pertinent — rarement utile sur SSD
Stop-Service SysMain -Force
Set-Service SysMain -StartupType Disabled

# Analyser le démarrage avec Autoruns (Sysinternals) — à faire en admin
```

---

## 49. Cas pratique 8 : profil utilisateur corrompu / temporaire

**Symptômes** : « Vous avez été connecté avec un profil temporaire », bureau vide, paramètres perdus.

**Diagnostic** :

```powershell
# 1. Vérifier l'état du profil dans le registre
Get-ChildItem "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList" |
    ForEach-Object {
        $p = Get-ItemProperty $_.PSPath
        [pscustomobject]@{
            SID        = $_.PSChildName
            Path       = $p.ProfileImagePath
            State      = $p.State          # 0 = OK
            RefCount   = $p.RefCount
            Temporaire = ($_.PSChildName -like "*.bak") -or ($p.State -ne 0)
        }
    } | Format-Table -AutoSize

# Signes typiques : une clé SID.bak + une clé SID "neuve" pointant vers C:\Users\TEMP
```

**Résolution** :

```
1. Sauvegarder les données du profil corrompu (C:\Users\<nom>\Desktop, Documents...)
   — même s'il semble vide, vérifier via un compte admin
2. Déconnecter l'utilisateur (pas de session verrouillée !)
3. Dans le registre : supprimer la clé SID.bak ET la clé SID temporaire
4. Supprimer/renommer C:\Users\<nom> (ou le laisser, Windows recréera)
   → méthode propre : Remove-CimInstance sur Win32_UserProfile (section 19.3)
5. L'utilisateur se reconnecte → nouveau profil sain
6. Restaurer ses données (pas tout le profil : uniquement les dossiers utiles)
```

> ⛔ **Ne jamais** « réparer » en renommant `.bak` à la main sans comprendre : si le `NTUSER.DAT` est corrompu, le profil replantera. Recréation propre + restauration sélective des données = plus fiable.

---

## 50. Cas pratique 9 : impossible de joindre le domaine / relation d'approbation

**Symptômes** : « La relation d'approbation entre cette station et le domaine a échoué ».

**Causes** : mot de passe du compte machine désynchronisé (restauration d'image, clone, poste éteint > 30 jours, double SID).

**Résolution** (sans quitter/réintégrer le domaine si possible) :

```powershell
# Option A : réinitialiser le mot de passe du compte machine (le poste doit voir un DC)
Reset-ComputerMachinePassword -Server "DC01.entreprise.local" -Credential (Get-Credential)
Restart-Computer

# Option B : quitter puis rejoindre (si A échoue)
Remove-Computer -UnjoinDomainCredential (Get-Credential) -Restart
# après redémarrage :
Add-Computer -DomainName "entreprise.local" -OUPath "OU=Postes Win11,DC=entreprise,DC=local" -Credential (Get-Credential) -Restart
```

**Diagnostic réseau préalable** (50 % des « échecs de jointure » sont du DNS) :

```powershell
# Le poste résout-il le domaine ? Le DC est-il joignable ?
Resolve-DnsName entreprise.local
Test-NetConnection DC01.entreprise.local -Port 389
nltest /dsgetdc:entreprise.local
# L'heure est-elle synchronisée ? (Kerberos tolère ±5 min !)
w32tm /query /status
```

---

## 51. Cas pratique 10 : GPO qui ne s'applique pas

**Symptômes** : le réglage n'est pas effectif malgré la GPO liée.

**Checklist de diagnostic** (dans l'ordre) :

```powershell
# 1. La GPO est-elle reçue ?
gpresult /r
# → Chercher la GPO dans "Objets de stratégie de groupe appliqués"
# → Si absente : filtrage, liaison, OU, réplication ?

# 2. Détail complet
gpresult /h C:\Admin\gpresult.html

# 3. Le poste est-il dans la bonne OU ?
Get-ADComputer $env:COMPUTERNAME | Select-Object DistinguishedName

# 4. La GPO est-elle liée ET activée ? (console GPMC)
# Vérifier : lien activé, GPO activée (pas "désactivée"), pas de "Block Inheritance" au-dessus

# 5. Filtrage de sécurité : le groupe "Ordinateurs authentifiés" a-t-il "Lire" + "Appliquer" ?
# (depuis la KB3163622, le filtrage par groupe exige ces droits — cause classique !)

# 6. Filtre WMI : s'évalue-t-il à vrai ?
Get-CimInstance -Query 'SELECT * FROM Win32_OperatingSystem WHERE Version LIKE "10.0.26%"'

# 7. Forcer + journal
gpupdate /force
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-GroupPolicy/Operational'; Level=2; StartTime=(Get-Date).AddDays(-1)} |
    Select-Object -First 5 TimeCreated, Id, Message
```

**Causes top 5** : filtrage de sécurité mal configuré (droits manquants) · héritage bloqué · GPO liée à la mauvaise OU · réplication SYSVOL incomplète · boucle de traitement (loopback) non configurée pour les GPO utilisateur sur postes partagés.

---

## 52. Cas pratique 11 : Wi-Fi 802.1X — échec d'authentification

**Symptômes** : « Impossible de se connecter à ENTREPRISE », ou connexion puis déconnexion en boucle.

**Diagnostic** :

```powershell
# 1. Le profil est-il correct ?
netsh wlan show profile name="ENTREPRISE"

# 2. Journaux WLAN (très verbeux, très utiles)
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-WLAN-AutoConfig/Operational'; Level=2; StartTime=(Get-Date).AddDays(-1)} |
    Select-Object TimeCreated, Id, Message | Format-List

# 3. Le certificat machine est-il présent et valide ? (EAP-TLS)
Get-ChildItem Cert:\LocalMachine\My | Where-Object { $_.NotAfter -gt (Get-Date) } |
    Select-Object Subject, NotAfter

# 4. L'heure est-elle correcte ? (EAP-TLS échoue si l'horloge dérive)
w32tm /query /status
```

**Résolutions** :

| Cause | Action |
|---|---|
| Certificat expiré / absent | Renouveler via auto-enrollment (`certutil -pulse`), vérifier le modèle AD CS |
| Serveur RADIUS non approuvé | Ajouter le nom du certificat NPS dans le profil (serveurs de confiance) |
| Identifiants PEAP erronés | Effacer les identifiants mémorisés : `netsh wlan delete profile name="ENTREPRISE"` puis re-déployer |
| Heure désynchronisée | Forcer la synchro NTP |
| Pilote Wi-Fi | Mettre à jour (constructeur, pas Windows Update) |

---

## 53. Cas pratique 12 : VPN qui ne se connecte pas

**Symptômes** : erreur 800/809/13801/691 sur la connexion VPN.

| Code | Signification | Piste |
|---|---|---|
| 800 | Serveur injoignable | Réseau, pare-feu, nom DNS |
| 809 | Bloqué par NAT/pare-feu | Ports UDP 500/4500, activer le NAT-T |
| 13801 | Échec IKEv2 (certificat) | Certificat serveur (SAN), chaîne de confiance |
| 691 | Identifiants refusés | Login/mot de passe, compte verrouillé |
| 942 | Certificat machine absent | Auto-enrollment, modèle de certificat |

```powershell
# Diagnostic
Get-VpnConnection | Select-Object Name, ConnectionStatus, TunnelType, ServerAddress
# Journaux RRAS côté client :
Get-WinEvent -FilterHashtable @{LogName='Application'; ProviderName='RasClient'; StartTime=(Get-Date).AddDays(-1)} |
    Select-Object TimeCreated, Id, Message | Format-List

# Tester la connectivité vers le serveur VPN
Test-NetConnection vpn.entreprise.example -Port 443
Resolve-DnsName vpn.entreprise.example
```

> 💡 En télétravail, 80 % des tickets VPN sont : Wi-Fi domestique instable, heure désynchronisée, ou certificat expiré. Vérifiez ces trois points avant d'ouvrir la console RRAS.

---

## 54. Cas pratique 13 : applications qui ne se lancent plus / Store corrompu

**Symptômes** : les applications du Store (ou toutes les UWP) plantent au lancement, le Store ne s'ouvre pas.

**Résolution** (dans l'ordre, du moins au plus invasif) :

```powershell
# 1. Réparer le Store
wsreset.exe   # vide le cache du Store (fenêtre noire, attendre la fin)

# 2. Réinscrire les applications pour l'utilisateur courant
Get-AppXPackage -AllUsers | ForEach-Object {
    Add-AppxPackage -DisableDevelopmentMode -Register "$($_.InstallLocation)\AppXManifest.xml"
}

# 3. Réparer une application précise (ex. : Calculatrice)
Get-AppxPackage *calculator* | Reset-AppxPackage

# 4. Vérifier l'intégrité système
dism /Online /Cleanup-Image /RestoreHealth
sfc /scannow

# 5. Si un profil est en cause : tester avec un nouvel utilisateur local
# (si ça marche avec le nouvel utilisateur → profil corrompu, voir cas n°8)
```

---

## 55. Cas pratique 14 : espace disque saturé après mise à niveau

**Symptômes** : C: plein à 95 %+ après une feature update (`Windows.old` = 20-30 Go).

**Résolution** :

```powershell
# 1. Voir ce qui prend de la place
Get-PSDrive C | Select-Object Used, Free
# Dossiers suspects :
Get-ChildItem C:\ -Directory | ForEach-Object {
    $size = (Get-ChildItem $_.FullName -Recurse -Force -ErrorAction SilentlyContinue |
             Measure-Object Length -Sum).Sum / 1GB
    [pscustomobject]@{ Dossier=$_.Name; Go=[math]::Round($size,1) }
} | Sort-Object Go -Descending | Select-Object -First 10

# 2. Nettoyage système (lance l'assistant)
cleanmgr.exe /sageset:1
cleanmgr.exe /sagerun:1

# 3. Supprimer Windows.old APRÈS validation (10 jours de rollback par défaut !)
# Paramètres → Système → Stockage → Fichiers temporaires → cocher "Version précédente de Windows"
# En ligne de commande :
dism /Online /Cleanup-Image /StartComponentCleanup /ResetBase
# /ResetBase : irréversible, supprime la possibilité de désinstaller les MàJ — à n'utiliser
# qu'après validation complète du poste

# 4. Recommandation disque (Storage Sense) : activer par GPO
$ss = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\StorageSense"
New-Item -Path $ss -Force | Out-Null
Set-ItemProperty -Path $ss -Name "AllowStorageSenseGlobal" -Value 1 -Type DWord
```

> ⚠️ Ne supprimez **jamais** `Windows.old` manuellement à la main dans l'Explorateur (droits système) : utilisez l'outil de nettoyage de disque. Et attendez la fin de la période de rollback (10 jours) avant `/ResetBase`.

---

## 56. Cas pratique 15 : Windows Hello / biométrie ne fonctionne plus

**Symptômes** : le PIN ou l'empreinte ne fonctionne plus après une MàJ ou un changement de domaine.

**Diagnostic** :

```powershell
# 1. Le TPM est-il sain ? (Windows Hello repose dessus)
Get-Tpm | Select-Object TpmReady, TpmEnabled, LockedOut

# 2. État de l'inscription Hello
# Paramètres → Comptes → Options de connexion → tout est grisé ?
# → souvent lié à une stratégie (désactivé par GPO) ou au PIN "oublié" par le TPM
```

**Résolution** :

```
1. Supprimer puis recréer le PIN : Paramètres → Comptes → Options de connexion
   → Code PIN → Supprimer → Redémarrer → Reconfigurer
2. Si le TPM a été effacé : la clé Hello est perdue → recréation obligatoire
   (c'est normal, c'est le principe de sécurité)
3. Pilote du lecteur d'empreinte : réinstaller depuis le constructeur
4. En entreprise : vérifier la GPO "Utiliser Windows Hello Entreprise"
   (nécessite une infrastructure : certificats ou clés, selon le modèle)
```

```powershell
# Réinitialiser le conteneur NGC (base des PIN Hello) — méthode radicale :
# 1. Prendre possession de C:\Windows\ServiceProfiles\LocalService\AppData\Local\Microsoft\NGC
# 2. Supprimer son contenu, redémarrer, recréer le PIN
takeown /f "C:\Windows\ServiceProfiles\LocalService\AppData\Local\Microsoft\NGC" /r /d y
icacls "C:\Windows\ServiceProfiles\LocalService\AppData\Local\Microsoft\NGC" /grant administrateurs:F /t
Remove-Item "C:\Windows\ServiceProfiles\LocalService\AppData\Local\Microsoft\NGC\*" -Recurse -Force
```

---

## 57. Cas pratique 16 : imprimante réseau / partage SMB inaccessible

**Symptômes** : « Windows ne peut pas accéder à \\SRV\partage », erreur 0x80070035, imprimante hors ligne.

**Diagnostic** :

```powershell
# 1. Résolution de noms
Resolve-DnsName srv-fichiers.entreprise.local
nbtstat -a SRV-FICHIERS   # NetBIOS (si encore utilisé)

# 2. Connectivité SMB
Test-NetConnection srv-fichiers.entreprise.local -Port 445

# 3. Version SMB négociée (doit être 3.x)
Get-SmbConnection | Select-Object ServerName, Dialect, NumOpens

# 4. Identifiants : avec quel compte suis-je authentifié ?
klist   # tickets Kerberos
# Tester avec des identifiants explicites :
net use \\srv-fichiers\partage /user:ENTREPRISE\jdupont *
```

**Causes fréquentes** :

| Cause | Indice | Solution |
|---|---|---|
| SMBv1 désactivé côté client, serveur ancien | Dialect vide / erreur | Mettre à jour le serveur (pas réactiver SMBv1 !) |
| Signature SMB obligatoire d'un côté | Négociation échouée | Aligner les GPO des deux côtés |
| Pare-feu (client ou serveur) | Port 445 filtré | Règle pare-feu (section 35) |
| Point and Print restreint (imprimantes) | Pilote refusé | GPO Point and Print (section 66) |
| Horloge désynchronisée | Kerberos échoue | w32tm /resync |

---

## 58. Fin de vie de Windows 10 (14/10/2025) : stratégie de migration

> 📌 Au 27/09/2026, Windows 10 est **hors support depuis près d'un an**. Les postes Windows 10 restants ne reçoivent plus de correctifs de sécurité (hors programme ESU payant). Chaque poste Windows 10 du parc est désormais un **risque documenté**.

### 58.1 Les 3 options pour les postes Windows 10 restants

| Option | Quand | Coût |
|---|---|---|
| **Migrer vers Windows 11** | Poste compatible (TPM 2.0, CPU listé) | Temps IT + validation |
| **Remplacer le poste** | Poste incompatible (< 8e gén, pas de TPM 2.0) | Achat |
| **ESU (Extended Security Updates)** | Temporisation < 12 mois, contrainte métier bloquante | Payant / poste / an, **prix croissant** |

> ⛔ L'ESU n'est **pas** une stratégie : c'est un pansement payant pour gagner du temps. Tout poste sous ESU doit avoir une date de sortie écrite.

### 58.2 Plan de migration en 5 phases

```
Phase 1 — INVENTAIRE (2-4 semaines)
  → Script de compatibilité (section 3.2) sur 100 % du parc
  → Classer : Compatible / Compatible après action (TPM, MBR2GPT) / Incompatible
  → Lier avec GLPI (section 59)

Phase 2 — PILOTE (4-6 semaines)
  → 5-10 % du parc, volontaires + IT
  → Valider : image/MDT ou Autopilot, GPO, applications métier, pilotes, BitLocker
  → Documenter les problèmes et leurs solutions

Phase 3 — VAGUES (8-16 semaines)
  → Par service / site, 15-25 % du parc par vague
  → Communication : 2 semaines avant, rappel J-3, support renforcé J+3
  → Fenêtre de rollback de 10 jours par poste (ne pas forcer /ResetBase trop tôt)

Phase 4 — RÉSIDUEL (4 semaines)
  → Postes problématiques : remplacement ou ESU avec date de sortie
  → Zéro poste Windows 10 "oublié"

Phase 5 — CLÔTURE
  → Rapport : 100 % migré ou plan daté pour chaque exception
  → Archiver les images/MDT Windows 10, couper les GPO spécifiques
```

### 58.3 Communication type (à adapter)

> « Votre poste sera migré vers Windows 11 le [date]. Sauvegardez vos fichiers sur OneDrive (déjà actif). La migration dure ~1 h, vos fichiers et applications sont conservés. En cas de problème : [numéro du support]. »

---

## 59. Inventaire du parc : PowerShell, GLPI, Intune, rapports

### 59.1 Inventaire PowerShell (sans agent)

```powershell
<#
.SYNOPSIS
    Inventaire complet d'un poste : OS, matériel, logiciels, BitLocker, MàJ.
#>
$inv = [ordered]@{}
$os  = Get-CimInstance Win32_OperatingSystem
$cs  = Get-CimInstance Win32_ComputerSystem
$inv.Hostname     = $env:COMPUTERNAME
$inv.OS           = $os.Caption
$inv.Build        = (Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion").CurrentBuild
$inv.UBR          = (Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion").UBR
$inv.InstallDate  = $os.InstallDate
$inv.Manufacturer = $cs.Manufacturer
$inv.Model        = $cs.Model.Trim()
$inv.Serial       = (Get-CimInstance Win32_BIOS).SerialNumber.Trim()
$inv.CPU          = (Get-CimInstance Win32_Processor).Name.Trim()
$inv.RAM_GB       = [math]::Round($cs.TotalPhysicalMemory / 1GB, 1)
$inv.DiskFree_GB  = [math]::Round((Get-CimInstance Win32_LogicalDisk -Filter "DeviceID='C:'").FreeSpace / 1GB, 1)
$inv.TPM2         = ((Get-CimInstance -Namespace root/cimv2/security/microsofttpm -ClassName Win32_Tpm -ErrorAction SilentlyContinue).SpecVersion -split ',')[0]
$inv.SecureBoot   = try { Confirm-SecureBootUEFI } catch { 'N/A' }
$inv.BitLocker    = (Get-BitLockerVolume -MountPoint "C:" -ErrorAction SilentlyContinue).ProtectionStatus
$inv.Domain       = $cs.Domain
$inv.LastBoot     = $os.LastBootUpTime

# Logiciels installés (registre Uninstall — rapide, sans WMI lent)
$inv.AppsCount = (Get-ChildItem "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall" |
                  Where-Object { (Get-ItemProperty $_.PSPath -ErrorAction SilentlyContinue).DisplayName }).Count

[pscustomobject]$inv | Export-Csv "\\SRV-FICHIERS\Inventaire$\INV_$($env:COMPUTERNAME).csv" -NoTypeInformation -Encoding UTF8
```

### 59.2 Lier avec GLPI

GLPI + **agent GLPI** (ou FusionInventory) sur chaque poste = inventaire automatique (matériel, logiciels, licences) remonté au serveur GLPI.

Points d'intégration Windows 11 :

- Déployer l'agent via **GPO / MDT / Intune** (MSI silencieux) :

```powershell
# Installation silencieuse de l'agent GLPI (exemple)
msiexec /i glpi-agent-1.x-x64.msi /quiet SERVER=https://glpi.entreprise.local `
    RUNNOW=1 INSTALLTYPE=from-scratch
```

- Planifier l'inventaire (tâche planifiée quotidienne ou au démarrage).
- Croiser avec le script de compatibilité (section 3.2) : champ personnalisé « Compatible Win11 » dans GLPI.
- Utiliser les **rapports GLPI** pour piloter les vagues de migration (section 58.2).

### 59.3 Intune comme source d'inventaire (parc cloud)

Intune → Appareils → exporter (CSV) : modèle, OS, version, conformité, chiffrement, dernier check-in. Compléter avec des **requêtes de conformité** personnalisées (ex. : build minimale 26100).

---

## 60. Supervision et journaux : Event Viewer, journaux clés, collecte

### 60.1 Collecte centralisée : les options

| Solution | Coût | Remarque |
|---|---|---|
| Abonnement aux journaux (Event Forwarding natif) | Gratuit | WEF : collecteur + GPO d'abonnement, parfait pour démarrer |
| Zabbix / agent | Gratuit | Surveillance + inventaire, pas d'analyse de logs poussée |
| Wazuh | Gratuit | SIEM léger, excellent pour les logs Windows |
| Microsoft Sentinel | Payant (ingestion) | SIEM cloud, intégration Entra/Defender native |

### 60.2 Windows Event Forwarding (WEF) — mise en place express

```powershell
# Sur le COLLECTEUR :
# 1. Configurer le service :
winrm quickconfig -q
# 2. Créer l'abonnement (console eventvwr → Abonnements → Créer) :
#    - Source : ordinateurs du domaine (via GPO)
#    - Journaux : System, Application, Security (filtré), BitLocker-Operational

# Sur les POSTES (via GPO) :
# Configuration ordinateur\Modèles d'administration\Composants Windows\Transfert d'événements
# → "Configurer l'adresse du serveur cible" : FQDN du collecteur
# + winrm : Set-Item WSMan:\localhost\Client\TrustedHosts -Value "collecteur.entreprise.local"
```

### 60.3 Événements à surveiller en priorité (alertes)

| ID | Journal | Signification |
|---|---|---|
| 4625 | Security | Échec d'ouverture de session (bruteforce ?) |
| 4672 | Security | Privilèges spéciaux attribués |
| 4720/4728 | Security | Création de compte / ajout à un groupe sensible |
| 1001 | System | BugCheck (écran bleu) |
| 6008 | System | Arrêt inattendu |
| 1116 | Defender/Operational | Malware détecté |
| 1014 | System (DNS Client) | Échec de résolution (réseau/DNS) |
| 8198/8200 | BitLocker-Operational | Échec de chiffrement |

---

(Bloc 4/6 — sections 46 à 60)

---

## 61. Scripts utiles : boîte à outils PowerShell de l'admin

### 61.1 Audit express d'un poste (à lancer en admin)

```powershell
<#
.SYNOPSIS
    Audit express : affiche l'état de santé/sécurité d'un poste Windows 11.
#>
Write-Host "=== AUDIT $($env:COMPUTERNAME) ===" -ForegroundColor Cyan
$ver = Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion"
Write-Host "OS      : $($ver.ProductName) $($ver.DisplayVersion) (build $($ver.CurrentBuild).$($ver.UBR))"
Write-Host "Installé: $((Get-CimInstance Win32_OperatingSystem).InstallDate)"
Write-Host "Dernier boot : $((Get-CimInstance Win32_OperatingSystem).LastBootUpTime)"
Write-Host ""
Write-Host "--- Sécurité ---" -ForegroundColor Yellow
Write-Host "TPM 2.0 prêt : $((Get-Tpm).TpmReady)"
Write-Host "Secure Boot : $(try { Confirm-SecureBootUEFI } catch { 'N/A' })"
$bl = Get-BitLockerVolume -MountPoint "C:"
Write-Host "BitLocker   : $($bl.ProtectionStatus) ($($bl.VolumeStatus), $($bl.EncryptionPercentage)%)"
$mp = Get-MpComputerStatus
Write-Host "Defender    : AV=$($mp.AntivirusEnabled) TempsRéel=$($mp.RealTimeProtectionEnabled)"
Write-Host "Signatures  : $($mp.AntivirusSignatureLastUpdated)"
Write-Host "LAPS (stratégie) : $((Get-ItemProperty 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\LAPS' -ErrorAction SilentlyContinue) -ne $null)"
Write-Host ""
Write-Host "--- Réseau ---" -ForegroundColor Yellow
Get-NetIPAddress -AddressFamily IPv4 | Where-Object { $_.InterfaceAlias -notlike "*Loopback*" } |
    ForEach-Object { Write-Host "$($_.InterfaceAlias) : $($_.IPAddress)" }
Write-Host "Domaine/Entra : $(dsregcmd /status | Select-String 'AzureAdJoined|DomainJoined')"
Write-Host ""
Write-Host "--- Disque ---" -ForegroundColor Yellow
Get-PSDrive C | Select-Object @{N='Libre(Go)';E={[math]::Round($_.Free/1GB,1)}},
    @{N='Utilisé(Go)';E={[math]::Round($_.Used/1GB,1)}} | Format-Table -AutoSize
```

### 61.2 Redémarrage planifié + notification (maintenance)

```powershell
# Redémarrer un parc à heure fixe avec avertissement
$computers = Get-Content C:\Admin\postes-vague2.txt
foreach ($pc in $computers) {
    shutdown /m \\$pc /r /t 600 /c "Maintenance planifiée : redémarrage dans 10 minutes. Sauvegardez votre travail."
}
# Annuler si besoin :
# shutdown /m \\$pc /a
```

### 61.3 Nettoyage à distance (disque plein)

```powershell
# Nettoyer les profils inutilisés depuis plus de 90 jours (à distance)
Invoke-Command -ComputerName "PC-COMPTA-042" -ScriptBlock {
    Get-CimInstance Win32_UserProfile | Where-Object {
        -not $_.Special -and $_.LastUseTime -lt (Get-Date).AddDays(-90) -and -not $_.Loaded
    } | ForEach-Object {
        Write-Output "Suppression : $($_.LocalPath)"
        Remove-CimInstance -InputObject $_
    }
    # Vider les temporaires Windows
    Remove-Item "C:\Windows\Temp\*" -Recurse -Force -ErrorAction SilentlyContinue
}
```

### 61.4 Forcer l'inventaire / la remontée Intune

```powershell
# Forcer la synchronisation Intune (côté client)
# Via le planificateur : tâche "PushLaunch" du fournisseur MDM, ou :
Get-ScheduledTask -TaskName "PushLaunch" -TaskPath "\Microsoft\Windows\EnterpriseMgmt\*" |
    Start-ScheduledTask
```

---

## 62. Sécurité avancée : Secure Boot, measured boot, attestation

### 62.1 Chaîne de confiance au démarrage

```
UEFI Secure Boot  →  vérifie la signature du bootloader
   → Measured Boot →  le TPM "mesure" chaque étape (hash dans les PCR)
      → BitLocker  →  ne déchiffre que si les mesures sont conformes
         → HVCI    →  le noyau ne charge que du code signé
            → ELAM →  l'antimalware démarre en premier
```

### 62.2 Vérifier l'état de la chaîne

```powershell
# Secure Boot + VBS (rappel section 32)
Confirm-SecureBootUEFI
Get-CimInstance -ClassName Win32_DeviceGuard -Namespace root\Microsoft\Windows\DeviceGuard

# Journal du démarrage mesuré (TPM)
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-TPM-WMI/Operational'; StartTime=(Get-Date).AddDays(-1)} |
    Select-Object -First 5 TimeCreated, Id, Message
```

### 62.3 Attestation (flottes critiques)

L'**attestation d'intégrité** (Microsoft Intune / Azure Attestation) permet de vérifier à distance que le démarrage d'un poste est sain avant de lui donner accès (accès conditionnel : « exiger un appareil sain »).

```powershell
# Vérifier que l'attestation est configurée (registre, poste Entra join)
Get-ItemProperty "HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard" -Name "Attestation" -ErrorAction SilentlyContinue
```

> 💡 Pour un chef de service : l'attestation + accès conditionnel = « les postes non sains n'accèdent pas aux données sensibles ». C'est le niveau au-dessus du simple antivirus.

---

## 63. Chiffrement avancé : BitLocker réseau (Network Unlock), DRA

### 63.1 BitLocker Network Unlock

Permet le **déchiffrement automatique au boot** pour les postes **filaire sur le LAN d'entreprise** (sans PIN), tout en exigeant le PIN hors réseau.

Prérequis :

- Serveurs WDS avec le rôle Network Unlock ;
- Certificat (clé publique) déployé sur les clients via GPO ;
- DHCP avec option 066/067 vers le WDS ;
- Postes avec protecteur **TPM+PIN** (le Network Unlock ajoute un protecteur réseau).

```powershell
# Côté client : ajouter le protecteur Network Unlock (après config serveur)
Add-BitLockerKeyProtector -MountPoint "C:" -NetworkUnlockProtector
```

### 63.2 Agent de récupération de données (DRA)

Le DRA est un certificat dont la clé privée permet de déchiffrer **n'importe quel volume** de l'organisation : c'est le « passe-partout » ultime.

```powershell
# Créer un DRA (sur une machine isolée, à conserver HORS LIGNE) :
cipher /R:C:\Admin\DRA-Entreprise
# → génère DRA-Entreprise.cer (public, à déployer par GPO) et DRA-Entreprise.pfx (PRIVÉ !)

# GPO : Composants Windows\BitLocker\Lecteurs du système d'exploitation
# → "Choisir comment les lecteurs chiffrés peuvent être récupérés"
# → ajouter le certificat DRA
```

> 🔐 **Règles d'or du DRA** : clé privée sur support amovible chiffré, stockée en coffre-fort, jamais sur le réseau. Deux personnes minimum connaissent son existence (anti-« bus factor »), mais une seule détient le support. Documenter la procédure d'usage d'urgence.

---

## 64. Conformité : stratégies de conformité Intune, base de référence

### 64.1 Stratégie de conformité type (Windows 11)

Intune → Appareils → **Conformité** → créer une stratégie Windows 11 :

| Paramètre | Valeur |
|---|---|
| Version minimale de l'OS | 10.0.26100 (24H2) |
| Chiffrement BitLocker requis | Oui |
| Antivirus requis / à jour | Oui |
| Pare-feu requis | Oui |
| Mot de passe requis (complexité) | Oui |
| Intégrité (attestation) | Selon criticité |
| Action en cas de non-conformité | Marquer non conforme → accès conditionnel bloque |

### 64.2 Accès conditionnel (le bras armé)

Entra ID → Protection → Accès conditionnel :

```
SI  (utilisateur du groupe "Tous") 
ET  (application = "Office 365" / "Toutes les applications cloud")
ALORS exiger : appareil conforme ET/OU appareil joint (hybride/Entra)
```

> ⚠️ **Toujours** tester une stratégie d'accès conditionnel en mode « Rapport seul » avant de l'appliquer, et prévoir un **compte d'urgence (break glass)** exclu de toutes les stratégies.

### 64.3 Rapports de conformité

```powershell
# Via Microsoft Graph : lister les appareils non conformes
# (nécessite le module Microsoft.Graph + consentement DeviceManagementManagedDevices.Read.All)
Connect-MgGraph -Scopes "DeviceManagementManagedDevices.Read.All"
Get-MgDeviceManagementManagedDevice -Filter "complianceState eq 'noncompliant'" |
    Select-Object DeviceName, OperatingSystem, OSVersion, ComplianceState, LastSyncDateTime
```

---

## 65. Kiosque et postes partagés : Assigned Access, invité

### 65.1 Assigned Access (kiosque mono-application)

Verrouille le poste sur **une seule application** (UWP ou navigateur) :

```powershell
# Configurer un kiosque (ex. : Edge sur un portail)
Set-AssignedAccess -AppUserModelId "Microsoft.MicrosoftEdge.Stable_8wekyb3d8bbwe!App" -UserName "kiosque"
# Le compte "kiosque" doit exister en local (standard, pas admin)

# Pour un kiosque multi-applications : utiliser le XML de configuration
# (Set-AssignedAccess avec -ConfigFile, voir doc "CreateAssignedAccessSettings")
```

### 65.2 Postes partagés : bonnes pratiques

| Point | Réglage |
|---|---|
| Suppression auto des profils | GPO « Supprimer les profils utilisateurs datant de plus de X jours » |
| Compte invité | **Désactivé** (compte Invité intégré = à laisser désactivé) ; créer des comptes locaux nominatifs ou Entra |
| Verrouillage de session | Court (5 min) par GPO |
| Nettoyage à la fermeture | Script de déconnexion : vider Téléchargements, historique navigateur |

```powershell
# Supprimer les profils inactifs depuis plus de 30 jours (GPO équivalente)
$del = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\System"
New-Item -Path $del -Force | Out-Null
# (La GPO native "Supprimer les profils utilisateurs..." se configure dans :
#  Configuration ordinateur\Modèles d'administration\Système\Profils utilisateur)
```

---

## 66. Impression en entreprise : Universal Print, Point and Print

### 66.1 Universal Print (cloud)

Service d'impression cloud Microsoft 365 : les imprimantes compatibles (ou via un connecteur) sont publiées dans Entra, déployées par Intune, **sans serveur d'impression**.

```powershell
# Côté client : installer une imprimante Universal Print (Intune la pousse en général)
# Vérifier les imprimantes :
Get-Printer | Select-Object Name, DriverName, PortName, Shared
```

### 66.2 Point and Print — durcissement (depuis PrintNightmare)

```powershell
# Restreindre Point and Print aux serveurs approuvés (GPO recommandée)
$pp = "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint"
New-Item -Path $pp -Force | Out-Null
Set-ItemProperty -Path $pp -Name "RestrictDriverInstallationToAdministrators" -Value 1 -Type DWord
Set-ItemProperty -Path $pp -Name "TrustedServers" -Value 1 -Type DWord
Set-ItemProperty -Path $pp -Name "ServerList" -Value "srv-print1;srv-print2" -Type String
# + "PackagePointAndPrintOnly" = 1 (n'accepter que les pilotes packagés)
```

### 66.3 Dépannage impression (express)

```powershell
# Redémarrer le spouleur (résout 70 % des cas)
Restart-Service Spooler -Force

# Vider la file bloquée
Stop-Service Spooler -Force
Remove-Item "C:\Windows\System32\spool\PRINTERS\*" -Force
Start-Service Spooler

# Voir les files et les erreurs
Get-PrintJob -PrinterName "Imprimante-Compta" | Select-Object Id, JobStatus, UserName
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-PrintService/Admin'; Level=2; StartTime=(Get-Date).AddDays(-1)} |
    Select-Object -First 5 TimeCreated, Id, Message
```

---

## 67. Langues, régions, claviers : déploiement multilingue

### 67.1 Ajouter une langue en ligne de commande

```powershell
# Installer le pack de langue français (si l'image est en anglais)
$lp = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate"  # (via WU ou source locale)
# Méthode DISM (depuis le CAB de langue) :
dism /Online /Add-Package /PackagePath:C:\Langues\Microsoft-Windows-Client-Language-Pack_x64_fr-fr.cab

# Définir la langue d'affichage
Set-WinUILanguageOverride -Language fr-FR
Set-WinUserLanguageList fr-FR -Force
Set-WinSystemLocale fr-FR
Set-WinHomeLocation -GeoId 84   # 84 = France
```

### 67.2 Clavier et paramètres régionaux (image de référence / unattend)

```powershell
# Clavier FR par défaut pour tous les nouveaux utilisateurs
# Dans unattend.xml (passes specialize/oobeSystem) :
# <InputLocale>040c:0000040c</InputLocale>  → 040c = fr-FR

# Copier les paramètres régionaux actuels vers l'écran d'accueil et les nouveaux comptes
# Panneau de configuration → Région → Administration → "Copier les paramètres"
# Équivalent automatisé (sysprep / MDT) :
control.exe intl.cpl
```

### 67.3 Fuseau horaire

```powershell
# Lister / définir
Get-TimeZone -ListAvailable | Where-Object { $_.Id -like "*Romance*" }
Set-TimeZone -Id "Romance Standard Time"   # Paris (UTC+1, heure d'été gérée)
# Vérifier la synchro NTP
w32tm /query /status
w32tm /resync
```

---

## 68. Télémétrie et confidentialité : niveaux, GPO, registre

### 68.1 Niveaux de télémétrie (données de diagnostic)

| Niveau | Données envoyées | Usage |
|---|---|---|
| 0 — Sécurité (Entreprise/Education uniquement) | Minimales (sécurité) | **Recommandé entreprise** |
| 1 — Basique | + données appareil de base | Minimum Pro |
| 2 — Amélioré | + usage | — |
| 3 — Complet | Tout | Jamais en entreprise |

```powershell
# Forcer le niveau Sécurité (Entreprise uniquement, sinon minimum Basique)
$tele = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\DataCollection"
New-Item -Path $tele -Force | Out-Null
Set-ItemProperty -Path $tele -Name "AllowTelemetry" -Value 0 -Type DWord
# 0=Sécurité, 1=Basique, 2=Amélioré, 3=Complet
# + "LimitDiagnosticLogCollection" = 1, "LimitDumpCollection" = 1
```

### 68.2 Autres verrous de confidentialité

```powershell
# Désactiver la publicité ciblée (ID publicitaire)
New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\AdvertisingInfo" -Force | Out-Null
Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\AdvertisingInfo" -Name "DisabledByGroupPolicy" -Value 1 -Type DWord

# Désactiver les expériences personnalisées (déjà vu section 23, rappel)
# Désactiver l'accès des apps aux données sensibles (position, micro, caméra) par défaut :
# → GPO "Confidentialité" : "Autoriser les applications Windows à accéder..." → Forcer le refus
```

> 📌 **Note RGPD** : documentez les niveaux de télémétrie choisis dans votre registre des traitements. Le niveau Sécurité + documentation = posture défendable.

---

## 69. Microsoft 365 Apps : déploiement, canaux, licences

### 69.1 Canaux de mise à jour

| Canal | Fréquence | Cible |
|---|---|---|
| Current Channel | Mensuel | Utilisateurs standard (défaut) |
| Monthly Enterprise Channel | Mensuel (décalé) | **Recommandé entreprise** |
| Semi-Annual Enterprise Channel | 2/an | Postes critiques / validation longue |

### 69.2 Déploiement avec l'Outil de déploiement Office (ODT)

`configuration.xml` type :

```xml
<Configuration>
  <Add OfficeClientEdition="64" Channel="MonthlyEnterprise">
    <Product ID="O365ProPlusRetail">
      <Language ID="fr-fr" />
      <ExcludeApp ID="Groove" />
      <ExcludeApp ID="Lync" />
    </Product>
  </Add>
  <Updates Enabled="TRUE" Channel="MonthlyEnterprise" />
  <Display Level="None" AcceptEULA="TRUE" />
  <Property Name="SharedComputerLicensing" Value="0" />
  <Property Name="AUTOACTIVATE" Value="1" />
</Configuration>
```

```powershell
# Télécharger puis installer
.\setup.exe /download configuration.xml
.\setup.exe /configure configuration.xml
```

### 69.3 Licences et activation

| Mode | Usage |
|---|---|
| Activation basée sur l'utilisateur (défaut) | Poste nominatif, licence M365 |
| **Shared Computer Licensing** | RDS, postes partagés (`SharedComputerLicensing=1`) |
| Clé KMS/MAK (Office LTSC) | Postes sans M365, version perpétuelle |

```powershell
# Vérifier l'état d'activation Office
cscript "C:\Program Files\Microsoft Office\Office16\ospp.vbs" /dstatus
```

---

## 70. Navigateurs en entreprise : Edge géré, stratégies

### 70.1 Pourquoi Edge en entreprise (ou Chrome géré)

Edge = Chromium + intégration Entra/SSO + stratégies GPO/Intune natives + IE Mode (pour les vieilles applications intranet).

### 70.2 Stratégies essentielles (modèles ADMX Edge)

| Stratégie | Effet |
|---|---|
| Configure favorites / ManagedFavorites | Favoris d'entreprise imposés |
| HomepageLocation / RestoreOnStartup | Page d'accueil intranet |
| PasswordManagerEnabled | Désactiver le gestionnaire de mots de passe intégré (si coffre d'entreprise) |
| SmartScreenEnabled | Forcer SmartScreen |
| IE Mode (InternetExplorerIntegration) | Compatibilité applications legacy |

```powershell
# Exemple : forcer la page d'accueil intranet via registre
$edge = "HKLM:\SOFTWARE\Policies\Microsoft\Edge"
New-Item -Path $edge -Force | Out-Null
Set-ItemProperty -Path $edge -Name "HomepageLocation" -Value "https://intranet.entreprise.local" -Type String
Set-ItemProperty -Path $edge -Name "HomepageIsNewTabPage" -Value $false -Type DWord
```

### 70.3 Profils et synchronisation

- Synchronisation du profil Edge via le compte Entra (favoris, mots de passe) : pratique, à encadrer (données hors UE ? → vérifier la résidence des données du tenant).
- Pour les postes partagés : **désactiver la synchronisation** et effacer les données à la fermeture (stratégie `ClearBrowsingDataOnExit`).

---

## 71. Sauvegarde et PRA poste de travail

### 71.1 Ce qu'on sauvegarde (et ce qu'on ne sauvegarde pas)

| Données | Solution | RPO |
|---|---|---|
| Fichiers utilisateurs | OneDrive KFM (Bureau/Documents/Images) | Continu |
| Profils spécifiques (métier) | Dossier redirigé ou script de copie vers partage | Quotidien |
| Configuration poste | **Rien** → redéploiement MDT/Autopilot | N/A |
| Clés BitLocker | AD / Entra (automatique) | À l'activation |

### 71.2 PRA « poste de travail » (procédure type)

```
INCIDENT : poste volé / HS / ransomware localisé
1. Isoler : désactiver le compte AD/Entra, révoquer les sessions, effacer à distance (Intune → Effacer)
2. Clés BitLocker : vérifier qu'elles sont en AD/Entra (récupération si besoin forensique)
3. Remplacer : déployer un poste neuf (MDT/Autopilot) — objectif < 4 h
4. Restaurer : l'utilisateur se connecte → OneDrive resynchronise → apps via Intune/winget
5. Post-mortem : cause, ticket GLPI, ajustement des protections
```

### 71.3 Effacement à distance (Intune)

Intune → Appareils → sélectionner → **Effacer** (avec ou sans conservation de l'inscription) ou **Retirer** (retire la gestion, conserve les données — pour un départ propre).

```powershell
# Effacement local sécurisé d'un disque avant recyclage (hors BitLocker)
cipher /w:C:
# Ou, plus radical : le chiffrement BitLocker + destruction de la clé = données irrécupérables
# (méthode "crypto-erase" : chiffrer puis supprimer les protecteurs)
```

---

## 72. Gestion de l'énergie : stratégies, veille moderne, batteries

### 72.1 Plans d'alimentation en entreprise

```powershell
# Voir les plans disponibles / actif
powercfg /list
# Activer un plan (GUID du plan "Performances élevées" en exemple)
powercfg /setactive 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c

# Imposer par GPO : Configuration ordinateur\Modèles d'administration\Système\Gestion de l'alimentation
# → "Spécifier un plan d'alimentation actif personnalisé"

# Empêcher la mise en veille pendant une tâche critique
powercfg /requestsoverride PROCESS monprocessus.exe Display System
```

### 72.2 Veille moderne (Modern Standby / S0)

Windows 11 privilégie la **veille moderne** (S0 low-power idle) sur les machines récentes : réveil instantané, MàJ en arrière-plan — mais **consommation résiduelle** et parfois réveils intempestifs.

```powershell
# Quel mode de veille supporte la machine ?
powercfg /a
# Voir les réveils intempestifs :
powercfg /waketimers
powercfg /lastwake

# Désactiver un périphérique qui réveille :
powercfg /devicedisablewake "Intel(R) Ethernet Connection"
```

### 72.3 Batteries (parc de portables)

```powershell
# Rapport de batterie (autonomie, usure)
powercfg /batteryreport /output C:\Admin\battery-report.html
# Indicateur clé : "Design Capacity" vs "Full Charge Capacity"
# → en dessous de 70-80 % de la capacité d'origine : batterie à remplacer

# Rapport d'énergie (diagnostic 60 s)
powercfg /energy /output C:\Admin\energy-report.html
```

> 💡 **Lien avec le métier énergie** : un parc de 200 portables en veille moderne consomme en permanence quelques watts chacun. Une GPO « mise en veille prolongée après 2 h » + extinction nocturne via tâche planifiée = économies mesurables. Voir le guide onduleurs pour le dimensionnement des protections électriques associées.

---

(Bloc 5/6 — sections 61 à 72)

---

## 73. Erreurs classiques (20) : ce qu'il ne faut pas faire

### Erreur n°1 — Migrer sans inventaire de compatibilité
Déployer Windows 11 « à l'aveugle » puis découvrir 40 % de postes sans TPM 2.0. **Toujours** commencer par le script de la section 3.2.

### Erreur n°2 — Utiliser le contournement de registre TPM/CPU en production
`AllowUpgradesWithUnsupportedTPMOrCPU` = pas de support, pas de MàJ garanties. En entreprise : les postes non compatibles se **remplacent**, ils ne se « bricolent » pas.

### Erreur n°3 — Activer BitLocker sans sauvegarder les clés
Chiffrement sans GPO de récupération = le premier écran bleu de récupération devient une perte de données. **Règle** : pas de clé en AD/Entra = pas de chiffrement.

### Erreur n°4 — Mettre à jour le BIOS sans suspendre BitLocker
Le classique du lundi matin : 15 appels « écran bleu BitLocker » après une campagne de MàJ BIOS. Procédure écrite + `Suspend-BitLocker -RebootCount 1` systématique.

### Erreur n°5 — Donner les droits d'administrateur local aux utilisateurs
« Pour qu'il puisse installer ses logiciels » = la porte ouverte aux ransomwares et aux configurations exotiques. Solutions : winget/Intune pour les logiciels validés, LAPS pour le dépannage.

### Erreur n°6 — Une seule GPO « fourre-tout »
Une GPO de 150 paramètres impossible à déboguer. **Une GPO = un objectif**, nommée `GPO-[Cible]-[Fonction]`.

### Erreur n°7 — Oublier les droits « Lire + Appliquer » sur le filtrage de sécurité
Depuis 2016, filtrer une GPO sur un groupe sans donner « Lire » aux « Ordinateurs authentifiés » = GPO silencieusement ignorée (voir cas pratique 10).

### Erreur n°8 — Tester les MàJ directement en production
Pas d'anneaux WUfB = la KB fautive touche 100 % du parc le même jour. Anneau pilote obligatoire (section 25).

### Erreur n°9 — Laisser les pilotes Windows Update actifs en production
Un pilote GPU poussé par WU un vendredi soir = 30 postes en BSOD le lundi. Pilotes via le constructeur, validés (section 26.3).

### Erreur n°10 — Sysprepper une machine jointe au domaine ou déjà utilisée
Image avec SID dupliqués, relations d'approbation cassées en série. Sysprep = **toujours** depuis une machine de référence propre, jamais jointe (section 9).

### Erreur n°11 — Supprimer un profil en effaçant juste `C:\Users\...`
Le registre garde la trace → profil temporaire au prochain logon. Utiliser `Win32_UserProfile` (sections 19.3, 49).

### Erreur n°12 — Stocker des mots de passe en clair dans unattend.xml / scripts
Le XML traîne sur un partage, dans un dépôt Git… Utiliser LAPS + comptes de service dédiés (sections 8.2, 34).

### Erreur n°13 — Injecter « tous les pilotes » dans MDT
Conflits, BSOD, task sequences de 3 heures. **Un dossier par modèle + profil de sélection** (section 11.2).

### Erreur n°14 — Négliger la période de rollback de 10 jours
`/ResetBase` ou suppression de `Windows.old` le jour J = impossible de revenir en arrière si un problème métier apparaît à J+5 (section 55).

### Erreur n°15 — Ne pas documenter les exceptions (DRA, exclusions Defender, GPO)
« Pourquoi ce poste n'est pas chiffré ? » — sans registre des exceptions, impossible de répondre à un audit. Tout écart = ticket + date de révision.

### Erreur n°16 — Laisser le compte Administrator intégré activé avec un mot de passe faible
Cible n°1 des attaques par force brute (RID 500, impossible à renommer efficacement). Le **désactiver**, gérer l'admin local via LAPS.

### Erreur n°17 — Configurer l'accès conditionnel sans compte de secours
Une stratégie trop stricte vous enferme dehors vous aussi. **Compte break glass** exclu de toutes les stratégies, avec authentification forte, dont les accès sont alertés (section 64.2).

### Erreur n°18 — Croire que « réinitialiser ce PC » suffit pour un départ
Sans Autopilot Reset / effacement Intune, l'ancien profil, les certificats et parfois les données restent accessibles. Procédure de réaffectation écrite (sections 13, 71).

### Erreur n°19 — Ne pas surveiller l'espace disque des postes
C: plein = MàJ impossibles, profils corrompus, tickets en cascade. Storage Sense par GPO + alerte Zabbix/Intune sous 15 % libre.

### Erreur n°20 — Faire du poste un « animal de compagnie » au lieu du « bétail »
Passer 3 heures à « réparer » un poste quand le redéploiement prend 45 minutes. **Doctrine** : poste jetable, données dans OneDrive, redéploiement standardisé (sections 40.3, 71).

---

## 74. Checklist : déploiement d'un poste de A à Z

### Phase préparation
- [ ] Poste compatible vérifié (TPM 2.0, Secure Boot, CPU, 256 Go SSD min.)
- [ ] Méthode de déploiement choisie : MDT / Autopilot / manuelle
- [ ] Média à jour (ISO du mois, 24H2)
- [ ] Pilotes du modèle validés (dossier dédié MDT ou package constructeur)
- [ ] Compte de jointure / profil Autopilot prêt
- [ ] Nom de poste selon la convention (`SITE-FONCTION-###`)

### Phase installation
- [ ] Installation / task sequence / Autopilot exécutée sans erreur
- [ ] Mises à jour Windows complètes (plus de MàJ en attente)
- [ ] Pilotes tous installés (aucun point d'exclamation dans le Gestionnaire de périphériques)
- [ ] Fuseau horaire, langue, clavier : fr-FR

### Phase configuration
- [ ] Jointure domaine / Entra vérifiée (`dsregcmd /status`, `Get-CimInstance Win32_ComputerSystem`)
- [ ] GPO appliquées (`gpresult /r` : toutes les GPO attendues présentes)
- [ ] BitLocker actif + clé en AD/Entra vérifiée
- [ ] LAPS fonctionnel (mot de passe récupérable)
- [ ] Defender à jour, protection temps réel active
- [ ] Applications du socle installées (M365, navigateur géré, VPN, agent supervision, agent GLPI)
- [ ] Wi-Fi d'entreprise / VPN testé
- [ ] Imprimantes déployées et testées

### Phase remise
- [ ] Session utilisateur testée (ouverture < 1 min, profil propre)
- [ ] OneDrive KFM actif et synchronisé
- [ ] Fiche de remise signée (utilisateur informé du support)
- [ ] Ticket GLPI de déploiement clôturé avec n° de série et date

---

## 75. Checklist : audit de sécurité d'un poste Windows 11

- [ ] Build ≥ 26100 (24H2) et dernière cumulative installée
- [ ] TPM 2.0 prêt (`Get-Tpm`), Secure Boot activé (`Confirm-SecureBootUEFI`)
- [ ] BitLocker : `ProtectionStatus = On`, clé en AD/Entra
- [ ] HVCI / Intégrité de la mémoire : activé
- [ ] Credential Guard : actif (si Entreprise)
- [ ] LSA Protection : activée
- [ ] Defender : temps réel OK, signatures < 7 jours, Tamper Protection active
- [ ] Règles ASR : en mode Block (post-audit)
- [ ] Pare-feu : profils actifs, règles locales fusionnées ou non selon politique
- [ ] LAPS : mot de passe géré et récupérable
- [ ] Compte Administrator intégré : désactivé
- [ ] Utilisateur quotidien : **non administrateur**
- [ ] SMBv1 : désactivé ; LLMNR : désactivé
- [ ] PowerShell v2 : désactivé
- [ ] UAC : niveau maximum
- [ ] Mises à jour : dans l'anneau attendu, pas de pause anormale
- [ ] Télémétrie : niveau Sécurité (Entreprise)
- [ ] Copilot / Widgets : désactivés (si politique)
- [ ] Store : selon politique (bloqué ou catalogue approuvé)
- [ ] Exclusions Defender : documentées et justifiées
- [ ] Dernière analyse complète < 30 jours

```powershell
# Score rapide : compte les points validés (extrait — à compléter selon votre politique)
$score = 0; $total = 0
function Test-Point($nom, $condition) {
    $script:total++
    if (& $condition) { $script:score++; Write-Host "[OK] $nom" -ForegroundColor Green }
    else { Write-Host "[KO] $nom" -ForegroundColor Red }
}
Test-Point "TPM 2.0 prêt" { (Get-Tpm).TpmReady }
Test-Point "Secure Boot" { try { Confirm-SecureBootUEFI } catch { $false } }
Test-Point "BitLocker actif" { (Get-BitLockerVolume -MountPoint "C:").ProtectionStatus -eq 'On' }
Test-Point "Defender temps réel" { (Get-MpComputerStatus).RealTimeProtectionEnabled }
Test-Point "Admin intégré désactivé" { (Get-LocalUser -Name "Administrateur").Enabled -eq $false }
Write-Host "`nScore : $score / $total" -ForegroundColor Cyan
```

---

## 76. Pense-bête de poche : commandes et raccourcis essentiels

### PowerShell — diagnostic express

```powershell
Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion, OsBuildNumber
Get-Tpm | Select-Object TpmPresent, TpmReady, TpmEnabled
Confirm-SecureBootUEFI
Get-BitLockerVolume | Select-Object MountPoint, ProtectionStatus, EncryptionPercentage
Get-MpComputerStatus | Select-Object AntivirusEnabled, RealTimeProtectionEnabled, AntivirusSignatureLastUpdated
gpresult /r
dsregcmd /status
systeminfo | Select-String "Version du système","Type du système"
```

### Réseau

```powershell
Test-NetConnection srv.entreprise.local -Port 445
Resolve-DnsName intranet.entreprise.local
Get-NetIPAddress -AddressFamily IPv4
netsh wlan show interfaces
ipconfig /flushdns
netsh winsock reset        # + redémarrage
```

### Dépannage système

```powershell
sfc /scannow
dism /Online /Cleanup-Image /RestoreHealth
chkdsk C: /f
shutdown /r /o /f /t 0     # redémarrer vers WinRE
reagentc /info
wusa /uninstall /kb:XXXXXXX /quiet /norestart
```

### AD / GPO / postes distants

```powershell
gpupdate /force
gpresult /h C:\Admin\gpresult.html
Reset-ComputerMachinePassword -Server DC01 -Credential (Get-Credential)
Enter-PSSession -ComputerName PC-042
Invoke-Command -ComputerName PC-042 -ScriptBlock { Get-BitLockerVolume }
```

### Raccourcis clavier (support)

| Touches | Action |
|---|---|
| `Win + X` | Menu admin (Terminal, Gestion du disque…) |
| `Win + R` → `msconfig` | Configuration système |
| `Win + R` → `gpedit.msc` | Stratégie de groupe locale |
| `Win + R` → `eventvwr` | Observateur d'événements |
| `Win + R` → `tpm.msc` | Console TPM |
| `Win + R` → `secpol.msc` | Stratégie de sécurité locale |
| `Maj + Redémarrer` | Démarrage avancé (WinRE) |
| `Ctrl + Maj + F3` (OOBE) | Mode audit |
| `Win + Ctrl + Maj + B` | Redémarrer le pilote graphique (écran figé) |

---

## 77. Glossaire

| Terme | Définition |
|---|---|
| **ADBA** | Active Directory-Based Activation : activation en volume via AD, sans serveur KMS dédié |
| **ADK** | Assessment and Deployment Kit : outils Microsoft (WSIM, USMT, WinPE) pour le déploiement |
| **Anneau (ring)** | Groupe de postes recevant les MàJ avec un délai donné (WUfB) |
| **AppLocker** | Contrôle des applications par règles (éditeur, chemin, hash) — Entreprise |
| **ASR** | Attack Surface Reduction : règles Defender bloquant les comportements malveillants |
| **Autopilot** | Service cloud transformant un PC OEM en poste d'entreprise sans image |
| **BitLocker** | Chiffrement de volume natif Windows (XTS-AES) |
| **CSP** | Configuration Service Provider : équivalent MDM des GPO pour Intune |
| **DRA** | Data Recovery Agent : certificat permettant de déchiffrer tout volume (passe-partout) |
| **ESP** | Enrollment Status Page : écran bloquant l'OOBE tant que la config n'est pas prête |
| **ESU** | Extended Security Updates : correctifs payants après la fin de support |
| **FSLogix** | Conteneurs VHDX pour les profils (VDI/RDS) |
| **GVLK** | Generic Volume License Key : clé générique pour activation KMS/ADBA |
| **HVCI** | Hypervisor-protected Code Integrity (« Intégrité de la mémoire ») |
| **KFM** | Known Folder Move : redirection Bureau/Documents/Images vers OneDrive |
| **LAPS** | Local Administrator Password Solution : mots de passe admin locaux uniques et rotatifs |
| **LTSC** | Long-Term Servicing Channel : édition à support long, sans feature updates |
| **MDT** | Microsoft Deployment Toolkit : déploiement d'images « Lite Touch » |
| **OOBE** | Out-Of-Box Experience : écrans de premier démarrage |
| **PCR** | Platform Configuration Registers : registres du TPM stockant les mesures de démarrage |
| **PXE** | Preboot Execution Environment : démarrage réseau (WDS/MDT) |
| **Quality update** | Mise à jour mensuelle cumulative (sécurité + correctifs) |
| **Feature update** | Mise à jour annuelle de version (ex. 24H2) |
| **Sysprep** | Généralisation d'une installation avant capture d'image |
| **TPM** | Trusted Platform Module : puce/firmware de sécurité (clés, mesures de démarrage) |
| **UBR** | Update Build Revision : numéro de révision après le build (ex. 26100.XXXX) |
| **USMT** | User State Migration Tool : migration de profils (ScanState/LoadState) |
| **VBS** | Virtualization-Based Security : socle (Credential Guard, HVCI) |
| **WDAC** | Windows Defender Application Control : contrôle d'applications par stratégie signée |
| **WEF** | Windows Event Forwarding : collecte centralisée native des journaux |
| **WinRE** | Windows Recovery Environment : environnement de récupération |
| **WUfB** | Windows Update for Business : gestion cloud des MàJ (anneaux, délais) |
| **XTS-AES** | Mode de chiffrement par blocs utilisé par BitLocker |

---

## 78. Quiz : 10 questions + réponses commentées

### Q1 — Un poste affiche « TPM 2.0 » dans le BIOS mais `Get-Tpm` retourne `TpmPresent : False`. Quelle est la première chose à vérifier ?
- [ ] A. Réinstaller Windows
- [ ] B. Activer Intel PTT / AMD fTPM (ou le TPM discret) dans le firmware UEFI
- [ ] C. Acheter une puce TPM externe
- [ ] D. Désactiver Secure Boot

**Réponse : B.** Sur beaucoup de machines, le TPM firmware est simplement désactivé dans l'UEFI. `Get-Tpm` ne voit que ce qui est exposé par le firmware. La réinstallation (A) ne change rien, et Secure Boot (D) est un sujet distinct.

### Q2 — Quelle commande suspend BitLocker pour exactement un redémarrage (avant une MàJ du BIOS) ?
- [ ] A. `Disable-BitLocker -MountPoint "C:"`
- [ ] B. `Suspend-BitLocker -MountPoint "C:" -RebootCount 1`
- [ ] C. `Manage-bde -off C:`
- [ ] D. `Remove-BitLockerKeyProtector -MountPoint "C:"`

**Réponse : B.** A et C **déchiffrent** le volume (long et inutile ici), D supprime un protecteur. `Suspend-BitLocker -RebootCount 1` expose temporairement la clé en clair pour un seul boot — c'est la procédure standard avant MàJ BIOS.

### Q3 — Vous déployez Windows 11 via MDT. Où placez-vous les pilotes pour éviter les conflits entre modèles ?
- [ ] A. Tous dans un seul dossier « Drivers »
- [ ] B. Un dossier par modèle + profil de sélection dans la task sequence
- [ ] C. Dans l'image WIM directement
- [ ] D. Via Windows Update après déploiement

**Réponse : B.** Un dossier par modèle (`Out-of-Box Drivers\Win11-x64\<Modèle>`) avec un profil de sélection = la bonne pratique (section 11.2). A provoque des conflits, C alourdit l'image, D n'est pas maîtrisé.

### Q4 — Un utilisateur se connecte avec un « profil temporaire ». Dans le registre `ProfileList`, vous voyez sa clé SID et une clé `SID.bak`. Que faites-vous ?
- [ ] A. Vous renommez `.bak` en supprimant l'autre clé
- [ ] B. Vous supprimez les deux clés après sauvegarde des données, puis l'utilisateur se reconnecte sur un profil neuf
- [ ] C. Vous restaurez le système
- [ ] D. Vous copiez `C:\Users\TEMP` vers son dossier

**Réponse : B.** La méthode propre : sauvegarde des données, suppression des clés + du profil (via `Win32_UserProfile`), reconnexion = profil sain (cas pratique 8). A peut replanter si `NTUSER.DAT` est corrompu.

### Q5 — Quelle différence entre une mise à jour « qualité » et « fonctionnalité » dans WUfB ?
- [ ] A. Aucune, ce sont des synonymes
- [ ] B. Qualité = correctifs mensuels cumulatifs ; Fonctionnalité = nouvelle version annuelle (24H2…)
- [ ] C. Qualité = pilotes ; Fonctionnalité = sécurité
- [ ] D. Qualité = payante ; Fonctionnalité = gratuite

**Réponse : B.** Les délais WUfB se règlent séparément : court pour la qualité (7-14 j), long pour les fonctionnalités (60-120 j) — section 26.1.

### Q6 — `Confirm-SecureBootUEFI` retourne une erreur « Un privilège requis n'est pas détenu par le client ». Qu'est-ce que cela signifie ?
- [ ] A. Il faut lancer PowerShell en administrateur
- [ ] B. La machine est en BIOS Legacy ou Secure Boot n'est pas supporté/activé
- [ ] C. Le TPM est verrouillé
- [ ] D. BitLocker est désactivé

**Réponse : B.** Cette erreur (même en admin) indique que l'API UEFI Secure Boot n'est pas disponible : firmware Legacy ou Secure Boot non supporté. C'est un poste non compatible Windows 11 en l'état.

### Q7 — En GPO, vous filtrez sur un groupe de sécurité mais la GPO ne s'applique pas. Cause la plus probable depuis 2016 ?
- [ ] A. Le contrôleur de domaine est en panne
- [ ] B. Il manque les droits « Lire » (+ « Appliquer ») pour « Ordinateurs authentifiés » sur la GPO
- [ ] C. Il faut redémarrer le poste 3 fois
- [ ] D. Le filtre WMI est obligatoire

**Réponse : B.** Depuis la KB3163622, le filtrage de sécurité exige que « Ordinateurs authentifiés » (ou « Utilisateurs authentifiés ») ait au minimum « Lire » sur la GPO — l'oubli classique (cas pratique 10, erreur n°7).

### Q8 — Quelle méthode de déploiement ne nécessite AUCUNE image maître à maintenir ?
- [ ] A. MDT
- [ ] B. Windows Autopilot
- [ ] C. Clonezilla
- [ ] D. DISM /Capture-Image

**Réponse : B.** Autopilot transforme l'installation OEM via Intune : pas de WIM à maintenir, pas de PXE (section 12). Les autres reposent sur une image.

### Q9 — Un poste BitLocker redemande la clé à chaque démarrage après un changement de carte mère. Quelle est la bonne séquence ?
- [ ] A. Saisir la clé, `Suspend-BitLocker -RebootCount 1`, redémarrer, recréer le protecteur TPM si besoin, `Resume-BitLocker`, vérifier la sauvegarde AD
- [ ] B. Désactiver BitLocker définitivement
- [ ] C. Effacer le TPM sans la clé
- [ ] D. Réinstaller Windows

**Réponse : A.** C'est la procédure complète (sections 18.1-18.3), avec l'étape critique souvent oubliée : **vérifier que la clé est sauvegardée en AD après recréation du protecteur**.

### Q10 — Windows 10 est hors support depuis le 14/10/2025. Un poste incompatible avec Windows 11 doit rester en service 6 mois pour une application métier. Que proposez-vous ?
- [ ] A. Le laisser tel quel, « ça a toujours marché »
- [ ] B. ESU (Extended Security Updates) avec une date de sortie écrite à 6 mois + isolement réseau renforcé
- [ ] C. Le contournement de registre TPM
- [ ] D. Désactiver Windows Update pour « ne pas casser l'application »

**Réponse : B.** L'ESU est le seul cadre supporté pour gagner du temps, **avec** une date de sortie écrite et des mesures compensatoires (VLAN isolé, pas d'Internet direct, surveillance renforcée). A/C/D augmentent le risque sans le traiter.

---

## 79. Pour aller plus loin : documentation, outils, communautés

### Documentation officielle
- **Microsoft Learn — Windows 11** : documentation de référence (déploiement, sécurité, MDM)
- **Windows release health** : problèmes connus par build, contournements officiels
- **Security Compliance Toolkit** : baselines + Policy Analyzer
- **Cycle de vie Microsoft** : dates de fin de support par version (à vérifier avant chaque projet)

### Outils
- **Sysinternals Suite** (Process Explorer, Autoruns, TCPView, PsTools) — diagnostic
- **SetupDiag** — échecs de mise à niveau
- **MDT + ADK + WinPE** — déploiement Lite Touch
- **Intune** / **MECM** — gestion moderne / co-management
- **Microsoft Graph PowerShell SDK** — automatisation Intune/Entra
- **Wireshark** — analyse réseau
- **CrystalDiskInfo** — santé SMART
- **Agent GLPI** — inventaire de parc

### Communautés (francophones et internationales)
- **Microsoft Q&A** (forums officiels, étiquette `windows-11`)
- **Reddit r/sysadmin** — retours d'expérience terrain
- **Serveurs Discord / forums** d'admins systèmes francophones
- **Blogs MVP** : déploiement, Autopilot, sécurité Windows

### Certifications utiles
- **MD-102** (Endpoint Administrator) : Intune, Autopilot, conformité — la certification « poste de travail moderne »
- **AZ-800/AZ-801** (Windows Server Hybrid Administrator) : AD, GPO, hybride
- **SC-300** (Identity Administrator) : Entra ID, accès conditionnel

### Prochaines étapes pour votre parc (suggestions)
1. Finaliser l'inventaire de compatibilité (section 3.2) couplé à GLPI
2. Choisir la cible : MDT vs Autopilot (ou les deux : MDT pour le fixe, Autopilot pour les nomades)
3. Déployer BitLocker + LAPS à 100 % avec clés centralisées
4. Mettre en place les anneaux WUfB et la baseline de sécurité
5. Planifier la sortie des derniers Windows 10 / ESU avec dates écrites

---

## 80. Annexe A : tableau des builds et versions

| Version | Build | UBR (exemple) | Canal | Fin de support Pro | Fin de support Entreprise |
|---|---|---|---|---|---|
| 21H2 | 22000 | .2538 | GA | 10/10/2023 | 08/10/2024 |
| 22H2 | 22621 | .5258 | GA | 08/10/2024 | 14/10/2025 |
| 23H2 | 22631 | .5624 | GA | 11/11/2025 | 10/11/2026 |
| 24H2 | 26100 | .7171 | GA | 13/10/2026 | 12/10/2027 |

> 📌 Vérifiez les dates et UBR sur le site officiel du cycle de vie Microsoft : elles évoluent à chaque cumulative. Au 27/09/2026, la cible de déploiement est **24H2**.

```powershell
# Identifier précisément un poste
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion" |
    Select-Object ProductName, DisplayVersion, CurrentBuild, UBR
# DisplayVersion = 24H2, CurrentBuild = 26100, UBR = révision cumulative
```

---

## 81. Annexe B : chemins de clés de registre les plus utiles

| Usage | Chemin |
|---|---|
| Version Windows | `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion` |
| Stratégies Windows Update | `HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` (+ `\AU`) |
| Delivery Optimization | `HKLM\SOFTWARE\Policies\Microsoft\Windows\DeliveryOptimization` |
| BitLocker (stratégies) | `HKLM\SOFTWARE\Policies\Microsoft\FVE` |
| LAPS Windows | `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\LAPS` (état) |
| Désactiver Copilot | `HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsCopilot` |
| Widgets / News | `HKLM\SOFTWARE\Policies\Microsoft\Dsh` |
| Store | `HKLM\SOFTWARE\Policies\Microsoft\WindowsStore` |
| Télémétrie | `HKLM\SOFTWARE\Policies\Microsoft\Windows\DataCollection` |
| Profils utilisateurs | `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList` |
| Exécution au démarrage (machine) | `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run` |
| Pare-feu (profils) | Configuré via `netsh` / `Set-NetFirewallProfile` (pas de clé unique) |
| WinRE | `reagentc /info` (pas de registre utile direct) |
| Certificats machine | `Cert:\LocalMachine\My` (fournisseur de certificats) |

---

## 82. Annexe C : modèle de plan de migration Windows 10 → 11

```text
PLAN DE MIGRATION WINDOWS 10 → WINDOWS 11
Entreprise : ______________________    Responsable : ______________________
Période : du ____/____/________ au ____/____/________

1. PÉRIMÈTRE
   - Postes totaux : ____    - Compatibles : ____    - À remplacer : ____
   - Postes sous ESU (avec date de sortie) : ____

2. MÉTHODE DE DÉPLOIEMENT
   [ ] MDT (Lite Touch)   [ ] Autopilot + Intune   [ ] Mixte   [ ] Manuelle
   Image / profil de référence : ________________________________________

3. VAGUES
   | Vague | Périmètre (service/site) | Nb postes | Date début | Date fin | Responsable |
   |-------|----------------------------|-----------|------------|----------|-------------|
   | Pilote| IT + volontaires          |           |            |          |             |
   | 1     |                            |           |            |          |             |
   | 2     |                            |           |            |          |             |
   | 3     |                            |           |            |          |             |

4. PRÉREQUIS PAR POSTE (checklist section 74)
   [ ] Compatibilité vérifiée   [ ] Sauvegarde OneDrive OK   [ ] Applications métier validées

5. COMMUNICATION
   - Annonce J-14 : __________   - Rappel J-3 : __________   - Support renforcé J+3 : __________

6. GESTION DES RISQUES
   | Risque | Probabilité | Impact | Mitigation |
   |--------|-------------|--------|------------|
   | KB fautive | Moyenne | Élevé | Anneaux WUfB + pause |
   | Application incompatible | Moyenne | Élevé | Validation vague pilote |
   | Clé BitLocker perdue | Faible | Critique | GPO sauvegarde AD obligatoire |

7. CRITÈRES DE SORTIE
   [ ] 100 % des postes compatibles migrés
   [ ] Chaque poste non migré a une fiche d'exception datée (remplacement / ESU)
   [ ] Clés BitLocker centralisées vérifiées à 100 %
   [ ] Documentation d'exploitation mise à jour

8. CLÔTURE
   Date : __________    Validation RSSI / Direction : __________
```

---

*Fin du guide — Windows 11 en entreprise (23H2/24H2). Bon déploiement !*
