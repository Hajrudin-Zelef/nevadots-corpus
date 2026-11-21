---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-3-5-lightning-nvidia-lance-un-agent-ia-open-2026
title: "Nemotron 3.5 Lightning : Nvidia Lance un Agent IA Open [2026]"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-08-11"]
keywords: ["modele", "nvidia", "nemotron", "lightning", "switchyard", "open", "sous", "lance"]
source: https://tech-insider.org/fr/nvidia-nemotron-3-5-lightning-nemo-switchyard-2026/
source_anchor: ""
source_lines: [1, 134]
section: "NVIDIA_Hardware"
---
Nvidia a lancé le 11 août 2026 deux briques logicielles qui changent la donne pour les développeurs d’agents IA en Europe : **Nemotron 3.5 Lightning**, un modèle ouvert de 30 milliards de paramètres taillé pour les tâches longues et autonomes, et **NeMo Switchyard**, une bibliothèque open source qui route le trafic entre plusieurs fournisseurs de modèles. L’annonce tombe six jours après l’entrée en application des règles de transparence de l’AI Act européen, le 2 août 2026, ce qui place immédiatement ces deux outils sous le regard des régulateurs bruxellois. Pour les équipes techniques françaises qui déploient des agents IA en production, la coïncidence de calendrier n’a rien d’anodin.

## Nemotron 3.5 Lightning : ce que Nvidia a réellement publié

Nemotron 3.5 Lightning repose sur une architecture hybride qui mêle blocs Mamba-2, mélange d’experts (MoE) et couches d’attention classique. Sur le papier, le modèle affiche 30 milliards de paramètres au total, mais seuls 3 milliards sont activés à chaque token traité (configuration dite « 30B A3B »). Cette parcimonie est le cœur de l’argument de vente : Nvidia ne cherche pas à dominer les classements de raisonnement pur, mais à proposer un modèle rapide et bon marché à faire tourner en boucle, comme le ferait un agent qui enchaîne des dizaines d’appels d’outils avant de rendre un résultat final.

La fenêtre de contexte annoncée grimpe jusqu’à 1 million de tokens, un chiffre qui rapproche Nemotron 3.5 Lightning des modèles à contexte long de Google ou d’Anthropic, mais avec un coût d’inférence bien inférieur grâce à l’activation partielle des paramètres. Nvidia distribue le modèle sous deux formats de poids, NVFP4 et BF16, et publie non seulement les poids mais aussi une partie des données d’entraînement et des recettes de fine-tuning, sous la licence **OpenMDW-1.1** (Open Model and Data Weights). Cette licence autorise un usage commercial, ce qui distingue Nemotron 3.5 Lightning d’un simple modèle de recherche publié pour la forme.

Autre détail qui parlera aux équipes francophones : la fiche technique du modèle liste explicitement le français, l’espagnol, l’allemand, l’italien et le japonais parmi les langues supportées, aux côtés de l’anglais et des langages de programmation courants. Ce n’est pas un support multilingue de façade : Nvidia positionne clairement Nemotron 3.5 Lightning comme un modèle déployable en Europe, du edge (cartes Jetson) jusqu’au datacenter (clusters DGX Spark), en passant par des piles de service standard comme vLLM, SGLang ou TensorRT-LLM.

## NeMo Switchyard : router les agents plutôt que les enfermer

Le deuxième outil publié le même jour, NeMo Switchyard, répond à un problème différent mais complémentaire. Une fois qu’une équipe a déployé plusieurs modèles (un modèle rapide pour les tâches simples, un modèle puissant pour les cas complexes, un modèle local pour les données sensibles), il faut un mécanisme pour aiguiller chaque requête vers le bon backend. C’est exactement le rôle de Switchyard : un proxy écrit en Rust, distribué sous licence Apache 2.0, qui expose un point d’entrée unique compatible avec l’API OpenAI tout en routant les appels, selon des règles définies par l’équipe, vers des modèles hébergés localement (vLLM, Ollama) ou chez des fournisseurs tiers.

Switchyard traduit également entre les formats d’API OpenAI et Anthropic, ce qui évite de réécrire l’intégration à chaque changement de fournisseur, et journalise des métriques opérationnelles utiles pour le pilotage des coûts. Le dépôt GitHub officiel, hébergé sous l’organisation NVIDIA-NeMo, précise toutefois que le projet est en phase « pré-alpha » et n’est pas recommandé pour un usage en production immédiat. Pour les équipes DevOps françaises qui expérimentent déjà des architectures multi-modèles, c’est un signal à suivre plutôt qu’à adopter dès aujourd’hui.

La logique stratégique de Nvidia devient plus claire une fois les deux annonces mises côte à côte, comme le détaille MarkTechPost dans son analyse du lancement. L’entreprise ne se contente plus de vendre les puces sur lesquelles tournent les modèles : elle publie désormais le modèle lui-même et l’infrastructure logicielle qui décide quel modèle traiter chaque requête. C’est une extension verticale de la chaîne de valeur de l’IA, du silicium jusqu’à la couche d’orchestration applicative, qui rappelle la stratégie déjà engagée avec CUDA, TensorRT et la gamme NIM.

## Tarification : combien coûte réellement Nemotron 3.5 Lightning

Comme pour tout modèle à poids ouverts, il n’existe pas de tarif unique fixé par Nvidia : le prix dépend du fournisseur d’hébergement choisi. DeepInfra facture le modèle 0,05 dollar par million de tokens en entrée et 0,20 dollar par million de tokens en sortie. CoreWeave propose une tarification légèrement supérieure, à 0,10 dollar en entrée et 0,25 dollar en sortie. OpenRouter, de son côté, propose un palier gratuit, avec toutefois une limitation notable : la génération en sortie plafonne à 65 536 tokens par appel, même si la fenêtre de contexte totale du modèle atteint 1 million de tokens. Pour un hébergement local, sur ses propres serveurs ou via un fournisseur cloud européen, le coût dépend uniquement du matériel utilisé, la licence OpenMDW-1.1 n’imposant aucune redevance.

| Fournisseur | Prix entrée (par million de tokens) | Prix sortie (par million de tokens) | Particularité | 
|---|---|---|---|
| DeepInfra | 0,05 $ | 0,20 $ | Tarif le plus bas identifié au lancement | 
| CoreWeave | 0,10 $ | 0,25 $ | Infrastructure Nvidia dédiée | 
| OpenRouter (gratuit) | 0 $ | 0 $ | Sortie plafonnée à 65 536 tokens par appel | 
| Auto-hébergement (vLLM/SGLang/TensorRT-LLM) | Coût matériel uniquement | Coût matériel uniquement | Aucune redevance de licence sous OpenMDW-1.1 | 

Cette fourchette de prix place Nemotron 3.5 Lightning dans la catégorie des modèles économiques, nettement en dessous des tarifs pratiqués par les modèles propriétaires de pointe. Le compromis est assumé : Nvidia ne prétend pas rivaliser avec les meilleurs modèles de raisonnement du marché sur la qualité brute, mais mise sur le rapport coût-débit pour les charges de travail agentiques répétitives, où un modèle plus cher n’apporterait pas de gain proportionnel.

## Le contexte réglementaire : l’AI Act change la donne pour les modèles ouverts

Le calendrier de cette double annonce ne pouvait pas mieux tomber pour illustrer un tournant réglementaire européen. Depuis le 2 août 2026, la Commission européenne applique de nouvelles règles de transparence issues de l’AI Act, pilotées par son AI Office en coordination avec les autorités nationales. Selon la Commission européenne, « à partir du 2 août 2026, l’AI Office de la Commission européenne, avec les autorités nationales, commencera à faire appliquer le règlement sur l’intelligence artificielle ».

La Commission précise également que « à la même date, de nouvelles règles de transparence commenceront à s’appliquer, exigeant de certains systèmes d’IA qu’ils informent les utilisateurs lorsqu’ils interagissent avec une IA et lorsqu’un contenu a été généré ou modifié par elle ». Concrètement, cela signifie que « les chatbots et autres systèmes d’IA interactifs devront indiquer aux utilisateurs qu’ils ont affaire à une IA, et non à un humain », et que « les deepfakes (images, vidéos ou fichiers audio modifiés ou générés par IA) devront être étiquetés ». La Commission ajoute que « le contenu généré ou modifié par IA devra également porter des marques lisibles par machine afin d’être détecté plus facilement ».

Pour un modèle comme Nemotron 3.5 Lightning, destiné à alimenter des agents autonomes qui interagissent potentiellement avec des utilisateurs finaux en Europe, ces obligations de marquage et de divulgation s’appliquent dès la mise en production, quel que soit le statut « open source » du modèle sous-jacent. Le fait que Nvidia publie les poids et les données d’entraînement facilite paradoxalement l’audit de conformité : contrairement à un modèle propriétaire en boîte noire, les équipes de conformité peuvent inspecter directement le comportement du modèle plutôt que de se fier aux seules déclarations du fournisseur.

## Comparatif : Nemotron 3.5 Lightning face aux autres modèles ouverts

Le marché des modèles à poids ouverts s’est considérablement densifié en 2026. DeepSeek V4, Mistral Large 3, GLM-5.2, Kimi K2.6 et désormais Nemotron 3.5 Lightning se disputent un segment où le critère décisif n’est plus seulement le score de référence, mais le coût par token traité et la facilité de déploiement sur infrastructure européenne. Le tableau ci-dessous résume les positionnements connus au 17 août 2026, à partir des informations publiées par chaque fournisseur.

| Modèle | Paramètres | Contexte | Licence | Positionnement | 
|---|---|---|---|---|
| Nemotron 3.5 Lightning (Nvidia) | 30B total / 3B actifs | 1 M tokens | OpenMDW-1.1 | Agents longs, faible coût d’inférence | 
| DeepSeek V4 | Architecture MoE, détails non divulgués | 128K tokens | Open-weight, licence permissive | Raisonnement et code à prix cassé | 
| Mistral Large 3 | Non divulgué publiquement | 128K tokens | Licence commerciale/ouverte hybride | Conformité RGPD, hébergement UE | 
| GLM-5.2 (Zhipu) | Architecture MoE, taille non détaillée | 128K tokens | Open-weight | Tarif ultra-bas sur l’API | 
| Kimi K2.6 (Moonshot AI) | Architecture MoE, taille non détaillée | 256K tokens | Open-weight | Agents et usage prolongé | 

Ce qui distingue Nemotron 3.5 Lightning de la concurrence chinoise (DeepSeek, GLM, Kimi) n’est pas la taille du modèle, plutôt modeste à 30 milliards de paramètres, mais la stratégie de distribution. Nvidia contrôle à la fois le matériel (GPU), la couche logicielle d’inférence (TensorRT-LLM) et désormais le modèle lui-même, ce qui lui permet d’optimiser Nemotron 3.5 Lightning spécifiquement pour ses propres puces, du Jetson embarqué jusqu’au DGX Spark de datacenter. Aucun des concurrents cités ne dispose de cette intégration verticale complète.

## Pourquoi Nvidia sort de son rôle de fournisseur de puces

Historiquement, Nvidia a construit sa position dominante sur la vente de GPU et sur l’écosystème logiciel CUDA qui verrouille les développeurs autour de son matériel. La série Nemotron, lancée initialement pour démontrer les capacités des puces Nvidia sur des charges de travail réelles, a progressivement pris une existence propre : d’abord comme modèles de démonstration, puis comme produits à part entière proposés en téléchargement libre sur le catalogue NGC (Nvidia GPU Cloud).

Avec Nemotron 3.5 Lightning et NeMo Switchyard publiés le même jour, Nvidia franchit une étape supplémentaire : elle ne fournit plus seulement un modèle isolé, mais une chaîne d’outils cohérente qui va du silicium jusqu’à l’orchestration d’agents multi-modèles. Cette stratégie répond à une pression concurrentielle réelle : à mesure que des fournisseurs cloud comme AWS, Google Cloud et Microsoft Azure développent leurs propres puces d’inférence, Nvidia doit convaincre les développeurs de rester dans son écosystème pour des raisons qui dépassent la seule performance brute du GPU. Offrir un modèle ouvert optimisé nativement pour sa pile logicielle est un moyen efficace de maintenir cette adhérence.

Pour les entreprises européennes, cette bataille a une conséquence pratique : elles peuvent désormais déployer un modèle d’agent performant, à coût maîtrisé, sur une infrastructure qu’elles contrôlent entièrement, sans dépendre d’un abonnement API à un fournisseur américain de modèles fermés. C’est un argument qui résonne particulièrement fort dans le débat français sur la souveraineté numérique, même si Nvidia reste elle-même une entreprise américaine et que la puce sur laquelle tourne le modèle reste, elle, absolument non souveraine.

## L’angle mort : la souveraineté européenne face à Nvidia et aux modèles chinois

La publication de Nemotron 3.5 Lightning intervient alors que le débat sur la souveraineté de l’IA européenne s’intensifie. Une analyse relayée début août 2026 souligne que la souveraineté de l’IA en Europe reste fragile, alors même que les autorités américaines ont temporairement coupé l’accès étranger à Mythos, un modèle développé par Anthropic, illustrant à quel point les entreprises européennes restent dépendantes de décisions prises hors du continent, qu’elles concernent des modèles fermés américains ou, désormais, des modèles ouverts comme ceux de Nvidia.

Dans ce contexte, la Commission européenne a lancé un appel à projets pour financer sept « méga-usines d’IA » (AI gigafactories) à hauteur de 5 milliards d’euros, un effort explicitement destiné à combler le retard de l’Europe face aux États-Unis et à la Chine sur l’entraînement de grands modèles. Le paradoxe est frappant : au moment même où Bruxelles muscle son cadre réglementaire et ses investissements pour bâtir une capacité de calcul souveraine, c’est un fournisseur américain de puces qui publie le modèle ouvert le plus visible du mois, optimisé pour son propre matériel. Pour Mistral AI, le champion français des modèles ouverts, la concurrence ne vient donc plus seulement d’OpenAI, de Google ou d’Anthropic, mais aussi d’un acteur qui contrôle une bonne partie de l’infrastructure sur laquelle tournent ses propres modèles.

## Impact sur le marché et sur les développeurs d’agents IA

L’impact immédiat de cette double publication se mesure d’abord dans l’écosystème des hébergeurs de modèles. DeepInfra, CoreWeave et OpenRouter ont tous intégré Nemotron 3.5 Lightning à leur catalogue dans les heures ou les jours suivant l’annonce du 11 août 2026, un délai très court qui traduit l’appétit du marché pour un modèle taillé sur mesure pour les agents à faible coût. Cette rapidité d’adoption contraste avec le rythme habituellement plus lent d’intégration des modèles propriétaires fermés, qui nécessitent des accords commerciaux formels avant tout accès API.

Pour les équipes de développement qui construisent des agents autonomes, capables d’enchaîner des dizaines d’appels d’outils sans supervision humaine constante, le choix d’un modèle ne se résume plus à sa position dans un classement de référence. La combinaison contexte long (jusqu’à 1 million de tokens), faible coût par token et activation partielle des paramètres change directement l’économie d’un agent qui doit relire l’historique complet d’une conversation ou d’un flux de travail à chaque étape. Un agent qui coûte dix fois moins cher à faire tourner peut se permettre dix fois plus d’itérations de vérification avant de rendre une réponse, ce qui améliore potentiellement la fiabilité sans changer de modèle.

NeMo Switchyard, de son côté, répond à un besoin identifié depuis plusieurs mois par les équipes qui opèrent des architectures multi-modèles en production : éviter de coder en dur l’intégration à un fournisseur unique. En traduisant nativement entre les formats d’API OpenAI et Anthropic, l’outil réduit la charge d’ingénierie nécessaire pour comparer, tester ou remplacer un modèle par un autre, ce qui accélère mécaniquement l’adoption de nouveaux modèles ouverts comme Nemotron dès leur sortie.

## Contexte historique : de la première génération Nemotron à Lightning

La famille Nemotron n’est pas née en 2026. Nvidia a commencé à publier des modèles sous ce nom pour démontrer les performances d’inférence de ses GPU sur des charges réelles, avant que la gamme ne devienne un produit à part entière avec sa propre feuille de route de versions. Chaque itération a progressivement réduit l’écart entre les modèles ouverts et les modèles fermés sur les tâches de code et de raisonnement, tout en maintenant l’avantage de coût propre aux poids ouverts auto-hébergeables.

Ce qui change avec la génération 3.5 Lightning, c’est le positionnement explicite sur les agents de longue durée plutôt que sur le chat conversationnel classique. Ce virage reflète une tendance plus large observée chez tous les grands fournisseurs de modèles en 2026 : la bataille des classements génériques cède progressivement la place à une segmentation par cas d’usage, où un modèle « agent » optimisé pour l’exécution d’outils peut coexister avec un modèle « raisonnement » optimisé pour la résolution de problèmes complexes, sans qu’aucun des deux ne cherche à dominer l’autre sur tous les critères à la fois.

## Ce que dit la Commission européenne sur le marquage des contenus IA

Le renforcement des règles européennes intervient à un moment où de plus en plus de contenus circulant en ligne sont générés ou modifiés par des systèmes comme ceux que Nemotron 3.5 Lightning pourrait alimenter. La Commission a été explicite sur ce point : « le contenu généré ou modifié par IA devra également porter des marques lisibles par machine afin d’être détecté plus facilement », selon le communiqué officiel de la Commission européenne. Cette exigence de marquage machine-readable s’applique indépendamment du fait que le modèle générateur soit propriétaire ou publié en open source, ce qui place la responsabilité de conformité sur les entreprises qui déploient le modèle, et non uniquement sur Nvidia en tant que fournisseur du modèle de base.

Une analyse publiée par Euronews souligne que le 2 août 2026 marque le moment où les règles européennes sur les modèles d’IA deviennent effectivement contraignantes, consolidant le rôle de la Commission comme régulateur le plus visible au monde sur cette technologie. Pour les entreprises françaises qui envisagent d’intégrer Nemotron 3.5 Lightning ou tout autre modèle ouvert récent dans leurs produits, la question n’est donc plus seulement technique (quel modèle choisir) mais aussi documentaire : quelle preuve de conformité fournir en cas de contrôle.

## Ce que cela signifie pour les équipes techniques françaises

Concrètement, une équipe française qui évalue Nemotron 3.5 Lightning aujourd’hui doit arbitrer entre plusieurs facteurs. Le coût d’inférence, d’abord, particulièrement attractif à 0,05 dollar par million de tokens en entrée chez DeepInfra, rend le modèle compétitif face à des alternatives européennes comme Mistral Large 3, sans toutefois offrir les mêmes garanties d’hébergement souverain que propose Mistral via ses partenariats avec des clouds européens. La possibilité d’auto-héberger le modèle sous licence OpenMDW-1.1, ensuite, permet de contourner cette limite en gardant les données sur une infrastructure choisie par l’entreprise elle-même, à condition de disposer du matériel Nvidia nécessaire pour faire tourner le modèle efficacement.

Enfin, l’immaturité assumée de NeMo Switchyard, encore en phase pré-alpha selon son propre dépôt GitHub, invite à la prudence pour tout déploiement en production avant la fin de l’année 2026. Les équipes intéressées peuvent commencer à l’expérimenter en environnement de test dès maintenant, tout en conservant une solution de routage plus mature en parallèle pour leurs charges de travail critiques. La fiche technique du modèle, disponible sur le catalogue NGC de Nvidia, reste la source la plus fiable pour suivre les mises à jour de licence et de checkpoints.

## Prédictions : où va la course aux modèles ouverts d’ici fin 2026

- **Nvidia publiera d’autres tailles de la famille Nemotron 3.5** avant la fin de l’année, en suivant la logique déjà observée avec les générations précédentes, qui proposaient plusieurs variantes de taille pour couvrir du edge jusqu’au datacenter.
- **NeMo Switchyard sortira de sa phase pré-alpha** d’ici le premier trimestre 2027, probablement accompagné d’intégrations officielles avec des fournisseurs cloud européens pour répondre à la demande de conformité RGPD.
- **Mistral AI accentuera sa communication sur la souveraineté** face à la montée des modèles ouverts américains optimisés pour du matériel non européen, en misant sur ses partenariats avec OVHcloud et Scaleway plutôt que sur la seule performance brute.
- **Les audits de conformité AI Act cibleront en priorité les déploiements d’agents autonomes** construits sur des modèles ouverts récents, précisément parce que leur adoption rapide laisse peu de temps aux équipes juridiques pour documenter les usages avant mise en production.
- **D’autres fournisseurs de puces suivront la stratégie de Nvidia** en publiant leurs propres modèles ouverts optimisés pour leur matériel, une dynamique déjà amorcée ailleurs dans l’industrie et qui devrait s’accélérer à mesure que la différenciation purement matérielle s’amenuise.

## Questions fréquentes

### Nemotron 3.5 Lightning est-il gratuit ?

Le modèle est publié sous licence OpenMDW-1.1, qui autorise le téléchargement et l’auto-hébergement sans redevance de licence. Seul le coût du matériel ou de l’hébergement cloud reste à la charge de l’utilisateur. Via des fournisseurs comme OpenRouter, un palier gratuit existe également, avec une limite de sortie de 65 536 tokens par appel.

### Quelle est la différence entre Nemotron 3.5 Lightning et les précédents modèles Nemotron ?

La génération 3.5 Lightning introduit une architecture hybride Mamba-2, MoE et attention, avec une fenêtre de contexte étendue à 1 million de tokens et un positionnement explicite sur les agents autonomes de longue durée, plutôt que sur le chat conversationnel généraliste.

### NeMo Switchyard peut-il remplacer un load balancer classique ?

Pas directement. Switchyard est spécialisé dans le routage de requêtes IA entre différents modèles et fournisseurs, avec traduction de formats d’API (OpenAI vers Anthropic notamment), ce qu’un load balancer générique ne fait pas nativement. Le projet reste toutefois en phase pré-alpha et n’est pas recommandé pour la production.

### Nemotron 3.5 Lightning est-il conforme à l’AI Act européen ?

Le modèle en lui-même n’est ni conforme ni non conforme : les obligations de transparence de l’AI Act, comme le marquage des contenus générés par IA, s’appliquent au déploiement final réalisé par l’entreprise qui utilise le modèle, pas au modèle de base publié par Nvidia. La disponibilité des poids ouverts facilite néanmoins l’audit technique du comportement du modèle.

### Quelles entreprises hébergent déjà Nemotron 3.5 Lightning ?

DeepInfra, CoreWeave et OpenRouter proposaient déjà un accès au modèle dans les jours suivant son annonce du 11 août 2026. Le modèle est aussi disponible en téléchargement direct sur le catalogue NGC de Nvidia pour un auto-hébergement complet.

### Faut-il un GPU Nvidia pour faire tourner Nemotron 3.5 Lightning ?

Le modèle est optimisé nativement pour les piles logicielles Nvidia comme TensorRT-LLM, mais reste compatible avec des serveurs d’inférence génériques comme vLLM ou SGLang, qui peuvent fonctionner sur d’autres architectures matérielles, avec toutefois des gains de performance moindres que sur du matériel Nvidia dédié.

### Comment Nemotron 3.5 Lightning se compare-t-il à Mistral Large 3 pour un déploiement en France ?

Mistral Large 3 conserve un avantage sur l’hébergement souverain via des partenaires cloud européens et sur la conformité RGPD documentée nativement. Nemotron 3.5 Lightning offre en échange un coût d’inférence plus bas et une fenêtre de contexte nettement supérieure, ce qui en fait un choix pertinent pour des agents à fort volume d’appels, à condition d’accepter une dépendance à l’infrastructure Nvidia.

### NeMo Switchyard est-il open source ?

Oui, le projet est publié sous licence Apache 2.0 sur GitHub, sous l’organisation NVIDIA-NeMo, avec un code source librement consultable et modifiable.
