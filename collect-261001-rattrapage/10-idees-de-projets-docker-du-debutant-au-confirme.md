Cours

L'expérience pratique est essentielle pour maîtriser Docker. Docker est un outil essentiel au développement de logiciels modernes et à la science des données, qui vous permet de créer, de déployer et de gérer des applications dans des conteneurs.

Dans cet article, je vous propose des exemples de projets Docker de niveau débutant, intermédiaire et avancé, axés sur les constructions en plusieurs étapes, l'optimisation des images Docker et l'application de Docker dans le domaine de la science des données. Ces projets sont conçus pour approfondir votre compréhension de Docker et améliorer vos compétences pratiques.

## Démarrer avec les projets Docker

Avant de vous lancer dans les projets, assurez-vous que Docker est installé sur votre machine. En fonction de votre système d'exploitation (Windows, macOS, Linux), vous pouvez télécharger Docker depuis le site officiel de Docker.

Vous devrez également avoir des connaissances de base dans les domaines suivants

- Dockerfiles (pour définir ce qui se trouve à l'intérieur de vos conteneurs)
- Docker Compose (pour les applications multi-conteneurs)
- Commandes CLI de base telles que `docker build` ,`docker run` ,`docker-compose up` , etc.

Si vous avez besoin de rafraîchir vos connaissances sur les concepts ci-dessus, consultez les cours Introduction à Docker ou Concepts de conteneurisation et de virtualisation.

Commençons !

## Projets Docker pour les débutants

Lorsque vous débutez avec Docker, il est important de choisir des projets qui correspondent à votre niveau de compétence tout en vous incitant à apprendre de nouveaux concepts. Voici quelques idées de projets pour vous aider à démarrer :

### Projet 1 : Mise en place d'un serveur web simple

Dans ce projet, vous allez créer un conteneur Docker qui exécute un serveur web de base à l'aide de Nginx. Nginx est l'un des serveurs web open-source les plus populaires pour le reverse proxying, l'équilibrage de charge, etc. À la fin de ce projet, vous aurez appris à créer et à exécuter des conteneurs avec Docker et à exposer des ports pour que l'application soit accessible depuis votre machine locale.

Niveau de difficulté : Débutant

Technologies utilisées : Docker, Nginx

#### Instructions étape par étape

- Installez Docker : Assurez-vous que Docker est installé sur votre système.
- Créez le répertoire du projet : Créez un nouveau dossier et un fichier `index.html` à l'intérieur qui sera servi par Nginx.
- Écrivez le fichier Docker : Un Dockerfile est un script qui définit l'environnement du conteneur. Il indique à Docker l'image de base à utiliser, les fichiers à inclure et les ports à exposer :

```
FROM nginx:alpine
COPY ./index.html /usr/share/nginx/html
EXPOSE 80
```
- Construisez l'image Docker : Naviguez dans le dossier de votre projet et construisez l'image à l'aide de :

`docker build -t my-nginx-app .`
- Exécutez le conteneur : Démarrez le conteneur et mappez le port 80 du conteneur au port 8080 de votre machine :

`docker run -d -p 8080:80 my-nginx-app`
- Accédez au serveur web : Ouvrez votre navigateur et naviguez vers http://localhost:8080 pour voir la page que vous avez créée.

### Projet 2 : Dockeriser un script Python

Ce projet consiste à conteneuriser un simple script Python qui traite les données d'un fichier CSV à l'aide de la bibliothèque pandas. L'objectif est d'apprendre à gérer les dépendances et à exécuter des scripts Python à l'intérieur de conteneurs Docker, ce qui rend le script portable et exécutable dans n'importe quel environnement.

Niveau de difficulté : Débutant

Technologies utilisées : Docker, Python, pandas

#### Instructions étape par étape

- Écrivez le script Python : Créez un script nommé `process_data.py` qui lit et traite un fichier CSV. Voici un exemple de script :

```
import pandas as pd
df = pd.read_csv('data.csv')
print(df.describe())
```
- Créez un fichier `requirements.txt` : Ce fichier répertorie les bibliothèques Python dont le script a besoin. Dans ce cas, nous n'avons besoin que de`pandas` :

`pandas`
- Écrivez le fichier Docker : Ce fichier définit l'environnement du script Python :

```
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "process_data.py"]
```
- Construisez l'image Docker :

`docker build -t python-script .`
- Exécutez le conteneur :

`docker run -v $(pwd)/data:/app/data python-script`
### Projet 3 : Construire une application multi-conteneurs simple

Ce projet vous aidera à vous familiariser avec Docker Compose en construisant une application multi-conteneurs. Vous créerez une application web simple en utilisant Flask comme interface et MySQL comme base de données. Docker Compose vous permet de gérer plusieurs conteneurs qui fonctionnent ensemble.

Niveau de difficulté : Débutant

Technologies utilisées : Docker, Docker Compose, Flask, MySQL

#### Instructions étape par étape

- Écrivez l'application Flask : Créez une application Flask simple qui se connecte à une base de données MySQL et affiche un message. En voici un exemple :

```
from flask import Flask
import mysql.connector
 
app = Flask(__name__)
 
def get_db_connection():
 	connection = mysql.connector.connect(
	 host="db",
	 user="root",
	 password="example",
	 database="test_db"
 	)
 	return connection
 
@app.route('/')
def hello_world():
 	connection = get_db_connection()
 	cursor = connection.cursor()
 	cursor.execute("SELECT 'Hello, Docker!'")
 	result = cursor.fetchone()
 	connection.close()
 	return str(result[0])
 
if __name__ == "__main__":
 	app.run(host='0.0.0.0')
```
- Créez le fichier `docker-compose.yml` : Docker Compose définit et exécute des applications Docker multi-conteneurs. Dans ce fichier, vous définirez l'application Flask et les services de base de données MySQL :

```
version: '3'
services:
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: test_db
    ports:
      - "3306:3306"
    volumes:
      - db_data:/var/lib/mysql
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - db
    environment:
      FLASK_ENV: development
    volumes:
      - .:/app
volumes:
  db_data:
```
- Écrivez le fichier Docker pour Flask : Cela créera l'image Docker pour l'application Flask :

```
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```
- Construisez et exécutez les conteneurs : Utilisez Docker Compose pour afficher l'ensemble de l'application :

`docker-compose up --build`
- **Accédez à l'application Flask :** Allez sur http://localhost:5000 dans votre navigateur.

## Devenez ingénieur en données

## Projets Docker de niveau intermédiaire

Les projets suivants sont destinés à ceux qui ont une solide compréhension des bases de Docker. Ils introduiront des concepts plus complexes, tels que les constructions en plusieurs étapes et les techniques d'optimisation.

### Projet 4 : Construction en plusieurs étapes d'une application Node.js

Les constructions en plusieurs étapes permettent de réduire la taille des images Docker en séparant les environnements de construction et d'exécution. Dans ce projet, vous allez conteneuriser une application Node.js en utilisant des constructions en plusieurs étapes.

Niveau de difficulté : Intermédiaire

Technologies utilisées : Docker, Node.js, Nginx

#### Instructions étape par étape

- Créez une application Node.js simple : Ecrivez un serveur Node.js basique qui renvoie un message simple. En voici un exemple :

```
const express = require('express');
const app = express();
 
app.get('/', (req, res) => res.send('Hello from Node.js'));
 
app.listen(3000, () => console.log('Server running on port 3000'));
```
- Écrire le fichier Docker avec une construction en plusieurs étapes : La première étape consiste à créer l'application et la seconde à l'exécuter avec une image de base plus légère.

```
# Stage 1: Build
FROM node:14 as build-stage
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
# Add the following line if there's a build step for the app
# RUN npm run build
# Stage 2: Run
FROM node:14-slim
WORKDIR /app
COPY --from=build-stage /app .
EXPOSE 3000
ENV NODE_ENV=production
CMD ["node", "server.js"]
```
- Construisez l'image :

`docker build -t node-multi-stage .`
- Exécutez le conteneur :

`docker run -p 3000:3000 node-multi-stage`
### Projet 5 : Dockeriser un modèle d'apprentissage automatique avec TensorFlow

Ce projet consistera à conteneuriser un modèle d'apprentissage automatique à l'aide de TensorFlow. L'objectif est de créer un environnement portable dans lequel vous pouvez exécuter des modèles TensorFlow sur différents systèmes sans vous soucier de la configuration sous-jacente.

Niveau de difficulté : Intermédiaire

Technologies utilisées : Docker, TensorFlow, Python

#### Instructions étape par étape

- Installez TensorFlow dans un script Python : Créez un script Python `model.py` qui charge et exécute un modèle TensorFlow pré-entraîné :

```
import tensorflow as tf
model = tf.keras.applications.MobileNetV2(weights='imagenet')
print("Model loaded successfully")
```
- Écrivez le fichier Docker : Définissez l'environnement de TensorFlow dans Docker :

```
FROM tensorflow/tensorflow:latest
WORKDIR /app
COPY . .
CMD ["python", "model.py"]
```
- Construisez l'image :

`docker build -t tensorflow-model .`
- Exécutez le conteneur :

`docker run tensorflow-model`
### Projet 6 : Créer un environnement de science des données avec Jupyter et Docker

Ce projet se concentre sur la création d'un environnement de science des données reproductible en utilisant Docker et les carnets Jupyter. L'environnement comprendra des bibliothèques Python populaires telles que pandas, NumPy et scikit-learn.

Niveau de difficulté : Intermédiaire

Technologies utilisées : Docker, Jupyter, Python, scikit-learn.

#### Instructions étape par étape

- Créez le fichier `docker-compose.yml` : Définissez le service Jupyter Notebook et les bibliothèques nécessaires. En voici un exemple :

```
version: '3'
services:
  jupyter:
    	image: jupyter/scipy-notebook
    	ports:
    	- "8888:8888"
    	volumes:
    	- ./notebooks:/home/joelwembo/work
```
- Démarrez le Jupyter Notebook : Utilisez Docker Compose pour démarrer le Jupyter Notebook.

`docker-compose up`
- Accédez au carnet de notes Jupyter : Ouvrez votre navigateur et allez sur http://localhost:8888.

## Projets Docker de niveau avancé

Ces projets de niveau avancé se concentreront sur des applications du monde réel et des concepts Docker avancés, tels que les pipelines d'apprentissage profond et les pipelines de données automatisés.

### Projet 7 : Réduire la taille d'une image Docker pour une application Python.

Dans ce projet, vous optimiserez une image Docker pour une application Python en utilisant des images de base minimales comme Alpine Linux et en mettant en œuvre des constructions en plusieurs étapes pour que la taille de l'image soit la plus petite possible.

Niveau de difficulté : Avancé

Technologies utilisées : Docker, Python, Alpine Linux

#### Instructions étape par étape

- Écrivez le script Python : Créez un script qui analyse les données à l'aide de pandas. Voici un exemple de script :

```
import pandas as pd
df = pd.read_csv('data.csv')
print(df.head())
```
- Optimisez le fichier Docker : Utilisez les constructions en plusieurs étapes et Alpine Linux pour créer une image légère.

```
# Stage 1: Build stage
FROM python:3.9-alpine as build-stage
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY script.py .
# Stage 2: Run stage
FROM python:3.9-alpine
WORKDIR /app
COPY --from=build-stage /app/script.py .
CMD ["python", "script.py"]
```
- Construisez l'image :

`docker build -t optimized-python-app .`
### Projet 8 : Dockeriser un pipeline d'apprentissage profond avec PyTorch

Ce projet consiste à conteneuriser un pipeline d'apprentissage profond en utilisant PyTorch. L'accent est mis sur l'optimisation du fichier Docker en termes de performance et de taille, ce qui facilite l'exécution de modèles d'apprentissage profond dans différents environnements.

Niveau de difficulté : Avancé

Technologies utilisées : Docker, PyTorch, Python

#### Instructions étape par étape

- Installez PyTorch dans un script Python : Créez un script qui charge un modèle PyTorch pré-entraîné et effectue l'inférence. En voici un exemple :

```
import torch
model = torch.hub.load('pytorch/vision', 'resnet18', pretrained=True)
print("Model loaded successfully")
```
- Écrivez le fichier Docker : Définissez l'environnement de PyTorch :

```
FROM pytorch/pytorch:1.9.0-cuda11.1-cudnn8-runtime
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY model.py .
CMD ["python", "model.py"]
```
- Construisez l'image :

`docker build -t pytorch-model .`
- Exécutez le conteneur :

`docker run pytorch-model` ### Projet 9 : Automatisation des pipelines de données avec Apache Airflow et Docker.

Dans ce projet, vous allez configurer et conteneuriser un environnement Apache Airflow pour automatiser les pipelines de données. Apache Airflow est un outil populaire d'orchestration de flux de travail complexes largement utilisé dans l'ingénierie des données.

Niveau de difficulté : Avancé

Technologies utilisées : Docker, Apache Airflow, Python, PostgreSQL

#### Instructions étape par étape

- Créez le fichier `docker-compose.yml` : Définissez les services Airflow et la base de données PostgreSQL :

```
version: '3'
services:
  postgres:
    image: postgres:latest
    environment:
      POSTGRES_USER: airflow
      POSTGRES_PASSWORD: airflow
      POSTGRES_DB: airflow
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
  webserver:
    image: apache/airflow:latest
    environment:
      AIRFLOW__CORE__SQL_ALCHEMY_CONN: postgresql+psycopg2://airflow:airflow@postgres/airflow
      AIRFLOW__CORE__EXECUTOR: LocalExecutor
    depends_on:
      - postgres
    ports:
      - "8080:8080"
    volumes:
      - ./dags:/opt/airflow/dags
    command: ["webserver"]
  scheduler:
    image: apache/airflow:latest
    environment:
      AIRFLOW__CORE__SQL_ALCHEMY_CONN: postgresql+psycopg2://airflow:airflow@postgres/airflow
      AIRFLOW__CORE__EXECUTOR: LocalExecutor
    depends_on:
      - postgres
      - webserver
    volumes:
      - ./dags:/opt/airflow/dags
    command: ["scheduler"]
volumes:
  postgres_data:
```
- Démarrez l'environnement Airflow : Utilisez Docker Compose pour faire apparaître l'environnement Airflow :

`docker-compose up`
- Accédez à l'interface utilisateur Airflow : Ouvrez votre navigateur et allez sur http://localhost:8080.

### Projet 10 : Déployer une API de science des données avec FastAPI et Docker

Créez et déployez une API de science des données à l'aide de FastAPI. Vous conteneuriserez l'API à l'aide de Docker et vous vous attacherez à l'optimiser pour les environnements de production.

Niveau de difficulté : Avancé

Technologies utilisées : Docker, FastAPI, Python, scikit-learn

#### Instructions étape par étape

- Écrivez l'application FastAPI : Créez une API simple qui utilise un modèle d'apprentissage automatique pour les prédictions. En voici un exemple :

```
from fastapi import FastAPI
import pickle
 
app = FastAPI()
with open("model.pkl", "rb") as f:
   model = pickle.load(f)
 
@app.post("/predict/")
def predict(data: list):
   return {"prediction": model.predict(data)}
```
- Écrivez le fichier Docker : Créez un fichier Docker qui définit l'environnement pour FastAPI :

```
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
```
- Construisez l'image :

`docker build -t fastapi-app .`
- Exécutez le conteneur :

`docker run -p 8000:8000 fastapi-app`
## Conseils pour travailler sur des projets Docker

Pendant que vous travaillez sur ces projets, gardez les conseils suivants à l'esprit :

- Commencez modestement : Commencez par des projets légèrement difficiles, puis passez à des tâches plus complexes. Il est essentiel d'acquérir de l'assurance pour les tâches les plus simples.
- Consignez vos progrès : Conservez un cursus détaillé de vos projets afin de suivre votre apprentissage et de vous en servir comme référence pour vos projets futurs.
- Rejoignez les communautés Docker : Participez à des forums en ligne et à des rencontres locales pour partager vos expériences, poser des questions et apprendre des autres.
- Expérimentez et personnalisez : N'ayez pas peur de modifier les projets, d'essayer différentes approches et d'explorer les nouvelles fonctionnalités de Docker.
- Continuez à apprendre : Continuez à développer vos connaissances sur Docker en explorant des sujets et des outils avancés tels que Kubernetes, Docker Swarm ou l'architecture microservices.

## Conclusion

La maîtrise de Docker ne se limite pas à l'apprentissage des commandes et des configurations. Il s'agit de comprendre comment Docker s'intègre dans le développement d'applications modernes, les flux de travail de la science des données et la gestion de l'infrastructure.

Les projets présentés dans ce guide vous donnent quelques idées pour acquérir les compétences de base et l'expérience pratique nécessaires pour exceller dans des scénarios réels.

À ce stade, je vous suggère de consolider vos connaissances en suivant ces cours :

## Devenez ingénieur en données

## FAQ

### Quelles sont les meilleures pratiques pour écrire des Dockerfiles efficaces ?

**Les meilleures pratiques pour écrire des Dockerfiles efficaces comprennent la minimisation du nombre de couches en combinant des commandes, l'utilisation de constructions en plusieurs étapes pour réduire la taille de l'image, la sélection d'images de base légères, la mise en cache des dépendances et l'évitement d'inclure des fichiers inutiles dans l'image finale.**

### Qu'est-ce qu'une construction en plusieurs étapes dans Docker ?

**Une construction en plusieurs étapes est une méthode permettant d'optimiser les images Docker en séparant les environnements de construction et d'exécution. Cela permet d'obtenir des images plus petites et plus sûres.**

### Comment réduire la taille d'une image Docker ?

**Utilisez des images de base minimales, gérez efficacement les dépendances et utilisez des constructions en plusieurs étapes pour réduire la taille des images et améliorer les performances.**

### Comment résoudre les erreurs courantes lors de la création d'images Docker ?

**Les erreurs les plus courantes lors de la création d'images Docker sont les problèmes de permission, la syntaxe incorrecte du fichier Docker et l'échec de l'installation des dépendances. Pour résoudre le problème, vérifiez les journaux de construction de Docker, assurez-vous que vous utilisez l'image de base correcte et confirmez que les chemins d'accès ou les autorisations de fichiers sont définis correctement. Des outils tels que `docker build --no-cache` peuvent vous aider à identifier les problèmes de mise en cache.**

### Puis-je utiliser Docker avec Kubernetes pour ces projets ?

**Oui, une fois que vous êtes à l'aise avec Docker, Kubernetes peut être l'étape suivante. Kubernetes permet de gérer les applications conteneurisées à grande échelle. Vous pouvez déployer vos projets Docker sur un cluster Kubernetes pour gérer plusieurs instances, gérer la mise à l'échelle et automatiser les déploiements.**

### Quelles sont les meilleures pratiques pour gérer les volumes Docker et les données persistantes ?

**Lorsque vous travaillez avec des volumes Docker, il est important d'utiliser des volumes nommés pour garantir la persistance des données lors des redémarrages de conteneurs. Sauvegardez régulièrement vos volumes et surveillez les goulets d'étranglement dus aux E/S sur disque. Évitez de stocker directement des données sensibles dans des conteneurs ; utilisez plutôt des solutions de stockage sécurisées ou des bases de données externes.**

### À quoi sert la directive ENTRYPOINT dans un fichier Docker ?

**La directive `ENTRYPOINT` d'un fichier Docker spécifie la commande qui sera toujours exécutée au démarrage du conteneur. Il permet de traiter le conteneur comme un exécutable, où des arguments peuvent être passés pendant l'exécution, ce qui améliore la flexibilité.**

### Quelle est la différence entre CMD et ENTRYPOINT dans un fichier Docker ?

**Les adresses `CMD` et `ENTRYPOINT` indiquent les commandes à exécuter au démarrage d'un conteneur. Cependant, `CMD` fournit des arguments par défaut qui peuvent être remplacés, tandis que `ENTRYPOINT` définit la commande qui s'exécute toujours. `ENTRYPOINT` est utile pour créer des conteneurs qui agissent comme des exécutables, tandis que `CMD` est plus flexible pour spécifier des commandes par défaut.**

Architecte de solutions cloud certifié AWS, DevOps, ingénieur cloud avec une compréhension approfondie de l'architecture et des concepts de haute disponibilité. Je possède des connaissances en matière d'ingénierie du cloud et de DevOps et je sais utiliser les ressources open-source pour exécuter des applications d'entreprise. Je construis des applications basées sur le cloud en utilisant AWS, AWS CDK, AWS SAM, CloudFormation, Serverless Framework, Terraform et Django.