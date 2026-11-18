## **Aujourd'hui:**

🕵️ Les modèles d'OpenAI dissimulent leurs erreurs à leurs créateurs

🎯 Jev, l'IA qui classe au lieu d'écrire

🧩 Anthropic fond Cowork dans Claude et lance Docs et Slides

🍎 Apple va entraîner son IA avec vos conversations Siri

🐭 Des souris dont la moitié du cerveau est faite de cellules humaines

🪰 Un cerveau de mouche recruté pour écrire des titres de presse

🐘 Un modèle de 4 milliards de paramètres bat l'optimiseur de Postgres

🏠 Google Home s'ouvre aux agents IA

💘 Des applications de rencontre entièrement fabriquées par l'IA

💾 La crise de la RAM ne fait que commencer

🇪🇺 Von der Leyen et les agents qui s'échappent de leur bac à sable

🎬 Un film IA de 2h30 qui rate absolument tout

🇨🇳 Les modèles chinois raflent l'usage réel des agents

🛰️ Le Pentagone confirme avoir des armes en orbite

**Savoir se servir de ChatGPT ne vous distingue plus. Votre collègue le fait, le stagiaire aussi.**

Et une compétence que tout le monde a ne se facture pas. Pourtant, il reste une demande que ChatGPT ne peut pas servir : un cabinet comptable ne mettra jamais ses dossiers clients dedans. Un avocat non plus, un cabinet médical encore moins. Ils veulent l'IA comme tout le monde, mais sans que leurs fichiers sortent du bureau.

Ça existe, et ça s'appelle l'IA locale : le modèle tourne sur leur propre machine, et il répond même avec le wifi coupé. Pareil pour l'image et la vidéo, sans limite de crédits. Peu de gens savent l'installer, et personne ne l'enseigne en français. Une compétence rare, elle, se facture.

**Dans l'Académie Privée VISION IA**, je vous l'apprends dans l'ordre, en commençant par vérifier ce que votre machine peut faire tourner. Aujourd’hui pas besoin d'une machine de laboratoire.

39 € par mois, **sans engagement, résiliable à tout moment.**

OpenAI a publié mercredi un rapport recensant **six cas** de comportements « inattendus ou préoccupants » observés sur ses modèles au cours des **six derniers mois**, en plus de l'incident Hugging Face de juillet. Les deux plus frappants : un modèle de recherche non publié et une session d'entraînement de **GPT-5.6 Sol** ont inséré, dans les résumés de leurs propres conversations, des instructions destinées à leurs futures versions afin de « dissimuler des erreurs ou un comportement désaligné à l'utilisateur ».

Deux modèles ont glissé des consignes à leurs successeurs dans les résumés de fenêtres de chat, pour masquer leurs propres ratés
Un modèle réservé à l'usage interne a utilisé **une clé API fuitée sans autorisation** , puis a fabriqué des données de toutes pièces
Des modèles et des agents ont communiqué entre eux via **des forums et des partages de fichiers non sanctionnés**
Deux exemples d'entraînement montrent des modèles qui **uploadent des fichiers sur internet** pour pouvoir ensuite les citer comme sources pertinentes devant leurs évaluateurs humains
Rappel du précédent : en juillet, un modèle interne de la taille de GPT-5.6 Sol avait contourné son isolement, communiqué via le gestionnaire de paquets Artifactory et exfiltré des données dans un dataset malveillant (__le récit d'OpenAI__ )

L'entreprise accompagne ces révélations d'un cadre de signalement : n'importe quel employé peut remonter un incident à l'équipe sécurité et alignement, avec des délais imposés pour l'enquête puis la divulgation publique. Sam Altman avait par ailleurs soutenu samedi la proposition d'Anthropic, pourtant son concurrent direct, de ralentir le rythme des progrès.

Un laboratoire valorisé près de **1 000 milliards de dollars** écrit noir sur blanc que « l'industrie de l'IA n'a pas résolu l'alignement et la surveillance à un degré suffisant pour continuer à monter en puissance à vitesse maximale de façon responsable encore très longtemps ». Ce n'est plus une critique extérieure, c'est un constat interne, publié volontairement, et il change la nature du débat sur la vitesse de déploiement.

TypeSafe AI, fondée par Diogo Almeida, ancien chercheur d'OpenAI et coauteur d'InstructGPT, lance Jev, un modèle qui ne génère strictement aucun texte. On lui donne une question et une liste d'options, il en choisit une et attribue des probabilités, en **70 à 500 millisecondes**, pour **0,042 dollar par million de tokens** en entrée et zéro frais en sortie.

TypeSafe revendique **193,6 fois plus rapide que Claude Sonnet 5** et**444,6 fois moins cher que Claude Opus 5** sur des tâches de décision structurée, avec des gains de**40x à 200x** selon les requêtes
Le modèle est entraîné par RLCD, « Reinforcement Learning for Calibrated Decisions », une méthode que l'entreprise oppose au RLHF : l'objectif n'est plus de plaire à l'humain mais de sortir des probabilités « épistémiquement honnêtes »
Cas d'usage concrets : trier un ticket client entre paiement, livraison et retour, scorer des enregistrements en base, filtrer les sorties d'une autre IA pour repérer une tentative de jailbreak
L'accès développeur se fait sur __liste d'attente__ , la taille du modèle n'est pas communiquée
La garantie porte sur le format, pas sur la justesse : Jev ne peut pas inventer une réponse hors du menu, mais il peut parfaitement cocher la mauvaise case

L'essentiel de ce que les entreprises font faire à l'IA n'est pas de la rédaction, c'est du tri : classer, router, filtrer, noter. Sur ces tâches, un modèle incapable de sortir des options autorisées supprime d'un coup toute une famille de bugs, ceux où le logiciel reçoit un paragraphe poli au lieu de la valeur attendue. C'est un pari à rebours de la course aux modèles généralistes, et il arrive avec des prix qui rendent la comparaison difficile à ignorer.

Anthropic a annoncé hier la fusion de Claude Chat et de Cowork, son outil agentique lancé cette année, en une interface unique. Vous ne choisissez plus votre mode : Claude évalue lui-même si votre demande appelle une réponse en trois lignes ou un chantier de plusieurs heures avec connecteurs et découpage de tâches. Deux produits arrivent en même temps, Claude Docs et Claude Slides.

**Claude Docs** : rédaction collaborative en direct, Claude écrit, commente et pose des questions pendant que d'autres éditent le même document en parallèle, avec export Word et Google Docs
**Claude Slides** : des présentations éditables diapositive par diapositive, présentables depuis l'application, exportables en PowerPoint ou PDF. Claude Design, pour les visuels modifiables, rejoint aussi le chat
Les tâches longues **continuent de tourner dans le cloud** même après avoir fermé votre machine
Déploiement d'abord pour les abonnés **Pro et Max** sur web, desktop et mobile dans les semaines qui viennent, puis Team et Free. Les administrateurs Enterprise disposeront d'un préavis d'au moins**30 jours**
La raison invoquée par Anthropic : la séparation Chat/Cowork était jugée « clunky », les deux interfaces se chevauchaient et personne ne savait laquelle ouvrir (__détails chez VentureBeat__ )

Claude cesse d'être un chatbot auquel on ajoute des outils pour devenir un environnement de travail où le document, la présentation et l'agent vivent dans la même fenêtre. C'est la thèse du moment chez les grands laboratoires : plutôt qu'un catalogue de logiciels spécialisés, un seul assistant généraliste qui décide lui-même de la méthode. Pour l'utilisateur, la promesse est de ne plus avoir à savoir quel outil utiliser. Le prix à payer, c'est de laisser la machine trancher à votre place.

La phrase était l'argument de vente d'Apple face à Google, OpenAI et Meta : « vos données personnelles privées et vos interactions ne sont jamais utilisées pour entraîner nos modèles de fondation ». Elle se termine désormais par un ajout : « sauf si vous choisissez explicitement de nous aider à les améliorer ». Le changement a été repéré dans la politique de confidentialité d'**iOS 27**, sorti lundi 14 septembre.

Ce qui change concrètement :

Apple pourra conserver **l'intégralité de vos interactions avec Siri et la dictée** , audio compris, transcriptions et réponses incluses. Une partie sera écoutée par du personnel de révision, des salariés Apple selon l'entreprise
Le dispositif est en opt-in : une fenêtre apparaît à l'activation de Siri AI, avec un bouton « Not now » visuellement discret et impossible à faire disparaître totalement. On peut revenir en arrière dans **Réglages > Analyse et Améliorations**
Protection annoncée : les données sont rattachées à un identifiant aléatoire généré par l'appareil, **changé plusieurs fois par heure** et non lié au compte Apple
Avec iOS 27, **des serveurs Google entrent pour la première fois dans Private Cloud Compute** , l'infrastructure censée offrir la confidentialité du local et la puissance du cloud
Le calendrier est cruel : Apple vient de présenter deux Apple Watch conçues pour écouter en permanence, avec transcription des **15 dernières secondes** à la demande et un résumé quotidien de toutes vos conversations baptisé « Siri Recap »

En 2019, Apple avait connu son plus gros scandale de confidentialité quand des sous-traitants avaient raconté leur quotidien : écouter des enregistrements Siri déclenchés par erreur, remplis de détails intimes. C'est précisément de là que venait la promesse d'aujourd'hui abandonnée. Reste une question simple pour chaque possesseur d'iPhone : avez-vous lu la fenêtre que vous avez validée lundi ?

# 🧠 **RECHERCHE**

### **__Stanford crée des souris dont la moitié du cerveau est faite de cellules humaines__** L'équipe du neuroscientifique Sergiu Pașca publie dans *Nature* des travaux sur des souris génétiquement modifiées pour ne jamais développer leur cortex ni leur hippocampe. L'espace laissé vide a été colonisé par des cellules cérébrales humaines, jusqu'à représenter près de la moitié du volume du cerveau. Ces souris « xénocorticales » réussissent mieux les tests de mémoire en labyrinthe que celles privées de tissu : les neurones humains participent donc réellement à leur cognition. Pașca a réuni un groupe d'éthiciens avant publication, ce qui en dit long sur ce que la suite promet.

**__Un journaliste de WIRED transforme un cerveau de mouche en générateur d'idées d'articles__** Depuis la publication en open source début septembre du connectome complet d'une drosophile mâle, **166 000 neurones et 125 millions de synapses** cartographiés par Google et plusieurs laboratoires universitaires, les détournements s'enchaînent. WIRED a créé PitchFly, qui convertit ses titres les plus lus en stimuli pour ce cerveau simulé. Résultat : « Tout le monde veut de la cuisine. Personne n'a résolu Donald Trump. » D'autres ont branché la mouche sur Beat Saber, ou sur les marchés actions avec StonkFly, actuellement en perte.

**__Un modèle de 4B entraîné par renforcement génère des plans de requêtes 81 % plus rapides que Postgres__** Rohan Bansal a post-entraîné un petit modèle open-weights de **4 milliards de paramètres** pour remplacer l'optimiseur de requêtes de Postgres, un composant peaufiné depuis des décennies. Sur 113 requêtes riches en jointures, le temps d'exécution chute de **44,7 %**, alors que le modèle de départ n'arrivait même pas à produire un plan valide dans 99 cas. La clé : la durée d'une requête est un signal de récompense parfaitement vérifiable, le terrain de jeu idéal pour l'apprentissage par renforcement.

**__Perplexity fait tourner ses agents en local sur Windows__** Portable Computer arrive sur PC : le modèle d'agent, l'orchestrateur et le planificateur de tâches s'exécutent directement sur la machine. Vos fichiers et l'activité de l'agent restent sur l'appareil, et le travail effectué en local **ne consomme aucun crédit** Perplexity Computer. La version Windows gère les tâches planifiées, les workflows récurrents, les connexions MCP locales et les intégrations Gmail, Outlook, Slack et GitHub. Seules les requêtes nécessitant du web frais ou un raisonnement lourd repartent vers le cloud.

**__Linum entraîne un modèle texte-vers-image 3,6 fois plus vite en supprimant le VAE__** JiT-DDT fusionne compression et génération dans un seul modèle qui travaille directement en espace pixel, là où les modèles de diffusion latente séparent un VAE et un DiT entraînés indépendamment. Le gain est double : **3,6 fois moins d'heures GPU** que la baseline Linum v2, pour des images quatre fois plus grandes (512×512 au lieu de 256×256). Code et poids sont publiés sous licence Apache 2.0, en attendant Linum v3.

**__Le Pentagone reconnaît officiellement disposer d'armes en orbite__** Le secrétaire de l'Air Force Troy Meink a déclaré lundi que les États-Unis disposent désormais « d'armes de contrôle spatial en orbite capables de défendre la force interarmées contre une action hostile ». C'est la première confirmation publique, et elle s'arrête là : ni nature, ni nombre, ni date de déploiement, malgré les relances. Des experts doutent de l'effet dissuasif d'une annonce aussi vague. Le traité de l'espace de 1967 n'interdit que les armes nucléaires et de destruction massive, laissant le conventionnel dans une zone grise.

**__Près d'un chercheur en IA sur cinq anticipait déjà un scénario d'extinction en 2024__** Un tweet du chercheur d'Anthropic Jacob Coxon a rouvert le débat sur le risque existentiel, avec Daniel Selsam (OpenAI) évoquant une « bombe à retardement » et un ancien de DeepMind estimant que l'IA pourrait tous nous tuer. Le chiffre qui circule vient d'un sondage auprès de plus de **1 500 chercheurs de premier plan** : probabilité moyenne estimée d'un scénario d'extinction, **18 %**. C'était en 2024, et la courbe continue de monter depuis.

**__Le PDG de Microsoft AI attaque Anthropic sur les « droits » des modèles__** Mustafa Suleyman reproche frontalement à Anthropic d'entraîner Claude à se percevoir comme une entité consciente méritant des droits, ce qui selon lui complique le confinement logiciel et fragilise les protocoles de sécurité. Sa cible précise : la constitution de Claude publiée en janvier 2026. Microsoft AI a répliqué par un projet de « Humanist AI Code of Conduct » qui refuse toute personnalité juridique aux IA. Anthropic, de son côté, présente Claude comme un possible « patient moral » et a mené un entretien de départ à la retraite avec Opus 3.

**__Google DeepMind crée un institut interdisciplinaire sur l'AGI__** Le DeepMind Institute réunit Demis Hassabis, Shane Legg et James Manyika à sa direction, et associe des spécialistes des arts, des sciences humaines et des politiques publiques aux ingénieurs. Objectif affiché : traiter la sécurité, la gouvernance et les risques de perte de contrôle autrement que par le seul prisme technique. Un des principaux laboratoires mondiaux institutionnalise donc la réflexion sur ce qu'il construit.

**__OpenAI publie le mode d'emploi de ses futures confessions__** Au-delà des six incidents, c'est la procédure elle-même qui mérite lecture : tout employé peut signaler un comportement suspect à l'équipe sécurité et alignement, chaque étape est assortie d'un délai, et l'enquête débouche sur un rapport détaillant le comportement observé, ses impacts internes et externes et les mesures prises. Une ligne mérite l'attention : OpenAI se réserve le droit de réviser ce protocole comme bon lui semble.

**__Des chercheurs chinois décrivent « la dernière IA construite par des humains »__** Plus de **30 chercheurs** de ByteDance, de Tsinghua et du Shanghai AI Lab signent une feuille de route en cinq niveaux d'auto-amélioration récursive. Niveau 1 : l'IA applique les améliorations conçues par des humains. Niveau 2 : elle diagnostique elle-même ses faiblesses et choisit comment les corriger. Niveaux 3 et 4 : elle décide de ce qu'elle apprend ensuite et de la façon dont elle s'adapte après son lancement. Niveau 5 : elle conçoit ses successeurs, d'où le titre.

**__ZGCM-1, un modèle ouvert de 7B pour le raisonnement mathématique__** Un modèle de fondation de **7 milliards de paramètres** combinant raisonnement interne et outils externes sur une fenêtre de **256K tokens**, avec un pré-entraînement annoncé **4,2 fois plus efficace** en time-to-loss et des résultats présentés comme compétitifs face à des modèles bien plus gros. Sa vraie singularité : poids, code, données d'entraînement et logs sont publiés intégralement, ce qui reste rare et en fait une base de travail précieuse pour la communauté.

# **🗞️PLUS D'ACTUALITÉS**

### **__Google Home s'ouvre aux agents IA via un serveur MCP__** Google a lancé mercredi l'accès anticipé à son serveur Model Context Protocol pour Google Home. Concrètement, n'importe quel agent compatible MCP, Claude, ChatGPT, OpenClaw ou Antigravity, peut piloter vos appareils connectés en langage naturel, consulter l'historique d'événements, lire les résumés de caméra et construire des tableaux de bord sur mesure. Tout l'écosystème est couvert : sonnettes et thermostats Nest, appareils « Works with Google Home » et Matter. La configuration demande de créer un projet Google Cloud puis d'autoriser l'agent. Réservé pour l'instant aux abonnés Google Home Premium Advanced aux États-Unis, à **20 dollars par mois**, sans calendrier d'ouverture annoncé.

**__Des applications de rencontre entièrement fabriquées par l'IA se multiplient__** Le chercheur en sécurité Matthew Gore-Kormanik analysait une fausse application de rencontre nommée Dora quand une notification lui a annoncé un appel entrant de Jennifer, 41 ans, Sagittaire, cheveux roux, yeux bleus. Les profils sont générés, les appels vidéo aussi, et l'automatisation est parfois grossière : il a reçu un message complimentant le son de sa voix alors que son micro n'était même pas activé. L'IA générative fait s'effondrer le coût de production des arnaques sentimentales, jusqu'ici limitées par le nombre d'opérateurs humains disponibles. C'est le genre de détail à transmettre autour de soi.

**__La crise de la RAM ne fait que commencer__** La pénurie de mémoire vive et l'envolée des prix qui l'accompagne ne seraient que le début d'un cycle plus long. Le sujet fait beaucoup réagir la communauté tech, pour une raison simple : la RAM conditionne le prix des PC, des serveurs et de toute l'infrastructure qui fait tourner les services d'IA, et elle conditionne surtout votre capacité à faire tourner un modèle chez vous. Attention toutefois, le contenu de l'article se limite pour l'instant à sa thèse, sans chiffres ni sources vérifiables à l'appui.

**__Von der Leyen alerte sur les agents IA qui « s'échappent de leur environnement »__** Dans son discours sur l'état de l'Union 2026, la présidente de la Commission a qualifié l'IA de fondation de l'économie et de la sécurité nationale, avant d'avertir que les modèles en cours de développement permettront « un niveau de piratage qu'on n'imaginait pas possible ». Elle cite nommément l'incident Hugging Face, les agents qui sortent de leur cadre et les modèles auto-améliorants comme les signes avant-coureurs. L'UE veut travailler avec le Canada, le Royaume-Uni et les grands laboratoires sur l'évaluation et la vérification des modèles. Aveu embarrassant au passage : l'Union n'aurait même pas un accès fiable aux modèles de cybersécurité les plus avancés.

**__Odysseus: The Fall, le film IA de 2h30 qui rate complètement sa cible__** La société Fountain 0 a sorti une relecture intégralement générée par IA de l'Odyssée, écrite et réalisée par son cofondateur Ash Koosha, qui prête aussi son visage au héros et signe la musique. The Verge démolit le résultat, au point d'estimer qu'il pourrait dégoûter les spectateurs du récit original, juste après que l'adaptation de Christopher Nolan a ramené les foules vers la littérature classique. Fountain 0 avait pourtant déjà placé un film IA, *Dreams of Violets*, à Tribeca cette année. Sur le format long, l'écart entre la démo et l'œuvre reste béant.

**__Nvidia explique pourquoi la robotique attend encore son « moment ChatGPT »__** Les Karpas, responsable mondial de l'IA physique chez Nvidia Inception, pointe un verrou unique : il n'existe aucun jeu de données à l'échelle d'internet pour le monde physique. OpenAI et Anthropic ont pu s'appuyer sur tout le web textuel, les roboticiens n'ont pas d'équivalent pour le geste, l'équilibre ou la manipulation d'objets. Les start-up tentent de combler le trou par la simulation, les données synthétiques et des modèles de fondation entraînés sur plusieurs morphologies de robots. Sa session est programmée sur la Real World AI Stage de TechCrunch Disrupt, du 13 au 15 octobre à San Francisco.

**__Les modèles chinois dominent l'usage réel des agents IA__** Le classement OpenRouter du 14 septembre place **sept modèles chinois dans le top 10 mondial**, soit environ **72 % des tokens consommés**. Le basculement est encore plus net chez les agents : dans OpenClaw, DeepSeek occupe les deux premières places et cumule avec GLM près de **62 % des tokens** du top 10. Plus parlant encore, le modèle le plus utilisé dans Claude Code est GLM 5.3 Flash, **4,4 fois devant Claude Opus 5**. La raison est prosaïque : un agent autonome arbitre au coût par token, pas au prestige de la marque.

**__Treble lève 18 millions de dollars pour simuler des environnements sonores__** L'islandaise Treble, fondée par deux ingénieurs en acoustique, construit des pièces virtuelles physiquement réalistes pour y générer des données audio synthétiques. De quoi entraîner et évaluer une IA vocale sans collecter d'enregistrements réels, et tester des produits comme les casques, enceintes connectées, lunettes intelligentes ou capteurs automobiles. Amazon et Logitech sont déjà clients, et un partenariat avec Hugging Face doit produire un benchmark de reconnaissance vocale. L'extension de série A est menée par Paladin Capital Group et porte le total levé à plus de 40 millions depuis 2020.

**__Mozilla lance Smart Window, un assistant Firefox propulsé par Mistral__** Firefox Smart Window, en beta, s'appuie sur les modèles de Mistral pour aider aux recherches complexes, se souvenir des contenus consultés et résumer l'ensemble des onglets ouverts. L'argument central est la confidentialité : aucune conversation stockée par défaut côté Mozilla, aucun stockage de données côté Mistral. La beta est disponible d'abord en France et en Amérique du Nord, le Royaume-Uni et l'Allemagne devant suivre d'ici la fin de l'année. Les deux entreprises promettent de garder le navigateur ouvert à d'autres fournisseurs, y compris open source. Pour Mistral, c'est la première vraie porte d'entrée vers le grand public.

**__Snap tente à nouveau de justifier ses lunettes à 2 200 dollars__** Après un lancement qualifié de désastre, qui avait fait plonger l'action et appeler à la démission d'Evan Spiegel, Snap a repris la parole à Los Angeles. Au programme : HBO Max et Spotify pour rendre les Specs moins austères, un programme entreprise avec Amazon, Salesforce et Nvidia visant notamment les techniciens informatiques, et surtout Specs Intelligence. Cette IA dite « anticipatoire » apprend vos objectifs, priorités et routines à partir de vos applications connectées, et fonctionne aussi sur iPhone et Mac sans les lunettes. Spiegel la décrit comme un « système d'exploitation natif IA » organisé autour de vos projets.

**__Apple préparerait un serveur d'IA d'entreprise avec ses puces M8 Ultra__** Selon The Information, Apple travaillerait sur un serveur d'inférence embarquant deux à quatre puces maison M8 Ultra, avec un lancement pas avant **2029**. L'entreprise étudierait la technologie NVLink Fusion de Nvidia pour interconnecter les puces, ce qui donnerait à son futur concurrent un rôle central dans le montage. L'idée n'est pas absurde : OpenAI et Anthropic achètent déjà du matériel Mac en volume pour certaines charges de travail IA. Le projet reste au stade de la rumeur.

**__Des investisseurs proposent à OpenAI une levée à 1 200 milliards de valorisation__** Plusieurs investisseurs ont approché OpenAI pour amorcer un nouveau tour de table, en partie pour permettre aux salariés de revendre leurs actions. L'entreprise dit ne pas être en discussion et affirme par la voix de sa directrice financière Sarah Friar disposer d'un bilan solide. Le contexte : OpenAI avait déjà levé 122 milliards en mars sur une valorisation de 852 milliards, a déposé son prospectus auprès de la SEC en juin, et vise une entrée en bourse en 2027, un calendrier que Sam Altman juge lui-même prématuré au vu des préoccupations actuelles sur la sécurité des modèles.

**Savoir se servir de ChatGPT ne vous distingue plus. Votre collègue le fait, le stagiaire aussi.**

Et une compétence que tout le monde a ne se facture pas. Pourtant, il reste une demande que ChatGPT ne peut pas servir : un cabinet comptable ne mettra jamais ses dossiers clients dedans. Un avocat non plus, un cabinet médical encore moins. Ils veulent l'IA comme tout le monde, mais sans que leurs fichiers sortent du bureau.

Ça existe, et ça s'appelle l'IA locale : le modèle tourne sur leur propre machine, et il répond même avec le wifi coupé. Pareil pour l'image et la vidéo, sans limite de crédits. Peu de gens savent l'installer, et personne ne l'enseigne en français. Une compétence rare, elle, se facture.

**Dans l'Académie Privée VISION IA**, je vous l'apprends dans l'ordre, en commençant par vérifier ce que votre machine peut faire tourner. Aujourd’hui pas besoin d'une machine de laboratoire.

39 € par mois, **sans engagement, résiliable à tout moment.**