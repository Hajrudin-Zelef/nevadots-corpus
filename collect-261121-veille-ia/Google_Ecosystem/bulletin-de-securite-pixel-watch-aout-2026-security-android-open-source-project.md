---
id: collect-261121-veille-ia/google-ecosystem/bulletin-de-securite-pixel-watch-aout-2026-security-android-open-source-project
title: "Bulletin de sécurité Pixel Watch – Août 2026 | Security | Android Open Source Project"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-08-01"]
keywords: ["securite", "bulletin", "android", "aout", "pixel", "failles", "watch", "references"]
source: https://source.android.com/docs/security/bulletin/pixel-watch/2026/2026-08-01
source_anchor: ""
source_lines: [1, 62]
section: "Google_Ecosystem"
---
*Publié le 20 août 2026*

Le bulletin de sécurité Pixel Watch contient des informations sur les failles de sécurité qui affectent les appareils Pixel Watch (appareils Google). Pour les appareils Google, les niveaux de correctifs de sécurité du 5 août 2026 ou ultérieurs corrigent tous les problèmes de ce bulletin et tous les problèmes du bulletin sur la sécurité d'Android paru en août 2026. Pour savoir comment vérifier le niveau du correctif de sécurité d'un appareil, consultez le centre d'aide Pixel Watch. Tous les appareils Google compatibles recevront une mise à jour vers le niveau de correctif du 5 août 2026. Nous encourageons tous les clients à accepter ces mises à jour sur leurs appareils.

## Annonces

En plus des failles de sécurité décrites dans le bulletin sur la sécurité d'Android, la Google Pixel Watch contient également des correctifs pour les failles de sécurité décrites dans ce bulletin et dans le bulletin sur les mises à jour Pixel.

## Correctifs de sécurité

Les failles sont regroupées selon les composants concernés. Vous trouverez une description du problème, ainsi qu'un tableau contenant l'ID de la faille CVE, les références associées, le type de faille, sa gravité et les versions mises à jour de l'Android Open Source Project (AOSP) (le cas échéant). Lorsque cela est possible, nous associons la modification publique qui a résolu le problème à l'ID de bug, comme la liste des modifications AOSP. Lorsque plusieurs modifications sont liées à un seul bug, des références supplémentaires sont associées aux numéros suivant l'ID de bug.

### Composants propriétaires Qualcomm

| CVE | Références | Gravité | Sous-composant | 
|---|---|---|---|
| CVE-2026-24078 | A-480963514 * | Modéré | Composant propriétaire | 

## Questions fréquentes et réponses

**Cette section répond aux questions fréquentes qui peuvent se poser après la lecture de ce bulletin.**

**1. Comment savoir si mon appareil a été mis à jour pour résoudre ces problèmes ?**

Les niveaux de correctifs de sécurité du 5 août 2026 ou ultérieurs corrigent tous les problèmes associés au niveau du correctif de sécurité du 5 août 2026 et à tous les niveaux de correctifs précédents. Pour savoir comment vérifier le niveau du correctif de sécurité d'un appareil, consultez les instructions des notes de version de la Pixel Watch.

**2. Pourquoi les failles de sécurité sont-elles réparties entre ce bulletin et les bulletins sur la sécurité d'Android
 ?**

Les failles de sécurité documentées dans les bulletins sur la sécurité d'Android doivent déclarer le dernier niveau du correctif de sécurité sur les appareils Android. Il n'est pas nécessaire de déclarer un niveau de correctif de sécurité pour d'autres failles de sécurité, telles que celles documentées dans ce bulletin.

**3. Que signifient les entrées de la colonne *Type* ?**

Les entrées de la colonne *Type* du tableau sur les détails des failles font référence à la
  classification de la faille de sécurité.

| Abréviation | Définition | 
|---|---|
| RCE | Exécution de code à distance | 
| EoP | Violation de droits | 
| ID | Divulgation d'informations | 
| DoS | Déni de service | 
| N/A | (Not available) Classification non disponible | 

**4. Que signifient les entrées de la colonne *Références* ?**

Les entrées figurant dans la colonne *Références* du tableau détaillé des failles peuvent inclure
 un préfixe qui identifie l’organisation à laquelle la valeur de référence est associée.

| Préfixe | Référence | 
|---|---|
| A- | ID de bug Android | 

**5. Que signifie un astérisque (*) à côté de l'ID de bug Android dans la colonne *Références* ?**

Les problèmes qui ne sont pas accessibles au public sont signalés par un astérisque (*) à côté de l'ID de bug Android dans la colonne "Références".

## Versions

| Version | Date | Remarques | 
|---|---|---|
| 1.0 | 20 août 2026 | Publication du bulletin |
