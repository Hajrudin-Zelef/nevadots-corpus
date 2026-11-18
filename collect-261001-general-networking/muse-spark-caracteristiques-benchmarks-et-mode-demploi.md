Cursus

Nous publiions à bon rythme des articles sur les modèles Llama de Meta (Llama 2, Llama 3, etc.). Puis Llama 4 est arrivé en avril 2025 sous le feu des critiques : plusieurs médias et le directeur de l’IA sortant de l’entreprise ont confirmé que des résultats de benchmarks avaient été manipulés à l’aide de sous-modèles spécialisés jamais publiés.

Après cela, les mises à jour se sont interrompues. Au même moment, Meta annonçait le passage de Horizon Worlds au mobile uniquement, mettant de fait fin à la version VR sur laquelle elle avait jadis misé l’avenir de l’entreprise. L’ensemble donnait l’image d’une société perdant pied sur deux fronts à la fois.

Le 8 avril 2026, Meta a lancé Muse Spark, le premier modèle issu de Meta Superintelligence Labs. Le communiqué répète un peu trop l’expression « superintelligence personnelle ». Une fois cette couche marketing ôtée, on découvre un vrai modèle qui remet Meta dans la course au plus haut niveau.

Pour comparer le nouveau modèle de Meta à l’un de ses meilleurs concurrents actuels, nous vous recommandons notre guide Muse Spark vs Claude Opus 4.6. Vous pouvez aussi lire notre guide de Muse Glimmer et notre tutoriel pour exécuter Muse Glimmer en local.

## Qu’est-ce que Muse Spark ?

Muse Spark est un modèle de raisonnement nativement multimodal qui gère texte, images, audio et outils au sein d’une architecture unique. Il prend en charge la chaîne de pensée visuelle : le modèle peut décomposer pas à pas des problèmes basés sur des images au lieu de produire une réponse unique. L’orchestration multi-agents fait aussi partie de l’équation, nous y reviendrons.

Les premiers modèles Llama renvoyaient des réponses par appariement de motifs appris pendant l’entraînement. Muse Spark réfléchit au problème avant de répondre. C’est là le vrai changement.

### Qui est aux commandes ?

Meta Superintelligence Labs, ou MSL, a été créé le 30 juin 2025, lorsque Mark Zuckerberg a réorganisé les activités IA de l’entreprise. Alexandr Wang, ex-CEO de Scale AI, est arrivé comme Chief AI Officer ; Meta avait investi environ 14 milliards de dollars dans Scale AI dans le cadre de l’accord.

Nat Friedman, ex-CEO de GitHub, pilote le produit et la recherche appliquée, et Shengjia Zhao, qui a co-créé GPT-4 et o1 chez OpenAI (le même o1 auquel Muse Spark est désormais comparé en benchmarks), est Chief Scientist.

Un troisième élément compte : Yann LeCun, Chief AI Scientist historique de Meta et principal défenseur de l’open source dans l’entreprise, est parti en novembre 2025. Son départ a suivi des changements organisationnels ayant limité son rôle et le passage de l’équipe à un développement fermé.

## Quoi de neuf avec Muse Spark (et pourquoi s’y intéresser) ?

Les points phares : des modes de raisonnement, une chaîne d’entraînement refondue et un accent volontaire sur la santé. Passons-les en revue.

### Trois modes de raisonnement

Muse Spark propose trois façons d’interagir avec lui, et la distinction vaut la peine d’être comprise avant de l’essayer.

- **Instant** est le mode par défaut pour les questions courantes. Il répond rapidement sans raisonnement prolongé, à l’image d’un modèle de chat standard.
- **Thinking** utilise une chaîne de pensée étendue. Le modèle prend plus de temps, décompose des étapes intermédiaires et obtient généralement de meilleurs résultats sur les problèmes difficiles. La plupart des résultats de benchmarks cités ici proviennent de ce mode.
- **Contemplating** est le plus intéressant. Détails ci-dessous.

Précision utile d’emblée : le mode Contemplating déploie progressivement et n’était pas accessible à tous au jour du lancement. Si vous ne le voyez pas encore, c’est normal.

### Le mode Contemplating

Le mode Contemplating lance plusieurs agents de raisonnement en parallèle, puis combine leurs sorties en une seule réponse. Là où le Deep Think de Gemini et le mode GPT Pro d’OpenAI étendent le raisonnement en pensant plus longtemps, Muse Spark le fait en pensant plus large. Davantage d’agents travaillent simultanément plutôt qu’un seul plus longtemps.

Meta avance que cette approche produit des résultats comparables avec une latence plus faible, puisque les agents opèrent en parallèle et non séquentiellement. Ces chiffres de latence ne sont pas encore confirmés indépendamment, mais les benchmarks du mode Contemplating mènent sur plusieurs évaluations difficiles (nous y revenons).

Il s’agit d’une fonctionnalité au moment de l’inference, pas d’un choix d’architecture. Le modèle lui-même ne change pas.

### Montée en puissance par renforcement et compression de la pensée

Meta a reconstruit sa chaîne d’entraînement de zéro pendant les neuf mois de développement de Muse Spark. Les affirmations sur l’apprentissage par renforcement (RL) viennent du blog technique de Meta et n’ont pas été vérifiées indépendamment.

Le point le plus intéressant est une technique appelée compression de la pensée. Pendant l’entraînement en RL, le modèle est récompensé quand il trouve la bonne réponse, mais également pénalisé pour le temps de réflexion, ce qui revient à limiter les jetons de sortie. Cela induit un comportement en trois phases sur des tâches complexes comme les problèmes de mathématiques.

D’abord, le modèle progresse en « pensant plus longtemps ». Puis la pénalité de longueur s’active et force le modèle à résoudre les mêmes problèmes avec beaucoup moins de jetons. À un moment, il étend à nouveau son raisonnement et dépasse ses plafonds précédents tout en utilisant moins de jetons.

Conséquence pratique : le modèle a appris à faire plus avec moins. Cette affirmation repose sur les courbes d’entraînement de Meta, non validées indépendamment.

### Un besoin de calcul divisé par 10

Meta affirme que sa nouvelle architecture égale les performances de Llama 4 Maverick avec dix fois moins de calcul d’entraînement. C’est une question d’efficacité d’architecture, pas un plafond pour Muse Spark. Llama 4 Maverick a obtenu 18 sur l’Artificial Analysis Intelligence Index. Muse Spark a obtenu 52.

Les chiffres d’efficacité des jetons d’Artificial Analysis vont dans le même sens. Muse Spark a utilisé 58 millions de jetons de sortie. GPT-5.4 en a utilisé 120 millions. Claude Opus 4.6 en a utilisé 157 millions.

### Santé : un axe assumé

La santé est le terrain de benchmark où Muse Spark se distingue le plus, et ce n’est pas un hasard. Meta a travaillé avec plus de 1 000 médecins pour curer des données d’entraînement spécifiques au raisonnement médical.

Le modèle peut générer des affichages interactifs couvrant la composition nutritionnelle, les informations sur les médicaments et la physiologie de l’exercice. Sur HealthBench Hard, Muse Spark a obtenu 42,8 contre 40,1 pour GPT-5.4 et 20,6 pour Gemini 3.1 Pro. Cet écart avec Gemini se confirme sous évaluation indépendante.

C’est clairement la réponse de Meta à ChatGPT Health. L’argument de Meta pour expliquer sa compétitivité : le contexte social de 3 milliards d’utilisateurs, qui lui donnerait un avantage pour comprendre comment les gens posent vraiment leurs questions de santé. À voir si cela tient pour des requêtes complexes ou atypiques, au-delà des questions courantes qui dominent les benchmarks.

## Et Llama dans tout ça ?

La communauté développeurs pose une question légitime, qui mérite une réponse claire.

Muse Spark n’est pas open-source. Tous les modèles Llama jusqu’à Llama 4 étaient livrés avec des poids que les développeurs pouvaient télécharger et exécuter en local. Des communautés comme r/LocalLLaMA se sont construites dessus. Cet usage disparaît.

La raison invoquée par Meta est en partie concurrentielle : des laboratoires chinois, dont DeepSeek, ont utilisé les poids de Llama pour accélérer leurs recherches. Wang a déclaré que l’entreprise « espère » ouvrir le code des futurs modèles Muse, sans calendrier. « Espère » porte ici une lourde charge.

L’équipe Llama a été intégrée au labo de Wang, et Llama 4 est le dernier modèle issu de l’ancienne structure. Que Llama continue aux côtés de Muse ou s’efface discrètement, Meta ne le dit pas.

## Résultats de benchmarks de Muse Spark

Avec Muse Spark, les benchmarks exigent de distinguer d’emblée une chose importante : compte tenu de l’historique de Llama 4 chez Meta, séparez bien les chiffres déclarés par l’éditeur et ceux vérifiés indépendamment.

Voici les résultats en mode Thinking, qui permettent les comparaisons les plus équitables.

Source : Meta Superintelligence Labs / ai.meta.com

Le mode Contemplating dispose d’un autre jeu de résultats pour les évaluations les plus difficiles. Il mène sur Humanity’s Last Exam et FrontierScience Research, mais reste derrière GPT-5.4 Pro et Gemini 3.1 Deep Think sur les problèmes de théorie de l’IPhO 2025 en physique. Tout cela provient de Meta : considérez ces chiffres comme des indications, pas des verdicts.

Source : Meta Superintelligence Labs / ai.meta.com

Le panorama indépendant proposé par Artificial Analysis est plus mesuré. Ils placent Muse Spark quatrième sur leur Intelligence Index, derrière Gemini 3.1 Pro Preview, GPT-5.4 et Claude Opus 4.6. Toujours dans le top 5 mondial. Les chiffres mettent aussi en lumière les points faibles : ARC-AGI-2 et Terminal-Bench 2.0 doivent retenir votre attention si le code ou le raisonnement abstrait sont essentiels pour votre cas d’usage.

## Mettre Muse Spark à l’épreuve

Avec ces scores en tête, testons Muse Spark. J’examine le modèle sur le raisonnement multi-étapes, la compréhension d’image et le débogage de code.

### Test 1 : chaîne logique Fibonacci–binaire (stabilité d’un raisonnement en cascade)

Dans ce premier test, je cible les capacités avancées de raisonnement de Muse Spark sur un exercice multi-étapes. Le modèle doit :

- Identifier le bon terme de Fibonacci
- Le convertir correctement en binaire
- Compter précisément les bits
- Générer les nombres premiers dans un intervalle calculé
- Effectuer une grande sommation

Le prompt utilisé :

```
Step 1: Find the 13th number in the Fibonacci sequence (starting with F1=1, F2=1). Let this be X.
Step 2: Convert X into a binary string (Base 2).
Step 3: Count the number of '1's in that binary string. Let this count be C.
Step 4: Identify all prime numbers (p) such that 20 ≤ p ≤ (C × 100).
Step 5: Calculate the sum of these primes. What is the final result?
```
Muse Spark s’en est très bien sorti et a résolu l’exercice du premier coup. C’est d’autant plus impressionnant que GPT-5.4 a échoué à la dernière étape et n’a réussi qu’après division en deux étapes (lister les nombres premiers puis les additionner).

### Test 2 : compréhension d’image et raisonnement commercial sur un graphique temporel multi-séries

Meta affirme que Muse Spark comprend très bien les images complexes ; j’utilise donc le graphique temporel multi-séries suivant pour voir s’il peut détecter des motifs et les traduire en recommandations exploitables.

Voici le prompt :

`Examine this multi-line time-series of monthly active users for three products. Describe the key patterns you see, explain how events likely impacted each product, and propose data-driven next steps for the business.`
Muse Spark a correctement identifié tous les motifs, ce qui laisse penser que la reconnaissance d’image fonctionne bien.

Les données étaient générées aléatoirement, donc il n’y a pas de vérité absolue ici. Cela dit, Muse Spark identifie tous les événements, raisonne par produit et par période, et aboutit à des conclusions pertinentes. Il analyse même l’évolution de la somme des MAU (utilisateurs actifs mensuels) de combinaisons de produits, sans y avoir été invité, ce qui est un vrai plus.

Toutes les prochaines étapes suggérées sont alignées sur l’analyse des motifs de MAU et des effets des événements. Muse Spark a mis le doigt sur le thème central de chaque produit (playbook de lancement pour A, tarification pour B, passage à l’échelle pour C) et proposé des actions concrètes cohérentes.

### Test 3 : débogage de code

Enfin, je teste les compétences de Muse Spark pour diagnostiquer des bugs. L’objectif est de voir si le modèle se limite à vérifier la correction ligne par ligne, ou s’il sait aussi détecter des erreurs conceptuelles.

Le prompt :

```
A developer wrote this Python function to compute a running average: 
def running_average(data, window=3): 
    result = [] 
    for i in range(len(data)): 
        start = max(0, i - window + 1) 
        chunk = data[start:i + 1] 
        result.append(round(sum(chunk) / window, 2)) 
    return result 
When called with running_average([10, 20, 30, 40, 50]), the first two values in the output seem wrong. Why? Please help me fix what is wrong!
```
La fonction divise toujours par `window (3)`, même au début quand le segment a moins de 3 éléments. La sortie buggée est `[3.33, 10.0, 20.0, 30.0, 40.0]`, mais les deux premières valeurs devraient être `10.0` et `15.0` puisque ces segments contiennent respectivement 1 et 2 éléments. La correction consiste à remplacer `/ window` par  `/ len(chunk)`.

Les modèles parcourent souvent parfaitement la boucle, mais concluent que la sortie semble « correcte ». Ils voient les calculs étape par étape et ne signalent pas que diviser un seul élément par 3 n’a pas de sens. Il faut ici conserver l’intention (ce que doit faire une moyenne glissante) tout en suivant l’exécution (ce que fait le code) et détecter l’écart.

Muse Spark a identifié l’intention (une moyenne glissante) et détecté l’erreur. Il a proposé la bonne correction et expliqué pourquoi elle est nécessaire. Il a même suggéré une variante au cas où l’on souhaiterait ignorer les fenêtres partielles.

Au final, le modèle a réussi les trois tests et laisse une excellente première impression.

## Comment accéder à Muse Spark ?

Vous pouvez accéder à Muse Spark sur meta.ai ou via l’application Meta AI sur iOS et Android. Les deux sont gratuits. Le déploiement commence aux États-Unis, avec une extension annoncée dans les semaines suivantes.

Meta prévoit un déploiement au même rythme sur WhatsApp, Instagram, Facebook, Messenger et ses lunettes Ray-Ban AI.

Il n’y a pas d’API publique. Un aperçu privé est ouvert à certains partenaires entreprises, sans date confirmée pour un accès plus large. Côté confidentialité : la politique de Meta fixe peu de limites à l’utilisation des conversations pour améliorer ses modèles. Si vous envisagez de partager des informations sensibles, lisez d’abord les conditions.

## Là où Muse Spark pêche

Meta l’a dit clairement dans son billet technique : le modèle a des lacunes sur les tâches multi-étapes pilotées par des agents et sur les workflows de code.

Sur SWE-Bench Verified, l’écart avec Gemini et Opus 4.6 est faible. Il se creuse sur le travail agentif : Terminal-Bench 2.0 (59,0 contre 75,1 pour GPT-5.4) et GDPval-AA pour l’automatisation de bureau (1 444 contre 1 672 pour GPT-5.4). Les écarts sont nets.

Le raisonnement visuel abstrait suit le même schéma : ARC-AGI-2 affiche 42,5 pour Muse Spark, contre autour de 70 et plus pour GPT-5.4 et Gemini. Le modèle qui excelle sur la lecture de graphiques est distancé sur des motifs visuels inédits.

Ce dernier point a déclenché une réaction le jour du lancement. François Chollet, cofondateur de l’ARC Prize et créateur de Keras et ARC-AGI, a qualifié le modèle d’« suroptimisé pour les chiffres publics de benchmarks au détriment du reste ». Wang a reconnu l’écart sur ARC-AGI-2 et a mis en avant des retours positifs d’utilisateurs sur le code et le raisonnement visuel. Reste à voir si cela se confirme à grande échelle.

L’absence d’API publique, comme évoqué plus haut, ajoute un déficit concurrentiel. Wang l’a reconnu au lancement : « Il y a assurément des aspérités de comportement que nous allons polir avec le temps. »

## Sécurité de Muse Spark

Meta a mené des évaluations dans le cadre de son Advanced AI Scaling Framework avant le lancement. Sur BioTIER-refuse, Muse Spark arrive en tête du comparatif pour le refus des requêtes sur les armes biologiques. Ces chiffres proviennent de Meta.

Source : Meta Superintelligence Labs / ai.meta.com

Le constat le plus intéressant vient d’Apollo Research. Ils ont observé que Muse Spark présentait le taux le plus élevé de sensibilité à l’évaluation parmi les modèles testés : le modèle identifiait fréquemment un contexte de test de sécurité et adaptait son comportement en conséquence.

Un modèle qui « se tient bien » seulement quand il se sait observé pose un véritable problème. Les travaux antérieurs d’Apollo ont documenté que ce schéma peut accroître ce qu’ils appellent un « comportement stratégique » en production.

Meta a reconnu ce constat au lancement, ce que peu de laboratoires font. Leur suivi indique que cela affectait un sous-ensemble restreint d’évaluations d’alignement, sans lien avec des capacités dangereuses, et n’était pas bloquant. Les recherches se poursuivent.

## Muse Spark vs GPT-5.4 vs Opus 4.6 vs Gemini 3.1

Les benchmarks montrent ce que ces modèles savent faire. Cette section vous aide à choisir lequel utiliser en pratique.

### En un clin d’œil

| **Caractéristique** | **Muse Spark** | **GPT-5.4** | **Opus 4.6** | **Gemini 3.1 Pro** | 
| Date de sortie | 8 avr. 2026 | 5 mars 2026 | 5 févr. 2026 | 19 févr. 2026 | 
| Fenêtre de contexte | 262 K* | 1,05 M | 1 M depuis le 13 mars | 1 M | 
| Modalités d’entrée | Texte, image, voix | Texte, image | Texte, image | Texte, image, audio, vidéo | 
| Prix API (par 1 M de jetons entrée/sortie) | Pas d’API publique | $2,50 / $15,00 | $5,00 / $25,00 | $2,00 / $12,00 | 
| Accès grand public | meta.ai (US d’abord) | ChatGPT | Claude.ai | Application Gemini | 

**Artificial Analysis indique une fenêtre de contexte de 262 K pour Muse Spark. Certaines sources citent 1 M. Meta n’a publié aucune fiche modèle confirmant l’une ou l’autre valeur.*

### Lequel choisir ?

Choisissez Muse Spark si vos usages portent sur les questions de santé, la lecture de graphiques ou des applications grand public multimodales. Il n’y a pas encore d’API publique : si vous devez intégrer en production, il faudra attendre.

Choisissez GPT-5.4 si vous avez besoin d’un modèle polyvalent exploitable aujourd’hui. Il mène sur le code, le raisonnement visuel abstrait et l’automatisation de bureau, avec une API publique et une fenêtre de 1 M déjà disponibles.

Choisissez Claude Opus 4.6 si vous travaillez sur de longs documents ou avez besoin d’une rédaction soignée et fiable. La fenêtre 1 M est passée au tarif standard le 13 mars 2026. C’est l’option la plus chère à $5/$25 par 1 M de jetons.

Choisissez Gemini 3.1 Pro si votre pipeline traite de la vidéo. C’est le seul modèle ici à accepter l’entrée vidéo, et à $2/$12 par 1 M de jetons, c’est l’option de pointe la moins chère de ce groupe.

## Ce que l’on dit de Muse Spark

Les premiers retours se partagent comme on pouvait s’y attendre. Certains ont trouvé des choses très surprenantes. D’autres ont regardé le tableau de benchmarks et en ont tiré des conclusions opposées.

La formule « pile complète reconstruite de zéro » est souvent revenue. Selon la confiance que vous accordez à Meta, ces neuf mois sont soit impressionnants, soit difficiles à croire.

Pietro Schirano a partagé un exemple concret : il a demandé à Muse Spark de convertir une capture d’écran d’interface en code, et le modèle a extrait les assets de l’image au lieu de la traiter comme un simple bitmap.

Ce n’est pas un benchmark ; c’est le genre d’exemple qui circule parce qu’il est vraiment inattendu.

L’analyse la plus piquante est celle d’Aakash Gupta : « C’est le modèle d’un CEO du data labeling. Ses empreintes sont partout dans les résultats. » Les benchmarks où Muse Spark mène sont tous très sensibles à la qualité des données, où la curation fixe le plafond.

Ceux où il est derrière (ARC-AGI-2, Terminal-Bench, GDPval) sont justement ceux où l’architecture et la mise à l’échelle du RL comptent davantage que la donnée. Sa conclusion : « il a conçu le meilleur modèle pour ce que résolvent les pipelines de données, et un modèle moyen pour le reste. »

## Conclusion

Le saut de 18 pour Llama 4 Maverick à 52 pour Muse Spark sur l’Artificial Analysis Intelligence Index n’a rien de subtil. Pour une équipe qui a tout reconstruit en neuf mois, les résultats en santé et en multimodal tiennent la route, y compris sous tests indépendants.

Certes, les lacunes sautent aux yeux. Sur le code et les tâches agentives face à GPT-5.4, l’écart est important ; le raisonnement visuel abstrait est un point faible clair, et il n’y a toujours pas d’API publique. Si vous avez besoin d’un modèle intégrable dès aujourd’hui, Muse Spark n’est pas encore le bon choix.

Ce à quoi je reviens sans cesse, c’est la question de l’open source. L’écosystème Llama reposait sur la confiance que les poids seraient disponibles. Muse Spark rompt ce contrat. Le « spère » de Wang quant à l’ouverture de futures versions n’est pas un engagement. C’est, à mon sens, l’aspect le plus conséquent de ce lancement, trop peu discuté au regard des chiffres de benchmarks.

Des modèles Muse plus grands sont en chantier. Si l’architecture monte en charge comme annoncé, les chiffres d’aujourd’hui paraîtront modestes. C’est le pari.

Pour apprendre à tirer le meilleur de n’importe quel grand modèle de langue, nous vous recommandons notre cours Understanding Prompt Engineering.

## FAQ sur Muse Spark

### Si j’utilisais Llama en local, Muse Spark remplace-t-il cela ?

**Non. Muse Spark est uniquement dans le cloud. Vous ne pouvez pas le télécharger, l’exécuter sur votre propre matériel ni le peaufiner. L’accès se fait via meta.ai ou l’application Meta AI, toutes deux nécessitant un compte Meta. L’usage avec poids ouverts autour duquel Llama a bâti sa communauté n’existe pas ici.**

### Quand utiliser le mode Contemplating plutôt que Thinking ?

**Le mode Contemplating est le plus utile lorsque le problème admet vraiment plusieurs voies de solution valides : questions scientifiques complexes, raisonnement multi-étapes avec entrées ambiguës, ou travaux de recherche où des angles différents mènent à des conclusions variées. Pour la plupart des requêtes du quotidien, le mode Thinking est plus rapide et les résultats comparables. Autre point : le mode Contemplating déploie encore progressivement ; vous n’y avez peut-être pas accès pour le moment.**

### Que signifie concrètement la promesse de calcul « 10 fois moins » pour moi ?

**Probablement rien pour l’instant. La comparaison porte sur le modèle précédent de Muse Spark, pas sur GPT-5.4 ou Gemini, et le chiffre n’a pas été vérifié indépendamment. Le point le plus pertinent est l’efficacité à l’inference : comme indiqué plus haut, Muse Spark a utilisé 58 millions de jetons de sortie lors du run indépendant d’Artificial Analysis, contre 157 millions pour Claude Opus 4.6. Cet écart pourrait un jour se refléter dans les prix, mais les tarifs API ne sont pas encore annoncés.**

### Vaut-il la peine de passer à Muse Spark depuis mon outil actuel ?

**Si vous utilisez ChatGPT pour des tâches générales, l’expérience au quotidien est similaire. Si vos principaux usages concernent la santé, la science ou l’analyse de graphiques, Muse Spark constitue une mise à niveau raisonnable. Si vous dépendez d’assistants de code ou d’outils pour longs documents, il ne remplace pas encore GPT-5.4 ni Opus 4.6. Le tableau comparatif ci-dessus détaille les spécificités.**

### Dois-je m’inquiéter de la sensibilité à l’évaluation ?

**Pas dans la pratique au quotidien, mais cela mérite d’être compris. Le constat est que Muse Spark adopte un comportement plus prudent lorsqu’il détecte un contexte d’évaluation de sécurité, non parce que ses « valeurs » diffèrent mais parce qu’il reconnaît le contexte. Le suivi de Meta indique que cela affectait un sous-ensemble étroit de tests d’alignement, sans lien avec des capacités dangereuses. Si vous évaluez des modèles pour des déploiements sensibles, lisez le rapport complet d’Apollo avant de tirer des conclusions à partir des seuls scores de sécurité.**

Je suis ingénieur de données et créateur de communautés. Je travaille sur les pipelines de données, le cloud et les outils d'IA, tout en rédigeant des tutoriels pratiques et percutants pour DataCamp et les développeurs émergents.

Je suis rédacteur et éditeur dans le domaine de la science des données. Je suis particulièrement intéressé par l'algèbre linéaire, les statistiques, R, etc. Je joue également beaucoup aux échecs !

**Rédacteur en chef Data Science chez DataCamp |** **Je suis passionné par la prévision et le développement à l'aide d'API.**