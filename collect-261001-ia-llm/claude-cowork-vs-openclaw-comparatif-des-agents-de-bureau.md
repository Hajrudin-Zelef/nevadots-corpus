Cursus

Vous avez un dossier d'entretiens à synthétiser, un rapport du lundi que personne ne veut assembler, et un ordinateur que vous préféreriez refermer à 18 h. Deux outils promettent de vous décharger de ce travail : Claude Cowork, le mode agentique d'Anthropic dans l'app de bureau Claude, et OpenClaw, une passerelle auto-hébergée qui relie vos applis de messagerie à des agents IA.

Ils se concurrencent, mais sans jouer dans la même catégorie. Cowork vous vend un agent à permissions contrôlées avec un usage subventionné, à télécharger et connecter. OpenClaw vous livre un processus Node, une licence MIT, et toute la responsabilité de ce qui se passe ensuite.

Dans cet article, je compare Claude Cowork et OpenClaw sur la persistance sans surveillance, les paramètres de sécurité par défaut, le coût réel à trois niveaux d'usage, les frictions à l'installation, le verrouillage de modèle et l'administration en entreprise. Pour aller plus loin sur chaque outil, consultez notre tutoriel Claude Cowork, notre guide d'OpenClaw et notre sélection des alternatives à Claude Cowork.

## En bref

- La persistance fait la vraie différence : OpenClaw tourne en service 24 h/24 avec cron, tandis que la promesse de Cowork « fermez votre ordinateur portable » vaut pour le web et le mobile en bêta, pas pour l'app de bureau qui accède à vos dossiers locaux.
- Les modèles de sécurité sont inversés, et un seul a un incident documenté : OpenClaw a par défaut un accès système étendu, et sa place de marché de skills a été exploitée lors de l'incident ClawHavoc.
- Choisissez Claude Cowork si vous voulez des garde-fous de permissions, des contrôles admin, et une facture de 17 $/mois à oublier.
- Choisissez OpenClaw si vous avez besoin de tâches planifiées sans surveillance, de modèles locaux via Ollama, ou de contrôler l'emplacement de vos données.

## Qu'est-ce que Claude Cowork ?

Claude Cowork est le mode de travail agentique d'Anthropic : vous donnez un objectif à Claude, il opère dans les dossiers et outils connectés que vous choisissez, et vous remet un résultat terminé pour relecture. Il fonctionne sur macOS, Windows (x64 et arm64), Linux et ChromeOS, avec le web et le mobile en bêta. Dans l'app de bureau Claude, un curseur sous la fenêtre de saisie vous permet de basculer entre Cowork et le mode chat habituel.

Le principe mis en avant par Anthropic sur la page produit de Cowork est : « dites quoi, pas comment ». Claude privilégie d'abord les connecteurs, se rabat sur votre navigateur si nécessaire, et ne prend le contrôle de votre écran qu'en dernier recours (l'usage ordinateur reste en préversion de recherche). Il découpe aussi les gros travaux en segments exécutés en parallèle, pour rédaction et recherche simultanées plutôt que séquentielles.

La personnalisation passe par des plugins, qui réunissent des skills (savoir métier), des connecteurs (Amplitude, Microsoft 365, Google Drive, Slack, et autres), et des sous-agents en une seule installation. Anthropic a livré des places de marché privées de plugins pour les admins le 24 février 2026, et des contrôles de déploiement entreprise le 9 avril 2026. Nous détaillons l'organisation de fichiers, les conversions par lot et l'automatisation Chrome dans notre tutoriel pratique de Cowork.

## Qu'est-ce qu'OpenClaw ?

OpenClaw est une passerelle auto-hébergée qui connecte des applis de chat à des agents IA, développée ouvertement par l'OpenClaw Foundation, une organisation à but non lucratif, sous licence MIT. Vous exécutez un processus Gateway sur votre machine ou un serveur, qui devient le pont entre un agent toujours disponible et un grand nombre de services de messagerie : Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo et WebChat sont pris en charge. La Gateway fait foi pour les sessions, le routage et les connexions aux canaux.

La configuration réside dans un fichier unique à `~/.openclaw/openclaw.json`, et une interface de contrôle via navigateur tourne en local sur `http://127.0.0.1:18789/`. La documentation est claire sur le public visé : développeurs et power users qui veulent un assistant IA personnel joignable de partout sans confier leurs données à un service hébergé. TechRadar recense plus de 50 intégrations officielles, plus ClawHub, le registre communautaire de skills.

Pour le faire tourner, notre tutoriel d'installation OpenClaw couvre l'installation et l'appairage des canaux, et notre tutoriel OpenClaw avec Ollama couvre la voie 100 % locale.

## Claude Cowork vs OpenClaw : comparaison point par point

Voici la version résumée avant d'aborder les critères qui tranchent vraiment. Notez à quel point peu de lignes portent sur la « puissance ».

| Dimension | Claude Cowork | OpenClaw | 
|---|---|---|
| Exécution sans surveillance | Tâches planifiées sur le chemin cloud ; web et mobile en bêta. Les sessions de bureau dépendent de l'état de veille de votre machine | Démon toujours actif avec cron et webhooks | 
| Permissions par défaut | Périmètre limité au dossier, approbation avant actions significatives, et suppression soumise à validation | Accès système complet avec permissions étendues par défaut | 
| Installation | Télécharger l'app de bureau, se connecter, choisir des dossiers ; appairage QR pour le mobile | Node 26 recommandé (22.22.3+, 24.15+ ou 25.9+), config JSON, WSL2 sous Windows | 
| Coût | 17 $/mois Pro annuel (20 $ mensuel), 100 $ Max 5x, 200 $ Max 20x, 20 $/siège Team | Logiciel gratuit ; environ 3 $ par million de tokens entrée et 15 $ par million de tokens sortie sur Claude Sonnet, plus l'hébergement | 
| Choix de modèle | Modèles Anthropic uniquement | Claude, Gemini, GPT/Codex, Grok, Mistral, DeepSeek, Cohere, Qwen, plus Ollama, LM Studio, vLLM, SGLang | 
| Où interagir avec l'agent | App de bureau Claude ; web et mobile en bêta | N'importe quel canal de messagerie appairé, plus l'interface de contrôle locale | 
| Administration entreprise | RBAC par équipe, plafonds de dépenses, permissions par département, OpenTelemetry vers votre SIEM | Rien en standard ; à vous de durcir l'environnement | 
| Licence | Propriétaire, par abonnement uniquement | MIT, pilotée par la communauté | 

### Persistance et planification sans surveillance

La page produit de Cowork promet : « Fermez votre ordinateur portable, il continue » et met en avant des tâches planifiables à n'importe quelle cadence, sans surveillance. Cette promesse concerne le chemin cloud, et la page indique elle-même que web et mobile sont en bêta.

L'app de bureau est la surface qui accède à vos dossiers et applications locaux, et Anthropic le dit dans sa FAQ : l'app de bureau ajoute ce que le web et le mobile ne peuvent pas atteindre. Nos tests pratiques de Cowork Dispatch ont montré que l'expérience de bureau est liée à la session et s'interrompt dès que le Mac se met en veille. L'un des conseils que nous donnons est d'aller dans Réglages système et de modifier la temporisation de veille.

OpenClaw n'a pas cet astérisque. La Gateway s'installe comme un service et tourne en tâche de fond, avec cron et webhooks dans sa boîte à outils ; une tâche à 07 h démarre que vous soyez au bureau ou non. Si votre machine se met en veille, c'est votre réglage d'alimentation, pas une limite produit.

Cadrez cela honnêtement : si votre travail vit dans des dossiers locaux et que vous voulez qu'il s'exécute la nuit, OpenClaw le fait dès aujourd'hui, et Cowork le fait en bêta sur la surface cloud, qui ne peut pas toucher votre système de fichiers local. Anthropic comblera cet écart, et le badge bêta suggère que c'est en cours. Pour l'heure, la promesse « je lance et j'oublie » est le point le plus faible de Cowork.

### Sécurité, permissions et responsabilité

Cowork se situe par défaut du côté prudent :

- Accès limité aux dossiers et outils explicitement sélectionnés
- La suppression requiert toujours votre approbation
- Peut être configuré pour afficher son plan et attendre validation avant des actions significatives
- Permission par application lorsque l'usage ordinateur s'active, avec possibilité d'arrêter à chaque étape

OpenClaw, à l'inverse, tourne par défaut avec des permissions étendues. Notre propre comparaison de Cowork Dispatch et d'OpenClaw le décrit clairement comme offrant un accès système complet, qui peut devenir un risque si vous ne le configurez pas soigneusement. Le risque n'est pas théorique : comme expliqué dans notre décryptage d'OpenClaw, l'incident ClawHavoc a transformé l'accès à la place de marché des skills en vol d'identifiants. Tout ce que vous installez depuis ClawHub hérite de la portée de l'agent : shell, fichiers, navigateur.

Mais Cowork n'est pas exempt non plus. Ses contrôles sont configurables, pas automatiques : un admin qui ne touche jamais aux permissions déploie un agent qui agit d'abord. Plus ennuyeux, la page tarifs d'Anthropic précise que l'activité Cowork n'est pas encore capturée dans les journaux d'audit ni l'API Compliance, ce qui fait tache sous un discours entreprise incluant RBAC, plafonds de dépenses et flux OpenTelemetry vers votre SIEM.

Mon analyse : Cowork est déployable dans une organisation avec équipe sécurité. OpenClaw est déployable dans une organisation avec équipe sécurité ayant validé explicitement un hôte sandboxé. Ce ne sont pas les mêmes prérequis.

### Le coût réel en usage léger, quotidien et intensif

Cowork facture par abonnement forfaitaire :

- **Pro :** 17 $/mois sur l'annuel (200 $ à l'avance), ou 20 $ facturé mensuellement
- **Max 5x :** 100 $/mois
- **Max 20x :** 200 $/mois
- **Team :** 20 $ par siège, de 2 à 150 personnes
- **Enterprise :** sur devis

Deux points comptent plus que les prix affichés. Anthropic prévient que Cowork consomme les limites plus vite que Chat, car Claude coordonne des sous-agents et des appels d'outils. Un seul run Cowork qui se déploie sur un dossier entier vaut bien plus qu'un message de chat, d'où l'orientation des gros utilisateurs vers Max 20x.

Le logiciel OpenClaw est gratuit ; le coût, ce sont les tokens plus l'hébergement. Notre tutoriel d'installation chiffre Claude Sonnet à environ 3 $ par million de tokens entrée et 15 $ par million en sortie, Opus étant plus cher. L'hébergement managé tiers démarre autour de 9,99 $/mois si vous ne voulez pas gérer la machine vous-même, selon notre panorama d'alternatives.

Le détail que presque personne ne souligne : si vous payez déjà pour Claude Pro ou Max, vous pouvez générer un jeton de configuration via le CLI Claude Code et faire tourner OpenClaw sur votre abonnement, au lieu d'une facturation API au compteur. Cela atténue l'argument coût : vous obtenez le démon cron et le routage multi-canaux d'OpenClaw sur le même abonnement qui finance Cowork. Ce n'est pas « gratuit », les limites d'usage d'Anthropic s'appliquent toujours, mais c'est le point le plus actionnable de ce comparatif.

### Choix de modèle et verrouillage

Cowork est lié à Anthropic. C'est le marché implicite contre un usage subventionné ; cela ne pose souci que si vous avez besoin d'un modèle non-Claude précis ou d'arbitrer les prix des tokens. La plupart du temps, l'un des quatre niveaux Claude (Fable, Opus, Sonnet, Haiku) couvre votre cas d'usage, Sonnet étant un choix par défaut raisonnable.

OpenClaw est agnostique par conception. La liste des fournisseurs inclut Claude, Gemini, OpenAI/ChatGPT/Codex, Grok, Mistral, DeepSeek, Cohere et Qwen, ainsi que des couches de routage comme OpenRouter, LiteLLM, Amazon Bedrock et Vertex AI. Le vrai différenciateur : l'exécution locale via Ollama, LM Studio, vLLM et SGLang, seule façon ici de faire tourner un agent sans qu'aucune donnée ne quitte votre matériel.

Ne sur-vendez pas ce critère. La plupart des utilisateurs hésitants entre ces deux outils feront tourner Claude de toute façon, et la documentation recommande d'utiliser le meilleur modèle de génération disponible pour la qualité et la sécurité. La flexibilité des modèles compte si vous avez une contrainte de confidentialité ou un flux à base de modèles locaux ; sinon, c'est un plus.

### Friction à l'installation

L'installation de Cowork se résume à télécharger et se connecter. Vous récupérez l'installeur pour macOS, Windows, Linux ou ChromeOS, vous vous connectez à une offre payante, cliquez sur l'onglet Cowork et pointez un dossier. L'appairage du téléphone se fait par QR code.

OpenClaw en demande davantage :

- 
Node 26 recommandé (22.22.3+, 24.15+ ou 25.9+ fonctionnent aussi)
- 
Une clé API
- 
Un fichier de config à `~/.openclaw/openclaw.json`
- 
WSL2 sous Windows (natif sur macOS et Linux)

Notre guide OpenClaw et notre tutoriel OpenClaw + Ollama vous accompagnent pas à pas.

Rien de difficile pour qui est à l'aise en ligne de commande, et `openclaw onboard` prend en charge le parcours guidé. C'est en revanche un vrai filtre pour les profils non techniques, cœur de cible de Cowork, et un non-sujet pour quiconque a déjà installé un service Node.

### Où converser avec l'agent

Cowork vit dans l'app de bureau Claude, avec le web et le mobile en bêta ; la conversation se passe donc dans l'interface d'Anthropic. OpenClaw vous retrouve dans ce que vous utilisez déjà : Telegram, Slack, WhatsApp, Signal, iMessage, Discord, Microsoft Teams, Google Chat, ou l'interface de contrôle locale. Des plugins communautaires étendent à WeChat, Yuanbao et Zalo.

C'est une question de confort plus que de capacité. Écrire à votre agent depuis Signal est agréable, cela ne le rend pas meilleur pour rapprocher un tableur.

### Administration entreprise

Cowork l'emporte par défaut, car OpenClaw ne joue pas sur ce terrain. Cowork offre aux admins des contrôles auxquels OpenClaw n'a pas de réponse :

- Désactiver Cowork à l'échelle de l'organisation depuis Admin Settings
- Contrôle d'accès par rôle (RBAC) par équipe
- Plafonds de dépenses
- Permissions d'outils par département
- Activité diffusée vers votre SIEM via OpenTelemetry

Cowork et le connecteur Slack sont inclus dans les sièges Team Standard et Premium.

Le bémol de la section sécurité s'applique : l'activité Cowork n'apparaît pas encore dans les journaux d'audit ni l'API Compliance. Si votre équipe conformité exige des traces immuables de chaque action d'agent, aucun des deux outils ne répond à ce besoin aujourd'hui ; mieux vaut le dire avant de signer.

### Où ils sont au coude-à-coude

Plusieurs dimensions paraissent différenciantes et ne le sont pas. Les deux orchestrent des sous-agents en sessions isolées. Les deux lisent et écrivent des fichiers locaux, et la liste de formats pris en charge par Cowork (Word, Excel, PowerPoint, PDF, CSV, YAML, notebooks Jupyter, et la plupart des fichiers de code) est fournie mais attendue pour cette catégorie d'outils.

L'étendue des intégrations est comparable en volume et différente en nature : Cowork a des connecteurs plus une place de marché de plugins regroupant skills et sous-agents, OpenClaw propose 50+ intégrations officielles, ClawHub, et 10+ canaux de chat.

## Quand choisir Claude Cowork vs OpenClaw

La décision porte rarement sur l'agent le plus « intelligent ». Il s'agit de choisir entre un produit avec contrat de support et une infrastructure que vous maintenez.

| Cas d'usage | Recommandé | Pourquoi | 
|---|---|---|
| Tâches nocturnes récurrentes sur fichiers locaux | OpenClaw | Démon toujours actif avec cron ; le chemin sans surveillance de Cowork est la bêta cloud | 
| Équipe non technique, pas de budget ops | Claude Cowork | Télécharger, se connecter, choisir un dossier ; pas de runtime Node ni de fichier de config | 
| Environnement réglementé avec revue sécurité | Claude Cowork | Périmètre par dossier, garde-fous d'approbation, RBAC et OpenTelemetry vers votre SIEM | 
| Exécuter un modèle local sans sortie de données | OpenClaw | Prise en charge native d'Ollama, LM Studio, vLLM et SGLang | 
| Nettoyage ponctuel de dossiers et conversions de formats | Claude Cowork | Inclus dans l'offre Pro à 17 $/mois avec usage subventionné | 
| Discuter avec un agent toute la journée depuis Signal ou Telegram | OpenClaw | Une Gateway dessert 10+ canaux simultanément | 
| Déployer à 100 sièges avec contrôle des dépenses | Claude Cowork | Offre Team à 20 $ par siège, bascules admin, permissions par département | 

### Choisissez Claude Cowork si…

- **Vous voulez un agent qui demande avant d'agir** . La suppression requiert votre approbation par défaut, et les permissions peuvent obliger Claude à présenter son plan d'abord.
- **Votre budget est une ligne d'abonnement** , pas une facture API au compteur. Pro à 17 $/mois couvre l'usage léger sans calculatrice à tokens.
- **Vous déployez pour une équipe** . Les paramètres admin couvrent l'accès par équipe, les plafonds de dépenses et les permissions d'outils par département.
- **Votre travail est déjà dans Microsoft 365, Google Drive ou Slack** , et vous préférez des connecteurs à un contrôle de navigateur.

### Choisissez OpenClaw si…

- **La tâche doit tourner pendant votre sommeil** . La Gateway s'installe comme un service et cron s'exécute indépendamment de l'état de session.
- **Vous voulez écrire à votre agent depuis l'appli que vous avez déjà ouverte** , que ce soit Telegram, Signal ou Microsoft Teams.
- **Vous avez besoin de flexibilité de modèles** , pour l'inférence locale via Ollama ou pour router entre fournisseurs avec LiteLLM ou OpenRouter.
- **Vous savez durcir un service Node** , car la posture par défaut est l'accès système complet, et ClawHavoc a montré le coût d'une mauvaise configuration.

## Peut-on utiliser Claude Cowork et OpenClaw ensemble ?

Oui, et c'est même une très bonne configuration. Leurs faiblesses sont inversées, donc les combiner couvre un terrain qu'aucun ne gère seul.

La division du travail suit les écarts de persistance et de sécurité abordés plus haut.

- **Cowork** prend le travail interactif sur documents locaux sensibles, où ses garde-fous d'approbation valent le verrouillage.
- **OpenClaw** prend les tâches planifiées de nuit sur fichiers locaux que l'app de bureau de Cowork, liée à la session, ne peut pas encore exécuter.

Ce qui rend cela pragmatique plutôt que coûteux : faites tourner OpenClaw avec un jeton d'abonnement Claude généré via le CLI Claude Code, et une seule facture finance les deux outils. Vous maintenez deux systèmes au lieu d'un, et les limites d'usage d'Anthropic s'appliquent aux deux. Si Cowork comble l'écart de planification locale, vous pourrez revenir à un seul outil ; considérez donc ce duo comme la réponse du moment, pas une architecture figée.

## Conclusion

Si vous voulez que le travail avance pendant votre sommeil et que vous savez faire tourner un service Node en sécurité, utilisez OpenClaw. Si vous voulez un agent acceptable pour votre service juridique et une facture à oublier, utilisez Claude Cowork. Tout le reste découle de ces deux phrases.

Mon propre setup, pour une version assumée : Cowork pour tout ce qui touche à des documents clients, parce que les garde-fous valent le verrouillage, et OpenClaw avec un jeton d'abonnement Claude pour les tâches planifiées que Cowork ne peut pas encore exécuter en local. Cela coûte un seul abonnement et un peu de temps en terminal. Et aucun des deux outils n'a besoin d'être l'unique réponse : c'est souvent là que l'on atterrit après les avoir vraiment essayés tous les deux.

## FAQ

### Claude Cowork est-il gratuit ?

Non. Cowork est inclus dans chaque offre payante de Claude : Pro à 17 $/mois avec remise annuelle (200 $ facturés à l'avance, ou 20 $ au mois), Max 5x à 100 $/mois, Max 20x à 200 $/mois, Team à 20 $ par siège, et Enterprise. Anthropic prévient que Cowork consomme les limites d'usage plus vite que Chat, car les tâches agentiques coordonnent des sous-agents et des appels d'outils.

### OpenClaw peut-il tourner sur mon abonnement Claude Pro au lieu de payer au token ?

Oui. Vous pouvez générer un jeton de configuration via le CLI Claude Code et utiliser votre abonnement Claude Pro ou Max plutôt qu'une facturation API au compteur. Les limites d'usage d'Anthropic s'appliquent toujours, mais cela supprime le coût au token qui rend autrement l'usage intensif d'OpenClaw onéreux, puisque Claude Sonnet tourne à environ 3 $ par million de tokens entrée et 15 $ par million en sortie.

### Claude Cowork continue-t-il à travailler quand mon ordinateur portable est fermé ?

Sur le web et le mobile, oui, et Anthropic les indique en bêta. Sur l'app de bureau, qui est la surface accédant à vos dossiers et applis locaux, nos tests pratiques Claude Cowork Dispatch montrent que les sessions s'arrêtent dès que la machine se met en veille. Si vous avez besoin aujourd'hui de lancer des tâches sur fichiers locaux la nuit, le démon Gateway toujours actif d'OpenClaw avec cron est un choix plus sûr.

### Quel est le plus sûr, Claude Cowork ou OpenClaw ?

Claude Cowork, par défaut. Il restreint Claude aux dossiers et outils que vous choisissez, exige une approbation avant toute suppression, et demande la permission par application lors de l'usage écran. OpenClaw tourne avec accès système complet par défaut, et l'incident ClawHavoc a transformé l'accès à la place de marché des skills en vol d'identifiants. Cowork n'est pas parfait non plus : Anthropic indique que l'activité Cowork n'est pas encore capturée dans les journaux d'audit ni l'API Compliance.

### Puis-je utiliser Claude Cowork et OpenClaw ensemble ?

Oui, et c'est ce que je fais. Cowork gère le travail interactif sur des documents locaux sensibles où les garde-fous comptent, tandis qu'OpenClaw exécute les tâches planifiées de nuit que Cowork ne peut pas encore lancer sur des fichiers locaux. En faisant tourner OpenClaw avec un jeton d'abonnement Claude, une seule facture couvre les deux.

### OpenClaw est-il gratuit ?

Le logiciel est gratuit et sous licence MIT, mais l'exécution ne l'est pas. Vous payez les tokens de modèle plus l'hébergement si vous ne l'exécutez pas sur votre propre machine ; l'hébergement managé tiers commence autour de 9,99 $/mois. Une façon de réduire le coût en tokens : si vous avez déjà Claude Pro ou Max, vous pouvez générer un jeton via le CLI Claude Code et faire tourner OpenClaw sur cet abonnement plutôt que de payer des tarifs API au compteur.

**Rédacteur en chef Data Science chez DataCamp |** **Je suis passionné par la prévision et le développement à l'aide d'API.**