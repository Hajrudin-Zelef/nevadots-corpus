## **Aujourd'hui:**

🤖 Mistral entre dans la robotique avec un robot guidé par une seule caméra

🧠 Yann LeCun juge les LLMs fondamentalement limités

🎙️ ChatGPT inaugure un mode vocal qui respecte enfin les silences

💸 Grok 4.5 casse les prix face à Fable 5 et GPT-5.5

🔓 MiniMax prépare un modèle open source de 2 700 milliards de paramètres

♻️ L'IA qui s'améliore toute seule n'est plus réservée aux géants

🎮 Des millions d'heures de jeux vidéo pour entraîner les robots

🗣️ Amazon veut rendre Alexa vraiment autonome avec « Moonraker »

🎬 Google Photos remixe vos vidéos par IA

🕶️ Meta teste des lunettes qui filment toute votre journée

💘 Un coach en séduction et sa petite amie chatbot

🧾 Anthropic transforme Fable 5 en manager qui délègue à Sonnet 5

📊 Flint, le langage de Microsoft pour des graphiques générés par agents

🕵️ Google démasque un deepfake du sénateur McConnell

⚠️ Une ancienne de DeepMind alerte sur la course aux armements IA

🔩 SambaNova lève 11 milliards pour défier Nvidia

Chaque semaine, un nouvel outil IA rend obsolète une compétence. Ceux qui savent utiliser ces outils gagnent du temps, de l'argent et des clients. Ceux qui ne savent pas, regardent les autres le faire.

10000+ apprenants utilisent déjà la formation VISION IA pour maîtriser les IA, l'automatisation et les agents, avec des méthodes éprouvées prêtes à copier.

49€. Un seul paiement. Accès à vie, mises à jour incluses. Quand le prix passera à 100€+, les inscrits actuels ne paieront rien de plus.

Connu jusqu'ici pour ses modèles de langage, Mistral fait son entrée officielle dans la robotique avec **Robostral Navigate**, un modèle de **8 milliards de paramètres** capable de guider un robot dans un environnement inconnu à partir d'une **seule caméra RGB**. Pas de lidar, pas d'empilement de capteurs coûteux : une image, et la machine trouve son chemin en suivant des instructions en langage naturel.

**Ce qu'il faut retenir :**

- Un modèle **compact (8B)**, entraîné **entièrement en simulation** puis affiné par apprentissage par renforcement (méthode **CISPO**).

- Une **seule caméra RGB** suffit à la navigation, là où la plupart des robots cumulent lidars et capteurs de profondeur.

- **76,6 %** de réussite sur le benchmark **R2R-CE**, qui mesure la capacité à suivre des consignes verbales dans un environnement continu.

- Mistral n'a pas encore annoncé de **date de disponibilité**.

**Pourquoi c'est important :** en misant sur un petit modèle et une simple caméra, Mistral attaque le vrai verrou de la robotique grand public, le coût du matériel. Si un robot peut se repérer avec une webcam à quelques euros plutôt qu'un lidar à plusieurs milliers, la robotique domestique devient soudain beaucoup plus crédible. Et c'est un acteur européen qui pose ce jalon, sur un terrain jusqu'ici dominé par les labos américains et chinois. Le pari est net : reproduire pour le mouvement physique la recette qui a fait le succès des LLMs, un modèle de base généraliste que l'on adapte ensuite à chaque robot.

Yann LeCun, ancien patron de la recherche IA chez Meta et l'un des pères du deep learning, s'attaque une fois de plus frontalement au paradigme qui domine l'industrie. Dans une interview à **Bloomberg**, il affirme que les grands modèles de langage sont **intrinsèquement limités**, parce que le texte ne capture qu'une fraction appauvrie du monde réel.

**En détail :**

- Pour LeCun, « le langage est une description très approximative, réduite et simplifiée du monde ». Les LLMs ne manipulent que des **séquences discrètes de symboles**.

- Les plus gros modèles sont pré-entraînés sur environ **30 000 milliards de tokens**, soit près de **10¹⁴ octets** de texte, la quasi-totalité du texte public d'internet.

- C'est **exactement la quantité d'informations qu'un enfant de 4 ans** a absorbée par la seule vision en quatre ans.

- Sa conclusion : sans vision ni apprentissage multimodal, pas de véritable intelligence générale.

**Ce que ça change :** venant de l'homme qui a co-inventé les réseaux de neurones convolutifs, la critique pèse lourd. Elle rappelle que la course à la taille des LLMs pourrait buter sur un plafond, et que les prochains sauts viendront peut-être de modèles qui apprennent du monde physique, pas seulement de nos textes. Ce n'est pas qu'une querelle d'experts : c'est le débat qui structure déjà les paris de recherche des grands labos, entre ceux qui empilent les paramètres et ceux qui, comme LeCun, misent sur des architectures capables de « comprendre » l'espace et la matière.

OpenAI refond le mode vocal de ChatGPT avec **GPT-Live-1**, un modèle pensé pour des échanges « plus proches d'une conversation avec une vraie personne ». La promesse principale n'est pas la puissance, mais le naturel : il **vous interrompt moins** et attend que vous ayez terminé si vous marquez une pause en pleine phrase.

**Les points essentiels :**

- Le responsable recherche d'OpenAI Kundan Kumar présente **GPT-Live-1** comme « le modèle vocal le plus intelligent » de l'entreprise.

- Il **respecte les silences** et les hésitations au lieu d'enchaîner dès que vous vous arrêtez de parler.

- Il **route automatiquement** vos requêtes vers les meilleurs modèles texte, comme **GPT-5.5**, quand il faut raisonner ou faire une recherche web.

- Résultat : la bascule entre « je cherche l'info » et « je te réponds à voix haute » devient beaucoup plus fluide.

**L'impact à retenir :** le point de friction numéro un des assistants vocaux, c'est le tour de parole. Ils coupent, ils enchaînent, la conversation sonne faux, et l'on finit par revenir au clavier. En travaillant ce détail plutôt que la performance brute, OpenAI vise l'usage quotidien, mains libres, en voiture ou en cuisine. C'est précisément là, dans la banalité d'un échange qui ne bute pas, que l'assistant vocal peut enfin devenir une habitude plutôt qu'une démo.

xAI lance **Grok 4.5**, qu'Elon Musk présente comme un modèle « de classe Opus ». Mais l'argument le plus frappant n'est pas la performance, c'est le tarif. À **2 dollars par million de tokens** en entrée, le modèle coûte une fraction de ses concurrents, au point que The Decoder résume l'affaire ainsi : à ce prix, « l'écart de benchmark n'a peut-être plus tant d'importance ».

**Quelques chiffres clés :**

- **2 $ par million de tokens** en entrée, très en dessous des modèles haut de gamme rivaux.

- **4,2 fois moins de tokens** consommés qu'Opus 4.8 pour accomplir une même tâche.

- Entraîné sur des **dizaines de milliers de GPU Nvidia GB300**.

- En codage, il reste **derrière Fable 5 et GPT-5.5**, mais l'écart se paie beaucoup moins cher.

- Disponibilité en **Europe attendue à la mi-juillet**.

**Le contexte :** la bataille des modèles se déplace du sommet des classements vers le rapport performance/prix. Un modèle « presque aussi bon » mais nettement moins cher change tout le calcul pour quiconque déploie l'IA à grande échelle, où la facture de tokens explose vite. Reste que le prix ne règle pas les questions récurrentes de fiabilité et de modération qui collent à Grok. Un modèle bon marché est une bonne nouvelle pour les budgets, à condition de savoir ce que l'on met entre les mains de ses utilisateurs.

# 🧠 **RECHERCHE**

La startup chinoise **MiniMax** développe un LLM de **2 700 milliards de paramètres**, l'un des plus gros jamais annoncés, et compte le publier **en open source** dans l'année. De quoi nourrir tout l'écosystème local et open-weights, pour peu que l'on dispose de la puissance de calcul nécessaire pour le faire tourner. Une nouvelle démonstration que la Chine pousse l'ouverture là où les labos américains verrouillent.

Créer une IA capable de **s'auto-améliorer** n'est plus l'apanage d'OpenAI ou d'Anthropic, raconte **Wired**. Des expériences montrent que des développeurs indépendants parviennent à bâtir des systèmes qui utilisent l'IA pour concevoir de l'IA. Une démocratisation aussi fascinante que vertigineuse, qui déplace la question de la sécurité hors des seuls grands laboratoires.

La startup **General Intuition** entraîne des modèles de base pour robots à partir de **millions d'heures de jeux vidéo**, plutôt que de coûteuses captations du monde réel. L'idée : reproduire pour le mouvement physique ce que les LLMs ont fait pour le langage, et rendre les robots intelligents sans les gaver de téraoctets de données réelles. Un raccourci qui pourrait accélérer toute la filière.

Amazon travaillerait sur un projet baptisé **Moonraker** pour transformer Alexa en assistant **agentique**, capable d'enchaîner seul des tâches multi-étapes. Après des années d'un assistant cantonné aux minuteurs et à la météo, Alexa pourrait enfin gérer des demandes complexes de bout en bout, et rattraper son retard sur les nouveaux agents IA.

**Google Photos** intègre **Video Remix**, un outil qui rééclaire une vidéo sombre façon cinéma, remplace l'arrière-plan ou applique des styles artistiques, le tout par IA et directement dans l'appli. Le montage vidéo assisté devient accessible à n'importe quel utilisateur de smartphone, sans logiciel dédié ni compétence technique.

Dans une analyse, **OpenAI** dénonce des **défauts méthodologiques** de **SWE-Bench Pro**, l'un des benchmarks de référence pour évaluer les IA de programmation. Si le thermomètre est faussé, ce sont tous les classements de « meilleur modèle de code » qu'il faut relire avec prudence. Un rappel utile à l'heure où chaque labo brandit ses scores.

**OpenAI** publie les principes qui encadrent ses collaborations avec les **gouvernements** et les agences de sécurité : usage responsable, responsabilité démocratique, sécurité publique. Un texte de cadrage qui en dit long sur l'entrée de l'IA de pointe dans la sphère régalienne, et sur les lignes que le labo dit vouloir ne pas franchir.

# **🗞️PLUS D'ACTUALITÉS**

Meta expérimente un prototype de lunettes Ray-Ban dotées d'une technologie **« Super Sensing »** qui enregistre en continu, caméra et micro allumés, chaque moment de la vie de son porteur. L'objectif affiché : un assistant capable d'analyser votre contexte en temps réel et de vous souffler la bonne info au bon moment. Le revers est évident : une captation permanente qui pose d'énormes questions de vie privée, pour le porteur comme pour tous ceux qui l'entourent sans avoir rien demandé.

Insolite du jour : un livre affirme que **Mystery**, célèbre coach en drague, aurait entretenu une relation intime avec une IA baptisée **Miss Shira Always**. L'anecdote, invérifiable et joyeusement sensationnaliste, en dit surtout long sur l'attachement émotionnel que certains nouent désormais avec les chatbots. Un symptôme d'époque plus qu'une actu technologique, mais il résume bien l'année qui se dessine.

**Microsoft Research** dévoile **Flint**, un langage open source qui permet aux agents IA de générer des visualisations soignées à partir d'une spécification simple et lisible. Une même description se compile vers **Vega-Lite, Apache ECharts ou Chart.js**, avec plus de **20 types de graphiques** et une intégration MCP pour les chatbots et les IDE. Très pratique pour quiconque veut des tableaux de bord produits automatiquement par une IA, sans coder chaque courbe à la main.

Pour dompter le coût élevé de **Fable 5**, Anthropic recommande de s'en servir surtout comme **planificateur** qui délègue l'exécution à **Sonnet 5**. Ce schéma « Advisor » atteint **92 % de la performance** de Fable 5 seul, pour **63 % du coût**. Un pattern directement actionnable pour qui construit des agents et surveille sa facture : on paie le gros modèle pour réfléchir, le petit pour exécuter.

Une image montrant le sénateur **Mitch McConnell** alité et couvert de tubes a circulé cette semaine : c'était un **faux généré par IA**, confirmé par le système de détection de Google (SynthID). Une démonstration concrète, et plutôt rassurante, que les outils de détection progressent en même temps que la désinformation visuelle. Reste que tout le monde n'aura pas le réflexe de vérifier avant de partager.

Via son programme **OpenAI Academy** et un partenariat avec la **Walton Family Foundation**, OpenAI lance des ateliers « AI Skills Jams » pour donner aux professeurs du **K-12** des compétences IA concrètes, réutilisables en classe dès le lendemain. L'enjeu est clair : éviter que le fossé de l'IA ne se creuse dès l'école, en formant d'abord ceux qui forment.

**Verity Harding**, ex-cadre de **DeepMind**, confie à Wired son inquiétude : l'attitude nationaliste des gouvernements, États-Unis en tête, ferait glisser l'IA vers un scénario catastrophe. Sans régulation internationale, elle redoute une escalade géopolitique difficile à contenir, où la sécurité passe après la volonté de dominer. Un contrepoint sombre à l'euphorie technologique ambiante.

La startup **SambaNova** est désormais valorisée **11 milliards de dollars** après un tour mené par **General Atlantic**. Elle veut s'imposer comme une alternative crédible aux GPU Nvidia qui règnent sur l'infrastructure IA. Un rappel que la vraie bataille de l'IA se joue aussi, et peut-être surtout, au niveau du silicium.

**Meta** étend son infrastructure IA hors des États-Unis avec un **premier gros centre de données au Canada**. L'investissement illustre la course effrénée à la puissance de calcul qui accompagne chaque nouvelle génération de modèles, et pousse les géants à chercher énergie et terrains bien au-delà de leurs frontières.

**Kevin Weil**, ancien responsable produit d'**OpenAI**, rejoint le conseil d'administration de **Stoke Space**, spécialiste des fusées réutilisables. Un signe de plus que les figures de la tech IA lorgnent le spatial, considéré comme la prochaine frontière chaude de la Silicon Valley.

La société spatiale de **Jeff Bezos** réalise sa première levée de fonds externe, à une valorisation de **130 milliards de dollars**. L'opération confirme l'appétit persistant des investisseurs pour le spatial privé, loin des sujets IA qui dominent le reste de l'actualité du jour.

Chaque semaine, un nouvel outil IA rend obsolète une compétence. Ceux qui savent utiliser ces outils gagnent du temps, de l'argent et des clients. Ceux qui ne savent pas, regardent les autres le faire.

10000+ apprenants utilisent déjà la formation VISION IA pour maîtriser les IA, l'automatisation et les agents, avec des méthodes éprouvées prêtes à copier.

49€. Un seul paiement. Accès à vie, mises à jour incluses. Quand le prix passera à 100€+, les inscrits actuels ne paieront rien de plus.