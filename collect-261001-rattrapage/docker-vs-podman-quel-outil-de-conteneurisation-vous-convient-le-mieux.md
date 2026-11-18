Cours

Les conteneurs exécutent les applications et les charges de travail de données du monde entier. Conçus pour la première fois dans les années 1970, les conteneurs permettent de regrouper en un seul objet tout ce qui est nécessaire à l'exécution d'une application ou d'une charge de travail. Les conteneurs permettent de résoudre le problème du "ça tourne sur ma machine" en offrant une solution isolée et portable pour développer, tester et expédier du code. Des outils comme Kubernetes s'appuient fortement sur les conteneurs en tant qu'élément central de leur architecture. Pour l'instant, les conteneurs ne vont nulle part.

Pour faire fonctionner ces conteneurs, vous aurez besoin d'une solution de gestion de conteneurs. Entrez dans Docker et Podman.

Docker et Podman sont utilisés pour construire, gérer et déployer des conteneurs. Ensemble, nous allons analyser les similitudes et les différences entre Docker et Podman, ainsi que les fonctionnalités uniques de chacun d'entre eux. Nous explorerons des sujets tels que l'architecture avec ou sans démon, la gestion multi-conteneurs et l'intégration multiplateforme. À la fin, vous serez armé des informations dont vous avez besoin pour choisir la solution de conteneur parfaite pour vos besoins.

**Si vous ne connaissez pas encore ces outils, vous pouvez également consulter notre Introduction à Docker et notre tutoriel Introduction à Podman pour l'apprentissage automatique.** 

## Devenez ingénieur en données

## Que sont Podman et Docker ?

Commençons par une vue d'ensemble de ces outils afin d'entamer notre comparaison :

### Aperçu de Docker

Docker est la norme de facto pour la construction, l'exécution et l'expédition de conteneurs. Les conteneurs sont des objets qui combinent des dépendances au niveau du système d'exploitation et une sorte de code d'application pour emballer et exécuter des éléments tels que des applications complètes ou des pipelines ETL dans leur propre environnement isolé. Les conteneurs sont comme de petits ordinateurs qui ne disposent que de l'essentiel pour exécuter une sorte de code.

Docker est assez jeune et a été publié pour la première fois en tant que projet open-source en 2013. Depuis, le projet a explosé.

Lorsqu'il s'agit d'exécuter des conteneurs dans une entreprise, presque toutes les équipes chargées des logiciels et des données utilisent Docker.

Les développeurs peuvent utiliser Docker sur les trois principaux systèmes d'exploitation, et il s'intègre parfaitement à presque toutes les technologies modernes. Cela signifie qu'un ingénieur de données peut écrire et empaqueter un pipeline de données en utilisant un conteneur Docker sur son Mac local, et expédier ce conteneur pour qu'il s'exécute sur AWS ECS.

Des outils tels que Docker CLI, Docker Desktop et Docker Hub permettent aux développeurs de tous niveaux de démarrer facilement.

Si vous cherchez un moyen plus pratique d'apprendre Docker, nous avons plusieurs projets Docker et des informations sur les certifications Docker pour vous aider à améliorer vos compétences en matière de Docker !

### Aperçu de Podman

Comme Docker, Podman est un outil open-source pour le développement et la gestion de conteneurs. Podman a été développé à l'origine par Red Hat en tant qu'alternative Linux-native à Docker et a été publié en 2019.

Plus particulièrement, l'architecture sous-jacente des deux systèmes d'exécution de conteneurs diffère ; alors que Docker utilise des démons, Podman fonctionne sans démon (plus d'informations à ce sujet plus loin).

Contrairement à Docker, Podman ne nécessite pas d'accès root à la machine sur laquelle s'exécutent les pods qu'il gère, ce qui en fait une option plus respectueuse de la sécurité pour les équipes qui utilisent des conteneurs pour exécuter leurs applications et leurs charges de travail.

Les utilisateurs de Podman bénéficient d'une expérience utilisateur similaire à celle de Docker ; les développeurs peuvent utiliser un CLI ou une interface graphique (Podman Desktop) pour interagir avec Podman dans leur environnement local.

Les utilisateurs de Linux, Mac et Windows peuvent utiliser Podman pour construire et tester leurs conteneurs localement avant de les déployer dans une sorte d'environnement distant, comme Kubernetes.

## Principales différences entre Podman et Docker

### Architecture avec ou sans démon

La plus grande différence entre Docker et Podman est l'architecture sous-jacente sur laquelle ils sont construits. Docker s'appuie fortement sur un démon, tandis que Podman est sans démon.

Vous pouvez considérer un démon comme un processus qui s'exécute en arrière-plan sur le système d'exploitation hôte. Dans le cas de Docker, son démon est responsable de la gestion des objets Docker (images et conteneurs) et de la communication avec d'autres systèmes. Pour exécuter son démon, Docker utilise un paquetage appelé dockerd.

Pourquoi est-ce important ? Tout d'abord, les démons nécessitent généralement un accès au niveau racine de la machine sur laquelle ils s'exécutent. Cette situation est propice aux failles de sécurité : si un acteur mal intentionné parvient à accéder à un démon, il a désormais accès à l'ensemble de la machine.

L'architecture sans démon de Podman présente quelques avantages. Étant donné que l'exécution des démons nécessite presque toujours les privilèges de l'administrateur, une architecture sans démon peut être considérée comme "sans racine". Cela signifie que les utilisateurs qui n'ont pas d'accès au niveau du système à la machine sur laquelle leurs conteneurs s'exécutent peuvent toujours utiliser Podman, ce qui n'est pas toujours le cas avec Docker.

Au lieu d'un démon, Podman utilise un paquetage Linux connu sous le nom de systemd. Étant donné que systemd fait partie intégrante du système d'exploitation Linux, Podman est souvent considéré comme plus "léger" que Docker ; les utilisateurs de Podman constatent généralement des temps de démarrage des conteneurs plus rapides que lorsqu'ils utilisent Docker.

### Construire des images et des conteneurs

Malgré leurs architectures fondamentalement différentes, Docker et Podman partagent le même objectif principal : créer et exécuter des images et des conteneurs. Cependant, leurs approches de ce processus diffèrent légèrement.

Avec Docker, une image est construite en ajoutant d'abord des commandes à un Dockerfile. Ensuite, une commande telle que docker build est exécutée. Cette opération appelle chacune des instructions du fichier Docker , ce qui permet de créer une image. Une image peut ensuite être "exécutée" en tant que conteneur. Comme vous l'avez peut-être deviné, cela se fait en utilisant la commande docker run, et en spécifiant un ID d'image ou un tag. Pour construire et exécuter plusieurs conteneurs, nous utiliserons un outil spécial appelé docker-compose, que nous explorerons un peu plus loin.

Le processus de construction des images et de leur exécution en tant que conteneurs est presque identique dans Podman. Plutôt qu'un fichier Docker (bien que ce nom de fichier fonctionne toujours), les utilisateurs de Podman créeront un fichier Container. La syntaxe de composition de l'image est la même. Une fois les commandes appropriées ajoutées aufichier de conteneurs , l'image peut être construite et exécutée à l'aide de l'interface de programmation Podman.

Pour l'essentiel, Podman est compatible avec la plupart des éléments de Docker. Vous trouverez des différences ici et là, mais pour l'essentiel, l'interface de programmation de Docker peut être remplacée par l'interface de programmation de Podman sans problème.

## Podman vs Docker Desktop

### Docker Desktop pour un accès multiplateforme simplifié

Il existe plusieurs façons de travailler avec Docker. Les praticiens expérimentés des logiciels et des données s'appuient généralement sur le CLI Docker (AKA le client Docker) pour interagir avec leurs images et leurs conteneurs Docker.

Cependant, il existe un moyen encore plus simple de commencer, et c'est Docker Desktop.

Docker Desktop est un outil gratuit basé sur une interface graphique qui permet aux utilisateurs de créer et de gérer les images et les conteneurs qui exécutent leurs applications ou leurs charges de travail. Un ingénieur des données peut utiliser Docker Desktop pour afficher les images disponibles sur sa machine et transformer cette image en conteneur. De même, un développeur de logiciels peut télécharger une image à partir de Docker Hub pour l'utiliser dans le cadre de son prochain projet.

L'interface utilisateur est simple et intuitive, tout en conservant une visibilité et un contrôle complets sur votre environnement Docker.

Cependant, Docker Desktop ne se limite pas à l'affichage et à la gestion des objets Docker.

Les utilisateurs peuvent notamment gérer (à l'octet près) les ressources disponibles pour leurs objets Docker, s'attacher à un conteneur en cours d'exécution ou lancer un cluster Kubernetes sur leur machine locale. Les utilisateurs de Docker Desktop peuvent choisir parmi des centaines d'extensions, ou commencer à utiliser Docker grâce à des tutoriels utiles et des exemples d'environnements. Heureusement pour vous, Docker Desktop est largement accessible et fonctionne sur Mac, Windows ou Linux.

### Caractéristiques et limitations de Podman Desktop

Podman Desktop ressemble beaucoup à son homologue Docker. À partir de l'interface utilisateur de Podman Desktop, les utilisateurs peuvent afficher et gérer des conteneurs, des images, des pods et des volumes. Comme pour Docker, Podman prend en charge des plugins et des intégrations permettant de faire fonctionner localement un cluster Red Hat OpenShift ou de travailler avec des LLM en utilisant le Podman AI Lab.

Si vous souhaitez utiliser un plugin personnalisé, vous pouvez l'installer à partir de Podman Desktop.

Si vous êtes à la fois un utilisateur de Docker et de Podman, vous serez peut-être surpris de voir des objets Docker et Podman dans l'interface utilisateur de Podman Desktop. Ce n'est pas une coïncidence ! Nous examinerons plus en détail ce que cela implique. En attendant, cela signifie que les utilisateurs peuvent interagir à la fois avec leurs objets Podman ET Docker, le tout via une seule vitre.

Le cas d'utilisation le plus courant des conteneurs consiste à les faire fonctionner via Kubernetes. Malgré le titre de norme industrielle de Docker pour la conteneurisation, Podman offre une expérience Kubernetes plus robuste sur Podman Desktop.

La possibilité d'afficher et de gérer les ressources Kubernetes comme les nœuds, les pods, les déploiements (et bien plus encore) fait de l'administration et du développement de Kubernetes un citoyen de première classe sur Podman Desktop. Ces outils, ainsi que des plugins comme l'intégration Red Hat OpenShift mentionnée plus haut, différencient Podman en tant qu'outil orienté vers les ateliers Kubernetes.

## Podman Compose vs Docker Compose

### Définir et gérer des applications multi-conteneurs avec Docker

Certaines applications et charges de travail peuvent être regroupées dans un seul conteneur. Certains ne le peuvent pas. Pour faciliter la gestion de plusieurs conteneurs, Docker propose un outil appelé Docker Compose. Docker Compose utilise un seul fichier YAML pour définir les composants de votre application.

Ensuite, à l'aide de la CLIdocker-compose, ces conteneurs et services peuvent être démarrés, arrêtés ou reconstruits. Un fichier YAML de Docker Compose peut ressembler à ceci :

```
yaml
version: '3'
services:
	app:
		image: python:3.10
		container_name: app
		command: run app --host=0.0.0.0
	database:
		image: postgres:13
		container_name: database
		ports: 5432
		volumes:
			- postgres_data:/var/lib/postgresql/data
volumes:
	postgres_data
```
Il se passe beaucoup de choses, mais ce que Docker Compose nous permet de faire, c'est de définir un fichier YAML avec deux services et un volume. Ensuite, la commande docker-compose up lancera ces conteneurs et nous aurons une application en cours d'exécution .

Pour les équipes chargées des logiciels et des données qui gèrent des applications et des charges de travail volumineuses, Docker Compose facilite le développement local, ainsi que l'expédition et l'exécution du code dans un environnement de production.

### L'approche de Podman pour les applications multi-conteneurs

L'exécution d'applications multi-conteneurs avec Podman ressemble à s'y méprendre à celle de Docker. Podman le fait en utilisant Podman Compose. Comme pour Docker Compose, Podman Compose utilise des fichiers YAML pour définir les composants d'une application de manière déclarative.

Le podman-compose peut alors être utilisé pour démarrer, arrêter ou redémarrer les services définis dans le fichier YAML.

Dans la plupart des cas, podman-compose peut être utilisé à la place de docker-compose (il y a quelques incompatibilités ici et là). Comme avec Docker, l'utilisation de Podman Compose permet de gérer des applications multi-conteneurs de manière indépendante et flexible.

Vous trouverez ci-dessous un tableau comparant Docker et Podman.

| **Caractéristique/aspect** | **Docker** | **Podman** | 
| **Architecture** | Docker s'appuie sur un démon en tant que composant architectural de base. | Architecture sans démon. | 
| **Sécurité** | Nécessite les privilèges de root pour construire, exécuter et gérer les conteneurs. | La nature sans démon de l'architecture de Podman en fait un outil de gestion de conteneurs plus respectueux de la sécurité. | 
| **Outil de l'utilisateur** | Docker Desktop, docker CLI | Podman Desktop, podman CLI | 
| **Compatibilité** | Windows, Mac, Linux | Native de Linux, disponible pour Windows et Mac. | 
| **Adoption** | Standard industriel pour l'orchestration de conteneurs avec une communauté massive et une compatibilité quasi universelle. | Alternative à Docker avec une communauté plus petite, mais en pleine croissance. | 

## Cas d'utilisation et meilleurs scénarios pour Podman vs Docker

Examinons maintenant la question clé que vous vous posez peut-être : quand utiliser Docker et quand utiliser Podman ? Regardons de plus près.

### Quand utiliser Docker ?

Docker est la norme de facto pour la construction, l'exécution et l'expédition de conteneurs. Si vous débutez dans la conteneurisation (surtout sur votre machine personnelle), essayez d'utiliser Docker.

Il est facile de mettre en place votre premier (ou cinquantième) conteneur et de le faire fonctionner à l'aide d'outils tels que Docker Desktop ou Docker CLI. Docker dispose d'une communauté massive, et il y a de fortes chances que ce que vous essayez de faire ait déjà été fait. Cela permet de faciliter les opérations de dépannage.

Docker offre une plus grande cohérence multiplateforme que Podman. Plus important encore, Docker s'intègre à presque tous les services basés sur des conteneurs, notamment AWS ECS, Azure AKS et Google Cloud Run.

Cela signifie que lorsque le moment est venu d'exécuter vos conteneurs en production, vous êtes en mesure d'intégrer facilement le service de votre choix. La possibilité de passer du développement local à la production est l'un des aspects les plus puissants de la conteneurisation de votre code avec Docker.

Les équipes d'ingénierie logicielle et de données ne sont pas les seules à utiliser Docker. Ingénieurs en IA et ML, scientifiques des donnéeset même les analystes de données utilisent Docker pour améliorer leur travail !

### Quand utiliser Podman

Pour les développeurs travaillant dans un environnement sensible à la sécurité ou fortement réglementé, Podman peut être votre gestionnaire de conteneurs du jour. Rappelez-vous que Podman est sans racine, ce qui signifie qu'un utilisateur exécutant Podman localement n'a pas besoin d'un accès root à sa machine pour construire et gérer des conteneurs localement. Vous trouverez ci-dessous quelques autres raisons pour lesquelles il peut être judicieux d'opter pour Podman plutôt que pour Docker.

- Vous développez localement sur une machine Linux.
- L'utilisation des ressources sous-jacentes et le temps de démarrage des conteneurs sont importants pour vous.
- Vous prévoyez d'expédier vos conteneurs vers un cluster Kubernetes ou vous souhaitez imiter un environnement Kubernetes sur votre machine locale.

Voici l'autre chose à garder à l'esprit : pour l'essentiel, Podman et Docker sont interchangeables. Cela signifie que si vous commencez à utiliser Docker et que vous vous rendez compte que Podman est l'outil qu'il vous faut, il est facile de passer d'une offre à l'autre.

## Conclusion

L'exploitation de conteneurs nécessite un outil pour gérer ces objets. Ensemble, nous avons exploré deux des outils de conteneurisation les plus populaires : Docker et Podman.

Standard de l'industrie pour la conteneurisation, Docker est utilisé par des millions de personnes pour exécuter les applications et les charges de travail de données du monde entier. L'architecture de Docker repose sur le démon Docker, qui nécessite un accès root au système sur lequel le conteneur s'exécute.

Pour interagir avec Docker, les développeurs peuvent utiliser le CLI Docker ou Docker Desktop, qui permettent tous deux de gérer des éléments tels que les images, les conteneurs et les volumes. L'adoption généralisée de Docker se traduit par une communauté nombreuse et dynamique, ainsi que par une prise en charge des trois principaux systèmes d'exploitation et de la quasi-totalité des services basés sur des conteneurs.

Podman offre une solution alternative de gestion des conteneurs. Podman est sans démon et sans racine, ce qui signifie qu'un utilisateur n'a pas besoin d'un accès racine à la machine qu'il utilise pour exécuter Podman. Ceci est intéressant pour les équipes qui demandent un outil de gestion des conteneurs plus soucieux de la sécurité. Comme Docker, Podman offre à la fois un CLI et une interface utilisateur pour construire et gérer des conteneurs. Bien que natif de Linux, Podman peut être exécuté sur Windows et Mac et s'intègre assez bien avec des outils comme AWS ECS et Azure AKS.

Quel que soit l'outil que vous choisissez, apprendre à "conteneuriser" le code que vous écrivez est l'un des moyens les plus rapides de développer vos compétences en matière de développement. Si vous souhaitez en savoir plus sur Docker et Podman, n'hésitez pas à mettre la main à la pâte avec des cours tels que Introduction à Docker ou Concepts de conteneurisation et de virtualisation. Bonne chance et bon codage !

## FAQ Podman vs Docker

### Qu'est-ce qu'un conteneur ?

**Un conteneur est un objet qui contient tout ce qui est nécessaire à l'exécution d'une application ou d'une charge de travail. Vous pouvez considérer les conteneurs comme de petits ordinateurs qui ne disposent que de l'essentiel pour exécuter une sorte de code. Heureusement, nous pouvons exécuter ces conteneurs sur nos machines locales ainsi que sur des serveurs qui rendent une solution accessible au monde entier.**

### Pourquoi utiliser un conteneur dans mon projet ?

**L'utilisation d'un conteneur dans vos projets vous permet de regrouper votre code en un seul objet. Pourquoi est-ce important ? Cela signifie que vous pouvez facilement partager votre code avec d'autres développeurs ou même l'envoyer en production sans avoir à recréer l'ensemble de votre environnement local.**

### Pourquoi dois-je utiliser un gestionnaire de conteneurs comme Docker ou Podman ?

**Pour construire, exécuter et gérer un conteneur, vous devez utiliser un gestionnaire de conteneurs. Docker et Podman fournissent des outils pour créer et tester votre conteneur avant de déployer votre solution dans le monde. Sans un outil comme Docker ou Podman, ces tâches seraient assez délicates.**

### Que signifie "sans démon" ?

**Un démon est un processus qui s'exécute toujours en arrière-plan. Le terme "sans démon" signifie que l'outil existe sans qu'un processus soit toujours en cours d'exécution en arrière-plan.**

### Existe-t-il d'autres gestionnaires de conteneurs que Docker ou Podman ?

**Containerd et LXC sont deux systèmes de gestion de conteneurs très répandus qui permettent de créer, d'exécuter et de gérer des conteneurs à grande échelle.** 

Jake est un ingénieur de données spécialisé dans la construction d'infrastructures de données résilientes et évolutives utilisant Airflow, Databricks et AWS. Jake est également l'instructeur des cours Introduction aux pipelines de données et Introduction à NoSQL de DataCamp.