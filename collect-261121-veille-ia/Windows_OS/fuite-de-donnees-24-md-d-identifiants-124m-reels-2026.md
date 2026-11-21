---
id: collect-261121-veille-ia/windows-os/fuite-de-donnees-24-md-d-identifiants-124m-reels-2026
title: "Fuite de Données : 24 Md d'Identifiants, 124M Réels [2026]"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-06-15"]
keywords: ["donnees", "millions", "identifiants", "fuite", "milliards", "cette", "mots", "passe"]
source: https://tech-insider.org/fr/fuite-donnees-24-milliards-identifiants-hibp-2026/
source_anchor: ""
source_lines: [1, 159]
section: "Windows_OS"
---
Le 15 juin 2026, Have I Been Pwned a ajouté à sa base un nouveau jeu de données baptisé « June 2026 Stealer Logs ». Le chiffre affiché à côté donne le vertige : 24 milliards de lignes d’identifiants. Mais ce chiffre brut cache une réalité plus nuancée, et c’est justement cette nuance qui occupe les professionnels de la cybersécurité en France et en Europe cette semaine.

Derrière l’annonce se trouve un cluster Elasticsearch resté ouvert sur internet, repéré par les chercheurs de Cybernews, et rempli de journaux volés par des logiciels infostealers. Une fois dédupliquées, les données représentent 56,3 millions d’adresses e-mail uniques et 124 millions de mots de passe uniques : un total déjà considérable, mais dix fois inférieur à l’annonce initiale. Cette fuite de données s’inscrit dans une tendance longue, celle des compilations géantes d’identifiants volés qui se multiplient depuis 2019, dont la taille brute sert de plus en plus d’argument de communication pour ceux qui les découvrent.

Cet article détaille ce que l’on sait de cette fuite de données, pourquoi l’écart entre 24 milliards et 124 millions compte autant, ce que cela change pour les entreprises et les particuliers européens, et comment vérifier concrètement son exposition.

## Que révèle la fuite « June 2026 Stealer Logs » ajoutée à Have I Been Pwned

Le nom donné par Have I Been Pwned (HIBP) résume bien la nature du jeu de données. Il ne s’agit pas de mots de passe issus d’un piratage unique, mais de journaux collectés par des logiciels malveillants de type infostealer sur des ordinateurs infectés dans le monde entier. Ces programmes aspirent tout ce qu’ils trouvent dans le navigateur d’un poste compromis, identifiants enregistrés et cookies de session actifs compris, parfois même des captures d’écran ou l’historique du presse-papiers.

Troy Hunt, qui exploite Have I Been Pwned depuis 2013, a intégré le jeu de données le 15 juin 2026 sous le nom « June 2026 Stealer Logs ». Environ 56 millions d’utilisateurs peuvent désormais vérifier leur exposition dans la section dédiée aux stealer logs de leur tableau de bord, et les organisations disposent d’un accès via l’API réservée aux domaines professionnels.

Le dataset ne provient pas d’une entreprise unique piratée. Il agrège des identifiants volés sur des appareils infectés partout dans le monde, compilés à partir d’environ 36 sources distinctes : anciennes compilations de fuites, dépôts publiés sur des forums de piratage, et canaux Telegram spécialisés dans la revente de données bancaires et d’identifiants. Ces derniers représenteraient à eux seuls près de 1,7 milliard des lignes brutes du cluster.

## 24 milliards de lignes, 124 millions de mots de passe : d’où vient l’écart

Le chiffre de 24 milliards, largement repris, correspond au nombre brut de lignes présentes dans le cluster Elasticsearch, soit environ 8,3 téraoctets de données. Une grande partie de ce volume est redondante. Les mêmes identifiants apparaissent des dizaines, parfois des centaines de fois, à force d’être recopiés d’une compilation à l’autre depuis plusieurs années.

Une fois le nettoyage effectué, HIBP retient 56,3 millions d’adresses e-mail uniques et 124 millions de mots de passe uniques. C’est cet écart qui a poussé plusieurs analystes à qualifier le chiffre de 24 milliards de trompeur, davantage un argument de communication qu’une mesure fidèle du risque réel. Une analyse de Phishing Tackle consacrée à l’incident souligne que la majorité des identifiants stockés dans ce type de cluster ont déjà circulé ailleurs, ce qui réduit leur nouveauté opérationnelle pour les attaquants, mais pas leur dangerosité pour les victimes qui n’ont toujours pas changé leurs mots de passe.

Cette confusion entre volume brut et exposition réelle n’est pas nouvelle. Elle explique en partie pourquoi les titres autour des fuites de données massives restent difficiles à interpréter pour le grand public, et pourquoi la lecture des chiffres dédupliqués demeure la seule façon fiable d’évaluer un risque individuel.

## Comment Cybernews a mis au jour le cluster non sécurisé

Mi-juin 2026, les chercheurs de Cybernews sont tombés sur un cluster Elasticsearch accessible sans authentification. Ce type d’erreur de configuration, banal mais dévastateur, revient régulièrement dans l’actualité de la cybersécurité. Une base de données pensée pour un usage interne, mise en ligne sans mot de passe ni pare-feu, devient consultable par n’importe qui muni de la bonne adresse IP.

Une fois le signalement effectué, le cluster a été mis hors ligne. Mais le temps qu’il reste exposé suffit largement. Des scanners automatisés parcourent en permanence les plages d’adresses IP publiques à la recherche de ce genre d’oubli, si bien qu’une base non protégée peut être repérée par des acteurs malveillants en quelques heures à peine, bien avant qu’un chercheur en sécurité ne la signale publiquement.

Aucune action réglementaire ou judiciaire spécifique à cet incident n’a été rendue publique à ce stade. Il ne s’agit pas d’un piratage d’entreprise identifiable, ce qui complique la question de la responsabilité : qui doit notifier qui, quand la fuite de données est elle-même une compilation de fuites déjà anciennes ?

## Qu’est-ce qu’un stealer log, et pourquoi ces compilations explosent

Un infostealer est un logiciel malveillant conçu pour un seul objectif : extraire discrètement tout ce qui a de la valeur sur un poste infecté, puis l’envoyer à son opérateur. Contrairement à un rançongiciel, il ne chiffre rien et ne réclame aucune rançon. Il agit en silence, souvent pendant des semaines, avant que la victime ne se rende compte de quoi que ce soit.

Le marché de l’infostealer a connu une croissance spectaculaire. Selon les données de Flashpoint, environ 1,8 milliard d’identifiants ont été volés en 2025 via des appareils infectés par ce type de malware, un total obtenu à partir de 5,8 millions de machines compromises, soit une hausse de 800 % sur une période de quatre mois seulement. Le Data Breach Investigations Report 2026 de Verizon, qui couvre les incidents survenus entre novembre 2024 et octobre 2025 et reste, en juin 2026, la référence du secteur, confirme cette dynamique en plaçant les identifiants volés parmi les vecteurs d’intrusion les plus cités. Cette explosion alimente directement les compilations comme celle découverte en juin 2026 : plus il y a d’appareils infectés, plus les journaux volés s’accumulent sur les forums et les canaux Telegram, et plus il devient tentant pour quelqu’un de les regrouper en un seul jeu de données consultable.

## Vérifier son exposition sur Have I Been Pwned

La meilleure façon de savoir si l’on figure dans cette fuite de données reste de consulter directement Have I Been Pwned avec son adresse e-mail. Le service, gratuit pour un usage individuel, indique précisément si une adresse apparaît dans le jeu de données « June 2026 Stealer Logs » ou dans l’une des centaines d’autres compilations déjà indexées.

Pour les mots de passe, HIBP propose depuis plusieurs années une API en k-anonymat, qui permet de vérifier si un mot de passe a fuité sans jamais transmettre ce mot de passe en clair. Le principe : on hache le mot de passe en SHA-1, on envoie uniquement les cinq premiers caractères du hash, et le service retourne toutes les correspondances possibles pour que la comparaison finale se fasse en local, côté client.

`curl https://api.pwnedpasswords.com/range/5BAA6`
Cette requête renvoie la liste des suffixes de hash correspondant au préfixe 5BAA6, avec le nombre de fois où chaque mot de passe a été vu dans des fuites connues. C’est justement le préfixe que donne le mot de passe « password » une fois haché, ce qui explique pourquoi il revient si souvent en exemple dans la documentation technique de développeurs.

## Historique des grandes compilations d’identifiants volés

Pour resituer l’ampleur de l’incident, voici comment la compilation de juin 2026 se compare aux principales méga-fuites recensées depuis 2019.

| Compilation | Année | Volume annoncé | Nature des données | 
|---|---|---|---|
| Collection #1 | 2019 | 773 millions | Emails et mots de passe compilés | 
| COMB (Compilation of Many Breaches) | 2021 | 3,2 milliards | Emails et mots de passe compilés | 
| Mother of All Breaches (MOAB) | 2024 | 26 milliards | Compilation multi-sources | 
| Compilation infostealer Cybernews | Juin 2025 | 16 milliards (brut) | Journaux infostealer | 
| June 2026 Stealer Logs | Juin 2026 | 24 Md brut / 124M mots de passe uniques | Journaux infostealer (Cybernews / HIBP) | 

La tendance est claire : les chiffres bruts gonflent d’année en année, mais l’écart entre volume brut et données réellement uniques grandit tout autant, à mesure que les mêmes identifiants sont recopiés d’une compilation à l’autre.

## Le marché noir des identifiants volés

Derrière chaque ligne de ce type de compilation se trouve une chaîne économique bien rodée. Un appareil s’infecte, souvent via un logiciel piraté ou une pièce jointe piégée. Le malware infostealer collecte les identifiants stockés dans le navigateur et les envoie à un serveur de commande. Ces journaux bruts sont ensuite vendus à l’unité ou par lot sur des forums spécialisés, puis republiés, recopiés, et mélangés à d’anciennes fuites sur des canaux Telegram dédiés au commerce de données bancaires et d’accès.

C’est cette republication en boucle qui explique pourquoi 1,7 milliard des lignes retrouvées dans le cluster de juin 2026 provenaient de canaux Telegram. Ces canaux fonctionnent comme des points de distribution, où les mêmes journaux circulent, se combinent avec d’autres, et gonflent artificiellement le volume total sans ajouter de victimes réellement nouvelles.

## Ce que cela change pour la France et l’Europe

Les chercheurs à l’origine de la découverte n’ont pas publié de répartition par pays pour ce jeu de données précis. Mais le contexte européen reste déterminant pour comprendre les conséquences. Le Règlement général sur la protection des données impose des obligations de notification aux entreprises victimes d’une violation touchant des données personnelles, et la directive NIS2, en cours de transposition dans plusieurs États membres, durcit encore les exigences pour les secteurs jugés critiques.

Cette compilation arrive alors que la France a déjà été confrontée à une série d’incidents distincts en 2026, dont la mise en vente de 250 000 passeports et cartes d’identité français sur le dark web. Le ministère de l’Intérieur a ainsi annoncé en avril 2026 une divulgation de données touchant l’ANTS et concernant 11,7 millions de comptes, tandis que shattered.io a signalé, en février 2026, l’exposition d’environ 15 millions de personnes liées à Cegedim Santé et de 1,2 million d’enregistrements associés à la DGFiP/FICOBA, dans la foulée d’une fuite ayant déjà touché 1,6 million de personnes chez France Travail en décembre 2025. Le pays est aussi partie prenante d’un contentieux européen : la France et trois autres pays ont été renvoyés devant la Cour de justice de l’Union européenne pour retard dans la transposition de NIS2, ce qui illustre la difficulté des États membres à faire appliquer uniformément ces règles pendant que le volume de données volées continue de croître, comme le rappelle l’agence européenne de cybersécurité ENISA.

## Comparaison avec les autres grandes fuites de 2026

La compilation de juin 2026 n’est pas un cas isolé. Elle s’ajoute à une série d’incidents distincts touchant des organisations et des citoyens européens au cours de l’année.

| Incident | Période | Échelle | Type | 
|---|---|---|---|
| June 2026 Stealer Logs | Juin 2026 | 124M mots de passe uniques (24 Md brut) | Compilation infostealer | 
| Passeports français sur le dark web | Juin 2026 | 250 000 documents | Vente de documents d’identité | 
| ShinyHunters vs Commission européenne | 2026 | 350 Go de données | Intrusion cloud AWS | 
| Faille Oracle PeopleSoft | 2026 | Environ 100 organisations ciblées | Vulnérabilité CVSS 9,8 | 

La juxtaposition de ces incidents montre une même logique à l’œuvre. La donnée volée circule, se recombine, et repart. Le groupe de revendication Ostraca a par exemple annoncé le 10 juin 2026 détenir 3,6 millions d’enregistrements issus de CapiFrance, avant de revendiquer, le 2 juillet 2026, 3,8 millions de personnes et 966 Go de documents liés à IAD Group ; plus récemment encore, FrenchBreaches a signalé le 18 août 2026 une fuite touchant Foodtrack.fr. Contrairement à la fuite de passeports français, qui vise des documents directement exploitables pour de la fraude, ou à l’intrusion attribuée à ShinyHunters contre la Commission européenne, qui cible une institution précise, la compilation de juin 2026 n’a pas de victime unique désignée. Elle agrège les dégâts de milliers d’infections individuelles, ce qui la rend à la fois moins spectaculaire à raconter et plus difficile à corriger d’un coup.

## 91 % de données déjà vues : le recyclage permanent des identifiants

Un travail publié par le chercheur Benjamin Brundage, de la société Synthient, sur un jeu de données comparable de 183 millions d’identifiants, a montré que 91 % de ces identifiants avaient déjà été observés dans des fuites antérieures. L’essentiel de ce qui circule aujourd’hui n’est donc pas neuf. Ce sont souvent les mêmes mots de passe, parfois vieux de plusieurs années, qui repassent de main en main entre acteurs malveillants.

Ce recyclage a une conséquence directe pour les utilisateurs qui réutilisent un même mot de passe sur plusieurs services. Chaque nouvelle compilation, même composée en majorité d’anciennes données, leur fait courir un risque de bourrage d’identifiants (credential stuffing) tant qu’ils n’ont pas changé ce mot de passe partout où ils l’utilisent.

## La riposte des autorités : Europol et l’opération Endgame

Face à cette économie de la donnée volée, les autorités européennes ne restent pas inactives. Europol a mené l’opération Endgame, une action coordonnée contre les infrastructures utilisées pour diffuser des logiciels infostealers et d’autres familles de malwares, qui a permis la saisie de 326 serveurs utilisés par des réseaux criminels. Cette opération illustre la stratégie privilégiée par les forces de l’ordre européennes : s’attaquer à l’infrastructure de distribution plutôt qu’à chaque fuite de données individuellement, sur le principe qu’un serveur de commande démantelé coupe l’alimentation de dizaines de compilations futures.

Le problème, reconnu par les chercheurs en sécurité, est que ces infrastructures se reconstituent rapidement. Les groupes derrière les infostealers changent de nom, migrent vers de nouveaux hébergeurs, et reprennent leur activité en quelques semaines. La compilation de juin 2026, qui inclut des données bien antérieures à l’opération Endgame, montre que le stock de données déjà volées continue de circuler longtemps après le démantèlement d’un réseau.

## Have I Been Pwned face à la concurrence

Have I Been Pwned reste la référence gratuite pour un particulier qui veut vérifier une adresse e-mail ou un mot de passe. Mais pour une entreprise qui doit surveiller l’exposition de ses employés en continu, plusieurs plateformes spécialisées existent. Des services comme SpyCloud ou Hudson Rock, cité par des chercheurs pour sa fonctionnalité Cavalier, se concentrent spécifiquement sur la détection de journaux infostealers et proposent un accès pensé pour les équipes de sécurité, avec des alertes en continu plutôt qu’une simple vérification ponctuelle.

La différence tient surtout à l’usage. HIBP fonctionne comme un service de vérification à la demande, gratuit et ouvert à tous, tandis que les plateformes destinées aux entreprises intègrent la détection d’identifiants volés directement dans des flux de renseignement sur la menace, avec une couverture plus large des sources fermées, comme les forums privés ou les canaux Telegram non publics, que ce qu’un particulier peut consulter seul.

## Obligations des entreprises : RGPD et NIS2

Pour une entreprise, découvrir que des employés figurent dans une compilation comme celle de juin 2026 ne déclenche pas automatiquement une obligation de notification à la CNIL, qui a pourtant été notifiée de 6 167 violations de données rien qu’en 2025 selon un rapport rendu public en mai 2026, un volume qui donne la mesure des incidents que l’autorité traite déjà indépendamment de ce type de compilation. Le RGPD s’applique aux violations touchant les données que l’entreprise elle-même détient et contrôle, pas aux identifiants volés sur l’ordinateur personnel d’un salarié infecté par un malware. En revanche, si ces identifiants réutilisés donnent ensuite accès à des systèmes professionnels, l’incident qui en découle peut, lui, relever d’une obligation de notification classique.

C’est précisément ce point d’entrée que ciblent la plupart des attaques par bourrage d’identifiants : un mot de passe professionnel réutilisé sur un service personnel compromis devient la porte d’entrée d’une intrusion bien plus large. Les équipes de sécurité interrogent de plus en plus systématiquement des bases comme HIBP pour les domaines de messagerie de leur organisation, une pratique que la directive NIS2 encourage indirectement en renforçant les obligations de gestion des risques pour les entités essentielles et importantes.

## Comment se protéger concrètement

Pour un particulier, la première mesure reste de ne jamais réutiliser un mot de passe d’un service à l’autre. Un gestionnaire de mots de passe reste l’outil le plus simple pour y parvenir sans effort de mémorisation, avec des solutions gratuites ou payantes, y compris des options auto-hébergées comme Vaultwarden pour les utilisateurs qui préfèrent garder le contrôle total de leurs données.

L’activation de l’authentification à deux facteurs limite aussi fortement l’impact d’un mot de passe volé, puisqu’elle exige une seconde preuve d’identité que l’attaquant ne possède pas. Pour les organisations, la mise en place d’un système d’authentification unique comme Keycloak permet de centraliser les accès et de révoquer rapidement une session compromise, plutôt que de devoir changer des dizaines de mots de passe séparés un par un.

Enfin, la vigilance face aux logiciels piratés et aux pièces jointes non sollicitées reste la meilleure prévention contre l’infection initiale, puisque c’est précisément là que commence la chaîne qui alimente ce type de compilation.

## Ce qu’il faut attendre dans les prochains mois

Plusieurs évolutions semblent probables dans les mois qui suivent cette fuite de données.

- **De nouvelles compilations à venir.** D’autres jeux de données de taille comparable ou supérieure devraient être signalés avant la fin de l’année, portés par la croissance continue du volume d’infections par infostealer observée depuis 2025.
- **Plus de transparence sur les chiffres.** Les services de surveillance d’identifiants volés, HIBP en tête, devraient continuer à distinguer plus clairement volume brut et exposition réelle dans leurs communications, pour limiter les titres trompeurs autour de chiffres comme les 24 milliards de juin 2026.
- **Une pression réglementaire accrue.** L’application progressive de NIS2 devrait s’intensifier en Europe, notamment dans les pays encore en retard de transposition, dont la France.
- **L’accélération des passkeys.** L’adoption des clés d’accès, qui suppriment le mot de passe traditionnel, devrait s’accélérer chez les grands fournisseurs de services grand public, précisément parce qu’un identifiant qui n’existe plus ne peut plus être volé par un infostealer.
- **De nouvelles opérations coordonnées.** D’autres actions de type Endgame sont probables, mais la réapparition rapide d’infrastructures similaires laisse penser que le phénomène des méga-compilations n’est pas près de s’arrêter.

## Foire aux questions

### Qu’est-ce que la fuite « June 2026 Stealer Logs » ?

C’est un jeu de données ajouté à Have I Been Pwned le 15 juin 2026, regroupant des identifiants collectés par des logiciels infostealers sur des appareils infectés dans le monde entier. Le volume brut atteint 24 milliards de lignes, mais seuls 56,3 millions d’e-mails et 124 millions de mots de passe sont uniques une fois dédupliqués.

### Comment savoir si mon adresse e-mail est concernée ?

Il suffit de saisir son adresse sur haveibeenpwned.com. Le service indique si elle apparaît dans le jeu de données « June 2026 Stealer Logs » ou dans l’une des nombreuses autres fuites déjà répertoriées.

### Est-ce une nouvelle fuite ou d’anciennes données recyclées ?

Les deux à la fois. Le cluster combine des données anciennes, parfois vieilles de plusieurs années et déjà vues ailleurs, avec des journaux plus récents issus d’infections en cours. Les chercheurs estiment qu’une large majorité des identifiants a déjà circulé dans de précédentes compilations.

### Quelle est la différence entre un stealer log et une fuite de données classique ?

Une fuite classique provient généralement du piratage d’un service unique. Un stealer log provient d’un logiciel malveillant installé sur l’ordinateur de la victime, qui aspire les identifiants stockés localement, indépendamment du service concerné.

### Que dois-je faire si mon mot de passe apparaît dans cette fuite de données ?

Changez-le immédiatement sur tous les services où vous l’avez réutilisé, activez l’authentification à deux facteurs partout où c’est possible, et surveillez les connexions inhabituelles sur vos comptes principaux.

### Cette fuite touche-t-elle particulièrement la France ?

Aucune répartition par pays n’a été publiée pour ce jeu de données précis. Mais elle s’ajoute à une série d’incidents ayant déjà touché des utilisateurs français en 2026, ce qui renforce l’exposition cumulée des internautes du pays.

### Les entreprises ont-elles une obligation légale de réagir ?

Le RGPD n’impose pas de notification automatique pour des identifiants volés hors de leur système. Mais si ces identifiants réutilisés permettent une intrusion dans leurs propres systèmes, l’incident qui en résulte peut relever d’une obligation de notification classique à la CNIL.

### Comment éviter d’être victime d’un infostealer à l’avenir ?

Évitez les logiciels piratés et les pièces jointes non sollicitées, maintenez votre système et votre antivirus à jour, et utilisez un gestionnaire de mots de passe plutôt que de stocker vos identifiants dans le navigateur.
