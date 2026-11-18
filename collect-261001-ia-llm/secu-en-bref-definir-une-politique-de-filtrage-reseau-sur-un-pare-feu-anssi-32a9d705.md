Sécu en bref : définir une politique de filtrage réseau sur un pare-feu selon l’ANSSI
Cet article décrypte les points clés du guide ANSSI "Recommandations pour la définition d'une politique de filtrage réseau d'un pare-feu" pour vous aider à déterminer la bonne stratégie de filtrage à appliquer sur votre pare-feu.
Vous souhaitez mettre en place une politique de filtrage stricte et efficace sur un SI, mais ne savez pas par où commencer ? Nous allons ici voir les principales bonnes pratiques à travers la revue du guide "Recommandations pour la définition d’une politique de filtrage réseau d’un pare-feu".
Ce guide de l'ANSSI permet d'obtenir les éléments organisationnels pour construire une politique de filtrage correctement structurée. Cela signifie qu’il vous guide dans la conception de votre politique de filtrage en vous indiquant les bonnes pratiques à adopter et celles à éviter.
Il contient 15 recommandations à utiliser comme guidelines et check-list pour valider que la conception et la mise en place de votre politique de filtrage respectent les bonnes pratiques de sécurité.
L'objectif étant de rendre votre politique de filtrage robuste, cohérente et plus facile à maintenir dans le temps. Voyons ici les principales idées et les apprentissages de ce document.
Retrouvez d'autres articles de la série Sécu' en Bref, parmi lesquels :
- Sécu’ en bref – ANSSI : bien architecturer un système de journalisation
- Sécu’ en bref – ANSSI : l’externalisation des SI
Règles, politique, pare-feu et filtrage
Avant de commencer à identifier les principaux points d'attention concernant les bonnes pratiques de conception d'une politique de filtrage, commençons par un bref rappel des termes qui s'y rapportent :
- Règle de filtrage : c'est une instruction appliquée par un pare-feu pour autoriser ou interdire un flux réseau en fonction de critères précis : adresse source, adresse destination, protocole, port, etc. Elle est évaluée séquentiellement dans le cadre d’une politique de filtrage.
- Pare-feu : équipement d’interconnexion qui contrôle les flux réseau entre des zones de confiance distinctes (ex : réseau interne, DMZ, internet). Il applique une politique de filtrage.
- Politique de filtrage : ensemble structuré de règles de filtrage appliquées à un pare-feu. Elle définit quels flux sont autorisés ou interdits entre les différentes zones d’un système d’information. Une politique de filtrage doit être cohérente, documentée et régulièrement révisée.
- Filtrage : processus technique d’analyse et de contrôle des flux réseau en fonction de critères prédéfinis (adresses IP, ports, protocoles). C'est la décision du pare-feu sur chaque paquet qu'il fait transiter.
- Segmentation : division d’un réseau en sous-réseaux ou zones logiques distinctes, afin de limiter la propagation des attaques et de réduire la surface d’exposition. Exemple : séparation des réseaux utilisateurs, serveurs, et invités.
- Cloisonnement : terme général qui combine la segmentation et le filtrage. Il désigne l’ensemble des mesures visant à isoler les zones du réseau et à contrôler les communications entre elles pour réduire la surface d’attaque.
Ces trois dernières notions sont explorées plus en détail dans l'article suivant :
La complexité des SI actuels et la multitude de réseaux, sous-réseaux et pare-feu qui le composent rendent ardue la conception d'une politique de filtrage efficace. Cependant, cette politique de filtrage est un composant clé de la défense en profondeur du SI.
- Elle limite les mouvements latéraux en cas de compromission.
- Elle réduit la surface d’attaque en n’autorisant que les flux strictement nécessaires.
- Elle facilite la détection des anomalies via la journalisation.
Pour aller plus loin, consultez notre article concernant la défense en profondeur.
La définition d'une bonne politique porte donc à la fois sur l'organisation et la cohérence de celle-ci lors de sa mise en place, mais aussi sur la possibilité de la maintenir dans le temps, en anticipant des évolutions certaines dans l'équipe informatique, les besoins opérationnels, les incidents techniques, les audits, les changements de technologies, etc.
Voyons à présent quelles sont ces bonnes pratiques.
Bien organiser sa politique de filtrage réseau
Le guide de l'ANSSI que nous résumons ici présente un modèle clair de conception d'une politique de filtrage réseau selon lequel tout ce qui n'est pas explicitement autorisé est interdit.
Cette approche "liste blanche" permet d'être sûr de ne rien oublier et de ne rien autoriser par omission. Elle est souvent la plus stricte, la plus efficace, mais aussi celle qui demande le plus de préparation avant sa mise en place.
Le tableau suivant nous est fourni afin de traiter les règles de filtrage par catégorie, mais aussi dans le bon ordre :
Dans ce modèle d'organisation, nous retrouvons notamment :
- Les règles de contrôle et protection du pare-feu lui-même (Section 1, 2 et 3). Il s'agit d'un composant du SI qui a, lui aussi, ses faiblesses, sa surface et ses vecteurs d'attaque. On parle notamment de ses services d'administration et de monitoring qui sont nécessairement actifs, mais aussi de l'envoi des logs, alertes et sauvegardes.
# Exemple de règle Section 1
Source : serveurs d'administration
Destination : Pare-feu
Protocole : SSH/HTTPS
Action : Autoriser
Journalisation : Oui
# Exemple de règle Section 2
Source : Pare-feu
Destination : Serveur de centralisation des logs
Protocole : TCP/514
Action : Autoriser
Journalisation : Oui
# Exemple de règle Section 3
Source : Any
Destination : Any
Service : Any
Action : Interdire
Journalisation : Oui
Notez que toutes les règles qui concernent le pare-feu lui-même sont journalisées, cela parce qu'il s'agit d'un composant critique et central du SI. Vous constaterez dans le guide que ce n'est pas le cas des règles d'autorisation des flux métiers, ce qui serait bien trop verbeux.
- Les règles d'autorisation des flux métiers et des services (Section 4) : c'est ici que les composants de la "liste blanche" seront définis. Elle contient donc tous les flux métier identifiés comme nécessaires au bon fonctionnement du SI, d'un système, service ou composant.
# Exemple de règle Section 4
Source : Zone Utilisateur
Destination : Active Directory
Service : Kerberos (TPC/88)
Action : Autoriser
Journalisation : Non
Même s'il s'agit d'une règle d'autorisation, il convient toujours de contrôler et de limiter au strict minimum la source, la destination et le type (protocole, ports) des flux. Ainsi, il sera très rare d'utiliser la directive "Any" sur des règles d'autorisation qui ciblent un ensemble d'objets sans restrictions.
- Les règles antiparasite (Section 5) qui visent à bloquer les flux non autorisés dont la trace (log) n'est volontairement pas conservée. L’objectif est de maintenir des journaux exploitables, de supprimer le bruit, sans perdre la visibilité sur les incidents de sécurité.
# Exemple de règle Section 5
Source : Réseau de test
Destination : 255.255.255.255
Service : UDP/137, UDP/138
Action : Interdire
Journalisation : Non
- La règle d'interdiction finale : à ne surtout pas oublier, c'est elle qui indique que tout ce qui n'a pas été explicitement autorisé par les règles précédentes est forcément bloqué, et journalisé.
# Exemple de règle Section 6
Source : Any
Destination : Any
Service : Any
Action : Interdire
Journalisation : Oui
Dans ce contexte, la journalisation des flux bloqués présente un intérêt en termes de sécurité. Les flux bloqués résultent forcément d'une activité non prévue, ou non nécessaire. Il est donc généralement utile de regarder fréquemment ces journaux afin de constater soit un incident, soit un composant qui ne devrait pas échanger depuis telle source vers telle destination et sur tel protocole (oubli, ou composant inutile ?).
Il est intéressant de relever cette remarque du guide concernant ce flux explicite de blocage et de journalisation :
Certaines solutions techniques appliquent automatiquement une règle d’interdiction à la fin de la politique de filtrage, mais celle-ci n’est généralement pas journalisée ou n’apparaît pas explicitement à la fin de la politique ; c’est la raison pour laquelle une règle finale explicite est ajoutée dans tous les cas. - Source - ANSSI
Pensez donc bien à ne pas utiliser la règle implicite de blocage, mais à déclarer votre propre règle d’interdiction finale, en activant également sa journalisation.
Comment mettre en forme sa politique de filtrage
Au-delà des règles de filtrage elles-mêmes, il est important d'avoir des règles de gestion commune de celles-ci. L'objectif ? S'assurer que n'importe quel membre d'une équipe puisse intervenir sur n'importe quel pare-feu, et qu'il puisse comprendre leur organisation, leur définition et leur raison d'être.
Ainsi, le guide de bonnes pratiques de l'ANSSI souligne l'importance d'adopter des règles de nomenclature, mais aussi de coloration et de description de ces dernières.
Les quelques règles décrites permettront par exemple de faciliter la recherche, la manipulation et la revue (audit) des règles.
Concernant la coloration, lorsque le pare-feu le permet, on peut par exemple adopter une convention telle que :
- Rouge : concerne le réseau externe
- Orange : concerne la DMZ publique
- Jaune : concerne la DMZ privée
- Vert : réseau interne
Il ne s'agit bien sûr que d'un exemple, la réalité et la conception de votre réseau pouvant vous amener à faire des choix différents.
La nomenclature des règles, c'est-à-dire la structure qui régit le nom qui leur est donné, a aussi une grande importance. Elle permet notamment d'identifier rapidement les caractéristiques techniques ou fonctionnelles de la règle (composants ou protocoles concernés). L'important étant bien sûr d'avoir des règles de nommage communes au sein d'une même équipe.
Enfin, le champ commentaire de chaque règle ne doit dans l'absolu jamais être vide. On peut par exemple y indiquer la date d'implémentation/mise à jour de la règle, la raison de sa mise en place, son éventuelle date d'expiration, le nom de la personne l'ayant mise en place, etc.
Il faut absolument éviter les règles de filtrage dont l’utilité n’est plus claire, mais qui sont conservées par crainte de provoquer un dysfonctionnement quelque part dans le SI. C'est généralement ce qui mène à l'accumulation de règles d’autorisation, ce qui casse complètement la segmentation du SI.
L'idéal est donc de mettre en place une convention commune de création, nommage et description des règles qui facilite leur compréhension par chaque personne susceptible d'intervenir sur vos pare-feu. Cela facilitera au passage la revue des règles par des tiers, par exemple lors d’un audit de sécurité, de l’intervention d’un prestataire sur vos pare-feu, etc.
Les bonnes pratiques et la documentation
Pour terminer, ce guide contient un certain nombre de bonnes pratiques globales qu'il vaut mieux avoir en tête lorsque l'on met en place une politique de filtrage.
Par exemple, la désactivation des flux implicites : il s'agit de "groupements" de flux préconçus qui embarquent souvent plus de protocoles ou de services que réellement nécessaires. Lorsqu'ils sont utilisés, on se retrouve alors à ajouter dans notre liste blanche des protocoles non utilisés, ce qui pourrait ouvrir des chemins de compromission plus tard.
Également, un chapitre entier est dédié à la documentation et au maintien dans le temps de notre politique de filtrage. Cette documentation vise notamment à formaliser les choix de nomenclature, d'organisation et de structure des règles décrites précédemment. Bien que des bonnes pratiques soient proposées, il reste nécessaire de les adapter à chaque contexte, et donc d'avoir une documentation.
Deux éléments sont mentionnés dans le guide de l'ANSSI concernant cette documentation : sa fréquence de revue et sa validation pratique (tests, outils d'analyse réseau).
Pour illustrer l'ensemble des bonnes pratiques de ce document, l'ANSSI nous propose la capture d'écran suivante :
On identifie notamment l'organisation par catégories (les 6 sections), la description des règles avec une date, etc.
Conclusion
Je vous laisse le soin d'aller consulter l'entièreté du document pour avoir des informations plus complètes sur ces bonnes pratiques. Il s’agit d’un guide pratique très intéressant et qui sera à coup sûr utile dans toutes les équipes informatiques et entreprises.
Pour aller plus loin et notamment procéder au nettoyage d'une politique de pare-feu existante, ce qui est un exercice encore plus périlleux, sachez qu’il existe aussi un guide de l’ANSSI pour vous accompagner dans cet exercice :
N’hésitez pas à utiliser les commentaires pour nous faire un retour sur l’utilisation et l’application de ces guides de l’ANSSI dans votre contexte, ou à nous indiquer si vous souhaitez plus de résumés de ce type sur IT-Connect !
Bonjour,
Lien au début de l’article down
Bonjour Tib,
Merci pour l’info, je vais corriger ça. L’ANSSI vient de déplacer ses guides sur un nouveau sous-domaine et visiblement ce lien n’a pas suivi.
Bonjour,
L’article de l’ANSSI date de 2016, n’y a-t-il pas eu une évolution sur les recommandations en 10 ans ? Surtout avec l’évolution des techniques d’attaques.
Mais cela reste un article très intéressant et un sujet au cœur de l’actualité !
Merci
Bonjour
L’exemple de la section 3 empèche l’éxécution de la section 4 et 5. Si source et destination sont à ANY, tous les flux seront bloqués quoi qu’on mette ensuite dans les autres sections. Si on se réfère à l’exemple Figure1 en fin d’article, la destination serait plutot: Destination : Pare-feu voir Pare-feu-any pour prendre en compte toutes les interfaces du pare-feu.
# Exemple de règle Section 3
Source : Any
Destination : Any
Service : Any
Action : Interdire
Journalisation : Oui