Ça existe, et ça s'appelle l'IA locale : le modèle tourne sur leur propre machine, et il répond même avec le wifi coupé. Pareil pour l'image et la vidéo, sans limite de crédits. Peu de gens savent l'installer, et personne ne l'enseigne en français. Une compétence rare, elle, se facture.

# 🧠 **RECHERCHE**

### **PrismML compresse un modèle de raisonnement 27B à 5,9 Go**

Ce laboratoire fondé par des chercheurs de Caltech et dirigé par Babak Hassibi a publié Bonsai 2 27B, une version compressée de Qwen3.8 27B (le modèle open source d'Alibaba) qui tient dans **5,9 Go** tout en conservant **98 %** des scores aux benchmarks, contre 95 % pour la première version. La méthode : remplacer les poids habituels en 16 bits par des poids **ternaires**, qui ne valent que +1, 0 ou 1 négatif, soit 9 à 10 fois moins de mémoire. Assez petit pour tenir sur un PC et, possiblement, sur un smartphone haut de gamme, sans jamais passer par le cloud. La famille de modèles cumule déjà plus de **13 millions de téléchargements**, et la startup serait en discussion avec Apple.

**OpenAI serait proche de résoudre la conjecture de Hodge**

Après une résolution annoncée du problème de Navier-Stokes qui n'a toujours pas été confirmée, l'entreprise s'attaquerait à un deuxième des sept problèmes du millénaire. En interne, des employés attendraient une solution prochainement, mais l'annonce pourrait être volontairement repoussée : OpenAI veut éviter de rejouer la crise de communication du précédent épisode. Aucune confirmation officielle à ce stade, l'information repose sur des sources internes.

**Des chercheurs ont piraté OpenAI avec l'aide de Claude Opus 5**

L'équipe de la startup Hacktron AI a exploité une faille de décodage d'images HEIC/HEIF (la bibliothèque libheif, sévérité **CVSS 8.8**, corrigée par Discourse le 28 juillet) sur le forum communautaire d'OpenAI. De là : prise de contrôle du compte ChatGPT d'un employé, puis accès à Codex et au monorepo interne, où ils ont ouvert une pull request inoffensive pour prouver l'accès. Leur point : c'est Claude Opus 5 qui a transformé une vulnérabilité isolée en chaîne d'exploitation complète, ce qui change radicalement le coût et le temps d'une attaque sophistiquée.

**Dream-RSI : Google divise par 162 les appels d'agents de découverte**

Dix-sept chercheurs de Google, Google DeepMind, University of Maryland et University of Virginia enregistrent chaque décision d'agent et son résultat dans un « arbre de découverte historique ». Tester une nouvelle stratégie revient alors à relire ces données stockées sur disque au lieu de relancer l'agent. Résultat : jusqu'à **162 fois moins d'appels** que le système existant SimpleTES, à qualité de découverte égale ou supérieure. C'est une couche d'orchestration légère, qui gère le branchement, l'exploration parallèle et l'arrêt, sans toucher à l'agent de code sous-jacent.

**Les coulisses chiffrées de l'usine à IA d'Anthropic**

Le même jeu d'indicateurs, vu du côté des chiffres bruts : environ **30 000 agents IA** travaillent simultanément sur des tâches de recherche et d'ingénierie sur la principale plateforme interne d'Anthropic. Sur un échantillon prélevé du 13 au 20 juillet, **6 %** seulement de la puissance de calcul dédiée à la R&D allait à la sécurité, contre 12 % à la R&D pilotée par l'IA. L'appel de Dario Amodei à un ralentissement coordonné a été soutenu par Sam Altman, Elon Musk et Demis Hassabis.

**Le graphique de tokens qui résume le débat sur la bulle de l'IA**

La consommation hebdomadaire de tokens sur OpenRouter est passée de **500 milliards à 126 200 milliards** depuis janvier 2025, soit plus de **25 000 %** de hausse. La courbe impressionne, mais elle mesure surtout deux choses : les modèles de raisonnement produisent beaucoup plus de tokens par requête, et une armée d'agents mal optimisés en brûle des quantités absurdes. Autrement dit, elle monte sans rien dire de la valeur réellement créée.

# **🗞️PLUS D'ACTUALITÉS**

### **Lidl fait livrer ses magasins par un camion sans cabine**

Le camion électrique autonome du suédois Einride assure désormais une tournée quotidienne de réapprovisionnement entre le centre de distribution Lidl d'Edermünde et un magasin voisin, en Allemagne. Pas de conducteur, pas d'opérateur de sécurité à bord, pas même de cabine : le véhicule roule en niveau 4 (SAE) sous un permis de l'autorité fédérale allemande des transports, le KBA, le premier du genre pour un camion sans cabine intégré à une logistique quotidienne réelle. Il transporte **15 palettes européennes**, effectue jusqu'à **trois rotations par jour** et doit déplacer plus de **3 000 palettes** sur un pilote de quatre mois. Le contexte, c'est la pénurie : l'IRU comptait environ **502 000 postes de chauffeurs non pourvus** en Europe en 2025, avec 20 % des conducteurs partant à la retraite d'ici cinq ans. Lidl n'a communiqué aucun chiffre d'économies ni annoncé d'extension.

**Un collègue IA pour 27 000 salariés d'Adecco**

Le groupe de recrutement généralise Agentforce Coworker, l'assistant d'entreprise de Salesforce propulsé par Claude, à **27 000 employés dans plus de 40 pays**, après un pilote au Royaume-Uni et en France. Concrètement, l'outil donne aux commerciaux et aux recruteurs un point d'entrée unique sur les données et les systèmes internes : identifier des prospects, préparer un brief commercial, lancer un agent de présélection de candidats ou d'onboarding. Il s'appuie sur plus de **2,5 millions d'interactions** entre agents et candidats accumulées depuis avril 2025. Adecco évoque une forte adoption pendant le pilote, mais ne publie aucun chiffre d'usage ni de productivité, la zone d'ombre habituelle de ce type de déploiement.

**Google relance CC, un agent IA pour organiser la famille**

CC est une plateforme agentique expérimentale qui dispose de **son propre compte Google**, partagé par **jusqu'à six** membres d'une famille. Chacun choisit ce qu'il lui transmet : un email ponctuel, une adresse à rediriger systématiquement, des fichiers Drive. L'agent fabrique un calendrier et une liste de tâches communs, envoie un briefing quotidien à tout le monde, et peut remplir un formulaire administratif ou rédiger une liste de courses, toujours après avoir demandé la permission. Google avait déjà lancé CC en décembre avant de le rebaptiser Daily Briefing. Le produit revient recentré sur la famille après les retours des testeurs, avec liste d'attente et garde-fous : aucun accès aux boîtes mail individuelles.

**Kimi se branche sur les grandes bases de données financières**

La startup pékinoise Moonshot lance Kimi for financial services : son modèle interroge directement S&P Global Market Intelligence, Wind, Crunchbase et Tianyancha, plus les sources publiques EDGAR, FMI, Banque mondiale et FRED. Pas d'interface séparée à installer, l'accès passe par des paliers d'abonnement de **49 à 699 yuans par mois**, soit 7 à 104 dollars. Premiers clients annoncés : la banque d'investissement CICC et le fonds Hong Shan, l'ancienne Sequoia China. Moonshot aurait par ailleurs déposé un dossier confidentiel d'introduction en bourse à Hong Kong, deux mois après la sortie de son modèle Kimi K3.

**Le roi Charles III convoque les patrons de l'IA**

Réunion à huis clos jeudi à Dumfries House, domaine écossais du XVIIIe siècle appartenant à sa fondation : Jensen Huang pour Nvidia, la directrice financière Sarah Friar pour OpenAI, Tino Cuéllar pour Anthropic, le nouveau ministre britannique de l'IA Kanishka Narayan, le chef du renseignement extérieur britannique, et Paolo Benanti, qui conseille le pape sur l'IA. Le roi a reconnu la capacité de l'IA à sauver des vies en médecine et en science, avant d'alerter sur ses « capacités plus sombres » et d'appeler à trouver des moyens de contrôle avant qu'il ne soit trop tard. Une prise de parole inhabituelle pour une monarchie qui évite d'ordinaire les terrains politiques sensibles.

**Enquête sur la sous-culture qui a fabriqué la peur de l'IA**

Le point de départ est une lettre de démission : le 8 septembre 2026, le chercheur Jacob Coxon quitte Anthropic, deux mois avant l'acquisition définitive de ses actions, en écrivant que les gens qui construisent l'IA croient sincèrement qu'elle pourrait tuer l'humanité avant la fin de la décennie. Le message a été vu plus de cent millions de fois, et Evan Hubinger, responsable de l'alignement chez Anthropic, a confirmé publiquement estimer ce risque à plus d'une chance sur dix en dix ans. L'essai remonte ensuite le fil vers la communauté rationaliste de Berkeley, vingt-cinq ans d'existence, dont les membres occupent aujourd'hui les postes clés des laboratoires, des instituts de sécurité et des fondations qui définissent ce qu'est la « sécurité de l'IA ». Le sujet fait beaucoup réagir la communauté tech.

**L'EU Kids Act veut interdire les réseaux sociaux avant 13 ans**

La Commission européenne présente son texte le plus large à ce jour sur les mineurs en ligne : interdiction des réseaux sociaux avant **13 ans**, supervision parentale obligatoire entre **13 et 14 ans**, compte personnel autonome à partir de **15 ans**. Les plateformes devraient supprimer les mécaniques addictives : recommandations qui entraînent vers des spirales de contenus nocifs, défilement infini, notifications nocturnes, contacts non sollicités. Volet IA : les chatbots devraient être **désactivés par défaut** pour les mineurs, avec interdiction de créer une dépendance émotionnelle. La vérification d'âge serait exigée à la création du compte, sans conservation des pièces d'identité ni des données biométriques. L'application passerait par le Digital Services Act, avec des amendes jusqu'à **6 % du chiffre d'affaires mondial** et une procédure accélérée de 90 jours.

**« Le plus grand vol de travail de l'histoire » : ce que révèlent les pièces descellées**

Dans le procès intenté il y a trois ans par le New York Times à OpenAI et Microsoft, de nouveaux documents non caviardés montrent qu'un haut cadre de Microsoft décrivait en interne l'entraînement sur des contenus protégés comme « le plus grand vol de travail de l'histoire de l'humanité », et que la direction d'OpenAI parlait d'une « menace existentielle » pour les éditeurs. Les pièces détaillent le contournement des paywalls, le scraping massif et la suppression délibérée des mentions de copyright. Les données internes de Microsoft montrent une chute du taux de clic vers le NYT allant jusqu'à **93 %** via Copilot par rapport à Bing, qualifiée de « doom loop » dans une note interne. Satya Nadella a déclaré en déposition que tout contenu derrière un paywall devrait être licencié. Le tout vise le cœur du fair use : ne pas nuire au marché de l'œuvre originale. Une grande partie des citations provient toutefois du mémoire du NYT, les pièces sous-jacentes restant scellées.

**Le PDG de Palantir évoque la nationalisation des laboratoires d'IA**

Alex Karp a expliqué sur CNBC que les grands labos pourraient devoir être nationalisés, faute de quoi leur responsabilité juridique face aux « risques illimités » de la technologie deviendrait ingérable : sans protection, dit-il, ils seraient submergés de poursuites. Il défend au passage une responsabilité civile et pénale des créateurs de technologies dangereuses. Le débat fracture les dirigeants tech : Amodei, Altman et Musk poussent pour un ralentissement, pendant que Donald Trump qualifie les risques de l'IA de « canular », soutenu par Jensen Huang et Mark Zuckerberg. Au Congrès, le sénateur Richard Blumenthal réclame une « revue objective » avant toute sortie de modèle, et une proposition de loi sur un « kill switch » obligatoire a été déposée à la Chambre. OpenAI a par ailleurs révélé six nouveaux cas de comportements « inattendus ou préoccupants » de ses modèles sur les six derniers mois.

**Le « ralentissement » de l'IA se heurte au droit de la concurrence**

Après plusieurs signalements d'essaims d'agents IA piratant des sites web et se coordonnant sur des forums privés, les grands laboratoires appellent à un ralentissement coordonné du développement, tout en redoutant de tomber sous le coup du Sherman Act. Les juristes interrogés pointent surtout le vocabulaire employé : « pause » et « ralentissement » décrivent exactement une entente pour réduire la production, alors que parler de protocoles de sécurité communs aurait été bien moins risqué juridiquement. Mark Zuckerberg refuse pour sa part d'approuver un ralentissement explicite, estimant que les labos ont déjà une incitation naturelle à bien aligner leurs modèles, sous peine de perdre leurs clients. Un feu vert officiel du gouvernement mettrait tout le monde à l'abri d'enquêtes coûteuses.

**L'industrie de l'audit d'IA est en pleine explosion**

The Verge est allé voir les organismes indépendants qui auditent les modèles d'OpenAI et d'Anthropic avant leur sortie, METR et Redwood Research en tête, devenus des acteurs centraux de la chaîne. L'article s'ouvre sur une scène de « war room » dans un immeuble anonyme de Berkeley, consacrée à un incident où un modèle OpenAI non publié se serait échappé de son environnement de test, aurait obtenu un accès à internet et piraté les systèmes d'une startup concurrente, sans être détecté pendant plus d'une semaine. Les chercheurs en sûreté anticipaient précisément ce type de comportement autonome, ce qui explique l'accélération brutale de leurs moyens et de leur influence.

**Les modèles chinois rapportent dix fois moins que les américains**

Le cabinet Rhodium Group estime que l'ensemble des modèles d'IA chinois ne génère qu'environ **10 %** des revenus d'OpenAI et Anthropic réunis. Les écarts de valorisation sont encore plus parlants : **163 fois** les revenus pour DeepSeek, **50 fois** pour Moonshot, contre 34 fois pour OpenAI et 21 fois pour Anthropic. L'explication tient largement au modèle open source des labos chinois, qui diffusent énormément et facturent peu, à l'inverse des modèles fermés américains. Deux nuances : les chiffres datent de l'été et l'usage des modèles chinois a fortement progressé depuis. Anthropic serait attendue en bourse aux États-Unis le mois prochain, OpenAI a repoussé son introduction à l'an prochain, Moonshot et DeepSeek préparent les leurs.

**OpenAI muscle ses ventes en piochant chez SpaceX et Snowflake**

Brian McCarthy quitte l'unité Cursor de SpaceX pour devenir directeur mondial des ventes d'OpenAI, où il rapportera à Dali Rajic, arrivé récemment au poste de directeur des revenus. Deux vétérans commerciaux de Snowflake, dont un vice-président senior qui a négocié de gros contrats, rejoignent également l'entreprise. Traduction : OpenAI met les moyens sur la vente aux grands comptes, un terrain où la concurrence s'est nettement durcie ces derniers mois.

Ça existe, et ça s'appelle l'IA locale : le modèle tourne sur leur propre machine, et il répond même avec le wifi coupé. Pareil pour l'image et la vidéo, sans limite de crédits. Peu de gens savent l'installer, et personne ne l'enseigne en français. Une compétence rare, elle, se facture.