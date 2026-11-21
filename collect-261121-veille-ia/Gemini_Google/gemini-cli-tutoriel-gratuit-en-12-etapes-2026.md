---
id: collect-261121-veille-ia/gemini-google/gemini-cli-tutoriel-gratuit-en-12-etapes-2026
title: "Gemini CLI : Tutoriel Gratuit en 12 Étapes [2026]"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "google", "jour", "code", "gratuit", "votre", "terminal", "comme"]
source: https://tech-insider.org/fr/gemini-cli-tutoriel-12-etapes-2026/
source_anchor: ""
source_lines: [1, 436]
section: "Gemini_Google"
---
Et si l’assistant de codage le plus puissant de 2026 tenait dans votre terminal ? C’est la promesse de **Gemini CLI**, l’agent IA open source lancé par Google le 25 juin 2025 sous licence Apache 2.0. Un an plus tard, jour pour jour, le projet a publié sa version **v0.49.0** sur GitHub (25 juin 2026) et s’est imposé comme l’une des portes d’entrée les plus suivies vers l’IA agentique : une fenêtre de contexte d’un million de tokens, un accès à la famille de modèles Gemini 3, et un quota qui a longtemps atteint 1 000 requêtes gratuites par jour avec un simple compte Google – avant que Google ne fasse évoluer les conditions d’accès mi-2026, comme nous le détaillons plus loin.

Ce tutoriel vous guide pas à pas, en 12 étapes et environ 40 minutes, pour installer, authentifier, configurer et exploiter Gemini CLI comme un développeur professionnel. Vous apprendrez à connecter des serveurs MCP, à écrire un fichier de contexte `GEMINI.md`, à automatiser vos scripts et à sécuriser l’exécution avec le sandbox. À la fin, vous disposerez d’un projet complet : un agent capable d’auditer et de documenter n’importe quel dépôt de code. Guide à jour au 03 juin 2026.

## Qu’est-ce que Gemini CLI et pourquoi l’adopter en 2026 ?

Gemini CLI est un **agent IA en ligne de commande** qui apporte la puissance des modèles Gemini directement dans votre terminal. Contrairement à un simple chatbot, il fonctionne selon une boucle agentique de type « raisonner puis agir » : il lit vos fichiers, exécute des commandes shell, interroge le web, écrit du code et vérifie ses propres résultats, le tout sous votre supervision. C’est cette autonomie encadrée qui distingue un agent d’un autocomplète classique.

Trois arguments expliquent son adoption fulgurante. D’abord, le **code source est ouvert** (Apache 2.0) : n’importe quel développeur peut inspecter le fonctionnement de l’agent, auditer sa sécurité et contribuer. Ensuite, le **quota généreux** a longtemps été le plus large du marché : dès le lancement, la connexion avec un compte Google personnel donnait 60 requêtes par minute et 1 000 requêtes par jour, sans carte bancaire – un accès gratuit qui a pris fin le 18 juin 2026, Google orientant désormais les comptes personnels vers les formules payantes Google AI Pro et Ultra, la première facturée environ 19,99 $/mois pour 1 500 requêtes par jour selon Comparly.ai (juillet 2026). Enfin, la **fenêtre de contexte d’un million de tokens** permet de charger des dépôts entiers, une documentation volumineuse ou plusieurs fichiers de logs sans découpage manuel.

Pour les équipes françaises et européennes, Gemini CLI présente un intérêt supplémentaire : la possibilité de le brancher sur **Vertex AI** et de choisir des régions de traitement au sein de l’UE, un point clé pour les organisations soumises au RGPD ou à des exigences de souveraineté des données. Là où un abonnement propriétaire impose son infrastructure, la nature ouverte de l’outil laisse le contrôle au développeur.

Face à la concurrence, Gemini CLI se positionne comme l’option « terminal-first » de Google, aux côtés d’alternatives comme Claude Code d’Anthropic ou GitHub Copilot. Le tableau ci-dessous résume ce positionnement. Chaque outil a ses forces : le choix dépend de votre modèle de facturation, de votre écosystème et de vos exigences de confidentialité.

| Critère | Gemini CLI | Claude Code | GitHub Copilot | 
|---|---|---|---|
| Éditeur |  | Anthropic | GitHub / Microsoft | 
| Licence | Open source (Apache 2.0) | Propriétaire | Propriétaire | 
| Interface | Terminal | Terminal | IDE + terminal (Copilot CLI) | 
| Palier gratuit | 1 000 requêtes/jour (compte Google) | Limité / essai | Palier gratuit plafonné | 
| Contexte | 1 M tokens | Étendu | Selon le modèle | 
| Support MCP | Oui, natif | Oui | Oui | 

Si vous hésitez encore entre écosystèmes, notre comparatif Claude Code vs Cursor et notre tutoriel Claude Code en 12 étapes apportent un éclairage complémentaire. Gemini CLI reste toutefois le point d’entrée le plus accessible pour tester l’IA agentique sans engagement financier.

## Prérequis et versions nécessaires

Avant de lancer l’installation, assurez-vous de réunir les prérequis suivants. Gemini CLI est distribué sous forme de paquet npm ; il exige donc un environnement Node.js à jour. La contrainte la plus stricte, inscrite dans le fichier `package.json` officiel, est **Node.js version 20.0.0 ou supérieure**. Nous recommandons une version LTS active (Node 22 ou Node 24) pour bénéficier des correctifs de sécurité et de meilleures performances.

| Composant | Version minimale | Recommandé (juin 2026) | Rôle | 
|---|---|---|---|
| Node.js | 20.0.0 | 22 LTS ou 24 LTS | Environnement d’exécution | 
| npm | 10.x | Fourni avec Node LTS | Installation du paquet | 
| Système | Windows 10, macOS 12, Linux | Windows 11 / WSL 2, macOS 14+, Ubuntu 22.04+ | Plateforme hôte | 
| Compte Google | Gratuit | Compte personnel ou Workspace | Authentification OAuth | 
| Docker / Podman | Optionnel | Dernière version stable | Sandbox (isolation des outils) | 
| Git | Optionnel | 2.40+ | Filtrage des fichiers ignorés | 

Côté authentification, deux chemins s’offrent à vous : un **compte Google** (méthode par défaut, la plus simple) ou une **clé API Gemini** obtenue sur Google AI Studio. Nous couvrirons les deux. Aucune carte bancaire n’est requise pour démarrer. Enfin, pour le mode sandbox – que nous verrons à l’étape 11 – un moteur de conteneurs comme Docker ou Podman est nécessaire ; il reste facultatif tant que vous ne demandez pas d’isolation renforcée. Si vous travaillez déjà avec des modèles locaux, notre guide Ollama : LLM en local montre une approche 100 % hors-ligne complémentaire.

## Étape 1 – Installer Node.js et vérifier l’environnement

Gemini CLI refusera de démarrer avec une version de Node.js antérieure à la 20. Commencez donc par contrôler votre installation. Ouvrez un terminal et exécutez les commandes de vérification. Si `node -v` renvoie une version inférieure à 20 (ou une erreur « command not found »), installez une version LTS via le gestionnaire de versions `nvm`, qui évite les problèmes de permissions liés à l’installation globale.

```
# Vérifier les versions actuelles
node -v    # doit afficher v20.x ou supérieur
npm -v
# Installer nvm (Linux / macOS), puis Node 22 LTS
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
# Recharger le shell, puis :
nvm install 22
nvm use 22
nvm alias default 22
# Contrôle final
node -v    # v22.x.x
```
Sous Windows, deux options fiables : installer Node.js LTS depuis le site officiel nodejs.org, ou travailler dans **WSL 2** (sous-système Windows pour Linux) et suivre les instructions Linux ci-dessus. WSL 2 est souvent l’expérience la plus fluide, car Gemini CLI exécute des commandes shell POSIX. Une fois `node -v` confirmé à la version 22 ou 24, votre environnement est prêt.

## Étape 2 – Installer Gemini CLI (npx, npm, Homebrew)

Il existe quatre façons d’installer ou de lancer Gemini CLI. La plus rapide pour un premier essai est `npx`, qui exécute le paquet sans installation permanente. Pour un usage quotidien, préférez l’installation globale via npm, ou Homebrew sur macOS et Linux. Choisissez la méthode adaptée à votre plateforme.

```
# Méthode 1 – Sans installation (essai immédiat)
npx @google/gemini-cli
# Méthode 2 – Installation globale via npm (usage quotidien)
npm install -g @google/gemini-cli
# Méthode 3 – Homebrew (macOS / Linux)
brew install gemini-cli
# Méthode 4 – MacPorts
sudo port install gemini-cli
# Vérifier l'installation
gemini --version
```
Une fois installé globalement, la commande `gemini` devient disponible partout dans votre terminal. Si `gemini --version` renvoie un numéro de version, l’installation est réussie. Gemini CLI propose trois **canaux de publication** : *stable* (recommandé pour la majorité des utilisateurs), *preview* (fonctionnalités en avance de phase) et *nightly* (build quotidien, pour tester les nouveautés). Par défaut, npm installe le canal stable.

**Astuce professionnelle :** si vous rencontrez une erreur de permission `EACCES` lors de l’installation globale sous Linux ou macOS, ne la contournez pas avec `sudo`. Utilisez plutôt `nvm` (étape 1), qui installe les paquets dans votre répertoire utilisateur et supprime définitivement ce type d’erreur. C’est le piège numéro un des débutants.

## Étape 3 – Premier lancement et authentification Google

Lancez maintenant l’agent en tapant simplement `gemini` dans un dossier de projet. Au premier démarrage, l’assistant vous propose de choisir un thème visuel, puis une méthode d’authentification. Sélectionnez **« Se connecter avec Google »** (Login with Google) : votre navigateur s’ouvre automatiquement, vous vous authentifiez avec votre compte Google, et la CLI stocke localement un jeton OAuth qu’elle rafraîchit ensuite toute seule.

```
# Se placer dans un projet, puis lancer l'agent
cd mon-projet
gemini
# Au premier lancement :
#  1. Choix du thème (ex. Default Dark)
#  2. Choix de l'authentification -> "Login with Google"
#  3. Le navigateur s'ouvre pour valider le compte
#  4. Retour au terminal : l'agent est prêt
```
Jusqu’au 18 juin 2026, cette méthode débloquait un **palier entièrement gratuit** de 60 requêtes par minute et 1 000 requêtes par jour. Depuis cette date, Google réserve ce niveau de service aux abonnés **Google AI Pro** (19,99 $/mois pour 1 500 requêtes par jour) ou **Ultra**, avec accès à la famille de modèles Gemini (routage automatique vers Gemini 3, principalement Flash pour les tâches courantes) et à la fenêtre d’un million de tokens. Pour un usage individuel intensif, l’abonnement Pro reste largement suffisant. Aucune configuration de clé n’est nécessaire dans ce mode, seule la connexion à un compte Google abonné.

Vous voyez apparaître l’invite de l’agent. Testez-la immédiatement avec une question simple, par exemple : *« Résume l’arborescence de ce projet et identifie le point d’entrée principal. »* L’agent va lister les fichiers, lire les plus pertinents, puis vous répondre. Si vous travaillez sur une machine distante en SSH sans navigateur, l’ouverture automatique échouera : dans ce cas, utilisez plutôt l’authentification par clé API décrite à l’étape suivante.

## Étape 4 – Authentification par clé API et Vertex AI

La connexion Google convient à la plupart des cas, mais trois situations imposent une **clé API** : les serveurs sans navigateur (CI/CD, machines distantes), le besoin de quotas payants plus élevés, et l’accès garanti aux modèles Pro. Rendez-vous sur Google AI Studio pour générer une clé, puis exportez-la dans la variable d’environnement `GEMINI_API_KEY`.

```
# Exporter la clé pour la session courante
export GEMINI_API_KEY="votre_cle_api_ici"
# Persistance recommandée : un fichier .env chargé automatiquement
# Ordre de priorité : ./.gemini/.env  >  ~/.gemini/.env  >  ~/.env
mkdir -p .gemini
echo 'GEMINI_API_KEY=votre_cle_api_ici' > .gemini/.env
# Variante entreprise : Vertex AI (régions UE possibles)
export GOOGLE_API_KEY="votre_cle_api_ici"
export GOOGLE_GENAI_USE_VERTEXAI=true
```
Gemini CLI charge automatiquement les fichiers `.env` selon un ordre de priorité précis : d’abord `./.gemini/.env` (spécifique au projet), puis `~/.gemini/.env`, puis `~/.env`. Cette hiérarchie permet d’isoler des clés différentes par projet. Le palier gratuit de la clé API reste plus restreint que l’ancien accès Google – environ 10 requêtes par minute et 250 par jour, limité aux modèles Flash – mais l’activation de la facturation débloque des débits bien supérieurs : Gemini 3.1 Pro Preview est ainsi facturé environ 2,00 $ par million de tokens en entrée (jusqu’à 200k tokens) depuis juillet 2026 selon Tembo, tandis que la formule Code Assist Enterprise grimpe à 54 $ par utilisateur et par mois pour 2 000 requêtes quotidiennes.

| Méthode | Débit gratuit | Quota gratuit / jour | Modèles | Cas d’usage | 
|---|---|---|---|---|
| Connexion Google (OAuth) | 60 req/min | 1 000 requêtes | Famille Gemini (routage auto) | Poste de travail individuel | 
| Clé API – palier gratuit | 10 req/min | 250 requêtes | Flash uniquement | Serveurs, scripts légers | 
| Clé API – facturation activée | Élevé | À l’usage (par token) | Gemini 3 Pro & Flash | Production, CI/CD | 
| Vertex AI | Selon quota GCP | Selon contrat | Gemini 3 Pro & Flash | Entreprise, RGPD / UE | 

**Sécurité :** ne validez jamais un fichier `.env` contenant votre clé dans Git. Ajoutez `.gemini/.env` et `.env` à votre `.gitignore` dès la création du projet. Une clé API exposée sur un dépôt public peut être exploitée en quelques minutes.

## Étape 5 – Vos premières commandes et le mode agent

Vous dialoguez avec Gemini CLI en langage naturel. Mais trois syntaxes spéciales décuplent sa puissance. Le symbole `@` injecte le contenu d’un fichier dans votre requête (avec filtrage des fichiers ignorés par Git). Le point d’exclamation `!` exécute une commande shell ou bascule en mode shell interactif – un mode devenu activé par défaut depuis la version **v0.9.0**, publiée le 15 octobre 2025. Enfin, les **commandes slash** (préfixées par `/`) pilotent la session elle-même.

```
# Poser une question en langage naturel
> Explique le rôle du fichier src/server.js et repère les failles potentielles.
# Injecter un fichier précis dans le contexte avec @
> @src/auth.js  Réécris cette fonction pour utiliser async/await.
# Injecter tout un dossier
> @src/  Dresse la liste des dépendances circulaires.
# Exécuter une commande shell avec !
> !npm test
# Basculer en mode shell (les commandes suivantes sont shell)
> !
# Lister les outils disponibles
> /tools
```
Lorsque l’agent décide d’utiliser un outil susceptible de modifier votre système – écrire un fichier, lancer une commande – il vous demande une **confirmation** avant d’agir. Vous pouvez approuver une fois, approuver pour toute la session, ou refuser. Ce garde-fou est essentiel : il vous garde aux commandes. Voici un exemple de sortie typique lorsqu’on demande à l’agent de créer un fichier.

```
> Crée un fichier README.md décrivant ce projet.
✦ Je vais analyser le projet puis rédiger le README.
  [outil] ReadFolder  .          -> 14 éléments
  [outil] ReadFile    package.json
  [outil] WriteFile   README.md   (nouveau fichier)
  Confirmer l'écriture de README.md ? [O]ui / [T]oujours / [N]on
> O
✦ README.md créé (48 lignes). Souhaitez-vous que j'ajoute un badge de build ?
```
Prenez le réflexe de taper `/help` pour afficher l’aide contextuelle et `/tools` pour voir la liste des outils intégrés : recherche Google, lecture et écriture de fichiers, exécution shell, récupération de pages web et mémoire. Ces outils sont le cœur de l’autonomie de l’agent.

## Étape 6 – Créer un fichier de contexte GEMINI.md

Le fichier `GEMINI.md` est la clé d’un agent réellement utile. Il s’agit d’un fichier de **contexte persistant** – des instructions que Gemini CLI charge automatiquement à chaque session pour comprendre votre projet : conventions de code, architecture, commandes de build, style attendu. C’est l’équivalent d’un onboarding permanent pour votre assistant.

La façon la plus rapide d’en générer un est la commande `/init` : l’agent analyse le dossier courant et produit un `GEMINI.md` sur mesure. Vous pouvez ensuite l’affiner à la main. Le chargement est **hiérarchique** : Gemini CLI fusionne un fichier global (`~/.gemini/GEMINI.md`), un fichier à la racine du projet, et d’éventuels fichiers dans les sous-dossiers, du plus général au plus spécifique.

```
# Générer automatiquement un fichier de contexte
> /init
# Exemple de GEMINI.md rédigé à la main
# ---------------------------------------------
# Projet : API de facturation (Node.js / Express 5)
#
# ## Conventions
# - TypeScript strict, pas de "any".
# - Tests avec Vitest ; couvrir chaque route.
# - Messages de commit en français, format Conventional Commits.
#
# ## Commandes
# - Installer : npm ci
# - Développer : npm run dev
# - Tester : npm test
# - Lint : npm run lint
#
# ## Architecture
# - src/routes  : points d'entrée HTTP
# - src/services: logique métier
# - src/db      : accès Postgres via le client pg
#
# ## À NE PAS FAIRE
# - Ne jamais logger de données personnelles (RGPD).
# - Ne pas modifier les migrations déjà appliquées.
```
Gérez ces fichiers avec la commande `/memory` : `/memory show` affiche le contexte chargé, `/memory refresh` le recharge après modification. Un bon `GEMINI.md` transforme radicalement la qualité des réponses : l’agent respecte vos conventions, utilise vos commandes et évite les pièges spécifiques à votre code. C’est l’investissement de cinq minutes le plus rentable de tout ce tutoriel.

## Étape 7 – Maîtriser les commandes slash essentielles

Les commandes slash contrôlent la session sans interrompre votre flux de travail. Il en existe une quarantaine, mais une douzaine couvre 95 % des besoins quotidiens. Mémorisez-les : elles vous feront gagner un temps considérable, notamment pour gérer le contexte (et donc votre consommation de tokens) sur les longues sessions.

| Commande | Effet | 
|---|---|
| `/help` | Affiche l’aide et la liste des commandes disponibles | 
| `/tools` | Liste les outils intégrés (fichiers, shell, recherche, web) | 
| `/memory` | Gère le contexte issu des fichiers GEMINI.md (show / refresh / add) | 
| `/init` | Analyse le dossier et génère un fichier de contexte adapté | 
| `/model` | Affiche et change le modèle Gemini utilisé | 
| `/mcp` | Gère les serveurs MCP (liste, authentification, rechargement) | 
| `/compress` | Remplace le contexte par un résumé pour économiser des tokens | 
| `/restore` | Restaure les fichiers à leur état avant une action de l’agent | 
| `/rewind` | Revient en arrière dans l’historique de la conversation | 
| `/stats` | Affiche les statistiques de session, de modèle et d’outils | 
| `/chat` | Sauvegarde, liste et reprend des points de contrôle de conversation | 
| `/clear` | Efface l’écran et l’historique visible de la session | 
| `/theme` | Change le thème visuel de l’interface | 
| `/quit` | Quitte Gemini CLI | 

Deux réflexes à adopter. Sur une session longue qui commence à ralentir, tapez `/compress` : l’agent remplace tout l’historique par un résumé compact, ce qui libère de la fenêtre de contexte et réduit la consommation. Et pour repartir sur un sujet totalement différent, `/clear` remet la session à zéro. Ces deux commandes évitent l’écueil du contexte saturé, principale cause de réponses lentes ou hors-sujet.

## Étape 8 – Changer de modèle : Gemini 3 Pro contre Flash

En 2026, Gemini CLI donne accès à deux familles de modèles : **Gemini 3 Pro**, taillé pour le raisonnement complexe et le code difficile, et **Gemini 3 Flash**, plus rapide et plus économe, idéal pour les tâches répétitives. Par défaut, l’agent utilise un mode « Auto » qui route intelligemment vos requêtes vers le modèle adapté. Vous pouvez toutefois forcer un choix.

```
# Choisir le modèle de façon interactive
> /model
# Forcer Gemini 3 Pro au lancement (drapeau -m / --model)
gemini -m gemini-3.1-pro-preview
# Lancer directement une requête sur un modèle donné
gemini -m gemini-3.1-pro-preview -p "Analyse la complexité de src/parser.ts"
```
Quand privilégier chaque modèle ? Réservez **Pro** aux refactorisations d’envergure, au débogage d’algorithmes ardus, à la conception d’architecture et à l’analyse de code volumineux où la profondeur de raisonnement fait la différence. Basculez sur **Flash** pour la génération de tests unitaires, la rédaction de documentation, les commits, les petites corrections et tout travail par lots où la vitesse et le coût priment. Le tableau ci-dessous résume l’arbitrage.

| Aspect | Gemini 3 Pro | Gemini 3 Flash | 
|---|---|---|
| Point fort | Raisonnement profond, code complexe | Vitesse, coût réduit | 
| Latence | Plus élevée | Faible | 
| Accès gratuit | Selon quota et routage | Inclus dans tous les paliers | 
| Cas idéal | Refactor, architecture, débogage | Tests, docs, tâches par lots | 
| Contexte | 1 M tokens | 1 M tokens | 

Notez que la disponibilité exacte des modèles Pro sur le palier gratuit dépend du routage et des quotas en vigueur, susceptibles d’évoluer. Pour un accès garanti et prioritaire aux modèles Pro, activez la facturation sur votre clé API ou passez par Vertex AI. Pour une comparaison des modèles côté API, consultez la documentation officielle des modèles Gemini.

## Étape 9 – Étendre Gemini CLI avec les serveurs MCP

Le **Model Context Protocol (MCP)** est un standard ouvert qui connecte les agents IA à des outils et sources de données externes : bases de données, API, systèmes de fichiers, dépôts Git, services tiers. Gemini CLI supporte MCP nativement. En déclarant un serveur MCP, vous donnez à l’agent de nouvelles capacités sans modifier son code. C’est le mécanisme d’extension le plus puissant de l’outil.

La configuration se fait dans un fichier `settings.json`. Trois emplacements existent, du plus général au plus spécifique : `~/.gemini/settings.json` (utilisateur), `.gemini/settings.json` (projet) et une configuration système. Ajoutez un bloc `mcpServers` décrivant chaque serveur par sa commande de lancement, ses arguments et ses variables d’environnement.

```
// .gemini/settings.json – déclaration de serveurs MCP
{
  "theme": "Default Dark",
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "./data"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "$GITHUB_TOKEN"
      }
    }
  }
}
```
Après avoir enregistré le fichier, relancez Gemini CLI ou tapez `/mcp` pour vérifier que les serveurs sont bien chargés et voir les outils qu’ils exposent. L’agent peut désormais, par exemple, lire des fichiers d’un dossier autorisé via le serveur *filesystem*, ou ouvrir une pull request via le serveur *github*. Pour découvrir l’écosystème complet de connecteurs, référez-vous à la spécification officielle du Model Context Protocol.

**Bonne pratique :** n’ajoutez que des serveurs MCP de confiance et accordez-leur le périmètre minimal (un dossier précis plutôt que tout le disque). Un serveur MCP mal configuré peut donner à l’agent un accès trop large. Le principe du moindre privilège s’applique ici comme partout ailleurs en sécurité.

## Étape 10 – Automatiser : mode non-interactif et scripts

Gemini CLI n’est pas réservé aux sessions interactives. Le **drapeau `-p`** (pour *prompt*) exécute une requête unique puis rend la main, ce qui le rend parfaitement scriptable. Vous pouvez ainsi l’intégrer dans un pipeline CI/CD, un hook Git ou une tâche planifiée. La sortie s’imprime sur la sortie standard, prête à être redirigée ou traitée.

```
# Requête unique, sortie sur stdout
gemini -p "Génère un message de commit à partir du diff suivant"
# Alimenter l'agent via un pipe
git diff --staged | gemini -p "Rédige un message de commit Conventional Commits"
# Exemple de script : générer des notes de version
#!/usr/bin/env bash
set -euo pipefail
LAST_TAG=$(git describe --tags --abbrev=0)
git log "${LAST_TAG}..HEAD" --pretty=format:'%s' \
  | gemini -p "Résume ces commits en notes de version claires, en français" \
  > CHANGELOG_DRAFT.md
echo "Brouillon de changelog écrit dans CHANGELOG_DRAFT.md"
```
Ce mode headless ouvre d’immenses possibilités d’automatisation : revue de code automatique sur chaque pull request, génération de documentation à la volée, triage d’issues, ou encore rédaction de tests à partir d’une spécification. Combinez-le avec une clé API et la facturation activée pour disposer de quotas fiables en environnement d’intégration continue, où les limites du palier gratuit seraient rapidement atteintes.

Pour aller plus loin, Gemini CLI propose une intégration **GitHub Actions** via la commande `/setup-github`, qui configure des workflows de triage d’issues et de revue de pull requests directement dans votre dépôt. C’est la manière la plus simple de faire travailler l’agent en continu sur votre projet, sans intervention manuelle.

## Étape 11 – Sécurité : sandbox, dossiers de confiance et checkpointing

Un agent qui exécute des commandes shell et modifie des fichiers doit être encadré. Gemini CLI intègre trois garde-fous complémentaires. Le premier est le **sandbox** : avec le drapeau `--sandbox` (ou `-s`), les outils potentiellement dangereux s’exécutent dans un conteneur isolé (Docker ou Podman), ce qui protège votre système hôte des effets de bord. Activez-le dès que vous laissez l’agent travailler de façon autonome.

```
# Lancer l'agent avec isolation par conteneur
gemini --sandbox
# Ou via une variable d'environnement
export GEMINI_SANDBOX=true
gemini
# Passer en mode plan (lecture seule) : l'agent propose sans exécuter
> /plan
# Restaurer les fichiers à l'état d'avant la dernière action
> /restore
```
Le deuxième garde-fou est le système de **dossiers de confiance** : Gemini CLI vous demande d’autoriser explicitement un dossier avant d’y opérer, et gère des permissions persistantes. Le troisième est le **checkpointing** : avant toute modification de fichier, l’agent crée un point de restauration. Les commandes `/restore` et `/rewind` vous permettent d’annuler une action ou de revenir en arrière dans la conversation, comme un « ctrl-Z » pour votre agent.

Enfin, le **mode plan** (`/plan`) fait passer l’agent en lecture seule : il élabore et vous présente un plan d’action sans rien exécuter. C’est idéal pour valider une stratégie avant de laisser l’agent agir. À l’inverse, méfiez-vous du mode « YOLO » (auto-approbation de tous les outils) : commode pour les démonstrations, il est dangereux sur du code réel. Ne l’utilisez jamais hors d’un environnement jetable et isolé.

## Étape 12 – Projet complet : auditer et documenter un dépôt

Assemblons maintenant tout ce que vous avez appris en un **projet complet et fonctionnel** : transformer Gemini CLI en agent d’audit et de documentation pour n’importe quel dépôt. L’objectif : obtenir un rapport de qualité de code, un fichier de contexte, et une documentation générée, en une seule session encadrée.

```
# 1. Se placer dans le dépôt et lancer l'agent en sandbox
cd mon-depot
gemini --sandbox
# 2. Générer le contexte projet
> /init
# 3. Passer en mode plan pour cadrer l'audit (lecture seule)
> /plan
> Audite ce dépôt : dépendances obsolètes, code mort, risques de sécurité,
  couverture de tests. Ne modifie rien, propose un plan priorisé.
# 4. Valider le plan, puis demander l'exécution ciblée
> Applique uniquement les points 1 et 3 du plan. Confirme chaque écriture.
# 5. Générer la documentation
> @src/  Rédige un fichier docs/ARCHITECTURE.md décrivant les modules
  et leurs interactions, en français.
# 6. Vérifier le résultat, puis committer
> !npm test
> git diff --staged | gemini -p "Rédige un message de commit détaillé"
```
Voici un extrait de la sortie attendue lors de la phase d’audit. Remarquez comment l’agent enchaîne les outils – lecture de l’arborescence, analyse des dépendances, recherche web pour vérifier une CVE – avant de produire un rapport structuré et priorisé.

```
✦ Audit du dépôt en cours...
  [outil] ReadFolder   src/           -> 27 fichiers
  [outil] ReadFile     package.json
  [outil] GoogleSearch "CVE express 4 vulnérabilité 2026"
  [outil] Shell        npm outdated
  Rapport d'audit (priorisé)
  ──────────────────────────
  1. [CRITIQUE] 2 dépendances avec CVE connues -> mettre à jour
  2. [MAJEUR]   Couverture de tests : 41 % (cible conseillée : 80 %)
  3. [MINEUR]   3 fonctions non utilisées dans src/utils/
  4. [INFO]     Aucun fichier LICENSE détecté
  Souhaitez-vous que je traite les points 1 et 3 ? [O/N]
```
Vous disposez désormais d’un workflow réutilisable, transférable d’un projet à l’autre. En sauvegardant votre `GEMINI.md` et votre `settings.json` dans un dépôt de configuration, vous reproduisez ce comportement partout. C’est là toute la force d’un agent open source et scriptable : il devient une brique d’infrastructure, pas seulement un gadget de démonstration.

## 5 pièges courants à éviter avec Gemini CLI

La plupart des frustrations rencontrées avec Gemini CLI proviennent d’une poignée d’erreurs évitables. Les connaître à l’avance vous fera gagner des heures.

1. **Version de Node.js trop ancienne.** En dessous de la 20, l’installation ou le lancement échoue avec des erreurs obscures. Vérifiez toujours`node -v` et passez par`nvm` pour une version LTS.
2. **Contourner les permissions npm avec sudo.** L’erreur`EACCES` pousse les débutants à installer en root, ce qui casse ensuite les mises à jour. Utilisez`nvm` ou`npx` à la place.
3. **Saturer le contexte avec `@`.** Injecter`@./` sur un énorme dépôt consomme des centaines de milliers de tokens d’un coup, ralentit l’agent et épuise le quota. Ciblez des dossiers précis et utilisez`/compress` .
4. **Committer sa clé API.** Un fichier`.env` ou`.gemini/.env` versionné expose votre clé. Ajoutez-les au`.gitignore` dès le départ.
5. **Laisser l’agent agir sans sandbox ni confirmation.** Le mode auto-approbation (« YOLO ») sur du code de production peut supprimer ou écraser des fichiers. Gardez les confirmations actives et activez`--sandbox` .

## Dépannage : 8 problèmes fréquents et leurs solutions

Voici les incidents les plus signalés par la communauté et la façon de les résoudre rapidement. Conservez ce tableau à portée de main.

| Problème | Cause probable | Solution | 
|---|---|---|
| `gemini: command not found` | Binaire npm global absent du PATH | Réinstaller globalement, ou utiliser `npx @google/gemini-cli` | 
| Le navigateur ne s’ouvre pas (SSH) | Machine distante sans interface graphique | Basculer sur l’authentification par `GEMINI_API_KEY` | 
| Erreur 429 / quota dépassé | Limite de requêtes atteinte | Attendre, utiliser `/compress` , ou passer à une clé API facturée | 
| Blocage derrière un proxy d’entreprise | Trafic HTTPS filtré | Définir `HTTPS_PROXY` et`HTTP_PROXY` | 
| Erreur de version Node | Node < 20 | `nvm install 22 && nvm use 22` | 
| Serveur MCP absent de `/mcp` | Chemin ou syntaxe `settings.json` erronés | Vérifier le JSON, relancer la CLI, tester `/mcp` | 
| « docker not found » en sandbox | Moteur de conteneurs non installé | Installer Docker/Podman, ou désactiver le sandbox | 
| Réponses lentes ou hors-sujet | Contexte saturé | `/compress` puis`/clear` ; réduire la portée de`@` | 

Deux conseils transverses. Si un comportement vous semble anormal, tapez `/about` pour connaître votre version exacte et `/bug` pour ouvrir un rapport sur GitHub avec le contexte pré-rempli. Et pensez à mettre à jour régulièrement : `npm update -g @google/gemini-cli`. Le rythme de publication est soutenu depuis le lancement – la CLI avait déjà atteint la v0.7.0 dès le 22 septembre 2025 au fil des mises à jour hebdomadaires, puis la v0.20.0 du 1er décembre 2025 avait ajouté le glisser-déposer multi-fichiers – et de nouveaux correctifs continuent d’arriver chaque semaine sur le canal stable.

## Astuces avancées pour aller plus loin

Une fois les bases maîtrisées, plusieurs fonctionnalités avancées vous distingueront. Elles transforment Gemini CLI d’un assistant ponctuel en un véritable copilote d’ingénierie sur mesure.

- **Commandes slash personnalisées.** Créez vos propres commandes dans des fichiers`.toml` placés dans`~/.gemini/commands` (globales) ou`.gemini/commands` (projet). Une commande`/test` peut par exemple encapsuler un prompt de génération de tests complet, réutilisable en une frappe.
- **Sous-agents et compétences (skills).** Les commandes`/agents` et`/skills` permettent de déléguer des tâches spécialisées à des agents dédiés et de charger des « compétences » réutilisables, pour orchestrer des workflows complexes.
- **Intégration IDE.** Via`/ide` , connectez Gemini CLI à VS Code pour partager le contexte de l’éditeur (fichiers ouverts, sélection) avec l’agent du terminal.
- **Gestion fine du contexte.** Sur les très longues sessions, alternez`/compress` (résumé) et`/chat save` (point de contrôle nommé) pour reprendre exactement là où vous vous étiez arrêté.
- **Mode plan systématique.** Prenez l’habitude de démarrer toute tâche risquée par`/plan` : vous validez la stratégie avant l’exécution, un réflexe qui évite bien des mauvaises surprises.

Ces mécanismes d’extension – commandes `.toml`, MCP, sous-agents, hooks de cycle de vie – font de Gemini CLI une plateforme d’automatisation plutôt qu’un simple outil. Les équipes les plus avancées versionnent leur configuration complète (`GEMINI.md`, `settings.json`, commandes personnalisées) dans un dépôt partagé, garantissant un comportement homogène de l’agent sur tous les postes. Pour une vue d’ensemble de l’écosystème des outils de codage IA, notre dossier sur le boom du vibe coding en 2026 replace Gemini CLI dans le paysage plus large.

## Gemini CLI face à la concurrence : quel agent choisir ?

Gemini CLI n’évolue pas seul. Le marché des agents de codage en terminal s’est densifié en 2025-2026, avec Claude Code d’Anthropic, GitHub Copilot (et son interface CLI), Cursor côté éditeur, ou encore des solutions open source comme Cline et Aider. Comment se situe l’outil de Google ?

Sa force cardinale reste le **rapport puissance-prix** : aucun concurrent n’offre un palier gratuit aussi généreux (1 000 requêtes par jour, un million de tokens de contexte) doublé d’un code entièrement ouvert. Pour un étudiant, un indépendant ou une startup européenne qui débute avec l’IA agentique, c’est un point d’entrée imbattable. La nature open source et la compatibilité Vertex AI en font aussi un choix rationnel pour les organisations soucieuses de gouvernance et de souveraineté des données.

En contrepartie, certains développeurs jugent Claude Code plus abouti sur les tâches d’ingénierie logicielle très complexes, tandis que Cursor séduit ceux qui préfèrent une expérience intégrée à l’éditeur. La bonne approche consiste souvent à **combiner les outils** : Gemini CLI pour l’exploration, l’automatisation et le travail par lots à coût nul ; un agent premium pour les refactorisations critiques. Nos guides Cursor en 12 étapes et GitHub Copilot en 12 étapes vous aideront à monter en compétence sur les alternatives et à composer votre propre pile.

### À lire également sur Tech Insider

## Foire aux questions sur Gemini CLI

### Gemini CLI est-il vraiment gratuit ?

Il l’a été jusqu’au 18 juin 2026 : la connexion avec un compte Google personnel donnait alors 60 requêtes par minute et 1 000 requêtes par jour sans carte bancaire, avec une fenêtre de contexte d’un million de tokens. Depuis cette date, Google réserve ce niveau de service aux formules payantes Google AI Pro (19,99 $/mois, 1 500 requêtes par jour) et Ultra ; les comptes Google personnels gratuits ne donnent plus un accès complet. Le code de l’outil reste toutefois open source (Apache 2.0). Pour des quotas plus élevés ou un accès prioritaire aux modèles Pro, vous pouvez activer la facturation sur une clé API (Gemini 3.1 Pro Preview facturé environ 2,00 $ par million de tokens en entrée) ou passer par Vertex AI / Code Assist Enterprise (54 $/utilisateur/mois, 2 000 requêtes par jour).

### Quelle version de Node.js faut-il pour Gemini CLI ?

La version minimale requise est Node.js 20.0.0. Nous recommandons une version LTS active (Node 22 ou 24) pour la stabilité et la sécurité. Le gestionnaire `nvm` est la façon la plus simple d’installer et de basculer entre versions sans problème de permissions.

### Quelle différence entre Gemini CLI et Gemini Code Assist ?

Gemini CLI est un agent autonome qui vit dans le terminal et exécute des actions (fichiers, shell, web). Gemini Code Assist est l’assistant intégré aux IDE (complétion, chat en contexte d’édition). Les deux partagent la technologie Gemini et peuvent se compléter ; Gemini CLI se distingue par son autonomie agentique et sa scriptabilité.

### Mes données de code sont-elles envoyées à Google ?

Les requêtes sont traitées par les modèles Gemini côté Google. Les conditions de confidentialité varient selon le mode : compte gratuit, clé API payante ou Vertex AI (qui offre des garanties de traitement en entreprise, avec choix de régions, y compris dans l’UE). Pour un projet sensible, privilégiez Vertex AI et consultez les politiques applicables via la commande `/privacy`.

### Gemini CLI fonctionne-t-il sous Windows ?

Oui, nativement via Node.js pour Windows, mais l’expérience la plus fluide passe souvent par WSL 2, car l’agent exécute des commandes shell de type POSIX. Installez Node 22 LTS dans votre distribution WSL et suivez les instructions Linux de ce tutoriel.

### Comment empêcher l’agent de modifier des fichiers sans mon accord ?

Par défaut, Gemini CLI demande une confirmation avant toute action à effet de bord. Ne désactivez pas ces confirmations (évitez le mode auto-approbation). Pour une sécurité renforcée, lancez l’agent avec `--sandbox` afin d’isoler l’exécution dans un conteneur, et utilisez `/plan` pour un mode lecture seule.

### Qu’est-ce que le protocole MCP et pourquoi l’utiliser ?

Le Model Context Protocol est un standard ouvert qui connecte l’agent à des outils et sources externes (bases de données, dépôts Git, API). En déclarant des serveurs MCP dans `settings.json`, vous étendez les capacités de Gemini CLI sans écrire de code. Vérifiez les serveurs chargés avec `/mcp`.

### Puis-je intégrer Gemini CLI à mes pipelines CI/CD ?

Absolument. Le mode non-interactif (`gemini -p "..."`) rend l’agent scriptable et intégrable dans n’importe quel pipeline. La commande `/setup-github` configure même des workflows GitHub Actions de revue et de triage. Pour une utilisation en CI, préférez une clé API avec facturation activée afin de disposer de quotas fiables.

## Conclusion : votre terminal devient agentique

En douze étapes, vous êtes passé d’un terminal muet à un environnement de développement piloté par l’IA. Vous savez installer Gemini CLI, l’authentifier de trois façons, écrire un fichier `GEMINI.md`, maîtriser les commandes slash, changer de modèle, brancher des serveurs MCP, automatiser vos scripts et sécuriser l’exécution. Surtout, vous disposez d’un projet complet réutilisable : un agent d’audit et de documentation prêt à l’emploi.

La force de Gemini CLI tient à sa combinaison unique : gratuité généreuse, code ouvert, contexte d’un million de tokens et extensibilité par MCP. Que vous soyez étudiant, indépendant ou ingénieur en entreprise, c’est aujourd’hui le moyen le plus accessible d’intégrer un agent IA à votre flux de travail quotidien. Le meilleur point de départ pour approfondir reste la page GitHub officielle du projet et la documentation en ligne. Lancez `gemini`, tapez `/init`, et laissez votre terminal travailler pour vous.
