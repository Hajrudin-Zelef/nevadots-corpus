*Un grand merci à Brian Granger, Fernando Pérez et Robert Kern pour leurs contributions à cet article !*

Pour les apprenants comme pour les data scientists confirmés, Jupyter Notebook compte parmi les outils phares de la data science : son environnement interactif est idéal pour enseigner, apprendre, partager ses travaux avec ses pairs et garantir la reproductibilité des recherches. Pourtant, en découvrant le notebook, vous croiserez très souvent IPython.

Dans certains cas, les deux semblent synonymes et vous conviendrez que cela devient vite déroutant dès que l’on veut creuser : les « magics » relèvent-ils de Jupyter ou d’IPython ? L’enregistrement et le chargement des notebooks sont-ils des fonctionnalités d’IPython ou de Jupyter ?

La liste des questions peut être longue.

L’objectif de cet article est d’exposer clairement quelques différences fondamentales entre les deux, en repartant de leurs origines pour expliquer leurs liens, puis en passant en revue des fonctionnalités propres à l’un ou l’autre, afin de vous aider à mieux faire la distinction.

Pensez aussi à consulter le guide ultime de Jupyter Notebook de DataCamp pour des astuces, bonnes pratiques, exemples et bien plus encore.

## Les origines d’IPython / Jupyter

Pour bien comprendre ce qu’est Jupyter Notebook et en quoi il se distingue d’IPython, il est utile de revenir sur la place de ces deux projets dans l’histoire (et l’avenir) des notebooks computationnels.

### Les débuts des notebooks computationnels : MATLAB, Mathematica et Maple

Au milieu des années 1980, MATLAB est lancé par The MathWorks, fondé par Jack Little, Steve Bangert et Cleve Moler.

Fin des années 1980, 1987 pour être précis, Theodore Gray commence à travailler sur ce qui deviendra l’interface notebook de Mathematica, publiée un an plus tard. Cette interface graphique permet de créer et modifier de manière interactive des documents notebook contenant du code joliment mis en forme, du texte, et de nombreuses fonctionnalités comme des formules composées, des graphiques, des composants GUI, des tableaux et des sons. On y trouve les fonctions classiques d’un traitement de texte, dont la correction orthographique multilingue en temps réel. Les documents peuvent être diffusés en mode diaporama pour des présentations.

La structure de ces notebooks reposait sur une hiérarchie de cellules facilitant plan et sections des documents — un principe que l’on retrouve aujourd’hui dans les notebooks Jupyter.

En 1989, Maple introduit également sa première interface de type notebook, incluse avec la version 4.3 pour Macintosh. Des versions pour X11 et Windows suivent en 1990. Ces notebooks précurseurs ont inspiré et posé les bases de ce que l’on appellera plus tard les « notebooks de data science ».

### L’essor des notebooks de data science

De nombreux notebooks computationnels ont vu le jour après Maple et Mathematica. Cette section se concentre toutefois sur ceux qui ont contribué à l’essor des notebooks de data science, dont certains restent très populaires auprès des data scientists, débutants comme confirmés.

#### Sage Notebook

Le notebook Sage, en tant que système accessible via navigateur, paraît au milieu des années 2000. En 2007, une nouvelle version plus puissante voit le jour, avec gestion des comptes utilisateurs et possibilité de publier des documents. Son interface rappelle celle de Google Docs, car la mise en page du notebook Sage s’inspire de Google Notebooks.

Les créateurs de Sage ont confirmé avoir été des utilisateurs assidus des notebooks Mathematica et des feuilles Maple. D’autres facteurs ont compté dans le développement de Sage : d’une part, des échanges étroits avec l’équipe d’IPython, Sage en mode terminal s’appuyant sur IPython ; d’autre part, « une tentative avortée » de deux étudiants pour proposer une interface graphique à IPython ; enfin, la montée des applications web « AJAX » (Asynchronous JavaScript And XML), qui évitent de recharger toute la page à chaque action.

Par exemple, lorsque vous soumettez un formulaire sur un site : sans AJAX, vous êtes redirigé vers une nouvelle page renvoyée par le serveur. Avec AJAX, JavaScript envoie la requête, récupère la réponse et met à jour l’écran, sans rechargement ni redirection.

Parmi les applications AJAX connues : Gmail, Google Maps, Facebook, Twitter, etc. Presque tout repose désormais sur AJAX.

#### IPython et Jupyter Notebook

Fin 2001, environ vingt ans après que Guido van Rossum a commencé à travailler sur Python au CWI (Pays-Bas), Fernando Pérez démarre le développement d’IPython. Comme Sage et bien d’autres projets, IPython est fortement influencé par les notebooks Mathematica et les feuilles Maple.

En 2005, une première tentative de notebook voit le jour avec Wx, une boîte à outils de widgets pour créer des interfaces graphiques multiplateformes. Deux étudiants du Google Summer of Code travaillent sur un prototype sous la direction de Robert Kern et Fernando Pérez. Robert pousse ensuite plus loin le chantier. Il s’agit moins d’un prototype de notebook que d’un nettoyage du cœur d’IPython pour faciliter l’écriture d’un frontal wxPython au shell IPython. Ces refontes ont contribué à rendre possible un notebook propre, et certaines ont été intégrées progressivement à IPython lorsque l’effort, cette fois réussi, s’est concrétisé.

Le deuxième prototype d’un notebook IPython est réalisé à l’été 2006 par Min Ragan-Kelley, conseillé par Brian Granger. Basé sur le web avec un backend SQL, il est finalement abandonné car trop complexe pour les technologies web de l’époque.

Le troisième prototype arrive en octobre 2010, développé par un tiers lors d’un hackathon de quelques jours. Enfin, au printemps-été 2011, Brian Granger travaille à temps plein sur un prototype de notebook web, s’appuyant sur ses travaux de 2010, lorsqu’il a créé avec Fernando Pérez l’architecture du kernel IPython et la spécification des messages, ainsi que PyZMQ avec Min Ragan Kelley.

PyZMQ est une bibliothèque qui fournit des bindings Python pour ZeroMQ : nécessaire pour les fonctionnalités de calcul parallèle, la console Qt et le notebook d’IPython.

PyZMQ et les websockets sont les technologies clés qui rendent le notebook possible. À l’automne, d’autres contributeurs (dont Matthias Bussonnier, Min et Fernando) rejoignent l’effort. Le 21 décembre 2011 sort la première version d’IPython Notebook (0.12).

Les années suivantes, l’équipe reçoit des prix (par exemple, l’Advancement of Free Software pour Fernando Pérez le 23 mars 2013, le Jolt Productivity Award) et des financements (Fondation Alfred P. Sloan, entre autres).

En 2014, naît finalement Project Jupyter, issu d’IPython.

La dernière version d’IPython avant la scission regroupait dans un seul dépôt le shell interactif, le serveur de notebook, la console Qt, etc. Le projet devenait vaste, avec des composants de plus en plus distincts.

Mais la taille n’est pas la seule raison de la création de Jupyter : de 2011 à 2014, l’IPython Notebook commence à fonctionner avec d’autres langages. Julia arrive en premier, puis R. Le fait que l’« IPython » Notebook — suggérant un notebook exclusivement Python — fonctionne aussi avec des kernels pour Julia et R déroute.

Pourquoi garder « IPython » si d’autres langages sont pris en charge ?

Le nom « Jupyter » s’inspire des principaux langages ouverts pour la science (Julia, Python et R). Même s’il peut sembler acronyme, il n’a jamais signifié que les autres langages n’étaient pas les bienvenus. Surtout, il représentait mieux le projet et faisait clin d’œil à ses racines scientifiques.

Après la création de Jupyter, les éléments agnostiques au langage d’IPython — format de notebook, protocole de messages, console Qt, application web du notebook, etc. — sont transférés dans le projet Jupyter. L’organisation GitHub principale de Jupyter se trouve ici.

Dans les communautés Jupyter et IPython, on parle de « The Big Split ».

IPython n’a plus que deux rôles : servir de backend Python au notebook Jupyter (le kernel) et fournir un shell Python interactif. Mais ce n’est pas tout : l’écosystème IPython inclut aussi un framework de calcul parallèle, dont nous parlerons plus loin.

Comme IPython, Project Jupyter est un nom-parapluie pour plusieurs projets : les trois applications principales sont le Notebook, une Console et une console Qt, mais on trouve aussi des sous-projets comme JupyterHub pour le déploiement des notebooks, nbgrader pour l’enseignement, etc. Un aperçu de l’architecture Jupyter est disponible ici.

Cette évolution explique la confusion de nombreux pythonistes face à IPython et Jupyter : l’un est issu de l’autre, et récemment. Certains peinent encore à employer les bons termes. Plus déroutant encore, l’évolution elle-même : l’héritage commun entraîne un chevauchement notable des fonctionnalités d’IPython et de Jupyter Notebook, parfois difficile à démêler. Les sections suivantes éclaireront ces distinctions. Pour en savoir plus sur l’histoire d’IPython, lisez les témoignages de Fernando Pérez et de William Stein sur leurs notebooks.

#### Notebooks R

R Markdown et Jupyter Notebook partagent l’objectif d’un flux de travail reproductible, en tissant code, résultats et texte dans un même document, avec support de widgets interactifs et export multi-formats.

Mais ils diffèrent aussi : R Markdown privilégie l’exécution par lot reproductible, la représentation en texte brut, le contrôle de version, des sorties de production et l’usage du même éditeur/outillage que pour les scripts R. Les notebooks, eux, affichent le résultat au fil du code, mettent en cache les sorties entre sessions, et facilitent le partage du code et des résultats dans un fichier unique. Ils mettent l’accent sur l’exécution interactive. Ils n’utilisent pas une représentation en texte brut, mais un format de données structuré, comme JSON.

C’est ce qui motive l’application « notebook » de RStudio : elle marie les atouts de R Markdown et ceux des notebooks computationnels.

Pour apprendre à travailler avec les notebooks R et comprendre précisément les différences entre Jupyter et les notebooks R Markdown en matière de partage, gestion de projet, contrôle de version, etc., lisez l’article de DataCamp Jupyter et R : notebooks avec R.

#### Autres notebooks de data science

D’autres notebooks méritent votre attention en data science. Ces dernières années, de nombreuses alternatives ont émergé : Beaker Notebook, Apache Zeppelin, Spark Notebook, DataBricks Cloud, etc., mais aussi des outils comme l’IDE Rodeo ou nteract, qui rendent vos analyses interactives et reproductibles. Notez que nteract se distingue en s’appuyant sur l’architecture Jupyter (protocoles et formats).

### L’avenir des notebooks

Les notebooks sont là pour durer. La nouvelle génération de Jupyter Notebook a récemment été introduite : JupyterLab. L’application Notebook intègre non seulement le support des notebooks, mais aussi un gestionnaire de fichiers, un éditeur de texte, un terminal, un moniteur des processus Jupyter, un gestionnaire de clusters IPython et un pager d’aide.

Rien de neuf ? En réalité, JupyterLab permet d’exploiter tous ces blocs de construction du calcul interactif de manières inédites.

À lire ici.

L’arsenal d’outils de Jupyter Notebook a grandi organiquement, guidé par les besoins des utilisateurs et des développeurs. JupyterLab apporte une architecture de nouvelle génération pour les réunir, avec une interface flexible et réactive, offrant une mise en page pilotée par l’utilisateur.

## IPython ou Jupyter ?

L’évolution du projet et la « Big Split » qui s’ensuit posent les bases pour comprendre les vraies différences entre les deux. Mais comme ils sont intimement liés, le doute persiste parfois sur ce qui appartient à quoi.

La section suivante passe en revue des fonctionnalités relevant soit de l’écosystème IPython, soit du projet Jupyter.

À vous d’identifier la bonne réponse et d’en apprendre plus sur chaque fonction !

### Kernels ?

Même si les kernels apparaissent en bonne place dans l’application Jupyter Notebook, le premier outil à utiliser un kernel et un protocole complets fut la console Qt, antérieure au Notebook. Aujourd’hui, les kernels sont exploités de façon souple par le Notebook, historiquement comme architecturalement, et relèvent de Jupyter et non du seul Notebook. Autrement dit, les kernels sont bien plus qu’une fonctionnalité : ils constituent une abstraction centrale de l’architecture Jupyter, utilisée par des outils hors-notebook comme la console texte, la console Qt, Thebe d’O’Reilly, Kernel Gateway, l’éditeur Hydrogen de nteract. Enfin, dans JupyterLab, un kernel peut aussi se connecter à tout, d’un notebook à une console web, voire un simple fichier texte.

Un kernel est un programme qui exécute et introspecte le code de l’utilisateur : il assure le calcul et la communication avec les interfaces front-end comme les notebooks. L’application Jupyter Notebook propose trois kernels principaux : IPython, IRkernel et IJulia.

Étant donné que « Jupyter » s’inspire de Julia, Python et R, rien d’étonnant. Le kernel IPython est maintenu par l’équipe Jupyter, conséquence logique de l’évolution du projet.

Vous pouvez cependant faire tourner bien d’autres langages dans l’application Jupyter Notebook : Scala, JavaScript, Haskell, Ruby, etc. Il s’agit de kernels maintenus par la communauté.

### Déploiement de notebooks ?

Le déploiement des notebooks est typiquement un sujet que vous rencontrerez avec Jupyter. Plusieurs packages de l’écosystème Jupyter vous y aident.

En voici quelques-uns :

- `docker-stacks` : des piles d’applications Jupyter et de kernels sous forme de conteneurs Docker.
- `ipywidgets` : des widgets HTML & JavaScript interactifs (curseurs, cases à cocher, champs texte, graphiques…) pour l’architecture Jupyter, reliant contrôles front-end et kernel.
- `jupyter-drive` : permet à IPython d’utiliser Google Drive pour la gestion des fichiers.
- `jupyter-sphinx-theme` : ajoute un thème Sphinx Jupyter à vos notebooks pour créer une documentation soignée et intelligente.
- `kernel_gateway` : un serveur web qui prend en charge différents mécanismes de lancement et de communication avec des kernels Jupyter. Des cas d’usage ici.
- `nbviewer` : pour partager vos notebooks. Galerie ici.
- `tmpnb` : crée des serveurs Jupyter Notebook temporaires via Docker. À essayer ici.
- `traitlets` : un framework qui dote les classes Python d’attributs avec vérification de type, valeurs par défaut dynamiques et callbacks « on change ». Utile aussi pour la configuration (fichiers, arguments de ligne de commande). traitlets alimente le système de configuration d’IPython et de Jupyter ainsi que l’API déclarative des widgets interactifs IPython.

### Utilisation du shell système ?

IPython peut être adapté à un usage de type shell système grâce à l’« escape » shell : les lignes commençant par `!` sont passées directement au shell. Par exemple, !ls exécutera `ls` dans le répertoire courant. Vous pouvez affecter le résultat d’une commande système à une variable Python avec la syntaxe `myfiles=!ls`. Pour afficher explicitement le résultat de ls sous forme de liste de chaînes sans l’assigner, utilisez deux points d’exclamation (`!!ls`) ou la commande magique `%sx` sans assignation.

```
# Assign the result to `ls`
ls = !ls
 
# Explicit `ls`
!!ls
 
# Or with magics
%sx
 
# Assign magics result
ls = %sx
```
Notez que les commandes `!!` ne peuvent pas être assignées à une variable, alors que le résultat d’une magic (si elle renvoie une valeur) peut l’être.

IPython permet aussi d’étendre la valeur des variables Python dans les appels système : il suffit d’englober variables ou expressions entre accolades (`{}`). Dans une commande avec `!` ou `!!`, toute variable Python préfixée par `$` est développée. Dans l’extrait ci-dessous, vous affichez l’attribut argv de la variable sys. Vous pouvez également utiliser les syntaxes `$`/`$$` pour référencer des variables Python depuis la sortie du système, puis vous en servir dans vos scripts.

Pour transmettre un `$` littéral au shell, utilisez un double `$$`. Vous en aurez besoin pour accéder aux variables d’environnement comme $PATH :

```
# Import and initialize
import math
x = 4
 
# System call with variable
!echo {math.factorial(x)}
 
# Expand a variable
!echo $sys.argv
 
# Use $$ for a literal $
!echo "A system variable: $$HOME"
```
En savoir plus ici.

Outre IPython, d’autres kernels proposent aussi des syntaxes spéciales pour exécuter des lignes au shell ! Ce ne sont pas forcément des « magics » au sens strict, car le nom et la richesse peuvent varier selon l’implémentation.

Vous pouvez également définir des **alias** pour des commandes système : ce sont des raccourcis vers des commandes bash. Un alias est un tuple : (« showTheDirectory », « ls »). Lancez `%alias?` pour plus d’infos !

**Astuce** : utilisez `%rehashx` pour charger tout votre $PATH comme alias IPython.

### Magics ?

Si vous avez parcouru le guide ultime de Jupyter Notebook de DataCamp ou si vous avez déjà utilisé Jupyter, vous connaissez sans doute les « magic commands ». Les magics reposent sur un élément de syntaxe invalide dans le langage sous-jacent et un mot qui évoque une commande. Sous le capot, ce sont des fonctions Python.

Le kernel IPython utilise le symbole % (qui n’est pas unitaire valide en Python). Les lignes commençant par %% marquent une « cell magic » : elles prennent en arguments non seulement le reste de la ligne, mais aussi toutes les lignes suivantes du bloc courant. Les cell magics peuvent modifier librement l’entrée reçue, qui n’a même pas besoin d’être du Python valide. Elles reçoivent le bloc entier comme une unique chaîne.

Les magics sont spécifiques aux kernels qui les fournissent et sont conçues pour rendre votre travail dans Jupyter Notebook plus interactif. Leur disponibilité dépend des développeurs du kernel et varie d’un kernel à l’autre. Vous l’aurez compris : les magics sont une fonctionnalité de kernel.

Quand vous utilisez le backend Python de Jupyter, IPython (le kernel), voici quelques astuces pour accéder à des fonctionnalités qui accélèrent, simplifient et enrichissent votre programmation. La liste n’est pas exhaustive. Consultez la liste complète des magics intégrées.

#### Visualisation

Une fonctionnalité majeure du kernel IPython est l’affichage des graphiques produits par les cellules de code. Il est conçu pour fonctionner de manière fluide avec la bibliothèque matplotlib. Pour l’activer, utilisez la magic `%matplotlib`.

Par défaut, le graphique s’ouvre dans une fenêtre séparée. Vous pouvez aussi préciser un backend (inline, qt, etc.) pour afficher les tracés en ligne ou via une autre interface GUI. Plus d’infos ici.

#### Navigation dans le système de fichiers

Les magics du kernel offrent aussi des moyens de naviguer dans votre système de fichiers. Les magics `%cd` et `%bookmark` permettent de changer de répertoire ou d’ajouter des favoris pour accéder plus vite aux dossiers fréquents.

#### Accès au débogueur

Vous pouvez invoquer un débogueur Python avec `%pdb` à chaque exception non interceptée. Il vous guide dans la portion de code à l’origine de l’erreur pour accélérer l’analyse.

La magic `%run` avec l’option -d exécute un script sous contrôle du débogueur Python et place automatiquement des points d’arrêt initiaux. La magic `%debug` offre un accès encore plus direct.

#### Extensions IPython

La magic `%load_ext` charge une extension IPython par son nom de module. Les extensions IPython sont des modules Python qui modifient le comportement du shell : elles peuvent enregistrer des magics, définir des variables et, plus généralement, enrichir l’espace utilisateur pour offrir de nouvelles fonctions dans les cellules. Exemples :

- `%load_ext oct2py.ipython` : appeler de façon transparente des M-files et fonctions Octave depuis Python,
- `%load_ext rpy2.ipython` : interface vers R embarqué dans un processus Python,
- `%load_ext Cython` : utiliser le compilateur Python→C,
- `sympy.init_printing()` : jolis rendus automatiques des objets Sympy Basic,
- `%load_ext fortranmagic` : utiliser Fortran dans vos sessions interactives.

… Et bien d’autres ! Vous pouvez créer et publier vos propres extensions IPython sur PyPI : il existe donc beaucoup d’extensions et de magics communautaires. Exemple : `ipython_unittest`, et consultez aussi cet index des extensions.

Gardez aussi un œil sur sparkmagic, un ensemble d’outils pour travailler de façon interactive avec des clusters Spark distants via Livy (serveur REST Spark) dans des notebooks Jupyter. La bibliothèque `sparkmagic` fournit une magic `%%spark` pour exécuter facilement du code contre un cluster Spark distant depuis un notebook IPython classique.

```
# Load in sparkmagic
%load_ext sparkmagic.magics
 
# Set the endpoint
%manage_spark
 
# Ask for help
%spark?
```
Rendez-vous ici pour plus d’exemples d’utilisation de ces magics avec un cluster Spark.

Notez qu’IPython propose deux autres magics, `%reload_ext` et `%unload_ext`, pour recharger et décharger des extensions directement depuis Jupyter Notebook.

#### Différents kernels, autres magics

Dans d’autres langages, l’élément de syntaxe utilisé par les magics peut déjà avoir un sens. Le kernel R, IRkernel, n’a pas de système de magics. Pour exécuter des commandes bash, vous utiliserez par exemple des fonctions R comme `system()` pour invoquer des commandes OS, par exemple `system("head -5 *.csv", intern=TRUE)`. En incluant l’argument `intern`, vous indiquez que vous souhaitez capturer la sortie de la commande en vecteur de caractères R. Pour afficher du Markdown, utilisez `display_markdown()`, à qui vous passez le code Markdown en vecteur de caractères. De même, le kernel Julia IJulia n’utilise pas les « magics ». D’autres syntaxes, plus naturelles en Julia, fonctionnent en dehors des cellules IJulia et sont souvent plus puissantes. Les développeurs d’IJulia ont toutefois prévu qu’en entrant une magic IPython dans une cellule IJulia, un message d’aide indique la manière d’obtenir un effet équivalent en Julia, lorsque c’est possible.

Par exemple, l’équivalent de `%load` d’IPython en IJulia est `IJulia.load()`.

À l’inverse, certains kernels comme IScala (Scala) prennent en charge des magics, de façon similaire à IPython, mais avec un jeu adapté à Scala et à la JVM. Une magic commence par `%` suivi d’un identifiant et, éventuellement, d’une saisie. Exemples notables :

```
# Type Information
%type 1
 
# Library Management
%libraryDependencies
%update
```
Comme indiqué plus haut, la bibliothèque `sparkmagic` fournit aussi des kernels Scala et Python pour se connecter automatiquement à un cluster Spark distant, exécuter du code et des requêtes SQL, gérer la configuration de votre serveur Livy et de vos jobs Spark, et générer des visualisations automatiques — sans écrire de code supplémentaire.

Par exemple, vous pouvez lancer des requêtes SparkSQL avec `%%sql` ou accéder aux informations et journaux d’application Spark via la magic `%%info`.

Si vous utilisez un autre kernel et vous demandez s’il supporte les magics, sachez que certains kernels s’appuient sur le projet metakernel et reprennent, dans bien des cas, les mêmes magics que le kernel IPython. La liste des magics metakernel est ici. metakernel est un modèle de kernel Jupyter/IPython incluant des magics de base.

Exemples :

- Le kernel MATLAB `matlab_kernel` ,
- Le kernel Octave `octave_kernel` ,
- Le kernel Java9 `java9_kernel` ,
- Le kernel Wolfram `wolfram_kernel` ,
- Le kernel SAS. … Et bien d’autres !

Ainsi, avec le kernel MATLAB, vous disposez par exemple des magics suivantes :

```
Available line magics:
%cd  %connect_info  %download  %edit  %get  %help  %html  %install  %install_magic  %javascript  %kernel  %kx  %latex  %load  %ls  %lsmagic  %magic  %parallel  %plot  %pmap  %px  %python  %reload_magics  %restart  %run  %set  %shell  %spell
 
Available cell magics:
%%debug  %%file  %%help  %%html  %%javascript  %%kx  %%latex  %%processing  %%px  %%python  %%shell  %%show  %%spell
```
Si vous comparez avec les magics disponibles par défaut dans le kernel IPython, vous noterez des recoupements :

```
Available line magics:
%alias  %alias_magic  %autocall  %automagic  %autosave  %bookmark  %cat  %cd  %clear  %colors  %config  %connect_info  %cp  %debug  %dhist  %dirs  %doctest_mode  %ed  %edit  %env  %gui  %hist  %history  %killbgscripts  %ldir  %less  %lf  %lk  %ll  %load  %load_ext  %loadpy  %logoff  %logon  %logstart  %logstate  %logstop  %ls  %lsmagic  %lx  %macro  %magic  %man  %matplotlib  %mkdir  %more  %mv  %notebook  %page  %pastebin  %pdb  %pdef  %pdoc  %pfile  %pinfo  %pinfo2  %popd  %pprint  %precision  %profile  %prun  %psearch  %psource  %pushd  %pwd  %pycat  %pylab  %qtconsole  %quickref  %recall  %rehashx  %reload_ext  %rep  %rerun  %reset  %reset_selective  %rm  %rmdir  %run  %save  %sc  %set_env  %store  %sx  %system  %tb  %time  %timeit  %unalias  %unload_ext  %who  %who_ls  %whos  %xdel  %xmode
 
Available cell magics:
%%!  %%HTML  %%SVG  %%bash  %%capture  %%debug  %%file  %%html  %%javascript  %%js  %%latex  %%perl  %%prun  %%pypy  %%python  %%python2  %%python3  %%ruby  %%script  %%sh  %%svg  %%sx  %%system  %%time  %%timeit  %%writefile
```
Pour discerner les magics spécifiques à IPython de celles exploitables ailleurs, posez-vous une question simple : la fonctionnalité est-elle propre à Python ou générale au langage que vous utilisez ?

Par exemple, `%pdb` (débogueur Python) ou `%matplotlib` sont spécifiques à Python et n’ont pas de sens avec un kernel JavaScript. En revanche, changer de répertoire avec `%cd` est assez général et devrait fonctionner dans tout environnement, sous réserve que le kernel gère les magics.

### Conversion et formatage des notebooks ?

La conversion et le formatage des notebooks relèvent de l’écosystème Jupyter. Deux outils typiques : `nbconvert` et `nbformat`.

Le premier convertit les notebooks vers d’autres formats pour présenter l’information sous des formes familières, publier des travaux, intégrer des notebooks dans des articles, collaborer et partager plus largement.

Le second définit le format des notebooks Jupyter : des documents JSON simples contenant des métadonnées (kernel, infos de langage), la version du format (majeure et mineure) et les cellules où sont stockés texte, code, etc.

### Enregistrement et chargement des notebooks ?

L’enregistrement et le chargement des notebooks sont des fonctionnalités de l’application Jupyter Notebook. Vous pouvez charger des notebooks (fichiers .ipynb) créés par d’autres en les téléchargeant puis en les ouvrant dans l’application Jupyter. Concrètement, créez un nouveau notebook, puis ouvrez le fichier via l’onglet « File », « Open », et sélectionnez votre notebook téléchargé.

Inversement, enregistrez vos propres notebooks via le même onglet « File » en choisissant « Download as » pour récupérer le fichier, ou « Save and Checkpoint » pour poser un jalon. Très utile pour une forme légère de contrôle de versions et revenir à un état antérieur si besoin. Bien sûr, vos modifications sont sauvegardées automatiquement toutes les quelques minutes, donc l’action n’est pas toujours nécessaire.

Notez que vous pouvez aussi préserver un notebook original en travaillant sur une copie et en y enregistrant vos changements !

### Raccourcis clavier et multi‑curseur ?

Sélection de cellules multiples, affichage/masquage des sorties, insertion de nouvelles cellules, etc. : autant d’actions disposant de raccourcis clavier dans Jupyter Notebook. La liste est accessible via le menu du haut : onglet « Help », puis « Keyboard Shortcuts ».

Le support multi‑curseur est également une fonctionnalité de Jupyter Notebook !

### Calcul parallèle distribué ?

Le réseau de calcul parallèle faisait partie d’IPython, mais depuis la version 4.0, il est scindé en un package autonome, `ipyparallel`. Ce package regroupe des scripts CLI pour contrôler des clusters Jupyter.

Bien que séparé, il demeure un composant puissant de l’écosystème IPython, souvent sous-estimé : au lieu d’un seul kernel Python, vous pouvez démarrer de nombreux kernels distribués sur plusieurs machines.

Cas d’usage typiques : exécuter un modèle de très nombreuses fois pour estimer la distribution de ses sorties ou leur sensibilité aux paramètres d’entrée. Lorsque les runs sont indépendants, on gagne du temps en les parallélisant sur plusieurs machines d’un cluster. Pensez à l’entraînement distribué de modèles ou à des simulations.

### Terminal ?

Cette fonctionnalité appartient à l’écosystème Jupyter : il existe Jupyter Console et une application de terminal Jupyter. Depuis l’origine, IPython désigne toutefois le terminal interactif initial pour Python, offrant une boucle REPL enrichie, particulièrement adaptée au calcul scientifique. C’était la référence avant 2011, date à laquelle le Notebook a introduit une interface web moderne et puissante pour Python.

On disposait aussi d’une console IPython, qui lançait deux processus : le shell terminal IPython et le profil/kernel par défaut (Python si non précisé). La console IPython est désormais dépréciée et, pour retrouver un usage similaire, il faut utiliser Jupyter Console, un frontal en terminal pour les kernels Jupyter. Il reprend l’expérience interactive d’IPython au terminal, mais permet de se connecter à n’importe quel kernel Jupyter, pas seulement à IPython. Vous pouvez ainsi tester tout kernel Jupyter installé, sans ouvrir un Notebook complet. La Console permet des interactions en terminal avec IJulia, IRKernel, etc.

Enfin, l’application Jupyter Notebook inclut aussi une application Terminal : un shell bash simple qui s’exécute dans votre navigateur. Vous le trouverez facilement en démarrant l’application puis en créant un nouveau terminal via le menu déroulant.

### Console Qt ?

La console Qt faisait partie d’IPython, mais elle a rejoint le projet Jupyter. C’est une application légère qui ressemble à un terminal tout en offrant des améliorations propres à une GUI : figures en ligne, véritable édition multilignes avec coloration syntaxique, aides contextuelles graphiques, etc. La console Qt peut utiliser n’importe quel kernel Jupyter.

## Conclusion

Cet article complète le guide ultime de DataCamp et retrace plus en détail l’histoire des notebooks computationnels, tout en présentant des fonctionnalités clés des projets IPython et Jupyter pour mieux comprendre leur évolution et leurs différences. La frontière n’est pas toujours nette si l’on oublie la perspective historique : certaines zones grises demeurent, difficiles à classer.