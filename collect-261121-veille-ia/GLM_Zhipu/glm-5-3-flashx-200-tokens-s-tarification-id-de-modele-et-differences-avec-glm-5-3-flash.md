---
id: collect-261121-veille-ia/glm-zhipu/glm-5-3-flashx-200-tokens-s-tarification-id-de-modele-et-differences-avec-glm-5-3-flash
title: "GLM-5.3-FlashX : 200 tokens/s, tarification, ID de modèle et différences avec GLM-5.3-Flash"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: []
keywords: ["glm-5", "flashx", "flash", "modele", "tokens", "prix", "zhipu", "palier"]
source: https://apimaster.ai/blog/glm-5-3-flashx-api
source_anchor: ""
source_lines: [1, 261]
section: "GLM_Zhipu"
---
**GLM-5.3-FlashX est le palier de service haute vitesse de GLM-5.3-Flash de Zhipu, publié le 18 septembre 2026 avec une vitesse d'inférence de pointe annoncée de 200 tokens/s.** Ce n'est pas un nouveau modèle : c'est le même système GLM-5.3-Flash — 320B de paramètres au total avec 18B activés, une fenêtre de contexte de 1M de tokens, jusqu'à 128 000 tokens en sortie, et une entrée native image, vidéo, fichier et texte — servi via une configuration d'inférence plus rapide.

L'ID de modèle de l'API est **`glm-5.3-flashx`** et il se place à côté de **`glm-5.3-flash`**. Le compromis est simple : FlashX coûte plus cher par token que Flash (Zhipu affiche **0,37 $ en entrée / 1,25 $ en sortie** par 1M de tokens contre **0,15 $ / 0,50 $** pour Flash) et renvoie des réponses plus rapides. Flash est aussi le palier dont les poids sont ouverts et celui inclus dans le GLM Coding Plan, où Flash obtient 3× le quota de GLM-5.3.

**Sur APIMaster, GLM-5.3-FlashX est disponible sous l'ID de modèle `glm-5.3-flashx` à partir de 0,32 $ en entrée / 1,12 $ en sortie par 1M de tokens** — environ 13 % en dessous du prix catalogue de Zhipu. Le reste de la famille GLM est remisé plus fortement : `glm-5.3-flash` à partir de **0,105 $ / 0,35 $** (environ 30 % de remise sur le catalogue), `glm-5.3` à partir de **0,69 $ / 2,17 $** (environ 51 % de remise), et `glm-5.2` à partir de **0,84 $ / 2,64 $** (environ 40 % de remise). Voir les prix des routes du jour sur la fiche GLM-5.3-FlashX.

**Vérification des prix APIMaster, 2026-09-24 :** Voici le prix officiel, le prix APIMaster, les routes actives et la fiche. Les prix changent ; **consultez la fiche du marché**。

| ID du modèle | Réduction | Prix actuel APIMaster | Fiche | 
|---|---|---|---|
| `glm-5.3-flashx` | 13% off | `$0.3213 / $1.1247` | Voir la réduction | 

Si vous avez besoin de débit, de latence interactive ou d'une boucle d'agent qui enchaîne de nombreux tours courts, FlashX est le palier conçu pour cela. Si vous optimisez le coût par tâche terminée et que vous pouvez attendre, Flash est moins cher pour un travail identique.

Essai GPT(GPT-6 Astra, GPT-6 Sol, GPT-5.6, GPT-5.5, gpt-5.6-luna, gpt-5.6-sol, gpt-5.6-terra, gpt-6.1-sol, gpt-6-astra, gpt-6-sol, gpt-6-luna, gpt-6.2-astra et gpt-6.2-sol disponibles)

Inscrivez-vous et recevez $20 d’essai GPT

## Qu'est-ce que GLM-5.3-FlashX ?

GLM-5.3-FlashX est le point de terminaison optimisé pour la vitesse de la famille GLM-5.3-Flash. Zhipu l'a annoncé le 18 septembre 2026, le présentant comme plus rapide et plus fluide pour les entreprises et les développeurs, avec l'API et le centre d'expérience officiel tous deux ouverts au lancement.

Deux choses méritent d'être distinguées :

- **Le modèle** — GLM-5.3-Flash, un modèle multimodal natif 320B-A18B que Zhipu a open-sourcé le 26 août 2026. L'architecture, les poids, la longueur de contexte et les capacités sont partagés.
- **Le palier de service** — FlashX, une configuration d'inférence optimisée pour le débit. Zhipu annonce des vitesses allant jusqu'à 200 tokens/s et attribue le gain à un travail d'infrastructure plutôt qu'à un checkpoint différent.

Cette distinction compte lorsque vous l'évaluez. Les benchmarks, les limites de contexte et le comportement multimodal décrits pour GLM-5.3-Flash s'appliquent à FlashX parce qu'il s'agit du même modèle. La latence et le prix, non : ceux-ci changent avec le palier de service.

## Spécifications du modèle en un coup d'œil

| Spécification | GLM-5.3-FlashX | 
|---|---|
| ID de modèle API | `glm-5.3-flashx` | 
| ID de modèle frère | `glm-5.3-flash` | 
| Publié | 18 septembre 2026 | 
| Paramètres totaux / activés | 320B / 18B | 
| Couches | 45 | 
| Fenêtre de contexte | 1M de tokens | 
| Tokens de sortie maximum | 128K | 
| Modalités d'entrée | Vidéo, image, texte, fichier | 
| Modalité de sortie | Texte | 
| Vitesse de pointe annoncée | 200 tokens/s | 
| Mode de réflexion | `thinking.type: enabled` uniquement ; il ne peut pas être désactivé | 
| Fonctionnalités API principales | Streaming, appel de fonctions, mise en cache du contexte, sortie structurée, streaming d'outils | 

Zhipu recommande `temperature: 1`, `top_p: 0.95` et `reasoning_effort: max`. Pour le travail multi-tours, il recommande de conserver l'historique de réflexion plutôt que de l'effacer (`clear_thinking: false`), et pour les requêtes en streaming, il recommande d'activer à la fois `stream: true` et `tool_stream: true`.

## GLM-5.3-FlashX vs GLM-5.3-Flash

| Dimension | GLM-5.3-Flash | GLM-5.3-FlashX | 
|---|---|---|
| Modèle sous-jacent | GLM-5.3-Flash | GLM-5.3-Flash | 
| Vitesse de pointe annoncée | Palier standard | Jusqu'à 200 tokens/s | 
| Prix catalogue entrée / 1M | 0,15 $ | 0,37 $ | 
| Prix catalogue sortie / 1M | 0,50 $ | 1,25 $ | 
| Limites de contexte et de sortie | 1M / 128K | 1M / 128K | 
| Entrée multimodale | Oui | Oui | 
| Poids ouverts | Oui, depuis le 26 août 2026 | Même modèle de base | 
| GLM Coding Plan | Inclus, avec 3× le quota de GLM-5.3 | Non inclus au lancement | 

Le format de requête est identique. Passer d'un palier à l'autre revient à modifier le champ `model`, et non à réécrire votre intégration — ce qui fait de FlashX un candidat raisonnable pour un test A/B sur les charges de travail où le temps par tour est le goulot d'étranglement.

## Ce que « 200 tokens/s » vous dit et ne vous dit pas

Zhipu publie 200 tokens/s comme un maximum. Lisez-le comme un plafond de vitesse de génération, et non comme une promesse sur votre charge de travail :

- **C'est un chiffre de pointe.** Le débit réel dépend de la longueur du prompt, des hits de cache, de la concurrence et du fournisseur sélectionné.
- **Ce n'est pas le temps jusqu'au premier token.** Un débit de décodage rapide paraît toujours lent si le modèle réfléchit plusieurs secondes avant d'émettre quoi que ce soit. Pour les produits interactifs, mesurez les deux.
- **Ce n'est pas la latence totale de la tâche.** Un tour d'agent qui appelle trois outils est borné par l'exécution des outils, pas par le débit de tokens.
- **Le contexte long change la donne.** À 1M de tokens, le prefill et le comportement du cache KV dominent. C'est exactement ce que cible l'architecture Flash.

La règle pratique : évaluez Flash et FlashX sur vos propres prompts, et comparez **le temps jusqu'au premier token, les tokens de sortie par seconde et le coût par tâche terminée** plutôt qu'un seul chiffre de vitesse.

## D'où vient la vitesse

Zhipu attribue l'accélération de FlashX à un investissement en infrastructure plutôt qu'à une nouvelle architecture, bâti sur les 100 000 accélérateurs d'IA nationaux qui servent déjà le trafic de GLM-5.3-Flash.

- **Un moteur d'inférence dédié sur SGLang** , écrit pour cette architecture plutôt qu'adapté depuis une pile généraliste.
- **Une optimisation mémoire pour les contextes de 1M de tokens** , incluant ReplaySSM, la quantification W8A8, la quantification de cache hybride INT8/FP8/BF16 et le Layer Split.
- **Une architecture de service désagrégée Encode–Prefill–Decode (EPD)** qui sépare l'encodage multimodal, le prefill du prompt et le décodage token par token en pools de workers planifiés indépendamment, afin que chaque étape monte en charge de son côté.
- **Une amélioration de 3× du service de bout en bout** par rapport à la ligne de base initiale sur le même matériel, ce qui, selon Zhipu, ramène le coût par token à un niveau comparable à celui des GPU NVIDIA grand public.

Un détail de ce travail mérite d'être relevé à part : Zhipu indique qu'un agent d'infrastructure propulsé par GLM-5.3 a aidé les ingénieurs à optimiser les kernels, diagnostiquer les goulots d'étranglement et améliorer la pile de service — le modèle participant au système qui le sert.

## Benchmarks : ce que Zhipu rapporte pour le modèle de base

Tous les chiffres suivants sont publiés par Zhipu pour GLM-5.3-Flash et s'appliquent à FlashX parce que le modèle est le même. Ce sont des résultats rapportés par le fournisseur, et non des reproductions indépendantes.

| Benchmark | GLM-5.3-Flash | GLM-5.2 | 
|---|---|---|
| DeepSWE v1.1 | 63,4 | 46,2 | 
| AutomationBench | 48,8 | 26,2 | 
| Z.ai Code Bench v1.0, effort max | 29,0 | Claude Opus 4.8 : 29,5 | 

Zhipu rapporte également que GLM-5.3-Flash obtient **57 sur l'Artificial Analysis Intelligence Index v4.1.1 à 0,045 $ par tâche** dans le cadre de sa tarification remisée — un niveau qui, selon lui, n'était auparavant disponible qu'à environ 10× le coût — et que ses performances de codage sont comparables à Claude Opus 4.8 sur le Z.ai Code Bench interne de l'entreprise.

Considérez ces éléments comme une indication, non comme une garantie. Les benchmarks des fournisseurs sont généralement exécutés sur des versions de harnais favorables au fournisseur ; la ligne Z.ai Code Bench ci-dessus a été mesurée sur Claude Code 2.1.207. Relancez votre propre évaluation avant de basculer du trafic de production.

## Architecture : pourquoi le palier Flash est peu coûteux à exécuter

FlashX hérite de la conception qui a rendu Flash peu coûteux à servir, ce qui explique pourquoi un palier plus rapide peut exister sans bond de prix vers les niveaux phares.

- **Attention hybride sparse et linéaire.** Zhipu le décrit comme le premier modèle frontière open-source à combiner les deux. L'attention linéaire capture les dépendances locales par modélisation d'état ; l'attention sparse récupère le contexte global pertinent via un indexeur léger.
- **IndexPool.** Quatre vecteurs de clé d'indexeur sont compressés en un seul par pooling pondéré, réduisant la latence et la surcharge mémoire de l'indexeur à un contexte de 1M de tokens.
- **Manifold-Constrained Hyper-Connections (mHC)** pour une meilleure efficacité de mise à l'échelle.
- **Un coût par token inférieur à GLM-5.3.** Zhipu rapporte 3,01× moins de calcul d'attention et un cache KV 4,44× plus petit que GLM-5.3. Par rapport à GLM-5.3, le nombre de paramètres activés passe de 32B à 18B et les couches de 92 à 45.
- **Un corpus de pré-entraînement multimodal de 30 000 milliards de tokens.**

Zhipu reconnaît franchement que le cache KV est encore légèrement plus grand que celui de Kimi-K3 et de DeepSeek-V4-Flash et qualifie cela de piste pour les travaux futurs — un rappel utile que les comparaisons d'architecture se font dimension par dimension, et non selon un classement unique.

## Ox-Alpha : le modèle anonyme testé en public

Avant le lancement de Flash, Zhipu a fait tourner GLM-5.3-Flash anonymement sous le nom **Ox-Alpha** sur OpenCode et OpenRouter. Selon Zhipu, il est devenu le modèle le plus populaire de la semaine et a établi des records d'utilisation sur les deux plateformes, tout ce trafic étant servi sur des puces d'IA chinoises.

Pour les développeurs, l'intérêt ne réside pas dans la position au classement mais dans la méthode de validation : du trafic réel, de vrais agents de codage, un nom de modèle inconnu et aucun attrait de marque. C'est un signal plus fort qu'un tableau de benchmarks, bien qu'il s'agisse toujours d'un déploiement contrôlé par le fournisseur, sans méthodologie publiée.

## Multimodal natif et codage visuel

GLM-5.3-Flash est le premier modèle de la série GLM-5 conçu comme multimodal dès le départ, et FlashX conserve cette caractéristique. Les images sont transmises sous forme de bloc de contenu avec `type: image_url` à l'intérieur de `messages[].content[]`, en utilisant soit une URL, soit une URL de données Base64, et plusieurs blocs peuvent être combinés dans un même message. L'entrée vidéo et fichier est documentée aux côtés de l'image et du texte.

La capacité qui compte le plus en pratique est le codage visuel : le modèle inspecte une interface rendue, la compare à la cible et itère. Zhipu documente des flux de travail pour reconstruire une application à partir de captures d'écran ou d'enregistrements, construire des scènes Blender, produire des prototypes de jeux Godot, exécuter des agents de navigateur et d'usage d'ordinateur, et reproduire des pièces de CAO — chacun avec le modèle vérifiant sa propre sortie rendue plutôt que de simplement générer du code.

La même boucle s'étend au travail documentaire. Zhipu démontre des livrables PPTX, PDF, DOCX et XLSX, de la recherche financière avec sourçage traçable, de la revue de contrats avec annotations suivies, et de la rédaction juridique, le modèle rendant et inspectant visuellement sa propre sortie pour détecter les débordements, les désalignements et les styles incohérents.

Un exemple donné par Zhipu est inhabituellement concret : sans aucun actif externe, GLM-5.3-Flash a tourné de manière autonome pendant 16 heures pour construire une résidence de chef et une cuisine d'essai d'environ 400 m² en tant que scène Blender.

## Tarification : combien coûte FlashX

Prix catalogue officiels par 1M de tokens, issus des pages de tarification de Zhipu (vérifiés le 18 septembre 2026) :

| Type de token | GLM-5.3-FlashX | GLM-5.3-Flash | GLM-5.3 | 
|---|---|---|---|
| Entrée (USD) | 0,37 $ | 0,15 $ | 1,40 $ | 
| Entrée en cache (USD) | 0,075 $ | 0,03 $ | 0,26 $ | 
| Sortie (USD) | 1,25 $ | 0,50 $ | 4,40 $ | 

Le stockage de l'entrée en cache est indiqué comme gratuit à durée limitée sur les trois paliers.

**Exemple de coût.** Pour 10M de tokens d'entrée non mis en cache et 1M de tokens de sortie, les prix catalogue donnent :

| Charge de travail | GLM-5.3-FlashX | GLM-5.3-Flash | GLM-5.3 | 
|---|---|---|---|
| 10M entrée + 1M sortie | 4,95 $ | 2,00 $ | 18,40 $ | 
| Même volume, toute l'entrée depuis le cache | 2,00 $ | 0,80 $ | 6,60 $ | 

FlashX coûte environ 2,5× Flash en entrée et en sortie, et reste bien en dessous de GLM-5.3. Savoir si cela en vaut la peine dépend entièrement de ce que vous coûte un tour lent — pour un pipeline par lots, c'est rarement le cas, et pour un agent interactif, c'est souvent le cas.

## Tarification GLM sur APIMaster

Le prix catalogue de Zhipu est ce que le modèle coûte sur la propre plateforme de Zhipu. Les prix des routes sur APIMaster sont ce que vous payez pour le même modèle via la place de marché, et chaque palier GLM se vend actuellement en dessous du catalogue :

| ID de modèle | Prix catalogue Zhipu / 1M | APIMaster à partir de / 1M | En dessous du catalogue | 
|---|---|---|---|
| `glm-5.3-flashx` | 0,37 $ / 1,25 $ | 0,32 $ / 1,12 $ | ≈13 % | 
| `glm-5.3-flash` | 0,15 $ / 0,50 $ | 0,105 $ / 0,35 $ | ≈30 % | 
| `glm-5.3` | 1,40 $ / 4,40 $ | 0,69 $ / 2,17 $ | ≈51 % | 
| `glm-5.2` | 1,40 $ / 4,40 $ | 0,84 $ / 2,64 $ | ≈40 % | 

*Entrée / sortie, USD par 1M de tokens, vérifiés sur la place de marché en direct le 20 septembre 2026. « À partir de » correspond à la route la moins chère vendant ce modèle au moment de la vérification ; la fiche de la place de marché fait foi au moment de la requête.*

Deux détails comptent lorsque vous lisez ce tableau :

- **« À partir de » est un prix de route.** Chaque modèle est servi par plusieurs routes à des prix différents : FlashX en a actuellement deux, la moins chère à 0,32 $ et une au prix catalogue de Zhipu à 0,37 $. Ce que vous payez dépend de la route qui sert votre requête, alors confirmez la fiche avant de basculer du trafic de production.
- **Le palier le plus récent porte la plus petite remise.** FlashX se situe environ 13 % en dessous du catalogue parce qu'il s'agit de la nouvelle configuration haute vitesse ; les réductions plus profondes concernent les paliers en dessous —`glm-5.3-flash` à environ 30 %, et`glm-5.3` /`glm-5.2` à 40–51 %. Si vous n'avez pas besoin du profil de latence de FlashX, l'avantage de prix est le plus important en dessous.

Calculée aux prix des routes, la charge de travail de 10M d'entrée / 1M de sortie ci-dessus coûte **4,34 $ sur la route FlashX la moins chère**, **1,40 $ sur la route Flash la moins chère**, et **9,09 $ sur la route GLM-5.3 la moins chère** — contre 4,95 $ / 2,00 $ / 18,40 $ au catalogue.

L'entrée en cache et le GLM Coding Plan sont tarifés par le fournisseur plutôt que par la route, donc les tarifs de cache et le quota du plan décrits ci-dessus restent inchangés.

Fiches en direct pour les quatre paliers : **GLM-5.3-FlashX** · **GLM-5.3-Flash** · **GLM-5.3** · **GLM-5.2**. Chaque fiche montre les routes qui vendent ce modèle en ce moment, leurs tarifs d'entrée et de sortie, et leur disponibilité. Pour appeler FlashX à ces tarifs, créez un compte APIMaster, ajoutez du crédit et utilisez l'ID de modèle `glm-5.3-flashx` — avec le même client compatible OpenAI que vous utilisez déjà.

## Comment appeler GLM-5.3-FlashX

FlashX utilise la même interface de complétions de chat que Flash, donc la migration se fait en une ligne.

```
from openai import OpenAI
client = OpenAI(
    api_key="YOUR_APIMASTER_KEY",
    base_url="https://apimaster.ai/v1",
)
response = client.chat.completions.create(
    model="glm-5.3-flashx",
    messages=[
        {
            "role": "user",
            "content": "Refactor this module for testability and show the diff.",
        }
    ],
    temperature=1,
    top_p=0.95,
    extra_body={
        "reasoning_effort": "max",
        "thinking": {"type": "enabled", "clear_thinking": False},
    },
)
print(response.choices[0].message.content)
```
Pour le streaming, ajoutez `stream: true` et `tool_stream: true` comme le recommande Zhipu. Notez que la réflexion ne peut pas être désactivée sur ce modèle, donc prévoyez des tokens de raisonnement dans votre estimation de coût et dans vos délais d'attente côté client.

Un chemin de migration pratique : gardez l'ID de modèle configurable, envoyez une tranche représentative du trafic de production à la fois vers `glm-5.3-flash` et `glm-5.3-flashx`, et comparez le taux d'achèvement, le temps jusqu'au premier token et le coût par tâche terminée avant de basculer une charge de travail.

## Choisir entre FlashX, Flash et GLM-5.3

- **Choisissez FlashX** pour les produits interactifs, les complétions côté IDE et les boucles d'agents à nombreux tours courts, où le temps réel par tour est la contrainte et où la charge de travail reste dans les capacités de la classe Flash.
- **Choisissez Flash** pour le traitement par lots, la génération hors ligne et les pipelines à gros volume où le coût par tâche compte plus que la latence — et pour les déploiements à poids ouverts ou auto-hébergés, puisque Flash est le palier dont les poids sont publiés.
- **Choisissez GLM-5.3** lorsque vous avez besoin du plafond du modèle phare plutôt que du profil de coût du palier Flash.
- **Ne supposez pas que le gain de vitesse s'applique uniformément.** Les requêtes à contexte long et à fort prefill et les tours d'agents liés aux outils peuvent bénéficier de beaucoup moins que les tâches de génération courte. Mesurez la charge de travail que vous exécutez réellement.

## Démarrez avec la famille GLM sur APIMaster.ai

APIMaster vous donne une seule clé compatible OpenAI pour la famille GLM aux côtés de Claude, GPT, DeepSeek, Qwen, Gemini, Kimi et d'autres modèles — avec une tarification à l'usage à partir de **1 $** et jusqu'à **70 % de réduction** sur les tarifs officiels sur certaines routes. Les prix des routes GLM commencent actuellement à **0,105 $ / 0,35 $** par 1M de tokens pour `glm-5.3-flash` et **0,32 $ / 1,12 $** pour `glm-5.3-flashx`.

Parce que FlashX conserve le même format de requête que Flash, les équipes qui appellent déjà GLM via APIMaster peuvent évaluer le palier plus rapide sans toucher à leur intégration au-delà de l'ID de modèle.

1. **Créez un compte APIMaster** .
2. Ajoutez du crédit à l'usage, à partir de **1 $** .
3. Créez une clé API dans la **console APIMaster** .
4. Pointez votre client compatible OpenAI vers `https://apimaster.ai/v1` et définissez l'ID de modèle que vous souhaitez évaluer.

**S'inscrire sur APIMaster** · **Explorer la place de marché des modèles** · **Tester l'identité d'un modèle**

## FAQ

**GLM-5.3-FlashX est-il un nouveau modèle ?**
Non. C'est le palier de service haute vitesse de GLM-5.3-Flash. Même modèle, même fenêtre de contexte, même entrée multimodale — une configuration d'inférence plus rapide et un prix différent.

**Quel est l'ID de modèle de GLM-5.3-FlashX ?**
`glm-5.3-flashx`. Le palier standard est `glm-5.3-flash`.

**À quelle vitesse va GLM-5.3-FlashX ?**
Zhipu annonce un maximum de 200 tokens/s. C'est un chiffre de pointe ; mesurez le temps jusqu'au premier token et le débit soutenu sur vos propres prompts.

**Combien coûte GLM-5.3-FlashX ?**
Zhipu affiche 0,37 $ par 1M de tokens d'entrée, 1,25 $ par 1M de tokens de sortie et 0,075 $ par 1M de tokens d'entrée en cache — environ 2,5× le prix de GLM-5.3-Flash en entrée et en sortie. Sur APIMaster, les routes pour `glm-5.3-flashx` commencent à **0,32 $ / 1,12 $** par 1M de tokens, environ 13 % en dessous du prix catalogue de Zhipu.

**Quelle est la fenêtre de contexte ?**
1M de tokens, avec jusqu'à 128 000 tokens de sortie, comme GLM-5.3-Flash.

**GLM-5.3-FlashX peut-il accepter des images et de la vidéo ?**
Oui. Il accepte les entrées vidéo, image, texte et fichier et renvoie du texte. Les images sont envoyées sous forme de bloc de contenu `image_url`, par URL ou URL de données Base64.

**GLM-5.3-FlashX est-il inclus dans le GLM Coding Plan ?**
Pas au lancement. GLM-5.3-Flash est entièrement disponible sur le plan avec 3× le quota de GLM-5.3, et les appels hors pointe, y compris tout le week-end, consomment 50 % des points standard.

**Puis-je désactiver la réflexion pour économiser des tokens ?**
Non. `thinking.type` ne prend en charge que `enabled`. Zhipu recommande de conserver l'historique de réflexion (`clear_thinking: false`) pour le travail multi-tours.

**Les chiffres de benchmark sont-ils indépendants ?**
Non. Ils sont publiés par Zhipu. Considérez-les comme indicatifs et relancez votre propre évaluation avant d'engager du trafic de production.

## Sources et lectures complémentaires

- Z.ai — Documentation des modèles GLM-5.3-Flash/FlashX — ID de modèles, paramètres, capacités, réglages recommandés, benchmarks et détails de service
- Z.ai — Tarification — prix catalogue officiels par 1M de tokens
- Z.ai — Blog technique GLM-5.3-Flash — architecture, comparaisons d'efficacité et tableaux d'évaluation
- Hugging Face — zai-org/GLM-5.3-Flash — poids ouverts pour le modèle de base
- PANews — Zhipu lance GLM-5.3-FlashX — rapport de lancement et contexte Ox-Alpha

Toutes les sources ont été vérifiées le 18 septembre 2026. Les prix et la disponibilité changent ; vérifiez les conditions actuelles avant d'engager une charge de travail importante.
