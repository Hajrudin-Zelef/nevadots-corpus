Si vous avez déjà lu quoi que ce soit sur le code, la programmation ou le développement logiciel, vous avez entendu parler de Git.

Pratique (et gratuit), cet outil est le système de contrôle de version le plus utilisé au monde. Il est si répandu que plus de 90 % des développeurs professionnels s'en servent, sans parler des experts d'autres domaines.

À bien des égards, Git est devenu synonyme de contrôle de version. Mais qu'est-ce que le contrôle de version et pourquoi est-ce si important ?

Plongez avec nous dans l'univers Git. Nous allons examiner de près ce qu'est Git, qui l'utilise et son histoire.

## Qu'est-ce que Git ?

Git est un système de contrôle de version distribué (dVCS). Comme son nom l'indique, le contrôle de version consiste à gérer et à suivre différentes versions d'un projet donné.

### Qu'est-ce qu'un système de contrôle de version (VCS) ?

Un VCS suit et enregistre les modifications apportées à n'importe quel fichier (ou groupe de fichiers), ce qui vous permet de retrouver des itérations spécifiques plus tard ou à la demande. Les VCS sont parfois appelés gestion de code source (SCM) ou systèmes de gestion de révisions (RCS).

Le contrôle de version permet à de nombreux membres d'une équipe de travailler ensemble sur un projet, même s'ils ne sont pas dans la même pièce, ni même dans le même pays.

Par exemple, disons que vous êtes auteur-compositeur. Vous travaillez chez vous sur une nouvelle chanson, mais vous n'en êtes pas tout à fait satisfait. Vous décidez donc de collaborer avec deux autres auteurs pour retravailler les passages à améliorer.

Vous et les deux autres auteurs commencez à retoucher les paroles et la partition, chacun travaillant de son côté. Lorsque les autres musiciens vous envoient leurs versions, certaines modifications vous plaisent, d'autres moins.

Imaginez maintenant que vous puissiez voir chaque changement dans chaque version, les tester pour entendre le résultat, puis synchroniser les retouches que vous retenez à travers les versions.

C'est exactement ce que permet Git. Chacun peut travailler localement (sur son propre ordinateur), enregistrer les changements qui fonctionnent, puis synchroniser ces changements vers un dépôt Git afin que les autres puissent voir la nouvelle version.

On pense souvent à Git comme à un outil de développement logiciel — ce qu'il est —, mais il peut être utilisé pour versionner tout type de fichier : lignes de code, maquette de site web ou même une chanson.

### Les avantages du contrôle de version

Au-delà de la collaboration, le contrôle de version présente d'autres atouts :

- **Modifications attribuables** . Chaque changement peut être rattaché à un membre de l'équipe.
- **Traçabilité fine et retour en arrière facilité** . Comme chaque changement est tracé, même les plus petits, il est facile de revenir à une version antérieure si nécessaire. Comme vous pouvez l'imaginer, c'est essentiel en développement logiciel.
- **Meilleure organisation et communication.** Les messages de commit — qui expliquent à l'équipe pourquoi vous avez effectué un changement — favorisent une bonne communication. Ils vous aident aussi à vous y retrouver si vous oubliez ce que vous avez modifié par le passé !
- **Concurrence** . Dans les projets logiciels, les développeurs modifient souvent le code source. En général, plusieurs personnes travaillent sur des sujets différents. L'une améliore la sécurité, l'autre développe une nouvelle fonctionnalité. Git leur permet de travailler en parallèle tout en limitant les conflits entre leurs changements.
- **Branches et fusions.** Les membres de l'équipe peuvent créer des branches séparées pour travailler, puis fusionner leurs modifications dans la branche principale. Les branches sont temporaires et peuvent être supprimées après la fusion.

### Git est-il le seul système de contrôle de version ?

Non, Git n'est pas le seul VCS, mais c'est le plus populaire et il est considéré comme l'outil de référence. Parmi les autres systèmes connus, citons Fossil, Mercurial et Subversion.

Ces systèmes présentent des nuances — par exemple dans la gestion des fonctions clés comme le branchement et la fusion —, mais l'idée générale reste la même. La grande différence tient au fait qu'ils soient centralisés ou distribués.

#### Systèmes de contrôle de version centralisés et distribués

Les systèmes centralisés et distribués, comme Git, remplissent la même fonction.

La différence principale : dans les systèmes centralisés, un serveur central reçoit les dernières versions sur lesquelles les membres de l'équipe poussent leur travail. Vous pouvez l'imaginer comme un projet unique et partagé par tous.

Avec les VCS distribués, chaque membre possède une copie locale (clone) de l'historique complet du projet sur son appareil. Il n'a donc pas besoin d'être en ligne pour modifier ou travailler sur son code. Au lieu d'un serveur central, il récupère ce clone depuis un dépôt en ligne.

Avec Git, le clone de chaque membre est un dépôt qui peut contenir tous les changements depuis le début du projet.

## L'histoire de Git

Git a été développé en 2005 par l'ingénieur logiciel finlandais Linus Torvalds, également à l'origine du noyau du système d'exploitation Linux.

Git a été créé pour répondre à un besoin immédiat. Avant son invention, les développeurs Linux du monde entier utilisaient le logiciel propriétaire BitKeeper, lui-même un dVCS.

Comme ce logiciel appartenait à une entreprise, cela créait des tensions au sein de la communauté Linux, majoritairement attachée à l'open source.

En échange d'une utilisation gratuite, BitMover, l'entreprise derrière BitKeeper, imposait des restrictions à la communauté Linux. Selon le Linux Journal, l'une d'elles interdisait de travailler sur des projets concurrents de contrôle de version.

Comme on pouvait s'y attendre, un développeur Linux a commencé à faire de l'ingénierie inverse de BitKeeper pour créer un produit open source. Fidèle à son avertissement, BitMover a cessé de fournir ses services au noyau Linux, plongeant le système de développement distribué dans l'incertitude.

Pour résoudre l'impasse, Torvalds a mis en pause, pour la première fois depuis 1991, ses travaux sur Linux et a créé Git, dont une version stable est sortie quelques mois seulement après le début du développement.

Fait intéressant : avant même l'adoption de BitKeeper, les développeurs envoyaient à Torvalds leurs « patches » (modifications) individuellement, qu'il intégrait au fil de l'eau. Et en 2016, 11 ans après la sortie de Git, BitKeeper est devenu open source.

### Pourquoi Git s'appelle-t-il Git ?

Dans son tout premier commit de code sur Git en 2005, Linus Torvalds a ajouté un fichier readme qui éclaire l'origine du nom. En voici un extrait :

À moins de préférer la version « Global Information Tracker », le nom Git est un clin d'œil un brin moqueur à ses capacités — ou à leur supposée absence.

### Brève histoire des VCS

Les systèmes de contrôle de version existent bien avant Git ou même BitKeeper. Voici une courte chronologie :

- 1972 - SCCS, premier VCS, est créé par Bell Labs. Il ressemble peu aux systèmes actuels.
- 1982 - Le système de gestion de révisions (RCS) est développé par un informaticien de l'université Purdue.
- 1986 - Le concurrent versions system (CVS) est développé. C'est le premier VCS à proposer un dépôt centralisé accessible à plusieurs utilisateurs.
- 1995 - Développement de Perforce, un VCS encore populaire aujourd'hui.
- 2000 - Apparition de Subversion (ou SVN), plus sophistiqué. Et de BitKeeper, l'un des premiers dVCS, qui a popularisé les systèmes distribués.
- 2005 - Invention de Git, qui devient rapidement l'outil privilégié des développeurs du monde entier.

## Git et GitHub, contrôle de version et dépôts

Git et GitHub sont des technologies complémentaires. Git est un système de contrôle de version, tandis que GitHub est une plateforme cloud qui héberge des dépôts Git et aide les équipes à les gérer.

Conçu en 2008 pour faciliter le travail collaboratif avec Git, ce modèle logiciel en tant que service (SaaS) a excellé dans cet objectif, attirant des millions d'utilisateurs dans le monde.

Au-delà des fonctions standard de Git, GitHub propose des fonctionnalités propres comme le suivi des bugs, des outils de gestion des tâches et l'intégration continue (CI). GitHub fonctionne en freemium : de nombreuses fonctionnalités sont gratuites, mais l'accès complet nécessite un abonnement payant. GitHub appartient à Microsoft depuis 2018.

GitHub n'est pas le seul service d'hébergement de dépôts, mais avec des millions d'utilisateurs et des centaines de millions de projets, c'est de loin le plus populaire. De nombreuses grandes entreprises y sont présentes, dont DataCamp.

Parmi les services concurrents, citons GitLab, entièrement libre et open source, conçu pour Git, et Bitbucket, qui prend en charge Git et Mercurial.

Nous l'avons mentionné : Git et le contrôle de version ne concernent pas que le code ou le développement logiciel. C'est aussi vrai pour GitHub, même si celui-ci n'est pas optimisé pour les projets non liés au code.

## Git, bien plus qu'un outil pour développeurs

Git peut servir à tout projet collaboratif où la gestion des versions compte : la rédaction d'un volumineux guide utilisateur ou même la création de musique liturgique (un projet réel à découvrir sur GitHub).

S'il est surtout associé au cœur du développement logiciel, les professionnels de domaines connexes utilisent Git au quotidien. C'est le cas des data scientists et des analystes : ils ont besoin de gérer le code qui soutient leurs travaux, et Git répond exactement à ce besoin.

Chez DataCamp, nous enseignons les outils et technologies indispensables pour travailler avec les données, Git compris. Retrouvez notre sélection de cours Git immersifs et engageants ici.

## Pourquoi Git est-il si populaire ?

Git est plébiscité pour de nombreuses raisons, à commencer par le fait qu'il est gratuit et open source.

- **Rapidité** . Git est rapide, surtout si l'on considère que les développeurs créent des branches et fusionnent des dépôts entiers. Comme chacun dispose d'une copie locale, pas besoin d'attendre qu'une multitude de petits changements soient poussés vers un serveur.
- **Traçabilité très fine.** Git offre un versioning extrêmement détaillé : même les plus petites modifications sont commit, et les développeurs peuvent laisser un commentaire horodaté expliquant chaque changement.
- **Travail hors ligne.** Avec des copies locales du dépôt complet, inutile d'être en ligne avant d'être prêt à pousser ses changements.
- **Ubiquité** . Aujourd'hui, Git est si courant que sa diffusion alimente encore sa popularité. Plus de 90 % des développeurs utilisent Git, et une entreprise a peu de raisons d'adopter un autre outil si tout le monde maîtrise déjà Git.
- **Collaboration** . Git facilite le travail en équipe : il simplifie la fusion de versions différentes d'un même projet tout en minimisant les conflits potentiels. Avec GitHub, les développeurs disposent d'un écosystème collaboratif agile qui soutient leur travail.

## Comment fonctionne Git ?

Pour comprendre toute la puissance et l'efficacité de Git, il faut se pencher sur quelques aspects techniques. Voici les principes de base :

1. **Dépôt (repo).** Un dépôt Git est un répertoire où sont stockés tous les fichiers d'un projet donné. Il contient toutes les révisions et l'historique du projet. Lorsque vous initialisez Git dans un dossier (`git init` ), celui-ci devient un dépôt.
2. **Commits** . Chaque changement — ou ensemble de changements — que vous validez dans Git s'appelle un commit. Chaque commit possède un identifiant unique (hachage SHA-1) qui permet à Git de suivre les modifications et leur ordre.
3. **Zone de staging.** Avant de valider vos changements avec un commit, vous les « staging ». La zone de staging est un espace de préparation où vous rassemblez vos modifications avant validation. Pour y ajouter des fichiers, utilisez la commande`git add` .
4. **Branches** . Git permet de créer plusieurs lignes de développement grâce aux branches. La branche par défaut s'appelle`master` . Pour développer une fonctionnalité ou corriger un bug, vous pouvez créer une nouvelle branche (`git branch <branch-name>` ) afin d'isoler vos changements sans impacter la ligne principale.
5. **Fusion (merge).** Une fois vos changements terminés sur une branche, vous pouvez les fusionner dans la branche`master` (ou toute autre) avec la commande`git merge` .
6. **Dépôts distants.** Même si vous travaillez en local, Git permet aussi de se connecter à des dépôts distants via`git remote` . C'est particulièrement utile pour collaborer. Comme évoqué, le dépôt distant le plus courant est GitHub.
7. **Push et pull.** Une fois connecté à un dépôt distant, vous pouvez`push` vos changements pour que d'autres puissent les voir et collaborer, et vous pouvez`pull` les mises à jour distantes pour synchroniser votre copie locale.
8. **Fetch** . Semblable à`pull` , la commande`git fetch` récupère les mises à jour d'un dépôt distant sans les fusionner automatiquement dans votre branche courante. Vous pouvez ainsi examiner les changements avant de les intégrer.
9. **Clone** . Pour obtenir une copie d'un dépôt Git existant, utilisez`git clone` . Cela crée sur votre machine un nouveau répertoire avec tous les fichiers et l'historique du dépôt.
10. **Résolution des conflits.** Lorsque plusieurs personnes travaillent sur la même portion de code, des conflits peuvent survenir. Git intègre des mécanismes pour les signaler et permettre une résolution manuelle avant la fusion.
11. **Log** . Pour consulter l'historique des commits, utilisez`git log` . Cette commande affiche la liste des commits, leurs identifiants uniques et les messages associés.

Comprendre ces éléments techniques constitue une base solide pour travailler avec Git. À mesure que vous vous familiarisez avec ces concepts et commandes, vous apprécierez la flexibilité, la puissance et l'efficacité que Git apporte au contrôle de version.

## Envie de vous lancer avec Git ?

Git est le système de contrôle de version distribué le plus utilisé au monde, et il a profondément changé la façon dont les développeurs et les métiers connexes gèrent leurs projets.

Des entreprises comme Google, Netflix et bien d'autres utilisent Git comme un standard de leur stack technologique. Git est si omniprésent que, pour tout projet lié au logiciel ou au code, on peut supposer qu'il fait partie du processus.

C'est aussi une compétence indispensable pour les métiers de la donnée, comme les analystes et les data scientists. Nous avons besoin de versionner le code qui nous permet d'exploiter les données et de créer des outils logiciels pour mener nos analyses.

Git est la norme de facto en matière de VCS. Si vous souhaitez travailler dans l'IT ou un domaine adjacent, c'est une compétence incontournable. Certes, Git n'est pas réputé pour sa simplicité, mais il est assez facile d'en maîtriser les bases et d'approfondir progressivement vos connaissances.

DataCamp peut vous accompagner. Notre cours Introduction to Git est conçu pour vous apprendre l'essentiel de Git de manière ludique et engageante. Une fois à l'aise, vous pouvez envisager de passer une certification GitHub pour valoriser vos compétences.

Pour découvrir pourquoi plus de neuf millions d'apprenants dans le monde adorent DataCamp, inscrivez-vous dès aujourd'hui à votre premier cours Git !

## FAQ

### Quel est l'objectif principal de Git ?

Git est un système de contrôle de version distribué conçu pour suivre les changements du code source pendant le développement logiciel. Il permet à plusieurs développeurs de travailler simultanément sur le même projet, en veillant à ce que leurs modifications n'entrent pas en conflit.

### En quoi Git diffère-t-il des autres systèmes de contrôle de version ?

Git est un système de contrôle de version distribué : chaque développeur possède une copie complète de l'historique du projet sur sa machine. Cela contraste avec les systèmes centralisés, où un dépôt unique et central sert de référence et depuis lequel on extrait le code.

### Git est-il réservé aux développeurs logiciels ?

Bien que Git soit surtout associé au développement logiciel, ses capacités de versioning sont utiles pour de nombreux types de projets : documentation, design, rédaction, etc.

### Quel est le lien entre Git et GitHub ?

Git est un système de contrôle de version, tandis que GitHub est une plateforme cloud qui héberge des dépôts Git. GitHub propose des fonctionnalités supplémentaires comme le suivi des bugs, la gestion des tâches et des outils de collaboration.

### Dois-je être en ligne pour utiliser Git ?

Non. L'un des avantages de Git est de pouvoir travailler hors ligne sur votre dépôt local. Une connexion Internet n'est nécessaire que lorsque vous souhaitez pousser vos changements vers un dépôt distant ou récupérer des mises à jour.

### Git est-il sécurisé ?

Git intègre plusieurs mécanismes pour garantir l'intégrité et l'authenticité du code. Par exemple, la signature des commits s'appuie sur la cryptographie pour vérifier la source et l'intégrité des validations.