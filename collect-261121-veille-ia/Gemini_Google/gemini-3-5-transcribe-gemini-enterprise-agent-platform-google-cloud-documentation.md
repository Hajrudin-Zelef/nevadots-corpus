---
id: collect-261121-veille-ia/gemini-google/gemini-3-5-transcribe-gemini-enterprise-agent-platform-google-cloud-documentation
title: "Gemini 3.5 Transcribe | Gemini Enterprise Agent Platform | Google Cloud Documentation"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["compatible", "gemini", "transcription", "transcribe", "audio", "codes", "detection", "temporels"]
source: https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/gemini/3-5-transcribe?utm_source=chatgpt.com
source_anchor: ""
source_lines: [1, 418]
section: "Gemini_Google"
---
Gemini 3.5 Transcribe est le modèle de Google permettant de convertir la voix en texte dans plusieurs langues. Il est disponible via Agent Platform. Basé sur les capacités de compréhension audio de Gemini, il fournit une transcription précise à faible latence avec détection de la langue basée sur les énoncés, identification du locuteur, codes temporels au niveau du mot, transcription intelligente et pondération vocale du vocabulaire personnalisé.

Gemini 3.5 Transcribe est le principal outil de transcription audio, qui comble l'écart entre les modèles multimodaux à raisonnement approfondi et les workflows de reconnaissance vocale hautement optimisés.

Pour commencer, consultez le notebook d'introduction à Gemini 3.5 Transcribe.

Il accepte deux méthodes de fonctionnement principales :

- **Transcription instantanée (en direct)** : diffuse l'audio et reçoit les résultats de la transcription de manière incrémentielle, en temps réel, à l'aide du modèle`gemini-3.5-transcribe-live` .
- **Transcription synchrone** : transcrit des fichiers audio préenregistrés complets en une seule requête à l'aide du modèle`gemini-3.5-transcribe` .

## Compatibilité des fonctionnalités et limites

Gemini 3.5 Transcribe est compatible avec les fonctionnalités suivantes sur ses deux points de terminaison :

| Fonctionnalité | Streaming en direct ( `gemini-3.5-transcribe-live` ) | Traitement des fichiers audio ( `gemini-3.5-transcribe` ) | Étape de lancement | Remarques/Limites | 
|---|---|---|---|---|
| **Détection automatique de la langue** | Compatible (plus de 85 langues) | Compatible (plus de 85 langues) | Compatible | Inclut le mélange de codes en cours de session. | 
| **Codes temporels au niveau de l'énoncé** | Compatible | Non compatible | Compatible |  | 
| **Codes temporels au niveau du mot** | Non compatible | Compatible | Expérimentation | Dégrade la précision de la transcription. | 
| **Biaisement du vocabulaire personnalisé** | Accepté (jusqu'à 1 000 termes) | Accepté (jusqu'à 1 000 termes) | Compatible | En général, les clients obtiennent de meilleurs résultats avec un maximum de 100 termes. | 
| **Dictée et mise en forme intelligentes** | Compatible (avec sélection manuelle du point de terminaison) | Compatible | Expérimentation | Inclut la suppression des mots de remplissage et la mise en forme alphanumérique basée sur l'intention. Pour le streaming en direct, utilisez-le avec la détection manuelle des points de terminaison. | 
| **Niveaux de sensibilité de la détection de fin de phrase** | Compatible | Non compatible | Compatible | Contrôle la sensibilité du début et de la fin de la parole pour la détection automatique de l'activité vocale (VAD). | 
| **Identification du locuteur** | Non compatible | Compatible (jusqu'à huit locuteurs) | Expérimentation | L'attribution pour trois intervenants ou plus est expérimentale. | 
| **Durée audio maximale** | Jusqu'à 10 minutes | 15 minutes maximum | Compatible | Le traitement des fichiers est limité à 15 minutes lorsque des fonctionnalités telles que la diarisation ou les codes temporels sont activées. | 
| **Édition vocale (`source_text`)** | Compatible (avec le mode `SMART` et la sélection manuelle du point de terminaison) | Compatible (avec le mode `SMART` ) | Expérimentation | Activé par défaut lorsque `mode="SMART"` . Accepte jusqu'à 64 Ko de`source_text` existant à modifier, réécrire ou reformater à l'aide d'instructions vocales. Incompatible avec les codes temporels au niveau du mot et l'identification du locuteur. | 

## Modes de transcription

Gemini 3.5 Transcribe accepte deux modes de transcription via le paramètre `mode` dans `AudioTranscriptionConfig` :

- **`VERBATIM` (par défaut)** : renvoie une transcription exacte mot à mot de tout ce qui est dit, en conservant les mots de remplissage bruts ("euh", "hum", "genre", "tu sais"), les répétitions, les pauses et les faux départs.
- **`SMART` (Transcription intelligente)** : optimise la transcription pour la lecture en appliquant un post-traitement intelligent :
  - **Suppression des disfluences** : supprime les mots de remplissage, les bégaiements et les faux départs.
  - **Autocorrections intégrées** : résout directement les corrections vocales (par exemple,*"On se voit mardi, en fait non, mercredi à deux heures"* devient*"On se voit mercredi à 14h"* ).
  - **Mise en forme structurée automatique** : structure automatiquement les pensées exprimées à l'oral en paragraphes, listes numérotées, listes à puces, dates, devises et nombres mis en forme.
  - **Correction grammaticale** : applique une ponctuation, une casse et un flux naturels.

| Audio parlé | Résultat de la fonction `VERBATIM` | Sortie `SMART` (Transcription intelligente) | 
|---|---|---|
| "Euh, pour la réunion, je pense qu'on devrait, euh, inviter Alice et, non, Bob et Carol." | "Euh, pour la réunion, je pense qu'on devrait inviter Alice et, non, Bob et Carol." | "Pour la réunion, je pense que nous devrions inviter Bob et Carol." | 
| "Premier élément : examiner le budget. Deuxième élément : finaliser le calendrier. Troisième élément : envoyer le récapitulatif." | "premier élément examiner le budget deuxième élément finaliser le calendrier troisième élément envoyer le récapitulatif" | "1. Vérifiez le budget 2. Finaliser le calendrier 3. Envoyer le récapitulatif" | 

## Transcription de la diffusion en direct

La `BidiGenerateContent (Live) API` reste ouverte pendant que vous diffusez de petits fragments audio vers le modèle et recevez les résultats de la transcription de manière incrémentielle, dès qu'ils sont disponibles. Cette option est utilisée pour le sous-titrage ou la transcription en temps quasi réel de l'entrée du micro.

Pour essayer la transcription en streaming sans écrire de code, ouvrez `gemini-3.5-transcribe-live` sur la page de l'API Gemini Live d'Agent Studio, puis enregistrez ou importez du contenu audio et regardez la transcription en streaming.

Pour transcrire un flux audio, créez un `LiveConnectConfig` et définissez `response_modalities` sur `["TEXT"]`, en plus de votre configuration `input_audio_transcription`.

```
import asyncio
from google import genai
from google.genai import types
client = genai.Client(enterprise=True, project=PROJECT_ID, location=LOCATION)
config = types.LiveConnectConfig(
    response_modalities=["TEXT"],
    input_audio_transcription=types.AudioTranscriptionConfig(
        language_codes=["it-IT", "en-US"],
    ),
)
async def streaming_main(audio_file, config):
    # Connect to the Live API session
    async with client.aio.live.connect(model="gemini-3.5-transcribe-live", config=config) as session:
        # In a complete implementation, you would chunk the audio and send via:
        # await session.send_realtime_input(audio=types.Blob(data=data, mime_type="audio/pcm;rate=16000"))
        # await session.send_realtime_input(audio_stream_end=True)
        async for message in session.receive():
            if message.server_content:
                server_content = message.server_content
                # Track the active interim segment
                interim = server_content.interim_input_transcription
                if interim and interim.text:
                    print(f"Interim: {interim.text}")
                # Save final transcript and clear the interim
                final = server_content.input_transcription
                if final and final.text:
                    print(f"Final: {final.text}")
```
### Ajuster la sensibilité de la détection de fin de phrase

Agent Platform vous permet de contrôler le compromis entre latence et précision pour les applications de streaming et en temps réel avec `gemini-3.5-transcribe-live`. Par défaut, le modèle de reconnaissance utilise la détection automatique de l'activité vocale (VAD) et attend un bref moment de silence après avoir détecté la parole avant de valider le segment `input_transcription` final. Cela permet de s'assurer qu'un utilisateur a terminé une phrase complète, ce qui maximise la précision au détriment d'un léger délai dans la réponse finale.

Vous pouvez ajuster `start_of_speech_sensitivity`, `end_of_speech_sensitivity`, `prefix_padding_ms` et `silence_duration_ms` dans `AutomaticActivityDetection` pour les applications urgentes, telles que les commandes vocales ou les bots vocaux, afin de détecter le début de la parole, de filtrer le bruit de fond ou de finaliser les transcriptions plus rapidement.
(Si vous avez besoin de limites de tour de parole contrôlées par le client au lieu de la détection automatique de l'activité vocale, utilisez la détection manuelle des points de terminaison.) Les niveaux de sensibilité ne s'appliquent qu'à la transcription de streaming en direct (`gemini-3.5-transcribe-live`) lorsque vous utilisez la détection d'activité vocale automatique. Ils ne sont pas compatibles avec le traitement synchrone des fichiers audio (`gemini-3.5-transcribe`).

### Niveaux de sensibilité

Lorsque vous utilisez la DAV automatique dans `RealtimeInputConfig`, vous pouvez configurer les paramètres suivants dans `AutomaticActivityDetection` en fonction de votre cas d'utilisation :

- **Sensibilité du début du contenu vocal (`start_of_speech_sensitivity`)** : contrôle la facilité avec laquelle le modèle détecte le début du contenu vocal.
  - `START_SENSITIVITY_UNSPECIFIED` : utilise la sensibilité de début de parole par défaut.
  - `START_SENSITIVITY_HIGH` : détecte plus souvent le début de la parole.
Optimisé pour capturer des paroles à voix basse ou des énoncés brefs, tels que des commandes d'un seul mot, le plus rapidement possible. Ce paramètre peut augmenter les faux déclenchements dans les environnements bruyants.
  - `START_SENSITIVITY_LOW` : détecte moins souvent le début de la parole.
Optimisé pour les environnements bruyants afin de réduire les déclenchements erronés dus aux bruits de fond.
- **Sensibilité de la fin du contenu vocal (`end_of_speech_sensitivity`)** : contrôle la rapidité avec laquelle le modèle met fin au contenu vocal détecté et valide la transcription finale.
  - `END_SENSITIVITY_UNSPECIFIED` : utilise la sensibilité de fin de parole par défaut.
  - `END_SENSITIVITY_HIGH` : met fin plus souvent à la parole détectée. Optimisé pour les énoncés courts, tels que des phrases simples ou des commandes comme "Rappelle-moi d'appeler le dentiste demain". Ce paramètre réduit le temps d'attente après l'arrêt de la parole, ce qui permet d'obtenir une réponse plus rapide tout en maintenant une précision raisonnable au niveau de la phrase.
  - `END_SENSITIVITY_LOW` : met fin moins souvent à la parole détectée. Optimisé pour la dictée de longs textes et les locuteurs qui font des pauses plus longues entre les phrases, ce qui permet d'éviter les coupures prématurées des énoncés au détriment d'une latence plus élevée.
- **Remplissage du préfixe (`prefix_padding_ms`)** : durée requise de la parole détectée (en millisecondes) avant que le modèle ne valide le début de la parole. Les valeurs inférieures capturent plus rapidement les énoncés brefs, tandis que les valeurs supérieures permettent de filtrer les pics de bruit courts tels que les clics ou les bruits secs.
- **Durée du silence (`silence_duration_ms`)** : durée requise de silence continu (en millisecondes) avant que le modèle ne valide la fin de la parole.
Des valeurs plus faibles permettent de finaliser les transcriptions plus rapidement après l'arrêt de la parole, tandis que des valeurs plus élevées permettent des pauses naturelles plus longues au milieu d'une phrase sans diviser l'énoncé.

```
import asyncio
from google import genai
from google.genai import types
client = genai.Client(enterprise=True, project=PROJECT_ID, location=LOCATION)
config = types.LiveConnectConfig(
    response_modalities=["TEXT"],
    realtime_input_config=types.RealtimeInputConfig(
        automatic_activity_detection=types.AutomaticActivityDetection(
            start_of_speech_sensitivity=types.StartSensitivity.START_SENSITIVITY_LOW,
            end_of_speech_sensitivity=types.EndSensitivity.END_SENSITIVITY_HIGH,
            prefix_padding_ms=200,
            silence_duration_ms=500,
        ),
    ),
    input_audio_transcription=types.AudioTranscriptionConfig(
        language_codes=["en-US"],
    ),
)
async def streaming_with_sensitivity(audio_file, config):
    # Connect to the Live API session
    async with client.aio.live.connect(
        model="gemini-3.5-transcribe-live", config=config
    ) as session:
        # In a complete implementation, you would chunk the audio and send via:
        # await session.send_realtime_input(
        #     audio=types.Blob(data=data, mime_type="audio/pcm;rate=16000")
        # )
        # await session.send_realtime_input(audio_stream_end=True)
        async for message in session.receive():
            if message.server_content:
                server_content = message.server_content
                # Track the active interim segment
                interim = server_content.interim_input_transcription
                if interim and interim.text:
                    print(f"Interim: {interim.text}")
                # Save final transcript and clear the interim
                final = server_content.input_transcription
                if final and final.text:
                    print(f"Final: {final.text}")
```
### Transcription instantanée avec le mode `SMART` et la détection manuelle des points de terminaison

Lorsque vous utilisez le mode `SMART` avec la transcription de streaming en direct (`gemini-3.5-transcribe-live`), vous devez utiliser la **détection manuelle des points de terminaison** (détection manuelle de l'activité vocale) au lieu de la détection automatique de l'activité vocale. Étant donné que le mode `SMART` applique un post-traitement au niveau de l'énoncé (par exemple, en résolvant les autocorrections intégrées et en structurant les listes ou les paragraphes), la DAV automatique peut diviser prématurément la pensée d'un utilisateur lors de pauses naturelles.

Pour configurer la détection manuelle des points de terminaison, désactivez `automatic_activity_detection` dans `RealtimeInputConfig` et marquez explicitement le début et la fin de la prise de parole de l'utilisateur à l'aide de `activity_start` et `activity_end` :

```
config = types.LiveConnectConfig(
    response_modalities=["TEXT"],
    realtime_input_config=types.RealtimeInputConfig(
        automatic_activity_detection=types.AutomaticActivityDetection(
            disabled=True,
        ),
    ),
    input_audio_transcription=types.AudioTranscriptionConfig(
        mode="SMART",
    ),
)
async with client.aio.live.connect(
    model="gemini-3.5-transcribe-live", config=config
) as session:
    # Signal start of speech turn
    await session.send_realtime_input(activity_start=types.ActivityStart())
    # Stream audio chunks...
    await session.send_realtime_input(
        audio=types.Blob(data=audio_bytes, mime_type="audio/pcm;rate=16000")
    )
    # Signal end of speech turn so SMART mode can process the complete utterance
    await session.send_realtime_input(activity_end=types.ActivityEnd())
```
### Édition vocale en direct

Lorsque vous utilisez `gemini-3.5-transcribe-live` avec `mode="SMART"` et la détection manuelle de fin de phrase (`automatic_activity_detection.disabled=True`), l'édition vocale est active à chaque tour de parole de la session :

- **Démarrage vide** : omettez`source_text` dans le champ`LiveConnectConfig` pour dicter à partir d'un état vide. Les corrections vocales ou les commandes de mise en forme ultérieures modifient automatiquement la transcription de la session accumulée.
- **Démarrage amorcé** : transmettez le`source_text` initial (jusqu'à 64 Ko) dans`input_audio_transcription` lors de la configuration de la session afin que la première prise de parole modifie immédiatement le texte préexistant.
- **Progression avec état** : contrairement aux requêtes synchrones, la session Live conserve le dernier`input_transcription` finalisé comme contexte de texte actif pour le tour suivant. Vous n'avez pas besoin de renvoyer`source_text` entre les tours. Si l'utilisateur modifie manuellement le tampon de texte à l'aide d'un clavier entre les tours de parole, fermez la session en cours et ouvrez une nouvelle session amorcée avec le tampon mis à jour dans`source_text` .

```
import asyncio
from google import genai
from google.genai import types
client = genai.Client(enterprise=True, project=PROJECT_ID, location=LOCATION)
config = types.LiveConnectConfig(
    response_modalities=["TEXT"],
    realtime_input_config=types.RealtimeInputConfig(
        automatic_activity_detection=types.AutomaticActivityDetection(
            disabled=True,
        ),
    ),
    input_audio_transcription=types.AudioTranscriptionConfig(
        mode="SMART",
        source_text="Draft agenda: 1. Q3 metrics 2. Hiring plan",
    ),
)
async def live_voice_edit(audio_bytes: bytes):
    async with client.aio.live.connect(
        model="gemini-3.5-transcribe-live",
        config=config,
    ) as session:
        # Signal start of spoken edit command
        await session.send_realtime_input(activity_start=types.ActivityStart())
        # Stream 16kHz mono PCM audio chunks
        await session.send_realtime_input(
            audio=types.Blob(data=audio_bytes, mime_type="audio/pcm;rate=16000")
        )
        # Signal end of utterance so SMART mode finalizes the edited text
        await session.send_realtime_input(activity_end=types.ActivityEnd())
        async for message in session.receive():
            if message.server_content and message.server_content.input_transcription:
                # Contains the full updated text after applying the voice edit
                print(f"Updated text: {message.server_content.input_transcription.text}")
```
## Transcription synchrone

Vous pouvez utiliser la méthode `generate_content` standard pour transcrire des fichiers audio complets qui ont déjà été enregistrés. Configurez les paramètres en créant un `AudioTranscriptionConfig` dans `GenerateContentConfig`.

### Codes temporels au niveau du mot

La définition de `word_timestamp=True` renvoie le code temporel au niveau du mot. La liste `audio_transcription.words` de la réponse contient chaque mot reconnu, ainsi que ses valeurs `start_offset` et `end_offset`.

```
from google import genai
from google.genai import types
# Initialize the client
client = genai.Client(enterprise=True, project=PROJECT_ID, location=LOCATION)
with open("input.wav", "rb") as f:
    audio_bytes = f.read()
response = client.models.generate_content(
    model="gemini-3.5-transcribe",
    contents=[
        types.Part.from_bytes(
            data=audio_bytes,
            mime_type="audio/wav",
        ),
    ],
    config=types.GenerateContentConfig(
        audio_transcription_config=types.AudioTranscriptionConfig(
            word_timestamp=True,
        ),
    ),
)
parts = getattr(response, "parts", []) or []
if parts and (audio_tx := getattr(parts[0], "audio_transcription", None)):
    for w in getattr(audio_tx, "words", []) or []:
        print(f"[{w.start_offset} - {w.end_offset}] {w.word}")
if text := "".join(p.text for p in parts if getattr(p, "text", None)):
    print(f"**{text}**")
```
### Identification du locuteur et vocabulaire personnalisé

La définition de `diarization=True` demande au modèle d'identifier et d'étiqueter les différents locuteurs. Vous pouvez également fournir un champ `custom_vocabulary` avec une liste d'expressions qui orientent `Gemini 3.5 Transcribe` vers la reconnaissance de termes spécifiques. Le modèle suit généralement les instructions de vocabulaire personnalisé de manière plus fiable lorsqu'une langue est également spécifiée à l'aide de `language_codes`.

```
response = client.models.generate_content(
    model="gemini-3.5-transcribe",
    contents=[
        types.Part.from_uri(
            file_uri="gs://cloud-samples-data/generative-ai/audio/coffee_order.wav",
            mime_type="audio/wav",
        ),
    ],
    config=types.GenerateContentConfig(
        audio_transcription_config=types.AudioTranscriptionConfig(
            diarization=True,
            language_codes=["en-US"],
            custom_vocabulary=["oatmilk", "oz"],
        ),
    ),
)
parts = getattr(response, "parts", []) or []
for p in parts:
    audio_tx = getattr(p, "audio_transcription", None)
    speaker = getattr(audio_tx, "speaker_label", "UNKNOWN") if audio_tx else "UNKNOWN"
    text = getattr(p, "text", "") or (getattr(audio_tx, "text", "") if audio_tx else "")
    if text:
        print(f"**{speaker}**: {text}")
```
### Transcription synchrone avec le mode `SMART`

```
response = client.models.generate_content(
    model="gemini-3.5-transcribe",
    contents=[
        types.Part.from_bytes(
            data=audio_bytes,
            mime_type="audio/wav",
        ),
    ],
    config=types.GenerateContentConfig(
        audio_transcription_config=types.AudioTranscriptionConfig(
            mode="SMART",
        ),
    ),
)
```
### Édition vocale synchrone

L'édition vocale est activée par défaut chaque fois que `mode="SMART"` est défini. En plus de nettoyer les disfluences et les autocorrections intégrées lors d'une nouvelle dictée, vous pouvez transmettre du texte existant dans `source_text` (jusqu'à 64 Ko) pour que le modèle applique des instructions d'édition, de réécriture ou de mise en forme directement à ce brouillon.

Au lieu de renvoyer un diff, `gemini-3.5-transcribe` renvoie le texte complet mis à jour dans `response.text`. Étant donné que les requêtes `generate_content` sont sans état, implémentez l'édition vocale multitour en transmettant la sortie du tour précédent en tant que `source_text` lors de la requête suivante :

```
from google import genai
from google.genai import types
client = genai.Client(enterprise=True, project=PROJECT_ID, location=LOCATION)
current_text = "The project sync is scheduled for Tuesday at 2 PM in Room 4B."
with open("edit_instruction.wav", "rb") as f:
    audio_bytes = f.read()
# Example spoken audio: "Change Tuesday to Wednesday and make it sound more formal."
response = client.models.generate_content(
    model="gemini-3.5-transcribe",
    contents=[
        types.Part.from_bytes(
            data=audio_bytes,
            mime_type="audio/wav",
        ),
    ],
    config=types.GenerateContentConfig(
        audio_transcription_config=types.AudioTranscriptionConfig(
            mode="SMART",
            source_text=current_text,
        ),
    ),
)
updated_text = response.text
print(updated_text)
```
## Langues acceptées

Les langues et codes de langue BCP-47 suivants sont compatibles avec Gemini 3.5 Transcribe :

| Langue | Code BCP-47 | Aptitude | Langue | Code BCP-47 | Aptitude | 
|---|---|---|---|---|---|
| Afrikaans | `af-ZA` | Expérimentation | Japonais | `ja-JP` | Compatible | 
| Amharique | `am-ET` | Expérimentation | Javanais | `jv-ID` | Expérimentation | 
| Arabe (Égypte) | `ar-EG` | Expérimentation | Créole capverdien | `kea-CV` | Expérimentation | 
| Arménien | `hy-AM` | Expérimentation | Kannada | `kn-IN` | Expérimentation | 
| Assamais | `as-IN` | Expérimentation | Kazakh | `kk-KZ` | Expérimentation | 
| Azéri | `az-AZ` | Expérimentation | Coréen | `ko-KR` | Compatible | 
| Biélorusse | `be-BY` | Expérimentation | Kirghyz | `ky-KG` | Expérimentation | 
| Bengali (Bangladesh) | `bn-BD` | Expérimentation | Letton | `lv-LV` | Expérimentation | 
| Bengali (Inde) | `bn-IN` | Expérimentation | Lingala | `ln-CD` | Expérimentation | 
| Bosniaque | `bs-BA` | Expérimentation | Lituanien | `lt-LT` | Expérimentation | 
| Bulgare | `bg-BG` | Expérimentation | Macédonien | `mk-MK` | Expérimentation | 
| Bulgare (aroumain) | `rup-BG` | Expérimentation | Malaisien | `ms-MY` | Expérimentation | 
| Birman | `my-MM` | Expérimentation | Malayalam | `ml-IN` | Expérimentation | 
| Cantonais (traditionnel) | `yue-Hant-HK` | Expérimentation | Maltais | `mt-MT` | Expérimentation | 
| Catalan | `ca-ES` | Compatible | Chinois mandarin (simplifié) | `cmn-Hans-CN` | Compatible | 
| Cebuano | `ceb` | Expérimentation | Marathi | `mr-IN` | Expérimentation | 
| Khmer central | `km-KH` | Expérimentation | Mongol | `mn-MN` | Expérimentation | 
| Croate | `hr-HR` | Compatible | Népalais | `ne-NP` | Expérimentation | 
| Tchèque | `cs-CZ` | Expérimentation | Norvégien | `nb-NO` | Expérimentation | 
| Danois | `da-DK` | Compatible | Oriya | `or-IN` | Expérimentation | 
| Néerlandais | `nl-NL` | Compatible | Polonais | `pl-PL` | Compatible | 
| Anglais (Australie) | `en-AU` | Compatible | Portugais (Brésil) | `pt-BR` | Compatible | 
| Anglais (Grande-Bretagne) | `en-GB` | Compatible | Portugais (Portugal) | `pt-PT` | Compatible | 
| Anglais (Inde) | `en-IN` | Compatible | Panjabi | `pa-IN` | Expérimentation | 
| Anglais (États-Unis) | `en-US` | Compatible | Panjabi (alphabet gurmukhī) | `pa-Guru-IN` | Expérimentation | 
| Estonien | `et-EE` | Expérimentation | Roumain | `ro-RO` | Compatible | 
| Persan | `fa-IR` | Expérimentation | Russe | `ru-RU` | Compatible | 
| Philippin | `fil-PH` | Expérimentation | Serbe | `sr-RS` | Expérimentation | 
| Finnois | `fi-FI` | Compatible | Sindhi (alphabet arabe) | `sd-Arab-IN` | Expérimentation | 
| Français | `fr-FR` | Compatible | Slovaque | `sk-SK` | Expérimentation | 
| Français (Canada) | `fr-CA` | Compatible | Slovène | `sl-SI` | Expérimentation | 
| Galicien | `gl-ES` | Expérimentation | Espagnol (Amérique latine) | `es-419` | Expérimentation | 
| Géorgien | `ka-GE` | Expérimentation | Espagnol (Espagne) | `es-ES` | Compatible | 
| Allemand | `de-DE` | Compatible | Espagnol (États-Unis) | `es-US` | Compatible | 
| Grec | `el-GR` | Compatible | Swahili (Kenya) | `sw-KE` | Expérimentation | 
| Gujarati | `gu-IN` | Expérimentation | Suédois | `sv-SE` | Compatible | 
| Haoussa | `ha-NG` | Expérimentation | Tadjik | `tg-TJ` | Expérimentation | 
| Hébreu | `he-IL` | Expérimentation | Télougou | `te-IN` | Expérimentation | 
| Hindi | `hi-IN` | Compatible | Thaï | `th-TH` | Expérimentation | 
| Hongrois | `hu-HU` | Expérimentation | Turc | `tr-TR` | Compatible | 
| Islandais | `is-IS` | Expérimentation | Ukrainien | `uk-UA` | Compatible | 
| Indonésien | `id-ID` | Expérimentation | Ouzbek | `uz-UZ` | Expérimentation | 
| Italien | `it-IT` | Compatible | Vietnamien | `vi-VN` | Compatible | 

## Disponibilité en fonction des régions

Gemini 3.5 Transcribe est disponible dans les emplacements Google Cloud suivants :

| Point de terminaison | Google Cloud Emplacement | Disponibilité pour le lancement | 
|---|---|---|
| `gemini-3.5-transcribe` | `global` | Compatible | 
| `gemini-3.5-transcribe` | `us` | Compatible | 
| `gemini-3.5-transcribe` | `eu` | Compatible | 
| `gemini-3.5-transcribe-live` | `global` | Compatible | 
| `gemini-3.5-transcribe-live` | `us` | Compatible | 
| `gemini-3.5-transcribe-live` | `eu` | Compatible | 

## Bonnes pratiques

- **Fournissez un son clair** : assurez-vous que les enregistrements audio présentent une séparation claire des voix et évitez les coupures importantes.
- **Fournissez des indications de langue lorsqu'elles sont connues** : si vous connaissez la langue audio à l'avance, spécifiez`language_codes` pour maximiser la précision.
- **Vocabulaire personnalisé cible** : incluez uniquement des termes de domaine, des noms de marque ou des noms propres distincts dans`custom_vocabulary` , plutôt que des mots courants.
- **Utiliser la détection manuelle des fins de phrase avec le mode `SMART` dans la transcription en direct** : désactivez la détection automatique de l'activité vocale (`automatic_activity_detection` ) et envoyez explicitement les signaux`activity_start` et`activity_end` pour que le modèle puisse appliquer la suppression des disfluences, les corrections automatiques et la mise en forme sur l'ensemble des énoncés.

Essayer dans Agent Studio Tarifs

| ID du modèle | `['gemini-3.5-transcribe', 'gemini-3.5-transcribe-live']` |  | 
|---|---|---|
| Modalités |  |  | 
| Capacités |  |  | 
| Outils |  |  | 
| API |  |  | 
| Options de consommation |  |  | 
| Régions où le service est disponible |              **Disponibilité des modèles**              |  | 
|  |              **Traitement ML**              |  | 
| Versions |  |  | 
| Contrôles de sécurité | **Prédiction en ligne** |  | 
|  | Pour en savoir plus, consultez Contrôles de sécurité. |  |
