Cursus

La course aux modèles ouverts a de nouveau bougé le 16 juillet 2026, lorsque Moonshot AI a lancé Kimi K3, un modèle de 2,8 billions de paramètres, doté d'une fenêtre de contexte d'un million de jetons et de la vision native. C'est le plus grand modèle ouvert publié par Moonshot, largement au-delà de Kimi K2 en taille, et le premier qu'ils décrivent comme relevant de la classe des 3 billions de paramètres.

Si vous cherchez l'histoire du lancement, l'analyse d'architecture, les graphiques de benchmarks, les comparaisons avec Claude, GPT et les autres laboratoires chinois, ainsi que la liste des limites de Moonshot, notre article de blog sur Kimi K3 couvre tout cela. Ce tutoriel est le pendant pratique : comment y accéder et comment il se comporte à l'usage. Je passe en revue cinq petits exemples, quatre via l'API où je montre l'usage réel des jetons et le coût, et deux dans l'application web kimi.com. Ensemble, ils montrent comment K3 gère :

- L'appel d'outils et le retour d'un JSON strict
- Le chargement à la volée d'une définition d'outil
- La réduction du coût des longs contextes grâce au cache automatique
- La lecture d'une capture d'écran et la correction de la mise en page
- La création d'un tableau de bord interactif à partir d'un seul prompt

Les quatre exemples API ont été exécutés le 17 juillet 2026 avec le modèle `kimi-k3` et ont coûté environ 11 cents à froid, ou quelques cents après l'activation du cache.

## Comment accéder à Kimi K3

Le moyen le plus rapide d'essayer le modèle est kimi.com, où l'application web et les applications mobiles utilisent Kimi K3 pour des tâches d'agent générales sans configuration.

Pour des travaux plus lourds comme des rapports et des tableaux de bord, il y a Kimi Work, une application de bureau.

Si vous vivez dans le terminal, Kimi Code est un agent de codage à installer via npm sous `@moonshot-ai/kimi-code`, et vous choisissez le modèle avec la commande `/model`. L'usage de K3 dans Kimi Code nécessite un abonnement payant, et la fenêtre d'un million de jetons exige un palier supérieur.

Ce tutoriel se concentre sur l'API brute et l'application web, mais l'agent en ligne de commande est disponible si vous en avez besoin.

K3 ne remplace toutefois pas ses aînés. Le tableau ci-dessous montre comment se répartit la gamme actuelle.

| **Modèle** | **Fenêtre de contexte** | **Idéal pour** | 
| `kimi-k3` | 1 48 576 jetons | Travaux phares : long codage, vision, connaissances | 
| `kimi-k2.7-code` | 262 144 jetons | Codage dédié, avec une option haute vitesse plus rapide | 
| `kimi-k2.6` | 262 144 jetons | Conversation générale texte, image et vidéo | 

En bref, K3 est le modèle à privilégier quand une tâche mêle code, outils, documents et images, ou lorsque vous avez réellement besoin de la fenêtre d'un million de jetons. Pour la génération de code pure où la vitesse prime sur le contexte, `kimi-k2.7-code` reste le choix le plus judicieux : ne supposez pas que le dernier modèle est toujours le meilleur pour votre cas.

## Configuration de l'API Kimi K3

L'API est compatible avec le SDK OpenAI, donc si vous l'avez déjà utilisé, presque rien de ce qui suit ne vous surprendra. Vous avez besoin de Python 3.9 ou plus et d'une clé API.

### Étape 1 : générer une clé API

Commencez par vous connecter à la plateforme Kimi et ouvrez la page API Keys dans la console. Créez une clé, copiez-la une fois et stockez-la en lieu sûr, car vous ne la reverrez pas. Prévoyez aussi un petit crédit sur le compte pour passer des appels ; pour tout ce tutoriel, quelques dollars suffisent largement.

*Création d'une clé API Kimi K3. Image de l'auteur.*

### Étape 2 : installer le SDK

Installez ensuite le SDK OpenAI dans votre environnement. Une seule commande suffit.

`python -m pip install --upgrade "openai>=1.0"`
Cela télécharge la bibliothèque cliente utilisée par le reste des exemples, sans installation spécifique à Kimi.

### Étape 3 : stocker la clé et initialiser le client

Il vaut mieux lire la clé depuis une variable d'environnement que de la coller dans votre code. Définissez `MOONSHOT_API_KEY` dans votre shell ou un fichier `.env` , puis pointez le client vers l'URL de base de Moonshot.

```
import os
from openai import OpenAI
client = OpenAI(
    api_key=os.environ["MOONSHOT_API_KEY"],
    base_url="https://api.moonshot.ai/v1",
)
```
Les deux seules différences par rapport à une configuration OpenAI standard sont le `base_url` et le nom du modèle, à savoir `kimi-k3`. Avec cela en place, vous êtes prêt à faire un premier appel.

### Étape 4 : effectuer votre premier appel

Passons à une première requête. J'ai demandé au modèle de se présenter en une phrase, ce qui a donné un petit moment de franchise.

```
completion = client.chat.completions.create(
    model="kimi-k3",
    messages=[{"role": "user", "content": "Introduce Kimi K3 in one sentence."}],
    max_completion_tokens=800,
)
print(completion.choices[0].message.content)
```
La réponse fut un refus poli de deviner : le modèle a indiqué ne pas disposer d'informations fiables sur Kimi K3, ayant été entraîné avant sa propre sortie, et m'a renvoyé vers les annonces de Moonshot. Rappel utile : un modèle ne se connaît pas lui-même. L'appel API que je viens d'effectuer coûte environ sept dixièmes de cent. Notez le plafond `max_completion_tokens` que je fixe à chaque appel dans ce tutoriel pour éviter que des sorties verbeuses ne fassent grimper la facture.

*Premier retour de l'API Kimi K3. Image de l'auteur.*

## Exemple 1 : raisonnement en streaming et réponse finale

K3 raisonne systématiquement, et l'API renvoie ce raisonnement sur un canal séparé de la réponse. En mode flux, chaque fragment peut contenir `reasoning_content`, le `content` final, ou les deux, ce qui vous permet d'afficher la pensée et la réponse séparément.

```
stream = client.chat.completions.create(
    model="kimi-k3",
    messages=[{"role": "user", "content": "A bat and a ball cost $1.10 together. The bat costs $1.00 more than the ball. How much is the ball?"}],
    max_completion_tokens=1200,
    stream=True,
    stream_options={"include_usage": True},
)
for chunk in stream:
    if not chunk.choices:
        continue
    delta = chunk.choices[0].delta
    reasoning = getattr(delta, "reasoning_content", None)
    if reasoning:
        print(reasoning, end="", flush=True)
    if delta.content:
        print(delta.content, end="", flush=True)
```
Le modèle a d'abord diffusé son raisonnement : il a reconnu la question de la batte et de la balle comme le classique Cognitive Reflection Test, a signalé la réponse intuitive fausse de 0,10 $, puis a posé l'algèbre pour arriver à 0,05 $ et vérifier que 1,05 $ plus 0,05 $ font bien 1,10 $. La séparation est précieuse : dans une application réelle, vous montrez le `content` à vos utilisateurs et conservez le `reasoning_content` pour les journaux, car afficher le raisonnement brut en production est rarement souhaitable. Cet appel a utilisé 488 jetons de sortie et coûté moins d'un cent.

*Raisonnement en streaming puis réponse finale. Image de l'auteur.*

## Exemple 2 : appel d'outils avec sortie structurée

Kimi K3 est le modèle de la gamme qui prend en charge `tool_choice="required"`, ce qui force au moins un appel d'outil pendant un tour. Pratique lorsque vous voulez que le modèle récupère des données avant de répondre plutôt que de deviner. Ici, je lui ai fournis deux faux outils, une consultation de prix et un contrôle de stock, j'ai imposé un appel d'outil, exécuté les outils en local, puis demandé le résultat en JSON strict via `response_format`.

```
first = client.chat.completions.create(
    model="kimi-k3",
    messages=messages,
    tools=TOOLS,
    tool_choice="required",
    max_completion_tokens=2500,
)
assistant_message = first.choices[0].message
messages.append(assistant_message)
for tool_call in assistant_message.tool_calls or []:
    args = json.loads(tool_call.function.arguments)
    messages.append({"role": "tool", "tool_call_id": tool_call.id, "content": run_tool(tool_call.function.name, args)})
```
Le modèle a appelé les deux outils avec le bon code produit, puis a renvoyé un récapitulatif de commande propre en JSON : cinq claviers mécaniques à 89 $ unitaires, un total de 445 $, et un indicateur de stock à true. Deux détails font la différence en pratique : vous devez réinsérer le message complet de l'assistant dans la conversation avant d'ajouter les résultats d'outil, et vous ne devez parser que le `content` pour le JSON, jamais le champ de raisonnement. La paire d'appels a coûté moins d'un cent au total.

Appels d'outils et sortie JSON structurée. Image de l'auteur.

## Exemple 3 : charger des outils dynamiquement

Si vous avez des dizaines d'outils, envoyer toutes leurs définitions à chaque requête gaspille des jetons et encombre le prompt. Kimi K3 vous permet d'injecter une définition d'outil en cours de conversation via un message `system` qui contient un champ `tools` et aucun `content`. L'outil devient disponible à partir de ce point, ce qui maintient les grands catalogues d'outils hors de votre préfixe mis en cache jusqu'à ce qu'un outil soit réellement nécessaire.

```
messages = [
    {"role": "user", "content": "Convert 100 US dollars to euros at a rate of 0.92."},
    {"role": "system", "tools": [{
        "type": "function",
        "function": {
            "name": "convert_currency",
            "description": "Convert an amount from one currency to another",
            "parameters": {
                "type": "object",
                "properties": {"amount": {"type": "number"}, "rate": {"type": "number"}},
                "required": ["amount", "rate"],
            },
        },
    }]},
]
completion = client.chat.completions.create(model="kimi-k3", messages=messages)
print(completion.choices[0].message.tool_calls)
```
K3 a pris en compte l'outil fraîchement chargé et a appelé `convert_currency` avec un montant de 100 et un taux de 0,92, comme prévu. Gardez en tête que le serveur ne conserve pas cette définition pour vous : renvoyez le message system lors des requêtes suivantes si vous voulez que l'outil reste disponible. C'était l'appel le moins cher de la série, à environ deux dixièmes de cent.


Appel d'un outil de conversion dynamique. Image de l'auteur.

## Exemple 4 : réduire le coût des longs contextes avec le cache

C'est ici que la fenêtre d'un million de jetons devient vraiment pratique. Le cache de contexte est automatique : pas d'ID de cache ni de durée de vie à gérer. Vous envoyez un long préfixe, vous le conservez strictement identique aux requêtes suivantes, et la partie répétée est facturée au tarif cache hit plutôt qu'au tarif cache miss. Pour rendre l'écart visible, j'ai utilisé une base de connaissances d'environ 33 000 jetons et posé une question dessus.

```
knowledge = Path("knowledge_base.md").read_text(encoding="utf-8")
completion = client.chat.completions.create(
    model="kimi-k3",
    messages=[
        {"role": "system", "content": knowledge},
        {"role": "user", "content": "What is the rated payload of the Atlas robot?"},
    ],
    max_completion_tokens=600,
)
```
La première fois, rien n'était en cache et la requête a coûté environ 9,9 cents pour quelque 33 000 jetons d'entrée. Une fois le préfixe vu, la même requête a touché le cache sur les 32 512 jetons de préfixe et coûté environ 1,1 cent, soit près d'un facteur neuf. La raison : l'écart de prix ; l'entrée en cache est facturée 0,30 $ par million de jetons contre 3,00 $ hors cache. Un point à noter : l'écriture en cache est asynchrone, donc le hit ne se voit pas sur un appel immédiatement consécutif. Il apparaît sur une requête ultérieure ; exécuter le script deux fois à une minute d'intervalle montre d'abord le miss, puis le hit.

Coût d'un cache miss vs cache hit. Image de l'auteur.

## Exemple 5 : détecter des bugs de mise en page sur une capture d'écran

La vision est native dans K3, et l'API offre un moyen simple de l'utiliser, même si elle n'accepte pas d'URL d'image publique. Vous envoyez l'image en data URL base64 et faites du `content` un tableau d'objets, une partie pour l'image, une pour le texte. J'ai rendu un petit tableau de bord avec quelques bugs de mise en page volontaires, enregistré une capture d'écran et demandé à K3 ce qui n'allait pas.

*Le tableau de bord avec des bugs de mise en page volontaires. Image de l'auteur.*

```
import base64
from pathlib import Path
image_data = base64.b64encode(Path("broken_dashboard.png").read_bytes()).decode()
completion = client.chat.completions.create(
    model="kimi-k3",
    messages=[{
        "role": "user",
        "content": [
            {"type": "image_url", "image_url": {"url": f"data:image/png;base64,{image_data}"}},
            {"type": "text", "text": "List the layout and alignment problems you can see, and give a short CSS fix for each."},
        ],
    }],
    max_completion_tokens=3500,
)
print(completion.choices[0].message.content)
```
K3 a bien lu l'image. Il a repéré la carte plus basse que la rangée et qui chevauche sa voisine, le badge posé sur un nombre (il a même mal lu le 3 910 masqué en 5 910, preuve du bug), l'espace irrégulier avant la dernière carte, la barre qui déborde vers la carte du dessus, ainsi que l'info-bulle posée sur les barres, et a proposé une correction CSS courte pour chacune, comme regrouper les cartes dans une grille. En revanche, il a omis le sous-titre à très faible contraste : la vision capte mieux ce qui saute aux yeux que les détails discrets. L'appel a coûté environ deux cents.

## Limites de Kimi K3

Les exemples API se sont bien passés, mais quelques aspérités méritent d'être signalées pour éviter les surprises. J'ai rencontré la plupart directement.

- 
Seul `reasoning_effort="max"` est disponible pour l'instant, vous ne pouvez donc pas encore réduire le raisonnement pour économiser.
- 
Les paramètres d'échantillonnage sont figés. Des valeurs comme `temperature` ,`top_p` et les pénalités sont verrouillées ; ne les joignez pas aux requêtes au lieu d'essayer de les ajuster.
- 
La sortie peut devenir longue et coûteuse. Limitez `max_completion_tokens` , comme dans les exemples, et validez toute boucle d'agent.
- 
Les URL d'image publiques ne sont pas prises en charge via l'API ; prévoyez le base64 ou des fichiers téléversés pour la vision.

Rien de tout cela n'est bloquant, mais ces points influencent la façon d'utiliser le modèle. Le coût de sortie est celui que je surveillerais en priorité.

## Conclusion

Au fil de mes essais, deux choses ressortent. L'appel d'outils et la sortie structurée ont fonctionné sans relance, et le cache a compté davantage que prévu : réutiliser le même long préfixe a rendu peu coûteuse la répétition d'une grosse requête. Pour l'analyse à l'échelle d'un dépôt, les appels répétés à long contexte ou l'ingénierie multimodale, K3 est un choix raisonnable par défaut ; pour un chat rapide et à bas coût ou un contrôle fin de l'échantillonnage, un modèle plus petit sera plus simple. Les détails sur les poids ouverts et la licence, que j'ai signalés plus haut, devraient être clarifiés après la sortie du 27 juillet.

Pour approfondir les schémas utilisés dans ces exemples, notre cours Developing AI Systems with the OpenAI API couvre le function calling et le raccordement des modèles à des outils externes en Python.

Je suis ingénieur de données et créateur de communautés. Je travaille sur les pipelines de données, le cloud et les outils d'IA, tout en rédigeant des tutoriels pratiques et percutants pour DataCamp et les développeurs émergents.