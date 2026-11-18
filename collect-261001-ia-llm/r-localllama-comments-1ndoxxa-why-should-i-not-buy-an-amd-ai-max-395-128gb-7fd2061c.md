Merci pour ton avis !
Explique-nous pourquoi ce contenu n’est pas utile.
      Pourquoi ne devrais-je **pas** acheter un AMD AI Max+ 395 128 Go tout de suite ? 
        
        
        
    
    
    Avec l'essor des modèles de taille moyenne MoE (gpt-oss-120B, GLM-4.5-air, et maintenant le Qwen3-80B-A3B qui arrive) et leurs excellentes performances pour les modèles locaux (enfin, du moins pour les deux premiers), la bande passante de calcul et de mémoire relativement faible du Strix Halo ne semble plus être un trop gros problème (à cause du faible nombre de paramètres actifs), et les 128 Go de VRAM pour 2k sont imbattables.
Donc maintenant, je suis très tenté d'en acheter un, mais je suis aussi conscient que je n'en ai pas vraiment besoin, alors s'il vous plaît, donnez-moi des arguments sur pourquoi je ne devrais pas l'acheter.
Mon portefeuille vous remercie d'avance.
Edit : merci pour votre réponse. Malheureusement, personne n'a vraiment réussi à me convaincre de renoncer à cet achat.
Maintenant, seule ma procrastination peut me sauver.
Section des commentaires
Votre publication devient populaire et nous l'avons juste mise en avant sur notre Discord ! Venez jeter un œil !
Vous avez également reçu un flair spécial pour votre contribution. Nous apprécions votre publication !
Je suis un bot et cette action a été effectuée automatiquement.
Je n'en avais clairement pas besoin, mais j'en ai quand même acheté un. Maintenant, je fais tourner gpt-oss-120b à >40tps et je consomme peu. Le truc utilise moins de puissance sous charge que mes autres serveurs quand ils sont inactifs.
Quelle sorte de vitesse pp tu obtiens ? (Combien de temps cela prendrait-il pour traiter un contexte de 5k-15k tokens ?)
Je possède le Framework Desktop (version 128 Go).
Voici la principale raison pour laquelle je dirais de NE PAS acheter : Tu veux que tout "fonctionne simplement". Le matériel à la pointe a besoin de logiciels/pilotes à la pointe. Attends-toi à des bugs, des crashs, et à lire des forums et Reddit à la recherche de solutions de contournement.
Je suis content de mon achat, mais j'adore aussi jouer avec les nouvelles technologies. Si ce n'est pas ton cas, tu devrais attendre.
C'est en fait une très bonne réponse.
Avec quoi as-tu eu des problèmes ?
(J'utilise Linux comme système principal depuis 17 ans maintenant, donc je connais assez bien le processus, mais les enfants mangent beaucoup de mon budget de bidouillage, donc je n'achèterais pas quelque chose qui serait trop compliqué à faire fonctionner).
Prends ma réponse avec un grain de sel parce que je me pose la même question mais...
Je pense que pour une utilisation faible/modérée, il est probablement moins cher d'utiliser une option basée sur le cloud pour faire fonctionner les gros modèles open source. Mais en local, on a l'avantage de 100 % de confidentialité et pour une utilisation intensive, cela pourrait finir par être moins cher à long terme.
Je pense aussi que attendre si possible a du sens, car les prix vont baisser et de nouvelles technologies sortiront éventuellement.
C'est un bon argument, 2k$ c'est comme 1000 heures de location de H100 dans le cloud.
Mais il semble aussi que ce soit beaucoup plus de travail si vous voulez gérer le llm fonctionnant sur votre propre GPU loué, plutôt que de simplement exécuter llama.cpp localement.
J'aimerais vraiment que quelqu'un fasse une version mATX avec genre deux slots PCI x8. 128 Go + 2 GPU de 24 à 32 Go seraient absurdes de puissance.
ÉDIT :
Je suppose que ça devra être PCI x4 avec les deux slots M2 convertis (merci à ceux qui ont répondu). Sigh ... Je n'arrive pas à croire qu'Intel/AMD et ARM aient tous raté l'occasion de profiter du marché des hobbyistes. Tout ce dont nous avions besoin, c'était :
~8 cœurs + une sorte de dGPU
~64 (4x16) + 14 = ~88 lignes PCI
~8 lignes de mémoire DDR5
pour moins de 1,5 K$. Au lieu de ça, on est bloqués avec des cartes mères Epyc à 800-1000 $ avant même de payer le CPU.
En fait, tu ne peux pas vraiment avoir autant de voies PCIe, malheureusement, à ma connaissance, il n'y a que 12 voies disponibles sur le CPU.
Mais tu peux toujours utiliser deux slots PCIe x4 si tu réutilises l'un des slots M.2 (pourquoi deux disques NVMe ?)
Je n'ai aucune idée de l'impact de la largeur des voies PCIe sur les performances pour l'inférence LLM, par contre.
Réponse authentique : ROCm. C'est pénible à maintenir et c'est vraiment un outil agaçant. Fais juste attention quand AMD sortira un GPU meilleur/nouveau, les choses vont soit casser très rapidement, soit devenir non supportées. Si tu utilises docker, fais attention aux images ROCm que tu utilises.
Mais Vulkan est déjà à 80% du chemin, donc je sais pas mec, et je pense qu'AMD consomme beaucoup moins d'énergie, donc fonce.
Mec, si tu as vraiment fait le pas sur ça, tu dois te sentir vraiment bien à propos de ça en ce moment XD
J'ai fait.
Je suis un peu déçu que Qwen 122B n'ait pas reçu de mise à jour depuis 3.5, mais je suis très heureux de mon achat quand même.
Mon conseil général concernant le matériel informatique est : « Si tu peux attendre, attends ; à long terme, les prix du matériel baissent et le support logiciel s'améliore. Si tu ne peux pas attendre, achète. »
Puisque tu dis que tu n'as pas besoin de ça en ce moment, tu devrais attendre. Quand tu auras un véritable besoin qui nécessite du nouveau matériel, achète du nouveau matériel à ce moment-là.
Une chose qui m'empêche, c'est que j'ai entendu dire que c'est mauvais pour la génération d'images.
Si tu peux attendre un mois ou trois, le prix va baisser un peu. Le Black Friday n'est pas si loin.
Parce que nous attendons toujours le truc Nvidia avec 128 Go de VRAM, qui va peut-être, un jour, sortir.
Est-ce que ça ne va pas coûter deux fois plus cher pour la même bande passante mémoire ?
>Mon portefeuille te remercie d'avance.
Désolé portefeuille, pas d'aide aujourd'hui.
Je demandais à GPTrécemment d'essayer d'estimer les cycles de produits d'AMD et d'Intel et quand une option de 256 Go avec autant de bande passante mémoire pourrait arriver. Il pensait à 2027-2028.
Intel a annulé son option et se dirige vers des options pour entreprises. Donc rien pour nous.
Apple n'est probablement pas prêt à sortir un Mac mini de 256 Go avant 2028.
Le 128 Go d'AMD pourrait être le meilleur choix pendant longtemps.
Le MAX+ 395 est probablement le meilleur CPU que vous puissiez avoir pour un appareil à petit facteur de forme, surtout que la vitesse d’horloge unique peut rivaliser avec des processeurs de bureau complets.
Cependant, ce qui me freine, c'est que le prix est premium et la bande passante de la mémoire n'est qu'à 256 Go/s.
Mon AMD 7900XTX remisé a 3 fois cette bande passante. Certes, il n'a que 24 Go, mais il existe de nombreux modèles utiles qui tiennent dans 24 Go de VRAM.
J'en ai un et je suis très content. (Connecté un eGPU avec une 4070 ti super dessus.)
Je ne sais pas pour ton cas d'utilisation, mais OpenRouter te donne 1 000 requêtes gratuites tant que tu as 10 $ sur ton compte.
Des modèles comme GLM 4.5 Air et GPT-OSS ont un niveau gratuit, et jusqu'à présent je n'ai pas atteint cette limite.
J'étais super super proche de commander un AMD AI Max+ 395 128 Go cette semaine. Je pense que j'ai évité la tentation pour l'instant. Je publierai quelques liens/pensées liés à ma décision d'acheter ou de ne pas acheter.
Je m'intéresse surtout aux situations de lot/concurrence (2-5 utilisateurs ou tâches concurrentes en même temps). Je serais beaucoup plus intéressé par le Strix Halo s'il montait jusqu'à 256-512 Go (bien qu'il y ait des données sur le clustering : https://www.jeffgeerling.com/blog/2025/i-clustered-four-framework-mainboards-test-huge-llms )
Dans mon esprit, voici quelques alternatives à comparer au Strix Halo de 128 Go pour les LLM. 1) un PC classique avec quelque chose comme 128 Go de RAM et une carte graphique exécutant ktransformers, 2) un Mac Studio avec au moins 128 Go de RAM (exécutant MLX ou GGUF), 3) peut-être Project Digits mais cela n'est toujours pas sorti.
Le Strix Halo a évidemment une mémoire plus rapide qu'un PC classique avec de la RAM système et une carte graphique - mais voici quelques benchmarks d'exemple pour ktransformers sur du matériel grand public (Core i9-14900KF + DDR5-4000 MT/s en double canal) + RTX 4090 : https://github.com/kvcache-ai/ktransformers/blob/main/doc/en/AMX.md
Le matériel Apple est plutôt bon pour un seul utilisateur à la fois, mais le traitement des invites n'est pas super rapide et la concurrence (plusieurs utilisateurs ou plusieurs tâches en même temps) n'est pas facile. Voici quelques benchmarks de llama.cpp pour différents chips de la série m : https://github.com/ggml-org/llama.cpp/discussions/4167 et ce PR a quelques notes sur la vitesse de llama.cpp en mode haute capacité avec llama-batched-bench : https://github.com/ggml-org/llama.cpp/pull/14363
Donc j'adorerais essayer le Strix Halo pour quelque chose comme ktransformers (connecter un GPU via le lien PCIe x4) ou pour faire tourner vllm afin d'obtenir une haute concurrence (puisque vllm sur Apple est uniquement CPU, ne supporte pas les quants, et est très lent, environ 8 tok/sec sur llama-2-7b-fp16 bs=1). J'ai trouvé ces benchmarks sur vllm sur le Framework Desktop : https://github.com/lhl/strix-halo-testing/tree/main/vllm (92 tok/s sur le Mac le plus puissant pour la taille de lot 1 q4 comparé à environ 357 tok/s pour la taille de lot 16 avec strix halo vllm q4 - et il semble que ce soit 149 tok/s sur llama-batched-bench pour M3 Ultra sur llama-2-7b-chat-q4km à bs=16 (ou -npl 16) pour un contexte 4k).
9/10 je recommanderais fortement. Je trouve tellement d'utilisations aléatoires pour l'IA bon marché que je ne savais pas que j'avais avant
C'est aussi ma QUESTION ! Je viens d’attraper le virus LocalLaLM. En ce moment, j'utilise mon ordinateur portable ROG avec un i9-13900H de 13e génération + Nvidia 4070. C'est suffisant pour me montrer les possibilités sans gratter la démangeaison. Alors, est-ce que je dépense 2 000 $ sur l'un de ces Strix Halo, un peu moins sur un bureau avec un 5070ti (je ne peux pas me permettre d'aller plus haut). Bien sûr, j'ai même vu quelques ordinateurs portables dans ce prix-là avec des processeurs Intel. Mais, sérieusement, je suis sur le point de me décider pour le GMKtec EVO-X2 AI Mini PC Ryzen Al Max+ 395 Mini Gaming Computers, 128 Go LPDDR5X 8000 MHz (16 Go*8) 2 To SSD PCIe 4.0. Est-ce que je fais une bêtise ?
Ce post spécifique a été supprimé en utilisant Redact. La motivation pourrait être liée à la vie privée, à la sécurité, à l'opsec, ou tout simplement un choix personnel d'enlever du vieux contenu.
friendly sugar fearless doll pen attempt yam obtainable liquid rustic