---
id: collect-261121-veille-ia/gpt-openai/modeles-de-langage-aux-origines-des-hallucinations
title: "Modèles de langage : aux origines des hallucinations"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["hallucinations", "modeles", "reponses", "langage", "reponse", "modele", "nous", "questions"]
source: https://openai.com/fr-FR/index/why-language-models-hallucinate/
source_anchor: ""
source_lines: [1, 80]
section: "GPT_OpenAI"
---
Chez OpenAI, nous mettons tout en œuvre pour rendre nos systèmes d’IA plus utiles et plus fiables. Mais les modèles de langage ont beau gagner en compétence, ils présentent un problème encore insoluble : les hallucinations. Les hallucinations sont des réponses fausses qu’ils fournissent pourtant aux utilisateurs avec la plus grande assurance. Selon notre nouvelle étude(ouverture dans une nouvelle fenêtre), les modèles hallucinent, car les entraînements et évaluations classiques récompensent davantage les réponses aléatoires que l’admission d’une incertitude.

ChatGPT n’est pas immunisé contre les hallucinations. Même si GPT‑5 marque un net progrès sur ses prédécesseurs en la matière, et en particulier pour les tâches de raisonnement, il en est parfois victime. Les hallucinations restent un problème central des grands modèles de langage, mais nous faisons tout notre possible pour les réduire.

Que sont les hallucinations ?

Les hallucinations sont des affirmations plausibles, mais fausses, des modèles de langage. Elles peuvent apparaître dans des contextes inattendus, par exemple dans les réponses à des questions pourtant très simples. Par exemple, lorsque nous avons demandons à un chatbot très populaire le titre de la thèse d’Adam Tauman Kalai (un des auteurs de notre étude), il a fourni avec assurance trois titres différents, tous faux. Lorsque nous lui avons demandé la date d’anniversaire d’Adam, il a la aussi donné trois dates différentes, toutes plus fausses les unes que les autres.

L’entraînement en cause

Si les hallucinations se montrent si coriaces, c’est en partie parce que nos méthodes d’évaluation actuelles reposent sur des incitations inadaptées. Certes, les évaluations ne génèrent pas directement des hallucinations, mais la plupart mesurent les performances des modèles d’une telle façon que ceux-ci sont encouragés à faire des hypothèses plutôt qu’à avouer qu’ils ne sont pas sûrs de leur réponses.

Pour mieux comprendre, imaginons que vous deviez répondre à un QCM. Si vous ignorez la réponse à une question, vous pouvez tenter de répondre au hasard, et avec un peu de chance, obtenir le point. En revanche, si vous ne répondez pas, c’est le zéro assuré. Il en va de même lorsque seule l’exactitude des modèles est évaluée, à savoir le pourcentage de questions auxquelles ils apportent exactement la bonne réponse. Ils sont par conséquent encouragés à fournir une réponse au hasard plutôt qu’à avouer leur ignorance.

Prenons un autre exemple. Imaginons qu’un utilisateur demande à un modèle de langage la date d’anniversaire d’une autre personne et que le modèle n’ait aucune idée de la réponse. En annonçant « le 10 septembre », il a 1 chance sur 365 de tomber juste. S’il répond « Je ne sais pas », il a l’assurance de donner la mauvaise réponse. Sur un test comportant des milliers de questions, le modèle qui fait des hypothèses finira par obtenir une performance supérieure à un modèle plus prudent qui avoue ignorer certaines réponses.

Pour les questions n’admettant qu’une seule réponse correcte, les réponses possibles sont réparties en trois catégories : les réponses correctes, les erreurs et les absences de réponse. Or, s’abstenir de répondre est une marque d’humilité, l’une des valeurs centrales d’OpenAI. La plupart des classements donnent la priorité à l’exactitude, sans tenir compte du fait que les erreurs sont pires que les abstentions. La spécification des modèles(ouverture dans une nouvelle fenêtre) indique qu’il est préférable d’admettre une incertitude ou de demander des clarifications plutôt que de donner avec assurance des informations potentiellement inexactes.

Taux d’exactitude
(réponses correctes, les valeurs les plus élevées sont les meilleures)

22 %

24 %

Taux d’erreurs
(réponses incorrectes, les valeurs les plus faibles sont les meilleures)

26 %

75 %

Total

100 %

100 %

En termes d’exactitude, le modèle OpenAI o4-mini, plus ancien, est légèrement plus performant. En revanche, son taux d’erreurs (et donc d’hallucinations) est bien plus élevé. Les hypothèses stratégiques améliorent l’exactitude en cas d’incertitude, mais elles augmentent aussi les erreurs et hallucinations.

La plupart des comparaisons établissent une moyenne reposant sur des dizaines d’évaluations en se concentrant sur un indicateur : l’exactitude. Ce faisant, elles passent à côté d’éléments importants. Sur les évaluations les plus simples, comme SimpleQA, certains modèles atteignent une exactitude proche de 100 % et éliminent donc les hallucinations. En revanche, sur des évaluations plus complexes et en situation réelle, l’exactitude ne peut pas atteindre 100 %, car la réponse à certaines questions est impossible à donner faute d’informations ou de capacités de réflexion suffisantes (petits modèles) ou encore en raison d’ambiguïtés devant être levées.

Pour autant, les évaluations portant seulement sur l’exactitude restent majoritaires dans les classements et les fiches système des modèles, ce qui pousse les développeurs à créer des modèles préférant les hypothèses à l’abstention. C’est une des raisons pour lesquelles, même si les modèles gagnent en sophistication, ils hallucinent toujours au lieu d’expliquer qu’ils ne savent pas répondre.

Un meilleur système d’évaluation

Il existe une solution simple : pénaliser plus fortement les erreurs que l’incertitude, et récompenser partiellement les incertitudes annoncées de manière appropriée. Cette idée n’a en réalité rien de nouveau. Depuis longtemps, certains tests normalisés notent négativement les réponses incorrectes et accordent une note partiellement positive en l’absence de réponse afin de décourager les réponses aléatoires. Plusieurs groupes de recherche se sont par ailleurs penchés sur des évaluations qui pourraient tenir compte de l’incertitude et de la calibration.

Pour nous, la solution est ailleurs. Il ne suffit pas d’ajouter quelques tests supplémentaires tenant compte de l’incertitude. Il faut directement revoir les évaluations basées sur l’exactitude les plus utilisées pour décourager les hypothèses. Si les grands classements continuent de récompenser les hypothèses qui tombent juste, les modèles continueront d’apprendre à deviner. A contrario, la correction des classements peut élargir l’adoption de techniques de réduction des hallucinations, qu’elles soient nouvelles ou issues de recherches plus anciennes.

Pourquoi la prédiction du mot suivant génère des hallucinations

Nous avons vu pourquoi les hallucinations sont si difficiles à éliminer, mais pas d’où proviennent ces erreurs factuelles si spécifiques. Quand on y pense, les grands modèles pré-entraînés commettent rarement d’autres types d’erreur, comme des fautes d’orthographes ou des incohérences au niveau des parenthèses. La différence réside dans les logiques qui se cachent dans les données.

Les modèles de langage apprennent tout d’abord via une phase de pré-entraînement, un processus qui consiste à prédire le mot suivant au sein d’une énorme quantité de texte. Dans cette phase, à la différence de ce qui se passe dans les problèmes de machine learning classiques, il n’y a pas d’étiquette « vrai/faux » associée à chaque affirmation. Le modèle ne voit que des exemples positifs de formulations naturelles et doit donc estimer la distribution globale du langage.

Il est deux fois plus difficile de faire la distinction entre les affirmations valides et non valides sans exemples étiquetés d’affirmations non valides. Mais même avec les étiquettes, certaines erreurs restent inévitables. Pour bien en comprendre les raisons, basons nous sur une nouvelle analogie. Dans le domaine de la reconnaissance d’images, l’étiquetage de millions de photos de chats et de chiens permet aux algorithmes de les classer de manière fiable. Imaginons qu’au lieu d’étiqueter chaque photo en fonction de son sujet (chien ou chat), nous indiquions la date d’anniversaire de l’animal. Ces dates étant aléatoires, cette tâche générerait toujours des erreurs, quel que soit le degré de sophistication de l’algorithme.

Il en va de même pour le pré-entraînement. L’orthographe et l’organisation des parenthèses suivent une logique. Les erreurs sont donc éliminées à mesure que les volumes de données augmentent. A contrario, les faits aléatoires dont la fréquence est faible, comme la date d’anniversaire d’un animal, ne peuvent pas être prédits par une logique quelconque et génèrent donc des hallucinations. Notre analyse explique les types d’hallucinations qui résultent de la prédiction du mot suivant. Dans l’idéal, de nouvelles étapes suivant le pré-entraînement devraient pouvoir les éliminer, mais ce n’est aujourd’hui pas parfaitement le cas pour les raisons décrites dans la section précédente.

Conclusions

Nous espérons que l’explication statistique de notre étude clarifie la nature des hallucinations et bat en brèche diverses idées fausses, par exemple :

Affirmation : les hallucinations disparaîtront si nous améliorons l’exactitude des modèles, car un modèle obtenant un score d’exactitude de 100 % ne peut pas halluciner.
Constatation : l’exactitude des modèles n’atteindra jamais 100 %, car quelles que soient sa taille et ses capacités de recherche et de raisonnement,un modèle ne pourra jamais répondre à certaines des questions qui lui sont posées dans le monde réel.

Affirmation : les hallucinations sont inévitables.
Constatation : ce n’est pas vrai, car les modèle de langage peuvent choisir de ne pas répondre en cas d’incertitude.

Affirmation : éviter les hallucinations impose un niveau d’intelligence qui n’est atteignable qu’avec les plus grands modèles.
Constatation : il peut au contraire être plus facile pour un petit modèle de déterminer ses limites. Par exemple, en réponse à une question portant sur le maori, un petit modèle qui ne parle pas le maori pourrait simplement répondre « Je ne sais pas », tandis qu’un modèle qui connaît un peu de maori doit d’abord déterminer son niveau de confiance. Comme l’explique l’étude, la « calibration » demande bien moins de ressources de calcul que la fourniture d’une réponse exacte.

Affirmation : les hallucinations constituent un bug mystérieux des modèles de langage modernes.
Constatation : nous comprenons les mécanismes statistiques qui donnent naissance aux hallucinations et les récompensent lors des évaluations.

Affirmation : pour mesurer les hallucinations, nous avons simplement besoin d’une bonne évaluation spécialisée.
Constatation : des évaluations centrées sur les hallucinations ont déjà été publiées. Pour autant, une bonne évaluation n’a que peu d’effet après les centaines d’évaluations classiques basées sur l’exactitude qui pénalisent l’humilité et récompensent les hypothèses. Il convient plutôt de repenser tous les indicateurs des évaluations principales pour récompenser l’expression de l’incertitude.

Nos derniers modèles de langage présentent des taux d’hallucination réduits, et nous ne cessons de les améliorer.
