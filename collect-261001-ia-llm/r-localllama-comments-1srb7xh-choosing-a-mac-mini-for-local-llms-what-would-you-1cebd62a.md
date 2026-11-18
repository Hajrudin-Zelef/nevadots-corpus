Merci pour ton avis !
Explique-nous pourquoi ce contenu n’est pas utile.
      Choisir un Mac Mini pour des LLM locaux — qu'est-ce que VOUS achèteriez vraiment ? 
        
        
        
    
    
    J'ai trois options sur mon radar et je n'arrive vraiment pas à me décider. Je ne cherche pas des fiches techniques — je veux entendre des gens qui utilisent vraiment ces trucs au quotidien :
M4 (32 Go) — le plus récent mais apparemment le plus lent des trois pour l'inférence ?
M2 Pro (32 Go) — j'ai entendu dire qu'il bat en fait le M4 de base sur le tok/s
M1 Max (64 Go) — le plus ancien mais avec la plus grande bande passante de mémoire
Je fais tourner Ollama, des assistants de code (Qwen/Kimi), peut-être quelques pipelines RAG. Le budget est de 2 à 3k $, donc je ne suis pas totalement bloqué en termes d'options. Et ouais, évidemment, openclaw pour arrêter de débourser sur des modèles fermés.
Le gros truc qui me retient : il y a de fortes rumeurs qu'Apple va sortir un Mac Mini M5 et un Mac Studio M5 autour de la WWDC 2026. Apparemment, le stock des modèles actuels s'épuise déjà (délais d'attente de 4 à 5 mois pour certaines configurations). Donc, dois-je me lancer maintenant ou attendre encore quelques mois ?
Qu'est-ce que tu utilises ? Et si tu devais acheter aujourd'hui, attendrais-tu le M5 ou prendrais-tu simplement le M4 Pro 48 Go et te mettrais au travail ?
Section des commentaires
merci à tous, j'ai beaucoup appris :
éviter M1/M2/base M4, la bande passante est une impasse
64 Go en minimum, 96-128 Go pour du vrai travail -- l'inférence de claw nécessite
TB5 est important pour le cluster futur - j'ai appris quelque chose de nouveau
le GPU gagne en vitesse mais le Mac gagne en puissance et en taille de modèle - le GPU semble être un $$ gaspillé en termes de puissance
la RAM nous permet de charger des modèles plus grands, la bande passante les rend rapides — nous avons besoin des deux
M5 Studio pourrait valoir le coup d'attendre
j'apprécie tous les retours 🙏
puis-tu expliquer le clustering TB5 ? C'est nouveau pour moi.
Fondamentalement, avec le TB5, tu peux connecter plusieurs Macs ensemble et ils agissent comme une seule machine pour exécuter des modèles. Donc, tu pourrais commencer avec un M4 Pro Mini et en ajouter un autre plus tard pour doubler ta RAM/ton calcul. Les anciens chips n'ont pas le TB5, donc tout ce que tu achètes est tout ce que tu auras pour toujours.
as-tu des idées sur quand Apple sortira le M5 Studio ? merci !
aucune idée mais tout ce qu'on entend ce sont des rumeurs disant quelque part vers juillet
Si ton cas d'utilisation est le codage agentique ou tout ce qui est lourd en outils, privilégie la mémoire unifiée plutôt que les cœurs bruts. 64 Go est le plancher pratique ; 32 Go te permet d'accéder à un Q4 d'un 30B et pas beaucoup de marge pour le contexte + OS + IDE. Un Mac mini avec 64 Go offre le meilleur rapport qualité/prix pour rester local sur des modèles de classe 30-35B avec un contexte décent. Si tu peux attendre le prochain rafraîchissement M5, le saut de bande passante est le véritable upgrade.
Voudrais-tu acheter studio ou mini ? si tu avais le choix ?
studio, sans aucun doute. En ce moment, avec les prix, tu as très peu d'options à 2-3k. La plupart des gens qui achètent un mac mini pour des homards ne l'utilisent pas avec un modèle local, tu vas attendre toute la journée pour une seule tâche sur la bande passante du Mini. Mais les clusters TB5 fonctionnent pour les macs, donc plus tard quand tu auras envie, et tu l'auras sûrement, tu pourras utiliser un M4 pro mini avec des machines plus puissantes comme un cluster. Les anciens chips/modèles n'ont pas de Tb5, donc peu importe ce qu'ils ont en termes de ram et de calcul, ça va être tout pour leur fonctionnalité.
Je n'avais pas pensé à combien le contexte + le système d'exploitation + la surcharge de l'EDI grignote ce qui est réellement disponible pour le modèle. Merci pour cela et d'autres éléments à prendre en compte.
Je possède un Mac Mini m2 Pro avec 16 Go de RAM et je l'adore, mais ce que tout le monde dit est vrai. Vous pouvez jouer avec de petits modèles (j'en ai) et vous habituer à la façon dont tout fonctionne, c'est sûr. Mais si vous voulez aller au-delà des démos mignonnes, vous aurez besoin d'un minimum de 64 Go de RAM, et de 128 Go de RAM de préférence.
Avec 128 Go de RAM, vous pouvez avoir un modèle considérable avec un grand contexte et même faire fonctionner quelques petits modèles aussi (le modèle plus grand délègue des tâches simples aux plus petits).
Je ne peux que rêver de faire de telles choses jusqu'à ce que j'obtienne un Mac de 128 Go. J'attends le Studio m5 qui aura un avantage significatif par rapport aux générations précédentes grâce aux Accélérateurs Neuraux intégrés au GPU (multiplication de matrices intégrée au matériel) qui accélère le traitement des prompts.
Vous pouvez absolument utiliser un Mac Mini m2 Pro pour apprendre. Mais finalement, vous voudrez 64 Go comme minimum absolu, sinon 128 Go.
merci ouais je voulais me lancer dans des modèles locaux mais on dirait que je vais juste rencontrer un mur assez rapidement avec quoi que ce soit sous 64 Go. je suppose que je vais devoir attendre le M5 alors. c'est tellement déroutant avec beaucoup de $$ en ligne.
Ce n'est pas un Mac Mini. Jamais.
Écoutez, je suis un gars de Mac. Mais je ne les utiliserais tout simplement pas. Vous pouvez? Bien sûr... mais ils ne sont pas rapides à cause de leur faible bande passante mémoire. C'est le problème avec le battage médiatique autour de Mac - ils ont donné aux gens des informations totalement fausses.
Les gens demandent pourquoi j'insiste sur les GPU et pas sur les Mac Studios/Mac minis ? Oui, vous pouvez acheter un Mac Studio Ultra M3 avec 512 Go de mémoire unifiée et charger un modèle massif et faire cracher des tokens à 2 par seconde.
Super pas utile à moins que vous vouliez attendre une éternité.
Vous avez déjà remarqué comment les escrocs de Mac parlent toujours de faire fonctionner des modèles locaux toute la nuit. Ouais, parce qu'ils sont lents. Personne ne va attendre 8 heures qu'un composant soit mis à jour par leur agent.
Un Mac Mini vs un 3090
Le RTX 3090 est nettement plus rapide, de 20 à 40 % de tps en plus.
- Nemotron-3-Nano 4B : RTX 3090 = 187 tok/s contre Mac Mini M4 = 25 tok/s
- General 7B–13B ou petit 33B Q4/Q5 : le build 3090 gagne de 20 à 40 %.
- Qwen3-30B (ancien M3 Ultra contre 3090) : le 3090 a été mieux en génération de tokens dans la plupart des tests.
Mac Studio M4 Max = 65 tps contre le beaucoup plus rapide RTX 5090 à 240 tps !!!
TLDR :
Ceci est régulièrement cité comme le principal limiteur pour Apple Silicon en inférence :
- Mac Mini M4 (de base) : 120 Go/s <--- aussi lent qu'une bouse !
- Mac Mini M4 Pro : 273 Go/s <--- encore plus lent qu'un 3090 !!!
- Mac Mini M4 Max / Studio : jusqu'à 546 Go/s
- RTX 3090 : 936 Go/s (GDDR6X)
- Pour contextualiser, le nouveau RTX 5090 atteint 1,792 Go/s.
Avoir beaucoup de mémoire signifie juste que vous pouvez charger un MODÈLE PLUS GRAND. MAIS ÇA, ça ne veut pas toujours dire MIEUX. La bande passante mémoire signifie une génération plus rapide. Et toute personne utilisant ces matériels régulièrement et quotidiennement et souhaitant remplacer les modèles de pointe et arrêter les abonnements, a besoin de VITESSE plus que de mémoire.
Imaginez si ChatGPT ou Claude prenaient 20 minutes pour générer une réponse à chaque fois.
Tu es en train de passer à côté de la vitesse de traitement des invites. Tout avant M5 aura des vitesses de pré-remplissage/PP glaciales par rapport à un GPU 40xx ou 50xx.
Ceci dit, j'ai aussi une machine à RAM unifiée et je m'en tiens principalement aux MOEs. Qwen 35B et Gemma 26B, ou Qwen Next 80B si je peux me permettre la RAM.
À mon avis, ça ne vaut pas la peine d'acheter un Mac de 32 Go, surtout si vous voulez coder dessus. Un PC avec une carte dédiée de 32 Go + 16 Go de RAM pourra faire fonctionner votre IDE local confortablement et avoir un bon contexte (je fais tourner qwen3.6 en quant 4-bit avec un contexte de 260K et il reste encore un peu de marge). Mais à 64 Go de RAM, les choses changent.
32 Go, ce n'est pas suffisant en 2026 si tu t'intéresses aux LLM
d'accord et c'est là où j'ai atterri
64 Go minimum, 96 à 128 Go pour un vrai travail -- besoin d'inférence par clause
Et si mon cas d'utilisation n'est pas la programmation ? Mais plutôt comme un assistant/employé dans mon job de PM ? Est-ce que le M1 Mini avec 16 Go de RAM est suffisant pour un modèle comme Qwen 3.5-9B ?
D'après ce que je comprends, oui, les modèles quantifiés peuvent fonctionner pour votre cas d'utilisation, mais plus de RAM est toujours bon et vous permet de jouer un peu jusqu'à ce que vous trouviez la bonne combinaison.
Si tu achètes quelque chose exprès pour llm, alors c'est soit 64 Go, soit 128 Go. Il n'y a pas d'autre moyen
La RAM est définitivement la priorité ici. Si tu peux obtenir le modèle 64 Go M1 Max, c'est le bon choix pour les modèles plus grands, même si l'efficacité du M4 est tentante. Pour les pipelines RAG et les assistants de codage, tu vas atteindre le plafond de mémoire bien avant la vitesse de la puce.
La bande passante de mémoire sur les puces Max fait une énorme différence pour les tokens par seconde. Comme tu utilises déjà OpenClaw pour éviter la taxe API, tu vas apprécier la vitesse. Concernant les rumeurs sur le M5, elles circulent toujours. Les M4 sont déjà de véritables bêtes. Prends la meilleure RAM que tu peux te permettre maintenant et mets-toi au travail.
Voici mes appareils
M3 max 96 Go M4 mini 24 Go M5 air 16 Go
Le seul qui peut vraiment faire tourner des modèles locaux de manière productive est le M3, la RAM est le facteur le plus important.
Un mini avec 64 Go de RAM est un point de départ, mais il est limité dans ce qui peut être exécuté efficacement.
Les vitesses à cœur unique se sont beaucoup améliorées M3 2724 M4 3432 M5 4167
Les vitesses des disques se sont également beaucoup améliorées.
Je considérerais de regrouper des Mac mini à l'avenir, c'est une façon d'augmenter progressivement les choses.
merci, donc en réalité le M3 Max 96 Go est le seul des trois à vraiment avoir du poids pour l'inférence alors
M1 Mac Mini 2020, 16 Go de RAM. Tu es limité aux modèles de milieu de gamme, mais honnêtement, juste en débutant, tu as juste besoin d'un modèle sur lequel tu peux compter.
Quand tu arriveras au point où tu auras atteint les limites de la machine, les M5 seront déjà une génération en arrière. Et en plus, ils sont bon marché.
C'est un exercice d'équilibre difficile entre la capacité de VRAM et la bande passante de la mémoire. Bien sûr, les GPU sont incroyablement rapides, mais sur le marché actuel, un RTX 5090 coûte environ 4 000 € et vous laisse seulement 32 Go de VRAM.
Si vous visez des modèles denses de 27B ou des MoEs de 30B, vous avez besoin de plus d'espace. Si vous ne pouvez pas faire entrer l'ensemble du modèle, les poids et le cache KV dans la VRAM, votre performance va chuter immédiatement. Bien sûr, vous pourriez brancher quatre 5090 et devenir pro... :D mais alors vous vous retrouvez avec une consommation d'énergie et une chaleur incroyables.
C'est pourquoi j'ai trouvé que le M4 Pro Mac Mini avec 64 Go de RAM était le compromis ultime. Bien que sa bande passante de 273 Go/s ne soit pas à la hauteur d'un GPU discret de haut niveau, c'est amplement suffisant pour une inférence fluide. Vous pouvez charger confortablement des modèles plus grands avec une fenêtre de contexte décente tout en consommant une puissance ridicule de 40W. Même à 15-20 t/s, vous pouvez le faire tourner 24h/24 et 7j/7 sans vous soucier de la facture d'électricité.
C'est la conclusion à laquelle je suis arrivé après avoir pesé les options. J'attends actuellement les prix du M5 Pro, mais le M4 Pro est déjà un monstre pour cela.
Concernant les modèles M1/M2 mentionnés dans le fil : gardez à l'esprit que les versions de base/Pro de ces puces ont une bande passante significativement plus faible. Même avec plus de RAM, vous pourriez voir une génération de tokens beaucoup plus lente par rapport à l'architecture du M4 Pro.
J'attendrais jusqu'à la WWDC