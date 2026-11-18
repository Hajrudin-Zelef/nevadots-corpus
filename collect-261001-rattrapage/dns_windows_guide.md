# DNS sous Windows Server en entreprise — Guide technique ultra-complet

> **Public :** administrateurs systèmes & réseaux, chefs de service.
> **Versions couvertes :** Windows Server 2019, 2022 et 2025.
> **Convention :** tous les exemples utilisent le domaine fictif `contoso.local` et des adresses RFC 1918 (`10.0.0.0/8`). Aucune donnée réelle.
> **Avertissement :** certaines applets PowerShell ou options GUI varient légèrement selon la version — les différences sont signalées par `⚠️ Version`.

---

## Sommaire

- Chapitre 1 — Fondamentaux DNS et DNS dans Active Directory (§1–§12)
- Chapitre 2 — Installation du rôle DNS et prise en main (§13–§22)
- Chapitre 3 — Les zones DNS (§23–§34)
- Chapitre 4 — Les enregistrements de ressources (§35–§46)
- Chapitre 5 — Transferts de zone AXFR/IXFR (§47–§51)
- Chapitre 6 — Vieillissement (aging) et scavenging (§52–§56)
- Chapitre 7 — Redirecteurs et indications de racine (§57–§62)
- Chapitre 8 — Split-brain DNS et stratégies DNS (§63–§68)
- Chapitre 9 — DNSSEC (§69–§76)
- Chapitre 10 — DNS, Active Directory et DHCP (§77–§84)
- Chapitre 11 — Haute disponibilité (§85–§89)
- Chapitre 12 — Dépannage (§90–§99) et 15 cas pratiques (§100–§114)
- Chapitre 13 — Sécurité (§115–§122)
- Chapitre 14 — 18 erreurs classiques (§123–§140)
- Chapitre 15 — Exploitation : checklists, sauvegarde, supervision (§141–§144)
- Pense-bête des commandes (§145) · Quiz (§146–§147) · Glossaire (§148) · Pour aller plus loin (§149)

**Détail des sections :**

1. Objectif de ce guide et périmètre · 2. Rappels : qu'est-ce que le DNS ? · 3. Résolution récursive vs itérative · 4. Hiérarchie DNS : racine, TLD, domaines · 5. Le rôle du DNS dans Active Directory · 6. Les enregistrements SRV d'AD en détail · 7. La zone `_msdcs` · 8. Sites AD et SRV par site · 9. Enregistrements A des contrôleurs de domaine · 10. Comment un client localise un DC (flux complet) · 11. DNS et stratégie de nommage · 12. TTL : principes · 13. Installation du rôle DNS (GUI) · 14. Installation via PowerShell et Server Core · 15. La console DNS (dnsmgmt.msc) · 16. Le module PowerShell DnsServer : tour d'horizon · 17. dnscmd.exe : l'outil historique · 18. Vérifier l'état du service DNS · 19. Configurer les adresses d'écoute · 20. Configurer la carte réseau du serveur DNS lui-même · 21. Checklist d'installation · 22. Premier test de résolution · 23. Les types de zones : panorama · 24. Zones primaires standard · 25. Zones secondaires · 26. Zones stub · 27. Zones de recherche directe vs inversée · 28. Zones intégrées à Active Directory · 29. Portées de réplication AD · 30. Avantages et limites des zones AD-integrated · 31. Créer une zone primaire (GUI + PowerShell) · 32. Créer une zone inversée · 33. Convertir une zone standard en zone AD-integrated · 34. Suspendre, recharger, supprimer une zone · 35. Anatomie d'un enregistrement · 36. A et AAAA · 37. CNAME : alias et pièges · 38. MX : messagerie · 39. SRV : services · 40. PTR : résolution inverse · 41. TXT : usages · 42. NS et glue records · 43. SOA : le début d'autorité · 44. CAA : autorité de certification · 45. Les enregistrements SRV d'AD : liste complète · 46. Créer des enregistrements en PowerShell : synthèse · 47. AXFR vs IXFR · 48. Configurer le transfert de zone (GUI) · 49. Configurer en PowerShell / dnscmd · 50. Sécuriser les transferts · 51. Notify, SOA serial et cohérence · 52. Le problème des enregistrements périmés · 53. No-refresh et refresh intervals : le mécanisme · 54. Activer le vieillissement sur une zone · 55. Activer le scavenging au niveau serveur · 56. Bonnes pratiques et pièges du scavenging · 57. Redirecteurs : principe · 58. Indications de racine : principe · 59. Redirecteurs vs racine : quand utiliser quoi · 60. Configurer les redirecteurs · 61. Redirecteurs conditionnels · 62. Chaînage et boucles : à éviter · 63. Le problème du split-brain · 64. Solutions classiques au split-brain · 65. Split-brain avec les stratégies DNS (zone scopes) · 66. Stratégies DNS : client subnets · 67. Stratégies DNS : exemples concrets · 68. Bonnes pratiques split-brain · 69. DNSSEC : pourquoi · 70. Chaîne de confiance : KSK, ZSK, DS · 71. Signer une zone sur Windows Server · 72. Gestion des clés et rollover · 73. Ancres de confiance (trust anchors) · 74. Validation DNSSEC côté résolveur · 75. NSEC vs NSEC3 · 76. Limites et points d'attention DNSSEC · 77. Mises à jour dynamiques : principe · 78. Secure vs non-secure vs none · 79. DNS et DHCP : qui enregistre quoi · 80. Configurer les credentials DHCP · 81. Le groupe DnsUpdateProxy : utilité et danger · 82. Protéger les enregistrements existants · 83. Scavenging et DHCP : le duo gagnant · 84. Cas des clients non-Windows et statiques · 85. Plusieurs serveurs DNS : architecture type · 86. AD-integrated : la HA naturelle · 87. Secondaires et transferts pour la HA · 88. Anycast DNS : principe · 89. Supervision de la disponibilité DNS · 90. Méthodologie de dépannage DNS · 91. nslookup : bases et pièges · 92. Resolve-DnsName : le successeur · 93. dnscmd pour le diagnostic · 94. Test-DnsServer · 95. Vider le cache : client et serveur · 96. Les journaux DNS : lequel regarder · 97. Journal analytique DNS · 98. Debug logging (dnscmd /LogLevel) · 99. Event IDs à connaître · 100–114. 15 cas pratiques commentés · 115. Surface d'attaque du DNS · 116. Cache poisoning et protections · 117. Restreindre la récursion · 118. Response Rate Limiting (RRL) · 119. Cache locking · 120. Le groupe DnsAdmins : risque d'escalade · 121. Durcissement : liste de mesures · 122. DNS et pare-feu : flux réseau · 123–140. 18 erreurs classiques · 141. Checklist de mise en production · 142. Checklist d'audit trimestriel · 143. Sauvegarde et restauration des zones · 144. Supervision : compteurs et alertes · 145. Pense-bête des commandes · 146. Quiz : 10 questions · 147. Réponses du quiz · 148. Glossaire · 149. Pour aller plus loin

---

# Chapitre 1 — Fondamentaux DNS et DNS dans Active Directory

## 1. Objectif de ce guide et périmètre

Ce guide couvre le DNS **tel qu'il tourne en entreprise sous Windows Server 2019/2022/2025**, avec un focus sur l'intégration Active Directory — parce qu'en pratique, 90 % des pannes DNS en entreprise sont des pannes d'AD déguisées (localisation de DC, réplication, scavenging agressif).
Chaque notion est accompagnée de commandes PowerShell testées syntaxiquement, de tableaux de référence et de cas réels.
Hypothèses : forêt `contoso.local`, deux sites AD (`Paris-Siege`, `Lyon-Usine`), serveurs `SRV-DNS-01`/`SRV-DNS-02` en `10.0.0.10`/`10.0.0.11`.

## 2. Rappels : qu'est-ce que le DNS ?

Le DNS (Domain Name System) est une base de données distribuée et hiérarchique qui associe des noms à des informations (adresses IP, serveurs de messagerie, services…).
Points clés à ne jamais perdre de vue en exploitation :

| Concept | Ce qu'il faut retenir |
|---|---|
| Hiérarchie | Racine `.` → TLD (`.fr`, `.local`) → domaines → sous-domaines. La délégation se fait par enregistrements NS. |
| Protocole | UDP 53 par défaut, TCP 53 pour les réponses > 512 octets (ou avec EDNS > 4096), les transferts de zone et DNSSEC. |
| Modèle | Autoritaire (le serveur *possède* la zone) vs récursif/résolveur (le serveur *cherche* pour le client). Un serveur Windows fait souvent les deux. |
| Cohérence | Pas de transaction globale : la donnée se propage par transferts de zone et réplication AD, avec un délai. |
| TTL | Chaque enregistrement a une durée de vie en cache. Baisser le TTL *avant* un changement, pas pendant. |

## 3. Résolution récursive vs itérative

C'est la distinction la plus mal comprise — et la source de la moitié des erreurs d'architecture.

**Résolution récursive (côté client → résolveur) :** le client pose une question et attend *la* réponse finale. Le résolveur fait tout le travail. C'est ce que fait un poste Windows quand il interroge son serveur DNS configuré.

**Résolution itérative (résolveur → serveurs autoritaires) :** le résolveur interroge la racine, qui répond « je ne sais pas, demande au serveur du TLD », qui répond « demande au serveur autoritaire », etc. Chaque serveur ne donne que le meilleur indice suivant (*referral*).

```powershell
# Voir la différence en pratique : trace complète de la résolution itérative
Resolve-DnsName -Name "www.contoso.local" -Server "10.0.0.10" -DnsOnly
# Avec -Trace (Windows 10/Server 2019+) : affiche chaque étape itérative
Resolve-DnsName -Name "www.example.com" -Trace
```

**Règle d'architecture :** vos serveurs DNS internes doivent offrir la récursion à vos clients internes (sinon rien ne se résout), mais **jamais** au monde entier (serveur exposé = récursion désactivée, §117).

## 4. Hiérarchie DNS : racine, TLD, domaines

```
.  (racine, 13 identités a.root-servers.net … m.root-servers.net)
└── local          ← votre forêt AD (non routé sur Internet, c'est voulu)
│   └── contoso
│       ├── _msdcs              ← enregistrements de localisation des DC
│       ├── Paris-Siege._sites ← SRV spécifiques au site
│       └── srv-fichiers        ← A : 10.0.1.20
└── fr / com / …               ← Internet, via redirecteurs ou racine
```

Le nom de votre forêt AD (`contoso.local`) est une **zone autoritaire** sur vos DC/DNS. Tout ce qui n'est pas dans vos zones est résolu par récursion (redirecteurs ou indications de racine, chapitre 7).

## 5. Le rôle du DNS dans Active Directory

Active Directory **ne fonctionne pas sans DNS**. Kerberos, LDAP, la réplication, l'ouverture de session : tout commence par une requête DNS. Concrètement :

1. Le client cherche un contrôleur de domaine via des requêtes **SRV** (`_ldap._tcp.dc._msdcs.contoso.local`).
2. Il affine par **site AD** (`_ldap._tcp.Paris-Siege._sites.dc._msdcs.contoso.local`) pour contacter le DC le plus proche.
3. Il résout le nom du DC en IP via un enregistrement **A**.
4. Kerberos utilise ensuite ces noms pour obtenir des tickets.

**Conséquence opérationnelle :** un DNS qui répond « vite mais faux » (vieux cache, zone périmée, split-brain mal géré) provoque des ouvertures de session lentes, des GPO non appliquées et des erreurs de réplication. Le DNS est un composant d'infrastructure critique, pas un « service annexe ».

## 6. Les enregistrements SRV d'AD en détail

Le service Netlogon de chaque DC enregistre automatiquement (toutes les heures par défaut, ou via `nltest /dsregdns`) une série de SRV. Syntaxe générique :

```
_service._proto.nom.  TTL  IN  SRV  priorité  poids  port  cible
```

Les plus importants pour `contoso.local` :

| Enregistrement SRV | Rôle |
|---|---|
| `_ldap._tcp.dc._msdcs` | Localiser *un* DC (LDAP, port 389) |
| `_ldap._tcp.<Site>._sites.dc._msdcs` | Localiser un DC *du site* |
| `_kerberos._tcp.dc._msdcs` | Localiser un DC pour Kerberos (port 88) |
| `_kerberos._tcp.<Site>._sites.dc._msdcs` | Kerberos du site |
| `_gc._tcp` | Catalogue global (port 3268) |
| `_gc._tcp.<Site>._sites` | Catalogue global du site |
| `_kpasswd._tcp` / `_kpasswd._udp` | Changement de mot de passe Kerberos (port 464) |
| `_ldap._tcp.pdc._msdcs` | Émulateur PDC (port 389) |

Priorité basse = préféré ; à priorité égale, le **poids** répartit la charge.

## 7. La zone `_msdcs.contoso.local`

`_msdcs` est une **zone séparée**, déléguée depuis `contoso.local`, qui contient les enregistrements critiques pour la forêt entière :

- Les SRV `_tcp.dc`, `_tcp.gc`, `_tcp.pdc`, `_udp.kerberos`…
- Un **CNAME par DC** nommé d'après son GUID d'objet NTDS (`<GUID>._msdcs.contoso.local` → nom du DC). La réplication AD utilise ces CNAME : si l'un manque, la réplication vers ce DC casse.

```powershell
# Lister la zone _msdcs
Get-DnsServerResourceRecord -ZoneName "_msdcs.contoso.local" -RRType SRV |
    Select-Object HostName, @{n='Cible';e={$_.RecordData.DomainName}}
# Vérifier le CNAME GUID d'un DC
Resolve-DnsName -Name "<GUID>._msdcs.contoso.local" -Type CNAME
```

⚠️ **Ne supprimez jamais la zone `_msdcs`**, même si elle « semble vide » dans la console (erreur n°8, §130).

## 8. Sites AD et SRV par site

Les sites AD (`Paris-Siege`, `Lyon-Usine`) servent à orienter les clients vers le DC le plus proche via les SRV `_sites`. Le client détermine son site grâce à son sous-réseau (Sites et services AD → Subnets).

```powershell
# Voir les enregistrements SRV d'un site
Get-DnsServerResourceRecord -ZoneName "_msdcs.contoso.local" |
    Where-Object { $_.HostName -like "*Paris-Siege*" } |
    Select-Object HostName, RecordType
```

**Bonnes pratiques :**
- Un sous-réseau = un site, toujours. Un sous-réseau oublié = des clients qui traversent le WAN pour s'authentifier.
- Au moins un DC (idéalement deux) par site de production + un catalogue global par site.
- Les enregistrements `_sites` sont enregistrés par Netlogon en fonction du site du DC lui-même.

## 9. Enregistrements A des contrôleurs de domaine

Chaque DC enregistre aussi :

- Un **A** à son nom (`SRV-DC-01.contoso.local` → `10.0.0.10`) — utilisé par Kerberos et la réplication.
- Un **A** au nom du **domaine nu** (`contoso.local` → IP de *chaque* DC) : c'est du round-robin DNS qui répartit les requêtes LDAP/Kerberos sur tous les DC. C'est normal d'y voir plusieurs IP.

```powershell
# Les A du domaine nu (round-robin normal)
Resolve-DnsName -Name "contoso.local" -Type A
```

Si vous filtrez ou supprimez ces A « parce qu'il y a trop d'IP », vous cassez la répartition de charge et la localisation des DC.

## 10. Comment un client localise un DC (flux complet)

1. Le client lit son suffixe DNS (`contoso.local`) et son site (via son IP → subnet → site).
2. Requête SRV : `_ldap._tcp.Paris-Siege._sites.dc._msdcs.contoso.local`.
3. Si aucun DC dans le site : repli sur `_ldap._tcp.dc._msdcs.contoso.local` (n'importe quel DC).
4. Tri par priorité/poids, résolution A du DC choisi, connexion LDAP port 389.
5. Kerberos : même logique avec `_kerberos._tcp…` (port 88).

```powershell
# Simuler ce que fait un client (depuis le poste)
nltest /dsgetdc:contoso.local /site:Paris-Siege
nltest /dnsgetdc:contoso.local
```

## 11. DNS et stratégie de nommage

- **Nom de forêt :** un suffixe non routable (`.local`, `.interne`, `.lan`) évite les collisions avec l'Internet public et le besoin de split-brain sur le nom de domaine lui-même. ⚠️ `.local` est aussi utilisé par mDNS/Bonjour : en pratique ça cohabite, mais documentez-le.
- **Noms de serveurs :** courts, sans accents, sans underscore (sauf les noms réservés type `_msdcs`, `_tcp`… qui sont légaux en DNS mais pas en NetBIOS).
- **Évitez les noms à un seul label** (`serveur` sans domaine) : ça casse Kerberos et les certificats. Toujours un FQDN.
- **Cohérence DHCP/DNS :** le suffixe DNS distribué par DHCP (option 015) doit correspondre à la zone AD.

## 12. TTL : principes

Le TTL (Time To Live, en secondes) dit aux résolveurs combien de temps garder l'enregistrement en cache.

| Usage | TTL conseillé |
|---|---|
| Enregistrements d'infrastructure stables (NS, SOA, A de serveurs) | 1 heure à 1 jour (3600–86400) |
| Enregistrements SRV d'AD | 600 s (10 min, valeur Netlogon par défaut) |
| Enregistrements appelés à changer (bascule, migration) | 300 s (5 min) **avant** l'opération |
| Enregistrements DHCP dynamiques | 1200 s (20 min) par défaut |

**Règle d'or des changements :** baissez le TTL à 300 s **au moins 2× l'ancien TTL avant** la migration, changez l'IP, puis remontez le TTL. Baisser le TTL *pendant* la migration ne sert à rien : les caches ont déjà l'ancienne valeur.

```powershell
# Lire le TTL d'un enregistrement (TimeToLive est un TimeSpan)
(Get-DnsServerResourceRecord -ZoneName "contoso.local" -Name "www" -RRType A).TimeToLive
# Changer le TTL : on réécrit l'enregistrement
$old = Get-DnsServerResourceRecord -ZoneName "contoso.local" -Name "www" -RRType A
$new = $old.Clone(); $new.TimeToLive = [TimeSpan]::FromMinutes(5)
Set-DnsServerResourceRecord -NewInputObject $new -OldInputObject $old -ZoneName "contoso.local" -PassThru
```

---

# Chapitre 2 — Installation du rôle DNS et prise en main

## 13. Installation du rôle DNS (GUI)

Via le **Gestionnaire de serveur → Ajouter des rôles et fonctionnalités → Rôle « Serveur DNS »**. Sur un contrôleur de domaine, la case est généralement déjà cochée (le rôle DNS s'installe avec AD DS si vous l'avez demandé dans l'assistant de promotion).

Points de vigilance GUI :
- Sur une installation **Server Core**, pas de GUI : utilisez PowerShell (§14).
- Après installation, la console `dnsmgmt.msc` apparaît dans Outils d'administration.
- Le service s'appelle `DNS Server` (nom de service : `DNS`).

## 14. Installation via PowerShell et Server Core

```powershell
# Installer le rôle DNS + outils d'administration (GUI ou Core)
Install-WindowsFeature -Name DNS -IncludeManagementTools

# Vérifier
Get-WindowsFeature -Name DNS | Select-Object Name, Installed, InstallState

# Sur un serveur distant
Install-WindowsFeature -Name DNS -IncludeManagementTools -ComputerName "SRV-DNS-02" -Credential (Get-Credential)

# Démarrage automatique + démarrage immédiat (normalement déjà OK après install)
Set-Service -Name DNS -StartupType Automatic
Start-Service -Name DNS
```

⚠️ Version : sur Windows Server 2025, `Install-WindowsFeature` existe toujours ; l'alternative moderne `Enable-WindowsOptionalFeature` ne concerne pas les rôles serveur.

## 15. La console DNS (dnsmgmt.msc)

`dnsmgmt.msc` (ou `dnsmgmt.msc /s` en autonome) affiche :
- Le serveur → **zones de recherche directe**, **zones de recherche inversée**, **redirecteurs conditionnels**, **points de confiance**.
- Clic droit sur le serveur → Propriétés : les onglets **Interfaces**, **Redirecteurs**, **Indications de racine**, **Avancé** (scavenging, recursion…), **Journalisation de débogage**.

Réflexes console :
- **Actualiser** (F5) après une action PowerShell : la console ne se rafraîchit pas toute seule.
- Affichage → **Avancé** pour voir les enregistrements cachés (type WINS, _msdcs détaillé).
- Ne modifiez jamais les fichiers de zone `.dns` à la main pendant que le service tourne.

## 16. Le module PowerShell DnsServer : tour d'horizon

Le module `DnsServer` (chargé automatiquement) couvre ~140 applets. Les familles à connaître :

| Famille | Exemples | Usage |
|---|---|---|
| Zones | `Get/Add/Remove/Set-DnsServerZone`, `Add-DnsServerPrimaryZone`… | Création, conversion, suppression |
| Enregistrements | `Get/Add/Remove/Set-DnsServerResourceRecord*` | CRUD des RR |
| Transferts | `Get/Set-DnsServerZoneTransfer` | AXFR/IXFR |
| Vieillissement | `Set-DnsServerZoneAging`, `Set-DnsServerScavenging` | Scavenging |
| Résolution | `Set/Get-DnsServerForwarder`, `Get-DnsServerRootHint`, `Set-DnsServerRecursion` | Redirecteurs/racine |
| Diagnostics | `Test-DnsServer`, `Get-DnsServerDiagnostics`, `Clear-DnsServerCache` | Dépannage |
| Stratégies | `Add-DnsServerClientSubnet`, `Add-DnsServerQueryResolutionPolicy` | DNS policies (2016+) |
| DNSSEC | `Add-DnsServerSigningKey`, `Set-DnsServerDnsSecZoneSetting` | Signature |

```powershell
# Lister toutes les applets du module
Get-Command -Module DnsServer | Select-Object Name | Sort-Object Name
# Aide détaillée d'une applet
Get-Help Add-DnsServerResourceRecordA -Full
```

## 17. dnscmd.exe : l'outil historique

`dnscmd` reste utile pour : le debug logging (`/Config /LogLevel`), le vieillissement forcé (`/AgeAllRecords`), et les scripts hérités. Il est disponible dès que le rôle DNS (ou les outils RSAT-DNS) est installé.

```powershell
# Exemples courants
dnscmd SRV-DNS-01 /Info                 # configuration complète du serveur
dnscmd SRV-DNS-01 /ZoneInfo contoso.local
dnscmd SRV-DNS-01 /ClearCache
dnscmd SRV-DNS-01 /ZoneReload contoso.local
```

**Recommandation :** privilégiez PowerShell pour tout ce qui est nouveau ; gardez `dnscmd` pour les 3 cas ci-dessus et le dépannage sur vieux systèmes.

## 18. Vérifier l'état du service DNS

```powershell
# Service + port d'écoute
Get-Service -Name DNS
Get-NetTCPConnection -LocalPort 53 -State Listen | Select-Object LocalAddress, OwningProcess
Get-NetUDPEndpoint -LocalPort 53 | Select-Object LocalAddress

# Le serveur se résout-il lui-même ?
Resolve-DnsName -Name "SRV-DNS-01.contoso.local" -Server "10.0.0.10"

# Test fonctionnel complet depuis le serveur
Test-DnsServer -IPAddress "10.0.0.10" -ZoneName "contoso.local"
```

Si le port 53 n'écoute pas : vérifiez les **interfaces** (§19) et qu'un autre service (un autre DNS, un contrôleur de domaine tiers) ne squatte pas le port.

## 19. Configurer les adresses d'écoute

Par défaut, le DNS écoute sur **toutes** les adresses. Sur un serveur multi-homé (ex : interface de sauvegarde, iLO partagé), restreignez :

```powershell
# N'écouter que sur 10.0.0.10
Set-DnsServer -ListenAddresses "10.0.0.10"
Get-DnsServer | Select-Object -ExpandProperty ListenAddresses

# Revenir à toutes les adresses
Set-DnsServer -ListenAddresses @()
```

GUI : Propriétés du serveur → onglet **Interfaces**. ⚠️ Si vous restreignez, n'oubliez pas `127.0.0.1` si des applications locales interrogent le DNS en loopback.

## 20. Configurer la carte réseau du serveur DNS lui-même

Règle d'or (source de l'erreur n°1, §123) : **un serveur DNS/AD doit se pointer lui-même en premier**.

| Serveur | DNS préféré | DNS auxiliaire |
|---|---|---|
| `SRV-DNS-01` (10.0.0.10) | `127.0.0.1` (ou 10.0.0.10) | `10.0.0.11` |
| `SRV-DNS-02` (10.0.0.11) | `127.0.0.1` (ou 10.0.0.11) | `10.0.0.10` |
| Serveur membre / poste | `10.0.0.10` | `10.0.0.11` |

```powershell
# Configurer via PowerShell (interface "Ethernet0" par ex.)
Set-DnsClientServerAddress -InterfaceAlias "Ethernet0" -ServerAddresses "127.0.0.1","10.0.0.11"
Get-DnsClientServerAddress -InterfaceAlias "Ethernet0" | Select-Object -ExpandProperty ServerAddresses
```

Ne mettez **jamais** un DNS externe (FAI, 8.8.8.8) dans la carte d'un DC : le DC doit résoudre l'AD en priorité, l'externe passe par les redirecteurs (§57).

## 21. Checklist d'installation

- [ ] Rôle DNS installé (`Get-WindowsFeature DNS` → Installed = True)
- [ ] Service `DNS` en démarrage automatique et démarré
- [ ] Interfaces d'écoute restreintes si multi-homé (§19)
- [ ] Carte réseau : lui-même en premier, partenaire en second (§20)
- [ ] Pare-feu : UDP/TCP 53 ouverts vers les clients et les autres DNS (§122)
- [ ] Redirecteurs configurés et testés (§60), ou indications de racine vérifiées
- [ ] Zone(s) créée(s), SOA/NS corrects (§31)
- [ ] Test bout-en-bout : `Test-DnsServer` + `Resolve-DnsName` depuis un client
- [ ] Supervision : le port 53 et le service sont monitorés (§144)

## 22. Premier test de résolution

```powershell
# 1. Le serveur répond-il ?
Resolve-DnsName -Name "contoso.local" -Server "10.0.0.10" -Type SOA

# 2. Récursion vers Internet OK ?
Resolve-DnsName -Name "www.example.com" -Server "10.0.0.10"

# 3. Un client du site résout-il un DC ?
Resolve-DnsName -Name "_ldap._tcp.dc._msdcs.contoso.local" -Type SRV -Server "10.0.0.10"

# 4. Temps de réponse (mesure simple)
Measure-Command { Resolve-DnsName -Name "www.contoso.local" -Server "10.0.0.10" -DnsOnly | Out-Null }
```

Si l'étape 2 échoue mais pas la 1 : problème de redirecteurs/racine ou de pare-feu sortant (voir cas pratique n°7/§107).

---

# Chapitre 3 — Les zones DNS

## 23. Les types de zones : panorama

| Type | Autoritaire ? | Modifiable localement ? | Usage |
|---|---|---|---|
| Primaire (standard) | Oui | Oui (fichier `.dns`) | Petit site sans AD, ou zone exposée |
| Primaire **AD-integrated** | Oui | Oui (répliquée via AD) | **Standard en entreprise** : multi-maîtres, sécurisée |
| Secondaire | Oui (lecture seule) | Non (copie du primaire) | Redondance, serveurs hors AD, DMZ |
| Stub | Non | Non (NS + SOA + glue uniquement) | Connaître les NS autoritaires d'une zone distante |
| Redirecteur conditionnel | Non | Non (renvoi vers des IP) | Router une zone vers des DNS spécifiques |

Règle : en domaine AD, **toutes les zones internes sont AD-integrated** sauf besoin particulier (zone exposée en DMZ → primaire standard + secondaire).

## 24. Zones primaires standard

Stockées dans `%SystemRoot%\System32\dns\<zone>.dns` (format texte). Un seul serveur inscriptible ; les autres sont secondaires.

```powershell
Add-DnsServerPrimaryZone -Name "dmz.contoso.local" -ZoneFile "dmz.contoso.local.dns" -DynamicUpdate None
```

Limites : fichier local = point de défaillance unique pour l'écriture, transferts de zone à configurer à la main, pas de mises à jour dynamiques sécurisées.

## 25. Zones secondaires

Copie en lecture seule d'une zone primaire, synchronisée par **transfert de zone** (AXFR/IXFR, chapitre 5). Utile pour :
- un DNS en DMZ qui ne doit pas être DC ;
- un site distant sans AD (liaison lente) ;
- un serveur BIND/Linux qui fait office de secondaire.

```powershell
Add-DnsServerSecondaryZone -Name "contoso.local" -ZoneFile "contoso.local.dns" -MasterServers "10.0.0.10"
# Forcer le rechargement depuis le maître
Sync-DnsServerZone -Name "contoso.local" -PassThru
```

Le secondaire **ne répond pas** aux mises à jour dynamiques : il les *refère* vers le primaire (les clients Windows suivent la référence automatiquement).

## 26. Zones stub

Une zone stub ne contient que les enregistrements **NS** de la zone distante (+ SOA + glue A). Le serveur stub interroge ensuite directement les NS distants pour chaque requête : il a toujours l'info à jour sans transfert complet.

```powershell
Add-DnsServerStubZone -Name "partenaire.local" -MasterServers "192.168.50.10" -ZoneFile "partenaire.local.dns"
```

**Stub vs redirecteur conditionnel** (erreur n°14, §136) : le stub suit automatiquement les changements de NS distants (il recharge les NS) ; le redirecteur conditionnel pointe vers des IP fixes. Préférez le stub quand l'autre partie administre ses NS et peut en ajouter.

## 27. Zones de recherche directe vs inversée

- **Directe** (`contoso.local`) : nom → IP (A, AAAA, CNAME, MX, SRV…).
- **Inversée** (`10.in-addr.arpa`, `1.0.10.in-addr.arpa`) : IP → nom (PTR). Indispensable pour : les logs lisibles, certains contrôles anti-spam, `nslookup` inverse, et des applications qui vérifient le reverse (SSH, monitoring).

```powershell
# Zone inversée pour 10.1.0.0/24 (fichier 1.10.in-addr.arpa.dns)
Add-DnsServerPrimaryZone -NetworkID "10.1.0.0/24" -ZoneFile "1.10.in-addr.arpa.dns" -DynamicUpdate Secure
```

**Créez une zone inversée pour chaque sous-réseau interne.** C'est l'oubli le plus fréquent (erreur n°12, §134).

## 28. Zones intégrées à Active Directory

La zone est stockée **dans la base AD** (partition d'application), pas dans un fichier. Chaque DC/DNS qui héberge la zone est inscriptible (multi-maîtres) : les mises à jour dynamiques sont acceptées partout et répliquées par AD.

```powershell
Add-DnsServerPrimaryZone -Name "contoso.local" -ReplicationScope "Forest" -DynamicUpdate "Secure"
```

`-ReplicationScope` : `Forest` (tous les DNS de la forêt), `Domain` (tous les DNS du domaine), `Legacy` (tous les DC, même sans DNS — compatibilité 2000). **Utilisez `Forest` pour `_msdcs` et les zones partagées, `Domain` par défaut sinon.**

## 29. Portées de réplication AD

| Portée | Partition AD | Répliqué vers |
|---|---|---|
| `Forest` | `ForestDnsZones` | Tous les DC/DNS de la **forêt** |
| `Domain` | `DomainDnsZones` | Tous les DC/DNS du **domaine** |
| `Legacy` | Partition de domaine | Tous les **DC** (compat Windows 2000) |
| Personnalisée | Partition d'application dédiée | DC inscrits manuellement (`dnscmd /EnlistDirectoryPartition`) |

```powershell
# Voir la partition d'une zone
Get-DnsServerZone -Name "contoso.local" | Select-Object ZoneName, IsDsIntegrated, DirectoryPartitionName
```

## 30. Avantages et limites des zones AD-integrated

**Avantages :** multi-maîtres (pas de primaire unique), réplication AD chiffrée et compressée (pas d'AXFR en clair), mises à jour dynamiques **sécurisées** (ACL par enregistrement), pas de fichier à sauvegarder séparément (sauvegarde = sauvegarde AD), convergence rapide intra-site.

**Limites :** uniquement entre DC Windows ; un secondaire BIND ne peut pas répliquer depuis AD (il faut un primaire standard ou exposer un transfert depuis un DC) ; la zone n'existe que si AD est sain (un problème de réplication AD = un problème DNS).

## 31. Créer une zone primaire (GUI + PowerShell)

GUI : clic droit **Zones de recherche directe → Nouvelle zone → Zone principale →** cocher **« Stocker la zone dans Active Directory »** → portée de réplication → mises à jour dynamiques.

```powershell
# AD-integrated, mises à jour sécurisées (le standard)
Add-DnsServerPrimaryZone -Name "contoso.local" -ReplicationScope "Domain" -DynamicUpdate "Secure" -PassThru

# Vérification
Get-DnsServerZone -Name "contoso.local" | Format-List ZoneName, ZoneType, IsDsIntegrated, DynamicUpdate, IsAutoCreated
```

`-DynamicUpdate` accepte : `None`, `NonsecureAndSecure`, `Secure`. En AD : toujours `Secure` (voir §78).

## 32. Créer une zone inversée

```powershell
# IPv4 : 10.2.0.0/24
Add-DnsServerPrimaryZone -NetworkID "10.2.0.0/24" -ReplicationScope "Domain" -DynamicUpdate "Secure"

# IPv6 : 2001:db8:10::/64 → la zone ip6.arpa est générée en nibbles inversés
Add-DnsServerPrimaryZone -NetworkID "2001:db8:10::/64" -ReplicationScope "Domain" -DynamicUpdate "Secure"

# Vérifier le nom généré
Get-DnsServerZone | Where-Object { $_.IsReverseLookupZone } | Select-Object ZoneName
```

Convention de nommage : pour `10.2.0.0/24`, la zone est `2.10.in-addr.arpa` (octets inversés). Pour un `/16`, c'est `10.in-addr.arpa`.

## 33. Convertir une zone standard en zone AD-integrated

GUI (méthode recommandée) : Propriétés de la zone → onglet **Général** → bouton **Modifier** à côté de « Type : Principal standard » → cocher **« Stocker la zone dans Active Directory »** → choisir la portée.

En ligne de commande (hérité) :

```powershell
dnscmd SRV-DNS-01 /ZoneChangeDirectoryPartition "contoso.local" /Forest
# Valeurs possibles : /Forest | /Domain | /Legacy
Get-DnsServerZone -Name "contoso.local" | Select-Object ZoneName, IsDsIntegrated
```

⚠️ La conversion inverse (AD → fichier) existe aussi mais fait perdre les mises à jour sécurisées : à éviter sauf besoin explicite (serveur DNS non-DC).

## 34. Suspendre, recharger, supprimer une zone

```powershell
# Suspendre (la zone ne répond plus mais reste configurée)
Suspend-DnsServerZone -Name "old.contoso.local" -PassThru
Resume-DnsServerZone  -Name "old.contoso.local" -PassThru

# Recharger depuis AD ou depuis le fichier
Sync-DnsServerZone -Name "contoso.local"

# Supprimer (irréversible : -Force ne demande pas confirmation)
Remove-DnsServerZone -Name "old.contoso.local" -Force
```

⚠️ `Remove-DnsServerZone` sur une zone AD-integrated la supprime de **tout l'annuaire** (réplication). Vérifiez deux fois le nom.

---

# Chapitre 4 — Les enregistrements de ressources

## 35. Anatomie d'un enregistrement

Un enregistrement = **Nom** + **TTL** + **Classe** (toujours `IN` = Internet) + **Type** + **Données** (+ horodatage si aging actif).

```powershell
# Inspecter un enregistrement en détail
Get-DnsServerResourceRecord -ZoneName "contoso.local" -Name "www" -RRType A |
    Format-List HostName, RecordType, TimeToLive, Timestamp, RecordData
```

- `Timestamp` vide/`0` = enregistrement **statique** (créé à la main, jamais scavengé).
- `Timestamp` renseigné = **dynamique**, soumis au vieillissement (§52).

## 36. A et AAAA

Le cœur du DNS : nom → IPv4 / IPv6.

```powershell
Add-DnsServerResourceRecordA    -Name "www"       -ZoneName "contoso.local" -IPv4Address "10.0.1.50" -TimeToLive 01:00:00
Add-DnsServerResourceRecordAAAA -Name "www"       -ZoneName "contoso.local" -IPv6Address "2001:db8:10::50"
Add-DnsServerResourceRecordA    -Name "intranet"  -ZoneName "contoso.local" -IPv4Address "10.0.1.51","10.0.1.52" -TimeToLive 00:05:00
```

Le 3ᵉ exemple crée **deux A** sur le même nom : round-robin DNS (répartition naïve, sans contrôle de santé — à ne pas confondre avec un vrai load balancer).

**Bonnes pratiques :** un A par usage, nom explicite (`srv-fichiers-01`, pas `serveur2`) ; TTL court (5 min) si l'IP peut changer (bascule), long (1 h+) si stable.

## 37. CNAME : alias et pièges

Un CNAME fait pointer un nom vers un autre nom (pas directement vers une IP).

```powershell
Add-DnsServerResourceRecordCName -Name "intranet" -ZoneName "contoso.local" -HostNameAlias "srv-web-01.contoso.local." -TimeToLive 01:00:00
# Notez le point final : sans lui, le suffixe de zone est ajouté automatiquement
```

**Pièges classiques :**
- Un nom avec CNAME **ne peut avoir aucun autre enregistrement** (pas de MX, pas de TXT, pas de second CNAME). D'où l'interdiction du CNAME au **sommet de zone** (erreur n°5, §127) : l'apex porte déjà SOA et NS.
- Chaînes de CNAME trop longues = résolutions lentes et fragiles. Un seul niveau suffit.
- Pour un alias « nu » d'apex vers l'extérieur, utilisez un A (ou un redirecteur applicatif), pas un CNAME.

## 38. MX : messagerie

Désigne les serveurs de messagerie du domaine, avec **préférence** (plus petit = prioritaire).

```powershell
Add-DnsServerResourceRecordMX -Name "@" -ZoneName "contoso.local" -MailExchange "mx1.contoso.local." -Preference 10 -TimeToLive 01:00:00
Add-DnsServerResourceRecordMX -Name "@" -ZoneName "contoso.local" -MailExchange "mx2.contoso.local." -Preference 20
# "@" = le sommet de zone
```

Chaque `MailExchange` **doit** avoir un A/AAAA (jamais un CNAME, RFC 2181). Vérifiez le reverse (PTR) des MX : beaucoup d'anti-spam le contrôlent.

## 39. SRV : services

Localisation générique d'un service : `_service._proto.nom → cible:port` avec priorité/poids.

```powershell
# Exemple : annuaire LDAP redondant
Add-DnsServerResourceRecordSrv -Name "_ldap._tcp" -ZoneName "contoso.local" `
    -DomainName "srv-ad-01.contoso.local." -Port 389 -Priority 10 -Weight 100
Add-DnsServerResourceRecordSrv -Name "_ldap._tcp" -ZoneName "contoso.local" `
    -DomainName "srv-ad-02.contoso.local." -Port 389 -Priority 10 -Weight 100
Add-DnsServerResourceRecordSrv -Name "_ldap._tcp" -ZoneName "contoso.local" `
    -DomainName "srv-ad-03.contoso.local." -Port 389 -Priority 20 -Weight 0
```

Ici : `srv-ad-01`/`02` se partagent la charge (même priorité, poids égaux), `srv-ad-03` n'est utilisé qu'en secours (priorité 20). C'est exactement le mécanisme qu'AD utilise (§6).

## 40. PTR : résolution inverse

Dans la zone inversée, le **nom** est le dernier octet (ou nibble IPv6), la donnée est le FQDN.

```powershell
# 10.1.0.50 → www.contoso.local (zone 1.10.in-addr.arpa)
Add-DnsServerResourceRecordPtr -Name "50" -ZoneName "1.10.in-addr.arpa" -PtrDomainName "www.contoso.local." -TimeToLive 01:00:00

# Vérification croisée (doit être cohérent dans les deux sens)
Resolve-DnsName -Name "10.1.0.50" -Type PTR
Resolve-DnsName -Name "www.contoso.local" -Type A
```

**Forward-confirmed reverse DNS** : certaines applications exigent que `PTR → nom → A → même IP`. Maintenez les deux zones synchronisées (le DHCP peut le faire, §79).

## 41. TXT : usages

Champ texte libre (255 caractères par chaîne, plusieurs chaînes possibles). Usages en entreprise :

| Usage | Exemple de contenu |
|---|---|
| SPF (anti-spoofing mail) | `v=spf1 mx ip4:10.0.5.25 -all` |
| Vérification de domaine (M365, certificats) | `MS=ms12345678` |
| DKIM/DMARC | `_dmarc.contoso.local` → `v=DMARC1; p=quarantine;` |
| Documentation | `owner=equipe-reseau; ticket=CHG-2026-0912` |

```powershell
Add-DnsServerResourceRecordTXT -Name "@" -ZoneName "contoso.local" -DescriptiveText "v=spf1 mx ip4:10.0.5.25 -all"
```

⚠️ Un TXT « documentation » est visible par **tous** : n'y mettez jamais de secret, mot de passe ou info sensible.

## 42. NS et glue records

Les NS délèguent l'autorité. Chaque zone a des NS ; les **glue records** sont les A/AAAA des NS eux-mêmes quand ils sont *dans* la zone déléguée (sinon, boucle impossible à résoudre).

```powershell
# Ajouter un NS autoritaire à la zone (avec son glue si interne)
Add-DnsServerResourceRecord -Name "@" -NS -ZoneName "contoso.local" -NameServer "SRV-DNS-02.contoso.local."
```

Vérifiez la cohérence : les NS de la zone = les NS chez le parent (délégation) = les serveurs qui répondent vraiment. Un NS « fantôme » (serveur décommissionné resté dans la liste) = timeouts intermittents (cas n°2, §101).

## 43. SOA : le début d'autorité

Un seul SOA par zone, au sommet. Champs :

| Champ | Rôle | Valeur conseillée |
|---|---|---|
| Serial | Version de la zone (déclenche IXFR/AXFR) | `AAAAMMJJNN` (ex : 2026092701) |
| Refresh | Fréquence de vérification du secondaire | 15 min – 2 h |
| Retry | Nouvel essai après échec | 5 – 15 min |
| Expire | Durée max d'utilisation d'une copie périmée | 1 – 4 semaines |
| Minimum (TTL négatif) | Durée de cache des réponses NXDOMAIN | 15 min – 1 h |

```powershell
# Lire le SOA
Get-DnsServerResourceRecord -ZoneName "contoso.local" -RRType SOA | Select-Object -ExpandProperty RecordData
# Modifier le serial (via clone + Set)
$old = Get-DnsServerResourceRecord -ZoneName "contoso.local" -RRType SOA
$new = $old.Clone()
$new.RecordData.SerialNumber = 2026092701
Set-DnsServerResourceRecord -NewInputObject $new -OldInputObject $old -ZoneName "contoso.local"
```

Incrémentez le serial à chaque modification manuelle d'une zone **standard** (les zones AD le font automatiquement).

## 44. CAA : autorité de certification

Un CAA déclare quelle autorité de certification a le droit d'émettre des certificats pour le domaine. Les CA publiques sont tenues de le vérifier.

```powershell
# Seule Let's Encrypt peut émettre pour contoso.local ; aucun wildcard
Add-DnsServerResourceRecord -CAA -Name "@" -ZoneName "contoso.local" -Flags 0 -Tag "issue" -Value "letsencrypt.org"
Add-DnsServerResourceRecord -CAA -Name "@" -ZoneName "contoso.local" -Flags 0 -Tag "issuewild" -Value ";"
```

Tags : `issue` (certificats simples), `issuewild` (wildcards), `iodef` (URL de rapport d'incident). En interne avec une CA d'entreprise, le CAA est moins critique mais reste une bonne hygiène pour les zones exposées.

## 45. Les enregistrements SRV d'AD : liste complète

Référence exhaustive de ce que Netlogon enregistre pour `contoso.local` (zone `_msdcs.contoso.local` + zone `contoso.local`) :

```
# --- Zone _msdcs.contoso.local ---
_ldap._tcp.dc._msdcs               SRV 0 100 389  SRV-DC-01.contoso.local
_ldap._tcp.<Site>._sites.dc._msdcs SRV 0 100 389  SRV-DC-01.contoso.local   (par site)
_kerberos._tcp.dc._msdcs           SRV 0 100 88   SRV-DC-01.contoso.local
_kerberos._tcp.<Site>._sites.dc._msdcs
_kerberos._udp.dc._msdcs           SRV 0 100 88   SRV-DC-01.contoso.local
_kpasswd._tcp / _kpasswd._udp      SRV 0 100 464  SRV-DC-01.contoso.local
_gc._tcp                           SRV 0 100 3268 SRV-DC-01.contoso.local  (si GC)
_gc._tcp.<Site>._sites
_ldap._tcp.pdc._msdcs               SRV 0 100 389  <émulateur PDC>
_ldap._tcp.DomainDnsZones / ForestDnsZones  SRV 0 100 389 ... (DC hébergeant ces partitions)
<GUID>._msdcs                      CNAME SRV-DC-01.contoso.local           (réplication)

# --- Zone contoso.local ---
_ldap._tcp / _kerberos._tcp / _kpasswd._tcp / _gc._tcp   (SRV génériques, sans _msdcs)
contoso.local                      A     10.0.0.10 / 10.0.0.11 ...          (A de chaque DC)
```

```powershell
# Forcer le ré-enregistrement (après un incident)
nltest /dsregdns
# Sur tous les DC du domaine (à exécuter par DC ou via Invoke-Command)
Invoke-Command -ComputerName "SRV-DC-01","SRV-DC-02" -ScriptBlock { nltest /dsregdns }
```

## 46. Créer des enregistrements en PowerShell : synthèse

Tableau récapitulatif des applets (toutes acceptent `-TimeToLive` en `[TimeSpan]` et `-AgeRecord` pour activer le vieillissement sur l'enregistrement) :

| Besoin | Applet |
|---|---|
| A / AAAA | `Add-DnsServerResourceRecordA` / `AAAA` |
| CNAME | `Add-DnsServerResourceRecordCName` |
| MX | `Add-DnsServerResourceRecordMX` |
| SRV | `Add-DnsServerResourceRecordSrv` |
| PTR | `Add-DnsServerResourceRecordPtr` |
| TXT | `Add-DnsServerResourceRecordTXT` |
| NS / SOA / CAA / DS… | `Add-DnsServerResourceRecord -NS/-CAA…` (générique) |

```powershell
# Lister tous les enregistrements d'une zone, triés par type
Get-DnsServerResourceRecord -ZoneName "contoso.local" |
    Sort-Object RecordType, HostName |
    Format-Table HostName, RecordType,
        @{n='TTL';e={$_.TimeToLive.ToString()}},
        @{n='Données';e={$_.RecordData.ToString()}} -AutoSize

# Supprimer proprement (préciser la donnée quand plusieurs RR coexistent)
Remove-DnsServerResourceRecord -ZoneName "contoso.local" -Name "intranet" -RRType A -RecordData "10.0.1.51" -Force
```

---

# Chapitre 5 — Transferts de zone AXFR/IXFR

## 47. AXFR vs IXFR

| | AXFR (Full) | IXFR (Incremental) |
|---|---|---|
| Principe | Copie **complète** de la zone | Seulement les **différences** depuis un serial |
| Déclenchement | Premier transfert, serial trop ancien, demande explicite | Vérification périodique (Refresh SOA) |
| Protocole | TCP 53 | TCP 53 |
| Coût | Lourd (toute la zone) | Léger |

Le secondaire compare son serial SOA à celui du maître : s'il est inférieur, il demande un IXFR ; si le maître ne gère pas l'IXFR (ou si l'écart est trop grand), repli en AXFR. Windows Server gère les deux nativement.

## 48. Configurer le transfert de zone (GUI)

Sur la zone **primaire** : Propriétés → onglet **Transferts de zone** → cocher « Autoriser les transferts de zone » → choisir :
- **Vers tout serveur** : ⚠️ jamais en production (fuite d'information : un AXFR liste toute la zone).
- **Uniquement vers les serveurs listés dans l'onglet Serveurs de noms** : le bon choix courant.
- **Uniquement vers les serveurs suivants** : liste explicite d'IP (le plus strict).

Onglet **Serveurs de noms** : y déclarer chaque secondaire autorisé (avec son glue si besoin).

## 49. Configurer en PowerShell / dnscmd

```powershell
# Autoriser uniquement vers des IP explicites (secondaires 10.0.0.11 et 192.168.99.10)
Set-DnsServerZoneTransfer -Name "contoso.local" -SecondaryServers @("10.0.0.11","192.168.99.10") -PassThru

# Vérifier
Get-DnsServerZoneTransfer -Name "contoso.local" | Format-List *

# Notifier ces serveurs après chaque changement
Set-DnsServerPrimaryZone -Name "contoso.local" -NotifyServers @("10.0.0.11","192.168.99.10")

# Côté secondaire : déclarer le maître et forcer le transfert
Set-DnsServerSecondaryZone -Name "contoso.local" -MasterServers "10.0.0.10"
Sync-DnsServerZone -Name "contoso.local"
```

`dnscmd` équivalent : `dnscmd SRV-DNS-01 /ZoneResetSecondaries contoso.local /SecureList 10.0.0.11 192.168.99.10`.

## 50. Sécuriser les transferts

Checklist de sécurisation :
1. **Liste blanche d'IP** (§49), jamais « tout serveur ».
2. **Pare-feu** : TCP 53 ouvert uniquement vers les secondaires autorisés.
3. **TSIG** : si le secondaire est BIND/Linux, signez les transferts (clé partagée) — côté Windows, la sécurisation repose surtout sur la liste d'IP et le pare-feu.
4. **Zones AD-integrated** : pas de transfert nécessaire entre DC (réplication AD) ; n'activez les transferts que si un secondaire non-DC existe.
5. **Audit** : un AXFR inattendu dans les logs = signal d'alerte (reconnaissance).

```powershell
# Tester qu'un serveur NON autorisé est bien refusé (depuis ce dernier)
# nslookup interactif : ls contoso.local  → doit échouer (refusé)
```

## 51. Notify, SOA serial et cohérence

Le **NOTIFY** (RFC 1996) : quand le primaire change, il *pousse* une notification aux secondaires listés, qui viennent chercher l'IXFR sans attendre le Refresh SOA. Windows l'envoie automatiquement aux serveurs autorisés.

```powershell
# Voir / configurer les serveurs notifiés
Get-DnsServerZone -Name "contoso.local" | Select-Object -ExpandProperty NotifyServers
Set-DnsServerPrimaryZone -Name "contoso.local" -NotifyServers @("10.0.0.11","192.168.99.10")
```

**Cohérence :** après une modification, vérifiez que le serial a augmenté **partout** :
```powershell
"10.0.0.10","10.0.0.11" | ForEach-Object {
    [pscustomobject]@{ Serveur=$_; Serial=(Resolve-DnsName contoso.local -Type SOA -Server $_).SerialNumber }
}
```
Un serial qui ne bouge pas sur le secondaire = transfert en échec (pare-feu, liste blanche, maître injoignable) → cas pratique n°7 (§106).

---

# Chapitre 6 — Vieillissement (aging) et scavenging

## 52. Le problème des enregistrements périmés

Avec les mises à jour dynamiques (DHCP, Netlogon), les enregistrements A/PTR sont créés et rafraîchis automatiquement. Mais quand une machine est **retirée du parc** (mise au rebut, renommée, VM supprimée), son enregistrement reste : la zone se remplit de **noms fantômes** qui pointent vers des IP réattribuées → connexions vers la mauvaise machine, inventaires faux, alertes parasites.

Le **scavenging** est le ramasse-miettes automatique du DNS Windows : il supprime les enregistrements dynamiques périmés. Mal configuré, il supprime aussi des enregistrements **valides** (cas n°5, §104). Ce chapitre est donc à lire deux fois.

## 53. No-refresh et refresh intervals : le mécanisme

Chaque enregistrement dynamique porte un **horodatage** (timestamp). Deux intervalles protègent contre les réplications inutiles :

```
Timeline d'un enregistrement dynamique (ex : no-refresh 7 j, refresh 7 j)
─────────────────────────────────────────────────────────────────────────
J0 : création (timestamp = J0)
J0 ──► J7   : NO-REFRESH — les refresh clients sont IGNORÉS (pas de réplication AD)
J7 ──► J14  : REFRESH — un refresh client met à jour le timestamp (répliqué)
J14+         : périmé — éligible au scavenging (supprimé au prochain cycle)
```

- **No-refresh interval** (défaut 7 jours) : période pendant laquelle un client *ne peut pas* rafraîchir son timestamp → évite une réplication AD à chaque renouvellement DHCP.
- **Refresh interval** (défaut 7 jours) : période pendant laquelle le client *peut* rafraîchir ; passé ce délai sans refresh, l'enregistrement est considéré comme périmé.
- **Scavenging period** (défaut 7 jours, niveau serveur) : fréquence du passage du ramasse-miettes.

**Règle de cohérence :** `no-refresh + refresh` doit être **supérieur** à la durée de bail DHCP typique, sinon des machines éteintes le week-end (bail 8 jours > cycle) se font scavenger à tort. Avec des baux DHCP de 8 jours, les défauts 7+7 jours sont corrects.

## 54. Activer le vieillissement sur une zone

Le vieillissement s'active en **deux endroits** : sur chaque zone, ET au niveau serveur (§55). L'un sans l'autre = rien ne se passe.

```powershell
# 1. Activer l'aging sur la zone (avec les intervalles)
Set-DnsServerZoneAging -Name "contoso.local" -Aging $true -RefreshInterval 7.00:00:00 -NoRefreshInterval 7.00:00:00 -PassThru
Set-DnsServerZoneAging -Name "1.10.in-addr.arpa" -Aging $true -RefreshInterval 7.00:00:00 -NoRefreshInterval 7.00:00:00

# 2. Vérifier
Get-DnsServerZoneAging -Name "contoso.local" | Format-List *

# 3. Appliquer l'horodatage aux enregistrements EXISTANTS (sinon ils restent "statiques" pour toujours)
dnscmd SRV-DNS-01 /AgeAllRecords contoso.local
```

⚠️ `/AgeAllRecords` : à n'exécuter qu'**une fois**, après avoir vérifié que les enregistrements existants sont légitimes. Il rend éligibles au scavenging des enregistrements qui ne l'étaient pas.

GUI : Propriétés de la zone → onglet **Général** → bouton **Vieillissement** → cocher « Éliminer les enregistrements de ressources périmés ».

## 55. Activer le scavenging au niveau serveur

```powershell
# Activer le scavenging serveur (cycle de 7 jours, valeurs par défaut 7j/7j)
Set-DnsServerScavenging -ScavengingState $true -ScavengingInterval 7.00:00:00 `
    -RefreshInterval 7.00:00:00 -NoRefreshInterval 7.00:00:00 -PassThru

# Vérifier
Get-DnsServerScavenging | Format-List *

# Forcer un passage manuel (avec -Force, sans confirmation)
Start-DnsServerScavenging -Force
```

GUI : Propriétés du serveur → onglet **Avancé** → cocher « Activer le nettoyage automatique des enregistrements périmés » + « Période de nettoyage ».

⚠️ **N'activez le scavenging serveur que sur UN SEUL serveur DNS** (le « scavengeur » désigné, typiquement le premier DC). Si plusieurs serveurs scavengent, ils se marchent dessus et les logs deviennent illisibles. Documentez lequel dans votre dossier d'exploitation.

## 56. Bonnes pratiques et pièges du scavenging

**Bonnes pratiques :**
1. Activez l'aging sur **toutes** les zones dynamiques (directes + inversées), pas seulement `contoso.local`.
2. `no-refresh + refresh` > durée de bail DHCP (ex : baux 8 j → 7+7 j OK ; baux 30 j → passez à 14+14 j).
3. Scavenging serveur sur **un seul** serveur ; cycle ≥ 7 jours.
4. Avant la première activation : inventaire des enregistrements statiques critiques (imprimantes, équipements réseau en IP fixe enregistrés à la main) → vérifiez qu'ils sont bien **statiques** (timestamp à 0) ou protégés.
5. Surveillez l'Event ID **2501** (nombre d'enregistrements scavengés) après chaque cycle les premiers mois.

**Pièges mortels :**
- Scavenging activé avec des baux DHCP longs (ex : 30 jours) et des intervalles 7+7 : des machines en congés se font supprimer → cas n°5 (§104).
- `/AgeAllRecords` lancé sans inventaire : des enregistrements créés à la main deviennent éligibles et disparaissent 14 jours plus tard.
- Enregistrements d'équipements réseau (imprimantes, onduleurs) créés par DHCP puis passés en IP fixe sans passer en statique : le DHCP ne les rafraîchit plus → scavengés.

---

# Chapitre 7 — Redirecteurs et indications de racine

## 57. Redirecteurs : principe

Un **redirecteur** (forwarder) est un serveur DNS vers lequel votre serveur **transfère** les requêtes récursives qu'il ne peut pas résoudre lui-même, au lieu d'interroger la racine. Typiquement : les DNS du FAI, `1.1.1.1`, `9.9.9.9`, ou le DNS central du groupe.

```
Client → SRV-DNS-01 (ne sait pas) → Redirecteur 1.1.1.1 → Internet → réponse → cache → client
```

Avantages : **cache partagé** (le redirecteur a souvent déjà la réponse → plus rapide), **un seul point de sortie** à autoriser au pare-feu, possibilité de filtrage (DNS filtrant type Quad9).

## 58. Indications de racine : principe

Les **indications de racine** (root hints) sont la liste des 13 serveurs racine (`a.root-servers.net` → `m.root-servers.net`, fichier `cache.dns`). Sans redirecteur, votre serveur fait lui-même la résolution **itérative** complète depuis la racine.

```
Client → SRV-DNS-01 → . (racine) → serveur du TLD → serveur autoritaire → réponse
```

Avantages : **aucune dépendance tierce** (pas de FAI, pas de Cloudflare), pas de logging de vos requêtes chez un tiers, conforme à une posture « zéro confiance externe ». Inconvénient : première résolution plus lente, tout le trafic DNS sortant (UDP/TCP 53 vers Internet) doit être autorisé.

## 59. Redirecteurs vs racine : quand utiliser quoi

| Critère | Redirecteurs | Indications de racine |
|---|---|---|
| Performance (cache chaud) | ✅ Meilleure | Moyenne |
| Indépendance / confidentialité | ❌ Dépend d'un tiers | ✅ Aucun tiers |
| Pare-feu sortant | Simple (quelques IP) | Large (tout l'Internet en 53) |
| Filtrage (malware, parental) | ✅ Possible (Quad9, OpenDNS) | ❌ Non |
| Robustesse si le FAI tombe | ❌ Si redirecteur = FAI | ✅ |
| Recommandé quand | Poste client standard, besoin de filtrage | Serveur exposé, contrainte de souveraineté |

**Recommandation entreprise :** redirecteurs vers **2 résolveurs fiables et rapides** (ex : `1.1.1.1` + `9.9.9.9`, ou les DNS de votre opérateur si SLA), avec **repli sur la racine désactivé ou activé selon votre politique** (case « Utiliser les indications de racine si aucun redirecteur n'est disponible » : à cocher = résilience, à décocher = contrôle strict du chemin).

## 60. Configurer les redirecteurs

```powershell
# Définir les redirecteurs (remplace la liste) et désactiver le repli racine
Set-DnsServerForwarder -IPAddress @("1.1.1.1","9.9.9.9") -UseRootHint $false -Timeout 3 -PassThru

# Ajouter un redirecteur sans écraser
Add-DnsServerForwarder -IPAddress "8.8.8.8" -PassThru

# Vérifier + tester chaque redirecteur
Get-DnsServerForwarder | Select-Object -ExpandProperty IPAddress
Test-DnsServer -IPAddress "1.1.1.1" -Context "Forwarder"
```

GUI : Propriétés du serveur → onglet **Redirecteurs** → Modifier. Le bouton **« Utiliser les indications de racine si aucun redirecteur n'est disponible »** correspond à `-UseRootHint`.

⚠️ **Timeout** : 3 secondes par défaut. Avec des redirecteurs lents, les clients subissent des latences en cascade → cas n°8 (§107).

## 61. Redirecteurs conditionnels

Un redirecteur conditionnel ne s'applique qu'à **une zone précise** : « pour `partenaire.local`, interroge toujours ces serveurs-là ».

```powershell
# Tout ce qui concerne partenaire.local va vers leurs DNS
Add-DnsServerConditionalForwarderZone -Name "partenaire.local" -MasterServers @("192.168.50.10","192.168.50.11") -PassThru

# Lister / modifier / supprimer
Get-DnsServerZone -Name "partenaire.local"
Set-DnsServerConditionalForwarderZone -Name "partenaire.local" -MasterServers @("192.168.50.12")
Remove-DnsServerZone -Name "partenaire.local" -Force
```

Cas d'usage rois : interconnexions VPN avec un partenaire, forêt AD distante sans approbation de forêt, zone d'un hébergeur cloud privé. **Priorité** : le redirecteur conditionnel est consulté *avant* les redirecteurs généraux.

## 62. Chaînage et boucles : à éviter

**Boucle de redirecteurs** : A redirige vers B, B redirige vers A → timeouts, logs saturés. Ça arrive lors de fusions d'entreprises (chaque partie pointe vers l'autre « pour résoudre ses noms »).

**Règles anti-boucle :**
1. Documentez la **chaîne complète** de résolution (qui redirige vers qui) dans le dossier d'exploitation.
2. Un redirecteur doit toujours être « plus proche de la réponse » que vous (résolveur Internet, ou DNS autoritaire de la zone cible).
3. Testez avec `Resolve-DnsName -Trace` : une boucle se voit (requêtes qui tournent).
4. Méfiez-vous des redirecteurs conditionnels croisés entre deux forêts : préférez une **approbation de forêt + DNS intégré**, ou des zones stub.

```powershell
# Détecter une boucle : la trace montre des allers-retours entre les mêmes IP
Resolve-DnsName -Name "srv.partenaire.local" -Trace | Select-Object -Last 20
```

---

# Chapitre 8 — Split-brain DNS et stratégies DNS

## 63. Le problème du split-brain

Le **split-brain** (ou DNS à « double visage ») : le même nom de zone (`contoso.com`) doit résoudre **différemment** selon qu'on est dedans ou dehors.
Exemple : `www.contoso.com` = `203.0.113.10` (IP publique, site web) pour Internet, mais `10.0.1.50` (IP interne) pour les postes du LAN — parce que le trafic interne ne doit pas faire l'aller-retour par le pare-feu (hairpinning), ou parce que l'appli n'est pas exposée.

Sans split-brain géré, les postes internes résolvent l'IP publique → latence, pare-feu qui bloque le hairpin, ou pire : injoignable.

## 64. Solutions classiques au split-brain

| Solution | Principe | Avantages / limites |
|---|---|---|
| **Zone interne dédiée** | Créer `contoso.com` en zone AD-integrated avec *uniquement* les enregistrements internes nécessaires | Simple, robuste. ⚠️ Il faut dupliquer chaque enregistrement public utilisé en interne (oubli = panne). |
| **Redirecteur conditionnel** | `contoso.com` → DNS publics de l'entreprise pour l'externe | Ne permet pas de différencier interne/externe : tout le monde a la même vue. |
| **Sous-zone** | `interne.contoso.com` en interne, le reste via redirecteurs | Propre si on peut imposer un nommage distinct. |
| **Stratégies DNS (2016+)** | Même zone, **vues différentes** selon le sous-réseau client | Le plus élégant : une seule zone à maintenir (§65). |

**Règle :** si votre nom de domaine AD est déjà un nom public (ex : forêt `contoso.com` au lieu de `contoso.local`), le split-brain n'est pas une option, c'est une obligation.

## 65. Split-brain avec les stratégies DNS (zone scopes)

Depuis Windows Server 2016, une zone peut avoir plusieurs **scopes** (vues) : chaque scope contient ses propres enregistrements, et une **stratégie de résolution** choisit le scope selon le client (sous-réseau, heure, etc.).

```powershell
# 1. Déclarer les sous-réseaux clients
Add-DnsServerClientSubnet -Name "Subnet-LAN" -IPv4Subnet @("10.0.0.0/8")
Add-DnsServerClientSubnet -Name "Subnet-VPN" -IPv4Subnet @("172.16.0.0/16")

# 2. Créer les scopes dans la zone (le scope par défaut "." existe déjà)
Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "Scope-Interne"
Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "Scope-VPN"

# 3. Enregistrements différenciés par scope
Add-DnsServerResourceRecordA -Name "www" -ZoneName "contoso.com" -IPv4Address "10.0.1.50" -ZoneScope "Scope-Interne"
Add-DnsServerResourceRecordA -Name "www" -ZoneName "contoso.com" -IPv4Address "172.16.0.50" -ZoneScope "Scope-VPN"
Add-DnsServerResourceRecordA -Name "www" -ZoneName "contoso.com" -IPv4Address "203.0.113.10"   # scope par défaut = externe

# 4. Stratégies : quel client voit quel scope (le chiffre = ordre d'évaluation)
Add-DnsServerQueryResolutionPolicy -Name "Policy-Interne" -Action ALLOW -ClientSubnet "EQ,Subnet-LAN" -ZoneName "contoso.com" -ZoneScope "Scope-Interne,1"
Add-DnsServerQueryResolutionPolicy -Name "Policy-VPN"     -Action ALLOW -ClientSubnet "EQ,Subnet-VPN" -ZoneName "contoso.com" -ZoneScope "Scope-VPN,1"

# 5. Vérifier
Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" | Format-Table Name, Action, ProcessingOrder
Get-DnsServerResourceRecord -ZoneName "contoso.com" -Name "www" -RRType A
```

⚠️ Version : stratégies DNS = **Windows Server 2016 et +** (2019/2022/2025 OK). Les scopes ne se répliquent que si la zone est AD-integrated.

## 66. Stratégies DNS : client subnets

Le **sous-réseau client** est déterminé par l'IP source de la requête (ou par l'info EDNS Client Subnet si elle traverse un redirecteur qui la transmet — rare en interne, à ne pas supposer).

```powershell
# Lister / affiner les subnets
Get-DnsServerClientSubnet | Format-Table Name, IPv4Subnet
# Un subnet peut combiner plusieurs plages
Set-DnsServerClientSubnet -Name "Subnet-LAN" -IPv4Subnet @("10.0.0.0/8","192.168.0.0/16")
```

**Piège :** un client dont l'IP n'est dans aucun subnet tombe sur le **scope par défaut**. En split-brain, ça veut dire « vue externe » : vérifiez que c'est bien le comportement voulu (ou ajoutez une stratégie de rattrapage explicite).

## 67. Stratégies DNS : exemples concrets

```powershell
# Exemple 1 : Géo-DNS interne — Lyon voit le serveur de fichiers de Lyon
Add-DnsServerClientSubnet -Name "Subnet-Lyon" -IPv4Subnet "10.2.0.0/16"
Add-DnsServerZoneScope -ZoneName "contoso.local" -Name "Scope-Lyon"
Add-DnsServerResourceRecordA -Name "fichiers" -ZoneName "contoso.local" -IPv4Address "10.2.1.20" -ZoneScope "Scope-Lyon"
Add-DnsServerResourceRecordA -Name "fichiers" -ZoneName "contoso.local" -IPv4Address "10.1.1.20"  # défaut = Paris
Add-DnsServerQueryResolutionPolicy -Name "Policy-Lyon" -Action ALLOW -ClientSubnet "EQ,Subnet-Lyon" -ZoneName "contoso.local" -ZoneScope "Scope-Lyon,1"

# Exemple 2 : politique horaire — la nuit, le portail de maintenance répond "en maintenance"
Add-DnsServerQueryResolutionPolicy -Name "Policy-Nuit" -Action ALLOW -TimeOfDay "EQ,22:00-06:00" `
    -ZoneName "contoso.com" -ZoneScope "Scope-Maintenance,1" -PassThru

# Exemple 3 : bloquer (DENY) la résolution d'une zone pour un subnet invité
Add-DnsServerQueryResolutionPolicy -Name "Policy-Invite-Deny" -Action DENY `
    -ClientSubnet "EQ,Subnet-Invite" -FQDN "EQ,*.interne.contoso.local" -PassThru
```

## 68. Bonnes pratiques split-brain

1. **Documentez la matrice** : pour chaque nom critique, qui voit quoi (tableau LAN / VPN / Externe).
2. **Ne dupliquez que le nécessaire** : chaque enregistrement public utilisé en interne doit exister dans la vue interne (le classique oublié : `autodiscover`, `vpn`, les enregistrements de vérification).
3. **Testez les 3 vues** après chaque changement (depuis une vraie machine de chaque subnet).
4. Préférez les **stratégies DNS** à la duplication manuelle de zones : une seule zone = un seul endroit à maintenir.
5. Surveillez la **divergence** : un script hebdo qui compare les scopes (export + diff).

---

# Chapitre 9 — DNSSEC

## 69. DNSSEC : pourquoi

Le DNS classique ne garantit **ni l'authenticité ni l'intégrité** : une réponse peut être forgée en transit (cache poisoning, homme-du-milieu). **DNSSEC** ajoute des signatures cryptographiques aux enregistrements : le résolveur peut *vérifier* que la réponse vient bien du propriétaire de la zone et n'a pas été altérée.

Ce que DNSSEC **fait** : authentifie l'origine des données, prouve l'inexistence (NSEC/NSEC3).
Ce que DNSSEC **ne fait pas** : ne chiffre pas (les requêtes restent lisibles — pour la confidentialité, voir DoT/DoH), ne garantit pas la disponibilité.

En entreprise : signez au minimum vos **zones exposées** (publiques) ; en interne, DNSSEC a du sens si votre menace inclut un attaquant sur le réseau (VLAN invité, Wi-Fi).

## 70. Chaîne de confiance : KSK, ZSK, DS

```
Racine (.) signée → DS du TLD → TLD signé → DS de contoso.com → contoso.com signé
```

- **KSK** (Key Signing Key, longue, ex : 2048 bits) : signe uniquement les clés. Sa *empreinte* (DS) est publiée chez le **parent**.
- **ZSK** (Zone Signing Key, plus courte, ex : 1280 bits) : signe les enregistrements de la zone. Rollover fréquent.
- **DS** (Delegation Signer) : l'enregistrement chez le parent qui ancre la confiance vers votre KSK.
- **DNSKEY / RRSIG / NSEC(3)** : les enregistrements techniques de la signature, générés automatiquement.

Le validateur part d'une **ancre de confiance** (trust anchor, ex : la clé de la racine, préinstallée) et descend la chaîne.

## 71. Signer une zone sur Windows Server

Méthode GUI (recommandée pour la première fois) : clic droit sur la zone → **DNSSEC → Signer la zone** → assistant (choix KSK/ZSK, algorithmes, NSEC3). Le serveur génère les clés, signe la zone, et gère le rollover.

En PowerShell :

```powershell
# 1. Ajouter les clés de signature (KSK 2048 bits + ZSK)
Add-DnsServerSigningKey -ZoneName "contoso.com" -CryptoAlgorithm RsaSha256 -KeyType KeySigningKey -KeyLength 2048
Add-DnsServerSigningKey -ZoneName "contoso.com" -CryptoAlgorithm RsaSha256 -KeyType ZoneSigningKey -KeyLength 1280

# 2. Paramétrer la signature (NSEC3 contre l'énumération de zone)
Set-DnsServerDnsSecZoneSetting -ZoneName "contoso.com" -SignWithNSEC3 $true -NSEC3HashAlgorithm SHA1 -NSEC3Iterations 10

# 3. Vérifier l'état de signature
Get-DnsServerDnsSecZoneSetting -ZoneName "contoso.com" | Format-List ZoneName, IsSigned, *
Get-DnsServerSigningKey -ZoneName "contoso.com" | Format-Table KeyId, KeyType, CryptoAlgorithm, IsActive
```

⚠️ **Prérequis** : zone **AD-integrated** (requis par Windows pour la signature), horloges synchronisées (la validation échoue si l'horloge dérive — les signatures ont une période de validité).

## 72. Gestion des clés et rollover

Le rollover (renouvellement) est le moment dangereux : une clé qui change sans coordination = **SERVFAIL** pour tous les validateurs.

| Clé | Rollover conseillé | Mécanisme Windows |
|---|---|---|
| ZSK | Tous les 90 jours | Automatique (pré-publication) |
| KSK | Tous les 1–2 ans | **Manuel** : générez la nouvelle KSK, publiez le nouveau **DS chez le parent/registrar**, attendez 2× TTL du DS, puis activez |

```powershell
# Rollover manuel de la KSK (après publication du DS chez le registrar)
Add-DnsServerSigningKey -ZoneName "contoso.com" -KeyType KeySigningKey -CryptoAlgorithm RsaSha256 -KeyLength 2048
# ... attendre la propagation du DS ...
# Puis retirer l'ancienne :
Remove-DnsServerSigningKey -ZoneName "contoso.com" -KeyId "<ancien KeyId>" -Force
```

**Checklist rollover KSK :** nouveau DS publié chez le registrar → `dig DS contoso.com` OK depuis l'extérieur → attendre 48 h → activer → surveiller les SERVFAIL → supprimer l'ancienne clé.

## 73. Ancres de confiance (trust anchors)

Côté **validateur** (votre résolveur interne qui vérifie les signatures), il faut les clés publiques de départ :

```powershell
# Importer l'ancre de la racine (pré-distribuée par Windows, à vérifier)
Get-DnsServerTrustAnchor -Name "." | Format-Table Name, TrustAnchorType

# Ajouter manuellement une ancre (ex : zone partenaire signée, en DS)
Add-DnsServerTrustAnchor -Name "partenaire.local" -Type DS -KeyTag 12345 `
    -CryptoAlgorithm RsaSha256 -DigestType Sha256 -Digest "ABCDEF0123456789..."

# Lister toutes les ancres
Get-DnsServerTrustAnchor | Format-Table Name, TrustAnchorType
```

Sans ancre pour une zone signée, le validateur la considère comme **non sécurisée** (pas d'erreur, mais pas de protection non plus) — sauf si vous forcez la validation.

## 74. Validation DNSSEC côté résolveur

Activer la validation sur vos résolveurs internes : toute réponse d'une zone signée est vérifiée ; signature invalide → **SERVFAIL** (plutôt qu'une fausse réponse).

```powershell
# GUI : Propriétés du serveur → onglet Avancé → "Activer la validation DNSSEC pour les réponses distantes"
# Vérifier l'état :
Get-DnsServerRecursion | Select-Object Enable, EnableDnsSecValidation
```

Testez :
```powershell
# Doit réussir (zone signée valide)
Resolve-DnsName -Name "dnssec-tools.org" -Type A -DnssecOk -Server "10.0.0.10"
# Test de validation négative (zone volontairement mal signée)
Resolve-DnsName -Name "www.dnssec-failed.org" -Type A -DnssecOk -Server "10.0.0.10"  # → SERVFAIL attendu
```

⚠️ Avant d'activer la validation en production : vérifiez que vos **redirecteurs** valident ou au moins transmettent les bits DNSSEC (sinon, vos validations échouent à cause du redirecteur, pas de la zone).

## 75. NSEC vs NSEC3

Pour prouver la **non-existence** d'un nom (réponse NXDOMAIN signée), deux mécanismes :

| | NSEC | NSEC3 |
|---|---|---|
| Principe | Chaîne les noms existants en clair | Chaîne des **hachés** des noms |
| Risque | Permet l'**énumération de zone** (zone walking : lister tous les noms) | Protège contre l'énumération (haché + sel + itérations) |
| Coût | Léger | Un peu plus lourd (calculs) |

**Recommandation : NSEC3** systématiquement pour les zones d'entreprise (vos noms de serveurs internes sont de l'information). Windows le propose dans l'assistant de signature et via `-SignWithNSEC3 $true` (§71).

## 76. Limites et points d'attention DNSSEC

1. **Complexité opérationnelle** : rollover KSK, DS chez le registrar, horloges → prévoyez des runbooks.
2. **Taille des réponses** : les réponses signées sont grosses → bascule UDP→TCP fréquente → ouvrez **TCP 53** partout (pare-feu !).
3. **Dernier kilomètre non protégé** : entre le poste client et votre résolveur, pas de DNSSEC (le bit DO est positionné par le résolveur). Pour chiffrer ce segment : DNS sur TLS/HTTPS (voir §149).
4. **Dépannage** : un SERVFAIL DNSSEC ressemble à une panne DNS classique → ayez le réflexe `Resolve-DnsName -DnssecOk` et vérifiez la chaîne (cas n°11, §110).
5. **Zones AD internes** : la signature des zones internes est possible mais rarement prioritaire — le risque principal en interne est ailleurs (cf. chapitre 13).

---

# Chapitre 10 — DNS, Active Directory et DHCP

## 77. Mises à jour dynamiques : principe

Au lieu de créer chaque enregistrement à la main, les machines **s'enregistrent elles-mêmes** (RFC 2136) : au démarrage, à chaque renouvellement DHCP, toutes les 24 h. Le client envoie un UPDATE signé (Kerberos en mode sécurisé) au serveur DNS autoritaire.

```powershell
# Voir le mode de mise à jour dynamique d'une zone
Get-DnsServerZone -Name "contoso.local" | Select-Object ZoneName, DynamicUpdate
# Valeurs : None | NonsecureAndSecure | Secure
```

## 78. Secure vs non-secure vs none

| Mode | Authentification | Usage |
|---|---|---|
| `None` | — | Zones statiques (DMZ exposée, zone publique) |
| `NonsecureAndSecure` | Optionnelle | ⚠️ À éviter : n'importe qui peut écraser n'importe quel enregistrement |
| `Secure` | Kerberos obligatoire, ACL par enregistrement | **Standard AD** : seul le propriétaire (compte machine) peut modifier son enregistrement |

**Règle :** toute zone AD-integrated = `Secure`. Le mode non-sécurisé n'a de sens que pour des zones hébergeant des équipements incapables de Kerberos **et** isolées — et encore, préférez des enregistrements statiques.

## 79. DNS et DHCP : qui enregistre quoi

Deux modèles (propriétés du serveur DHCP → onglet **DNS**) :

**Modèle A — Le client s'enregistre lui-même** (défaut Windows) : le client DHCP met à jour son **A** (nom → IP) ; le serveur DHCP met à jour le **PTR** (IP → nom). Simple, pas de compte dédié.

**Modèle B — Le DHCP enregistre pour le client** (« Toujours mettre à jour dynamiquement… » coché) : le DHCP fait A + PTR pour tous les clients, y compris non-Windows (imprimantes, téléphones, Linux sans nsupdate). **Requiert un compte dédié** (§80).

| Client | A | PTR |
|---|---|---|
| Windows (modèle A) | Le client | Le DHCP |
| Non-Windows / modèle B | Le DHCP (avec credentials) | Le DHCP |

```powershell
# Voir la config DNS d'une étendue DHCP (module DhcpServer)
Get-DhcpServerv4DnsSetting -ComputerName "SRV-DHCP-01"
```

## 80. Configurer les credentials DHCP

Sans compte dédié, le DHCP enregistre avec son **compte machine** : si plusieurs serveurs DHCP coexistent (ou après une réinstallation), les enregistrements deviennent orphelins (le nouveau serveur n'a pas le droit de les écraser). La bonne pratique : un **compte de service dédié**, mot de passe géré, membre du groupe **DnsUpdateProxy**.

```powershell
# 1. Créer le compte de service (adaptez selon votre nommage)
New-ADUser -Name "svc-dhcp-dns" -SamAccountName "svc-dhcp-dns" -AccountPassword (Read-Host -AsSecureString) `
    -Enabled $true -PasswordNeverExpires $true -Description "Mises à jour DNS pour DHCP"

# 2. L'ajouter au groupe DnsUpdateProxy du domaine
Add-ADGroupMember -Identity "DnsUpdateProxy" -Members "svc-dhcp-dns"

# 3. Déclarer les credentials sur chaque serveur DHCP (GUI : propriétés IPv4 → onglet Avancé → Informations d'identification)
Set-DhcpServerDnsCredential -Credential (Get-Credential "CONTOSO\svc-dhcp-dns") -ComputerName "SRV-DHCP-01"

# 4. Vérifier
Get-DhcpServerDnsCredential -ComputerName "SRV-DHCP-01"
```

⚠️ Le compte doit avoir un mot de passe **qui n'expire pas** (ou géré via gMSA — voir §149) sinon les mises à jour s'arrêtent le jour de l'expiration.

## 81. Le groupe DnsUpdateProxy : utilité et danger

**Utilité :** les membres de `DnsUpdateProxy` peuvent créer des enregistrements **sans en devenir propriétaires** : n'importe quel serveur DHCP du groupe peut ensuite les mettre à jour. Sans ça, le premier DHCP qui enregistre une imprimante verrouille l'enregistrement (ACL) et les autres ne peuvent plus le toucher.

**Danger :** les enregistrements créés par un membre de DnsUpdateProxy sont **non sécurisés** (pas d'ACL propriétaire). Conséquences :
- **Ne mettez JAMAIS un contrôleur de domaine dans DnsUpdateProxy** (erreur n°17, §139) : ses enregistrements deviendraient modifiables par tous.
- Limitez le groupe au strict nécessaire (comptes DHCP dédiés).
- Combinez avec le scavenging (§56) : les enregistrements non rafraîchis seront nettoyés.

## 82. Protéger les enregistrements existants

Quand vous basculez un DHCP vers le modèle B (ou changez de compte), les **anciens** enregistrements appartiennent à l'ancien propriétaire → le nouveau compte ne peut pas les mettre à jour → doublons et conflits.

Procédure de bascule propre :
1. Dans la console DNS, affichez la colonne **Horodatage**.
2. Pour les enregistrements critiques (serveurs en DHCP — à éviter, mais ça existe), passez-les en **statiques** : décochez « Supprimer cet enregistrement lorsqu'il devient périmé » — ou recréez-les à la main.
3. Option radicale mais propre sur une zone de test : `dnscmd /AgeAllRecords` puis laissez le scavenging nettoyer, **après** sauvegarde (§143).

## 83. Scavenging et DHCP : le duo gagnant

Le cycle vertueux : bail DHCP 8 jours → no-refresh 7 j + refresh 7 j → scavenging 7 j (§53). Le DHCP rafraîchit les enregistrements à chaque renouvellement de bail ; le scavenging supprime ceux des machines parties.

**Point de vigilance :** les équipements à bail **long ou réservation** (imprimantes, onduleurs, caméras) : si leur enregistrement est dynamique et que l'équipement ne rafraîchit pas (firmware basique), il sera scavengé. Pour ces équipements : **réservation DHCP + enregistrement statique** (ou modèle B avec credentials, et vérifiez le rafraîchissement).

## 84. Cas des clients non-Windows et statiques

- **Linux/macOS** : peuvent s'enregistrer via `nsupdate` + Kerberos (krb5) — en pratique, laissez le DHCP faire le travail (modèle B).
- **Équipements réseau** (imprimantes, onduleurs, switchs) : enregistrements **statiques** créés à la main, documentés dans l'inventaire. Pas de scavenging dessus (timestamp à 0).
- **Serveurs** : **IP fixe + A statique**, jamais de DHCP (sauf réservation + documentation). Un serveur dont l'IP change au gré du DHCP est une panne en puissance.

```powershell
# Créer un enregistrement statique (pas de timestamp → jamais scavengé)
Add-DnsServerResourceRecordA -Name "prt-compta-01" -ZoneName "contoso.local" -IPv4Address "10.1.5.30" -TimeToLive 01:00:00
# Vérifier qu'il est statique : Timestamp doit être vide
Get-DnsServerResourceRecord -ZoneName "contoso.local" -Name "prt-compta-01" | Select-Object HostName, Timestamp
```

---

# Chapitre 11 — Haute disponibilité

## 85. Plusieurs serveurs DNS : architecture type

Minimum viable en entreprise : **2 serveurs DNS** par site important, sur **2 hyperviseurs / 2 baies** distincts si virtualisés.

```
Site Paris-Siege :  SRV-DNS-01 (10.0.0.10)  +  SRV-DNS-02 (10.0.0.11)
Site Lyon-Usine  :  SRV-DNS-03 (10.2.0.10)  +  SRV-DNS-04 (10.2.0.11)
```

- Les clients ont **2 DNS** (DHCP options 006, ou GPO) : le client interroge le second si le premier ne répond pas (timeout ~1-2 s, variable selon l'OS).
- Chaque DC est DNS (zones AD-integrated) : la redondance DNS suit la redondance AD.
- ⚠️ Les clients n'équilibrent **pas** la charge entre leurs 2 DNS : ils utilisent le premier tant qu'il répond. Pour répartir, variez l'ordre via DHCP par site.

## 86. AD-integrated : la HA naturelle

Avec des zones AD-integrated, **chaque DC/DNS est un primaire** : pas de maître unique, pas de transfert à configurer, écriture possible partout. La réplication AD (15 s intra-site par défaut, compressée, chiffrée) propage les changements.

```powershell
# Vérifier que la zone est bien présente et saine sur chaque DC/DNS
$serveurs = "SRV-DNS-01","SRV-DNS-02","SRV-DNS-03"
foreach ($s in $serveurs) {
    Get-DnsServerZone -Name "contoso.local" -ComputerName $s |
        Select-Object @{n='Serveur';e={$s}}, ZoneName, ZoneType, IsDsIntegrated
}
```

**Limite assumée :** si AD est en panne (réplication cassée), le DNS multi-maîtres diverge. C'est le prix de l'intégration — d'où l'importance de superviser la réplication AD (`repadmin /showrepl`, `dcdiag /test:dns`).

## 87. Secondaires et transferts pour la HA

Quand un DNS **n'est pas** DC (DMZ, site sans AD, appliance), utilisez des **zones secondaires** (§25) : le primaire notifie (§51), le secondaire transfère.

```powershell
# Sur le secondaire de DMZ : vérifier la fraîcheur (serial identique au primaire)
$primaire   = (Resolve-DnsName "dmz.contoso.local" -Type SOA -Server "10.0.0.10").SerialNumber
$secondaire = (Resolve-DnsName "dmz.contoso.local" -Type SOA -Server "192.168.99.10").SerialNumber
if ($primaire -ne $secondaire) { Write-Warning "DIVERGENCE : primaire=$primaire secondaire=$secondaire" }
```

**Expire SOA** (§43) : sur un secondaire isolé longtemps, la zone expire → le serveur **cesse de répondre** pour cette zone. Réglez l'Expire à 2–4 semaines, pas moins.

## 88. Anycast DNS : principe

L'**anycast** : plusieurs serveurs DNS partagent la **même IP**, annoncée via BGP depuis plusieurs sites ; le routage envoie chaque client au plus proche. Si un site tombe, le trafic bascule automatiquement (convergence BGP, quelques secondes à minutes).

- En entreprise, c'est pertinent pour des **résolveurs centraux** multi-sites ou des zones publiques critiques.
- Windows Server ne fait pas de BGP natif : l'anycast se met en place avec des routeurs/pare-feu en frontal (ou un ADC). Mentionné ici pour que le terme ne soit pas un mystère en réunion d'architecture.
- Alternative simple sans BGP : **2 IP virtuelles** (une par site) + bascule manuelle documentée.

## 89. Supervision de la disponibilité DNS

Ce qu'il faut monitorer (Zabbix, SCOM, PRTG, ou script maison) :

| Sonde | Seuil d'alerte | Commande |
|---|---|---|
| Port 53 TCP/UDP répond | Down > 2 min | `Test-NetConnection -ComputerName 10.0.0.10 -Port 53` |
| Résolution d'un témoin interne | Échec | `Resolve-DnsName temoin.contoso.local -Server 10.0.0.10` |
| Résolution récursive (Internet) | Échec | `Resolve-DnsName www.example.com -Server 10.0.0.10` |
| Temps de réponse | > 500 ms répété | `Measure-Command { Resolve-DnsName … }` |
| Serial SOA cohérent entre serveurs | Divergence > 1 h | Script §87 |
| Scavenging : Event 2501 | Compte anormal | Collecte des événements DNS |

```powershell
# Sonde complète minimale (à planifier toutes les 5 min)
$dns = "10.0.0.10"
$tests = @(
    @{ Nom="Témoin interne";  OK = [bool](Resolve-DnsName "temoin.contoso.local" -Server $dns -ErrorAction SilentlyContinue) },
    @{ Nom="Récursion";       OK = [bool](Resolve-DnsName "www.example.com"     -Server $dns -ErrorAction SilentlyContinue) },
    @{ Nom="SRV AD";          OK = [bool](Resolve-DnsName "_ldap._tcp.dc._msdcs.contoso.local" -Type SRV -Server $dns -ErrorAction SilentlyContinue) }
)
$tests | ForEach-Object { if (-not $_.OK) { Write-Warning "ÉCHEC : $($_.Nom) sur $dns" } }
```

---

# Chapitre 12 — Dépannage

## 90. Méthodologie de dépannage DNS

Ne partez jamais dans tous les sens. Ordre systématique :

1. **Qualifier** : un poste ? un site ? tout le monde ? Un nom ? tous les noms ? Interne ? Internet ?
2. **Localiser la couche** : client (cache, suffixe, carte) → serveur (service, zone, transfert) → récursion (redirecteurs, pare-feu) → autoritaire distant.
3. **Reproduire** avec `Resolve-DnsName` en précisant `-Server` : comparez serveur par serveur.
4. **Éliminer le cache** : videz le cache client PUIS serveur avant de conclure (§95).
5. **Lire les logs** : événements DNS + journal analytique (§96-98).
6. **Corriger, vérifier, documenter** : notez la cause racine dans le ticket, pas juste « redémarré le service ».

```powershell
# Kit de premier niveau (à garder sous la main)
$Nom = "appli-metier.contoso.local"; $Srv = "10.0.0.10"
Resolve-DnsName -Name $Nom -Server $Srv -DnsOnly          # le serveur sait-il ?
Resolve-DnsName -Name $Nom -Server $Srv -CacheOnly        # vient-ce du cache ?
Resolve-DnsName -Name $Nom                                 # config client par défaut
```

## 91. nslookup : bases et pièges

`nslookup` existe toujours, mais il a des comportements trompeurs :

```
C:\> nslookup www.contoso.local
Serveur :   SRV-DNS-01.contoso.local
Address:    10.0.0.10
Nom :       www.contoso.local
Address:    10.0.1.50
```

**Pièges :**
- En mode interactif, `nslookup` ajoute parfois des suffixes de façon inattendue ; préférez `Resolve-DnsName` en script.
- `nslookup` n'affiche qu'**une** réponse A même s'il y en a plusieurs (round-robin masqué).
- Le timeout par défaut (2 s × 2 essais) est court : un « DNS request timed out » peut être un pare-feu qui *drop* (pas de rejet ICMP) plutôt qu'un DNS lent.
- Pour tester un transfert : `nslookup` → `ls contoso.local` — refusé si non autorisé (§50).

## 92. Resolve-DnsName : le successeur

L'applet moderne : pipeline PowerShell, types complets, DNSSEC, EDNS, cache.

```powershell
# Usages essentiels
Resolve-DnsName -Name "www.contoso.local" -Type ALL -Server "10.0.0.10"
Resolve-DnsName -Name "contoso.local" -Type MX -DnsOnly            # sans cache client
Resolve-DnsName -Name "contoso.local" -Type MX -CacheOnly         # QUE le cache
Resolve-DnsName -Name "www.contoso.local" -Server "10.0.0.10" -NoHostsFile  # ignore le hosts
Resolve-DnsName -Name "10.1.0.50"                                  # reverse auto (PTR)
Resolve-DnsName -Name "contoso.com" -Type SOA -DnssecOk           # avec bit DNSSEC
Resolve-DnsName -Name "www.example.com" -Trace                    # trace itérative complète

# Sortie exploitable en script
(Resolve-DnsName "www.contoso.local" -Type A -Server "10.0.0.10").IPAddress
```

**Réflexe :** un résultat différent entre `-DnsOnly` et sans option = le cache client vous mentait. Un résultat différent entre deux `-Server` = divergence de serveurs.

## 93. dnscmd pour le diagnostic

```powershell
dnscmd SRV-DNS-01 /Info                    # config serveur complète
dnscmd SRV-DNS-01 /ZoneInfo contoso.local  # détail d'une zone (aging, transferts…)
dnscmd SRV-DNS-01 /ZonePrint contoso.local # dump complet de la zone (équiv. fichier)
dnscmd SRV-DNS-01 /EnumZones               # toutes les zones + types
dnscmd SRV-DNS-01 /Statistics              # compteurs (requêtes, récursion, transferts)
```

`/Statistics` est précieux pour objectiver « le DNS est lent » : `Recursive queries`, `Recursive query failures`, `UDP/TCP queries received`.

## 94. Test-DnsServer

L'applet de test fonctionnel intégrée : elle exécute une batterie de tests (résolution simple, récursive, transfert…) contre un serveur.

```powershell
# Batterie complète contre 10.0.0.10
Test-DnsServer -IPAddress "10.0.0.10" | Format-Table TestName, Result -AutoSize

# Test ciblé : le serveur fait-il autorité pour la zone ?
Test-DnsServer -IPAddress "10.0.0.10" -ZoneName "contoso.local" -Context "Authoritative"

# Le redirecteur répond-il ?
Test-DnsServer -IPAddress "1.1.1.1" -Context "Forwarder" -ComputerName "SRV-DNS-01"
```

Un `Result = Failure` sur le contexte `Recursion` + `Authoritative = Pass` = problème de redirecteurs/racine, pas de zone.

## 95. Vider le cache : client et serveur

Le cache est le suspect n°1 des « ça marche sur son poste mais pas le mien ».

```powershell
# Côté client
Clear-DnsClientCache
ipconfig /displaydns | Select-String "www.contoso.local" -Context 2,6  # inspecter avant de vider

# Côté serveur (vide le cache du service DNS : récursion + réponses négatives)
Clear-DnsServerCache -ComputerName "SRV-DNS-01" -Force

# Voir la taille du cache serveur
(Get-DnsServerStatistics -ComputerName "SRV-DNS-01").CacheStatistics
```

⚠️ Vider le cache serveur en pleine journée = pic de requêtes récursives (tout est à re-résoudre). À faire en heure creuse si le serveur est très sollicité. Le cache **négatif** (NXDOMAIN, TTL = Minimum du SOA, §43) est souvent le vrai coupable après la création d'un enregistrement « qui ne se résout pas ».

## 96. Les journaux DNS : lequel regarder

| Journal | Où | Contenu |
|---|---|---|
| Événements DNS Server | Observateur d'événements → Journaux Windows → DNS Server | Démarrage, transferts, scavenging (2501), erreurs AD (4013/4015) |
| Analytique DNS | Journaux des applications et services → Microsoft → Windows → DNS-Server → Analytical | **Chaque requête/réponse** (à activer, verbeux) |
| Audit DNS | … → DNS-Server → Audit | Modifications de zones (qui a créé/supprimé quoi) |
| Debug logging | Fichier `%SystemRoot%\System32\dns\dns.log` | Requêtes brutes avec flags (ancien mais précis) |
| Netlogon | `%SystemRoot%\debug\netlogon.log` | Enregistrements SRV des DC (5774/5775 en échec) |

Activez l'**Audit** en permanence (coût faible, précieux en enquête) ; l'**Analytique** uniquement pendant un dépannage ciblé (volume énorme).

```powershell
# Activer le journal analytique (désactivé par défaut)
wevtutil sl Microsoft-Windows-DNSServer/Analytical /e:true
# Le désactiver après le dépannage !
wevtutil sl Microsoft-Windows-DNSServer/Analytical /e:false
```

## 97. Journal analytique DNS

Une fois activé, chaque requête apparaît avec : horodatage, IP client, QNAME, QTYPE, réponse (RCODE), durée. C'est l'équivalent d'un Wireshark ciblé DNS.

```powershell
# Requêtes vers un nom suspect sur les 30 dernières minutes
$debut = (Get-Date).AddMinutes(-30)
Get-WinEvent -LogName "Microsoft-Windows-DNSServer/Analytical" |
    Where-Object { $_.TimeCreated -ge $debut -and $_.Message -match "appli-metier" } |
    Select-Object TimeCreated, @{n='Détail';e={$_.Message.Substring(0,120)}} | Format-Table -AutoSize
```

Filtrez par IP client pour traquer un poste qui spamme (boucle applicative, malware de type DGA) : triez par QNAME distincts.

## 98. Debug logging (dnscmd /LogLevel)

Le debug logging écrit dans `dns.log` avec un niveau de détail réglable par masque hexadécimal.

```powershell
# Activer un debug complet temporaire (valeur courante de debug exhaustif)
dnscmd SRV-DNS-01 /Config /LogLevel 0x8100F331
dnscmd SRV-DNS-01 /Config /LogFilePath "C:\Windows\System32\dns\dns.log"
dnscmd SRV-DNS-01 /Config /LogFileMaxSize 0x4000000   # 64 Mo

# Désactiver après usage (0x0 = aucun log)
dnscmd SRV-DNS-01 /Config /LogLevel 0x0
```

⚠️ Ne laissez **jamais** le debug logging actif en permanence : le fichier grossit vite et chaque requête coûte des E/S disque. Activez, reproduisez, capturez, désactivez.

## 99. Event IDs à connaître

| ID | Source | Signification | Action |
|---|---|---|---|
| 2 / 3 | DNS Server | Démarrage/arrêt du service | Corrélé aux maintenances |
| 2501 | DNS Server | Scavenging : N enregistrements supprimés | Vérifier que N est plausible (§56) |
| 4004 / 4015 | DNS Server | Le serveur n'a pas pu charger les zones AD | Vérifier AD/réplication, puis `nltest /dsregdns` |
| 4013 | DNS Server | Zones AD non chargées au démarrage (AD pas prêt) | Souvent transitoire ; persistant = problème AD |
| 4515 | DNS Server | **Zone en double** (fichier + AD) | Supprimer le doublon (cas n°13, §112) |
| 7697 | DNS Server | Échec de transfert de zone | Liste blanche, pare-feu, maître |
| 5774 / 5775 | Netlogon | Échec d'enregistrement DNS du DC | Droits, zone, DNS du DC lui-même |
| 5781 | Netlogon | Aucun DC localisable (dynamique) | Conséquence, pas la cause : cherchez le DNS |

```powershell
# Balayer les erreurs DNS des dernières 24 h
Get-WinEvent -LogName "DNS Server" |
    Where-Object { $_.TimeCreated -gt (Get-Date).AddHours(-24) -and $_.Level -le 2 } |
    Select-Object TimeCreated, Id, @{n='Message';e={$_.Message.Substring(0,100)}} |
    Sort-Object TimeCreated
```

---

## 100. Cas pratique n°1 : le client ne résout plus rien

**Symptômes :** un poste (ou un VLAN entier) ne résout aucun nom, interne comme externe. Les IP fonctionnent (ping 10.0.1.50 OK).

**Diagnostic :**
```powershell
# Sur le poste : que vaut sa config ?
Get-DnsClientServerAddress -InterfaceAlias "Ethernet0" | Select-Object -ExpandProperty ServerAddresses
# Teste-t-il le bon serveur ?
Resolve-DnsName "www.contoso.local" -Server "10.0.0.10" -ErrorAction Stop
# Le serveur DNS est-il joignable en 53 ?
Test-NetConnection -ComputerName "10.0.0.10" -Port 53
```
**Causes fréquentes :** mauvaise IP DNS distribuée par DHCP (option 006), pare-feu local/VLAN qui bloque l'UDP 53, carte réseau du poste avec DNS en dur obsolète, ou le serveur DNS lui-même arrêté.

**Résolution :** corriger la source (DHCP, GPO, pare-feu), `Clear-DnsClientCache`, puis valider avec `Resolve-DnsName` sans `-Server` (config par défaut du poste).

## 101. Cas pratique n°2 : résolution intermittente

**Symptômes :** ça marche, puis ça ne marche plus, puis ça remarche. Souvent « le matin ça rame ».

**Diagnostic :**
```powershell
# Le poste a-t-il DEUX DNS dont un mort ?
Get-DnsClientServerAddress | Select-Object InterfaceAlias, ServerAddresses
# Tester chaque serveur séparément, plusieurs fois
1..5 | ForEach-Object { Resolve-DnsName "www.contoso.local" -Server "10.0.0.11" -DnsOnly | Select-Object -ExpandProperty IPAddress }
# NS fantôme dans la zone ?
Get-DnsServerResourceRecord -ZoneName "contoso.local" -RRType NS
```
**Causes fréquentes :** DNS secondaire configuré mais hors ligne (le client bascule après timeout), NS fantôme dans la zone (serveur décommissionné), round-robin vers une IP morte (§36), ou scavenging qui a supprimé un enregistrement encore utilisé.

**Résolution :** supprimez les serveurs morts de la config DHCP et des NS de zone ; remplacez le round-robin aveugle par un vrai équilibreur si la dispo compte.

## 102. Cas pratique n°3 : un nom interne résout vers une IP externe

**Symptômes :** `intranet.contoso.com` résout `203.0.113.10` (public) au lieu de `10.0.1.50` sur le LAN → hairpinning, lenteurs, blocages pare-feu.

**Diagnostic :**
```powershell
Resolve-DnsName "intranet.contoso.com" -Server "10.0.0.10" -DnsOnly
# D'où vient la réponse ? De votre zone ou du redirecteur ?
Get-DnsServerResourceRecord -ZoneName "contoso.com" -Name "intranet" -ErrorAction SilentlyContinue
Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com"
```
**Causes fréquentes :** pas de zone/vue interne pour ce nom (split-brain non géré, §63), stratégie DNS dont le subnet ne couvre pas le VLAN du client (§66), ou enregistrement interne oublié lors de la publication d'un nouveau service public.

**Résolution :** créer l'enregistrement dans la vue/scope interne (ou la zone interne), tester depuis chaque type de réseau (LAN, VPN, Wi-Fi invité).

## 103. Cas pratique n°4 : enregistrements dupliqués / round-robin inattendu

**Symptômes :** un nom résout vers 3 IP dont une obsolète ; une appli tombe une fois sur trois.

**Diagnostic :**
```powershell
# Lister TOUS les A du nom (nslookup n'en montre qu'un !)
Get-DnsServerResourceRecord -ZoneName "contoso.local" -Name "appli" -RRType A |
    Select-Object HostName, Timestamp, @{n='IP';e={$_.RecordData.IPv4Address}}
# Lequel est statique, lequel est dynamique ?
```
**Causes fréquentes :** ancien A statique oublié lors d'une migration + nouveau A dynamique (doublon), ou deux machines qui s'enregistrent sous le même nom (image dupliquée sans sysprep → même nom d'hôte).

**Résolution :** supprimer l'enregistrement obsolète (`Remove-DnsServerResourceRecord` en précisant `-RecordData`), corriger la cause (sysprep, inventaire), et baisser le TTL avant les prochaines migrations (§12).

## 104. Cas pratique n°5 : le scavenging a supprimé des enregistrements valides

**Symptômes :** après un week-end prolongé ou des congés, des machines/équipements « disparaissent » du DNS.

**Diagnostic :**
```powershell
# Le scavenging a-t-il tourné ? Combien de suppressions ?
Get-WinEvent -LogName "DNS Server" | Where-Object Id -eq 2501 |
    Select-Object -First 5 TimeCreated, Message
# Comparer bail DHCP et intervalles
Get-DhcpServerv4Scope -ComputerName "SRV-DHCP-01" | Select-Object Name, LeaseDuration
Get-DnsServerZoneAging -Name "contoso.local" | Select-Object NoRefreshInterval, RefreshInterval
Get-DnsServerScavenging | Select-Object ScavengingState, ScavengingInterval
```
**Cause typique :** `no-refresh + refresh` (ex : 7+7 j) < indisponibilité des machines (congés 3 semaines) ou < bail DHCP (30 j). Le timestamp n'a pas été rafraîchi → périmé → supprimé.

**Résolution :** restaurez les enregistrements (recréation ou restauration §143), **augmentez** les intervalles (ex : 14+14 j) ou réduisez les baux DHCP, et pour les équipements critiques : enregistrements **statiques**.

## 105. Cas pratique n°6 : les mises à jour dynamiques échouent

**Symptômes :** les nouveaux postes n'apparaissent pas dans le DNS ; erreurs Netlogon 5774/5775 sur les DC.

**Diagnostic :**
```powershell
# La zone accepte-t-elle les mises à jour sécurisées ?
Get-DnsServerZone -Name "contoso.local" | Select-Object DynamicUpdate, IsDsIntegrated
# Le client pointe-t-il un DNS qui fait autorité pour la zone ?
Get-DnsClientServerAddress
# Test manuel d'enregistrement
ipconfig /registerdns
Get-WinEvent -LogName "System" | Where-Object Id -in 5774,5775 | Select-Object -First 3
```
**Causes fréquentes :** zone en `None` au lieu de `Secure`, client qui pointe vers un DNS externe ou un secondaire, permissions insuffisantes sur la zone (héritage cassé), ou horloge désynchronisée (Kerberos refuse → l'update sécurisé échoue silencieusement).

**Résolution :** repassez la zone en `Secure`, corrigez le DNS du client, vérifiez la synchro horaire (`w32tm /query /status`), puis `nltest /dsregdns` / `ipconfig /registerdns`.

## 106. Cas pratique n°7 : transfert de zone refusé

**Symptômes :** le secondaire reste avec un vieux serial ; Event 7697 côté secondaire.

**Diagnostic :**
```powershell
# Côté primaire : qui est autorisé ?
Get-DnsServerZoneTransfer -Name "contoso.local" | Select-Object SecureSecondaries, SecondaryServers
# Le port TCP 53 est-il ouvert du secondaire vers le primaire ?
Test-NetConnection -ComputerName "10.0.0.10" -Port 53
# Comparer les serials
(Resolve-DnsName "contoso.local" -Type SOA -Server "10.0.0.10").SerialNumber
(Resolve-DnsName "contoso.local" -Type SOA -Server "192.168.99.10").SerialNumber
```
**Causes fréquentes :** IP du secondaire absente de la liste blanche (§50), pare-feu qui bloque TCP 53 (l'AXFR/IXFR est en TCP, pas UDP !), ou maître injoignable.

**Résolution :** ajoutez l'IP (`Set-DnsServerZoneTransfer`), ouvrez TCP 53, forcez `Sync-DnsServerZone` côté secondaire.

## 107. Cas pratique n°8 : lenteurs de résolution (timeouts)

**Symptômes :** chaque première résolution prend 3–6 s ; navigation « qui hésite ».

**Diagnostic :**
```powershell
# Mesurer : où part le temps ?
Measure-Command { Resolve-DnsName "www.contoso.local" -Server "10.0.0.10" -DnsOnly }
Measure-Command { Resolve-DnsName "www.example.com"   -Server "10.0.0.10" -DnsOnly }
# Les redirecteurs répondent-ils vite ?
1..3 | ForEach-Object { Measure-Command { Resolve-DnsName "www.example.com" -Server "1.1.1.1" -DnsOnly } }
Get-DnsServerForwarder
```
**Causes fréquentes :** redirecteur lent ou injoignable (timeout 3 s × N redirecteurs avant repli), NS fantôme, EDNS bloqué par un vieux pare-feu (les grosses réponses UDP sont droppées → repli TCP lent), ou cache froid après un `Clear-DnsServerCache` en heure de pointe.

**Résolution :** remplacez le redirecteur lent, vérifiez `-Timeout`, testez avec `-DnssecOk` pour isoler un problème EDNS, et ne videz le cache qu'en heure creuse (§95).

## 108. Cas pratique n°9 : SRV d'AD manquants après promotion d'un DC

**Symptômes :** `dcdiag /test:dns` en échec, les clients n'utilisent pas le nouveau DC, `nltest /dnsgetdc` ne le liste pas.

**Diagnostic :**
```powershell
# Le DC s'est-il enregistré ?
Get-DnsServerResourceRecord -ZoneName "_msdcs.contoso.local" -RRType SRV |
    Where-Object { $_.RecordData.DomainName -match "SRV-DC-03" }
# Que dit Netlogon ?
Get-WinEvent -LogName "System" | Where-Object Id -in 5774,5775 | Select-Object -First 5 TimeCreated, Message
```
**Causes fréquentes :** la carte réseau du nouveau DC ne pointe pas vers un DNS interne (§20), la zone `_msdcs` n'accepte pas les mises à jour, ou le service Netlogon n'a pas encore tourné (attendre ~1 h ou forcer).

**Résolution :** corrigez le DNS de la carte, puis `nltest /dsregdns` sur le nouveau DC, et revérifiez les SRV + le CNAME GUID (§7).

## 109. Cas pratique n°10 : le DC ne s'enregistre plus (Netlogon 5774/5775)

**Symptômes :** Event 5774/5775 réguliers sur un DC ; ses SRV disparaissent progressivement (scavenging) → les clients ne le trouvent plus.

**Diagnostic :**
```powershell
# Forcer puis observer
nltest /dsregdns; Start-Sleep 5
Get-WinEvent -LogName "System" -MaxEvents 20 | Where-Object Id -in 5774,5775
# Horloge (Kerberos) ?
w32tm /query /status
# La zone est-elle en Secure + AD-integrated ?
Get-DnsServerZone -Name "contoso.local" | Select-Object DynamicUpdate, IsDsIntegrated
```
**Causes fréquentes :** mot de passe du compte machine désynchronisé, horloge dérivée > 5 min, zone passée en `None`, ou ACL de la zone cassées.

**Résolution :** `w32tm /resync`, `nltest /dsregdns`, en dernier recours `netdom resetpwd` + redémarrage Netlogon. Traitez la cause, pas le symptôme : un DC qui ne s'enregistre plus est souvent un DC dont l'AD va mal.

## 110. Cas pratique n°11 : DNSSEC — validation qui échoue (SERVFAIL)

**Symptômes :** une zone signée ne se résout plus (`SERVFAIL`), les autres zones fonctionnent.

**Diagnostic :**
```powershell
# Sans validation, ça répond ?
Resolve-DnsName "www.contoso.com" -Server "10.0.0.10" -DnssecOk:$false
# Avec validation ?
Resolve-DnsName "www.contoso.com" -Server "10.0.0.10" -DnssecOk
# La chaîne est-elle intacte ? (DS chez le parent, signatures valides, horloge)
```
**Causes fréquentes :** rollover KSK sans mise à jour du DS chez le registrar (§72), signatures expirées (horloge du serveur), ou redirecteur qui **casse** DNSSEC (ne transmet pas les enregistrements) → dans ce cas la validation échoue à cause du redirecteur, pas de la zone (§74).

**Résolution :** vérifiez le DS public (`Resolve-DnsName contoso.com -Type DS -Server 8.8.8.8`), l'horloge, et contournez temporairement le redirecteur suspect en interrogeant la racine.

## 111. Cas pratique n°12 : boucle de redirecteurs

**Symptômes :** timeouts généralisés sur certaines zones ; le journal analytique montre des requêtes en boucle entre deux IP.

**Diagnostic :**
```powershell
Resolve-DnsName -Name "srv.partenaire.local" -Trace | Select-Object -Last 25
Get-DnsServerForwarder -ComputerName "SRV-DNS-01"
Get-DnsServerZone -Name "partenaire.local"   # conditional forwarder ?
```
**Cause :** A redirige vers B qui redirige vers A (classique après une fusion, §62).

**Résolution :** cassez la boucle au bon endroit (le redirecteur doit pointer vers un serveur **autoritaire** ou un résolveur Internet, jamais vers un pair qui vous renvoie la balle), documentez la chaîne complète.

## 112. Cas pratique n°13 : zone AD-integrated qui ne se réplique pas

**Symptômes :** un enregistrement créé sur `SRV-DNS-01` n'apparaît pas sur `SRV-DNS-02`, même après des heures.

**Diagnostic :**
```powershell
# La réplication AD fonctionne-t-elle ? (le DNS n'est que le messager)
repadmin /showrepl SRV-DNS-02
dcdiag /test:replications
# La zone est-elle AD-integrated des DEUX côtés ?
Get-DnsServerZone -Name "contoso.local" -ComputerName "SRV-DNS-01" | Select-Object IsDsIntegrated, DirectoryPartitionName
Get-DnsServerZone -Name "contoso.local" -ComputerName "SRV-DNS-02" | Select-Object IsDsIntegrated, DirectoryPartitionName
# Doublon fichier + AD ? (Event 4515)
Get-WinEvent -LogName "DNS Server" | Where-Object Id -eq 4515 | Select-Object -First 3
```
**Causes fréquentes :** la réplication AD est cassée (le DNS est innocent), portée de réplication différente entre serveurs, ou **zone en double** (fichier `.dns` + AD : le serveur charge le fichier et ignore l'AD → Event 4515).

**Résolution :** réparez d'abord AD (`repadmin`, `dcdiag`), harmonisez les portées (§29), supprimez le doublon fichier si 4515.

## 113. Cas pratique n°14 : cache poisoning suspecté

**Symptômes :** un nom résout vers une IP inconnue de façon intermittente ; plusieurs postes impactés (donc ce n'est pas le cache d'un seul poste).

**Diagnostic :**
```powershell
# D'où vient la réponse ? Cache serveur ou autoritaire ?
Resolve-DnsName "banque.contoso.com" -Server "10.0.0.10" -CacheOnly
Resolve-DnsName "banque.contoso.com" -Server "10.0.0.10" -DnsOnly
# Le serveur autoritaire légitime dit quoi ? (court-circuitez votre infra)
Resolve-DnsName "banque.contoso.com" -Type A -Server "8.8.8.8" -DnsOnly
```
**Conduite à tenir :** 1) `Clear-DnsServerCache` immédiat ; 2) vérifiez le cache locking (§119) et que la récursion n'est pas ouverte (§117) ; 3) journal analytique pour identifier la réponse forgée et sa source ; 4) si zone critique : envisagez DNSSEC (§69). Un « poisoning » est souvent en réalité un **vieux redirecteur compromis** ou un équipement d'interception (proxy, portail captif) — vérifiez avant d'incriminer le protocole.

## 114. Cas pratique n°15 : après migration / changement d'IP d'un DC

**Symptômes :** après changement d'IP d'un DC (ou migration de serveur), des clients tentent encore l'ancienne IP ; réplication en erreur.

**Checklist post-changement :**
```powershell
# 1. Les A du DC sont-ils à jour ? (domaine nu + nom du DC + _msdcs)
Get-DnsServerResourceRecord -ZoneName "contoso.local" -RRType A |
    Where-Object { $_.RecordData.IPv4Address -eq "10.0.0.99" }  # ancienne IP
# 2. Supprimer les A obsolètes (préciser -RecordData)
# 3. Forcer le ré-enregistrement
Invoke-Command -ComputerName "SRV-DC-03" -ScriptBlock { nltest /dsregdns; ipconfig /registerdns }
# 4. Vider les caches (serveurs + clients critiques)
Clear-DnsServerCache -ComputerName "SRV-DNS-01","SRV-DNS-02" -Force
# 5. Vérifier la réplication AD vers le DC
repadmin /showrepl SRV-DC-03
```
**Prévention :** avant toute migration, baissez les TTL à 300 s (§12), planifiez le scavenging, et gardez l'ancienne IP en alias le temps de la propagation (TTL × 2).

---

# Chapitre 13 — Sécurité

## 115. Surface d'attaque du DNS

Le DNS est une cible de choix : il voit **tout** le trafic de nommage, il est interrogé par **tout** le monde, et une réponse falsifiée redirige silencieusement les utilisateurs.

| Menace | Impact | Contre-mesure (section) |
|---|---|---|
| Cache poisoning | Redirection vers un faux serveur | Cache locking (§119), DNSSEC (§69), ports source aléatoires |
| Récursion ouverte | Amplification DDoS, exfiltration | Restreindre la récursion (§117) |
| AXFR sauvage | Cartographie complète du SI | Liste blanche de transferts (§50) |
| Détournement de mises à jour dynamiques | Enregistrements forgés | Updates sécurisés uniquement (§78) |
| Tunneling DNS (exfiltration via requêtes) | Fuite de données | Filtrage, analytique DNS (§97), pare-feu |
| Escalade via DnsAdmins | Contrôle du DC | Ne pas donner DnsAdmins (§120) |
| DGA / C2 de malware | Communication malware | Monitoring des QNAME (§97), DNS filtrant |

## 116. Cache poisoning et protections

L'attaque : un attaquant devine/forge une réponse à une requête récursive en cours et **empoisonne le cache** du résolveur pour la durée du TTL.

Défenses en profondeur (toutes activables sur Windows Server) :
1. **Ports source UDP aléatoires** (activés par défaut depuis 2008) : rend la devinette beaucoup plus dure.
2. **Cache locking** (§119) : une entrée en cache ne peut pas être écrasée avant un pourcentage de son TTL.
3. **0x20 encoding** (casse aléatoire) : le résolveur Windows mélange minuscules/majuscules dans la question et vérifie la réponse.
4. **DNSSEC** (§69) : la parade cryptographique définitive pour les zones signées.
5. Ne jamais exposer la récursion (§117).

```powershell
# Vérifier que le verrouillage du cache est actif (100 % par défaut)
Get-DnsServerCache | Select-Object LockingPercent, MaxTTL, MaxNegativeTTL
```

## 117. Restreindre la récursion

Un serveur **autoritaire public** (qui héberge vos zones Internet) ne doit **pas** faire de récursion pour le monde entier : c'est la porte ouverte à l'amplification DDoS (petite requête → grosse réponse, avec IP source usurpée).

```powershell
# Désactiver totalement la récursion (serveur autoritaire exposé)
Set-DnsServerRecursion -Enable $false

# Ou : récursion uniquement pour vos réseaux (2016+, recursion scopes)
Add-DnsServerRecursionScope -Name "Interne" -EnableRecursion $true
Add-DnsServerClientSubnet -Name "Subnet-LAN" -IPv4Subnet "10.0.0.0/8"
Add-DnsServerQueryResolutionPolicy -Name "Recursion-Interne" -Action ALLOW `
    -ClientSubnet "EQ,Subnet-LAN" -RecursionScope "Interne" -PassThru
```

**Test externe :** depuis Internet, `nslookup www.example.com <votre-IP-publique>` doit **échouer** (refusé) sur un autoritaire pur, **réussir** sur un résolveur interne (depuis le LAN uniquement).

## 118. Response Rate Limiting (RRL)

Le RRL limite le nombre de réponses **identiques** envoyées à un même client : il casse l'amplification DDoS (l'attaquant ne reçoit plus qu'un filet de réponses) sans pénaliser les clients légitimes.

```powershell
# Activer le RRL (Windows Server 2016+)
Set-DnsServerResponseRateLimiting -Enable $true -ResponsesPerSec 5 -WindowInSec 5 `
    -LeakRate 2 -MaxResponses 50 -PassThru

# Vérifier
Get-DnsServerResponseRateLimiting | Format-List Enable, ResponsesPerSec, WindowInSec
```

⚠️ Version : RRL = **2016 et +**. Réglages : commencez permissif (`ResponsesPerSec 10`), observez les faux positifs (un NAT qui concentre 500 postes derrière une IP peut déclencher le RRL — whitelistez vos NAT sortants si besoin).

## 119. Cache locking

Le verrouillage du cache empêche l'écrasement d'une entrée avant qu'un pourcentage de son TTL se soit écoulé. À 100 % (défaut), une entrée empoisonnée par erreur ne peut pas être « corrigée » par une réponse légitime avant expiration — mais une entrée légitime ne peut pas non plus être empoisonnée.

```powershell
# 100 % = protection maximale (défaut, recommandé)
Set-DnsServerCache -LockingPercent 100 -PassThru
# En dépannage uniquement : baisser temporairement pour forcer un rafraîchissement
Set-DnsServerCache -LockingPercent 0; Clear-DnsServerCache -Force; Set-DnsServerCache -LockingPercent 100
```

## 120. Le groupe DnsAdmins : risque d'escalade

**Fait peu connu, critique :** un membre du groupe **DnsAdmins** peut, sur un DC qui est aussi DNS, charger une DLL arbitraire comme plugin du service DNS (`ServerLevelPluginDll`) → exécution de code **SYSTEM** sur le DC → compromission du domaine. (Vecteur documenté depuis 2017, toujours d'actualité par design.)

**Règles :**
1. **Ne mettez personne dans DnsAdmins** en routine. Les admin DNS quotidiens n'en ont pas besoin.
2. Déléguez via des **ACL fines** : les opérateurs peuvent gérer les enregistrements d'une zone sans être DnsAdmins (clic droit zone → Propriétés → Sécurité).
3. Auditez le groupe régulièrement :
```powershell
Get-ADGroupMember -Identity "DnsAdmins" | Select-Object Name, SamAccountName
```
4. Si un DC est DNS (cas général), considérez DnsAdmins comme équivalent à « Domain Admins » dans votre modèle de menace.

## 121. Durcissement : liste de mesures

- [ ] Récursion désactivée ou restreinte sur tout serveur exposé (§117)
- [ ] Transferts de zone en liste blanche (§50)
- [ ] Mises à jour dynamiques en `Secure` sur toutes les zones AD (§78)
- [ ] Cache locking à 100 % (§119), RRL activé sur les exposés (§118)
- [ ] Groupe DnsAdmins vide ou quasi-vide, audité (§120)
- [ ] Journal **Audit** DNS activé en permanence (§96)
- [ ] `cache.dns` (root hints) à jour : `Get-DnsServerRootHint` vs liste officielle
- [ ] Pas de DNS externe dans la carte des DC (§20)
- [ ] Global Query Block List active (bloque `wpad`/`isatap` par défaut — anti-hijacking WPAD) :
```powershell
Get-DnsServerGlobalQueryBlockList | Select-Object -ExpandProperty ListName
```
- [ ] Mises à jour Windows à jour sur les DNS (le service DNS a eu des CVE critiques, ex : SIGRed CVE-2020-1350 — 17 ans de vulnérabilité RCE wormable)
- [ ] Comptes de service DHCP dédiés, mots de passe gérés (§80)

## 122. DNS et pare-feu : flux réseau

| Flux | Ports | Sens | Commentaire |
|---|---|---|---|
| Clients → DNS interne | UDP/TCP 53 | Entrant | TCP pour les grosses réponses/DNSSEC |
| DNS → redirecteurs / Internet | UDP/TCP 53 | Sortant | Restreignez aux redirecteurs si vous en utilisez |
| DNS ↔ DNS (transferts) | TCP 53 | Bilatéral | AXFR/IXFR = TCP uniquement |
| DC ↔ DC (AD-integrated) | 53 + ports AD (389, 636, 3268, 445, 135, RPC dyn.) | Bilatéral | La réplication de zone passe par AD |
| DHCP → DNS (updates) | UDP/TCP 53 | Sortant DHCP | Mises à jour dynamiques |
| Admin → DNS | RPC/WMI/WinRM | Entrant | Console DNS, PowerShell distant |

```powershell
# Règle pare-feu : autoriser le DNS entrant (si vous gérez le pare-feu Windows)
New-NetFirewallRule -DisplayName "DNS entrant UDP/TCP 53" -Direction Inbound `
    -Protocol UDP -LocalPort 53 -Action Allow -Profile Domain
New-NetFirewallRule -DisplayName "DNS entrant TCP 53" -Direction Inbound `
    -Protocol TCP -LocalPort 53 -Action Allow -Profile Domain
```

⚠️ Le piège classique : on ouvre l'UDP 53 mais pas le **TCP 53** → les transferts de zone et les réponses DNSSEC échouent mystérieusement.

---

# Chapitre 14 — 18 erreurs classiques

## 123. Erreur n°1 : le serveur DNS ne se pointe pas lui-même en premier

**Symptôme :** lenteurs d'ouverture de session, erreurs Netlogon, le DC « ne se trouve pas lui-même ».
**Cause :** la carte du DC pointe vers un autre DNS (ou un externe) en préféré.
**Correction :** lui-même (`127.0.0.1`) en premier, le partenaire en second (§20). À vérifier **après chaque changement d'IP**.

## 124. Erreur n°2 : l'îlot DNS (DNS island)

**Symptôme :** après la promotion d'un nouveau DC/DNS, il ne réplique pas et ne trouve pas les autres DC.
**Cause :** le nouveau serveur se pointe **lui-même en premier** alors que sa zone est encore vide → il s'isole (il ne connaît que lui).
**Correction :** pendant la promotion, pointez le futur DC vers un DNS **existant** en préféré ; ne le faites se pointer lui-même qu'**après** la réplication initiale. Puis remettez la config standard (§20).

## 125. Erreur n°3 : scavenging activé avec des intervalles incohérents

**Symptôme :** des enregistrements valides disparaissent (voir cas n°5, §104).
**Cause :** `no-refresh + refresh` < bail DHCP ou < absences typiques.
**Correction :** alignez les trois (bail DHCP ≤ no-refresh + refresh, scavenging ≥ 7 j), et un seul serveur scavenger (§56).

## 126. Erreur n°4 : des enregistrements « statiques » qui ne le sont pas

**Symptôme :** l'imprimante du service compta disparaît du DNS tous les mois.
**Cause :** enregistrement créé par DHCP (dynamique, timestampé) pour un équipement passé ensuite en IP fixe sans repasser l'enregistrement en statique.
**Correction :** recréez l'enregistrement à la main (timestamp à 0, §84) ou décochez « Supprimer cet enregistrement lorsqu'il devient périmé ».

## 127. Erreur n°5 : un CNAME au sommet de zone (apex)

**Symptôme :** la zone ne charge plus, ou les MX du domaine ne fonctionnent plus.
**Cause :** un CNAME à l'apex cohabite avec le SOA et les NS — interdit par les RFC, le serveur Windows le refuse ou produit une zone invalide.
**Correction :** utilisez un A à l'apex (quitte à le maintenir via script/API), jamais un CNAME (§37).

## 128. Erreur n°6 : un TTL d'une heure sur un enregistrement critique avant migration

**Symptôme :** le jour de la bascule, la moitié des clients va encore sur l'ancienne IP pendant des heures.
**Cause :** TTL trop long + pas de baisse préalable.
**Correction :** 48 h avant : TTL à 300 s ; jour J : changez l'IP ; J+2 : remontez le TTL (§12).

## 129. Erreur n°7 : des redirecteurs lents ou morts

**Symptôme :** toute la résolution Internet est lente par intermittence.
**Cause :** redirecteur injoignable (timeout 3 s à chaque requête froide) ou surchargé.
**Correction :** `Test-DnsServer -Context Forwarder`, remplacez par des résolveurs rapides et redondants, réglez `-Timeout` (§60).

## 130. Erreur n°8 : la zone `_msdcs` supprimée ou corrompue

**Symptôme :** plus aucun client ne trouve de DC ; `dcdiag /test:dns` catastrophique.
**Cause :** suppression « de ménage » de la zone `_msdcs` (elle paraît redondante avec `contoso.local`).
**Correction :** recréez la zone AD-integrated `_msdcs.contoso.local` (portée **Forest**), redémarrez Netlogon sur chaque DC puis `nltest /dsregdns` partout. Et ne recommencez jamais (§7).

## 131. Erreur n°9 : DHCP sans compte de service dédié

**Symptômes :** après remplacement d'un serveur DHCP, les mises à jour DNS échouent (« accès refusé »), doublons d'enregistrements.
**Cause :** les enregistrements appartiennent à l'ancien compte machine du DHCP.
**Correction :** compte de service dédié + groupe DnsUpdateProxy **avant** la mise en production (§80).

## 132. Erreur n°10 : transferts de zone ouverts à « tout serveur »

**Symptôme :** aucun — c'est silencieux. Un `dig AXFR contoso.local @votre-DNS` liste toute votre infrastructure (noms de serveurs, parfois leur rôle).
**Cause :** liste blanche non configurée.
**Correction :** liste blanche d'IP uniquement (§50). Testez depuis l'extérieur après chaque changement.

## 133. Erreur n°11 : mises à jour dynamiques non sécurisées sur une zone AD

**Symptôme :** aucun — jusqu'au jour où un poste vérolé écrase l'enregistrement d'un serveur.
**Cause :** zone en `NonsecureAndSecure` « pour que les imprimantes puissent s'enregistrer ».
**Correction :** repassez en `Secure` ; les équipements qui ne font pas Kerberos passent par le DHCP (modèle B, §79) ou des enregistrements statiques.

## 134. Erreur n°12 : pas de zone inversée

**Symptôme :** `nslookup 10.1.0.50` répond « *** can't find », logs illisibles, applications qui vérifient le reverse en échec.
**Cause :** zone `in-addr.arpa` jamais créée.
**Correction :** une zone inversée par sous-réseau, mises à jour sécurisées, PTR via DHCP (§32, §40).

## 135. Erreur n°13 : un seul serveur DNS distribué par DHCP

**Symptôme :** quand `SRV-DNS-01` est en maintenance, tout le site est aveugle.
**Cause :** option DHCP 006 avec une seule IP.
**Correction :** toujours **deux** DNS dans le DHCP (et dans les configs statiques), sur deux serveurs physiques/VM distincts (§85).

## 136. Erreur n°14 : confondre zone stub et redirecteur conditionnel

**Symptôme :** après un changement de NS chez le partenaire, la résolution casse (ou au contraire on s'étonne qu'elle suive toute seule).
**Cause :** on attendait le comportement de l'autre mécanisme.
**Correction :** stub = suit les NS distants automatiquement ; conditional forwarder = IP fixes, à maintenir (§26, §61). Choisissez en conscience.

## 137. Erreur n°15 : ne jamais vider ni tester le cache

**Symptôme :** « on a corrigé l'enregistrement mais ça ne marche toujours pas » pendant des heures.
**Cause :** cache négatif (NXDOMAIN) ou cache serveur périmé ; personne ne pense à `Clear-DnsServerCache`.
**Correction :** réflexe `-DnsOnly` vs `-CacheOnly` pour diagnostiquer (§92), vidage ciblé après correction (§95).

## 138. Erreur n°16 : TTL trop long pendant une migration (variante)

**Symptôme :** rollback impossible proprement : l'ancienne IP reste en cache partout.
**Cause :** on a remonté le TTL trop tôt, ou on n'a jamais baissé celui des enregistrements SRV d'AD (600 s par défaut — correct, mais vérifiez).
**Correction :** plan de TTL écrit dans le dossier de changement, avec les heures de baisse/remontée (§12).

## 139. Erreur n°17 : un DC dans le groupe DnsUpdateProxy

**Symptôme :** aucun — jusqu'à l'audit de sécurité (ou l'incident).
**Cause :** « pour que ça marche » on y a mis le compte machine du DC/DHCP.
**Correction :** retirez-le immédiatement : ses enregistrements deviennent non sécurisés et modifiables par tout membre du groupe (§81).

## 140. Erreur n°18 : récursion ouverte sur un serveur exposé

**Symptôme :** votre DNS sert d'amplificateur dans un DDoS ; votre FAI vous appelle.
**Cause :** serveur autoritaire public avec récursion activée pour tout le monde.
**Correction :** `Set-DnsServerRecursion -Enable $false` sur l'exposé, ou recursion scopes limités au LAN (§117), + RRL (§118).

---

# Chapitre 15 — Exploitation

## 141. Checklist de mise en production

**Avant :**
- [ ] Architecture validée : 2+ DNS par site, AD-integrated, portées de réplication définies (§85-86)
- [ ] Zones créées (directes + inversées), SOA/NS cohérents (§31-32, §43)
- [ ] Redirecteurs testés un par un (`Test-DnsServer -Context Forwarder`) ou root hints vérifiés (§60)
- [ ] Aging/scavenging paramétrés **et** documentés (qui scavenger, quels intervalles) (§54-56)
- [ ] DHCP : compte dédié + DnsUpdateProxy + modèle A/B choisi (§80)
- [ ] Transferts en liste blanche, récursion restreinte si exposé (§50, §117)
- [ ] TTL de migration baissés si bascule prévue (§12)

**Le jour J :**
- [ ] `Test-DnsServer` complet sur chaque serveur (§94)
- [ ] Résolution testée depuis chaque VLAN/site (interne, externe, SRV AD)
- [ ] Supervision branchée (sondes §89)
- [ ] Journal d'audit DNS activé (§96)

**Après (semaine 1) :**
- [ ] Revue des Event 2501/4004/4015/4515 (§99)
- [ ] Vérification des serials SOA entre serveurs (§51)
- [ ] Dossier d'exploitation à jour (schéma, chaînes de redirecteurs, scopes)

## 142. Checklist d'audit trimestriel

- [ ] Groupe **DnsAdmins** : membres légitimes uniquement (§120)
- [ ] Groupe **DnsUpdateProxy** : pas de DC dedans (§139)
- [ ] Zones : pas de doublon fichier+AD (Event 4515), NS à jour (pas de fantômes)
- [ ] Scavenging : Event 2501 plausibles, intervalles toujours cohérents avec les baux DHCP
- [ ] Enregistrements statiques critiques : inventaire à jour (serveurs, imprimantes, équipements)
- [ ] Redirecteurs : temps de réponse toujours OK ; root hints à jour
- [ ] DNSSEC : clés valides, prochains rollovers planifiés (§72)
- [ ] Sauvegarde des zones testée (restauration sur maquette, §143)
- [ ] Revue des stratégies DNS / scopes : la matrice split-brain est-elle toujours vraie ? (§68)
- [ ] Correctifs Windows installés sur les DNS (CVE DNS, §121)

## 143. Sauvegarde et restauration des zones

**Zones AD-integrated :** sauvegardées avec l'**état système** de l'AD (Windows Server Backup). Pas de fichier à copier : la restauration passe par l'AD.

**Zones standard (fichier) :** copiez `%SystemRoot%\System32\dns\*.dns` + exportez la config :

```powershell
# Sauvegarde : export de la zone + config du serveur
Export-DnsServerZone -Name "dmz.contoso.local" -FileName "sav-dmz.contoso.local.dns"
dnscmd SRV-DNS-01 /ZoneExport dmz.contoso.local sav-dmz2.txt
Copy-Item "C:\Windows\System32\dns\*.dns" "\\sauvegarde\dns\SRV-DNS-01\"

# Sauvegarde de la config complète (à garder avec)
Get-DnsServerZone | Export-Csv "\\sauvegarde\dns\zones.csv" -NoTypeInformation
Get-DnsServerForwarder | Export-Csv "\\sauvegarde\dns\forwarders.csv" -NoTypeInformation
```

**Restauration d'une zone standard :** recopiez le `.dns`, recréez la zone en pointant vers le fichier existant (`Add-DnsServerPrimaryZone -ZoneFile`), `Sync-DnsServerZone`.

**Restauration d'un enregistrement supprimé par erreur :** pas de corbeille AD pour le DNS → recréez-le (`Add-DnsServerResourceRecord*`) ou restaurez l'état système (lourd). D'où l'importance de l'**audit** (§96) pour savoir *quoi* recréer.

## 144. Supervision : compteurs et alertes

Compteurs de performance Windows utiles (`\DNS\...`) :

| Compteur | Ce qu'il dit | Alerte si |
|---|---|---|
| `Total Query Received/sec` | Charge | Variation brutale inexpliquée |
| `Recursive Queries/sec` | Part de récursion | Explosion = boucle ou attaque |
| `Recursive Query Failures/sec` | Échecs de récursion | > 1 % des récursives |
| `Zone Transfer Requests Received` | Demandes AXFR | Toute demande hors secondaires connus |
| `Dynamic Updates Received/sec` | Mises à jour dynamiques | Chute brutale = DHCP/AD en panne |

```powershell
# Relever les compteurs DNS en une ligne (échantillon 30 s)
Get-Counter "\DNS\Total Query Received/sec", "\DNS\Recursive Query Failures/sec" -SampleInterval 5 -MaxSamples 6

# Statistiques du service (alternative intégrée)
Get-DnsServerStatistics -ComputerName "SRV-DNS-01" | Select-Object -ExpandProperty QueryStatistics
```

Intégrez au minimum les sondes du §89 dans votre supervision (Zabbix/PRTG/SCOM) : **port 53, témoin interne, récursion, SRV AD, cohérence des serials**.

---

## 145. Pense-bête des commandes

### Zones
```powershell
Add-DnsServerPrimaryZone -Name "contoso.local" -ReplicationScope "Domain" -DynamicUpdate "Secure"
Add-DnsServerPrimaryZone -NetworkID "10.1.0.0/24" -ReplicationScope "Domain" -DynamicUpdate "Secure"
Add-DnsServerSecondaryZone -Name "contoso.local" -ZoneFile "contoso.local.dns" -MasterServers "10.0.0.10"
Add-DnsServerStubZone -Name "partenaire.local" -MasterServers "192.168.50.10"
Add-DnsServerConditionalForwarderZone -Name "partenaire.local" -MasterServers "192.168.50.10"
Get-DnsServerZone | Format-Table ZoneName, ZoneType, IsDsIntegrated
Suspend-DnsServerZone / Resume-DnsServerZone / Remove-DnsServerZone -Name "zone"
Sync-DnsServerZone -Name "contoso.local"
Export-DnsServerZone -Name "contoso.local" -FileName "export.txt"
```

### Enregistrements
```powershell
Add-DnsServerResourceRecordA -Name "www" -ZoneName "contoso.local" -IPv4Address "10.0.1.50" -TimeToLive 01:00:00
Add-DnsServerResourceRecordCName -Name "intranet" -ZoneName "contoso.local" -HostNameAlias "srv-web-01.contoso.local."
Add-DnsServerResourceRecordMX -Name "@" -ZoneName "contoso.local" -MailExchange "mx1.contoso.local." -Preference 10
Add-DnsServerResourceRecordSrv -Name "_ldap._tcp" -ZoneName "contoso.local" -DomainName "srv-ad-01.contoso.local." -Port 389 -Priority 10 -Weight 100
Add-DnsServerResourceRecordPtr -Name "50" -ZoneName "1.10.in-addr.arpa" -PtrDomainName "www.contoso.local."
Add-DnsServerResourceRecordTXT -Name "@" -ZoneName "contoso.local" -DescriptiveText "v=spf1 mx -all"
Get-DnsServerResourceRecord -ZoneName "contoso.local" -RRType A
Remove-DnsServerResourceRecord -ZoneName "contoso.local" -Name "www" -RRType A -RecordData "10.0.1.50" -Force
```

### Transferts, aging, redirecteurs
```powershell
Set-DnsServerZoneTransfer -Name "contoso.local" -SecondaryServers @("10.0.0.11")
Set-DnsServerZoneAging -Name "contoso.local" -Aging $true -RefreshInterval 7.00:00:00 -NoRefreshInterval 7.00:00:00
Set-DnsServerScavenging -ScavengingState $true -ScavengingInterval 7.00:00:00
Start-DnsServerScavenging -Force
Set-DnsServerForwarder -IPAddress @("1.1.1.1","9.9.9.9") -UseRootHint $false
Get-DnsServerRootHint | Format-Table NameServer, IPAddress
```

### Diagnostic
```powershell
Resolve-DnsName -Name "www.contoso.local" -Type A -Server "10.0.0.10" -DnsOnly
Resolve-DnsName -Name "www.contoso.local" -CacheOnly
Resolve-DnsName -Name "_ldap._tcp.dc._msdcs.contoso.local" -Type SRV
Test-DnsServer -IPAddress "10.0.0.10" -ZoneName "contoso.local"
Clear-DnsServerCache -Force; Clear-DnsClientCache
dnscmd SRV-DNS-01 /ClearCache; dnscmd SRV-DNS-01 /ZoneReload contoso.local
nltest /dsregdns; nltest /dnsgetdc:contoso.local; dcdiag /test:dns
Get-DnsServerDiagnostics | Format-List
```

### Stratégies DNS & DNSSEC
```powershell
Add-DnsServerClientSubnet -Name "Subnet-LAN" -IPv4Subnet "10.0.0.0/8"
Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "Scope-Interne"
Add-DnsServerQueryResolutionPolicy -Name "P1" -Action ALLOW -ClientSubnet "EQ,Subnet-LAN" -ZoneName "contoso.com" -ZoneScope "Scope-Interne,1"
Add-DnsServerSigningKey -ZoneName "contoso.com" -KeyType KeySigningKey -CryptoAlgorithm RsaSha256
Set-DnsServerDnsSecZoneSetting -ZoneName "contoso.com" -SignWithNSEC3 $true
Get-DnsServerTrustAnchor
```

---

## 146. Quiz : 10 questions

1. Quelle est la différence entre une résolution **récursive** et une résolution **itérative**, et qui fait quoi ?
2. Citez 3 enregistrements SRV qu'un contrôleur de domaine enregistre automatiquement, avec leur rôle.
3. Pourquoi ne faut-il **jamais** mettre un DNS externe (ex : 8.8.8.8) dans la carte réseau d'un DC ?
4. Expliquez le mécanisme no-refresh / refresh avec des intervalles de 7 jours : quand un enregistrement créé à J0 devient-il éligible au scavenging ?
5. Quelle est la différence entre une **zone stub** et un **redirecteur conditionnel** ?
6. Dans quel cas préférer les **indications de racine** aux **redirecteurs** ?
7. Pourquoi un **CNAME à l'apex** d'une zone est-il interdit ?
8. Que vérifie-t-on avec `Test-DnsServer -Context "Forwarder"`, et que signifie un échec ?
9. Quel est le risque de mettre un compte dans le groupe **DnsAdmins** sur un DC qui est aussi serveur DNS ?
10. Un enregistrement doit changer d'IP dans 48 h pour une migration : détaillez la procédure TTL correcte.

---

## 147. Réponses du quiz

1. **Récursive** : le client demande au résolveur et attend la réponse finale (le résolveur fait tout le travail). **Itérative** : le résolveur interroge successivement racine → TLD → serveur autoritaire, chacun ne donnant qu'une indication (referral) vers l'étape suivante.
2. `_ldap._tcp.dc._msdcs` (localiser un DC, port 389), `_kerberos._tcp.dc._msdcs` (Kerberos, port 88), `_gc._tcp` (catalogue global, port 3268) — plus les variantes `_sites` par site AD, `_kpasswd`, `_ldap._tcp.pdc._msdcs` (émulateur PDC).
3. Parce que le DC doit résoudre les enregistrements SRV de l'AD en priorité. Un DNS externe ne connaît pas la zone AD → échecs de localisation de DC, ouvertures de session lentes, réplication en erreur. L'externe passe par les **redirecteurs**, jamais par la carte réseau.
4. J0→J7 : no-refresh (les rafraîchissements sont ignorés) ; J7→J14 : refresh (le client peut mettre à jour son timestamp) ; après J14 sans rafraîchissement : l'enregistrement est **périmé** et sera supprimé au prochain cycle de scavenging.
5. La **zone stub** ne contient que les NS de la zone distante et suit automatiquement leurs changements ; le **redirecteur conditionnel** envoie les requêtes vers des IP fixes qu'il faut maintenir à la main.
6. Quand on veut l'indépendance vis-à-vis d'un tiers (souveraineté, confidentialité des requêtes), quand le pare-feu autorise tout le 53 sortant, ou pour un serveur exposé où l'on ne veut aucune dépendance externe.
7. Parce qu'un nom avec CNAME ne peut porter **aucun autre** enregistrement (RFC), or l'apex porte déjà le SOA et les NS : la zone deviendrait invalide.
8. On teste que chaque redirecteur configuré répond correctement. Un échec = le redirecteur est injoignable/lent → à remplacer ou à investiguer (pare-feu, panne FAI), car il ralentit toute la résolution Internet (timeouts en cascade).
9. Un membre de DnsAdmins peut charger une DLL arbitraire dans le service DNS (`ServerLevelPluginDll`) → exécution de code **SYSTEM** sur le DC → compromission complète du domaine. DnsAdmins ≈ Domain Admins sur un DC/DNS.
10. **J-2** : baisser le TTL à 300 s (5 min). **Jour J** : changer l'IP. **J+2** (après 2× l'ancien TTL) : remonter le TTL à sa valeur normale. Baisser le TTL *pendant* la migration ne sert à rien car les caches ont déjà l'ancienne valeur.

---

## 148. Glossaire

| Terme | Définition |
|---|---|
| AD-integrated (zone) | Zone stockée dans Active Directory, multi-maîtres, répliquée par AD |
| Aging (vieillissement) | Mécanisme d'horodatage des enregistrements dynamiques |
| Anycast | Plusieurs serveurs partageant une même IP annoncée en BGP |
| Apex (sommet de zone) | Le nom de la zone elle-même (ex : `contoso.local`) |
| Autoritaire | Serveur qui possède les données d'une zone |
| AXFR / IXFR | Transfert de zone complet / incrémental |
| CAA | Enregistrement déclarant les CA autorisées à émettre pour le domaine |
| Cache locking | Protection contre l'écrasement prématuré des entrées en cache |
| Cache négatif | Mise en cache des réponses NXDOMAIN |
| Conditional forwarder | Renvoi des requêtes d'une zone vers des serveurs désignés |
| DnsUpdateProxy | Groupe AD permettant aux DHCP d'enregistrer sans devenir propriétaires |
| DNSSEC | Signatures cryptographiques garantissant authenticité et intégrité |
| DS (Delegation Signer) | Enregistrement chez le parent ancrant la chaîne DNSSEC |
| EDNS | Extensions du DNS (réponses > 512 octets, bits DNSSEC…) |
| Forwarder (redirecteur) | Serveur vers qui on transfère les requêtes récursives |
| Glue record | A/AAAA d'un NS situé dans la zone qu'il délègue |
| KSK / ZSK | Clés DNSSEC : signature des clés / signature de la zone |
| _msdcs | Sous-zone contenant les enregistrements de localisation des DC |
| Notify | Notification poussée du primaire vers les secondaires après changement |
| NSEC / NSEC3 | Preuve d'inexistence signée (NSEC3 : hachée, anti-énumération) |
| NXDOMAIN | Réponse « le nom n'existe pas » |
| QNAME / QTYPE | Nom et type demandés dans une requête |
| RCODE | Code de réponse (NOERROR, NXDOMAIN, SERVFAIL, REFUSED…) |
| Récursion | Le serveur fait tout le travail de résolution pour le client |
| Referral | Indication « demande au serveur suivant » en résolution itérative |
| RRL | Response Rate Limiting : limite anti-amplification DDoS |
| Round-robin DNS | Plusieurs A/AAAA sur un nom, retournés en rotation |
| Scavenging | Suppression automatique des enregistrements dynamiques périmés |
| Scope (zone scope) | Vue d'une zone dans les stratégies DNS (split-brain) |
| Serial (SOA) | Numéro de version de la zone, déclencheur des transferts |
| Split-brain | Même zone, réponses différentes selon le client (interne/externe) |
| SRV | Enregistrement de localisation de service (priorité, poids, port) |
| Stub (zone) | Zone ne contenant que les NS d'une zone distante |
| TTL | Durée de vie en cache d'un enregistrement |
| Trust anchor | Clé de confiance de départ d'un validateur DNSSEC |
| Zone | Portion de l'espace de noms administrée par un serveur |

---

## 149. Pour aller plus loin

**Pratiquer en labo :**
- Montez une maquette : 2 DC/DNS (Server 2022/2025 Core), une zone AD-integrated, un DHCP avec compte dédié, activez le scavenging et observez les Events 2501.
- Simulez chaque cas pratique du chapitre 12 (cassez, diagnostiquez, réparez) : c'est le meilleur entraînement.
- Testez le split-brain par stratégies DNS avec 2 scopes, puis validez depuis 2 subnets.

**Approfondir :**
- **dcdiag /test:dns /v** et **repadmin** : le DNS et l'AD sont indissociables, maîtrisez les deux diagnostics.
- **DNS sur TLS (DoT, port 853) / DNS sur HTTPS (DoH)** : chiffrement du dernier kilomètre — natif côté client Windows 11/Server 2022+, à évaluer selon votre politique de confidentialité.
- **gMSA** (Group Managed Service Account) pour le compte DHCP-DNS : mot de passe géré automatiquement par AD, sans expiration manuelle.
- **IPAM** (IP Address Management, rôle Windows Server) : inventaire centralisé IP/DNS/DHCP pour les parcs importants.
- **BIND 9** en secondaire de vos zones : interopérabilité (transferts TSIG) pour les environnements mixtes.
- RFC de référence : 1034/1035 (bases), 2136 (updates dynamiques), 1996 (notify), 4033–4035 (DNSSEC), 6840, 7766 (DNS sur TCP).

**Runbooks à écrire pour votre équipe :**
1. « Ajout d'un serveur DNS au site X » (checklist §141 adaptée).
2. « Migration d'IP d'un serveur » (procédure TTL §12 + checklist §114).
3. « Rollover KSK DNSSEC » (§72).
4. « Le DNS ne répond plus » (arbre de décision basé sur §90–99).
5. « Audit trimestriel DNS » (§142, à planifier dans l'agenda d'équipe).

*Fin du guide — bon courage, et que vos serials soient cohérents.*
