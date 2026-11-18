UniFi Controller d’Ubiquiti : utilisez un Raspberry Pi comme une Cloud Key
De quoi faire une belle économie !
Le 05 février 2019 à 11h16
UniFi Controller d’Ubiquiti : utilisez un Raspberry Pi comme une Cloud Key
De quoi faire une belle économie !
Hardware
Hardware
9 min
Ubiquiti est une marque bien connue des amateurs de solutions Wi-Fi destinées aux professionnels, à travers sa gamme UniFi. Celle-ci repose sur une gestion logicielle proposée dans des produits clé en main ou à installer sur votre propre machine. Nous avons tenté le coup avec un Raspberry Pi.
Comme nous aurons l’occasion de le voir dans un prochain dossier, les solutions Wi-Fi dites « professionnelles » ont un fonctionnement un peu différent de celles proposées au grand public.
Le routeur n’est en effet pas le centre d’un système sans fil auquel on peut ajouter des satellites et/ou répéteurs. Chaque élément dispose de son propre rôle : routeur, switchs, points d’accès, caméras, etc. La gestion du réseau sans fil, bien que pouvant être individuelle, est le plus souvent pensée pour être centralisée et multisite.
Elle passe en général par un élément connu sous le petit nom de contrôleur, proposé sous forme d’un matériel clé en main à connecter à votre réseau, ou d’un logiciel à installer sur une machine locale ou distante. Un nombre croissant de constructeurs proposent d’ailleurs des services hébergés, Netgear ayant par exemple fait le choix de ne se reposer que sur cette méthode pour la gestion centralisée de ses produits via Insight.
Aujourd’hui, nous allons évoquer le cas d’un concurrent bien connu des bidouilleurs : Ubiquiti.
Ubiquiti, c’est fouillis mais plutôt réussi
L'Américain s'est taillé en quelques années une place de choix dans le secteur des réseaux, notamment sans fil. Son offre est complète, modulaire, efficace et relativement ouverte. Elle bénéficie aussi d'interfaces agréables à utiliser, ce qui est plutôt rare dans le secteur. Le tout avec des produits plus ou moins abordables.
Ubiquiti a néanmoins un défaut, de taille : sa proposition commerciale est peu lisible. Distinguer ses gammes et les noms de produits/services, comprendre son fonctionnement... rien de tout cela n'est vraiment simple de prime abord. Il faut donc fouiller dans la documentation, analyser et parfois tester pour y parvenir.
Reste que la solution de gestion centralisée de la marque, UniFi Controller, est l'une des plus convaincantes du marché. Elle peut être utilisée comme service hébergé, mais c'est assez cher : 299 dollars par an pour 10 appareils ou moins, 498 dollars jusqu'à 20 appareils, 697 dollars jusqu'à 30 puis 199 dollars par tranche de dix ensuite.
Heureusement, elle est également installable sur la machine (locale/distante) de votre choix ou proposée à travers quatre produits clé en main : les Cloud Key, Cloud Key Gen2 (Plus) et UniFi XG Server.
Les contrôleurs matériels
La première (UC-CK) exploite un SoC à quatre cœurs (MediaTek MT7623), 2 Go de DDR, un port réseau Gigabit et une alimentation PoE (passif/802.3af) ou USB Type-C (5V, 1A). Elle est donnée pour une consommation minimum de 5 watts. On regrette surtout qu'elle soit livrée sans solution de fixation pour rack, son format y étant peu adapté.
Elle est néanmoins compacte et légère : 110 grammes, pour des dimensions de 121,9 x 43,4 x 21,7 mm. Elle dispose d'un micro-bouton reset et d'un autre permettant de l'éteindre. Elle est simplement livrée avec un câble réseau et une carte SD de 8 Go qui est utilisée en complément des 16 Go intégrés, contenant le système. Ce dernier est un dérivé de Debian Jessie (8.11) dans sa version actuelle (3.10.20-ubnt-mtk). Cette Cloud Key se vend aux alentours de 100 euros.
La Gen2 (UCK-G2) annoncée plus récemment se veut bien plus complète avec un SoC huit cœurs (Qualcomm APQ8053), 32 Go de stockage, du Bluetooth, un écran en façade et la possibilité d'un montage en rack (via un adaptateur 1U CKG2-RM). Elle est un peu plus volumineuse et lourde : 150 grammes pour 119,75 x 46,8 x 27,1 mm.
Elle est présentée comme la manière de gérer jusqu'à une cinquantaine d'appareils, sans précisions sur les capacités de la version précédente sur ce point.
Une déclinaison Gen2 Plus (UCK-G2-PLUS) est également proposée avec un disque dur (2,5") de 1 To intégré, pouvant être changé. De quoi lui permettre de gérer 20 caméras, ou 15 caméras et jusqu'à 50 appareils selon le constructeur. Le tout pèse 585 grammes pour des dimensions de 134,2 x 131,16 x 27,1 mm.
Le montage peut également être effectué en rack via l'adaptateur CKG2-RM, mais l'alimentation a été revue à la hausse. Ainsi, outre le PoE (12,95 watts) ou l'USB Type-C, un adaptateur Quick Charge 2.0/3.0 (9V, 2A) peut être utilisé. Ce produit doit permettre à Ubiquiti d'évoluer dans la vidéosurveillance, la société venant d'annoncer sa gamme Protect.
Côté tarif, comptez respectivement 220 euros et 260 euros environ. Vu la faible différence de prix, on aurait préféré que le constructeur ne propose que le Gen2 Plus, mais sans stockage par défaut, à un tarif plus abordable.
Enfin, on trouve l'UniFi XG Server (UAS-XG), qui est, comme son nom l'indique, un serveur (sous Ubuntu). Il intègre un Xeon D-1521, 32 Go de mémoire, un SSD de 120 Go en complément de 2x 2 To de stockage plus classique.
Il propose deux ports 10 Gb/s (RJ45), un Gigabit pour la gestion à distance (IPMI), deux USB 2.0, deux USB 3.0 et du Bluetooth BLE 4.0 pour la phase d'installation. Il intègre UniFi Controller/Video mais peut également faire fonctionner les applications de votre choix en parallèle. Il est proposé à pas moins de 2 000 euros pièce.
Installation d'UniFi Controller sur un Raspberry Pi
Les solutions logicielles d'Ubiquiti reposent en bonne partie sur Debian et ses dérivés. Il est donc possible d'installer le logiciel Unifi Controler sur une machine sous Linux, mais pas seulement : des versions macOS et Windows sont disponibles. Deux canaux existent : LTS (support à long terme) et classique. Notez que dans tous les cas, Java est nécessaire.
Notre objectif du jour est de créer notre propre « Cloud Key », mais pour un tarif moindre. De quoi gérer quelques points d'accès au sein d'un petit réseau local. Nous allons utiliser un simple Raspberry Pi. Un dépôt APT est en effet proposé par Ubiquiti, permettant une installation simple (Raspbian étant lui aussi un dérivé de Debian).
Nous partons du principe que votre Raspberry Pi est configuré sous Raspbian Lite, à jour et accessible via SSH ou une interface graphique. Si ce n'est pas le cas, suivez notre guide d'installation :
Pensez également à lui attribuer une IP fixe au niveau de votre routeur afin d'éviter qu'il n'en change, ce qui pourrait poser des soucis pour l'accès à l'interface ou même la communication avec les appareils.
Il faut tout d'abord ajouter le dépôt d'Ubiquiti afin qu'APT puisse l'utiliser, ainsi que les clés GPG qui servent à vérifier la provenance des fichiers. Pour cela, ouvrez un terminal ou une connexion SSH et tapez :
echo 'deb http://www.ui.com/downloads/unifi/debian stable ubiquiti' | sudo tee /etc/apt/sources.list.d/100-ubnt-unifi.list
sudo apt install dirmngr
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv 06E85760C0A52C50
Il faut ensuite mettre à jour le contenu des dépôts, puis s'occuper du cas de Java. En effet, les dépôts de Raspbian proposent la version 9 qui n'est pas encore supportée par Unifi Controller. En attendant, deux choix s'offrent à nous : OpenJDK 8 ou la version d'Oracle. Cette dernière étant la plus performante, nous la choisissons :
sudo apt update
sudo apt install oracle-java8-jdk
Notez que si vous préférez OpenJDK, la procédure à suivre est :
sudo apt update
sudo apt remove openjdk-9-jre-headless
sudo apt install openjdk-8-jre-headless
On peut alors installer Unifi Controller :
sudo apt install unifi
Une fois la procédure terminée, Unifi Controller sera accessible sur le port 8443 de votre Raspberry Pi. Vous pouvez donc le gérer à travers les applications mobiles ou l'interface web via l'URL suivante :
https://IP_de_votre_Raspberry_PI:8443
Contrairement à une Cloud Key vous n'aurez en effet pas le choix entre son interface de gestion et un accès à UniFi Controller, qui sera la seule possibilité. La mise à jour du système devra donc se faire de manière classique (via APT). La procédure de configuration et l'interface sont identiques à la version classique.
Une démonstration en ligne est accessible par ici (après création d'un compte).
Les étapes post-installation
Vous pouvez effectuer quelques tâches une fois l'installation finie, comme désactiver le service MongoDB puisque UniFi Controller dispose du sien :
sudo systemctl stop mongodb
sudo systemctl disable mongodb
Vous pouvez également indiquer que le système ne doit pas mettre à jour UniFi Controller en même temps que le reste du système afin de pouvoir procéder à cette étape de manière manuelle :
sudo apt-mark hold unifi
Puis pour la mise à jour manuelle :
sudo apt-mark unhold unifi
sudo apt update
sudo apt install --only-upgrade unifi
sudo apt-mark hold unifi
Vous pouvez voir à tout moment la liste des paquets « marqués » de la sorte :
sudo apt-mark showhold
UniFi Controller au-delà du Raspberry Pi
Bien entendu, cela fonctionne pour toute machine utilisant un système dérivé de Debian avec APT comme gestionnaire de paquets, et donc d'autres micro PC sous Armbian par exemple. Vous pouvez également l'utiliser depuis une machine de récupération, un serveur sous la forme d'une machine virtuelle ou d'un système de conteneurs tel que Docker.
UniFi Controller peut être utilisé via une connexion filaire ou sans fil (si celle-ci ne dépend pas d'un point d'accès UniFi qu'elle contrôle) et sur des machines moins performantes qu'un RPi 3B+, même si cela n'est pas forcément conseillé du fait de la puissance et de la mémoire limitées. Nous l'avons néanmoins installé avec succès sur un RPi Zero W.
Commentaires (13)
Le 05/02/2019 à 12h23
Dans mon ancien boulot j’administrais environ 1000 AP et 30 switch sur un contrôleur Unifi déporté (qu’on hébergeais sur un “petit” serveur dédié). C’est du java et ça consomme pas mal de RAM (c’est proportionnel au nombre d’appareils gérés). En plus, il y doit y avoir des fuites mémoires parce qu’il faut forcément le redémarrer au bout d’un moment.
En dehors de ces défauts, le logiciel est vraiment bien et le matériel aussi. Les MAJ sont assez fréquentes et la communauté est (relativement) écoutée.
Si on ne met pas de stats (ou qu’il n’y a pas de trafic), un Rpi 2 peut faire l’affaire, mais devient très vite ingérable de part sa lenteur.
Ubiquiti propose des produits au rapport qualité/perf/prix plus que correct, je recommande à ceux qui veulent mettre les mains dedans ou qui ont envie d’avoir un réseau wifi (ou pas), de qualité et “hautement” paramétrable.
P.S. : je ne travaille pas pour eux, ils font juste du bon boulot ;)
Le 05/02/2019 à 13h10
+1 pour Ubiquiti et Unifi.
Très pratique pour les entreprises.
J’ai aussi réalisé plusieurs interconnexion avec leurs radio (Bullet, AirFiber…) c’est très simple et efficace.
Le 05/02/2019 à 15h41
Je cherche un installateur qui pourrait mettre un place une installation de vidéosurveillance Unifi dans le parking de ma résidence (dans le 94). Hélas, je n’ai pour l’instant pas réussi à en trouver un seul… Si vous avez des noms, je suis preneur !
J’aime cette solution car je l’utilise chez moi à titre personnel et j’aimerais pouvoir utiliser du matériel que je connais et en lequel j’ai confiance.
Le 05/02/2019 à 16h10
Bonjour ! Je viens de t’écrire un MP pour te proposer mes services :) J’espère à très bientôt !
Le 06/02/2019 à 10h59
Vu le nombre de commentaires, je ne suis pas le seul a ne pas comprendre de quoi on parle dans l’article…
Le 06/02/2019 à 11h17
Il n’y a rien de compliqué pourtant, par contre pour un particulier l’intérêt est plutôt faible en fait.
Le 06/02/2019 à 13h03
Bah tout dépend, je préfère voir un particulier utiliser des AP en complément du routeur de sa box que d’utiliser de mauvais répéteurs. Après ubiquiti ou pas, contrôleur ou pas, RPi ou pas, c’est un autre sujet.
Le souci reste qu’on considère souvent que coller un AP lambda avec le même SSID est une bonne solution. Disons que ça fonctionne à peu près, mais ça pose aussi pas mal de soucis (dont on se fout quand on veut juste un signal qui passe sans se poser plus de questions).
Il y a une astuce : lire l’article ;)
Le 07/02/2019 à 16h33
A la construction de ma maison, j’ai prévu dès le départ 3 AP Unifi pour avoir un excellent wifi partout.
Je ne suis pas déçu. J’avoue que par fainéantise, j’ai préféré acheter une cloud key :p. Je viens d’ailleurs de commander la Gen2 Plus car j’ai aussi prévu de mettre quelques caméras chez moi (c’est clair que quand on construit c’est plus facile de faire passer quelques cables Ethernet).
Pour le POE, j’ai pris des TP-LINK 5 ports (dont 4 POE), juste pour le fait qu’ils restent passifs. Je me serais bien laissé tenter par le switch POE Unifi, mais il semble assez bruyant.
Je reste aussi a l’affut de leur plateforme UAG que je trouve intéressante. Le “petit” modèle est maintenant trop petit pour faire de l’introspection. Quand on regarde les nouveaux modèles qui sont sortis côté routeurs, c’est clair que la gamme UAG va être remise au gout du jour.
D’ailleurs David, tu as jeté un coup d’oeil à ce que sait faire la gamme UAG ?
Le 07/02/2019 à 16h38
Tu as pris quoi comme AP par curiosité ? Parce que même si la couverture est bonne, le débit reste assez léger (surtout si on passe en Mesh). Mais je suis en train de tester dans différents cas pour avoir des données chiffrées.
Par contre attention, passif pour le PoE ça veut juste dire que ; 1. ça ne respecte pas la norme, 2. le jus est envoyé tout le temps sans aucune vérification. Cela n’a rien à voir avec une consommation et une chauffe plus ou moins importante. Voir :
https://www.inpact-hardware.com/article/1100/poe-et-802-3atafbt-ce-quil-faut-savoir-alimentation-par-cable-reseau
Après si tu veux dire que le switch n’a pas de ventilation, attention quand même à sa chauffe ;) (mais sur 5 ports pour des AP ça devrait aller).
Pour l’article, je vais commencer par les AP et la gestion distante, la partie routage ce sera plus tard. C’est déjà bien assez long/compliqué comme ça
Le 07/02/2019 à 17h00
Oppss.
Petite correction. C’est de la gamme USG (Unifi Security Gateway) et non UAG dont je voulais parler. La gamme UAG c’est encore autre chose.
David, c’est vrai que leur gamme n’est pas très lisible. Mais quel plaisir d’avoir un système qui marche “tout seul” par la suite. J’en ai même monté chez mes parents ;).
Le 07/02/2019 à 17h24
Pour les AP, c’est les AC Pro. Je suis resté dans des prix “raisonnables”. Après ça monte en flèche ! Pas de mesh chez moi. Je n’ai pas constaté de problèmes de débits, mais je n’ai certainement pas poussé les tests comme toi.
Pour les POE, c’est bien passif au niveau ventilation. J’ai pris celui la TP LINK TL-SG1005P. A 56W maxi (passé cela, c’est ventilateur obligatoire), c’est suffisant pour les 3 AP + CloudKey. Le second attend pour les caméras (3 G3 Pro qu’il faut que je commande). Pour avoir regardé la température, ce qui chauffe le plus, c’est le transformateur externe :p. Le switch en acier dissipe bien mieux la chaleur. C’est chaud au toucher, pas brulant. Mais j’ai laissé de l’espace autour pour la ventilation.
Le 07/02/2019 à 20h43
Ah Ubiquiti ! Je suis content d’avoir découvert cette marque, je suis satisfait de leurs produits même si j’ai encore du mal sur certains réglages. Je suis curieux de lire les prochains articles sur Ubiquiti.
Il y a une petite coquille dans l’article, c’est 802.3af au lieu de 802.11af.
Le 07/02/2019 à 21h32
J’ai un peu la config présentée, USG, switch 8 ports 150W, UAP-Pro et j’avais installé le controller sur un raspberry mais depuis 1 mois je l’ai basculé sur mon NAS avec Docker.
Dans ma future maison j’envisage des AP-IN-Wall, je ne sais pas ce que ça vaut.
En temps que petit particulier amateur le controller est vraiment sympa à bidouiller avec toutes ses fonctionnalités mais par contre peu de docs ou tutos en français.
Signaler un commentaire
Voulez-vous vraiment signaler ce commentaire ?