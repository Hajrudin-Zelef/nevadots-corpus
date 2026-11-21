---
id: collect-261121-veille-ia/modeles-open/kling-image-4-0-tarifs-api-sortie-8k-et-series-dimages
title: "Kling Image 4.0 : tarifs API, sortie 8K et séries d’images"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["image", "kling", "images", "souvre", "nouvel", "onglet", "dimages", "novembre"]
source: https://omniart.studio/blog/models-insights/kling-image-4-0-explained
source_anchor: ""
source_lines: [1, 76]
section: "Modeles_Open"
---
Kling Image 4.0 est un modèle d’images fixes avec le texte vers image, la génération à partir d’images de référence, des séries d’images coordonnées et un processus distinct pour les images longues. La mise à jour officielle de l’API Kling (S’ouvre dans un nouvel onglet), consultée le 18 novembre 2026, date la disponibilité de l’API au 17 novembre. Il est distinct du modèle vidéo Kling 4.0.

Kling Image 4.0 est disponible sur OmniArt via fal depuis le 18 novembre 2026, pour le forfait Starter et les forfaits supérieurs. Le guide de lancement OmniArt traite des crédits et des réglages. Cet article couvre l’API en amont, plus large.

## Ce que propose Kling Image 4.0#

Les notes de version de Kling (S’ouvre dans un nouvel onglet), consultées le 18 novembre, décrivent une sortie native en 2K, 4K et 8K, jusqu’à dix images de référence et des séries planifiées d’images liées. Kling signale des progrès en réalisme, en rendu du texte et en suivi des instructions. Ce sont des déclarations du fournisseur. Nos vérifications de dimensions et de facturation n’établissent pas de classement de qualité.

Sur fal, la génération standard a deux points d’accès : texte vers image (S’ouvre dans un nouvel onglet) et image vers image (S’ouvre dans un nouvel onglet). Leurs schémas d’API, consultés le 18 novembre, distinguent deux types de résultat :

| Type de résultat | Ce que l’API demande | 
|---|---|
| `single` | Images indépendantes ; `num_images` accepte 1–9 | 
| `series` | Une série cohérente ; `series_amount` accepte 2–10, ou`auto` | 

Les deux acceptent 2K, 4K et 8K. Une série demande au modèle de planifier ensemble des images liées. Augmenter le nombre d’images indépendantes ne sélectionne pas le mode série. OmniArt propose actuellement 1–4 images indépendantes par requête.

## Images de référence et syntaxe du prompt#

Le schéma image vers image de fal (S’ouvre dans un nouvel onglet), consulté le 18 novembre, accepte 1–10 images de référence. Mentionnez-les dans le prompt sous la forme `@Image1`, `@Image2`, et ainsi de suite. Les éléments en amont utilisent `@Element1`, `@Element2`, et ainsi de suite. Les éléments sont plafonnés à sept ; images et éléments réunis ne peuvent pas dépasser dix.

Chaque référence fal doit mesurer au moins 300 pixels dans les deux dimensions, avoir un format de 0,4 à 2,5 et une taille de fichier d’au plus 30 MB, la limite de fal. OmniArt limite chaque envoi à 20 MB et n’expose pas les éléments.

Les schémas d’image standard de fal acceptent des prompts de 1 à 50 000 caractères. Leurs formats sont Auto, 16:9, 9:16, 1:1, 4:3, 3:4, 3:2, 2:3 et 21:9. Ce sont les champs d’entrée de fal. L’API directe de Kling utilise sa propre syntaxe structurée pour les références et des limites de prompt qui dépendent de la langue ; ne substituez pas un format de requête à l’autre.

## Long Image est un point d’accès distinct#

Le point d’accès Long Image de fal (S’ouvre dans un nouvel onglet) génère le visuel d’une page. Son schéma (S’ouvre dans un nouvel onglet), consulté le 18 novembre, propose les skills `webpage` et `ecommerce` en 1K ou 2K. Il renvoie une image, pas un site fonctionnel.

Le skill `ecommerce` exige 1–3 images de produit. Ses choix de longueur sont court, moyen et long. Le skill `webpage` accepte plutôt des références facultatives de style et de sujet ; les images produit de `ecommerce` ne servent pas pour lui. La longueur est ignorée pour les conceptions de page web. La limite du prompt est de 166 000 caractères. OmniArt ne propose pas ce point d’accès.

## Tarifs de l’API Kling Image 4.0 sur fal#

La vérification des tarifs fal des 17 et 18 novembre a relevé **0,063 $ US par unité** pour les deux points d’accès d’image standard et **0,235 $ US par unité** pour Long Image. Voir la page texte vers image (S’ouvre dans un nouvel onglet), la page image vers image (S’ouvre dans un nouvel onglet) et la page Long Image (S’ouvre dans un nouvel onglet). Ce sont des prix unitaires, et non un tableau publié par image.

Pour estimer le coût d’une image, nous avons multiplié ce tarif unitaire par l’en-tête `x-fal-billable-units` d’un appel payant à chaque réglage, le 18 novembre :

| Réglage texte vers image | Unités mesurées | Coût calculé | 
|---|---|---|
| 2K, 16:9 | 1,0 | 0,063 $ US | 
| 4K, 1:1 | 1,4921 | Environ 0,094 $ US | 
| 8K, 16:9 | 2,4921 | Environ 0,157 $ US | 

Un appel image vers image en 2K avec une image de référence et le format Auto a enregistré 1,0476 unité. Par rapport à la base de 1,0 unité en 2K, cela représente environ 0,0476 unité de plus, soit 0,003 $ US pour la référence. C’est un cas mesuré, pas un test de chaque nombre de références. Nous n’avons pas mesuré la facturation de Long Image ; son tarif unitaire de 0,235 $ US ne fixe pas à lui seul le prix d’une page entière.

La page des tarifs image de Kling (S’ouvre dans un nouvel onglet), consultée le 18 novembre, liste des modèles d’image plus anciens et indique que ses packs de ressources générales excluent Image 4.0. Nous n’avons pas pu y vérifier un prix direct de Kling Image 4.0. N’appliquez pas les tarifs des anciens modèles Kling à cette version, et ne traitez pas les tarifs de fal comme le prix direct de Kling.

## Ce que les fichiers téléchargés ont mesuré#

Les mêmes vérifications fal du 18 novembre ont renvoyé ces fichiers PNG :

| Réglage | Dimensions | Taille du fichier | Durée approximative | 
|---|---|---|---|
| 2K, 16:9 | 2720 × 1536 | 8,2 MB | 40 secondes | 
| 4K, 1:1 | 4096 × 4096 | 27,8 MB | 70 secondes | 
| 8K, 16:9 | 10880 × 6144 | 108,7 MB | 138 secondes | 

Chaque ligne correspond à un appel. Les formats diffèrent, ce n’est donc pas une comparaison contrôlée de vitesse ou de compression. La taille du fichier dépend du contenu ; la durée écoulée n’est pas une garantie de service. L’appel 2K Auto avec une référence a aussi renvoyé 2720 × 1536.

Le résultat 8K montre pourquoi une étiquette de résolution ne suffit pas comme spécification de livraison. Ce fichier dépassait 100 MB. OmniArt propose actuellement 1440p et 2160p, correspondant au 2K et au 4K de fal, et laisse le 8K indisponible à cause de la taille de sortie.

## Kling Image 4.0 face à Kling Image O3 et V3#

La comparaison ci-dessous s’appuie sur la documentation O3 (S’ouvre dans un nouvel onglet) et la documentation V3 (S’ouvre dans un nouvel onglet) de fal, consultées le 18 novembre. Elle compare les réglages d’API, pas la qualité d’image.

| Modèle | Plafond de sortie documenté | Processus d’images liées | 
|---|---|---|
| Kling Image V3 Standard | 2K | Texte vers image standard, avec un contrôle facultatif d’élément de visage | 
| Kling Image O3 / Omni 3 | 4K | Génération en série de 2–9 images liées | 
| Kling Image 4.0 | 8K | Série de 2–10 images ou nombre automatique ; point d’accès Long Image distinct | 

La génération en série précède donc Image 4.0. Les changements documentés comprennent un plafond de sortie plus élevé, une série maximale plus grande et le processus de page longue. Nous n’avons pas comparé ces modèles avec le même prompt. Choisissez selon les réglages dont votre tâche a besoin, puis examinez les fichiers obtenus.

## Utiliser le modèle sur OmniArt#

Ouvrez Kling Image 4.0 ou la création d’images. OmniArt propose jusqu’à dix images de référence, une sortie PNG, et 1440p ou 2160p. Il ne propose pas encore le 8K, les séries, les éléments ni Long Image. Le guide de lancement explique les prix de base de 6 et 9 crédits, les suppléments de référence et l’exemption de génération d’images du forfait Ultra.
