Une faille NGINX vieille de dix-huit ans, logée dans le code source depuis 2008, a été activement exploitée quelques jours à peine après sa divulgation publique, le 13 mai 2026. Baptisée **NGINX Rift** et référencée **CVE-2026-42945**, elle affiche un score CVSS de 9,2 sur 10 en version 4.0. Un mois plus tard, le 18 juin 2026, l’éditeur F5 a dû publier un correctif d’urgence hors cycle pour colmater deux autres failles critiques, **CVE-2026-42530** et **CVE-2026-42055**, elles aussi notées 9,2. Puis, en juillet 2026, une quatrième vulnérabilité critique, **CVE-2026-42533** (CVSS 9,2 en version 4.0, 8,1 en version 3.1 selon Orca Security), a nécessité un nouveau correctif via les versions **NGINX 1.31.3** et **1.30.4**, comme le confirme la documentation officielle de nginx.org. En l’espace d’un peu plus de deux mois, le serveur web le plus utilisé au monde a donc cumulé quatre vulnérabilités critiques presque consécutives.

Le sujet dépasse largement le cercle des administrateurs systèmes. Selon les statistiques de W3Techs publiées en juillet 2026, NGINX équipe 31,8 % des sites dont le serveur web est identifiable, loin devant Apache (24,4 % en janvier 2026) et le serveur de Cloudflare (25,8 %). Une faille NGINX critique touche donc, potentiellement, plus d’un site sur trois dans le monde : infrastructures cloud, passerelles Kubernetes, reverse proxies d’entreprise et API gateways compris.

Ce dossier revient sur la chronologie de NGINX Rift, détaille les correctifs disponibles, et analyse pourquoi cette succession de failles critiques relance le débat sur la dette technique accumulée dans les briques logicielles les plus déployées de la planète.

## Qu’est-ce que NGINX Rift, la faille CVE-2026-42945 ?

Cette faille NGINX désigne un débordement de tampon en zone de tas (*heap-based buffer overflow*, référencé CWE-122) situé dans le module `ngx_http_rewrite_module`, la brique chargée de réécrire les URL à la volée. Le bug a été rendu public le 13 mai 2026 par l’équipe d’AlmaLinux, en parallèle de la fiche officielle publiée sur la base de données nationale des vulnérabilités américaine (NVD). Son score CVSS atteint 9,2 sur l’échelle version 4.0 (8,1 en version 3.1), ce qui la classe parmi les vulnérabilités jugées critiques.

Concrètement, un attaquant non authentifié peut déclencher la faille en envoyant une simple requête HTTP forgée vers un serveur mal configuré. Dans le pire des cas, cela peut mener à une exécution de code à distance. Dans le meilleur des cas pour la victime, cela provoque tout de même le plantage du processus de travail (*worker process*) de NGINX, donc une interruption de service. Le nom « Rift » fait référence à la fracture entre la taille de tampon calculée par le serveur et celle réellement écrite en mémoire.

## Chronologie : de la divulgation à l’exploitation active en quelques jours

La rapidité avec laquelle cette faille NGINX est passée du statut de découverte académique à celui de menace active illustre l’état actuel de la course entre attaquants et défenseurs.

- **13 mai 2026** : divulgation publique de CVE-2026-42945, publication simultanée des correctifs par NGINX/F5.
- **15 mai 2026** : premiers dépôts de preuve de concept (PoC) sur GitHub, dont une variante permettant l’exécution de code via une technique de « heap feng shui » inter-requêtes.
- **17 mai 2026** : la société de renseignement sur les vulnérabilités VulnCheck confirme, via des*honeypots* , une exploitation active en conditions réelles à peine quelques jours après la divulgation, comme le rapporte The Hacker News.
- **18 juin 2026** : F5 publie un correctif hors cycle pour deux nouvelles failles critiques distinctes, CVE-2026-42530 et CVE-2026-42055.
- **27 juin 2026** : le NVD met à jour la sévérité de CVE-2026-42945 vers le niveau critique dans le référentiel CVSS v4.0.

Cette chronologie serrée laisse peu de marge de manœuvre aux équipes qui gèrent des parcs de serveurs importants, d’autant que deux jeux de correctifs distincts sont sortis à moins de cinq semaines d’intervalle.

## Anatomie technique de la faille

### Le module de réécriture, cœur du problème

La vulnérabilité apparaît lorsqu’une directive `rewrite` est suivie d’une directive `rewrite`, `if` ou `set`, combinée à une capture PCRE non nommée (du type `$1` ou `$2`) dont la chaîne de remplacement contient un point d’interrogation. NGINX calcule la taille du tampon de destination avec une méthode d’échappement, puis écrit réellement les données avec une méthode différente. Des caractères comme `+`, `%` ou `&` s’étendent différemment selon la méthode utilisée, ce qui provoque un dépassement du tampon alloué. Les octets qui débordent proviennent directement de l’URI envoyée par l’attaquant, ce qui rend la corruption mémoire contrôlable, et non un simple plantage aléatoire.

### ASLR, la différence entre plantage et prise de contrôle

Sur un système où la randomisation de l’espace d’adressage (ASLR) est active et correctement configurée, l’exploitation se traduit « seulement » par un plantage du processus, donc un déni de service. Ce n’est que sur des hôtes où l’ASLR est désactivée, ou contournée par une chaîne d’exploitation plus élaborée, que l’exécution de code à distance devient réellement possible. C’est la variante avec exécution de code, publiée par le chercheur connu sous le pseudonyme jelasin, qui a le plus inquiété la communauté de la sécurité en mai 2026.

## Le 18 juin, rebelote : F5 corrige deux nouvelles failles critiques

Un mois après NGINX Rift, F5 a publié le 18 juin 2026 une mise à jour hors cycle corrigeant quatre vulnérabilités au total : deux critiques et deux de sévérité élevée, selon The Hacker News. Les deux failles critiques sont :

- **CVE-2026-42530** (CVSS v4.0 : 9,2) : une vulnérabilité de type*use-after-free* dans le module`ngx_http_v3_module` , qui gère le protocole HTTP/3 et QUIC.
- **CVE-2026-42055** (CVSS v4.0 : 9,2) : un débordement de tampon en zone de tas affectant les modules`ngx_http_proxy_v2_module` et`ngx_http_grpc_module` .

Contrairement à NGINX Rift, ces deux failles n’étaient, à la date de publication de cet article, pas répertoriées comme activement exploitées et ne figuraient pas dans le catalogue des vulnérabilités connues exploitées (KEV) de la CISA. F5 précise que les produits BIG-IP, BIG-IQ, F5 AI Gateway, F5 Distributed Cloud, F5OS, F5 Silverline, NGINX One Console et Traffix SDC ne sont pas concernés : seuls NGINX Open Source, NGINX Plus, NGINX Gateway Fabric, NGINX Instance Manager et NGINX Ingress Controller le sont. Le correctif publié en juillet 2026 pour CVE-2026-42533 a, lui, élargi le périmètre des composants touchés : selon Orca Security, étaient vulnérables NGINX Open Source de la version 0.9.6 à 1.31.2, NGINX Plus antérieur aux versions 37.0.3.1 et R36 P7, NGINX Ingress Controller (3.5.0–3.7.2, 4.0.0–4.0.1 et 5.0.0–5.4.2), NGINX App Protect WAF (4.9.0–4.16.0 et 5.1.0–5.8.0) ainsi que NGINX Instance Manager (2.16.0–2.22.0).

## Versions affectées et correctifs disponibles

Le tableau ci-dessous résume les trois failles critiques recensées sur l’écosystème NGINX entre mai et juin 2026, auxquelles est venue s’ajouter une quatrième, CVE-2026-42533, corrigée en juillet 2026 par les versions NGINX 1.31.3 et 1.30.4.

| CVE | Score CVSS (v4.0) | Composant touché | Date de divulgation | Statut d’exploitation | 
|---|---|---|---|---|
| CVE-2026-42945 (NGINX Rift) | 9,2 | ngx_http_rewrite_module | 13 mai 2026 | Activement exploitée | 
| CVE-2026-42530 | 9,2 | ngx_http_v3_module (HTTP/3) | 18 juin 2026 | Non exploitée à ce jour | 
| CVE-2026-42055 | 9,2 | ngx_http_proxy_v2_module / ngx_http_grpc_module | 18 juin 2026 | Non exploitée à ce jour | 

Pour identifier précisément si un parc est concerné, le second tableau détaille les versions vulnérables et les versions corrigées, produit par produit.

| Produit | Versions vulnérables | Version corrigée | 
|---|---|---|
| NGINX Open Source (Rift) | 0.6.27 à 1.30.0 | 1.30.1 ou 1.31.0+ | 
| NGINX Plus (Rift) | R32 à R36 | R32 P6, R35 P2 ou R36 P4 | 
| NGINX Open Source (42530/42055) | 1.31.0 à 1.31.1 | 1.31.2 | 
| NGINX Plus (42530/42055) | R32 à R36 et 37.0.0 à 37.0.1 | R32 P6 / R36 P4 ou 37.0.2.1 | 
| NGINX Gateway Fabric | 2.0.0 à 2.6.3 et 1.3.0 à 1.6.2 | 2.6.4 (ou version ultérieure) | 
| NGINX Ingress Controller | 3.5.0–3.7.2, 4.0.0–4.0.1, 5.0.0–5.5.0 | Versions ultérieures publiées par F5 | 

## NGINX, colonne vertébrale du web mondial : quel impact réel ?

NGINX n’est pas un simple serveur web parmi d’autres. Selon W3Techs, il occupait encore 33,3 % du marché en janvier 2026 avant de refluer légèrement à 31,8 % en juillet, un tassement à mettre en lien avec la progression du serveur proxy de Cloudflare (25,8 %). Il reste malgré tout la référence numéro un, devant Apache, tombé à 24,4 % en début d’année. Au-delà des sites web classiques, NGINX sert aussi de reverse proxy, de répartiteur de charge, de passerelle API et de contrôleur d’entrée (*ingress controller*) pour une écrasante majorité de clusters Kubernetes dans le monde.

C’est cette omniprésence qui complique la réponse à incident. De nombreuses équipes ne savent pas précisément où NGINX est déployé dans leur organisation : il se cache aussi bien dans des conteneurs applicatifs que dans des appliances réseau, des CDN internes ou des passerelles d’API tierces. Une faille NGINX critique dans son module de réécriture d’URL, fonctionnalité utilisée par une part significative des configurations de production, transforme donc un correctif technique isolé en chantier d’inventaire à l’échelle de toute l’infrastructure.

## Une dette technique vieille de 18 ans : comment est-ce possible ?

Le code fautif du module de réécriture existe depuis 2008, année où NGINX ne pesait encore qu’une fraction de son poids actuel sur le web mondial. Pendant dix-huit ans, cette logique de calcul de tampon a survécu à des centaines de versions, d’audits de sécurité et de revues de code, sans être détectée. Ce n’est pas un cas isolé dans l’histoire des logiciels d’infrastructure : les composants les plus anciens et les plus stables sont aussi, paradoxalement, ceux que l’on relit le moins souvent avec un œil neuf, précisément parce qu’ils sont considérés comme éprouvés.

NGINX Rift rappelle une leçon récurrente de la sécurité des infrastructures critiques : la maturité d’un logiciel ne garantit pas l’absence de failles, elle garantit seulement que les failles restantes sont plus difficiles à débusquer. Pour un projet aussi central que NGINX, chaque ligne de code écrite en 2008 reste, en 2026, une surface d’attaque potentielle tant qu’elle n’a pas été formellement revérifiée.

## NGINX Rift face aux autres failles critiques de 2026

2026 n’aura pas été une année calme pour les équipes de sécurité. Cette faille NGINX s’ajoute à une liste déjà longue de vulnérabilités critiques touchant des briques d’infrastructure massivement déployées, que Tech Insider a suivies au fil de l’eau.

| Faille | Produit | Donnée clé | 
|---|---|---|
| NGINX Rift (CVE-2026-42945) | NGINX (F5) | CVSS 9,2, faille vieille de 18 ans, exploitée en quelques jours | 
| Faille Chrome zero-day | Google Chrome | CVSS 8,8, 65,7 % des instances exposées | 
| SharePoint RCE | Microsoft SharePoint | CVSS 8,8, échéance de correction au 4 juillet | 
| Zero-day Adobe ColdFusion | Adobe ColdFusion | Exploitée en moins de 2 heures après divulgation | 
| Cisco SD-WAN | Cisco | 7 zero-days recensés, dont 2 notés CVSS 10 | 

Ce tableau illustre une tendance de fond : les délais entre divulgation et exploitation active se comptent désormais en heures ou en jours, rarement en semaines. Sur ce point précis, cette faille NGINX se situe dans la moyenne haute de 2026, entre les quelques heures qui ont suffi pour la faille Adobe ColdFusion et les fenêtres plus longues observées sur d’autres produits.

## La réponse encore timide des autorités françaises et européennes

Le CERT-FR a toutefois publié, le 25 mai 2026, un avis consacré à NGINX mais portant sur une référence distincte, **CVE-2026-9256**, qui recensait comme vulnérables les versions 0.1.17 à 1.31.0 de NGINX Open Source et la version 37.0.0 de NGINX Plus. À la date de publication de cet article, l’organisme n’a en revanche pas émis d’avis dédié et numéroté spécifiquement à NGINX Rift ni aux deux CVE corrigées le 18 juin, même s’il continue de surveiller en continu les vulnérabilités affectant les briques web les plus déployées sur le territoire français. Au Canada, le Centre canadien pour la cybersécurité a, de son côté, publié en juillet 2026 l’avis AV26-704, qui recense NGINX Agent dans ses versions 2.37.0 à 2.46.5 parmi les composants concernés. Cette communication en ordre dispersé, faille par faille et pays par pays, illustre une difficulté structurelle : les autorités nationales ne peuvent matériellement pas publier un avis dédié pour chacune des vulnérabilités critiques touchant les logiciels open source les plus répandus.

Le dossier NGINX Rift s’inscrit dans un contexte réglementaire européen déjà mouvant. Tech Insider a récemment détaillé le renvoi de la France devant la Cour de justice de l’Union européenne pour retard de transposition de la directive NIS2, ainsi que la montée en puissance annoncée du Cybersecurity Act 2, qui doit renforcer les moyens de l’ENISA. Les deux textes visent justement à réduire les délais de correction des vulnérabilités critiques dans les infrastructures numériques essentielles, un objectif que la succession de failles NGINX rend d’autant plus concret.

## Quelles conséquences pour les entreprises et les fournisseurs cloud ?

Pour les hébergeurs, les fournisseurs de CDN et les éditeurs SaaS qui embarquent NGINX ou des dérivés comme OpenResty, la succession des trois CVE de 2026 se traduit par un cycle de correctifs en cascade : chaque nouvelle version corrigée doit être re-testée, re-packagée puis redéployée sur des flottes parfois composées de dizaines de milliers d’instances. Le rapport annuel sur la réponse à incident publié par l’unité de recherche Unit 42 de Palo Alto Networks souligne, dans son édition 2026, la place croissante que prennent les vulnérabilités dans les composants d’infrastructure partagée parmi les vecteurs d’intrusion initiaux traités par les équipes de réponse à incident.

Pour une entreprise française de taille intermédiaire qui héberge son propre reverse proxy NGINX, le coût n’est pas seulement technique. Il faut mobiliser des équipes DevOps ou SRE en urgence, parfois interrompre des déploiements planifiés, et documenter la remédiation pour répondre aux obligations de traçabilité qui accompagnent désormais la directive NIS2. Multiplié par trois correctifs critiques en cinq semaines, l’effort cumulé pèse sur des équipes techniques déjà sollicitées par ailleurs.

## Ce que les équipes techniques doivent faire dès maintenant

La priorité immédiate consiste à identifier précisément les versions de NGINX déployées, puis à planifier la montée de version en fonction du niveau d’exposition de chaque instance (accessible depuis Internet ou non). Les commandes suivantes permettent de vérifier rapidement la version installée sur un serveur Linux :

```
# Vérifier la version de NGINX installée
nginx -v
# Sur une distribution basée sur Debian/Ubuntu
apt list --installed | grep nginx
# Sur une distribution basée sur RHEL/AlmaLinux
rpm -qa | grep nginx
# Sur un cluster Kubernetes, vérifier l'image de l'Ingress Controller
kubectl get pods -A -o jsonpath="{.items[*].spec.containers[*].image}" | tr ' ' '\n' | grep -i nginx
```
En complément de la mise à jour, plusieurs mesures d’atténuation permettent de réduire le risque en attendant une fenêtre de maintenance :

- Auditer les fichiers de configuration à la recherche de directives `rewrite` suivies d’une directive`rewrite` ,`if` ou`set` utilisant des captures PCRE non nommées.
- Remplacer les captures non nommées (`$1` ,`$2` ) par des captures nommées lorsque c’est possible, en attendant la mise à jour.
- Vérifier que l’ASLR est bien activée sur l’ensemble des hôtes exposés, en particulier les conteneurs et machines virtuelles récemment provisionnés.
- Mettre en place une règle de pare-feu applicatif (WAF) en « virtual patching » le temps de qualifier la montée de version en environnement de test.
- Surveiller les journaux pour détecter des plantages anormaux et répétés des processus *worker* de NGINX, signe potentiel de tentatives d’exploitation.

## 5 prédictions pour la suite

Au vu de la chronologie observée depuis mai 2026, plusieurs évolutions semblent probables dans les prochains mois :

1. **D’autres CVE viseront des modules NGINX anciens.** L’attention portée à NGINX Rift va probablement pousser des chercheurs indépendants à auditer d’autres modules hérités des débuts du projet, avec de nouvelles découvertes possibles d’ici la fin 2026.
2. **L’analyse automatisée de versions va s’accélérer.** Les entreprises qui ont découvert leur exposition tardivement vont investir davantage dans des scanners de vulnérabilités intégrés au pipeline de déploiement, à l’image d’outils comme Trivy.
3. **La pression réglementaire européenne va s’intensifier.** Entre NIS2 et le Cybersecurity Act 2, les obligations de délai de correction pour les infrastructures numériques essentielles devraient se préciser, avec cette faille NGINX comme exemple récurrent dans les débats.
4. **Le recours au « virtual patching » par WAF va progresser.** Face à des cycles de correctifs rapprochés, de plus en plus d’organisations vont s’appuyer sur des règles de pare-feu applicatif comme filet de sécurité temporaire.
5. **De nouvelles vagues d’exploitation opportunistes sont à prévoir.** Comme pour la plupart des failles critiques largement médiatisées en 2026, les serveurs qui n’auront pas appliqué les correctifs d’ici la fin de l’été risquent de devenir des cibles privilégiées pour des campagnes automatisées.

## Foire aux questions

### Qu’est-ce que CVE-2026-42945, alias « NGINX Rift » ?

Il s’agit d’un débordement de tampon en zone de tas dans le module de réécriture d’URL de NGINX (`ngx_http_rewrite_module`), noté CVSS 9,2 sur l’échelle version 4.0, divulgué le 13 mai 2026 et activement exploité depuis.

### Mon serveur NGINX est-il vulnérable ?

Si votre version de NGINX Open Source se situe entre 0.6.27 et 1.30.0, ou votre NGINX Plus entre R32 et R36, et que votre configuration utilise des directives `rewrite` avec captures non nommées, vous êtes potentiellement exposé à cette faille NGINX. La commande `nginx -v` permet de vérifier rapidement la version installée.

### Comment savoir si mon serveur a déjà été compromis ?

Recherchez dans les journaux des redémarrages ou plantages anormaux et répétés des processus *worker* de NGINX, ainsi que des requêtes contenant des motifs de réécriture inhabituels ou des chaînes anormalement longues dans l’URI.

### Les failles du 18 juin sont-elles liées à NGINX Rift ?

Non, CVE-2026-42530 et CVE-2026-42055 touchent des modules différents (HTTP/3 et proxy/gRPC) de celui concerné par NGINX Rift. Elles ont cependant été révélées seulement cinq semaines plus tard, ce qui a renforcé la pression sur les équipes de correction.

### NGINX Ingress Controller sur Kubernetes est-il concerné ?

Oui, plusieurs versions de NGINX Ingress Controller (3.5.0 à 3.7.2, 4.0.0 à 4.0.1 et 5.0.0 à 5.5.0) sont concernées par les failles corrigées le 18 juin 2026. Une mise à jour de l’image du contrôleur est nécessaire.

### Existe-t-il un correctif temporaire si je ne peux pas mettre à jour immédiatement ?

Remplacer les captures PCRE non nommées par des captures nommées dans les directives de réécriture réduit le risque lié à NGINX Rift. Une règle de pare-feu applicatif en mode « virtual patching » peut aussi limiter l’exposition en attendant la montée de version définitive.

### Quelle est la différence entre NGINX Open Source et NGINX Plus face à ces failles ?

Les deux éditions sont concernées, mais avec des versions et des numéros de correctifs différents : NGINX Plus reçoit des correctifs dédiés identifiés par des lettres « P » (par exemple R36 P4), distribués séparément du cycle de NGINX Open Source.

### NGINX reste-t-il le serveur web le plus utilisé au monde malgré cette faille ?

Oui. Malgré un léger recul de 33,3 % à 31,8 % de parts de marché entre janvier et juillet 2026 selon W3Techs, NGINX conserve la première place devant Apache et le serveur de Cloudflare.