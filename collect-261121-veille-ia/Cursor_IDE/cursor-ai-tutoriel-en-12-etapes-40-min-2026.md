---
id: collect-261121-veille-ia/cursor-ide/cursor-ai-tutoriel-en-12-etapes-40-min-2026
title: "Cursor AI : Tutoriel en 12 Étapes, 40 Min [2026]"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["cursor", "code", "milliards", "lediteur", "dollars", "selon", "juin", "version"]
source: https://tech-insider.org/fr/cursor-tutoriel-12-etapes-2026/
source_anchor: ""
source_lines: [1, 412]
section: "Cursor_IDE"
---
**Cursor** s’est imposé en moins de trois ans comme l’éditeur de code doté d’intelligence artificielle le plus discuté de la planète. Édité par la start-up américaine **Anysphere**, il a atteint une valorisation de 9,9 milliards de dollars dès juin 2025 à la faveur d’une Série C de 900 millions de dollars (selon TechCrunch et Unite.AI), franchi la barre des 2 milliards de dollars de revenus annualisés début 2026, bouclé une Série D de 2,3 milliards de dollars le 13 novembre 2025 le valorisant à 29,3 milliards, puis engagé, selon CNBC en avril 2026, des discussions pour lever 2 milliards supplémentaires à plus de 50 milliards de dollars. Il revendique aujourd’hui environ un million d’utilisateurs, dont près de 300 000 payants fin 2025, qui produisent près d’un milliard de lignes de code chaque jour ; signe que l’élan dépasse désormais le poste de travail, l’application Cursor pour iOS est passée en bêta publique fin juin 2026 avec la version 3.9, une annonce qui a cumulé 3,9 millions de vues selon Explainx. Ce tutoriel **Cursor AI** vous accompagne, en 12 étapes et environ 40 minutes, de l’installation jusqu’à la livraison d’un projet complet.

Contrairement à une simple extension, **Cursor AI** est un environnement de développement (IDE) à part entière, dérivé de Visual Studio Code, mais repensé autour de l’IA. Depuis la sortie de **Cursor 2.0** le 29 octobre 2025, l’éditeur embarque son propre modèle maison, *Composer*, et une interface multi-agents capable d’orchestrer jusqu’à huit agents en parallèle ; la troisième génération majeure, **Cursor 3.0**, est arrivée le 2 avril 2026, suivie le 24 avril par la version 3.2 qui ajoute la commande `/multitask` et la prise en charge des espaces de travail multi-racines (multi-root workspaces), puis le 29 mai 2026 par la version 3.6, qui introduit un « Auto-review Run Mode » selon Rapid Dev ; le rythme des sorties ne faiblit pas puisqu’en juin 2026, la version 3.7 a introduit les agents cloud et des sous-agents `/in-cloud` selon Anycap.ai, suivie le 18 juin par la 3.8 et son nouveau skill `/automate` (Pondero.ai), puis le 22 juin par la 3.9, qui ajoute une page Customize et une Marketplace de règles et d’extensions (Anycap.ai). Nous allons exploiter toutes ces briques – Tab, édition en ligne, Chat, Agent, règles de projet, MCP et CLI – pour construire une véritable API de gestion de tâches.

Ce guide s’adresse aux développeurs de tous niveaux, du débutant curieux au professionnel qui veut industrialiser son usage de l’IA. Voici ce que vous saurez faire à la fin :

- Installer et configurer **Cursor** sur Windows, macOS ou Linux, et importer votre environnement VS Code.
- Utiliser les quatre modes clés (Tab, édition en ligne, Chat, Agent) et choisir le bon modèle selon la tâche.
- Cadrer l’IA avec des règles de projet, générer un code multi-fichiers et laisser l’agent tester et corriger seul.
- Étendre l’éditeur avec MCP, l’automatiser via Cursor CLI et sécuriser vos données en entreprise.

## Cursor en 2026 : l’éditeur IA à 9,9 milliards de dollars

**Cursor** est un « fork » de VS Code : vous retrouvez la même ergonomie, les mêmes raccourcis et la compatibilité avec la quasi-totalité des extensions, thèmes et paramètres de l’éditeur de Microsoft. La différence tient à la couche d’IA, profondément intégrée plutôt que greffée. Là où GitHub Copilot reste une extension d’autocomplétion, Cursor transforme l’éditeur en assistant capable de lire l’intégralité de votre base de code, de modifier plusieurs fichiers en même temps et d’exécuter des commandes dans le terminal.

Le tournant majeur remonte à **Cursor 2.0** (29 octobre 2025), qui a introduit deux nouveautés structurantes, comme l’a détaillé InfoWorld. D’abord **Composer**, le premier modèle de codage maison d’Anysphere : un modèle « mixture-of-experts » spécialisé par apprentissage par renforcement, annoncé comme quatre fois plus rapide que les modèles agentiques de niveau comparable, qui boucle la plupart de ses tours en moins de 30 secondes. Ensuite une interface pensée pour le travail multi-agents, où l’on peut lancer plusieurs agents sur des tâches distinctes et comparer leurs résultats. Composer a continué d’évoluer depuis : la version Composer 2.5, lancée le 18 mai 2026, a vu son usage doubler dès sa première semaine, selon LearnAgent, signe de l’appétit des développeurs pour ce modèle maison plus rapide. Le SDK de Cursor a lui aussi été enrichi en juin 2026, avec l’ajout de trois nouvelles primitives – magasins personnalisés (custom stores), outils et auto-révision (auto-review) –, selon JoinNextDev, de quoi faciliter l’intégration de Composer dans des workflows sur mesure.

Cette trajectoire explique l’engouement. Selon les chiffres relayés par la presse spécialisée, Anysphere est passée de 100 millions de dollars de revenus annualisés en janvier 2025 à 500 millions en juin, puis a franchi le milliard en novembre 2025 et les 2 milliards début 2026 ; la valorisation aurait grimpé jusqu’à environ 29,3 milliards de dollars fin 2025, à la faveur d’une Série D de 2,3 milliards de dollars bouclée le 13 novembre 2025, menée par Accel et Coatue selon SiliconANGLE, avec l’entrée de nouveaux investisseurs comme Nvidia et Google, selon Business Wire. Reuters a d’ailleurs souligné que cette valorisation avait presque triplé en cinq mois, passant d’environ 10 milliards de dollars à 29,3 milliards. Plus de la moitié des entreprises du classement Fortune 500 utiliseraient l’outil. Pour comprendre le contexte plus large de cet essor, notre analyse du phénomène du « vibe coding » en 2026 remet ces montants en perspective.

## Prérequis : versions, configuration système et comptes

Avant de lancer ce tutoriel **Cursor AI**, vérifiez que votre poste et vos comptes sont prêts. Cursor est disponible sur les trois grandes plateformes de bureau ; le projet fil rouge, lui, s’appuie sur Python. Voici la liste précise du matériel et des logiciels recommandés en 2026.

| Élément | Minimum | Recommandé | 
|---|---|---|
| Système (Windows) | Windows 10 64 bits | Windows 11 | 
| Système (macOS) | macOS 10.15 Catalina | macOS 13+ (Apple Silicon) | 
| Système (Linux) | Ubuntu 20.04 / .deb, .rpm, AppImage | Ubuntu 22.04+ | 
| Mémoire vive (RAM) | 8 Go | 16 Go ou plus | 
| Espace disque | ~1 Go pour l’éditeur | SSD, 5 Go libres | 
| Connexion Internet | Obligatoire (modèles cloud) | Fibre / haut débit stable | 
| Python (projet) | Python 3.10 | Python 3.12 | 
| Git | Git 2.30+ | Dernière version | 

Côté comptes, prévoyez une adresse e-mail (ou un compte GitHub / Google pour la connexion) et, idéalement, un dépôt Git vide pour versionner votre travail. Cursor fonctionne dès l’offre gratuite *Hobby*, mais l’essai des fonctions agentiques les plus lourdes est plus confortable avec la période d’essai Pro. Installez également **Python 3.12** et **Git** : nous laisserons l’agent créer l’environnement virtuel, mais les interpréteurs doivent être présents sur la machine.

## Le projet fil rouge : une API de gestion de tâches en FastAPI

Pour ne pas rester dans la théorie, nous allons construire un projet complet et fonctionnel : une **API REST de gestion de tâches** (« to-do ») écrite en Python avec le framework FastAPI, une persistance SQLite et une suite de tests automatisés. C’est un cas d’usage réaliste qui met en jeu toutes les capacités de **Cursor** : génération multi-fichiers, exécution du terminal, correction d’erreurs et rédaction de tests.

À l’issue des 12 étapes, vous disposerez d’une API exposant les opérations CRUD (créer, lire, mettre à jour, supprimer) sur des tâches, documentée automatiquement via OpenAPI, couverte par des tests `pytest`, et prête à être versionnée. L’arborescence cible ressemblera à ceci :

```
gestion-taches/
├── .cursor/
│   └── rules/
│       └── regles-projet.mdc     # règles de projet pour l'agent
├── .cursorignore                 # fichiers exclus de l'indexation
├── app/
│   ├── __init__.py
│   ├── main.py                   # point d'entrée FastAPI
│   ├── models.py                 # modèles Pydantic + SQLModel
│   └── database.py               # connexion SQLite
├── tests/
│   └── test_taches.py            # tests pytest
├── requirements.txt
└── README.md
```
## Étape 1 – Télécharger et installer Cursor

Rendez-vous sur le site officiel cursor.com et cliquez sur *Download*. Le site détecte votre système et propose l’installeur adapté. Vous pouvez aussi passer par un gestionnaire de paquets, plus pratique pour scripter l’installation ou la maintenir à jour.

```
# Windows (winget)
winget install Anysphere.Cursor
# macOS (Homebrew)
brew install --cask cursor
# Linux : télécharger l'AppImage depuis cursor.com, puis
chmod +x Cursor-*.AppImage
./Cursor-*.AppImage
```
Sous Windows, l’installeur classique `.exe` reste le plus simple : double-cliquez et suivez l’assistant. Sur macOS, glissez l’application dans le dossier *Applications*. Sous Linux, l’AppImage est la voie officielle, mais des paquets `.deb` et `.rpm` existent également. Au premier lancement, Cursor vérifie les mises à jour : l’éditeur suit un rythme de versions très soutenu (quasi hebdomadaire) depuis la série 2.x, aussi acceptez systématiquement les mises à jour proposées pour bénéficier des derniers modèles.

### Piège n°1 : ne pas confondre Cursor et l’extension VS Code

Cursor est un logiciel **autonome**, pas une extension à installer dans VS Code. Vous n’avez pas besoin d’avoir VS Code au préalable. Beaucoup de débutants cherchent « Cursor » dans la place de marché des extensions VS Code et ne trouvent rien : c’est normal, il faut télécharger l’application depuis cursor.com.

## Étape 2 – Importer VS Code, se connecter et choisir son modèle

Au premier démarrage, Cursor propose d’**importer votre configuration VS Code** : extensions, thèmes, raccourcis clavier et paramètres. Acceptez si vous venez de VS Code, l’expérience sera immédiatement familière. Connectez-vous ensuite avec GitHub, Google ou une adresse e-mail. La connexion est nécessaire car les modèles s’exécutent dans le cloud.

Ouvrez ensuite le sélecteur de modèle (en haut du panneau de discussion). En 2026, **Cursor** donne accès à un catalogue impressionnant de grands modèles de langage, que vous pouvez changer à la volée selon la tâche. Les principaux disponibles à la mi-2026 sont résumés ci-dessous.

| Fournisseur | Modèles disponibles dans Cursor (2026) | Usage type | 
|---|---|---|
| Anysphere (maison) | Composer 2, Composer 2.5 | Vitesse maximale, tâches agentiques | 
| Anthropic | Claude Opus 4.8, Claude Sonnet 5, Claude 4.7 Opus | Raisonnement, refactorisations complexes | 
| OpenAI | GPT-5.5, GPT-5.x Codex | Génération de code polyvalente | 
|  | Gemini 3.1 Pro, Gemini 3.5 Flash | Grands contextes, rapidité | 
| xAI | Grok 4.3 | Alternative généraliste | 
| Autres | GLM 5.2 (Z.ai), Kimi K2.5 (Moonshot) | Options open-weight / coût réduit | 

Notre conseil pratique : utilisez **Composer 2** (ou le mode « Auto ») pour les tâches courantes où la réactivité prime, et basculez sur **Claude Opus 4.8** ou **Claude Sonnet 5** pour les refactorisations délicates ou le débogage difficile. Pour approfondir les différences entre ces grands modèles, consultez notre comparatif Claude vs ChatGPT vs Gemini vs Mistral.

## Étape 3 – Maîtriser l’interface : Tab, Cmd+K, Chat et Agent

Quatre points d’entrée résument l’expérience **Cursor**. Les mémoriser dès le départ change radicalement votre productivité. Sur macOS, la touche *Cmd* remplace *Ctrl*.

- **Tab** – l’autocomplétion prédictive. Cursor ne complète pas seulement la ligne en cours, il prédit votre prochaine modification (y compris à plusieurs lignes) et vous laisse l’accepter d’une pression sur*Tab* .
- **Ctrl/Cmd + K** – l’édition en ligne. Sélectionnez du code, appuyez sur K, décrivez la modification en langage naturel : Cursor réécrit la sélection sur place.
- **Ctrl/Cmd + L** – le Chat. Un panneau latéral pour poser des questions sur votre code, coller des erreurs ou demander des explications.
- **Ctrl/Cmd + I** – l’Agent (Composer). Le mode le plus puissant : l’IA planifie, crée et modifie plusieurs fichiers, exécute des commandes et itère jusqu’au résultat.

Prenez cinq minutes pour ouvrir un fichier de test et essayer chacun de ces raccourcis. La bascule entre le mode *Ask* (Cursor répond sans rien modifier) et le mode *Agent* (Cursor agit sur le projet) se fait directement dans le panneau : c’est un réflexe essentiel pour garder la maîtrise de ce que fait l’outil.

## Étape 4 – Définir les règles du projet avec .cursor/rules

C’est l’étape que les débutants sautent, et c’est une erreur. Les **règles de projet** (Project Rules) sont des instructions persistantes qui orientent chaque réponse de l’IA dans votre dépôt. Elles évitent que l’agent parte dans tous les sens et réduisent drastiquement le nombre d’allers-retours.

L’ancienne approche reposait sur un fichier unique `.cursorrules` à la racine. Depuis 2025, la méthode recommandée utilise des fichiers `.mdc` (Markdown enrichi) placés dans `.cursor/rules/`, ce qui permet d’organiser des règles par type de fichier ou par contexte. Créez le fichier suivant :

```
# .cursor/rules/regles-projet.mdc
---
description: Règles pour l'API de gestion de tâches
alwaysApply: true
---
- Langage : Python 3.12, framework FastAPI.
- Utiliser SQLModel pour les modèles et la persistance SQLite.
- Typage strict : annoter tous les paramètres et retours de fonction.
- Écrire les commentaires et docstrings EN FRANÇAIS.
- Chaque endpoint doit renvoyer des codes HTTP explicites.
- Générer des tests pytest pour toute nouvelle route.
- Ne jamais coder en dur de secrets : utiliser des variables d'environnement.
```
Ajoutez également un fichier `.cursorignore` pour empêcher l’indexation de fichiers volumineux ou sensibles (l’équivalent d’un `.gitignore` pour l’IA). Cela accélère l’indexation et protège vos secrets.

```
# .cursorignore
.venv/
__pycache__/
*.db
.env
node_modules/
dist/
```
## Étape 5 – Générer le squelette du projet avec l’Agent

Place à la magie. Ouvrez l’Agent (*Ctrl/Cmd + I*), vérifiez que vous êtes bien en mode **Agent** et non *Ask*, puis saisissez une consigne précise. La qualité du résultat dépend directement de la clarté de la demande : décrivez le quoi, le comment et les contraintes.

```
Crée la structure d'une API REST de gestion de tâches avec FastAPI.
Contraintes :
- Utilise SQLModel + SQLite (fichier taches.db).
- Modèle Tache : id, titre (str), terminee (bool, défaut False),
  date_creation (datetime auto).
- Endpoints CRUD complets : POST /taches, GET /taches,
  GET /taches/{id}, PATCH /taches/{id}, DELETE /taches/{id}.
- Crée app/main.py, app/models.py, app/database.py, requirements.txt.
- Respecte les règles du projet (.cursor/rules).
```
Cursor affiche un **plan**, puis propose les fichiers à créer sous forme de diff. Vous validez ou refusez chaque modification. En quelques secondes, l’agent génère une base cohérente. Voici, par exemple, le type de code produit pour `app/models.py` :

```
# app/models.py
from datetime import datetime
from sqlmodel import SQLModel, Field
class Tache(SQLModel, table=True):
    """Modèle représentant une tâche à accomplir."""
    id: int | None = Field(default=None, primary_key=True)
    titre: str = Field(index=True)
    terminee: bool = Field(default=False)
    date_creation: datetime = Field(default_factory=datetime.utcnow)
class TacheCreation(SQLModel):
    """Schéma d'entrée pour créer une tâche."""
    titre: str
class TacheMaj(SQLModel):
    """Schéma d'entrée pour mettre à jour une tâche."""
    titre: str | None = None
    terminee: bool | None = None
```
**Piège n°2 :** ne validez jamais un gros diff sans le lire. Le mode Agent peut créer ou écraser plusieurs fichiers d’un coup. Parcourez chaque changement, surtout sur un projet existant, et utilisez le contrôle de version Git comme filet de sécurité.

## Étape 6 – Accélérer avec Tab et l’édition en ligne (Cmd+K)

Une fois le squelette en place, le travail au fil de l’eau s’appuie surtout sur **Tab** et **Ctrl/Cmd + K**. Ouvrez `app/main.py` et commencez à écrire une nouvelle route : après quelques caractères, Cursor propose souvent l’implémentation complète, que vous acceptez avec *Tab*. La fonction d’autocomplétion prédit aussi les éditions suivantes (import manquant, retour de type, gestion d’erreur).

Pour une modification ciblée, sélectionnez un bloc, appuyez sur *Ctrl/Cmd + K* et décrivez le changement. Exemples de consignes efficaces :

```
Ajoute une pagination (paramètres limit et offset) à GET /taches.
Transforme cette fonction en async et gère l'erreur 404
si la tâche n'existe pas.
Ajoute une validation : le titre ne doit pas dépasser 200 caractères.
```
L’édition en ligne est idéale pour les retouches chirurgicales, là où l’Agent est plus adapté aux tâches transversales touchant plusieurs fichiers. Alterner intelligemment entre les deux est le vrai secret de la vitesse avec **Cursor AI**. Pour une approche 100 % terminal, notre tutoriel Claude Code montre une philosophie complémentaire.

## Étape 7 – Interroger son code : @Codebase, @Web et @Docs

Dans le Chat (*Ctrl/Cmd + L*), les symboles **@** permettent d’injecter du contexte précis dans vos requêtes. C’est ce qui distingue Cursor d’un chatbot générique : il travaille sur *votre* code, pas dans le vide.

- **@Codebase** – Cursor recherche dans l’intégralité du projet indexé pour répondre (« Où est gérée la connexion à la base ? »).
- **@Files** /**@Folders** – cibler un fichier ou un dossier précis comme contexte.
- **@Web** – Cursor effectue une recherche web pour intégrer des informations à jour.
- **@Docs** – pointer vers une documentation officielle (FastAPI, SQLModel, etc.) indexée par Cursor.
- **@Git** – analyser des commits ou des différences pour comprendre l’historique.

```
@Codebase Explique comment la session SQLite est ouverte et fermée.
@Docs FastAPI Montre-moi comment ajouter une gestion CORS.
@Web Quelle est la dernière version stable de SQLModel et ses changements ?
```
Cette capacité à interroger le code repose sur l’**indexation** de votre dépôt. À l’ouverture d’un projet, Cursor construit un index sémantique (embeddings) en arrière-plan. Sur un très gros dépôt, patientez quelques minutes le temps que l’indexation se termine, sinon les réponses @Codebase seront partielles.

## Étape 8 – Laisser l’Agent exécuter le terminal et corriger les erreurs

C’est ici que **Cursor** dépasse le simple assistant. En mode Agent, l’IA peut proposer et exécuter des commandes dans le terminal intégré, lire la sortie, détecter les erreurs et se corriger toute seule. Demandez-lui d’installer les dépendances et de lancer le serveur :

```
Crée un environnement virtuel .venv, installe les dépendances
de requirements.txt, puis démarre le serveur avec uvicorn
et vérifie que /docs répond.
```
L’agent exécute alors les commandes (après votre autorisation) et affiche la sortie. En cas d’échec – par exemple un module manquant – il lit le message d’erreur et propose la correction. Exemple de sortie type dans le terminal :

```
$ python -m venv .venv && source .venv/bin/activate
$ pip install -r requirements.txt
Successfully installed fastapi-0.115.x sqlmodel-0.0.x uvicorn-0.3x.x
$ uvicorn app.main:app --reload
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
INFO:     Application startup complete.
# Documentation interactive disponible sur http://127.0.0.1:8000/docs
```
**Piège n°3 :** activez le mode d’exécution automatique (« auto-run ») en connaissance de cause. Il est très pratique, mais il autorise l’agent à lancer des commandes sans confirmation. Sur une machine de production ou un dépôt sensible, gardez la validation manuelle activée et surveillez chaque commande, en particulier les suppressions de fichiers.

## Étape 9 – Générer et lancer les tests avec pytest

Un projet sérieux se teste. Demandez à l’Agent de créer une suite `pytest` couvrant les principaux endpoints. Grâce à la règle de projet définie à l’étape 4, Cursor sait déjà qu’il doit générer des tests pour chaque route.

```
# tests/test_taches.py
from fastapi.testclient import TestClient
from app.main import app
client = TestClient(app)
def test_creation_tache():
    """Vérifie la création d'une tâche."""
    reponse = client.post("/taches", json={"titre": "Écrire l'article"})
    assert reponse.status_code == 201
    donnees = reponse.json()
    assert donnees["titre"] == "Écrire l'article"
    assert donnees["terminee"] is False
def test_liste_taches():
    """Vérifie que la liste des tâches est renvoyée."""
    reponse = client.get("/taches")
    assert reponse.status_code == 200
    assert isinstance(reponse.json(), list)
```
Lancez les tests avec `pytest -q`. Si un test échoue, ne corrigez pas à la main : collez la sortie dans le Chat ou demandez à l’Agent « les tests échouent, corrige jusqu’à ce que tout passe ». Cursor lit la trace, identifie la cause (souvent un code de statut mal configuré) et itère. C’est la boucle « écrire → tester → corriger » automatisée, l’un des grands atouts du mode agentique.

## Étape 10 – Réviser le code avec Bugbot et les agents en arrière-plan

Au-delà de l’écriture, Cursor aide à **relire** le code. **Bugbot** est la fonction de revue automatisée d’Anysphere : branchée sur vos *pull requests*, elle repère les bugs potentiels, les problèmes de sécurité et les régressions, puis propose des correctifs. C’est un garde-fou précieux, notamment quand une partie du code a été générée par IA.

Les **agents en arrière-plan** (et les agents cloud) constituent l’autre grande nouveauté de l’ère Cursor 2.0 : vous lancez une tâche longue (refactorisation, migration, écriture de tests sur tout un module) et l’agent travaille pendant que vous continuez sur autre chose. L’interface multi-agents permet de suivre plusieurs de ces chantiers en parallèle et de comparer les résultats avant d’accepter.

Pour notre projet, une consigne typique en arrière-plan serait : « Ajoute la journalisation (logging) structurée sur tous les endpoints et écris les tests correspondants. » Pendant l’exécution, vous restez libre de coder ailleurs. À la fin, vous relisez le diff proposé, exactement comme une revue de code humaine.

## Étape 11 – Étendre Cursor avec MCP (Model Context Protocol)

Le **Model Context Protocol (MCP)** est un standard ouvert qui permet de connecter Cursor à des outils et sources de données externes : bases de données, gestionnaires de tickets, documentation interne, services cloud. Concrètement, un serveur MCP expose des « outils » que l’agent peut appeler, comme il appellerait le terminal.

La configuration se fait dans un fichier `mcp.json`, au niveau du projet (`.cursor/mcp.json`) ou global. Voici un exemple minimal branchant un serveur MCP de système de fichiers et un serveur PostgreSQL :

```
// .cursor/mcp.json
{
  "mcpServers": {
    "fichiers": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "./"]
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres",
               "postgresql://user:pass@localhost:5432/ma_base"]
    }
  }
}
```
Une fois le serveur déclaré, ses outils apparaissent dans les paramètres de Cursor et l’agent peut les utiliser dans ses raisonnements. C’est ce qui permet, par exemple, de demander « interroge la base et génère un endpoint qui renvoie les tâches en retard » sans quitter l’éditeur. **Piège n°4 :** un serveur MCP exécute du code sur votre machine et peut accéder à des données sensibles – n’installez que des serveurs de sources fiables.

## Étape 12 – Automatiser avec Cursor CLI

Dernière brique : **Cursor CLI**, l’interface en ligne de commande qui apporte l’agent hors de l’éditeur graphique – dans un terminal, un script ou un pipeline d’intégration continue (CI). Elle ouvre la voie à des automatisations : correction de bugs déclenchée par un workflow, génération de documentation, revue de code dans la CI.

```
# Installation de l'agent CLI
curl https://cursor.com/install -fsS | bash
# Lancer une tâche agentique en une commande
cursor-agent "Ajoute une route GET /taches/stats qui renvoie
le nombre de tâches terminées et en cours"
# Mode non interactif, utile en CI (sortie exploitable)
cursor-agent -p "Corrige les avertissements de typage" --output-format text
```
Dans un pipeline, vous pouvez ainsi faire réviser automatiquement chaque *pull request* par un agent, ou générer un changelog. Votre projet de gestion de tâches est désormais complet : structuré, testé, documenté et automatisable. Versionnez-le avec `git init && git add . && git commit -m "API de tâches générée avec Cursor"` et vous disposez d’un dépôt propre.

## Sécurité et confidentialité : maîtriser ses données avec Cursor

Puisque les modèles de **Cursor** s’exécutent dans le cloud, une question revient sans cesse en entreprise : mon code part-il chez un tiers ? La réponse tient dans le **mode Privacy** (confidentialité), que nous vous recommandons d’activer dès le départ, surtout sur du code propriétaire. Lorsqu’il est activé, votre code n’est pas conservé sur les serveurs après traitement et n’est pas utilisé pour entraîner les modèles. C’est un critère décisif pour les équipes soumises à des obligations de conformité, notamment en Europe où le RGPD impose une vigilance particulière sur les transferts de données.

Concrètement, vérifiez trois points avant tout déploiement en équipe. D’abord, activez le mode Privacy dans les paramètres et confirmez qu’il s’applique à tous les modèles utilisés – y compris les modèles d’API tierces. Ensuite, exploitez le fichier `.cursorignore` pour exclure de l’indexation les fichiers sensibles (clés, secrets, données personnelles) : ce qui n’est pas indexé n’est pas envoyé. Enfin, pour les organisations, l’offre Enterprise ajoute l’authentification unique (SSO), des contrôles d’administration et des garanties contractuelles renforcées.

Gardez aussi à l’esprit le rythme d’évolution de l’outil : Anysphere publie des mises à jour très fréquentes, documentées dans son journal des versions. Les fonctions de sécurité, les modèles disponibles et les modalités de facturation peuvent donc changer d’un mois à l’autre. Pour situer l’entreprise dans le paysage plus large de l’IA – son histoire, ses levées de fonds et sa place face à Microsoft – la fiche de synthèse de Cursor (entreprise) sur Wikipédia offre un panorama utile. Dans tous les cas, considérez la sécurité comme une brique à configurer *avant* d’ouvrir votre premier dépôt professionnel, et non après.

## Tarifs de Cursor en 2026 : Free, Pro, Pro+, Ultra et Teams

La tarification de **Cursor** repose désormais sur un système de crédits d’usage. L’offre gratuite *Hobby* permet de découvrir l’outil avec environ 2 000 complétions par mois, selon MobileAppDaily en juillet 2026, tandis que les offres payantes débloquent davantage d’utilisation des modèles. Cursor propose au total quatre grandes formules – Hobby, Individual, Teams et Enterprise –, l’offre Individual s’échelonnant de 20 à 200 dollars par mois selon Superblocks, une fourchette confirmée par Emergent.sh en juillet 2026. Les prix officiels sont libellés en dollars ; les montants en euros ci-dessous sont des estimations indicatives.

| Offre | Prix mensuel | Prix annuel (−20 %) | Pour qui ? | 
|---|---|---|---|
| Hobby (gratuit) | 0 $ | 0 $ | Découverte, usage occasionnel | 
| Pro | 20 $ (≈ 18 €) | 16 $/mois | Développeur individuel | 
| Pro+ | 60 $ (≈ 55 €) | 48 $/mois | Usage intensif | 
| Ultra | 200 $ (≈ 185 €) | 160 $/mois | Power users, gros volumes | 
| Teams | 40 $/utilisateur | – | Équipes, gestion centralisée | 
| Enterprise | Sur devis | Sur devis | Grandes organisations, SSO, sécurité | 

Point d’attention : en juin 2026, Cursor a fait évoluer son offre Teams en séparant l’usage en deux pools distincts – l’un pour les modèles maison (Composer / Auto), l’autre pour les modèles d’API tierces (Claude, GPT, Gemini). Depuis, l’offre Teams se scinde elle-même en deux niveaux, Standard à 40 dollars par utilisateur et par mois et Premium à 120 dollars, selon NoCode MBA en août 2026 ; à titre de comparaison, une configuration Pro pour trois postes revient à 720 dollars la première année, d’après Credit for Startups en septembre 2026. Ce système de crédits a suscité une certaine confusion dans la communauté, certains utilisateurs regrettant le manque de lisibilité par rapport à un abonnement à prix fixe. Surveillez votre consommation dans le tableau de bord pour éviter les dépassements. La documentation détaillée est disponible sur la page modèles et tarifs de Cursor.

## Cursor face à GitHub Copilot et Claude Code

Faut-il choisir **Cursor** plutôt que ses concurrents ? Tout dépend de votre flux de travail. Copilot reste une extension légère intégrée à votre éditeur existant ; Claude Code privilégie le terminal ; Cursor mise sur un IDE complet et l’orchestration multi-agents. Le tableau suivant synthétise les différences clés.

| Critère | Cursor | GitHub Copilot | Claude Code | 
|---|---|---|---|
| Forme | IDE autonome (fork VS Code) | Extension d’éditeur | Agent en ligne de commande | 
| Prix d’entrée payant | 20 $/mois (Pro) | 10 $/mois (Pro) | Selon offre Claude | 
| Modèles multiples | Oui (Claude, GPT, Gemini, Grok, Composer) | Oui (Claude, GPT, Gemini) | Modèles Claude | 
| Édition multi-fichiers | Excellente (Composer) | Bonne | Excellente | 
| Modèle maison | Composer 2 | Non | Non | 
| Agents en parallèle | Oui (jusqu’à 8) | Partiel | Oui | 

Pour un comparatif détaillé face à ses rivaux directs, consultez nos analyses Claude Code vs Cursor et Windsurf vs Cursor. En résumé : Cursor brille lorsque l’on veut un environnement unifié et agentique, quand Copilot séduit par sa simplicité et son intégration native à l’écosystème GitHub.

## 5 pièges fréquents à éviter avec Cursor

Au-delà des pièges déjà signalés au fil des étapes, voici les erreurs les plus courantes qui plombent l’expérience des nouveaux utilisateurs de **Cursor AI**.

- **Rester en mode Agent en permanence.** Pour une simple question, utilisez le mode*Ask* : il n’écrit rien et évite les modifications non désirées. Réservez l’Agent aux tâches qui doivent modifier des fichiers.
- **Négliger les règles de projet.** Sans`.cursor/rules` , l’IA ignore vos conventions et vous répétez sans cesse les mêmes consignes. Investir cinq minutes dans les règles fait gagner des heures.
- **Donner des consignes vagues.** « Améliore ce code » produit des résultats erratiques. Précisez l’objectif, les contraintes et le fichier concerné.
- **Ignorer l’indexation.** Poser des questions @Codebase avant la fin de l’indexation donne des réponses incomplètes sur les gros dépôts. Attendez la fin de l’indexation.
- **Faire une confiance aveugle au code généré.** L’IA se trompe, invente parfois des API ou introduit des failles. Relisez, testez et utilisez Bugbot avant de fusionner.

## Dépannage : 8 problèmes courants et leurs solutions

Voici les incidents les plus fréquemment rencontrés avec **Cursor** et la marche à suivre pour les résoudre rapidement.

| Problème | Cause probable | Solution | 
|---|---|---|
| « You’ve hit your usage limit » | Crédits d’usage épuisés | Vérifier le tableau de bord, passer à un plan supérieur ou attendre le renouvellement mensuel | 
| Tab ne propose rien | Fonction désactivée ou fichier non pris en charge | Activer Cursor Tab dans les paramètres ; vérifier la connexion Internet | 
| @Codebase donne des réponses incomplètes | Indexation non terminée | Attendre la fin de l’indexation ; relancer l’indexation dans les paramètres | 
| L’agent ne peut pas lancer le terminal | Auto-run désactivé ou permissions | Autoriser l’exécution ; vérifier les paramètres de sécurité de l’agent | 
| Extensions VS Code absentes | Import non effectué | Réimporter la config VS Code via la palette de commandes | 
| Réponses lentes | Modèle lourd sélectionné | Basculer sur Composer 2 ou un modèle « Fast » | 
| Serveur MCP non détecté | Erreur dans mcp.json | Valider le JSON ; vérifier la commande et le chemin ; consulter les logs MCP | 
| Cursor CLI introuvable | PATH non mis à jour | Rouvrir le terminal ou ajouter le binaire au PATH | 

**Piège n°5 :** si Cursor devient lent sur un très gros projet, ajustez votre `.cursorignore` pour exclure les dossiers volumineux (dépendances, artefacts de build, données). Une indexation plus légère améliore nettement la réactivité et la pertinence des réponses.

## Astuces avancées pour les développeurs exigeants

Une fois les bases maîtrisées, ces techniques font passer votre usage de **Cursor** au niveau supérieur.

- **Des règles par contexte.** Créez plusieurs fichiers`.mdc` dans`.cursor/rules/` : un pour le backend, un pour les tests, un pour la documentation. Chaque règle peut s’appliquer à des motifs de fichiers précis (glob).
- **Le mode plan avant l’action.** Demandez d’abord un plan (« propose un plan sans coder »), validez-le, puis lancez l’exécution. Vous gardez le contrôle sur les tâches complexes.
- **Exploiter @Docs.** Indexez la documentation officielle de vos frameworks pour que l’IA cite les API réelles plutôt que d’en inventer – un rempart contre les « hallucinations ».
- **Multi-agents pour explorer des variantes.** Lancez plusieurs agents sur une même tâche avec des modèles différents (Composer 2 vs Claude Opus 4.8) et comparez les diffs.
- **Bugbot systématique.** Activez la revue automatique sur chaque pull request pour attraper les régressions avant la fusion.
- **Cursor CLI en CI.** Intégrez`cursor-agent` dans vos pipelines pour automatiser la revue de code ou la génération de changelog.

Enfin, gardez à l’esprit que l’IA est un copilote, pas un pilote automatique. Les développeurs les plus productifs avec **Cursor AI** ne sont pas ceux qui délèguent tout, mais ceux qui décomposent le travail en tâches claires, vérifient chaque étape et conservent une solide compréhension de leur code. Si vous souhaitez aussi faire tourner des modèles en local pour des raisons de coût ou de confidentialité, notre tutoriel Ollama est un excellent complément.

### Related Coverage

## FAQ : vos questions sur Cursor AI

### Cursor est-il gratuit ?

Oui, Cursor propose une offre gratuite *Hobby*, sans carte bancaire, incluant environ 2 000 complétions par mois selon MobileAppDaily, suffisante pour découvrir l’outil. Les fonctions agentiques intensives nécessitent toutefois une offre payante : Pro à 20 $/mois, Pro+ à 60 $/mois ou Ultra à 200 $/mois, avec 20 % de réduction en facturation annuelle ; côté équipes, Teams se décline désormais en Standard (40 $/utilisateur/mois) et Premium (120 $/utilisateur/mois), selon NoCode MBA.

### Cursor remplace-t-il VS Code ?

Cursor est un IDE autonome dérivé de VS Code : il peut le remplacer entièrement, en important vos extensions, thèmes et raccourcis. Vous n’avez pas besoin d’installer VS Code au préalable ; Cursor est un logiciel indépendant à télécharger sur cursor.com.

### Quels modèles d’IA Cursor utilise-t-il ?

En 2026, Cursor donne accès à son modèle maison Composer 2 ainsi qu’aux grands modèles du marché : Claude Opus 4.8 et Sonnet 5 (Anthropic), GPT-5.5 (OpenAI), Gemini 3.1 Pro (Google) et Grok 4.3 (xAI), entre autres. Vous changez de modèle à la volée selon la tâche.

### Qu’est-ce que le modèle Composer de Cursor ?

Composer est le premier modèle de codage maison d’Anysphere, dévoilé avec Cursor 2.0 le 29 octobre 2025. C’est un modèle « mixture-of-experts » spécialisé, annoncé comme quatre fois plus rapide que les modèles agentiques comparables, qui boucle la plupart de ses tours en moins de 30 secondes.

### Cursor est-il meilleur que GitHub Copilot ?

Ce sont deux philosophies différentes. Copilot est une extension légère intégrée à votre éditeur, idéale pour l’autocomplétion et l’écosystème GitHub. Cursor est un IDE complet centré sur l’IA, avec édition multi-fichiers, mode agent et orchestration multi-agents. Cursor va plus loin dans l’automatisation ; Copilot est plus simple et moins cher à l’entrée.

### Cursor fonctionne-t-il sous Linux et macOS ?

Oui. Cursor est disponible sur Windows (10+), macOS (10.15+, dont Apple Silicon) et Linux (paquets .deb, .rpm et AppImage). Une connexion Internet est nécessaire car les modèles s’exécutent dans le cloud.

### Le code généré par Cursor est-il fiable ?

Le code généré est souvent de bonne qualité, mais il n’est jamais infaillible : l’IA peut introduire des bugs, inventer des API ou des failles de sécurité. Relisez toujours les diffs, écrivez des tests et utilisez Bugbot pour la revue automatisée avant de fusionner.

### Qu’est-ce que MCP dans Cursor ?

Le Model Context Protocol (MCP) est un standard ouvert qui connecte Cursor à des outils externes (bases de données, gestionnaires de tickets, documentation). Un serveur MCP expose des « outils » que l’agent peut appeler. La configuration se fait dans un fichier `mcp.json`. N’installez que des serveurs de sources fiables, car ils exécutent du code sur votre machine.

*Article publié le 02 juin 2026. Les tarifs et versions évoluant rapidement, vérifiez toujours les informations à jour sur la documentation officielle de Cursor.*
