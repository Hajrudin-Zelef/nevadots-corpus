Cours

Git est un outil incontournable dans la boîte à outils des développeurs modernes, reconnu pour ses puissantes capacités de gestion de versions. Créé par Linus Torvalds en 2005 pour soutenir le développement du noyau Linux, Git est depuis devenu l’ossature d'innombrables projets logiciels à travers le monde. Son efficacité et sa flexibilité dans la gestion des versions, associées à un solide support de la collaboration, en font un indispensable pour des équipes de toutes tailles.

Cet article vise à vous préparer aux entretiens techniques en couvrant les 20 principales questions d'entretien Git, du niveau débutant au niveau avancé. Que vous découvriez Git ou que vous cherchiez à approfondir vos connaissances, ces questions-réponses vous aideront à démontrer votre maîtrise et à réussir votre entretien.

## Devenez ingénieur en données

## Questions Git pour débuter

Si vous débutez avec Git, certaines questions d'entretien porteront probablement sur des notions de base et des cas d'usage simples. Pour réviser ces fondamentaux, consultez le cours Introduction to Git de DataCamp.

### Qu’est-ce qu’un dépôt Git ?

Un dépôt Git stocke les fichiers d’un projet et l’historique de leurs révisions, et facilite le contrôle de version en suivant les modifications dans le temps. Il peut être hébergé localement dans un dossier sur votre machine ou en ligne sur une plateforme comme GitHub. Cela permet de collaborer, de revenir à des versions antérieures et de gérer efficacement le développement via des commandes comme commit, push et pull.

### Comment fonctionne Git ?

Git enregistre les changements apportés aux fichiers et répertoires d’un projet sous forme d’instantanés successifs. Vous pouvez suivre les modifications, créer des branches pour développer en parallèle, fusionner des branches et revenir à des états antérieurs si nécessaire. Il favorise la collaboration et assure un contrôle de version efficace dans les projets logiciels.

### Qu’est-ce que git add ?

La commande `git add` sert à mettre en scène (stager) des modifications en vue du prochain commit. Elle prépare les ajouts, suppressions ou modifications effectués dans l’espace de travail afin qu’ils soient inclus dans le prochain instantané. Notez qu’elle ne valide pas les changements : elle se contente de les placer dans l’index.

### Qu’est-ce que git push ?

La commande `git push` permet d’envoyer le contenu du dépôt local vers un dépôt distant. Elle transfère les commits du dépôt local vers un serveur distant, typiquement GitHub ou GitLab. Cette commande favorise la collaboration en partageant vos changements avec les autres membres du projet.

Pour en savoir plus sur git push et git pull, consultez notre tutoriel dédié.

### Qu’est-ce que git status ?

La commande `git status` affiche l’état courant du dépôt. Elle indique quels fichiers ont été modifiés, lesquels sont prêts à être commités et lesquels ne sont pas suivis. Elle aide à suivre l’avancement du travail et à identifier ce qui doit être ajouté à l’index ou validé.

### Qu’est-ce qu’un commit dans Git ?

Un commit représente un instantané des changements apportés aux fichiers d’un dépôt à un moment donné. Lorsque vous validez vos modifications, vous enregistrez l’état actuel de vos fichiers et vous pouvez ajouter un message descriptif expliquant les changements (fortement recommandé).

Chaque commit possède un identifiant unique, ce qui permet de retracer l’historique du dépôt. Les commits sont essentiels au contrôle de version : ils permettent de revenir en arrière, de relire l’historique des modifications et de collaborer en partageant des mises à jour.


*Consultez l’aide-mémoire Git de DataCamp pour préparer votre entretien*

### Qu’est-ce que le branchement dans Git ?

Le branchement consiste à diverger de la ligne principale de développement (généralement appelée `main`, anciennement `master`) afin de travailler sur de nouvelles fonctionnalités, des correctifs ou des expérimentations sans impacter la base de code principale. Cela permet de faire coexister plusieurs lignes de développement en parallèle au sein d’un même dépôt.

Chaque branche suit sa propre lignée de commits, ce qui permet à plusieurs développeurs de travailler simultanément sur des sujets distincts. Le branchement facilite la collaboration, l’expérimentation et l’organisation : une fois les changements terminés et testés, ils peuvent être fusionnés dans la branche principale.

### Qu’est-ce qu’un conflit dans Git ?

Les conflits surviennent lorsque des modifications incompatibles sont apportées à la même portion d’un fichier par des contributeurs différents, généralement lors d’une fusion (merge) ou d’un rebase. Git ne peut pas les résoudre automatiquement et requiert une intervention manuelle.

Pour résoudre un conflit, ouvrez le fichier concerné : Git marque les sections en conflit avec les indicateurs `<<<<<<<`, `=======` et `>>>>>>>`. Éditez le fichier pour conserver la bonne version, supprimez les marqueurs, puis :

```
git add <resolved-file>
git commit
```
`git mergetool` rendent ce processus plus visuel et facile à suivre.
### Qu’est-ce que la fusion (merge) dans Git ?

La fusion est une opération fondamentale qui facilite la collaboration et l’intégration des changements entre différentes branches d’un projet. Elle consiste à combiner les modifications de plusieurs branches dans une seule, généralement la branche principale (par ex. master ou main).

Une fusion intègre les changements d’une branche dans une autre et crée un nouveau commit qui rassemble les historiques des deux branches. Pour en savoir plus sur la résolution des conflits de fusion, consultez notre tutoriel dédié.

## Obtenez une certification pour le poste de Data Engineer de vos rêves

Nos programmes de certification vous aident à vous démarquer et à prouver aux employeurs potentiels que vos compétences sont adaptées à l'emploi.

## Questions Git intermédiaires

### Qu’est-ce qu’un remote dans Git ?

Un remote est un dépôt hébergé sur un serveur ou un autre ordinateur pour collaborer et partager du code. Il sert de point central où les développeurs peuvent pousser leurs changements locaux et récupérer ceux des autres.

Les remotes sont généralement configurés sur des plateformes comme GitHub, GitLab ou Bitbucket. Ils permettent un développement distribué et facilitent le travail en équipe en offrant un emplacement commun pour stocker et synchroniser le code entre plusieurs contributeurs.

### Comment annuler un commit déjà poussé et rendu public ?

La commande `git revert <commit-hash>` permet d’annuler un commit déjà poussé et public.

Procédure pas à pas :

1. Identifiez le commit à annuler en trouvant son hash. Utilisez `git log` pour parcourir l’historique et récupérer le hash souhaité.

2. Une fois le hash obtenu, exécutez la commande git revert suivie de ce hash pour créer un nouveau commit qui annule les changements introduits par le commit ciblé. Par exemple :

`git revert <commit-hash>`
3. Git ouvre un éditeur pour saisir le message de revert. Modifiez-le si nécessaire, puis enregistrez et fermez.

4. Après l’enregistrement, Git crée un nouveau commit qui annule effectivement les changements introduits par le commit d’origine. Ce nouveau commit s’ajoute à l’historique.

5. Enfin, poussez le nouveau commit vers le dépôt distant pour rendre le revert public :

`git push origin <branch-name>` Avec `git revert`, vous créez un commit d’annulation sans réécrire l’historique. C’est plus sûr que `git reset` ou `git amend`, qui peuvent modifier l’historique et perturber des collaborateurs ayant déjà récupéré les changements.

### Qu’est-ce que git stash ?

`git stash` stocke temporairement des modifications locales qui ne sont pas prêtes à être commitées. Il permet d’enregistrer vos travaux en cours sans les valider dans le dépôt.

Le stash est utile lorsque vous changez de branche sans vouloir committer ni perdre vos changements. Plus tard, vous pourrez appliquer le stash à votre espace de travail ou le dépiler pour reprendre là où vous vous êtes arrêté.

### Qu’est-ce que git reflog ?

git reflog affiche les journaux de références, qui enregistrent les changements du pointeur HEAD et l’historique des commits consultés dans le dépôt. Il fournit une liste chronologique des actions récentes (commits, checkouts, merges, resets).

Le reflog est précieux pour retrouver des commits ou des branches perdus et comprendre la séquence des actions menées dans le dépôt.

### Comment faire suivre une branche locale par une branche distante existante ?

Pour faire suivre une branche locale par une branche distante, utilisez `git branch` avec l’option `--set-upstream-to` ou `-u`, suivie du nom de la branche distante.

La syntaxe est la suivante :

`git branch --set-upstream-to=<remote-name>/<branch-name>`
ou

`git branch -u <remote-name>/<branch-name>`
## Questions Git avancées

### Comment gérez-vous plusieurs configurations selon les projets dans Git ?

Utilisez `git config` avec les indicateurs `--global`, `--system` ou `--local` pour ajuster les réglages à différents niveaux. Vous pouvez aussi employer `includeIf` dans la configuration Git pour inclure des paramètres spécifiques en fonction du chemin du dépôt.

### Comment gérer les gros fichiers avec Git ?

Les gros fichiers peuvent alourdir le dépôt et dégrader les performances. Utilisez Git LFS pour stocker ces fichiers en dehors du dépôt Git tout en gardant des pointeurs légers dans l’historique. Cela réduit la taille du dépôt et améliore les performances. Git LFS prend en charge divers fournisseurs de stockage et s’intègre naturellement aux workflows Git.

### À quoi sert git submodule et comment en mettre un à jour ?

La commande `git submodule` permet de gérer des dépendances externes au sein d’un dépôt Git. Elle vous autorise à inclure des dépôts externes comme sous-modules dans votre dépôt principal, pratique pour intégrer du code tiers tout en le gardant séparé de votre base de code.

Pour mettre à jour un sous-module :

1. 
Placez-vous dans le répertoire du sous-module dans le dépôt principal.
2. 
Utilisez `git fetch` pour récupérer les derniers changements du dépôt distant du sous-module.
3. 
Pour avancer vers le dernier commit de la branche suivie par le sous-module, utilisez `git pull` .
4. 
Sinon, pour viser un commit ou une branche spécifique, faites `git checkout` avec le hash ou le nom de branche voulu.
5. 
Une fois à l’état désiré, validez dans le dépôt principal pour enregistrer la nouvelle révision du sous-module.

### Qu’est-ce que git cherry-pick et quand l’utiliser ?

`git cherry-pick` applique un commit précis d’une branche sur une autre, sans fusionner l’ensemble de la branche.

`git cherry-pick <commit-hash>``main` mais vous avez aussi besoin du correctif sur une branche `release` : vous pouvez ne récupérer que ce commit plutôt que de fusionner toute la branche `main` dans `release`.
Utile aussi lorsqu’un commit a été fait par erreur sur la mauvaise branche : cherry-pickez-le sur la bonne, puis revertissez-le de celle où il n’a rien à faire.

### Qu’est-ce que git bisect et à quoi sert-il ?

`git bisect` est un outil de débogage qui utilise la recherche binaire pour trouver le commit qui a introduit un bug. Plutôt que de tester des commits un à un, vous indiquez à Git un commit "bon" (sans bug) et un commit "mauvais" (avec bug) ; Git va alors checkout des commits intermédiaires, divisant l’espace de recherche par deux jusqu’à trouver le responsable.

```
git bisect start
git bisect bad                # le commit courant contient le bug
git bisect good <commit-hash> # cet ancien commit était sain
# Git extrait un commit intermédiaire ; vous testez, puis :
git bisect good   # ou git bisect bad
# répétez jusqu'à identification du premier mauvais commit
git bisect reset  # retour à l'état initial
```
C’est bien plus rapide que des tests manuels dans un grand dépôt.

### Que sont les hooks Git et comment les utiliser ?

Les hooks Git sont des scripts exécutés automatiquement à des moments clés du workflow Git. Ils résident dans le répertoire `.git/hooks/` d’un dépôt et peuvent être écrits dans n’importe quel langage de script.

On distingue deux types :

- 
**Côté client** : exécutés en local — par exemple`pre-commit` (avant la création d’un commit) ou`commit-msg` (validation du format du message de commit).
- 
**Côté serveur** : exécutés sur le dépôt distant — par exemple`pre-receive` (avant d’accepter des commits poussés).

Un usage courant est un hook `pre-commit` qui lance automatiquement un linter ou une suite de tests avant d’autoriser un commit, afin d’imposer des standards de qualité.

Notez que les hooks ne sont pas copiés lors d’un clonage ; les équipes les partagent donc via un script dédié ou un outil comme `pre-commit` (le paquet Python).

## Questions sur des concepts Git souvent confondus

### Quelle est la différence entre git fetch et git pull ?

La principale différence entre git fetch et git pull tient à leur effet sur le dépôt local.

`git fetch` récupère les changements d’un dépôt distant et met à jour les branches de suivi à distance (par ex. origin/master) sans modifier votre répertoire de travail ni fusionner quoi que ce soit dans la branche courante. Vous pouvez ainsi examiner les nouveautés sans impacter votre travail.

`git pull` récupère aussi les changements, mais va plus loin : il enchaîne un fetch puis un merge dans votre branche courante, intégrant directement les mises à jour distantes.

### À quoi sert git reset ?

La commande `git reset` repositionne HEAD sur un état donné. Elle permet d’annuler des changements, de retirer des fichiers de l’index ou de déplacer HEAD vers un autre commit. Trois modes principaux existent :

- `--soft` : déplace HEAD vers un commit spécifique en conservant les changements dans l’index. Les fichiers restent modifiés et prêts à être re-commités.

- `--mixed` : déplace HEAD et retire les changements de l’index. Les fichiers restent modifiés dans l’espace de travail, mais ne sont plus en scène.

- `--hard` : déplace HEAD et supprime toutes les modifications dans l’espace de travail et l’index. À utiliser avec précaution : les changements non commités sont définitivement perdus.

**Important :** n’utilisez jamais `git reset --hard` sur des commits déjà poussés sur une branche partagée. Cela réécrit l’historique et posera de sérieux problèmes à vos collègues. Préférez `git revert` pour des commits publics.

### Pourquoi privilégier git push --force-with-lease à git push --force ?

`git push --force-with-lease` est une façon plus prudente de forcer un push que `git push --force` car elle évite d’écraser par inadvertance le travail d’autrui sur le dépôt distant.

Avec `git push --force`, vous forcez la mise à jour sans vérifier si la branche distante a été modifiée depuis votre dernier fetch, ce qui peut effacer le travail d’autres développeurs.

À l’inverse, `git push --force-with-lease` vérifie que la branche distante n’a pas évolué depuis votre dernière récupération. Si c’est le cas, le push est refusé, empêchant l’écrasement involontaire des changements des autres.

### Qu’est-ce que git rebase et en quoi diffère-t-il de git merge ?

git rebase et `git merge` intègrent des changements d’une branche dans une autre, mais de manière différente.

- 
`git merge` combine les historiques de deux branches en créant un nouveau « merge commit ». Cela préserve l’historique complet des divergences et des réunions, utile pour l’audit et la transparence d’équipe.
- 
`git rebase` « rejoue » les commits d’une branche au-dessus d’une autre pour obtenir un historique linéaire, sans commits de fusion. Le journal est plus lisible, mais l’historique est réécrit. Règle d’or :*ne rebasez jamais une branche sur laquelle d’autres travaillent* .

### Quelle est la différence entre git clone et git fork ?

**Cloner** crée une copie locale d’un dépôt distant sur votre machine. Vous restez connecté au dépôt d’origine et pouvez y pousser des changements (avec les droits nécessaires).

`git clone https://github.com/user/repo.git`
**Forker**crée une copie côté serveur du dépôt de quelqu’un d’autre sous votre propre compte — généralement sur GitHub ou GitLab. Vous possédez ce fork et pouvez y pousser librement. Lorsque vos changements sont prêts, vous ouvrez une pull request vers le dépôt d’origine.

Le fork est le flux de travail standard pour contribuer à des projets open source lorsque vous n’avez pas d’accès en écriture direct au dépôt initial.

## Bien se préparer à un entretien sur Git

Mettre en avant vos connaissances et votre expérience Git en entretien est crucial pour démontrer votre maîtrise des workflows de collaboration et d’outillage de développement.

Voici quelques conseils pour préparer votre entretien technique et présenter efficacement vos compétences Git :

### Maîtriser les fondamentaux de Git

Assurez-vous de bien comprendre les fondamentaux : dépôts, branches, fusions, commits et les commandes de base comme `pull`, `push`, `clone` et `commit`. Cette base structurera vos échanges en entretien. Il est aussi utile de bien saisir les principes clés du contrôle de version et les différences entre Git et d’autres systèmes.

Enfin, familiarisez-vous avec des méthodologies Git comme Git Flow, GitHub Flow et GitLab Flow. Évaluez leurs avantages et limites, et sachez quand les appliquer.

Notre guide complet sur Git est un bon point de départ pour réviser les fondamentaux.

### Pratiquer sur des cas concrets

Plus vous utilisez Git, plus vous ancrez vos connaissances. La pratique régulière vous rend à l’aise avec les commandes et les habitudes de travail. Intégrez Git à votre quotidien, expérimentez la création et la fusion de branches, et entraînez-vous à résoudre des conflits.

Si vous manquez d’idées de projets, contribuer à l’open source sur GitHub est une excellente façon de vous exposer à des outils et workflows de collaboration utilisés dans l’industrie.

### Connaître les problèmes courants et leur dépannage

Vous rencontrerez forcément des problèmes avec Git : conflits de fusion, état de HEAD détaché, revert de changements, récupération de commits perdus, etc. Diagnostiquer ces situations renforce vos compétences de dépannage et votre compréhension des mécanismes internes de Git.

En analysant activement les messages d’erreur et en pratiquant, vous gagnerez en efficacité pour identifier et résoudre les incidents, réduirez les risques et prendrez confiance dans la gestion des workflows de version.

### S’entraîner avec des entretiens blancs

Les entretiens blancs vous aident à cibler vos axes d’amélioration, tant sur les connaissances Git que sur la communication.

Ils offrent aussi l’occasion de vous exercer sur des scénarios réalistes liés à Git et des exercices de code. Cette pratique renforce la confiance et améliore votre capacité à expliquer clairement votre raisonnement le jour J.

## Conclusion

Git est un système de gestion de versions puissant, largement utilisé pour gérer les changements de code, collaborer et conserver l’historique des projets. Maîtriser Git est indispensable en entretien technique : cela prouve votre aisance avec des outils et workflows essentiels, votre capacité à collaborer et à gérer le code efficacement en équipe.

Comprendre les concepts et commandes Git permet d’adopter des pratiques de versionnage efficaces, garantes de l’intégrité du code, de la continuité des projets et de processus de développement fluides. Ces compétences sont précieuses pour les ingénieurs et développeurs en quête d’entretiens réussis et d’une carrière épanouissante.

Pour aller plus loin, consultez :