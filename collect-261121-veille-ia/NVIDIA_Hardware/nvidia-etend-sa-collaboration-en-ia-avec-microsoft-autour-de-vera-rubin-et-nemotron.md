---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-etend-sa-collaboration-en-ia-avec-microsoft-autour-de-vera-rubin-et-nemotron
title: "NVIDIA étend sa collaboration en IA avec Microsoft autour de Vera Rubin et Nemotron"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["microsoft", "modeles", "nvidia", "nemotron", "foundry", "vera", "rubin", "donnees"]
source: https://www.unite.ai/fr/nvidia-expands-microsoft-ai-collaboration-with-vera-rubin-and-nemotron/
source_anchor: ""
source_lines: [1, 37]
section: "NVIDIA_Hardware"
---
## Points clésNVIDIA et Microsoft étendent l'informatique confidentielle à l'infrastructure d'IA à…

Voir plus

- NVIDIA et Microsoft étendent l'informatique confidentielle à l'infrastructure d'IA à l'échelle du rack afin que les clients réglementés puissent vérifier les environnements avant de divulguer des données sensibles.
- EY a adapté les modèles Nemotron avec l'entraînement programmé et a rapporté une amélioration de la précision de 55 % ainsi qu'une réduction des coûts de 15x par rapport aux modèles frontières propriétaires.
- Le partenariat s'étend désormais au-delà d'Azure vers les systèmes sur site, les appareils de périphérie industrielle et les PC Windows Cloud via Foundry Local et les GPU RTX PRO.

Le 19 novembre 2026, à Microsoft Ignite, NVIDIA a annoncé un élargissement de sa collaboration en IA avec Microsoft, qui couvre les systèmes Vera Rubin NVL72 exploités dans les centres de données Azure, une prise en charge élargie des modèles ouverts Nemotron dans Microsoft Foundry, ainsi que des options de déploiement pour les systèmes sur site, les appareils de périphérie industriels et les PC Windows Cloud.

Les deux entreprises élargissent l’accès aux machines virtuelles Azure propulsées par Vera Rubin NVL72 dans le cadre de la préversion Vera Rubin d’Azure. Ces systèmes sont conçus pour le raisonnement et les applications agentiques, ainsi que pour l’entraînement et l’inférence de modèles à grande échelle. La préversion couvre donc à la fois le développement et le déploiement de modèles.

Au-delà de la capacité de calcul, Microsoft et NVIDIA s’emploient à étendre l’informatique confidentielle aux infrastructures d’IA sur serveurs bare metal et à l’échelle du rack, afin de protéger les informations exclusives et réglementées. NVIDIA Confidential Computing et les mécanismes d’attestation aident les clients à vérifier l’environnement informatique avant d’y transférer des modèles, des données ou des clés sensibles. Les travaux en cours visent à étendre ces protections à l’ensemble des infrastructures interconnectées des usines d’IA. Les entreprises collaborent également à l’amélioration de l’efficacité énergétique grâce à NVIDIA MaxLPS, afin d’aider les centres de données à tirer davantage parti de la puissance disponible.

Microsoft utilise également la technologie NVIDIA dans ses propres activités. Microsoft AI utilise Dynamo pour l’inférence de génération de trajectoires en apprentissage par renforcement pendant le post-entraînement, processus très exigeant en calcul qui génère les expériences utilisées pour améliorer les modèles. Dynamo coordonne l’inférence sur une vaste flotte de GPU, en optimisant les performances et en contribuant à gérer les requêtes, les mises à jour des modèles et la reprise après des défaillances dans un déploiement reposant sur les travaux d’ingénierie conjoints de Microsoft AI et NVIDIA. Microsoft utilise aussi des GPU RTX PRO 6000 Blackwell pour les modèles de recherche de Bing.

## Personnalisation des modèles ouverts dans Foundry

Pour les développeurs d’entreprise, l’offre sans serveur Programmed Training de Microsoft permet de contrôler les flux de travail de post-entraînement, tandis que Foundry gère l’infrastructure sous-jacente. Foundry Managed Compute permet en outre de déployer des poids de modèles personnalisés, reliant ainsi l’adaptation des modèles à leur déploiement par l’intermédiaire des services gérés de Microsoft.

NVIDIA a indiqué qu’EY avait utilisé Programmed Training pour adapter des modèles ouverts Nemotron à la classification de dossiers d’achats d’entreprise. Les résultats rapportés font état d’une amélioration de 55 % de la précision sur le jeu de données d’évaluation d’EY et d’une réduction des coûts d’un facteur 15 par rapport aux modèles propriétaires de pointe, ce qui constitue un exemple mesuré de personnalisation pour une tâche métier spécialisée.

La gamme Nemotron 3.5 élargie dans Foundry Managed Compute comprend Super, destiné aux agents exécutant des tâches de longue durée dans les domaines du codage, de la recherche et des processus métier, ainsi que Lightning, conçu pour les applications qui exigent des réponses rapides et l’utilisation d’outils. Embed et Rerank prennent en charge la recherche d’informations, Parse extrait des informations structurées de documents et Content Safety détecte les contenus préjudiciables. NVIDIA a annoncé la prochaine disponibilité de Bolt, destiné au raisonnement à haut débit et à faible latence, et de Transcribe, pour la reconnaissance vocale.

## Des déploiements au-delà d’Azure

La prise en charge élargie de Nemotron dans Foundry Local apporte le raisonnement, le codage et les agents capables d’utiliser des outils aux environnements Azure Local, tandis que des modèles de recherche aident les applications à trouver des éléments probants dans des textes, des images de documents, des graphiques et des tableaux. NVIDIA a indiqué qu’Azure Local prendrait en charge les GPU RTX PRO 6000 Blackwell de partenaires matériels pour les déploiements d’IA sur site.

Azure Local s’étend également à Jetson Thor et Jetson Orin pour des usages industriels, notamment la vision industrielle, la robotique et les opérations autonomes, tandis que des modèles Cosmos d’IA physique sont intégrés à Foundry Local. Ces ajouts élargissent la collaboration, qui ne se limite plus à l’infrastructure centralisée du cloud et s’étend à des systèmes situés au plus près des données générées par les activités industrielles.

NVIDIA a indiqué que les deux entreprises présenteraient à Ignite des expériences locales fondées sur de grands modèles de langage sur des PC Windows équipés de RTX Spark. Pour les PC Windows 365 Cloud accélérés par GPU, Direct Stream prend en charge jusqu’à deux flux 4K à 60 images par seconde, avec une faible latence, même sur des réseaux difficiles. Les inscriptions à une préversion limitée sont ouvertes ; une préversion publique est prévue pour début 2027.

## Outils scientifiques et enjeux pour les entreprises

BioNeMo Agent Toolkit et ALCHEMI Toolkit de NVIDIA rejoignent également le catalogue Microsoft Discovery, qui s’enrichit de modèles pour les sciences de la vie, la découverte de médicaments, la science des matériaux et la chimie sur la plateforme de recherche agentique de Microsoft. BioNeMo Agent Toolkit permet aux chercheurs d’exécuter des flux de travail en biologie computationnelle à l’aide du langage naturel, des agents coordonnant les outils nécessaires et les appels aux modèles.

La collaboration couvre ainsi à la fois l’offre et l’utilisation de capacités de calcul pour l’IA : Microsoft exploite la technologie NVIDIA dans ses propres activités de développement de modèles et de recherche, tout en proposant aux entreprises des infrastructures et des services de modèles connexes. Le déploiement annoncé se fera par étapes : des systèmes sont déjà exploités dans Azure, une préversion limitée de Direct Stream est ouverte aux inscriptions et d’autres modèles sont encore à venir.
