Cursus

La conteneurisation a transformé la façon dont les équipes d’ingénierie gèrent et font évoluer les applications, en particulier pour la gestion des données, l’analytique et l’apprentissage automatique. En emballant les applications dans des environnements isolés et légers, les conteneurs garantissent des performances cohérentes du développement à la production.

Parmi les différentes plateformes disponibles, Docker s’impose comme la solution la plus populaire. Sa flexibilité et sa simplicité permettent aux professionnels des données de créer des pipelines reproductibles, évolutifs et efficaces, tout en favorisant la collaboration.

Dans cet article, nous vous proposons un plan d’apprentissage concret de Docker, avec des étapes pour déployer votre première application simple. Allons-y !

## Qu’est-ce que Docker et pourquoi l’apprendre ?

Docker est une plateforme open-source qui simplifie le déploiement, la mise à l’échelle et la gestion des applications via la conteneurisation.

Les conteneurs sont des environnements portables et légers qui regroupent tout le nécessaire pour exécuter une application — code, runtime, bibliothèques et réglages — afin d’assurer des performances constantes sur différents systèmes. Dans les projets data, Docker sert à construire et gérer ces conteneurs, permettant aux applications de tourner de manière fiable sur toute infrastructure.

À la différence des machines virtuelles (VM), qui nécessitent leur propre système d’exploitation et un hyperviseur, Docker ne virtualise que la couche applicative. Résultat : des conteneurs plus rapides à démarrer, moins gourmands en ressources et plus simples à configurer.

*Applications conteneurisées versus machines virtuelles. Source : Docker*

Pour les professionnels des données, Docker permet de créer des environnements reproductibles afin d’exécuter les pipelines avec la même fiabilité du développement à la production. Il réduit les problèmes de dépendances, fluidifie les workflows et favorise la collaboration grâce à des environnements standardisés et partageables.

De plus, Docker s’intègre avec des outils data populaires comme Jupyter, TensorFlow, et Apache Hadoop.

Maîtriser Docker peut accélérer votre productivité, optimiser vos workflows et rendre vos projets facilement déployables et scalables !

## Apprendre Docker depuis zéro : votre premier déploiement

La meilleure façon d’apprendre Docker, c’est de pratiquer. Laissez-nous vous guider dans un premier déploiement simple. Ensuite, nous verrons des plans d’apprentissage pour approfondir vos connaissances.

### Étape 1 : comprendre les concepts clés

Avant de passer à la pratique, il est important d’assimiler quelques fondamentaux. Voici les principaux concepts Docker :

- Conteneurs : unités légères et isolées qui embarquent une application avec toutes ses dépendances, garantissant un fonctionnement identique dans différents environnements.
- Images : modèle en lecture seule utilisé pour créer des conteneurs. Il inclut tout le nécessaire (code, bibliothèques, outils système). Les images sont généralement construites à partir de Dockerfiles.
- Dockerfile : fichier texte contenant les instructions de construction d’une image Docker (installation de logiciels, copie de fichiers, configuration de l’environnement, etc.).
- Docker Hub : Docker Hub est un registre public pour stocker, partager et télécharger des images Docker. Il facilite la distribution et la réutilisation d’environnements préconfigurés.
- Volumes : mécanisme pour persister les données générées et utilisées par les conteneurs Docker, en les stockant hors du cycle de vie des conteneurs afin d’éviter toute perte.
- Réseaux : les réseaux Docker facilitent la communication entre conteneurs. Chaque conteneur peut être connecté à un ou plusieurs réseaux pour échanger des données en toute sécurité.

Aperçu de l’architecture Docker. Source : *Docker*

Comprendre ces notions de base est indispensable avant de déployer des applications avec Docker. Les maîtriser vous donnera des fondations solides et rendra la pratique beaucoup plus efficace.

Le cours Introduction to Docker peut vous aider de manière significative à consolider vos acquis.

### Étape 2 : installer Docker

Pour commencer à utiliser Docker, vous devez l’installer sur votre système. Voici les instructions selon votre plateforme. Pour plus de détails, reportez-vous à la documentation officielle Docker via les liens fournis.

#### 1. Installer Docker sur Windows

Prérequis :

- Windows 10 64 bits : Pro, Enterprise ou Education (Build 19041 ou supérieur)
- Windows 11 64 bits : Home, Pro, Enterprise ou Education
- Backend WSL 2

Étapes :

1. Activer WSL 2 (Windows Subsystem for Linux) :

- Ouvrez PowerShell en tant qu’administrateur.
- Exécutez les commandes suivantes :

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart 
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart wsl --set-default-version 2
```
2. Installer Docker Desktop pour Windows :

- Téléchargez Docker Desktop depuis le site officiel de Docker.
- Lancez l’installeur et suivez les instructions.
- Choisissez l’option WSL 2 comme backend par défaut pendant l’installation.

3. Démarrer Docker Desktop :

- Lancez Docker Desktop depuis le menu Démarrer.
- Docker devrait démarrer automatiquement ; sinon, lancez-le manuellement.

4. Vérifier l’installation :

- Ouvrez PowerShell ou l’invite de commandes.
- Exécutez la commande suivante pour vérifier que Docker est bien installé :

`sudo docker --version`
Documentation officielle : Docker Desktop for Windows

#### 2. Installer Docker sur macOS

Prérequis :

- macOS 10.15 ou version ultérieure

Étapes :

1. Télécharger Docker Desktop pour macOS :

- Rendez-vous sur le site officiel de Docker et téléchargez l’installateur Docker Desktop.

2. Installer Docker Desktop :

- Ouvrez le fichier `.dmg` téléchargé.
- Glissez l’icône Docker dans le dossier Applications.

3. Démarrer Docker Desktop :

- Ouvrez Docker depuis le dossier Applications.
- Suivez l’assistant pour finaliser l’installation.

4. Vérifier l’installation :

- Ouvrez un terminal.
- Exécutez la commande suivante pour vérifier l’installation de Docker :

`docker --version`
Documentation officielle : Docker Desktop for Mac

#### 3. Installer Docker sur Linux

Distributions prises en charge :

- Ubuntu
- Debian
- Fedora
- CentOS
- RHEL

Étapes pour Ubuntu/Debian :

1. Désinstaller les anciennes versions :

- Avant d’installer Docker Engine, supprimez les paquets en conflit avec cette commande :

`for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done`
2. Configurer le dépôt apt de Docker :

- Exécutez les commandes suivantes :

```
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo \"$VERSION_CODENAME\") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
3. Installer les paquets Docker :

- Installez Docker Engine et les composants nécessaires :

`sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`
4. Vérifier l’installation :

- Exécutez la commande suivante pour vérifier que Docker est installé :

`sudo docker --version`
Documentation officielle : Docker Engine on Debian

## Devenez ingénieur en données

### Étape 3 : exécuter votre premier conteneur

Maintenant que Docker est installé, il est temps d’exécuter votre premier conteneur. Nous allons démarrer avec la simple image hello-world, parfaite pour débuter.

1. Ouvrez votre terminal (ou l’invite de commandes sous Windows) et exécutez :

`sudo docker run hello-world`
Cette commande indique à Docker de chercher l’image `hello-world` en local. Si elle est introuvable, Docker la téléchargera depuis Docker Hub puis l’exécutera.

Si Docker est correctement installé, vous verrez le message « Hello from Docker! » accompagné d’explications sur le fonctionnement du processus. Cette sortie confirme que Docker a bien récupéré l’image, créé un nouveau conteneur et exécuté le code à l’intérieur.

### Étape 4 : construire votre première image Docker

Dans cette étape, vous allez créer une image Docker personnalisée pour un mini projet de data science. Elle inclura Python et des bibliothèques courantes comme pandas, NumPy et scikit-learn.

1. Créer un nouveau répertoire :

- 
  - Commencez par créer un répertoire pour votre projet et placez-vous dedans :

```
mkdir my-data-science-app
cd my-data-science-app
```
2. Créer un Dockerfile :

Dans ce répertoire, créez un fichier nommé `Dockerfile` (sans extension) :

```
# Use an official Python runtime as a parent image
FROM python:3.8-slim
 
# Set the working directory in the container
WORKDIR /app
 
# Copy the current directory contents into the container at /app
COPY . /app
 
# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt
 
# Make port 8888 available to the world outside this container
EXPOSE 8888
 
# Run a Jupyter notebook server when the container launches
CMD ["jupyter", "notebook", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]
```
Ce Dockerfile effectue les actions suivantes :

- `FROM python:3.8-slim` : définit l’image de base sur Python 3.8, variante « slim », idéale pour un environnement data léger.
- `WORKDIR /app` : fixe le répertoire de travail du conteneur à /app. Les commandes suivantes s’y exécuteront.
- `COPY . /app` : copie tous les fichiers du répertoire courant de l’hôte vers`/app` dans le conteneur (scripts, fichiers de configuration, etc.).
- `RUN pip install --no-cache-dir -r requirements.txt` : installe les bibliothèques Python listées dans`requirements.txt` (pandas, NumPy, scikit-learn, ...).
- `EXPOSE 8888` : expose le port 8888 à l’hôte, port par défaut des notebooks Jupyter.
- `CMD ["jupyter", "notebook", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]` : lance un serveur Jupyter Notebook au démarrage du conteneur, accessible depuis votre navigateur.

3. Créer le fichier `requirements.txt` :

Dans le même répertoire, créez un fichier `requirements.txt` listant les paquets Python requis :

```
pandas
numpy
scikit-learn
matplotlib
```
Ce fichier énumère les bibliothèques Python qui seront installées dans votre conteneur Docker.

4. Construire l’image Docker :

- Avec le Dockerfile et `requirements.txt` prêts, construisez l’image avec :

`sudo docker build -t my-data-science-app .`
Cette commande fait :

- `docker build` : lance le processus de construction d’une image.
- `-t my-data-science-app` : étiquette l’image « my-data-science-app ».
- `.` : le point final désigne le contexte de construction (le répertoire courant).

5. Exécuter l’image Docker :

Une fois la construction terminée, exécutez votre nouvelle image en tant que conteneur :

`sudo docker run -p 8888:8888 my-data-science-app`
Détails de la commande :

- `docker run` : lance un conteneur à partir de l’image spécifiée.
- `-p 8888:8888` : mappe le port 8888 de votre machine locale vers le port 8888 du conteneur pour accéder au serveur Jupyter via`localhost:8888` .
- `my-data-science-app` : image à exécuter.

### Étape 5 : utiliser Docker Compose

Docker Compose est indispensable pour gérer des applications multi-conteneurs. Dans un projet de data science, vous pouvez avoir des conteneurs distincts pour un notebook Jupyter, une base de données et un outil de visualisation.

Avec Docker Compose, vous définissez ces services dans un unique fichier YAML, ce qui permet de lancer l’ensemble de votre environnement en une seule commande.

1. Créer un fichier `docker-compose.yml` :

- Dans votre répertoire de projet, créez un fichier `docker-compose.yml` et ajoutez les lignes suivantes :

```
version: '3.8'
services:
  jupyter:
        image: jupyter/scipy-notebook:latest
        volumes:
        - ./notebooks:/home/joelwembo/work
        ports:
        - "8888:8888"
        environment:
        - JUPYTER_ENABLE_LAB=yes
 
  postgres:
        image: postgres:13-alpine
        environment:
        POSTGRES_USER: myuser
        POSTGRES_PASSWORD: mypassword
        POSTGRES_DB: mydatabase
        volumes:
        - postgres_data:/var/lib/postgresql/data
        ports:
        - "5432:5432"
 
  redis:
        image: redis:alpine
        ports:
        - "6379:6379"
 
volumes:
  postgres_data:
 
```
Voici ce que fait ce fichier Docker Compose :

- `version: '3.8'` : version du format de fichier Docker Compose, largement utilisée.
- `services:` : section qui définit les différents conteneurs (services) de votre application.
- `jupyter:` : service pour l’environnement Jupyter, essentiel à l’analyse interactive.
- `image: jupyter/scipy-notebook:latest` : image Docker pour Jupyter incluant NumPy, pandas, matplotlib, etc.
- `volumes: - ./notebooks:/home/joelwembo/work` : monte le dossier`notebooks` de l’hôte dans`/home/joelwembo/work` du conteneur pour persister et partager vos notebooks. Remarque : il s’agit de mon chemin personnel, le vôtre sera différent.
- `ports: - "8888:8888"` : mappe le port 8888 de l’hôte sur celui du conteneur (port Jupyter par défaut).
- `environment: - JUPYTER_ENABLE_LAB=yes` : active JupyterLab, interface plus puissante pour travailler avec Jupyter.
- `postgres:` : service PostgreSQL, souvent utilisé pour stocker et gérer des jeux de données volumineux.
- `image: postgres:13-alpine` : image PostgreSQL 13 basée sur Alpine Linux, légère.
- `environment:` : variables d’environnement pour configurer l’utilisateur, le mot de passe et le nom de la base.
- `volumes: - postgres_data:/var/lib/postgresql/data` : volume Docker pour persister les données de la base au-delà du cycle de vie du conteneur.
- `ports: - "5432:5432"` : expose le port PostgreSQL par défaut (5432).
- `redis:` : service Redis, utilisé comme magasin de données en mémoire ou cache rapide.
- `image: redis:alpine` : image Redis basée sur Alpine, légère.
- `ports: - "6379:6379"` : expose le port Redis par défaut (6379).
- `volumes:` : section qui définit les volumes nommés partagés entre services ou persistants entre redémarrages.
- `postgres_data:` : crée un volume nommé pour les données PostgreSQL.

Pour lancer votre application multi-conteneurs, exécutez simplement :

`sudo docker compose up`
Cette commande construit les images (si nécessaire) et démarre les conteneurs définis dans votre `docker-compose.yml`. Elle met en route l’ensemble des services et leur permet de communiquer comme prévu.

Vous venez de déployer votre première application Docker ! Plutôt motivant, n’est-ce pas ? Passons maintenant à un plan pour votre formation continue.

## Exemple de plan d’apprentissage Docker

Voici un plan semaine par semaine que vous pouvez suivre et adapter à vos besoins.

Si vous préférez une voie plus balisée, le parcours Containerization and Virtualization with Docker and Kubernetes est fait pour vous. Il contient quatre cours essentiels.

### Semaine 1 : se familiariser avec les bases de Docker

- Objectif : comprendre les concepts clés de Docker et les commandes de base.
- Tâches :
- Installer Docker et configurer Docker Desktop.
- Apprendre les commandes : `docker run` ,`docker ps` ,`docker stop` ,`docker rm` , etc.
- Lancer des conteneurs simples pour l’exploration (Ubuntu, Alpine) et tester des commandes de base.
- Explorer les images officielles Docker et exécuter des conteneurs d’outils de visualisation simples (ex. : matplotlib).
- Ressources : Containerization and Virtualization Concepts, Introduction to Docker.

### Semaine 2 : créer et exécuter des images Docker pour les outils data

- Objectif : apprendre à créer et gérer des images Docker personnalisées.
- Tâches :
- Écrire un Dockerfile pour un environnement d’analyse (Python + pandas, NumPy).
- Construire l’image avec `docker build` .
- Exécuter des conteneurs à partir de votre image et tester des outils comme Jupyter Notebook.
- Créer des Dockerfiles pour TensorFlow et PostgreSQL, avec une configuration et un chaînage corrects.
- Ressources : Introduction to Docker.

### Semaine 3 : découvrir Docker Compose

- Objectif : comprendre et utiliser Docker Compose pour des environnements multi-conteneurs.
- Tâches :
- Installer Docker Compose et apprendre les bases.
- Écrire des fichiers `docker-compose.yml` pour définir des applications multi-conteneurs.
- S’exercer, par exemple, avec une application web et une base (ex. : Flask + PostgreSQL).
- Tester différentes configurations et dépendances de services.
- Ressources : Intermediate Docker.

### Semaine 4 : approfondir le réseau et les volumes Docker

- Objectif : maîtriser les capacités réseau et stockage de Docker.
- Tâches :
- Découvrir les types de réseaux Docker (bridge, host, overlay) et leur création.
- Créer des réseaux personnalisés et y connecter des conteneurs.
- Explorer les volumes pour le stockage persistant et s’exercer à les gérer.
- Mettre en place une configuration où les conteneurs communiquent via un réseau personnalisé et utilisent des volumes pour la persistance.
- Ressources : Intermediate Docker.

### Semaine 5 : déployer et gérer des conteneurs en production

- Objectif : comprendre le déploiement et l’exploitation des conteneurs en production.
- Tâches :
- Appliquer les bonnes pratiques de déploiement (Docker Hub ou registre privé).
- Explorer l’orchestration avec Docker Swarm : création et gestion de services.
- Comprendre la montée en charge des conteneurs et la gestion des logs.
- Déployer une application conteneurisée sur un environnement de staging ou de production.
- Ressources : Intermediate Docker.

### Semaine 6 : introduction à Kubernetes pour l’orchestration

- Objectif : se familiariser avec Kubernetes pour faire évoluer et gérer des applications conteneurisées.
- Tâches :
- Installer et configurer un environnement Kubernetes local (Minikube ou Kind).
- Apprendre les bases : pods, services, déploiements, namespaces.
- Déployer une application conteneurisée simple sur Kubernetes.
- Explorer la mise à l’échelle et la gestion d’applications, par exemple pour un outil comme Jupyter.
- Ressources : Introduction to Kubernetes.

## Conseils et méthodes pour apprendre Docker

Pour finir, voici quelques conseils et bonnes pratiques pour accélérer votre montée en compétences sur Docker.

### Pratiquez régulièrement

La clé pour maîtriser Docker, c’est une pratique régulière et concrète. Commencez par des projets personnels, comme conteneuriser un site web simple ou mettre en place un environnement de développement local.

À mesure que vous gagnez en aisance, attaquez-vous à des tâches plus complexes : construire une application multi-conteneurs ou déployer un serveur web.

Expérimenter régulièrement différents cas d’usage — comme créer des images Docker personnalisées ou configurer une pipeline CI/CD — renforcera votre compréhension et ancrera les concepts. Participez aussi à des challenges Docker en ligne pour vous exercer de manière structurée et motivante.

### Exploitez les ressources en ligne

Les ressources en ligne sont précieuses pour apprendre Docker. La documentation officielle est un excellent point de départ, fiable et à jour.

Au-delà, il existe de nombreux cours complets sur des plateformes comme DataCamp qui vous guideront pas à pas.

Si vous préférez le format vidéo, les tutoriels YouTube de la chaîne officielle Docker et d’autres formateurs proposent des contenus utiles et accessibles pour consolider vos acquis.

### Rejoignez la communauté Docker

S’impliquer dans la communauté Docker est un excellent accélérateur. Participez aux forums (Docker Community Forums, Reddit) pour échanger, poser vos questions et partager vos retours avec d’autres apprenants et professionnels.

Assister à des meetups, webinaires ou conférences Docker est aussi un très bon moyen de rester à jour et d’élargir votre réseau.

### Contribuez à l’open source

Contribuer à des projets open source utilisant Docker est une excellente façon d’acquérir une expérience concrète et pertinente.

Des plateformes comme GitHub offrent des opportunités de collaborer, d’apprendre des autres et d’appliquer vos compétences dans des contextes variés. Cette collaboration expose à différentes approches et solutions, enrichissant votre compréhension des capacités de Docker.

### Restez à jour

Enfin, rester informé des dernières évolutions de Docker est essentiel pour maintenir votre niveau. Suivez le blog officiel et les notes de version pour découvrir les nouveautés. Les discussions communautaires sur les forums et les réseaux sociaux vous aideront aussi à repérer les tendances et bonnes pratiques pour affiner continuellement vos compétences.

Avec une pratique régulière, un engagement actif dans la communauté et un apprentissage continu, vous bâtirez de solides bases sur Docker et resterez prêt à adopter les dernières avancées du domaine.

## Conclusion

Docker a révolutionné le déploiement d’applications et la gestion des données en emballant applications et dépendances dans des conteneurs isolés. Il résout des problèmes classiques d’incohérence d’environnements et garantit des workflows constants, évolutifs et efficaces pour les professionnels des données.

Ce guide vous a présenté les étapes essentielles pour maîtriser Docker, des concepts clés aux déploiements pratiques. Pour aller plus loin, explorez des ressources comme les cours Introduction to Docker et Intermediate Docker sur DataCamp. En progressant, il est tout aussi important de développer vos compétences en orchestration avec Kubernetes — découvrez le cours Introduction to Kubernetes pour démarrer.

En pratiquant régulièrement, en exploitant les ressources disponibles et en restant actif dans la communauté, vous libérerez tout le potentiel de Docker pour gérer des environnements data complexes.

## Obtenez une certification pour le poste de Data Engineer de vos rêves

Nos programmes de certification vous aident à vous démarquer et à prouver aux employeurs potentiels que vos compétences sont adaptées à l'emploi.

## FAQs

### Combien de temps faut-il pour apprendre Docker ?

**Le temps nécessaire pour apprendre Docker dépend de votre expérience. Pour les débutants, quelques jours suffisent pour se familiariser avec les bases (conteneurs, images, Dockerfiles). Pour aller plus loin — Docker Compose, Kubernetes — comptez quelques semaines de pratique régulière.**

### Dois-je apprendre Kubernetes après Docker ?

**Docker est essentiel pour la conteneurisation, tandis que Kubernetes est précieux pour gérer et orchestrer des déploiements à grande échelle sur plusieurs conteneurs. Après avoir maîtrisé Docker, apprendre Kubernetes est vivement recommandé pour les applications complexes ou lorsque vous devez automatiser la mise à l’échelle et la gestion des conteneurs.**

### Quels projets réaliser pour pratiquer Docker ?

**Pour acquérir de l’expérience pratique avec Docker, commencez par de petits projets : conteneuriser un site personnel ou créer un environnement de développement local pour une application web. Ensuite, construisez une application multi-conteneurs avec Docker Compose ou déployez une pipeline CI/CD simple. Vous pouvez aussi vous exercer en contribuant à des projets open source qui utilisent Docker.**

### Docker est-il indispensable pour les professionnels des données ?

**Oui, Docker est extrêmement utile pour les professionnels des données. Il permet de créer des environnements reproductibles pour garantir que vos pipelines fonctionnent de manière cohérente du développement à la production. Docker simplifie aussi la gestion des dépendances et renforce la collaboration entre équipes : un outil incontournable pour les workflows data modernes.**

### Quelles sont les meilleures ressources pour apprendre Docker ?

Parmi les meilleures ressources pour démarrer avec Docker :

- La documentation officielle de Docker
- Les cours de DataCamp Introduction to Docker et Intermediate Docker
- Des tutoriels YouTube de la chaîne officielle de Docker
- Les forums communautaires Docker et des dépôts GitHub pour pratiquer et collaborer

Architecte de solutions cloud certifié AWS, DevOps, ingénieur cloud avec une compréhension approfondie de l'architecture et des concepts de haute disponibilité. Je possède des connaissances en matière d'ingénierie du cloud et de DevOps et je sais utiliser les ressources open-source pour exécuter des applications d'entreprise. Je construis des applications basées sur le cloud en utilisant AWS, AWS CDK, AWS SAM, CloudFormation, Serverless Framework, Terraform et Django.