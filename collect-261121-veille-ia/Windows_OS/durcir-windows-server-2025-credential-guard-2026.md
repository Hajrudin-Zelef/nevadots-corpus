---
id: collect-261121-veille-ia/windows-os/durcir-windows-server-2025-credential-guard-2026
title: "Durcir Windows Server 2025 : Credential Guard [2026]"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["windows", "server", "credential", "guard", "microsoft", "depuis", "durcir", "defaut"]
source: https://tech-insider.org/fr/durcir-windows-server-2025-credential-guard-2026/
source_anchor: ""
source_lines: [1, 334]
section: "Windows_OS"
---
Depuis le Patch Tuesday d’août 2026, Microsoft a corrigé 751 failles, dont la CVE-2026-62784, une vulnérabilité d’exécution de code à distance touchant directement le processus LSASS sur lequel repose toute la protection des identifiants Windows. Le même correctif KB5120233 a aussi mis fin, selon des confirmations d’utilisateurs remontées sur Microsoft Q&A le 14 août 2026, à des dysfonctionnements persistants de Remote Credential Guard qui cassaient l’authentification Kerberos en double saut (double-hop) sur les serveurs membres. Sur les parcs Windows Server encore configurés avec les réglages par défaut, ce type de faille ouvre une porte royale au vol d’identifiants et aux mouvements latéraux. Windows Server 2025 change pourtant la donne : depuis novembre 2025, selon la documentation Microsoft Learn, Credential Guard est activé par défaut sur les serveurs membres éligibles, et un nouveau module PowerShell baptisé OSConfig permet d’appliquer et de surveiller des baselines de sécurité sans dépendre uniquement des stratégies de groupe. Ce tutoriel détaille, étape par étape, comment durcir Windows Server 2025 en combinant Credential Guard, la sécurité basée sur la virtualisation (VBS), les recommandations ANSSI pour les serveurs membres Active Directory et les nouveaux outils d’automatisation de Microsoft.

L’objectif n’est pas de cocher une case de conformité, mais de fermer la voie d’attaque la plus rentable pour un opérateur de ransomware en 2026 : le vol d’identifiants depuis la mémoire LSASS, suivi d’un pass-the-hash ou d’un pass-the-ticket vers le contrôleur de domaine. Comptez environ 120 minutes pour dérouler l’intégralité des 13 étapes sur un serveur de test, plus une phase de validation en environnement de préproduction avant tout déploiement en production.

## Pourquoi durcir Windows Server 2025 est devenu prioritaire en 2026

Windows Server 2025 est la version LTSC qui succède à Windows Server 2022, avec une intégration beaucoup plus profonde de la sécurité basée sur la virtualisation. Credential Guard lui-même n’est pas propre à cette édition : la documentation Microsoft Learn confirme, depuis juin 2025, sa compatibilité avec Windows Server 2016, 2019, 2022 et 2025. Ce qui change avec la version 2025, c’est le comportement par défaut : sur les machines qui remplissent les prérequis matériels et de licence (UEFI, Secure Boot, TPM, jointes à un domaine, hors contrôleur de domaine), Credential Guard démarre automatiquement, sans intervention de l’administrateur, un comportement que Microsoft Learn documentait encore le 27 avril 2026 et que l’équipe iPurple Team avait déjà signalé le 17 mars 2026 comme hérité de Windows 11 22H2. Un tour d’horizon myITforum de mars 2026 confirmait d’ailleurs que les nouveaux déploiements de Windows Server 2025 activent Credential Guard par défaut via VBS pour isoler les identifiants, et depuis le 23 avril 2026, les montées de version vers Windows Server 2025 héritent elles aussi de cette activation par défaut, sauf désactivation explicite. C’est une rupture avec Windows Server 2022, où l’activation restait manuelle et souvent oubliée lors des déploiements pressés.

Le contexte de menace rend ce changement urgent. Le rapport de mise à jour du 11 août 2026 (KB5120233) liste plusieurs failles d’élévation de privilèges qui servent typiquement de tremplin vers une attaque sur LSASS, en plus de la CVE-2026-62784 mentionnée plus haut. Les équipes de réponse à incident continuent d’observer des outils de type Mimikatz utilisés pour extraire des tickets Kerberos et des hashs NTLM directement depuis la mémoire d’un serveur membre mal protégé. Ce risque autour de NTLM n’est pas propre à Credential Guard : Microsoft avait planifié dès août 2025 un durcissement de NTLMv1, dont le déploiement sur Windows Server 2025 a débuté en novembre 2025 et qui affecte directement certains parcours d’authentification liés à Credential Guard. Durcir Windows Server 2025 avec Credential Guard, VBS et une politique LSA Protection cohérente réduit drastiquement la surface d’attaque exploitable une fois qu’un attaquant a obtenu un accès local.

Le guide ANSSI de sécurisation des serveurs membres Active Directory reste la référence en France pour ce type de durcissement. Il insiste sur des contrôles simples à fort impact : chiffrement BitLocker, isolation des services, synchronisation NTP maîtrisée, et pare-feu restrictif. Ce tutoriel combine ces recommandations avec les nouveaux mécanismes natifs de Windows Server 2025 pour produire une configuration robuste, applicable aussi bien sur un serveur physique que sur une machine virtuelle Hyper-V.

Ce guide s’adresse aux administrateurs systèmes, ingénieurs infrastructure et responsables sécurité qui gèrent un parc Windows Server en environnement d’entreprise, qu’il s’agisse de trois serveurs sur site ou de plusieurs centaines répartis entre datacenter et cloud hybride. Chaque étape est reproductible en environnement de test avant tout déploiement, avec des commandes PowerShell prêtes à l’emploi et les chemins exacts de stratégie de groupe. Aucune connaissance préalable de VBS ou de Credential Guard n’est nécessaire : nous expliquons chaque mécanisme avant de montrer comment l’activer.

## Prérequis : matériel, firmware et versions logicielles

Avant de vous lancer, vérifiez que votre infrastructure remplit les conditions suivantes. Credential Guard et VBS reposent sur des extensions matérielles de virtualisation : sans elles, aucune stratégie de groupe ne les activera.

| Composant | Exigence minimale | Remarque | 
|---|---|---|
| Système d’exploitation | Windows Server 2025 (Standard ou Datacenter, 64 bits) | Édition Core prise en charge | 
| Firmware | UEFI natif | Le mode Legacy/CSM empêche VBS de démarrer | 
| Secure Boot | Activé dans le firmware | Vérifiable via `Confirm-SecureBootUEFI` | 
| Virtualisation matérielle | Intel VT-x + EPT ou AMD-V + NPT/RVI | SLAT obligatoire | 
| TPM | TPM 2.0 recommandé | TPM 1.2 techniquement supporté mais déconseillé pour les baselines ANSSI | 
| Mémoire vive | 8 Go minimum | Marge pour l’overhead VBS/HVCI | 
| Rôle Hyper-V | Activé, même sans VM hébergée | VBS s’appuie sur l’hyperviseur Windows | 
| Statut du serveur | Joint à un domaine, non contrôleur de domaine | Condition de l’activation par défaut de Credential Guard | 

Si votre serveur est virtualisé sous Hyper-V, VMware ou un autre hyperviseur, la virtualisation imbriquée doit être explicitement exposée à la machine virtuelle invitée. Sans cela, VBS échouera silencieusement au démarrage, même si tous les autres prérequis sont réunis. C’est l’un des pièges les plus fréquents que nous détaillons plus loin.

## Étape 1 à 3 : vérifier la compatibilité et activer UEFI, Secure Boot, TPM 2.0

**Étape 1 : auditer le matériel avant toute modification.** Ne touchez à aucun réglage avant d’avoir confirmé que le serveur remplit les prérequis. Exécutez le script suivant en PowerShell administrateur :

```
# Vérifier la virtualisation matérielle activée dans le firmware
systeminfo | Select-String "Virtualization Enabled In Firmware"
# Vérifier l'état de Secure Boot
Confirm-SecureBootUEFI
# Vérifier la présence et la version du TPM
Get-Tpm
# Vérifier l'état d'installation du rôle Hyper-V
Get-WindowsFeature Hyper-V | Format-Table DisplayName, InstallState
```
**Étape 2 : activer Secure Boot et passer en mode UEFI.** Sur un serveur physique, entrez dans le firmware au démarrage (touche variable selon le constructeur : F2, F10, Del) et vérifiez que le mode de démarrage est réglé sur UEFI natif, avec Secure Boot activé. Sur une VM Hyper-V, utilisez plutôt Generation 2 lors de la création de la machine, qui active nativement UEFI et Secure Boot.

**Étape 3 : confirmer que le TPM 2.0 est actif.** Si `Get-Tpm` renvoie `TpmPresent : False` alors que le composant physique existe, le TPM est probablement désactivé dans le firmware ou en mode compatibilité 1.2. Sur les VM Hyper-V Generation 2, activez le module TPM virtuel via `Set-VMKeyProtector` et `Enable-VMTPM` avant de démarrer la machine.

## Étape 4 : installer Hyper-V, fondation de la sécurité basée sur la virtualisation

Même sur un serveur qui n’hébergera jamais de machine virtuelle, le rôle Hyper-V doit être présent, car VBS s’appuie sur l’hyperviseur Windows pour créer un conteneur mémoire isolé du système d’exploitation principal. Cette isolation est ce qui empêche un attaquant disposant de privilèges administrateur locaux de lire les secrets protégés par Credential Guard.

```
# Installer le rôle Hyper-V (sans l'interface de gestion si le serveur est en Core)
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart
```
C’est précisément à cette étape que surgissent les conflits avec des hyperviseurs tiers. Si le serveur physique héberge déjà VMware ESXi ou un autre hyperviseur en cohabitation, l’installation de Hyper-V échouera ou provoquera une instabilité au redémarrage. Sur un serveur dédié à la charge applicative (pas d’hyperviseur tiers), cette étape se déroule sans friction.

## Étape 5 et 6 : activer VBS et Credential Guard via stratégie de groupe

VBS isole les identifiants dérivés (hashs NTLM, tickets Kerberos TGT/TGS) et d’autres secrets gérés par LSASS dans un environnement protégé par l’hyperviseur. Credential Guard s’appuie directement sur cette isolation : LSASS ne stocke plus les secrets en clair ou sous une forme réversible, il communique avec un « LSASS isolé » via un canal RPC restreint aux composants système privilégiés.

**Étape 5 : activer VBS.** Dans la console de gestion des stratégies de groupe, le chemin unifié pour Windows Server 2025 est :

`Configuration ordinateur → Modèles d'administration → Système → Device Guard → Activer la sécurité basée sur la virtualisation`

Réglez cette stratégie sur **Activé**, puis, sous « Configuration Credential Guard », sélectionnez **Activé avec verrouillage UEFI** pour les serveurs membres. Ce verrouillage empêche un attaquant, ou même un administrateur mal intentionné, de désactiver Credential Guard à distance : la désactivation exige un accès physique et une confirmation au niveau du firmware.

**Étape 6 : configurer Credential Guard.** Pour un déploiement en laboratoire ou une vérification rapide, la configuration peut aussi passer par le registre :

```
# Activer VBS
New-Item -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard' -Force | Out-Null
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard' `
    -Name 'EnableVirtualizationBasedSecurity' -Type DWord -Value 1
# Activer Credential Guard avec verrouillage UEFI (valeur 1)
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Lsa' `
    -Name 'LsaCfgFlags' -Type DWord -Value 1
# Vérifier l'état après redémarrage
Get-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Lsa' | Select-Object LsaCfgFlags
```
En environnement de production, préférez toujours la stratégie de groupe ou le module OSConfig décrit à l’étape suivante : la modification directe du registre ne survit pas toujours aux réévaluations de baseline et peut créer des divergences de configuration difficiles à auditer.

## Étape 7 : déployer les baselines avec le module OSConfig

OSConfig est la nouveauté la plus utile de Windows Server 2025 pour les équipes qui gèrent des dizaines de serveurs membres. Ce module PowerShell applique des baselines prédéfinies (contrôleur de domaine, serveur membre, groupe de travail), surveille la conformité en continu et signale toute dérive de configuration, y compris sur des serveurs Core ou déconnectés du réseau d’entreprise.

```
# Installer OSConfig depuis PowerShell Gallery
Install-Module -Name OSConfig -Scope AllUsers
Import-Module OSConfig
# Lister les baselines disponibles
Get-OSConfigBaseline
# Simuler l'application de la baseline serveur membre (sans rien modifier)
Invoke-OSConfigBaseline -Name 'WindowsServer2025-MemberServer' -WhatIf
# Appliquer la baseline après validation
Invoke-OSConfigBaseline -Name 'WindowsServer2025-MemberServer' `
    -ComplianceLogPath 'C:\OSConfig\Logs\MemberServer.json' -Confirm:$false
# Vérifier la conformité après application
Get-OSConfigCompliance -Name 'WindowsServer2025-MemberServer'
```
La baseline serveur membre couvre plus de 300 réglages : Credential Guard, protection LSASS/PPL, imposition de TLS 1.2 minimum, SMB 3.0 obligatoire, Kerberos en AES uniquement, et suppression de WDigest des méthodes d’authentification actives. Lancez toujours l’option `-WhatIf` en premier sur un serveur de test : certaines applications legacy s’appuient encore sur des protocoles que la baseline désactive par défaut.

## Étape 8 : importer les baselines via le Security Compliance Toolkit

Le Security Compliance Toolkit (SCT) reste pertinent pour les organisations qui pilotent leur durcissement à travers des GPO centralisées plutôt que par un agent local. Microsoft publie des paquets de baseline pour Windows Server 2025 sous forme de sauvegardes de GPO, accompagnées d’une documentation Excel détaillant chaque réglage.

Le flux de travail classique :

- Téléchargez et extrayez le paquet SCT correspondant à Windows Server 2025.
- Dans la Console de gestion des stratégies de groupe, faites un clic droit sur « Objets de stratégie de groupe » puis « Importer les paramètres ».
- Sélectionnez la sauvegarde de GPO correspondant à votre rôle (serveur membre, contrôleur de domaine).
- Liez la GPO importée à l’unité d’organisation appropriée, en respectant la logique de tiering ANSSI (Tier 0, 1, 2).

SCT et OSConfig ne s’excluent pas : dans une architecture mature, SCT définit la politique centrale via GPO, tandis qu’OSConfig assure l’application locale et la détection de dérive sur les serveurs qui reçoivent difficilement les mises à jour de GPO (serveurs cloud isolés, environnements déconnectés).

## Étape 9 : activer LSA Protection (RunAsPPL) en complément de Credential Guard

LSA Protection, aussi appelée RunAsPPL, exécute le processus LSASS en tant que Protected Process Light : seul du code signé et fiable peut s’y injecter ou en lire la mémoire. C’est un mécanisme complémentaire, pas redondant, avec Credential Guard.

Credential Guard déplace les secrets eux-mêmes dans un conteneur isolé par l’hyperviseur : même en cas de compromission de LSASS, l’attaquant ne trouve rien à extraire. RunAsPPL protège LSASS en tant que processus : il empêche l’injection de code malveillant qui tenterait de manipuler ou de contourner ce mécanisme. Combinés, les deux rendent une attaque de type Mimikatz pratiquement inopérante sur un serveur correctement configuré.

```
# Activer LSA Protection (RunAsPPL)
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Lsa' `
    -Name 'RunAsPPL' -Type DWord -Value 1
# Forcer l'application dès le démarrage
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Lsa' `
    -Name 'RunAsPPLBoot' -Type DWord -Value 1
```
Sur Windows Server 2025, ces deux clés sont généralement déjà positionnées par la baseline de sécurité ou par OSConfig ; la configuration manuelle sert surtout à la vérification ou à des environnements qui n’utilisent pas encore ces outils d’automatisation.

## Étape 10 et 11 : appliquer les recommandations ANSSI pour serveurs membres AD DS

Le guide ANSSI « Back to basics » pour les serveurs membres insiste sur des contrôles simples mais à fort impact, qui complètent Credential Guard sans le remplacer. Quatre priorités ressortent : le chiffrement des disques, la réduction des services actifs, la maîtrise du temps réseau et le filtrage réseau strict.

**Étape 10 : chiffrer les volumes avec BitLocker.** ANSSI recommande de chiffrer les disques système et données pour prévenir le vol physique, en particulier sur les serveurs situés dans des salles techniques peu surveillées ou chez un hébergeur tiers.

```
# Chiffrer le volume système avec protecteur TPM
Enable-BitLocker -MountPoint "C:" -TpmProtector
# Chiffrer un volume de données
Enable-BitLocker -MountPoint "D:" -TpmProtector
# Configurer la synchronisation NTP sur la hiérarchie du domaine
w32tm /config /syncfromflags:domhier /update
w32tm /resync
```
**Étape 11 : isoler les services et restreindre le pare-feu.** N’activez que les rôles et fonctionnalités strictement nécessaires au service rendu par le serveur. Un serveur de fichiers n’a aucune raison d’exposer IIS ou un rôle SQL Server actif. Restreignez l’accès en gestion à distance (RDP, WinRM) aux seuls sous-réseaux d’administration et bloquez SMB et LDAP sortants vers l’extérieur du périmètre autorisé. Cette isolation, associée à la logique de tiering (comptes Tier 0 réservés aux contrôleurs de domaine, jamais utilisés pour se connecter à un serveur membre Tier 2), limite le rayon d’action d’un attaquant même s’il parvient à compromettre un premier serveur.

## Étape 12 et 13 : Attack Surface Reduction, WDAC et audit de conformité final

**Étape 12 : activer les règles de réduction de la surface d’attaque (ASR) et déployer WDAC.** Credential Guard et RunAsPPL protègent les secrets une fois qu’un attaquant est déjà sur la machine. ASR et Windows Defender Application Control (WDAC) réduisent la probabilité qu’un tel accès soit obtenu, en bloquant l’exécution de code non fiable.

```
# Activer une règle ASR de blocage du vol d'identifiants depuis les composants système
Add-MpPreference -AttackSurfaceReductionRules_Ids `
    '9e6c4e1f-7d60-472f-ba1a-a39ef669e4b2' `
    -AttackSurfaceReductionRules_Actions Enabled
# Générer une politique WDAC en mode audit sur un serveur de référence
New-CIPolicy -Level SignedVersion -FilePath C:\WDAC\BasePolicy.xml -UserPEs 3
# Convertir la politique en binaire déployable
ConvertFrom-CIPolicy C:\WDAC\BasePolicy.xml C:\WDAC\BasePolicy.bin
# Déployer la politique
Copy-Item C:\WDAC\BasePolicy.bin `
    C:\Windows\System32\CodeIntegrity\CiPolicies\Active\BasePolicy.bin
```
**Étape 13 : vérifier l’application et patcher.** Terminez toujours par une vérification que les correctifs de sécurité du mois sont installés, en particulier ceux liés à LSASS. Credential Guard n’est pas un substitut au patch management, et son historique de correctifs le rappelle : la mise à jour KB5055523 du 8 avril 2025 avait désactivé par erreur les comptes de machine protégés par Credential Guard à cause d’un problème de rotation des mots de passe Kerberos, un défaut que Microsoft n’a corrigé côté catalogue x64 (chaîne .msu erronée) que le 1er juin 2026. La CVE-2026-62784 illustre par ailleurs qu’une faille non corrigée dans lsasrv.dll peut compromettre la logique même que Credential Guard protège.

```
# Vérifier la présence du correctif d'août 2026
Get-HotFix | Where-Object {$_.HotFixID -eq 'KB5120233'}
# Contrôle final de conformité de la baseline
Get-OSConfigCompliance -Name 'WindowsServer2025-MemberServer' | Format-List
```
## Comment Credential Guard bloque une attaque de vol d’identifiants : scénario concret

Pour comprendre l’intérêt réel de ce durcissement, il est utile de comparer le déroulement d’une attaque type sur un serveur non protégé et sur un serveur qui applique les 13 étapes de ce tutoriel. Le scénario suivant reprend une chaîne d’attaque observée régulièrement lors d’incidents de type ransomware : un attaquant obtient un accès initial via un poste utilisateur compromis, puis se déplace latéralement jusqu’à un serveur membre du domaine.

### Avant durcissement : la mémoire LSASS en libre accès

Sur un serveur Windows Server 2025 encore en configuration par défaut désactivée ou sur un serveur legacy migré sans durcissement, un attaquant disposant de privilèges administrateur locaux peut dumper la mémoire du processus LSASS avec un outil comme Mimikatz ou un simple export via l’outil de diagnostic Task Manager (« Créer un fichier de vidage »). Ce fichier contient les hashs NTLM et parfois les tickets Kerberos en cache des comptes récemment authentifiés sur la machine, y compris des comptes à privilèges élevés si un administrateur du domaine s’est connecté récemment pour une opération de maintenance. L’attaquant rejoue ensuite ces identifiants via pass-the-hash ou pass-the-ticket pour progresser vers le contrôleur de domaine, souvent en quelques minutes seulement après l’accès initial.

### Après durcissement : un dump inexploitable

Sur un serveur où Credential Guard, VBS et LSA Protection (RunAsPPL) sont actifs, ce même dump mémoire ne contient plus les secrets exploitables. Credential Guard a déplacé les identifiants dérivés dans le conteneur isolé par l’hyperviseur, hors de portée du processus LSASS classique. RunAsPPL empêche par ailleurs l’injection du code de dump lui-même dans le processus, puisque seul du code signé et approuvé peut s’y attacher. L’attaquant obtient un fichier de vidage vide de tout secret utilisable, ce qui casse la chaîne d’attaque à cette étape précise et l’oblige à chercher un autre vecteur, généralement plus lent et plus détectable par les outils de surveillance (EDR, SIEM). C’est cette rupture de chaîne, bien plus qu’une case cochée dans un audit, qui justifie le temps investi dans ce tutoriel.

## Superviser le durcissement : journaux d’événements et intégration SIEM

Activer Credential Guard et VBS ne suffit pas si personne ne surveille leur état dans la durée. Un pilote mis à jour, une réinstallation de firmware ou une modification manuelle malencontreuse peuvent désactiver silencieusement ces protections. Windows Server 2025 journalise l’état de VBS et de Credential Guard dans le journal d’événements système, sous le fournisseur `Microsoft-Windows-DeviceGuard`. Les identifiants d’événements 7000 à 7003 signalent respectivement le démarrage, l’échec de démarrage, l’arrêt et les changements de configuration de VBS.

```
# Extraire les événements liés à VBS et Credential Guard des dernières 24 heures
Get-WinEvent -FilterHashtable @{
    LogName = 'System'
    ProviderName = 'Microsoft-Windows-DeviceGuard'
    StartTime = (Get-Date).AddHours(-24)
} | Select-Object TimeCreated, Id, Message | Format-Table -AutoSize
# Exporter le résultat pour ingestion par un SIEM (Sentinel, Splunk, Elastic)
Get-WinEvent -FilterHashtable @{LogName='System'; ProviderName='Microsoft-Windows-DeviceGuard'} |
    Export-Csv -Path 'C:\OSConfig\Logs\deviceguard-events.csv' -NoTypeInformation
```
Pour les environnements qui utilisent Microsoft Sentinel, connectez la source Windows Security Events et créez une règle d’alerte sur l’identifiant d’événement 7001 (échec de démarrage de VBS), qui signale généralement un problème matériel, un pilote incompatible ou une modification de firmware non planifiée. Combinez cette surveillance avec le rapport de conformité OSConfig planifié en tâche récurrente, par exemple toutes les six heures via le Planificateur de tâches Windows, pour détecter toute dérive de baseline avant qu’elle ne soit exploitée.

## Pièges courants à éviter lors du durcissement

Plusieurs erreurs reviennent systématiquement dans les retours d’expérience des équipes qui déploient Credential Guard et VBS sur un parc Windows Server 2025.

- **Pilotes non signés ou obsolètes.** Les pilotes en mode noyau anciens, en particulier certains agents antivirus, clients VPN legacy ou pilotes de stockage tiers, échouent au chargement une fois HVCI activé, provoquant un écran bleu au démarrage.
- **Virtualisation imbriquée mal configurée.** Sur une VM hébergée par un hyperviseur tiers, VBS ne démarre pas si l’hôte n’expose pas correctement les extensions de virtualisation à l’invité, sans message d’erreur explicite.
- **Rupture de l’authentification legacy.** Les baselines désactivent progressivement WDigest, NTLMv1 et RC4. Une application interne encore dépendante de ces protocoles cesse de fonctionner sans avertissement préalable.
- **Activation forcée sur un contrôleur de domaine.** Microsoft exclut explicitement les contrôleurs de domaine de l’activation par défaut de Credential Guard. Forcer cette activation peut casser certains flux Kerberos spécifiques aux DC.
- **Sous-dimensionnement matériel.** VBS et HVCI consomment de la mémoire et du CPU. Sur un serveur SQL ou RDS déjà proche de la saturation, l’activation sans marge matérielle dégrade les performances perçues par les utilisateurs.
- **Verrouillage UEFI oublié en cas d’urgence.** Le mode « Activé avec verrouillage UEFI » empêche toute désactivation à distance de Credential Guard, y compris en cas de besoin légitime urgent. Documentez la procédure de désactivation physique avant d’en avoir besoin, pas pendant un incident.

## Dépannage : problèmes fréquents et solutions

Voici les incidents les plus rencontrés lors du déploiement, avec la cause probable et la correction à appliquer.

| Symptôme | Cause probable | Solution | 
|---|---|---|
| VBS reste inactif après application de la GPO | Secure Boot désactivé ou firmware en mode Legacy | Repasser le firmware en UEFI natif et activer Secure Boot avant de relancer la GPO | 
| Credential Guard indique « configuré » mais pas « en cours d’exécution » | Redémarrage manquant après application de la stratégie | Redémarrer le serveur ; Credential Guard ne s’active jamais à chaud | 
| Écran bleu au démarrage après activation de HVCI | Pilote en mode noyau incompatible | Démarrer en mode sans échec, identifier le pilote via l’observateur d’événements, mettre à jour ou retirer le pilote | 
| Get-Tpm renvoie TpmPresent : False | TPM désactivé dans le firmware ou en mode 1.2 non reconnu | Activer le TPM dans le BIOS/UEFI ou activer le TPM virtuel sur une VM Generation 2 | 
| Install-Module OSConfig échoue | Absence d’accès à PowerShell Gallery ou politique d’exécution restrictive | Configurer un proxy sortant vers PowerShell Gallery ou héberger le module sur un dépôt interne NuGet | 
| BitLocker demande la clé de récupération à chaque démarrage | Mesures TPM modifiées (mise à jour firmware, changement de configuration de boot) | Suspendre BitLocker avant toute mise à jour firmware planifiée, puis réactiver | 
| Une application métier cesse de fonctionner après la baseline | Dépendance à NTLM, WDigest ou RC4 désormais bloqués | Auditer les journaux d’authentification, migrer l’application vers Kerberos AES, ou créer une exception documentée et temporaire | 
| Get-OSConfigCompliance retourne « non conforme » après application | Réglage modifié manuellement après l’application de la baseline, ou dérive locale | Réappliquer la baseline avec Invoke-OSConfigBaseline et investiguer la source de la dérive | 
| Hyper-V refuse de s’installer | Hyperviseur tiers déjà présent ou virtualisation imbriquée non exposée | Désinstaller l’hyperviseur concurrent ou activer l’exposition de la virtualisation au niveau de l’hôte | 
| Un pilote EDR tiers plante après activation de RunAsPPL | Le pilote n’est pas signé pour s’exécuter en tant que Protected Process | Contacter l’éditeur pour une version compatible LSA Protection ou activer une exception ciblée documentée | 

## Exemple de sortie attendue après un déploiement réussi

Sur un serveur correctement durci, la commande `Get-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Lsa' -Name LsaCfgFlags` doit renvoyer la valeur `1` (activé avec verrouillage UEFI). L’outil `msinfo32` ou la commande `Get-ComputerInfo | Select-Object DeviceGuardSecurityServicesRunning` doit lister à la fois `CredentialGuard` et `HypervisorEnforcedCodeIntegrity` comme services actifs. Une exécution de `Get-OSConfigCompliance -Name 'WindowsServer2025-MemberServer'` conforme affiche un statut global « Compliant » avec le détail des quelque 300 réglages contrôlés, sans écart non justifié. Si l’un de ces indicateurs manque après redémarrage, reprenez la vérification des prérequis matériels avant de recommencer les étapes de configuration.

## Conseils avancés pour la production et projet complet clé en main

En production, ne déployez jamais Credential Guard et VBS directement sur l’ensemble du parc. Constituez d’abord un groupe pilote représentatif (serveurs de fichiers, serveurs applicatifs, serveurs SQL) et laissez tourner la configuration en mode audit OSConfig pendant au moins deux semaines avant de basculer en application forcée. Utilisez le rapport de conformité pour identifier les écarts avant qu’ils ne deviennent des incidents en heures ouvrées.

Pour les environnements soumis à la directive NIS2, documentez chaque étape de ce durcissement dans votre dossier de conformité : Credential Guard et les baselines OSConfig répondent directement aux exigences de gestion des accès privilégiés et de réduction de la surface d’attaque exigées par le texte. Combinez ce travail avec une gestion rigoureuse des secrets applicatifs, par exemple via un coffre-fort centralisé, pour éviter que des identifiants de service ne circulent en clair dans des scripts de déploiement.

Prévoyez également une procédure de retour arrière documentée avant chaque vague de déploiement. Si un serveur critique refuse de redémarrer après application de la baseline OSConfig, la restauration passe par un démarrage en mode sans échec avec réseau, suivi d’une désactivation temporaire de la clé `EnableVirtualizationBasedSecurity` le temps d’identifier la cause. Conservez toujours une sauvegarde d’état système récente et testez la procédure de restauration sur l’environnement pilote avant de l’appliquer à des serveurs de production critiques. Sur le plan budgétaire, ce durcissement ne nécessite aucun investissement matériel si vos serveurs datent de moins de cinq ans : la quasi-totalité des serveurs vendus depuis 2021 intègrent nativement UEFI, Secure Boot et un TPM 2.0. Le coût réel se situe dans le temps d’ingénierie nécessaire pour tester la compatibilité applicative, généralement estimé entre deux et quatre semaines pour un parc de taille moyenne, en fonction du nombre d’applications legacy à auditer.

Voici un script consolidé qui reprend l’essentiel des étapes de ce tutoriel pour un déploiement automatisé sur un serveur membre, à adapter selon votre politique interne et à tester impérativement en préproduction avant tout usage en production :

```
# Script de durcissement Windows Server 2025 - à exécuter en administrateur
# Étape 1 : audit matériel
$secureBoot = Confirm-SecureBootUEFI
$tpm = Get-Tpm
if (-not $secureBoot -or -not $tpm.TpmPresent) {
    Write-Warning "Prérequis matériels non satisfaits. Arrêt du script."
    exit 1
}
# Étape 2 : installation Hyper-V (fondation VBS)
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
# Étape 3 : activation VBS et Credential Guard (verrouillage UEFI)
New-Item -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard' -Force | Out-Null
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard' `
    -Name 'EnableVirtualizationBasedSecurity' -Type DWord -Value 1
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Lsa' `
    -Name 'LsaCfgFlags' -Type DWord -Value 1
# Étape 4 : LSA Protection (RunAsPPL)
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Lsa' `
    -Name 'RunAsPPL' -Type DWord -Value 1
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Lsa' `
    -Name 'RunAsPPLBoot' -Type DWord -Value 1
# Étape 5 : chiffrement BitLocker (recommandation ANSSI)
Enable-BitLocker -MountPoint "C:" -TpmProtector
# Étape 6 : NTP hiérarchie de domaine (recommandation ANSSI)
w32tm /config /syncfromflags:domhier /update
w32tm /resync
# Étape 7 : application de la baseline OSConfig
Install-Module -Name OSConfig -Scope AllUsers -Force
Import-Module OSConfig
Invoke-OSConfigBaseline -Name 'WindowsServer2025-MemberServer' `
    -ComplianceLogPath 'C:\OSConfig\Logs\MemberServer.json' -Confirm:$false
Write-Output "Durcissement appliqué. Redémarrage requis pour activer VBS/Credential Guard."
Restart-Computer -Confirm
```
## Windows Server 2022 vs 2025 : ce qui change côté sécurité

La différence la plus visible entre les deux versions tient à la posture par défaut. Windows Server 2022 exigeait une activation manuelle complète de Credential Guard et de VBS ; Windows Server 2025 les active automatiquement dès lors que le matériel le permet, ce qui réduit le risque d’oubli lors d’un déploiement rapide. Pour les organisations qui migrent progressivement leur parc de Windows Server 2022 vers 2025, ce changement de posture par défaut signifie qu’un serveur fraîchement migré peut se retrouver avec des protections plus strictes que prévu, ce qui casse parfois des scripts d’administration hérités qui n’avaient jamais été testés avec Credential Guard actif. Planifiez systématiquement une phase de test post-migration dédiée à ce point avant de considérer la bascule comme terminée.

| Aspect | Windows Server 2022 | Windows Server 2025 | 
|---|---|---|
| Credential Guard | Désactivé par défaut, activation manuelle requise | Activé par défaut sur serveurs membres éligibles | 
| Gestion des baselines | Security Compliance Toolkit (GPO uniquement) | Security Compliance Toolkit + module OSConfig (local et centralisé) | 
| Détection de dérive de configuration | Limitée, dépendante d’outils tiers | Intégrée nativement via Get-OSConfigCompliance | 
| LSA Protection (RunAsPPL) | Configuration manuelle recommandée | Intégrée aux baselines de sécurité par défaut | 
| Authentification legacy (WDigest, NTLMv1, RC4) | Encore présente sauf durcissement explicite | Désactivée par défaut dans les baselines mises à jour | 

## Foire aux questions

**Qu’est-ce que Credential Guard et pourquoi est-il activé par défaut sur Windows Server 2025 ?**

Credential Guard isole les identifiants dérivés (hashs NTLM, tickets Kerberos) dans un conteneur mémoire protégé par l’hyperviseur, hors de portée d’un attaquant disposant de privilèges administrateur locaux. Microsoft l’active par défaut sur les serveurs membres éligibles depuis Windows Server 2025 pour réduire le nombre de déploiements qui restent vulnérables faute d’activation manuelle.

**VBS ralentit-il les performances du serveur ?**

Oui, dans une mesure limitée. VBS et HVCI ajoutent une consommation supplémentaire de mémoire et de CPU liée à l’isolation par l’hyperviseur. Sur un serveur correctement dimensionné, l’impact reste marginal ; sur un serveur déjà proche de la saturation (SQL Server, RDS sous forte charge), prévoyez une marge matérielle avant d’activer ces protections.

**Credential Guard doit-il être activé sur les contrôleurs de domaine ?**

Non, pas par la méthode par défaut. Microsoft exclut explicitement les contrôleurs de domaine de l’activation automatique de Credential Guard, en raison de scénarios Kerberos spécifiques qui peuvent être affectés. Suivez les recommandations dédiées aux DC plutôt que d’étendre la configuration serveur membre sans validation préalable.

**Quelle est la différence entre Credential Guard et LSA Protection (RunAsPPL) ?**

Credential Guard protège les secrets eux-mêmes en les déplaçant hors de la mémoire accessible par LSASS. LSA Protection (RunAsPPL) protège le processus LSASS contre l’injection de code malveillant. Les deux mécanismes sont complémentaires et se déploient ensemble dans une configuration durcie.

**Le module OSConfig remplace-t-il le Security Compliance Toolkit ?**

Non, les deux coexistent. Le Security Compliance Toolkit reste pertinent pour un pilotage centralisé via GPO, tandis qu’OSConfig ajoute une couche d’application locale et de détection de dérive, particulièrement utile pour les serveurs Core ou peu connectés au réseau d’entreprise.

**Comment vérifier que Credential Guard fonctionne réellement après activation ?**

Exécutez `Get-ComputerInfo | Select-Object DeviceGuardSecurityServicesRunning` après redémarrage : la sortie doit lister `CredentialGuard` parmi les services actifs. Un simple réglage de stratégie sans ce contrôle post-redémarrage ne garantit pas que la protection est réellement opérationnelle.

**Que faire si mes anciens pilotes ne sont pas compatibles avec HVCI ?**

Identifiez le pilote fautif via l’observateur d’événements après un démarrage en mode sans échec, puis contactez l’éditeur pour une version signée compatible avec l’intégrité du code appliquée par l’hyperviseur. En dernier recours, documentez une exception temporaire tout en planifiant le remplacement du composant incompatible.

**Les recommandations ANSSI sont-elles obligatoires en France ?**

Elles ne constituent pas une obligation légale directe pour toutes les organisations, mais elles deviennent de facto une référence d’exigence pour les entités soumises à la directive NIS2 ou à des obligations sectorielles renforcées. Suivre ce guide ANSSI facilite la démonstration de conformité lors d’un audit de sécurité.

**Combien de temps prend un déploiement complet sur un parc de production ?**

Comptez environ 120 minutes pour dérouler les 13 étapes sur un serveur pilote, puis deux à quatre semaines pour une bascule complète en production, en incluant l’audit des applications legacy, la phase pilote en mode audit OSConfig et le déploiement progressif par vagues d’unités d’organisation. Ne tentez jamais un déploiement massif en une seule opération sur un parc de production sans phase pilote préalable.

### Related Coverage

Sources externes citées dans ce tutoriel : documentation officielle Microsoft sur la configuration de Credential Guard, vue d’ensemble de Credential Guard, le guide ANSSI de sécurisation d’un serveur Windows autonome, le benchmark CIS pour Microsoft Windows Server, le site officiel de l’ANSSI et le bulletin de sécurité Microsoft d’août 2026.
