---
id: collect-261121-veille-ia/claude-anthropic/claude-skills-tutoriel-en-13-etapes-70-min-2026
title: "Claude Skills : Tutoriel en 13 Étapes, 70 Min [2026]"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2025-10-16"]
keywords: ["claude", "skills", "skill", "anthropic", "charge", "chaque", "conversation", "tutoriel"]
source: https://tech-insider.org/fr/claude-skills-tutoriel-2026/
source_anchor: ""
source_lines: [1, 369]
section: "Claude_Anthropic"
---
Un développeur qui colle la même checklist de déploiement dans Claude pour la dixième fois de la semaine a un problème d’organisation, pas un problème d’IA. Anthropic a résolu ce problème avec **Claude Skills** (ou Agent Skills), un système de dossiers que Claude charge automatiquement quand il en a besoin, au lieu de tout garder en mémoire à chaque conversation. Introduites le 16 octobre 2025 et publiées comme standard ouvert le 18 décembre 2025 sur agentskills.io, les Skills fonctionnent désormais sur claude.ai, Claude Code, l’API et le Claude Agent SDK. Ce tutoriel montre comment en créer une de zéro, la tester, la sécuriser et la déployer en 13 étapes, avec un projet complet à la fin que vous pourrez copier tel quel.

## Qu’est-ce que Claude Skills ? Définition, lancement et standard ouvert

Une Claude Skill est un dossier contenant un fichier `SKILL.md`, accompagné en option de scripts et de ressources annexes. Dans son billet d’ingénierie de lancement, Anthropic les décrit comme des “dossiers organisés d’instructions, de scripts et de ressources que les agents peuvent découvrir et charger dynamiquement” pour mieux exécuter une tâche précise. L’analogie que l’entreprise utilise dans son billet d’ingénierie est parlante : une Skill ressemble au guide d’intégration qu’on écrirait pour un nouveau collègue, avec la procédure, les pièges connus et les bonnes pratiques du métier.

La mécanique qui rend tout ça utile s’appelle la divulgation progressive (*progressive disclosure*). Claude ne charge pas le contenu complet de chaque Skill installée à chaque tour de conversation, ce qui saturerait rapidement la fenêtre de contexte. Il charge trois niveaux de contenu à des moments différents : le nom et la description de la Skill sont toujours présents dans le prompt système (environ 100 tokens par Skill), le corps du `SKILL.md` ne se charge que lorsque la Skill se déclenche (moins de 5 000 tokens recommandés), et les fichiers annexes ou scripts ne se chargent que si Claude les référence explicitement. Ce découpage permet d’installer des dizaines de Skills sans pénaliser le contexte disponible pour le reste de la conversation.

Anthropic fournit quatre Skills prêtes à l’emploi pour la bureautique : PowerPoint (`pptx`), Excel (`xlsx`), Word (`docx`) et PDF (`pdf`), disponibles sur claude.ai, l’API, Claude Platform sur AWS et Microsoft Foundry. L’écosystème s’est étoffé rapidement depuis le lancement : Anthropic recensait déjà 17 Skills jugées prêtes pour la production en juin 2026 selon The Data Scientist, tandis que Collective Brain cataloguait de son côté 56 Skills validées réparties en 7 catégories à la même période. Le 18 décembre 2025, Anthropic a publié la spécification Agent Skills comme standard ouvert et portable entre outils IA. Plusieurs rapports parus fin 2025 et début 2026 indiquent qu’OpenAI a annoncé la prise en charge du format dans Codex peu après cette publication, ce qui en ferait l’un des rares formats d’extension IA à circuler entre éditeurs concurrents plutôt que de rester propriétaire.

## Claude Skills vs MCP vs Projects Claude : les différences clés

La confusion la plus fréquente concerne le rôle respectif des Skills et du Model Context Protocol (MCP), que nous avons déjà couvert en détail sur Tech Insider. Anthropic tranche clairement la question dans sa documentation : MCP fournit la connectivité sécurisée vers des outils et des données externes (une base Postgres, un dépôt GitHub, un espace Slack), tandis qu’une Skill fournit le savoir procédural pour utiliser correctement ces outils une fois connectés. Les deux systèmes sont complémentaires, pas concurrents : une Skill peut très bien contenir des instructions expliquant comment enchaîner plusieurs appels à un serveur MCP.

La différence avec les instructions personnalisées façon Projects ou un fichier CLAUDE.md est tout aussi nette. Un CLAUDE.md se charge intégralement au démarrage de chaque session, même si son contenu ne sert jamais dans la conversation en cours. Une Skill, elle, reste invisible (hormis son nom et sa description) jusqu’à ce que la tâche en cours corresponde réellement à son usage. C’est la raison pour laquelle la documentation de Claude Code recommande de transformer en Skill toute section d’un CLAUDE.md qui a fini par ressembler à une procédure de plusieurs étapes plutôt qu’à un simple fait sur le projet.

| Critère | Claude Skills | Serveur MCP | CLAUDE.md / Projects | 
|---|---|---|---|
| Rôle principal | Savoir procédural réutilisable | Connectivité sécurisée à des outils/données externes | Contexte permanent du projet | 
| Chargement | À la demande (divulgation progressive) | Outils déclarés au démarrage, exécutés à la demande | Intégral à chaque session | 
| Coût en contexte au repos | ~100 tokens (nom + description) | Dépend du nombre d’outils exposés | Taille totale du fichier | 
| Format | Dossier + SKILL.md | Serveur (process local ou distant) | Fichier Markdown unique | 
| Portabilité | Standard ouvert (agentskills.io) | Standard ouvert (Anthropic, 2024) | Propre à chaque outil | 
| Cas d’usage typique | Checklist de revue, format de rapport, convention d’équipe | Lire une base de données, interroger une API tierce | Stack technique, conventions de nommage | 

## Prérequis : versions, comptes et outils nécessaires

Avant de commencer, vérifiez que vous disposez de ce qui suit. La plupart des fonctionnalités avancées décrites plus loin (sous-agents, injection de contexte dynamique, champs `effort` et `background`) nécessitent une version relativement récente de Claude Code, alors autant partir sur une base à jour.

- **Claude Code** installé et fonctionnel (dernière version disponible via`npm install -g @anthropic-ai/claude-code` ou l’installeur natif) — vérifiez votre version avec`claude --version` ou la commande`/status` une fois dans une session.
- **Un compte Claude** sur un plan Pro, Max, Team ou Enterprise si vous comptez aussi utiliser des Skills personnalisées sur claude.ai (l’exécution de code doit être activée dans les paramètres).
- **Un accès à l’API Claude** (clé API valide) si vous voulez tester les Skills pré-construites (`pptx` ,`xlsx` ,`docx` ,`pdf` ) par ce canal.
- **Git** installé, pour l’exemple de Skill que nous allons construire (elle s’appuie sur`git diff` ).
- **Un terminal** et un éditeur de texte pour écrire du Markdown et du YAML.
- **Python 3** (optionnel) si vous voulez tester le script`package_skill.py` du dépôt officiel pour empaqueter une Skill destinée à claude.ai ou à l’API.

Aucune de ces briques n’est exotique : si vous avez déjà suivi notre tutoriel d’installation de Claude Code, vous avez déjà tout ce qu’il faut pour la partie locale de ce guide.

## Étape 1 : choisir l’emplacement de votre Skill

Première décision, et elle conditionne tout le reste : où stocker le dossier de la Skill. Claude Code reconnaît quatre emplacements, avec un ordre de priorité strict quand deux Skills portent le même nom : le niveau entreprise prime sur le personnel, qui prime lui-même sur le niveau projet. Le déploiement de Skills au niveau entreprise, qui permet à une organisation entière d’imposer ou de partager des Skills via ses paramètres managés, est disponible depuis le 18 décembre 2025, date à laquelle Anthropic l’a formalisé dans son guide officiel des Skills. Une Skill à n’importe lequel de ces niveaux peut aussi remplacer une Skill “bundled” (embarquée nativement dans Claude Code) qui porterait le même nom.

| Emplacement | Chemin | Portée | 
|---|---|---|
| Entreprise | Paramètres managés (settings.json d’organisation) | Tous les utilisateurs de l’organisation | 
| Personnel | `~/.claude/skills/<nom-skill>/SKILL.md` | Tous vos projets, sur votre machine | 
| Projet | `.claude/skills/<nom-skill>/SKILL.md` | Ce projet uniquement, partageable via Git | 
| Plugin | `<plugin>/skills/<nom-skill>/SKILL.md` | Partout où le plugin est activé | 

Pour ce tutoriel, nous allons commencer par une Skill personnelle : elle est immédiate à tester, disponible dans tous vos projets, et ne nécessite aucune coordination d’équipe. Une fois validée, rien n’empêche de la déplacer vers `.claude/skills/` à la racine d’un dépôt pour la partager avec le reste de l’équipe via un commit Git classique — c’est d’ailleurs la méthode recommandée pour diffuser une convention d’équipe sans passer par un plugin.

Un détail qui surprend souvent les débutants : les Skills se chargent aussi depuis des dossiers `.claude/skills/` imbriqués, en dessous du dossier où vous lancez Claude Code, jusqu’à la racine du dépôt. Dans un monorepo, un sous-dossier comme `apps/web/.claude/skills/` peut donc exposer des Skills propres à cette application, qui ne deviennent actives que lorsque Claude touche un fichier dans ce sous-dossier.

## Étape 2 : créer l’arborescence de dossiers

Le projet fil rouge de ce tutoriel est une Skill nommée `pr-description`, qui génère une description de pull request structurée à partir du diff Git en cours et d’un numéro de ticket. C’est un besoin réel, présent dans quasiment toutes les équipes, et il illustre bien l’injection de contexte dynamique et les arguments que nous verrons plus loin. Créez d’abord le dossier personnel :

```
mkdir -p ~/.claude/skills/pr-description
cd ~/.claude/skills/pr-description
```
Le nom du dossier n’est pas cosmétique : c’est lui qui détermine la commande que vous taperez pour invoquer la Skill manuellement. Un dossier `pr-description` donne la commande `/pr-description`. Le champ `name` du frontmatter, que nous verrons à l’étape suivante, ne sert qu’à l’affichage dans les listes de Skills pour une installation personnelle ou projet ; seule une Skill de plugin utilise ce champ pour construire le nom de commande final.

## Étape 3 : rédiger le frontmatter YAML de SKILL.md

Chaque `SKILL.md` commence par un bloc YAML entre deux lignes `---`. Deux champs sont couverts par la spécification officielle et recommandés dans tous les cas : `name` et `description`. Anthropic impose des contraintes strictes sur le champ `name` : 64 caractères maximum, uniquement des lettres minuscules, des chiffres et des tirets, aucune balise XML, et surtout aucun des mots réservés “anthropic” ou “claude”. Le champ `description` doit être non vide, ne pas dépasser 1024 caractères, et ne contenir aucune balise XML.

La documentation officielle de la plateforme Claude insiste sur ce point : la qualité de ce champ `description` détermine à elle seule si votre Skill sert à quelque chose. C’est le texte que Claude compare à chaque nouvelle demande pour décider s’il doit charger la Skill : il doit donc dire à la fois ce que fait la Skill et quand l’utiliser, pas seulement l’un des deux.

```
---
name: pr-description
description: Génère une description de pull request structurée (résumé, changements, tests, risques) à partir du diff Git en cours. Utiliser quand l'utilisateur demande une description de PR, un message de pull request, ou veut résumer ses changements avant de les pousser.
---
```
Claude Code accepte, en plus de ces deux champs, une liste étendue d’attributs optionnels qui ne font pas partie du standard portable mais permettent un contrôle beaucoup plus fin du comportement de la Skill. Le tableau ci-dessous résume les plus utiles ; nous en utiliserons plusieurs dans les étapes suivantes.

| Champ | Rôle | Valeur par défaut | 
|---|---|---|
| `when_to_use` | Contexte de déclenchement additionnel, ajouté à la description | Absent | 
| `argument-hint` | Indice affiché dans l’autocomplétion, ex. `[numero-ticket]` | Absent | 
| `arguments` | Noms d’arguments positionnels pour la substitution `$nom` | Absent | 
| `disable-model-invocation` | Empêche Claude de déclencher la Skill tout seul | `false` | 
| `user-invocable` | Masque la Skill du menu `/` si`false` | `true` | 
| `allowed-tools` | Outils pré-autorisés sans confirmation pendant l’invocation | Absent | 
| `context` | `fork` exécute la Skill dans un sous-agent isolé | Absent (contexte principal) | 
| `model` | Modèle à utiliser le temps de la Skill | Hérite de la session | 

Important à savoir si vous comptez un jour distribuer la même Skill sur claude.ai ou via l’API : ces deux surfaces, ainsi que le script officiel `package_skill.py` du dépôt anthropics/skills, n’acceptent que six champs de frontmatter — `name`, `description`, `license`, `compatibility`, `metadata` et `allowed-tools`. Tout champ hors de cette liste (comme `argument-hint` dans l’exemple ci-dessus) fait échouer l’empaquetage avec une erreur explicite plutôt que d’être simplement ignoré.

## Étape 4 : rédiger le corps d’instructions

Sous le frontmatter, le corps du `SKILL.md` est le contenu que Claude lit réellement une fois la Skill déclenchée (le “niveau 2” de la divulgation progressive, plafonné à environ 5 000 tokens selon la documentation officielle). C’est du Markdown standard : titres, listes, blocs de code. La règle d’or reste la concision, puisque ce contenu reste dans le contexte pour le reste de la conversation une fois chargé — chaque ligne a un coût récurrent en tokens.

```
## Instructions
À partir du diff fourni ci-dessous, rédige une description de pull
request avec ces sections :
1. **Résumé** — une phrase expliquant le changement
2. **Changements** — liste à puces des modifications significatives
3. **Tests** — comment le changement a été vérifié
4. **Risques** — effets de bord potentiels, à zéro si aucun
Si un numéro de ticket est fourni en argument, ajoute une ligne
"Closes #NUMERO" à la fin.
## Diff en cours
!`git diff HEAD`
```
La dernière ligne de cet exemple, `` !`git diff HEAD` ``, est ce qu’Anthropic appelle l’injection de contexte dynamique : nous la détaillons à l’étape 7. Gardez pour l’instant en tête la structure générale — instructions claires, puis données injectées — car c’est le patron que vous retrouverez dans la quasi-totalité des Skills utiles en développement logiciel.

## Étape 5 : tester votre premier Skill avec Claude Code

Enregistrez le fichier, placez-vous dans un dépôt Git contenant des modifications non commitées, puis lancez Claude Code avec la commande `claude`. Deux façons de vérifier que la Skill fonctionne : la laisser se déclencher toute seule, ou l’appeler explicitement.

- **Déclenchement automatique** : tapez une demande qui correspond à la description, par exemple “Rédige-moi une description de PR pour ces changements”.
- **Invocation directe** : tapez`/pr-description` dans le prompt.

Dans les deux cas, Claude doit répondre avec un résumé structuré de vos modifications réelles, pas un exemple générique. Si rien ne se déclenche automatiquement, le problème vient presque toujours de la `description` : reformulez-la pour qu’elle colle exactement aux mots qu’un utilisateur emploierait naturellement. Claude Code surveille en continu les dossiers de Skills : toute modification apportée à un `SKILL.md` existant est prise en compte immédiatement dans la session en cours, sans redémarrage. Ce n’est que la création d’un tout nouveau dossier de Skills de premier niveau qui exige de relancer Claude Code pour qu’il commence à le surveiller.

## Étape 6 : ajouter des fichiers de support (scripts et références)

Une Skill ne se limite pas à un seul fichier. C’est le “niveau 3” de la divulgation progressive : fichiers Markdown additionnels, scripts exécutables, gabarits, données de référence — rien de tout cela ne consomme de tokens tant que Claude ne l’ouvre pas explicitement. Pour notre Skill `pr-description`, ajoutons un script qui valide qu’un ticket existe avant de générer la description :

```
pr-description/
├── SKILL.md              (instructions principales)
├── FORMAT.md             (gabarit détaillé, chargé si besoin)
└── scripts/
    └── check_ticket.sh   (script exécuté, jamais chargé en contexte)
```
Deux mécaniques différentes cohabitent ici, et la distinction compte. Un fichier Markdown référencé (comme `FORMAT.md`) charge son contenu texte dans le contexte quand Claude le lit. Un script (comme `check_ticket.sh`) s’exécute via bash, et seul son résultat — “ticket #482 trouvé” ou un message d’erreur — entre dans le contexte, jamais le code du script lui-même. C’est ce qui rend les scripts nettement plus économes qu’une Skill qui demanderait à Claude de régénérer la même logique à chaque fois : Firecrawl citait en août 2026 l’exemple de la Skill Caveman Claude, qui réduit de 65 % en moyenne le nombre de tokens de sortie générés en forçant des réponses délibérément minimalistes. Référencez toujours ces fichiers annexes explicitement depuis le `SKILL.md` principal, sinon Claude n’a aucun moyen de savoir qu’ils existent :

```
## Ressources additionnelles
- Pour le gabarit détaillé, voir [FORMAT.md](FORMAT.md)
- Pour valider un numéro de ticket, exécute scripts/check_ticket.sh
```
## Étape 7 : injecter du contexte dynamique avec Bash

Revenons sur la ligne `` !`git diff HEAD` `` introduite à l’étape 4. La syntaxe `` !`commande` `` déclenche l’injection de contexte dynamique, une extension propre à Claude Code : la commande s’exécute avant que Claude ne lise le contenu de la Skill, et son résultat remplace la ligne dans le texte final. Concrètement, Claude ne voit jamais la commande brute — il voit directement la sortie de `git diff HEAD`, déjà intégrée aux instructions.

Cette technique évite un aller-retour inutile : sans elle, Claude devrait d’abord lire la Skill, comprendre qu’il doit exécuter `git diff`, lancer la commande lui-même, puis relire le résultat. Avec l’injection dynamique, les données arrivent déjà prêtes dans le même chargement. C’est particulièrement utile pour tout ce qui change à chaque exécution : diff Git, date du jour, statut d’un service, nombre de tests en échec.

```
## Contexte
Branche actuelle : !`git branch --show-current`
Fichiers modifiés : !`git diff --name-only HEAD`
Diff complet :
!`git diff HEAD`
```
Par défaut, ces commandes s’exécutent via bash. Si votre équipe travaille sous Windows sans Git Bash, le champ de frontmatter `shell: powershell` bascule l’exécution vers PowerShell pour les blocs `` !`commande` `` de cette Skill précise.

## Étape 8 : utiliser arguments et substitutions de variables

Notre Skill doit accepter un numéro de ticket en argument, par exemple via `/pr-description 482`. Claude Code propose plusieurs formes de substitution pour ce cas de figure, résumées dans le tableau suivant.

| Variable | Résultat | 
|---|---|
| `$ARGUMENTS` | Tous les arguments passés, sous forme de chaîne complète | 
| `$ARGUMENTS[N]` ou`$N` | Argument à l’index N (base 0), ex. `$0` pour le premier | 
| `$nom` | Argument nommé, déclaré via le champ `arguments` du frontmatter | 
| `${CLAUDE_SESSION_ID}` | Identifiant de la session en cours | 
| `${CLAUDE_SKILL_DIR}` | Dossier contenant le SKILL.md, utile pour référencer un script joint | 
| `${CLAUDE_PROJECT_DIR}` | Racine du projet, indépendamment de l’emplacement de la Skill | 

Pour donner un nom explicite à notre argument plutôt que d’utiliser l’indice `$0`, déclarez-le dans le frontmatter :

```
---
name: pr-description
description: Génère une description de pull request structurée à partir du diff Git. Utiliser quand l'utilisateur demande une description de PR ou veut résumer ses changements.
argument-hint: "[numero-ticket]"
arguments: [ticket]
---
Si un ticket est fourni ($ticket), ajoute une ligne "Closes #$ticket"
à la fin de la description.
```
Un piège à connaître : un indice sans argument correspondant (`$1` alors qu’un seul argument a été passé) reste affiché tel quel dans le texte, sans erreur. Un argument nommé sans valeur, lui, se réduit silencieusement à une chaîne vide. Testez toujours votre Skill avec et sans argument avant de la considérer prête.

## Étape 9 : contrôler qui peut invoquer le Skill

Toutes les Skills ne devraient pas se déclencher automatiquement. Un déploiement en production ou l’envoi d’un message Slack sont des actions à effet de bord : vous ne voulez pas que Claude décide seul de les lancer parce que le code “a l’air prêt”. Deux champs de frontmatter permettent de restreindre l’invocation.

| Frontmatter | Vous pouvez invoquer | Claude peut invoquer | 
|---|---|---|
| (par défaut) | Oui | Oui | 
| `disable-model-invocation: true` | Oui | Non | 
| `user-invocable: false` | Non | Oui | 

`disable-model-invocation: true` convient aux workflows à effet de bord comme un déploiement : seule une invocation manuelle via `/deploy` fonctionne, et si Claude tente de reproduire les étapes de son propre chef, Claude Code bloque l’appel et lui indique explicitement de ne pas improviser la même procédure autrement. À l’inverse, `user-invocable: false` convient à une Skill de connaissance de fond, comme la documentation d’un système legacy : ce n’est pas une action que l’utilisateur déclenche lui-même, mais une information que Claude doit connaître quand c’est pertinent.

## Étape 10 : exécuter un Skill dans un sous-agent isolé

Pour des Skills longues ou consommatrices de contexte (un audit de sécurité complet, un run de tests exhaustif), le champ `context: fork` exécute la Skill dans un sous-agent isolé plutôt que dans la conversation principale. Le sous-agent tourne en arrière-plan par défaut ; seul le résultat final revient enrichir le contexte principal, pas chaque étape intermédiaire.

```
---
name: full-pr-review
description: Effectue une revue complète d'une pull request (sécurité, style, tests, performance). Utiliser pour une revue approfondie avant fusion.
context: fork
agent: code-reviewer
background: true
---
Analyse la pull request en cours sous quatre angles : sécurité,
respect des conventions du projet, couverture de tests, et impact
sur les performances. Produis un rapport structuré par section.
```
Le champ `agent` précise quel type de sous-agent utiliser quand `context: fork` est actif. Si vous préférez attendre le résultat dans le tour de conversation qui a déclenché la Skill plutôt que de la laisser tourner en tâche de fond, passez `background: false` — utile en interactif, moins pertinent pour une Skill que vous invoquez puis oubliez le temps qu’elle travaille.

## Étape 11 : utiliser les Skills pré-construites via l’API

Les quatre Skills documentaires d’Anthropic (`pptx`, `xlsx`, `docx`, `pdf`) sont accessibles directement via l’API Claude, sans rien installer. Elles nécessitent l’outil d’exécution de code (*code execution tool*) comme conteneur d’exécution, ainsi qu’un en-tête bêta spécifique : `skills-2025-10-02`. Si vous envoyez ou récupérez des fichiers via la Files API dans le même appel, ajoutez également l’en-tête `files-api-2025-04-14`.

```
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: skills-2025-10-02,files-api-2025-04-14" \
  -H "content-type: application/json" \
  -d '{
    "model": "claude-opus-4-8",
    "max_tokens": 4096,
    "container": {
      "skills": [{"type": "pptx", "skill_id": "pptx"}]
    },
    "messages": [
      {"role": "user", "content": "Crée une présentation de 5 slides résumant ce rapport trimestriel."}
    ]
  }'
```
Sur ce canal, l’environnement d’exécution tourne dans un conteneur isolé, sans accès réseau et sans possibilité d’installer de nouveaux paquets à la volée : seules les dépendances préconfigurées par Anthropic sont disponibles. C’est une contrainte à garder en tête si vous comptez faire tourner une Skill personnalisée gourmande en dépendances externes via l’API plutôt que via Claude Code, où l’accès réseau est complet.

Pour créer et téléverser vos propres Skills sur ce canal, l’API expose des points de terminaison dédiés sous `/v1/skills`. Une fois téléversées, les Skills personnalisées de l’API sont partagées à l’échelle de tout l’espace de travail (workspace) : tous les membres y ont accès, contrairement à claude.ai où chaque Skill personnalisée reste individuelle à l’utilisateur qui l’a importée.

## Étape 12 : empaqueter et partager via un plugin ou Git

Deux méthodes de partage principales existent pour une équipe qui travaille dans Claude Code. La plus simple consiste à committer le dossier `.claude/skills/` à la racine du dépôt : chaque membre de l’équipe qui clone le projet et lance Claude Code récupère automatiquement les Skills, sans étape d’installation supplémentaire. C’est l’approche à privilégier pour tout ce qui est spécifique à un seul projet.

Pour une Skill destinée à plusieurs dépôts, la distribution via un plugin Claude Code est plus adaptée : elle permet de regrouper Skills, agents, hooks et serveurs MCP dans un seul paquet installable. Les Skills livrées par un plugin sont automatiquement préfixées par son nom (`mon-plugin:ma-skill`), ce qui évite tout conflit avec les Skills personnelles ou de projet des utilisateurs qui l’installent. À noter : ajouter un fichier `.claude-plugin/plugin.json` directement dans un dossier de Skill suffit à le faire charger comme un plugin à part entière, capable d’embarquer aussi des agents, des hooks et des serveurs MCP.

Pour une distribution en dehors de l’écosystème Claude Code (téléversement sur claude.ai ou via l’API), utilisez le script `package_skill.py` fourni dans le dépôt officiel anthropics/skills. Il valide que votre frontmatter respecte bien les six champs autorisés par la spécification portable avant de générer l’archive à téléverser.

## Étape 13 : publier sur claude.ai et gérer le cross-surface

Sur claude.ai, direction Paramètres puis la section Fonctionnalités (*Settings > Features*), où vous pouvez téléverser une Skill personnalisée sous forme d’archive ZIP, à condition d’être sur un plan Pro, Max, Team ou Enterprise avec l’exécution de code activée. L’enjeu dépasse le simple confort individuel : selon le Ramp AI Index publié en mai 2026, Claude devançait déjà ChatGPT en usage professionnel en avril 2026, avec 34,4 % de parts d’adoption en entreprise contre 32,3 % pour son concurrent, ce qui rend la question du partage propre des Skills d’autant plus stratégique pour les équipes. Gardez cependant un point capital à l’esprit : les trois surfaces (claude.ai, API, Claude Code) ne synchronisent jamais leurs Skills personnalisées entre elles.

- Une Skill téléversée sur claude.ai n’est pas automatiquement disponible via l’API.
- Une Skill téléversée via l’API n’apparaît pas sur claude.ai.
- Les Skills de Claude Code restent basées sur le système de fichiers local, complètement séparées des deux autres surfaces.

Si vous utilisez Cowork ou les sessions cloud de Claude Code (y compris les routines planifiées), sachez qu’elles ne lisent jamais le dossier `~/.claude/skills/` de votre machine locale : ces sessions chargent les Skills activées pour votre compte claude.ai au démarrage, plus les Skills de projet committées dans le dépôt cloné. Pour qu’une Skill personnelle fonctionne aussi dans une routine planifiée, il faut donc soit l’activer explicitement sur votre compte claude.ai, soit la committer dans `.claude/skills/` du dépôt, soit la livrer via un plugin déclaré dans le `.claude/settings.json` du dépôt.

## Projet complet : un Skill de description de PR prêt à l’emploi

Voici la version finale, complète et fonctionnelle de la Skill construite tout au long de ce tutoriel. Copiez-la telle quelle pour démarrer, puis adaptez les sections aux conventions de votre équipe.

### Structure des fichiers

```
~/.claude/skills/pr-description/
├── SKILL.md
├── FORMAT.md
└── scripts/
    └── check_ticket.sh
```
### Code complet

```
---
name: pr-description
description: Génère une description de pull request structurée (résumé, changements, tests, risques) à partir du diff Git en cours. Utiliser quand l'utilisateur demande une description de PR, un message de pull request, ou veut résumer ses changements avant de les pousser.
argument-hint: "[numero-ticket]"
arguments: [ticket]
allowed-tools: Bash(git diff:*) Bash(git branch:*)
---
## Instructions
À partir du diff ci-dessous, rédige une description de pull request
avec ces sections : Résumé (une phrase), Changements (liste à
puces), Tests (comment le changement a été vérifié), Risques (effets
de bord potentiels, "Aucun" si non applicable).
Si un ticket est fourni ($ticket), exécute
scripts/check_ticket.sh $ticket puis ajoute une ligne
"Closes #$ticket" à la fin si le script confirme que le ticket
existe.
Pour le gabarit détaillé attendu par l'équipe, voir [FORMAT.md](FORMAT.md).
## Contexte
Branche actuelle : !`git branch --show-current`
Fichiers modifiés : !`git diff --name-only HEAD`
Diff complet :
!`git diff HEAD`
```
Le champ `allowed-tools: Bash(git diff:*) Bash(git branch:*)` pré-autorise ces deux commandes précises pendant l’invocation de la Skill, pour éviter une confirmation manuelle à chaque test. L’autorisation s’efface automatiquement dès votre prochain message, elle ne persiste pas au-delà du tour en cours. Une fois ce fichier en place et testé selon la méthode de l’étape 5, la Skill est prête à être committée dans `.claude/skills/` à la racine d’un dépôt d’équipe.

## 5 pièges courants à éviter

- **Une description trop vague.** “Aide avec les PR” ne dit ni ce que fait la Skill ni quand l’invoquer. Claude ne la déclenchera jamais de façon fiable. Formulez toujours la description comme une réponse aux deux questions : que fait cette Skill, et à quel moment dois-je l’utiliser.
- **Un champ `name` qui viole les contraintes.** Majuscules, espaces, ou les mots réservés “claude”/”anthropic” font échouer la validation lors de l’empaquetage vers claude.ai ou l’API. Restez sur des minuscules, des chiffres et des tirets.
- **Confondre les emplacements de stockage.** Une Skill dans`~/.claude/skills/` n’est visible que sur votre machine. Beaucoup de développeurs découvrent trop tard qu’un collègue qui clone le même dépôt ne voit rien, faute d’avoir committé la Skill dans`.claude/skills/` du projet.
- **Un SKILL.md surchargé.** Tout mettre dans le corps principal plutôt que dans des fichiers de référence annexes annule l’intérêt de la divulgation progressive : chaque chargement de la Skill consomme alors inutilement du contexte. La recommandation officielle est de rester sous 500 lignes dans le fichier principal.
- **Faire confiance à une Skill dont l’origine est inconnue.** Une Skill malveillante peut orienter Claude vers des appels d’outils ou des commandes qui ne correspondent pas à son objectif affiché. N’installez que des Skills que vous avez écrites vous-même ou qui proviennent directement d’Anthropic.
- **Oublier `disable-model-invocation` sur une action à effet de bord.** Sans cette protection, rien n’empêche Claude de déclencher tout seul une Skill de déploiement parce que le contexte de la conversation “ressemblait” à une demande de mise en production.

## Sécurité : ce qu’Anthropic recommande

La documentation officielle est directe sur ce point : une Skill donne à Claude de nouvelles capacités via des instructions et du code, ce qui signifie qu’une Skill malveillante peut le pousser à exécuter des actions qui ne correspondent pas à l’objectif affiché. Cette vigilance est d’autant plus nécessaire que les sources tierces se multiplient : la collection GitHub d’Obviousworks, mise à jour le 30 mai 2026, agrège déjà des dizaines de Skills provenant à la fois d’Anthropic et de contributeurs communautaires, sans garantie d’audit homogène. Anthropic recommande de traiter l’installation d’une Skill exactement comme l’installation d’un logiciel, avec le même niveau de méfiance envers les sources inconnues.

- **N’utilisez que des sources fiables** : vos propres Skills, ou celles publiées directement par Anthropic.
- **Auditez systématiquement** le contenu complet d’une Skill externe — le SKILL.md, les scripts, les images et toute autre ressource jointe — à la recherche d’appels réseau, d’accès fichiers ou d’opérations qui ne correspondent pas à l’objectif annoncé.
- **Méfiez-vous particulièrement des Skills qui récupèrent du contenu depuis des URL externes** : ce contenu peut inclure des instructions malveillantes, et une dépendance externe pourtant fiable au départ peut changer de comportement dans le temps.
- **Gardez à l’esprit que les Skills ne sont pas couvertes par les accords de rétention zéro des données (ZDR)** : les définitions de Skills et les données d’exécution suivent la politique de rétention standard d’Anthropic, pas une politique zéro-rétention même si votre organisation en bénéficie par ailleurs.
- **Traitez toute intégration en production avec prudence renforcée** , en particulier lorsque la Skill a accès à des données sensibles ou à des opérations critiques.

## Dépannage : 8 problèmes fréquents et solutions

- **La Skill n’apparaît pas dans le menu `/`.** Vérifiez que le fichier s’appelle exactement`SKILL.md` (sensible à la casse) et qu’il se trouve directement à la racine de son dossier, pas dans un sous-dossier. Vérifiez aussi que`user-invocable` n’est pas positionné à`false` .
- **Claude ne déclenche jamais la Skill automatiquement.** La cause numéro un est une`description` mal formulée. Réécrivez-la en incluant les mots exacts qu’un utilisateur emploierait pour formuler sa demande.
- **Erreur “Unexpected key(s) in SKILL.md frontmatter” lors de l’empaquetage.** Vous utilisez un champ Claude Code (comme`argument-hint` ou`context` ) qui ne fait pas partie des six champs portables. Retirez-le ou gardez deux versions du frontmatter selon la destination.
- **La Skill fonctionne dans Claude Code mais pas sur claude.ai.** Normal : les Skills ne se synchronisent jamais automatiquement entre les surfaces. Il faut la téléverser séparément sur chaque canal où vous voulez l’utiliser.
- **Le champ `allowed-tools` avec `${CLAUDE_SKILL_DIR}` continue à demander une confirmation.** Cette substitution dans`allowed-tools` nécessite une version relativement récente de Claude Code. Mettez à jour avec`claude --version` pour vérifier, puis mettez à jour l’outil si besoin.
- **Deux Skills du même nom entrent en conflit dans un monorepo.** C’est un comportement attendu : la variante imbriquée apparaît sous un nom qualifié par son chemin (par exemple`apps/web:deploy` ), tandis que le nom court reste réservé à la Skill de la racine du projet.
- **Un script s’exécute mais son résultat n’apparaît nulle part.** Vérifiez que le SKILL.md demande explicitement à Claude d’exécuter le script et d’utiliser sa sortie, plutôt que de simplement mentionner son existence.
- **Une nouvelle Skill ajoutée en cours de session reste invisible.** La détection à chaud couvre les modifications d’un SKILL.md existant, mais la création d’un tout nouveau dossier racine de Skills nécessite un redémarrage de Claude Code pour être détectée.

## Astuces avancées pour aller plus loin

Une fois les bases maîtrisées, plusieurs mécanismes permettent d’aller nettement plus loin dans la spécialisation de vos Skills. Le champ `paths` accepte des motifs glob qui limitent l’activation automatique d’une Skill aux fichiers correspondants : une Skill de conventions React ne se déclenchera ainsi que lorsque Claude travaille sur des fichiers `.tsx`, sans jamais polluer le contexte système prompt pour le reste du projet.

Le champ `hooks` permet d’attacher des scripts au cycle de vie propre d’une Skill (avant ou après son exécution), utile pour journaliser chaque déclenchement d’une Skill sensible ou valider un état avant de la laisser s’exécuter. Le champ `metadata`, lui, accepte n’importe quelle donnée clé-valeur libre : pratique pour votre propre tooling interne (numéro de version, équipe propriétaire, catalogue de Skills), sans que Claude Code n’agisse dessus directement.

Pour les équipes qui gèrent un catalogue de Skills conséquent, le dépôt open source anthropics/skills mérite un détour : il embarque notamment une Skill “Claude API” livrant une documentation de référence à jour pour huit langages de programmation, directement intégrée par défaut dans Claude Code. Le succès de ce dépôt officiel donne une idée de l’engouement suscité par le format : selon Taskade, il totalisait plus de 157 000 étoiles GitHub en juillet 2026, dépassé par la Skill communautaire Superpowers pour Claude Code qui affichait de son côté plus de 243 000 étoiles à la même date. C’est un bon modèle à étudier pour structurer vos propres Skills de référence technique, avec la même logique de fichiers annexes chargés à la demande plutôt qu’un unique fichier monolithique.

Enfin, pensez à la commande `/run-skill-generator`, décrite dans la documentation de Claude Code sur les Skills, une Skill embarquée dans Claude Code qui observe comment lancer votre application depuis un environnement propre puis enregistre la recette (commandes d’installation, variables d’environnement, script de lancement) comme Skill de projet réutilisable dans `.claude/skills/run-<nom>/`. Toute la logique de découverte manuelle que vous referiez sinon à chaque nouvelle session se transforme ainsi en Skill persistante, partagée avec le reste de l’équipe dès qu’elle est committée.

## Foire aux questions

**Les Claude Skills sont-elles gratuites ?**

Créer et utiliser une Skill personnalisée dans Claude Code ne coûte rien au-delà de votre abonnement Claude existant. Sur claude.ai, le téléversement de Skills personnalisées nécessite un plan Pro, Max, Team ou Enterprise avec l’exécution de code activée. Les Skills pré-construites (pptx, xlsx, docx, pdf) sont incluses sur claude.ai et facturées à l’usage normal des tokens sur l’API, où le tarif standard de Claude Sonnet 5 est fixé depuis le 10 août 2026 à 2 $ par million de tokens en entrée et 10 $ par million de tokens en sortie, d’après les notes de version d’Anthropic.

**Claude Skills fonctionne-t-il avec d’autres assistants IA que Claude ?**

Le format a été publié comme standard ouvert le 18 décembre 2025 sur agentskills.io, précisément pour permettre cette portabilité. Plusieurs rapports publiés fin 2025 et courant 2026 indiquent qu’OpenAI a annoncé une prise en charge du format dans Codex. Le niveau de compatibilité réel dépend cependant de l’implémentation de chaque outil tiers.

**Quelle est la différence entre une Skill et un plugin Claude Code ?**

Une Skill est une unité de savoir procédural : un dossier avec un SKILL.md. Un plugin est un paquet de distribution qui peut regrouper plusieurs Skills avec des agents, des hooks et des serveurs MCP. Une Skill peut exister seule ; un plugin est une façon d’en empaqueter et d’en partager plusieurs ensemble.

**Où stocker mes Skills pour qu’elles soient partagées avec toute l’équipe ?**

Committez le dossier dans `.claude/skills/` à la racine du dépôt Git. Chaque membre de l’équipe qui clone le projet et lance Claude Code y a accès automatiquement, sans étape d’installation manuelle.

**Les Skills sont-elles sûres à utiliser en production ?**

Une Skill que vous avez écrite vous-même, testée et versionnée dans Git ne pose pas plus de risque qu’un script interne classique. Le risque vient des Skills récupérées de sources tierces non vérifiées : Anthropic recommande explicitement de les traiter comme n’importe quel logiciel externe, avec audit du code avant toute utilisation en contexte sensible.

**Combien de Skills puis-je installer sans ralentir Claude ?**

Grâce à la divulgation progressive, chaque Skill installée ne coûte qu’environ 100 tokens au repos (son nom et sa description). Le nombre de Skills qui reste confortable dépend surtout de la lisibilité de vos descriptions : au-delà de plusieurs dizaines, il devient plus difficile d’écrire des descriptions suffisamment distinctes pour que Claude choisisse la bonne à chaque fois.

**Puis-je utiliser une Skill écrite pour Claude Code dans l’API ?**

Pas directement si elle utilise des champs propres à Claude Code comme `context`, `argument-hint` ou l’injection dynamique `` !`commande` ``. Pour la rendre compatible avec l’API ou claude.ai, limitez le frontmatter aux six champs portables du standard et retirez toute syntaxe spécifique à Claude Code du corps du fichier.

**Comment mettre à jour une Skill déjà installée ?**

Dans Claude Code, modifiez directement le fichier SKILL.md : la détection à chaud applique le changement dans la session en cours, sans redémarrage. Sur claude.ai ou via l’API, il faut retéléverser une nouvelle archive pour remplacer la version existante.

## Ce qu’il faut retenir

Claude Skills résout un problème précis : arrêter de répéter les mêmes instructions à chaque conversation, sans pour autant les charger en permanence comme le ferait un CLAUDE.md surdimensionné. La divulgation progressive à trois niveaux, le frontmatter YAML normalisé et le statut de standard ouvert publié en décembre 2025 en font une brique durable de l’écosystème Claude plutôt qu’une fonctionnalité isolée. Commencez petit — une seule Skill personnelle, testée localement — avant de la faire évoluer vers un partage d’équipe via Git ou un plugin.
