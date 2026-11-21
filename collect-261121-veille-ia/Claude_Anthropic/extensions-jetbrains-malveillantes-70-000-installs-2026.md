---
id: collect-261121-veille-ia/claude-anthropic/extensions-jetbrains-malveillantes-70-000-installs-2026
title: "Extensions JetBrains Malveillantes : 70 000 Installs [2026]"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-06-16"]
keywords: ["jetbrains", "extensions", "malveillantes", "stepsecurity", "selon", "juin", "cles", "plugins"]
source: https://tech-insider.org/fr/jetbrains-extensions-ia-malveillantes-2026/
source_anchor: ""
source_lines: [1, 152]
section: "Claude_Anthropic"
---
Le 16 juin 2026, JetBrains a confirmé avoir retiré 15 extensions malveillantes de son Marketplace après avoir découvert qu’elles se faisaient passer pour des outils d’intelligence artificielle afin de voler les clés API de leurs utilisateurs. Selon l’éditeur, ces extensions ont cumulé environ 70 000 installations avant leur suppression, dont plus de 53 000 pour les deux plugins les plus récents. La campagne, active depuis fin octobre 2025 selon les chercheurs de StepSecurity, a fonctionné pendant près de huit mois avant d’être neutralisée.

L’incident dépasse le simple cas isolé. Il s’inscrit dans une vague plus large d’attaques visant les marketplaces d’extensions pour IDE, où des chercheurs ont recensé des centaines de millions d’installations d’extensions compromises depuis 2025, rien que sur le Marketplace de Visual Studio Code. Pour les développeurs qui ont fait des assistants IA un outil de travail quotidien, l’affaire des extensions JetBrains malveillantes illustre un nouveau front : le vol de clés API, porte d’entrée vers des services IA payants et parfois vers des systèmes d’entreprise entiers.

## Chronologie d’une campagne restée invisible huit mois

Selon les informations publiées par JetBrains et confirmées par les chercheurs de StepSecurity, la campagne a débuté fin octobre 2025 sous couvert de comptes éditeurs en apparence légitimes. Les attaquants ont publié des extensions se présentant comme des assistants IA pour la revue de code, la génération de tests unitaires ou le chat, en reprenant des noms proches d’outils réels associés à OpenAI, DeepSeek et SiliconFlow, notamment des plugins baptisés DeepSeek AI Assist, DeepSeek Junit Test ou DeepSeek Git Commit.

Le 16 juin 2026, JetBrains indique avoir reçu des signalements concernant 15 de ces extensions. L’entreprise réagit le jour même avec un retrait immédiat du Marketplace, un blocage des comptes éditeurs associés et une désactivation à distance des plugins déjà installés. Trois jours plus tard, le 19 juin 2026, StepSecurity constate que le serveur utilisé pour récupérer les clés volées répond toujours aux requêtes, signe que l’infrastructure de l’attaquant n’a pas été démantelée, seulement coupée de ses cibles JetBrains.

## Comment les extensions malveillantes siphonnaient les clés API

Le mécanisme décrit par StepSecurity reste simple et volontairement discret. Une fois l’extension installée, l’utilisateur saisissait sa clé API dans les paramètres du plugin, comme il l’aurait fait avec n’importe quel assistant IA légitime, puis cliquait sur le bouton Apply. À cet instant, la clé partait en clair, via une requête HTTP non chiffrée, vers un serveur codé en dur dans l’extension.

Aucune fenêtre de consentement, aucun message d’erreur : le plugin fonctionnait normalement en façade. Cela explique en partie pourquoi la supercherie a pu tenir huit mois sans déclencher d’alerte visible chez les utilisateurs. Pour un attaquant, une clé API dérobée permet d’utiliser à distance les crédits d’un compte IA payant, d’accéder à des historiques de requêtes selon le fournisseur, voire de rebondir vers d’autres services si la même clé ou les mêmes identifiants sont réutilisés ailleurs, une pratique encore courante chez les développeurs pressés.

## Un serveur de commande et contrôle basé à Pékin

Les analystes de StepSecurity ont tracé le point de chute des données volées jusqu’à une adresse IP unique, 39.107.60.51, hébergée sur Alibaba Cloud à Pékin. Cette même adresse revenait dans le code des 15 extensions incriminées, un indice technique qui a permis de relier l’ensemble de la campagne à une source unique malgré la diversité des noms de plugins et des comptes éditeurs utilisés.

JetBrains a publié cette adresse IP dans son bulletin de sécurité et recommande aux administrateurs de l’ajouter à leurs listes de blocage pare-feu ou DNS. Selon StepSecurity, le serveur restait joignable et répondait aux requêtes le 19 juin 2026, trois jours après la purge côté JetBrains. Un rappel utile : retirer des extensions d’un marketplace ne désactive pas l’infrastructure qui les pilotait depuis l’étranger.

## La réponse de JetBrains : purge, bannissement et kill-switch à distance

Dans son bulletin intitulé *Addressing Malicious Third-Party AI Plugins*, publié le 16 juin 2026, JetBrains détaille trois actions immédiates : le retrait des 15 extensions du Marketplace, la suspension permanente de 7 comptes éditeurs, et l’activation d’un kill-switch à distance marquant les plugins comme défectueux afin qu’ils se désactivent automatiquement au prochain redémarrage de l’IDE, même chez les utilisateurs qui n’auraient pas vu l’alerte.

L’éditeur affirme également qu’aucun code source interne, environnement de développement ou système d’infrastructure JetBrains n’a été compromis dans l’incident. Les extensions malveillantes ciblaient exclusivement les clés API que les utilisateurs saisissaient eux-mêmes dans les paramètres du plugin, pas les systèmes de JetBrains. L’entreprise recommande à toute personne ayant installé l’un des 15 plugins de considérer sa clé API comme compromise et de la régénérer sans délai auprès de son fournisseur IA.

## L’ampleur du problème : un mal endémique aux marketplaces d’extensions

L’affaire des extensions JetBrains malveillantes ne sort pas de nulle part. Les marketplaces d’extensions pour IDE sont devenues une cible privilégiée depuis 2025, avec des volumes d’installation qui donnent le vertige comparés aux 70 000 du cas JetBrains.

| Incident | Marketplace | Extensions concernées | Installations touchées | Période | 
|---|---|---|---|---|
| Plugins IA piégés JetBrains | JetBrains Marketplace | 15 extensions | ~70 000 | Oct. 2025 à juin 2026 | 
| « ChatGPT – 中文版 » et ChatMoss/CodeMoss | VS Code Marketplace | 2 extensions | ~1,5 million | 2025 | 
| Extensions à dépendances malveillantes connues | VS Code Marketplace | 1 283 extensions | 229 millions | 2025 | 
| Extensions jugées potentiellement dangereuses | VS Code Marketplace | 2 969 extensions (5,61 % de 52 880 analysées) | 613 millions (cumulées) | 2025 | 
| Extensions exposant des jetons d’accès | VS Code Marketplace | 100+ extensions | 85 000+ | Octobre 2025 | 
| Extensions avec malware dans les dépendances | VS Code Marketplace | 19 extensions | Non communiqué | Févr. à déc. 2025 | 
| BigBlack.bitcoin-black et BigBlack.codo-ai | VS Code Marketplace | 2 extensions | 41 (16 + 25) | Décembre 2025 | 

Ces chiffres montrent une asymétrie structurelle. Les marketplaces d’extensions fonctionnent sur un modèle de publication rapide suivi d’une modération après coup, où le signalement communautaire ou la recherche externe joue souvent un rôle aussi important que la vérification automatisée en amont. Le cas des extensions JetBrains malveillantes, avec ses 70 000 installations en huit mois, reste modeste comparé aux 613 millions d’installations cumulées que Koi Security attribue à l’ensemble des extensions potentiellement dangereuses recensées sur le Marketplace de VS Code. En octobre 2025 déjà, The Hacker News rapportait qu’une centaine d’extensions VS Code exposaient des jetons d’accès, et en décembre 2025, Infosecurity Magazine détaillait une campagne distincte logeant du code malveillant dans les dépendances de 19 extensions. Le problème touche donc tout l’écosystème des extensions IA, pas une seule plateforme.

## JetBrains Marketplace, VS Code Marketplace, Open VSX : le comparatif sécurité

Face à ce type de campagne, la question naturelle est de savoir si une marketplace protège mieux ses utilisateurs qu’une autre. La réponse, sur la base des informations rendues publiques, reste nuancée : aucune des trois grandes plateformes d’extensions pour IDE ne documente publiquement un contrôle manuel systématique avant publication.

| Critère | JetBrains Marketplace | VS Code Marketplace | Open VSX Registry | 
|---|---|---|---|
| Modèle de contrôle | Signalement puis retrait a posteriori | Signalement (« Report a concern ») puis retrait | Non détaillé publiquement | 
| Désactivation à distance des plugins déjà installés | Oui, kill-switch confirmé en juin 2026 | Retrait du Marketplace, désactivation non systématique | Non détaillé publiquement | 
| Extensions malveillantes retirées (connu) | 15 (une campagne) | 110+ (année 2025) | Non communiqué publiquement | 
| Installations cumulées touchées (connu) | ~70 000 | Plusieurs centaines de millions cumulées, selon plusieurs études | Non communiqué publiquement | 

Microsoft a par exemple retiré plus de 110 extensions malveillantes du VS Code Marketplace au cours de la seule année 2025, un chiffre qui illustre le volume que la modération après coup doit absorber. JetBrains, de son côté, a démontré avec l’incident de juin 2026 qu’elle dispose d’un mécanisme de désactivation à distance capable de neutraliser des plugins déjà installés, une capacité que tous les gestionnaires d’extensions n’annoncent pas aussi clairement. Pour Open VSX, le registre ouvert utilisé notamment par des forks de VS Code, les politiques de vérification détaillées ne sont pas documentées publiquement de façon comparable, ce qui rend un comparatif chiffré difficile à établir avec rigueur.

## Pourquoi les assistants de code IA sont devenus une cible prioritaire

Les assistants IA pour le développement sont passés, en l’espace de deux ou trois ans, du statut de gadget testé par curiosité à celui d’outil que beaucoup de développeurs ouvrent plusieurs fois par jour. Cette adoption a un revers direct : chaque assistant IA repose sur une clé API, souvent reliée à un compte facturé à l’usage, parfois connectée à des dépôts de code privés ou à des environnements d’entreprise.

Pour un attaquant, imiter un outil IA plutôt qu’une extension généraliste présente un double avantage. La demande est forte, car les développeurs cherchent activement de nouveaux plugins IA et acceptent d’en tester plusieurs. La configuration elle-même y aide aussi : entrer une clé API dans un champ de paramètres est un geste routinier, presque automatique, que la campagne JetBrains a exploité sans avoir besoin de le modifier. Le fait que les extensions incriminées reprenaient des noms proches de marques réelles comme DeepSeek plutôt que d’inventer une identité de toutes pièces renforçait la confiance au moment de l’installation.

## L’impact pour les développeurs et les entreprises en France et en Europe

JetBrains n’a pas communiqué de répartition géographique des 70 000 installations concernées, et aucune entreprise française ou européenne n’a été identifiée publiquement à ce stade comme victime confirmée. L’exposition potentielle reste néanmoins réelle : les IDE JetBrains (IntelliJ IDEA, PyCharm, WebStorm, GoLand, Rider) comptent parmi les environnements de développement les plus utilisés dans les entreprises technologiques européennes, du secteur bancaire aux éditeurs de logiciels.

Pour une équipe technique, le risque ne se limite pas au coût direct d’une clé API réutilisée par un tiers. Une clé exposée peut ouvrir l’accès à des historiques de conversation avec l’assistant IA, susceptibles de contenir des extraits de code propriétaire collés pour obtenir de l’aide, ou servir de point d’entrée si l’organisation partage les mêmes schémas d’authentification entre plusieurs services. Les équipes de sécurité interne doivent désormais intégrer un nouvel élément à leurs audits : la provenance et la légitimité des extensions IA installées par les développeurs, souvent sans validation centralisée préalable.

## Ce qu’une clé API compromise permet réellement de faire

Le préjudice d’une fuite de clé API dépend largement de ce à quoi cette clé donne accès. Dans le cas de la campagne JetBrains, la clé volée correspond à un compte de fournisseur IA facturé à l’usage : un tiers peut donc consommer des crédits payants au nom de la victime, sans qu’aucune alerte ne se déclenche avant la réception de la facture ou l’épuisement du quota.

Le risque ne s’arrête pas toujours là. Selon le fournisseur IA concerné, une clé compromise peut aussi exposer un historique de requêtes, donc potentiellement des fragments de code envoyés pour analyse ou complétion. C’est précisément cette incertitude, difficile à quantifier sans audit complet, qui pousse JetBrains à recommander une rotation systématique des clés plutôt qu’une évaluation au cas par cas pour les 70 000 installations concernées.

## Comment savoir si vous êtes concerné

JetBrains a publié la liste des 15 extensions retirées ainsi que l’adresse IP du serveur de collecte dans son bulletin de sécurité du 16 juin 2026. Un développeur ou une équipe IT peut vérifier son exposition de plusieurs façons : consulter l’historique des extensions installées dans chaque IDE JetBrains, rechercher spécifiquement les plugins portant les noms DeepSeek AI Assist, DeepSeek Junit Test et DeepSeek Git Commit, et examiner les journaux réseau sortants à la recherche de connexions vers l’adresse 39.107.60.51.

Même en l’absence de trace d’installation active, JetBrains rappelle que ses systèmes ont désactivé les plugins à distance dès le 16 juin 2026. Une extension qui aurait disparu de l’interface sans action de l’utilisateur constitue donc, en elle-même, un indice qu’elle faisait partie de la liste incriminée.

## Les mesures à prendre immédiatement

Au-delà de la vérification, JetBrains et StepSecurity recommandent une série d’actions concrètes pour toute personne ou organisation ayant pu être exposée aux extensions JetBrains malveillantes :

- Régénérer immédiatement toute clé API configurée dans une extension IA tierce installée avant le 16 juin 2026
- Bloquer l’adresse IP 39.107.60.51 au niveau du pare-feu ou du DNS d’entreprise
- Vérifier les journaux de facturation des fournisseurs IA concernés à la recherche d’une activité inhabituelle
- Auditer la liste complète des extensions installées sur les postes de développement, IDE par IDE
- Limiter, quand c’est possible, les clés API à des périmètres et quotas restreints plutôt qu’à des accès larges

Voici l’indicateur de compromission principal publié par les chercheurs, utile pour un blocage rapide côté pare-feu ou proxy :

```
# Indicateur de compromission confirmé - campagne JetBrains Marketplace (juin 2026)
IP du serveur de collecte : 39.107.60.51
Hébergeur : Alibaba Cloud (Pékin, Chine)
Protocole d'exfiltration observé : HTTP non chiffré
Extensions confirmées : DeepSeek AI Assist, DeepSeek Junit Test, DeepSeek Git Commit (+12 autres, retirées)
Action recommandée : bloquer l'IP en sortie, régénérer toute clé API exposée
```
## Ce que cette affaire révèle sur la sécurité des outils IA en 2026

L’incident JetBrains s’inscrit dans une tendance documentée depuis 2025 : les outils liés à l’intelligence artificielle sont devenus un vecteur d’attaque à part entière, au même titre que les pièces jointes piégées ou les faux sites de connexion l’ont longtemps été. La différence tient à la cible. Ce vecteur touche directement les professionnels techniques, développeurs, ingénieurs DevOps, équipes data, plutôt que le grand public, ce qui change la nature du préjudice potentiel et complique la détection par les outils de sécurité grand public.

Le fait que la campagne ait duré environ huit mois avant d’être détectée interroge aussi sur les délais de détection dans un écosystème où de nouvelles extensions apparaissent en continu. Ni JetBrains ni les chercheurs externes n’ont expliqué publiquement pourquoi la détection a pris autant de temps. Mais la mécanique observée, un vol de clé silencieux sans dysfonctionnement visible du plugin, explique en partie pourquoi ce type de campagne peut passer sous les radars aussi longtemps qu’une extension reste globalement fonctionnelle à l’usage.

## Cinq prédictions pour les prochains mois

1. **Un renforcement de la vérification en amont.** Après cet incident, JetBrains devrait durcir ses critères d’admission sur le Marketplace pour les extensions qui manipulent des clés API ou des identifiants, à l’image des ajustements déjà opérés par Microsoft après ses propres vagues de retraits en 2025.
2. **Une multiplication des campagnes similaires ailleurs.** Le résultat obtenu par cette attaque, 70 000 installations avant détection, est le type de résultat qui incite d’autres groupes à reproduire la méthode sur d’autres marketplaces d’extensions ou d’autres écosystèmes de plugins IA.
3. **Une pression accrue pour le chiffrement obligatoire des communications d’extensions.** Le fait que les clés aient transité en HTTP non chiffré est une faille évitable. Il est probable que les grandes plateformes imposent progressivement des vérifications techniques bloquant les appels réseau non chiffrés au moment de la publication.
4. **Une adoption plus large des gestionnaires de secrets côté entreprise.** Les équipes de sécurité vont probablement pousser pour que les clés API IA ne soient plus saisies directement dans les paramètres d’un plugin, mais gérées via des coffres-forts centralisés avec rotation automatique.
5. **Un débat réglementaire européen sur la responsabilité des marketplaces.** Avec la multiplication de ce type d’incidents touchant des professionnels européens, la question de la responsabilité des opérateurs de marketplaces d’extensions pourrait s’inviter dans les discussions autour de la mise en œuvre du Cyber Resilience Act, qui impose déjà des obligations de sécurité aux éditeurs de logiciels vendus dans l’Union européenne.

## Foire aux questions

### Quelles extensions JetBrains faut-il désinstaller immédiatement ?

JetBrains a confirmé le retrait de 15 extensions, dont DeepSeek AI Assist, DeepSeek Junit Test et DeepSeek Git Commit. Si l’une d’elles apparaît dans votre historique d’installation, considérez votre clé API comme compromise et régénérez-la sans attendre.

### Mon code source a-t-il été exposé ?

Selon JetBrains, l’incident a ciblé exclusivement les clés API saisies dans les paramètres des extensions malveillantes. L’entreprise affirme qu’aucun code source interne ni environnement de développement n’a été compromis dans ses propres systèmes, mais elle ne peut pas garantir ce qui a pu transiter via un assistant IA utilisant une clé volée.

### Comment vérifier si une extension a été désactivée automatiquement ?

JetBrains a activé un kill-switch à distance le 16 juin 2026. Si une extension liée à cette campagne a disparu de votre IDE sans action de votre part, c’est le signe qu’elle faisait partie des 15 plugins visés.

### Qui est derrière cette campagne ?

L’attribution formelle n’a pas été publiée. Les seuls éléments techniques disponibles pointent vers un serveur de collecte hébergé sur Alibaba Cloud à Pékin, sans confirmation officielle d’un groupe ou d’un État précis.

### Les extensions VS Code sont-elles plus sûres que celles de JetBrains ?

Les chiffres disponibles ne permettent pas de l’affirmer. Le Marketplace de VS Code a connu des incidents nettement plus massifs en volume d’installations, jusqu’à plusieurs centaines de millions cumulées selon certaines études, mais aucune des grandes plateformes ne documente publiquement un contrôle manuel systématique avant publication.

### Faut-il arrêter d’utiliser des extensions IA tierces ?

Non, mais la prudence s’impose. Mieux vaut privilégier les extensions publiées par des éditeurs identifiés et déjà établis, vérifier le nombre d’avis et la date de publication, et limiter les clés API à des quotas restreints plutôt qu’à des accès larges.

### JetBrains va-t-il indemniser les utilisateurs touchés ?

Aucune annonce en ce sens n’a été faite à ce jour. JetBrains recommande la régénération des clés API comme principale mesure corrective, sans mention d’indemnisation dans son bulletin du 16 juin 2026.

### Cet incident est-il lié à une faille dans un logiciel JetBrains ?

Non. Il ne s’agit pas d’une vulnérabilité technique dans les IDE JetBrains eux-mêmes, mais d’extensions tierces malveillantes publiées volontairement sur le Marketplace par des comptes d’éditeurs frauduleux, aujourd’hui bannis.

### Related Coverage

Pour suivre l’ensemble de notre couverture des failles et campagnes malveillantes touchant les entreprises européennes, retrouvez nos derniers articles dans la rubrique Cybersécurité de Tech Insider.
