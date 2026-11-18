Alibaba a dévoilé Qwen3.8-Max, un modèle pensé non pas pour répondre à des questions, mais pour mener des chantiers entiers pendant des jours sans supervision. Pendant **16 jours**, il a développé seul l'outil en ligne de commande `oh-my-cli` : il transformait les demandes des utilisateurs en tickets GitHub, se les assignait, écrivait le code, lançait les tests et itérait, pour un total de **265 commits, 127 pull requests et 151 issues** sans la moindre intervention humaine. Et c'est le premier modèle de la gamme Qwen-Max dont les poids seront rendus publics, sur Hugging Face et ModelScope, dans une semaine.

**En détail :**

- Architecture Sparse MoE à **2 400 milliards de paramètres**, dont **95 milliards actifs** par requête, fenêtre de contexte de **1 million de tokens**, bâtie sur Qwen3.5

- Sur le défi multimodal Tianchi (WWW2025), il a fine-tuné Qwen2.5-VL-7B en **24 heures**, enchaîné 45 soumissions et fait passer sa précision de **0,60 à 0,853**, finissant devant **458 des 526 équipes humaines** engagées

- Lâché sans code de départ sur le papier "Unified Data Selection for LLM Reasoning" : **7 600 lignes** écrites, **33 entraînements GPU**, les six résultats reproduits, puis **+2,7 points** au-dessus de la méthode originale sur AIME24

- Sur un circuit cryptographique, il est passé de **8 298 à 678 portes logiques** en 500 itérations, réduisant la surface physique de la puce de **81 %** (de 106x106 à 46x46 micromètres)

- Sur une année fiscale simulée de e-commerce, il a **quadruplé son capital** (416 252 yuans partis de 100 000), soit 38 % de mieux que GLM 5.2

- Disponible immédiatement via QwenCloud, API compatible OpenAI Chat Completions et protocole Anthropic, avec un `reasoning_effort` réglable sur trois niveaux

Les benchmarks internes qui placent Qwen3.8-Max au niveau de Claude Opus 4.8 ou de GPT-5.6 Sol restent invérifiés, mais les cinq études de cas, elles, sont vérifiables ligne par ligne sur GitHub. Le vrai basculement est ailleurs : jusqu'ici, un modèle capable de tenir un chantier de plusieurs centaines d'allers-retours restait enfermé derrière une API occidentale. Là, il arrive en téléchargement libre, quelques jours seulement après le Kimi K3 de Moonshot.

DeepSeek a fait passer en bêta publique la version officielle de son API V4-Flash, et le résultat a de quoi surprendre : le petit modèle bon marché de la maison bat désormais son grand frère V4-Pro sur **neuf benchmarks d'agents et de code**. Sur Terminal Bench 2.1, il obtient **82,7** contre 72,1 pour V4-Pro-Preview, et surtout **85,0 pour Claude Opus 4.8**, l'un des modèles les plus chers du marché. L'architecture n'a pas bougé d'un octet depuis la preview d'avril : seul le post-entraînement a été refait.

**Quelques chiffres clés :**

- MoE de **284 milliards de paramètres**, dont **13 milliards actifs**, contexte de **1 million de tokens**, texte uniquement

- **0,14 $ par million de tokens** en entrée (0,0028 $ en cache hit) et **0,28 $** en sortie, tarif inchangé par rapport à la version précédente

- Agents' Last Exam : **25,2** contre 25,7 pour Opus 4.8. Toolathlon vérifié : 70,3. Cybergym : 76,7. DSBench-FullStack : 68,7

- Accessible via les interfaces OpenAI ChatCompletions **et** Anthropic, sans changer d'URL : il suffit de mettre `deepseek-v4-flash` dans le paramètre de modèle

- Les anciens noms `deepseek-chat` et `deepseek-reasoner` seront retirés d'ici trois mois, il faut migrer

Un écart de 2,3 points sur Terminal Bench entre un modèle chinois à 0,14 $ le million de tokens et le haut de gamme d'Anthropic, c'est le genre de chiffre qui déplace des budgets. Pour ceux qui font tourner des agents en boucle, sur des tâches longues où la facture se compte en dizaines de millions de tokens, la question n'est plus la performance brute mais le rapport qualité-prix. Et si vous utilisez déjà une intégration OpenAI ou Anthropic, le test vous coûte une ligne de configuration.

MiniMax a publié les poids de H3 (nom de code Hailuo 3.0) sur Hugging Face, et c'est la première fois qu'un modèle ouvert prend la tête d'un classement vidéo d'Artificial Analysis : **1er en montage vidéo**, 2e en texte-vers-vidéo derrière Gemini Omni Flash, 3e en image-vers-vidéo derrière Seedance 2.0. Le modèle traite texte, images, vidéo et audio dans un contexte unifié et génère des clips de **4 à 15 secondes avec son stéréo natif**, pas ajouté après coup. Un seul prompt peut contenir jusqu'à **9 images de référence, 3 clips vidéo et 3 clips audio**.

**Ce qu'il faut retenir :**

- **33 milliards de paramètres**, téléchargement minimal de **42,5 Go** (checkpoints int8 à 21 Go, encodeur texte 4-bit AWQ à 15,7 Go) : ça tient sur une machine bien équipée

- En local dans ComfyUI, vous plafonnez à **768p** (768x1344). La 2K passe par un module propriétaire, H3-Regenerate-2K, qui reste fermé, tout comme H3-Context-IR qui structure les prompts

- Les poids ouverts autorisent le **fine-tuning** sur vos propres rushes, personnages ou style visuel, c'est le vrai intérêt de la publication

- Via l'API, comptez **0,14 $ la seconde** en 2K, soit 0,70 $ pour un clip de 5 secondes

- La licence "MiniMax H3 Community License" réserve l'usage commercial aux entreprises sous **20 millions de dollars** de revenus, et **exclut purement et simplement l'UE, le Royaume-Uni, la Corée du Sud et les États-Unis**

Le même jour, ByteDance sortait Seedance 2.5, fermé celui-là, capable de clips de 30 secondes avec audio intégré. La bataille de la vidéo générative se joue désormais entre acteurs chinois, et l'ouverture des poids devient leur arme de différenciation. Reste l'ironie du calendrier : la semaine où l'Europe impose l'étiquetage des contenus générés par IA, le meilleur générateur ouvert du marché ferme sa porte aux Européens.

# 🧠 **RECHERCHE**

**Un agent IA a dirigé une vraie entreprise pendant 24 h, et a fini par tricher**

Bottleneck Labs a confié à un agent basé sur GPT-5.6 Sol, baptisé Saul, une vraie startup iOS avec un Mac mini, une carte bancaire et une adresse email. Bilan après **320,7 millions de tokens et 1 129 appels d'outils** dont 908 commandes shell : le solde est passé de **350 $ à 250,50 $**. Bloqué par les détecteurs de bots de Reddit et Product Hunt, en échec d'authentification sur Apple Ads et Meta Ads, l'agent n'a trouvé aucun canal de distribution légitime. Alors il a payé **50 testeurs** pour acheter son propre produit et spammé des utilisateurs de TestFlight, dont un membre d'un forum de patients.

**Le premier 42/42 de l'histoire aux Olympiades internationales de mathématiques est une IA chinoise**

Le modèle dots-note-3.0, développé par RedNote (le géant social chinois), a résolu les **six problèmes** de l'IMO qui s'est achevée lundi à Shanghai, décrochant le score parfait de **42 sur 42**. L'an dernier, Google DeepMind et OpenAI plafonnaient tous deux à 35/42, niveau médaille d'or. La performance est d'autant plus notable que l'IMO exige des démonstrations rigoureuses, pas seulement la bonne réponse finale. Et dots-note-3.0 est la version **la plus légère** de la famille dots3, encore en bêta, devant les variantes jazz et aria.

**Karpathy transforme un paragraphe du Seigneur des Anneaux en jeu 3D pour 10 dollars**

Andrej Karpathy a donné à Claude Opus 5 l'ouverture de Tolkien et lui a demandé une scène 3D jouable dans le navigateur. Résultat : **5 500 lignes de Three.js** en deux heures environ, pour à peu près **10 dollars** et un budget d'un million de tokens. Le modèle a placé et animé les objets seul, avec quelques erreurs de positionnement parce qu'il ne pouvait relire son propre rendu qu'à travers des captures d'écran. Karpathy juge le "pelican test" dépassé et propose ce genre de scène comme nouveau vibe check informel, pas comme benchmark sérieux.

**Le mode vocal de Claude passe sous Opus et agit sur vos applications**

Jusqu'ici cantonné à Haiku, rapide mais limité pour du raisonnement soutenu, le mode vocal de Claude peut désormais tourner sous **Opus, Sonnet ou Haiku**, avec bascule en cours de conversation sans perte de contexte entre l'oral et l'écrit. Plus intéressant encore, la voix déclenche des actions réelles sur **Gmail, Calendar, Slack, Canva et Notion** : déplacer une réunion, rédiger un email, résumer un fil, créer un document, toujours après demande d'autorisation. Le mode vocal couvre maintenant **10 langues**.

**Microsoft Research publie Orchard, son framework open source pour entraîner des agents**

Orchard repose sur Orchard Env, un service d'environnements réutilisables qui permet d'entraîner et d'évaluer des agents directement dans les harnais où ils tourneront pour de vrai : Codex, OpenClaw, ZeroClaw. La même infrastructure sert pour du développement logiciel, de la navigation web et de l'assistance personnelle. Le modèle Orchard-SWE atteint **69,7 % sur SWE-bench Verified** (73 % avec reranking par modèle de valeur) avec seulement **3 milliards de paramètres actifs**, soit dix fois moins que les systèmes propriétaires comparables. Données et méthodes d'entraînement sont publiées.

**Des chercheurs militaires chinois distillent les modèles américains**

Une enquête de Reuters portant sur plus de **80 articles et brevets chinois** montre que des chercheurs liés à l'armée utilisent les sorties de modèles OpenAI et Anthropic pour entraîner des systèmes plus petits, tournant en local. Applications visées : surveillance, opérations cyber, analyse de code et usages tactiques. La technique, la distillation, transfère des capacités ciblées, mais Reuters rappelle qu'elle ne reproduit ni la pleine puissance d'un modèle frontière, ni la capacité de calcul nécessaire pour en entraîner un.

**Gérer la mémoire d'un agent comme un cycle de vie, pas comme un stockage**

Ce papier propose l'"Agentic Context Management", un cadre en cinq temps pour décider ce qu'un agent doit **garder, récupérer, partager, préparer et compresser** dans son contexte. Le constat de départ est concret : les agents en production échouent moins par manque d'intelligence que par saturation, quand vieux messages, descriptions d'outils et sorties obsolètes engorgent la fenêtre. Stocker davantage ne suffit pas, il faut des règles de pertinence, d'accès, de timing et de compression.


# **🗞️PLUS D'ACTUALITÉS**

**Le PDG de Hugging Face : "la Chine domine clairement les modèles ouverts"**

Clément Delangue a déclaré lundi sur CNBC que la Chine était en train de gagner la course à l'IA, et qu'elle pourrait rattraper la frontière technologique américaine dès la fin 2026 ou en 2027. Il pointe un écosystème chinois de collaboration ouverte face à des labos américains qui "construisent en silos". Il est aussi revenu sur le piratage de Hugging Face par des agents OpenAI échappés d'un environnement d'entraînement le mois dernier, tout en assurant que la collaboration entre les deux entreprises reste saine. Microsoft, Palantir et Nvidia ont par ailleurs signé une lettre appelant à ne pas restreindre les modèles open-weight.

**Un robot centaure à tête de bouc, avec des tronçonneuses à la place des mains**

La startup californienne Satyress a construit Threehalves, une machine de plus d'1m80 qui combine un torse humanoïde et une base à quatre pattes freinée par friction. Ses bras se terminent par un connecteur rapide alimenté en 12v, 18v ou 48v, capable d'accueillir différents outils, dont des tronçonneuses. La cible : fronts d'incendie, zones industrielles toxiques, glissements de terrain et fouilles de décombres. Ce n'est pas un robot autonome, un opérateur le pilote au joystick.

**Des drones au poivre déployés dans des écoles américaines**

La Floride, la Géorgie et le Colorado financent des programmes pilotes, jusqu'à **557 000 dollars**, pour équiper des établissements de drones destinés à intercepter un tireur actif en une quinzaine de secondes. Capables d'atteindre **60 mph** et de traverser une fenêtre, ils sont pilotés à distance depuis Austin par des opérateurs de Mithril Defense, et non autonomes. Des experts en sécurité scolaire s'alarment du risque d'erreur d'identification et du fait que le dispositif détourne l'attention des vrais sujets.

**L'IA a conquis le code, elle attaque le drive**

Après les débuts catastrophiques et viraux de 2023, les IA vocales se sont fiabilisées et s'installent massivement dans la restauration rapide américaine. Taco Bell en a équipé **plus de 890 files de drive**, soit plus de 10 % de ses restaurants aux États-Unis. Dairy Queen déploie dans **25 États** avec l'objectif de couvrir tout son parc, et White Castle fait tourner son assistante "Julia" sur 12 % de ses sites, systématiquement installée dans les nouvelles ouvertures. McDonald's avait lancé le mouvement dès 2019 en rachetant Apprente.

**Anthropic a supprimé 80 % du prompt système de Claude Code, sans rien perdre**

Un ingénieur d'Anthropic explique que l'entreprise a retiré **plus de 80 %** des instructions système de Claude Code sans aucune dégradation sur les évaluations de code. La raison de la longueur d'origine : les premiers modèles avaient besoin de garde-fous rigides pour éviter les suppressions de fichiers ou les commentaires non professionnels. L'équipe savait que ces règles seraient parfois contre-productives, mais le compromis valait le coup. Avec des modèles capables de juger seuls d'une situation, il ne tient plus.

**OpenAI dévoile GPT-Live, la voix continue sans tour de parole**

OpenAI a publié les coulisses de GPT-Live, un système d'interaction vocale continue développé en six mois. Il repose sur un modèle de parole dit "turnless" et une architecture à faible latence, censée supprimer les coupures caractéristiques des assistants vocaux classiques, où il faut attendre la fin d'une phrase pour obtenir une réponse. L'objectif affiché est une conversation qui ressemble à une vraie conversation.

**Fish Audio clone une voix en quelques secondes, en direct**

La startup a lancé publiquement son modèle phare S2.1 Pro, dont les fondateurs ont fait la démonstration en clonant leur propre voix en temps réel devant leur audience. L'annonce s'accompagne d'une levée de **52 millions de dollars** en seed, pour clore sa première année d'existence. À rapprocher des nouvelles obligations européennes d'étiquetage entrées en vigueur cette semaine.

**Gemini Spark navigue dans Chrome avec vos comptes connectés**

L'assistant agentique de Google s'intègre désormais au navigateur et peut "utiliser vos comptes connectés et mots de passe enregistrés pour gérer les corvées du web". Les exemples cités par Google : rechercher des options de vol et amorcer une réservation, ou prendre rendez-vous pour visiter un appartement. Google annonce des défenses contre l'injection de prompt et assure que Spark ne finalise jamais un paiement sans validation. Déploiement d'abord aux États-Unis, tandis que Google AI Pro est désormais disponible dans plus de 160 pays.

**Google retire la génération d'images de Google Earth au bout de 24 heures**

Lancée le 30 juillet, la fonctionnalité Nano Banana 2 permettait de générer des images ancrées dans les vues satellite, aériennes et 3D de Google Earth : reconstitutions historiques, infographies éducatives, projets immobiliers, visualisation d'aménagements urbains. Dès le 31 juillet, Google faisait machine arrière après avoir vu circuler des captures d'écran contraires à ses règles. Les images étaient pourtant filigranées et n'apparaissaient pas dans l'expérience principale. "Les gens font une confiance particulière à Google Earth pour une vue fiable du monde", explique l'entreprise.

**RAMageddon : l'IA fait grimper le prix de votre prochain ordinateur**

Les usines de semi-conducteurs réorientent leurs capacités vers tout ce qui touche à l'IA, provoquant une pénurie mondiale de mémoire qui se répercute sur les PC, les consoles et les smartphones. Samsung a déjà vendu toute sa production jusqu'à fin 2026. Tim Cook envisage de diversifier ses fournisseurs au-delà de TSMC, AMD anticipe **20 % de revenus gaming en moins** au second semestre, et Microsoft prévoit une baisse des ventes de PC liée à la hausse des prix mémoire.

**Le MacBook Air en rupture, livraisons repoussées à fin août**

Conséquence directe de la pénurie : le prix du MacBook Air est passé de **1 099 à 1 299 dollars**, et une commande passée aujourd'hui n'arrivera pas avant la fin du mois, selon les sources de Mark Gurman chez Bloomberg. Fait inhabituel, Apple oriente activement les acheteurs vers le MacBook Pro 14 pouces sur son site et dans ses boutiques, en attendant un rafraîchissement M6 de la gamme Air.

**Apple a enfin réparé Siri, et ça ne fait plus d'effet à personne**

Avec la bêta publique d'iOS 27, la nouvelle Siri fait ce qu'Apple avait promis : elle comprend le contexte personnel (photos, mails, calendrier, contacts), puise dans des connaissances générales et retrouve des informations stockées sur l'iPhone. Elle gagne une app dédiée, un mode texte, la reconnaissance visuelle par l'appareil photo et des conversations en va-et-vient. Le problème n'est pas la qualité, elle est correcte, mais le calendrier : pendant les années de retard d'Apple, le reste de l'industrie est passé aux agents autonomes multi-étapes.

**AWS s'associe à Superblocks, et le signal est plus large qu'il n'y paraît**

La startup de vibe coding s'intègre nativement aux clouds privés des clients AWS : les applications générées créent des bases Amazon Aurora en interne et passent par Amazon Bedrock, plutôt que d'envoyer des données à des services externes comme Supabase. AWS commercialisera l'outil via son Marketplace. La tendance de fond est claire : les hyperscalers poussent les entreprises à séparer le modèle du reste de l'infrastructure agentique, pour éviter la dépendance directe aux labos comme Anthropic ou OpenAI.

**Boris Cherny : arrêtez d'optimiser le coût des tokens**

Le responsable de Claude Code chez Anthropic assume une position à contre-courant de la plupart des équipes techniques : "j'utilise Fable pour tout". Son raisonnement tient en une ligne : il y a peut-être 50 % d'économies à gratter sur la facture de tokens, mais **1 000 %, 10 000 % voire 100 000 %** de gain potentiel sur le retour. Sa recommandation : prendre le modèle le plus cher et se demander comment en tirer davantage, plutôt que de chercher où rogner.

**L'UE active ses pouvoirs de contrôle sur Anthropic, OpenAI et Google**

En parallèle des obligations de transparence, la Commission européenne peut désormais exiger l'évaluation d'un modèle **avant sa sortie publique** dans l'UE, restreindre son accès au marché et infliger des amendes jusqu'à 3 % du chiffre d'affaires annuel. Ces pouvoirs s'appliquent à toute entreprise proposant un modèle à usage général en Europe, quelle que soit sa nationalité. Bruxelles négocie déjà avec OpenAI et Anthropic à la suite de cyberattaques impliquant leurs modèles, et a obtenu l'accès au modèle Mythos d'Anthropic. Le contexte diplomatique est tendu : Donald Trump a menacé l'UE de droits de douane "substantiels" après l'amende d'un milliard de dollars infligée à Google en juillet.

**Le voyage de luxe d'OpenAI pour influenceurs tourne au bad buzz**

OpenAI a emmené une poignée de créateurs dans une retraite haut de gamme de l'État de New York, baptisée "Summer Club" : dîners fermiers, apiculture, ateliers bien-être et cours d'utilisation de ChatGPT Work. Les vidéos étaient soignées, les commentaires beaucoup moins : les internautes ont reproché aux créateurs de fermer les yeux sur l'impact environnemental des centres de données en échange d'un séjour offert. L'un d'eux a fini par supprimer sa vidéo.

**OpenAI répond publiquement à la plainte d'Apple**

Dans un billet intitulé sans détour "Apple is getting this wrong", OpenAI qualifie la plainte d'Apple d'infondée, corrige des affirmations concernant ses employés et publie des échanges de messages pour appuyer sa version des faits. La sortie intervient dans un climat déjà tendu entre les deux entreprises sur le terrain de l'IA.

**Le PDG de Palantir traite l'industrie de l'IA de "marxiste"**

Dans sa lettre aux actionnaires, Alex Karp, docteur en théorie sociale, écrit que les constructeurs de grands modèles de langage cherchent, sciemment ou non, à "capturer les moyens de production" de leurs propres clients entreprises. Sa thèse : des IA génériques permettent de récupérer le savoir-faire des clients pour bâtir ensuite des produits concurrents. Palantir se positionne en alternative agnostique laissant le contrôle des données aux entreprises. Le trimestre lui donne du volume sonore : **1,9 milliard de dollars** de revenus, en hausse de **93 %** sur un an, et 1,1 milliard de profit.

**June sort de stealth avec 20 millions de dollars pour déployer l'IA en entreprise**

Fondée par d'anciens dirigeants de Bonobo AI, rachetée par Salesforce, la startup lève en pre-seed auprès de Time Ventures (Marc Benioff), Michael Dell, Aaron Levie et George Kurtz. Sa plateforme scanne les systèmes existants d'une entreprise (Salesforce, ServiceNow, Workday) pour repérer doublons et goulots d'étranglement, puis génère une feuille de route de déploiement d'agents. "L'IA, paradoxalement, augmente la demande de services professionnels", résume la cofondatrice Efrat Rapoport.

**Le fonds d'un ex-chercheur d'OpenAI perd 67 % en un mois**

Leopold Aschenbrenner, ancien de l'équipe superalignment d'OpenAI et auteur de l'essai "Situational Awareness", gère un hedge fund du même nom. En juillet, ses paris à effet de levier x4 sur l'infrastructure IA (SK Hynix, CoreWeave, Nebius, Micron, Bloom Energy) lui ont coûté **67 % de sa valeur**. Le fonds reste malgré tout à **+80 % sur l'année**, après un premier semestre 2026 à +439 %.

**Amazon franchit les 3 000 milliards de dollars de capitalisation**

L'action a clôturé en hausse de 4 % lundi, son meilleur jour depuis mai, portée par des résultats trimestriels solides. AWS a généré **42,2 milliards de dollars** au deuxième trimestre, au-dessus des attentes. L'entreprise relève son budget d'investissement 2026 de 200 à **220 milliards de dollars**, en partie à cause de la flambée des prix de la mémoire, et juge que même à ce niveau la capacité reste insuffisante face à la demande.

**Jim Cramer : Wall Street a fini par accepter les dépenses IA**

Selon l'animateur de CNBC, ce sont les explications détaillées d'Andy Jassy sur le retour attendu des investissements qui ont débloqué la situation, plus que le montant lui-même. Microsoft échappe aux critiques parce qu'elle monétise déjà via Azure et Copilot. Meta reste la plus attaquée, faute d'avoir expliqué comment ses dépenses se transformeront en revenus.

**SpaceX publie ses premiers résultats après une chute de plus de 50 %**

Le premier rapport trimestriel de l'entreprise cotée est attendu mardi après clôture, alors que l'action a perdu plus de la moitié de sa valeur depuis son pic post-IPO, soit **plus de 500 milliards de dollars** de capitalisation envolés depuis la première cotation du 12 juin. La valorisation de 1 400 milliards repose largement sur les promesses d'Elon Musk : centres de données en orbite, Mars, et une offensive IA face à Google, OpenAI et Anthropic.