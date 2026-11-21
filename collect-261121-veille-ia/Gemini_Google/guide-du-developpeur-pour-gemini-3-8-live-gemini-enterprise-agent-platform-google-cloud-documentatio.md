---
id: collect-261121-veille-ia/gemini-google/guide-du-developpeur-pour-gemini-3-8-live-gemini-enterprise-agent-platform-google-cloud-documentatio
title: "Guide du développeur pour Gemini 3.8 Live | Gemini Enterprise Agent Platform | Google Cloud Documentation"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["live", "gemini", "audio", "video", "avatar", "langue", "modeles", "google"]
source: https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/guides/gemini-3-8-live
source_anchor: ""
source_lines: [1, 215]
section: "Gemini_Google"
---
**Gemini 3.8 Live** est un modèle conversationnel en temps réel conçu pour les interactions vocales et vidéo bidirectionnelles à faible latence et la synthèse d'avatar live en face à face. Conçu pour les workflows conversationnels continus, 3.8 Live fournit un dialogue affectif activé par défaut, un filtrage audio proactif, des outils de blocage à annulation automatique et un changement de langue dynamique.

Ce document traite des sujets suivants :

- Comment 3.8 Live s'intègre à la famille de modèles Gemini.
- Principales fonctionnalités de 3.8 Live.
- Intégration au SDK Google Gen AI.
- Règles et conventions d'API requises.
- Migration depuis des modèles d'API Gemini Live antérieurs.

## Rôle dans la famille Gemini

3.8 Live est conçu pour les **interactions de streaming en temps réel**. Il fonctionne via des WebSockets bidirectionnels, traitant la voix, les flux de caméra en direct et les diffusions d'écran avec une latence inférieure à la seconde, tout en générant des avatars audio 24 kHz et vidéo 24 FPS synchronisés.

### Spécifications et comparaisons des modèles

Le tableau suivant compare les spécifications de 3.8 Live et des modèles d'API Gemini Live antérieurs :

| Attribut | Gemini 3.8 Live | Audio natif de l'API Gemini 2.5 Flash Live (baseline GA) | 
|---|---|---|
| **ID du modèle** | `gemini-3.8-live` | `gemini-live-2.5-flash-native-audio` | 
| **Étape de lancement** | Disponibilité générale | Disponibilité générale | 
| **Modes d'entrée** | Audio (PCM 16 kHz), vidéo (JPEG 1 FPS), texte | Audio (PCM 16 kHz), vidéo (JPEG 1 FPS), texte | 
| **Modes de sortie** | Audio (PCM 24 kHz), vidéo (MP4 24 FPS pour l'avatar live), texte | Audio (PCM 24 kHz), texte | 
| **Synthèse d'avatar live** | Sortie vidéo synchronisée à 24 FPS | Non compatible | 
| **Dialogue affectif** | Activée par défaut | Nécessite une configuration explicite | 
| **Audio proactif** | Activée par défaut | Nécessite une configuration explicite | 
| **Exécution de l'appel de fonction** | Asynchrone non bloquant et bloquant avec annulation automatique ( `behavior="BLOCKING"` ) | Asynchrone et synchrone | 
| **Gestion des interruptions** | Rétrogradation de l'interruption polie (attend que l'utilisateur parle) | Interruption immédiate (peut chevaucher l'audio de l'utilisateur) | 
| **Langages acceptés** | Passage dynamique d'une langue à une autre en cours de diffusion | Langues disponibles | 
| **Pondération des domaines** | Compatible avec `custom_vocabulary` dans`AudioTranscriptionConfig` | Transcription de référence standard | 
| **Contrôle visuel des jetons** | `media_resolution` configurable (`LOW` ,`MEDIUM` ,`HIGH` ) | Budget de jetons fixe par image | 

### Quand utiliser Gemini 3.8 Live ?

Utilisez 3.8 Live dans les scénarios suivants :

- Vous avez besoin d'une faible latence de conversation, d'une prise de parole naturelle et d'une récupération rapide lorsque l'utilisateur interrompt l'agent (barge-in).
- Votre application appelle des outils externes et nécessite une exécution non bloquante pour que la sortie audio continue pendant que les systèmes de backend traitent les requêtes.
- Vous souhaitez afficher des avatars numériques parlants avec une animation faciale et une synchronisation labiale synchronisées à 24 FPS sans pipelines de rendu externes.
- Vos utilisateurs parlent plusieurs langues, ce qui nécessite une détection de la langue en cours de conversation et un passage d'une langue à l'autre parmi les langues acceptées.
- Vous avez besoin d'une pondération du vocabulaire spécifique à un domaine pour la terminologie technique, les codes SKU ou les noms de marque.

Pour connaître les spécifications complètes et les limites de quota, consultez la page du modèle 3.8 Live.

## Principales fonctionnalités de Gemini 3.8 Live

3.8 Live introduit les fonctionnalités suivantes :

- **Synthèse vidéo d'avatar live à 24 FPS** : Gemini 3.8 Live génère des flux vidéo MP4 synchronisés à 24 FPS (`response_modalities=["VIDEO"]` ) directement à partir de la sortie du modèle. Les expressions faciales et les mouvements des lèvres générés s'alignent sur la synthèse vocale en temps réel.Vous pouvez utiliser des avatars prédéfinis ou fournir des images de référence personnalisées pour créer des concierges numériques, des agents de service client, des tuteurs virtuels ou des personnages interactifs. Pour en savoir plus sur la configuration, consultez Configurer des avatars live.
- **Dialogue affectif et audio proactif activés par défaut** :
  - **Dialogue affectif** : activé par défaut dans 3.8 Live. Le modèle traite la prosodie acoustique, les indices vocaux, les pauses et l'inflexion de la voix dans l'entrée audio de l'utilisateur pour ajuster le ton de la voix et le rythme de la conversation.
  - **Audio proactif** : activé par défaut dans 3.8 Live. Le modèle filtre le bruit ambiant et les voix en arrière-plan, et ne génère des réponses audio que lorsque l'utilisateur s'adresse à lui.
- **Appel de fonction de blocage asynchrone et à annulation automatique** : 3.8 Live fournit des comportements d'appel d'outil conçus pour la reconnaissance vocale en temps réel :
  - **Exécution asynchrone non bloquante** : lorsque le modèle génère un appel d'outil, la session audio reste active afin que l'agent puisse diffuser des confirmations de conversation (*"Laissez-moi récupérer les détails de votre compte..."* ) et gérer les entrées de suivi pendant que votre application cliente appelle les API de backend.
  - **Appels bloquants à annulation automatique (`behavior="BLOCKING"`)** : lorsqu'une déclaration de fonction définit`behavior="BLOCKING"` , la génération est suspendue jusqu'à l'arrivée de la réponse de l'outil. Si une nouvelle entrée utilisateur arrive pendant l'exécution de l'appel bloquant, le serveur de l'API Gemini Live annule l'appel en attente.
  - **Gestion des interruptions polies** : dans les modèles précédents, le renvoi d'une réponse d'outil avec`scheduling="INTERRUPT"` pendant que l'utilisateur parlait chevauchait l'audio de l'utilisateur. Dans la version 3.8 Live, le serveur rétrograde la planification vers`WHEN_IDLE` lorsque l'utilisateur parle et lit la réponse une fois que l'utilisateur a fini de parler.
- **Changement de langue dynamique et vocabulaire personnalisé** :
  - **Compatibilité multilingue** : détecte les langues compatibles et passe de l'une à l'autre en cours de session sans redémarrer ni reconfigurer la connexion.
  - **Biais de vocabulaire personnalisé** : accepte les mots clés spécifiques à un domaine, les noms propres, les termes médicaux et les codes produit dans`AudioTranscriptionConfig` (via`input_audio_transcription` ) à l'aide de`custom_vocabulary` pour améliorer la précision de la transcription.
- **Ancrage visuel multimodal avec contrôle `media_resolution`** : accepte les flux vidéo en temps réel (images JPEG à 1 FPS, résolution recommandée`768x768` ) et vous permet de configurer`media_resolution` (`MEDIA_RESOLUTION_LOW` ,`MEDIA_RESOLUTION_MEDIUM` ,`MEDIA_RESOLUTION_HIGH` ) pour équilibrer l'utilisation des jetons par image et les détails visuels.

## Guide de démarrage rapide

Avant de commencer, authentifiez-vous auprès de Google Cloud à l'aide des identifiants par défaut de l'application (ADC).

Dans l'exemple de code suivant, remplacez `PROJECT_ID`

### Installer le SDK

Installez ou mettez à jour le SDK Google Gen AI et les dépendances audio :

```
pip install --upgrade google-genai websockets numpy
```
### Démarrer une session bidirectionnelle avec appel d'outil asynchrone

L'exemple suivant établit une session en temps réel avec `gemini-3.8-live`, sélectionne une voix prédéfinie (`Aoede`), configure la pondération du vocabulaire personnalisé et exécute des appels de fonction asynchrones non bloquants :

```
import asyncio
from google import genai
from google.genai import types
PROJECT_ID = "
```
`PROJECT_ID`"
LOCATION = "us-central1"
MODEL_ID = "gemini-3.8-live"
# Initialize the Gen AI client for enterprise.
client = genai.Client(enterprise=True, project=PROJECT_ID, location=LOCATION)
# Define a tool for asynchronous execution.
order_lookup_tool = types.FunctionDeclaration(
    name="lookup_order_status",
    description="Fetches real-time shipping status for a customer order ID.",
    parameters=types.Schema(
        type="OBJECT",
        properties={
            "order_id": types.Schema(
                type="STRING",
                description="The order ID, for example, ORD-8472",
            ),
        },
        required=["order_id"],
    ),
)
# Configure the live connection.
config = types.LiveConnectConfig(
    response_modalities=["AUDIO"],
    system_instruction=types.Content(
        parts=[
            types.Part.from_text(
                text=(
                    "You are an enterprise concierge. When you call a tool, "
                    "briefly acknowledge the request and tell the user what you're doing. "
                    "If a tool returns no results, inform the user before trying again. "
                    "Never issue more than two consecutive tool calls without speaking."
                )
            )
        ]
    ),
    speech_config=types.SpeechConfig(
        voice_config=types.VoiceConfig(
            prebuilt_voice_config=types.PrebuiltVoiceConfig(voice_name="Aoede")
        ),
    ),
    input_audio_transcription=types.AudioTranscriptionConfig(
        custom_vocabulary=["ORD-8472", "Gemini Live", "Express Shipping"]
    ),
    output_audio_transcription=types.AudioTranscriptionConfig(),
    tools=[types.Tool(function_declarations=[order_lookup_tool])],
)
async def handle_tool_call(session, function_call):
    """Executes backend calls asynchronously without blocking incoming audio."""
    order_id = function_call.args.get("order_id", "")
    await asyncio.sleep(2.0)  # Simulate backend API latency.
    if not order_id.startswith("ORD-"):
        result = {
            "status": "invalid_argument",
            "retryable": False,
            "message": f"Order ID '{order_id}' is invalid. Ask the user to verify.",
        }
    else:
        result = {
            "status": "ok",
            "retryable": False,
            "order_id": order_id,
            "shipping_status": "Out for delivery by 4:00 PM",
        }
    # Send tool response matching the call ID.
    await session.send_tool_response(
        function_responses=[
            types.FunctionResponse(
                id=function_call.id,
                name=function_call.name,
                response=result,
            )
        ]
    )
async def main():
    async with client.aio.live.connect(model=MODEL_ID, config=config) as session:
        print(f"Connected to {MODEL_ID}")
        # Send initial text or stream 16 kHz PCM audio chunks.
        await session.send_realtime_input(
            text="Hi! Can you check the status of order ORD-8472?"
        )
        async for message in session.receive():
            # Handle asynchronous tool calls.
            if message.tool_call:
                for call in message.tool_call.function_calls:
                    asyncio.create_task(handle_tool_call(session, call))
            # Process audio output chunks and transcriptions.
            if message.server_content:
                if (
                    message.server_content.output_transcription
                    and message.server_content.output_transcription.text
                ):
                    print(
                        f"Agent: {message.server_content.output_transcription.text}",
                        end="",
                        flush=True,
                    )
if __name__ == "__main__":
    asyncio.run(main())
## Règles et conventions d'API requises

Lorsque vous créez avec 3.8 Live, suivez ces conventions d'API :

- **Réponses de fonction informatives** : ne renvoyez pas un dictionnaire vide (`{}` ) ou`None` à partir d'un gestionnaire d'outils. Lorsqu'un outil renvoie une erreur ou une charge utile vide, 3.8 Live peut générer des appels de fonction répétés avec des arguments modifiés, sauf si la réponse inclut des champs d'état. Renvoyez des champs structurés dans chaque réponse de l'outil :
  - `status` : chaîne d'état lisible par machine, telle que`"ok"` ,`"no_results"` ou`"invalid_argument"` .
  - `retryable` : défini sur`false` lorsque la répétition de l'appel renvoie le même résultat.
  - `message` ou`guidance` : texte spécifiant l'état à signaler à l'utilisateur.
- **Limites de nouvelles tentatives d'outil au niveau du prompt** : incluez une limite de nouvelles tentatives dans votre`system_instruction` . Par exemple :*"Si un appel d'outil ne renvoie aucun résultat, informez l'utilisateur au lieu d'effectuer des recherches répétées avec des variations de paramètres.
N'exécutez jamais plus de deux appels d'outils consécutifs sans parler à l'utilisateur."*
- **Correspondance `FunctionResponse` stricte** :
  - Chaque tour `FunctionResponse` doit fournir le`id` exact du`FunctionCall` précédent.
  - Transmettez les objets de réponse directement dans `FunctionResponse.response` .
- Chaque tour 
- **Formats audio en streaming** :
  - Audio d'entrée : PCM mono 16 kHz, 16 bits, little-endian (`audio/pcm;rate=16000` ), diffusé par fragments de 20 ms à 100 ms.
  - Sortie audio : PCM mono 24 kHz.
- Audio d'entrée : PCM mono 16 kHz, 16 bits, little-endian (
- **Suppression des anciens paramètres** : ne transmettez pas`enable_affective_dialog` ni`proactivity` dans votre configuration. Ces fonctionnalités sont activées par défaut dans la version 3.8 Live.
- **Amorçage de l'historique des conversations** : si vous amorcez l'historique multitour à l'aide de`send_client_content` , attendez que l'application cliente reçoive le frame de serveur`setup_complete` et configurez`HistoryConfig(initial_history_in_client_content=True)` .

## Migrer vers Gemini 3.8 Live

Pour en savoir plus sur la migration de `gemini-live-2.5-flash-native-audio` vers `gemini-3.8-live`, consultez Migrer de l'API Gemini 2.5 Flash Live Audio natif vers Gemini 3.8 Live.

## Checklist pour les développeurs

Vérifiez les éléments suivants avant de déployer votre application :

## Autres ressources

Pour en savoir plus, consultez les ressources suivantes :
