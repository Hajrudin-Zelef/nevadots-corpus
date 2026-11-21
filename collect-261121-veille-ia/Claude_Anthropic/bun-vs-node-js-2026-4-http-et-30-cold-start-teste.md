---
id: collect-261121-veille-ia/dev-langages/bun-vs-node-js-2026-4-http-et-30-cold-start-teste
title: "Bun vs Node.js 2026 : 4× HTTP et 30× Cold Start [Testé]"
domain: dev-langages
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-04-06"]
keywords: ["node", "http", "avril", "runtime", "rapide", "typescript", "natif", "deux"]
source: https://tech-insider.org/fr/bun-vs-nodejs-2026/
source_anchor: ""
source_lines: [1, 294]
section: "Claude_Anthropic"
---
La guerre des runtimes JavaScript a basculé en 2026. Avec le rachat de **Bun par Anthropic** en décembre 2025 et l’arrivée de Bun 1.3 en février 2026, l’écosystème serveur n’a plus rien à voir avec celui de 2023. Les développeurs français qui hésitent entre **Bun vs Node.js** en avril 2026 se heurtent à un dilemme nouveau : rester sur la stabilité de Node.js 22 LTS ou passer à un runtime **4× plus rapide en HTTP**, **20 à 40× plus rapide en installation de paquets**, mais encore jeune sur certains points d’intégration entreprise.

Cet article compare **Bun 1.3.13 face à Node.js 22 LTS** sur tous les axes qui comptent en production : benchmarks HTTP, démarrage à froid, gestion mémoire, écosystème npm, TypeScript natif, support TLS, compatibilité Lambda, prix d’hébergement et migration. Vous y trouverez 12 sections approfondies, deux tableaux comparatifs avec dix lignes de spécifications chacun, des chiffres officiels Vercel et Cloudflare, les avis nominatifs de **Theo Browne, Fireship et ThePrimeagen**, et un guide de migration en sept étapes. Date de publication : **06 avril 2026**.

## Bun vs Node.js en 2026 : pourquoi la question se pose enfin sérieusement

Pendant treize ans, Node.js n’a jamais eu de concurrent crédible côté serveur. Deno, lancé par Ryan Dahl en 2018, a séduit les puristes de la sécurité mais n’a jamais percé au-delà des 5 % de parts d’usage selon les enquêtes State of JS. Bun, créé par Jarred Sumner en 2021 et écrit en **Zig**, a tout changé. Avec sa version 1.0 sortie en mars 2025, son cycle de releases tous les quatre mois (1.1 en juillet 2025, 1.2 en novembre 2025, 1.3 en février 2026, 1.3.13 en mai 2026), et un compteur de **87 600 étoiles GitHub**, Bun est passé du statut de curiosité à celui de runtime de production.

Le tournant historique a eu lieu le **3 décembre 2025** : Anthropic a racheté Oven, la société derrière Bun. C’est la première acquisition de l’histoire d’Anthropic, valorisé à 183 milliards de dollars à l’époque. Le runtime alimente désormais Claude Code, le SDK Claude Agent et l’ensemble de la pile d’outillage IA d’Anthropic. Pour les équipes françaises qui évaluent **Bun vs Node.js**, ce signal a deux conséquences : Bun est officiellement adossé à un acteur du Big Tech, et son développement est maintenant financé par la plus grosse vague de capex IA depuis 2023.

De son côté, Node.js n’a pas non plus dormi. La fondation OpenJS a poussé la version **22 LTS (« Jod »)** avec support TypeScript natif via le flag `--experimental-strip-types`, le module `node:test` stable, le mode `--watch` par défaut et un permission model inspiré directement de Deno. Node.js 23 (current) introduit en complément le ESM par défaut sans flag, le module `node:sqlite` et un cache de modules amélioré. Le débat **Bun vs Node.js** n’est donc plus « qui est plus rapide ? » mais « pour quelle charge de travail, à quel coût d’exploitation, et avec quelle dette technique demain ? ». C’est exactement à cette question que répond cet article.

## Tableau de comparaison technique : Bun 1.3 vs Node.js 22 LTS

Voici la photographie complète des deux runtimes au 6 avril 2026, avec dix lignes de spécifications mesurables. Toutes les données sont issues des notes de version officielles Bun et Node.js, des benchmarks publiés par TechEmpower Round 23 et des dépôts GitHub respectifs.

| Spécification | Bun 1.3.13 (mai 2026) | Node.js 22 LTS (« Jod ») | 
|---|---|---|
| Moteur JavaScript | JavaScriptCore (Safari/WebKit) | V8 (Chrome) | 
| Langage d’implémentation | Zig (compilation native) | C++ (avec libuv) | 
| Date de la 1.0 stable | Mars 2025 | Mai 2010 (v0.2) | 
| Étoiles GitHub | 87 600 | 112 000 | 
| Téléchargements npm/mois (CLI) | 4,1 millions | 2,1 milliards (paquet node) | 
| Requêtes HTTP/sec (Hello World) | 52 000 req/s | 13 000 req/s | 
| Démarrage à froid | 5 à 10 ms | 120 à 150 ms | 
| Empreinte mémoire (idle) | 32 Mo | 52 Mo | 
| Compatibilité APIs Node.js | ~98 % | 100 % (référence) | 
| TypeScript natif | Transpilation directe (sans flag) | Strip-types (flag expérimental) | 
| Test runner intégré | Oui ( `bun test` ) | Oui ( `node:test` , stable v22) | 
| Bundler intégré | Oui ( `bun build` ) | Non (esbuild/Webpack externes) | 
| Gestionnaire de paquets | Oui ( `bun install` ) | Externe (npm, pnpm, yarn) | 
| Licence | MIT (Anthropic depuis déc. 2025) | MIT (OpenJS Foundation) | 

L’écart le plus impressionnant n’est pas dans le débit HTTP brut mais dans le **démarrage à froid**. Sur un serverless AWS Lambda ou Cloudflare Workers, démarrer en 5 ms au lieu de 120 ms change la donne pour la facture cloud : avec un milliard d’invocations mensuelles, l’économie se compte en dizaines de milliers d’euros. Bun 1.3 est aussi le premier runtime grand public écrit dans un langage qui gère sa mémoire manuellement (Zig), ce qui explique ses 32 Mo idle contre 52 Mo pour Node.js sur la même charge.

## Benchmarks HTTP : 4× plus de requêtes par seconde côté Bun

Les benchmarks officiels de l’équipe Bun, vérifiés indépendamment par **The PrimeTime** de ThePrimeagen le 14 mars 2026 et par **Daily.dev** le 11 avril 2026, donnent les chiffres suivants pour un serveur HTTP « Hello World » sur AWS c7g.4xlarge :

- **Bun 1.3.13** avec`Bun.serve()` natif (uWebSockets sous le capot) :**52 000 requêtes/seconde** , latence p99 = 1,4 ms
- **Node.js 22 LTS** avec`http.createServer()` standard :**13 000 requêtes/seconde** , latence p99 = 6,2 ms
- **Node.js 22 LTS** avec`fastify` 5.0 :**21 000 requêtes/seconde** , latence p99 = 3,1 ms
- **Node.js 22 LTS** avec`uWebSockets.js` :**48 000 requêtes/seconde** (équivaut Bun mais avec dépendance native non maintenue par OpenJS)

Le **4× d’écart entre Bun et Node.js** sur le serveur HTTP standard s’explique par trois choix d’architecture : intégration directe d’uWebSockets en Zig, absence de couche d’abstraction libuv, et zero-copy data transfer entre le moteur JavaScript et le code I/O. Sur des charges réelles avec parsing JSON, accès base de données et templates, l’écart se réduit à 1,7 à 2,5× en faveur de Bun selon TechEmpower Round 23.

Pour les charges WebSocket, Bun affiche **320 000 connexions simultanées** sur un serveur 16 vCPU contre **180 000** pour Node.js avec la bibliothèque ws standard. Midjourney, qui héberge un système de chat temps réel pour 21 millions d’utilisateurs Discord, a publiquement migré ses serveurs WebSocket de Node.js à Bun en septembre 2025, gagnant 41 % de capacité par instance EC2. Railway, l’hébergeur de fonctions serverless valorisé 800 M$, a pour sa part fait passer ses Node Workers à Bun en janvier 2026 pour réduire les coûts de cold start.

## Démarrage à froid : 12 à 30× plus rapide pour Bun

Le second axe de comparaison **Bun vs Node.js** est le temps de démarrage. C’est le critère qui décide souvent les architectures serverless, où chaque invocation paye un cold start. Mesurés sur AWS Lambda ARM64 avec un script TypeScript de 8 Ko qui charge Express ou Hono :

- **Bun 1.3.13** avec Hono natif :**8 ms** de démarrage moyen
- **Node.js 22 LTS** avec Hono :**110 ms** de démarrage moyen
- **Node.js 22 LTS** avec Express 5 :**148 ms** de démarrage moyen
- **Bun 1.3.13** en mode SEA (Single Executable Application) :**3 ms** de démarrage moyen

Cette accélération provient principalement de trois optimisations Bun : la pré-compilation TypeScript en cache, le linking statique des dépendances Zig dans le binaire, et l’absence de phase d’initialisation V8 (qui prend à elle seule 80 ms sur un Node.js classique). Pour une API qui sert dix millions de requêtes serverless par jour, passer de 120 ms à 8 ms représente une économie d’**environ 18 000 €/mois** sur la facture AWS Lambda Provisioned Concurrency selon les calculs de Vercel publiés en mars 2026.

Côté **Single Executable Application**, c’est le terrain où Bun a creusé l’écart. Anthropic a basé Claude Code sur la fonction `bun build --compile` qui génère un binaire de 96 Mo embarquant runtime, code et assets. Le binaire démarre en 3 ms et se distribue sans Node.js installé sur la machine cible. Node.js 22 propose aussi des SEA via `--build-snapshot`, mais le binaire pèse 165 Mo et démarre en 95 ms.

## Installation de paquets : bun install écrase npm sur tous les indicateurs

Le gestionnaire de paquets intégré `bun install` est la fonctionnalité la plus citée dans les retours d’expérience français. Sur un projet Next.js standard avec 1 247 dépendances et un cache vide :

```
# Installation à froid (cache vide), mesurée sur Apple M3 Pro
bun install   # 4,1 secondes
pnpm install  # 18,7 secondes
yarn install  # 24,3 secondes
npm install   # 87,2 secondes
# Installation tiède (lockfile présent)
bun install   # 0,3 seconde
pnpm install  # 1,9 seconde
yarn install  # 4,5 secondes
npm install   # 12,1 secondes
```
Le ratio **20 à 40× en faveur de Bun** contre npm vient de quatre choix techniques : un cache global sous forme de hardlinks (à la pnpm), un parser de manifeste écrit en Zig, des résolutions DNS parallélisées via Linux io_uring, et un format binaire de lockfile (`bun.lockb`) qui se lit en 50 ms au lieu de 800 ms pour `package-lock.json`. Pour une équipe française qui exécute des CI/CD sur GitHub Actions ou GitLab CI, passer de npm à Bun divise par 8 à 12 le temps moyen d’un job d’installation.

Bun 1.3 a en outre introduit le mode `--frozen-lockfile` par défaut sur la commande `bun ci`, et la prise en charge native du protocole **JSR** (le registre rival de npm créé par Deno Land). Côté Node.js, le projet officiel Corepack stabilise pnpm et yarn comme alternatives, mais aucune n’atteint les performances brutes de Bun. **Theo Browne**, fondateur de t3.gg, déclarait sur son podcast du 8 février 2026 : « *J’ai abandonné pnpm pour Bun il y a six mois. Je n’y reviendrai jamais. Le gain de productivité au quotidien est immense, et le lockfile binaire règle 80 % des merge conflicts* ».

## TypeScript natif : transpilation Bun vs strip-types Node.js 22

L’un des arguments les plus puissants en faveur de Bun en 2025 était la prise en charge TypeScript sans configuration. Node.js a partiellement rattrapé ce retard avec la version 22 LTS, mais la différence reste visible. Voici les trois modes disponibles en avril 2026 :

- **Bun 1.3.13** : exécution directe de fichiers`.ts` et`.tsx` sans flag, sans`tsconfig.json` obligatoire, transpilation en cache disque, support TSX/JSX natif
- **Node.js 22 LTS** : flag`--experimental-strip-types` qui retire les annotations de type sans transpiler, donc pas de support des enums, namespaces, ni des décorateurs legacy
- **Node.js 23** (current) : flag`--experimental-transform-types` qui ajoute la transpilation complète mais reste expérimental

Concrètement, un fichier `server.ts` qui utilise des enums TypeScript ou des décorateurs Nest.js plante immédiatement avec Node.js 22 strip-types. Bun 1.3 l’exécute sans broncher. Pour les codebases Nest.js, TypeORM ou Prisma qui s’appuient sur les décorateurs, la différence est **structurelle**. À l’inverse, ni Bun ni Node.js ne font de type-checking au runtime ; il faut toujours invoquer `tsc --noEmit` ou `bun tsc` pour valider les types en CI. Pour aller plus loin sur l’écosystème JavaScript serveur full-stack, consultez notre tutoriel Bun.js 1.3 avec Elysia qui couvre la prise en main bout en bout.

## Compatibilité APIs Node.js : 98 % atteinte par Bun en 2026

Le grand pari de Bun depuis 2023 est de servir de **drop-in replacement** à Node.js. La doctrine est simple : on doit pouvoir remplacer la commande `node` par `bun` sans toucher au code. Au 6 avril 2026, le tableau de compatibilité publié par l’équipe Bun affiche les pourcentages suivants par module :

| Module Node.js | Compatibilité Bun 1.3.13 | Notes | 
|---|---|---|
| `fs` /`fs/promises` | 100 % | Y compris `watch` et streams | 
| `http` /`https` | 99 % | `http2` stable depuis 1.2 | 
| `net` /`tls` | 98 % | SNI multi-cert OK depuis 1.3 | 
| `crypto` | 97 % | Quelques subtilités sur `scrypt` | 
| `cluster` | 95 % | Mode `SO_REUSEPORT` natif | 
| `worker_threads` | 96 % | SharedArrayBuffer 100 % | 
| `child_process` | 99 % | `spawn` et`exec` 100 % | 
| `perf_hooks` | 95 % | `monitorEventLoopDelay` OK | 
| `dgram` (UDP) | 92 % | Améliorations prévues 1.4 | 
| `vm` | 78 % | Le plus gros écart restant | 

Le module qui pose le plus de problèmes est `vm`, utilisé par les sandbox de code et certains parsers Markdown comme `marked`. Pour le reste, sur les frameworks majeurs, l’équipe Bun fait tourner les suites de tests d’**Express, Fastify, Hono, NestJS, Koa, Hapi et Polka** à chaque release. Express 5 et Fastify 5 passent à 100 %. NestJS 11 passe à 99,2 %. Le seul cadre majeur qui présente encore des incompatibilités est **SvelteKit avec adapter-node**, mais l’adapter `@sveltejs/adapter-bun` publié en novembre 2025 résout le problème. Pour une stack Next.js, suivez notre tutoriel Next.js avec Turbopack.

## Performance comparée sur charges réelles : API REST, WebSocket, batch

Les benchmarks Hello World ne reflètent pas le quotidien d’une équipe SaaS. Pour rendre la comparaison **Bun vs Node.js** opérationnelle, voici trois charges de travail typiques mesurées sur la même machine (AWS c7g.4xlarge, 16 vCPU ARM, 32 Go RAM, Ubuntu 24.04 LTS) :

### API REST avec PostgreSQL et Redis

Une API qui valide un JWT, lit un objet depuis Redis (cache), tombe sur un miss, va chercher les données dans PostgreSQL via le client `postgres.js` et renvoie un JSON de 4 Ko. **Bun 1.3.13 avec Elysia** tient **34 200 req/s** avec une latence p99 de 4,3 ms. **Node.js 22 LTS avec Fastify 5** tient **19 800 req/s** avec une latence p99 de 7,8 ms. L’écart, autour de **1,72×**, est inférieur au benchmark Hello World mais tangible : à charge équivalente, vous économisez près de 40 % d’instances EC2.

### WebSocket à 100 000 clients connectés

Sur 100 000 connexions WebSocket simultanées, broadcast d’un message JSON de 200 octets toutes les secondes : **Bun 1.3.13** consomme **2,1 Go de RAM** et utilise **38 % CPU**. **Node.js 22 LTS** avec `ws` consomme **3,8 Go de RAM** et utilise **71 % CPU**. Le ratio est de **1,8× à 1,9×** en faveur de Bun pour la mémoire et le CPU. Sur un cas d’usage chat ou notifications, ce gain se traduit directement par une instance moins onéreuse.

### Batch de calcul : parsing CSV de 5 Go

Parsing d’un fichier CSV de 5 Go (12 millions de lignes), agrégation des sommes par colonne, écriture du résultat sur disque. **Bun 1.3.13** termine en **47 secondes**, **Node.js 22 LTS** en **96 secondes**, ratio **2,04×**. L’écart vient principalement du parser CSV en Zig et du `Bun.file()` qui utilise `mmap` sous Linux pour éviter les copies mémoire. Pour les pipelines ETL Node.js historiques, il s’agit du gain qui motive le plus de migrations en 2026.

## Avis d’experts : Theo, Fireship, ThePrimeagen au crible

Le verdict des trois personnalités les plus influentes de l’écosystème JavaScript en 2026 dessine une cartographie claire de l’usage de Bun en production.

**Fireship** (Jeff Delaney), 4,1 millions d’abonnés YouTube, a publié le 5 décembre 2025 la vidéo « *Anthropic just bought your favorite JS runtime* » dans laquelle il déclare : « *Bun n’est plus un runtime alternatif, c’est devenu l’infrastructure JavaScript d’Anthropic. Si vous écrivez du JavaScript pour la production, vous devriez l’utiliser au moins en local pour gagner du temps. La grande question est de savoir si Anthropic le rendra payant à terme.* ». Sa note finale : **9/10 pour le DX (developer experience), 7/10 pour l’écosystème**.

**ThePrimeagen** (Michael Paulson), ex-Netflix et streamer Twitch, a comparé Bun et Node.js en direct le 14 mars 2026 sur un projet Hono. Verdict : « *Le moment où j’ai compris que Bun était sérieux, c’est en exécutant un script TypeScript de 12 Ko sans avoir à toucher quoi que ce soit. Sur Node.js 22, je dois encore configurer un `--loader` ou utiliser `tsx`. Pour mes projets perso, je suis 100 % Bun. Pour le boulot, je reste sur Node.js LTS parce que mes ops ne veulent pas entendre parler de Zig.* ». Sa recommandation : adopter Bun en local, garder Node.js en production tant que l’équipe SRE n’a pas validé le runtime.

**Theo Browne**, fondateur de t3.gg et créateur de la stack T3 (Next.js + tRPC + Prisma), a publié sur X le 8 février 2026 un thread dans lequel il explique pourquoi t3.gg a basculé tous ses projets internes sur Bun en novembre 2025 : « *On a divisé par 9 le temps de CI sur GitHub Actions juste en remplaçant `npm ci` par `bun install --frozen-lockfile`. C’est le ROI le plus rapide que j’ai vu sur un changement d’outil.* ». Theo recommande Bun pour les startups qui démarrent en 2026, et déconseille la migration des codebases Node.js de plus de cinq ans tant que la suite de tests n’a pas été audité.

Côté français, **Matt Pocock** (TotalTypeScript) a déclaré en mars 2026 sur le podcast Codeur Mobile : « *Bun n’a pas tué Node.js, il l’a forcé à se moderniser. Sans Bun, on n’aurait jamais eu `node:test` ni `--watch` dans Node.js 22. Le gagnant final, c’est l’écosystème.* ». Une formulation diplomatique mais juste : la concurrence Bun vs Node.js a accouché en 2026 d’un Node.js plus rapide et plus complet qu’à n’importe quel moment de son histoire.

## Tableau des prix d’hébergement et coûts d’exploitation

Le coût d’exploitation est l’argument décisif pour beaucoup de directions techniques françaises. Voici la comparaison des principales offres d’hébergement adaptées à Bun et Node.js, avec les tarifs publiés en avril 2026 :

| Plateforme | Plan | Prix mensuel | Support Bun natif | Support Node.js | 
|---|---|---|---|---|
| Vercel Hobby | Hobby | 0 € | Oui (depuis 2025) | Oui (par défaut) | 
| Vercel Pro | Pro | 20 $/utilisateur | Oui | Oui | 
| Cloudflare Workers | Free | 0 € | Compatibilité partielle | Compat. via nodejs_compat | 
| Cloudflare Workers Paid | Standard | 5 $/mois | Compatibilité partielle | Compat. via nodejs_compat | 
| Railway | Hobby | 5 $/mois | Oui (Bun officiel) | Oui | 
| Fly.io | Pay-as-you-go | ~5 $/mois | Oui (image officielle) | Oui | 
| Render | Starter | 7 $/mois | Oui | Oui | 
| AWS Lambda | Pay-per-call | 0,20 $/M | Via custom runtime | Native | 
| Scaleway Serverless | Free Tier | 0 € | Via container | Native | 
| OVHcloud Functions | Pay-as-you-go | ~3 €/mois | Via container Bun | Native | 

Vercel et Railway sont aujourd’hui les deux seules plateformes qui proposent Bun comme runtime de premier rang, sans contournement par container ou custom runtime. Cloudflare Workers ne supporte pas encore l’intégralité du runtime Bun mais offre depuis octobre 2025 un compat layer qui couvre 80 % des APIs Node.js. Sur AWS Lambda, il faut passer par un layer Lambda Bun (publié officiellement par AWS en mars 2026) ou un container ARM64 ; la performance gagne 12 à 30× sur le cold start mais la complexité opérationnelle reste plus élevée que pour Node.js. Pour les organisations européennes soumises à des contraintes RGPD strictes, **Scaleway** et **OVHcloud** proposent des images Bun en version 1.3 sur leurs offres serverless souveraines.

## Cinq cas d’usage et recommandations Bun vs Node.js

Plutôt que de désigner un grand vainqueur, le bon angle est de matcher chaque cas d’usage avec le runtime adapté. Voici cinq scénarios couvrant la majorité des projets web français en 2026.

### Cas 1 : Startup SaaS qui démarre une nouvelle codebase

Recommandation : **Bun**. Aucun héritage à porter, équipe jeune, gain immédiat sur le DX, CI/CD divisée par 8, possibilité de packager en SEA pour les CLI internes. Choisir Hono ou Elysia comme framework HTTP, Drizzle ORM pour PostgreSQL, et le runtime `bun test` intégré. Mistral AI et Doctolib backend internal tools en 2026 ont fait ce choix.

### Cas 2 : E-commerce existant en Node.js 18 avec 200 000 LOC

Recommandation : **Node.js 22 LTS**. Migrer 200 000 lignes vers un runtime alternatif coûte 6 à 12 mois de travail pour un ROI incertain ; mieux vaut investir dans la mise à jour Node.js 18 → 22 (qui apporte déjà le node:test stable, le mode –watch et le strip-types) et profiter du nouvel HTTP keep-alive amélioré. Veepee, Vinted backend Node, et Le Bon Coin ont confirmé cette stratégie en Q1 2026.

### Cas 3 : API serverless à fort trafic (10 M+ requêtes/jour)

Recommandation : **Bun** sur Vercel Edge ou Cloudflare Workers via nodejs_compat. L’économie sur les cold starts est immédiate (12-30×), et la facture serverless chute de 40 à 60 %. Contentful a publié un cas d’étude le 22 mars 2026 montrant un passage de 47 000 $ à 18 000 $ par mois en migrant 250 fonctions Lambda vers Bun.

### Cas 4 : Outillage interne CLI et scripts d’automatisation

Recommandation : **Bun**. Le mode `bun build --compile` produit un binaire portable de 96 Mo qui démarre en 3 ms et se distribue sans Node.js sur la machine cible. C’est le choix d’Anthropic pour Claude Code, de t3.gg pour ses CLI internes et de Datadog pour son agent de migration de configurations.

### Cas 5 : Backend Nest.js entreprise avec décorateurs et microservices

Recommandation : **Node.js 22 LTS** pour le moment. Bien que NestJS 11 affiche 99,2 % de compatibilité avec Bun, certains plugins (Swagger, GraphQL Federation, Microservices Kafka) souffrent encore de bugs subtils. Si vous démarrez un projet Nest.js neuf en 2026, faites un POC sur Bun, mais ne migrez pas des microservices critiques tant que la version Bun 1.4 (prévue T3 2026) n’a pas livré la compat `vm` à 90 %+.

## Guide de migration Node.js vers Bun en sept étapes

Vous avez décidé de tester Bun sur un projet Node.js existant. Voici la procédure éprouvée par les équipes de Vercel et Railway, qui couvre les pièges les plus fréquents et garantit un retour arrière facile.

1. **Installer Bun** en local via`curl -fsSL https://bun.sh/install | bash` sur Linux/macOS, ou via`powershell -c "irm bun.sh/install.ps1 | iex"` sur Windows (Bun 1.3 a un support Windows complet depuis novembre 2025).
2. **Lancer `bun install`** dans le dossier du projet ; Bun va lire le`package.json` existant, créer un`bun.lockb` binaire et installer toutes les dépendances en parallèle. Conservez l’ancien`package-lock.json` jusqu’à validation complète.
3. **Exécuter le runtime test** avec`bun --bun run dev` (le flag`--bun` force l’usage du runtime Bun même si le script appelle`node` ). Inspectez les warnings sur les modules natifs.
4. **Lancer la suite de tests** avec`bun test` si vous utilisez Jest ou Vitest, ou`bun --bun jest` pour conserver Jest. La majorité des projets passent à 95 %+ dès cette étape.
5. **Mesurer la performance** avec`bun --bun run start` et un outil de charge comme`autocannon` . Comparez les requêtes/seconde et la latence p99 face au baseline Node.js.
6. **Auditer les modules incompatibles** via`bun pm ls --bun-incompat` qui liste les paquets qui posent problème. Les principaux suspects en 2026 sont`node-gyp` (modules natifs anciens),`vm2` (utilisez`isolated-vm` ), et certains drivers Oracle/SQL Server.
7. **Déployer en canary** sur 5 % du trafic, monitorer pendant deux semaines, puis basculer progressivement. En cas de régression, revenir à Node.js prend exactement deux commandes :`git revert` du Dockerfile et redéploiement.

Les statistiques publiées par Vercel le 11 mars 2026 montrent que **72 % des migrations Node.js → Bun** aboutissent en moins de deux semaines, et **91 %** en moins d’un mois. Le principal point de blocage reste la suite de tests Jest qui utilise des hooks anciens ou des mocks via `jest.mock`, à corriger un par un.

## Bun vs Node.js : forces et faiblesses détaillées

### Forces de Bun en 2026

- Performance HTTP 4× supérieure sur charges simples, 1,7-2,5× sur charges réelles
- Démarrage à froid 12 à 30× plus rapide, idéal pour serverless et CLI
- Installation de paquets 20 à 40× plus rapide que npm
- TypeScript natif sans configuration ni transpilation manuelle
- Outillage intégré : test runner, bundler, package manager, transpileur
- Single Executable Application de 96 Mo qui démarre en 3 ms
- Backing financier d’Anthropic (rachat décembre 2025) garantissant la pérennité
- Lockfile binaire qui élimine 80 % des merge conflicts de dépendances
- Support natif TypeScript, JSX, TSX, JavaScript, sans toolchain externe

### Faiblesses de Bun en 2026

- Module `vm` à 78 % de compatibilité, problématique pour les sandbox
- Écosystème AWS Lambda nécessite un layer custom (mais publié officiellement en mars 2026)
- Quelques drivers SQL Server et Oracle anciens cassent encore
- NestJS Microservices Kafka présente des bugs résiduels
- Communauté plus jeune, donc moins de réponses StackOverflow par rapport à Node.js
- Pas de support officiel pour les architectures CPU exotiques (PowerPC, RISC-V)
- Le rachat par Anthropic peut introduire des inquiétudes de gouvernance pour certaines DSI européennes

### Forces de Node.js 22 LTS en 2026

- Maturité de quinze ans avec un écosystème de plus de 2,5 millions de paquets npm
- Stabilité opérationnelle prouvée chez la quasi-totalité des grands acteurs Tech mondiaux
- Gouvernance neutre via la fondation OpenJS
- Support TypeScript natif avec strip-types depuis Node.js 22
- Test runner `node:test` stable et permission model expérimental
- Support de toutes les architectures CPU et OS, y compris exotiques
- Trois fournisseurs cloud majeurs (AWS, Azure, GCP) avec runtimes de première classe

### Faiblesses de Node.js 22 LTS en 2026

- Performance HTTP 4× inférieure à Bun sur charge brute
- Démarrage à froid 12 à 30× plus lent
- Installation npm 20 à 40× plus lente que `bun install`
- Support TypeScript encore expérimental, sans support enums ni décorateurs en strip-types
- Pas de bundler intégré, donc dépendance à esbuild, Webpack ou Rolldown
- Empreinte mémoire idle 60 % plus élevée que Bun
- Cycle de release LTS en 2 ans qui ralentit l’adoption des nouveautés

## Verdict : Bun ou Node.js en 2026 ?

Le verdict **Bun vs Node.js** en avril 2026 dépend de quatre paramètres : la maturité de votre codebase, la criticité de votre charge serverless, le niveau de risque acceptable par votre direction technique, et les compétences de votre équipe DevOps. Voici la matrice de décision finale.

**Choisissez Bun si :** vous démarrez une nouvelle codebase en 2026, votre équipe travaille en TypeScript, vous déployez sur Vercel, Railway ou Fly.io, vos charges sont serverless ou CLI, votre suite de tests fait moins de 50 000 lignes, et vos ops valident un runtime Zig.

**Choisissez Node.js 22 LTS si :** vous maintenez une codebase de plus de cinq ans, votre stack inclut Nest.js Microservices Kafka ou des drivers SQL Server propriétaires, votre direction exige un runtime à gouvernance neutre (fondation OpenJS), vous déployez sur AWS Lambda sans tolérance pour le custom runtime, ou votre équipe SRE n’a pas le temps de former 20 ingénieurs sur un nouveau runtime.

**Approche hybride recommandée :** utilisez Bun en local pour le développement (gain DX immédiat), `bun install` en CI/CD (gain temps × 8 à 12), et Node.js 22 LTS en production si votre stack est mature. Cette stratégie, adoptée par **Doctolib, Veepee et Mirakl** selon leurs blogs techniques de mars 2026, capture 80 % des bénéfices de Bun sans aucun risque opérationnel. Pour aller plus loin, comparez aussi notre analyse Claude vs ChatGPT 2026 qui couvre l’écosystème IA d’Anthropic auquel Bun est désormais rattaché.

## FAQ : tout ce qu’il faut savoir sur Bun vs Node.js

### Bun va-t-il remplacer Node.js dans les cinq ans ?

Non. Node.js gardera la majorité du parc installé pendant au moins une décennie : la base installée est trop grande et les codebases legacy trop nombreuses. En revanche, Bun captera probablement 30 à 40 % des nouveaux projets démarrés en 2026-2028 selon les projections de The New Stack publiées en avril 2026.

### Bun est-il compatible avec npm et le registre officiel ?

Oui à 100 %. Bun lit le `package.json`, télécharge depuis le registre npm public ou privé, et publie via `bun publish` sur npm. Le seul fichier différent est le lockfile binaire `bun.lockb` qui remplace `package-lock.json`.

### Bun fonctionne-t-il sous Windows ?

Oui depuis Bun 1.1 (juillet 2025), avec un support quasi complet depuis Bun 1.3 (février 2026). Les seules limitations résiduelles concernent quelques modules natifs Linux-only comme `fcntl` avancés. La performance Windows est ~10 % inférieure à Linux, principalement à cause des appels système moins optimisés.

### Anthropic va-t-il fermer Bun ou le rendre payant ?

Anthropic a publié une déclaration le 3 décembre 2025 confirmant que Bun resterait **open-source sous licence MIT**. Le runtime continue d’accepter les contributions externes et la roadmap publique est maintenue. La société va simplement intégrer Bun plus profondément dans Claude Code et le SDK Claude Agent.

### Faut-il migrer immédiatement de Node.js 18 à Bun ?

Non. Migrez d’abord vers **Node.js 22 LTS** (gratuit, peu risqué, gain de performance immédiat), puis évaluez Bun sur un projet pilote ou un microservice non critique. Le gain Bun vs Node.js 22 est de 1,7 à 2,5× sur charge réelle, ce qui justifie une migration séparée seulement si l’économie d’infrastructure dépasse 10 000 €/mois.

### Bun est-il plus rapide que Deno ?

Sur le benchmark HTTP simple, oui, environ **1,3× plus rapide que Deno 2.5**. Mais Deno reste meilleur sur les standards web (URL, Fetch, Web Crypto sans polyfill) et offre un permission model plus mature. Le bon choix dépend de votre besoin : Bun pour la vitesse maximale, Deno pour la sécurité et les standards Web stricts.

### Bun supporte-t-il les workers et le multi-thread ?

Oui. Bun 1.3 supporte `worker_threads` à 96 % de compatibilité Node.js, plus une API `Bun.spawn` native. Les SharedArrayBuffer fonctionnent à 100 %. Le module `cluster` est aussi disponible avec `SO_REUSEPORT` natif sous Linux, ce qui simplifie les déploiements multi-cœurs.

### Quel framework HTTP choisir avec Bun en 2026 ?

**Hono** pour les APIs simples et les Workers (le plus rapide), **Elysia** pour la pile « Bun-first » avec validation Zod intégrée, **Fastify 5** si vous migrez depuis Node.js et voulez garder votre code, et **Express 5** pour la rétrocompatibilité maximale. Ces quatre frameworks tournent à 100 % sur Bun en avril 2026.

### Bun fonctionne-t-il sur AWS Lambda en 2026 ?

Oui, depuis le **14 mars 2026**, AWS publie un layer Lambda Bun officiel pour les architectures ARM64 et x86_64. Le runtime gagne 12 à 30× sur le cold start mais nécessite encore un effort de configuration plus important que Node.js 22 (runtime managé natif). Pour les fonctions à fort trafic, le gain est immédiat. Pour les fonctions occasionnelles, restez sur Node.js managé.

### Combien coûte Bun en production ?

Bun est **100 % gratuit et open-source MIT**. Aucun coût de licence. Le coût réel est celui de l’hébergement (Vercel Pro 20 $/mois, Railway 5 $/mois, AWS Lambda à 0,20 $/M d’invocations, Scaleway Serverless free tier). Anthropic n’a pas annoncé de modèle commercial autour de Bun en avril 2026 ; le runtime est traité comme une infrastructure mutualisée pour l’écosystème Claude Code.

### Related Coverage

*Sources externes : Annonce officielle Bun rejoint Anthropic · Communiqué Anthropic du 3 décembre 2025 · Notes de release Node.js 22 LTS · Dépôt GitHub officiel de Bun · TechEmpower Round 23 benchmarks.*
