# Guide technique ultra-complet : DHCP sous Windows Server en entreprise

> **Public** : administrateurs systèmes, chefs de service infrastructures (angle métier "systèmes & énergies" : un plan d'adressage fiable = moins de tickets, moins d'interruptions de production).
> **Versions couvertes** : Windows Server 2019, 2022 et 2025 (le rôle DHCP est stable depuis 2012 R2 ; je signale chaque différence de version).
> **Environnements de référence** : contoso.local, plage 192.0.2.0/24 (TEST-NET-1, RFC 5737) — adapte à ton plan d'adressage réel.
> **Prérequis** : AD DS déployé, DNS interne fonctionnel, serveur membre du domaine, IPv4 (IPv6 traité en section 12bis).

---

## Sommaire

| Bloc | Sections | Contenu |
|------|----------|---------|
| A — Fondations | 1–12 | Rappels DORA, rôle du DHCP, installation, autorisation AD |
| B — Étendues et baux | 13–30 | Création d'étendues, exclusions, durée des baux, réservations |
| C — Options DHCP | 31–44 | Options standard, PXE/WDS, précédence serveur/étendue/réservation |
| D — Scopes avancés | 45–58 | Superscopes, multicast, stratégies, filtres MAC, classes |
| E — Haute disponibilité | 59–68 | Basculement hot standby / load balance, relais DHCP, VLANs |
| F — DNS dynamique & sauvegarde | 69–76 | DNS dynamique, compte dédié, export/import, migration |
| G — Dépannage | 77–91 | 15 cas pratiques commentés, journaux, audit |
| H — Sécurité | 92–100 | Rogue DHCP, DHCP snooping, durcissement |
| I — Bonnes pratiques | 101–108 | Dimensionnement, durées de bail, redondance, documentation |
| Annexes | A1–A6 | Checklists, pense-bête de poche, glossaire, quiz, erreurs classiques, pour aller plus loin |

---

# Bloc A — Fondations

## 1. Pourquoi le DHCP est critique en entreprise

Sans DHCP, chaque poste, imprimante, téléphone IP et capteur IoT devrait être configuré à la main : adresse IP, masque, passerelle, DNS. À 50 machines c'est pénible, à 500 c'est ingérable, à 2000 c'est une source permanente d'incidents.

Le DHCP (Dynamic Host Configuration Protocol, RFC 2131/2132) automatise l'attribution :

- **Adresse IPv4** (et IPv6 via DHCPv6)
- **Masque de sous-réseau**
- **Passerelle par défaut** (option 003)
- **Serveurs DNS** (option 006)
- **Nom de domaine DNS** (option 015)
- Et des dizaines d'options métier : PXE (066/067), NTP (042), WINS, proxy WPAD (252)...

En entreprise, le DHCP est presque toujours couplé à **Active Directory** (autorisation obligatoire, voir section 9) et au **DNS dynamique** (voir section 69). Un DHCP mal dimensionné ou non redondé = des utilisateurs qui ne peuvent plus se connecter au réseau au renouvellement de leur bail. C'est un service d'infrastructure au même niveau que DNS et AD.

## 2. Le cycle DORA : Discover, Offer, Request, Acknowledge

Tout échange DHCP tient en 4 messages. À connaître par cœur pour le dépannage :

```text
Client (0.0.0.0:68)                    Serveur DHCP (*:67)
       |                                         |
       |--- 1. DHCPDISCOVER (broadcast) --------->|
       |     "Qui peut me donner une adresse ?"   |
       |                                         |
       |<-- 2. DHCPOFFER (broadcast/unicast) ----|
       |     "Je propose 192.0.2.50, masque      |
       |      255.255.255.0, bail 8 jours"       |
       |                                         |
       |--- 3. DHCPREQUEST (broadcast) --------->|
       |     "J'accepte 192.0.2.50"               |
       |     (broadcast pour prévenir les        |
       |      autres serveurs qui ont offert)    |
       |                                         |
       |<-- 4. DHCPACK (broadcast/unicast) ------|
       |     "OK, c'est à toi pour 8 jours"      |
       |                                         |
       |=== Le client configure son IP ==========|
```

Détails à retenir :

| Message | Émetteur | Destination | Port src/dst | Contenu clé |
|---------|----------|-------------|--------------|--------------|
| DHCPDISCOVER | Client | Broadcast 255.255.255.255 | 68 → 67 | Transaction ID, MAC client |
| DHCPOFFER | Serveur | Broadcast ou unicast | 67 → 68 | IP proposée, masque, durée du bail, options |
| DHCPREQUEST | Client | Broadcast | 68 → 67 | IP acceptée (ou demande de renouvellement) |
| DHCPACK | Serveur | Broadcast ou unicast | 67 → 68 | Confirmation + options finales |

> **Astuce dépannage** : dans Wireshark, filtre `bootp`. Si tu vois le DISCOVER mais pas le OFFER, le serveur ne répond pas (service arrêté, étendue épuisée, filtre MAC, ou le broadcast ne traverse pas — relais DHCP manquant, voir section 64).

Variantes du cycle à connaître :

- **Renouvellement (T1 = 50 % du bail)** : le client envoie un DHCPREQUEST en **unicast** directement au serveur qui lui a donné le bail. Pas de DISCOVER.
- **Rebinding (T2 = 87,5 % du bail)** : si le serveur ne répond pas, le client rebroadcast un DHCPREQUEST à n'importe quel serveur.
- **Expiration** : le client recommence un DORA complet et doit arrêter d'utiliser l'adresse.
- **DHCPNAK** : le serveur refuse (ex. : le client demande une adresse d'un autre sous-réseau après un déplacement). Le client doit redémarrer un DORA.
- **DHCPRELEASE** : le client libère volontairement son bail (`ipconfig /release`).
- **DHCPINFORM** : le client a déjà une IP (configurée manuellement) et demande **uniquement les options** (DNS, domaine...). Le serveur répond par un ACK sans attribuer d'adresse.

## 3. Ports, protocoles et flux réseau du DHCP

```text
UDP 67  : serveur DHCP (écoute)
UDP 68  : client DHCP (écoute)
```

Règles pare-feu Windows (créées automatiquement à l'installation du rôle) :

```powershell
# Vérifier les règles pare-feu DHCP créées par le rôle
Get-NetFirewallRule -DisplayGroup "DHCP Server" | 
    Select-Object DisplayName, Enabled, Direction, Action
```

À retenir pour les **pare-feu intermédiaires** (entre VLANs) :

- Autoriser UDP 67/68 **dans les deux sens** entre le serveur DHCP et les agents de relais.
- Le trafic client→serveur initial est en broadcast : il ne traverse pas les routeurs sans agent de relais (section 64).

## 4. DHCP vs IP statique : que mettre en DHCP en entreprise

| Type d'équipement | Recommandation | Méthode |
|-------------------|----------------|---------|
| Postes utilisateurs (fixe/portable) | DHCP dynamique | Étendue standard |
| Postes en mobilité (Wi-Fi) | DHCP, bail court | Étendue dédiée, bail 8–24 h |
| Invités (Wi-Fi guest) | DHCP isolé, bail très court | VLAN + étendue dédiée, bail 1–4 h |
| Imprimantes / MFP (copieurs) | **Réservation DHCP** | Réservation par MAC (section 22) |
| Serveurs | IP statique **ou** réservation | Statique hors plage DHCP de préférence |
| Contrôleurs de domaine / DNS | **IP statique obligatoire** | Hors plage DHCP, documenté |
| Serveur DHCP lui-même | **IP statique obligatoire** | Évidemment |
| Téléphones IP | DHCP + option 150/156 (selon constructeur) | Étendue voix dédiée (VLAN voix) |
| Switchs / points d'accès (management) | Réservation ou statique | Réservation conseillée |
| Caméras / IoT / GTB | DHCP, VLAN dédié, bail long | Étendue IoT, options spécifiques |

> **Lien métier copieurs** : les MFP (multifonctions) doivent avoir une **adresse IP fixe** car les pilotes d'impression, les carnets d'adresses LDAP, les dossiers de numérisation SMB et les flux de fax pointent vers cette IP. Si l'IP du copieur change, tout le service impression tombe. La **réservation DHCP** (section 22) est la méthode recommandée : IP fixe gérée centralement, sans aller configurer chaque copieur à la main.

## 5. Architecture type d'un DHCP d'entreprise

```text
                    ┌─────────────────────────┐
                    │   Active Directory      │
                    │  + DNS (dynamique)      │
                    └────────────┬────────────┘
                                 │ autorisation
              ┌──────────────────┴──────────────────┐
              │                                     │
   ┌──────────▼──────────┐              ┌───────────▼──────────┐
   │  DHCP primaire      │◄────────────►│  DHCP secondaire     │
   │  SRV-DHCP-01        │  basculement │  SRV-DHCP-02         │
   │  192.0.2.10         │  (failover)  │  192.0.2.11          │
   └──────────┬──────────┘              └───────────┬──────────┘
              │                                     │
     ┌────────┴────────┐                   ┌────────┴────────┐
     │ Étendues :      │                   │ Réplicas        │
     │ - LAN 192.0.2.0 │                   │ (hot standby    │
     │ - WiFi 192.0.3.0│                   │  ou load bal.)  │
     │ - Voix 192.0.4.0│                   └─────────────────┘
     │ - Guest 192.0.5.0
     └────────┬────────┘
              │ agent de relais (IP helper)
              │ sur chaque VLAN / routeur
     ┌────────┴─────────────────────────────────┐
     │ VLAN 10 LAN │ VLAN 20 WiFi │ VLAN 30 Voix │
     └──────────────────────────────────────────┘
```

Principes :

1. **Toujours 2 serveurs DHCP** en basculement (section 59) — jamais un seul en production.
2. **Une étendue par VLAN/sous-réseau**.
3. **Agent de relais** sur chaque routeur/VLAN (section 64) — le broadcast DHCP ne traverse pas les routeurs.
4. **DNS dynamique** activé pour que chaque bail crée/met à jour l'enregistrement DNS (section 69).

## 6. Glossaire éclair des termes DHCP (version courte — le glossaire complet est en annexe A3)

- **Étendue (scope)** : plage d'adresses IP qu'un serveur peut distribuer sur un sous-réseau.
- **Bail (lease)** : attribution temporaire d'une IP à un client, avec une durée.
- **Réservation** : bail permanent lié à une adresse MAC.
- **Exclusion** : plage d'adresses **jamais** distribuée (équipements en IP statique).
- **Option** : paramètre réseau transmis avec le bail (passerelle, DNS...).
- **Superscope** : regroupement de plusieurs étendues (multinet sur un même segment).
- **Basculement (failover)** : réplication entre 2 serveurs DHCP (hot standby ou load balance).
- **Agent de relais** : service qui relaie les broadcasts DHCP entre VLANs.
- **Rogue DHCP** : serveur DHCP illégitime/pirate sur le réseau.

## 7. Installation du rôle DHCP — via le Gestionnaire de serveur (GUI)

1. **Gestionnaire de serveur** → **Gérer** → **Ajouter des rôles et fonctionnalités**.
2. Type d'installation : **Installation basée sur un rôle ou une fonctionnalité**.
3. Sélectionner le serveur (membre du domaine, IP statique déjà configurée).
4. Cocher **Serveur DHCP** → **Ajouter les fonctionnalités** requises (outils d'administration).
5. Suivant jusqu'à **Installer**. Fermer l'assistant.
6. **Post-installation obligatoire** : cliquer sur le drapeau jaune → **Terminer la configuration DHCP** → crée les groupes de sécurité (`DHCP Administrators`, `DHCP Users`) et **autorise** le serveur dans AD (demande des droits d'administrateur d'entreprise).

> ⚠️ Sans l'étape 6, le service DHCP **ne distribue aucune adresse** dans un domaine AD (protection anti-rogue, voir section 9).

## 8. Installation du rôle DHCP — via PowerShell (méthode recommandée)

```powershell
# 1. Installer le rôle + outils d'administration (RSAT DHCP)
Install-WindowsFeature -Name DHCP -IncludeManagementTools

# Vérifier l'installation
Get-WindowsFeature -Name DHCP | Select-Object Name, Installed, InstallState

# 2. Redémarrer le service si besoin
Restart-Service -Name DHCPServer

# 3. Vérifier que le service est en automatique et démarré
Get-Service -Name DHCPServer | Select-Object Name, Status, StartType

# 4. Autoriser le serveur dans Active Directory (droits Enterprise Admin requis)
Add-DhcpServerInDC -DnsName "srv-dhcp-01.contoso.local" -IPAddress 192.0.2.10

# 5. Vérifier l'autorisation
Get-DhcpServerInDC

# 6. Créer les groupes de sécurité (fait normalement par la post-installation GUI)
# Si install 100% PowerShell, les créer à la main :
# net localgroup "DHCP Administrators" /add
# net localgroup "DHCP Users" /add
```

Installation sur **Server Core** (sans GUI) — identique, c'est même le cas d'usage idéal :

```powershell
# Sur Server Core, tout se fait en PowerShell : le module DhcpServer est complet
Import-Module DhcpServer
Get-Command -Module DhcpServer | Measure-Object   # ~120 cmdlets disponibles
```

Désinstallation propre (si besoin) :

```powershell
# D'abord retirer l'autorisation AD, puis le rôle
Remove-DhcpServerInDC -DnsName "srv-dhcp-01.contoso.local" -IPAddress 192.0.2.10
Uninstall-WindowsFeature -Name DHCP -IncludeManagementTools -Restart
```

## 9. Autorisation DHCP dans Active Directory — le garde-fou anti-rogue

Dans un domaine AD, un serveur DHCP **non autorisé ne distribue pas d'adresses**. C'est la première défense contre les rogue DHCP (section 92).

Fonctionnement :

1. Au démarrage, le service DHCP envoie un broadcast `DHCPINFORM` pour détecter les autres serveurs DHCP du domaine.
2. Il interroge AD (`CN=NetServices,CN=Services,CN=Configuration,DC=...`) pour vérifier s'il est dans la liste des serveurs autorisés.
3. Si absent → le service **reste démarré mais ne répond à aucun DISCOVER**. L'observateur d'événements loggue l'événement **1046** ("Le serveur DHCP n'est pas autorisé").

```powershell
# Lister les serveurs DHCP autorisés dans le domaine
Get-DhcpServerInDC | Format-Table DnsName, IPAddress -AutoSize

# Autoriser (à exécuter avec un compte Enterprise Admin)
Add-DhcpServerInDC -DnsName "srv-dhcp-02.contoso.local" -IPAddress 192.0.2.11

# Retirer l'autorisation (serveur décommissionné)
Remove-DhcpServerInDC -DnsName "srv-dhcp-02.contoso.local" -IPAddress 192.0.2.11
```

> ⚠️ **En workgroup** (sans AD), il n'y a pas d'autorisation : n'importe quel serveur DHCP fonctionne. En entreprise avec AD, c'est obligatoire.

Vérification de l'état d'autorisation en console : **DHCP** → clic droit sur le serveur → si "Autoriser" est proposé, il ne l'est pas encore.

## 10. La console DHCP (dhcpmgmt.msc) — tour d'horizon

```powershell
# Ouvrir la console
dhcpmgmt.msc
```

Arborescence :

```text
DHCP
└── SRV-DHCP-01.contoso.local
    ├── Pool d'adresses IPv4
    │   └── Étendue [192.0.2.0] LAN-Bureaux
    │       ├── Pool d'adresses   (plage + exclusions)
    │       ├── Baux d'adresses   (baux actifs)
    │       ├── Réservations      (IP fixes par MAC)
    │       ├── Options d'étendue (003, 006, 015...)
    │       └── Stratégies        (filtrage par classe/VLAN)
    ├── Options de serveur        (s'appliquent à toutes les étendues)
    ├── Stratégies
    └── Filtres (Autoriser / Refuser)
```

Actions essentielles (clic droit) :

| Nœud | Actions clés |
|------|--------------|
| Serveur | Autoriser/retirer, sauvegarder/restaurer, configurer le basculement |
| Étendue | Activer/désactiver, créer, réconcilier, statistiques |
| Baux d'adresses | Afficher, supprimer un bail |
| Réservations | Ajouter (MAC + IP) |
| Options d'étendue | Configurer 003/006/015... |

## 11. Le module PowerShell DhcpServer — la vraie console de l'admin

En production, **tout se fait en PowerShell** : c'est scriptable, auditable, et identique sur Server Core.

```powershell
# Importer (automatique si le rôle est installé)
Import-Module DhcpServer

# Lister toutes les cmdlets par verbe
Get-Command -Module DhcpServer | Group-Object { $_.Name.Split('-')[0] } |
    Sort-Object Count -Descending | Format-Table Name, Count -AutoSize

# Familles principales :
#  Get-/Add-/Set-/Remove-DhcpServerv4Scope        -> étendues
#  Get-/Add-/Set-/Remove-DhcpServerv4Lease        -> baux
#  Get-/Add-/Set-/Remove-DhcpServerv4Reservation -> réservations
#  Get-/Set-DhcpServerv4OptionValue               -> options
#  Get-/Add-/Remove-DhcpServerv4ExclusionRange    -> exclusions
#  Get-/Add-DhcpServerv4Failover                  -> basculement
#  Get-/Add-DhcpServerv4Policy                    -> stratégies
#  Get-/Add-DhcpServerv4Filter                    -> filtres MAC
#  Export-/Import-DhcpServer                      -> sauvegarde
```

> 💡 Toutes les commandes de ce guide ciblent le serveur local par défaut. Pour cibler un serveur distant, ajoutez `-ComputerName "srv-dhcp-02.contoso.local"` (WinRM requis).

## 12. Journal des événements DHCP — où regarder en premier

| Journal | Chemin | Événements clés |
|---------|--------|-----------------|
| DHCP-Server | `Applications and Services Logs\Microsoft\Windows\DHCP-Server` | 1020 (étendue épuisée), 1046 (non autorisé), 1034 (basculement) |
| Système | `Windows Logs\System` source `DHCPServer` | Démarrage/arrêt du service |

```powershell
# Les 20 derniers événements DHCP-Server
Get-WinEvent -LogName "Microsoft-Windows-DHCP-Server/Admin" -MaxEvents 20 |
    Select-Object TimeCreated, Id, LevelDisplayName,
        @{N="Message";E={$_.Message.Substring(0,[Math]::Min(120,$_.Message.Length))}} |
    Format-Table -AutoSize

# Surveiller en continu les alertes d'épuisement (ID 1020)
Get-WinEvent -LogName "Microsoft-Windows-DHCP-Server/Admin" |
    Where-Object { $_.Id -eq 1020 } |
    Select-Object TimeCreated, Message | Format-List
```

## 12bis. DHCPv6 — ce qu'il faut savoir (sans s'y noyer)

Windows Server gère aussi le DHCPv6 (attribution d'adresses IPv6 + options comme DNS IPv6). En entreprise française typique, l'IPv6 interne reste rare ; le DHCPv6 sert surtout si tu déploies de l'IPv6 natif.

```powershell
# Créer une étendue DHCPv6 (exemple)
Add-DhcpServerv6Scope -Prefix 2001:db8:1:: -Name "LAN-IPv6" -Preference 255 -ValidLifeTime 8.00:00:00 -PreferredLifeTime 6.00:00:00

# Options DHCPv6 (ex : DNS IPv6 = option 23)
Set-DhcpServerv6OptionValue -ScopeId 2001:db8:1:: -OptionId 23 -Value 2001:db8::53
```

> Ce guide se concentre sur **DHCPv4**, qui reste le standard en entreprise. Les concepts (baux, réservations, basculement) sont transposables.

---

# Bloc B — Étendues et baux

## 13. Concevoir son plan d'adressage avant de créer la moindre étendue

Règle d'or : **on ne crée pas d'étendue sans plan d'adressage écrit et validé**. Modèle de découpage type pour une PME/ETI :

| VLAN | Usage | Sous-réseau | Passerelle | Plage DHCP | Exclusions | Bail |
|------|-------|-------------|------------|------------|------------|------|
| 10 | LAN bureaux | 192.0.2.0/24 | 192.0.2.1 | .50 – .200 | .1 – .49 (infra) | 8 jours |
| 20 | Wi-Fi corporate | 192.0.3.0/24 | 192.0.3.1 | .50 – .200 | .1 – .49 | 1 jour |
| 30 | Voix (ToIP) | 192.0.4.0/24 | 192.0.4.1 | .50 – .200 | .1 – .49 | 8 jours |
| 40 | Invités | 192.0.5.0/24 | 192.0.5.1 | .50 – .200 | .1 – .49 | 4 heures |
| 50 | Imprimantes/MFP | 192.0.6.0/24 | 192.0.6.1 | — (réservations) | tout sauf réservations | — |
| 60 | Serveurs | 192.0.7.0/24 | 192.0.7.1 | — (statique) | tout | — |
| 70 | IoT/GTB | 192.0.8.0/24 | 192.0.8.1 | .50 – .200 | .1 – .49 | 30 jours |

> ⚠️ 192.0.2.0/24, 198.51.100.0/24, 203.0.113.0/24 sont des plages **TEST-NET réservées à la documentation** (RFC 5737) — ne les utilise jamais en production. En réel : 10.0.0.0/8, 172.16.0.0/12 ou 192.168.0.0/16.

## 14. Créer une étendue — console GUI

1. Console DHCP → **Pool d'adresses IPv4** → clic droit → **Nouvelle étendue**.
2. Nom : `LAN-Bureaux` ; description : `VLAN 10 — postes fixes et portables`.
3. Plage : `192.0.2.50` → `192.0.2.200`, masque `255.255.255.0` (longueur 24).
4. **Exclusions** : `192.0.2.1` – `192.0.2.49` (passerelle, switchs, points d'accès en statique).
5. Durée du bail : 8 jours (défaut).
6. Options : routeur `192.0.2.1`, DNS `192.0.2.53`, nom de domaine `contoso.local`.
7. **Activer l'étendue** : Oui.

## 15. Créer une étendue — PowerShell (reproductible)

```powershell
# Créer l'étendue LAN bureaux
Add-DhcpServerv4Scope `
    -Name "LAN-Bureaux" `
    -Description "VLAN 10 - Postes fixes et portables" `
    -StartRange 192.0.2.50 `
    -EndRange 192.0.2.200 `
    -SubnetMask 255.255.255.0 `
    -LeaseDuration (New-TimeSpan -Days 8) `
    -State Active

# Ajouter l'exclusion pour l'infrastructure en statique
Add-DhcpServerv4ExclusionRange `
    -ScopeId 192.0.2.0 `
    -StartRange 192.0.2.1 `
    -EndRange 192.0.2.49

# Configurer les options d'étendue (routeur, DNS, domaine)
Set-DhcpServerv4OptionValue `
    -ScopeId 192.0.2.0 `
    -Router 192.0.2.1 `
    -DnsServer 192.0.2.53, 192.0.2.54 `
    -DnsDomain "contoso.local"

# Vérifier
Get-DhcpServerv4Scope -ScopeId 192.0.2.0 | Format-List *
Get-DhcpServerv4ExclusionRange -ScopeId 192.0.2.0
Get-DhcpServerv4OptionValue -ScopeId 192.0.2.0
```

Créer **plusieurs étendues** d'un coup (script de provisioning) :

```powershell
$scopes = @(
    @{ Name="LAN-Bureaux";  Id="192.0.2.0"; Start="192.0.2.50";  End="192.0.2.200"; Router="192.0.2.1"; LeaseDays=8 }
    @{ Name="WiFi-Corp";    Id="192.0.3.0"; Start="192.0.3.50";  End="192.0.3.200"; Router="192.0.3.1"; LeaseDays=1 }
    @{ Name="Voix-ToIP";    Id="192.0.4.0"; Start="192.0.4.50";  End="192.0.4.200"; Router="192.0.4.1"; LeaseDays=8 }
    @{ Name="Guest";         Id="192.0.5.0"; Start="192.0.5.50";  End="192.0.5.200"; Router="192.0.5.1"; LeaseDays=0 }
)

foreach ($s in $scopes) {
    $lease = if ($s.LeaseDays -eq 0) { New-TimeSpan -Hours 4 } else { New-TimeSpan -Days $s.LeaseDays }
    Add-DhcpServerv4Scope -Name $s.Name -StartRange $s.Start -EndRange $s.End `
        -SubnetMask 255.255.255.0 -LeaseDuration $lease -State Active
    Set-DhcpServerv4OptionValue -ScopeId $s.Id -Router $s.Router `
        -DnsServer 192.0.2.53 -DnsDomain "contoso.local"
}
```

## 16. Activer / désactiver une étendue

```powershell
# Désactiver (maintenance : le serveur ne distribue plus sur cette étendue)
Set-DhcpServerv4Scope -ScopeId 192.0.2.0 -State Inactive

# Réactiver
Set-DhcpServerv4Scope -ScopeId 192.0.2.0 -State Active

# Vérifier l'état de toutes les étendues
Get-DhcpServerv4Scope | Select-Object Name, ScopeId, State, StartRange, EndRange
```

> 💡 Désactiver une étendue **ne supprime pas les baux existants** : les clients gardent leur IP jusqu'à expiration. Pour forcer le renouvellement : `ipconfig /release` + `/renew` côté client, ou réduire temporairement la durée du bail.

## 17. Exclusions : protéger les IP statiques

Les exclusions définissent des adresses **jamais attribuées** par le DHCP. Usage : passerelle, switchs, imprimantes en statique, serveurs, points d'accès.

```powershell
# Ajouter une exclusion
Add-DhcpServerv4ExclusionRange -ScopeId 192.0.2.0 -StartRange 192.0.2.1 -EndRange 192.0.2.20

# Ajouter une exclusion unique (ex : un serveur en .25)
Add-DhcpServerv4ExclusionRange -ScopeId 192.0.2.0 -StartRange 192.0.2.25 -EndRange 192.0.2.25

# Lister les exclusions
Get-DhcpServerv4ExclusionRange -ScopeId 192.0.2.0

# Supprimer une exclusion (quand l'équipement passe en réservation DHCP)
Remove-DhcpServerv4ExclusionRange -ScopeId 192.0.2.0 -StartRange 192.0.2.25 -EndRange 192.0.2.25
```

> ⚠️ **Erreur classique n°1** : oublier d'exclure les IP statiques → le DHCP attribue une IP déjà utilisée → **conflit d'adresses** (voir cas pratique n°1, section 77).

## 18. Durée du bail : comment la choisir

La durée du bail détermine la fréquence des renouvellements et la vitesse de récupération des adresses.

| Contexte | Durée recommandée | Pourquoi |
|----------|-------------------|----------|
| Postes fixes (LAN) | 8 jours (défaut) | Stable, peu de trafic DHCP |
| Portables / Wi-Fi corporate | 1 jour | Les utilisateurs bougent, libère vite les adresses |
| Wi-Fi invités | 4 heures (voire 1 h) | Forte rotation, évite l'épuisement |
| Téléphonie IP | 8 jours | Équipements fixes |
| IoT / GTB / capteurs | 30 jours | Équipements rarement redémarrés |
| Événementiel / formation (salle) | 8 heures | Pic ponctuel d'utilisateurs |

```powershell
# Modifier la durée du bail d'une étendue
Set-DhcpServerv4Scope -ScopeId 192.0.3.0 -LeaseDuration (New-TimeSpan -Days 1)

# Bail invité : 4 heures
Set-DhcpServerv4Scope -ScopeId 192.0.5.0 -LeaseDuration (New-TimeSpan -Hours 4)
```

> 💡 La durée du bail **ne s'applique qu'aux nouveaux baux et renouvellements**. Pour forcer l'application immédiate : supprimer les baux existants (section 20) — à faire en heure creuse.

## 19. Visualiser les baux

```powershell
# Tous les baux d'une étendue
Get-DhcpServerv4Lease -ScopeId 192.0.2.0 |
    Select-Object IPAddress, HostName, ClientId, LeaseExpiryTime, AddressState |
    Format-Table -AutoSize

# Baux actifs uniquement
Get-DhcpServerv4Lease -ScopeId 192.0.2.0 |
    Where-Object { $_.AddressState -eq "Active" }

# Trouver qui a une IP donnée (utile en dépannage)
Get-DhcpServerv4Lease -ScopeId 192.0.2.0 |
    Where-Object { $_.IPAddress -eq "192.0.2.87" } |
    Format-List IPAddress, HostName, ClientId, LeaseExpiryTime

# Compter les baux par état
Get-DhcpServerv4Lease -ScopeId 192.0.2.0 |
    Group-Object AddressState | Select-Object Name, Count
```

États possibles d'un bail (`AddressState`) :

| État | Signification |
|------|---------------|
| `Active` | Bail attribué et valide |
| `Expired` | Bail expiré, adresse récupérable |
| `Released` | Client a libéré (`ipconfig /release`) |
| `ReservationActive` / `ReservationInactive` | Réservation (voir section 22) |

## 20. Supprimer un bail (libérer une adresse)

```powershell
# Supprimer un bail précis (l'adresse redevient disponible)
Remove-DhcpServerv4Lease -ScopeId 192.0.2.0 -IPAddress 192.0.2.87

# Supprimer TOUS les baux expirés/inactifs d'une étendue (nettoyage)
Get-DhcpServerv4Lease -ScopeId 192.0.2.0 |
    Where-Object { $_.AddressState -ne "Active" -and $_.AddressState -notlike "Reservation*" } |
    Remove-DhcpServerv4Lease

# Forcer un client à renouveler : côté client
# ipconfig /release puis ipconfig /renew
```

> ⚠️ Supprimer un bail **actif** ne déconnecte pas le client immédiatement : il garde son IP jusqu'au prochain renouvellement (T1). Pour un effet immédiat, agir côté client (`ipconfig /release`).

## 21. Réconciliation : réparer la base des baux

La base DHCP (`dhcp.mdb` sous `C:\Windows\System32\dhcp`) peut se désynchroniser du registre (après un crash, une restauration...). La **réconciliation** compare et corrige.

```powershell
# Réconcilier une étendue (corrige les incohérences)
Invoke-DhcpServerv4ScopeReconciliation -ScopeId 192.0.2.0

# Réconcilier toutes les étendues
Get-DhcpServerv4Scope | ForEach-Object {
    Invoke-DhcpServerv4ScopeReconciliation -ScopeId $_.ScopeId
}
```

En console : clic droit sur l'étendue → **Réconcilier**. Le rapport indique les baux orphelins ou manquants.

## 22. Réservations : des IP fixes gérées par le DHCP

Une réservation lie **une adresse IP à une adresse MAC** : le client reçoit toujours la même IP, mais la configuration reste centralisée (passerelle, DNS modifiables sans toucher l'équipement).

```powershell
# Créer une réservation : copieur MFP du service comptabilité
Add-DhcpServerv4Reservation `
    -ScopeId 192.0.6.0 `
    -IPAddress 192.0.6.10 `
    -ClientId "00-1B-A9-3F-2C-7D" `
    -Name "MFP-Compta-RDC" `
    -Description "Copieur multifonction - Comptabilité RDC"

# Format du ClientId : MAC en hexadécimal séparé par des tirets
# Pour trouver la MAC d'un équipement : regarder l'étiquette, ou depuis un bail existant :
Get-DhcpServerv4Lease -ScopeId 192.0.6.0 |
    Where-Object { $_.HostName -like "*MFP*" } |
    Select-Object IPAddress, HostName, ClientId

# Lister les réservations
Get-DhcpServerv4Reservation -ScopeId 192.0.6.0 | Format-Table IPAddress, Name, ClientId

# Supprimer une réservation (le copieur est remplacé)
Remove-DhcpServerv4Reservation -ScopeId 192.0.6.0 -ClientId "00-1B-A9-3F-2C-7D"
```

> **Lien métier copieurs** : chaque MFP du parc = **une réservation DHCP** avec un nom explicite (`MFP-<Service>-<Étage>`). Avantages :
>
> 1. **IP fixe garantie** : les pilotes d'impression déployés par GPO, les dossiers de numérisation SMB (`\\srv-print\scans`) et les connecteurs LDAP pointent vers une IP stable.
> 2. **Zéro config sur le copieur** : à l'installation, le technicien met le MFP en DHCP, la réservation fait le reste.
> 3. **Changement de DNS/passerelle** : une modification de l'option d'étendue se propage à tous les MFP au prochain renouvellement — pas besoin de passer sur chaque machine.
> 4. **Traçabilité** : `Get-DhcpServerv4Reservation` = inventaire IP des copieurs, exportable en CSV pour la CMDB.
>
> ⚠️ L'adresse réservée doit être **dans la plage de l'étendue** mais **hors des baux dynamiques** : soit exclue de la plage dynamique, soit dans une étendue dédiée aux MFP (VLAN 50 du plan section 13 — recommandé).

Script : générer les réservations MFP depuis un CSV d'inventaire :

```powershell
# mfp.csv : IPAddress,MacAddress,Name,Location
# 192.0.6.10,00-1B-A9-3F-2C-7D,MFP-Compta-RDC,Comptabilite RDC
Import-Csv "C:\Admin\mfp.csv" | ForEach-Object {
    Add-DhcpServerv4Reservation `
        -ScopeId 192.0.6.0 `
        -IPAddress $_.IPAddress `
        -ClientId $_.MacAddress `
        -Name $_.Name `
        -Description "MFP - $($_.Location)"
}
```

## 23. Réservations : options spécifiques par réservation

Une réservation peut avoir **ses propres options**, qui écrasent celles de l'étendue (voir précédence, section 38) :

```powershell
# Le MFP compta utilise un serveur d'impression dédié comme passerelle de numérisation (exemple)
Set-DhcpServerv4OptionValue `
    -ReservedIP 192.0.6.10 `
    -OptionId 003 `
    -Value 192.0.6.2
```

Cas d'usage : MFP avec **serveur SMTP dédié** pour le scan-to-mail (option 069 — serveurs SMTP), ou **serveur de temps** spécifique.

## 24. Statistiques d'étendue : surveiller le taux d'utilisation

```powershell
# Statistiques d'une étendue
Get-DhcpServerv4ScopeStatistics -ScopeId 192.0.2.0 |
    Select-Object ScopeId, Free, InUse, PercentageInUse, Reserved, Pending

# Toutes les étendues triées par taux d'utilisation (les plus pleines d'abord)
Get-DhcpServerv4ScopeStatistics |
    Select-Object ScopeId,
        @{N="Utilisation_%";E={[math]::Round($_.PercentageInUse,1)}},
        Free, InUse |
    Sort-Object "Utilisation_%" -Descending |
    Format-Table -AutoSize

# Alerte si une étendue dépasse 85 % (à mettre en supervision, voir section 108)
$seuil = 85
Get-DhcpServerv4ScopeStatistics | Where-Object { $_.PercentageInUse -gt $seuil } |
    ForEach-Object {
        Write-Warning "Étendue $($_.ScopeId) à $([math]::Round($_.PercentageInUse,1)) % — prévoir une extension !"
    }
```

> 🎯 **Seuil d'alerte** : 80 % = vigilance, 90 % = action (étendre la plage ou réduire la durée des baux). À 100 %, les nouveaux clients **n'obtiennent plus d'IP** (cas pratique n°2, section 78).

## 25. Étendre ou réduire une étendue

```powershell
# Étendre : passer de .50-.200 à .50-.250
Set-DhcpServerv4Scope -ScopeId 192.0.2.0 -StartRange 192.0.2.50 -EndRange 192.0.2.250

# Réduire : attention, les baux existants hors nouvelle plage restent valides jusqu'à expiration
Set-DhcpServerv4Scope -ScopeId 192.0.2.0 -StartRange 192.0.2.50 -EndRange 192.0.2.150
```

> ⚠️ On ne peut étendre que **dans les limites du masque** de l'étendue. Pour passer de /24 à /23, il faut **recréer l'étendue** (supprimer + recréer) — planifier en heure creuse, avec basculement (section 59) pour éviter la coupure.

## 26. Supprimer une étendue (procédure propre)

```powershell
# 1. Désactiver d'abord (les clients gardent leur IP jusqu'à expiration)
Set-DhcpServerv4Scope -ScopeId 192.0.5.0 -State Inactive

# 2. Supprimer les baux si besoin de libérer vite
# Get-DhcpServerv4Lease -ScopeId 192.0.5.0 | Remove-DhcpServerv4Lease

# 3. Supprimer l'étendue
Remove-DhcpServerv4Scope -ScopeId 192.0.5.0
```

## 27. Pool d'adresses : lecture et interprétation

La console affiche le **pool d'adresses** = plage totale + exclusions + état. En PowerShell :

```powershell
# Vue synthétique : plage, exclusions, baux
$scope = Get-DhcpServerv4Scope -ScopeId 192.0.2.0
$stats = Get-DhcpServerv4ScopeStatistics -ScopeId 192.0.2.0
[PSCustomObject]@{
    Étendue     = $scope.Name
    Plage       = "$($scope.StartRange) - $($scope.EndRange)"
    Masque      = $scope.SubnetMask
    Bail        = $scope.LeaseDuration
    État        = $scope.State
    Libres      = $stats.Free
    Utilisées   = $stats.InUse
    Réservées   = $stats.Reserved
    Utilisation = "$([math]::Round($stats.PercentageInUse,1)) %"
} | Format-List
```

## 28. Baux : nettoyage automatique des baux expirés

Windows DHCP **ne supprime pas automatiquement** les baux expirés de la base (ils restent en état `Expired`). Pour une base propre :

```powershell
# Script de nettoyage hebdomadaire (à planifier, voir section 108)
foreach ($scope in Get-DhcpServerv4Scope) {
    $expired = Get-DhcpServerv4Lease -ScopeId $scope.ScopeId |
        Where-Object { $_.AddressState -eq "Expired" -or $_.AddressState -eq "Released" }
    if ($expired) {
        Write-Output "Étendue $($scope.ScopeId) : $($expired.Count) baux expirés supprimés"
        $expired | Remove-DhcpServerv4Lease
    }
}
```

## 29. Identifier un client par sa MAC (ClientId)

Le `ClientId` affiché par `Get-DhcpServerv4Lease` est la MAC en hexadécimal avec tirets. Pour retrouver un équipement :

```powershell
# Rechercher tous les baux d'une MAC (ex : un copieur qui change de VLAN)
$mac = "00-1B-A9-3F-2C-7D"
Get-DhcpServerv4Scope | ForEach-Object {
    Get-DhcpServerv4Lease -ScopeId $_.ScopeId |
        Where-Object { $_.ClientId -eq $mac }
} | Select-Object ScopeId, IPAddress, HostName, LeaseExpiryTime
```

> 💡 Sur le client Windows : `ipconfig /all` → "Adresse physique". Sur un copieur : page de configuration réseau (imprimée depuis le panneau) ou étiquette sous l'appareil.

## 30. Bonnes pratiques — Étendues et baux (récap)

1. ✅ Une étendue par VLAN, nommée explicitement (`VLAN10-LAN-Bureaux`).
2. ✅ Exclusions systématiques pour l'infrastructure en IP statique.
3. ✅ Durée de bail adaptée au profil (8 j fixe, 1 j Wi-Fi, 4 h invités).
4. ✅ Réservations pour les MFP/imprimantes, jamais d'IP statique "sauvage" sur les copieurs.
5. ✅ Supervision du taux d'utilisation (alerte à 80 %).
6. ✅ Documentation du plan d'adressage (section 107).
7. ❌ Jamais deux serveurs DHCP **indépendants** sur la même étendue sans basculement configuré (conflits garantis).
8. ❌ Jamais d'étendue sans agent de relais sur les VLANs distants.

---

# Bloc C — Options DHCP

## 31. Les options DHCP : principe général

Les options sont des paramètres réseau envoyés **avec** le bail. Le client les applique à sa pile TCP/IP. Chaque option a un **numéro** (RFC 2132) :

```text
DHCPACK : IP=192.0.2.50, masque=255.255.255.0, bail=8 jours
          + option 003 (routeur) = 192.0.2.1
          + option 006 (DNS)     = 192.0.2.53, 192.0.2.54
          + option 015 (domaine) = contoso.local
```

```powershell
# Lister toutes les options définies sur le serveur (catalogue)
Get-DhcpServerv4OptionDefinition | Select-Object OptionId, Name, Type | Format-Table -AutoSize
```

## 32. Les 6 options indispensables en entreprise

| Option | Nom | Valeur type | Obligatoire ? |
|--------|-----|-------------|---------------|
| 003 | Routeur (passerelle) | 192.0.2.1 | ✅ Oui |
| 006 | Serveurs DNS | 192.0.2.53, 192.0.2.54 | ✅ Oui |
| 015 | Nom de domaine DNS | contoso.local | ✅ Oui (AD) |
| 044 | Serveurs WINS/NBNS | 192.0.2.60 | ⚠️ Si WINS encore utilisé |
| 046 | Type de nœud WINS/NBT | 0x8 (hybride) | ⚠️ Avec 044 |
| 051 | Durée du bail | (gérée par l'étendue) | Automatique |

```powershell
# Configurer les options d'une étendue en une commande
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 `
    -Router 192.0.2.1 `
    -DnsServer 192.0.2.53, 192.0.2.54 `
    -DnsDomain "contoso.local" `
    -WinsServer 192.0.2.60 `
    -WinsNodeType 8

# Vérifier
Get-DhcpServerv4OptionValue -ScopeId 192.0.2.0 | Format-Table OptionId, Name, Value -AutoSize
```

## 33. Option 003 — Routeur / passerelle par défaut

```powershell
# Une seule passerelle (cas standard)
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -OptionId 3 -Value 192.0.2.1

# Plusieurs passerelles (redondance, ordre de préférence)
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -OptionId 3 -Value 192.0.2.1, 192.0.2.2
```

> ⚠️ La redondance de passerelle via l'option 003 est **médiocre** (pas de détection de panne rapide côté client). En entreprise, préférer **HSRP/VRRP** sur les routeurs : une seule IP virtuelle dans l'option 003.

## 34. Option 006 — Serveurs DNS

```powershell
# DNS primaire + secondaire (toujours 2 minimum en entreprise)
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -OptionId 6 -Value 192.0.2.53, 192.0.2.54
```

Ordre = ordre d'interrogation par le client. Mettre les **contrôleurs de domaine** (qui hébergent le DNS AD) en premier.

> ⚠️ Ne jamais mettre un DNS public (8.8.8.8) dans l'option 006 d'un poste du domaine : le client ne résoudrait plus les noms AD (`contoso.local`) → ouverture de session impossible.

## 35. Option 015 — Nom de domaine DNS

```powershell
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -OptionId 15 -Value "contoso.local"
```

Effets :

- Le client complète les noms courts (`ping srv-print` → `srv-print.contoso.local`).
- Utilisé pour l'enregistrement DNS dynamique (section 69).

## 36. Options 044/046 — WINS (héritage, encore présent)

WINS (Windows Internet Name Service) est obsolète mais survit dans les environnements avec de vieilles applications NetBIOS.

```powershell
# Serveurs WINS + type de nœud hybride (0x8 = H-node : WINS d'abord, broadcast ensuite)
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -OptionId 44 -Value 192.0.2.60
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -OptionId 46 -Value 8
```

Types de nœud : 1 = B-node (broadcast), 2 = P-node (WINS seul), 4 = M-node (mixte), **8 = H-node (hybride, recommandé)**.

> 💡 Si aucun serveur WINS dans l'entreprise : **ne pas configurer** 044/046. Inutile et source de timeouts NetBIOS.

## 37. Options 066/067 — Serveur de démarrage et PXE (lien WDS/MDT)

Le **démarrage PXE** permet d'installer des OS via le réseau (WDS — Windows Deployment Services, MDT). Le DHCP indique au client PXE **où** trouver le serveur de démarrage :

| Option | Rôle | Valeur type |
|--------|------|-------------|
| 066 | Nom du serveur de démarrage | `srv-wds.contoso.local` ou IP |
| 067 | Nom du fichier de démarrage | `boot\x64\wdsnbp.com` (BIOS) / `boot\x64\wdsmgfw.efi` (UEFI) |

```powershell
# Configurer PXE sur l'étendue (si WDS sur un autre serveur que le DHCP)
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -OptionId 66 -Value "srv-wds.contoso.local"
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -OptionId 67 -Value "boot\x64\wdsnbp.com"
```

> ⚠️ **Si WDS et DHCP sont sur le même serveur** : NE PAS configurer 066/067 ! Cocher plutôt dans WDS : *"Ne pas écouter sur le port 67"* + *"Configurer les options DHCP 060"*. Sinon conflit de ports (les deux écoutent en UDP 67).
>
> ⚠️ **UEFI vs BIOS** : le fichier de boot diffère (`wdsnbp.com` pour BIOS, `wdsmgfw.efi` pour UEFI). Avec un parc mixte, utiliser des **stratégies DHCP** (section 52) pour servir le bon fichier selon l'architecture annoncée par le client (option 93).

Vérification côté client PXE : au boot, le client affiche l'IP du serveur de démarrage. Si le client PXE n'obtient pas d'IP → voir cas pratique n°6 (section 82).

## 38. Précédence des options : serveur < étendue < réservation < stratégie < classe

Quand une même option est définie à plusieurs niveaux, **le niveau le plus spécifique gagne** :

```text
Options de SERVEUR  (toutes les étendues)
        │
        ▼  écrasé par
Options d'ÉTENDUE   (un sous-réseau)
        │
        ▼  écrasé par
Options de RÉSERVATION (un équipement)
        │
        ▼  écrasé par
STRATÉGIE / CLASSE (filtrage fin)
```

```powershell
# Option de serveur : s'applique à toutes les étendues qui ne la redéfinissent pas
Set-DhcpServerv4OptionValue -DnsServer 192.0.2.53, 192.0.2.54 -DnsDomain "contoso.local"
# (sans -ScopeId = niveau serveur)

# Lister les options de niveau serveur
Get-DhcpServerv4OptionValue | Format-Table OptionId, Name, Value -AutoSize
```

**Stratégie recommandée** :

- **Niveau serveur** : DNS (006), domaine (015) — identiques partout.
- **Niveau étendue** : routeur (003) — différent par VLAN.
- **Niveau réservation** : exceptions (un MFP avec SMTP dédié...).
- **Stratégies** : cas fins (PXE par architecture, classes de fournisseurs).

## 39. Options personnalisées (définies par l'utilisateur)

Pour les options non présentes dans le catalogue (ex. : option 150 pour Cisco TFTP, 156 pour Alcatel) :

```powershell
# 1. Créer la définition d'option (une fois, niveau serveur)
Add-DhcpServerv4OptionDefinition `
    -Name "Cisco TFTP Server" `
    -OptionId 150 `
    -Type IPAddress `
    -Description "Serveurs TFTP pour téléphones Cisco" `
    -MultiValued

# 2. Assigner la valeur sur l'étendue voix
Set-DhcpServerv4OptionValue -ScopeId 192.0.4.0 -OptionId 150 -Value 192.0.4.10, 192.0.4.11

# 3. Vérifier
Get-DhcpServerv4OptionValue -ScopeId 192.0.4.0 -OptionId 150
```

Types disponibles : `Byte`, `Word`, `DWord`, `String`, `IPAddress`, `BinaryData`, `EncapsulatedData`.

## 40. Options utiles — tableau de référence élargi

| Option | Nom | Usage entreprise |
|--------|-----|------------------|
| 001 | Masque de sous-réseau | Envoyé automatiquement |
| 003 | Routeur | Passerelle par défaut |
| 006 | Serveurs DNS | DNS AD |
| 012 | Nom d'hôte | Peut imposer un nom (rare) |
| 015 | Nom de domaine | Suffixe DNS |
| 042 | Serveurs NTP | Serveurs de temps (ex. : contrôleur de domaine) |
| 044/046 | WINS | Héritage NetBIOS |
| 051 | Durée du bail | Automatique |
| 058/059 | T1/T2 renouvellement | Automatique (50 % / 87,5 %) |
| 060 | Class ID fournisseur | PXEClient (WDS) |
| 066 | Serveur de boot | PXE / WDS |
| 067 | Fichier de boot | PXE / WDS |
| 093 | Architecture client | PXE (BIOS=0, UEFI x64=7, UEFI x64 HTTP=16) |
| 119 | Suffixes de recherche DNS | Liste de domaines |
| 121 | Routes statiques sans classe | Routes spécifiques poussées par DHCP |
| 150 | Serveurs TFTP (Cisco) | Téléphonie Cisco |
| 156 | Alcatel | Téléphonie Alcatel-Lucent |
| 252 | WPAD | URL du fichier PAC proxy |

```powershell
# Exemple : pousser des serveurs NTP (option 042) au niveau serveur
Set-DhcpServerv4OptionValue -OptionId 42 -Value 192.0.2.53

# Exemple : WPAD pour la configuration proxy automatique
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -OptionId 252 -Value "http://proxy.contoso.local/wpad.dat"
```

## 41. Option 121 — Routes statiques poussées par DHCP

Permet de distribuer des **routes spécifiques** sans toucher chaque poste (ex. : joindre un réseau distant via un routeur secondaire) :

```powershell
# Format : "destination/masque,passerelle" — syntaxe PowerShell :
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -OptionId 121 `
    -Value "192.0.9.0/24,192.0.2.254", "192.0.10.0/24,192.0.2.254"
```

> ⚠️ À manier avec précaution : une route erronée ici = trafic détourné pour **tout le VLAN**. Tester sur une réservation d'abord (précédence, section 38).

## 42. Supprimer / réinitialiser une option

```powershell
# Supprimer une option d'une étendue (retombe sur le niveau serveur s'il existe)
Remove-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -OptionId 252

# Supprimer une option de niveau serveur
Remove-DhcpServerv4OptionValue -OptionId 42

# Supprimer une définition d'option personnalisée
Remove-DhcpServerv4OptionDefinition -OptionId 150
```

## 43. Exporter la configuration des options (documentation)

```powershell
# Documenter toutes les options de toutes les étendues en CSV
Get-DhcpServerv4Scope | ForEach-Object {
    $scopeId = $_.ScopeId
    Get-DhcpServerv4OptionValue -ScopeId $scopeId | ForEach-Object {
        [PSCustomObject]@{
            Étendue  = $scopeId.IPAddressToString
            OptionId = $_.OptionId
            Nom      = $_.Name
            Valeur   = ($_.Value -join ", ")
        }
    }
} | Export-Csv "C:\Admin\dhcp_options.csv" -NoTypeInformation -Encoding UTF8
```

## 44. Bonnes pratiques — Options (récap)

1. ✅ DNS (006) + domaine (015) au **niveau serveur** : un seul endroit à maintenir.
2. ✅ Routeur (003) au **niveau étendue** : un par VLAN.
3. ✅ Exceptions au **niveau réservation** uniquement.
4. ✅ Documenter les options personnalisées (qui les utilise ? pourquoi ?).
5. ❌ Ne jamais mettre de DNS public sur les postes du domaine.
6. ❌ Ne pas configurer 066/067 si WDS est sur le même serveur que DHCP.

---

# Bloc D — Scopes avancés : superscopes, multicast, stratégies, filtres

## 45. Superscopes — regrouper des étendues (multinet)

Un **superscope** regroupe plusieurs étendues IPv4 logiques sur le **même segment physique**. Cas d'usage : migration d'adressage (ancienne plage + nouvelle plage cohabitent), ou besoin de plus d'adresses qu'un /24 sur un même VLAN.

```powershell
# 1. Créer deux étendues sur le même segment
Add-DhcpServerv4Scope -Name "LAN-Ancien"  -StartRange 192.0.2.50  -EndRange 192.0.2.200  -SubnetMask 255.255.255.0 -LeaseDuration (New-TimeSpan -Days 8) -State Active
Add-DhcpServerv4Scope -Name "LAN-Nouveau" -StartRange 192.0.11.50 -EndRange 192.0.11.200 -SubnetMask 255.255.255.0 -LeaseDuration (New-TimeSpan -Days 8) -State Active

# 2. Créer le superscope et y ajouter les étendues
Add-DhcpServerv4Superscope -SuperscopeName "Superscope-LAN" -ScopeId 192.0.2.0, 192.0.11.0

# 3. Vérifier
Get-DhcpServerv4Superscope -SuperscopeName "Superscope-LAN"
```

Comportement : le serveur attribue d'abord depuis la **première** étendue ; quand elle est pleine, il pioche dans la suivante.

> ⚠️ Prérequis réseau : le routeur du segment doit router les **deux** sous-réseaux (adresses secondaires sur l'interface, ou inter-VLAN). Sans ça, les clients de la 2e plage n'ont pas de passerelle fonctionnelle.

```powershell
# Retirer une étendue d'un superscope
Remove-DhcpServerv4Superscope -ScopeId 192.0.11.0

# Supprimer le superscope (les étendues sont conservées)
Remove-DhcpServerv4Superscope -SuperscopeName "Superscope-LAN"
```

## 46. Étendues de multidiffusion (multicast scopes)

Pour les applications de **multicast** (déploiement d'images WDS en multicast, visioconférence), le DHCP peut attribuer des adresses multicast (224.0.0.0 – 239.255.255.255, RFC 2365).

```powershell
# Créer une étendue multicast
Add-DhcpServerv4MulticastScope `
    -Name "Multicast-WDS" `
    -Description "Multicast pour deploiement WDS" `
    -StartRange 239.0.1.1 `
    -EndRange 239.0.1.254 `
    -TTL 32 `
    -LeaseDuration (New-TimeSpan -Days 1) `
    -State Active

# Lister les étendues multicast
Get-DhcpServerv4MulticastScope
```

> 💡 Usage réel : **rare** hors WDS/multicast vidéo. La plupart des entreprises n'en ont pas besoin. Le TTL limite la portée (32 = site local typiquement).

## 47. Stratégies DHCP — le filtrage fin (introduction)

Les **stratégies** permettent d'attribuer des IP/options différentes **au sein d'une même étendue** selon des critères : adresse MAC (préfixe constructeur), classe utilisateur, classe fournisseur, nom d'hôte...

Cas d'usage typiques :

- Servir le bon **fichier PXE** selon BIOS/UEFI (option 93).
- Réserver une sous-plage aux **téléphones IP** (préfixe MAC du constructeur).
- Appliquer des options différentes aux **MFP** (lien métier copieurs).

```powershell
# Exemple : stratégie PXE — clients UEFI x64 (option 93 = 7)
Add-DhcpServerv4Policy `
    -Name "PXE-UEFI-x64" `
    -ScopeId 192.0.2.0 `
    -Condition OR `
    -VendorClass Hex, "PXEClient:Arch:00007" `
    -Description "Clients PXE en UEFI x64"

# Assigner le fichier de boot UEFI à cette stratégie
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -PolicyName "PXE-UEFI-x64" `
    -OptionId 67 -Value "boot\x64\wdsmgfw.efi"
```

## 48. Stratégies par classe fournisseur (vendor class)

La **classe fournisseur** est déclarée par le client dans son DISCOVER (option 60). Valeurs courantes :

| Classe | Équipement |
|--------|------------|
| `PXEClient:Arch:00000` | PXE BIOS |
| `PXEClient:Arch:00007` | PXE UEFI x64 |
| `PXEClient:Arch:00016` | PXE UEFI x64 HTTP boot |
| `MSFT 5.0` | Client Windows |

```powershell
# Lister les classes fournisseurs connues du serveur
Get-DhcpServerv4Class -Type Vendor

# Stratégie complète : BIOS vs UEFI
Add-DhcpServerv4Policy -Name "PXE-BIOS" -ScopeId 192.0.2.0 `
    -Condition OR -VendorClass Hex, "PXEClient:Arch:00000"
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -PolicyName "PXE-BIOS" `
    -OptionId 67 -Value "boot\x64\wdsnbp.com"

Add-DhcpServerv4Policy -Name "PXE-UEFI" -ScopeId 192.0.2.0 `
    -Condition OR -VendorClass Hex, "PXEClient:Arch:00007", "PXEClient:Arch:00016"
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -PolicyName "PXE-UEFI" `
    -OptionId 67 -Value "boot\x64\wdsmgfw.efi"
```

## 49. Stratégies par classe utilisateur (user class)

La **classe utilisateur** (option 77) est configurée **côté client** — utile pour distinguer des populations :

```powershell
# Côté client Windows : définir sa classe utilisateur
ipconfig /setclassid "Ethernet" "Imprimantes"

# Côté serveur : créer la classe puis la stratégie
Add-DhcpServerv4Class -Name "MFP" -Type User -Data "Imprimantes" -Description "Multifonctions"

Add-DhcpServerv4Policy -Name "Policy-MFP" -ScopeId 192.0.6.0 `
    -Condition OR -UserClass Hex, "Imprimantes"

# Options spécifiques aux MFP : serveur SMTP dédié (option 69), par exemple
Set-DhcpServerv4OptionValue -ScopeId 192.0.6.0 -PolicyName "Policy-MFP" `
    -OptionId 69 -Value 192.0.6.25
```

> **Lien métier copieurs** : avec une classe utilisateur `MFP`, tu peux pousser automatiquement aux copieurs : serveur SMTP de scan-to-mail dédié, serveur LDAP du carnet d'adresses, ou un DNS interne spécifique — sans toucher chaque machine.

## 50. Stratégies par plage d'adresses (sous-plage dédiée)

Une stratégie peut restreindre l'attribution à une **sous-plage** de l'étendue :

```powershell
# Les téléphones (préfixe MAC 00:1B:2A = constructeur fictif) prennent dans .180-.200
Add-DhcpServerv4Policy `
    -Name "Telephones-IP" `
    -ScopeId 192.0.4.0 `
    -Condition OR `
    -MacAddress EQ, "001B2A*" `
    -StartRange 192.0.4.180 `
    -EndRange 192.0.4.200 `
    -Description "Telephones IP du constructeur X"
```

Ordre d'évaluation : les stratégies sont évaluées **dans l'ordre** (champ `ProcessingOrder`). La première qui correspond gagne.

```powershell
# Voir l'ordre des stratégies et le modifier
Get-DhcpServerv4Policy -ScopeId 192.0.4.0 | Select-Object Name, ProcessingOrder, Enabled
Set-DhcpServerv4Policy -Name "Telephones-IP" -ScopeId 192.0.4.0 -ProcessingOrder 1
```

## 51. Filtres MAC : liste d'autorisation (Allow) et de refus (Deny)

Les **filtres** agissent au **niveau serveur** (toutes les étendues) : autoriser ou refuser des MAC.

```powershell
# 1. Activer les listes (par défaut : Allow activée, Deny désactivée)
Set-DhcpServerv4FilterList -Allow $true -Deny $true

# 2. Ajouter une MAC en liste blanche (ex : seuls les équipements connus obtiennent une IP)
Add-DhcpServerv4Filter -List Allow -MacAddress "00-1B-A9-3F-2C-7D" -Description "MFP-Compta-RDC"

# 3. Bannir une MAC (ex : équipement personnel non autorisé)
Add-DhcpServerv4Filter -List Deny -MacAddress "AA-BB-CC-DD-EE-FF" -Description "Poste perso - refuse"

# 4. Lister
Get-DhcpServerv4Filter -List Allow
Get-DhcpServerv4Filter -List Deny
```

> ⚠️ **Allow = mode strict** : si la liste Allow est activée **et non vide**, seules les MAC listées obtiennent un bail. À n'activer que sur des VLANs contrôlés (ex. : VLAN imprimantes, VLAN serveurs de test). **Jamais** sur le LAN utilisateurs sans un processus d'enregistrement des MAC — sinon helpdesk submergé.
>
> 💡 Alternative moderne : **802.1X** sur les switchs (authentification avant DHCP) — plus robuste que le filtrage MAC (MAC spoofable en 30 secondes).

## 52. Stratégie PXE complète : BIOS + UEFI + HTTP boot

```powershell
# Nettoyer d'abord les options 066/067 globales si WDS est sur un autre serveur
# (elles restent utiles comme valeurs par défaut)

# BIOS legacy
Add-DhcpServerv4Policy -Name "PXE-BIOS" -ScopeId 192.0.2.0 -Condition OR `
    -VendorClass Hex, "PXEClient:Arch:00000"
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -PolicyName "PXE-BIOS" `
    -OptionId 66 -Value "srv-wds.contoso.local"
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -PolicyName "PXE-BIOS" `
    -OptionId 67 -Value "boot\x64\wdsnbp.com"

# UEFI x64 (TFTP)
Add-DhcpServerv4Policy -Name "PXE-UEFI" -ScopeId 192.0.2.0 -Condition OR `
    -VendorClass Hex, "PXEClient:Arch:00007"
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -PolicyName "PXE-UEFI" `
    -OptionId 66 -Value "srv-wds.contoso.local"
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -PolicyName "PXE-UEFI" `
    -OptionId 67 -Value "boot\x64\wdsmgfw.efi"

# UEFI HTTP boot (moderne, plus rapide que TFTP)
Add-DhcpServerv4Policy -Name "PXE-UEFI-HTTP" -ScopeId 192.0.2.0 -Condition OR `
    -VendorClass Hex, "PXEClient:Arch:00016"
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -PolicyName "PXE-UEFI-HTTP" `
    -OptionId 66 -Value "srv-wds.contoso.local"
Set-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -PolicyName "PXE-UEFI-HTTP" `
    -OptionId 67 -Value "boot\x64\wdsmgfw.efi"
```

## 53. Classes personnalisées : créer ses propres classes

```powershell
# Classe fournisseur personnalisée (ex : un constructeur de MFP)
Add-DhcpServerv4Class -Name "MFP-ConstructeurX" -Type Vendor `
    -Data "ConstructeurX-MFP" -Description "MFP du constructeur X"

# Classe utilisateur personnalisée
Add-DhcpServerv4Class -Name "Visiteurs" -Type User `
    -Data "Guest" -Description "Equipements invites"

# Lister
Get-DhcpServerv4Class | Select-Object Name, Type, Data
```

## 54. Désactiver / supprimer stratégies et filtres

```powershell
# Désactiver une stratégie (sans la supprimer)
Set-DhcpServerv4Policy -Name "PXE-BIOS" -ScopeId 192.0.2.0 -Enabled $false

# Supprimer
Remove-DhcpServerv4Policy -Name "PXE-BIOS" -ScopeId 192.0.2.0

# Retirer un filtre
Remove-DhcpServerv4Filter -List Deny -MacAddress "AA-BB-CC-DD-EE-FF"

# Désactiver une liste de filtres
Set-DhcpServerv4FilterList -Allow $false
```

## 55. Cas d'usage : VLAN imprimantes verrouillé (lien métier copieurs)

Objectif : sur le VLAN 50 (MFP), **seuls les copieurs enregistrés** obtiennent une IP.

```powershell
# 1. Étendue dédiée MFP (plage = que des réservations potentielles)
Add-DhcpServerv4Scope -Name "VLAN50-MFP" `
    -StartRange 192.0.6.10 -EndRange 192.0.6.100 `
    -SubnetMask 255.255.255.0 `
    -LeaseDuration (New-TimeSpan -Days 30) -State Active

# 2. Exclure toute la plage des attributions dynamiques...
Add-DhcpServerv4ExclusionRange -ScopeId 192.0.6.0 -StartRange 192.0.6.10 -EndRange 192.0.6.100

# 3. ... et ne servir que via réservations (une par MFP, voir section 22)
# (les adresses exclues restent attribuables par réservation !)

# 4. Options MFP : pas de passerelle Internet, DNS interne, SMTP dédié
Set-DhcpServerv4OptionValue -ScopeId 192.0.6.0 `
    -DnsServer 192.0.2.53 -DnsDomain "contoso.local" -OptionId 69 -Value 192.0.6.25
```

> 💡 Une adresse **exclue** peut quand même faire l'objet d'une **réservation** : c'est le pattern idéal pour un parc de copieurs — zéro attribution sauvage, 100 % traçable.

## 56. Tester une stratégie avant de la généraliser

```powershell
# Simuler : quel bail recevrait cette MAC ?
# (pas de cmdlet natif de simulation — méthode manuelle :)

# 1. Créer la stratégie désactivée
Add-DhcpServerv4Policy -Name "TEST-Telephone" -ScopeId 192.0.4.0 `
    -Condition OR -MacAddress EQ, "001B2A*" -Enabled $false

# 2. Vérifier la correspondance du pattern avec un bail existant
Get-DhcpServerv4Lease -ScopeId 192.0.4.0 |
    Where-Object { $_.ClientId -like "00-1B-2A*" } |
    Select-Object IPAddress, HostName, ClientId

# 3. Activer après validation
Set-DhcpServerv4Policy -Name "TEST-Telephone" -ScopeId 192.0.4.0 -Enabled $true
```

## 57. Ordre de traitement complet (précédence finale)

Quand un DISCOVER arrive, le serveur évalue dans cet ordre :

```text
1. Filtres MAC (Allow/Deny) → refusé ? STOP
2. Stratégies de l'étendue (dans l'ordre ProcessingOrder)
3. Réservation (MAC connue ?)
4. Classe utilisateur / fournisseur du client
5. Options d'étendue → options de serveur (héritage)
```

## 58. Bonnes pratiques — Scopes avancés (récap)

1. ✅ Superscope uniquement pour migration ou multinet justifié — pas comme solution au manque d'adresses (préférer un masque plus large).
2. ✅ Stratégies PXE testées sur une machine avant généralisation.
3. ✅ Filtre Allow réservé aux VLANs contrôlés (MFP, labos).
4. ✅ Nommer les stratégies explicitement (`PXE-UEFI`, `Telephones-ConstructeurX`).
5. ❌ Pas de chevauchement de plages entre stratégies d'une même étendue.
6. ❌ Pas de filtre Deny comme seule sécurité (MAC spoofable).

---

# Bloc E — Haute disponibilité et multi-VLAN

## 59. Basculement DHCP (failover) : principes

Depuis Windows Server 2012, le basculement **natif** remplace l'ancien cluster DHCP ou le "80/20". Deux modes :

| | Hot Standby (actif/passif) | Load Balance (actif/actif) |
|---|---|---|
| Principe | Primaire actif, secondaire en attente | Charge répartie (ratio configurable) |
| Ratio | — | 50/50 par défaut |
| Bascule | Automatique si primaire injoignable | Les deux servent en permanence |
| Cas d'usage | Secondaire moins puissant, simplicité | Deux serveurs équivalents, répartition |
| Réplication | Continue (baux + config) | Continue (baux + config) |

> 💡 **Recommandation** : **Load Balance 50/50** en général (les deux serveurs travaillent, pas de capacité dormante). **Hot Standby** si le secondaire est une VM légère ou sur un site distant.

Prérequis :

- Les 2 serveurs **autorisées dans AD** (section 9).
- L'étendue existe **déjà sur les deux serveurs** (même ScopeId) — ou la créer via le failover (elle sera répliquée).
- Connectivité TCP **647** entre les deux serveurs (port de réplication failover).

## 60. Configurer le basculement — Load Balance pas à pas

```powershell
# Sur le PRIMAIRE (srv-dhcp-01) : créer la relation de basculement
# L'étendue 192.0.2.0 doit exister sur les deux serveurs

Add-DhcpServerv4Failover `
    -Name "Failover-LAN" `
    -ScopeId 192.0.2.0 `
    -PartnerServer "srv-dhcp-02.contoso.local" `
    -Mode LoadBalance `
    -LoadBalancePercent 50 `
    -SharedSecret "MotDePasse-Partage-Securise-123!" `
    -ServerRole Active

# Vérifier
Get-DhcpServerv4Failover -ComputerName "srv-dhcp-01.contoso.local"
```

Paramètres clés :

- `-SharedSecret` : secret partagé pour authentifier la réplication (le changer régulièrement, le stocker en coffre).
- `-LoadBalancePercent` : % des requêtes traitées par le local (50 = équilibré).
- `-MaxClientLeadTime` : durée max d'extension de bail par le partenaire (défaut 1 h).

En GUI : console DHCP → clic droit sur l'étendue → **Configurer le basculement** → assistant (choix du partenaire, mode, secret partagé).

## 61. Configurer le basculement — Hot Standby pas à pas

```powershell
Add-DhcpServerv4Failover `
    -Name "Failover-WiFi" `
    -ScopeId 192.0.3.0, 192.0.4.0 `
    -PartnerServer "srv-dhcp-02.contoso.local" `
    -Mode HotStandby `
    -ReservePercent 5 `
    -SharedSecret "MotDePasse-Partage-Securise-123!" `
    -ServerRole Active `
    -SwitchoverInterval (New-TimeSpan -Minutes 60)

# -ReservePercent 5 : le standby garde 5 % des adresses pour les nouveaux clients
#                    si le primaire tombe (évite de tout donner d'un coup)
# -SwitchoverInterval : bascule automatique après 60 min d'indisponibilité du primaire
```

> ⚠️ En Hot Standby, le serveur en `Standby` ne répond pas aux DISCOVER tant que le primaire est joignable. Vérifier le rôle : `Get-DhcpServerv4Failover | Select-Object Name, Mode, ServerRole, State`.

## 62. Réplication du basculement : ce qui est répliqué (et ce qui ne l'est pas)

**Répliqué automatiquement** :

- Baux (leases) — en continu
- Réservations
- Exclusions
- Options d'étendue
- Stratégies
- Durée du bail

**NON répliqué** (à configurer manuellement sur chaque serveur) :

- Options de **serveur** (niveau serveur, section 38)
- Filtres MAC (Allow/Deny)
- Classes personnalisées
- Configuration du DNS dynamique (compte dédié, section 71)
- Paramètres d'audit/journalisation

```powershell
# Forcer une réplication immédiate
Invoke-DhcpServerv4FailoverReplication `
    -ComputerName "srv-dhcp-01.contoso.local" `
    -Name "Failover-LAN"

# Vérifier l'état de la réplication
Get-DhcpServerv4Failover -ComputerName "srv-dhcp-01.contoso.local" |
    Select-Object Name, Mode, State, ServerRole
# State doit être "Normal"
```

> 💡 **Piège classique** : on modifie une option de niveau **serveur** sur le primaire et on s'étonne qu'elle ne se réplique pas. Script de synchro manuelle en section 63.

## 63. Script : synchroniser les options de niveau serveur entre les 2 nœuds

```powershell
# À exécuter après toute modif d'option serveur / filtre / classe sur le primaire
$primary   = "srv-dhcp-01.contoso.local"
$secondary = "srv-dhcp-02.contoso.local"

# 1. Options de niveau serveur
$serverOptions = Get-DhcpServerv4OptionValue -ComputerName $primary
foreach ($opt in $serverOptions) {
    # Supprimer puis recréer côté secondaire (idempotent)
    Remove-DhcpServerv4OptionValue -ComputerName $secondary -OptionId $opt.OptionId -ErrorAction SilentlyContinue
}
# (recréation : à adapter selon tes options — exemple DNS + domaine)
Set-DhcpServerv4OptionValue -ComputerName $secondary `
    -DnsServer 192.0.2.53, 192.0.2.54 -DnsDomain "contoso.local" -OptionId 42 -Value 192.0.2.53

# 2. Filtres MAC
Set-DhcpServerv4FilterList -ComputerName $secondary `
    -Allow (Get-DhcpServerv4FilterList -ComputerName $primary).Allow `
    -Deny  (Get-DhcpServerv4FilterList -ComputerName $primary).Deny
Get-DhcpServerv4Filter -ComputerName $primary -List Allow | ForEach-Object {
    Add-DhcpServerv4Filter -ComputerName $secondary -List Allow `
        -MacAddress $_.MacAddress -Description $_.Description -ErrorAction SilentlyContinue
}

Write-Output "Synchronisation primaire -> secondaire terminee. Verifier avec :"
Write-Output "  Get-DhcpServerv4OptionValue -ComputerName $secondary"
```

## 64. DHCP sur plusieurs VLANs : l'agent de relais (relay agent)

**Le problème** : le DHCPDISCOVER est un **broadcast** (255.255.255.255). Les routeurs ne forwardent pas les broadcasts → sans relais, seul le VLAN du serveur DHCP obtient des adresses.

**La solution** : un **agent de relais** sur chaque VLAN/routeur qui convertit le broadcast en **unicast** vers le(s) serveur(s) DHCP, en ajoutant l'info du sous-réseau d'origine (option 82 / champ `giaddr`).

```text
Client VLAN 20 (192.0.3.0/24)
   │ DHCPDISCOVER (broadcast)
   ▼
┌──────────────────────────────┐
│ Routeur / Switch L3 VLAN 20  │
│  ip helper-address 192.0.2.10│  ← agent de relais
│  ip helper-address 192.0.2.11│  ← vers les 2 serveurs !
└──────────────┬───────────────┘
               │ unicast UDP 67, giaddr=192.0.3.1
               ▼
        Serveur DHCP (VLAN 10)
        → choisit l'étendue 192.0.3.0 grâce au giaddr
```

Le serveur utilise le champ **giaddr** (adresse de l'agent de relais) pour savoir **dans quelle étendue** piocher. D'où l'importance d'**une étendue par VLAN**.

## 65. Configurer l'agent de relais — côté routeur/switch (exemples constructeurs)

**Cisco IOS** :

```text
interface Vlan20
 description WiFi-Corporate
 ip address 192.0.3.1 255.255.255.0
 ip helper-address 192.0.2.10
 ip helper-address 192.0.2.11
```

**HP/Aruba (Comware)** :

```text
interface Vlan-interface20
 ip address 192.0.3.1 255.255.255.0
 dhcp select relay
 dhcp relay server-address 192.0.2.10
 dhcp relay server-address 192.0.2.11
```

> ⚠️ Toujours déclarer **les deux serveurs DHCP** dans le helper : si tu ne mets que le primaire et qu'il tombe, le VLAN n'a plus de DHCP malgré le basculement.

## 66. Configurer l'agent de relais — Windows Server (RRAS)

Si un serveur Windows fait office de routeur (RRAS), on peut y activer le relais DHCP :

```powershell
# Installer RRAS (routage uniquement, sans NAT/VPN si non nécessaire)
Install-WindowsFeature -Name Routing -IncludeManagementTools

# Via la console RRAS (rrasmgmt.msc) :
#   IPv4 -> Général -> clic droit -> Nouveau protocole de routage -> Agent de relais DHCP
#   Clic droit sur "Agent de relais DHCP" -> Propriétés -> ajouter 192.0.2.10 et 192.0.2.11
#   Clic droit -> Nouvelle interface -> choisir l'interface du VLAN client
```

> 💡 En pratique d'entreprise, le relais est sur les **switchs L3/routeurs**, pas sur Windows. RRAS = solution de secours ou labo.

## 67. Vérifier que le relais fonctionne

```powershell
# Côté serveur DHCP : les requêtes relayées apparaissent avec le giaddr
# Activer le log détaillé temporairement et filtrer :
Get-WinEvent -LogName "Microsoft-Windows-DHCP-Server/Operational" -MaxEvents 50 |
    Where-Object { $_.Message -like "*giaddr*" } |
    Select-Object TimeCreated, Message | Format-List

# Test client : depuis un poste du VLAN distant
# ipconfig /release puis ipconfig /renew
# Doit obtenir une IP de la bonne étendue (192.0.3.x pour le VLAN 20)
```

Si le client n'obtient rien : vérifier dans l'ordre (cas pratique n°4, section 80) :

1. Le helper est-il configuré sur la bonne interface VLAN ?
2. Le pare-feu laisse-t-il passer UDP 67/68 entre relais et serveurs ?
3. L'étendue du VLAN existe-t-elle et est-elle active ?
4. Y a-t-il des adresses libres ?

## 68. Bonnes pratiques — HA et multi-VLAN (récap)

1. ✅ 2 serveurs DHCP en Load Balance 50/50 (ou Hot Standby justifié).
2. ✅ Secret partagé robuste, stocké en coffre, rotation annuelle.
3. ✅ IP helper vers **les deux** serveurs sur chaque VLAN.
4. ✅ Une étendue par VLAN, même nommage (`VLAN20-WiFi-Corp`).
5. ✅ Surveillance de l'état du failover (`State = Normal`).
6. ✅ Script de synchro des paramètres non répliqués (section 63).
7. ❌ Jamais de helper vers un seul serveur.
8. ❌ Jamais deux étendues avec le même ScopeId sur des serveurs non en failover.

---

# Bloc F — DNS dynamique, sauvegarde, migration

## 69. DNS dynamique : pourquoi le DHCP doit mettre à jour le DNS

Sans DNS dynamique : un poste obtient `192.0.2.87` mais `poste-123.contoso.local` ne résout pas → impossible de joindre les machines par leur nom, la supervision échoue, les GPO ciblées par nom patinent.

Avec DNS dynamique : à chaque bail, le serveur DHCP **crée/met à jour** l'enregistrement A (et PTR) dans la zone AD.

```powershell
# Activer les mises à jour DNS dynamiques sur une étendue
Set-DhcpServerv4Scope -ScopeId 192.0.2.0 -DynamicDnsUpdateEnabled $true

# Vérifier
Get-DhcpServerv4Scope -ScopeId 192.0.2.0 | Select-Object Name, DynamicDnsUpdateEnabled
```

## 70. Configuration fine du DNS dynamique (onglet DNS de l'étendue)

Options disponibles (console → propriétés de l'étendue → onglet **DNS**, ou PowerShell) :

| Option | Recommandation |
|--------|----------------|
| Activer les mises à jour DNS dynamiques | ✅ Oui |
| Toujours mettre à jour (même si le client ne demande pas) | ✅ Oui (clients non-Windows, MFP, téléphones) |
| Supprimer l'enregistrement quand le bail expire/supprimé | ✅ Oui (évite les DNS fantômes) |
| Nom de protection (DHCID) | ✅ Oui (voir section 72) |

```powershell
# Configuration complète en PowerShell
Set-DhcpServerv4DnsSetting `
    -ComputerName "srv-dhcp-01.contoso.local" `
    -DynamicUpdates "Always" `
    -DeleteDnsRRonLeaseExpiry $true `
    -UpdateDnsRRForOlderClients $true `
    -DisableDynamicUpdateForDnsPTRRecord $false `
    -NameProtection $true

# Vérifier
Get-DhcpServerv4DnsSetting -ComputerName "srv-dhcp-01.contoso.local" | Format-List *
```

> **Lien métier copieurs** : les MFP ne mettent généralement **pas** à jour le DNS eux-mêmes (pas de client DNS dynamique). Avec *"Toujours mettre à jour"*, le serveur DHCP crée `mfp-compta-rdc.contoso.local` → tu peux joindre le copieur par son nom depuis les postes et la supervision. **Indispensable** pour un parc MFP propre.

## 71. Compte de service dédié pour les mises à jour DNS (sécurisation)

Par défaut, le DHCP met à jour le DNS avec le **compte machine** du serveur. Problème : si plusieurs serveurs DHCP (failover) mettent à jour les mêmes enregistrements, ou si un enregistrement est créé par un client, les permissions se mélangent → erreurs, enregistrements non supprimés.

**Bonne pratique** : créer un **compte de service dédié** (utilisateur AD simple, mot de passe robuste, *"le mot de passe n'expire jamais"* + documenté en coffre) :

```powershell
# 1. Créer le compte dans l'AD (via ADUC ou PowerShell)
New-ADUser -Name "svc-dhcp-dns" -SamAccountName "svc-dhcp-dns" `
    -UserPrincipalName "svc-dhcp-dns@contoso.local" `
    -AccountPassword (Read-Host -AsSecureString "Mot de passe") `
    -Enabled $true -PasswordNeverExpires $true `
    -Description "Compte de mises a jour DNS dynamiques pour DHCP"

# 2. L'ajouter au groupe "DnsUpdateProxy" (autorise la mise à jour des zones sécurisées)
Add-ADGroupMember -Identity "DnsUpdateProxy" -Members "svc-dhcp-dns"

# 3. Configurer le DHCP pour utiliser ce compte (sur CHAQUE serveur DHCP)
# En console : clic droit serveur -> Propriétés -> onglet Avancé -> Informations d'identification
# En PowerShell :
Set-DhcpServerDnsCredential -ComputerName "srv-dhcp-01.contoso.local" `
    -Credential (Get-Credential -UserName "contoso\svc-dhcp-dns" -Message "Compte DNS DHCP")
```

> ⚠️ Le groupe **DnsUpdateProxy** : ses membres peuvent créer des enregistrements DNS **non sécurisés**. Ne jamais y mettre un compte à privilèges. Et **ne pas** mettre les comptes machines des serveurs DHCP eux-mêmes dans ce groupe si tu utilises le compte dédié (sinon les protections ne s'appliquent pas correctement).
>
> ⚠️ Si le mot de passe du compte change/expire : les mises à jour DNS **échouent silencieusement** → DNS fantômes. Superviser l'événement **1056** (échec d'identification DNS).

## 72. Protection de nom (Name Protection / DHCID)

La **protection de nom** empêche qu'un client malveillant (ou un homonyme) **écrase** l'enregistrement DNS d'une autre machine. Le DHCP ajoute un enregistrement **DHCID** qui lie le nom à la MAC du client légitime.

```powershell
# Activer (inclus dans la config section 70 : -NameProtection $true)
# Vérifier :
Get-DhcpServerv4DnsSetting | Select-Object NameProtection
```

> 💡 À activer systématiquement sur les réseaux avec des équipements non gérés (Wi-Fi invités, salles de formation). Sur un LAN 100 % managé, c'est un plus de sécurité à coût nul.

## 73. Sauvegarde du DHCP — Export-DhcpServer (méthode moderne)

```powershell
# Sauvegarde complète : config + baux + (optionnel) secrets de basculement
Export-DhcpServer `
    -ComputerName "srv-dhcp-01.contoso.local" `
    -File "D:\Backup\DHCP\dhcp-backup.xml" `
    -Leases `
    -Force

# Inclure aussi les paramètres de sécurité (ACL) — Windows Server 2022/2025
Export-DhcpServer `
    -ComputerName "srv-dhcp-01.contoso.local" `
    -File "D:\Backup\DHCP\dhcp-backup-full.xml" `
    -Leases -Force
```

Contenu du fichier XML : étendues, options, réservations, baux, stratégies, filtres, config failover. **Lisible** (XML) → utile aussi comme documentation.

Automatiser (tâche planifiée quotidienne) :

```powershell
# Script C:\Admin\Backup-Dhcp.ps1
$date = Get-Date -Format "yyyy-MM-dd"
$dir  = "D:\Backup\DHCP"
if (-not (Test-Path $dir)) { New-Item -ItemType Directory -Path $dir | Out-Null }
Export-DhcpServer -ComputerName "srv-dhcp-01.contoso.local" `
    -File "$dir\dhcp-$date.xml" -Leases -Force
# Rotation : garder 30 jours
Get-ChildItem "$dir\dhcp-*.xml" | Where-Object { $_.LastWriteTime -lt (Get-Date).AddDays(-30) } |
    Remove-Item
```

```powershell
# Créer la tâche planifiée (quotidienne 02h00)
$action    = New-ScheduledTaskAction -Execute "powershell.exe" -Argument "-File C:\Admin\Backup-Dhcp.ps1"
$trigger   = New-ScheduledTaskTrigger -Daily -At 02:00
$principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -LogonType ServiceAccount
Register-ScheduledTask -TaskName "Backup DHCP quotidien" -Action $action -Trigger $trigger -Principal $principal
```

## 74. Sauvegarde — netsh dhcp (méthode classique, toujours valable)

```powershell
# Sauvegarde complète via netsh (format binaire propriétaire)
netsh dhcp server \\srv-dhcp-01 export D:\Backup\DHCP\dhcp-netsh.dat all

# Sauvegarde d'une étendue seule
netsh dhcp server \\srv-dhcp-01 scope 192.0.2.0 dump > D:\Backup\DHCP\scope-192.0.2.0.txt
# (le "dump" produit un script netsh rejouable — excellent pour la doc !)
```

> 💡 Le `dump` netsh est un **script texte rejouable** : parfait pour documenter une étendue ou la recréer à l'identique ailleurs.

## 75. Restauration — Import-DhcpServer

```powershell
# Restaurer sur un serveur (neuf ou après crash)
# Prérequis : rôle DHCP installé, serveur autorisé dans AD
Import-DhcpServer `
    -ComputerName "srv-dhcp-02.contoso.local" `
    -File "D:\Backup\DHCP\dhcp-backup.xml" `
    -Leases `
    -ScopeOverwrite `
    -Force

# -ScopeOverwrite : écrase les étendues existantes avec le même ScopeId
# Sans -Leases : restaure la config sans les baux (utile pour une migration propre)
```

Procédure de **reprise après crash** :

1. Installer Windows Server + rôle DHCP sur la machine de remplacement.
2. Lui donner la **même IP** que l'ancien serveur (ou mettre à jour les IP helpers).
3. Autoriser dans AD (`Add-DhcpServerInDC`).
4. `Import-DhcpServer` avec le dernier backup.
5. Reconfigurer le compte DNS dédié (section 71) — **non inclus** dans l'export.
6. Reconfigurer le basculement (section 59) — vérifier le secret partagé.
7. Tester : `ipconfig /renew` depuis un poste de chaque VLAN.

## 76. Migration vers un nouveau serveur (procédure sans coupure)

```powershell
# === Sur l'ANCIEN serveur (srv-dhcp-01) ===
Export-DhcpServer -ComputerName "srv-dhcp-01.contoso.local" `
    -File "\\srv-dhcp-02\Partage\dhcp-migration.xml" -Leases -Force

# === Sur le NOUVEAU serveur (srv-dhcp-02) ===
# 1. Installer le rôle + autoriser
Install-WindowsFeature -Name DHCP -IncludeManagementTools
Add-DhcpServerInDC -DnsName "srv-dhcp-02.contoso.local" -IPAddress 192.0.2.11

# 2. Importer (sans écraser si le nouveau est vierge, -ScopeOverwrite inutile)
Import-DhcpServer -ComputerName "srv-dhcp-02.contoso.local" `
    -File "\\srv-dhcp-02\Partage\dhcp-migration.xml" -Leases -Force

# 3. Basculer les IP helpers vers le nouveau (ou monter le failover entre les deux)
# 4. Désactiver les étendues de l'ancien, attendre l'expiration des baux, décommissionner
```

> 💡 Migration **sans coupure** : monter d'abord un **failover** entre l'ancien et le nouveau (section 59), laisser répliquer, puis passer le nouveau en primaire et retirer l'ancien. Zéro interruption.

---

# Bloc G — Dépannage : 15 cas pratiques commentés

> Méthode générale : **1)** le client obtient-il une IP ? (`ipconfig`) **2)** DORA visible ? (Wireshark `bootp` ou logs serveur) **3)** Quelle étape bloque ? (DISCOVER sans OFFER → serveur/relais ; OFFER sans REQUEST → client ; REQUEST sans ACK → étendue/options).

## 77. Cas n°1 — Conflit d'adresses IP (le classique)

**Symptômes** : message Windows *"Conflit d'adresse IP avec un autre système sur le réseau"*, connectivité intermittente sur deux machines.

**Causes** :

- Une IP statique configurée **dans la plage DHCP** sans exclusion (erreur n°1).
- Un équipement avec IP statique rebranché après un changement de plage.
- Deux serveurs DHCP indépendants sur le même segment.

**Diagnostic** :

```powershell
# 1. Vérifier si l'IP en conflit est dans une plage DHCP
Get-DhcpServerv4Scope | Where-Object {
    $_.StartRange -le [ipaddress]"192.0.2.87" -and $_.EndRange -ge [ipaddress]"192.0.2.87"
} | Select-Object Name, ScopeId, StartRange, EndRange

# 2. Vérifier les exclusions couvrent-elles les IP statiques connues ?
Get-DhcpServerv4ExclusionRange -ScopeId 192.0.2.0

# 3. Détecter la MAC du "squatteur" : ping puis arp
ping 192.0.2.87
arp -a | Select-String "192.0.2.87"

# 4. Le bail DHCP correspondant existe-t-il ?
Get-DhcpServerv4Lease -ScopeId 192.0.2.0 | Where-Object { $_.IPAddress -eq "192.0.2.87" }
```

**Résolution** :

```powershell
# Ajouter l'exclusion pour l'IP statique légitime
Add-DhcpServerv4ExclusionRange -ScopeId 192.0.2.0 -StartRange 192.0.2.87 -EndRange 192.0.2.87

# Si le conflit vient d'un bail DHCP attribué à tort : supprimer le bail
Remove-DhcpServerv4Lease -ScopeId 192.0.2.0 -IPAddress 192.0.2.87
```

**Prévention** : inventaire des IP statiques + exclusions systématiques (section 17). Activer la **détection de conflit** : le serveur ping l'adresse avant de l'attribuer (console → propriétés du serveur → onglet **Avancé** → *Nombre de tentatives de détection de conflit*, mettre 1 ou 2). Coût : léger délai à l'attribution.

```powershell
# Activer la détection de conflit (2 essais) — niveau serveur
Set-DhcpServerSetting -ComputerName "srv-dhcp-01.contoso.local" -ConflictDetectionAttempts 2
```

## 78. Cas n°2 — Étendue épuisée (plus d'adresses libres)

**Symptômes** : les nouveaux clients n'obtiennent pas d'IP (`ipconfig` → 169.254.x.x APIPA), les anciens fonctionnent. Événement **1020** dans le journal DHCP-Server.

**Diagnostic** :

```powershell
# Taux d'utilisation par étendue
Get-DhcpServerv4ScopeStatistics |
    Select-Object ScopeId,
        @{N="Utilisation_%";E={[math]::Round($_.PercentageInUse,1)}},
        Free, InUse, Pending |
    Sort-Object "Utilisation_%" -Descending | Format-Table -AutoSize

# Analyser : qui consomme ? (baux par âge, équipements inconnus)
Get-DhcpServerv4Lease -ScopeId 192.0.3.0 |
    Where-Object { $_.AddressState -eq "Active" } |
    Group-Object { $_.HostName -replace '\..*$','' } |
    Sort-Object Count -Descending | Select-Object -First 10 Name, Count
```

**Résolution (par ordre de préférence)** :

1. **Réduire la durée du bail** (ex. : Wi-Fi de 8 jours → 1 jour) : libère les adresses des visiteurs partis.
2. **Nettoyer** les baux expirés (section 28).
3. **Étendre la plage** (section 25) si le masque le permet.
4. **Recréer l'étendue** avec un masque plus large (/24 → /23) — heure creuse.
5. **Scinder** : créer un VLAN supplémentaire (solution d'architecture).

```powershell
# Exemple : Wi-Fi saturé, on passe le bail à 8 heures pour accélérer la rotation
Set-DhcpServerv4Scope -ScopeId 192.0.3.0 -LeaseDuration (New-TimeSpan -Hours 8)
```

**Prévention** : supervision du taux à 80 % (section 24), dimensionnement initial avec 30 % de marge (section 101).

## 79. Cas n°3 — Rogue DHCP : un serveur pirate distribue des adresses

**Symptômes** : des clients obtiennent une **passerelle ou des DNS bizarres** (ex. : DNS = 192.0.2.99 inconnu), pannes Internet intermittentes, parfois par vague (selon qui répond le plus vite au DISCOVER).

**Diagnostic** :

```powershell
# Sur un client affecté : qui est le serveur DHCP ?
ipconfig /all | Select-String "Serveur DHCP"

# Si ce n'est pas 192.0.2.10/11 → rogue confirmé. Trouver sa MAC :
arp -a | Select-String "192.0.2.99"

# Sur le réseau : capturer les OFFER (Wireshark, filtre bootp.option.dhcp == 2)
# L'IP source des OFFER suspects = le rogue
```

**Résolution immédiate** :

1. Identifier le port du switch via la MAC (`show mac address-table` côté switch).
2. **Shutdown** le port / débrancher l'équipement.
3. Sur les clients : `ipconfig /release` + `/renew` pour reprendre un bail légitime.

**Prévention (défense en profondeur, détails section 92-96)** :

- Autorisation AD (section 9) — bloque les serveurs Windows non autorisés.
- **DHCP snooping** sur les switchs — bloque les OFFER venant des ports non trusted.
- 802.1X — empêche les équipements non autorisés de parler au réseau.

> ⚠️ L'autorisation AD ne bloque que les serveurs **Windows**. Une box 4G, un routeur domestique ou un serveur Linux pirate **ne sont pas bloqués** par l'autorisation AD → le DHCP snooping est indispensable.

## 80. Cas n°4 — Un VLAN entier n'obtient plus d'IP (relais DHCP en cause)

**Symptômes** : tout un VLAN (ex. : Wi-Fi) n'a plus de DHCP, les autres VLANs fonctionnent.

**Checklist ordonnée** :

```powershell
# 1. L'étendue existe et est active ?
Get-DhcpServerv4Scope -ScopeId 192.0.3.0 | Select-Object Name, State

# 2. Reste-t-il des adresses ?
Get-DhcpServerv4ScopeStatistics -ScopeId 192.0.3.0 | Select-Object Free, InUse

# 3. Le service DHCP tourne-t-il sur les 2 serveurs ?
Get-Service -ComputerName "srv-dhcp-01.contoso.local" -Name DHCPServer
Get-Service -ComputerName "srv-dhcp-02.contoso.local" -Name DHCPServer
```

Puis côté réseau :

4. `show ip interface brief` / `display ip interface` : l'interface VLAN est-elle up ?
5. L'`ip helper-address` est-il toujours configuré ? (une mise à jour de switch peut l'effacer)
6. Le pare-feu inter-VLAN laisse-t-il passer UDP 67/68 ?
7. Test depuis le routeur : `ping 192.0.2.10` (joignabilité du serveur DHCP).

**Cas vécu** : après un remplacement de switch, la config `ip helper-address` n'a pas été reprise → tout le VLAN voix sans DHCP. **Leçon** : la config réseau des VLANs (helpers inclus) doit être versionnée/sauvegardée comme le reste.

## 81. Cas n°5 — Un seul client n'obtient pas d'IP

**Symptômes** : un poste reste en 169.254.x.x, ses voisins fonctionnent.

**Diagnostic côté client** :

```powershell
# 1. Le service client DHCP tourne ?
Get-Service -Name Dhcp

# 2. Carte réseau : câble, pilote, état ?
Get-NetAdapter | Select-Object Name, Status, LinkSpeed, MacAddress

# 3. Forcer un renouvellement en regardant les erreurs
ipconfig /release
ipconfig /renew
# Noter le message d'erreur exact

# 4. La MAC est-elle bannie par un filtre Deny ?
Get-DhcpServerv4Filter -List Deny | Where-Object { $_.MacAddress -eq "AA-BB-CC-DD-EE-FF" }

# 5. Y a-t-il une réservation pointant vers une autre MAC ? (conflit de réservation)
Get-DhcpServerv4Reservation -ScopeId 192.0.2.0 | Where-Object { $_.IPAddress -eq "192.0.2.87" }
```

**Causes fréquentes** : câble défectueux, port switch en erreur (errdisable), filtre Deny, 802.1X qui échoue, client avec IP statique résiduelle sur une autre plage, pilote NIC corrompu.

## 82. Cas n°6 — Le PXE ne fonctionne pas (pas d'IP au boot réseau)

**Symptômes** : au boot PXE, le client affiche *"No DHCP offers"* ou timeout, alors que Windows obtient une IP normalement.

**Particularité** : le client PXE (firmware) est **plus strict** que Windows sur les timings et les options.

Checklist :

```powershell
# 1. Les options 066/067 sont-elles configurées ? (si WDS séparé du DHCP)
Get-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -OptionId 66
Get-DhcpServerv4OptionValue -ScopeId 192.0.2.0 -OptionId 67

# 2. Les stratégies par architecture existent-elles ? (BIOS vs UEFI)
Get-DhcpServerv4Policy -ScopeId 192.0.2.0 | Select-Object Name, Enabled

# 3. WDS et DHCP sur le même serveur ? -> vérifier la config WDS (pas d'options 66/67 !)
# Console WDS -> Propriétés du serveur -> onglet DHCP :
#   [x] Ne pas écouter sur le port 67
#   [x] Configurer les options DHCP sur 060 (PXEClient)
```

Autres causes :

- **IP helper** : certains relais ne forwardent pas correctement les requêtes PXE (option 60). Sur Cisco : `ip helper-address` suffit normalement ; sinon ajouter `ip forward-protocol udp 4011`.
- **Délai** : le firmware PXE timeout vite ; si le serveur DHCP est lent (détection de conflit activée, section 77), le PXE échoue alors que Windows réussit. → Désactiver la détection de conflit ou augmenter le timeout PXE.
- **UEFI Secure Boot** : un fichier de boot non signé est refusé (pas un problème DHCP, mais symptôme similaire).

## 83. Cas n°7 — Le basculement ne réplique plus (State ≠ Normal)

**Symptômes** : les baux créés sur le primaire n'apparaissent pas sur le secondaire. `Get-DhcpServerv4Failover` → `State = CommunicationInterrupted` ou `PartnerDown`.

**Diagnostic** :

```powershell
# État détaillé
Get-DhcpServerv4Failover -ComputerName "srv-dhcp-01.contoso.local" |
    Format-List Name, Mode, State, ServerRole, PartnerServer

# Causes à vérifier dans l'ordre :
# 1. Connectivité TCP 647 entre les deux serveurs
Test-NetConnection -ComputerName "srv-dhcp-02.contoso.local" -Port 647

# 2. Le service DHCP tourne sur le partenaire ?
Get-Service -ComputerName "srv-dhcp-02.contoso.local" -Name DHCPServer

# 3. Le secret partagé correspond-il toujours ? (après un changement de mot de passe)
# 4. Horloges synchronisées ? (écart > 5 min = échec d'auth Kerberos/secret)
w32tm /query /status

# 5. Journal DHCP-Server : événements 1034/1035 (failover)
Get-WinEvent -LogName "Microsoft-Windows-DHCP-Server/Admin" -MaxEvents 30 |
    Where-Object { $_.Id -in 1034, 1035, 1040 } | Format-Table TimeCreated, Id, Message -AutoSize
```

**Résolution** :

```powershell
# Forcer la réplication
Invoke-DhcpServerv4FailoverReplication -ComputerName "srv-dhcp-01.contoso.local" -Name "Failover-LAN"

# Si le partenariat est cassé : le supprimer et le recréer
Remove-DhcpServerv4Failover -ComputerName "srv-dhcp-01.contoso.local" -Name "Failover-LAN"
# puis Add-DhcpServerv4Failover (section 60/61)
```

## 84. Cas n°8 — Les mises à jour DNS dynamiques échouent

**Symptômes** : les baux sont attribués mais les noms ne résolvent pas (`nslookup poste-123` → introuvable). Événement **1056** (échec d'authentification du compte DNS).

**Diagnostic** :

```powershell
# 1. Le DNS dynamique est-il activé sur l'étendue ?
Get-DhcpServerv4Scope -ScopeId 192.0.2.0 | Select-Object Name, DynamicDnsUpdateEnabled

# 2. Config globale
Get-DhcpServerv4DnsSetting | Format-List *

# 3. Le compte dédié est-il valide ? (mot de passe expiré ? verrouillé ?)
Get-ADUser -Identity "svc-dhcp-dns" -Properties PasswordExpired, LockedOut, PasswordLastSet |
    Select-Object Name, PasswordExpired, LockedOut, PasswordLastSet

# 4. Le compte est-il dans DnsUpdateProxy ?
Get-ADGroupMember -Identity "DnsUpdateProxy" | Select-Object Name

# 5. La zone DNS accepte-t-elle les mises à jour sécurisées ?
Get-DnsServerZone -Name "contoso.local" | Select-Object ZoneName, DynamicUpdate
# Doit être "Secure"
```

**Résolution** : réinitialiser le mot de passe du compte, le reconfigurer (`Set-DhcpServerDnsCredential`, section 71), vérifier que la zone est en mise à jour **sécurisée uniquement**.

## 85. Cas n°9 — Client avec APIPA 169.254.x.x : arbre de décision complet

L'adresse **APIPA** (169.254.0.0/16) = le client n'a reçu **aucune** réponse DHCP.

```text
Client en 169.254.x.x
│
├─ D'autres clients du même VLAN ont-ils une IP ?
│  ├─ NON → problème global : voir cas n°4 (relais) ou n°2 (étendue pleine)
│  └─ OUI → problème local au client : continuer
│
├─ ipconfig /renew → message ?
│  ├─ "Impossible de contacter le serveur DHCP" → réseau/câble/switch/802.1X
│  ├─ Timeout silencieux → filtre Deny ? (section 51) / réservation conflictuelle ?
│  └─ Erreur immédiate → service client DHCP arrêté ? (services.msc → DHCP Client)
│
├─ Wireshark (filtre bootp) sur le client :
│  ├─ DISCOVER émis, pas de OFFER → le broadcast ne sort pas (câble/port/VLAN)
│  ├─ OFFER reçu, pas de ACK → le client refuse (rare : option corrompue ?)
│  └─ Pas de DISCOVER → pile réseau HS (réinstaller le pilote, netsh winsock reset)
│
└─ Dernier recours : netsh int ip reset + reboot
```

```powershell
# Réinitialisation complète de la pile réseau côté client (à faire en local, pas à distance !)
netsh winsock reset
netsh int ip reset
# puis redémarrer
```

## 86. Cas n°10 — Baux fantômes après migration (doublons DNS/IP)

**Symptômes** : après une migration de serveur DHCP, des clients ont des IP en double, des noms DNS pointent vers de vieilles IP.

**Causes** : l'ancien serveur n'a pas été **désactivé** avant la mise en service du nouveau → deux DHCP actifs → baux divergents.

**Résolution** :

```powershell
# 1. S'assurer qu'un seul serveur est actif par étendue
# Sur l'ancien : désactiver les étendues
Set-DhcpServerv4Scope -ComputerName "srv-dhcp-OLD.contoso.local" -ScopeId 192.0.2.0 -State Inactive

# 2. Nettoyer les enregistrements DNS périmés (scavenging)
# Sur le DNS : activer le nettoyage (aging/scavenging) si pas fait
Set-DnsServerScavenging -ScavengingState $true -ScavengingInterval 7.00:00:00 -ApplyOnAllZones
Get-DnsServerZone -Name "contoso.local" |
    Set-DnsServerZoneAging -Aging $true -RefreshInterval 7.00:00:00 -NoRefreshInterval 7.00:00:00

# 3. Forcer le nettoyage manuel une fois
Start-DnsServerScavenging -Verbose
```

**Prévention** : procédure de migration avec failover (section 76), jamais deux serveurs indépendants.

## 87. Cas n°11 — Lenteurs à l'obtention du bail (logon lent)

**Symptômes** : les postes mettent 30–60 s à obtenir une IP au démarrage, l'ouverture de session est lente.

**Causes et réglages** :

| Cause | Correctif |
|-------|-----------|
| Détection de conflit activée (ping avant attribution) | La garder à 1 essai max, ou la désactiver sur LAN fiable |
| Serveur DHCP surchargé / distant (WAN) | Rapprocher (serveur local ou relais efficace) |
| STP sur les ports switch (30 s de blocking) | Activer **PortFast** sur les ports d'accès utilisateurs |
| 802.1X lent | Optimiser le timeout RADIUS |
| Client avec plusieurs NIC (Wi-Fi + Ethernet) | Désactiver la carte inutilisée |

```powershell
# Vérifier le temps de réponse du serveur (depuis un client)
Measure-Command { ipconfig /renew } | Select-Object TotalSeconds
# > 5 s = anormal sur un LAN local
```

> 💡 **PortFast** (ou équivalent) sur les ports des postes utilisateurs : le spanning-tree classique bloque le port 30–50 s au branchement, ce qui retarde le DORA. C'est la cause n°1 des "le réseau met une minute au démarrage".

## 88. Cas n°12 — Options DHCP non reçues par le client

**Symptômes** : le client a une IP mais pas la bonne passerelle / pas de DNS / pas de domaine.

**Diagnostic** :

```powershell
# 1. Quelles options le serveur envoie-t-il vraiment ?
Get-DhcpServerv4OptionValue -ScopeId 192.0.2.0 | Format-Table OptionId, Name, Value -AutoSize

# 2. Y a-t-il une STRATÉGIE qui écrase les options pour ce client ?
Get-DhcpServerv4Policy -ScopeId 192.0.2.0 | Format-Table Name, Enabled, ProcessingOrder

# 3. Une réservation avec options spécifiques ?
Get-DhcpServerv4Reservation -ScopeId 192.0.2.0 |
    Where-Object { $_.IPAddress -eq "192.0.2.87" }

# 4. Côté client : vider le cache et renouveler
ipconfig /release; ipconfig /flushdns; ipconfig /renew; ipconfig /all
```

**Piège** : une option définie au niveau **réservation** ou **stratégie** écrase l'option d'étendue (précédence, section 38/57). Vérifier les niveaux du plus spécifique au plus général.

## 89. Cas n°13 — Le serveur DHCP ne démarre plus (base corrompue)

**Symptômes** : le service DHCPServer ne démarre pas, événement **1014** (base de données corrompue).

**Résolution** :

```powershell
# 1. Arrêter le service
Stop-Service -Name DHCPServer -Force

# 2. Restaurer la base depuis la sauvegarde automatique
# Le DHCP fait une sauvegarde auto dans C:\Windows\System32\dhcp\backup
Copy-Item "C:\Windows\System32\dhcp\dhcp.mdb" "C:\Windows\System32\dhcp\dhcp.mdb.corrompu"
Copy-Item "C:\Windows\System32\dhcp\backup\new\dhcp.mdb" "C:\Windows\System32\dhcp\dhcp.mdb" -Force

# 3. Redémarrer et réconcilier
Start-Service -Name DHCPServer
Get-DhcpServerv4Scope | ForEach-Object {
    Invoke-DhcpServerv4ScopeReconciliation -ScopeId $_.ScopeId
}

# 4. Si pas de backup auto : restaurer l'export XML (section 75)
Import-DhcpServer -ComputerName "srv-dhcp-01.contoso.local" `
    -File "D:\Backup\DHCP\dhcp-backup.xml" -Leases -ScopeOverwrite -Force
```

**Prévention** : export quotidien automatisé (section 73) **+** sauvegarde auto native (vérifier qu'elle fonctionne : le dossier `backup\new` doit contenir un `dhcp.mdb` récent).

## 90. Cas n°14 — Épuisement par un équipement bavard (DHCP starvation)

**Symptômes** : étendue pleine en quelques heures alors que le nombre d'équipements est stable. Dans les baux : des dizaines d'entrées avec des **MAC aléatoires** ou le même hostname.

**Cause** : attaque **DHCP starvation** (outil type Yersinia) ou équipement défectueux qui redemande en boucle avec des MAC forgées.

**Diagnostic** :

```powershell
# Baux créés récemment en masse ?
Get-DhcpServerv4Lease -ScopeId 192.0.3.0 |
    Where-Object { $_.AddressState -eq "Active" } |
    Group-Object ClientId | Where-Object { $_.Count -gt 3 } |
    Select-Object Name, Count

# MAC aléatoires = beaucoup de ClientId uniques créés sur une courte période
# Vérifier les logs : rafale de DISCOVER depuis une même source (giaddr / port switch)
```

**Résolution** :

1. Identifier le port du switch (via l'IP source des DISCOVER ou le snooping).
2. Shutdown le port, identifier l'équipement (attaque ? IoT défectueux ? boucle ?).
3. Nettoyer les baux parasites, réduire temporairement le bail pour purger vite.
4. Activer **DHCP snooping + rate limiting** (section 94) pour bloquer les rafales.

## 91. Cas n°15 — Après un changement d'IP du serveur DHCP, plus rien ne fonctionne

**Symptômes** : changement d'IP du serveur DHCP (ou migration) → les clients ne renouvellent plus, les relais n'atteignent plus le serveur.

**Checklist post-changement** :

```powershell
# 1. Ré-autoriser dans AD avec la nouvelle IP (l'autorisation lie DNS + IP)
Remove-DhcpServerInDC -DnsName "srv-dhcp-01.contoso.local" -IPAddress 192.0.2.10
Add-DhcpServerInDC    -DnsName "srv-dhcp-01.contoso.local" -IPAddress 192.0.2.99

# 2. Mettre à jour TOUS les ip helper-address (chaque VLAN !)
# 3. Mettre à jour le DNS dynamique / compte de service si lié à l'IP
# 4. Mettre à jour la supervision (sondes vers la nouvelle IP)
# 5. Mettre à jour le failover (le partenariat référence le serveur : vérifier)
Get-DhcpServerv4Failover | Select-Object Name, PartnerServer, State

# 6. Forcer le renouvellement côté clients (GPO / script de logon temporaire)
# ipconfig /renew
```

> 💡 **Leçon** : l'IP d'un serveur DHCP est une **référence d'infrastructure** (helpers, supervision, doc). La changer = projet mini-migration. Préférer le failover pour absorber les changements (section 76).

## 91bis. Journaux DHCP : activer et exploiter l'audit

Le DHCP loggue quotidiennement dans `C:\Windows\System32\dhcp\DhcpSrvLog-*.log` (un fichier par jour de la semaine).

```powershell
# Vérifier que l'audit est activé
Get-DhcpServerAuditLog -ComputerName "srv-dhcp-01.contoso.local" |
    Select-Object Enable, Path

# Activer si besoin
Set-DhcpServerAuditLog -ComputerName "srv-dhcp-01.contoso.local" -Enable $true -Path "C:\Windows\System32\dhcp"

# Analyser le log du jour : codes d'événements (ID)
# 10 = nouveau bail, 11 = renouvellement, 12 = release, 13 = conflit détecté,
# 14 = réservation, 15 = NACK, 16 = suppression de bail
$log = Get-ChildItem "C:\Windows\System32\dhcp\DhcpSrvLog-$(Get-Date -Format 'ddd').log" |
    Select-Object -First 1
Import-Csv $log.FullName -Header ID,Date,Time,Description,IP,HostName,MAC | 
    Where-Object { $_.ID -eq 13 } |  # conflits détectés
    Format-Table Date, Time, IP, HostName, MAC -AutoSize
```

> 💡 Les logs d'audit sont la **source de vérité** pour : prouver quel équipement avait quelle IP à quel moment (traçabilité légale), détecter les starvations (rafales ID 10), et auditer les rogue (OFFER multiples).

---

# Bloc H — Sécurité

## 92. Menaces contre le DHCP en entreprise

| Menace | Description | Impact |
|--------|-------------|--------|
| Rogue DHCP | Serveur illégitime qui répond aux DISCOVER | Redirection du trafic (MITM), DNS menteur, coupure |
| DHCP starvation | Épuisement volontaire de l'étendue (MAC forgées) | Déni de service (plus d'IP pour les légitimes) |
| DHCP spoofing | Fausse réponse OFFER/ACK ciblée | MITM ciblé |
| Écoute passive | Capture des DISCOVER (noms de machines) | Reconnaissance réseau |

## 93. Défense n°1 : autorisation Active Directory (rappel)

Déjà couverte section 9 : un serveur Windows non autorisé ne distribue pas. **Limites** : ne bloque ni les serveurs Linux, ni les box/routeurs domestiques, ni les téléphones en partage de connexion. → Compléter avec le DHCP snooping.

## 94. Défense n°2 : DHCP snooping sur les switchs (indispensable)

Le **DHCP snooping** fait du switch un pare-feu DHCP : seuls les ports **trusted** (vers les serveurs DHCP légitimes / uplinks) peuvent envoyer des messages **serveur** (OFFER, ACK, NAK). Les ports **untrusted** (utilisateurs) ne peuvent qu'envoyer des messages **client** (DISCOVER, REQUEST).

**Cisco IOS** :

```text
! Activer globalement (par VLAN)
ip dhcp snooping
ip dhcp snooping vlan 10,20,30,40,50

! Ports vers les serveurs DHCP / uplinks : TRUSTED
interface GigabitEthernet1/0/1
 description Uplink vers SRV-DHCP-01
 ip dhcp snooping trust

! Optionnel : limiter le débit DHCP sur les ports utilisateurs (anti-starvation)
interface range GigabitEthernet1/0/2 - 48
 ip dhcp snooping limit rate 15

! Vérifier
show ip dhcp snooping
show ip dhcp snooping binding
```

**HP/Aruba (Comware)** :

```text
dhcp snooping enable
interface GigabitEthernet1/0/1
 dhcp snooping trust
```

Points d'attention :

- Activer le snooping **VLAN par VLAN** (ne pas oublier les nouveaux VLANs).
- Les ports d'**uplink inter-switch** doivent être trusted (sinon les OFFER légitimes venant d'un autre switch sont bloqués).
- Le **rate limiting** (15 paquets/s) bloque les starvations sans gêner l'usage normal.
- La table de **binding** (MAC ↔ IP ↔ port ↔ VLAN) sert aussi à **Dynamic ARP Inspection** et **IP Source Guard** — triple protection avec une seule config.

> **Lien réseau / métier** : en tant que chef de service systèmes & énergies, c'est typiquement un chantier **conjoint** avec l'équipe réseau : le DHCP snooping se configure sur les switchs, pas sur Windows. Prévoir une fenêtre de maintenance (un port mal classé = DHCP coupé sur ce port).

## 95. Défense n°3 : 802.1X (la vraie solution d'accès)

802.1X authentifie l'équipement **avant** même qu'il puisse envoyer un DISCOVER. Un rogue DHCP branché sur un port 802.1X ne peut pas parler au réseau.

```text
[Équipement] --EAP--> [Switch] --RADIUS--> [NPS/AD]
     │                     │
     │   Port bloqué tant   │
     │   que non authentifié│
     ▼                     ▼
  Pas de DHCP tant que l'authentification n'a pas réussi
```

Windows Server fournit le **NPS** (Network Policy Server, rôle à installer) comme serveur RADIUS. C'est un projet en soi, mais c'est **la** réponse structurelle aux rogue DHCP + équipements non autorisés.

## 96. Durcissement du serveur DHCP Windows

```powershell
# 1. Compte de service dédié avec droits minimaux (pas de compte admin du domaine)
# 2. Pare-feu : n'autoriser UDP 67/68 que depuis les relais et le LAN de management
# 3. Désactiver les services inutiles sur le serveur DHCP dédié
# 4. Audit des modifications : qui touche au DHCP ?
# Activer l'audit des cmdlets sensibles via les logs PowerShell (déjà natif)

# 5. Restreindre qui peut administrer le DHCP (groupes locaux)
net localgroup "DHCP Administrators"
# N'y mettre que les admins DHCP, pas "Domain Admins" entier si possible

# 6. Protéger les sauvegardes (elles contiennent tout le plan d'adressage)
icacls "D:\Backup\DHCP" /inheritance:r /grant:r "Administrateurs:F" /grant:r "SYSTEM:F"

# 7. Détection de conflit activée (section 77) — aussi une mesure anti-spoofing légère
Set-DhcpServerSetting -ConflictDetectionAttempts 1
```

Checklist de durcissement :

- [ ] Serveur dédié (ou cohabitation documentée : DC+DNS+DHCP acceptable en PME, jamais avec un serveur applicatif exposé)
- [ ] IP statique, hors plage DHCP, documentée
- [ ] Autorisation AD vérifiée (`Get-DhcpServerInDC`)
- [ ] Compte DNS dédié, mot de passe en coffre (section 71)
- [ ] DHCP snooping actif sur tous les VLANs utilisateurs
- [ ] Sauvegardes chiffrées/protégées, testées (restauration annuelle en labo)
- [ ] Journaux d'audit activés et centralisés (SIEM / syslog)
- [ ] Secret de basculement robuste, rotation annuelle

## 97. Détecter un rogue DHCP en continu (supervision)

```powershell
# Script de détection : interroge le réseau et compare les serveurs DHCP qui répondent
# Méthode simple : depuis une sonde, analyser les OFFER reçus
# (nécessite un outil type dhcpdump ou un script Python avec scapy — exemple de principe :)

# Alternative sans agent : surveiller les logs d'audit pour des OFFER multiples
# + sonde Nmap périodique depuis un poste de supervision :
# nmap --script broadcast-dhcp-discover -e Ethernet
# Comparer les "Server Identifier" répondants avec la liste autorisée (192.0.2.10, 192.0.2.11)

# Alerte Zabbix (UserParameter exemple) : compte les serveurs DHCP vus
# UserParameter=dhcp.rogue[*],powershell -File C:\Admin\Detect-RogueDhcp.ps1
```

> 💡 En pratique : une **sonde Nmap planifiée** (`broadcast-dhcp-discover`) par VLAN + alerte si un `Server Identifier` inconnu répond. Simple, efficace, sans agent.

## 98. Réagir à un incident rogue DHCP (procédure)

1. **Confirmer** : `ipconfig /all` sur un poste affecté → noter l'IP du serveur DHCP illégitime.
2. **Localiser** : `arp -a` → MAC → table MAC du switch → port.
3. **Isoler** : shutdown le port du switch (pas juste débrancher — garder la preuve).
4. **Assainir** : `ipconfig /release` + `/renew` sur les postes affectés (ou attendre le renouvellement T1).
5. **Chercher la cause** : équipement personnel ? box 4G ? boucle réseau ? malveillance ?
6. **Durcir** : vérifier le DHCP snooping sur le VLAN concerné, revoir la sensibilisation.
7. **Tracer** : consigner dans le registre des incidents (qui, quoi, quand, port, MAC).

## 99. DHCP et conformité : traçabilité des attributions

En cas d'incident de sécurité, on doit pouvoir dire **qui avait quelle IP à quel moment** :

```powershell
# Rechercher dans les logs d'audit archivés (exemple : IP suspecte le 2026-09-20)
$logs = Get-ChildItem "D:\Backup\DHCP\Logs\DhcpSrvLog-*.log"
foreach ($log in $logs) {
    Import-Csv $log.FullName -Header ID,Date,Time,Description,IP,HostName,MAC |
        Where-Object { $_.IP -eq "192.0.2.87" -and $_.ID -in 10,11 } |
        Select-Object Date, Time, HostName, MAC, Description
}
```

> ⚠️ **Conservation** : les logs d'audit DHCP doivent être archivés (au moins 1 an en entreprise, selon ta politique). Les fichiers natifs tournent sur 7 jours → les copier vers un archivage (script section 73 à étendre aux logs).

## 100. Bonnes pratiques — Sécurité (récap)

1. ✅ Autorisation AD systématique.
2. ✅ DHCP snooping sur **tous** les VLANs utilisateurs + rate limiting.
3. ✅ 802.1X à terme (feuille de route).
4. ✅ Sonde anti-rogue par VLAN.
5. ✅ Durcissement du serveur (section 96).
6. ✅ Logs archivés 1 an minimum.
7. ❌ Ne jamais brancher d'équipement réseau non géré (box, routeur domestique) sur le LAN.
8. ❌ Ne pas se reposer uniquement sur l'autorisation AD.

---

# Bloc I — Bonnes pratiques d'exploitation

## 101. Dimensionnement : combien d'adresses prévoir ?

Règle : **nombre d'équipements simultanés × 1,3** (30 % de marge), en tenant compte de la durée du bail :

| Profil | Équipements | Bail | Adresses à prévoir |
|--------|-------------|------|-------------------|
| Bureau 100 postes fixes | 100 | 8 j | 130 (/24 suffit) |
| Open space + portables 200 | 200 | 1 j | 260 → /23 ou 2×/24 |
| Wi-Fi visiteurs (événementiel) | 500 simultanés | 4 h | 650 → /23 |
| Site industriel / IoT | 300 capteurs | 30 j | 390 → /23 |

```powershell
# Calcul rapide : taille de plage nécessaire
$equipements = 200
$marge = 1.3
$adresses = [math]::Ceiling($equipements * $marge)
Write-Output "Prévoir $adresses adresses → $(if ($adresses -le 254) { '/24 suffit' } else { 'prévoir /23 ou plus' })"
```

> 💡 Le bail court **compense** partiellement le manque d'adresses (rotation rapide), mais ne remplace pas un dimensionnement correct.

## 102. Durées de bail : synthèse décisionnelle

| | Bail long (8–30 j) | Bail court (1–4 h) |
|---|---|---|
| Trafic DHCP | Faible | Plus élevé (renouvellements fréquents) |
| Récupération d'adresses | Lente | Rapide |
| Sensibilité aux pannes DHCP | Faible (les clients gardent leur IP) | Forte (renouvellement fréquent) |
| Idéal pour | Postes fixes, IoT, MFP | Wi-Fi invités, événementiel |

**Règle d'or** : plus le bail est court, plus la **redondance DHCP** (failover) devient critique.

## 103. Redondance : l'architecture minimale viable

```text
Minimum syndical en production :
  2 serveurs DHCP en Load Balance 50/50 (ou Hot Standby)
  + IP helpers vers les 2 sur chaque VLAN
  + Sauvegarde quotidienne testée
  + Supervision (taux d'utilisation + état du failover)

Niveau "robuste" :
  + DHCP snooping partout
  + DNS dynamique avec compte dédié
  + Documentation du plan d'adressage versionnée
  + Procédure de migration/restauration testée en labo 1×/an
```

## 104. Documentation : le plan d'adressage versionné

Modèle de document (à versionner en Git ou SharePoint) :

```markdown
# Plan d'adressage — Site principal (v2.3 — 2026-09-27)

## VLANs
| VLAN | Nom | Sous-réseau | Passerelle | DHCP (étendue) | Bail |
|------|-----|-------------|------------|----------------|------|
| 10 | LAN-Bureaux | 192.0.2.0/24 | 192.0.2.1 | .50-.200 | 8 j |

## IP statiques (hors DHCP)
| IP | Équipement | Rôle |
|----|------------|------|
| 192.0.2.1 | Routeur-RDC | Passerelle VLAN 10 |

## Réservations DHCP (MFP)
| IP | MAC | Nom | Localisation |
|----|-----|-----|--------------|
| 192.0.6.10 | 00-1B-A9-3F-2C-7D | MFP-Compta-RDC | Compta RDC |
```

Génération automatique depuis le DHCP :

```powershell
# Exporter le plan d'adressage réel en CSV (référence pour la doc)
Get-DhcpServerv4Scope | ForEach-Object {
    $s = $_
    $stats = Get-DhcpServerv4ScopeStatistics -ScopeId $s.ScopeId
    [PSCustomObject]@{
        Nom         = $s.Name
        SousReseau  = $s.ScopeId.IPAddressToString
        Plage       = "$($s.StartRange) - $($s.EndRange)"
        Masque      = $s.SubnetMask.IPAddressToString
        Bail        = $s.LeaseDuration.ToString()
        État        = $s.State
        Utilisation = "$([math]::Round($stats.PercentageInUse,1)) %"
    }
} | Export-Csv "C:\Admin\plan-adressage.csv" -NoTypeInformation -Encoding UTF8

# Exporter les réservations (inventaire MFP)
Get-DhcpServerv4Scope | ForEach-Object {
    Get-DhcpServerv4Reservation -ScopeId $_.ScopeId
} | Select-Object ScopeId, IPAddress, Name, ClientId, Description |
    Export-Csv "C:\Admin\reservations.csv" -NoTypeInformation -Encoding UTF8
```

## 105. Maintenance préventive : calendrier type

| Fréquence | Action |
|-----------|--------|
| Quotidien | Sauvegarde auto (script section 73), supervision des alertes |
| Hebdomadaire | Nettoyage des baux expirés (section 28), revue du taux d'utilisation |
| Mensuel | Revue des réservations (MFP remplacés ?), audit des filtres MAC |
| Trimestriel | Test de bascule du failover (couper le primaire 5 min en heure creuse) |
| Semestriel | Revue du plan d'adressage, rotation du secret de basculement |
| Annuel | Test de restauration en labo, revue des durées de bail, audit de sécurité |

```powershell
# Test de bascule trimestriel (procédure) :
# 1. Sur le primaire : Stop-Service DHCPServer
# 2. Depuis un client de chaque VLAN : ipconfig /release ; ipconfig /renew
# 3. Vérifier que le secondaire répond (ipconfig /all -> Serveur DHCP = .11)
# 4. Redémarrer le primaire : Start-Service DHCPServer
# 5. Vérifier l'état du failover : State = Normal
```

## 106. Supervision : quoi monitorer (Zabbix / autre)

| Sonde | Seuil | Moyen |
|-------|-------|-------|
| Service DHCPServer démarré | — | Agent Zabbix / WinRM |
| Taux d'utilisation par étendue | Alerte 80 %, critique 90 % | Script PowerShell (section 24) |
| État du failover = Normal | Alerte si ≠ Normal | `Get-DhcpServerv4Failover` |
| Événement 1020 (épuisement) | Alerte immédiate | Log Windows |
| Événement 1046 (non autorisé) | Alerte immédiate | Log Windows |
| Événement 1056 (échec DNS) | Alerte | Log Windows |
| Sonde anti-rogue par VLAN | Alerte si serveur inconnu | Nmap planifié (section 97) |
| Fraîcheur de la sauvegarde | Alerte si > 26 h | Vérification du fichier |

```powershell
# Script de supervision : retourne les étendues en alerte (format simple pour Zabbix)
$alertes = Get-DhcpServerv4ScopeStatistics | Where-Object { $_.PercentageInUse -gt 80 } |
    Select-Object @{N="Scope";E={$_.ScopeId.IPAddressToString}},
                  @{N="Pct";E={[math]::Round($_.PercentageInUse,1)}}
if ($alertes) { $alertes | ConvertTo-Json } else { Write-Output "OK" }
```

## 107. Gestion des changements : procédure type

Toute modification DHCP en production suit ce cycle :

1. **Demande** : qui, quoi, pourquoi, quel VLAN/étendue.
2. **Analyse d'impact** : combien de clients affectés ? besoin d'heure creuse ?
3. **Sauvegarde** : `Export-DhcpServer` avant toute modif (section 73).
4. **Test** : si possible sur une étendue de labo ou une réservation test.
5. **Application** : en heure creuse pour les changements d'options/passerelle.
6. **Vérification** : `ipconfig /renew` sur un échantillon, logs, supervision.
7. **Documentation** : mise à jour du plan d'adressage (section 104).
8. **Retour arrière** : `Import-DhcpServer` si problème (procédure connue d'avance).

## 108. Pense-bête d'exploitation (commandes du quotidien)

```powershell
# --- Quotidien ---
Get-DhcpServerv4ScopeStatistics | # taux d'utilisation
    Sort-Object PercentageInUse -Descending | Select-Object -First 5
Get-DhcpServerv4Failover | Select-Object Name, State  # état du basculement

# --- Chercher un équipement ---
# Par IP :
Get-DhcpServerv4Lease -ScopeId 192.0.2.0 | Where-Object IPAddress -eq 192.0.2.87
# Par nom :
Get-DhcpServerv4Scope | % { Get-DhcpServerv4Lease -ScopeId $_.ScopeId } |
    Where-Object HostName -like "*MFP*"

# --- Forcer le renouvellement d'un parc (via GPO / script de démarrage) ---
# ipconfig /renew

# --- Sauvegarde manuelle avant une modif ---
Export-DhcpServer -File "D:\Backup\DHCP\avant-modif-$(Get-Date -Format yyyyMMdd-HHmm).xml" -Leases -Force

# --- Redémarrer le service (rarement nécessaire) ---
Restart-Service -Name DHCPServer
```

---

# Annexes

## A1. Checklists

### Checklist — Mise en service d'un serveur DHCP

- [ ] Windows Server 2019/2022/2025 installé, à jour, IP statique configurée
- [ ] Serveur membre du domaine (ou workgroup assumé et documenté)
- [ ] Rôle DHCP installé (`Install-WindowsFeature DHCP -IncludeManagementTools`)
- [ ] Post-configuration effectuée (groupes de sécurité créés)
- [ ] Serveur **autorisé dans AD** (`Get-DhcpServerInDC` le liste)
- [ ] Plan d'adressage écrit et validé (section 13)
- [ ] Étendues créées (une par VLAN), actives
- [ ] Exclusions configurées pour les IP statiques
- [ ] Options : 003/006/015 au minimum, niveaux corrects (section 38)
- [ ] Réservations MFP/imprimantes créées et testées
- [ ] DNS dynamique activé + compte dédié (sections 69-71)
- [ ] Basculement configuré avec le 2e serveur, état Normal
- [ ] IP helpers configurés vers les 2 serveurs sur chaque VLAN
- [ ] DHCP snooping actif sur les switchs (tous les VLANs utilisateurs)
- [ ] Sauvegarde quotidienne planifiée et testée
- [ ] Supervision en place (section 106)
- [ ] Documentation à jour (plan d'adressage versionné)

### Checklist — Audit annuel du DHCP

- [ ] Restauration testée en labo depuis la sauvegarde
- [ ] Test de bascule du failover effectué
- [ ] Rotation du secret partagé de basculement
- [ ] Revue des durées de bail (toujours adaptées ?)
- [ ] Nettoyage des réservations obsolètes (équipements remplacés)
- [ ] Revue des filtres MAC (Allow/Deny toujours pertinents ?)
- [ ] Vérification du DHCP snooping (nouveaux VLANs couverts ?)
- [ ] Rotation du mot de passe du compte DNS dédié
- [ ] Archivage des logs d'audit vérifié
- [ ] Plan d'adressage à jour et versionné

### Checklist — Intervention "plus de DHCP sur un VLAN"

- [ ] Le problème est-il global ou localisé ? (un VLAN ? un client ? tout ?)
- [ ] Service DHCP démarré sur les 2 serveurs ?
- [ ] Étendue active et non pleine ?
- [ ] IP helper présent sur le VLAN ?
- [ ] Pare-feu UDP 67/68 OK entre relais et serveurs ?
- [ ] Failover en état Normal ?
- [ ] Rogue DHCP ? (`ipconfig /all` → Serveur DHCP)
- [ ] Logs : événements 1020 / 1046 / 1034 ?

## A2. Pense-bête de poche

```text
┌─────────────────────────────────────────────────────────────┐
│              PENSE-BÊTE DHCP — Windows Server                │
├─────────────────────────────────────────────────────────────┤
│ DORA : Discover → Offer → Request → Ack (UDP 67/68)         │
│ Renouvellement : T1 = 50 % du bail (unicast)                │
│                  T2 = 87,5 % (broadcast)                    │
├─────────────────────────────────────────────────────────────┤
│ Options : 003 passerelle │ 006 DNS │ 015 domaine            │
│           044/046 WINS   │ 066/067 PXE │ 042 NTP            │
├─────────────────────────────────────────────────────────────┤
│ Précédence : Serveur < Étendue < Réservation < Stratégie    │
├─────────────────────────────────────────────────────────────┤
│ Baux : 8 j fixe │ 1 j Wi-Fi │ 4 h invités │ 30 j IoT/MFP    │
├─────────────────────────────────────────────────────────────┤
│ Alertes : étendue > 80 % │ failover ≠ Normal │ event 1020   │
├─────────────────────────────────────────────────────────────┤
│ Commandes :                                                  │
│  Get-DhcpServerv4Scope                  (étendues)          │
│  Get-DhcpServerv4ScopeStatistics        (utilisation)       │
│  Get-DhcpServerv4Lease -ScopeId <id>    (baux)              │
│  Get-DhcpServerv4Reservation            (réservations)      │
│  Get-DhcpServerv4Failover               (basculement)       │
│  Export-DhcpServer -File <xml> -Leases  (sauvegarde)        │
├─────────────────────────────────────────────────────────────┤
│ Client : ipconfig /release │ /renew │ /all │ /setclassid    │
│ Rogue ? → ipconfig /all (Serveur DHCP) → arp -a → switch    │
├─────────────────────────────────────────────────────────────┤
│ APIPA 169.254.x.x = aucun OFFER reçu → voir arbre §85       │
└─────────────────────────────────────────────────────────────┘
```

## A3. Glossaire complet

| Terme | Définition |
|-------|------------|
| ACK (DHCPACK) | Message de confirmation finale du serveur |
| Agent de relais | Équipement/service qui relaie les broadcasts DHCP entre sous-réseaux (champ giaddr) |
| APIPA | Adressage automatique 169.254.0.0/16 quand aucun DHCP ne répond |
| Autorisation AD | Enregistrement du serveur DHCP dans Active Directory (obligatoire en domaine) |
| Bail (lease) | Attribution temporaire d'une IP à un client |
| Basculement (failover) | Réplication temps réel entre 2 serveurs DHCP |
| Classe fournisseur | Identifiant déclaré par le client (option 60, ex. : PXEClient) |
| Classe utilisateur | Étiquette configurée côté client (option 77) pour les stratégies |
| DHCID | Enregistrement DNS lié à la protection de nom |
| DHCP snooping | Fonction switch qui filtre les messages DHCP par port (trusted/untrusted) |
| DISCOVER | 1er message du client (broadcast : "qui peut me donner une IP ?") |
| DNS dynamique | Mise à jour automatique des zones DNS par le DHCP |
| DORA | Discover, Offer, Request, Acknowledge — le cycle DHCP |
| Étendue (scope) | Plage d'adresses distribuée sur un sous-réseau |
| Exclusion | Plage jamais attribuée par le DHCP |
| giaddr | Champ du paquet DHCP indiquant l'agent de relais (donc le sous-réseau d'origine) |
| Hot Standby | Mode de basculement actif/passif |
| IP helper-address | Commande routeur/switch désignant les serveurs DHCP (agent de relais) |
| Load Balance | Mode de basculement actif/actif avec répartition de charge |
| Multicast scope | Étendue d'adresses multicast (224.0.0.0/4) |
| NAK (DHCPNAK) | Refus du serveur (le client doit recommencer un DORA) |
| OFFER (DHCPOFFER) | Proposition d'adresse par le serveur |
| Option DHCP | Paramètre réseau transmis avec le bail (numéroté, RFC 2132) |
| Protection de nom | Mécanisme anti-écrasement des enregistrements DNS (DHCID) |
| Réconciliation | Réparation des incohérences entre la base des baux et le registre |
| Réservation | Attribution permanente d'une IP à une adresse MAC |
| REQUEST | 3e message : le client accepte (ou renouvelle) |
| Rogue DHCP | Serveur DHCP illégitime |
| Scavenging | Nettoyage automatique des enregistrements DNS périmés |
| Starvation | Attaque par épuisement de l'étendue (MAC forgées) |
| Stratégie | Règle d'attribution fine (MAC, classe, plage) au sein d'une étendue |
| Superscope | Regroupement de plusieurs étendues sur un même segment |
| T1 / T2 | Timers de renouvellement (50 % / 87,5 % du bail) |
| WPAD | Web Proxy Auto-Discovery (option 252) |

## A4. Quiz — 10 questions + réponses

**Q1. Quelles sont les 4 étapes du cycle DORA et qui émet chaque message ?**
> R : 1. DISCOVER (client, broadcast) — 2. OFFER (serveur) — 3. REQUEST (client, broadcast) — 4. ACK (serveur). Le REQUEST est en broadcast pour prévenir les autres serveurs ayant fait une offre.

**Q2. Pourquoi un serveur DHCP Windows non autorisé dans AD ne distribue-t-il pas d'adresses ?**
> R : Protection anti-rogue intégrée : au démarrage, le service vérifie son autorisation dans la partition Configuration de l'AD. Non autorisé → il reste démarré mais ignore les DISCOVER (événement 1046).

**Q3. Un client en Wi-Fi invités obtient une IP en 169.254.x.x. Citez 3 causes possibles dans l'ordre de vérification.**
> R : 1. Étendue pleine (bail trop long pour la rotation des visiteurs) — 2. Agent de relais (IP helper) manquant ou mal configuré sur le VLAN invités — 3. Service DHCP arrêté / étendue désactivée. (Voir arbre de décision section 85.)

**Q4. Quelle est la précédence des options DHCP entre les niveaux serveur, étendue, réservation et stratégie ?**
> R : Serveur < Étendue < Réservation < Stratégie. Le niveau le plus spécifique l'emporte toujours.

**Q5. Quelle différence entre Hot Standby et Load Balance pour le basculement DHCP ?**
> R : Hot Standby = actif/passif (le secondaire ne sert qu'en cas de panne du primaire, avec un % d'adresses réservées) ; Load Balance = actif/actif (les deux servent en permanence, charge répartie selon le ratio configuré, 50/50 par défaut).

**Q6. Pourquoi faut-il déclarer les DEUX serveurs DHCP dans l'ip helper-address ?**
> R : Si seul le primaire est déclaré et qu'il tombe, le relais n'envoie les DISCOVER qu'à un serveur mort → le VLAN n'a plus de DHCP malgré le basculement configuré côté serveurs.

**Q7. Un copieur (MFP) doit garder une IP fixe. Quelle méthode recommander et pourquoi pas une IP statique configurée sur le copieur ?**
> R : Réservation DHCP par adresse MAC : IP fixe garantie + gestion centralisée (passerelle/DNS modifiables sans toucher le copieur) + traçabilité (inventaire exportable). L'IP statique locale oblige à intervenir sur chaque copieur à chaque changement réseau et n'est pas visible dans l'inventaire DHCP.

**Q8. Qu'est-ce que le DHCP snooping et sur quel équipement se configure-t-il ?**
> R : Fonction des switchs (pas de Windows) : seuls les ports "trusted" (vers les serveurs DHCP légitimes) peuvent émettre des OFFER/ACK ; les ports utilisateurs "untrusted" ne peuvent qu'émettre des requêtes client. Bloque les rogue DHCP au niveau 2. Complété par le rate limiting anti-starvation.

**Q9. Quelles informations ne sont PAS répliquées par le basculement DHCP natif ?**
> R : Les options de niveau serveur, les filtres MAC (Allow/Deny), les classes personnalisées, la configuration DNS dynamique (compte dédié), les paramètres d'audit. À synchroniser manuellement (script section 63).

**Q10. Un poste obtient une IP mais pas de DNS interne, alors que l'option 006 est configurée sur l'étendue. Où chercher ?**
> R : Dans l'ordre de précédence inverse (du plus spécifique au plus général) : 1. Une stratégie applicable à ce client qui redéfinit 006 — 2. Une réservation avec option 006 spécifique — 3. Vérifier que l'option d'étendue est bien appliquée (Get-DhcpServerv4OptionValue) — 4. Côté client : ipconfig /renew après /release (bail ancien conservé).

## A5. Les 15 erreurs classiques (et comment les éviter)

| # | Erreur | Conséquence | Prévention |
|---|--------|-------------|------------|
| 1 | IP statiques dans la plage DHCP sans exclusion | Conflits d'adresses | Exclusions systématiques (§17) |
| 2 | Un seul serveur DHCP en production | Panne = plus de renouvellements | Failover obligatoire (§59) |
| 3 | IP helper vers un seul serveur | VLAN sans DHCP si panne | Helper vers les 2 (§65) |
| 4 | DNS public (8.8.8.8) dans l'option 006 | Échec d'ouverture de session AD | DNS AD uniquement (§34) |
| 5 | Options 066/067 + WDS sur le même serveur | Conflit port UDP 67 | Options DHCP 060 côté WDS (§37) |
| 6 | Deux DHCP indépendants, même étendue | Baux divergents, conflits | Failover natif (§59) |
| 7 | Bail de 8 jours sur Wi-Fi invités | Épuisement de l'étendue | Bail 4 h (§18) |
| 8 | Pas de compte DNS dédié | Échecs de mise à jour DNS, fantômes | svc-dhcp-dns + DnsUpdateProxy (§71) |
| 9 | Filtre Allow activé sur le LAN sans processus MAC | Utilisateurs bloqués, helpdesk saturé | Allow réservé aux VLANs contrôlés (§51) |
| 10 | Modification d'option serveur sans synchro manuelle | Config divergente entre les 2 nœuds | Script §63 |
| 11 | Pas de sauvegarde / jamais testée | Reconstruction manuelle après crash | Export quotidien + test annuel (§73) |
| 12 | DHCP snooping oublié sur un nouveau VLAN | Rogue DHCP possible | Checklist de création de VLAN (§94) |
| 13 | Changement d'IP du serveur sans mise à jour des helpers | VLANs sans DHCP | Procédure §91 |
| 14 | Étendue créée sans relais sur VLAN distant | Broadcast bloqué, pas d'IP | Une étendue par VLAN + helper (§64) |
| 15 | Logs d'audit non archivés | Impossible de tracer une IP dans le passé | Archivage 1 an (§99) |

## A6. Pour aller plus loin

**Documentation officielle Microsoft** :

- *DHCP — Vue d'ensemble* (Windows Server 2022/2025) : `https://learn.microsoft.com/fr-fr/windows-server/networking/technologies/dhcp/dhcp-top`
- *Cmdlets DhcpServer (PowerShell)* : `https://learn.microsoft.com/fr-fr/powershell/module/dhcpserver/`
- *Basculement DHCP — Guide pas à pas* : `https://learn.microsoft.com/fr-fr/windows-server/networking/technologies/dhcp/dhcp-failover`
- *DNS dynamique et DHCP* : `https://learn.microsoft.com/fr-fr/windows-server/networking/technologies/dhcp/dhcp-top`

**RFC de référence** :

- RFC 2131 — Dynamic Host Configuration Protocol (le protocole)
- RFC 2132 — DHCP Options and BOOTP Vendor Extensions (les options)
- RFC 3046 — DHCP Relay Agent Information Option (option 82)
- RFC 4388 — DHCP Lease Query (interrogation des baux)
- RFC 6926 — DHCPv6 Relay Agent (IPv6)

**Sujets connexes à étudier** (guides existants dans `~/workspace/user/files/`) :

- `proxmox_guide.md` — virtualisation des serveurs DHCP (HA VM)
- `debian_ubuntu_guide.md` — alternative ISC DHCP / Kea sur Linux
- `onduleurs_ups_guide.md` — protéger électriquement les serveurs DHCP (un DHCP sans courant = plus de renouvellements)
- `zabbix_guide.md` — supervision des sondes DHCP (section 106)

**Outils complémentaires** :

- Wireshark (filtre `bootp`) — analyse DORA
- Nmap (`--script broadcast-dhcp-discover`) — détection rogue
- DhcpExplorer / dhcpdump — tests manuels
- Yersinia (labo uniquement !) — comprendre les attaques starvation pour mieux s'en protéger

---

*Guide rédigé le 2026-09-27 — versions Windows Server 2019/2022/2025. Les exemples utilisent des plages de documentation (RFC 5737) : adapte à ton plan d'adressage réel avant toute mise en production.*
