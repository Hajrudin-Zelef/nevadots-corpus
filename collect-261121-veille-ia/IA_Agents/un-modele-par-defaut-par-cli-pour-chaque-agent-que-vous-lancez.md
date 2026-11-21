---
id: collect-261121-veille-ia/ia-agents/un-modele-par-defaut-par-cli-pour-chaque-agent-que-vous-lancez
title: "Un modèle par défaut par CLI, pour chaque agent que vous lancez"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["defaut", "modele", "model", "chaque", "agent", "claude", "vous", "opus"]
source: https://agentsroom.dev/features/default-model
source_anchor: ""
source_lines: [1, 118]
section: "IA_Agents"
---
# Choisissez le modèle une fois,

chaque agent de ce CLI démarre dessus

Un agent laissé sur Par défaut tourne sur ce que son CLI choisit : Opus 5.5 sur la plupart des forfaits Claude, Gemini 3.8 Flash sur Antigravity CLI. Choisissez un modèle par défaut par CLI dans Réglages > Providers IA, et chaque agent, ticket et étape d'équipe qui n'a pas de modèle à lui se lance dessus.

Un projet peut avoir son propre défaut, un modèle que vous avez désactivé n'est jamais lancé, et un agent qui nomme un modèle le garde. Sans choix, rien ne change : le CLI garde son propre défaut.

Lu au lancement, du plus précis au choix du CLI.

Chaque CLI d'agent a un modèle par défaut, et ce n'est pas toujours celui que vous choisiriez. Claude Code démarre sur le défaut de votre type de compte, Opus 5.5 sur Pro, Max, Ultra, Team, Enterprise et l'API d'après la documentation d'Anthropic, lue le 8 novembre 2026. Antigravity CLI lancé sans --model tourne sur Gemini 3.8 Flash (High), mesuré sur agy 1.3.5. Codex tourne sur le modèle de votre config.toml, ou sur son propre défaut intégré.

Sur un seul terminal, vous le changez dans la configuration du CLI et vous n'y pensez plus. Avec vingt agents, des tickets lancés depuis le board et des agents lancés par d'autres agents, chaque lancement laissé sur Par défaut prend ce modèle. C'est ainsi qu'est né ce réglage : les tickets Antigravity d'un utilisateur continuaient de tourner sur Gemini 3.8 Flash alors qu'il travaillait sur Gemini 4 Argon.

AgentsRoom ajoute un réglage par CLI, Modèle par défaut, lu au démarrage d'un agent. Il est enregistré avec votre compte, donc chaque ordinateur que vous utilisez lance le même modèle, et un projet peut le remplacer par le sien.

Une vraie session, filmée dans l'app : un agent laissé sur Par défaut démarre sur le modèle que choisit Claude Code, Opus 5.5. Dans Réglages > Providers IA, onglet Claude, Sonnet 5.5 est choisi sous Modèle par défaut, sans rien d'autre à enregistrer. L'agent suivant, lui aussi laissé sur Par défaut, démarre sur Sonnet 5.5, tandis que le premier garde Opus 5.5 : le réglage est lu au démarrage d'un agent.

## Comment régler le modèle par défaut dans chaque CLI

Sans AgentsRoom, dans la configuration du CLI lui-même. Vérifié le 8 novembre 2026 sur Claude Code 2.1.318 et codex-cli 0.167.0, d'après la documentation de chaque éditeur.

| CLI | Où vit le défaut | Pour une session | Bon à savoir | 
|---|---|---|---|
| Claude Code | `~/.claude/settings.json → "model": "opus"``.claude/settings.json``/model` | `claude --model opus``ANTHROPIC_MODEL=opus` | /model enregistre votre choix comme défaut des nouvelles sessions. Appuyez sur s dans son sélecteur pour changer seulement pour cette session. --model et ANTHROPIC_MODEL ne valent que pour la session qu'ils lancent. | 
| Codex | `~/.codex/config.toml → model = "<model id>"``.codex/config.toml` | `codex --model <model id>``-c model="<model id>"` | Les options de la ligne de commande passent avant le .codex/config.toml d'un projet (chargé seulement dans un projet de confiance), qui passe avant un fichier --profile et avant ~/.codex/config.toml. | 
| Antigravity CLI | `gemini-3.8-flash-high` | `agy --model <model id>` | Lancé sans --model, agy 1.3.5 tourne sur Gemini 3.8 Flash (High). Mesuré par AgentsRoom, pas documenté par Google. | 

## Ce que fait le réglage Modèle par défaut

Un réglage par CLI, lu au lancement, le même sur chaque ordinateur où vous êtes connecté.

### Un défaut par CLI

Réglages > Providers IA, l'onglet du CLI, Modèle par défaut. Claude Code sur Opus, Codex sur le modèle qui vous convient, Antigravity CLI sur Gemini 4 Argon : chaque CLI garde le sien.

### Un projet peut le remplacer

Paramètres du projet > Providers IA, sous la liste des modèles. Une entrée de projet ne peut que nommer un modèle : remettez-la sur Par défaut et le projet suit de nouveau votre compte.

### Chaque lancement laissé sur Par défaut

Les agents enregistrés, les tickets du backlog, les étapes d'équipe et les agents lancés par un autre agent qui ne portent pas de modèle à eux démarrent dessus. L'agent lui-même reste sur Par défaut : videz le réglage et il revient au choix du CLI.

### Désactiver des modèles

Modèles disponibles pour les agents, sur le même onglet : un modèle que vous désactivez disparaît de tous les sélecteurs de modèle et n'est jamais lancé. Un agent enregistré dessus démarre sur le premier modèle encore actif, et le retrouve dès que vous le réactivez.

### Le même sur chaque ordinateur

Les deux réglages suivent votre compte AgentsRoom. Un ticket lancé depuis le téléphone passe par votre ordinateur, qui les applique.

### Réglable par un agent

Un agent peut lire et changer les deux en MCP, avec settings_set sur defaultModels et hiddenModels, pour votre compte ou pour un seul projet.

## Comment le régler

Quatre étapes, une fois par CLI.

1. 1### Ouvrez Réglages > Providers IAChoisissez l'onglet du CLI : Claude, Codex, Antigravity, Copilot et les autres ont chacun le leur.
2. 2### Choisissez un Modèle par défautLe sélecteur liste les modèles de ce CLI, sauf ceux que vous avez désactivés. Laissez-le sur Par défaut pour garder le choix du CLI.
3. 3### Si un projet a besoin d'un autre modèleOuvrez Paramètres du projet > Providers IA et choisissez un modèle par défaut sous la liste des modèles : les agents de ce projet l'utilisent à la place.
4. 4### Lancez comme d'habitudeChaque agent, ticket ou étape d'équipe de ce CLI qui n'a pas de modèle à lui démarre dessus. Un agent en cours garde son modèle jusqu'à son prochain lancement.

## Ce qu'il ne fait pas

- Le défaut est lu au démarrage d'un agent. Il ne change jamais un agent en cours, et un /model tapé dans le terminal du CLI n'est ni bloqué ni filtré.
- Un agent, un ticket ou une étape d'équipe qui nomme un modèle le garde. Le défaut ne remplit que ceux laissés sur Par défaut.
- Si vos propres options de CLI pour ce provider portent déjà un drapeau de modèle, AgentsRoom n'en ajoute pas un second : votre option gagne.
- Laissé vide, c'est le CLI qui choisit. AgentsRoom ne connaît le défaut d'un CLI que là où il est stable et mesuré (Antigravity CLI) : pour Claude Code et Codex, il dépend de votre compte, donc réglez un Modèle par défaut si le choix du CLI est un modèle que vous avez désactivé.
- Le téléphone n'a pas encore d'écran pour le modifier, et son sélecteur de modèle affiche toujours Par défaut sans nommer le modèle. Les lancements depuis le téléphone l'appliquent bien, puisqu'ils passent par l'ordinateur.

## Comment régler le modèle par défaut dans Claude Code ?

## Comment changer le modèle par défaut de Codex CLI ?

## Qui gagne : le modèle de l'agent, le défaut du projet ou le défaut du compte ?

## Changer le modèle par défaut relance-t-il mes agents en cours ?

## Pourquoi mon agent Antigravity CLI démarre-t-il sur Gemini 3.8 Flash ?

## Puis-je empêcher les agents d'utiliser un modèle trop cher ?

## Le modèle par défaut est-il le même sur tous mes ordinateurs ?

## Est-ce la même chose qu'Adaptive Mode ?

## Comment AgentsRoom passe-t-il le modèle au CLI ?

## À utiliser avec

### Effort de raisonnement

Réglez l'effort de raisonnement une fois par CLI : chaque agent Claude Code, Codex, Kimi Code ou Grok Build démarre à ce niveau. Surchargez-le par projet, agent ou ticket.

### Adaptive Mode : un modèle par message

Adaptive Mode lit votre prompt et suggère le modèle IA le plus économique pour la tâche avant l'envoi. Des modèles légers comme Claude Haiku pour les petites corrections et les traductions, des modèles flagship comme Claude Opus pour les grosses refactos et l'architecture. Dépensez moins par tâche, faites plus de tâches par jour. Compatible Claude, Codex et Antigravity.

### Multi-Provider

AgentsRoom supporte 24 fournisseurs IA : Claude, Codex, GitHub Copilot CLI, OpenCode, Antigravity CLI, Aider, Grok Build, Mistral Vibe, Kimi Code, Amp, oh-my-pi, Freebuff, Devin, Cursor, Command Code, Muse Code, Pi, MiniMax Code, ZCode, Hermes Agent, Goose, Qwen Code, Kiro et Rovo Dev. Assignez un provider différent par agent, changez de provider en cours de conversation et conservez le contexte grâce au handoff automatique.

### Claude et Codex ensemble

Répartissez vos nouveaux agents entre Claude Code et Codex : un ticket sur deux, ou 10 % de la session Claude puis 10 % de la semaine Codex. Deux abonnements consommés au même rythme, pas l'un après l'autre. Les règles de quota font le reste.

## Un modèle par défaut par CLI

Téléchargez AgentsRoom, ouvrez Réglages > Providers IA, et choisissez le modèle sur lequel démarre chaque agent de ce CLI.

App companion : suivez vos agents en déplacement

Utilisez Claude, Codex, Antigravity CLI ou un autre fournisseur IA.

Remontez bugs et demandes directement dans votre backlog public.
