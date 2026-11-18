Cours

Docker est devenu un outil incontournable pour les développeurs et les professionnels des données qui ont besoin de créer, de partager et d'exécuter des applications de manière cohérente dans différents environnements. Que vous créiez des conteneurs pour le développement local ou que vous déployiez des microservices en production, il est essentiel de maîtriser les commandes Docker.

Dans ce guide, je vais vous présenter 18 commandes Docker essentielles, couvrant les images, les conteneurs, la mise en réseau, les volumes et Compose, qui amélioreront votre flux de travail et rendront votre travail avec Docker plus fluide et plus efficace.

## Qu'est-ce que Docker ?

Docker est une plateformetform pour le développement, l'expédition et l'exécution d'applications. Il vous permet de séparer votre application de l'infrastructure sous-jacente, ce qui accélère la livraison des logiciels et vous permet de gérer votre configuration de la même manière que vous gérez vos applications.

Docker exécute des applications dans des environnements isolés à l'aide de paquets légers appelés conteneurs, qui comprennent tout ce dont une application a besoin pour fonctionner, comme les dépendances et les installations, ce qui vous aide à économiser les ressources du système. Nous pouvons facilement partager des conteneurs avec des coéquipiers, en exécuter plusieurs simultanément et les gérer tous à l'aide des outils et de la plateforme de Docker.

Nous pouvons utiliser Docker pour beaucoup de choses, y compris :

- Déploiement et mise à l'échelle réactifs.
- Exécution d'un plus grand nombre de charges de travail sur le même matériel.
- Livraison rapide et cohérente des applications.

Lorsque vous utilisez Docker, vous travaillez avec différents objets Docker tels que des images, des conteneurs, des réseaux, des plugins et des volumes. Ceux-ci constituent les éléments de base de l'installation Docker. Sous le capot, Docker utilise les caractéristiques du noyau Linux pour faire fonctionner tout cela. Nous interagissons avec lui en utilisant des commandes simples dans le terminal, et chaque commande Docker commence par `docker`.

**> Si vous débutez, l'introduction à Docker vous aidera à vous familiariser avec Docker.***e cours Introduction à Docker offre une base pratique pour apprendre les bases de la conteneurisation.***rse offre une base pratique pour apprendre les principes de base de la conteneurisation.**

## Commandes de base de Docker

Maintenant que nous avons expliqué ce qu'est Docker et comment il fonctionne, examinons quelques-unes des commandes les plus courantes. Ils vous aideront à construire, exécuter et gérer des conteneurs dans votre travail quotidien.

### docker --version et docker info

Dans les commandes Docker, tout ce qui commence par `--` est appelé un drapeau. 

Par exemple, `--version` est un drapeau qui indique la version du CLI de Docker que vous utilisez. Vous pouvez également utiliser `docker version` (sans le drapeau) pour obtenir des informations détaillées sur la version de tous les composants Docker. 

La sortie est divisée en deux parties :

- Client présente des informations sur le CLI de Docker et les outils associés.
- Serveur affiche des détails sur le moteur Docker et ce sur quoi il s'exécute.

Vous pouvez également formater cette sortie en utilisant l'option `--format` avec un modèle personnalisé.

La commande `docker info` vous donne un aperçu complet de votre configuration Docker. C'est la même chose que d'aller sur le site `docker system info`, mais avec un nom plus court. Vous verrez des détails comme la version de votre noyau, le nombre de conteneurs et d'images, ainsi que d'autres détails concernant le système. 

En fonction de votre pilote de stockage, vous pouvez également voir des informations telles que les noms des pools et des fichiers de données. Comme pour `docker version`, vous pouvez formater la sortie en utilisant `--format` ou `-f`.

### docker pull <image>

La commande pull télécharge une image Docker à partir d'un registre, généralement Docker Hub, une bibliothèque publique d'images préconstruites que vous pouvez utiliser sans rien configurer vous-même. Vous pouvez l'exécuter en tant que `docker pull`  ou `docker pull`, et les deux feront la même chose.

- La syntaxe complète se présente comme suit : `docker image pull [OPTIONS] NAME[:TAG|@DIGEST]`

Si vous ne spécifiez pas de balise, Docker utilisera `:latest` par défaut. Par exemple : `docker image pull debian` tire l'image `debian:latest`.

Vous pouvez également ajouter des options après la commande pour personnaliser la manière dont l'image est extraite, par exemple en limitant la bande passante ou en ignorant la vérification de l'image. L'image ci-dessous présente toutes les options disponibles et leurs fonctions.

Options pour la commande docker pull. Sour*ce : Documentation Docker*

### docker run <image>

La commande `docker run`  crée et démarre un nouveau conteneur à partir d'une image spécifiée, c'est-à-dire qu'elle exécute l'image dans un nouveau conteneur. Il s'agit d'un raccourci pour `docker container run`, et les deux fonctionnent de la même manière.

- Voici la syntaxe de base : `docker container run [OPTIONS] IMAGE [COMMAND] [ARG...]`

Si vous avez déjà lancé un conteneur et que vous souhaitez le relancer avec toutes les modifications précédentes, utilisez : `docker start` .

La commande run propose de nombreuses options permettant de personnaliser l'exécution de votre conteneur. Nous allons passer en revue quelques-unes des plus courantes en les illustrant par des exemples :

| Flag | Exemple de commande | Description | 
| `-- name` | `docker run --name test -d nginx:alpine` | L'identifiant personnalisé spécifié pour le conteneur nommé test utilisant l'image `nginx:alpine` . | 
| `-w` ,`--workdir` | `docker run -w /path/to/dir/ -i -t ubuntu pwd` | Exécute la commande dans le répertoire spécifié, dans cet exemple, `/path/to/dir/` | 
| `--pid` | `docker run --rm -it --pid=host alpine` | Par défaut, l'espace de noms PID est activé dans tous les conteneurs, ce qui permet de séparer les processus. L'exemple utilise un conteneur alpin avec l'option `--pid = host` . | 
| `--cidfile` | `docker run --cidfile /tmp/docker_test.cid ubuntu echo "test"` | Ceci crée un conteneur et imprime un test sur la console L'option `cidfile` permet à Docker de créer un nouveau fichier et d'y inscrire l'identifiant du conteneur. | 

### docker stop <container> et docker start <container> :

La commande `docker start`  démarre un ou plusieurs conteneurs arrêtés. Par exemple, dans `docker start my_container`, `my_container` est le nom du conteneur que nous voulons démarrer. Nous pouvons également utiliser son alias : `docker container start`.

- Sa syntaxe complète est la suivante : `docker container start [OPTIONS] CONTAINER [CONTAINER...]`

De même, `docker stop` arrête un ou plusieurs conteneurs en cours d'exécution. Par exemple, dans `docker stop my_container`, `my_container` est le nom d'un conteneur en cours d'exécution.

Il a également un alias : `docker container stop`. 

- Sa syntaxe complète se présente comme suit : `docker container stop [OPTIONS] CONTAINER [CONTAINER...]`

Comme `start`, la commande `stop` comporte des options permettant de personnaliser la manière dont les conteneurs sont arrêtés. Nous allons maintenant vous en présenter quelques-uns :

Options pour `docker stop`. Source*: Documentation Docker*

## Maîtriser Docker et Kubernetes

## Travailler avec des images Docker

Les images sont la base de tout conteneur. Dans cette section, nous verrons comment créer, gérer et inspecter les images Docker à l'aide de ses commandes courantes.

### construction de docker

La commande `docker build` est l'une des fonctionnalités les plus utilisées de Docker. Bien qu'il fasse partie d'un écosystème plus large qui prend en charge des cas d'utilisation avancés, nous nous concentrerons sur la façon de l'utiliser pour construire une image à partir d'un simple fichier Docker. 

Un `Dockerfile` est un fichier texte brut (sans extension) qui contient des instructions étape par étape que Docker utilise pour construire une image. Voici comment en créer un :

1. Dans le répertoire racine de votre application, créez un fichier nommé `Dockerfile` avec le contenu suivant :

```
# syntax=docker/dockerfile:1
FROM node:lts-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000
```
Ce fichier Docker commence par une image de base légère qui inclut Node.js et Yarn. Il copie le code source de votre application dans l'image, installe les dépendances et définit comment démarrer l'application.

1. Maintenant, construisez l'image à l'aide de la commande suivante :

`docker build -t getting-started.`
Le drapeau `-t` vous permet de marquer l'image. Dans ce cas, nous l'avons nommé `getting-started.` Le `.` à la fin indique à Docker de rechercher le Dockerfile dans le répertoire actuel.

### images docker

La commande `docker images` répertorie toutes vos images de premier niveau, leur référentiel, leurs balises et leur taille. Vous pouvez également utiliser ses alias :

- `docker image list`
- `docker image ls`

Sa syntaxe est la suivante : `docker image ls [OPTIONS] [REPOSITORY[:TAG]]`

Cette commande comporte plusieurs options. Par exemple, pour afficher toutes les images, y compris les images intermédiaires, vous pouvez ajouter l'indicateur `-a` ou `--all` comme suit : `docker images -a`.

### docker rmi <image>

La commande `docker rmi`  supprime une ou plusieurs images de votre système. Si une image comporte plusieurs balises, l'exécution de cette commande avec une balise spécifique ne supprimera que cette balise. Mais si la balise est la seule liée à l'image, la balise et l'image seront toutes deux supprimées.

Vous pouvez également utiliser l'un de ces alias :

- `docker image remove`
- `docker image rm`

Sa syntaxe est la suivante : `docker image rm [OPTIONS] IMAGE [IMAGE...]`

Si vous devez supprimer une image encore utilisée par un conteneur en cours d'exécution, vous devrez la forcer en ajoutant l'option `-f` ou `--force`.

## Gestion des conteneurs Docker

Nous devons souvent gérer des conteneurs - les démarrer, les arrêter, les inspecter ou les supprimer au fur et à mesure de l'évolution de notre application. Je vais donc vous présenter les commandes Docker les plus utiles pour gérer les conteneurs dans votre travail quotidien.

### docker exec <conteneur> <commande>

La commande `docker exec` vous permet d'exécuter une commande à l'intérieur d'un conteneur en cours d'exécution sans le redémarrer. C'est particulièrement utile pour déboguer ou vérifier manuellement quelque chose à l'intérieur du conteneur. Vous pouvez également utiliser son alias : `docker container exec`.

La commande ne fonctionne que si le processus principal du conteneur (PID 1) est en cours d'exécution. Il ne s'exécute pas automatiquement si le conteneur redémarre.

Sa syntaxe est la suivante : `docker exec [OPTIONS] CONTAINER COMMAND [ARG...]`

Vous pouvez utiliser plusieurs drapeaux optionnels. Par exemple, `--privileged` donne à la commande des autorisations étendues à l'intérieur du conteneur. Pour obtenir la liste complète des optionsions, consultez les documents officiels.

Voici un exemple d'exécution d'une commande sur le conteneur :

`docker exec -d mycontainer touch /tmp/execWorks`
La commande `touch` crée un nouveau fichier `/tmp/execWorks` dans le conteneur en cours d'exécution `mycontainer`, en arrière-plan.

### docker logs <container>

La commande `docker logs` vous permet de consulter les journaux d'un conteneur spécifique et d'afficher tout ce qui est imprimé sur la sortie standard et les erreurs au moment de l'exécution. Vous pouvez également utiliser son alias : `docker container logs`

Sa syntaxe est la suivante : `docker container logs [OPTIONS] CONTAINER`

Vous pouvez ajouter un certain nombre d'options utiles. Par exemple :

- `--details` affiche des attributs supplémentaires tels que les variables d'environnement et les étiquettes.
- `--until` vous permet de récupérer les journaux jusqu'à un moment précis.
- `docker logs -f --until=2s test` suit la sortie des journaux du conteneur`test` et s'arrête après avoir affiché les deux dernières secondes de journaux.

Voici toutes les options que nous pouvons utiliser avec `docker logs`:

Options pour `docker log` . Donc*urce : Documentation Docker*

### docker rm <container>

La commande `docker rm`  supprime un ou plusieurs conteneurs de votre système. Vous pouvez également utiliser ses alias :

- `docker container remove`
- `docker container rm`

Sa syntaxe est la suivante : `docker container rm [OPTIONS] CONTAINER [CONTAINER...]`

Cette commande comporte quelques options que vous pouvez utiliser. En voici quelques-unes :

Options pour `docker rm` . Source*e : Documentation Docker*

Par exemple, vous pouvez utiliser `docker rm /redis` pour supprimer le conteneur identifié par le lien `/redis`. Notez toutefois que cette commande ne supprime que les conteneurs en cours d'exécution. 

Si vous souhaitez supprimer les conteneurs arrêtés, vous devez utiliser `docker container prune`. Pour garder votre environnement propre, apprenez à supprimer en toute sécurité les ressources Docker inutilisées grâce à ce tutoriel Docker prune.

### docker restart <container>

La commande `docker restart`  arrête puis redémarre un ou plusieurs conteneurs. Vous pouvez également utiliser son alias : `docker container restart`. 

Sa syntaxe est la suivante : `docker restart [OPTIONS] CONTAINER [CONTAINER...]`

La commande restart dispose de plusieurs options utiles pour personnaliser le redémarrage des conteneurs. Vous trouverez ci-dessous les deux options, accompagnées d'exemples.

| Option | Description | Exemple | 
| -s, --signal | Signal à envoyer au conteneur. | `docker restart -s SIGTERM mycontainer` | 
| -t, --timeout | Nombre de secondes à attendre avant de tuer le conteneur. | `docker restart -t 10 mycontainer` | 

> Pour une pratique concrète, explorez ces idées de projets Docker.*ces idées de projets Docker qui* qui vont du niveau débutant au niveau avancé.

## Réseau Docker

La mise en réseau des conteneurs permet la communication entre les conteneurs et les charges de travail externes. Par défaut, les conteneurs disposent d'un réseau activé et peuvent établir des connexions sortantes, mais ils ne savent pas automatiquement sur quel type de réseau ils se trouvent ni à quelles autres charges de travail ils sont connectés.

À moins que vous n'utilisiez le pilote réseau `none` (qui désactive le réseau), les conteneurs peuvent interagir avec des éléments de réseau tels que les adresses IP, les passerelles et les DNS. 

Explorons quelques commandes courantes de mise en réseau de dockers que vous pouvez utiliser.

### docker network ls

La commande `docker network ls` répertorie tous les réseaux connus du moteur Docker, y compris ceux qui se trouvent sur plusieurs hôtes d'un cluster. Son alias est : `docker network list`.

- Voici sa syntaxe : `docker network ls [OPTIONS]`

Vous pouvez utiliser plusieurs options, comme le montre l'image ci-dessous :

Par défaut, cette commande affiche le nom de chaque réseau :

- ID
- Nom
- Conducteur
- Champ d'application

Vous pouvez utiliser des drapeaux pour personnaliser la sortie. Par exemple, le drapeau `--no-trunc` affiche les identifiants complets du réseau au lieu des identifiants abrégés. Voici comment utiliser cette commande :

`docker network ls --no-trunc`
### docker network create <nom du réseau>

La commande `docker network create` crée un nouveau réseau Docker. Par défaut, il utilise le pilote `bridge`, à moins que vous n'en spécifiiez un autre à l'aide de l'indicateur `--driver` (ou `-d`).

Docker prend en charge les pilotes réseau intégrés tels que :

- `bridge` pour les réseaux à hôte unique.
- `overlay` pour les réseaux multi-hôtes en mode essaim.

Vous pouvez également utiliser des pilotes tiers ou personnalisés si nécessaire.

- Voici la syntaxe de base : `docker network create [OPTIONS] NETWORK`

La commande offre de nombreuses options pour différents objectifs. Consultez la documentation officielle pour voir la liste complète des options.

Voici un exemple de création d'un réseau de passerelles :

`docker network create -d bridge my-bridge-network`
Les réseaux Bridge sont limités à un seul moteur Docker, de sorte qu'ils ne connectent pas les conteneurs sur différents hôtes.

Une fois le mode Swarm activé, vous pouvez créer un réseau qui s'étend sur plusieurs hôtes Docker :

`docker network create --scope=swarm --attachable -d overlay my-multihost-network`
> Vous êtes curieux de savoir comment Docker se compare à Kubernetes ? Cette analyse *de Kubernetes vs Docker*couvre les principales différences et les cas d'utilisation.

## Volumes Docker

Les volumes Docker sont utilisés pour stocker des données qui doivent persister, même lorsqu'un conteneur s'arrête ou est supprimé. Vous pouvez les créer explicitement ou laisser Docker les créer automatiquement lors du démarrage d'un conteneur.

Les volumes sont stockés sur le système hôte mais sont isolés des fichiers principaux de l'hôte. Ils sont montés dans des conteneurs d'une manière similaire aux montages bind, mais avec une meilleure portabilité et une meilleure sécurité.

Explorons quelques commandes de volume docker pertinentes :

### docker volume ls

La commande `docker volume ls` répertorie tous les volumes connus de Docker. Vous pouvez également utiliser son alias : `docker volume list`.

- Sa syntaxe est la suivante : `docker volume ls [OPTIONS]`

Cette commande prend en charge quelques options facultatives pour vous aider à filtrer ou à formater la sortie - voici ce qu'elles sont :

Options pour la commande `docker volume ls`. Sou*rce : Documentation Docker*

### docker volume create <nom du volume>

La commande `docker volume create` crée un nouveau volume pour le stockage des données persistantes. Si vous ne fournissez pas de nom, Docker en génère un pour vous automatiquement.

La création de volumes est une étape courante lorsque vous souhaitez que les données persistent au-delà de la durée de vie d'un seul conteneur.

- Voici la syntaxe : `docker volume create [OPTIONS] [VOLUME_NAME]`

Voyons un exemple de création d'un volume et de configuration d'un conteneur pour l'utiliser :

```
docker volume create hello
docker run -d -v hello:/world busybox ls /world
```
Dans cet exemple, un volume appelé `hello` est créé. Il est ensuite monté dans un conteneur sur le site `/world`. Cela permet au conteneur d'écrire ou de lire des données sur ce volume.

Plusieurs conteneurs peuvent utiliser le même volume, ce qui est utile si un conteneur doit écrire des données pendant qu'un autre les lit.

Note : Les noms de volumes doivent être uniques d'un pilote à l'autre. Vous ne pouvez pas utiliser le même nom de volume dans deux pilotes de stockage différents.

## Commandes Docker Compose

Docker Compose facilite la gestion des applications multi-conteneurs à l'aide d' un simple fichier YAML. Il supporte différents environnements tels que le développement, les tests, la mise en scène, la production et l'analyse critique. En une seule commande, vous pouvez contrôler les services, configurer les réseaux et gérer les volumes, le tout en un seul endroit.

Examinons quelques commandes de composition courantes :

### docker-compose up

La commande `docker compose up` construit, (re)crée, démarre et attache des conteneurs pour un service. Si les conteneurs ne sont pas encore en cours d'exécution, il démarre également tous les services liés automatiquement.

- Voici la syntaxe : `docker compose up [OPTIONS] [SERVICE...]`

Par défaut, cette commande combine les résultats de tous les conteneurs. Si vous souhaitez vous concentrer sur des services spécifiques, vous pouvez le faire :

- Utilisez le drapeau `--attach` pour vous attacher à certains services.
- Utilisez l'option `--no-attach` pour exclure d'autres personnes

Par exemple, `docker compose up --no-attach`  démarre tous les services à l'exception de celui que vous avez exclu des journaux.

Lorsque la commande se termine, les conteneurs s'arrêtent également. Pour qu'ils continuent à fonctionner en arrière-plan, utilisez l'option `--detach`: 

`docker compose up --detach`
Avant d'exécuter cette commande, assurez-vous d'avoir navigué (`cd`) jusqu'au répertoire où se trouve votre fichier `docker-compose.yml`.

### docker-compose down

La commande `docker compose down` arrête les conteneurs et supprime les conteneurs, les images, les réseaux et les volumes créés par `docker compose up`. 

- Sa syntaxe est la suivante : `docker compose down [OPTIONS] [SERVICES]`

Vous pouvez utiliser différentes options, dont les suivantes :

Options pour la commande `docker compose down`. Source : Docker docs.

Par défaut, la commande supprime les éléments suivants :

- Conteneurs pour les services définis dans le fichier Compose.
- Réseaux définis dans la section des réseaux du fichier Compose.
- Le réseau par défaut, s'il y en a un.

Les éléments suivants ne sont pas supprimés par défaut:

- Réseaux et volumes définis comme externes.
- Les volumes anonymes, c'est-à-dire les volumes qui n'ont pas de nom.

Les volumes anonymes ne sont pas montés automatiquement lorsque vous exécutez à nouveau `docker compose up`, car ils n'ont pas de nom. Si vous avez besoin d'un stockage de données persistant, il est préférable d'utiliser des volumes nommés ou des montages bind.

## Meilleures pratiques pour l'utilisation des commandes Docker

Docker est un moyen puissant de créer, d'expédier et d'exécuter des applications dans des conteneurs. Mais pour l'utiliser à bon escient et faire en sorte que votre installation soit efficace et facilement extensible, vous devez suivre certaines bonnes pratiques.

### Utilisez les volumes Docker pour les données persistantes

Par défaut, les fichiers du conteneur sont stockés dans une couche inscriptible qui est perdue lorsque le conteneur est supprimé ! Cette couche est propre à chaque conteneur et n'est pas facilement accessible. Docker utilise différents types de montages pour conserver les données, l'un desch étant les volumes, gérés par le démon Docker et stockés sur l'hôte.

Les volumes nous le permettent :

- Conservez les données même après la suppression d'un conteneur.
- Stockez les données critiques pour les performances avec une vitesse de niveau hôte.
- Gérez facilement le stockage grâce à Docker.

Ils sont idéaux pour les données à long terme ou lorsque plusieurs conteneurs doivent partager l'accès. Gardez à l'esprit que si vous avez besoin d'accéder à des fichiers directement à partir de l'hôte, les montages bind peuvent être mieux adaptés, car les volumes sont entièrement gérés par Docker.

### Automatiser avec Docker Compose

La gestion manuelle de plusieurs conteneurs peut s'avérer fastidieuse. Docker Compose simplifie les choses en vous permettant de tout définir dans un seul fichier YAML afin que vous puissiez vous concentrer sur la construction.

Voici pourquoi il s'agit d'une bonne pratique à suivre :

- Une simple commande: Démarrez, arrêtez, adaptez ou reconstruisez vos services en une seule fois.
- La cohérence pour tous: Que vous travailliez dans le domaine du développement, des tests ou de la production, Compose permet d'aligner les environnements afin que vous ne soyez pas confronté à des problèmes du type "ça marche sur ma machine".
- Mise en réseau intégrée: Compose crée un réseau partagé afin que vos services puissent communiquer facilement en utilisant des noms de service au lieu d'adresses IP.
- Mise à l'échelle facile: Vous pouvez rapidement augmenter ou diminuer la taille des services avec l'option `--scale` , ce qui est idéal pour tester la façon dont votre application gère différentes charges.
- Une configuration claire et collaborative: L'ensemble de votre configuration, y compris les conteneurs, les réseaux et les volumes, est contrôlé par version et lisible.

## Conclusion

Docker peut sembler beaucoup au début, mais une fois que vous maîtrisez les commandes de base, il offre de nombreuses possibilités. De l'exécution de votre premier conteneur à la gestion des réseaux, des volumes et des services, vous êtes désormais mieux équipé pour créer et exécuter en toute confiance des applications conteneurisées.

Et si vous souhaitez continuer à apprendre, voici quelques ressources intéressantes à explorer :

- Concepts de conteneurisation et de virtualisation - un cours parfait pour construire votre base conceptuelle.
- Introduction à Docker - un cours adapté aux débutants pour vous aider à démarrer.
- Intermediate Docker - cours pour ceux qui veulent aller plus loin.
- Conteneurisation et virtualisation avec Docker et Kubernetes - piste de compétences à développer sur Kubernetes et l'orchestration du monde réel.

## Maîtriser Docker et Kubernetes

## FAQ

### Quelles sont les commandes Docker les plus utilisées ?

**Parmi les commandes Docker les plus courantes, citons `docker run`, `docker ps`, `docker build`, `docker pull` et `docker-compose up`.**

### En quoi les volumes Docker diffèrent-ils des montages bind ?

**Les volumes Docker sont gérés par Docker et sont idéaux pour la portabilité et la persistance des données, tandis que les montages bind sont liés à un chemin de fichier spécifique sur le système hôte.**

### Puis-je lancer plusieurs conteneurs à l'aide d'une seule commande ?

**Oui, l'utilisation de `docker-compose up` avec un fichier `docker-compose.yml` vous permet d'exécuter plusieurs services simultanément.**

### Quelle est la différence entre docker start et docker run ?

`docker run` crée et démarre un nouveau conteneur, tandis que `docker start` redémarre un conteneur existant arrêté.

### Comment puis-je dresser la liste de tous les conteneurs arrêtés ?

**Utilisez `docker ps -a` pour voir tous les conteneurs, y compris ceux qui sont arrêtés.**

### Quel est l'objectif de la commande docker exec ?

**Il vous permet d'exécuter des commandes à l'intérieur d'un conteneur en cours d'exécution, souvent utilisé pour le débogage ou les vérifications manuelles.**

### Docker est-il réservé aux systèmes basés sur Linux ?

**Non, Docker prend également en charge macOS et Windows, en utilisant des machines virtuelles légères pour permettre la conteneurisation.**

### Que fait la commande docker-compose down ?

**Il arrête et supprime les conteneurs, les réseaux et les volumes créés par `docker-compose up`.**

### Comment supprimer les images Docker qui pendent ?

**Lancez `docker image prune` pour nettoyer les images inutilisées et libérer de l'espace disque.**

### Docker peut-il être utilisé dans des environnements de production ?

**Absolument. Docker est largement utilisé en production pour déployer des applications évolutives et reproductibles à travers des configurations cloud et on-prem.**

Je suis un stratège du contenu qui aime simplifier les sujets complexes. J'ai aidé des entreprises comme Splunk, Hackernoon et Tiiny Host à créer un contenu attrayant et informatif pour leur public.