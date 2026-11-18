Cours

Claude Code est puissant, mais sans configuration réutilisable, vous vous retrouvez à répéter les mêmes instructions encore et encore. Les modèles Claude Code résolvent ce problème : ils transforment des consignes récurrentes, des workflows, des autorisations d’outils et des intégrations en fichiers de projet réutilisables que Claude peut découvrir et appliquer.

Dans cet article, nous verrons ce que sont les modèles Claude Code, les principaux types disponibles, la manière dont chacun se comporte, comment choisir le bon type pour votre flux de travail, et où trouver des modèles prêts à l’emploi.

Cet article suppose que vous avez déjà une configuration Claude Code de base. Si vous débutez, commencez par ce tutoriel Claude Code avant d’aller plus loin avec les modèles. Si vous apprenez encore comment Claude Code s’intègre au développement en ligne de commande, consultez ce guide d’introduction à la Claude Code CLI.

## En bref

- 
Les modèles Claude Code sont des configurations réutilisables basées sur des fichiers (stockées dans `.claude/` ) qui vous évitent de réexpliquer votre stack et vos workflows à chaque session.
- 
Il en existe six types : skills (workflows répétables), agents (rôles et permissions cadrés), commandes (actions manuelles en slash), hooks (garde-fous automatiques), MCP (connexions à des outils et données externes) et plugins (regroupements des cinq précédents).
- 
`CLAUDE.md` contient toujours le brief de votre projet ; les modèles y ajoutent des comportements modulaires et réutilisables.
- 
Choisissez selon le déclencheur : les hooks appliquent des règles automatiquement, les agents apportent une expertise métier, les skills encodent des workflows répétables, les commandes s’exécutent à la demande, MCP accède à des systèmes externes, et les plugins emballent et partagent une configuration complète.
- 
Commencez petit avec la documentation officielle d’Anthropic, une collection communautaire comme aitmpl.com, ou vos propres fichiers, et créez un premier skill pour votre tâche la plus répétée avant d’étendre.

## Introduction aux agents d'intelligence artificielle

## Que sont les modèles Claude Code ?

Les modèles Claude Code sont des fichiers de configuration réutilisables qui personnalisent le comportement de Claude Code dans un projet ou à l’échelle de votre environnement local.

Point essentiel : les modèles sont basés sur des fichiers. Vous ne les installez pas via une interface de réglages classique pour cliquer sur des écrans de configuration. Claude Code détecte plutôt des fichiers et dossiers spécifiques, charge les métadonnées pertinentes dans le contexte et s’appuie dessus pour décider comment se comporter.

En pratique, il s’agit généralement de fichiers Markdown, JSON ou orientés shell, stockés dans des dossiers au niveau du projet comme `.claude/`, ou empaquetés dans des répertoires de type plugin pour le partage.

Une structure typique au niveau projet peut ressembler à ceci :

```
my-app/
├── CLAUDE.md
├── .mcp.json
└── .claude/
    ├── skills/
    │   └── database-migration/
    │       └── SKILL.md
    ├── agents/
    │   └── security-auditor.md
    ├── commands/
    │   └── summarize-pr.md
    └── settings.json
```
### Modèles Claude Code vs CLAUDE.md

`CLAUDE.md` reste important, mais son rôle est différent. Considérez `CLAUDE.md` comme le brief du projet : ce qu’est le projet, quelles commandes comptent, quels standards de code s’appliquent et quelles conventions d’architecture Claude doit garder en mémoire.

Pour un guide pas-à-pas, consultez notre guide de rédaction CLAUDE.md.

Les modèles sont plus modulaires :

- Un skill peut encoder un workflow de migration.
- Un agent peut isoler une posture de revue sécurité.
- Un hook peut s’exécuter après des éditions de fichiers.
- Une configuration MCP peut connecter Claude à GitHub, SQLite ou un autre système externe.

C’est aussi là que les modèles s’articulent avec la conception globale de votre workflow Claude Code. Des modèles solides donnent le meilleur résultat lorsqu’ils s’accompagnent de bonnes pratiques de planification, de test et de passation de contexte.

Retrouvez davantage de ces pratiques dans notre guide des bonnes pratiques.

Les commandes personnalisées relèvent également du système de skills, même si l’ancien format `.claude/commands/` fonctionne encore. Le nouveau format recommandé est `.claude/skills/<name>/SKILL.md`, qui prend en charge l’appel par commande slash et l’appel automatique par Claude.

## Quels types de modèles Claude Code puis-je utiliser ?

L’écosystème des modèles Claude Code se structure généralement en six catégories : skills, agents, commandes, hooks, intégrations MCP et plugins.

Les cinq premiers modifient directement le comportement de Claude. Les plugins sont un peu différents : il s’agit d’un format de distribution pouvant regrouper skills, agents, hooks, commandes, serveurs MCP et autres composants dans un package réutilisable.

Nous allons examiner chacune de ces catégories ci-dessous.

### 1. Skills

Les skills sont des ensembles d’instructions pour des tâches multi-étapes répétables. Un skill est généralement un dossier contenant un fichier `SKILL.md` avec un frontmatter YAML et un corps en Markdown.

Le frontmatter décrit ce que fait le skill et comment il doit se comporter ; le corps indique à Claude les étapes à suivre. Pour un décryptage dédié, consultez ce guide Claude Skills.

Claude utilise la description du skill pour décider quand il est pertinent. Par défaut, l’utilisateur comme Claude peuvent appeler un skill : vous pouvez taper `/skill-name`, ou Claude peut le charger automatiquement lorsque la tâche en cours correspond à sa description. Vous pouvez aussi désactiver l’appel automatique du modèle pour les workflows où vous souhaitez garder la main, comme le déploiement.

Voici un court exemple de fichier : `.claude/skills/database-migration/SKILL.md`

```
---
name: database-migration
description: Use when creating, reviewing, or modifying database migrations. Ensures migrations are reversible, tested, and checked before and after execution.
allowed-tools:
  - Read
  - Write
  - Bash
---
# Database Migration Skill
When working on a database migration:
1. Inspect the existing schema and migration history before writing changes.
2. Confirm whether the migration is additive, destructive, or data-transforming.
3. Create a reversible migration whenever the framework supports rollback.
4. Run the project’s migration check command before applying the migration.
5. Run tests that cover the affected models, queries, or API endpoints.
6. After writing the migration, summarize:
   - schema changes
   - rollback behavior
   - affected tables
   - test commands run
```
C’est utile car les instructions sont procédurales. Vous ne dites pas seulement à Claude de « faire attention aux migrations » ; vous lui fournissez une checklist répétable.

Les skills conviennent à tout ce que vous colleriez sinon dans Claude plus de deux fois : génération d’endpoints d’API, rédaction de journaux de modifications, échafaudage de tests, création de notes de version, revue de pull requests ou vérifications de migration.

Pour plus d’inspiration sur les workflows que les développeurs transforment en automatisations IA réutilisables, consultez notre liste Agent Skills.

### 2. Agents

Les agents, plus précisément des sous-agents personnalisés dans Claude Code, sont des assistants IA spécialisés avec leur propre définition Markdown, frontmatter YAML, restrictions d’outils, choix de modèle et prompt système.

Ils peuvent résider dans `.claude/agents/` pour une portée projet, ou `~/.claude/agents/` pour une portée personnelle. Les agents sont créés en les demandant à Claude ou en éditant directement les fichiers Markdown du dossier `.claude/agents/`.

Il existe une vraie différence entre skills et agents. Un skill définit comment exécuter une tâche. Un agent définit qui Claude doit être pendant le travail : son rôle, son focus, ses permissions et ses limites.

Voyons un exemple d’agent :

```
---
name: security-auditor
description: Reviews code for security vulnerabilities and produces a findings report without modifying files.
tools: Read, Glob, Grep, Bash
model: sonnet
---
You are a security auditor.
Your task is to inspect the codebase for vulnerabilities, risky patterns, and missing safeguards.
Rules:
- Do not edit files.
- Do not suggest broad rewrites unless directly tied to a security issue.
- Focus on authentication, authorization, input validation, secrets, dependency risk, and unsafe shell or SQL usage.
- Produce a findings report with severity, affected files, evidence, and recommended next steps.
```
Cet agent est utile car il fixe des limites claires. En session générale, Claude pourrait se mettre à corriger les problèmes dès qu’il en trouve. Un agent auditeur sécurité est instrué d’inspecter et de rapporter uniquement, sans modifications.

Les agents sont idéaux pour des domaines spécialisés comme l’audit sécurité, la revue documentaire, la revue d’architecture, l’ingénierie des données ou les contrôles de qualité du code, où l’isolement du contexte et des permissions est crucial.

Ils sont également efficaces en tandem avec des skills spécialisés. Par exemple, un agent auditeur sécurité peut appeler un skill de rapport de constats, tandis qu’un agent relecteur frontend peut utiliser un skill de test de composants.

### 3. Commandes

Les commandes sont des raccourcis invoqués en slash, comme `/generate-tests`, `/check-deps` ou `/summarize-pr`. Historiquement, les commandes personnalisées étaient stockées comme fichiers Markdown sous `.claude/commands/`, le nom du fichier servant de nom de commande.

Claude Code prend toujours en charge cet ancien format, mais nous recommandons d’utiliser des skills pour les nouveaux workflows de type commande : ils gèrent le même appel `/name` et peuvent être déclenchés automatiquement lorsque pertinent.

Les commandes sont préférables lorsque vous souhaitez un déclenchement explicite. Un skill peut se lancer automatiquement si Claude détecte une tâche correspondante, mais une commande ne doit s’exécuter que lorsque vous la lancez. Elles sont donc idéales comme points de contrôle : « générer les tests maintenant », « résumer cette PR maintenant », « vérifier les dépendances maintenant », « préparer un message de commit maintenant ».

### 4. Hooks

Les hooks sont des règles d’automatisation exécutées en réponse à des événements du cycle de vie de Claude Code. Ce sont des commandes shell définies par l’utilisateur, exécutées à des moments précis, qui offrent un contrôle déterministe du comportement.

La différence avec les autres modèles précédemment abordés est qu’ils ne sont pas déclenchés par ce que vous demandez, mais par ce que fait Claude.

Concrètement, vous n’avez pas à espérer que Claude pense à formater un fichier après l’avoir édité ; un hook peut le faire automatiquement.

Les événements de hook actuels incluent notamment `PreToolUse`, `PostToolUse`, `Notification` et `Stop`.

Exemple : lancer un formateur après que Claude a édité ou écrit un fichier :

```
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write"
          }
        ]
      }
    ]
  }
}
Example: block risky shell commands before Claude runs them:
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "python3 .claude/hooks/block-dangerous-bash.py"
          }
        ]
      }
    ]
  }
}
```
Les hooks sont idéaux pour les règles que Claude ne doit pas pouvoir contourner : exécuter un linter, formater les fichiers modifiés, bloquer l’édition de fichiers protégés, vérifier le code généré ou envoyer des notifications lorsque Claude a besoin d’entrées.

Pour un tutoriel approfondi, lisez notre guide des hooks Claude Code.

### 5. Intégrations MCP

Les intégrations MCP connectent Claude Code à des outils, sources de données et API externes via le Model Context Protocol. MCP joue le rôle de couche connecteur entre systèmes d’IA et outils externes. Dans Claude Code, cela permet à Claude d’aller au-delà des fichiers locaux et des commandes shell.

Claude peut ainsi interagir avec des services externes comme GitHub, des bases de données, des systèmes de documentation, des plateformes cloud ou des API internes, selon les serveurs MCP que vous configurez. Pour une explication complète et un projet de démo, consultez notre tutoriel Model Context Protocol.

Un serveur MCP peut exposer trois grands types de capacités :

- **Tools** : fonctions exécutables appelables par Claude, comme créer un ticket GitHub ou exécuter une requête de base de données.
- **Resources** : sources de contexte en lecture seule, comme un fichier, une ligne de base ou un document.
- **Prompts** : modèles de tâches réutilisables exposés par le serveur.

Un `.mcp.json` au niveau projet peut configurer plusieurs serveurs côte à côte :

```
{
  "mcpServers": {
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "sqlite": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-sqlite",
        "./data/app.db"
      ]
    }
  }
}
```
C’est important car Claude ne peut raisonner qu’à partir du contexte et des outils auxquels il a accès. Sans MCP, il peut inspecter des fichiers locaux, mais pas votre gestionnaire de tickets, votre base de données, votre environnement cloud ou vos API internes.

MCP est idéal lorsque Claude doit travailler avec votre stack réelle plutôt qu’un instantané de code statique, et lorsqu’il a besoin d’accès à des données externes.

### 6. Plugins

Les plugins sont des bundles empaquetés. Ils peuvent inclure des skills, des agents, des hooks, des configurations MCP, des commandes et d’autres composants au sein d’une seule structure installable.

Dans Claude Code, un plugin comprend généralement un manifest `.claude-plugin/plugin.json` et des dossiers de composants tels que `skills/`, `agents/`, `hooks/` et `.mcp.json` à la racine du plugin.

Exemple de structure de plugin :

```
frontend-workflow-plugin/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   └── component-test/
│       └── SKILL.md
├── agents/
│   └── frontend-reviewer.md
├── hooks/
│   └── hooks.json
└── .mcp.json
```
Exemple de plugin json :

```
{
  "name": "frontend-workflow",
  "displayName": "Frontend Workflow",
  "version": "1.0.0",
  "description": "Frontend development workflow with review agents, test skills, and formatting hooks",
  "author": {
    "name": "Your Team"
  }
}
```
Les plugins n’ajoutent pas un nouveau type de comportement ; ils rendent les autres types portables. Utilisez-les pour partager une configuration complète au sein d’une équipe, réutiliser le même workflow sur plusieurs projets, ou installer un bundle communautaire plutôt que de créer chaque fichier manuellement.

Pour en créer un de zéro, consultez le guide pas-à-pas des plugins Claude Code de DataCamp.

## Quel type de modèle choisir ?

On le comprend : tous ces types peuvent prêter à confusion. Ils modifient tous le comportement de Claude. La différence principale tient à leur mode de déclenchement et au degré de contrôle qu’ils apportent.

Pour clarifier, voici une comparaison entre eux :

| **Type de modèle** | **Déclenché par** | **Idéal pour** | **Peu adapté à** | **Cas d’usage** | 
| Skill | Claude automatiquement ou utilisateur via `/skill-name` | Workflows multi-étapes répétables | Tâches ponctuelles | Appliquer automatiquement une checklist de migration quand Claude modifie des fichiers de schéma | 
| Agent | Demande utilisateur ou délégation par Claude | Expertise métier et isolement des permissions | Sessions généralistes | Un auditeur sécurité qui peut lire les fichiers mais ne doit pas les éditer | 
| Commande | Commande slash utilisateur | Actions à la demande et points de contrôle | Garde-fous automatiques | /generate-tests quand vous êtes prêt à tester | 
| Hook | Événement du cycle de vie de Claude | Garde-fous et contrôles qualité automatisés | Tâches nécessitant un raisonnement interactif | Formater les fichiers après chaque édition | 
| MCP | Appel d’outil par Claude | Accès à des systèmes externes | Workflows simples limités au local | Interroger PostgreSQL ou créer un ticket GitHub | 
| Plugin | Installation ou activation | Diffusion équipe et workflows groupés | Ajustements locaux à but unique | Un bundle frontend avec agents, skills et hooks | 

Une règle simple pour décider :

- Si vous souhaitez **imposer automatiquement une règle** chaque fois que Claude touche au code, utilisez un**hook** .
- Si vous voulez que Claude **adopte une expertise métier poussée** pour une tâche précise, utilisez un**agent** .
- Si vous voulez **encoder un workflow** que Claude doit répéter fidèlement, utilisez un**skill** .
- Si vous voulez **déclencher vous-même une action** au bon moment, utilisez une**commande** ou un skill de type commande.
- Si Claude a besoin de **services externes ou de données en direct** , utilisez**MCP** .
- Si vous souhaitez installer ou partager une **configuration de workflow complète** , utilisez un**plugin** .

En pratique, ces types se combinent souvent. Un plugin sécurité peut regrouper un agent `security-auditor`, un skill `audit-findings`, une commande de contrôle des dépendances et un hook pré-commit. L’agent définit le rôle, le skill définit la structure du rapport, la commande offre un point de contrôle explicite, et le hook applique le garde-fou.

Pour les workflows où Claude doit suivre un plan formel avant mise en œuvre, le spéc-driven development est souvent plus adapté que des prompts au fil de l’eau.

## Où trouver des modèles Claude Code ?

Trois sources pratiques : Anthropic, les collections communautaires et vos propres créations.

Commencez par les ressources officielles d’Anthropic et la documentation. La documentation Claude Code d’Anthropic couvre les skills, sous-agents, hooks, MCP et plugins ; c’est l’endroit idéal pour vérifier les formats de fichiers et comportements à jour avant toute mise en production.

Ensuite, utilisez les collections communautaires. Le hub le plus visible est aitmpl.com, qui se présente comme un catalogue de configurations prêtes à l’emploi pour les projets Claude Code. Sa navigation propose actuellement Skills, Agents, Commands, Settings, Hooks, MCPs et Plugins.

La commande d’installation interactive actuelle est :

`npx claude-code-templates@latest`
La documentation du projet présente aussi un alias plus court :

`npx cct@latest`
Pour des composants spécifiques, le README GitHub en ligne propose des commandes d’installation comme :

```
npx claude-code-templates@latest --agent development-tools/code-reviewer --yes
npx claude-code-templates@latest --command performance/optimize-bundle --yes
npx claude-code-templates@latest --hook git/pre-commit-validation --yes
npx claude-code-templates@latest --mcp database/postgresql-integration --yes
```
Vous pouvez aussi installer un stack complet en lot via plusieurs options dans une seule commande.

En évaluant des modèles communautaires, vérifiez quelques signaux de qualité :

- 
La `description` est-elle assez précise pour que Claude déclenche correctement le skill ou l’agent ?
- 
Les `allowed-tools` sont-elles bien circonscrites, ou le modèle demande-t-il inutilement de larges permissions d’écriture et de bash ?
- 
Le dépôt est-il entretenu récemment ?
- 
Le modèle explique-t-il ce qu’il modifie ?
- 
Inclut-il des hooks ou des serveurs MCP qui exécutent du code que vous n’avez pas audité ?

Enfin, rédigez les vôtres. C’est souvent la meilleure option pour des workflows très liés à votre stack. Un modèle communautaire offre une bonne base, mais il ne connaît pas votre politique de migration interne, vos conventions de nommage, votre modèle de données ni votre tolérance au risque de déploiement.

## Pour conclure

Les modèles Claude Code permettent de passer d’un assistant à la session à un environnement de développement persistant.

Les six catégories présentées s’empilent en couches : les skills encodent les workflows, les agents définissent les rôles, les commandes créent des actions explicites, les hooks imposent des garde-fous, MCP connecte des systèmes externes et les plugins emballent le tout pour la réutilisation.

Le meilleur point de départ n’est pas un énorme bundle de plugins. Commencez par un skill pour votre workflow le plus répétitif. Dès que vous identifiez les frictions restantes dans le comportement par défaut de Claude, ajoutez un agent pour une revue spécialisée, un hook pour l’application automatique, ou un serveur MCP pour un accès en direct aux systèmes.

Pour aller plus loin avec Claude Code, découvrez nos cours Claude Code 101 et Claude Code in Action.

## FAQ sur les modèles Claude Code

### Les modèles Claude Code sont-ils identiques à CLAUDE.md ?

**Non. CLAUDE.md sert surtout à des consignes générales au niveau du projet : stack technique, conventions de code, structure du projet et commandes préférées. Les modèles Claude Code sont plus modulaires. Ils emballent des workflows, rôles, commandes, hooks ou intégrations que Claude peut utiliser au besoin.**

### Dois-je utiliser un skill ou un agent ?

**Utilisez un skill lorsque vous voulez que Claude suive un processus répétable, par exemple générer des tests, écrire des changelogs ou revoir des migrations. Utilisez un agent lorsque vous voulez que Claude adopte un rôle spécifique, par exemple auditeur sécurité, relecteur de documentation ou architecte frontend. Dans de nombreux workflows, vous pouvez employer les deux de concert.**

### Les modèles Claude Code sont-ils propres au projet ou globaux ?

**Les deux sont possibles, selon l’emplacement de stockage. Les modèles spécifiques à un projet se trouvent généralement dans le répertoire `.claude/` du projet. Des modèles globaux sont utiles si vous souhaitez le même comportement sur plusieurs projets.**

### Les modèles Claude Code communautaires sont-ils sûrs à installer ?

**Pas automatiquement. Les modèles communautaires peuvent être très utiles, mais ils peuvent inclure des permissions d’outils, des commandes shell, des hooks ou des configurations MCP ayant un impact sur votre environnement local.**

### Quel est le meilleur type de modèle pour démarrer ?

**Commencez par un skill. C’est généralement la façon la plus simple de transformer des consignes répétées en workflows réutilisables, sans complexifier votre setup. Une fois un premier skill utile en place, vous pourrez ajouter des agents, des hooks, MCP et des plugins.**

Je m'appelle Austin, je suis blogueur et rédacteur technique et j'ai des années d'expérience en tant que data scientist et data analyst dans le domaine de la santé. J'ai commencé mon parcours technologique avec une formation en biologie et j'aide maintenant les autres à faire la même transition grâce à mon blog technologique. Ma passion pour la technologie m'a conduit à écrire pour des dizaines d'entreprises SaaS, inspirant les autres et partageant mes expériences.