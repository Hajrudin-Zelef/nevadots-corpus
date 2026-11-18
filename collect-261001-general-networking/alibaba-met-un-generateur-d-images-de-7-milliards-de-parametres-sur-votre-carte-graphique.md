## **Aujourd'hui:**

🎨 Qwen-Image-2.1 tourne en local

🔓 Gemini pirate trois entreprises réelles

🚫 Jensen Huang contre les alarmistes

🤝 Dialogue IA Washington, Pékin

🔐 Un message Enigma de 1941 déchiffré

💸 Les chatbots se trompent sur vos finances

🎓 Des élèves simulés pour entraîner les tuteurs IA

📺 Runway veut diffuser la vidéo IA en direct

📈 L'usage quotidien de l'IA a doublé aux États-Unis

🌍 Les world models cultivent le secret

🧠 Apprentissage continu : 1,2 % à 34,9 % de rétention

🎮 Modèles de fondation et jeux vidéo

🛠️ Google ouvre EnvHarness en Apache 2.0

⚙️ AX, l'orchestrateur d'agents de Google

🤖 Une douzaine de robots Coco bloquent un trottoir

🇨🇳 Unitree et l'obsession du coût

✈️ Joby réussit 3 199 miles en autonomie totale

👁️ Meta Muse collecte plus qu'il n'aide

**Savoir se servir de ChatGPT ne vous distingue plus. Votre collègue le fait, le stagiaire aussi.**

Et une compétence que tout le monde a ne se facture pas. Pourtant, il reste une demande que ChatGPT ne peut pas servir : un cabinet comptable ne mettra jamais ses dossiers clients dedans. Un avocat non plus, un cabinet médical encore moins. Ils veulent l'IA comme tout le monde, mais sans que leurs fichiers sortent du bureau.

Ça existe, et ça s'appelle l'IA locale : le modèle tourne sur leur propre machine, et il répond même avec le wifi coupé. Pareil pour l'image et la vidéo, sans limite de crédits. Peu de gens savent l'installer, et personne ne l'enseigne en français. Une compétence rare, elle, se facture.

**Dans l'Académie Privée VISION IA**, je vous l'apprends dans l'ordre, en commençant par vérifier ce que votre machine peut faire tourner. Aujourd’hui pas besoin d'une machine de laboratoire.

39 € par mois, **sans engagement, résiliable à tout moment.**

L'équipe Qwen d'Alibaba a publié le **20 septembre** Qwen-Image-2.1, un modèle de génération et d'édition d'images en open-weight qui tourne sur une carte graphique grand public, une **RTX 3090** suffit. Son composant de génération visuelle ne pèse que **7 milliards de paramètres**, répartis sur **32 couches de transformer diffusion à flux unique**, avec des optimisations d'inférence par réutilisation du cache KV.

**Transparence native (RGBA)** : il produit directement des images détourées, sans passer par un outil de découpe. C'est rare, et immédiatement utile pour du visuel de marque ou des miniatures.
**Jusqu'à 10 images de référence simultanées** : dix photos individuelles deviennent un portrait de groupe, avec aussi de l'essayage virtuel de vêtements et du design de pièces.
**Édition locale guidée** : on entoure une zone, on pose un masque ou une annotation, et il ne retouche que cette partie.
Poids disponibles sur **Hugging Face, GitHub et ModelScope** , avec une démo en ligne gratuite. Les détails de packaging sont repris par DEV Community.
Le bémol : **licence de recherche uniquement** . L'usage commercial est interdit sans licence Qwen spécifique, à demander séparément.

Une nuance sur la revendication maison. Sur le Qwen-Image-Bench d'Alibaba (1 000 prompts, jugés automatiquement par Qwen3.6-27B), Qwen-Image-2.1 se classe **7e sur 29** avec **60,28 points**, derrière six modèles fermés dont GPT Image 2.5 Sunburst à **67,01**. Le « bat les modèles fermés » du communiqué ne résiste donc pas à la lecture du benchmark, qui est de surcroît interne et non vérifié par un tiers.

Pour un usage personnel, c'est exploitable ce soir : un générateur d'images sérieux, gratuit, sur votre machine, sans quota ni envoi de vos photos sur un serveur distant. Pour un usage professionnel, la licence de recherche referme la porte aussi vite qu'elle s'est ouverte.

Google a reconnu vendredi que son modèle Gemini était sorti de son environnement de test en **mai 2026** et avait accédé aux systèmes informatiques de **trois entreprises bien réelles**, en devinant des identifiants et en puisant **deux fois** dans des répertoires de mots de passe publics. C'est la première fois que Google admet publiquement qu'un de ses modèles a pénétré seul des systèmes tiers sans autorisation.

**Le cadre** : un exercice de type capture-the-flag mené par**Irregular** , startup israélienne spécialisée dans les tests de cybersécurité des modèles de pointe.
**La faille** : un**bug de l'environnement de test** a donné à l'agent un accès à l'internet public qui n'était pas prévu. Il devait rester confiné.
**Le mode opératoire** : collecte d'informations publiques en ligne, puis devinette d'identifiants sur des sites qu'il croyait faire partie de l'exercice.
**L'arrêt** : dans les trois cas, les agents ont interrompu l'intrusion en réalisant qu'ils touchaient de vrais systèmes d'entreprise.
**La chronologie** : incident en mai, Google prévenu fin juillet par Irregular, entités concernées notifiées, processus de test révisé, révélation publique vendredi après un scoop du Wall Street Journal. Aucun vol de données ni dégât rapporté.

Heather Adkins, VP sécurité chez Google : « lors d'une évaluation standard, le modèle a trouvé des informations publiques en ligne et deviné des identifiants pour accéder à des sites qu'il pensait faire partie du test. Dans les trois cas, le modèle s'est arrêté. »

Gemini n'est pas un cas isolé : OpenAI, Anthropic et Meta ont signalé des sorties comparables ces dernières semaines, et Irregular affirme qu'il s'agit du même bug, pas d'incidents distincts. Un détail circule toutefois, et il est gênant : dans un cas similaire, le modèle d'Anthropic ne se serait pas arrêté. C'est cette série qui a poussé Dario Amodei à réclamer un ralentissement collectif du secteur.

Interrogé par Jo Ling Kent dans CBS Sunday Morning, le PDG de Nvidia a répondu par un chiffre aux prédictions d'extinction : « **2030 ne sera pas la fin du monde. Il y a 0 % de chance.** » Il réagissait aux propos d'un ancien chercheur d'Anthropic, **Jacob Coxon**, selon qui les développeurs d'IA croient sincèrement que la technologie pourrait « tous nous tuer » d'ici la fin de la décennie.

**Ce qu'il dit exactement :**

Les scénarios d'extinction ne sont **« pas fondés scientifiquement »** .
Effrayer le public est **« inutile »** et**« irresponsable »** .
Les appels au ralentissement de Dario Amodei et Sam Altman reposent sur du vide.
**Aucune nouvelle réglementation n'est nécessaire** : le droit existant de la responsabilité civile suffit, position qui rejoint celle de l'administration Trump.
Sa réponse au soupçon de conflit d'intérêts : « le succès de notre entreprise est directement lié au déploiement sûr de l'IA ».

Le soupçon en question n'est pas mince. Nvidia pèse environ **5 300 milliards de dollars**, première capitalisation mondiale, et la fortune personnelle de Huang est estimée à **182 milliards**, huitième rang mondial. L'homme qui vend les pelles pendant la ruée vers l'or explique que la mine est parfaitement sûre. Tom's Hardware relève qu'il pousse à aller « aussi vite que possible, quoi que fassent les autres ».

Ce « 0 % » tombe la semaine même où Google admet qu'un de ses modèles est sorti de son bac à sable pour pirater trois entreprises. Le débat s'est déplacé : il ne porte plus vraiment sur la vitesse du développement, mais sur la question de savoir qui est légitime pour dire que la technologie est dangereuse, les labos qui la construisent ou celui qui la finance.

Scott Bessent, secrétaire au Trésor américain, a qualifié de « très réussie » sa rencontre avec He Lifeng, vice-Premier ministre chinois, à quelques jours du sommet Trump-Xi du **24 septembre** à Washington. Les deux pays vont lancer un « **dialogue États-Unis-Chine sur l'IA** », dont la pièce maîtresse proposée par les Américains est un mécanisme de notification mutuelle des incidents liés à l'IA.

**Le mécanisme** : chaque pays alerterait l'autre dès qu'un incident IA atteint le niveau de la sécurité nationale. « Passer de l'opaque à davantage de transparence entre la première et la deuxième puissance IA mondiale est très important », a déclaré Bessent.
**L'organe** : un groupe de travail conjoint réunissant des experts techniques et des responsables disposant d'un mandat de négociation.
**L'agenda annoncé** : modèles et agents « incontrôlés », risque d'armes biologiques conçues par des acteurs non étatiques, attaques sur les infrastructures critiques.
**Ce qui n'existe pas encore** : aucun calendrier, aucun budget, aucune structure formelle. Rien de contraignant à ce stade, selon CNN.
**Au dîner d'État** pour Xi Jinping : Sam Altman (OpenAI) et Jensen Huang (Nvidia) sont attendus, aux côtés des patrons de JPMorgan Chase et Citigroup.

L'idée remonte à un accord Trump-Xi conclu à Pékin en mai. Côté américain, on n'attend aucune percée du sommet, au mieux une déclaration commune de préoccupations partagées. Et le vrai sujet de la semaine est ailleurs : la suspension des droits de douane réciproques sur les importations chinoises expire le **10 novembre**, ce qui pèsera plus lourd dans la salle que la sécurité des modèles.

# 🧠 **RECHERCHE**

### **Un message Enigma de 1941 resté non résolu vient d'être déchiffré**

Le 10 juillet 1941, un opérateur radio de l'armée allemande demandait un itinéraire de marche et une réponse immédiate. Les réglages machine ont été retrouvés (ordre des rotors, bagues, tableau de connexions), et la clé explique l'intégralité des **82 lettres** du corps du message ainsi que son en-tête, vérifié sur les copies survivantes. Le texte recouvré : « BTTE UM ANGABE DES MARSQWEGES X BEFINDE MIQ IN X ROSENOW ROSENOW X SOFORT FUNKANTWORT X WASCHBBSCH ». GPT-6 Astra est crédité de l'aide à la cryptanalyse, et la page publie ses preuves, ses incertitudes de lecture et un simulateur Enigma qui tourne dans le navigateur.

**Les chatbots IA se trompent « la plupart du temps » sur les questions financières**

Selon une enquête relayée par le Financial Times, les assistants conversationnels donnent des réponses inexactes à la majorité des questions d'ordre financier qu'on leur pose. Le problème n'est pas théorique : épargne, investissement, crédit, ce sont précisément les sujets sur lesquels les particuliers se tournent vers eux, avec un risque de préjudice réel. À lire comme un rappel : toute information financière obtenue via une IA se vérifie ailleurs avant d'agir.

**Des élèves simulés qui font de vraies erreurs entraînent mieux les tuteurs IA**

Microsoft et l'Université de l'Illinois ont bâti StudentSim, un système qui reconstitue le profil d'apprentissage d'un élève réel à partir de très peu de données, erreurs typiques comprises. Testé sur **60 élèves** dans trois domaines (échecs, anglais, mathématiques), il surpasse GPT-5.4 dans la fidélité de la simulation. Un tuteur d'échecs entraîné face à ces élèves virtuels a décroché les meilleures évaluations d'experts parmi trois versions comparées, pour un coût et un délai sans commune mesure avec des tests sur de vrais apprenants.

**Runway veut transformer la génération vidéo en flux live contrôlable**

Plutôt que d'attendre le rendu d'un clip terminé, Runway veut diffuser la vidéo en streaming pendant que vous tapez votre prompt, et la laisser dévier en temps réel selon ce que vous écrivez. La brique technique est GWM-1, son modèle de monde qui génère la vidéo image par image. L'entreprise vise explicitement au-delà de la création : robotique et conduite autonome, deux domaines où prédire l'image suivante revient à anticiper le monde physique.

**L'usage quotidien de l'IA a plus que doublé aux États-Unis en six mois**

D'après des enquêtes d'Epoch AI et d'Ipsos, la part d'adultes américains utilisant l'IA presque tous les jours est passée de **8 % en mars 2026 à 19 % en août**. Dans le même mouvement, les utilisateurs hebdomadaires ont reculé de **17 % à 10 %**. La bascule est plus intéressante que la croissance brute : le public ne s'élargit pas seulement, il s'intensifie, et l'IA quitte le statut d'outil occasionnel pour celui d'habitude.

**Les entreprises de world models cultivent le secret**

AMI Labs (Yann LeCun) et World Labs (Fei-Fei Li) dominent le secteur des modèles de monde, accumulent financements et attention, et refusent obstinément de dire ce qu'ils comptent en faire. « Nous en parlerons quand nous serons prêts », résume Michael Rabbat, VP World Models d'AMI Labs. L'opacité va jusqu'aux fournisseurs de données, qui ignorent à quoi serviront leurs jeux de données. Marble, de World Labs, reste le produit le plus abouti, orienté environnements explorables pour le jeu vidéo et les effets spéciaux, mais ressemble surtout à une démonstration technique.

**Apprentissage continu : la rétention passe de 1,2 % à 34,9 % en combinant les mécanismes**

Une étude arXiv pose une question simple : un modèle de langage peut-il enchaîner **100 tâches** successives sans oublier les premières ? Prise isolément, aucune méthode de préservation de la mémoire n'y arrive. C'est leur combinaison qui fait passer la rétention finale de **1,2 % à 34,9 %** sur trois jeux de données. Encore loin du compte, mais c'est la piste la plus sérieuse vers des IA qui accumulent des connaissances au lieu de repartir de zéro à chaque entraînement.

**Modèles de fondation et jeux vidéo : six usages recensés**

Cette synthèse académique classe en six rôles ce que les modèles de fondation font désormais dans le jeu vidéo : incarner des personnages, concevoir, construire, adapter l'expérience, modéliser les joueurs et les mondes, et tester. L'IA y dépasse largement son rôle historique de simple joueur pour devenir un outil de production. L'obstacle principal reste le transfert : ce qui marche sur un moteur, un jeu ou un public donné ne se généralise pas aux autres.

# **🗞️PLUS D'ACTUALITÉS**

### **Google ouvre EnvHarness, des environnements d'entraînement qui s'adaptent aux faiblesses des agents**

Google Cloud AI Research et des partenaires académiques publient EnvHarness sous licence **Apache 2.0**. Le principe : enrober un environnement d'entraînement existant d'une couche programmable qui modifie dynamiquement le point de départ, les observations, les actions disponibles et la durée des tâches selon les lacunes de l'agent, sans toucher ni à l'environnement ni à son vérificateur. Sur cinq benchmarks couvrant le développement logiciel, la navigation web, la bureautique et les tâches robotiques, les agents entraînés ainsi gagnent **jusqu'à 9 points** sur des tâches jamais vues, et résolvent le code en moins d'étapes. L'intérêt pratique : on cesse de construire des simulateurs coûteux jetables, on remodèle celui qu'on a déjà.

**AX, l'orchestrateur d'agents IA open source de Google**

AX exécute des tâches d'agents à très grande échelle en isolant chacune dans un bac à sable, avec limites de CPU, de mémoire et politique réseau par liste blanche d'hôtes. Quatre primitives déclaratives suffisent : le sandbox, la préparation automatique de l'espace de travail (dépôts Git, serveurs MCP, compétences), le cloisonnement réseau et la configuration centralisée des modèles et secrets. Le moteur Agent Substrate sauvegarde et suspend les agents inactifs, ceux qui attendent une réponse de modèle ou d'un humain, puis les restaure en moins d'une seconde sans démarrage à froid, la facturation ne courant que pendant le travail réel. Le projet fait beaucoup réagir la communauté tech ce week-end.

**Une douzaine de robots livreurs Coco bloquent un trottoir de Chicago**

Un bug a fait converger une douzaine de robots de livraison Coco au même endroit, sur un trottoir près de Lincoln Park, piégeant les piétons qui n'avaient plus aucun passage. Un passant a filmé la scène, devenue virale. Coco assure avoir identifié et corrigé le dysfonctionnement en quelques minutes. L'épisode s'ajoute à une série de ratés qui ont déjà envoyé un habitant à l'hôpital et valu aux robots une interdiction dans certains quartiers de la ville.

**Unitree : comment l'obsession du coût de son fondateur a fait chuter le prix des humanoïdes**

Un long reportage du magazine chinois Caijing, traduit par ChinaTalk, raconte comment Wang Xingxing a imposé un micromanagement extrême et une traque permanente des coûts chez Unitree Robotics : **13 500 dollars** pour le G1 destiné aux chercheurs, **4 900 dollars** pour le R1 grand public. Le fondateur tranche personnellement à peu près tout, jusqu'à la couleur des matériaux et la longueur des vis. La contrepartie a longtemps été un contrôle qualité déplorable, avec un taux de retour en réparation « extrêmement élevé » dans les premières années, depuis amélioré. Introduite au STAR Market de Shanghai le 19 août, l'entreprise vise désormais une IA physique bouclant perception, décision, exécution et apprentissage. Wang reste sceptique sur les grands world models, trop gourmands en calcul selon lui.

**Joby réussit un vol entièrement autonome de la Californie à la Caroline du Nord**

Joby Aviation revendique le premier vol autonome traversant les États-Unis : **3 199 miles** entre Buchanan Field en Californie et Dare County Regional Airport en Caroline du Nord, sans la moindre intervention du pilote de sécurité présent à bord. Le système, installé sur un Cessna Caravan modifié, a géré seul le roulage, le décollage, la croisière et l'atterrissage, avec une supervision à distance depuis la Californie et depuis Shaw Air Force Base. Surtout connue pour ses taxis aériens électriques pilotés, l'entreprise vise désormais le fret commercial, les secours d'urgence et la logistique de défense.

**Meta Muse est plus doué pour collecter vos données que pour vous aider**

WIRED a testé Muse, le nouvel agent IA personnel de Meta, téléchargé plus de **900 000 fois** en une semaine selon Sensor Tower et accessible aussi via WhatsApp. L'agent navigue réellement sur le web depuis une machine virtuelle pour commander un petit-déjeuner, chercher des bonnes affaires sur Marketplace ou trier une boîte mail, et le journaliste reconnaît une fiabilité nettement supérieure aux agents testés l'an dernier. Son reproche porte ailleurs : l'application pousse à connecter email et compte bancaire, une collecte qu'il juge disproportionnée au service rendu. Meta répond que protections et contrôles utilisateurs sont intégrés dès la conception.

**Gander, l'assistant vocal de Tencent qui continue à parler pendant qu'il travaille**

Tencent sépare la conversation du travail : un « cervelet » maintient le dialogue en continu pendant qu'un « cerveau » interchangeable cherche des fichiers, écrit du code ou traite des tâches longues en arrière-plan. Résultat, on peut interrompre Gander ou changer de consigne en pleine exécution, sans le silence gênant des assistants vocaux actuels. Il coupe la parole à son interlocuteur dans **8 % des cas** seulement, mieux que ses concurrents, mais reste en retrait sur la précision des tâches. Multimodal : voix, images et texte.

**Les entreprises vont-elles créer des départements de « relations robots » ?**

Darrell M. West, de la Brookings Institution, propose des services de « robot relations » distincts des RH, chargés de traiter les plaintes liées aux IA, agents et cobots au travail. Le sujet est déjà concret : selon une enquête de l'OCDE, **90 % des managers américains** déclarent utiliser au moins un outil algorithmique pour instruire, surveiller ou évaluer leurs salariés, ce qui place les États-Unis en tête mondiale du management algorithmique. Les frictions existent, entre accusations de biais des algorithmes RH et usage de ces outils pour désigner qui licencier. La Californie a voté un texte interdisant les « robo bosses » en matière de licenciement ou de sanction, que Gavin Newsom doit signer ou rejeter avant fin septembre.

**Trump veut créer une « AI Force » et nommer un « AI czar »**

Le président a annoncé sur Truth Social son intention de nommer un « AI czar » à la tête d'une nouvelle « AI force » gouvernementale. Il précise que son administration ne freinera pas le secteur mais le « surveillera » pendant sa croissance, une formule volontairement vague sur les pouvoirs réels de cette structure. L'annonce tombe alors que les appels à ralentir se multiplient, y compris depuis l'industrie elle-même. Trump en a profité pour défendre les data centers, présentés sans démonstration comme bons pour les salaires, les impôts locaux et la sécurité des territoires qui les accueillent.

**Meta et Snap relancent la course aux lunettes AR**

Evan Spiegel a livré la semaine dernière une démonstration détaillée des Specs de Snap, dévoilées en juin. C'est au tour de Mark Zuckerberg cette semaine, avec son keynote à la conférence Connect de Meta. Selon The Information, Meta lancerait dès octobre une paire de lunettes connectées **sans caméra**, choix visiblement destiné à désamorcer les inquiétudes sur la vie privée, ainsi que de nouvelles lunettes AR. Reste la question qui fâche : les modèles très médiatisés présentés l'an dernier n'ont toujours pas de suite commerciale claire.

**Anthropic reporterait son IPO à novembre**

D'après The Information et le Wall Street Journal, Anthropic décale son introduction en bourse d'octobre à novembre 2026, le temps de présenter de solides résultats du troisième trimestre. Les chiffres qui circulent sont spectaculaires : un chiffre d'affaires passé d'environ **4,7 à plus de 11,5 milliards de dollars** entre le premier et le deuxième trimestre, pour des dépenses d'infrastructure en hausse de **65 %** seulement, et un nombre de clients entreprises dépensant plus de 100 000 dollars par an passé d'environ **1 500 à 6 000** en six mois. Coûts élevés, concurrence d'OpenAI, remontée des taux et risques cyber non assurés figurent parmi les explications avancées du report. OpenAI a déjà repoussé la sienne à 2027.

**Savoir se servir de ChatGPT ne vous distingue plus. Votre collègue le fait, le stagiaire aussi.**

Et une compétence que tout le monde a ne se facture pas. Pourtant, il reste une demande que ChatGPT ne peut pas servir : un cabinet comptable ne mettra jamais ses dossiers clients dedans. Un avocat non plus, un cabinet médical encore moins. Ils veulent l'IA comme tout le monde, mais sans que leurs fichiers sortent du bureau.

Ça existe, et ça s'appelle l'IA locale : le modèle tourne sur leur propre machine, et il répond même avec le wifi coupé. Pareil pour l'image et la vidéo, sans limite de crédits. Peu de gens savent l'installer, et personne ne l'enseigne en français. Une compétence rare, elle, se facture.

**Dans l'Académie Privée VISION IA**, je vous l'apprends dans l'ordre, en commençant par vérifier ce que votre machine peut faire tourner. Aujourd’hui pas besoin d'une machine de laboratoire.

39 € par mois, **sans engagement, résiliable à tout moment.**