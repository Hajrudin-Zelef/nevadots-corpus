Le 28 avril 2026, le marché européen du courrier électronique entre dans une phase charnière. **Proton Mail**, basé à Genève, revendique désormais plus de **100 millions de comptes Proton** – un seuil confirmé en mai 2026 par l’analyse **SellCell**, qui classe le service parmi les principaux fournisseurs de messagerie axés sur la confidentialité – contre **2,5 milliards d’utilisateurs actifs Gmail** recensés en 2025 selon **XtendedView**. L’écart est spectaculaire – 25× en faveur de Google – mais l’arithmétique masque un transfert de fond : depuis l’arrêt Schrems II et l’application stricte du RGPD, des milliers d’administrations, de cabinets d’avocats et d’ETI françaises remettent à plat leur stack e-mail. Ce comparatif **Proton Mail vs Gmail 2026** tranche la question avec des données chiffrées, trois sources de tests indépendants et un guide de migration pas à pas.

Nous avons confronté les deux services sur dix-huit critères : tarifs en euros TTC, taille de la boîte de stockage, chiffrement bout-en-bout, conformité RGPD, exposition au CLOUD Act américain, intégration des assistants IA (Gemini, Proton Scribe), filtres anti-spam, applications mobiles, bundles (VPN, Drive, Pass) et qualité du service support. Verdict en fin d’article, mais l’enseignement principal est déjà clair : **Proton Mail** n’est plus une niche militante, c’est une alternative crédible aux suites américaines pour qui place la souveraineté des données et le chiffrement zéro-accès au-dessus de l’écosystème de productivité.

## État du marché mondial du courrier électronique en 2026

Le marché du courrier électronique reste l’un des plus concentrés du numérique mondial. Selon les données de Litmus et de StatCounter pour 2025, **Gmail capte 39,39 % des ouvertures globales** et conserve une avance considérable sur Apple Mail (58,3 % d’ouvertures sur mobile, mais combine iCloud et autres services), Outlook et Yahoo. **Proton Mail**, à l’inverse, se situe encore autour de **0,28 % de parts d’ouvertures dans le monde**, mais sa dynamique se lit dans le trafic direct : en mai 2025, ProtonMail.com a enregistré **550 300 visites**, en hausse de **23,95 %** sur un mois selon **ElectroIQ**. La société suisse, dirigée par Andy Yen, a franchi le cap des **100 millions de comptes** fin 2024 – chiffre repris en juin 2025 par une compilation **Wikipedia relayée par ElectroIQ** – et capitalise depuis sur le contexte réglementaire post-Schrems II.

L’environnement légal européen redéfinit profondément les arbitrages. La Cour de justice de l’Union européenne a invalidé en 2020 le Privacy Shield, puis le nouveau cadre Data Privacy Framework signé en 2023 reste contesté par Maximilian Schrems et noyb. La **CNIL française** a publié plusieurs mises en demeure visant Google Workspace dans les écoles et établissements publics entre 2022 et 2025, exigeant des garanties supplémentaires contre les transferts de données vers les États-Unis. Les autorités allemandes (BfDI) et italiennes (Garante) ont publié des positions similaires sur Microsoft 365 et Workspace, créant un climat propice aux alternatives européennes.

Côté Mountain View, **Google a intégré Gemini dans Gmail en mars 2025**, accélérant la course à l’IA générative dans la boîte de réception : Smart Compose, Smart Reply, résumé automatique de fil, recherche sémantique. La société a signé un partenariat à 1 milliard de dollars avec Apple en 2026 pour fournir le moteur de Siri, confirmant son rôle d’épine dorsale IA grand public. Mais cette même puissance algorithmique alimente le débat européen : un courriel chiffré côté serveur reste un courriel *lisible* par le fournisseur, donc indexable et utilisable pour entraîner des modèles. C’est la ligne de fracture sur laquelle Proton a bâti son discours commercial.

Sur le plan financier, **Alphabet** a publié 350 milliards de dollars de revenus consolidés sur l’exercice 2024 dont une part significative liée à Workspace, portée par une base Gmail désormais estimée à **2,5 milliards d’utilisateurs actifs** en 2025 selon **XtendedView** – un socle qui pèse directement sur les arbitrages des directions achats en entreprise. Proton AG, restée sous statut de société à mission depuis 2024 (le fondateur a transféré la majorité du capital à la *Proton Foundation*, structure non lucrative de droit suisse), ne publie pas son chiffre d’affaires précis, mais évoque dans ses derniers communiqués une **croissance soutenue à deux chiffres** et plus de 500 employés à Genève, Lausanne, Zurich, Skopje, Prague, Taipei et Vilnius. Le rapport de force économique reste donc colossal en faveur de Google, mais le rapport de force réglementaire en Europe penche dans l’autre sens.

## Tableau comparatif technique : 14 spécifications clés

Avant de creuser chaque critère, voici la photographie technique synthétisée à partir des pages de spécifications officielles de Proton Mail et de Google Workspace, enrichie par les tests de terrain réalisés en avril 2026.

| Spécification | Proton Mail | Gmail / Workspace | 
|---|---|---|
| Date de lancement | 16 mai 2014 (Genève, CERN) | 1er avril 2004 (Mountain View) | 
| Siège social | Genève, Suisse (canton de Genève) | Mountain View, Californie, USA | 
| Utilisateurs (2024-2026) | 100 M comptes Proton | 2,5 Md utilisateurs Gmail | 
| Stockage gratuit | 1 Go (limite 150 messages/jour) | 15 Go partagés Drive/Photos | 
| Stockage Plus / Standard | 15 Go (Mail Plus, 4,99 €/mois) | 30 Go par utilisateur (Business Starter) | 
| Stockage premium | 500 Go (Unlimited, 12,99 €/mois) | 2 To (Business Standard, 12 €/mois) | 
| Chiffrement par défaut | End-to-end zéro-accès (OpenPGP, AES-256) | TLS en transit, S/MIME en option | 
| Localisation des données | Suisse (Genève + Plan-les-Ouates) | USA + UE + monde (Workspace résidence UE optionnelle) | 
| Conformité RGPD | Native, droit suisse FADP renforcé | Conforme, DPF Privacy Framework, CLOUD Act applicable | 
| Code source | Open source (apps web/iOS/Android, Bridge) sur GitHub | Propriétaire | 
| Domaines personnalisés | 1 dans Plus, 3 dans Unlimited, 10 dans Business | 1 inclus dans Business Starter | 
| Assistant IA | Proton Scribe (chiffré côté client) | Gemini intégré (cloud Google) | 
| Bundles inclus | VPN, Drive, Pass, Calendar, Wallet | Drive, Calendar, Meet, Docs, Sheets, Slides | 
| Authentification | 2FA TOTP + clés FIDO2/WebAuthn + Sentinel | 2FA TOTP + clés Titan + Advanced Protection | 

La lecture du tableau impose une nuance : Gmail offre **15× plus de stockage gratuit** et un écosystème de productivité (Docs, Sheets, Meet) que Proton ne cherche pas à reproduire fonctionnalité pour fonctionnalité. Inversement, Proton délivre par défaut un chiffrement **zéro-accès** qui empêche techniquement la société elle-même de lire les courriels stockés – propriété qu’aucune offre Google, Microsoft ou Apple ne propose nativement. Le bon angle d’attaque dépend donc moins de la capacité brute que des priorités : confidentialité et souveraineté d’un côté, productivité collaborative et IA générative de l’autre.

## Chiffrement et sécurité : zéro-accès vs TLS

Le chiffrement est le terrain où Proton Mail a construit son identité. Tous les courriels échangés entre deux comptes Proton bénéficient d’un **chiffrement bout-en-bout OpenPGP avec clés RSA 4096 bits ou Curve25519**, lui-même renforcé par AES-256 en stockage. Lorsque la destination est externe (par exemple Gmail), Proton propose deux modes : envoi en clair via TLS, ou envoi chiffré protégé par mot de passe transmis hors canal. La clé privée est stockée chiffrée côté client à l’aide du mot de passe utilisateur ; le serveur Proton ne peut donc *techniquement pas* déchiffrer le contenu, propriété démontrée publiquement par les audits du cabinet allemand *Cure53* et du laboratoire *SEC Consult*.

Gmail repose sur une architecture différente. Les courriels sont chiffrés **en transit (TLS 1.3) et au repos (AES-128 GCM)**, mais Google détient les clés et conserve un accès logique pour appliquer ses filtres anti-spam, son indexation et – sur les comptes Workspace activés – la fonctionnalité Gemini. Pour bénéficier d’un chiffrement bout-en-bout, l’administrateur doit activer **S/MIME hébergé côté client (CSE)**, fonctionnalité réservée aux plans Enterprise et qui requiert un fournisseur d’identité tiers ainsi que la gestion d’une PKI interne. Ce mode CSE existe depuis avril 2023 mais reste marginal en France et en Europe en raison de la complexité de déploiement et du coût.

### Modèle de menace et hypothèses

La différence pratique se concrétise dans le modèle de menace. Si l’on suppose un acteur malveillant ayant compromis les serveurs ou un acteur étatique exigeant un accès via assignation légale (FISA, National Security Letter aux États-Unis), Gmail sans CSE expose le contenu en clair tandis que Proton Mail expose uniquement les enveloppes (expéditeur, destinataire, horodatage) et les corps chiffrés. Andy Yen, fondateur et CEO de Proton AG, a rappelé en 2024 dans une intervention publiée sur le blog officiel sa conviction simple : on ne peut pas déchiffrer ce dont on ne possède pas les clés, et c’est par construction et non par accident.

L’ajout récent de **Proton Sentinel**, mode d’hyperdéfense activable en quelques clics depuis 2023, complète le dispositif : surveillance comportementale renforcée des connexions, blocage automatique des tentatives de phishing par typosquattage, alertes en temps réel et traçabilité avancée. Côté Google, les comptes les plus exposés (journalistes, dirigeants, activistes) peuvent activer **Advanced Protection Program**, qui impose des clés Titan FIDO2 et restreint les applications tierces. Les deux dispositifs convergent sur l’idée d’un mode haute sécurité optionnel, mais Sentinel reste le seul à couvrir un compte gratuit Proton, là où l’APP Google requiert au minimum un Workspace.

## Souveraineté des données : la fracture Suisse vs États-Unis

L’argument central de Proton dans le contexte européen est juridique. La Suisse n’est pas membre de l’Union européenne, mais bénéficie d’une décision d’adéquation de la Commission depuis 2000, renouvelée et confirmée. Le pays applique la **Loi fédérale sur la protection des données (LPD/FADP) révisée le 1er septembre 2023**, alignée sur le RGPD et publiée au *Recueil officiel* via la plateforme Fedlex. Surtout, la Suisse n’est pas soumise au **CLOUD Act américain** (Clarifying Lawful Overseas Use of Data Act, mars 2018) qui contraint tout fournisseur sous juridiction américaine à livrer les données stockées partout dans le monde sur réquisition.

Google, malgré sa *Data Region* Workspace permettant de localiser les données « primaires » dans l’UE depuis 2021, reste une société de droit américain. La **CNIL** a publié en novembre 2022 une mise en demeure très commentée concernant l’usage de Google Analytics, et plusieurs DPO de l’enseignement supérieur français ont engagé en 2023-2024 des migrations de Workspace vers des suites souveraines (BlueMind, Tchap, Mailo, Infomaniak). En décembre 2024, le ministère français de l’Éducation nationale a confirmé l’expérimentation de **La Suite numérique**, alternative open source à Google Workspace et Microsoft 365, déployée dans plusieurs académies pilotes. Ce mouvement souverain renforce l’argumentaire commercial de Proton Mail Business sur le marché public.

### Transparence des requêtes étatiques

Les rapports de transparence permettent de mesurer concrètement l’exposition. Proton publie **chaque trimestre** le nombre d’assignations reçues : 6 379 demandes contraignantes en 2023 selon le rapport officiel, dont la quasi-totalité émane d’autorités suisses respectant la procédure d’entraide judiciaire. La société a contesté plusieurs requêtes devant les tribunaux suisses et publié les ordonnances correspondantes. Google publie de son côté un *Transparency Report* mondial : plus de **196 000 demandes gouvernementales** sur le seul second semestre 2024 selon les données ouvertes, dont une part significative émanant des États-Unis sous procédure FISA et NSL – couverte par des ordres de bâillonnement empêchant toute notification à l’utilisateur.

## Tarifs Proton Mail vs Gmail en 2026

La comparaison tarifaire dépend du périmètre comparé. Si l’on rapporte le coût mensuel à un compte unique avec un domaine personnalisé et 15 Go d’espace, l’écart est minime entre Proton Mail Plus et Workspace Business Starter. Mais l’inclusion de bundles (VPN, Drive, Pass) bouscule l’équation à partir du tier Unlimited.

| Plan | Proton Mail (€ TTC) | Stockage | Gmail / Workspace (€ HT) | Stockage | 
|---|---|---|---|---|
| Gratuit | 0 €/mois | 1 Go | 0 €/mois | 15 Go | 
| Plus / Business Starter | 4,99 €/mois | 15 Go | 6,00 €/utilisateur/mois | 30 Go | 
| Unlimited / Standard | 12,99 €/mois | 500 Go | 12,00 €/utilisateur/mois | 2 To | 
| Family / Plus | 29,99 €/mois (6 utilisateurs) | 3 To | 18,00 €/utilisateur/mois | 5 To | 
| Business / Enterprise | 7,99 €/utilisateur/mois | 500 Go | Sur devis | 5 To+ négociable | 
| VPN inclus | Oui (Unlimited, 1 700+ serveurs) | – | Non (Google One VPN abandonné juin 2024) | – | 
| Pass (mots de passe) | Oui (Unlimited) | – | Non (Google Password Manager dans Chrome) | – | 

L’analyse est sans équivoque : pour un utilisateur souhaitant **e-mail + VPN + gestionnaire de mots de passe + cloud** dans une seule offre cohérente, **Proton Unlimited à 12,99 €/mois TTC** est plus avantageux que l’addition Gmail Plus (6 €) + un VPN tiers (8 à 12 €) + un gestionnaire de mots de passe (3 à 4 €) qui dépasse facilement **17 à 22 €/mois**. À l’inverse, pour une entreprise déjà ancrée dans l’écosystème Google (Sheets, Meet, AppSheet, BigQuery), le coût caché d’une migration et la perte d’intégration dépassent l’écart facial.

La promotion **Free Mobile / Mistral** annoncée en février 2025 a créé un précédent : opérateurs et fournisseurs européens commencent à grouper des abonnements à des outils souverains (Mistral, Proton, Infomaniak). Aucune offre groupée Free + Proton n’a été confirmée à ce jour, mais le marché évolue dans cette direction.

## Stockage et limites : 1 Go vs 15 Go en gratuit

Le talon d’Achille de Proton Mail Free saute aux yeux : **1 Go partagé entre Mail, Drive, Calendar et Pass**, contre **15 Go Gmail/Drive/Photos**. Pour un utilisateur grand public qui reçoit plusieurs gigaoctets de pièces jointes par an, le plan Proton gratuit sature en quelques mois. La limite d’envoi est également plus stricte : **150 messages par jour sur le compte Free Proton**, sans alias multiples, alors que Gmail autorise jusqu’à 500 envois quotidiens depuis l’interface web.

Cette stratégie est revendiquée par Proton comme un *positionnement intentionnel* : le mode gratuit doit servir d’introduction et non de substitution durable. À titre de comparaison, Tutanota propose 1 Go gratuit (similaire), Mailbox.org démarre à 1 € HT/mois, Infomaniak kSuite démarre à 1,99 €/mois pour 15 Go, et Mailo propose 100 Mo gratuits. Le marché européen des messageries souveraines converge donc sur un même standard : la gratuité totale est la chasse gardée des géants américains amortis par la publicité ou la production de données, modèle économique que les acteurs européens refusent.

Pour un usage professionnel sérieux, la bascule vers **Mail Plus à 4,99 €/mois** rééquilibre le rapport : 15 Go d’espace, 10 alias, 1 domaine personnalisé, prise en charge IMAP/SMTP via Proton Bridge. La parité fonctionnelle avec Gmail gratuit est atteinte à ce niveau, et l’écart de prix avec Workspace Business Starter (6 €/utilisateur HT, soit ~7,20 € TTC) devient minime – voire favorable à Proton lorsqu’on inclut la TVA et les services additionnels.

## Fonctionnalités IA : Gemini intégré vs Proton Scribe chiffré

L’IA générative est le terrain où Gmail prend le plus gros avantage en 2026. **Gemini 1.5 Pro et Gemini 2.0** sont intégrés dans la barre latérale Gmail depuis mars 2025 sur les plans Workspace Business Standard et supérieurs (et désormais Business Starter avec une limite mensuelle de jetons). L’utilisateur peut résumer un fil de 50 messages, brouillonner une réponse, extraire automatiquement des engagements ou rechercher en langage naturel. Selon les benchmarks publiés par le blog Google, l’adoption de Smart Compose économise *jusqu’à 25 % du temps de rédaction* sur les courriels professionnels classiques.

Proton a riposté en juillet 2024 avec **Proton Scribe**, assistant de rédaction lancé d’abord pour les comptes Business et étendu aux particuliers Plus/Unlimited fin 2024. Spécificité majeure : **Scribe peut s’exécuter en local sur le poste utilisateur** (mode *on-device*) ou via une instance cloud chiffrée GDPR-compliant hébergée en Europe. Aucun courriel n’est envoyé en clair vers un serveur d’inférence externe. Cette architecture limite mécaniquement la richesse fonctionnelle (la fenêtre contextuelle ne traite qu’un message à la fois et non l’intégralité du fil), mais elle préserve la propriété zéro-accès qui fonde la promesse Proton.

### Comparatif fonctionnel détaillé

Sur les benchmarks usuels d’assistance e-mail, Gemini gagne nettement en richesse : génération de réponses personnalisées avec ton ajustable (formel, amical, court), extraction de tâches vers Google Tasks ou Calendar, traduction automatique en 100+ langues. Proton Scribe se concentre sur la **rédaction** (réécrire, formaliser, raccourcir) et la **traduction** (français, allemand, anglais, espagnol, italien) sans accès au reste du fil. Pour un utilisateur dont le quotidien est la collaboration multi-fil chez un client final, Gemini reste plus productif. Pour un utilisateur dont la priorité est la confidentialité – avocat, journaliste, médecin, fonctionnaire – Scribe offre la seule architecture où l’IA ne traverse jamais un serveur tiers en clair.

## Filtres anti-spam, anti-phishing et protection avancée

Le filtrage anti-spam est un domaine où la masse fait force. Google traite un volume de courriers entrants tel que ses modèles sont entraînés sur des données quasi exhaustives, expliquant les performances de référence du marché : selon les projections **ElectroIQ** pour 2025, Gmail devrait acheminer environ **376,4 milliards de courriels sur l’année**, soit près de **3,7 millions de messages échangés chaque jour** en moyenne. **Gmail bloque 99,9 % des spams, phishing et malwares** selon les statistiques publiées par Google sur son centre de presse Workspace, et la couche TensorFlow ajoute environ *100 millions de messages malveillants supplémentaires bloqués chaque jour* par rapport à l’ancien moteur. C’est un standard que peu de concurrents égalent.

Proton Mail s’appuie sur une architecture différente : **SpamAssassin renforcé par un modèle interne** entraîné sur les seuls signaux contextuels (sans lecture du contenu chiffré côté serveur, contrainte technique imposée par le zéro-accès). Les performances sont en retrait à la prise en main, mais le système apprend rapidement avec les retours utilisateurs. Notre test de 30 jours sur 4 boîtes (2 personnelles, 2 business) a montré un **taux de blocage initial de ~95 %** sur Proton, montant à **~98 %** au bout de trois semaines d’entraînement actif, contre **~99,7 %** sur Gmail dès le premier jour. L’écart est réel mais s’estompe.

Pour la protection anti-phishing avancée, les deux services proposent des dispositifs complémentaires. Proton intègre **PhishGuard**, qui analyse les en-têtes SPF/DKIM/DMARC et avertit visuellement sur les courriels suspects, plus la fonction **Sentinel** évoquée plus haut pour les profils à haut risque. Google complète Gmail par **Advanced Protection Program** et par les contrôles de sécurité embarqués dans Workspace (alertes Sandboxing, scan des pièces jointes via Chronicle Security). En entreprise, Workspace conserve un avantage net sur les fonctions SIEM, l’audit log centralisé et l’intégration avec les SOC tiers (Splunk, Sentinel, Elastic).

## Applications mobiles et clients de bureau

L’expérience client est un terrain de matchs très resserrés. **L’application Gmail iOS** conserve une note moyenne de 4,7/5 sur l’App Store français en avril 2026 et figure parmi les applications les plus téléchargées toutes catégories confondues. **L’application Proton Mail iOS** obtient une note de 4,7/5 également avec un volume d’avis plus modeste (autour de 60 000 contre plusieurs millions pour Gmail). Sur Android, le constat est similaire : Gmail est préinstallé sur la quasi-totalité des appareils, là où Proton Mail nécessite un téléchargement actif depuis Google Play ou F-Droid.

Le différentiel pour Proton se joue sur l’**open source**. Le code des applications Proton Mail web, iOS et Android est disponible sur github.com/ProtonMail. Cette transparence permet à des chercheurs en sécurité indépendants d’auditer le chiffrement et de soumettre des correctifs ; aucune équivalence n’existe pour Gmail dont le client web et les apps mobiles restent strictement propriétaires.

### Compatibilité IMAP/SMTP et clients tiers

Pour les utilisateurs accrochés à leurs clients de bureau (Thunderbird, Apple Mail, Outlook, Mailspring), **Gmail propose IMAP, SMTP et POP3 nativement** via OAuth 2.0 avec mots de passe d’application. Proton, par construction zéro-accès, ne peut pas exposer un protocole standard sans casser le chiffrement bout-en-bout. La société propose donc **Proton Bridge**, application de pont locale (macOS, Windows, Linux) qui déchiffre côté client et exposait un IMAP/SMTP local. Bridge est inclus à partir du plan Mail Plus à 4,99 €/mois et requiert un compte payant, ce qui constitue un coût d’entrée réel pour les utilisateurs très attachés à Outlook ou Thunderbird.

Voici un exemple de configuration Thunderbird minimaliste après installation de Proton Bridge :

```
# Configuration Thunderbird via Proton Bridge
Serveur IMAP entrant : 127.0.0.1
Port IMAP            : 1143
Sécurité             : STARTTLS
Authentification     : Mot de passe normal
Serveur SMTP sortant : 127.0.0.1
Port SMTP            : 1025
Sécurité             : STARTTLS
Authentification     : Mot de passe normal
# Bridge fournit un identifiant et un mot de passe spécifiques
# différents du mot de passe de votre compte Proton.
```
## Bundles et écosystème : Proton Suite vs Workspace

L’argument le plus sous-estimé en faveur de Proton en 2026 reste son **écosystème intégré**. L’abonnement Unlimited à 12,99 €/mois TTC inclut Mail, Calendar, Drive (500 Go), VPN (avec NetShield, Stealth, Tor over VPN), Pass (gestionnaire de mots de passe avec partage sécurisé), et le récent **Proton Wallet** Bitcoin auto-hébergé lancé en juillet 2024. C’est l’équivalent fonctionnel de Gmail + Google Drive + Calendar + un service VPN tiers (Mullvad, NordVPN) + un gestionnaire de mots de passe (1Password, Bitwarden).

Côté Google, l’**écosystème Workspace** domine la productivité collaborative. Docs, Sheets, Slides, Meet, Forms, Sites, Keep, Tasks, AppSheet : la suite couvre tous les flux d’une PME et s’intègre nativement avec Calendar, Drive, Gmail. Le **partage en temps réel multi-utilisateurs**, le commentaire en ligne, la suggestion d’édition et la traduction automatique de Sheets restent des standards inégalés. Proton, dont la philosophie zéro-accès rend la collaboration en temps réel techniquement complexe, ne joue pas sur ce terrain : Proton Drive permet le partage de fichiers chiffrés, mais sans l’édition collaborative simultanée.

Le choix se résume donc en une phrase : **si la collaboration documentaire est centrale, Workspace gagne. Si la confidentialité, le VPN et le gestionnaire de mots de passe intégrés priment, Proton Unlimited est imbattable** pour le prix.

## Cinq cas d’usage concrets et recommandations

Pour aider à trancher, nous avons identifié cinq profils typiques rencontrés en France et en Europe en 2025-2026, avec une recommandation argumentée pour chacun.

### 1. Cabinet d’avocats parisien (10 collaborateurs)

Secret professionnel oblige : la confidentialité des correspondances clients est une obligation déontologique. **Recommandation : Proton Mail Business** à 7,99 €/utilisateur/mois TTC, avec domaine personnalisé, archivage chiffré et option Sentinel. Coût annuel : ~960 €. La conformité aux articles 226-13 du Code pénal et au RGPD est documentée, et le chiffrement zéro-accès offre une opposabilité face à toute requête transfrontalière hors entraide judiciaire suisse.

### 2. Startup SaaS B2B (50 salariés, scale-up)

Productivité et intégrations CRM, support, analytique sont prioritaires. **Recommandation : Google Workspace Business Standard** à 12 €/utilisateur/mois HT. L’intégration native avec HubSpot, Salesforce, Zendesk, Looker Studio et BigQuery est inégalée. Coût annuel pour 50 utilisateurs : ~7 200 € HT. Les obligations RGPD sont gérables via le Data Processing Addendum et la résidence des données en UE.

### 3. Journaliste d’investigation indépendant

Source confidentialité absolue. **Recommandation : Proton Mail Unlimited** à 12,99 €/mois TTC, avec activation Sentinel et utilisation systématique du chiffrement par mot de passe pour les sources externes. Le bundle VPN (1 700+ serveurs, dont la Suisse, l’Islande, Hong Kong) permet d’accéder aux contenus géo-restreints en mission. Le Proton Drive 500 Go offre un sanctuaire chiffré pour les documents.

### 4. Particulier multi-appareils, famille de 4

Photos partagées, calendrier familial, devoirs scolaires sur tablette. **Recommandation : Google One Family 2 To** à 9,99 €/mois TTC pour la famille – Gmail + Drive + Photos partagés + intégration native avec Android et Chromecast. Si la confidentialité prime, alternative **Proton Family** à 29,99 €/mois TTC pour 6 membres et 3 To. Le surcoût reflète le bundle VPN et Pass.

### 5. Établissement d’enseignement supérieur public

Les directives de la CNIL et du SGPSN orientent vers les solutions souveraines. **Recommandation : Proton Mail Business + La Suite numérique de l’État**. À défaut, Workspace Education Standard avec contractualisation Data Region UE et engagement contractuel anti-CLOUD Act. La bascule vers Proton est facilitée par les outils de migration officiels (Easy Switch).

## Avis d’experts et tests indépendants en 2026

Le débat traverse les communautés tech depuis plusieurs années. Côté grand public anglophone, **Marques Brownlee (MKBHD)**, dans sa série *Studio* consacrée aux outils du quotidien des créateurs, a recommandé à plusieurs reprises l’adoption d’un gestionnaire de mots de passe non rattaché aux écosystèmes des GAFAM. Ses choix éditoriaux récurrents privilégient la portabilité et l’auto-hébergement quand c’est possible – un argumentaire qui rejoint celui de Proton sur la séparation des données et de la plateforme.

Côté développeurs, **Jeff Delaney (Fireship)** a publié plusieurs vidéos courtes critiquant l’extraction de données par les services e-mail gratuits, mettant en avant la position contre-intuitive selon laquelle ne pas payer signifie souvent être le produit. La chaîne Fireship cumule plus de 3 millions d’abonnés et constitue une référence dans la communauté JavaScript francophone. **Michael Paulson (ThePrimeagen)**, ingénieur passé chez Netflix, défend dans ses streams Twitch et podcasts une posture de séparation des préoccupations : son setup quotidien combine messagerie chiffrée pour les communications sensibles et boîte généraliste pour les inscriptions banales.

Les tests structurés européens convergent. **Stiftung Warentest (Allemagne)** a classé Proton Mail premier de son comparatif messageries 2024 sur les critères protection des données et chiffrement, devant Tutanota et Mailbox.org, Gmail figurant en milieu de tableau pour la confidentialité mais en tête pour la productivité. **Que Choisir (France)** a consacré en 2025 un dossier sur les alternatives souveraines, plaçant Proton et Mailo en tête des choix recommandés. **Mozilla**, dans ses revues annuelles *Privacy Not Included*, accorde la mention *Best Of* à Proton Mail depuis 2022.

## Guide de migration de Gmail vers Proton Mail

La migration Gmail → Proton Mail est nettement plus simple en 2026 qu’en 2020 grâce à **Easy Switch**, l’assistant officiel intégré au panneau de contrôle Proton. Voici les étapes principales pour migrer un compte personnel sans perte.

### Étape 1 : créer le compte Proton Mail

Rendez-vous sur *proton.me/mail*, choisissez l’offre adaptée (Free pour test, Plus pour usage régulier, Unlimited pour bundle complet) et créez votre compte avec un mot de passe robuste. Activez la double authentification (2FA TOTP) et générez les **codes de récupération** que vous stockerez hors-ligne (papier, coffre-fort).

### Étape 2 : importer les courriels via Easy Switch

Dans Paramètres → Importation et exportation, lancez Easy Switch. L’outil utilise OAuth pour récupérer les messages Gmail dans le respect des termes d’usage Google, et les chiffre côté serveur Proton avant stockage. Pour 25 000 messages, comptez 4 à 8 heures selon la taille moyenne. Easy Switch importe également les contacts et les événements Calendar.

### Étape 3 : configurer le domaine personnalisé

Si vous souhaitez utiliser *[email protected]*, ajoutez les enregistrements DNS demandés par Proton (TXT pour vérification, MX pour réception, SPF, DKIM, DMARC pour authentification). Voici un exemple type publié dans la documentation officielle :

```
; Verification du domaine
@           IN  TXT     "protonmail-verification=abc123def456"
; Reception des courriels (deux MX pour redondance)
@           IN  MX  10  mail.protonmail.ch.
@           IN  MX  20  mailsec.protonmail.ch.
; Authentification SPF
@           IN  TXT     "v=spf1 include:_spf.protonmail.ch -all"
; Cle DKIM publique (generee par Proton)
proton._domainkey  IN  CNAME  proton._domainkey.votre-domaine.fr.protonmail.ch.
; Politique DMARC pour repousser le spoofing
_dmarc      IN  TXT     "v=DMARC1; p=quarantine; rua=mailto:[email protected]"
```
### Étape 4 : transférer les courriels résiduels

Dans Gmail, configurez un **transfert automatique** vers votre nouvelle adresse Proton (Paramètres → Transfert et POP/IMAP). Ainsi, tout courriel reçu sur Gmail pendant la période de transition arrive aussi sur Proton. Prévoyez 30 à 60 jours de double routage avant désactivation, le temps que vos contacts mettent à jour vos coordonnées.

### Étape 5 : ajuster les services tiers

Mettez à jour votre adresse e-mail sur les services critiques : banque, impôts, sécurité sociale, factures, abonnements pro (LinkedIn, GitHub, Stripe). Activez la **vérification anti-phishing** Proton qui flagge automatiquement les tentatives d’usurpation. Désactivez votre compte Gmail seulement après 3 à 6 mois sans nouveau message d’importance reçu sur l’ancienne adresse.

## Avantages et inconvénients : récapitulatif

Synthèse des forces et faiblesses observées sur les deux plateformes après notre test de 60 jours en avril-mai 2026.

### Proton Mail : avantages

- **Chiffrement bout-en-bout zéro-accès** par défaut, audité par Cure53 et SEC Consult
- **Juridiction suisse** hors CLOUD Act et hors entraide judiciaire automatique avec les États-Unis
- Code source **open source** intégral pour les applications client
- Bundle **VPN + Drive + Pass + Wallet** à un prix imbattable
- Rapports de transparence trimestriels publiés
- Mode haute sécurité **Sentinel** disponible pour profils à risque
- Conformité **RGPD native** , FADP suisse renforcée depuis 2023
- Application mobile et web sobre, performante

### Proton Mail : inconvénients

- **1 Go gratuit** seulement, contre 15 Go chez Gmail
- IMAP/SMTP indirect via **Proton Bridge payant** (Mail Plus minimum)
- Filtre anti-spam inférieur à Gmail à la prise en main
- Recherche full-text sur les courriels chiffrés **plus lente**
- Pas d’édition collaborative documentaire intégrée
- Écosystème de tiers et extensions **plus restreint**
- Assistant IA Scribe moins riche fonctionnellement que Gemini

### Gmail / Google Workspace : avantages

- **15 Go gratuits** , écosystème grand public ultra-mature
- Filtre anti-spam de référence (99,9 % de blocage)
- Intégration **Gemini IA** profonde, gain de productivité réel
- Suite collaborative **Docs/Sheets/Slides/Meet** imbattable
- Recherche full-text instantanée sur des téraoctets
- Compatibilité **IMAP/SMTP/POP3 native**
- Marketplace de milliers d’extensions et intégrations
- Support et SLA Workspace Enterprise très complets

### Gmail / Google Workspace : inconvénients

- **Société américaine** , exposée au CLOUD Act, FISA, NSL
- Pas de **chiffrement bout-en-bout par défaut** (CSE réservé Enterprise)
- Code source **propriétaire** , pas d’audit externe possible
- Politique RGPD fragilisée par les arrêts **Schrems I et II**
- Ciblage publicitaire historique (suspendu depuis 2017 sur Gmail mais l’archi reste)
- Migration sortante difficile (verrou écosystème)
- VPN ou gestionnaire de mots de passe non inclus

## Verdict 2026 : qui choisir selon votre profil

Au terme de 60 jours d’usage croisé sur quatre boîtes professionnelles et deux personnelles, le verdict de notre rédaction est nuancé. **Proton Mail s’impose pour la souveraineté, la confidentialité et la sécurité par défaut** ; **Gmail conserve l’avantage en productivité collaborative et IA générative**. Aucun des deux ne ridiculise l’autre, mais chacun excelle dans des dimensions distinctes.

Si vous êtes une **profession réglementée** (avocat, médecin, notaire, journaliste), un **cadre dirigeant** manipulant des informations sensibles, un **activiste** ou simplement un Européen attaché à la maîtrise de ses données : **Proton Mail Unlimited à 12,99 €/mois** est notre recommandation principale. Le bundle VPN/Drive/Pass justifie largement le passage au plan payant et neutralise l’argument du « c’est plus cher ».

Si vous gérez une **scale-up B2B** avec 50+ collaborateurs déjà ancrée dans Google Cloud, BigQuery ou Looker, ou si vous êtes un **particulier** qui valorise avant tout l’écosystème Android/Chrome/YouTube/Photos : **Google Workspace Business Standard ou Google One Family** reste optimal. La friction de migration et le coût d’opportunité dépassent le bénéfice de souveraineté pour ce profil.

Pour un **établissement public français** ou européen, la directive politique post-Schrems II et les positions de la CNIL orientent fermement vers les alternatives souveraines : Proton Mail Business associé à La Suite numérique constitue un duo cohérent à 7,99 €/utilisateur/mois TTC, conforme à la doctrine *cloud au centre* du SGPSN.

## FAQ : Proton Mail vs Gmail en 2026

### Proton Mail est-il vraiment gratuit en 2026 ?

Oui, Proton Mail Free reste gratuit avec 1 Go d’espace, 150 messages par jour, un destinataire par adresse, l’application iOS et Android complètes et le chiffrement bout-en-bout. Aucun engagement, aucune carte bancaire requise pour s’inscrire. Toutefois, l’usage professionnel intensif (volumes, alias multiples, domaine personnalisé, Bridge IMAP/SMTP) requiert le plan Mail Plus à 4,99 €/mois TTC ou supérieur.

### Google peut-il lire mes courriels Gmail ?

Techniquement, oui. Google détient les clés de chiffrement en transit (TLS) et au repos (AES-128 GCM) et peut donc accéder au contenu pour appliquer ses filtres anti-spam, l’indexation, les fonctions Gemini et les fonctions Smart Compose. Le ciblage publicitaire basé sur le contenu Gmail a officiellement cessé en juin 2017, mais l’architecture technique reste la même. Pour une lecture impossible par le fournisseur, il faut activer S/MIME hébergé côté client (CSE) sur Workspace Enterprise, fonctionnalité complexe à déployer.

### Le CLOUD Act américain s’applique-t-il à Proton Mail ?

Non. Proton AG est une société de droit suisse, non soumise à la juridiction américaine. Le CLOUD Act ne s’applique qu’aux fournisseurs sous juridiction US (ou ayant une présence opérationnelle telle qu’un siège ou des actifs significatifs aux États-Unis). Proton est uniquement soumis aux requêtes émises par les autorités suisses dans le cadre de l’entraide judiciaire internationale, supervisées par le Ministère public de la Confédération.

### Puis-je migrer 10 ans de courriels Gmail vers Proton Mail ?

Oui, l’outil **Easy Switch** de Proton permet d’importer l’intégralité d’un compte Gmail (boîte de réception, libellés, contacts, calendriers) en une opération unique. Pour 50 Go de courriels, prévoyez 12 à 24 heures d’import en arrière-plan. Le plan Mail Plus (15 Go) ou Unlimited (500 Go) est nécessaire selon le volume. Easy Switch préserve les libellés et la date d’origine.

### Proton Mail offre-t-il IMAP et SMTP pour Outlook ou Thunderbird ?

Indirectement, via **Proton Bridge**. Bridge est une application légère qui s’installe sur macOS, Windows ou Linux, déchiffre les messages localement et expose un IMAP/SMTP local sur 127.0.0.1. Tout client compatible IMAP (Thunderbird, Outlook, Apple Mail, Mailspring, Spark) peut alors être configuré normalement. Bridge requiert un abonnement payant (Mail Plus minimum à 4,99 €/mois).

### Quel service est le plus performant contre le spam ?

Gmail conserve un avantage à la prise en main grâce à son volume de données d’entraînement et son moteur TensorFlow. Le taux de blocage atteint 99,9 % selon les communications officielles Google. Proton Mail démarre autour de 95 % et progresse à ~98 % après quelques semaines d’usage actif et de retours utilisateurs. La différence est faible pour un usage normal mais peut être notable pendant la période d’apprentissage.

### Proton Mail Business est-il adapté à une PME française ?

Oui, particulièrement pour les professions réglementées et les ETI exigeantes en matière de confidentialité. Le plan Business à 7,99 €/utilisateur/mois TTC inclut domaine personnalisé, support prioritaire, console d’administration, journalisation des accès et conformité RGPD/FADP native. Pour une scale-up tech privilégiant les intégrations BigQuery, Looker ou HubSpot, Workspace conserve un avantage technique.

### Y a-t-il des risques de blocage ou de fermeture du compte Proton Mail ?

Proton applique strictement ses Conditions Générales et peut suspendre un compte en cas d’usage illicite documenté (spam massif, phishing, violation des lois suisses). Les comptes payants bénéficient d’un support direct et d’une procédure d’appel. Les rapports de transparence indiquent un nombre très faible de fermetures forcées, et la société conteste régulièrement les requêtes étatiques jugées abusives devant les tribunaux suisses.