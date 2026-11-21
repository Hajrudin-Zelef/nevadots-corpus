---
id: collect-261121-veille-ia/google-ecosystem/bulletin-android-xr-aout-2026-security-android-open-source-project
title: "Bulletin Android XR – Août 2026 | Security | Android Open Source Project"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-08-01"]
keywords: ["securite", "bulletin", "android", "aout", "appareils", "reference", "jour", "failles"]
source: https://source.android.com/docs/security/bulletin/xr/2026/2026-08-01
source_anchor: ""
source_lines: [1, 68]
section: "Google_Ecosystem"
---
*Publié le 3 août 2026* 

Le bulletin de sécurité XR contient des informations sur les failles de sécurité affectant la plate-forme XR. La mise à jour complète de XR comprend le niveau du correctif de sécurité du 5 août 2026 ou version ultérieure du Bulletin sur la sécurité Android d'août 2026, en plus de tous les problèmes mentionnés dans ce bulletin.

Nous encourageons tous les clients à accepter ces mises à jour sur leurs appareils.

## Annonces

- Le bulletin de sécurité XR d'août 2026 ne contient aucun correctif de sécurité XR.

## Questions fréquentes et réponses

Cette section répond aux questions fréquentes qui peuvent survenir après la lecture de ce bulletin.

**1. Comment savoir si mon appareil a été mis à jour pour résoudre ces problèmes ?**

Pour savoir comment vérifier le niveau du correctif de sécurité d'un appareil, consultez les instructions du calendrier de mise à jour des appareils Google.

- 
    Les niveaux de correctifs de sécurité du 1er août 2026 ou version ultérieure corrigent tous les problèmes associés au
niveau du correctif de sécurité du 1er août 2026.

Les fabricants d'appareils qui incluent ces mises à jour doivent définir le niveau de la chaîne de correctifs comme suit :

- [ro.build.version.security_patch]:[2026-08-01]

Pour certains appareils équipés d'Android 10 ou version ultérieure, la mise à jour du système Google Play comportera une chaîne de date correspondant au niveau du correctif de sécurité du 1er août 2026. Pour en savoir plus sur l'installation des mises à jour de sécurité, veuillez consulter cet article.

**2. Que signifient les entrées de la colonne *Type* ?**

Les entrées de la colonne *Type* du tableau sur les détails des failles font référence à la classification de la faille de sécurité.

| Abréviation | Définition | 
|---|---|
| RCE | Exécution de code à distance | 
| EoP | Violation de droits | 
| ID | Divulgation d'informations | 
| DoS | Déni de service | 
| N/A | (Not available) Classification non disponible | 

**3. Que signifient les entrées de la colonne *Références* ?**

Les entrées figurant dans la colonne *Références* du tableau détaillé des failles peuvent inclure un préfixe qui identifie l'organisation à laquelle la valeur de référence est associée.

| Préfixe | Référence | 
|---|---|
| A- | ID de bug Android | 
| QC- | Numéro de référence Qualcomm | 
| M- | Numéro de référence MediaTek | 
| N- | Numéro de référence NVIDIA | 
| B- | Numéro de référence Broadcom | 
| U- | Numéro de référence UNISOC | 

**4. Que signifie un astérisque (*) à côté de l'ID de bug Android dans la colonne *Références* ?**

Les problèmes qui ne sont pas accessibles au public sont précédés d'un astérisque (*) à côté de l'ID de référence correspondant. La mise à jour de ce problème est généralement incluse dans les derniers pilotes binaires pour les appareils Pixel disponibles sur le site Google Developers.

**5. Pourquoi les failles de sécurité sont-elles réparties entre ce bulletin et
les bulletins de sécurité des appareils / partenaires, comme le
bulletin Pixel ?**

Les failles de sécurité documentées dans ce bulletin de sécurité doivent déclarer le dernier niveau du correctif de sécurité sur les appareils Android. Il n'est pas nécessaire de déclarer un niveau de correctif de sécurité pour d'autres failles de sécurité documentées dans les bulletins de sécurité des appareils / partenaires. Les fabricants d'appareils Android et de chipsets peuvent également publier des informations sur les failles de sécurité spécifiques à leurs produits, comme Google, Huawei, LGE, Motorola, Nokia, ou Samsung.

## Versions

| Version | Date | Remarques | 
|---|---|---|
| 1.0 | 3 août 2026 | Publication du bulletin |
