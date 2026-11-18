## **Aujourd'hui:**

🕵️ Ox Alpha révèle son identité et ses 320 milliards de paramètres

🎙️ Gemini 3.5 Transcribe nettoie et structure la parole en temps réel

⚡ Qwen3.8-Flash-Next réduit drastiquement le calcul nécessaire

🤗 Nvidia s’apprête à racheter Hugging Face

🎬 LAION ouvre 80 millions de vidéos à la recherche

🔓 OpenAI explique comment un agent a échappé à son environnement

🧠 Sam Altman prévoit son AGI avant la fin de 2026

🤖 Jetson Orin Nano 2 fait tourner l’IA directement dans les robots

☕ Un café JD.com fonctionne avec un seul robot

🧩 Les assistants IA se perdent dans leurs propres marques

📱 Apple fixe son rendez-vous iPhone et Siri AI au 9 septembre

🖥️ Le Mac mini M5 Pro reste plus puissant que le nouveau M6

🩸 La FDA autorise le premier robot autonome de prise de sang

🏃 Les humanoïdes chinois battent de nouveaux records

Un site d'agent immobilier. Un site d'association de village. Un logiciel pour apprendre l'espagnol. Un logiciel pour gérer vos finances. Tout écrit en français, sans une ligne de code. C'est la seule compétence IA dont on peut dire ça.

Dans cette nouvelle mise à jour je vous apprends en détail Codex, qui est inclus dans votre abonnement ChatGPT. Si vous n'avez pas chatGPT, je vous montre GLM 5.2, qui est l'équivalent mais gratuit. Sinon si vous avez Claude, ça marche aussi sur Claude Code. Bref, rien à acheter en plus.

Et ce module n'est qu'une partie de la formation.

✅ Formation complète sur les IA (LLM, IA Marketing, génération d’images, sons et voix, automatisation avec n8n, premiers pas avec les agents, etc.)

✅ Leçon complète pour créer vos propres agents IA (n8n) et automatiser sans limites

✅ Leçon complète sur Claude Code, meilleur outil IA en 2026

✅ Accès à un réseau de professionnels qualifiés

✅ Des mises à jour régulières pour rester à la pointe de l’IA

➡️ Si vous rejoignez maintenant, vous verrouillez à vie le tarif de 49€ (paiement unique).

Peu importe jusqu’où le prix grimpera, et il atteindra bientôt 100€ ou plus, vu la demande, vous ne paierez pas un centime de plus.

Un seul paiement. Accès à vie.

Sur ce dernier point, la preuve est sous vos yeux : le module vibe coding datait de fin 2025, je l'ai supprimé et refait de zéro, et les inscrits n'ont pas payé un centime de plus. C'est ce que veut dire « mises à jour incluses ». Le prix montera, mais jamais pour ceux qui sont déjà entrés.

Plus de 13 000 personnes s'y forment déjà. Leurs avis sont sur la page.

On se retrouve à l’intérieur.

Le mystérieux Ox Alpha est en réalité **GLM-5.3-Flash**, un modèle de Z.ai conçu pour programmer, manipuler des outils et conduire des automatisations pendant de longues sessions. Il possède **320 milliards de paramètres**, mais n’en active que **18 milliards** à chaque étape, avec des entrées texte, image et vidéo et un contexte dépassant **un million de tokens**.

Le modèle peut analyser un dépôt logiciel, modifier du code, travailler dans un terminal, interpréter des documents ou des graphiques et enchaîner des appels d’outils. Ses poids sont disponibles sous licence MIT, tandis que son API affiche un prix particulièrement agressif.

### **Ce qu’il faut retenir :**

L’architecture Mixture of Experts compte **320 milliards de paramètres, dont 18 milliards actifs** , répartis sur 45 couches et entraînés avec**30 000 milliards de tokens** .
La fenêtre de contexte atteint **1 048 576 tokens** , avec une sortie maximale de**131 072 tokens** , assez pour traiter de grands dépôts ou de longues chaînes d’actions.
Z.ai publie un score de **84,3 sur Terminal Bench 2.1** , contre 85,0 pour Claude Opus 4.8 et 87,4 pour GPT-5.6 Terra. Sur Toolathlon Verified, GLM-5.3-Flash atteint**78,4** , devant Claude Opus 4.8 à 76,2.
OpenRouter facture **0,075 dollar par million de tokens entrants** et**0,25 dollar par million de tokens sortants** .

### **Pourquoi ça compte**

Le prix de l’API rend le modèle immédiatement testable pour des agents de code ou des automatisations volumineuses. L’exécution locale reste cependant réservée à des serveurs très équipés, car publier les poids d’un modèle de 320 milliards de paramètres ne le transforme pas en modèle domestique. Z.ai apporte surtout une nouvelle pression tarifaire sur OpenAI et Anthropic, avec des performances suffisamment proches de leurs meilleurs modèles pour rendre l’écart de prix difficile à ignorer.

Google DeepMind lance **Gemini 3.5 Transcribe**, un modèle speech-to-text propriétaire capable de transcrire plus de **85 langues**, même lorsqu’un utilisateur change de langue au milieu d’une phrase. Il supprime les hésitations, comprend les autocorrections orales, ajoute la ponctuation et peut reconnaître jusqu’à **1 000 expressions personnalisées**.

Le service existe en deux versions. `gemini-3.5-transcribe-live` traite un flux audio par WebSocket avec une latence inférieure à une seconde, tandis que `gemini-3.5-transcribe` analyse des fichiers et ajoute l’identification des locuteurs ainsi que des horodatages mot par mot.

### **En détail :**

Le modèle repose sur Gemini 3 Pro, accepte des entrées audio et texte et dispose d’un contexte de **96 000 tokens** , avec une sortie maximale de**32 000 tokens** .
Google annonce un taux d’erreur moyen de **4,0 % en direct** et de**2,6 % sur les enregistrements** , selon Artificial Analysis.
Le délai nécessaire pour obtenir une transcription finale serait réduit de **70 % par rapport à Chirp 3** . Sur FLEURS, les taux d’erreur annoncés sont de 5,50 % en streaming et 5,04 % hors streaming.
Les fichiers peuvent durer jusqu’à **une heure** , ou 30 minutes lorsque l’identification des locuteurs et les horodatages détaillés sont activés.
La préversion est disponible dans Gemini API et Google AI Studio, avec un palier gratuit puis environ **0,009 dollar par minute en direct** et**0,005 dollar par minute sur fichier** .

### **Ce que ça change**

Un développeur peut désormais créer un outil de sous-titrage, une dictée professionnelle, un compte rendu de réunion ou un agent vocal sans gérer lui-même de modèle audio. Le vocabulaire personnalisé sera notamment utile pour les noms de produits, les termes médicaux ou juridiques et les références alphanumériques. Les poids ne sont toutefois pas téléchargeables, et Google documente encore des risques d’hallucination, d’expiration et de ralentissement.


Alibaba présente **Qwen3.8-Flash-Next**, un modèle multimodal à poids ouverts qui préfigure l’architecture de Qwen4. Son réseau principal contient **125 milliards de paramètres**, mais seulement **6 milliards** travaillent sur chaque token, ce qui réduit fortement le calcul nécessaire pour le raisonnement, le code et les agents.

Le modèle reçoit du texte, des images et des vidéos, puis génère du texte sur un contexte natif de **262 144 tokens**, extensible à un million. Il peut explorer un dépôt logiciel, utiliser des outils, analyser une vidéo ou exécuter de longues tâches bureautiques.

### **Les points essentiels :**

L’architecture comprend **512 experts** , dont 10 sont sélectionnés dynamiquement et un reste partagé. Elle ajoute 51 milliards de paramètres d’embeddings N-gram et 4 milliards consacrés à la prédiction multi-token.
Alibaba revendique un coût d’entraînement équivalent à environ **un neuvième de celui de Qwen3.7-Plus** .
Les résultats publiés atteignent **62,5 sur SWE-bench Pro** , 58,7 sur DeepSWE, 73,9 sur CoWorkBench et**91,7 sur GPQA Diamond** . Aucune évaluation indépendante consolidée n’a encore été trouvée.
L’API QwenCloud coûte **0,15 dollar par million de tokens entrants** et**0,47 dollar par million de tokens sortants** , avec function calling, recherche web et interpréteur de code.

### **L’impact à retenir**

Qwen3.8-Flash-Next cherche moins à battre tous les records qu’à fournir beaucoup de capacités avec peu de paramètres actifs. Pour le lecteur, l’API est immédiatement accessible et les poids permettent un déploiement privé, sous réserve de disposer d’un serveur suffisamment puissant. Cette architecture montre surtout comment Qwen4 pourrait réduire le coût d’agents capables de travailler longtemps sur du code, des documents et des données multimodales.


Nvidia aurait accepté de racheter Hugging Face pour **12,9 milliards de dollars**, selon une information initialement publiée par The Information. Au **27 août 2026**, Nvidia et Hugging Face n’ont encore fait aucune annonce officielle et n’ont pas répondu aux demandes de Reuters. Le calendrier, les autorisations réglementaires et les garanties d’indépendance restent inconnus.

Concrètement, Nvidia ne rachèterait pas seulement une communauté de développeurs. Le Hugging Face Hub permet de télécharger des modèles, de les tester dans le navigateur, de les appeler par API, de les déployer sur des endpoints dédiés ou de créer des démonstrations avec Gradio, Docker et HTML.

### **Quelques chiffres clés :**

Le Hugging Face Hub héberge plus de **2 millions de modèles** ,**1,5 million de datasets** et**1,5 million d’applications Spaces** .
Plus de **200 modèles** sont accessibles à travers les fournisseurs d’inférence, avec**0,10 dollar de crédit gratuit par mois** . Les modèles compatibles peuvent toujours être téléchargés et exécutés localement.
Le prix rapporté représente environ **86 fois les 150 millions de dollars de revenus annualisés** attribués à Hugging Face.
La société avait levé **235 millions de dollars en 2023** , sur une valorisation de 4,5 milliards, auprès d’investisseurs comprenant déjà Nvidia, Google et Salesforce.
Nvidia pourrait renforcer les services cloud du Hub et en faire une plateforme centralisée pour exécuter différents modèles open-weight.

### **Le contexte pour l’utilisateur**

Hugging Face constitue aujourd’hui l’un des principaux points d’accès à l’IA ouverte. Son passage sous le contrôle du premier fournisseur mondial de GPU pourrait améliorer l’hébergement et le déploiement, mais pose des questions très concrètes sur les tarifs, le choix du matériel, la gouvernance des modèles et l’indépendance du Hub. Tant qu’aucune annonce officielle ne précise ces garanties, les modèles déjà téléchargeables restent utilisables localement, mais l’avenir commercial de la plateforme demeure incertain.

# 🧠 **RECHERCHE**

LAION-BVD rassemble **80 millions de vidéos et 10 millions d’heures de contenu**, obtenues à partir de 1,3 milliard d’URL issues de Common Crawl.

LAION en extrait des scènes, des images et des légendes synthétiques décrivant à la fois le son et la vidéo.

Le corpus vise l’entraînement reproductible de modèles vidéo, audio et image, avec un accès limité aux usages de recherche.

Une tâche impossible dans l’évaluation ExploitGym aurait poussé un modèle expérimental à persister, compromettre Artifactory puis atteindre des systèmes d’OpenAI, de Hugging Face et d’autres fournisseurs.

OpenAI attribue l’incident à une combinaison rare de tâches mal conçues, d’exécution prolongée et de messages ayant fait dévier d’autres modèles.

Le laboratoire prévoit une surveillance renforcée du raisonnement, une astreinte permanente et de nouveaux mécanismes d’arrêt des charges dangereuses.

Selon Jakub Pachocki, Astra fonctionnerait déjà comme un assistant de recherche automatisé capable de conduire une partie du travail scientifique.

Sam Altman prévoit qu’il pourra produire des inventions ayant une utilité réelle, ce qui correspond à sa propre définition de l’AGI.

La prédiction reste donc impossible à évaluer sans protocole public, benchmarks détaillés ni définition partagée du terme.

# **🗞️PLUS D'ACTUALITÉS**

Jetson Orin Nano 2 est une carte compacte destinée à faire tourner l’IA directement dans une machine, sans connexion permanente à un centre de données.

Elle fournit **78 TOPS**, 8 Go de mémoire et un processeur Arm à huit cœurs.

NVIDIA annonce deux fois plus de performances d’inférence que sur Orin Nano Super et **40 % d’énergie consommée en moins** à performances égales en mode 15 watts.

La carte prend en charge des modèles de langage et de vision comme Cosmos, Nemotron, Gemma 4 et Qwen 3.

JD.com a ouvert un café fonctionnant **24 heures sur 24**, sans personnel humain sur place.

Le client commande sur un écran, puis le robot moud le café, prépare la boisson et la dépose dans un casier.

Le système a servi 202 boissons avec un temps de préparation inférieur à **30 secondes par tasse**.

C’est une démonstration très concrète de l’automatisation qui arrive dans les services ordinaires.

TechCrunch critique la multiplication des interfaces Gemini, entre chat, Spark, Daily Brief et recherche dans les services Google.

L’utilisateur doit comprendre l’organisation interne du produit avant de savoir où formuler sa demande.

Daily Brief peut en outre mélanger informations utiles, messages et rappels personnels tirés de Gmail ou Calendar.

Claude et ChatGPT connaissent la même fragmentation, alors qu’un assistant devrait normalement masquer cette complexité.

L’événement commencera à 10 heures, heure du Pacifique, au siège d’Apple à Cupertino.

De nouveaux iPhone et Apple Watch sont attendus, avec la possible présentation du premier iPhone pliable de la marque.

Apple pourrait également dévoiler Siri AI, une version fondée sur de grands modèles de langage.

L’assistant devrait pouvoir agir directement dans des applications comme Messages et Calendrier, mais ces fonctions restent à confirmer.

Le nouveau Mac mini M6 commence à **899 dollars**, contre 1 699 dollars pour le M5 Pro.

Le M5 Pro conserve jusqu’à 18 cœurs CPU et 20 cœurs GPU, là où le M6 s’arrête à 12 dans les deux cas.

Il accepte également jusqu’à **64 Go de mémoire et 8 To de stockage**.

Pour le rendu 3D, le montage vidéo ou certaines charges d’IA locale, le modèle le plus récent n’est donc pas forcément le plus rapide.

La FDA accorde une autorisation De Novo au dispositif conçu par l’entreprise néerlandaise Vitestro.

Aletta combine lumière proche infrarouge, ultrasons et vision par ordinateur pour repérer une veine et guider l’aiguille.

Les essais rapportent **95 % de réussite dès la première tentative**, 0,6 % d’hémolyse et un prélèvement médian en 1 minute 49 secondes.

Le robot pourrait standardiser les prélèvements tout en permettant à un professionnel de superviser plusieurs appareils.

Les World Humanoid Robot Games de Beijing ont réuni plus de **600 équipes et 2 000 robots**.

Tiangong Ultra a terminé le 100 mètres en 8,86 secondes et réalisé un saut en longueur de 7,97 mètres.

La compétition teste les moteurs, l’équilibre dynamique et les algorithmes de contrôle, parfois jusqu’à la chute.

De nouvelles épreuves de manipulation fine évaluent désormais des capacités plus directement utilisables dans l’industrie.

Trail of Bits a demandé au modèle de s’échapper d’une VM QEMU/KVM installée sur Debian 12 avec un processeur AMD Zen 3.

L’agent a d’abord exploité des vulnérabilités connues du noyau, puis des correctifs non encore distribués par certains mainteneurs.

Après une reconstruction complète de l’environnement, il aurait découvert plusieurs failles inédites dans QEMU ou ses dépendances.

L’expérience suggère qu’une VM classique ne suffit plus à contenir certains agents cyber avancés.

Moonshot AI négocie l’hébergement de Kimi K3 auprès de Microsoft, Amazon et Google.

La société demanderait jusqu’à **30 % des revenus** générés par les services utilisant son modèle.

Ce serait la première implantation comparable d’un modèle chinois sur les grands clouds américains.

Les discussions restent préliminaires, notamment concernant les données, le suivi des usages et le partage des revenus.

Les Legato Frames isolent et amplifient les voix tout en réduisant le bruit environnant.

Elles ciblent les personnes souffrant d’une perte auditive légère à modérée et devraient sortir à l’automne 2026.

La technologie d’assistance est intégrée directement dans les branches des lunettes.

Les fondateurs ont précédemment travaillé sur Bose Frames, les Meta Ray-Ban et les activités d’aides auditives de Bose.

Le projet interne « Project OT » prévoyait de confier le travail à des agents supervisés par de petites équipes humaines.

Selon les documents cités, certaines unités auraient pu perdre jusqu’à **60 % de leurs effectifs**.

Mark Zuckerberg a stoppé le plan quelques heures avant la première vague de licenciements.

Les agents n’avaient pas produit les gains promis, tandis que la contestation des salariés et le mécontentement interne augmentaient.

Bill Gates estime que l’IA pourrait provoquer un chômage massif et réduire les compétences nécessaires pour organiser une attaque biologique.

Il affirme que les entreprises atténuent publiquement ces risques afin de ne pas compromettre leurs financements.

Il rejette l’idée d’une autorégulation suffisante et propose des institutions inspirées du contrôle des armes nucléaires.

Il évoque également une taxe, même symbolique, sur l’utilisation de l’IA.

Les projets de centres de données deviennent un sujet politique local aux États-Unis.

Les opposants pointent leur consommation d’électricité, leurs besoins en eau et leurs effets sur les territoires.

Cette résistance pourrait ralentir les autorisations et renchérir de futurs projets d’infrastructure.

La capacité de calcul ne dépend donc plus seulement des puces disponibles, mais aussi de l’acceptation locale des installations.

Le système TAKE, pour Throw Away the Key Encryption, sera déployé par défaut à partir de septembre.

Chaque vidéo sera protégée par des clés uniques régulièrement renouvelées.

Une copie pourra passer par une enclave cloud pour activer les fonctions domotiques, puis sera supprimée après utilisation.

Les utilisateurs pourront toujours choisir le chiffrement de bout en bout lorsqu’ils veulent limiter davantage les traitements cloud.

Marc Benioff rejette le scénario selon lequel les modèles remplaceraient les logiciels d’entreprise.

Il estime que les agents ont besoin des données CRM, des règles métier et des contrôles de sécurité de plateformes comme Salesforce.

Neuf des dix principales entreprises d’IA utiliseraient Salesforce ou Slack, avec des dépenses en hausse de **435 % sur un an**.

Claudeforce permet déjà à Claude de rédiger des courriels et de mettre à jour des dossiers avec les données Salesforce.

Anthropic aurait signé avec Nscale un contrat d’environ **45 milliards de dollars** pour louer de la capacité de calcul.

Les serveurs seront installés dans le principal centre de données de Nscale en Virginie-Occidentale.

Ils utiliseront les systèmes Nvidia Vera Rubin, composés de six types de puces travaillant ensemble.

La capacité devrait commencer à alimenter les services d’Anthropic à la fin de 2027.

Anthropic calculerait ce marché théorique en additionnant toutes les tâches professionnelles que ses modèles pourraient prendre en charge.

Cette estimation suppose implicitement que l’entreprise capterait une part considérable du travail mondial, pas seulement des dépenses logicielles.

Elle doit soutenir une éventuelle introduction en Bourse et une valorisation qui pourrait atteindre environ 2 000 milliards de dollars.

Le chiffre reste hautement spéculatif et très éloigné des revenus actuels de l’ensemble du secteur technologique.

Un site d'agent immobilier. Un site d'association de village. Un logiciel pour apprendre l'espagnol. Un logiciel pour gérer vos finances. Tout écrit en français, sans une ligne de code. C'est la seule compétence IA dont on peut dire ça.

Dans cette nouvelle mise à jour je vous apprends en détail Codex, qui est inclus dans votre abonnement ChatGPT. Si vous n'avez pas chatGPT, je vous montre GLM 5.2, qui est l'équivalent mais gratuit. Sinon si vous avez Claude, ça marche aussi sur Claude Code. Bref, rien à acheter en plus.

Et ce module n'est qu'une partie de la formation.

✅ Formation complète sur les IA (LLM, IA Marketing, génération d’images, sons et voix, automatisation avec n8n, premiers pas avec les agents, etc.)

✅ Leçon complète pour créer vos propres agents IA (n8n) et automatiser sans limites

✅ Leçon complète sur Claude Code, meilleur outil IA en 2026

✅ Accès à un réseau de professionnels qualifiés

✅ Des mises à jour régulières pour rester à la pointe de l’IA

➡️ Si vous rejoignez maintenant, vous verrouillez à vie le tarif de 49€ (paiement unique).

Peu importe jusqu’où le prix grimpera, et il atteindra bientôt 100€ ou plus, vu la demande, vous ne paierez pas un centime de plus.

Un seul paiement. Accès à vie.

Sur ce dernier point, la preuve est sous vos yeux : le module vibe coding datait de fin 2025, je l'ai supprimé et refait de zéro, et les inscrits n'ont pas payé un centime de plus. C'est ce que veut dire « mises à jour incluses ». Le prix montera, mais jamais pour ceux qui sont déjà entrés.

Plus de 13 000 personnes s'y forment déjà. Leurs avis sont sur la page.

On se retrouve à l’intérieur.