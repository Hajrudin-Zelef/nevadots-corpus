---
id: collect-261121-veille-ia/modeles-open/glimpse-api-essai-gratuit-caracteristiques
title: "Glimpse API : essai gratuit, caractéristiques"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-16"]
keywords: ["felo", "glimpse", "messages", "tokens", "raisonnement", "modele", "votre", "response"]
source: https://openapi.felo.ai/models/elorian/glimpse
source_anchor: ""
source_lines: [1, 137]
section: "Modeles_Open"
---
## Points clés

- Raisonnement visuel avec espace de travail dédié
- Réexamen des images et vidéos réelles
- Zoom sur les détails des images
- Ralentissement et suivi d'objets dans les vidéos
- Appel d'outils et sorties structurées
- Contexte de 262 000 tokens

Comparez les prix des API entre fournisseurs. Sélectionnez un fournisseur pour voir tous les tarifs et conditions.

| Fournisseur · Détails des tarifs |  |  |  |  | 
|---|---|---|---|---|
|  |  |  |  | Disponibilité | 
|---|---|---|---|---|
|  | $0.6 | $3 | $0.06 |  | 

Les statistiques historiques ne sont pas un contrôle en direct. Une donnée manquante ne signifie pas une panne.

## Ce que $10 permettent d'acheter

- Entrée
- 16,7 M tokens
- Sortie
- 3,3 M tokens

Estimation d'après les prix actuels d'entrée et de sortie, hors remises.

## Raisonnement

- Toujours activé
- Non

## Démarrage rapide

Un code prêt à l'emploi pour appeler ce modèle avec les API compatibles de Felo API Platform. Commencez dans le Playground pour tester des prompts avant d'écrire du code.

### Configurer votre clé API

Créez une clé depuis votre compte Felo API Platform et définissez-la comme variable d'environnement.

`export FELO_API_KEY="YOUR_API_KEY"`
### Effectuer votre première requête

Utilisez cet ID de modèle avec le point de terminaison Felo API Platform. Les en-têtes de requête facultatifs comme X-Request-Id facilitent le diagnostic.

`glimpse`
Ce modèle est capable de raisonnement. Si le protocole sélectionné et la route du modèle exposent des contrôles de raisonnement, conservez les métadonnées de raisonnement renvoyées lorsque vous poursuivez la conversation.

```
const response = await fetch("https://openapi.felo.ai/api/v1/messages", {
  method: 'POST',
  headers: {
    Authorization: `Bearer ${process.env.FELO_API_KEY}`,
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
  "model": "glimpse",
  "max_tokens": 1024,
  "messages": [
    {
      "role": "user",
      "content": "Hello"
    }
  ]
}),
});
if (!response.ok) throw new Error(await response.text());
console.log(await response.json());
```
#### Utiliser des SDK tiers

Avec les SDK compatibles, définissez baseURL sur `https://openapi.felo.ai/api/v1`. Pour les clients de style Claude, utilisez l'URL de base compatible Anthropic. `https://openapi.felo.ai/api`.

### Activer le streaming

Ajoutez stream: true pour recevoir les réponses sous forme de server-sent events.

```
curl -N https://openapi.felo.ai/api/v1/messages \
  -H "Authorization: Bearer $FELO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
  "model": "glimpse",
  "max_tokens": 1024,
  "messages": [
    {
      "role": "user",
      "content": "Hello"
    }
  ],
  "stream": true
}'
```
### Endpoint

Felo API Platform expose des surfaces d'appel de modèles compatibles. Choisissez la surface que votre application ou votre client agent prend déjà en charge.

Messages

Messages compatibles Anthropic pour les clients de style Claude.

POST

`https://openapi.felo.ai/api/v1/messages`
### Paramètres

`model`stringglimpseID public du modèle à appeler.`max_tokens`integer—Nombre maximal de tokens générés.`messages`array—Messages de la conversation pour Chat Completions.`stop`———`min_p`———`tools`array—Définitions de tool calling conformes au schéma compatible.`top_k`———`top_p`———`logprobs`———`reasoning`———`logit_bias`———`temperature`number—Contrôle la variété des réponses lorsque c'est pris en charge.`tool_choice`string or object—Contrôle si un outil doit être appelé.`top_logprobs`———`response_format`object—Demande une sortie structurée lorsque c'est pris en charge.`presence_penalty`———`frequency_penalty`———`include_reasoning`———`repetition_penalty`———`structured_outputs`———
## Questions fréquentes

## Qu'est-ce que Glimpse?

Elorian: Glimpse est le premier modèle d'Elorian, dévoilé le 16 octobre 2026. Il se distingue par son approche du raisonnement visuel : il utilise un espace de travail visuel pour revenir sur les images et vidéos réelles pendant qu'il raisonne. Concrètement, il peut zoomer sur les coins d'une image pour inspecter des détails, ralentir une vidéo à un taux d'images plus élevé, ou suivre un objet spécifique sur plusieurs frames. Le modèle traite directement les images et les vidéos et répond en texte. Le modèle prend en charge le texte, les images et les vidéos en entrée, avec une fenêtre de contexte de 262 000 tokens. Il supporte également l'appel d'outils et les sorties structurées. Elorian le présente comme un modèle de recherche, accessible via API et via une interface de chat sur liste d'attente. Il vise à combler un vide : selon Elorian, les modèles généralistes de pointe sont soit insuffisamment précis, soit trop coûteux pour les charges de travail visuelles. Glimpse est conçu pour des tâches telles que l'étiquetage de données synthétiques et la planification de haut niveau en robotique, la revue vidéo sur des sites de construction et de fabrication, le sous-titrage vidéo dense pour les archives médiatiques, le comptage précis d'objets sur les chantiers, ou encore l'analyse visuelle fréquente à grand volume. Il peut également être utilisé pour des revues vidéo horodatées et la comparaison de plusieurs images. Elorian indique que Glimpse surpasse GPT-6 Astra sur le raisonnement à partir de graphiques scientifiques, le comptage d'objets en vocabulaire ouvert, le sous-titrage de courtes vidéos et la compréhension d'événements de manipulation, et qu'il égale GPT-6 Astra sur le raisonnement incarné. Cependant, des tests indépendants montrent qu'il ne surpasse pas systématiquement GPT-6 Astra et Claude Opus 5.5, et qu'il peut se contredire dans de longues réponses.

## Comment l'usage est-il facturé ?

Votre facture dépend du fournisseur utilisé, de ses tarifs et de votre usage. Des entrées plus longues ou d'autres conditions peuvent avoir des tarifs différents.

## Quels types d'entrées le modèle accepte-t-il ?

Elorian: Glimpse accepte le texte, les images et les vidéos en entrée, et produit des réponses en texte.

## Quelle est la longueur de contexte prise en charge ?

Le modèle dispose d'une fenêtre de contexte de 262 000 tokens.

## À quelles tâches le modèle est-il particulièrement adapté ?

Il est conçu pour l'étiquetage de données synthétiques, la planification en robotique, la revue vidéo sur sites de construction et de fabrication, le sous-titrage vidéo dense, le comptage d'objets et l'analyse visuelle fréquente à grand volume.

## Comment se compare-t-il aux autres modèles de la série ?

Selon Elorian, Glimpse surpasse GPT-6 Astra sur plusieurs tâches de raisonnement visuel et l'égale sur le raisonnement incarné. Toutefois, des tests indépendants indiquent que GPT-6 Astra reste plus fort sur les puzzles visuels fins et le sous-titrage vidéo dense, tandis que Claude Opus 5.5 est légèrement meilleur pour lire les graphiques scientifiques.

## Le modèle est-il exempt de limitations ?

Non. Il peut se contredire dans de longues réponses multi-parties et ne surpasse pas systématiquement GPT-6 Astra et Claude Opus 5.5 sur toutes les tâches.
