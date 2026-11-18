Merci pour ton avis !
Explique-nous pourquoi ce contenu n’est pas utile.
      Pourquoi le Strix Halo est un mauvais achat pour la plupart des gens 
        
        
        
    
    
    J'ai vu pas mal de posts qui font la promo du Strix Halo comme un bon achat, et je me suis souvent demandé si j'aurais dû l'acheter moi-même. J'ai appris beaucoup de choses sur la façon dont ces modèles sont exécutés depuis. Dans ce post, j'aimerais partager des mesures empiriques, d'où je pense que ces chiffres viennent, et expliquer pourquoi peu de gens devraient acheter ce système. J'espère que ça vous sera utile !
Modèle testé
- 
      llama.cpp
- 
      Gpt-oss-120b
- 
      Un des modèles de la plus haute qualité qui peut tourner sur du matériel milieu de gamme.
- 
      La taille totale de ce modèle est d'environ 59 Go, dont environ 57 Go de couches expertes.
Systèmes testés
Premier système :
- 
      128 Go Strix Halo
- 
      Quad channel LPDDR5-8000
Deuxième système (mon système) :
- 
      Dual channel DDR5-6000 + pcie5 x16 + une rtx 5090
- 
      Une rtx 5090 avec la plus grande taille de contexte nécessite environ 2/3 des experts (38 Go de données) pour résider dans la RAM du système.
- 
      cuda backed
- 
      mmap off
- 
      batch 4096
- 
      ubatch 4096
Voici les chiffres soumis par les utilisateurs pour le Strix Halo :
| test | t/s | 
|---|---|
| pp4096 | 1012.63 ± 0.63 | 
| tg128 | 52.31 ± 0.05 | 
| pp4096 @ d20000 | 357.27 ± 0.64 | 
| tg128 @ d20000 | 32.46 ± 0.03 | 
| pp4096 @ d48000 | 230.60 ± 0.26 | 
| tg128 @ d48000 | 32.76 ± 0.05 | 
Qu'est-ce qu'on peut en tirer ?
Les performances ne sont acceptables qu'avec un contexte de 0. Au fur et à mesure que le contexte grandit, les performances pp s'effondrent. Les performances tg subissent également un léger ralentissement.
Et voici les chiffres de mon système :
| test | t/s | 
|---|---|
| pp4096 | 4065.77 ± 25.95 | 
| tg128 | 39.35 ± 0.05 | 
| pp4096 @ d20000 | 3267.95 ± 27.74 | 
| tg128 @ d20000 | 36.96 ± 0.24 | 
| pp4096 @ d48000 | 2497.25 ± 66.31 | 
| tg128 @ d48000 | 35.18 ± 0.62 | 
Attends une seconde, comment les chiffres de décodage sont-ils si proches ? Le Strix Halo a une mémoire 2,5 fois plus rapide que mon système.
Regardons de plus près gpt-oss-120b. Ce modèle fait 59 Go. Il y a environ 0,76 Go de données de couche qui sont lues pour chaque token. Puisque chaque token a besoin de ces données, elles sont conservées dans la VRAM. Chaque token doit également lire 4 experts arbitraires, ce qui représente 1,78 Go supplémentaires. Étant donné que nous pouvons faire tenir 1/3 des experts dans la VRAM, cela porte la répartition totale à 1,35 Go dans la VRAM et 1,18 Go dans la RAM du système avec un contexte de 0.
Maintenant, la VRAM sur une 5090 est beaucoup plus rapide que la mémoire unifiée du Strix Halo et que le dual channel DDR5-6000. Au final, en effectuant ~53 % de vos lectures dans une VRAM ultra rapide et 47 % de vos lectures dans une RAM système un peu lente, le temps de décodage est très similaire avec de petites tailles de contexte par rapport à l'exécution de toutes vos lectures dans la mémoire modérément rapide du Strix Halo.
Pourquoi le Strix Halo a-t-il un ralentissement lorsque le contexte de décodage augmente ?
Probablement parce que lorsque la taille de votre contexte augmente, le décodage doit également lire le plus grand cache KV.
Et pourquoi mon système subit-il moins de ralentissement à mesure que le contexte augmente ?
Vous pouvez voir que, bien qu'avec un contexte de 0, le Strix Halo ait une avance en tg, il chute rapidement une fois que vous avez du contexte à traiter et que mon système gagne. C'est parce que tout le cache KV est stocké dans la VRAM, qui a des lectures de mémoire ultra rapides. Le temps de décodage est dominé par la lecture lente de la mémoire dans la RAM du système, donc cela ne fait pas bouger l'aiguille.
Pourquoi les temps de préremplissage se dégradent-ils si rapidement sur le Strix Halo ?
Bonne question ! J'adorerais savoir !
Puis-je simplement ajouter un GPU à la machine Strix Halo pour améliorer mon préremplissage ?
Malheureusement non. La capacité à utiliser un GPU pour améliorer les temps de préremplissage dépend fortement de la bande passante pcie et le Strix Halo n'offre que pcie x4.
Mesures réelles de l'effet de la bande passante pcie sur le préremplissage
Ces tests ont été effectués en modifiant les paramètres du BIOS sur ma machine.
| config | prefill tps | 
|---|---|
| pcie5 x16 | ~4100 | 
| pcie4 x16 | ~2700 | 
| pcie4 x4 | ~1000 | 
Pourquoi la bande passante pci est-elle si importante ?
Voici ma meilleure compréhension de haut niveau de ce que llama.cpp fait avec un gpu + cpu moe :
- 
      Tout d'abord, il exécute le routeur sur les 4096 tokens pour déterminer quels experts il faut pour chaque token.
- 
      Chaque token utilisera 4 des 128 experts, donc en moyenne chaque expert sera mappé à 128 tokens (4096 * 4 / 128).
- 
      Ensuite, pour chaque expert, téléchargez les poids sur le GPU et exécutez-les sur tous les tokens qui ont besoin de cet expert.
- 
      Cela en vaut la peine car le préremplissage est gourmand en calcul et l'exécuter simplement sur le CPU est beaucoup plus lent.
- 
      Ce processus est pipeliné : vous téléchargez les poids pour le token suivant, tout en exécutant le calcul pour le courant.
- 
      Maintenant, tous les experts pour gpt-oss-120b font ~57 Go. Cela prendra ~0,9 s pour le télécharger en utilisant pcie5 x16 à son maximum de 64 Go/s. Cela place un plafond en pp de ~4600tps.
- 
      Pour pcie4 x16, vous n'obtiendrez que 32 Go/s, donc votre maximum est d'environ ~2300tps. Pour pcie4 x4 comme le Strix Halo via occulink, c'est 1/4 de ce nombre.
- 
      En pratique, aucun des deux n'obtiendra sa pleine bande passante, mais les ratios absolus se maintiennent.
Autres avantages d'un ordinateur normal avec une rtx 5090
- 
      Meilleur refroidissement
- 
      Boîtier de meilleure qualité
- 
      Une 5090 aura presque certainement une valeur de revente plus élevée qu'une machine Strix Halo
- 
      Plus extensible
- 
      CPU plus puissant
- 
      Gaming haut de gamme
- 
      Les modèles qui tiennent entièrement dans la VRAM se décoderont également plusieurs fois plus vite qu'un Strix Halo.
- 
      La génération d'images sera beaucoup plus rapide.
À quoi sert le Strix Halo
- 
      Consommation d'énergie au repos extrêmement faible
- 
      C'est petit
- 
      Peut-être que tout ce qui vous intéresse, ce sont les chatbots avec presque 0 contexte
TLDR
Si vous pouvez vous permettre 1000 à 1500 $ de plus, vous feriez beaucoup mieux de simplement construire un ordinateur avec une rtx 5090. Le rapport qualité-prix est tellement plus fort. Même si vous ne voulez pas dépenser ce genre d'argent, vous devriez vous demander si votre cas d'utilisation est réellement couvert par le Strix Halo. Peut-être ne rien acheter à la place.
Corrections
Veuillez me corriger sur tout ce que j'ai mal compris ! Je ne suis qu'un novice !
EDIT :
WOW ! Le kit ddr5 que j'ai acheté en juin a vu son prix doubler depuis que je l'ai acheté. Peut-être que 50 % de plus est maintenant une sous-estimation.
Section des commentaires
Tes chiffres pour Strix Halo sont pas bons. Voici mes derniers chiffres gpt-oss-120b sur llama.cpp avec ROCm 7.10 :
Info de dernière minute : les ordinateurs plus chers sont plus rapides que les ordinateurs moins chers. Plus d'infos à 23h.
Ça doit être des chiffres qui datent. Parce que le Strix Halo est meilleur que ça maintenant et s'améliore de jour en jour. Voici un nouveau test qui vient de se terminer il y a une minute.
Bien sûr, même si le Strix Halo ne peut pas espérer avoir la puissance de calcul pour rivaliser avec la 5090 pour PP. En TG, j'ose dire qu'il se bat à armes égales avec la 5090. Même avec un contexte important.
Ouais, sans vouloir t'offenser, mais tu conseilles aux gens de dépenser potentiellement des milliers de plus et de consommer beaucoup plus d'énergie pour faire de l'inférence... Je vois pas l'intérêt, dans mon cas j'ai un desktop super rapide et solide comme un roc, minuscule, qui est excellent pour le prix... ça me donne pas les vitesses d'inférence d'un modèle de pointe, mais c'est carrément dingue pour jouer avec des modèles locaux sans se ruiner. Je suis beaucoup plus content avec ça plutôt que de construire un desktop pour égaler les vitesses de ma Strix avec une 5090 en plus.
Je veux dire, faut un peu comparer des setups qui coûtent le même prix, non ?
La vraie réponse, c'est de brancher 4 5090 sur ta strix halo.
Je rigole, je rigole, 4 3090, ça le fait.
Ma config actuelle, c'est 128 Go de strix halo avec 2 3090 et une 4090 de 48 Go, ce qui me permet de charger des modèles plus gros comme GLM-4.6
Je suis passé de mon PC de bureau 2x3090 x 128 Go DDR5 à un Halo Strix et je ne pourrais pas être plus content. GLM 4.5 Air qui fait de l'inférence à 120w est plus rapide que le même modèle qui tourne sur mon PC de bureau de 800w. Et maintenant, mon PC est de nouveau libre pour jouer !
C'est du pipeau, ça ?
Plusieurs testeurs ont montré que le modèle OSS fonctionne nettement mieux sur le matériel Nvidia, mais les différences de performance sont moindres lors de l'utilisation de modèles sans les couches expertes.
Cela dit, ce week-end dernier, NewEgg n'avait pas de 5090 à moins de 3200 $, avant les autres composants, contre 2000 $ pour la Strix…
Je suis pas fan de Strix Halo et je trouve ça un peu cher et sur-vendu, mais la plupart des gens n'ont pas de RTX5090 et un système capable de faire tourner de la DDR5 6000.
Au fait, il y a eu un post il y a quelques jours avec un fork de llama.cpp qui améliore les performances avec l'augmentation du contexte.
Tu as considéré la RAM (100 Go/s) comme le goulot d'étranglement pour la vitesse, mais en réalité, c'est le pcie 5.0 à 64 Go/s. Ça va diminuer ta vitesse théorique nette, où 47% est servi depuis la RAM.
En plus, t'as pas calculé pour plusieurs KVCache, t'as juste pris 20k de contexte. Dans les tâches réelles, le contexte grandit beaucoup plus vite, et c'est ça le problème. Si tu pouvais calculer pour plusieurs tailles de contexte, ce serait plus juste. Genre 20k, 40k, 60k, 80k, 100k, 150k, 200k.
Ce que je trouve intéressant dans ce post, c'est qu'il s'intitule "Pourquoi le Strix Halo est un mauvais achat pour la plupart des gens", mais nulle part il n'explique pourquoi les gens envisageraient d'acheter un Halo et quels sont les cas d'utilisation les plus courants, voire même des cas d'utilisation, et en quoi c'est un mauvais choix pour la plupart de ces cas d'utilisation. Comment pouvez-vous dire que c'est un mauvais achat pour la plupart des gens sans établir ce que la plupart des gens qui pourraient l'acheter veulent pouvoir faire et quels problèmes et limitations ils pourraient rencontrer avec lui par rapport à une 5090 ou d'autres options ?
J'ai un Halo. J'ai aussi une 5090 (et une RTX Pro 6000 aussi). Pour l'usage pour lequel je l'ai acheté, le Halo est BEAUCOUP plus utile et considérablement plus rapide que la 5090. La 6000 pourrait bien sûr le détruire pour les mêmes utilisations, mais alors je gaspillerais la 6000 pour quelque chose que le Halo peut faire assez bien, et à quel point ce serait stupide ? La 5090 est également BEAUCOUP mieux adaptée aux autres tâches qu'elle effectue qu'à ce pour quoi j'utilise le Halo.
Votre argument ne soutient pas votre thèse, et vous ne comprenez clairement pas autant de choses sur ce sujet que vous voulez le croire... Vous connaissez peut-être quelques détails techniques, mais vous ne comprenez presque rien des MULTIPLES façons dont les gens peuvent utiliser ces outils, ce qui est d'une importance capitale pour ce sujet.
J'ai un minisforum s1 max qui arrive et j'ai hâte de le tester à fond.
Tu vas adorer. N'écoute pas ce type. Tous ceux que je connais avec des Strix Halo les adorent. AMD améliore ROCm de plus en plus. Ils viennent d'envoyer des Strix Halo aux mainteneurs de llamacpp pour qu'ils voient quelles optimisations de performances ils peuvent faire.
Dépenser encore 2 000 balles pour une 5090, c'est dingue. Tu peux littéralement pas battre le rapport qualité-prix d'un système Strix Halo. J'ai eu le mien pour 1 650 il y a un moment et c'est mon truc quotidien. En dehors de l'IA, j'ai 128 Go de RAM super rapide associés à un CPU presque aussi performant qu'un 9950. Même comme labo domestique, c'est une affaire de fou.
Ça donne quoi les chiffres avec qwen3 coder et un grand contexte ?
Intéressant. Dis-nous combien coûterait ton ordi + GPU aujourd'hui chez un grand détaillant.
Ce post spécifique a été supprimé via Redact. La motivation est inconnue, mais cela pourrait inclure la vie privée, la sécurité, l'opsec, ou un désir général de réduire son empreinte numérique.
dam paltry fact hungry jar aspiring head brave cooperative fear
D'accord avec la plupart.
Mais pour la dernière partie, je crois que Strix Halo + GPU a encore du potentiel. À mon avis, le comportement actuel lié à la bande passante PCIe est en fait dû à une implémentation inférieure de llama.cpp.
L'heuristique de base pertinente est la suivante : pour les poids MoE déchargés sur la RAM, si la taille du lot est petite (pour le décodage, c'est 1), alors c'est définitivement limité par la bande passante mémoire, donc on le calcule simplement sur le CPU. Si la taille du lot est plus grande (surtout le préremplissage), alors la complexité computationnelle surmontera la bande passante mémoire/PCIe, donc on transfère les poids vers le GPU.
Le plus gros problème ici est de savoir quelle est la taille de "grand" ? Actuellement, llama.cpp utilise un nombre très grossier : 32. Oui, c'est un nombre fixe, quelle que soit votre configuration CPU/GPU. Faisons quelques calculs rapides. Supposons que les 120b soient tous des paramètres MoE. Un lot de 32 éléments nécessitera exactement 4*32=128 multiplications d'experts, c'est-à-dire 120G OPs. Maintenant, la performance dépend du taux de réutilisation des experts, c'est-à-dire combien d'experts doivent être lus. Si l'utilisation des experts est répartie uniformément, alors nous devons lire 60 Go de données pour 120G OPs. Les CPU grand public modernes pourraient facilement faire des centaines de GFLOPS, donc cela ne vaut évidemment pas la peine d'envoyer les données via PCIe. En réalité, il y aura une certaine réutilisation des experts, donc la meilleure stratégie varie en fonction du modèle/de l'entrée/de la taille du lot. Il y a une PR dans lk_llama il y a des mois qui s'attaque à cela (https://github.com/ikawrakow/ik_llama.cpp/pull/520). Avec quelques ajustements de paramètres, ils peuvent obtenir ~2x de performances PP avec une petite taille de lot.
Maintenant, passons au cas de Strix Halo. En suivant les calculs ci-dessus, vous verrez qu'envoyer les poids via PCIe ne vaudra jamais la peine pour Strix Halo - même avec une taille de lot de 4096. Le GPU+NPU de Strix Halo a une capacité théorique de 126 TOPs, c'est-à-dire facilement ~100x plus rapide qu'un CPU grand public classique. Et sa bande passante RAM est ~4x la bande passante PCIe 5 x16. Ce serait fou d'envoyer les poids via PCIe au lieu de calculer in-situ dans la RAM.
Si tout ce que tu veux faire, c'est faire tourner des MoEs sur ton système qui sont à peine plus gros que tes 32 Go de RAM, alors oui, tu as raison. Mais disons que tu veux faire plus grand, quelque chose qui remplirait presque 96 Go de RAM.
Je m'en fous de la vitesse de ta bande passante mémoire, tu vas te faire défoncer. J'ai vu la même chose se produire sur les anciens Apple Studio M1 par rapport au 5090. Le 5090 déchire tout jusqu'à ce qu'il atteigne un mur. Et plus l'allocation de mémoire dépasse 32 Go, plus le 5090 souffre. La mémoire devient plus précieuse que la bande passante, parce que tu ne brasses pas constamment la mémoire, limité par le PCIe, ou que tu dois diviser le surplus vers les CPU qui ne peuvent tout simplement pas rivaliser avec les GPU.
Tu as trouvé un seul point de données et tu as décidé d'en faire une généralisation complète.
a) 5090 seul, ça coûte près de 3000 $ ces temps-ci, et avec le reste du système, les prix avoisinent les 4000 $.
b) gpt-oss-120b, c'est MOE. Forcément, ce sera plus rapide sur la 5090, car seule une petite partie est réellement chargée. Essaie maintenant un modèle dense de taille moyenne sur la 5090 et compare-le à l'AMD 395.
c) C'est quoi la machine Strix Halo ? Un laptop ou un miniPC ? Vu qu'il n'y a aucune info. Je demande parce qu'il y a un écart de perf dû à la consommation entre un laptop (85C) et un MiniPC (140W).
d) C'est quoi les chiffres quand Lemonade est utilisé pour l'exécution hybride (iGPU + NPU) ?
gpt-oss-120b-mxfp-GGUF via Lemonade est supporté.
Y'a tellement de commentaires haineux. Changer sa config RTX 5090 pour une RTX 3090 et t'auras 70% de ses perfs pour -2000 dollars.
Alors, est-ce que les performances s'améliorent vraiment quand tu quantifies le cache kv en q8 ou moins ?
Le cache KV quant flingue les perfs de pré-remplissage pour moi. Je sais pas pourquoi !
J'ai un rig 5090, un de 7 litres en fait, donc c'est même pas si peu portable que ça qu'une boîte Strix Halo, mais ça n'a vraiment aucun sens de diviser le travail sur la mémoire principale du système, c'est juste un goulot d'étranglement énorme.
Les perfs du Strix Halo, comme beaucoup l'ont montré, s'améliorent et 30+tok/s sont atteignables avec un contexte large. Ça veut dire que c'est utilisable.
Je pense que si vous en avez besoin, ce serait vraiment bien, mais c'est la prochaine itération de ces puces Halo qui commencera vraiment à devenir intéressante. S'ils peuvent continuer à ajouter encore plus de canaux de mémoire, et bien sûr il y aura plus de calcul disponible, alors on commencera à voir 100tok/s sortir de ce modèle 120b et à ce stade, on parle de suffisamment rapide pour une utilisation générale.
Ce sera aussi tellement bien pour les algorithmes généraux du CPU de pouvoir exploiter toute cette bande passante mémoire. Une fois que vous commencez à dépasser la moitié d'un To/s, c'est un autre jeu.
Je pense aussi qu'une fois que les logiciels rattraperont leur retard, il y aura un avantage de réactivité pour les systèmes à mémoire unifiée qui pourront sauter le transfert de bus.
Ça veut dire que les jours où ça avait encore un sens de construire un PC de bureau dans un petit format sont comptés. Comme il se doit. L'unifié est juste logique.
On dirait que t'as juste aucune envie d'apprendre quoi que ce soit sur l'utilisation du strix halo. Peut-être qu'on devrait demander l'avis de gens qui veulent vraiment apprendre les choses correctement.