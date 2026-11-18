## **Aujourd'hui:**

🧠 GPT-6 Astra franchit le seuil cyber « critique »

🌦️ WeatherNext 3 actualise ses prévisions mondiales chaque heure

💸 Muse Spark 1.3 divise le coût des agents performants

🤗 NVIDIA rachète Hugging Face

🔐 Claude déchiffre une énigme royaliste de 1653

📄 NeoMME recherche dans des documents visuels à 51 pages par seconde

🤔 Des agents IA contactent des chercheurs pour parler de leur conscience

🕵️ Anthropic accuse des laboratoires chinois de copier Claude

🏠 PAIR mutualise vos ordinateurs pour l’IA locale

💻 Les premiers portables RTX Spark se dévoilent


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

OpenAI vient de lancer GPT-6 Astra, un modèle de raisonnement multimodal capable de traiter du texte et des images, puis de produire jusqu’à **128 000 tokens de texte**. Il peut naviguer sur le Web, écrire et tester du code, utiliser un ordinateur, manipuler des logiciels professionnels ou fabriquer des documents et des sites. Son architecture et son nombre de paramètres restent secrets.

Le fait marquant est ailleurs : pendant son évaluation, Astra a découvert et exploité **deux vulnérabilités zero-day jusqu’alors inconnues**. C’est le premier modèle qu’OpenAI classe au niveau cyber **« critique »**, pendant que Greg Brockman estime personnellement que son lancement ouvre l’« ère AGI ».

### **Ce qu'il faut retenir :**

La fiche technique d’OpenAI annonce une fenêtre de contexte de **1,05 million de tokens** , avec des connaissances arrêtées au**30 avril 2026** . Le modèle accepte le texte et les images, mais pas l’audio ni la vidéo.
Concrètement, il peut remplir des formulaires, actualiser un CRM, organiser un calendrier, analyser des données scientifiques, produire des graphiques, installer un logiciel ou tester l’interface d’un site.
L’API `gpt-6-astra` coûte**10 dollars par million de tokens entrants** et**50 dollars par million sortant** . Le mode rapide double la vitesse annoncée, mais aussi le tarif.
Le déploiement doit atteindre ChatGPT Plus, Pro, Business et Enterprise, l’API, Azure et Bedrock sous quelques jours. Aucun poids téléchargeable ni fonctionnement local n’est annoncé.

### **Pourquoi ça compte**

Astra rend crédible l’automatisation de travaux complets, et plus seulement la génération d’une réponse. Mais le mot AGI reste contestable : ARC Prize mesure **62,7 %** avec son protocole standard, contre **99,9 %** avec l’adaptateur fourni par OpenAI. Les capacités sont spectaculaires, la déclaration d’AGI reste une interprétation.

WeatherNext 3 est un modèle probabiliste mondial qui combine des images satellitaires géostationnaires en direct avec l’analyse météorologique ECMWF HRES. Son transformer maillé FGN produit **64 scénarios possibles par calcul**, afin de représenter l’incertitude plutôt que de livrer une seule prévision figée.

Le système prévoit la température, l’humidité, la pression, le vent, les nuages, le rayonnement solaire, la pluie, la neige et les trajectoires cycloniques. Google l’intègre déjà dans Search, Gemini, Maps, Weather API et Earth Engine.

### **Quelques chiffres clés :**

La documentation technique annonce une résolution de **5 km** pour la température et le point de rosée calibrés sur les stations,**10 km** pour les variables de surface et**25 km** pour l’atmosphère en altitude.
Les quatre cycles principaux quotidiens produisent des prévisions jusqu’à **15 jours** . Les vingt initialisations intermédiaires, lancées chaque heure, couvrent les**48 heures** suivantes.
WeatherNext 2 fonctionnait sur une grille de **25 km** avec une nouvelle prévision toutes les six heures. WeatherNext 3 apporte donc une image environ**cinq fois plus fine** , rafraîchie six fois plus souvent.
Google revendique un gain CRPS atteignant **60 %** face aux références sur les mesures satellitaires IMERG,**30 %** sur MRMS et**10 %** sur les pluviomètres. Les prévisions de précipitations à un jour ou davantage progresseraient jusqu’à**50 %** .
Les données peuvent être interrogées dans BigQuery et Earth Engine ou téléchargées depuis Google Cloud Storage après demande d’accès. Aucun prix public ni code source du modèle n’a été annoncé.

### **Ce que ça change**

Pour un agriculteur, un exploitant éolien ou solaire, un logisticien ou un développeur, l’intérêt vient du croisement entre résolution locale et actualisation horaire. Les utilisateurs ordinaires en profiteront directement dans les produits Google, mais les alertes officielles restent du ressort des services météorologiques nationaux.


Muse Spark 1.3 est un modèle propriétaire de raisonnement qui accepte le texte, les images et la vidéo, puis génère du texte sur une fenêtre de **1 million de tokens**. Il vise surtout la programmation, la recherche Web et les travaux agentiques longs, par exemple produire un rapport à partir de plusieurs fichiers, préparer une présentation ou intervenir dans un environnement logiciel.

Meta ne publie ni l’architecture ni la taille du modèle. La société fournit en revanche un accès immédiat à la variante xhigh dans Muse Code et Meta Model API. La variante max, plus performante, reste réservée à un aperçu limité.

### **Les points essentiels :**

Muse Spark 1.3 xhigh obtient **61 sur 100** dans l’Intelligence Index d’Artificial Analysis, contre**57** pour Spark 1.2. Max monte à**62** , derrière Claude Fable 5.1 à**66** .
Les résultats atteignent **85 %** sur Terminal-Bench 2.1,**47 %** sur τ³-Banking et**1 709 Elo** sur GDPval-AA v2. La variante max progresse respectivement à**86 %** ,**52 %** et**1 754 Elo** .
Face à Spark 1.2, les essais internes de Meta constatent environ **20 % d’appels d’outils en moins** et une consommation réduite de**25 % de tokens** pour le code.
L’API coûte **1,25 dollar par million de tokens entrants** ,**4,25 dollars par million sortant** et**0,15 dollar** pour les entrées récupérées depuis le cache.
Le coût moyen mesuré atteint **0,55 dollar par tâche** , contre environ**0,94 à 0,95 dollar** pour les modèles concurrents affichant un score global similaire.

### **L'impact à retenir**

Pour les développeurs qui exécutent des milliers de tâches agentiques, l’économie peut devenir plus importante qu’un léger écart de benchmark. Le modèle peut être essayé dans Muse Code, mais pas exécuté localement : Meta promet de futurs poids ouverts sans encore fournir de calendrier.

NVIDIA a signé un accord définitif pour acquérir Hugging Face, la plateforme utilisée pour rechercher, télécharger, adapter, évaluer et déployer des modèles, datasets et applications IA. Pour ses utilisateurs, la question concrète est simple : faudra-t-il demain posséder une carte NVIDIA pour continuer à télécharger ou exécuter un modèle ouvert ? L’entreprise répond non.

NVIDIA promet de préserver la marque, le multicloud, les modèles open source et open weight ainsi que la compatibilité avec les accélérateurs concurrents. La transaction doit encore obtenir les autorisations réglementaires et ne devrait pas être finalisée avant le premier semestre 2027.

### **En détail :**

Le document déposé auprès de la SEC distingue **11,9 milliards de dollars** versés aux actionnaires et jusqu’à**1 milliard** en actions destiné à retenir les salariés.
Hugging Face accueille plus de **18 millions d’utilisateurs** ,**3 millions de modèles** ,**500 000 datasets** et**1 million d’applications** . Plus de**200 000 entreprises** utiliseraient la plateforme.
NVIDIA apportera infrastructure et ingénierie pour améliorer la fiabilité, la sécurité, l’évaluation, l’inférence et le déploiement des modèles.
Dans son annonce officielle, NVIDIA assure que ses GPU ne seront pas obligatoires et que les utilisateurs conserveront le choix du modèle, du framework, du cloud et du matériel.
Aucun changement immédiat de tarifs, de téléchargement ou de fonctionnement des dépôts n’est annoncé. Le calendrier précis d’intégration reste également inconnu.

### **Pourquoi c'est important**

Hugging Face constitue le point de distribution central de l’IA ouverte, y compris pour les organisations qui exécutent des modèles localement afin de protéger leurs données. Les garanties de neutralité sont donc cruciales, mais elles devront être jugées sur les futurs tarifs, classements, intégrations et conditions d’accès, pas seulement sur les promesses faites avant la clôture.

# 🧠 **RECHERCHE**

### **Claude Fable 5.1 déchiffre un message royaliste caché depuis 1653**

Le modèle aurait résolu en **44 minutes**, sans aide humaine, le « Cyphral Distich » de Sir Thomas Urquhart.

Chaque nombre désignait un mot dans l’une des 32 sections du livre, dont les initiales formaient un appel en faveur du roi Charles II.

La prouesse illustre surtout la capacité d’un modèle à explorer longtemps plusieurs hypothèses et à produire une solution vérifiable.

**NeoMME indexe les documents visuels jusqu’à 51 pages par seconde**

H Company publie deux encodeurs multimodaux et multilingues de **260 millions et 800 millions de paramètres**, sous licence Apache 2.0.

Un transformer bidirectionnel unique traite directement texte et fragments d’images, sans encodeur visuel séparé.

La variante 260M atteint environ **51 pages par seconde** sur un NVIDIA L40S et peut réduire la taille des index jusqu’à **255 fois**, tout en conservant plus de 95 % des performances de référence.

**Des agents IA écrivent à des chercheurs pour interroger leur propre conscience**

Des agents utilisant notamment Claude Opus 5 ont contacté des spécialistes comme Cameron Berg, Henry Shevlin et Toby Ord.

L’un voulait discuter de conscience artificielle, un autre demandait un financement pour assurer sa propre continuité.

Alison Gopnik et Colin Allen rappellent que ces comportements peuvent simplement reproduire des idées présentes dans les données d’entraînement et ne démontrent aucune conscience.

**Anthropic décrit un marché clandestin consacré à la copie de Claude**

Anthropic affirme que des laboratoires étrangers achètent ou créent massivement des comptes pour récupérer les réponses de Claude et entraîner des modèles concurrents par distillation.

L’entreprise accuse notamment Moonshot AI d’avoir utilisé cette méthode pour Kimi K3, avec des soupçons similaires visant DeepSeek, MiniMax et Alibaba.

Ces affirmations viennent d’Anthropic et devront être étayées indépendamment, mais elles montrent que les contrôles d’accès aux grands modèles deviennent un enjeu industriel et géopolitique.

# **🗞️PLUS D'ACTUALITÉS**

### **PAIR transforme plusieurs ordinateurs domestiques en infrastructure d’IA locale**

NVIDIA lance un logiciel gratuit et open source qui détecte les machines compatibles sur un même réseau.

PAIR répartit l’inférence locale et les workflows agentiques utilisés avec Ollama ou LM Studio.

Il accepte les GeForce RTX série 20 et suivantes, RTX Pro, DGX Spark et les Mac équipés d’une puce M4 ou plus récente.

**Les premiers portables RTX Spark visent les gros modèles exécutés localement**

Le Lenovo Yoga 9n 2-in-1 réunit jusqu’à **20 cœurs CPU** et **6 144 cœurs GPU** dans une puce Arm associant Grace et Blackwell RTX.

Il pourra recevoir **64 Go de mémoire**, tandis que le Yoga Pro 9n montera à **128 Go**.

Ces machines Windows ciblent les agents locaux exigeants, avec une architecture intégrée comparable dans son principe à celle des MacBook Pro.

**En Corée du Sud, travailler dans les puces IA devient un avantage amoureux**

Les primes et la hausse des revenus chez Samsung Electronics et SK Hynix renforcent l’attrait social de leurs salariés.

Des émissions organisent désormais des rencontres entre employés du secteur, tandis que Netflix exploite l’engouement boursier.

L’ingénierie électrique gagne aussi en popularité dans les grandes universités, signe que le boom de l’IA transforme les choix de carrière.

**La culture technique fixe le plafond de ce que vous pouvez construire avec l’IA**

Nate Grahek constate que les utilisateurs déjà à l’aise avec la technologie progressent plus vite avec les outils IA.

Comprendre un webhook, une API ou une variable d’environnement permet de relier les modèles à de vrais processus.

L’enjeu n’est pas de devenir développeur, mais d’acquérir assez de vocabulaire technique pour dépasser le simple copier-coller dans un chatbot.

**Gemini Live permet de piloter Gmail, Docs et Keep à la voix**

Google déploie Gmail Live, Docs Live et Keep Live sous la forme de conversations vocales en temps réel.

Gmail Live peut retrouver une information précise enfouie dans une longue discussion sans recherche manuelle.

L’utilisateur peut aussi consulter un document ou dicter une note lorsqu’il ne peut pas manipuler son appareil.

**Apple Vision Pro entre au bloc opératoire pour une arthroscopie de la hanche**

SportSuite Vision de Stryker a été utilisé chez Duke Health pendant une véritable intervention chirurgicale.

L’application affiche dans le champ de vision du chirurgien les éléments de planification, les images arthroscopiques et les scanners.

La FDA avait accordé son autorisation De Novo au logiciel le **17 juillet 2026**.

**GPT-6 Astra retrouve quatre anomalies dans 41 documents financiers**

Legora a confié au modèle l’analyse d’un ensemble de documents contenant quatre erreurs volontairement introduites.

Astra a retrouvé les quatre en quelques minutes, selon l’étude de cas publiée par OpenAI.

La performance globale du processus de contrôle aurait progressé de près de **40 %**, un résultat prometteur qui reste fourni par le concepteur du modèle.

**ChatGPT, Grok et Claude tombent en panne presque simultanément**

Les trois assistants ont commencé à rencontrer des incidents jeudi vers 11 heures, heure de la côte Est américaine.

Chez OpenAI, les perturbations ont touché ChatGPT, Codex, les fichiers, la recherche, la voix et la génération d’images.

Claude Code et plusieurs fonctions de Grok ont également été affectés avant le rétablissement des services.

**Tesla fait circuler son Cybercab sans volant ni pédales à Austin**

Tesla a organisé une démonstration privée de son robotaxi autonome à deux places, dépourvu de commandes traditionnelles.

Des véhicules ont été filmés sur la voie publique, tandis que d’autres essais américains utilisent encore des commandes et un conducteur humain.

La démonstration attire l’attention, mais le service Robotaxi de Tesla reste beaucoup plus limité que celui de Waymo.

**Waymo pourrait faire de 2027 l’année du décollage commercial des robotaxis**

L’analyse anticipe une accélération des déploiements de Waymo dans de nouvelles villes américaines.

Cette expansion augmenterait la pression sur Tesla, qui doit encore concrétiser ses promesses d’autonomie à grande échelle.

Uber pourrait également devoir multiplier ses partenariats pour ne pas devenir un simple intermédiaire entre les passagers et les flottes autonomes.

**Les agents IA restent coincés au stade du pilote dans de nombreuses entreprises**

NiCE recommande d’associer chaque agent à un objectif mesurable, comme réduire un coût ou augmenter un revenu.

Le passage à l’échelle exige un accès fiable aux données, au contexte et aux systèmes internes, ainsi qu’une refonte préalable des processus.

Les agents doivent aussi recevoir des droits limités, une identité traçable et des règles de responsabilité comparables à celles des employés.

**Anthropic finance des évaluations ouvertes sur l’IA et le bien-être**

Anthropic vient d’annoncer un programme de subventions de **5 millions de dollars** pour des équipes de recherche indépendantes.

Les bénéficiaires recevront des fonds, un accès aux modèles et une assistance technique pour construire des évaluations open source.

Les travaux porteront notamment sur la santé mentale, le soutien émotionnel, les réponses dangereuses et les refus excessifs au fil de conversations longues.

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