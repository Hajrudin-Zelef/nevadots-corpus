---
id: collect-261121-veille-ia/qwen/qwen3-8-flash-next-alibaba-previsualise-qwen4-2026
title: "Qwen3.8-Flash-Next : Alibaba Prévisualise Qwen4 [2026]"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: ["2026-08-26"]
keywords: ["qwen3", "milliards", "flash-next", "modele", "parametres", "tokens", "qwen4", "modeles"]
source: https://tech-insider.org/fr/qwen3-8-flash-next-alibaba-qwen4-2026/
source_anchor: ""
source_lines: [1, 141]
section: "Qwen"
---
Le 26 août 2026, l’équipe Qwen d’Alibaba a mis en ligne les poids de **Qwen3.8-Flash-Next** sur Hugging Face et ModelScope, sans conférence de presse ni grand lancement marketing. Trois semaines plus tard, ce modèle s’impose comme l’un des sujets IA les plus commentés de la rentrée technique 2026 : il ne s’agit pas d’un simple modèle “Flash” économique de plus, mais d’un aperçu assumé de l’architecture qui équipera **Qwen4**, la prochaine génération de la famille de modèles chinoise. Avec 180 milliards de paramètres au total dont seulement 6 milliards actifs par requête, une fenêtre de contexte native de 262 144 tokens et une licence commerciale conditionnelle, Qwen3.8-Flash-Next relance en France et en Europe le débat sur la place des modèles ouverts face à GPT-6 Astra, Claude Fable 5.1 et Gemini 3.8 Flash.

## Qu’est-ce que Qwen3.8-Flash-Next exactement ?

Qwen3.8-Flash-Next est un modèle de langage multimodal à mélange d’experts (Mixture-of-Experts, ou MoE), doté d’un encodeur visuel qui lui permet de traiter du texte, des images et de la vidéo en entrée, pour une sortie textuelle. Le dépôt officiel sur GitHub indique une date de publication au 26 août 2026, confirmée par plusieurs suivis de sorties de modèles indépendants. Le modèle est décrit par Alibaba comme le jumeau open-weight de l’API hébergée Qwen3.8-Flash, disponible sur QwenCloud, mais avec une différence de taille : n’importe quel développeur peut télécharger les poids depuis Hugging Face ou ModelScope et les faire tourner sur sa propre infrastructure, y compris en Europe.

Ce qui distingue vraiment Qwen3.8-Flash-Next des publications précédentes de la marque, c’est sa fonction annoncée : il s’agit explicitement d’un **galop d’essai pour Qwen4**, l’architecture de nouvelle génération qu’Alibaba prépare pour la suite de sa gamme. Plusieurs analyses spécialisées, dont celle de Datacamp et du suivi de sorties AI Release Tracker, présentent le modèle comme la première démonstration publique du design MoE multimodal qui structurera Qwen4 dans ses différentes tailles.

## Une architecture MoE à 180 milliards de paramètres, 6 milliards actifs

Sur le plan technique, Qwen3.8-Flash-Next combine trois blocs : un modèle principal de 125 milliards de paramètres, une table d’embeddings n-gramme de 51 milliards de paramètres, et un module de prédiction multi-tokens (MTP) de 4 milliards de paramètres, pour un total d’environ 180 milliards de paramètres sur disque. Le point clé, et la raison pour laquelle ce modèle intéresse autant les ingénieurs, c’est que seulement **6 milliards de paramètres sont activés par token** traité, grâce à un routage vers 512 experts au total, dont 10 experts spécialisés plus 1 expert partagé sollicités à chaque étape. Le réseau utilise une dimension cachée de 2 560 et compte 48 couches de transformeur, selon les spécifications relevées sur le modèle publié sur Hugging Face.

Cette conception permet à Alibaba de revendiquer un entraînement environ neuf fois moins coûteux que celui de Qwen3.7-Plus, tout en visant des performances de code et de raisonnement comparables à des modèles denses beaucoup plus lourds. C’est cette efficacité, plus que la taille brute, qui explique pourquoi plusieurs commentateurs qualifient Qwen3.8-Flash-Next de “modèle à 6 milliards de paramètres actifs qui rivalise avec des modèles à près de 400 milliards”.

## Contexte natif de 262 144 tokens, extensible à 1 million

Le modèle open-weight est entraîné nativement sur une fenêtre de contexte de 262 144 tokens, soit environ 262 000 mots-équivalents. Ce chiffre correspond à la configuration de base des poids téléchargeables. L’API hébergée sur QwenCloud, elle, étend cette capacité jusqu’à 1 million de tokens via une technique de mise à l’échelle de type YaRN, avec une limite pratique d’environ 991 000 tokens en entrée et 131 072 tokens en sortie. Il faut noter une nuance importante souvent gommée dans les comparatifs rapides : l’extension à 1 million de tokens est une capacité de service, pas une caractéristique native des poids ouverts, qui restent calibrés à 262 144 tokens tant qu’ils ne sont pas explicitement étendus par l’utilisateur.

## La licence Qwen Community License 1.0 : ni Apache 2.0, ni gratuit pour tous

Contrairement à une partie de la gamme Qwen publiée sous licence Apache 2.0, Qwen3.8-Flash-Next est distribué sous la **Qwen Community License 1.0**, une licence plus restrictive. Le texte de la licence autorise l’usage commercial et le fine-tuning dans la plupart des cas, mais impose deux garde-fous concrets : une obligation d’attribution pour toute entreprise dépassant 100 millions d’utilisateurs mensuels actifs ou 20 millions de dollars de revenus mensuels, et une licence séparée obligatoire pour quiconque propose un service de type “modèle en tant que service” ou un assistant de codage IA commercial basé sur Qwen3.8-Flash-Next. Pour une PME française qui veut simplement l’utiliser en interne, ces seuils ne changent rien au quotidien. Pour un éditeur de logiciels qui viserait à revendre l’accès au modèle, la situation est nettement plus contraignante qu’avec du Apache 2.0 pur.

## Un prix agressif : 0,16 $ en entrée, 0,47 $ en sortie par million de tokens

Sur QwenCloud, l’API hébergée sous le nom “qwen3.8-flash” est facturée 0,16 dollar par million de tokens en entrée et 0,47 dollar par million de tokens en sortie, avec un tarif réduit à environ 0,016 dollar par million de tokens pour les lectures mises en cache. Le site de comparaison de modèles Artificial Analysis qualifie ce positionnement de “prix modéré”, en le situant sous la médiane du marché qu’il évalue à 0,30 dollar en entrée et 1,15 dollar en sortie pour l’ensemble des modèles suivis. Pour les entreprises qui préfèrent l’auto-hébergement plutôt que l’API propriétaire d’Alibaba, l’argument économique change de nature : il n’y a plus de coût par token, mais un coût d’infrastructure GPU, largement compensé par la faible proportion de paramètres actifs par requête.

## Les benchmarks disponibles : DeepSWE, SWE-bench Pro et les inconnues

Sur le terrain des chiffres, il faut être précis pour ne pas céder à la surenchère marketing qui entoure souvent ces annonces. Alibaba revendique, en auto-évaluation, un score de 58,7 sur le benchmark DeepSWE et de 62,5 sur SWE-bench Pro, deux tests centrés sur les capacités d’ingénierie logicielle. Ce sont des scores solides pour un modèle à seulement 6 milliards de paramètres actifs, mais à ce stade, **aucune source consultée ne publie de tableau chiffré comparant directement Qwen3.8-Flash-Next à GPT-6 Astra, Claude Fable 5.1, Gemini 3.8 Flash ou DeepSeek V4** sur des benchmarks communs et vérifiés. Les analyses disponibles évoquent une performance “compétitive avec des modèles denses bien plus lourds” en code et en raisonnement, une formulation qualitative qu’il convient de traiter comme telle plutôt que comme un classement définitif.

Sur le podium des modèles généralistes de septembre 2026, c’est toujours le trio GPT-6 Astra, Claude Opus 5 et Gemini 3.8 Flash qui domine les classements toutes catégories, avec des tarifs d’API très supérieurs à ceux de Qwen3.8-Flash-Next. La comparaison pertinente pour ce dernier se situe plutôt face à d’autres modèles ouverts orientés production à bas coût, comme GLM-5.3-Flash ou les déclinaisons Flash de DeepSeek V4.

## Qwen3.8-Flash-Next face à Qwen3.8-Max : deux stratégies opposées

Publié quelques semaines plus tôt, Qwen3.8-Max visait la qualité maximale avec un modèle nettement plus dense, revendiqué à 2,4 billions de paramètres en configuration ouverte. Qwen3.8-Flash-Next inverse la logique : moins de paramètres activés, un contexte plus long, un coût par token beaucoup plus faible, et un objectif assumé de très haut débit pour des cas d’usage de production, d’agents autonomes et de traitement de documents volumineux. Les deux modèles ne visent donc pas le même public : Qwen3.8-Max pour les tâches où la qualité prime sur tout, Qwen3.8-Flash-Next pour les déploiements à grande échelle où chaque token coûte cher en cumulé. Aucune source ne fournit à ce jour un tableau détaillé de benchmarks tête-à-tête entre les deux modèles, ce qui limite la précision d’une comparaison strictement chiffrée.

## De Qwen à Qwen4 : l’histoire d’une montée en puissance éclair

La trajectoire de la gamme Qwen illustre la vitesse de la course aux modèles ouverts depuis 2023. Alibaba a lancé sa première génération Qwen à l’été 2023, suivie de Qwen2 en 2024, puis de la famille Qwen3 qui a multiplié les variantes (Qwen3, Qwen3.5, Qwen3.7-Plus) tout au long de 2025 et du premier semestre 2026. La bascule vers Qwen3.8 à l’été 2026, avec Qwen3.8-Max en modèle dense haut de gamme puis Qwen3.8-Flash-Next fin août, marque une étape de transition explicitement présentée comme préparatoire à Qwen4. Ce rythme de publication, plusieurs modèles majeurs en quelques semaines, est devenu la norme chez les grands laboratoires chinois en 2026, à l’image de la cadence observée chez DeepSeek ou chez Zhipu avec sa gamme GLM.

## Vers Qwen4 : ce que révèle cette prévisualisation d’architecture

Les sources techniques sont unanimes sur un point : Qwen4 est une architecture annoncée mais pas encore un modèle nommé et daté. Aucune source consultée ne donne de date de sortie précise pour Qwen4, qu’il s’agisse du quatrième trimestre 2026 ou d’une échéance en 2027. Ce qui se dégage en revanche, par déduction à partir du design de Qwen3.8-Flash-Next, c’est la direction que prendra probablement la future gamme : généralisation du design MoE multimodal à plusieurs tailles de modèles, maintien d’un ratio paramètres actifs/paramètres totaux très bas pour réduire les coûts d’inférence, contexte natif dépassant probablement les 262 000 tokens, et poursuite d’une stratégie de publication ouverte au moins pour certains segments de la gamme. Il s’agit là d’une lecture des signaux disponibles, et non d’une confirmation officielle d’Alibaba sur le contenu exact de Qwen4.

## La bataille des poids ouverts : Alibaba face à DeepSeek, GLM, Mistral et Meta

Qwen3.8-Flash-Next arrive dans un paysage de modèles ouverts particulièrement dense en cette rentrée 2026. Le comparatif DeepSeek V4 vs Qwen3.8 Max vs GLM-5.3, publié quelques semaines plus tôt, montrait déjà un écart de prix pouvant atteindre un facteur 14 entre les modèles ouverts chinois les moins chers et les plus chers. La sortie de Qwen3.8-Flash-Next vient ajouter une option supplémentaire sur le segment “Flash”, où se disputent déjà GLM-5.3-Flash, les variantes Flash de DeepSeek V4 et de Gemini. Le comparatif Grok 4.6 vs Qwen3.8-Max vs Mistral Large 3 avait déjà pointé un écart de prix allant jusqu’à un facteur 4 entre les offres propriétaires américaines et les modèles ouverts chinois, un différentiel qui reste d’actualité avec Qwen3.8-Flash-Next.

Pour Alibaba, l’enjeu dépasse la seule performance technique : chaque nouveau modèle ouvert renforce l’écosystème Qwen sur Hugging Face et ModelScope, où le dépôt de Qwen3.8-Flash-Next pèse environ 360 gigaoctets. Aucune statistique de téléchargement ou d’adoption précise n’est disponible publiquement à ce stade pour ce modèle spécifique, ce qui empêche de quantifier son adoption réelle face à des concurrents comme DeepSeek V4 ou Llama.

## Quel impact pour l’Europe et la souveraineté numérique ?

La disponibilité de Qwen3.8-Flash-Next sur Hugging Face et ModelScope, deux plateformes largement utilisées en Europe, en fait un candidat naturel pour les organisations qui cherchent à auto-héberger un modèle frontière sans dépendre d’une API américaine. La Qwen Community License 1.0 n’impose aucune restriction géographique explicite, ce qui laisse la porte ouverte à un déploiement en France sous réserve de respecter les conditions d’attribution et de revente évoquées plus haut. Ce débat s’inscrit dans un contexte plus large où l’Europe cherche ses propres alternatives, qu’il s’agisse du LLM souverain EUROPA porté par la Commission européenne ou de Quasar 438B, le modèle espagnol de Multiverse Computing lancé le 2 septembre 2026, soit quelques jours seulement après Qwen3.8-Flash-Next. Aucune source ne mentionne à ce jour un hébergement français spécifique ou une certification de conformité à l’AI Act européen pour le modèle d’Alibaba, ce qui reste un point de vigilance pour toute administration ou entreprise française qui envisagerait de le déployer en production.

## Tableau comparatif : Qwen3.8-Flash-Next face aux modèles fermés et ouverts

| Modèle | Éditeur | Paramètres actifs | Contexte natif | Licence | Prix entrée/sortie (par M tokens) | 
|---|---|---|---|---|---|
| Qwen3.8-Flash-Next | Alibaba | 6 Md (sur 180 Md) | 262 144 tokens | Qwen Community License 1.0 | 0,16 $ / 0,47 $ | 
| Qwen3.8-Max | Alibaba | Dense, non communiqué | Non communiqué | Ouverte (conditions variables) | Non communiqué | 
| GPT-6 Astra | OpenAI | Fermé, non communiqué | Non communiqué publiquement | Propriétaire | Accès via abonnement Pro/Business/Enterprise | 
| Claude Fable 5.1 | Anthropic | Fermé, non communiqué | Non communiqué publiquement | Propriétaire | 10 $ / 50 $ (cache à 0,25 $) | 
| Claude Opus 5 | Anthropic | Fermé, non communiqué | Non communiqué publiquement | Propriétaire | 5 $ / 25 $ | 
| Gemini 3.8 Flash |  | Fermé, non communiqué | Non communiqué publiquement | Propriétaire | Non communiqué | 

Les cases “non communiqué” ne sont pas des oublis éditoriaux : elles reflètent le fait qu’aucune des sources consultées ne publie ces chiffres pour les modèles fermés cités, qui restent par nature moins transparents sur leur architecture interne que les modèles à poids ouverts comme la gamme Qwen.

## Tableau : la trajectoire Qwen, de la genèse à l’aperçu de Qwen4

| Génération | Période de sortie | Positionnement | 
|---|---|---|
| Qwen (v1) | Été 2023 | Premiers modèles ouverts d’Alibaba | 
| Qwen2 | 2024 | Extension de la gamme, meilleure couverture multilingue | 
| Qwen3 / Qwen3.5 | 2025 | Généralisation du raisonnement et des agents | 
| Qwen3.7-Plus | Début 2026 | Référence de coût avant l’arrivée de Qwen3.8 | 
| Qwen3.8-Max | Été 2026 | Modèle dense haut de gamme, 2,4 billions de paramètres revendiqués | 
| Qwen3.8-Flash-Next | 26 août 2026 | Aperçu open-weight de l’architecture Qwen4, MoE à 6 Md actifs | 
| Qwen4 | Non annoncé | Architecture prévisualisée, aucun modèle nommé ni daté à ce jour | 

## Comment interroger le modèle : un exemple d’appel API

Pour les développeurs qui souhaitent tester rapidement le modèle hébergé sans passer par l’auto-hébergement des poids ouverts, Alibaba expose Qwen3.8-Flash-Next via une API compatible avec le format OpenAI sur QwenCloud. Voici un exemple minimal d’appel en Python :

```
from openai import OpenAI
client = OpenAI(
    api_key="VOTRE_CLE_QWENCLOUD",
    base_url="https://dashscope-intl.aliyuncs.com/compatible-mode/v1"
)
response = client.chat.completions.create(
    model="qwen3.8-flash",
    messages=[
        {"role": "user", "content": "Résume les changements d'architecture entre Qwen3.8-Max et Qwen3.8-Flash-Next."}
    ],
    max_tokens=500
)
print(response.choices[0].message.content)
```
Pour l’auto-hébergement des poids ouverts, le modèle est également listé sur des passerelles multi-fournisseurs comme OpenRouter, ce qui facilite les tests comparatifs sans engagement direct avec l’infrastructure d’Alibaba.

## Cinq prédictions pour la suite de la bataille des LLM ouverts

- **Qwen4 restera fragmenté en plusieurs tailles.** Sur la base de la stratégie déjà observée avec Qwen3.x, il est probable qu’Alibaba décline Qwen4 en plusieurs variantes (Max, Flash, voire Nano), plutôt que de publier un modèle unique.
- **La pression sur les prix des modèles Flash va s’intensifier.** Avec GLM-5.3-Flash, DeepSeek V4-Flash et désormais Qwen3.8-Flash-Next sur un segment de prix proche, une nouvelle baisse tarifaire chez l’un des trois acteurs chinois est plausible d’ici la fin de l’année 2026.
- **Les entreprises européennes vont scruter la Qwen Community License 1.0 de près.** Les seuils d’attribution à 100 millions d’utilisateurs ou 20 millions de dollars de revenus mensuels pourraient devenir un point de friction pour les startups européennes qui grandissent vite sur des produits construits autour du modèle.
- **Des benchmarks tiers indépendants devraient combler le vide actuel.** L’absence de comparaison chiffrée directe entre Qwen3.8-Flash-Next et GPT-6 Astra, Claude Fable 5.1 ou Gemini 3.8 Flash est une situation transitoire : des plateformes comme Artificial Analysis publient généralement ce type de classement dans les semaines suivant une sortie majeure.
- **La rivalité OpenAI-Anthropic-Google avec les modèles ouverts chinois va peser sur la stratégie de licence des trois géants américains.** Aucun des trois n’a pour l’instant annoncé de réponse open-weight directe à Qwen3.8-Flash-Next, mais la pression concurrentielle sur ce segment spécifique continue de monter.

## Ce qu’il faut retenir de cette sortie discrète mais stratégique

Qwen3.8-Flash-Next n’a pas eu droit à une conférence de presse mondiale, mais son rôle dans la stratégie d’Alibaba dépasse largement son statut de simple modèle “Flash” économique. En prévisualisant ouvertement l’architecture MoE multimodale de Qwen4, avec un ratio de 6 milliards de paramètres actifs pour 180 milliards au total, Alibaba envoie un signal clair : la prochaine génération de sa gamme continuera de miser sur l’efficacité d’inférence plutôt que sur la taille brute. Pour les développeurs et les entreprises européennes, la question n’est plus seulement de savoir si ce modèle rivalise avec GPT-6 Astra ou Claude Fable 5.1 sur des benchmarks encore incomplets, mais de déterminer si sa licence, son coût et sa disponibilité sur Hugging Face en font une option crédible pour des déploiements souverains, à un moment où la France et l’Union européenne cherchent activement leurs propres alternatives.

## Foire aux questions

### Qwen3.8-Flash-Next est-il gratuit à utiliser ?

Les poids du modèle sont téléchargeables gratuitement sous la Qwen Community License 1.0, ce qui permet un usage commercial et un fine-tuning dans la plupart des cas. Des obligations d’attribution ou de licence séparée s’appliquent uniquement au-delà de certains seuils d’utilisateurs ou de revenus, ou pour les services de type modèle-en-tant-que-service. L’utilisation via l’API hébergée QwenCloud, elle, est facturée au token.

### Quelle est la différence entre Qwen3.8-Flash-Next et Qwen3.8-Max ?

Qwen3.8-Max est un modèle dense visant la qualité maximale, tandis que Qwen3.8-Flash-Next est un modèle à mélange d’experts optimisé pour un coût d’inférence faible et un contexte long, avec seulement 6 milliards de paramètres actifs par token contre un nombre bien plus élevé pour Qwen3.8-Max.

### Qwen4 est-il déjà disponible en septembre 2026 ?

Non. À la mi-septembre 2026, Qwen4 reste une architecture annoncée mais non publiée sous forme de modèle nommé et daté. Qwen3.8-Flash-Next en est présenté comme un aperçu technique, pas comme une version préliminaire de Qwen4 elle-même.

### Qwen3.8-Flash-Next est-il meilleur que GPT-6 Astra ou Claude Fable 5.1 ?

Aucune comparaison chiffrée directe et vérifiée n’existe à ce jour entre ces modèles sur des benchmarks communs. Qwen3.8-Flash-Next revendique des scores solides en ingénierie logicielle (58,7 sur DeepSWE, 62,5 sur SWE-bench Pro) pour un coût d’inférence très inférieur, mais cela ne permet pas d’affirmer une supériorité générale sur les modèles fermés les plus chers.

### Peut-on héberger Qwen3.8-Flash-Next en France pour des raisons de souveraineté ?

Rien dans la licence n’interdit explicitement un hébergement en France. Le modèle est distribué via Hugging Face et ModelScope, deux plateformes accessibles en Europe. En revanche, aucune certification spécifique de conformité à l’AI Act européen n’est mentionnée par Alibaba à ce jour, ce qui reste un point à vérifier avant tout déploiement en production dans un secteur régulé.

### Quelle est la taille du fichier à télécharger pour utiliser Qwen3.8-Flash-Next en local ?

Le dépôt Hugging Face du modèle pèse environ 360 gigaoctets, ce qui nécessite une infrastructure GPU conséquente pour un hébergement local, même si seuls 6 milliards de paramètres sont activés par token lors de l’inférence.

### Qwen3.8-Flash-Next fonctionne-t-il avec des images et des vidéos ?

Oui, le modèle intègre un encodeur visuel et accepte du texte, des images et de la vidéo en entrée. La sortie reste en revanche uniquement textuelle.
