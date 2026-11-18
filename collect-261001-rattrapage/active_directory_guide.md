# Active Directory & GPO en entreprise — Guide technique ultra-complet

> **Public** : Zelef, chef de service systèmes & énergies.
> **Objectif** : tout maîtriser d'Active Directory Domain Services (AD DS) et des stratégies de groupe (GPO) en environnement d'entreprise, de l'architecture au dépannage.
> **Versions couvertes** : Windows Server 2019, 2022 et 2025.
> **Ton** : direct, dense, pratique. Chaque section = théorie + commandes + pièges à éviter.
> **Avertissement global** : testez toujours les commandes destructrices (suppression, restauration, FSMO) dans un labo avant la production.

---

## Sommaire

1. Introduction : pourquoi Active Directory ?
2. Vocabulaire et concepts fondamentaux
3. Forêt, arbre, domaine : architecture logique
4. Approbations (trusts) entre domaines et forêts
5. Catalogue global (Global Catalog)
6. Partitions d'annuaire
7. Contrôleurs de domaine : rôles et architecture
8. Prérequis avant de promouvoir un contrôleur de domaine
9. Promouvoir le premier DC d'une nouvelle forêt (PowerShell)
10. Ajouter un DC supplémentaire dans un domaine existant
11. Rétrograder un contrôleur de domaine proprement
12. Retrait forcé d'un DC mort (metadata cleanup)
13. RODC : contrôleur de domaine en lecture seule
14. Scénario agence : déployer un RODC en site distant
15. Design des unités d'organisation (OU)
16. Bonnes pratiques de structure OU : par site vs par département
17. Protection contre la suppression accidentelle des OU
18. Délégation d'administration
19. Créer des utilisateurs : ADAC vs PowerShell
20. Création en masse via CSV (New-ADUser)
21. Attributs utilisateur importants et bonnes pratiques
22. Désactiver, déplacer, supprimer des comptes : cycle de vie
23. Groupes : portées (global, universel, local de domaine)
24. Imbrication de groupes : stratégie AGDLP/AGUDLP
25. Groupes dynamiques : état des lieux et alternatives
26. Contacts, ordinateurs et comptes de service
27. Comptes de service gérés (gMSA)
28. Les 5 rôles FSMO : vue d'ensemble
29. Maître de schéma (Schema Master)
30. Maître d'attribution des noms de domaine (Domain Naming Master)
31. Maître RID (Relative ID Master)
32. Émulateur PDC (PDC Emulator)
33. Maître d'infrastructure (Infrastructure Master)
34. Transférer un rôle FSMO (Move-ADDirectoryServerOperationMasterRole)
35. Saisir un rôle FSMO (seize via ntdsutil) — procédure d'urgence
36. Bonnes pratiques de placement des rôles FSMO
37. Sites et services : concepts
38. Créer et configurer des sites, sous-réseaux
39. Liaisons de sites : coût, planification, bridgehead
40. Réplication AD : KCC et topologie
41. Surveiller la réplication : repadmin
42. Diagnostiquer avec dcdiag
43. Dépannage réplication : USN rollback
44. Dépannage réplication : lingering objects (objets rémanents)
45. Stratégie de mot de passe par défaut du domaine
46. FGPP : stratégies de mot de passe affinées (PSO)
47. Bonnes pratiques mots de passe et verrouillage de compte
48. Corbeille AD : activation et fonctionnement
49. Restaurer un objet supprimé (Restore-ADObject)
50. Sauvegarde : état système avec wbadmin
51. Sauvegarder via ntdsutil et bonnes pratiques de sauvegarde
52. DSRM : mode de restauration des services d'annuaire
53. Restauration non faisant autorité vs faisant autorité
54. Migration inter-forêts avec ADMT
55. Mise à niveau du niveau fonctionnel (forêt et domaine)
56. GPO : principes fondamentaux
57. Ordre d'application : LSDOU
58. Héritage, blocage d'héritage et application forcée (Enforced)
59. Filtrage de sécurité des GPO
60. Filtrage WMI
61. Bouclage de traitement (loopback) : fusion vs remplacement
62. Préférences de stratégie de groupe (GPP)
63. GPP : lecteurs réseau, imprimantes, registre, raccourcis
64. Ciblage au niveau élément (Item-Level Targeting)
65. GPO de sécurité : audit et droits utilisateur
66. Déploiement de logiciels via GPO (MSI)
67. Scripts de démarrage / arrêt / ouverture / fermeture de session
68. Dépannage GPO : gpresult, rsop.msc, journaux
69. GPO qui ne s'applique pas : méthodologie complète
70. Bonnes pratiques de nommage et gestion du cycle de vie des GPO
71. Santé du domaine : checklist quotidienne
72. Santé du domaine : checklist hebdomadaire et mensuelle
73. Scripts de rapport automatisés
74. Supervision : compteurs et alertes critiques
75. Pense-bête de poche : commandes essentielles
76. Glossaire
77. Quiz : 10 questions + réponses
78. Erreurs classiques (18)
79. Cas pratiques commentés (16)
80. Pour aller plus loin
81. Checklists de mise en production
82. Annexe : versions et niveaux fonctionnels

---

## 1. Introduction : pourquoi Active Directory ?

Active Directory Domain Services (AD DS) est le service d'annuaire de Microsoft. En entreprise, il centralise :

- **L'authentification** : un seul identifiant pour ouvrir sa session, accéder aux partages, aux imprimantes, aux applications (SSO via Kerberos).
- **L'autorisation** : qui a le droit de faire quoi, via les groupes de sécurité.
- **La gestion centralisée** : stratégies de groupe (GPO) pour configurer des milliers de postes depuis un point unique.
- **L'inventaire** : chaque utilisateur, ordinateur, imprimante est un objet avec des attributs interrogeables.

Sans AD, chaque poste gère ses comptes locaux : cauchemar dès 20+ machines. Avec AD, un départ = un compte désactivé, et l'accès est coupé partout.

**Ce que ce guide n'est pas** : un remplacement pour la documentation Microsoft ni pour un labo. Chaque commande destructrice doit être testée hors production.

**Versions** : les cmdlets PowerShell du module `ActiveDirectory` sont stables depuis 2012 R2. Les différences 2019/2022/2025 concernent surtout les niveaux fonctionnels, la sécurité (LDAP signing, SMB) et quelques nouveautés (voir section 82). Quand une commande dépend de la version, c'est signalé par ⚠️.

---

## 2. Vocabulaire et concepts fondamentaux

| Terme | Définition |
|---|---|
| **Annuaire** | Base de données hiérarchique stockant des objets (utilisateurs, ordinateurs, groupes...) et leurs attributs. Fichier physique : `C:\Windows\NTDS\ntds.dit`. |
| **Objet** | Élément de l'annuaire (utilisateur, groupe, OU, ordinateur, imprimante...). |
| **Attribut** | Propriété d'un objet (`sAMAccountName`, `userPrincipalName`, `memberOf`...). |
| **Schéma** | Définition des classes d'objets et attributs autorisés. Commun à toute la forêt. |
| **DN (Distinguished Name)** | Chemin unique d'un objet : `CN=Zelef,OU=Utilisateurs,DC=entreprise,DC=lan`. |
| **CN / OU / DC** | Common Name, Organizational Unit, Domain Component (composants du DN). |
| **UPN** | User Principal Name, format `prenom.nom@entreprise.lan` — ce que l'utilisateur tape à l'ouverture de session. |
| **sAMAccountName** | Nom de connexion pré-Windows 2000, format `ENTREPRISE\zelef`, limité à 20 caractères. |
| **SID** | Identifiant de sécurité unique (`S-1-5-21-...`). Les droits portent sur les SID, pas sur les noms. |
| **Forêt** | Ensemble de domaines partageant le même schéma et le même catalogue global. Frontière de sécurité. |
| **Domaine** | Frontière de réplication et d'administration (stratégies de mot de passe, etc.). |

**Point clé à retenir** : les autorisations NTFS et les appartenances aux groupes sont liées aux **SID**, pas aux noms. Renommer un utilisateur ne casse rien ; supprimer puis recréer un compte (nouveau SID) casse tout.

```powershell
# Voir le SID d'un utilisateur
Get-ADUser zelef | Select-Object Name, SID

# Voir le DN
Get-ADUser zelef | Select-Object DistinguishedName
```

---

## 3. Forêt, arbre, domaine : architecture logique

```
Forêt entreprise.lan
├── Arbre entreprise.lan
│   ├── Domaine racine : entreprise.lan
│   ├── Domaine enfant : prod.entreprise.lan
│   └── Domaine enfant : dev.entreprise.lan
└── Arbre filiale.lan (même forêt, espace de noms disjoint)
    └── Domaine racine : filiale.lan
```

- **Domaine** : unité de réplication. Chaque DC d'un domaine réplique la partition de ce domaine avec les autres DC du même domaine.
- **Arbre** : ensemble de domaines liés par des relations parent-enfant avec un espace de noms DNS contigu (`prod.entreprise.lan` sous `entreprise.lan`).
- **Forêt** : un ou plusieurs arbres. Le **premier domaine créé** est le domaine racine de forêt. Le schéma et la configuration sont communs à toute la forêt.

**Règle d'or du design** : **une seule forêt, un seul domaine** dans 95 % des PME/ETI. Chaque domaine supplémentaire = des DC en plus, des approbations à gérer, des GPO à dupliquer. On ne crée un domaine ou une forêt supplémentaire que pour :

- une vraie frontière de sécurité (exigence réglementaire, isolement total) ;
- un besoin d'autonomie administrative complète (rare) ;
- une entité juridique séparée avec sa propre équipe IT.

> ⚠️ On ne peut pas fusionner deux forêts facilement ni renommer un domaine sans douleur (rendom existe mais c'est risqué, Exchange ne le supporte pas). **Choisissez bien le nom de domaine dès le départ** : utilisez un sous-domaine d'un nom public que vous possédez (ex. `ad.entreprise.fr` ou `interne.entreprise.fr`), jamais un `.local` (conflits mDNS/Bonjour) ni un nom public nu sans réflexion.

---

## 4. Approbations (trusts) entre domaines et forêts

Une **approbation** (trust) permet aux utilisateurs d'un domaine d'accéder aux ressources d'un autre.

| Type | Portée | Création |
|---|---|---|
| Approbation **parent-enfant** | Automatique, transitive, bidirectionnelle | Créée à la promotion du domaine enfant |
| Approbation **racine d'arborescence** | Entre racines d'arbres d'une même forêt, transitive | Automatique |
| Approbation **de raccourci** (shortcut) | Entre deux domaines d'une même forêt pour optimiser l'authentification | Manuelle |
| Approbation **externe** | Entre domaines de forêts différentes, non transitive | Manuelle |
| Approbation **de forêt** | Entre deux forêts entières, transitive | Manuelle |

```powershell
# Lister les approbations du domaine
Get-ADTrust -Filter *

# Créer une approbation de forêt (à exécuter côté initiateur)
# Le mot de passe d'approbation doit être identique des deux côtés
New-ADTrust -SourceForest "entreprise.lan" -TargetForest "partenaire.lan" `
  -TrustType Forest -TrustDirection Bidirectional `
  -TrustPassword (Read-Host "Mot de passe d'approbation" -AsSecureString)

# Vérifier une approbation
netdom trust entreprise.lan /domain:partenaire.lan /verify
```

**Direction** : entrante, sortante ou bidirectionnelle. Une approbation **entrante** vers votre domaine signifie que l'autre domaine vous fait confiance (leurs utilisateurs accèdent à vos ressources).

**Sécurité** : utilisez le **filtrage SID** (activé par défaut sur les approbations inter-forêts) pour empêcher l'usurpation de SID. Pour les environnements sensibles, préférez l'**authentification sélective** : seuls les utilisateurs explicitement autorisés (droit `Allowed to authenticate`) peuvent traverser.

```powershell
# Activer l'authentification sélective sur une approbation
netdom trust entreprise.lan /domain:partenaire.lan /SelectiveAUTH:Yes
```

---

## 5. Catalogue global (Global Catalog)

Le **catalogue global (GC)** est un index partiel de **tous les objets de la forêt** : il contient tous les attributs de son propre domaine + un sous-ensemble d'attributs des autres domaines (ceux marqués dans le schéma comme répliqués vers le GC, ex. `sAMAccountName`, `userPrincipalName`).

**À quoi ça sert** :

1. **Ouverture de session** : pour les groupes universels, le DC doit interroger un GC pour calculer l'appartenance complète.
2. **Recherche inter-domaines** : trouver un utilisateur d'un autre domaine sans connaître son domaine.
3. **Applications** : Exchange/Outlook s'appuient historiquement sur le GC.

```powershell
# Lister les GC de la forêt
Get-ADDomainController -Filter { IsGlobalCatalog -eq $true } |
  Select-Object Name, Domain, Site

# Activer le rôle GC sur un DC
Set-ADObject "CN=NTDS Settings,CN=DC01,CN=Servers,CN=Site-Paris,CN=Sites,CN=Configuration,DC=entreprise,DC=lan" `
  -Add @{ options = 1 }
# Méthode simple via l'interface : Sites et services AD > serveur > Propriétés NTDS Settings > cocher "Catalogue global"
```

**Bonne pratique** : dans un domaine unique, faites de **tous les DC des GC** (aucun inconvénient). En multi-domaines, placez au moins un GC par site. Attention au rôle **maître d'infrastructure** : ne le colocalisez pas avec un GC sauf si tous les DC sont GC ou s'il n'y a qu'un seul domaine (voir section 33).

**Port** : le GC répond sur TCP **3268** (LDAP GC) et **3269** (LDAPS GC).

---

## 6. Partitions d'annuaire

La base AD est découpée en **partitions** (naming contexts), chacune avec sa propre portée de réplication :

| Partition | Contenu | Répliquée vers |
|---|---|---|
| **Domaine** (`DC=entreprise,DC=lan`) | Utilisateurs, groupes, OU, ordinateurs du domaine | Tous les DC du domaine |
| **Configuration** (`CN=Configuration,...`) | Topologie des sites, schéma de réplication | Tous les DC de la forêt |
| **Schéma** (`CN=Schema,...`) | Définitions des classes et attributs | Tous les DC de la forêt |
| **DNS d'application** (`DC=DomainDnsZones,...`) | Zones DNS intégrées à AD (portée domaine) | Tous les DC DNS du domaine |
| **DNS d'application** (`DC=ForestDnsZones,...`) | Zones DNS intégrées à AD (portée forêt) | Tous les DC DNS de la forêt |

```powershell
# Lister les partitions connues par un DC
Get-ADRootDSE | Select-Object -ExpandProperty namingContexts

# Voir les détails via le RootDSE
(Get-ADRootDSE).Properties["namingContexts"]
```

**Implication pratique** : une modification du schéma ou de la configuration se réplique à **toute la forêt**. C'est pour cela que le rôle de maître de schéma est si sensible (section 29).

---

## 7. Contrôleurs de domaine : rôles et architecture

Un **contrôleur de domaine (DC)** héberge une copie inscriptible (ou en lecture seule pour un RODC) de la base AD et fournit :

- **Authentification** : Kerberos (port 88) et NTLM.
- **LDAP** : annuaire sur TCP 389 / 636 (LDAPS).
- **DNS** : quasi toujours intégré à AD (recommandé).
- **Réplication** : avec les autres DC.

**Ports à ouvrir entre DC** (pare-feu) : 53 (DNS), 88 (Kerberos), 135 (RPC endpoint mapper), 389 (LDAP), 445 (SMB), 636 (LDAPS), 3268/3269 (GC), plage RPC dynamique 49152-65535 (ou restreinte, voir ci-dessous).

```powershell
# Restreindre la plage RPC de réplication (bonne pratique pare-feu)
# À faire sur chaque DC, puis redémarrer
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\NTDS\Parameters" `
  -Name "TCP/IP Port" -Value 50000 -Type DWord
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters" `
  -Name "DCTcpipPort" -Value 50001 -Type DWord
```

⚠️ **Ne jamais** : faire un snapshot/restore sauvage d'un DC virtualisé sans précaution (risque USN rollback, section 43), mettre un DC en pause prolongée, cloner un DC.

**Virtualisation** : depuis 2012, les DC virtualisés sont protégés par le **VM-GenerationID** (Hyper-V, VMware) à condition que l'hyperviseur et l'OS invité soient récents. Règle : un DC virtualisé se sauvegarde et se restaure **comme un DC** (état système), jamais par simple snapshot.

---

## 8. Prérequis avant de promouvoir un contrôleur de domaine

Checklist **avant** `Install-ADDSDomainController` / `Install-ADDSForest` :

- [ ] **Nom du serveur** définitif (renommer un DC après coup = douloureux, possible mais à éviter).
- [ ] **IP statique** configurée, DNS pointant vers lui-même (ou vers le DC existant pour un DC supplémentaire, puis vers lui-même une fois promu).
- [ ] **Rôle AD DS** installé : `Install-WindowsFeature AD-Domain-Services -IncludeManagementTools`.
- [ ] **Fuseau horaire et heure corrects** (Kerberos tolère 5 min de décalage par défaut).
- [ ] **Disque NTFS** avec espace suffisant pour `ntds.dit` + SYSVOL (jamais sur un lecteur compressé/chiffré EFS).
- [ ] **DNS** : le nom de domaine doit se résoudre ; pour une nouvelle forêt, le serveur peut s'auto-héberger.
- [ ] **Compte** membre des Admins de l'entreprise (nouvelle forêt) ou Admins du domaine (DC supplémentaire).
- [ ] **Niveau fonctionnel** cible décidé (2016 mini recommandé ; 2025 si tout le parc est en 2022/2025, voir section 82).
- [ ] **Mot de passe DSRM** choisi et **stocké dans le coffre** (section 52).

```powershell
# Installer le rôle AD DS
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools

# Tester les prérequis d'une nouvelle forêt (ne fait rien, rapporte seulement)
Test-ADDSDomainControllerInstallation -DomainName "entreprise.lan" -Credential (Get-Credential)
```

---

## 9. Promouvoir le premier DC d'une nouvelle forêt (PowerShell)

```powershell
# Promouvoir le serveur en premier DC d'une nouvelle forêt
Install-ADDSForest `
  -DomainName "ad.entreprise.fr" `
  -DomainNetbiosName "ENTREPRISE" `
  -ForestMode "WinThreshold" `      # 2016+ : "WinThreshold" ; 2025 : "Win2025" (voir section 82)
  -DomainMode "WinThreshold" `
  -InstallDns `
  -CreateDnsDelegation:$false `
  -DatabasePath "C:\Windows\NTDS" `
  -LogPath "C:\Windows\NTDS" `
  -SysvolPath "C:\Windows\SYSVOL" `
  -NoRebootOnCompletion:$false `
  -SafeModeAdministratorPassword (Read-Host "Mot de passe DSRM" -AsSecureString) `
  -Force
```

**Ce qui se passe** : création de la forêt, du domaine racine, du schéma, du partage SYSVOL, installation et configuration du DNS intégré, le serveur devient GC et détient les 5 rôles FSMO.

**Après la promotion** :

```powershell
# Vérifier que le DC fonctionne
dcdiag /v | Select-String "passed|failed"
Get-ADDomain | Select-Object Name, DomainMode
Get-ADForest | Select-Object Name, ForestMode
```

---

## 10. Ajouter un DC supplémentaire dans un domaine existant

```powershell
# Sur le futur DC : pointer son DNS vers un DC existant, joindre le domaine, puis :
Install-ADDSDomainController `
  -DomainName "ad.entreprise.fr" `
  -Credential (Get-Credential "ENTREPRISE\administrateur") `
  -InstallDns `
  -CreateDnsDelegation:$false `
  -DatabasePath "C:\Windows\NTDS" `
  -LogPath "C:\Windows\NTDS" `
  -SysvolPath "C:\Windows\SYSVOL" `
  -NoRebootOnCompletion:$false `
  -SafeModeAdministratorPassword (Read-Host "Mot de passe DSRM" -AsSecureString) `
  -Force
```

**Options utiles** :

```powershell
# Installer depuis un média (IFM) pour les liaisons lentes : réplication initiale allégée
ntdsutil "activate instance ntds" "ifm" "create sysvol full C:\IFM" quit quit
# Puis :
Install-ADDSDomainController -DomainName "ad.entreprise.fr" `
  -InstallationMediaPath "C:\IFM" -Credential (Get-Credential) -Force
```

**Post-installation** :

1. Vérifier la réplication : `repadmin /showrepl`, `repadmin /replsummary`.
2. Vérifier le DNS : le nouveau DC doit apparaître dans la zone `_msdcs`.
3. Le passer en GC si besoin (coché par défaut).
4. Mettre à jour les clients/serveurs DHCP pour inclure son IP en DNS secondaire.
5. Vérifier SYSVOL partagé : `net share`, `dcdiag /test:netlogons`.

---

## 11. Rétrograder un contrôleur de domaine proprement

**Jamais** de simple extinction ou de suppression de VM : toujours **rétrograder** (demote) pour nettoyer les métadonnées.

```powershell
# Transférer d'abord les rôles FSMO si le DC en détient (voir section 34)
Move-ADDirectoryServerOperationMasterRole -Identity "DC02" -OperationMasterRole 0,1,2,3,4

# Rétrograder (le serveur devient membre simple)
Uninstall-ADDSDomainController `
  -DemoteOperationMasterRole `
  -RemoveDnsDelegation:$false `
  -Force `
  -Credential (Get-Credential "ENTREPRISE\administrateur")
# Le serveur redémarre. Le rôle AD DS peut ensuite être désinstallé :
Uninstall-WindowsFeature AD-Domain-Services -IncludeManagementTools
```

⚠️ **Dernier DC du domaine** : `Uninstall-ADDSDomainController` avec `-LastDomainControllerInDomain` ; **dernier DC de la forêt** : `-LastDomainInForest`. Ces cas détruisent le domaine/la forêt : triple vérification + sauvegarde au préalable.

**Vérifications après rétrogradation** :

```powershell
# Le DC ne doit plus apparaître
Get-ADDomainController -Filter *
# Nettoyer les enregistrements DNS résiduels (_msdcs, zone directe)
# Vérifier la réplication sur les DC restants
repadmin /replsummary
```

---

## 12. Retrait forcé d'un DC mort (metadata cleanup)

Quand un DC est **mort sans rétrogradation** (crash disque, VM supprimée), ses métadonnées polluent l'annuaire. Nettoyage :

```powershell
# 1. Identifier le DC mort
Get-ADDomainController -Filter * | Select-Object Name, Enabled

# 2. Supprimer l'objet serveur (fait le metadata cleanup moderne)
# Via Sites et services AD : supprimer l'objet serveur -> confirme la suppression des paramètres NTDS
# Ou en PowerShell :
Remove-ADDomainController -Identity "DC03" -Force
# (échoue si le DC est encore joignable : c'est une sécurité)

# 3. Saisir les rôles FSMO qu'il détenait éventuellement (voir section 35)
# 4. Nettoyer le DNS : supprimer les enregistrements A, NS et _msdcs du DC mort
# 5. Vérifier qu'il ne reste rien :
Get-ADObject -Filter { Name -eq "DC03" }
repadmin /replsummary
dcdiag /v
```

L'ancienne méthode `ntdsutil > metadata cleanup` existe toujours mais `Remove-ADDomainController` et la suppression via la console font le travail proprement depuis 2008 R2.

---

## 13. RODC : contrôleur de domaine en lecture seule

Un **RODC** (Read-Only Domain Controller) héberge une copie **non inscriptible** de la base AD. Cas d'usage typiques :

- **Agence / site distant** sans local technique sécurisé (vol du serveur = pas de base inscriptible à exploiter).
- **DMZ / réseau exposé** : authentifier sans exposer un DC inscriptible.
- **Contrainte réglementaire** : séparer strictement l'administration.

**Caractéristiques** :

- Ne réplique qu'en **entrant** (jamais de réplication sortante).
- Par défaut, **ne met en cache aucun mot de passe** : chaque authentification est relayée vers un DC inscriptible (sauf si la stratégie de réplication de mot de passe l'autorise).
- Ne détient **aucun rôle FSMO**, n'est jamais GC par défaut (peut le devenir).
- Les modifications locales (changement de mot de passe sur site) sont relayées vers un DC inscriptible.

```powershell
# Promouvoir un RODC (le compte doit être pré-créé ou droits suffisants)
Install-ADDSDomainController `
  -DomainName "ad.entreprise.fr" `
  -ReadOnlyReplica `
  -SiteName "Site-Agence-Lyon" `
  -Credential (Get-Credential) `
  -SafeModeAdministratorPassword (Read-Host "DSRM" -AsSecureString) `
  -Force

# Stratégie de réplication des mots de passe : autoriser la mise en cache
# pour un groupe (ex. utilisateurs de l'agence)
Add-ADDomainControllerPasswordReplicationPolicy -Identity "RODC-Lyon" `
  -AllowedList "GG_Utilisateurs_Lyon"

# Voir qui a un mot de passe en cache sur le RODC
Get-ADDomainControllerPasswordReplicationPolicy -Identity "RODC-Lyon" -RevealedList
Get-ADDomainControllerPasswordReplicationPolicyUsage -Identity "RODC-Lyon" -RevealedAccounts
```

**Bonnes pratiques RODC** :

- Pré-créez le compte RODC et définissez la stratégie de mot de passe **avant** la promotion.
- **Refusez** explicitement les comptes sensibles (admins du domaine, comptes de service critiques) via `DeniedList`.
- Placez le RODC dans un **site AD dédié** avec le sous-réseau de l'agence.
- Si un RODC est **volé/compromis** : réinitialisez les mots de passe de tous les comptes dont le secret était en cache + supprimez le compte RODC.

---

## 14. Scénario agence : déployer un RODC en site distant

Contexte : agence de 25 personnes à Lyon, liaison VPN vers Paris, pas de local sécurisé.

```powershell
# 1. Créer le site et le sous-réseau (sur un DC inscriptible à Paris)
New-ADReplicationSite -Name "Site-Lyon"
New-ADReplicationSubnet -Name "10.10.20.0/24" -Site "Site-Lyon"

# 2. Créer une liaison de site vers le site principal
New-ADReplicationSiteLink -Name "Lien-Paris-Lyon" `
  -SitesIncluded "Site-Paris","Site-Lyon" -Cost 100 `
  -ReplicationFrequencyInMinutes 60

# 3. Pré-créer le compte RODC avec délégation à un technicien local
Add-ADDomainControllerAccount -DomainControllerAccountName "RODC-Lyon" `
  -DomainName "ad.entreprise.fr" -SiteName "Site-Lyon" `
  -AllowPasswordReplicationAccountName "GG_Utilisateurs_Lyon"

# 4. Sur le serveur de l'agence (joint au domaine), promouvoir en RODC
Install-ADDSDomainController -DomainName "ad.entreprise.fr" -ReadOnlyReplica `
  -SiteName "Site-Lyon" -UseExistingAccount -Credential (Get-Credential) -Force

# 5. Configurer le DNS des postes de Lyon : RODC en primaire, DC Paris en secondaire
```

**Checklist agence** : DHCP local distribuant le RODC en DNS → test d'ouverture de session câble débranché du VPN (échoue si mot de passe non en cache : normal) → puis avec VPN (met en cache) → re-test sans VPN (réussit grâce au cache).

---

## 15. Design des unités d'organisation (OU)

Les OU servent à **trois** choses : appliquer des GPO, déléguer l'administration, structurer l'annuaire. Elles ne sont **pas** une frontière de sécurité (contrairement aux domaines).

**Principes** :

1. **Concevez pour les GPO d'abord** : une OU = un périmètre de stratégie homogène.
2. **Peu profondes** : 2 à 4 niveaux suffisent. Au-delà, l'ordre d'application devient illisible.
3. **Stables** : évitez de réorganiser les OU tous les 6 mois (les GPO liées suivent, mais les scripts et la doc cassent).
4. **Nommage clair** : pas d'accents ni de caractères spéciaux dans les noms d'OU (compatibilité scripts).

**Structure type recommandée** (entreprise multi-sites) :

```
DC=ad,DC=entreprise,DC=fr
├── OU=Admin                    # comptes admin, groupes sensibles
├── OU=Postes
│   ├── OU=Paris                # GPO spécifiques site
│   │   ├── OU=Fixes
│   │   └── OU=Portables
│   └── OU=Lyon
├── OU=Utilisateurs
│   ├── OU=Paris
│   │   ├── OU=Direction
│   │   ├── OU=Exploitation
│   │   └── OU=Maintenance
│   └── OU=Lyon
├── OU=Serveurs
│   ├── OU=ControleursDomaine   # OU par défaut, GPO renforcée
│   ├── OU=Applicatifs
│   └── OU=Hyperviseurs
└── OU=Groupes
```

> 💡 Séparez **utilisateurs** et **ordinateurs** dans des OU distinctes : les GPO « configuration utilisateur » et « configuration ordinateur » ne s'appliquent proprement que si les objets sont bien rangés (sauf bouclage, section 61).

---

## 16. Bonnes pratiques de structure OU : par site, par département

| Approche | Avantages | Inconvénients |
|---|---|---|
| **Par site** (Paris/Lyon) | GPO réseau/imprimantes locales simples, délégation au technicien local | Départements éclatés sur plusieurs OU |
| **Par département** (Compta/Atelier) | GPO métier homogènes | Sites éclatés, imprimantes par site compliquées |
| **Mixte** (recommandée) | Site au niveau 1, département au niveau 2 (ou inverse selon le besoin dominant) | Un peu plus profonde |

**Règle pratique** : si vos GPO diffèrent surtout par **site** (imprimantes, proxy, Wi-Fi), mettez le site au niveau 1. Si elles diffèrent surtout par **métier** (logiciels, droits), mettez le département au niveau 1. Le filtrage de sécurité et le ciblage GPP (sections 59, 64) compensent le reste.

```powershell
# Créer une arborescence d'OU complète
$ous = @(
  "OU=Postes,DC=ad,DC=entreprise,DC=fr",
  "OU=Paris,OU=Postes,DC=ad,DC=entreprise,DC=fr",
  "OU=Fixes,OU=Paris,OU=Postes,DC=ad,DC=entreprise,DC=fr",
  "OU=Portables,OU=Paris,OU=Postes,DC=ad,DC=entreprise,DC=fr",
  "OU=Utilisateurs,DC=ad,DC=entreprise,DC=fr",
  "OU=Paris,OU=Utilisateurs,DC=ad,DC=entreprise,DC=fr"
)
foreach ($ou in $ous) {
  $name = ($ou -split ",")[0] -replace "OU=",""
  $path = ($ou -split ",",2)[1]
  New-ADOrganizationalUnit -Name $name -Path $path -ProtectedFromAccidentalDeletion $true
}
```

---

## 17. Protection contre la suppression accidentelle des OU

Chaque OU (et beaucoup d'objets) porte un drapeau **« Protéger l'objet contre une suppression accidentelle »**. C'est votre ceinture de sécurité n°1.

```powershell
# Vérifier la protection d'une OU
Get-ADOrganizationalUnit -Identity "OU=Postes,DC=ad,DC=entreprise,DC=fr" |
  Select-Object Name, ProtectedFromAccidentalDeletion

# Protéger toutes les OU de premier niveau d'un coup
Get-ADOrganizationalUnit -Filter * -SearchBase "DC=ad,DC=entreprise,DC=fr" -SearchScope OneLevel |
  Set-ADOrganizationalUnit -ProtectedFromAccidentalDeletion $true

# Pour supprimer une OU protégée : d'abord retirer la protection
Set-ADOrganizationalUnit -Identity "OU=Test,DC=ad,DC=entreprise,DC=fr" `
  -ProtectedFromAccidentalDeletion $false
Remove-ADOrganizationalUnit -Identity "OU=Test,DC=ad,DC=entreprise,DC=fr" -Recursive -Confirm:$false
```

> ⚠️ Erreur classique n°1 (voir section 78) : tenter de supprimer une OU et obtenir « accès refusé » → c'est la protection, pas un problème de droits. Et inversement : **activez-la partout**, y compris sur les OU créées par script.

---

## 18. Délégation d'administration

La délégation donne à un groupe des droits **limités** sur une OU, sans en faire des admins du domaine.

**Cas typiques** :

- Techniciens d'agence : réinitialiser les mots de passe de leur site uniquement.
- Équipe helpdesk : déverrouiller les comptes, sans toucher aux groupes sensibles.
- Responsable applicatif : gérer les groupes d'une application.

```powershell
# Méthode 1 : assistant graphique (clic droit sur l'OU > Délégation de contrôle)
# Méthode 2 : PowerShell — ex. permettre au helpdesk de réinitialiser les mots de passe
$ou = "OU=Paris,OU=Utilisateurs,DC=ad,DC=entreprise,DC=fr"
$group = Get-ADGroup "GG_Helpdesk"
# Droits : Reset Password + Change Password sur les objets user descendants
dsacls $ou /G "$($group.SamAccountName):CA;Reset Password;user"
```

**Délégations prédéfinies utiles** (assistant) : « Créer, supprimer et gérer des comptes d'utilisateurs », « Réinitialiser les mots de passe », « Gérer les liaisons de stratégie de groupe ».

**Bonnes pratiques** :

- Déléguez à des **groupes**, jamais à des individus.
- Documentez chaque délégation (qui, quoi, où) — un tableau dans votre wiki.
- **Ne déléguez jamais** sur l'OU `Domain Controllers` ni sur `AdminSDHolder`-protégés sans raison impérieuse.
- Auditez régulièrement : script qui liste les ACL non standard sur les OU.

```powershell
# Lister les entrées de contrôle d'accès d'une OU
(Get-Acl "AD:\OU=Paris,OU=Utilisateurs,DC=ad,DC=entreprise,DC=fr").Access |
  Where-Object { $_.IdentityReference -notlike "*SYSTEM*" -and $_.IdentityReference -notlike "*Administrateurs*" } |
  Format-Table IdentityReference, ActiveDirectoryRights -AutoSize
```

---

## 19. Créer des utilisateurs : ADAC vs PowerShell

**Centre d'administration Active Directory (ADAC, `dsacls.msc`)** : interface moderne, parfait pour la création unitaire et la corbeille. **Utilisateurs et ordinateurs AD (`dsa.msc`)** : l'ancienne console, encore très utilisée.

```powershell
# Création unitaire en PowerShell
New-ADUser -Name "Amina Diallo" `
  -GivenName "Amina" -Surname "Diallo" `
  -SamAccountName "a.diallo" `
  -UserPrincipalName "a.diallo@ad.entreprise.fr" `
  -Path "OU=Exploitation,OU=Paris,OU=Utilisateurs,DC=ad,DC=entreprise,DC=fr" `
  -AccountPassword (ConvertTo-SecureString "MotDePasseTemporaire!2026" -AsPlainText -Force) `
  -Enabled $true -ChangePasswordAtLogon $true `
  -Title "Technicienne maintenance" -Department "Exploitation" -Company "Entreprise"

# Ajouter au groupe du service
Add-ADGroupMember -Identity "GG_Exploitation_Paris" -Members "a.diallo"
```

**Champs à renseigner systématiquement** : nom complet, UPN, service (`Department`), fonction (`Title`), responsable (`Manager`), date d'expiration pour les temporaires (`AccountExpirationDate`). Votre futur vous remerciera pendant les audits.

---

## 20. Création en masse via CSV (New-ADUser)

Le cas le plus courant : une rentrée, 40 intérimaires, un fichier RH.

**Fichier `arrivees.csv`** (UTF-8, séparateur `;`) :

```csv
Prenom;Nom;Login;Service;Site;Fonction
Amina;Diallo;a.diallo;Exploitation;Paris;Technicienne maintenance
Karim;Benali;k.benali;Maintenance;Lyon;Electrotechnicien
```

**Script** :

```powershell
$users = Import-Csv -Path "C:\Scripts\arrivees.csv" -Delimiter ";" -Encoding UTF8
$domain = "ad.entreprise.fr"

foreach ($u in $users) {
  $ou = "OU=$($u.Service),OU=$($u.Site),OU=Utilisateurs,DC=ad,DC=entreprise,DC=fr"
  # Vérifier que l'OU existe
  if (-not (Get-ADOrganizationalUnit -Filter "Name -eq '$($u.Service)'" -SearchBase "OU=$($u.Site),OU=Utilisateurs,DC=ad,DC=entreprise,DC=fr")) {
    Write-Warning "OU introuvable pour $($u.Login), ignoré"
    continue
  }
  # Éviter les doublons
  if (Get-ADUser -Filter "SamAccountName -eq '$($u.Login)'") {
    Write-Warning "Le compte $($u.Login) existe déjà, ignoré"
    continue
  }
  $mdp = ConvertTo-SecureString "Bienvenue!2026" -AsPlainText -Force
  New-ADUser -Name "$($u.Prenom) $($u.Nom)" `
    -GivenName $u.Prenom -Surname $u.Nom `
    -SamAccountName $u.Login `
    -UserPrincipalName "$($u.Login)@$domain" `
    -Path $ou -Department $u.Service -Title $u.Fonction `
    -AccountPassword $mdp -Enabled $true -ChangePasswordAtLogon $true
  Add-ADGroupMember -Identity "GG_$($u.Service)_$($u.Site)" -Members $u.Login
  Write-Host "Créé : $($u.Login)" -ForegroundColor Green
}
```

> 💡 Le mot de passe initial doit être **communiqué de façon sécurisée** (pas par mail en clair) et l'option « changer au premier logon » force l'utilisateur à le personnaliser immédiatement.

---

## 21. Attributs utilisateur importants et bonnes pratiques

| Attribut | Usage |
|---|---|
| `sAMAccountName` | Login court (max 20 car.). Convention : `p.nom`. |
| `userPrincipalName` | Login long `p.nom@domaine`. Alignez-le sur l'email si possible. |
| `displayName` | Affichage dans le carnet d'adresses : « NOM, Prénom » ou « Prénom NOM » (choisissez une convention). |
| `manager` | Responsable hiérarchique (objet lié). Indispensable pour les workflows. |
| `department` / `title` / `company` | Service, fonction, société. Base des tris et des groupes dynamiques. |
| `employeeID` | Matricule RH. Clé de rapprochement avec la SIRH. |
| `accountExpires` | Date de fin pour temporaires/stagiaires/intérimaires. **Toujours renseigner** pour les non-CDI. |
| `info` / `description` | Notes libres (« Intérimaire via Agence X, fin prévue... »). |

```powershell
# Trouver les comptes dont l'expiration arrive dans 30 jours
$limit = (Get-Date).AddDays(30)
Get-ADUser -Filter { AccountExpirationDate -lt $limit -and Enabled -eq $true } `
  -Properties AccountExpirationDate, Department |
  Select-Object Name, SamAccountName, Department, AccountExpirationDate |
  Sort-Object AccountExpirationDate

# Exporter l'annuaire pour la RH (sans données sensibles)
Get-ADUser -Filter * -Properties Department, Title, Enabled |
  Select-Object Name, SamAccountName, Department, Title, Enabled |
  Export-Csv "C:\Rapports\annuaire.csv" -NoTypeInformation -Encoding UTF8
```

---

## 22. Désactiver, déplacer, supprimer des comptes : cycle de vie

**Procédure de départ** (à automatiser autant que possible) :

```powershell
$login = "a.diallo"

# 1. Désactiver immédiatement (coupe tous les accès)
Disable-ADAccount -Identity $login

# 2. Réinitialiser le mot de passe avec une valeur aléatoire
$random = ConvertTo-SecureString ([guid]::NewGuid().ToString()) -AsPlainText -Force
Set-ADAccountPassword -Identity $login -NewPassword $random -Reset

# 3. Déplacer vers l'OU des comptes désactivés (sort des GPO utilisateurs actives)
Move-ADObject -Identity (Get-ADUser $login).DistinguishedName `
  -TargetPath "OU=Desactives,OU=Utilisateurs,DC=ad,DC=entreprise,DC=fr"

# 4. Retirer des groupes (sauf "Domain Users")
Get-ADUser $login -Properties MemberOf | Select-Object -ExpandProperty MemberOf |
  ForEach-Object { Remove-ADGroupMember -Identity $_ -Members $login -Confirm:$false }

# 5. Masquer du carnet d'adresses si Exchange, noter la date
Set-ADUser $login -Description "Départ le $(Get-Date -Format 'yyyy-MM-dd')"
```

**Suppression** : attendez le délai légal/RH (souvent 30 à 90 jours), la corbeille AD (section 48) vous protège des suppressions accidentelles mais **ne remplace pas** une procédure.

```powershell
# Comptes inactifs depuis plus de 90 jours (candidats au nettoyage)
$date = (Get-Date).AddDays(-90)
Search-ADAccount -AccountInactive -TimeSpan 90 -UsersOnly |
  Where-Object { $_.Enabled } |
  Select-Object Name, SamAccountName, LastLogonDate
```

---

## 23. Groupes : portées (global, universel, local de domaine)

| Portée | Contenu possible | Visible/utilisable | Usage typique |
|---|---|---|---|
| **Global (G)** | Utilisateurs, ordinateurs, autres groupes globaux **du même domaine** | Tout le domaine (et forêts approuvées) | Regrouper des utilisateurs par métier/site |
| **Universel (U)** | Utilisateurs/groupes globaux de **toute la forêt** | Toute la forêt (via le catalogue global) | Regrouper des globaux de plusieurs domaines |
| **Local de domaine (DL)** | Tout (utilisateurs, globaux, universels, d'autres domaines) | **Domaine local uniquement** | Donner des droits sur des ressources du domaine |

**Moyen mnémotechnique** : **G**lobal = **G**ens (les utilisateurs), **D**L = **D**roits (sur les ressources).

```powershell
# Créer les trois types
New-ADGroup -Name "GG_Exploitation_Paris" -GroupScope Global -GroupCategory Security `
  -Path "OU=Groupes,DC=ad,DC=entreprise,DC=fr"
New-ADGroup -Name "GU_Exploitation" -GroupScope Universal -GroupCategory Security `
  -Path "OU=Groupes,DC=ad,DC=entreprise,DC=fr"
New-ADGroup -Name "DL_Partage_Atelier_RW" -GroupScope DomainLocal -GroupCategory Security `
  -Path "OU=Groupes,DC=ad,DC=entreprise,DC=fr"
```

**Groupes de sécurité vs distribution** : les groupes de **sécurité** ont un SID et servent aux droits ; les groupes de **distribution** servent aux listes de diffusion (messagerie) et n'ont pas de SID utilisable pour les ACL. En cas de doute : sécurité.

---

## 24. Imbrication de groupes : stratégie AGDLP/AGUDLP

**AGDLP** (mono-domaine) : **A**ccounts → **G**lobal → **D**omain **L**ocal → **P**ermissions.
**AGUDLP** (multi-domaines) : Accounts → Global → **U**niversal → Domain Local → Permissions.

```
Utilisateurs (a.diallo, k.benali)
   └── GG_Exploitation_Paris (Global : les gens)
          └── GU_Exploitation (Universel : fédère les sites)
                 └── DL_Partage_Atelier_RW (Local de domaine : les droits)
                        └── ACL du partage \\SRV\Atelier (Modify)
```

**Pourquoi** : quand Karim change de service, on le déplace d'un groupe global à un autre — **zéro modification des ACL**. Quand un nouveau partage ouvre, on crée un DL et on y imbrique les groupes existants.

```powershell
# Mettre en place la chaîne
Add-ADGroupMember -Identity "GG_Exploitation_Paris" -Members "a.diallo","k.benali"
Add-ADGroupMember -Identity "GU_Exploitation" -Members "GG_Exploitation_Paris"
Add-ADGroupMember -Identity "DL_Partage_Atelier_RW" -Members "GU_Exploitation"

# Vérifier l'appartenance récursive d'un utilisateur
Get-ADUser "a.diallo" -Properties MemberOf | Select-Object -ExpandProperty MemberOf
# Ou : tous les groupes effectifs (récursif)
(Get-ADUser "a.diallo" -Properties tokenGroups).tokenGroups |
  ForEach-Object { (New-Object System.Security.Principal.SecurityIdentifier $_).Translate([System.Security.Principal.NTAccount]) }
```

**Limites** : évitez plus de 3-4 niveaux d'imbrication (lisibilité, temps d'ouverture de session). Documentez la logique dans la description des groupes.

---

## 25. Groupes dynamiques : état des lieux et alternatives

⚠️ **AD DS natif ne propose pas de groupes dynamiques** (contrairement à Entra ID / Azure AD). Les appartenances sont statiques. Alternatives :

1. **Script planifié** : une tâche qui synchronise un groupe selon un attribut (ex. `Department=Exploitation` → membre de `GG_Exploitation`).

```powershell
# Exemple : synchroniser GG_Exploitation_Paris depuis l'attribut Department+Site
$group = "GG_Exploitation_Paris"
$attendus = Get-ADUser -Filter { Department -eq "Exploitation" } -Properties Department |
  Where-Object { $_.DistinguishedName -like "*OU=Paris*" } |
  Select-Object -ExpandProperty SamAccountName
$actuels = Get-ADGroupMember -Identity $group | Select-Object -ExpandProperty SamAccountName
# Ajouter les manquants
$attendus | Where-Object { $_ -notin $actuels } | ForEach-Object {
  Add-ADGroupMember -Identity $group -Members $_
}
# Retirer les intrus
$actuels | Where-Object { $_ -notin $attendus } | ForEach-Object {
  Remove-ADGroupMember -Identity $group -Members $_ -Confirm:$false
}
```

2. **Requêtes LDAP enregistrées** (Saved Queries) dans la console : affichent dynamiquement mais ne sont **pas** des groupes utilisables dans les ACL/GPO.
3. **Entra ID** : si synchronisé (Entra Connect), les groupes dynamiques Entra existent côté cloud uniquement.

---

## 26. Contacts, ordinateurs et comptes de service

**Contacts** : objets annuaire sans identifiant de sécurité (pas de SID), utilisés pour le carnet d'adresses (prestataires externes).

```powershell
New-ADObject -Name "Jean Prestataire" -Type contact `
  -Path "OU=Contacts,DC=ad,DC=entreprise,DC=fr" `
  -OtherAttributes @{ mail = "jean@prestataire.fr"; telephoneNumber = "0123456789" }
```

**Ordinateurs** : créés automatiquement à la jonction au domaine, ou pré-créés (pré-staging) pour contrôler l'OU de destination.

```powershell
# Pré-créer un compte d'ordinateur dans la bonne OU
New-ADComputer -Name "PC-ATELIER-042" `
  -Path "OU=Fixes,OU=Paris,OU=Postes,DC=ad,DC=entreprise,DC=fr" `
  -Description "Poste atelier - ligne 2"

# Rediriger le conteneur par défaut des nouveaux ordinateurs vers une OU gérée
redircmp "OU=Postes,DC=ad,DC=entreprise,DC=fr"
# Idem pour les utilisateurs (rarement utile, mais existe)
redirusr "OU=Utilisateurs,DC=ad,DC=entreprise,DC=fr"
```

> 💡 `redircmp` : indispensable. Sinon les PC jonchés tombent dans `CN=Computers`, hors de vos GPO/OU. À faire **une fois** par domaine.

**Comptes de service classiques** : bannissez les comptes « utilisateur » avec mot de passe qui n'expire jamais partagés entre applis. Préférez les **gMSA** (section suivante).

---

## 27. Comptes de service gérés (gMSA)

Les **gMSA** (group Managed Service Accounts) : mots de passe **gérés automatiquement** par AD (rotation toutes les 30 jours), utilisables par plusieurs serveurs, sans intervention humaine.

```powershell
# 1. Préparer la forêt (une seule fois) : créer la clé racine KDS
Add-KdsRootKey -EffectiveTime ((Get-Date).AddHours(-10))
# ⚠️ La clé n'est utilisable que 10 h après sa création (ou forcez une date passée en labo uniquement)

# 2. Créer le gMSA
New-ADServiceAccount -Name "gmsa_Sauvegarde" `
  -DNSHostName "gmsa_Sauvegarde.ad.entreprise.fr" `
  -PrincipalsAllowedToRetrieveManagedPassword "GG_Serveurs_Sauvegarde"

# 3. Sur chaque serveur autorisé : installer le compte
Install-ADServiceAccount -Identity "gmsa_Sauvegarde"

# 4. L'utiliser dans un service ou une tâche planifiée
# Service : compte "AD.ENTREPRISE.FR\gmsa_Sauvegarde$" (mot de passe vide)
# Tâche planifiée :
$action = New-ScheduledTaskAction -Execute "C:\Scripts\backup.ps1"
Register-ScheduledTask -TaskName "Sauvegarde quotidienne" -Action $action `
  -User "ad.entreprise.fr\gmsa_Sauvegarde$" -RunLevel Highest
```

**Cas d'usage** : services Windows, tâches planifiées, pools IIS, SQL Server. **Limites** : ne fonctionne pas pour les services interactifs nécessitant un profil complet ni hors du domaine.

---

## 28. Les 5 rôles FSMO : vue d'ensemble

Certaines opérations ne supportent pas le multi-maître : 5 rôles **FSMO** (Flexible Single Master Operations) sont attribués à des DC désignés.

| # | Rôle | Portée | Sensible à la panne ? |
|---|---|---|---|
| 1 | Maître de **schéma** | Forêt | Non (sauf modif du schéma en cours) |
| 2 | Maître d'attribution des **noms de domaine** | Forêt | Non (sauf ajout/suppression de domaine) |
| 3 | Maître **RID** | Domaine | Oui à moyen terme (épuisement des pools RID) |
| 4 | **Émulateur PDC** | Domaine | **Oui** (heure, verrouillages, GPO, mots de passe) |
| 5 | Maître d'**infrastructure** | Domaine | Peu (sauf multi-domaines) |

```powershell
# Voir tous les détenteurs FSMO de la forêt
Get-ADForest | Select-Object SchemaMaster, DomainNamingMaster
Get-ADDomain | Select-Object RIDMaster, PDCEmulator, InfrastructureMaster

# Ou en une ligne :
netdom query fsmo
```

---

## 29. Maître de schéma (Schema Master)

**Rôle** : seul DC autorisé à **modifier le schéma** (ajouter des classes/attributs). Portée forêt, un seul détenteur.

**Quand on y touche** : installation d'Exchange, de SCCM, d'applications étendant le schéma, `adprep /forestprep` lors d'une montée de version.

```powershell
# Voir le détenteur
(Get-ADForest).SchemaMaster

# Autoriser les mises à jour du schéma sur ce DC (verrou de sécurité)
# Registre : HKLM\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Schema Update Allowed = 1
```

**Bonnes pratiques** :

- Ne transférez ce rôle que si nécessaire ; il peut rester sur le premier DC.
- **Sauvegardez l'état système** avant toute extension de schéma (irréversible : on peut désactiver un attribut, jamais le supprimer vraiment).
- Testez l'extension en labo.

---

## 30. Maître d'attribution des noms de domaine (Domain Naming Master)

**Rôle** : autorise l'**ajout et la suppression de domaines** dans la forêt. Vérifie l'unicité des noms.

```powershell
(Get-ADForest).DomainNamingMaster
```

Peu sollicité au quotidien. Comme le maître de schéma, il vit généralement sur le premier DC de la forêt et on l'y laisse.

---

## 31. Maître RID (Relative ID Master)

**Rôle** : distribue des **pools de RID** aux DC. Chaque objet créé consomme un RID (le SID = SID du domaine + RID). Quand un DC épuise son pool (500 par défaut), il en redemande au maître RID.

**Danger** : si le maître RID est indisponible longtemps et que les pools s'épuisent, **impossible de créer** utilisateurs/ordinateurs/groupes.

```powershell
(Get-ADDomain).RIDMaster

# Vérifier la consommation du pool RID sur un DC
dcdiag /test:ridmanager /v
# Événement 16650 dans le journal Directory Service = pool épuisé : urgence
```

**Bonnes pratiques** : placez-le sur un DC **bien répliqué et disponible** ; surveillez l'événement 16650 ; ne le saisissez (seize) qu'en dernier recours car un ancien maître RID qui reviendrait en ligne pourrait distribuer des pools en double → **SID en double** (catastrophe).

---

## 32. Émulateur PDC (PDC Emulator)

Le rôle le plus **critique** au quotidien :

- **Référence de temps** du domaine (hiérarchie NTP : PDC de la forêt → DC → clients).
- **Verrouillages de compte** : c'est lui qui comptabilise les échecs et verrouille.
- **Changements de mot de passe urgents** : répliqués en priorité vers lui.
- **Édition des GPO** : l'éditeur (GPMC) cible le PDC par défaut pour éviter les conflits.
- **Scripts/objets** sensibles au « dernier écrivain ».

```powershell
(Get-ADDomain).PDCEmulator

# Configurer la source de temps du PDC (ex. serveurs NTP externes)
w32tm /config /manualpeerlist:"0.fr.pool.ntp.org 1.fr.pool.ntp.org" /syncfromflags:manual /reliable:yes /update
Restart-Service w32time
w32tm /query /status
w32tm /query /peers

# Sur les autres DC : synchronisation depuis la hiérarchie du domaine (par défaut)
w32tm /config /syncfromflags:domhier /update
```

**Bonnes pratiques** : PDC sur le DC le plus **robuste et central** (souvent le premier DC du site principal) ; **ne le placez jamais sur un RODC** (impossible de toute façon) ; surveillez-le en priorité ; prévoyez son transfert rapide en cas de maintenance.

---

## 33. Maître d'infrastructure (Infrastructure Master)

**Rôle** : maintient les références inter-domaines (quand un objet d'un autre domaine est membre d'un groupe local, il met à jour le DN fantôme/phantom).

**Règle de placement** (multi-domaines) : **ne le mettez PAS sur un DC qui est aussi catalogue global**, sauf si :

- tous les DC du domaine sont des GC, ou
- il n'y a qu'un seul domaine dans la forêt (cas le plus courant : la règle ne s'applique pas).

```powershell
(Get-ADDomain).InfrastructureMaster
```

En domaine unique, oubliez cette contrainte : mettez-le où vous voulez.

---

## 34. Transférer un rôle FSMO (Move-ADDirectoryServerOperationMasterRole)

Le **transfert** est l'opération **propre** : l'ancien détenteur est en ligne et consent.

```powershell
# Transférer les 5 rôles vers DC02 (noms ou numéros 0-4)
Move-ADDirectoryServerOperationMasterRole -Identity "DC02" `
  -OperationMasterRole SchemaMaster, DomainNamingMaster, RIDMaster, PDCEmulator, InfrastructureMaster `
  -Force

# Avec les numéros (0=Schéma, 1=Nommage, 2=RID, 3=PDC, 4=Infrastructure)
Move-ADDirectoryServerOperationMasterRole -Identity "DC02" -OperationMasterRole 0,1,2,3,4 -Force

# Transférer un seul rôle (ex. PDC avant maintenance du DC01)
Move-ADDirectoryServerOperationMasterRole -Identity "DC02" -OperationMasterRole PDCEmulator -Force
```

**Quand transférer** : maintenance planifiée du détenteur, rééquilibrage, décommission d'un DC (obligatoire avant rétrogradation si le DC détient des rôles — sinon la rétrogradation les transfère automatiquement, mais mieux vaut le faire explicitement avant).

---

## 35. Saisir un rôle FSMO (seize via ntdsutil) — procédure d'urgence

La **saisie** (seize) s'utilise quand le détenteur est **mort et ne reviendra jamais**. C'est une opération **destructrice** : l'ancien détenteur ne doit **jamais** être reconnecté au réseau (risque de RID en double, de schéma divergent).

```powershell
# Procédure ntdsutil (le seul moyen de "seize" ; la cmdlet PowerShell ne fait que des transferts)
ntdsutil
roles
connections
connect to server DC02
quit
seize schema master
seize naming master
seize rid master
seize pdc
seize infrastructure master
quit
quit
```

**Après une saisie** :

1. Vérifiez : `netdom query fsmo`.
2. **Ne rallumez jamais** l'ancien DC sur le réseau. Réinstallez-le from scratch s'il doit revenir.
3. Nettoyez ses métadonnées (section 12).
4. Surveillez la réplication pendant 48 h.

> ⚠️ Erreur classique : saisir un rôle alors que le détenteur est juste **temporairement injoignable** (panne réseau). Résultat : deux maîtres potentiels → corruption. La saisie = détenteur **définitivement perdu**.

---

## 36. Bonnes pratiques de placement des rôles FSMO

**Petite structure (1-2 DC, un domaine)** : laissez les 5 rôles sur le premier DC. Simplicité > micro-optimisation.

**Structure moyenne (3+ DC, un site)** :

| Rôle | Placement recommandé |
|---|---|
| Schéma + Nommage (forêt) | DC principal du site central |
| RID + PDC + Infrastructure (domaine) | DC principal (souvent le même) |

**Multi-sites** : les 5 rôles sur un DC du site central (le mieux connecté), **jamais** sur un RODC ni sur un DC de site distant fragile.

**Règles** :

- Documentez quel DC détient quoi (dans votre wiki + étiquette physique/virtuelle).
- Avant toute maintenance du détenteur : transférez le PDC a minima.
- Ne placez jamais un rôle FSMO sur un DC que vous prévoyez de décommissionner.
- Surveillez avec un script quotidien (section 73).

---

## 37. Sites et services : concepts

Un **site AD** représente un **emplacement réseau bien connecté** (LAN ou liaisons rapides), pas forcément un site géographique. AD s'en sert pour :

- **Optimiser la réplication** : réplication fréquente intra-site (toutes les ~15 s après notification), planifiée inter-sites (défaut 180 min).
- **Diriger les clients** vers le DC le plus proche (via les enregistrements DNS `_ldap._tcp.<Site>._sites`).
- **Cibler les GPO** et DFS.

**Vocabulaire** :

| Terme | Définition |
|---|---|
| **Site** | Regroupement de sous-réseaux bien connectés. |
| **Sous-réseau** | Objet `10.10.20.0/24` associé à un site. **Tout sous-réseau client doit être déclaré**, sinon les clients tombent dans `Default-First-Site-Name`. |
| **Liaison de site (Site Link)** | Chemin logique entre sites : coût, fréquence, planification. |
| **Bridgehead (tête de pont)** | DC désigné pour la réplication inter-sites (choisi par le KCC, ou manuel). |
| **ISTG** | Serveur générateur de topologie inter-sites (un par site, rôle KCC). |

---

## 38. Créer et configurer des sites, sous-réseaux

```powershell
# Créer les sites
New-ADReplicationSite -Name "Site-Paris"
New-ADReplicationSite -Name "Site-Lyon"

# Déclarer les sous-réseaux (TOUS les réseaux : clients, serveurs, DMZ, Wi-Fi)
New-ADReplicationSubnet -Name "10.10.10.0/24" -Site "Site-Paris" -Description "LAN Paris"
New-ADReplicationSubnet -Name "10.10.20.0/24" -Site "Site-Lyon"  -Description "LAN Lyon"
New-ADReplicationSubnet -Name "10.10.30.0/24" -Site "Site-Paris" -Description "Wi-Fi Paris"
New-ADReplicationSubnet -Name "192.168.99.0/24" -Site "Site-Paris" -Description "DMZ"

# Déplacer un serveur (DC) dans son site : via la console
# "Sites et services AD" > Servers > clic droit > Déplacer
# Ou en PowerShell (objet serveur) :
Get-ADDomainController "DC-Lyon" | Move-ADObject -TargetPath "CN=Servers,CN=Site-Lyon,CN=Sites,CN=Configuration,DC=ad,DC=entreprise,DC=fr"

# Renommer le site par défaut si vous ne l'utilisez plus
Rename-ADObject -Identity "CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=ad,DC=entreprise,DC=fr" -NewName "Site-Paris"
```

**Vérification côté client** : `nltest /dsgetsite` doit retourner le bon site. Sinon : sous-réseau manquant ou mal déclaré.

---

## 39. Liaisons de sites : coût, planification, bridgehead

```powershell
# Créer une liaison de site
New-ADReplicationSiteLink -Name "Lien-Paris-Lyon" `
  -SitesIncluded "Site-Paris","Site-Lyon" `
  -Cost 100 -ReplicationFrequencyInMinutes 60

# Modifier le coût (plus le coût est bas, plus la liaison est préférée)
Set-ADReplicationSiteLink -Identity "Lien-Paris-Lyon" -Replace @{ cost = 50 }

# Planification : répliquer uniquement la nuit (ex. 20h-6h)
# Via la console : propriétés de la liaison > Modifier la planification
# En PowerShell, le schedule est un tableau d'octets ; préférez la console pour ce réglage fin

# Désigner manuellement un bridgehead (déconseillé sauf besoin précis)
# Sites et services > serveur > Propriétés > cocher le transport IP comme "serveur tête de pont préféré"
```

**Coûts** : reflétez la **qualité des liaisons** (bande passante, fiabilité). Le KCC calcule les chemins les moins coûteux, y compris via des sites intermédiaires (pontage de liaisons de sites, activé par défaut : « Bridge all site links »).

**Fréquence** : 15 min minimum via la console (valeur basse = 15). En dessous, passez par la notification inter-sites (possible mais rarement nécessaire).

---

## 40. Réplication AD : KCC et topologie

Le **KCC** (Knowledge Consistency Checker) tourne sur chaque DC (toutes les 15 min) et construit automatiquement la topologie de réplication :

- **Intra-site** : anneau bidirectionnel + connexions supplémentaires (chaque DC a au moins 2 partenaires).
- **Inter-sites** : via les bridgeheads, selon les liaisons de sites.

**Types de réplication** :

| Partition | Mode |
|---|---|
| Domaine, Configuration, Schéma | Multi-maître (tout DC inscriptible accepte les écritures) |
| SYSVOL (stratégies) | DFSR (depuis 2008 ; FRS est mort, ne l'utilisez plus) |

```powershell
# Forcer le KCC à recalculer la topologie
repadmin /kcc

# Voir les partenaires de réplication entrante
repadmin /showrepl DC01

# Topologie inter-sites
repadmin /bridgeheads
```

**À retenir** : dans 99 % des cas, **ne touchez pas** aux objets de connexion créés par le KCC (`<automatically generated>`). Créez des connexions manuelles uniquement pour des besoins temporaires (dépannage) et supprimez-les après.

---

## 41. Surveiller la réplication : repadmin

`repadmin` est l'outil n°1 du dépannage réplication.

```powershell
# Résumé de l'état de réplication de tous les DC (le premier à lancer)
repadmin /replsummary

# Détail par DC : derniers succès/échecs, USN
repadmin /showrepl
repadmin /showrepl DC02

# File d'attente de réplication
repadmin /queue

# Forcer la réplication d'une partition vers tous les partenaires
repadmin /syncall DC01 /AdeP
# /A : toutes les partitions, /d : noms distinctifs, /e : tous les sites, /P : push

# Vérifier l'état du SYSVOL/DFSR
dfsrdiag pollad
Get-DfsrState  # (module DFSR)

# Connaître l'USN le plus élevé et les "high watermarks"
repadmin /showutdvec DC01 "DC=ad,DC=entreprise,DC=fr"
```

**Lecture de `/replsummary`** : la colonne des échecs doit être à **0** partout. Un échec isolé et récent peut être transitoire ; un échec qui grandit = investigation immédiate (réseau, DNS, heure, espace disque).

---

## 42. Diagnostiquer avec dcdiag

```powershell
# Diagnostic complet du DC local
dcdiag

# Diagnostic verbeux, tous les tests
dcdiag /v

# Tester tous les DC du site / de l'entreprise
dcdiag /s:DC02
dcdiag /a          # tous les DC du site
dcdiag /e          # tous les DC de l'entreprise

# Tests ciblés utiles
dcdiag /test:dns /v          # santé DNS (le plus critique)
dcdiag /test:replications
dcdiag /test:netlogons
dcdiag /test:advertising     # le DC s'annonce-t-il correctement ?
dcdiag /test:fsmocheck
dcdiag /test:ridmanager
```

**Tests à surveiller en priorité** : `DNS`, `Replications`, `NetLogons`, `Advertising`, `FSMOCheck`, `RidManager`, `Services`, `SystemLog`. Un `dcdiag` propre sur tous les DC = 80 % de la santé AD.

> 💡 Lancez `dcdiag /e /v` **avant** toute opération sensible (promotion, transfert FSMO, décommission) : c'est votre photo « avant travaux ».

---

## 43. Dépannage réplication : USN rollback

**USN rollback** : le cauchemar des DC virtualisés. Chaque DC maintient un **USN** (Update Sequence Number) qui s'incrémente à chaque écriture. Les partenaires retiennent le dernier USN vu (`high watermark`).

**Scénario** : on restaure un snapshot d'un DC vieux de 3 jours. Son USN **recule**. Ses partenaires croient déjà avoir tout jusqu'à l'USN 1000, le DC restauré est à 800 : les écritures 801-1000 (mots de passe changés, comptes créés) **ne seront jamais répliquées** → divergence silencieuse, comptes verrouillés mystérieusement, etc.

**Symptômes** : événement **2108/1084** (NTDS Replication), `repadmin /showrepl` avec des USN incohérents.

**Prévention** :

1. Ne restaurez **jamais** un DC par snapshot hyperviseur. Utilisez la sauvegarde d'état système.
2. Activez la protection **VM-GenerationID** (hyperviseur récent + OS 2012+) : en cas de restore, l'ID change, le DC s'en rend compte et se met en sécurité (il demande une réplication complète / se désactive proprement avec l'événement 2162).
3. Un seul DC physique ou un DC hors snapshots critiques dans les petits parcs.

**Remède** : le DC en USN rollback doit être **rétrogradé de force, nettoyé (metadata cleanup) et repromu**. Il n'y a pas de « réparation » fiable.

---

## 44. Dépannage réplication : lingering objects (objets rémanents)

**Lingering object** : un objet supprimé sur les autres DC mais qui **réapparaît** sur un DC resté trop longtemps déconnecté (au-delà de la **tombstone lifetime**, 180 jours par défaut). Quand il se reconnecte, il réinjecte l'objet fantôme.

**Symptômes** : événement **1388** (NTDS Replication) : « le DC a reçu une mise à jour pour un objet supprimé » ; `repadmin /showrepl` en erreur.

**Traitement** :

```powershell
# 1. Identifier les objets rémanents (comparer deux DC)
repadmin /removelingeringobjects DC01 <GUID_DC_Reference> "DC=ad,DC=entreprise,DC=fr" /advisory_mode
# /advisory_mode : journalise sans supprimer (toujours commencer par là)

# 2. Suppression réelle (après analyse du journal d'événements 1937)
repadmin /removelingeringobjects DC01 <GUID_DC_Reference> "DC=ad,DC=entreprise,DC=fr"

# 3. Durcir : activer la protection stricte contre la réplication des objets rémanents
# Registre sur chaque DC :
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\NTDS\Parameters" `
  -Name "Strict Replication Consistency" -Value 1 -Type DWord
```

**Prévention** : ne laissez jamais un DC déconnecté plus de 180 jours ; surveillez `repadmin /replsummary` (un DC silencieux depuis des semaines = alerte).

---

## 45. Stratégie de mot de passe par défaut du domaine

La **stratégie de mot de passe du domaine** se définit dans la **Default Domain Policy** (liée à la racine du domaine) : elle s'applique à **tous** les utilisateurs du domaine.

Chemin GPO : `Configuration ordinateur > Stratégies > Paramètres Windows > Paramètres de sécurité > Stratégies de comptes > Stratégie de mot de passe`.

| Paramètre | Défaut | Recommandation entreprise |
|---|---|---|
| Longueur minimale | 7 | **12-14** (16 pour les admins) |
| Complexité | Activée | Activée (ou passphrases longues) |
| Durée de vie maximale | 42 jours | 90-180 j utilisateurs ; **ne pas expirer** les comptes de service gMSA (géré par AD) ; admins : 60-90 j + MFA |
| Durée de vie minimale | 1 jour | 1 jour (anti-contournement du changement immédiat) |
| Historique | 24 | 24 |

**Verrouillage de compte** (`Stratégies de comptes > Stratégie de verrouillage`) :

| Paramètre | Recommandation |
|---|---|
| Seuil de verrouillage | 5 tentatives |
| Durée du verrouillage | 30 min (ou jusqu'au déverrouillage admin) |
| Compteur réinitialisé après | 30 min |

```powershell
# Lire la stratégie effective du domaine
Get-ADDefaultDomainPasswordPolicy | Format-List *

# Déverrouiller un compte
Unlock-ADAccount -Identity "a.diallo"

# Comptes verrouillés en ce moment
Search-ADAccount -LockedOut | Select-Object Name, SamAccountName
```

> ⚠️ La stratégie définie dans une GPO liée à une **OU** ne s'applique **pas** aux mots de passe (sauf FGPP, section suivante). Seule la Default Domain Policy (ou une GPO liée à la racine avec précédence) compte.

---

## 46. FGPP : stratégies de mot de passe affinées (PSO)

Les **FGPP** (Fine-Grained Password Policies) permettent des politiques **différentes par groupe** via des objets **PSO** (Password Settings Object). Ordre de précédence : PSO lié directement à l'utilisateur > PSO du groupe (le plus petit `msDS-PasswordSettingsPrecedence` gagne).

```powershell
# Créer un PSO strict pour les administrateurs
New-ADFineGrainedPasswordPolicy -Name "PSO-Admins" `
  -Precedence 10 `
  -MinPasswordLength 16 `
  -PasswordHistoryCount 24 `
  -ComplexityEnabled $true `
  -ReversibleEncryptionEnabled $false `
  -MinPasswordAge "1.00:00:00" `
  -MaxPasswordAge "60.00:00:00" `
  -LockoutThreshold 3 `
  -LockoutObservationWindow "00:30:00" `
  -LockoutDuration "00:30:00"

# L'appliquer au groupe des admins du domaine
Add-ADFineGrainedPasswordPolicySubject -Identity "PSO-Admins" -Subjects "Admins du domaine"

# Créer un PSO assoupli pour les comptes de service legacy (exemple)
New-ADFineGrainedPasswordPolicy -Name "PSO-Services" -Precedence 50 `
  -MinPasswordLength 20 -PasswordHistoryCount 24 -ComplexityEnabled $true `
  -ReversibleEncryptionEnabled $false -MinPasswordAge "1.00:00:00" `
  -MaxPasswordAge "180.00:00:00" -LockoutThreshold 0

# Voir le PSO effectif d'un utilisateur
Get-ADUserResultantPasswordPolicy -Identity "a.diallo" | Select-Object Name, Precedence
```

**Bonnes pratiques** : un PSO « Admins » strict, un PSO « Utilisateurs » standard (= défaut du domaine), precedence basse = prioritaire. Documentez les PSO comme des GPO.

---

## 47. Bonnes pratiques mots de passe et verrouillage

1. **Longueur > complexité** : une passphrase de 16 caractères (`correct-cheval-batterie-agrafes`) bat `P@ssw0rd!` en sécurité et en mémorisation.
2. **Ne forcez pas** les rotations trop fréquentes (42 j par défaut pousse aux `Motdepasse1`, `Motdepasse2`...). 90-180 j + surveillance des compromissions.
3. **Bannissez les mots de passe connus** : comparez les hachés aux listes de fuites (HaveIBeenPwned, outils type Lithnet Password Protection).
4. **MFA partout** où c'est possible, surtout admins et accès distants.
5. **Comptes de service** : gMSA (section 27), jamais de compte utilisateur avec « le mot de passe n'expire jamais ».
6. **Verrouillage** : 5 essais / 30 min est un bon équilibre. Surveillez les verrouillages répétés (attaque par dictionnaire ?).
7. **Communication** : à l'embauche, mot de passe initial remis en main propre ou via canal séparé, changement imposé à la première ouverture.

```powershell
# Rapport : comptes avec "mot de passe n'expire jamais" (à réduire au minimum)
Get-ADUser -Filter { PasswordNeverExpires -eq $true -and Enabled -eq $true } `
  -Properties PasswordNeverExpires |
  Select-Object Name, SamAccountName, DistinguishedName

# Rapport : mots de passe vieux de plus de 180 jours
$seuil = (Get-Date).AddDays(-180)
Get-ADUser -Filter { PasswordLastSet -lt $seuil -and Enabled -eq $true } `
  -Properties PasswordLastSet |
  Select-Object Name, SamAccountName, PasswordLastSet | Sort-Object PasswordLastSet
```

---

## 48. Corbeille AD : activation et fonctionnement

La **corbeille AD** (AD Recycle Bin) permet de **restaurer des objets supprimés avec tous leurs attributs** (y compris les appartenances aux groupes). Sans elle, la restauration ne récupère qu'une coquille (tombstone).

**Prérequis** : niveau fonctionnel de forêt **2008 R2 minimum** (donc OK en 2019/2022/2025). **Activation irréversible**.

```powershell
# Activer la corbeille (cible : nom de la forêt)
Enable-ADOptionalFeature -Identity "Recycle Bin Feature" `
  -Scope ForestOrConfigurationSet -Target "ad.entreprise.fr" -Confirm:$false

# Vérifier
Get-ADOptionalFeature -Filter { Name -eq "Recycle Bin Feature" } |
  Select-Object Name, @{N="Activée";E={$_.EnabledScopes}}
```

**Cycle de vie d'un objet supprimé** (avec corbeille) : objet supprimé → état **deleted** (restaurable, 180 jours par défaut = `msDS-deletedObjectLifetime`) → objet **recyclé** → destruction physique.

---

## 49. Restaurer un objet supprimé (Restore-ADObject)

```powershell
# 1. Trouver l'objet supprimé
Get-ADObject -Filter { Name -like "*Diallo*" } -IncludeDeletedObjects |
  Select-Object Name, ObjectClass, Deleted, LastKnownParent

# 2. Restaurer à son emplacement d'origine
Get-ADObject -Filter { SamAccountName -eq "a.diallo" } -IncludeDeletedObjects |
  Restore-ADObject

# 3. Restaurer vers une OU différente (plus sûr : on contrôle)
Get-ADObject -Filter { SamAccountName -eq "a.diallo" } -IncludeDeletedObjects |
  Restore-ADObject -NewName "Amina Diallo" `
    -TargetPath "OU=Restaures,OU=Utilisateurs,DC=ad,DC=entreprise,DC=fr"

# 4. Restaurer une OU entière avec son contenu
Get-ADObject -Filter { Name -eq "Exploitation" -and ObjectClass -eq "organizationalUnit" } `
  -IncludeDeletedObjects | Restore-ADObject
# Puis restaurer les objets enfants un par un (la restauration est hiérarchique : parent d'abord)
```

**Après restauration** : réactivez le compte (`Enable-ADAccount`), vérifiez les appartenances aux groupes (restaurées avec la corbeille, **pas** sans), réinitialisez le mot de passe (par sécurité).

**Sans corbeille** (tombstone classique) : restauration d'autorité via `ntdsutil authoritative restore` (section 53) ou recréation manuelle — les appartenances aux groupes sont **perdues**.

---

## 50. Sauvegarde : état système avec wbadmin

On ne sauvegarde pas `ntds.dit` à la main : on fait une **sauvegarde de l'état système** (System State) qui inclut AD, SYSVOL, registre, services.

```powershell
# Sauvegarde de l'état système vers un disque dédié (jamais sur le disque système)
wbadmin start systemstatebackup -backupTarget:E: -quiet

# Planifier une sauvegarde quotidienne à 22h
wbadmin enable backup -addtarget:E: -systemstate -schedule:22:00 -quiet

# Vérifier les sauvegardes disponibles
wbadmin get versions

# Sauvegarde bare-metal complète (état système + volumes critiques) — recommandée
wbadmin start backup -backupTarget:E: -allCritical -systemstate -quiet
```

**Bonnes pratiques** :

- Sauvegardez **au moins un DC par domaine** chaque jour (idéalement deux DC différents).
- Conservez plusieurs générations (rotation 7-14 jours).
- **Testez la restauration** en labo isolé au moins une fois par semestre (une sauvegarde non testée = pas de sauvegarde).
- Stockez une copie **hors site** (copie du dossier WindowsImageBackup).
- Surveillez le succès via le journal `Microsoft-Windows-Backup` et une alerte.

---

## 51. Sauvegarder via ntdsutil et bonnes pratiques de sauvegarde

`ntdsutil` permet aussi de créer un snapshot applicatif (utile pour monter une copie et comparer, pas comme sauvegarde principale) :

```powershell
# Créer un snapshot de la base (montable en lecture seule pour inspection)
ntdsutil "activate instance ntds" "snapshot" "create" quit quit
# Lister / monter / démonter
ntdsutil "activate instance ntds" "snapshot" "list all" quit quit
```

**Ce qu'il faut sauvegarder, en résumé** :

| Élément | Moyen | Fréquence |
|---|---|---|
| État système des DC | wbadmin | Quotidien |
| GPO (sauvegardes GPMC) | `Backup-GPO -All` | Hebdo + avant chaque changement |
| Documentation (FSMO, topologie, mots de passe DSRM) | Coffre sécurisé | À chaque changement |
| Scripts de provisionning | Dépôt versionné | Continu |

```powershell
# Sauvegarder toutes les GPO (complément indispensable)
Backup-GPO -All -Path "E:\Sauvegardes\GPO" -Comment "Sauvegarde hebdo $(Get-Date -Format 'yyyy-MM-dd')"
```

---

## 52. DSRM : mode de restauration des services d'annuaire

Le **DSRM** (Directory Services Restore Mode) est le mode sans échec spécial des DC pour restaurer la base AD. Il utilise un **mot de passe local propre au DC**, défini à la promotion.

```powershell
# Changer le mot de passe DSRM d'un DC (à faire régulièrement / à chaque changement d'admin)
ntdsutil "set dsrm password" "reset password on server DC01" quit quit
# Ou via PowerShell (2016+) :
Set-DSRMPassword -NewPassword (Read-Host "Nouveau mot de passe DSRM" -AsSecureString)
```

**Bonnes pratiques** :

- Stockez les mots de passe DSRM dans le **coffre à mots de passe** d'entreprise (pas dans un fichier texte !).
- Un mot de passe DSRM **différent par DC** (ou au minimum documenté).
- Pour démarrer en DSRM : `msconfig` > Démarrer > options de démarrage, ou `bcdedit /set safeboot dsrepair`, ou F8 au boot.
- En DSRM, le DC ne fait **pas** autorité : parfait pour restaurer l'état système avant de choisir le type de restauration (section 53).

---

## 53. Restauration non faisant autorité vs faisant autorité

**Restauration non faisant autorité** (cas standard : un DC est mort, on le réinstalle) : on restaure l'état système, puis le DC **se resynchronise** avec ses partenaires (les données les plus récentes gagnent). Aucune manipulation supplémentaire.

**Restauration faisant autorité** (on veut **réimposer** des objets supprimés par erreur à toute la forêt) :

```powershell
# 1. Démarrer en DSRM, restaurer l'état système :
wbadmin start recovery -version:10/15/2026-22:00 -itemType:systemstate -quiet
# (en pratique : wbadmin get versions pour choisir la version)

# 2. Marquer les objets comme "faisant autorité" AVANT de redémarrer normalement
ntdsutil "activate instance ntds" "authoritative restore" `
  "restore object OU=Exploitation,OU=Paris,OU=Utilisateurs,DC=ad,DC=entreprise,DC=fr" `
  quit quit
# Pour toute la base (nucléaire : à éviter sauf catastrophe) :
# ntdsutil "activate instance ntds" "authoritative restore" "restore database" quit quit

# 3. Redémarrer en mode normal : les objets restaurés se répliquent en écrasant les suppressions
```

**Arbre de décision** :

- Objet(s) supprimé(s) par erreur, corbeille AD activée → `Restore-ADObject` (section 49). **C'est 99 % des cas.**
- DC mort → réinstallation + restauration non faisant autorité (ou simplement promouvoir un nouveau DC).
- Suppression massive répliquée partout, pas de corbeille → restauration faisant autorité depuis une sauvegarde antérieure à la suppression.

---

## 54. Migration inter-forêts avec ADMT

**ADMT** (Active Directory Migration Tool) migre utilisateurs, groupes, ordinateurs et profils d'une forêt source vers une forêt cible. Scénarios : fusion d'entreprises, rachat, refonte de forêt.

**Prérequis** :

1. Approbation de forêt bidirectionnelle (section 4).
2. DNS : chaque forêt résout l'autre.
3. ADMT installé sur un serveur de la forêt **cible** (+ SQL Server Express pour sa base).
4. Clé de migration des mots de passe (`admt key`) si on migre les mots de passe (nécessite un DC source 2016+ et la DLL Password Export Server).

**Étapes** :

```powershell
# 1. Sur la source : autoriser la migration (une fois)
#    Registre : HKLM\SYSTEM\CurrentControlSet\Control\Lsa\TcpipClientSupport = 1 (DWORD)
#    + redémarrage du DC source (désactive le filtrage SID pour la migration)

# 2. Migrer les comptes de service et groupes globaux d'abord, puis les utilisateurs
#    (via l'assistant ADMT : "User Account Migration Wizard")

# 3. Migrer les postes (avec traduction des profils locaux)
#    L'agent ADMT se déploie ; le poste redémarre et rejoint le nouveau domaine

# 4. Conserver le SID historique (SID History) pour garder l'accès aux ressources source
#    Option "Migrate user SIDs to target domain" dans l'assistant
```

**Ordre de migration** : groupes globaux → utilisateurs → postes → ressources. **Ne migrez jamais un DC** avec ADMT (on promeut des DC natifs dans la cible).

⚠️ ADMT n'est plus mis à jour par Microsoft depuis longtemps ; pour les migrations modernes, évaluez aussi les outils tiers (Quest, Binary Tree) selon la complexité.

---

## 55. Mise à niveau du niveau fonctionnel (forêt et domaine)

Le **niveau fonctionnel** détermine les fonctionnalités AD disponibles. Tous les DC doivent être au minimum à ce niveau d'OS.

| Niveau | DC minimum requis | Notes |
|---|---|---|
| 2012 R2 | 2012 R2 | Corbeille, etc. |
| 2016 (`WinThreshold`) | 2016 | Dernier niveau « classique » |
| 2025 (`Win2025`) ⚠️ | 2022+ (2025 pour certaines fonctions) | Nouveau avec Server 2025 : pages 32K dans ntds.dit, NUMA, etc. |

```powershell
# Voir les niveaux actuels
Get-ADForest | Select-Object ForestMode
Get-ADDomain | Select-Object DomainMode

# Monter le niveau du domaine (irréversible !)
Set-ADDomainMode -Identity "ad.entreprise.fr" -DomainMode "WinThreshold"

# Monter le niveau de la forêt (après tous les domaines)
Set-ADForestMode -Identity "ad.entreprise.fr" -ForestMode "WinThreshold"
```

**Procédure de montée de version des DC** (ex. 2019 → 2022) :

1. `dcdiag /e`, `repadmin /replsummary` : santé parfaite.
2. Sauvegarde état système.
3. Promouvoir un nouveau DC sous le nouvel OS, transférer les FSMO, vérifier, rétrograder l'ancien.
4. Ne monter le niveau fonctionnel qu'une fois **tous** les DC au nouvel OS (irréversible).

---

## 56. GPO : principes fondamentaux

Une **GPO** (Group Policy Object) = un ensemble de paramètres appliqués aux utilisateurs et/ou ordinateurs. Stockage double :

- **Conteneur de stratégie (GPC)** dans AD (`CN=Policies,CN=System,...`) : métadonnées, version.
- **Modèle de stratégie (GPT)** dans SYSVOL (`\\domaine\SYSVOL\domaine\Policies\{GUID}`) : les fichiers réels (Registry.pol, scripts...).

**Deux moitiés** : **Configuration ordinateur** (appliquée au démarrage, contexte SYSTEM) et **Configuration utilisateur** (appliquée à l'ouverture de session).

**Bonnes pratiques structurelles** :

- **Une GPO = un objectif** (« Durcissement navigateurs », « Imprimantes atelier »). Jamais de GPO « fourre-tout ».
- Nommez explicitement : `ORDI - Fond d'écran entreprise`, `USER - Mappage lecteurs`, avec préfixe du périmètre.
- **Désactivez la moitié inutilisée** (ex. une GPO 100 % « configuration ordinateur » → désactiver la config utilisateur) : accélère le traitement.
- Documentez chaque GPO dans son commentaire (GPMC > Détails > Commentaire).

```powershell
# Lister les GPO du domaine
Get-GPO -All | Select-Object DisplayName, GpoStatus, ModificationTime |
  Sort-Object DisplayName

# Créer une GPO et la lier à une OU
New-GPO -Name "ORDI - Verrouillage session 10 min" -Comment "Sécurité : verrouillage automatique"
New-GPLink -Name "ORDI - Verrouillage session 10 min" `
  -Target "OU=Fixes,OU=Paris,OU=Postes,DC=ad,DC=entreprise,DC=fr"
```

---

## 57. Ordre d'application : LSDOU

Les GPO s'appliquent dans l'ordre **LSDOU**, chaque niveau **écrasant** le précédent en cas de conflit :

1. **L**ocal (stratégie locale du poste, `gpedit.msc`)
2. **S**ite (GPO liée au site AD)
3. **D**omaine (GPO liée au domaine, ex. Default Domain Policy)
4. **OU** (de la racine vers l'OU la plus proche de l'objet — la plus proche gagne)

```
Site-Paris (GPO site)
└── Domaine ad.entreprise.fr (Default Domain Policy, GPO domaine)
    └── OU=Postes (GPO "Postes - Base")
        └── OU=Paris (GPO "Paris - Imprimantes")
            └── OU=Fixes  ← le PC est ici : cette GPO gagne en cas de conflit
```

**Exceptions** : **Blocage d'héritage** (section 58), **Application forcée / Enforced** (section 58), **Bouclage** (section 61), **Filtrage de sécurité/WMI** (sections 59-60).

```powershell
# Voir l'ordre des liens GPO sur une OU (LinkOrder : 1 = appliqué en dernier = gagne)
Get-GPInheritance -Target "OU=Fixes,OU=Paris,OU=Postes,DC=ad,DC=entreprise,DC=fr" |
  Select-Object -ExpandProperty GpoLinks |
  Select-Object DisplayName, Enabled, Enforced, Order
```

---

## 58. Héritage, blocage d'héritage et application forcée (Enforced)

- **Héritage** : par défaut, un objet reçoit les GPO de tous ses parents (LSDOU).
- **Blocage d'héritage** (clic droit sur l'OU > « Bloquer l'héritage ») : l'OU **ignore** les GPO des niveaux supérieurs (sauf celles marquées Enforced). À utiliser avec parcimonie : ça rend le dépannage infernal.
- **Application forcée (Enforced)** : la GPO s'applique **quoi qu'il arrive**, même avec blocage d'héritage, et **gagne** sur les conflits (elle remonte en tête de précédence).

```powershell
# Bloquer l'héritage sur une OU
Set-GPInheritance -Target "OU=DMZ,DC=ad,DC=entreprise,DC=fr" -IsBlocked Yes

# Marquer un lien comme Enforced
Set-GPLink -Name "SECU - Base durcissement" `
  -Target "DC=ad,DC=entreprise,DC=fr" -Enforced Yes

# Voir l'héritage effectif
Get-GPInheritance -Target "OU=Fixes,OU=Paris,OU=Postes,DC=ad,DC=entreprise,DC=fr"
```

**Règle d'or** : préférez le **filtrage de sécurité** (section 59) au blocage d'héritage. Le blocage est un marteau ; le filtrage est un scalpel.

---

## 59. Filtrage de sécurité des GPO

Par défaut, une GPO liée s'applique aux **Utilisateurs authentifiés**. Le filtrage de sécurité restreint à un groupe précis.

**Exemple** : la GPO « Imprimantes atelier » ne doit toucher que les PC de l'atelier :

```powershell
# 1. Créer le groupe (ordinateurs de l'atelier)
New-ADGroup -Name "GG_PC_Atelier" -GroupScope Global -GroupCategory Security `
  -Path "OU=Groupes,DC=ad,DC=entreprise,DC=fr"
# (ajouter les comptes d'ordinateurs dedans)

# 2. Remplacer le filtrage par défaut
Set-GPPermissions -Name "Imprimantes atelier" `
  -TargetName "Utilisateurs authentifiés" -TargetType Group -PermissionLevel None
Set-GPPermissions -Name "Imprimantes atelier" `
  -TargetName "GG_PC_Atelier" -TargetType Group -PermissionLevel GpoApply
```

⚠️ **Piège classique** (erreur n°3, section 78) : retirer « Utilisateurs authentifiés » **sans** ajouter le droit **Lecture** à un groupe. Depuis MS16-072 (2016), les **comptes d'ordinateurs** ont besoin du droit **Lecture** sur la GPO pour la traiter. Si vous filtrez, donnez au minimum `GpoRead` aux ordinateurs concernés (ou laissez « Utilisateurs authentifiés » en lecture seule + votre groupe en « Appliquer »).

```powershell
# Méthode sûre : lecture pour les ordinateurs du domaine, application pour le groupe cible
Set-GPPermissions -Name "Imprimantes atelier" -TargetName "Ordinateurs du domaine" `
  -TargetType Group -PermissionLevel GpoRead
Set-GPPermissions -Name "Imprimantes atelier" -TargetName "GG_PC_Atelier" `
  -TargetType Group -PermissionLevel GpoApply
```

---

## 60. Filtrage WMI

Le **filtrage WMI** applique une GPO uniquement si une requête WQL est vraie sur le poste (version d'OS, modèle, RAM...).

```powershell
# Créer un filtre WMI : Windows 11 uniquement
New-GPWmiFilter -Name "Filtre - Windows 11" `
  -Query 'SELECT * FROM Win32_OperatingSystem WHERE Version LIKE "10.0.226%" AND ProductType = "1"' `
  -Description "Cible Windows 11 22H2+"

# L'associer à une GPO
Set-GPO -Name "ORDI - Config Windows 11" -WmiFilter "Filtre - Windows 11"
```

**Filtres WMI utiles** :

```sql
-- Windows 10 uniquement
SELECT * FROM Win32_OperatingSystem WHERE Version LIKE "10.0.190%" AND ProductType = "1"
-- Postes avec au moins 8 Go de RAM
SELECT * FROM Win32_ComputerSystem WHERE TotalPhysicalMemory >= 8589934592
-- Modèle de PC précis (ex. déploiement pilote)
SELECT * FROM Win32_ComputerSystem WHERE Model LIKE "%Latitude 5440%"
-- Serveurs uniquement
SELECT * FROM Win32_OperatingSystem WHERE ProductType = "3"
```

⚠️ **Performance** : chaque filtre WMI est évalué à chaque traitement de stratégie. Un WMI lent ou une requête mal écrite **ralentit toutes les ouvertures de session**. Préférez le filtrage de sécurité quand c'est possible ; réservez le WMI aux critères non exprimables en groupes (version d'OS, matériel).

---

## 61. Bouclage de traitement (loopback) : fusion vs remplacement

Problème : les GPO « configuration utilisateur » suivent l'**utilisateur**, pas le poste. Mais sur un **serveur TSE/RDS** ou un **poste d'atelier partagé**, on veut que la config utilisateur dépende du **poste** (ex. verrouiller le bureau sur les PC d'atelier quel que soit l'utilisateur).

Solution : le **bouclage** (loopback), paramètre ordinateur :
`Configuration ordinateur > Stratégies > Modèles d'administration > Système > Stratégie de groupe > Configurer le mode de traitement bouclé`.

- **Fusion (Merge)** : les GPO utilisateur du poste **s'ajoutent** à celles de l'utilisateur. En cas de conflit, **celles de l'utilisateur gagnent**.
- **Remplacement (Replace)** : seules les GPO utilisateur liées au poste s'appliquent ; celles de l'utilisateur sont **ignorées**.

```powershell
# Activer le bouclage en mode Fusion via registre (équivalent du paramètre de stratégie)
# À déployer via une GPO "configuration ordinateur" liée à l'OU des serveurs RDS :
# Clé : HKLM\SOFTWARE\Policies\Microsoft\Windows\System\UserPolicyMode = 1 (Fusion) / 2 (Remplacement)
```

**Cas d'usage** : serveurs RDS (bureau verrouillé identique pour tous), postes en libre-service, salles de réunion, kiosques. **Piège** : oublier le bouclage actif quand on dépannage une GPO utilisateur « qui ne s'applique pas » sur un RDS.

---

## 62. Préférences de stratégie de groupe (GPP)

Les **préférences** (coche verte) vs les **stratégies** (policies) :

| | Stratégies | Préférences |
|---|---|---|
| Effet | **Imposées**, non modifiables par l'utilisateur (grisé) | Appliquées par défaut, **modifiables** ensuite par l'utilisateur |
| Persistance | Réappliquées, retirées si la GPO disparaît | Selon l'option (appliquer une fois, retirer si hors périmètre...) |
| Périmètre | Paramètres du système | Lecteurs, imprimantes, registre, fichiers, raccourcis, variables d'env... |

**Options communes des GPP** (onglet « Commun ») :

- **Appliquer une fois et ne pas réappliquer** : idéal pour un paramétrage initial que l'utilisateur peut changer.
- **Ciblage au niveau de l'élément** (section 64) : le vrai super-pouvoir des GPP.
- **Exécuter dans le contexte de sécurité de l'utilisateur** vs SYSTEM.

> 💡 Règle simple : **stratégie** pour la sécurité et la conformité (verrouillage, pare-feu), **préférence** pour le confort (lecteurs réseau, imprimantes par défaut, raccourcis).

---

## 63. GPP : lecteurs réseau, imprimantes, registre, raccourcis

**Lecteurs réseau** (`Configuration utilisateur > Préférences > Paramètres Windows > Mappages de lecteurs`) :

```powershell
# Équivalent script (mais préférez la GPP avec ciblage) :
# New-PSDrive -Name "S" -PSProvider FileSystem -Root "\\srv-fichiers\Commun" -Persist
```

En GPP : Nouvel élément > Lecteur mappé : lettre `S:`, chemin `\\srv-fichiers\Commun`, action **Mettre à jour**, reconnecter coché, ciblage sur le groupe `GG_Exploitation_Paris`.

**Imprimantes** (`Préférences > Paramètres du Panneau de configuration > Imprimantes`) : déployer l'imprimante partagée `\\srv-print\Atelier-HP`, la définir par défaut avec ciblage sur l'OU/le site.

**Registre** (`Préférences > Paramètres Windows > Registre`) : pousser une valeur, ex. page d'accueil du navigateur d'entreprise, avec action Mettre à jour.

**Raccourcis** (`Préférences > Paramètres Windows > Raccourcis`) : déposer un raccourci vers l'intranet sur le Bureau de tous les utilisateurs, ciblé par groupe.

**Bonnes pratiques GPP** :

- Action **Mettre à jour** (pas Remplacer, qui supprime/recrée à chaque fois = lent).
- Un élément = un ciblage clair.
- Évitez les chemins en dur vers des serveurs uniques : utilisez des **noms DNS d'espace de noms DFS** (`\\ad.entreprise.fr\partages\commun`).

---

## 64. Ciblage au niveau élément (Item-Level Targeting)

Le **ciblage** applique un élément GPP uniquement si des conditions sont vraies : groupe, OU, site, adresse IP, version d'OS, jour/heure, état du registre, etc. Conditions combinables en ET/OU.

**Exemples concrets** :

| Besoin | Ciblage |
|---|---|
| Imprimante Lyon uniquement | « Le site AD est Site-Lyon » |
| Lecteur compta pour les comptables | « L'utilisateur est membre de GG_Compta » |
| Raccourci VPN sur les portables | « Le nom d'ordinateur commence par LT- » (via requête LDAP/WMI) |
| Paramètre hors heures ouvrées | Plage de dates/heures |

C'est ce qui permet **une seule GPO « Mappages »** pour toute l'entreprise avec 10 éléments ciblés, plutôt que 10 GPO. Lisible, maintenable.

---

## 65. GPO de sécurité : audit et droits utilisateur

**Audit** (`Configuration ordinateur > Stratégies > Paramètres Windows > Paramètres de sécurité > Stratégies d'audit`) : activez a minima (recommandation) :

| Stratégie | Succès | Échec |
|---|---|---|
| Ouverture/fermeture de session | ✅ | ✅ |
| Gestion des comptes | ✅ | ✅ |
| Accès aux objets (partages sensibles) | ✅ | ✅ |
| Modification de stratégie | ✅ | ✅ |
| Utilisation des privilèges | ❌ (trop verbeux) | ✅ |

Sur 2019+ : utilisez les **sous-catégories d'audit avancé** (`Configuration avancée de la stratégie d'audit`) pour un contrôle fin.

**Attribution des droits utilisateur** (`Stratégies locales > Attribution des droits utilisateur`) :

- « Ouvrir une session locale » : restreignez sur les serveurs (pas les utilisateurs lambda sur un serveur applicatif !).
- « Arrêter le système », « Sauvegarder fichiers et répertoires » : cercle restreint.
- « Ajouter des stations de travail au domaine » : par défaut 10 pour tout utilisateur authentifié — **limitez** via ce paramètre ou `ms-DS-MachineAccountQuota`.

```powershell
# Limiter la jonction de domaine à 0 pour les utilisateurs standard (seuls les admins/Helpdesk via délégation)
Set-ADDomain -Identity "ad.entreprise.fr" -Replace @{ "ms-DS-MachineAccountQuota" = 0 }
```

---

## 66. Déploiement de logiciels via GPO (MSI)

`Configuration ordinateur (ou utilisateur) > Stratégies > Installation de logiciel` : déploie des **MSI** (ou ZAP, obsolète).

- **Attribué (Assigned)** : installé au démarrage (ordinateur) ou à l'ouverture (utilisateur), **obligatoire**.
- **Publié (Published)** : disponible dans « Programmes et fonctionnalités » / ajout de programmes, **optionnel** (utilisateur uniquement).

```powershell
# 1. Placer le MSI sur un partage accessible en LECTURE par "Ordinateurs du domaine"
#    (ou "Utilisateurs authentifiés" pour un déploiement utilisateur)
# 2. Dans la GPO : Nouveau > Package > chemin UNC (JAMAIS de lettre de lecteur !)
#    \\srv-logiciels\deploy\agent_inventaire.msi
# 3. Méthode de déploiement : Attribué
```

**Bonnes pratiques** :

- Testez le MSI en silencieux d'abord : `msiexec /i package.msi /qn`.
- Utilisez les **transformations MST** pour pré-configurer (clés, options).
- **Mises à jour** : nouvelle GPO ou « mise à niveau » du package dans la même GPO.
- **Désinstallation** : « Désinstaller quand hors du périmètre » pour le nettoyage automatique.
- Pour du lourd (Office, etc.) : préférez SCCM/Intune/PDQ au déploiement GPO, limité aux petits agents/outils.
- Surveillez les **démarrages lents** : un MSI attribué ordinateur retarde le boot ; planifiez.

---

## 67. Scripts de démarrage / arrêt / ouverture / fermeture de session

| Script | Contexte | Déclenchement |
|---|---|---|
| Démarrage | **Ordinateur** (SYSTEM) | Boot, avant l'ouverture de session |
| Arrêt | Ordinateur (SYSTEM) | Extinction |
| Ouverture de session | **Utilisateur** | Logon |
| Fermeture de session | Utilisateur | Logoff |

Emplacement GPO : `Configuration ordinateur/utilisateur > Stratégies > Paramètres Windows > Scripts`.

```powershell
# Exemple de script de démarrage : installer l'agent d'inventaire si absent
# startup_inventaire.ps1 (déployé via la GPO, exécution SYSTEM)
if (-not (Test-Path "C:\Program Files\Inventaire\agent.exe")) {
    Start-Process msiexec.exe -ArgumentList '/i "\\srv-logiciels\deploy\agent.msi" /qn' -Wait
}
```

**PowerShell vs Batch** : préférez PowerShell ; réglez la **stratégie d'exécution** via GPO (`Activer l'exécution de scripts` = `AllSigned` ou `RemoteSigned` selon votre maturité de signature).

⚠️ **Délais** : les scripts synchrones retardent boot/logon. Mettez un **timeout**, rendez-les idempotents, et loguez dans un partage central pour le dépannage.

---

## 68. Dépannage GPO : gpresult, rsop.msc, journaux

```powershell
# Rapport des stratégies appliquées (utilisateur + ordinateur)
/gpresult /r
gpresult /r /scope:computer
gpresult /r /scope:user

# Rapport HTML complet (le plus utile à distance)
gpresult /h C:\Temp\gpresult.html /f

# Forcer l'actualisation
gpupdate /force
gpupdate /force /target:computer

# Forcer à distance (sans déranger l'utilisateur si /target:computer)
Invoke-GPUpdate -Computer "PC-ATELIER-042" -Target "Computer" -Force
```

**rsop.msc** : jeu de stratégies résultant, vue console. **GPMC > Modélisation de stratégie de groupe** : simule l'application pour un utilisateur/poste (idéal pour « que se passerait-il si... »).

**Journaux** : `Observateur d'événements > Journaux des applications et des services > Microsoft > Windows > GroupPolicy > Operational`. Événements clés : **1500-1503** (traitement), **1058/1030** (SYSVOL inaccessible — souvent DNS ou DFSR).

**Test de connectivité SYSVOL** : `\\ad.entreprise.fr\SYSVOL\ad.entreprise.fr\Policies` doit s'ouvrir depuis le poste.

---

## 69. GPO qui ne s'applique pas : méthodologie complète

Checklist ordonnée (dans 90 % des cas, le problème est dans les 5 premiers) :

1. [ ] **Liaison** : la GPO est-elle liée au bon niveau (domaine/OU/site) ? `Get-GPInheritance`.
2. [ ] **Objet au bon endroit** : l'utilisateur/le PC est-il vraiment dans l'OU ciblée ? (Erreur classique : objet dans `CN=Computers`.)
3. [ ] **Filtrage de sécurité** : le groupe a-t-il `Appliquer` + `Lecture` ? Les ordinateurs ont-ils `Lecture` ? (section 59, piège MS16-072.)
4. [ ] **Filtre WMI** : la requête retourne-t-elle vrai sur le poste ? Testez avec `Get-WmiObject`.
5. [ ] **Héritage bloqué / Enforced** : `Get-GPInheritance -Target ...` montre `GpoInheritanceBlocked`.
6. [ ] **Moitié désactivée** : la GPO a-t-elle la moitié (ordinateur/utilisateur) dont vous avez besoin d'activée ?
7. [ ] **Réplication SYSVOL** : le paramètre existe-t-il sur le SYSVOL du DC qui sert le client ? (`dfsrdiag`, comparez deux DC.)
8. [ ] **gpupdate / reboot** : les stratégies ordinateur nécessitent souvent un redémarrage ; `gpupdate /force` ne suffit pas toujours.
9. [ ] **Bouclage** : actif sur ce poste ? (section 61.)
10. [ ] **Délai de réplication AD** : GPO créée sur DC01, poste authentifié sur DC02 pas encore répliqué → attendez ou forcez `repadmin /syncall`.
11. [ ] **Journaux** : GroupPolicy Operational, événements 1058/1030/1500.

```powershell
# Script de diagnostic rapide à lancer sur le poste en cause
gpresult /r
nltest /dsgetsite
Test-Path "\\ad.entreprise.fr\SYSVOL\ad.entreprise.fr\Policies"
```

---

## 70. Bonnes pratiques de nommage et gestion du cycle de vie des GPO

**Nommage** : `[PÉRIMÈTRE] - [Objet] - [Action]` :

- `ORDI - Tous - Verrouillage session 10 min`
- `USER - Exploitation - Mappage lecteur S`
- `SECU - Serveurs - Audit avancé`
- `SRV-RDS - Bouclage bureau verrouillé`

**Cycle de vie** :

1. Créer avec un **commentaire** (objectif, demandeur, date).
2. Tester sur une **OU pilote** (quelques postes).
3. **Sauvegarder** (`Backup-GPO`) avant chaque modification majeure.
4. Déployer progressivement.
5. **Désactiver** (pas supprimer) les GPO obsolètes pendant 3 mois, puis supprimer.
6. Revue annuelle : GPO non liées, vides, ou en doublon → nettoyage.

```powershell
# Trouver les GPO non liées (candidates au nettoyage)
Get-GPO -All | Where-Object {
  ($_ | Get-GPOReport -ReportType Xml) -notmatch "LinksTo"
} | Select-Object DisplayName

# Sauvegarder une GPO avant modification
Backup-GPO -Name "ORDI - Tous - Verrouillage session 10 min" `
  -Path "E:\Sauvegardes\GPO" -Comment "Avant passage à 5 min - $(Get-Date -Format 'yyyy-MM-dd')"
```

---

## 71. Santé du domaine : checklist quotidienne

À automatiser (script section 73) + coup d'œil humain de 5 minutes :

- [ ] `repadmin /replsummary` : 0 échec sur tous les DC.
- [ ] `dcdiag` rapide sur chaque DC (ou au moins les FSMO) : tout « passed ».
- [ ] Espace disque des DC (> 20 % libre sur le volume NTDS/SYSVOL).
- [ ] Verrouillages de comptes anormaux (pic = attaque ou service avec vieux mot de passe).
- [ ] Sauvegarde d'état système de la nuit : **succès** vérifié.
- [ ] Événements critiques (ID 2108, 1084, 1388, 16650, 4740 en masse).
- [ ] Le PDC est joignable et l'heure est saine (`w32tm /query /status`).

---

## 72. Santé du domaine : checklist hebdomadaire et mensuelle

**Hebdomadaire** :

- [ ] Revue des comptes créés/désactivés/supprimés de la semaine.
- [ ] Comptes verrouillés récurrents (même utilisateur 3 semaines d'affilée = problème).
- [ ] GPO modifiées (qui, quand) : `Get-GPO -All | Sort-Object ModificationTime -Descending`.
- [ ] Ordinateurs inactifs > 60 jours : à désactiver/supprimer après validation.
- [ ] Sauvegardes GPO à jour.
- [ ] Correctifs Windows sur les DC (fenêtre de maintenance planifiée, **un DC à la fois**, jamais tous ensemble).

**Mensuelle** :

- [ ] Revue des appartenances aux groupes sensibles (Admins du domaine, Opérateurs de sauvegarde...).
- [ ] Comptes avec mot de passe qui n'expire jamais : justification à jour ?
- [ ] Délégations sur les OU : toujours valides ?
- [ ] Test de restauration (labo isolé) au moins 2x/an.
- [ ] Documentation à jour (topologie, FSMO, mots de passe DSRM au coffre).
- [ ] Nettoyage : comptes/ordinateurs obsolètes, GPO orphelines.

```powershell
# Membres des groupes sensibles (revue mensuelle)
$groupes = "Admins du domaine","Admins de l’entreprise","Opérateurs de sauvegarde","Admins du schéma"
foreach ($g in $groupes) {
  Write-Host "== $g ==" -ForegroundColor Cyan
  Get-ADGroupMember -Identity $g -Recursive | Select-Object Name, objectClass
}
```

---

## 73. Scripts de rapport automatisés

**Rapport de santé quotidien** (à planifier via une tâche sur un serveur d'admin, avec un gMSA) :

```powershell
# Sante-AD-Quotidien.ps1
$rapport = "C:\Rapports\AD\Sante-$(Get-Date -Format 'yyyy-MM-dd').txt"
$dc = Get-ADDomainController -Filter * | Select-Object -ExpandProperty HostName

"=== REPLICATION ===" | Out-File $rapport
repadmin /replsummary | Out-File $rapport -Append

"`n=== DCDIAG (erreurs) ===" | Out-File $rapport -Append
foreach ($s in $dc) {
  "--- $s ---" | Out-File $rapport -Append
  dcdiag /s:$s | Select-String "failed|error" | Out-File $rapport -Append
}

"`n=== FSMO ===" | Out-File $rapport -Append
netdom query fsmo | Out-File $rapport -Append

"`n=== COMPTES VERROUILLES ===" | Out-File $rapport -Append
Search-ADAccount -LockedOut | Select-Object Name, SamAccountName | Out-File $rapport -Append

"`n=== DISQUE DC ===" | Out-File $rapport -Append
foreach ($s in $dc) {
  Get-WmiObject Win32_LogicalDisk -ComputerName $s -Filter "DriveType=3" |
    Select-Object @{N="Serveur";E={$s}}, DeviceID,
      @{N="Libre %";E={[math]::Round($_.FreeSpace/$_.Size*100,1)}} |
    Out-File $rapport -Append
}
# Envoi par mail (serveur SMTP interne)
Send-MailMessage -To "exploitation@entreprise.fr" -From "ad-rapport@entreprise.fr" `
  -Subject "[AD] Rapport santé $(Get-Date -Format 'yyyy-MM-dd')" `
  -Body (Get-Content $rapport | Out-String) -SmtpServer "smtp.entreprise.fr"
```

**Rapport mouvements de comptes (hebdo)** : interrogez le journal de sécurité du PDC (ID 4720 création, 4726 suppression, 4722 activation, 4725 désactivation) ou activez l'audit et centralisez (SIEM/Wazuh — voir votre guide Wazuh).

---

## 74. Supervision : compteurs et alertes critiques

| Source | ID | Gravité | Action |
|---|---|---|---|
| NTDS Replication | 2108, 1084 | 🔴 | Réplication en échec durable |
| NTDS Replication | 1388 | 🟠 | Lingering object potentiel |
| Directory Service | 16650 | 🔴 | Pool RID épuisé |
| Directory Service | 2162 | 🔴 | USN rollback détecté (VM-GenerationID) |
| Sécurité (PDC) | 4740 | 🟠 | Verrouillage de compte (pic = alerte) |
| Sécurité | 4728/4732/4756 | 🟠 | Ajout à un groupe sensible |
| Sécurité | 4720/4726 | 🟢 | Création/suppression de compte (revue) |
| DNS Server | 4015 | 🟠 | Problème DNS sur DC |
| Microsoft-Windows-Backup | 14, 8 | 🔴 | Échec sauvegarde |

**Seuils** : espace disque < 20 % = alerte, < 10 % = critique ; réplication > 60 min sans succès = alerte ; un DC injoignable > 15 min = critique (sauf maintenance déclarée).

---

## 75. Pense-bête de poche : commandes essentielles

```powershell
# --- Info ---
netdom query fsmo
Get-ADDomainController -Filter * | Select Name,Site,IsGlobalCatalog
repadmin /replsummary
dcdiag /e
nltest /dsgetsite          # site du poste
nltest /dclist:domaine     # liste des DC

# --- Utilisateurs / Groupes ---
New-ADUser / Get-ADUser / Set-ADUser / Disable-ADAccount / Unlock-ADAccount
New-ADGroup / Add-ADGroupMember / Get-ADGroupMember
Search-ADAccount -LockedOut | Unlock-ADAccount
Search-ADAccount -AccountInactive -TimeSpan 90 -UsersOnly

# --- GPO ---
Get-GPO -All
Backup-GPO -All -Path E:\Sauvegardes\GPO
gpresult /r ; gpresult /h rapport.html
gpupdate /force
Invoke-GPUpdate -Computer PC-01 -Force

# --- Réplication / Dépannage ---
repadmin /showrepl
repadmin /syncall DC01 /AdeP
repadmin /kcc
dcdiag /test:dns /v

# --- FSMO ---
Move-ADDirectoryServerOperationMasterRole -Identity DC02 -OperationMasterRole 0,1,2,3,4 -Force

# --- Divers ---
w32tm /query /status
redircmp "OU=Postes,DC=ad,DC=entreprise,DC=fr"
Enable-ADOptionalFeature -Identity "Recycle Bin Feature" -Scope ForestOrConfigurationSet -Target "ad.entreprise.fr"
```

---

## 76. Glossaire

| Terme | Définition |
|---|---|
| **ACL** | Access Control List : liste des autorisations sur un objet (qui peut faire quoi). |
| **ADAC** | Centre d'administration Active Directory (`dsacls.msc`), console moderne. |
| **AD DS** | Active Directory Domain Services : le rôle serveur d'annuaire. |
| **ADMT** | Active Directory Migration Tool : migration inter-forêts. |
| **AGDLP / AGUDLP** | Stratégie d'imbrication : Comptes → Global → (Universel) → Local de Domaine → Permissions. |
| **Approbation (Trust)** | Relation de confiance entre domaines/forêts. |
| **Arbre** | Domaines de même espace de noms DNS dans une forêt. |
| **Attribut** | Propriété d'un objet AD (ex. `telephoneNumber`). |
| **Bridgehead** | DC tête de pont pour la réplication inter-sites. |
| **Catalogue global (GC)** | Index partiel de tous les objets de la forêt (ports 3268/3269). |
| **Classe d'objet** | Type défini dans le schéma (user, group, computer...). |
| **CN** | Common Name, composant du DN. |
| **Corbeille AD** | Fonctionnalité de restauration des objets supprimés avec leurs attributs. |
| **DACL / SACL** | Parties de l'ACL : autorisations (DACL) et audit (SACL). |
| **DC** | Contrôleur de domaine. |
| **DFSR** | DFS Replication : réplication de SYSVOL (remplace FRS). |
| **DN** | Distinguished Name : chemin unique d'un objet. |
| **Domaine** | Frontière de réplication et d'administration. |
| **DSRM** | Directory Services Restore Mode : mode de restauration d'un DC. |
| **Enforced** | Option de lien GPO : s'applique malgré le blocage d'héritage, prioritaire. |
| **FGPP** | Fine-Grained Password Policy : stratégies de mot de passe par groupe (PSO). |
| **Forêt** | Ensemble des domaines partageant schéma et configuration. Frontière de sécurité. |
| **FSMO** | Flexible Single Master Operations : les 5 rôles maître unique. |
| **GC** | Voir Catalogue global. |
| **GPC / GPT** | Conteneur (AD) et modèle (SYSVOL) d'une GPO. |
| **GPMC** | Console de gestion des stratégies de groupe (`gpmc.msc`). |
| **GPO** | Group Policy Object : stratégie de groupe. |
| **GPP** | Préférences de stratégie de groupe (coche verte). |
| **GUID** | Identifiant unique d'objet (ex. identifiant d'une GPO). |
| **Héritage (GPO)** | Application cumulative des GPO parentes (LSDOU). |
| **IFM** | Install From Media : promotion d'un DC depuis un média pour liaisons lentes. |
| **Infrastructure Master** | FSMO maintenant les références inter-domaines. |
| **ISTG** | Inter-Site Topology Generator : DC générant la topologie inter-sites. |
| **Item-Level Targeting** | Ciblage conditionnel des éléments GPP. |
| **KCC** | Knowledge Consistency Checker : calcule la topologie de réplication. |
| **Kerberos** | Protocole d'authentification principal d'AD (port 88). |
| **LDAP / LDAPS** | Protocole d'accès à l'annuaire (389 / 636). |
| **LDS / AD LDS** | Instance d'annuaire légère (hors domaine). |
| **Lingering object** | Objet rémanent : supprimé partout mais réinjecté par un DC trop longtemps isolé. |
| **Loopback (bouclage)** | Mode GPO où la config utilisateur dépend du poste (Fusion/Remplacement). |
| **LSDOU** | Ordre d'application : Local, Site, Domaine, OU. |
| **Maître de schéma** | FSMO autorisant les modifications du schéma (forêt). |
| **Maître RID** | FSMO distribuant les pools de RID (domaine). |
| **Metadata cleanup** | Nettoyage des métadonnées d'un DC mort. |
| **MS16-072** | Correctif imposant le droit Lecture aux ordinateurs sur les GPO filtrées. |
| **Naming Master** | FSMO d'attribution des noms de domaine (forêt). |
| **NTDS.dit** | Fichier de la base AD sur chaque DC. |
| **NTP / w32time** | Hiérarchie de temps, pilotée par l'émulateur PDC. |
| **OU** | Organizational Unit : conteneur pour GPO et délégation. |
| **PDC Emulator** | FSMO : temps, verrouillages, mots de passe urgents, édition GPO. |
| **Phantom** | Objet fantôme : référence inter-domaine maintenue par l'Infrastructure Master. |
| **PSO** | Password Settings Object : objet de stratégie FGPP. |
| **RDS** | Remote Desktop Services : bureaux distants (cas typique du bouclage). |
| **repadmin** | Outil de diagnostic de la réplication. |
| **RID** | Relative Identifier : fin du SID, alloué par pool. |
| **RODC** | Read-Only Domain Controller : DC en lecture seule. |
| **RSOP** | Resultant Set of Policy : stratégies résultantes (`rsop.msc`, `gpresult`). |
| **Schéma** | Définition des classes et attributs, commun à la forêt. |
| **Seize** | Saisie forcée d'un rôle FSMO (détenteur mort). |
| **SID** | Security Identifier : identifiant unique portant les droits. |
| **SID History** | Conservation des anciens SID lors d'une migration (ADMT). |
| **Site** | Emplacement réseau bien connecté (réplication, proximité client). |
| **Site Link** | Liaison logique entre sites (coût, fréquence). |
| **sAMAccountName** | Login court pré-Windows 2000 (`DOMAINE\login`, 20 car. max). |
| **SYSVOL** | Partage répliqué (`\\domaine\SYSVOL`) contenant les GPO et scripts. |
| **Tombstone** | Objet supprimé conservé temporairement pour la réplication (180 j). |
| **Transfert (FSMO)** | Déplacement propre d'un rôle (détenteur en ligne). |
| **UPN** | User Principal Name : `prenom.nom@domaine`, login moderne. |
| **USN** | Update Sequence Number : compteur d'écritures par DC. |
| **USN rollback** | Divergence quand l'USN d'un DC recule (restore de snapshot). |
| **WMI Filter** | Filtre GPO basé sur une requête WMI (version OS, matériel...). |

---

## 77. Quiz : 10 questions + réponses

**Q1. Dans quel ordre s'appliquent les GPO ?**
<details><summary>Réponse</summary>LSDOU : Local → Site → Domaine → OU (de la plus éloignée à la plus proche de l'objet). En cas de conflit, la dernière appliquée gagne, sauf lien Enforced ou blocage d'héritage.</details>

**Q2. Un utilisateur change de service. Selon AGDLP, que modifiez-vous ?**
<details><summary>Réponse</summary>Uniquement son appartenance aux groupes **globaux** (ex. le retirer de `GG_Compta` et l'ajouter à `GG_Exploitation`). Les groupes locaux de domaine et les ACL sur les ressources ne changent pas.</details>

**Q3. Le détenteur du rôle PDC tombe en panne un vendredi soir. Quelles conséquences immédiates ?**
<details><summary>Réponse</summary>La référence de temps du domaine est perdue (dérive possible), les verrouillages de compte ne sont plus centralisés, l'édition des GPO via la GPMC peut poser problème (elle cible le PDC). La plupart des authentifications continuent. Si la panne est définitive : saisir le rôle (seize) sur un autre DC ; si temporaire : attendre ou transférer.</details>

**Q4. Pourquoi ne faut-il jamais restaurer un DC à partir d'un snapshot hyperviseur ?**
<details><summary>Réponse</summary>Risque d'**USN rollback** : l'USN du DC restauré recule, ses partenaires croient déjà avoir répliqué ces numéros et ignorent les écritures intermédiaires → divergence silencieuse (mots de passe, comptes). On restaure un DC via une sauvegarde d'état système, jamais par snapshot.</details>

**Q5. Une GPO liée à une OU ne s'applique pas à un PC qui est bien dans l'OU. Citez 4 vérifications.**
<details><summary>Réponse</summary>1) Filtrage de sécurité : le PC a-t-il les droits Lecture + Appliquer (piège MS16-072) ? 2) Héritage bloqué sur l'OU ? 3) Filtre WMI faux sur ce poste ? 4) Réplication SYSVOL/GPO pas encore arrivée sur le DC qui sert le client (`gpresult /r`, `Get-GPInheritance`).</details>

**Q6. Quelle est la différence entre transférer et saisir un rôle FSMO ?**
<details><summary>Réponse</summary>Le **transfert** (`Move-ADDirectoryServerOperationMasterRole`) se fait avec le détenteur en ligne : propre et réversible. La **saisie** (`ntdsutil` → seize) se fait quand le détenteur est mort et ne reviendra jamais : l'ancien DC ne doit plus jamais être reconnecté (risque de RID dupliqués).</details>

**Q7. À quoi sert un RODC dans une agence sans local sécurisé ?**
<details><summary>Réponse</summary>Il authentifie localement sans exposer une base inscriptible : en cas de vol du serveur, l'attaquant ne récupère qu'une copie en lecture seule, et par défaut aucun mot de passe n'est mis en cache (sauf stratégie de réplication autorisée). Les écritures sont relayées vers un DC inscriptible.</details>

**Q8. Vous supprimez par erreur l'OU « Compta » (200 utilisateurs). La corbeille AD est activée. Que faites-vous ?**
<details><summary>Réponse</summary>`Get-ADObject -Filter {Name -eq "Compta"} -IncludeDeletedObjects | Restore-ADObject`, puis restaurer les objets enfants, réactiver les comptes, vérifier les appartenances aux groupes (conservées grâce à la corbeille). Pas besoin de restauration faisant autorité.</details>

**Q9. Quelle GPO définit la longueur minimale des mots de passe du domaine, et peut-on en mettre une différente par service ?**
<details><summary>Réponse</summary>La **Default Domain Policy** (liée à la racine du domaine) définit la stratégie par défaut. Oui pour différencier : les **FGPP/PSO** permettent des politiques par groupe (ex. 16 caractères pour les admins), avec un ordre de précédence.</details>

**Q10. `repadmin /replsummary` montre des échecs croissants entre deux sites. Citez 4 causes possibles et les commandes de diagnostic.**
<details><summary>Réponse</summary>Causes : liaison réseau/VPN coupée, DNS défaillant, décalage d'heure > 5 min (Kerberos), pare-feu bloquant les ports AD. Diagnostics : `dcdiag /test:dns /v`, `repadmin /showrepl`, `w32tm /query /status`, `Test-NetConnection DC-Distant -Port 389`, vérification des liaisons de sites et coûts.</details>

---

## 78. Erreurs classiques (18)

### Erreur n°1 — Suppression d'une OU « protégée » impossible (et inversement, OU non protégée supprimée par erreur)
**Symptôme** : « Accès refusé » à la suppression d'une OU alors qu'on est admin du domaine.
**Cause** : la case « Protéger l'objet contre une suppression accidentelle » est cochée (c'est le comportement par défaut à la création via les consoles).
**Remède** : décocher la protection (ou `Set-ADOrganizationalUnit -ProtectedFromAccidentalDeletion $false`), puis supprimer.
**Prévention** : activez la protection sur **toutes** les OU de production via script (section 17) ; l'erreur inverse (suppression d'une OU non protégée) se rattrape avec la corbeille AD (section 49).

### Erreur n°2 — GPO liée mais jamais appliquée : l'objet est dans le conteneur par défaut
**Symptôme** : `gpresult /r` ne montre pas la GPO ; pourtant elle est liée « au domaine ».
**Cause** : le PC/l'utilisateur est dans `CN=Computers` / `CN=Users`, pas dans une OU. Les GPO liées à des **OU** ne touchent pas les conteneurs par défaut, et on ne peut pas lier de GPO à un conteneur `CN=`.
**Remède** : déplacer l'objet dans la bonne OU ; exécuter `redircmp`/`redirusr` une fois pour toutes (section 26).
**Prévention** : `redircmp` dès la création du domaine ; procédure de jonction incluant le pré-staging.

### Erreur n°3 — Filtrage de sécurité : GPO invisible à cause de MS16-072
**Symptôme** : après avoir remplacé « Utilisateurs authentifiés » par un groupe restreint, plus personne ne reçoit la GPO.
**Cause** : depuis 2016, le **compte ordinateur** doit avoir le droit **Lecture** sur la GPO pour la traiter. En retirant « Utilisateurs authentifiés », on a retiré la lecture aux ordinateurs.
**Remède** : ajouter « Ordinateurs du domaine » (ou « Utilisateurs authentifiés ») en `GpoRead`, et le groupe cible en `GpoApply` (section 59).
**Prévention** : ne jamais mettre `None` sans ajouter la lecture ; tester sur un poste pilote.

### Erreur n°4 — Heure non synchronisée : authentifications Kerberos en échec
**Symptôme** : ouvertures de session impossibles, erreurs « l'horloge n'est pas synchronisée », réplication en échec, `dcdiag` rouge.
**Cause** : décalage > 5 minutes entre client et DC (Kerberos), souvent après une panne NTP, une VM dont l'horloge dérive, ou un PDC mal configuré.
**Remède** : `w32tm /resync` sur le poste ; vérifier la hiérarchie (`w32tm /query /status`), reconfigurer le PDC sur des sources externes fiables (section 32) ; désactiver la synchro d'heure de l'hyperviseur vers les DC (le PDC fait foi).
**Prévention** : supervision du décalage NTP ; le PDC synchronisé sur 2-3 sources externes ; GPO éventuelle pour forcer `w32time`.

### Erreur n°5 — Rétrograder un DC en l'éteignant simplement
**Symptôme** : erreurs de réplication persistantes, objets serveur fantômes, `dcdiag` en échec, lenteurs d'ouverture de session.
**Cause** : un DC éteint sans rétrogradation laisse ses métadonnées, ses rôles FSMO éventuels et ses enregistrements DNS.
**Remède** : `Remove-ADDomainController` / suppression via Sites et services (metadata cleanup, section 12), saisie des FSMO si besoin (section 35), nettoyage DNS.
**Prévention** : procédure écrite de décommission : transférer FSMO → `Uninstall-ADDSDomainController` → vérifier → désinstaller le rôle.

### Erreur n°6 — Saisir un rôle FSMO alors que le détenteur est juste injoignable
**Symptôme** : deux DC se croient maîtres RID ; SID en double ; objets impossibles à créer ; schéma divergent.
**Cause** : `seize` lancé pendant une simple coupure réseau ou une maintenance prolongée, puis l'ancien détenteur revient en ligne.
**Remède** : il n'y en a pas de bon — c'est une situation de crise : isoler l'ancien détenteur, le réinstaller from scratch, nettoyer les métadonnées. D'où la règle : **seize = mort définitive**.
**Prévention** : ne saisir qu'après confirmation que le serveur est irrécupérable (disque mort, VM détruite) ; toujours préférer le transfert.

### Erreur n°7 — Restore de snapshot d'un DC virtualisé (USN rollback)
**Symptôme** : mots de passe « qui ne passent plus », comptes créés qui disparaissent selon le DC interrogé, événement 2108/1084.
**Cause** : retour en arrière de l'USN (section 43).
**Remède** : rétrogradation forcée + metadata cleanup + repromotion du DC fautif. Pas de réparation « douce ».
**Prévention** : sauvegardes d'état système uniquement ; VM-GenerationID actif ; interdire les snapshots sur les DC dans la politique de virtualisation ; former l'équipe.

### Erreur n°8 — Schéma étendu sans sauvegarde (irréversible)
**Symptôme** : après une extension de schéma ratée (mauvais OID, attribut mal défini), impossible de revenir en arrière proprement.
**Cause** : les suppressions de classes/attributs du schéma sont **définitives dans leur effet** (on peut désactiver, pas effacer).
**Remède** : restauration faisant autorité de **toute la forêt** depuis une sauvegarde antérieure — opération lourde, à éviter.
**Prévention** : sauvegarde état système avant ; test en labo ; ne jamais étendre le schéma « pour essayer » en production.

### Erreur n°9 — Mot de passe DSRM inconnu le jour où on en a besoin
**Symptôme** : impossible de démarrer en DSRM pour restaurer ; restauration bloquée en pleine crise.
**Cause** : mot de passe défini à la promotion puis oublié, jamais documenté, jamais changé au départ des admins.
**Remède** : préventif uniquement — le changer régulièrement via `ntdsutil`/`Set-DSRMPassword` et le stocker au coffre (section 52).
**Prévention** : entrée « mots de passe DSRM » dans le coffre d'entreprise, rotation annuelle, vérification semestrielle.

### Erreur n°10 — GPO « fourre-tout » modifiée sans sauvegarde
**Symptôme** : un changement dans une GPO géante casse 300 postes ; impossible de revenir en arrière précisément.
**Cause** : pas de `Backup-GPO` avant modification, pas de commentaire, pas d'OU pilote.
**Remède** : restaurer depuis la dernière sauvegarde GPO (`Restore-GPO`) ; sinon, corriger à la main paramètre par paramètre.
**Prévention** : une GPO = un objectif ; `Backup-GPO` systématique avant changement ; commentaire obligatoire ; déploiement pilote.

### Erreur n°11 — Bouclage (loopback) oublié sur un serveur RDS
**Symptôme** : les GPO utilisateur « ne s'appliquent pas » sur le RDS alors que tout est correct côté utilisateur.
**Cause** : le bouclage en mode Remplacement ignore les GPO utilisateur de l'utilisateur ; en mode Fusion, l'ordre de précédence surprend.
**Remède** : vérifier le paramètre `UserPolicyMode` dans `gpresult /r` (section « Stratégie de groupe ») ; ajuster Fusion/Remplacement selon le besoin (section 61).
**Prévention** : documenter les serveurs en bouclage ; GPO dédiée `SRV-RDS - Bouclage` bien nommée.

### Erreur n°12 — Filtre WMI qui ralentit toutes les ouvertures de session
**Symptôme** : logons de 2-3 minutes après l'ajout d'un filtre WMI « anodin ».
**Cause** : requête WMI lente ou évaluée sur des milliers de postes (ex. `Win32_Product`, notoirement lent).
**Remède** : remplacer par un filtrage de sécurité (groupes) ; si WMI indispensable, requête minimale sur `Win32_OperatingSystem`/`Win32_ComputerSystem`.
**Prévention** : tester le temps d'évaluation (`Measure-Command { Get-WmiObject -Query ... }`) ; bannir `Win32_Product`.

### Erreur n°13 — Stratégie de mot de passe mise dans une GPO d'OU (sans effet)
**Symptôme** : « j'ai mis 14 caractères minimum dans la GPO de l'OU Compta, ça ne marche pas ».
**Cause** : les paramètres de stratégie de mot de passe/verrouillage ne s'appliquent que depuis la **Default Domain Policy** (ou GPO liée à la racine avec précédence). Une GPO d'OU est ignorée pour ces paramètres.
**Remède** : configurer dans la Default Domain Policy ; pour différencier par population, utiliser les **FGPP/PSO** (section 46).
**Prévention** : le savoir (c'est le but de cette fiche !) ; documenter les PSO existants.

### Erreur n°14 — DC laissé sans sauvegarde d'état système pendant des mois
**Symptôme** : crash disque un lundi matin ; dernière sauvegarde : 4 mois ; la restauration faisant autorité est impossible (tombstone lifetime dépassée ? non, mais perte de données).
**Cause** : tâche `wbadmin` jamais planifiée ou en échec silencieux (alerte non configurée).
**Remède** : reconstruire un DC par promotion (si d'autres DC sains) ; sinon restauration + procédure de crise.
**Prévention** : `wbadmin enable backup -systemstate -schedule`, supervision du journal `Microsoft-Windows-Backup`, test de restauration semestriel en labo.

### Erreur n°15 — Sous-réseaux non déclarés dans Sites et services
**Symptôme** : clients d'une agence authentifiés sur un DC distant (ouvertures lentes), `nltest /dsgetsite` retourne `Default-First-Site-Name`.
**Cause** : le sous-réseau de l'agence n'est pas déclaré/associé au bon site.
**Remède** : créer le sous-réseau et l'associer au site (section 38) ; `nltest /dsgetsite` pour vérifier ; `gpupdate`.
**Prévention** : checklist « nouveau site / nouveau VLAN » incluant systématiquement la déclaration AD ; audit trimestriel des sous-réseaux vs réalité réseau.

### Erreur n°16 — Compte de service avec mot de passe en dur qui expire
**Symptôme** : une application tombe en panne tous les 42/90 jours ; « on change le mot de passe du compte svc_appli partout ».
**Cause** : compte utilisateur classique utilisé comme compte de service, mot de passe avec expiration, documenté nulle part.
**Remède** : migrer vers un **gMSA** (section 27) ; à défaut, PSO dédié + procédure de rotation documentée.
**Prévention** : interdire les comptes de service « utilisateur » ; inventaire des comptes de service ; gMSA par défaut pour tout nouveau besoin.

### Erreur n°17 — Suppression d'un groupe utilisé dans des ACL sans vérifier
**Symptôme** : après suppression d'un groupe « qui ne servait plus », des accès partages/applications cassés.
**Cause** : le groupe était dans des ACL NTFS ou des groupes imbriqués ; la suppression est immédiate et la corbeille ne restaure pas toujours les usages externes.
**Remède** : restaurer le groupe (corbeille), vérifier les ACL ; à défaut, recréer et réassigner (nouveau SID = tout refaire).
**Prévention** : avant suppression, vérifier `memberOf` inverse et les ACL des partages critiques ; désactiver/renommer d'abord (« _OBSOLETE »), supprimer 3 mois après.

### Erreur n°18 — Mise à jour Windows simultanée de tous les DC
**Symptôme** : après un week-end de patching, plus aucun DC joignable pendant les reboots simultanés ; authentifications en échec.
**Cause** : tous les DC redémarrés en même temps (WSUS/GPO mal réglée).
**Remède** : ne jamais patcher les DC en parallèle : **un par un**, en vérifiant `dcdiag`/`repadmin` entre chaque.
**Prévention** : groupes de maintenance étalés ; le PDC en dernier ; fenêtre de maintenance dédiée aux DC ; procédure écrite.

---

## 79. Cas pratiques commentés (16)

### Cas n°1 — Créer une arborescence complète pour une nouvelle agence
**Contexte** : ouverture de l'agence de Lyon (30 utilisateurs, 25 PC, 1 imprimante).
**Objectif** : OU, groupes, GPO de base, tout par script pour reproductibilité.
```powershell
$dom = "DC=ad,DC=entreprise,DC=fr"
# OU
New-ADOrganizationalUnit -Name "Lyon" -Path "OU=Utilisateurs,$dom" -ProtectedFromAccidentalDeletion $true
New-ADOrganizationalUnit -Name "Lyon" -Path "OU=Postes,$dom" -ProtectedFromAccidentalDeletion $true
# Groupes
New-ADGroup "GG_Utilisateurs_Lyon" -GroupScope Global -GroupCategory Security -Path "OU=Groupes,$dom"
New-ADGroup "GG_PC_Lyon" -GroupScope Global -GroupCategory Security -Path "OU=Groupes,$dom"
# GPO imprimante, filtrée sur les PC de Lyon
New-GPO -Name "ORDI - Lyon - Imprimante atelier" -Comment "Déploiement imprimante Lyon"
New-GPLink -Name "ORDI - Lyon - Imprimante atelier" -Target "OU=Lyon,OU=Postes,$dom"
Set-GPPermissions -Name "ORDI - Lyon - Imprimante atelier" -TargetName "Utilisateurs authentifiés" -TargetType Group -PermissionLevel None
Set-GPPermissions -Name "ORDI - Lyon - Imprimante atelier" -TargetName "Ordinateurs du domaine" -TargetType Group -PermissionLevel GpoRead
Set-GPPermissions -Name "ORDI - Lyon - Imprimante atelier" -TargetName "GG_PC_Lyon" -TargetType Group -PermissionLevel GpoApply
```
**Commentaire** : tout est scripté → l'agence suivante se déploie en changeant 3 variables. La GPO imprimante est configurée ensuite dans la GPMC (GPP > Imprimantes partagées).

### Cas n°2 — Intégrer 40 intérimaires en une matinée
**Contexte** : pic d'activité, 40 intérimaires arrivent lundi 8h.
**Objectif** : comptes créés, expirant dans 30 jours, mot de passe initial, groupes par atelier.
**Solution** : CSV + script de la section 20, avec `-AccountExpirationDate (Get-Date).AddDays(30)`. Imprimer la liste login/mot de passe initiaux, remise en main propre par le chef d'atelier.
**Commentaire** : l'expiration automatique évite les comptes fantômes. Prévoir le script inverse (désactivation) — section 22.

### Cas n°3 — Départ d'un salarié : procédure complète
**Contexte** : départ d'un technicien un vendredi soir.
**Objectif** : couper les accès immédiatement, proprement, avec traçabilité.
**Solution** : script section 22 (désactivation + mot de passe aléatoire + déplacement OU + retrait des groupes + description datée). Vérifier `Search-ADAccount -LockedOut` n'est pas concerné, archiver la fiche.
**Commentaire** : ne supprimez pas le compte tout de suite (besoin RH/legal, réattribution de fichiers). La suppression intervient après le délai défini (ex. 90 jours).

### Cas n°4 — Restaurer un utilisateur supprimé par erreur
**Contexte** : le helpdesk a supprimé `j.martin` au lieu de `j.martine`.
**Objectif** : restaurer avec groupes et attributs.
```powershell
Get-ADObject -Filter { SamAccountName -eq "j.martin" } -IncludeDeletedObjects | Restore-ADObject
Enable-ADAccount j.martin
Set-ADAccountPassword j.martin -Reset -NewPassword (ConvertTo-SecureString "Temporaire!2026" -AsPlainText -Force)
Set-ADUser j.martin -ChangePasswordAtLogon $true
Get-ADUser j.martin -Properties MemberOf | Select -ExpandProperty MemberOf
```
**Commentaire** : grâce à la corbeille, les appartenances aux groupes sont revenues. Sans corbeille, il aurait fallu tout réassigner à la main.

### Cas n°5 — Déployer un nouveau DC sur un site distant
**Contexte** : l'agence de Lyon grandit (60 personnes), le RODC ne suffit plus.
**Objectif** : promouvoir un DC inscriptible à Lyon.
**Étapes** : 1) `dcdiag /e` + `repadmin /replsummary` propres. 2) Serveur 2022 joint au domaine, DNS vers Paris. 3) `Install-ADDSDomainController` avec `-SiteName "Site-Lyon"` (ou déplacer après). 4) Vérifier réplication, DNS (`_msdcs`), GC. 5) Ajouter son IP dans le DHCP de Lyon. 6) Surveiller 48h.
**Commentaire** : utilisez `-InstallationMediaPath` (IFM) si la liaison est lente. Ne transférez aucun FSMO à Lyon (reste à Paris, section 36).

### Cas n°6 — Transférer les rôles FSMO avant maintenance du DC principal
**Contexte** : remplacement des disques du DC01 (détient les 5 FSMO), intervention de 4h.
**Objectif** : zéro impact.
```powershell
# Avant : santé
dcdiag /e | Select-String "passed|failed"
# Transfert vers DC02
Move-ADDirectoryServerOperationMasterRole -Identity "DC02" -OperationMasterRole 0,1,2,3,4 -Force
netdom query fsmo
# ... maintenance ...
# Retour (optionnel, pour garder la doc cohérente)
Move-ADDirectoryServerOperationMasterRole -Identity "DC01" -OperationMasterRole 0,1,2,3,4 -Force
```
**Commentaire** : le transfert est sans coupure (quelques secondes). Le PDC est le rôle dont l'absence se sent le plus : transférez-le en priorité si vous ne déplacez pas tout.

### Cas n°7 — Mettre en place une PSO pour les administrateurs
**Contexte** : audit interne : les admins ont la même politique mot de passe que les utilisateurs.
**Objectif** : 16 caractères min, 60 jours max, verrouillage à 3 essais pour les admins.
**Solution** : script de la section 46 (`PSO-Admins`, Precedence 10, appliqué à « Admins du domaine »). Vérifier avec `Get-ADUserResultantPasswordPolicy` sur un admin.
**Commentaire** : communiquez avant d'appliquer (les admins devront changer leur mot de passe au prochain cycle). Combinez avec du MFA.

### Cas n°8 — Diagnostiquer une réplication en échec entre deux sites
**Contexte** : `repadmin /replsummary` montre 12 échecs vers le site de Lyon, en augmentation.
**Objectif** : identifier la cause racine.
**Méthode** :
```powershell
repadmin /showrepl DC-Lyon          # détail des erreurs (ex. 1722 = RPC inaccessible)
dcdiag /test:dns /v                 # le DNS est coupable dans 50% des cas
Test-NetConnection DC-Lyon -Port 389
w32tm /query /status                # décalage d'heure ?
Get-WinEvent -LogName "Directory Service" | Where-Object Id -in 2108,1084 | Select -First 5
```
**Résolution typique** : VPN tombé, règle pare-feu, ou DNS. Après réparation : `repadmin /syncall /AdeP` puis surveillance.
**Commentaire** : notez l'heure de début des échecs et croisez avec les changements réseau — la cause est presque toujours un changement récent.

### Cas n°9 — Nettoyer un DC mort après crash disque
**Contexte** : DC03 (site distant) : disque mort, pas de sauvegarde récente, les 2 autres DC sont sains.
**Objectif** : nettoyer l'annuaire.
**Étapes** : 1) Vérifier qu'il ne détenait aucun FSMO (`netdom query fsmo`). 2) `Remove-ADDomainController -Identity "DC03" -Force` ou suppression via Sites et services. 3) Nettoyer le DNS (enregistrements A, NS, `_msdcs`). 4) `repadmin /replsummary`, `dcdiag /e`. 5) Reconstruire un serveur neuf et le promouvoir (jamais de « réparation » de l'ancien).
**Commentaire** : si DC03 détenait des FSMO, les saisir (section 35) **avant** le nettoyage.

### Cas n°10 — Déployer une imprimante par GPP avec ciblage par site
**Contexte** : chaque site a son imprimante ; les utilisateurs nomades doivent avoir la bonne par défaut.
**Objectif** : une seule GPO « Imprimantes », 3 éléments ciblés.
**Solution** : GPO `USER - Tous - Imprimantes` liée à la racine Utilisateurs ; 3 éléments GPP « Imprimante partagée » (`\\srv-print\Paris`, `\\srv-print\Lyon`...) chacun avec ciblage « le site AD est ... ». Cocher « Définir comme imprimante par défaut ».
**Commentaire** : le ciblage par site suit l'utilisateur quand il se déplace — mieux qu'une GPO par OU de site pour les nomades.

### Cas n°11 — Verrouiller les postes d'atelier partagés (bouclage)
**Contexte** : 10 PC d'atelier partagés par 40 opérateurs en 3x8 ; il faut un bureau identique et verrouillé.
**Objectif** : config utilisateur imposée par le poste.
**Solution** : OU `Atelier` avec les 10 PC ; GPO `ORDI - Atelier - Bouclage` activant le loopback en **Remplacement** ; GPO `USER - Atelier - Bureau verrouillé` (même OU) configurant le bureau : fond imposé, pas de panneau de config, verrouillage 5 min.
**Commentaire** : en mode Remplacement, les GPO utilisateur de l'opérateur sont ignorées sur ces PC — documentez-le pour éviter des tickets « mes lecteurs ont disparu à l'atelier ».

### Cas n°12 — Migrer les comptes de service vers des gMSA
**Contexte** : 8 applications utilisent `svc_appli` (mot de passe qui n'expire jamais, connu de 3 personnes).
**Objectif** : passer au gMSA, un par application.
**Étapes** (par application) : 1) `Add-KdsRootKey` (une fois). 2) Créer le gMSA, autoriser les serveurs. 3) `Install-ADServiceAccount` sur chaque serveur. 4) Changer le compte du service (services.msc → `ad\gmsa_appli$`, mot de passe vide). 5) Redémarrer le service, vérifier les logs. 6) Désactiver l'ancien `svc_appli` après 1 mois de validation.
**Commentaire** : faites-le application par application, jamais en big-bang. Certaines vieilles applis ne supportent pas les gMSA : PSO dédié en repli.

### Cas n°13 — Auditer et réduire les membres des groupes sensibles
**Contexte** : préparation d'un audit de sécurité.
**Objectif** : liste justifiée des admins.
```powershell
"Admins du domaine","Admins de l’entreprise","Opérateurs de sauvegarde" | ForEach-Object {
  Write-Host "== $_ ==" -ForegroundColor Cyan
  Get-ADGroupMember -Identity $_ -Recursive | Format-Table Name, objectClass
}
```
**Suite** : pour chaque membre, justification écrite (fonction, besoin). Retirer les comptes de service et les anciens prestataires. Mettre en place la revue mensuelle (section 72).
**Commentaire** : le groupe « Admins du domaine » doit tenir sur une demi-page. Au-delà, c'est un symptôme.

### Cas n°14 — Sauvegarder et restaurer une GPO critique
**Contexte** : modification risquée de la GPO « Proxy entreprise ».
**Objectif** : pouvoir revenir en arrière en 2 minutes.
```powershell
Backup-GPO -Name "USER - Tous - Proxy entreprise" -Path "E:\Sauvegardes\GPO" `
  -Comment "Avant changement proxy $(Get-Date -Format 'yyyy-MM-dd')"
# ... modification ...
# En cas de problème :
Restore-GPO -Name "USER - Tous - Proxy entreprise" -Path "E:\Sauvegardes\GPO\<GUID_sauvegarde>"
gpupdate /force
```
**Commentaire** : la sauvegarde GPO ne sauvegarde que la GPO, pas son lien ni son filtrage — documentez-les aussi (export `Get-GPOReport -ReportType Html`).

### Cas n°15 — Préparer la montée de niveau fonctionnel vers 2016/2025
**Contexte** : dernier DC 2012 R2 décommissionné, parc en 2019/2022.
**Objectif** : monter le niveau fonctionnel.
**Étapes** : 1) `Get-ADForest/Domain` : vérifier les niveaux. 2) `dcdiag /e`, `repadmin /replsummary` : santé parfaite. 3) Sauvegarde état système. 4) `Set-ADDomainMode` puis `Set-ADForestMode` (irréversible). 5) Activer la corbeille si ce n'est pas fait. 6) Documenter.
**Commentaire** : ne montez le niveau que quand **tous** les DC sont au bon OS et que vous êtes sûr de ne pas devoir réintroduire un vieux DC.

### Cas n°16 — Mettre en place le rapport de santé quotidien
**Contexte** : pas de supervision AD, on découvre les pannes par les utilisateurs.
**Objectif** : mail quotidien automatique.
**Solution** : script de la section 73 déposé sur le serveur d'administration, tâche planifiée quotidienne 7h avec un gMSA, envoi vers l'équipe exploitation. Ajouter une alerte immédiate (pas quotidienne) sur les ID critiques 2108/16650/2162 via le planificateur (« déclencher sur un événement »).
**Commentaire** : commencez simple (mail quotidien), affinez ensuite (filtrage du bruit, intégration au SIEM/Wazuh). Un rapport que personne ne lit ne sert à rien : désignez un lecteur responsable par roulement.

---

## 80. Pour aller plus loin

**Documentation officielle** :
- Microsoft Learn : « Active Directory Domain Services Overview » — la référence, à jour pour Server 2025.
- Référence des cmdlets : module `ActiveDirectory` et `GroupPolicy` (docs Microsoft).

**Sécurisation avancée** :
- **Tiering administratif** (modèle ESAE / « Red Forest » simplifié) : comptes admin par niveau (Tier 0 = DC), jamais de compte Tier 0 sur un poste utilisateur.
- **LAPS** (Windows Local Administrator Password Solution, intégré depuis 2023) : mots de passe admin locaux uniques et rotatifs.
- **Protected Users** : groupe imposant Kerberos durci (pas de NTLM, pas de délégation).
- **Authentication Policies & Silos** : restreindre où un compte peut s'authentifier (2012 R2+).
- Durcissement LDAP : exiger **LDAP signing** et **LDAPS** (avis Microsoft ADV190023).

**Outils complémentaires** :
- **PingCastle** : audit de sécurité AD gratuit, rapport actionnable — à lancer trimestriellement.
- **ADRecon / AD ACL Scanner** : inventaire et revue des ACL.
- **Specops / Lithnet** : protection contre les mots de passe compromis.
- **Wazuh** (votre guide existant) : centraliser les journaux AD et alerter sur les ID critiques de la section 74.

**Montée en compétence** :
- Labo : 2-3 VM (un DC 2022, un DC 2025, un client Windows 11) sous Hyper-V/Proxmox — rejouez chaque cas pratique de la section 79.
- Exercices : cassez volontairement (GPO filtrée, heure décalée, DC isolé) puis réparez avec ce guide.
- Veille : blog « Directory Services » Microsoft, notes de version de Windows Server.

**Sujets connexes à vos autres guides** : la supervision AD via **Zabbix/Prometheus** (compteurs NTDS), les sauvegardes avec **Veeam/Borg** (complément de wbadmin), l'automatisation avec **Ansible** (module `microsoft.ad`), le durcissement avec votre guide **Debian/Ubuntu** pour les serveurs membres Linux joints au domaine (SSSD/realmd).

---

## 81. Checklists de mise en production

### Nouveau domaine / nouvelle forêt
- [ ] Nom de domaine choisi définitivement (sous-domaine d'un nom public possédé).
- [ ] Niveau fonctionnel cible décidé.
- [ ] Premier DC promu, DNS intégré, GC actif.
- [ ] `redircmp` exécuté vers `OU=Postes`.
- [ ] Corbeille AD activée.
- [ ] Mots de passe DSRM au coffre.
- [ ] Arborescence OU créée et protégée.
- [ ] Groupes de base (AGDLP) créés.
- [ ] Default Domain Policy configurée (mots de passe, verrouillage, audit de base).
- [ ] PSO Admins créé.
- [ ] Sauvegarde état système planifiée + testée.
- [ ] Rapport de santé quotidien en place.
- [ ] Documentation (FSMO, topologie, schéma réseau) au wiki.

### Nouveau DC
- [ ] `dcdiag /e` propre avant.
- [ ] Promotion réussie, redémarrage OK.
- [ ] `repadmin /showrepl` : réplication entrante/sortante OK.
- [ ] DNS : enregistrements `_msdcs` présents.
- [ ] GC activé si prévu.
- [ ] SYSVOL partagé (`net share`).
- [ ] DHCP/DNS clients mis à jour.
- [ ] Ajouté à la supervision et aux sauvegardes.

### Nouvelle GPO
- [ ] Objectif unique, nommage conforme, commentaire renseigné.
- [ ] Testée sur OU pilote.
- [ ] `Backup-GPO` effectué.
- [ ] Filtrage de sécurité vérifié (lecture ordinateurs !).
- [ ] `gpresult /r` sur poste pilote : appliquée.
- [ ] Déploiement progressif, puis généralisation.
- [ ] Entrée au journal des changements.

---

## 82. Annexe : versions et niveaux fonctionnels

| Version | Niveau fonctionnel max | Points d'attention |
|---|---|---|
| Windows Server 2019 | 2016 (`WinThreshold`) | Fin du support mainstream : 09/01/2024 ; support étendu jusqu'au 09/01/2029. |
| Windows Server 2022 | 2016 (`WinThreshold`) | Pas de nouveau niveau fonctionnel ; améliorations sécurité (SMB AES-256, DNS-over-HTTPS). |
| Windows Server 2025 | 2025 (`Win2025`) ⚠️ | Nouveau niveau : base NTDS en pages 32K, NUMA, DLNA... Ne montez à `Win2025` que si tous les DC sont en 2022+ et que vous n'avez plus besoin de compatibilité antérieure. |

⚠️ **Avertissement version** : le niveau fonctionnel `Win2025` est irréversible et encore jeune au moment de la rédaction (2026) — validez en labo avant production, surtout si des applications tierces étendent le schéma.

**Matrice de compatibilité rapide** : un DC 2025 peut cohabiter avec des DC 2019/2022 en niveau 2016. La montée vers `Win2025` exige l'absence de DC 2019 et antérieurs.

**Fin de vie à surveiller** : planifiez le retrait des DC 2019 avant janvier 2029 (fin du support étendu). Inscrivez-le à votre feuille de route.

---

*Fin du guide. Gardez ce document à jour : chaque changement d'architecture (nouveau site, nouveau niveau fonctionnel, nouvelle PSO) mérite une mise à jour des sections concernées et de la documentation d'exploitation.*
