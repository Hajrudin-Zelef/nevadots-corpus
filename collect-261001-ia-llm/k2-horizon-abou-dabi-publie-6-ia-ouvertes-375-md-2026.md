Le 3 septembre 2026, un institut de recherche basé à Abou Dabi a publié en une seule journée six modèles d’intelligence artificielle allant de 0,9 à 375 milliards de paramètres, avec les poids, le code, les données d’entraînement ET les recettes de fabrication en accès libre. L’annonce de l’Institute of Foundation Models (IFM), rattaché à la Mohamed bin Zayed University of Artificial Intelligence (MBZUAI), rebat les cartes du débat sur l’IA ouverte à un moment où Bruxelles cherche justement des alternatives transparentes aux modèles fermés américains et chinois.

Baptisée **K2 Horizon**, cette famille de modèles se distingue par une amplitude rarement vue chez un seul éditeur : un modèle de 0,9 milliard de paramètres taillé pour tourner sur une montre connectée jusqu’à un modèle phare de 375 milliards de paramètres pensé pour les charges de travail agentiques en entreprise. Le tout sous licence Apache 2.0, la licence open source la plus permissive du marché. Pour un institut installé à Paris, Abou Dabi et dans la Silicon Valley, l’enjeu dépasse la simple prouesse technique : il touche directement au débat français et européen sur la souveraineté numérique.

## K2 Horizon : six modèles, une seule philosophie d’ouverture

Le lancement du 3 septembre 2026 ne concerne pas un modèle unique mais une flotte complète de six systèmes, chacun calibré pour un usage matériel précis. Le plus petit, K2 Horizon 0,9B, vise les appareils très contraints comme les montres connectées et les lunettes intelligentes, avec une fenêtre de contexte de 128 000 tokens obtenue via une extension YaRN RoPE. Les modèles 3,7B et 7B, tous deux denses, ciblent les smartphones et les déploiements à faible coût, avec une fenêtre de contexte native de 524 288 tokens héritée des dernières phases d’entraînement.

Le modèle 32B, dense lui aussi, est présenté par IFM comme le plus puissant de la gamme dans cette catégorie, pensé pour l’hébergement local et les serveurs on-premise avec 512 000 tokens de contexte. Vient ensuite K2 Horizon 36B-A4B, un modèle épars combinant une architecture Mixture-of-Value-Attention (MoVA) et un mélange d’experts (MoE) avec seulement 4 milliards de paramètres actifs par requête, pensé pour absorber des charges de travail quotidiennes lourdes à moindre coût de calcul. Enfin, le modèle phare K2 Horizon 375B-A23B est un système MoE épars avec 375 milliards de paramètres au total et 23 milliards de paramètres actifs par token, positionné par IFM parmi les meilleurs modèles de sa catégorie de taille pour le raisonnement général, le code et les tâches agentiques.

Ce qui distingue vraiment cette sortie du reste du marché open source, c’est le niveau de transparence. IFM ne se contente pas de publier des poids figés : l’institut diffuse aussi le code d’entraînement, les points de contrôle intermédiaires (checkpoints) et, pour plusieurs modèles, les données et les recettes de construction complètes. Cette approche permet à des chercheurs extérieurs de reproduire les résultats et d’auditer la manière dont chaque modèle a été construit, une pratique encore rare à cette échelle de paramètres.

## Le tableau complet de la famille K2 Horizon

Voici la répartition détaillée des six modèles annoncés le 3 septembre 2026, avec leur architecture, leur fenêtre de contexte et leur usage cible tel que défini par IFM.

| Modèle | Paramètres (total / actifs) | Architecture | Contexte | Usage cible | 
|---|---|---|---|---|
| K2 Horizon 0,9B | 0,9 Md / 0,9 Md | Dense | 128 000 tokens | Montres, lunettes, matériel très contraint | 
| K2 Horizon 3,7B | 3,7 Md / 3,7 Md | Dense | 524 288 tokens | Smartphones, fine-tuning mono-nœud | 
| K2 Horizon 7B | 7 Md / 7 Md | Dense | 524 288 tokens | Smartphones, déploiement bas coût, code | 
| K2 Horizon 32B | 32 Md / 32 Md | Dense | 512 000 tokens | Hébergement local, serveurs on-premise | 
| K2 Horizon 36B-A4B | 36 Md / 4 Md | MoE + MoVA (épars) | 512 000 tokens | Charges de travail quotidiennes intensives | 
| K2 Horizon 375B-A23B | 375 Md / 23 Md | MoE (épars) | 524 000 tokens | Raisonnement, code, agents en entreprise | 

## Des scores encore partiels mais des signaux encourageants

Sur le terrain des benchmarks, la communication d’IFM reste pour l’instant incomplète : aucun tableau officiel complet de scores MMLU ou HumanEval n’a été publié pour l’ensemble des six modèles au moment du lancement. Les chiffres disponibles proviennent surtout d’évaluations indépendantes menées dans les jours suivant l’annonce. Le modèle phare 375B-A23B aurait obtenu un score de 47 sur l’Artificial Analysis Intelligence Index, un net progrès par rapport au précédent modèle de raisonnement d’IFM, K2 Think V2, qui plafonnait à 17 sur le même index. Cette évaluation indépendante souligne que le modèle se montre particulièrement performant sur les tâches agentiques et l’exécution de tâches réelles, tout en étant moins dominant sur le raisonnement à forte densité de connaissances que sa taille pourrait le laisser penser.

Côté modèles légers, le plus petit de la famille, K2 Horizon 0,9B, afficherait un score de 48,5 sur AIME 2026 et de 79,9 sur HumanEval+, des résultats notables pour un modèle suffisamment compact pour tourner en quantification sur une montre connectée. Ces chiffres, bien qu’issus de sources indépendantes plutôt que d’un rapport technique officiel exhaustif, positionnent K2 Horizon comme une famille crédible plutôt que comme un simple coup de communication.

## Qui se cache derrière l’Institute of Foundation Models

L’Institute of Foundation Models a été fondé en mai 2025 par la Mohamed bin Zayed University of Artificial Intelligence (MBZUAI), l’université émiratie spécialisée en IA. L’institut opère depuis trois sites : Abou Dabi, la Silicon Valley et Paris. Cette implantation parisienne mérite d’être soulignée, car elle donne à IFM une présence opérationnelle directe sur le sol français, à un moment où le débat sur la souveraineté de l’IA agite les administrations et les entreprises françaises.

Il est important de ne pas confondre IFM avec le Technology Innovation Institute (TII), l’autre grand laboratoire d’IA basé à Abou Dabi, connu pour sa série de modèles Falcon lancée dès 2023 avec Falcon 40B. TII est rattaché à l’Advanced Technology Research Council (ATRC) d’Abou Dabi, tandis qu’IFM dépend de MBZUAI. Les deux organisations sont donc distinctes, même si elles partagent la même ville et une ambition similaire de démocratiser l’IA ouverte depuis le Golfe. TII avait par exemple lancé Falcon-H1 Arabic en janvier 2026, un modèle hybride Mamba-Transformer dédié à la langue arabe.

## Attention à ne pas confondre K2 Horizon et Kimi K2

Le nom “K2” prête à confusion avec la série Kimi K2 de l’éditeur chinois Moonshot AI, mais il s’agit de deux projets totalement distincts, sans aucun lien capitalistique ni technique. Moonshot AI a construit sa propre trajectoire de sorties rapides tout au long de 2026 : Kimi K2 a ouvert la voie, suivi de Kimi K2.5 en janvier 2026 avec un encodeur visuel natif, puis de Kimi K2.6 en avril 2026 (1 000 milliards de paramètres), de Kimi K2.7 Code en juin 2026 pour les tâches de programmation, et enfin de Kimi K3 le 16 juillet 2026, un modèle MoE de 2,8 billions de paramètres avec une fenêtre de contexte d’un million de tokens.

Sur le papier, Kimi K3 dépasse largement K2 Horizon 375B-A23B en nombre de paramètres bruts. Mais la comparaison n’est pas si simple : IFM mise sur la transparence totale (poids, données, code, recettes) là où Moonshot AI publie surtout des poids et une documentation technique plus classique. Les deux approches répondent à des besoins différents, celle d’IFM s’adressant davantage aux chercheurs et institutions qui veulent auditer et reproduire un modèle de bout en bout.

## Comparatif face aux autres familles de modèles ouverts

Le marché des modèles à poids ouverts s’est considérablement densifié en 2026. Rien qu’au mois d’août, neuf lancements distincts et vérifiables ont eu lieu, provenant de six laboratoires différents, dont trois avec des poids ouverts. Voici comment K2 Horizon se positionne face aux principales alternatives disponibles début septembre 2026.

| Famille | Éditeur | Taille (total / actifs) | Contexte | Licence | 
|---|---|---|---|---|
| K2 Horizon 375B-A23B | IFM (MBZUAI) | 375 Md / 23 Md | 524 000 tokens | Apache 2.0 | 
| Kimi K3 | Moonshot AI | 2,8 billions | 1 000 000 tokens | Modified MIT | 
| DeepSeek V4 Pro-0813 | DeepSeek | 1,7 billion | 1 000 000 tokens | MIT | 
| Qwen3.8-27B | Alibaba | 27 Md dense | 262 144 tokens (ext. 1M) | Apache 2.0 | 
| GLM-5.3 | Zhipu AI | 753 Md | 1 000 000 tokens | MIT | 
| Mistral Large 3 | Mistral AI | Non communiqué | 1 000 000 tokens | Apache 2.0 | 

Ce tableau illustre une tendance de fond : sur le seul critère du nombre de paramètres, K2 Horizon 375B-A23B reste modeste face aux mastodontes chinois. Mais IFM ne joue pas la même partition. L’institut mise sur l’amplitude de la gamme (six tailles couvrant tout le spectre matériel, d’une montre à un cluster de serveurs) et sur la profondeur de la divulgation, deux arguments que ni Moonshot AI ni DeepSeek ne mettent autant en avant. Pour situer ces choix dans le paysage plus large des modèles ouverts récents, notre comparatif Qwen3.8-Max et notre analyse du trio DeepSeek V4, Qwen3.8 Max et GLM-5.3 détaillent les rapports de force actuels.

## Disponibilité, outils et absence de tarification officielle

Les six modèles K2 Horizon sont disponibles en téléchargement libre sur Hugging Face, avec un support dès le premier jour pour les frameworks d’inférence vLLM, SGLang et Ollama. Cette compatibilité immédiate avec les outils les plus utilisés par la communauté open source facilite grandement l’adoption par les équipes techniques qui veulent tester rapidement les modèles sur leur propre infrastructure.

Aucune tarification d’API commerciale de premier niveau n’a été communiquée par IFM au moment du lancement. Le modèle économique repose donc, pour l’instant, sur l’auto-hébergement : les entreprises et les chercheurs qui veulent utiliser K2 Horizon doivent déployer les modèles sur leur propre infrastructure ou passer par un fournisseur d’inférence tiers, plutôt que de payer un tarif au token facturé directement par IFM. Pour une entreprise française soumise à des contraintes de résidence des données, cette absence de dépendance à une API propriétaire hébergée à l’étranger peut constituer un argument commercial en soi.

## Ce que disent les responsables d’IFM

Eric Xing, président de MBZUAI et fondateur de l’Institute of Foundation Models, a défendu la philosophie du lancement en expliquant : “K2 Horizon is fully open, making it possible for other researchers, developers and institutions to develop and inspect code, training data and methodology” (“K2 Horizon est entièrement ouvert, ce qui permet à d’autres chercheurs, développeurs et institutions de développer et d’inspecter le code, les données d’entraînement et la méthodologie”), selon des propos rapportés par The National.

Hector Liu, directeur du laboratoire de recherche d’IFM dans la Silicon Valley, a précisé la logique de conception derrière chaque taille de modèle : “Every model is built to compete with the best open models at its size, and everyone ships with the weights, code, training data and methodology behind it” (“Chaque modèle est conçu pour rivaliser avec les meilleurs modèles ouverts de sa catégorie de taille, et chacun est livré avec les poids, le code, les données d’entraînement et la méthodologie qui les sous-tendent”), également cité par The National.

Ces deux prises de parole insistent sur le même point : IFM ne cherche pas à revendiquer le titre de meilleur modèle toutes catégories confondues, mais à imposer un standard de transparence que peu d’acteurs de cette taille acceptent aujourd’hui d’appliquer. Une analyse détaillée du lancement est également disponible sur MarkTechPost.

## Le contexte : une cadence de sorties devenue effrénée

Pour comprendre pourquoi K2 Horizon fait parler de lui, il faut le replacer dans le rythme de sorties de modèles observé depuis le printemps 2026. Entre fin mai et début juin, plusieurs modèles majeurs sont sortis coup sur coup : MiniMax M2.5, M2.7 et M3 Highspeed, Nvidia Nemotron 3 Ultra 550B et Google Gemma 4 12B, portant la cadence à un “LLM majeur” toutes les 48 heures selon certains observateurs spécialisés. Le mois d’août 2026 a confirmé cette tendance avec neuf lancements distincts, incluant GLM-5.3, Qwen3.8-27B, Gemini 3.7 Flash, Grok 4.6 et Qwen3.8-Max.

Dans ce contexte de surproduction de modèles, la stratégie d’IFM tranche par son absence de course à la taille brute. L’institut préfère miser sur une gamme complète et une documentation exhaustive, un pari qui rappelle les débuts de la vague open source occidentale mais appliqué à une échelle et une transparence inédites.

## Un signal fort pour le débat français sur l’IA souveraine

La présence d’un laboratoire IFM à Paris change la donne pour les acteurs français qui cherchent des alternatives crédibles aux modèles fermés américains. Les nouvelles obligations européennes visent en priorité les modèles les plus récents disponibles en France, à savoir GPT-5.6, Claude Opus 5, DeepSeek V4-Pro et les nouveaux modèles de Google, tous désormais dans le viseur réglementaire de Bruxelles. Dans ce climat, un modèle entièrement ouvert et documenté comme K2 Horizon, produit par un institut ayant pignon sur rue à Paris, coche plusieurs cases recherchées par les administrations et les entreprises soucieuses de conformité et de traçabilité des données d’entraînement.

Cela dit, les sources disponibles ne montrent pas encore d’adoption confirmée par un ministère français, une institution européenne ou une grande entreprise du CAC 40. Ce qui est documenté avec certitude, c’est la présence opérationnelle d’IFM sur le sol français et une empreinte de lancement mondiale, pas encore des métriques d’adoption concrètes en Europe. Le parallèle avec LLMs4Europe, le programme doté de 20 millions d’euros lancé par la Commission européenne fin août 2026 avec plus de 70 partenaires, est frappant : les deux initiatives misent sur l’ouverture complète comme réponse à la dépendance technologique, mais l’une est un consortium public européen quand l’autre est un institut de recherche financé depuis le Golfe.

## Impact sur le marché des modèles ouverts européens

L’arrivée de K2 Horizon complique la position de Mistral AI sur son propre terrain de prédilection : l’IA ouverte et documentée. Mistral Large 3, publié sous licence Apache 2.0 fin juillet 2026 avec une fenêtre de contexte d’un million de tokens, reste l’un des rares grands modèles européens disponibles à la fois en open source et à l’échelle commerciale. Mais K2 Horizon pousse le curseur de la transparence encore plus loin en publiant systématiquement les données d’entraînement et les recettes de construction, quelque chose que peu d’éditeurs, européens ou non, acceptent de faire à cette échelle. Le cas de Quasar 438B, présenté début septembre comme le modèle européen le mieux classé sur l’Artificial Analysis Intelligence Index mais limité à deux langues, montre que la course à l’ouverture ne se joue pas uniquement sur les scores bruts.

Pour les startups françaises et européennes qui construisent des produits sur des fondations ouvertes, cette diversification de l’offre est une bonne nouvelle à court terme : plus de choix, plus de concurrence sur les prix d’hébergement, et une pression à la baisse sur le coût d’accès à des modèles performants. À moyen terme, elle pourrait aussi complexifier le paysage réglementaire, chaque nouvel entrant devant démontrer sa conformité à l’AI Act européen indépendamment de son pays d’origine. Notre analyse des modèles d’entreprise IBM Granite 4.2 montre une dynamique similaire du côté des acteurs américains qui misent eux aussi sur l’ouverture pour regagner la confiance des DSI européens.

## Historique : de Falcon 40B à K2 Horizon, la trajectoire du Golfe

Abou Dabi n’en est pas à son coup d’essai dans l’IA ouverte. Dès 2023, le Technology Innovation Institute avait publié Falcon 40B en open source, une première étape qui avait positionné l’émirat comme un acteur sérieux du paysage IA mondial, bien avant l’explosion actuelle des modèles ouverts chinois et américains. MBZUAI, l’université qui chapeaute IFM, avait annoncé la création de l’institut dès 2023 avant son lancement effectif en mai 2025, avec l’ambition explicite de construire, comprendre et gérer les risques liés aux modèles de fondation avancés.

Cette continuité stratégique montre qu’Abou Dabi ne considère pas l’IA ouverte comme un coup de communication ponctuel mais comme un axe d’investissement de long terme, avec deux organisations distinctes (TII et IFM) poursuivant des approches complémentaires : Falcon plutôt tourné vers les usages régionaux et la langue arabe, K2 Horizon plutôt tourné vers une couverture matérielle complète et une transparence scientifique maximale.

## Ce que cela signifie pour les développeurs européens

Pour un développeur ou une équipe data en France, la disponibilité immédiate de K2 Horizon sur Hugging Face avec un support natif pour vLLM, SGLang et Ollama signifie qu’il est possible de tester ces modèles dès aujourd’hui sans attendre une intégration commerciale locale. Le modèle 7B, avec sa fenêtre de contexte de plus de 524 000 tokens, se prête particulièrement bien à des cas d’usage d’entreprise comme l’analyse de longs documents contractuels ou la synthèse de bases de connaissances internes, sans dépendre d’un fournisseur cloud américain.

Le modèle 32B, positionné pour l’hébergement local, pourrait aussi intéresser les hébergeurs français type OVHcloud ou Scaleway qui cherchent à enrichir leur catalogue de modèles souverains hébergeables sur sol européen, même si le modèle lui-même n’est pas d’origine européenne. C’est précisément cette ambiguïté entre origine géographique du modèle et souveraineté de l’hébergement qui alimente les débats en cours à Bruxelles et à Paris.

## Nos prédictions pour les prochains mois

Sur la base de la trajectoire observée depuis le lancement du 3 septembre 2026, plusieurs évolutions nous semblent probables dans les mois qui viennent.

- IFM devrait publier des rapports techniques complets avec des tableaux de benchmarks standardisés (MMLU, GPQA, SWE-Bench) pour combler le vide actuel laissé par des évaluations encore partielles et menées par des tiers.
- La présence du laboratoire parisien d’IFM devrait se traduire par des annonces de partenariats avec des acteurs académiques ou industriels français dans les six à douze prochains mois, étant donné l’intérêt manifeste de la France pour des alternatives ouvertes non américaines.
- Mistral AI devrait accélérer la publication de nouvelles variantes ouvertes pour ne pas laisser le terrain de la transparence totale à un institut basé dans le Golfe, notamment sur le segment des petits modèles embarqués face au K2 Horizon 0,9B.
- Le rythme de sorties de modèles majeurs devrait rester extrêmement soutenu jusqu’à la fin de l’année 2026, avec un risque de lassitude croissant chez les entreprises qui peinent à suivre et évaluer chaque nouvelle famille de modèles.
- Les régulateurs européens pourraient commencer à distinguer explicitement, dans leurs textes d’application de l’AI Act, les modèles “ouverts aux poids” des modèles “totalement ouverts” comme K2 Horizon, ce dernier niveau de transparence facilitant grandement les obligations d’audit prévues par la réglementation.

## Les limites à garder à l’esprit

Malgré la communication enthousiaste autour de cette sortie, plusieurs zones d’ombre subsistent. La composition exacte des jeux de données d’entraînement n’a pas été détaillée dans son intégralité : IFM communique sur le principe de la divulgation (poids, code, données, recettes) sans fournir de répartition précise du corpus en pourcentages ou en nombre de tokens pour chaque source. Cette transparence “de principe” n’équivaut donc pas encore à une traçabilité totale, ligne par ligne, des données utilisées.

De la même façon, l’absence de benchmarks officiels complets à la date du lancement oblige à s’appuyer sur des évaluations indépendantes, qui peuvent varier selon la méthodologie et les jeux de test choisis. Il faudra donc attendre les prochaines semaines pour obtenir une image plus stabilisée des performances réelles de chaque modèle de la famille K2 Horizon face à ses concurrents directs. Pour comparer ces performances à d’autres institutions académiques travaillant sur l’IA en Europe, voir également notre couverture du benchmark EU MMLU lancé par la Commission européenne.

## Foire aux questions

**Qu’est-ce que K2 Horizon exactement ?**

K2 Horizon est une famille de six modèles d’IA à poids ouverts publiée le 3 septembre 2026 par l’Institute of Foundation Models (IFM), un laboratoire de recherche rattaché à MBZUAI à Abou Dabi. Les modèles vont de 0,9 à 375 milliards de paramètres et sont diffusés sous licence Apache 2.0.

**K2 Horizon a-t-il un lien avec Kimi K2 de Moonshot AI ?**

Non. Il s’agit de deux projets totalement indépendants, développés par des organisations différentes (IFM pour K2 Horizon, Moonshot AI pour Kimi K2/K3), qui partagent uniquement une partie de leur nom par coïncidence.

**K2 Horizon est-il lié aux modèles Falcon ?**

Non plus. Falcon est développé par le Technology Innovation Institute (TII), rattaché à l’Advanced Technology Research Council d’Abou Dabi, tandis que K2 Horizon vient d’IFM, un institut dépendant de MBZUAI. Les deux organisations sont basées à Abou Dabi mais restent indépendantes l’une de l’autre.

**Où peut-on télécharger K2 Horizon ?**

Les six modèles sont disponibles gratuitement sur Hugging Face, avec un support natif pour les frameworks d’inférence vLLM, SGLang et Ollama dès le jour du lancement.

**Existe-t-il une API payante pour K2 Horizon ?**

Aucune tarification d’API commerciale de premier niveau n’avait été communiquée par IFM au moment du lancement. Le modèle repose actuellement sur l’auto-hébergement des poids ouverts plutôt que sur un service payant à l’usage.

**K2 Horizon est-il meilleur que Mistral Large 3 ou DeepSeek V4 ?**

Les données publiques disponibles début septembre 2026 ne permettent pas d’affirmer une supériorité généralisée. Le modèle phare 375B-A23B affiche des résultats encourageants sur les tâches agentiques, mais reste plus petit en nombre de paramètres que DeepSeek V4 Pro ou GLM-5.3. Sa force distinctive est le niveau de transparence de la publication, pas nécessairement le score brut sur chaque benchmark.

**Pourquoi IFM a-t-il un laboratoire à Paris ?**

IFM opère depuis trois sites (Abou Dabi, Silicon Valley et Paris) depuis son lancement officiel en mai 2025 par MBZUAI, dans le cadre d’une stratégie d’implantation internationale visant à recruter des chercheurs et à nouer des liens avec l’écosystème européen de l’IA.

**Quel modèle K2 Horizon convient à une petite entreprise française ?**

Pour un déploiement local sans API tierce, le modèle 7B (contexte de plus de 524 000 tokens) ou le modèle 32B (pensé pour l’hébergement on-premise) sont les choix les plus réalistes pour une PME souhaitant garder le contrôle de ses données sans investir dans une infrastructure GPU massive.