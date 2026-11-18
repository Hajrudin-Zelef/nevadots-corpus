Le marché mondial du bug bounty et du hacking éthique communautaire a franchi les 2,45 milliards de dollars en 2026, porté par plus de 2,2 millions de chercheurs enregistrés sur les grandes plateformes, selon les données compilées par Voxbooster. Rien qu’en France, YesWeHack pilote depuis Paris le programme de divulgation coordonnée de plusieurs administrations, dont le ministère des Armées, aux côtés d’un écosystème européen désormais aussi actif que celui de la Silicon Valley. Pourtant, la majorité des débutants qui se lancent abandonnent avant leur premier rapport valide, souvent faute de méthode plutôt que faute de compétences techniques.

Ce tutoriel détaille, étape par étape, comment construire un environnement de travail légal et reproductible, choisir ses premiers programmes, mener une reconnaissance efficace avec les outils actuels (Subfinder, httpx, Nuclei, Burp Suite), et rédiger un rapport qui ne finira pas en doublon ignoré. L’objectif : passer de zéro à une première prime validée, sans enfreindre la loi française ni le droit européen au passage.

## Qu’est-ce que le bug bounty et pourquoi s’y lancer en 2026

Un programme de bug bounty est un accord contractuel par lequel une entreprise autorise des chercheurs externes à tester la sécurité de ses systèmes, en échange d’une récompense financière pour chaque vulnérabilité valide et reproductible. Contrairement à un test d’intrusion classique facturé au temps passé, le bug bounty rémunère au résultat : pas de faille trouvée, pas de paiement. Ce modèle attire les entreprises parce qu’il démultiplie la couverture de test. Selon les statistiques 2026 de Voxbooster, les programmes de bug bounty découvrent en moyenne 5,5 fois plus de vulnérabilités qu’un audit de pentest traditionnel sur un périmètre comparable, simplement parce que des centaines de chercheurs testent en parallèle au lieu d’une équipe de deux ou trois auditeurs.

Pour un débutant, l’intérêt est double. D’abord pédagogique : contrairement à un CTF (Capture The Flag) qui simule des vulnérabilités artificielles, le bug bounty confronte à de vrais systèmes en production, avec de vrais faux positifs, de vraies contraintes de scope et de vrais délais de réponse. Ensuite financier, même si les attentes doivent rester réalistes. HackerOne a versé 81 millions de dollars de primes sur les douze mois clos en juin 2025, en hausse de 13 % sur un an, répartis sur plus de 1 950 programmes actifs. Mais ce chiffre cache une réalité moins glamour : le chercheur actif moyen gagne environ 42 000 dollars par an sur la plateforme, et seuls 19 % des rapports soumis sont finalement jugés valides. Autrement dit, quatre rapports sur cinq n’aboutissent à rien, ce qui rend la méthode plus déterminante que la motivation brute.

Le paysage des plateformes s’est aussi consolidé. HackerOne, Bugcrowd et Intigriti dominent le marché international, tandis que YesWeHack conserve une position forte en Europe, notamment sur les programmes publics français et européens soumis à des exigences de souveraineté. Cette diversité change la donne pour un débutant francophone : il est possible de démarrer sur un programme européen, en français, avec un support client dans le même fuseau horaire, plutôt que de se lancer directement dans la compétition mondiale de HackerOne.

## L’écosystème du bug bounty en France et en Europe

La France occupe une place particulière dans ce paysage. YesWeHack, fondée à Paris en 2015, s’est positionnée dès le départ comme une alternative européenne aux plateformes américaines, en misant sur la conformité RGPD et l’hébergement de données en Europe, deux arguments qui pèsent lourd auprès des administrations et des grandes entreprises réglementées. La plateforme gère notamment le programme de divulgation coordonnée de vulnérabilités du ministère des Armées, une première pour un acteur privé français sur un périmètre aussi sensible. Cette proximité avec la sphère publique explique aussi pourquoi de plus en plus de collectivités locales et d’opérateurs d’importance vitale ouvrent des programmes de bug bounty, souvent en parallèle de leur mise en conformité avec la directive NIS2, qui impose des obligations renforcées de gestion des vulnérabilités aux entités essentielles et importantes.

Cette dynamique réglementaire crée une opportunité concrète pour les débutants francophones : les programmes publics liés au secteur public ou aux entités soumises à NIS2 publient généralement des scopes très documentés, avec des règles d’engagement précises, ce qui réduit l’ambiguïté qui décourage souvent les nouveaux venus sur les plateformes généralistes. À l’inverse, les programmes privés les plus lucratifs restent majoritairement anglophones et internationaux, ce qui signifie qu’une bonne maîtrise de l’anglais technique reste un atout décisif pour progresser au-delà des premiers mois.

## Prérequis techniques et légaux avant de commencer

Avant d’ouvrir un terminal, il faut réunir un socle de connaissances et d’outils. Voici la configuration recommandée pour ce tutoriel, testée sur une machine Linux (Kali Linux ou Ubuntu 24.04) avec au minimum 8 Go de RAM et 50 Go d’espace disque libre pour les listes de mots et les outils.

| Prérequis | Version recommandée (sept. 2026) | Usage | 
|---|---|---|
| Système d’exploitation | Kali Linux 2026.2 ou Ubuntu 24.04 LTS | Environnement de test isolé | 
| Go | 1.23 ou supérieur | Compilation des outils ProjectDiscovery | 
| Python | 3.12+ | Scripts d’automatisation et outils annexes | 
| Burp Suite Community Edition | Dernière version stable PortSwigger | Interception et analyse manuelle du trafic HTTP | 
| Subfinder / httpx / Nuclei | Dernières releases GitHub ProjectDiscovery | Reconnaissance et détection automatisée | 
| VPN ou machine dédiée | Connexion stable et traçable | Respect des règles de scope IP si exigées | 
| Compte plateforme | YesWeHack, HackerOne, Intigriti ou Bugcrowd | Accès légal aux programmes | 

Sur le plan légal, un point est non négociable : en France, l’accès ou le maintien frauduleux dans un système de traitement automatisé de données est réprimé par le Code pénal, indépendamment de l’intention affichée du chercheur. La directive européenne relative aux attaques contre les systèmes d’information renforce ce cadre au niveau de l’Union. Autrement dit, “ethical” ne veut rien dire juridiquement si le test sort du périmètre autorisé par le programme. Seule une autorisation écrite, un scope clairement défini et le respect strict des règles publiées par l’entreprise (safe harbor) protègent le chercheur. Le site gouvernemental cyber.gouv.fr rappelle régulièrement ce principe dans ses guides de divulgation coordonnée de vulnérabilités.

Autre point souvent négligé : le RGPD s’applique dès qu’un test touche, même accidentellement, des données personnelles. Un chercheur qui tombe sur une base d’utilisateurs exposée doit documenter l’accès minimal nécessaire à la preuve de concept, ne jamais exfiltrer de données réelles, et signaler immédiatement la découverte sans l’exploiter davantage. C’est un réflexe à intégrer dès l’étape 1, pas une réflexion a posteriori.

## Étape 1 : Choisir sa plateforme de bug bounty

Le choix de la plateforme conditionne le type de programmes accessibles, la langue de support et les modalités de paiement. Pour un débutant francophone, YesWeHack présente l’avantage d’une communauté et d’un support en français, ainsi que d’un accès à des programmes publics français (administrations, collectivités) qui imposent des exigences de conformité claires et un scope souvent bien documenté. HackerOne et Bugcrowd offrent un volume de programmes bien plus large à l’échelle mondiale, avec une compétition proportionnellement plus forte. Intigriti, basée en Belgique, se positionne comme une alternative européenne avec une forte présence de marques du e-commerce et de la fintech.

La recommandation pratique : créer un compte sur au moins deux plateformes dès le départ, mais concentrer son énergie sur une seule au début. Passer d’un programme à l’autre sans approfondir dilue l’apprentissage. Une fois le compte créé, il faut généralement passer par une phase de vérification d’identité (KYC) avant de pouvoir recevoir des paiements, ce qui peut prendre plusieurs jours. Autant l’anticiper avant d’avoir trouvé sa première faille.

## Étape 2 : Sélectionner son premier programme

Toutes les plateformes distinguent les programmes publics (ouverts à tous les chercheurs inscrits) des programmes privés (accessibles sur invitation, généralement après avoir fait ses preuves). Pour débuter, il faut filtrer les programmes publics selon trois critères : un scope large incluant des applications web classiques, une table de récompenses publiée (les programmes “reconnaissance uniquement”, sans paiement, sont utiles pour le portfolio mais ne doivent pas être la priorité), et un historique de réponse rapide de l’entreprise, visible dans les statistiques du programme (temps de première réponse, temps de résolution).

Il faut absolument lire la politique du programme dans son intégralité avant de commencer, en particulier trois sections : le scope exact (domaines, sous-domaines, applications mobiles inclus ou exclus), les types de tests interdits (déni de service, ingénierie sociale, spam, tests automatisés agressifs), et les catégories de vulnérabilités explicitement hors périmètre (souvent le clickjacking sur des pages sans action sensible, l’absence de rate limiting sur des endpoints non critiques, ou les en-têtes de sécurité manquants sans impact démontré). Soumettre un rapport sur un élément explicitement exclu est la première cause de rejet chez les débutants.

## Étape 3 : Installer la boîte à outils de reconnaissance

La suite ProjectDiscovery (Subfinder, httpx, Nuclei, dnsx) constitue la base de reconnaissance la plus utilisée par la communauté en 2026, en complément de Burp Suite pour l’analyse manuelle. Voici l’installation via Go, qui garantit d’obtenir les dernières versions directement depuis les dépôts officiels.

```
# Installer Go si nécessaire
sudo apt update && sudo apt install golang-go -y
# Installer les outils ProjectDiscovery
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
go install -v github.com/projectdiscovery/dnsx/cmd/dnsx@latest
# Ajouter le binPath Go au PATH
export PATH=$PATH:$(go env GOPATH)/bin
echo 'export PATH=$PATH:$(go env GOPATH)/bin' >> ~/.bashrc
# Vérifier les installations
subfinder -version
httpx -version
nuclei -version
```
Nuclei nécessite ensuite une mise à jour de ses modèles de détection, qui sont maintenus par la communauté et mis à jour quasi quotidiennement pour intégrer les nouvelles CVE et les nouveaux patterns de mauvaise configuration.

```
nuclei -update-templates
nuclei -update
```
Pour Burp Suite, la version Community Edition, gratuite, suffit largement à débuter : elle inclut le proxy d’interception, Repeater pour rejouer des requêtes modifiées, et Intruder en version limitée. La version Professional de PortSwigger ajoute le scanner actif automatisé et Intruder sans restriction de débit, un investissement à envisager une fois les premiers rapports validés et les premières primes encaissées, pas avant.

## Étape 4 : Cartographier la surface d’attaque (reconnaissance passive)

La reconnaissance passive consiste à identifier tous les sous-domaines et actifs numériques d’une cible sans envoyer la moindre requête directe vers ses serveurs, en s’appuyant uniquement sur des sources publiques (certificats TLS, journaux de transparence, moteurs de recherche spécialisés). Cette étape est cruciale car un sous-domaine oublié, souvent un environnement de préproduction ou de démonstration mal désindexé, concentre une part disproportionnée des vulnérabilités trouvées par les débutants.

```
# Énumération passive des sous-domaines
subfinder -d exemple-cible.com -all -recursive -o subdomains.txt
# Résolution DNS et filtrage des domaines actifs
cat subdomains.txt | dnsx -silent -o resolved.txt
# Vérification HTTP/HTTPS avec titres de page et technologies
cat resolved.txt | httpx -silent -title -tech-detect -status-code -o live_hosts.txt
cat live_hosts.txt
```
Le fichier `live_hosts.txt` obtenu constitue la carte de la surface d’attaque. Il faut le confronter méthodiquement au scope publié du programme : tout sous-domaine hors périmètre doit être écarté, même s’il paraît vulnérable. Tester en dehors du scope, même par curiosité, viole les conditions du programme et peut entraîner une exclusion définitive de la plateforme.

## Étape 5 : Scanner les vulnérabilités connues avec Nuclei

Une fois la liste des hôtes actifs et dans le scope établie, Nuclei permet de détecter rapidement les mauvaises configurations courantes et les vulnérabilités déjà documentées dans ses modèles communautaires : panneaux d’administration exposés, fichiers de configuration accessibles, versions de logiciels obsolètes avec CVE connue, absence d’en-têtes de sécurité critiques.

```
# Scan avec limitation du débit pour respecter les règles du programme
nuclei -l live_hosts.txt -severity medium,high,critical -rate-limit 10 -o nuclei_results.txt
# Scan ciblé sur les expositions de fichiers sensibles
nuclei -l live_hosts.txt -tags exposure,misconfig -rate-limit 10 -o exposures.txt
```
Le paramètre `-rate-limit` n’est pas optionnel. La quasi-totalité des programmes imposent une limite de requêtes par seconde, et un scan trop agressif peut être interprété comme une tentative de déni de service, motif d’exclusion immédiate. Un résultat positif de Nuclei n’est jamais une preuve suffisante : il s’agit d’un indice à valider manuellement avant toute soumission, car le taux de faux positifs sur les scans automatisés reste significatif.

## Sécuriser son propre environnement de test

Un point souvent oublié par les débutants concerne la sécurité de leur propre poste de travail. Un chercheur qui teste des applications potentiellement compromises ou qui télécharge des outils communautaires depuis des dépôts non vérifiés s’expose lui-même à des risques : payloads malveillants déguisés en outils de recon, extensions de navigateur compromises, ou scripts partagés sur des forums qui embarquent des portes dérobées. La pratique recommandée consiste à isoler l’environnement de test dans une machine virtuelle dédiée, sans accès aux identifiants personnels ni aux données sensibles du chercheur, et à ne jamais exécuter un outil téléchargé sans en avoir vérifié la provenance sur le dépôt officiel du projet.

Il est également conseillé d’utiliser une identité et une adresse e-mail dédiées à l’activité de bug bounty, séparées de l’identité professionnelle ou personnelle habituelle. Cela simplifie la gestion administrative (facturation des primes, déclarations fiscales le cas échéant) et limite l’exposition en cas de conflit avec une entreprise testée. En France, les primes perçues via une activité de bug bounty régulière constituent un revenu imposable, qu’il convient de déclarer selon le régime applicable (bénéfices non commerciaux pour une activité indépendante, ou micro-entreprise au-delà d’un certain volume), un point que beaucoup de débutants découvrent trop tard.

## Étape 6 : Explorer manuellement avec Burp Suite

C’est à cette étape que la majorité des vulnérabilités réellement payées sont découvertes, car les scanners automatisés ratent systématiquement les failles de logique métier. Configurez Burp Suite comme proxy dans votre navigateur (généralement 127.0.0.1:8080), installez le certificat CA de Burp, puis naviguez normalement sur l’application cible en laissant Burp capturer chaque requête dans l’onglet Proxy.

L’objectif de cette phase est de comprendre le fonctionnement métier de l’application : comment les identifiants d’objets sont générés, comment les rôles utilisateurs sont vérifiés côté serveur, comment les paramètres de requête influencent les réponses. C’est en modifiant méthodiquement ces paramètres avec Repeater, un par un, que se révèlent les vulnérabilités de contrôle d’accès. Selon les données 2026 compilées par Voxbooster, le contrôle d’accès défaillant (Broken Access Control) représente environ 38 % des vulnérabilités rapportées sur l’ensemble des plateformes, ce qui en fait de loin la catégorie la plus rentable pour un débutant méthodique, bien avant les injections ou les failles XSS.

## Étape 7 : Tester le contrôle d’accès (IDOR et BOLA)

Les références directes non sécurisées à des objets (IDOR – Insecure Direct Object Reference) et les autorisations défaillantes au niveau objet (BOLA – Broken Object Level Authorization) surviennent lorsqu’une application vérifie qu’un utilisateur est authentifié, mais pas qu’il a réellement le droit d’accéder à la ressource demandée. C’est la catégorie de vulnérabilité la plus accessible aux débutants, car elle ne nécessite ni payload sophistiqué ni contournement de filtre, seulement de la méthode.

La méthode consiste à créer deux comptes de test sur la plateforme cible (si le programme l’autorise), à identifier une requête qui référence un identifiant (numéro de commande, identifiant de profil, identifiant de document), puis à remplacer cet identifiant par celui appartenant au second compte tout en restant authentifié avec le premier. Si la ressource du second compte est retournée, la faille est confirmée. Il faut documenter chaque requête avec Repeater, capturer les requêtes et réponses complètes, et vérifier que le comportement est reproductible avant de rédiger le rapport.

## Étape 8 : Rechercher les failles d’authentification et de session

Les mécanismes d’authentification restent une source fiable de vulnérabilités valides : absence de limitation du nombre de tentatives sur les formulaires de connexion, réinitialisation de mot de passe prévisible ou basée sur un jeton faible, jetons de session qui ne sont pas invalidés après déconnexion, ou processus d’inscription permettant de confirmer l’existence d’un compte via les messages d’erreur (énumération d’utilisateurs).

Pour chaque test, il faut vérifier séparément trois éléments : la résistance du flux de réinitialisation de mot de passe à la prédiction ou à la réutilisation de jeton, le comportement de l’application face à des tentatives de connexion répétées (présence ou non de CAPTCHA, de verrouillage temporaire, de limitation de débit), et la validité du jeton de session après une action de déconnexion explicite. Ces tests, contrairement aux scans automatisés, exigent une observation attentive plutôt qu’un volume de requêtes élevé, ce qui les rend compatibles avec des programmes au rate limiting strict.

## Étape 9 : Identifier les injections et failles XSS

Les injections SQL et les scripts intersites (XSS) restent des catégories classiques, bien que leur fréquence relative ait diminué avec la généralisation des frameworks modernes qui échappent les entrées par défaut. Elles restent néanmoins fréquentes sur les formulaires de recherche, les paramètres d’URL peu testés, et les fonctionnalités d’export de données (PDF, CSV) qui contournent souvent l’échappement appliqué à l’affichage HTML standard.

```
# Test manuel XSS réfléchi sur un paramètre
curl -s "https://cible-autorisee.com/recherche?q=%3Cscript%3Ealert(1)%3C%2Fscript%3E" | grep -o ""
# Vérification d'une injection SQL basique avec délai (à n'exécuter QUE dans le scope autorisé)
curl -s -o /dev/null -w "%{time_total}\n" "https://cible-autorisee.com/produit?id=1' AND SLEEP(3)-- -"
```
Ce type de test doit toujours commencer par un payload d’observation non destructif avant tout payload de preuve d’exploitation. Injecter une charge utile qui modifie ou supprime des données en production, même pour prouver une injection SQL, est interdit par la quasi-totalité des programmes et peut avoir des conséquences légales, même avec une autorisation de test.

## Aller plus loin : reconnaissance active et découverte de contenu

Au-delà de la reconnaissance passive, la découverte de contenu par force brute (fuzzing) permet de révéler des chemins et paramètres non liés depuis l’interface publique de l’application : anciens points de terminaison d’API laissés actifs, panneaux d’administration à l’URL prévisible, fichiers de sauvegarde oubliés sur le serveur. L’outil ffuf, écrit en Go et largement adopté par la communauté, permet ce type de test tout en respectant une cadence de requêtes maîtrisée.

```
# Installer ffuf
go install github.com/ffuf/ffuf/v2@latest
# Fuzzing de répertoires avec limitation de débit
ffuf -u https://cible-autorisee.com/FUZZ -w /usr/share/wordlists/dirb/common.txt -rate 10 -o ffuf_results.json
# Récupérer les URLs historiques archivées (Wayback Machine)
go install github.com/tomnomnom/waybackurls@latest
echo "cible-autorisee.com" | waybackurls > historical_urls.txt
```
Les URLs historiques récupérées via waybackurls révèlent souvent des paramètres ou des routes qui ont existé à un moment donné, y compris sur des versions antérieures de l’application qui ne sont plus visibles depuis la navigation normale. Croiser cette liste avec les résultats de httpx et de ffuf permet de repérer des points d’entrée que la majorité des chercheurs concurrents n’auront pas testés, ce qui augmente sensiblement les chances de trouver une vulnérabilité inédite plutôt qu’un doublon.

## Étape 10 : Documenter et reproduire la vulnérabilité

Une fois une anomalie identifiée, il faut immédiatement isoler et documenter sa reproduction avant de continuer à explorer. Chaque étape doit être capturée : requête HTTP brute exportée depuis Burp, capture d’écran horodatée du résultat, et description précise de l’état préalable nécessaire (compte connecté, session active, paramètre spécifique). Un rapport qui ne peut pas être reproduit par l’équipe de sécurité de l’entreprise est systématiquement fermé comme “non reproductible”, quelle que soit la réalité de la faille.

Il est recommandé de tester la reproduction au moins deux fois, à quelques minutes d’intervalle, en partant d’un état propre (nouvelle session, cache navigateur vidé) pour éliminer les faux positifs liés à un état de session résiduel. C’est également le moment d’évaluer honnêtement l’impact réel : une vulnérabilité technique sans impact métier démontrable (accès à une information déjà publique, par exemple) sera généralement reclassée en sévérité faible ou refusée, même si elle est techniquement valide.

## Étape 11 : Rédiger un rapport qui sera accepté

La qualité de la rédaction pèse autant que la qualité technique de la découverte dans la décision finale d’une équipe de triage. Un rapport efficace suit une structure constante : un titre précis indiquant le type de vulnérabilité et l’endpoint concerné, un résumé de l’impact métier en une ou deux phrases compréhensibles par un non-technicien, les étapes de reproduction numérotées avec les requêtes exactes, une preuve de concept (capture d’écran ou vidéo courte), et une évaluation de sévérité justifiée, idéalement selon le référentiel CVSS.

Il faut éviter deux écueils fréquents chez les débutants : exagérer l’impact pour obtenir une prime plus élevée (les équipes de triage expérimentées détectent rapidement cette pratique et elle nuit durablement à la réputation du compte), et au contraire noyer le rapport dans des détails techniques non pertinents qui masquent l’essentiel. Un rapport clair et concis, même sur une vulnérabilité de sévérité moyenne, obtient généralement une réponse plus rapide et un traitement plus favorable qu’un rapport confus sur une faille critique.

## Étape 12 : Gérer les retours et les délais de traitement

Après soumission, plusieurs issues sont possibles : validation directe, demande d’informations complémentaires, reclassement de sévérité, marquage comme doublon d’un rapport existant, ou fermeture pour absence d’impact suffisant. Les statistiques par programme publiées sur les plateformes indiquent généralement un délai moyen de première réponse et un délai moyen de résolution, deux indicateurs à consulter avant même de choisir son programme initial. Un programme dont le délai de première réponse dépasse plusieurs semaines décourage généralement les chercheurs les plus actifs, ce qui peut paradoxalement réduire la concurrence sur ce programme précis.

En cas de désaccord sur la sévérité ou le montant de la prime proposée, la plupart des plateformes prévoient un mécanisme de médiation. Il est préférable de l’utiliser avec mesure et arguments techniques précis plutôt que de multiplier les relances, qui peuvent être perçues négativement par l’équipe de triage lors de l’examen des futurs rapports du même chercheur.

## Étape 13 : Passer à l’échelle et se spécialiser

Une fois les premiers rapports validés, la progression naturelle consiste à se spécialiser sur une classe de vulnérabilités ou un type de technologie (applications mobiles, API GraphQL, infrastructures cloud) plutôt qu’à multiplier les programmes généralistes. Cette spécialisation permet d’accéder plus rapidement aux programmes privés sur invitation, généralement mieux rémunérés et moins saturés de chercheurs. Elle justifie aussi l’investissement dans Burp Suite Professional, dont le scanner actif automatisé et l’absence de limitation de débit sur Intruder deviennent rentables dès que le volume de tests augmente.

Construire un historique public de découvertes (avec l’accord des entreprises concernées, via les pages de remerciements ou “Hall of Fame”) reste également le levier le plus efficace pour obtenir des invitations à des programmes privés, bien plus qu’un nombre élevé de certifications.

## Table des récompenses moyennes par sévérité en 2026

Les montants varient fortement d’un programme à l’autre, mais les données indicatives compilées à partir de plusieurs plateformes en 2026 donnent un ordre de grandeur utile pour calibrer ses attentes.

| Sévérité | Récompense moyenne indicative | Exemple de vulnérabilité | 
|---|---|---|
| Critique | ≈ 4 200 $ | Exécution de code à distance, prise de contrôle de compte admin | 
| Élevée | ≈ 2 150 $ | IDOR avec accès à des données sensibles d’autres utilisateurs | 
| Moyenne | ≈ 750 $ | XSS stocké, contournement partiel d’autorisation | 
| Faible | ≈ 250 $ | Absence d’en-tête de sécurité avec impact limité | 

Ces chiffres doivent être pris comme des repères, pas comme des garanties. Certains programmes majeurs révisent régulièrement leurs barèmes à la baisse, comme l’a documenté The Register en mai 2026 concernant l’Internet Bug Bounty de HackerOne, dont la récompense critique est passée de 9 250 à 2 257 dollars. Toujours vérifier la table de récompenses spécifique au programme ciblé avant de soumettre un rapport.

## Erreurs courantes des débutants en bug bounty

Certaines erreurs reviennent systématiquement et expliquent une large part des rapports jugés non valides sur les plateformes majeures.

- **Ignorer le scope** : tester des sous-domaines ou des fonctionnalités explicitement exclus, ce qui entraîne un rejet immédiat et parfois une sanction sur le compte.
- **Soumettre des doublons** : ne pas vérifier si une vulnérabilité similaire a déjà été signalée, alors que la plupart des programmes publient un historique des rapports déjà traités.
- **Scanner trop agressivement** : dépasser la limite de requêtes par seconde tolérée, ce qui peut être interprété comme une attaque par déni de service.
- **Exagérer la sévérité** : présenter une faille mineure comme critique sans preuve d’impact métier réel, ce qui nuit à la crédibilité du compte sur le long terme.
- **Négliger la reproduction** : soumettre un rapport sans avoir vérifié que la faille est reproductible de façon fiable et constante.

## Exemple de sortie attendue lors d’une reconnaissance réussie

Voici à quoi ressemble une sortie typique de reconnaissance une fois les commandes de l’étape 4 exécutées sur un domaine réel, avec les identifiants remplacés par un exemple générique.

```
$ cat live_hosts.txt
https://app.exemple-cible.com [200] [Tableau de bord] [React,Nginx]
https://staging.exemple-cible.com [401] [Non autorisé] [Apache,PHP]
https://api.exemple-cible.com [200] [API Gateway] [Kong]
https://old-portal.exemple-cible.com [200] [Ancien portail 2022] [jQuery 1.9,IIS]
$ nuclei -l live_hosts.txt -severity high,critical -rate-limit 10
[2026-09-22 10:14:02] [exposed-config] [high] https://old-portal.exemple-cible.com/config.old.php
[2026-09-22 10:14:19] [outdated-jquery] [medium] https://old-portal.exemple-cible.com
```
Dans cet exemple type, le sous-domaine `old-portal`, visiblement un ancien portail non maintenu, concentre les résultats les plus intéressants, ce qui illustre la règle générale : les actifs oubliés valent presque toujours mieux que les applications principales, plus souvent auditées et corrigées en priorité par les équipes internes.

## Guide de dépannage : problèmes courants et solutions

| Problème | Cause probable | Solution | 
|---|---|---|
| Subfinder ne retourne aucun sous-domaine | Absence de clés API pour les sources tierces | Configurer le fichier provider-config.yaml avec des clés API gratuites | 
| Nuclei signale un faux positif | Modèle communautaire trop générique | Valider manuellement chaque résultat avant soumission | 
| Burp Suite bloque le trafic HTTPS | Certificat CA non installé dans le navigateur | Réimporter le certificat via burp/cert dans les paramètres du proxy | 
| Compte suspendu après un scan | Dépassement de la limite de requêtes autorisée | Contacter le support de la plateforme et réduire drastiquement le rate-limit | 
| Rapport marqué “non reproductible” | Documentation insuffisante des conditions préalables | Refaire le test à froid et capturer chaque requête/réponse | 
| Rapport marqué “doublon” | Vulnérabilité déjà connue de l’entreprise | Consulter l’historique des rapports publics avant de soumettre | 
| Aucune réponse après plusieurs semaines | Programme peu actif ou en sous-effectif côté sécurité | Vérifier les statistiques de délai de réponse avant de choisir un programme | 
| go install échoue avec une erreur de module | Version de Go obsolète | Mettre à jour Go vers la version 1.23 ou supérieure | 
| IP bloquée par un WAF pendant les tests | Détection de comportement automatisé | Espacer les requêtes et ajouter un délai entre chaque appel | 

## Conseils avancés pour progresser plus vite

Au-delà des bases, plusieurs pratiques distinguent les chercheurs réguliers des débutants qui abandonnent après quelques semaines. La première consiste à automatiser la surveillance continue des sous-domaines sur les programmes déjà testés : un script planifié qui relance Subfinder et httpx une fois par semaine permet de détecter un nouvel actif exposé avant les autres chercheurs, souvent la fenêtre la plus rentable pour trouver une faille.

La deuxième consiste à lire systématiquement les rapports publics divulgués sur HackerOne et les articles de recherche publiés par la communauté : comprendre comment d’autres chercheurs ont raisonné sur des applications similaires accélère considérablement la courbe d’apprentissage, bien plus qu’une formation théorique isolée. Enfin, tenir un journal structuré de chaque test mené, même infructueux, évite de répéter les mêmes vérifications sur les mêmes actifs et permet de repérer des patterns récurrents propres à une entreprise ou une stack technique donnée.

## Projet complet : de zéro à la première soumission

Pour résumer l’ensemble du processus en un flux de travail reproductible, voici le script d’enchaînement complet qui reprend les étapes de reconnaissance décrites plus haut, à adapter avec le domaine réellement autorisé par le programme choisi.

```
#!/bin/bash
# Workflow de reconnaissance bug bounty - à utiliser UNIQUEMENT sur un scope autorisé
CIBLE="exemple-cible.com"
DOSSIER="recon_$(date +%Y%m%d)"
mkdir -p $DOSSIER
echo "[1/4] Énumération passive des sous-domaines..."
subfinder -d $CIBLE -all -recursive -silent -o $DOSSIER/subdomains.txt
echo "[2/4] Résolution DNS..."
cat $DOSSIER/subdomains.txt | dnsx -silent -o $DOSSIER/resolved.txt
echo "[3/4] Vérification des hôtes actifs..."
cat $DOSSIER/resolved.txt | httpx -silent -title -tech-detect -status-code -o $DOSSIER/live_hosts.txt
echo "[4/4] Scan de vulnérabilités connues (rate-limit respecté)..."
nuclei -l $DOSSIER/live_hosts.txt -severity medium,high,critical -rate-limit 10 -o $DOSSIER/nuclei_results.txt
echo "Terminé. Résultats dans $DOSSIER/"
echo "Rappel : ne tester QUE les actifs confirmés dans le scope du programme."
```
Ce script constitue une base de départ, pas une solution clé en main : chaque programme impose ses propres contraintes de scope et de débit, qu’il faut adapter manuellement avant chaque exécution. La discipline de vérification du scope avant chaque lancement reste la garantie la plus importante contre une exclusion de plateforme.

## Comparatif des principales plateformes pour débuter

| Plateforme | Origine | Point fort pour un débutant francophone | 
|---|---|---|
| YesWeHack | France (Paris) | Support en français, programmes publics français et européens | 
| Intigriti | Belgique | Forte présence e-commerce et fintech européenne | 
| HackerOne | États-Unis | Volume de programmes le plus large au monde | 
| Bugcrowd | États-Unis | Programmes orientés grandes entreprises technologiques | 

## Combien de temps avant la première prime

Il n’existe pas de statistique universelle vérifiée sur le délai moyen jusqu’au premier rapport valide, car cela dépend fortement du niveau technique de départ, du temps investi chaque semaine et du choix des premiers programmes. Une estimation raisonnable, fondée sur l’expérience de la communauté, situe ce délai entre un et six mois pour un débutant assidu qui étudie la sécurité web en parallèle, pratique sur des plateformes d’entraînement légales (labs dédiés) et concentre ses efforts sur un nombre restreint de programmes plutôt que de se disperser. Trouver un rapport valide ne garantit d’ailleurs pas systématiquement un paiement immédiat : la faille doit rester reproductible, non dupliquée et suffisamment impactante aux yeux de l’équipe de triage.

Ce qui accélère nettement la progression, c’est la régularité plutôt que l’intensité ponctuelle. Consacrer quelques heures chaque semaine à un seul programme, en approfondissant sa compréhension de l’application plutôt qu’en multipliant les cibles superficielles, produit statistiquement de meilleurs résultats que des sessions marathon isolées.

## Foire aux questions

**Le bug bounty est-il légal en France sans diplôme spécifique ?**

Oui, tant que le chercheur reste strictement dans le scope autorisé par le programme et respecte ses règles publiées. Aucun diplôme n’est requis légalement, mais tester en dehors du périmètre autorisé reste un délit au regard du Code pénal, quel que soit le niveau de compétence du testeur.

**Faut-il payer pour Burp Suite Professional dès le début ?**

Non. La version Community Edition, gratuite, couvre largement les besoins d’un débutant. L’investissement dans la version Professional devient pertinent une fois le volume de tests augmenté, notamment pour le scanner actif automatisé.

**Peut-on faire du bug bounty sur mobile ou uniquement sur des applications web ?**

Les deux sont possibles. De nombreux programmes incluent des applications mobiles iOS et Android dans leur scope, mais cela nécessite des outils et compétences additionnels (décompilation, interception de trafic mobile) qu’il est préférable d’aborder après avoir maîtrisé les bases web.

**Quelle est la différence entre un programme public et un programme privé ?**

Un programme public est ouvert à tout chercheur inscrit sur la plateforme, tandis qu’un programme privé nécessite une invitation, généralement accordée après un historique de rapports valides. Les programmes privés sont souvent moins saturés de chercheurs et parfois mieux rémunérés.

**Que faire si l’entreprise conteste la sévérité de mon rapport ?**

La plupart des plateformes proposent un mécanisme de médiation. Il est préférable d’apporter des arguments techniques précis et mesurés plutôt que de multiplier les relances, ce qui peut nuire à la réputation du compte sur le long terme.

**Les CTF (Capture The Flag) suffisent-ils pour se préparer au bug bounty réel ?**

Les CTF développent des réflexes techniques utiles, mais les vulnérabilités y sont conçues artificiellement pour être trouvées. Le bug bounty en conditions réelles exige en plus une compréhension du scope, de la logique métier et de la rédaction de rapport, des compétences que les CTF n’enseignent pas directement.

**Un rapport refusé compte-t-il contre le chercheur ?**

Un rapport refusé pour manque d’impact ou doublon n’entraîne généralement pas de sanction, sauf s’il révèle un non-respect répété du scope ou des règles du programme. La plupart des plateformes calculent toutefois un taux de précision affiché publiquement, qu’il vaut mieux soigner sur le long terme.

**Comment savoir si un programme paie réellement ses chercheurs ?**

Les plateformes publient des statistiques par programme : total des primes versées, prime moyenne, fourchette de la prime la plus élevée, et primes versées sur les 90 derniers jours. Ces chiffres, consultables avant de choisir un programme, donnent une indication fiable de l’activité réelle de paiement.