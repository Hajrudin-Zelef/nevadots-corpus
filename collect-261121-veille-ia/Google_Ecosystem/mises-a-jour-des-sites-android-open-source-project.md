---
id: collect-261121-veille-ia/google-ecosystem/mises-a-jour-des-sites-android-open-source-project
title: "Mises à jour des sites | Android Open Source Project"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2025-03-27"]
keywords: ["jour", "mise", "android", "version", "source", "alimentation", "audio", "page"]
source: https://source.android.com/docs/whatsnew/site-updates
source_anchor: ""
source_lines: [1, 345]
section: "Google_Ecosystem"
---
Cette page liste les modifications mensuelles apportées au site et les mises à jour de la documentation.

## Modifications apportées à AOSP

À partir du 27 mars 2025, la dernière branche de version sera toujours référencée par le nouveau fichier manifeste `android-latest-release`, qui peut être utilisé directement avec Repo. Pour créer et contribuer dans AOSP, utilisez `android-latest-release`. Le fichier manifeste `android-latest-release` est défini sur la dernière branche de version d'AOSP, `android17-release`. Pour en savoir plus, consultez À propos de android-latest-release.

## Demander la source Android

Pour obtenir le code source des composants tiers distribués par Google sous forme binaire dans un produit Android, consultez Obtenir le code source Android.

## Octobre 2026

Voici les modifications apportées au site en octobre. Vous trouverez les bulletins publiés régulièrement, tels que les bulletins sur la sécurité d'Android, sur leurs pages respectives.

| Emplacement | Modifier | 
|---|---|
| Architecture | Ajout de la section Exécution de proxy dans Android, qui détaille les exigences du noyau, l'intégration des hooks du fournisseur et les principes de conception pour éviter l'inversion de priorité dans les transactions de binder. | 
|  | Mise à jour du processus de publication de l'image de noyau générique (GKI) et de la section Demander une nouvelle version pour documenter la fréquence de publication de la GKI de 16 ko, les branches compatibles et le processus de nouvelle version d'urgence, et mise à jour de la section Taille de page de 16 ko. | 
| Automobile | Mise à jour de la gestion de l'alimentation et de la règle d'alimentation avec un diagramme de machine d'état d'alimentation SVG illustrant le flux d'annulation de l'arrêt, clarification du comportement du groupe de règles d'alimentation lorsque l'arrêt est annulé, et documentation des chemins d'accès aux fichiers de règles d'alimentation du fournisseur pour les remplacements de règles système. | 
|  | Mise à jour de la section Routage audio multizone pour clarifier les périphériques de sortie audio MUMD (Multi-User Multi-Display) et les règles d'attribution des zones audio. | 
|  | Mise à jour de la page Applications multimédias avec la bibliothèque d'applications pour voitures avec le lien de téléchargement de la dernière version de TestMediaCalApp (16338213). | 
|  | Mise à jour de la bibliothèque du framework de test SDV avec un tableau de décision comparant les cibles de test d'hôte et de test unitaire, mise à jour de la création de tests du système SDV, ajout de listes de répertoires d'exemples de référence sur les exemples de code de base et mise à jour de la page Résoudre les problèmes d'échec de l'API middleware avec les étapes de résolution des problèmes de perte de données d'abonné et de dépassement de tampon. | 
|  | Mise à jour des sections Configurer un IDE pour le développement SDV, Déboguer un SDV et Créer et déployer des packs de services pour les aligner sur la refonte d'Android Studio for Platform (ASfP), y compris le débogage LLDB intégré, les workflows de déploiement APEX et les partitions de packs de services compatibles. | 
|  | Mise à jour de DriverUI pour ajouter des conseils de dépannage pour la transparence de la composition alpha et le mélange de la fenêtre d'affichage HAR, et mise à jour de Premiers pas avec Display Safety. | 
|  | Mise à jour de l'API Metrics Configuration Generator (MCG) pour documenter les points de terminaison et les configurations de métriques de la version 2 ( `/api/v2/` ), et mise à jour de Cloud Telemetry Simulation, du guide de déploiement de Cloud Simulator et du guide de l'utilisateur de Cloud Simulator pour les aligner sur les configurations des variables de backend et les états de simulation. | 
| Compatibilité | Réorganisation de la documentation sur les tests audio CTS Verifier dans une structure axée sur l'équipement, dans les tests de fréquence audio et les guides des périphériques. | 
|  | Déplacement de la section "Optimiser la CTS" de source.android.com vers le centre d'aide GMS. | 
|  | Mise à jour des tests ITS de l'appareil photo et des notes de version de la suite de tests d'image de l'appareil photo Android 17 avec les dépendances du package Python 3.14. | 
| Cuttlefish | Mise à jour de Cuttlefish : redémarrage et réinitialisation pour supprimer l'option `--netsim_bt` obsolète. | 
| Écran | Ajout de Courbes de gain adaptatives de marge (HAGC), détaillant l'intégration de SMPTE 2094-50 pour le rendu des courbes de gain adaptatives de marge HDR. | 
| Enterprise | Mise à jour du framework de résolution des règles relatives aux appareils pour documenter la stratégie de résolution par identifiant pour `PolicyIdentifier` . | 
| Performances | Ajout de la section Budgets de mémoire système, qui définit les principes de budgétisation de la mémoire, les méthodologies de mesure de la stabilisation du démarrage et les consignes de configuration minimale par rapport à une configuration à faible RAM. | 
|  | Ajout de Optimiser le code et les ressources de la plate-forme avec R8, qui détaille la minification du code et des ressources R8, les options de configuration ProGuard, le traçage des références de test et la modélisation des règles d'API. | 
| Sécurité | Publication du bulletin sur la sécurité d'Android d'octobre 2026, du bulletin sur la mise à jour d'Android Automotive OS d'octobre 2026, du bulletin sur la sécurité de Wear OS d'octobre 2026, du bulletin sur Android XR d'octobre 2026 et du bulletin sur la mise à jour des appareils Pixel d'octobre 2026, et mise à jour des remerciements concernant la sécurité d'Android. | 
|  | Mise à jour du bulletin sur la sécurité d'Android de septembre 2026 pour supprimer CVE-2026-49921, CVE-2026-45529, CVE-2026-28627 et CVE-2026-28666, du bulletin sur la sécurité d'Android de juin 2026 pour supprimer CVE-2026-0069 et CVE-2026-0094, et du bulletin sur la sécurité d'Android de mars 2026 pour supprimer CVE-2025-48654. | 
|  | Restauration des pages de documentation obsolètes sur le matériel, Gatekeeper et Keystore qui avaient été abandonnées. | 
| Tests | Mise à jour d'OmniLab Android Test Station pour clarifier le formatage des valeurs pour les options basées sur le temps. | 

## Septembre 2026

Voici les modifications apportées au site en septembre. Vous trouverez les bulletins publiés régulièrement, tels que les bulletins sur la sécurité d'Android, sur leurs pages respectives.

| Emplacement | Modifier | 
|---|---|
| Architecture | Ajout des builds de version du bootloader générique (GBL), calqués sur les builds de version de l'image de noyau générique (GKI), fournissant des liens de téléchargement, des artefacts certifiés, des builds EFI pour les développeurs et des instructions de compilation de sources pour Android 17 et Android 16, et mise à jour de Déployer le GBL. | 
|  | Mise à jour des pages Annotations dans AIDL et Backends AIDL pour documenter le mappage Rust `Option<T>` pour les types AIDL non-`Default` (tels que`IBinder` , les interfaces et`ParcelFileDescriptor` ) dans les champs parcelables et les paramètres`out` sans`@nullable` , tout en clarifiant l'application forcée de non-null à l'exécution. | 
|  | Mise à jour de la section Présentation de la configuration pour documenter la configuration de l'appareil au démarrage anticipé ( `init_dev_config` ) introduite dans Android 17 pour l'initialisation dynamique des propriétés par SKU, et mise à jour de la section APEX du fournisseur pour décrire la sélection dynamique de l'APEX du fournisseur et l'abandon de la sélection sysprop persistante. | 
|  | Ajout de nouvelles versions et abandon des anciennes versions sur les pages Versions android17-6.18, Versions android16-6.12, Versions android16-6.12 obsolètes, Versions android15-6.6, Versions android15-6.6 obsolètes, Versions android14-6.1, Versions android14-6.1 obsolètes, Versions android13-5.15 et Versions android13-5.10, et mise à jour de la présentation sur la page Versions d'images de noyau génériques (GKI). | 
| Automobile | Mise à jour de la section Moteur de règles audio configurable pour documenter le respect strict des énumérations, les critères de correspondance des attributs et de l'utilisation audio, les règles de remplacement et le routage audio multizone distinguant les points de terminaison du moteur CAP des attributions de zones CarAudioService. | 
|  | Mise à jour de la page Véhicule défini par logiciel pour ajouter une matrice de référence de la version publique d'AOSP d'Android 17 (26Q2) détaillant les profils d'exécution multi-VM, la génération de code VSIDL indépendante du transport, les protocoles ECU externes et les agents d'exécution APEX. | 
|  | Mise à jour de la section AAOS SDV system architecture (Architecture système SDV AAOS) pour expliquer comment les définitions déclaratives VSIDL et de tampon de protocole sont compilées à l'aide de `vsidlc` dans les liaisons de bibliothèque cliente entre les transports inter-VM et les protocoles ECU externes. | 
|  | Mise à jour de la terminologie et du glossaire AAOS SDV pour ajouter un tableau de mappage de l'industrie automobile comparant les SWC AUTOSAR, ara::com, SOME/IP SD, COVESA VSS, les ECU et le rendu de cluster ISO 26262/ASIL aux concepts AAOS SDV. | 
|  | Mise à jour de la présentation de l'intégration SOME/IP, de la section Écrire un fichier de mappage SOME/IP et de la section Écrire une définition de type SOME/IP pour mapper les règles d'encodage de charge utile AUTOSAR ARXML et `vsomeip` aux types VSIDL. | 
|  | Mise à jour des guides d'intégration de la plate-forme avec des liens structurés vers les guides d'attestation SDV Core, SDV Media, QNX/QVM, SOME/IP, SDV Gateway sur IVI et DICE pour la version 26Q2. | 
|  | Ajout de repères de navigation sur les anciennes pages IVI à fort trafic pour guider les développeurs vers les équivalents AAOS SDV de nouvelle génération, y compris Présentation de la HAL du véhicule (VHAL), Présentation des écrans automobiles, API du tableau de bord, API Multi-Display Communications et Présentation du système de vue étendue (EVS). | 
|  | Mise à jour de la présentation de l'UI évolutive et de Qu'est-ce qu'Android Automotive ? pour simplifier les conseils sur le cockpit numérique unifié et les descriptions des services de véhicules multi-VM. | 
| Compatibilité | Mise à jour des notes de version de CTS 17 pour ajouter une colonne "Exigence/API" qui met en correspondance les exigences et les API du CDD avec de nouveaux scénarios de test dans les modules CTS. | 
|  | Mise à jour de la section Tester les images de l'appareil photo à l'aide de l'automatisation pour documenter les tablettes qualifiées pour les tests de scènes à large gamme, y compris la Honor Pad 20 et la Samsung Galaxy Tab S10 FE. | 
|  | Mise à jour des notes de version de l'ITS de l'appareil photo Android 17 avec les dépendances de packages Python mises à jour, y compris numpy, OpenCV et scipy. | 
|  | Mise à jour de la section Exécuter les tests de lecture multimédia CTS Verifier pour spécifier que l'outil d'analyse de lecture (PAT) mobile est requis et que le PAT TV n'est pas compatible. | 
|  | Mise à jour de la section Configurer les tests automatisés CTS avec des instructions pour télécharger et restaurer des packages CTS fragmentés définis par le contenu ( `android-chunked-cts.zip` ). | 
| Connectivité | Mise à jour de la section Découpage de réseau 5G pour documenter le composant de descripteur de trafic ConnectionCapability introduit dans Android 17 (Radio HAL AIDL 2.5+), y compris les tables de mappage NetworkCapabilities et les consignes de configuration de l'opérateur. | 
|  | Mise à jour de la section Étalonnage de la présence pour aligner le langage des exigences, ajout d'un lien vers le dernier CDD et réorganisation de la navigation dans la section. | 
|  | Mise à jour de l'intégration de l'opérateur pour le transfert d'eSIM afin d'ajouter des conseils pour l'intégration à grande échelle. | 
|  | Mise à jour de la spécification de mesure de distance hors bande version 3 pour standardiser les canaux de communication basés sur la connexion et affiner les définitions de type pour l'UWB, le Wi-Fi NAN RTT, le Wi-Fi PD et le sondage de canal BLE. | 
| Performances | Mise à jour de la section Congélateur d'applications mises en cache avec les API de plate-forme officielles, les conditions d'utilisation et les bonnes pratiques pour les appelants concernant le gel et le dégel des processus. | 
| Autorisations | Mise à jour des règles de fuseau horaire avec un avis d'abandon pour les mises à jour tzdata d'Android 10 (Q). | 
| Sécurité | Publication du bulletin sur la sécurité d'Android de septembre 2026, du bulletin sur les mises à jour d'Android Automotive OS de septembre 2026, du bulletin sur la sécurité de Wear OS de septembre 2026, du bulletin sur Android XR de septembre 2026, du bulletin sur les mises à jour des Pixel de septembre 2026 et des notes de version sur la sécurité d'Android 17, et mise à jour des remerciements concernant la sécurité d'Android. | 
|  | Ajout de la section Correctifs de sécurité supplémentaires détaillant le cycle de vie de la livraison et de l'application des correctifs de sécurité supplémentaires. | 
|  | Mise à jour de la page Attestation de clé et d'ID pour documenter le champ `is_lost_device` dans la spécification de mappage CDDL ProvisioningInfo de l'attestation de clé, et mise à jour des conseils sur les clés et certificats de provisionnement de clés à distance (RKP). | 
|  | Mise à jour du bulletin sur la sécurité d'Android de septembre 2026 pour supprimer CVE-2026-58820, du bulletin sur la sécurité d'Android de juin 2026 pour supprimer CVE-2025-22424, CVE-2025-22426 et CVE-2025-48600, et du bulletin sur la sécurité de Wear OS de septembre 2024 pour supprimer CVE-2024-40664. | 
| Configuration | Mise à jour de la page Noms de code, balises et numéros de version pour ajouter des tags d'ID de build pour les versions de rétroportage de sécurité de septembre 2026. | 
| Tests | Mise à jour de Cloud Orchestration dans OmniLab ATS pour documenter les options de configuration des instances d'appareils virtuels, y compris `--max_orchestration_virtual_devices` , l'allocation de processeur et de mémoire, les types de cartes SIM du simulateur de modem et les cartes SD. | 
| Mises à jour | Suppression des pages "Android Upgrade Party" pour les mises à jour de l'OS et "Invitation à la mise à niveau d'Android". Ces deux fonctionnalités sont obsolètes et ne sont pas remplacées. | 

## Août 2026

Il s'agit des modifications apportées au site en août. Vous trouverez les bulletins publiés régulièrement, tels que les bulletins sur la sécurité d'Android, sur leurs pages respectives.

| Emplacement | Modifier | 
|---|---|
| Architecture | Ajout de la section Processus de respin GBL, qui détaille le processus de demande et de publication des builds de version candidate (RC) de respin du bootloader générique (GBL). | 
|  | Mise à jour de Déployer GBL pour inclure les versions candidates (RC) précompilées du bootloader générique (GBL) Android 17 et lien vers le workflow de demande de nouvelle version. | 
|  | Mise à jour de Stable AIDL pour documenter les en-têtes de version au niveau du module pour les backends C++ et NDK ( `<module_name>/version.h` ) afin de vérifier les versions d'interface au moment de la compilation sans appels RPC d'exécution. | 
|  | Ajout de nouvelles versions et abandon d'anciennes versions sur les pages Builds android17-6.18, Builds obsolètes android17-6.18, Builds android16-6.12, Builds android15-6.6, Builds android14-6.1, Builds android14-5.15, Builds android13-5.15 et Builds android12-5.10, et mise à jour de la présentation sur la page Builds d'images de noyau génériques (GKI). | 
| Automobile | Mise à jour de la section Gestion de l'alimentation pour clarifier la phase d'avertissement précoce `PRE_SHUTDOWN_PREPARE` , les limites du minuteur, les critères d'achèvement de l'écouteur et le flux d'annulation lors de l'arrêt du véhicule. | 
|  | Mise à jour de la présentation et de la section Maximiser la conformité des tests avec les invariants WindowManager pour aligner les conseils sur l'UI évolutive d'AAOS sur les consignes destinées aux développeurs d'applications et les contraintes de mise en page de WindowManager. | 
|  | Mise à jour de la section Options pour les développeurs sécurisées pour détailler les restrictions `config.xml` (`config_restricted_dev_options_for_all_users` ), les paramètres`BuildNumberPreferenceController` et les exigences concernant le certificat de droits de développeur (DRC) pour AAOS. | 
|  | Mise à jour de la section État et provisionnement du maillage pour clarifier le provisionnement de l'attestation de VM, la gestion des certificats et la validation de l'identité pour les véhicules définis par logiciel (SDV). | 
|  | Mise à jour des sections Architecture, Présentation et Plate-forme de référence pour marquer la cible de la plate-forme de référence `trout` comme obsolète et documenter son abandon. | 
| Compatibilité | Mise à jour des notes de version de l'ITS de l'appareil photo Android 17 pour mettre à jour les versions et les dépendances des packages Python épinglés pour la suite de tests d'image de l'appareil photo. | 
|  | Mise à jour de la page Acheter et configurer un boîtier Gen2 pour ajouter des instructions, des consignes pour l'opérateur et un script de test de charge téléchargeable ( `ServoReliability_Gen2ITS.zip` ) permettant d'effectuer des tests de fiabilité du servo et du contrôleur, et documenter les autorisations du port série Linux hôte et la configuration du groupe d'appel pour les bancs d'essai ITS de la caméra Gen2. | 
|  | Mise à jour de Exécuter des tests CTS-V pour supprimer la section obsolète "Étalonnage du champ de vision de la caméra". | 
|  | Mise à jour des téléchargements de la Compatibility Test Suite pour ajouter les versions d'août 2026, y compris 17 R2, 16.1_R4, 16_R6, 15_R9 et 14_R13. | 
|  | Mise à jour du document de définition de compatibilité Android 17 pour supprimer l'exigence de profil géré en double [C-1-11] et aligner les spécifications de latence audio en boucle sur les exigences de test. | 
|  | Mise à jour de la section Configurer les tests automatisés CTS avec des liens de configuration et de configuration de virtualenv. | 
| Graphismes | Mise à jour de la section Verrouillage de tampon non signalé avec AutoSingleLayer avec une image de trace et une description de référence mises à jour. | 
| Performances | Mise à jour de Limiteur de mémoire pour documenter les intervalles de niveau de RAM, les paramètres de plafond de mémoire ( `memoryCeilingKb` ), les comportements par défaut du système et les chemins de configuration pour Android 17, tout en supprimant les références obsolètes aux remplacements de fournisseurs. | 
| Autorisations | Mise à jour des sections Rôles Android et Streaming d'application associée pour ajouter des ID d'ancrage et des liens profonds aux rôles Android principaux. | 
| Sécurité | Publication du bulletin sur la sécurité d'Android d'août 2026, du bulletin sur la mise à jour d'Android Automotive OS d'août 2026, du bulletin sur la sécurité de Wear OS d'août 2026, du bulletin sur Android XR d'août 2026, du bulletin sur la mise à jour des Pixel d'août 2026 et du bulletin sur la sécurité de la Pixel Watch d'août 2026, et mise à jour des Remerciements concernant la sécurité d'Android. | 
|  | Mise à jour du bulletin sur la sécurité d'Android de juin 2026 et du bulletin sur la sécurité d'Android de mars 2026 pour supprimer les entrées CVE retirées (y compris CVE-2026-0010). | 
|  | Mise à jour de la section Attestation de clé et d'ID pour expliquer comment vérifier la compatibilité de l'attestation d'ID d'appareil Keystore. | 
|  | Mise à jour de la limitation du débit et de Weaver pour rétablir le délai avant verrouillage de l'écran documenté à 36 heures après 12 tentatives infructueuses et lien vers les exigences de limitation du débit du CDD Android 17. | 
| Configuration | Mise à jour de Compiler avec Jack pour marquer la chaîne d'outils Jack avec une icône d'abandon dans la navigation. | 
|  | Mise à jour des références de cible de compilation de `trunk_staging` à`aosp_current` dans la documentation SAC, y compris Définir les valeurs des options de lancement de fonctionnalités et Utiliser Cuttlefish avec une taille de page de 16 Ko sur ARM64. | 
| Tests | Mise à jour du guide de mise à niveau vers OmniLab Android Test Station 2.0 et d'OmniLab Android Test Station pour ajouter des conseils sur les suites de tests protégées par mot de passe dans ATS 2.0. | 

## Juillet 2026

Voici les modifications apportées au site en juillet. Vous trouverez les bulletins publiés régulièrement, tels que les bulletins sur la sécurité d'Android, sur leurs pages respectives.

| Emplacement | Modifier | 
|---|---|
| Architecture | Ajout d'une référence à la fin de la prise en charge de Mainline pour les versions d'Android antérieures sur Mainline. | 
|  | Ajout d'une sortie JSON pour les builds de version android17-6.18. | 
|  | Mise à jour de la section Partition de démarrage générique pour indiquer que le vbmeta chaîné pour les images `boot` et`init_boot` est vivement recommandé pour permettre les mises à jour OTA indépendantes. | 
|  | Mise à jour de Android Health (anciennement Health Fitness) pour clarifier que les deux noms font référence au même module Mainline. | 
| Automobile | Mise à jour de la présentation de l'UI évolutive pour souligner que l'UI évolutive est fortement recommandée et requise pour la conformité CTS dans Android Automotive OS (AAOS) 16 et versions ultérieures. | 
|  | Mise à jour de la section Appareils Pixel en tant que plates-formes de développement pour préciser que les builds Android Automotive OS (AAOS) sur téléphone sont obsolètes à partir d'Android 16 (B). | 
|  | Mise à jour des versions d'Android Automotive OS pour ajouter la version 26Q2 d'AAOS. | 
| Compatibilité | Mise à jour de la présentation de Camera ITS pour ajouter des conseils sur les tests de sous-caméras, des liens vers l'API multicaméra et la division des tests pour réduire le temps d'exécution des tests. | 
|  | Ajout de la section Exécuter les tests Mobly du gestionnaire de NPU, qui documente l'exécution des tests Mobly du gestionnaire de NPU CTS et les paramètres configurables. | 
|  | Mise à jour des tests ITS de l'appareil photo pour ajouter des descriptions d'échec rapide aux tests `scene0` . | 
|  | Mise à jour de la section Audio USB multicanal pour actualiser les spécifications d'alimentation du mixeur stéréo à six voies et les liens de remplacement valides. | 
|  | Mise à jour de la section Développement de la CTS pour refléter le calendrier de publication et les informations sur la branche de développement de test Gerrit du partenaire CTS 17. | 
| Connectivité | Mise à jour des exigences HCI pour corriger les définitions de plage de bins RSSI pour les événements de surveillance de l'état du contrôleur de rapport de qualité Bluetooth (BQR). | 
|  | Mise à jour de la section Prendre en charge les applications d'appel tierces pour documenter les exigences d'intégration obligatoires des journaux d'appels VoIP à partir d'Android 17, y compris les règles de conformité, la résolution des intents et l'application de la CTS. | 
|  | Mise à jour de l'intégration de l'opérateur pour le transfert d'eSIM pour référencer `ManageSubscription(5: ACTIVATE TERMINAL ICCID)` au lieu de`manageSubscription(4: UPDATE SUBSCRIPTION)` . | 
|  | Mise à jour de Wi-Fi Aware pour référencer le nouveau PDF sur l'architecture Wi-Fi Aware pour Android 17 et versions ultérieures. | 
| Cuttlefish | Mise à jour de la page Créer un appareil hybride Cuttlefish pour ajouter des recommandations d'environnement hôte (telles que les instances Bare Metal Google Cloud C4A avec OmniLab ATS) pour les déploiements CHD ARM64 évolutifs, et ajout d'une note indiquant que les appareils hybrides Cuttlefish sont compatibles avec Android 15 et versions ultérieures. | 
| Performances | Mise à jour de la section Limiteur de mémoire pour modifier la description et le format de la commande manuelle. | 
| Sécurité | Publication du bulletin sur la sécurité d'Android de juillet 2026, du bulletin sur les mises à jour d'Android Automotive OS de juillet 2026, du bulletin sur la sécurité de Wear OS de juillet 2026, du bulletin sur Android XR de juillet 2026 et du bulletin sur les mises à jour des Pixel de juillet 2026, et mise à jour des remerciements concernant la sécurité d'Android. | 
|  | Ajout du schéma de signature des APK v3.2 pour documenter le schéma de signature des APK hybrides introduit dans Android 17 afin de prendre en charge la cryptographie post-quantique (PQC). | 
|  | Mise à jour d'ASPIRE pour ajouter l'article UCSan. | 
|  | Mise à jour de la page Attestation de clé et d'ID pour ajouter la version 500 du schéma d'attestation. | 
| Tests | Mise à jour de la section Mappage des tests pour documenter les attributs de validation `file_patterns` et les validations au moment de la compilation sur AOSP Gerrit, ainsi que les importations au niveau du groupe de tests avec des exemples de code JSON. | 
|  | Mise à jour du guide de mise à niveau d'OmniLab Android Test Station 2.0 et de Cloud Orchestration dans OmniLab ATS pour refléter la disponibilité générale et la préparation à l'intégration. | 
|  | Mise à jour des notes de version d'OmniLab Android Test Station pour la version R53. | 

## Juin 2026

### Version 17 d'Android

Android 17 est disponible. Pour connaître les modifications apportées à Android 17, consultez les notes de version d'Android 17.

### Autres mises à jour du site en juin

Voici les modifications apportées au site en juin. Vous trouverez les bulletins publiés régulièrement, tels que les bulletins sur la sécurité d'Android, sur leurs pages respectives.

| Emplacement | Modifier | 
|---|---|
| Architecture | Ajout de nouvelles versions et abandon d'anciennes versions sur les pages Builds android17-6.18, Builds obsolètes android17-6.18, Builds android16-6.12, Builds android15-6.6, Builds android14-6.1, Builds android14-5.15, Builds android13-5.15 et Builds android12-5.10, et mise à jour de la présentation sur la page Builds d'images de noyau génériques (GKI). | 
|  | Mise à jour de la section Image système partagée pour clarifier les exigences de dissociation des APK et d'inclusion des modules pour Android 16 et versions ultérieures. | 
|  | Mise à jour des liens prédéfinis et des conseils de déploiement sur Déployer GBL pour Android 17. | 
|  | Mise à jour du module de chiffrement GKI certifiable FIPS 140-3 pour inclure des informations et des détails de certification pour le noyau `android17-6.18` . | 
| Automobile | Mise à jour des flux d'intégration dans le guide d'interaction vocale pour remplacer le PNG du déclencheur de mot clé par un graphique vectoriel évolutif (SVG). | 
|  | Mise à jour de l'API Instrument Cluster et de l'infodivertissement embarqué pour détailler l'exposition du nom de l'application sur l'hôte de l'application du tableau de bord. | 
|  | Mise à jour de Gestion de l'alimentation et de Règle d'alimentation pour refléter la refactorisation du service de gestion de l'alimentation de la voiture (CPMS) et du daemon de règle d'alimentation de la voiture (CPPD). | 
|  | Mise à jour de Qu'est-ce qu'Android Automotive ? et In-Vehicle Infotainment (Infodivertissement embarqué) pour introduire des liens vers la documentation sur les véhicules définis par logiciel (SDV) et mettre à jour les titres et le style de l'IVI. | 
|  | Mise à jour de la présentation de l'UI évolutive pour ajouter des GIF de configuration illustrant les comportements de mise en page. | 
|  | Mise à jour de la documentation sur les véhicules définis par logiciel (SDV) dans Véhicule défini par logiciel, Télécharger, compiler et exécuter, Emplacement du code, Développement cloud, Configuration de l'organisation, Accéder aux dépôts GitHub, Implémenter la pile SOME/IP, Guide de déploiement, Déploiement, Catalogues de signaux de véhicules et Compiler et déployer des bundles de services pour refléter la migration vers GitHub et l'accès simplifié aux sources. | 
|  | Mise à jour de la page Utiliser la passerelle SDV sur l'IVI avec les recommandations concernant l'AID (Android Interface Definition) et déplacement des exigences courantes d'identité et d'attestation de la VM SDV vers des pages partagées dans Bootloader SDV Core, Transfert de l'interface CDI HLOS Android, Compatibilité du bootloader avec l'identité et l'attestation de la VM SDV et Configurer l'identité pour les services natifs. | 
|  | Ajout d'Android Automotive 26Q2 avec des informations sur la version et des mises à jour de fonctionnalités. | 
|  | Mise à jour de la section État et provisionnement du maillage pour ajouter des figures illustrant les flux de provisionnement d'attestation de VM. | 
|  | Mise à jour de la section Définir l'architecture du service pour améliorer l'exemple de code et les définitions de service `ClimateControl` . | 
| Compatibilité | Mise à jour de la définition de compatibilité Android 17 pour utiliser des valeurs numériques pour les références de la classe de performances multimédias (MPC). | 
|  | Mise à jour de la section Exécuter les tests CTS-V pour modifier le seuil de test du champ de vision de la caméra. | 
|  | Mise à jour de la définition de compatibilité Android 16 et de la définition de compatibilité Android 17 pour aligner les exigences sur les propositions de spécifications, affiner le langage et appliquer des corrections post-examen aux exigences concernant les appareils portables, la compatibilité Web, le multimédia, les outils pour les développeurs, la connectivité matérielle et le modèle de sécurité. | 
|  | Mise à jour de toutes les versions du document de définition de compatibilité Android et de la définition de compatibilité Android 17. | 
|  | Mise à jour des téléchargements de la Compatibility Test Suite pour ajouter les téléchargements de la CTS Android 17, les tags de synchronisation de la source et les références d'arborescence. | 
|  | Publication des notes de version de CTS 17. | 
|  | Mise à jour de la page Configurer les tests automatisés CTS pour ajouter des liens vers les shims CTS Android 17. | 
|  | Mise à jour de la présentation de l'ITS de l'appareil photo pour ajouter des instructions de configuration des options pour les développeurs de l'appareil testé et des outils de vérification de l'alignement des scènes ( `check_alignment.py` ). | 
|  | Mise à jour des tests ITS de l'appareil photo pour diviser les descriptions de test en sections structurées par configuration. | 
| Connectivité | Mise à jour des exigences HCI vers les exigences HCI Bluetooth de Google v1.06. | 
|  | Mise à jour de la page Implémenter des onglets personnalisés Android pour la connexion au portail captif afin d'ajouter une clause de non-responsabilité concernant les commentaires pour les onglets personnalisés Android dans les portails captifs. | 
| Écran | Mise à jour de Fenêtrage du bureau, Compatibilité avec le mode multifenêtre, Présentation du multi-écran et Compatibilité avec les décorations système avec des détails améliorés sur le fenêtrage du bureau et des conseils sur le multi-écran. | 
| Performances | Ajout de NPU Manager et mise à jour de Optimisation des performances Android pour documenter l'architecture, l'intégration et la surveillance des performances de NPU Manager. | 
|  | Ajout du limiteur de mémoire pour fournir un guide de configuration et des définitions correctes pour la gestion de la mémoire des processus visibles. | 
| Autorisations | Mise à jour des rôles Android pour inclure le rôle `SUPERVISION` , ainsi que les autorisations et les exigences associées. | 
| Référence | Mise à jour de la documentation Tradefed. | 
| Sécurité | Publication du bulletin sur la sécurité d'Android de juin 2026, du bulletin sur les mises à jour d'Android Automotive OS de juin 2026, du bulletin sur la sécurité de Wear OS de juin 2026, du bulletin sur la sécurité d'Android XR de juin 2026 et des remerciements concernant la sécurité d'Android. Les bulletins de la version preview pour les partenaires de juin ont été convertis en bulletins publics, et les présentations ont été mises à jour dans le bulletin sur les mises à jour d'Android Automotive OS, les bulletins sur la sécurité d'Android, les bulletins sur la sécurité de Wear et le bulletin sur la sécurité d'Android XR. | 
|  | Mise à jour du bulletin sur la sécurité d'Android de juin 2026 pour ajouter des liens vers la version AOSP et mettre à jour le tableau des versions de sécurité. | 
|  | Mise à jour du bulletin sur la sécurité d'Android de mars 2026 pour supprimer la CVE-2026-0007 du bulletin. | 
|  | Publication du bulletin de mise à jour Pixel de juin 2026 et mise à jour des bulletins de mise à jour Pixel avec des informations sur les failles de sécurité affectant les appareils Pixel compatibles et les améliorations fonctionnelles. | 
|  | Publication du bulletin de sécurité de la Pixel Watch de juin 2026 et mise à jour des bulletins de sécurité de la Pixel Watch, détaillant les correctifs de sécurité, les correctifs des composants Qualcomm et les liens vers les aperçus des langues. | 
|  | Publication des notes de version de sécurité d'Android 17, qui détaillent les fonctionnalités de sécurité, le renforcement et les corrections de failles introduits dans Android 17. | 
| Configuration | Mise à jour de la section Noms de code, tags et numéros de version avec les numéros de version, les noms de branche et les tags des versions AOSP et des versions de préproduction. | 
| Tests | Mise à jour du guide de mise à niveau d'OmniLab Android Test Station 2.0 avec des instructions et des étapes de migration pour la suite de tests de compatibilité multi-appareil (MCTS) d'Android 17. | 
| Nouveautés | Publication des premières notes de version sur les notes de version d'Android 17 et mise à jour de la présentation des notes de version. | 

## Mai 2026

Voici les modifications apportées au site en mai. Vous trouverez les bulletins publiés régulièrement, tels que les bulletins sur la sécurité d'Android, sur leurs pages respectives.

| Emplacement | Modifier | 
|---|---|
| Architecture | Mise à jour des consignes concernant l'API AIDL avec les bonnes pratiques pour développer des interfaces HAL stables et rétrocompatibles, y compris des recommandations pour les parcelables de requête/réponse uniques et les opérations asynchrones. | 
|  | Ajout de nouvelles versions aux versions android16-6.12, aux versions android15-6.6, aux versions android14-6.1, aux versions android14-5.15, aux versions android13-5.15 et aux versions android12-5.10. | 
| Automobile | Ajout de la section Gérer les ressources audio pendant la suspension, qui détaille la robustesse de la suspension de la HAL audio et les considérations relatives aux systèmes d'assistant pour empêcher la détection du mot clé de bloquer la suspension du système. | 
|  | Ajout de la section Compatibilité avec l'écran de la voiture pour documenter les exigences de compatibilité des applications AAOS. | 
|  | Mise à jour de la documentation sur les plug-ins d'interface utilisateur pour l'automobile afin de supprimer la fonctionnalité de personnalisation de gModal (boîte de dialogue modale de contenu ou AppStyledDialog) pour l'aligner sur l'implémentation. | 
|  | Mise à jour de la documentation sur les jetons de conception OEM pour refléter les derniers détails d'implémentation de la personnalisation des applications GAS. | 
|  | Mise à jour de la section Applications multimédias avec la bibliothèque d'applications pour voitures pour inclure des exemples de code permettant de lancer des activités de la bibliothèque d'applications pour voitures. | 
| Compatibilité | Mise à jour de la section Obtenir un boîtier de fusion de capteurs avec des instructions et un guide sur l'inspection et le remplacement des vis et des bras de servomoteur. | 
|  | Mise à jour de la section Tester les images de l'appareil photo à l'aide de l'automatisation pour ajouter la Honor Pad 20 à la liste d'autorisation des tablettes, et pour réorganiser et mettre à jour les instructions d'exécution des scènes ITS individuelles. | 
|  | Mise à jour des tests ITS de l'appareil photo pour ajouter des mécanismes d'échec et des images pour le test d'ombrage de l'objectif et d'uniformité des couleurs. | 
|  | Mise à jour des téléchargements de la Compatibility Test Suite avec des liens de téléchargement et des tags de synchronisation source pour les versions de mai 2026, y compris 16.1_R3, 16_R5, 15_R8 et 14_R12. | 
|  | Mise à jour du document de définition de la compatibilité Android 14, du document de définition de la compatibilité Android 15, du document de définition de la compatibilité Android 16 et du document de définition de la compatibilité Android 17 pour supprimer l'exigence 9.1 [C-5-1] pour les appareils Automotive. | 
| Connectivité | Ajout de la section Connectivité par satellite pour présenter la prise en charge de la connectivité par satellite sur Android. | 
| Graphismes | Suppression de *Implémenter la comptabilisation de la mémoire DMABUF et GPU* de Graphismes, car la comptabilisation sysfs par tampon a été supprimée au profit de la comptabilisation par processus basée sur procfs. | 
| Performances | Mise à jour de la section Gel des applications mises en cache pour fournir des informations complètes aux OEM et aux développeurs d'applications. La mise à jour couvre les comportements robustes d'Android 14, l'impact sur l'état du système (optimisations du démarrage à froid et de la batterie), les exemptions de congélation (comme les verrouillages de fichiers et les liaisons `BIND_WAIVE_PRIORITY` ) et le dépannage des modes d'échec courants tels que les transactions de liaison synchrones/asynchrones et les problèmes d'exécution des tâches planifiées. Il détaille également les outils de diagnostic, y compris logcat, dumpsys, les commandes Activity Manager et le traçage Perfetto. | 
| Alimentation | Mise à jour de la présentation de l'analyse de la puissance pour convertir la liste des SoC compatibles avec Wattson en tableau et ajouter de nouveaux appareils. | 
| Sécurité | Publication du bulletin sur la sécurité d'Android de mai 2026, du bulletin sur la mise à jour d'Android Automotive OS de mai 2026, du bulletin sur la sécurité de Wear OS de mai 2026, du bulletin sur la mise à jour des Pixel de mai 2026, du bulletin sur Android XR de mai 2026 et des remerciements concernant la sécurité d'Android. | 
|  | Mise à jour des fonctionnalités de sécurité Android et ajout de la limitation du débit pour documenter l'amélioration de l'implémentation de la limitation du débit de l'écran de verrouillage dans Android 16 QPR2 et versions ultérieures. Cette mise à jour détaille une règle de limitation du débit par défaut plus stricte pour se protéger contre les attaques par force brute, un `SoftwareRateLimiter` secondaire dans le serveur système et la détection des tentatives en double pour améliorer la facilité d'utilisation sans compromettre la sécurité. | 
|  | Mise à jour de la section Intégrité du flux de contrôle dans le noyau pour clarifier les différences entre Clang CFI (pour Linux 6.0 et versions antérieures) et Clang KCFI (pour Linux 6.1 et versions ultérieures), en précisant que KCFI ne nécessite pas d'optimisation au moment de l'édition des liens (LTO) et réduit ainsi les frais généraux liés au temps de compilation. | 
| Configuration | Mise à jour de la page Noms de code, tags et numéros de version avec les numéros de version et les tags de la version de sécurité de mai 2026. | 
| Télévision | Mise à jour de la section Tunneling multimédia pour ajouter les flux de séquence attendus pour la recherche précise et la lecture à vitesse variable. | 
| Tests | Ajout de Cloud Orchestration in OmniLab ATS (Orchestration cloud dans OmniLab ATS) et de Virtual devices in OmniLab ATS (Appareils virtuels dans OmniLab ATS), qui fournissent des guides sur la configuration de l'orchestration cloud et la gestion des appareils virtuels dans OmniLab Android Test Station. | 

## Avril 2026

Voici les modifications apportées au site en avril. Vous trouverez les bulletins publiés régulièrement, tels que les bulletins sur la sécurité d'Android, sur leurs pages respectives.

| Emplacement | Modifier | 
|---|---|
| Architecture | Mise à jour des conseils concernant le bootloader générique (GBL) pour détailler les exigences relatives aux variables UEFI fournies par le micrologiciel, y compris `gbl_fw_api_level` , pour déterminer la version sur Déployer le GBL. | 
|  | Mise à jour du processus de publication de l'image de noyau générique (GKI) avec les dates de disponibilité de la GKI pour Android 12 en 2026. | 
|  | Mise à jour des instructions pour l'ajout de modules GKI sur la page Configurer les fonctionnalités du noyau en tant que modules GKI. | 
|  | Ajout de nouveaux builds aux builds de version android16-6.12, aux builds de version android15-6.6, aux builds de version android14-6.1, aux builds de version android14-5.15, aux builds de version android13-5.15 et aux builds de version android12-5.10. | 
|  | Branches android13-5.10-2025-03, android13-5.15-2025-03, android14-5.15-2025-03, android14-6.1-2025-03 et android15-6.6-2025-03 obsolètes sur les builds de version d'image de noyau générique (GKI). | 
| Automobile | Publication de Car-apps-release-20, avec des mises à jour pour Dialer, DriverUI, la bibliothèque Car, Car Media, Sensitive App Protection et Radio. | 
|  | Ajout de l'importation `com.android.oem.tokens` manquante dans Verrouillage d'application. | 
|  | Ajout de Applications multimédias avec la bibliothèque d'applications Car. | 
|  | Mise à jour du chemin de configuration de la surutilisation des ressources Watchdog sur Surveiller l'utilisation de la mémoire flash. | 
|  | Ajout de Car-apps-release-21 et Car-apps-release-22. | 
| Build | Mise à jour de la page Noms de code, balises et numéros de version avec les numéros de version des versions d'AOSP. | 
| Compatibilité | Ajout de la Honor Pad V9 à la liste des tablettes compatibles avec les tests Camera ITS sur Tester les images de l'appareil photo à l'aide de l'automatisation. | 
|  | Mise à jour de la section Exécuter les tests CTS Verifier du framework audio avec de nouvelles procédures de test et des captures d'écran pour les tests de fréquence. | 
|  | Mise à jour des tests ITS de l'appareil photo lors de l'exécution de `scene5` . | 
|  | Suppression d'une exigence obsolète concernant le tag NFC Forum de type 1 sur Android 12 7.4.4. Communication en champ proche, Android 13 7.4.4. , Android 14 7.4.4. , Android 15 7.4.4. Communications en champ proche et Android 16 7.4.4. Near-Field Communications. | 
| Cuttlefish | Ajout d'une description pour la cible de compilation réservée au fournisseur Cuttlefish sur la page Créer un appareil hybride Cuttlefish. | 
| Graphismes | Ajout de la présentation des fonctionnalités de Vulkan 1.4. | 
|  | Mise à jour des instructions de mappage Winscope `IntDef` sur Exécuter Winscope. | 
| Sécurité | Publication du bulletin sur la sécurité d'Android d'avril 2026, du bulletin sur la mise à jour d'Android Automotive OS d'avril 2026, du bulletin sur la sécurité de Wear OS d'avril 2026, du bulletin sur la mise à jour des Pixel d'avril 2026, du bulletin sur Android XR d'avril 2026 et des remerciements concernant la sécurité d'Android. | 
|  | Ajout de Weaver, la couche d'abstraction matérielle (HAL) pour la gestion des clés cryptographiques sur l'appareil, qui fournit un stockage sécurisé et un contrôle des accès pour les clés utilisées pour l'authentification et le chiffrement, protégeant ainsi contre toute utilisation non autorisée, même sur les appareils dont la sécurité est compromise. | 
|  | Ajout du document sur la sécurité d'Android à la liste des rapports. | 
|  | Suppression de CVE-2026-21735 et CVE-2025-48650 du bulletin sur la sécurité d'Android de mars 2026, et de CVE-2025-48581 du bulletin sur la sécurité d'Android de novembre 2025. | 
|  | Mise à jour du déni de service pour le service mobile ou Wi-Fi sur Mises à jour de sécurité et ressources. | 
| Tests | Mise à jour de la version de Python et des exigences d'OS pour la CLI ATS sur Installer Python 3. | 
|  | Mise à jour de la page OmniLab Android Test Station pour préciser que les utilisateurs doivent utiliser `--force_update` pour passer à la dernière version, et qu'OmniLab ATS 2.0 est disponible à partir de la version R52. | 

## Mars 2026

Voici les modifications apportées au site en mars. Vous trouverez les bulletins publiés régulièrement, tels que les bulletins sur la sécurité d'Android, sur leurs pages respectives.

| Emplacement | Modifier | 
|---|---|
| Architecture | Mise à jour des conseils concernant le bootloader générique (GBL) pour préciser que n'importe quel type de partition FAT peut être utilisé (pas seulement FAT32), et mise à jour de la taille de partition `android_esp` requise à 8 Mo sur Déployer le GBL. | 
|  | Mise à jour du processus de publication de l'image de noyau générique (GKI) pour supprimer les informations sur les versions hebdomadaires, car elles ne sont plus publiées, et pour reformater les tableaux. | 
|  | Sur les noyaux Android courants, ajout du noyau `android17-6.18` aux tableaux, suppression des noyaux 5.4, car ils ont atteint la fin de leur cycle de vie, et suppression des informations sur les tests fonctionnels du noyau Linux (LKFT), car ils ont été arrêtés. | 
|  | Mise à jour de la définition de "VM protégée" pour refléter qu'il s'agit d'un concept universel dans l'AVF sur la page Présentation du Framework de virtualisation Android (AVF). | 
| Automobile | Ajout du guide d'intégration de Verrouillage d'application pour l'application dissociée Verrouillage d'application sensible et mise à jour de Intégrer des applications dissociées. | 
| Compatibilité | Mise à jour de l'article Acheter et configurer une box Gen2 avec un tableau révisé et un numéro de téléphone mis à jour pour contacter le fabricant. Ajout de la Samsung Galaxy Tab S10 FE à la liste d'autorisation des tablettes compatibles sur Tester les images de l'appareil photo à l'aide de l'automatisation. | 
|  | Ajout de la documentation sur le support d'extension Flexi-Tele, qui permet de tester l'ITS avec différentes distances de mire pour les téléobjectifs, à la section Acheter et configurer un système de support modulaire. | 
|  | Annulation des mises à jour du test CTS Verifier Pro Audio et des tests CTS Verifier de l'audio USB, en attendant les prochaines versions. | 
| Connectivité | Mise à jour de la section Sélection du réseau Wi-Fi avec des détails sur les paramètres d'évaluation du réseau connecté et ajout d'informations sur la fonctionnalité de filtre de mobilité de l'opérateur. | 
|  | Mise à jour des exigences HCI Bluetooth pour inclure Opus en tant que codec compatible, en l'ajoutant aux masques de bits et aux listes de codecs, et en définissant ses fonctionnalités spécifiques au fournisseur. | 
| Interaction | Sur la page Implémenter des effets d'enveloppe linéaire par morceaux, mise à jour des exigences concernant les effets haptiques d'enveloppe linéaire par morceaux (PWLE), y compris une nouvelle formule pour le calcul de la plage de fréquences de netteté. | 
| Sécurité | Publication du bulletin sur la sécurité d'Android de mars 2026, du bulletin sur les mises à jour d'Android Automotive OS de mars 2026, du bulletin sur les mises à jour des Pixel de mars 2026, du bulletin sur la sécurité de la Pixel Watch de mars 2026, du bulletin sur la sécurité de Wear OS de mars 2026 et du bulletin sur Android XR de mars 2026. | 
|  | Sur la page Attestation de clé et d'ID, clarification des champs inclus dans l'extension d'attestation dans différentes conditions. | 
|  | Mise à jour des remerciements concernant la sécurité d'Android pour saluer les chercheurs qui ont contribué à identifier des failles de sécurité dans Android. | 
| Configuration | Sur la page Noms de code, tags et numéros de build, ajout de nouvelles colonnes pour la **version de CTS** et le**nom de la version trimestrielle** , et ajout de numéros de version pour les versions de rétroportage de sécurité de mars 2026. | 
| Tests | Dans Transmettre des options et des filtres à la suite et aux modules, ajout d'exemples pour transmettre des options aux modules, y compris l'utilisation de `set-option` pour les classes de test Java dans`java_test_host` . | 
|  | Sur la page Rescue Party, mise à jour de la commande ADB pour déclencher une boucle de plantage `system_server` de bas niveau en fonction des commentaires des utilisateurs, et conversion de la page du format HTML au format Markdown. | 
|  | Mise à jour d'OmniLab Android Test Station pour indiquer que le programme d'installation est désormais compatible avec Ubuntu 22.04. | 

## Février 2026

Voici les modifications apportées au site en février. Vous trouverez les bulletins publiés régulièrement, tels que les bulletins sur la sécurité d'Android, sur leurs pages respectives.

| Emplacement | Modifier | 
|---|---|
| Architecture | Mise à jour des conseils concernant la partition système EFI (ESP) du bootloader générique (GBL) pour faire référence aux systèmes de fichiers FAT et clarification du fait que le GUID doit correspondre à la valeur d'une ESP sur Déployer le GBL. | 
|  | Ajout de nouvelles versions aux versions android16-6.12, aux versions android15-6.6, aux versions android14-6.1, aux versions android14-5.15, aux versions android13-5.15 et aux versions android12-5.10. | 
|  | Mise à jour de la documentation sur l'option pour les développeurs 16 ko pour le déverrouillage OEM sur Activer le bouton 16 ko. | 
|  | Ajout de mises à jour du backend AIDL NDK pour `linkToDeath` sur Lien vers la mort. | 
|  | Ajout de la compatibilité manquante du backend `cpp` pour FMQ sur Fast Message Queue avec AIDL. | 
|  | Mise à jour des informations sur le cycle de vie de FCM. | 
| Automobile | Clarification de la non-obligation de conformité à la suite de tests d'images (ITS) pour les implémentations du service de caméra Automotive (ACS) dans la présentation, car la standardisation du développement de caméras sur Android Automotive OS à l'aide du framework Camera2 ne nécessite pas de tests ITS pour l'ACS. | 
| Compatibilité | Mise à jour des liens de téléchargement de CTS et CTS-Verifier pour les versions de février 2026, y compris les versions 16.1_R2, 16_R4, 15_R7, 14_R11 et 13_R15 sur la page Téléchargements de la suite de tests de compatibilité. | 
|  | Les instructions d'achat et de configuration pour les tests Bluetooth multi-appareils ont été déplacées de la page principale de configuration de la CTS vers un guide dédié, Configurer le point d'accès BananaPi-R3. | 
|  | Mise à jour des exigences d'orientation de la caméra dans les documents Android 16 Compatibility Definition, Android 15 Compatibility Definition et Android 14 Compatibility Definition. | 
|  | Ajout de Exécutions de test lorsque les flags de fonctionnalité sont désactivés sur l'appareil. | 
| Connectivité | Ajout de la section Implémenter des onglets personnalisés Android pour la connexion au portail captif pour en savoir plus sur les onglets personnalisés Android dans les portails captifs. | 
|  | Mise à jour de la section Préparer l'UICC de la spécification du profil CTS Android pour spécifier une clé ADM1 fixe et non une clé ADM1 aléatoire, ce qui évite les échecs de test CTS. | 
| Graphismes | Ajout de la relecture de DPU mise en production. | 
| Interaction | Correction de la durée minimale requise entre les points de l'enveloppe linéaire par morceaux (PWLE) à 20 ms sur Implémenter des effets d'enveloppe linéaire par morceaux, ce qui permet d'obtenir un rendu cohérent des effets haptiques sur les appareils compatibles avec cette fonctionnalité. | 
| Alimentation | La documentation de Wattson a été étendue pour inclure la compatibilité avec les GPU, et la famille d'appareils Pixel 10 a été ajoutée à la liste du matériel compatible avec l'analyse de la consommation d'énergie sur les pages Présentation de l'analyse de la consommation d'énergie et Collecter et afficher des traces. | 
| Sécurité | Publication du bulletin sur la sécurité d'Android de février 2026, du bulletin de mise à jour d'Android Automotive OS de février 2026, du bulletin de mise à jour Pixel de février 2026, du bulletin sur la sécurité de Wear OS de février 2026 et du bulletin Android XR de février 2026. | 
|  | Mise à jour de la documentation sur l'attestation des clés pour fournir des conseils plus clairs sur la représentation des niveaux de correctif de démarrage et du fournisseur dans les certificats d'attestation sur la page Attestation de clé et d'ID. | 
|  | Restauration de la mise en forme des couleurs Material Design pour les sections des rapports HWASan sur la page Comprendre les rapports HWASan, ce qui améliore la clarté des diagnostics de sécurité de la mémoire et de l'analyse des erreurs. | 
|  | Mise à jour des remerciements concernant la sécurité Android pour saluer les chercheurs qui ont contribué à identifier des failles de sécurité dans la version de février 2026. | 
|  | Suppression de CVE-2025-48612 du bulletin sur la sécurité d'Android de décembre 2025 et de CVE-2025-22442 du bulletin sur la sécurité d'Android d'avril 2025 de la liste des failles corrigées. | 
|  | Mise à jour du nom de l'option pour les développeurs MTE et clarification du fait que `default_memtag` est une propriété du bootloader sur la page Compatibilité du bootloader MTE. | 
|  | Clarification de `validated_attested_entity` sur la page Schéma. | 
| Tests | Mise à jour des notes de version d'ATS pour la version R52 sur la page Notes de version d'OmniLab Android Test Station. | 
|  | Ajout du guide de mise à niveau vers OmniLab Android Test Station 2.0, et mise à jour d'OmniLab Android Test Station et des questions fréquentes sur ATS. | 
| Virtualisation | Clarification de la définition d'une machine virtuelle protégée (pVM) pour refléter qu'il s'agit d'un concept universel dans le framework de virtualisation Android dans la présentation du framework de virtualisation Android (AVF). | 

## Janvier 2026

Voici les modifications apportées au site en janvier. Vous trouverez les bulletins publiés régulièrement, tels que les bulletins sur la sécurité d'Android, sur leurs pages respectives.

| Emplacement | Modifier | 
|---|---|
| Architecture | Ajout d'un guide d'implémentation pour le bootloader générique (GBL) à la présentation du bootloader générique (GBL) et à Déployer le GBL. | 
|  | La section Passer des tas ION aux tas DMA-BUF (noyau 5.4 uniquement) est obsolète, car ION n'est plus compatible avec le noyau Linux 5.4, qui est arrivé en fin de vie. | 
|  | Mise à jour du nom du verrou de `mmap_sem` à`mmap_lock` dans les notes de version du kernel. | 
|  | Mise à jour du lien vers les informations sur le kernel LTS dans la présentation du kernel. | 
|  | Mise à jour du tableau des versions de GKI avec les dates de sortie jusqu'en 2026 et la notation des noyaux compatibles 16K sur la page Processus de publication de l'image de noyau générique (GKI). | 
|  | Déplacement des consignes de respin GKI vers Demander un respin, une page dédiée, et mise à jour de Processus de publication de l'image de noyau générique (GKI). | 
|  | Ajout d'un exemple NDK pour Binder afin de gérer les applications mises en cache et gelées. | 
|  | Les branches android14-6.1-2024-12 et android15-6.6-2024-12 sont obsolètes à partir du 1er janvier 2026. | 
|  | Ajout de nouvelles versions aux versions android16-6.12, versions android15-6.6, versions android14-6.1, versions android14-5.15, versions android13-5.15, versions android13-5.10 et versions android12-5.10. | 
| Automobile | Ajout d'applications à la liste des applications dissociées dans Intégrer des applications dissociées. | 
|  | Publication des notes de version pour Android Automotive 25Q4. | 
|  | Mise à jour des notes de version des applications dissociées avec des informations sur les contenus multimédias CAL. | 
| Build | Mise à jour des conseils sur la définition des valeurs des flags de fonctionnalité sur Définir les valeurs des flags de lancement de fonctionnalités. | 
| Compatibilité | Mise à jour de la documentation sur la qualité d'encodage vidéo basée sur le contraste perceptuel (PC-VEQ) pour mentionner l'utilisation de Media3 Transformer dans Exécuter des tests de classe de performances multimédias. | 
|  | Ajout d'informations sur le nombre de modules ajoutés et supprimés dans les notes de version de CTS 16.1. | 
|  | Clarification des étapes de configuration conditionnées par le matériel de l'appareil dans Configurer les tests automatisés CTS. | 
| Connectivité | Correction du chemin d'accès à l'image dans Mesure de la distance : séquence de messages hors bande et spécification de la charge utile. | 
|  | Correction des normes NFC référencées dans Commandes NCI propriétaires d'Android. | 
|  | Mise à jour des exigences Bluetooth HCI avec des fonctionnalités pour les événements Sniff Offload et Bluetooth Quality Report (BQR) pour la surveillance de l'énergie, les statistiques RF avancées et la surveillance de l'état du contrôleur. | 
| Sécurité | Mise à jour des notes de version de sécurité d'Android 14 avec les nouvelles failles CVE-2023-45784 et CVE-2023-45782. | 
|  | Ajout de la CVE-2025-36911 au bulletin de mise à jour Pixel de janvier 2026 et mise à jour des remerciements concernant la sécurité d'Android. | 
|  | Mise à jour d'Android Security AutoRepro avec un style de structuration et des notes sur l'intégration d'agents de codage dans les modèles et les exemples. | 
|  | Mise à jour du bulletin sur la sécurité d'Android de décembre 2025 pour supprimer la CVE-2025-48631. | 
|  | Mise à jour des remerciements concernant la sécurité Android pour CVE-2025-48581 dans la liste de novembre 2025. | 
|  | Ajout de la section Bulletins de sécurité Android XR et publication du bulletin Android XR de janvier 2026. | 
|  | Publication du bulletin sur la sécurité d'Android de janvier 2026, du bulletin de mise à jour d'Android Automotive OS de janvier 2026, du bulletin de mise à jour Pixel de janvier 2026 et du bulletin sur la sécurité de Wear OS de janvier 2026. | 
| Tests | Ajout d'exemples de commandes pour les dry runs `collect-tests-only` pour Tradefed dans Tester une configuration Trade Federation (TF). | 
| Mises à jour | Correction du nom de la variable `FinalDessertSize` en`FactorySize` dans Dimensionner la super partition. | 
|  | Correction du lien d'image dans Services NFC. |
