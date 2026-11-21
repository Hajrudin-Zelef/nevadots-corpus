---
id: collect-261121-veille-ia/google-ecosystem/bulletin-sur-la-securite-d-android-aout-2026-security-android-open-source-project
title: "Bulletin sur la sécurité d'Android – Août 2026 | Security | Android Open Source Project"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-08-01"]
keywords: ["android", "securite", "bulletin", "google", "aout", "tous", "jour", "correctifs"]
source: https://source.android.com/docs/security/bulletin/2026/2026-08-01
source_anchor: ""
source_lines: [1, 77]
section: "Google_Ecosystem"
---
*Publié le 3 août 2026*

Ce bulletin sur la sécurité d'Android contient des informations sur les failles de sécurité qui affectent les appareils Android. Les niveaux de correctifs de sécurité du 5 août 2026 ou version ultérieure corrigent tous ces problèmes. Pour savoir comment vérifier le niveau du correctif de sécurité d'un appareil, consultez Vérifier la version d'Android installée et la mettre à jour.

Dans les 48 heures suivant la publication initiale de ce bulletin, nous publierons les correctifs de code source correspondants dans le dépôt du projet Android Open Source (AOSP). Nous mettrons ensuite à jour ce bulletin en y ajoutant les liens AOSP.

Pour en savoir plus sur les protections de la plate-forme de sécurité Android et de Google Play Protect, qui améliorent la sécurité de la plate-forme Android, consultez la section Mesures d'atténuation d'Android et de Google Play Protect.

Nous informons nos partenaires Android de tous les problèmes au moins un mois avant la publication du bulletin.

## Mesures de protection pour Android et les services Google

Il s'agit d'un résumé des mesures d'atténuation fournies par la plate-forme de sécurité Android et des protections des services, telles que Google Play Protect. Ces fonctionnalités réduisent la probabilité que des failles de sécurité puissent être exploitées avec succès sur Android.

- L'exploitation de nombreux problèmes sur Android est rendue plus difficile par les améliorations apportées aux versions plus récentes de la plate-forme Android. Nous encourageons tous les utilisateurs à passer à la dernière version d'Android, si possible.
- L'équipe de sécurité Android surveille activement les utilisations abusives via Google Play Protect et avertit les utilisateurs des applications potentiellement dangereuses. Google Play Protect est activé par défaut sur les appareils dotés des services Google Mobile. Il est particulièrement important pour les utilisateurs qui installent des applications en dehors de Google Play.

## Questions fréquentes et réponses

Cette section répond aux questions fréquentes qui peuvent se poser après la lecture de ce bulletin.

**1. Comment savoir si mon appareil a été mis à jour pour résoudre ces problèmes ?**

Pour savoir comment vérifier le niveau du correctif de sécurité d'un appareil, consultez Vérifier la version d'Android installée et la mettre à jour.

- Les niveaux de correctifs de sécurité du 1er août 2026 ou version ultérieure corrigent tous les problèmes associés au niveau du correctif de sécurité du 1er août 2026 et à tous les niveaux de correctifs précédents.

Les fabricants d'appareils qui incluent ces mises à jour doivent définir le niveau de la chaîne de correctifs comme suit :

- [ro.build.version.security_patch]:[2026-08-01]

Pour certains appareils équipés d'Android 10 ou version ultérieure, la mise à jour du système Google Play comporte une chaîne de date correspondant au niveau du correctif de sécurité du 1er août 2026. Pour savoir comment installer les mises à jour de sécurité, consultez cet article.

**2. Pourquoi ce bulletin comporte-t-il deux niveaux de correctif de sécurité ?**

Ce bulletin comporte deux niveaux de correctifs de sécurité afin que les partenaires Android puissent corriger plus rapidement un sous-ensemble de failles similaires sur tous les appareils Android. Nous encourageons les partenaires Android à corriger tous les problèmes mentionnés dans ce bulletin et à utiliser le dernier niveau de correctif de sécurité.

Nous encourageons les partenaires à regrouper les correctifs pour tous les problèmes qu'ils traitent dans une seule mise à jour.

**3. Que signifient les entrées de la colonne *Type* ?**

Les entrées de la colonne *Type* du tableau sur les détails des failles font référence à la classification de la faille de sécurité.

| Abréviation | Définition | 
|---|---|
| RCE | Exécution de code à distance | 
| EoP | Violation de droits | 
| ID | Divulgation d'informations | 
| DoS | Déni de service | 
| N/A | (Not available) Classification non disponible | 

**4. Que signifient les entrées de la colonne *Références* ?**

Les entrées figurant dans la colonne *Références* du tableau détaillé des failles peuvent inclure un préfixe qui identifie l'organisation à laquelle la valeur de référence est associée.

| Préfixe | Référence | 
|---|---|
| A- | ID de bug Android | 
| QC- | Numéro de référence Qualcomm | 
| M- | Numéro de référence MediaTek | 
| N- | Numéro de référence NVIDIA | 
| B- | Numéro de référence Broadcom | 
| U- | Numéro de référence UNISOC | 

**5. Que signifie un astérisque (*) à côté de l'ID du bug Android dans la colonne *Références* ?**

Les problèmes qui ne sont pas disponibles publiquement sont indiqués par un astérisque (*) à côté de l'ID de référence correspondant. La mise à jour pour ce problème est généralement incluse dans les derniers pilotes binaires pour les appareils Pixel disponibles sur le site Google Developers.

**6. Pourquoi les failles de sécurité sont-elles réparties entre ce bulletin et les bulletins de sécurité des appareils / partenaires, comme le bulletin Pixel ?**

Les failles de sécurité documentées dans ce bulletin de sécurité doivent déclarer le dernier niveau du correctif de sécurité sur les appareils Android. Il n'est pas nécessaire de déclarer un niveau de correctif de sécurité pour d'autres failles de sécurité documentées dans les bulletins sur la sécurité des appareils / partenaires. Les fabricants d'appareils Android et de chipsets peuvent également publier des informations sur les failles de sécurité spécifiques à leurs produits, comme Google, Huawei, LGE, Motorola, Nokia ou Samsung.

## Versions

| Version | Date | Remarques | 
|---|---|---|
| 1.0 | 3 août 2026 | Bulletin publié |
