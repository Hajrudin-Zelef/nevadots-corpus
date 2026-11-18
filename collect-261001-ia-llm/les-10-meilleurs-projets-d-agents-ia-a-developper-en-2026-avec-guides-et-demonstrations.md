Cours

Les agents IA sont des systèmes avancés, axés sur des objectifs, qui perçoivent le contexte et exécutent des flux de travail en plusieurs étapes avec un minimum de supervision, contrairement aux chatbots de base. Ils peuvent appeler des API, utiliser des logiciels, interroger des données et exploiter la mémoire et les boucles de rétroaction.

Ces agents sont devenus un élément central de l'écosystème de l'IA, et les connaître vous aidera à démarrer votre carrière dans ce domaine ou à améliorer vos compétences actuelles. La meilleure façon de comprendre un nouveau concept est de le mettre en pratique.

Dans cet article, nous examinerons 10 projets d'agents IA utiles pour tous les niveaux de compétence :

- Débutants : Développez rapidement à l'aide d'outils low-code tels que Langflow, Flowise et Make AI.
- s intermédiaires: Veuillez utiliser des frameworks tels que LangGraph, Mistral Agents et Qwen-Agent pour créer des agents personnalisés.
- Avancé : Concevez des systèmes multi-agents avec Haystack, ADK et CrewAI.

## Projets d'agent IA pour débutants

Les projets simples utilisent des outils GUI ou des outils d'agent low-code. Vous pouvez glisser-déposer et connecter visuellement des composants, ajouter votre clé API LLM et exécuter le pipeline. Ces générateurs vous permettent de créer des prototypes d'agents avec état, d'intégrer des sources de données et des API, et d'enchaîner des invites et des appels d'outils sans code standard.

### 1. Professeur de langues avec Langflow

Language Tutor avec Langflow est un petit système agentique qui génère des passages de lecture courts adaptés au vocabulaire actuel de l'apprenant. Il fonctionne sur Langflow avec une base de données Postgres (via Docker) et utilise psycopg2 pour lire/écrire des mots.

Vous téléchargez votre vocabulaire à partir d'un fichier CSV, ajoutez de nouveaux mots à l'aide d'un outil dans le chat, et un outil de narration récupère les mots que vous avez enregistrés pour demander à un LLM d'écrire une histoire dans la langue de votre choix. L'agent principal achemine votre demande, « ajouter un mot » ou « créer une histoire », et renvoie le résultat.

Guide : Langflow : Un guide avec un projet de démonstration

### 2. Agent IA analyste de données avec Flowise

L'agent IA Data Analyst avec Flowise est un flux de travail qui vous permet de poser des questions sur une base de données et d'obtenir des réponses avec le code SQL exact utilisé.

Vous connectez Flowise à une base de données SingleStore, ajoutez un bloc de code personnalisé pour lire le schéma du tableau, puis l'intégrez dans une invite qui demande à un LLM (via une chaîne LLM avec OpenAI) de générer une requête SQL.

La requête est stockée, nettoyée, exécutée via un autre bloc de code personnalisé, et les résultats ainsi que la requête sont formatés par une invite finale et une chaîne LLM.

Guide : Flowise : Un guide avec un projet de démonstration

### 3. Automatisation du service client avec Make AI

L'agent IA du service client avec Make répond automatiquement aux demandes de location soumises via un formulaire Tally.

Lorsqu'une personne remplit un formulaire Tally, Make AI récupère les détails de la location à partir d'un document Google Doc. Ensuite, un module OpenAI rédige une réponse en utilisant ces informations et la question posée par la personne. Enfin, le module de messagerie électronique envoie la réponse directement à l'adresse électronique indiquée dans le formulaire.

Guide : Créer une IA : Un guide avec des exemples pratiques

## Projets intermédiaires d'agent IA

Les projets intermédiaires d'agents IA se concentrent sur la création de flux de travail complets à l'aide de cadres et d'API agents modernes. À ce stade, vous ne vous contentez pas de connecter des API, vous concevez également des interfaces utilisateur simples afin que les utilisateurs puissent interagir directement avec les agents.

### 4. Coach en nutrition avec Mistral Agents

Nutrition Coach avec Mistral Agents est un projet d'agent IA qui enregistre vos repas, estime les calories et suggère un prochain repas sain, accompagné d'une image.

L'application comprend l'API Agents de Mistral, qui dispose d'un agent de recherche Web permettant d'obtenir des estimations caloriques. Il dispose également d'un estimateur de secours, d'un enregistreur pour consigner les repas, les calories et les horodatages, ainsi que d'un agent de génération d'images pour visualiser le plat suggéré.

Les utilisateurs peuvent saisir leurs repas et leurs préférences, et l'application recherchera ou estimera la teneur en calories, enregistrera la saisie, suggérera un repas complémentaire et affichera une image générée automatiquement du plat, accompagnée d'un résumé clair des outils utilisés.

Guide : API Mistral Agents : Un guide avec un projet de démonstration

### 5. Assistant de recherche approfondie avec Jan-v1

Le projet d'agent IA Deep Research Assistant avec Jan‑v1 est une application qui transforme un sujet en un rapport de recherche soigné à l'aide de l'inférence locale Jan‑v1, d'une recherche Web asynchrone et d'un formatage strict du rapport. Il génère des requêtes intelligentes, extrait des sources et synthétise un rapport clair et professionnel.

Il utilise Streamlit pour l'interface utilisateur, llama-cpp pour exécuter localement un modèle GGUF Jan-v1, Serper pour la recherche sur le Web et un ensemble de fonctions d'aide pour le découpage et le nettoyage.

Les utilisateurs saisissent un sujet, choisissent le niveau de détail, l'orientation, la période et le format ; l'application génère des requêtes, effectue des recherches asynchrones, compile des notes et demande à Jan‑v1 de produire un rapport structuré, puis affiche les sources et vous permet d'exporter au format TXT/JSON. Les barres de progression permettent de suivre les étapes, et l'état de la session évite de recharger le modèle entre les exécutions.

Guide : Janvier-V1 : Un guide avec un projet de démonstration

### 6. Extension de synthèse Web en temps réel avec Qwen-Agent

L'extension Real-Time Web Summarizer est un module complémentaire Chrome qui capture le texte visible de n'importe quelle page et diffuse un résumé clair et concis en temps réel, alimenté localement par Qwen3 via Ollama et un backend FastAPI.

Les utilisateurs peuvent cliquer sur « Résumer » dans la fenêtre contextuelle, comme illustré ci-dessous. L'extension récupère le texte de la page, l'envoie à http://127.0.0.1:7864/summarize_stream_status et affiche la réponse en continu.

Pour le configurer, il est nécessaire de télécharger le modèle Qwen à l'aide d'Ollama, de démarrer le serveur FastAPI, de charger l'extension décompressée, et vous pourrez alors obtenir des résumés instantanés sur n'importe quelle page, présentés dans un format clair, de type éditeur.

Guide : Agent Qwen : Un guide avec un projet de démonstration

### 7. Analyse en temps réel avec LangGraph

Real-time Analytics est un assistant optimisé par LangGraph qui répond aux questions, effectue des recherches sur le Web et exécute du code Python. Il combine Mistral Medium 3 pour le raisonnement, Tavily pour la recherche sur le Web et un REPL Python pour exécuter du code en fonction des invites utilisateur.

Lorsque les utilisateurs posent une question, l'agent détermine s'il convient d'effectuer une recherche, d'exécuter du code ou d'utiliser les deux méthodes. Il fournit ensuite la réponse finale ainsi que des informations sur les outils utilisés. Le processus de configuration comprend l'ajout de la clé API Tavily, l'installation des paquets nécessaires, l'initialisation du LLM et des outils, la création de l'agent LangGraph et son invocation avec les messages des utilisateurs.

Guide : Tutoriel Mistral Medium 3 : Développement d'applications agencées

## Projets avancés d'agents IA

Les projets d'agents IA avancés rassemblent plusieurs agents travaillant avec divers outils dans un seul flux de travail. Ces projets utilisent des frameworks agents puissants tels que Haystack, ADK et CrewAI, vous permettant de concevoir des systèmes collaboratifs complexes dans lesquels les agents peuvent se coordonner, se spécialiser et accomplir des tâches plus sophistiquées.

### 8. RAG et agents IA Web avec Haystack AI

Haystack Agentic RAG and Web Access est un assistant qui répond aux questions à l'aide d'une base de connaissances privée et, si nécessaire, de résultats Web en temps réel. Il achemine d'abord les requêtes vers la génération augmentée par la récupération, puis passe à la recherche sur le Web pour obtenir des informations récentes et en temps réel.

Il utilise les pipelines et agents Haystack, un magasin de documents en mémoire avec des intégrations OpenAI, un outil RAG personnalisé et un outil de recherche Web Tavily personnalisé intégré sous le nom de ComponentTool. L'agent est alimenté par GPT 4.1 Mini avec une invite système qui guide la sélection des outils.

Les utilisateurs peuvent poser des questions, et l'agent récupère les informations dans la base de connaissances ou effectue une recherche Tavily sur des sujets d'actualité. Il fournit ensuite une réponse accompagnée des outils utilisés.

Guide : Tutoriel Haystack AI : Création de workflows agencés

### 9. Planificateur de voyage avec ADK et A2A

Travel Planner avec ADK et A2A est une application multi-agents complète conçue pour planifier des voyages de A à Z. Les utilisateurs n'ont qu'à saisir leur destination, leurs dates de voyage et leur budget. Des agents spécialisés vous recommandent ensuite des vols, des hébergements et des activités. Un agent hôte coordinateur organise tous les composants, tandis qu'une interface utilisateur Streamlit affiche l'itinéraire complet.

Les utilisateurs remplissent le formulaire dans Streamlit, l'agent hôte transmet les données à l'ensemble des trois agents, fusionne leurs réponses JSON et renvoie un plan structuré comprenant les vols, les hôtels et les activités.

### 10. RAG agentique avec CrewAI

Le pipeline Agentic RAG est un système de routage des requêtes qui fournit des réponses contextuelles à partir de fichiers PDF locaux ou, si nécessaire, du Web en temps réel.

Il utilise FAISS sur des segments PDF, Groq pour des réponses LLM rapides et un flux de travail web crewAI pour le contexte externe. Une invite du routeur détermine si les données locales sont suffisantes (« Oui/Non ») et les utilitaires gèrent la récupération, le web scraping et la synthèse.

Les utilisateurs posent une question et le routeur examine le contexte du PDF. Si la réponse est « Oui », le système récupère les correspondances les plus pertinentes dans la base de données vectorielle. Si la réponse est « Non », il effectue une recherche sur le Web, compile les informations recueillies, et le modèle linguistique formule la réponse finale.

Guide : RAG agentique : Tutoriel étape par étape avec projet de démonstration

## Conclusions finales

Concevoir vos propres projets d'IA à partir de zéro est l'un des meilleurs moyens de dynamiser votre carrière. Il vous aide à aller au-delà de la théorie et à acquérir des compétences pratiques. Vous apprendrez à définir un problème, à connecter des outils et des agents, à valider les résultats, à créer une interface utilisateur simple et à apporter des améliorations en fonction des commentaires réels des utilisateurs.

Chaque projet que vous terminez vient s'ajouter à votre portfolio, que vous pouvez partager sur des plateformes telles que GitHub ou Hugging Face, ou sous forme de démonstration en direct. Cela démontre aux employeurs potentiels que vous êtes capable de concevoir et de déployer des systèmes fiables, et pas seulement d'utiliser des modèles existants. Cela démontre votre compréhension du développement et de l'ingénierie des produits, ce qui vous rend plus attractif pour les responsables du recrutement dans des domaines tels que l'apprentissage automatique appliqué et l'ingénierie de l'IA.

**Je recommande de commencer par de petits projets et de viser à les publier fréquemment. Permettez à votre travail de parler de lui-même, chaque projet témoigne de votre évolution et de votre expertise dans le domaine de l'IA. Si vous débutez dans le domaine de l'IA agentielle, je vous recommande également de suivre le cours cours Introduction aux agents IA et de consulter notre aide-mémoire sur les agents IA.** 

En tant que data scientist certifié, je suis passionné par l'utilisation des technologies de pointe pour créer des applications innovantes d'apprentissage automatique. Avec une solide expérience en reconnaissance vocale, en analyse de données et en reporting, en MLOps, en IA conversationnelle et en NLP, j'ai affiné mes compétences dans le développement de systèmes intelligents qui peuvent avoir un impact réel. En plus de mon expertise technique, je suis également un communicateur compétent, doué pour distiller des concepts complexes dans un langage clair et concis. En conséquence, je suis devenu un blogueur recherché dans le domaine de la science des données, partageant mes idées et mes expériences avec une communauté grandissante de professionnels des données. Actuellement, je me concentre sur la création et l'édition de contenu, en travaillant avec de grands modèles linguistiques pour développer un contenu puissant et attrayant qui peut aider les entreprises et les particuliers à tirer le meilleur parti de leurs données.