---
id: collect-261121-veille-ia/google-ecosystem/bulletin-de-mise-a-jour-pixel-aout-2026-security-android-open-source-project
title: "Bulletin de mise à jour Pixel – Août 2026 | Security | Android Open Source Project"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-08-01"]
keywords: ["securite", "jour", "aout", "android", "bulletin", "pixel", "appareils", "reference"]
source: https://source.android.com/docs/security/bulletin/pixel/2026/2026-08-01
source_anchor: ""
source_lines: [1, 76]
section: "Google_Ecosystem"
---
*Publié le 4 août 2026*

Le bulletin de mise à jour Pixel contient des informations sur les failles de sécurité et les améliorations fonctionnelles qui affectent les appareils Pixel compatibles (appareils Google). Pour les appareils Google, les niveaux de correctifs de sécurité du 5 août 2026 ou ultérieurs corrigent tous les problèmes mentionnés dans ce bulletin et dans le bulletin sur la sécurité d'Android paru en août 2026. Pour savoir comment vérifier le niveau du correctif de sécurité d'un appareil, consultez Vérifier la version d'Android et la mettre à jour.

Tous les appareils Google compatibles recevront une mise à jour vers le niveau de correctif du 5 août 2026. Nous encourageons tous les clients à accepter ces mises à jour sur leurs appareils.

## Annonces

- En plus des failles de sécurité décrites dans le bulletin sur la sécurité d'Android paru en août 2026, les appareils Google contiennent également des correctifs pour les failles de sécurité décrites ci-dessous.

## Correctifs de sécurité

Les failles sont regroupées selon les composants concernés. Vous trouverez une description du problème, ainsi qu'un tableau contenant l'ID de la faille CVE, les références associées, le type de faille, sa gravité, et les versions mises à jour du Projet Android Open Source (AOSP) (le cas échéant). Lorsque cela est possible, nous associons la modification publique qui a résolu le problème à l'ID du bug, comme la liste des modifications AOSP. Lorsque plusieurs modifications sont liées à un seul bug, des références supplémentaires sont associées à des numéros suivant l'ID du bug.

### Pixel

| CVE | Références | Type | Gravité | Sous-composant | 
|---|---|---|---|---|
| CVE-2026-0163 | A-493648935 * | Eop | Élevée | Unité de traitement vidéo | 

## Correctifs fonctionnels

Pour en savoir plus sur les nouvelles corrections de bugs et les nouveaux correctifs fonctionnels inclus dans cette version, consultez le forum de la communauté Pixel.

## Questions fréquentes et réponses

Cette section répond aux questions fréquentes qui peuvent survenir après la lecture de ce bulletin.

**1. Comment savoir si mon appareil a été mis à jour pour résoudre ces problèmes ?**

Les niveaux de correctifs de sécurité du 5 août 2026 ou ultérieurs corrigent tous les problèmes associés au niveau du correctif de sécurité du 5 août 2026 et à tous les niveaux de correctifs précédents. Pour savoir comment vérifier le niveau du correctif de sécurité d'un appareil, consultez les instructions du calendrier de mise à jour des appareils Google.

**2. Que signifient les entrées de la colonne *Type* ?**

Les entrées de la colonne *Type* du tableau sur les détails des failles
 font référence à la classification de la faille de sécurité.

| Abréviation | Définition | 
|---|---|
| RCE | Exécution de code à distance | 
| EoP | Violation de droits | 
| ID | Divulgation d'informations | 
| DoS | Déni de service | 
| N/A | (Not available) Classification non disponible | 

**3. Que signifient les entrées de la colonne *Références* ?**

Les entrées figurant dans la colonne *Références* du tableau détaillé des failles
 peuvent inclure un préfixe qui identifie l’organisation à laquelle la valeur de référence
 est associée.

| Préfixe | Référence | 
|---|---|
| A- | ID de bug Android | 
| QC- | Numéro de référence Qualcomm | 
| M- | Numéro de référence MediaTek | 
| N- | Numéro de référence NVIDIA | 
| B- | Numéro de référence Broadcom | 
| U- | Numéro de référence UNISOC | 

**4. Que signifie un astérisque (*) à côté de l'ID de bug Android dans la colonne *Références* ?**

Les problèmes qui ne sont pas accessibles au public sont signalés par un astérisque (*) à côté de l'ID de bug Android
 dans la colonne *Références*. La mise à jour de ce problème est généralement
 incluse dans les derniers pilotes binaires pour les appareils Pixel disponibles sur le
 site Google Developers.

**5. Pourquoi les failles de sécurité sont-elles réparties entre ce bulletin et les bulletins sur la sécurité d'Android ?**

Les failles de sécurité documentées dans les bulletins sur la sécurité d'Android doivent déclarer le dernier niveau du correctif de sécurité sur les appareils Android. Il n'est pas nécessaire de déclarer un niveau du correctif de sécurité pour d'autres failles de sécurité, telles que celles documentées dans ce bulletin.

## Versions

| Version | Date | Remarques | 
|---|---|---|
| 1.0 | 4 août 2026 | Publication du bulletin |
