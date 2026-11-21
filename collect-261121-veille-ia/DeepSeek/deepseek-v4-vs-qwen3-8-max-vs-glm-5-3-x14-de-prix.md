---
id: collect-261121-veille-ia/deepseek/deepseek-v4-vs-qwen3-8-max-vs-glm-5-3-x14-de-prix
title: "DeepSeek V4 vs Qwen3.8 Max vs GLM-5.3 : x14 de Prix"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "modele", "prix", "glm-5", "trois", "flash", "qwen3", "parametres"]
source: https://tech-insider.org/fr/deepseek-v4-vs-qwen3-8-max-vs-glm-5-3-2026/
source_anchor: ""
source_lines: [1, 232]
section: "DeepSeek"
---
Trois lancements en deux semaines, trois architectures MoE, un seul objectif affiché : reprendre l’initiative face à OpenAI et Anthropic sur le terrain du prix et du code. Entre le 31 juillet et le 15 août 2026, DeepSeek, Alibaba et Zhipu AI (Z.ai) ont chacun publié leur nouveau modèle phare : DeepSeek V4 Flash 0731, Qwen3.8 Max et GLM-5.3. Les trois visent le même public, celui des développeurs et des entreprises qui veulent des modèles agentiques puissants sans payer le tarif Claude Opus 5 ou GPT-5.6. Mais derrière cette promesse commune se cachent des écarts considérables : de 284 milliards à 2,4 billions de paramètres, et un rapport de prix qui va jusqu’à 14 fois entre le moins cher et le plus cher sur les tokens de sortie. Ce comparatif détaille les architectures, les tarifs, les benchmarks disponibles et les cas d’usage réels pour aider les équipes françaises et européennes à choisir le bon modèle avant la rentrée.

## Trois lancements en deux semaines : ce qui s’est passé en août 2026

La séquence est serrée. DeepSeek a publié la version **0731** de son modèle Flash le 31 juillet, avec un focus explicite sur les gains agentiques plutôt que sur une nouvelle architecture. Deux jours plus tard, le 2 août, Alibaba a dévoilé Qwen3.8 Max, un modèle massif de 2,4 billions de paramètres présenté comme capable de mener des tâches complexes en autonomie sur plusieurs jours. Puis, le 14 août, Zhipu AI a sorti GLM-5.3 avec un positionnement marketing sans détour : « Built to Code. Ready for Cyber Defense. »

Les trois laboratoires partagent un point commun frappant : aucun n’a changé de base pré-entraînée. DeepSeek V4 Flash 0731 garde l’architecture MoE à 284 milliards de paramètres de la préversion d’avril, réentraînée uniquement sur des données orientées agents. GLM-5.3 reprend le même socle à 743 milliards de paramètres que GLM-5.2, amélioré « entièrement par du post-entraînement à grande échelle », selon les documents techniques de Z.ai. Seul Qwen3.8 Max change d’échelle avec un saut massif à 2,4 billions de paramètres, ce qui en fait de loin le plus gros des trois modèles de cette génération.

Ce virage vers le post-entraînement plutôt que le pré-entraînement coûteux illustre une tendance de fond en Chine en 2026 : les laboratoires optimisent l’existant pour gagner sur des benchmarks agentiques précis (code, terminal, sécurité) au lieu de relancer des cycles d’entraînement à plusieurs centaines de millions de dollars. Pour l’écosystème européen, qui suit de près les alternatives à Mistral Large 3, cette bataille de prix change directement le calcul de rentabilité d’un déploiement d’agents IA à grande échelle.

## DeepSeek V4 Flash 0731 : la référence low-cost à poids ouverts

DeepSeek V4 Flash 0731 reste fidèle à la philosophie qui a fait le succès de DeepSeek depuis 2025 : un modèle Mixture-of-Experts à **284 milliards de paramètres totaux** dont seulement **13 milliards sont actifs par token**. Cette sparsité extrême explique pourquoi le modèle tient un prix aussi bas tout en offrant une fenêtre de contexte d’un million de tokens et une sortie maximale de 384 000 tokens.

Le vrai changement de la version 0731 ne touche pas l’architecture mais l’entraînement. DeepSeek a reposté le modèle uniquement sur des données orientées agents, ce qui se traduit par un score de **82,7 % sur Terminal-Bench 2.1**, contre 61,8 % pour la préversion d’avril selon les données publiées par MarkTechPost. C’est un bond de plus de 20 points sans changer un seul paramètre du réseau.

Côté licence, DeepSeek V4 Flash 0731 est publié en **poids ouverts sous licence MIT**, disponible le jour même de l’annonce selon la fiche technique publiée sur Hugging Face. C’est un argument de poids pour les équipes qui veulent héberger le modèle en interne, un critère de plus en plus scruté en France depuis l’entrée en vigueur des obligations de transparence de l’article 50 de l’AI Act européen, effective depuis le 2 août 2026. Le modèle est aussi servi via l’API officielle DeepSeek, via OpenRouter et via Together AI, avec une limite de concurrence fixée à 2 500 requêtes simultanées sur l’API directe.

Le point fort commercial reste le cache : 0,14 $ par million de tokens d’entrée en cas de défaut de cache, mais seulement **0,0028 $** si le contenu est déjà en cache, soit une remise de 98 %. Pour des applications à contexte répétitif (agents qui relisent le même code, chatbots avec un prompt système long), cette architecture de tarification rend DeepSeek V4 Flash quasiment imbattable sur le coût réel par requête.

## Qwen3.8 Max : le mastodonte à 2,4 billions de paramètres

Alibaba change de catégorie avec Qwen3.8 Max. Le modèle affiche **2,4 billions de paramètres totaux**, avec 95 milliards de paramètres actifs par requête selon les données publiées au lancement le 2 août 2026. C’est près de neuf fois la taille totale de DeepSeek V4 Flash, ce qui en fait l’un des plus gros modèles disponibles publiquement en 2026, comme le souligne notre actualité dédiée au lancement de Qwen3.8-Max.

Sur le plan des capacités, Qwen3.8 Max se distingue par deux choses. D’abord, la **modalité native** : le modèle accepte du texte, de l’image et de la vidéo en entrée, ce qu’aucun des deux autres modèles de ce comparatif ne propose. Ensuite, un positionnement explicite sur les tâches longues : Alibaba met en avant sa capacité à « gérer des tâches complexes de façon autonome sur plusieurs jours », y compris la reproduction de résultats de recherche ou la conception de circuits, selon la couverture de lancement.

Sur les benchmarks, Qwen3.8 Max affiche des scores solides là où les données existent : **92,6** sur GPQA Diamond, **67,7** sur SWE-Bench Pro, **86,6** sur Terminal-Bench 2.1 et **86,1** sur OSWorld-Verified, chiffres vendeur-déclarés relayés par plusieurs agrégateurs de spécifications. Ce sont, à ce jour, les seuls scores GPQA Diamond et SWE-Bench disponibles pour les trois modèles de ce comparatif, ce qui fait de Qwen3.8 Max le mieux documenté sur le raisonnement scientifique.

Côté tarif, deux chiffres circulent et il faut les distinguer. Le tarif officiel Alibaba Cloud Model Studio annonce 12 yuans pour 1 million de tokens d’entrée et 36 yuans en sortie, soit environ **1,67 $ et 5,00 $** au taux de change courant, identique au tarif de Qwen3.7-Max. Mais la majorité des agrégateurs API et guides développeurs cités début août listent plutôt **2 $ en entrée et 6 $ en sortie** par million de tokens. C’est ce second tarif, plus élevé, qui sert de référence dans ce comparatif car c’est celui rencontré par la majorité des intégrateurs tiers.

Sur la licence, Qwen3.8 Max est disponible en API dès le lancement via Alibaba Cloud Model Studio et QwenCloud, mais les poids ouverts n’étaient pas encore publiés au moment de l’annonce. Alibaba a communiqué une mise à disposition « la semaine suivante » sur Hugging Face et ModelScope, ce qui en ferait le premier modèle de la gamme Qwen-Max à passer en poids ouverts. Le nom exact de la licence n’a pas été précisé dans les sources disponibles au moment de la rédaction.

## GLM-5.3 : le spécialiste code et cyberdéfense de Zhipu AI

Zhipu AI, qui opère sous la marque Z.ai, a choisi un axe différent avec GLM-5.3 : ne pas retoucher la base et concentrer tous les efforts de post-entraînement sur le code et les tâches longues. Le modèle garde une architecture MoE à environ **743 milliards de paramètres totaux** (une source la donne à 753 milliards) avec **40 milliards de paramètres actifs par token**, un chiffre identique à celui de GLM-5.2 sorti en juin.

La fenêtre de contexte atteint elle aussi 1 million de tokens, avec une sortie maximale de 128 000 tokens. Sur les benchmarks propres à Zhipu, GLM-5.3 obtient **28,3 sur Terminal-Bench 3.0** (une version plus récente et donc non directement comparable au Terminal-Bench 2.1 utilisé par DeepSeek et Qwen) et **66,9 sur DeepSWE 1.1**, un résultat que Zhipu présente comme le meilleur parmi les modèles open source sur cette suite, devant Kimi K3. En interne, Z.ai revendique un gain de **50 % sur son benchmark de code maison** par rapport à GLM-5.2, un chiffre à prendre avec la prudence habituelle réservée aux benchmarks propriétaires non audités.

Le vrai atout différenciant de GLM-5.3 est sa compatibilité multi-protocole. Le modèle expose à la fois un endpoint OpenAI Chat Completions, un endpoint OpenAI Responses et un **endpoint compatible Anthropic Messages** (accessible via `/api/anthropic`), ce qui permet de le brancher directement sur des outils construits pour Claude, comme Claude Code, sans réécrire la couche d’intégration. C’est une décision stratégique claire : capter les équipes déjà outillées autour de l’écosystème Anthropic sans qu’elles aient à migrer leur code.

Sur le prix, GLM-5.3 conserve la grille tarifaire de GLM-5.2 : **1,40 $** par million de tokens en entrée, **0,26 $** en entrée mise en cache, et **4,40 $** en sortie. Pour un usage typique en boucle d’agent où la majorité du contexte est relu depuis le cache, le coût d’entrée effectif tombe autour de 0,60 $ par million de tokens selon les estimations de plusieurs analystes indépendants. Au lancement, seule l’API hébergée est disponible : Z.ai prévoit de publier les poids ouverts sur Hugging Face autour du 28 août 2026, deux semaines après l’annonce initiale.

## Tableau comparatif : spécifications techniques des trois modèles

Voici l’ensemble des caractéristiques techniques vérifiées pour chaque modèle, à date du 22 août 2026.

| Caractéristique | DeepSeek V4 Flash 0731 | Qwen3.8 Max | GLM-5.3 | 
|---|---|---|---|
| Développeur | DeepSeek | Alibaba | Zhipu AI (Z.ai) | 
| Date de sortie | 31 juillet 2026 | 2 août 2026 | 14 août 2026 | 
| Architecture | MoE, 284 Md paramètres | MoE, 2,4 T paramètres | MoE, ~743 Md paramètres | 
| Paramètres actifs | ~13 Md / token | 95 Md / requête | ~40 Md / token | 
| Fenêtre de contexte | 1 000 000 tokens | 1 000 000 tokens | 1 000 000 tokens | 
| Sortie maximale | 384 000 tokens | 131 072 tokens | 128 000 tokens | 
| Modalités d’entrée | Texte (mode thinking/non-thinking) | Texte, image, vidéo | Texte (agentique, code) | 
| Licence à date | Open weight, MIT | API seule (poids annoncés) | API seule (poids annoncés ~28 août) | 
| Prix entrée (cache miss) | 0,14 $ / 1M tokens | 2,00 $ / 1M tokens | 1,40 $ / 1M tokens | 
| Prix entrée (cache hit) | 0,0028 $ / 1M tokens | Non communiqué | 0,26 $ / 1M tokens | 
| Prix sortie | 0,28 $ / 1M tokens | 6,00 $ / 1M tokens | 4,40 $ / 1M tokens | 
| Endpoints compatibles | API propre, style OpenAI | API OpenAI-compatible | OpenAI + Anthropic Messages | 
| Disponibilité | API DeepSeek, OpenRouter, Together AI | Alibaba Cloud Model Studio, QwenCloud | Z.ai API, GLM Coding Plan | 

Ce tableau met en évidence un point souvent négligé dans la couverture anglophone : les trois modèles convergent tous vers une fenêtre de contexte d’un million de tokens. En 2025, ce chiffre était encore réservé aux modèles les plus chers. En août 2026, il devient un standard même sur les offres les moins onéreuses du marché chinois.

## Benchmarks : que disent réellement les scores publiés

Comparer des benchmarks entre trois laboratoires qui ne publient pas les mêmes suites de tests est un exercice à manier avec prudence. Terminal-Bench 2.1 et Terminal-Bench 3.0, par exemple, ne sont pas la même échelle : un score de 28,3 sur la version 3.0 de GLM-5.3 n’est pas directement comparable à un score de 82,7 sur la version 2.1 de DeepSeek V4 Flash. Voici néanmoins l’ensemble des chiffres vérifiés, avec leur source, pour que chacun puisse se faire son idée.

| Benchmark | DeepSeek V4 Flash 0731 | Qwen3.8 Max | GLM-5.3 | 
|---|---|---|---|
| Terminal-Bench 2.1 | 82,7 % | 86,6 | Non testé sur cette version | 
| Terminal-Bench 3.0 | Non communiqué | Non communiqué | 28,3 | 
| SWE-Bench Pro | Non communiqué | 67,7 | Non communiqué | 
| GPQA Diamond | Non communiqué | 92,6 | Non communiqué | 
| OSWorld-Verified | Non communiqué | 86,1 | Non communiqué | 
| DeepSWE 1.1 | Non communiqué | Non communiqué | 66,9 | 
| Gain vs génération précédente | +20,9 pts sur Terminal-Bench 2.1 vs préversion d’avril | Nouvelle échelle (2,4T vs Qwen3.7 non chiffré publiquement) | +50 % sur benchmark code interne Z.ai vs GLM-5.2 | 

Trois sources indépendantes confirment ces chiffres : le rapport technique de MarkTechPost pour le score Terminal-Bench 2.1 de DeepSeek, l’agrégateur de spécifications qui recense les scores GPQA Diamond et SWE-Bench Pro de Qwen3.8 Max, et le tracker de sorties de modèles BenchLM qui documente le calendrier de publication des trois modèles. Le constat qui ressort : Qwen3.8 Max est de loin le mieux documenté sur les benchmarks de raisonnement scientifique et de résolution de bugs logiciels, tandis que DeepSeek et GLM se concentrent sur des métriques agentiques et de terminal qui valorisent leur spécialisation coût-efficacité et code respectivement.

Un classement français publié par Blog du Modérateur plaçait mi-août Qwen 3.8 Max en 8e position et Gemini 3.7 Flash High en 9e position de son top 20 des modèles les plus performants sur Text Arena, aux côtés de Claude Fable 5 en tête. Ce classement généraliste confirme que Qwen3.8 Max s’installe déjà dans le haut du panier, alors que GLM-5.3 et DeepSeek V4 Flash, plus récents ou plus spécialisés, n’y figuraient pas encore à cette date.

## Tarification détaillée : quel modèle coûte le moins cher en production

Le prix par million de tokens ne raconte qu’une partie de l’histoire. Ce qui compte en production, c’est le coût réel d’une charge de travail type. Prenons un scénario courant pour un agent de support ou de code : 10 millions de tokens d’entrée (essentiellement du contexte répété) et 2 millions de tokens de sortie sur un mois.

| Modèle | Entrée (cache miss) | Entrée (cache hit) | Sortie | Coût estimé 10M in + 2M out | 
|---|---|---|---|---|
| DeepSeek V4 Flash 0731 | 0,14 $ / 1M | 0,0028 $ / 1M | 0,28 $ / 1M | ~1,96 $ (hors cache) / ~0,59 $ (avec cache à 90 %) | 
| GLM-5.3 | 1,40 $ / 1M | 0,26 $ / 1M | 4,40 $ / 1M | ~22,80 $ (hors cache) / ~11,60 $ (avec cache à 90 %) | 
| Qwen3.8 Max | 2,00 $ / 1M | Non communiqué | 6,00 $ / 1M | ~32,00 $ (tarif marché, sans cache documenté) | 

L’écart entre le moins cher et le plus cher atteint **14 fois** sur le prix de sortie brut (0,28 $ contre 4,40 $ pour le cache miss d’entrée le plus onéreux face au moins cher), et grimpe encore plus haut si l’on compare le tarif de cache hit de DeepSeek face au tarif d’entrée standard de Qwen3.8 Max. Pour une startup française qui fait tourner des milliers de requêtes agentiques par jour, la différence se chiffre vite en dizaines de milliers d’euros sur l’année. C’est un argument qui pèse d’autant plus lourd dans un contexte où la réglementation AI Act impose déjà des coûts de conformité supplémentaires aux éditeurs européens.

## Licence et disponibilité : poids ouverts contre API fermée

C’est sur ce terrain que DeepSeek V4 Flash 0731 prend une avance nette. Le modèle est disponible en poids ouverts sous licence MIT dès le jour du lancement, ce qui signifie qu’une équipe peut le télécharger, l’auditer et le déployer sur son propre matériel sans dépendre d’une API tierce. C’est un argument décisif pour les organisations soumises à des exigences de souveraineté des données, un sujet particulièrement sensible en France depuis les débats autour du choix d’une IA souveraine face à OpenAI.

Qwen3.8 Max et GLM-5.3 suivent une stratégie différente : sortir l’API en premier, publier les poids quelques semaines plus tard. Alibaba a annoncé une disponibilité des poids « la semaine suivant » le lancement du 2 août, ce qui correspondrait à une mise en ligne sur Hugging Face et ModelScope autour de la mi-août. Zhipu AI a été plus précis, en ciblant explicitement le **28 août 2026** pour la publication des poids de GLM-5.3 sur son organisation Hugging Face. Au moment de la rédaction de cet article, le 22 août, seul DeepSeek V4 Flash a effectivement livré ses poids : Qwen3.8 Max et GLM-5.3 restent, pour l’instant, des modèles à accès API uniquement.

Cette différence de calendrier a une conséquence pratique immédiate. Toute équipe qui veut tester ces trois modèles aujourd’hui doit passer par l’API hébergée pour Qwen3.8 Max et GLM-5.3, avec les implications habituelles en matière de latence réseau, de dépendance à un fournisseur unique et de transfert de données hors Union européenne à considérer avant tout déploiement en production sur des données sensibles.

## Auto-hébergement de DeepSeek V4 Flash : ce qu’il faut savoir avant de se lancer

Disposer de poids ouverts ne signifie pas que n’importe quelle équipe peut faire tourner DeepSeek V4 Flash 0731 sur un serveur de bureau. Avec 284 milliards de paramètres au format natif, le modèle complet exige plusieurs centaines de gigaoctets de mémoire GPU rien que pour charger les poids, avant même de compter le contexte d’un million de tokens qui consomme lui aussi une quantité significative de VRAM à l’inférence. En pratique, un déploiement à pleine précision réclame un cluster de plusieurs GPU haut de gamme de type H100 ou équivalent, ce qui écarte d’emblée les petites structures d’un hébergement on-premise sans passer par de la quantification.

La bonne nouvelle, c’est que la sparsité du modèle joue en sa faveur : seuls 13 milliards de paramètres sont activés par token, ce qui réduit le besoin en calcul par rapport à un modèle dense de taille équivalente, même si la mémoire nécessaire pour stocker l’intégralité des experts reste, elle, incompressible. Les équipes qui veulent tester le modèle sans investir dans du matériel dédié peuvent passer par des versions quantifiées en 4 ou 8 bits, disponibles via la communauté open source peu après la publication officielle des poids, au prix d’une perte de qualité généralement limitée sur les tâches de code et de résumé.

Pour la grande majorité des équipes françaises et européennes, la voie la plus réaliste reste donc un hébergement géré chez un fournisseur cloud disposant de capacité GPU en Europe, plutôt qu’un déploiement entièrement on-premise. Cela permet de garder un contrôle sur la localisation des données tout en évitant l’investissement initial en infrastructure, un compromis que plusieurs fournisseurs de cloud souverain européens commencent à proposer spécifiquement pour les modèles open weight chinois comme DeepSeek. Le calcul à faire avant de choisir : comparer le coût mensuel d’un cluster GPU loué face à la facture API DeepSeek officielle, qui reste très compétitive même sans les remises de cache, surtout pour des volumes en dessous de plusieurs centaines de millions de tokens par mois.

## Cinq scénarios concrets pour choisir entre les trois modèles

Au-delà des tableaux de spécifications, voici cinq situations typiques rencontrées par des équipes techniques en France et en Europe, et le modèle qui y répond le mieux selon les données disponibles.

- **Un éditeur SaaS qui automatise son support client à fort volume** : avec des dizaines de milliers de tickets par mois et un prompt système répétitif, la tarification en cache de DeepSeek V4 Flash (0,0028 $ par million de tokens en cache hit) réduit la facture de plus de 90 % par rapport à un tarif sans cache, un levier que ni Qwen3.8 Max ni GLM-5.3 n’égalent sur le papier.
- **Une agence qui traite des vidéos de démonstration produit** : seul Qwen3.8 Max accepte nativement la vidéo en entrée parmi les trois modèles, ce qui en fait le choix par défaut pour l’analyse automatisée de contenus multimédias sans passer par un pipeline de transcription séparé.
- **Une équipe sécurité qui construit un pipeline de revue de code et de détection de vulnérabilités** : le positionnement explicite « Ready for Cyber Defense » de GLM-5.3, combiné à son gain de 50 % sur le benchmark de code interne de Z.ai, en fait un candidat naturel pour ce cas d’usage, d’autant que son endpoint compatible Anthropic Messages permet de le brancher sur des outils déjà construits pour Claude Code.
- **Un laboratoire de recherche qui analyse des corpus documentaires complets** : les trois modèles offrent un contexte d’un million de tokens, mais DeepSeek V4 Flash pousse la sortie maximale à 384 000 tokens, contre 128 000 pour GLM-5.3 et environ 131 000 pour Qwen3.8 Max, un avantage net pour générer de longs rapports de synthèse en une seule passe.
- **Une startup qui veut héberger son modèle en interne dès aujourd’hui pour des raisons de conformité RGPD** : DeepSeek V4 Flash est, à ce jour, le seul des trois modèles réellement disponible en poids ouverts, ce qui le rend immédiatement déployable sur une infrastructure européenne sans attendre une publication future.
- **Une équipe produit qui migre progressivement depuis un modèle propriétaire type GPT ou Claude** : la double compatibilité OpenAI et Anthropic de GLM-5.3 minimise le travail de réécriture du code d’intégration, un atout que ni DeepSeek ni Qwen3.8 Max ne proposent sous cette forme précise.

## Guide de migration : passer d’un modèle à l’autre sans tout réécrire

Les trois fournisseurs exposent des API compatibles avec le format OpenAI Chat Completions, ce qui limite en théorie le travail de migration à un changement d’URL de base et de clé d’authentification. Voici la démarche recommandée pour tester les trois modèles sur un même code applicatif.

**Étape 1** : conserver le SDK client OpenAI existant dans votre application. Les trois modèles acceptent ce format, ce qui évite de réécrire la couche d’appel API.

**Étape 2** : changer uniquement le paramètre `base_url` et la clé d’API selon le fournisseur ciblé, comme illustré ci-dessous.

```
# DeepSeek V4 Flash 0731 (via API officielle)
client = OpenAI(
    base_url="https://api.deepseek.com/v1",
    api_key="VOTRE_CLE_DEEPSEEK"
)
response = client.chat.completions.create(
    model="deepseek-v4-flash",
    messages=[{"role": "user", "content": "Résume ce document de 50 pages"}]
)
# Qwen3.8 Max (via Alibaba Cloud Model Studio)
client = OpenAI(
    base_url="https://dashscope-intl.aliyuncs.com/compatible-mode/v1",
    api_key="VOTRE_CLE_ALIBABA_CLOUD"
)
response = client.chat.completions.create(
    model="qwen3.8-max",
    messages=[{"role": "user", "content": "Analyse cette vidéo de démonstration"}]
)
# GLM-5.3 (via Z.ai, endpoint compatible Anthropic disponible aussi)
client = OpenAI(
    base_url="https://api.z.ai/api/coding/paas/v4",
    api_key="VOTRE_CLE_ZAI"
)
response = client.chat.completions.create(
    model="glm-5.3",
    messages=[{"role": "user", "content": "Revois ce diff de code et signale les failles"}]
)
```
**Étape 3** : adapter la gestion du cache. DeepSeek et GLM facturent différemment les tokens mis en cache : il faut structurer le prompt système en tête de requête et le contenu variable en fin de requête pour maximiser les hits de cache et profiter des tarifs réduits de 0,0028 $ (DeepSeek) ou 0,26 $ (GLM) par million de tokens.

**Étape 4** : pour les équipes déjà équipées d’outils construits sur l’écosystème Claude, tester en priorité l’endpoint Anthropic Messages de GLM-5.3 (`/api/anthropic`), qui permet une bascule sans toucher au code d’intégration Claude Code ou aux agents existants.

**Étape 5** : exécuter un test A/B sur un échantillon représentatif de requêtes de production avant tout basculement complet, en surveillant à la fois la qualité des réponses et le coût réel facturé, qui peut différer sensiblement de l’estimation théorique selon le taux de cache atteint.

## Avantages et inconvénients de chaque modèle

### DeepSeek V4 Flash 0731

- **Avantages** : prix le plus bas du marché, poids ouverts MIT disponibles immédiatement, sortie maximale la plus élevée (384K tokens), tarif de cache extrêmement agressif.
- **Inconvénients** : pas de modalité image ou vidéo, benchmarks de raisonnement scientifique (GPQA, SWE-Bench) non publiés, architecture inchangée depuis la préversion d’avril.

### Qwen3.8 Max

- **Avantages** : le plus documenté sur les benchmarks de raisonnement (GPQA Diamond 92,6, SWE-Bench Pro 67,7), seul modèle multimodal du trio, taille massive à 2,4 billions de paramètres pour les tâches longues.
- **Inconvénients** : le plus cher des trois sur l’entrée comme sur la sortie, poids ouverts non disponibles au 22 août 2026, absence sur OpenRouter à la date de lancement.

### GLM-5.3

- **Avantages** : double compatibilité OpenAI et Anthropic pour une migration facilitée, positionnement clair sur le code et la cyberdéfense, gain revendiqué de 50 % sur le benchmark interne de code face à GLM-5.2.
- **Inconvénients** : prix intermédiaire ni le moins cher ni le mieux benchmarké, poids ouverts annoncés seulement pour fin août, gain de 50 % basé sur un benchmark propriétaire non audité par un tiers.

## Ce que ce trio dit de la course à l’IA open source en 2026

La cadence de sortie observée cet été confirme un rythme désormais habituel dans l’écosystème chinois de l’IA générative. BenchLM a recensé **128 sorties de modèles** sur les douze mois précédant le 18 août 2026, soit une sortie notable environ tous les trois jours. Dans ce contexte, la stratégie de post-entraînement plutôt que de ré-entraînement complet, adoptée à la fois par DeepSeek et par Zhipu AI, ressemble à une réponse rationnelle à la pression concurrentielle : il coûte beaucoup moins cher de repolir un modèle existant sur des données agentiques ciblées que de relancer un cycle de pré-entraînement de plusieurs mois.

Pour les acteurs européens, cette accélération pose une question de fond. Le classement des modèles européens de BenchLM plaçait, au 21 août 2026, Ministral 3 14B en tête des modèles conçus en Europe, loin derrière les scores affichés par les modèles chinois et américains sur les mêmes benchmarks. Mistral AI reste le laboratoire européen le plus actif, avec Mistral Large 3 proposé à 2 $ / 6 $ par million de tokens en entrée et sortie sous licence Apache 2.0, un tarif proche de celui de Qwen3.8 Max mais avec l’avantage d’un hébergement possible sur le sol européen. La question du choix ne se limite donc pas au prix ou au benchmark brut : la localisation des données et la conformité à l’AI Act pèsent de plus en plus dans la décision, en particulier depuis que l’article 50 sur la transparence des systèmes d’IA est entré en application le 2 août 2026 pour tous les fournisseurs actifs sur le marché européen.

## Verdict : quel modèle choisir selon votre profil

Sur la base des données de prix, de licence et de benchmarks disponibles au 22 août 2026, aucun des trois modèles ne domine sur tous les critères, mais chacun a un profil clair.

**DeepSeek V4 Flash 0731 l’emporte sur le rapport coût-efficacité et la souveraineté immédiate.** Avec un prix jusqu’à 14 fois inférieur à GLM-5.3 sur la sortie et des poids ouverts déjà disponibles, c’est le choix par défaut pour toute équipe qui veut démarrer aujourd’hui sans attendre une publication de poids future, et qui n’a pas besoin de modalités image ou vidéo.

**Qwen3.8 Max l’emporte sur la puissance brute et la polyvalence multimodale.** C’est le seul des trois à traiter nativement la vidéo, et le seul dont les scores de raisonnement scientifique (GPQA Diamond, SWE-Bench Pro) sont publiquement documentés à un niveau élevé. Le compromis : c’est aussi le plus cher, et ses poids restent, à cette date, indisponibles au téléchargement.

**GLM-5.3 l’emporte sur la facilité d’intégration pour les équipes déjà sur l’écosystème Claude ou GPT.** Sa double compatibilité API et son positionnement code et cyberdéfense en font un candidat solide pour les pipelines de développement, à condition d’accepter un tarif ni le plus bas ni le mieux benchmarké publiquement.

Pour une équipe qui doit trancher aujourd’hui sans faire de test A/B complet, la recommandation la plus défendable reste de démarrer avec DeepSeek V4 Flash 0731 pour sa disponibilité immédiate en poids ouverts et son prix imbattable, puis de réévaluer Qwen3.8 Max et GLM-5.3 fin août quand leurs poids ouverts respectifs seront enfin publiés et que des benchmarks tiers indépendants (au-delà des chiffres vendeur) auront eu le temps de circuler.

## Foire aux questions

### Quel est le modèle le moins cher entre DeepSeek V4 Flash, Qwen3.8 Max et GLM-5.3 ?

DeepSeek V4 Flash 0731 est le moins cher des trois, à 0,14 $ par million de tokens d’entrée (0,0028 $ en cache hit) et 0,28 $ en sortie. GLM-5.3 se situe au milieu à 1,40 $ / 4,40 $, et Qwen3.8 Max est le plus cher à 2 $ / 6 $ par million de tokens au tarif marché courant.

### Les poids de Qwen3.8 Max et GLM-5.3 sont-ils déjà disponibles au téléchargement ?

Non, au 22 août 2026, seul DeepSeek V4 Flash 0731 est disponible en poids ouverts. Alibaba a annoncé une publication des poids de Qwen3.8 Max la semaine suivant son lancement du 2 août, et Zhipu AI vise le 28 août 2026 pour GLM-5.3. Ces deux modèles ne sont accessibles pour l’instant que via leurs API hébergées respectives.

### Quel modèle a la meilleure fenêtre de contexte ?

Les trois modèles offrent une fenêtre de contexte d’un million de tokens, ce qui en fait un standard partagé cet été 2026. La différence se joue sur la sortie maximale : DeepSeek V4 Flash autorise jusqu’à 384 000 tokens de sortie, contre environ 131 000 pour Qwen3.8 Max et 128 000 pour GLM-5.3.

### GLM-5.3 est-il vraiment plus performant en code que GLM-5.2 ?

Zhipu AI revendique un gain de 50 % sur son benchmark de code interne par rapport à GLM-5.2. Ce chiffre provient d’un test propriétaire non audité par un organisme tiers, il faut donc le considérer comme une indication de tendance plutôt qu’une mesure indépendante vérifiée.

### Peut-on utiliser GLM-5.3 avec un SDK conçu pour Claude ?

Oui. GLM-5.3 expose un endpoint compatible avec le format Anthropic Messages (`/api/anthropic`), ce qui permet de le brancher sur des outils comme Claude Code sans réécrire la couche d’intégration, en changeant simplement l’URL de base et la clé d’API.

### DeepSeek V4 Flash convient-il à un déploiement souverain en Europe ?

C’est le candidat le plus pratique des trois pour un hébergement sur infrastructure européenne, puisque ses poids sont déjà publiés sous licence MIT et peuvent être déployés sur un cloud privé ou souverain sans dépendre de l’API du fournisseur d’origine, sous réserve de disposer du matériel nécessaire pour faire tourner un modèle MoE à 284 milliards de paramètres.

### Qwen3.8 Max est-il capable de traiter des vidéos ?

Oui, Qwen3.8 Max accepte nativement le texte, l’image et la vidéo en entrée. C’est la seule capacité multimodale native parmi les trois modèles de ce comparatif, DeepSeek V4 Flash et GLM-5.3 se limitant au texte.

### Quel modèle choisir pour une startup française avec un budget limité ?

DeepSeek V4 Flash 0731 reste le choix le plus rationnel pour un budget serré, grâce à son prix d’entrée jusqu’à 14 fois inférieur à celui de GLM-5.3 sur la sortie et à son tarif de cache à 0,0028 $ par million de tokens qui réduit drastiquement la facture sur les charges de travail à contexte répétitif comme les chatbots de support.
