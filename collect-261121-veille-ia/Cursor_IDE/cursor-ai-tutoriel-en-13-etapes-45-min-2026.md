---
id: collect-261121-veille-ia/cursor-ide/cursor-ai-tutoriel-en-13-etapes-45-min-2026
title: "Cursor AI : Tutoriel en 13 Étapes, 45 Min [2026]"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["cursor", "code", "cette", "selon", "tutoriel", "projet", "etapes", "simple"]
source: https://tech-insider.org/fr/cursor-ai-tutoriel-13-etapes-2026/
source_anchor: ""
source_lines: [1, 367]
section: "Cursor_IDE"
---
Cursor AI a dépassé 2 milliards de dollars de revenu annualisé et franchi le cap du million d’abonnés payants en février 2026, selon les chiffres de TechCrunch et The Information relayés par CloudZero. L’éditeur new-yorkais Anysphere revendique une adoption dans 64 % des entreprises du Fortune 500. Pour les développeurs qui envisagent de basculer vers cet éditeur de code piloté par l’IA, reste une question très concrète : comment le configurer correctement, sans perdre une heure à deviner où cliquer. En France comme dans le reste de l’Europe, cette question s’accompagne souvent d’une autre, plus réglementaire : comment adopter un outil de ce type sans exposer du code sensible ou des données personnelles à des serveurs mal identifiés.

Ce tutoriel couvre l’installation de Cursor AI, la création du compte, le choix de la formule tarifaire, la mise en place des règles de projet (`.cursor/rules` et `AGENTS.md`), le mode Agent, les serveurs MCP et le déploiement d’un Cloud Agent. Treize étapes, 45 minutes environ, avec un projet complet à la fin pour tout mettre en pratique sur du code réel plutôt que sur un simple test isolé. Une section dédiée au déploiement en équipe et un tableau de dépannage avec dix problèmes fréquents complètent l’ensemble.

## Qu’est-ce que Cursor AI et pourquoi son adoption explose en 2026

Cursor AI est un éditeur de code construit à partir de VS Code, développé par la société Anysphere. Contrairement à une extension comme GitHub Copilot qui s’ajoute à un éditeur existant, Cursor AI intègre l’IA directement dans le cœur du produit : autocomplétion multi-lignes, chat contextuel, édition en ligne et surtout un mode Agent capable de modifier plusieurs fichiers de façon autonome.

Cette bascule dépasse le simple effet de mode. Selon le rapport DORA 2025 sur le développement logiciel assisté par IA, 90 % des professionnels de la tech utilisent désormais l’IA au travail, avec un temps médian de deux heures par jour passé sur ces outils. Cursor AI capte une bonne partie de cette demande grâce à son positionnement agentique : au lieu de suggérer une ligne de code isolée, l’outil comprend une tâche entière, écrit les fichiers nécessaires, exécute les commandes utiles et propose une pull request prête à relire.

Cette croissance a aussi un revers. Toujours selon les données relayées par CloudZero, seuls 22 % des directeurs financiers arrivent à relier précisément les dépenses en IA aux résultats business qu’elles produisent. Pour une équipe de développement, cela veut dire une chose simple : configurer Cursor AI correctement dès le départ, avec des règles de projet claires et une formule adaptée à l’usage réel, évite le gaspillage de crédits et les mauvaises surprises en fin de mois.

Cursor AI n’est pas non plus seul sur ce marché. GitHub Copilot reste l’outil le plus utilisé au niveau mondial, avec, selon GitHub, des millions d’utilisateurs individuels et des dizaines de milliers de clients entreprise. Nous détaillons cette comparaison plus loin, mais retenez déjà que le choix entre les deux dépend surtout du niveau d’autonomie que vous voulez confier à l’IA, et du budget que votre équipe peut y consacrer chaque mois.

Pour les équipes françaises, l’intérêt de Cursor AI dépasse le simple gain de vitesse. Les entreprises qui recrutent des développeurs juniors ou en reconversion constatent que l’outil réduit le temps d’appropriation d’un dépôt existant, à condition que les règles de projet soient rédigées correctement dès l’arrivée d’une nouvelle recrue. C’est un usage moins spectaculaire que la génération de code pure, mais souvent plus rentable sur la durée : un fichier `AGENTS.md` bien tenu vaut parfois mieux qu’une journée entière de documentation technique que personne ne relira.

## Prérequis : versions, comptes et configuration matérielle

Avant de lancer l’installation, vérifiez que votre poste de travail répond aux exigences minimales. Cursor AI tourne sur les trois grands systèmes d’exploitation et ne demande pas de machine surpuissante, mais quelques prérequis évitent les mauvaises surprises, surtout si vous comptez faire tourner des serveurs MCP en local.

| Élément | Minimum requis | Recommandé | 
|---|---|---|
| Système d’exploitation | Windows 10, macOS 12, distribution Linux récente | Windows 11, macOS 14 ou plus | 
| Mémoire vive | 8 Go | 16 Go ou plus | 
| Espace disque | 2 Go libres | 5 Go libres (cache et extensions inclus) | 
| Connexion Internet | Requise en permanence | Connexion stable, faible latence | 
| Compte | Adresse email | Compte GitHub (synchronisation des dépôts) | 
| Node.js (optionnel) | Non requis pour l’éditeur seul | Version LTS, utile pour lancer des serveurs MCP locaux | 

Un compte GitHub n’est pas obligatoire, un simple email suffit pour démarrer. Il devient toutefois utile assez vite : il permet de synchroniser vos dépôts, et surtout de déclencher un Cloud Agent directement depuis un commentaire sur une pull request. Pour les équipes soumises au RGPD, prévoyez aussi un point de vigilance : le code envoyé au modèle transite par les serveurs de Cursor AI ou de ses fournisseurs de modèles, donc les projets sous NDA ou contenant des données personnelles méritent une revue avant connexion, surtout si vous ajoutez des serveurs MCP donnant accès à des bases de données internes.

Si votre équipe vient d’un autre éditeur, la migration reste globalement simple puisque Cursor AI, Windsurf et VS Code partagent la même base technique et le même format d’extensions. La bascule depuis un IDE plus éloigné comme IntelliJ ou Eclipse demande en revanche un peu plus de temps d’adaptation, principalement sur les raccourcis clavier et l’organisation des panneaux, même si l’essentiel des concepts (explorateur de fichiers, terminal intégré, débogueur) reste familier.

## Étapes 1 à 3 : télécharger, installer et lancer Cursor AI

### Étape 1 – Télécharger l’installeur adapté à votre système

Rendez-vous sur le site officiel de Cursor et cliquez sur le bouton de téléchargement. Le site détecte automatiquement votre système d’exploitation et propose le bon fichier : binaire universel pour macOS (Apple Silicon et Intel), exécutable pour Windows, ou paquet AppImage/.deb pour Linux. Aucune inscription n’est nécessaire à cette étape, la création de compte intervient au premier lancement.

### Étape 2 – Installer et importer vos réglages VS Code

Lancez l’installeur téléchargé et suivez l’assistant. Sur macOS, glissez simplement l’application dans le dossier Applications. Au premier démarrage, Cursor AI propose d’importer en un clic vos extensions, raccourcis clavier et thème depuis une installation VS Code existante. C’est l’un des vrais avantages d’un éditeur construit sur la même base : la migration prend quelques secondes plutôt qu’une demi-journée de reconfiguration.

Comme la plupart des forks de VS Code, Cursor installe généralement un raccourci en ligne de commande. Une fois l’application ouverte au moins une fois, vous pouvez normalement ouvrir n’importe quel dossier de travail directement depuis un terminal :

```
cd mon-projet
cursor .
```
### Étape 3 – Premier lancement et prise en main de l’interface

Connectez-vous avec un email, un compte Google ou un compte GitHub. L’interface reprend les repères classiques d’un éditeur de code : explorateur de fichiers à gauche, zone d’édition au centre, terminal intégré en bas. Trois éléments spécifiques à Cursor AI méritent d’être repérés dès cette étape : le panneau de chat contextuel (raccourci `Ctrl+L` ou `Cmd+L`), l’édition en ligne rapide (`Ctrl+K` ou `Cmd+K`) et le bouton de bascule entre mode Chat et mode Agent, situé dans la barre de saisie du panneau latéral.

Prenez le temps de parcourir le petit didacticiel intégré qui s’affiche au premier lancement : il présente le fonctionnement du Tab prédictif, capable de deviner la prochaine modification à plusieurs endroits du fichier et pas seulement à la position du curseur. C’est une fonction moins spectaculaire que le mode Agent, mais c’est souvent elle qui fait gagner le plus de temps au quotidien, sur des tâches répétitives comme le renommage cohérent d’une variable dans tout un fichier.

## Étapes 4 et 5 : créer un compte et choisir sa formule tarifaire

### Étape 4 – Créer un compte Cursor et lier votre dépôt

Une fois connecté dans l’application, un compte est automatiquement créé sur le tableau de bord en ligne de Cursor. Vérifiez votre adresse email, puis liez votre compte GitHub depuis les réglages si vous comptez utiliser les Cloud Agents ou la revue de code automatisée Bugbot. Cette liaison est aussi ce qui permettra, plus tard, de déclencher un agent directement depuis un commentaire sur une pull request.

### Étape 5 – Comparer les formules Hobby, Pro, Pro+, Ultra et Teams

Le tarif de Cursor AI en 2026 repose sur un système de crédits par formule plutôt que sur un nombre fixe de requêtes. Voici le détail publié sur la page officielle de tarification, recoupé avec l’analyse du cabinet CloudZero publiée en mai 2026.

| Formule | Prix mensuel | Prix annuel (mensualisé) | Pour qui | 
|---|---|---|---|
| Hobby | Gratuit | — | Découverte, usage occasionnel, sans carte bancaire | 
| Pro | 20 $ | ~16 $ | Développeur individuel avec un usage régulier | 
| Pro+ | 60 $ | ~48 $ | Usage intensif, gros volumes de requêtes agent | 
| Ultra | 200 $ | ~160 $ | Power users, pool de crédits élevé (environ 400 $ équivalent) | 
| Teams | 40 $/utilisateur | ~32 $/utilisateur | Équipes : Bugbot, SSO, facturation centralisée | 
| Enterprise | Sur devis | Sur devis | Grands comptes : SCIM, logs d’audit, usage mutualisé | 

La formule Hobby suffit pour tester l’outil et comprendre le mode Agent, mais ses limites de requêtes se font sentir dès qu’on travaille sur un vrai projet au quotidien. Pour un développeur solo qui utilise Cursor AI plusieurs heures par jour, la formule Pro à 20 $/mois reste le point d’entrée le plus raisonnable. La formule Teams ajoute surtout des fonctions de gouvernance : contrôle d’accès centralisé, mode confidentialité d’équipe et connexion SSO via SAML ou OIDC, des critères qui pèsent lourd pour toute entreprise européenne soumise à des audits de sécurité.

L’engagement annuel fait mécaniquement baisser la facture mensuelle affichée, mais il fige aussi votre budget sur douze mois dans un secteur où les tarifs bougent encore régulièrement, comme le montre plus loin l’exemple de Windsurf qui a changé de modèle tarifaire en mars 2026. Pour une équipe qui découvre tout juste Cursor AI, mieux vaut démarrer en mensuel le temps de mesurer la consommation réelle de crédits sur six à huit semaines avant de s’engager sur un an.

## Étapes 6 et 7 : configurer le modèle IA et les réglages de l’éditeur

### Étape 6 – Choisir son modèle IA par défaut

Ouvrez Settings puis l’onglet Models. Cursor AI propose plusieurs familles de modèles récentes (Claude, GPT et Gemini selon les intégrations disponibles au moment de votre installation), ainsi qu’un mode de sélection automatique qui choisit le modèle le plus adapté à la tâche. Vous pouvez désactiver les modèles que vous n’utilisez jamais pour simplifier le menu, et surtout ajouter votre propre clé API si vous préférez payer directement le fournisseur du modèle plutôt que de consommer les crédits inclus dans votre formule. Cette option de clé personnelle est particulièrement utile pour les équipes qui ont déjà un contrat entreprise avec un fournisseur de modèle et veulent éviter de payer deux fois.

Le choix du modèle n’est pas qu’une question de préférence personnelle, c’est aussi un arbitrage coût-qualité. Un modèle plus capable consomme davantage de crédits par requête, ce qui a du sens pour un refactor d’architecture complexe mais devient du gaspillage pour générer un test unitaire simple ou corriger une faute de frappe dans un message d’erreur. Le mode de sélection automatique tente de faire cet arbitrage à votre place, mais une équipe expérimentée gagne souvent à fixer manuellement un modèle économique pour les tâches routinières et à réserver le modèle premium aux vraies décisions d’architecture.

### Étape 7 – Personnaliser Tab, raccourcis et thème

Dans Settings > Editor, ajustez le comportement de l’autocomplétion Tab : délai avant suggestion, acceptation partielle mot par mot avec `Ctrl+→`, et portée du contexte pris en compte (fichier courant seulement, ou fichiers ouverts dans l’onglet). Comme Cursor AI reprend le moteur de VS Code, les raccourcis clavier personnalisés se gèrent de la même façon, via un fichier de configuration. Voici un exemple de personnalisation courante, qui réassigne l’acceptation d’une suggestion Agent à une touche dédiée :

```
[
  {
    "key": "ctrl+enter",
    "command": "composer.submitComposerPrompt",
    "when": "composerFocus"
  },
  {
    "key": "ctrl+shift+l",
    "command": "workbench.action.chat.open"
  }
]
```
Ce fichier se modifie depuis la palette de commandes, en cherchant “Preferences: Open Keyboard Shortcuts (JSON)”. Les noms de commandes exacts peuvent varier d’une version à l’autre : le plus fiable reste de chercher l’action voulue dans l’interface graphique, puis de récupérer son identifiant technique avant de l’éditer en JSON.

## Étapes 8 et 9 : mettre en place vos règles de projet avec .cursor/rules

C’est l’étape la plus déterminante de tout ce tutoriel. Les grands modèles de langage ne conservent aucune mémoire d’une complétion à l’autre : sans mécanisme de contexte persistant, vous répéteriez les mêmes instructions d’architecture à chaque nouvelle conversation. Les règles de projet résolvent ce problème.

### Étape 8 – Créer le dossier .cursor/rules et votre premier fichier .mdc

À la racine de votre projet, créez un dossier `.cursor/rules` puis un premier fichier à l’intérieur. Le point qui piège le plus de développeurs débutants sur Cursor AI, comme le précise la documentation officielle sur les règles : l’extension doit obligatoirement être `.mdc`, pas `.md`. Selon cette même documentation, **« un simple fichier .md placé dans .cursor/rules est ignoré par le système de règles car il ne contient pas l’en-tête nécessaire pour spécifier description, globs et alwaysApply »**. Voici un exemple de règle complète pour un projet React :

```
---
description: "Conventions de composants React"
globs: src/components/**/*.tsx
alwaysApply: false
---
Utilise des composants fonctionnels avec des hooks, jamais de classes.
Chaque composant reçoit ses props typées via une interface TypeScript dédiée.
Les appels réseau passent systématiquement par le client défini dans src/lib/api.ts.
N'ajoute jamais de style inline : utilise les classes Tailwind existantes.
```
Le même principe s’applique côté backend, avec un fichier ciblé sur un tout autre dossier et un tout autre langage :

```
---
description: "Conventions API Python"
globs: api/**/*.py
alwaysApply: false
---
Chaque fonction publique a une signature typée et une docstring courte en une ligne.
Les exceptions métier héritent d'une classe commune AppException, jamais d'Exception nue.
Toute requête vers la base de données passe par le repository dédié, jamais d'ORM direct dans les routes.
```
### Étape 9 – Organiser les règles par dossier et cibler les bons fichiers avec globs

Sur un projet de taille réelle, un seul fichier de règles devient vite ingérable. Cursor AI permet d’organiser les fichiers `.mdc` en sous-dossiers, par exemple `.cursor/rules/frontend/composants.mdc` ou `.cursor/rules/backend/validation.mdc`. Le champ `globs` détermine quels fichiers déclenchent la règle, et `alwaysApply` détermine si elle s’applique en permanence ou seulement quand un fichier correspondant est ouvert. Selon la documentation officielle, ces règles sont **« incluses au tout début du contexte du modèle, ce qui donne à l’IA des instructions cohérentes pour générer du code, interpréter des modifications ou aider sur un workflow »**. Concrètement, mieux vaut trois règles courtes et ciblées qu’un seul fichier de 300 lignes que le modèle finit par ignorer partiellement faute de place dans son contexte.

## Étape 10 : utiliser AGENTS.md pour un contexte simplifié

Pour un projet simple ou un prototype, écrire des fichiers `.mdc` avec en-tête YAML peut sembler excessif. Cursor AI accepte une alternative plus légère : un fichier `AGENTS.md` classique à la racine du projet, sans métadonnées à gérer. Le système prend aussi en charge des fichiers `AGENTS.md` imbriqués dans les sous-dossiers, avec, selon la documentation, **« les instructions les plus spécifiques qui prennent le pas »** sur les plus générales.

```
# AGENTS.md
## Stack technique
- Backend : Python 3.12, FastAPI, PostgreSQL
- Tests : pytest, couverture minimale 80 %
- Style : formatage via ruff, pas d'exceptions silencieuses
## Conventions
- Toute route API retourne un schéma Pydantic explicite
- Les migrations passent par Alembic, jamais de modification manuelle du schéma
- Les secrets viennent des variables d'environnement, jamais du code source
## Commandes utiles
- Lancer les tests : pytest -v
- Démarrer le serveur local : uvicorn app.main:app --reload
```
En pratique, beaucoup d’équipes démarrent avec un `AGENTS.md` unique, puis migrent vers des fichiers `.mdc` ciblés dès que le projet grossit et que certaines règles doivent s’appliquer uniquement à un sous-dossier précis. Les deux mécanismes cohabitent sans problème dans un même dépôt, et rien n’empêche de garder un `AGENTS.md` général pour le contexte métier (ce que fait l’application, qui sont les utilisateurs) tout en déléguant les conventions de code strictes aux fichiers `.mdc`.

## Étape 11 : maîtriser le mode Agent et les workflows agentiques

Cursor AI propose trois niveaux d’intervention. L’édition en ligne (`Ctrl+K`) modifie un bloc de code précis sur votre demande. Le chat (`Ctrl+L`) répond à des questions et peut proposer des modifications que vous appliquez manuellement. Le mode Agent, lui, va plus loin : il lit les fichiers pertinents du projet, écrit ou modifie plusieurs fichiers à la fois, exécute des commandes dans le terminal intégré, corrige les erreurs qu’il rencontre, puis présente un diff complet à valider avant application.

Pour basculer en mode Agent, ouvrez le panneau latéral et sélectionnez Agent dans le menu déroulant au-dessus de la zone de saisie. Un bon prompt agentique décrit un objectif plutôt qu’une action isolée : plutôt que “ajoute une validation”, préférez “ajoute une validation d’email sur le formulaire d’inscription, avec un message d’erreur clair et un test unitaire associé”. L’agent découpe alors la tâche en sous-étapes, les exécute, et signale les points qui nécessitent votre arbitrage, par exemple un choix de bibliothèque ou une ambiguïté dans la spécification.

La différence se voit bien sur un cas concret. Demander en mode Chat “comment ajouter la pagination à cette liste ?” renvoie une explication et un extrait de code à copier-coller soi-même. Demander la même chose en mode Agent déclenche une série d’actions : lecture du composant de liste existant, modification du composant pour accepter des paramètres de page, ajustement de l’appel API correspondant, et proposition d’un test qui vérifie le comportement sur une deuxième page de résultats. Le gain de temps est réel, mais il déplace aussi la charge de travail : moins de temps à écrire, davantage de temps à relire attentivement ce qui a changé dans plusieurs fichiers à la fois.

Ce mode agentique n’est pas propre à Cursor AI. Des outils comme Claude Code ou Google Antigravity suivent la même logique avec des approches différentes de la gestion du contexte et de l’autonomie accordée à l’IA. La bonne pratique, quel que soit l’outil, reste la même : ne jamais valider un diff sans l’avoir lu, même quand le résultat semble correct au premier coup d’œil.

## Étape 12 : connecter des serveurs MCP à Cursor AI

Le Model Context Protocol, ou MCP, est un standard ouvert introduit par Anthropic et désormais repris par la plupart des éditeurs IA du marché, dont Cursor. Sur le site officiel du protocole, la documentation de Cursor résume sa fonction ainsi : **« le Model Context Protocol permet à Cursor de se connecter à des outils et des sources de données externes »**. Concrètement, un serveur MCP donne à l’agent un accès structuré à une base de données, un gestionnaire de tickets, un outil de design ou une API interne, sans que vous ayez à copier-coller manuellement du contexte à chaque conversation.

La configuration se fait dans un fichier `.cursor/mcp.json` à la racine du projet pour une portée locale, ou `~/.cursor/mcp.json` pour une portée globale à tous vos projets. Voici un exemple de connexion à un serveur MCP local lancé via Node.js :

```
{
  "mcpServers": {
    "postgres-projet": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://user:password@localhost:5432/mabase"
      }
    }
  }
}
```
Les serveurs MCP ne se limitent pas à Node.js : la documentation officielle précise qu’il est possible d’**« écrire des serveurs MCP dans n’importe quel langage capable d’écrire sur stdout ou de servir un point de terminaison HTTP, comme Python, JavaScript ou Go »**. Pour un serveur distant exposé en HTTP, la syntaxe change légèrement : un champ `url` remplace `command` et `args`. Une fois le serveur déclaré, redémarrez Cursor AI et vérifiez son statut dans Settings > MCP : un point vert confirme la connexion, un point rouge signale généralement un problème de commande ou de variable d’environnement manquante.

Trois cas d’usage reviennent le plus souvent en entreprise. Un serveur MCP relié à un gestionnaire de tickets permet à l’agent de lire directement la description d’un ticket avant de coder, plutôt que de la faire recopier manuellement. Un serveur relié à un outil de design donne accès aux spécifications visuelles sans captures d’écran intermédiaires. Un serveur relié à une base de données de lecture seule permet à l’agent de vérifier un schéma réel plutôt que de le deviner à partir d’un fichier de migration potentiellement obsolète. Dans les trois cas, la règle reste la même : ne jamais donner plus de droits que la tâche ne l’exige.

## Étape 13 : déployer un Cloud Agent et automatiser vos tâches

Les Cloud Agents, anciennement appelés Background Agents, changent d’échelle par rapport au mode Agent local. D’après la documentation officielle, ils **« reposent sur les mêmes principes que l’agent classique, mais s’exécutent dans des machines virtuelles isolées dans le cloud, avec un environnement de développement complet, plutôt que sur la machine locale »**. Cela veut dire un dépôt cloné, les dépendances installées, les secrets nécessaires et un accès réseau, le tout sans mobiliser votre ordinateur.

Un Cloud Agent se déclenche de plusieurs façons : depuis l’application de bureau en sélectionnant “Cloud” dans le menu de l’agent, depuis l’interface web sur cursor.com/agents, depuis l’application iOS, ou directement depuis Slack, GitHub, Bitbucket ou Linear en mentionnant `@cursor`. Sur une pull request GitHub par exemple :

```
@cursor corrige le bug décrit dans ce ticket, ajoute un test de
non-régression, et pousse le correctif sur une nouvelle branche
```
L’agent clone le dépôt, travaille sur une branche séparée, exécute les tests, puis pousse ses modifications sous forme de pull request. Plusieurs agents peuvent tourner en parallèle sans bloquer votre machine, et chacun fournit des artefacts (captures d’écran, vidéos, journaux d’exécution) pour vérifier le travail effectué. Il est même possible de prendre la main à distance sur le bureau de l’agent pour tester une modification avant de la fusionner.

Un usage typique en équipe consiste à laisser tourner un Cloud Agent la nuit sur des tâches de fond : mise à jour de dépendances mineures, correction de tests instables, ou génération d’un rapport de couverture de code. Le développeur retrouve le lendemain matin une pull request déjà ouverte, avec les artefacts d’exécution attachés, plutôt qu’une tâche encore à démarrer.

## Projet complet : construire une API de bout en bout avec l’agent Cursor

Pour relier toutes les étapes précédentes, voici un projet minimal mais complet : une route d’API qui valide une inscription utilisateur, construite avec FastAPI et pilotée par le mode Agent de Cursor AI.

Première étape, créez la règle de projet qui cadre le style attendu :

```
---
description: "Conventions API FastAPI"
globs: app/**/*.py
alwaysApply: true
---
Toutes les routes utilisent des schémas Pydantic pour la validation d'entrée et de sortie.
Les erreurs métier lèvent une HTTPException avec un code et un message explicites.
Chaque nouvelle route s'accompagne d'un test pytest dans le dossier tests/.
```
Ensuite, en mode Agent, le prompt suivant suffit à démarrer le travail :

*“Crée une route POST /users qui valide un email et un mot de passe (12 caractères minimum, au moins un chiffre), retourne une erreur 422 explicite si la validation échoue, et ajoute un test pytest qui couvre le cas valide et deux cas d’échec.”*

L’agent lit d’abord la structure existante du projet, applique la règle définie plus haut, puis génère un résultat proche de celui-ci :

```
from fastapi import APIRouter, HTTPException
from pydantic import BaseModel, EmailStr, field_validator
router = APIRouter()
class UserCreate(BaseModel):
    email: EmailStr
    password: str
    @field_validator("password")
    @classmethod
    def password_strength(cls, value: str) -> str:
        if len(value) < 12 or not any(char.isdigit() for char in value):
            raise ValueError("Le mot de passe doit contenir 12 caractères et un chiffre minimum")
        return value
@router.post("/users", status_code=201)
def create_user(payload: UserCreate):
    if payload.email.endswith("@exemple-interdit.com"):
        raise HTTPException(status_code=422, detail="Domaine email refusé")
    return {"email": payload.email, "status": "créé"}
```
Dernière étape, relisez le diff proposé, lancez les tests en local pour confirmer, puis démarrez le serveur :

```
pytest -v
uvicorn app.main:app --reload
curl -X POST http://localhost:8000/users \
  -H "Content-Type: application/json" \
  -d '{"email":"[email protected]","password":"motdepasse123"}'
```
Ce cycle complet, règle de projet puis prompt agentique puis relecture du diff, reste le même quelle que soit la taille de la tâche. C'est cette boucle qu'il faut automatiser dans vos réflexes pour tirer parti de Cursor AI sans accumuler de dette technique invisible. Pour aller plus loin sur ce même projet, la suite logique consiste à demander à l'agent d'ajouter un endpoint de connexion avec génération de jeton, puis de connecter un serveur MCP vers votre base de données de test pour qu'il vérifie lui-même le schéma des tables existantes avant d'écrire une nouvelle migration.

## Cursor AI vs GitHub Copilot vs Windsurf : comparatif et tarifs 2026

Trois éditeurs IA dominent les discussions des équipes de développement en 2026. Voici comment ils se comparent sur les critères qui comptent le plus au moment de choisir.

| Critère | Cursor AI | GitHub Copilot | Windsurf | 
|---|---|---|---|
| Base technique | Fork complet de VS Code | Extension pour VS Code, JetBrains, Neovim | Fork de VS Code (marque conservée sous Cognition AI) | 
| Entrée de gamme | Gratuit (Hobby) | Gratuit (2 000 complétions/mois, 50 requêtes chat) | Gratuit | 
| Palier individuel courant | 20 $/mois (Pro) | 10 $/mois (Pro) | environ 20 $/mois (Pro, depuis mars 2026) | 
| Palier le plus élevé (individuel) | 200 $/mois (Ultra) | 100 $/mois (Max) | 200 $/mois (Max) | 
| Palier équipe | 40 $/utilisateur/mois | 19 $/utilisateur/mois (Business) | 40 $/utilisateur/mois | 
| Mode Agent natif | Oui | Oui | Oui | 
| Support MCP | Oui, natif | Oui, natif | Partiel selon les sources sectorielles | 
| Société éditrice | Anysphere | Microsoft / GitHub | Cognition AI (racheté fin 2025) | 

GitHub Copilot conserve un avantage de diffusion difficile à battre : selon la page officielle de GitHub Copilot, l'outil revendique "des millions d'utilisateurs individuels et des dizaines de milliers de clients entreprise", ce qui en fait l'outil IA pour développeurs le plus adopté au monde. GitHub cite aussi des gains mesurés en interne : jusqu'à 55 % de productivité en plus sur l'écriture de code sans perte de qualité, et jusqu'à 75 % de satisfaction professionnelle supplémentaire chez les développeurs qui l'utilisent. L'entreprise brésilienne Grupo Boticário est citée en exemple avec une hausse de productivité développeur de 94 % après adoption. Si vous démarrez tout juste avec l'IA en entreprise, notre tutoriel GitHub Copilot détaille sa propre installation pas à pas.

Windsurf occupe une position plus mouvementée : après l'échec d'un rachat par OpenAI à 3 milliards de dollars mi-2025 et un accord de licence de 2,4 milliards de dollars signé par Google DeepMind pour récupérer l'équipe fondatrice, la marque et la base de clients sont finalement passées sous le contrôle de Cognition AI, la société derrière l'agent autonome Devin, fin 2025. Les tarifs ont changé en mars 2026 avec un passage à des quotas d'usage plutôt qu'à des crédits fixes, ce qui rend certaines comparaisons de prix datées rapidement obsolètes. Pour une équipe qui évalue les trois options, mieux vaut vérifier les tarifs affichés le jour même sur chaque site officiel plutôt que de se fier à un comparatif figé, y compris celui-ci.

Le vrai critère de choix reste l'usage. Une équipe qui veut le maximum d'autonomie agentique et une gestion fine des règles de contexte penche vers Cursor AI. Une équipe déjà largement équipée en environnement Microsoft et JetBrains, avec un budget serré, reste souvent sur GitHub Copilot. Et pour les cas où le code ne doit jamais quitter l'infrastructure de l'entreprise, l'exécution locale via Ollama mérite d'être évaluée en complément, même si elle sacrifie une partie des capacités agentiques cloud.

## Déployer Cursor AI à l'échelle d'une équipe

Adopter Cursor AI pour un développeur seul prend une heure. Le déployer proprement sur une équipe de vingt personnes demande une méthode, sous peine de se retrouver avec vingt configurations différentes et zéro règle partagée.

1. **Commencez par un groupe pilote de trois à cinq développeurs** plutôt qu'un déploiement en une fois. Ce groupe teste les règles de projet, identifie les frictions et évite qu'une mauvaise configuration se propage à toute l'équipe.
2. **Versionnez les fichiers .cursor/rules et AGENTS.md dans le dépôt Git** comme n'importe quel autre fichier de configuration. Ils doivent passer en revue de code au même titre qu'un changement d'architecture, puisqu'ils influencent directement le comportement de l'IA pour toute l'équipe.
3. **Centralisez la facturation sur la formule Teams** plutôt que de laisser chaque développeur payer sa propre formule Pro. Le contrôle d'accès et le suivi de consommation deviennent alors visibles pour un seul administrateur au lieu d'être dispersés.
4. **Fixez un budget mensuel de crédits par équipe et surveillez-le dès la première semaine.** Les pics de consommation liés au mode Agent et aux Cloud Agents peuvent surprendre une organisation qui découvre l'outil, surtout si plusieurs développeurs lancent des tâches lourdes le même jour.
5. **Formez les nouvelles recrues sur les règles existantes avant de les laisser utiliser le mode Agent sans supervision.** Un développeur junior qui ne connaît pas encore les conventions du projet aura plus de mal à repérer un diff problématique généré par l'agent.

Un dernier point mérite l'attention des équipes françaises et européennes : la formule Teams inclut un mode confidentialité que beaucoup d'organisations laissent désactivé par défaut faute de savoir qu'il existe. Vérifiez ce réglage avant le déploiement plutôt qu'après un premier incident.

## Pièges courants à éviter avec Cursor AI

- **Laisser le mode Agent tourner sans relire les diffs.** L'agent peut produire du code qui compile et passe les tests tout en introduisant une faille de sécurité ou une régression fonctionnelle discrète. La relecture manuelle reste non négociable, même quand le résultat semble propre.
- **Oublier que les crédits Pro, Pro+ et Ultra ne sont pas illimités.** Un usage intensif du mode Agent sur de gros refactors peut consommer un quota mensuel en quelques jours. Surveillez la consommation dans le tableau de bord dès la première semaine d'usage.
- **Multiplier les fichiers .mdc sans jamais les auditer.** Des règles contradictoires entre deux fichiers produisent un comportement imprévisible de l'agent, qui doit arbitrer silencieusement entre deux instructions opposées.
- **Connecter un serveur MCP avec des clés API trop permissives.** Un serveur MCP relié à une base de données de production avec des droits d'écriture complets transforme une erreur de prompt en incident réel. Limitez les permissions au strict nécessaire.
- **Confondre AGENTS.md et .cursor/rules et dupliquer les instructions.** Les deux mécanismes cohabitent, mais des consignes contradictoires entre les deux fichiers créent de la confusion pour l'agent comme pour l'équipe.
- **Ignorer la confidentialité des données pour du code sous NDA ou soumis au RGPD.** Sans mode confidentialité activé, du code sensible peut transiter par des serveurs tiers. Vérifiez les réglages de rétention des données avant de connecter un dépôt sensible.

## Dépannage : les problèmes les plus fréquents et leurs solutions

| Problème | Cause probable | Solution | 
|---|---|---|
| Tab ne propose plus de suggestions | Modèle désactivé ou quota mensuel atteint | Vérifier Settings > Models et le solde de crédits restant | 
| Les règles .mdc ne s'appliquent pas | Extension .md utilisée au lieu de .mdc, ou frontmatter mal formé | Renommer le fichier en .mdc et valider la syntaxe YAML de l'en-tête | 
| L'agent boucle sur la même erreur | Contexte insuffisant ou règles contradictoires entre deux fichiers | Simplifier le prompt et vérifier les règles marquées alwaysApply | 
| L'import des paramètres VS Code échoue | Version de VS Code trop ancienne ou profil corrompu | Réinstaller les extensions manuellement depuis le marketplace intégré | 
| Le serveur MCP ne se connecte pas | Chemin de commande incorrect ou variable d'environnement manquante | Tester la commande du serveur en local, hors Cursor, avant de la déclarer | 
| Facturation Bugbot inattendue | Fonction à l'usage activée par défaut sur la formule Teams | Désactiver l'option dans les réglages d'équipe ou fixer un plafond mensuel | 
| Le Cloud Agent ne se déclenche pas depuis Slack | Intégration Slack non autorisée par l'administrateur du workspace | Réautoriser l'application Cursor dans les paramètres Slack de l'organisation | 
| Perte de contexte sur une longue conversation | Limite de fenêtre de contexte du modèle atteinte | Démarrer une nouvelle conversation et résumer l'essentiel dans une règle .mdc | 
| Synchronisation GitHub bloquée | Jeton d'accès expiré ou révoqué | Reconnecter le compte GitHub depuis les réglages du compte Cursor | 
| Latence élevée en mode Agent | Charge serveur élevée aux heures de pointe ou modèle premium saturé | Basculer temporairement sur un modèle alternatif dans Settings > Models | 

## Astuces avancées pour les développeurs expérimentés

- **Combinez règles globales et règles par dossier sur les monorepos.** Une règle à la racine fixe les conventions générales, des règles dans chaque sous-projet affinent le contexte sans surcharger l'ensemble.
- **Réservez le modèle le plus coûteux aux tâches qui le justifient.** Basculez sur un modèle plus léger pour la génération de tests ou la documentation, et gardez le modèle premium pour les refactors complexes ou l'architecture.
- **Scriptez le déclenchement de Cloud Agents via l'API pour les tâches récurrentes.** Mise à jour de dépendances, génération de changelog ou synchronisation de traductions peuvent tourner sans intervention humaine, sur un déclencheur planifié.
- **Combinez Cursor AI et l'exécution locale via Ollama pour les portions de code sensibles.** Gardez le cloud pour la majorité du travail et un modèle local pour les fichiers qui ne doivent jamais sortir de votre infrastructure.
- **Auditez régulièrement les serveurs MCP connectés.** Un serveur oublié avec des identifiants encore valides reste une porte d'entrée non surveillée sur vos systèmes.
- **Activez le mode confidentialité d'équipe avant tout onboarding.** Il est plus simple de l'imposer dès le départ que de revenir en arrière une fois que plusieurs développeurs ont déjà connecté des dépôts sensibles.
- **Documentez les prompts qui fonctionnent bien pour les tâches récurrentes.** Un prompt qui a donné un bon résultat sur une migration de base de données mérite d'être conservé dans un fichier partagé plutôt que réinventé à chaque fois par chaque développeur.

## Foire aux questions

**Cursor AI est-il gratuit ?**

La formule Hobby est gratuite et ne demande pas de carte bancaire, mais elle limite le nombre de requêtes Agent et de complétions Tab. Un usage professionnel quotidien nécessite en pratique une formule Pro à partir de 20 $/mois.

**Cursor AI est-il disponible en français ?**

L'interface est principalement en anglais, mais le chat et l'agent comprennent et répondent en français sans difficulté particulière. Vous pouvez rédiger vos règles de projet et vos prompts directement en français.

**Quelle est la différence entre .cursor/rules et AGENTS.md ?**

AGENTS.md est un fichier unique et simple, sans métadonnées, idéal pour démarrer. Les fichiers .mdc dans .cursor/rules permettent un ciblage précis par type de fichier grâce aux globs, et une activation conditionnelle via alwaysApply. Les deux mécanismes peuvent cohabiter dans un même projet.

**Cursor AI peut-il fonctionner hors ligne ?**

Non. L'éditeur lui-même s'ouvre sans connexion, mais toutes les fonctions IA (Tab, chat, Agent) nécessitent une connexion Internet pour interroger les modèles hébergés dans le cloud.

**Le code envoyé à Cursor AI sert-il à entraîner les modèles ?**

Cela dépend de vos réglages de confidentialité et de la formule choisie. Les formules Teams et Enterprise proposent un mode confidentialité renforcé et une rétention de données réduite. Vérifiez les paramètres de rétention avant de connecter un dépôt contenant des données sensibles ou soumises au RGPD.

**Peut-on utiliser sa propre clé API avec Cursor AI ?**

Oui, l'option est disponible dans Settings > Models. Elle permet de faire transiter les requêtes par votre propre contrat avec le fournisseur du modèle plutôt que de consommer les crédits inclus dans votre formule Cursor.

**Cursor AI remplace-t-il un développeur ?**

Non. L'outil accélère l'écriture, le refactoring et les tâches répétitives, mais les choix d'architecture, la relecture de sécurité et la validation métier restent une responsabilité humaine. Les pièges listés plus haut dans cet article montrent justement ce qui arrive quand cette relecture disparaît.

**Comment migrer de GitHub Copilot vers Cursor AI ?**

Installez Cursor AI, importez vos paramètres VS Code au premier lancement (étape 2 de ce tutoriel), puis reconstituez progressivement vos habitudes de prompt sous forme de règles .mdc ou AGENTS.md. La bascule technique prend quelques minutes, l'adaptation des réflexes d'équipe prend généralement une à deux semaines.

**Combien coûte Cursor AI pour une équipe de dix développeurs ?**

Sur la formule Teams à 40 $ par utilisateur et par mois, une équipe de dix développeurs paie 400 $ mensuels hors taxes, facturation centralisée incluse. Ce montant n'inclut pas les dépassements éventuels de crédits ni la revue de code Bugbot facturée à l'usage.

### Couverture connexe

Pour suivre l'actualité complète des outils de développement, retrouvez tous nos tutoriels dans la rubrique Logiciels.
