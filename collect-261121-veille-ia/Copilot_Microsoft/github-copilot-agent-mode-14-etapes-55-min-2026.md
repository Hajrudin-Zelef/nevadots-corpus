---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-agent-mode-14-etapes-55-min-2026
title: "GitHub Copilot Agent Mode : 14 Étapes, 55 Min [2026]"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["github", "copilot", "mode", "lagent", "code", "agent", "etapes", "projet"]
source: https://tech-insider.org/fr/github-copilot-agent-mode-2026/
source_anchor: ""
source_lines: [1, 422]
section: "Copilot_Microsoft"
---
GitHub Copilot a changé de visage en 2026. À l’assistant qui complétait vos lignes de code s’est ajouté un agent capable de lire un projet entier, de planifier une série de modifications, de les appliquer dans plusieurs fichiers à la fois, puis de lancer lui-même les commandes nécessaires pour vérifier que tout fonctionne. Ce mode s’appelle l’Agent Mode, parfois désigné sous le nom de Copilot App pour son interface dédiée à l’agent : selon un état des lieux publié par News Creeta en juin 2026, cette interface agentique est désormais incluse sur l’ensemble des paliers payants, de Pro (10 $/mois) à Enterprise (39 $/utilisateur/mois), en passant par Pro+, Max et Business. Il tourne directement dans Visual Studio Code et change la façon d’aborder une tâche de développement ordinaire.

Ce tutoriel montre comment l’installer, le configurer et l’utiliser correctement, avec des exemples testés sur un vrai petit projet du début à la fin. Vous verrez aussi les erreurs les plus fréquentes, les solutions aux blocages courants, et comment garder le contrôle sur les coûts liés aux crédits d’utilisation, un sujet devenu central depuis le passage à la facturation à l’usage en juin 2026. Comptez environ 55 minutes pour suivre les 14 étapes, en partant d’un poste vierge.

## Qu’est-ce que GitHub Copilot Agent Mode ?

L’Agent Mode est le troisième niveau d’interaction proposé par GitHub Copilot dans la vue Chat de VS Code, après Ask (poser une question) et Edit (modifier un fichier ciblé). Plutôt que de répondre à une question ou de retoucher un seul fichier, l’agent prend en charge une tâche complète formulée en langage naturel. Il explore le code du projet, construit un plan d’action, écrit ou modifie plusieurs fichiers, exécute des commandes dans le terminal intégré, observe les résultats, puis corrige ce qui ne fonctionne pas. Depuis l’indexation par recherche sémantique introduite en mars 2026, cette phase d’exploration du code est devenue 50 % plus rapide sur les projets volumineux, une amélioration détaillée dans le Copilot Digest de GitHub publié en avril 2026. Le tout sans que vous ayez à copier-coller la moindre ligne. GitHub détaille ce fonctionnement dans son explicatif officiel sur l’Agent Mode, publié sur le blog de l’entreprise.

Concrètement, la différence avec l’ancien Copilot tient à l’autonomie du raisonnement. Un prompt du type « ajoute un endpoint de recherche paginée et les tests associés » suffit à déclencher une séquence complète : lecture du schéma de données existant, écriture du code, génération des tests, exécution de la suite de tests, puis correction automatique si un test échoue. Vous gardez la main à chaque étape grâce à un système de validation des modifications et des commandes, détaillé plus loin dans ce tutoriel.

GitHub met en avant des chiffres significatifs sur sa propre page produit : les développeurs qui utilisent Copilot se disent jusqu’à 55 % plus productifs pour écrire du code, sans perte de qualité annoncée, et jusqu’à 75 % plus satisfaits de leur travail que ceux qui ne l’utilisent pas. L’entreprise cite aussi le cas du groupe brésilien **Grupo Boticário**, qui rapporte une hausse de productivité développeur de 94 % après l’adoption de l’outil. Sur le plan technique, l’agent propulsé par Claude affichait déjà un score de 56 % sur le benchmark SWE-bench Verified dès avril 2025, une performance rappelée par Dev.to dans son bilan d’avril 2026 et souvent citée comme référence pour juger de la fiabilité de l’Agent Mode sur des tâches réelles de correction de bugs. Ces chiffres proviennent en grande partie de GitHub lui-même et méritent d’être lus comme des données commerciales plutôt que comme une étude indépendante, mais ils donnent une idée de l’ampleur du pari que l’entreprise a fait sur l’Agent Mode.

Pour les équipes basées en France et en Europe, l’intérêt dépasse le simple gain de vitesse. Les offres Business et Enterprise ajoutent des options pensées pour les organisations soumises au RGPD (authentification unique, journaux d’audit, politiques de rétention), abordées plus loin dans la section consacrée à la sécurité. Si vous cherchez plutôt un panorama des autres assistants IA pour développeurs, notre rubrique logiciels regroupe l’ensemble de nos tutoriels sur les outils de développement.

## Prérequis avant de commencer

Avant de lancer la première étape, vérifiez que votre environnement correspond à ce tableau. Rien d’exotique : l’essentiel tient dans une version récente de VS Code et un abonnement GitHub Copilot actif, même sur le palier gratuit pour les premiers tests.

| Élément | Version ou condition minimale | Remarque | 
|---|---|---|
| Système d’exploitation | Windows 10/11, macOS ou Linux | Aucune restriction propre à l’Agent Mode | 
| Visual Studio Code | Version récente (1.99 ou ultérieure) | Vérifiez avec `code --version` | 
| Compte GitHub | Actif, avec abonnement Copilot | Free suffit pour découvrir, Pro recommandé pour un usage réel | 
| Extensions VS Code | GitHub Copilot + GitHub Copilot Chat | Installables depuis le Marketplace intégré | 
| Git | Dernière version stable | Indispensable pour suivre les modifications de l’agent | 
| Python | 3.11 ou plus récent | Utilisé pour le projet complet de ce tutoriel | 
| Node.js | 20 LTS ou plus récent (optionnel) | Utile si vous adaptez le projet en JavaScript | 
| Connexion réseau stable | Requise | L’agent interroge le cloud à chaque étape de raisonnement | 

Un dernier conseil avant de vous lancer : travaillez toujours dans un dépôt Git initialisé, même pour un projet de test. L’Agent Mode modifie parfois plusieurs fichiers en une seule séquence, et pouvoir revenir en arrière avec `git diff` ou `git checkout` change tout en cas de résultat inattendu.

## Étape 1 – Choisir le bon plan GitHub Copilot

Dès le 27 avril 2026, BigHat Group rapportait que GitHub s’apprêtait à généraliser un système de crédits IA mensuels à l’ensemble des paliers payants à compter du 1er juin 2026, tout en maintenant les prix d’abonnement en l’état : 10 $ pour Pro, 19 $ pour Business et 39 $ pour Enterprise. Ce système de crédits remplace le mécanisme de requêtes premium mis en place dès mai 2025, quand GitHub avait fixé un quota de 300 requêtes premium mensuelles pour les abonnés Pro et facturait 0,04 $ chaque requête supplémentaire au-delà de ce plafond. Plus vous consommez de requêtes vers des modèles premium, plus vous puisez dans ce crédit. Comprendre cette grille avant de commencer évite les mauvaises surprises en fin de mois, surtout si vous comptez faire tourner l’Agent Mode plusieurs heures par jour.

| Palier | Prix | Complétions de code | Crédits mensuels inclus | Accès à l’Agent Mode | 
|---|---|---|---|---|
| Free | 0 $/mois | 2 000/mois, 50 requêtes chat | Modèles de base uniquement | Limité | 
| Pro | 10 $/mois | Illimitées | 15 $ de crédits | Oui | 
| Pro+ | 39 $/mois | Illimitées | 70 $ de crédits, modèles premium (dont Opus) | Oui, étendu | 
| Max | 100 $/mois | Illimitées | 200 $ de crédits, accès prioritaire aux nouveautés | Oui, complet | 
| Business | 19 $/utilisateur/mois | Illimitées | Géré par l’organisation | Oui, avec politiques admin | 
| Enterprise | Sur devis | Illimitées | Personnalisé | Oui, avec options de conformité | 

Pour suivre ce tutoriel, le palier **Pro à 10 $/mois** suffit largement : GitHub avait fixé ce tarif dès avril 2025, aux côtés d’un palier Pro+ à 39 $/mois offrant 1 500 requêtes premium mensuelles, avant de confirmer en mars 2026 la disponibilité générale de l’Agent Mode sur VS Code et JetBrains avec 300 requêtes premium mensuelles pour Pro et des complétions illimitées, un quota depuis converti en crédits dans le nouveau système de facturation, mais qui donne une idée du volume disponible pour tester le projet complet présenté plus loin. Passez à Pro+ (39 $/mois, 1 500 requêtes premium selon la même grille de mars 2026, avec accès aux modèles de pointe) si vous voulez accéder aux modèles premium comme Opus pour des tâches de raisonnement plus complexes, ou au palier Max (100 $/mois, 200 $ de crédits IA inclus selon un état des lieux de Developers Digest publié en juin 2026) si votre volume de travail agentique dépasse une ou deux heures par jour. Les équipes qui gèrent plusieurs développeurs ont tout intérêt à regarder du côté de Business, facturé 19 $/utilisateur/mois avec 300 requêtes premium par utilisateur, un plafond déjà en place entre le 12 et le 19 mai 2025 lorsque GitHub avait lié ce même quota à l’Agent Mode, aux côtés d’un plafond de 1 000 requêtes premium mensuelles pour Enterprise, facturé 39 $/utilisateur/mois pour un accès complet. Ces deux paliers centralisent la facturation et ajoutent des contrôles d’administration au niveau de l’organisation. La grille tarifaire officielle reste la référence à consulter avant de vous engager, car ces paliers évoluent au rythme des annonces produit de GitHub.

Un point à anticiper si vous changez de palier en cours de mois : sur les paliers individuels, les crédits non consommés ne se reportent pas automatiquement d’un mois sur l’autre. Si une semaine chargée en refactoring ou en migration de dépendances s’annonce, mieux vaut passer au palier supérieur avant de démarrer plutôt que de vous retrouver bloqué en pleine session agentique, au milieu d’une série de modifications non committées.

## Étape 2 – Installer VS Code et les extensions Copilot

Téléchargez Visual Studio Code depuis le site officiel si ce n’est pas déjà fait, puis ouvrez le panneau Extensions avec `Ctrl+Shift+X` (ou `Cmd+Shift+X` sur Mac). Recherchez « GitHub Copilot » et installez l’extension proposée par GitHub : elle embarque automatiquement GitHub Copilot Chat, qui contient l’interface de l’Agent Mode. Redémarrez VS Code une fois l’installation terminée.

Vérifiez ensuite que votre version de l’éditeur est suffisamment récente. Ouvrez un terminal et lancez :

`code --version`
Vous devez obtenir une sortie proche de celle-ci, avec un numéro de version et un identifiant de build :

```
1.99.3
f97cb4a8d0d4c76f0f8f9e5c1a2b3d4e5f6a7b8c
x64
```
Si le numéro affiché est inférieur à 1.99, mettez à jour via `Aide > Rechercher les mises à jour` avant de continuer. Une version trop ancienne est la cause la plus fréquente d’un Agent Mode invisible dans le sélecteur de mode, un problème détaillé dans la section dépannage.

## Étape 3 – Connecter votre compte GitHub à VS Code

Ouvrez la palette de commandes avec `Ctrl+Shift+P` (`Cmd+Shift+P` sur Mac), tapez `GitHub Copilot: Sign In` et validez. VS Code ouvre votre navigateur par défaut pour finaliser l’authentification OAuth avec votre compte GitHub. Une fois l’autorisation confirmée dans le navigateur, revenez dans VS Code : une icône Copilot apparaît dans la barre d’état, en bas à droite de la fenêtre.

Cliquez sur cette icône pour vérifier que votre abonnement est bien reconnu. Si vous venez de souscrire un palier payant, patientez une ou deux minutes : la synchronisation entre le compte GitHub et l’extension n’est pas toujours instantanée. Un compte reconnu mais sans abonnement actif affiche l’Agent Mode grisé dans le sélecteur de mode de l’étape suivante.

Si votre compte dépend d’une organisation sous licence Business ou Enterprise, un administrateur doit au préalable vous avoir attribué un siège Copilot : la connexion individuelle ne suffit pas à elle seule à activer l’accès. Dans ce cas, l’icône Copilot de la barre d’état affiche un message d’attente plutôt qu’une erreur, ce qui prête parfois à confusion la première fois qu’on le rencontre.

## Étape 4 – Activer l’Agent Mode et lancer votre premier prompt

Ouvrez la vue Chat avec `Ctrl+Shift+I` (`Cmd+Shift+I` sur Mac), ou en cliquant sur l’icône Copilot dans la barre latérale. En haut du panneau de discussion, un menu déroulant liste les modes disponibles : Ask, Edit, Agent, Plan, Cloud et Copilot CLI. Sélectionnez **Agent**. Le 11 mars 2026, GitHub a fait passer en disponibilité générale les agents personnalisés, les sous-agents et le mode Plan pour JetBrains, tout en annonçant le retrait progressif de l’entrée « Edit » historique du sélecteur au profit d’une bascule directe vers l’Agent Mode ou vers un agent personnalisé, une évolution à surveiller si vous découvrez ce menu pour la première fois. Le tutoriel officiel de l’équipe VS Code détaille chacun de ces modes si vous voulez explorer les options Plan et Cloud au-delà de ce guide.

Ouvrez un dossier de projet existant, ou créez-en un vide pour tester. Dans le champ de saisie, écrivez une instruction claire et contextualisée plutôt qu’une phrase vague. Voici un exemple de prompt correct pour un premier essai :

```
Crée un script Python qui lit un fichier CSV nommé ventes.csv
(colonnes : produit, quantite, prix_unitaire), calcule le chiffre
d'affaires total par produit, et affiche un tableau trié par
chiffre d'affaires décroissant. Ajoute une gestion d'erreur si
le fichier est absent.
```
L’agent commence par annoncer un plan en quelques phrases : créer le script, définir la fonction de lecture du CSV, agréger les montants, gérer le cas du fichier manquant. Il crée ensuite le fichier, écrit le code, puis propose d’exécuter le script dans le terminal intégré pour vérifier qu’il fonctionne. Une fenêtre de progression affiche chaque action au fur et à mesure, avec un résumé du type :

```
▸ Lecture du dossier de travail
▸ Création de ventes.py
▸ Écriture de la fonction analyser_ventes()
▸ Proposition de commande : python ventes.py
▸ En attente de votre validation pour exécuter la commande
```
C’est précisément à ce moment que le contrôle humain entre en jeu, décrit dans les deux étapes suivantes.

## Étape 5 – Comprendre le cycle plan, édition, exécution, vérification

L’Agent Mode fonctionne selon une boucle en quatre temps qui se répète jusqu’à ce que la tâche soit terminée ou que vous l’interrompiez. D’abord, l’agent **planifie** : il décompose votre demande en une suite d’actions concrètes et affiche généralement ce plan avant d’agir. Ensuite vient l’**édition** : il crée ou modifie les fichiers nécessaires, en s’appuyant sur le contexte du projet qu’il a lu au préalable (structure des dossiers, dépendances, conventions de nommage existantes).

Troisième temps, l’**exécution** : l’agent propose des commandes terminal pour tester ce qu’il vient d’écrire, qu’il s’agisse de lancer un script, d’installer une dépendance ou de faire tourner une suite de tests. Enfin, la **vérification** : il lit la sortie de la commande, détecte les erreurs éventuelles, et repart en phase d’édition pour les corriger. Ce cycle peut se répéter plusieurs fois sur une tâche complexe, ce qui explique pourquoi une session d’Agent Mode dure parfois plusieurs minutes pour une demande qui semblait simple au départ.

Cette boucle autonome est la vraie différence avec le mode Edit, qui s’arrête après une seule modification sans vérifier le résultat. Elle explique aussi pourquoi la qualité du prompt initial compte autant : plus le contexte fourni est précis, moins l’agent a besoin d’itérations pour converger vers un résultat correct, et moins vous consommez de crédits au passage.

## Étape 6 – Approuver les modifications et les commandes terminal

Par défaut, l’Agent Mode ne vous laisse jamais complètement les mains liées. Chaque modification de fichier s’affiche dans un éditeur de différences multi-fichiers avant d’être définitivement appliquée. Deux boutons apparaissent : **Keep** pour accepter les changements, et **Undo** pour les rejeter. Vous pouvez examiner chaque fichier modifié individuellement avant de valider l’ensemble.

Le même principe s’applique aux commandes terminal. L’agent propose une commande, l’affiche en clair, et attend votre confirmation avant de l’exécuter, sauf si vous avez explicitement autorisé l’exécution automatique pour certains types de commandes dans les paramètres. Prenez l’habitude de lire ce qui s’affiche avant de cliquer, en particulier pour tout ce qui touche à la suppression de fichiers, aux migrations de base de données ou aux commandes Git qui réécrivent l’historique. Ce réflexe simple évite la majorité des incidents liés à l’usage agentique.

Pour les tâches répétitives et sans risque, comme le formatage de code ou l’exécution de tests en lecture seule, il est possible d’élargir la liste des commandes approuvées automatiquement dans les réglages de l’extension. Faites-le progressivement, commande par commande, plutôt que d’activer une confiance totale dès le premier jour.

Chaque session conserve par ailleurs un historique consultable des actions menées : fichiers touchés, commandes lancées, réponses obtenues. Sur un projet d’équipe, ce journal aide à comprendre a posteriori pourquoi tel fichier a changé, un peu comme un message de commit détaillé mais généré automatiquement. Sur les paliers Business et Enterprise, une partie de cet historique remonte aussi dans les journaux d’audit accessibles aux administrateurs, ce qui facilite une revue de sécurité si un incident survient.

## Étape 7 – Personnaliser l’agent avec copilot-instructions.md et AGENTS.md

Un agent qui ignore les conventions de votre équipe perd vite en utilité. GitHub Copilot lit automatiquement un fichier d’instructions placé à la racine du dépôt, dans `.github/copilot-instructions.md`, à chaque nouvelle session. Vous pouvez y décrire l’architecture du projet, les bibliothèques à privilégier, le style de code attendu, ou les zones du code à ne jamais toucher sans validation explicite. Voici un exemple pour un projet Python orienté API :

```
# .github/copilot-instructions.md
## Contexte du projet
API interne construite avec FastAPI et SQLAlchemy.
Base de données PostgreSQL, migrations gérées par Alembic.
## Conventions à respecter
- Toutes les fonctions asynchrones utilisent async/await.
- Les réponses d'erreur suivent le format {"detail": "message"}.
- Chaque nouvelle route doit être accompagnée d'un test dans tests/.
- Ne jamais modifier le dossier migrations/ sans le signaler explicitement.
## À éviter
- Pas de dépendance externe sans la mentionner dans le plan proposé.
- Pas de commit ni de push automatique.
```
Depuis 2026, GitHub Copilot prend également en charge `AGENTS.md`, une convention de fichier partagée par plusieurs éditeurs d’outils IA pour développeurs, qui permet de décrire l’agent une seule fois pour plusieurs assistants compatibles. La structure ressemble à ceci :

```
# AGENTS.md
## Build
pip install -r requirements.txt
## Tests
pytest -v
## Style
Suivre PEP 8. Formatage automatique avec black avant chaque commit.
## Notes pour l'agent
Le dossier legacy/ contient du code hérité non testé.
Éviter d'y toucher sauf demande explicite.
```
Si les deux fichiers coexistent dans un même dépôt, considérez `copilot-instructions.md` comme la source prioritaire pour Copilot spécifiquement, et `AGENTS.md` comme un socle commun utile si votre équipe teste aussi d’autres assistants agentiques. Pour des règles plus ciblées, GitHub Copilot accepte aussi des fichiers d’instructions par dossier avec un motif `applyTo`, utile sur un monorepo où le frontend et le backend suivent des règles différentes.

## Étape 8 – Connecter un serveur MCP pour étendre l’agent

Le Model Context Protocol (MCP) est un standard ouvert qui permet à un agent IA de se connecter à des outils externes : accès à une base de données, appel à une API interne, lecture d’un système de fichiers spécifique, interaction avec un gestionnaire de tickets. Le site officiel du protocole recense les serveurs disponibles et la spécification technique complète. GitHub Copilot Agent Mode supporte nativement les serveurs MCP depuis 2025, et GitHub a confirmé en juin 2026 le déploiement de l’Agent Mode avec support MCP à l’ensemble des utilisateurs de VS Code, ce qui démultiplie ce que l’agent peut accomplir sans quitter l’éditeur.

Pour en ajouter un, ouvrez les paramètres de VS Code (`Ctrl+,`), recherchez « copilot mcp », ou créez directement un fichier `.vscode/mcp.json` à la racine du projet. Voici un exemple de configuration pour le serveur MCP officiel de GitHub, qui donne à l’agent un accès structuré à vos dépôts, issues et pull requests :

```
{
  "servers": {
    "github": {
      "type": "stdio",
      "command": "docker",
      "args": [
        "run", "-i", "--rm",
        "ghcr.io/github/github-mcp-server"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${input:github_token}"
      }
    }
  }
}
```
Une fois le fichier enregistré, VS Code demande de confirmer le démarrage du serveur puis affiche un bouton « outils » dans la vue Chat, où vous pouvez activer ou désactiver chaque capacité exposée par le serveur MCP. Redémarrez VS Code après une modification de la configuration pour que les changements soient pris en compte. La syntaxe exacte évolue régulièrement : vérifiez la documentation officielle du serveur MCP que vous ajoutez avant de copier une configuration trouvée en ligne.

## Étape 9 – Choisir le bon modèle pour chaque tâche

Le sélecteur de modèle, situé juste à côté du sélecteur de mode dans la vue Chat, permet de choisir quel modèle traite votre requête. GitHub a déployé GPT-5.4 comme nouveau modèle de référence pour le codage agentique en mars 2026 (accessible sur les paliers Pro, Pro+, Business et Enterprise), avant de le compléter par GPT-5.5 dès avril 2026 ; l’entreprise a par ailleurs annoncé en juillet 2026 la mise en retraite de plusieurs modèles au 1er septembre 2026, dont Claude Opus 4.5 et Gemini 3.1 Pro, ce qui invite à vérifier régulièrement la liste des modèles disponibles dans le sélecteur. Tous les modèles ne consomment pas la même quantité de crédits : un modèle léger convient très bien à une tâche simple comme renommer des variables ou écrire un test unitaire basique, tandis qu’une tâche qui demande un raisonnement plus profond, comme refactoriser une architecture ou déboguer un problème de concurrence, tire davantage parti d’un modèle premium comme ceux accessibles via les paliers Pro+ et Max.

Cette possibilité de changer de modèle à la volée est l’un des arguments que GitHub met en avant face à ses concurrents, aux côtés de son intégration native avec les dépôts GitHub. Si vous voulez comparer les modèles disponibles en détail, notamment leurs performances respectives sur des tâches de programmation, nos comparatifs Opus 4.8 vs GPT-5.5 vs Mistral Large 3 et Grok 4.5 vs Opus 4.8 vs Gemini 3.1 Pro détaillent les écarts de score sur les benchmarks de développement logiciel.

Une règle simple pour débuter : laissez le modèle par défaut pour l’essentiel de votre usage quotidien, et ne basculez sur un modèle premium que pour les tâches où vous avez déjà vu le modèle standard échouer ou tourner en boucle. Cela préserve vos crédits mensuels pour les moments où ils font vraiment la différence.

Un exemple concret pour fixer les idées : demander à l’agent d’ajouter un endpoint REST qui suit un schéma déjà présent dans le projet est une tâche que le modèle par défaut traite très bien, car le contexte à imiter est déjà là. À l’inverse, lui demander de repenser la stratégie de cache d’une application entière, avec des compromis à évaluer entre cohérence des données et latence, sollicite un raisonnement plus long où un modèle premium montre un net avantage. La différence se voit autant dans la qualité du plan proposé que dans le nombre d’itérations nécessaires pour y arriver.

## Étape 10 à 13 – Construisez un projet complet : une API météo testée de bout en bout

Passons à la pratique avec un projet complet, assez simple pour se terminer en une vingtaine de minutes, mais assez complet pour mobiliser tout ce qui vient d’être expliqué : lecture de contexte, écriture multi-fichiers, exécution de commandes, génération de tests et correction automatique. L’objectif est une petite API qui renvoie la météo actuelle d’une ville, en s’appuyant sur l’API publique et gratuite Open-Meteo, qui ne nécessite aucune clé d’API.

### Étape 10 – Initialiser le projet

Créez un dossier de projet, initialisez un dépôt Git et un environnement virtuel Python :

```
mkdir api-meteo && cd api-meteo
git init
python3 -m venv venv
source venv/bin/activate
pip install fastapi uvicorn httpx pytest
```
Ouvrez ce dossier dans VS Code, faites un premier commit vide ou avec un fichier `.gitignore`, puis ouvrez la vue Chat en Agent Mode.

### Étape 11 – Générer l’API avec l’Agent Mode

Soumettez le prompt suivant à l’agent :

```
Crée une API FastAPI dans main.py avec un endpoint GET /meteo/{ville}.
Cet endpoint doit : 1) appeler l'API de géocodage Open-Meteo
(https://geocoding-api.open-meteo.com/v1/search) pour trouver les
coordonnées de la ville, 2) appeler l'API de prévisions Open-Meteo
(https://api.open-meteo.com/v1/forecast) avec current_weather=true,
3) renvoyer un JSON avec ville, pays, temperature_c, vent_kmh et
heure_mesure. Utilise httpx en mode asynchrone. Renvoie une erreur
404 explicite si la ville n'est pas trouvée.
```
L’agent lit le prompt, annonce son plan, puis génère un fichier proche de celui-ci :

```
# main.py
from fastapi import FastAPI, HTTPException
import httpx
app = FastAPI(title="API Météo")
GEOCODING_URL = "https://geocoding-api.open-meteo.com/v1/search"
FORECAST_URL = "https://api.open-meteo.com/v1/forecast"
@app.get("/meteo/{ville}")
async def get_meteo(ville: str):
    async with httpx.AsyncClient() as client:
        geo = await client.get(GEOCODING_URL, params={
            "name": ville, "count": 1, "language": "fr"
        })
        geo_data = geo.json()
        if not geo_data.get("results"):
            raise HTTPException(status_code=404, detail=f"Ville '{ville}' introuvable")
        lieu = geo_data["results"][0]
        forecast = await client.get(FORECAST_URL, params={
            "latitude": lieu["latitude"],
            "longitude": lieu["longitude"],
            "current_weather": True
        })
        meteo = forecast.json()["current_weather"]
        return {
            "ville": lieu["name"],
            "pays": lieu.get("country"),
            "temperature_c": meteo["temperature"],
            "vent_kmh": meteo["windspeed"],
            "heure_mesure": meteo["time"]
        }
```
### Étape 12 – Générer et exécuter les tests automatisés

Enchaînez avec un second prompt, dans la continuité de la même session :

```
Ajoute un fichier test_main.py avec pytest et TestClient de FastAPI.
Teste le cas d'une ville valide (Paris) et le cas d'une ville
inexistante. Exécute ensuite la suite de tests et corrige le code
si un test échoue.
```
L’agent crée le fichier de test, propose la commande `pytest -v`, l’exécute après votre validation, puis lit la sortie :

```
# test_main.py
from fastapi.testclient import TestClient
from main import app
client = TestClient(app)
def test_meteo_paris():
    response = client.get("/meteo/Paris")
    assert response.status_code == 200
    data = response.json()
    assert data["ville"] == "Paris"
    assert "temperature_c" in data
def test_meteo_ville_inconnue():
    response = client.get("/meteo/Xyzabc123")
    assert response.status_code == 404
```
### Étape 13 – Vérifier, corriger et déployer localement

Si tout se passe bien, le terminal affiche une sortie de ce type :

```
$ pytest -v
============================= test session starts ==============================
collected 2 items
test_main.py::test_meteo_paris PASSED                                    [ 50%]
test_main.py::test_meteo_ville_inconnue PASSED                          [100%]
============================== 2 passed in 1.84s ================================
```
Si un test échoue à la première tentative, typiquement parce que la clé retournée par l’API de géocodage ne correspond pas exactement à celle attendue dans le code, l’agent lit la trace d’erreur, propose une correction ciblée, puis relance les tests automatiquement. C’est le cycle décrit à l’étape 5, observable en conditions réelles. Lancez enfin le serveur local pour un dernier test manuel :

```
$ uvicorn main:app --reload
$ curl -s http://127.0.0.1:8000/meteo/Lyon | python3 -m json.tool
{
    "ville": "Lyon",
    "pays": "France",
    "temperature_c": 21.4,
    "vent_kmh": 11.2,
    "heure_mesure": "2026-06-15T14:00"
}
```
Vous disposez maintenant d’un projet complet, testé, généré en grande partie par l’agent mais entièrement relu et validé par vos soins à chaque étape, exactement comme le veut le flux de travail recommandé.

## Étape 14 – Suivre vos crédits et automatiser via Copilot CLI

Le tableau de bord de facturation, accessible depuis les paramètres de votre compte sur GitHub, affiche la consommation de crédits en temps quasi réel, ventilée par modèle utilisé. Prenez l’habitude de le consulter une fois par semaine si vous êtes sur le palier Pro, dont l’enveloppe de 15 $ de crédits se consomme plus vite qu’on ne l’imagine sur des sessions agentiques intensives.

Pour les tâches répétitives ou scriptables, GitHub propose aussi une extension en ligne de commande, indépendante de VS Code : Copilot CLI, entrée en préversion publique dès septembre 2025 comme agent de codage IA nativement conçu pour le terminal, disponible sur l’ensemble des paliers. Installez-la via GitHub CLI :

```
gh extension install github/gh-copilot
# Exemple d'usage : demander une commande shell
gh copilot suggest "trouver tous les fichiers Python modifiés cette semaine"
# Exemple d'usage : expliquer une commande existante
gh copilot explain "find . -mtime -7 -name '*.py'"
```
Cette voie en ligne de commande complète bien l’Agent Mode intégré à VS Code : elle convient aux scripts d’intégration continue, aux environnements sans interface graphique, ou tout simplement aux développeurs qui passent le plus clair de leur temps dans un terminal plutôt que dans un éditeur. Copilot CLI est passé en disponibilité générale pour tous les abonnés Copilot le 25 février 2026 ; chaque invite y consomme désormais une requête premium, facturée 0,04 $ au-delà du quota inclus dans votre palier, exactement le même mécanisme de facturation que pour l’Agent Mode intégré à VS Code.

## Cinq erreurs fréquentes avec l’Agent Mode (et comment les éviter)

La plupart des déconvenues rapportées par les développeurs qui débutent avec l’Agent Mode viennent des mêmes habitudes mal ajustées. En voici six à corriger dès le départ.

- **Un prompt trop vague.** Écrire « corrige le bug » sans préciser lequel, ni le comportement attendu, pousse l’agent à deviner. Décrivez le symptôme observé, le fichier concerné si vous le connaissez, et le résultat souhaité.
- **Valider les changements sans les relire.** Cliquer sur Keep par réflexe, sans ouvrir le diff, revient à donner un accès en écriture non supervisé à votre code. Prenez systématiquement dix secondes pour parcourir les fichiers modifiés.
- **Laisser filer des commandes destructrices.** Une commande comme une suppression de fichiers ou une migration de base de données mérite une lecture attentive avant validation, jamais une approbation automatique.
- **Oublier de committer avant une longue session.** Sans point de restauration Git propre, revenir en arrière après une séquence de modifications ratée devient pénible. Committez avant de lancer une tâche agentique ambitieuse.
- **Ignorer le compteur de crédits.** Une session qui boucle plusieurs fois sur le même problème consomme des crédits sans forcément progresser. Surveillez le tableau de bord, surtout sur le palier Pro.
- **Utiliser un modèle premium pour tout, même le trivial.** Renommer une variable ne nécessite pas le modèle le plus coûteux disponible. Réservez les modèles premium aux tâches qui le justifient réellement.

## Dépannage : 9 problèmes courants et leurs solutions

La majorité de ces blocages se résolvent en moins de cinq minutes une fois la cause identifiée. Voici les blocages les plus signalés par les développeurs qui configurent l’Agent Mode pour la première fois, avec une piste de résolution pour chacun.

| Problème | Cause probable | Solution | 
|---|---|---|
| Agent absent du sélecteur de mode | VS Code ou l’extension sont trop anciens | Mettez à jour VS Code (1.99+) et réinstallez l’extension Copilot | 
| Compte non reconnu après connexion | Synchronisation lente entre GitHub et l’extension | Déconnectez-vous puis reconnectez-vous, patientez quelques minutes | 
| L’agent boucle sans converger | Prompt trop large ou tâche mal délimitée | Reformulez en une tâche plus petite, redémarrez une session propre | 
| Commandes terminal qui échouent silencieusement | Environnement virtuel non activé ou permissions insuffisantes | Activez l’environnement attendu avant de lancer l’agent | 
| Instructions personnalisées ignorées | Chemin ou nom de fichier incorrect | Vérifiez l’emplacement exact : .github/copilot-instructions.md à la racine | 
| Serveur MCP qui ne se connecte pas | Commande de démarrage ou variable d’environnement mal configurée | Consultez les journaux MCP dans la vue de sortie de VS Code | 
| Crédits épuisés en cours de tâche | Session agentique longue sur un modèle premium | Basculez sur un modèle standard ou patientez le renouvellement mensuel | 
| Modifications appliquées sur la mauvaise branche | Branche Git active non vérifiée avant de lancer l’agent | Contrôlez systématiquement votre branche avec git status avant de démarrer | 
| Réponses lentes ou latence élevée | Charge réseau ou modèle premium très sollicité | Vérifiez votre connexion, testez avec un modèle standard pour comparer | 

## Astuces avancées pour aller plus loin

Une fois les bases maîtrisées, quelques réglages supplémentaires changent nettement l’expérience au quotidien.

- **Passez par le mode Plan avant l’Agent Mode** sur les tâches ambitieuses. Plan génère une feuille de route détaillée que vous pouvez amender avant de laisser l’agent exécuter quoi que ce soit, ce qui réduit les allers-retours inutiles.
- **Utilisez des instructions ciblées par dossier** avec la syntaxe applyTo plutôt qu’un seul fichier d’instructions générique, surtout sur un monorepo qui mélange plusieurs technologies.
- **Essayez le mode Cloud** pour les tâches longues, comme une migration de dépendances sur un gros projet, qui peuvent s’exécuter en arrière-plan sans monopoliser votre machine locale.
- **Combinez Copilot CLI avec vos pipelines existants** pour automatiser des vérifications ou des suggestions de commandes directement dans un script shell.
- **Créez des modes de discussion personnalisés** pour les tâches récurrentes de votre équipe, comme la revue de sécurité d’une pull request ou la génération de documentation, afin de ne pas réécrire le même prompt à chaque fois.

## GitHub Copilot Agent Mode face à Cursor et Windsurf

Trois outils dominent aujourd’hui la conversation sur la programmation assistée par agent : GitHub Copilot, Cursor et Windsurf. Leur approche technique diverge sur un point central. Copilot reste une extension qui s’installe sur n’importe quelle version de VS Code, tandis que Cursor et Windsurf sont des forks complets de l’éditeur, distribués comme des applications à part entière. Le tableau ci-dessous résume les différences qui comptent le plus au moment de choisir.

| Critère | GitHub Copilot | Cursor AI | Windsurf | 
|---|---|---|---|
| Base technique | Extension pour VS Code, JetBrains, Neovim | Fork complet de VS Code | Fork de VS Code (sous Cognition AI) | 
| Entrée de gamme | Gratuit (2 000 complétions/mois) | Gratuit (Hobby) | Gratuit | 
| Palier individuel courant | 10 $/mois (Pro) | 20 $/mois (Pro) | environ 20 $/mois (Pro) | 
| Palier le plus élevé | 100 $/mois (Max) | 200 $/mois (Ultra) | 200 $/mois (Max) | 
| Palier équipe | 19 $/utilisateur/mois (Business) | 40 $/utilisateur/mois | 40 $/utilisateur/mois | 
| Support MCP | Oui, natif | Oui, natif | Partiel selon les sources | 
| Société éditrice | Microsoft / GitHub | Anysphere | Cognition AI | 

Le principal avantage de Copilot tient à sa diffusion : il s’installe sur l’éditeur que vous utilisez déjà, sans migration, et couvre aussi JetBrains et Neovim en plus de VS Code. Cursor et Windsurf misent au contraire sur une intégration plus poussée entre l’éditeur et l’agent, au prix d’un changement d’outil complet. Si vous voulez approfondir la configuration de Cursor spécifiquement, notre tutoriel Cursor AI en 13 étapes couvre l’installation et la prise en main de cet éditeur.

Dans les faits, le choix se joue souvent sur des critères plus organisationnels que techniques. Une équipe déjà installée sur VS Code, JetBrains ou Neovim a peu d’intérêt à migrer vers un éditeur entièrement nouveau juste pour l’Agent Mode, alors que Copilot s’ajoute en quelques minutes à un poste existant. À l’inverse, une équipe qui démarre un projet de zéro et cherche l’intégration agentique la plus poussée dès l’installation peut légitimement préférer un éditeur conçu autour de l’agent dès le départ.

## Sécurité, RGPD et résidence des données en Europe

Pour une équipe qui code en France ou ailleurs en Europe, la question de la conformité se pose dès qu’un agent IA touche à du code source sensible ou à des données de production. Les paliers Business et Enterprise de GitHub Copilot ajoutent des couches de contrôle absentes des offres individuelles : authentification unique (SSO), journaux d’audit détaillés sur chaque action de l’agent, et des politiques de gestion centralisées au niveau de l’organisation. GitHub a par ailleurs annoncé en juin 2026 que les clients Enterprise pourraient activer un Autonomous Agent Mode à partir de juillet 2026, une capacité supplémentaire à évaluer avec la même rigueur de conformité avant tout déploiement sur du code sensible.

Sur le plan du RGPD, le point de vigilance principal reste le même qu’avec n’importe quel outil cloud : vérifiez précisément ce que votre contrat Enterprise garantit en matière de traitement et de conservation des données envoyées au modèle, avant de connecter l’agent à un dépôt contenant des données personnelles ou des secrets de production. Un fichier d’instructions comme celui présenté à l’étape 7 peut d’ailleurs servir à interdire explicitement à l’agent de manipuler certains dossiers sensibles, une pratique simple qui réduit une bonne partie du risque.

Si votre organisation gère déjà l’authentification unique pour d’autres outils internes, notre tutoriel Keycloak détaille la mise en place d’un SSO open source, une brique souvent utilisée en complément des contrôles d’accès proposés par les offres Copilot Business et Enterprise. Pour la partie scan de vulnérabilités du code généré, notre tutoriel Trivy montre comment vérifier automatiquement les dépendances et les images ajoutées à un projet, un réflexe utile quand une bonne partie du code provient désormais d’un agent.

Autre réglage à vérifier avant de généraliser l’outil à toute une équipe : les paramètres de rétention et d’utilisation des données pour l’entraînement des modèles, disponibles dans les réglages d’organisation sur GitHub. Sur les paliers Business et Enterprise, ces réglages permettent d’exclure le code de votre organisation de tout usage au-delà de la génération de réponses, un point que les équipes juridiques et sécurité demandent presque systématiquement avant validation.

## Foire aux questions

### Quelle est la différence entre l’Agent Mode et le mode Edit classique ?

Le mode Edit modifie un ou plusieurs fichiers selon vos instructions, puis s’arrête. L’Agent Mode va plus loin : il planifie la tâche, modifie les fichiers nécessaires, exécute des commandes pour vérifier le résultat, et corrige automatiquement ce qui ne fonctionne pas, sans attendre une nouvelle instruction à chaque étape.

### GitHub Copilot Agent Mode fonctionne-t-il en français ?

Oui. L’interface et les modèles sous-jacents comprennent le français aussi bien que l’anglais pour la formulation des prompts. Le code généré, lui, reste écrit dans la langue de programmation demandée, avec des commentaires que vous pouvez explicitement demander en français dans votre fichier d’instructions.

### Quel est le prix minimum pour utiliser l’Agent Mode ?

Le palier Pro à 10 $/mois est le premier à débloquer l’Agent Mode sans restriction sur les complétions de code. Le palier gratuit donne un accès limité, insuffisant pour un usage agentique régulier.

### L’agent peut-il supprimer ou casser mon code ?

Techniquement oui, s’il propose une commande destructrice que vous approuvez sans la lire. C’est pour cette raison que chaque modification de fichier passe par une validation Keep/Undo, et que chaque commande terminal attend votre confirmation par défaut. Le risque vient presque toujours d’une validation trop rapide, pas de l’agent lui-même.

### L’Agent Mode fonctionne-t-il avec JetBrains ou seulement avec VS Code ?

GitHub Copilot propose des extensions pour VS Code, les IDE JetBrains et Neovim. Les fonctionnalités les plus récentes, dont certaines options de l’Agent Mode, apparaissent généralement d’abord sur VS Code avant d’être portées sur les autres environnements. La documentation officielle de GitHub Copilot tient à jour la liste des fonctionnalités disponibles par IDE.

### Qu’est-ce qu’un serveur MCP, et dois-je en installer un ?

Un serveur MCP (Model Context Protocol) donne à l’agent accès à des outils externes, comme une base de données ou une API interne. Ce n’est pas obligatoire pour débuter : l’Agent Mode fonctionne très bien sans, en s’appuyant sur le système de fichiers local et le terminal. Un serveur MCP devient utile quand vous voulez connecter l’agent à des systèmes en dehors du projet ouvert dans VS Code.

### Comment savoir combien de crédits il me reste ?

Le tableau de bord de facturation de votre compte GitHub affiche la consommation de crédits en cours, détaillée par modèle. Il est accessible depuis les paramètres de facturation, en dehors de VS Code.

### L’Agent Mode remplace-t-il un développeur junior ?

Non, il change la nature du travail plutôt que de le remplacer. L’agent exécute vite des tâches bien définies, mais reste dépendant de la qualité du prompt et d’une relecture humaine à chaque étape. La compétence qui gagne en valeur n’est plus de taper du code ligne par ligne, mais de savoir décomposer un problème, écrire un prompt précis et juger si le résultat produit est correct.

### Contenus similaires

- Cursor AI : Tutoriel en 13 Étapes, 45 Min [2026] : l’installation et la prise en main de l’éditeur concurrent basé sur un fork de VS Code.
- Keycloak : SSO Open Source en 14 Étapes, 60 Min [2026] : pour sécuriser l’accès à vos outils internes en complément des offres Copilot Business et Enterprise.
- Trivy : Scanner de Failles en 13 Étapes, 45 Min [2026] : analyser automatiquement les dépendances et images d’un projet, y compris le code généré par un agent.
- Opus 4.8 vs GPT-5.5 vs Mistral Large 3 : prix x10 [2026] : comparatif des modèles disponibles dans le sélecteur de modèle de Copilot Chat.
- Grok 4.5 vs Opus 4.8 vs Gemini 3.1 Pro : 0 Accès UE [2026] : un autre comparatif de modèles utile pour choisir le bon moteur selon la tâche.
