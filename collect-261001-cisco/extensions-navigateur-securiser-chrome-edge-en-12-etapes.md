Une extension de navigateur qui change de propriétaire du jour au lendemain, un module d’IA qui lit vos cookies de session, un correctif Chrome publié en urgence parce qu’une faille est déjà exploitée : la liste des raisons de faire le ménage dans ses extensions n’a jamais été aussi longue en 2026. Selon une étude relayée en avril 2026, les extensions dopées à l’IA présentent 60 % de risques de vulnérabilité supplémentaires par rapport aux extensions classiques, et sont trois fois plus susceptibles d’accéder à des données sensibles comme les cookies de session ou les identifiants. Ce tutoriel vous montre, étape par étape, comment auditer, restreindre et sécuriser les extensions installées sur Chrome, Edge et Firefox, avec les bons réglages, les bons outils et les pièges à éviter.

L’actualité récente donne du poids à la démarche. Le CERT-FR a confirmé le 1er septembre 2026 que la vulnérabilité CVE-2026-85046 dans Google Chrome est activement exploitée. Quelques semaines plus tôt, Chrome 152, devenu la version stable de référence le 25 août 2026 selon Supercharge Browser, corrigeait 26 failles de sécurité, dont deux vulnérabilités critiques de type use-after-free (CVE-2026-84353 et CVE-2026-84352) permettant une évasion du bac à sable ; il a depuis cédé la place à Chrome 153, lancé le 8 septembre 2026 sur un nouveau cycle de publication accéléré à deux semaines. Et un rapport publié fin août 2026 signale que 19 extensions de navigateur ont été rachetées puis « armées » avec du code malveillant, touchant potentiellement des millions d’installations actives. Autant dire que remettre à plat sa liste d’extensions n’est plus une option pour qui navigue, télétravaille ou gère des comptes sensibles depuis son navigateur.

## Pourquoi les extensions de navigateur sont devenues une cible prioritaire

Une extension de navigateur tourne avec des privilèges bien plus larges qu’une application mobile classique. Elle peut lire le contenu de chaque page visitée, intercepter les requêtes réseau, injecter du JavaScript et, selon les autorisations accordées, accéder aux cookies de session qui maintiennent vos connexions à votre banque, votre messagerie ou votre espace professionnel. Ce niveau d’accès explique pourquoi les extensions sont devenues un vecteur d’attaque à part entière, documenté noir sur blanc par le CERT-FR et le CERT Santé dans leurs bulletins hebdomadaires.

Le problème s’est aggravé avec l’arrivée des extensions dites « IA », qui promettent de résumer une page, réécrire un e-mail ou automatiser une tâche de navigation. Pour fonctionner, ces modules demandent souvent un accès complet au contenu des pages, ce qui élargit mécaniquement leur surface d’attaque. Une analyse publiée en mars 2026 a mis en évidence une vulnérabilité (CVE-2026-0628) dans le panneau Gemini Live de Chrome, corrigée dans la version 143 de janvier 2026 : des extensions malveillantes pouvaient y injecter du code JavaScript pour obtenir une escalade de privilèges dans le navigateur. La même étude a signalé des failles d’injection de prompt dans le navigateur agentique Comet, capables de détourner des gestionnaires de mots de passe et d’exfiltrer des secrets.

Autre phénomène documenté : le rachat silencieux d’extensions populaires. Un développeur indépendant vend son module à un tiers qui, quelques semaines plus tard, pousse une mise à jour intégrant du code de collecte de données ou de détournement de trafic publicitaire. Un bulletin de fin août 2026 recense 19 extensions concernées par ce scénario, marquées « ACTION_REQUISE » par les outils de suivi spécialisés. Comme les mises à jour se font automatiquement et sans confirmation explicite de l’utilisateur, ce type de dérive passe totalement inaperçu tant qu’on ne fait pas d’audit régulier.

## Prérequis avant de commencer

Ce tutoriel fonctionne sur les trois navigateurs les plus utilisés en France et en Europe. Vérifiez que vous disposez des versions suivantes ou plus récentes avant de commencer, car les pages de gestion des extensions et les fonctionnalités de sécurité décrites ci-dessous évoluent d’une version à l’autre — d’autant que Google est passé, avec Chrome 153 lancé le 8 septembre 2026, à un cycle de publication accéléré à deux semaines, ce qui resserre encore l’écart entre deux vérifications :

- Google Chrome 153 ou version ultérieure, stable depuis le 8 septembre 2026 (menu ⋮ puis Aide > À propos de Google Chrome pour vérifier)
- Microsoft Edge basé sur Chromium, version 140 ou ultérieure, avec Browser Essentials activé
- Mozilla Firefox 145 ou ultérieure
- Un compte administrateur sur la machine si vous gérez un parc d’entreprise via une stratégie de groupe
- 15 à 20 minutes pour l’audit initial, un peu plus si vous gérez plusieurs postes
- Optionnel : un compte Chrome Enterprise Browser Cloud Management ou Microsoft Edge Management Service si vous administrez plusieurs postes

Aucune extension supplémentaire n’est strictement nécessaire pour suivre ce guide, mais nous verrons plus loin des outils optionnels comme ExtensionPedia ou CRXcavator pour évaluer le niveau de risque d’un module avant de l’installer.

## Étape 1 : lister toutes les extensions installées

La première étape consiste à dresser un inventaire complet, navigateur par navigateur. C’est fastidieux mais indispensable : on ne peut pas sécuriser ce qu’on ne connaît pas. Ouvrez chaque page dédiée aux extensions dans votre navigateur :

```
Chrome  : chrome://extensions
Edge    : edge://extensions
Firefox : about:addons (rubrique Extensions)
```
Pour chaque module listé, notez trois informations : le nom de l’éditeur, la date de dernière mise à jour et les autorisations demandées. Sur Chrome et Edge, cliquez sur « Détails » pour chaque extension afin d’afficher ces éléments. Sur Firefox, un clic sur le nom du module ouvre sa fiche complète avec les permissions accordées.

Un exemple de sortie typique pour un poste de travail avec un usage courant :

```
Extension : Gestionnaire de mots de passe X
Éditeur   : Vérifié
Dernière MAJ : il y a 6 jours
Permissions : Lire et modifier toutes vos données sur tous les sites
Extension : Convertisseur PDF Rapide
Éditeur   : Non vérifié
Dernière MAJ : il y a 14 mois
Permissions : Lire et modifier toutes vos données sur tous les sites,
              Gérer vos téléchargements
```
Dans cet exemple, la seconde extension cumule deux signaux d’alerte : un éditeur non vérifié et une absence de mise à jour depuis plus d’un an, alors qu’elle réclame un accès complet à toutes les pages visitées. C’est exactement le profil que les attaquants recherchent pour racheter un module à l’abandon et y injecter du code malveillant.

## Étape 2 : évaluer le niveau de risque de chaque extension

Une fois l’inventaire terminé, il faut trier. Les critères à appliquer systématiquement sont les suivants : l’éditeur est-il identifiable et vérifié, l’extension est-elle mise à jour régulièrement, les autorisations demandées sont-elles proportionnées à la fonction annoncée, et le nombre d’utilisateurs actifs est-il cohérent avec l’ancienneté du module. Une extension à « fonction banale » (convertisseur, capture d’écran, thème visuel) qui réclame un accès à toutes les données de tous les sites doit systématiquement être questionnée : ce type de profil est régulièrement utilisé pour voler des portefeuilles de cryptomonnaies ou détourner des sessions de connexion.

Pour objectiver cette évaluation sans tout faire manuellement, des outils dédiés existent. ExtensionPedia permet de rechercher n’importe quel module et d’obtenir une note de risque basée sur l’analyse des permissions, la réputation de l’éditeur et les comportements suspects observés. Pour un usage professionnel à plus grande échelle, CRXcavator et LayerX analysent en masse les extensions installées sur un parc de postes et signalent celles qui présentent un profil de risque élevé.

| Niveau de risque | Signaux typiques | Action recommandée | 
|---|---|---|
| Faible | Éditeur vérifié, mises à jour récentes, permissions limitées au strict nécessaire | Conserver, revérifier tous les 6 mois | 
| Modéré | Éditeur identifiable mais permissions larges par rapport à la fonction | Restreindre les permissions de site, surveiller | 
| Élevé | Éditeur non vérifié, mise à jour ancienne, accès à toutes les données | Désinstaller ou remplacer par une alternative | 
| Critique | Changement d’éditeur récent, comportement réseau inhabituel signalé | Désinstaller immédiatement, changer les mots de passe utilisés dans le navigateur | 
| Extension IA générique | Accès complet au contenu des pages pour fonctionner | N’installer que depuis des éditeurs reconnus, isoler dans un profil dédié | 

## Étape 3 : restreindre les permissions au lieu de tout supprimer

Supprimer une extension utile n’est pas toujours la meilleure option. Chrome, Edge et Firefox permettent de restreindre finement l’accès d’un module à certains sites plutôt qu’à l’ensemble du web. Dans Chrome ou Edge, ouvrez la page des extensions, cliquez sur « Détails » pour le module concerné, puis dans la section « Autorisations de site », remplacez « Sur tous les sites » par « Sur des sites spécifiques » ou « Au clic ». Cette dernière option est la plus restrictive : l’extension ne s’active que lorsque vous cliquez explicitement sur son icône.

```
# Dans chrome://extensions, pour chaque extension :
1. Cliquer sur "Détails"
2. Section "Autorisations de site"
3. Choisir :
   - "Au clic"                 -> le plus restrictif, recommandé par défaut
   - "Sur des sites spécifiques" -> pour les extensions utilisées sur un périmètre connu
   - "Sur tous les sites"      -> à réserver aux extensions de confiance absolue
                                  (gestionnaire de mots de passe vérifié, par exemple)
```
Cette approche « au clic » change concrètement l’exposition d’un poste de travail. Une extension de traduction, par exemple, n’a besoin d’accéder au contenu d’une page que lorsque vous lui demandez de traduire quelque chose : il n’y a aucune raison de lui laisser un accès permanent à votre messagerie professionnelle ou à votre espace bancaire en ligne.

## Étape 4 : désactiver définitivement le mode développeur

Le mode développeur, présent sur Chrome et Edge, permet d’installer des extensions non signées en dehors du Chrome Web Store ou du Microsoft Store, une pratique appelée sideloading. C’est une fonctionnalité légitime pour les développeurs qui testent leurs propres extensions, mais c’est aussi la méthode préférée des malwares pour s’injecter directement dans le navigateur sans passer par les contrôles du magasin officiel.

Si vous n’êtes pas développeur d’extensions, désactivez ce mode. Sur la page chrome://extensions ou edge://extensions, repérez le bouton bascule « Mode développeur » en haut à droite de l’écran et vérifiez qu’il est désactivé. Si une extension inconnue apparaît dans votre liste sans que vous vous souveniez de l’avoir installée depuis le magasin officiel, c’est un signal d’alarme immédiat : désinstallez-la et changez les mots de passe utilisés récemment dans ce navigateur.

## Étape 5 : activer les outils de scan intégrés au navigateur

Les navigateurs modernes intègrent désormais leurs propres outils de détection. Sur Windows 11, Microsoft Edge dispose d’une fonctionnalité appelée Browser Essentials, capable de scanner les extensions installées pour repérer celles qui ont un impact négatif documenté sur la sécurité ou les performances. Pour l’activer, ouvrez le panneau latéral d’Edge, sélectionnez Browser Essentials, puis lancez un scan de performance et de sécurité.

Sur Chrome, l’équivalent se trouve dans les paramètres de sécurité, sous « Confidentialité et sécurité » puis « Sécurité », où l’option de navigation sécurisée avancée (Enhanced Safe Browsing) permet à Google d’analyser en temps réel les pages visitées et, indirectement, de mieux détecter les comportements suspects liés aux extensions. Activez cette option si ce n’est pas déjà fait.

```
Chrome : Paramètres > Confidentialité et sécurité > Sécurité
         > Sécurité renforcée (Enhanced Safe Browsing) > Activer
Edge   : Panneau latéral > Browser Essentials > Lancer un scan
```
## Étape 6 : mettre à jour le navigateur immédiatement

Aucun réglage d’extension ne compense un navigateur non mis à jour. Le CERT-FR a confirmé début septembre 2026 que la faille CVE-2026-85046 affectant Google Chrome est activement exploitée dans la nature. Malwarebytes recommandait fin août 2026 de mettre à jour Chrome avant même de reprendre sa navigation, tant les correctifs récents couvraient des failles critiques — un conseil d’autant plus pertinent que Chrome 152, stable depuis le 25 août 2026, a déjà été remplacé par Chrome 153 le 8 septembre 2026, lequel avait introduit dès août 2026 la nouvelle API browser.publicSuffix selon Google Chrome Developers, signe d’un rythme de publication qui ne laisse plus de place à un navigateur laissé à l’abandon. Voici la procédure de vérification pour chaque navigateur :

```
Chrome  : Menu ⋮ > Aide > À propos de Google Chrome
          -> la mise à jour se télécharge automatiquement
          -> cliquer sur "Relancer" pour l'appliquer
Edge    : Menu ... > Aide et commentaires > À propos de Microsoft Edge
          -> vérifie et installe automatiquement les mises à jour
Firefox : Menu ≡ > Aide > À propos de Firefox
          -> "Rechercher des mises à jour" puis "Redémarrer pour mettre à jour"
```
Configurez également la mise à jour automatique quand elle est disponible plutôt que de compter sur une vérification manuelle régulière. La fenêtre entre la divulgation d’une faille et son exploitation active s’est nettement réduite ces derniers mois, et attendre plusieurs jours avant d’appliquer un correctif expose inutilement le poste de travail.

## Étape 7 : cloisonner les usages sensibles dans un profil dédié

Les trois navigateurs permettent de créer plusieurs profils indépendants, chacun avec sa propre liste d’extensions, ses propres cookies et son propre historique. C’est la parade la plus efficace contre le risque résiduel d’une extension compromise : créez un profil « Banque et administratif » totalement dépourvu d’extensions, réservé exclusivement aux opérations sensibles (accès bancaire, déclarations fiscales, portail employeur), et gardez vos extensions habituelles dans un profil « Navigation courante » séparé.

```
Chrome / Edge :
1. Cliquer sur l'icône de profil en haut à droite
2. "Ajouter un profil" ou "Ajouter un compte"
3. Choisir "Continuer sans compte" pour un profil local isolé
4. Ne jamais installer d'extension dans ce profil dédié
Firefox :
about:profiles > "Créer un nouveau profil"
```
Cette séparation limite mécaniquement l’impact d’une extension malveillante : même si un module compromis parvient à lire les cookies de session du profil de navigation courante, il n’aura aucun accès à la session ouverte dans le profil bancaire isolé, puisque les deux environnements ne partagent ni cookies ni stockage local.

## Étape 8 : mettre en place une liste blanche pour un parc de postes

Pour une entreprise ou une administration, l’audit manuel poste par poste ne tient pas dans la durée. La recommandation qui s’impose depuis 2026 consiste à basculer d’une logique de liste noire (bloquer les extensions connues comme dangereuses) vers une logique de liste blanche (n’autoriser que les extensions explicitement validées). Chrome Enterprise Browser Cloud Management et Microsoft Edge Management Service permettent tous les deux de définir centralement quelles extensions sont autorisées sur l’ensemble d’un parc, et de bloquer automatiquement toute installation en dehors de cette liste.

Concrètement, la démarche recommandée est la suivante : inventorier toutes les extensions actuellement installées sur les postes qui accèdent à des données personnelles ou professionnelles sensibles, valider une liste restreinte avec l’équipe sécurité, puis pousser une stratégie de groupe qui bloque tout le reste. Chaque changement d’éditeur d’une extension déjà présente sur la liste blanche doit être traité comme un événement à réévaluer, et non comme une simple mise à jour de routine.

## Étape 9 : surveiller le comportement réseau des extensions

Un module compromis ne se contente généralement pas de lire des données localement : il les envoie quelque part. La surveillance des connexions réseau sortantes inhabituelles, via une solution comme Microsoft Defender sur les postes de travail, permet de repérer une extension qui communique avec un serveur externe alors que sa fonction annoncée ne le justifie pas. Sur un poste personnel, les pare-feu applicatifs intégrés à la plupart des suites de sécurité offrent une granularité suffisante pour identifier ce type de comportement.

Pour les extensions qui demandent la permission technique « declarativeNetRequest », un audit renforcé est recommandé : cette autorisation donne à l’extension la capacité de modifier ou de bloquer des requêtes réseau, ce qui peut être détourné pour rediriger du trafic ou injecter du contenu publicitaire malveillant sans que l’utilisateur ne s’en aperçoive.

## Étape 10 : vérifier régulièrement les extensions dopées à l’IA

Les extensions présentées comme « assistants IA », capables de résumer, traduire ou automatiser une navigation, méritent un traitement à part. Leur promesse fonctionnelle nécessite presque toujours un accès étendu au contenu des pages, ce qui rend l’évaluation du risque plus complexe qu’avec une extension classique. La règle pratique : n’installer ce type de module que depuis des éditeurs identifiables (grands éditeurs de navigateurs, entreprises connues du secteur de l’IA), vérifier la politique de traitement des données affichée sur la fiche du magasin d’extensions, et réévaluer périodiquement si l’éditeur reste le même.

Le cas du navigateur agentique Comet, documenté début 2026, illustre le risque spécifique de cette catégorie : une vulnérabilité d’injection de prompt pouvait détourner un gestionnaire de mots de passe intégré et exfiltrer des identifiants sans action explicite de l’utilisateur. La mise à jour vers les versions publiées après février 2026 corrige ce point précis, mais le principe reste valable pour tout navigateur ou extension combinant capacités d’IA générative et accès large au contenu des pages.

## Étape 11 : nettoyer les extensions abandonnées

Une extension qui n’a pas reçu de mise à jour depuis plus d’un an, même si elle n’a jamais montré de comportement suspect, représente un risque latent. Un éditeur qui abandonne un projet devient une cible facile pour un rachat discret, précisément le scénario documenté avec les 19 extensions « armées » signalées fin août 2026. La règle simple à appliquer : toute extension non mise à jour depuis 12 mois et non indispensable doit être désinstallée, quitte à la réinstaller plus tard si le besoin réapparaît et si l’extension a repris un développement actif.

```
# Vérification manuelle, à répéter tous les 3 mois
Pour chaque extension installée :
  Si (dernière mise à jour > 12 mois) ET (usage non quotidien) :
      -> Désinstaller
  Si (changement d'éditeur détecté) :
      -> Désinstaller immédiatement + changer les mots de passe du navigateur
  Si (permissions > besoin réel de la fonction) :
      -> Restreindre à "Au clic"
```
## Étape 12 : documenter et planifier l’audit suivant

La sécurisation des extensions n’est pas une opération ponctuelle mais un cycle. Notez la date de votre audit, la liste des extensions conservées et leur niveau de risque évalué, puis fixez une date de rappel à trois mois. Pour un usage personnel, un simple rappel dans l’agenda suffit. Pour une organisation, ce cycle doit être intégré au processus de gestion des vulnérabilités déjà en place pour les autres composants du système d’information, au même titre que les correctifs serveurs ou les mises à jour d’antivirus.

## Le projet complet : script d’audit et checklist réutilisable

Voici un récapitulatif complet, sous forme de checklist, à suivre dans l’ordre pour un audit initial complet des extensions sur un poste Windows, macOS ou Linux :

```
CHECKLIST D'AUDIT DES EXTENSIONS DE NAVIGATEUR - 2026
[ ] 1. Ouvrir chrome://extensions, edge://extensions et about:addons
[ ] 2. Lister éditeur, date de MAJ et permissions de chaque extension
[ ] 3. Vérifier chaque extension sur ExtensionPedia (ou équivalent)
[ ] 4. Désinstaller les extensions non utilisées depuis 12+ mois
[ ] 5. Désinstaller les extensions à éditeur non vérifié + permissions larges
[ ] 6. Passer les extensions restantes en "Au clic" quand c'est possible
[ ] 7. Désactiver le mode développeur sur Chrome et Edge
[ ] 8. Activer Enhanced Safe Browsing (Chrome) et Browser Essentials (Edge)
[ ] 9. Mettre à jour le navigateur vers la dernière version stable
[ ] 10. Créer un profil dédié sans extension pour les usages sensibles
[ ] 11. Pour une entreprise : basculer sur une liste blanche centralisée
[ ] 12. Noter la date de l'audit et planifier le suivant à J+90
```
Ce projet, une fois mis en place, prend moins de 20 minutes à exécuter lors des audits suivants, contre 45 à 60 minutes pour le premier passage sur un poste chargé en extensions accumulées au fil des années.

## Résultats attendus après un audit complet

Sur un poste de travail type, comportant entre 15 et 25 extensions installées au fil du temps, un audit complet aboutit généralement à la suppression de 30 à 50 % des modules, soit parce qu’ils ne sont plus utilisés, soit parce que leurs permissions sont disproportionnées par rapport à leur fonction réelle. Un exemple de rapport final après audit :

```
Résumé de l'audit - Poste "PC-Bureau-01"
Date : 06/09/2026
Extensions initiales      : 21
Extensions supprimées     : 9
  - 6 non utilisées depuis 12+ mois
  - 2 éditeur non vérifié + permissions larges
  - 1 changement d'éditeur détecté (action critique)
Extensions conservées     : 12
  - 8 passées en mode "Au clic"
  - 4 conservées en accès complet (gestionnaires vérifiés)
Mode développeur          : Désactivé
Navigateur                : À jour (dernière version stable)
Prochain audit programmé  : 06/12/2026
```
## 5 pièges fréquents à éviter

**Premier piège : confondre popularité et sécurité.** Une extension téléchargée par des millions d’utilisateurs n’est pas automatiquement sûre. Le nombre d’installations reflète la notoriété au moment du lancement, pas le comportement actuel du code après un éventuel rachat de l’éditeur.

**Deuxième piège : ignorer les mises à jour silencieuses.** Les extensions se mettent à jour automatiquement sans demander de confirmation explicite à chaque changement de permissions, contrairement aux applications mobiles. Un module qui demandait un accès limité à son installation peut réclamer un accès complet six mois plus tard sans qu’aucune alerte visible ne s’affiche.

**Troisième piège : laisser le mode développeur activé « au cas où ».** Ce réglage, souvent activé une fois pour tester une extension personnelle puis oublié, reste une porte ouverte au sideloading de code malveillant pendant des mois, voire des années.

**Quatrième piège : traiter toutes les extensions IA de la même façon.** Un module IA d’un éditeur reconnu et transparent sur sa politique de données n’a pas le même profil de risque qu’une extension IA obscure sortie de nulle part avec des avis achetés. Le nom « IA » dans la description ne dit rien du niveau de sécurité réel.

**Cinquième piège : ne pas revenir sur l’audit après l’avoir fait une fois.** La sécurité des extensions se dégrade avec le temps : nouvelles failles découvertes, changements d’éditeur, extensions oubliées qui continuent de tourner en arrière-plan. Un audit unique sans suivi perd son intérêt en quelques mois.

## Comparatif des fonctionnalités de sécurité par navigateur

| Fonctionnalité | Chrome | Edge | Firefox | 
|---|---|---|---|
| Restriction de permission “Au clic” | Oui | Oui | Partielle (via extensions tierces) | 
| Scan intégré des extensions | Enhanced Safe Browsing | Browser Essentials | Non natif | 
| Gestion centralisée entreprise | Chrome Enterprise Browser Cloud Management | Microsoft Edge Management Service | Firefox Enterprise Policies | 
| Profils multiples isolés | Oui | Oui | Oui (about:profiles) | 
| Blocage du mode développeur | Via stratégie de groupe | Via stratégie de groupe | Via about:config | 

## Conseils avancés pour aller plus loin

Pour les utilisateurs avancés et les équipes IT, quelques réglages supplémentaires renforcent nettement la posture de sécurité. Sur Chrome et Edge, la stratégie de groupe ExtensionInstallBlocklist permet de bloquer explicitement des identifiants d’extensions connus comme malveillants, en complément de la liste blanche. Sur Firefox, l’option de télémétrie des modules complémentaires (about:config, clé extensions.webextensions.restrictedDomains) permet d’empêcher certaines extensions de s’exécuter sur des domaines sensibles définis manuellement, comme les portails bancaires ou les intranets d’entreprise.

Autre pratique utile : conserver une machine virtuelle ou un conteneur de navigation jetable pour tester une nouvelle extension avant de l’installer sur le poste principal. Cette approche, courante dans les équipes de sécurité, permet d’observer le comportement réseau d’un module inconnu pendant 48 heures sans exposer les données réelles de l’utilisateur.

Enfin, pour les organisations soumises à la directive NIS2 ou traitant des données personnelles au sens du RGPD, l’audit des extensions de navigateur mérite d’être formalisé dans la politique de sécurité des systèmes d’information, avec une fréquence de révision alignée sur celle des autres inventaires logiciels du parc.

## Dépannage : 8 problèmes courants et leurs solutions

**1. Une extension refuse de se désinstaller.** Certaines extensions poussées par une stratégie de groupe d’entreprise ne peuvent pas être supprimées manuellement. Vérifiez dans chrome://policy si l’extension est listée comme forcée, auquel cas la demande de suppression doit passer par l’administrateur IT.

**2. Le mode “Au clic” casse le fonctionnement d’une extension.** Certains modules ont besoin d’un accès permanent pour fonctionner correctement (bloqueurs de publicité en temps réel, par exemple). Dans ce cas, préférez « Sur des sites spécifiques » plutôt que « Sur tous les sites », en listant uniquement les domaines réellement utilisés.

**3. ExtensionPedia ou un outil similaire ne trouve pas une extension récente.** Les bases de données de risque mettent parfois plusieurs semaines à référencer les nouvelles extensions. En l’absence de données, appliquez le principe de précaution : limitez les permissions au strict minimum en attendant.

**4. Le mode développeur se réactive tout seul après une mise à jour du navigateur.** Certaines mises à jour majeures réinitialisent des paramètres avancés. Reprenez l’habitude de vérifier ce réglage après chaque mise à jour importante de Chrome ou Edge.

**5. Impossible de créer un second profil de navigateur.** Sur les postes gérés en entreprise, la création de profils supplémentaires peut être bloquée par une stratégie de groupe. Contactez le support IT pour obtenir un profil isolé dédié aux usages sensibles.

**6. Une extension légitime déclenche une alerte de sécurité après une mise à jour.** Vérifiez d’abord si l’éditeur a changé sur la fiche du magasin d’extensions. Si l’éditeur reste identique et que l’alerte semble être un faux positif, consultez les avis récents d’autres utilisateurs avant de réinstaller.

**7. Le navigateur refuse d’installer la dernière mise à jour de sécurité.** Un espace disque insuffisant ou une stratégie de groupe obsolète bloquent parfois les mises à jour automatiques. Vérifiez l’espace disponible et, sur un poste d’entreprise, signalez le blocage à l’équipe IT sans attendre.

**8. Impossible de savoir si une extension a réellement changé d’éditeur.** Comparez le nom du développeur affiché sur la fiche actuelle du magasin avec une capture d’écran ou une note prise lors de l’installation initiale. En l’absence de trace, désinstallez puis réinstallez l’extension depuis zéro pour repartir sur une base de permissions à jour et consciente.

## Cas concret : comment une extension légitime bascule dans la malveillance

Le scénario mérite d’être détaillé, car il explique pourquoi un audit ponctuel ne suffit jamais. Tout commence généralement par un développeur indépendant qui publie une extension utile, gratuite, sans intention malveillante : un bloqueur de fenêtres pop-up, un outil de capture d’écran ou un gestionnaire d’onglets. L’extension gagne en popularité sur plusieurs mois, parfois plusieurs années, accumulant des centaines de milliers d’installations actives. Puis, faute de temps ou de motivation pour continuer à maintenir le projet, le développeur accepte une offre de rachat d’un tiers, souvent présenté comme une simple société de maintenance logicielle.

Le nouvel éditeur publie alors une mise à jour, en apparence anodine, qui ajoute une nouvelle permission au manifeste de l’extension : accès aux requêtes réseau, lecture du contenu de toutes les pages, ou injection de scripts supplémentaires. Cette mise à jour s’installe automatiquement chez tous les utilisateurs existants, sans qu’aucune fenêtre de confirmation explicite n’apparaisse dans la majorité des cas, puisque l’utilisateur a déjà accordé une confiance initiale à l’extension. C’est précisément ce mécanisme qui a été documenté fin août 2026 pour les 19 extensions rachetées puis armées : le changement d’éditeur est passé inaperçu pendant plusieurs semaines avant d’être signalé par des chercheurs en sécurité.

Ce cas concret illustre pourquoi la vérification de l’identité de l’éditeur ne peut pas être un contrôle réalisé une seule fois à l’installation. C’est un point de contrôle qui doit être répété à chaque audit périodique, en comparant le nom de l’éditeur actuel avec celui noté lors de l’installation initiale. Les outils de gestion centralisée comme LayerX automatisent cette comparaison à l’échelle d’un parc entier de postes, en envoyant une alerte dès qu’un changement d’éditeur est détecté sur une extension déjà déployée, ce qui réduit la fenêtre d’exposition de plusieurs semaines à quelques heures.

## Extensions de protection à considérer pour renforcer son navigateur

Paradoxalement, certaines extensions permettent justement de renforcer la sécurité du navigateur plutôt que de l’affaiblir, à condition de les choisir avec le même niveau d’exigence que celui appliqué au reste de l’inventaire. Les bloqueurs de traceurs et de scripts malveillants filtrent une partie du trafic publicitaire avant même qu’il n’atteigne la page affichée, ce qui réduit la surface d’attaque exposée aux campagnes de publicité malveillante (malvertising). Ces extensions doivent elles-mêmes provenir d’éditeurs identifiables et faire l’objet de mises à jour régulières, le même paradoxe de confiance s’appliquant à un outil de sécurité qu’à n’importe quel autre module.

Les gestionnaires de mots de passe intégrés à des solutions reconnues constituent une autre catégorie d’extension à privilégier plutôt qu’à bannir : ils réduisent le risque de réutilisation de mots de passe faibles, un facteur de compromission bien plus fréquent que celui d’une extension malveillante isolée. La condition reste la même : vérifier que l’éditeur est établi, que le code fait l’objet d’audits de sécurité publiés, et que l’historique de mises à jour est régulier et documenté.

Enfin, des extensions dédiées à la vérification de la réputation des sites visités, qui croisent l’URL affichée avec des bases de données de sites frauduleux connus, complètent utilement les protections déjà intégrées aux navigateurs modernes. Elles ne remplacent pas une vigilance de base face au phishing, mais ajoutent une couche de vérification automatisée qui a fait ses preuves dans les entreprises appliquant une politique de sécurité en profondeur, où plusieurs mécanismes de protection se chevauchent délibérément.

## Sources et références officielles

Pour suivre l’actualité des vulnérabilités touchant les navigateurs, consultez régulièrement les avis de sécurité du CERT-FR, qui publie chaque semaine les failles critiques identifiées sur les logiciels les plus répandus. Le blog sécurité de Malwarebytes détaille régulièrement les correctifs Chrome à appliquer en priorité. Pour la gestion des extensions, les pages d’aide officielles restent la référence la plus fiable : Google Chrome, Microsoft Edge et Mozilla Firefox. Enfin, pour le contexte européen sur la cybersécurité liée à l’IA, l’ENISA publie des lignes directrices régulièrement mises à jour.

## Foire aux questions

**Faut-il désinstaller toutes ses extensions par précaution ?**

Non, ce n’est ni nécessaire ni réaliste. L’objectif est de conserver les extensions utiles en limitant leurs permissions au strict nécessaire, et de supprimer celles qui présentent un risque disproportionné par rapport à leur usage réel.

**Les extensions du Chrome Web Store sont-elles automatiquement sûres ?**

Non. Le passage par le magasin officiel réduit le risque par rapport au sideloading, mais n’élimine pas la possibilité qu’une extension légitime soit rachetée plus tard et modifiée pour intégrer du code malveillant lors d’une mise à jour.

**Quelle est la différence entre “Au clic” et “Sur des sites spécifiques” ?**

« Au clic » exige une action explicite de l’utilisateur à chaque utilisation de l’extension sur une page. « Sur des sites spécifiques » donne un accès permanent, mais limité à une liste de domaines définie manuellement. Le premier réglage est plus restrictif et donc plus sûr par défaut.

**Les extensions IA sont-elles plus dangereuses que les extensions classiques ?**

Selon les données disponibles en 2026, les extensions dopées à l’IA présentent un taux de vulnérabilité supérieur de 60 % et un risque d’accès à des données sensibles trois fois plus élevé que les extensions classiques, principalement à cause des permissions étendues nécessaires à leur fonctionnement.

**À quelle fréquence faut-il refaire cet audit ?**

Tous les trois mois pour un usage personnel suffit dans la plupart des cas. Pour une organisation soumise à des exigences de conformité comme NIS2, l’audit doit être intégré au cycle de gestion des vulnérabilités existant, généralement mensuel ou trimestriel selon la criticité des postes.

**Faut-il désactiver complètement le mode développeur même pour un usage occasionnel ?**

Si vous devez tester une extension non publiée sur le magasin officiel, activez le mode développeur temporairement, effectuez le test, puis désactivez-le immédiatement après. Le laisser actif en permanence multiplie inutilement la fenêtre d’exposition au sideloading.

**Un profil de navigateur sans extension est-il vraiment plus sûr pour les opérations bancaires ?**

Oui, car il élimine mécaniquement le risque qu’une extension compromise intercepte les données de session sur ce profil précis, les cookies et le stockage local n’étant pas partagés entre profils.

**Les outils comme ExtensionPedia ou CRXcavator sont-ils gratuits ?**

ExtensionPedia propose une consultation gratuite pour vérifier une extension individuelle. CRXcavator et LayerX ciblent davantage un usage professionnel à l’échelle d’un parc de postes, avec des offres adaptées à la gestion centralisée en entreprise.