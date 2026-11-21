---
id: collect-261121-veille-ia/windows-os/cyberchef-decoder-des-donnees-en-13-etapes-2026
title: "CyberChef : Décoder des Données en 13 Étapes [2026]"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-06-17"]
keywords: ["cyberchef", "decoder", "donnees", "gchq", "etapes", "projet", "base64", "script"]
source: https://tech-insider.org/fr/cyberchef-decoder-donnees-tutoriel-2026/
source_anchor: ""
source_lines: [1, 292]
section: "Windows_OS"
---
Un log de proxy rempli de chaînes en Base64, un script PowerShell qui ressemble à du charabia, un jeton JWT volé dans une session détournée : les analystes SOC croisent ce genre de données brutes tous les jours. En France comme ailleurs en Europe, la charge de travail ne baisse pas. Le bulletin CERT-FR du 31 août au 6 septembre 2026 recensait à lui seul une faille critique dans **JFrog Artifactory** (CVE-2026-82329, score CVSS 9,8) et une autre dans **Google Chrome** (CVE-2026-85046) déjà exploitée activement. Le Patch Tuesday de Microsoft de septembre 2026 a corrigé 973 CVE en une seule salve, dont deux zero-day confirmées en conditions réelles. Face à ce volume, disposer d’un outil capable de décoder, déchiffrer et disséquer une donnée suspecte en quelques clics n’est plus un luxe réservé aux grandes équipes de réponse à incident.

C’est exactement le rôle de **CyberChef**, le “Cyber Swiss Army Knife” développé par le **GCHQ**, l’agence de renseignement électronique britannique. Ce tutoriel montre, en 13 étapes concrètes, comment installer CyberChef, comprendre son fonctionnement par recettes, et l’utiliser pour décoder du Base64, casser un XOR, analyser un JWT suspect, désobfusquer un script PowerShell malveillant et bâtir un projet complet d’analyse de phishing. Comptez environ 60 minutes pour suivre l’ensemble du guide en pratiquant chaque étape.

## Qu’est-ce que CyberChef et pourquoi l’utiliser en 2026

CyberChef est une application web open source publiée sous licence **Apache License 2.0**, maintenue sur le dépôt GitHub **gchq/CyberChef**. Contrairement à un decoder en ligne classique qui ne fait qu’une seule conversion, CyberChef fonctionne par “recettes” : on empile plusieurs opérations (décodage Base64, puis XOR, puis extraction de texte) dans un pipeline visuel, et le résultat s’affiche en temps réel à chaque modification. L’outil regroupe des centaines d’opérations de chiffrement, d’encodage, de compression, de hachage, de parsing binaire et d’analyse forensique dans une seule interface glisser-déposer.

La dernière version stable documentée sur le dépôt officiel est **CyberChef 11.2.0**, publiée le 17 juin 2026, qui fait suite à la version majeure 11.0.0 sortie le 28 avril 2026. Le changelog de cette 11.2.0 mentionne explicitement une correction de sécurité, signe que le projet reste activement maintenu par les équipes du GCHQ malgré son âge (le projet a été rendu public en 2016). Ce qui distingue surtout CyberChef des outils concurrents, c’est son modèle d’exécution : toutes les opérations tournent **côté client, dans le navigateur**. Aucune donnée n’est envoyée vers un serveur tiers, ce qui compte énormément quand on manipule un extrait de malware, un identifiant volé ou une preuve dans une enquête judiciaire.

Pour un analyste SOC, un répondant CSIRT ou un chercheur en sécurité travaillant en France ou ailleurs en Europe, cette architecture “tout côté client” simplifie aussi la conformité RGPD : les données sensibles ne quittent jamais le poste de travail, sauf si l’on choisit explicitement de déployer une instance CyberChef sur un serveur interne. Le projet reste par ailleurs listé par le National Cyber Security Centre britannique comme l’un des outils défensifs open source de référence issus du renseignement.

L’outil a été rendu public par le GCHQ en 2016, dans une démarche assez inhabituelle pour une agence de renseignement : publier en open source un outil interne utilisé quotidiennement par ses propres analystes. Depuis, la communauté a repris le flambeau du développement aux côtés des équipes du GCHQ, avec des contributions régulières visibles sur le dépôt GitHub. Le nom “Cyber Swiss Army Knife” n’est pas usurpé : l’outil couvre à la fois les besoins d’un analyste réseau qui décortique une capture, d’un répondant à incident qui désobfusque un script, et d’un chercheur qui rétro-ingénierie un format de fichier propriétaire, le tout dans la même interface sans jamais changer d’application.

## Prérequis techniques et versions nécessaires

Avant de commencer, voici l’ensemble des prérequis matériels et logiciels. La bonne nouvelle : CyberChef ne demande ni licence, ni compte, ni carte bancaire. Le tableau ci-dessous résume les versions à utiliser selon le mode d’installation choisi (navigateur seul, Docker, ou compilation depuis les sources).

| Composant | Version recommandée | Obligatoire pour | 
|---|---|---|
| Navigateur web | Chrome, Firefox ou Edge à jour (dernières versions 2026) | Toutes les méthodes | 
| CyberChef | 11.2.0 (17 juin 2026) | Installation locale ou usage direct | 
| Docker Engine | Version récente avec support BuildKit | Installation via conteneur | 
| Node.js | Version 18 | Compilation depuis les sources | 
| npm | Version 8 | Compilation depuis les sources | 
| Git | Toute version récente | Cloner le dépôt source | 
| RAM machine hôte | 4 Go minimum, 8 Go conseillés | Traitement de gros fichiers (captures réseau, dumps) | 

La documentation officielle du wiki GitHub précise noir sur blanc la procédure de build : installer Git, installer Node.js version 18 et npm version 8 (“nvm est généralement un bon moyen de gérer ses versions de Node” selon les mainteneurs), puis cloner le dépôt. Aucune base de données, aucun compte cloud, aucune clé API n’est nécessaire pour démarrer. C’est l’un des tutoriels de cybersécurité les plus rapides à mettre en place de ce site, avec Hashcat pour l’audit de mots de passe.

## Étape 1 : Découvrir l’interface de CyberChef

Rendez-vous sur la démo officielle hébergée par le GCHQ à l’adresse gchq.github.io/CyberChef, ou lancez votre propre instance locale (voir étapes 2 et 3). L’interface se divise en quatre colonnes.

- **Operations** (colonne de gauche) : la bibliothèque de toutes les opérations disponibles, classées par catégorie (Favourites, Data format, Encryption/Encoding, Networking, Compression, Extractors, etc.) avec une barre de recherche en haut.
- **Recipe** (deuxième colonne) : la zone où l’on glisse-dépose les opérations choisies, dans l’ordre d’exécution voulu. Chaque opération devient un bloc que l’on peut paramétrer, réordonner ou désactiver sans le supprimer.
- **Input** (troisième colonne, en haut) : la donnée brute à analyser, collée à la main, importée depuis un fichier local, ou glissée directement depuis l’explorateur de fichiers.
- **Output** (troisième colonne, en bas) : le résultat de la recette appliquée à l’input, recalculé en direct à chaque modification d’un paramètre.

Un détail qui change la vie au quotidien : chaque recette peut être sauvegardée sous forme d’URL. CyberChef encode la recette entière (les opérations et leurs paramètres) dans les paramètres de l’URL de la page. Partager une méthode d’analyse à un collègue revient donc simplement à copier-coller un lien, sans jamais transmettre la donnée sensible elle-même si l’input n’a pas été inclus.

## Étape 2 : Installer CyberChef avec Docker

Pour un déploiement rapide sur un poste d’analyste ou un serveur interne isolé, Docker reste la méthode la plus simple. Le projet officiel du GCHQ fournit désormais sa propre image Docker, mais son nom exact sur Docker Hub n’est pas documenté publiquement de façon stable à ce jour. L’image communautaire la plus utilisée et maintenue en parallèle, référencée directement depuis le dépôt GitHub officiel, est **mpepping/cyberchef**, avec des builds quotidiens suivant les tags de version.

`docker run -d -p 8000:8000 --name cyberchef --restart unless-stopped mpepping/cyberchef`
Une fois le conteneur démarré, ouvrez un navigateur sur `http://localhost:8000` (ou l’adresse IP du serveur si l’installation est distante). Pour vérifier que le conteneur tourne correctement et consulter ses journaux en cas de souci :

```
docker ps --filter name=cyberchef
docker logs -f cyberchef
```
Il existe aussi des images alternatives comme **obeoneorg/cyberchef**, construites sur une base NGINX non privilégiée, qui conviennent bien à un déploiement derrière un reverse proxy d’entreprise avec TLS. Quelle que soit l’image retenue, gardez à l’esprit que le conteneur ne fait que servir l’application web statique : le calcul reste exécuté dans le navigateur du poste client, pas sur le serveur Docker lui-même.

## Étape 3 : Compiler CyberChef depuis les sources avec Node.js

Pour contribuer au projet, tester une opération personnalisée avant de la proposer en pull request, ou simplement disposer d’une version toujours à jour sans dépendre d’une image Docker tierce, la compilation depuis les sources reste la méthode recommandée par les mainteneurs eux-mêmes.

```
git clone https://github.com/gchq/CyberChef.git
cd CyberChef
npm install
npm start
```
La commande `npm start` lance un serveur de développement local (généralement accessible sur le port 8080) avec rechargement à chaud : toute modification du code source déclenche une recompilation immédiate visible dans le navigateur. Pour générer une version de production prête à être déployée sur un serveur web statique (Apache, NGINX, ou même un simple bucket S3), utilisez plutôt :

`npm run build`
Le dossier `build/prod` généré contient alors une application 100 % statique (HTML, CSS, JavaScript) qu’il suffit de copier sur n’importe quel serveur web, sans base de données ni backend applicatif à maintenir.

## Étape 4 : Décoder Base64, hexadécimal et URL encoding

Commençons par le cas le plus fréquent en analyse de logs : une chaîne encodée en Base64 planquée dans une requête HTTP suspecte ou un e-mail de phishing. Dans le panneau Operations, tapez “From Base64” dans la recherche, puis glissez-la dans la Recipe. Collez ensuite votre chaîne dans l’Input.

Exemple concret avec une charge suspecte trouvée dans un log de proxy :

```
Input : cG93ZXJzaGVsbCAtZW5jIEpBQmpBSFVBWndCbEFHNEE=
Recipe : From Base64
Output : powershell -enc JABjAHUAZwBlAG4AbwA=
```
On voit tout de suite qu’un premier décodage Base64 révèle… un second Base64 imbriqué, typique de l’obfuscation en couches employée par de nombreux droppers PowerShell. Il suffit d’empiler une deuxième opération “From Base64” dans la Recipe pour poursuivre la chaîne, sans jamais quitter l’interface. Le même principe fonctionne pour l’hexadécimal (“From Hex”), l’URL encoding (“URL Decode”) ou l’encodage HTML (“From HTML Entity”) : ce sont les briques de base que l’on combine ensuite pour des cas plus complexes.

Astuce pratique : activez l’opération “Magic” en tête de recette (voir étape 9) si vous ne savez pas quel encodage a été utilisé. Elle testera automatiquement plusieurs hypothèses et affichera un score de “lisibilité” pour chacune.

## Étape 5 : Casser un chiffrement XOR par force brute

Le XOR à clé unique reste l’une des techniques d’obfuscation les plus utilisées par les malwares pour masquer une configuration, une adresse de commande et contrôle ou une chaîne de caractères sensible dans un binaire. CyberChef propose une opération dédiée, “XOR Brute Force”, qui teste toutes les clés possibles sur un octet (0x00 à 0xFF) et affiche les résultats filtrés par un critère de “printable characters”.

- Glissez l’opération “XOR Brute Force” dans la Recipe.
- Réglez le paramètre “Crib (known plaintext string)” avec un mot que vous pensez présent dans le résultat déchiffré, par exemple “http” pour repérer une URL ou “cmd” pour une commande.
- Lancez l’analyse : CyberChef affiche uniquement les clés dont le résultat contient la chaîne recherchée, ce qui réduit immédiatement des centaines de résultats à quelques lignes exploitables.

Une fois la bonne clé identifiée grâce au brute force, remplacez l’opération par un simple “XOR” avec la clé exacte en paramètre : c’est plus rapide à exécuter et cela documente clairement, dans la recette sauvegardée, quelle clé a permis le déchiffrement. Cette démarche recoupe directement le travail habituel avec Wireshark lorsqu’on extrait un flux binaire suspect d’une capture réseau avant de le désobfusquer.

## Étape 6 : Décoder et vérifier un JSON Web Token (JWT) suspect

Les jetons JWT circulent partout dans les applications web modernes, et un jeton volé ou forgé figure souvent dans les preuves d’une compromission de session. Un JWT se compose de trois parties séparées par des points : un en-tête, une charge utile (payload) et une signature, chacune encodée en Base64 URL-safe.

```
Input : eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImlhdCI6MTc1ODAwMDAwMH0.4f9c3a1e2b8d7f6a5c4b3a2918f7e6d5c4b3a291
Recipe : JWT Decode
Output : {"alg":"HS256"}
{"sub":"admin","iat":1758000000}
```
L’opération “JWT Decode” affiche instantanément l’en-tête et la charge utile en clair, sans nécessiter la clé secrète (la partie payload n’est jamais chiffrée, seulement encodée et signée). C’est suffisant pour repérer un champ “sub” ou “role” anormal indiquant une escalade de privilèges. Pour aller plus loin et vérifier la validité de la signature elle-même, CyberChef propose également “JWT Verify” et “JWT Sign”, à condition de disposer de la clé ou du certificat correspondant. Ce type d’analyse s’articule naturellement avec un travail de threat intelligence structuré dans une plateforme comme MISP, où l’IOC extrait (ici, la valeur du champ “sub” compromis) peut être partagé avec d’autres équipes.

## Étape 7 : Désobfusquer un script PowerShell malveillant

Les scripts PowerShell obfusqués restent un vecteur d’attaque courant, souvent combinés à plusieurs couches d’encodage pour échapper aux antivirus signature-based. Un enchaînement typique observé dans les campagnes de 2026 combine Base64, compression Gzip et concaténation de chaînes.

Pour un payload encodé avec le paramètre `-EncodedCommand` de PowerShell (souvent abrégé `-enc`), la recette CyberChef standard est :

```
Recipe :
1. From Base64
2. Decode text (UTF-16LE)
```
PowerShell encode en effet ses commandes en UTF-16LE avant de les convertir en Base64, une subtilité qui piège souvent les analystes débutants quand le résultat du “From Base64” seul ressemble à du texte entrecoupé de caractères nuls illisibles. Si le script est en plus compressé (signature typique : les octets commencent par `H4sI` une fois en Base64), ajoutez une opération “Gunzip” ou “Raw Inflate” entre les deux étapes ci-dessus, selon l’algorithme de compression utilisé. Chaque nouvelle couche découverte s’ajoute simplement en bas de la Recipe, sans jamais retaper l’input : c’est tout l’intérêt du pipeline visuel face à une succession de commandes en ligne dans un terminal.

## Étape 8 : Extraire des indicateurs de compromission (IOC) depuis des logs bruts

Face à un fichier de logs volumineux (souvent plusieurs milliers de lignes), rechercher manuellement chaque adresse IP, domaine ou hash suspect prend un temps considérable. CyberChef propose une famille d’opérations “Extract” (Extract IP addresses, Extract domains, Extract email addresses, Extract hashes, Extract URLs) qui utilisent des expressions régulières optimisées pour repérer ces motifs automatiquement.

- Collez le fichier de logs complet en Input (glisser-déposer le fichier fonctionne aussi, y compris pour des fichiers de plusieurs Mo).
- Ajoutez “Extract IP addresses” à la Recipe, avec l’option “Display total” activée pour compter les occurrences.
- Enchaînez avec “Unique” pour dédupliquer la liste, puis “Sort” pour la trier.
- Terminez par “Filter” si vous souhaitez exclure les plages d’IP internes (RFC 1918) avant de transmettre la liste à un outil de threat intelligence.

Cette combinaison “Extract → Unique → Sort → Filter” constitue l’une des recettes les plus réutilisées en triage d’incident, car elle transforme un fichier brut illisible en une liste d’IOC propre en quelques secondes, prête à être injectée dans un outil de corrélation comme Suricata pour générer des règles de détection.

## Étape 9 : Utiliser Magic Wand pour identifier un encodage inconnu

Que faire quand on ne sait absolument pas comment une donnée a été encodée ? C’est le cas d’usage de l’opération “Magic”, accessible via l’icône en forme de baguette au-dessus du panneau Output. Magic applique en parallèle des dizaines de décodages courants (Base64, Hex, ROT13, Gzip, différents charsets) et note chaque résultat selon un score de vraisemblance basé sur la fréquence des caractères imprimables et la présence de mots reconnaissables.

Dans les paramètres de Magic, augmentez la valeur “Depth” (profondeur) à 3 ou 4 pour permettre à l’opération de tester des encodages imbriqués (comme le double Base64 vu à l’étape 4) sans avoir à les empiler manuellement un par un. Attention cependant : sur un gros fichier binaire, une profondeur élevée peut ralentir sérieusement le navigateur, car chaque combinaison testée consomme de la mémoire côté client. Sur des captures réseau volumineuses ou des dumps mémoire, mieux vaut réduire la profondeur à 1 ou 2 et cibler manuellement l’opération suivante une fois une piste identifiée.

## Étape 10 : Créer, sauvegarder et partager une recette CyberChef

Une fois une recette au point, deux options s’offrent pour la conserver. La première, déjà mentionnée, consiste à copier l’URL de la page : CyberChef sérialise automatiquement la Recipe (mais pas l’Input, sauf activation explicite) dans les paramètres d’URL. La seconde, plus adaptée à un référentiel d’équipe, utilise le bouton “Save recipe” en haut du panneau Recipe, qui exporte la séquence d’opérations au format JSON.

```
[
  { "op": "From Base64", "args": ["A-Za-z0-9+/=", true] },
  { "op": "Decode text", "args": ["UTF-16LE (1200)"] }
]
```
Ce fichier JSON peut être versionné dans un dépôt Git interne aux côtés des runbooks d’investigation, puis réimporté via “Load recipe” par n’importe quel membre de l’équipe SOC. C’est la méthode recommandée pour construire une bibliothèque de recettes standardisées (une par famille de malware, une par type de log source) plutôt que de redécouvrir la même chaîne d’opérations à chaque garde.

## Étape 11 : Automatiser CyberChef avec CyberChef Server (API REST)

Pour intégrer CyberChef dans un pipeline automatisé (par exemple pour traiter en masse les pièces jointes d’une boîte mail de phishing signalé, ou enrichir automatiquement les alertes d’un SIEM), le GCHQ maintient un projet séparé nommé **CyberChef-server**, disponible sur gchq/CyberChef-server. Il expose une API REST permettant d’envoyer une recette et une donnée d’entrée, puis de récupérer le résultat “baked” (cuit, dans le vocabulaire du projet) sans passer par une interface graphique.

```
git clone https://github.com/gchq/CyberChef-server
cd CyberChef-server
docker build -t cyberchef-server .
docker run -d -p 3000:3000 cyberchef-server
```
Une fois le serveur lancé, un appel HTTP suffit pour exécuter une recette de façon programmatique, par exemple depuis un script Python de triage automatique ou une règle SOAR :

```
curl -X POST http://localhost:3000/bake \
  -H "Content-Type: application/json" \
  -d '{
    "input": "cG93ZXJzaGVsbCAtZW5j",
    "recipe": [{ "op": "From Base64", "args": [] }]
  }'
```
Cette approche permet de brancher CyberChef en amont d’un outil de scan de secrets comme GitLeaks dans une chaîne CI/CD, en décodant automatiquement les valeurs encodées avant de les soumettre à la détection de fuite.

## Étape 12 : Écrire une opération JavaScript personnalisée

Quand aucune des opérations natives ne correspond exactement au besoin (un format de log propriétaire, un algorithme d’obfuscation maison rencontré chez un client), CyberChef intègre une opération générique baptisée “JavaScript”, qui exécute du code arbitraire directement sur l’Input.

```
function decodeCustomFormat(input) {
  // Retire un préfixe propriétaire "ENC:" puis inverse la chaîne
  const cleaned = input.replace(/^ENC:/, "");
  return cleaned.split("").reverse().join("");
}
return decodeCustomFormat(input);
```
Glissez l’opération “JavaScript” dans la Recipe, collez ce type de code dans son éditeur intégré, et le résultat s’affiche dans l’Output comme n’importe quelle autre opération. Pour les besoins récurrents, il est également possible de contribuer une opération permanente au projet en soumettant une pull request sur le dépôt GitHub officiel, qui documente la structure exacte attendue (fichier dans `src/core/operations/`, tests unitaires associés) dans son wiki de contribution.

## Étape 13 : Projet complet — analyser un e-mail de phishing de bout en bout

Mettons tout bout à bout sur un cas réaliste, celui d’un e-mail de phishing signalé par un utilisateur, contenant une pièce jointe HTML avec un lien obfusqué. Voici le déroulé complet, étape par étape, tel qu’on le suivrait dans un vrai triage SOC.

1. Exportez l’e-mail au format .eml et ouvrez-le dans un éditeur de texte pour en extraire les en-têtes bruts (Received, Return-Path, Authentication-Results).
2. Collez les en-têtes dans CyberChef et appliquez “Extract email addresses” pour repérer rapidement tous les expéditeurs et domaines impliqués dans la chaîne de relais.
3. Isolez le corps HTML de la pièce jointe et appliquez “Strip HTML tags” puis “Extract URLs” pour lister tous les liens présents, y compris ceux cachés dans des attributs non visibles à l’affichage.
4. Pour chaque URL suspecte utilisant un raccourcisseur ou un encodage, appliquez “URL Decode” puis “From Base64” si un paramètre de la requête semble encodé (cas fréquent des kits de phishing qui embarquent l’adresse e-mail de la victime encodée dans le lien pour pré-remplir un faux formulaire).
5. Si la pièce jointe contient un script JavaScript obfusqué, réutilisez la recette “From Base64 → Decode text” mise au point à l’étape 7, en adaptant le charset si nécessaire.
6. Calculez le hash SHA-256 de la pièce jointe complète avec l’opération “SHA2” pour le documenter dans le ticket d’incident et le comparer à des bases de réputation.
7. Sauvegardez la recette complète au format JSON (étape 10) et archivez-la avec le rapport d’incident pour que la prochaine campagne similaire soit traitée deux fois plus vite.

Ce projet illustre bien la logique de CyberChef : chaque étape isolée est triviale, mais la capacité à les enchaîner sans changer d’outil ni recopier des données d’un terminal à un autre fait gagner un temps considérable sur un triage réel, où les premières minutes après un signalement comptent souvent le plus.

## Intégrer CyberChef dans un workflow SOC

CyberChef n’a pas vocation à remplacer un SIEM, une plateforme de threat intelligence ou un système de détection réseau : il vient combler l’étape manuelle de décodage qui précède ou suit l’analyse automatisée. Dans un workflow typique, les logs bruts remontent d’abord dans un outil de collecte centralisé, les règles de détection réseau s’appuient sur des moteurs comme Suricata, les IOC confirmés sont partagés via une plateforme comme MISP, et CyberChef intervient au milieu, quand un humain doit comprendre ce que contient réellement une donnée opaque avant de décider d’une action.

Certaines équipes vont plus loin en intégrant CyberChef comme brique de traitement dans des plateformes de gestion de cas comme IntelOwl, qui référence d’ailleurs une image Docker officielle nommée `intelowlproject/intelowl_cyberchef` pour automatiser le décodage d’observables directement dans son pipeline d’analyse. Ce niveau d’intégration a du sens pour les équipes qui traitent plusieurs dizaines d’alertes par jour et ne peuvent pas se permettre de rouvrir manuellement CyberChef à chaque fois.

## Pourquoi les équipes de sécurité françaises et européennes s’y mettent en 2026

Le paysage des vulnérabilités observé sur les bulletins CERT-FR de l’été 2026 donne une idée assez nette de pourquoi ce type d’outil gagne du terrain. Entre les correctifs pour SPIP (CVE-2026-77806, CVSS 9,8), pour Papercut (CVE-2026-82078, activement exploitée), pour Citrix NetScaler (CVE-2026-19490) ou encore pour GitLab (CVE-2026-85706, score maximal de 10), les équipes de sécurité doivent analyser un volume croissant de preuves techniques brutes : extraits de logs, requêtes capturées, charges suspectes trouvées dans des tickets de support. La base de données officielle du CVE Program a d’ailleurs franchi un rythme de publication particulièrement soutenu en 2026, ce qui laisse peu de place à des méthodes d’analyse manuelles et non standardisées.

Dans ce contexte, un outil référencé par le classement OWASP Top 10 comme complément utile à l’analyse des vulnérabilités d’injection et de désérialisation, ou mobilisé pour cartographier une technique dans le référentiel MITRE ATT&CK (notamment les tactiques de Defense Evasion et d’Obfuscated Files or Information), trouve naturellement sa place dans la boîte à outils quotidienne. CyberChef ne remplace aucun de ces référentiels, mais il accélère la phase où l’on doit transformer une preuve brute en donnée exploitable pour documenter un incident selon ces standards reconnus.

## Pièges courants à éviter avec CyberChef

Avant de passer au dépannage, voici les erreurs les plus fréquentes observées chez les débutants comme chez les analystes plus expérimentés qui reprennent l’outil après une pause.

- **Coller des données sensibles réelles dans l’instance publique gchq.github.io/CyberChef.** Même si le traitement reste côté client, l’habitude est risquée en environnement professionnel : privilégiez une instance auto-hébergée pour tout ce qui touche à un incident confirmé ou des données clients.
- **Oublier l’ordre d’encodage inverse.** Pour décoder, il faut appliquer les opérations dans l’ordre inverse de l’encodage original. Un script encodé en “Compress puis Base64” se décode avec “From Base64 puis Decompress”, jamais l’inverse.
- **Confondre UTF-8 et UTF-16LE lors du décodage PowerShell.** C’est l’erreur numéro un citée à l’étape 7 : un résultat truffé de caractères nuls entre chaque lettre signale presque toujours un mauvais choix de charset.
- **Charger un fichier binaire volumineux sans adapter le mode d’affichage.** Par défaut, l’Output tente d’afficher tout résultat comme du texte, ce qui peut geler le navigateur sur un dump mémoire de plusieurs centaines de Mo. Basculez l’Output en mode “Hex” ou activez le rendu par blocs avant de charger un gros fichier.
- **Négliger la sauvegarde de la recette avant de fermer l’onglet.** Sans sauvegarde explicite (URL ou export JSON), toute la chaîne d’opérations construite pendant l’analyse est perdue à la fermeture du navigateur.

## Dépannage : les problèmes les plus fréquents et leurs solutions

Voici les incidents les plus souvent rencontrés en installation ou en usage quotidien, avec la cause probable et la correction à appliquer. La plupart de ces problèmes se règlent en moins de cinq minutes une fois la cause identifiée, mais ils reviennent régulièrement chez les nouveaux utilisateurs qui découvrent l’outil sans être passés par la documentation officielle du wiki GitHub.

| Symptôme | Cause probable | Solution | 
|---|---|---|
| Le conteneur Docker démarre puis s’arrête immédiatement | Port 8000 déjà utilisé par un autre service | Changez le mapping de port, ex. `-p 8080:8000` | 
| “npm install” échoue avec des erreurs de dépendances | Version de Node.js incompatible (trop récente ou trop ancienne) | Utilisez nvm pour installer précisément Node.js 18 avant de relancer | 
| Le navigateur devient très lent voire se fige | Fichier trop volumineux traité en mode texte avec Magic à forte profondeur | Réduisez la profondeur de Magic, basculez l’Output en Hex, ou augmentez la RAM allouée au navigateur | 
| Le décodage Base64 produit des caractères illisibles | Mauvais alphabet Base64 (variante URL-safe non standard) ou mauvais charset de sortie | Testez l’option “URL safe” dans les paramètres de “From Base64”, puis ajustez “Decode text” | 
| L’opération JWT Decode renvoie une erreur de format | Le jeton comporte des espaces ou retours à la ligne parasites collés par erreur | Appliquez d’abord “Remove whitespace” avant “JWT Decode” | 
| L’API CyberChef-server renvoie une erreur 500 | Recette JSON mal formée ou nom d’opération incorrect | Vérifiez la casse exacte du nom d’opération dans le payload JSON envoyé | 
| Le XOR Brute Force ne trouve aucun résultat lisible | La clé fait plusieurs octets, pas un seul | Utilisez “XOR” avec une clé multi-octets déduite manuellement, ou testez “XOR Brute Force” avec un crib plus court | 
| La recette sauvegardée en URL ne se recharge pas chez un collègue | URL tronquée par un client mail ou un outil de messagerie qui coupe les liens longs | Privilégiez l’export JSON de la recette plutôt que le partage d’URL pour les recettes complexes | 

## Astuces avancées pour les analystes confirmés

Une fois les bases maîtrisées, plusieurs fonctionnalités moins visibles méritent d’être exploitées au quotidien. Le mode “Auto Bake” peut être désactivé (icône en haut du panneau Output) sur les recettes très lourdes, pour ne déclencher le recalcul qu’à la demande via le bouton “Bake” plutôt qu’à chaque frappe de clavier, ce qui évite de saturer le navigateur pendant qu’on ajuste les paramètres d’une opération coûteuse.

Le raccourci clavier `Ctrl+Alt+F` (ou `Cmd+Alt+F` sur macOS) ouvre la recherche d’opérations sans quitter le clavier, un gain de temps réel une fois l’habitude prise. Les breakpoints, activables sur n’importe quelle opération de la Recipe via un clic droit, permettent d’arrêter l’exécution à une étape précise pour inspecter un résultat intermédiaire sans avoir à désactiver manuellement les opérations suivantes un par un.

Pour les équipes qui traitent régulièrement des captures réseau, la combinaison “Extract IP addresses” suivie de “Sort” et de “Frequency distribution” (une opération qui n’est pas toujours mise en avant dans les tutoriels) donne rapidement une vue statistique des IP les plus contactées, utile pour repérer un pattern de balise C2 (commande et contrôle) à intervalles réguliers avant même de sortir un outil de visualisation dédié comme Wireshark.

Autre astuce peu documentée : la catégorie “Favourites” du panneau Operations accepte le glisser-déposer depuis n’importe quelle autre catégorie. Un analyste qui répète les mêmes dix opérations chaque semaine (From Base64, XOR, Extract URLs, Decode text, JWT Decode) a intérêt à les épingler une bonne fois pour toutes, plutôt que de retaper leur nom dans la barre de recherche à chaque nouvelle session. Ce réglage se conserve localement dans le navigateur via le stockage local, donc il persiste d’une visite à l’autre sur le même poste tant que le cache n’est pas vidé manuellement.

## CyberChef face aux alternatives : tableau comparatif

CyberChef occupe une niche particulière : ni un decoder en ligne à usage unique, ni un framework d’analyse forensique complet. Voici comment il se positionne face aux options les plus proches.

| Outil | Type | Traitement des données | Coût | Cas d’usage principal | 
|---|---|---|---|---|
| CyberChef | Application web / conteneur | 100 % côté client (navigateur) | Gratuit, open source (Apache 2.0) | Décodage, encodage, analyse de données multicouches | 
| Decoder en ligne générique | Formulaire web à usage unique | Souvent côté serveur du site tiers | Gratuit | Une seule conversion simple, ponctuelle | 
| CyberChef-server (API) | Service backend REST | Côté serveur, sur infrastructure maîtrisée | Gratuit, à héberger soi-même | Automatisation dans un pipeline SOAR ou SIEM | 
| Outils CLI (base64, xxd, openssl) | Ligne de commande | Local, poste de travail | Gratuit, préinstallé sur Linux/macOS | Scripts d’automatisation, environnements sans interface graphique | 
| Suite forensique complète (type Autopsy) | Application lourde dédiée | Local, sur image disque complète | Gratuit à payant selon la suite | Analyse forensique de disques et systèmes de fichiers entiers | 

Dans la pratique, ces outils ne s’excluent pas : un analyste chevronné garde souvent un terminal ouvert pour les scripts répétitifs, une suite forensique pour l’analyse de disque complet, et CyberChef pour tout ce qui demande d’expérimenter visuellement avec un enchaînement d’opérations avant de savoir exactement quoi chercher.

## Foire aux questions

**CyberChef est-il gratuit ?**

Oui, entièrement. Le projet est publié sous licence Apache License 2.0 par le GCHQ, sans version payante, sans compte à créer et sans limite de fonctionnalités.

**Est-il prudent d’analyser un malware réel directement dans CyberChef ?**

Le décodage de texte ou de configuration extraite (chaînes, URLs, JWT) est sans risque puisque CyberChef n’exécute jamais le code du malware, il ne fait que transformer des données. En revanche, n’ouvrez jamais un binaire exécutable suspect ailleurs que dans un environnement isolé de type sandbox, indépendamment de l’outil utilisé pour l’analyse textuelle.

**Quelle est la différence entre CyberChef et un simple decoder Base64 en ligne ?**

Un decoder en ligne classique fait une seule conversion à la fois et transmet souvent la donnée à un serveur tiers. CyberChef enchaîne plusieurs opérations dans un pipeline visuel, traite tout dans le navigateur sans envoi réseau, et permet de sauvegarder la méthode complète pour la réutiliser.

**Peut-on utiliser CyberChef entièrement hors ligne ?**

Oui, à condition d’avoir installé une instance locale via Docker ou compilation depuis les sources (étapes 2 et 3). Une fois les fichiers chargés, aucune connexion internet n’est nécessaire pour utiliser l’outil.

**CyberChef peut-il remplacer un bac à sable (sandbox) d’analyse de malware ?**

Non. CyberChef traite des données statiques (texte, fichiers, configurations extraites) mais n’exécute jamais de code. Une sandbox dynamique reste indispensable pour observer le comportement réel d’un exécutable malveillant.

**Comment mettre à jour son instance CyberChef vers la dernière version ?**

Pour une installation Docker, arrêtez le conteneur, tirez la nouvelle image avec `docker pull` puis relancez-le. Pour une installation depuis les sources, faites un `git pull` suivi d’un `npm install` et `npm run build` pour reconstruire la version de production.

**CyberChef fonctionne-t-il correctement sur mobile ou tablette ?**

L’application s’affiche dans n’importe quel navigateur moderne, y compris mobile, mais l’interface en quatre colonnes glisser-déposer reste pensée pour un écran large. Pour un usage ponctuel de dépannage en déplacement, cela fonctionne. Pour un travail d’analyse quotidien, un écran d’ordinateur reste largement préférable.

**Où signaler un bug ou proposer une nouvelle opération à CyberChef ?**

Directement sur le dépôt GitHub officiel gchq/CyberChef, via un ticket “issue” détaillant le comportement observé, ou une pull request pour une contribution de code suivant les conventions documentées dans le wiki du projet.
