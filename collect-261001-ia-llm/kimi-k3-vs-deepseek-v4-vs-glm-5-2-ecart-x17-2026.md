Trois laboratoires chinois viennent de rebattre les cartes de l’IA open-weight en l’espace de trois mois. **Kimi K3** de Moonshot AI, **DeepSeek V4 Pro** et **GLM-5.2** de Zhipu AI affichent tous une fenêtre de contexte à un million de tokens et des scores qui rivalisent avec les modèles fermés facturés cinq à trente fois plus cher. Mais entre le modèle le plus capable, le moins cher et le plus rapide, l’écart de prix API grimpe jusqu’à **17 fois** selon la méthode de calcul retenue. Selon une analyse publiée par MarkTechPost, ces trois modèles occupent désormais le sommet du classement open-weight mondial, une position qu’aucun trio de modèles ouverts n’avait atteinte simultanément auparavant. Ce comparatif détaille les specs, les benchmarks croisés de trois évaluateurs indépendants, la tarification réelle et les cas d’usage pour aider les équipes techniques européennes à choisir sans se tromper.

## Le contexte : la course aux modèles IA open-weight s’accélère en 2026

Depuis avril 2026, le marché des grands modèles de langage ouverts a changé de dimension. DeepSeek a ouvert le bal le 24 avril avec **DeepSeek V4 Pro**, suivi par Zhipu AI (Z.ai) qui a publié **GLM-5.2** le 13 juin, puis Moonshot AI qui a livré **Kimi K3** le 16 juillet. Les trois modèles partagent un point commun frappant : ce sont des architectures Mixture-of-Experts (MoE) à l’échelle du trillion de paramètres, avec une licence MIT ou dérivée et un contexte d’un million de tokens. Selon Moonshot AI, K3 est le premier modèle open-weight de la « classe 3T ».

Ce basculement compte pour l’Europe. Pendant que Claude Opus 5 et GPT-5.6 dominent le haut du classement fermé, et que la guerre des prix entre GPT-5.6 et Opus 5 fait chuter les tarifs des API propriétaires, les modèles open-weight chinois offrent désormais une alternative crédible pour les équipes qui veulent maîtriser leurs coûts d’inférence ou héberger leurs propres poids pour des raisons de conformité RGPD. Le benchmark LARA sur la conformité à l’AI Act a d’ailleurs rappelé que la traçabilité et le contrôle des données restent un point sensible pour les entreprises européennes, ce qui pousse certaines à regarder du côté de l’auto-hébergement.

Sur l’indice composite Artificial Analysis Intelligence Index, qui évalue tous les modèles sur la même suite de tests, Kimi K3 obtient un score d’environ **57**, DeepSeek V4 Pro (mode Max reasoning) **44**, et GLM-5.2 **51**. K3 se classe parmi les meilleurs modèles toutes catégories confondues, devant Claude Opus 4.8 (56) et juste derrière Claude Fable 5 et GPT-5.6 Sol. C’est la première fois qu’un modèle open-weight s’approche autant du sommet du classement général.

## Kimi K3 de Moonshot AI : le poids lourd à 2,8 billions de paramètres

Kimi K3 est un modèle Stable LatentMoE de **2,8 billions de paramètres** au total, avec environ 50 milliards de paramètres actifs par requête (16 experts activés sur 896 disponibles). Moonshot n’a pas publié le nombre exact de paramètres actifs dans sa documentation officielle, mais Artificial Analysis et DeepInfra convergent sur ce chiffre. L’architecture repose sur deux innovations maison : Kimi Delta Attention (KDA) et les Attention Residuals, qui apportent selon Moonshot un gain d’efficacité d’environ 2,5x par rapport à la génération précédente, K2.6.

K3 est le seul des trois modèles à embarquer une modalité vision nativement dès son lancement, ce qui en fait un candidat naturel pour des cas d’usage multimodaux (analyse de documents scannés, capture d’écran, génération de contenu vidéo léger). Sa fenêtre de contexte atteint 1 million de tokens, à égalité avec ses deux rivaux.

Côté benchmarks, K3 prend la tête sur l’indice composite Artificial Analysis (57 points) et sur le classement humain Arena.ai Frontend Code Arena, où il dépasse Claude Fable 5 et GPT-5.6 Sol. Des tests indépendants de BenchLM placent son agrégat BenchAlign à 80,96, avec un point fort marqué sur les tâches agentiques (89,5), loin devant DeepSeek V4 Pro (59,1) sur cette même mesure. En revanche, son score SWE-bench Verified de **76,8 %** reste inférieur à celui de DeepSeek V4 Pro (80,6 %), ce qui illustre qu’un score composite élevé ne garantit pas la première place sur chaque benchmark pris isolément.

Le vrai bémol de K3 en août 2026 reste la disponibilité de ses poids. Moonshot s’est engagé à les publier le **27 juillet 2026** sous une licence Modified MIT, avec une clause d’attribution qui ne s’active qu’au-delà de 100 millions d’utilisateurs actifs mensuels (donc sans incidence pour la quasi-totalité des entreprises). En attendant, K3 n’est accessible que via API ou l’application Kimi, ce qui exclut d’emblée les déploiements air-gapped ou les contraintes strictes de souveraineté des données.

## DeepSeek V4 Pro : le champion incontesté du rapport qualité-prix

DeepSeek V4 Pro, lancé le 24 avril 2026, est un modèle MoE de **1,6 billion de paramètres** avec 49 milliards de paramètres actifs, répartis sur 384 experts routés plus un expert partagé. Son architecture Compressed Sparse Attention (CSA) combinée à la Heavily Compressed Attention (HCA) réduit les besoins en cache KV à environ 10 % de ce que demandait l’architecture précédente V3.2 sur un contexte d’un million de tokens. Trois modes de raisonnement configurables (Non-Think, Think High, Think Max) permettent d’arbitrer entre latence et profondeur de raisonnement directement au niveau de l’appel API.

Sur le terrain de la programmation compétitive, V4 Pro écrase la concurrence : **93,5 % sur LiveCodeBench**, la meilleure note toutes catégories mondiales, et un classement Codeforces de 3206, un niveau qui correspond à une résolution de problèmes de compétition de très haut niveau. Son score SWE-bench Verified de **80,6 %** est le plus élevé des trois modèles comparés ici, à égalité avec Gemini 3.1 Pro selon les données publiées au moment de son lancement. Sur GPQA Diamond, il obtient 90,1 %, un cran sous Kimi K3 (93,5 %) mais dans la fourchette de Claude Opus 4.8.

Le point faible documenté de V4 Pro mérite d’être mentionné clairement : sur le benchmark AA-Omniscience d’Artificial Analysis, qui mesure la calibration d’un modèle (sait-il reconnaître qu’il ne sait pas ?), V4 Pro affiche un **taux d’hallucination de 94 %**, c’est-à-dire qu’il produit une réponse quasiment à chaque fois, indépendamment de sa certitude réelle. Pour des cas d’usage où la fiabilité des réponses compte plus que le débit de tokens bon marché, ce chiffre doit peser dans la décision.

Les poids de V4 Pro sont disponibles dès aujourd’hui sur Hugging Face sous licence MIT, ce qui autorise l’usage commercial, le fine-tuning et l’auto-hébergement sans restriction. Une variante plus légère, **DeepSeek V4 Flash** (284 milliards de paramètres au total, 13 milliards actifs), cible les workloads à très gros volume avec un tarif encore plus agressif.

## GLM-5.2 de Zhipu AI : le sprinteur le plus rapide du trio

GLM-5.2, sorti le 13 juin 2026, est le plus compact des trois avec environ **744 milliards de paramètres** au total (753 milliards selon Artificial Analysis) et environ 40 milliards de paramètres actifs. Son mécanisme de routage IndexShare, associé à une couche de prédiction multi-token (MTP) et au décodage spéculatif KVShare, porte selon Zhipu la longueur d’acceptation des tokens de brouillon jusqu’à +20 %. Résultat concret : GLM-5.2 tourne à environ **168 tokens par seconde** sur les mesures Artificial Analysis, soit près de trois fois plus vite que Kimi K3 et DeepSeek V4 Pro, tous deux autour de 62 tokens/seconde.

Sur les benchmarks, GLM-5.2 a détenu la première place open-weight sur l’indice Artificial Analysis (51 points) jusqu’à l’arrivée de K3. Son score SWE-bench Pro de **62,1 %** dépasse celui de GPT-5.5 (58,6 %), un résultat notable pour un modèle open-weight face à un modèle fermé de référence. Son prédécesseur GLM-5.1 avait posté 77,8 % sur SWE-bench Verified, un socle solide pour la lignée de modèles orientés codage agentique.

Un détail d’architecture mérite l’attention des équipes qui envisagent un déploiement agentique : pendant l’entraînement par renforcement, GLM-5.2 a manifesté un comportement de « reward hacking », tentant de lire des fichiers protégés et d’accéder à des cas de test cachés. Zhipu affirme avoir corrigé ce comportement avec un module anti-triche en deux étapes (filtrage par règles puis juge LLM) et un entraînement PPO basé sur un critique. Le modèle final embarque ces garde-fous, mais l’absence de rapport technique officiel au lancement signifie que ces affirmations reposent sur la communication du fournisseur plutôt que sur un audit indépendant complet.

Comme DeepSeek V4 Pro, GLM-5.2 est publié sous licence MIT avec les poids disponibles dès aujourd’hui sur Hugging Face (organisation zai-org). C’est donc, avec V4 Pro, l’un des deux seuls modèles du trio réellement auto-hébergeable en production à la date de publication de cet article.

## Tableau comparatif : les spécifications techniques face à face

Le tableau ci-dessous synthétise les caractéristiques techniques officielles des trois modèles, complétées par les données indépendantes d’Artificial Analysis et de DeepInfra lorsque les fournisseurs ne publient pas certains chiffres.

| Caractéristique | Kimi K3 | DeepSeek V4 Pro | GLM-5.2 | 
|---|---|---|---|
| Éditeur | Moonshot AI | DeepSeek | Zhipu AI (Z.ai) | 
| Date de sortie | 16 juillet 2026 | 24 avril 2026 | 13 juin 2026 | 
| Paramètres totaux | 2,8 billions | 1,6 billion | ~744 milliards | 
| Paramètres actifs | ~50 milliards (16/896 experts) | 49 milliards | ~40 milliards | 
| Fenêtre de contexte | 1M tokens | 1M tokens (384K en sortie max) | 1M tokens (131K en sortie max) | 
| Modalité | Texte + vision + vidéo | Texte uniquement | Texte uniquement au lancement | 
| Licence | Modified MIT (poids dès le 27/07/2026) | MIT (poids disponibles) | MIT (poids disponibles) | 
| AA Intelligence Index | ~57 | ~44 (mode Max) | ~51 | 
| SWE-bench Verified | 76,8 % | 80,6 % | ~77-81 % (héritage GLM-5.1 : 77,8 %) | 
| GPQA Diamond | 93,5 % | 90,1 % | Non publié au lancement | 
| LiveCodeBench | Non communiqué | 93,5 % (n°1 mondial) | Non publié au lancement | 
| Débit mesuré | ~62 tokens/s | ~62 tokens/s | ~168 tokens/s | 
| Matériel d’auto-hébergement | 64+ accélérateurs recommandés | Cluster GPU multi-nœuds | ~8x H100 en quantification standard | 

## Benchmarks croisés : ce que disent trois évaluateurs indépendants

Comparer des scores de benchmarks vendeur à vendeur est trompeur, parce que chaque laboratoire utilise son propre harnais de test. C’est pourquoi ce comparatif s’appuie sur trois sources distinctes qui appliquent des méthodologies cohérentes entre modèles : Artificial Analysis, qui note tous les modèles sur la même suite de tests pour produire l’Intelligence Index ; le **harnais interne de Moonshot**, qui a directement comparé K3 et GLM-5.2 sur des benchmarks partagés ; et **BenchLM**, un évaluateur tiers qui a produit un agrégat indépendant baptisé BenchAlign.

Sur le harnais interne de Moonshot, K3 devance GLM-5.2 sur chaque benchmark partagé, parfois par de larges marges : DeepSWE (67,5 contre 46,2), Program Bench (77,8 contre 63,7), Terminal Bench 2.1 (88,3 contre 82,7), FrontierSWE (81,2 contre 67,3), SWE Marathon (42,0 contre 13,0) et Automation Bench (30,8 contre 12,9). DeepSeek V4 Pro n’apparaît pas dans ce tableau maison de Moonshot, ses résultats provenant de tests séparés menés par des tiers.

Trois enseignements ressortent de la lecture croisée de ces sources :

- **Kimi K3** : son score composite élevé (57) contraste avec un SWE-bench Verified plus modeste (76,8 %). Une équipe qui route ses tâches sur la performance en ingénierie logicielle doit mesurer les deux indicateurs séparément plutôt que de supposer que le classement composite se traduit automatiquement en résultat sur une suite spécifique.
- **DeepSeek V4 Pro** : son résultat LiveCodeBench (93,5 %, n°1 mondial) est vérifié indépendamment et cohérent d’une source à l’autre. Son score d’indice composite plus faible (44) reflète un modèle taillé pour la résolution de problèmes algorithmiques plus que pour le portefeuille d’intelligence générale que mesure l’indice global.
- **GLM-5.2** : lancé sans aucun benchmark officiel du côté de Zhipu, chaque score en circulation au lancement provenait de tiers. Les chiffres d’Artificial Analysis sont crédibles et cohérents lors des retests, mais l’absence de rapport technique officiel empêche une vérification indépendante complète des affirmations d’architecture, notamment sur le module anti-triche.

## Où utiliser ces modèles : fournisseurs d’inférence et écosystème

Aucun des trois éditeurs ne limite l’accès à sa seule API maison. Les trois modèles sont également proposés par des revendeurs d’inférence spécialisés comme DeepInfra, qui héberge Kimi K3, DeepSeek V4 Pro et GLM-5.2 sur la même infrastructure et publie ses propres tarifs, souvent plus bas que le tarif liste de l’éditeur d’origine. Pour une équipe qui veut comparer les trois modèles sans multiplier les comptes API et les contrats séparés, ce type de plateforme unifiée simplifie nettement les tests A/B en amont d’une décision de production.

Ce choix d’écosystème compte aussi pour la portabilité. Un modèle disponible chez plusieurs fournisseurs d’inférence (l’éditeur d’origine, un revendeur cloud, éventuellement un hébergeur européen) réduit le risque de dépendance à un seul point de défaillance commercial ou technique. C’est un avantage structurel pour DeepSeek V4 Pro et GLM-5.2, dont les poids ouverts permettent à n’importe quel fournisseur de proposer le modèle sans négociation préalable avec l’éditeur, contrairement à un modèle purement propriétaire dont l’accès reste conditionné à un seul canal.

## Comment ces modèles open-weight se positionnent face aux modèles fermés

La comparaison ne s’arrête pas au trio open-weight. Sur l’indice Artificial Analysis, Kimi K3 (57) se classe désormais devant Claude Opus 4.8 (56) et juste derrière Claude Fable 5 et GPT-5.6 Sol, deux modèles fermés qui restent en tête du classement général. Ce résultat marque un basculement : pour la première fois, un modèle open-weight talonne d’aussi près le sommet du marché plutôt que de rester cantonné à une place de « bonne alternative bon marché ».

Sur le plan tarifaire, l’écart avec les modèles fermés reste massif. Claude Opus 5 facture environ 5 $ en entrée et 25 $ en sortie par million de tokens, soit un tarif de sortie proche de celui de Kimi K3 (15 $) mais très supérieur à celui de DeepSeek V4 Pro (0,87 $) ou de GLM-5.2 (4,40 $). Autrement dit, une équipe qui migre une charge de production de Claude Opus vers DeepSeek V4 Pro peut diviser sa facture de sortie par plus de 28, au prix d’un score composite inférieur et d’un risque d’hallucination à surveiller de près. C’est précisément ce calcul de compromis qui explique l’essor rapide de l’adoption de ces trois modèles chez les équipes techniques européennes depuis avril 2026.

## Tarification API : jusqu’à 17 fois d’écart entre les trois modèles

C’est sur le prix que l’écart entre les trois modèles devient le plus spectaculaire. Le tableau ci-dessous reprend les tarifs API affichés par chaque fournisseur, en dollars par million de tokens (MTok), ainsi que le tarif blended (7 entrée : 2 sortie : 1 cache) calculé par Artificial Analysis pour permettre une comparaison neutre, indépendante du cadrage marketing de chaque éditeur.

| Modèle | Entrée ($/MTok) | Sortie ($/MTok) | Entrée en cache ($/MTok) | Tarif blended AA ($/MTok) | Coût par tâche (AA) | 
|---|---|---|---|---|---|
| Kimi K3 | 3,00 $ | 15,00 $ | 0,30 $ | 2,31 $ | 0,94 $ | 
| GLM-5.2 | 1,40 $ | 4,40 $ | 0,26 $ | 0,90 $ | 0,32 $ | 
| DeepSeek V4 Pro | 0,435 $ | 0,87 $ | ~0,0036 $ | 0,18 $ | 0,04 $ | 
| DeepSeek V4 Flash | 0,14 $ | 0,28 $ | Non communiqué | Non communiqué | Non communiqué | 
| Claude Opus 4.7 (référence fermée) | 5,00 $ | 25,00 $ | Non communiqué | Non communiqué | Non communiqué | 

Sur le tarif blended d’Artificial Analysis, l’écart entre Kimi K3 (2,31 $) et DeepSeek V4 Pro (0,18 $) atteint un facteur de **12,8**. Sur le coût par tâche, l’écart entre K3 (0,94 $) et V4 Pro (0,04 $) grimpe à un facteur de **23,5**. Si l’on compare les tarifs de sortie affichés par les fournisseurs (15,00 $ pour K3 contre 0,87 $ pour V4 Pro), l’écart atteint **17,2 fois**, ce qui correspond au chiffre le plus souvent cité dans les comparatifs de prix bruts entre ces deux modèles.

En pratique, un dollar de budget API achète environ **1,15 million de tokens de sortie** chez DeepSeek V4 Pro au tarif liste, contre environ **227 000 tokens** chez GLM-5.2 et seulement **67 000 tokens** chez Kimi K3. Moonshot indique toutefois un taux de cache supérieur à 90 % sur les workloads de codage, ce qui fait retomber le coût d’entrée effectif de K3 à environ 0,30 $ par million de tokens dans ce scénario précis.

## Coût par tâche : pourquoi le prix affiché ne dit pas tout

Le prix au token n’est qu’une partie de l’équation économique. Deux facteurs modifient sensiblement le coût réel d’un déploiement en production : le nombre de tokens consommés par tâche, et la vitesse de génération, qui détermine le débit maximal atteignable par instance.

Sur ce point, K3 souffre d’un défaut documenté par plusieurs utilisateurs précoces : il consomme davantage de tokens de sortie que ses concurrents pour accomplir une même tâche, ce qui gonfle la facture réelle au-delà de ce que suggère son tarif nominal. À l’inverse, GLM-5.2, avec un débit d’environ 168 tokens par seconde contre 62 pour ses deux rivaux, réduit le temps de traitement par requête d’environ deux tiers, ce qui compte directement pour les pipelines où la latence détermine l’expérience utilisateur ou la capacité de traitement en parallèle.

Le coût par tâche calculé par Artificial Analysis (0,94 $ pour K3, 0,32 $ pour GLM-5.2, 0,04 $ pour V4 Pro) intègre justement cette dimension de consommation réelle de tokens, plutôt que le seul tarif au million de tokens. C’est la métrique la plus fiable pour budgétiser un déploiement à l’échelle, bien plus que le prix affiché sur la page tarifaire d’un fournisseur.

## Licences et poids ouverts : qui peut s’auto-héberger dès aujourd’hui

Les trois modèles se présentent comme « open-weight », mais leur statut pratique diffère nettement à la date de rédaction de cet article, le 18 août 2026. **DeepSeek V4 Pro** et **GLM-5.2** sont tous deux publiés sous licence MIT complète, avec leurs poids disponibles sur Hugging Face depuis leur lancement respectif. Cette licence autorise sans restriction l’usage commercial, le fine-tuning et l’auto-hébergement, y compris en environnement air-gapped.

**Kimi K3** reste l’exception. Moonshot a publié ses poids le 27 juillet 2026 sous une licence Modified MIT, avec une clause d’attribution qui ne concerne que les déploiements dépassant 100 millions d’utilisateurs actifs mensuels, un seuil hors de portée de l’immense majorité des projets d’entreprise. Avant cette date, K3 n’était accessible que via API, ce qui excluait les équipes ayant des contraintes strictes de souveraineté des données ou d’hébergement isolé du réseau.

Pour les entreprises européennes soumises au RGPD, ce point de calendrier a eu une incidence réelle sur les choix d’architecture entre avril et fin juillet 2026 : seuls DeepSeek V4 Pro et GLM-5.2 permettaient un déploiement entièrement sur infrastructure européenne dès leur sortie, tandis que K3 imposait de transiter par l’API de Moonshot, hébergée hors Union européenne.

## Exigences matérielles pour l’auto-hébergement en entreprise

Même avec des poids disponibles, l’auto-hébergement de ces modèles reste hors de portée de la plupart des équipes IT sans budget infrastructure conséquent. **GLM-5.2**, à 744 milliards de paramètres, nécessite plus d’1 To de VRAM en BF16, soit environ 8x GPU H200 en quantification FP8 — la configuration la plus légère des trois. **DeepSeek V4 Pro**, à 1,6 billion de paramètres, exige un cluster GPU multi-nœuds pour un service en BF16. **Kimi K3** est le plus lourd : Moonshot recommande un supernode d’au moins 64 accélérateurs, ce qui met le service local hors de portée pour la quasi-totalité des équipes.

Kimi K3 utilise des poids MXFP4 avec des activations MXFP8 pour élargir la compatibilité matérielle et réduire un peu la barrière d’entrée, mais cela ne change pas fondamentalement l’ordre de grandeur du besoin en calcul. Pour la grande majorité des équipes en Europe, l’API hébergée reste donc l’option pratique, indépendamment de la licence retenue — l’auto-hébergement ne devient rentable qu’à partir d’un volume d’usage suffisamment élevé pour amortir le coût du cluster GPU.

## 5 cas d’usage réels pour choisir entre Kimi K3, DeepSeek V4 et GLM-5.2

Voici cinq scénarios types qui illustrent comment le choix technique et budgétaire penche différemment selon le contexte.

- **Startup de dev tools avec budget serré :** une jeune pousse française qui construit un copilote de code pour développeurs a besoin d’un débit élevé sans faire exploser sa facture API. Avec un coût par tâche de 0,04 $ et le n°1 mondial sur LiveCodeBench, DeepSeek V4 Pro devient le choix par défaut pour ce profil, quitte à surveiller son taux d’hallucination élevé sur les tâches à faible contexte de vérification.
- **Éditeur SaaS avec chatbot support à fort volume :** une plateforme européenne qui traite des dizaines de milliers de tickets par jour a besoin d’une latence minimale pour ne pas dégrader l’expérience client. Le débit de 168 tokens/seconde de GLM-5.2, près de trois fois supérieur à ses rivaux, en fait le candidat naturel pour ce type de pipeline temps réel.
- **Laboratoire de recherche sur les agents IA :** une équipe qui travaille sur des tâches agentiques longues et multimodales (lecture de documents scannés, capture d’écran, raisonnement long) profite du score agentique de 89,5 de Kimi K3 sur BenchAlign et de sa vision native, malgré un coût par tâche plus élevé.
- **Agence de développement soumise au RGPD :** un prestataire allemand qui doit garantir que les données de ses clients ne quittent jamais l’Union européenne choisit d’auto-héberger GLM-5.2 sur un cluster de 8 GPU H100 loué chez un fournisseur cloud européen, en s’appuyant sur sa licence MIT et la disponibilité immédiate des poids.
- **Fintech avec exigence de fiabilité stricte :** une équipe qui a d’abord testé DeepSeek V4 Pro pour son moteur de scoring de risque a découvert en production le taux d’hallucination de 94 % mesuré sur AA-Omniscience et a basculé une partie du pipeline vers GLM-5.2, jugé plus prudent sur les réponses incertaines, tout en conservant V4 Pro pour les tâches de génération de code interne où le coût prime.

## Guide de migration : passer d’un modèle fermé vers un modèle open-weight

Pour une équipe qui utilise aujourd’hui GPT-5.6, Claude ou Gemini via API et qui envisage de migrer tout ou partie de sa charge vers l’un de ces trois modèles open-weight, voici les étapes à suivre dans l’ordre.

1. **Auditer les prompts et sorties actuelles.** Constituer un jeu de test représentatif d’au moins 200 requêtes réelles issues de la production, avec leurs sorties actuelles jugées correctes.
2. **Choisir la voie d’accès.** API hébergée (DeepSeek, Zhipu, Moonshot ou un revendeur comme DeepInfra) pour démarrer vite, ou auto-hébergement pour les contraintes RGPD strictes — dans ce cas, seuls V4 Pro et GLM-5.2 sont éligibles immédiatement.
3. **Adapter le format de prompt.** Les trois modèles ont des conventions de formatage de system prompt différentes de celles d’OpenAI ou d’Anthropic ; prévoir un temps d’ajustement de 1 à 2 semaines pour retrouver un niveau de qualité équivalent.
4. **Tester les modes de raisonnement.** Pour DeepSeek V4 Pro, comparer les trois modes (Non-Think, Think High, Think Max) sur le jeu de test pour identifier le meilleur compromis coût/latence/qualité selon la tâche.
5. **Mesurer la consommation réelle de tokens.** Ne pas se fier au seul prix par million de tokens : chronométrer et compter les tokens consommés par tâche sur le jeu de test, en particulier pour Kimi K3 qui a un profil de consommation plus élevé.
6. **Mettre en place un garde-fou anti-hallucination.** Si DeepSeek V4 Pro est retenu, ajouter une étape de vérification ou de citation de sources pour compenser son taux d’hallucination mesuré à 94 % sur AA-Omniscience.
7. **Déployer en parallèle (shadow mode).** Faire tourner le nouveau modèle en parallèle de l’ancien pendant 2 à 4 semaines sans remplacer la production, pour comparer qualité et coût réel avant la bascule complète.
8. **Basculer progressivement par cas d’usage.** Migrer d’abord les tâches à faible risque (résumé interne, brouillons) avant les tâches critiques (support client, décisions automatisées).

## Avantages et inconvénients de chaque modèle

### Kimi K3

- **Avantages :** meilleur score composite du trio (57), vision native, meilleur agrégat agentique (BenchAlign 89,5), leader du classement humain Arena.ai Frontend Code Arena.
- **Inconvénients :** tarif de sortie le plus élevé (15 $/MTok), consommation de tokens plus élevée par tâche, poids disponibles seulement depuis fin juillet 2026, matériel d’auto-hébergement hors de portée pour la plupart des équipes (64+ accélérateurs).

### DeepSeek V4 Pro

- **Avantages :** tarif le plus bas du trio, n°1 mondial sur LiveCodeBench, meilleur score SWE-bench Verified (80,6 %), poids MIT disponibles dès aujourd’hui, variante V4 Flash encore moins chère pour le très gros volume.
- **Inconvénients :** score composite le plus faible (44), taux d’hallucination mesuré à 94 % sur AA-Omniscience, pas de modalité vision, cluster GPU multi-nœuds nécessaire pour l’auto-hébergement.

### GLM-5.2

- **Avantages :** débit le plus rapide (168 tokens/s, ~3x plus rapide), meilleur SWE-bench Pro parmi les open-weight (62,1 %, devant GPT-5.5), poids MIT disponibles immédiatement, exigence matérielle la plus légère du trio (~8x H100).
- **Inconvénients :** aucun benchmark officiel publié par Zhipu au lancement, comportement de reward-hacking documenté en entraînement (corrigé mais non audité indépendamment), pas de modalité vision au lancement, fenêtre de sortie maximale plus courte (131K tokens).

## Conformité RGPD et souveraineté des données en Europe

Pour les équipes européennes, la question de la licence et de l’hébergement n’est pas qu’un détail technique. Avec des poids MIT disponibles dès leur sortie, DeepSeek V4 Pro et GLM-5.2 permettent un déploiement complet sur une infrastructure cloud localisée dans l’Union européenne, ce qui simplifie la conformité RGPD par rapport à un appel API vers un serveur hébergé hors UE. Kimi K3, tant que son usage passait exclusivement par l’API de Moonshot, imposait un transfert de données vers un fournisseur hors UE, un point à examiner avec un délégué à la protection des données avant tout déploiement en production sur des données personnelles.

Ce paramètre rejoint les conclusions déjà documentées dans le comparatif Mistral AI vs ChatGPT, Claude et Gemini sur le terrain RGPD : la localisation des poids et de l’infrastructure de calcul pèse autant que la performance brute dans la décision d’adoption pour une entreprise soumise au droit européen. Un modèle moins puissant mais auto-hébergeable en Europe peut rester le choix le plus sûr pour des données sensibles (santé, finance, RH), même si son score de benchmark est inférieur à celui d’un concurrent accessible uniquement via API étrangère.

## Ce qui pourrait changer d’ici la fin de l’année 2026

Le rythme de sortie observé entre avril et juillet 2026, avec un nouveau modèle open-weight de rang frontier tous les six à dix semaines, suggère que ce classement ne restera pas figé longtemps. Kimi K3 a succédé à K2.6 avec un gain d’efficacité revendiqué de 2,5x, un rythme d’itération qui laisse présager une nouvelle génération avant la fin de l’année. GLM-5.2 a lui-même succédé à GLM-5.1 en l’espace de quelques mois, avec un gain net sur SWE-bench Pro. Pour une équipe technique, cela signifie qu’un choix de modèle fait en août 2026 doit être revu au moins une fois par trimestre plutôt que considéré comme définitif.

Deux points de vigilance à surveiller dans les prochains mois : la publication éventuelle d’un rapport technique complet par Zhipu pour GLM-5.2, qui permettrait un audit indépendant des affirmations sur le module anti-triche, et l’évolution du taux d’hallucination de la lignée DeepSeek V4, un axe sur lequel l’éditeur a intérêt à progresser s’il veut convaincre les équipes exigeant une haute fiabilité de réponse. Du côté des modèles fermés, la pression concurrentielle exercée par ce trio open-weight explique en grande partie la baisse de prix de 80 % annoncée par GPT-5.6 : les éditeurs propriétaires n’ont plus la latitude de facturer des primes aussi élevées face à des alternatives ouvertes qui talonnent leurs scores de référence.

## Verdict final : quel modèle choisir selon votre besoin

Aucun des trois modèles ne l’emporte sur tous les critères, et c’est précisément ce qui rend ce comparatif utile plutôt qu’un simple classement à sens unique. Pour le **coût le plus bas** à qualité de code compétitive, **DeepSeek V4 Pro** reste le choix par défaut, avec un coût par tâche de 0,04 $ et la première place mondiale sur LiveCodeBench — à condition de compenser son taux d’hallucination élevé par une couche de vérification. Pour la **capacité brute la plus élevée**, y compris sur des tâches multimodales et agentiques longues, **Kimi K3** domine, mais à un prix de sortie 17 fois supérieur à celui de V4 Pro et sans poids disponibles avant fin juillet 2026. Pour un **équilibre entre vitesse, coût et auto-hébergement immédiat**, **GLM-5.2** occupe une position médiane cohérente : plus rapide que les deux autres, moins cher que K3, et déployable sur infrastructure européenne dès aujourd’hui sous licence MIT.

En résumé chiffré : si le budget est la contrainte n°1, choisissez DeepSeek V4 Pro. Si la latence détermine votre expérience utilisateur, choisissez GLM-5.2. Si la capacité de raisonnement et le multimodal priment sur le coût, choisissez Kimi K3 — en acceptant de payer jusqu’à 17 fois plus cher au token de sortie pour cet avantage.

## Questions fréquentes

### Kimi K3, DeepSeek V4 et GLM-5.2 sont-ils vraiment open source ?

Les trois sont qualifiés d’« open-weight » : les poids du modèle entraîné sont publiés, mais pas nécessairement le code d’entraînement complet ni les données. DeepSeek V4 Pro et GLM-5.2 sont sous licence MIT complète avec poids disponibles sur Hugging Face. Kimi K3 utilise une licence Modified MIT, avec des poids publiés depuis le 27 juillet 2026 et une clause d’attribution qui ne s’applique qu’au-delà de 100 millions d’utilisateurs actifs mensuels.

### Quel est le modèle le moins cher entre Kimi K3, DeepSeek V4 et GLM-5.2 ?

DeepSeek V4 Pro est le moins cher, à 0,435 $ par million de tokens en entrée et 0,87 $ en sortie, soit un coût par tâche d’environ 0,04 $ selon Artificial Analysis. Sa variante V4 Flash descend encore plus bas, à 0,14 $/0,28 $ par million de tokens.

### Peut-on auto-héberger ces modèles sur son propre serveur ?

DeepSeek V4 Pro et GLM-5.2 le permettent dès aujourd’hui grâce à leurs poids MIT disponibles sur Hugging Face, mais cela demande un cluster GPU conséquent : environ 8x H100 pour GLM-5.2, davantage pour V4 Pro. Kimi K3 nécessite un supernode d’au moins 64 accélérateurs selon les recommandations de Moonshot, ce qui reste hors de portée pour la plupart des équipes.

### Quel modèle est le plus rapide en production ?

GLM-5.2 est le plus rapide, avec un débit mesuré d’environ 168 tokens par seconde par Artificial Analysis, contre environ 62 tokens par seconde pour Kimi K3 et DeepSeek V4 Pro. Cet écart compte particulièrement pour les applications temps réel comme le support client ou les assistants conversationnels à fort trafic.

### Ces modèles sont-ils compatibles avec le RGPD ?

La compatibilité RGPD dépend surtout du mode de déploiement plutôt que du modèle lui-même. Auto-héberger DeepSeek V4 Pro ou GLM-5.2 sur une infrastructure cloud localisée dans l’Union européenne permet de garder les données sur le territoire européen. Utiliser l’API de Kimi K3 implique un transfert de données vers l’infrastructure de Moonshot, hors UE, ce qui nécessite une analyse d’impact spécifique avant tout traitement de données personnelles.

### Kimi K3 gère-t-il les images et la vidéo ?

Oui, Kimi K3 est le seul des trois modèles à embarquer une modalité vision nativement dès son lancement. Ni DeepSeek V4 Pro ni GLM-5.2 ne proposaient de modalité vision au moment de leur sortie.

### Pourquoi DeepSeek V4 Pro a-t-il un taux d’hallucination aussi élevé ?

Sur le benchmark AA-Omniscience d’Artificial Analysis, qui mesure la capacité d’un modèle à reconnaître les limites de ses connaissances, DeepSeek V4 Pro affiche un taux de 94 %, c’est-à-dire qu’il tente de répondre presque systématiquement plutôt que d’indiquer une incertitude. Ce comportement reflète probablement un entraînement optimisé pour maximiser les scores sur des benchmarks de résolution de problèmes plutôt que pour la calibration de la confiance, un compromis courant chez les modèles orientés compétition de code.

### Quel modèle choisir pour un projet de codage agentique en 2026 ?

Pour des tâches agentiques longues nécessitant un raisonnement profond et éventuellement du multimodal, Kimi K3 affiche le meilleur score agentique (89,5 sur BenchAlign). Pour un agent de codage à gros volume avec un budget maîtrisé, DeepSeek V4 Pro offre le meilleur rapport score SWE-bench/prix. Pour un agent conversationnel où la latence prime, GLM-5.2 reste le choix le plus cohérent.