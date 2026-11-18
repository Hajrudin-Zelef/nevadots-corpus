Cursus

Votre équipe vient d’obtenir un budget pour déployer GitHub Copilot à l’échelle de l’organisation d’ingénierie. Pour en tirer le meilleur parti, il faut comprendre comment s’articulent les paramètres de politique, les exclusions de fichiers et les requêtes de journaux d’audit, car c’est là que se trouve la vraie valeur de la plateforme.

La surface de configuration est vaste parce que les besoins le sont tout autant. Un développeur solo sur des projets personnels ne fait pas face aux mêmes enjeux de confidentialité et de conformité qu’un administrateur d’entreprise qui gère des milliers de sièges sur des dépôts réglementés. La structure de formules par paliers de GitHub Copilot est conçue pour couvrir cet éventail.

Ce guide présente chaque niveau de formule Copilot, les frontières de confidentialité et de propriété intellectuelle (PI) qui les distinguent, ainsi que les mécanismes d’administration nécessaires pour passer à l’échelle dans une organisation.

Avant d’entrer dans l’administration, vous devriez déjà être à l’aise avec les organisations, les dépôts et le système d’autorisations de GitHub. Si vous découvrez l’écosystème, commencez par notre guide Comment utiliser GitHub Copilot.

Si vous hésitez encore entre Copilot et le reste du marché, notre sélection des 13 meilleurs assistants de programmation IA en 2026 couvre l’ensemble du paysage concurrentiel. Pour une comparaison ciblée avec l’un des principaux concurrents, consultez notre guide Cursor vs. GitHub Copilot.

## En bref

- GitHub propose quatre niveaux individuels (Free, Student, Pro, Pro+) et deux niveaux organisationnels (Business et Enterprise) pour Copilot, chacun avec des périmètres distincts de confidentialité, de gouvernance et d’usage.
- Les formules Business et Enterprise apportent des garanties contractuelles que les données d’interaction ne sont jamais utilisées pour l’entraînement, tandis que les formules individuelles sont en opt-out par défaut depuis avril 2026.
- Choisissez d’abord votre formule GitHub Copilot selon vos exigences de conformité et de gouvernance ; optimisez ensuite le choix des modèles et les quotas d’usage.
- Les règles d’exclusion de fichiers et les paramètres de politique à l’échelle de l’organisation ne sont disponibles que sur Business et Enterprise, ce qui en fait la base pour les équipes qui traitent du code propriétaire.
- GitHub Copilot Enterprise nécessite un abonnement actif à GitHub Enterprise Cloud, ce qui porte le coût minimal réel à 60 $ par utilisateur et par mois.
- La gestion des sièges, les requêtes de journaux d’audit et l’application des politiques peuvent être automatisées via l’API REST, transformant les licences en infrastructure-as-code.

## Aperçu des formules GitHub Copilot

GitHub propose plusieurs paliers pour son écosystème. Notamment, la plateforme finalise sa transition vers une facturation à l’usage, remplaçant l’ancien cadre « Premium Request Unit » (PRU) par les GitHub AI Credits en juin 2026.

Dans le nouveau système, les complétions de code de base et les suggestions « Next Edit » restent illimitées et ne consomment pas de crédits.

En revanche, les opérations avancées comme le chat multi‑fichiers, les workflows agentiques, les sessions de codage longues et les relectures de code approfondies consomment des AI Credits en fonction des jetons (entrée, sortie et cache) par rapport aux tarifs API publiés du modèle concerné.

Les prix d’abonnement mensuels de base sont restés stables, mais ce changement modifie la manière dont les administrateurs budgètent les dépassements et suivent l’usage effectif.

| **Niveau de formule** | **Public visé** | **Tarif de base** | **Volume mensuel inclus** | **Différenciateurs clés** | 
| Free | Utilisateurs individuels occasionnels | Gratuit | AI Credits limités | Accès basique aux complétions et au Chat. | 
| Student | Étudiants et enseignants vérifiés | Gratuit | AI Credits étendus | Accès plus large aux modèles pour l’apprentissage. | 
| Pro | Développeurs individuels | 10 $ / mois | 1 000 Base + 500 Flex (1 500 au total) | Intégrations IDE étendues et support multi‑modèles. | 
| Pro+ | Utilisateurs individuels intensifs | 39 $ / mois | 3 900 Base + 3 100 Flex (7 000 au total) | Gros quotas de jetons ; inclut l’accès à GitHub Spark. | 
| Business | Équipes et organisations | 19 $ / utilisateur / mois | 1 900 crédits / utilisateur (3 000 du 1er juin au 1er sept. 2026) | Gestion centralisée des sièges, journaux d’audit, exclusions de fichiers, indemnisation PI. | 
| Enterprise | Grandes entreprises | 39 $ / utilisateur / mois | 3 900 crédits / utilisateur (7 000 du 1er juin - 1er sept. 2026) | Indexation des dépôts, affinement personnalisé, gouvernance globale. | 

### Formules individuelles : Free, Student, Pro et Pro+

Les formules individuelles diffèrent par l’accès aux modèles, les limites d’usage et les capacités expérimentales. Par exemple, la formule Free permet l’exploration de base, tandis que Pro+ donne accès à GitHub Spark, un environnement conçu pour créer des applications assistées par l’IA.

Actuellement, les nouvelles inscriptions aux comptes payants individuels de GitHub, comme Pro, Pro+ et Student, sont suspendues. Les comptes existants peuvent passer de Pro à Pro+, mais les nouveaux comptes ne peuvent pas s’inscrire tant que GitHub n’a pas achevé la transition vers la facturation aux AI Credits.

### Business et Enterprise

Avec Business et Enterprise, les formules GitHub Copilot passent d’une simple extension d’IDE à un véritable actif d’infrastructure d’entreprise, entièrement auditable.

GitHub Copilot Business introduit des fonctionnalités essentielles de gestion :

- Attribution et retrait centralisés des sièges.
- Politiques de référence à l’échelle de l’organisation.
- Journaux d’audit structurés et suivi des événements de conformité.
- Exclusions de contenu et de fichiers de dépôts.
- Indemnisation commerciale en propriété intellectuelle.

GitHub Copilot Enterprise va plus loin en matière de contrôle et de capacités :

- Copilot Spaces : un hub de connaissances permettant d’interroger Copilot sur la documentation interne, les wikis et les normes de code système.
- Intégration renforcée de GitHub.com Chat.
- Héritage hiérarchique des politiques entre organisations filles.

GitHub Copilot Enterprise nécessite un abonnement actif à GitHub Enterprise Cloud. Comme GitHub Enterprise Cloud coûte 21 $ par utilisateur et par mois et que la licence Copilot Enterprise est à 39 $ par utilisateur et par mois, le coût minimal réel est de 60 $ par utilisateur et par mois pour Enterprise. Cela ne s’applique pas à la formule GitHub Copilot Business, qui peut être achetée nativement par des organisations utilisant GitHub Free ou GitHub Team.

Les organisations n’ont pas accès aux avantages Enterprise comme l’héritage des politiques, mais disposent tout de même de l’indemnisation PI, de l’audit, de l’exclusion de fichiers et de la gestion des politiques au niveau organisationnel ; une bonne alternative pour des équipes d’ingénierie de taille moyenne.

Si vous envisagez un abonnement Enterprise, notre guide GitHub Copilot Enterprise vous montrera comment exploiter ses fonctionnalités, comme Copilot Spaces et la nouvelle Usage Metrics API.

## Ce qui distingue les formules individuelles des formules Business

La gestion des données, l’indemnisation PI et la facturation sont les principaux domaines où les formules individuelles et Business diffèrent fortement. Les fonctionnalités supplémentaires pour les utilisateurs sont utiles, mais comprendre ces écarts est essentiel pour décider entre gérer une pile de licences Pro personnelles et souscrire une formule Business.

### Traitement des données et paramètres d’entraînement par défaut

Pour les équipes qui gèrent des systèmes propriétaires, la confidentialité des données est généralement le facteur décisif entre des formules personnelles et une souscription Business.

En avril 2026, GitHub a modifié la collecte des données d’interaction pour les formules individuelles Copilot. Pour les utilisateurs Free, Pro et Pro+, les données d’interaction peuvent désormais servir à l’entraînement des modèles par défaut, sauf si l’utilisateur se désinscrit explicitement.

Clarifions la différence entre le code au repos et les données d’interaction, afin de savoir ce qui est utilisé pour l’entraînement de l’IA :

- **Code au repos :** le code brut présent dans votre dépôt privé n’est pas lu ni intégré à des jeux d’entraînement publics.
- **Données d’interaction :** inclut les invites, requêtes de chat, contexte du curseur, blocs de code environnants transmis via l’API de l’IDE pendant les sessions actives, métriques d’acceptation des suggestions et journaux de feedback.

Les contrats Business et Enterprise garantissent strictement que les données d’interaction ne sont jamais utilisées à des fins d’entraînement, quelles que soient les circonstances. Aucune action manuelle de l’utilisateur n’est requise.

Pour approfondir l’usage des données et la résolution des problèmes dans Copilot, lisez notre guide GitHub Copilot : confidentialité et dépannage.

### Indemnisation en propriété intellectuelle

GitHub Copilot Business et Enterprise incluent une indemnisation en propriété intellectuelle (PI) pour le code généré. Les formules individuelles n’en bénéficient pas.

Concrètement, l’indemnisation signifie que GitHub s’engage contractuellement à fournir une protection juridique dans des circonstances spécifiées si le code généré entraîne des litiges de PI. Cela ne supprime pas tous les risques juridiques, mais modifie la discussion sur la responsabilité pour les équipes qui livrent des logiciels commerciaux.

Un freelance qui livre du code à des clients doit y prêter attention. La différence entre « outil de productivité personnel » et « plateforme de développement soutenue par l’organisation » devient très concrète dès lors qu’entrent en jeu des contrats et des livraisons commerciales.

### Facturation, sièges et passage aux AI Credits

La facturation individuelle est en libre-service et liée aux comptes personnels. Les formules Business centralisent la facturation avec des sièges attribués par l’administrateur. De plus, au lieu que chaque utilisateur gère un lot de crédits indépendant, l’organisation mutualise ses AI Credits mensuels selon le nombre d’utilisateurs.

Les formules Enterprise vont plus loin, avec des limites d’application budgétaire granulaires, des regroupements par centres de coûts et des allocations par département afin d’éviter qu’un seul groupe de développement, via des workflows agentiques intensifs, n’épuise tout le stock de crédits de l’entreprise.

## SKUs et considérations de confidentialité

Comprendre les protections de confidentialité et les SKUs est essentiel. Les frontières architecturales régissant les flux de données, les protections juridiques et le suivi selon les différents paliers sont résumées ci‑dessous :

| **Niveau de formule** | **Données d’interaction utilisées pour l’entraînement ?** | **Indemnisation PI contractuelle ?** | **Exclusions de contenu / fichiers ?** | **Accès aux journaux d’audit ?** | 
| Free | Oui (opt‑out possible) | Non | Non | Non | 
| Student | Oui (opt‑out possible) | Non | Non | Non | 
| Pro | Oui (opt‑out possible) | Non | Non | Non | 
| Pro+ | Oui (opt‑out possible) | Non | Non | Non | 
| Business | Non | Oui | Oui | Oui | 
| Enterprise | Non | Oui | Oui | Oui | 

### Changements de politique d’entraînement d’avril 2026

Le passage d’un modèle en opt‑in à un cadre en opt‑out pour les formules individuelles constitue un vecteur majeur de fuite de conformité. La charge utile des données d’interaction capturée automatiquement pendant une session IDE active inclut :

- Des historiques de chat détaillés et le contexte des invites.
- Des suggestions de code multi‑lignes et des taux d’acceptation locaux.
- Le contexte du curseur de l’éditeur actif, qui récupère souvent le contexte des fichiers adjacents, les instructions d’import et les déclarations de variables des onglets ouverts.

Imaginez qu’un développeur utilise un compte personnel Copilot Pro dans un dépôt d’entreprise. Si l’entraînement reste activé, les données d’interaction liées à cette session peuvent entrer dans l’écosystème d’entraînement de GitHub. C’est une raison courante pour laquelle les organisations adoptent les formules Business.

### Choisir le bon SKU selon vos exigences de confidentialité

Selon la nature des travaux, vous n’aurez pas besoin du même SKU.

- **Développeur solo / projets personnels :** les formules Free ou Pro offrent une flexibilité maximale. Désactivez simplement l’entraînement dans vos paramètres de confidentialité si vous travaillez sur du code propriétaire.
- **Freelances / sous‑traitants :** la formule Business fournit une barrière de protection. Les contrats clients interdisent souvent l’envoi de données à des fournisseurs de LLM externes ; un siège dédié au sein de l’organisation protège vos engagements.
- **Équipes en entreprise avec obligations de conformité** : la formule Business constitue la base standard, garantissant l’isolation des flux de données et la gouvernance administrative.
- **Secteurs réglementés (finance, santé) :** la formule Enterprise est généralement indispensable, permettant l’intégration à des configurations de sécurité spécialisées, des exigences strictes de résidence des données et des couches d’affinement localisées.

## Exclure des fichiers spécifiques de Copilot

La mise en place de règles d’exclusion de fichiers dans GitHub Copilot est l’un des moyens les plus efficaces de sécuriser votre environnement. L’exclusion de contenu empêche l’agent IDE local de traiter certains fichiers, les rendant totalement invisibles pour les complétions en ligne, les boîtes de dialogue et les opérations agentiques en arrière‑plan.

Notez que GitHub Copilot CLI, l’agent cloud de Copilot et le mode Agent dans Copilot Chat dans les IDE ne prennent pas en charge l’exclusion de contenu.

### Configurer les règles d’exclusion

Les équipes d’administration peuvent appliquer des exclusions au niveau global des paramètres de l’organisation ou au niveau des dépôts ciblés. Il suffit d’ouvrir les paramètres du dépôt ou de l’organisation en cliquant sur le bouton Settings en haut à droite.

Choisissez « Code and automation » dans les paramètres Copilot de la barre latérale. Renseignez ensuite vos exclusions dans la zone « Paths to exclude in this repository » comme suit :

```
# Ignore the /src/some-dir/kernel.rs file in this repository.
- "/src/some-dir/kernel.rs"
# Ignore files called secrets.json anywhere in this repository.
- "secrets.json"
# Ignore all files whose names begin with secret anywhere in this repository.
- "secret*"
# Ignore files whose names end with .cfg anywhere in this repository.
- "*.cfg"
# Ignore all files in or below the /scripts directory of this repository.
- "/scripts/**"
```
**Le paramétrage au niveau de l’organisation est similaire, sauf que l’option se trouve sous « Repositories and Paths to exclude » et utilise le format suivant :**

```
REPOSITORY-REFERENCE:
  - "/PATH/TO/DIRECTORY/OR/FILE"
  - "/PATH/TO/DIRECTORY/OR/FILE"
  - …
```
Conserver le `REPOSITORY-REFERENCE` fait partie intégrante du paramétrage. Les bases de configuration courantes doivent prioriser les identifiants sensibles, les profils d’orchestration de production, les modules algorithmiques propriétaires sensibles ou les dossiers fortement réglementés.

### Comment les exclusions s’appliquent aux fonctionnalités de Copilot

Lorsqu’une correspondance d’exclusion est détectée, l’isolation des données est totale sur tous les sous‑systèmes Copilot :

- **Complétions en ligne :** impossibilité de générer du contexte à l’intérieur du fichier ou d’en tirer pour alimenter les fichiers adjacents.
- **Copilot chat / agents :** le système renvoie un avis indiquant que le fichier ne peut pas être examiné en raison des politiques de l’organisation.

Les moteurs IDE locaux standards fonctionnent de la même manière. Les fonctions de confort comme le parsing de texte, la coloration syntaxique et l’IntelliSense localisée compilent normalement, car la couche d’exclusion s’applique explicitement aux flux de télémétrie externes de Copilot.

Les administrateurs doivent tester soigneusement les modèles de chemins dans des dépôts de préproduction ; des jokers mal formés peuvent échouer en mode « ouvert » et exposer des données que vous souhaitiez isoler.

## Gestion des politiques à l’échelle de l’organisation

Appliquer des politiques GitHub Copilot au niveau de l’organisation garantit que la sécurité de l’entreprise est définie par l’équipe d’administration, et non par les préférences individuelles des développeurs.

### Paramètres de politique disponibles

Les organisations peuvent contrôler plusieurs paramètres pour les développeurs :

- **Activation des fonctionnalités :** activer ou désactiver globalement Copilot Chat dans les environnements de développement, les interfaces en ligne de commande (via Copilot CLI) ou les systèmes avancés de revue de code agentiques.
- **Filtre de code public :** mécanisme juridique qui empêche Copilot de proposer des suggestions trop proches de dépôts open source publics sur GitHub, réduisant les risques de non‑conformité aux licences.
- **Restrictions de choix de modèles :** limiter les modèles (par exemple, variantes spécifiques de GPT ou Claude) que les développeurs peuvent sélectionner, afin de gérer la latence, la consommation de crédits et la performance. Pour un aperçu des modèles disponibles sur la plateforme GitHub, consultez ce guide pratique des GitHub Models.
- **Instructions personnalisées d’organisation :** injecter des fichiers markdown standard qui ajoutent les conventions de code, cadres de sécurité et paradigmes d’architecture à chaque requête émise par vos développeurs.

Si votre équipe maîtrise moins bien le modèle d’organisation et d’autorisations de GitHub, le cours Intermediate GitHub Concepts fournit un bon socle. Pour les équipes d’ingénierie qui généralisent les outils en ligne de commande, voyez notre Tutoriel GitHub Copilot CLI.

### Héritage des politiques au niveau Enterprise

Dans les grands environnements d’entreprise, le moteur de politiques suit une cascade d’héritage stricte : politique Enterprise > politique d’organisation > préférences utilisateur

Les administrateurs Enterprise peuvent verrouiller des politiques globalement sur toutes les entités, autoriser des dérogations sélectives par organisation, ou déléguer totalement le contrôle dans la hiérarchie. Par exemple, l’entreprise peut imposer des paramètres globaux pour restreindre l’usage de certains modèles.

Au niveau d’une équipe, elle peut imposer au pôle services financiers des filtres de code public stricts, tout en autorisant davantage d’expérimentation au pôle R&D logiciel interne.

## Journaux d’audit

Quand les auditeurs de conformité demandent une vérification de votre chaîne d’approvisionnement logicielle, ou que les équipes sécurité doivent remonter une fuite de données, GitHub Copilot journalise les modifications de la plateforme.

### Événements Copilot dans le journal d’audit

Le système consigne un registre complet des opérations d’administration, notamment :

- Attributions et révocations explicites de sièges, changements de groupes de facturation.
- Modifications du filtre de duplication de code public.
- Changements des modèles d’exclusion de fichiers et de répertoires.
- États d’activation des fonctionnalités (ex. : activation des modes de revue de code agentiques)

Le niveau de granularité dépend de votre abonnement. Les formules Business se concentrent sur les flux d’actions au périmètre de l’organisation, tandis que les comptes Enterprise donnent accès à une télémétrie forensique inter‑organisations.

### Recherche, filtrage et export

Les flux de journaux d’audit sont accessibles nativement via le panneau Organization Settings. Les administrateurs peuvent interroger l’interface avec des qualificateurs d’action spécifiques :

```
# Filtrer pour identifier qui a modifié les droits d'accès à Copilot
action:copilot.cfb_seat_assignment_created
# Identifier les changements des exclusions globales dans une période
action:copilot.content_exclusion_updated created:2026-05-01..2026-05-31
```
Les comptes Enterprise permettent de diffuser ces événements d’audit vers des SIEM externes (comme Splunk ou Datadog) pour l’alerte automatisée et la conservation centralisée et immuable.

## Gérer les sièges Copilot avec l’API REST

L’attribution manuelle de sièges depuis un tableau de bord convient aux petites équipes, mais ne passe pas à l’échelle avec des flux d’onboarding massifs. Utiliser les endpoints seats de l’API REST de GitHub Copilot vous permet de traiter l’identité et les accès entièrement en tant que code.

C’est l’une de mes parties préférées de l’administration Copilot, car elle transforme la gestion des licences en un processus que les équipes d’ingénierie peuvent automatiser proprement.

### Principaux endpoints API

Les workflows API courants incluent :

- Lister les attributions de sièges
- Attribuer des sièges
- Retirer des sièges
- Récupérer des métriques d’usage
- Lire les paramètres Copilot de l’organisation

L’authentification requiert généralement :

- Des jetons d’accès personnels à privilèges fins
- Des autorisations d’application GitHub
- Des privilèges d’administrateur d’organisation

Pour accéder à ces fonctions d’administration, vos scripts d’intégration doivent s’authentifier avec un Personal Access Token (PAT) doté des étendues admin:org ou s’exécuter via une application GitHub autorisée avec des privilèges explicites de gestion Copilot au niveau de l’organisation.

Pour approfondir les intégrations programmatiques de la plateforme, je vous recommande de suivre notre parcours de compétences GitHub Foundations.

### Schémas d’automatisation courants

Parmi les schémas pratiques :

- 
**Onboarding identitaire automatisé :** connecter un SIRH (Workday, Okta…) directement à GitHub via des webhooks. Lorsqu’un ingénieur rejoint une équipe donnée, un script déclenche une requête`POST` pour lui provisionner automatiquement Copilot.
- 
**Récupération des sièges inactifs :** un script Cron planifié interroge l’utilisation active via l’API. Si un utilisateur n’a pas utilisé Copilot depuis plus de 30 jours, le script exécute un`DELETE` pour récupérer la licence et préserver le pool de crédits.
- 
**Tableaux de bord financiers :** extraction quotidienne des données d’allocation et de consommation pour les injecter dans des plateformes de BI internes (telles que Tableau) et faciliter la refacturation par centre de coûts.

#### Exemple : attribuer un siège Copilot avec Python

Le script suivant montre comment attribuer par programmation un siège d’organisation à un développeur spécifique avec Python :

```
	import requests
	# Identity Configuration
TOKEN = "YOUR_ORGANIZATION_ADMIN_PAT"
ORG = "your-corporate-org"
USERNAME = "target-developer-user"
url = f"https://api.github.com/orgs/{ORG}/copilot/billing/selected_users"
headers = {
    "Authorization": f"Bearer {TOKEN}",
    "Accept": "application/vnd.github+json",
    "X-GitHub-Api-Version": "2022-11-28"
}
payload = {
    "selected_usernames": [USERNAME]
}
response = requests.post(url, json=payload, headers=headers)
if response.status_code == 201:
    print(f"Successfully allocated Copilot seat to {USERNAME}.")
else:
    print(f"Failed allocation. Status: {response.status_code}")
    print(response.json())
```
## Dernières réflexions

La structure des formules GitHub Copilot paraît simple depuis la page de tarifs. Une fois que vous gérez des équipes, les différences deviennent bien plus substantielles.

Les frontières de confidentialité, les politiques d’entraînement, l’auditabilité et les contrôles de gouvernance pèsent souvent plus lourd que l’accès brut aux modèles. C’est pourquoi les discussions GitHub Copilot Business vs Enterprise deviennent généralement des échanges sur la sécurité et les opérations, plus que de simples sujets d’ingénierie.

Si je devais conseiller une équipe aujourd’hui, je partirais des exigences de gouvernance :

- Avez‑vous besoin de garanties contractuelles de confidentialité ?
- Avez‑vous besoin de journaux d’audit ?
- Avez‑vous besoin d’une gestion centralisée des politiques ?

Ensuite, j’optimiserais le volume d’usage et l’accès aux fonctionnalités.

Pour développer les compétences techniques de votre équipe et vous préparer aux certifications officielles, explorez ces parcours avancés :

## FAQ sur les formules GitHub Copilot

### Quelle est la différence entre GitHub Copilot Business et Enterprise ?

**Business inclut la gestion centralisée des sièges, les journaux d’audit, l’indemnisation PI et les contrôles de politique. Enterprise ajoute l’héritage des politiques à l’échelle de l’entreprise et des fonctionnalités de gouvernance élargies.**

### GitHub Copilot entraîne‑t‑il ses modèles sur le code des dépôts privés ?

**Non. GitHub indique que le code des dépôts privés n’est pas utilisé directement pour l’entraînement. Toutefois, les données d’interaction des formules individuelles peuvent être collectées sauf si l’utilisateur se désinscrit. Les formules Business et Enterprise empêchent contractuellement l’entraînement sur les données d’interaction.**

### À quoi servent les journaux d’audit GitHub Copilot ?

**Les journaux d’audit aident les administrateurs à suivre les attributions de sièges, les changements de politique, les activations de fonctionnalités et l’activité de gouvernance dans l’organisation.**

### Qu’est‑ce que l’exclusion de fichiers dans GitHub Copilot ?

**L’exclusion de fichiers empêche Copilot d’accéder à des fichiers ou répertoires spécifiés pour les complétions, le chat et les suggestions générées par l’IA. Cette fonctionnalité est disponible uniquement avec les formules Business et Enterprise.**

Je suis un data scientist avec de l'expérience dans l'analyse spatiale, l'apprentissage automatique et les pipelines de données. J'ai travaillé avec GCP, Hadoop, Hive, Snowflake, Airflow et d'autres processus d'ingénierie et de science des données.