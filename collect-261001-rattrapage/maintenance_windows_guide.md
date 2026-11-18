# Maintenance et exploitation Windows en entreprise

**Guide technique ultra-complet — serveurs & postes de travail**
Versions couvertes : **Windows Server 2019 / 2022 / 2025** — **Windows 11 23H2 / 24H2**
Public : administrateurs systèmes, techniciens d'exploitation, chefs de service.
Langue : français. Ton : direct, opérationnel, sans fioritures.

> **Avertissement** : les commandes de ce guide modifient parfois le système en profondeur
> (registre, BCD, pilotes, stratégies). Testez toujours sur un anneau pilote avant la
> production. Les exemples utilisent des noms fictifs (`srv-fic-01`, `contoso.local`,
> `10.0.0.0/8`) : adaptez-les à votre infrastructure.

---

## Sommaire

1. Pourquoi ce guide et comment l'utiliser
2. Périmètre : versions couvertes et conventions
3. Inventaire : la base de toute exploitation
4. Plan de maintenance : le calendrier annuel
5. Checklist quotidienne (serveurs)
6. Checklist quotidienne (postes)
7. Checklist hebdomadaire
8. Checklist mensuelle
9. Checklist trimestrielle et annuelle
10. Patch Tuesday : principe et calendrier
11. Anneaux de déploiement : test, pilote, production
12. Stratégie de ciblage par criticité
13. WSUS : architecture et dimensionnement
14. WSUS : installation et configuration
15. WSUS : maintenance du serveur (Server Cleanup Wizard)
16. Windows Update for Business : principes
17. WUfB : stratégies de report (quality/feature)
18. Intune vs GPO : pilotage des mises à jour
19. Fenêtres de maintenance et heures d'activité
20. Rollback : désinstaller un correctif
21. DISM : comprendre le magasin de composants
22. DISM : /ScanHealth, /CheckHealth, /RestoreHealth
23. DISM : monter et réparer une image WIM
24. SFC /scannow : mode d'emploi
25. CHKDSK et vérification du système de fichiers
26. Nettoyage disque : cleanmgr et Disk Cleanup
27. Storage Sense : configuration centralisée
28. Dossiers temporaires et caches à purger
29. WinSxS : analyse et nettoyage
30. Journaux IIS et fichiers de trace : rotation
31. Politique d'espace disque : seuils et alertes
32. Event Viewer : prise en main
33. Get-WinEvent : interroger les journaux en PowerShell
34. wevtutil : l'outil en ligne de commande
35. Filtres XPath et vues personnalisées
36. Abonnements d'événements (WEF) et collecteur
37. Journal Système : ce qu'on y cherche
38. Journal Sécurité : audit et alertes
39. Journaux Active Directory : DS, DNS Server, DFSR
40. Journaux DHCP, NPS et autres rôles
41. Rétention et archivage des journaux
42. Planificateur de tâches : concepts
43. Créer une tâche en PowerShell
44. Déclencheurs avancés et conditions
45. Comptes d'exécution : SYSTEM, comptes de service, gMSA
46. gMSA : création et utilisation
47. Audit et hygiène des tâches planifiées
48. Disques : gestion avec PowerShell et MMC
49. Partitionnement : bonnes pratiques
50. Quotas NTFS
51. Optimisation : défragmentation HDD, TRIM SSD
52. Santé des disques : SMART
53. Storage Spaces : concepts et administration
54. Performance : les outils (Gestionnaire des tâches, Resource Monitor, perfmon)
55. Compteurs clés : CPU
56. Compteurs clés : mémoire
57. Compteurs clés : disque
58. Compteurs clés : réseau
59. Analyseur de performances : jeux de collecteurs
60. PAL : Performance Analysis of Logs
61. Scripts de collecte de performances
62. WinRE : accéder à l'environnement de récupération
63. WinRE : activer, désactiver, personnaliser
64. Réparation du démarrage : diagnostic
65. BCD : bcdedit en pratique
66. bootrec : /fixmbr, /fixboot, /rebuildbcd
67. Restauration du système et clichés instantanés
68. Réinitialisation et réinstallation propre
69. BSOD : lire un écran bleu
70. Configurer les fichiers de vidage (dump)
71. WinDbg : installation et prise en main
72. Analyser un minidump : !analyze -v
73. Codes d'arrêt courants : tableau de référence
74. Driver Verifier : traquer un pilote fautif
75. Dépannage réseau : méthode générale
76. ipconfig : lecture et cas d'usage
77. Test-Connection, ping, pathping
78. Tracert et diagnostic de routage
79. Test-NetConnection : le couteau suisse
80. netstat et Get-NetTCPConnection
81. DNS côté client : nslookup et Resolve-DnsName
82. DNS : vider le cache, vérifier les suffixes
83. DHCP côté client : renouvellement et diagnostic
84. Wi-Fi : diagnostic de base
85. Cas pratique 1 : pas d'accès réseau du tout
86. Cas pratique 2 : adresse APIPA 169.254.x.x
87. Cas pratique 3 : DNS ne résout plus
88. Cas pratique 4 : un seul site inaccessible
89. Cas pratique 5 : lenteurs réseau intermittentes
90. Cas pratique 6 : partage SMB inaccessible
91. Cas pratique 7 : imprimante réseau hors ligne
92. Cas pratique 8 : VPN qui ne se connecte plus
93. Cas pratique 9 : port fermé / application injoignable
94. Cas pratique 10 : conflit d'adresse IP
95. Cas pratique 11 : profil réseau Public au lieu de Domaine
96. Cas pratique 12 : débit anormalement bas
97. Cas pratique 13 : RDP impossible vers un serveur
98. Cas pratique 14 : double pile IPv4/IPv6 capricieuse
99. Cas pratique 15 : proxy / PAC qui casse tout
100. AD/GPO côté client : gpupdate et gpresult
101. RSOP et journaux du service de stratégie de groupe
102. nltest : diagnostiquer la relation au domaine
103. Test-ComputerSecureChannel et réinitialisation
104. Temps : w32tm, la cause n°1 des échecs Kerberos
105. Jonction au domaine : procédure et dépannage
106. Monitoring : ce qu'on supervise sur Windows
107. Seuils d'alerte recommandés
108. Zabbix : agent2 et templates Windows
109. Prometheus : windows_exporter
110. Centralisation des journaux (syslog / Graylog / Loki)
111. PowerShell : fondamentaux d'administration
112. PSRemoting et WinRM : mise en place sécurisée
113. Invoke-Command : exécution à distance
114. Sessions persistantes et fichiers distants
115. Inventaire matériel en PowerShell
116. Inventaire logiciels installés
117. Inventaire des correctifs installés
118. Rapport espace disque multi-serveurs
119. Rapport comptes AD expirés / inactifs
120. Rapport certificats qui expirent
121. Exécution planifiée des scripts de rapport
122. PRA/PCA : RTO, RPO et scénarios
123. Stratégie de sauvegarde Windows
124. Windows Server Backup en pratique
125. VSS : clichés instantanés de volumes
126. Sauvegarde des contrôleurs de domaine (état du système)
127. Tests de restauration : procédure
128. Documentation d'exploitation : le DTI
129. Procédures : standard de rédaction
130. Gestion des changements
131. Passation et continuité (angle chef de service)
132. 20 erreurs classiques d'exploitation Windows
133. Pense-bête des commandes PowerShell
134. Pense-bête des commandes CMD / classiques
135. Quiz : 10 questions
136. Réponses du quiz
137. Glossaire
138. Pour aller plus loin

---

## 1. Pourquoi ce guide et comment l'utiliser

Ce guide couvre le **cycle de vie opérationnel** d'un parc Windows : du patch mensuel au
dépannage d'un écran bleu, en passant par la supervision et la reprise d'activité.
Il s'adresse au technicien qui intervient comme au chef de service qui organise.

**Mode d'emploi :**

- En intervention : utilisez le sommaire et le pense-bête (§133-134).
- En routine : appliquez les checklists (§5-9) telles quelles, adaptez les seuils.
- En projet : les sections WSUS/WUfB (§13-18), monitoring (§106-110) et PRA (§122-127)
  servent de cahier des charges.
- Chaque commande PowerShell est autonome : copiable, modifiable, réutilisable dans
  vos scripts. Les noms de machines, domaines et IP sont fictifs.

**Règle d'or du chef de service** : on n'exploite bien que ce qu'on connaît.
Sans inventaire à jour (§3), sans supervision (§106) et sans documentation (§128),
toute maintenance est du bricolage. Ce guide commence donc par l'organisation,
pas par la technique.

---

## 2. Périmètre : versions couvertes et conventions

| Plateforme | Versions couvertes | Fin de support (rappel) |
|---|---|---|
| Windows Server | 2019 (1809), 2022 (21H2), 2025 (24H2) | 2019 : 09/01/2029 (étendu) ; 2022 : 14/10/2031 ; 2025 : 10/10/2034 |
| Windows 11 | 23H2 (22631), 24H2 (26100) | 23H2 : 11/11/2025 (Pro) ; 24H2 : 13/10/2026 (Pro) — vérifiez le cycle en cours |

**Conventions du guide :**

- `PS>` = invite PowerShell (exécuté en administrateur sauf mention contraire).
- `CMD>` = invite de commandes classique.
- Les cmdlets sont données pour **PowerShell 5.1** (inclus dans Windows) sauf mention
  de PowerShell 7.
- Les chemins de GPO sont donnés sous la forme
  `Configuration ordinateur > Stratégies > Modèles d'administration > ...`.
- ⚠️ signale une opération à risque ou dépendante de la version.

> ⚠️ **Windows Server 2025** : WSUS y est toujours présent mais Microsoft a annoncé en
> 2024 sa **dépréciation** (plus aucune fonctionnalité nouvelle). Pour un nouveau
> déploiement, privilégiez **Windows Update for Business** (§16) ou une solution
> tierce, et ne basez plus votre stratégie à 5 ans sur WSUS seul.

---

## 3. Inventaire : la base de toute exploitation

Pas d'inventaire = pas de maintenance sérieuse. L'inventaire minimal par machine :

| Champ | Exemple | Source |
|---|---|---|
| Nom / rôle | `srv-fic-01` / serveur de fichiers | CMDB |
| OS + build | Server 2022 21H2 (20348.2402) | `Get-ComputerInfo` |
| IP / VLAN | 10.10.20.11 / VLAN 20 | IPAM |
| Garantie / contrat | Dell ProSupport jusqu'au 2028-06 | Fournisseur |
| Criticité | P1 (production) | Analyse d'impact |
| Fenêtre de maintenance | Dim 02:00–05:00 | Planning |
| Responsable | Équipe infra | Annuaire |

Collecte automatisée (détail §115-117). Stockez le résultat dans une CMDB, un tableur
versionné, ou à défaut un partage avec historique. **Règle** : tout serveur ajouté au
domaine est inventorié le jour même ; revue complète trimestrielle.

Script express d'inventaire (à lancer via PSRemoting, §113) :

```powershell
$servers = 'srv-fic-01','srv-ad-01','srv-prn-01'
Invoke-Command -ComputerName $servers {
    [pscustomobject]@{
        Machine   = $env:COMPUTERNAME
        OS        = (Get-ComputerInfo).WindowsProductName
        Build     = (Get-ComputerInfo).OsBuildNumber
        IP        = (Get-NetIPAddress -AddressFamily IPv4 |
                     Where-Object { $_.InterfaceAlias -notlike '*Loopback*' }).IPAddress -join ', '
        DisqueC_Go = [math]::Round((Get-PSDrive C).Free / 1GB, 1)
        MAJ       = (Get-HotFix | Sort-Object InstalledOn -Descending |
                     Select-Object -First 1).HotFixID
    }
} | Format-Table -AutoSize
```

---

## 4. Plan de maintenance : le calendrier annuel

Le plan de maintenance est **le** document du chef de service. Il fixe qui fait quoi,
quand, avec quelle fenêtre d'intervention. Modèle type :

| Périodicité | Activités | Responsable | Fenêtre |
|---|---|---|---|
| Quotidienne | Checklists §5-6, supervision | Astreinte | 08:30 |
| Hebdomadaire | Checklist §7, revue des alertes | Technicien | Ven 16:00 |
| Mensuelle | Patch Tuesday (§10-11), checklist §8 | Équipe infra | Sam/Dim |
| Trimestrielle | Checklist §9, revue inventaire, test PRA partiel | Chef de service | Planifié |
| Annuelle | Test PRA complet, revue contrats, audit AD | Chef de service | Planifié |

**Principes :**

1. **Une fenêtre de maintenance contractuelle** par classe de serveurs (ex : serveurs
   applicatifs le 2ᵉ dimanche du mois 02:00–06:00). Communiquez-la aux métiers.
2. **Gel des changements** pendant les périodes critiques métier (clôtures, paies,
   inventaires).
3. **Toute intervention hors fenêtre** = changement exceptionnel (§130) avec validation.
4. **Journalisez tout** : un ticket ou une entrée de journal par intervention, même
   de 5 minutes. La mémoire est un mauvais outil d'exploitation.

---

## 5. Checklist quotidienne (serveurs)

À exécuter chaque matin ouvré, idéalement en partie automatisée (rapport e-mail).

- [ ] **Supervision** : 0 alerte critique non acquittée (Zabbix/Prometheus, §106).
- [ ] **Espace disque** : aucun volume < 15 % libre sur les serveurs P1/P2.
- [ ] **Sauvegardes** : tous les jobs de la nuit en succès (vérifier le rapport, pas
      seulement l'absence d'alerte).
- [ ] **Services critiques** : AD DS, DNS, DHCP démarrés sur les contrôleurs ;
      spouleur, partages sur les serveurs concernés.
- [ ] **Journal Système** : erreurs/disques (`disk`, `ntfs`), erreurs de démarrage.
- [ ] **Journal Sécurité** : pics d'échecs d'ouverture de session (ID 4625).
- [ ] **Réplication AD** : `repadmin /replsummary` sans erreur > 0.
- [ ] **Antivirus/EDR** : définitions à jour, 0 menace non traitée.
- [ ] **Onduleurs** : état normal si supervisés (lien avec l'exploitation énergie).

Script de contrôle matinal (extrait ; version complète §118) :

```powershell
# Contrôle express : disque + services critiques sur une liste de serveurs
$servers = Get-Content C:\Scripts\serveurs.txt
Invoke-Command -ComputerName $servers {
    $disk = Get-CimInstance Win32_LogicalDisk -Filter "DeviceID='C:'"
    [pscustomobject]@{
        Machine   = $env:COMPUTERNAME
        C_LibrePct = [math]::Round($disk.FreeSpace / $disk.Size * 100, 1)
        DNS       = (Get-Service Dnscache).Status
        Spooler   = (Get-Service Spooler).Status
    }
} | Where-Object { $_.C_LibrePct -lt 20 } | Format-Table -AutoSize
```

---

## 6. Checklist quotidienne (postes)

- [ ] **File de tickets** : nouveaux incidents utilisateurs triés avant 09:30.
- [ ] **Déploiements en échec** : rapports Intune/GPO/SCCM (logiciels, mises à jour).
- [ ] **Postes critiques** (accueil, ateliers, caisses) : démarrés et à jour.
- [ ] **Antivirus** : postes sans remontée > 48 h = à investiguer.
- [ ] **Espace disque** : alerte sous 10 % sur C: (profils utilisateurs).

> Astuce : un rapport PowerShell quotidien envoyé par e-mail (§121) remplace
> avantageusement la vérification manuelle poste par poste.

---

## 7. Checklist hebdomadaire

- [ ] Revue des **alertes** de la semaine : patterns récurrents ? actions correctives ?
- [ ] **Journaux d'événements** des serveurs critiques : erreurs nouvelles vs baseline.
- [ ] **Espace disque** : tendance (un volume qui perd 2 %/semaine sera plein dans X
      semaines — anticipez, §31).
- [ ] **Correctifs en attente** : serveurs/postes non conformes au patch du mois.
- [ ] **Tâches planifiées** : 0 échec sur les tâches critiques (§47).
- [ ] **Sauvegardes** : un contrôle de restauration de fichier test par semaine
      (tourniquet sur les serveurs).
- [ ] **Sécurité** : comptes verrouillés, nouveaux comptes créés, groupes sensibles
      modifiés (journal Sécurité des DC).
- [ ] **Nettoyage** : corbeilles, dossiers temp des serveurs TSE/RDS.

---

## 8. Checklist mensuelle

- [ ] **Patch Tuesday** : déploiement anneaux test → pilote → production (§11).
- [ ] **Rapport de conformité** : % de machines à jour par anneau (objectif ≥ 98 %).
- [ ] **Redémarrages** : tous les serveurs redémarrés au moins une fois dans le mois
      (les serveurs « uptime 400 jours » sont des bombes à retardement).
- [ ] **Comptes AD** : désactiver les comptes inactifs > 90 jours (§119).
- [ ] **Certificats** : liste des certificats expirant dans < 60 jours (§120).
- [ ] **Quotas et partages** : revue des dossiers > 80 % de quota.
- [ ] **GPO** : sauvegarde (Backup-GPO) avant toute modification.
- [ ] **Documentation** : procédures mises à jour après chaque changement notable.
- [ ] **Revue des changements** du mois avec l'équipe (15 min suffisent).

---

## 9. Checklist trimestrielle et annuelle

**Trimestrielle :**

- [ ] Inventaire complet : rapprochement CMDB ↔ AD ↔ supervision.
- [ ] Test de restauration PRA **partiel** (un serveur, un scénario).
- [ ] Revue des droits : groupes sensibles AD, administrateurs locaux (LAPS).
- [ ] Mise à jour des images de référence / séquences de déploiement.
- [ ] Nettoyage WSUS (§15), purge des journaux archivés.
- [ ] Revue des contrats : garanties, supports, licences.

**Annuelle :**

- [ ] Test PRA **complet** (§127).
- [ ] Audit de sécurité AD (mots de passe, Kerberoasting, délégation).
- [ ] Plan de renouvellement matériel (serveurs > 5 ans).
- [ ] Bilan d'exploitation : MTTR, disponibilité, incidents majeurs — présenté
      à la direction.
- [ ] Mise à jour du DTI (§128) et du plan de maintenance (§4).

---

## 10. Patch Tuesday : principe et calendrier

Microsoft publie les mises à jour de sécurité le **2ᵉ mardi de chaque mois**
(« Patch Tuesday », vers 19h heure de Paris). Le cycle d'exploitation mensuel type :

| Jour | Action |
|---|---|
| J (mardi) | Publication. Lecture des notes, repérage des correctifs critiques / 0-day. |
| J+1 → J+7 | Déploiement anneau **test** (§11), validation applicative. |
| J+7 → J+14 | Déploiement anneau **pilote** (échantillon représentatif). |
| J+14 → J+21 | Déploiement **production**, par vagues, dans les fenêtres (§19). |
| J+21 → J+30 | Rattrapage des non-conformes, rapport de conformité (§8). |

**Hors cycle** : les correctifs **hors bande** (0-day exploité, ex. PrintNightmare)
suivent une procédure accélérée : test réduit → pilote restreint → production sous
48-72 h, avec validation du changement exceptionnel (§130).

Types de mises à jour à connaître :

| Type | Contenu | Redémarrage |
|---|---|---|
| Correctif cumulatif mensuel (B) | Sécurité + qualité cumulées | Oui (souvent) |
| Correctif hors bande (OOB) | Urgence sécurité | Oui |
| Correctif .NET | Sécurité .NET Framework | Oui |
| Mise à jour de la pile de maintenance (SSU) | Fiabilise Windows Update lui-même | Parfois |
| Pilotes | Via WU (optionnel) | Variable |

> Règle : ne jamais « sauter » un cumulatif pour gagner du temps : ils sont cumulatifs,
> le mois suivant inclut le précédent. Un serveur à jour en N-1 se met à jour en N
> sans étape intermédiaire.

---

## 11. Anneaux de déploiement : test, pilote, production

| Anneau | Contenu | Délai après Patch Tuesday | Objectif |
|---|---|---|---|
| **Test** | 2-5 machines par OS/rôle (VM de labo) | J+1 | Détecter les régressions grossières |
| **Pilote** | 5-10 % du parc, utilisateurs volontaires, tous métiers | J+7 | Valider en conditions réelles |
| **Production** | Reste du parc, par vagues (serveurs non critiques → critiques) | J+14 | Conformité ≥ 98 % |

**Serveurs** : ne jamais patcher tous les membres d'un cluster / tous les DC la même
nuit. Règle : **N+1 minimum** — toujours un membre sain pendant l'opération.
Exemple : 2 DC → DC1 semaine 1, DC2 semaine 2 ; cluster Hyper-V → nœud par nœud
avec migration des VM.

**Critères de passage d'un anneau au suivant :**

- 0 BSOD / 0 non-démarrage sur l'anneau précédent ;
- applications métier validées (ouverture, impression, batch) ;
- taux d'échec d'installation < 2 %.

**Blocage** : si un correctif casse une application, on le **décline/bloque pour
l'anneau production** et on documente (KB, symptômes, contournement) en attendant
le correctif suivant. On ne laisse pas un parc diverger sans trace écrite.

---

## 12. Stratégie de ciblage par criticité

Ne traitez pas un serveur de fichiers et un poste de la même façon :

| Classe | Exemples | Patch | Redémarrage |
|---|---|---|---|
| P1 critique | DC, DNS/DHCP, hyperviseurs, ERP | Fenêtre dédiée, un par un | Planifié, annoncé |
| P2 important | Serveurs applicatifs, fichiers, impression | Fenêtre mensuelle | Planifié |
| P3 standard | Postes utilisateurs | Heures d'activité + report (§19) | Différé 7 jours max |
| Labo | Machines de test | Immédiat | Libre |

**Groupes de ciblage** : en WSUS via groupes d'ordinateurs ; en WUfB/Intune via
groupes Entra ID ou anneaux de mise à jour ; en GPO via OU ou filtrage de sécurité.
Nommez les groupes de façon explicite : `WU-Anneau-Test`, `WU-Anneau-Pilote`,
`WU-Prod-Serveurs-P1`, `WU-Prod-Postes`.

---

## 13. WSUS : architecture et dimensionnement

> ⚠️ WSUS est **déprécié** par Microsoft depuis septembre 2024 (maintenu, mais sans
> évolution). Ne lancez plus un nouveau WSUS en 2026 sauf contrainte (réseau isolé,
> parc 100 % local sans Intune). La cible moderne est Windows Update for Business (§16).

Architecture classique : un serveur **WSUS amont** synchronisé depuis Microsoft
Update, des **réplicas aval** par site, les clients configurés par GPO.

| Paramètre | Recommandation |
|---|---|
| OS | Windows Server 2022 (rôle WSUS) |
| Stockage contenu | 150-250 Go minimum (selon produits/langues sélectionnés) |
| Base | WID (Windows Internal Database) jusqu'à ~10 000 clients ; SQL au-delà |
| Ports | 8530 (HTTP) / 8531 (HTTPS) par défaut depuis Server 2012 |
| Produits | Uniquement ceux du parc (chaque produit/langue = Go téléchargés) |
| Classifications | Correctifs critiques, sécurité, cumulatifs, SSU |

**Pièges classiques :**

- Sélectionner « tous les produits » → des centaines de Go et une console inutilisable.
- Oublier les **langues** : limitez aux langues du parc.
- Laisser les mises à jour **remplacées** (superseded) approuvées → base obèse.

---

## 14. WSUS : installation et configuration

Installation du rôle (PowerShell) :

```powershell
# Sur le serveur WSUS (Server 2022)
Install-WindowsFeature -Name UpdateServices -IncludeManagementTools
# Configuration initiale : contenu sur D:, base WID
& 'C:\Program Files\Update Services\Tools\wsusutil.exe' postinstall CONTENT_DIR=D:\WSUS
```

Configuration de base (à faire dans la console, une fois) :

1. **Assistant de configuration** : choisir l'amont (Microsoft Update), le proxy si
   besoin, les **langues** (français + anglais), les **produits** (Windows 11,
   Windows Server 2019/2022/2025, Office si géré…), les **classifications**.
2. **Planification de synchronisation** : 2-4 fois/jour (ex. 06:00, 12:00, 18:00).
3. **Groupes d'ordinateurs** : créer `Test`, `Pilote`, `Prod-Serveurs`, `Prod-Postes`.
4. **Approbations** : manuelles par groupe (recommandé) ou règle d'approbation
   automatique limitée (ex. correctifs critiques → groupe Test uniquement).

GPO client type (appliquée par anneau) :

```text
Configuration ordinateur > Stratégies > Modèles d'administration >
Composants Windows > Windows Update > Gérer les mises à jour proposées par WSUS
  - Spécifier l'emplacement du service : http://wsus.contoso.local:8530
  - Activer le ciblage côté client : WU-Prod-Postes
```

Vérification côté client :

```powershell
# Le client voit-il le WSUS ?
(Get-ItemProperty 'HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate').WUServer
# Forcer une détection
USOClient StartScan   # Windows 10/11 (remplace wuauclt /detectnow)
# Rapport d'état
Get-WindowsUpdateLog  # génère WindowsUpdate.log lisible sur le Bureau
```

---

## 15. WSUS : maintenance du serveur (Server Cleanup Wizard)

Un WSUS non entretenu meurt étouffé (base > 10 Go, console qui timeout). Maintenance
**mensuelle** :

1. **Décliner les mises à jour remplacées** : dans la console, vue « Toutes les mises
   à jour », filtrer Statut d'approbation = Non approuvée + État = Remplacée →
   clic droit **Refuser**. Ou en PowerShell :

```powershell
# Décliner les mises à jour remplacées et non nécessaires (à adapter)
[void][reflection.assembly]::LoadWithPartialName("Microsoft.UpdateServices.Administration")
$wsus = [Microsoft.UpdateServices.Administration.AdminProxy]::GetUpdateServer()
$updates = $wsus.GetUpdates() | Where-Object {
    $_.IsSuperseded -and -not $_.IsDeclined
}
$updates | ForEach-Object { $_.Decline() }
"Déclinées : $($updates.Count)"
```

2. **Assistant Nettoyage du serveur** : cocher tout (mises à jour expirées,
   remplacées, fichiers inutiles, ordinateurs inactifs, révisions).
3. **Réindexer la base WID** (script `WsusDBMaintenance.sql` fourni par Microsoft,
   à exécuter mensuellement via sqlcmd sur `\\.\pipe\MICROSOFT##WID\tsql\query`).
4. **Surveiller l'espace** : le dossier `WsusContent` ne doit pas saturer le volume.

> Si la console MMC ne s'ouvre plus : augmentez la mémoire privée du pool
> d'applications `WsusPool` dans IIS (recommandé : 4 Go+) et recyclez-le. C'est le
> symptôme n°1 d'un WSUS négligé.

---

## 16. Windows Update for Business : principes

WUfB = pilotage des mises à jour **sans serveur intermédiaire** : les clients
téléchargent depuis Windows Update / Microsoft Update, mais **vous** contrôlez le
*quand* via stratégies (GPO, Intune, MDM). C'est la voie recommandée par Microsoft.

| Élément | WSUS | WUfB |
|---|---|---|
| Source des binaires | Serveur local | Microsoft Update (CDN) |
| Bande passante WAN | Faible (1 téléchargement/site) | Plus élevée (optimisable : Delivery Optimization, §ci-dessous) |
| Infrastructure | Serveur + base + maintenance | Aucune (stratégies uniquement) |
| Granularité | Approbation par KB | Reports en jours, pauses, anneaux |
| Avenir | Déprécié | Stratégique |

**Delivery Optimization** (optimisation de la distribution) : indispensable avec WUfB
pour ne pas saturer les liens WAN — les postes s'échangent les morceaux en P2P sur
le LAN. À activer par GPO/Intune (`Téléchargement > Optimisation de la distribution`,
mode « LAN » ou « Groupe »).

---

## 17. WUfB : stratégies de report (quality/feature)

Deux familles de reports (en jours) pilotent le déploiement :

| Stratégie | Contenu | Report conseillé (Prod) |
|---|---|---|
| Mises à jour qualité (Quality) | Correctifs mensuels | 7-14 jours |
| Mises à jour de fonctionnalités (Feature) | Nouvelles versions Windows 11 | 60-120 jours |

GPO correspondantes :

```text
Configuration ordinateur > Stratégies > Modèles d'administration >
Composants Windows > Windows Update > Gérer les mises à jour proposées par
Windows Update Entreprise
  - Sélectionner le moment de réception des builds Preview et des mises à
    jour de fonctionnalités : canal « Général », report 90 jours
  - Sélectionner le moment de réception des mises à jour qualité : report 10 jours
```

**Pause** : possibilité de suspendre 35 jours max (qualité) en cas de correctif
défectueux — le bouton d'urgence du §11.

**Anneaux via Intune** : créez des profils « Update rings for Windows 10 and later »
par anneau (Test : report 0 j / Pilote : 5 j / Prod : 10 j), assignés à des groupes
Entra ID. C'est l'équivalent moderne des groupes WSUS.

---

## 18. Intune vs GPO : pilotage des mises à jour

| Critère | GPO (AD local) | Intune (cloud) |
|---|---|---|
| Prérequis | Domaine AD, postes joints | Licences Intune, postes joints Entra ID / hybrides |
| Postes nomades | Non gérés hors VPN | Gérés partout |
| Rapports | Limités (scripts) | Update Compliance / rapports natifs |
| Coexistence | — | La co-gestion est possible (SCCM + Intune) |

**Règle de conflit** : si un poste reçoit à la fois une GPO WSUS (clé `WUServer`) et
une stratégie WUfB/Intune, **le WSUS gagne** (la présence de `WUServer` désactive
WUfB). Pour migrer WSUS → WUfB : supprimez proprement les stratégies WSUS
(`WUServer`, `WUStatusServer`, `UseWUServer=0`) avant d'appliquer les anneaux.

Vérification du mode actif côté client :

```powershell
# 0 = Microsoft Update direct (WUfB), 1 = WSUS configuré
$wu = 'HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU'
(Get-ItemProperty $wu -ErrorAction SilentlyContinue).UseWUServer
# Source réelle des stratégies (MDM vs GPO) : voir aussi rsop / MDM Diagnostics
```

---

## 19. Fenêtres de maintenance et heures d'activité

**Serveurs** : pas de redémarrage automatique sauvage. Pilotez par GPO :

```text
Composants Windows > Windows Update > Gérer l'expérience utilisateur final
  - Ne pas redémarrer automatiquement avec des utilisateurs connectés : Activé
  - Toujours redémarrer automatiquement à l'heure planifiée : selon fenêtre
```

Ou imposez l'installation + redémarrage via script/ordonnanceur pendant la fenêtre
contractuelle (§4), avec notification aux utilisateurs (RDS : `msg *`).

**Postes Windows 11** : les **heures d'activité** (ex. 08:00–19:00) empêchent le
redémarrage pendant le travail ; au-delà, le poste redémarre seul. Configurez-les
par GPO/Intune et laissez un **délai de grâce** (ex. 3-7 jours) avant redémarrage
forcé pour les correctifs qualité.

```powershell
# Consulter / définir les heures d'activité (Windows 11)
Get-ItemProperty 'HKLM:\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings' |
  Select-Object ActiveHoursStart, ActiveHoursEnd
# Exemple de notification avant maintenance sur un serveur RDS
msg * /SERVER:srv-rds-01 "Maintenance planifiée à 22h : sauvegarde en cours, merci de fermer vos sessions."
```

---

## 20. Rollback : désinstaller un correctif

Quand un correctif pose problème et qu'aucun contournement n'existe :

```powershell
# 1. Identifier le KB fautif
Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 10 HotFixID, InstalledOn, Description

# 2a. Désinstaller (méthode classique, invite admin)
wusa /uninstall /kb:5034765 /quiet /norestart

# 2b. Via DISM (utile si wusa échoue, notamment sur serveurs)
DISM /Online /Remove-Package /PackageName:Package_for_RollupFix~31bf3856ad364e35~amd64~~26100.1234.1.1

# 3. Bloquer sa réinstallation
#  - WSUS : refuser la mise à jour pour les groupes concernés
#  - WUfB : mettre en pause les mises à jour qualité (35 j max) ou outil wushowhide.diagcab (postes)
```

**Nom du package** pour DISM : `DISM /Online /Get-Packages | findstr RollupFix`.

**Limites** :

- Un cumulatif ne se « répare » pas partiellement : on le retire en entier.
- Après 10 jours (postes) / selon configuration (serveurs), les fichiers de
  désinstallation peuvent être purgés → rollback impossible sans restauration.
- Sur un **contrôleur de domaine**, évitez le rollback sauf nécessité absolue :
  préférez la restauration d'état système (§126) en cas de corruption AD.

> Procédure d'équipe : tout rollback est tracé (ticket : KB, machines, motif,
> validation) et suivi d'une revue au patch suivant (le correctif réapparaît).

---

## 21. DISM : comprendre le magasin de composants

Le **magasin de composants** (`C:\Windows\WinSxS`) est la base de données des
fichiers système Windows. Quand il est corrompu, les mises à jour échouent
(erreur `0x80073712`, `0x800f081f`) et `sfc` ne suffit plus. **DISM**
(Deployment Image Servicing and Management) est l'outil de réparation.

Ordre de bataille en cas de système « bizarre » après un patch raté :

1. `DISM /Online /Cleanup-Image /RestoreHealth` (§22)
2. `sfc /scannow` (§24)
3. Redémarrage, re-test de Windows Update

---

## 22. DISM : /ScanHealth, /CheckHealth, /RestoreHealth

```cmd
:: 1. Vérification rapide (lit un marqueur, quelques secondes)
DISM /Online /Cleanup-Image /CheckHealth

:: 2. Analyse approfondie (plusieurs minutes)
DISM /Online /Cleanup-Image /ScanHealth

:: 3. Réparation : télécharge les fichiers sains depuis Windows Update
DISM /Online /Cleanup-Image /RestoreHealth
```

**Si le poste n'a pas accès à Windows Update** (ou si la réparation échoue avec
`0x800f081f` = source introuvable), fournissez une source saine (ISO de la **même
build**) :

```cmd
:: Monter l'ISO puis utiliser install.wim / install.esd comme source
DISM /Online /Cleanup-Image /RestoreHealth /Source:WIM:D:\sources\install.wim:1 /LimitAccess
:: Variante avec un ESD (éditions grand public)
DISM /Online /Cleanup-Image /RestoreHealth /Source:ESD:D:\sources\install.esd:1 /LimitAccess
```

> L'index (`:1`) doit correspondre à **l'édition exacte** (Pro, Entreprise…).
> Vérifiez avec `DISM /Get-WimInfo /WimFile:D:\sources\install.wim`.
> `/LimitAccess` empêche DISM d'aller chercher sur Windows Update.

Journal détaillé : `C:\Windows\Logs\DISM\dism.log`.

---

## 23. DISM : monter et réparer une image WIM

Utile pour réparer une image de référence (déploiement) ou extraire un fichier
système sain :

```cmd
:: Monter l'image en lecture/écriture
DISM /Mount-Wim /WimFile:D:\images\ref.wim /index:1 /MountDir:C:\mnt\wim

:: ... modifications (ajout de pilotes, correctifs, fichiers) ...

:: Démonter en enregistrant
DISM /Unmount-Wim /MountDir:C:\mnt\wim /Commit
:: Ou abandonner les modifications
DISM /Unmount-Wim /MountDir:C:\mnt\wim /Discard
```

Ajouter un correctif à une image montée :

```cmd
DISM /Image:C:\mnt\wim /Add-Package /PackagePath:D:\patches\windows11.0-kb5034765-x64.msu
```

---

## 24. SFC /scannow : mode d'emploi

Le **Vérificateur des fichiers système** compare les fichiers protégés à leur
empreinte et restaure les versions saines depuis le magasin de composants.

```cmd
:: Analyse + réparation (redémarrage parfois nécessaire à la fin)
sfc /scannow

:: Analyse seule (idéal en diagnostic, sans modification)
sfc /verifyonly

:: Réparer un fichier précis (chemin complet requis)
sfc /scanfile=C:\Windows\System32\kernel32.dll
```

- Durée : 10 à 30 minutes. Ne pas interrompre.
- Résultat « des fichiers endommagés ont été réparés » → redémarrez et relancez
  une fois pour confirmer (« aucune violation d'intégrité »).
- Si SFC échoue de façon répétée : passez par DISM (§22) **avant** de relancer SFC
  (SFC puise dans le magasin que DISM répare).
- Journal : `C:\Windows\Logs\CBS\CBS.log` (verbeux ; filtrez sur `[SR]`).

Extraction des lignes SFC du CBS.log :

```powershell
Select-String -Path C:\Windows\Logs\CBS\CBS.log -Pattern '\[SR\]' |
  Select-Object -ExpandProperty Line | Out-File C:\Temp\sfc-sr.txt
```

---

## 25. CHKDSK et vérification du système de fichiers

```cmd
:: Vérification en lecture seule (sans démontage)
chkdsk C:

:: Réparation des erreurs logiques (redémarrage requis sur le volume système)
chkdsk C: /f

:: + recherche des secteurs défectueux et récupération (long sur gros volumes)
chkdsk D: /f /r

:: NTFS uniquement : répare en ligne sans démontage quand possible
chkdsk C: /scan
chkdsk C: /spotfix
```

Équivalent PowerShell (plus scriptable) :

```powershell
# État de santé logique du volume
Repair-Volume -DriveLetter C -Scan
# Réparation en ligne
Repair-Volume -DriveLetter C -SpotFix
```

**Règles pratiques :**

- Un `chkdsk /r` sur un volume de plusieurs To peut durer **des heures** : planifiez
  en fenêtre de maintenance.
- Des erreurs CHKDSK récurrentes = disque en fin de vie → contrôlez le SMART (§52)
  et préparez le remplacement, pas le 4ᵉ chkdsk.
- Sur **ReFS** (Storage Spaces, §53), `chkdsk` est remplacé par la réparation
  automatique (integrity streams) ; `Repair-Volume` reste utilisable.

---

## 26. Nettoyage disque : cleanmgr et Disk Cleanup

> ⚠️ `cleanmgr.exe` est **déprécié** (toujours présent sur Server 2019/2022/2025 et
> Windows 11, mais sans évolution). Privilégiez Storage Sense (§27) pour
> l'automatisation et DISM pour WinSxS (§29).

Nettoyage manuel assisté :

```cmd
:: Ouvre le sélecteur de catégories (à cocher une fois)
cleanmgr /sageset:10
:: Exécute le nettoyage avec le profil n°10 (scriptable)
cleanmgr /sagerun:10
```

Catégories utiles sur serveur : *Fichiers temporaires*, *Fichiers journaux*,
*Nettoyage de Windows Update*, *Fichiers d'optimisation de livraison*,
*Corbeille*, *Miniatures*.

**Automatisation** (tâche planifiée mensuelle, §43) :

```powershell
# Profil silencieux : à exécuter via une tâche planifiée en SYSTEM
Start-Process cleanmgr.exe -ArgumentList '/sagerun:10' -Wait
```

Vérifiez toujours **avant/après** : `Get-PSDrive C` ou §31. Un nettoyage qui ne
libère rien signale un autre problème (logs qui regrossissent, §30).

---

## 27. Storage Sense : configuration centralisée

Storage Sense (Assistant stockage) automatise le nettoyage : fichiers temp,
corbeille, contenu du dossier Téléchargements, OneDrive à la demande.

GPO (Windows 10/11, partiellement Server) :

```text
Configuration ordinateur > Stratégies > Modèles d'administration > Système >
Assistant stockage
  - Autoriser l'Assistant stockage : Activé
  - Configurer la cadence de nettoyage : Chaque mois
  - Supprimer les fichiers de la corbeille après : 30 jours
  - Supprimer les fichiers du dossier Téléchargements après : 60 jours
```

État et déclenchement manuel en PowerShell :

```powershell
# L'assistant est-il activé ?
Get-ItemProperty 'HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\StorageSense\Parameters\StoragePolicy' |
  Select-Object 01, 04, 08
# Forcer un passage (Windows 11)
Start-Process "$env:SystemRoot\System32\cleanmgr.exe" -ArgumentList '/verylowdisk' -Wait
```

> Sur **Windows Server**, Storage Sense est limité : préférez des tâches planifiées
> maison (§43) qui purgent les dossiers identifiés au §28.

---

## 28. Dossiers temporaires et caches à purger

| Emplacement | Contenu | Purge sûre ? |
|---|---|---|
| `C:\Windows\Temp` | Temp système | Oui (fichiers non verrouillés) |
| `C:\Windows\SoftwareDistribution\Download` | Cache Windows Update | Oui **si** le service `wuauserv` est arrêté |
| `%TEMP%` / `%LOCALAPPDATA%\Temp` | Temp utilisateur | Oui (session fermée de préférence) |
| `C:\Windows\Logs\CBS` | Logs CBS/DISM | Oui (anciens `.log`, garder le courant) |
| `C:\inetpub\logs\LogFiles` | Logs IIS | Oui avec rotation (§30) |
| `C:\ProgramData\Microsoft\Windows\WER` | Rapports d'erreurs Windows | Oui (anciens) |
| Corbeilles `C:\$Recycle.Bin` | — | Oui via `Clear-RecycleBin` |

Script de purge mensuelle (tâche SYSTEM, à adapter) :

```powershell
# Purge sécurisée des temporaires (ne supprime que les fichiers de +7 jours non verrouillés)
$paths = @("$env:SystemRoot\Temp", "$env:TEMP")
foreach ($p in $paths) {
    Get-ChildItem $p -Recurse -Force -ErrorAction SilentlyContinue |
      Where-Object { $_.LastWriteTime -lt (Get-Date).AddDays(-7) } |
      Remove-Item -Force -Recurse -ErrorAction SilentlyContinue
}
Clear-RecycleBin -Force -ErrorAction SilentlyContinue

# Purge du cache Windows Update (service arrêté au préalable)
Stop-Service wuauserv -Force
Remove-Item "$env:SystemRoot\SoftwareDistribution\Download\*" -Recurse -Force -ErrorAction SilentlyContinue
Start-Service wuauserv
```

> ⚠️ Ne purgez **jamais** `C:\Windows\WinSxS` à la main : utilisez DISM (§29).
> Ne touchez pas non plus à `C:\Windows\System32\config` ni aux profils utilisateurs
> actifs.

---

## 29. WinSxS : analyse et nettoyage

Analyse de l'encombrement réel :

```cmd
DISM /Online /Cleanup-Image /AnalyzeComponentStore
```

Exemple de sortie : « Taille réelle du magasin », « sauvegardes et fonctionnalités
désactivées » récupérables. Nettoyage :

```cmd
:: Nettoyage standard des versions remplacées
DISM /Online /Cleanup-Image /StartComponentCleanup

:: Nettoyage agressif : supprime TOUTES les versions remplacées
:: (rend impossible la désinstallation des correctifs !)
DISM /Online /Cleanup-Image /StartComponentCleanup /ResetBase
```

> ⚠️ `/ResetBase` est **irréversible** : après son passage, impossible de
> désinstaller un correctif (§20). Ne l'utilisez qu'après validation complète du
> patch mensuel, jamais sur un serveur critique en période sensible.

Planificateur intégré : la tâche `Microsoft\Windows\Servicing\StartComponentCleanup`
tourne déjà automatiquement. Vérifiez qu'elle n'est pas désactivée (§47).

---

## 30. Journaux IIS et fichiers de trace : rotation

Les logs IIS (`C:\inetpub\logs\LogFiles\W3SVC*\`) grossissent vite sur un serveur
web exposé. Stratégie :

1. **Dans IIS** : limitez la rétention (journalisation > planifier : quotidien,
   taille max) ou centralisez vers un collecteur.
2. **Purge scriptée** des fichiers de plus de N jours :

```powershell
# Rotation des logs IIS : supprime les .log de +30 jours
$logRoot = 'C:\inetpub\logs\LogFiles'
Get-ChildItem $logRoot -Recurse -Filter '*.log' |
  Where-Object { $_.LastWriteTime -lt (Get-Date).AddDays(-30) } |
  Remove-Item -Force
```

3. **Autres traces gourmandes** : `C:\Windows\System32\LogFiles` (notamment
   `HTTPERR`), logs d'applications métier, dumps `MEMORY.DMP` oubliés après un
   BSOD (§70) — un seul fichier peut peser la taille de la RAM.

> Mettez les logs applicatifs **hors du volume système** dès l'installation
> (ex. `D:\Logs`). Un C: plein = serveur instable (§31).

---

## 31. Politique d'espace disque : seuils et alertes

Seuils d'exploitation recommandés :

| Volume | Alerte (warning) | Critique | Action |
|---|---|---|---|
| C: système | < 20 % libre | < 10 % libre ou < 10 Go | Nettoyage + analyse immédiate |
| Données | < 15 % libre | < 5 Go | Extension / archivage |
| Logs dédié | < 20 % libre | < 10 % libre | Rotation forcée (§30) |
| Sauvegarde | < 20 % libre | Job en échec | Purge des anciennes sauvegardes |

Surveillance native sans superviseur (quota d'alerte par volume via tâche
planifiée, §43) :

```powershell
# Alerte si un volume fixe passe sous 15 % libre (à planifier chaque heure)
$seuil = 15
Get-CimInstance Win32_LogicalDisk -Filter "DriveType=3" | ForEach-Object {
    $pct = [math]::Round($_.FreeSpace / $_.Size * 100, 1)
    if ($pct -lt $seuil) {
        $msg = "$($_.DeviceID) : $pct % libre sur $($env:COMPUTERNAME)"
        Write-EventLog -LogName Application -Source 'SuiviDisque' `
          -EventId 1001 -EntryType Warning -Message $msg
    }
}
# (Créer la source une fois : New-EventLog -LogName Application -Source 'SuiviDisque')
```

Top 10 des dossiers les plus lourds (diagnostic express) :

```powershell
Get-ChildItem C:\ -Directory -ErrorAction SilentlyContinue | ForEach-Object {
    $size = (Get-ChildItem $_.FullName -Recurse -File -ErrorAction SilentlyContinue |
             Measure-Object Length -Sum).Sum
    [pscustomobject]@{ Dossier = $_.FullName; Go = [math]::Round($size / 1GB, 2) }
} | Sort-Object Go -Descending | Select-Object -First 10
```

---

## 32. Event Viewer : prise en main

L'**Observateur d'événements** (`eventvwr.msc`) est le point d'entrée visuel.
Journaux Windows essentiels :

| Journal | Contenu |
|---|---|
| **Application** | Erreurs des applications et services |
| **Système** | Pilotes, services, matériel, démarrage/arrêt |
| **Sécurité** | Audits d'ouverture de session, accès objets (si audit activé) |
| **Installation** | Historique des installations (MSI, correctifs) |
| Journaux des applications et services | Par rôle : `Microsoft > Windows > ...` (ex. TerminalServices, SMBClient) |

Réflexes :

- Triez par **niveau** (Critique/Erreur) puis par **date** : les erreurs en grappe
  au même moment = un incident unique, pas dix.
- L'onglet **Détails > convivial** donne les champs exploitables (noms, codes).
- Un ID d'événement + sa **source** suffisent à une recherche web ciblée
  (ex. « Event ID 55 Ntfs »).

---

## 33. Get-WinEvent : interroger les journaux en PowerShell

`Get-WinEvent` remplace l'ancien `Get-EventLog` (limité aux journaux classiques).
C'est l'outil n°1 d'exploitation scriptée.

```powershell
# 50 dernières erreurs du journal Système
Get-WinEvent -LogName System -MaxEvents 50 | Where-Object LevelDisplayName -eq 'Error'

# Filtre efficace côté fournisseur (beaucoup plus rapide) : dernières 24 h, niveaux 1-2
$filtre = @{ LogName = 'System'; Level = 1, 2; StartTime = (Get-Date).AddDays(-1) }
Get-WinEvent -FilterHashtable $filtre | Select-Object TimeCreated, Id, ProviderName, Message |
  Format-Table -AutoSize -Wrap

# Compter les erreurs par ID sur 7 jours (détection de patterns)
Get-WinEvent -FilterHashtable @{ LogName = 'System'; Level = 2; StartTime = (Get-Date).AddDays(-7) } |
  Group-Object Id | Sort-Object Count -Descending | Select-Object -First 15 Count, Name

# Journal Sécurité : échecs d'ouverture de session (4625) sur 24 h
Get-WinEvent -FilterHashtable @{ LogName = 'Security'; Id = 4625; StartTime = (Get-Date).AddDays(-1) } |
  Select-Object TimeCreated,
    @{n='Compte'; e={ $_.Properties[5].Value }},
    @{n='IP';     e={ $_.Properties[19].Value }} |
  Format-Table -AutoSize
```

> ⚠️ L'index des `Properties[]` varie selon la version : vérifiez une fois sur un
> événement réel (onglet Détails) avant d'industrialiser.

Interroger une machine distante (PSRemoting ou `-ComputerName` sur les journaux
classiques uniquement ; préférez `Invoke-Command`, §113).

---

## 34. wevtutil : l'outil en ligne de commande

`wevtutil` est le pendant CMD de Get-WinEvent, utile en WinRE ou en script batch.

```cmd
:: Lister les journaux disponibles
wevtutil el

:: 10 derniers événements du journal Système, texte lisible, plus récents d'abord
wevtutil qe System /c:10 /rd:true /f:text

:: Exporter un journal en .evtx (pour analyse hors ligne)
wevtutil epl System C:\Temp\system.evtx

:: Vider un journal (après export !)
wevtutil cl System

:: Taille et rétention d'un journal
wevtutil gl System
```

Effacer **tous** les journaux d'un poste en dépannage (radical, à tracer) :

```cmd
for /f "tokens=*" %l in ('wevtutil el') do wevtutil cl "%l"
```

---

## 35. Filtres XPath et vues personnalisées

Dans l'Observateur : clic droit sur un journal > **Filtrer le journal actuel** >
onglet **XML** > cocher « Modifier la requête manuellement ». Exemple : toutes les
erreurs disque et NTFS sur 7 jours :

```xml
<QueryList>
  <Query Id="0" Path="System">
    <Select Path="System">
      *[System[(Level=2 or Level=3)
        and (EventID=7 or EventID=11 or EventID=55 or EventID=98 or EventID=140 or EventID=153)
        and TimeCreated[timediff(@SystemTime) &lt;= 604800000]]]
    </Select>
  </Query>
</QueryList>
```

**Vues personnalisées** : enregistrez vos filtres usuels (clic droit > Créer une vue
personnalisée) : « Erreurs disque 7 j », « Échecs logon 24 h », « Redémarrages
inattendus ». Exportables en XML pour déploiement homogène sur le parc
(import via la console ou GPO de préférences).

En PowerShell, le même filtre s'exprime via `-FilterXml` :

```powershell
$xml = @'
<QueryList><Query Id="0" Path="System"><Select Path="System">
*[System[(Level=2) and TimeCreated[timediff(@SystemTime) &lt;= 86400000]]]
</Select></Query></QueryList>
'@
Get-WinEvent -FilterXml $xml | Select-Object TimeCreated, Id, ProviderName
```

---

## 36. Abonnements d'événements (WEF) et collecteur

Le **transfert d'événements Windows (WEF)** centralise les journaux vers un
**collecteur** sans agent tiers : les sources « poussent » (push) leurs événements.

**Côté collecteur** (un serveur dédié ou le serveur de supervision) :

```cmd
:: Initialiser le service Windows Event Collector
wecutil qc /q
```

Puis créer l'abonnement (console `eventvwr.msc` > Abonnements, ou `wecutil cs`
avec un XML). Exemple minimal d'abonnement « Erreurs Système de tous les serveurs » :

```xml
<Subscription xmlns="http://schemas.microsoft.com/2006/03/windows/events/subscription">
  <SubscriptionId>Erreurs-Systeme-Serveurs</SubscriptionId>
  <Description>Erreurs du journal Systeme des serveurs</Description>
  <Uri>http://schemas.microsoft.com/wbem/wsman/1/windows/EventLog</Uri>
  <ConfigurationMode>MinLatency</ConfigurationMode>
  <Query><![CDATA[
    <QueryList><Query Id="0"><Select Path="System">*[System[Level=2]]</Select></Query></QueryList>
  ]]></Query>
  <LogFile>ForwardedEvents</LogFile>
</Subscription>
```

**Côté sources** (GPO) :

```text
Configuration ordinateur > Stratégies > Modèles d'administration >
Composants Windows > Transfert d'événements > Configurer le gestionnaire
d'abonnement cible :
  Server=http://collecteur.contoso.local:5985/wsman/SubscriptionManager/WEC,Refresh=3600
```

Points de vigilance : WinRM/HTTP ouvert (5985), compte du collecteur autorisé
(groupe « Lecteurs du journal des événements » sur les sources), taille du journal
`ForwardedEvents` dimensionnée (plusieurs Go sur un gros parc).

---

## 37. Journal Système : ce qu'on y cherche

| ID | Source | Signification | Réflexe |
|---|---|---|---|
| 41 | Kernel-Power | Arrêt brutal (coupure, plantage) | Corréler avec onduleur/BSOD |
| 1074 | User32 | Arrêt/redémarrage initié (qui, pourquoi) | Vérifier les reboots planifiés |
| 6005/6006/6008 | EventLog | Démarrage/arrêt du journal ; 6008 = arrêt inattendu | Compter les 6008/semaine |
| 55, 98, 140, 153 | Ntfs / disk | Corruption / erreurs disque | SMART + chkdsk (§25, §52) |
| 7, 11, 15 | disk | Erreur du pilote de disque | Câble/contrôleur/disque |
| 1001 | BugCheck | BSOD enregistré (avec code) | Analyse dump (§72) |
| 7023/7031/7034 | Service Control Manager | Service en échec / arrêté inopinément | Dépendances, compte de service |
| 7045 | Service Control Manager | **Nouveau service installé** | Suspect si non planifié (persistance malveillante) |
| 19/20 | WindowsUpdateClient | Échec d'installation de MAJ | Code d'erreur Windows Update |

Requête hebdo type (à mettre dans la vue personnalisée §35) : niveaux
Critique/Erreur du journal Système, groupés par ID (§33).

---

## 38. Journal Sécurité : audit et alertes

Prérequis : la **stratégie d'audit** doit être activée (GPO : `Configuration
ordinateur > Paramètres Windows > Paramètres de sécurité > Stratégies locales >
Stratégie d'audit`, ou audit avancé). Sans audit, le journal Sécurité est vide
d'intérêt.

| ID | Signification | Alerte si… |
|---|---|---|
| 4624 | Ouverture de session réussie (type 2 locale, 3 réseau, 10 RDP) | Connexion RDP inhabituelle (heure, IP) |
| 4625 | Échec d'ouverture de session | Pic soudain = attaque par force brute |
| 4634/4647 | Fermeture de session | — |
| 4648 | Ouverture de session avec identifiants explicites | Usage anormal (runas massif) |
| 4672 | Privilèges spéciaux attribués | Associé à un logon admin |
| 4720/4728/4732/4756 | Création de compte / ajout à un groupe (sensibles) | Toute occurrence non planifiée |
| 4726 | Suppression de compte | Vérifier le bien-fondé |
| 4738 | Modification de compte | — |
| 4740 | Compte verrouillé | Pic = brute force ou service avec vieux mot de passe |
| 4768/4769 | Tickets Kerberos TGT/TGS | Anomalies (chiffrements faibles) |
| 1102 | Journal d'audit effacé | **Toujours suspect** |

Surveillance minimale sur les DC : 4625 (pics), 4740 (pics), 4720/4728/4732/4756
(toute occurrence), 1102 (toute occurrence). Remontez-les via WEF (§36) ou votre
SIEM.

```powershell
# Comptes verrouillés dans les dernières 24 h (sur un DC)
Get-WinEvent -FilterHashtable @{ LogName='Security'; Id=4740; StartTime=(Get-Date).AddDays(-1) } |
  Select-Object TimeCreated, @{n='Compte'; e={ $_.Properties[0].Value }} |
  Format-Table -AutoSize
```

---

## 39. Journaux Active Directory : DS, DNS Server, DFSR

Sur un contrôleur de domaine, ajoutez ces journaux à la surveillance :

| Journal (Applications et services) | IDs clés | Sens |
|---|---|---|
| Directory Service | 1084, 1311, 1864 | Réplication AD : erreurs, latence |
| DNS Server | 4015, 4004, 4521 | Échecs DNS sur le DC |
| DFS Replication | 2213, 2104, 5002 | Réplication SYSVOL/DFSR en panne |
| Active Directory Web Services | — | Requêtes ADWS (PowerShell AD) |

Contrôle express de la réplication (quotidien, §5) :

```cmd
:: Résumé de la réplication : toute erreur > 0 est à traiter
repadmin /replsummary

:: Détail des échecs
repadmin /showrepl * /errorsonly

:: État des contrôleurs du domaine
dcdiag /q
```

> `dcdiag` sans `/q` est verbeux : en routine, `/q` (erreurs seules) suffit.
> Un `dcdiag` propre + `repadmin` propre = un AD sain à 95 %.

---

## 40. Journaux DHCP, NPS et autres rôles

| Rôle | Journal / outil | À surveiller |
|---|---|---|
| DHCP | `Microsoft > Windows > DHCP-Server` (opérationnel) + `C:\Windows\System32\dhcp\DhcpSrvLog-*.log` | Étendue saturée (ID 1020), conflits, NACK |
| NPS (RADIUS) | Journaux NPS (`C:\Windows\System32\LogFiles\IN*.log`) | Échecs d'authentification Wi-Fi/VPN |
| Serveur de fichiers | Journal Sécurité (audit d'accès si activé) + SMBServer/SMBClient | Erreurs SMB, partages inaccessibles |
| RDS | `Microsoft > Windows > TerminalServices-*` | Échecs de connexion, licences |
| Hyper-V | `Microsoft > Windows > Hyper-V-*` | Arrêts VM, réplication |

Exemple : détecter une étendue DHCP saturée :

```powershell
Get-WinEvent -LogName 'Microsoft-Windows-DHCP-Server/Operational' |
  Where-Object Id -eq 1020 | Select-Object -First 5 TimeCreated, Message
```

> Les logs texte DHCP (`DhcpSrvLog-Lun.log`) sont au format CSV daté : pensez à
> les purger/rotater comme les logs IIS (§30).

---

## 41. Rétention et archivage des journaux

Dimensionnement par défaut (souvent trop juste sur serveur) :

```powershell
# Voir la taille max et la rétention actuelles
wevtutil gl System
wevtutil gl Security

# Augmenter : journal Sécurité à 1 Go, écrasement des plus anciens si besoin
wevtutil sl Security /ms:1073741824
# Ou : archiver au lieu d'écraser (ne pas écraser, archiver)
wevtutil sl Security /rt:false /ab:true
```

**Politique type :**

- Serveurs : Système/Sécurité ≥ 512 Mo–1 Go, rétention ≥ 90 jours (ou transfert
  vers collecteur/SIEM).
- DC : Sécurité ≥ 1 Go (les audits sont volumineux).
- Archivage : export `.evtx` mensuel vers un partage d'archives
  (`wevtutil epl`), conservé 1 an (contraintes légales/assurance à vérifier
  avec votre DPO).

> Un journal Sécurité qui écrase tous les 3 jours = aucune capacité d'enquête
> a posteriori. C'est un problème d'exploitation, pas de « place disque ».

---

## 42. Planificateur de tâches : concepts

Le **Planificateur de tâches** (`taskschd.msc`) exécute les automatismes Windows.
Vocabulaire :

| Concept | Rôle |
|---|---|
| **Action** | Quoi faire (programme, script, e-mail déprécié) |
| **Déclencheur** | Quand (horaire, au démarrage, à l'ouverture de session, sur événement) |
| **Condition** | Contraintes (secteur, réseau, batterie) |
| **Principal** | Sous quel compte (SYSTEM, utilisateur, gMSA) |
| **Paramètres** | Comportement (relance en cas d'échec, arrêt si trop long) |

Toujours configurer : **« Arrêter la tâche si elle s'exécute plus de X »**
(évite les scripts bloqués qui s'accumulent) et **« Si la tâche échoue, redémarrer
toutes les X minutes »** (résilience).

---

## 43. Créer une tâche en PowerShell

```powershell
# Exemple : purge des logs IIS tous les dimanches à 03:00, en SYSTEM
$action    = New-ScheduledTaskAction -Execute 'powershell.exe' `
               -Argument '-NoProfile -ExecutionPolicy Bypass -File "C:\Scripts\Purge-IISLogs.ps1"'
$trigger   = New-ScheduledTaskTrigger -Weekly -DaysOfWeek Sunday -At '03:00'
$principal = New-ScheduledTaskPrincipal -UserId 'SYSTEM' -LogonType ServiceAccount -RunLevel Highest
$settings  = New-ScheduledTaskSettingsSet -ExecutionTimeLimit '02:00:00' `
               -RestartCount 3 -RestartInterval (New-TimeSpan -Minutes 15) `
               -StartWhenAvailable
Register-ScheduledTask -TaskName 'Maintenance\Purge logs IIS' `
  -Action $action -Trigger $trigger -Principal $principal -Settings $settings `
  -Description 'Purge des logs IIS de +30 jours (guide §30)'
```

Gestion courante :

```powershell
Get-ScheduledTask -TaskPath '\Maintenance\' | Get-ScheduledTaskInfo |
  Select-Object TaskName, LastRunTime, LastTaskResult
# LastTaskResult = 0 (0x0) : OK ; 0x1 : code de sortie 1 ; autre : à investiguer

# Exécuter immédiatement / désactiver / supprimer
Start-ScheduledTask -TaskName 'Maintenance\Purge logs IIS'
Disable-ScheduledTask -TaskName 'Maintenance\Purge logs IIS'
Unregister-ScheduledTask -TaskName 'Maintenance\Purge logs IIS' -Confirm:$false
```

> Organisez vos tâches dans des **dossiers** (`\Maintenance\`, `\Rapports\`,
> `\Sauvegardes\`) : un planificateur avec 80 tâches à la racine est ingérable.

---

## 44. Déclencheurs avancés et conditions

Au-delà de l'horaire simple :

```powershell
# Déclencheur sur événement : à chaque échec du spouleur (ID 372, journal System)
$triggerEvt = New-ScheduledTaskTrigger -AtLogOn  # base, remplacée ci-dessous
# Les déclencheurs sur événement se définissent via CIM/XML :
$task = Get-ScheduledTask -TaskName 'Maintenance\Surveillance spouleur'
$task.Triggers[0].Subscription = @'
<QueryList><Query Id="0" Path="System">
<Select Path="System">*[System[Provider[@Name='Microsoft-Windows-PrintService'] and EventID=372]]</Select>
</Query></QueryList>
'@
Set-ScheduledTask -InputObject $task

# Conditions utiles (objet Settings) :
$settings = New-ScheduledTaskSettingsSet -StartWhenAvailable `
  -RunOnlyIfNetworkAvailable -AllowStartIfOnBatteries:$false `
  -DontStopIfGoingOnBatteries:$false -ExecutionTimeLimit '01:00:00'
```

Cas d'usage : redémarrer un service après un événement précis, lancer un script
d'inventaire à l'ouverture de session, purger uniquement si le réseau est
disponible (copie vers partage).

---

## 45. Comptes d'exécution : SYSTEM, comptes de service, gMSA

| Compte | Droits | Usage |
|---|---|---|
| **SYSTEM** | Locaux illimités, réseau = compte machine `DOMAINE\MACHINE$` | Tâches purement locales |
| **SERVICE LOCAL / SERVICE RÉSEAU** | Restreints | Services Windows, rarement les tâches |
| Compte de service dédié (utilisateur AD) | Ce que vous lui donnez | Tâches accédant à des ressources réseau spécifiques |
| **gMSA** | Géré par AD, mot de passe auto-roté | **Recommandé** pour tâches/services (§46) |
| Compte utilisateur « réel » | — | **À proscrire** (mot de passe qui expire = tâche en échec) |

**Règle** : une tâche qui échoue avec `0x41306` ou « le mot de passe a expiré »
signale presque toujours un compte utilisateur classique : migrez vers gMSA.

Droits minimaux : accordez au compte **exactement** ce qu'il faut (lecture sur le
partage, écriture dans le dossier de logs) via des groupes AD dédiés
(`GRP-Taches-Maintenance`), jamais « Administrateurs du domaine » par facilité.

---

## 46. gMSA : création et utilisation

Les **comptes de service gérés de groupe** (gMSA) : AD gère et fait tourner le mot
de passe automatiquement (tous les 30 jours). Idéal pour tâches planifiées et
services applicatifs.

```powershell
# --- Sur un contrôleur de domaine (une seule fois par forêt) ---
# 1. Créer la clé racine KDS (attendre ~10 h de réplication en prod ; -EffectiveTime immédiat en labo)
Add-KdsRootKey -EffectiveTime ((Get-Date).AddHours(-10))

# 2. Créer le gMSA (autoriser les serveurs à récupérer le mot de passe)
New-ADServiceAccount -Name 'gmsa-taches-maint' -DNSHostName 'gmsa-taches-maint.contoso.local' `
  -PrincipalsAllowedToRetrieveManagedPassword 'GRP-SRV-Maintenance'

# --- Sur chaque serveur autorisé ---
Install-ADServiceAccount -Identity 'gmsa-taches-maint'
Test-ADServiceAccount -Identity 'gmsa-taches-maint'   # doit retourner True

# Utilisation dans une tâche planifiée (le $ final est obligatoire, pas de mot de passe)
$principal = New-ScheduledTaskPrincipal -UserId 'contoso\gmsa-taches-maint$' -LogonType Password
Register-ScheduledTask -TaskName 'Maintenance\Rapport quotidien' -Action $action `
  -Trigger $trigger -Principal $principal -Settings $settings
```

> Le compte gMSA a besoin du droit **« Ouvrir une session en tant que tâche »**
> (SeBatchLogonRight) : accordé via la GPO « Ouvrir une session en tant que tâche ».
> Sans lui, la tâche échoue avec une erreur d'ouverture de session.

---

## 47. Audit et hygiène des tâches planifiées

Revue **hebdomadaire** (§7) : listez les tâches en échec et les tâches suspectes.

```powershell
# Tâches dont la dernière exécution a échoué (résultat != 0)
Get-ScheduledTask | Get-ScheduledTaskInfo |
  Where-Object { $_.LastTaskResult -ne 0 -and $_.LastRunTime -gt (Get-Date).AddDays(-7) } |
  Select-Object TaskName, TaskPath, LastRunTime,
    @{n='ResultHex'; e={ '0x{0:X}' -f $_.LastTaskResult }} |
  Format-Table -AutoSize

# Tâches exécutées sous un compte utilisateur (à migrer vers gMSA/SYSTEM)
Get-ScheduledTask | Where-Object {
    $_.Principal.UserId -notmatch '^(SYSTEM|.*\$|NT AUTHORITY\\|BUILTIN\\)'
} | Select-Object TaskName, TaskPath, @{n='Compte'; e={ $_.Principal.UserId } } |
  Format-Table -AutoSize
```

**Hygiène sécurité** : les tâches planifiées sont une technique de persistance
classique des attaquants. Toute tâche inconnue (nom bizarre, exécutable dans
`%TEMP%` ou `%APPDATA%`, compte SYSTEM + script obscur) = incident de sécurité
potentiel : isolez, ne supprimez pas avant analyse (gardez une copie du script).

---

## 48. Disques : gestion avec PowerShell et MMC

`diskmgmt.msc` pour le visuel, PowerShell pour l'industrialisation :

```powershell
# Vue d'ensemble : disques physiques, bus, santé
Get-PhysicalDisk | Select-Object FriendlyName, MediaType, BusType, HealthStatus,
  OperationalStatus, @{n='Go'; e={ [math]::Round($_.Size / 1GB) }} | Format-Table -AutoSize

# Volumes et espace libre
Get-Volume | Where-Object DriveType -eq 'Fixed' |
  Select-Object DriveLetter, FileSystemType, HealthStatus,
    @{n='TotalGo'; e={ [math]::Round($_.Size / 1GB, 1) }},
    @{n='LibreGo'; e={ [math]::Round($_.SizeRemaining / 1GB, 1) }},
    @{n='LibrePct'; e={ [math]::Round($_.SizeRemaining / $_.Size * 100, 1) }} |
  Format-Table -AutoSize

# Initialiser / partitionner / formater un nouveau disque (ATTENTION : destructif)
Initialize-Disk -Number 2 -PartitionStyle GPT
New-Partition -DiskNumber 2 -UseMaximumSize -DriveLetter E |
  Format-Volume -FileSystem NTFS -NewFileSystemLabel 'Donnees' -Confirm:$false
```

> ⚠️ `Initialize-Disk` / `Clear-Disk` **détruisent les données**. Vérifiez trois
> fois le numéro de disque (`Get-Disk | ft Number, FriendlyName, Size`) avant
> toute opération, surtout à distance.

---

## 49. Partitionnement : bonnes pratiques

| Principe | Détail |
|---|---|
| GPT, pas MBR | MBR limité à 2 To ; GPT obligatoire en UEFI |
| Séparer OS et données | C: système, D: données/logs, volumes dédiés par rôle |
| Alignement | Laisser l'assistant Windows gérer (1 Mo par défaut, OK pour SSD/RAID) |
| Taille C: serveur | ≥ 100 Go (120-150 Go confortable avec WinSxS + logs) |
| Lettres stables | Figer les lettres via `Set-Partition -NewDriveLetter` ; documenter |
| ReFS vs NTFS | NTFS par défaut ; ReFS pour gros volumes de données/Sauvegardes (intégrité) |

Redimensionner (étendre un volume sur de l'espace adjacent libre) :

```powershell
# Étendre D: au maximum disponible
$part = Get-Partition -DriveLetter D
Resize-Partition -DriveLetter D -Size (Get-PartitionSupportedSize -DriveLetter D).SizeMax
```

> Pour **réduire** un volume système, l'espace doit être contigu en fin de volume ;
> les fichiers non déplaçables (pagefile, clichés) peuvent bloquer : désactivez-les
> temporairement si besoin, en fenêtre de maintenance.

---

## 50. Quotas NTFS

Deux mécanismes, ne pas confondre :

| Mécanisme | Portée | Outil |
|---|---|---|
| **Quotas NTFS** | Par volume, par utilisateur | `fsutil quota` / propriétés du volume |
| **FSRM** (File Server Resource Manager) | Par dossier, bien plus fin (seuils, notifications, blocage par type de fichier) | Rôle FSRM + `dirquota` |

Quota NTFS express (volume E:, 10 Go par utilisateur, avertissement à 8 Go) :

```cmd
:: Activer le suivi + appliquer les quotas sur E:
fsutil quota enforce E:
:: Définir un seuil par défaut : limite 10 Go, avertissement 8 Go (en octets)
fsutil quota modify E: 10737418240 8589934592 "contoso\utilisateur-fic"
```

**Recommandation** : sur un serveur de fichiers d'entreprise, utilisez **FSRM**
(quotas par dossier partagé, modèles, rapports de fichiers volumineux, File
Screening anti-ransomwares par extension). Le quota NTFS simple suffit pour des
besoins ponctuels (ex. limiter les profils sur un volume).

---

## 51. Optimisation : défragmentation HDD, TRIM SSD

| Média | Action | Fréquence |
|---|---|---|
| HDD | Défragmentation (`-Defrag`) | Hebdo (tâche intégrée `\Microsoft\Windows\Defrag\ScheduledDefrag`) |
| SSD | **ReTrim** (informe le contrôleur des blocs libres), jamais de défragmentation classique | Mensuelle (intégrée) |

```powershell
# État d'optimisation de tous les volumes
Get-Volume | Where-Object DriveType -eq 'Fixed' | ForEach-Object {
    Optimize-Volume -DriveLetter $_.DriveLetter -Analyze -Verbose
}

# Forcer : défragmentation HDD / ReTrim SSD (Windows choisit selon le média avec -ReTrim/-Defrag explicites)
Optimize-Volume -DriveLetter D -Defrag -Verbose      # HDD
Optimize-Volume -DriveLetter C -ReTrim -Verbose       # SSD

# Vérifier que la tâche planifiée intégrée est active
Get-ScheduledTask -TaskName 'ScheduledDefrag' -TaskPath '\Microsoft\Windows\Defrag\'
```

> Sur **VM**, la défragmentation du guest a peu d'intérêt si le stockage sous-jacent
> est déjà optimisé (SAN/SSD) : elle consomme des IOPS pour rien. Désactivez-la sur
> les VM à stockage flash et surveillez plutôt la latence (§57).

---

## 52. Santé des disques : SMART

Windows expose les compteurs de fiabilité via le sous-système de stockage :

```powershell
# Compteurs SMART/fiabilité : usure SSD, secteurs réalloués, température
Get-PhysicalDisk | Get-StorageReliabilityCounter |
  Select-Object DeviceId, Wear, TemperatureCelsius, ReadErrorsTotal,
    WriteErrorsTotal, PowerOnHours | Format-Table -AutoSize
# Wear = % d'usure (SSD) ; au-delà de 80-90 %, planifiez le remplacement
```

**Politique d'exploitation :**

- `Wear` > 80 % → remplacement planifié au prochain trimestre.
- Erreurs de lecture/écriture qui augmentent → remplacement prioritaire +
  vérification des sauvegardes.
- Température > 55 °C en continu → ventilation/positionnement à revoir.
- Complétez avec l'outil du constructeur (diagnostic long) avant de déclarer un
  disque « sain » après des erreurs.

> Le SMART n'est pas une boule de cristal : ~1/3 des pannes disque ne sont pas
> précédées d'alerte SMART. D'où l'importance des sauvegardes (§123) et du RAID /
> Storage Spaces (§53), jamais du « disque unique avec SMART au vert ».

---

## 53. Storage Spaces : concepts et administration

Les **espaces de stockage** mutualisent des disques physiques en pools, puis en
disques virtuels avec résilience (miroir, parité).

| Type de résilience | Disques min | Tolérance | Usage |
|---|---|---|---|
| Simple (sans résilience) | 1 | Aucune | Données temporaires |
| Miroir double | 2 | 1 disque | Usage général |
| Miroir triple | 5 | 2 disques | Données critiques |
| Parité simple | 3 | 1 disque | Archivage (écritures lentes) |
| Parité double | 7 | 2 disques | Archivage critique |

Administration :

```powershell
# État du pool et des disques virtuels
Get-StoragePool | Where-Object IsPrimordial -eq $false |
  Select-Object FriendlyName, HealthStatus, OperationalStatus,
    @{n='TotalTo'; e={ [math]::Round($_.Size / 1TB, 2) }} | Format-Table -AutoSize
Get-VirtualDisk | Select-Object FriendlyName, ResiliencySettingName, HealthStatus,
  OperationalStatus, @{n='To'; e={ [math]::Round($_.Size / 1TB, 2) }} | Format-Table -AutoSize

# Réparer un disque virtuel dégradé après remplacement du disque physique
Repair-VirtualDisk -FriendlyName 'DataMirror'

# Ajouter un disque au pool
Add-PhysicalDisk -StoragePoolFriendlyName 'PoolData' -PhysicalDisks (Get-PhysicalDisk -CanPool $true)
```

**En production** : surveillez `HealthStatus`/`OperationalStatus` (remontée
supervision §106) et testez la **reconstruction** avant d'en avoir besoin.
Pour du SDS sérieux en cluster, regardez **Storage Spaces Direct (S2D)** —
hors périmètre de ce guide, voir §138.

---

## 54. Performance : les outils (Gestionnaire des tâches, Resource Monitor, perfmon)

| Outil | Lancement | Usage |
|---|---|---|
| Gestionnaire des tâches | `taskmgr` / Ctrl+Maj+Échap | Vue instantanée, processus gourmands, démarrage |
| **Analyseur de ressources** | `resmon` | **Le meilleur premier réflexe** : CPU/disque/réseau/mémoire par processus, en temps réel |
| Analyseur de performances | `perfmon` | Compteurs précis, jeux de collecteurs, historique |
| Compteurs PowerShell | `Get-Counter` | Scriptable, à distance |

**Méthode express** (2 minutes) : `resmon` → onglet Disque : trier par « Total
(octets/s) » → le processus en tête est le coupable dans 80 % des « le serveur
est lent ». Onglet Réseau : idem pour la bande passante. Onglet UC : vérifier la
« Longueur de la file du processeur ».

---

## 55. Compteurs clés : CPU

| Compteur | Seuil d'alerte | Interprétation |
|---|---|---|
| `\Processor(_Total)\% Processor Time` | > 85 % sur 15 min | Saturation CPU |
| `\System\Processor Queue Length` | > 4 (par cœur : > 2×nb cœurs) | Vrai goulot CPU (plus fiable que le %) |
| `\Processor(_Total)\% Privileged Time` | > 30 % du total | Activité noyau/pilotes excessive |
| `\Process(*)\% Processor Time` | — | Identifier le processus fautif |

```powershell
# CPU total + file d'attente, 5 échantillons espacés de 10 s
Get-Counter '\Processor(_Total)\% Processor Time', '\System\Processor Queue Length' `
  -SampleInterval 10 -MaxSamples 5 |
  Select-Object -ExpandProperty CounterSamples |
  Select-Object Path, CookedValue, Timestamp | Format-Table -AutoSize
```

> Un CPU à 100 % avec une file d'attente faible = un processus mono-thread qui
> boucle (pas un manque de cœurs). Cherchez le processus, pas du CPU en plus.

---

## 56. Compteurs clés : mémoire

| Compteur | Seuil | Sens |
|---|---|---|
| `\Memory\Available MBytes` | < 10 % de la RAM | Mémoire sous pression |
| `\Memory\% Committed Bytes In Use` | > 85 % | Engagement excessif (RAM + pagefile) |
| `\Memory\Pages/sec` | > 1000 soutenu | Pagination dure (disque lent = catastrophe) |
| `\Memory\Pool Nonpaged Bytes` | Croissance continue | **Fuite mémoire noyau/pilote** |
| `\Process(*)\Working Set` | — | Consommation par processus |

```powershell
# Mémoire disponible + % engagé
$os = Get-CimInstance Win32_OperatingSystem
[pscustomobject]@{
    TotalGo      = [math]::Round($os.TotalVisibleMemorySize / 1MB, 1)
    LibreGo      = [math]::Round($os.FreePhysicalMemory / 1MB, 1)
    LibrePct     = [math]::Round($os.FreePhysicalMemory / $os.TotalVisibleMemorySize * 100, 1)
    PageFileUtil = (Get-Counter '\Paging File(_Total)\% Usage').CounterSamples.CookedValue
} | Format-List
```

**Fuite mémoire** : si le pool non paginé croît sans jamais redescendre après
arrêt des applications, suspectez un pilote → `poolmon.exe` (WDK) puis Driver
Verifier (§74). Sur un serveur, une fuite = redémarrage planifié en attendant le
correctif, pas un « on verra ».

---

## 57. Compteurs clés : disque

| Compteur | Seuil | Sens |
|---|---|---|
| `\PhysicalDisk(*)\Avg. Disk sec/Read` et `/Write` | > 20 ms soutenu (HDD), > 5 ms (SSD) | **Latence** : le vrai indicateur |
| `\PhysicalDisk(*)\Disk Reads/sec`, `Disk Writes/sec` | — | IOPS |
| `\PhysicalDisk(*)\% Disk Time` | > 90 % | Disque saturé |
| `\PhysicalDisk(*)\Current Disk Queue Length` | > 2 par disque | File d'attente |

```powershell
# Latence moyenne lecture/écriture par disque, 3 échantillons de 15 s
Get-Counter '\PhysicalDisk(*)\Avg. Disk sec/Read', '\PhysicalDisk(*)\Avg. Disk sec/Write' `
  -SampleInterval 15 -MaxSamples 3 |
  Select-Object -ExpandProperty CounterSamples |
  Where-Object InstanceName -ne '_Total' |
  Select-Object InstanceName, Path,
    @{n='LatenceMs'; e={ [math]::Round($_.CookedValue * 1000, 2) }}, Timestamp |
  Format-Table -AutoSize
```

> Sur VM : la latence vue par l'invité inclut l'hôte et la baie. Si l'invité est
> « lent disque » mais ses compteurs sont bons, regardez l'hôte/Hyper-V et le
> stockage partagé avant d'ajouter des vDisques.

---

## 58. Compteurs clés : réseau

| Compteur | Seuil | Sens |
|---|---|---|
| `\Network Interface(*)\Bytes Total/sec` | > 80 % de la capacité du lien | Saturation |
| `\Network Interface(*)\Output Queue Length` | > 2 | Congestion sortante |
| `\TCPv4\Segments Retransmitted/sec` | > 5 % des segments | Perte / qualité de lien |
| `\Network Interface(*)\Packets Received Errors` | > 0 croissant | Problème physique (câble, duplex) |

```powershell
# Débit total par interface (octets/s), 5 échantillons de 5 s
Get-Counter '\Network Interface(*)\Bytes Total/sec' -SampleInterval 5 -MaxSamples 5 |
  Select-Object -ExpandProperty CounterSamples |
  Where-Object { $_.InstanceName -notmatch 'Loopback|isatap|Teredo' } |
  Select-Object InstanceName,
    @{n='Mo/s'; e={ [math]::Round($_.CookedValue / 1MB, 2) }}, Timestamp |
  Format-Table -AutoSize
```

> Erreurs/duplex : un lien négocié en 100 Mb/s half-duplex au lieu de 1 Gb/s
> full-duplex explique bien des « lenteurs réseau » (§97, cas 12).

---

## 59. Analyseur de performances : jeux de collecteurs

Pour un problème intermittent (« lent tous les jours à 14h »), collectez au lieu
de regarder en direct : **Jeux de collecteurs de données** > **Défini par
l'utilisateur** > Nouveau > « Créer manuellement » > Compteur de performances.

Jeu « Diagnostic standard » (modèle à créer une fois, réutiliser partout) :

- `\Processor(_Total)\% Processor Time`, `\System\Processor Queue Length`
- `\Memory\Available MBytes`, `\Memory\Pages/sec`, `\Memory\Pool Nonpaged Bytes`
- `\PhysicalDisk(*)\Avg. Disk sec/Read`, `\PhysicalDisk(*)\Avg. Disk sec/Write`
- `\Network Interface(*)\Bytes Total/sec`
- Intervalle : 15 s. Durée : 24-72 h. Format : binaire circulaire (limite 500 Mo).

Planifiez le démarrage/arrêt (ou laissez tourner en circulaire) puis analysez dans
perfmon : ajoutez les compteurs, repérez les pics corrélés à l'heure du symptôme.
**Corrélation > valeur absolue** : un pic disque à 14h02 tous les jours = un batch,
pas un disque mourant.

---

## 60. PAL : Performance Analysis of Logs

**PAL (Performance Analysis of Logs)** est un outil gratuit (PowerShell + seuils
documentés) qui analyse un journal de compteurs perfmon et génère un rapport HTML
avec alertes (« seuil dépassé ») : idéal pour objectiver un problème de perfs
devant un éditeur ou la direction.

Usage type :

1. Collectez avec le jeu §59 (format `.blg`).
2. Passez le `.blg` dans PAL avec le seuil correspondant au rôle
   (« File Server », « SQL Server », « Terminal Services »…).
3. Le rapport signale les compteurs hors seuils avec explications.

> Mention : PAL n'est plus maintenu activement mais reste fonctionnel et ses
> **seuils** demeurent une excellente référence documentaire, même lus à la main.

---

## 61. Scripts de collecte de performances

Collecte légère et scriptable sans perfmon (à pousser via PSRemoting, §113) :

```powershell
# Snapshot de santé perf : CPU, mémoire, disque, réseau — une ligne par serveur
$servers = Get-Content C:\Scripts\serveurs.txt
Invoke-Command -ComputerName $servers {
    $cpu  = (Get-Counter '\Processor(_Total)\% Processor Time' -SampleInterval 5 -MaxSamples 3 |
             Select-Object -ExpandProperty CounterSamples | Measure-Object CookedValue -Average).Average
    $os   = Get-CimInstance Win32_OperatingSystem
    $disk = Get-Counter '\PhysicalDisk(_Total)\Avg. Disk sec/Read' -ErrorAction SilentlyContinue |
             Select-Object -ExpandProperty CounterSamples
    [pscustomobject]@{
        Machine   = $env:COMPUTERNAME
        CPU_Pct   = [math]::Round($cpu, 1)
        MemLibrePct = [math]::Round($os.FreePhysicalMemory / $os.TotalVisibleMemorySize * 100, 1)
        LatDiskMs = if ($disk) { [math]::Round($disk.CookedValue * 1000, 1) } else { 'n/a' }
    }
} | Sort-Object CPU_Pct -Descending | Format-Table -AutoSize
```

Historisation : écrivez ce snapshot toutes les 15 min dans un CSV central
(`Export-Csv -Append`) via une tâche planifiée (§43) — vous obtenez un mini
perfmon sans infrastructure, exploitable sous Excel ou Grafana.

---

## 62. WinRE : accéder à l'environnement de récupération

**WinRE** (Windows Recovery Environment) = le mode de réparation hors ligne
(dépannage, invite de commandes, restauration système, réparation du démarrage).

Accès :

| Méthode | Détail |
|---|---|
| Paramètres > Récupération > Démarrage avancé > Redémarrer | Windows 11 |
| `shutdown /r /o /f /t 0` | Redémarre directement sur les options avancées |
| Touche Maj + Redémarrer (menu Démarrer) | Le plus rapide |
| 2-3 démarrages ratés consécutifs | Bascule automatique (ne pas en abuser) |
| Support d'installation > Réparer l'ordinateur | Quand WinRE local est absent/corrompu |

Vérifier l'état de WinRE (doit être **Enabled**) :

```cmd
reagentc /info
```

Si « Disabled » ou image introuvable : `reagentc /enable` (nécessite la partition
de récupération et `winre.wim` présents).

---

## 63. WinRE : activer, désactiver, personnaliser

```cmd
:: État détaillé
reagentc /info

:: Activer / désactiver
reagentc /enable
reagentc /disable

:: Redéfinir l'emplacement de winre.wim (ex. après recréation de la partition)
reagentc /setreimage /path R:\Recovery\WindowsRE
```

**Bonnes pratiques :**

- Ne supprimez jamais la partition de récupération pour « gagner 500 Mo ».
- Sur les serveurs, testez l'accès WinRE **avant** d'en avoir besoin (au moins une
  fois par an, en fenêtre de maintenance).
- Gardez une **clé USB d'installation** de la même version à portée de main en
  salle serveur : c'est le WinRE de secours (§62).
- Ajoutez vos outils à WinRE via `winrecfg` / image personnalisée seulement si
  vous maîtrisez la chaîne (sinon : clé USB bootable avec vos scripts).

---

## 64. Réparation du démarrage : diagnostic

Symptômes : « Bootmgr is missing », logo Windows qui boucle, écran noir après le
logo, `0xc000000e` / `0xc0000225` (périphérique inaccessible).

Méthode (depuis WinRE > Dépannage > Invite de commandes) :

1. **Identifier les volumes** : les lettres changent sous WinRE ! `diskpart` >
   `list volume` — notez quelle lettre porte `\Windows`.
2. **Réparation automatique** d'abord : Dépannage > Réparation du démarrage
   (règle les cas simples : BCD corrompu léger).
3. Si échec : **manuel** avec `bcdedit` (§65) puis `bootrec` (§66).
4. En UEFI : la partition EFI (FAT32, ~100 Mo) doit être montée pour `bcdboot`.

```cmd
:: Sous WinRE : lister les volumes et trouver Windows + partition EFI
diskpart
list volume
:: (repérez : volume Windows = ex. C:, volume EFI = FAT32 "EFI")
exit
```

---

## 65. BCD : bcdedit en pratique

Le **BCD** (Boot Configuration Data) remplace `boot.ini`. Lecture :

```cmd
:: Magasin BCD courant (depuis Windows démarré)
bcdedit /enum

:: Depuis WinRE : préciser le magasin
bcdedit /store C:\Boot\BCD /enum
```

Opérations courantes :

```cmd
:: Sauvegarder le BCD avant toute modification (OBLIGATOIRE)
bcdedit /export C:\Temp\bcd-backup

:: Restaurer
bcdedit /import C:\Temp\bcd-backup

:: Définir l'OS par défaut / le délai du menu
bcdedit /default {current}
bcdedit /timeout 10

:: Recréer les entrées de démarrage (UEFI) : S: = lettre de la partition EFI montée
bcdboot C:\Windows /s S: /f UEFI
:: Variante BIOS/MBR
bcdboot C:\Windows /s C: /f BIOS
```

> `bcdboot` **recrée** des fichiers de démarrage sains à partir du Windows
> installé : c'est souvent plus efficace que de rafistoler un BCD corrompu.

---

## 66. bootrec : /fixmbr, /fixboot, /rebuildbcd

Depuis l'invite WinRE (surtout utile en **BIOS/MBR** ; en UEFI préférez `bcdboot`
§65) :

```cmd
:: Réécrire le MBR (sans toucher à la table de partitions)
bootrec /fixmbr

:: Réécrire le secteur de démarrage de la partition système
bootrec /fixboot
:: Si "Accès refusé" sous WinRE : monter la partition EFI puis bcdboot (§65),
:: ou : diskpart > sel vol EFI > assign letter=S: > exit, puis bcdboot C:\Windows /s S: /f UEFI

:: Rechercher les installations Windows et reconstruire le BCD
bootrec /scanos
bootrec /rebuildbcd
```

Séquence type MBR corrompu : `bootrec /fixmbr` → `bootrec /fixboot` →
`bootrec /rebuildbcd` → redémarrage. Si le disque n'est pas vu du tout
(`scanos` ne trouve rien) : problème **matériel/BIOS** (disque, câble, contrôleur),
pas logiciel — ne perdez pas 2 h sur bootrec.

---

## 67. Restauration du système et clichés instantanés

La **restauration du système** (points de restauration) annule les changements
système (pilotes, registre, correctifs) sans toucher aux données utilisateurs.

```powershell
# Vérifier / activer la protection sur C:
vssadmin list shadows
Get-ComputerRestorePoint
Enable-ComputerRestore -Drive 'C:\'

# Créer un point manuel avant une opération à risque
Checkpoint-Computer -Description 'Avant MAJ pilote RAID' -RestorePointType 'MODIFY_SETTINGS'
```

Restauration : WinRE > Dépannage > Restauration du système, ou `rstrui.exe` depuis
Windows.

**Limites** : désactivée par défaut sur beaucoup de serveurs ; ne remplace pas une
sauvegarde (§123) ; inutile contre une corruption de données applicatives.
**Politique** : activez-la sur les postes (avec espace réservé 5-10 %) et créez un
point avant chaque changement à risque (scriptable, ci-dessus).

---

## 68. Réinitialisation et réinstallation propre

Quand le système est trop dégradé (ou pour recycler un poste) :

| Option | Effet | Quand |
|---|---|---|
| **Réinitialisation** (« Réinitialiser ce PC ») | Réinstalle Windows, conserve ou non les fichiers | Poste très instable, avant réaffectation |
| **Nouvelle installation** (USB/ISO) | Système neuf | Serveur corrompu sans sauvegarde système, ou standardisation |
| **Réinstallation via image maître** (MDT/Intune Autopilot) | Poste conforme au standard | **À privilégier** en entreprise |

```powershell
# Lancer la réinitialisation (interactive)
systemreset.exe
# Réinitialisation "tout supprimer" en ligne de commande (poste à recycler)
systemreset.exe --cleanpc   # selon version : ouvre l'assistant
```

> En entreprise, un poste qui cumule les problèmes système récurrents ne se
> « répare » pas indéfiniment : on le **réinstalle depuis l'image standard**.
> C'est plus rapide et plus fiable que 3 h de DISM/SFC/chkdsk en cascade.
> Pour les serveurs : même logique — un serveur « rafistolé » reste fragile ;
> reconstruisez-le proprement puis restaurez données et rôles (d'où l'importance
> de la documentation §128 et des sauvegardes §123).

---

## 69. BSOD : lire un écran bleu

Un **BSOD** (écran bleu) = le noyau a rencontré une erreur irrécupérable et a
stoppé la machine pour protéger les données. L'écran affiche :

- Le **code d'arrêt** (ex. `IRQL_NOT_LESS_OR_EQUAL`) + sa valeur hexadécimale
  (ex. `0x0000000A`) ;
- Parfois le **pilote fautif** (ex. `ntoskrnl.exe`, `nvlddmkm.sys`) ;
- Un QR code (peu utile en pratique).

**Premiers réflexes :**

1. **Photographiez l'écran** (code + pilote) : c'est souvent la seule trace si
   aucun dump n'est configuré.
2. Notez le contexte : que faisait la machine ? (mise à jour récente ? nouveau
   matériel/pilote ? charge particulière ?)
3. Redémarrez : BSOD **isolé** après un patch = à surveiller ; BSOD **répété** =
   diagnostic (§72-74).
4. Récupérez le dump (`C:\Windows\Minidump\`, §70) **avant** tout nettoyage.

---

## 70. Configurer les fichiers de vidage (dump)

Sans dump configuré, impossible d'analyser un BSOD après coup. Vérifiez :

```powershell
# Configuration actuelle des vidages
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' |
  Select-Object CrashDumpEnabled, DumpFile, MinidumpDir, AutoReboot, LogEvent

# Valeurs de CrashDumpEnabled : 1 = complet, 2 = noyau, 3 = petit (minidump 256 Ko),
# 7 = automatique (recommandé : gère la taille selon la RAM)
```

Réglage recommandé **serveurs** : **vidage automatique** (7) — ni le complet
(taille = RAM, trop lourd) ni le seul minidump (parfois insuffisant). Vérifiez
aussi :

- Espace libre ≥ taille de la RAM + 1 Go sur le volume du dump ;
- Fichier d'échange sur le volume système ≥ 800 Mo (requis pour écrire le dump) ;
- `DumpFile` = `%SystemRoot%\MEMORY.DMP`, `MinidumpDir` = `%SystemRoot%\Minidump`.

> Après un BSOD, **copiez** `MEMORY.DMP` / le `.dmp` ailleurs avant toute
> manipulation : un nouveau BSOD écrase le précédent, et certains nettoyages
> suppriment les dumps (§28).

---

## 71. WinDbg : installation et prise en main

**WinDbg** (Windows Debugger) est l'outil d'analyse des dumps. Installation :

1. **Microsoft Store** : « WinDbg » (version moderne, recommandée) ; ou via le
   **SDK Windows** (option Debugging Tools).
2. Au premier lancement : configurez le **chemin des symboles** (indispensable
   pour une analyse lisible) :

```text
.sympath srv*C:\Symbols*https://msdl.microsoft.com/download/symbols
```

3. Ouvrez le dump : Fichier > Open dump file > `C:\Windows\Minidump\*.dmp`.

Commandes de base une fois le dump chargé :

```text
!analyze -v        :: analyse automatique détaillée (le 90 % du travail)
k                 :: pile d'appels (stack)
lm                :: modules/pilotes chargés
!thread           :: thread fautif
```

> Les symboles se téléchargent à la première analyse (connexion Internet requise,
> quelques minutes). Sans symboles corrects, `!analyze` reste approximatif.

---

## 72. Analyser un minidump : !analyze -v

`!analyze -v` produit un rapport dont les lignes clés sont :

```text
IRQL_NOT_LESS_OR_EQUAL (a)
...
Probably caused by : driver-fic.sys ( driver_fic+1234 )
...
STACK_TEXT:
...
IMAGE_NAME:  driver-fic.sys
FAILURE_BUCKET_ID:  AV_driver-fic!Unknown_Function
```

Lecture :

| Champ | Sens |
|---|---|
| Code d'arrêt + nom | La famille d'erreur (§73) |
| `Probably caused by` | Le suspect n°1 (indication, pas preuve absolue) |
| `IMAGE_NAME` | Le module incriminé |
| `STACK_TEXT` | La pile d'appels : ce que faisait le noyau |
| `FAILURE_BUCKET_ID` | Signature : cherchez-la telle quelle sur le web |

**Conduite à tenir :**

1. Si le fautif est un **pilote tiers** (`*.sys` non-Microsoft) : mettez-le à jour,
   revenez à la version précédente, ou désactivez le matériel/logiciel associé.
2. Si c'est `ntoskrnl.exe` / `hal.dll` : c'est rarement le vrai coupable — cherchez
   plus bas dans la pile, suspectez **RAM** (test mémoire) ou **pilote** qui a
   corrompu le noyau.
3. BSOD variés et changeants à chaque crash = **matériel** (RAM, alimentation,
   surchauffe) dans 80 % des cas, pas un pilote.

Test mémoire intégré : `mdsched.exe` (redémarrage requis) ; en doute, MemTest86+
depuis une clé USB.

---

## 73. Codes d'arrêt courants : tableau de référence

| Code | Nom | Cause typique | Piste |
|---|---|---|---|
| `0x0A` | IRQL_NOT_LESS_OR_EQUAL | Pilote accédant à une adresse invalide | Pilote (MAJ/restauration) |
| `0x1A` | MEMORY_MANAGEMENT | RAM défectueuse ou pilote | Test mémoire, pilotes |
| `0x3B` | SYSTEM_SERVICE_EXCEPTION | Pilote/service système | Dump + MAJ pilote |
| `0x7E` | SYSTEM_THREAD_EXCEPTION_NOT_HANDLED | Pilote (souvent graphique/stockage) | Mode sans échec, restauration pilote |
| `0x50` | PAGE_FAULT_IN_NONPAGED_AREA | RAM ou pilote, antivirus | RAM, exclusion AV |
| `0x7A` | KERNEL_DATA_INPAGE_ERROR | Disque : page noyau illisible | SMART, chkdsk, câble |
| `0x77` | KERNEL_STACK_INPAGE_ERROR | Idem, pile du noyau | Disque |
| `0xF4` | CRITICAL_OBJECT_TERMINATION | Processus critique tué (souvent disque) | Disque/SMART |
| `0xEF` | CRITICAL_PROCESS_DIED | Processus système mort | Disque, corruption système (DISM/SFC) |
| `0x133` | DPC_WATCHDOG_VIOLATION | Pilote bloquant (SSD/NVMe, Wi-Fi) | Pilote stockage/réseau |
| `0x124` | WHEA_UNCORRECTABLE_ERROR | **Matériel** (CPU, RAM, PCIe) | Diag constructeur, températures |
| `0x109` | CRITICAL_STRUCTURE_CORRUPTION | Pilote vérolé / rootkit | Driver Verifier, analyse AV |
| `0xC000021A` | WINLOGON_FATAL_ERROR | Sous-système utilisateur mort | Corruption système, restauration |
| `0x21` | QUOTA_UNDERFLOW | Pilote (retour de quota incorrect) | Pilote |

---

## 74. Driver Verifier : traquer un pilote fautif

**Driver Verifier** (`verifier.exe`) soumet les pilotes à des contrôles stricts
pour faire planter **immédiatement et avec un fautif identifié** un pilote qui
corrompt silencieusement le système.

```cmd
:: Lancer l'assistant
verifier
```

Procédure :

1. « Créer des paramètres personnalisés » > cochez : Special Pool, Pool Tracking,
   Force IRQL Checking, I/O Verification, Deadlock Detection.
2. « Sélectionner les pilotes dans une liste » > cochez les pilotes **tiers
   suspects** (jamais tous les pilotes Microsoft en production).
3. Redémarrez : le système devient **plus lent** (normal) et plantera vite si un
   pilote surveillé faute → le dump désignera clairement le coupable.
4. Après diagnostic : `verifier /reset` puis redémarrage pour **désactiver**.

> ⚠️ Driver Verifier en production = risque de BSOD en boucle si le pilote est
> vraiment fautif. Prévoyez le mode sans échec (`verifier /reset` depuis WinRE si
> besoin) et une fenêtre de maintenance. Ne le laissez jamais activé « au cas où ».

---

## 75. Dépannage réseau : méthode générale

Ne cliquez pas au hasard : suivez la pyramide, du bas vers le haut.

1. **Physique** : câble branché ? voyant du switch ? Wi-Fi connecté au bon SSID ?
2. **Local** : IP valide ? (`ipconfig`, §76) Passerelle joignable ? (`ping`, §77)
3. **Résolution** : le DNS répond-il ? (`nslookup`, §81)
4. **Routage** : où ça casse ? (`tracert`/`pathping`, §77-78)
5. **Transport** : le port est-il ouvert ? (`Test-NetConnection`, §79)
6. **Applicatif** : le service répond-il ? (logs, test applicatif)

**Règle d'or** : isolez le périmètre — « est-ce que ça marche depuis une autre
machine / un autre réseau ? » divise le problème par deux à chaque fois.
Et **documentez** : heure, symptômes exacts, messages d'erreur recopiés, commandes
lancées et résultats. Un ticket « ça marche pas » est inexploitable.

---

## 76. ipconfig : lecture et cas d'usage

```cmd
:: Configuration complète : IP, masque, passerelle, DNS, DHCP, bail
ipconfig /all

:: Renouveler le bail DHCP (après correction côté serveur)
ipconfig /release
ipconfig /renew

:: Vider / afficher le cache DNS client
ipconfig /flushdns
ipconfig /displaydns
```

Lecture d'un `ipconfig /all` sain (exemple) :

```text
Carte Ethernet Ethernet0 :
   Adresse IPv4 . . . . . . . . : 10.10.20.45
   Masque de sous-réseau . . . . : 255.255.255.0
   Passerelle par défaut . . . . : 10.10.20.1
   Serveur DHCP . . . . . . . . . : 10.10.20.10
   Serveurs DNS . . . . . . . . . : 10.10.10.11
                                     10.10.10.12
   Bail obtenu . . . . . . . . . : dimanche 27 septembre 2026 08:12:03
```

Anomalies à repérer d'un coup d'œil : IP en `169.254.x.x` (APIPA → pas de DHCP,
§86), passerelle absente, DNS en `8.8.8.8` sur un poste du domaine (doit pointer
vers les DC !), suffixe DNS manquant.

---

## 77. Test-Connection, ping, pathping

```powershell
# ping moderne : 4 échos, avec résolution et temps
Test-Connection -ComputerName 10.10.20.1 -Count 4

# ping continu avec horodatage (surveillance d'une coupure)
Test-Connection -ComputerName srv-fic-01 -Count 9999 |
  Select-Object @{n='Heure';e={Get-Date -Format 'HH:mm:ss'}},
    Address, ResponseTime, StatusCode

# Équivalent CMD classique
ping -t 10.10.20.1
pathping 10.10.30.5   # ping + traceroute avec stats par saut (lent : ~5 min)
```

Interprétation :

- **Délai dépassé** vers la passerelle = problème local (câble, switch, carte).
- **OK passerelle, KO au-delà** = routage / lien inter-sites / pare-feu.
- **Pertes intermittentes** = notez le % : < 1 % tolérable, > 5 % = à traiter
  (saturation, duplex, Wi-Fi).
- ⚠️ Un ping qui ne répond pas ne prouve pas une panne : ICMP est souvent filtré.
  Confirmez toujours avec un test **TCP** sur le port réel (§79).

---

## 78. Tracert et diagnostic de routage

```cmd
:: Traceroute classique (lent : résout chaque saut)
tracert 10.10.30.5

:: Sans résolution DNS (beaucoup plus rapide, à préférer en diagnostic)
tracert -d 10.10.30.5
```

```powershell
# Équivalent PowerShell (nécessite souvent une élévation / module)
Test-NetConnection -ComputerName 10.10.30.5 -TraceRoute
```

Lecture : le saut où les `*` commencent = le point de rupture (ou un équipement
qui ne répond pas à ICMP — vérifiez que les sauts suivants répondent avant de
conclure). Comparez avec un `tracert` depuis une machine qui fonctionne : une
**route asymétrique ou changée** après une modification réseau est un classique.

Table de routage locale (routes persistantes parasites après un VPN mal
désinstallé, par exemple) :

```cmd
route print -4
:: Supprimer une route persistante fautive
route delete 10.99.0.0
```

---

## 79. Test-NetConnection : le couteau suisse

```powershell
# Le ping est-il OK + quel est le chemin ?
Test-NetConnection -ComputerName srv-fic-01

# Tester un PORT TCP précis (le vrai test : "l'application répond-elle ?")
Test-NetConnection -ComputerName srv-fic-01 -Port 445      # SMB
Test-NetConnection -ComputerName srv-ad-01  -Port 389      # LDAP
Test-NetConnection -ComputerName srv-web-01 -Port 443      # HTTPS
Test-NetConnection -ComputerName srv-rds-01 -Port 3389     # RDP

# Détail complet : IP résolue, latence, route
Test-NetConnection -ComputerName intranet.contoso.local -Port 443 -InformationLevel Detailed |
  Select-Object ComputerName, RemoteAddress, TcpTestSucceeded, PingSucceeded,
    @{n='LatenceMs';e={$_.PingReplyDetails.RoundtripTime}}, TraceRoute
```

> `TcpTestSucceeded : False` + ping OK = le service n'écoute pas, le pare-feu
> bloque, ou vous frappez à la mauvaise IP. C'est **le** test qui tranche entre
> « problème réseau » et « problème applicatif ».

---

## 80. netstat et Get-NetTCPConnection

Qui écoute quoi, qui est connecté à qui :

```cmd
:: Connexions actives + ports d'écoute, avec le PID (-o) et résolution (-b = exécutable)
netstat -ano
netstat -anob
```

```powershell
# Équivalent PowerShell, filtrable
Get-NetTCPConnection | Where-Object State -eq 'Listen' |
  Select-Object LocalAddress, LocalPort,
    @{n='PID';e={$_.OwningProcess}},
    @{n='Processus';e={(Get-Process -Id $_.OwningProcess -ErrorAction SilentlyContinue).ProcessName}} |
  Sort-Object LocalPort | Format-Table -AutoSize

# Qui occupe le port 8080 ?
Get-NetTCPConnection -LocalPort 8080 |
  Select-Object LocalAddress, LocalPort, State, OwningProcess,
    @{n='Processus';e={(Get-Process -Id $_.OwningProcess).ProcessName}}
```

Cas d'usage : « l'application ne démarre plus : port déjà utilisé » (trouvez le
squatteur), « connexions TIME_WAIT par milliers » (épuisement de ports éphémères
sur un serveur très sollicité → ajuster `MaxUserPort`/`TcpTimedWaitDelay` avec
parcimonie et mesure).

---

## 81. DNS côté client : nslookup et Resolve-DnsName

```cmd
:: Requête simple (utilise le DNS configuré)
nslookup intranet.contoso.local

:: Interroger un serveur précis (pour comparer : mon DNS vs un autre)
nslookup intranet.contoso.local 10.10.10.11
nslookup intranet.contoso.local 10.10.10.12

:: Types d'enregistrements (MX, SRV pour AD, TXT...)
nslookup -type=SRV _ldap._tcp.contoso.local
```

```powershell
# Version PowerShell (plus propre, scriptable)
Resolve-DnsName intranet.contoso.local
Resolve-DnsName intranet.contoso.local -Server 10.10.10.11
Resolve-DnsName -Name _ldap._tcp.contoso.local -Type SRV
```

Diagnostic AD : si les SRV `_ldap._tcp.<domaine>` ne se résolvent pas, **rien**
dans le domaine ne fonctionnera (jonction, GPO, logon). C'est le test n°1 d'un
poste « le domaine ne répond plus ».

---

## 82. DNS : vider le cache, vérifier les suffixes

```cmd
ipconfig /flushdns
```

```powershell
# Suffixes DNS configurés (indispensables pour résoudre les noms courts)
Get-DnsClientGlobalSetting | Select-Object -ExpandProperty SuffixSearchList

# Serveurs DNS par interface
Get-DnsClientServerAddress -AddressFamily IPv4 |
  Select-Object InterfaceAlias, ServerAddresses

# Corriger : imposer les DNS du domaine sur une interface (ex. Ethernet0)
Set-DnsClientServerAddress -InterfaceAlias 'Ethernet0' -ServerAddresses '10.10.10.11','10.10.10.12'
```

**Classique** : un poste avec un DNS externe (box, 8.8.8.8) en premier ne résout
plus les noms internes et perd le domaine par intermittence. Sur un domaine AD,
les clients doivent **toujours** pointer vers les DC/DNS internes en premier.

---

## 83. DHCP côté client : renouvellement et diagnostic

```cmd
ipconfig /release
ipconfig /renew
ipconfig /all   :: vérifier : serveur DHCP, bail, durée
```

```powershell
# Journal des événements DHCP client
Get-WinEvent -LogName 'Microsoft-Windows-DHCP Client Events/Admin' -MaxEvents 20 |
  Select-Object TimeCreated, Id, Message | Format-Table -AutoSize -Wrap
```

Si `ipconfig /renew` échoue (« impossible de contacter le serveur DHCP ») :

1. Le client voit-il le réseau ? (ping passerelle, §77)
2. Le serveur DHCP est-il joignable ? (`Test-NetConnection srv-dhcp -Port 67`
   ne marche pas en TCP — utilisez le journal serveur §40 et un test depuis un
   poste sain du même VLAN)
3. **Relais DHCP (IP helper)** configuré sur le routeur du VLAN ? (cause n°1
   après un changement de VLAN)
4. Étendue saturée ? (ID 1020 côté serveur, §40)
5. Filtrage MAC / classeur DHCP qui exclut le poste ?

---

## 84. Wi-Fi : diagnostic de base

```cmd
:: Réseaux visibles et signal
netsh wlan show networks mode=bssid

:: État de la connexion actuelle : SSID, signal, canal, débit
netsh wlan show interfaces

:: Rapport HTML complet (utile pour le support)
netsh wlan show wlanreport
:: Génère C:\ProgramData\Microsoft\Windows\WlanReport\wlan-report-latest.html
```

```powershell
# Profils Wi-Fi mémorisés
netsh wlan show profiles
# Oublier un profil corrompu (clé changée côté AP par ex.)
netsh wlan delete profile name="WiFi-Atelier"
```

Réflexes : signal < -70 dBm = couverture insuffisante ; débit négocié très
inférieur au standard = interférences ou pilote ; **pilote Wi-Fi à jour** avant
toute chose (c'est la cause n°1 des déconnexions inexpliquées sur Windows 11).

---

## 85. Cas pratique 1 : pas d'accès réseau du tout

**Symptôme** : le poste n'atteint rien, pas même la passerelle. Pas d'Internet,
pas d'intranet.

**Diagnostic commenté :**

```cmd
:: 1. La carte a-t-elle une IP ?
ipconfig
:: -> Si "Média déconnecté" : câble/switch. Si APIPA : voir cas n°2.
```

```powershell
# 2. La carte est-elle UP côté Windows ?
Get-NetAdapter | Select-Object Name, Status, LinkSpeed
# Status = Disconnected -> physique. Disabled -> réactiver :
Enable-NetAdapter -Name 'Ethernet0' -Confirm:$false
```

```powershell
# 3. Passerelle joignable ?
Test-Connection -ComputerName 10.10.20.1 -Count 2
# KO -> tester le câble sur une autre prise, vérifier le voyant du switch,
#      essayer un autre port du switch (port HS ou VLAN mal affecté).
```

**Résolution typique** : câble débranché/cassé (60 %), port du switch en erreur
(`err-disabled` après une boucle), carte désactivée dans Windows, ou pilote
corrompu (réinstaller le pilote). **Toujours** tester avec un câble et un port
connus bons avant d'incriminer la carte.

---

## 86. Cas pratique 2 : adresse APIPA 169.254.x.x

**Symptôme** : `ipconfig` montre `169.254.x.x`, masque `255.255.0.0`, pas de
passerelle. Le poste s'est auto-attribué une adresse = **aucun serveur DHCP
n'a répondu**.

**Diagnostic commenté :**

```cmd
ipconfig /all
:: Vérifier : "Serveur DHCP . . . : 255.255.255.255" ou absent -> aucun contact
```

1. **Un seul poste ?** → local : `ipconfig /release` + `/renew` ; si échec,
   câble/switch (cas n°1) ou pile réseau corrompue :
   ```cmd
   netsh winsock reset
   netsh int ip reset
   :: puis redémarrage
   ```
2. **Tout un VLAN ?** → serveur/relais : le service DHCP tourne-t-il ?
   l'étendue est-elle saturée (§40) ? le relais IP helper est-il toujours
   configuré sur le routeur (vérifiez après chaque changement de VLAN) ?
3. **Après un changement** (nouveau switch, nouveau VLAN) → dans 90 % des cas,
   le relais DHCP n'a pas été configuré sur le nouvel équipement.

**Piège** : un serveur DHCP « sauvage » (box, point d'accès mal configuré)
distribue de mauvaises IP : `ipconfig /all` montre alors un serveur DHCP
inattendu → traquez-le via son adresse MAC (`arp -a`).

---

## 87. Cas pratique 3 : DNS ne résout plus

**Symptôme** : le ping d'IP fonctionne (`ping 10.10.30.5` OK) mais les noms
échouent (`ping intranet` → « hôte introuvable »).

**Diagnostic commenté :**

```powershell
# 1. Quels DNS le poste utilise-t-il ?
Get-DnsClientServerAddress -AddressFamily IPv4 | Select-Object InterfaceAlias, ServerAddresses
# -> Si DNS externe/box en premier sur un poste du domaine : corriger (§82).

# 2. Le DNS configuré répond-il ?
Resolve-DnsName intranet.contoso.local -Server 10.10.10.11
# KO -> tester le second DNS ; si les deux KO depuis ce poste mais OK depuis
# un autre : pare-feu local ou réseau (port 53 UDP/TCP filtré).

# 3. Simple problème de cache ?
ipconfig /flushdns
```

**Résolutions typiques** : remettre les DNS internes en premier (§82) ; corriger
le suffixe DNS ; côté serveur : zone DNS corrompue ou service DNS arrêté
(journal DNS Server §39). **Test décisif** : `nslookup nom 10.10.10.11` vs
`nslookup nom 10.10.10.12` — si un seul répond, le problème est ce serveur-là,
pas « le DNS » en général.

---

## 88. Cas pratique 4 : un seul site inaccessible

**Symptôme** : tout marche sauf `https://appli-metier.contoso.local` (ou un site
Internet précis).

**Diagnostic commenté :**

```powershell
# 1. Résolution OK ?
Resolve-DnsName appli-metier.contoso.local
# 2. Port ouvert ?
Test-NetConnection -ComputerName appli-metier.contoso.local -Port 443
# 3. Où ça casse ?
tracert -d appli-metier.contoso.local
```

- **Résolution KO** → DNS (cas n°3), entrée manquante.
- **Port fermé** → service arrêté sur le serveur, pare-feu (Windows ou réseau),
  mauvaise IP (DNS menteur / fichier `hosts` — vérifiez
  `C:\Windows\System32\drivers\etc\hosts` !).
- **Tracert bloqué à un saut précis** → équipement intermédiaire (souvent après
  un changement de routage ou une règle de pare-feu).
- **Site Internet unique KO** → proxy (§99), filtrage, ou panne du site
  (vérifiez depuis un smartphone en 4G/5G pour trancher).

**Le fichier `hosts`** est un classique sous-estimé : une ligne oubliée par un
ancien admin redirige le nom vers une IP morte. Toujours le contrôler.

---

## 89. Cas pratique 5 : lenteurs réseau intermittentes

**Symptôme** : « c'est lent » par moments, sans panne franche.

**Diagnostic commenté :**

```powershell
# 1. Objectiver : ping long avec horodatage vers la ressource
Test-Connection -ComputerName srv-fic-01 -Count 600 |
  Where-Object { $_.ResponseTime -gt 100 -or $_.StatusCode -ne 0 } |
  Select-Object @{n='Heure';e={Get-Date -Format 'HH:mm:ss'}}, ResponseTime, StatusCode
# -> Des pics réguliers (ex. toutes les heures) = un batch, une sauvegarde, un antivirus.
```

2. **Heure des pics** : corrélez avec sauvegardes, antivirus (scan planifié),
   WSUS/WUfB (téléchargements), sauvegardes des VM sur l'hôte.
3. **Wi-Fi ?** → signal, interférences, roaming (§84).
4. **Duplex** : `Get-NetAdapter | Select Name, LinkSpeed, FullDuplex` — un lien
   en half-duplex ou 100 Mb/s explique tout (§58).
5. **Saturation** : compteurs `\Network Interface(*)\Bytes Total/sec` (§58) aux
   heures de pics.

**Résolution** : déplacer les batchs gourmands hors heures ouvrées, activer le
QoS si pertinent, passer le lien en 1 Gb/s full-duplex forcé si la négociation
échoue, ou ajouter de la bande passante — **après** avoir prouvé la saturation
par les compteurs, pas avant.

---

## 90. Cas pratique 6 : partage SMB inaccessible

**Symptôme** : `\\srv-fic-01\partage` → « chemin introuvable » ou « accès refusé ».

**Diagnostic commenté :**

```powershell
# 1. Le serveur répond-il sur SMB (445) ?
Test-NetConnection -ComputerName srv-fic-01 -Port 445
# KO -> serveur éteint, pare-feu, ou SMB désactivé.

# 2. Quelles sont les erreurs SMB côté client ?
Get-WinEvent -LogName 'Microsoft-Windows-SMBClient/Connectivity' -MaxEvents 10 |
  Select-Object TimeCreated, Id, Message | Format-Table -AutoSize -Wrap

# 3. Accès refusé -> droits NTFS + droits de partage (les DEUX doivent autoriser)
# Sur le serveur : vérifier le partage et les ACL
Get-SmbShare -Name 'partage' | Select-Object Name, Path, Description
Get-SmbShareAccess -Name 'partage'
```

**Distinguer** :

- « Chemin d'accès introuvable » (0x80070035) = réseau/nom/partage inexistant.
- « Accès refusé » (0x80070005) = authentification ou droits (vérifiez avec quel
  compte : `whoami`, session verrouillée avec d'anciens identifiants ?).
- **Après une mise à jour** : SMBv1 désactivé par défaut depuis longtemps — un
  vieux NAS/copieur qui ne parle que SMBv1 ne se connecte plus (ne **pas**
  réactiver SMBv1 par facilité : mettez à jour l'équipement ou isolez-le).

---

## 91. Cas pratique 7 : imprimante réseau hors ligne

**Symptôme** : l'imprimante apparaît « hors ligne » alors qu'elle est allumée.

**Diagnostic commenté :**

```powershell
# 1. L'imprimante répond-elle ?
Test-Connection -ComputerName 10.10.40.25 -Count 2
# 2. Le port d'impression est-il joignable (9100 = RAW, 515 = LPR) ?
Test-NetConnection -ComputerName 10.10.40.25 -Port 9100
# 3. État côté Windows
Get-Printer | Where-Object Name -like '*Atelier*' | Select-Object Name, PrinterStatus, PortName
```

**Causes fréquentes** : IP de l'imprimante changée (DHCP sans réservation →
**toujours** des réservations DHCP ou IP fixes pour les imprimantes), SNMP mal
configuré (Windows la déclare hors ligne si la communauté SNMP ne répond pas :
désactivez « État SNMP » sur le port ou configurez-le), spouleur planté côté
serveur d'impression (`Restart-Service Spooler`).

**Bon réflexe** : imprimez la page de configuration réseau depuis l'imprimante
elle-même — elle donne sa vraie IP, souvent différente de celle que Windows croit.

---

## 92. Cas pratique 8 : VPN qui ne se connecte plus

**Symptôme** : le client VPN échoue (erreur 800, 809, 13801…).

**Diagnostic commenté :**

1. **Internet OK sans VPN ?** (sinon : cas n°1-3 d'abord).
2. **Code d'erreur exact** : 809 = ports IPsec bloqués (UDP 500/4500) — classique
   derrière une box/hôtel ; 800 = serveur injoignable ; 13801 = certificat.
3. **Depuis un autre réseau** (partage de connexion smartphone) : si ça marche →
   le réseau local filtre (pare-feu de la box, Wi-Fi public).
4. Côté client : journal `Applications et services > Microsoft > Windows >
   NetworkProfile` + événements RAS :

```powershell
Get-WinEvent -LogName Application -MaxEvents 50 |
  Where-Object { $_.ProviderName -like '*Ras*' } |
  Select-Object TimeCreated, Id, Message | Format-Table -AutoSize -Wrap
```

**Résolutions** : ouvrir/relayer UDP 500 et 4500 + protocole ESP (50) côté
pare-feu ; réparer le profil VPN (supprimer/recréer) ; vérifier le certificat
client (expiré ? §120) ; mettre à jour le client VPN.

---

## 93. Cas pratique 9 : port fermé / application injoignable

**Symptôme** : `Test-NetConnection -Port XXXX` → `TcpTestSucceeded : False`.

**Diagnostic commenté :**

```powershell
# 1. Le service écoute-t-il VRAIMENT sur ce serveur ?
Invoke-Command -ComputerName srv-appli-01 {
    Get-NetTCPConnection -State Listen | Where-Object LocalPort -eq 8443
}
# Rien -> le service n'est pas démarré ou n'écoute pas sur ce port/interface.

# 2. Le pare-feu Windows laisse-t-il passer ?
Get-NetFirewallRule -DisplayName '*appli*' | Select-Object DisplayName, Enabled, Direction, Action
Get-NetFirewallPortFilter -Protocol TCP | Where-Object LocalPort -eq 8443 |
  Get-NetFirewallRule | Select-Object DisplayName, Enabled, Action

# 3. Test depuis le serveur lui-même (loopback) :
#    OK en local + KO à distance = pare-feu (Windows ou réseau).
```

**Ordre de vérification** : service démarré → écoute sur la bonne interface
(`0.0.0.0` vs `127.0.0.1` — une appli liée au loopback n'est joignable que
localement) → pare-feu Windows → pare-feu réseau inter-VLAN. **90 % des « ports
fermés » sont un service arrêté ou un pare-feu Windows**, pas le réseau.

---

## 94. Cas pratique 10 : conflit d'adresse IP

**Symptôme** : message « Conflit d'adresse IP », connectivité intermittente
(ça marche, ça coupe).

**Diagnostic commenté :**

```cmd
:: Voir qui squatte l'IP : l'adresse MAC du conflit apparaît dans les événements
ipconfig /all
arp -a | findstr 10.10.20.45
```

```powershell
# Journal système : l'événement 4199 (Tcpip) signale le conflit avec la MAC adverse
Get-WinEvent -FilterHashtable @{ LogName='System'; Id=4199 } -MaxEvents 5 |
  Select-Object TimeCreated, Message | Format-Table -AutoSize -Wrap
```

**Résolution** : identifier la machine via la MAC (table du switch : `show mac
address-table`), puis : IP en dur oubliée sur un équipement (imprimante, caméra)
→ passer en DHCP + réservation ; étendue DHCP qui chevauche des IP fixes →
**exclure** la plage des IP fixes de l'étendue ; deux DHCP sauvages → supprimer
l'intrus (§86).

---

## 95. Cas pratique 11 : profil réseau Public au lieu de Domaine

**Symptôme** : le pare-feu se durcit tout seul, les partages et la découverte
réseau ne marchent plus, parfois les GPO ne s'appliquent pas.

**Diagnostic commenté :**

```powershell
# Quel profil le réseau a-t-il ?
Get-NetConnectionProfile | Select-Object Name, InterfaceAlias, NetworkCategory
# NetworkCategory = Public au lieu de DomainAuthenticated -> problème
```

**Causes** : le poste ne parvient pas à contacter un DC au moment de la
détection (DC injoignable, DNS faux §87, service NLA démarré avant le réseau).
**Résolution** : corriger la cause racine (DNS/DC), puis forcer la
re-détection :

```powershell
Restart-Service nlasvc -Force
# Vérifier 30 s après :
Get-NetConnectionProfile | Select-Object Name, NetworkCategory
```

> Ne « corrigez » pas en passant le profil en Privé à la main via le registre :
> vous masquez le symptôme (le poste ne voit toujours pas le domaine) et vous
> affaiblissez le pare-feu.

---

## 96. Cas pratique 12 : débit anormalement bas

**Symptôme** : copie de fichiers à 10 Mo/s sur un lien gigabit.

**Diagnostic commenté :**

```powershell
# 1. Négociation du lien
Get-NetAdapter | Select-Object Name, LinkSpeed, FullDuplex, MediaConnectionState
# LinkSpeed = 100 Mbps ou FullDuplex = False -> cause trouvée.

# 2. Erreurs d'interface (compteurs)
Get-Counter '\Network Interface(*)\Packets Received Errors',
            '\Network Interface(*)\Packets Outbound Errors' |
  Select-Object -ExpandProperty CounterSamples | Format-Table -AutoSize

# 3. Mesure réelle : copie d'un fichier témoin de 1 Go chronométrée
Measure-Command { Copy-Item \\srv-fic-01\partage\fichier-1go.bin C:\Temp\ }
```

**Causes** : câble abîmé (paire HS → 100 Mb/s), port switch forcé en 100/half,
pilote de carte réseau obsolète, **SMB signing** ou antivirus qui inspecte les
flux, Wi-Fi saturé. **Méthode** : mesurez **avant/après** chaque changement ;
un « ça semble mieux » n'est pas un diagnostic.

---

## 97. Cas pratique 13 : RDP impossible vers un serveur

**Symptôme** : le client RDP échoue (« l'ordinateur distant n'a pas répondu »,
erreur d'authentification, écran noir).

**Diagnostic commenté :**

```powershell
# 1. Port 3389 ouvert ?
Test-NetConnection -ComputerName srv-rds-01 -Port 3389
# 2. Le service est-il en écoute sur le serveur (via un autre accès : console, PSRemoting) ?
Invoke-Command -ComputerName srv-rds-01 { Get-Service TermService | Select-Object Status }
# 3. RDP autorisé dans le pare-feu ?
Invoke-Command -ComputerName srv-rds-01 {
    Get-NetFirewallRule -DisplayGroup 'Bureau à distance' | Select-Object DisplayName, Enabled
}
```

**Arbre de décision** : port fermé → service arrêté ou pare-feu ; port ouvert
mais échec d'authentification → compte verrouillé/expiré, heure désynchronisée
(Kerberos, §104), NLA ; connexion OK puis écran noir → session bloquée ou
problème graphique (tentez `/admin`) ; **après un patch** → vérifiez les
correctifs RDP récents (rollback §20 si régression avérée).

---

## 98. Cas pratique 14 : double pile IPv4/IPv6 capricieuse

**Symptôme** : résolution ou connexion aléatoire ; certaines applis préfèrent
IPv6 et échouent alors qu'IPv4 marche.

**Diagnostic commenté :**

```powershell
# Que résout le nom ? (souvent une adresse IPv6 link-local ou ULA en premier)
Resolve-DnsName srv-fic-01 | Select-Object Name, Type, IPAddress

# Forcer le test en IPv4 pour comparer
Test-NetConnection -ComputerName srv-fic-01 -Port 445   # utilise la résolution normale
ping -4 srv-fic-01
ping -6 srv-fic-01
```

**Résolutions** : si IPv6 n'est **pas** exploité dans votre infra (pas de DHCPv6,
pas de DNSv6), le plus propre est de **préférer IPv4** plutôt que de désactiver
IPv6 sauvagement (la désactivation totale casse des composants Windows : ne
décochez pas IPv6 sur la carte !). Ajustez les **stratégies de préfixe**
(`netsh interface ipv6 show prefixpolicies`) ou corrigez le DNS pour ne plus
publier d'enregistrements AAAA parasites.

---

## 99. Cas pratique 15 : proxy / PAC qui casse tout

**Symptôme** : Internet OK pour certains sites/applis, KO pour d'autres ;
erreurs de certificat ; applis qui « ne voient pas Internet » alors que le
navigateur oui.

**Diagnostic commenté :**

```powershell
# 1. Quel proxy est configuré ?
Get-ItemProperty 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings' |
  Select-Object ProxyEnable, ProxyServer, AutoConfigURL
# ProxyEnable=1 + ProxyServer vide/bizarre -> résidu d'un ancien proxy.

# 2. Le PAC est-il accessible et sain ?
Invoke-WebRequest -Uri 'http://proxy.contoso.local:8080/proxy.pac' -UseBasicParsing |
  Select-Object StatusCode

# 3. Test sans proxy (session de test)
netsh winhttp show proxy   # proxy au niveau machine (services !)
```

**Points clés** : le proxy **machine** (`netsh winhttp`) ≠ le proxy
**utilisateur** (navigateur) — les services et beaucoup d'applis utilisent le
premier ; un fichier PAC corrompu/injoignable = pannes intermittentes par site ;
après la suppression d'un proxy, pensez à nettoyer **les deux** niveaux + les
variables d'environnement `http_proxy` résiduelles.

---

## 100. AD/GPO côté client : gpupdate et gpresult

```cmd
:: Appliquer les stratégies (ordinateur + utilisateur)
gpupdate /force

:: Cibler : uniquement l'ordinateur (nécessite souvent un redémarrage)
gpupdate /target:computer /force
```

```powershell
# Rapport des stratégies appliquées (texte)
gpresult /r
# Rapport HTML complet (à ouvrir dans le navigateur) - idéal pour le support
gpresult /h C:\Temp\gpresult.html
# Stratégies pour un autre utilisateur/ordinateur (depuis un poste admin)
gpresult /s srv-poste-042 /user contoso\jdupont /h C:\Temp\gpo-042.html
```

**Lecture du rapport** : vérifiez « Dernière application de la stratégie de
groupe » (une date vieille de plusieurs jours = problème), la liste des GPO
appliquées **et refusées** (avec le motif : filtre de sécurité, WMI, GPO vide).
Une GPO « refusée » pour cause de filtrage de sécurité est le n°1 des « ma GPO ne
s'applique pas ».

---

## 101. RSOP et journaux du service de stratégie de groupe

- `rsop.msc` : jeu de stratégies résultant (visuel, équivalent de `gpresult`).
- Journal dédié : `Applications et services > Microsoft > Windows >
  GroupPolicy > Opérationnel` — chaque application de GPO y est tracée avec les
  erreurs par extension (scripts, registre, mappages…).

```powershell
# Erreurs GroupPolicy sur 7 jours
Get-WinEvent -LogName 'Microsoft-Windows-GroupPolicy/Operational' -MaxEvents 200 |
  Where-Object LevelDisplayName -eq 'Error' |
  Select-Object TimeCreated, Id, Message | Format-Table -AutoSize -Wrap
```

**IDs utiles** : 1030/1058 (accès SYSVOL/NETLOGON impossible → réseau/DNS),
1129 (échec de traitement au démarrage), 7016/7017 (extensions en timeout).
Si les erreurs 1030/1058 sont massives sur le parc : suspectez **DFSR/SYSVOL**
côté DC (§39), pas les postes.

---

## 102. nltest : diagnostiquer la relation au domaine

`nltest` interroge le canal sécurisé et les DC :

```cmd
:: Quel DC sert ce poste ?
nltest /dsgetdc:contoso.local

:: État du canal sécurisé avec le domaine
nltest /sc_query:contoso.local
:: Doit retourner : "Trusted DC ... The command completed successfully"
:: Drapeaux attendus : 0x... (dont 0x200 = secure channel OK selon version)

:: Lister les DC du domaine
nltest /dclist:contoso.local

:: Forcer la découverte d'un DC (après correction DNS/réseau)
nltest /dsgetdc:contoso.local /force
```

Si `/dsgetdc` échoue : le poste ne trouve pas de DC → DNS (les SRV
`_ldap._tcp.contoso.local`, §81), réseau (ports 389/636/88/445 vers le DC, §79),
ou heure (§104). **Toujours** dans cet ordre.

---

## 103. Test-ComputerSecureChannel et réinitialisation

Le **canal sécurisé** (mot de passe du compte machine) peut se désynchroniser
(restauration d'une VM, double compte machine, clone mal syspreppé). Symptôme :
« La relation d'approbation entre cette station et le domaine a échoué ».

```powershell
# Tester le canal sécurisé
Test-ComputerSecureChannel
# False -> tenter la réparation (nécessite un compte autorisé)
Test-ComputerSecureChannel -Repair -Credential (Get-Credential)
# True -> canal réparé sans quitter/réintégrer le domaine
```

Si la réparation échoue : **quitter puis réintégrer** le domaine (méthode
classique, redémarrage requis entre les deux) :

```powershell
# Quitter (compte admin local requis après le reboot)
Remove-Computer -UnjoinDomainCredential (Get-Credential) -Restart -Force
# Puis réintégrer
Add-Computer -DomainName 'contoso.local' -Credential (Get-Credential) -Restart -Force
```

> Avant de réintégrer : supprimez le compte machine obsolète dans l'AD
> (utilisateurs et ordinateurs AD) pour éviter les doublons, et vérifiez qu'il
> n'existe pas de clone avec le même SID (sysprep !).

---

## 104. Temps : w32tm, la cause n°1 des échecs Kerberos

Kerberos tolère **5 minutes** d'écart (par défaut). Au-delà : échecs d'ouverture
de session, « relation d'approbation », GPO qui ne s'appliquent plus.

```cmd
:: État de la synchro : source, écart, dernière synchro réussie
w32tm /query /status
:: -> "Source : DC-contrôleur.contoso.local" attendu sur un poste du domaine
::    "Stratum", "Last Successful Sync Time" : vérifier la fraîcheur

:: Pairs configurés
w32tm /query /peers

:: Forcer une resynchronisation
w32tm /resync /rediscover
```

**Hiérarchie** : les postes se synchronisent sur le DC qui les authentifie, les
DC sur le PDC de la forêt, le PDC sur une source externe fiable (NTP du
fournisseur, `pool.ntp.org`, horloge interne). **Ne pointez jamais les postes
directement sur Internet** : tout passe par la hiérarchie AD.

```cmd
:: Sur le PDC : déclarer une source externe fiable
w32tm /config /manualpeerlist:"0.fr.pool.ntp.org 1.fr.pool.ntp.org" /syncfromflags:manual /reliable:yes /update
net stop w32time && net start w32time
```

**Cas VM** : désactivez la synchronisation d'horloge de l'hyperviseur **pour les
DC** (sinon l'hôte écrase w32tm) ; laissez-la pour les postes simples. Un DC
virtualisé qui dérive = tout le domaine qui dérive.

---

## 105. Jonction au domaine : procédure et dépannage

Checklist **avant** de joindre :

- [ ] DNS du poste = DC/DNS interne (§82), suffixe DNS correct.
- [ ] Heure à moins de 5 min du DC (§104).
- [ ] Le compte machine n'existe pas déjà (ou est réinitialisé).
- [ ] Compte autorisé à joindre (droit « Ajouter des stations de travail », 10
      par défaut pour les utilisateurs standard).
- [ ] Ports vers le DC ouverts (DNS 53, Kerberos 88, LDAP 389, SMB 445).

```powershell
# Jonction scriptée (déploiement)
Add-Computer -DomainName 'contoso.local' -OUPath 'OU=Postes,OU=Paris,DC=contoso,DC=local' `
  -Credential (Get-Credential) -Restart -Force
```

Échecs fréquents : « le domaine spécifié n'existe pas » = DNS ; « accès refusé »
= droits ou compte machine existant protégé ; erreur réseau = pare-feu/ports.
**Après** la jonction : vérifiez `nltest /dsgetdc`, `gpresult /r` (les GPO
s'appliquent au prochain redémarrage/ouverture de session), et déplacez le compte
dans la bonne OU (**jamais** laisser les postes dans `Computers` en production).

---

## 106. Monitoring : ce qu'on supervise sur Windows

Le socle minimal, par machine :

| Famille | Quoi | Source |
|---|---|---|
| **Disponibilité** | Ping, agent joignable | Superviseur |
| **Système** | CPU, mémoire, espace disque C: et volumes | Compteurs / agent |
| **Services** | Services critiques démarrés (DNS, DHCP, AD DS, Spooler…) | Agent |
| **Journaux** | Erreurs Système/Sécurité (patterns §37-38) | WEF / agent / SIEM |
| **Mises à jour** | Conformité patch, redémarrage en attente | Script / Intune |
| **Matériel** | SMART, RAID, température (via constructeur) | Agent + outils OEM |
| **Sauvegarde** | Succès du dernier job | Rapport du logiciel |
| **Certificats** | Expiration < 60 j | Script §120 |
| **AD (DC)** | Réplication, SYSVOL, temps, rôles FSMO | Scripts + `repadmin` |

**Règle** : chaque alerte doit avoir un **propriétaire** et une **procédure**
(lien vers la section du guide). Une alerte sans procédure = du bruit qu'on
finit par ignorer.

Redémarrage en attente (indicateur simple et précieux) :

```powershell
# Le serveur attend-il un redémarrage ? (patch, installation...)
$pending = @(
    'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing\RebootPending',
    'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate\Auto Update\RebootRequired',
    'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\PendingFileRenameOperations'
)
$pending | ForEach-Object {
    [pscustomobject]@{ Cle = $_; EnAttente = (Test-Path $_) }
} | Format-Table -AutoSize
```

---

## 107. Seuils d'alerte recommandés

| Métrique | Warning | Critique | Remarque |
|---|---|---|---|
| Espace libre volume système | < 20 % | < 10 % | §31 |
| CPU (15 min) | > 80 % | > 95 % | + file d'attente §55 |
| Mémoire libre | < 15 % | < 5 % | §56 |
| Latence disque lecture | > 20 ms | > 50 ms | HDD ; diviser par ~4 sur SSD §57 |
| Ping | perte > 5 % | hôte injoignable | — |
| Service critique arrêté | — | immédiat | Selon rôle |
| Erreurs journal Système (nouveau pattern) | — | immédiat | Via WEF/SIEM §36 |
| Échecs logon 4625 (pic) | ×5 vs baseline | ×20 vs baseline | §38 |
| Réplication AD en erreur | — | immédiat | §39 |
| Certificat expire dans | < 60 j | < 14 j | §120 |
| Sauvegarde non réussie | > 24 h | > 48 h | §123 |
| Redémarrage en attente | > 7 j | > 30 j | §106 |

**Évitez l'alert fatigue** : un seuil qui déclenche chaque semaine sans action =
un mauvais seuil. Revisitez les seuils trimestriellement avec l'équipe.

---

## 108. Zabbix : agent2 et templates Windows

**Zabbix agent 2** (recommandé, avec plugins) se déploie par GPO/script. Points
d'attention : autoriser le serveur Zabbix dans le pare-feu (port 10050), TLS
PSK si les flux traversent des zones non maîtrisées.

Templates à appliquer :

- **Windows by Zabbix agent** : CPU, mémoire, disques, interfaces, services,
  `system.localtime` (dérive d'horloge !), uptime.
- Complétez avec des **items personnalisés** (`UserParameter` ou clés
  `system.run[]` — à restreindre via `AllowKey`/`DenyKey` pour la sécurité) :

```ini
# zabbix_agent2.conf - exemples d'items maison
# Espace libre C: en % (déjà dans le template, exemple pédagogique)
# Redémarrage en attente (0/1) via script PowerShell
AllowKey=system.run[*]
```

```powershell
# C:\Scripts\zabbix\pending-reboot.ps1 -> retourne 1 si reboot en attente
$paths = @(
  'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing\RebootPending',
  'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate\Auto Update\RebootRequired'
)
if ($paths | Where-Object { Test-Path $_ }) { 1 } else { 0 }
```

**Services** : utilisez la découverte `service.discovery` + prototypes pour
surveiller automatiquement les services critiques par rôle (liste par host group :
`Windows-DC`, `Windows-FileServer`…).

---

## 109. Prometheus : windows_exporter

**windows_exporter** expose les métriques Windows au format Prometheus
(port **9182** par défaut). Collecteurs utiles à activer :

```cmd
:: Installation en service (exemple)
windows_exporter.exe --collectors.enabled="cpu,cs,logical_disk,mem,net,os,service,system,textfile"
```

| Collecteur | Métriques |
|---|---|
| `cpu`, `cs`, `os`, `system` | Charge, mémoire, uptime |
| `logical_disk` | Espace libre/utilisé par volume |
| `net` | Débit/erreurs interfaces |
| `service` | État des services Windows |
| `textfile` | **Vos métriques maison** : déposez des `.prom` générés par script |

Exemple `textfile` : le script §106 (reboot en attente) écrit
`C:\Program Files\windows_exporter\textfile_inputs\maintenance.prom` :

```text
# HELP windows_pending_reboot Redémarrage en attente (1=oui)
# TYPE windows_pending_reboot gauge
windows_pending_reboot 0
```

```powershell
# Génération du fichier .prom par tâche planifiée
$pending = if (Test-Path 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing\RebootPending') { 1 } else { 0 }
@'
# HELP windows_pending_reboot Redémarrage en attente (1=oui)
# TYPE windows_pending_reboot gauge
windows_pending_reboot {0}
'@ -f $pending | Out-File 'C:\Program Files\windows_exporter\textfile_inputs\maintenance.prom' -Encoding ascii
```

Alertes Prometheus type : `windows_logical_disk_free_bytes / windows_logical_disk_size_bytes < 0.15`,
`windows_service_state{state="stopped"} == 1` sur services critiques.

---

## 110. Centralisation des journaux (syslog / Graylog / Loki)

Options pour centraliser sans WEF (§36) ou en complément :

| Solution | Protocole | Remarque |
|---|---|---|
| **WEF natif** | WinRM/HTTPS | Sans agent, parfait en domaine |
| **NXLog / Winlogbeat** | Syslog / Beats | Vers Graylog, ELK, Loki |
| **Agent Zabbix** | Zabbix | `eventlog[]` pour les journaux |
| **Promtail + Loki** | HTTP | Couplé à windows_exporter/Grafana |

Exemple Winlogbeat minimal (vers Logstash/Elasticsearch) :

```yaml
winlogbeat.event_logs:
  - name: System
    level: critical, error, warning
  - name: Security
    event_id: 4625, 4740, 4720, 4728, 4732, 4756, 1102
  - name: Microsoft-Windows-Sysmon/Operational
output.logstash:
  hosts: ["logstash.contoso.local:5044"]
```

**Sysmon** (Sysinternals) mérite une mention : il enrichit énormément le journal
`Microsoft-Windows-Sysmon/Operational` (créations de processus avec ligne de
commande, connexions réseau par processus) — précieux en sécurité, à déployer
avec une configuration de référence (ex. SwiftOnSecurity) adaptée.

---

## 111. PowerShell : fondamentaux d'administration

```powershell
# Aide et découverte (vos deux meilleurs amis)
Get-Help Get-Service -Full
Get-Command -Module NetAdapter
Get-Command *disk*

# Pipeline : filtrer, trier, sélectionner
Get-Service | Where-Object Status -eq 'Running' | Sort-Object DisplayName |
  Select-Object Name, DisplayName, StartType | Format-Table -AutoSize

# CIM plutôt que WMI (moderne, compatible PSRemoting)
Get-CimInstance Win32_OperatingSystem | Select-Object Caption, Version, LastBootUpTime

# Exporter proprement
Get-Service | Export-Csv C:\Temp\services.csv -NoTypeInformation -Encoding UTF8
Get-Service | ConvertTo-Json | Out-File C:\Temp\services.json
```

**Politique d'exécution** (pour vos scripts signés ou internes) :

```powershell
Set-ExecutionPolicy RemoteSigned -Scope LocalMachine -Force
# RemoteSigned : scripts locaux OK, scripts téléchargés = signature requise
```

> Écrivez vos scripts en **fonctions avancées** (`[CmdletBinding()]`,
> paramètres typés, `-WhatIf` supporté) dès qu'ils dépassent 20 lignes ou qu'ils
> modifient le système. Un script « one-shot » devient toujours un script de
> production un jour.

---

## 112. PSRemoting et WinRM : mise en place sécurisée

PSRemoting = exécuter du PowerShell **à distance** via WinRM. Activation :

```powershell
# Sur chaque machine cible (ou par GPO : voir ci-dessous)
Enable-PSRemoting -Force
# Vérifier
Test-WSMan srv-fic-01
```

Par GPO (recommandé en domaine) :

```text
Configuration ordinateur > Stratégies > Modèles d'administration >
Composants Windows > WinRM > Service WinRM
  - Autoriser la gestion de serveur à distance via WinRM : Activé (IPv4: *)
+ Pare-feu Windows : autoriser « Gestion à distance Windows » (TCP 5985)
+ Service WinRM en démarrage automatique
```

**Sécurité :**

- En domaine avec Kerberos, le trafic WinRM est **chiffré** par défaut :
  n'activez HTTPS/5986 que si vous traversez des zones non maîtrisées.
- **N'utilisez pas TrustedHosts avec `*`** en production : c'est l'équivalent de
  désactiver l'authentification mutuelle.
- Restreignez qui peut se connecter : groupe « Utilisateurs de la gestion à
  distance » (Remote Management Users), jamais les comptes utilisateurs standard.
- Désactivez l'authentification de base (`AllowBasic = false`) côté serveur.

```powershell
# Durcir : vérifier la config WinRM
winrm get winrm/config/service
# Doit montrer : AllowBasic=false (sauf besoin explicite), AllowUnencryptedTraffic=false
```

---

## 113. Invoke-Command : exécution à distance

```powershell
# Une commande sur N machines (Kerberos implicite en domaine)
Invoke-Command -ComputerName srv-fic-01, srv-prn-01 {
    Get-Service Spooler | Select-Object MachineName, Status
}   # Note : $env:COMPUTERNAME dans le bloc distant = la cible

# Avec identifiants explicites (hors domaine ou compte dédié)
$cred = Get-Credential -UserName 'contoso\admin-expl' -Message 'Compte exploitation'
Invoke-Command -ComputerName srv-dmz-01 -Credential $cred -ScriptBlock {
    Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 3
}

# Exécuter un script local SUR les machines distantes
Invoke-Command -ComputerName (Get-Content C:\Scripts\serveurs.txt) `
  -FilePath C:\Scripts\Collecte-Sante.ps1

# Récupérer des fichiers distants (rapports générés à distance)
Invoke-Command -ComputerName srv-fic-01 {
    Get-ChildItem C:\Temp\rapport-*.csv | Select-Object -ExpandProperty FullName
} | ForEach-Object {
    Copy-Item -Path $_ -Destination C:\Temp\Central\ -FromSession $s
}
```

> Les objets retournés sont **désérialisés** : les méthodes ne fonctionnent plus
> (ex. `Stop()` sur un service distant). Faites tout le traitement **dans** le
> bloc distant, ne retournez que des données.

---

## 114. Sessions persistantes et fichiers distants

```powershell
# Session réutilisable (plus rapide que N connexions)
$s = New-PSSession -ComputerName srv-fic-01
Invoke-Command -Session $s { Get-PSDrive C }
Invoke-Command -Session $s { Get-Service | Where-Object Status -eq 'Stopped' }
Remove-PSSession $s

# Copier des fichiers vers/depuis la machine distante (PS 5.1+)
Copy-Item -Path C:\Scripts\Deploy.ps1 -Destination C:\Temp\ -ToSession $s
Copy-Item -Path C:\Temp\resultat.csv -Destination C:\Temp\Central\ -FromSession $s

# Entrer en interactif (dépannage)
Enter-PSSession -ComputerName srv-fic-01
# ... commandes exécutées à distance ...
Exit-PSSession
```

**Limites** : double-hop (rebond vers une 3ᵉ machine) bloqué par Kerberos par
défaut → utilisez CredSSP (à vos risques, à restreindre) ou mieux : exécutez
depuis la cible avec un gMSA (§46). Et rappelez-vous : **WinRM ≠ RDP** — pour une
console interactive complète, RDP reste nécessaire.

---

## 115. Inventaire matériel en PowerShell

```powershell
# Fiche matérielle complète d'une machine (locale ou via Invoke-Command)
$cs  = Get-CimInstance Win32_ComputerSystem
$os  = Get-CimInstance Win32_OperatingSystem
$bios = Get-CimInstance Win32_BIOS
$cpu = Get-CimInstance Win32_Processor | Select-Object -First 1
[pscustomobject]@{
    Machine      = $env:COMPUTERNAME
    Fabricant    = $cs.Manufacturer
    Modele       = $cs.Model
    Serie        = $bios.SerialNumber
    CPU          = $cpu.Name
    Coeurs       = $cpu.NumberOfCores
    RAM_Go       = [math]::Round($cs.TotalPhysicalMemory / 1GB, 1)
    OS           = $os.Caption
    Build        = "$($os.Version) ($($os.BuildNumber))"
    DernierBoot  = $os.LastBootUpTime
    Disques      = (Get-PhysicalDisk |
                    ForEach-Object { "$($_.FriendlyName) $([math]::Round($_.Size/1GB)) Go $($_.MediaType)" }) -join ' | '
} | Format-List
```

Industrialisation : bouclez sur la liste des serveurs avec `Invoke-Command`
(§113), `Export-Csv -Append` vers un fichier central, planifiez mensuellement
(§121). C'est votre CMDB « v1 » en attendant mieux.

---

## 116. Inventaire logiciels installés

Deux sources complémentaires (aucune n'est exhaustive seule) :

```powershell
# 1. Registre Uninstall (rapide, couvre 95 % des logiciels classiques)
$paths = @(
  'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*',
  'HKLM:\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*'
)
Get-ItemProperty $paths -ErrorAction SilentlyContinue |
  Where-Object DisplayName |
  Select-Object DisplayName, DisplayVersion, Publisher, InstallDate |
  Sort-Object DisplayName | Format-Table -AutoSize

# 2. Win32_Product : À ÉVITER (déclenche une reconfiguration MSI à chaque appel !)
# Préférez le registre ci-dessus, ou Get-Package :
Get-Package | Select-Object Name, Version, ProviderName | Sort-Object Name |
  Format-Table -AutoSize
```

> ⚠️ `Get-CimInstance Win32_Product` / `Get-WmiObject Win32_Product` provoque une
> **vérification de chaque MSI** (lent, peut casser des applis). Ne l'utilisez
> jamais en inventaire. C'est l'erreur classique n°1 des scripts trouvés sur le web.

---

## 117. Inventaire des correctifs installés

```powershell
# Correctifs (KB) installés, du plus récent au plus ancien
Get-HotFix | Sort-Object InstalledOn -Descending |
  Select-Object HotFixID, Description, InstalledOn, InstalledBy |
  Format-Table -AutoSize

# Conformité : telle machine a-t-elle le cumulatif du mois ? (ex. KB5034765)
$kb = 'KB5034765'
if (Get-HotFix -Id $kb -ErrorAction SilentlyContinue) {
    "OK : $kb installé"
} else {
    "MANQUANT : $kb"
}

# À l'échelle du parc (via PSRemoting)
Invoke-Command -ComputerName (Get-Content C:\Scripts\serveurs.txt) {
    [pscustomobject]@{
        Machine = $env:COMPUTERNAME
        DernierKB = (Get-HotFix | Sort-Object InstalledOn -Descending |
                     Select-Object -First 1).HotFixID
        DateKB  = (Get-HotFix | Sort-Object InstalledOn -Descending |
                     Select-Object -First 1).InstalledOn
    }
} | Format-Table -AutoSize
```

Croisez avec la date du Patch Tuesday (§10) : un serveur dont le dernier KB date
de 3 mois = non-conforme → à intégrer au rattrapage (§8).

---

## 118. Rapport espace disque multi-serveurs

Le rapport quotidien du §5, version complète :

```powershell
# Rapport-EspaceDisque.ps1 — à planifier chaque matin (§121)
$servers = Get-Content C:\Scripts\serveurs.txt
$seuilPct = 20
$result = Invoke-Command -ComputerName $servers {
    Get-CimInstance Win32_LogicalDisk -Filter "DriveType=3" | ForEach-Object {
        [pscustomobject]@{
            Machine   = $env:COMPUTERNAME
            Volume    = $_.DeviceID
            TotalGo   = [math]::Round($_.Size / 1GB, 1)
            LibreGo   = [math]::Round($_.FreeSpace / 1GB, 1)
            LibrePct  = [math]::Round($_.FreeSpace / $_.Size * 100, 1)
        }
    }
}
$alertes = $result | Where-Object LibrePct -lt $seuilPct
$date = Get-Date -Format 'yyyy-MM-dd'
$result | Export-Csv "C:\Rapports\disques-$date.csv" -NoTypeInformation -Encoding UTF8
if ($alertes) {
    $corps = $alertes | Format-Table -AutoSize | Out-String
    Send-MailMessage -To 'exploitation@contoso.local' -From 'rapports@contoso.local' `
      -Subject "[ALERTE] Espace disque < $seuilPct % ($date)" -Body $corps `
      -SmtpServer 'smtp.contoso.local' -Encoding UTF8
}
```

> Adaptez `Send-MailMessage` à votre relais (authentification, TLS). Sur les
> environnements récents, préférez un connecteur ou un script d'envoi via votre
> solution de supervision plutôt que SMTP anonyme.

---

## 119. Rapport comptes AD expirés / inactifs

```powershell
# Prérequis : module ActiveDirectory (RSAT) sur la machine d'exécution
Import-Module ActiveDirectory

# Comptes inactifs depuis plus de 90 jours (hors comptes de service connus)
$limite = (Get-Date).AddDays(-90)
Search-ADAccount -UsersOnly -AccountInactive -TimeSpan 90 |
  Where-Object { $_.Enabled -and $_.SamAccountName -notlike 'svc-*' } |
  Select-Object Name, SamAccountName, LastLogonDate, DistinguishedName |
  Export-Csv C:\Rapports\comptes-inactifs.csv -NoTypeInformation -Encoding UTF8

# Comptes expirés ou expirant dans moins de 30 jours
Get-ADUser -Filter 'Enabled -eq $true' -Properties AccountExpirationDate, PasswordLastSet |
  Where-Object { $_.AccountExpirationDate -and $_.AccountExpirationDate -lt (Get-Date).AddDays(30) } |
  Select-Object Name, SamAccountName, AccountExpirationDate |
  Sort-Object AccountExpirationDate | Format-Table -AutoSize

# Comptes verrouillés actuellement
Search-ADAccount -LockedOut | Select-Object Name, SamAccountName, LastLogonDate |
  Format-Table -AutoSize
```

**Processus** (mensuel, §8) : le rapport part au chef de service → validation
métier → **désactivation** (pas suppression immédiate : 30 jours de quarantaine
en OU dédiée avant suppression, au cas où).

---

## 120. Rapport certificats qui expirent

```powershell
# Certificats machine expirant dans moins de 60 jours (magasin LocalMachine\My)
$seuil = (Get-Date).AddDays(60)
Get-ChildItem Cert:\LocalMachine\My |
  Where-Object { $_.NotAfter -lt $seuil } |
  Select-Object Subject,
    @{n='ExpireLe'; e={ $_.NotAfter }},
    @{n='JoursRestants'; e={ [math]::Round(($_.NotAfter - (Get-Date)).TotalDays) }},
    Thumbprint |
  Sort-Object ExpireLe | Format-Table -AutoSize

# Multi-serveurs : à exécuter via Invoke-Command sur les serveurs à certificats
# (RDS, IIS, ADFS, VPN...) listés dans C:\Scripts\serveurs-certificats.txt
```

**Politique** : alerte à 60 j (warning), 14 j (critique) — §107. Documentez pour
chaque certificat : usage, autorité, procédure de renouvellement, responsable.
Un certificat expiré un dimanche à 3h du matin sur le VPN d'astreinte, c'est du
vécu — la liste à jour évite ça.

---

## 121. Exécution planifiée des scripts de rapport

Assemblez les rapports §115-120 en une tâche quotidienne :

```powershell
# Tâche "Rapports quotidiens" : 07:30, en gMSA, avec limite de durée
$action = New-ScheduledTaskAction -Execute 'powershell.exe' `
  -Argument '-NoProfile -ExecutionPolicy Bypass -File "C:\Scripts\Rapports-Quotidiens.ps1"'
$trigger = New-ScheduledTaskTrigger -Daily -At '07:30'
$principal = New-ScheduledTaskPrincipal -UserId 'contoso\gmsa-taches-maint$' -LogonType Password
$settings = New-ScheduledTaskSettingsSet -ExecutionTimeLimit '01:00:00' -StartWhenAvailable
Register-ScheduledTask -TaskName 'Rapports\Quotidiens' -Action $action `
  -Trigger $trigger -Principal $principal -Settings $settings `
  -Description 'Inventaire + espace disque + correctifs + certificats (guide §115-120)'
```

`Rapports-Quotidiens.ps1` appelle chaque script, consolide les CSV dans
`C:\Rapports\AAAA-MM-JJ\`, envoie un e-mail de synthèse avec les alertes en
objet. **Testez chaque script à la main** avant de le planifier, et surveillez
`LastTaskResult` (§47) : un rapport qui ne tourne plus est pire que pas de
rapport (faux sentiment de sécurité).

---

## 122. PRA/PCA : RTO, RPO et scénarios

| Notion | Définition | Exemple |
|---|---|---|
| **RTO** (Recovery Time Objective) | Durée max d'interruption acceptable | 4 h pour l'ERP |
| **RPO** (Recovery Point Objective) | Perte de données max acceptable | 1 h pour la comptabilité |
| **PRA** (Plan de Reprise d'Activité) | Reprise après sinistre majeur | Incendie salle serveur |
| **PCA** (Plan de Continuité d'Activité) | Maintien pendant la crise | Mode dégradé |

**Scénarios à couvrir** (a minima) :

1. Perte d'un serveur non critique (restauration simple).
2. Perte d'un contrôleur de domaine (réinstallation + réplication, ou
   restauration d'état système §126).
3. Perte de l'hyperviseur / du cluster (bascule, reconstruction).
4. Ransomware (restauration depuis sauvegardes **hors ligne**, §123).
5. Perte de la salle (site de secours, §123).

**Règle du chef de service** : RTO/RPO se **négocient avec la direction et les
métiers**, pas dans le bureau de l'informaticien. Faites signer le tableau
RTO/RPO par application : c'est votre contrat en cas de crise, et ça dimensionne
le budget sauvegarde.

---

## 123. Stratégie de sauvegarde Windows

| Donnée | Méthode | Fréquence type |
|---|---|---|
| Volumes système serveurs | Image complète (bare metal) | Hebdo + avant changement |
| Données (partages, bases) | Sauvegarde fichiers / applicative | Quotidienne |
| AD (contrôleurs) | État du système | Quotidienne (un DC suffit, en rotation) |
| Postes | Dossiers redirigés / OneDrive / image pour postes critiques | Continue/hebdo |

**Règle 3-2-1** : 3 copies, 2 supports différents, 1 hors site (ou hors ligne).
Contre les ransomwares : au moins une copie **immuable ou déconnectée**
(disque externe rotatif, bande, stockage objet avec verrouillage).

**À tester** : une sauvegarde non testée = pas de sauvegarde (§127).
**À documenter** : quoi, où, fréquence, rétention, chiffrement, responsable,
procédure de restauration pas à pas (§128).

---

## 124. Windows Server Backup en pratique

Windows Server Backup (fonctionnalité à ajouter) suffit pour les petits parcs :

```powershell
# Installer la fonctionnalité (+ outils en ligne de commande)
Install-WindowsFeature Windows-Server-Backup -IncludeManagementTools

# Sauvegarde complète (tous les volumes critiques) vers un disque dédié
wbadmin start backup -backupTarget:E: -include:C:,D: -allCritical -quiet

# Sauvegarde de l'état du système seul (utile pour un DC, voir §126)
wbadmin start systemstatebackup -backupTarget:E: -quiet

# Planifier : sauvegarde quotidienne à 22:00
wbadmin enable backup -addtarget:E: -schedule:22:00 -include:C:,D: -allCritical -quiet
```

Vérifications d'exploitation :

```cmd
:: Derniers résultats de sauvegarde
wbadmin get versions
:: Détail d'une version
wbadmin get items -version:09/26/2026-22:00
```

**Limites** : pas de déduplication avancée, pas de gestion centralisée
multi-serveurs, restauration granulaire limitée → au-delà d'une dizaine de
serveurs, passez à une vraie solution (Veeam, etc., §138).

---

## 125. VSS : clichés instantanés de volumes

**VSS** (Volume Shadow Copy Service) fige un volume pour sauvegarder des fichiers
ouverts (bases, boîtes partagées) de façon cohérente.

```cmd
:: Lister les clichés existants
vssadmin list shadows
:: Espace utilisé par les clichés
vssadmin list shadowstorage
:: Redimensionner l'espace réservé (ex. 10 % du volume D:)
vssadmin resize shadowstorage /for=D: /on=D: /maxsize=10%
```

**Clichés instantanés pour les partages** (versions précédentes) : activez-les sur
les volumes de données (2 clichés/jour ouvrés typiquement) — les utilisateurs
restaurent eux-mêmes leurs fichiers écrasés (clic droit > Versions précédentes),
ce qui divise les tickets « j'ai écrasé mon fichier ».

Écrivains VSS en erreur = sauvegardes incohérentes :

```cmd
vssadmin list writers
:: Tout écrivain durablement en "Failed" / "Timed out" -> redémarrer le service
:: associé, voire le serveur en fenêtre de maintenance
```

---

## 126. Sauvegarde des contrôleurs de domaine (état du système)

Un DC se sauvegarde via l'**état du système** (AD, SYSVOL, registre, COM+) :

```cmd
wbadmin start systemstatebackup -backupTarget:E: -quiet
```

**Restauration** (deux cas) :

1. **Il reste des DC sains** : ne restaurez pas — **réinstallez** un Windows,
   promovez-le en DC, laissez la réplication repeupler. C'est plus propre qu'une
   restauration (pas de risque d'USN rollback).
2. **Tous les DC sont perdus** : restauration d'état système en mode
   **restauration des services d'annuaire (DSRM)** sur un DC, avec le mot de
   passe DSRM (à conserver sous enveloppe scellée, §128).

> ⚠️ **USN rollback** : ne restaurez jamais un snapshot/clone d'un DC sans
> procédure (restauration non autoritaire) — vous corrompriez la réplication AD
> de tout le domaine. En cas de doute : réinstallation + promotion.

**Bonnes pratiques** : sauvegardez l'état système d'**au moins un DC par jour**
(en rotation), conservez le mot de passe DSRM hors du DC lui-même, et documentez
la procédure de reconstruction complète du domaine (elle servira le jour où…).

---

## 127. Tests de restauration : procédure

**Trimestriel** (partiel) et **annuel** (complet, §9). Procédure type :

1. Choisir le scénario (ex. : restauration du serveur de fichiers sur un hôte
   isolé).
2. Restaurer **sans toucher à la production** (réseau isolé ou VLAN de test).
3. Vérifier : démarrage, services, données (échantillon de fichiers avec
   contrôle d'intégrité), applications (ouverture d'un dossier, impression test).
4. Mesurer : temps réel vs RTO, perte de données vs RPO.
5. **Compte rendu écrit** : ce qui a marché, ce qui a coincé, actions
   correctives, mise à jour du PRA et des procédures.

```powershell
# Exemple de contrôle d'intégrité après restauration d'un partage :
# comparer un échantillon de fichiers (nom + taille + hash)
$ref = Import-Csv C:\PRA\referentiel-fichiers.csv   # généré avant sinistre
foreach ($f in $ref | Select-Object -First 50) {
    $ok = (Test-Path $f.Chemin) -and
          ((Get-FileHash $f.Chemin -Algorithm SHA256).Hash -eq $f.Hash)
    [pscustomobject]@{ Fichier = $f.Chemin; Integre = $ok }
} | Format-Table -AutoSize
```

> Un test qui échoue n'est pas un échec : c'est le test qui a servi. Ce qui est
> un échec, c'est de découvrir le problème le jour du vrai sinistre.

---

## 128. Documentation d'exploitation : le DTI

Le **DTI** (Dossier Technique d'Intervention / d'exploitation) est la bible de
l'équipe. Contenu minimal par système :

| Rubrique | Contenu |
|---|---|
| Fiche d'identité | Rôle, criticité, RTO/RPO, responsable, contrats |
| Architecture | Schéma, IP, VLAN, dépendances (AD, DNS, stockage, onduleur) |
| Installation | Procédure de reconstruction pas à pas (testée !) |
| Exploitation | Checklists, fenêtres, procédures courantes (ce guide en est la base) |
| Supervision | Seuils, alertes, procédures associées |
| Sauvegarde | Quoi/où/fréquence/rétention + procédure de restauration |
| Accès | Comptes (renvoi au coffre-fort), accès physiques, contacts |
| Historique | Incidents majeurs, changements notables |

**Règles** : un DTI par système critique, versionné, relu annuellement, accessible
**hors du système lui-même** (le DTI du serveur de fichiers ne doit pas être
uniquement sur le serveur de fichiers). En crise, on n'a pas le temps de chercher.

---

## 129. Procédures : standard de rédaction

Modèle de procédure (1 page recto idéalement, 3 max) :

```text
TITRE : Redémarrage planifié du serveur SRV-FIC-01
OBJECTIF : appliquer les correctifs mensuels sans perte de données
PRÉREQUIS : fenêtre validée, sauvegarde OK, information utilisateurs
ÉTAPES :
  1. Vérifier qu'aucun job n'est en cours : ...
  2. Notifier : msg * /SERVER:srv-fic-01 "..."
  3. Installer les MAJ : ...
  4. Redémarrer, vérifier les services : Get-Service ...
  5. Contrôle post : partages accessibles, journal Système sans erreur
RETOUR ARRIÈRE : si échec > 30 min -> rollback §20 / restauration §124
TRACÉ : ticket n°, heure début/fin, opérateur
```

**Principes** : impératif, commandes copiables, critères de succès explicites,
procédure de retour arrière **systématique**, nom du rédacteur + date de dernière
relecture. Une procédure non relue depuis 2 ans est une procédure fausse.

---

## 130. Gestion des changements

Tout changement hors routine suit un mini-processus (proportionné à votre taille) :

| Étape | Contenu |
|---|---|
| **Demande** | Quoi, pourquoi, impact, risque |
| **Validation** | Chef de service (ou comité selon criticité) |
| **Planification** | Fenêtre (§4), communication, retour arrière |
| **Exécution** | Par la personne désignée, avec la procédure (§129) |
| **Vérification** | Tests post-changement, supervision |
| **Clôture** | Ticket complété, doc mise à jour, retour d'expérience si incident |

**Changements standard** (pré-approuvés) : patch mensuel selon anneaux (§11),
redémarrage planifié, ajout d'utilisateur — procédure connue, pas de validation
au cas par cas. **Changements normaux** : migration, nouveau rôle, modification
GPO sensible — validation requise. **Urgents** : incident de sécurité, panne P1 —
validation a posteriori sous 24 h, mais traçabilité immédiate.

---

## 131. Passation et continuité (angle chef de service)

Le service ne doit jamais dépendre d'une seule personne (« bus factor ») :

- [ ] **Coffre-fort des accès** : mots de passe admin, DSRM, certificats, clés —
      dans un coffre chiffré d'équipe, jamais dans la tête d'un seul.
- [ ] **Binômes** : chaque système critique a un référent **et** un suppléant
      formé (qui a déjà fait la manip, pas juste lu la doc).
- [ ] **Passation** : à chaque départ/arrivée, revue des DTI (§128), des tâches
      planifiées (§47) et des accès (révoquer le jour J).
- [ ] **Astreinte** : planning écrit, téléphone d'astreinte, procédure d'escalade
      (qui appeler à 3h du matin pour quoi), compensation.
- [ ] **Journal de bord** : un canal unique (ticket/OneNote/wiki) où l'équipe note
      interventions et observations — relu en revue hebdo (§7).

> Test annuel : « si je suis injoignable pendant 2 semaines, l'équipe tourne-t-elle ? »
> Si la réponse est non, la priorité n°1 n'est pas technique, elle est
> organisationnelle.

---

## 132. 20 erreurs classiques d'exploitation Windows

1. **Ne jamais redémarrer les serveurs** (« uptime 400 jours ») → patchs non
   finalisés, fuites mémoire, pannes surprises. Redémarrage mensuel minimum (§8).
2. **`Win32_Product` en inventaire** → reconfiguration MSI en cascade (§116).
3. **WSUS « tous produits, toutes langues »** → base obèse, console inutilisable (§13).
4. **Désactiver IPv6 en décochant la carte** → composants Windows cassés (§98).
5. **Réactiver SMBv1** pour un vieux NAS → faille de sécurité ; mettre à jour
   l'équipement (§90).
6. **Purger WinSxS à la main** → système instable ; utiliser DISM (§28-29).
7. **`chkdsk /r` lancé à 14h sur un volume de prod** → indisponibilité de plusieurs
   heures (§25).
8. **Mot de passe en dur dans un script** → utilisez gMSA (§46) ou un coffre.
9. **Tâche planifiée sous un compte utilisateur** → échec à la prochaine
   expiration de mot de passe (§45).
10. **Sauvegarde jamais testée** → découverte le jour du sinistre (§127).
11. **Snapshot de DC restauré sans procédure** → USN rollback, AD corrompu (§126).
12. **`/ResetBase` avant validation du patch** → rollback impossible (§29).
13. **DNS externe en premier sur un poste du domaine** → domaine intermittent (§82).
14. **Tous les DC patchés la même nuit** → aucun annuaire pendant l'opération (§11).
15. **Journal Sécurité trop petit** → écrasé en 3 jours, aucune enquête possible (§41).
16. **Driver Verifier oublié activé** → BSOD en production (§74).
17. **Pas de point de restauration avant un changement pilote** (§67).
18. **Ignorer les alertes** (« oui on sait, le disque est plein depuis 2 mois ») →
    l'alerte fatigue tue la supervision (§107).
19. **Documenter nulle part** → le savoir part avec la personne (§128, §131).
20. **Patcher sans fenêtre ni communication** → utilisateurs bloqués, confiance
    perdue (§4, §19).

---

## 133. Pense-bête des commandes PowerShell

```powershell
# --- Système & santé ---
sfc /scannow
DISM /Online /Cleanup-Image /RestoreHealth
Get-ComputerInfo | Select-Object WindowsProductName, OsBuildNumber
Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 5
systeminfo | Select-String 'Heure de démarrage'

# --- Disques ---
Get-PSDrive C
Get-Volume | Format-Table DriveLetter, FileSystemType, SizeRemaining, Size
Optimize-Volume -DriveLetter C -ReTrim -Verbose
Get-PhysicalDisk | Get-StorageReliabilityCounter | Format-Table DeviceId, Wear, TemperatureCelsius

# --- Journaux ---
Get-WinEvent -LogName System -MaxEvents 20
Get-WinEvent -FilterHashtable @{LogName='System'; Level=2; StartTime=(Get-Date).AddDays(-1)}
wevtutil qe System /c:10 /rd:true /f:text

# --- Réseau ---
Test-Connection -ComputerName 10.10.20.1 -Count 4
Test-NetConnection -ComputerName srv-fic-01 -Port 445
Resolve-DnsName intranet.contoso.local
Get-NetAdapter | Select-Object Name, Status, LinkSpeed
Get-NetTCPConnection -State Listen | Sort-Object LocalPort

# --- AD / GPO ---
gpupdate /force
gpresult /r ; gpresult /h C:\Temp\gpo.html
nltest /dsgetdc:contoso.local ; nltest /sc_query:contoso.local
Test-ComputerSecureChannel -Repair
w32tm /query /status ; w32tm /resync
repadmin /replsummary ; dcdiag /q

# --- Services & processus ---
Get-Service | Where-Object Status -eq 'Running'
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10
resmon   # analyseur de ressources

# --- Tâches planifiées ---
Get-ScheduledTask | Get-ScheduledTaskInfo | Where-Object LastTaskResult -ne 0

# --- À distance ---
Enable-PSRemoting -Force
Invoke-Command -ComputerName srv-01, srv-02 { Get-Service Spooler }
Enter-PSSession -ComputerName srv-01

# --- Certificats ---
Get-ChildItem Cert:\LocalMachine\My | Select-Object Subject, NotAfter
```

---

## 134. Pense-bête des commandes CMD / classiques

```cmd
:: --- Démarrage / récupération ---
shutdown /r /o /f /t 0        :: redémarrer sur options avancées (WinRE)
reagentc /info                :: état de WinRE
bcdedit /enum                 :: magasin de démarrage
bootrec /fixmbr & bootrec /fixboot & bootrec /rebuildbcd
chkdsk C: /f

:: --- Réseau ---
ipconfig /all & ipconfig /flushdns & ipconfig /renew
ping -t 10.10.20.1 & tracert -d 10.10.30.5 & pathping 10.10.30.5
netstat -ano & arp -a & route print -4
nslookup intranet.contoso.local 10.10.10.11
netsh wlan show interfaces & netsh wlan show wlanreport
netsh winsock reset & netsh int ip reset   :: (puis reboot)

:: --- Système ---
sfc /scannow
DISM /Online /Cleanup-Image /RestoreHealth
cleanmgr /sageset:10 & cleanmgr /sagerun:10
vssadmin list shadows & vssadmin list writers
wbadmin get versions
wecutil qc   :: initier le collecteur d'événements
verifier      :: Driver Verifier (assistant)
mdsched       :: diagnostic mémoire (reboot)
```

---

## 135. Quiz : 10 questions

1. Votre serveur affiche « uptime 412 jours ». Pourquoi est-ce un problème
   d'exploitation, et que faites-vous ?
2. Un correctif du Patch Tuesday fait planter l'application métier sur l'anneau
   pilote. Décrivez la conduite à tenir pour l'anneau production.
3. `sfc /scannow` rapporte des fichiers irréparables. Quelle est la séquence
   correcte avant de le relancer ?
4. Le journal Sécurité d'un DC s'écrase tous les 2 jours. Quel est le risque et
   les deux corrections possibles ?
5. Un poste affiche `169.254.12.34`. Que signifie cette adresse et quelles sont
   les 3 vérifications dans l'ordre ?
6. `Test-NetConnection srv -Port 443` échoue mais le ping passe. Listez les 3
   causes les plus probables dans l'ordre de vérification.
7. Un utilisateur signale « la relation d'approbation a échoué » à l'ouverture de
   session. Donnez les deux méthodes de réparation, de la moins intrusive à la
   plus intrusive.
8. Pourquoi `Get-CimInstance Win32_Product` est-il à proscrire en inventaire ?
   Que faut-il utiliser à la place ?
9. Définissez RTO et RPO, et expliquez qui les valide dans l'entreprise.
10. Citez 3 raisons pour lesquelles une tâche planifiée peut échouer avec une
    erreur d'ouverture de session, et la solution recommandée.

---

## 136. Réponses du quiz

1. **Uptime 412 jours** = correctifs jamais finalisés (redémarrages en attente),
   risque de fuite mémoire et de panne au prochain reboot « surprise ». Action :
   planifier un redémarrage en fenêtre de maintenance (§8), vérifier la conformité
   patch après reboot.
2. **Bloquer le KB** pour la production (refus WSUS / pause WUfB), documenter
   (KB, symptômes, contournement), informer l'éditeur, re-tester au patch suivant
   (§11, §20). Ne jamais « forcer » un KB qui casse le métier.
3. **DISM d'abord** : `DISM /Online /Cleanup-Image /RestoreHealth` répare le
   magasin de composants dans lequel SFC puise, **puis** relancer `sfc /scannow`
   (§21-24).
4. **Risque** : aucune capacité d'enquête a posteriori (incident, audit).
   Corrections : augmenter la taille max (`wevtutil sl Security /ms:…`, §41)
   et/ou transférer vers un collecteur/SIEM (WEF §36, §110).
5. **APIPA** = aucun serveur DHCP n'a répondu. Vérifications : (1) connectivité
   locale (câble, `ping` passerelle), (2) `ipconfig /renew` + journal DHCP
   client, (3) côté infra : service DHCP, étendue saturée, relais IP helper du
   VLAN (§86, §83, §40).
6. **Port 443 fermé, ping OK** : (1) le service n'écoute pas / est arrêté,
   (2) pare-feu Windows, (3) pare-feu réseau inter-VLAN (§93). Vérifier l'écoute
   avec `Get-NetTCPConnection -State Listen` sur le serveur.
7. (1) `Test-ComputerSecureChannel -Repair` (répare le canal sans quitter le
   domaine), (2) quitter puis réintégrer le domaine (`Remove-Computer` /
   `Add-Computer`) si la réparation échoue (§103).
8. `Win32_Product` **déclenche une reconfiguration de chaque MSI** (lent, peut
   casser des applications). Utiliser le registre `Uninstall` ou `Get-Package`
   (§116).
9. **RTO** = durée max d'interruption acceptable ; **RPO** = perte de données max
   acceptable. Ils sont **négociés et signés avec la direction et les métiers**,
   pas décidés par l'IT seule (§122).
10. (1) mot de passe du compte expiré/modifié, (2) compte sans le droit « Ouvrir
    une session en tant que tâche », (3) compte désactivé. **Solution** : migrer
    vers un **gMSA** (mot de passe géré par AD) ou SYSTEM selon le besoin (§45-46).

---

## 137. Glossaire

| Terme | Définition |
|---|---|
| **AD DS** | Active Directory Domain Services — annuaire d'entreprise |
| **APIPA** | Adressage automatique 169.254.x.x quand aucun DHCP ne répond |
| **BCD** | Boot Configuration Data — magasin de démarrage (remplace boot.ini) |
| **BSOD** | Blue Screen Of Death — écran bleu d'erreur fatale du noyau |
| **DISM** | Outil de maintenance des images et du magasin de composants |
| **DFSR** | DFS Replication — réplication SYSVOL entre DC |
| **DSRM** | Directory Services Restore Mode — mode de restauration AD |
| **EDR** | Endpoint Detection and Response — antivirus nouvelle génération |
| **FSRM** | File Server Resource Manager — quotas/rapports avancés |
| **FSMO** | Rôles maîtres d'opérations AD (PDC, RID, Infrastructure, Schéma, Nom de domaine) |
| **gMSA** | Group Managed Service Account — compte de service à mot de passe géré par AD |
| **GPO** | Group Policy Object — stratégie de groupe |
| **NLA** | Network Location Awareness — détection du profil réseau |
| **OOB** | Out-Of-Band — correctif publié hors cycle mensuel |
| **PAL** | Performance Analysis of Logs — analyse des journaux de compteurs |
| **PCA** | Plan de Continuité d'Activité |
| **PRA** | Plan de Reprise d'Activité |
| **PSRemoting** | Exécution PowerShell à distance via WinRM |
| **ReFS** | Resilient File System — système de fichiers résilient |
| **RPO** | Recovery Point Objective — perte de données max acceptable |
| **RSOP** | Resultant Set Of Policy — résultante des stratégies appliquées |
| **RTO** | Recovery Time Objective — durée max d'interruption acceptable |
| **SFC** | System File Checker — vérificateur des fichiers système |
| **SMART** | Self-Monitoring, Analysis and Reporting Technology — santé des disques |
| **SSU** | Servicing Stack Update — MAJ de la pile de maintenance Windows Update |
| **SYSVOL** | Partage répliqué contenant GPO et scripts d'ouverture de session |
| **VSS** | Volume Shadow Copy Service — clichés instantanés |
| **WEF** | Windows Event Forwarding — transfert d'événements vers un collecteur |
| **WinRE** | Windows Recovery Environment — environnement de récupération |
| **WSUS** | Windows Server Update Services — serveur local de mises à jour (déprécié) |
| **WUfB** | Windows Update for Business — pilotage des MAJ sans serveur local |

---

## 138. Pour aller plus loin

**Documentation officielle (à consulter en priorité) :**

- Microsoft Learn : *Windows Server* (déploiement, rôles, dépannage) —
  `learn.microsoft.com/windows-server`
- Référence des ID d'événements Windows et des compteurs de performances —
  Microsoft Learn
- Documentation **windows_exporter** (collecteurs, textfile) —
  dépôt GitHub `prometheus-community/windows_exporter`
- Documentation **Zabbix** : template *Windows by Zabbix agent*

**Outils :**

- **Sysinternals Suite** (Process Explorer, Autoruns, Sysmon, Procmon) —
  indispensables en dépannage avancé
- **WinDbg** (Microsoft Store) + symboles Microsoft — analyse de dumps (§71)
- **PAL** (Performance Analysis of Logs) — analyse des journaux perfmon (§60)
- **Wireshark** — capture réseau quand `tracert`/`Test-NetConnection` ne suffisent plus

**Sujets connexes à ce guide :**

- Supervision : *guide Zabbix* et *guide Prometheus/Grafana* (métriques, alerting)
- Sauvegarde : solutions d'entreprise (Veeam…), PRA/PCA — §122-127
- Sécurité : durcissement AD, LAPS, EDR — guides dédiés
- Automatisation : PowerShell avancé, DSC/Intune — §111-121
- Énergie : onduleurs, dimensionnement, arrêt automatique (NUT) — en lien avec
  l'exploitation des salles serveurs

**Pratique recommandée** : montez un **labo** (Hyper-V ou Proxmox) avec 1 DC +
2 serveurs membres + 1 client Windows 11, et rejouez chaque section « cas
pratique » de ce guide. C'est en cassant puis réparant qu'on devient
opérationnel — jamais en lisant seulement.
