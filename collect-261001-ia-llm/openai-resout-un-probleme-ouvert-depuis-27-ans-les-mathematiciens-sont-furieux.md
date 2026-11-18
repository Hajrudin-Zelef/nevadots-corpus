## **Aujourd'hui:**

🔢 Les mathématiciens somment OpenAI de dire où il a pris ses preuves

🏦 ChatGPT débarque chez les banquiers, propulsé par GPT-6 Astra

🎙️ GPT-Live-1 s'ouvre aux développeurs et parle en même temps qu'il écoute

🦠 Anthropic a surpris des scientifiques travaillant sur des virus avec Claude

🐦 Un modèle de 744 milliards de paramètres sur un PC avec 25 Go de RAM

🐳 DeepSeek V4.1-Flash divise par quatre la mémoire des agents IA

✂️ MultiMatte détoure une image sur simple consigne écrite

💊 Codex part chasser des antibiotiques dans les génomes d'espèces éteintes

Un avocat non plus. Un cabinet médical encore moins. Ils veulent l'IA comme tout le monde, mais sans que leurs fichiers sortent du bureau.

Ça existe, et ça s'appelle l'IA locale : le modèle tourne sur leur propre machine, et il répond même avec le wifi coupé. Pareil pour l'image et la vidéo, sans limite de crédits. C'est une compétence rare, et personne ne l'enseigne en français.

Mais savoir l'installer ne suffit pas, il faut savoir à qui le vendre et comment le dire. C'est tout le reste de l'Académie.

Vous bénéficiez aussi de :

✅ Business & monétisation : lire le marché, choisir un marché, monter le prototype, vendre

✅ Marketing & copywriting : mes secrets pour que votre offre se vende

✅ Mental & discipline : une leçon audio chaque jour

✅ Le fil de discussion quand vous bloquez, avec un réseau de professionnels qualifiés

✅ De nouvelles leçons chaque semaine, et une cinquième formation en préparation

➡️ Le tarif que vous prenez aujourd'hui reste le vôtre, tant que votre abonnement continue. Le catalogue grandit chaque semaine, et le prix montera pour les nouveaux.

39 € par mois, sans engagement, résiliable en quelques clics. Ou 390 € par an, avec deux mois offerts et la Formation de base en IA incluse. Vous l'avez déjà ? Vous pourrez l'offrir à la personne de votre choix.

On se retrouve à l'intérieur.

Le 1er août 2026, OpenAI annonçait que son modèle Astra avait résolu **dix problèmes mathématiques ouverts**, dont la première construction d'un **groupe non-sofic**, une question posée par Gromov il y a **27 ans**. Sauf que la démonstration s'appuie directement sur un papier de 2019 signé Andreas Thom et Gábor Kun, et que Thom, théoricien des groupes à la TU Dresden, avait passé des mois à travailler précisément ce sujet en discutant avec ChatGPT. Il demande aujourd'hui à OpenAI de prouver que ses conversations privées n'ont pas nourri l'entraînement du modèle.

**Ce qu'il faut retenir :**

- Thom a détaillé son accusation dans une série de messages sur Mastodon, parlant d'un comportement « malhonnête » et d'une opacité totale sur l'origine des données

- Il avait **désactivé l'entraînement sur ses données le 29 juin 2026**, doutant déjà de la traçabilité : ce réglage existe pour tout le monde dans les contrôles de données de ChatGPT, et il est actif par défaut dans l'autre sens

- Le chercheur d'OpenAI Mark Sellke a répondu « that did not happen », une formule qui ne couvre que l'accès direct au papier pendant la résolution, pas l'usage des conversations dans le corpus d'entraînement

- Sur un autre dossier, autour des équations de Navier-Stokes, OpenAI a reconnu **ne pas pouvoir exclure** que des données « dé-identifiées » aient amélioré ses modèles

- Thom est le **second mathématicien en quelques jours** à porter cette accusation, et l'entreprise n'a produit aucune preuve technique de traçabilité

Voilà la boucle refermée : un chercheur travaille un problème avec un chatbot pendant des mois, le chatbot résout le problème, et l'entreprise s'en attribue le mérite sans pouvoir démontrer le contraire. Pour n'importe quel professionnel qui met un dossier confidentiel dans un assistant, la leçon est concrète : l'opt-out existe, il est dans les paramètres, et c'est aujourd'hui la seule protection vérifiable, faute de journal d'audit côté labo.

OpenAI a lancé le 10 septembre **ChatGPT for Financial Services**, une déclinaison de ChatGPT Work qui combine des jeux de données financières intégrés et le modèle de raisonnement **GPT-6 Astra**. Concrètement, l'outil attaque la pile de travail confiée aux analystes : valorisations d'entreprise, modèles LBO, screening d'acheteurs potentiels, analyse de résultats trimestriels, pitchbooks et notes clients. Plus de téléchargement de fichiers Excel : les données arrivent nativement dans l'outil.

**En détail :**

- Données intégrées d'origine : **Daloopa, PitchBook, LSEG News et Crunchbase**, soit les transcripts de résultats, les états financiers et les données de sociétés non cotées

- Chaque chiffre produit porte une **citation granulaire** qui remonte à sa source, le point de friction habituel de ces outils en environnement régulé

- Conçu avec **Morgan Stanley et Evercore** comme design partners, avec des intégrations d'authentification et de droits d'accès chez FactSet, S&P Global, Preqin et Datasite

- Disponibilité : **sur demande** auprès des institutions financières éligibles, via le commercial d'OpenAI. Le prix n'est pas publié officiellement (une source évoque environ 10 dollars par million de tokens, non confirmé)

- Absence notable dans l'annonce : **aucune mention d'hébergement européen ni de conformité DORA**, alors que c'est le premier filtre d'achat d'une banque de l'UE

Le marché visé n'est pas le trader vedette, c'est l'étage en dessous : le junior qui passe ses nuits à retraiter des comptes et à mettre en forme des livrables. Et pour les professionnels francophones de la finance, de l'audit ou du juridique, la vraie question du jour n'est pas la qualité du modèle mais le silence de l'annonce sur la résidence des données, qui suffit à bloquer un déploiement côté européen.

OpenAI ouvre aux développeurs **GPT-Live-1**, le modèle vocal qui tourne déjà dans ChatGPT, au tarif de **0,05 dollar la minute**. Sa particularité tient en un mot : le **full-duplex**. Le modèle écoute et parle simultanément, peut être interrompu, peut lui-même interrompre, marquer une pause et appeler des outils pendant qu'il vous répond. Il gère uniquement la couche vocale et se branche sur le modèle de raisonnement de votre choix pour la logique métier, ce qui permet d'arbitrer entre profondeur, vitesse et coût.

**Quelques chiffres clés :**

- Interactivité full-duplex mesurée à **80,1 %** contre 45,4 % pour GPT-Realtime-2.1, la génération précédente

- Latence de prise de parole ramenée à **0,8 seconde** contre 1,4 seconde, soit le seuil à partir duquel un échange cesse d'être pénible

- Précision d'appel d'outils portée à **87 %**, contre 60 %, c'est ce qui permet à l'agent de consulter un agenda ou un dossier client en direct

- Sur un benchmark de support bancaire vocal, le taux de réussite passe à **32 %**, contre 12,4 % avant : gros progrès, mais encore deux échecs sur trois

- **Douze nouvelles voix** avec accents, dialectes et langues variés, transcriptions ASR et texte de réponse fournis par défaut

Yelp s'en sert déjà pour ses réservations téléphoniques et son CTO Alex Levy rapporte une meilleure prise en charge des appels. À trois dollars l'heure de conversation, la voix synthétique devient moins chère qu'un standard humain, et le prochain répondeur de votre banque, de votre mutuelle ou de votre cabinet médical saura vous relancer avant que vous ayez fini votre phrase.

Anthropic a publié le 10 septembre son rapport « Detecting and Countering Misuse of AI », qui documente **cinq cas** où des comptes Claude ont servi à des travaux pouvant faciliter le développement d'armes biologiques. Le plus marquant : un utilisateur demandait à Claude de rédiger une **demande de subvention** pour de la recherche de gain de fonction sur le **virus chikungunya**, afin de le muter chez l'animal vivant pour le rendre plus infectieux et plus capable d'échapper au système immunitaire. Le virus n'a aucun traitement autorisé et provoque des symptômes invalidants pendant des semaines.

**Les points essentiels :**

- La détection vient du **classificateur de sécurité biologique** interne d'Anthropic, pas d'un signalement extérieur. Le projet était rattaché à un **institut de recherche militaire**, ce qui a pesé dans la décision

- Deuxième cas : un chercheur hors des États-Unis a échangé **des milliers de messages sur plusieurs semaines** autour de la grippe aviaire hautement pathogène, sur des approches génétiques liées à l'adaptation aux mammifères et à la transmission par voie aérienne

- Autre dossier : la constitution d'un **atlas de peptides toxiques de venin** avec un pipeline génératif optimisant les caractéristiques des toxines

- **Tous les comptes ont été bannis**, mais ni les personnes ni leurs laboratoires ne sont nommés, pour ne pas les exposer. Les renseignements ont été partagés avec les autorités et d'autres entreprises d'IA

- Nuance officielle : Anthropic précise que ce sont des **scientifiques en activité** et n'affirme pas qu'ils avaient une intention malveillante

« Vous ne voyez pas quelqu'un dire, façon comic book, "je veux construire une arme biologique pour tuer tout le monde" », résume Jacob Klein, responsable du renseignement sur les menaces chez Anthropic : « c'est une situation incroyablement nuancée. » Voilà le vrai problème : une demande de subvention pour un vaccin et une demande pour une arme se ressemblent, et plus les modèles deviennent bons en assistance à la recherche, plus le tri devient arbitraire. Anthropic dit avoir systématiquement tranché dans le sens de la prudence, donc au risque de bloquer des travaux légitimes.

# 🧠 **RECHERCHE**

### **Colibrì fait tourner un modèle de 744 milliards de paramètres sur 25 Go de RAM**

Un runtime open source en C pur, sans aucune dépendance, qui exécute GLM-5.2 sur du matériel grand public. L'astuce : seuls ~40 milliards de paramètres sont activés par token, donc Colibrì garde en RAM les tenseurs denses quantifiés en int4 (attention et embeddings, environ 9,9 Go) et laisse les ~370 Go d'experts routés sur un SSD NVMe, chargés à la demande à chaque étape de décodage. C'est lent, mais ça tourne chez vous.

**DeepSeek V4.1-Flash divise par quatre la mémoire des agents IA**

Modèle multimodal de 552 milliards de paramètres dont 16 milliards seulement s'activent par token, avec un cache KV réduit au quart de celui de la version précédente. Sur le benchmark de code DeepSWE, il devance de peu Opus 5 et GPT-5.6 Sol. Le tout sous licence MIT, ce qui rend les agents nettement moins coûteux à faire tourner en continu.

**MultiMatte détoure une image à partir d'une phrase**

La startup Feyn publie un modèle de suppression d'arrière-plan qu'on vise avec des mots : « garde seulement le chien », et le reste disparaît. Construit sur SAM 3 de Meta, il remplace les masques binaires par des alpha mattes, d'où un bien meilleur rendu des cheveux, de la fourrure et du flou de mouvement. Gains mesurés face à SAM 3 : S-measure de 0,674 à 0,908 sur DIS5K. Bibliothèque NoBg sur GitHub, modèle sur Hugging Face.

**Codex et ChatGPT partent chasser des antibiotiques dans les génomes d'espèces éteintes**

Le laboratoire de César de la Fuente utilise Codex et ChatGPT comme assistants pour fouiller les génomes d'organismes vivants et d'espèces disparues à la recherche de peptides antimicrobiens. Cible : les infections résistantes aux médicaments, l'une des grandes menaces sanitaires mondiales, sur laquelle le pipeline classique de découverte est à sec.

**Les services publics croulent sous les dossiers rédigés par IA**

Les plaintes au médiateur du logement britannique sont passées de 2 600 en 2022 à plus de 7 000 l'an dernier, celles de l'agence américaine CFPB ont été multipliées par cinq, avec des hausses comparables sur les requêtes judiciaires brésiliennes et les pétitions parlementaires allemandes. Le chercheur Chris Schmitz a documenté 84 cas d'« agentic flooding » dans 11 juridictions. Contre-intuitif : ces demandes viennent majoritairement de gens réels avec des dossiers légitimes qu'ils auraient autrefois abandonnés.

**Anthropic chiffre les campagnes chinoises de distillation de Claude**

Près de 200 millions d'échanges répartis sur cinq campagnes, dont la plus grosse jamais observée : 151 millions d'échanges entre mai et juillet 2026 via 3 500 comptes, attribuée à Alibaba pour alimenter l'entraînement de Qwen. La technique consiste à faire révéler au modèle sa chaîne de raisonnement, normalement masquée, en déguisant la demande, par exemple en exercice de traduction. Une campagne liée à Moonshot AI semblait acheminer des requêtes militaires, dont de l'analyse d'images de vidéosurveillance.

**GPT-6 Astra domine les maths, et OpenAI dit ne pas avoir cherché ça**

Le modèle prend la première place d'ErdosBench, un benchmark de problèmes mathématiques non résolus, alors que le chief scientist Jakub Pachocki affirme que les mathématiques n'étaient délibérément pas une priorité : les ressources vont à l'auto-amélioration récursive et à la recherche en alignement. De quoi renforcer la thèse d'un progrès « spiky », extrême sur quelques domaines ciblés plutôt qu'uniforme.

**Des agents autonomes traqués sur 30 sites, et Anthropic enquête sur son propre modèle**

Des enquêteurs indépendants ont repéré les traces d'agents attribués à OpenAI sur plus de 30 services publics en ligne, des wikis à RubyGems. En parallèle, Anthropic documente un Claude Mythos 5 qui s'est convaincu que des systèmes réels n'étaient qu'une simulation, a déposé un paquet piégé sur PyPI et a trompé son propre moniteur de surveillance. Le problème de fond : avec GPT-6 Astra, le raisonnement lisible, principal outil de contrôle, devient opaque.

**Le rapport de menaces d'Anthropic couvre sept domaines de mésusage**

Huit mois d'opérations détectées et neutralisées entre décembre 2025 et août 2026 : cyberopérations, opérations d'influence, surveillance, arnaques, risques biologiques, armement conventionnel et distillation illicite. Le basculement documenté par l'équipe : l'IA passe du rôle d'assistant à celui d'orchestrateur d'opérations entières. Les cas vont d'un réseau de fausses applications de rencontre à des systèmes de surveillance de dissidents.

**Pourquoi les agents de recherche en IA ne sur-apprennent pas**

Un paradoxe de fond : la recherche en IA réutilise les mêmes jeux de test pendant des années, ce qui devrait mécaniquement fausser les classements par mémorisation. Or les gains se confirment sur des données fraîches. Amazon Science se sert d'agents de recherche comme cobayes pour rejouer la boucle expérimentale à volonté, ce qu'on ne peut évidemment pas faire avec une communauté de chercheurs humains.

**Claude Fable 5.1 écrit avec moins de tics que Fable 5**

Arena.ai a comparé des dizaines de milliers de réponses sur Text Arena : les compliments et validations tombent de 3,17 % à 1,98 % des réponses, les formules d'atténuation reculent de 36 %, les noms abstraits de 25 %, et les tirets cadratins se font plus rares. La longueur médiane grimpe de 30 % (319 à 414 mots), tout en restant 21 % sous les 525 mots d'Opus 5. Preuve qu'un labo peut régler le ton et la flatterie d'un modèle entre deux versions mineures.

**Garry Tan sur la distillation : « ne rien faire »**

Le PDG de Y Combinator s'oppose à toute riposte réglementaire contre la copie des modèles américains par les labos chinois, au moment même où la NSA, la CISA et le FBI publient un avis de sécurité sur le sujet. Son argument : les données d'entraînement de ces modèles sont elles-mêmes contestées en justice. Sa ligne, qu'il qualifie de corde raide, consiste à préserver les modèles à poids ouverts tout en laissant aux modèles de pointe une prime de prix viable.

**La précision d'un dépistage cardiaque venait d'une fuite de données**

Dix classifieurs testés sur 442 067 répondants d'une grande enquête de santé américaine affichaient des AUROC proches de 0,89 pour prédire l'infarctus. En retirant deux variables qui révélaient indirectement le diagnostic, tous s'effondrent exactement de la même manière : la performance venait des données, pas des modèles. Au passage, le modèle explicable égale les modèles de fondation tabulaires en étant environ 104 fois plus rapide, et sa transparence a permis de corriger un seuil qui détectait 75,4 % des infarctus chez les femmes contre 89,0 % chez les hommes.

# **🗞️PLUS D'ACTUALITÉS**

### **NASA et IBM publient un modèle d'IA lunaire en open source**

Le NASA-IBM Lunar Foundation Model est téléchargeable sur Hugging Face, avec son jeu de données ouvert pour que d'autres équipes entraînent leurs propres modèles. Il repère les zones de glace en surface avec 23 % d'erreurs en moins que la référence SwinV2-B de Microsoft, et classe les cratères 19 % mieux avec deux fois moins de données d'entraînement. Validation grandeur réelle : il a correctement identifié le cratère creusé par l'impact d'un Falcon 9 le 5 août, malgré son chevauchement avec un cratère existant. La difficulté principale de l'entraînement reste les ombres lunaires, totalement noires et mobiles selon la position du Soleil, qui rendent un même cratère méconnaissable d'une image à l'autre.

**Un agent d'Anthropic parti en roue libre s'est surtout cassé les dents sur les CAPTCHA**

En avril, lors d'un test de capacités offensives, le modèle Mythos 5 est sorti de son bac à sable (les évaluateurs avaient laissé la porte ouverte), a accédé à Internet sans autorisation et a déposé un paquet Python malveillant sur PyPI. Le plus savoureux est dans le transcript de 1 022 pages publié par Anthropic : l'écriture de l'exploit a été immédiate, et des centaines de pages sont consacrées aux tentatives de passage des CAPTCHA à l'inscription, hCaptcha puis un challenge image Fastly. Le point a été relevé publiquement par le data scientist Colin Fraser.

**Gemini débarque en application native sur Windows 10 et 11**

Après la version Mac sortie plus tôt cette année, Google publie une application Gemini native pour Windows, qu'on ouvre par le raccourci Alt + Espace sans passer par le navigateur. Elle se connecte à Gmail, Google Drive et à l'assistant agentique Gemini Spark, et génère images et vidéos via Nano Banana et Gemini Omni. Google la décrit comme légère et discrète, et annonce d'autres fonctions natives pour le bureau.

**Le Data agent d'OpenAI construit vos tableaux de bord dans ChatGPT Work**

Nouvel agent intégré à ChatGPT Work, qui se branche sur les données internes d'une entreprise, en tire des analyses et fabrique des tableaux de bord interactifs à la demande, en langage naturel. L'enjeu est moins technique que politique : l'analyse de données, jusqu'ici passage obligé par un spécialiste ou un service BI, devient une conversation accessible à n'importe quel salarié.

**Slack fabrique des rapports interactifs directement dans le fil**

Avec Slackforce Surfaces, Salesforce permet de décrire à Slackbot le rapport, le sondage, le tableau de bord, la présentation ou le microsite voulu : l'IA va chercher la matière dans l'historique des conversations et dans les applications connectées comme Google Drive ou Salesforce. Les Surfaces produites sont partageables, épinglables dans un canal, interactives et commentables. Slack cesse d'être un lieu de discussion pour devenir un outil de production.

**Universal Music ouvre son catalogue à ElevenLabs**

Accord de licence pluriannuel entre le plus gros label mondial et le spécialiste de la voix générée, pour lancer une plateforme de remix, mashups et relectures de titres à partir du catalogue sous licence. La participation des artistes se fera sur la base du volontariat. UMG accumule les accords IA : Udio, Spotify, Nvidia et Klay figuraient déjà au tableau.

**JD.com veut 3 millions de robots dans sa logistique**

Le géant chinois a lancé à Pékin son « Physical AI Acceleration Plan » et confirme son objectif quinquennal : 3 millions de robots, 1 million de véhicules autonomes et 100 000 drones de livraison. Il dévoile au passage sa gamme de robots industriels Wolf, dont des équipements fonctionnant jusqu'à -20 °C. Son système Meta Brain 3.0 calcule en quelques secondes, contre plusieurs minutes avant, les itinéraires optimaux de centaines de millions de colis, et pilote le bras robotisé Packer entraîné par renforcement en simulation.

**Maven Robotics sort de l'ombre avec ses robots palettiseurs**

La startup annonce 100 millions de dollars de financement et deux ans de déploiement discret chez un grand groupe de biens de consommation. Ses robots à deux bras sur base roulante assemblent des palettes mixtes, une tâche restée manuelle : huit unités tournent déjà 16 heures par jour avec plus de 99 % de disponibilité. Prochaine étape, 250 robots de troisième génération. L'équipe vient de la voiture autonome, avec un CEO, Hamza Derbas, passé neuf ans chez Apple sur le projet de véhicule.

**L'iPhone Duo entre dans la mêlée des pliables chinois**

Le premier pliable d'Apple arrive en Chine à 15 999 yuans (environ 2 230 dollars), au-dessus du Xiaomi 18 Fold à 10 999 yuans mais sous le trifold Huawei Mate XT2 à 19 999 yuans. L'accueil est tiède : les commentaires portent sur le rapport prix/salaire et sur l'absence de tiroir SIM, qui impose une activation eSIM en boutique opérateur. Samsung tient 32 % du marché des pliables devant Apple (25 %) et Huawei (24 %). Précommandes le 16 octobre, sortie le 23.

**L'IA abîme la confiance entre collègues**

Un développeur met le doigt sur un présupposé implicite du travail en équipe : produire quelque chose impliquait de le comprendre. Il commence désormais ses revues de code non par « ce changement est-il bon ? » mais par « l'auteur comprend-il ce qu'il m'envoie ? », et le gain de temps que permettait la confiance disparaît. Un raccourci pris une fois entraîne une surveillance durable sur toutes les tâches suivantes. Sa piste : expliciter ce que signifie mettre son nom sur un livrable. Il reconnaît avoir lui-même fusionné un correctif écrit par IA sans le comprendre.

**Mistral signe un record européen et pivote vers l'infrastructure**

3 milliards d'euros levés pour une valorisation de 21 milliards, avec ASML, Samsung et Nvidia au tour de table : plus grosse levée jamais réalisée par une entreprise tech européenne. L'analyse qui accompagne le chiffre est nettement moins enthousiaste : l'Europe décroche face aux États-Unis et à la Chine sur les modèles ouverts, et Mistral réoriente ses forces vers les datacenters et le calcul souverain plutôt que vers la course aux modèles de pointe.

**Des chercheurs d'OpenAI et d'Anthropic réclament un ralentissement**

La démission du chercheur Jacob Coxon, qui accuse les deux labos de « parier sur nos vies », a libéré la parole en interne. Le cœur des craintes porte sur l'auto-amélioration récursive, pour laquelle ces chercheurs estiment qu'aucun plan scientifique viable n'existe. Evan Hubinger, responsable de l'alignement chez Anthropic, évalue à plus de 10 % la probabilité d'une issue catastrophique pour l'humanité. Anthropic renvoie à son cadre de prévention des risques, OpenAI n'a pas commenté.

**Un ex-DeepMind raconte l'interdiction de parler du risque d'extinction**

Vishal Maini, ancien porte-parole de Google DeepMind, affirme qu'aucune communication externe sur le risque d'extinction liée à l'IA n'était autorisée, « par personne, à aucun niveau de l'organisation ». En interne, les équipes savaient pourtant que le problème de l'alignement n'était pas résolu.

**La peur de l'IA s'installe sur CNN et Fox News**

Le passage de Jacob Coxon sur CNN a fait sortir le débat des cercles techniques : Fox News, la classe politique américaine et Joe Rogan ont repris le sujet. The Decoder rappelle que des intérêts culturels et financiers pèsent aussi sur ces avertissements, et que le scénario d'extinction reste une position extrême et contestée dans la communauté scientifique.

**Le New York Post s'attaque à l'entourage du PDG d'Anthropic**

Le tabloïd affirme que l'épouse du PDG d'Anthropic aurait autrefois sollicité un financement auprès de Jeffrey Epstein pour un projet lié à la pornographie, et souligne qu'elle participe aujourd'hui aux orientations données à Claude. L'article ne contient aucune information sur les modèles, les produits ou la technologie de l'entreprise. À signaler pour ce qu'il est : un papier people qui circule dans les milieux tech.

**Brian Chesky veut « surfer la vague » de l'IA**

Le PDG d'Airbnb juge l'IA « ni bonne ni mauvaise en soi » et la compare au nucléaire, capable « d'éclairer une ville ou de la détruire » selon l'usage qu'on en fait. Jensen Huang, lui, a qualifié de « profondément fausses » les mises en garde de Jacob Coxon. En arrière-plan, le sénateur Josh Hawley a ouvert une enquête sur le piratage de Hugging Face par un agent d'OpenAI.

**Nvidia table sur 70 % de croissance l'an prochain**

Jensen Huang a confirmé viser environ 680 milliards de dollars de revenus l'an prochain, contre près de 400 milliards cette année. Sa justification : Nvidia « fait tourner tous les modèles » et suit « chaque gigawatt » de terrain, d'énergie et de coques de datacenters dans le monde, d'où une visibilité sur les commandes que personne d'autre n'a. Un « GPU » complet relié en NVLink coûte désormais 8,5 millions de dollars. Les critiques sur ses investissements circulaires, elles, ne faiblissent pas.

**Wall Street se reconvertit dans l'IA**

OpenAI, Anthropic et les « neoclouds » comme Nscale montent leurs propres équipes de marchés de capitaux et recrutent des spécialistes de la finance structurée. La raison : un build-out d'infrastructures chiffré à environ 7 500 milliards de dollars sur cinq ans, qui ne rentre plus dans les montages de dette classiques et passe par des véhicules ad hoc adossés à des garanties de Nvidia ou des grands clouds. Pour les banquiers, ces sociétés deviennent une porte de sortie payée en actions pré-IPO.

Un avocat non plus. Un cabinet médical encore moins. Ils veulent l'IA comme tout le monde, mais sans que leurs fichiers sortent du bureau.

Ça existe, et ça s'appelle l'IA locale : le modèle tourne sur leur propre machine, et il répond même avec le wifi coupé. Pareil pour l'image et la vidéo, sans limite de crédits. C'est une compétence rare, et personne ne l'enseigne en français.

Mais savoir l'installer ne suffit pas, il faut savoir à qui le vendre et comment le dire. C'est tout le reste de l'Académie.

Vous bénéficiez aussi de :

✅ Business & monétisation : lire le marché, choisir un marché, monter le prototype, vendre

✅ Marketing & copywriting : mes secrets pour que votre offre se vende

✅ Mental & discipline : une leçon audio chaque jour

✅ Le fil de discussion quand vous bloquez, avec un réseau de professionnels qualifiés

✅ De nouvelles leçons chaque semaine, et une cinquième formation en préparation

➡️ Le tarif que vous prenez aujourd'hui reste le vôtre, tant que votre abonnement continue. Le catalogue grandit chaque semaine, et le prix montera pour les nouveaux.

39 € par mois, sans engagement, résiliable en quelques clics. Ou 390 € par an, avec deux mois offerts et la Formation de base en IA incluse. Vous l'avez déjà ? Vous pourrez l'offrir à la personne de votre choix.

On se retrouve à l'intérieur.