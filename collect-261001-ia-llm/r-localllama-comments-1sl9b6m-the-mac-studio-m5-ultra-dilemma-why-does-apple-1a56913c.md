Merci pour ton avis !
Explique-nous pourquoi ce contenu n’est pas utile.
      Le dilemme du Mac Studio M5 Ultra : pourquoi Apple rend-il les niveaux de mémoire si gênants pour LLM 
        
        
        
    
    
    
      Je suis un développeur intensif guidé par l'IA qui vit essentiellement dans mon IDE. Je viens de tester le nouveau M14 Pro (M5 Max) avec 128 Go de RAM, et honnêtement ? Il atteint à peine le "minimum syndical" pour mon flux de travail. Je faisais tourner qwen-coder-next:80b en Q4, et bien que la vitesse de génération était correcte, le traitement des préremplissages/indications ressemblait à regarder de la peinture sécher. J'ai payé environ 5 800 $ pour cette configuration Max, et j'ai fini par la retourner. Ce n'est tout simplement pas suffisant.
    
Maintenant, je regarde le futur Mac Studio. D'après les prix précédents, le modèle de base M5 Ultra sera probablement autour de 4 600 $. Mais voici le hic : l'Ultra de base est équipé de 96 Go. C’est la définition de "inutile mais cher". 96 Go c'est une sentence de mort pour tout ce qui dépasse 70B si vous voulez vraiment travailler pendant que le modèle fonctionne.
Si je passe à 256 Go, Apple va probablement me taxer encore 2 000 $ de plus. Ça semble être un énorme surplus, mais comme il n'y a pas de niveau 128 Go ou 192 Go pour l'Ultra, je suis coincé entre l'enclume et le marteau. C'est frustrant parce qu'un Ultra de base devrait être le meilleur compromis, mais la gestion de la mémoire par Apple fait que le Max haute gamme a l'air mieux que l'Ultra d’entrée de gamme, ce qui est juste bizarre.
Quelques questions pour les légendes ici :
- 
      Des fuites "fais-moi confiance, mec" sur les véritables niveaux de mémoire pour le M5 Ultra ? Y a-t-il un espoir pour un niveau intermédiaire de 128 Go ou 128 Go+ ?
- 
      Alternatives matérielles locales ? J'ai regardé Nvidia, mais c'est un vrai casse-tête. Les P40 et V100 sont de l'histoire ancienne. Même une configuration 3090/4090 nécessite 3 cartes pour rivaliser avec la VRAM du Mac, et à ce stade, le coût est pratiquement le même que celui du Mac, mais avec le "bonus" supplémentaire d'une facture d'électricité énorme et d'une pièce qui ressemble à un sauna.
- 
      Je suis dans l'écosystème Mac depuis plus de 15 ans - c'est une dépendance à ce stade. Comment puis-je obtenir des "tokens infinis" (ou au moins une expérience utilisable de 70B+) sans vendre un rein pour 256 Go de mémoire unifiée ?
Section des commentaires
Mais voilà le truc
On peut toujours dire quand les gens utilisent un LLM pour réfléchir à leur place lorsque le résultat est un modèle dense de 70 milliards.
et honnêtement ?
Les blagues mises à part, j'ai remarqué que j'ai commencé à utiliser ce langage aussi. Le pire ? Je pense que les LLM influencent mon style d'écriture.
Ce n'est pas juste de la mimique, c'est de l'évolution.
Mon PTSD déclenché, merci
Essayez oMLX. La persistance du cache KV sur disque fait une énorme différence. La plupart des tours se terminent avec en gros pas de temps de pré-remplissage.
Je devrais rédiger un petit guide…
Édition : Petit guide rédigé ! https://www.reddit.com/r/oMLX/comments/1slcfit/local_inference_on_apple_silicon_with_subsecond/
Est-ce que ce n'est pas un peu ce que font les checkpoints et les slots dans lcpp, ou est-ce que c'est quelque chose de différent ? Si c'est différent, y a-t-il quelque chose de similaire qui utilise lcpp ?
Un peu ? llama.cpp pré-alloue un nombre fixe de "slots" au démarrage. Chacun est un contexte d'inférence isolé avec son propre cache KV. Le slot 0 et le slot 1 peuvent exécuter exactement la même invite système et chacun maintiendra sa propre copie. Le nombre de slots est fixe au démarrage, la mémoire évolue avec le nombre de slots × la longueur du contexte, et la persistance se fait via une sauvegarde/restauration explicite des checkpoints. Vous déterminez manuellement si vous souhaitez sérialiser l'état KV d'un slot sur le disque et le recharger plus tard. Pensez à chaque slot comme à une ligne téléphonique dédiée avec sa propre mémoire et un bouton "pause/reprendre".
oMLX adopte l'approche vLLM/SGLang adaptée pour la mémoire unifiée d'Apple Silicon : un arbre préfixe partagé de blocs de cache KV où les requêtes entrantes correspondent automatiquement à des préfixes déjà calculés et ne paient que pour le suffixe nouveau. Cinq requêtes partageant une invite système le calculent une fois ; la mémoire évolue avec un contenu unique, pas avec le parallélisme. Les pages du cache entrent et sortent du NVMe selon les besoins, survivent aux redémarrages et gèrent automatiquement l'éviction par niveaux chaud/froid. En gros, il n'y a pas de capacité à faire des checkpoints manuellement.
En résumé : les slots de llama.cpp sont explicites, isolés et potentiellement persistants ; le cache de préfixe d'oMLX est automatique, partagé et sauvegardé sur disque. Pour un fan-out multi-agent avec des invites partagées, oMLX est plus efficace en mémoire. Pour "je veux une session longue durée que je peux geler et dégeler à la main", les slots de llama.cpp sont plus simples.
Edit : De mon point de vue, llama.cpp me donne plus, comme un outil pour construire un harnais autour du comportement explicite que je veux (recherche/chargement automatique des slots). oMLX est plus "piles incluses" et essaie en partie d'être un harnais pour le faire à votre place sans que vous ayez à configurer quoi que ce soit de spécial.
Faites fonctionner le modèle sur le studio 128 Go et travaillez à partir d'un MacBook 64 Go = profit (pour Apple).
Je pense que choisir 128 Go comme point idéal est un peu arbitraire.
Même pour votre flux de travail actuel, qwen-coder-next 80b est bien, mais il est surpassé par le plus petit qwen 3.5 27b dans la plupart des benchmarks. Donc, une perspective pourrait être que 96 Go est parfaitement acceptable, le décrire comme "inutile mais cher" est un peu absurde.
D'un autre côté, même si vous obtenez une machine de 128 Go, il est inévitable qu'il y aura des moments où vous regretterez de ne pas avoir 256 Go. Par exemple, pour MiniMax-M2.7 (un grand bond par rapport à qwen-coder-next, beaucoup plus proche de quelque chose comme Claude Sonnet 3.6), il peut *techniquement* tenir dans 108 Go à la quantification Q4, mais vous n'aurez pas beaucoup de marge pour une longueur de contexte plus longue ou d'autres charges des applications système. Et qui sait, peut-être que le prochain modèle ouvert de pointe sera dans la fourchette de 400b paramètres.
Je ne m'inquiéterais pas trop à ce sujet, le domaine change si rapidement qu'il n'y a pas moyen de prédire quelle quantité de mémoire est la plus optimale. Prenez juste autant de mémoire que vous pouvez vous permettre, et priorisez la bande passante mémoire (c'est-à-dire Ultra > Max) afin que vous puissiez avoir des vitesses de génération rapides peu importe le modèle que vous choisissez.
Cela dépend. Si tu aimes Sonnet/Opus et que tu utilises Claude Code, Minimax 2.7 est terrible pour le travail de codage comparé à qwen-coder-next.
La formation de Minimax est pleine de :
appels de sonde
lectures redondantes
boucles de lecture → analyse → re-lecture
traces synthétiques lourdes en JSON
schémas d'outils hallucinés
appels “glob” (
read_file("*"))
boucles de réessai
“réflexion” auto-référentielle entre les appels
Le fait que tout soit globé signifie que ça se perd et peut potentiellement faire des choses dangereuses - surtout dans le claude-code. Une fois que tu commences à écrire un harness d'agent pour gérer cela, tu réalises que tu as un problème infini d'un modèle à un trillion de paramètres à gérer.
Il ignore les invites système, ignore les invites de claude-code, il émet des appels d'outil quand on lui dit de ne pas le faire, il réessaye les mêmes appels plein de fois, il continue de sonder l'environnement et de globé et globé et globé et globé et d'halluciner des outils manquants... tu traces ses outils et tu vois tous les noms d'outils décoys et tu réalises qu'ils ont entraîné leurs paramètres d'outils sur une tonne de déchets distillés sans autre direction que ILS veulent que minimax soit l'agent - pas ton IDE/Claude Code/Cursor/OpenCode.
Pour le codage, qwen3-coder-next est meilleur que Qwen3.5 27b principalement parce que 27b n'est pas configuré pour les appels d'outils agentiques. 27b est génial pour discuter de code, mais pas nécessairement pour prospérer dans Claude Code / Open Code
Blackwell a6000x2 fait ressentir les Macs comme bon marché
On peut essayer quelque chose comme le Strix Halo, ça pourrait t'y amener....
C'est presque comme si l'IA n'était pas vraiment fonctionnelle étant donné les contraintes actuelles en matière d'ordinateurs et d'électricité, choquant, je sais.
Turboquant pourrait aider, qui sait.
3a. Mac a toujours été à prix premium, pour être honnête, c'est l'option la plus intéressante pour ton argent maintenant, ce qui est fou, pour dire le moins.... c'est un problème qui te concerne
3b. Tu peux essayer de trouver quelqu'un pour souder la mémoire pour toi ou utiliser une autre machine pour l'IA tournant sous Linux avec les Blackwells ci-dessus ou le Strix Halo et utiliser un MacBook Air ou un équivalent comme ta machine de travail..
Si tu veux vraiment de bonnes réponses et de l'aide pour ça, tu devrais utiliser les forums Level1Techs, ils sont beaucoup mieux adaptés aux trucs pro-consommateur LLM que cette communauté, à mon avis..
Parce qu'ils ne le construisent pas pour un LLM local
Je pense que 256 et 384 sont des points idéaux étant donné la prévalence de bons modèles récemment dans la gamme de taille ~370B. Et 512 pourrait même ne pas être proposé sur m5 ultra pendant un certain temps à cause de la rampocalypse.
Le préremplissage est censé avoir été beaucoup amélioré par l'architecture m5. Je ne sais pas quoi te dire.
Tout ce qui est en dessous de 512 Go n'est pas rentable. Même 512 Go c'est un peu faible. S'ils proposaient 768 Go, je prendrais comme Reksio prend du jambon.
Mec, tout mon disque dur principal local est un SSD de 128 Go
Ce post n'a pas bien vieilli... les prix sont pires.
Il se passe TELLEMENT de choses en termes d'efficacité des modèles locaux. Tu devrais juste essayer une autre variante du modèle.
voilà un modèle mlx 4 bits qui pourrait fonctionner
https://huggingface.co/mlx-community/Qwen3-Next-80B-A3B-Instruct-4bit
regarde un peu autour. voici quelqu'un qui fait tourner un modèle 80b sur un m1 à 35tps
https://www.reddit.com/r/LocalLLaMA/comments/1ni2chb/qwen3next_80b_mlx_mac_runs_on_latest_lm_studio/
La vitesse de traitement des prompts du M5 Max est très correcte. J'ai tout ce qu'il faut sous la main que tu peux exécuter localement, et le M5 Max est probablement l'un des meilleurs après le RTX 5090 et 6000 pour le traitement des prompts. Pourquoi penses-tu qu'Ultra va être tellement mieux ?
Je pense que ton problème, c'est que tu ne mets pas en cache les prompts correctement. Parce que je peux tourner sur mes Sparks QWEN3.5 122b et 397 et ils font le traitement des prompts à peut-être environ 1500, je n'ai pas les chiffres en tête, mais ça semble assez rapide parce que le caching est adéquat. Donc, je te conseille juste de bien gérer le caching.
Si tu as besoin de beaucoup de RAM, alors le Mac Pro avait 1 To de RAM je crois xd
tu peux acheter le Mac Studio de 512 Go ou même attendre la version de 1 To
utilise-le pour un setup de bureau et comme serveur quand tu veux utiliser ton MBP
« Développeur heavy utilisant l'IA, qui vit presque dans son IDE » correspond étonnamment mal au contenu technique du post. Sans scénarios concrets sur ce que l'on souhaite réellement faire avec la configuration, quels sont les fenêtres de contexte nécessaires et à quoi ressemble le flux de travail réel, les affirmations concernant les besoins en mémoire semblent assez générales. Avant de présenter 128 Go comme un minimum à peine suffisant, tu devrais peut-être revoir les bases du contexte, du cache KV et de la quantification.
Ce n'est pas appelé le Mac LLM. C'est difficile à imaginer mais les gens l'utilisent pour des choses autres que l'IA locale.
Vous pouvez toujours construire une machine threadripper à 20 000 $ qui a également 96 Go.
Toujours, ouais, tu peux toujours faire ça.