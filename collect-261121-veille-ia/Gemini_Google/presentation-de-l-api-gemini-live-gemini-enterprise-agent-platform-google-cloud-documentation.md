---
id: collect-261121-veille-ia/gemini-google/presentation-de-l-api-gemini-live-gemini-enterprise-agent-platform-google-cloud-documentation
title: "Présentation de l'API Gemini Live | Gemini Enterprise Agent Platform | Google Cloud Documentation"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "live", "video", "audio", "agents", "modele", "permet", "vocales"]
source: https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/live-api
source_anchor: ""
source_lines: [1, 67]
section: "Gemini_Google"
---
L'API Gemini Live permet des interactions vocales et vidéo en temps réel et à faible latence avec Gemini. Il traite des flux continus d'audio, de vidéo ou de texte pour fournir des réponses vocales immédiates et naturelles. Cela crée une expérience conversationnelle naturelle pour vos utilisateurs. Les avatars live permettent d'interagir avec une icône d'apparence humaine qui se synchronise avec les réponses vocales, offrant ainsi une expérience améliorée et plus conviviale aux utilisateurs.

## Exemples de cas d'utilisation

L'API Gemini Live peut être utilisée pour créer des agents vocaux et vidéo en temps réel pour divers secteurs, y compris :

- **Ambassadeurs de marque d'entreprise** : représentants de marque interactifs déployés sur des propriétés Web ou des applications mobiles d'entreprise, générés à partir de l'image d'employés autorisés ou de talents rémunérés ayant signé des autorisations d'entreprise.
- **E-commerce et commerce** : assistants d'achat qui proposent des recommandations personnalisées et agents d'assistance qui résolvent les problèmes des clients.
- **Jeux vidéo** : personnages non jouables (PNJ) interactifs, assistants d'aide dans le jeu et traduction en temps réel du contenu du jeu.
- **Interfaces de nouvelle génération** : expériences vocales et vidéo dans la robotique, les lunettes connectées et les véhicules.
- **Santé** : guides de navigation qui facilitent la logistique des établissements et des soins.
- **Services financiers** : agents conversationnels qui trient les demandes bancaires quotidiennes.
- **Éducation** : mentors et compagnons d'apprentissage IA qui fournissent des instructions et des commentaires personnalisés.

## Principales fonctionnalités

L'API Gemini Live offre un ensemble complet de fonctionnalités pour créer des agents vocaux et vidéo robustes :

- **Qualité audio élevée** : l'API Gemini Live fournit une voix naturelle et réaliste dans plusieurs langues.
- **Voix prédéfinies et personnalisées** : configurez les réponses avec 30 voix HD prédéfinies ou répliquez une voix personnalisée par session.
- **Compatibilité multilingue** : discutez dans 24 langues.
- **Interruption** : les utilisateurs peuvent interrompre le modèle à tout moment pour des interactions réactives.
- **Dialogue affectif** : adapte le style et le ton de la réponse à l'expression de l'utilisateur.
- **Utilisation d'outils** : intègre des outils tels que l'appel de fonction et la recherche Google pour des interactions dynamiques.
- **Transcriptions audio** : fournit des transcriptions textuelles de l'entrée utilisateur et de la sortie du modèle. Pour la transcription en tant qu'objectif final plutôt qu'en tant que canal secondaire d'une conversation, consultez Gemini 3.5 Transcribe.
- **Audio proactif** : vous permet de contrôler quand le modèle répond et dans quels contextes.

## Caractéristiques techniques

Le tableau suivant présente les caractéristiques techniques de l'API Gemini Live :

| Catégorie | Détails | 
|---|---|
| Modes d'entrée | Audio (audio PCM 16 bits brut, 16 kHz, little-endian), images/vidéo (JPEG 1 FPS), texte | 
| Modes de sortie | Audio (audio PCM 16 bits brut, 24 kHz, little-endian), texte | 
| Vidéo (avatars live) | Vidéo (mp4) | 
| Protocole | Connexion WebSocket avec état (WSS) | 

## Modèles compatibles

Les modèles suivants sont compatibles avec l'API Gemini Live. Sélectionnez le modèle approprié en fonction de vos exigences d'interaction.

| ID du modèle | Disponibilité | Cas d'utilisation | Principales fonctionnalités | 
|---|---|---|---|
| `gemini-3.8-live` | Disponibilité générale | **Recommandation** . Agents vocaux à faible latence. Permet de passer d'une langue à l'autre de manière fluide. |  | 
| `gemini-live-2.5-flash-native-audio` | Disponibilité générale | **Recommandation** . Agents vocaux à faible latence. Permet de passer d'une langue à l'autre de manière fluide et de changer de ton émotionnel. |  | 
| `gemini-3.5-transcribe-live-preview` | Aperçu | Reconnaissance vocale uniquement. Sous-titrage, dictée et transcription des appels en temps réel. Renvoie du texte, pas du contenu audio, et ne tient donc pas de conversation. |  | 

## Commencer

Sélectionnez le guide correspondant à votre environnement de développement :

Connectez-vous à l'API Gemini Live à l'aide du SDK IA générative pour créer une application multimodale en temps réel avec un backend Python.

Connectez-vous à l'API Gemini Live à l'aide de WebSockets pour créer une application multimodale en temps réel avec un frontend JavaScript et un backend Python.

### Intégration de partenaires

Si vous souhaitez intégrer certains de nos partenaires, ces plates-formes ont déjà intégré l'API Gemini Live via le protocole WebRTC pour simplifier le développement d'applications audio et vidéo en temps réel.

#### Architecture de référence LiveKit et ADK

Pour les assistants vocaux avancés de qualité production nécessitant une orchestration de session, un routage et une délégation multi-agents sophistiqués, vous pouvez créer une intégration bidirectionnelle à faible latence à l'aide de **LiveKit (WebRTC)** avec l'**API Gemini Live** via l'**Agent Development Kit (ADK)**.

Cette architecture vous permet de déployer un agent orchestrateur qui gère l'interaction initiale avec le client et achemine de manière dynamique les requêtes vers des sous-agents spécialisés (par exemple, en déléguant le contexte à un agent de réservation de vols ou d'hôtels à l'aide de l'orchestration `transferAgent`).

Pour explorer et déployer cette conception de référence, consultez l'implémentation officielle de la codebase de référence sur GitHub.
