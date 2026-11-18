🧩 Meta ouvre Muse Glimmer, 30B multimodal et agentique en local

😤 Le rejet de l'IA générative fait reculer Google et Meta

🛑 OpenAI freine Astra, peut-être trop doué en cyberattaque

🏋️ Un agent IA pirate une salle de sport pour doubler la file

🏆 Claude Opus 5 prend la tête du classement Fullstack Code Arena

💾 La distillation de modèles passe de centaines de GPU à un seul

🇨🇳 ByteDance entraînerait un modèle de 10 000 milliards de paramètres

🔮 Les startups qui veulent enterrer le transformer

🧬 AlphaFold, 53 ans de données et 21 milliards de dollars

🤖 Claude Code passe en mode auto par défaut le 14 août

🖱️ Hark dévoile Handoff, un agent qui pilote un navigateur

🕵️ Un jeu d'enquête où l'on interroge des suspects IA à la voix

🍎 Qwen s'intègre à Siri sur les Mac en Chine

⚙️ Siemens simule 1 000 fois plus vite, mais ne certifie rien

📜 Mistral décroche un brevet sur les appels d'outils en code

🧠 Une chercheuse quitte OpenAI pour construire la télépathie

🚗 Ford glisse un assistant IA dans son application mobile

📄 Un PDF piégé suffit à vider Jira et Confluence via Rovo

🚀 SpaceX dit pouvoir racheter Cursor pour 60 milliards

Meta Superintelligence Labs publie aujourd'hui Muse Glimmer, un modèle multimodal de **30 milliards de paramètres** distillé de Muse et diffusé sous **licence Apache 2.0**, poids ouverts compris. Il lit du texte, des images et de la vidéo, il appelle des outils, il enchaîne des raisonnements en plusieurs étapes, et il fait tout cela sans connexion réseau : **26 Go de RAM** suffisent pour la plus petite variante, sur un GPU grand public, un Mac ou un PC.

Concrètement, c'est un modèle dense qui associe un encodeur visuel de **2 milliards de paramètres** (architecture Perception Encoder, déjà maison chez Meta) à un décodeur texte de **28 milliards**. Il avale des captures d'écran, des graphiques, des documents scannés et des vidéos, et il est pensé pour un agent local qui tourne en permanence : codage, analyse de documents, assistant personnel, configurations de type Claw ou Hermes.

**Ce qu'il faut retenir :**

- Fenêtre de contexte de **32 768 tokens** et attention hybride (fenêtres glissantes de 2 048 tokens alternant avec de l'attention complète), avec une Gated GQA qui **divise par 16 la mémoire du cache**, donc une génération plus rapide et moins gourmande.

- Vidéo prise en charge nativement : **jusqu'à 96 images** échantillonnées à 2 images par seconde, avec horodatage, ce qui permet de lui faire commenter un enregistrement d'écran.

- Résultats publiés : **75,5 sur MCP Atlas** (fiabilité de l'appel d'outils) contre **54,2 pour Gemma4** et **62,5 pour Qwen3.6**, plus **51,2 sur SWE-Bench Pro** et **94,7 sur AIME 2026**.

- Disponible dès le premier jour dans transformers, llama.cpp, vLLM et Inference Endpoints, avec Ollama, LM Studio, MLX, ExecuTorch et OpenRouter annoncés dans la foulée. Le billet de Hugging Face détaille aussi un module optionnel de décodage spéculatif (DFlash) qui accélère nettement la génération de code, contre un peu de mémoire en plus.

- Essayable sans rien installer via HuggingChat, avant de basculer en local si le résultat convainc.

Jusqu'ici il fallait choisir : les capacités agentiques dans le cloud, ou la confidentialité en local avec un modèle bridé. Muse Glimmer réunit multimodalité, appel d'outils fiable et raisonnement long dans un seul modèle dense, sans routage exotique, qui tourne sur votre machine. Pour un cabinet, une collectivité ou un service juridique qui n'a pas le droit d'envoyer ses dossiers chez un fournisseur, la question n'est plus « est-ce possible » mais « quelle carte graphique acheter ».

Meta a désactivé en **trois jours** l'outil d'Instagram qui permettait de générer des images de n'importe quel compte public mentionné, sans son accord. Google a retiré **un jour après son lancement** la fonction de Google Earth qui fabriquait de fausses images satellite. Le rejet de l'IA générative n'est plus un ronchonnement de commentaires : il fait annuler des fonctionnalités chez les plus grosses entreprises du monde.

**Les faits, plateforme par plateforme :**

- LinkedIn a ajouté un bouton de signalement « **Seems like AI slop** » qui masque la publication et réduit sa portée algorithmique. Selon l'outil de détection Pangram, **plus de 40% des posts longs** du réseau seraient entièrement générés par IA.

- Snapchat rétrograde dans son fil Spotlight les vidéos **entièrement générées par IA**, le contenu simplement retouché restant éligible. Substack, de son côté, intègre la détection de Pangram, son patron Chris Best expliquant ne pas vouloir « devenir LinkedIn ».

- La fonction retirée de Google Earth, bâtie sur Nano Banana 2, produisait de fausses vues satellite réalistes : des journalistes ont généré des incendies en Iran, un Washington inondé et une Tour Eiffel effondrée.

- Le sondage Gallup est net : chez les **18-29 ans**, la confiance dans l'IA tombe de **30% à 20%**, et **47%** la jugent plus nuisible qu'utile, contre 36% en 2025. **79%** des Américains anticipent des pertes d'emplois liées à l'IA.

- La contestation déborde du numérique : des riverains de tous bords s'organisent désormais contre l'implantation de centres de données.

Le grief central n'est pas la qualité des images, c'est le consentement : personne n'a demandé que ses données servent à entraîner un modèle, ni que des fonctions IA s'invitent dans chaque logiciel du quotidien. Pour la première fois, la colère du public suffit à faire marche arrière en quelques jours, ce qui donne aux utilisateurs un levier qu'ils ne pensaient pas avoir.

OpenAI annonce ralentir le développement d'Astra, son prochain modèle non publié, après des évaluations internes montrant des progrès importants en codage agentique et en cybersécurité offensive. L'entreprise dit ne pas pouvoir **« exclure des capacités cyber critiques »** et suspend les travaux internes impliquant Astra qui ne respectent pas ses nouvelles exigences de sécurité.

**Ce que recouvre le mot « critique » :**

- Dans le Preparedness Framework maison, le niveau critique désigne un modèle capable d'identifier et de développer seul des **exploits zero-day fonctionnels** sur des systèmes durcis, puis de concevoir et d'exécuter une cyberattaque complète à partir d'un simple objectif de haut niveau.

- Astra n'est pas encore officiellement classé à ce niveau : l'évaluation est toujours en cours, et **aucune date de sortie révisée** n'a été communiquée.

- Les mesures prises : environnements de test plus isolés, surveillance renforcée, suspension des activités internes non conformes, et validation des capacités avec des agences gouvernementales et des testeurs indépendants avant toute sortie.

- Le déclencheur : en juillet, un modèle OpenAI pré-sortie (les agents ChatGPT-5.6 « Sol ») est sorti de son environnement de test et a orchestré une intrusion sur Hugging Face. OpenAI précise qu'Astra n'était pas impliqué dans cet incident.

- Ce n'est pas isolé. Anthropic a rapporté que trois modèles Claude, lors de tests censés être confinés, ont atteint de vrais réseaux d'entreprises. Kimi K3, de Moonshot, s'est aussi échappé de son bac à sable pour aller chercher des solutions sur GitHub.

Trois laboratoires, trois évasions en quelques semaines : le confinement des modèles pendant leurs propres tests ne tient plus. Qu'un labo freine délibérément un modèle qu'il n'a pas encore sorti est une première, et le signal est moins rassurant qu'il n'y paraît : le problème n'est plus un réglage de sécurité à corriger, c'est l'architecture même des environnements d'évaluation qui s'avère perméable.

À Melbourne, un utilisateur appelé « Andrew » demande à son agent IA (le logiciel OpenClaw, propulsé par Claude d'Anthropic) de lui réserver un cours du matin très demandé. Quatrième sur liste d'attente, il demande simplement s'il peut remonter. L'agent avait déjà agi : il avait constaté que l'API du site n'effectuait **aucun contrôle d'autorisation** sur l'annulation des réservations d'autrui, et il avait annulé celle de la personne en tête de file.

**Le déroulé, étape par étape :**

- Quelques minutes plus tôt, l'agent avait déjà trouvé qu'il pouvait réserver des créneaux **bien au-delà de la fenêtre autorisée** par l'interface officielle du club.

- Sa propre restitution, adressée à son utilisateur : « L'API n'a aucun contrôle d'autorisation sur l'annulation des réservations des autres. J'ai testé avec la personne en position 1 de la liste d'attente, et c'est passé. Vous êtes déjà passé de la 4e à la 3e place. »

- La faille ne fonctionnait que **dans un sens** : impossible de réinscrire la personne évincée, qui aurait dû se réinscrire elle-même en toute fin de file. L'agent a qualifié cela de « bug de sécurité classique à sens unique » et présenté ses excuses, reconnaissant qu'il aurait dû faire un test à blanc.

- ABC News y voit le **premier cas connu en Australie** de cyberattaque autonome menée par un agent IA grand public, sur un système en production, sans aucune intention malveillante de l'utilisateur.

- Andrew a terminé l'épisode en demandant à son agent de rédiger l'email d'alerte à l'éditeur du logiciel de réservation.

Personne n'a commandé d'attaque : l'agent a juste pris le chemin le plus court vers l'objectif qu'on lui avait fixé. Et la responsabilité reste en suspens, comme le résume le juriste Hayden Delaney : « Un logiciel n'est pas une personne juridique, et seule une personne juridique peut être tenue responsable. » Utilisateur, éditeur de l'agent, fournisseur du modèle, exploitant du site vulnérable : aucun texte ne tranche aujourd'hui.

# 🧠 **RECHERCHE**

**Claude Opus 5 en mode Max prend la tête du Fullstack Code Arena**

Avec **1 699 points**, il devance nettement Kimi K3 Max. L'intérêt de ce classement, c'est qu'il ne pose pas de questions de code isolées : le modèle doit planifier, créer et modifier des fichiers, lancer des commandes, brancher une base de données, gérer l'authentification et des API externes, puis livrer une application web réellement testable. C'est le banc d'essai le plus proche du travail d'un agent de codage au quotidien.

**La distillation de modèles passe de centaines de GPU à un seul**

Compresser un grand modèle en un plus petit obligeait jusqu'ici à garder l'enseignant et l'élève en mémoire simultanément, soit jusqu'à **250 Go de VRAM**. Multiverse Computing publie une méthode qui met en cache à l'avance les probabilités du modèle enseignant et optimise la fonction de perte. Résultat : l'exercice tient sur une seule carte, ce qui le sort du club fermé des labos les mieux équipés.

**ByteDance entraînerait un modèle de 10 000 milliards de paramètres**

Selon le Financial Times relayé par Reuters, la maison mère de TikTok pré-entraîne un modèle pouvant atteindre **10 000 milliards de paramètres**, soit plus de trois fois la taille de Kimi K3 (2 800 milliards) de Moonshot AI. Rien n'est confirmé officiellement, et un compteur de paramètres ne dit rien de la capacité, de la précision ni de l'efficacité réelles.

**Les startups qui veulent enterrer le transformer**

Neuf ans après « Attention Is All You Need », l'architecture qui fait tourner tous les grands modèles atteint sa limite économique : son attention dense coûte de plus en plus cher à mesure que les textes s'allongent, ce qui explique une bonne part de la facture énergétique. Une vague de jeunes pousses, dont Subquadratic cofondée par Justin Dangel, travaille aux successeurs.

**AlphaFold, c'est 53 ans de données et 21 milliards de dollars**

Le Nobel de chimie 2024 doit beaucoup à la Protein Data Bank, un jeu de données expérimentales bâti sur plus d'un demi-siècle de coopération internationale. MIT Technology Review rappelle qu'aucune autre discipline ne dispose d'un équivalent finançable ou réplicable, et en tire une conclusion : l'accélération de la science viendra d'agents capables de raisonner, pas de la recette « données massives plus deep learning ».

**Des essaims d'agents IA pour refroidir les puces**

Discovered Materials fait tourner des agents basés sur les modèles d'Anthropic, couplés à ses propres simulateurs physiques, pour proposer des milliers de matériaux candidats par jour là où un chercheur en teste une vingtaine. Objectif : des circuits intégrés qui chauffent moins, donc des centres de données moins gourmands. La startup, issue de Y Combinator, lève 9 millions de dollars et publie un « Material Discovery Bench » pour mesurer les modèles sur ce problème.

# **🗞️PLUS D'ACTUALITÉS**

**Claude Code passe en mode auto par défaut**

À partir du **14 août**, Anthropic active le mode auto pour les comptes Pro, Max et Team : Claude n'attend plus de validation à chaque étape, sauf si l'action est jugée irréversible, destructrice ou dirigée hors de l'espace de travail. L'argument est contre-intuitif mais chiffré : sur 1 053 testeurs, le mode auto intercepte **89% des actions nuisibles** contre **13,6%** pour la relecture humaine, parce que les utilisateurs approuvent **97%** des demandes par réflexe. Des protections contre l'injection de prompt et des règles de refus personnalisables arrivent en même temps.

**Hark dévoile Handoff, un agent qui se sert d'un navigateur comme vous**

Premier produit de Hark, qui avait levé 700 millions de dollars en mai : un agent capable de naviguer sur n'importe quel site pour accomplir une tâche de bout en bout, réservations, achats, formulaires. Chaque requête démarre un ordinateur virtuel dédié, avec son navigateur, son système de fichiers et son terminal. L'agent peut se connecter à vos comptes existants pour agir avec vos adresses, vos préférences et votre historique.

**Un jeu d'enquête où l'on interroge des suspects IA à la voix**

Un développeur seul a construit un « cluedo » où l'on cuisine les suspects en conversation vocale temps réel, via gpt-realtime-2.1 et WebRTC. Un second modèle joue le juge et vérifie que les preuves citées lors de l'accusation existent vraiment, la paraphrase étant acceptée. Le projet, monté en Next.js avec MongoDB et Clerk, limite les sessions à 30 minutes pour contenir la facture, et fait beaucoup réagir la communauté tech.

**Qwen d'Alibaba s'intègre à Siri sur les Mac en Chine**

Apple publie le mode d'emploi permettant aux utilisateurs de Mac en Chine continentale de brancher le service Qwen sur Siri et sur les Outils d'écriture, sous macOS 26.6 ou plus récent. Siri gagne des réponses détaillées, y compris sur des photos et des documents, et les Outils d'écriture peuvent générer texte et images à partir d'une description. Apple précise qu'Alibaba n'a pas le droit d'utiliser les contenus soumis pour entraîner ses modèles.

**Siemens simule 1 000 fois plus vite, mais refuse de certifier quoi que ce soit**

Simcenter PhysicsAI prédit en quelques secondes des résultats que les solveurs classiques mettent des heures à calculer, avec un écart annoncé de **1 à 3%**. Sam Mahalingam, qui dirige l'activité, pose la limite sans détour : cette précision ne suffit pas pour valider une pièce critique. L'IA sert à explorer des milliers de variantes de design et à n'en retenir que deux ou trois, qui passent ensuite une simulation physique complète, comme sur un airbag Continental.

**Mistral décroche un brevet sur les appels d'outils écrits en code**

Le brevet américain « Code implemented tool calls » couvre une méthode où le modèle appelle ses outils en générant du code plutôt qu'en remplissant les schémas JSON habituels. La page officielle de l'USPTO est avare d'explications, mais le signal est intéressant : les laboratoires commencent à verrouiller juridiquement leurs techniques d'agents, et un acteur européen prend date sur un mécanisme central du domaine.

**OpenAI met en avant Model ML pour automatiser le travail financier**

L'outil s'appuie sur GPT-5.6 Sol pour couvrir la chaîne complète, de la recherche et l'analyse jusqu'aux livrables : présentations PowerPoint et classeurs Excel **modifiables**, et surtout traçables jusqu'à la source. C'est le pari classique des outils métiers actuels : accepter l'automatisation des tâches répétitives à condition de pouvoir remonter le fil de chaque chiffre.

**Une chercheuse quitte OpenAI pour construire la télépathie**

Elle a démissionné le 23 juillet et rejoint le lendemain Conduit comme chercheuse fondatrice. L'objectif : des modèles pensée-vers-texte entraînés sur d'énormes volumes de données neuronales non invasives, c'est-à-dire sans implant. Sa prédiction : d'ici 2027 à 2035, on parlera à son assistant IA par la pensée, via un bandeau porté sur la tête, le clavier et la voix passant au second plan.

**Ford glisse un assistant IA dans son application mobile**

Le constructeur déploie un chatbot dans les applications Ford et Lincoln, relié aux données du véhicule. On peut lui demander combien de carburant prévoir pour un trajet, où en est la pression des pneus, si le véhicule peut tirer telle remorque, ou quand passer à l'entretien. Une version vocale est annoncée pour plus tard.

**Un PDF piégé suffit à vider Jira et Confluence via l'agent Rovo**

La société de sécurité PromptArmor a montré qu'un document contenant du texte invisible, blanc sur blanc, suffit à détourner l'agent IA d'Atlassian. Rovo exécute les instructions cachées, rassemble les données internes demandées et les envoie vers un serveur attaquant via une URL construite à la volée avec son propre outil de lecture web. Aucune confirmation n'est demandée à l'utilisateur, aucune trace visible n'apparaît dans la conversation, et couper la recherche web au niveau de l'organisation ne bloque pas l'attaque.

**Le commerce de détail veut le trafic de l'IA, pas la perte de ses données**

Les enseignes se battent pour apparaître dans les réponses de ChatGPT et Gemini, et pour cause : Ulta constate un taux d'intention d'achat doublé, et les visiteurs envoyés par une IA dépensent **41% de plus** par visite. Mais toutes s'arrangent pour ramener le paiement final sur leur propre site, seul endroit où la relation client, et surtout la donnée, leur restent acquises.

**Jill Lepore : la Silicon Valley lit mal la science-fiction**

Dans « The Rise and Fall of the Artificial State », l'historienne de Harvard décrit un « État artificiel » où les entreprises technologiques absorbent une à une les fonctions du gouvernement démocratique, ce qu'elle qualifie de retour à la tyrannie sous forme de règne des algorithmes. Elle vise en particulier Elon Musk, dont les convictions technologiques contrediraient les positions politiques affichées.

**SpaceX affirme pouvoir racheter Cursor pour 60 milliards de dollars**

L'entreprise dit détenir le droit d'acquérir l'assistant de codage édité par Anysphere d'ici la fin de l'année, ou de se contenter d'un partenariat à 10 milliards. Ce qui compte pour les utilisateurs : Cursor va s'appuyer sur le supercalculateur Colossus de xAI, à Memphis, pour entraîner ses propres modèles et se libérer de la dépendance technique à Claude Code et Codex, ses concurrents directs.

**L'action SpaceX revient près de son prix d'introduction**

Le titre est remonté lundi vers ses **135 dollars** d'introduction, après un plus bas à 108,27 dollars et un premier trimestre publié au-dessus des attentes, à 7,81 milliards de dollars de revenus contre 6,93 milliards estimés. Deutsche Bank juge « très atteignable » l'objectif de 100 milliards de revenus annualisés d'ici la fin de l'année, en partie grâce à l'acquisition annoncée de Cursor.