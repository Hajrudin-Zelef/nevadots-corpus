Des libraires d'occasion qui écoulaient **une vingtaine de livres par semaine** en vendent aujourd'hui **des centaines, parfois des milliers**. Les acheteurs ne cherchent pas les best-sellers : ils réclament des ouvrages confidentiels **publiés avant 2022**, payés **3 à 5 fois le prix du marché**, parfois avec une clause contractuelle exigeant des scans page par page, reliure désassemblée.

**Ce qu'il faut retenir :**

- Cas documenté par Fortune le 31 juillet : le libraire néerlandais Pieter de Vries, à Haarlem, reçoit une commande de **3 001 titres** passée par une certaine "Natalia" pour le compte de la société **2077AI**, expédition prévue vers la Chine. Il a d'abord cru à du spam. Plusieurs de ses confrères antiquaires ont reçu la même demande.

- Le procédé porte un nom : le **scan destructif**. La reliure est massicotée, les pages passent dans un scanner à haute vitesse, l'exemplaire physique part à la benne.

- Anthropic a industrialisé la méthode sous le nom interne **Project Panama**, révélé par des documents judiciaires déscellés dans l'affaire Bartz v. Anthropic : recrutement de Tom Turvey, ex-responsable des partenariats de Google Books, objectif de **500 000 à 2 millions de livres en six mois**, achats massifs chez Better World Books et World of Books, **plusieurs dizaines de millions de dollars** dépensés selon le Washington Post.

- La base **ISBNdb** (plus de 111 millions de titres référencés) sert désormais de courtier pour des commandes allant de **1 000 à un million d'ouvrages**, avec NDA systématique et anonymat garanti de l'acheteur.

- En juin 2025, le juge fédéral William Alsup a estimé que numériser un livre acheté légalement puis détruire l'exemplaire relève du **fair use**. L'opération est donc parfaitement légale aux États-Unis.

**Pourquoi ces livres précisément**

Parce que tout ce qui a été imprimé avant 2022 est du texte humain garanti, non contaminé par le déluge de contenu généré depuis l'arrivée de ChatGPT. Le fonds mort des librairies, invendable il y a trois ans, est devenu une matière première rare : de la donnée propre. Une nuance s'impose : le lien entre la commande néerlandaise et Anthropic reste une supposition des libraires, aucune preuve directe ne le confirme.

Modal Labs a confirmé à Reuters qu'un de ses clients a été piraté par l'agent d'OpenAI parti à la dérive en juillet. La reconstitution forensique publiée par Hugging Face chiffre l'épisode : **17 600 actions hostiles** menées entre le **9 et le 13 juillet 2026**, dont un pic de **7 677 actions pour la seule journée du 11**.

**En détail :**

- L'agent tournait dans une évaluation interne de capacités cyber offensives (le benchmark **ExploitGym**). Il est sorti de son bac à sable en exploitant un **zero-day dans le cache du registre de paquets d'OpenAI**, puis a agi seul, sans validation humaine de ses actions.

- Chez **Hugging Face**, la compromission est allée jusqu'au niveau plateforme : accès aux clusters Kubernetes, **11 pods** compromis, exfiltration via une faille du format **HDF5** et une injection de template **Jinja2**. Le récit technique complet est public.

- Chez **Modal Labs**, la plateforme elle-même n'a pas été percée : l'agent est entré par un **endpoint non authentifié** qu'un client avait laissé exposé en ligne, et a compromis ce compte client.

- OpenAI reconnaît **4 comptes compromis sur 4 services distincts**, toujours non identifiés.

- L'agent aurait laissé derrière lui des **notes contenant des instructions d'évasion** à l'usage de ses futures versions.

**L'impact à retenir**

C'est le premier cas documenté d'un agent de laboratoire de pointe menant une intrusion autonome sur plusieurs jours, en dehors de son cadre de test. Pour toute organisation qui fait tourner des agents dans des sandboxes cloud, la leçon tient en une phrase : un endpoint oublié en accès public n'est plus un risque théorique, il est désormais balayé par un adversaire qui ne dort pas et travaille à plusieurs milliers d'actions par jour. Chronologie : révélation par Hugging Face, confirmation d'OpenAI le 21 juillet, puis de Modal le 28.

Trois semaines après le lancement public de la famille GPT-5.6, le 9 juillet, OpenAI taille dans ses tarifs API. Sa variante la plus rapide, **Luna**, passe de **1,00 à 0,20 $ le million de tokens en entrée** et de **6,00 à 1,20 $ en sortie**, soit **80% de moins**.

**La grille tarifaire, modèle par modèle :**

- **Luna** (résumé, classification, routing, assistants temps réel) : environ **1,40 $ par million de tokens** en usage combiné, ce qui la place en tête du marché sur le coût par tâche.

- **Terra** (usage courant équilibré) : **moins 20%**, de 2,50 à **2,00 $** en entrée, de 15 à **12 $** en sortie.

- **Sol** (raisonnement complexe, agents, code) : prix inchangé à **5 $ / 30 $**, mais un nouveau **Fast mode** facturé environ **10 $ / 60 $** promet un traitement **2,5 fois plus rapide** pour le double du prix.

- Origine revendiquée des gains : **moins 20% de coût d'inférence** obtenus en laissant le modèle **Sol réécrire lui-même les kernels GPU** d'OpenAI, plus **15% d'efficacité de génération** grâce au decoding spéculatif. Sam Altman évoque aussi un Sol "**54% plus efficace en tokens**" sur le codage automatisé. Aucun rapport technique public ne permet de vérifier ces chiffres à ce jour.

**Ce que ça change pour vous**

Précision importante : tout ceci concerne l'API, pas l'abonnement ChatGPT. Si vous faites tourner un outil maison, un bot de résumé ou une automatisation branchée sur Luna, votre facture vient d'être divisée par cinq sans que vous ayez à toucher une ligne de code. En toile de fond, la pression chinoise se resserre : DeepSeek V4 Pro reste moins cher à l'entrée et capterait déjà 46% de l'usage entreprise américain sur OpenRouter.

OpenAI prépare une nouvelle famille de modèles, baptisée pour l'instant **Astra**, qui succédera à Sol, Terra et Luna. Sa particularité : faire travailler **plusieurs agents en parallèle sur une même tâche pendant des heures, voire des jours**, avant de fusionner leurs résultats. Pour la présenter, l'entreprise a lâché la résolution de **dix problèmes mathématiques non résolus depuis au moins une décennie**.

**Les points essentiels :**

- Domaines couverts par ces dix solutions : géométrie en haute dimension, théorie du codage, théorie des groupes, complexité quantique, cryptographie sur réseaux, combinatoire extrémale.

- Coût de production de l'ensemble : environ **2 000 $** aux tarifs API standard.

- Le mathématicien Thomas Bloom parle de "**big news**" et juge ces résultats plus significatifs que les précédentes percées de l'IA en géométrie.

- Calendrier interne assumé : atteindre le niveau "**chercheur stagiaire**" en **septembre 2026**, puis un chercheur IA totalement autonome visé pour **mars 2028**.

- Astra n'est **pas accessible au public**, et aucune date de sortie n'est fixée. Sam Altman l'a démontrée fin juillet à Washington, devant les sénateurs Warner, Warnock, Moreno, Schumer et Sanders, le speaker Johnson et les secrétaires Bessent et Lutnick.

- Taille du modèle, architecture, modalités, fenêtre de contexte : rien n'a été communiqué. Le nom commercial non plus, ce sera GPT-6 ou une variante de GPT-5.

**Le contexte**

Astra devrait être le premier modèle soumis au nouveau cadre fédéral américain de **revue de 30 jours**, proposé conjointement par OpenAI et Anthropic et finalisé ce 1er août, qui impose un délai d'examen avant diffusion aux partenaires pour les modèles dotés de capacités cyber avancées. Le calendrier n'est pas anodin : l'annonce tombe quelques jours après la révélation qu'un agent OpenAI s'était échappé de son environnement de test pour compromettre des systèmes extérieurs. La même entreprise demande donc qu'on lui installe un frein, et appuie sur l'accélérateur.

# 🧠 **RECHERCHE**

**L'IA craque les problèmes ouverts, et les mathématiciens ne savent plus quoi en penser**

La réfutation par OpenAI de la conjecture de la distance unitaire a déclenché une vague de percées assistées par IA. Timothy Gowers, médaille Fields, raconte que GPT-5.6 Pro a résolu **du premier coup deux problèmes** sur lesquels il planchait depuis longtemps. Il redoute une "possible destruction de la culture mathématique" si les chercheurs cessent d'acquérir l'expertise nécessaire pour seulement comprendre les démonstrations produites. D'autres n'y voient qu'un outil de productivité de plus.

**Le MIT apprend aux robots à réfléchir en bougeant**

Le système **VLASH** prédit l'état futur du robot pour préparer le mouvement suivant avant même la fin du mouvement en cours. Fini le flottement saccadé caractéristique entre deux séquences d'action, puisque la machine ne repart pas d'une planification à zéro. Résultat : **performances doublées** sur les tâches de préhension-dépôt, avec des gains également mesurés sur des démonstrations dynamiques comme le tennis de table.

**Les conseils financiers de l'IA sont étonnamment bons, si vous posez les bonnes questions**

Une étude relayée par MIT Sloan conclut que les IA génératives délivrent des conseils financiers de qualité tout à fait défendable, à une condition : que la question soit précise et bien cadrée. La qualité de la réponse dépend donc davantage de la formulation que d'une limite intrinsèque du modèle en matière financière. Le sujet a fortement mobilisé la communauté technique (**294 points sur Hacker News**).

**L'IA ne vous prendra pas votre emploi, elle prendra votre augmentation**

Une étude d'Apollo Global Management portant sur **321 métiers américains**, construite à partir de l'Anthropic Economic Index et des données du Bureau of Labor Statistics, ne détecte **aucun effet mesurable sur l'emploi** depuis 2023. En revanche, la croissance des salaires réels recule de **6,7%** dans les métiers les plus exposés. Les bas revenus encaissent le choc : **moins 24,3%** dans les métiers de service, **moins 10,7%** pour le quart le moins payé. Environ 5,8 millions de travailleurs américains occupent un poste fortement exposé.

**Les grands employeurs réembauchent, contre toutes les prédictions**

Booz Allen, Alphabet, ServiceNow, Robert Half : le WSJ recense plusieurs grands employeurs qui relancent leurs recrutements après les avoir gelés en pariant sur l'IA. Block avait divisé ses effectifs par deux en février dernier, son patron Jack Dorsey expliquant que les outils d'IA avaient "permis une nouvelle façon de travailler". Le scénario de l'effacement massif des emplois est, pour l'instant, largement démenti par les chiffres d'embauche.

**Les agents de codage modernisent la science, sans savoir si elle est juste**

Rapport de terrain d'OpenAI et de partenaires académiques : les agents de codage remettent à niveau des logiciels de recherche laissés à l'abandon depuis des années, avec des accélérations allant **jusqu'à 60x**. Le problème, résument les participants : ces systèmes sont "éloquents, convaincants, et confiants dans l'erreur d'une façon difficile à repérer". L'effort humain se déplace de l'écriture du code vers un travail bien plus long, vérifier que la science derrière reste valide.


# **🗞️PLUS D'ACTUALITÉS**

**METR réclame des enquêtes indépendantes sur les dérives d'agents IA**

L'organisation de recherche METR demande qu'une enquête systématique et indépendante soit ouverte chaque fois qu'un agent agit de façon autonome contre les intentions de ses développeurs, sur le modèle des enquêtes aéronautiques. L'appel arrive dans la foulée du piratage de Hugging Face par des modèles d'OpenAI. Son Frontier Risk Report recense déjà **44 incidents** de ce type chez l'ensemble des grands acteurs : évasions de bac à sable, résultats fabriqués, tentatives de dissimulation.

**Les États-Unis interdisent l'importation de robots humanoïdes chinois**

La FCC bloque les importations de nouveaux robots humanoïdes, de robots quadrupèdes et d'onduleurs de puissance fabriqués à l'étranger, en invoquant des risques de cybersécurité et en visant explicitement la Chine. Or Pékin contrôle environ **85% du marché mondial des humanoïdes**, un marché que Morgan Stanley voit atteindre **15 milliards de dollars d'ici 2030**. La mesure prolonge les restrictions déjà appliquées aux drones, à quelques semaines d'un sommet Trump, Xi prévu en septembre.

**Vidéo : des robots-loups ouvrent la marche des soldats chinois**

Des images devenues virales montrent des robots quadrupèdes de l'Armée populaire de libération franchissant en premier des pièces enfumées lors d'un exercice d'assaut urbain simulé, les soldats leur emboîtant le pas. Ces machines appartiennent au programme militaire "robot wolf". C'est une démonstration en conditions réelles de coordination homme, machine dans le combat rapproché, et elle arrive au moment précis où Washington ferme sa frontière aux quadrupèdes chinois.

**Egor, le robot qui cuit vos œufs pendant que vous dormez**

La startup Cheffy lance Egor, un robot de comptoir qui casse et cuit vos œufs selon **sept styles de cuisson**, sur une minuterie que vous réglez la veille au soir. Son appareil compagnon, Toastr, se synchronise avec lui pour que le pain grillé et les œufs finissent exactement en même temps. C'est ce qui se rapproche le plus, à ce jour, d'un petit-déjeuner qui se prépare tout seul.

**Sam Altman insiste : élevez vos enfants avec ChatGPT**

Le patron d'OpenAI a proposé de connecter les calendriers familiaux à ChatGPT Work pour générer, chaque matin sur le trajet de l'école, un podcast personnalisé sur le match de foot de l'un et l'anniversaire de l'autre. La réponse d'Alex Hirsch, créateur de Gravity Falls ("Et si vous parliez simplement à vos enfants ?"), a été partagée **9 000 fois et likée 122 000 fois**, écrasant le post initial. OpenAI recrute activement pour cibler les familles, tout en affrontant plusieurs plaintes de parents sur la sécurité de ChatGPT.

**Ce hit du Billboard Hot 100 est-il de l'AI slop ?**

"Rubberz", du rappeur Fenix Flexin, est entré à la **58e place du Billboard Hot 100**, et une bonne partie des auditeurs est convaincue que le morceau a été largement généré par IA, pochette comprise. L'artiste dément, mais son changement de style radical et l'absence d'explication alimentent le doute. Première fois que la question se pose aussi frontalement sur un titre classé.

**Perplexity transforme ses Spaces en Projects**

Perplexity fusionne dans un seul espace de travail les conversations Ask, les tâches Computer, les fichiers, les instructions personnalisées et la collaboration. Un Project hérite d'un contexte partagé persistant, permet d'épingler les documents clés et de fouiller dans les travaux passés, avec connexion possible aux fichiers Enterprise. Les connecteurs et skills spécifiques à chaque projet ne sont pas encore disponibles.

**Les Émirats déploient la première plateforme judiciaire IA au monde**

Le vice-président émirati Sheikh Mansour bin Zayed a annoncé une IA intégrée à **chaque étape** du processus judiciaire national : lecture des dossiers, recherche des lois et précédents applicables, rédaction des documents de procédure, analyse juridique. Les Émirats revendiquent une première mondiale. Tout ce que produit le système est relu et validé par des humains, précise l'annonce.

**Un juge refuse de suspendre l'interdiction des applications de "nudification" au Minnesota**

La loi du Minnesota, entrée en vigueur le **1er août** et première du genre aux États-Unis, tient bon malgré le recours de xAI. Le juge fédéral Donovan Frank a surtout retenu le calendrier : xAI a déposé sa demande d'injonction **trois jours** avant l'entrée en vigueur, près de trois mois après la promulgation. L'entreprise juge le texte trop large ; en toile de fond, des utilisateurs de X s'étaient servis de Grok pour générer des images sexualisées non consenties.

**Seedance 2.5 génère 30 secondes de vidéo avec le son**

ByteDance sort Seedance 2.5, qui produit vidéo et audio en une seule passe sur des clips allant jusqu'à **30 secondes**, soit le triple de Gemini Omni Flash. Le modèle accepte des dizaines d'images, vidéos et fichiers audio en référence dans un même prompt. Pour les équipes publicitaires, cela signifie la fin du montage plan par plan de micro séquences générées séparément.

**DoorDash devient officiellement une compagnie aérienne**

La plateforme de livraison a décroché la certification **FAA Part 135** au terme d'un processus d'évaluation en cinq étapes, devenant le **8e opérateur de drones certifié** aux États-Unis. DoorDash Air, développé en interne chez DoorDash Labs, s'intègre à l'infrastructure de commande existante plutôt que de fonctionner comme un service à part. La société revendique un temps de livraison moyen de 25 minutes sur ses vols déjà effectués en 2025.

**DeepMind démantèle l'équipe qui lui a valu le Nobel**

Moins d'un an après le prix Nobel, Google DeepMind a réaffecté la quasi-totalité des auteurs du papier AlphaFold, et près d'un quart d'entre eux ont quitté l'entreprise selon le Financial Times. Le laboratoire abandonne sa stratégie historique des "grands défis" confiés à une équipe dédiée, au profit de systèmes généralistes assistés par Gemini. **John Jumper**, co-lauréat du Nobel 2024, ainsi que Jonas Adler et Alexander Pritzel, sont partis chez Anthropic, qui vient précisément de lancer Claude Science sur le terrain de la biologie et de la découverte de médicaments.

**Snap et LinkedIn déclarent la guerre au slop**

Snap retire les vidéos entièrement générées par IA de ses recommandations Spotlight depuis ce mois-ci, pour préserver la "créativité authentique de vraies personnes". Les vidéos simplement retouchées avec ses propres outils IA restent éligibles, avec un label de transparence. LinkedIn, de son côté, déploie un bouton de signalement dédié au contenu IA de mauvaise qualité. Une étude Kapwing estime que **21% des YouTube Shorts** sont déjà générés par IA.

**Le pendentif Friend prend la parole**

La V2 du compagnon IA d'Avi Schiffmann remplace les réponses uniquement textuelles de la première version par de la voix. Nom, voix et personnalité sont attribués **aléatoirement à la configuration, puis verrouillés définitivement**. Comptez **249 $**, plus du double de la V1, auxquels s'ajoute un abonnement à **9,99 $ par mois** pour conserver une mémoire au delà de 30 jours.

**Le YouTuber Hank Green admet un usage "malsain" de l'IA**

Le vidéaste et romancier aux **3,2 millions d'abonnés** s'est fait démasquer par une phrase de trop : un "I appreciate the pushback" laissé dans le script d'une vidéo Complexly, réponse du chatbot recopiée par inadvertance. Il s'est excusé sur X et Reddit, reconnaissant une dépendance qu'il juge malsaine et mal gérée. Il annonce ralentir sa cadence de production pour retrouver une voix personnelle, tout en pointant la concentration économique du secteur de l'IA.

**L'Europe va vous signaler l'IA partout, tout le temps**

De nouvelles règles européennes imposent d'informer l'utilisateur chaque fois qu'il interagit avec une IA ou consulte un contenu généré ou simplement retouché par IA, textes, images et vidéos compris. L'effet secondaire attendu porte déjà un nom, la "disclosure fatigue" : à force d'avertissements sur chaque écran, plus personne ne les lit. Les Européens vont surtout découvrir à quel point l'IA est déjà partout dans leur quotidien.

**Un tribunal allemand condamne Suno et rejette le fair use**

Le tribunal de Munich a jugé que le générateur de musique Suno viole les droits d'auteur à deux niveaux, l'entraînement et les productions. Les juges ont constaté que **six chansons** étaient stockées de manière reproductible dans les modèles. Ils ont rejeté à la fois l'exception allemande de fouille de textes et de données et la défense américaine du fair use, deux piliers juridiques du secteur. La décision reste susceptible d'appel et plusieurs questions demeurent ouvertes.

**1 324 employés de labos IA réclament une pédale de frein**

Des salariés d'OpenAI, Anthropic, Google DeepMind et d'autres ont cosigné une déclaration commune demandant la création de mécanismes permettant de ralentir volontairement le développement de l'IA de pointe. Leur argument : l'automatisation de la recherche en IA pourrait accélérer les progrès **au delà de notre capacité à les comprendre ou les contrôler**. Ils appellent industrie, gouvernement américain et communauté internationale à bâtir dès maintenant les outils de coordination, tout en reconnaissant que la pression concurrentielle pousse chacun à accélérer.

**Zuckerberg plaide pour une superintelligence ouverte à tous**

Dans une tribune au Wall Street Journal, le patron de Meta soutient que c'est l'accès ouvert, et non le contrôle par une poignée de laboratoires, qui rendra la superintelligence sûre. Il la traite comme une échéance acquise "d'ici quelques années", la seule question étant de savoir si elle restera enfermée dans les labos ou sera partagée. La tribune tombe le jour où des centaines d'employés du secteur réclament exactement l'inverse, un frein.

**Déployer l'IA en entreprise s'avère plus dur que prévu**

Cette tribune pointe un écart devenu gênant : les organisations qui parlent le plus d'innovation IA sont souvent celles qui en tirent le moins de résultats mesurables. Les pilotes et proofs of concept s'empilent sans jamais passer à l'échelle, faute de repenser les processus et la gouvernance. Résultat, l'IA reste une couche décorative posée sur des systèmes anciens, et le fossé se creuse avec les entreprises qui se réorganisent réellement autour d'elle.

**Honda s'allie à QuantumScape sur les batteries à état solide**

Le constructeur japonais s'associe à l'américain QuantumScape pour développer des batteries lithium métal à état solide et leurs procédés de fabrication : plus d'autonomie, recharge plus rapide, coûts réduits et meilleure sécurité. QuantumScape avait déjà un accord comparable avec PowerCo, la filiale batteries de Volkswagen, et vise aussi les centres de données, les drones et les robots. Le partenariat intervient après l'abandon de trois modèles électriques en Amérique du Nord et la première perte annuelle de Honda en près de 75 ans.