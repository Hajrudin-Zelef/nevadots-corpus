Merci pour ton avis !
Explique-nous pourquoi ce contenu n’est pas utile.
Le Ryzen AI MAX+ 395 est une vraie pépite (dans le bon sens)
J'ai passé une commande pour la version 128 Go de la carte mère Framework Desktop](https://frame.work/products/framework-desktop-mainboard-amd-ryzen-ai-max-300-series?v=FRAFMK0006) pour l'inférence AI principalement, et bien que j'ai attendu patiemment qu'elle soit expédiée, j'ai récemment eu des doutes sur le rapport coût/bénéfice/évolutivité future puisque la RAM, le CPU/iGPU sont soudés à la carte mère.
Donc, j'ai décidé de faire un exercice rapide de choix de pièces pour PC afin de correspondre aux spécifications que Framework propose avec leur carte de 128 Go. J'ai commencé à regarder des cartes mères offrant 4 canaux, en pensant que je trouverais quelque chose de bon marché... erreur !
- 
      La carte mère grand public la moins chère offrant DDR5 à haute vitesse (8000 MT/s) avec plus de 2 canaux coûte plus de 600 $.
- 
      Le CPU équivalent au 395 MAX+ dans les benchmarks est le 9955HX3d, qui coûte environ ~660 $ sur Amazon. Un dissipateur thermique silencieux avec des doubles ventilateurs de Noctua coûte 130 $.
- 
      La RAM de G.Skill 4x24 (128 Go au total) à 8000 MT/s vous revient à environ 450 $.
- 
      L'iGPU 8060s est similaire en performance à l'RTX 4060 ou 4060 Ti 16 Go, coûtant environ 400 $.
Le total pour cette configuration est d'environ 2240 $. C'est évidemment 500 $ de plus que la carte de Framework. Mis à part le coût, la vitesse est compromise puisque le GPU dans cette configuration va accéder à la plupart de la RAM système à un certain coût puisque ça vit à l'extérieur de la puce GPU et doit passer par le PCIE 5 pour accéder directement à la mémoire. La consommation totale d'énergie à partir du mur à pleine charge du système est au moins deux fois celle de la configuration du 395. Plus de puissance = Plus de bruit de ventilateur = Plus de chaleur.
En comparaison, les M4 Pro/Max offrent une bande passante mémoire plus élevée, mais galèrent à faire fonctionner des modèles de diffusion, et coûtent aussi 2 fois plus cher avec les mêmes spécifications de RAM/GPU. Le 395 fonctionne sous Linux/Windows, offrant plus de flexibilité et de polyvalence (jeux sous Windows, inférence sous Linux). Nvidia est tellement loin en termes de coût qu'il est absurde de le comparer. L'équivalent le plus proche (mais à une vitesse d'inférence beaucoup plus élevée) est 4x 3090 qui coûte plus cher, consomme plusieurs fois la puissance, et génère beaucoup plus de chaleur.
AMD a ici une vraie pépite. Pour les bricoleurs et les passionnés cherchant à développer, tester, et acquérir plus de connaissances dans ce domaine, le MAX+ 395 est à peu près la seule option viable à ce montant $$, avec cette faible consommation d'énergie. J'ai décidé de poursuivre ma commande, mais je me demande si d'autres ont exploré cette voie en cherchant des réponses similaires..!
EDIT : Le 9955HX3d ne prend pas en charge 4 canaux. L'autre option est le Threadripper qui a des vitesses de mémoire plus lentes.
Section des commentaires
D'accord, mais peut-on avoir des chiffres ?
Ici d'un autre post :
https://www.reddit.com/r/LocalLLaMA/search/?q=Strix%20Halo%20&cId=83c898d3-4b06-4e77-bbd1-08c11245f877&iId=5253497b-6cd2-4d7d-a86e-ef252c4bf071
Notez qu'il est presque impossible de faire fonctionner quatre barrettes DDR5 à 8000 de vitesse.
Ta conclusion est correcte, mais tes données sont fausses, le 9955HX3d a moins de la moitié de la bande passante mémoire (89 contre 256 Go/sec) de l'AI Max, car le Max a un bus mémoire deux fois plus large + l'avantage de la mémoire soudée.
La façon dont AMD a réussi à doubler le bus mémoire est en volant des lignes PCIe, ce qui explique sa très mauvaise connectivité PCIe. C'est pourquoi c'est un unicorn pour le travail d'IA.
Les seuls CPU qu'AMD fabrique avec la même bande passante ou une bande passante supérieure sont les séries Epyc 9000, mais ils n'ont pas de GPU.
https://www.techpowerup.com/cpu-specs/ryzen-9-9955hx3d.c4036
https://www.techpowerup.com/cpu-specs/ryzen-ai-max-395.c3994
Bons points. D'autres ont commenté sur mon erreur concernant le 9950x3d. La configuration que j'ai en tête ne pourra jamais rivaliser à ce stade.
Le Ryzen AI Max est un camion, capable de transporter de lourdes charges à des vitesses raisonnables. Un PC de consommation avec un GPU puissant est une voiture de sport : il est beaucoup plus rapide, mais il ne peut pas transporter autant.
Pour les modèles de diffusion, je choisirais toujours un rtx 5080 et les composants nécessaires pour égaler le prix du Ryzen AI Max - si vous voulez comparer dans la même gamme de prix. Vous n’aurez pas besoin de 128 Go de RAM pour générer des images ou même des vidéos.
Mais pour les LLM, plus de RAM est préférable.
Y a-t-il un avantage à utiliser le framework au lieu du mini PC de gmtek ? C'est le même prix, mais le gmtek est déjà un PC complet. Est-ce que je manque quelque chose ? Combien devrais-je dépenser de plus pour que la carte framework soit une carte complète ? 300 € ?
ETA prime a assemblé un ensemble il n'y a pas longtemps à partir du Board lui-même. Mon installation s'inspire de cela et coûtera encore 250 $.
Tout ce dont vous avez besoin en plus est un boîtier mini ITX, une alimentation flex, aucun, un ventilateur Noctua simple, et un câble AC.
Super analyse, c'est quelque chose que la plupart d'entre nous soulignent :)
Mais tu as raté un petit détail. Le 9950X n'a pas de RAM à canal quadruple. Il faut donc ajouter le vrai coût d'un CPU compatible avec un canal quadruple au $600 pour une carte mère de puissance équivalente à la 395 :)
Et ces cartes mères pourraient nécessiter des RDIMM de nos jours, donc le coût de la RAM sera encore plus élevé.
C'est vrai ! J'avais oublié que les Threadrippers ont ces canaux supplémentaires et que les modèles récents coûtent plus de 2k $. Mince, c'est une config coûteuse à ce stade.
Je crois qu'un setup Threadripper Pro pas cher avec 8 canaux et 256/512 Go de RAM DDR4 coûterait moins cher et offrirait des performances similaires.
Les puces AMD sont limitées en bande passante par rapport aux plus grandes cartes de Nvidia. Doubler la mémoire n'améliorera pas ce critère.
Les cartes Nvidia ne tiennent pas la route dès que vous exécutez un modèle plus grand que la VRAM. Pour une comparaison équitable, vous avez besoin de plusieurs cartes, sinon vous bouleversez l'équation coût/puissance/bruit.
Tu pourrais être déçu par la vitesse de ton préremplissage. Je pense que c'est une machine assez mauvaise pour le llm local. J'ai dépensé environ 1k de plus pour un 5090 + 96 Go de RAM et ma tg sur gpt-oss-120b est presque à la même vitesse, tandis que pp est comme 4-15x plus rapide.
À combien ça va coûter exactement ?
Ne me fais pas dire ce que je n'ai pas dit, un ancien rig de minage avec 4x 3090 va déchirer le 395, mais le coût + la consommation d'énergie + la chaleur, ce n'est plus comparable.
Vous pouvez l'exécuter en mode basse consommation (par exemple 54W) et cela n'affecte que très peu les performances du LLM.
Cela se transforme en $$$ sur une année d'exécution 24/7 et dans les zones à coût élevé d'électricité, cela peut compenser le coût de l'appareil.
Comparer 4x MI50 pour 600 $ d'achat et 20 c par kWh, cela ferait 1752 $ d'électricité sur une année (2300 $) contre 1x 395+ pour 1700 $ et 175 $ d'électricité (1875 $).
Donc, souvent, le 395+ a un coût total de possession plus bas pour une utilisation intensive quotidienne de LLM local 24/7.
< 256Go/s de bande passante mémoire. Un beau contexte, mais des inférences lentes.
Toujours trop cher
J'ai eu tellement de réactions négatives ici il y a quelques mois pour avoir précommandé l'un de ces trucs au lieu de dénicher de vieux équipements d'entreprise sur eBay. Et tant de modèles sont sortis depuis qui sont parfaitement adaptés, je suis vraiment excité que le mien ait été expédié ce matin.
Je souhaite qu'ils aient une variante de 256 Go
Juste une question. Peut-on raisonnablement l'utiliser pour la génération d'images/vidéos/sons ?
Quelqu'un dans les commentaires l'a utilisé pour faire tout ce qui précède avec ROCM 7. Pas super rapide mais ça fait largement le job.
Juste une correction : 4×24 Go = 96 Go, pas 128 Go. De plus, le DDR5-8000 n'est pas réaliste sur un Ryzen 9950X avec quatre DIMM. Le contrôleur de mémoire intégré ne peut pas soutenir ces vitesses avec des modules de 4×24 Go (double rang). Dans les configurations 2DPC, la mémoire se stabilise généralement autour de 3400–3600 MT/s pour la stabilité, indépendamment du profil XMP/EXPO évalué du DIMM. Si vous voulez des vitesses DDR5-8000, vous auriez besoin d'un kit 2×16 Go ou 2×24 Go et d'une carte validée pour cela, mais pas avec 96 Go répartis sur quatre emplacements.
Ouais. Quelqu'un d'autre a repéré mon mauvais calcul, et maintenant cette config est impossiblement chère.
Je suis vraiment fan du mien. Je pense que si AMD sort une mémoire unifiée de 256 Go, cela va complètement modifier le marché.