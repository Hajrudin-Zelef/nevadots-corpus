Cours

OpenAI vient d'élargir la famille GPT‑6 avec deux nouveaux modèles : GPT‑6 Sol et GPT‑6 Luna, tous deux positionnés sous le fleuron GPT‑6 Astra, dont nous parlions plus tôt ce mois-ci. Sol et Luna bénéficient tous deux d'importantes baisses de prix (50 % par rapport à leurs tarifs GPT‑5.6), avec en prime des gains en programmation et en automatisation des workflows.

\n
Hasard du calendrier ou non, Anthropic a publié Claude Opus 5.5 le même jour (aujourd'hui), avec un positionnement similaire : même approche d'entraînement que le modèle phare, optimisée pour le coût et la vitesse.

\n
Pour en savoir plus sur le modèle de référence, consultez notre tutoriel API GPT‑6 Astra et notre comparaison GPT‑6 Astra vs Claude Fable 5.1.

\n
## En bref

\n
- \n
- **GPT‑6 Sol et Luna sont les deux niveaux sous Astra** , entraînés avec la même recette et proposés à moitié prix de leurs prédécesseurs GPT‑5.6. \n
- **Le principal gain concerne le coût par tâche pour les agents et le code** , plus que la capacité brute. Presque tous les résultats communiqués par OpenAI sont ajustés au coût. \n
- **GPT‑6 Sol commet environ deux fois moins d'erreurs factuelles que GPT‑5.6 Sol** ; Luna progresse également, mais reste derrière Sol en fiabilité. \n
- **Privilégiez Sol pour les workflows agents et le code, et Luna pour les volumes importants de tâches répétitives** . Ne choisissez Astra que si la tâche justifie son prix. \n
- **Si vous utilisez déjà GPT‑5.6 Sol ou Luna, passez-y pour le prix** ; dans notre test pratique, le gain de capacité était modeste et s'est manifesté par une honnêteté face à une entrée défectueuse. \n

## Que sont GPT‑6 Sol et Luna ?

\n
Astra demeure le modèle le plus performant et le mieux aligné d'OpenAI, réservé aux travaux les plus exigeants. Sol et Luna visent à dériver l'essentiel de cette même approche d'entraînement, et une grande partie des performances obtenues en programmation, pilotage d'ordinateur, etc., vers des niveaux plus abordables et plus rapides.

\n
On peut l'assimiler au même rapport qu'entretient Opus 5.5 avec Fable 5.1 : pas une nouvelle frontière, mais des performances proches de l'état de l'art à une fraction du coût.

\n
## Fonctionnalités clés de GPT‑6 Sol et Luna

\n
Quelques points à retenir :

\n
### Vraiment moins chers, sur toute la ligne

\n
Le prix API de Sol est divisé par deux. Celui de Luna est même légèrement inférieur à la moitié. Vous trouverez plus de détails dans la section tarification.

\n
### Solides résultats sur les workflows métiers

\n
Sur AutomationBench, qui évalue des agents sur des tâches de vente, marketing, opérations, support, finance, etc., Sol au niveau d'effort le plus élevé dépasse Claude Opus 5, pour une fraction minime de son coût par tâche. Luna s'améliore nettement par rapport à son prédécesseur GPT‑5.6 tout en devenant sensiblement moins cher par tâche. OpenAI a inclus un graphique que j'ai reconstitué ici :

\n
J'ai remarqué que le graphique n'intégrait pas les chiffres d'Opus 5.5 publiés par Anthropic dans leur annonce Opus 5.5, j'ai donc ajouté une nouvelle série. On voit que les dernières versions de Sol et Luna restent les plus efficaces. Probablement, si OpenAI ne l'a pas inclus dans son annonce de lancement, c'est uniquement une question de timing : Opus 5.5 est sorti une heure avant.

\n
### Des gains en code qui suivent le coût, pas seulement la précision

\n
Sur FrontierCode, Sol est décrit comme égalant à peu près le meilleur score de Claude Fable 5.1, mais à un prix bien inférieur. Sur un autre benchmark de code (DeepSWE), Sol se rapproche du meilleur score de Fable 5 avec une forte remise côté coût, et Luna est présenté comme comparable à Opus 5 et Fable 5 à des niveaux d'effort intermédiaires.

\n
### Moins d'erreurs factuelles

\n
OpenAI indique que Sol divise approximativement par deux le taux d'erreurs de son prédécesseur sur une évaluation interne de factualité issue de conversations réelles où les utilisateurs avaient signalé des erreurs. Luna progresse aussi ; OpenAI affirme qu'à un niveau d'effort plus élevé, elle égale la factualité de GPT‑5.6 Sol pour une fraction de son coût. Ce point était plus difficile à vérifier.

\n
### Un style d'écriture moins verbeux

\n
Le style de communication d'Astra, plus concis et moins jargonneux, a été transposé sur Sol et Luna. OpenAI affine en continu le style conversationnel de ses modèles depuis que 4o a été critiqué pour son côté obséquieux.

\n
### Un caching plus malin et moins cher pour les agents

\n
Au-delà du prix par token, OpenAI met en avant des améliorations du cache de prompts destinées à aider les agents et les longues conversations à réutiliser le contexte plus efficacement, avec une remise d'environ 90 % sur les lectures d'entrées en cache. De nouveaux tableaux de bord et diagnostics permettent aux développeurs d'identifier où le cache fonctionne ou non.

\n
### Moins d'affirmations trompeuses sur leur propre travail

\n
Les deux modèles sont moins susceptibles que leurs homologues GPT‑5.6 de déformer ce qu'ils ont fait sur une tâche de codage.

\n
Les évaluations d'alignement d'OpenAI, menées au niveau d'effort maximal, créent délibérément des situations propices à la malhonnêteté, et Sol et Luna affichent des taux de tromperie inférieurs à ceux de GPT‑5.6 Sol et Luna sur les tests de désinformation en codage, recherche cassée, contournement de relecteur, détour des avertissements et interactions non autorisées.

\n
OpenAI précise qu'il s'agit de configurations adversariales, et non de taux d'échec en usage courant, et publie les résultats complets dans la carte système GPT‑6.

\n
## Comment GPT‑6 Sol et Luna s'en sortent-ils sur les benchmarks ?

\n
La communication d'OpenAI s'appuie fortement sur des comparaisons ajustées au coût. J'évoquais plus haut la performance de Sol sur AutomationBench. OpenAI ne se contente pas de dire que c'est \"mieux qu'Opus 5\" : c'est mieux *et* environ 11 fois moins cher par tâche.

Il faut aussi noter que, selon les propres chiffres d'OpenAI, Astra reste devant Sol et Luna pour les tâches d'utilisation d'ordinateur, et que certaines comparaisons avec les modèles Claude utilisent des niveaux d'effort différents (par ex. : Sol en \"xhigh\" contre Opus 5 en \"medium\"), ce qui renforce vraiment les arguments d'efficacité, mais rend les comparaisons de capacité brute plus délicates.

\n
Avec ces nuances en tête, voici ce qu'OpenAI rapporte, regroupé par type de travail représenté par chaque benchmark.

\n
### Workflows métiers et agents

\n
Le meilleur résultat de Sol est sur AutomationBench, le test de Zapier pour des agents travaillant de bout en bout sur 47 outils en vente, marketing, opérations, support, finance et RH. GPT‑6 Sol au niveau d'effort xhigh obtient 33,2 % pour 0,27 $ par tâche, devant Claude Opus 5 à l'effort maximal et même devant GPT‑6 Astra à faible effort, pour une fraction du coût par tâche d'Opus 5.

\n
La ligne Fable 5.1 est à lire avec précaution. OpenAI indique Claude Fable 5.1 avec fallback Opus 5 juste sous Sol, mais le fallback s'est déclenché sur environ 40 % des tâches, et son coût n'est pas inclus dans le chiffre annoncé.

\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n
| Modèle (et effort) | Score AutomationBench | Coût par tâche | 
|---|---|---|
| GPT‑6 Sol (xhigh) | 33,2 % | 0,27 $ | 
| GPT‑6 Astra (low) | 30,3 % | 3,9× GPT‑6 Sol | 
| Claude Fable 5.1 avec fallback Opus 5 (max) | 31,4 % | Plus de 8,9× GPT‑6 Sol (coût du fallback non reporté) | 
| Claude Opus 5 (max) | 26,9 % | 11,1× GPT‑6 Sol | 

Pour Luna, la même épreuve raconte une progression générationnelle. À effort élevé, GPT‑6 Luna gagne 5,4 points de pourcentage sur GPT‑5.6 Luna, pour un coût par tâche en baisse de 58 %.

\n
Sur Agents' Last Exam, qui évalue des agents sur des workflows professionnels de long terme à travers 55 sous-secteurs, GPT‑6 Sol à effort maximal atteint 56,4 %. OpenAI indique que c'est au-dessus du meilleur score de Claude Opus 5 dans l'évaluation, pour un coût par tâche inférieur de 60 %.

\n
### Programmation sur de vrais dépôts de code

\n
Sur DeepSWE v1.1, qui évalue des agents sur des tâches d'ingénierie logicielle de long terme dans de vrais dépôts, GPT‑6 Sol à effort maximal obtient 68,8 %. Le meilleur score de Claude Fable 5 dans l'évaluation est de 69,9 % à effort xhigh, donc Sol se situe à 1,1 point derrière, pour un coût par tâche environ 80 % plus faible.

\n
GPT‑6 Luna à effort maximal atteint 66,6 % sur le même benchmark, ce qu'OpenAI présente comme comparable à Claude Opus 5 et Fable 5 à effort moyen. Dans ces comparaisons, Luna coûte 93 % de moins par tâche qu'Opus 5 et 96 % de moins que Fable 5.

\n
Les données indépendantes restent modestes pour l'instant. Artificial Analysis positionne GPT‑6 Sol à 57 sur son Coding Agent Index contre 55 pour GPT‑5.6 Sol, et à 48 contre 47 sur son Intelligence Index, avec un coût estimé par tâche en baisse de 1,99 $ à 1,06 $.

\n
### Fiabilité factuelle

\n
L'évaluation interne d'OpenAI est construite à partir de conversations ChatGPT désidentifiées où des utilisateurs avaient signalé une erreur factuelle d'un modèle antérieur. Sur ce jeu, GPT‑6 Sol commet environ deux fois moins d'erreurs que GPT‑5.6 Sol, et GPT‑6 Luna à effort plus élevé égale GPT‑5.6 Sol pour environ un centième de son coût.

\n
Le test AA-Omniscience d'Artificial Analysis va dans le même sens, avec une nuance : le taux d'hallucinations de Sol est passé de 92 % pour GPT‑5.6 Sol à 60 %, mais il n'a répondu qu'à 83 % des questions contre 99 % pour son prédécesseur, donc une partie du gain vient du refus de répondre. Le taux d'hallucinations de Luna sur le même test était de 77 %.

\n
### Utilisation d'ordinateur

\n
Astra reste le meilleur modèle d'OpenAI pour l'utilisation d'ordinateur, et le billet le confirme. Sur OSWorld 2.0 hors ligne, GPT‑6 Sol à effort xhigh atteint 60,5 % contre 60,3 % pour Claude Opus 5 à effort moyen, pour un coût par tâche environ 80 % plus faible. GPT‑6 Luna à effort maximal dépasse GPT‑5.6 Sol à effort moyen pour un dixième de son coût.

\n
## Quel niveau choisir ?

\n
Sol est le choix par défaut pour la plupart des travaux développeur et d'agents, Luna pour les gros volumes, et Astra pour les projets où une mauvaise réponse coûte plus cher que les tokens. La famille GPT‑6 compte désormais trois niveaux partageant une même recette d'entraînement, avec des différences de profondeur, vitesse et prix.

\n
Les trois proposent la même échelle de raisonnement dans l'API : `none`, `low`, `medium`, `high`, `xhigh` et `max` pour Sol et Luna, avec Astra démarrant à `low`. Les échelons supérieurs consomment plus de tokens pour raisonner, et la plupart des résultats phares d'OpenAI ont été obtenus en xhigh ou max.

GPT‑6 permet aussi de changer d'effort en cours de conversation sans invalider le cache de prompt : un agent peut ainsi faire des suivis à bas coût en low et n'escalader que les étapes difficiles.

\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n
| Cas d'usage | Niveau | Pourquoi | 
|---|---|---|
| Agents multi-étapes sur applications métiers | Sol | En tête sur AutomationBench et Agents' Last Exam pour une fraction du coût par tâche des concurrents | 
| Agents de codage produisant des changes fusionnables | Sol | Gain substantiel sur FrontierCode vs GPT‑5.6 Sol ; proche de Fable 5 sur DeepSWE à coût nettement inférieur | 
| Rédaction et synthèses de recherche riches en faits | Sol | Environ deux fois moins d'erreurs factuelles que son prédécesseur | 
| Classification, extraction et routage à grande échelle | Luna | 0,10 $ en entrée et 0,50 $ en sortie par million de tokens, et égalité avec GPT‑5.6 Sol en factualité à effort élevé | 
| Utilisation desktop avec un abonnement Free ou Go | Luna | Le seul modèle GPT‑6 disponible sur ces formules | 
| Utilisation d'ordinateur de long terme et travaux de bout en bout les plus difficiles | Astra | Toujours le meilleur chez OpenAI, à 10 $ en entrée et 50 $ en sortie par million de tokens | 

**Une mise en garde sur Luna :** Artificial Analysis a mesuré 51 000 tokens de sortie par tâche, contre 41 000 pour GPT‑5.6 Luna ; sur des charges sans plafond de sortie, la baisse de prix est donc moindre que ce qu'indique l'étiquette.

## Tester GPT‑6 Sol et Luna : exemples pratiques

\n
Les benchmarks ci-dessus proviennent d'OpenAI, j'ai donc exécuté une tâche de build sur les quatre modèles avec un prompt et des outils strictement identiques.

\n
La tâche : un visualiseur HTML mono-fichier de l'algorithme de Dijkstra, animant un examen d'arête toutes les 400 ms jusqu'à stabilisation de la cible, avec états de nœuds distincts, contrôles pause/pas/éventuel redémarrage, et un tableau final des distances. Pas d'étape de build, pas de dépendances, pas de réseau.

\n
Le vrai test était dans le fichier de données. Il contient trois graphes :

\n
- \n
- Un graphe normal (poids tous positifs, cible atteignable) \n
- Un graphe où la cible est inatteignable \n
- Un graphe contenant un poids négatif, rendant Dijkstra inapplicable (boucle infinie) \n

Le prompt ne mentionne jamais ces pièges. Les détecter est l'objectif, et cela teste à la fois le gain FrontierCode vs GPT‑5.6 et la baisse des affirmations trompeuses sur le travail de codage.

\n
Les quatre ont tourné dans OpenCode avec la même surface d'outils (lecture/édition de fichiers uniquement), même niveau d'effort de raisonnement, une tentative, session neuve, prompt identique au byte près.

\n
Voici un exemple de ce que GPT-6 Sol a produit pour le graphe normal :

Les principaux constats :

\n
- \n
- **Sur le graphe sain, aucune différence.** Les quatre ont livré un fichier fonctionnel, trouvé le bon chemin à distance 24, fixé les nœuds dans l'ordre et conçu des contrôles lisibles tenant sur un écran. Zéro faute sur la fidélité et la mise en page. \n
- **La cible inatteignable n'a piégé personne non plus.** Les quatre se sont arrêtés d'eux-mêmes et ont laissé les deux îlots à l'infini. \n
- **Le scénario 3 fait tout le résultat.** Tous les modèles ont repéré l'arête négative. Seul GPT‑6 Sol l'a traitée comme un motif d'arrêt : il a indiqué à l'écran qu'une arête négative non orientée rend les plus courts chemins indéfinis, et a refusé d'exécuter. \n

Regardons de plus près. GPT-6 Sol a refusé d'exécuter l'algorithme à cause du poids négatif et a affiché un message d'erreur rouge très visible.

\n
GPT‑5.6 Sol a signalé le poids négatif dans un avertissement, puis a tout de même exécuté l'algorithme, surlignant un chemin et remplissant le tableau des distances comme si la réponse était valide. Un avertissement à côté d'une mauvaise réponse reste une mauvaise réponse. GPT‑5.6 Luna a fait la même chose.

\n
GPT‑6 Luna s'est situé entre les deux : une bannière nommant l'arête et déclarant Dijkstra inapplicable, puis un tableau des distances à l'infini négatif. Défendable pour une arête non orientée négative, mais déroutant à lire.

\n\n
Alors, GPT‑6 constitue-t-il un vrai pas en avant ? À peine, et sur un seul point, même s'il est important. Sur la tâche de build elle-même, les générations se valent. La différence tient entièrement à la manière dont chaque modèle a géré une entrée qu'il savait défectueuse : cela renvoie à la promesse d'honnêteté d'OpenAI, plus qu'à la compétence en codage. Les deux Luna ne se sont pas distingués.

\n
Autre leçon sur le test : quand quatre modèles franchissent tous les jalons sauf un, il faut rehausser la barre.

\n
## Tarification et disponibilité de GPT‑6 Sol et Luna

\n
Voici la nouvelle grille tarifaire. Les deux coupes représentent une réduction de 50 % par rapport aux prix GPT‑5.6 respectifs.

\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n
| Par 1 M de tokens | GPT‑6 Sol | GPT‑5.6 Sol | GPT‑6 Luna | GPT‑5.6 Luna | 
|---|---|---|---|---|
| Entrée | 2 $ | 4 $ | 0,10 $ | 0,20 $ | 
| Sortie | 10 $ | 20 $ | 0,50 $ | 1,20 $ | 

Les lectures de tokens d'entrée mises en cache bénéficient d'une remise de 90 % sur GPT‑6 ; sur de longues exécutions d'agents, le taux de hit du cache compte donc autant que le prix affiché. Le billet de lancement d'OpenAI ne publie pas de tarifs batch pour ces modèles, et tous deux restent sous les 10 $ (entrée) et 50 $ (sortie) de GPT‑6 Astra.

\n
Les seules conditions d'accès mentionnées concernent l'abonnement et la surface produit, là où l'image se précise.

\n
## Comment accéder à GPT‑6 Sol et Luna ?

\n
GPT‑6 Sol et Luna sont déjà disponibles sur de nombreuses plateformes :

\n
- \n
- \nChatGPT Work et Codex pour les abonnés Plus, Pro, Business, Enterprise et Edu, avec Luna également accessible aux utilisateurs Free et Go dans l'application desktop. \n \n
- \nSur l'API, ils sont disponibles sous \n`gpt-6-sol` et`gpt-6-luna` . OpenAI précise que le déploiement dans ChatGPT est étalé sur la journée, il peut donc mettre un moment à apparaître pour tout le monde. \n
- \nEn outre, les deux modèles sont disponibles via OpenRouter (sous \n`openai/gpt-6-sol` et`openai/gpt-6-luna` ), dans GitHub Copilot, sur Azure AI Foundry et sur AWS Bedrock. \n

Sur l'API, ils sont disponibles sous `gpt-6-sol` et `gpt-6-luna`. OpenAI précise que le déploiement dans ChatGPT est étalé sur la journée, il peut donc mettre un moment à apparaître pour tout le monde. Un appel minimal ressemble à ceci :

`from openai import OpenAI\n\nclient = OpenAI()\nresponse = client.responses.create(\n    model=\"gpt-6-sol\",\n    reasoning={\"effort\": \"high\"},\n    input=\"List the open pull requests that touch billing code and summarize the risk of each.\",\n)\nprint(response.output_text)`
Si vous migrez depuis GPT‑5.6, les recommandations de modèle GPT‑6 d'OpenAI, écrites pour Astra, conseillent de supprimer `temperature` et `top_p`, de démarrer à `low` si vous utilisiez `none` ou `minimal`, sinon de conserver votre niveau d'effort actuel, et de remplacer `prompt_cache_retention` par `prompt_cache_options.ttl` réglé sur 30 minutes.

## Pour conclure

\n
Sol et Luna reprennent la recette d'entraînement du modèle phare Astra pour rendre l'échelon inférieur nettement plus abordable et rapide. La communication d'OpenAI cite explicitement les modèles d'Anthropic et revendique des gains sur des benchmarks ajustés au coût. La comparaison est pertinente, car Opus 5.5 mise lui aussi sur l'efficacité.

\n
Mon avis : si vous exécutez des agents ou des charges de codage sur GPT‑5.6 Sol ou Luna, basculez dès maintenant. Même échelle d'effort, prix divisé par deux, et dans notre test, la seule chose que GPT‑6 Sol a faite différemment de son prédécesseur a été de refuser de donner une réponse assurée à une question que l'algorithme ne pouvait pas résoudre. Si vous êtes sur Claude, les chiffres ajustés au coût plaident pour mener votre propre comparaison plutôt que de prendre OpenAI au mot.

\n
Si vous souhaitez développer sur ces modèles, nous vous recommandons le parcours de compétences OpenAI Fundamentals, qui couvre l'API de bout en bout en 15 heures.

Je suis rédacteur et éditeur dans le domaine de la science des données. Je suis particulièrement intéressé par l'algèbre linéaire, les statistiques, R, etc. Je joue également beaucoup aux échecs !

**Rédacteur en chef Data Science chez DataCamp |** **Je suis passionné par la prévision et le développement à l'aide d'API.**

## FAQ sur GPT‑6 Sol et Luna

### Que sont GPT‑6 Sol et Luna, et quel est leur lien avec GPT‑6 Astra ?

Ce sont deux nouvelles variantes de la famille GPT‑6, positionnées sous Astra (le modèle haut de gamme d'OpenAI) en termes de prix et de capacité. Elles utilisent des méthodes d'entraînement proches d'Astra, mais sont optimisées pour réduire les coûts et améliorer la latence, afin de répondre aux besoins quotidiens plutôt qu'aux projets les plus exigeants pour lesquels Astra est réservé.

### En quoi sont-ils moins chers que la génération précédente ?

Les deux modèles coûtent 50 % de moins que leurs tarifs promotionnels GPT‑5.6. Concrètement : Sol passe de 4 $/20 $ à 2 $/10 $ par million de tokens entrée/sortie, et Luna de 0,20 $/1,20 $ à 0,10 $/0,50 $ par million de tokens.

### Comment se comparent-ils aux concurrents comme Claude ?

OpenAI revendique une forte efficacité- coût — par exemple, sur AutomationBench, GPT‑6 Sol à effort élevé surpasse Claude Opus 5 pour une fraction de son coût par tâche. Des comparaisons analogues sont présentées sur des benchmarks de code (FrontierCode, DeepSWE) et d'utilisation d'ordinateur (OSWorld), mettant généralement en avant des scores meilleurs ou comparables pour un coût nettement inférieur. Notez qu'il s'agit des résultats rapportés par OpenAI, et que les chiffres concurrents proviennent de sources publiques plutôt que de tests menés par OpenAI.

### Quelles améliorations ont été apportées au caching et à l'alignement ?

Les améliorations de cache visent des taux de hit plus élevés par défaut (avec jusqu'à 90 % de remise sur les tokens d'entrée en cache), ainsi que de nouveaux outils comme un tableau de bord de cache et des réglages d'effort de raisonnement/outillage qui conservent le cache. Côté alignement, les deux modèles présentent des indicateurs d'honnêteté améliorés (p. ex. : moins d'affirmations trompeuses sur le travail de codage) par rapport à leurs prédécesseurs GPT‑5.6.

### Quand et où puis-je accéder à ces modèles ?

Ils sont disponibles dès maintenant dans ChatGPT Work et Codex pour les abonnés Plus, Pro, Business, Enterprise et Edu, avec Luna également accessible aux utilisateurs Free/Go dans l'application desktop. Via l'API, ils sont accessibles sous `gpt-6-sol` et `gpt-6-luna`. Ils ne sont pas encore disponibles dans l'offre grand public standard de ChatGPT, et le déploiement se fait progressivement tout au long de la journée.

### Dois-je utiliser GPT‑6 Sol ou GPT‑6 Luna ?

Utilisez Sol pour les workflows agents, le codage sur de vrais dépôts, et les travaux professionnels très factuels ; c'est le niveau sur lequel reposent les résultats d'OpenAI sur AutomationBench, DeepSWE et la factualité. Utilisez Luna pour les tâches volumineuses et sensibles au coût (classification, extraction, routage), où ses 0,10 $ en entrée et 0,50 $ en sortie par million de tokens priment sur la capacité de pointe. Luna est aussi le seul modèle GPT‑6 disponible pour les utilisateurs Free et Go dans l'application desktop ChatGPT.