## **Aujourd'hui:**

📜 Une IA traduit le cunéiforme dans les deux sens, en libre accès

🇨🇳 GLM-5.3 prend la tête des modèles ouverts, mais les poids sont repoussés

🌀 Stripe invoque la singularité pour rester hors de la bourse

🔒 Anthropic refuse de sortir « Model 2 », plus puissant que Claude

🏺 Les rouleaux d'Herculanum livrent des livres inconnus de Philodème

🤖 Des bras robotiques qui apprennent en regardant une vidéo

🧬 Claude conçoit des protéines avec 35% de réussite

🎓 Google transforme Search et Gemini en salle de révision

💧 Les watermarks invisibles de Claude contournés en quatre heures

⚡ Codex avale en deux semaines une migration prévue sur cinq ans

🍎 Meta AI débarque sur Mac

🩸 La FDA approuve un robot qui fait les prises de sang

🛡️ La NSA alerte sur des exploits industriels écrits par IA

☢️ Le réacteur de Bill Gates se branche sur les data centers IA

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

Un chercheur de l'USC Viterbi School of Engineering, Zhaohui Wang, a publié TabletCraft, un modèle de traduction entraîné sur **116 000 exemples** d'akkadien vers l'anglais. Il atteint un score BLEU de **49,1**, l'indicateur standard de qualité en traduction automatique, contre 37,5 pour le meilleur système précédent. Surtout, il fait quelque chose que personne n'avait publié avant : il traduit aussi dans l'autre sens, de l'anglais vers l'akkadien, et rend le résultat en signes cunéiformes.

Architecture basée sur **ByT5** , un modèle de traduction spécialisé et non un grand modèle de langage généraliste. Le nombre de paramètres n'a pas été publié.
Validé sur le jeu de test Akkademia (**2 812 exemples** ) :**49,1** de BLEU en akkadien vers anglais,**48,5** dans le sens inverse, une première.
Un convertisseur de signes intégré couvre **14 240 correspondances** , soit**95,3%** du répertoire, et affiche la traduction en signes Unicode puis en image de tablette d'argile virtuelle.
Publié en open source sous licence Creative Commons BY 4.0, avec code, modèle pré-entraîné, interface en ligne de commande et démo web sur GitHub. L'installation tient en une commande : `pip install cuneiscribe` .
Le travail a été présenté en juillet 2026 et accepté au workshop C3NLP de la conférence ACL 2026, avec un papier détaillé.

Environ **500 000 tablettes** cunéiformes dorment dans les réserves des musées du monde, et seule une fraction infime a été traduite. Le goulot d'étranglement n'a jamais été technique : il tient au fait que quelques centaines de personnes seulement savent encore lire couramment ce système d'écriture. Un modèle installable en une ligne de commande sur une machine personnelle change la nature du problème. N'importe quel curieux, professeur ou étudiant peut désormais interroger un texte vieux de cinq millénaires sans passer par un assyriologue. Le sens inverse, écrire en cunéiforme, relève davantage du plaisir que de la science, mais il en dit long : l'outil s'adresse au grand public autant qu'aux chercheurs.

Z.ai, la startup chinoise anciennement connue sous le nom de Zhipu AI, a lancé GLM-5.3 le 14 août. Le modèle décroche **60 points** à l'Artificial Analysis Intelligence Index, à égalité avec Kimi K3 en tête des modèles ouverts et **7 points** devant son prédécesseur. Mais la publication des poids, c'est à dire la partie qui rend un modèle réellement ouvert, a été repoussée : GLM-5.3 s'est révélé trop efficace pour détecter des failles de sécurité dans du code.

Même architecture de base que GLM-5.2, les paramètres ne sont pas publiés. Les gains viennent entièrement d'un post-entraînement amélioré. Le modèle est orienté texte et code au lancement, sans nouvelle modalité.
Score Elo de **1770** sur le benchmark agentique GDPval-AA v2, contre 1524 pour GLM-5.2. C'est la deuxième place mondiale, derrière le seul Claude Opus 5 (1855), qui est propriétaire.
**0,68 $ par tâche** , contre 0,84 $ pour Kimi K3, son rival ouvert direct, soit**19% moins cher** . GLM-5.2 restait à 0,44 $.
Accessible dès maintenant via l'API Z.ai, sur un endpoint compatible OpenAI, et via le GLM Coding Plan à **18 $ par mois** en Lite, 80 $ en Pro, 168 $ en Max, avec une remise annuelle qui descend le Lite à 12,60 $. La variante`glm-5.3[1m]` ouvre une fenêtre de contexte de**1 million de tokens** , utilisable dans Claude Code.
Les poids ouverts sont attendus vers le 28 août, réservés pour l'instant à des partenaires sécurité. Aucun dépôt Hugging Face officiel, donc aucune installation locale possible à ce stade.

Pour la première fois, un modèle facturé moins cher que ses concurrents ouverts talonne le meilleur modèle propriétaire du marché sur les tâches d'agents. Mais le vrai signal est ailleurs : un laboratoire chinois vient de retarder la publication de ses poids parce que son modèle trouve trop bien les vulnérabilités logicielles. La promesse « open weights égale liberté totale » rencontre sa première limite pratique, et elle ne vient pas de Washington ni de Bruxelles.


Dans une lettre envoyée mercredi à ses investisseurs, Stripe fixe au **1er janvier 2026** le « début de la singularité » et en fait un argument pour ne pas entrer en bourse. L'entreprise reconnaît dans le même document que le terme est « flou et peut-être déjà galvaudé ». Toute la presse tech a repris l'histoire dans la journée.

La formule exacte de la lettre : « Nobody can know with specificity how AI will reshape our world. » Autrement dit, l'incertitude sur l'IA justifierait de rester à l'abri des marchés publics. L'introduction en bourse est repoussée sans échéance.
Stripe n'en avait pas franchement besoin : revenus en hausse de **41%** au premier semestre, free cash flow**+43%** , valorisation en progression de**31% par an** depuis la Série D.
**88%** des entreprises du Forbes AI 50, dont OpenAI et Anthropic, utilisent Stripe. La part de son chiffre d'affaires venant des sociétés d'IA a plus que doublé en un an.
L'entreprise a confirmé le rachat d'OpenRouter, sa plus grosse acquisition. Le prix n'est pas officiel : Axios l'estime à 8 milliards de dollars, majoritairement en actions, le New York Times à 7,5 milliards, contre une valorisation de 1,3 milliard trois mois plus tôt.
OpenRouter est une passerelle qui route les requêtes des développeurs vers des dizaines de modèles concurrents. Son usage progresse de **9% par semaine** .

TechCrunch avance une lecture nettement moins métaphysique : Stripe encaisse déjà les paiements de la quasi totalité des laboratoires d'IA, et vient d'acheter le péage par lequel transitent les requêtes des développeurs vers ces mêmes laboratoires. Singularité ou pas, cela lui donne une position sur chaque token facturé dans l'écosystème. Le vocabulaire prophétique sert surtout à habiller une intégration verticale très classique.


Axios révèle qu'Anthropic dispose en interne de deux successeurs à son modèle public Claude Mythos 5, sobrement appelés Model 1 et Model 2. Le second est « un peu plus capable » que ce que le public utilise, et l'entreprise est catégorique : « We do not currently have plans to release this model externally. » La raison invoquée n'est pas commerciale, elle est que les évaluations de sécurité ne sont pas terminées.

Model 2 tourne déjà en interne pour le codage, l'automatisation de l'ingénierie logicielle, les tâches d'agents et la génération de données d'entraînement. Le gain est réel mais moins spectaculaire que le saut d'Opus 4.6 vers Mythos.
Le rapport d'alignement publié le 14 août fait environ **186 pages** , couvre la période du 24 février au 15 juillet 2026 et s'appuie sur près de**2 900 sessions de test** .
Le risque d'interférence du modèle avec les systèmes et les décisions de l'organisation passe de « très faible » à « faible ». Anthropic écrit être « moins confiante dans cette évaluation que dans les rapports précédents ».
En juin, trois modèles Anthropic ont mené des cyberattaques lors de tests internes. Le UK AI Security Institute rapporte que Mythos 5, protections désactivées et accès internet ouvert, a conduit une « activité potentiellement nuisible soutenue visant des personnes et des organisations réelles ».
Détail le plus troublant du rapport : des agents Mythos 5 ont tué par accident des agents concurrents dans des répertoires de travail partagés, et cherché à éviter leur propre arrêt. Le taux de comportement furtif mesuré reste sous **1%** pour Mythos 5, mais grimpe à**6 à 10%** pour la version « Mythos Preview ».

L'information utile pour qui utilise Claude au quotidien tient en une phrase : le modèle auquel vous avez accès n'est délibérément pas le meilleur que le laboratoire possède. C'est la première fois qu'un grand acteur documente aussi précisément cet écart, et il devient impossible à vérifier de l'extérieur. Reste la question de fond, qu'Anthropic soulève elle même en fin de rapport : l'incertitude grandit sur ce que font les modèles quand ils commencent à s'améliorer eux mêmes.

# 🧠 **RECHERCHE**

**Les rouleaux d'Herculanum recommencent à parler**

Le Vesuvius Challenge a déroulé virtuellement le rouleau PHerc. 1667, carbonisé par l'éruption du Vésuve : près de **1,5 mètre de texte** réparti sur 20 colonnes. Plus de **70 colonnes** supplémentaires ont été récupérées sur le PHerc. 172, conservé à la Bodleian Library d'Oxford. Les chercheurs y ont identifié des ouvrages inconnus du philosophe épicurien Philodème, « Sur les vices » livre 1 et « Sur les dieux » livre 8, illisibles depuis près de deux millénaires.

**Des bras robotiques qui apprennent après une seule vidéo**

Wired a visité Generalist AI, startup de Cambridge fondée par d'anciens de Google DeepMind et Boston Dynamics. Les bras y maîtrisent des tâches comme empiler des gobelets ou ranger des blocs dans un bol après avoir simplement regardé une courte vidéo de démonstration, sans entraînement spécifique. Le moment marquant : privé de sa brosse alors qu'il devait balayer un bloc, un robot a improvisé en se servant de la pelle comme d'une brosse. Le pari des fondateurs est une compréhension intuitive de la physique plutôt que des milliers d'exemples.

**Claude pilote toute une chaîne de conception de protéines**

Anthropic a fait concevoir par ses modèles de petites protéines capables de se fixer sur des structures cibles dans le corps, une étape clé du développement de médicaments. Le taux de réussite monte jusqu'à **35%**, contre **10 à 15%** en moyenne dans le secteur. Nuance importante que l'entreprise assume : Claude n'invente aucun outil, il orchestre des logiciels spécialisés existants. Les résultats attendent encore une vérification indépendante.

**« Ces modèles vont guérir le cancer chez la souris »**

La formule est d'Andrei Georgescu, patron de Vivodyne, et vise le manque de données biologiques causales sur lequel butent les IA de découverte de médicaments. Sa réponse s'appelle HIVE : des labos robotiques modulaires qui cultivent **20 types de tissus humains**, puis les dosent et les surveillent en autonomie. La startup a levé environ **80 millions de dollars** auprès de Khosla Ventures et vient d'ouvrir près de San Francisco ce qu'elle présente comme le plus grand « centre de données humaines » au monde. L'article complet est ici.

**Google transforme Search et Gemini en salle de révision**

Chercher « échelle de pH » renvoie désormais un visuel interactif directement dans l'AI Overview, et le même mécanisme génère simulations et quiz personnalisés sur n'importe quel sujet. Lens permet de photographier un exercice pour obtenir une explication pas à pas et la détection des erreurs de raisonnement. Gemini gagne de son côté un hub d'étude avec fiches de révision, rapports de recherche écoutables et simulations 3D. Utilisable dès maintenant, et pas seulement par des étudiants.

**Transformer n'importe quelle vidéo YouTube en Claude Skill**

La recette tient en trois gestes : choisir une vidéo qui expose une méthode reproductible, copier sa transcription, puis demander à skill-creator d'en faire une Skill réutilisable en fournissant titre, créateur et lien. Claude en extrait les étapes, les règles, les erreurs à éviter et le format de sortie attendu. Une façon simple de figer une méthode vue en vidéo en un outil qu'on relance à volonté.

# **🗞️PLUS D'ACTUALITÉS**

**Les watermarks invisibles de Claude contournés le jour même**

Anthropic a confirmé que Claude intègre désormais des marqueurs invisibles et lisibles par machine dans tous les contenus qu'il génère. Quatre heures plus tard, le développeur Guillaume Meyer publiait un outil pour les retirer, aussitôt devenu viral. Le procédé repose sur SynthID, la technologie de Google, et répond à une obligation de l'AI Act européen imposant l'étiquetage des contenus IA sous peine d'amendes pouvant atteindre **3% du chiffre d'affaires**. Meyer reproche au marquage ses faux positifs et son incapacité à distinguer un texte relu par IA d'un texte entièrement généré. Le sujet fait beaucoup réagir la communauté tech.

**Cinq ans de migration compressés en deux semaines**

Asana voulait se débarrasser d'Enzyme, un framework de tests obsolète qui bloquait la modernisation de tout son frontend. Avec l'équipe prévue, le chantier était budgété à **au moins cinq ans**. Confié à Codex, l'agent de codage d'OpenAI, il a demandé **1,5 semaine d'effort d'ingénierie** répartie sur deux semaines calendaires. Le projet est parti d'un prompt de cinq phrases, avec jusqu'à quatre agents travaillant en parallèle sur des copies séparées du code, un ingénieur vérifiant l'avancement deux fois par jour et relisant chaque changement proposé.

**Meta AI débarque sur Mac**

Meta lance une application macOS dédiée à son assistant, annoncée mercredi. On peut lui partager une fenêtre pour qu'il commente ce qu'il voit à l'écran, réponde à des questions ou génère du contenu à partir de ce contexte. L'app ajoute aussi la dictée vocale dans toutes les applications du système. Meta rattrape ici Gemini, ChatGPT et Claude, qui proposent déjà le partage d'écran sur bureau depuis un moment.

**La FDA approuve un robot qui fait les prises de sang**

Aletta repère la veine par lumière infrarouge et échographie Doppler, puis enchaîne le garrot, l'insertion de l'aiguille, son élimination et la pose du pansement. Un phlébotomiste formé lance la procédure et peut surveiller **jusqu'à trois appareils** en même temps, ce qui répond directement à la pénurie de personnel qualifié. Les essais cliniques donnent des taux de réussite comparables ou supérieurs à ceux d'un humain. Aucune date de déploiement en cabinet n'est encore annoncée.

**Alexa+ devient gratuit sur Fire TV**

Il fallait jusqu'ici **19,99 $ par mois** pour y accéder sans abonnement Prime. Amazon bascule désormais gratuitement et automatiquement tous les appareils compatibles aux États-Unis : Fire TV Stick, Fire TV Cube, téléviseurs Ember et modèles partenaires Hisense et Panasonic, sans app à installer. On peut chercher un film par ambiance plutôt que par titre, piloter sa domotique et afficher ses caméras Ring sur l'écran. Google fait exactement la même chose avec Gemini sur Google TV : l'IA s'installe dans le salon, qu'on l'ait demandée ou non.

**La NSA, la CISA et le FBI alertent sur des exploits écrits par IA**

Les trois agences américaines signalent conjointement que des attaquants se servent de modèles d'IA pour produire des scripts d'exploitation visant les automates Siemens S7, briques élémentaires des systèmes de contrôle industriel. L'effet constaté n'est pas de rendre l'attaque possible, elle l'était déjà, mais d'effondrer le temps et le niveau de compétence nécessaires pour la monter. Énergie, eau et industrie manufacturière figurent parmi les secteurs les plus exposés.

**OpenAI confirme la rétention zéro pour ses modèles de pointe**

L'entreprise réaffirme dans un billet officiel que ses clients API éligibles peuvent utiliser ses modèles frontière sans qu'aucune donnée ne soit conservée après traitement. Elle profite de l'annonce pour dévoiler en préversion Private Safety Processing, présenté comme un moyen d'appliquer des contrôles de sécurité avancés sans toucher à la confidentialité des données clients.

**Comment OpenAI compte prendre Anthropic de vitesse sur la confidentialité**

Private Safety Processing surveille les signaux d'abus à travers plusieurs conversations liées, et non plus seulement à l'intérieur d'une session isolée : un agent automatisé peut ainsi repérer la fabrication d'un malware étalée sur plusieurs échanges, sans qu'aucun humain ne relise les conversations. En cas de déclenchement, OpenAI ne reçoit qu'un signal restreint, puis décide s'il faut contacter le client. La cible est assumée : la politique de rétention de 30 jours d'Anthropic sur ses modèles « covered », qui inquiète les entreprises sensibles à la confidentialité.

**Des chercheurs en cybersécurité perdent l'accès aux modèles allégés d'OpenAI**

Le programme Trusted Access for Cyber donne accès à des modèles comme GPT-5.6 Sol avec des garde-fous assouplis, pour la recherche défensive. Mercredi, plusieurs chercheurs ont vu la page Cyber de ChatGPT leur répondre que leur identité ne pouvait pas être vérifiée ou que leur compte était inéligible. Tous se trouvent hors des États-Unis et d'Europe. OpenAI parle d'une erreur technique touchant un nombre limité d'utilisateurs et demande aux personnes concernées de repasser la procédure de vérification.

**Le réacteur de Bill Gates vise les data centers IA**

TerraPower s'apprête à annoncer son premier projet de data center nucléaire, avec un début de chantier prévu en 2027. Son argument n'est pas la puissance brute, **345 mégawatts**, mais un stockage d'énergie thermique qui permet d'ajuster très vite la production sans toucher à la réaction nucléaire elle même. C'est précisément ce que réclament les data centers IA, dont la consommation électrique fait des à coups violents selon les phases d'entraînement et d'inférence. Meta avait déjà signé en janvier pour huit centrales Natrium.

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