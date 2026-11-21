---
id: collect-261121-veille-ia/deepseek/comment-installer-deepseek-harness-en-10-minutes-guide-d-installation-2026
title: "Comment installer DeepSeek Harness en 10 minutes (Guide d'installation 2026)"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-08-13"]
keywords: ["deepseek", "vous", "harness", "installation", "installer", "vide", "fournisseur", "modele"]
source: https://www.atlascloud.ai/blog/tips/how-to-install-deepseek-harness
source_anchor: ""
source_lines: [1, 368]
section: "DeepSeek"
---
Installer DeepSeek Harness prend une commande. Trente secondes plus tard, vous regardez une belle interface, complètement vide, sur `http://127.0.0.1:3080`, et personne ne vous a dit quoi faire ensuite.

Le README est notoirement maigre. Le papier Cordis qu'il lie parle de composabilité spatiotemporelle. Vous vouliez juste que l'outil lise votre code.

Ce guide commence à cet écran vide. Vous obtiendrez la commande d'installation, oui, mais la partie qui vous coûte un après-midi est la connexion d'un fournisseur de modèle, et il y a trois valeurs par défaut dans cette étape qui cassent silencieusement les modèles DeepSeek et jettent 75% de votre fenêtre de contexte. Presque personne ne les documente.

**Points clés à retenir**

- `npx @deepseek-ai/dsh web` est l'installation complète. Node.js`^22.19.0` ou`>=24.0.0` est le seul prérequis.
- DeepSeek Harness est un harnais, pas un modèle. Il est livré sans aucune information d'identification, donc il ne fait rien tant que vous n'attachez pas un fournisseur.
- Tout point de terminaison compatible OpenAI fonctionne, y compris l'API propre de DeepSeek, une passerelle, ou un serveur Ollama local.
- Lorsque vous ajoutez un fournisseur personnalisé, le harnais devine le dialecte de réflexion à partir de votre URL de base. Si la supposition est erronée, `reasoning_content` se casse.
- Les modèles déclarés manuellement utilisent par défaut une fenêtre de contexte de 262 144, donc la fenêtre complète de 1 048 576 de V4 reste désactivée jusqu'à ce que vous spécifiiez le contraire.

Terminal affichant des tests réussis en vert à côté d'un harnais modulaire lumineux, illustrant comment installer DeepSeek Harness

## La version en 60 secondes de ce que vous construisez

Voici le résultat avant toute théorie. Trois fichiers dans un dossier vide, une invite à coller, et l'agent lit le code, exécute pytest, regarde deux tests échouer, change un seul caractère, et ré-exécute jusqu'à ce que la suite soit verte.

DeepSeek Harness lisant fizzbuzz.py, exécutant pytest, corrigeant le bug de décalage et ré-exécutant pour 3 tests réussis

*La boucle complète : lire, exécuter, diagnostiquer, corriger, ré-exécuter. Chaque étape atterrit dans un journal de session en ajout seulement que vous pouvez rejouer plus tard.*

C'est la démo que nous allons construire ensemble à l'étape 6. Elle est délibérément assez petite pour être reproduite dans un répertoire temporaire, et elle ne dépend d'aucun référentiel externe qui pourrait changer la semaine prochaine.

## Pourquoi la plupart des tentatives d'installation de DeepSeek Harness échouent à l'étape deux

L'installation est vraiment triviale. Le blocage se produit immédiatement après, et c'est une décision de conception plutôt qu'un bug : DeepSeek Harness est livré sans clés, sans fournisseur par défaut et sans modèle intégré.

DeepSeek a open-sourcé le projet le 13 août 2026 sous licence MIT, et il a décollé rapidement. Au 17 août 2026, le référentiel compte 144 361 étoiles et 14 689 forks (GitHub, août 2026), ce qui fait beaucoup de personnes arrivant au même écran vide la même semaine.

Le fil Hacker News de la semaine de lancement capture la réaction partagée. Les gens aiment la transparence : tout ce que le modèle voit est enregistré dans un journal de session en ajout seulement, et un commentateur a noté que "les modèles US ne vous laissent pas voir ça." Les plaintes sont tout aussi constantes. "Le README est assez nu en dehors des instructions d'installation," a écrit un développeur, et le papier Cordis a reçu le verdict qu'il ressemble à de la "salade de mots" (Hacker News, août 2026).

Donc, les quatre choses qui bloquent réellement les gens sont toutes post-installation :

- Node est trop vieux, donc `npx` échoue avant même de commencer.
- Le port 3080 est déjà occupé par un autre serveur de développement.
- Le fournisseur personnalisé renvoie 401, ou la liste des modèles revient vide.
- Le modèle se connecte mais la sortie de raisonnement arrive déformée, ou des fichiers longs épuisent la fenêtre de contexte prématurément.

Les étapes 1 à 5 ci-dessous visent directement ces quatre points.

## Avant d'installer DeepSeek Harness : choisissez un modèle et une clé

Répondez d'abord : le harnais lui-même est gratuit et local, mais il ne fera rien d'utile tant que vous ne lui donnez pas une URL de base compatible OpenAI, une clé et au moins un ID de modèle. Décidez cela avant d'installer et l'ensemble de la configuration prend dix minutes.

Tout se passe dans un seul onglet de navigateur à `127.0.0.1:3080`. Vous choisissez entre trois routes, et les trois fonctionnent.

**Tableau B : options d'accès aux modèles qui fonctionnent avec DeepSeek Harness aujourd'hui**

| Route | URL de base | Prix par 1M tokens | Contexte | Paiement | Meilleur pour | 
|---|---|---|---|---|---|
| API officielle DeepSeek | https://api.deepseek.com | V4-Flash $0.22 en / $0.66 out hors-pic, $0.44 / $1.32 pic. Hits de cache à partir de $0.007 | 1M | Varie par région | Comportement first-party, hits de cache très bon marché | 
| Passerelle compatible OpenAI (exemple: Atlas Cloud) | https://api.atlascloud.ai/v1 | V4-Flash $0.14 en / $0.28 out. V4-Pro $1.68 / $3.38 | 1 048 576 | Carte, pas de minimum | Tarifs fixes sans surcharge aux heures de pointe | 
| Ollama local | http://localhost:11434/v1 | Aucun coût de tokens | Dépend du modèle | Aucun | Code privé, travail hors ligne | 

Deux choses à savoir avant de choisir. L'API first-party de DeepSeek est passée à une facturation par pic et hors-pic, où les heures de pointe sont 01:00-04:00 et 06:00-10:00 UTC et hors-pic est exactement la moitié du pic (Docs API DeepSeek, août 2026). Sa tarification des entrées par cache-hit est extrêmement basse, donc une charge de travail qui relit le même contexte de manière répétée peut être très bon marché en first-party.

Une passerelle échange cela contre la prévisibilité. Atlas Cloud sert les mêmes modèles DeepSeek à un tarif fixe sans surcharge de pointe et sans abonnement, ce qui est l'exemple que j'utiliserai dans la configuration ci-dessous car il ne nécessite aucun moyen de paiement régional. Remplacez par la route qui correspond à votre situation ; la forme de la configuration est identique pour toutes.

## Comment installer DeepSeek Harness, étape par étape

Il y a trois façons de procéder. Choisissez une ligne, puis suivez les étapes.

**Tableau A : méthodes d'installation comparées**

| Méthode | Temps | Nécessite | Mises à jour | Meilleur pour | 
|---|---|---|---|---|
| npx | Moins d'une min | Node 22.19+ ou 24+ | Ré-exécuter npx | Presque tout le monde | 
| Depuis source | 5 à 10 min | Node, pnpm, git | git pull et rebuild | Contributeurs, auteurs de plugins | 
| Build desktop | Environ 1 min | Rien | Réinstaller | Personnes évitant l'installation de Node | 

### Étape 1 : Vérifiez vos prérequis avant d'installer DeepSeek Harness

L'échec le plus courant est une version de Node qui semble assez récente mais ne l'est pas. Le référentiel nécessite `^22.19.0 || >=24.0.0`. Rien sur la série 23.x n'est qualifié à aucun niveau de patch.

bash```
1node -v      # doit être >= 22.19.0 sur 22.x, ou >= 24.0.0
2npm -v
3
```

Si `node -v` affiche 22.14 ou 20.x, mettez à niveau avant de continuer. Vous n'avez besoin de pnpm que si vous prévoyez de construire à partir des sources ou de créer des plugins :

bash```
1npm install -g pnpm
2
```

### Étape 2 : Installez DeepSeek Harness avec une seule commande

C'est l'installation complète. Elle télécharge et lance le profil web en une seule fois.

bash```
1npx @deepseek-ai/dsh web
2
```

Ouvrez `http://127.0.0.1:3080`. Si ce port est déjà occupé, le lanceur transmet tous les flags qu'il ne reconnaît pas directement au profil, donc vous pouvez le déplacer :

bash```
1npx @deepseek-ai/dsh web --port 8080
2
```

Ce que vous obtenez est une coquille vide. Pas de fournisseur, pas de modèle, pas de clé. C'est attendu, et c'est là que la plupart des guides s'arrêtent.

L'interface web de DeepSeek Harness à 127.0.0.1:3080 immédiatement après l'installation, sans fournisseur ni modèle configuré

*Installation terminée, et complètement inerte. Tout à partir d'ici est du câblage.*

### Étape 3 : Obtenez une clé API pour DeepSeek Harness

Quelle que soit la route que vous avez choisie dans le tableau B, vous avez besoin d'une clé et d'une URL de base. Pour la route first-party, inscrivez-vous sur `platform.deepseek.com` et créez une clé là-bas. Les options de facturation diffèrent selon la région, donc vérifiez que votre moyen de paiement est pris en charge avant de vous y engager.

Pour la route passerelle utilisée dans cette procédure, créez une clé dans le tableau de bord Atlas Cloud sous Clés API, puis exportez-la pour que le harnais puisse la lire sans la stocker dans un fichier de paramètres :

bash```
1export ATLASCLOUD_API_KEY="sk-votre-clé-ici"
2
```

La page Clés API du tableau de bord Atlas Cloud avec une clé nouvellement créée, partiellement masquée

*Copiez la clé une fois. Elle n'est plus affichée après avoir quitté la page.*

### Étape 4 : Ajoutez votre fournisseur de modèle à DeepSeek Harness

Dans l'interface, allez dans Paramètres, puis Modèles, puis **Ajouter un fournisseur personnalisé**. Le formulaire nécessite un ID de fournisseur, un nom d'affichage, une URL de base, un protocole API, une information d'identification et au moins un modèle.

Un avertissement que les docs répètent à juste titre : **l'ID de fournisseur est permanent.** Il est écrit dans les requêtes, les sessions sauvegardées, les valeurs par défaut des modèles et les références d'identification. Si vous ne l'aimez pas plus tard, votre seule option est de créer un nouveau fournisseur et de supprimer l'ancien.

| Champ | Ce qu'il faut entrer | 
|---|---|
| ID fournisseur | atlas (minuscules, permanent) | 
| Nom d'affichage | Atlas Cloud | 
| URL de base | https://api.atlascloud.ai/v1 | 
| Protocole API | openai-completions | 
| Clé API | Votre clé | 
| Modèles | Cliquez sur **Récupérer les modèles disponibles** , ou tapez deepseek-ai/deepseek-v4-flash manuellement | 

Si la récupération des modèles renvoie 401, la clé est erronée. Si elle renvoie une liste vide, le point de terminaison n'expose simplement pas d'index de modèles, ce qui est inoffensif : tapez l'ID du modèle manuellement et continuez.

Le formulaire Ajouter un fournisseur personnalisé rempli, avec l'URL de base, le protocole API et le contrôle Récupérer les modèles marqués

*Les trois champs qui décident si l'étape suivante fonctionne : l'URL de base, le protocole et la liste des modèles.*

### Étape 5 : Les trois valeurs par défaut de DeepSeek Harness qui cassent silencieusement les modèles DeepSeek

C'est la partie qu'aucun autre guide d'installation ne couvre, et c'est la raison pour laquelle votre configuration peut sembler connectée mais se comporter étrangement.

La couche LLM du harnais déduit quel dialecte de réflexion parler à partir de l'URL de votre point de terminaison. Les docs internes sont brutaux sur la conséquence : "pi-ai devine à partir de l'URL du point de terminaison ; l'URL d'une passerelle privée ne dit rien, donc un dialecte DeepSeek serait parlé dans le dialecte OpenAI sans aucun moyen de le corriger." En termes simples, toute URL de base qui n'est pas clairement DeepSeek est traitée comme OpenAI, et la gestion de `reasoning_content` de DeepSeek se dérègle.

Les deuxième et troisième pièges sont la capacité. Un modèle déclaré manuellement revient à `defaultContextWindow` de 262 144 et `defaultMaxTokens` de 32 768. V4 prend en charge 1 048 576 tokens de contexte, donc accepter la valeur par défaut jette les trois quarts de celle-ci.

Écrivez-les dans votre fichier de paramètres :

yaml```
1# $DSH_HOME/settings.yaml   (par défaut ~/.dsh/settings.yaml)
2llm-pi-ai:
3  providers:
4    atlas:
5      displayName: Atlas Cloud
6      api: openai-completions
7      baseURL: https://api.atlascloud.ai/v1
8      apiKeyEnv: ATLASCLOUD_API_KEY
9      compat:
10        thinkingFormat: deepseek   # 1. arrêter la supposition basée sur l'URL
11      defaultContextWindow: 1048576  # 2. la valeur par défaut n'est que 262144
12      defaultMaxTokens: 32768        # 3. augmenter pour les tâches à sortie longue
13      models:
14        - id: deepseek-ai/deepseek-v4-flash
15          contextWindow: 1048576
16        - id: deepseek-ai/deepseek-v4-pro
17          contextWindow: 1048576
18
```

Trois choses à garder à l'esprit. Les paramètres résolvent d'abord le modèle, puis la route, puis l'entrée de catalogue installée, puis la supposition dérivée de l'URL, donc une valeur par modèle l'emporte toujours. Les deux commutateurs `compat`, `thinkingFormat` et `supportsReasoningEffort`, existent **uniquement** sous `openai-completions` ; placez-les sur un autre protocole et la résolution échoue. Et cet adaptateur ne couvre délibérément pas Bedrock, Vertex, Azure ou Codex, dont les flux d'authentification nécessitent plus qu'une clé, un point de terminaison et des en-têtes.

### Étape 6 : Exécutez votre première vraie tâche dans DeepSeek Harness

Maintenant, la démo du début de cet article. Créez un dossier vide et ajoutez trois fichiers.

`fizzbuzz.py`, contenant un bug de décalage d'un :

python```
1def fizzbuzz(n):
2    out = []
3    for i in range(1, n):
4        if i % 15 == 0:
5            out.append("FizzBuzz")
6        elif i % 3 == 0:
7            out.append("Fizz")
8        elif i % 5 == 0:
9            out.append("Buzz")
10        else:
11            out.append(str(i))
12    return out
13
```

`test_fizzbuzz.py`, où deux des trois tests échouent :

python```
1from fizzbuzz import fizzbuzz
2
3def test_starts_correctly():
4    assert fizzbuzz(5)[:3] == ["1", "2", "Fizz"]
5
6def test_covers_every_number():
7    assert len(fizzbuzz(15)) == 15
8
9def test_fifteen_is_fizzbuzz():
10    assert fizzbuzz(15)[-1] == "FizzBuzz"
11
```

Et `requirements.txt` :

text```
1pytest>=8.0
2
```

Exécuter la suite manuellement d'abord vaut le coup, pour savoir dans quoi l'agent s'engage :

text```
1.FF                                                                      [100%]
2=================================== FAILURES ===================================
3___________________________ test_covers_every_number ___________________________
4E       AssertionError: assert 14 == 15
5___________________________ test_fifteen_is_fizzbuzz ___________________________
6E       AssertionError: assert '14' == 'FizzBuzz'
7=========================== short test summary info ============================
82 failed, 1 passed in 0.01s
9
```

Réglez le mode agent sur Standard et le modèle sur `deepseek-ai/deepseek-v4-flash`, puis collez cette invite exactement :

Exécutez pytest dans cet espace de travail. Deux tests échouent. Trouvez la cause racine dans fizzbuzz.py, corrigez-la avec le plus petit changement possible, puis ré-exécutez pytest et montrez-moi la sortie finale. Ne modifiez pas le fichier de test.


La correction correcte est un caractère : `range(1, n)` devient `range(1, n + 1)`, et la suite signale `3 passed`.

Maintenant, la partie qui fait la différence entre un harnais et une fenêtre de chat. Chaque exécution est enregistrée dans un journal de session en ajout seulement, et vous pouvez le forker. Revenez au point où l'agent a lu `fizzbuzz.py` pour la première fois et créez une branche pour une deuxième tentative :

Forkez cette session à partir de l'étape où vous avez lu fizzbuzz.py pour la première fois. Cette fois, réécrivez-le comme une recherche basée sur un dictionnaire au lieu de corriger la branche if, puis exécutez pytest à nouveau.


Forkage d'un journal de session DeepSeek Harness pour produire une deuxième implémentation basée sur un dictionnaire à partir du même point de départ

*Un point de départ, deux branches. C'est la fonctionnalité que la foule de la semaine de lancement a réellement appréciée.*

### Étape 7 : Passez en mode headless pour les scripts et l'intégration continue

Deux profils s'initialisent lors de la première utilisation : `web` et `headless`. Vous avez utilisé `web`, et `dsh web` n'est qu'un alias pour `dsh --profile web`.

Headless exécute une session fraîche persistée, imprime la réponse finale et se termine, ce qui est exactement la forme qu'un travail CI souhaite :

bash```
1dsh --profile headless "Exécutez les tests et corrigez les échecs. Rapportez le diff."
2
```

Tout autre profil doit être créé via `dsh plugin`. Les profils se trouvent dans `$DSH_HOME/profiles/<name>`, qui par défaut est `~/.dsh/profiles/<name>`.

Sortie de terminal d'une exécution headless de DeepSeek Harness imprimant la réponse finale et se terminant

*Mode headless : une session, une réponse, code de sortie sur lequel vous pouvez brancher.*

## Autres façons d'installer DeepSeek Harness

La route `npx` couvre la plupart des gens. Ces trois-là valent la peine d'être connues.

### Installer DeepSeek Harness depuis les sources avec pnpm

Si vous voulez lire le code, le corriger ou écrire des plugins contre lui :

bash```
1git clone https://github.com/deepseek-ai/deepseek-harness
2cd deepseek-harness
3pnpm install
4pnpm run build
5pnpm dsh web
6
```

### Le build desktop de 5 Mo

Un projet communautaire, `hairyf/deepseek-harness-desktop`, encapsule le harnais dans Tauri et livre un installateur d'environ 5 Mo pour Windows, macOS et Linux sans aucune configuration Node. Ce n'est pas une version officielle de DeepSeek, donc traitez-la en conséquence : au moment de la rédaction, elle compte environ 400 étoiles et a été créée un jour après le harnais lui-même.

### Exécuter DeepSeek Harness entièrement local avec Ollama

Ollama propose une intégration first-party. La version courte est une commande :

bash```
1ollama launch dsh
2
```

Cela installe et exécute le harnais avec Ollama câblé, en gardant ses paramètres à `~/.ollama/launch/dsh/settings.yaml` (Docs Ollama, août 2026). Une mise en garde à lire avant de supposer que tout est hors ligne : la recherche web intégrée est activée automatiquement et nécessite un accès cloud Ollama plus un modèle qui prend en charge les outils. Un modèle vraiment local vous donne un coût de token zéro, au prix de la vitesse et, généralement, d'une capacité d'appel d'outils plus faible.

DeepSeek Harness connecté à un serveur Ollama local exécutant la même tâche de test en échec

*Même tâche, pas de tour de réseau, pas de facture par token.*

## DeepSeek Harness est-il gratuit ? Ce que son exécution coûte réellement

Réponse d'abord : le logiciel est gratuit et sous licence MIT, y compris pour un usage commercial. Les tokens ne sont pas gratuits, sauf si vous exécutez des modèles localement. Rien dans le harnais lui-même n'est mesuré, limité ou lié à un compte DeepSeek.

**Tableau C : ce qui est réellement gratuit et ce qui ne l'est pas**

| Composant | Gratuit ? | Notes | 
|---|---|---|
| Le logiciel harnais | Oui | Licence MIT, aucun compte requis | 
| Tokens de modèle via une API | Non | Facturés par token par celui qui sert le modèle | 
| Tokens de modèle via Ollama local | Oui | Vous payez en matériel et latence à la place | 
| Contexte long | Dépend | Tarifé par token, donc une fenêtre de 1M coûte ce que vous y mettez | 

Un exemple concret, utilisant des chiffres ronds plutôt qu'une exécution spécifique. Disons qu'une session de débogage comme l'étape 6 consomme 200 000 tokens d'entrée et 20 000 tokens de sortie, ce qui est réaliste une fois que l'agent a lu quelques fichiers et itéré. Au taux fixe de la passerelle de $0.14 en et $0.28 out, cette session coûte environ 3,4 cents. Sur l'API first-party aux taux hors-pic avec cache-miss, c'est environ 5,7 cents, et pendant les heures de pointe environ 11 cents. Si la plupart de vos entrées sont des hits de cache, le first-party devient nettement moins cher côté entrée, car l'entrée par cache-hit commence à $0.007 pour 1M tokens.

Les leviers pratiques, par ordre d'impact : gardez les sessions courtes pour que le contexte ne s'accumule pas, effectuez le travail de routine sur Flash et réservez Pro pour les tâches vraiment difficiles, et utilisez le mode Minimal pour les exécutions de type benchmark car il donne au modèle exactement deux outils et aucune compaction de contexte.

## Avant de déployer : licence DeepSeek Harness et risques de prévisualisation

Trois choses courtes, et ensuite la FAQ.

La licence est MIT, donc l'utilisation commerciale est autorisée. Mais le projet se présente comme une prévisualisation développeur et indique, en majuscules, qu'il y aura des changements cassant la compatibilité. Épinglez une version et ne l'intégrez pas encore dans un pipeline de production.

Vos informations d'identification sont stockées en texte brut sur votre machine, dans `$DSH_HOME/.credentials.yaml` avec les paramètres à côté dans `$DSH_HOME/settings.yaml`, tous deux par défaut dans `~/.dsh`. Si votre répertoire home du harnais se retrouve jamais dans un référentiel, ajoutez-le à `.gitignore` :

text```
1.dsh/
2
```

Et l'évidence facile à oublier : votre code va au point de terminaison que vous avez configuré à l'étape 4. Lisez les conditions d'utilisation des données de ce fournisseur avant de pointer un agent vers une base de code privée.

## Foire aux questions

### DeepSeek Harness est-il gratuit ?

Le harnais est gratuit et sous licence MIT, sans compte ni abonnement requis, et vous pouvez l'utiliser commercialement. Les tokens de modèle sont facturés séparément par le fournisseur que vous connectez. Le pointer vers un modèle Ollama local vous donne une configuration vraiment sans coût de token, payée en matériel et en vitesse.

### Ai-je besoin d'une clé API DeepSeek pour installer DeepSeek Harness ?

Non. L'installation et une clé DeepSeek ne sont pas liées. `npx @deepseek-ai/dsh web` s'exécute sans aucune information d'identification. Vous n'avez besoin d'une clé que lorsque vous voulez que l'agent appelle réellement un modèle, et il peut s'agir d'une clé pour tout point de terminaison compatible OpenAI, y compris un serveur local.

### DeepSeek Harness peut-il exécuter d'autres modèles que DeepSeek ?

Oui. Les fournisseurs personnalisés acceptent `openai-completions`, et l'adaptateur couvre les protocoles qui peuvent être décrits avec une clé, un point de terminaison et des en-têtes. Ajoutez un deuxième fournisseur en ajoutant un autre bloc sous `providers:` dans `settings.yaml` avec son propre ID, URL de base et modèles. Bedrock, Vertex, Azure et Codex sont délibérément hors de portée car leur authentification nécessite plus que cela.

### Pourquoi mon fournisseur personnalisé renvoie-t-il 401 ou "modèle inconnu" ?

Un 401 signifie presque toujours que la clé est erronée ou n'est pas lue à partir de la variable d'environnement nommée dans `apiKeyEnv`. Un modèle inconnu signifie généralement que le point de terminaison n'expose pas d'index de modèles, donc rien n'a été récupéré : tapez l'ID du modèle manuellement à la place. Vérifiez également l'orthographe de l'ID du fournisseur, car il ne peut pas être renommé, seulement remplacé.

### Où DeepSeek Harness stocke-t-il ma clé API et mes paramètres ?

Les clés vont dans `$DSH_HOME/.credentials.yaml` et les paramètres de modèle écrits à la main dans `$DSH_HOME/settings.yaml`, tous deux sous `~/.dsh` sauf si vous remplacez `DSH_HOME`. Les sessions se trouvent dans `$DSH_HOME/storages` et les profils dans `$DSH_HOME/profiles/<name>`. Gardez tout le répertoire hors du contrôle de version.

### DeepSeek Harness est-il prêt pour la production ?

Pas encore, de son propre aveu. Le projet est livré comme une prévisualisation développeur et prévient explicitement des changements cassant la compatibilité, ce qui est une description juste d'un logiciel de quelques jours. Utilisez-le pour le développement local et l'assistance CI, épinglez la version que vous avez testée, et relisez la documentation des fournisseurs après les mises à niveau.

*Vérifié par rapport à la prévisualisation développeur de DeepSeek Harness le 17 août 2026. Ce projet livre des changements cassants par conception, donc si un nom de champ dans votre build semble différent du YAML ci-dessus, consultez la documentation officielle des fournisseurs avant de supposer que la configuration est erronée.*
