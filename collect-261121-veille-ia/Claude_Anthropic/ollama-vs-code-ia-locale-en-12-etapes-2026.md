---
id: collect-261121-veille-ia/claude-anthropic/ollama-vs-code-ia-locale-en-12-etapes-2026
title: "Ollama + VS Code : IA Locale en 12 Étapes [2026]"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["code", "dollars", "ollama", "millions", "continue", "locale", "selon", "cursor"]
source: https://tech-insider.org/fr/ollama-vscode-assistant-ia-local-2026/
source_anchor: ""
source_lines: [1, 311]
section: "Claude_Anthropic"
---
Le 16 juin 2026, Cursor annonçait le rachat de Continue, l’un des tout premiers assistants de code IA open source largement adoptés. Trois jours plus tard, la version 2.0.0 du projet sortait, en même temps que le dépôt GitHub basculait en lecture seule. Dans la même fenêtre, SpaceX annonçait de son côté le rachat de Cursor pour 60 milliards de dollars en actions selon plusieurs médias spécialisés dont The New Stack, plaçant l’éditeur sous le giron de xAI avec une clôture attendue au troisième trimestre 2026. Deux rachats en quelques jours, pour rappeler une évidence à tout développeur : un outil cloud peut changer de main, fermer un service ou disparaître du jour au lendemain. Un assistant de code qui tourne entièrement sur votre machine ne dépend d’aucune fusion d’entreprise. C’est exactement ce que permet la combinaison d’**Ollama**, de l’extension Continue et d’un modèle de code open source directement dans VS Code. Ce tutoriel construit cet environnement de bout en bout : installation, choix du modèle, configuration, optimisation matérielle, sécurité et dépannage.

## Pourquoi installer un assistant de code IA local en 2026 ?

Le marché des assistants de code a changé de visage en quelques mois. GitHub Copilot revendique plus de 20 millions d’utilisateurs cumulés début 2026, avec un tarif qui démarre autour de 10 dollars par mois pour un compte individuel et grimpe à 19 dollars par utilisateur pour un compte Business. Cursor, de son côté, a vu sa valorisation grimper jusqu’à intéresser SpaceX pour un rachat annoncé à 60 milliards de dollars. Ollama, l’alternative locale au cœur de ce tutoriel, n’est pas en reste côté financement : partie d’un tour de pré-amorçage de 125 000 dollars en avril 2025, l’entreprise a bouclé une Série B de 65 millions de dollars menée par Theory Ventures le 9 juillet 2026, portant son financement total à 88 millions de dollars et faisant état de 8,9 millions de développeurs mensuels actifs, selon TechCrunch et BusinessWire. Ces deux outils partagent un point commun : le code que vous écrivez transite par leurs serveurs pour générer une suggestion. Pour une part croissante des développeurs européens, et français en particulier, cette dépendance pose un problème concret de conformité RGPD et de confidentialité du code source, surtout pour les équipes qui travaillent sous accord de confidentialité strict ou sur des données sensibles.

**Ollama** répond à ce besoin autrement. C’est un moteur d’exécution qui télécharge, charge et sert des modèles de langage ouverts directement sur votre poste, sans connexion réseau obligatoire une fois le modèle récupéré. Associé à l’extension Continue dans VS Code, il reproduit l’essentiel de ce que propose Copilot : complétion automatique, chat contextuel, refactorisation assistée, tout en gardant le code sur la machine locale. Le projet affichait déjà 5,0 millions d’utilisateurs actifs et plus de 1 200 modèles dans son registre en mai 2026 selon Presenc AI Research, et l’entreprise revendiquait, selon BusinessWire, 8,9 millions de développeurs mensuels et plus de 67 000 intégrations tierces en juillet 2026, un chiffre également repris dans une lettre aux investisseurs relayée par ExplainX AI qui évoquait plus de 9 millions de builders actifs. Le mot-clé “Ollama” dépasse aujourd’hui 33 000 recherches mensuelles rien qu’en France selon les données de suivi de mots-clés, un signal net de l’intérêt grandissant pour ces solutions locales, aux côtés de LM Studio (14 800 recherches mensuelles) et de la requête générique “IA locale” (480 recherches mensuelles).

Le rachat de Continue par Cursor n’empêche pas d’utiliser l’extension aujourd’hui. La version 2.0.0, taguée le 19 juin 2026, reste distribuée sous licence Apache 2.0 et continue de fonctionner exactement comme avant avec un modèle local. Ce qui change, c’est l’absence de nouvelles fonctionnalités à venir côté éditeur d’origine et la fermeture du service de synchronisation cloud de Continue, dont les données ont été supprimées après le 15 juillet 2026 pour les comptes qui n’avaient pas exporté leurs réglages. Pour un usage 100 % local comme celui décrit ici, cette bascule ne change concrètement rien au fonctionnement du duo Ollama et Continue.

L’équipe d’Ollama documentait déjà ce couplage avant même le rachat, dans un billet de blog officiel qui présentait Continue comme “un assistant de code entièrement open source à l’intérieur de votre éditeur”. Trois profils reviennent le plus souvent dans les retours d’expérience publiés en 2026 : un cabinet d’avocats ou une administration qui refuse par principe d’envoyer du code ou des documents vers un serveur situé hors de l’Union européenne, une équipe qui développe sous accord de confidentialité strict pour un client, et un développeur indépendant qui veut simplement éviter un abonnement mensuel récurrent pour une fonctionnalité qu’il utilise surtout pour de l’autocomplétion basique. Dans les trois cas, la latence réseau disparaît aussi comme effet secondaire bienvenu : une requête qui reste sur la machine locale répond en général plus vite qu’un aller-retour vers un centre de données, même performant.

## Ce que vous allez construire

Ce tutoriel n’est pas une simple liste de commandes à copier-coller. Il construit un projet complet et testable de bout en bout, avec une vérification concrète à chaque étape plutôt qu’une simple installation suivie d’un espoir que tout fonctionne. À la fin, vous disposerez d’un environnement de développement local qui tourne sans connexion internet, avec les éléments suivants en place et vérifiés :

- Un serveur Ollama actif en arrière-plan, capable de servir plusieurs modèles de code simultanément
- Un modèle de code adapté à votre matériel, téléchargé et testé en ligne de commande
- L’extension Continue installée et connectée à Ollama dans VS Code
- Une complétion automatique en temps réel pendant que vous tapez, avec des délais réglés pour votre machine
- Un panneau de chat capable de lire votre dépôt via l’indexation par embeddings (fonction @codebase)
- Une configuration optimisée pour votre carte graphique ou, à défaut, votre processeur
- Un mode agent capable de mener une tâche de codage sur plusieurs fichiers sans supervision constante

Comptez environ 90 minutes pour l’ensemble des douze étapes, en incluant le temps de téléchargement des modèles, qui dépend surtout du débit de votre connexion.

## Prérequis : matériel, logiciels et versions

Avant de commencer, vérifiez que votre machine correspond à l’un des profils suivants. Le choix du modèle de code à l’étape 3 dépend directement de cette configuration.

| Profil matériel | RAM | GPU / VRAM | Modèle conseillé | 
|---|---|---|---|
| Portable modeste, sans GPU dédié | 8-16 Go | Aucun (CPU uniquement) | qwen2.5-coder:7b quantifié | 
| Poste de développement standard | 16-32 Go | 8-12 Go VRAM (RTX 4060/4070, Apple M2/M3) | qwen2.5-coder:7b ou 14b | 
| Poste de développement avancé | 32-64 Go | 24 Go VRAM (RTX 4090/5090, Apple M3/M4 Max) | qwen3-coder:30b | 
| Station de travail ou serveur dédié | 64 Go et plus | 48 Go VRAM ou multi-GPU | Devstral Small 2 24B, Qwen3.6-27B | 

Côté logiciels, préparez les éléments suivants avant de démarrer :

- Un système d’exploitation à jour : Windows 10/11, macOS 13 ou plus récent, ou une distribution Linux récente comme Ubuntu 22.04+
- Ollama en dernière version stable (v0.33.1, publiée le 26 août 2026, avec un candidat v0.33.2 déjà en test le lendemain selon Local AI Master et PromptQuorum, plusieurs versions après la v0.32.0 du 11 juillet 2026 qui avait transformé la commande `ollama` en un véritable agent CLI interactif)
- Visual Studio Code en version récente, téléchargeable sur code.visualstudio.com
- L’extension Continue, disponible sur le VS Code Marketplace
- Entre 15 et 45 Go d’espace disque libre selon le nombre de modèles installés
- Une connexion internet uniquement nécessaire pour le téléchargement initial des modèles et de l’extension

## Étape 1 : Installer Ollama sur votre système

Sur macOS et Linux, l’installation tient en une seule ligne de commande dans un terminal. Sur Windows, un installeur graphique fait le travail.

```
# macOS et Linux
curl -fsSL https://ollama.com/install.sh | sh
# Windows : téléchargez OllamaSetup.exe depuis ollama.com
# ou, via winget en PowerShell :
winget install Ollama.Ollama
```
Une fois l’installation terminée, vérifiez la version installée :

```
ollama --version
# Sortie attendue, par exemple :
# ollama version is 0.30.10
```
Sur macOS et Windows, l’installeur ajoute aussi une icône dans la barre de menu ou la zone de notification qui garde Ollama actif en arrière-plan. Sur Linux, l’installation configure un service systemd qui démarre automatiquement avec la session. Le projet est entièrement open source et son code source est consultable sur le dépôt GitHub officiel, ce qui permet de vérifier exactement ce que fait le script d’installation avant de l’exécuter, une précaution raisonnable dès qu’une commande est exécutée avec les droits d’un compte utilisateur standard.

## Étape 2 : Démarrer le service et vérifier l’API

Si le service ne démarre pas automatiquement, lancez-le manuellement dans un terminal dédié :

`ollama serve`
Laissez ce terminal ouvert, ou fermez-le si vous avez déjà l’icône de la barre de menu active : les deux méthodes lancent le même serveur, qui écoute par défaut sur le port 11434. Vérifiez que l’API répond correctement depuis un second terminal :

```
curl http://localhost:11434/api/version
# Sortie attendue :
# {"version":"0.30.10"}
```
Si cette commande renvoie une erreur de connexion refusée, le service n’est pas encore démarré ou un pare-feu local bloque le port. La section dépannage plus bas couvre ce cas précis.

## Étape 3 : Choisir le bon modèle de code pour votre matériel

C’est l’étape la plus déterminante pour la qualité de l’expérience finale. Un modèle trop volumineux pour votre VRAM tournera au ralenti ou plantera au chargement. Un modèle trop petit donnera des suggestions moins pertinentes sur du code complexe. Voici les options les plus citées dans les classements et bancs d’essai indépendants publiés en 2026 pour un usage sur Ollama.

| Modèle | Taille | RAM / VRAM conseillée | Repère observé | 
|---|---|---|---|
| qwen2.5-coder:7b | 7B, dense | 8 Go (CPU possible) | Autocomplétion légère, machines modestes, plus de 20 langages supportés | 
| qwen3-coder:30b | 30B MoE, 3,3B actifs | ~19 Go de téléchargement (Q4_K_M), 24 Go VRAM conseillés | Fenêtre de contexte de 256K tokens, bon compromis chat + agent | 
| Devstral Small 2 24B (Mistral) | 24B, dense | 14 Go VRAM | 46,8 % SWE-bench Verified rapporté, conçu pour les agents type Cline, Aider, OpenHands | 
| Qwen3.6-27B | 27B, dense | 24 Go VRAM | 77,2 % SWE-bench Verified rapporté par certains bancs d’essai indépendants pour cette catégorie de taille | 

Notez la différence entre un modèle “dense”, qui active tous ses paramètres à chaque calcul, et un modèle “MoE” (mixture of experts) comme qwen3-coder:30b, qui n’active qu’une fraction de ses paramètres par requête. C’est ce qui permet à un modèle de 30 milliards de paramètres de tourner à une vitesse proche d’un modèle bien plus petit. Ollama gère nativement les deux familles depuis la mise à jour 0.30.10, qui a aussi ajouté le support de Gemma 4 et de Llama 4 aux côtés des modèles Qwen. Le registre s’est encore étoffé en juillet 2026 avec l’arrivée de laguna-xs-2.1, un modèle MoE 33B (3B de paramètres actifs) qui pousse le contexte à 256K tokens et revendique 70,9 % sur le benchmark SWE-bench Verified selon Prompt Quorum, une option à surveiller pour les profils matériel les plus généreux du tableau ci-dessus. Le 9 juillet 2026, Ollama a par ailleurs étoffé son offre cloud avec l’ajout de modèles ouverts plus lourds comme GLM et DeepSeek, selon MentorCruise, une piste pour les développeurs qui veulent dépasser les limites de VRAM locale sans changer d’outil.

Le nom de chaque modèle sur Ollama porte en général un suffixe de quantification, comme `q4_K_M` ou `q8_0`, qui indique sur combien de bits chaque paramètre est représenté en mémoire. Une quantification plus agressive (4 bits) réduit fortement l’espace disque et la VRAM nécessaires, au prix d’une perte de précision généralement faible sur du code classique mais plus sensible sur des raisonnements longs. Une quantification plus légère (8 bits) se rapproche davantage du modèle d’origine mais double presque la mémoire requise. Sans préférence particulière, la variante `q4_K_M` proposée par défaut lors d’un `ollama pull` reste le compromis le plus raisonnable pour démarrer.

## Étape 4 : Télécharger et tester un modèle en ligne de commande

Téléchargez le modèle choisi avec la commande `ollama pull`. Pour ce tutoriel, les exemples utilisent qwen2.5-coder:7b comme base sûre pour la plupart des machines, avec qwen3-coder:30b en option pour le matériel plus généreux.

```
ollama pull qwen2.5-coder:7b
# Sur une machine avec 24 Go de VRAM ou plus :
ollama pull qwen3-coder:30b
```
Une fois le téléchargement terminé, testez directement le modèle dans le terminal, sans passer par VS Code, pour confirmer qu’il répond correctement :

`ollama run qwen2.5-coder:7b "Écris une fonction Python qui calcule la suite de Fibonacci de façon itérative, avec un commentaire de type docstring."`
Exemple de sortie obtenue :

```
def fibonacci(n):
    """Retourne le n-ième terme de la suite de Fibonacci (itératif)."""
    a, b = 0, 1
    for _ in range(n):
        a, b = b, a + b
    return a
```
Si la réponse arrive en plusieurs dizaines de secondes sur un modèle censé être léger, c’est le premier signal que le modèle tourne sur le processeur plutôt que sur la carte graphique. La section optimisation plus loin détaille comment vérifier et corriger ce point.

## Étape 5 : Installer VS Code et l’extension Continue

Si VS Code n’est pas encore installé, téléchargez-le depuis code.visualstudio.com. Installez ensuite l’extension Continue, soit depuis l’onglet Extensions de VS Code en cherchant “Continue”, soit en ligne de commande :

`code --install-extension continue.continue`
Un point de contexte important : Continue a été racheté par Cursor le 16 juin 2026, et sa version 2.0.0, sortie le 19 juin, est la dernière officielle. Le dépôt GitHub est désormais marqué comme non maintenu et en lecture seule. Cela ne bloque en rien l’installation ni l’usage décrit ici : le code reste sous licence Apache 2.0, l’extension continue de fonctionner normalement dans VS Code, et la connexion à un modèle local via Ollama ne dépend d’aucun service cloud de l’éditeur d’origine. Si vous utilisiez la synchronisation cloud de vos configurations Continue, sachez que ces données ont été supprimées après le 15 juillet 2026 : seule la configuration locale, celle que ce tutoriel met en place, reste pertinente désormais.

## Étape 6 : Connecter Continue à Ollama

Ouvrez le fichier de configuration de Continue. Depuis la version 1.x, il s’agit d’un fichier YAML situé dans `~/.continue/config.yaml` (accessible aussi depuis l’icône d’engrenage du panneau Continue dans VS Code). Remplacez son contenu par une configuration qui pointe vers votre serveur Ollama local :

```
name: Assistant Local
version: 1.0.0
schema: v1
models:
  - name: Chat Qwen Coder
    provider: ollama
    model: qwen2.5-coder:7b
    apiBase: http://localhost:11434
    roles:
      - chat
      - edit
  - name: Autocomplétion Qwen Coder
    provider: ollama
    model: qwen2.5-coder:7b
    apiBase: http://localhost:11434
    roles:
      - autocomplete
  - name: Embeddings Local
    provider: ollama
    model: nomic-embed-text
    apiBase: http://localhost:11434
    roles:
      - embed
context:
  - provider: codebase
  - provider: file
  - provider: terminal
  - provider: diff
```
Enregistrez le fichier. Continue recharge automatiquement la configuration et affiche les modèles disponibles dans le sélecteur du panneau de chat, accessible par le raccourci Ctrl+L (Cmd+L sur macOS). La documentation officielle de Continue reste accessible malgré le passage du dépôt en lecture seule et détaille l’ensemble des champs disponibles dans ce schéma, notamment pour ajouter des modèles supplémentaires ou changer un rôle sans tout réécrire. Le champ `roles` est celui qui détermine où chaque modèle intervient : un même modèle peut très bien cumuler plusieurs rôles, ou au contraire être dédié à un seul, ce qui devient utile dès que vous ajoutez un deuxième modèle plus gros réservé au chat.

## Étape 7 : Configurer l’autocomplétion en temps réel

Le rôle `autocomplete` défini à l’étape précédente active déjà la complétion automatique. Pour l’ajuster à votre matériel, ajoutez une section dédiée dans le même fichier `config.yaml` :

```
tabAutocompleteOptions:
  debounceDelay: 300
  maxPromptTokens: 1024
  disableInFiles:
    - "*.md"
    - "*.txt"
```
Le paramètre `debounceDelay` fixe le délai, en millisecondes, entre la fin de la frappe et l’envoi de la requête au modèle. Sur une machine sans GPU dédié, montez cette valeur à 500 ou 800 pour éviter de saturer le processeur à chaque caractère tapé. Le paramètre `maxPromptTokens` limite la taille du contexte envoyé au modèle pour l’autocomplétion : une valeur plus basse accélère la réponse au prix d’un contexte plus restreint sur le fichier en cours.

## Étape 8 : Configurer le chat, le contexte et les embeddings

Le modèle d’embeddings `nomic-embed-text` ajouté à l’étape 6 permet à Continue d’indexer votre dépôt de code pour répondre à des questions qui portent sur l’ensemble du projet, pas seulement sur le fichier ouvert. Téléchargez-le s’il n’est pas déjà présent :

`ollama pull nomic-embed-text`
Une fois ce modèle en place, ouvrez le panneau de chat de Continue et tapez `@codebase` suivi de votre question pour interroger l’ensemble du projet plutôt qu’un seul fichier. Les fournisseurs de contexte ajoutés dans la configuration (`@file`, `@terminal`, `@diff`) permettent respectivement d’attacher un fichier précis, la sortie du terminal actif, ou les modifications en cours dans Git à votre question. Sur un premier import, l’indexation d’un gros dépôt peut prendre plusieurs minutes : Continue affiche une barre de progression dans la barre d’état de VS Code pendant ce calcul.

## Étape 9 : Test grandeur nature du projet complet

Il est temps de vérifier que toute la chaîne fonctionne ensemble. Créez un nouveau dossier de projet, ouvrez-le dans VS Code, et créez un fichier `calculatrice.py` vide. Commencez à taper une signature de fonction :

`def diviser(a, b):`
Après une courte pause, une suggestion grisée doit apparaître automatiquement, proposant par exemple une gestion de la division par zéro. Appuyez sur Tab pour l’accepter. Ouvrez ensuite le panneau de chat (Ctrl+L) et posez une question qui mobilise le contexte du projet entier : “@codebase quelles fonctions de ce dossier ne gèrent pas les exceptions ?”. Le modèle doit lister les fonctions concernées en se basant sur l’index construit à l’étape précédente. Terminez en sélectionnant un bloc de code dans l’éditeur, clic droit, puis “Continue : Edit”, et demandez une refactorisation, par exemple l’ajout d’une vérification de type. Ajoutez un dernier test pour valider la génération de tests unitaires : demandez au chat “@file calculatrice.py génère des tests unitaires avec pytest pour la fonction diviser, y compris le cas de la division par zéro”. Le modèle doit produire un fichier de test complet avec au moins deux cas, le cas normal et le cas d’exception. Si ces quatre interactions, complétion, chat contextuel, édition et génération de tests, fonctionnent sans message d’erreur, l’environnement complet est opérationnel et prêt pour un usage quotidien.

## Étape 10 : Optimiser les performances (GPU, quantification, Flash Attention)

Si les réponses restent lentes malgré un GPU compatible, vérifiez d’abord qu’Ollama détecte bien la carte graphique :

```
ollama ps
# La colonne PROCESSOR doit indiquer "100% GPU"
# et non "100% CPU"
```
Sur les GPU Nvidia de génération récente (RTX 5090, 5080, 5070 notamment), Ollama active automatiquement l’accélération CUDA et le Flash Attention. Sur du matériel plus ancien, cette optimisation peut nécessiter une activation manuelle via une variable d’environnement :

`OLLAMA_FLASH_ATTENTION=1 ollama serve`
Si la VRAM disponible reste juste, privilégiez une version quantifiée plus agressive du même modèle (suffixe `q4_K_M` plutôt que `q8_0`, par exemple), au prix d’une légère perte de précision sur les réponses les plus complexes. Sur les puces Apple Silicon, Ollama s’appuie sur le moteur MLX en complément de llama.cpp : introduit en aperçu dans la version 0.19 dès mars 2026, ce moteur est passé en version stable avec la 0.30 le 13 mai 2026, doublant la vitesse d’inférence sur Apple Silicon selon Andrew.ooo, avant d’être encore affiné dans la 0.30.10 ; les notes de version du 14 août 2026 signalent par ailleurs, selon ReleaseBot, une nouvelle accélération de Qwen3.5 sur GPU Apple grâce à des moteurs MLX et llama.cpp mis à jour.

## Étape 11 : Sécuriser l’environnement et les données de code

Un environnement local n’est pas automatiquement un environnement sûr. Trois réglages simples réduisent le risque de fuite accidentelle. D’abord, ajoutez le dossier de configuration de Continue à votre `.gitignore` global pour éviter de committer par erreur des clés d’API si vous configurez aussi des modèles cloud en complément :

```
# Dans ~/.gitignore_global ou le .gitignore du projet
.continue/
```
Ensuite, si votre machine est accessible sur un réseau partagé, n’exposez pas le port 11434 au-delà de `localhost` sans authentification : par défaut, Ollama n’écoute que sur l’interface locale, ce qui suffit pour l’usage décrit dans ce tutoriel. Enfin, si votre configuration mélange un modèle local pour le code sensible et un modèle cloud pour d’autres tâches, vérifiez à chaque session que le rôle `chat` par défaut pointe bien vers le modèle local avant d’ouvrir un fichier confidentiel, pour éviter d’envoyer par erreur du code protégé vers un fournisseur externe.

## Étape 12 : Automatiser avec le mode agent

Depuis le lancement initial de la commande `ollama launch` en juin 2026, selon Angelo Lima, puis son intégration native lors de la mise à jour 0.32.0 du 11 juillet 2026, suivie de la 0.32.1 le 16 juillet 2026 qui a notamment amélioré la gestion des appels d’outils (tool calling) pour Gemma 4, une évolution qualifiée par Dedimax de plus grand changement pour le projet depuis l’introduction du support GPU, Ollama propose une expérience d’agent intégrée directement en ligne de commande, avec la commande `ollama launch` qui démarre un agent de codage complet, télécharge le modèle s’il manque encore, et configure les variables d’environnement nécessaires en une seule étape :

`ollama launch qwen3-coder:30b`
Ce mode agent peut aussi être piloté depuis Continue en associant le modèle local à des outils agentiques comme Cline, Aider ou OpenHands, cités plus haut comme compatibles avec Devstral Small 2 24B et Qwen3-Coder-Next pour des tâches qui touchent plusieurs fichiers d’affilée : lecture du dépôt, planification du changement, édition, puis exécution des tests, sans validation manuelle à chaque ligne.

## Alternatives et comparatif : Ollama face à GitHub Copilot et Cursor

Ollama et Continue ne sont pas les seules briques disponibles pour un assistant de code local. LM Studio, qui totalise 14 800 recherches mensuelles en France, propose une interface graphique pour charger des modèles GGUF et sert aussi une API compatible OpenAI que Continue peut consommer à la place d’Ollama, ce qui en fait une alternative intéressante pour un développeur qui préfère parcourir un catalogue de modèles visuellement plutôt que de retenir des noms de commandes. Pour les développeurs qui travaillent sous JetBrains (IntelliJ, PyCharm, WebStorm) plutôt que VS Code, l’extension Continue existe aussi en version plugin JetBrains, avec la même logique de configuration et le même fichier `config.yaml` partagé entre les deux éditeurs. Les utilisateurs de Neovim disposent d’intégrations communautaires équivalentes, moins packagées mais fonctionnelles avec la même API Ollama exposée sur le port 11434, ce qui signifie qu’un seul serveur Ollama peut servir plusieurs éditeurs différents sur la même machine sans configuration supplémentaire.

| Critère | Ollama + Continue (local) | GitHub Copilot | Cursor | 
|---|---|---|---|
| Coût | Gratuit (hors matériel) | À partir de 10 $/mois (Individual), 19 $/utilisateur/mois (Business) | Payant, plans par abonnement | 
| Confidentialité du code | 100 % local, rien ne quitte la machine | Envoyé aux serveurs cloud GitHub / partenaires | Envoyé aux serveurs cloud Cursor | 
| Fonctionnement hors ligne | Oui, après téléchargement du modèle | Non | Non | 
| Statut du projet (juillet 2026) | Continue : version finale 2.0.0, dépôt en lecture seule, code Apache 2.0 toujours utilisable | Développement actif chez Microsoft/GitHub | Développement actif, rachat par SpaceX annoncé (clôture visée T3 2026) | 
| Choix du modèle | Libre : Qwen, Devstral, Llama, Gemma, etc. | Modèles propriétaires ou partenaires imposés | Modèles propriétaires ou partenaires imposés | 

## Quand rester en local, quand basculer vers le cloud

Un assistant local n’a pas vocation à remplacer un modèle cloud dans toutes les situations, et le présenter comme une solution universelle serait malhonnête. La bonne question à se poser n’est pas “local ou cloud” dans l’absolu, mais “quel outil pour quelle tâche”. Pour l’autocomplétion pendant la frappe, pour les questions rapides sur un fichier ouvert, ou pour toute tâche qui touche à du code sous obligation de confidentialité, un modèle local comme ceux configurés dans ce tutoriel couvre l’essentiel des besoins sans compromis sur la confidentialité.

À l’inverse, pour une refactorisation d’architecture qui touche des dizaines de fichiers avec des dépendances croisées complexes, ou pour un raisonnement qui demande de tenir compte de contraintes métier difficiles à formuler dans un prompt court, un modèle cloud de pointe garde souvent un avantage réel : sa taille dépasse largement ce qu’il est possible de faire tourner sur une machine de bureau, même haut de gamme. La configuration présentée à l’étape 6 n’empêche d’ailleurs pas d’ajouter un second modèle cloud dans le même fichier `config.yaml`, avec son propre rôle, pour basculer volontairement d’un modèle à l’autre selon la tâche du moment plutôt que de choisir un seul outil pour tout faire. Beaucoup d’équipes qui ont adopté cette approche en 2026 réservent le modèle local au code propriétaire ou soumis à un contrat de confidentialité, et le modèle cloud aux tâches sur du code déjà public, comme des projets open source personnels.

## Les pièges courants à éviter

La plupart des déceptions rencontrées avec un assistant de code local viennent d’un petit nombre d’erreurs récurrentes, faciles à éviter une fois identifiées.

- **Choisir un modèle trop gros pour sa VRAM.** Un modèle de 30 milliards de paramètres sur une carte à 8 Go force un déchargement partiel vers la RAM système, avec un effondrement des performances à la clé. Repartez du tableau de l’étape 3 si les réponses mettent plus de 20 à 30 secondes à arriver.
- **Oublier qu’Ollama doit tourner avant d’ouvrir VS Code.** Sans le service actif, Continue affiche une erreur de connexion silencieuse dans le panneau de chat plutôt qu’un message explicite.
- **Attendre une qualité identique à un modèle propriétaire de pointe.** Un modèle quantifié de 7 à 30 milliards de paramètres reste un compromis. Sur des tâches d’architecture complexes ou des bases de code très volumineuses, la différence avec un modèle cloud haut de gamme reste perceptible.
- **Committer le dossier de configuration par erreur.** Si un modèle cloud est ajouté en complément avec une clé d’API, un`.gitignore` manquant peut exposer cette clé dans l’historique Git du projet.
- **Ignorer la fenêtre de contexte du modèle choisi.** Un modèle à 32K tokens de contexte tronque silencieusement les fichiers volumineux envoyés au chat, ce qui produit des réponses incomplètes sans avertissement clair.
- **Négliger la mise à jour des pilotes GPU.** Une version de pilote Nvidia ou AMD trop ancienne empêche Ollama de détecter correctement la carte, et le calcul bascule sur le processeur sans message d’erreur visible.
- **Confondre le port par défaut avec une exposition réseau volontaire.** Modifier`OLLAMA_HOST` pour écouter sur toutes les interfaces réseau, par exemple pour un usage multi-machines, sans ajouter de contrôle d’accès expose le serveur de modèles à tout appareil du même réseau.

## Dépannage : résoudre les problèmes courants

Voici les symptômes les plus fréquemment rencontrés lors de la mise en place de cet environnement, avec leur cause probable et la correction à appliquer.

| Symptôme | Cause probable | Solution | 
|---|---|---|
| “ollama: command not found” | Le binaire n’est pas dans le PATH après installation | Redémarrez le terminal, ou réinstallez avec le script officiel | 
| “connection refused” sur le port 11434 | Le service Ollama n’est pas démarré | Lancez `ollama serve` dans un terminal dédié | 
| Le téléchargement du modèle s’arrête ou expire | Connexion instable sur un fichier de plusieurs Go | Relancez `ollama pull` , qui reprend le téléchargement interrompu | 
| Continue n’affiche aucun modèle disponible | Fichier config.yaml mal formé ou Ollama non démarré | Vérifiez l’indentation YAML et testez l’API avec curl avant de rouvrir VS Code | 
| L’autocomplétion est très lente | Modèle trop volumineux ou calcul sur CPU | Passez à un modèle plus petit ou vérifiez la détection GPU avec `ollama ps` | 
| Erreur de mémoire ou crash au chargement | VRAM insuffisante pour le modèle choisi | Utilisez une quantification plus agressive ou un modèle plus petit | 
| Le GPU n’est pas détecté | Pilotes graphiques obsolètes ou absents | Mettez à jour les pilotes Nvidia/AMD/Apple avant de relancer Ollama | 
| Erreur de syntaxe dans config.yaml | Indentation incorrecte (les espaces comptent en YAML) | Repartez de l’exemple de l’étape 6 et validez avec un linter YAML | 
| @codebase ne trouve rien dans le dépôt | Indexation par embeddings pas encore terminée ou modèle d’embeddings manquant | Vérifiez que nomic-embed-text est bien téléchargé et attendez la fin de l’indexation | 
| VS Code ne détecte pas l’extension après installation | Redémarrage de l’éditeur non effectué | Rechargez la fenêtre via la palette de commandes (“Reload Window”) | 

## Conseils avancés pour aller plus loin

Une fois l’environnement de base stable, plusieurs réglages permettent d’en tirer davantage. Séparer les rôles entre deux modèles différents, un modèle léger et rapide pour l’autocomplétion et un modèle plus large pour le chat, comme configuré à l’étape 6, donne le meilleur équilibre entre réactivité et qualité des réponses : c’est la logique déjà retenue par la configuration de ce tutoriel.

Continue permet aussi de créer des commandes personnalisées, appelées “slash commands”, pour des actions répétitives comme générer des tests unitaires ou rédiger un message de commit à partir du diff en cours. Ces commandes se définissent dans le même fichier `config.yaml`, sous une section `prompts` dédiée. Pour les équipes qui veulent partager une configuration commune, ce fichier peut être versionné dans un dépôt central et distribué à chaque poste, en laissant le champ `apiBase` configurable par variable d’environnement si certains développeurs préfèrent un serveur Ollama partagé sur le réseau local plutôt qu’une instance par machine.

Enfin, pensez à réévaluer le choix du modèle tous les deux ou trois mois. Le rythme de sortie des modèles ouverts spécialisés dans le code reste soutenu, et un modèle jugé secondaire aujourd’hui peut devenir le meilleur choix pour votre profil matériel après une nouvelle quantification ou une nouvelle version, sans qu’aucune autre partie de cette configuration n’ait besoin de changer.

Si plusieurs développeurs de la même équipe travaillent sur des machines aux capacités différentes, évitez d’imposer un seul modèle pour tout le monde. Un plus gros modèle sur la machine d’un développeur senior ne sert à rien si trois autres membres de l’équipe tournent sur des portables sans GPU dédié : dans ce cas, laissez chaque poste choisir son propre modèle d’autocomplétion selon le tableau de l’étape 3, et réservez un modèle commun, plus large, hébergé sur un serveur Ollama partagé sur le réseau local, pour les tâches de chat et d’agent qui demandent davantage de puissance. Cette approche hybride évite d’acheter du matériel neuf pour toute l’équipe alors qu’un seul serveur suffit à mutualiser l’accès aux modèles les plus gourmands.

## Foire aux questions

### Ollama est-il gratuit ?

Oui, Ollama est un logiciel open source et gratuit. Le seul coût réel est celui du matériel nécessaire pour faire tourner les modèles dans de bonnes conditions, en particulier la VRAM si vous visez des modèles plus grands que 7 milliards de paramètres.

### Quelle est la différence entre Ollama et LM Studio ?

Les deux servent des modèles en local, mais Ollama fonctionne d’abord en ligne de commande avec une API HTTP, pensée pour s’intégrer à des outils tiers comme Continue. LM Studio propose une interface graphique complète pour parcourir, télécharger et discuter avec des modèles, avec en plus un serveur compatible API OpenAI activable en un clic.

### Continue.dev fonctionne-t-il encore après son rachat par Cursor ?

Oui. La version 2.0.0, sortie le 19 juin 2026, reste installable et pleinement fonctionnelle avec un modèle local via Ollama. Le dépôt GitHub est en lecture seule et n’accueillera plus de nouvelles fonctionnalités officielles, mais le code Apache 2.0 continue de tourner normalement, y compris pour la configuration décrite dans ce tutoriel.

### Quel modèle choisir si je n’ai pas de carte graphique dédiée ?

Partez sur qwen2.5-coder:7b en version quantifiée, qui tourne sur processeur avec 8 Go de RAM disponibles. Les temps de réponse seront plus longs qu’avec un GPU, mais l’autocomplétion et le chat restent utilisables pour un usage ponctuel.

### Mon code envoyé à Ollama quitte-t-il mon ordinateur ?

Non, tant que la configuration pointe vers `http://localhost:11434` comme indiqué à l’étape 6. Le modèle tourne entièrement sur votre machine et aucune requête ne part vers un serveur externe, sauf si vous ajoutez volontairement un modèle cloud dans la même configuration Continue.

### Ollama peut-il remplacer complètement GitHub Copilot ?

Pour l’autocomplétion et le chat contextuel du quotidien, oui, dans la plupart des cas. Sur des tâches d’architecture très complexes ou des raisonnements multi-étapes sur un très gros dépôt, un modèle cloud haut de gamme garde souvent l’avantage. Beaucoup d’équipes utilisent les deux en parallèle, en réservant le local au code sensible.

### Comment mettre à jour un modèle déjà téléchargé ?

Relancez simplement la commande `ollama pull` suivie du même nom de modèle. Ollama ne télécharge que les blocs modifiés par rapport à la version déjà présente sur le disque, ce qui accélère nettement la mise à jour par rapport à un premier téléchargement.

### Puis-je utiliser Ollama avec JetBrains ou Neovim au lieu de VS Code ?

Oui. Continue propose un plugin JetBrains qui reprend la même logique de configuration que celle décrite ici pour VS Code. Pour Neovim, plusieurs intégrations communautaires consomment directement l’API Ollama sur le port 11434, avec un principe de fonctionnement identique.
