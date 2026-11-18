Cours

Dès que vous activez GitHub Copilot pour une équipe, les questions de configuration s'enchaînent : quelles données quittent l'IDE ? Quels dépôts doivent être exclus des suggestions ? Que se passe-t-il lorsqu'une suggestion correspond à du code public ?

Que vous soyez développeur ou administrateur déployant Copilot à l’échelle de l’organisation, maîtriser les aspects de confidentialité et de sécurité fait partie d’une bonne utilisation de l’outil.

Dans cet article, nous passons en revue la gestion de vos données par Copilot et la manière de configurer les paramètres de confidentialité, les exclusions de contenu et des garde-fous comme le filtre de duplication, ainsi que les étapes de dépannage les plus efficaces lorsque quelque chose ne fonctionne plus.

## Comment GitHub Copilot gère vos données

Comme nous l'expliquons dans notre tutoriel GitHub Copilot : bonnes pratiques, Copilot envoie un instantané du contexte de code environnant aux serveurs de GitHub dès que vous tapez dans votre IDE. Le modèle traite ce contexte et renvoie une suggestion : ce sont ces données d'interaction qui alimentent l'expérience.

Pour les utilisateurs des offres individuelles (Free, Pro et Pro+), la déclaration de confidentialité de GitHub autorise l'utilisation de ces données d'interaction pour l'entraînement des modèles. Les utilisateurs peuvent s'y opposer à tout moment dans leurs paramètres de confidentialité personnels. Les offres Business et Enterprise relèvent de conditions contractuelles distinctes excluant totalement l'utilisation des données d'interaction pour l'entraînement — aucune action requise de la part des utilisateurs.

### Ce qui relève des données d'interaction

Le code des dépôts privés stocké au repos n'est pas utilisé. En revanche, les données d'interaction générées lorsque vous utilisez activement Copilot dans un dépôt privé peuvent servir à l'entraînement, sauf si vous vous y opposez. Mais que recouvrent précisément les « données d'interaction » ?

Lorsque vous utilisez Copilot, le système collecte plusieurs types de signaux pour améliorer son assistance :

- **Entrées et invites :** Toutes les commandes ou questions envoyées à Copilot Chat ou au CLI.
- **Sorties :** Les suggestions de code ou réponses textuelles du modèle, y compris votre acceptation ou refus.
- **Contexte de code :** Les extraits entourant la position du curseur et le contenu des fichiers ouverts utilisés pour fournir des suggestions pertinentes.
- **Métadonnées et structure :** Noms de fichiers, structure du dépôt et votre navigation dans l'IDE.
- **Retour utilisateur :** Vos évaluations (pouce levé/baissé) et vos commentaires.

### Différences de traitement selon l'offre

GitHub propose plusieurs niveaux de compte : Free, Pro, Business et Enterprise. Le traitement des données varie selon le type de compte, comme indiqué dans le tableau ci-dessous.

| **Dimension** | **Free / Pro / Pro+** | **Business** | **Enterprise** | 
| Utilisation pour l'entraînement des modèles | Nécessite une opposition | Non. Exclu contractuellement | Non. Exclu contractuellement | 
| Code de dépôt privé au repos | Non utilisé | Non utilisé | Non utilisé | 
| Rétention des invites/sorties | IDE : non conservées. Hors IDE : 28 jours | IDE : non conservées. Hors IDE : 28 jours | IDE : non conservées. Hors IDE : 28 jours | 
| Contrôles admin | Individuels uniquement | Politiques au niveau de l'organisation et gestion des licences | Tous les contrôles Business, plus héritage des politiques à l'échelle de l'entreprise et journaux d'audit | 
| Exclusions de contenu | Non disponible | Disponible au niveau repo et organisation | Disponible à l'échelle de l'entreprise | 
| Protection PI | Non incluse | Oui, avec le filtre de duplication activé | Oui, avec le filtre de duplication activé | 

Outre l'opt-out pour les offres individuelles, il faut retenir que les utilisateurs Business et Enterprise bénéficient des exclusions de contenu, de la protection PI et de contrôles admin au niveau de l'organisation, absents des offres gratuites et individuelles. Pour une comparaison détaillée au-delà de la gestion des données, consultez notre guide GitHub Copilot : offres.

**Quelques précisions utiles :** Les exclusions de contenu ne s'appliquent pas encore au mode Edit, au mode Agent dans les chats IDE, au GitHub Copilot CLI, ni à l'agent cloud. La couverture de protection PI exige que le filtre de détection de duplication soit activé et que la suggestion soit utilisée sans modification.

## Configurer les paramètres de confidentialité de Copilot

La confidentialité des données est devenue un enjeu crucial pour toute entreprise. Voyons les paramètres qui déterminent si vos interactions servent à entraîner les modèles lorsque vous utilisez GitHub Copilot.

### Opt-out pour Free, Pro et Pro+

Pour votre compte individuel, vous pouvez refuser l'utilisation de vos données à des fins d'entraînement en allant dans GitHub Settings et en positionnant « Allow GitHub to use my data for AI model training » sur Disabled, comme illustré ci-dessous.

Cette opposition stoppe les collectes futures et ne réduit pas les fonctionnalités de Copilot. Toutefois, GitHub ne peut pas garantir le retrait des données déjà utilisées pour des entraînements antérieurs ; vos données préalablement collectées peuvent donc subsister dans des jeux d'entraînement existants.

### Politiques d'organisation et d'entreprise

Les utilisateurs Business et Enterprise sont déjà exclus de l'entraînement des modèles, mais les administrateurs doivent tout de même revoir les politiques de partage des données pour contrôler les fonctionnalités Copilot activées dans l'organisation :

- **Settings** de l'organisation >**Copilot** >**Policies** permet de gérer les fonctionnalités, l'attribution des licences et la sélection des modèles pour tous les membres.
- Les politiques au niveau org priment sur les préférences individuelles : tout réglage défini ici s'applique à tous.
- Les propriétaires Enterprise peuvent définir des politiques héritées par plusieurs organisations et auditer l'état depuis un tableau de bord unique.

## Utiliser les exclusions de contenu de Copilot

Vous pouvez empêcher Copilot d'accéder à certains contenus. Depuis les paramètres du dépôt, définissez les éléments que Copilot doit ignorer.

### Fonctionnement des exclusions de contenu

Pour les fichiers exclus :

- Les suggestions inline ne seront pas proposées.
- Leur contenu ne servira pas à générer des suggestions dans d'autres fichiers.
- Leur contenu ne sera pas utilisé par GitHub Copilot Chat pour ses réponses.
- La relecture de code par Copilot ne s'appliquera pas à ces fichiers.

Les exclusions peuvent être configurées par les administrateurs de dépôts, les propriétaires d'organisation et les propriétaires Enterprise.

### Configurer des exclusions au niveau du dépôt et de l'organisation

Au niveau repo, ouvrez Settings > Copilot > Content Exclusion, et indiquez des chemins à l'aide de motifs glob. Exemples courants : `**"**/secrets/**"**` pour exclure tout chemin contenant un répertoire secrets, et `**"*.env"**` pour exclure tous les fichiers d'environnement. 

L'API REST offre une option programmatique si vous gérez des exclusions à grande échelle et souhaitez versionner votre configuration.

Au niveau org, le chemin est : Org Settings > Copilot > Content Exclusion. Les règles définies ici s'appliquent à tous les dépôts de l'organisation.

Les règles au niveau org et repo sont additives : elles s'appliquent simultanément. Les règles au niveau Enterprise priment sur celles au niveau org et repo.

Pour vérifier que les exclusions fonctionnent : ouvrez un fichier exclu et demandez à Copilot Chat d'« expliquer ce fichier ». Si Chat fournit une réponse pertinente sur son contenu, l'exclusion ne s'applique pas. C'est le signal pour recharger l'extension et vérifier la syntaxe de la règle.

### Limitations à connaître

En utilisant les exclusions de contenu GitHub Copilot, gardez à l'esprit :

- Copilot CLI, le mode Agent et les Cloud Agents ne respectent pas les exclusions (mi-2026).
- Fuite sémantique : des infos de typage et définitions au survol issues de fichiers exclus peuvent encore influer indirectement sur les suggestions.
- Les symlinks et systèmes de fichiers distants ne sont pas couverts.

## Référencement de code et filtre de duplication

GitHub Copilot aide à comprendre la provenance du code proposé en référençant et en liant la source. Lorsque vous acceptez une suggestion de ce type, Copilot enregistre l'URL de la source et sa licence.

Vous pouvez ainsi décider d'utiliser ou non l'extrait et du type d'attribution à fournir.

### Fonctionnement du filtre de duplication

Lorsqu'une suggestion est générée, Copilot la compare à du code public connu. Si elle correspond à un dépôt public au-delà d'un seuil de similarité, elle est bloquée ou signalée avec attribution.

En cas d'acceptation, Copilot enregistre :

- La date et l'heure d'acceptation
- Le fichier où la suggestion a été ajoutée
- Un extrait du code ajouté
- La licence et l'URL de la source

Vous pouvez voir les références de code directement dans l'IDE lorsqu'une correspondance est signalée. Dans VS Code, elles apparaissent dans le panneau de sortie Copilot aux côtés de la suggestion.

### Ce que le filtre ne détecte pas

Le filtre de duplication ne fait pas correspondre :

- Les courts extraits et motifs trop génériques pour être signalés.
- Le code restructuré ou partiellement modifié par rapport à une source.

Son objectif est de repérer les correspondances textuelles (identiques ou quasi identiques), pas la similarité conceptuelle.

### Protection PI et garanties contractuelles

GitHub offre une protection en propriété intellectuelle (PI) aux clients Copilot Business et Enterprise. Si une suggestion entraîne une réclamation, GitHub prend en charge la défense juridique.

Deux conditions s'appliquent :

1. Le filtre de duplication doit être activé
2. Vous devez disposer d'une offre éligible

Les utilisateurs Free et Pro ne sont pas couverts. Il s'agit d'une garantie commerciale : elle n'empêche pas l'apparition de correspondances, mais couvre le risque juridique le cas échéant.

## Gérer les politiques Copilot sur GitHub.com

La page de politique Copilot sur GitHub.com est l'endroit où les admins org et Enterprise pilotent ce que Copilot peut faire pour l'équipe.

### Activer et désactiver des fonctionnalités Copilot

Les admins peuvent activer/désactiver indépendamment les complétions de code, Chat, la relecture de code, l'intégration GitHub CLI et le mode agent. Cette granularité est utile pour un déploiement progressif ou pour restreindre certaines capacités à des équipes spécifiques.

L'attribution des licences se fait également ici : vous choisissez qui a accès, par utilisateur ou par groupe.

### Configurer les modèles d'IA autorisés

Copilot prend en charge plusieurs modèles sous-jacents, et les administrateurs peuvent restreindre ceux autorisés pour l'organisation. Vous pouvez le verrouiller sur un modèle précis ou autoriser toutes les options disponibles et laisser le choix aux développeurs.

Pour les environnements publics et réglementés, GitHub propose des options conformes au programme FedRAMP (Federal Risk and Authorization Management Program).

Vérifiez les paramètres de politique Copilot dans l'onglet Copilot de votre organisation pour voir ce qui est disponible avec votre offre.

### Audit et conformité

Les indicateurs d'usage de Copilot, y compris les taux de complétion, le nombre d'utilisateurs actifs et les tendances par fonctionnalité et modèle, sont disponibles sous l'onglet Insights au niveau Enterprise et organisation (Insights > Copilot usage).

La politique « Copilot usage metrics » doit être activée pour accéder au tableau de bord. Des détails par membre sont disponibles via export NDJSON.

Les données de sièges et de licences sont distinctes et accessibles sous Org Settings > Copilot > Access.

Pour un tour d'horizon détaillé de la nouvelle Usage Metrics API et d'autres fonctionnalités avancées, n'hésitez pas à lire notre guide GitHub Copilot Enterprise.

Notez que le journal d'audit n'inclut pas les données de session client telles que les invites ; une solution personnalisée est nécessaire pour cela. Pour conserver l'historique au-delà de 180 jours ou mettre en place des alertes d'anomalie, GitHub recommande de diffuser le journal d'audit vers une plateforme SIEM via la fonctionnalité de streaming intégrée.

## Dépanner GitHub Copilot

Lorsque Copilot cesse de fonctionner, la cause est presque toujours l'une des suivantes. Vérifiez ces points avant d'ouvrir un ticket.

### Suggestions absentes ou interrompues

Commencez par l'icône d'état Copilot dans la barre d'état de l'IDE. Une diagonale sur l'icône indique qu'une exclusion de contenu est active pour le fichier courant.

Si l'icône paraît normale mais qu'aucune suggestion n'apparaît, contrôlez, dans l'ordre :

- Mettez à jour votre IDE et l'extension Copilot.
- Vérifiez que votre abonnement est actif et que votre compte dispose d'une licence attribuée.
- Contrôlez les règles d'exclusion pour le fichier et le repo, et testez votre connexion réseau.

Les configurations de proxy et de VPN bloquent fréquemment en silence : l'IDE doit joindre les serveurs Copilot sur l'infrastructure GitHub et certains proxys d'entreprise les bloquent sans message explicite.

### Exclusion de contenu inattendue

Après l'ajout ou la modification d'exclusions, un délai allant jusqu'à 30 minutes peut s'appliquer dans les IDE où les paramètres sont déjà chargés.

Pour appliquer les changements immédiatement :

- Dans VS Code, ouvrez la **Command Palette** et lancez**Developer: Reload Window** .
- Dans les IDE JetBrains et Visual Studio, fermez puis rouvrez l'application.
- Dans Vim/Neovim, aucune action : les exclusions sont récupérées automatiquement à chaque ouverture de fichier.

Après rechargement, testez explicitement : ouvrez le fichier exclu et demandez à Chat de l'expliquer. Si Chat décrit le contenu du fichier, l'exclusion ne s'applique pas ; revérifiez la syntaxe de la règle.

Notez que trois fonctionnalités Copilot ne gèrent pas les exclusions : Copilot CLI, l'agent de codage Copilot (agent cloud) et le mode Agent dans Copilot Chat en IDE. Si vous observez des accès inattendus avec ces modes, ce n'est pas une mauvaise configuration.

### Problèmes d'authentification et de jetons

Si Copilot est indisponible dans VS Code malgré une session ouverte, déconnectez-vous via l'icône Accounts en bas à gauche, rechargez la fenêtre (F1 > Developer: Reload Window dans VS Code), puis reconnectez-vous.

Dans Visual Studio, vérifiez que le compte GitHub connecté correspond à celui disposant d'une licence Copilot, rafraîchissez les informations d'identification si besoin ou supprimez/réajoutez le compte et redémarrez Visual Studio.

### Limitation de taux

Le modèle de facturation à l'usage de Copilot implique des capacités différentes par offre, et les modèles premium consomment cette capacité plus vite que les modèles de base.

Si les suggestions s'interrompent en cours de session ou si Copilot Chat renvoie des erreurs, passer en sélection de modèle automatique (ou vers un modèle à multiplicateur plus faible) peut résoudre le problème le temps que la fenêtre d'usage se réinitialise.

Les admins Enterprise peuvent surveiller l'usage de façon proactive via le tableau de bord d'analytique Copilot pour anticiper les limites.

En cas de problèmes de service, consultez githubstatus.com avant de déboguer localement.

## En conclusion

GitHub Copilot offre aux équipes un contrôle réel sur la gestion de leurs données : protections contractuelles liées à l'offre, exclusions de contenu fines et filtre de duplication.

Comprendre ces réglages (et savoir bien les configurer) vous permet d'adopter Copilot en toute sérénité, que vous soyez développeur individuel ou en déploiement à l'échelle de l'entreprise. Si quelque chose ne fonctionne pas comme prévu, les étapes de dépannage ci-dessus devraient vous remettre rapidement sur les rails.

Si vous souhaitez pratiquer avec GitHub Copilot, apprendre à le personnaliser et exploiter toutes ses fonctions intelligentes, nous vous recommandons vivement notre cours Software Development with GitHub Copilot.

## FAQ sur la confidentialité et le dépannage de GitHub Copilot

### GitHub Copilot envoie-t-il le code de mes dépôts privés aux serveurs de GitHub ?

**Copilot envoie le contexte de code immédiat depuis votre éditeur vers les serveurs de GitHub pour générer une suggestion. Il ne puise pas dans le code de vos dépôts privés stockés au repos sur GitHub.** 

### Comment empêcher GitHub d'utiliser mes données Copilot pour entraîner ses modèles ?

**Accédez à GitHub Settings, puis Copilot, et désactivez « Allow GitHub to use my data for AI model training ». Cet opt-out s'applique immédiatement aux collectes futures. Les utilisateurs Copilot Business et Enterprise sont automatiquement exclus de l'entraînement et n'ont rien à changer.**

### Qu'est-ce qu'une exclusion de contenu et comment la configurer ?

**Une exclusion de contenu est une règle qui empêche Copilot de lire ou de générer des suggestions à partir de fichiers ou de chemins spécifiques. Vous la configurez au niveau du dépôt via Settings > Copilot > Content Exclusion, avec des motifs glob comme  `'*.env'` ou `'**/secrets/**'`. Les propriétaires d'organisation peuvent définir des exclusions valables pour tous les dépôts.**

### Le filtre de duplication de GitHub Copilot protège-t-il des problèmes de droits d'auteur ?

**Il filtre les correspondances textuelles (identiques ou quasi) avec du code public connu et peut signaler la licence source. Il ne détecte pas le code conceptuellement similaire ni les réécritures partielles. Pour une protection PI complète, les clients Copilot Business et Enterprise bénéficient aussi d'une protection juridique si une suggestion déclenche une réclamation, à condition que le filtre de duplication soit activé.**

### Pourquoi GitHub Copilot a-t-il cessé d'afficher des suggestions dans un fichier précis ?

**Une diagonale sur l'icône d'état Copilot signifie qu'une règle d'exclusion est active pour ce fichier. Vérifiez les paramètres d'exclusion au niveau du repo et de l'org pour voir si le fichier correspond à une règle. Si oui et que vous souhaitez autoriser les suggestions, modifiez le motif d'exclusion.**

### Le mode agent respecte-t-il les exclusions de contenu ?

**Non. En mai 2026, le mode agent et les Cloud Agents de Copilot ne respectent pas les règles d'exclusion de contenu.**

### Comment résoudre les erreurs d'authentification GitHub Copilot ?

S**e déconnecter de GitHub dans votre IDE puis vous reconnecter. Vérifiez que le compte utilisé dispose d'une licence Copilot active. Pour les comptes Enterprise, réauthentifiez-vous si vous avez récemment changé de mot de passe ou si le SSO a évolué. Dans Visual Studio, contrôlez l'absence de versions dupliquées ou conflictuelles de l'extension Copilot.**

### Qu'est-ce que la protection PI de GitHub Copilot et qui y a droit ?

**La protection PI signifie que GitHub prend en charge les frais de défense si une suggestion Copilot déclenche une réclamation en propriété intellectuelle. Elle est disponible pour les clients Copilot Business et Copilot Enterprise, sous deux conditions : le filtre de duplication doit être activé et la suggestion doit être utilisée telle quelle. Les utilisateurs des offres Free et Pro ne sont pas couverts.**