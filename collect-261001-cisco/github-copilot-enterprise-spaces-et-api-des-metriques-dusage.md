Cours

Vous avez déployé GitHub Copilot Enterprise dans l’organisation, attribué les licences, configuré les politiques, et vos développeurs l’utilisent déjà dans leurs IDE. Il faut maintenant répondre aux questions difficiles :

- Comment optimiser Copilot pour qu’il apprenne mieux le contexte d’ingénierie propre à votre entreprise ?
- Comment mesurer la valeur de Copilot ? Quels départements l’adoptent avec succès et lesquels l’ignorent totalement ?

C’est là que GitHub Copilot Spaces et l’API des métriques d’usage entrent en jeu. Spaces permet à Copilot d’ingérer le savoir technique de votre organisation. L’API des métriques d’usage aide les administrateurs à mesurer l’adoption, la rétention et les tendances de productivité à l’échelle de l’entreprise.

Dans cet article, nous verrons :

- Ce que comprend GitHub Copilot Enterprise
- Comment fonctionnent Copilot Spaces
- Comment configurer Spaces à l’échelle
- Les endpoints de l’API des métriques d’usage de GitHub Copilot
- Les workflows d’authentification et de reporting
- Des stratégies concrètes pour mesurer le ROI

Si vous n’êtes pas à l’aise avec les organisations GitHub, les pull requests et les modèles d’autorisations, le cours Intermediate GitHub Concepts couvre ces fondamentaux. Si vous découvrez aussi Copilot, notre tutoriel How to Use GitHub Copilot présente les fonctionnalités de base sur lesquelles ce guide s’appuie.

## Renforcez la confidentialité et la gouvernance de vos données

Garantissez la conformité et protégez votre entreprise avec DataCamp for Business. Des cours spécialisés et un suivi centralisé pour protéger vos données.

## Qu’est-ce que GitHub Copilot Enterprise ?

GitHub Copilot Enterprise se situe au sommet de la gamme des offres Copilot de GitHub.

Comparé à GitHub Copilot Business ou Pro+, Enterprise met l’accent sur la gouvernance, le contexte organisationnel et les capacités de mesure. Il est conçu pour les entreprises gérant de vastes environnements d’ingénierie, plutôt que pour des développeurs individuels ou de petites équipes.

Deux capacités comptent surtout dans la pratique :

1. Contexte organisationnel personnalisé via les **Spaces**
2. Télémétrie à l’échelle de l’organisation via l’**API des métriques d’usage**

Ces deux fonctionnalités font passer Copilot d’un simple « autocomplétion intelligente » à quelque chose qui s’apparente à une véritable plateforme interne d’ingénierie propulsée par l’IA.

Les entreprises qui tirent le plus de valeur de GitHub Copilot Enterprise l’intègrent comme un élément clé de leur infrastructure interne. Elles soignent le contexte organisationnel, mesurent l’adoption en continu et ajustent les politiques à partir des données d’usage, pas d’hypothèses.

Pour un tour d’horizon plus large de l’écosystème GitHub, nous vous recommandons notre guide Introduction to GitHub Products.

### En quoi Enterprise diffère de Business et Pro+

GitHub Copilot Enterprise étend l’offre Business avec notamment :

- Métriques d’usage au niveau de l’organisation
- Contrôles de gouvernance renforcés
- Héritage des politiques à l’échelle de l’entreprise
- Quotas supérieurs pour les requêtes premium (1 000 contre 300 en offre Business)
- Accès et gestion de modèles supplémentaires

Enterprise nécessite GitHub Enterprise Cloud en plus de l’abonnement Copilot Enterprise. Cela ajoute un coût supplémentaire par utilisateur : assurez-vous donc que votre organisation a réellement besoin de gouvernance, de télémétrie et d’administration au niveau entreprise.

| **Fonctionnalité** | **Pro+** | **Business** | **Enterprise** | 
| Usage individuel | Oui | Non | Non | 
| Gestion centralisée des licences | Non | Oui | Oui | 
| Journaux d’audit | Non | Oui | Oui | 
| Exclusions de fichiers | Non | Oui | Oui | 
| Prise en charge de Spaces | Oui, avec Copilot | Oui, visibilité admin limitée | Oui, gestion complète au niveau entreprise | 
| API des métriques d’usage | Non | Niveau organisation | Niveau entreprise + organisation | 
| Héritage des politiques entreprise | Non | Non | Oui | 

**Remarque :** Les abonnés Business accèdent à l’API des métriques d’usage au niveau de l’organisation (`/orgs/{org}/…`). Les abonnés Enterprise disposent en plus de rapports agrégés au niveau de l’entreprise (`/enterprises/{enterprise}/…`) couvrant toutes les organisations dans une vue unique.

### Pour qui est GitHub Copilot Enterprise

GitHub Copilot Enterprise s’adresse aux organisations disposant d’environnements GitHub matures.

Clients Enterprise typiques :

- Grandes organisations d’ingénierie
- Secteurs réglementés
- Équipes plateformes multi-équipes
- Entreprises avec standards internes de développement
- Organisations nécessitant une gouvernance centralisée

Notez que cela n’améliore pas intrinsèquement les performances de Copilot. Cette nuance est importante : beaucoup d’équipes surdimensionnent initialement leur achat en pensant qu’Enterprise = « meilleur Copilot », alors qu’Enterprise ajoute surtout des outils de gouvernance et de mesure.

## Copilot Spaces : un contexte sur mesure pour votre organisation

Copilot Spaces résout l’une des principales limites des assistants de code généralistes.

Par défaut, Copilot maîtrise correctement le savoir public en programmation. Il ne comprend pas automatiquement vos API internes, vos décisions d’architecture, vos conventions de code, vos workflows de déploiement ou votre documentation d’onboarding.

Spaces fournit un contexte organisationnel sélectionné que Copilot peut exploiter en conversation et pour l’assistance au codage.

Concrètement, Spaces aide Copilot à répondre à des questions telles que :

- « Comment structurons-nous en interne nos gestionnaires d’API ? »
- « Quelle bibliothèque d’authentification notre équipe plateforme recommande-t-elle ? »
- « Quel workflow de déploiement ce microservice doit-il utiliser ? »
- « Quelles conventions de nommage notre équipe backend applique-t-elle ? »

### Ce que prennent en charge les Spaces

Spaces couvre une palette de contenus organisationnels plus large que l’ancien système Knowledge Bases.

Types de contenus pris en charge :

- Fichiers de code
- Documentation Markdown
- Fichiers JSON
- Fichiers téléversés
- Images
- GitHub Issues
- Pull requests

Chaque type de contenu apporte une valeur différente.

Les fichiers de code aident Copilot à comprendre les schémas d’implémentation. Les fichiers Markdown détaillent l’architecture et l’onboarding. Les pull requests exposent les discussions de revue et les décisions d’ingénierie passées. L’ensemble améliore la compréhension des pratiques de développement de votre organisation.

Point subtil mais important : Spaces n’est pas simplement une base vectorielle adossée à GitHub. Elle inclut des contrôles de partage et des workflows de gouvernance pensés pour l’entreprise.

### La fin de Knowledge Bases

GitHub a mis fin à l’ancienne fonctionnalité Copilot Knowledge Bases le 1er novembre 2025.

Spaces remplace Knowledge Bases avec :

- Un support de contenu plus large
- De meilleurs contrôles de partage
- Une administration améliorée
- Une gestion plus flexible au niveau de l’organisation

Vous trouverez encore de la documentation et des billets de blog obsolètes mentionnant Knowledge Bases. Soyez prudent avec les anciens tutoriels : beaucoup d’endpoints et de workflows ont changé entre 2025 et 2026.

## Créer et configurer des Copilot Spaces

Côté administration, créer un Copilot Space est assez simple. Le défi, c’est d’en gérer des dizaines, voire des centaines, à travers les équipes.

La structure initiale a tendance à perdurer. J’ai vu des organisations créer par inadvertance une « jungle documentaire » dans Spaces faute de règles de responsabilité claires dès le départ.

Tout le monde peut créer un Copilot Space, testons donc dans un dépôt personnel. Les étapes sont similaires au niveau Enterprise, avec quelques pages différentes.

### Configurer un Space

La création suit généralement ce workflow :

1. Accéder à la page Copilot Spaces dans la zone d’administration Enterprise
2. Créer un nouveau Space

1. Sélectionner les dépôts et sources de contenu, y compris les MCP et autres outils utiles

1. Ajouter des sources via le bouton « + Add sources » à droite

1. Choisir de partager le Space ou définir les paramètres de partage à ce stade

1. Vérifier que Copilot peut référencer le contenu pendant les conversations

Note pour les utilisateurs Enterprise : votre administrateur peut désactiver le partage des Spaces personnels. Si vous utilisez votre propre compte, cela peut limiter le partage d’un Copilot Space qui n’utilise pas les dépôts de l’entreprise.

Après la configuration, les administrateurs doivent tester le Space avec des invites concrètes.

Par exemple :

`How does our authentication middleware handle token refresh logic?`
Ou :

`Show me an example of how our backend services structure database migrations.`
Si Copilot ne peut pas répondre avec précision, la cause est généralement :

- Dépôts manquants
- Documentation de faible qualité
- Autorisations incorrectes
- Temps d’indexation insuffisant

### Partage et contrôles d’accès

Spaces prend en charge deux grands modèles de visibilité :

- Spaces individuels
- Spaces à l’échelle de l’organisation

Les membres d’une entreprise peuvent voir leurs espaces individuels gérés par les paramètres globaux de l’entreprise. Les administrateurs Enterprise peuvent aussi gérer centralement les politiques d’accès anticipé et de disponibilité des fonctionnalités.

Les Spaces privés conviennent aux équipes en expérimentation ou aux initiatives sensibles. Les Spaces organisationnels sont idéaux pour les standards d’ingénierie, l’onboarding ou les frameworks communs.

Une erreur fréquente est la sur-centralisation. Un unique Space tentaculaire et global devient vite bruyant et moins utile pour Copilot.

### Organiser les Spaces par équipe ou par domaine

Il n’existe pas une seule structure universelle.

Les schémas courants incluent un Space par équipe, un Space par produit, ou des Spaces de standards partagés. Chacun a une portée différente et exploite les mêmes réglages de façon spécifique.

#### Un Space par équipe

Utile lorsque les groupes d’ingénierie opèrent de façon relativement indépendante.

Exemples :

- Ingénierie plateforme
- Ingénierie des données
- Développement mobile

#### Un Space par produit

Utile pour les organisations structurées par produits plutôt que par départements.

Exemples :

- Paiements
- Analytique
- Infrastructure
- Plateforme client

#### Un Space de standards partagés

Beaucoup d’organisations maintiennent un Space partagé séparé pour :

- Lignes directrices de sécurité
- Conventions de code
- Workflows de déploiement
- Standards d’architecture

En pratique, les approches hybrides donnent généralement les meilleurs résultats : chaque équipe dispose de son Space, complété par de grands Spaces de standards partagés.

## L’API des métriques d’usage de Copilot

Spaces résout le problème du contexte. L’API des métriques d’usage résout le problème de la mesure. Elle a remplacé plusieurs anciens systèmes de télémétrie que GitHub a retirés lors de la consolidation des API en 2026.

Sans mesures claires, les organisations perdent rapidement de vue le succès de l’adoption de Copilot. Les directions veulent des preuves que l’investissement améliore les workflows des développeurs, et pas seulement une ligne d’abonnement supplémentaire.

Le tableau de bord est en disponibilité générale depuis février 2026 et accessible via votre compte enterprise → AI Controls → Copilot → Metrics → Copilot usage metrics dans l’onglet Insights.

### Ce que mesure l’API

L’API des métriques d’usage expose plusieurs catégories de télémétrie opérationnelle.

Parmi les métriques courantes :

- Utilisateurs actifs
- Lignes de code proposées vs lignes de code acceptées
- Profils d’usage des IDE
- Usage des modèles
- Interactions avec les agents
- Répartition par langage

Cela offre une vision bien plus fine que de simples comptages de licences.

Une équipe avec 100 licences attribuées mais seulement 15 utilisateurs actifs n’a pas le même profil d’adoption qu’une équipe avec un usage quotidien régulier et des taux d’acceptation élevés.

### La transition d’API en 2026

GitHub a retiré plusieurs anciennes API de télémétrie (User-level Feature Engagement Metrics API, Direct Data Access API, Copilot Metrics API) entre 2025 et 2026, pour une extinction complète en avril 2026.

Elles comprenaient :

- L’ancienne Metrics API
- Les Feature Engagement APIs
- Les Direct Data Access APIs

Les nouveaux endpoints de métriques d’usage, disponibles depuis février 2026, ont unifié ces systèmes de reporting dans un modèle plus cohérent, avec versionnage en cas de changements incompatibles.

C’est important car beaucoup d’anciens billets et exemples GitHub référencent encore des endpoints dépréciés. Avant d’industrialiser vos intégrations, vérifiez systématiquement la documentation la plus récente.

## Interroger l’API des métriques d’usage

Maintenant que le but de l’API des métriques est clair, voyons comment l’utiliser concrètement.

### Authentification et autorisations

Les endpoints des métriques d’usage GitHub Copilot nécessitent généralement quelques autorisations sur votre Personal Access Token (PAT), en PAT classique ou finement granulé.

- 
Pour les PAT classiques, votre admin enterprise doit vous attribuer les permissions `manage_billing:copilot` et`read:org` .
- 
Pour les tokens finement granulés, utilisez un GitHub App user access token ou installation access token avec la permission `Enterprise Copilot metrics enterprise permissions (read)` .

En général, les tokens finement granulés sont à privilégier car ils limitent l’exposition inutile des permissions.

### Endpoints au niveau organisation

Les deux rapports les plus courants au niveau organisation sont :

- 
`organization-1-day`
- 
`organization-28-day`

#### Rapport organisationnel sur une journée

Le rapport sur une journée est idéal pour le suivi opérationnel et l’analyse de tendances à court terme. L’historique remonte au 10 octobre 2025 et reste accessible pendant un an à partir de la date courante.

La commande curl ci-dessous appelle l’API de rapport 1 jour et renvoie une réponse JSON avec des liens de téléchargement. Renseignez `YOUR_TOKEN` pour le Bearer et choisissez un `DAY` au format `YYYY-MM-DD`.

```
curl -L \
 -H "Accept: application/vnd.github+json" \
 -H "Authorization: Bearer <YOUR_TOKEN>" \
-H “X-GitHub-Api-Version: 2026-03-10” \
"https://api.github.com/enterprises/ENTERPRISE/copilot/metrics/reports/enterprise-1-day?day=DAY"
```
Les URLs dans `download_links` sont signées et à durée limitée : elles expirent rapidement après leur génération. Votre workflow doit récupérer l’URL puis télécharger le fichier immédiatement, dans la même exécution.

La réponse peut ne contenir que `download_links` et `report_day`, mais voici le schéma complet possible :

```
{
  "type": "object",
  "title": "Copilot Metrics 1 Day Report",
  "description": "Links to download the Copilot usage metrics report for an enterprise/organization for a specific day.",
  "properties": {
    "download_links": {
      "type": "array",
      "items": {
        "type": "string",
        "format": "uri"
      },
      "description": "The URLs to download the Copilot usage metrics report for the enterprise/organization for the specified day."
    },
    "report_day": {
      "type": "string",
      "format": "date",
      "description": "The day of the report in YYYY-MM-DD format."
    }
  },
  "required": [
    "download_links",
    "report_day"
  ]
}
```
#### Rapport organisationnel sur 28 jours

Le rapport sur 28 jours met en évidence les schémas d’adoption et les évolutions à plus long terme. Les commandes sont quasi identiques, en pointant vers l’API 28 jours.

Exemple de requête :

```
curl -L \
 -H "Accept: application/vnd.github+json" \
 -H "Authorization: Bearer <YOUR_TOKEN>" \
-H “X-GitHub-Api-Version: 2026-03-10” \
https://api.github.com/enterprises/ENTERPRISE/copilot/metrics/reports/enterprise-28-day/latest
```
Vous obtiendrez une réponse similaire, avec toutefois `response_start_day` et `response_end_day`.

#### Structure des rapports organisationnels

Les rapports JSON sur 1 jour et 28 jours au niveau organisation peuvent ressembler à ceci :

```
[
  {
    "user_id": 1001,
    "user_login": "octocat",
    "day": "2026-05-14",
    "organization_id": "999",
    "team_id": 42,
    "slug": "frontend"
  },
  {
    "user_id": 1001,
    "user_login": "octocat",
    "day": "2026-05-14",
    "organization_id": "999",
    "team_id": 43,
    "slug": "backend"
  },
  {
    "user_id": 1002,
    "user_login": "hubot",
    "day": "2026-05-14",
    "organization_id": "999",
    "team_id": 42,
    "slug": "frontend"
  }
]
```
Vous obtenez ainsi une vue d’ensemble des utilisateurs d’une organisation, de leurs équipes et de leurs tags d’équipe.

### Endpoints au niveau utilisateur

Les rapports au niveau utilisateur offrent une visibilité plus granulaire de l’adoption. Vous pouvez comprendre, à un niveau élevé, comment chaque personne utilise Copilot.

Endpoints courants :

- 
`users-1-day`
- 
`users-28-day`
- 
`user-teams-1-day`

Ces rapports aident les administrateurs à identifier :

- Les utilisateurs très actifs
- Les équipes à faible adoption
- Les besoins de formation
- Les tendances d’usage par département

Ces requêtes ressemblent beaucoup aux rapports 1 jour et 28 jours au niveau organisation, en pointant simplement vers un autre endpoint.

#### Rapport utilisateur sur une journée

Exemple d’appel `users-1-day` :

```
curl -L \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2026-03-10" \
  "https://api.github.com/enterprises/ENTERPRISE/copilot/metrics/reports/users-1-day?day=DAY"
```
#### Rapport utilisateur sur 28 jours

Exemple d’appel `users-28-day` :

```
curl -L \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2026-03-10" \
   https://api.github.com/enterprises/ENTERPRISE/copilot/metrics/reports/users-28-day/latest
```
#### Rapport user-teams sur une journée

Un endpoint `user-teams-1-day` existe également et associe chaque utilisateur à ses équipes. Il ne contient pas de métriques d’usage : il sert de clé de jointure pour agréger des données par équipe.

#### Structure des rapports au niveau utilisateur

Le niveau de détail est bien plus élevé, puisqu’il s’agit de l’usage d’un utilisateur donné :

```
[{
  "code_acceptance_activity_count": 1,
  "code_generation_activity_count": 1,
  "day": "2025-10-01",
  "enterprise_id": "1",
  "loc_added_sum": 8,
  "loc_deleted_sum": 0,
  "loc_suggested_to_add_sum": 10,
  "loc_suggested_to_delete_sum": 0,
  "totals_by_cli": {
    "last_known_cli_version": {
      "cli_version": "1.0.8",
      "sampled_at": "2025-10-01T00:01:43.000Z"
    },
    "prompt_count": 2,
    "request_count": 2,
    "session_count": 2,
    "token_usage": {
      "avg_tokens_per_request": 4400.0,
      "output_tokens_sum": 5000,
      "prompt_tokens_sum": 3800
    }
  },
  "totals_by_feature": [{
    "code_acceptance_activity_count": 1,
    "code_generation_activity_count": 1,
    "feature": "code_completion",
    "loc_added_sum": 8,
    "loc_deleted_sum": 0,
    "loc_suggested_to_add_sum": 10,
    "loc_suggested_to_delete_sum": 0,
    "user_initiated_interaction_count": 0
  }],
  "totals_by_ide": [{
    "code_acceptance_activity_count": 1,
    "code_generation_activity_count": 1,
    "ide": "vscode",
    "last_known_ide_version": {
      "ide_version": "1.85.0",
      "sampled_at": "2025-10-01T00:00:02.000Z"
    },
    "last_known_plugin_version": {
      "plugin": "",
      "plugin_version": "",
      "sampled_at": "2025-10-01T00:00:02.000Z"
    },
    "loc_added_sum": 8,
    "loc_deleted_sum": 0,
    "loc_suggested_to_add_sum": 10,
    "loc_suggested_to_delete_sum": 0,
    "user_initiated_interaction_count": 0
  }],
  "totals_by_language_feature": [{
    "code_acceptance_activity_count": 1,
    "code_generation_activity_count": 1,
    "feature": "code_completion",
    "language": "unknown",
    "loc_added_sum": 8,
    "loc_deleted_sum": 0,
    "loc_suggested_to_add_sum": 10,
    "loc_suggested_to_delete_sum": 0
  }],
  "totals_by_language_model": [],
  "totals_by_model_feature": [],
  "used_agent": false,
  "used_chat": false,
  "used_cli": true,
  "user_id": 1,
  "user_login": "login1",
  "user_initiated_interaction_count": 0,
  "etl_id": "green",
  "day_partition": "2025-10-01",
  "entity_id_partition": 1
}]
```
Ces métriques sont surtout utiles comme signaux d’adoption au niveau équipe. Les taux d’acceptation et les volumes d’usage sont des indicateurs opérationnels, pas des mesures de qualité des développeurs.

Pour consulter l’ensemble potentiel des métriques exposées, reportez-vous à la documentation des données de métriques d’usage GitHub la plus à jour.

Les rapports au niveau utilisateur incluent les interactions CLI. Si vos équipes utilisent Copilot en ligne de commande, notre GitHub Copilot CLI Tutorial couvre la mise en place et les workflows courants.

## Mettre en place un workflow de reporting Copilot

Appeler l’API à la main est utile pour expérimenter et comprendre le schéma. Pour passer à l’action, mieux vaut automatiser.

Les équipes qui tirent le plus de valeur de Copilot Enterprise bâtissent généralement des pipelines légers de reporting qui croisent la télémétrie d’usage avec leurs métriques d’ingénierie internes.

### Les indicateurs clés pour démontrer le ROI

Toutes les métriques Copilot ne se valent pas. Les plus utiles incluent :

- Progression du nombre d’utilisateurs actifs
- Tendance des taux d’acceptation
- Code proposé vs code conservé
- Réduction des temps de cycle des PR
- Fréquence d’usage des IDE

GitHub a publié des benchmarks tels que :

- Achèvement des tâches 55 % plus rapide
- 88 % de code conservé

Ces chiffres indiquent des gains de productivité significatifs. Vos résultats varieront selon les équipes et workflows, d’où l’intérêt de l’API des métriques d’usage. Une équipe backend infrastructure n’utilise pas Copilot comme une équipe frontend de prototypage.

### Des données brutes à un dashboard équipe

Un workflow de reporting léger ressemble souvent à ceci :

1. Appel API planifié
2. Stockage des réponses dans une base ou un tableur
3. Transformation en tables de reporting
4. Visualisation dans votre plateforme de BI

La pile technique compte moins que la régularité.

Même un simple enchaînement de scripts Python planifiés et d’exports CSV peut offrir une visibilité opérationnelle utile.

Architecture type :

GitHub API

↓

Script Python planifié

↓

PostgreSQL / CSV / Tableur

↓

Power BI / Tableau / Looker

## Pour conclure

GitHub Copilot Enterprise consiste à préparer votre infrastructure pour un code « AI-ready ». Spaces apporte le contexte organisationnel qui rend Copilot plus pertinent dans des environnements d’ingénierie réels. L’API des métriques d’usage fournit la télémétrie nécessaire pour évaluer le succès de l’adoption.

Les organisations qui obtiennent les meilleurs résultats avec Copilot Enterprise ont des points communs :

- Elles soignent le contexte interne
- Elles suivent l’adoption en continu
- Elles prennent au sérieux la gouvernance de Copilot
- Elles mesurent les résultats plutôt que de supposer des gains de productivité

Cet état d’esprit compte bien plus que d’attribuer des licences.

Pour approfondir vos compétences Copilot et IA, nous vous recommandons le cours Software Development with GitHub Copilot ou le parcours de compétences AI for Software Engineering.

## FAQ GitHub Copilot

### Qu’est-ce que GitHub Copilot Spaces ?

**GitHub Copilot Spaces sont des collections sélectionnées de dépôts, de documentation, d’issues et d’autres contenus organisationnels qui ancrent les réponses de Copilot dans la connaissance spécifique à l’entreprise.**

### Qu’a remplacé GitHub Copilot Knowledge Bases ?

**GitHub a mis fin à Knowledge Bases le 1er novembre 2025. Spaces est le système de remplacement, avec un support de contenu plus large et de meilleurs contrôles de partage.**

### Que suit l’API des métriques d’usage de GitHub Copilot ?

**L’API suit les utilisateurs actifs, les suggestions de code, les taux d’acceptation, l’usage par langage, la télémétrie IDE et d’autres métriques d’adoption organisationnelle.**

### Quelles permissions sont requises pour l’API des métriques d’usage ?

**La plupart des endpoints de l’API des métriques d’usage requièrent des permissions telles que `manage_billing:copilot` ou `read:org`, selon le modèle d’authentification et l’endpoint utilisé.**

Je suis un data scientist avec de l'expérience dans l'analyse spatiale, l'apprentissage automatique et les pipelines de données. J'ai travaillé avec GCP, Hadoop, Hive, Snowflake, Airflow et d'autres processus d'ingénierie et de science des données.