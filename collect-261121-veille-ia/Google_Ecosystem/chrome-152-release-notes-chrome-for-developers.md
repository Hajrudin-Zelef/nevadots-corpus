---
id: collect-261121-veille-ia/google-ecosystem/chrome-152-release-notes-chrome-for-developers
title: "Chrome 152 | Release notes | Chrome for Developers"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-08-25"]
keywords: ["chrome", "permet", "entree", "reference", "chromestatus", "specification", "cible", "utilisateur"]
source: https://developer.chrome.com/release-notes/152
source_anchor: ""
source_lines: [1, 151]
section: "Google_Ecosystem"
---
**Date de disponibilité de la version stable** : 25 août 2026

Sauf indication contraire, les modifications suivantes s'appliquent à la version stable de Chrome 152 canal pour Android, ChromeOS, Linux, macOS et Windows.

## CSS et interface utilisateur

### Prise en charge de `CSSPseudoElement` pour `::backdrop`, `::scroll-marker` et `::view-transition`

La prise en charge de `CSSPseudoElement`, précédemment définie pour `::after`, `::before` et `::marker`, s'étend à plusieurs nouveaux pseudo-éléments :

- `::backdrop` : permet de fermer une boîte de dialogue lorsque l'utilisateur clique sur l'arrière-plan sans interférer avec les clics dans le contenu de la boîte de dialogue. Il n'est donc plus nécessaire d'utiliser une logique d'intersection complexe pour déterminer où le clic a eu lieu.
- `::scroll-marker` : permet de collecter des statistiques sur les clics.
- `::view-transition` : ouvre la voie à la prise en charge des transitions de vue tenant compte de la géométrie et à l'interception d'une transition de vue en cours pour en démarrer une nouvelle.


Entrée ChromeStatus.com | Spécification

### Couleurs alpha relatives (fonction CSS Color 5 `alpha()`)

Les couleurs alpha relatives font référence à une couleur d'origine et ne modifient que le canal alpha, ce qui permet aux développeurs de définir la transparence par rapport à une valeur de couleur existante.


Bug de suivi #492246715 | Entrée ChromeStatus.com | Spécification

### Propriété CSS `window-drag`

La propriété CSS `window-drag` permet au contenu Web de désigner des zones de l'interface utilisateur d'une application Web de bureau installée qui se comportent comme des zones de barre de titre de fenêtre déplaçables. Lorsqu'elle est appliquée, les interactions du pointeur, telles que le cliquer-glisser, déplacent la fenêtre d'application de premier niveau au lieu de déclencher une interaction normale avec la page. Cette fonctionnalité normalise et renomme la propriété CSS `app-region` existante, remplace ses noms de valeurs par `move` et `none`, et ajoute un comportement d'héritage explicite.


Bug de suivi #477608113 | Entrée ChromeStatus.com | Spécification

## DOM et HTML

### Exposer l'attribut HTML global `autocorrect`

L'attribut HTML `autocorrect` permet aux auteurs Web de contrôler si la correction automatique s'applique à l'entrée utilisateur dans les éléments modifiables, y compris les hôtes `<input>`, `<textarea>` et `contenteditable`. Cette fonctionnalité expose l'attribut HTML global `autocorrect` et le reflète sur `HTMLElement`.


Bug de suivi #40871769 | Entrée ChromeStatus.com | Spécification

### OpaqueRange

`OpaqueRange` représente une étendue de texte en direct dans une valeur de contrôle de formulaire, telle qu'un `<textarea>` ou un `<input>` basé sur du texte. Les développeurs peuvent ainsi utiliser le texte de la valeur à l’aide d’API de type plage. Il permet d'effectuer des opérations telles que `getBoundingClientRect()`, `getClientRects()` et l'intégration à l'API CSS Custom Highlight pour les suggestions, les mises en surbrillance et les pop-ups ancrés. Il préserve l'encapsulation en n'exposant que les décalages de valeur tout en renvoyant `null` pour `startContainer` et `endContainer`.


Bug de suivi #421421332 | Entrée ChromeStatus.com | Spécification

### Cible de référence pour ARIA inter-racine

La cible de référence permet de transférer des attributs d'ID tels que `<label for>`, `aria-labelledby`, `popovertarget` et `commandfor` vers des éléments du DOM fantôme d'un composant, tout en conservant l'encapsulation de l'état interne. Lorsqu'un hôte fantôme spécifie un élément dans son arborescence fantôme pour qu'il serve de cible de référence, toutes les références d'ID pointant vers l'hôte fantôme sont transférées à l'élément cible de référence. La cible de référence peut être définie de manière déclarative à l'aide de l'attribut `shadowrootreferencetarget` sur `<template>` ou en JavaScript à l'aide de `ShadowRoot.prototype.referenceTarget`.


Bug de suivi #346835896 | Entrée ChromeStatus.com | Spécification

## Applications Web

### Attribution des notifications pour les PWA sur macOS

Lorsqu'une progressive web app (PWA) est installée sur un appareil macOS, ses notifications sont attribuées nativement à la PWA elle-même (à l'aide de son propre nom et de sa propre icône dans le centre des notifications) plutôt qu'à Google Chrome. Les notifications des PWA sont alignées sur les applications macOS natives : Chrome ne prend plus en charge le champ `requireInteraction` pour les notifications sur macOS, et l'API Badging nécessite des autorisations de notification pour que le badge de l'application s'affiche.


Bug de suivi #327449602 | Entrée ChromeStatus.com | Spécification

### Sous-applications pour les applications Web isolées

Les sous-applications permettent aux développeurs de créer plusieurs applications au sein d'une même installation d'application Web isolée (AWI). Chaque sous-application possède son propre nom, ses propres icônes et ses propres intégrations à l'OS. Elle apparaît dans la barre des tâches du bureau, avec une identité distincte de celle de l'AWI parente, tout en conservant un processus d'installation et de mise à jour unique et unifié pour l'AWI. Les administrateurs peuvent contrôler cette fonctionnalité à l'aide de règles d'entreprise.


Bug de suivi #414729785 | Entrée ChromeStatus.com | Spécification

### Mode d'affichage sans cadre pour les applications Web isolées

Le mode d'affichage sans cadre permet aux applications Web isolées d'occuper toute la fenêtre du navigateur en supprimant les bordures et les barres de titre des fenêtres standards. Il optimise ainsi l'espace de travail disponible et permet aux développeurs de mettre en œuvre des expériences utilisateur uniques avec des hiérarchies de menus et des éléments de branding personnalisés. Les administrateurs peuvent gérer cette fonctionnalité à l'aide de règles d'entreprise de gestion des fenêtres.


Bug de suivi #477512407 | Entrée ChromeStatus.com | Spécification

## Performances et mise en réseau

### API CPU Performance

L'API CPU Performance permet aux applications Web de déterminer le niveau de performances du processeur de l'appareil d'un utilisateur. Les applications Web peuvent utiliser ces informations pour améliorer l'expérience utilisateur, éventuellement en combinaison avec l'API Compute Pressure pour réagir aux changements de pression du processeur. Les utilisateurs et les administrateurs peuvent remplacer le niveau de performances signalé à l'aide des paramètres Chrome et des règles d'entreprise.


Bug de suivi #449760252 | Entrée ChromeStatus.com | Spécification

### Listes d'autorisation de connexion

Les listes d'autorisation de connexion offrent un contrôle explicite sur les points de terminaison externes en limitant les connexions initiées via l'API Fetch ou d'autres API de plate-forme Web à partir d'un document ou d'un worker. Les serveurs distribuent une liste de points de terminaison autorisés via un en-tête de réponse HTTP, et Chrome évalue les points de terminaison de destination par rapport à cette liste d'autorisation avant d'établir des connexions.


Bug de suivi #447954811 | Entrée ChromeStatus.com | Spécification

## Média, capteurs et entrée

### `MediaCapabilities.decodingInfo.encryptionScheme`

Ajoute l'attribut `encryptionScheme` au dictionnaire `KeySystemTrackConfiguration` utilisé dans `navigator.mediaCapabilities.decodingInfo()`. Cela permet aux applications Web de vérifier si un schéma de chiffrement spécifique (tel que `'cenc'` ou `'cbcs'`) est pris en charge.


Bug de suivi #498284510 | Entrée ChromeStatus.com | Spécification

### Capture des préférences audio dans `getDisplayMedia()`

Cet indice permet aux applications Web de signaler au navigateur qu'elles préfèrent le partage audio en plus de la vidéo. Cela permet aux développeurs de s'assurer que les applications qui s'appuient sur la capture audio fonctionnent de manière fluide.


Bug de suivi #535514300 | Entrée ChromeStatus.com | Spécification

### WebGPU : contrôle de la taille du sous-groupe

Ajoute la fonctionnalité GPU facultative `"subgroup-size-control"` qui permet de définir explicitement la taille du sous-groupe dans un shader de calcul. Cela optimise les performances du shader de calcul à l'aide d'opérations de sous-groupe avec des tailles de sous-groupe spécifiques sur les plates-formes matérielles ciblées, en particulier pour les charges de travail d'IA.


Bug de suivi #463721943 | Entrée ChromeStatus.com | Spécification

## Confidentialité et sécurité

### Avertissements concernant les sites suspects

Pour les utilisateurs de la protection renforcée de la navigation sécurisée, Chrome affiche un pop-up d'avertissement pouvant être ignoré lorsqu'ils consultent des sites présentant des signaux indiquant qu'ils sont potentiellement malveillants. Cet avertissement s'ajoute aux avertissements interstitiels existants pour les sites Web confirmés comme malveillants. Les administrateurs peuvent configurer cette fonctionnalité à l'aide des règles de navigation sécurisée.


## Phases d'évaluation

### Évaluation avant arrêt pour XSLT côté client

XSLT côté client est obsolète et sa suppression de la plate-forme Web est prévue. Une évaluation avant arrêt est disponible à partir de Chrome 152 pour donner aux sites un délai de migration supplémentaire.


Évaluation avant arrêt | Bug de suivi #435623334 | Entrée ChromeStatus.com

### Règles de spéculation : contrôles modérés des heuristiques de la fenêtre d'affichage

Fournit des contrôles expérimentaux pour les heuristiques de la fenêtre d'affichage des règles de spéculation, ce qui permet aux développeurs de tester si d'autres paramètres d'heuristique offrent de meilleurs résultats de préchargement et de prérendu que les paramètres par défaut.


Bug de suivi #529423512 | Entrée ChromeStatus.com

### API de remplacement d'image de l'user-agent

Permet aux développeurs d'observer lorsqu'un navigateur modifie ou remplace un média image dans une page au nom de l'utilisateur (par exemple, à l'aide de l'IA générative) afin que les documents puissent adapter d'autres contenus et éviter toute confusion potentielle pour l'utilisateur.


Bug de suivi #544822216 | Entrée ChromeStatus.com | Spécification

## Abandon et suppression

### Supprimer l'API Private Aggregation

Suite à l'annonce de Chrome concernant le maintien de l'approche actuelle concernant les cookies tiers, l'API Private Aggregation est obsolète et supprimée (ainsi que les API Privacy Sandbox associées).
