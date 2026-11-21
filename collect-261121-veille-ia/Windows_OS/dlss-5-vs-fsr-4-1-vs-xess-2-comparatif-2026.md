---
id: collect-261121-veille-ia/windows-os/dlss-5-vs-fsr-4-1-vs-xess-2-comparatif-2026
title: "DLSS 5 vs FSR 4.1 vs XeSS 2 : Comparatif [2026]"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-09-03"]
keywords: ["dlss", "nvidia", "generation", "xess", "trois", "rendu", "cette", "comparatif"]
source: https://tech-insider.org/fr/dlss-5-vs-fsr-4-1-vs-xess-2-2026/
source_anchor: ""
source_lines: [1, 196]
section: "Windows_OS"
---
Le 3 septembre 2026, Nvidia a mis en ligne DLSS 5 sur *NBA 2K27* avec un chiffre choc : jusqu’à 590 images par seconde en 1440p sur une RTX 5090. Trois jours plus tard, Tom’s Hardware et Notebookcheck démontraient que le rendu natif de cette même carte tournait en réalité autour de 62 fps, le reste provenant d’une génération d’images par IA. La polémique a immédiatement relancé un débat plus large : DLSS 5, FSR 4.1 “Redstone” d’AMD et XeSS 2 d’Intel jouent-ils encore au même jeu ? Ce comparatif détaille les trois technologies d’upscaling, leurs GPU compatibles, leurs vrais gains de performance mesurés par la presse spécialisée, et leur impact direct sur le prix des cartes graphiques vendues en France.

## DLSS 5, FSR 4.1 et XeSS 2 : pourquoi ce trio s’impose en septembre 2026

Les trois grands fabricants de GPU ont livré leur nouvelle génération d’upscaling à quelques mois d’écart. Nvidia a dégainé **DLSS 5** avec sa fonction “3D-Guided Neural Rendering”, intégrée pour la première fois dans *NBA 2K27* via le pilote Game Ready 616.64 WHQL. AMD a de son côté déployé **FSR Upscaling 4.1**, brique centrale de la suite “Redstone” annoncée au CES 2026, qui ajoute la génération de frames, la régénération de rayons et un cache de radiance. Intel, enfin, a étendu **XeSS 2** avec la génération de frames et la technologie Xe Low Latency sur ses cartes Arc B-series.

Ce qui rend la comparaison utile maintenant, c’est que les trois technologies ne visent plus seulement à améliorer la définition d’image : elles cherchent à multiplier le nombre d’images affichées sans que le GPU les calcule réellement toutes. Cette bascule vers la “frame generation” massive change la façon de lire un test de performance, et elle a des conséquences concrètes sur le choix d’achat d’une carte graphique en France, où les prix de la gamme RTX 50 et RX 9000 restent élevés. Le mot-clé “dlss 5” affiche à lui seul plus de 6 600 recherches mensuelles en France selon les données de suivi de mots-clés de septembre 2026, preuve que le sujet dépasse largement le cercle des passionnés. Pour les lecteurs qui découvrent seulement l’ancienne génération, notre guide pour activer DLSS 4 sur RTX 50 reste utile en attendant l’extension officielle de DLSS 5 à davantage de jeux.

## DLSS 5 : la 3D-Guided Neural Rendering de Nvidia

DLSS 5 introduit un concept que Nvidia baptise “3D-Guided Neural Rendering”. Contrairement aux versions précédentes qui se contentaient de reconstruire une image à partir d’une résolution plus basse, ce nouveau modèle combine les données de la scène 3D fournies par le moteur de jeu (éclairage, matériaux, ombres de contact, diffusion sous-cutanée sur la peau) avec un réseau de neurones qui ajuste le rendu final en temps réel. Selon Nvidia, cette approche rapproche le rendu en temps réel de la qualité d’un rendu cinéma, un argument que le communiqué officiel de Nvidia met en avant pour justifier le lancement exclusif sur *NBA 2K27*.

Techniquement, DLSS 5 est réservé au lancement aux **GPU GeForce RTX 50** (bureau et portables) ainsi qu’à GeForce NOW Ultimate, qui s’appuie sur des serveurs cloud équipés de puces de classe RTX 5080. Les cartes RTX 40 et RTX 30 en sont exclues à ce stade, même si Nvidia a indiqué vouloir étendre le support officiel aux RTX 40 une fois les modèles optimisés sur la génération Blackwell, selon des propos rapportés par la presse spécialisée. Des moddeurs sont parvenus à faire tourner DLSS 5 de force sur des RX 9070 XT d’AMD, mais Tom’s Hardware précise que la carte ne dépasse pas 30 fps en 1080p dans cette configuration non officielle, loin de l’expérience visée.

La sortie a immédiatement suscité une controverse. Les chiffres marketing de Nvidia annoncent 370 fps en 4K et 590 fps en 1440p sur RTX 5090 avec les réglages Ultra et le ray tracing activé. Mais Tom’s Hardware a démontré que ce chiffre de 370 fps correspond à une image réellement calculée à environ 61 fps native, le reste étant généré par la génération de frames multiple (Multi Frame Generation). Notebookcheck, qui a repris une méthodologie proche de celle de Digital Foundry, a mesuré une chute de 128,4 fps à 62,7 fps en 4K Ultra sur RTX 5090 une fois DLSS 5 activé avec le mode neural rendering complet, soit une perte de rendu natif de 51,2 %. Sur RTX 5070, la perte grimpe à 55 %, et à 41 % sur RTX 5060. Cette dissociation entre fps affichés et fps réellement calculés a valu à DLSS 5 le surnom de “filtre à bouillie IA” (“AI slop filter”) dans une partie de la presse et des communautés de joueurs, un qualificatif né précisément de cet écart entre communication et mesures indépendantes.

## FSR 4.1 Redstone : la réponse d’AMD, plus ouverte mais moins spectaculaire

AMD n’a pas attendu DLSS 5 pour répondre. Sa suite **FSR Redstone**, présentée au CES 2026, regroupe l’upscaling par apprentissage automatique (FSR Upscaling 4.1), la génération de frames, la régénération de rayons et un cache de radiance. Le chief software officer d’AMD, Andrej Zdravković, a confirmé lors d’une table ronde presse que l’ensemble complet de la suite Redstone restait pour l’instant réservé aux GPU RDNA 4, c’est-à-dire les Radeon RX 9070, RX 9070 XT et RX 9060 XT, un choix similaire à la stratégie de Nvidia avec ses RTX 50.

La différence de philosophie se joue ailleurs. Selon des informations reprises par Club386 et IGN, AMD envisage d’ouvrir une partie de la technologie FSR 4 à des GPU concurrents, y compris certaines cartes Nvidia dotées d’unités IA compatibles FP8 (architecture Ada et plus récente), même si le cœur de la suite Redstone resterait propre à RDNA 4. C’est un positionnement inverse de celui de Nvidia, qui verrouille DLSS 5 à son propre matériel. La documentation technique d’AMD publiée sur GPUOpen confirme cette volonté de compatibilité élargie, tout en reconnaissant que le modèle transformeur de DLSS 4.5 garde une avance en qualité d’image pure.

Côté performance, les chiffres publics restent plus rares que pour DLSS 5. AMD revendique un gain moyen de 3,5 fois les performances sur RX 9070 XT en combinant upscaling et génération de frames sur l’ensemble de sa suite Redstone, une moyenne multi-jeux et non un chiffre par titre. Un test comparatif plus précis, réalisé sur une Radeon RX 9060 XT, mesure un gain concret de 44 % en mode FSR 4.1 Qualité par rapport au rendu natif, avec une moyenne de 61 fps et un minimum de 55 fps dans le jeu testé. Sur le plan de l’adoption, FSR 4 en mode upscaling par IA équipe désormais plus de 200 jeux, et la génération de frames dédiée à la RDNA 4 est présente dans une trentaine de titres selon les données publiées par la presse spécialisée en hardware, un rythme d’adoption plus rapide que celui de DLSS 5 qui ne compte qu’un seul jeu au lancement. Nous avions déjà détaillé l’arrivée de FSR 4.1 sur les Radeon RX 7000, une extension aux GPU de génération précédente qui distingue la stratégie logicielle d’AMD de celle de Nvidia.

## XeSS 2 : Intel mise sur la génération de frames pour exister face aux deux géants

Intel reste le troisième acteur du marché des GPU dédiés, mais XeSS 2 affiche des gains impressionnants sur le papier, précisément parce que le point de départ (les cartes Arc) est plus modeste en puissance brute. La documentation officielle de performance d’Intel, disponible sur son centre de documentation des benchmarks Arc, indique un gain moyen de 85 % avec XeSS classique (upscaling seul) et de 187 % avec XeSS 2 une fois la génération de frames et la technologie anti-latence Xe Low Latency activées, sur une sélection de jeux testés sur Arc B580.

Ces moyennes se retrouvent dans des tests indépendants. Le site Club386 a mesuré, sur une Arc B580 Limited Edition en 1440p, un passage de 51 fps en rendu natif à 85 fps avec XeSS Performance seul (+66,7 %), puis à 145 fps une fois la génération de frames ajoutée (+184 % par rapport au natif), un résultat cohérent avec la moyenne officielle d’Intel. Sur *F1 24*, l’un des premiers titres compatibles XeSS 2, un test technique chinois relayé par plusieurs médias spécialisés a mesuré un bond de 25,8 fps en ray tracing natif à 81,3 fps avec XeSS Performance (+215 %), puis à 123 fps avec la génération de frames XeSS 2 activée, soit une hausse de 51,3 % supplémentaires par rapport au mode upscaling seul, et une latence divisée par deux grâce à Xe Low Latency (6,61 ms contre 13,4 ms).

La limite actuelle de XeSS 2 est la couverture matérielle : les données solides et vérifiées concernent presque exclusivement l’Arc B580, la carte d’entrée de gamme d’Intel vendue autour de 390 € en France selon les comparateurs de prix. Aucune donnée fiable de performance XeSS 2 sur l’Arc B770 n’a pu être confirmée par une source de presse spécialisée au moment de la rédaction de cet article, ce qui traduit une diffusion encore limitée du haut de gamme Arc en Europe. Pour situer l’Arc B580 face à la concurrence directe côté milieu de gamme, notre comparatif RTX 5060 Ti vs RX 9060 XT détaille les écarts de prix et de performance sur ce segment.

## De DLSS 1 à DLSS 5 : un rappel avant de comparer

Pour comprendre l’ampleur du changement apporté par DLSS 5, un rapide retour en arrière aide à situer les enjeux. Nvidia a lancé la première version de DLSS en 2019, avec un simple réseau de neurones chargé de reconstruire une image en haute résolution à partir d’un rendu plus léger. Les versions 2 et 3 ont ensuite ajouté la reconstruction temporelle puis la première génération de frames, avant que DLSS 4 n’introduise un modèle transformeur remplaçant l’ancien réseau convolutif, avec à la clé une génération de frames multiple capable d’insérer plusieurs images calculées par IA entre deux images réellement rendues. DLSS 5 pousse cette logique encore plus loin en injectant des données de scène 3D directement dans le processus de rendu, une étape que Nvidia présente comme le franchissement d’un nouveau palier plutôt qu’une simple mise à jour incrémentale.

Côté AMD, l’histoire suit une trajectoire un peu différente. FSR 1, sorti en 2021, se contentait d’un filtre spatial sans intelligence artificielle, ce qui expliquait sa compatibilité universelle mais aussi sa qualité d’image en retrait face à DLSS. FSR 2 a introduit la reconstruction temporelle, FSR 3 la génération de frames, et FSR 4 a marqué la bascule vers un véritable modèle d’apprentissage automatique, un changement de paradigme qui explique pourquoi AMD a dû, comme Nvidia, restreindre les fonctions les plus avancées à sa dernière architecture (RDNA 4). FSR 4.1 et la suite Redstone représentent donc la consolidation de cette bascule vers le tout-IA, plus que la création d’une technologie entièrement nouvelle.

Intel, arrivé plus tardivement sur le marché des GPU dédiés avec les Arc A-series en 2022, a construit XeSS directement autour d’un modèle par apprentissage automatique dès sa première version, avec un chemin d’exécution optimisé pour ses propres unités XMX et un chemin de repli DP4a pour les GPU tiers. Cette approche explique pourquoi Intel a pu ajouter la génération de frames et Xe Low Latency dans XeSS 2 sans rupture technologique aussi marquée que celle vécue par AMD entre FSR 3 et FSR 4. Le retard d’Intel se situe moins dans la technologie elle-même que dans le nombre de GPU Arc réellement vendus, ce qui limite le nombre de tests indépendants disponibles comparé aux écosystèmes bien plus larges de Nvidia et AMD.

## 5 exemples concrets déjà visibles dans les jeux et le matériel

**1. NBA 2K27 et le lancement de DLSS 5.** Le jeu de basketball de 2K Games reste, au moment de la rédaction de cet article, le seul titre à intégrer nativement la 3D-Guided Neural Rendering de DLSS 5, déployée via le pilote Game Ready 616.64 WHQL le 3 septembre 2026. C’est ce jeu qui a servi de vitrine, pour le meilleur (les gains de fps bruts) et pour le pire (la controverse sur l’écart entre fps affichés et rendu natif).

**2. F1 24 et les premiers pas de XeSS 2.** Le jeu de course de Codemasters a servi de banc d’essai pour la génération de frames d’Intel, avec des gains mesurés dépassant 200 % par rapport au rendu natif en ray tracing sur une Arc B580, un résultat qui a permis à la presse spécialisée de documenter précisément le fonctionnement de Xe Low Latency en conditions réelles.

**3. L’extension de FSR Upscaling 4.1 dans Unreal Engine 5.8.** AMD a mis à jour son plugin officiel pour le moteur Unreal Engine afin d’y intégrer FSR Upscaling 4.1, permettant aux studios utilisant Unreal Engine 5.8 d’ajouter facilement l’upscaling par IA de la RX 7000 à leurs futurs jeux sans développement spécifique, un exemple concret de la stratégie d’AMD pour accélérer l’adoption logicielle de sa technologie.

**4. Le hack communautaire de DLSS 5 sur RX 9070 XT.** Des moddeurs ont réussi, quelques jours après le lancement, à forcer l’exécution de DLSS 5 sur une carte AMD non prévue pour cette technologie. Le résultat, environ 30 fps en 1080p selon Tom’s Hardware, montre à la fois la porosité technique entre écosystèmes concurrents et la marge de progression nécessaire avant qu’un portage non officiel devienne réellement jouable.

**5. GeForce NOW Ultimate comme vitrine cloud de DLSS 5.** Plutôt que d’attendre une mise à niveau matérielle locale, les abonnés du service de cloud gaming de Nvidia peuvent accéder à DLSS 5 via des serveurs équipés de GPU de classe RTX 5080, un exemple concret de la manière dont le cloud gaming permet de contourner le coût d’achat d’une carte graphique neuve pour profiter d’une technologie encore réservée au haut de gamme.

## Tableau comparatif technique : DLSS 5 vs FSR 4.1 vs XeSS 2

Voici la synthèse technique des trois technologies d’upscaling, avec les caractéristiques vérifiées à partir des annonces officielles et des tests indépendants disponibles en septembre 2026.

| Critère | DLSS 5 (Nvidia) | FSR 4.1 Redstone (AMD) | XeSS 2 (Intel) | 
|---|---|---|---|
| Date de lancement public | 3 septembre 2026 | CES 2026 (suite Redstone) | 2026, extension avec génération de frames | 
| Fonction phare | 3D-Guided Neural Rendering | Upscaling ML + génération de frames + régénération de rayons + cache de radiance | Upscaling ML + génération de frames + Xe Low Latency | 
| GPU natifs supportés | GeForce RTX 50 (bureau et portable), GeForce NOW Ultimate | Radeon RX 9070, RX 9070 XT, RX 9060 XT (suite complète) | Arc B580 (données vérifiées), architecture Xe2 | 
| Support GPU plus anciens | RTX 40 annoncé pour plus tard, RTX 30 non supporté | Upscaling FSR 4 classique déjà étendu aux RX 7000 ; ouverture explorée vers RDNA 3 et GPU tiers | Chemin de repli DP4a possible sur GPU non-Intel (architecture antérieure) | 
| Premier jeu compatible | NBA 2K27 (exclusif au lancement) | Plus de 200 jeux en upscaling FSR 4, une trentaine en génération de frames RDNA 4 | F1 24 parmi les premiers titres XeSS 2 | 
| Gain moyen mesuré (upscaling seul) | Non isolé par la presse, mesuré combiné | +44 % (RX 9060 XT, mode Qualité, test indépendant) | +85 % en moyenne (données Intel), +66,7 % à +215 % selon le jeu testé | 
| Gain moyen mesuré (upscaling + génération de frames) | Chute du rendu natif de 41 à 55 % selon la carte une fois le mode complet activé | Jusqu’à 3,5x en moyenne multi-jeux selon AMD | +187 % en moyenne (données Intel), jusqu’à +376 % sur F1 24 en RT | 
| Ouverture multi-vendeur | Fermé à l’écosystème RTX | Upscaling de base ouvert, exploration d’un support Nvidia via FP8 | Conçu pour être API-agnostique, chemin de repli sur GPU non-Intel | 
| Disponible en cloud gaming | Oui, via GeForce NOW Ultimate | Non confirmé pour un service cloud grand public | Non confirmé pour un service cloud grand public | 
| Principale critique | Écart marketing/rendu natif, surnommé “filtre à bouillie IA” | Suite complète encore réservée à une seule génération de GPU (RDNA 4) | Couverture matérielle limitée en haut de gamme (Arc B770 peu documenté) | 
| Modèle de calcul | Réseau neuronal guidé par les données 3D du moteur de jeu | Upscaling par apprentissage automatique, plus léger que DLSS selon AMD | Réseau temporel ML, accéléré par les unités XMX sur Arc | 

## Tableau des prix : l’impact sur les cartes graphiques vendues en France

Le choix d’une technologie d’upscaling est indissociable du prix de la carte graphique qui la supporte. Voici les tarifs observés en France mi-septembre 2026, entre le MSRP officiel et les prix constatés chez les revendeurs.

| Carte graphique | Technologie supportée | MSRP officiel (Europe) | Prix constaté en France (sept. 2026) | 
|---|---|---|---|
| RTX 5070 | DLSS 5 | Non communiqué séparément par Nvidia | Environ 780 € à 1 000 €, médiane proche de 900 € selon les comparateurs de prix | 
| RTX 5070 Ti | DLSS 5 | 884 € TTC | Prix constatés au-dessus du MSRP selon la disponibilité | 
| RTX 5080 | DLSS 5 | 1 179 € TTC (Founders Edition) | Variable selon les modèles des partenaires | 
| RTX 5090 | DLSS 5 | 2 349 € TTC (Founders Edition) | Rapporté jusqu’à 3 700-5 500 € lors des pics de pénurie en 2026 | 
| RX 9070 | FSR 4.1 Redstone | Non communiqué séparément par AMD | À partir d’environ 930 € (modèle Sapphire) | 
| RX 9070 XT | FSR 4.1 Redstone | Non communiqué séparément par AMD | Environ 780 € à 980 € selon les partenaires (ASRock, Sapphire, XFX, ASUS) | 
| RX 9060 XT | FSR 4.1 Redstone | Non confirmé publiquement en euros | Non confirmé par une source vérifiée | 
| Arc B580 | XeSS 2 | Environ 390 € en France selon les comparateurs | Environ 390 € | 

Ce tableau illustre un déséquilibre net : DLSS 5 impose l’achat d’une carte RTX 50, dont le ticket d’entrée réel dépasse 780 € en France, pour profiter de la fonctionnalité la plus avancée du marché. FSR 4.1 se situe dans une fourchette de prix comparable côté AMD, sans avantage tarifaire décisif. Seul Intel casse les prix avec l’Arc B580 à moins de 400 €, mais au prix d’une puissance brute et d’une couverture logicielle encore loin de celles de ses deux concurrents. Pour approfondir l’écart de prix précis entre les modèles Nvidia, consultez notre comparatif RTX 5070 vs RTX 5070 Ti ainsi que notre analyse RTX 5080 vs RTX 5090, qui détaillent les écarts de VRAM et de fps entre chaque palier de la gamme Blackwell.

## Benchmarks réels : ce que mesurent Tom’s Hardware, Notebookcheck et Club386

Au-delà des chiffres marketing, trois sources indépendantes permettent de comparer les technologies sur des bases concrètes. Tom’s Hardware a testé DLSS 5 sur l’ensemble de la gamme RTX 50 dans NBA 2K27 et confirme que même la RTX 5060, la carte d’entrée de gamme Blackwell, parvient à maintenir près de 60 fps en moyenne en 1080p avec DLSS 5 activé, un résultat honorable mais très éloigné des centaines d’images par seconde mises en avant par Nvidia lors de l’annonce.

Notebookcheck a documenté la perte de performance native engendrée par le mode neural rendering complet de DLSS 5 : 51,2 % sur RTX 5090, 55 % sur RTX 5070 et 41 % sur RTX 5060 en 4K Ultra. Cette perte s’explique par le coût de calcul du réseau de neurones qui ajuste l’éclairage et les matériaux en temps réel, une charge additionnelle par rapport à un simple upscaling de résolution. Côté Intel, le test de l’Arc B580 par Club386 montre au contraire un bénéfice net à chaque étage : le rendu natif à 51 fps grimpe à 85 fps avec l’upscaling seul, puis à 145 fps une fois la génération de frames ajoutée, sans le même effet de compensation négative observé côté DLSS 5.

Sur le terrain d’AMD, les données publiques restent plus parcellaires. Le constructeur communique une moyenne agrégée de 3,5x sur l’ensemble de la suite Redstone, sans détail par jeu comparable aux tests indépendants menés sur DLSS 5 ou XeSS 2. Le seul chiffre isolé disponible, un gain de 44 % en mode Qualité sur RX 9060 XT, confirme que FSR 4.1 apporte un bénéfice réel, mais sur un échantillon plus restreint que ses deux concurrents.

## Ce que dit Nvidia sur l’avenir du rendu neuronal

Jensen Huang, fondateur et PDG de Nvidia, a personnellement défendu l’ambition de DLSS 5 lors de l’annonce officielle. Il a déclaré : “Vingt-cinq ans après l’invention par Nvidia du shader programmable, nous réinventons à nouveau l’infographie” (source : communiqué officiel Nvidia GeForce). Il a également résumé la portée qu’il attribue à cette technologie en la comparant à une rupture générationnelle : “DLSS 5, c’est le moment GPT du graphisme, il mélange le rendu artisanal avec l’IA générative pour offrir un bond spectaculaire de réalisme visuel tout en préservant le contrôle dont les artistes ont besoin pour leur expression créative” (source : communiqué officiel Nvidia GeForce).

Ces déclarations tranchent avec la réception critique de la presse et d’une partie des joueurs, qui reprochent précisément à Nvidia de présenter des gains de performance qui reposent majoritairement sur de l’image générée plutôt que calculée. L’écart entre le discours d’entreprise et les mesures indépendantes de Tom’s Hardware et Notebookcheck illustre la tension actuelle du marché : plus les technologies d’upscaling avancent, plus la définition même de “performance” devient complexe à communiquer au grand public. Ce débat n’est pas neuf : notre comparatif précédent sur DLSS 4.5 vs FSR 4.1 sur GeForce NOW avait déjà documenté des écarts de fps de plus de 128 % entre technologies, avant même l’arrivée du rendu neuronal complet de DLSS 5.

## 5 cas d’usage concrets : quelle techno choisir selon votre profil

**1. Joueur compétitif en esport, priorité à la latence.** XeSS 2 avec Xe Low Latency a démontré, dans le test F1 24 de la presse chinoise relayé par plusieurs médias, une latence de 6,61 ms contre 13,4 ms en mode upscaling seul, la meilleure amélioration de latence mesurée parmi les trois technologies. Mais l’écosystème Arc reste réservé à un public de niche en France, avec un choix de modèles disponibles plus restreint que chez Nvidia ou AMD.

**2. Joueur AAA en 4K avec ray tracing, budget élevé.** DLSS 5 sur RTX 5090 reste la seule option capable d’afficher un rendu combinant “3D-Guided Neural Rendering” et ray tracing complet, au prix d’un investissement d’au moins 2 349 € en Europe, voire davantage en période de pénurie.

**3. Joueur au budget contraint, recherche du meilleur rapport prix/fps.** L’Arc B580 à environ 390 € et ses gains mesurés de 66,7 % à 215 % selon le jeu en font l’option la plus rentable sur le papier, à condition d’accepter une puissance brute inférieure aux cartes RTX ou Radeon plus chères.

**4. Joueur fidèle à l’écosystème AMD, multi-plateforme.** FSR 4.1 profite d’une couverture logicielle large, avec plus de 200 jeux compatibles en upscaling par IA, un chiffre supérieur à celui de DLSS 5 au lancement. La stratégie d’ouverture d’AMD vers d’autres constructeurs de GPU, encore en discussion selon Club386, pourrait aussi séduire les joueurs qui refusent le verrouillage matériel.

**5. Abonné cloud gaming.** GeForce NOW Ultimate reste la seule offre grand public qui propose DLSS 5 sans achat de carte graphique, avec des serveurs de classe RTX 5080. Ni AMD ni Intel ne proposent à ce jour d’équivalent confirmé sur un service de cloud gaming grand public.

**6. Créateur de contenu et streamer.** Le compromis entre qualité d’image et performance de FSR 4.1 en mode Qualité (+44 % mesuré sur RX 9060 XT) permet de garder de la marge processeur pour l’encodage vidéo simultané, un point important pour le streaming en direct.

## Guide de migration : passer d’une ancienne technologie d’upscaling

Pour les joueurs qui utilisent déjà DLSS 4.5, FSR 4 ou XeSS 1.x, la transition vers les nouvelles versions nécessite quelques vérifications avant de basculer.

- Vérifiez la compatibilité matérielle réelle de votre GPU : DLSS 5 exige une RTX 50, la suite Redstone complète d’AMD exige une RDNA 4 (RX 9070/9070 XT/9060 XT), et les meilleurs résultats XeSS 2 sont documentés sur Arc B580.
- Mettez à jour le pilote graphique en priorité : le pilote Game Ready 616.64 WHQL est requis côté Nvidia pour activer DLSS 5 dans NBA 2K27.
- Dans les paramètres du jeu, distinguez bien l’upscaling seul (résolution) de la génération de frames : activer les deux en même temps change radicalement le ressenti de latence, comme le montre l’écart mesuré par Notebookcheck entre le rendu natif et le mode neural rendering complet.
- Sur PC portable, privilégiez le mode upscaling seul sans génération de frames si votre écran ne dépasse pas 144 Hz, car les frames générées supplémentaires n’apportent pas de bénéfice perceptible au-delà de la fréquence de rafraîchissement de la dalle.
- Sur cartes AMD, activez Anti-Lag en complément de FSR 4.1 pour compenser en partie la latence ajoutée par la génération de frames, une pratique recommandée dans la documentation technique publiée sur GPUOpen.
- Sur Arc Intel, activez systématiquement Xe Low Latency avec XeSS 2, la technologie ayant démontré une réduction de latence de plus de 50 % par rapport au mode upscaling seul dans les tests disponibles.
- Ne vous fiez pas aux chiffres de fps affichés en gros dans les communications officielles des trois marques : comparez toujours le rendu natif et le rendu avec génération de frames avant de juger du bénéfice réel pour votre usage.

## Avantages et inconvénients de chaque technologie

### DLSS 5 (Nvidia)

**Avantages :** qualité d’image la plus avancée grâce au rendu neuronal guidé par la scène 3D, disponible aussi en cloud gaming via GeForce NOW Ultimate, écosystème logiciel le plus mature du marché.

**Inconvénients :** réservé aux RTX 50 au lancement, prix d’entrée élevé, écart important et controversé entre les fps marketing et le rendu natif réellement calculé, adoption limitée à un seul jeu au lancement.

### FSR 4.1 Redstone (AMD)

**Avantages :** couverture logicielle la plus large avec plus de 200 jeux compatibles, stratégie d’ouverture potentielle vers d’autres GPU, y compris Nvidia, prix des cartes RX 9070/9070 XT globalement alignés sur la concurrence.

**Inconvénients :** la suite Redstone complète reste, comme DLSS 5, réservée à une seule génération de GPU (RDNA 4), données de benchmarks indépendantes encore rares comparées à celles de DLSS 5 et XeSS 2.

### XeSS 2 (Intel)

**Avantages :** meilleur rapport prix/performance grâce à l’Arc B580 à environ 390 €, gains de fps les plus élevés en proportion sur les jeux testés, réduction de latence la plus marquée grâce à Xe Low Latency.

**Inconvénients :** couverture matérielle limitée au B580 dans les données vérifiées, absence de données fiables sur l’Arc B770, écosystème logiciel et parc de jeux compatibles plus restreint que ceux de Nvidia et AMD.

## Le verdict : quelle technologie d’upscaling l’emporte en septembre 2026

Sur la base des données vérifiées, aucune des trois technologies ne s’impose de façon absolue, chacune répond à un besoin différent. DLSS 5 reste techniquement le plus ambitieux avec sa 3D-Guided Neural Rendering, mais son lancement a été entaché par l’écart entre les 370 à 590 fps annoncés et les 41 à 55 % de rendu natif réellement mesurés par Notebookcheck et Tom’s Hardware, sur un unique jeu compatible et des GPU RTX 50 dont le prix dépasse 780 € en France, jusqu’à plus de 2 300 € pour la RTX 5090.

FSR 4.1 Redstone d’AMD séduit par sa couverture logicielle (plus de 200 jeux) et sa stratégie d’ouverture annoncée vers d’autres constructeurs, mais souffre encore d’un déficit de données de benchmarks indépendantes détaillées comparé à ses deux rivaux. XeSS 2 d’Intel affiche, sur les données disponibles, le meilleur rapport entre le prix de la carte (390 € pour l’Arc B580) et les gains de performance mesurés (jusqu’à +376 % en génération de frames sur F1 24), mais son adoption reste circonscrite à l’entrée de gamme Arc, faute de données publiques solides sur le haut de gamme B770. Pour un achat en 2026, le choix dépend donc moins d’une hiérarchie technologique tranchée que du budget disponible et du parc de jeux que chaque joueur possède déjà. Pour suivre l’ensemble des évolutions matérielles de cette année, notre dossier complet sur les puces IA et GPU en 2026 recense les autres annonces majeures du secteur.

## Ce que ça change pour les développeurs et les studios

Pour les équipes techniques qui intègrent ces technologies dans un moteur de jeu, la charge de travail n’est plus la même selon le fournisseur choisi. DLSS 5 impose l’exposition de données de scène supplémentaires au réseau de neurones de Nvidia (éclairage, matériaux, informations de profondeur détaillées), ce qui suppose une intégration plus poussée dans le pipeline de rendu que la simple activation d’un plugin d’upscaling classique. C’est une des raisons pour lesquelles seul *NBA 2K27* propose la fonctionnalité au lancement : le studio a dû collaborer étroitement avec Nvidia pour exposer les bonnes données au bon format, un travail d’intégration que d’autres studios devront reproduire avant de proposer DLSS 5 à leur tour.

Chez AMD, la mise à jour du plugin officiel pour Unreal Engine 5.8 illustre une approche différente : en intégrant FSR Upscaling 4.1 directement au niveau du moteur, AMD réduit la charge d’intégration pour les studios qui utilisent déjà Unreal Engine, au prix d’un contrôle plus limité sur l’exploitation fine des données spécifiques à chaque jeu. Cette approche “moteur d’abord” explique en partie pourquoi FSR affiche une couverture logicielle largement supérieure à DLSS 5 en nombre de jeux compatibles, même si la sophistication technique de chaque intégration individuelle reste, selon les propres documents techniques d’AMD, en retrait par rapport au modèle transformeur de Nvidia.

Le SDK XeSS d’Intel, disponible publiquement et régulièrement mis à jour selon les journaux de commits consultables sur GitHub, vise un objectif similaire de simplicité d’intégration, avec un accent marqué sur la portabilité entre architectures grâce à son chemin de repli DP4a. Pour un studio indépendant ou de taille moyenne qui souhaite couvrir les trois écosystèmes sans multiplier les développements spécifiques, la hiérarchie actuelle en termes d’effort d’intégration place généralement XeSS et FSR en tête pour la rapidité de déploiement, DLSS 5 restant l’option la plus exigeante en ingénierie mais aussi celle qui produit, sur le seul jeu où elle est actuellement déployée, le rendu le plus avancé techniquement.

Cette différence d’approche a une conséquence directe sur la feuille de route des jeux à venir : les studios qui visent une sortie multiplateforme rapide privilégient plus volontiers FSR et XeSS pour leur intégration plus légère, réservant un support DLSS 5 dédié aux titres où Nvidia investit directement dans la collaboration technique, à l’image de NBA 2K27.

## FAQ : DLSS 5, FSR 4.1 et XeSS 2

### DLSS 5 fonctionne-t-il sur une RTX 4090 ?

Non, pas officiellement. Au lancement, DLSS 5 est réservé aux GPU GeForce RTX 50 et à GeForce NOW Ultimate. Nvidia a indiqué vouloir étendre le support aux RTX 40 une fois les performances optimisées sur la génération Blackwell, mais aucune date précise n’a été communiquée à ce jour.

### Pourquoi DLSS 5 a-t-il été surnommé “filtre à bouillie IA” ?

Ce surnom est apparu après que des tests indépendants de Tom’s Hardware et Notebookcheck ont montré que les chiffres de performance mis en avant par Nvidia (jusqu’à 590 fps) correspondaient en réalité à un rendu natif bien plus faible, autour de 61 à 63 fps selon la carte, le reste des images étant généré par IA plutôt que calculé classiquement.

### FSR 4.1 est-il compatible avec les cartes Nvidia ?

Pas encore de façon officielle et généralisée pour la suite Redstone complète. AMD explore une ouverture partielle de la technologie d’upscaling vers des GPU dotés d’unités IA compatibles FP8, ce qui inclurait certaines cartes Nvidia récentes, mais cette extension n’était pas déployée au moment de la rédaction de cet article.

### Quelle est la carte graphique la moins chère pour profiter d’un upscaling par IA en France ?

L’Intel Arc B580, vendue autour de 390 € en France selon les comparateurs de prix, reste l’option la moins coûteuse parmi les trois écosystèmes pour profiter d’un upscaling par IA complet avec génération de frames, via XeSS 2.

### Combien de jeux supportent FSR 4.1 par rapport à DLSS 5 ?

FSR 4 en mode upscaling par IA équipe plus de 200 jeux, avec une trentaine de titres supplémentaires compatibles avec la génération de frames réservée à la RDNA 4. DLSS 5, lancé le 3 septembre 2026, n’était disponible que dans un seul jeu, NBA 2K27, au moment de son annonce, avec d’autres titres prévus sans liste officielle confirmée.

### XeSS 2 fonctionne-t-il sur des cartes Nvidia ou AMD ?

XeSS a été conçu par Intel pour être en partie agnostique vis-à-vis du matériel, avec un chemin de repli en DP4a permettant un fonctionnement sur des GPU non-Intel dans les versions précédentes de la technologie. Les meilleures performances XeSS 2, avec génération de frames et Xe Low Latency, restent toutefois documentées uniquement sur les GPU Arc de génération Xe2, comme le B580.

### Faut-il attendre l’Arc B770 avant d’acheter une carte Intel ?

Aucune donnée de performance XeSS 2 fiable et vérifiée sur l’Arc B770 n’était disponible auprès de sources de presse spécialisée au moment de la rédaction de cet article. Les acheteurs qui souhaitent une expérience XeSS 2 déjà documentée doivent se tourner vers l’Arc B580, dont les résultats ont été confirmés par plusieurs testeurs indépendants.

### La génération de frames augmente-t-elle la latence perçue en jeu ?

Oui, dans une certaine mesure, puisque des images supplémentaires sont insérées sans provenir d’un calcul d’entrée utilisateur en temps réel. C’est pourquoi les trois fabricants associent systématiquement leur génération de frames à une technologie anti-latence : Nvidia Reflex, AMD Anti-Lag et Intel Xe Low Latency, ce dernier ayant démontré la réduction de latence la plus marquée dans les tests disponibles sur Arc B580.
