---
id: collect-261121-veille-ia/claude-anthropic/mistral-medium-3-5-77-6-swe-bench-1-50-api-2026
title: "Mistral Medium 3.5 : 77,6% SWE-Bench, 1,50$ API [2026]"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-04-29"]
keywords: ["mistral", "medium", "modele", "dense", "milliards", "avril", "parametres", "fiche"]
source: https://tech-insider.org/fr/mistral-medium-3-5-vibe-le-chat-work-mode-avril-2026/
source_anchor: ""
source_lines: [1, 194]
section: "Claude_Anthropic"
---
Paris, 29 avril 2026 – La start-up française **Mistral AI** a frappé un grand coup hier en dévoilant simultanément **Mistral Medium 3.5**, un modèle dense de 128 milliards de paramètres en poids ouverts, un nouvel agent de codage à distance baptisé **Vibe**, et un mode **Work** dans son assistant grand public **Le Chat**. L’annonce, publiée sur le blog officiel de l’entreprise et confirmée par sa fiche Hugging Face datée du 29 avril 2026, marque la consolidation la plus ambitieuse jamais tentée par le champion européen de l’intelligence artificielle générative. Sur le banc d’essai **SWE-Bench Verified**, Medium 3.5 atteint **77,6 %**, et sur le test agentique **τ³-Telecom**, le score grimpe à **91,4 %**, des chiffres qui placent la jeune pousse parisienne au coude-à-coude avec les meilleurs modèles propriétaires américains, à un tarif API près de trois fois inférieur.

Dix-huit mois après son tour de table record de **1,7 milliard d’euros** en série C valorisant l’entreprise à **11,7 milliards d’euros post-money**, mené par ASML avec la participation de DST Global, Andreessen Horowitz, Bpifrance, General Catalyst, Index Ventures, Lightspeed et NVIDIA, Mistral devait prouver qu’il pouvait transformer ce capital en feuille de route produit cohérente. Le lancement du 29 avril est sa réponse : un seul modèle généraliste — commercialisé sous la référence de build v26.04 selon la fiche Hugging Face — qui remplace simultanément Magistral et Medium 3.1 dans Le Chat ainsi que Devstral 2 dans Vibe, et qui devient l’ossature d’une suite intégrée pour le grand public, les développeurs et les entreprises. Cette stratégie de modèle unique tranche radicalement avec la fragmentation que l’on observe chez OpenAI ou Google.

## Mistral Medium 3.5 : la fiche technique d’un modèle dense de 128 milliards de paramètres

Le cœur du lancement est **Mistral Medium 3.5**, un modèle de langage *dense*, c’est-à-dire *non* Mixture-of-Experts, ce qui le distingue immédiatement de DeepSeek V4, Mixtral et de la plupart des nouveaux modèles à très grande échelle. Avec ses 128 milliards de paramètres tous activés à chaque inférence, Medium 3.5 sacrifie l’efficacité économique du sparse pour la stabilité et la prévisibilité du dense. La fiche Hugging Face publiée le 29 avril 2026 confirme une fenêtre de contexte de **256 000 tokens**, soit environ 200 000 mots, suffisamment pour ingérer un rapport annuel complet du CAC 40 ou un code source de plusieurs centaines de fichiers en une seule requête. Selon le guide de déploiement publié par Clore.ai en août 2026, ce transformeur dense de 128 milliards de paramètres affiche un temps jusqu’au premier token (TTFT) compris entre 50 et 250 millisecondes en mode « instant », une latence qui le rend directement exploitable pour des interfaces conversationnelles temps réel.

Le modèle est **multimodal** : il accepte texte et image en entrée et produit du texte. Il intègre nativement l’**appel de fonctions** (function calling) et la sortie JSON structurée, deux fonctionnalités désormais indispensables pour les workflows agentiques. Mistral introduit également un paramètre original : un **réglage de l’effort de raisonnement** configurable par requête, qui permet au développeur d’arbitrer en temps réel entre latence et profondeur d’analyse, sans avoir à basculer entre un modèle « rapide » et un modèle « lent » comme c’est le cas avec OpenAI o3 et GPT-5.5.

Sur le plan opérationnel, Mistral revendique la possibilité de *self-host* Medium 3.5 sur **seulement quatre GPU**, un argument clé pour les administrations européennes et les grandes entreprises soumises à des obligations de souveraineté des données. Le 1er juillet 2026, NVIDIA a publié sur Hugging Face une version quantisée **Mistral-Medium-3.5-128B-NVFP4**, conservant la fenêtre de contexte complète de 256 000 tokens tout en réduisant l’empreinte mémoire nécessaire au déploiement on-premises. Le modèle est distribué sous une **licence MIT modifiée**, ce qui le maintient dans la catégorie des *open weights* mais avec des restrictions d’usage commercial qui le rapprochent davantage de Llama 4 que d’un véritable open source au sens OSI. Les poids sont disponibles publiquement sur Hugging Face dès le jour du lancement.

## SWE-Bench 77,6 %, τ³-Telecom 91,4 % : pourquoi ces scores changent la donne

Les chiffres communiqués par Mistral le 29 avril 2026 placent Medium 3.5 dans le peloton de tête des modèles agentiques. Sur **SWE-Bench Verified**, le test de référence qui mesure la capacité d’un modèle à résoudre des bugs réels issus de dépôts GitHub open source en autonomie, le score de **77,6 %** dépasse celui de Mistral Large 3 et talonne directement les modèles propriétaires les mieux placés du marché. Ce résultat est d’autant plus significatif qu’il provient d’un modèle *en poids ouverts* : aucun modèle ouvert n’avait jusqu’à présent franchi ce seuil symbolique avec une licence permissive sur quatre GPU. Ce score de 77,6 % a été confirmé indépendamment en juillet 2026 par le classement AI Rankings, qui situe Medium 3.5 nettement devant son prédécesseur Devstral 2 (environ 72,2 % sur le même test), validant ainsi le remplacement opéré au sein de Vibe.

Le score de **91,4 %** sur **τ³-Telecom** est encore plus révélateur de l’orientation produit. τ³-Telecom, troisième itération du benchmark tau-bench développé pour évaluer les agents conversationnels en situation client réelle (assistance technique, gestion de contrats, escalade), est devenu en 2025 le standard de référence pour les déploiements en centre de contacts. Un score supérieur à 90 % signifie que l’agent gère sans intervention humaine la quasi-totalité des conversations de niveau 1 et 2. Selon les retours préliminaires de la communauté testeur, Medium 3.5 s’impose comme l’un des deux ou trois modèles capables de soutenir ce seuil de manière reproductible.

### Comparaison des benchmarks face aux modèles concurrents

| Modèle | Éditeur | Paramètres | Contexte | SWE-Bench Verified | Prix entrée / 1M tokens | Prix sortie / 1M tokens | 
|---|---|---|---|---|---|---|
| Mistral Medium 3.5 | Mistral AI (FR) | 128 Md (dense) | 256K | 77,6 % | 1,50 $ | 7,50 $ | 
| Mistral Large 3 | Mistral AI (FR) | Non divulgué | 256K | ~71 % | 2,00 $ | 6,00 $ | 
| Claude Opus 4.7 | Anthropic (US) | Non divulgué | 200K-1M | ~80 % | 15,00 $ | 75,00 $ | 
| GPT-5.5 | OpenAI (US) | Non divulgué | 400K | ~78 % | 5,00 $ | 15,00 $ | 
| Gemini 3.5 Flash | Google (US) | Non divulgué | 1M | ~72 % | 2,00 $ | 8,00 $ | 
| DeepSeek V4 | DeepSeek (CN) | 1,6 T (MoE) | 128K | ~74 % | 0,55 $ | 1,74 $ | 

Le tarif API de **1,50 dollar par million de tokens en entrée** et **7,50 dollars par million de tokens en sortie** positionne Medium 3.5 sur un segment économique radicalement différent de Claude Opus 4.7 et GPT-5.5. Ce tarif a d’ailleurs évolué depuis le lancement : selon le suivi de ConductAtlas daté du 12 au 15 août 2026, la fiche modèle est passée d’une tarification initiale en euros (1,25 €/6,40 € par million de tokens) à la grille actuelle de 1,50 $/7,50 $, un ajustement qui confirme la stratégie de Mistral d’aligner ses prix sur le marché américain en dollars. Pour un usage d’agent de codage produisant en moyenne 200 000 tokens de sortie par tâche, la facture Mistral s’élève à environ 1,50 dollar contre 15 dollars chez Anthropic. Sur une équipe de 50 développeurs effectuant 20 tâches par jour, l’écart annuel dépasse les 1,2 million d’euros.

## Vibe : l’agent de codage à distance qui défie Cursor et Claude Code

Le deuxième volet de l’annonce est **Vibe**, l’agent de codage à distance de Mistral. Vibe existait déjà depuis fin 2025 sous forme d’interface en ligne de commande propulsée par Devstral 2. Avec la mise à jour du 29 avril, Vibe abandonne Devstral 2 au profit de Medium 3.5 et bascule sur une architecture *remote-first* : l’agent s’exécute désormais dans une sandbox cloud isolée, accède au dépôt Git via une connexion sécurisée, et peut être piloté à distance depuis n’importe quel terminal, y compris un téléphone mobile.

Ce positionnement entre directement en concurrence avec **Claude Code** d’Anthropic, **Cursor** et le récent agent autonome de **Replit**. La différence stratégique tient en deux points. D’abord, Vibe propulsé par Medium 3.5 affiche un coût d’exploitation divisé par dix par rapport à Claude Code, ce qui ouvre les agents de codage à distance à des équipes intermédiaires (PME tech, ESN, agences). Ensuite, Vibe peut être déployé en self-hosted sur un cluster on-premises de quatre GPU, une option inédite chez les concurrents américains qui imposent tous une exécution cloud propriétaire.

Concrètement, Vibe accepte des instructions en langage naturel – par exemple « ajoute la pagination cursor-based à l’endpoint /products et écris les tests Jest correspondants » – et produit une *pull request* complète, testée, documentée, prête à être révisée. Lors des démonstrations internes diffusées par Mistral, l’agent a résolu 8 issues GitHub réelles consécutives sur un dépôt Django de 47 000 lignes, sans intervention humaine, en moins de 38 minutes au total.

## Le Chat Work mode : multi-étapes et productivité de bureau

Le troisième volet est le **Work mode** introduit dans **Le Chat**, l’assistant grand public et professionnel de Mistral. Work mode permet à Le Chat d’orchestrer des tâches multi-étapes complexes : recherche web, navigation sur des sites authentifiés, manipulation de documents bureautiques (Word, Excel, Google Workspace), génération de présentations, envoi d’emails via un compte connecté. Là où le mode standard de Le Chat répondait à une question, Work mode *exécute* une mission.

La promesse fonctionnelle ressemble à celle du *Computer Use* d’Anthropic ou de l’*Operator* d’OpenAI, mais avec une différenciation européenne assumée : l’ensemble des données traitées par Work mode transitent par des centres de données situés dans l’Union européenne, ce qui rend la fonctionnalité immédiatement compatible avec le RGPD et le futur AI Act dont l’application a été repoussée au 2 décembre 2027 dans le cadre du Digital Omnibus. Pour les directions juridiques des grandes entreprises françaises, c’est un argument différenciant majeur, et c’est précisément la cible visée par Mistral.

### Réactions du marché et premiers déploiements

Dans les heures qui ont suivi l’annonce, plusieurs grandes entreprises européennes ont indiqué publiquement leur intérêt pour l’évaluation de Medium 3.5 et Work mode dans leurs pilotes en cours. La capitalisation boursière des cotés européens de la chaîne IA – ASML, ASM International, BE Semiconductor – a affiché des hausses modérées en séance, signe que le marché interprète positivement la consolidation de l’écosystème logiciel européen autour d’un champion crédible. Le titre Microsoft, en revanche, est resté stable, indiquant que les investisseurs ne perçoivent pas encore Medium 3.5 comme une menace directe pour OpenAI sur le marché américain.

## Pourquoi le timing du 29 avril 2026 est stratégique pour Mistral

Le calendrier du lancement n’est pas neutre. Le 29 avril 2026 intervient **onze jours** après le séisme Apple Q2 2026 publié le 18 avril (111,2 Md$ de revenus, +17 %), **deux semaines** après les résultats d’Alphabet Q1 2026 marqués par la surprise du segment Google Cloud (+63 %), et **douze jours** après la révélation par le Wall Street Journal du manque à gagner de revenus 2026 chez OpenAI dans le contexte du méga-pari Stargate à 1 400 milliards de dollars. Dans ce paysage où chaque acteur américain communique des chiffres titanesques, Mistral choisit délibérément de jouer la carte du produit utilisable, du modèle ouvert et du coût maîtrisé.

Le lancement coïncide également avec la dernière ligne droite de **VivaTech 2026**, le salon technologique européen qui se tiendra à Paris Porte de Versailles à la mi-juin. Mistral a confirmé qu’Arthur Mensch tiendrait la keynote d’ouverture du salon. Dévoiler Medium 3.5 six semaines avant ce rendez-vous médiatique permet au modèle d’accumuler des retours d’usage, des intégrations partenaires et des cas clients publics qui pourront être mis en avant à VivaTech. La séquence rappelle la stratégie pré-Dreamforce de Salesforce ou pré-re:Invent d’AWS.

## Le positionnement souverain : argument différenciant pour l’Europe

L’angle **souveraineté** est désormais au cœur du discours commercial de Mistral. Dans un contexte où Bruxelles durcit l’application des règles numériques européennes en 2026 et où la perspective de représailles tarifaires américaines contre les services numériques continentaux pèse sur les arbitrages d’achat des grandes entreprises et administrations, l’origine française de Mistral, la possibilité d’auto-hébergement, et la disponibilité des poids ouverts forment un trio d’arguments puissant. Selon les analystes du *French Tech Journal*, le baromètre VivaTech 2026 indique que la souveraineté est devenue la première préoccupation tech des dirigeants européens, devant la productivité.

Mistral capitalise sur ce momentum réglementaire. La mise à disposition des poids du modèle dès le jour du lancement, sous une licence MIT modifiée permissive pour l’inférence en interne, satisfait les exigences les plus strictes des départements DSI publics. Les administrations françaises, encadrées par le programme *French Tech Souverain* et les directives de la DINUM, disposent depuis longtemps de freins à l’achat de licences cloud-only américaines ; Medium 3.5 leur offre une option crédible, performante, à un prix prédictible.

## Le pari économique : 1,50 $ contre 15 $ par tâche d’agent

La grille tarifaire de Medium 3.5 – 1,50 $ en entrée et 7,50 $ en sortie par million de tokens – est l’arme commerciale principale de Mistral. À titre de comparaison, Claude Opus 4.7 facture 15 $ en entrée et 75 $ en sortie, soit un ratio de 10× sur la sortie. GPT-5.5 d’OpenAI s’établit à 5 $ en entrée et 15 $ en sortie, soit un ratio de 2× sur la sortie pour des performances comparables sur SWE-Bench. Seul DeepSeek V4 affiche un tarif inférieur (0,55 $ / 1,74 $), mais avec une exécution principalement sur infrastructure Huawei Ascend en Chine, ce qui est incompatible avec les politiques d’achat de la plupart des entreprises occidentales.

### Tableau du coût total de possession pour un déploiement type

| Cas d’usage | Volume mensuel | Mistral Medium 3.5 | Claude Opus 4.7 | GPT-5.5 | Économie vs Claude | 
|---|---|---|---|---|---|
| Chatbot support N1 (50K conv.) | 500 M tokens E / 100 M S | 1 500 $ | 15 000 $ | 4 000 $ | −90 % | 
| Agent codage équipe 50 dev. | 1 Md tokens E / 200 M S | 3 000 $ | 30 000 $ | 8 000 $ | −90 % | 
| Analyse documents juridiques | 5 Md tokens E / 50 M S | 7 875 $ | 78 750 $ | 25 750 $ | −90 % | 
| Recherche scientifique R&D | 2 Md tokens E / 300 M S | 5 250 $ | 52 500 $ | 14 500 $ | −90 % | 
| Self-hosted (4× H100) | Illimité | ~12 000 $ (amort.) | Non disponible | Non disponible | n/a | 

Pour les organisations qui dépassent un seuil critique de consommation – au-delà de 2 milliards de tokens par mois – l’option *self-hosted* de Mistral devient économiquement imbattable. Quatre GPU NVIDIA H100, désormais loués autour de 2,08 $ l’heure (indice OrnN) ou amortis sur 24 mois pour un parc en propre, suffisent à servir des dizaines d’utilisateurs internes en latence interactive. Aucun concurrent américain de niveau équivalent ne permet aujourd’hui un tel déploiement on-premises sans contrat de licence enterprise négocié.

## Réactions des analystes : un tournant pour l’écosystème IA européen

**Arthur Mensch**, cofondateur et PDG de Mistral AI, a déclaré dans l’annonce officielle de la série C de septembre 2025 : « Cet investissement alimente notre recherche scientifique pour continuer à repousser la frontière de l’IA. » Cette phrase prend tout son sens avec le lancement du 29 avril : Mistral utilise effectivement les 1,7 milliard d’euros levés pour publier des modèles de plus en plus performants à un rythme désormais comparable à celui d’OpenAI ou Anthropic.

Selon les analyses publiées par *The French Tech Journal*, le baromètre VivaTech 2026 montre que les dirigeants européens sont confiants dans la tech mais focalisés sur la souveraineté – un terrain naturel pour Mistral. Du côté de Bpifrance, présent au capital depuis le tour de série C, le message est clair : la stratégie d’export du logiciel souverain européen est en train de payer. Les premières adoptions documentées de Le Chat en entreprise dans les secteurs bancaire et public français renforcent ce constat.

Côté américain, le silence officiel d’OpenAI et d’Anthropic sur l’annonce du 29 avril est éloquent. Les deux acteurs sont en pleine campagne pour défendre leur statut auprès des entreprises européennes face au durcissement réglementaire de Bruxelles. L’arrivée d’un modèle ouvert, performant et bien tarifé sur leur principal marché de croissance hors États-Unis complique leur narratif. Selon plusieurs canaux internes rapportés par TechCrunch dans la semaine, OpenAI prépare une réponse tarifaire ciblée sur la zone EMEA, à confirmer dans les prochaines semaines.

## Mistral Medium 3.5 face à Claude Opus 4.7, GPT-5.5 et Gemini 3.5

La comparaison frontale entre Medium 3.5 et les modèles frontaliers américains révèle une stratégie de positionnement claire : Mistral ne prétend pas dépasser Claude Opus 4.7 sur les benchmarks les plus exigeants – Opus 4.7 affiche encore près de 80 % sur SWE-Bench Verified et trône en tête de l’Arena Elo. En revanche, Medium 3.5 propose un rapport performance/prix difficile à battre dès lors qu’on accepte un compromis de 2 à 3 points de pourcentage sur les benchmarks de raisonnement avancé. Pour 80 % des cas d’usage entreprise réels – assistance support, génération de code routinier, synthèse documentaire, classification – le compromis est largement favorable à Mistral.

Face à Gemini 3.5 Flash, le combat est plus serré. Gemini 3.5 Flash affiche une fenêtre de contexte de 1 million de tokens, soit quatre fois celle de Medium 3.5, et bénéficie de l’intégration native dans Google Workspace, Vertex AI et l’écosystème Android. Mistral compense par les poids ouverts, l’auto-hébergement, et l’absence de dépendance à un fournisseur cloud unique. Pour une grande entreprise française qui souhaite éviter à la fois Microsoft Azure et Google Cloud, Mistral devient l’option par défaut.

Contre GPT-5.5, dont le lancement a marqué la barre des 82,7 % sur Terminal-Bench le 23 avril dernier, Medium 3.5 affiche un retard de quelques points mais propose une économie de 70 % sur l’API. Pour les déploiements à très grande échelle où le coût domine, Mistral reprend la main. Le pari de la jeune pousse parisienne consiste à monétiser ce différentiel auprès des organisations dont la consommation justifie l’investissement dans un fournisseur européen alternatif.

## Contexte historique : de 2023 à 2026, la course de fond de Mistral

Fondée à Paris en avril 2023 par **Arthur Mensch** (ex-DeepMind), **Guillaume Lample** et **Timothée Lacroix** (ex-Meta AI), Mistral AI a levé en trois ans environ **3,9 milliards de dollars** selon le suivi de Crunchbase et Clay. La trajectoire : amorçage record en juin 2023 (113 M€, mené par Lightspeed) ; série A en décembre 2023 (385 M€) ; série B mi-2024 (640 M€, mené par General Catalyst) ; série C en septembre 2025 (1,7 Md€, mené par ASML). Cette dernière a hissé la valorisation à **11,7 milliards d’euros post-money**, ce qui en fait, selon Crunchbase, le plus gros tour de table jamais bouclé par une entreprise IA européenne.

Côté effectif, Mistral comptait environ **456 employés** au début du printemps 2026 selon le dossier publié par Amy.vc, à comparer aux dizaines de milliers de salariés cumulés chez OpenAI, Anthropic et xAI. Cette taille réduite est à la fois une contrainte – moins de chercheurs, moins de produits, moins de support – et un atout : itérations rapides, alignement stratégique, allocation focalisée du capital. La séquence de produits Magistral → Devstral 2 → Mistral Medium 3.5 en l’espace de douze mois illustre cette vitesse d’exécution.

Le modèle d’affaires de Mistral repose sur trois piliers : l’**API** publique (compétitive en prix), les **déploiements enterprise** sur site ou cloud privé (marges élevées), et les **poids ouverts** distribués gratuitement (acquisition et goodwill). Ce trio rappelle la trajectoire de Red Hat dans le Linux d’entreprise – open source comme acquisition, services comme monétisation. Avec environ 8,1 millions de visites mensuelles sur son site selon Amy.vc, la marque Mistral commence à exister auprès du grand public, dimension nouvelle qui justifiera prochainement une équipe marketing étoffée.

## Cinq prédictions pour les douze prochains mois

**Prédiction 1 : Mistral lèvera une série D supérieure à 3 milliards d’euros avant la fin 2026.** Avec l’élargissement de la suite produit (modèle, agent de codage, assistant grand public en mode agentique), Mistral va devoir financer une accélération commerciale et un éventuel scale-up de ses capacités de calcul. La fenêtre de marché actuelle, alors que les valorisations des frontaliers IA américains explosent (OpenAI à 852 Md$, Anthropic en discussions à très haute valorisation), est propice à une levée massive.

**Prédiction 2 : un grand groupe français annoncera l’industrialisation de Vibe avant VivaTech 2026.** Les attentes médiatiques autour du salon parisien de juin créent une pression forte pour qu’au moins un acteur du CAC 40 – type BNP Paribas, AXA, Orange ou Schneider Electric – confirme un déploiement à grande échelle d’agents de codage Vibe en interne. La direction commerciale de Mistral travaillerait déjà sur plusieurs PoC selon les sources de marché.

**Prédiction 3 : OpenAI baissera ses prix EMEA dans les 90 jours.** Face à l’écart tarifaire creusé par Medium 3.5, OpenAI ne peut pas laisser ses clients européens migrer sans réagir. Une grille EMEA spécifique, soit via un partenariat infrastructure local (potentiellement avec OVH ou Scaleway), soit via une remise volumétrique ciblée, est à attendre avant la fin du trimestre.

**Prédiction 4 : Mistral publiera un modèle « Large 4 » MoE de très grande échelle d’ici fin 2026.** Le segment dense 128B est exploité par Medium 3.5. Pour rester crédible face à GPT-5.5 et Claude Opus 4.7 sur les benchmarks de pointe, Mistral devra publier un modèle MoE de plusieurs centaines de milliards de paramètres actifs. La feuille de route déjà esquissée par Arthur Mensch lors de précédentes interventions publiques pointe dans cette direction.

**Prédiction 5 : Le Chat deviendra l’assistant IA grand public n°1 en France au S2 2026.** ChatGPT reste largement en tête en termes d’utilisateurs grand public sur le marché français, mais le Work mode combiné à l’avantage souverain et à la médiatisation autour de Mistral pourrait suffire à faire basculer le marché B2C national. Une campagne marketing massive est probablement en préparation pour la rentrée.

## Implications pour les développeurs et les DSI européens

Pour les **développeurs**, l’arrivée de Medium 3.5 en poids ouverts via Hugging Face change la donne pratique : il devient possible de prototyper un agent de codage de niveau industriel sur une station de travail équipée de quatre GPU consumer (par exemple, quatre RTX 4090 quantisées en INT8), pour un coût matériel inférieur à 12 000 euros. Cela élargit drastiquement la base de développeurs capables d’expérimenter localement des modèles agentiques, là où Claude Opus 4.7 et GPT-5.5 restent inaccessibles hors API cloud.

Pour les **DSI**, l’arbitrage devient plus simple : Mistral apporte une alternative crédible, performante et souveraine, sans imposer le compromis qualité auquel on aurait pu s’attendre il y a deux ans. Les programmes de pilotes IA dans les ministères français, les hôpitaux publics, les opérateurs de transport et les énergéticiens peuvent désormais inclure Mistral comme option principale, et non plus comme alternative *de complément* derrière OpenAI ou Anthropic. Le narratif RGPD/AI Act devient un argument de vente concret plutôt qu’une contrainte.

Pour les **directions financières**, le pari économique est favorable. La grille tarifaire Mistral permet de bâtir des cas d’usage IA à ROI positif là où le tarif Anthropic les rendait marginaux. Les budgets IT 2027 devront probablement intégrer une ligne « modèles européens » distincte des fournisseurs hyperscale, ce qui sera la marque d’une nouvelle maturité du marché.

## Risques et angles morts du lancement

L’enthousiasme autour de Medium 3.5 ne doit pas masquer plusieurs zones d’ombre. Premièrement, la **fiabilité factuelle** de Le Chat fait débat. Une étude NewsGuard publiée le 28 avril 2026 indique que Le Chat répète des informations inexactes ou non vérifiées dans environ la moitié des cas lorsqu’il est interrogé sur certaines campagnes de désinformation. Mistral n’a pas répondu publiquement aux sollicitations de NewsGuard à cette date, ce qui constitue un point de tension PR à très court terme.

Deuxièmement, l’**écosystème d’outils tiers** reste massivement orienté OpenAI et Anthropic. Les frameworks LangChain, LlamaIndex, AutoGen, CrewAI fonctionnent tous nativement avec Mistral, mais la documentation et les exemples communautaires sont nettement moins étoffés. Pour les nouveaux développeurs qui apprennent l’IA en 2026, choisir Mistral implique d’accepter un parcours d’apprentissage légèrement plus rugueux.

Troisièmement, la **capacité d’inférence** de Mistral reste modeste comparée aux capex titanesques des hyperscalers américains. Si Vibe et Work mode connaissent un succès viral à la suite du lancement, la jeune pousse pourrait faire face à des limitations de débit qui détérioreraient l’expérience client. La levée série D évoquée plus haut prendra alors un caractère d’urgence stratégique.

## Calendrier produit Mistral 2026 : ce qu’il faut surveiller

Au-delà du 29 avril, le calendrier produit de Mistral pour 2026 s’annonce dense. Les analystes attendent : une mise à jour incrémentale Medium 3.6 d’ici l’été, l’ouverture d’une bêta publique pour Vibe sur iOS et Android avant VivaTech, le déploiement de Le Chat Enterprise avec SSO SAML et journalisation détaillée d’ici septembre, et l’éventuelle annonce d’un partenariat infrastructure souverain avec OVHcloud ou Scaleway dans le sillage du *French Tech Souverain*. Aucun de ces points n’a été officiellement confirmé par Mistral à la date du 29 avril, mais ils dessinent la trajectoire logique du déploiement commercial de l’entreprise.

L’inscription du calendrier dans la séquence préparatoire à VivaTech 2026 (16-19 juin Porte de Versailles) renforce cette lecture. Pour la première fois depuis la fondation de la société, Mistral abordera le salon parisien en position de leader de marché incontesté du segment IA française, avec un produit grand public utilisable, un agent de codage déployable, un modèle ouvert performant, et une valorisation à plus de dix milliards. Le récit politico-industriel construit par l’Élysée et Bpifrance autour de la souveraineté technologique européenne dispose désormais d’un véhicule produit crédible pour exister à l’international.

## FAQ : tout ce qu’il faut savoir sur Mistral Medium 3.5

### Quand Mistral Medium 3.5 a-t-il été annoncé ?

Mistral AI a publié Mistral Medium 3.5 le **29 avril 2026**, annoncé simultanément sur sa page officielle d’actualités et sur Hugging Face. Le modèle est immédiatement disponible via l’API La Plateforme, dans Le Chat (en remplacement de Magistral), dans Vibe (en remplacement de Devstral 2) et en téléchargement libre pour auto-hébergement.

### Quels sont les benchmarks officiels de Mistral Medium 3.5 ?

Selon la fiche Hugging Face publiée le 29 avril 2026, Medium 3.5 obtient **77,6 %** sur **SWE-Bench Verified** et **91,4 %** sur le benchmark agentique **τ³-Telecom**. Mistral souligne dans son annonce que ces scores positionnent le modèle au niveau des frontaliers propriétaires américains pour la plupart des cas d’usage entreprise réels, à un tarif API significativement inférieur.

### Combien coûte l’API de Mistral Medium 3.5 ?

Le tarif officiel de l’API est de **1,50 $ par million de tokens en entrée** et **7,50 $ par million de tokens en sortie**. À titre de comparaison, Claude Opus 4.7 coûte 15 $ / 75 $, et GPT-5.5 coûte 5 $ / 15 $ pour les mêmes volumes. Pour les très gros volumes, l’option d’auto-hébergement sur quatre GPU H100 devient plus économique encore.

### Qu’est-ce que Vibe et en quoi diffère-t-il de Cursor ?

Vibe est l’agent de codage à distance de Mistral, désormais propulsé par Medium 3.5. Contrairement à Cursor qui est un IDE local connecté à plusieurs modèles cloud, Vibe est une plate-forme d’exécution agentique remote-first : l’agent fonctionne dans une sandbox isolée, accède au dépôt Git, et produit des pull requests complètes pilotables depuis n’importe quel terminal. Vibe est également déployable on-premises sur quatre GPU.

### Qu’est-ce que le Work mode dans Le Chat ?

Work mode est un nouveau mode introduit dans Le Chat le 29 avril 2026, qui permet à l’assistant d’orchestrer des tâches multi-étapes complexes : navigation web, manipulation de documents bureautiques, génération de présentations, envoi d’emails. Il se compare fonctionnellement au *Computer Use* d’Anthropic et à l’*Operator* d’OpenAI, avec la garantie d’un traitement des données dans l’Union européenne.

### Mistral Medium 3.5 est-il open source ?

Medium 3.5 est distribué en **poids ouverts** sous une **licence MIT modifiée**. Les poids sont téléchargeables librement sur Hugging Face, ce qui permet l’inspection, l’évaluation et le déploiement en interne. Toutefois, certaines restrictions d’usage commercial s’appliquent, ce qui le distingue d’une licence open source au sens strict de l’OSI. La situation est comparable à Llama 4 de Meta.

### Quelle est la valorisation actuelle de Mistral AI ?

Lors de sa série C bouclée le **9 septembre 2025**, Mistral a été valorisée à **11,7 milliards d’euros post-money**, levant **1,7 milliard d’euros** auprès d’un consortium mené par ASML et incluant DST Global, Andreessen Horowitz, Bpifrance, General Catalyst, Index Ventures, Lightspeed et NVIDIA. Aucune valorisation 2026 actualisée n’a été officiellement publiée par l’entreprise à la date du 29 avril 2026.

### Combien d’employés compte Mistral AI ?

Selon le dossier publié par Amy.vc au printemps 2026, Mistral AI emploie environ **456 personnes**, principalement basées à son siège parisien. La structure reste légère comparée aux dizaines de milliers de salariés cumulés chez OpenAI, Anthropic ou xAI, ce qui constitue à la fois une contrainte et un atout pour la vélocité d’exécution.

### Peut-on auto-héberger Mistral Medium 3.5 ?

Oui. Mistral revendique la possibilité d’auto-héberger Medium 3.5 sur **quatre GPU** seulement (typiquement quatre H100 ou équivalent). Les poids sont disponibles publiquement sur Hugging Face dès le 29 avril 2026. Cette option en fait l’un des rares modèles de classe frontalière déployable on-premises sans contrat enterprise négocié, un argument décisif pour les administrations publiques et les grandes entreprises soumises à des obligations de souveraineté.

### Quels concurrents Mistral cible-t-il directement ?

Mistral cible frontalement **Anthropic Claude**, **OpenAI GPT-5.5** et **Google Gemini 3.5** sur le segment entreprise européen. Sur le segment des agents de codage, Vibe se positionne contre **Claude Code**, **Cursor** et **Replit Agent**. Sur le segment grand public, Le Chat Work mode entre en compétition avec **ChatGPT**, **Claude** et **Gemini**. Le différenciateur transverse reste la souveraineté européenne et le tarif.

### L’AI Act européen s’applique-t-il à Mistral Medium 3.5 ?

L’application générale de l’AI Act a été repoussée au **2 décembre 2027** dans le cadre du Digital Omnibus adopté début 2026. Mistral, en tant qu’éditeur européen, sera concerné par le règlement mais bénéficie d’une posture conforme par défaut sur les questions de transparence, de gouvernance des données et de souveraineté. Cette compatibilité native constitue un argument commercial différenciant face aux concurrents américains.

### Où télécharger les poids de Mistral Medium 3.5 ?

Les poids du modèle sont disponibles publiquement sur Hugging Face à l’adresse `mistralai/Mistral-Medium-3.5-128B` depuis le 29 avril 2026. La documentation officielle, les exemples d’intégration et les conditions de licence sont publiés sur `docs.mistral.ai` et sur le blog officiel `mistral.ai/news`.

### Related Coverage

### Sources externes

- Mistral AI – page d’actualités officielle
- Mistral-Medium-3.5-128B sur Hugging Face
- Documentation officielle des modèles Mistral
- TechCrunch – couverture Mistral AI
- Amy.vc – dossier complet série C Mistral AI
