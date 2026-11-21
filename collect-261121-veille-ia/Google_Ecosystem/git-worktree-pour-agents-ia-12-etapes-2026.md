---
id: collect-261121-veille-ia/google-ecosystem/git-worktree-pour-agents-ia-12-etapes-2026
title: "Git Worktree pour Agents IA : 12 Étapes [2026]"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["worktree", "projet", "sans", "agents", "meme", "propre", "terminal", "claude"]
source: https://tech-insider.org/fr/git-worktree-agents-ia-2026/
source_anchor: ""
source_lines: [1, 399]
section: "Google_Ecosystem"
---
Il y a encore deux ans, un développeur lançait un seul assistant IA à la fois et attendait sa réponse avant de passer à autre chose. En 2026, ce modèle a explosé. Claude Code corrige un bug pendant que Cursor écrit une fonctionnalité et que GitHub Copilot génère des tests, le tout sur le même dépôt, en même temps. Le problème, c’est que Git n’a jamais été conçu pour qu’un seul dossier de travail accueille trois sessions concurrentes sans qu’elles se marchent dessus.

La réponse ne vient pas d’un nouvel outil IA, mais d’une commande Git vieille de plus de dix ans : **git worktree**. Elle permet de créer plusieurs dossiers de travail reliés au même dépôt, chacun sur sa propre branche, sans dupliquer l’historique et sans les conflits classiques du changement de branche. Ce tutoriel montre comment l’installer, la structurer et l’utiliser concrètement pour faire tourner plusieurs agents de codage IA en parallèle, avec un projet complet à la fin.

Douze étapes composent ce guide, de la vérification de votre version de Git jusqu’au nettoyage final des dossiers devenus inutiles, en passant par un script d’automatisation et un exemple de bout en bout sur un vrai projet Node.js. Comptez environ 45 minutes pour tout suivre, y compris les tests. Vous repartirez avec une structure de dossiers réutilisable sur n’importe quel projet, quel que soit l’agent IA que vous utilisez au quotidien.

## Pourquoi git worktree s’impose dans les workflows d’agents IA en 2026

Le scénario est devenu banal dans les équipes qui utilisent Claude Code, Cursor ou GitHub Copilot au quotidien : vous lancez un agent sur une tâche, puis vous voulez en démarrer un second sur un autre ticket sans attendre que le premier termine. Avec un dépôt classique, il faut choisir entre attendre, ou changer de branche et perdre le contexte du premier agent (fichiers modifiés, processus lancés, serveur de développement actif). Aucune des deux options ne convient à un usage sérieux.

Git worktree règle ce problème directement à la racine, sans passer par des clones séparés ni des scripts maison. Chaque worktree est un dossier indépendant avec son propre état de fichiers, sa propre zone d’index et sa propre branche extraite, tout en partageant la même base d’objets Git (commits, arbres, blobs). Résultat : trois agents peuvent travailler sur trois branches différentes du même projet, dans trois dossiers distincts, sans jamais se gêner.

Ce n’est pas une fonctionnalité expérimentale. Elle existe depuis Git 2.5, sorti en 2015, bien avant l’essor des agents de codage. Ce qui change en 2026, c’est l’usage : la documentation de Claude Code recommande d’isoler chaque session dans son propre worktree pour éviter les interférences entre tâches, et la pratique s’est généralisée à Cursor, Copilot et OpenAI Codex CLI par la force des choses. Ce guide part de zéro et construit, étape par étape, un vrai poste de travail multi-agents.

## Prérequis : versions, outils et comptes nécessaires

Avant de commencer, réunissez les éléments suivants. Rien d’exotique : l’essentiel tient dans un terminal et une installation Git à jour.

- **Git 2.40 ou supérieur** (git worktree fonctionne dès Git 2.5, mais les sous-commandes move et repair sont plus fiables sur les versions récentes)
- Un terminal (macOS Terminal, Windows Terminal avec WSL2, ou tout terminal Linux)
- Un compte GitHub ou GitLab avec un dépôt existant, ou un projet local à initialiser
- Au moins un agent de codage IA installé : Claude Code CLI, Cursor, GitHub Copilot ou OpenAI Codex CLI
- Node.js 20 ou supérieur, ou Python 3.11 ou supérieur, selon le langage de votre projet (utilisé pour l’exemple de ce tutoriel)
- Environ 2 Go d’espace disque libre par worktree actif si votre projet a des dépendances lourdes
- Des notions de base sur les branches Git (checkout, merge, pull) : ce tutoriel ne les réexplique pas

Vérifiez votre version avec `git --version`. Sous macOS, une mise à jour passe par `brew upgrade git`. Sous Ubuntu ou Debian, `sudo apt update && sudo apt install git` suffit généralement. Sous Windows, réinstallez la dernière version depuis git-scm.com ou passez par WSL2, largement recommandé pour les agents de codage en 2026.

## Comprendre git worktree : les concepts essentiels avant de commencer

Un dépôt Git classique contient un seul dossier `.git` et un seul dossier de travail. Quand vous changez de branche, Git réécrit les fichiers de ce même dossier pour refléter l’état de la nouvelle branche. C’est rapide, mais ça impose un choix : un seul état visible à la fois.

Git worktree change la donne en séparant deux choses qui semblaient liées : la base de données Git (l’historique, les commits, les objets) et le dossier de travail (les fichiers que vous voyez et éditez). Une seule base de données peut alimenter plusieurs dossiers de travail simultanés, chacun pointant vers une branche différente. Trois règles gouvernent ce fonctionnement :

- **Une branche ne peut être extraite que dans un seul worktree à la fois.** Git refuse explicitement de la checkout ailleurs tant qu’elle reste active quelque part.
- **Les objets sont partagés, pas dupliqués.** Un commit créé dans un worktree est immédiatement visible dans les autres, sans opération réseau.
- **Chaque worktree a son propre index et son propre HEAD.** Vous pouvez avoir des fichiers modifiés et non validés dans un worktree sans que cela affecte les autres.

Cette architecture est exactement ce dont un agent de codage IA a besoin. Un agent qui modifie dix fichiers pour corriger un bug ne doit pas voir ses changements se mélanger avec ceux d’un second agent qui construit une fonctionnalité sur une autre branche. Avec des clones séparés, on obtient la même isolation, mais au prix d’un espace disque et d’un temps de clonage démultipliés. Le worktree partage l’historique et ne duplique que ce qui doit vraiment l’être.

Concrètement, chaque worktree ajouté laisse une trace dans un dossier administratif caché, `.git/worktrees/<nom>`, situé dans le dépôt principal. C’est là que Git range les informations propres à ce worktree : sa référence HEAD, son fichier de configuration local et l’emplacement du dossier de travail associé. Vous n’avez jamais besoin d’y toucher directement, mais savoir que cette structure existe explique pourquoi une commande comme `git worktree repair` fonctionne : elle recalcule simplement le contenu de ces dossiers administratifs quand un chemin a changé.

## Étape 1 – Vérifier et préparer votre installation Git

Ouvrez un terminal dans le dossier de votre projet et confirmez que git worktree est disponible.

```
git --version
git worktree --help
```
Vous devriez voir un résumé des sous-commandes disponibles : add, list, lock, move, prune, remove, repair et unlock. Si la commande `git worktree` renvoie une erreur du type « git: ‘worktree’ is not a git command », votre version de Git est trop ancienne et il faut la mettre à jour avant d’aller plus loin.

Profitez-en pour repérer le chemin racine de votre dépôt, car vous en aurez besoin à chaque étape suivante :

```
git rev-parse --show-toplevel
# Exemple de sortie : /Users/marie/projets/api-recettes
```
## Étape 2 – Créer votre premier worktree avec git worktree add

La commande centrale de ce tutoriel est `git worktree add`. Elle prend un chemin de destination et, en option, une branche à extraire ou à créer. Depuis la racine de votre dépôt principal, lancez :

`git worktree add ../api-recettes-agent-claude -b agent/fix-auth`
Cette ligne crée un nouveau dossier `api-recettes-agent-claude` à côté de votre dépôt principal, avec une nouvelle branche `agent/fix-auth` créée à partir de votre HEAD actuel. Git affiche une confirmation :

```
Preparing worktree (new branch 'agent/fix-auth')
HEAD is now at 3f9a21c Corrige la validation des tokens
Branch 'agent/fix-auth' set up to track 'origin/main'.
```
Si vous voulez travailler sur une branche existante plutôt qu’en créer une nouvelle, omettez le drapeau `-b` et indiquez simplement le nom de la branche : `git worktree add ../api-recettes-review feature/pagination`. Git refusera si cette branche est déjà extraite ailleurs, ce qui est précisément le garde-fou qui évite les conflits silencieux entre deux agents.

## Étape 3 – Lister et auditer vos worktrees actifs

Une fois que plusieurs worktrees existent, il devient facile de perdre le fil. La commande `git worktree list` affiche tous les worktrees enregistrés, leur chemin, le commit sur lequel ils pointent et leur branche.

```
git worktree list
/Users/marie/projets/api-recettes            3f9a21c [main]
/Users/marie/projets/api-recettes-agent-claude  3f9a21c [agent/fix-auth]
/Users/marie/projets/api-recettes-review        7c1e4f0 [feature/pagination]
```
Pour un traitement automatisé (par exemple dans un script de nettoyage ou un tableau de bord interne), la variante `git worktree list --porcelain` donne une sortie stable, ligne par ligne, pensée pour être analysée par un programme plutôt que lue à l’œil. C’est la commande à retenir : lancez-la avant chaque session de travail multi-agents pour vérifier qu’aucun ancien worktree oublié ne traîne encore.

## Étape 4 – Adopter une arborescence pensée pour le multi-agent

Les noms de dossiers au hasard fonctionnent pour un test rapide, mais deviennent vite ingérables dès que trois ou quatre agents tournent en même temps. Un schéma répandu dans les guides communautaires consiste à regrouper tous les worktrees d’agents sous un sous-dossier dédié, souvent nommé `.trees`, avec un identifiant de tâche par worktree :

```
api-recettes/                  (dépôt principal, branche main)
.trees/
  fix-auth-401/                (agent Claude Code)
  pagination-v2/                (agent Cursor)
  tests-integration/            (agent Codex CLI)
```
Ce découpage a deux avantages concrets. D’abord, chaque nom de dossier documente lui-même la tâche en cours, ce qui évite d’ouvrir un worktree au hasard pour se souvenir de ce qu’il contient. Ensuite, un simple `.trees/` ajouté au `.gitignore` du dépôt principal empêche ces dossiers de dépendances imbriquées de polluer les commits si jamais quelqu’un travaille par erreur depuis la racine. Gardez le dépôt principal propre et réservez-le à la lecture, aux revues de code et aux fusions, jamais à l’édition directe pendant qu’un agent tourne ailleurs.

Le tableau suivant résume la convention à adopter selon l’agent utilisé, à titre de point de départ pour votre propre équipe.

| Agent IA | Mode de lancement | Fichier de contexte par worktree | Commande d’ouverture | 
|---|---|---|---|
| Claude Code | Terminal, dans le dossier du worktree | CLAUDE.md | cd .trees/<tache> && claude | 
| Cursor | Fenêtre IDE dédiée | .cursor/rules | cursor .trees/<tache> | 
| GitHub Copilot (VS Code) | Fenêtre IDE dédiée | copilot-instructions.md | code .trees/<tache> | 
| OpenAI Codex CLI | Terminal, dans le dossier du worktree | AGENTS.md | cd .trees/<tache> && codex | 

Cette table n’a rien de figé : adaptez les noms de dossiers et les fichiers de contexte aux conventions déjà en place dans votre équipe. Ce qui compte, c’est la cohérence, pas le respect exact de ce modèle.

## Étape 5 – Isoler une session Claude Code dans son propre worktree

Une fois le worktree créé, déplacez-vous dedans et lancez votre agent normalement, comme s’il s’agissait d’un dépôt classique.

```
cd .trees/fix-auth-401
claude
```
Claude Code démarre sa session dans ce dossier précis et n’a aucune visibilité sur ce qui se passe dans les autres worktrees. Si votre projet utilise un fichier `CLAUDE.md` pour donner des instructions persistantes à l’agent, placez-en une copie adaptée dans chaque worktree : un agent qui corrige un bug d’authentification n’a pas besoin des mêmes consignes qu’un agent qui écrit des tests d’intégration. Cette isolation des instructions, en plus de l’isolation des fichiers, rend le multi-agent réellement fiable plutôt qu’une source de confusion. Pour aller plus loin sur la configuration fine de Claude Code, notre tutoriel sur les sous-agents Claude Code détaille comment déléguer des rôles spécifiques à chaque session.

## Étape 6 – Faire tourner Cursor et GitHub Copilot en parallèle sans conflit

Le principe reste identique pour les agents intégrés à un IDE. Ouvrez chaque worktree dans sa propre fenêtre plutôt que dans un onglet du même projet : Cursor et VS Code (avec l’extension Copilot) traitent chaque fenêtre comme un espace de travail totalement séparé, ce qui correspond exactement à l’isolation offerte par le worktree.

```
cursor .trees/pagination-v2
code .trees/tests-integration
```
Un piège classique ici : ouvrir les deux worktrees comme deux dossiers d’un même espace de travail multi-racine. Techniquement possible, cette approche mélange les vues de fichiers à l’écran et augmente le risque qu’un agent modifie le mauvais dossier par inadvertance. Une fenêtre par worktree, sans exception, reste la règle la plus sûre pour un usage sérieux avec plusieurs agents actifs.

## Étape 7 – Ajouter OpenAI Codex CLI à la rotation d’agents

Le même schéma s’applique à un agent en ligne de commande comme Codex CLI. Depuis son propre worktree, lancez simplement l’outil :

```
cd .trees/tests-integration
codex
```
Codex CLI, comme Claude Code, fonctionne en boucle lecture-édition-vérification directement sur les fichiers du dossier courant. Comme ce dossier est un worktree indépendant, l’agent peut exécuter ses propres commandes de test, installer ses propres dépendances et même faire planter son serveur de développement local sans jamais toucher aux deux autres sessions en cours. Notre présentation complète de Codex CLI couvre l’installation et l’authentification si ce n’est pas encore fait.

À ce stade, vous avez trois agents actifs sur trois branches différentes du même dépôt, dans trois dossiers physiquement distincts. C’est le cœur du workflow. Les étapes suivantes traitent des détails pratiques qui déterminent si cette configuration tient sur la durée ou s’effondre au bout de deux jours.

## Étape 8 – Gérer les fichiers .env, les ports et les bases de données par worktree

Un fichier `.env` n’est jamais suivi par Git, donc il n’existe pas automatiquement dans un nouveau worktree. Sans lui, la plupart des projets refusent de démarrer. Copiez-le manuellement, ou via un petit script, à chaque création de worktree :

`cp ../api-recettes/.env .trees/fix-auth-401/.env`
Le vrai piège arrive ensuite : si les trois agents lancent chacun un serveur de développement sur le port 3000, deux d’entre eux échoueront au démarrage. Attribuez un port distinct par worktree, soit directement dans chaque `.env`, soit via une variable d’environnement passée au lancement :

```
PORT=3001 npm run dev    # worktree fix-auth-401
PORT=3002 npm run dev    # worktree pagination-v2
PORT=3003 npm run dev    # worktree tests-integration
```
Pour les projets avec base de données, le même raisonnement s’applique : soit une base SQLite distincte par worktree (le plus simple), soit un schéma ou une base PostgreSQL séparée par tâche si votre projet dépend d’un serveur partagé. Ignorer ce point est la cause numéro un des échecs silencieux quand un agent « ne voit pas » les changements d’un autre : les deux ne travaillent tout simplement pas sur les mêmes données.

## Étape 9 – node_modules, environnements virtuels et l’explosion du disque

Git worktree ne partage que ce qui est suivi par Git. Les dossiers `node_modules`, les environnements virtuels Python ou les dossiers `vendor` ne le sont pas, et doivent donc être réinstallés dans chaque worktree.

```
# Dans chaque worktree Node.js
npm install
# Dans chaque worktree Python
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```
Avec trois ou quatre worktrees actifs, cette duplication peut représenter plusieurs gigaoctets rapidement, surtout sur des projets Node.js avec de grosses arborescences de dépendances. Deux options réduisent l’impact réel. La première consiste à utiliser un gestionnaire de paquets qui mutualise le cache global, comme pnpm : les fichiers sont stockés une seule fois sur le disque et liés physiquement dans chaque `node_modules`, ce qui limite l’espace réellement consommé même si chaque worktree paraît complet. La seconde, plus radicale, consiste à ne réinstaller les dépendances que dans les worktrees réellement actifs, et à en supprimer certains dès qu’une tâche est terminée plutôt que de les laisser s’accumuler pendant des semaines.

## Étape 10 – Verrouiller, déplacer et réparer vos worktrees

Trois commandes complètent la boîte à outils pour une utilisation prolongée. `git worktree lock` empêche Git de considérer un worktree comme obsolète, utile pour une tâche longue qui dure plusieurs jours ou pour un worktree stocké sur un disque externe qui n’est pas toujours connecté :

```
git worktree lock .trees/fix-auth-401 --reason "Migration longue, ne pas purger"
git worktree unlock .trees/fix-auth-401
```
`git worktree move` renomme ou déplace un worktree existant sans casser son lien avec le dépôt principal, ce qui est plus sûr qu’un simple `mv` dans le terminal :

`git worktree move .trees/fix-auth-401 .trees/fix-auth-legacy`
Si malgré tout un worktree a été déplacé à la main, ou si les métadonnées internes se retrouvent désynchronisées après une synchronisation de dossier ou une restauration de sauvegarde, `git worktree repair` recalcule les références et remet tout en ordre :

`git worktree repair`
## Étape 11 – Nettoyer avec remove et prune, puis fusionner le travail validé

Une fois qu’un agent a terminé sa tâche et que sa branche a été fusionnée, supprimez proprement le worktree correspondant plutôt que de le laisser traîner :

`git worktree remove .trees/fix-auth-401`
Git refuse cette suppression si des fichiers non validés subsistent, sauf à ajouter `--force`, ce qui protège contre une perte de travail accidentelle. Si un worktree a été supprimé manuellement avec un simple `rm -rf` plutôt qu’avec la commande dédiée, Git conserve une référence fantôme dans ses métadonnées. La commande `git worktree prune` nettoie ces références orphelines :

```
git worktree prune
git worktree list   # confirme que le worktree fantôme a disparu
```
Prenez l’habitude de lancer `git worktree list` et `git worktree prune` en début de semaine. Sur un projet actif avec plusieurs agents, les worktrees oubliés s’accumulent plus vite qu’on ne l’imagine.

## Étape 12 – Automatiser la création de worktrees avec un script shell

Répéter les mêmes commandes à chaque nouvelle tâche devient vite fastidieux. Un petit script bash suffit à standardiser la création d’un worktree d’agent, copie du `.env` comprise :

```
#!/usr/bin/env bash
# nouveau-worktree-agent.sh
set -e
TACHE=$1
if [ -z "$TACHE" ]; then
  echo "Usage : ./nouveau-worktree-agent.sh nom-de-la-tache"
  exit 1
fi
RACINE=$(git rev-parse --show-toplevel)
CIBLE="$RACINE/.trees/$TACHE"
git worktree add "$CIBLE" -b "agent/$TACHE"
cp "$RACINE/.env" "$CIBLE/.env"
echo "Worktree prêt : $CIBLE (branche agent/$TACHE)"
```
Rendez-le exécutable une fois avec `chmod +x nouveau-worktree-agent.sh`, puis lancez `./nouveau-worktree-agent.sh corrige-pagination` pour obtenir un worktree prêt à l’emploi en une seule commande, avec une convention de nommage cohérente sur toute l’équipe.

## Projet complet : orchestrer 3 agents IA en parallèle sur un vrai dépôt

### Le scénario : une API de recettes à améliorer

Prenons un exemple concret et complet, du premier commit à la fusion finale. Le projet est une petite API REST Node.js, `api-recettes`, avec trois tickets ouverts : un bug d’authentification, une fonctionnalité de pagination, et un manque de couverture de tests. Plutôt que de les traiter l’un après l’autre, l’objectif est de les faire avancer en parallèle grâce à trois worktrees et trois agents différents.

### Mise en place des trois worktrees

```
cd ~/projets/api-recettes
git pull origin main
./nouveau-worktree-agent.sh fix-auth-401
./nouveau-worktree-agent.sh pagination-v2
./nouveau-worktree-agent.sh tests-integration
git worktree list
# ~/projets/api-recettes                          [main]
# ~/projets/api-recettes/.trees/fix-auth-401       [agent/fix-auth-401]
# ~/projets/api-recettes/.trees/pagination-v2      [agent/pagination-v2]
# ~/projets/api-recettes/.trees/tests-integration  [agent/tests-integration]
```
Chaque worktree reçoit ensuite ses dépendances et son port dédié, puis son agent :

```
# Terminal 1
cd .trees/fix-auth-401 && npm install && PORT=3001 npm run dev &
claude
# Terminal 2
cd .trees/pagination-v2 && npm install
cursor .
# Terminal 3
cd .trees/tests-integration && npm install
codex
```
### Résultat : trois pull requests indépendantes en une matinée

Claude Code corrige la validation des tokens expirés et committe sur `agent/fix-auth-401`. Cursor construit la pagination sur `agent/pagination-v2`, avec son propre serveur de développement sur le port 3002, sans jamais voir les fichiers modifiés par Claude Code. Codex CLI écrit une suite de tests d’intégration sur `agent/tests-integration`, en lisant le code de la branche principale, indépendamment des deux autres chantiers.

Une fois chaque tâche validée, la fusion se fait normalement depuis le dépôt principal, une branche à la fois :

```
cd ~/projets/api-recettes
git checkout main
git merge agent/fix-auth-401
git worktree remove .trees/fix-auth-401
git branch -d agent/fix-auth-401
```
Répétez pour les deux autres branches une fois leurs pull requests respectives approuvées. Le résultat concret : trois chantiers qui, traités en série, auraient occupé toute une journée, avancent en parallèle et convergent en fin de matinée, chacun avec son propre historique de commits, propre et traçable.

## Git worktree face aux alternatives : branches, clones et subtrees

Git worktree n’est pas la seule façon d’isoler du travail parallèle. Voici comment elle se compare aux approches historiques, sur les critères qui comptent réellement pour un usage avec des agents IA.

| Approche | Isolation des fichiers | Espace disque | Historique partagé | Adapté aux agents IA | 
|---|---|---|---|---|
| Git worktree | Complète (dossier séparé) | Faible (objets partagés) | Oui, instantané | Oui, conçu pour ça | 
| Changement de branche classique | Aucune (même dossier) | Minimal | Oui | Non, un seul état à la fois | 
| Clone séparé du dépôt | Complète | Élevé (historique dupliqué) | Non, nécessite un push/pull | Partiel, mais lourd | 
| Git subtree / submodule | Partielle | Variable | Complexe à synchroniser | Non, pensé pour un autre usage | 

Le clone séparé reste utile dans un cas précis : si vous devez travailler sur deux dépôts distants différents, ou tester une version totalement isolée sans même partager la configuration Git locale. Pour tout le reste, et en particulier pour plusieurs agents sur le même projet, le worktree est la solution la plus légère et la plus rapide à mettre en place.

Le changement de branche classique garde son utilité pour un travail strictement séquentiel, une seule tâche à la fois, sans besoin de revenir en arrière rapidement. Dès qu’un second agent ou un second développeur entre en jeu sur le même projet au même moment, la bascule vers les worktrees change la donne : plus besoin de stasher des modifications en cours pour aller vérifier autre chose, plus de risque d’oublier un fichier non validé avant de changer de contexte.

## Tableau récapitulatif des commandes git worktree

| Commande | Rôle | Exemple | 
|---|---|---|
| worktree add | Crée un nouveau worktree, avec ou sans nouvelle branche | git worktree add ../app-feature -b feature/x | 
| worktree list | Liste tous les worktrees enregistrés | git worktree list –porcelain | 
| worktree remove | Supprime un worktree propre (sans fichiers non validés) | git worktree remove ../app-feature | 
| worktree prune | Nettoie les références orphelines après une suppression manuelle | git worktree prune | 
| worktree lock | Empêche la purge automatique d’un worktree | git worktree lock ../app-feature –reason “en cours” | 
| worktree unlock | Retire le verrou posé par lock | git worktree unlock ../app-feature | 
| worktree move | Déplace ou renomme un worktree sans casser le lien | git worktree move ../ancien ../nouveau | 
| worktree repair | Recalcule les métadonnées après un déplacement manuel | git worktree repair | 

## 5 pièges courants à éviter avec les worktrees et les agents IA

- **Oublier de copier le fichier .env :** le worktree hérite du code suivi par Git, jamais des fichiers ignorés. Sans script d’automatisation, cette étape saute une fois sur deux et l’agent se retrouve à déboguer une erreur de configuration au lieu de sa vraie tâche.
- **Laisser deux agents partager le même port ou la même base de données :** les symptômes sont trompeurs, un agent semble « ignorer » les changements de l’autre alors qu’en réalité les deux serveurs se battent pour la même ressource.
- **Travailler directement dans le dépôt principal pendant qu’un agent est actif ailleurs :** le dossier racine doit rester réservé aux fusions et à la lecture, jamais à l’édition simultanée.
- **Accumuler des worktrees jamais nettoyés :** sur un projet actif, dix worktrees oubliés en un mois ne sont pas rares, et chacun consomme de l’espace disque via ses propres node_modules ou environnements virtuels.
- **Vouloir extraire deux fois la même branche :** Git bloque cette opération par conception, mais le message d’erreur surprend souvent les nouveaux utilisateurs, qui pensent avoir trouvé un bug plutôt qu’une protection.
- **Donner des noms de branches trop proches les uns des autres :** agent/fix, agent/fix2 et agent/fix-final finissent par se confondre dans les revues de code. Préférez un identifiant de ticket ou une description courte et unique, alignée sur le nom du dossier du worktree.

## Dépannage : 8 problèmes fréquents et leurs solutions

**« fatal: ‘nom-branche’ is already checked out at … »**

Cette branche est déjà active dans un autre worktree. Utilisez `git worktree list` pour la retrouver, ou créez une nouvelle branche dédiée avec `-b` plutôt que de réutiliser l’existante.

**« fatal: ‘<chemin>’ already exists »**

Le dossier cible existe déjà, souvent parce qu’un worktree précédent n’a pas été supprimé proprement. Vérifiez avec `git worktree list`, puis `git worktree remove` avant de recréer.

**Un worktree supprimé avec rm -rf apparaît encore dans git worktree list**

Git conserve une référence administrative tant qu’on ne lance pas `git worktree prune`. C’est le comportement attendu, pas un bug.

**« fatal: cannot remove a locked working tree »**

Le worktree a été verrouillé volontairement. Retirez le verrou avec `git worktree unlock <chemin>` avant de le supprimer.

**npm run dev échoue avec « port already in use »**

Deux worktrees tentent d’utiliser le même port. Attribuez un port distinct à chaque worktree via la variable PORT ou un fichier .env spécifique.

**L’agent IA ne voit pas les derniers commits de la branche principale**

Un worktree ne se synchronise pas automatiquement avec origin/main. Lancez `git fetch origin` puis `git rebase origin/main` ou `git merge origin/main` depuis le worktree concerné.

**« fatal: is not a working tree » après une synchronisation cloud (Dropbox, iCloud, OneDrive)**

Les outils de synchronisation cloud perturbent les métadonnées internes de Git. Excluez le dossier .git et les dossiers .trees de la synchronisation, ou lancez `git worktree repair` pour recalculer les références.

**git worktree move échoue avec une erreur de verrouillage**

Fermez tout processus qui a un fichier ouvert dans ce worktree (éditeur, serveur de développement, terminal actif) avant de tenter le déplacement.

**Une pull request d’agent contient des fichiers inattendus (node_modules, .env)**

Le .gitignore du dépôt principal ne couvre pas toujours les dossiers créés dans .trees. Ajoutez explicitement .trees/ à la racine du .gitignore pour éviter tout commit accidentel.

**Deux agents proposent des correctifs qui se contredisent sur le même fichier**

Ce n’est pas un problème de worktree à proprement parler, mais un problème de découpage des tâches. Vérifiez que les tickets assignés à chaque agent touchent des fichiers ou des modules suffisamment distincts avant de lancer les sessions en parallèle, sans quoi la fusion finale demandera un arbitrage manuel de toute façon.

## Astuces avancées pour les équipes européennes

Pour les équipes qui travaillent sur des dépôts contenant des données sensibles ou soumises au RGPD, gardez à l’esprit qu’un worktree copie les fichiers suivis sur le disque local, comme n’importe quel checkout Git. Si votre politique de sécurité interdit certaines données de configuration en dehors d’un environnement contrôlé, appliquez les mêmes règles de chiffrement de disque et de gestion des secrets à tous les worktrees, pas seulement au dépôt principal. Un secret oublié dans un `.env` copié à la hâte dans `.trees/` reste un secret exposé.

Côté productivité, un alias Git simplifie la vie au quotidien. Ajoutez ceci dans votre `~/.gitconfig` :

```
[alias]
    wt = worktree
    wtl = worktree list
```
Vous gagnez alors la possibilité de taper `git wtl` au lieu de la commande complète, un détail qui compte quand vous vérifiez l’état de vos worktrees une dizaine de fois par jour. Sur VS Code, l’extension native de gestion de dépôts affiche désormais les worktrees dans la barre latérale Source Control, ce qui évite de revenir sans cesse au terminal juste pour lister les sessions actives. Dans les IDE JetBrains (IntelliJ, PyCharm, WebStorm), chaque worktree s’ouvre simplement comme un projet distinct, avec sa propre fenêtre et son propre index de fichiers, sans configuration supplémentaire.

Enfin, pour les pipelines d’intégration continue, évitez de faire tourner des builds directement dans un dossier `.trees` partagé avec les développeurs. Préférez un checkout dédié dans l’environnement CI, qui reste indépendant des sessions d’agents locales et évite tout effet de bord entre une exécution automatisée et un poste de travail humain.

Pour ceux qui travaillent en SSH sur un serveur distant plutôt qu’en local, combiner git worktree avec un multiplexeur de terminal comme tmux ou screen apporte un vrai confort. Chaque worktree correspond alors à une fenêtre tmux nommée d’après la tâche en cours, ce qui permet de détacher la session, de fermer son ordinateur portable, et de la retrouver intacte le lendemain avec l’agent toujours actif au même endroit. C’est particulièrement utile pour les tâches longues, comme une migration de base de données ou une refactorisation étendue, que vous ne voulez pas interrompre en fermant votre terminal par erreur.

## Foire aux questions

### Git worktree ralentit-il mon dépôt ?

Non. Les opérations Git courantes (commit, log, diff) restent aussi rapides que d’habitude puisque la base d’objets reste unique. Seul l’espace disque augmente, à cause des dépendances réinstallées dans chaque worktree, pas à cause de l’historique Git lui-même.

### Combien de worktrees puis-je créer en même temps ?

Il n’existe pas de limite technique fixée par Git. En pratique, la limite vient de votre espace disque et du nombre de sessions d’agents que vous pouvez réellement suivre. Trois à cinq worktrees actifs représentent déjà un maximum raisonnable pour un développeur seul.

### Puis-je utiliser git worktree avec GitHub Desktop ou un autre client graphique ?

La plupart des clients graphiques grand public ne gèrent pas nativement la création de worktrees, même s’ils affichent correctement les dossiers une fois créés en ligne de commande. Le plus simple reste de créer et gérer les worktrees via le terminal, puis d’ouvrir chaque dossier normalement dans votre client ou IDE préféré.

### Un worktree peut-il pointer vers un dépôt distant différent ?

Non. Tous les worktrees d’un même groupe partagent obligatoirement la même configuration de dépôt distant (remote), puisqu’ils partagent la même base d’objets Git. Pour travailler avec un remote totalement différent, un clone séparé reste la bonne solution.

### Que se passe-t-il si je supprime le dépôt principal par erreur ?

Les worktrees secondaires deviennent inutilisables, car ils dépendent tous du dossier .git central du dépôt principal. Ne supprimez jamais le dépôt d’origine tant que des worktrees actifs en dépendent, et gardez toujours une copie poussée sur votre remote (GitHub, GitLab) comme filet de sécurité.

### Git worktree fonctionne-t-il aussi bien sous Windows que sous macOS et Linux ?

Oui, la commande est identique sur les trois systèmes. Sous Windows, l’utilisation via WSL2 reste recommandée pour les workflows avec agents de codage IA, car la plupart des outils (Claude Code, Codex CLI) sont d’abord pensés pour un environnement Unix.

### Faut-il fusionner les branches d’agents une par une, ou toutes en même temps ?

Une par une, dans l’ordre où elles sont validées. Fusionner plusieurs branches d’agents simultanément multiplie le risque de conflits difficiles à attribuer à une tâche précise, alors qu’une fusion séquentielle garde un historique clair et un point de retour simple en cas de problème.

### Peut-on utiliser git worktree sur un très gros monorepo ?

Oui, et c’est même l’un des cas d’usage où le gain est le plus net. Sur un monorepo volumineux, cloner une copie complète pour chaque tâche coûte cher en temps et en espace disque. Le worktree évite ce coût puisque l’historique n’est jamais dupliqué. Le seul point de vigilance reste la réinstallation des dépendances par worktree, qui peut prendre plus de temps que sur un petit projet si le monorepo contient de nombreux paquets internes.

### Related Coverage

## Sources et documentation officielle

Pour approfondir chaque commande présentée dans ce tutoriel, la documentation officielle de Git reste la référence à jour : git-scm.com/docs/git-worktree. Les intégrations avec les agents IA mentionnées s’appuient sur la documentation de Claude Code, sur celle de GitHub Copilot, sur le dépôt officiel d’OpenAI Codex CLI et sur la documentation du contrôle de source de Visual Studio Code.
