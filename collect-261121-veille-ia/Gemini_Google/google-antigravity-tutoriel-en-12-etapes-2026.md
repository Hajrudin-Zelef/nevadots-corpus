---
id: collect-261121-veille-ia/gemini-google/google-antigravity-tutoriel-en-12-etapes-2026
title: "Google Antigravity : Tutoriel en 12 Étapes [2026]"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-08-26"]
keywords: ["google", "antigravity", "vous", "code", "agents", "depuis", "selon", "taches"]
source: https://tech-insider.org/fr/antigravity-google-tutoriel-12-etapes-2026/
source_anchor: ""
source_lines: [1, 402]
section: "Gemini_Google"
---
Depuis le 18 novembre 2025, **Google Antigravity** a changé la donne pour les développeurs francophones. Lancé le même jour que Gemini 3 en version 1.0 et proposé sans frais (0 $) pendant l’aperçu public – avec, selon Google, un quota Gemini 3.1 Pro qui se rafraîchit toutes les 5 heures –, cet **IDE agentique** ne se contente pas de compléter votre code : il planifie, écrit, exécute, teste et vérifie des tâches entières pendant que vous supervisez. C’est le passage de l’autocomplétion à l’agent autonome, et l’outil a depuis bien évolué : une application de bureau et une CLI Antigravity 2.0 sont arrivées le 19 mai 2026 lors de Google I/O, la version stable est passée à la 2.1.1 le 22 juin 2026 selon Wikipédia, puis Google a enchaîné les versions 2.9.1, 2.10.0 et 2.11.0 en seulement sept jours entre le 20 et le 26 août 2026 – cette dernière ajoutant une UI générative pour du HTML inline. Dans ce tutoriel mis à jour en août 2026, vous allez installer Google Antigravity, le configurer et construire un projet complet – une application web de gestion de tâches – en 12 étapes, en environ 40 minutes.

Ce guide s’adresse aux développeurs de France et d’Europe qui veulent comprendre concrètement comment fonctionne un IDE « agent-first ». Nous couvrons les prérequis avec versions, le choix des modèles (Gemini 3 Pro, Claude Sonnet 4.6, GPT-OSS-120B), l’écriture d’un fichier `AGENTS.md`, la différence entre l’Éditeur et le Manager d’agents, la vérification dans le navigateur, plus une liste de pièges, un dépannage détaillé et une section tarifs. À la fin, vous disposerez d’un projet fonctionnel versionné avec Git.

## Qu’est-ce que Google Antigravity ?

**Google Antigravity** était à l’origine une plateforme de développement agentique construite sur un fork profondément modifié de Visual Studio Code. Concrètement, cela signifiait que vos extensions, thèmes et raccourcis VS Code fonctionnaient, tandis que l’expérience restait réorganisée autour d’agents IA autonomes. Mais depuis l’annonce d’Antigravity 2.0 le 19 mai 2026 lors de Google I/O, Google indique que cette base VS Code a été remplacée par une application de bureau autonome dédiée à l’orchestration des agents, tout en conservant la compatibilité avec vos extensions, thèmes et raccourcis existants. D’après le blog officiel Google pour les développeurs, l’outil reste « conçu pour vous faire opérer à un niveau supérieur, orienté tâche » plutôt que ligne par ligne.

La plateforme repose sur deux surfaces de travail. La première, l’**Éditeur** (Editor View), est une IDE classique dopée à l’IA, avec complétion par tabulation et commandes en ligne pour un flux synchrone – proche de ce que propose Cursor. La seconde, le **Manager** (Manager Surface, parfois appelée « Mission Control »), est une interface dédiée où vous lancez, orchestrez et observez plusieurs agents travaillant en parallèle et de manière asynchrone, sur différents espaces de travail. Depuis le lancement d’Antigravity 2.0 le 19 mai 2026 à l’occasion de Google I/O, ce Manager existe même en application de bureau autonome accompagnée d’une CLI dédiée, une première pour une surface entièrement consacrée à l’orchestration d’agents, selon le blog d’antigravity.google. C’est cette seconde surface qui distingue vraiment Antigravity de ses concurrents.

Les agents opèrent de bout en bout sur trois terrains : l’éditeur, le terminal et le navigateur. Ils écrivent du code, lancent l’application, testent des composants et itèrent sur l’interface sans intervention humaine constante. Plutôt que de vous noyer sous des journaux bruts, ils produisent des **Artifacts** : listes de tâches, plans d’implémentation, captures d’écran et même enregistrements du navigateur. Vous laissez vos commentaires directement sur ces Artifacts, et l’agent les intègre sans s’arrêter. Une base de connaissances permet en outre aux agents de sauvegarder du contexte et des extraits utiles pour améliorer les tâches suivantes.

Selon la page Wikipédia consacrée à Google Antigravity, la plateforme englobe une IDE, une CLI et un SDK pour construire vos propres agents ; TechCrunch confirme que le lancement d’Antigravity 2.0, le 19 mai 2026, a formalisé cette offre autour de trois briques – l’application de bureau, la CLI et le SDK – toutes dédiées à l’orchestration d’agents. Comme l’analyse le média spécialisé The New Stack, Antigravity a démarré comme un concurrent de Cursor avant de devenir une plateforme de développement à part entière, avec orchestration multi-agents. Elle s’inscrit dans la vague du vibe coding qui a redéfini le développement logiciel en 2026, où l’on décrit une intention et où l’agent produit le code. Antigravity pousse cette logique jusqu’à l’orchestration de plusieurs agents simultanés.

## Google Antigravity face à Cursor, Windsurf et Copilot

Avant de plonger dans l’installation, il faut situer **Google Antigravity** dans l’écosystème. Trois différences majeures ressortent : l’orchestration multi-agents asynchrone via le Manager, la gratuité pendant l’aperçu public, et l’intégration native à l’écosystème Google (AI Studio, Firebase, Android). Là où Cursor et Windsurf misent sur un flux à agent unique enrichi, Antigravity assume une posture d’agent autonome de premier niveau, avec une IDE reléguée au rang d’outil parmi d’autres.

| Critère | Google Antigravity | Cursor | Windsurf | GitHub Copilot | 
|---|---|---|---|---|
| Approche | Agent-first, multi-agents | Éditeur + agent | Éditeur + Cascade | Assistant intégré | 
| Orchestration parallèle | Oui (Manager) | Limitée | Limitée | Non | 
| Modèles principaux | Gemini 3, Claude 4.6, GPT-OSS | Multi-modèles | Multi-modèles | GPT, Claude, Gemini | 
| Contrôle du navigateur | Natif (sous-agent) | Partiel | Partiel | Non | 
| Base VS Code | Fork modifié | Fork modifié | Fork modifié | Extension | 
| Prix de départ | Gratuit (aperçu) | Payant après essai | Gratuit limité | Payant | 

Si vous hésitez encore entre ces outils, deux comparatifs approfondis existent sur le site : Windsurf vs Cursor 2026 et Claude Code vs Cursor 2026. Ils détaillent les scores SWE-bench et les fenêtres de contexte. Antigravity se positionne comme une troisième voie, plus orientée délégation. À noter : la plateforme a subi plusieurs réductions de quotas depuis son lancement, un point sur lequel nous reviendrons dans la section tarifs.

Un dernier repère : contrairement à GitHub Copilot, étroitement lié à la chaîne d’outils Microsoft, Antigravity mise sur l’écosystème Google et sur le modèle Gemini 3 Pro comme moteur par défaut. Le résultat est un outil plus autonome, mais aussi plus gourmand en quotas – d’où l’importance de bien le configurer, ce que nous faisons dès maintenant.

## Prérequis : matériel, système et versions

Dès son lancement public le 18 novembre 2025, Google Antigravity était disponible sur les trois grandes familles de systèmes d’exploitation – Windows, macOS et Linux –, selon le blog officiel Google Antigravity. Vérifiez que votre machine respecte les versions minimales officielles ci-dessous avant de télécharger quoi que ce soit. Pour le projet de démonstration, nous utiliserons également Python et un navigateur Chromium pour le sous-agent navigateur.

| Prérequis | Version minimale | Recommandé | Rôle | 
|---|---|---|---|
| Windows | Windows 10 64 bits | Windows 11 | Système hôte | 
| macOS | Monterey 12 | Sonoma 14+ | Système hôte | 
| Linux | 64 bits, glibc 2.28+ | Ubuntu 22.04+ | Système hôte | 
| Compte Google | Compte personnel | Compte personnel | Authentification, aperçu gratuit | 
| RAM | 8 Go | 16 Go+ | Agents et éditeur | 
| Espace disque | 4 Go libres | 10 Go+ | Installation et projets | 
| Python (démo) | 3.11 | 3.12+ | API du projet | 
| Navigateur | Chrome/Chromium récent | Chrome à jour | Sous-agent navigateur | 

Points d’attention pour l’Europe : l’aperçu gratuit requiert un **compte Google personnel** (les comptes Workspace scolaires ou d’entreprise peuvent être bloqués par votre administrateur). Aucune carte bancaire n’est demandée pour la version gratuite. Sous Linux, la page de téléchargement d’Antigravity liste toujours, pour la version 2.11.0 mise en ligne en août 2026, les mêmes contraintes `glibc 2.28+` et `glibcxx 3.4.25+` qui excluent les distributions très anciennes ; une Ubuntu 22.04 LTS ou plus récente ne pose aucun problème. Enfin, prévoyez une connexion stable : les agents dialoguent en continu avec les serveurs de Google.

Pour le projet complet de ce tutoriel, installez Python 3.11 ou supérieur et vérifiez sa présence avec `python3 --version`. Nous n’aurons besoin d’aucune base de données externe : l’application de gestion de tâches utilisera SQLite, embarqué avec Python. Le navigateur Chrome est requis uniquement pour l’étape de vérification automatisée par le sous-agent navigateur.

## Étapes 1 et 2 : télécharger et installer Google Antigravity

**Étape 1 – Téléchargement.** Rendez-vous sur antigravity.google et cliquez sur « Get started » (Commencer). La page détecte votre système d’exploitation et propose l’installeur adapté : `.exe` pour Windows, `.dmg` pour macOS, `.deb` ou archive `.tar.gz` pour Linux. Vous pouvez aussi passer par un gestionnaire de paquets, ce qui simplifie les mises à jour. Voici les commandes selon votre plateforme :

```
# macOS (Homebrew)
brew install --cask antigravity
# Windows (winget, PowerShell)
winget install Google.Antigravity
# Linux (Debian/Ubuntu, après téléchargement du .deb)
sudo apt install ./antigravity_latest_amd64.deb
# Linux (archive générique)
tar -xzf antigravity-linux-x64.tar.gz -C ~/apps/
~/apps/antigravity/antigravity
```
**Étape 2 – Installation.** Lancez l’installeur téléchargé et suivez les invites standard. Sous macOS, glissez l’application dans le dossier *Applications* puis autorisez son exécution dans *Réglages Système > Confidentialité et sécurité* si Gatekeeper la bloque. Sous Windows, si SmartScreen affiche un avertissement, cliquez sur « Informations complémentaires » puis « Exécuter quand même » (l’aperçu public n’est pas encore signé par tous les certificats). Au premier lancement, l’application vérifie les mises à jour ; laissez-la se mettre à jour vers la dernière version disponible.

Sortie attendue au premier démarrage dans le terminal (installation Linux via archive) :

```
[antigravity] starting…
[antigravity] checking for updates… up to date
[antigravity] no workspace open – showing welcome screen
[antigravity] sign-in required to access model providers
```
Si l’application se ferme immédiatement sous Linux, c’est presque toujours une question de dépendances graphiques manquantes (`libnss3`, `libgbm1`). Nous traitons ce cas précis dans la section dépannage. Une fois l’écran d’accueil affiché, vous êtes prêt pour la connexion.

## Étapes 3 et 4 : connexion Google et import de votre config VS Code

**Étape 3 – Connexion.** Sur l’écran d’accueil, cliquez sur « Sign in with Google ». Le navigateur par défaut s’ouvre pour l’authentification OAuth. Utilisez un **compte Google personnel** : c’est lui qui débloque l’accès gratuit à la gamme complète de modèles pendant l’aperçu public. Après validation, une redirection ramène automatiquement vers Antigravity. Vous voyez alors apparaître votre quota de départ et la liste des fournisseurs de modèles disponibles.

**Étape 4 – Import de votre configuration VS Code.** Comme Antigravity est un fork de VS Code, il propose d’importer vos extensions, thèmes et raccourcis lors du premier lancement. Acceptez si vous venez de VS Code : vos réglages (linters, formateurs, palette de commandes) sont conservés. Vous pouvez aussi choisir un thème et une disposition. Si vous utilisez déjà Claude Code, sachez qu’un même fichier de contexte projet est réutilisable d’un outil à l’autre, ce qui évite de tout reconfigurer.

Créez maintenant le dossier de travail du projet et ouvrez-le dans Antigravity (menu *File > Open Folder*). C’est dans ce dossier que l’agent va écrire tout le code de l’application de gestion de tâches.

```
mkdir -p ~/projets/gestionnaire-taches
cd ~/projets/gestionnaire-taches
git init
# Puis dans Antigravity : File > Open Folder > ~/projets/gestionnaire-taches
```
Initialiser Git dès maintenant est une bonne pratique : Antigravity crée des points de contrôle (checkpoints) au fil des modifications, et un dépôt Git vous donne un filet de sécurité supplémentaire pour revenir en arrière si un agent part dans une mauvaise direction. Nous ferons un premier commit après la génération du projet.

## Étape 5 : choisir le bon modèle IA

Le choix du modèle conditionne la vitesse, la qualité et la consommation de quota. Google Antigravity expose un sélecteur de modèles (icône du modèle dans la barre de l’agent, ou *Settings > Model providers*). Le moteur par défaut appartient à la famille Gemini 3, lancée par Google pour les développeurs en même temps qu’Antigravity, et depuis le 20 mai 2026, au lendemain de l’ouverture de Google I/O 2026, **Gemini 3.5 Flash** est disponible en accès général aussi bien dans Antigravity que via l’API Gemini – l’une des quelque 100 annonces faites par Google à cette occasion –, présenté par Google comme jusqu’à 4 fois plus rapide que les autres modèles de pointe – et Google est allé plus loin en intégrant, dès août 2026, **Gemini 3.6 Flash** comme nouveau modèle rapide par défaut, selon Emergent. De son côté, **Gemini 3.1 Pro** offre désormais une fenêtre de contexte pouvant atteindre 1 000 000 de tokens, précieuse sur les gros projets. Vous pouvez même assigner un modèle différent à chaque agent d’une même mission. Voici la gamme disponible d’après la documentation officielle et la fiche Wikipédia à jour en 2026.

| Modèle | Éditeur | Idéal pour | Coût en quota | 
|---|---|---|---|
| Gemini 3 Pro |  | Raisonnement, planification, gros contexte | Élevé | 
| Gemini 3 Flash |  | Itérations rapides, édition ciblée | Faible | 
| Claude Sonnet 4.6 | Anthropic | Code robuste, refactorisation | Moyen | 
| Claude Opus 4.6 | Anthropic | Tâches complexes, architecture | Élevé | 
| GPT-OSS-120B | OpenAI (open-weight) | Autonomie, coût maîtrisé | Faible à moyen | 

Notre recommandation pratique, mise à jour après le passage à Antigravity 2.0 : utilisez **Gemini 3.1 Pro**, avec sa fenêtre de contexte de 1 000 000 de tokens, pour la phase de planification sur les gros projets (l’agent doit décomposer le projet), puis basculez sur **Gemini 3.6 Flash** – la version rapide mise à jour en août 2026, qui succède à Gemini 3.5 Flash lancé à l’I/O et annoncé jusqu’à 4 fois plus rapide – ou **Claude Sonnet 4.6** pour les itérations d’écriture et de correction, moins coûteuses en quota. Réservez **Claude Opus 4.6** aux décisions d’architecture délicates. Pour comparer plus finement les modèles Gemini face à la concurrence, consultez notre analyse Grok 4 vs ChatGPT vs Gemini 3.

### Activer Claude avec votre propre clé API

Les modèles Claude peuvent transiter par votre propre compte Anthropic. Dans *Settings > Model providers > Anthropic*, collez votre clé API. Cela déporte la facturation Claude sur votre compte Anthropic et libère du quota Google. C’est utile si vous atteignez régulièrement les limites de l’aperçu gratuit. Définissez ensuite Claude Sonnet 4.6 comme modèle par défaut d’un agent donné pour tester ce routage.

```
# Exemple : définir la clé Anthropic via une variable d'environnement
# (à placer avant de lancer Antigravity depuis un terminal)
export ANTHROPIC_API_KEY="sk-ant-votre-cle-ici"
# Vérifier qu'elle est bien exportée
echo $ANTHROPIC_API_KEY | cut -c1-7
# Sortie attendue : sk-ant-
```
## Étape 6 : cadrer l’agent avec un fichier AGENTS.md

Un agent autonome sans règles produit du code incohérent. La solution est le fichier `AGENTS.md`, une convention désormais partagée par de nombreux outils agentiques : un document Markdown, placé à la racine du projet, qui décrit les conventions, la pile technique, les commandes de test et les interdits. Antigravity lit ce fichier au début de chaque mission et s’y conforme. C’est l’équivalent d’un cahier des charges permanent pour vos agents.

Créez le fichier `AGENTS.md` à la racine du dossier `gestionnaire-taches`. Voici un modèle complet pour notre projet :

```
# AGENTS.md – Gestionnaire de tâches
## Objectif
Construire une application web de gestion de tâches (to-do) avec une API REST
et une interface HTML/JS minimale, sans framework front lourd.
## Pile technique
- Backend : Python 3.11+, FastAPI, base SQLite (fichier taches.db)
- Front : HTML + JavaScript vanilla (fetch), aucun build
- Tests : pytest
## Conventions
- Code et commentaires en français.
- Noms de variables explicites, pas d'abréviations obscures.
- Chaque endpoint documenté avec un docstring.
- Aucune dépendance non listée dans requirements.txt.
## Commandes
- Installer : pip install -r requirements.txt
- Lancer : uvicorn main:app --reload
- Tester : pytest -q
## Interdits
- Ne pas ajouter de base de données externe (Postgres, MySQL).
- Ne pas exposer de secret en clair dans le code.
- Ne pas modifier ce fichier AGENTS.md sans validation.
```
Ce fichier fait deux choses essentielles. D’abord, il verrouille la pile technique : sans lui, un agent pourrait choisir Django, Flask ou Node selon son humeur. Ensuite, il impose des garde-fous (« Interdits ») qui réduisent drastiquement les dérapages. Complétez-le avec la **base de connaissances** intégrée : quand un agent découvre une information utile (une commande, un choix de conception), il peut la sauvegarder pour les tâches suivantes, ce qui renforce la cohérence au fil du projet.

Astuce : gardez `AGENTS.md` court et impératif. Un fichier de 30 lignes bien ciblées est plus efficace qu’un pavé de 300 lignes que l’agent survole. Mettez-le à jour à mesure que le projet grandit, mais restez concis.

## Étape 7 : Éditeur ou Manager, deux façons de travailler

Google Antigravity propose deux modes de travail qu’il faut bien distinguer pour être efficace. Le choix dépend de la nature de la tâche : édition fine et synchrone, ou délégation autonome et asynchrone.

### La vue Éditeur : le flux synchrone

La vue Éditeur ressemble à VS Code : arborescence de fichiers, éditeur central, terminal intégré, et une barre latérale d’agent. Vous y bénéficiez de la complétion par tabulation et de commandes en ligne (sélectionnez du code, décrivez la modification, l’agent l’applique). C’est le mode idéal pour les retouches précises, la lecture de code, le débogage pas à pas – bref, tout ce qui demande votre présence active. Comparé à Cursor, l’expérience est très familière.

### Le Manager : l’orchestration asynchrone

Le Manager (ou « Mission Control ») est le cœur de la promesse d’Antigravity. Vous y lancez une **mission** : une description de haut niveau que l’agent transforme en plan, puis exécute pendant que vous faites autre chose. Vous pouvez démarrer plusieurs missions en parallèle – par exemple un agent qui écrit le backend et un autre qui prépare les tests – chacune sur son propre espace de travail ; dès le lancement d’Antigravity 2.0 le 19 mai 2026, Google a présenté sa plateforme comme un environnement agent-first unifié capable de faire tourner plusieurs agents IA en parallèle – jusqu’à cinq de front, propulsés par Gemini 3.5 Flash, selon Emergent. Le Manager affiche l’avancement, les Artifacts produits et les points où votre validation est requise.

Pour ce tutoriel, nous utiliserons le Manager pour la génération initiale du projet (une tâche large et autonome), puis la vue Éditeur pour les corrections fines. C’est la combinaison la plus productive : déléguer le gros œuvre, garder la main sur les finitions.

## Étape 8 : lancer votre première mission (le projet complet)

C’est le moment clé. Ouvrez le Manager, cliquez sur « New mission » et rédigez une consigne claire. La qualité du prompt détermine la qualité du résultat : soyez précis sur le quoi, laissez l’agent décider du comment (le fichier `AGENTS.md` cadre déjà les choix techniques). Voici la mission que nous utilisons :

```
Mission : Créer une application de gestion de tâches complète.
Besoins fonctionnels :
1. API REST FastAPI avec ces endpoints :
   - GET /taches : liste toutes les tâches
   - POST /taches : crée une tâche { "titre": str, "faite": bool }
   - PUT /taches/{id} : met à jour une tâche
   - DELETE /taches/{id} : supprime une tâche
2. Stockage SQLite dans taches.db, créé automatiquement au démarrage.
3. Une page index.html servie à la racine, avec un champ d'ajout,
   la liste des tâches, une case à cocher "faite" et un bouton supprimer.
4. Un fichier requirements.txt et un fichier de tests pytest couvrant
   la création et la suppression d'une tâche.
Respecte AGENTS.md. Propose d'abord un plan, puis implémente.
```
Sélectionnez Gemini 3 Pro pour cette mission (planification + génération), puis lancez. L’agent commence par produire un **plan d’implémentation** sous forme d’Artifact : arborescence des fichiers, ordre des tâches, choix de conception. Validez le plan (ou corrigez-le) avant l’exécution. L’agent écrit ensuite les fichiers un à un. Voici un extrait représentatif du backend généré :

```
# main.py – extrait généré par l'agent
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
import sqlite3
app = FastAPI(title="Gestionnaire de taches")
def get_db():
    conn = sqlite3.connect("taches.db")
    conn.row_factory = sqlite3.Row
    return conn
def init_db():
    with get_db() as db:
        db.execute(
            "CREATE TABLE IF NOT EXISTS taches ("
            "id INTEGER PRIMARY KEY AUTOINCREMENT, "
            "titre TEXT NOT NULL, faite INTEGER DEFAULT 0)"
        )
class Tache(BaseModel):
    titre: str
    faite: bool = False
@app.on_event("startup")
def demarrage():
    init_db()
@app.get("/taches")
def lister_taches():
    """Retourne la liste complète des tâches."""
    with get_db() as db:
        lignes = db.execute("SELECT * FROM taches").fetchall()
        return [dict(l) for l in lignes]
@app.post("/taches")
def creer_tache(tache: Tache):
    """Crée une nouvelle tâche et renvoie son identifiant."""
    with get_db() as db:
        cur = db.execute(
            "INSERT INTO taches (titre, faite) VALUES (?, ?)",
            (tache.titre, int(tache.faite)),
        )
        return {"id": cur.lastrowid, "titre": tache.titre, "faite": tache.faite}
```
Remarquez que le code respecte le `AGENTS.md` : français, SQLite, docstrings, aucune dépendance surprise. L’agent poursuit avec les endpoints PUT et DELETE, la page `index.html`, le `requirements.txt` et les tests. Pendant qu’il travaille, il produit des Artifacts que nous allons maintenant lire.

## Étape 9 : lire et valider les Artifacts

Les **Artifacts** sont la manière dont Antigravity rend le travail des agents vérifiable, au lieu de vous inonder de journaux techniques. Chaque mission en produit plusieurs types, que vous consultez dans un onglet dédié du Manager.

- **Liste de tâches** : la décomposition du travail, avec l’état d’avancement de chaque sous-tâche (à faire, en cours, terminé).
- **Plan d’implémentation** : les décisions de conception, l’arborescence des fichiers, l’ordre d’exécution. C’est le document que vous validez avant l’écriture.
- **Captures d’écran** : des images de l’interface au fur et à mesure, pratiques pour repérer un problème visuel sans lancer l’application.
- **Enregistrements du navigateur** : de courtes séquences d’actions montrant l’agent qui teste l’application dans Chrome.

L’atout majeur : vous laissez vos commentaires **directement sur l’Artifact**, et l’agent les intègre sans interrompre son exécution. Par exemple, sur le plan d’implémentation, vous pouvez annoter « Ajoute une validation : le titre ne doit pas être vide » et l’agent ajustera le code en conséquence. Cette boucle de rétroaction non bloquante est ce qui rend le travail multi-agents réellement gérable.

Prenez l’habitude de lire le plan d’implémentation *avant* de laisser l’agent coder pendant dix minutes. Corriger une hypothèse erronée au stade du plan coûte quelques secondes ; la corriger après 300 lignes générées coûte bien plus. C’est la discipline centrale du travail avec un IDE agentique.

## Étape 10 : vérifier l’application avec le sous-agent navigateur

La force distinctive d’Antigravity est sa capacité à **contrôler un navigateur** pour vérifier ce qu’il a construit. Le sous-agent navigateur ouvre Chrome, charge l’application, clique, remplit des champs et confirme que tout fonctionne – puis produit un enregistrement en Artifact. Pour l’activer, installez l’extension Chrome d’Antigravity depuis la boîte de dialogue proposée au premier usage du navigateur, et autorisez la connexion entre l’IDE et le navigateur.

Une fois le backend et le front générés, demandez à l’agent de lancer et de vérifier l’application :

```
Consigne à l'agent :
Lance l'application (uvicorn main:app --reload), ouvre http://127.0.0.1:8000
dans le navigateur, ajoute une tâche "Acheter du pain", coche-la comme faite,
puis supprime-la. Confirme par une capture que la liste est vide à la fin.
```
L’agent démarre le serveur, ouvre l’URL, exécute le scénario et enregistre la séquence. Voici l’extrait de front généré qui alimente la page (à titre d’illustration du résultat) :

```
<!-- index.html – extrait généré -->
<script>
async function chargerTaches() {
  const r = await fetch('/taches');
  const taches = await r.json();
  const liste = document.getElementById('liste');
  liste.innerHTML = '';
  for (const t of taches) {
    const li = document.createElement('li');
    li.textContent = t.titre + (t.faite ? ' ✓' : '');
    liste.appendChild(li);
  }
}
async function ajouter() {
  const titre = document.getElementById('titre').value;
  await fetch('/taches', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ titre, faite: false })
  });
  chargerTaches();
}
chargerTaches();
</script>
```
Attention à la sécurité : donner à un agent le contrôle du navigateur et du terminal est puissant mais sensible. Un agent qui navigue sur des pages tierces peut être exposé à des attaques par **injection de prompt** (des instructions cachées dans une page web tentant de détourner l’agent). Limitez le sous-agent navigateur à des URL de confiance (votre application locale), et ne le laissez pas parcourir librement le web avec des accès sensibles. Nous détaillons ces précautions dans la section astuces avancées.

## Étapes 11 et 12 : réviser, tester, committer et livrer

**Étape 11 – Réviser et itérer.** Repassez en vue Éditeur pour relire le code généré. Antigravity affiche les modifications sous forme de *diffs* que vous approuvez ou rejetez. Si un endpoint ne vous convient pas, sélectionnez-le et lancez une commande en ligne (« ajoute une gestion d’erreur 404 si l’id n’existe pas »). Les points de contrôle (checkpoints) automatiques vous permettent de revenir à un état antérieur en un clic si une itération casse quelque chose. C’est le moment d’affiner, pas de tout regénérer.

**Étape 12 – Tester et livrer.** Lancez la suite de tests générée pour valider le projet complet. Dans le terminal intégré :

```
pip install -r requirements.txt
pytest -q
```
Sortie attendue :

```
....                                                      [100%]
4 passed in 0.42s
```
Une fois les tests au vert, versionnez le projet. Antigravity peut rédiger le message de commit pour vous, mais vous pouvez aussi le faire manuellement :

```
git add .
git commit -m "feat: gestionnaire de taches (API FastAPI + front + tests)"
# Vérifier l'application une dernière fois
uvicorn main:app --reload
# Ouvrir http://127.0.0.1:8000 dans le navigateur
```
Félicitations : vous disposez d’un projet complet, testé et versionné, généré et vérifié par des agents. Vous pouvez désormais le déployer (par exemple sur un hébergeur compatible ASGI) ou l’enrichir avec de nouvelles missions. Le tout, sans avoir écrit une seule ligne à la main – mais en gardant le contrôle à chaque étape grâce aux plans, aux diffs et aux Artifacts.

## 5 pièges courants à éviter avec Google Antigravity

Les IDE agentiques transforment la productivité, mais ils introduisent aussi de nouveaux pièges. Voici les cinq erreurs les plus fréquentes chez les débutants – et comment les éviter.

1. **Lancer une mission sans AGENTS.md.** Sans règles, l’agent improvise la pile technique et les conventions. Résultat : du code incohérent, parfois des dépendances non désirées. Rédigez toujours`AGENTS.md` avant la première mission.
2. **Ne pas relire le plan d’implémentation.** Approuver un plan à la va-vite, c’est laisser l’agent bâtir sur une mauvaise fondation. Lisez le plan, corrigez-le, puis seulement laissez l’agent coder.
3. **Épuiser son quota sur des tâches triviales.** Utiliser Gemini 3 Pro ou Claude Opus 4.6 pour renommer une variable gaspille du quota. Réservez les gros modèles à la planification et à l’architecture ; passez sur Flash pour les petites itérations.
4. **Laisser le sous-agent navigateur en liberté.** Un agent qui parcourt le web peut subir une injection de prompt. Cantonnez-le à vos URL locales et de confiance.
5. **Ignorer Git et les checkpoints.** Sur une session longue, un agent peut casser du code qui marchait. Sans commits réguliers, vous perdez du temps à défaire les dégâts. Committez après chaque étape stable.

Un sixième piège mérite d’être cité : la **confiance aveugle**. Un agent produit du code plausible, pas forcément correct. Les tests générés automatiquement ne couvrent pas tout. Gardez un œil critique, surtout sur la logique métier et la sécurité. L’IDE agentique est un accélérateur, pas un pilote automatique infaillible.

## Dépannage : 8 problèmes fréquents et leurs solutions

Voici les incidents les plus courants rencontrés lors de l’installation et de l’utilisation de Google Antigravity, avec la cause probable et la solution.

| Problème | Cause probable | Solution | 
|---|---|---|
| L’application ne démarre pas sous Linux | Dépendances graphiques manquantes | Installer `libnss3` ,`libgbm1` ,`libasound2` via apt | 
| Impossible de se connecter avec Google | Compte Workspace bloqué par l’admin | Utiliser un compte Google personnel | 
| « Rate limit reached » | Quota gratuit épuisé | Attendre le rafraîchissement (~5 h) ou passer sur Flash / clé Anthropic | 
| L’agent perd le fil en cours de tâche | Contexte trop long, erreur mémoire | Découper la mission, revenir à un checkpoint | 
| Le sous-agent navigateur n’ouvre rien | Extension Chrome non installée | Installer l’extension et autoriser la connexion IDE ↔ navigateur | 
| SmartScreen bloque l’installeur (Windows) | Binaire d’aperçu non signé | « Informations complémentaires » > « Exécuter quand même » | 
| Les modèles Claude sont grisés | Clé API Anthropic absente | Ajouter la clé dans Settings > Model providers | 
| Les extensions VS Code manquent | Import non effectué au 1er lancement | Réimporter via la palette (Import VS Code settings) | 

Deux cas méritent un mot de plus. Pour l’erreur de **quota**, gardez en tête que les limites de l’aperçu gratuit ont été revues à la baisse plusieurs fois depuis novembre 2025 ; si vous travaillez intensivement, router Claude via votre propre clé API ou souscrire à un plan payant devient vite nécessaire. Pour la **perte de contexte**, la meilleure parade reste de découper : une mission bien bornée réussit là où une mission fourre-tout dérape. Enfin, si Antigravity se fige (UI freeze) sur une opération longue, redémarrez l’application – vos fichiers et checkpoints sont conservés sur le disque.

## Astuces avancées pour aller plus loin

Une fois à l’aise avec le flux de base, ces techniques vous feront gagner en efficacité et en sûreté.

- **Agents en parallèle.** Dans le Manager, lancez une mission « backend » et une mission « tests et documentation » simultanément, sur des espaces de travail distincts. Vous divisez le temps total sans mélanger les contextes.
- **Modèle par agent.** Assignez Gemini 3 Pro à l’agent d’architecture et Gemini 3 Flash aux agents d’exécution. Vous optimisez le rapport qualité/quota mission par mission.
- **Base de connaissances vivante.** Encouragez l’agent à consigner les décisions importantes (choix de bibliothèque, conventions d’API) dans la base de connaissances. Les missions suivantes en héritent, ce qui améliore la cohérence.
- **Durcir la sécurité.** Traitez tout contenu web comme non fiable. Ne connectez pas de secrets de production à un agent qui contrôle le navigateur. Révisez les diffs avant d’exécuter des commandes système sensibles, et privilégiez un environnement isolé (conteneur, machine virtuelle) pour les projets sensibles.
- **Gérer les crédits.** Depuis mars 2026, Google propose des crédits IA (25 $ pour 2 500 crédits, soit 0,01 $ le crédit) en complément des quotas. Surveillez votre consommation dans les réglages pour éviter les mauvaises surprises en fin de projet.

Enfin, gardez `AGENTS.md` comme document vivant. À mesure que le projet grossit, ajoutez-y les décisions structurantes (choix de base de données réel, conventions de nommage des routes, stratégie de tests). Un `AGENTS.md` bien tenu est le meilleur multiplicateur de qualité pour un IDE agentique – bien plus qu’un prompt parfait pour une mission isolée. Pour approfondir l’usage d’assistants en ligne de commande complémentaires, voyez notre tutoriel Gemini CLI.

## Tarifs, crédits et limites en 2026

Google Antigravity reste **gratuit pendant l’aperçu public** – 0 $ pour les particuliers dès le lancement de novembre 2025, selon le Google Developers Blog –, mais l’histoire de ses quotas est mouvementée. Le blog officiel d’antigravity.google précisait déjà, en novembre 2025, que les limites de l’aperçu pour Gemini 3/3.1 Pro se rafraîchissaient toutes les 5 heures ; depuis, ces limites ont été réduites à plusieurs reprises : d’après les rapports d’utilisateurs, le plafond initial d’environ 250 requêtes par jour a été ramené à une vingtaine dès décembre 2025, avant un passage à des limites hebdomadaires. En mars 2026, Google a introduit un système de crédits IA. À l’occasion de Google I/O 2026 (19-20 mai 2026), l’entreprise a lancé l’abonnement **Google AI Ultra** à 100 $/mois, pensé pour donner un accès prioritaire à Antigravity avec des limites d’usage cinq fois supérieures à celles du plan Pro, selon Google ; puis, en août 2026, un palier Ultra encore plus élevé est passé de 250 $ à 200 $/mois tout en offrant des limites vingt fois supérieures au plan Pro, d’après Emergent.

| Offre | Prix (indicatif) | Accès aux modèles | Limites | 
|---|---|---|---|
| Gratuit (aperçu) | 0 $ | Gamme complète | Quota compute, rafraîchi ~toutes les 5 h | 
| Google AI Pro | 20 $/mois | Gamme complète | Limites élevées, priorité | 
| Google AI Ultra | 100 $/mois | Gamme complète | 5× les limites du plan Pro | 
| Google AI Ultra Max | 200 $/mois | Gamme complète | 20× les limites Pro (baissé de 250 $ à 200 $ en août 2026, selon Emergent) | 
| Crédits IA | 25 $ / 2 500 crédits | Complément | 0,01 $ par crédit | 

Pour les utilisateurs européens, ces tarifs en dollars sont facturés à leur équivalent en euros par Google. Notre conseil pour l’aperçu gratuit : réservez-le à l’apprentissage et aux petits projets. Dès que vous travaillez quotidiennement avec des agents, soit vous routez Claude via votre propre clé Anthropic, soit vous passez sur un plan payant. Les allers-retours de quotas de fin 2025 ont montré que compter uniquement sur la gratuité pour un usage professionnel est risqué. Pour situer ces coûts face à d’autres assistants, notre comparatif Copilot vs ChatGPT donne des points de repère utiles.

## Foire aux questions

### Google Antigravity est-il vraiment gratuit ?

Oui, pendant l’aperçu public, avec un compte Google personnel et sans carte bancaire. Vous accédez à la gamme complète de modèles, mais dans les limites d’un quota qui se rafraîchit environ toutes les 5 heures. Ce quota a été réduit plusieurs fois depuis novembre 2025 ; pour un usage intensif, un plan payant ou une clé API Anthropic devient nécessaire.

### Quels modèles IA Antigravity prend-il en charge ?

D’après la documentation officielle et la fiche Wikipédia à jour en 2026 : Gemini 3 Pro (par défaut), Gemini 3 Flash, Claude Sonnet 4.6, Claude Opus 4.6 et GPT-OSS-120B. Vous pouvez assigner un modèle différent à chaque agent d’une même mission, et router Claude via votre propre compte Anthropic.

### Puis-je réutiliser mes extensions VS Code ?

Oui. Antigravity étant un fork profondément modifié de Visual Studio Code, vos extensions, thèmes et raccourcis sont importables au premier lancement. Si l’import n’a pas eu lieu, relancez-le via la palette de commandes (« Import VS Code settings »).

### Quelle différence entre l’Éditeur et le Manager ?

L’Éditeur est une IDE synchrone classique (complétion, commandes en ligne) pour le travail que vous supervisez activement. Le Manager orchestre des agents autonomes et asynchrones, en parallèle, sur plusieurs espaces de travail. On délègue le gros œuvre au Manager et on peaufine dans l’Éditeur.

### Antigravity fonctionne-t-il hors ligne ?

Non pour les fonctions IA : les agents dialoguent avec les serveurs de Google (ou d’Anthropic pour Claude). L’éditeur de base reste utilisable hors ligne comme n’importe quel VS Code, mais la complétion IA, les missions et le sous-agent navigateur exigent une connexion internet.

### Quels sont les risques de sécurité ?

Le principal risque des IDE agentiques est l’injection de prompt : un agent qui lit des contenus web non fiables peut être manipulé. Comme l’agent contrôle aussi le terminal et le navigateur, limitez ses accès, révisez les diffs avant d’exécuter des commandes sensibles et travaillez de préférence dans un environnement isolé pour les projets sensibles.

### Antigravity remplace-t-il Cursor ou Copilot ?

Pas exactement : il vise un autre usage. Cursor et Copilot excellent dans l’assistance à l’écriture au fil de l’eau ; Antigravity brille dans la délégation de tâches entières à des agents autonomes. Beaucoup de développeurs les combinent selon le contexte. Nos comparatifs Windsurf vs Cursor et Claude Code vs Cursor aident à choisir.

### Sur quels systèmes Antigravity est-il disponible ?

Sur Windows 10 64 bits et plus, macOS Monterey 12 et plus, et Linux 64 bits (glibc 2.28+, glibcxx 3.4.25+). Une Ubuntu 22.04 LTS ou plus récente convient parfaitement. Le téléchargement se fait sur antigravity.google.

### Lectures complémentaires

**En résumé** : Google Antigravity incarne le tournant agentique du développement. En 12 étapes, vous l’avez installé, configuré avec `AGENTS.md`, connecté aux modèles Gemini 3 et Claude 4.6, puis utilisé pour construire, vérifier et livrer une application complète. La clé n’est pas de tout déléguer aveuglément, mais d’orchestrer : cadrer les agents, lire les plans, valider les Artifacts et garder Git comme filet de sécurité. Avec cette discipline, l’IDE agentique devient un véritable multiplicateur de productivité pour les développeurs français et européens en 2026.
