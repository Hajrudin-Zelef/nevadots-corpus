---
id: collect-261121-veille-ia/windows-os/veeam-vs-acronis-vs-commvault-2026-comparatif-backup
title: "Veeam vs Acronis vs Commvault 2026 : Comparatif Backup"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-08-05"]
keywords: ["veeam", "acronis", "backup", "commvault", "aout", "sauvegarde", "comparatif", "cloud"]
source: https://tech-insider.org/fr/veeam-vs-acronis-vs-commvault-2026/
source_anchor: ""
source_lines: [1, 182]
section: "Windows_OS"
---
Le 5 août 2026, le CERT-FR a publié l’avis CERTFR-2026-AVI-0968 sur de multiples vulnérabilités touchant Veeam Service Provider Console et Veeam ONE. Trois semaines plus tard, une entrée distincte de la base européenne EUVD (EUVD-2026-11595) a confirmé une faille d’élévation de privilèges notée 8,8 sur 10 dans Veeam Backup & Replication. Pour les DSI et RSSI français, la question n’est plus seulement de savoir quel logiciel de sauvegarde restaure le plus vite après un rançongiciel. Elle est aussi de savoir quel éditeur corrige le plus vite ses propres failles, et lequel protège vraiment les données pendant l’attaque. Ce comparatif met face à face Veeam Backup & Replication, Acronis Cyber Protect et Commvault Cloud Platform sur les critères qui comptent en 2026 : architecture, immuabilité, prix par charge de travail, positionnement Gartner et conformité NIS2.

## Pourquoi ce comparatif Veeam, Acronis et Commvault s’impose en août 2026

Trois signaux convergent au même moment. D’abord la sécurité des outils de sauvegarde eux-mêmes est devenue un sujet à part entière. L’avis CERT-FR du 5 août 2026 liste des impacts sérieux sur les produits Veeam concernés : atteinte à la confidentialité, contournement de la politique de sécurité, déni de service à distance, exécution de code arbitraire et injection SQL. Un serveur de sauvegarde compromis donne accès non seulement aux données de production, mais aussi aux copies de restauration, ce qui en fait une cible stratégique pour les groupes de rançongiciel. Le site a déjà documenté ce risque dans son article sur le rançongiciel INC, qui a visé spécifiquement des infrastructures Veeam chez plus de 830 victimes recensées.

Ensuite, le calendrier réglementaire européen pousse les entreprises à revoir leur stratégie de résilience. La directive NIS2 impose des obligations de continuité d’activité aux opérateurs essentiels et importants, et le Cyber Resilience Act ajoute des exigences de transparence sur les vulnérabilités logicielles. Un éditeur de sauvegarde qui publie un correctif en quelques jours n’a pas le même profil de risque qu’un éditeur silencieux pendant des semaines. Enfin, les trois plateformes ont toutes changé de visage depuis un an : Veeam est passé en version 13.1, Acronis a réorienté sa plateforme Cyber Protect Cloud vers les prestataires de services managés, et Commvault a rebaptisé son offre Commvault Cloud Platform, propulsée par Metallic AI. Comparer ces trois éditeurs aujourd’hui donne une photographie beaucoup plus fidèle qu’un comparatif écrit il y a dix-huit mois.

## Veeam Backup & Replication : présentation, architecture et version 13.1

Veeam Backup & Replication reste le nom le plus cité dès qu’on évoque la sauvegarde d’infrastructures virtualisées en entreprise. La dernière build généralement disponible fin août 2026 est la version 13.1.1.18, publiée le 13 août 2026, avec un flux de maintenance parallèle en version 13.0.3.63 sorti le 25 août 2026. La branche précédente, la 12.3.2.4854, avait été livrée les 8 et 9 juin 2026 et reste largement déployée dans les parcs qui n’ont pas encore migré vers la v13.

L’architecture Veeam couvre un périmètre technique très large. Les hyperviseurs pris en charge incluent VMware vSphere de la version 6.x à 8.0, Microsoft Hyper-V, Nutanix AHV, Proxmox VE, Scale Computing HyperCore, HPE Morpheus VM Essentials, oVirt, XCP-ng, Citrix XenServer et Sangfor aSV. Côté cloud public, Veeam sauvegarde et restaure des instances AWS EC2, RDS, DynamoDB, Redshift, EFS et FSx, des machines virtuelles et bases Azure (SQL, Cosmos DB), ainsi que des ressources Google Cloud (VM, SQL, Spanner). La plateforme couvre aussi Microsoft Entra ID, Kubernetes, les partages NAS et, selon des revues indépendantes, Microsoft 365 et Salesforce. C’est aujourd’hui la matrice de compatibilité la plus large des trois éditeurs comparés ici.

## Acronis Cyber Protect Cloud : présentation et positionnement MSP

Acronis a construit sa stratégie autour d’une idée simple : fusionner sauvegarde et cybersécurité dans une seule console, plutôt que de superposer deux outils distincts. Cyber Protect Cloud embarque un moteur anti-malware et une détection comportementale de type EDR (Endpoint Detection and Response) directement sur l’agent de sauvegarde. La dernière branche documentée en juin 2026 correspond à la version 26.06, avec des agents Windows, macOS et Linux en build 26.6.42752. Le rythme de publication d’Acronis suit un schéma mensuel AA.MM, contrairement au cycle de versions majeures de Veeam ou Commvault.

Le positionnement d’Acronis a changé de manière significative depuis 2024. L’éditeur ne figure plus dans le Magic Quadrant Gartner dédié à la sauvegarde d’entreprise, Gartner justifiant cette sortie par une orientation de plus en plus marquée vers les prestataires de services managés (MSP), les postes de travail et les charges de travail en périphérie de réseau plutôt que vers la sauvegarde d’entreprise au sens large. Cela ne rend pas Acronis moins pertinent, cela signale simplement un public cible différent : PME, indépendants et surtout MSP qui gèrent des dizaines de clients depuis une console multi-tenant unique, avec une tarification par poste très lisible.

## Commvault Cloud Platform : architecture et Metallic AI

Commvault a achevé sa transition de marque en 2025-2026 : l’ancienne offre Commvault Complete Backup & Recovery et la solution SaaS Metallic sont désormais réunies sous le nom Commvault Cloud Platform, propulsée par Metallic AI. Les documents accessibles publiquement ne donnent pas de numéro de version unique pour cette plateforme unifiée, contrairement à Veeam ou Acronis, ce qui reflète une approche produit davantage centrée sur des mises à jour continues en mode cloud que sur des jalons de version classiques.

Sur le plan technique, Commvault protège VMware, Hyper-V, Azure Stack, Nutanix AHV et les charges conteneurisées, avec une couverture documentée de plus de 30 services AWS. Sa signature reste la fonctionnalité Air Gap Protect, qui isole les sauvegardes dans un domaine de sécurité air-gappé et immuable chiffré en AES-256. Commvault met aussi en avant des mécanismes d’orchestration de reprise comme Cloud Rewind et la récupération orchestrée d’Active Directory, deux fonctions pensées pour les grands comptes qui doivent redémarrer une chaîne d’applications entière après un incident, pas seulement une machine virtuelle isolée.

## Tableau comparatif : Veeam vs Acronis vs Commvault en 2026

| Critère | Veeam Backup & Replication | Acronis Cyber Protect Cloud | Commvault Cloud Platform | 
|---|---|---|---|
| Dernière version connue (août 2026) | 13.1.1.18 (13/08/2026) | 26.06, agent 26.6.42752 | Non communiquée (mise à jour continue) | 
| Modèle de licence | Veeam Universal License (par charge de travail) | Par poste / par charge de travail | Sur devis, par capacité ou abonnement | 
| Hyperviseurs supportés | VMware, Hyper-V, Nutanix AHV, Proxmox VE, XCP-ng, XenServer, oVirt, Sangfor aSV | Agents Windows/macOS/Linux, VM via agent | VMware, Hyper-V, Azure Stack, Nutanix AHV | 
| Cloud public | AWS, Azure, Google Cloud (matrice large) | Couverture cloud via agents, matrice partielle | 30+ services AWS, Azure | 
| Immuabilité | Object Lock S3/Azure Blob/Wasabi, dépôts Linux durcis | Non détaillée publiquement | Air Gap Protect, AES-256 | 
| Air gap | Veeam Data Cloud Vault, copies hors ligne | Non documenté explicitement | Domaine air-gappé natif (Air Gap Protect) | 
| Anti-malware intégré | Secure Restore (scan avant restauration) | EDR intégré à l’agent de sauvegarde | Détection via Metallic AI | 
| Restauration vérifiée en sandbox | SureBackup | Non mis en avant | Cloud Rewind (orchestration) | 
| Authentification renforcée | RBAC, SSO SAML, validation à quatre yeux | Console multi-tenant, MFA | RBAC entreprise | 
| Cible principale | ETI, grands comptes, environnements hybrides | PME, indépendants, MSP | Grands comptes, environnements hybrides complexes | 
| Statut Magic Quadrant Gartner 2026 | Leader (en tête de l’exécution) | Hors périmètre depuis 2024 | Leader | 
| Note Gartner Peer Insights | 4,8/5 (124 avis, au 31/03/2026) | Avis positifs, moyenne agrégée non publiée | Non publiée dans les sources consultées | 

## Modèles de déploiement : on-premise, cloud ou hybride

Le choix d’un éditeur de sauvegarde ne se limite pas aux fonctionnalités : le modèle de déploiement pèse tout autant sur le coût total et sur la surface d’attaque exposée. Veeam Backup & Replication reste conçu en priorité pour un déploiement on-premise, avec un serveur de sauvegarde installé sur l’infrastructure du client, ce qui donne un contrôle total sur les données mais reporte aussi sur l’équipe IT la responsabilité du durcissement de ce serveur, exactement le composant visé par l’avis CERT-FR du 5 août 2026. Veeam propose toutefois des briques cloud complémentaires, comme Veeam Data Cloud Vault pour le stockage immuable hors site, et un modèle SaaS via des partenaires hébergeurs, illustré par l’offre référencée sur le catalogue G-Cloud britannique.

Acronis Cyber Protect Cloud a été pensé dès l’origine comme une plateforme cloud multi-tenant, hébergée par Acronis ou par un partenaire MSP, avec des agents légers installés sur chaque poste ou serveur protégé. Ce choix réduit la charge d’administration pour une PME qui ne veut pas gérer d’infrastructure de sauvegarde dédiée, mais implique aussi de confier la garde des données à un tiers, un point à vérifier au regard des exigences de souveraineté des données propres à certains secteurs réglementés en France. Commvault Cloud Platform, de son côté, propose un déploiement hybride natif : les workloads peuvent rester on-premise, être répliqués vers le cloud public, ou vivre nativement dans un environnement cloud, avec une seule console de gestion pour orchestrer l’ensemble. Cette flexibilité explique en partie pourquoi Commvault reste privilégié par les grands comptes qui opèrent des environnements hérités aux côtés de charges de travail cloud-natives récentes.

Pour une entreprise française soumise à NIS2 ou évoluant dans un secteur sensible, la question de la localisation des données mérite d’être posée explicitement à chaque éditeur avant signature. Un contrat de sauvegarde cloud qui héberge les données hors de l’Union européenne peut compliquer une démonstration de conformité, même si la solution technique elle-même est irréprochable.

## Le bulletin CERT-FR du 5 août 2026 : ce qu’il faut vraiment savoir

L’avis CERTFR-2026-AVI-0968 vise deux composants précis de l’écosystème Veeam : Veeam Service Provider Console dans les versions antérieures à 9.3.0.35057, et Veeam ONE dans les versions antérieures à 13.1.0.7034. Le CERT-FR recommande d’appliquer sans délai les correctifs listés dans le bulletin de sécurité de l’éditeur. Pour les hébergeurs et MSP français qui exploitent Service Provider Console afin de superviser plusieurs clients depuis une seule interface, une console compromise ouvre potentiellement l’accès à l’ensemble des locataires gérés, ce qui explique le niveau de vigilance demandé par l’agence.

En parallèle, la base européenne des vulnérabilités EUVD référence sous l’identifiant EUVD-2026-11595 une élévation de privilèges locale dans Veeam Backup & Replication, avec un score CVSS 3.1 de 8,8. Les versions concernées sont la branche 13 antérieure à 13.0.1 et la branche 12 antérieure à 12.3.2, ainsi que des plages similaires pour Veeam Backup and Recovery. Le score EPSS, qui estime la probabilité d’exploitation dans les 30 jours, se situe autour de 0,22 %, et la vulnérabilité n’apparaît ni dans le catalogue KEV de la CISA ni dans son équivalent européen au moment de la dernière mise à jour de la fiche. Aucune activité d’exploitation n’a par ailleurs été détectée par les honeypots Shadowserver. Le détail technique de cette faille et de son correctif figure dans la note KB4743 publiée par Veeam, qui documente les vulnérabilités corrigées dans la version 12.3.2.

Un point de prudence s’impose sur un troisième signalement. Des rapports de veille sur les menaces publiés fin août 2026 évoquent une éventuelle faille d’exécution de code à distance, référencée de manière informelle CVE-2026-44963, qui reposerait sur une désérialisation non sécurisée dans Veeam Backup & Replication 12.x. À la date du 29 août 2026, cette information n’a été confirmée ni par une analyse technique publique, ni par une reconnaissance officielle de l’éditeur. Elle doit donc être traitée comme une rumeur à surveiller, pas comme un fait acquis. Pour approfondir la méthode de défense en profondeur recommandée face à ce type de scénario, l’article du site sur la règle de sauvegarde 3-2-1-1-0 anti-ransomware détaille comment limiter l’impact d’une compromission du serveur de sauvegarde lui-même.

## Immuabilité, air gap et détection IA : la vraie bataille anti-ransomware

Sur le papier, les trois éditeurs promettent une protection contre le rançongiciel. Dans le détail, les mécanismes ne se valent pas. Veeam s’appuie sur l’immuabilité au niveau du stockage objet : verrouillage Object Lock sur AWS S3, Azure Blob ou Wasabi, et dépôts Linux durcis avec attributs étendus non modifiables. Veeam Data Cloud Vault ajoute une couche de stockage immuable et chiffré préconfigurée, pensée pour les équipes qui ne veulent pas gérer elles-mêmes l’infrastructure de stockage cible. Avant toute restauration, la fonction Secure Restore analyse le point de restauration avec un moteur anti-malware, et SureBackup permet de tester automatiquement, dans un environnement isolé, qu’une machine restaurée démarre réellement et fonctionne.

Commvault mise sur une approche différente avec Air Gap Protect : les sauvegardes sont isolées dans un domaine de sécurité logiquement déconnecté du réseau de production, chiffré en AES-256, avec une orchestration de reprise pensée pour redémarrer des chaînes applicatives complètes plutôt que des machines isolées via Cloud Rewind. Acronis, de son côté, ne documente pas publiquement de mécanisme d’immuabilité ou d’air gap comparable à ceux de Veeam et Commvault dans ses ressources accessibles. Sa force réside ailleurs : la fusion native entre sauvegarde et protection contre les menaces, avec un moteur de détection comportemental capable de bloquer des attaques zero-day directement sur le poste protégé, avant même qu’un fichier chiffré n’atteigne la sauvegarde. Pour un DSI, le choix se résume souvent à une question de philosophie : bloquer la menace en amont (Acronis) ou garantir que la copie de secours reste intacte quoi qu’il arrive (Veeam, Commvault).

## Positionnement Gartner Magic Quadrant 2024-2026 : qui mène le marché

Le classement Gartner reste la référence la plus citée par les acheteurs en entreprise, et son évolution récente raconte une histoire cohérente. En 2024, dans le Magic Quadrant « Enterprise Backup and Recovery Software Solutions », Veeam a été positionné Leader pour la huitième fois consécutive et classé numéro un en capacité d’exécution pour la cinquième année de suite. Commvault a lui aussi été reconnu Leader, une treizième année consécutive selon l’éditeur. Le groupe des Leaders comprenait alors Veeam, Commvault, Rubrik, Cohesity, Veritas et Dell. Acronis, en revanche, est sorti du quadrant cette année-là, Gartner justifiant cette exclusion par un recentrage de l’éditeur vers les MSP et les charges de travail en périphérie plutôt que vers la sauvegarde d’entreprise classique.

En 2025, Gartner a renommé l’étude « Backup and Data Protection Platforms ». Le groupe des Leaders s’est élargi à Cohesity, Commvault, Druva, Dell, Rubrik, Veeam et Veritas, avec Druva qui rejoint pour la première fois ce groupe. Veeam est resté numéro un en capacité d’exécution pour la sixième année consécutive, mais avec un écart resserré face à ses poursuivants. Commvault s’est classé deuxième en exécution et troisième en vision stratégique, confirmant une position très solide. Selon une analyse publiée en juillet 2026 par Blocks & Files, l’édition 2026 du quadrant regroupe Cohesity, Commvault, Rubrik et Veeam dans un mouchoir de poche parmi les Leaders, Veeam gardant la tête d’une courte tête. Le message pour un acheteur français est clair : Veeam et Commvault restent les deux valeurs sûres validées par les analystes indépendants, tandis qu’Acronis a fait un choix stratégique assumé de ne plus concourir sur ce segment.

## Avis clients et benchmarks indépendants

Au-delà des analystes, les avis d’utilisateurs réels donnent un signal complémentaire. Une synthèse publiée en mai 2026 par le comparateur indépendant Comparisec, qui décrit Veeam comme la référence de la catégorie sauvegarde et reprise après sinistre pour sa couverture de charges de travail et la maturité de sa vérification de restauration, rassemble trois sources de notation distinctes : une note de 4,6 sur 5 sur G2 (626 avis), une note de 4,8 sur 5 sur Gartner Peer Insights (124 avis vérifiés) et une note de 8,7 sur 10 sur PeerSpot (280 avis). Veeam a par ailleurs été désigné « Customers’ Choice » par Gartner Peer Insights pour la catégorie Backup & Data Protection Platforms, avec un taux de recommandation de 98 % au 31 mars 2026, un chiffre que l’éditeur a lui-même mis en avant dans sa communication d’avril 2026.

Les avis Gartner Peer Insights consultés pour Acronis Cyber Protect mettent en avant la rapidité de la sauvegarde d’image complète et l’intérêt de disposer d’un EDR intégré directement dans l’outil de sauvegarde, un utilisateur notant que la solution avait bloqué plusieurs attaques zero-day sur cinq ans d’usage. Pour Commvault Cloud, les fiches Gartner Peer Insights confirment sa présence dans les catégories sauvegarde d’entreprise et sauvegarde en tant que service, mais aucune moyenne agrégée précise n’était accessible au moment de la rédaction. Cette absence de donnée publique n’est pas un signal négatif en soi : elle reflète surtout un mode de vente Commvault davantage orienté grands comptes et cycles de vente longs, où les retours passent plus souvent par des références commerciales directes que par des plateformes d’avis en libre accès comme PeerSpot.

## Tarification 2026 : combien coûte chaque solution

Les trois éditeurs publient des niveaux de transparence tarifaire très différents. Veeam communique une fourchette de liste claire pour sa licence universelle VUL, avec trois paliers par charge de travail et par an. Des tarifs de revendeurs identifiés en août 2026, vendus par lots de dix instances, affichent des remises substantielles par rapport au prix catalogue. Acronis publie des prix par poste très lisibles pour son offre destinée aux postes de travail et serveurs, tandis que sa version Cloud pour MSP reste facturée sur devis, par charge de travail et par mois, avec des tarifs négociés au cas par cas. Commvault ne publie aucun tarif public : toute la gamme Cloud Platform se vend sur devis, ce qui reste la norme pour les plateformes visant les très grands comptes.

| Éditeur | Édition | Prix public | Unité | 
|---|---|---|---|
| Veeam | VUL Standard | 250 $ | par charge de travail / an | 
| Veeam | VUL Advanced | 350 $ | par charge de travail / an | 
| Veeam | VUL Premium | 450 $ | par charge de travail / an | 
| Veeam (revendeur, pack de 10) | Foundation | ~156 $ | par charge de travail / an | 
| Veeam (revendeur, pack de 10) | Premium | ~228 $ | par charge de travail / an | 
| Veeam SaaS (G-Cloud UK) | Standard à Enterprise+ | 2,95 £ à 6,49 £ | par charge de travail / mois | 
| Acronis | Cyber Protect Standard | 85 $ | par poste / an | 
| Acronis | Backup Advanced | 109 $ | par poste / an | 
| Acronis | Cyber Protect Advanced | 129 $ | par poste / an | 
| Acronis | Serveur Standard | 595 $ | par serveur / an | 
| Acronis | Serveur Advanced | 779 $ à 925 $ | par serveur / an | 
| Acronis | Hôte virtuel | 705 $ à 1 019 $ | par hôte / an | 
| Commvault | Cloud Platform | Sur devis | — | 

Ces montants sont exprimés en dollars ou en livres sterling car ce sont les devises dans lesquelles les grilles publiques ont été communiquées par les éditeurs et leurs revendeurs. En France, les partenaires locaux facturent généralement en euros, TVA en sus, avec des remises volume qui rapprochent souvent le coût réel des paliers bas des grilles publiques. Pour un parc de 200 charges de travail, l’écart entre un contrat Veeam VUL Standard au prix catalogue et un contrat Acronis Cyber Protect Advanced pour la même volumétrie de postes peut dépasser 24 000 dollars par an, ce qui justifie pleinement un chiffrage précis avant tout arbitrage.

## Cinq cas d’usage réels : quelle solution pour quel profil

- **PME de 30 à 80 postes sans équipe IT dédiée** : Acronis Cyber Protect Standard ou Advanced couvre la sauvegarde et l’anti-malware dans une seule console, avec un prix par poste prévisible et une prise en main rapide, sans nécessiter d’expertise en architecture de stockage.
- **ETI multi-sites avec VMware et Hyper-V mélangés** : Veeam Backup & Replication reste le choix le plus sûr grâce à sa matrice d’hyperviseurs la plus large du marché, qui évite de dupliquer les outils quand les sites n’utilisent pas tous la même virtualisation.
- **Grand compte avec une empreinte AWS lourde** : Commvault Cloud Platform, avec sa couverture de plus de 30 services AWS et son orchestration Cloud Rewind, convient mieux aux environnements cloud-natifs complexes qu’une simple sauvegarde de machines virtuelles.
- **MSP gérant plusieurs dizaines de clients** : Acronis Cyber Protect Cloud ou Veeam Service Provider Console, selon que la priorité va à l’intégration sécurité (Acronis) ou à la richesse fonctionnelle de sauvegarde pure (Veeam), les deux proposant une console multi-tenant.
- **Établissement de santé ou collectivité soumis à NIS2** : Commvault Air Gap Protect ou Veeam Data Cloud Vault, pour disposer d’une copie air-gappée et immuable capable de résister même si l’infrastructure de production et le serveur de sauvegarde principal sont compromis simultanément.
- **Banque ou assureur soumis à DORA** : la combinaison SureBackup de Veeam ou Cloud Rewind de Commvault permet de documenter des tests de restauration réguliers et vérifiables, une exigence de plus en plus scrutée par les régulateurs financiers européens.

## Avantages et inconvénients de chaque plateforme

**Veeam Backup & Replication.** Avantages : la matrice de compatibilité la plus large du marché, un écosystème de partenaires stockage très étendu, des notes utilisateurs élevées sur trois plateformes indépendantes, et un rythme de correctifs suivi de près par les CERT nationaux. Inconvénients : c’est aussi l’éditeur qui a concentré le plus d’avis de sécurité publics en 2026, ce qui impose une discipline de patch management stricte, et le prix catalogue VUL peut grimper vite sur de gros volumes sans négociation.

**Acronis Cyber Protect.** Avantages : le seul des trois à fusionner nativement sauvegarde et EDR, une tarification par poste très lisible, et une console pensée dès le départ pour les MSP multi-clients. Inconvénients : absence du Magic Quadrant Gartner depuis 2024, documentation publique plus limitée sur l’immuabilité et l’air gap, et une matrice d’hyperviseurs moins détaillée publiquement que celle de ses deux concurrents.

**Commvault Cloud Platform.** Avantages : treize années consécutives en position de Leader chez Gartner selon l’éditeur, une fonctionnalité air gap native robuste (Air Gap Protect), et une orchestration de reprise pensée pour des chaînes applicatives entières plutôt que des machines isolées. Inconvénients : aucune tarification publique disponible, ce qui complique la budgétisation en amont, et un numéro de version unique introuvable dans la documentation publique, signe d’une gouvernance produit plus opaque pour un acheteur qui compare rapidement plusieurs éditeurs.

## Guide de migration : changer d’éditeur de sauvegarde sans rompre la continuité

Changer de solution de sauvegarde est une opération à risque si elle est mal préparée, car elle touche directement le filet de sécurité de toute l’entreprise. Voici la séquence recommandée pour passer d’un éditeur à un autre sans période de vulnérabilité.

1. Cartographier l’existant : inventaire des charges de travail, des politiques de rétention en place et des exigences réglementaires applicables (NIS2, DORA, secteur santé).
2. Définir les objectifs de RTO et RPO cible pour chaque catégorie d’application avant de comparer les fonctionnalités des éditeurs candidats.
3. Déployer la nouvelle solution en parallèle de l’ancienne, sur un sous-ensemble non critique, pendant au moins un cycle de rétention complet.
4. Valider chaque restauration test dans un environnement isolé (SureBackup côté Veeam, Cloud Rewind côté Commvault) avant de basculer un système en production.
5. Migrer les charges critiques par vagues, en conservant la solution historique active jusqu’à ce que la nouvelle ait produit au moins deux cycles de sauvegarde complets et vérifiés.
6. Réviser les accès et l’authentification : SSO, RBAC et validation à plusieurs personnes doivent être reconfigurés sur la nouvelle plateforme dès le premier jour, pas après coup.
7. Décommissionner l’ancienne solution seulement après un audit de conformité confirmant que toutes les obligations de rétention légale sont couvertes par la nouvelle plateforme.
8. Documenter le changement pour les auditeurs NIS2 ou Cyber Resilience Act, avec la date de bascule et les preuves de tests de restauration.

Pour les entreprises qui construisent leur infrastructure de sauvegarde en interne plutôt que de dépendre entièrement d’un éditeur commercial, le site propose aussi un guide dédié pour monter un NAS DIY avec TrueNAS, une option complémentaire pour héberger une copie de sauvegarde locale à moindre coût.

## Conformité NIS2, DORA et Cyber Resilience Act : impact sur le choix

Le cadre réglementaire européen change la manière dont un DSI doit évaluer un outil de sauvegarde. La directive NIS2 impose aux opérateurs essentiels et importants de démontrer des capacités de continuité d’activité et de gestion des incidents, ce qui rend obligatoire la preuve de tests de restauration réguliers, pas seulement l’existence d’une sauvegarde. Le site détaille les obligations précises de mise en conformité dans son guide sur la directive NIS2 en 12 étapes. Le Cyber Resilience Act, de son côté, pousse les éditeurs de logiciels à publier une nomenclature logicielle (SBOM) et à divulguer leurs vulnérabilités dans des délais contraints, un mouvement que Veeam illustre bien avec la publication rapide de sa note KB4743 après la découverte de la faille référencée EUVD-2026-11595. Le dossier consacré au Cyber Resilience Act et à la SBOM explique comment ces obligations de transparence s’appliquent concrètement aux éditeurs de logiciels de sauvegarde.

Pour les établissements financiers, le règlement DORA ajoute une couche supplémentaire : les tests de résilience opérationnelle numérique doivent démontrer qu’un incident majeur, y compris une attaque contre l’infrastructure de sauvegarde elle-même, n’empêche pas la reprise d’activité dans des délais définis à l’avance. Concrètement, cela favorise les éditeurs capables de documenter des scénarios de reprise vérifiés de bout en bout, un point sur lequel Veeam (SureBackup) et Commvault (Cloud Rewind) ont une longueur d’avance documentée sur Acronis dans les sources publiques disponibles à ce jour.

## Performance et retours d’expérience terrain

Aucune des trois plateformes ne publie de chiffres bruts de débit de sauvegarde ou de taux de déduplication vérifiés par un laboratoire indépendant type GigaOm ou ESG dans les sources consultées pour cet article. C’est un point de vigilance à connaître avant tout achat : les promesses de vitesse restent largement qualitatives dans la documentation publique. Les retours d’expérience disponibles sur Gartner Peer Insights pour Acronis évoquent une sauvegarde d’image complète jugée très rapide par les utilisateurs, sans chiffre MB/s précis communiqué. La synthèse Comparisec de mai 2026 qualifie Veeam de référence de catégorie pour la maturité de sa vérification de restauration, ce qui est un indicateur de fiabilité opérationnelle plus révélateur qu’un simple débit brut pour une équipe qui doit vraiment redémarrer après un sinistre.

Ce manque de benchmarks chiffrés indépendants et publics constitue en soi une recommandation pratique : avant tout engagement contractuel sur un volume important de charges de travail, il reste indispensable de mener un test de charge (proof of concept) sur un échantillon représentatif de son propre environnement, plutôt que de se fier uniquement aux chiffres marketing des éditeurs.

## Verdict : quelle solution choisir en 2026

Sur la base des données disponibles fin août 2026, Veeam Backup & Replication reste le choix le plus solide pour les ETI et grands comptes avec une infrastructure virtualisée hétérogène, à condition d’accepter une discipline de patch management stricte face au rythme des avis de sécurité qui le concernent. Commvault Cloud Platform s’impose pour les environnements cloud-natifs complexes et les organisations qui doivent démontrer une orchestration de reprise éprouvée face aux régulateurs, malgré l’opacité de sa grille tarifaire publique. Acronis Cyber Protect reste imbattable sur le rapport simplicité-prix pour les PME et les MSP qui veulent une seule console pour la sauvegarde et la sécurité des postes, même s’il a quitté le radar des grands cabinets d’analystes pour la sauvegarde d’entreprise pure.

Le vrai enseignement de l’avis CERT-FR du 5 août 2026 dépasse le choix d’un éditeur. Quelle que soit la solution retenue, une copie de sauvegarde air-gappée et immuable, testée régulièrement dans un environnement isolé, n’est plus une option de confort. C’est la seule garantie qu’une compromission du serveur de sauvegarde lui-même, comme celle documentée dans la faille CVSS 8,8 référencée EUVD-2026-11595, ne se transforme pas en perte totale de données.

## Questions fréquentes

### Veeam est-il toujours sûr à utiliser après l’avis CERT-FR du 5 août 2026 ?

Oui, à condition d’appliquer les correctifs recommandés. Le CERT-FR ne demande pas d’abandonner Veeam Service Provider Console ou Veeam ONE, mais de mettre à jour ces composants vers les versions corrigées (9.3.0.35057 et 13.1.0.7034 respectivement) sans délai. La rapidité de publication des correctifs par Veeam, documentée dans sa note KB4743, est d’ailleurs un argument en faveur de sa maturité de gestion des vulnérabilités.

### Quelle est la différence essentielle entre Veeam, Acronis et Commvault ?

Veeam couvre la plus large matrice d’hyperviseurs et de cloud publics, Commvault se distingue par son orchestration de reprise et son air gap natif pour les environnements complexes, et Acronis fusionne sauvegarde et cybersécurité (EDR) dans un seul agent, avec une tarification par poste plus simple à lire pour une PME.

### Combien coûte Veeam par rapport à Acronis pour 100 postes ?

Au tarif catalogue public, la licence universelle Veeam Standard démarre à 250 dollars par charge de travail et par an, contre 85 dollars par poste et par an pour Acronis Cyber Protect Standard. Sur un parc de 100 unités, l’écart de prix catalogue dépasse 16 000 dollars par an, mais les remises volume négociées avec des revendeurs peuvent réduire sensiblement cet écart.

### Pourquoi Commvault ne publie-t-il pas ses prix ?

Comme la plupart des éditeurs qui ciblent en priorité les grands comptes, Commvault vend ses licences sur devis, avec des tarifs adaptés au volume de données, au nombre de charges de travail et aux options d’orchestration retenues. C’est une pratique commerciale courante à ce niveau du marché, mais elle impose de solliciter plusieurs devis avant toute décision budgétaire.

### Faut-il un troisième outil pour une copie air-gappée si on utilise déjà Acronis ?

C’est recommandé tant qu’Acronis ne documente pas publiquement de mécanisme d’immuabilité ou d’air gap comparable à Veeam Data Cloud Vault ou Commvault Air Gap Protect. Une solution simple consiste à répliquer une copie hors ligne ou vers un stockage objet verrouillé (Object Lock) indépendant, en complément de la sauvegarde principale.

### La faille CVE-2026-44963 évoquée dans certains rapports est-elle confirmée ?

Non, pas à la date du 29 août 2026. Il s’agit d’un signalement de veille sur les menaces qui n’a été confirmé ni par une analyse technique publique indépendante, ni par une reconnaissance officielle de Veeam. Les administrateurs doivent surveiller les communications officielles de l’éditeur et du CERT-FR plutôt que d’agir sur la base de ce seul rapport non vérifié.

### Quelle solution est la mieux notée par les Gartner Magic Quadrant en 2026 ?

Veeam et Commvault sont tous deux classés Leaders dans le Magic Quadrant 2026 « Backup and Data Protection Platforms », regroupés avec Cohesity et Rubrik dans un groupe de tête très resserré. Selon une analyse de juillet 2026, Veeam garde une courte avance en capacité d’exécution. Acronis ne figure plus dans ce quadrant depuis 2024, l’éditeur ayant recentré son offre sur les MSP et les postes de travail plutôt que sur la sauvegarde d’entreprise classique.

### Peut-on utiliser ces solutions pour se conformer à NIS2 ?

Elles y contribuent, mais aucune ne suffit à elle seule. NIS2 exige une stratégie de continuité d’activité documentée, avec des tests de restauration réguliers et prouvables. Les fonctions de vérification automatisée comme SureBackup chez Veeam ou Cloud Rewind chez Commvault facilitent cette preuve, mais la conformité repose aussi sur les procédures internes, la gouvernance des accès et la notification d’incidents, pas uniquement sur l’outil de sauvegarde choisi.
