Cours

*Logo GitHub. Source : GitHub Logos and Usage* 

Imaginez que vous travaillez sur un projet de data science et que vous avez bien avancé. Soudain, un bug survient. Vous aimeriez revenir à la dernière version fonctionnelle, mais vous ne vous souvenez plus de toutes les modifications effectuées. Ou peut-être collaborez-vous avec d'autres personnes, et fusionner les contributions de chacun devient un casse-tête. Si ces situations vous parlent, vous n'êtes pas seul.

Ces problèmes courants se résolvent avec GitHub, la plateforme de référence pour la gestion de versions et la collaboration. Dans cet article, nous verrons comment GitHub peut transformer la manière dont vous pilotez vos projets data. Nous explorerons également des techniques de collaboration et des stratégies pour gagner en productivité.

Commençons par les fondamentaux de la gestion de versions.

## Qu'est-ce que la gestion de versions ?

La gestion de versions est un système qui enregistre l'historique des modifications apportées à des fichiers dans le temps. Elle permet à plusieurs personnes de travailler ensemble sur un projet tout en conservant une trace précise des évolutions. Sans gestion de versions, le suivi des changements de code devient vite chaotique et source d'erreurs, surtout en équipe lorsque plusieurs contributeurs travaillent en parallèle sur des parties différentes du code.

## À quoi sert GitHub ?

Comme vous vous en doutez, GitHub excelle en gestion de versions. Mais la plateforme va bien au-delà, avec de nombreux autres usages, notamment :

- 
**Créer un portfolio de projets :** GitHub vous permet de créer un profil public pour présenter vos compétences et vos projets data à des recruteurs ou à vos collègues.
- 
**Collaborer :** GitHub facilite le travail d'équipe sur des projets, le partage d'extraits de code et la relecture mutuelle des contributions.
- 
**Contribuer à l'open source :** Avec GitHub, vous pouvez explorer et contribuer à des projets open source en data science, accélérant ainsi votre apprentissage et l'innovation.

## Comment fonctionne GitHub ?

Pour tirer pleinement parti de GitHub, il est essentiel d'en comprendre les composants clés et leur fonctionnement conjoint.

- **Dépôts (repositories) :** Ce sont des dossiers qui stockent les fichiers de votre projet et l'historique de leurs versions. Pensez-y comme à une armoire numérique pour vos projets data. Chaque dépôt a une URL unique et contient des fichiers, des branches et des commits.
- **Forks :** Un fork est une copie personnelle du dépôt d'un autre utilisateur. Vous pouvez y apporter des changements de manière indépendante, puis proposer de les réintégrer au dépôt d'origine.
- **Pull requests :** Les PR sont un moyen formel de proposer vos modifications au propriétaire du projet pour relecture et fusion. Elles facilitent la revue de code et la collaboration.
- **Issues :** Elles servent à suivre des tâches, des bugs ou des améliorations.
- **Branches :** Une branche est une version parallèle d'un dépôt. Créez des branches pour développer une fonctionnalité ou corriger un problème, puis fusionnez-les dans la branche principale lorsqu'elles sont prêtes. Pour en savoir plus, consultez ce tutoriel sur Git Clone Branch.
- **Fusion (merging) :** La fusion combine vos modifications avec le projet d'origine pour garder le tout organisé et à jour, par exemple en fusionnant une branche de fonctionnalité dans la branche principale.

## Git vs. GitHub

Vous vous demandez peut-être quel est le lien entre Git et GitHub. Ces deux termes sont parfois confondus, mais il existe une différence essentielle.

**Git est un système de gestion de versions distribué (DVCS) qui aide les développeurs à gérer leur code. Il suit les changements et permet de créer différentes versions, ou branches, du code, ce qui facilite le travail collaboratif. Git propose aussi des fonctionnalités comme la zone d'index (staging) et l'historique des commits, offrant une traçabilité fine des modifications.**

GitHub, pour sa part, ajoute des fonctionnalités comme le contrôle d'accès, le suivi des bugs, la gestion des tâches et des wikis, ce qui simplifie la collaboration sur les projets. Avec GitHub, vous pouvez gérer votre code, suivre les changements, relire les contributions et discuter des problèmes — le tout au même endroit. La plateforme s'intègre en outre à de nombreux outils et services pour fluidifier les workflows de développement. 

| Catégorie | Git | GitHub | 
| Définition | Système de gestion de versions distribué | Plateforme web construite au-dessus de Git | 
| Objectif | Aide à gérer le code, suivre les changements et créer des branches | Héberge des dépôts Git et fournit des outils de collaboration supplémentaires | 
| Fonctionnalités | Zone d'index, historique des commits, branches et fusion | Contrôle d'accès, suivi des bugs, gestion des tâches, wikis et intégrations | 
| Bénéfice | Permet le travail collaboratif et un suivi détaillé des modifications de code | Améliore la collaboration, la gestion de projet et les processus de revue de code | 

## Comment utiliser GitHub

Jusqu'ici, nous avons défini GitHub et la gestion de versions, et comparé Git à GitHub. Passons maintenant à la pratique.

Nous allons d'abord voir comment créer un compte GitHub, personnaliser votre expérience et choisir une formule. Ensuite, nous créerons un dépôt, en le configurant, en ajoutant une description et en gérant sa visibilité. Puis nous aborderons la création de branches pour travailler sur différentes versions du projet, avant de passer aux commits, où nous apprendrons à modifier des fichiers et à documenter les changements.

### Créer un compte

Voici les étapes pour créer un compte GitHub et démarrer :

1. Rendez-vous sur GitHub et cliquez sur le bouton **Sign****up** .
2. Suivez les instructions pour créer votre compte. Indiquez votre adresse e-mail, choisissez un nom d'utilisateur et un mot de passe.
3. Personnalisez votre expérience en choisissant une formule adaptée et en ajustant vos préférences lors de la configuration. L'offre gratuite suffit largement aux débutants et aux profils data juniors.

    *Créer un compte GitHub. Image par l'auteur*

### Créer un dépôt

Après la création du compte, la prochaine étape est de créer un dépôt. Procédez ainsi pour votre premier dépôt :

1. 
Cliquez sur l'icône **+** en haut à droite et sélectionnez**New****repository** .
2. 
Ajoutez un nom et une description, puis choisissez si le dépôt doit être public ou privé. Les dépôts publics sont visibles de tous. Les dépôts privés ne sont accessibles qu'à vous et aux collaborateurs que vous invitez.
3. 
Vous pouvez, en option, ajouter un fichier README, un fichier `.gitgnore` et une licence. Vous pourrez aussi les ajouter plus tard.
4. 
Cliquez sur **Create****repository** .

*Création d'un dépôt. Image par l'auteur*

Une fois ces étapes suivies, une fenêtre de configuration rapide pour votre nouveau dépôt s'affiche. Vous pouvez démarrer en créant un nouveau fichier ou en téléversant un fichier existant dans le dépôt.

*Configuration d'un nouveau dépôt. Image par l'auteur*

*Téléversement de fichiers. Image par l'auteur*

### Créer des branches

Une fois le dépôt prêt, créez des branches. Voici comment procéder :

1. 
Dans votre dépôt, cliquez sur **Branch:main** , près du haut de la page.
2. 
Cliquez ensuite sur le bouton **New branch** en haut à droite.
3. 
Saisissez un nom de branche et cliquez sur **Create new branch** .
4. 
Vous pouvez basculer d'une branche à l'autre via le menu déroulant des branches et sélectionner celle sur laquelle vous voulez travailler.

*Création de branches. Image par l'auteur*

### Effectuer des commits

Après la création de branches, passez aux commits. Étapes à suivre :

1. 
Accédez au fichier que vous souhaitez modifier dans votre dépôt.
2. 
Modifiez-le en cliquant sur l'icône crayon, puis effectuez vos changements dans l'éditeur de texte.
3. 
Cliquez sur **Commit****Changes** . Faites défiler jusqu'à la section**Commit****changes** . Renseignez un message de commit décrivant précisément vos modifications — c'est essentiel.

*Commits. Image par l'auteur*

### Créer une pull request

Une fois vos commits réalisés, créez une pull request. Procédure :

1. 
Allez dans l'onglet **Pull requests** de votre dépôt.
2. 
Cliquez sur **New****pull****request** . GitHub comparera automatiquement les changements entre les branches.
3. 
Vérifiez les différences pour confirmer que tout est correct, en comparant votre branche avec la branche principale.
4. 
Créez la pull request en cliquant sur **Create****pull****request** . Ajoutez un titre et une description.
5. 
Ajoutez des reviewers si nécessaire, puis soumettez. C'est facultatif, mais très utile si vous souhaitez un regard expert et des retours avant fusion.

Création d'une pull request. Image par l'auteur

### Fusionner des branches

Après relecture et approbation de votre pull request, il ne reste plus qu'à fusionner. Étapes :

1. Une fois la pull request approuvée, ouvrez-la dans l'onglet **Pull****requests** .
2. Cliquez sur le bouton **Merge****pull****request** .
3. Confirmez la fusion avec **Confirm****merge** .

Au-delà de l'interface GitHub, vous pouvez réaliser la plupart des actions avec Git en ligne de commande. Par exemple, créez un dépôt Git en local et récupérez les changements d'un dépôt distant avec la commande `git pull`. Se familiariser avec cet environnement devient crucial au fil de votre carrière. Le cours Introduction to Git de DataCamp est une excellente ressource pour progresser. Vous pouvez aussi commencer avec notre GitHub and Git Tutorial for Beginners. 

## Alternatives à GitHub

GitHub est la plateforme la plus populaire pour la gestion de versions et la collaboration. Cependant, plusieurs alternatives proposent des approches et avantages spécifiques. Il est important, pour les professionnels de la data, de connaître ces solutions susceptibles de mieux répondre à certains besoins.

Voici un aperçu de quelques alternatives à GitHub.

### GitLab

Il s'agit d'une plateforme DevOps qui offre la gestion de dépôts Git, le suivi des issues et des pipelines CI/CD. GitLab est apprécié pour son approche tout-en-un intégrant un large éventail d'outils et de services.

*Logo GitLab*

Quelques fonctionnalités clés :

- **CI/CD intégrée :** Des pipelines intégrés pour l'intégration et le déploiement continus, automatisant les tests et la mise en production.
- **Suivi des issues :** Un système pour gérer tâches, bugs et demandes de fonctionnalités, avec tableaux, jalons et étiquettes.
- **Auto DevOps :** Configure automatiquement les pipelines selon la structure du projet, ce qui accélère la mise en place.
- **Registry de conteneurs :** Un registre intégré pour gérer les images Docker.
- **Fonctions de sécurité :** Des outils de tests de sécurité intégrés pour le monitoring continu et la détection de vulnérabilités dans votre code.

### Bitbucket

Bitbucket est un produit de la suite Atlassian qui gère des dépôts Git, y compris des dépôts Mercurial. Il s'intègre naturellement avec d'autres produits Atlassian comme Jira et Trello.

*Logo Bitbucket*

Fonctionnalités clés :

- **Permissions par branche :** Contrôle fin de l'accès et des droits de modification sur les branches, pour plus de sécurité.
- **Intégration CI/CD :** Intégration avec Bitbucket Pipelines pour l'intégration et la livraison continues, avec tests et déploiements automatisés.
- **Intégration Jira :** Connexion directe à Jira pour une gestion de projet complète et le suivi de l'avancement du développement.
- **Pull requests :** Commentaires en ligne et revues de PR pour des revues de code efficaces.
- **Déploiement :** Prise en charge des déploiements via Bitbucket Pipelines pour des mises en production fluides.

### SourceForge

SourceForge est l'une des premières plateformes d'hébergement et de gestion de projets open source. Elle propose de nombreux outils pour les développeurs, dont des dépôts de code, le suivi des bugs et des fonctions de gestion de projet.

Fonctionnalités principales :

- **Hébergement de projets :** Hébergement gratuit pour les projets open source, avec bande passante et stockage illimités.
- **Statistiques de téléchargement :** Des statistiques détaillées pour suivre la popularité et la progression des projets.
- **Forums de discussion :** Des forums intégrés pour animer la communauté et assurer le support.
- **Système de mise à disposition de fichiers :** Un système avancé pour gérer les téléchargements et les versions.
- **Support SVN et Git :** Prise en charge de Subversion (SVN) et de Git.

### AWS CodeCommit

AWS CodeCommit est un service de gestion de code source entièrement managé par Amazon Web Services. Il permet de stocker et gérer votre code de manière sécurisée dans le cloud, avec une intégration fluide aux autres services AWS.

Fonctionnalités principales :

- **Scalabilité :** S'adapte à vos besoins de dépôts sans gestion d'infrastructure.
- **Sécurité :** Intégration avec AWS Identity and Access Management (IAM) pour un contrôle d'accès granulaire.
- **Intégration :** Fonctionne avec CodePipeline, CodeBuild et CodeDeploy pour une chaîne CI/CD complète.
- **Haute disponibilité :** Bâti sur l'infrastructure AWS pour une haute disponibilité et durabilité de vos dépôts.
- **Chiffrement :** Données chiffrées en transit et au repos pour renforcer la sécurité.

### Cursor Origin

Cursor Origin est une forge Git proposée par Anysphere, l'équipe derrière l'éditeur de code IA Cursor. À la différence des plateformes plus anciennes, elle est pensée dès l'origine pour le développement assisté par IA : elle héberge vos dépôts comme toute forge Git, mais traite les commits et pull requests générés par des agents comme des éléments de première classe. Vous pouvez aussi mirrorer vos dépôts GitHub existants vers Origin, ce qui permet de l'adopter sans bouleverser votre workflow actuel.

Fonctionnalités principales :

- **Conception "AI-agent-first" :** Les agents de codage de Cursor peuvent pousser des commits, ouvrir des pull requests et agir directement sur les dépôts, idéal pour le développement piloté par agents.
- **Mirroring GitHub :** Miroir de vos dépôts GitHub existants vers Origin pour une adoption progressive plutôt qu'une migration complète.
- **Intégration Cursor :** Intégration étroite avec l'éditeur Cursor et ses agents cloud pour réunir hébergement, édition et exécution d'agents au sein d'un même écosystème.
- **Workflows Git standard :** Dépôts, branches et pull requests fonctionnent comme prévu, avec une mise en route pilotée en CLI.
- **Revue de pull requests :** Relecture intégrée pour valider les PR générées par IA avant leur fusion dans la branche principale.

Pour un pas-à-pas via la CLI et le mirroring d'un dépôt GitHub, consultez notre tutoriel Cursor Origin.

## Conclusion

Dans cet article, nous avons passé en revue les bases de GitHub, son importance et son fonctionnement. Nous avons d'abord clarifié la notion de gestion de versions et son rôle pour maîtriser les évolutions du code et des données. Enfin, nous avons proposé un guide pratique de GitHub : inscription, création de dépôts, commits, gestion des pull requests et fusion des branches.

La clé de la réussite dans la data, c'est l'apprentissage continu. Pour aller plus loin sur GitHub et ses applications en data science, écoutez notre épisode du podcast DataFramed sur The Future of Programming with Kyle Daigle, qui livre la vision du COO de GitHub sur l'évolution du développement. Vous pouvez aussi explorer GitHub Concepts, un cours complet sur l'écosystème GitHub, et consulter notre guide des certifications GitHub pour comprendre comment elles peuvent booster votre carrière.

Professionnel expérimenté des données et écrivain passionné par l'autonomisation des experts en herbe dans le domaine des données.

## Foire aux questions

### GitHub est-il difficile à apprendre ?

**Si Git, la technologie sous-jacente, demande un temps d'apprentissage, GitHub propose une interface conviviale pour gérer les fonctionnalités de Git.**

### Quelle est la différence entre Git et GitHub ?

**Git est un système de gestion de versions qui suit les changements de fichiers, tandis que GitHub est une plateforme qui héberge des dépôts Git en ligne.**

### GitHub est-il sûr pour stocker des données sensibles ?

**Les dépôts publics sur GitHub sont visibles de tous. Évitez d'y stocker des données sensibles comme des mots de passe ou des clés d'API. Utilisez plutôt des dépôts privés.**

### Dois-je payer pour utiliser GitHub ?

**GitHub propose une offre gratuite avec des dépôts publics illimités et des dépôts privés (stockage limité). Les formules payantes offrent des fonctionnalités additionnelles comme plus de stockage et des options de collaboration avancées.**

### Quelles sont les alternatives à GitHub ?

**GitLab, Bitbucket, SourceForge, AWS CodeCommit**

### Comment démontrer mes connaissances de GitHub en entretien ?

Pour valoriser vos compétences GitHub en entretien, envisagez d'obtenir une certification GitHub qui valide vos acquis, renforce votre CV, accroît votre confiance et démontre votre engagement dans l'apprentissage.