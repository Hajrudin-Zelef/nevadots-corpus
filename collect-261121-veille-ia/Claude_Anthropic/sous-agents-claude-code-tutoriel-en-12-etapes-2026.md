---
id: collect-261121-veille-ia/claude-anthropic/sous-agents-claude-code-tutoriel-en-12-etapes-2026
title: "Sous-Agents Claude Code : Tutoriel en 12 Étapes [2026]"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-14"]
keywords: ["claude", "code", "sous-agents", "tutoriel", "sous-agent", "etapes", "projet", "chaque"]
source: https://tech-insider.org/fr/sous-agents-claude-code-2026/
source_anchor: ""
source_lines: [1, 465]
section: "Claude_Anthropic"
---
Claude Code sait déjà lire un dépôt entier, corriger un bug ou lancer une suite de tests. Mais sur un projet volumineux, chaque recherche de code, chaque journal de build et chaque page de documentation consultée s’accumule dans la même fenêtre de contexte que votre conversation principale. Au bout de quelques échanges, elle déborde de détails que vous ne relirez jamais. Les sous-agents corrigent ce défaut d’architecture : avec la version 2.1.271 de Claude Code, sortie le 14 septembre 2026 selon AI-TLDR, chaque sous-agent tourne toujours dans sa propre fenêtre de contexte, avec son propre prompt système et ses propres outils. Ce tutoriel montre, en 12 étapes, comment créer, configurer et déployer des sous-agents Claude Code personnalisés, avec la syntaxe exacte, les pièges à éviter et un projet complet à copier tel quel.

## Sous-agents Claude Code : ce qui change dans votre façon de coder

Un sous-agent est un assistant IA spécialisé qui traite un type de tâche précis dans sa propre fenêtre de contexte, défini comme un fichier Markdown avec un en-tête YAML qui fixe sa configuration et ses permissions. Quand Claude reconnaît une tâche qui correspond à la description d’un sous-agent, il lui délègue le travail. Le sous-agent l’exécute de façon indépendante, avec son propre prompt système, ses propres outils et des permissions indépendantes qui isolent son accès aux outils de celui de toute autre instance de sous-agent, puis renvoie uniquement un résumé à la conversation principale. C’est la définition exacte que donne la documentation officielle d’Anthropic sur les sous-agents personnalisés, mise à jour en août 2026.

Anthropic met en avant cinq bénéfices concrets pour justifier l’usage des sous-agents Claude Code : préserver le contexte principal en gardant l’exploration hors de la conversation, imposer des restrictions d’outils strictes, réutiliser une configuration d’un projet à l’autre grâce aux sous-agents de niveau utilisateur, spécialiser le comportement avec un prompt système ciblé, et maîtriser les coûts en renvoyant certaines tâches vers des modèles plus rapides comme Haiku. Ce dernier point compte double pour une équipe qui facture ses heures de développement assisté par IA au coût réel de l’API.

Claude Code embarque déjà trois sous-agents génériques prêts à l’emploi : Explore pour la recherche en lecture seule, Plan pour la préparation en mode plan, et general-purpose pour les tâches complexes qui combinent recherche et modification. Ce tutoriel va plus loin. Il montre comment écrire des sous-agents adaptés à votre pile technique et à vos conventions d’équipe, avec des exemples réels tirés de la documentation Anthropic : un relecteur de code, un débogueur, un analyste de données et un agent de requêtes en lecture seule protégé par un hook.

## Prérequis : les versions et outils nécessaires

Avant de commencer, la documentation officielle est claire sur un seul point de départ obligatoire : avoir Claude Code installé et authentifié, dans un terminal ouvert sur un répertoire de projet (un dépôt Git n’est pas obligatoire, un dossier vide fonctionne aussi). Si Claude Code n’est pas encore installé sur votre machine, notre tutoriel d’installation de Claude Code couvre la procédure complète en 13 étapes. Le tableau ci-dessous résume ce qu’il vous faut avant d’attaquer l’étape 1.

| Élément | Exigence | Pourquoi c’est nécessaire | 
|---|---|---|
| Claude Code CLI | Installé et authentifié | Les sous-agents sont une fonctionnalité native du CLI, pas un module séparé | 
| Version du CLI | 2.1.198 ou plus récente recommandée | Plusieurs comportements cités dans ce guide (modèle hérité par Explore, exécution en arrière-plan par défaut) datent de cette version | 
| Node.js | 18 ou plus récent | Nécessaire si vos sous-agents lancent des serveurs MCP via npx, comme l’exemple Playwright plus bas | 
| Terminal | macOS, Linux, WSL ou Windows PowerShell | La syntaxe `claude mcp` et `claude –agent` fonctionne à l’identique sur les quatre | 
| Éditeur de texte | N’importe lequel | Les sous-agents sont de simples fichiers Markdown avec en-tête YAML | 
| Dépôt Git (optionnel) | Recommandé pour les sous-agents de projet | Permet de versionner `.claude/agents/` et de le partager avec l’équipe | 

Un point de vocabulaire à clarifier avant de continuer : jusqu’à la version 2.1.63 de Claude Code, l’outil qui exécute les sous-agents s’appelait Task. Il a été renommé Agent depuis, mais les anciennes références `Task(…)` dans les paramètres et les définitions de sous-agents continuent de fonctionner comme alias.

## Étape 1 : repérer les sous-agents déjà intégrés à Claude Code

Avant d’écrire le moindre fichier, il vaut mieux savoir ce que Claude Code propose déjà. Trois sous-agents génériques sont enregistrés par défaut dans toute session interactive. Explore est un agent rapide, en lecture seule, optimisé pour la recherche et l’analyse de code : les outils Write et Edit lui sont refusés. Depuis la version 2.1.198, Explore hérite du modèle de la conversation principale au lieu de tourner systématiquement sur Haiku, avec un plafond à Opus sur l’API Claude. Plan sert de sous-agent de recherche pendant le mode plan, également en lecture seule. General-purpose, enfin, reste invocable à tout moment via l’outil Agent pour les tâches complexes qui demandent à la fois de l’exploration et des modifications, avec accès à tous les outils disponibles pour un sous-agent. Ces trois sous-agents intégrés, Explore, Plan et general-purpose, sont livrés en standard avec Claude Code pour couvrir chacun un type de tâche ciblé, comme le confirme la documentation à jour d’août 2026.

Deux agents auxiliaires s’y ajoutent en coulisse : statusline-setup (modèle Sonnet), utilisé quand vous lancez la commande `/statusline`, et claude-code-guide (modèle Haiku), sollicité pour répondre à vos questions sur les fonctionnalités de Claude Code. Vous n’avez normalement pas à les invoquer directement.

Un détail utile pour le dépannage plus loin dans ce guide : Explore et Plan sont les deux seuls sous-agents à ignorer vos fichiers CLAUDE.md et l’état Git de la session parente, afin de garder la recherche rapide et économique. Tous les autres sous-agents, intégrés ou personnalisés, chargent ces deux éléments au démarrage. Si vous voulez bloquer un sous-agent intégré précis, ajoutez-le au tableau `deny` de vos permissions avec la syntaxe `Agent(Explore)`. Pour empêcher Claude de déléguer à n’importe quel sous-agent, refusez directement l’outil Agent.

## Étape 2 : créer votre premier sous-agent personnalisé

Un sous-agent Claude Code est un fichier Markdown avec un en-tête YAML. Deux méthodes permettent de le créer : demander à Claude de l’écrire, ou rédiger le fichier vous-même.

### Méthode A : demander à Claude d’écrire le fichier

Dans une session Claude Code, décrivez le sous-agent voulu et son emplacement :

```
Crée un sous-agent personnel code-improver dans ~/.claude/agents/ qui
scanne les fichiers et suggère des améliorations de lisibilité, de
performance et de bonnes pratiques. Il doit expliquer chaque problème,
montrer le code actuel et fournir une version améliorée. Rends-le en
lecture seule et fais-le tourner sur Sonnet.
```
Claude écrit alors le fichier avec un `name`, une `description`, une liste `tools`, un `model` et un prompt système. Le résultat ressemble à ceci :

```
---
name: code-improver
description: Scans files and suggests improvements for readability, performance, and best practices. Use after writing or modifying code.
tools: Read, Grep, Glob
model: sonnet
---
You are a code improvement specialist. For each issue you find, explain
the problem, show the current code, and provide an improved version.
```
Comme ce fichier vit dans `~/.claude/agents/`, le sous-agent devient disponible dans tous vos projets. Pour le limiter à un seul projet, déplacez-le dans le dossier `.claude/agents/` de ce projet.

Un changement de version mérite d’être signalé si vous suivez un ancien tutoriel : jusqu’à la version 2.1.197, la commande `/agents` ouvrait un assistant interactif avec un onglet Running listant les sous-agents actifs et un onglet Library pour créer, modifier et supprimer des définitions. Depuis la version 2.1.198, `/agents` n’ouvre plus cet assistant : elle affiche simplement un rappel d’éditer `.claude/agents/` directement ou de demander à Claude de le faire. Les emplacements de fichiers et les champs du frontmatter, eux, n’ont pas changé, seul l’assistant en terminal a été retiré.

### Méthode B : écrire le fichier à la main

Le même résultat s’obtient directement, sans passer par Claude :

```
---
name: code-reviewer
description: Reviews code for quality and best practices
tools: Read, Glob, Grep
model: sonnet
---
You are a code reviewer. When invoked, analyze the code and provide
specific, actionable feedback on quality, security, and best practices.
```
Testez ensuite votre sous-agent en lui déléguant explicitement une tâche : « Utilise l’agent code-improver pour suggérer des améliorations sur ce projet ». Si Claude ne trouve pas le nouveau sous-agent, redémarrez Claude Code. Ce cas se produit uniquement quand `~/.claude/agents/` n’existait pas encore au lancement de la session : un processus déjà démarré ne détecte pas un dossier `agents` fraîchement créé. En dehors de ce cas précis, Claude Code surveille `~/.claude/agents/` et `.claude/agents/` et applique une modification de fichier en quelques secondes, sans redémarrage.

## Étape 3 : maîtriser les champs du frontmatter YAML

Seuls deux champs sont obligatoires : `name` et `description`. Tous les autres sont optionnels et prennent une valeur par défaut raisonnable si vous les omettez. Voici la liste complète documentée par Anthropic.

| Champ | Obligatoire | Rôle | 
|---|---|---|
| name | Oui | Identifiant unique en minuscules avec traits d’union | 
| description | Oui | Indique à Claude quand déléguer à ce sous-agent | 
| tools | Non | Liste blanche d’outils autorisés, hérite de tout si omis | 
| disallowedTools | Non | Liste noire d’outils retirés du pool hérité ou spécifié | 
| model | Non | sonnet, opus, haiku, fable, un ID complet, ou inherit (défaut) | 
| permissionMode | Non | default, acceptEdits, auto, dontAsk, bypassPermissions ou plan | 
| maxTurns | Non | Nombre maximal de tours agentiques avant arrêt | 
| skills | Non | Skills à précharger intégralement dans le contexte au démarrage | 
| mcpServers | Non | Serveurs MCP disponibles pour ce sous-agent uniquement | 
| hooks | Non | Hooks de cycle de vie propres à ce sous-agent | 
| memory | Non | Portée de mémoire persistante : user, project ou local | 
| background | Non | Force l’exécution en arrière-plan si réglé sur true | 
| isolation | Non | worktree pour une copie Git isolée du dépôt | 
| color | Non | Couleur d’affichage dans la liste des tâches | 

Le corps du fichier, sous l’en-tête YAML, devient le prompt système du sous-agent. Un point souvent mal compris : un sous-agent ne reçoit que ce prompt, plus quelques détails d’environnement comme le répertoire de travail. Il ne reçoit pas le prompt système complet de Claude Code. Gardez donc vos instructions autonomes et explicites, sans supposer que le sous-agent connaît le contexte de votre conversation en cours.

Ce que le sous-agent charge réellement à son démarrage va plus loin que son seul prompt. Il reçoit aussi le message de délégation que Claude rédige pour résumer la tâche, toute la hiérarchie de vos fichiers CLAUDE.md (utilisateur, projet, CLAUDE.local.md), et un instantané de l’état Git pris au tout début de la session parente. À l’inverse, certains éléments de la conversation principale ne franchissent jamais cette frontière : votre style de sortie personnalisé, la mémoire automatique de la session principale, et la taille de fenêtre de contexte, qui dépend du modèle propre au sous-agent plutôt que de celui de la conversation qui l’a lancé. Depuis juillet 2026, Claude Opus 5 est devenu le modèle Opus par défaut de Claude Code et conserve la fenêtre de contexte d’un million de tokens, selon Anthropic : un sous-agent configuré sur ce modèle peut à lui seul ingérer bien plus de code, ce qui change la donne pour les workflows qui enchaînent plusieurs sous-agents volumineux.

## Étape 4 : choisir la bonne portée (scope) pour chaque sous-agent

L’emplacement du fichier détermine qui peut utiliser le sous-agent. Quand plusieurs sous-agents partagent le même nom, Claude Code applique celui de l’emplacement le plus prioritaire.

| Emplacement | Portée | Priorité | Création | 
|---|---|---|---|
| Paramètres managés | Toute l’organisation | 1 (la plus haute) | Déployé via les paramètres managés | 
| Option CLI –agents | Session en cours | 2 | JSON passé au lancement de Claude Code | 
| .claude/agents/ | Projet en cours | 3 | Demandé à Claude ou créé manuellement | 
| ~/.claude/agents/ | Tous vos projets | 4 | Demandé à Claude ou créé manuellement | 
| Dossier agents/ d’un plugin | Là où le plugin est actif | 5 (la plus basse) | Installé avec un plugin | 

Les sous-agents de projet conviennent à un travail spécifique à un dépôt : versionnez-les pour que toute l’équipe en profite. Claude Code les découvre en remontant depuis le répertoire de travail courant, donc chaque dossier `.claude/agents/` rencontré entre ce répertoire et la racine du dépôt est scanné. Les sous-agents utilisateur, eux, restent personnels mais actifs sur toutes vos machines et tous vos projets.

Gardez les valeurs `name` uniques dans toute l’arborescence d’un même dossier `.claude/agents/`, sous-dossiers compris. Si deux fichiers déclarent le même nom, Claude Code n’en charge qu’un seul, choisi par l’ordre de lecture du système de fichiers plutôt que par une règle documentée. La commande `/doctor` signale ces doublons et propose de renommer ou de supprimer tous les fichiers sauf un.

Pour une définition rapide qui ne survit qu’à la session, la syntaxe `–agents` accepte du JSON directement en ligne de commande, pratique pour tester ou automatiser un script : c’est la version CLI du même mécanisme que le SDK Agent expose via le paramètre `agents` des options de `query()`, qui permet de faire naître plusieurs sous-agents en une seule requête programmatique. Au total, le SDK reconnaît trois modes de création de sous-agents : programmatique via ce paramètre, fichier Markdown dans `.claude/agents/`, ou sous-agent intégré comme Explore ou Plan.

```
claude --agents '{
  "code-reviewer": {
    "description": "Expert code reviewer. Use proactively after code changes.",
    "prompt": "You are a senior code reviewer. Focus on code quality, security, and best practices.",
    "tools": ["Read", "Grep", "Glob", "Bash"],
    "model": "sonnet"
  },
  "debugger": {
    "description": "Debugging specialist for errors and test failures.",
    "prompt": "You are an expert debugger. Analyze errors, identify root causes, and provide fixes."
  }
}'
```
## Étape 5 : restreindre les outils et les permissions

Le champ `tools` fonctionne comme une liste blanche, `disallowedTools` comme une liste noire retirée du pool hérité. Si les deux sont présents, `disallowedTools` s’applique d’abord, puis `tools` est résolu contre ce qui reste. Un exemple qui n’autorise que la lecture et la recherche, sans aucun accès en écriture ni aux outils MCP :

```
---
name: safe-researcher
description: Research agent with restricted capabilities
tools: Read, Grep, Glob, Bash
---
```
Et l’inverse, qui hérite de tout sauf de l’écriture et de la modification de fichiers :

```
---
name: no-writes
description: Inherits the available tools except file writes
disallowedTools: Write, Edit
---
```
Ces deux champs acceptent aussi des motifs au niveau des serveurs MCP : `mcp__

Le champ `mcpServers` va plus loin : il donne à un sous-agent l’accès à des serveurs MCP absents de la conversation principale, ce qui évite que leurs descriptions d’outils consomment du contexte ailleurs que là où elles servent. Reprenons l’exemple officiel d’un testeur de navigateur basé sur Playwright, déjà couvert par notre tutoriel sur les serveurs MCP pour Claude Code :

```
---
name: browser-tester
description: Tests features in a real browser using Playwright
mcpServers:
  - playwright:
      type: stdio
      command: npx
      args: ["-y", "@playwright/mcp@latest"]
  - github
---
Use the Playwright tools to navigate, screenshot, and interact with pages.
```
Le serveur Playwright MCP de Microsoft nécessite Node.js 18 ou plus récent, puisqu’il se lance via `npx`. Notez la différence de syntaxe : une définition en ligne comme `playwright` reste isolée à ce sous-agent, tandis qu’une simple chaîne comme `github` réutilise une connexion déjà configurée au niveau de la session.

## Étape 6 : choisir le modèle IA et maîtriser les coûts

Le champ `model` accepte un alias (`sonnet`, `opus`, `haiku`, `fable`), un identifiant complet comme `claude-opus-4-8`, ou `inherit`, valeur par défaut si le champ est omis. C’est l’un des leviers de coût les plus concrets pour une équipe qui multiplie les sous-agents : depuis juillet 2026, Sonnet 5 est devenu le modèle par défaut des sièges Pro avec, lui aussi, une fenêtre de contexte d’un million de tokens selon Anthropic, tandis qu’un agent de recherche de fichiers simple peut continuer à tourner sur Haiku, nettement moins cher par token.

Claude Code résout le modèle effectif d’un sous-agent dans cet ordre de priorité, du plus fort au plus faible :

1. La variable d’environnement CLAUDE_CODE_SUBAGENT_MODEL, quand elle est réglée sur un alias ou un ID de modèle
2. Le paramètre model passé pour cette invocation précise
3. Le champ model défini dans le frontmatter du sous-agent
4. Le modèle de la conversation principale

Depuis la version 2.1.196, régler CLAUDE_CODE_SUBAGENT_MODEL sur `inherit` équivaut à ne pas la définir du tout : la résolution continue avec le paramètre d’invocation puis le frontmatter. Sur les versions antérieures, `inherit` forçait tous les sous-agents sur le modèle de la conversation principale et ignorait les deux autres sources. Si votre organisation restreint les modèles disponibles via une liste blanche, Claude Code ignore silencieusement une valeur qui pointe vers un modèle exclu et fait tourner le sous-agent sur le modèle hérité à la place.

Pour comparer les tarifs exacts entre Sonnet, Opus et les modèles concurrents au moment de dimensionner votre budget de sous-agents, notez que la tarification API de Sonnet 5 était fixée à 2 $ / 10 $ par million de tokens (entrée/sortie) jusqu’au 31 août 2026 selon Anthropic ; notre comparatif Claude Code vs Cursor 2026 détaille les écarts de coût par requête entre les deux outils.

## Étape 7 : ajouter des hooks pour un contrôle conditionnel

La liste `tools` reste binaire : un outil est autorisé ou non. Pour un contrôle plus fin, comme autoriser Bash mais bloquer uniquement les commandes SQL d’écriture, les hooks `PreToolUse` interceptent chaque appel d’outil avant son exécution. Voici l’exemple officiel d’un sous-agent `db-reader`, limité aux requêtes de lecture :

```
---
name: db-reader
description: Execute read-only database queries. Use when analyzing data or generating reports.
tools: Bash
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/validate-readonly-query.sh"
---
You are a database analyst with read-only access. Execute SELECT queries
to answer questions about the data.
```
Claude Code transmet l’entrée du hook en JSON via stdin. Le script lit ce JSON, extrait la commande Bash, et sort avec le code 2 pour bloquer une opération d’écriture :

```
#!/bin/bash
# ./scripts/validate-readonly-query.sh
INPUT=$(cat)
COMMAND=$(echo "$INPUT" | jq -r '.tool_input.command // empty')
if echo "$COMMAND" | grep -iE '\b(INSERT|UPDATE|DELETE|DROP|CREATE|ALTER|TRUNCATE)\b' > /dev/null; then
  echo "Blocked: Only SELECT queries are allowed" >&2
  exit 2
fi
exit 0
```
Rendez le script exécutable avec `chmod +x ./scripts/validate-readonly-query.sh` sur macOS et Linux. Sur Windows, écrivez le script en PowerShell et ajoutez `shell: powershell` à l’entrée du hook. Les événements les plus utiles pour un sous-agent sont `PreToolUse` (avant l’appel), `PostToolUse` (après l’appel) et `Stop`, converti automatiquement en `SubagentStop` quand l’agent tourne comme sous-agent plutôt que comme session principale.

Ces hooks peuvent aussi être déclarés au niveau du projet, dans `settings.json`, pour réagir au démarrage ou à l’arrêt de n’importe quel sous-agent via les événements `SubagentStart` et `SubagentStop`, avec un filtre par nom d’agent.

## Étape 8 : donner une mémoire persistante à vos sous-agents

Le champ `memory` attribue à un sous-agent un dossier qui survit d’une conversation à l’autre. L’agent s’en sert pour accumuler des connaissances : schémas de base de données récurrents, conventions de nommage, décisions d’architecture déjà expliquées une fois. Trois portées sont possibles.

- **user** — stocké dans ~/.claude/agent-memory/nom-de-l-agent/, pour un savoir valable sur tous vos projets
- **project** — stocké dans .claude/agent-memory/nom-de-l-agent/, partageable via le contrôle de version, portée recommandée par défaut
- **local** — stocké dans .claude/agent-memory-local/nom-de-l-agent/, propre à un projet mais volontairement exclu du dépôt

Une fois la mémoire activée, le prompt système du sous-agent inclut automatiquement les instructions de lecture et d’écriture de son dossier de mémoire, ainsi que les 200 premières lignes ou 25 Ko de son fichier MEMORY.md, selon la limite atteinte en premier. Les outils Read, Write et Edit sont alors activés automatiquement pour que le sous-agent gère lui-même ses fichiers de mémoire :

```
---
name: code-reviewer
description: Reviews code for quality and best practices
memory: user
---
You are a code reviewer. As you review code, update your agent memory with
patterns, conventions, and recurring issues you discover.
```
Demandez explicitement au sous-agent de consulter puis de mettre à jour sa mémoire : « Relis ce fichier de mémoire avant de commencer » en début de tâche, puis « Maintenant que tu as terminé, note ce que tu as appris dans ta mémoire » à la fin. Cette mémoire fait partie du système de mémoire automatique de Claude Code : si vous la désactivez globalement, le champ `memory` d’un sous-agent perd tout effet.

## Étape 9 : invoquer vos sous-agents (langage naturel, @-mention, –agent)

Trois méthodes existent, de la simple suggestion ponctuelle à l’usage par défaut de toute la session. En langage naturel, il suffit de nommer le sous-agent dans votre message, sans syntaxe particulière :

```
Utilise le sous-agent test-runner pour corriger les tests en échec
Demande au sous-agent code-reviewer de regarder mes changements récents
```
L’@-mention garantit qu’un sous-agent précis s’exécute, plutôt que de laisser Claude choisir : tapez `@` puis sélectionnez le sous-agent dans la liste, exactement comme pour mentionner un fichier. Votre message complet part quand même vers Claude, qui rédige le prompt de délégation à partir de ce que vous avez demandé : l’@-mention contrôle uniquement quel sous-agent est invoqué, pas le contenu du prompt qu’il reçoit.

Pour faire tourner toute une session comme un sous-agent donné, avec son prompt système, ses restrictions d’outils et son modèle, utilisez l’option `–agent` :

`claude --agent code-reviewer`
Pour en faire le comportement par défaut de tout un projet, ajoutez `agent` dans `.claude/settings.json` :

```
{
  "agent": "code-reviewer"
}
```
Le drapeau passé en ligne de commande prend le dessus si les deux sont présents en même temps. Ce mode fonctionne aussi bien avec les sous-agents intégrés qu’avec vos sous-agents personnalisés, et le choix persiste quand vous reprenez une session interrompue.

## Étape 10 : exécuter en arrière-plan et enchaîner plusieurs sous-agents

Depuis la version 2.1.198, les sous-agents tournent en arrière-plan par défaut : côté SDK, ce changement se traduit par un paramètre `run_in_background` qui, laissé vide, bascule désormais l’exécution du sous-agent en arrière-plan sans intervention explicite. Claude ne les fait passer au premier plan que lorsqu’il a besoin du résultat immédiatement pour continuer, sauf si le frontmatter impose ce comportement via le champ booléen `background: true`, qui force l’arrière-plan quelle que soit la décision de Claude. Un sous-agent en arrière-plan garde un jeu d’outils plus restreint qu’un sous-agent au premier plan, et toute invite de permission qu’il rencontre remonte dans votre session principale, avec le nom du sous-agent concerné. Approuvez pour le laisser continuer, ou appuyez sur Échap pour refuser cet appel précis sans arrêter le sous-agent.

Pour des recherches indépendantes, plusieurs sous-agents peuvent tourner simultanément :

```
Recherche les modules d'authentification, de base de données et d'API
en parallèle avec des sous-agents séparés
```
Chaque sous-agent explore sa zone indépendamment, puis Claude synthétise les résultats. Attention toutefois : quand plusieurs sous-agents renvoient chacun un résultat détaillé, la conversation principale peut se remplir plus vite que prévu. Pour un travail multi-étapes, vous pouvez aussi enchaîner les sous-agents en séquence, chacun transmettant le contexte pertinent au suivant :

```
Utilise le sous-agent code-reviewer pour trouver les problèmes de
performance, puis utilise le sous-agent optimizer pour les corriger
```
Par défaut, un sous-agent ne peut pas lui-même en faire naître d’autres : Claude Code lui retire l’outil Agent tant que l’imbrication n’est pas activée. Pour l’autoriser, réglez la variable CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH sur le nombre de niveaux souhaités, par exemple 2, dans `settings.json`. Deux limites protègent aussi la session : 200 sous-agents maximum au total par défaut (ajustable via CLAUDE_CODE_MAX_SUBAGENTS_PER_SESSION), et 20 sous-agents simultanés maximum par défaut (ajustable via CLAUDE_CODE_MAX_CONCURRENT_SUBAGENTS) — un plafond fixé par la version 2.1.217 de juillet 2026 selon Digital Applied, la même mise à jour qui a mis fin, depuis le 21 juillet 2026, à la possibilité par défaut pour un sous-agent d’en faire naître d’autres sans configuration explicite de la profondeur d’imbrication.

Un sous-agent terminé n’est pas forcément fini pour de bon. Demandez à Claude de continuer un travail déjà livré (« Continue cette revue de code et analyse maintenant la logique d’autorisation ») et Claude Code reprend le même sous-agent avec tout son historique de conversation, plutôt que de repartir de zéro. Cette reprise passe par l’outil SendMessage, avec l’identifiant ou le nom du sous-agent comme destinataire. Seuls les sous-agents intégrés Explore et Plan ne renvoient aucun identifiant réutilisable : ce sont des agents à usage unique, donc préférez general-purpose ou un sous-agent personnalisé dès que la tâche demande plusieurs allers-retours.

## Projet complet : une équipe de sous-agents pour la revue de code

Voici un projet fonctionnel qui assemble trois sous-agents complémentaires : un relecteur en lecture seule, un débogueur qui corrige, et un analyste de données pour les requêtes SQL. Créez la structure suivante à la racine de votre projet :

```
mon-projet/
├── .claude/
│   └── agents/
│       ├── code-reviewer.md
│       ├── debugger.md
│       └── data-scientist.md
└── (le reste de votre code)
```
### 1. Le relecteur, en lecture seule

Ce sous-agent ne modifie jamais rien : ses outils s’arrêtent à Read, Grep, Glob et Bash, ce qui suffit pour lancer `git diff` et lire les fichiers changés.

```
---
name: code-reviewer
description: Expert code review specialist. Proactively reviews code for quality, security, and maintainability. Use immediately after writing or modifying code.
tools: Read, Grep, Glob, Bash
model: inherit
---
You are a senior code reviewer ensuring high standards of code quality and security.
When invoked:
1. Run git diff to see recent changes
2. Focus on modified files
3. Begin review immediately
Review checklist:
- Code is clear and readable
- Functions and variables are well-named
- No duplicated code
- Proper error handling
- No exposed secrets or API keys
- Input validation implemented
- Good test coverage
- Performance considerations addressed
Provide feedback organized by priority:
- Critical issues (must fix)
- Warnings (should fix)
- Suggestions (consider improving)
```
### 2. Le débogueur, avec droit de correction

À la différence du relecteur, ce sous-agent inclut Edit, puisque corriger un bug suppose de modifier le code :

```
---
name: debugger
description: Debugging specialist for errors, test failures, and unexpected behavior. Use proactively when encountering any issues.
tools: Read, Edit, Bash, Grep, Glob
---
You are an expert debugger specializing in root cause analysis.
When invoked:
1. Capture error message and stack trace
2. Identify reproduction steps
3. Isolate the failure location
4. Implement minimal fix
5. Verify solution works
For each issue, provide:
- Root cause explanation
- Evidence supporting the diagnosis
- Specific code fix
- Testing approach
- Prevention recommendations
```
### 3. L’analyste de données, pour les requêtes SQL

Ce troisième sous-agent illustre un cas d’usage hors codage pur, avec un modèle fixé explicitement sur Sonnet plutôt que sur `inherit` :

```
---
name: data-scientist
description: Data analysis expert for SQL queries, BigQuery operations, and data insights. Use proactively for data analysis tasks and queries.
tools: Bash, Read, Write
model: sonnet
---
You are a data scientist specializing in SQL and BigQuery analysis.
When invoked:
1. Understand the data analysis requirement
2. Write efficient SQL queries
3. Use BigQuery command line tools (bq) when appropriate
4. Analyze and summarize results
5. Present findings clearly
Always ensure queries are efficient and cost-effective.
```
Une fois les trois fichiers en place, redémarrez Claude Code pour que le dossier `.claude/agents/` fraîchement créé soit détecté. Enchaînez ensuite naturellement : « Utilise le sous-agent code-reviewer sur mes changements récents, puis passe les problèmes trouvés au sous-agent debugger pour qu’il les corrige ». Committez le dossier `.claude/agents/` dans votre dépôt Git : toute l’équipe récupère la même configuration au prochain `git pull`.

## Les pièges les plus courants (et comment les éviter)

**Oublier qu’un nouveau dossier agents/ demande un redémarrage.** Claude Code surveille les modifications de fichiers dans un dossier `agents` déjà existant, mais ne détecte pas la création du dossier lui-même pendant qu’une session tourne. Si votre sous-agent flambant neuf reste invisible, redémarrez avant de chercher plus loin.

**Donner bypassPermissions à un sous-agent par confort.** Ce mode saute les invites de permission, y compris pour les écritures dans `.git`, `.claude`, `.vscode` ou `.husky`. Une définition de sous-agent partagée dans un dépôt public avec ce réglage devient un vecteur d’exécution silencieuse pour quiconque clone le projet.

**Dupliquer un nom de sous-agent sans le savoir.** Deux fichiers portant le même `name` dans la même arborescence `.claude/agents/` ne provoquent pas d’erreur visible : Claude Code en charge un seul, selon l’ordre de lecture du système de fichiers. Le résultat est un comportement qui semble aléatoire d’une session à l’autre.

**Confondre la restriction tools et le contrôle réellement nécessaire.** Retirer Bash de la liste `tools` empêche toute commande shell, y compris les requêtes en lecture seule légitimes. Quand seule une sous-catégorie de commandes doit être bloquée, comme les écritures SQL, un hook `PreToolUse` fait le travail que `tools` ne peut pas faire seul.

**Ignorer que les sous-agents de plugin n’acceptent pas hooks, mcpServers ni permissionMode.** Ces trois champs sont silencieusement ignorés pour un sous-agent distribué par un plugin, pour des raisons de sécurité. Si vous en avez besoin, copiez le fichier dans `.claude/agents/` ou `~/.claude/agents/`.

**Sous-estimer la consommation de contexte des sous-agents parallèles.** Faire tourner cinq sous-agents en parallèle qui renvoient chacun un résultat détaillé peut remplir la conversation principale presque aussi vite qu’un seul long échange sans délégation. Pour un travail qui a vraiment besoin de parallélisme soutenu, les équipes d’agents donnent à chaque travailleur son propre contexte indépendant : lancées en aperçu de recherche en février 2026, après la disponibilité générale des sous-agents en 2025, selon HatchWorks, elles ciblent justement ce cas où plusieurs sous-agents parallèles saturent la conversation principale.

**Répéter dans le prompt des règles déjà couvertes par CLAUDE.md.** Un sous-agent standard charge déjà toute la hiérarchie de vos fichiers CLAUDE.md au démarrage, donc dupliquer ces règles dans sa description ou son prompt système n’ajoute rien, sauf pour Explore et Plan, qui sautent volontairement CLAUDE.md pour rester rapides. Si une consigne doit absolument atteindre l’un de ces deux agents, elle doit être répétée dans le prompt de délégation que vous donnez à Claude, pas dans le fichier du sous-agent.

## Dépannage : les problèmes fréquents et leurs solutions

Voici les huit situations les plus fréquemment rencontrées en configurant des sous-agents Claude Code, avec la cause exacte et la correction documentée par Anthropic.

| Symptôme | Cause probable | Solution | 
|---|---|---|
| Le sous-agent n’apparaît pas après sa création | Le dossier agents/ vient d’être créé pendant que la session tournait | Redémarrez Claude Code pour que le nouveau dossier soit détecté | 
| « Agent would be spawned with zero tools » | Aucune entrée de la liste tools ne correspond à un outil réel | Vérifiez l’orthographe exacte de chaque nom d’outil dans le frontmatter | 
| « Subagent spawn limit reached » | Plus de 200 sous-agents ont été générés dans la session (limite par défaut) | Lancez /clear pour réinitialiser le compteur, ou augmentez CLAUDE_CODE_MAX_SUBAGENTS_PER_SESSION | 
| « Concurrent subagent limit reached » | 20 sous-agents tournent déjà en même temps (limite par défaut) | Attendez qu’un sous-agent se termine, ou augmentez CLAUDE_CODE_MAX_CONCURRENT_SUBAGENTS | 
| Deux sous-agents semblent entrer en conflit | Le même name est déclaré deux fois dans la même arborescence agents/ | Lancez /doctor pour repérer les doublons et renommer l’un des deux fichiers | 
| Le sous-agent tourne sur le mauvais modèle | CLAUDE_CODE_SUBAGENT_MODEL est définie et prend le dessus sur le frontmatter | Vérifiez la variable d’environnement en premier, elle a la priorité la plus haute | 
| La mémoire du sous-agent ne persiste pas | La mémoire automatique est désactivée globalement (autoMemoryEnabled ou la variable dédiée) | Réactivez la mémoire automatique, le champ memory dépend entièrement d’elle | 
| Un sous-agent repris perd son modèle personnalisé | Comportement des versions antérieures à 2.1.211 lors d’une reprise | Mettez à jour vers 2.1.211 ou plus récent, la valeur par invocation est alors conservée à la reprise | 

Pour retrouver l’identifiant exact d’un sous-agent et consulter son historique complet, les transcriptions se trouvent dans `~/.claude/projects/{project}/{sessionId}/subagents/`, chacune stockée sous la forme `agent-{agentId}.jsonl`. Ces fichiers persistent indépendamment de la conversation principale et sont nettoyés automatiquement après 30 jours par défaut.

## Astuces avancées : sécurité, coûts et bonnes pratiques

Anthropic recommande quatre principes pour concevoir des sous-agents qui restent utiles sur la durée : des sous-agents focalisés sur une seule tâche précise, des descriptions détaillées puisque Claude s’appuie dessus pour décider quand déléguer, un accès aux outils limité au strict nécessaire pour la sécurité et la concentration, et des sous-agents de projet versionnés pour que toute l’équipe les améliore collectivement.

### La sécurité des serveurs MCP à l’intérieur d’un sous-agent

Un sous-agent qui embarque un serveur MCP via `mcpServers` hérite du même risque que n’importe quelle connexion MCP. En juillet 2025, une vulnérabilité critique référencée CVE-2025-6514 a touché le paquet npm `mcp-remote`, utilisé pour relier des clients MCP locaux à des serveurs distants. Avec un score CVSS de 9,6, la faille permettait une injection de commandes système à distance quand le client se connectait à un serveur malveillant qui renvoyait une URL `authorization_endpoint` piégée, comme l’a détaillé The Hacker News en citant l’équipe de recherche JFrog à l’origine de la découverte. Le paquet, qui comptait plus de 437 000 téléchargements, a été corrigé en version 0.1.16. La leçon pour un sous-agent : ne connectez un serveur MCP distant que si sa provenance est fiable, et gardez vos dépendances à jour.

Plus largement, une analyse de sécurité 2025 relayée par Wikipedia sur le Model Context Protocol pointe des risques structurels dans l’écosystème MCP, dont l’injection de prompt et les « outils empoisonnés » capables d’exfiltrer des données via un outil connecté. Un sous-agent en lecture seule, sans Write ni Edit, réduit mécaniquement la surface d’attaque même si l’un de ses outils MCP se révèle compromis.

### Isolation Git pour les sous-agents qui modifient du code

Par défaut, un sous-agent démarre dans le répertoire de travail de la conversation principale, et une commande `cd` ne persiste pas d’un appel d’outil à l’autre. Pour donner à un sous-agent une copie isolée du dépôt plutôt qu’un accès direct à votre copie de travail, ajoutez `isolation: worktree` dans son frontmatter. Le worktree est nettoyé automatiquement si le sous-agent ne modifie rien, ce qui le rend pratique pour des sous-agents qui testent des correctifs en parallèle sans risquer d’écraser votre travail en cours.

Enfin, pensez à combiner `skills` et sous-agents plutôt que de choisir l’un ou l’autre par défaut. Un champ `skills` précharge le contenu complet d’un skill dans le contexte du sous-agent au démarrage, ce qui lui donne une connaissance métier ciblée (conventions d’API, patterns de gestion d’erreurs) sans qu’il ait à la découvrir lui-même pendant l’exécution.

## Questions fréquentes

**Un sous-agent Claude Code coûte-t-il plus cher qu’une conversation classique ?**

Pas nécessairement. Un sous-agent qui tourne sur Haiku coûte moins cher par token qu’une conversation principale sur Sonnet ou Opus. Le vrai risque de coût vient du nombre de sous-agents lancés en parallèle plutôt que du principe lui-même.

**Faut-il un dépôt Git pour utiliser des sous-agents de projet ?**

Non. Un dossier vide fonctionne. Un dépôt Git devient utile surtout pour versionner `.claude/agents/` et le partager avec une équipe.

**Quelle est la différence entre un sous-agent et un fork de conversation ?**

Un sous-agent démarre avec un contexte neuf et son propre prompt système. Un fork, lancé avec `/subtask`, hérite au contraire de tout l’historique de la conversation en cours, ce qui le rend utile pour une tâche annexe qui demanderait sinon trop de contexte à réexpliquer.

**Peut-on utiliser des sous-agents en dehors du terminal Claude Code ?**

Oui. Les définitions de sous-agents restent valables via le SDK Agent, ce qui permet de lancer Claude Code de façon programmatique dans un pipeline CI/CD, en dehors de toute session interactive.

**Un sous-agent personnalisé peut-il remplacer Explore ou Plan ?**

Oui, en le nommant explicitement `Explore` ou `Plan` dans un sous-agent de projet ou d’utilisateur : cette définition prend le dessus sur le sous-agent intégré du même nom, en conservant tout de même son propre champ `model`.

**Comment partager mes sous-agents avec toute mon équipe sans passer par –add-dir ?**

Deux options : les placer dans `.claude/agents/` et les committer dans le dépôt, ou les distribuer via un plugin, qui les charge automatiquement pour tous les utilisateurs de ce plugin.

**Que se passe-t-il si mon organisation restreint les modèles disponibles ?**

Claude Code vérifie la variable d’environnement, le paramètre par invocation et la valeur du frontmatter contre la liste blanche availableModels de l’organisation. Une valeur qui pointe vers un modèle exclu est ignorée, et le sous-agent tourne sur le modèle hérité à la place, sans erreur bloquante.

**Les sous-agents fonctionnent-ils de la même façon sur Windows ?**

La syntaxe `claude mcp`, `claude –agent` et les frontmatters YAML sont identiques sur macOS, Linux, WSL et PowerShell. Seuls les scripts de hooks changent : écrivez-les en PowerShell et ajoutez `shell: powershell` à l’entrée du hook sur Windows.

**Peut-on reprendre un sous-agent après qu’il a terminé sa tâche ?**

Oui, tant qu’il ne s’agit pas d’Explore ou de Plan. Demandez simplement à Claude de continuer le travail : le sous-agent reprend avec son historique complet, y compris tous les appels d’outils et résultats précédents, au lieu de repartir d’un contexte vide.
