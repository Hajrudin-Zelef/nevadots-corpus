---
id: collect-261121-veille-ia/claude-anthropic/ollama-executer-un-llm-en-local-12-etapes-2026
title: "Ollama : Exécuter un LLM en Local, 12 Étapes [2026]"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-08-17"]
keywords: ["ollama", "local", "executer", "commande", "modele", "votre", "sans", "llama"]
source: https://tech-insider.org/fr/ollama-llm-local-12-etapes-2026/
source_anchor: ""
source_lines: [1, 532]
section: "Claude_Anthropic"
---
Exécuter un grand modèle de langage (LLM) sur votre propre machine, sans abonnement, sans clé d’API et sans envoyer la moindre donnée à un serveur américain : c’est exactement ce que permet **Ollama**. En quelques minutes, vous lancez Llama 3, DeepSeek-R1 ou Qwen 3 directement sur votre ordinateur. Ce tutoriel Ollama vous guide pas à pas, de l’installation jusqu’à un projet RAG complet, en 12 étapes et environ 30 minutes.

Au 17 août 2026, Ollama est devenu la passerelle de référence vers l’IA locale. Le projet, écrit en Go et publié sous licence MIT, dépasse les **175 000 étoiles sur GitHub**, revendique **8,9 millions de développeurs actifs mensuels** – dont **85 % des entreprises du Fortune 500**, selon TechFundingNews – et plus de **67 000 intégrations communautaires** selon les chiffres publiés début juillet 2026, et a levé **88 millions de dollars** au total, dont un tour de série B de **65 millions de dollars** mené par Theory Ventures le 9 juillet 2026, aux côtés de Benchmark, 8VC et Y Combinator. Cette base d’utilisateurs a doublé depuis janvier 2026, avec près d’un million de nouvelles installations chaque semaine mi-2026, et le projet a livré sa dernière version stable, la **0.34.1**, publiée le 14 septembre 2026 sur GitHub. Construit au-dessus du moteur d’inférence llama.cpp, il masque toute la complexité de la quantification et de l’accélération GPU derrière une seule commande. Le résultat : `ollama run llama3.2` suffit à dialoguer avec une IA.

Pour la France et l’Europe, l’enjeu dépasse la simple commodité. Exécuter un **LLM en local** signifie que vos prompts, vos documents et vos données clients ne quittent jamais votre poste. Aucun transfert hors UE, aucune dépendance à un fournisseur soumis au *Cloud Act* : c’est la voie la plus directe vers une IA conforme au RGPD et alignée sur la souveraineté numérique. Voici ce que vous allez apprendre.

- Installer Ollama sur macOS, Linux et Windows et vérifier l’installation ;
- Télécharger, lancer et gérer des modèles open source (Llama, DeepSeek, Qwen, Gemma, gpt-oss) ;
- Interroger l’API REST locale (port 11434) et l’API compatible OpenAI ;
- Intégrer Ollama en Python, créer un *Modelfile* et générer des sorties JSON structurées ;
- Activer l’appel d’outils (tool calling), les modèles de raisonnement et Ollama Cloud ;
- Construire un assistant RAG complet qui répond sur vos documents locaux.

## Qu’est-ce qu’Ollama et pourquoi exécuter un LLM en local en 2026 ?

Ollama est un logiciel libre qui télécharge, gère et sert des modèles de langage en local via une interface en ligne de commande et une API HTTP. Pensez-y comme au « Docker des LLM » : une commande `pull` pour récupérer un modèle, une commande `run` pour l’exécuter, et un serveur qui écoute en arrière-plan. Depuis la version **0.32.0 du 11 juillet 2026** – consolidée depuis par les versions **0.33.1 du 26 août 2026** (citée comme référence par PromptQuorum) et **0.34.0 du 5 septembre 2026** (repérée comme dernière version par Traceary et releases.sh) –, la simple commande `ollama` lance même un agent interactif capable de dialoguer, de coder et d’effectuer des recherches web, brouillant un peu plus la frontière avec les assistants cloud. Sous le capot, Ollama empaquette les poids du modèle, son *template* de prompt et ses paramètres dans un format unifié, puis délègue le calcul à llama.cpp.

Pourquoi se donner la peine d’exécuter un LLM en local alors que ChatGPT ou Claude sont accessibles en un clic ? Trois raisons dominent en 2026. D’abord la **confidentialité** : tout reste sur votre machine, ce qui élimine la fuite de données sensibles et simplifie la conformité RGPD. Ensuite le **coût** : après le téléchargement, l’inférence est gratuite et illimitée, sans facturation au token ni quota. Enfin l’**indépendance** : pas de coupure d’API, pas de modèle déprécié du jour au lendemain, et un fonctionnement même hors ligne.

Le moment est idéal car les modèles ouverts ont rattrapé une grande partie de l’écart avec les API propriétaires. DeepSeek-R1 (cumulant plus de 88 millions de téléchargements sur la bibliothèque Ollama), Qwen 3, Gemma 4 – dont l’appel d’outils profite du moteur MLX, passé en version stable dès la **0.30 le 13 mai 2026** avec un gain de vitesse d’environ **2x sur Apple Silicon** selon l’analyse d’Andrew.ooo, puis dont la gestion du cache a encore été affinée dans la version 0.32.1 du 16 juillet 2026 – ou encore `gpt-oss` – le modèle ouvert publié par OpenAI – offrent désormais du raisonnement, de la vision et l’appel d’outils. Le modèle Qwen 3.6 27B illustre ce rattrapage : il atteint 77,2 % sur le benchmark de codage SWE-bench tout en tenant dans 24 Go de VRAM en quantification Q4, ce qui en fait mi-2026 l’un des meilleurs modèles de code disponibles sur Ollama. Vous pouvez faire tourner un assistant de codage compétent sur un simple ordinateur portable récent.

| Critère | Ollama (LLM en local) | API cloud (ChatGPT / Claude) | 
|---|---|---|
| Coût d’inférence | Gratuit, illimité | Facturation au token | 
| Confidentialité des données | 100 % local, rien ne sort | Envoi vers serveurs externes | 
| Conformité RGPD | Native (aucun transfert) | Dépend du fournisseur | 
| Fonctionnement hors ligne | Oui | Non | 
| Niveau brut du meilleur modèle | Très bon (ouvert) | État de l’art | 
| Matériel requis | Le vôtre (RAM / GPU) | Aucun | 

Si vous hésitez encore sur le modèle à viser, notre comparatif Claude vs ChatGPT vs Gemini vs Mistral 2026 situe le niveau des références propriétaires que les modèles ouverts cherchent à égaler.

## Prérequis et configuration matérielle pour Ollama

La bonne nouvelle : Ollama fonctionne sur du matériel grand public. La règle d’or porte sur la mémoire. En quantification `Q4_K_M` (le format par défaut, qui réduit la taille des poids à environ 4 bits par paramètre), comptez grossièrement 1 Go de mémoire vive ou vidéo par milliard de paramètres, plus une marge pour le contexte. Un GPU n’est pas obligatoire – Ollama tourne sur CPU – mais il accélère la génération d’un facteur 5 à 20.

| Taille du modèle | RAM / VRAM recommandée | Exemple de modèle | Usage type | 
|---|---|---|---|
| 1 à 3 milliards | 4 à 8 Go | llama3.2:3b, gemma3:1b | Tâches simples, edge, mobile | 
| 7 à 8 milliards | 8 à 16 Go | llama3.1:8b, qwen3:8b | Assistant généraliste | 
| 13 à 14 milliards | 16 à 24 Go | phi4:14b, deepseek-r1:14b | Raisonnement, qualité accrue | 
| 30 à 34 milliards | 24 à 48 Go | qwen3:32b, gpt-oss:20b | Production exigeante | 
| 70 milliards et plus | 48 Go et plus | llama3.1:70b | Station de travail / serveur | 

Côté système, Ollama exige **macOS 12 (Monterey) ou plus récent**, **Windows 10/11** (64 bits) ou une distribution Linux récente (Ubuntu 22.04+ recommandé). L’accélération GPU couvre quatre familles : **NVIDIA CUDA** (la plus mature), **AMD ROCm**, **Apple Metal** (automatique sur les Mac Apple Silicon) et **Vulkan**, ce dernier ouvrant la voie aux iGPU et à un éventail plus large de cartes. Sur un Mac M-series, l’accélération est transparente : la mémoire unifiée sert à la fois de RAM et de VRAM.

Conseil pratique : commencez avec un modèle de 3 à 8 milliards de paramètres. Vous obtiendrez des réponses fluides même sur CPU, et vous monterez en gamme une fois familiarisé. Inutile d’investir dans une carte à 2 000 € avant d’avoir mesuré vos besoins réels.

## Étape 1 – Installer Ollama sur macOS, Linux et Windows

L’installation d’Ollama tient en une commande. Choisissez la ligne correspondant à votre système d’exploitation. Sur macOS, Homebrew est le plus propre ; sur Linux, le script officiel détecte automatiquement votre GPU et installe le service systemd ; sur Windows, le gestionnaire de paquets `winget` évite tout téléchargement manuel.

```
# macOS (via Homebrew)
brew install ollama
# Linux (script officiel – relisez-le avant exécution)
curl -fsSL https://ollama.com/install.sh | sh
# Windows (via winget, dans PowerShell)
winget install Ollama.Ollama
```
Sur Linux, le script crée un utilisateur système `ollama` et démarre un service en arrière-plan qui écoute sur le port 11434. Sur macOS et Windows, une application de barre des tâches – actuellement en version **3.7.4**, publiée le 18 août 2026 après la 3.7.0 du 27 juillet 2026 selon le changelog Ollaman – lance le serveur au démarrage. Vérifiez immédiatement que tout est en place avec la commande de version.

```
ollama --version
# Sortie attendue :
# ollama version is 0.30.11
```
Si vous préférez vérifier que le serveur répond, une simple requête HTTP confirme qu’Ollama écoute. Ce point sera central dès l’étape consacrée à l’API.

```
curl http://localhost:11434
# Réponse : Ollama is running
```
Sous Linux, si la commande renvoie une erreur de connexion, démarrez le service avec `sudo systemctl start ollama` puis activez son lancement automatique via `sudo systemctl enable ollama`. Vous pouvez télécharger directement les binaires depuis la page officielle de téléchargement si votre environnement bloque les scripts.

## Étape 2 – Télécharger et lancer votre premier modèle

Place à la pratique. La commande `ollama run` télécharge le modèle s’il est absent, le charge en mémoire puis ouvre une session de chat interactive. Pour un premier essai léger et rapide, Llama 3.2 (3 milliards de paramètres) est idéal : moins de 2,5 Go à télécharger et des réponses quasi instantanées même sans GPU.

```
ollama run llama3.2
# Le modèle se télécharge (barre de progression), puis :
# >>> Bonjour, peux-tu te présenter en une phrase ?
# Je suis un assistant IA exécuté localement via Ollama,
# prêt à répondre à vos questions sans connexion externe.
#
# >>> /bye   (pour quitter la session)
```
Dans la session interactive, plusieurs commandes spéciales commencent par une barre oblique : `/?` affiche l’aide, `/bye` quitte, `/clear` efface le contexte de la conversation, et `/show info` détaille le modèle chargé. Vous pouvez aussi coller un texte multi-lignes en l’entourant de triples guillemets.

Si vous voulez seulement télécharger un modèle sans l’exécuter – par exemple pour préparer plusieurs modèles à l’avance – utilisez `pull`. Précisez une étiquette (tag) après les deux-points pour choisir une taille précise.

```
ollama pull qwen3:8b          # Qwen 3, version 8 milliards
ollama pull deepseek-r1:8b    # Modèle de raisonnement
ollama pull gemma3:4b         # Google Gemma 3, compact
ollama pull nomic-embed-text  # Modèle d'embeddings (pour le RAG)
```
Les modèles sont stockés dans `~/.ollama/models` sur macOS et Linux, et dans le profil utilisateur sous Windows. Vous pouvez déplacer cet emplacement avec la variable d’environnement `OLLAMA_MODELS`, utile si votre disque système est petit.

## Étape 3 – Maîtriser les commandes CLI essentielles d’Ollama

La ligne de commande d’Ollama est volontairement minimaliste, y compris dans la version **0.33.2 du 27 août 2026** – la plus récente recensée par LocalAIMaster au moment d’écrire ces lignes. Une poignée de verbes couvre 95 % des usages quotidiens. Le tableau ci-dessous regroupe les commandes que vous utiliserez le plus, de la gestion des modèles à la supervision de la mémoire.

| Commande | Rôle | 
|---|---|
| `ollama run MODELE` | Lancer une session interactive (télécharge si besoin) | 
| `ollama pull MODELE` | Télécharger un modèle sans l’exécuter | 
| `ollama list` | Lister les modèles installés et leur taille | 
| `ollama ps` | Voir les modèles actuellement chargés en mémoire | 
| `ollama show MODELE` | Afficher paramètres, template et licence | 
| `ollama stop MODELE` | Décharger un modèle de la mémoire | 
| `ollama rm MODELE` | Supprimer un modèle du disque | 
| `ollama cp SRC DST` | Copier ou dériver un modèle existant | 
| `ollama serve` | Démarrer le serveur manuellement (premier plan) | 

Deux commandes méritent une attention particulière. `ollama ps` indique non seulement quels modèles sont chargés, mais aussi s’ils tournent sur CPU ou GPU et combien de mémoire ils consomment – précieux pour diagnostiquer une lenteur. `ollama show`, lui, révèle le *template* de prompt et la fenêtre de contexte par défaut, deux paramètres déterminants pour la qualité des réponses.

```
ollama list
# NAME              ID            SIZE      MODIFIED
# llama3.2:latest   a80c4f17acd5  2.0 GB    il y a 2 minutes
# qwen3:8b          500a1f067a9f  5.2 GB    il y a 5 minutes
ollama ps
# NAME              PROCESSOR     CONTEXT   UNTIL
# llama3.2:latest   100% GPU      4096      4 minutes restantes
```
Par défaut, Ollama décharge un modèle de la mémoire après cinq minutes d’inactivité pour libérer des ressources. Nous verrons à l’étape performances comment ajuster ce comportement avec `OLLAMA_KEEP_ALIVE`.

## Étape 4 – Choisir le bon modèle : Llama, DeepSeek, Qwen, Gemma, gpt-oss

La bibliothèque officielle d’Ollama propose des centaines de modèles. Inutile de tous les essayer : quelques familles couvrent la quasi-totalité des besoins. Le choix dépend de votre tâche (conversation, code, raisonnement, vision, embeddings) et de votre budget mémoire.

| Modèle | Éditeur | Point fort | Tag conseillé | 
|---|---|---|---|
| Llama 3.2 / 3.1 | Meta | Généraliste polyvalent | llama3.2 / llama3.1:8b | 
| DeepSeek-R1 | DeepSeek | Raisonnement étape par étape | deepseek-r1:8b | 
| Qwen 3 | Alibaba | Multilingue, appel d’outils | qwen3:8b | 
| Gemma 3 |  | Compact et efficace | gemma3:4b | 
| Phi-4 | Microsoft | Excellent rapport taille/qualité | phi4 | 
| gpt-oss | OpenAI | Modèle ouvert, agentique | gpt-oss:20b | 
| Qwen2.5-Coder | Alibaba | Génération de code | qwen2.5-coder | 
| LLaVA | Communauté | Vision (analyse d’images) | llava | 
| nomic-embed-text | Nomic | Embeddings pour RAG | nomic-embed-text | 

Quelques repères pour décider. Pour un **assistant généraliste** francophone, Llama 3.2 ou Qwen 3 en 8B offrent le meilleur équilibre : Llama 3.1, disponible en 8B, 70B et 405B avec une fenêtre de contexte de 128 000 tokens et l’appel d’outils (état d’août 2026), ne pèse qu’environ 4,7 Go en quantification Q4 pour sa version 8B et se contente de 8 à 16 Go de RAM, ce qui en fait mi-2026 l’un des modèles compacts les plus recommandés. Pour des tâches qui exigent de la **logique** (mathématiques, planification, débogage), un modèle de raisonnement comme DeepSeek-R1 « réfléchit » avant de répondre et gagne nettement en exactitude. Pour le **code**, Qwen2.5-Coder rivalise avec des assistants payants, et la nouvelle famille Qwen 3.6 (27B et 35B, avec un contexte étendu à 256 000 tokens) s’impose début août 2026 comme l’option la plus récente pour la génération de code. Et pour analyser des **images**, LLaVA accepte un chemin de fichier directement dans le prompt.

Notez le suffixe d’étiquette : `:8b`, `:14b`, `:latest`. Sans étiquette, Ollama télécharge la version par défaut, souvent un format intermédiaire. Précisez toujours la taille pour contrôler votre empreinte mémoire.

## Étape 5 – Interroger l’API REST d’Ollama (port 11434)

Au-delà du chat interactif, la vraie puissance d’Ollama réside dans son **API REST locale**, exposée par défaut sur `http://localhost:11434`. Toute application – script, service, outil no-code – peut y envoyer des requêtes HTTP. Deux points d’entrée dominent : `/api/generate` pour une complétion simple et `/api/chat` pour un dialogue avec historique.

```
curl http://localhost:11434/api/generate -d '{
  "model": "llama3.2",
  "prompt": "Pourquoi exécuter un LLM en local ? Réponds en une phrase.",
  "stream": false
}'
```
Le point d’entrée `/api/chat` accepte un tableau `messages` avec des rôles (`system`, `user`, `assistant`), exactement comme les API conversationnelles classiques. C’est celui à privilégier pour bâtir un assistant qui conserve le fil de la discussion.

```
curl http://localhost:11434/api/chat -d '{
  "model": "llama3.2",
  "messages": [
    { "role": "system", "content": "Tu réponds toujours en français." },
    { "role": "user", "content": "Explique le RGPD en une phrase." }
  ],
  "stream": false
}'
```
Le paramètre `stream` est essentiel. À `true` (valeur par défaut), Ollama renvoie une suite d’objets JSON token par token, idéal pour afficher la réponse au fil de l’eau. À `false`, vous recevez un unique objet complet, plus simple à parser dans un script. Les autres points d’entrée incluent `/api/tags` (lister les modèles), `/api/show` (détails d’un modèle), `/api/ps` (modèles chargés) et `/api/embed` (embeddings). La référence complète figure dans la documentation officielle de l’API.

## Étape 6 – Utiliser l’API compatible OpenAI

Voici la fonctionnalité qui change tout pour les développeurs : Ollama expose une **API compatible OpenAI** sous le préfixe `/v1`. Concrètement, n’importe quel code écrit pour la bibliothèque `openai` fonctionne avec Ollama en changeant uniquement l’URL de base. Vos applications existantes basculent vers l’IA locale sans réécriture.

```
from openai import OpenAI
client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama",  # valeur ignorée, mais requise par la bibliothèque
)
reponse = client.chat.completions.create(
    model="llama3.2",
    messages=[{"role": "user", "content": "Bonjour, qui es-tu ?"}],
)
print(reponse.choices[0].message.content)
```
Le point d’entrée `/v1/chat/completions` reproduit fidèlement le format d’OpenAI, y compris le *streaming* et les sorties JSON. Un point d’entrée `/v1/embeddings` existe également. Cette compatibilité explique pourquoi tant d’outils – LangChain, LlamaIndex, ou des extensions d’éditeur – fonctionnent immédiatement avec Ollama : ils croient parler à OpenAI.

L’astuce est précieuse pour migrer en douceur. Vous développez en local gratuitement, puis basculez vers une API distante en production en modifiant deux lignes. Pour aller plus loin dans la création d’agents, notre guide Créer un agent IA avec Mistral applique le même principe côté API.

## Étape 7 – Intégrer Ollama en Python

Pour un contrôle plus fin, Ollama publie une bibliothèque Python officielle (version 0.6.2) et une bibliothèque JavaScript (0.6.3). Installez la première avec pip ; elle encapsule l’API REST dans des fonctions idiomatiques.

`pip install ollama`
L’appel le plus courant est `ollama.chat()`. Il prend un nom de modèle et une liste de messages, et renvoie un dictionnaire dont le contenu se lit via `reponse["message"]["content"]`.

```
import ollama
reponse = ollama.chat(
    model="llama3.2",
    messages=[{"role": "user", "content": "Donne-moi 3 noms pour un chatbot RGPD."}],
)
print(reponse["message"]["content"])
```
Pour une expérience réactive, activez le *streaming* : la fonction devient un générateur que vous parcourez pour afficher chaque fragment dès qu’il arrive. C’est ce qui donne l’effet « machine à écrire » des interfaces de chat modernes.

```
import ollama
flux = ollama.chat(
    model="llama3.2",
    messages=[{"role": "user", "content": "Écris un haïku sur le code."}],
    stream=True,
)
for partie in flux:
    print(partie["message"]["content"], end="", flush=True)
print()
```
La bibliothèque expose aussi `ollama.generate()`, `ollama.embed()`, `ollama.list()` et `ollama.pull()`, ce qui permet de tout piloter depuis Python : télécharger un modèle, l’interroger, gérer la mémoire. Si vous débutez en Python pour le web, notre tutoriel Flask montre comment exposer ces appels derrière une petite API.

## Étape 8 – Créer un modèle personnalisé avec un Modelfile

Un **Modelfile** est à Ollama ce qu’un Dockerfile est à Docker : un fichier texte qui décrit comment construire un modèle dérivé. Vous partez d’un modèle de base, vous lui imposez une personnalité (via `SYSTEM`), vous réglez ses paramètres d’inférence, et vous obtenez un modèle réutilisable d’une seule commande.

Créez un fichier nommé `Modelfile` avec le contenu suivant. Ici, nous fabriquons « Léa », une assistante technique francophone à la température basse (réponses plus déterministes) et au contexte élargi.

```
FROM llama3.2
# Paramètres d'inférence
PARAMETER temperature 0.6
PARAMETER num_ctx 8192
PARAMETER top_p 0.9
# Personnalité de l'assistant
SYSTEM """
Tu es Léa, une assistante technique francophone.
Tu réponds toujours en français, de manière concise et précise.
Si tu n'es pas sûre, tu le dis plutôt que d'inventer.
"""
```
Construisez ensuite le modèle, puis exécutez-le comme n’importe quel autre. Léa conservera systématiquement sa consigne système, sans que vous ayez à la répéter à chaque requête.

```
ollama create lea -f Modelfile
# transferring model data... success
ollama run lea
# >>> Présente-toi.
# Bonjour, je suis Léa, votre assistante technique francophone.
```
Les directives les plus utiles d’un Modelfile sont `FROM` (modèle de base), `SYSTEM` (consigne permanente), `PARAMETER` (température, `num_ctx` pour la taille du contexte, `num_predict` pour la longueur maximale), `TEMPLATE` (format du prompt) et `ADAPTER` (pour appliquer un adaptateur LoRA finement entraîné). Vous pouvez ainsi packager un assistant métier – juridique, support, documentation interne – et le distribuer à toute une équipe.

## Étape 9 – Générer des sorties structurées en JSON

Les LLM sont bavards par nature, ce qui complique leur intégration dans du code. Depuis fin 2024, Ollama prend en charge les **sorties structurées** : vous fournissez un schéma JSON et le modèle est contraint de produire une réponse strictement conforme. Fini les expressions régulières fragiles pour extraire une donnée – vous recevez du JSON valide à coup sûr.

En Python, le plus élégant consiste à décrire la structure avec Pydantic, puis à passer son schéma au paramètre `format`. Le modèle remplit alors les champs attendus.

```
from pydantic import BaseModel
import ollama
class Produit(BaseModel):
    nom: str
    prix: float
    en_stock: bool
reponse = ollama.chat(
    model="llama3.2",
    messages=[{"role": "user",
               "content": "Décris un clavier mécanique à 89,99 euros, en stock."}],
    format=Produit.model_json_schema(),
)
produit = Produit.model_validate_json(reponse["message"]["content"])
print(produit)
# nom='Clavier mécanique' prix=89.99 en_stock=True
```
Cette technique transforme un LLM local en moteur d’extraction fiable : analyse de factures, classification de tickets, structuration de données issues de texte libre. Combinée à la confidentialité du traitement local, elle est particulièrement adaptée aux données personnelles soumises au RGPD, qui ne doivent pas transiter par un service tiers.

Via l’API REST, le même résultat s’obtient en passant le schéma JSON dans le champ `format` de la requête. Choisissez un modèle suffisamment capable (8B et plus) : les très petits modèles respectent moins fidèlement les schémas complexes.

## Étape 10 – Appel d’outils (tool calling) pour des agents IA

L’**appel d’outils** (ou *function calling*) permet à un modèle d’invoquer vos fonctions : consulter une base de données, appeler une API météo, lancer un calcul. Le modèle ne s’exécute pas lui-même la fonction ; il indique laquelle appeler et avec quels arguments, à charge pour votre code de l’exécuter et de renvoyer le résultat. C’est la brique de base de tout agent IA.

La bibliothèque Python d’Ollama simplifie l’opération : passez directement vos fonctions Python dans le paramètre `tools`, et la bibliothèque génère automatiquement leur schéma à partir des annotations de type et de la docstring.

```
import ollama
def meteo(ville: str) -> str:
    """Renvoie la météo actuelle d'une ville donnée."""
    donnees = {"Paris": "18 °C, nuageux", "Lyon": "21 °C, ensoleillé"}
    return donnees.get(ville, "Ville inconnue")
reponse = ollama.chat(
    model="qwen3",
    messages=[{"role": "user", "content": "Quel temps fait-il à Lyon ?"}],
    tools=[meteo],
)
for appel in reponse["message"].get("tool_calls", []):
    if appel["function"]["name"] == "meteo":
        resultat = meteo(**appel["function"]["arguments"])
        print("Résultat de l'outil :", resultat)
        # Résultat de l'outil : 21 °C, ensoleillé
```
En 2026, Ollama prend en charge l’appel d’outils **en flux continu** : l’application peut afficher le texte généré tout en déclenchant les appels de fonction en temps réel. Tous les modèles ne savent pas appeler des outils ; Qwen 3, Llama 3.1+ et gpt-oss font partie des plus fiables sur cette tâche. La documentation dédiée au tool calling détaille le format complet et la gestion des réponses d’outils.

## Étape 11 – Modèles de raisonnement (thinking) et Ollama Cloud

Les **modèles de raisonnement** comme DeepSeek-R1 produisent une phase de « réflexion » interne avant de formuler leur réponse, ce qui améliore nettement les tâches logiques. Ollama expose un paramètre `think` qui active ou désactive cette phase, et accepte même des niveaux d’intensité : `"low"`, `"medium"`, `"high"` ou `"max"`.

```
# En session interactive
ollama run deepseek-r1:8b
# Via l'API, avec contrôle du niveau de raisonnement
curl http://localhost:11434/api/chat -d '{
  "model": "deepseek-r1:8b",
  "messages": [{ "role": "user", "content": "Combien font 17 x 24 ?" }],
  "think": "medium",
  "stream": false
}'
```
Mais que faire si votre machine ne peut pas charger un modèle de 120 ou 671 milliards de paramètres ? C’est là qu’intervient **Ollama Cloud**. Introduit pour combler le fossé matériel, ce service exécute des modèles géants sur des serveurs distants tout en conservant exactement la même interface CLI et API. Vous gardez vos petits modèles en local et basculez ponctuellement vers le cloud pour les tâches lourdes.

```
# Connexion à votre compte Ollama Cloud
ollama signin
# Exécuter un modèle géant hébergé (aucun GPU local requis)
ollama run gpt-oss:120b-cloud
ollama run deepseek-v3.1:671b-cloud
ollama run qwen3-vl:235b-cloud
```
Le suffixe `-cloud` indique que l’inférence se déroule à distance. Attention toutefois : dès lors que vous utilisez Ollama Cloud, vos données quittent votre machine, et l’argument de confidentialité du « tout local » ne s’applique plus. Réservez le cloud aux contenus non sensibles, et conservez vos données personnelles ou confidentielles sur les modèles locaux.

## Étape 12 – Projet complet : un assistant RAG local sur vos documents

Assemblons tout dans un projet concret et utile : un assistant **RAG** (Retrieval-Augmented Generation) qui répond à vos questions à partir de vos propres documents, sans qu’aucun fichier ne quitte votre poste. Le principe : on découpe les documents en morceaux, on les transforme en vecteurs avec un modèle d’embeddings, puis pour chaque question on récupère les passages les plus pertinents et on les fournit au LLM comme contexte.

Prérequis : créez un dossier `documents/` contenant quelques fichiers `.txt` ou `.md`, et installez les dépendances. Nous utilisons `nomic-embed-text` pour les embeddings et `llama3.2` pour la génération.

```
pip install ollama numpy
ollama pull nomic-embed-text
ollama pull llama3.2
mkdir documents   # placez-y vos fichiers .txt / .md
```
Voici le script complet. Il indexe vos documents au démarrage, puis ouvre une boucle de questions-réponses. La recherche utilise une similarité cosinus sur des vecteurs maintenus en mémoire avec NumPy – pas besoin de base vectorielle externe pour un volume modeste.

```
import os
import numpy as np
import ollama
DOSSIER = "./documents"
MODELE_EMBED = "nomic-embed-text"
MODELE_CHAT = "llama3.2"
def charger_documents(dossier):
    morceaux = []
    for nom in os.listdir(dossier):
        if nom.endswith((".txt", ".md")):
            chemin = os.path.join(dossier, nom)
            with open(chemin, encoding="utf-8") as f:
                texte = f.read()
            # Découpage en morceaux d'environ 1000 caractères
            for i in range(0, len(texte), 800):
                morceaux.append(texte[i:i + 1000])
    return morceaux
def indexer(morceaux):
    vecteurs = []
    for m in morceaux:
        rep = ollama.embed(model=MODELE_EMBED, input=m)
        vecteurs.append(rep["embeddings"][0])
    return np.array(vecteurs)
def rechercher(question, morceaux, matrice, k=3):
    rep = ollama.embed(model=MODELE_EMBED, input=question)
    q = np.array(rep["embeddings"][0])
    scores = matrice @ q / (
        np.linalg.norm(matrice, axis=1) * np.linalg.norm(q) + 1e-10
    )
    indices = scores.argsort()[-k:][::-1]
    return [morceaux[i] for i in indices]
def repondre(question, contexte):
    invite = (
        "Réponds uniquement à partir du contexte ci-dessous. "
        "Si l'information est absente, dis-le clairement.\n\n"
        "Contexte :\n" + "\n---\n".join(contexte) +
        "\n\nQuestion : " + question
    )
    flux = ollama.chat(
        model=MODELE_CHAT,
        messages=[{"role": "user", "content": invite}],
        stream=True,
    )
    for partie in flux:
        print(partie["message"]["content"], end="", flush=True)
    print()
if __name__ == "__main__":
    print("Indexation des documents locaux...")
    morceaux = charger_documents(DOSSIER)
    matrice = indexer(morceaux)
    print(f"{len(morceaux)} morceaux indexés. Posez vos questions (Ctrl+C pour quitter).")
    while True:
        q = input("\nVous : ")
        contexte = rechercher(q, morceaux, matrice)
        print("Assistant : ", end="")
        repondre(q, contexte)
```
Lancez le script avec `python assistant.py`. Posez une question dont la réponse figure dans vos documents : l’assistant retrouve les passages pertinents et répond en s’appuyant dessus, en français. Vous disposez d’un moteur de recherche documentaire intelligent, 100 % local et conforme au RGPD par conception. Pour l’orchestrer dans des flux plus complexes, voyez notre tutoriel n8n auto-hébergé, qui sait appeler Ollama comme nœud d’IA.

## Optimiser les performances : GPU, quantification et contexte

Une fois Ollama opérationnel, plusieurs leviers améliorent la vitesse et la stabilité. Le premier est l’**accélération GPU** : Ollama la détecte automatiquement, mais vérifiez avec `ollama ps` que la colonne « PROCESSOR » indique bien « GPU ». Sur une machine multi-GPU NVIDIA, la variable `CUDA_VISIBLE_DEVICES` permet de cibler une carte précise.

Plusieurs variables d’environnement règlent finement le serveur, notamment `OLLAMA_NUM_PARALLEL`, dont l’intérêt a été chiffré par un banc d’essai SitePoint de mars 2026 : sur Llama 3.1 8B, le débit passe d’environ 62 tokens par seconde pour un utilisateur unique à 155 tokens par seconde cumulés avec 50 utilisateurs simultanés. Ces variables se définissent avant le lancement, ou via `systemctl edit ollama` sous Linux.

```
# Garder le modèle en mémoire 30 minutes (au lieu de 5 par défaut)
export OLLAMA_KEEP_ALIVE=30m
# Autoriser plusieurs requêtes simultanées
export OLLAMA_NUM_PARALLEL=4
# Exposer Ollama au réseau local (et non au seul localhost)
export OLLAMA_HOST=0.0.0.0:11434
# Cibler un GPU NVIDIA précis
export CUDA_VISIBLE_DEVICES=0
```
Le deuxième levier est la **quantification**. Elle compresse les poids du modèle, réduisant la mémoire et accélérant l’inférence au prix d’une légère perte de précision. Le tableau ci-dessous résume les compromis courants. `Q4_K_M` est le défaut recommandé : il offre le meilleur équilibre pour la plupart des usages.

| Format | Bits / poids | Taille relative | Qualité | 
|---|---|---|---|
| Q4_K_M | ~4,5 | La plus compacte (défaut) | Très bonne | 
| Q5_K_M | ~5,5 | Intermédiaire | Excellente | 
| Q6_K | ~6,5 | Plus lourde | Quasi optimale | 
| Q8_0 | ~8 | Lourde | Quasi sans perte | 
| FP16 | 16 | Maximale | Référence | 

Enfin, la **fenêtre de contexte** (`num_ctx`) influe directement sur la mémoire consommée. Augmentez-la pour traiter de longs documents, mais sachez qu’un contexte de 32 768 tokens consomme bien plus de VRAM qu’un contexte de 4 096. Ajustez selon votre matériel.

## Ollama vs llama.cpp vs LM Studio : quelle solution choisir ?

Ollama n’est pas seul sur le terrain de l’IA locale. Deux alternatives reviennent souvent pour un usage individuel : **llama.cpp**, le moteur d’inférence bas niveau sur lequel Ollama lui-même s’appuie, et **LM Studio**, une application de bureau avec interface graphique. Pour un déploiement en production à forte charge, un banc d’essai Red Hat d’août 2025 pointait déjà un écart net face à **vLLM** – 41 tokens par seconde de crête contre 793 pour vLLM, avec une latence p99 de 673 ms contre seulement 80 ms –, et les 79 résultats publics agrégés par OpenBenchmarking.org depuis janvier 2026 (dernière mise à jour le 22 mars 2026) confirment qu’Ollama reste avant tout optimisé pour l’usage local mono-utilisateur plutôt que pour le service à grande échelle. Le bon choix dépend donc de votre profil.

| Critère | Ollama | llama.cpp | LM Studio | 
|---|---|---|---|
| Interface | CLI + API | CLI / bibliothèque | Graphique (GUI) | 
| Courbe d’apprentissage | Faible | Élevée | Très faible | 
| Gestion des modèles | Automatique (pull) | Manuelle (GGUF) | Catalogue intégré | 
| API serveur | Oui (REST + OpenAI) | Oui (serveur léger) | Oui (OpenAI) | 
| Licence | MIT (libre) | MIT (libre) | Propriétaire (gratuit) | 
| Public visé | Développeurs, intégration | Experts, embarqué | Débutants, exploration | 

En résumé : choisissez **llama.cpp** si vous voulez le contrôle maximal, l’optimisation fine ou un déploiement embarqué – c’est le moteur, mais il demande de gérer manuellement les fichiers GGUF. Choisissez **LM Studio** si vous préférez cliquer plutôt que taper des commandes et explorer des modèles dans une interface. Choisissez **Ollama** si vous développez des applications : son API, sa gestion automatique des modèles et sa compatibilité OpenAI en font l’option la plus productive pour intégrer un LLM dans un projet.

## Pièges courants à éviter avec Ollama

Voici les erreurs les plus fréquentes que rencontrent les débutants, et comment les contourner avant qu’elles ne vous fassent perdre du temps.

- **Choisir un modèle trop gros pour sa mémoire.** Tenter de lancer un modèle 70B avec 16 Go de RAM provoque un débordement vers le disque (swap) et des réponses désespérément lentes. Respectez le tableau de configuration matérielle et commencez petit.
- **Oublier l’étiquette de taille.**`ollama run qwen3` télécharge la taille par défaut, qui peut être bien plus lourde que prévu. Précisez toujours`:8b` ,`:4b` , etc.
- **Croire qu’Ollama est forcément lent sur CPU.** Sans GPU, privilégiez les modèles 3B à 8B ; un 14B sur CPU sera frustrant. La vitesse dépend surtout de la taille du modèle, pas d’Ollama.
- **Confondre fenêtre de contexte et mémoire du chat.** Un`num_ctx` trop faible tronque silencieusement les longs documents : le modèle « oublie » le début. Augmentez-le pour le RAG, en surveillant la VRAM.
- **Exposer Ollama sans protection.** Définir`OLLAMA_HOST=0.0.0.0` ouvre le serveur à tout le réseau. Ne le faites que derrière un pare-feu ou un reverse proxy authentifié, jamais directement sur Internet.
- **Négliger le format des sorties.** Pour intégrer un LLM dans du code, n’analysez pas du texte libre : utilisez les sorties structurées (étape 9). Vous éviterez des heures de débogage de regex.

## Dépannage : 8 problèmes fréquents et leurs solutions

Même bien installé, Ollama peut présenter des comportements déroutants. Ce tableau couvre les incidents les plus courants signalés par la communauté et la marche à suivre.

| Symptôme | Cause probable | Solution | 
|---|---|---|
| « connection refused » sur le port 11434 | Serveur non démarré | `ollama serve` ou`systemctl start ollama` | 
| Port 11434 déjà utilisé | Conflit avec un autre service | Changer le port via `OLLAMA_HOST=127.0.0.1:11500` | 
| Génération très lente | Exécution sur CPU | Vérifier `ollama ps` ; installer les pilotes GPU | 
| « out of memory » au chargement | Modèle trop grand | Choisir une taille inférieure ou un format plus quantifié | 
| Le modèle « oublie » le contexte | `num_ctx` trop faible | Augmenter le contexte dans le Modelfile | 
| GPU non détecté (Linux) | Pilotes CUDA/ROCm absents | Installer les pilotes puis `systemctl restart ollama` | 
| Réponses incohérentes | Température trop élevée | Baisser `temperature` à 0,3–0,6 | 
| Disque saturé | Modèles accumulés | `ollama rm` ; déplacer`OLLAMA_MODELS` | 

En cas de doute persistant, les journaux du serveur sont votre meilleur allié. Sous Linux, consultez-les avec `journalctl -u ollama -f` ; sous macOS et Windows, l’application de barre des tâches propose un accès aux logs. La plupart des erreurs de chargement y sont explicitées en clair.

## Astuces avancées et bonnes pratiques

Vous maîtrisez les bases ? Voici comment passer au niveau supérieur et intégrer Ollama dans un véritable flux de travail.

- **Ajouter une interface web avec Open WebUI.** Cette interface open source, lançable via Docker, offre une expérience proche de ChatGPT (historique, multi-utilisateurs, gestion des modèles) en se connectant à votre serveur Ollama local.
- **Conteneuriser Ollama.** L’image officielle`ollama/ollama` facilite le déploiement reproductible. Pour les fondamentaux, suivez notre tutoriel Docker avant de monter un service Ollama isolé.
- **Précharger les modèles au démarrage.** Combinez`OLLAMA_KEEP_ALIVE=-1` et un appel initial pour garder un modèle résident en mémoire en permanence, éliminant la latence de premier chargement sur un serveur de production.
- **Utiliser des embeddings de qualité.** Pour un RAG en français, testez`nomic-embed-text` et`mxbai-embed-large` ; le second, plus lourd, capture souvent mieux les nuances sémantiques.
- **Surveiller la consommation.**`ollama ps` et`nvidia-smi` (sur GPU NVIDIA) permettent de suivre l’occupation mémoire en temps réel et d’anticiper les saturations.
- **Versionner vos Modelfiles.** Traitez vos assistants personnalisés comme du code : un dépôt Git de Modelfiles documente vos prompts système et vos paramètres, et rend l’équipe autonome.

Pour situer Ollama dans l’écosystème plus large des outils d’IA, notre tutoriel Claude Code illustre une approche complémentaire côté assistant de développement, tandis que l’analyse Mistral AI et la souveraineté européenne éclaire les enjeux stratégiques de l’IA ouverte en France.

### Related Coverage

## FAQ : questions fréquentes sur Ollama

### Ollama est-il vraiment gratuit ?

Oui. Ollama est un logiciel libre sous licence MIT, sans aucun coût de licence ni abonnement. Les modèles open source qu’il exécute sont également gratuits. Le seul coût est indirect : le matériel (RAM, GPU) et l’électricité. Seul Ollama Cloud, optionnel, peut introduire une facturation pour l’exécution distante de modèles géants.

### Quel matériel faut-il pour exécuter un LLM en local ?

Pour démarrer, 8 Go de RAM suffisent à faire tourner un modèle 3B à 8B en quantification Q4_K_M. Un GPU NVIDIA, AMD ou une puce Apple Silicon accélère fortement la génération. Pour des modèles de 30B et plus, prévoyez 24 Go de mémoire ou davantage. Aucun GPU n’est strictement obligatoire : Ollama fonctionne sur CPU, simplement plus lentement.

### Ollama est-il conforme au RGPD ?

Exécuté en local, Ollama traite toutes les données sur votre machine sans aucun transfert externe, ce qui en fait l’une des manières les plus simples de rester conforme au RGPD : il n’y a pas de transfert hors UE ni de sous-traitant à encadrer. Attention cependant à Ollama Cloud, qui envoie les données vers des serveurs distants et soulève à nouveau ces questions de conformité.

### Quelle est la différence entre Ollama et llama.cpp ?

llama.cpp est le moteur d’inférence bas niveau qui effectue réellement les calculs. Ollama s’appuie dessus et y ajoute une couche conviviale : téléchargement automatique des modèles, gestion de la mémoire, API REST et compatibilité OpenAI. En clair, llama.cpp est le moteur, Ollama est la voiture complète, plus simple à conduire au quotidien.

### Quel modèle choisir pour le français ?

Pour un usage francophone généraliste, Llama 3.2/3.1 et Qwen 3 offrent une excellente maîtrise du français en version 8B. Pour le raisonnement, DeepSeek-R1 brille ; pour le code, Qwen2.5-Coder. Vous pouvez renforcer la réponse en français en ajoutant une consigne système dédiée via un Modelfile, comme montré à l’étape 8.

### Comment exposer Ollama à d’autres machines du réseau ?

Définissez la variable `OLLAMA_HOST=0.0.0.0:11434` avant de démarrer le serveur, puis redémarrez Ollama. Les autres postes pourront alors l’interroger via l’adresse IP de la machine hôte. Par sécurité, placez-le derrière un pare-feu ou un reverse proxy authentifié, et ne l’exposez jamais directement sur Internet sans protection.

### Peut-on utiliser Ollama avec LangChain ou LlamaIndex ?

Oui, sans difficulté. Grâce à l’API compatible OpenAI et aux intégrations natives de ces frameworks, Ollama s’utilise comme fournisseur de modèles local pour construire des chaînes RAG, des agents ou des pipelines complexes. Il suffit de pointer le client vers `http://localhost:11434` ou son point d’entrée `/v1`.

### Quelle version d’Ollama utiliser en 2026 ?

Utilisez toujours la dernière version stable, la 0.32.6 alignée le 4 août 2026 au moment de la rédaction. Ollama évolue rapidement : la 0.32.0 (11 juillet 2026) a transformé la commande `ollama` en agent interactif capable de chat, de code et de recherche web, tandis que la 0.32.1 (16 juillet 2026) a amélioré l’appel d’outils de Gemma 4 et la gestion du cache MLX. Vérifiez votre version avec `ollama --version` et mettez à jour via votre gestionnaire de paquets ou le script d’installation.

## Conclusion : votre IA locale, souveraine et gratuite

En douze étapes, vous êtes passé de zéro à un assistant RAG complet tournant entièrement sur votre machine. Ollama a transformé l’exécution d’un LLM en local en une opération aussi simple qu’une commande, tout en offrant un écosystème riche : API REST et compatible OpenAI, Modelfiles, sorties structurées, appel d’outils, modèles de raisonnement et accès cloud optionnel.

Pour la France et l’Europe, cette approche coche toutes les cases : coût nul, confidentialité native, conformité RGPD et indépendance vis-à-vis des fournisseurs étrangers. À mesure que les modèles ouverts comme DeepSeek, Qwen et gpt-oss continuent de progresser, l’écart avec les API propriétaires se resserre. Le meilleur moment pour adopter l’IA locale, c’est maintenant – et avec ses 175 000 étoiles sur GitHub et une adoption qui atteint désormais 85 % des entreprises du Fortune 500 selon TechFundingNews, Ollama est la porte d’entrée la plus accessible.
