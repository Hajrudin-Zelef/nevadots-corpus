Tailwind CSS est devenu, en 2026, le framework CSS utilitaire de référence pour les développeurs front-end en France et en Europe. Avec la version 4, l’outil a connu sa plus grande refonte depuis sa création : un nouveau moteur baptisé Oxide, une configuration entièrement pilotée par le CSS et des performances de compilation qui font passer les rebuilds de plusieurs millisecondes à quelques microsecondes. Selon le dépôt officiel `tailwindlabs/tailwindcss`, le projet dépasse les 93 500 étoiles sur GitHub (février 2026) et cumule désormais 307 releases publiées, la plus récente étant la **4.3.3**, taguée par Tailwind Labs le 16 juillet 2026 ; côté npm, Tailwind totalisait **36,4 millions de téléchargements hebdomadaires** début 2026 selon Programming-Helper, dont 17,7 millions pour la branche v4 (portée par des versions comme la 4.1.18) contre 6 millions encore sur la v3.4.19 LTS en janvier 2026, confirmant son adoption massive.

Ce tutoriel Tailwind CSS vous accompagne pas à pas, en 13 étapes, pour construire un projet complet et fonctionnel – une page de présentation responsive avec mode sombre, container queries et animations – en partant de zéro avec Vite. Vous y trouverez les prérequis avec versions exactes, plus de cinq blocs de code prêts à copier, les pièges les plus courants, des exemples de sortie console, une section dépannage de huit cas et des astuces avancées. À la fin, vous disposerez d’un workflow Tailwind CSS v4 de production que vous pourrez réutiliser sur n’importe quel projet.

## Tailwind CSS v4 : ce qui change vraiment en 2026

Avant de coder, il faut comprendre pourquoi Tailwind CSS v4 n’est pas une simple mise à jour incrémentale mais un changement de paradigme. La version 4.0, lancée le **22 janvier 2025** par Tailwind Labs et présentée par l’équipe comme une réécriture « entièrement nouvelle, optimisée pour la performance et la flexibilité », repose sur le moteur Oxide. La branche 4.3 a été lancée le 8 mai 2026 avec la version 4.3.0, qui a ajouté cinq nouveaux domaines d’utilitaires — container size, barres de défilement, zoom, `tab-size` et un `@variant` étendu, selon Changes.Watch —, suivie de la 4.3.1 le 12 juin 2026 d’après DevTalk, puis de la 4.3.2 le 29 juin 2026, une mise à jour corrective touchant quatre outils selon Tle Apps, et enfin de la 4.3.3 le 16 juillet 2026, publiée par Tailwind Labs et marquant la 307e release GitHub du projet. D’après endoflife.date, la **4.3.3** restait la dernière version activement supportée à cette date, la 4.3.0 ayant été suivie de ces trois correctifs en un peu plus de deux mois.

Le saut de performance est l’argument le plus visible. D’après le tableau de mesures publié par Tailwind dans l’annonce de la v4, un build complet passe de **378 ms en v3 à 100 ms en v4** (soit 3,78x plus rapide), un rebuild incrémental avec nouveau CSS chute de **44 ms à 5 ms** (8,8x), et un rebuild incrémental sans nouveau CSS s’effondre de **35 ms à 192 µs**, ce que l’équipe décrit comme **182x plus rapide**. Le 10 avril 2026, DevWharf a confirmé la tendance en mesurant, sur le moteur Rust/Lightning CSS de la v4, des builds complets jusqu’à **10 fois plus rapides** et des rebuilds incrémentaux jusqu’à **100 fois plus rapides** qu’en v3. Sur les gros projets, ces gains transforment l’expérience de développement : le rafraîchissement est instantané.

| Caractéristique | Tailwind v3 | Tailwind v4 (Oxide) | 
|---|---|---|
| Build complet | 378 ms | 100 ms (3,78x) | 
| Rebuild + nouveau CSS | 44 ms | 5 ms (8,8x) | 
| Rebuild sans nouveau CSS | 35 ms | 192 µs (182x) | 
| Configuration | `tailwind.config.js` | CSS-first ( `@theme` ) | 
| Import | `@tailwind base; ...` | `@import "tailwindcss"` | 
| Container queries | Plugin requis | Natif, sans plugin | 
| Tokens de design | JS | Variables CSS natives | 

Les autres nouveautés majeures à retenir : une **configuration CSS-first** qui supprime le fichier `tailwind.config.js` par défaut au profit de la directive `@theme` ; l’exposition de **tous les tokens de design en variables CSS natives**, utilisables partout dans vos feuilles de style ; le support **natif des container queries** sans plugin ; de nouveaux utilitaires de **transformation 3D** ; et une base bâtie sur les fonctionnalités CSS modernes comme `@property` et `color-mix()`. C’est précisément ce qui dicte les prérequis navigateur, que nous voyons à l’étape suivante.

## Prérequis et versions exactes pour ce tutoriel

Tailwind CSS v4 s’appuie sur des fonctionnalités CSS modernes. Le ciblage navigateur officiel est **Safari 16.4+, Chrome 111+ et Firefox 128+**. Si votre projet doit supporter des navigateurs plus anciens, restez sur Tailwind v3.4 – la v4 ne dégrade pas gracieusement vers les anciennes syntaxes. Ce guide s’appuie sur la version **4.3.3**, publiée le 16 juillet 2026 par Tailwind Labs comme 307e release du dépôt GitHub et dernière version activement supportée à cette date selon endoflife.date ; voici l’environnement exact utilisé dans ce guide.

| Outil | Version recommandée | Rôle | 
|---|---|---|
| Node.js | 20 LTS ou supérieur | Environnement d’exécution | 
| npm | 10+ | Gestionnaire de paquets | 
| Vite | Dernière version | Serveur de dev et bundler | 
| tailwindcss | 4.3 | Le framework | 
| @tailwindcss/vite | 4.3 | Plugin Vite officiel | 
| Navigateur | Chrome 111+ / Safari 16.4+ | Test et rendu | 
| VS Code | Dernière version | Éditeur (+ extension Tailwind) | 

Vérifiez d’abord votre version de Node.js. Tailwind v4 et les outils de build modernes nécessitent au minimum Node.js 20 LTS. Ouvrez un terminal et lancez les commandes suivantes.

```
node --version
# Sortie attendue : v20.x.x ou supérieur (v22.x.x recommandé)
npm --version
# Sortie attendue : 10.x.x ou supérieur
```
Si `node --version` renvoie une version inférieure à 20, mettez à jour Node.js depuis nodejs.org avant de continuer. Installez également l’extension officielle **Tailwind CSS IntelliSense** dans VS Code : elle offre l’autocomplétion des classes, le survol documenté et le linting, indispensables avec la v4 où le thème vit désormais dans le CSS.

## Étape 1 : Créer le projet Vite de base

Nous utilisons Vite comme outil de build car il offre le plugin Tailwind officiel le plus performant et un serveur de développement ultra-rapide. Créez un nouveau projet Vite en mode « vanilla » (HTML/JS pur) pour rester concentré sur Tailwind plutôt que sur un framework JavaScript.

```
npm create vite@latest mon-projet-tailwind -- --template vanilla
cd mon-projet-tailwind
npm install
```
La sortie console confirme la création de la structure du projet :

```
Scaffolding project in ./mon-projet-tailwind...
Done. Now run:
  cd mon-projet-tailwind
  npm install
  npm run dev
```
Vous obtenez une arborescence avec `index.html`, un dossier `src/` contenant `main.js` et `style.css`, ainsi que `package.json` et `vite.config.js` (que nous créerons s’il n’existe pas). Ce socle est volontairement minimal : nous allons y greffer Tailwind proprement.

## Étape 2 : Installer Tailwind CSS v4 et le plugin Vite

Le grand changement d’installation en v4 est l’arrivée d’un plugin Vite dédié, `@tailwindcss/vite`, qui remplace l’ancienne chaîne PostCSS + autoprefixer. Il est plus rapide et nécessite moins de configuration. Installez les deux paquets.

`npm install tailwindcss @tailwindcss/vite`
Notez la différence fondamentale avec la v3 : il n’y a **plus de commande** `npx tailwindcss init` pour générer un `tailwind.config.js`, et l’on n’installe ni `postcss` ni `autoprefixer` séparément. Le plugin Vite gère tout. Selon le contexte, Tailwind v4 propose désormais quatre points d’entrée officiels : `@tailwindcss/vite` pour Vite, `@tailwindcss/postcss` pour une chaîne PostCSS (utile avec Next.js), un plugin dédié pour **Webpack** introduit avec la version 4.2.0, publiée le 18 février 2026 selon InfoQ (aux côtés de quatre nouvelles palettes de couleurs), et `@tailwindcss/cli` pour une compilation en ligne de commande sans bundler.

| Intégration | Paquet | Cas d’usage | 
|---|---|---|
| Vite | `@tailwindcss/vite` | Vite, projets vanilla, React, Vue | 
| PostCSS | `@tailwindcss/postcss` | Next.js, Webpack, Laravel Mix | 
| CLI | `@tailwindcss/cli` | Build autonome, scripts, prototypes | 

## Étape 3 : Activer le plugin dans la configuration Vite

Créez (ou ouvrez) le fichier `vite.config.js` à la racine du projet et enregistrez le plugin Tailwind. C’est la seule configuration JavaScript que ce tutoriel requiert.

```
// vite.config.js
import { defineConfig } from 'vite'
import tailwindcss from '@tailwindcss/vite'
export default defineConfig({
  plugins: [
    tailwindcss(),
  ],
})
```
C’est tout. Le plugin scanne automatiquement vos fichiers `.html`, `.js`, `.jsx`, `.vue`, etc., détecte les classes Tailwind utilisées et génère uniquement le CSS nécessaire. Plus besoin de configurer manuellement le tableau `content` comme en v3 : la détection est désormais automatique et plus intelligente, ce qui élimine l’une des principales sources d’erreur de l’ancienne version.

## Étape 4 : Importer Tailwind dans votre feuille CSS

Ouvrez `src/style.css`, supprimez tout son contenu généré par Vite, et remplacez-le par une seule ligne. C’est ici que la philosophie CSS-first de la v4 prend tout son sens.

```
/* src/style.css */
@import "tailwindcss";
```
Cette unique directive `@import "tailwindcss"` remplace les trois anciennes lignes de la v3 (`@tailwind base; @tailwind components; @tailwind utilities;`). Elle charge le reset (Preflight), les composants et l’ensemble des utilitaires. Assurez-vous ensuite que ce fichier est bien importé dans votre point d’entrée JavaScript.

```
// src/main.js
import './style.css'
```
Lancez maintenant le serveur de développement pour vérifier que tout fonctionne.

```
npm run dev
# Sortie attendue :
  VITE v7.x.x  ready in 312 ms
  ->  Local:   http://localhost:5173/
  ->  Network: use --host to expose
  ->  press h + enter to show help
```
Ouvrez `http://localhost:5173/`. Le style par défaut du navigateur a changé (marges réinitialisées, police sans-serif) : c’est la preuve que le Preflight de Tailwind est actif. Si rien ne change, consultez la section dépannage en fin d’article.

## Étape 5 : Écrire votre premier composant avec les utilitaires

Remplacez le contenu du `<body>` de `index.html` par une structure de page d’accueil. Le principe de Tailwind est de composer le style directement dans le HTML via des classes utilitaires : `flex` pour la flexbox, `p-6` pour le padding, `text-xl` pour la taille de police, etc.

```
<!-- index.html (extrait du body) -->
<body class="bg-slate-50 text-slate-900 antialiased">
  <header class="border-b border-slate-200">
    <nav class="mx-auto flex max-w-5xl items-center justify-between p-6">
      <span class="text-lg font-bold tracking-tight">Studio Lumiere</span>
      <ul class="flex gap-6 text-sm font-medium">
        <li><a href="#" class="hover:text-indigo-600">Accueil</a></li>
        <li><a href="#" class="hover:text-indigo-600">Projets</a></li>
        <li><a href="#" class="hover:text-indigo-600">Contact</a></li>
      </ul>
    </nav>
  </header>
  <main class="mx-auto max-w-5xl px-6 py-20">
    <h1 class="text-4xl font-bold tracking-tight sm:text-6xl">
      Concevez plus vite avec Tailwind</h1>
    <p class="mt-6 max-w-2xl text-lg text-slate-600">
      Un studio de design qui transforme vos idees en interfaces
      performantes et accessibles.</p>
    <button class="mt-8 rounded-lg bg-indigo-600 px-5 py-3
      font-semibold text-white transition hover:bg-indigo-500">
      Demarrer un projet</button>
  </main>
</body>
```
Enregistrez le fichier : grâce au moteur Oxide et au HMR de Vite, la page se met à jour instantanément. Vous disposez déjà d’un en-tête, d’un titre responsive (le préfixe `sm:` agrandit la police sur écran moyen) et d’un bouton avec effet de survol. Chaque classe correspond à une propriété CSS – c’est la base du modèle utility-first.

## Étape 6 : Personnaliser le thème avec la directive @theme

Voici le cœur de la nouveauté v4. Pour personnaliser couleurs, polices, espacements ou points de rupture, on n’édite plus un fichier JavaScript : on déclare des variables CSS dans un bloc `@theme`. Ajoutez ce bloc dans `src/style.css`, juste après l’import.

```
/* src/style.css */
@import "tailwindcss";
@theme {
  --color-brand: #4f46e5;
  --color-brand-dark: #4338ca;
  --color-surface: #0f172a;
  --font-display: "Poppins", ui-sans-serif, system-ui, sans-serif;
  --spacing-section: 6rem;
  --breakpoint-3xl: 120rem;
}
```
Chaque variable génère automatiquement les utilitaires correspondants. `--color-brand` crée les classes `bg-brand`, `text-brand`, `border-brand`, etc. `--font-display` crée `font-display`. `--breakpoint-3xl` crée le préfixe responsive `3xl:`. Vous pouvez désormais remplacer `bg-indigo-600` par `bg-brand` dans votre bouton. Et puisque ces tokens sont de véritables variables CSS, vous pouvez aussi les consommer en CSS classique : `color: var(--color-brand);`.

### Une palette P3 et des variables accessibles partout

L’annonce de la v4 met en avant une palette de couleurs redessinée en espace **P3**, offrant des teintes plus vives sur les écrans modernes. Parce que tous les tokens sont exposés comme variables CSS natives, vous pouvez les manipuler dynamiquement en JavaScript ou les interpoler avec `color-mix()`, ce qui était impossible proprement quand la configuration vivait dans un fichier JS isolé du runtime.

## Étape 7 : Implémenter le mode sombre

Le mode sombre s’active avec le variant `dark:`. Par défaut, il suit la préférence système (`prefers-color-scheme`). Ajoutez les variantes sombres à vos éléments existants.

```
<body class="bg-slate-50 text-slate-900 antialiased
  dark:bg-slate-950 dark:text-slate-100">
<button class="mt-8 rounded-lg bg-brand px-5 py-3 font-semibold
  text-white transition hover:bg-brand-dark
  dark:bg-indigo-500 dark:hover:bg-indigo-400">
  Demarrer un projet</button>
```
Pour basculer le thème manuellement (avec un bouton plutôt que la préférence système), redéfinissez le variant `dark` en CSS pour qu’il réagisse à une classe sur l’élément `<html>`.

```
/* src/style.css */
@import "tailwindcss";
@custom-variant dark (&:where(.dark, .dark *));
```
Ajoutez ensuite un peu de JavaScript pour mémoriser le choix de l’utilisateur dans le `localStorage` – un comportement attendu sur tout site professionnel.

```
// src/main.js
import './style.css'
const root = document.documentElement
const stored = localStorage.getItem('theme')
if (stored === 'dark' ||
   (!stored && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
  root.classList.add('dark')
}
document.querySelector('#theme-toggle')?.addEventListener('click', () => {
  root.classList.toggle('dark')
  localStorage.setItem('theme',
    root.classList.contains('dark') ? 'dark' : 'light')
})
```
## Étape 8 : Rendre la page responsive

Tailwind est mobile-first : une classe sans préfixe s’applique à toutes les tailles, et les préfixes `sm:`, `md:`, `lg:`, `xl:`, `2xl:` ne s’activent qu’à partir du point de rupture indiqué. Construisons une grille de cartes de projets qui passe d’une colonne sur mobile à trois colonnes sur grand écran.

```
<section class="mx-auto max-w-5xl px-6 py-16">
  <div class="grid grid-cols-1 gap-6 md:grid-cols-2 lg:grid-cols-3">
    <article class="rounded-xl border border-slate-200 p-6
      dark:border-slate-800">
      <h3 class="font-display text-xl font-semibold">Identite visuelle</h3>
      <p class="mt-2 text-slate-600 dark:text-slate-400">
        Logos, chartes et systemes de design complets.</p>
    </article>
    <!-- Dupliquez l'article pour les autres projets -->
  </div>
</section>
```
Redimensionnez la fenêtre du navigateur : la grille passe de 1 à 2 puis 3 colonnes. Les points de rupture par défaut sont `sm` (40rem / 640px), `md` (48rem / 768px), `lg` (64rem / 1024px), `xl` (80rem / 1280px) et `2xl` (96rem / 1536px). Vous pouvez en ajouter via `@theme`, comme le `3xl` défini à l’étape 6.

## Étape 9 : Utiliser les container queries natives

Les **container queries**, l’une des grandes nouveautés de la v4, sont désormais intégrées sans aucun plugin. Contrairement aux media queries qui réagissent à la taille de l’écran, elles réagissent à la taille du *conteneur parent* – idéal pour des composants vraiment réutilisables. Marquez un conteneur avec `@container`, puis stylez ses enfants avec le préfixe `@`.

```
<div class="@container">
  <div class="flex flex-col gap-4 @md:flex-row @md:items-center">
    <img src="/avatar.jpg" alt="" class="size-16 rounded-full" />
    <div>
      <p class="font-semibold">Camille Roux</p>
      <p class="text-sm text-slate-500">Directrice artistique</p>
    </div>
  </div>
</div>
```
Ici, `@md:flex-row` ne s’applique pas selon la largeur de l’écran mais selon celle du conteneur `@container`. Placez ce composant dans une barre latérale étroite ou dans une zone large : il s’adapte tout seul à l’espace disponible. C’est un changement profond pour la conception de bibliothèques de composants. Pour approfondir, la documentation MDN sur les container queries CSS détaille la spécification sous-jacente.

## Étape 10 : Factoriser des composants réutilisables

Répéter de longues listes de classes nuit à la lisibilité. Deux approches existent en v4. La première, recommandée par l’équipe Tailwind, consiste à utiliser des composants de votre framework (un composant `Button` en React ou Vue). La seconde, en CSS pur, s’appuie sur `@apply` à l’intérieur d’une couche de composants.

```
/* src/style.css */
@import "tailwindcss";
@layer components {
  .btn-primary {
    @apply rounded-lg bg-brand px-5 py-3 font-semibold text-white
           transition hover:bg-brand-dark
           focus:outline-none focus:ring-2 focus:ring-brand focus:ring-offset-2;
  }
}
```
Vous pouvez maintenant écrire `<button class="btn-primary">Envoyer</button>` partout. Attention toutefois : abuser de `@apply` recrée le problème que Tailwind cherche à résoudre (du CSS dispersé et difficile à maintenir). Réservez-le aux motifs vraiment récurrents comme les boutons, les champs de formulaire ou les badges.

### Exemple : un champ de formulaire accessible

```
@layer components {
  .input-field {
    @apply w-full rounded-md border border-slate-300 px-4 py-2 text-slate-900
           placeholder:text-slate-400
           focus:border-brand focus:ring-2 focus:ring-brand/30
           dark:border-slate-700 dark:bg-slate-900 dark:text-slate-100;
  }
}
```
L’utilitaire `ring-brand/30` illustre la syntaxe d’opacité moderne : le `/30` applique 30 % d’opacité à la couleur, en interne via `color-mix()`. C’est plus lisible et plus performant que les anciens utilitaires d’opacité séparés.

## Étape 11 : Ajouter transitions et transformations 3D

La v4 introduit des utilitaires de **transformation 3D** natifs, permettant de manipuler les éléments dans l’espace directement depuis le HTML. Combinez `perspective`, `rotate-x`, `rotate-y` et `transform-3d` pour un effet de carte qui pivote au survol.

```
<div class="perspective-distant">
  <article class="transform-3d transition-transform duration-500
    hover:rotate-y-12 hover:rotate-x-6
    rounded-xl bg-white p-8 shadow-lg dark:bg-slate-900">
    <h3 class="text-xl font-semibold">Carte interactive</h3>
    <p class="mt-2 text-slate-600 dark:text-slate-400">
      Survolez cette carte pour voir l'effet 3D.</p>
  </article>
</div>
```
Pour les animations d’apparition, combinez les utilitaires `transition`, `duration-*` et `ease-*`. Tailwind v4 a aussi élargi les utilitaires de dégradés (dégradés coniques et radiaux) et ajouté `color-mix()` au cœur du moteur, ce qui rend les transitions de couleur bien plus douces qu’auparavant.

## Étape 12 : Compiler pour la production

Tailwind ne génère que les classes réellement utilisées : votre CSS de production reste minuscule, souvent quelques kilo-octets après compression. Lancez le build Vite.

```
npm run build
# Sortie attendue :
vite v7.x.x building for production...
14 modules transformed.
dist/index.html                  1.85 kB | gzip:  0.74 kB
dist/assets/index-a1b2c3d4.css   9.42 kB | gzip:  2.61 kB
dist/assets/index-e5f6g7h8.js    1.12 kB | gzip:  0.63 kB
built in 487ms
```
Le fichier CSS final ne pèse ici que 9,42 ko (2,61 ko gzippé) car le moteur Oxide a éliminé toutes les classes inutilisées. Testez le résultat localement avec `npm run preview` avant de déployer le dossier `dist/` sur votre hébergeur (Vercel, Netlify, Cloudflare Pages, ou un serveur classique). Aucune étape de purge manuelle n’est nécessaire : c’est automatique.

## Étape 13 : Le projet complet assemblé

Voici la feuille de style finale qui regroupe thème, mode sombre et composants – le fichier `src/style.css` complet de notre projet de production.

```
/* src/style.css -- version finale */
@import "tailwindcss";
@custom-variant dark (&:where(.dark, .dark *));
@theme {
  --color-brand: #4f46e5;
  --color-brand-dark: #4338ca;
  --font-display: "Poppins", ui-sans-serif, system-ui, sans-serif;
  --spacing-section: 6rem;
  --breakpoint-3xl: 120rem;
}
@layer components {
  .btn-primary {
    @apply rounded-lg bg-brand px-5 py-3 font-semibold text-white
           transition hover:bg-brand-dark
           focus:outline-none focus:ring-2 focus:ring-brand focus:ring-offset-2;
  }
  .input-field {
    @apply w-full rounded-md border border-slate-300 px-4 py-2
           focus:border-brand focus:ring-2 focus:ring-brand/30
           dark:border-slate-700 dark:bg-slate-900 dark:text-slate-100;
  }
}
```
Avec ces trois fichiers – `vite.config.js`, `src/style.css` et `index.html` – vous disposez d’un projet Tailwind CSS v4 complet, responsive, accessible, avec mode sombre persistant, container queries et animations 3D. La structure est volontairement framework-agnostique : copiez la même configuration dans un projet React, Vue ou Svelte et tout fonctionne à l’identique.

## 5 pièges courants à éviter avec Tailwind v4

La migration vers la v4 et le modèle CSS-first introduisent des erreurs spécifiques que rencontrent la plupart des développeurs. Voici les cinq plus fréquentes et comment les contourner.

- **Conserver les anciennes directives `@tailwind`.** En v4,`@tailwind base; @tailwind components; @tailwind utilities;` ne fonctionne plus. Utilisez l’unique`@import "tailwindcss";` . C’est l’erreur n°1 lors d’une migration.
- **Chercher un `tailwind.config.js`.** Il n’existe plus par défaut. Toute personnalisation passe par`@theme` dans le CSS. Si vous avez vraiment besoin d’un fichier JS (plugin tiers), chargez-le explicitement avec`@config "./tailwind.config.js";` .
- **Mélanger la chaîne PostCSS de la v3.** Ne gardez ni`postcss` , ni`autoprefixer` , ni`postcss-import` si vous utilisez`@tailwindcss/vite` . Ils entrent en conflit avec le plugin et provoquent des doublons de préfixes.
- **Cibler des navigateurs trop anciens.** La v4 exige Safari 16.4+, Chrome 111+ et Firefox 128+. Si votre audience utilise des navigateurs antérieurs, restez sur Tailwind 3.4 – la v4 ne propose pas de fallback automatique.
- **Abuser de `@apply`.** Recréer des dizaines de classes`.card` ,`.title` ,`.wrapper` avec`@apply` annule l’intérêt du modèle utility-first. Préférez des composants de framework et réservez`@apply` aux quelques motifs vraiment répétés.

## Dépannage : 8 problèmes fréquents et leurs solutions

Si quelque chose ne fonctionne pas, ce tableau couvre les huit symptômes les plus signalés par la communauté francophone et internationale.

| Symptôme | Cause probable | Solution | 
|---|---|---|
| Aucun style appliqué | `style.css` non importé dans`main.js` | Ajouter `import './style.css'` | 
| Les classes ne s’actualisent pas | Plugin Vite absent de `vite.config.js` | Vérifier `plugins: [tailwindcss()]` | 
| `Cannot find module @tailwindcss/vite` | Paquet non installé | `npm i @tailwindcss/vite` | 
| `@tailwind` sans effet | Syntaxe v3 obsolète | Remplacer par `@import "tailwindcss"` | 
| Classe personnalisée ignorée | Variable absente de `@theme` | Déclarer le token dans `@theme` | 
| Mode sombre ne bascule pas | `@custom-variant dark` manquant | Ajouter la déclaration + classe `dark` | 
| CSS énorme en production | Build non lancé / cache | `npm run build` et vider`dist/` | 
| Couleurs ternes | Navigateur sans support P3 | Mettre à jour le navigateur | 

En cas de doute, supprimez le dossier `node_modules` et le fichier `package-lock.json`, puis relancez `npm install`. Une grande partie des erreurs de Tailwind v4 vient de versions mixtes (un paquet en 3.x, un autre en 4.x) restées dans le cache. Le guide de migration officiel détaille par ailleurs chaque changement entre la v3 et la v4.

## Astuces avancées pour aller plus loin

Une fois les bases maîtrisées, plusieurs techniques font passer votre productivité au niveau supérieur. Première astuce : exploitez les **valeurs arbitraires** entre crochets, par exemple `top-[117px]` ou `bg-[#1da1f2]`, pour les cas où aucun utilitaire prédéfini ne convient – sans quitter le HTML. Tailwind les génère à la volée.

Deuxième astuce : combinez les variants. `dark:hover:lg:bg-brand` est parfaitement valide et applique le fond seulement en mode sombre, au survol, sur grand écran. Cette composabilité élimine quasiment tout besoin d’écrire des media queries manuelles. Troisième astuce : utilisez le variant `group` et `peer` pour styliser un élément en fonction de l’état d’un parent ou d’un frère – par exemple afficher un libellé quand un champ reçoit le focus, avec `peer-focus:text-brand`.

Enfin, pour les équipes : adoptez l’écosystème officiel. **Tailwind Plus** (anciennement Tailwind UI) fournit des centaines de composants prêts à l’emploi, et **Catalyst** propose un kit de composants React entièrement typé. Ces produits officiels, recensés sur le site de Tailwind, accélèrent considérablement la mise en place d’interfaces cohérentes tout en restant 100 % compatibles avec la configuration v4 décrite ici.

## Intégration avec Next.js, Laravel et les autres

Ce tutoriel utilise Vite, mais la v4 s’intègre à tout l’écosystème. Pour **Next.js**, qui repose sur une chaîne PostCSS, installez `@tailwindcss/postcss` et déclarez-le dans `postcss.config.mjs` au lieu du plugin Vite. Pour **Laravel**, le plugin Vite officiel s’utilise exactement comme dans ce guide puisque Laravel embarque Vite depuis plusieurs versions. Pour un site statique sans bundler, le paquet `@tailwindcss/cli` compile votre CSS en une commande.

```
# Chaine PostCSS (Next.js, Webpack)
npm install tailwindcss @tailwindcss/postcss
# CLI autonome (sites statiques, prototypes)
npm install tailwindcss @tailwindcss/cli
npx @tailwindcss/cli -i ./src/input.css -o ./dist/output.css --watch
```
Dans tous les cas, le fichier CSS reste identique : un `@import "tailwindcss";` suivi de votre bloc `@theme`. C’est l’un des grands atouts de la v4 – la logique de personnalisation est portable d’un environnement à l’autre. Que vous travailliez sur une application Next.js 16 ou une interface React 19.2, votre thème Tailwind se transpose sans modification.

## Tailwind v4 face au CSS classique et à Bootstrap

Beaucoup de développeurs français se demandent encore s’il faut adopter Tailwind ou rester sur une approche traditionnelle. La différence tient au modèle mental. Avec le CSS classique ou un préprocesseur comme Sass, vous écrivez des sélecteurs et des règles dans des fichiers séparés, puis vous jonglez entre HTML et CSS en inventant des noms de classes (le fameux problème du nommage BEM). Avec Tailwind, vous composez l’interface directement dans le balisage à l’aide d’utilitaires atomiques, sans changer de contexte et sans inventer de noms. Le CSS final ne contient que ce que vous utilisez réellement.

Face à Bootstrap, la distinction est encore plus nette. Bootstrap fournit des composants pré-stylés (boutons, cartes, modales) avec une apparence reconnaissable, ce qui accélère le prototypage mais uniformise les interfaces. Tailwind, lui, ne fournit aucun composant tout fait par défaut : il vous donne des briques de bas niveau pour construire *votre* design system. Résultat : des interfaces uniques, un poids CSS minimal et une maintenance simplifiée. Le tableau ci-dessous résume les principaux arbitrages.

| Critère | CSS / Sass classique | Bootstrap 5 | Tailwind CSS v4 | 
|---|---|---|---|
| Approche | Sélecteurs nommés | Composants prêts | Utilitaires atomiques | 
| Nommage de classes | Manuel (BEM) | Imposé | Aucun à inventer | 
| Personnalisation | Totale mais lente | Limitée (overrides) | Totale via `@theme` | 
| Poids CSS final | Croît avec le projet | ~25 ko+ (gzip) | Quelques ko (purge auto) | 
| Uniformité visuelle | Variable | Forte (reconnaissable) | Sur-mesure | 
| Courbe d’apprentissage | Faible | Faible | Moyenne (mémoriser les classes) | 

Le bon choix dépend du contexte. Pour un prototype jetable à livrer en une journée, Bootstrap reste pertinent. Pour une application maintenue sur le long terme, avec une identité visuelle propre et des contraintes de performance, Tailwind v4 est aujourd’hui le standard de l’industrie : selon l’enquête State of CSS 2025 relayée par AIBase News en janvier 2026, 51 % des développeurs déclarent l’utiliser, pour 75 millions de téléchargements mensuels, et W3Techs mesurait au 4 septembre 2026 une part de marché de 1,7 % des sites dont la technologie CSS est identifiée (0,3 % de l’ensemble du web), tandis que TechnologyChecker.io détectait déjà Tailwind CSS sur 2 787 808 domaines au 28 mars 2026 – une adoption confirmée par les enquêtes annuelles auprès des développeurs front-end et par l’intégration dans des frameworks majeurs.

## Optimiser performance et accessibilité en production

Un site Tailwind bien construit est naturellement léger, mais quelques bonnes pratiques font la différence en production. Côté performance, laissez le moteur Oxide gérer la purge – ne désactivez jamais la détection automatique – et servez votre CSS compressé (gzip ou Brotli) depuis votre hébergeur. Sur Vercel, Netlify ou Cloudflare Pages, cette compression est activée par défaut. Évitez d’injecter des feuilles de style externes redondantes qui annuleraient le bénéfice de la purge.

Côté accessibilité (un enjeu réglementaire en Europe avec le RGAA et la directive européenne sur l’accessibilité), Tailwind facilite les bonnes pratiques sans les imposer. Utilisez systématiquement les variants d’état pour les interactions clavier : `focus-visible:ring-2` garantit un anneau de focus visible pour les utilisateurs au clavier, sans gêner les utilisateurs à la souris. Pensez aussi au contraste : préférez des paires de couleurs validées (par exemple `text-slate-900` sur `bg-white`) et testez-les avec un outil de contraste.

```
<!-- Bouton accessible au clavier et au lecteur d'ecran -->
<button type="button"
  class="rounded-lg bg-brand px-5 py-3 font-semibold text-white
         hover:bg-brand-dark
         focus-visible:outline-none focus-visible:ring-2
         focus-visible:ring-brand focus-visible:ring-offset-2"
  aria-label="Envoyer le formulaire de contact">
  Envoyer
</button>
```
Deux derniers réflexes utiles : la classe utilitaire `sr-only` masque visuellement un texte tout en le laissant lisible par les lecteurs d’écran (parfait pour les libellés d’icônes), et le variant `motion-reduce:` permet de désactiver vos animations 3D pour les utilisateurs ayant activé la réduction de mouvement dans leur système – par exemple `motion-reduce:transition-none`. Ces détails, triviaux à ajouter avec Tailwind, font la différence entre une démo et un produit réellement déployable en Europe.

## Foire aux questions sur Tailwind CSS v4

### Le fichier tailwind.config.js a-t-il vraiment disparu ?

Oui, par défaut. En v4, la configuration se fait dans le CSS via la directive `@theme`. Le fichier JavaScript n’est plus généré ni nécessaire. Vous pouvez toutefois le réintroduire manuellement si un plugin tiers l’exige, en le chargeant avec `@config "./tailwind.config.js";` en haut de votre feuille de style.

### Dois-je migrer mes projets v3 vers v4 ?

Si votre projet cible des navigateurs récents (Chrome 111+, Safari 16.4+, Firefox 128+), la migration vaut largement le coup pour les gains de performance – jusqu’à 182x sur les rebuilds incrémentaux selon les mesures officielles. Tailwind fournit un outil de migration automatique, `npx @tailwindcss/upgrade`, qui convertit la plupart du code. D’après EOSL.date, 20 versions de Tailwind CSS étaient déjà en fin de vie en juillet 2026 et seules deux branches restaient activement supportées à cette date – la 4.2 ayant elle-même atteint sa fin de vie le 8 mai 2026 – ce qui rend la migration vers une branche 4.3.x maintenue d’autant plus recommandée. Tailwind Labs a par ailleurs lancé **Tailwind CSS 5.0** début juillet 2026 comme nouvelle version majeure ; ce guide reste construit sur la branche 4.3.x, encore massivement déployée en production, mais gardez un œil sur le guide de migration officiel si vous envisagez de sauter directement vers la v5. Pour les projets devant supporter d’anciens navigateurs, restez sur la v3.4.

### Pourquoi Tailwind v4 est-il si rapide ?

Le nouveau moteur Oxide repense complètement le pipeline de compilation. Résultat : des builds complets jusqu’à 5x plus rapides et des rebuilds incrémentaux mesurés en microsecondes plutôt qu’en millisecondes. Concrètement, le rafraîchissement à chaud (HMR) devient imperceptible, même sur de grandes bases de code.

### Quelle différence entre media queries et container queries ?

Une media query (`md:`) réagit à la taille de la fenêtre du navigateur. Une container query (`@md:`) réagit à la taille du conteneur parent marqué `@container`. Les secondes rendent les composants vraiment réutilisables : un même composant s’adapte qu’il soit placé dans une barre latérale étroite ou dans une zone principale large. La v4 les intègre nativement, sans plugin.

### Faut-il encore configurer la purge des classes inutilisées ?

Non. La détection des classes est automatique en v4 : le moteur scanne vos fichiers source et ne génère que le CSS utilisé. Il n’y a plus de tableau `content` à maintenir comme en v3, ce qui supprime une source d’erreur fréquente (classes manquantes en production). Votre CSS final reste minimal sans aucune intervention.

### Vite ou PostCSS : que choisir ?

Si votre projet utilise Vite (y compris React, Vue, Svelte ou Laravel), choisissez `@tailwindcss/vite` : c’est l’intégration la plus rapide et la plus simple. Si vous êtes sur Next.js, Webpack ou une chaîne PostCSS existante, utilisez `@tailwindcss/postcss`. Le reste du code – import et thème – est identique dans les deux cas.

### Tailwind est-il adapté aux débutants ?

Oui, à condition de connaître les bases du CSS, car chaque classe utilitaire correspond à une propriété CSS. L’extension VS Code « Tailwind CSS IntelliSense » facilite grandement l’apprentissage en suggérant les classes et en affichant le CSS généré au survol. La configuration CSS-first de la v4 réduit encore la barrière d’entrée en supprimant le fichier de configuration JavaScript.

## Conclusion : un workflow Tailwind v4 prêt pour la production

En 13 étapes, vous avez bâti un projet Tailwind CSS v4 complet : installation via le plugin Vite, configuration CSS-first avec `@theme`, mode sombre persistant, design responsive, container queries natives, composants factorisés et transformations 3D, le tout compilé en un CSS de production de quelques kilo-octets. Le passage à la v4 et au moteur Oxide n’est pas qu’une question de vitesse : c’est une simplification profonde du modèle mental, où le CSS redevient la seule source de vérité.

Pour aller plus loin, explorez les composants officiels de Tailwind Plus, expérimentez avec les nouveaux utilitaires de la v4.3, et appliquez cette configuration à vos frameworks favoris. La portabilité du thème entre Vite, PostCSS et CLI fait de Tailwind v4 un choix sûr pour tout projet front-end européen en 2026, d’autant que l’adoption en entreprise a bondi à **35 % des nouveaux projets en 2026** (contre 18 % en 2024, selon les enquêtes sectorielles de Tech Insider publiées en mars 2026) et que **40 % des startups SaaS** l’utilisaient déjà en mars 2026 d’après les données BuiltWith relayées par Tech Insider.

### À lire également

*Sources externes : annonce officielle Tailwind CSS v4, documentation d’installation Vite, dépôt GitHub tailwindlabs/tailwindcss, Vite.*