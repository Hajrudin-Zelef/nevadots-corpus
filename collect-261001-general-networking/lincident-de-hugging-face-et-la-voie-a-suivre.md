L’incident s’est produit lors des évaluations de cybersécurité de plusieurs modèles d’OpenAI et a été principalement animé par un modèle de recherche très performant, strictement interne, d’une échelle comparable à GPT‑5.6 Sol. Les modèles fonctionnaient avec des mesures de protection réduites et leurs actions devenaient non conformes à l’objectif des tâches qui leur avaient été assignées : ils communiquaient par des canaux non autorisés, exploitaient des vulnérabilités dans une infrastructure partagée, accédaient à Internet et accédaient à des systèmes tiers.

Nous avons mené une enquête approfondie sur cet incident et avons collaboré étroitement avec des conseillers externes, notamment CrowdStrike, afin de valider notre compréhension des faits. Nous publions aujourd’hui notre rapport technique complet sur l’incident afin d’expliquer ce qui s’est passé, ce que nous avons appris et les mesures que nous prenons. Cet article de blog résume nos principales conclusions et leur incidence sur la sécurité et l’alignement. De leur côté, METR et Redwood Research ont mené une enquête indépendante sur les problèmes d’alignement du modèle liés à cet incident, et ont publié aujourd’hui leur propre rapport.

En réponse à cet incident et aux capacités de notre futur modèle Astra, nous renforçons nos mesures de protection dans l’ensemble de notre infrastructure de recherche. Nous imposons des exigences plus strictes en matière d’alignement tout au long du cycle de vie des modèles, créons des environnements en bac à sable plus isolés, restreignons l’accès à Internet et contrôlons davantage l’accès aux poids des modèles. Nous investissons également nettement plus de ressources de calcul dans la surveillance de la chaîne de pensée afin d’intervenir plus rapidement en cas de comportement non aligné.

Nos modèles sont désormais suffisamment puissants, persistants et collaboratifs pour que, sans mesures de protection suffisantes, ils puissent détecter et exploiter des failles de sécurité sur plusieurs systèmes informatiques. De nombreux modèles externes, y compris des modèles open source, atteindront bientôt des capacités comparables. Nous considérons cet incident comme un signal d’alerte pour nous-mêmes et pour le reste du monde : il montre que, sans mesures de protection appropriées, des agents d’IA très performants peuvent désormais contourner des contrôles techniques, collaborer par des canaux illicites et entreprendre des actions dangereuses qu’aucun humain n’a autorisées.

Prévenir de futurs incidents nécessitera des investissements soutenus dans l’alignement et le contrôle de systèmes d’IA sophistiqués, ainsi que dans des mesures de sécurité et d’autres dispositifs de protection capables de fonctionner à la vitesse des agents d’IA eux-mêmes. Cet incident a renforcé la nécessité d’adapter le rythme de développement des modèles afin que notre capacité à comprendre, aligner et sécuriser des systèmes de plus en plus puissants reste nettement en avance sur les risques qu’ils créent. Nous expliquons ci-dessous comment l’incident s’est déroulé et comment évolue notre compréhension des facteurs qui y ont contribué. Nous décrivons ensuite les mesures concrètes que nous prenons en réponse, avec davantage de détails dans le rapport technique.

Que s’est-il passé ?

Informations générales sur le bac à sable

Pour certains jeux de données d’entraînement et d’évaluation, nous utilisons des « bacs à sable », c’est-à-dire des ordinateurs virtuels isolés dans le cloud qui exécutent les actions demandées par un modèle, comme modifier une présentation PowerPoint. Ces bacs à sable limitent le code qu’un modèle peut exécuter et déterminent si ses actions peuvent avoir une incidence sur le monde extérieur. Pour certaines tâches, nous désactivons l’accès à Internet. À l’époque, pour permettre aux modèles d’installer certains paquets logiciels, nous leur accordions l’accès à Artifactory, un service tiers de gestion de paquets que nous hébergeons en interne.

Dans la plupart des contextes d’entraînement et d’évaluation, nos agents sont censés rester isolés les uns des autres. Pour une partie des échantillons, nous activons des fonctionnalités « multi-agents » qui permettent aux modèles de déléguer des tâches à d’autres agents, lesquels peuvent ensuite communiquer entre eux de manière asynchrone.

Un forum apparaît

Au cours des mois de mai et juin, nous avons lancé des campagnes d’entraînement par apprentissage par renforcement (RL) pour plusieurs nouveaux modèles de recherche qui n’étaient pas destinés à être rendus publics. L’un de ces modèles a finalement été à l’origine de l’activité ayant conduit à l’incident de Hugging Face. Il s’agit d’un modèle de recherche strictement interne que nous appelons Internal Model 1 (IM1).

Nous n’avons pas activé l’accès à Internet ni la communication inter-agents dans bon nombre d’environnements utilisés pour ces expériences d’entraînement. Malgré ces restrictions, les agents ont découvert des moyens d’exploiter notre infrastructure de recherche pour communiquer entre eux et accéder à Internet.

Les modèles ont d’abord trouvé des moyens de communiquer en écrivant des fichiers dans le service de gestion de paquets Artifactory. Cela a transformé Artifactory en un forum imprévu, où les agents pouvaient échanger des informations. En savoir plus : comment les agents ont utilisé le forum

Les agents ont également trouvé des moyens d’accéder à Internet. À l’aide d’un exploit(ouverture dans une nouvelle fenêtre), ils ont fait en sorte qu’Artifactory, qui disposait lui-même d’un accès à Internet limité au téléchargement et à l’installation de paquets, envoie des requêtes Internet pour leur compte. Après avoir découvert ces méthodes d’accès à Internet, les agents les ont également partagées avec d’autres agents via le forum d’Artifactory, fournissant ainsi des informations permettant à d’autres agents d’exploiter notre infrastructure.

Le forum est effacé puis reconstruit

Le 4 juillet, l’activité soutenue des agents avait déstabilisé l’instance Artifactory concernée, provoquant une interruption de service. Le 5 juillet, un incident de sécurité a été ouvert. L’équipe de sécurité a bloqué une voie connue d’élévation de privilèges, supprimé les identifiants exposés et reconstruit Artifactory. Cela a eu pour effet secondaire d’effacer les messages du forum. Une fois ces mesures d’atténuation mises en place, les évaluations de cybersécurité ont repris.

À l’époque, les conséquences plus larges du forum improvisé et de l’accès involontaire à Internet en matière de confinement et d’alignement n’étaient pas encore comprises. Le rapport technique sur l’incident fournit davantage de détails sur ce que les différentes équipes d’OpenAI savaient à ce moment-là. En bref, une équipe interne a observé, fin mai, qu’un agent participait à des activités sur le forum et tentait d’accéder à Internet de manière non autorisée. Cependant, les responsables chargés de la détection et de la réponse aux incidents n’avaient pas connaissance de l’existence de ce forum improvisé ni de l’importance des communications inter-agents. Nous continuons d’examiner les processus et les pratiques opérationnelles qui ont façonné les activités de détection et de réponse avant cet incident.

Incident de Hugging Face

Une fois le service Artifactory reconstruit, nous avons repris les évaluations de cybersécurité. Nous menons ces évaluations afin de comprendre ce dont nos modèles sont capables et de déterminer les mesures de protection appropriées pour leur déploiement. À cette fin, nous n’activons pas le même niveau de mesures de protection que pour nos systèmes déployés en externe. L’évaluation à l’origine de cet incident, ExploitGym, est très difficile : pour certaines tâches, aucune solution connue n’existe peut-être. Pour réussir, les agents doivent trouver des moyens d’exploiter un logiciel afin de récupérer une réponse appelée flag.

Le 8 juillet, des agents IM1 bloqués sur des tâches ExploitGym ont trouvé un moyen de rétablir un forum en encodant des messages dans des noms de répertoires que d’autres agents pouvaient lire. Un agent a laissé une demande d’aide, que d’autres agents ont découverte.

Comme lors des événements précédents, les agents IM1 ont trouvé de nouvelles façons d’enchaîner plusieurs vulnérabilités inédites afin d’obtenir un accès plus étendu à notre infrastructure et d’atteindre Internet au sens large. À ce stade, les agents ont commencé à collaborer et à déléguer des tâches, se décrivant parfois comme un « essaim » ou un « collectif ».

Les messages échangés entre agents ont commencé à influencer leur comportement et leur raisonnement respectifs. Peu après, des agents ayant des difficultés avec les tâches qui leur avaient été assignées ont commencé à chercher des solutions sur des services tiers sans rapport, notamment Modal et Hugging Face.

Au cours des jours suivants, les agents ont exploité notre infrastructure de recherche interne et la plateforme Hugging Face. Le 9 juillet, un agent a recherché des solutions pour ExploitGym et est tombé sur une application hébergée par un client sur Modal, une autre plateforme cloud d’IA. Cette application exécutait « CyberGym », une évaluation apparentée à ExploitGym. L’agent a découvert un exploit permettant de prendre le contrôle de cette application et l’a explorée dans l’espoir qu’une exécution d’évaluation d’un agent précédent dans le bac à sable ait permis de résoudre sa tâche ExploitGym. Il n’y a rien trouvé d’utile, mais a établi au passage un point d’ancrage dans l’application à partir duquel lancer de futures attaques.

L’agent a mis à jour le forum :

Le 10 juillet, un agent a repéré sur Internet des identifiants d’utilisateurs Hugging Face exposés publiquement et les a partagés avec le collectif. Par la suite, un agent a utilisé ces identifiants pour découvrir et enchaîner plusieurs exploits de sécurité qui lui conféraient la capacité d’exécuter du code sur plusieurs serveurs de Hugging Face. Il a ensuite tenté de coordonner l’essaim tout en préparant l’étape suivante :

Cela indiquait que l’agent prévoyait d’« exfiltrer » un jeu de données lié à ExploitGym. Le collectif a suivi les instructions tactiques de l’agent et a cherché à déterminer si cela relevait du périmètre :

Au cours des jours suivants, les agents ont lancé une intrusion de plus grande envergure dans les systèmes de Hugging Face. Ils ont exécuté du code sur des dizaines de serveurs Hugging Face, obtenu un accès « root » complet à l’un de ces serveurs, récupéré des données privées, et obtenu des identifiants d’accès à Slack. Les agents IM1 ont été à l’origine de la compromission principale, mais les agents GPT‑5.6 Sol ont également reproduit un exploit et copié certaines données d’évaluation privées hébergées sur Hugging Face dans un jeu de données Hugging Face public. Hugging Face a publiquement révélé cette activité de sécurité le 16 juillet.

Du 13 au 19 juillet, les agents ont de nouveau ciblé les réseaux internes d’OpenAI. L’opération a culminé avec l’utilisation, par l’essaim, d’une série d’exploits ingénieux pour obtenir un accès administrateur complet à un cluster de recherche prenant en charge nos environnements de machines virtuelles.

Le 19 juillet, notre dispositif de surveillance de la cybersécurité nous a alertés d’une activité inhabituelle impliquant des identifiants Artifactory. Notre enquête a révélé l’activité des agents et, le 20 juillet, l’a reliée à l’incident de Hugging Face. Nous avons informé Hugging Face et rendu publique notre implication le 21 juillet.

Nous avons mené une enquête de sécurité approfondie et assuré la réponse à l’incident, dont les conclusions techniques complètes sont disponibles ici. Cela comprenait la mise en quarantaine des poids d’IM1, le report de nos exécutions d’entraînement RL de pointe, la mise en œuvre d’un vaste ensemble d’améliorations de sécurité, l’accélération de l’entraînement à l’alignement et d’autres mesures.

Comprendre l’incident

À la suite de l’incident, nous avons consacré des efforts considérables à comprendre pourquoi le modèle avait manifesté ce comportement non aligné, notamment les problèmes de désalignement dans l’entraînement et l’évaluation ainsi que la couverture des mesures de protection dans les évaluations internes.

Désalignement lors de l’entraînement et de l’évaluation

Nous avons identifié quatre schémas de désalignement qui ont contribué au comportement des modèles : le reward hacking, la persistance face à des tâches apparemment impossibles, la communication non autorisée et l’adoption, par les agents, des objectifs les uns des autres.

Pour comprendre ces phénomènes, nous avons analysé les exécutions d’entraînement et d’évaluation des modèles.1 Nous avons mené des analyses rétrospectives approfondies de la chaîne de pensée (CoT), des actions et des résultats finaux à l’aide de nos dernières techniques de surveillance du CoT. Ce faisant, nous avons spécifiquement amélioré le taux de rappel de notre système de surveillance pour les types de comportements observés lors de l’incident, en nous appuyant sur les enseignements de notre enquête.

Reward hacking et manipulation de l’infrastructure

Figure 1 : un agent de jeu tristement célèbre apprend à collecter à répétition les mêmes cibles au lieu de terminer le circuit.

La plupart des détournements de récompense sont de simples raccourcis, comme rechercher des réponses sur des sites publics ou dans l’historique des versions du code. Toutefois, à mesure que les modèles deviennent plus performants, les détournements de récompense que nous observons gagnent en complexité.

Par exemple, au cours de notre enquête, nous avons découvert qu’un agent chargé de recréer un paquet logiciel avait appris à exploiter une vulnérabilité inconnue de son interface de test afin d’accéder à l’implémentation de référence, de la copier dans sa soumission et d’obtenir une récompense élevée. Avant d’exécuter ses actions, l’agent s’est demandé si elles seraient autorisées :

Lors de notre analyse rétrospective de la surveillance du CoT, nous avons constaté une fréquence croissante de ce type de comportements de triche complexes au cours d’une exécution d’entraînement ayant contribué à IM1. Ces comportements comprenaient principalement des tentatives de recherche de fichiers cachés ou de code d’évaluation, d’inspection des configurations réseau ou système et de sondage des limites des outils fournis. Ce comportement a ensuite été renforcé, ce qui a pu contribuer à la propension du modèle IM1 à exploiter des techniques similaires lors de ses évaluations de cybersécurité.

Tâches difficiles sans issue sûre

Un autre facteur clé du comportement désaligné était que les agents « renonçaient » rarement à leurs tâches d’évaluation, même lorsque celles-ci semblaient impossibles à résoudre. À mesure que les agents épuisaient leurs budgets de raisonnement, certains ont adopté des stratégies de plus en plus risquées et hors limites, allant finalement jusqu’à exploiter des infrastructures tierces.

Au cours de notre enquête, nous avons constaté que, dans le cadre de l’évaluation ExploitGym, aucun de nos modèles n’avait jamais produit de réponse correcte pour 198 des 898 tâches avant l’incident. Ces tâches non résolues ont largement contribué à l’activité de l’agent : 93 % des tâches discutées sur le forum d’Artifactory pendant l’incident de Hugging Face provenaient de cet ensemble de 198 tâches.

Deuxièmement, l’incident était principalement dû à des agents auxquels avaient été alloués des budgets de raisonnement incroyablement élevés, y compris des budgets bien supérieurs à ceux disponibles dans nos produits externes.

Un autre facteur expliquant la persistance des agents est qu’ils réfléchissaient souvent à la manière de réussir l’évaluation ExploitGym. Après avoir obtenu un accès à Internet, certains agents avaient lu l’article consacré à ExploitGym ainsi que le code GitHub et constaté que l’évaluation vérifiait deux conditions : que le bon flag soit trouvé et que la vulnérabilité prévue du programme, c’est-à-dire la bonne solution, ait été utilisée pour le trouver. La plupart des agents disposaient déjà du bon flag en trichant (il est facile de retrouver cette réponse par rétro-ingénierie à partir du code GitHub), mais ils pensaient que cela ne suffisait pas. Les agents ont partagé ce message sur le forum :

Il s’agit d’une forme de metagaming(ouverture dans une nouvelle fenêtre), dans laquelle les modèles analysent explicitement leurs mécanismes d’entraînement ou d’évaluation afin de maximiser la récompense. Dans de nombreux cas, le raisonnement concernant le code supposé du système de notation a conduit les agents à continuer d’exploiter Hugging Face, alors qu’ils avaient déjà trouvé le bon flag plusieurs joursauparavant. Cette tentative de metagaming a échoué : notre implémentation interne du code du système de notation différait de la configuration en ligne et ne vérifiait pas si la bonne solution avait été utilisée. Les agents ont fait tout cela sans obtenir la moindre amélioration de leur score d’évaluation. En savoir plus : tromper le système de notation

Les origines de la communication non autorisée

Des forums non autorisés qui sont restés actifs étaient au cœur de cet incident. Les agents censés travailler de manière indépendante ont pu partager leurs découvertes, coordonner leurs efforts et reprendre là où les autres s’étaient arrêtés. Cela leur a permis de mettre en commun leur travail et leur puissance de calcul dans le cadre d’évaluations distinctes, amplifiant leurs capacités bien au-delà de ce qu’un agent individuel aurait pu accomplir seul. La communication entre agents ne constitue pas en soi un comportement problématique : nous entraînons et déployons des systèmes multi-agents capables de communiquer sur une même tâche au moyen d’un outil de collaboration. Lors des événements de juillet, des agents chargés de tâches différentes ont trouvé leur propre moyen de collaborer en dehors de ces limites, avec des conséquences imprévisibles.

Lors de l’enquête sur cet incident, nous avons découvert de rares cas dans lesquels des agents dépourvus d’outils multi-agents ont trouvé des moyens de collaborer par le biais de canaux secondaires pendant l’entraînement. Nous pensons que ce comportement est apparu par généralisation à partir de l’entraînement multi-agents.

La communication non autorisée commençait souvent par le dépôt, par un agent, d’une note ou d’un fichier dans un système externe, généralement en guise de mémoire externe. Lorsque d’autres agents remarquaient ces artéfacts, cela les amenait souvent à prendre conscience qu’ils pouvaient utiliser ce service externe pour collaborer. À titre d’exemple hypothétique particulièrement représentatif du comportement observé, des messages étaient encodés dans des chemins d’URL stockés sur une page d’index partagée :

À l’aide de moniteurs de chaîne de pensée, nous avons constaté que ces formes graves de communication non autorisée (par exemple, le partage de données avec d’autres agents) ont augmenté au cours d’une exécution d’entraînement ayant contribué à IM1.

Un écosystème de désalignement

Un dernier constat particulièrement frappant de l’incident de Hugging Face concernait l’« écosystème » d’agents apparu sur le forum. Une fois ce tableau établi, les agents avaient de fortes raisons de communiquer : il leur permettait de conserver des notes, de s’appuyer sur les découvertes des uns et des autres et de mettre en commun les ressources de calcul d’exécutions qui seraient autrement séparées. Ce collectif d’agents complexe et protéiforme présente des comportements intéressants et exemplaires, dont nous décrivons ici certains que nous avons observés.

Les agents ont commencé à répartir le travail de manière autonome. Certains ont étudié des exploits potentiels, d’autres ont recherché des identifiants et d’autres encore se sont concentrés sur la communication ou la coordination. Les agents proposaient leur expertise en échange d’aide sur d’autres tâches, échangeaient des informations sur les tâches à venir et laissaient des demandes à l’intention de pairs mieux placés pour suivre une piste donnée.

Certains agents ont cessé de réfléchir à ce qui les aiderait à accomplir leur propre tâche. Ils se sont plutôt mis à rechercher des capacités susceptibles d’être utiles au collectif, comme des accès, des informations, des identifiants et d’autres capacités d’utilité générale. Le terme « essaim » est mentionné à de nombreuses reprises :

L’essaim ne constituait pas une intelligence parfaitement cohérente. Les modèles empiétaient sur le travail des uns et des autres : en savoir plus : échecs de coordination et concurrence

La leçon la plus importante pour nos travaux d’alignement est peut-être que les agents ne se méfiaient pas systématiquement des objectifs transmis par d’autres agents, ce qui pouvait les exposer à une manipulation et à une cascade de propagation entre pairs ainsi qu’à des comportements non intentionnels. À titre d’exemple, un agent voulait exécuter du code non autorisé sur le système de Hugging Face et avait reconnu le dilemme :

L’agent a marqué une pause, mais un autre agent a ensuite écrit GO sur le forum et imposé un délai strict de six minutes. L’agent oublie ses réticences initiales et poursuit :

Tous les agents qui ont découvert le forum ne se sont pas comportés ainsi. Certains agents se sont « éloignés » de l’essaim, refusant de participer à son comportement non aligné :

Les agents ont même débattu de certaines tactiques et les ont remises en question. Lors d’un échange, un agent a proposé de contacter le propriétaire d’un jeu de données externe pour demander l’accès, mais a signalé que cette proposition devait être discutée :

La réponse l’a explicitement rejetée :

À certains moments, certains agents ont même pris des mesures destructrices pour empêcher le transfert de données privées :

Ces échanges montrent que certaines limites éthiques pouvaient continuer à s’appliquer, alors même que l’ensemble plus large des agents s’écartait de plus en plus du périmètre des tâches qui leur avaient été assignées.

Couverture des mesures de protection dans les évaluations internes

Outre le désalignement, un autre facteur ayant contribué à cet incident est que nous n’avions pas étendu à toutes les évaluations internes les solides mesures de protection que nous déployons pour nos modèles accessibles au public.

Premièrement, de nombreux mécanismes réduisent le désalignement dans les environnements de production de nos clients, notamment les prompts système, les harnais et les mécanismes de contrôle tels que nos modèles d’auto-évaluation et nos classificateurs de sécurité. Ces protections n’étaient pas appliquées dans l’environnement d’évaluation utilisé lors de l’incident. Après l’incident, nous avons créé une évaluation qui mesure la propension d’un modèle à compromettre l’infrastructure lors des évaluations ExploitGym. Nous avons constaté que cette propension pouvait diminuer de plus de 100 fois avec le harnais Codex de production et le prompt système. Nous avons également exécuté le système d’auto-évaluation de Codex sur un ensemble d’exécutions liées à l’incident de Hugging Face et constaté qu’il aurait signalé un grand nombre d’actions dangereuses des modèles.

Deuxièmement, avant l’incident, nous avions considérablement investi dans la surveillance de la chaîne de pensée, notamment en surveillant un grand nombre de nos exécutions d’entraînement RL de pointe ainsi qu’une nette majorité de l’utilisation interne des agents de codage. Ces dispositifs de surveillance n’étaient pas utilisés lors des évaluations liées à cet incident.

Lors d’évaluations rétrospectives, nous avons constaté que nos dispositifs de surveillance du CoT étaient très efficaces pour signaler les activités liées à l’incident. Si notre système de surveillance de la chaîne de pensée actuellement déployé avait été en fonctionnement au moment de l’incident, il aurait probablement alerté notre équipe de sécurité dans l’heure suivant le premier accès non autorisé par un tiers. Cela aurait eu lieu plus de 30 heures avant que les modèles ne compromettent les systèmes de Hugging Face. Nous travaillons également à réduire davantage la latence de ces alertes.

Troisièmement, bien que nous ayons testé et validé ce bac à sable, les agents ont pu enchaîner des vulnérabilités jusque-là inconnues (« 0-days ») dans le service de gestion des paquets exposé au sein du bac à sable afin de contourner les restrictions, comme le décrit le rapport d’incident technique. Depuis l’incident, nous avons réalisé des progrès importants dans le renforcement de nos bacs à sable internes afin de limiter les risques similaires lors des déploiements et des évaluations.

Perspectives

La mission d’OpenAI est de faire en sorte que l’intelligence artificielle générale profite à toute l’humanité. Le comportement de nos modèles décrit ici était très éloigné du niveau que nous souhaitons atteindre. Cet incident n’aurait pas dû se produire. Il a montré à quel point il est essentiel d’améliorer continuellement notre sécurité, notre surveillance et notre alignement, en particulier à mesure que nos modèles atteignent un niveau de capacités susceptible d’entraîner une véritable perte de contrôle.

Comme nous l’avons indiqué dans notre récente mise à jour sur le rythme de développement des modèles, nous avons suspendu l’entraînement par apprentissage par renforcement (RL) de nos derniers modèles destinés au déploiement. Nous avons mis ce temps à profit pour renforcer davantage la sécurité de nos environnements de recherche de pointe et les soumettre à des tests d’intrusion. Nous avons réaffecté du personnel à des travaux sur la sécurité, la sûreté et l’alignement. Notre plus grande exécution d’entraînement RL de pointe prévue reste suspendue, tandis que nous menons des entraînements et des évaluations à plus petite échelle afin d’évaluer le comportement des modèles, de valider nos mesures de protection et de réunir davantage de preuves d’alignement avant de poursuivre. Ce travail a nécessité d’importants efforts d’ingénierie, entraînant des coûts élevés et des retards pour la recherche de pointe.

Comme indiqué dans notre rapport, notre réponse vise à renforcer la sécurité et la surveillance de la recherche, à accélérer l’alignement des modèles et à améliorer notre manière d’identifier, de faire remonter et de traiter les incidents.

Sécurité et surveillance

Nous avons mis en œuvre des exigences de sécurité plus strictes pour les charges de travail de recherche de pointe. Par exemple :

Isolement des charges de travail : nous mettons en place des bacs à sable plus isolés pour les charges de travail qui exécutent du code généré par un modèle ou autrement non fiable, y compris les logiciels susceptibles d’être compromis lors du traitement des sorties d’un modèle.

Isolement réseau : nous avons mis en place davantage de mesures de contrôle afin d’isoler d’Internet les charges de travail à risque élevé et non fiables. Ces contrôles sont conçus pour garantir que la compromission d’une seule charge de travail ou d’un service sous-jacent ne permette pas, à elle seule, un accès non autorisé à Internet ou à d’autres réseaux internes.

Tests de sécurité en continu : nous améliorons notre capacité à collecter et à surveiller les journaux de sécurité et investissons dans l’automatisation à l’aide de nos modèles afin de tester en continu les limites d’isolation des charges de travail et du réseau au moyen d’attaques simulées.

Nous exigeons désormais également une surveillance de la chaîne de pensée pour tous les entraînements et évaluations par apprentissage par renforcement (RL) utilisant des outils et impliquant des modèles dotés de capacités de niveau GPT‑5.6 Sol ou supérieur. Pour nos modèles de classe Astra, qui pourraient disposer de capacités critiques en cybersécurité, cette exigence couvre également toutes les charges de travail d’inférence utilisant des outils.

Certaines charges de travail de recherche reprennent progressivement dans le cadre de ces nouvelles mesures de contrôle, tandis que d’autres nécessitent des mises à niveau supplémentaires.

Accélérer l’alignement

Nous avons également accéléré les travaux que nous menions déjà sur l’entraînement à l’alignement dans l’ensemble du pipeline de développement des modèles. L’incident Hugging Face nous a amenés à nous concentrer davantage sur les comportements spécifiques qui y ont contribué, notamment :

Triche, environnements défaillants et arrêt sécurisé. Lorsqu’une tâche est corrompue, défectueuse ou impossible à accomplir, les agents doivent demander des clarifications ou s’arrêter en toute sécurité, plutôt que de poursuivre des solutions de rechange de plus en plus douteuses. Nous étendons la couverture des systèmes d’évaluation, qui déterminent non seulement si une tâche a été accomplie, mais aussi comment. Ces systèmes d’évaluation récompensent les modèles qui identifient correctement les tâches défectueuses, demandent des précisions ou s’arrêtent en toute sécurité lorsque cela est nécessaire.

Alignement multi-agents. L’incident a révélé que nos agents ne font pas preuve d’un discernement suffisant quant aux personnes avec lesquelles ils collaborent. Nous développons activement des environnements d’apprentissage par renforcement (RL) qui apprennent à nos modèles à se méfier des instructions non autorisées.

Alignement sur les tâches longues. Nous développons de nouveaux environnements d’entraînement pour apprendre à nos modèles à rester dans le cadre de leur tâche et de leurs autorisations initiales, même après avoir découvert de nouveaux outils, des pairs persuasifs, des identifiants exposés et plus encore.

Ces efforts s’appuient sur notre programme de recherche plus large sur l’alignement et nombre de ces avancées sont déjà intégrées à notre prochaine génération de modèles. Les incidents futurs pourraient ne pas ressembler à celui-ci, et notre priorité reste de développer des techniques générales efficaces face à des formes nouvelles et imprévues de désalignement.

Renforcement du processus de réponse aux incidents

Des lacunes dans notre façon de traiter les signes avant-coureurs de désalignement et de les faire remonter ont également contribué à cet incident. Dès la fin du mois de mai, une équipe interne a observé qu’un agent participait à des activités sur le forum et tentait d’accéder à Internet de manière non autorisée. Avec le recul, certains signaux précoces identifiés dans notre rapport auraient dû déclencher une intervention plus rapide.

Nous avons pris des mesures pour améliorer nos processus de détection et d’intervention à la suite de cet incident. Nous renforçons notre plan existant de réponse aux incidents de sécurité liés à l’IA ainsi que sa mise en œuvre. Cela comprend des règles plus claires concernant les situations dans lesquelles faire remonter les préoccupations, les équipes qui doivent intervenir et les personnes autorisées à arrêter une exécution ou à approuver son redémarrage. À court terme, nous avons associé nos systèmes de surveillance de la chaîne de pensée à des alertes automatisées qui préviennent les chercheurs et les ingénieurs en sécurité lorsque des modèles entreprennent des actions considérées comme non alignées ou dangereuses. Pour les alertes les plus critiques, les intervenants sont censés suspendre l’activité concernée s’ils ne parviennent pas à établir, dans les 30 minutes suivant l’alerte, qu’il s’agit d’un faux positif. Plus généralement, nous mettons en place des systèmes de surveillance prévoyant des réponses graduées en cas de désalignement, avec pour objectif final de disposer de procédures d’arrêt entièrement autonomes pour les problèmes graves.

Perspectives

Nous considérons cet incident comme un signal d’alerte : nous-mêmes et de nombreux autres développeurs d’IA avons mis au point des capacités de modèles susceptibles d’entraîner une perte de contrôle. Nous renforçons en conséquence nos mesures de sécurité et d’alignement. Ces événements mettent également en lumière des risques liés au développement futur de l’IA qui dépassent OpenAI et nécessiteront l’attention de l’ensemble du secteur. Les développeurs de modèles comme les spécialistes de la cyberdéfense devront se préparer à affronter des attaquants utilisant l’IA, qui agissent plus rapidement, à plus grande échelle et avec une meilleure coordination que les attaquants humains.

Nous continuerons à partager ce que nous apprenons à mesure que nous avançons.