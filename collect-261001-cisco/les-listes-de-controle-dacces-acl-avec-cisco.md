## I. Présentation

**Dans ce tutoriel, nous allons aborder la notion d'ACL, c'est-à-dire les listes de contrôle d'accès, avec quelques exemples pratiques à mettre en œuvre sur Cisco.**

### Une ACL, c’est quoi et pourquoi ?

Les **ACL**, pour ***A**ccess **C**ontrol **L**ist*, sont des règles appliquées aux trafics transitant via les interfaces du routeur que ce soit en entrée (*in*) ou en sortie (*out*). Les ACL filtrent le trafic en demandant aux interfaces d’acheminer ou non les paquets qui y transitent. Pour ce faire, le routeur lit l'en-tête de chaque paquet afin de déterminer s'il doit être acheminé ou non en fonction des conditions définies dans la liste de contrôle d’accès ACLs.

Les avantages de les mettre en place sont nombreux, on peut citer par exemple :

- Maitriser le réseau en déterminant quel type de trafic sera acheminé ou bloqué.
- Augmenter le niveau de sécurité d’accès réseau de manière basique en accordant ou non l’accès à un segment du réseau.
- Optimiser le réseau à d'autres fins que la sécurité, par exemple pour contrôler la bande-passante, restreindre le contenu des mises à jour de routage ou identifier et classer le trafic par fonctionnalités de qualité de service (QoS).

**Les ACL s'appliquent selon un ordre séquentiel en évaluant les paquets au début de la liste d’instruction**. Si le paquet répond aux critères de la première instruction (appelées ACE pour *Access Control Entry*), il ignore le reste des règles. Pour reproduire ce tutoriel, vous pouvez utiliser du matériel virtuel Cisco sous Packet Tracer.

## II. ACL : entrante ou sortante

Une question très importante que l'on peut se poser lorsque l’on veut mettre en place l’ACLs, c'est savoir sur quelle interface faut-il appliquer les ACLs ? L’interface entrante ou sortante du routeur ? Répondons à cette question dans cette première partie de l'article.

Les ACLs peuvent être associés à une interface particulière et pour une direction du flux (en entrée ou en sortie). En outre, ces règles de filtrages peuvent être appliquées avant que le routeur ne prenne sa décision de routage (interface en entrée), ce qui est un bon moyen d'économiser les ressources matérielles du routeur, ou après que le routeur ait pris sa décision de transfert et déterminé l’interface de sortie à utiliser pour acheminer le paquet.

Un petit exemple :

Dans la topologie ci-dessus, l’interface entrante de **R1** est **Gi 1/0,** et l’interface sortante est **Gi 0/0**.

L’interface entrante de **R2** est **Gi 0/1** et l’interface sortante est **Gi 0/3**. Si par exemple, nous avons activé une ACL sur l’interface **Gi 0/2** de **R2**, cette ACL ne pourrait pas filtrer les paquets envoyés de PC2 au serveur S1, car les paquets ne passent pas l’interface **Gi 0/2**.

**En somme, pour filtrer un paquet, on doit activer une ACL sur l’interface qui traite le paquet.**

Si on active l’ACL sur **R1** pour les paquets entrants sur l’interface Gi1/0, R1 va **comparer chaque paquet entrant sur cette interface aux entrées de l’ACL** **afin de décider le sort de ce paquet** : continuer sans changement et acheminer le paquet ou le rejeter.

## III. La correspondance des paquets

C’est la manière dont on configure les ACL, on indique les informations qui seront utilisées par le processus de filtrage, cela peut être **une adresse IP ou bien un protocole réseau**. En se basant sur ces informations, le routeur déterminera s’il doit autoriser on refuser les paquets.

Le diagramme ci-dessous illustre mes propos :

Lors du traitement de l’ACL, le routeur traite le paquet comme suit :

**Les ACLs utilisent la logique de première correspondance.** Une fois qu’un paquet correspond à une ligne dans l’ACL, le routeur exécute cette règle puis le processus s’arrête.

L’exemple ci-dessous nous permettra de voir ce que cela signifie précisément :

Sur l'exemple ci-dessous, l'ACL est appliquée sur l’interface **Gi1/3** et elle contient trois règles :

- Si la source = 10.1.1.1, on autorise le paquet (*permit* )
- Si la source = 10.1.1.X, on refuse le paquet (*deny* )
- Si la source = 10.X.X.X, on autorise le paquet (*permit* )

On considère qu’un paquet est envoyé par le **PC1 (10.1.1.1)** au serveur **S1**, le routeur **R1** compare ce paquet à l’ACL correspondant à la première ligne de l'ACL, le paquet sera autorisé, car l'adresse IP du PC correspond à celle définie dans l'ACL.

Ensuite, considérons qu’un paquet est envoyé par le **PC2** **(10.1.1.2)** au serveur **S1.** À l’arrivée du paquet, le routeur va poursuivre la même logique de recherche en comparant le paquet à la première ligne de notre ACL. Il ne fera pas une correspondance, car 10.1.1.2 (adresse IP de PC2) n'est pas égal à l'adresse IP définie dans la première règle (10.1.1.1). Donc, R1 passe ensuite à la deuxième instruction à savoir "*10.1.1.x deny*", où **x** correspond à n’importe quelle valeur qui peut exister dans le dernier octet. En ne comparant que les trois premiers octets, R1 réalise que notre paquet émis depuis PC2 a une adresse IP source qui correspond à "10.1.1". De ce fait, R1 considère qu’il s’agit bien d’une correspondance à la deuxième règle de notre ACL et applique l’action comme indiqué dans la règle, à savoir refuser le paquet (*deny*). **Le routeur R1 arrête également le traitement ACL sur ce paquet, en ignorant la troisième règle de l’ACL.**


**Si jamais un paquet ne correspond à aucune des règles de la liste ACLs, le paquet est rejeté. On parle d'un refus implicite.**

Il est important de savoir que le traitement des ACL en mode séquentiel, en lisant les règles dans l'ordre, s’applique sur tout type d’ACL.

## IV. Les types d'ACLs

Il existe plusieurs types d'ACL, que nous allons découvrir ensemble sans plus attendre.

### A. Les ACL standards

Dans ce type, l’ACL ne peut être liée qu'à l’adresse IP source du paquet. Ces ACLs sont identifiables par identifiant correspondant à un nombre allant de **1 à 99** et de **1300 à 1999**.

Nous pourrons utiliser ce type d'ACL pour autoriser ou interdire un segment du réseau ou l'adresse IP d’une machine à communiquer avec un autre segment de réseau ou une autre machine.

Afin de concrétiser la notion d’ACL standard, nous allons les mettre en place sur un routeur Cisco.

Dans l’exemple ci-dessus, les trois ordinateurs, situés sur des segments réseau différents, communiquent entre eux. Le but est d’interdire au réseau « **10.1.1.0/24 »** de communiquer avec le réseau « **10.1.2.0/24 »** tout en ayant la possibilité de communiquer avec le réseau « **10.1.3.0/24 »** pour ce faire, nous allons, sur l’interface **Gi0/2**, interdire les paquets provenant du réseau « **10.1.1.0/24 »**


1 - Après s’être connecté sur le routeur en mode de configuration globale en tapant les commandes "*enable"* puis "*configuration terminal",* on commence par la création de la règle :

Router(config)#access-list 1 deny 10.1.2.0 0.0.0.255

En précisant "access-list 1" on attribue **un ID à notre ACL**, puis ensuite on précise que l'on veut refuser avec "deny", et enfin on précise l'adresse IP de destination (10.1.2.0) et le masque au format inversé appelé *wildcards mask* (0.0.0.225).

**Wildcards mask ?** De quoi parle-t-on ? Une petite explication s’impose. En effet, souvent, l’objectif de mettre en œuvre l’ACL c’est de faire correspondre une plage d’adresse IP (ou un sous-réseau complet) à l'ACL plutôt qu’une seule adresse IP. Cela est faisable à l’aide ce que l’on appelle **Wildcards mask,** ou en français **masque générique (ou inversé).** Notez qu’il ne s’agit pas d’un masque de sous-réseau. Par le biais de cette notation, nous avons **la possibilité d’ignorer des parties de l’adresse IP, comme si elles correspondaient déjà.**

Cela signifie que :

- Quand on met 0 : le routeur doit comparer cet octet comme d’habitude. La présence d'un "0" "figera" la partie correspondante de l'adresse IP
- Lorsqu'on met 255 : le routeur ignore cet octet, considérant qu’il correspond déjà.

C’est-à-dire pour la règle « *deny 10.1.2.0 0.0.0.255* » le routeur va refuser tous les paquets où les trois premiers octets de l'adresse IP source correspondent à ce masque générique. D'autre valeurs sont possible pour le wildcard mask, permettant d'affiner le filtrage en fonction des plages IP ou des regroupements de plages. Ainsi, un wildcard mask en 0.0.255.255 correspondra à un masque en /16; un wildcard mask en 0.0.63.255 correspondra à un masque en /18. Pour calculer la valeur, rien de plus simple, il suffit de soustraire la valeur du masque de sous-réseau à 255.

Exemple :

255.255.255.255

-255.255.255.0

= 0.0.0.255

**On retrouve bien notre wildcard mask correspondant à notre /24.**

Deuxièmement, comme évoqué précédemment, il faut autoriser explicitement les réseaux que l’on veut laisser passer, dans notre exemple c’est « **10.1.3.0/24 ».** À défaut, il n’y aura pas de correspondance entre l’IP source contenue dans l’en-tête du paquet et les règles ACL, ce qui veut dire que le routeur va les refuser implicitement et notre réseau en 10.3.0/24 ne pourra pas non plus communiquer  avec le **10.1.2.0/24.**

Router(config)#access-list 1 permit 10.1.3.0 0.0.0.255

Notez qu'ici, on pourrait utiliser le mot clé "any" à la place de la plage réseau et du wildcard mask. Ce mot clé , qui veut dire "n'importe qui" peut être utilisé lorsque la règle s'adresse à tous les réseaux et hôtes. Comme on à déjà interdit en premier lieu notre premier réseau, on peut très bien estimer que le reste est permis.

Troisièmement, on sélection l’interface concernée par la règle :

Router(config)#interface gigabitEthernet 0/2


Router(config-if)# ip access-group 1 out

Le "1" étant le numéro de l’ACL créée à la première étape, et "out" signifie que la règle s’applique sur les paquets sortants de l’interface.

Nous avons vu les ACLs Standard et leur champ d’application, passons maintenant au deuxième type d’ACLs à savoir les « *ACLs étendues* ».

### B. Les ACL étendues

Les ACL étendues présentent plusieurs similitudes par rapport aux ACL Standards décrites dans la section précédente. Tout comme une ACL standard, **on active les ACL étendues sur les interfaces pour les paquets entrants ou sortants**, puis le routeur cherche dans la liste de manière séquentielle.

Les ACL étendues utilisent également la logique de première correspondance, car dès que la première instruction est mise en correspondance, le routeur arrête la recherche dans la liste d'ACL, en effectuant l’action définie.

En comparaison des ACL standards, **les ACLs étendues vont permettre d'analyser une plus grande variété de champs au sein de l'en-tête d'un paquet**. Cela rend les ACL étendues plus puissantes, plus précises, mais aussi un peu plus complexes.

Les ACL étendues suivent la même logique que les ACL standards, elles sont identifiables par un numéro, allant de **100 à 199** et de **200 à 2699**.

Un exemple sera plus parlant, nous allons créer une règle qui aura pour but d’**interdire le Ping de PC2 vers le PC3, tout en l'autorisant vers le PC1,** en posant les règles sur les sous-réseaux (tous en /24). Mettons ça en place en reprenant la même topologie :

Voici la configuration à appliquer sur notre routeur Cisco :

Router(config)# access-list 100 deny icmp 10.1.3.0 0.0.0.255 10.1.2.0 0.0.0.255
Router(config)# access-list 100 permit icmp 10.1.1.0 0.0.0.255 10.1.2.0 0.0.0.255
Router(config)#interface gig 0/2
Router(config-if)#ip access-group 100 in

Dans l'exemple ci-dessus, "*icmp*" correspond au protocole, en l'occurrence ici c'est un moyen de bloquer le ping. Ensuite, "10.1.3.0" c'est l'adresse IP d'origine suivie de son masque générique, et "10.1.2.0" l'adresse IP de destination suivie aussi de son masque générique.

**le numéro de port source et port de destination**. Les numéros de port identifient le service qui envoie ou reçoit les données. Quand le mot "*tcp*" ou "*udp*" est inclus dans la règle de l'ACL, cela permet de préciser le port source et le port de destination afin d'avoir une règle plus précise.

Voici un exemple :

Router(config)# access-list 100 permit tcp 10.1.1.0 0.0.0.255 10.1.2.0 0.0.0.255 eq 21

Concernant « **eq »** il s’agit d’un opérateur qui veut dire égal, et le numéro de port 21 utilisé par le serveur FTP. On autorise le réseau 10.1.1.0/24 à communiquer sur le port 21 (pour le FTP) avec le réseau 10.1.2.0/24.

Prenons un autre exemple...

L’exemple suivant se concentre sur la compréhension de la syntaxe de base. Ici, l'objectif est de créer une ACL pour empêcher l'utilisateur David d’utiliser le protocole FTP pour se connecter sur les serveurs. Pour cela, on va le bloquer en entrée sur l’interface **Gi0/0** de **R3**, car c’est l’interface la plus proche de la source, donc l’ACL sera associée à cette interface. Quant à l'utilisateur Florian, on va lui interdire l’accès au **serveur Web de serveur1**. Dans cet exemple, on active l’ACL sur **R2** en entrée de l'interface **Gi0/0**.

Voici la topologie :

Je vous propose que l'on se connecte sur **R2** afin de créer la première règle.

La première chose à faire, c’est interdire David d’accéder à tous les serveurs FTP :

access-list 101 deny tcp host 172.16.3.10 172.16.1.0 0.0.0.255 eq ftp

Au sein de la commande ci-dessus, on précise bien l'hôte de David avec "**host 172.16.3.10**", car cela correspond à la source à bloquer selon notre schéma initial.

La deuxième règle va permettre d'autoriser tout le reste du trafic :

access-list 101 permit ip any any

On applique l'ACL à l'interface concernée :

R2(config)# interface gi0/0 
R2(config-if)#ip access-group 101 in

Interdisons maintenant à Florian d’accéder au serveur Web :

access-list 101 deny tcp host 172.16.2.10 host 172.16.1.100 eq www

Note : vous pouvez remarquer que j'ai utilisé le même numéro d'ACL que pour la première réalisée. Cela n'a pas d'importance, car ce numéro est interne au routeur, et je ne suis pas sur le même routeur.


Encore une fois, on autorise tout le reste du trafic ensuite :

access-list 101 permit ip any any

Après avoir créé les règles d'ACL, il nous reste plus qu’à les appliquer toujours à partir du routeur R3 :

R3(config)# interface gi0/0
R3(config-if)#ip access-group 101 in

Voilà, mission accomplie ! 

### C. Named IP Access Lists

Comme vous le savez, toutes les listes d’accès doivent être identifiées par un nom ou un numéro. Ce type de liste d’accès est plus pratique, car on peut spécifier un nom significatif qui est facile à retenir et associer à une règle.

Les ACL nommées peuvent correspondre aux mêmes champs qu’une ACL standard et étendue, cependant, elles présentent trois grandes différences par rapport aux listes de contrôles d’accès numérotées, voyons cela ensemble :

- L’utilisation de noms au lieu de chiffres pour identifier la liste ACL facilite la mémorisation et l'identification de l'ACL.
- Utilisation de sous-commandes ACL, et non de commandes globales, pour définir l'action et les paramètres correspondants.
- Utilisation des fonctionnalités d'édition de l'ACL qui permettent de supprimer des lignes individuelles de l'ACL et d'insérer de nouvelles instructions à une liste d'accès nommée.

On va reproduire le même schéma :

Cette fois-ci, on va changer un peu les règles du jeu : le but sera d’interdire David d’accéder au serveur Web (Serveur1) via le protocole http. On aura besoin donc d’une ACL qui contiendra plusieurs informations : IP source, IP destination et un protocole bien précis. Pas le choix, il faudrait utiliser une ACL étendue.

Profitons de cet exemple pour utiliser une ACL nommée et poursuivre notre découverte des ACLs.

Sur le R1, tout d’abord, on crée l’ACLs avec le nom "David-HTTP" :

`R2(config)# ip access-list extended David-HTTP`
Puis, on se retrouve dans cette ACL, donc on ajoute notre règle :

`R2(config-ext-nacl)#deny tcp host 172.16.3.10 host 172.16.1.100 eq www`
Vous constatez bien que le prompt a changé, car on se trouve dans l’invite de configuration de l’ACL nommée. On pose une interdiction via la directive "deny" depuis l’adresse source "172.16.3.10" vers le serveur web avec l'adresse IP "172.16.1.100". **« www »** c’est pour faire référence au service web, mais on peut bien évidement mettre le port 80 au lieu de www.

En dernier lieu, on autorise toutes les autres connexions, car souvenez-vous, la directive implicite (deny any ) se trouve à la fin de chaque ACL !

R1(config-ext-nacl)#permit ip any any

Il ne reste plus qu’à associer l’ACL à l’interface G0/0 de R2 comme vous savez le faire maintenant. Vous pouvez retrouver d'autres exemples sur le site de Cisco.

## V. Placement des ACL

Bon, nous savons maintenant qu'il y a deux types d'ACL : standards et étendues. On sait aussi qu'il y a deux possibilités pour les placer : en entrée ou en sortie d'une interface. Mais alors, où placer tel type d'ACL? Sur quel routeur? Quelle interface?

Dans tous les cas, il existe une règle simple et immuable : **une ACL standard se placera toujours au plus proche de la destination, une ACL étendue se placera toujours au plus proche de la source.**

Pourquoi? Tout simplement, car une ACL standard, comme vous avez pu le constater, est très restrictive (blocage de tout le trafic d'un réseau ou d'un hôte), un placement au plus proche de la source risque de limiter fortement les possibilités de celles-ci. En revanche, l'ACL étendue filtrant au niveau de la couche 4 (ports TCP/UDP), son placement au plus proche de la source permet d'éviter de faire transiter des paquets qu'on ne souhaite pas voir sur le réseau, ainsi, on économise du temps de calcul.

Un petit exemple, en reprenant le schéma de la partie II :

Admettons que le PC1 soit la source, à savoir que c'est à partir de lui que je vais déterminer mes règles. Je veux interdire à ce PC1 d'accéder au réseau du serveur S1, je vais donc placer l'ACL standard sur l'interface **Gi 0/3** de **R2 en sortie** **.** En revanche, si je veux interdire à PC1 d'accéder en SSH au serveur S1, je vais placer mon ACL étendue sur l'interface **Gi 1/0** de **R1 en entrée.**

## VI. Conclusion

Nous avons vu ensemble **comment les ACLs peuvent être utiles pour contrôler le réseau afin de filtrer certains flux**. Les ACL permettent un filtrage au niveau du réseau et peuvent être complétées avec le filtrage applicatif (Proxy) qui va venir **au niveau de la couche Application du modèle OSI.** Le filtrage de paquet via **ACLs opère niveaux 3 et 4 du modèle OSI**, ce qui permet de faire déjà beaucoup de choses, mais peu ne pas suffire dans certains cas.

*Merci à Florian Duchemin pour sa relecture technique avant publication.*

Bonjour,

Déjà super tutoriel, merci pour le temps passé dessus.

J’ai cependant l’impression qu’une erreur s’est glissée dans le texte, à ce niveau

« Je vous propose que l’on se connecte sur R2 afin de créer la première règle. »

Sauf erreur de ma part, je pense qu’il s’agit en réalité du R3 dont on parle.

Merci pour le boulot que vous faites, le site est une vraie bible.

Merci ça m’a un peu embrouillé aussi

tout est mélangé notamment sur les ACL standard

dans celle ci on indique la source du paquet IP, or dans les exemples de conf c’est la destination.

cela apporte trop de confusions pour un lecteur non assidu,

vous auriez peut etre du vous relire

A propos des ACL étendues,

il y’aurait une erreur de frappe:

<>

=> [100;199]et[2000;2699] au lieu de [100;199]et[200;2699].

bonjour,

excellent tutoriel.

il y a une erreur sur l’acl etendue.

vous avez inversé la config entre le routeur R2 et R3 david florient

Merci beaucoup pour vos tutos , cependant y’a beaucoup d’erreur je pense , par exemple sur les acls étendus , y’a confusion entre l’ip source et l’ip de destination à plusieurs endroits, ça embrouille

Bonjour, je pense qu’il y a une petite confusion au niveau de la configuration de l’ACL standard.

« nous allons, sur l’interface Gi0/2, interdire les paquets provenant du réseau « 10.1.1.0/24 » »

« Router(config)#access-list 1 deny 10.1.2.0 0.0.0.255 »

cette configuration est appliquée à Gi0/2 en sortie.

N’est-ce pas plutôt « »access-list 1 deny 10.1.1.0 0.0.0.255″ » appliquée à Gi0/2 en sortie ?