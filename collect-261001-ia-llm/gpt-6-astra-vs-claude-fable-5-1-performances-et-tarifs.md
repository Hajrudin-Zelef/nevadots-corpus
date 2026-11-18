Cours

En l’espace de deux jours, Anthropic et OpenAI ont publié leurs nouveaux modèles phares. Ils affichent exactement le même prix : 10 $ par million de jetons en entrée et 50 $ par million de jetons en sortie, ce qui, paradoxalement, complique la question habituelle « lequel est le moins cher ? » : des tarifs identiques ne donnent pas des factures identiques.

Dans cet article, je compare GPT-6 Astra et Claude Fable 5.1 sur le codage et le travail agentique, le raisonnement, l’usage de l’ordinateur, la sécurité et leur coût réel à l’usage. Pour un détour approfondi sur chaque modèle, consultez notre guide GPT-6 Astra et notre guide Claude Fable 5.1.

## TL;DR

- Le tableau de benchmarks d’OpenAI place Astra devant Fable 5.1 presque partout, tandis qu’Artificial Analysis, évaluateur indépendant, place Fable 5.1 en tête sur ses deux indices phares.
- Les prix catalogue sont identiques ; les seules différences de la grille tarifaire portent sur les lectures de cache, où Fable 5.1 est 4 fois moins cher, et sur le surcoût long contexte d’Astra.
- Au coût mesuré par tâche, la tendance s’inverse. Artificial Analysis estime Fable 5.1 à 3,76 $ par tâche de l’Intelligence Index contre 1,67 $ pour Astra, car Astra consomme bien moins de jetons pour son score.
- Privilégiez GPT-6 Astra pour l’usage de l’ordinateur, les livrables professionnels, le raisonnement en maths et sciences, la défense cyber et un coût par tâche inférieur.
- Privilégiez Claude Fable 5.1 pour la profondeur de raisonnement, pour les requêtes au-delà de 272 K jetons où Astra ajoute un surcoût et Anthropic non, et pour les boucles d’agents où la facture est dominée par les lectures de cache plutôt que par la sortie.

## Vous souhaitez vous lancer dans l'IA générative ?

Apprenez à travailler avec des LLM en Python directement dans votre navigateur

## Qu’est-ce que GPT-6 Astra ?

GPT-6 Astra est le modèle phare de pointe d’OpenAI, successeur de GPT-5.6 Sol, conçu autour de l’exécution agentique plutôt que du chat. OpenAI le positionne sur l’usage de l’ordinateur, le travail professionnel et l’ingénierie logicielle, et c’est le premier modèle OpenAI à franchir le seuil de cybersécurité « Critical » dans le Preparedness Framework de l’entreprise. Il offre une fenêtre de contexte de 1 500 000 jetons, une sortie max de 128 K et une date de connaissance arrêtée au 30 avril 2026.

Deux points saillants dans l’annonce. Dans Codex, Astra conserve des notes d’une fenêtre de contexte à l’autre au lieu de les compacter en résumé, de sorte que les fenêtres précédentes restent consultables ; et il décide quand poser une question de clarification au lieu de toujours deviner ou toujours demander.

Pour la liste complète des fonctionnalités, les tableaux de benchmarks et les modalités d’accès, consultez notre guide GPT-6 Astra. À lire en parallèle avec notre couverture de son prédécesseur : GPT-5.6 Sol, Terra et Luna.

## Qu’est-ce que Claude Fable 5.1 ?

Claude Fable 5.1 est le modèle de pointe généralement disponible d’Anthropic pour un raisonnement exigeant et des travaux agentiques de longue durée. Il propose une fenêtre de contexte d’1 M de jetons avec 128 K de sortie max, une pensée adaptative toujours active et une date de connaissance à juin 2026. La documentation d’Anthropic indique une latence plus élevée que Claude Opus 5 et Claude Sonnet 5, tous deux moins chers.

Claude Mythos 5.1 est le même modèle avec des garde-fous différents, accessible sur invitation via Project Glasswing. Le changement clé pour tous les autres concerne le prix de lecture du cache, réduit de 75 % à 0,25 $ par million de jetons, alors que les tarifs catalogue restent inchangés.

Notre guide Claude Fable 5.1 détaille l’intégralité des benchmarks, et notre tutoriel API Claude Fable 5.1 construit un agent développeur conscient du dépôt, avec un vrai détail des coûts selon l’effort.

## GPT-6 Astra vs Claude Fable 5.1 : comparaison directe

En bref : le tableau comparatif d’OpenAI montre Astra devant Fable 5.1 sur presque toutes les lignes publiées, et Artificial Analysis montre l’inverse sur ses deux indices. Lequel croire dépend du poids que vous accordez à un éditeur qui note son concurrent.

| Fonctionnalité | GPT-6 Astra | Claude Fable 5.1 | 
|---|---|---|
| Date de sortie | 3 septembre 2026 | 1 septembre 2026 | 
| ID du modèle API | `gpt-6-astra` | `claude-fable-5-1` | 
| Fenêtre de contexte | 1,05 M de jetons | 1 M de jetons | 
| Sortie maximale | 128 K jetons | 128 K jetons | 
| Date de connaissance | 30 avril 2026 | Juin 2026 | 
| Prix catalogue par 1 M de jetons | 10 $ en entrée / 50 $ en sortie | 10 $ en entrée / 50 $ en sortie | 
| Lecture d’entrée en cache par 1 M | 1,00 $ (2,00 $ au-delà de 272 K) | 0,25 $ | 
| Tarifs au-delà de 272 K jetons d’entrée | 2x entrée et cache, 1,5x sortie | Pas de surcoût | 
| FrontierMath Tier 4 (v2) | 97,6 % | 87,8 % | 
| Humanity’s Last Exam, avec outils | 57,2 % | 65,0 % | 
| ScreenSpot-Pro (sans outils) | 92,7 % | 87,3 % (Fable 5, depuis Mythos) | 
| AutomationBench | 41,4 % | 31,4 % | 
| ExploitBench | 100 % | 70 % | 
| AA Intelligence Index (effort max) | 61 | 66 | 
| AA : coût par tâche de l’Intelligence Index (max) | 1,67 $ | 3,76 $ | 
| Point fort | Usage PC, maths, cybersécurité, coût par tâche | Profondeur de raisonnement, boucles d’agents dominées par le cache | 

### Codage et workflows agentiques

Astra mène sur les benchmarks de code qu’OpenAI a publiés, mais l’écart est mince et l’indice indépendant n’est pas d’accord. Sur Terminal-Bench 4.0, qui teste l’ingénierie logicielle, la configuration système et l’analyse de données en terminal, OpenAI annonce 57,7 % pour Astra contre 55,8 % pour Fable 5.1. Anthropic publie le même 55,8 %, donc au moins cette ligne ne fait pas débat.

L’écart se creuse sur DeepSWE v1.1, où Astra atteint 74,1 % contre 67,4 %, et se referme presque totalement sur FrontierCode 1.1 Main, où 53,3 % contre 50,9 % se situe dans la fourchette déjà occupée par Claude Fable 5 (53,5 %) et Claude Opus 5 (53,4 %).

| Benchmark | GPT-6 Astra | Claude Fable 5.1 | Notes | 
|---|---|---|---|
| Terminal-Bench 4.0 | 57,7 % | 55,8 % | Le tableau d’OpenAI affiche 57,7 % tandis que la légende du graphique indique 57,9 % | 
| DeepSWE v1.1 | 74,1 % | 67,4 % | Donné par OpenAI | 
| FrontierCode 1.1 Main | 53,3 % | 50,9 % | Équivalent à Fable 5 (53,5 %) et Opus 5 (53,4 %) | 
| Migration de base interne | 63,9 % | 57,8 % | Éval interne OpenAI, non répliquée | 
| CursorBench 3.2.0 | Non publié | 73,4 % | Donné par Anthropic ; SpaceXAI a confirmé 73,4 % à effort max | 
| AA Coding Agent Index | 67 dans Codex | 70 dans Claude Code | Indépendant, mais harnais différents | 

La dernière ligne est celle qui me fait réfléchir. Artificial Analysis place Fable 5.1 dans Claude Code à 70, le meilleur score de son Coding Agent Index, et Astra dans Codex à 67, à peu près au niveau de Claude Opus 5 et Claude Fable 5. Deux avertissements :

- Les deux modèles ont tourné dans des harnais différents, Codex vs Claude Code, donc une partie des 3 points tient à l’outillage plutôt qu’au modèle. Notre comparatif Codex vs Claude Code montre leurs comportements très différents.
- Astra y parvient bien plus économiquement. Artificial Analysis l’a mesuré à un tiers des jetons de GPT-5.6 Sol à effort max dans Codex, et à un cinquième des jetons de Claude Opus 5 en xhigh.

Astra gagne les lignes publiées sur le code d’une courte tête et remporte haut la main l’argument de l’efficacité, tandis que Fable 5.1 détient le seul score d’agent de code indépendant qui les surpasse tous deux.

### Raisonnement et travail scientifique

C’est la séparation la plus nette, et elle va dans les deux sens. Astra prend les lignes maths et sciences : 97,6 % contre 87,8 % sur FrontierMath Tier 4 v2, 96,0 % contre 93,7 % sur GPQA Diamond, et 64,6 % contre 52,6 % sur Terminal-Bench Science 0.1, le benchmark de recherche agentique autour duquel Anthropic avait articulé son propre lancement.

Fable 5.1 remporte Humanity’s Last Exam avec outils, 65,0 % contre 57,2 %, et l’écart est net. Artificial Analysis confirme indépendamment la tendance, notant Fable 5.1 à 66 sur son Intelligence Index contre 61 pour Astra, le plus haut score jamais mesuré.

Un bémol sur ce 66 : Artificial Analysis a exécuté Fable 5.1 avec le fallback côté serveur par défaut d’Anthropic, qui redirige les requêtes signalées sécurité vers Claude Opus 4.8 ou Claude Opus 5 ; ce fallback a produit environ 4 % des jetons de sortie de l’indice. Le score reflète Fable 5.1 tel qu’il sera effectivement appelé, pas le modèle nu isolé.

Si votre travail est de la physique ou des maths de niveau master, Astra. S’il s’agit de raisonnement large et difficile à la Humanity’s Last Exam, Fable 5.1.

### Usage PC et livrables professionnels

Astra domine cet axe, surtout parce qu’Anthropic n’a pas publié de chiffres comparables. OpenAI annonce 72,6 % pour Astra sur l’ensemble hors ligne d’OSWorld 2.0 contre 70,2 % pour Claude Opus 5, et 92,7 % sur ScreenSpot-Pro contre 87,3 % pour Claude Fable 5. Les chiffres propres à Fable 5.1 (77,9 % partiel, 41,7 % strict) proviennent d’une autre version de tâches et d’une grille de notation différente.

Les chiffres sur les artefacts sont moins ambigus : 95,9 % sur BenchCAD contre 84,3 %, et 41,4 % contre 31,4 % sur AutomationBench.

Pour des agents qui cliquent dans de vrais logiciels et produisent des slides ou du CAD, Astra est le meilleur choix.

### Sécurité, cybersécurité et alignement

Les chiffres d’alignement d’Astra sont l’aspect le plus sous-estimé de son lancement. Sur le benchmark interne d’OpenAI pour l’usage PC sécurisé, où un score plus bas est meilleur, il atteint 2,4 % contre 9,5 % pour Fable 5.1.

En cybersécurité, il change de division : 100 % sur ExploitBench, et 86 défis FrontierCyber résolus sur 226 contre 34 pour GPT-5.6 Sol selon les tests indépendants d’Irregular. Anthropic a débridé Fable 5.1 juste assez pour trouver des vulnérabilités mais pas pour les exploiter.

Deux nuances, à garder en tête :

- OpenAI indique que le raisonnement écrit d’Astra est *plus difficile* à surveiller que celui de Sol, car il résout en moins d’étapes écrites.
- L’AISI britannique a observé Astra exécuter des attaques simulées sur la chaîne d’approvisionnement dans 2 cas sur 500, même quand le cadre interdisait l’accès internet, en baisse par rapport à 60 sur 499 quand le cadre était ambigu.

Astra est à la fois l’agent le plus sûr et l’attaquant le plus capable, d’où le contrôle d’accès.

### Tarification : ce que vous payez vraiment

Les tarifs catalogue sont identiques, donc sur la grille, la seule différence entre ces deux modèles vient du cache et du long contexte. Ce cadrage ne tient que si les deux consomment le même nombre de jetons pour la même tâche, ce qui n’est pas le cas : d’où l’importance des mesures par tâche ci-dessous, bien plus parlantes que la grille.

#### Tarifs jeton à jeton

| Tarif | GPT-6 Astra | Claude Fable 5.1 | 
|---|---|---|
| Entrée, par 1 M de jetons | 10,00 $ | 10,00 $ | 
| Sortie, par 1 M de jetons | 50,00 $ | 50,00 $ | 
| Lecture d’entrée en cache, par 1 M | 1,00 $ | 0,25 $ | 
| Écriture de cache (5 min), par 1 M | 12,50 $ | 12,50 $ | 
| Remise batch | 50 % | 50 % | 
| Tarifs au-delà de 272 K jetons d’entrée | 20,00 $ en entrée / 2,00 $ en cache / 75,00 $ en sortie | Pas de surcoût | 

Entrée, sortie, écritures de cache et remise batch s’alignent au centime près. La lecture de cache est l’exception, avec un écart de 4x : Anthropic ramène les lectures de cache de Fable 5.1 à 0,25 $, tandis qu’OpenAI facture celles d’Astra 1,00 $, soit 90 % de remise vs l’entrée.

Au-delà de 272 K jetons d’entrée, l’écart passe à 8x, car le surcoût d’OpenAI double aussi les lectures de cache à 2,00 $ en même temps que l’entrée, tandis que la doc tarifaire d’Anthropic maintient la fenêtre de 1 M de jetons au tarif standard. Notez la faiblesse du seuil au regard de l’annonce d’Astra : 272 K, c’est à peu près un quart de sa fenêtre de 1,05 M ; toute requête au-delà d’un quart de son contexte est facturée au palier supérieur.

Les lectures de cache sont le tarif qui se cumule sur une longue boucle, car un agent renvoie à chaque tour le même système, les définitions d’outils et le contexte d’un dépôt. Notre guide du prompt caching détaille la différence écriture vs lecture de cache, si elle vous est nouvelle.

#### Ce que coûte une vraie charge selon la grille

Tout ce tableau est un calcul catalogue sur une forme de jetons figée, pas un résultat mesuré. Il répond à : « si les deux modèles consommaient des jetons identiques, combien la grille facturerait-elle ? » La section suivante répond à la question de ce qu’ils consomment réellement.

| Charge | GPT-6 Astra | Claude Fable 5.1 | Différence | 
|---|---|---|---|
| Assistant équilibré : 1 M entrée / 250 K sortie | 22,50 $ | 22,50 $ | 0 $, 0 % | 
| Recherche, sous le seuil : 10 M entrée / 1 M sortie | 150 $ | 150 $ | 0 $, 0 % | 
| Recherche, au-delà du seuil : 10 M entrée / 1 M sortie | 275 $ | 150 $ | 125 $, 83 % de plus pour Astra | 
| Boucle très cache : préfixe 100 K, 1 000 lectures | 201 $ | 126 $ | 75 $, 59 % de plus pour Astra | 

La formule est la même : (volume ÷ 1 M) × tarif, additionnée sur l’entrée neuve, les lectures et écritures de cache, et la sortie. La ligne « boucle cache » suppose un préfixe de 100 K écrit une fois puis relu 1 000 fois, plus 5 K d’entrée neuve et 1 K de sortie par requête.

La ligne « recherche au-delà du seuil » est la plus parlante. Gardez chaque requête sous 272 K jetons d’entrée, et les deux coûtent exactement 150 $. Faites passer les mêmes 10 M de jetons dans des requêtes qui dépassent chacune 272 K, et Astra grimpe à 275 $ tandis que Fable 5.1 reste à 150 $, car Anthropic facture la fenêtre d’1 M au tarif standard.

La boucle très cache est celle que beaucoup rencontreront. À 1 000 lectures en cache d’un préfixe de 100 K, l’écart de 0,75 $ par million de jetons mis en cache devient 75 $ sur une charge autrement identique. Notez la « forme » : volontairement pauvre en sortie, 1 K de sortie par requête pour 100 K de lectures en cache. C’est la seule forme où l’avantage de cache de Fable 5.1 décide de la facture.

#### Ce que coûte vraiment chaque tâche

Dès qu’on mesure la consommation réelle de jetons au lieu de la supposer, le tableau s’inverse. Artificial Analysis tarife chaque modèle par tâche de l’Intelligence Index, et sa méthode intègre déjà entrée, lectures/écritures de cache, pensée et réponse ; l’avantage cache de Fable 5.1 est donc inclus.

| Niveau d’effort | GPT-6 Astra : score/coût par tâche | Claude Fable 5.1 : score/coût par tâche | 
|---|---|---|
| max | 61 / 1,67 $ | 66 / 3,76 $ | 
| xhigh | 61 / 1,20 $ | 65 / 2,72 $ | 

À effort max, Fable 5.1 coûte 2,25 fois Astra pour le même lot de tâches à prix catalogue identique. Artificial Analysis aboutit au même constat sur son indice de code, où Astra égale Fable 5 "pour moins de la moitié du coût, grâce à des gains d’efficacité de jetons significatifs". L’échelle d’Astra descend à 0,46 $ par tâche à faible effort.

Fable 5.1 n’est pas pénalisé par sa grille ici, mais par son volume de sortie. Artificial Analysis l’a mesuré avec environ 1,7x les jetons de sortie de Fable 5 à effort max, d’où un coût par tâche 20 % plus élevé que son prédécesseur malgré la baisse du cache. Sans cette baisse, on serait à environ 5,16 $ : la remise agit vraiment, mais pas assez.

Vous payez ce surcoût pour quelque chose. Fable 5.1 marque 5 points de plus sur le même indice, et 4 points de plus en xhigh. La question est de savoir si 5 points valent 2,25x : à vous de juger, mais l’idée que des prix catalogue identiques donnent des factures identiques ne résiste pas aux mesures.

Les légendes d’OpenAI vont dans le même sens, pour ce que vaut l’auto-déclaration d’un éditeur : coût API estimé par tâche environ 31 % sous Fable 5.1 sur Terminal-Bench Science 0.1, 63 % sous sur Terminal-Bench 4.0 et 86 % sous sur BenchCAD, mesurés aux réglages d’effort choisis.

## Comment GPT-6 Astra et Claude Fable 5.1 ont performé

Astra a légèrement remporté notre test globalement, bien que les deux modèles aient fourni une simulation fonctionnelle du premier coup.

### Le test

J’ai exécuté une tâche difficile contre les deux modèles dans des conditions identiques, plutôt que plusieurs tâches superficielles. Le test retenu ici : une simulation de physique from scratch dans un seul fichier HTML, choisie car elle éprouve exactement ce que les deux éditeurs disent avoir amélioré : la justesse soutenue sur une construction longue, sans bibliothèque d’appui.

Cette occurrence est une version rafraîchie du test. La variante publiée utilisait un conteneur carré en rotation ; ici, on remplace par un hexagone en rotation avec un obstacle central en contre-rotation et une masse proportionnelle à la taille, ce qui augmente la difficulté tout en gardant un confinement clair à juger. Voici l’invite, collée mot pour mot dans un nouveau chat pour chaque modèle :

```
Build a single-file HTML page (inline CSS and JS, canvas, no build step, no external libraries, no network) that simulates a few dozen balls of varying sizes bouncing under gravity inside a **slowly rotating hexagonal container**, with a **smaller counter-rotating obstacle at the centre** that the balls also collide with. 
Ball mass should scale with size, so larger balls shove smaller ones around. 
The balls should collide with each other, with the hexagon's walls, and with the central obstacle, and lose a little energy on each collision so the system settles rather than gaining energy over time. 
Both the hexagon and the inner obstacle keep rotating throughout, so the balls should slosh and re-pile as they turn.
Ship it as one working file named `index.html` that starts animating on load. 
Do not install packages. 
Do not open, screenshot, or headless-render the page (no Playwright, Puppeteer, or Chrome). 
Do not ask me clarifying questions — make reasonable assumptions and note them briefly in a comment at the top of the file.
```
Les deux modèles tournent à haut niveau d’effort de raisonnement, une tentative chacun sans relance, dans le même agent de code et avec les mêmes outils. Barème : passage/échec sur exécution, puis note de 1 à 5 sur la justesse physique, la stabilité sur 30 secondes et la qualité visuelle.

### Ce que GPT-6 Astra a produit

Astra a passé le test d’exécution et a obtenu 5/5 sur les trois axes. Il y est parvenu en 6 tours et 9 appels d’outils, avec un mix intéressant : écriture du fichier, 5 relectures, 2 grep, puis 2 patches avant de conclure.

La simulation est correcte. 44 balles, masse proportionnelle à l’aire, confinement maintenu sur 30 secondes pendant que l’hexagone tourne. Astra a dimensionné l’obstacle central assez grand par rapport à la chambre, de sorte que les balles le percutent régulièrement au lieu de se figer hors de sa portée.

Il a aussi construit une page éditoriale autour de la simulation, avec un titre en sérif, des libellés monospace élargis et un panneau de télémétrie affichant les vitesses de rotation. Personne ne l’avait demandé, mais c’est plutôt agréable. Il a livré des contrôles pause et redémarrage, non demandés, mais utiles pour l’inspection.

Astra fait tourner la chambre à 0,09 rad/s et l’obstacle à -0,16 rad/s, environ 3 fois plus lent que Fable pour la paroi externe et 5 fois plus lent pour l’obstacle. Plus lisible, et une mise à l’épreuve plus douce des frontières mobiles. L’invite demande une rotation lente, donc c’est conforme.

### Ce que Claude Fable 5.1 a produit

Fable a également passé l’exécution, avec 4 en justesse physique, 5 en stabilité et 4 en qualité visuelle. Il n’a eu besoin que de 2 tours et d’un seul appel d’outil : une écriture, sans relecture ni correctif.

La physique est saine sans être parfaite. Les balles adhèrent légèrement aux parois, d’où le point perdu en justesse, et en quelques secondes elles se regroupent dans les angles inférieurs, hors de portée de l’obstacle contre-rotatif qui reste inactif la plupart du temps.

Fable a mis l’accent sur l’instrumentation plutôt que sur la présentation. Un HUD affiche en temps réel le nombre de balles, le framerate, l’énergie cinétique et les deux vitesses de rotation, et un clic dans l’hexagone ajoute une balle au point visé. Ce dernier détail s’avère très pratique pour tester les collisions à la main.

### Résultats

| Mesure | GPT-6 Astra | Claude Fable 5.1 | 
|---|---|---|
| Tours | 6 | 2 | 
| Appels d’outils | 9 | 1 | 
| Exécution | OK | OK | 
| Justesse physique | 5 | 4 | 
| Stabilité dans le temps | 5 | 5 | 
| Qualité visuelle | 5 | 4 | 
| Score barème | 5,0 | 4,3 | 

Astra remporte ce test sur la qualité, avec un écart plus marqué sur la présentation que sur la physique. Leur divergence tient à la définition du travail : Astra a lu l’invite comme un brief à interpréter, ajoutant des contrôles, une mise en page éditoriale et une rotation plus lente, agréable à observer. Fable l’a lue comme un cahier des charges, l’a satisfait en un jet et a consacré le reste à un HUD de diagnostic.

Un seul run par modèle : prenez cela comme un point de données, pas comme un benchmark. Il rapporte les tours, pas les jetons ni le coût, et n’évalue que du code de simulation from scratch, pas l’usage PC ni le raisonnement vus plus haut.

## Quand choisir GPT-6 Astra vs Claude Fable 5.1

Avec des tarifs identiques, la décision dépend de la forme de votre charge, de la consommation réelle de jetons et de l’agent de code que vous utilisez déjà. Les trois chiffres à retenir : le facteur 4x sur les lectures de cache, le seuil de surcoût à 272 K, et le rapport 2,25x sur le coût mesuré par tâche.

### Choisissez GPT-6 Astra si…

- **Vos agents opèrent de vrais logiciels.** Avec 72,6 % sur l’offline d’OSWorld 2.0 et 92,7 % sur ScreenSpot-Pro, c’est le seul des deux avec un historique publié sur l’ancrage et les clics dans des applications desktop.
- **Vous avez besoin de livrables professionnels soignés.** Slides, feuilles de calcul et sorties CAD sont des cibles d’entraînement déclarées, et le 95,9 % sur BenchCAD contre 84,3 % est l’écart le plus large publié sur les artefacts.
- **Le travail est en maths ou sciences physiques.** FrontierMath Tier 4 v2 à 97,6 % contre 87,8 %, et GPQA Diamond à 96,0 % contre 93,7 %.
- **Vous faites de la sécurité défensive.** 100 % sur ExploitBench et 86 défis FrontierCyber résolus sur 226 le placent loin devant, sous réserve d’accepter des capacités soumises au programme Daybreak d’OpenAI.
- **Vous utilisez déjà Codex.** Les notes de contexte inter-fenêtres sont une fonctionnalité de Codex, et changer de harnais pour 3 points d’indice rapporte rarement.
- **Le coût par tâche compte plus que le score plafond.** 1,67 $ contre 3,76 $ sur l’Intelligence Index d’Artificial Analysis à effort max, et 0,46 $ à faible effort si 57 au lieu de 61 vous suffit.

### Choisissez Claude Fable 5.1 si…

- **Vous exécutez de longues boucles d’agents où la facture, c’est la lecture de cache, pas la sortie.** À 0,25 $ par million de lectures en cache contre 1,00 $, la charge « très cache » ci-dessus coûte 126 $ au lieu de 201 $. L’avantage se réduit dès que les jetons de sortie dominent, ce que montrent les mesures par tâche.
- **Vos requêtes sont volumineuses.** Anthropic n’ajoute pas de surcoût long contexte, donc la charge de 10 M de jetons reste à 150 $, là où Astra grimpe à 275 $, et les lectures de cache d’Astra doublent aussi au-delà du seuil.
- **Vous voulez le meilleur score de raisonnement indépendant et êtes prêt à le payer.** Artificial Analysis le note 66 sur l’Intelligence Index contre 61, et il remporte Humanity’s Last Exam avec outils 65,0 % contre 57,2 %.
- **Vous travaillez dans Claude Code.** Fable 5.1 dans Claude Code est le record de l’indice d’agents de code d’Artificial Analysis avec 70, et l’effort élevé y est le défaut.

## Comment démarrer avec GPT-6 Astra et Claude Fable 5.1

| Surface | GPT-6 Astra | Claude Fable 5.1 | 
|---|---|---|
| App grand public | ChatGPT Plus, Pro, Business, Enterprise | Claude web, mobile, desktop (Pro, Max, Team, Enterprise) | 
| API éditeur | OpenAI API | Claude API | 
| Plateformes cloud | Amazon Bedrock, Microsoft Azure/Foundry | Amazon Bedrock, Google Cloud, Microsoft Azure/Foundry | 
| Agents de code | Codex | Claude Code, Cursor | 
| Routeurs tiers | OpenRouter, Vercel AI Gateway | OpenRouter, Vercel AI Gateway | 
| ID du modèle API | `gpt-6-astra` | `claude-fable-5-1` | 

Deux détails d’accès peuvent vous bloquer avant le premier appel. Astra est désactivé par défaut pour les espaces Enterprise jusqu’à activation par un administrateur, et Fable 5.1 impose une rétention des données de 30 jours et n’est pas compatible avec Priority Tier. La présence cloud est quasi à parité, Google Cloud étant actuellement la seule plateforme où Fable 5.1 est disponible et GPT-6 Astra non.

### Utiliser GPT-6 Astra et Claude Fable 5.1 dans un agent de code

Chaque modèle est natif de son harnais : Astra dans Codex et Fable 5.1 dans Claude Code, où vous basculez avec `/model claude-fable-5-1` ou le flag `--model`. Via API, le swap tient à une chaîne, bien que les SDK diffèrent.

```
from anthropic import Anthropic
client = Anthropic()
response = client.messages.create(
    model="claude-fable-5-1",  # OpenAI SDK equivalent: model="gpt-6-astra"
    max_tokens=16000,          # thinking plus response share this budget
    output_config={"effort": "high"},
    messages=[{"role": "user", "content": "Refactor this module..."}],
)
print(response.content[0].text)
```
Trois changements de Fable 5.1 casseront du code écrit pour Fable 5 : la sélection d’outil forcée renvoie désormais un 400, les anciens modèles ne lisent pas ses blocs de pensée, et ces blocs sont liés exactement à l’historique qui les précède. Notre tutoriel API Claude Fable 5.1 couvre ces points, et notre guide débutant de l’OpenAI API présente l’équivalent côté OpenAI.

## Conclusion

Choisissez selon la forme de vos charges et le coût mesuré, pas selon les tableaux de benchmarks. GPT-6 Astra est meilleur pour les agents qui cliquent dans des logiciels, font des maths ou produisent des livrables prêts pour le client, et semble le moins cher par tâche malgré une grille identique. Claude Fable 5.1 offre le meilleur score de raisonnement indépendant et coûte moins sur de très grandes requêtes et sur des boucles dominées par le cache.

La grille tarifaire est le piège ici. Deux modèles à 10/50 $ paraissent interchangeables, et le seul écart visible — les lectures de cache — tourne à l’avantage d’Anthropic par 4x. Puis vous mesurez ce que chacun dépense pour terminer une tâche, et Astra ressort à 44 % du coût de Fable 5.1. Je ne l’aurais pas prédit à la lecture des pages prix, et c’est le premier point à vérifier sur votre propre trafic avant de trancher.

Si vous voulez utiliser ces modèles plutôt que lire sur eux, je vous recommande notre cours Introduction to Claude Models côté Anthropic et Working with the OpenAI API côté OpenAI.

## FAQ

### GPT-6 Astra est-il meilleur que Claude Fable 5.1 ?

Tout dépend des benchmarks que vous consultez. Dans le tableau d’OpenAI, GPT-6 Astra devance Claude Fable 5.1 sur presque toutes les lignes publiées, dont 97,6 % contre 87,8 % sur FrontierMath Tier 4 v2 et 64,6 % contre 52,6 % sur Terminal-Bench Science 0.1. Artificial Analysis, évaluateur indépendant, dit l’inverse : Fable 5.1 à 66 sur son Intelligence Index contre 61 pour Astra. Choisissez Astra pour l’usage PC, les maths et la cybersécurité, et Fable 5.1 pour la profondeur de raisonnement et les longues boucles en cache.

### Combien coûtent GPT-6 Astra et Claude Fable 5.1 ?

Les deux affichent 10 $ par million de jetons en entrée et 50 $ par million en sortie, avec la même remise de 50 % en batch et 12,50 $ pour l’écriture de cache. Sur la grille, la seule différence concerne les lectures de cache : 0,25 $ par million pour Claude Fable 5.1 contre 1,00 $ pour GPT-6 Astra, montant à 2,00 $ au-delà de 272 K jetons d’entrée, où Astra facture aussi 20 $ l’entrée et 75 $ la sortie. Anthropic n’ajoute aucun surcoût, quelle que soit la longueur. Mesuré par tâche, cette différence s’inverse : Artificial Analysis estime Fable 5.1 à 3,76 $ par tâche contre 1,67 $ pour Astra.

### Quel est le meilleur pour coder, GPT-6 Astra ou Claude Fable 5.1 ?

GPT-6 Astra mène les benchmarks de code publiés par chaque éditeur : 57,7 % contre 55,8 % sur Terminal-Bench 4.0 et 74,1 % contre 67,4 % sur DeepSWE v1.1. Artificial Analysis n’est pas d’accord au niveau du harnais, plaçant Claude Fable 5.1 dans Claude Code à 70 sur son Coding Agent Index contre 67 pour GPT-6 Astra dans Codex. Comme ces runs utilisent des harnais différents, une partie de l’écart vient de l’outillage.

### Où puis-je accéder à GPT-6 Astra et Claude Fable 5.1 ?

GPT-6 Astra est disponible dans ChatGPT avec les offres Plus, Pro, Business et Enterprise, via l’OpenAI API et dans Codex. En Enterprise, un administrateur doit l’activer car l’accès est désactivé par défaut au lancement. Claude Fable 5.1 fonctionne sur les apps web, mobile et desktop de Claude, l’API Claude et Claude Code, et exige une rétention des données de 30 jours. Les deux sont disponibles sur Amazon Bedrock, Google Cloud, Microsoft Foundry et des routeurs tiers comme OpenRouter.

### Quels sont les IDs de modèle API de GPT-6 Astra et Claude Fable 5.1 ?

Les IDs de modèle sont `gpt-6-astra` pour OpenAI et `claude-fable-5-1` pour Anthropic. Sur Amazon Bedrock, Claude Fable 5.1 est `anthropic.claude-fable-5-1`. Notez trois changements API entre Fable 5 et 5.1 : la sélection forcée d’outil renvoie un 400, les anciens modèles ne lisent pas ses blocs de pensée, et ces blocs sont liés exactement à l’historique précédent. Les refus arrivent en HTTP 200 avec `stop_reason: \"refusal\"`.

**Rédacteur en chef Data Science chez DataCamp |** **Je suis passionné par la prévision et le développement à l'aide d'API.**