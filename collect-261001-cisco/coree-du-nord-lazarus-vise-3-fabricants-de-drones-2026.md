Trois fabricants européens de drones et d’équipements de défense. Une fausse offre d’emploi. Un cheval de Troie baptisé ScoringMathTea. En octobre 2025, les chercheurs de l’éditeur slovaque ESET ont mis au jour une campagne de cyberespionnage menée par le groupe Lazarus, aligné sur la Corée du Nord, contre l’industrie de défense du continent. Neuf mois plus tard, cette opération, surnommée **DreamJob**, reste l’un des cas les mieux documentés de vol technologique étatique visant le secteur des drones militaires en Europe.

Le groupe responsable n’est pas un inconnu. Washington le tient pour responsable de plusieurs milliards de dollars de vols de cryptomonnaies, de l’attaque contre Sony Pictures en 2014 et de la propagation du rançongiciel WannaCry en 2017. Cette fois, sa cible n’est plus une banque ni un studio de cinéma, mais le savoir-faire industriel qui permet de concevoir des drones aujourd’hui déployés en Ukraine. Voici les faits, les chiffres et les conséquences pour l’industrie de défense européenne.

## Que s’est-il passé : l’alerte d’ESET Research sur l’opération DreamJob

Le 23 octobre 2025, ESET Research publie un communiqué détaillant une nouvelle vague de l’opération DreamJob, une série de campagnes attribuées de longue date au groupe Lazarus. Cette fois, la cible est précise : le secteur européen des drones et véhicules aériens sans pilote (UAV). Selon l’éditeur, l’offensive a touché **successivement trois sociétés** situées en Europe centrale et du Sud-Est, actives dans la conception d’équipements militaires.

Peter Kálnai, le chercheur d’ESET à l’origine de la découverte, explique dans le communiqué de presse que l’objectif de la campagne semble viser le vol d’informations techniques sur la conception de drones. Un fichier malveillant analysé par l’équipe mentionnerait même explicitement un drone, ce qui renforce cette hypothèse. La découverte a ensuite été relayée par plusieurs médias spécialisés, dont CSO Online et Security Affairs, confirmant la portée internationale de l’alerte.

## Comment l’attaque a été menée : ingénierie sociale et faux recrutements

L’accès initial ne repose sur aucune faille logicielle. Il repose sur la crédulité humaine. C’est la marque de fabrique de l’opération DreamJob depuis sa première apparition, il y a plusieurs années.

### Le leurre : une offre d’emploi sur mesure

Les attaquants contactent leurs cibles avec une offre d’emploi fictive, présentée comme prestigieuse et financièrement alléchante. La victime reçoit un document leurre, accompagné d’un lecteur PDF trojanisé qui se fait passer pour un outil légitime. Dès l’ouverture, l’infection démarre en arrière-plan, sans que l’utilisateur s’en aperçoive.

### La chaîne d’infection et l’évasion

Pour contourner la détection, Lazarus insère ses charges malveillantes dans des chaînes d’exécution complexes : droppers, chargeurs et téléchargeurs, souvent dissimulés dans des projets open source hébergés sur GitHub. La dernière évolution du groupe, selon ESET, réside dans l’introduction de nouvelles bibliothèques de « DLL proxying » et la sélection de nouveaux projets open source à trojaniser, une méthode qui améliore l’évasion sans pour autant masquer l’identité du groupe aux yeux des chercheurs habitués à sa signature.

## ScoringMathTea : le cheval de Troie au cœur de la campagne

La charge utile principale porte un nom presque anodin : ScoringMathTea. C’est un cheval de Troie d’accès à distance (RAT) qui donne aux attaquants un contrôle quasi total du poste infecté. Une quarantaine de commandes sont disponibles : manipulation de fichiers et de processus, collecte d’informations système, ouverture de connexions réseau, et téléchargement de nouvelles charges depuis un serveur de commande et contrôle.

Ce RAT n’a rien de nouveau. La télémétrie d’ESET permet d’en retracer l’usage sur plusieurs années et plusieurs continents, toujours dans une logique d’espionnage industriel ciblé plutôt que de gain financier immédiat.

| Période | Cible ou leurre | Pays | Secteur | 
|---|---|---|---|
| Octobre 2022 | Documents usurpant l’identité d’Airbus | Portugal, Allemagne | Aérospatial | 
| Janvier 2023 | Entreprise technologique | Inde | Technologie | 
| Mars 2023 | Industriel de défense | Pologne | Défense | 
| Octobre 2023 | Société d’automatisation industrielle | Royaume-Uni | Industrie | 
| Septembre 2025 | Fabricant aérospatial | Italie | Aérospatial | 
| Mars à octobre 2025 | Trois fabricants de drones et de défense | Europe centrale et du Sud-Est | Défense, UAV | 

*Source : télémétrie ESET Research, communiqué du 23 octobre 2025 et analyse technique WeLiveSecurity.*

## Trois entreprises, un objectif : la technologie des drones

ESET n’a pas rendu publics les noms des trois sociétés visées, mais en a précisé le profil : elles produisent des équipements militaires actuellement utilisés en Ukraine dans le cadre de l’aide européenne. L’une d’entre elles fabriquerait au moins deux modèles de drones déployés sur le terrain, et participerait à la chaîne d’approvisionnement de drones monorotors avancés, un type d’appareil que la Corée du Nord développe activement de son côté. Ces trois cibles évoluent dans un marché européen des drones à usage commercial et militaire évalué à 7,58 milliards de dollars en 2025 et qui devrait grimper à 8,52 milliards en 2026, selon le cabinet Mordor Intelligence, une croissance qui explique en partie l’appétit de Pyongyang pour ce savoir-faire.

Ce choix n’a rien d’accidentel. Pyongyang investit massivement dans ses capacités de fabrication de drones militaires et s’appuie largement sur la rétro-ingénierie et le vol de propriété intellectuelle pour rattraper son retard technologique, dans un contexte où le marché européen des drones militaires est lui-même estimé à plus de 6,4 milliards de dollars à l’horizon 2026 par MarketsandMarkets. Les entreprises visées fabriquent justement des matériels que la Corée du Nord produit déjà sur son propre sol, et cherche à améliorer.

## Pourquoi maintenant : la guerre en Ukraine en toile de fond

Le calendrier n’est pas neutre. ESET relève que cette campagne coïncide avec le déploiement de soldats nord-coréens en Russie, dans la région de Koursk, pour soutenir Moscou. Les chercheurs avancent une hypothèse : Lazarus chercherait à collecter du renseignement sur les systèmes d’armement occidentaux employés dans le conflit russo-ukrainien, drones en tête.

Ce lien entre cyberespionnage et effort de guerre n’est pas isolé. D’autres services de renseignement, russes notamment, ont multiplié les opérations contre des cibles européennes liées au soutien militaire à Kiev, comme le montre la campagne attribuée au groupe Turla contre des intérêts français. La différence avec Lazarus tient à la finalité : moins la perturbation immédiate que l’accumulation discrète de savoir-faire industriel.

## Qui est le groupe Lazarus

Actif depuis au moins 2009 selon ESET, voire depuis 2007 selon d’autres estimations, Lazarus (également désigné HIDDEN COBRA) est un groupe de menace persistante avancée (APT) aligné sur les intérêts de la Corée du Nord. Il se distingue par la diversité de ses opérations : espionnage, sabotage et cybercriminalité financière à grande échelle.

La société de cybersécurité Group-IB le décrit ainsi : « **Lazarus Group est une organisation de menace persistante avancée parrainée par un État, attribuée à la République populaire démocratique de Corée (RPDC), active depuis au moins 2007** », selon une analyse publiée par Group-IB.

Pour NCC Group, cette longévité est justement ce qui frappe : « **L’acteur de la menace nord-coréen Lazarus opère depuis plus de dix ans et est à l’origine de cyberincidents tristement célèbres, comme l’attaque contre Sony Pictures en 2014 et la propagation du rançongiciel WannaCry en 2017** », note la société dans une rétrospective consacrée au groupe.

L’opération DreamJob désigne spécifiquement les campagnes Lazarus fondées sur l’ingénierie sociale via de fausses offres d’emploi prestigieuses. Les cibles privilégiées sont l’aérospatiale et la défense, suivies de l’ingénierie, de la technologie, des médias et du divertissement.

## Un historique d’attaques hors norme

La campagne contre les fabricants de drones européens n’est qu’un chapitre récent d’un dossier bien plus vaste. Depuis plus d’une décennie, Lazarus enchaîne des opérations qui ont marqué l’histoire de la cybersécurité, par leur ampleur ou par les sommes détournées.

| Année | Cible | Impact | 
|---|---|---|
| 2014 | Sony Pictures Entertainment | Vol massif de données internes et perturbation opérationnelle | 
| 2016 | Banque centrale du Bangladesh | 951 millions de dollars visés par des ordres SWIFT frauduleux, 81 millions effectivement détournés | 
| 2017 | Rançongiciel WannaCry | Environ 200 000 ordinateurs infectés dans plus de 150 pays | 
| 2022 | Pont blockchain Ronin (Axie Infinity) | 620 millions de dollars de cryptoactifs dérobés | 
| 2025 | Plateforme d’échange Bybit | Environ 1,5 milliard de dollars dérobés (près de 400 000 ETH), le plus important vol de cryptoactifs jamais enregistré sur une plateforme d’échange | 

*Sources : NCC Group, Wikipedia, Europol, analyses spécialisées en traçage de cryptoactifs.*

## 6,75 milliards de dollars : l’ampleur du cybercrime nord-coréen

Les vols de cryptomonnaies ne sont pas de simples faits divers pour Pyongyang. Ils constituent une source de revenus significative pour un régime lourdement sanctionné. Selon une estimation relayée par DL News, les acteurs liés à la Corée du Nord auraient détourné au moins 3,4 milliards de dollars en cryptoactifs depuis 2007. Des analyses plus récentes, citant les données de la société Chainalysis, évoquent un cumul proche de **6,75 milliards de dollars depuis 2016**, dont **2,02 milliards rien que sur l’année 2025**.

Cette manne financière explique en partie pourquoi Washington a choisi de sanctionner directement les structures derrière ces opérations. Le département du Trésor américain a déclaré : « **Les mesures annoncées aujourd’hui identifient les groupes de piratage nord-coréens communément désignés, au sein du secteur privé mondial de la cybersécurité, sous les noms de « Lazarus Group », « Bluenoroff » et « Andariel », comme des agences, instruments ou entités contrôlées par le gouvernement de la Corée du Nord** », dans un communiqué de l’OFAC.

Le Council on Foreign Relations, groupe de réflexion américain, résume la finalité habituelle de ces opérations : « **Cet acteur de la menace cible et compromet principalement des entités en Corée du Sud et des intérêts sud-coréens à des fins d’espionnage, de perturbation et de destruction** », précise son observatoire des opérations cyber. La campagne contre les fabricants de drones européens montre que le champ d’action du groupe dépasse désormais largement la seule péninsule coréenne.

## Quel impact sur l’industrie européenne de la défense ?

L’Union européenne mise gros sur l’autonomie stratégique de sa base industrielle de défense. Le Fonds européen de la défense (FED) dispose d’un budget de 7,953 milliards d’euros pour la période 2021-2027, selon la Commission européenne, une enveloppe destinée à soutenir la recherche et le développement de technologies critiques, dont les systèmes de drones. Ces fonds publics viennent s’ajouter à un marché déjà porteur : le segment militaire des drones en Europe est évalué à 6 473,7 millions de dollars pour 2026 par le cabinet MarketsandMarkets, ce qui donne une idée des enjeux économiques qui pèsent derrière chaque vol de propriété intellectuelle dans ce secteur.

Un vol de propriété intellectuelle chez un sous-traitant, même de taille modeste, peut peser directement sur cette stratégie. Quand un concurrent étatique copie une conception de drone plutôt que de la développer en interne, il économise des années de recherche et des budgets considérables, tout en réduisant l’avantage compétitif de l’entreprise victime. Pour les PME du secteur, souvent moins équipées en cybersécurité que les grands groupes de défense, l’exposition à ce type de campagne représente un risque disproportionné par rapport à leur taille.

Le secteur des drones illustre bien cette asymétrie. Contrairement aux grands programmes d’avions de combat ou de navires, portés par un nombre restreint de géants industriels dotés de services de sécurité étoffés, la chaîne d’approvisionnement des UAV compte une multitude de start-ups et de PME spécialisées dans un composant précis : capteurs, systèmes de guidage, moteurs légers ou logiciels de vol. Ce tissu industriel très fragmenté grossit vite : le marché européen des drones commerciaux, estimé à 5,60 milliards de dollars en 2025, devrait atteindre 6,97 milliards de dollars en 2026, selon Market Data Forecast, ce qui multiplie d’autant le nombre de cibles potentielles pour un acteur comme Lazarus. Chacune de ces entreprises constitue un maillon faible potentiel, et il suffit d’en compromettre une seule pour remonter une partie de la chaîne de conception.

## Lazarus face aux autres groupes APT actifs contre l’Europe

Lazarus n’est pas le seul acteur étatique à cibler des intérêts européens. D’autres groupes, souvent liés à la Russie, opèrent avec des motivations et des méthodes différentes, ce qui complique la tâche des équipes de défense qui doivent composer avec plusieurs profils de menace simultanément.

| Groupe | Origine attribuée | Objectif principal | Technique caractéristique | 
|---|---|---|---|
| Lazarus / HIDDEN COBRA | Corée du Nord | Espionnage industriel, financement du régime | Ingénierie sociale, fausses offres d’emploi (DreamJob) | 
| APT28 / Fancy Bear | Russie (renseignement militaire) | Espionnage politique et militaire | Spearphishing, exploitation de serveurs exposés | 
| Sandworm | Russie (renseignement militaire) | Sabotage d’infrastructures critiques | Malwares destructeurs, ciblage des systèmes industriels | 
| APT29 / Cozy Bear | Russie (renseignement extérieur) | Espionnage diplomatique et gouvernemental | Compromissions de chaîne d’approvisionnement, phishing ciblé | 

Ce qui distingue Lazarus, c’est sa polyvalence. Le groupe mène de front des opérations d’espionnage pur, comme celle visant les drones européens, et des attaques directement lucratives, comme le piratage de Bybit. Peu d’acteurs étatiques cumulent ainsi mission de renseignement et financement autonome du régime qu’ils servent.

## NIS2 : le cadre réglementaire censé protéger les infrastructures critiques

Face à la multiplication de ces campagnes, l’Union européenne s’est dotée d’un outil juridique renforcé. La directive NIS2 est entrée en vigueur le 16 janvier 2023, avec une date limite de transposition dans le droit national fixée au 17 octobre 2024 pour l’ensemble des États membres, selon la Commission européenne.

Le texte élargit considérablement le nombre d’entités soumises à des obligations de sécurité et de notification d’incidents, un périmètre qui inclut désormais de nombreux sous-traitants industriels, y compris dans la défense. Sur le papier, une entreprise victime d’une campagne comme DreamJob devrait aujourd’hui la signaler à son autorité nationale compétente. Reste que la mise en œuvre demeure inégale selon les pays, comme le montre le renvoi de la France et de trois autres pays devant la Cour de justice de l’Union européenne pour retard de transposition.

## Le silence relatif des autorités européennes

Un point frappe à la lecture du dossier public : neuf mois après la révélation d’ESET, aucune agence européenne, ni l’ENISA ni un CERT national, n’a publié d’alerte officielle spécifiquement dédiée à cette campagne contre le secteur des drones. La réaction est venue du secteur privé, pas des institutions. C’est un schéma courant en matière de cyberespionnage industriel : les victimes, souvent des PME, n’ont ni l’obligation ni toujours l’intérêt commercial à communiquer publiquement sur une intrusion qui touche à leur propriété intellectuelle.

Cette discrétion contraste avec la communication beaucoup plus musclée qui entoure généralement les rançongiciels, où la pression d’un compte à rebours et d’une fuite de données publique pousse les victimes à réagir vite. L’espionnage industriel, lui, reste largement invisible du grand public tant qu’un chercheur comme Peter Kálnai ne le documente pas noir sur blanc. Pour les rédacteurs de la directive NIS2, c’est précisément l’angle mort que le texte tente de combler en imposant un devoir de notification, même en l’absence de rançon ou de fuite spectaculaire.

## Comment les entreprises de défense peuvent se protéger

Face à une attaque qui repose avant tout sur l’ingénierie sociale, les correctifs techniques ne suffisent pas. ESET a publié une série d’indicateurs de compromission (IoC) permettant aux équipes de sécurité de vérifier si leurs systèmes ont été exposés à cette campagne.

```
Catégories d'indicateurs de compromission publiés par ESET Research :
- Hachages SHA-1 des fichiers malveillants (leurres, chargeurs, charge utile ScoringMathTea)
- Domaines de commande et contrôle (C2)
- Adresses IP associées à l'infrastructure d'attaque
- Noms des projets open source détournés et hébergés sur GitHub
Référence : blog technique ESET Research, "Gotta fly: Lazarus targets the UAV sector"
```
Au-delà de la détection technique, les recommandations restent classiques mais trop souvent négligées : vérification systématique de l’identité des recruteurs avant tout échange de fichiers, formation régulière du personnel à l’ingénierie sociale, séparation stricte des postes ayant accès aux données sensibles, et surveillance comportementale des postes de travail via des outils de détection et réponse (EDR). Aucune de ces mesures n’est coûteuse à l’échelle d’un groupe de défense. Beaucoup le sont, en revanche, pour une PME sous-traitante qui n’a ni RSSI dédié ni budget de sécurité conséquent.

## Cinq prévisions pour la cybersécurité de la défense européenne

Sur la base des éléments rassemblés par ESET Research et du schéma habituel des campagnes Lazarus, plusieurs évolutions semblent probables dans les prochains mois.

- **Un ciblage prolongé.** Tant que la guerre en Ukraine se poursuit et que Pyongyang approfondit sa coopération militaire avec Moscou, Lazarus devrait continuer à cibler les sous-traitants de défense européens, en particulier dans le secteur des drones.
- **Une extension géographique.** Après l’Europe centrale et du Sud-Est, d’autres marchés producteurs de systèmes UAV pourraient devenir des cibles, en cohérence avec le schéma d’expansion sectorielle habituel de l’opération DreamJob.
- **Davantage de signalements sous NIS2.** L’élargissement du périmètre de la directive devrait faire remonter un nombre croissant de campagnes similaires dans les statistiques officielles des autorités nationales.
- **De nouvelles sanctions.** Si l’attribution à Lazarus s’étend à d’autres victimes confirmées, une nouvelle vague de désignations, à l’image de celles déjà prononcées par l’OFAC, est plausible.
- **Un durcissement des procédures RH.** Les entreprises de défense devraient renforcer la vérification des offres d’emploi non sollicitées, ce type d’attaque reposant presque entièrement sur la crédulité humaine plutôt que sur une faille technique.

## Questions fréquentes

**Qu’est-ce que l’opération DreamJob ?**

Il s’agit d’une série de campagnes de cyberespionnage menées par le groupe Lazarus, qui utilise de fausses offres d’emploi prestigieuses pour infiltrer des organisations, principalement dans les secteurs de l’aérospatiale et de la défense.

**Qui est le groupe Lazarus ?**

Lazarus, également appelé HIDDEN COBRA, est un groupe de menace persistante avancée aligné sur les intérêts de la Corée du Nord, actif depuis au moins 2009 selon ESET (2007 selon Group-IB), connu pour ses opérations d’espionnage, de sabotage et de cybercriminalité financière.

**Quelles entreprises ont été touchées par la campagne visant les drones ?**

ESET Research indique que trois entreprises situées en Europe centrale et du Sud-Est ont été visées successivement entre mars et octobre 2025. Leurs identités n’ont pas été rendues publiques, mais elles fabriquent des équipements militaires, dont des drones, déployés en Ukraine.

**Comment fonctionne le malware ScoringMathTea ?**

C’est un cheval de Troie d’accès à distance capable d’exécuter une quarantaine de commandes : manipulation de fichiers et de processus, collecte d’informations système, ouverture de connexions réseau et téléchargement de nouvelles charges depuis un serveur de commande et contrôle.

**Pourquoi la Corée du Nord cible-t-elle les fabricants de drones ?**

Pyongyang investit massivement dans ses propres capacités de fabrication de drones militaires et s’appuie largement sur la rétro-ingénierie et le vol de propriété intellectuelle pour combler son retard technologique, selon les chercheurs d’ESET.

**Cette campagne est-elle liée à la guerre en Ukraine ?**

ESET souligne que les entreprises visées produisent du matériel militaire utilisé en Ukraine dans le cadre de l’aide européenne, et que la campagne coïncide avec le déploiement de troupes nord-coréennes en Russie, dans la région de Koursk.

**Comment les entreprises peuvent-elles se protéger contre ce type d’attaque ?**

Les recommandations de base restent les plus efficaces : vérification systématique des recruteurs et des offres d’emploi non sollicitées, sensibilisation du personnel à l’ingénierie sociale, détection comportementale sur les postes de travail, et cloisonnement des systèmes sensibles.

**La France et l’Europe occidentale sont-elles concernées ?**

Les victimes identifiées se situent en Europe centrale et du Sud-Est, mais la France dispose d’une base industrielle de défense et de drones significative et reste soumise aux mêmes obligations de vigilance au titre de la directive NIS2. Le schéma d’expansion sectorielle habituel de Lazarus n’exclut pas de futures cibles plus à l’ouest.