---
id: collect-261121-veille-ia/windows-os/faille-cisco-sd-wan-7-zero-days-a-10-10-2026
title: "Faille Cisco SD-WAN : 7 Zero-Days à 10/10 [2026]"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["cisco", "sd-wan", "manager", "faille", "deux", "cvss", "zero-days", "sept"]
source: https://tech-insider.org/fr/faille-cisco-sd-wan-2026/
source_anchor: ""
source_lines: [1, 189]
section: "Windows_OS"
---
Mise à jour du 07 juillet 2026 – En six mois, l’infrastructure **Cisco Catalyst SD-WAN** est devenue l’un des terrains de chasse les plus actifs du cyberespionnage mondial. Depuis février 2026, sept vulnérabilités distinctes ont été exploitées en conditions réelles, dont deux **contournements d’authentification notés 10 sur 10** sur l’échelle CVSS – le score maximal. Derrière une partie de ces intrusions se cache UAT-8616, un acteur qualifié de « hautement sophistiqué » par les chercheurs de Cisco Talos, dont l’infrastructure recoupe des réseaux de relais opérationnels typiques des opérations soutenues par un État. Pour les entreprises françaises et européennes qui ont bâti l’épine dorsale de leur réseau sur cette technologie, la **faille Cisco SD-WAN** n’est plus une hypothèse : c’est un incident en cours.

Cet article détaille la chronologie complète de la campagne, la mécanique technique des deux zero-days critiques, le profil de l’acteur UAT-8616, l’ampleur de la surface d’attaque exposée sur Internet, et surtout ce que cela implique pour les opérateurs d’infrastructures critiques soumis à la directive NIS 2. Les faits, les scores CVSS et les versions correctives proviennent des avis officiels de Cisco, du catalogue KEV de la CISA, et des analyses de Mandiant, Tenable, VulnCheck et Censys.

## Sept zero-days Cisco SD-WAN exploités en 2026 : le récapitulatif

Le 5 juin 2026, Cisco a confirmé l’exploitation d’une septième vulnérabilité de sa gamme SD-WAN depuis le début de l’année, comme l’a rapporté SecurityWeek. Ce rythme est inhabituel, même pour un produit aussi répandu. La **faille Cisco SD-WAN** ne désigne donc pas un événement isolé, mais une série coordonnée de découvertes et d’exploitations qui, mises bout à bout, permettent à un attaquant de passer de zéro accès au contrôle total du plan de gestion réseau.

Le tableau ci-dessous récapitule les sept vulnérabilités concernées, avec leur score CVSS, leur nature et le produit affecté. Deux d’entre elles – CVE-2026-20127 et CVE-2026-20182 – atteignent la note maximale de 10,0, réservée aux failles exploitables à distance, sans authentification et sans interaction de la victime.

| Les sept vulnérabilités Cisco Catalyst SD-WAN exploitées en 2026. Sources : avis Cisco, catalogue KEV de la CISA, Tenable, Mandiant. |  |  |  |  | 
|---|---|---|---|---|
| Identifiant CVE | CVSS | Type | Produit affecté | Statut CISA KEV | 
|---|---|---|---|---|
| CVE-2026-20182 | **10,0** | Contournement d’authentification | Controller & Manager | 14 mai 2026 | 
| CVE-2026-20127 | **10,0** | Contournement d’authentification | Controller & Manager | 25 févr. 2026 | 
| CVE-2022-20775 | 7,8 | Élévation de privilèges (root) | vBond, vEdge, vSmart, vManage | 25 févr. 2026 | 
| CVE-2026-20133 | 7,5 | Divulgation d’informations | Manager | 20 avr. 2026 | 
| CVE-2026-20128 | 7,5 | Accès aux identifiants | Manager | 20 avr. 2026 | 
| CVE-2026-20122 | 5,4 | Écrasement de fichier arbitraire | Manager | 20 avr. 2026 | 
| CVE-2026-20245 | Root (auth. locale) | Exécution de commandes root | Manager (CLI) | Juin 2026 | 

À noter : CVE-2022-20775 n’est pas une vulnérabilité de 2026. Découverte en 2022, elle a été « réactivée » dans les chaînes d’exploitation observées cette année, ce qui illustre un principe fondamental de la sécurité de la bordure réseau : une faille corrigée mais non déployée reste une porte ouverte. Le score CVSS de CVE-2026-20245 n’a pas été communiqué publiquement par Cisco à la date de cet article ; nous la décrivons donc par son impact (exécution de commandes en tant que root) plutôt que par un chiffre invérifiable.

## CVE-2026-20182 et CVE-2026-20127 : deux contournements notés 10/10

Les deux vulnérabilités les plus graves partagent la même racine : un défaut dans le mécanisme d’authentification de peering du Cisco Catalyst SD-WAN Controller et du SD-WAN Manager. Concrètement, le processus de poignée de main des connexions de contrôle (*control connection handshaking*) ne valide pas correctement l’identité des pairs qui tentent de rejoindre le tissu réseau. Un attaquant distant non authentifié peut donc envoyer des requêtes forgées pour se faire passer pour un composant légitime.

Selon l’avis de sécurité officiel de Cisco, CVE-2026-20182 est notée CVSS 10,0 (critique) et classée CWE-287 (« Improper Authentication »). En cas de succès, l’attaquant obtient une session en tant que compte interne hautement privilégié – non root, mais doté d’un accès à NETCONF. Or NETCONF est le protocole qui pilote la configuration de l’ensemble du fabric SD-WAN. Autrement dit, une seule requête bien formée peut suffire à reconfigurer le routage, injecter des règles, ou détourner le trafic de dizaines de sites distants.

Cisco PSIRT, l’équipe d’intervention de l’éditeur, a reconnu avoir eu connaissance d’une exploitation « limitée » de CVE-2026-20182 dès mai 2026. Les actions observées incluent l’ajout de clés SSH, la modification de configurations liées à NETCONF et des tentatives d’élévation vers root. La société de sécurité Rapid7 a confirmé indépendamment que le correctif était disponible et que la faille était activement exploitée au moment de sa divulgation, le 14 mai 2026.

La première des deux failles à 10/10, CVE-2026-20127, avait été rendue publique près de trois mois plus tôt, le 25 février 2026. Elle avait été immédiatement ajoutée au catalogue des vulnérabilités activement exploitées (KEV) de la CISA. Le fait que deux failles de gravité maximale, quasiment identiques dans leur mécanique, aient été découvertes à si peu d’intervalle suggère une classe entière de défauts d’authentification dans l’architecture de peering – et donc, probablement, d’autres découvertes à venir.

## UAT-8616 : le groupe étatique derrière les intrusions

La signature la plus préoccupante de cette campagne porte un nom : UAT-8616. D’après la synthèse publiée par Tenable, ce groupe exploite des infrastructures Cisco SD-WAN depuis au moins 2023 – soit bien avant que les failles de 2026 ne soient rendues publiques. Il cible spécifiquement les secteurs d’infrastructures critiques, et son infrastructure de commande recoupe des réseaux de relais opérationnels (Operational Relay Box, ou ORB).

Ce détail des ORB est déterminant pour l’analyse. Les relais opérationnels – souvent constitués d’équipements compromis, de routeurs domestiques piratés ou de VPS anonymes – sont la marque de fabrique des acteurs étatiques cherchant à masquer l’origine géographique de leurs opérations. Ce sont eux qui ont rendu l’attribution du cyberespionnage si difficile ces dernières années. Le chevauchement d’UAT-8616 avec ce type d’infrastructure oriente fortement l’analyse vers un acteur aligné sur un État, motivé par l’espionnage à long terme plutôt que par le gain financier immédiat.

Le mode opératoire post-compromission confirme cette lecture. Une fois l’accès obtenu, UAT-8616 ne déploie ni rançongiciel ni mineur de cryptomonnaie. Le groupe injecte des clés SSH pour garantir sa persistance, manipule NETCONF pour reconfigurer le réseau, crée des comptes malveillants, puis efface méthodiquement les journaux pour couvrir ses traces. Cette discrétion – la volonté de rester présent sans être détecté – est l’inverse exact du comportement des groupes cybercriminels, qui cherchent à monétiser rapidement leur intrusion.

## Anatomie de l’attaque : du contournement au contrôle du fabric

Comprendre pourquoi ces failles sont si dangereuses suppose de comprendre ce qu’est réellement un contrôleur SD-WAN. Dans une architecture Cisco Catalyst SD-WAN, le Manager (anciennement vManage) et le Controller constituent le plan de gestion et de contrôle : le « cerveau » qui décide comment le trafic circule entre le siège, les agences, les datacenters et le cloud. Compromettre ce cerveau, ce n’est pas pirater un serveur parmi d’autres – c’est prendre la main sur l’ensemble du système nerveux du réseau d’entreprise.

### Étape 1 : le pair malveillant et la manipulation de NETCONF

La première étape exploite le contournement d’authentification. En se faisant passer pour un pair légitime, l’attaquant fait apparaître un composant SD-WAN temporaire, contrôlé par lui, qui peut mener des actions de confiance au sein des plans de gestion et de contrôle. Via NETCONF, il peut alors lire ou modifier la configuration de n’importe quel équipement du fabric. Pour une entreprise multisite, cela signifie qu’un seul point d’entrée exposé sur Internet peut compromettre des dizaines, voire des centaines de sites distants en cascade.

### Étape 2 : le retour arrière logiciel et l’escalade vers root

Le compte obtenu via le contournement n’est pas root. Pour franchir ce dernier palier, les attaquants ont recours à une technique élégante et redoutable : le retour arrière logiciel. En s’appuyant sur le mécanisme de mise à jour intégré, ils déploient temporairement une version antérieure et vulnérable du logiciel, exploitent CVE-2022-20775 (une faille de traversée de chemin en ligne de commande, notée 7,8) pour obtenir root, puis restaurent la version d’origine. L’équipement paraît à jour, mais l’attaquant dispose désormais des privilèges les plus élevés. C’est cette manœuvre qui transforme un accès administrateur en contrôle total et furtif de la machine.

## La 7e faille : CVE-2026-20245 et l’exécution de commandes root

La vulnérabilité la plus récente, CVE-2026-20245, a été signalée à Cisco par Mandiant, la filiale de Google spécialisée dans la réponse aux incidents. D’après le Google Threat Intelligence Group, il s’agit d’une élévation de privilèges dans l’interface en ligne de commande du SD-WAN Manager : la fonction de téléversement de fichiers ne filtre pas correctement les données malveillantes. Un attaquant local authentifié peut ainsi exécuter des commandes arbitraires en tant que root.

Mandiant a documenté un cas d’exploitation réelle survenu dès avril 2026, sur l’infrastructure SD-WAN d’un fournisseur de services. L’attaquant a utilisé la commande `request tenant-upload` pour importer un fichier CSV piégé, créant un compte non autorisé baptisé `troot` doté de privilèges root :

```
# Commande observée par Mandiant (avril 2026)
request tenant-upload tenant-list /home/admin/evil_tenant.csv vpn 0
# Resultat : creation d'un compte "troot" avec privileges root
```
Le fait que CVE-2026-20245 exige un accès local authentifié n’est pas rassurant : il s’inscrit précisément dans la logique de chaîne décrite plus haut. Les contournements à 10/10 fournissent le pied dans la porte ; CVE-2026-20245 (ou CVE-2022-20775) fournit l’escalade vers root. Les chercheurs Chester Sng, Pete Boonyakarn, Logeswaran Nadarajan et Lukasz Lamparski, de Mandiant, ont mené l’investigation qui a permis d’identifier ce nouveau vecteur d’élévation.

## Un PoC public déclenche une ruée de dix groupes d’attaquants

Si UAT-8616 est l’acteur le plus discret, il est loin d’être le seul. En mars 2026, un code de démonstration (proof of concept, ou PoC) a été publié par ZeroZenX Labs. Sa mise en ligne a provoqué un effet d’aubaine immédiat : selon Tenable, au moins dix groupes d’attaquants supplémentaires se sont mis à exploiter la chaîne CVE-2026-20133 / CVE-2026-20128 / CVE-2026-20122 dans les jours qui ont suivi.

La mécanique de cette chaîne, documentée par VulnCheck, illustre comment trois failles de gravité moyenne, combinées, deviennent une arme d’accès initial :

- **CVE-2026-20133** (divulgation d’informations, non authentifiée) fait fuiter le fichier d’identifiants DCA ;
- **CVE-2026-20128** (lecture de fichier authentifiée) lit le secret utilisateur DCA ainsi exposé ;
- **CVE-2026-20122** (écrasement de fichier authentifié) téléverse un webshell via l’API.

À la différence d’UAT-8616, ces groupes opportunistes déploient des charges bruyantes : webshells, frameworks de red team détournés, mineurs de cryptomonnaie et voleurs d’identifiants. VulnCheck note d’ailleurs qu’un PoC largement partagé était mal attribué – il prétendait exploiter CVE-2026-20127 alors qu’il chaînait en réalité les trois autres failles. Cette confusion illustre un risque bien connu : la divulgation publique d’un code d’exploitation démocratise l’attaque et abaisse drastiquement le niveau de compétence requis pour compromettre un équipement exposé.

## Combien d’équipements exposés ? Les chiffres de Censys et VulnCheck

La question que se posent tous les RSSI européens est simple : combien d’équipements sont réellement atteignables depuis Internet ? Les moteurs de recherche d’exposition apportent des réponses convergentes, mais nuancées. Selon VulnCheck, Censys et Shodan dénombrent entre 450 et 550 instances de SD-WAN Manager exposées, tandis que ZoomEye en compte environ 275 et FOFA plus de 1 000. Un relevé Censys de mai 2026 portait le total à près de 2 000 équipements Catalyst SD-WAN visibles en ligne, plans de gestion et de contrôle confondus.

| Estimations d’exposition sur Internet des équipements Cisco Catalyst SD-WAN. Sources : VulnCheck, Censys. |  |  | 
|---|---|---|
| Source | Instances exposées | Périmètre | 
|---|---|---|
| Censys / Shodan | 450 – 550 | SD-WAN Manager | 
| ZoomEye | ~275 | SD-WAN Manager | 
| FOFA | > 1 000 | SD-WAN Manager | 
| Censys (mai 2026) | ~2 000 | Catalyst SD-WAN (global) | 

Ces chiffres, en apparence modestes, sont trompeurs. Le plan de gestion d’un SD-WAN n’est pas censé être exposé sur Internet : chaque instance visible représente une organisation entière – et l’ensemble de ses sites distants – à un pas d’une compromission totale. L’avis Censys sur CVE-2026-20127 souligne par ailleurs qu’une part significative des hôtes exposés laisse aussi ouverts les ports SSH (22) ou NETCONF (830), élargissant d’autant la surface d’attaque. Si la majorité des instances se concentrent aux États-Unis, l’Europe en héberge une proportion loin d’être négligeable, notamment chez les opérateurs télécoms et les grandes entreprises multisites.

## Directive d’urgence de la CISA et réponse de Cisco

La gravité de la situation a poussé l’agence américaine de cybersécurité, la CISA, à publier la directive d’urgence 26-03 le 14 mai 2026, imposant aux agences fédérales de remédier à CVE-2026-20182 dans des délais très courts, avec une échéance fixée au 17 mai. Les directives d’urgence sont rares : la CISA n’en émet que lorsqu’une menace présente un risque « inacceptable » pour les réseaux fédéraux. Chaque CVE de la campagne a par ailleurs été inscrit au catalogue KEV, ce qui, aux États-Unis, transforme le correctif en obligation légale pour les administrations.

Côté éditeur, Cisco a publié des correctifs pour l’ensemble des branches maintenues. Le tableau suivant reprend, pour la faille CVE-2026-20182, les premières versions corrigées telles que listées dans l’avis officiel. Les organisations exécutant une version antérieure à la 20.9 doivent migrer, et trois branches (20.11, 20.13 et 20.14) ont atteint leur fin de maintenance logicielle – un facteur aggravant pour les parcs vieillissants.

| Versions corrigées pour CVE-2026-20182, d’après l’avis de sécurité Cisco (publié le 14 mai, mis à jour le 16 juin 2026). |  | 
|---|---|
| Branche logicielle | Première version corrigée (CVE-2026-20182) | 
|---|---|
| 20.9 | 20.9.9.1 | 
| 20.10 / 20.11 | 20.12.7.1 | 
| 20.12 | 20.12.5.4 / 20.12.6.2 / 20.12.7.1 | 
| 20.13 / 20.14 | 20.15.5.2 | 
| 20.15 | 20.15.4.4 / 20.15.5.2 | 
| 20.16 / 20.18 | 20.18.2.2 | 
| 26.1 | 26.1.1.1 | 

Au-delà du correctif, les recommandations défensives convergent : retirer immédiatement les plans de gestion de toute exposition Internet, restreindre l’accès à NETCONF (port 830) et SSH (port 22) à des réseaux d’administration dédiés, auditer les comptes et les clés SSH ajoutés récemment, et rechercher les traces d’effacement de journaux. Une plateforme de supervision comme un SIEM open source facilite la détection de ces anomalies à l’échelle d’un parc distribué.

## La France et l’Europe en première ligne

Contrairement à ce que la concentration américaine des instances exposées pourrait laisser croire, l’Europe n’est pas spectatrice. Dès le 15 mai 2026, le CERT Santé français (Cyberveille, rattaché au ministère de la Santé) a publié une alerte sur CVE-2026-20182 à destination des établissements de santé – un secteur particulièrement dépendant du SD-WAN pour relier hôpitaux, cliniques et plateaux techniques. L’ANSSI, via son CERT-FR, suit de près ces avis Cisco dans le cadre de sa mission de protection des opérateurs d’importance vitale.

Le contexte réglementaire européen amplifie l’enjeu. Avec la directive NIS 2, des dizaines de milliers d’entités deviennent responsables de la sécurité de leur chaîne d’approvisionnement numérique et de leurs équipements réseau. Une instance SD-WAN non corrigée n’est plus seulement un risque technique : c’est un potentiel manquement de conformité, passible de sanctions. Le Cyber Resilience Act renforce encore cette logique en imposant aux fabricants et aux utilisateurs des obligations de notification et de correction dans des délais contraints.

Le tout s’inscrit dans une année noire pour la cybersécurité française. La France figure parmi les cinq pays les plus ciblés au monde, avec une hausse d’environ 29 % des incidents de rançongiciel au premier semestre 2026, et une série de compromissions retentissantes déjà couvertes dans nos colonnes, de la cyberattaque contre l’ANTS aux statistiques faisant de la France l’un des pays les plus piratés. Dans ce climat, la sécurisation de la bordure réseau devient une priorité stratégique nationale.

## ArcaneDoor, Salt Typhoon : la bordure réseau, cible des États

La campagne UAT-8616 ne surgit pas de nulle part. Elle prolonge une tendance de fond documentée depuis plusieurs années : les équipements de bordure réseau – pare-feu, VPN, routeurs, contrôleurs SD-WAN – sont devenus la cible privilégiée des acteurs étatiques. Ces appareils présentent une combinaison idéale pour l’espionnage : ils sont exposés à Internet, ils voient passer tout le trafic, ils sont rarement surveillés aussi finement que les serveurs, et ils n’acceptent généralement pas d’agent de détection.

En 2024, la campagne ArcaneDoor avait visé les pare-feu Cisco ASA et Firepower via des zero-days, dans le cadre d’opérations attribuées à un acteur étatique. En 2024-2025, le groupe Salt Typhoon, lié à la Chine, avait compromis des opérateurs télécoms occidentaux en s’appuyant précisément sur des équipements réseau pour établir un espionnage de longue durée. La campagne SD-WAN de 2026 s’inscrit dans cette continuité, avec un raffinement supplémentaire : au lieu de viser la périphérie, elle vise le plan de contrôle – le point d’orchestration depuis lequel tout le réseau peut être manipulé.

Ce glissement de la périphérie vers le cœur du réseau change la nature du risque. Compromettre un pare-feu, c’est ouvrir une brèche ; compromettre un contrôleur SD-WAN, c’est prendre le contrôle de la carte routière elle-même. Pour un adversaire cherchant à observer discrètement les communications d’une administration ou d’un opérateur d’infrastructure critique, il n’existe pas de meilleur poste d’écoute.

## Impact sur le marché : le SD-WAN, colonne vertébrale des entreprises

Cisco est le leader historique du marché du SD-WAN, une technologie devenue incontournable pour connecter les sites distants à moindre coût sans dépendre des liaisons MPLS traditionnelles. Cette domination fait de son plan de gestion une cible à très fort effet de levier : un défaut dans le produit Cisco se répercute sur des milliers d’organisations à travers le monde, des banques aux distributeurs, en passant par la santé et les services technologiques – précisément les secteurs que Mandiant identifie comme dépendants du SD-WAN.

Pour les directions informatiques, l’épisode de 2026 impose une remise en question de la gouvernance des équipements réseau. Trop d’organisations traitent encore le SD-WAN comme une « boîte » que l’on installe et que l’on oublie, avec des cycles de correctifs lents et une visibilité limitée sur les versions déployées. Le retour arrière logiciel exploité par les attaquants démontre qu’un parc « à jour » sur le papier peut rester vulnérable si le mécanisme de mise à jour lui-même n’est pas verrouillé.

L’impact se mesurera aussi sur le marché de la cyberassurance. L’exposition d’un plan de gestion réseau sur Internet, ou l’ouverture de ports NETCONF et SSH, deviendront très probablement des critères d’exclusion ou de surprime lors de la souscription. La faille Cisco SD-WAN de 2026 servira de cas d’école pour justifier des exigences de durcissement plus strictes, à l’image de ce qui s’est produit après les grandes vagues de rançongiciels.

## Comparaison : Cisco face à Fortinet et aux autres

Cisco n’est évidemment pas le seul fournisseur de SD-WAN, et il serait injuste d’en faire un cas isolé. Ses concurrents directs – Fortinet, VMware (VeloCloud), Palo Alto Networks – ont tous connu leur lot de vulnérabilités critiques sur les équipements de bordure. Fortinet, en particulier, a été frappé par plusieurs failles majeures ces dernières années sur ses passerelles, ce qui a nourri un débat de fond sur la sécurité intrinsèque des architectures d’accès distant.

Le point commun à tous ces incidents n’est pas une négligence propre à un éditeur, mais une réalité structurelle : les équipements qui concentrent le contrôle du réseau sont, par nature, des cibles de très grande valeur. Ce qui distingue la campagne Cisco de 2026, c’est le nombre de failles exploitées sur une courte période, la présence confirmée d’un acteur étatique persistant, et surtout le fait que la vulnérabilité touche le plan de contrôle plutôt qu’une simple passerelle terminale. La leçon vaut pour toute l’industrie : la comparaison entre fournisseurs compte moins que la discipline opérationnelle appliquée à leur exploitation.

Pour les équipes qui souhaitent renforcer leur posture défensive au-delà du seul correctif, des outils libres comme Wazuh (SIEM open source) ou CrowdSec permettent de détecter les comportements anormaux – création de comptes, injections de clés, effacements de journaux – qui caractérisent ce type d’intrusion furtive.

## Cinq prévisions pour la sécurité de la bordure réseau

À partir de la trajectoire observée en 2026, plusieurs évolutions apparaissent probables pour le second semestre et au-delà :

1. **D’autres zero-days SD-WAN émergeront.** La découverte de deux contournements d’authentification quasi identiques suggère une classe de défauts encore incomplètement explorée ; la prolifération des PoC accélérera les découvertes.
2. **Les régulateurs européens durciront les exigences.** Sous l’impulsion de NIS 2 et du Cyber Resilience Act, l’exposition d’un plan de gestion réseau deviendra un manquement de conformité explicitement sanctionnable.
3. **Les acteurs étatiques délaisseront la périphérie pour le plan de contrôle.** Le succès d’UAT-8616 sur le « cerveau » du réseau incitera d’autres groupes ORB à viser les contrôleurs plutôt que les seuls pare-feu.
4. **Cisco accélérera son virage vers une gestion cloud et zero-trust.** La pression poussera à retirer les plans de gestion d’Internet par défaut et à généraliser l’authentification forte des pairs.
5. **La cyberassurance intégrera l’exposition réseau comme critère.** L’ouverture de NETCONF ou SSH sur Internet deviendra un signal d’alerte à la souscription, avec surprimes ou exclusions à la clé.

### Couverture associée

## FAQ : la faille Cisco SD-WAN de 2026

### Quelles sont les failles Cisco SD-WAN les plus graves de 2026 ?

Les deux vulnérabilités les plus critiques sont CVE-2026-20127 et CVE-2026-20182, toutes deux notées 10,0 sur l’échelle CVSS. Il s’agit de contournements d’authentification affectant le Cisco Catalyst SD-WAN Controller et le Manager, exploitables à distance et sans authentification préalable. Elles permettent d’obtenir un accès hautement privilégié au plan de gestion du réseau.

### Qui est UAT-8616 ?

UAT-8616 est un acteur de menace suivi par Cisco Talos, qualifié de « hautement sophistiqué ». Il exploite des infrastructures Cisco SD-WAN depuis au moins 2023, cible les secteurs d’infrastructures critiques, et son infrastructure recoupe des réseaux de relais opérationnels (ORB), ce qui oriente l’analyse vers un acteur aligné sur un État et motivé par l’espionnage.

### Mon équipement Cisco SD-WAN est-il concerné ?

Les produits affectés sont principalement le Catalyst SD-WAN Manager (ex-vManage) et le Controller, ainsi que certains composants comme vBond, vEdge et vSmart pour la faille CVE-2022-20775. Consultez l’avis de sécurité officiel de Cisco pour vérifier votre branche logicielle et déployer la première version corrigée correspondante. Toute version antérieure à la 20.9 nécessite une migration.

### La faille Cisco SD-WAN a-t-elle touché la France ou l’Europe ?

Si la majorité des instances exposées se concentrent aux États-Unis, l’Europe héberge une part significative des équipements vulnérables. En France, le CERT Santé a publié une alerte dès le 15 mai 2026, et l’ANSSI suit les avis Cisco dans le cadre de la protection des opérateurs d’importance vitale et de la directive NIS 2.

### Que faire pour se protéger immédiatement ?

Trois actions prioritaires : appliquer sans délai les correctifs Cisco correspondant à votre version ; retirer le plan de gestion (Manager/Controller) de toute exposition sur Internet et restreindre l’accès à NETCONF (port 830) et SSH (port 22) à un réseau d’administration dédié ; auditer les comptes récents, les clés SSH ajoutées et les journaux effacés pour détecter une éventuelle compromission déjà en cours.

### Pourquoi la CISA a-t-elle émis une directive d’urgence ?

La CISA a publié la directive d’urgence 26-03 le 14 mai 2026 pour CVE-2026-20182, imposant une remédiation rapide aux agences fédérales américaines. Ces directives sont exceptionnelles et réservées aux menaces jugées « inacceptables ». Elles témoignent du niveau de gravité et de l’exploitation active de la faille au moment de sa divulgation.

### Combien d’équipements Cisco SD-WAN sont exposés sur Internet ?

Les estimations varient selon les moteurs de recherche d’exposition : entre 450 et 550 instances de SD-WAN Manager selon Censys et Shodan, environ 275 pour ZoomEye, plus de 1 000 pour FOFA, et jusqu’à près de 2 000 équipements Catalyst SD-WAN au total selon un relevé Censys de mai 2026. Chaque instance exposée représente une organisation entière potentiellement à risque.

*Article publié le 07 juillet 2026. Les scores CVSS, dates et versions correctives proviennent des avis officiels de Cisco, du catalogue KEV de la CISA et des analyses publiées par Mandiant, Tenable, VulnCheck et Censys. Les informations reflètent l’état des connaissances à la date de publication et sont susceptibles d’évoluer à mesure que l’enquête se poursuit.*
