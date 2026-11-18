Sundar Pichai a confirmé ce matin que **Koray Kavukcuoglu** prend la tête de Google DeepMind avec le titre de Senior Vice President, en reportant directement au PDG d'Alphabet. Demis Hassabis, cofondateur et patron historique, quitte la direction opérationnelle pour devenir président de DeepMind et Chief Scientist d'Alphabet. Derrière le communiqué policé, c'est un laboratoire sous tension qui change de main.

Kavukcuoglu n'est pas un parachuté : arrivé en 2012, deux ans avant le rachat par Google, docteur en informatique de NYU sous la direction de Yann LeCun, il a contribué à DQN, AlphaGo et WaveNet. Il supervise désormais les modèles Gemini, la recherche frontière, l'app Gemini et les plateformes développeurs.

Google n'a pas sorti de modèle frontière depuis **Gemini 3.1 Pro en février** , pendant qu'Anthropic (Mythos) et OpenAI (GPT-5.6) ont pris l'avantage
**Gemini 3.5 Pro a raté trois dates de sortie** : juin, mi-juillet, puis août, et Gemini 3.6 Flash est jugé derrière OpenAI, Anthropic, xAI, Meta et plusieurs labos chinois sur les benchmarks d'intelligence
L'hémorragie de talents est spectaculaire : Jeff Dean part après **27 ans** chez Google monter sa propre boîte, Noam Shazeer (co-lead de Gemini) est passé chez OpenAI en juin, et le prix Nobel John Jumper est parti chez Anthropic avec Jonas Adler et Alexander Pritzel, trois des artisans d'AlphaFold
Climat interne dégradé selon Fortune : semaines de 60 heures, moral bas, et un mouvement syndical lancé en mai
Une équipe interne baptisée **Code Strike** a été montée pour rattraper le retard sur le code, terrain où Morningstar juge Anthropic et OpenAI « à des kilomètres devant »

L'audience n'est pas le problème : l'app Gemini vient de franchir le milliard d'utilisateurs mensuels et les modèles Gemma cumulent plus de 900 millions de téléchargements. Le problème est la frontière technologique, et les analystes lisent cette nomination comme un recentrage : moins de projets académiques et de world models chers à Hassabis, plus de LLM et d'outils pour développeurs. Pour vous, rien à tester aujourd'hui, mais le prochain Gemini dira si le virage a fonctionné.

Alibaba a publié **Qwen-MM-Plugins**, un dépôt open source sous licence Apache-2.0 qui ajoute des capacités multimodales aux agents de code que vous utilisez déjà. L'idée est maligne : plutôt que de vous forcer à adopter Qwen Code, Qwen transforme sa vision et son audio en une **couche d'outils réutilisable par n'importe quel agent**.

Une fois installé, votre agent gagne des fonctions qu'il découvre et enchaîne tout seul : lire une image ou une vidéo, faire de l'OCR, localiser un objet dans une photo, segmenter, transcrire de l'audio, recadrer. Vous pointez un fichier avec `@` et vous demandez en français ce que vous voulez.

**8 modules installables séparément** : core (images, vidéos, documents, fichiers 3D), api (vision, OCR, grounding, transcription, segmentation via les modèles Qwen VL et Omni), search (recherche web, extraction de pages, recherche inversée d'image), video-memory (mémoire hiérarchique pour vidéos longues), video-edit (génération et montage image, vidéo, audio), blender (modélisation et rendu 3D), freecad (CAO paramétrique, formats STEP, STL, FEM) et edu-agent
**Six agents compatibles** : Claude Code, Codex, Qoder, OpenClaw, Qwen Code et Gemini CLI, avec un fichier de configuration partagé
Un seul script fait tout : `curl -fsSL https://raw.githubusercontent.com/QwenLM/Qwen-MM-Plugins/main/install.sh | bash` installe, configure, vérifie et désinstalle
**Le module core ne demande aucune clé API.** Les autres réclament une clé DashScope, et Serper pour la recherche web
Gratuit à installer, vous ne payez que les appels d'API tiers que vous déclenchez

C'est le genre de brique qui change la nature d'un outil sans changer d'outil. Un agent de code qui savait seulement lire du texte peut désormais ouvrir une facture scannée, décrire une vidéo, sortir un rendu Blender ou manipuler un plan CAO. Et pendant qu'OpenAI et Anthropic vendent des modèles multimodaux fermés, Qwen distribue gratuitement de quoi rendre leurs propres agents voyants.

Depuis le 11 août, les **Meta Glasses sont interdites dans tous les tribunaux d'Angleterre et du Pays de Galles**. Le His Majesty's Courts & Tribunals Service, qui gère l'ensemble des juridictions pénales, civiles et familiales, les récupère à l'entrée et les rend à la sortie. Détail savoureux : le smartphone, lui, reste toléré, tant qu'il ne filme pas.

La raison est évidente une fois posée. Filmer sans autorisation dans un tribunal est un outrage à la cour, passible de poursuites. Or on ne colle pas discrètement un téléphone sur son visage, alors qu'une paire de lunettes à caméra intégrée permet d'enregistrer une audience entière sans que personne ne s'en rende compte.

**7 millions de paires** vendues en 2025, avec une caméra 12 mégapixels, de la vidéo jusqu'en 3K à 30 images par seconde, à partir de**299 dollars**
L'État de New York a devancé Londres : interdiction des lunettes connectées (Meta, Xreal, RayNeo) dans ses **1 240 tribunaux depuis le 20 juillet** , pour empêcher l'identification secrète de jurés et de témoins
La faille qui inquiète les institutions : la LED censée signaler l'enregistrement peut être neutralisée par un mod à 60 dollars, documenté par 404 Media
Le déclencheur remonte à février : lors d'un procès en Californie, un juge a dû menacer l'entourage de Mark Zuckerberg d'outrage à la cour pour qu'il retire ses lunettes
Un tribunal londonien avait déjà eu affaire aux lunettes connectées, mais pour un tout autre usage : un homme est accusé d'avoir été soufflé ses réponses en contre-interrogatoire via les haut-parleurs intégrés

Le rejet dépasse largement les prétoires : la chaîne de pubs Wetherspoons, des restaurants, des théâtres, des salles de concert et des conventions britanniques les bannissent aussi, après une vague de vidéos de personnes filmées sans leur accord et parfois identifiées en ligne. Meta a refusé de commenter. Quand un produit se fait confisquer à l'entrée des tribunaux d'un pays entier, ce n'est plus un problème d'image, c'est un problème de conception.

# 🧠 **RECHERCHE**

**L'IA n'est plus artificielle : des cerveaux cultivés en labo pour remplacer les LLM**

Des biologistes fabriquent, à partir de cellules souches, des organoïdes cérébraux humains : des amas de quelques millions de neurones vivants qui émettent de vraies ondes cérébrales après huit mois de culture. À UC San Diego, ces amas guident des robots dans des labyrinthes ; à Johns Hopkins, ils servent de base à du biocomputing ; une startup de Melbourne leur fait jouer à Pong et à Doom. Le pari : une intelligence vivante plutôt qu'artificielle, en alternative frontale aux grands modèles de langage.

**AMIE, l'IA médicale de Google, mène des consultations vidéo en temps réel**

Google Research et Google DeepMind font passer AMIE du chat texte à la visio. Construit sur Gemini et Project Astra avec une architecture multi-agents, le système interprète les signaux visuels et sonores, guide un examen physique à distance et raisonne sur le diagnostic en direct. Dans une étude randomisée avec des acteurs patients et des généralistes, AMIE est bien évalué sur l'anamnèse, la précision diagnostique et la qualité de communication, et les patients simulés ont préféré la vidéo au texte. Ça reste un système de recherche, non déployé cliniquement.

**Une nouvelle technique révèle les pensées cachées des modèles d'IA**

Des chercheurs de l'université de Tübingen, du Max Planck et de MATS ont trouvé comment extraire, via une simple API, le raisonnement interne que les modèles frontière sont censés garder pour eux. « Tous les grands fournisseurs que nous avons testés partagent cette vulnérabilité », résume Alexander Panfilov : la faille permettait de récupérer des mots de passe et des clés d'API glissés dans le raisonnement, elle a depuis été corrigée. Effet de bord explosif : le raisonnement de Kimi K3, de Moonshot AI, ressemble étrangement à celui de Claude Opus 4.8 et de GPT-5.6, contrairement à DeepSeek. Un indice de distillation, pas une preuve.

**CARE-X, le modèle de Microsoft qui lit les radios du thorax**

Microsoft Research présente un modèle vision-langage unifié qui interprète les radiographies thoraciques en combinant rédaction de compte rendu en texte libre et prédictions structurées vérifiables. Bâti sur Qwen3-VL-4B-Instruct, il est affiné par apprentissage par renforcement (DAPO) pour récompenser la justesse clinique plutôt que la fluidité du texte, et sait appeler des outils de mesure pour calculer un ratio cardiothoracique. Validation sur des données réelles de l'hôpital Narayana Health en Inde, y compris des pathologies rares en soins intensifs. Modèle de recherche, sans approbation réglementaire.

**Un LLM local peut-il faire tourner mon assistant IA ?**

L'expérience que beaucoup ont en tête sans jamais la mener : l'auteur rejoue **27 tâches réelles de production** à travers deux modèles locaux, séparés par une génération de matériel, pour savoir ce qu'il faut vraiment pour remplacer Claude comme cerveau d'un agent personnel qui pilote **90 outils**. Le verdict est nuancé et détaillé, et c'est surtout un excellent inventaire de ce qui casse quand on rapatrie son assistant à la maison.

**L'IA s'empare des mathématiques**

OpenAI affirme avoir produit les solutions de **dix problèmes mathématiques non résolus depuis des décennies**. James Maynard, professeur à Oxford et médaille Fields, raconte à The Verge une année de remise en question devant une discipline traditionnellement lente qui se met soudain à courir. Le parallèle avec AlphaFold en biologie s'impose, avec la même question au bout : que reste-t-il du métier quand la machine trouve la démonstration ?

**Mémoire agentique : ALTK-Evolve fait aussi bien qu'ACE avec moins de tokens**

IBM Research compare son système ALTK-Evolve à ACE (Agentic Context Engineering). Les deux font apprendre un agent de ses propres échecs passés sans toucher aux poids du modèle : quand un agent rate une tâche multi-étapes, ce n'est presque jamais par manque de connaissances, c'est qu'il gère mal la pagination d'une API ou résout la mauvaise entité. Les deux refusent de compresser les leçons en résumés génériques et comptent plutôt les occurrences. La vraie différence se joue sur la consolidation et la livraison de cette mémoire, donc sur la facture en tokens.

**L'IPO géante d'Anthropic se heurte au scepticisme des investisseurs**

Selon le Wall Street Journal, Anthropic prépare une entrée en bourse pour septembre ou octobre, potentiellement la plus grosse jamais réalisée, sur une valorisation de 965 milliards de dollars. En réunion, les investisseurs posent des questions rugueuses : la concurrence chinoise, les tensions avec l'administration Trump, les protestations locales contre les data centers. Le prix qui sortira de cette IPO servira de référence pour valoriser toute l'industrie.

**Metis : un modèle de fondation avec une mémoire persistante intégrée**

Et si la mémoire était une capacité du modèle lui-même, plutôt qu'un système de récupération bricolé autour ? Metis propose un état mémoire persistant logé dans le backbone, mis à jour lors des passes avant ordinaires, pendant que les poids appris restent gelés. Au lieu de stocker les échanges sous forme de texte à retrouver plus tard, le modèle compresse les interactions passées directement dans cet état. De quoi se souvenir d'une conversation d'il y a trois semaines sans la réinjecter dans le prompt.

# **🗞️PLUS D'ACTUALITÉS**

**ChatGPT débarque enfin sur Linux**

OpenAI publie une préversion de son application de bureau pour Linux, la plateforme la plus réclamée par sa communauté depuis des mois. Le paquet embarque ChatGPT, ChatGPT Work et Codex, et couvre Ubuntu 24.04 et 26.04 LTS, Debian 13, Fedora 43 et 44. Avec ce lancement, l'app est disponible sur tous les systèmes de bureau majeurs. Anthropic avait dégainé un mois plus tôt avec Claude pour Linux.

**Une société qui promet du « 100% humain, jamais d'IA » tourne entièrement à l'IA**

Research Gold vend des revues systématiques et des méta-analyses médicales prêtes pour la relecture par les pairs, avec un argument massue : « 100% rédigé par des humains, jamais par l'IA ». Enquête de 404 Media : les docteurs méthodologistes affichés sur le site n'existent pas, profils et photos générés par IA. D'autres, bien réels, découvrent que leur identité est utilisée sans leur accord. Et quand le journaliste appelle, c'est un agent IA qui décroche, refusant d'admettre qu'il en est un. Le sujet fait beaucoup réagir la communauté tech.

**Vous vous trompez sur la façon de penser les tendances en ligne**

Le cyber-ethnographe Ruby Thelot explique à WIRED pourquoi la viralité a cessé d'être un indicateur fiable : dès qu'elle devient une cible à optimiser, elle cesse d'être une mesure, loi de Goodhart appliquée aux réseaux. Internet s'est balkanisé en îlots numériques étanches, ce qui fausse totalement la perception de ce qui « marche » vraiment. Il démonte au passage le mythe du burnout des applications de rencontre, et explique pourquoi elles se jettent sur l'IA pour relancer l'engagement.

**Keet génère des cours vidéo sur n'importe quel sujet**

Une application mobile issue de Y Combinator qui construit un parcours d'apprentissage complet sur le thème de votre choix : courtes vidéos explicatives, exercices sous forme de jeux, progression étalée dans le temps comme un vrai curriculum. Les fondateurs s'appuient sur les catégories académiques de Biglan (Hard-Pure, Hard-Applied, Soft-Pure, Soft-Applied) pour adapter le format au type de savoir enseigné. L'idée : rendre à l'autodidacte la structure que l'école lui fournissait gratuitement.

**Abbott branche son capteur de glucose sur Google Health**

Le capteur de glucose en continu Lingo d'Abbott, vendu sans ordonnance et destiné aux adultes non diabétiques, va remonter ses données dans l'app Google Health. Google Health Coach en tirera des recommandations nutritionnelles générées par IA, en croisant glycémie, activité et sommeil. Les deux entreprises lancent aussi une vaste étude sur la santé métabolique. Réserve importante : plusieurs médecins continuent de douter de l'utilité réelle d'un suivi glycémique permanent chez une personne non diabétique.

**OpenAI commence à tester la publicité dans ChatGPT**

C'était annoncé, c'est lancé : OpenAI teste l'affichage d'annonces dans ChatGPT pour financer l'accès gratuit. L'entreprise promet des publicités clairement étiquetées, visuellement séparées des réponses, et surtout des réponses de l'IA qui restent indépendantes des annonceurs. Des protections de confidentialité et des réglages utilisateur sont annoncés en parallèle. Le vrai test viendra à l'usage : la frontière entre une réponse et une recommandation sponsorisée est plus mince dans un chat que dans une page de résultats.

**ChatGPT et Gemini franchissent tous deux le milliard d'utilisateurs**

Sundar Pichai a annoncé sur X que Gemini dépasse le milliard d'utilisateurs mensuels, ce qui en fait le produit Google à la croissance la plus rapide de l'histoire de l'entreprise, et le quatorzième à atteindre ce cap. OpenAI avait confirmé le même seuil pour ChatGPT le 6 août, dans un billet passé quasiment inaperçu, alors que des données externes le suggéraient déjà dès juin. Deux assistants, deux milliards d'utilisateurs cumulés, en trois ans et demi.

**Pourquoi les emails de confirmation Amazon sont devenus si vagues**

Depuis cet été, les confirmations de commande Amazon ne nomment plus les produits achetés : à la place, une catégorie générique et un clipart. « Votre article Beauté est confirmé », annonce ainsi un email pour des pastilles de nettoyage d'appareil dentaire. Pour connaître le contenu de sa commande, il faut quitter sa boîte mail et retourner sur Amazon. Le calendrier coïncide avec la montée des agents IA capables de lire vos emails, Gemini dans Gmail en tête : Amazon préfère aveugler Google que partager ses données d'achat.

**Les auteurs de l'étude Google de 2022 sous le feu des critiques**

Au printemps 2022, Google a discrètement recruté treize auteurs de science-fiction reconnus, dont Ken Liu et Robin Sloan, pour tester Wordcraft, un éditeur de texte bâti sur LaMDA, l'ancêtre de Gemini. Tout était sous accord de confidentialité, les nouvelles écrites avec l'outil ont été publiées avec un livre blanc. Quatre ans plus tard, la redécouverte de l'expérience vaut aux participants une volée de reproches d'une partie du monde littéraire, une autrice les accusant d'avoir légitimé « la machine à plagiat ».

**Manus redevient indépendante, Pékin ayant forcé Meta à défaire son rachat**

Meta avait annoncé en décembre 2025 l'acquisition de Manus, startup d'agents IA généralistes fondée en Chine en 2022, pour 2 milliards de dollars. Les régulateurs chinois (NDRC) ont exigé en avril l'annulation pure et simple de l'opération. Manus annonce reprendre « bientôt » son activité en entreprise indépendante, et prévient ses utilisateurs qu'ils devront sauvegarder les données générées depuis le 29 décembre 2025. Une démonstration nette de qui contrôle vraiment les actifs IA chinois.

**Les Galaxy Buds vont se transformer en aides auditives**

La FDA a autorisé la fonction d'aide auditive de Samsung sur les Galaxy Buds 3 Pro et 4 Pro, attendue d'ici la fin de l'année aux États-Unis et sur d'autres marchés. Un test auditif de cinq minutes intégré à l'app Samsung Health fournit un résultat de qualité médicale et classe la perte auditive en quatre stades. L'amplification s'appuie sur la réduction de bruit et le beamforming pour isoler la voix de l'interlocuteur en face. Samsung emboîte le pas aux AirPods Pro d'Apple.

**Novo Nordisk confie sa découverte de médicaments aux agents IA d'AWS**

AWS devient le fournisseur cloud privilégié et le partenaire IA stratégique du laboratoire danois, avec des agents déployés sur l'identification de cibles thérapeutiques et la conception de traitements. Un hub de co-innovation ouvre à Londres, où ingénieurs AWS et chercheurs Novo Nordisk travailleront sur les mêmes données. Plus de 25 000 employés utilisent déjà une plateforme d'IA générative sur Amazon Bedrock couvrant 2 500 cas d'usage, et un système bâti sur Claude 3.5 a réduit de plus de 90% le temps de production de la documentation d'essais cliniques.

**Saber dément avoir remplacé ses scénaristes par ChatGPT**

Matthew Karch, PDG de Saber Interactive, affirme que « ni Saber ni Unigine n'ont remplacé de scénariste par de l'IA » sur le jeu Rideshare Stimulator. L'ancienne scénariste principale, Stella Sacco, raconte l'inverse sur Bluesky : évincée en cours de développement, avec des voix de passagers générées par IA et non déclarées sur la fiche Steam du jeu. Deux versions, aucune preuve publique pour trancher, et un secteur où ce genre de litige va se multiplier.

**Spotify va étiqueter les artistes IA et les sortir des recommandations**

Dès la mi-septembre, un badge « AI Persona » apparaîtra sur les profils, dans la recherche et sur les pistes des artistes dont l'identité est générée par IA. Spotify ne se contentera pas de l'auto-déclaration : la plateforme examinera en priorité les profils photoréalistes dépassant certains seuils d'audience, avec possibilité de contester le label. Ces artistes seront exclus par défaut des recommandations éditoriales et algorithmiques, sauf si vous les suivez explicitement. Nuance importante : la mesure vise l'identité du profil, pas la manière dont la musique a été produite.

**Bernie Sanders somme OpenAI, Anthropic et Meta de suspendre le développement de l'IA**

Le sénateur américain adresse une lettre ouverte à Sam Altman, Dario Amodei et Mark Zuckerberg, dans laquelle il affirme que les capacités de l'IA ont franchi un seuil critique et exige une pause immédiate. Le ton ne laisse pas de place au doute : « Que ce soit très clair : si vous ne prenez pas les mesures qui s'imposent maintenant, mes collègues et moi au Sénat le ferons. » Aucun des trois laboratoires n'a annoncé la moindre intention de ralentir.

**La responsable de l'éthique d'OpenAI s'en va après moins d'un an**

Chloe Bakalar quitte son poste de head of ethics chez OpenAI moins de douze mois après son arrivée, rapporte le Financial Times. Un passage aussi court à un poste aussi exposé relance mécaniquement la question du poids réel accordé aux enjeux éthiques et de sécurité dans l'organisation. Le sujet fait beaucoup réagir la communauté tech.

**Brad Lightcap, directeur des opérations historique d'OpenAI, s'en va**

Entré en 2018, quatre ans directeur financier puis directeur des opérations à partir de 2022, Brad Lightcap annonce dans un message interne qu'il quitte OpenAI pour « lancer quelque chose de nouveau », sans plus de précision. Il rejoint une liste qui s'allonge : Fidji Simo, Bill Peebles, Kevin Weil. Un remaniement de fond de la direction, au moment précis où l'entreprise prépare une possible entrée en bourse.

**Le fonds souverain norvégien dévoile sa participation dans SpaceX**

Le plus gros fonds souverain du monde, 2 300 milliards de dollars sous gestion, affiche un profit semestriel record de 184,9 milliards de dollars, porté par le rallye des valeurs technologiques asiatiques et un rendement de 9,4%. Il révèle pour la première fois détenir 0,05% de SpaceX, soit environ 1,2 milliard de dollars. Son portefeuille tech comprend aussi 1,3% de Nvidia (61,8 milliards), 1,2% d'Apple (52,7 milliards) et 1% de Tesla.