# Guide HTML & CSS — Le manuel complet

> **Pour Zelef** — chef de service systèmes & énergies : pages internes, dashboards, documentation.
> Ton : direct, tutoriel + référence, dense. Zéro blabla, que de l'utile.
> **Convention de ce guide** : chaque section numérotée `## N.` est autonome. Les blocs de code sont valides HTML5/CSS3 et copiables tels quels.

---

## 1. Bienvenue : comment utiliser ce guide

Ce guide couvre **HTML5** de A à Z + une **introduction solide à CSS** (sélecteurs, box model, flexbox, grid, responsive, variables). Il est pensé pour un usage pro interne :

- **Sections 1–10** : prise en main, tooling, syntaxe de base.
- **Sections 11–35** : HTML en profondeur (texte, liens, images, tableaux, formulaires, médias, sémantique).
- **Sections 36–49** : SEO, accessibilité, ARIA.
- **Sections 50–75** : CSS complet pour mettre en page (flexbox, grid, responsive).
- **Sections 76–88** : bonnes pratiques, erreurs classiques, cas pratiques commentés, pense-bête, glossaire, quiz.

**Méthode de travail conseillée :**

1. Lis les sections 1 à 10 d'une traite (30 min).
2. Fais les cas pratiques (sections 80–83) en tapant le code toi-même.
3. Reviens aux sections de référence (tableaux d'attributs, pense-bête §85) quand tu codes.

> 💡 Tout le code de ce guide est en HTML5 sémantique moderne. Aucune balise obsolète (`<font>`, `<center>`, `<table>` de mise en page…).

---

## 2. C'est quoi, HTML ?

**HTML** (HyperText Markup Language) est le langage de **balisage** qui décrit la **structure** d'une page web. Il ne fait pas la mise en forme (c'est CSS) ni l'interactivité (c'est JavaScript). Retiens le trio :

| Couche      | Rôle                | Langage    |
|-------------|---------------------|------------|
| Structure   | Quoi afficher       | HTML       |
| Présentation| Comment l'afficher  | CSS        |
| Comportement| Ce que ça fait      | JavaScript |

Exemple minimal :

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="utf-8">
  <title>Ma première page</title>
</head>
<body>
  <h1>Bonjour</h1>
  <p>Ceci est un paragraphe.</p>
</body>
</html>
```

Points clés :

- HTML est un langage de **balises** : `<h1>…</h1>` encadre du contenu.
- Les navigateurs **interprètent** le HTML et construisent le **DOM** (Document Object Model), un arbre d'objets manipulable.
- HTML est **tolérant** : un document mal formé s'affiche quand même — mais un code propre évite 90 % des bugs.

---

## 3. Histoire rapide : pourquoi HTML5

| Année | Version | Ce qui change |
|-------|---------|---------------|
| 1991  | HTML (Tim Berners-Lee) | 18 balises, pages scientifiques |
| 1997  | HTML 4.01 | Standard du web « classique » |
| 2000  | XHTML | HTML strict façon XML (échec d'adoption) |
| 2014  | **HTML5** | Sémantique (`<header>`, `<article>`…), `<video>`, `<audio>`, `<canvas>`, formulaires enrichis, API |
| 2021+ | HTML Living Standard | Le HTML n'a plus de numéro : c'est un standard **vivant** maintenu par le WHATWG |

**Ce que HTML5 a apporté d'essentiel :**

- Balises **sémantiques** : le code décrit le *sens*, pas juste l'apparence.
- Médias **natifs** (`<video>`, `<audio>`) : fini Flash.
- Formulaires intelligents : `type="email"`, `required`, `date`, validation native.
- API : géolocalisation, stockage local, drag & drop, etc.

> 🎯 Dans ce guide, « HTML » = HTML5 / HTML Living Standard. Tout le reste est de l'histoire.

---

## 4. Tooling : éditeurs et environnement

### Éditeurs recommandés

| Éditeur | Points forts | Prix |
|---------|--------------|------|
| **VS Code** | Standard de fait, extensions HTML/CSS, Emmet intégré, Live Server | Gratuit |
| Sublime Text | Ultra rapide, léger | Gratuit / licence |
| WebStorm | IDE complet, refactorings puissants | Payant |
| Notepad++ | Léger, Windows | Gratuit |

### Extensions VS Code indispensables

- **Live Server** : recharge la page à chaque sauvegarde (`Alt+L, Alt+O`).
- **Prettier** : formate le code automatiquement à la sauvegarde.
- **Emmet** : déjà intégré — tape `!` + `Tab` pour générer un squelette HTML5.
- **HTMLHint** / **W3C Validator** : signale les erreurs en direct.
- **axe Accessibility Linter** : repère les problèmes d'accessibilité.

### Raccourci Emmet à connaître

Tape puis `Tab` :

| Abréviation | Résultat |
|-------------|----------|
| `!` | Squelette HTML5 complet |
| `ul>li*3` | `<ul>` avec 3 `<li>` |
| `form>input:text+input:submit` | Formulaire avec champ texte + bouton |
| `table>tr*2>td*3` | Tableau 2 lignes × 3 colonnes |

### Organisation conseillée d'un projet

```text
mon-site/
├── index.html
├── css/
│   └── style.css
├── js/
│   └── main.js
├── img/
│   └── logo.svg
└── docs/
    └── procedure-onduleur.html
```

> 💡 Sépare toujours structure (HTML), présentation (CSS) et comportement (JS) en fichiers distincts. C'est la base de tout projet maintenable.

---

## 5. Tooling : DevTools du navigateur

Les **outils de développement** (F12 ou `Ctrl+Maj+I`) sont ton meilleur ami. Onglets essentiels :

| Onglet | Usage |
|--------|-------|
| **Elements** | Inspecte le DOM, modifie HTML/CSS en direct |
| **Console** | Erreurs JS, tests rapides |
| **Network** | Temps de chargement, requêtes, poids des images |
| **Lighthouse** | Audit perf / accessibilité / SEO en 1 clic |
| **Application** | Stockage local, cookies |

**Réflexes à prendre :**

1. `Clic droit → Inspecter` sur n'importe quel élément pour voir son HTML/CSS.
2. Dans **Elements**, double-clique un style CSS pour le modifier en direct (sans toucher au fichier).
3. **Lighthouse** (`Onglet Lighthouse → Analyze page load`) : vise un score ≥ 90 en Performance et Accessibilité pour tes pages internes.
4. L'émulateur mobile (`Ctrl+Maj+M`) : teste ton responsive sans téléphone.

> 🎯 80 % du débogage HTML/CSS se fait dans l'onglet Elements. Apprivoise-le avant tout.

---

## 6. Validation W3C : ton contrôle qualité

Le **validateur du W3C** (https://validator.w3.org) vérifie que ton HTML respecte le standard. C'est gratuit et sans compte.

**3 façons de valider :**

1. **Par URL** : colle l'adresse de ta page (si elle est en ligne).
2. **Par upload** : envoie ton fichier `.html`.
3. **Par saisie directe** : colle ton code.

**Ce que la validation détecte :**

- Balises non fermées ou mal imbriquées (`<p><div></p></div>`)
- Attributs inconnus ou mal orthographiés
- `id` en double dans la page
- `<img>` sans `alt`
- Mauvais placement (`<div>` dans `<head>`, etc.)

**Bonnes pratiques :**

- Vise **zéro erreur** avant chaque mise en production (les avertissements sont tolérables s'ils sont compris).
- Intègre la validation dans ta routine : coder → tester dans le navigateur → valider → corriger.
- Les frameworks génèrent parfois du HTML imparfait : valide le **rendu final** (code source de la page affichée).

> ⚠️ Un HTML valide n'est pas une garantie d'accessibilité ni de bon SEO — mais un HTML invalide est une garantie de problèmes.

---

## 7. Anatomie d'un document HTML

Voici le squelette complet, à connaître par cœur :

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Titre de la page — Nom du site</title>
  <meta name="description" content="Description courte pour les moteurs de recherche.">
  <link rel="stylesheet" href="css/style.css">
  <link rel="icon" href="img/favicon.ico">
</head>
<body>
  <header>
    <h1>Titre principal</h1>
  </header>
  <main>
    <p>Contenu…</p>
  </main>
  <footer>
    <p>© 2026 Mon service</p>
  </footer>
  <script src="js/main.js"></script>
</body>
</html>
```

| Zone | Rôle | Visible ? |
|------|------|-----------|
| `<!DOCTYPE html>` | Déclare HTML5, évite le quirks mode (§8) | Non |
| `<html lang="fr">` | Racine du document ; `lang` = langue (crucial pour lecteurs d'écran et SEO) | Non |
| `<head>` | Métadonnées : titre, charset, CSS, SEO | Non |
| `<body>` | Tout le contenu affiché | **Oui** |

**Règles d'or :**

- **Un seul** `<h1>` par page (= le sujet de la page).
- Les `<script>` en fin de `<body>` (ou avec `defer`) pour ne pas bloquer l'affichage.
- `lang="fr"` toujours renseigné — un lecteur d'écran prononce différemment selon la langue.

---

## 8. Doctype et quirks mode

La toute première ligne, `<!DOCTYPE html>`, n'est pas une balise : c'est une **instruction** au navigateur.

```html
<!DOCTYPE html>
```

**Sans doctype** (ou avec un doctype incorrect), le navigateur bascule en **quirks mode** (« mode déglingué ») : il imite les bugs d'Internet Explorer 5 pour afficher les très vieilles pages. Conséquences :

- Le **box model** est calculé différemment (§56) — tes largeurs CSS deviennent fausses.
- Des comportements CSS incohérents entre navigateurs.

**Checklist :**

- [ ] `<!DOCTYPE html>` en **première ligne**, rien avant (pas même un commentaire ni une ligne vide — certains vieux navigateurs bronchent).
- [ ] Écrit exactement comme ça : insensible à la casse, mais la forme minuscule est la convention.
- [ ] Ne **jamais** utiliser les vieux doctypes (`HTML 4.01 Transitional…`) : ils déclenchent le mode presque-standard.

> 💡 Si ta mise en page CSS semble « cassée sans raison », vérifie d'abord le doctype. C'est la cause n°1 des mystères de débutant.

---

## 9. Syntaxe : balises, attributs, imbrication

### Balise ouvrante / fermante

```html
<p>Ceci est un paragraphe.</p>
```

- `<p>` = balise ouvrante, `</p>` = balise fermante (note le `/`).
- Certaines balises sont **orphelines** (auto-fermantes) : pas de contenu, pas de balise fermante.

```html
<img src="logo.png" alt="Logo">
<br>
<hr>
<input type="text">
<meta charset="utf-8">
```

> En HTML5, `<br>` suffit (la forme XHTML `<br />` est tolérée mais inutile).

### Attributs

```html
<a href="https://example.com" target="_blank" rel="noopener">Lien</a>
```

| Règle | Exemple |
|-------|---------|
| Toujours entre **guillemets doubles** | `href="page.html"` ✅ / `href=page.html` ⚠️ |
| Noms en **minuscules** | `class`, pas `CLASS` |
| Attributs booléens : présence = vrai | `<input required>` équivaut à `required="required"` |
| Pas d'espaces autour du `=` (convention) | `id="menu"` ✅ |

### Imbrication : la règle du poupée russe

```html
<!-- ✅ Correct : fermeture dans l'ordre inverse -->
<p>Un <strong>mot important</strong> ici.</p>

<!-- ❌ Incorrect : chevauchement -->
<p>Un <strong>mot important</p></strong>
```

### Les 3 familles de balises

| Famille | Exemples | Comportement |
|---------|----------|--------------|
| **Block** | `<p>`, `<div>`, `<h1>`, `<ul>` | Occupe toute la largeur, retour à la ligne avant/après |
| **Inline** | `<a>`, `<strong>`, `<span>`, `<img>` | Dans le flux du texte, pas de retour à la ligne |
| **Inline-block** (via CSS) | — | Comme inline mais avec largeur/hauteur réglables |

---

## 10. Commentaires et entités HTML

### Commentaires

```html
<!-- Ceci est un commentaire : invisible à l'écran, visible dans le code source -->

<!--
  Commentaire
  sur plusieurs lignes —
  pratique pour structurer un long fichier
-->

<!-- TODO: ajouter le tableau des onduleurs quand les specs arrivent -->
```

**Usages pro :** baliser les grandes zones (`<!-- /header -->`), laisser des TODO, désactiver temporairement du code.

> ⚠️ Les commentaires sont **visibles** dans le code source (Ctrl+U). N'y mets jamais d'infos sensibles (mots de passe, remarques sur des collègues…).

### Entités HTML (caractères spéciaux)

Certains caractères ont un sens en HTML : pour les afficher, on utilise des **entités**.

| Caractère | Entité | Usage |
|-----------|--------|-------|
| `<` | `&lt;` | Afficher du code : `&lt;div&gt;` → `<div>` |
| `>` | `&gt;` | Idem |
| `&` | `&amp;` | « R&D » → `R&amp;D` |
| `"` | `&quot;` | Dans un attribut délimité par `"` |
| espace insécable | `&nbsp;` | Empêche un retour à la ligne : `10&nbsp;kVA` |
| `€` | `&euro;` | (ou taper € directement en UTF-8) |
| `©` | `&copy;` | © |

**Règle :** avec `<meta charset="utf-8">`, tu peux taper directement `é`, `€`, `→`. Les entités restent indispensables pour `<`, `>`, `&`.

---

## 11. Titres et hiérarchie

```html
<h1>Rapport d'intervention — Onduleur Q4</h1>
<h2>Contexte</h2>
<h3>Équipement concerné</h3>
<h3>Symptômes observés</h3>
<h2>Actions réalisées</h2>
<h2>Recommandations</h2>
```

**Règles strictes (SEO + accessibilité) :**

1. **Un seul `<h1>`** par page : le sujet principal.
2. **Ne jamais sauter de niveau** : pas de `<h4>` directement sous un `<h2>`.
3. Les titres structurent le **plan** du document — les lecteurs d'écran permettent de naviguer de titre en titre (`H` dans NVDA).
4. Ne choisis **jamais** un niveau de titre pour sa taille visuelle : la taille, c'est CSS (`h2 { font-size: … }`).

**Mauvais réflexe classique :**

```html
<!-- ❌ Titre choisi pour faire "joli petit" -->
<h4>Mon sous-titre</h4>

<!-- ✅ Structure logique, taille gérée en CSS -->
<h2>Mon sous-titre</h2>
```

> 🎯 Test : lis tes titres seuls, dans l'ordre. Si ça forme un sommaire cohérent, ta hiérarchie est bonne.

---

## 12. Paragraphes et texte inline

```html
<p>Un paragraphe regroupe une idée. Les retours à la ligne
dans le code source ne créent <strong>pas</strong> de retour
à la ligne à l'écran : il faut un nouveau <code>&lt;p&gt;</code>.</p>
```

### Balises inline essentielles

| Balise | Sens | Rendu par défaut |
|--------|------|------------------|
| `<strong>` | Importance forte | **gras** |
| `<em>` | Emphase | *italique* |
| `<mark>` | Surlignage / pertinent | <mark>surligné</mark> |
| `<code>` | Morceau de code | `monospace` |
| `<kbd>` | Touche clavier | <kbd>Ctrl</kbd> |
| `<samp>` | Sortie d'un programme | monospace |
| `<var>` | Variable | *italique* |
| `<small>` | Texte secondaire | plus petit |
| `<del>` / `<ins>` | Supprimé / inséré | <del>barré</del> / <ins>souligné</ins> |
| `<sub>` / `<sup>` | Indice / exposant | H<sub>2</sub>O, m<sup>2</sup> |
| `<abbr>` | Abréviation | `title` = signification |
| `<time>` | Date/heure machine | — |

**Exemples :**

```html
<p>Appuyez sur <kbd>Ctrl</kbd> + <kbd>S</kbd> pour enregistrer.</p>
<p>La tension est de <var>U</var> = 230&nbsp;V.</p>
<p>Le <abbr title="Uninterruptible Power Supply">UPS</abbr> est en défaut.</p>
<p>Intervention le <time datetime="2026-09-26">26 septembre 2026</time>.</p>
<p>Ancien prix : <del>1 200 €</del>, nouveau : <ins>990 €</ins>.</p>
```

> 💡 Préfère `<strong>` à `<b>` et `<em>` à `<i>` : les premiers portent du **sens** (les lecteurs d'écran insistent dessus), les seconds ne sont que visuels.

---

## 13. Listes : ordonnées, non ordonnées, définitions

### Liste non ordonnée (`<ul>`)

```html
<ul>
  <li>Vérifier le niveau d'électrolyte</li>
  <li>Contrôler la tension de floating</li>
  <li>Nettoyer les filtres</li>
</ul>
```

### Liste ordonnée (`<ol>`)

```html
<ol>
  <li>Consigner l'armoire</li>
  <li>Mesurer la tension du bus DC</li>
  <li>Remplacer le module</li>
</ol>
```

Attributs utiles de `<ol>` : `start="5"` (commence à 5), `reversed` (décroissant), `type="A"` (`A, B, C…`), `type="i"` (chiffres romains).

### Liste de définitions (`<dl>`)

```html
<dl>
  <dt>UPS</dt>
  <dd>Uninterruptible Power Supply — onduleur.</dd>
  <dt>VFI</dt>
  <dd>Voltage and Frequency Independent — double conversion.</dd>
</dl>
```

### Listes imbriquées

```html
<ul>
  <li>Onduleurs
    <ul>
      <li>Easy UPS 3S</li>
      <li>Easy UPS 3M</li>
    </ul>
  </li>
  <li>Groupes électrogènes</li>
</ul>
```

> ⚠️ Un `<li>` doit **toujours** être enfant direct de `<ul>`, `<ol>` ou `<menu>`. Jamais de `<li>` orphelin, jamais de texte brut directement dans `<ul>`.

---

## 14. Liens hypertextes

```html
<!-- Lien externe -->
<a href="https://www.apc.com">Site APC</a>

<!-- Lien interne -->
<a href="docs/procedure.html">Procédure de consignation</a>

<!-- Ancre vers une section de la même page -->
<a href="#maintenance">Aller à la maintenance</a>
…
<h2 id="maintenance">Maintenance</h2>

<!-- Email / téléphone -->
<a href="mailto:support@entreprise.fr">Nous écrire</a>
<a href="tel:+33123456789">01 23 45 67 89</a>

<!-- Téléchargement forcé -->
<a href="docs/rapport.pdf" download> Télécharger le rapport (PDF)</a>
```

### Attributs importants

| Attribut | Rôle |
|----------|------|
| `href` | Destination (URL, ancre, `mailto:`, `tel:`) |
| `target="_blank"` | Ouvre dans un nouvel onglet |
| `rel="noopener"` | **Obligatoire** avec `target="_blank"` (sécurité : empêche la page cible de piloter la tienne) |
| `download` | Propose le téléchargement au lieu de l'affichage |
| `title` | Info-bulle (à utiliser avec parcimonie) |

### Bonnes pratiques (accessibilité + SEO)

- [ ] **Jamais** « cliquez ici » : l'intitulé doit décrire la destination → `Télécharger la procédure de consignation (PDF, 2 Mo)` ✅
- [ ] `target="_blank"` + `rel="noopener"` systématiques pour les liens externes.
- [ ] Les liens doivent être **visibles** : soulignés ou nettement distincts du texte (pas que par la couleur — §47).

```html
<!-- ❌ -->
<p>Pour la doc, <a href="doc.pdf">cliquez ici</a>.</p>
<!-- ✅ -->
<p>Consultez la <a href="doc.pdf">documentation de l'onduleur (PDF)</a>.</p>
```

---

## 15. Images : `<img>`

```html
<img src="img/onduleur-3s.jpg" alt="Onduleur Easy UPS 3S 30 kVA en baie" width="800" height="600">
```

| Attribut | Rôle |
|----------|------|
| `src` | Chemin de l'image (**obligatoire**) |
| `alt` | **Texte alternatif** : lu par les lecteurs d'écran, affiché si l'image ne charge pas (**obligatoire**) |
| `width` / `height` | Dimensions intrinsèques → **évite les sauts de mise en page** (CLS) |
| `loading="lazy"` | Chargement différé hors écran (perf, §76) |
| `decoding="async"` | Décodage non bloquant |

### Écrire un bon `alt` (arbre de décision, §42 pour le détail)

- Image **informative** → décris la fonction : `alt="Schéma unifilaire du TGBT"`.
- Image **décorative** → `alt=""` (vide : les lecteurs d'écran l'ignorent).
- Image **lien/bouton** → décris l'action : `alt="Télécharger le rapport"`.
- **Jamais** « image de… » (c'est implicite), jamais de pavé de 3 phrases.

### Formats : lequel choisir ?

| Format | Usage | Particularité |
|--------|-------|---------------|
| **SVG** | Logos, icônes, schémas | Vectoriel, léger, net à toutes tailles |
| **WebP** | Photos, images générales | ~30 % plus léger que JPEG |
| **JPEG** | Photos (si WebP indisponible) | Avec pertes, pas de transparence |
| **PNG** | Captures, transparence | Lourd pour les photos |
| **AVIF** | Photos nouvelle génération | Excellent taux de compression, support croissant |
| **GIF** | Petites animations simples | Limité à 256 couleurs |

> 🎯 Règle : SVG pour tout ce qui est vectoriel, WebP/AVIF pour les photos, `width`/`height` + `loading="lazy"` systématiques.

---

## 16. Chemins de fichiers : relatifs vs absolus

C'est la source n°1 des « images qui ne s'affichent pas ». Comprends une fois, c'est réglé à vie.

```
site/
├── index.html
├── docs/
│   └── procedure.html
├── css/
│   └── style.css
└── img/
    └── logo.svg
```

| Depuis… | Pour atteindre `img/logo.svg` | Type |
|---------|-------------------------------|------|
| `index.html` | `img/logo.svg` | Relatif |
| `docs/procedure.html` | `../img/logo.svg` | Relatif (`..` = dossier parent) |
| N'importe où | `/img/logo.svg` | Absolu **depuis la racine du site** |
| N'importe où | `https://intranet.entreprise.fr/img/logo.svg` | Absolu complet |

**Règles :**

- `./` = dossier courant (optionnel : `img/x.png` ≡ `./img/x.png`).
- `../` = remonte d'un niveau. `../../` = deux niveaux.
- Le chemin absolu `/img/logo.svg` part de la **racine du site** — pratique, mais casse tout si le site est servi depuis un sous-dossier.
- **En CSS**, les chemins dans `url()` sont relatifs **au fichier CSS**, pas au HTML !

```css
/* css/style.css → pour viser img/fond.png : */
.hero { background-image: url("../img/fond.png"); }
```

> ⚠️ Erreur classique : `C:\Users\…\img\logo.png` ou `file:///…` dans `src`. Ça marche sur ton PC, nulle part ailleurs. Toujours des chemins relatifs au projet.

---

## 17. Figure, picture, srcset : images avancées

### `<figure>` + `<figcaption>` : image légendée

```html
<figure>
  <img src="img/courbe-charge.png" alt="Courbe de charge de l'onduleur sur 24 h">
  <figcaption>Fig. 3 — Courbe de charge mesurée le 26/09/2026.</figcaption>
</figure>
```

### `<picture>` : art direction (images différentes selon l'écran)

```html
<picture>
  <source media="(max-width: 600px)" srcset="img/schema-mobile.svg">
  <source media="(max-width: 1200px)" srcset="img/schema-tablette.svg">
  <img src="img/schema-desktop.svg" alt="Schéma unifilaire du local technique">
</picture>
```

### `srcset` : même image, plusieurs résolutions

```html
<img src="img/photo-800.jpg"
     srcset="img/photo-400.jpg 400w, img/photo-800.jpg 800w, img/photo-1600.jpg 1600w"
     sizes="(max-width: 600px) 400px, (max-width: 1200px) 800px, 1600px"
     alt="Baie de brassage du local technique">
```

Le navigateur choisit la meilleure résolution selon l'écran et la densité de pixels. **Indispensable** pour des pages rapides sur mobile.

---

## 18. Tableaux : les bases

```html
<table>
  <caption>Parc onduleurs — site principal</caption>
  <thead>
    <tr>
      <th scope="col">Équipement</th>
      <th scope="col">Puissance</th>
      <th scope="col">Local</th>
      <th scope="col">Prochaine maintenance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">UPS-01</th>
      <td>30 kVA</td>
      <td>Local TGBT</td>
      <td>15/10/2026</td>
    </tr>
    <tr>
      <th scope="row">UPS-02</th>
      <td>60 kVA</td>
      <td>Datacenter</td>
      <td>02/11/2026</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td colspan="4">2 équipements suivis — mise à jour : 26/09/2026</td>
    </tr>
  </tfoot>
</table>
```

| Élément | Rôle |
|---------|------|
| `<caption>` | Titre du tableau (lu en premier par les lecteurs d'écran) |
| `<thead>` / `<tbody>` / `<tfoot>` | En-tête / corps / pied — structure + permet le défilement du corps en CSS |
| `<th>` | Cellule d'en-tête (**gras + centrée** par défaut) |
| `scope="col"` / `scope="row"` | Indique si l'en-tête décrit une colonne ou une ligne (**accessibilité**) |
| `colspan` / `rowspan` | Fusionne des cellules (colonnes / lignes) |

> ⚠️ **Jamais** de tableau pour la mise en page (c'était les années 2000). Tableau = données tabulaires uniquement.

---

## 19. Tableaux accessibles avancés

### En-têtes complexes : `id` + `headers`

Quand un tableau a des en-têtes sur deux niveaux, `scope` ne suffit plus : on lie chaque cellule à ses en-têtes.

```html
<table>
  <caption>Mesures électriques par départ</caption>
  <thead>
    <tr>
      <th id="depart" scope="col">Départ</th>
      <th id="u" scope="col">Tension (V)</th>
      <th id="i" scope="col">Courant (A)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="d1" scope="row">TGBT-1</th>
      <td headers="d1 u">231</td>
      <td headers="d1 i">142</td>
    </tr>
  </tbody>
</table>
```

### Checklist tableau accessible

- [ ] `<caption>` présent (ou `aria-label` sur le `<table>`).
- [ ] En-têtes en `<th>` avec `scope` (ou `id`/`headers` si complexe).
- [ ] Pas de cellules vides utilisées pour l'espacement.
- [ ] `colspan`/`rowspan` vérifiés (une fusion cassée décale tout).
- [ ] Sur mobile : tableau scrollable horizontalement (`overflow-x: auto` sur un conteneur) plutôt que compressé à l'illisible.

```css
/* Conteneur scrollable pour tableaux sur mobile */
.table-scroll { overflow-x: auto; }
.table-scroll table { min-width: 640px; }
```

---

## 20. Formulaires : structure de base

```html
<form action="/api/tickets" method="post">
  <div>
    <label for="nom">Nom du demandeur</label>
    <input type="text" id="nom" name="nom" required>
  </div>
  <div>
    <label for="email">E-mail</label>
    <input type="email" id="email" name="email" required>
  </div>
  <button type="submit">Envoyer</button>
</form>
```

| Attribut de `<form>` | Rôle |
|----------------------|------|
| `action` | URL qui reçoit les données |
| `method="get"` | Données dans l'URL (recherche, filtres — **jamais** de données sensibles) |
| `method="post"` | Données dans le corps de la requête (formulaires, création) |
| `enctype="multipart/form-data"` | **Obligatoire** pour l'upload de fichiers (§28) |
| `novalidate` | Désactive la validation native (si tu valides en JS) |
| `autocomplete="on"` | Active la complétion du navigateur |

**Règle d'or (accessibilité) :** chaque champ a un `<label>` avec `for` = `id` du champ. Cliquer le label donne le focus au champ — et les lecteurs d'écran annoncent l'intitulé.

```html
<!-- ✅ -->
<label for="tel">Téléphone</label>
<input type="tel" id="tel" name="tel">

<!-- ❌ (placeholder seul = inaccessible, il disparaît à la saisie) -->
<input type="tel" placeholder="Téléphone">
```

> 💡 Le `placeholder` est un **indice**, pas un label. Utilise les deux : label visible + placeholder d'exemple.

---

## 21. Tous les types d'input — partie 1 : texte et choix

```html
<!-- Texte simple -->
<label for="t1">Référence équipement</label>
<input type="text" id="t1" name="ref" maxlength="20" placeholder="Ex : UPS-01">

<!-- Mot de passe (masqué) -->
<label for="mdp">Mot de passe</label>
<input type="password" id="mdp" name="mdp" autocomplete="current-password">

<!-- E-mail (validation native du format) -->
<label for="mail">E-mail</label>
<input type="email" id="mail" name="mail" required>

<!-- Téléphone (clavier adapté sur mobile) -->
<label for="tel">Téléphone</label>
<input type="tel" id="tel" name="tel" pattern="[0-9]{10}">

<!-- URL -->
<label for="site">Site du fournisseur</label>
<input type="url" id="site" name="site" placeholder="https://…">

<!-- Recherche -->
<label for="q">Rechercher</label>
<input type="search" id="q" name="q">

<!-- Zone de texte multiligne -->
<label for="desc">Description du problème</label>
<textarea id="desc" name="desc" rows="5" cols="40"></textarea>
```

| Type | Clavier mobile / comportement |
|------|-------------------------------|
| `text` | Standard |
| `password` | Masqué, propose le gestionnaire de mots de passe |
| `email` | Clavier avec `@` ; validation du format |
| `tel` | Pavé numérique |
| `url` | Clavier avec `/` et `.com` |
| `search` | Champ avec croix d'effacement |

### Cases à cocher et boutons radio

```html
<!-- Cases à cocher : choix multiples -->
<fieldset>
  <legend>Équipements concernés</legend>
  <label><input type="checkbox" name="eq" value="ups" checked> Onduleur</label>
  <label><input type="checkbox" name="eq" value="ge"> Groupe électrogène</label>
  <label><input type="checkbox" name="eq" value="tgbt"> TGBT</label>
</fieldset>

<!-- Radios : choix unique (même name = même groupe) -->
<fieldset>
  <legend>Criticité</legend>
  <label><input type="radio" name="crit" value="basse"> Basse</label>
  <label><input type="radio" name="crit" value="haute" checked> Haute</label>
  <label><input type="radio" name="crit" value="critique"> Critique</label>
</fieldset>
```

> 💡 Astuce : enrober l'`<input>` dans le `<label>` évite d'avoir à gérer `for`/`id` — la liaison est implicite.

---

## 22. Tous les types d'input — partie 2 : nombres, dates, divers

```html
<!-- Nombre avec bornes et pas -->
<label for="kva">Puissance (kVA)</label>
<input type="number" id="kva" name="kva" min="10" max="500" step="10" value="60">

<!-- Curseur -->
<label for="charge">Charge estimée : <output id="o-charge">50</output> %</label>
<input type="range" id="charge" name="charge" min="0" max="100" value="50"
       oninput="document.getElementById('o-charge').value = this.value">

<!-- Date / heure -->
<label for="d">Date d'intervention</label>
<input type="date" id="d" name="date" min="2026-01-01" max="2026-12-31">
<label for="h">Heure</label>
<input type="time" id="h" name="heure">
<label for="dt">Date et heure</label>
<input type="datetime-local" id="dt" name="rdv">
<label for="m">Mois</label>
<input type="month" id="m" name="mois">
<label for="s">Semaine</label>
<input type="week" id="s" name="semaine">

<!-- Couleur -->
<label for="c">Couleur du statut</label>
<input type="color" id="c" name="couleur" value="#16a34a">

<!-- Fichier -->
<label for="pj">Photo du défaut</label>
<input type="file" id="pj" name="pj" accept="image/*" multiple>

<!-- Caché (données techniques, pas pour l'utilisateur) -->
<input type="hidden" name="site_id" value="12">
```

| Type | Rendu |
|------|-------|
| `number` | Champ + flèches d'incrément |
| `range` | Curseur |
| `date`, `time`, `datetime-local`, `month`, `week` | Sélecteurs natifs (calendrier…) |
| `color` | Sélecteur de couleur natif |
| `file` | Bouton « Parcourir… » |
| `hidden` | Invisible, envoyé avec le formulaire |

> ⚠️ `type="number"` : pour un **téléphone** ou un **code postal**, préfère `type="text"` + `inputmode="numeric"` + `pattern` — `number` ajoute des flèches parasites et gère mal les zéros initiaux.

```html
<label for="cp">Code postal</label>
<input type="text" id="cp" name="cp" inputmode="numeric" pattern="[0-9]{5}">
```

---

## 23. Boutons et soumission

```html
<!-- Soumet le formulaire -->
<button type="submit">Créer le ticket</button>

<!-- Réinitialise le formulaire -->
<button type="reset">Effacer</button>

<!-- Bouton générique (piloté en JS) -->
<button type="button" id="btn-calcul">Calculer l'autonomie</button>

<!-- Version image (évite : préfère <button> + CSS) -->
<input type="image" src="img/go.png" alt="Envoyer">

<!-- Désactivé -->
<button type="submit" disabled>Envoyer (brouillon incomplet)</button>
```

| `type` | Effet |
|--------|-------|
| `submit` | Envoie le formulaire (défaut si `type` omis **dans** un formulaire — piège classique !) |
| `reset` | Réinitialise les champs |
| `button` | Ne fait rien seul — pour JavaScript |

> ⚠️ **Piège n°1** : un `<button>` sans `type` dans un formulaire = `type="submit"` implicite. Ton bouton « Annuler » enverra le formulaire si tu oublies `type="button"`.

```html
<!-- ❌ Ce bouton "Fermer" soumet le formulaire ! -->
<button>Fermer</button>
<!-- ✅ -->
<button type="button">Fermer</button>
```

---

## 24. Select, datalist, textarea, output

### Liste déroulante

```html
<label for="famille">Famille d'équipement</label>
<select id="famille" name="famille" required>
  <option value="">— Choisir —</option>
  <optgroup label="Énergie">
    <option value="ups">Onduleur</option>
    <option value="ge" selected>Groupe électrogène</option>
  </optgroup>
  <optgroup label="Climatisation">
    <option value="clim">Climatiseur</option>
  </optgroup>
</select>
```

- `multiple` + `size="4"` : liste à choix multiples.
- `<optgroup>` : regroupe les options sous un libellé.

### Datalist : suggestions + saisie libre

```html
<label for="marque">Marque</label>
<input list="marques" id="marque" name="marque">
<datalist id="marques">
  <option value="Schneider Electric"></option>
  <option value="Eaton"></option>
  <option value="Vertiv"></option>
  <option value="Riello"></option>
</datalist>
```

### Output : afficher un résultat calculé

```html
<form oninput="total.value = (kva.valueAsNumber || 0) * (nb.valueAsNumber || 0)">
  <label>Puissance unitaire (kVA) <input type="number" name="kva" value="30"></label>
  <label>Nombre <input type="number" name="nb" value="2"></label>
  <p>Total : <output name="total" for="kva nb">60</output> kVA</p>
</form>
```

---

## 25. Fieldset, legend : regrouper les champs

```html
<form action="/api/tickets" method="post">
  <fieldset>
    <legend>Demandeur</legend>
    <label for="nom">Nom</label>
    <input type="text" id="nom" name="nom" required>
    <label for="svc">Service</label>
    <input type="text" id="svc" name="service">
  </fieldset>

  <fieldset>
    <legend>Équipement</legend>
    <label for="eq">Référence</label>
    <input type="text" id="eq" name="equipement" required>
  </fieldset>

  <fieldset disabled>
    <legend>Réservé au support</legend>
    <label for="tech">Technicien assigné</label>
    <input type="text" id="tech" name="tech">
  </fieldset>
</form>
```

- `<legend>` = titre du groupe, annoncé par les lecteurs d'écran avant chaque champ du groupe.
- `disabled` sur le `<fieldset>` désactive **tous** ses champs d'un coup.
- En CSS, on peut retirer la bordure par défaut : `fieldset { border: 0; padding: 0; }` (en gardant `legend` visible !).

> 🎯 Un long formulaire sans fieldset = un mur de champs. Groupe par thème : demandeur / équipement / description / pièces jointes.

---

## 26. Validation native HTML5

Le navigateur valide **gratuitement** avant l'envoi — sans JavaScript.

```html
<form action="/api/tickets" method="post">
  <label for="ref">Référence (obligatoire, 3–10 caractères)</label>
  <input type="text" id="ref" name="ref" required minlength="3" maxlength="10">

  <label for="mail">E-mail pro</label>
  <input type="email" id="mail" name="mail" required>

  <label for="tel">Téléphone (10 chiffres)</label>
  <input type="tel" id="tel" name="tel" pattern="[0-9]{10}"
         title="10 chiffres sans espaces, ex : 0612345678">

  <label for="date">Date (dans le futur)</label>
  <input type="date" id="date" name="date" min="2026-09-27" required>

  <button type="submit">Envoyer</button>
</form>
```

| Attribut | Effet |
|----------|-------|
| `required` | Champ obligatoire |
| `minlength` / `maxlength` | Longueur du texte |
| `min` / `max` | Valeur min/max (number, date…) |
| `pattern` | Expression régulière (§27) |
| `title` | Message d'aide affiché avec l'erreur de `pattern` |
| `type="email"` / `type="url"` | Validation du format |

**Pseudo-classes CSS pour le feedback visuel :**

```css
input:required { border-left: 3px solid #f59e0b; }  /* obligatoire */
input:valid { border-color: #16a34a; }               /* valide */
input:invalid { border-color: #dc2626; }             /* invalide */
```

> ⚠️ La validation HTML5 est un **confort**, pas une sécurité : toujours re-valider côté serveur. `novalidate` sur le `<form>` la désactive (utile si tu valides en JS).

---

## 27. Validation : pattern et messages personnalisés

### `pattern` : expressions régulières utiles

```html
<!-- Référence type "UPS-01" : 3 lettres, tiret, 2 chiffres -->
<input type="text" pattern="[A-Z]{3}-[0-9]{2}" title="Format : ABC-12">

<!-- Téléphone français -->
<input type="tel" pattern="(0|\+33)[1-9][0-9]{8}" title="Ex : 0612345678">

<!-- Code postal -->
<input type="text" pattern="[0-9]{5}" title="5 chiffres">

<!-- Mot de passe : 8+ caractères, 1 majuscule, 1 chiffre -->
<input type="password" pattern="(?=.*[A-Z])(?=.*[0-9]).{8,}"
       title="8 caractères min, dont 1 majuscule et 1 chiffre">
```

| Motif regex | Sens |
|-------------|------|
| `[0-9]{5}` | Exactement 5 chiffres |
| `[A-Z]{3}` | Exactement 3 majuscules |
| `.{8,}` | 8 caractères ou plus |
| `(?=.*[A-Z])` | Contient au moins une majuscule (lookahead) |
| `(0\|\+33)` | `0` ou `+33` |

### Messages d'erreur personnalisés (JS minimal)

```html
<form id="f-ticket">
  <label for="ref2">Référence</label>
  <input type="text" id="ref2" name="ref" required pattern="[A-Z]{3}-[0-9]{2}">
  <button type="submit">Envoyer</button>
</form>
<script>
  const ref = document.getElementById('ref2');
  ref.addEventListener('invalid', () => {
    ref.setCustomValidity('Format attendu : 3 lettres, un tiret, 2 chiffres (ex : UPS-01).');
  });
  ref.addEventListener('input', () => ref.setCustomValidity(''));
</script>
```

> 💡 `setCustomValidity('')` à chaque saisie : sinon le message d'erreur reste bloqué même après correction.

---

## 28. Upload de fichiers

```html
<form action="/api/upload" method="post" enctype="multipart/form-data">
  <label for="photo">Photo du défaut (JPG/PNG, max 5 Mo)</label>
  <input type="file" id="photo" name="photo" accept="image/jpeg,image/png" required>

  <label for="docs">Documents (plusieurs fichiers)</label>
  <input type="file" id="docs" name="docs" multiple accept=".pdf,.docx">

  <button type="submit">Envoyer</button>
</form>
```

| Point | Détail |
|-------|--------|
| `enctype="multipart/form-data"` | **Obligatoire** sur le `<form>`, sinon le fichier n'est pas transmis |
| `method="post"` | Obligatoire aussi (GET ne transporte pas de fichiers) |
| `accept` | Filtre la boîte de dialogue (`image/*`, `.pdf`, `audio/*`…) — indicatif, à contrôler côté serveur |
| `multiple` | Autorise plusieurs fichiers |
| `capture="environment"` | Sur mobile : ouvre directement l'appareil photo (dos) |

```html
<!-- Prise de photo directe sur mobile -->
<input type="file" accept="image/*" capture="environment">
```

> ⚠️ `accept` n'est qu'un **filtre d'interface** : un utilisateur malveillant peut envoyer n'importe quoi. Contrôle le type MIME réel, la taille et le contenu côté serveur. Toujours.

---

## 29. Vidéo : `<video>`

```html
<video controls width="640" height="360" preload="metadata"
       poster="img/apercu-maintenance.jpg">
  <source src="video/maintenance-ups.webm" type="video/webm">
  <source src="video/maintenance-ups.mp4" type="video/mp4">
  <track kind="subtitles" src="video/sous-titres-fr.vtt" srclang="fr" label="Français" default>
  <p>Votre navigateur ne supporte pas la vidéo.
     <a href="video/maintenance-ups.mp4">Télécharger la vidéo (MP4)</a></p>
</video>
```

| Attribut | Rôle |
|----------|------|
| `controls` | Affiche les contrôles natifs (lecture, volume, plein écran) |
| `poster` | Image d'aperçu avant lecture |
| `preload="metadata"` | Ne charge que les métadonnées (durée…) — **perf** |
| `autoplay` | Démarre seul — **à éviter** sauf `muted` (les navigateurs bloquent l'autoplay sonore) |
| `loop` / `muted` | Boucle / muet |
| `<track>` | Sous-titres WebVTT — **accessibilité** |

**Formats :** fournis **WebM + MP4** (H.264) pour couvrir tous les navigateurs. Le navigateur lit le premier `<source>` qu'il supporte.

> 🎯 Vidéo de fond décorative ? `autoplay muted loop playsinline` + **jamais** de son + alternative statique si `prefers-reduced-motion` (§49).

---

## 30. Audio : `<audio>`

```html
<audio controls preload="none">
  <source src="audio/alarme-ups.ogg" type="audio/ogg">
  <source src="audio/alarme-ups.mp3" type="audio/mpeg">
  <p><a href="audio/alarme-ups.mp3">Télécharger l'enregistrement (MP3)</a></p>
</audio>
```

| Attribut | Rôle |
|----------|------|
| `controls` | Barre de lecture native |
| `preload="none"` | Ne charge rien avant le clic (économe) |
| `preload="metadata"` / `"auto"` | Métadonnées / tout |

**Accessibilité audio :**

- Fournis une **transcription textuelle** sous le lecteur (obligatoire pour un contenu informatif).
- Ne déclenche **jamais** de son automatique : c'est désorientant pour les lecteurs d'écran (et interdit par les navigateurs sans interaction préalable).

```html
<figure>
  <audio controls src="audio/bruit-ventilateur.mp3"></audio>
  <figcaption>Enregistrement du ventilateur défectueux (12 s).
    <a href="docs/transcription-bruit.txt">Transcription</a></figcaption>
</figure>
```

---

## 31. Iframe : embarquer du contenu

```html
<!-- Carte OpenStreetMap embarquée -->
<iframe src="https://www.openstreetmap.org/export/embed.html?bbox=…"
        width="600" height="400" title="Carte du site principal"
        loading="lazy"></iframe>

<!-- Vidéo YouTube (version respectueuse : youtube-nocookie) -->
<iframe width="560" height="315"
        src="https://www.youtube-nocookie.com/embed/VIDEO_ID"
        title="Tutoriel : remplacement d'un module UPS"
        allow="accelerometer; autoplay; encrypted-media; picture-in-picture"
        allowfullscreen loading="lazy"></iframe>
```

| Attribut | Rôle |
|----------|------|
| `title` | **Obligatoire** : décrit le contenu embarqué aux lecteurs d'écran |
| `loading="lazy"` | Charge quand visible (perf) |
| `sandbox` | Restreint ce que l'iframe peut faire (scripts, formulaires…) — sécurité |
| `allowfullscreen` | Autorise le plein écran |

```html
<!-- Iframe durcie : aucun script, aucun formulaire -->
<iframe src="https://exemple.com/widget" sandbox title="Widget météo" loading="lazy"></iframe>
```

> ⚠️ Chaque iframe = un **document complet** à charger (lourd). N'en abuse pas sur un dashboard. Et `sandbox` sans `allow-scripts` bloque le JS embarqué — parfait pour du contenu tiers non fiable.

---

## 32. Balises sémantiques : structurer la page

```html
<body>
  <header>
    <img src="img/logo.svg" alt="Logo Entreprise">
    <nav aria-label="Navigation principale">
      <ul>
        <li><a href="/">Accueil</a></li>
        <li><a href="/tickets">Tickets</a></li>
        <li><a href="/docs">Documentation</a></li>
      </ul>
    </ul>
    </nav>
  </header>

  <main>
    <article>
      <h1>…</h1>
      …
    </article>
    <aside>
      <h2>Liens utiles</h2>
      …
    </aside>
  </main>

  <footer>
    <p>© 2026 — Service Systèmes & Énergies</p>
  </footer>
</body>
```

| Balise | Sens |
|--------|------|
| `<header>` | En-tête (de page ou de section) |
| `<nav>` | Bloc de navigation |
| `<main>` | Contenu principal — **un seul** par page |
| `<article>` | Contenu autonome (article, ticket, fiche) |
| `<section>` | Regroupement thématique (avec un titre !) |
| `<aside>` | Contenu complémentaire (liens, encadrés) |
| `<footer>` | Pied (de page ou de section) |

**Pourquoi c'est crucial :** les lecteurs d'écran listent ces « landmarks » pour naviguer (`D` dans NVDA). Un `<div>` ne dit rien ; un `<nav>` dit « navigation ».

---

## 33. Balises sémantiques : le contenu fin

```html
<article>
  <header>
    <h1>Remplacement des batteries UPS-02</h1>
    <p>Publié le <time datetime="2026-09-20">20 septembre 2026</time>
       par <address><a href="mailto:tech@entreprise.fr">Karim T.</a></address></p>
  </header>

  <section>
    <h2>Diagnostic</h2>
    <p>…</p>
    <blockquote>
      <p>« La tension de floating est tombée à 12,1 V par bloc. »</p>
      <cite>— Rapport de mesure n°42</cite>
    </blockquote>
  </section>

  <section>
    <h2>Procédure</h2>
    <pre><code># Vérification avant intervention
ups-test --bank A --full</code></pre>
  </section>

  <footer>
    <p>Tags : <a href="/tags/ups">onduleur</a>, <a href="/tags/batterie">batterie</a></p>
  </footer>
</article>
```

| Balise | Usage |
|--------|-------|
| `<address>` | Coordonnées de l'auteur (pas une adresse postale quelconque !) |
| `<blockquote>` + `<cite>` | Citation longue + source |
| `<q>` | Citation courte inline |
| `<pre>` | Texte préformaté (conserve espaces/retours) — idéal pour le code |
| `<hr>` | Séparation thématique (pas juste un « trait décoratif ») |
| `<details>` + `<summary>` | Accordéon natif, sans JS ! |

```html
<details>
  <summary>Voir la procédure de consignation</summary>
  <ol>
    <li>Couper le départ…</li>
    <li>Vérifier l'absence de tension…</li>
  </ol>
</details>
```

---

## 34. Balises obsolètes : à bannir

Ces balises existent encore (compatibilité) mais **ne doivent plus être utilisées**. Le validateur W3C les signale.

| ❌ Obsolète | ✅ Remplacement |
|-------------|-----------------|
| `<font>`, `<basefont>` | CSS `font-family`, `color` |
| `<center>` | CSS `text-align: center` / flexbox |
| `<b>` (présentation pure) | `<strong>` (sens) ou CSS `font-weight` |
| `<i>` (présentation pure) | `<em>` (sens) ou CSS `font-style` |
| `<u>` | CSS `text-decoration` (et évite : ressemble à un lien) |
| `<strike>` | `<del>` ou CSS `text-decoration: line-through` |
| `<table>` de mise en page | Flexbox / Grid (§59–62) |
| `<frame>`, `<frameset>` | `<iframe>` (ou rien) |
| `<marquee>`, `<blink>` | CSS animations (et pitié, ne les imite pas) |
| `bgcolor`, `align`, `border` (attributs) | CSS |

**Test express :** si tu dois mettre en forme, c'est CSS. Si la balise ne décrit que l'apparence, elle est suspecte.

---

## 35. Head : les meta essentielles

```html
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Tickets — Service Systèmes & Énergies</title>
  <meta name="description" content="Suivi des tickets d'intervention : onduleurs, groupes, TGBT.">
  <meta name="robots" content="noindex, nofollow">  <!-- page interne : pas d'indexation -->
  <meta name="theme-color" content="#0f172a">       <!-- couleur barre mobile -->
  <meta name="author" content="Service Systèmes & Énergies">
  <link rel="stylesheet" href="/css/style.css">
  <link rel="icon" href="/img/favicon.ico" sizes="any">
  <link rel="icon" href="/img/favicon.svg" type="image/svg+xml">
  <link rel="apple-touch-icon" href="/img/apple-touch-icon.png">
</head>
```

| Meta | Rôle |
|------|------|
| `charset="utf-8"` | **Première** meta du head — encodage |
| `viewport` | **Indispensable** au responsive (§65) |
| `description` | Résumé affiché dans Google (~155 caractères) |
| `robots` | `noindex` = page interne non indexée |
| `theme-color` | Teinte de l'interface mobile |

**Ordre conseillé :** charset → viewport → title → description → CSS → favicons → scripts `defer`.

---

## 36. Favicon : le détail qui fait pro

Le favicon = la petite icône d'onglet. Un site interne sans favicon fait « chantier ».

```html
<link rel="icon" href="/img/favicon.ico" sizes="any"><!-- fallback universel -->
<link rel="icon" href="/img/favicon.svg" type="image/svg+xml"><!-- moderne -->
<link rel="apple-touch-icon" href="/img/apple-touch-icon.png"><!-- iOS -->
```

**Recette minimale :**

1. Crée un SVG simple (logo ou initiale) → `favicon.svg`.
2. Exporte un PNG 180×180 → `apple-touch-icon.png`.
3. (Optionnel) Génère le `.ico` multi-tailles pour vieux navigateurs.

**Favicon SVG inline élégant :**

```html
<link rel="icon" href="data:image/svg+xml,
  <svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'>
    <rect width='100' height='100' rx='20' fill='%230f172a'/>
    <text x='50' y='68' font-size='60' text-anchor='middle' fill='white'
          font-family='sans-serif'>⚡</text>
  </svg>">
```

> 💡 Teste l'affichage en mode sombre : un favicon foncé sur onglet sombre = invisible. Prévois un fond contrasté ou `prefers-color-scheme` dans le SVG.

---

## 37. SEO on-page : les fondamentaux

Le SEO (Search Engine Optimization) commence dans le HTML — même pour un intranet, ces réflexes structurent bien les pages.

**Checklist SEO on-page :**

- [ ] **Un `<title>` unique** par page, ~50–60 caractères, mot-clé en premier : `Tickets onduleurs — Service Systèmes & Énergies` ✅ / `Accueil` ❌
- [ ] **Une `meta description` unique** par page, ~150–160 caractères, incitative.
- [ ] **Un seul `<h1>`**, hiérarchie sans saut (§11).
- [ ] URLs **lisibles** : `/docs/maintenance-ups` ✅ / `/page.php?id=42` ❌
- [ ] Images avec `alt` descriptif + nom de fichier explicite (`schema-tgbt.svg` ✅).
- [ ] **Temps de chargement** < 3 s (poids images, §76).
- [ ] Page **responsive** (Google indexe en mobile-first).
- [ ] Pas de contenu important uniquement en image (le texte d'une image n'est pas lu… sauf via `alt`).

```html
<title>Maintenance des onduleurs — Guide du service énergies</title>
<meta name="description" content="Procédures de maintenance préventive des onduleurs :
  checklists, périodicités, valeurs de référence. Service Systèmes & Énergies.">
```

> 🎯 Pour des pages **internes**, l'essentiel SEO = `noindex` (§35) + structure propre. Pour de la doc publique, applique toute la checklist.

---

## 38. Open Graph et Twitter Cards : beaux partages

Quand on colle ton URL dans Teams, Slack ou LinkedIn, ces metas génèrent la **carte d'aperçu** (titre + image + description).

```html
<!-- Open Graph (Facebook, LinkedIn, Teams, Slack…) -->
<meta property="og:title" content="Guide de maintenance des onduleurs">
<meta property="og:description" content="Checklists, périodicités et valeurs de référence.">
<meta property="og:type" content="article">
<meta property="og:url" content="https://docs.entreprise.fr/guides/maintenance-ups">
<meta property="og:image" content="https://docs.entreprise.fr/img/og-ups.jpg">
<meta property="og:image:width" content="1200">
<meta property="og:image:height" content="630">
<meta property="og:locale" content="fr_FR">

<!-- Twitter Cards -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="Guide de maintenance des onduleurs">
<meta name="twitter:description" content="Checklists, périodicités et valeurs de référence.">
<meta name="twitter:image" content="https://docs.entreprise.fr/img/og-ups.jpg">
```

**Image OG idéale :** 1200×630 px, < 300 Ko, texte lisible en miniature, URLs **absolues** (https://…).

> 💡 Teste le rendu : validateur de cartes Twitter, LinkedIn Post Inspector, ou simplement colle l'URL dans Teams.

---

## 39. Données structurées : JSON-LD

Le **JSON-LD** décrit le contenu aux moteurs dans un vocabulaire standard (schema.org) — placé dans un `<script type="application/ld+json">`.

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "TechArticle",
  "headline": "Maintenance préventive des onduleurs",
  "description": "Checklists et périodicités de maintenance des UPS.",
  "inLanguage": "fr",
  "author": {
    "@type": "Organization",
    "name": "Service Systèmes & Énergies"
  },
  "datePublished": "2026-09-26"
}
</script>
```

**Autres types utiles :**

| Type schema.org | Usage |
|-----------------|-------|
| `TechArticle` / `Article` | Documentation, guides |
| `FAQPage` | Pages de questions fréquentes (affiche en résultats enrichis) |
| `HowTo` | Procédures pas à pas |
| `BreadcrumbList` | Fil d'Ariane |
| `Organization` | Infos de l'entreprise |

**Exemple FAQ (résultats enrichis Google) :**

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [{
    "@type": "Question",
    "name": "À quelle fréquence tester les batteries d'un onduleur ?",
    "acceptedAnswer": {
      "@type": "Answer",
      "text": "Test d'impédance annuel et contrôle visuel trimestriel."
    }
  }]
}
</script>
```

> 🎯 Valide ton JSON-LD : Google Rich Results Test. Une virgule oubliée = tout le bloc ignoré.

---

## 40. Sitemap, robots.txt, canonical, hreflang

### robots.txt (à la racine du site)

```text
User-agent: *
Disallow: /admin/
Disallow: /api/
Allow: /

Sitemap: https://docs.entreprise.fr/sitemap.xml
```

### Balise canonical : éviter le contenu dupliqué

```html
<!-- Cette page existe aussi via ?tri=date : on désigne l'URL de référence -->
<link rel="canonical" href="https://docs.entreprise.fr/guides/maintenance-ups">
```

### hreflang : versions multilingues

```html
<link rel="alternate" hreflang="fr" href="https://docs.entreprise.fr/fr/guide">
<link rel="alternate" hreflang="en" href="https://docs.entreprise.fr/en/guide">
<link rel="alternate" hreflang="x-default" href="https://docs.entreprise.fr/fr/guide">
```

### Sitemap XML (extrait)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://docs.entreprise.fr/guides/maintenance-ups</loc>
    <lastmod>2026-09-26</lastmod>
    <changefreq>monthly</changefreq>
  </url>
</urlset>
```

| Fichier/élément | Rôle |
|-----------------|------|
| `robots.txt` | Dit aux robots quoi explorer |
| `sitemap.xml` | Liste les pages à indexer |
| `canonical` | URL de référence en cas de doublons |
| `hreflang` | Associe les versions linguistiques |

---

## 41. Accessibilité : pourquoi et principes

**L'accessibilité web** = rendre tes pages utilisables par **tous** : aveugles (lecteurs d'écran), malvoyants, daltoniens, personnes à mobilité réduite (clavier seul), etc. En France, le **RGAA** (référentiel public) impose des obligations aux services publics — et c'est une bonne pratique partout.

### Les 4 principes (WCAG)

| Principe | Signification | Exemple |
|----------|---------------|---------|
| **Perceptible** | L'info doit être percevable | `alt` sur images, contrastes suffisants |
| **Utilisable** | Navigable au clavier | Focus visible, pas de piège clavier |
| **Compréhensible** | Lisible et prévisible | Labels explicites, erreurs claires |
| **Robuste** | Interprétable par les techs d'assistance | HTML valide, ARIA correct |

### Ce que ça change concrètement pour toi

- Tes dashboards seront utilisables par un collègue malvoyant (zoom 200 % sans casse).
- Tes formulaires de tickets seront remplissables **sans souris**.
- Bonus : l'accessibilité améliore le **SEO** et la **robustesse** générale.

> 💡 Réflexe : 80 % de l'accessibilité = **HTML sémantique correct** (titres, labels, alt, landmarks). ARIA ne vient qu'en complément (§44–45).

---

## 42. Texte alternatif : l'arbre de décision

Le `alt` est souvent mal écrit. Voici la méthode :

```
L'image est-elle décorative (bordure, illustration d'ambiance) ?
├── OUI → alt=""
└── NON → L'image porte-t-elle de l'information ?
    ├── NON (redondante avec le texte adjacent) → alt=""
    └── OUI → Décris la FONCTION, pas l'apparence
```

**Exemples concrets :**

```html
<!-- ❌ -->
<img src="graph.png" alt="image d'un graphique">
<!-- ✅ : ce que le graphique APPREND -->
<img src="graph.png" alt="Charge de l'onduleur : pic à 78 % à 14 h, stable sinon">

<!-- Image décorative -->
<img src="img/motif-fond.svg" alt="">

<!-- Image-lien : décris l'ACTION -->
<a href="rapport.pdf">
  <img src="img/pdf-icon.svg" alt="Télécharger le rapport d'intervention (PDF)">
</a>

<!-- Schéma complexe : alt court + description longue -->
<img src="img/unifilaire.svg" alt="Schéma unifilaire du TGBT"
     aria-describedby="desc-unifilaire">
<p id="desc-unifilaire">Le schéma montre : arrivée EDF 400 V… [description détaillée]</p>
```

> ⚠️ `alt` trop long (> 125 caractères) : coupe en `alt` court + `aria-describedby`. Les vieux lecteurs d'écran tronquent.

---

## 43. Landmarks et skip links

### Landmarks : les repères de navigation

Les balises sémantiques (§32) créent des **landmarks** que les lecteurs d'écran listent :

```html
<header>…</header>          <!-- banner -->
<nav aria-label="Principale">…</nav>  <!-- navigation -->
<main>…</main>              <!-- main -->
<aside aria-label="Compléments">…</aside>  <!-- complementary -->
<footer>…</footer>          <!-- contentinfo -->
```

- Si **deux** `<nav>`, différencie-les avec `aria-label` (« Principale » / « Fil d'Ariane »).
- `<main>` : **un seul** par page, jamais imbriqué dans header/footer.

### Skip link : aller au contenu

Le premier élément interactif de la page : un lien « Aller au contenu » **visible au focus clavier**.

```html
<body>
  <a class="skip-link" href="#contenu">Aller au contenu principal</a>
  <header>…</header>
  <main id="contenu" tabindex="-1">
    …
  </main>
</body>
```

```css
.skip-link {
  position: absolute; left: -9999px; top: 0;
  background: #0f172a; color: #fff; padding: .75rem 1rem; z-index: 100;
}
.skip-link:focus { left: 0; }  /* apparaît quand on tabule dessus */
```

> 🎯 Teste au clavier : `Tab` depuis le haut de la page → le skip link apparaît → `Entrée` → tu arrives au contenu sans traverser tout le menu.

---

## 44. ARIA : rôles

**ARIA** (Accessible Rich Internet Applications) ajoute des informations aux technologies d'assistance. **Règle n°1 : pas d'ARIA si le HTML natif suffit.**

```html
<!-- ❌ ARIA inutile -->
<div role="button" tabindex="0">Envoyer</div>
<!-- ✅ HTML natif (clavier + lecteur d'écran gratuits) -->
<button type="submit">Envoyer</button>
```

### Quand ARIA est légitime

| Situation | Solution ARIA |
|-----------|---------------|
| Menu hamburger custom | `aria-expanded`, `aria-controls` |
| Onglets custom | `role="tablist"`, `role="tab"`, `aria-selected` |
| Message d'erreur dynamique | `role="alert"` |
| Barre de progression custom | `role="progressbar"` + `aria-valuenow` |
| Zone qui se met à jour (dashboard) | `aria-live="polite"` |

```html
<!-- Bouton qui déplie un panneau -->
<button aria-expanded="false" aria-controls="panneau-filtres" id="btn-filtres">
  Filtres
</button>
<div id="panneau-filtres" hidden>
  …
</div>
```

```html
<!-- Zone live : le lecteur d'écran annonce les mises à jour -->
<div aria-live="polite" id="statut-ups">
  UPS-01 : en ligne — charge 42 %
</div>
```

> ⚠️ `aria-live="assertive"` interrompt l'utilisateur : réserve-le aux **alertes critiques** (défaut onduleur !), pas aux mises à jour de routine.

---

## 45. ARIA : états et propriétés essentiels

| Attribut | Rôle | Exemple |
|----------|------|---------|
| `aria-label` | Nom accessible quand il n'y a pas de texte visible | `<button aria-label="Fermer">×</button>` |
| `aria-labelledby` | Référence l'élément qui sert de label | `aria-labelledby="titre-dialog"` |
| `aria-describedby` | Description complémentaire | Champ + aide |
| `aria-hidden="true"` | Cache aux lecteurs d'écran (icône décorative) | `<svg aria-hidden="true">…` |
| `aria-expanded` | État déplié/replié | Accordéon, menu |
| `aria-current="page"` | Page active dans la navigation | |
| `aria-invalid="true"` | Champ en erreur | + message lié |
| `aria-disabled="true"` | Désactivé mais focusable (vs `disabled`) | |

```html
<!-- Navigation : page courante -->
<nav aria-label="Principale">
  <ul>
    <li><a href="/" aria-current="page">Accueil</a></li>
    <li><a href="/tickets">Tickets</a></li>
  </ul>
</nav>

<!-- Champ en erreur lié à son message -->
<label for="mail2">E-mail</label>
<input type="email" id="mail2" aria-invalid="true" aria-describedby="err-mail2">
<p id="err-mail2" role="alert">Format d'e-mail invalide.</p>

<!-- Icône décorative dans un bouton : cachée aux lecteurs d'écran -->
<button type="submit">
  <svg aria-hidden="true" focusable="false">…</svg>
  Envoyer
</button>
```

> ⚠️ `aria-hidden="true"` sur un élément **focusable** = catastrophe (focus invisible). Ne le mets que sur du non-interactif.

---

## 46. Navigation clavier et focus

**Test de base :** débranche ta souris. Tu dois pouvoir tout faire au `Tab` / `Maj+Tab` / `Entrée` / `Espace` / flèches.

### Ordre de tabulation

- Suit l'**ordre du DOM** : structure ton HTML dans l'ordre logique (pas de positionnement CSS qui contredit l'ordre visuel).
- `tabindex="0"` : rend focusable (dans l'ordre naturel).
- `tabindex="-1"` : focusable **en JS** uniquement (utile pour déplacer le focus vers un message d'erreur).
- `tabindex > 0` : **interdit** — casse l'ordre naturel.

### Focus visible (obligatoire)

```css
/* Style de focus personnalisé ET visible */
:focus-visible {
  outline: 3px solid #2563eb;
  outline-offset: 2px;
}

/* ❌ JAMAIS ça sans remplacement */
/* :focus { outline: none; } */
```

### Piège clavier

Si tu ouvres une **modale**, le focus doit rester dedans tant qu'elle est ouverte (focus trap) et revenir au bouton d'origine à la fermeture. Sans JS robuste, préfère `<dialog>` natif :

```html
<dialog id="dlg">
  <h2>Confirmer l'arrêt</h2>
  <p>Couper l'onduleur UPS-02 ?</p>
  <form method="dialog">
    <button value="non">Annuler</button>
    <button value="oui">Confirmer</button>
  </form>
</dialog>
<button onclick="document.getElementById('dlg').showModal()">Arrêter l'UPS</button>
```

> 🎯 `<dialog>` gère nativement : focus trap, `Échap` pour fermer, fond assombri (`::backdrop`). Zéro JS à écrire.

---

## 47. Contrastes et couleurs

### Ratios WCAG

| Niveau | Texte normal | Grand texte (≥ 24 px ou ≥ 18,66 px gras) |
|--------|--------------|------------------------------------------|
| **AA** (minimum) | 4,5:1 | 3:1 |
| **AAA** (renforcé) | 7:1 | 4,5:1 |

**Couples qui passent / qui cassent (fond blanc) :**

| Couleur | Ratio | Verdict |
|---------|-------|---------|
| `#000000` (noir) | 21:1 | ✅ AAA |
| `#374151` (gris foncé) | 10,9:1 | ✅ AAA |
| `#6b7280` (gris moyen) | 4,8:1 | ✅ AA |
| `#9ca3af` (gris clair) | 2,8:1 | ❌ même pas AA |
| `#dc2626` (rouge) | 4,8:1 | ✅ AA |
| `#16a34a` (vert) | 3,7:1 | ❌ pour texte normal |

### Règles

- [ ] Vérifie avec un outil (WebAIM Contrast Checker, extension axe).
- [ ] **Ne jamais coder l'info par la seule couleur** : un statut « défaut » rouge doit aussi avoir un texte/icône/forme (« ⚠ Défaut »).
- [ ] Les **placeholder** ont souvent un contraste insuffisant : ne pas y mettre d'info critique.
- [ ] Teste en **niveaux de gris** : si l'info disparaît, elle dépendait trop de la couleur.

```html
<!-- ❌ Couleur seule -->
<span style="color: red">●</span>
<!-- ✅ Couleur + texte + forme -->
<span class="statut statut-defaut"><span aria-hidden="true">●</span> Défaut</span>
```

---

## 48. Formulaires accessibles : la checklist

- [ ] Chaque champ a un `<label>` visible (pas de placeholder seul).
- [ ] Champs obligatoires **indiqués dans le label** : `Nom *` + mention `* Champs obligatoires` en haut.
- [ ] `fieldset` + `legend` pour les groupes (§25).
- [ ] Messages d'erreur : liés au champ (`aria-describedby` + `role="alert"`), **explicites** (« La date doit être après aujourd'hui » pas « Erreur »).
- [ ] Le focus va au **premier champ en erreur** après soumission.
- [ ] `autocomplete` renseigné (nom, email, tel…) : aide tout le monde, crucial sur mobile.

```html
<form action="/api/tickets" method="post">
  <p>* Champs obligatoires</p>

  <div>
    <label for="nom3">Nom *</label>
    <input type="text" id="nom3" name="nom" required autocomplete="name">
  </div>

  <div>
    <label for="mail3">E-mail professionnel *</label>
    <input type="email" id="mail3" name="email" required autocomplete="email"
           aria-describedby="aide-mail3">
    <p id="aide-mail3">Nous enverrons la confirmation à cette adresse.</p>
  </div>

  <button type="submit">Créer le ticket</button>
</form>
```

**Valeurs `autocomplete` courantes :** `name`, `given-name`, `family-name`, `email`, `tel`, `organization`, `street-address`, `postal-code`, `country`, `current-password`, `new-password`.

---

## 49. Préférences utilisateur : reduced motion, dark mode

### `prefers-reduced-motion` : respecter les utilisateurs sensibles

```css
/* Désactive animations/transitions pour ceux qui l'ont demandé */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

À mettre **systématiquement** dans tes CSS dès que tu animes quelque chose (§72).

### `prefers-color-scheme` : dark mode natif (§73 pour le détail)

```css
:root { --fond: #ffffff; --texte: #111827; }
@media (prefers-color-scheme: dark) {
  :root { --fond: #111827; --texte: #f9fafb; }
}
body { background: var(--fond); color: var(--texte); }
```

> 💡 Ces media queries lisent les **réglages OS** de l'utilisateur : tu n'as rien à demander, juste à respecter.

---

## 50. CSS : introduction et liaison

**CSS** (Cascading Style Sheets) décrit la présentation. Une règle = **sélecteur** + **déclarations**.

```css
/* sélecteur */ h1 {
  color: #0f172a;        /* déclaration : propriété + valeur */
  font-size: 2rem;
  margin-bottom: 1rem;
}
```

### 3 façons d'appliquer du CSS

```html
<!-- 1. Feuille externe (✅ À PRIVILÉGIER : cache, réutilisable) -->
<link rel="stylesheet" href="css/style.css">

<!-- 2. <style> dans le head (⚠️ page unique, prototypes) -->
<style>
  .alerte { color: #dc2626; }
</style>

<!-- 3. Attribut style (❌ sauf cas dynamiques en JS) -->
<p style="color: red;">Urgent</p>
```

**Ordre de priorité** (la « cascade ») : `style=""` > `<style>`/feuille externe (selon l'ordre) — voir spécificité §53.

### Premier fichier CSS type

```css
/* css/style.css */
:root {
  --bleu: #2563eb;
  --fond: #f8fafc;
}

body {
  font-family: system-ui, sans-serif;
  background: var(--fond);
  color: #1e293b;
  line-height: 1.6;
  margin: 0;
}
```

> 🎯 `system-ui` : utilise la police système (rapide, native, zéro téléchargement). Parfait pour des outils internes.

---

## 51. Sélecteurs CSS : les bases

| Sélecteur | Cible | Exemple |
|-----------|-------|---------|
| `*` | Tout | `* { box-sizing: border-box; }` |
| `p` | Toutes les balises `<p>` | `p { margin: 0 0 1rem; }` |
| `.alerte` | Classe `alerte` | `<p class="alerte">` |
| `#menu` | ID `menu` (unique) | `<nav id="menu">` |
| `[type="email"]` | Attribut | `<input type="email">` |
| `h1, h2` | Groupe | Les deux |

```html
<p class="alerte critique">Défaut onduleur !</p>
<p class="alerte">Avertissement batterie.</p>
```

```css
.alerte { padding: 1rem; border-left: 4px solid #f59e0b; }
.alerte.critique { border-color: #dc2626; background: #fef2f2; } /* les deux classes */
```

**Classes vs ID :**

- **Classe** (`.`) : réutilisable, c'est 99 % de ton CSS.
- **ID** (`#`) : unique par page ; utile pour les ancres et le JS, **évite** en CSS (spécificité trop forte, §53).

> 💡 Convention de nommage : minuscules + tirets (`.carte-equipement`), jamais d'accents ni d'espaces.

---

## 52. Sélecteurs avancés : combinateurs et attributs

### Combinateurs

```css
/* Descendant : tout <a> DANS <nav> (peu importe la profondeur) */
nav a { color: #2563eb; }

/* Enfant direct : <li> enfants directs de <ul class="menu"> */
ul.menu > li { list-style: none; }

/* Frère adjacent : <p> juste APRÈS un <h2> */
h2 + p { margin-top: 0; }

/* Frères suivants : tous les <p> après un <h2> (même parent) */
h2 ~ p { color: #334155; }
```

### Sélecteurs d'attributs

```css
/* Présence */
a[target] { /* liens avec target */ }

/* Valeur exacte */
input[type="email"] { border-color: #2563eb; }

/* Commence par : liens externes http */
a[href^="http"]::after { content: " ↗"; }

/* Finit par : liens PDF */
a[href$=".pdf"]::before { content: "📄 "; }

/* Contient */
img[src*="logo"] { /* images dont le src contient "logo" */ }
```

| Opérateur | Sens |
|-----------|------|
| `[attr]` | L'attribut existe |
| `[attr="v"]` | Égal à `v` |
| `[attr^="v"]` | Commence par `v` |
| `[attr$="v"]` | Finit par `v` |
| `[attr*="v"]` | Contient `v` |

> 🎯 `a[href^="http"]` + `::after "↗"` : signale les liens externes **sans toucher au HTML**. Élégant et maintenable.

---

## 53. Spécificité et cascade : qui gagne ?

Quand plusieurs règles ciblent le même élément, le navigateur calcule un **score** : `(inline, ID, classes, éléments)`.

```css
p { color: black; }              /* (0,0,0,1) */
.alerte { color: orange; }       /* (0,0,1,0) → gagne sur p */
#msg { color: blue; }            /* (0,1,0,0) → gagne sur .alerte */
p.alerte { color: green; }       /* (0,0,1,1) → gagne sur .alerte seul */
```

**Règles de départage :**

1. `!important` gagne sur tout (à éviter — c'est un aveu d'échec).
2. Score le plus élevé.
3. À égalité : la règle **déclarée en dernier** gagne.

```html
<p id="msg" class="alerte">Défaut</p>  <!-- bleu : #msg gagne -->
```

### Bonnes pratiques

- [ ] **Zéro ID** en CSS, **zéro `!important`** → tes scores restent bas et surchargeables.
- [ ] Préfère des classes explicites (`.btn`, `.btn-primaire`) à des sélecteurs longs (`header nav ul li a`).
- [ ] Si une règle « ne s'applique pas », c'est 99 % un problème de **spécificité** : inspecte dans DevTools (les règles barrées = perdantes).

> 💡 DevTools → onglet Elements → panneau Styles : les déclarations **barrées** sont celles battues par la spécificité. Ton meilleur prof de CSS.

---

## 54. Héritage en CSS

Certaines propriétés **s'héritent** (les enfants reprennent la valeur du parent), d'autres non.

| Héritées ✅ | Non héritées ❌ |
|-------------|-----------------|
| `color`, `font-*`, `line-height` | `margin`, `padding`, `border` |
| `text-align`, `visibility` | `background`, `width`, `height` |
| `list-style`, `cursor` | `display`, `position`, `float` |

```css
body {
  font-family: system-ui, sans-serif;  /* hérité par TOUTE la page */
  color: #1e293b;                      /* hérité aussi */
  margin: 0;                           /* NON hérité : à mettre où besoin */
}
```

### Mots-clés de contrôle

```css
.carte {
  color: inherit;   /* force l'héritage depuis le parent */
  margin: initial;  /* revient à la valeur par défaut du navigateur */
  border: unset;    /* inherit si héritable, sinon initial */
}
```

> 🎯 Tire parti de l'héritage : déclare `font-family` et `color` sur `body`, pas sur chaque élément. Moins de code, plus de cohérence.

---

## 55. Unités et couleurs en CSS

### Unités : absolues vs relatives

| Unité | Type | Équivalence / usage |
|-------|------|---------------------|
| `px` | Absolue | Pixel — bordures, détails fins |
| `rem` | Relative | × taille de police racine (`html`, défaut 16 px) — **textes, espacements** |
| `em` | Relative | × taille de police de l'élément parent — espacements proportionnels |
| `%` | Relative | % du parent — largeurs fluides |
| `vw` / `vh` | Relative | % de la largeur/hauteur du viewport — plein écran |
| `ch` | Relative | Largeur du « 0 » — largeurs de texte optimales (`max-width: 65ch`) |

```css
html { font-size: 16px; }   /* 1rem = 16px */
h1 { font-size: 2rem; }      /* = 32px, suit le réglage utilisateur */
.carte { width: 90%; max-width: 60ch; padding: 1.5rem; }
.hero { min-height: 50vh; }  /* moitié de la hauteur d'écran */
```

> 🎯 `rem` pour les tailles de texte : si l'utilisateur a réglé une police plus grande (malvoyance), tout suit. `px` fige — à réserver aux bordures.

### Couleurs : 4 notations

```css
.rouge-nomme  { color: red; }                  /* 148 noms — lisible, limité */
.rouge-hex    { color: #dc2626; }              /* hexadécimal — le standard */
.rouge-rgb    { color: rgb(220 38 38 / 0.8); } /* rgb + alpha (transparence) */
.rouge-hsl    { color: hsl(0 84% 60%); }       /* teinte/saturation/luminosité — intuitif */
```

**HSL, ton ami :** pour une palette, fixe la teinte et joue sur saturation/luminosité.

```css
:root {
  --teinte: 217; /* bleu */
  --primaire: hsl(var(--teinte) 91% 60%);
  --primaire-fonce: hsl(var(--teinte) 91% 45%);
  --primaire-clair: hsl(var(--teinte) 91% 92%);
}
```

---

## 56. Box model : le concept central

Chaque élément = **4 couches** emboîtées (de l'intérieur vers l'extérieur) :

```
┌──────────── margin ────────────┐
│ ┌────────── border ──────────┐ │
│ │ ┌─────── padding ────────┐ │ │
│ │ │      CONTENU           │ │ │
│ │ │  (width × height)      │ │ │
│ │ └────────────────────────┘ │ │
│ └────────────────────────────┘ │
└────────────────────────────────┘
```

```css
.boite {
  width: 300px;            /* contenu */
  padding: 20px;           /* intérieur : fond coloré inclus */
  border: 5px solid #333;   /* bordure */
  margin: 30px;            /* extérieur : transparent, espace avec voisins */
}
```

### `box-sizing` : le réglage qui change tout

```css
/* ✅ À mettre en tête de CHAQUE projet */
*, *::before, *::after { box-sizing: border-box; }
```

| `box-sizing` | `width: 300px` + `padding: 20px` + `border: 5px` = largeur totale |
|--------------|-------------------------------------------------------------------|
| `content-box` (défaut historique) | 300 + 40 + 10 = **350 px** 😱 |
| `border-box` | **300 px** tout compris ✅ |

> 💡 Avec `border-box`, `width` = largeur **totale** visible. Les calculs de mise en page deviennent intuitifs. C'est le défaut de tous les frameworks modernes.

### Fusion des marges (margin collapsing)

Deux marges verticales adjacentes **fusionnent** (la plus grande gagne) :

```css
h2 { margin-bottom: 30px; }
p  { margin-top: 20px; }
/* Espace réel entre les deux : 30px (pas 50px) */
```

Pas un bug : c'est la spec. On le contourne avec flexbox/grid (qui ne fusionnent pas) ou un padding.

---

## 57. Typographie CSS

```css
body {
  font-family: system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
  font-size: 1rem;          /* 16 px par défaut */
  line-height: 1.6;        /* interlignage : 1.5–1.7 = confortable */
  color: #1e293b;
}

h1, h2, h3 {
  line-height: 1.2;        /* titres plus resserrés */
  font-weight: 700;
  text-wrap: balance;      /* équilibre les lignes des titres (moderne) */
}

p { max-width: 65ch; }     /* ~65 caractères/ligne = lisibilité optimale */

.code {
  font-family: ui-monospace, "Cascadia Code", Consolas, monospace;
}
```

| Propriété | Effet |
|-----------|-------|
| `font-weight` | 100–900 ou `bold`/`normal` |
| `font-style` | `italic`, `normal` |
| `text-align` | `left`, `center`, `right`, `justify` |
| `text-transform` | `uppercase`, `lowercase`, `capitalize` |
| `letter-spacing` | Chasse (espacement des lettres) |
| `text-decoration` | `underline`, `line-through`, `none` |
| `white-space: nowrap` | Empêche le retour à la ligne |
| `text-overflow: ellipsis` | `…` quand ça dépasse (avec `overflow: hidden`) |

```css
/* Titre qui tronque proprement */
.titre-carte {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

> ⚠️ `text-align: justify` sur de petites largeurs crée des « rivières » de blancs. Sur mobile, préfère `left`.

---

## 58. Display : block, inline, et les autres

```css
.block       { display: block; }        /* toute la largeur, retour ligne */
.inline      { display: inline; }       /* dans le flux, pas de width/height */
.inline-bloc { display: inline-block; } /* inline + width/height réglables */
.cacher      { display: none; }         /* retiré du flux ET des lecteurs d'écran */
.flex        { display: flex; }         /* §59 */
.grille      { display: grid; }         /* §61 */
```

| Valeur | Particularité |
|--------|---------------|
| `display: none` | L'élément **n'existe plus** (mise en page + accessibilité) |
| `visibility: hidden` | Invisible mais **occupe toujours sa place** (et reste focusable ⚠️) |
| `opacity: 0` | Transparent mais **cliquable/focusable** (piège !) |

**Cacher proprement selon le besoin :**

```css
/* Caché pour tout le monde */
.cache { display: none; }

/* Caché visuellement mais lu par les lecteurs d'écran (labels discrets…) */
.sr-only {
  position: absolute; width: 1px; height: 1px;
  overflow: hidden; clip-path: inset(50%);
}
```

> ⚠️ `visibility: hidden` + `tabindex` = focus invisible au clavier. Pour masquer temporairement un élément interactif, préfère `display: none` ou `hidden`.

---

## 59. Flexbox : le conteneur

**Flexbox** = mise en page **1D** (une ligne OU une colonne). Parfait pour : barres de navigation, cartes, formulaires, centrages.

```css
.conteneur {
  display: flex;
  flex-direction: row;      /* row | row-reverse | column | column-reverse */
  justify-content: center;  /* alignement sur l'AXE PRINCIPAL */
  align-items: center;      /* alignement sur l'AXE SECONDAIRE */
  gap: 1rem;                /* espace entre enfants (moderne, remplace les margins) */
  flex-wrap: wrap;          /* retour à la ligne si ça déborde */
}
```

### `justify-content` (axe principal)

| Valeur | Effet |
|--------|-------|
| `flex-start` | Au début (défaut) |
| `center` | Centré |
| `flex-end` | À la fin |
| `space-between` | Répartis, collés aux bords |
| `space-around` | Répartis, marges égales autour |
| `space-evenly` | Espaces parfaitement égaux |

### `align-items` (axe secondaire)

`stretch` (défaut : étire) | `flex-start` | `center` | `flex-end` | `baseline`

**Le centrage parfait (fini les calculs) :**

```css
.centrer {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}
```

> 💡 `gap` marche aussi en grid. Oublie les `margin-right` sur tous-les-enfants-sauf-le-dernier : `gap` fait ça proprement.

---

## 60. Flexbox : les items et cas concrets

```css
.item {
  flex-grow: 1;    /* grandit pour remplir l'espace (proportion) */
  flex-shrink: 1;  /* rétrécit si manque de place */
  flex-basis: 200px; /* taille de départ */
  /* raccourci : */
  flex: 1;         /* = flex: 1 1 0% → se partagent l'espace équitablement */
  align-self: flex-start; /* aligne CET item seul (surcharge align-items) */
  order: 2;        /* change l'ordre VISUEL (pas l'ordre DOM/lecteur d'écran ⚠️) */
}
```

### Cas concret 1 : barre de navigation

```html
<nav class="navbar">
  <a class="logo" href="/">⚡ Énergies</a>
  <ul class="liens">
    <li><a href="/tickets">Tickets</a></li>
    <li><a href="/docs">Docs</a></li>
  </ul>
  <button type="button">+ Nouveau ticket</button>
</nav>
```

```css
.navbar { display: flex; align-items: center; gap: 1rem; padding: 1rem; }
.navbar .liens { display: flex; gap: 1rem; list-style: none; margin: 0; padding: 0; }
.navbar button { margin-left: auto; } /* pousse le bouton à droite */
```

### Cas concret 2 : carte équipement

```css
.carte { display: flex; gap: 1rem; align-items: flex-start; }
.carte img { flex-shrink: 0; }        /* l'image ne s'écrase pas */
.carte .infos { flex: 1; }            /* le texte prend le reste */
```

> ⚠️ `order` ne change que l'affichage : un lecteur d'écran et le `Tab` suivent toujours l'ordre DOM. Ne t'en sers pas pour réordonner du contenu porteur de sens.

---

## 61. CSS Grid : les bases

**Grid** = mise en page **2D** (lignes ET colonnes). Parfait pour : dashboards, galeries, pages complètes.

```css
.grille {
  display: grid;
  grid-template-columns: 200px 1fr 1fr;  /* 3 colonnes : fixe + 2 flexibles */
  grid-template-rows: auto 1fr auto;     /* 3 lignes */
  gap: 1rem;
}
```

### Unités grid

| Notation | Sens |
|----------|------|
| `1fr` | 1 fraction de l'espace **restant** |
| `repeat(3, 1fr)` | 3 colonnes égales |
| `repeat(auto-fit, minmax(250px, 1fr))` | **Responsive auto** : autant de colonnes de 250 px min que possible |
| `minmax(200px, 1fr)` | Entre 200 px et l'espace dispo |

```css
/* Galerie responsive sans media query ! */
.galerie {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1rem;
}
```

### Placer les items

```css
.item-important {
  grid-column: 1 / 3;   /* de la ligne 1 à la ligne 3 → occupe 2 colonnes */
  grid-row: 1 / 2;
  /* ou */
  grid-column: span 2;  /* occupe 2 colonnes */
}
```

> 🎯 `repeat(auto-fit, minmax(250px, 1fr))` : LA recette d'une grille responsive sans breakpoint. Retiens-la.

---

## 62. Grid : areas et layout de page

### `grid-template-areas` : dessine ta page en ASCII

```css
.page {
  display: grid;
  grid-template-columns: 220px 1fr;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  min-height: 100vh;
}
.page > header  { grid-area: header; }
.page > nav     { grid-area: sidebar; }
.page > main    { grid-area: main; }
.page > footer  { grid-area: footer; }
```

```html
<div class="page">
  <header>…</header>
  <nav>…</nav>
  <main>…</main>
  <footer>…</footer>
</div>
```

### Version mobile : on redessine

```css
@media (max-width: 768px) {
  .page {
    grid-template-columns: 1fr;
    grid-template-areas:
      "header"
      "main"
      "sidebar"
      "footer";
  }
}
```

### Flexbox ou Grid ? La règle

| Besoin | Choix |
|--------|-------|
| Aligner des éléments sur **un axe** (nav, ligne de boutons) | **Flexbox** |
| Disposer en **lignes ET colonnes** (dashboard, page) | **Grid** |
| Ils se **combinent** : grid pour la page, flexbox dans les cartes | ✅ |

> 💡 Un dashboard typique : `grid` pour la coquille (header/sidebar/main), `flexbox` à l'intérieur des cartes de KPI.

---

## 63. Positionnement CSS

```css
.boite { position: static; }    /* défaut : dans le flux normal */
```

| Valeur | Comportement |
|--------|--------------|
| `relative` | Décalé par rapport à **sa position normale** (`top/left/…`) ; reste dans le flux |
| `absolute` | Sorti du flux, positionné par rapport au **parent positionné** le plus proche |
| `fixed` | Sorti du flux, positionné par rapport au **viewport** (reste visible au scroll) |
| `sticky` | Hybride : normal, puis « collé » quand on scrolle jusqu'à lui |

```css
/* Badge sur une carte */
.carte { position: relative; }
.badge {
  position: absolute;
  top: 0.5rem; right: 0.5rem;
}

/* En-tête qui reste visible au scroll */
.entete { position: sticky; top: 0; background: #fff; z-index: 10; }

/* Bouton d'aide flottant */
.aide { position: fixed; bottom: 1rem; right: 1rem; }
```

> ⚠️ `absolute` se réfère au parent **positionné** le plus proche (pas `static`). Oublier `position: relative` sur le parent = l'élément part se caler sur la page entière. Classique !

---

## 64. Z-index et contextes d'empilement

`z-index` contrôle **qui passe devant qui** — mais seulement entre éléments **positionnés** (ou flex/grid items).

```css
.modale { position: fixed; z-index: 100; }
.menu-deroulant { position: absolute; z-index: 50; }
```

**Échelle conseillée** (évite les `z-index: 9999`) :

| Couche | z-index |
|--------|---------|
| Contenu normal | auto / 0 |
| Header sticky | 10 |
| Menu déroulant | 50 |
| Modale + overlay | 100 |
| Tooltip | 200 |
| Skip link | 300 |

### Le piège : les contextes d'empilement

Un `z-index` ne s'applique qu'**à l'intérieur** du contexte de son parent. Un enfant avec `z-index: 9999` dans un parent à `z-index: 1` **restera sous** un voisin à `z-index: 2`.

Créent un contexte : `position` + `z-index`, `opacity < 1`, `transform`, `filter`, `isolation: isolate`.

```css
/* Isole proprement un composant */
.carte { isolation: isolate; }
```

> 💡 Si ton menu passe **sous** un élément malgré un gros z-index, c'est un problème de contexte, pas de valeur. Remonte au parent.

---

## 65. Responsive : viewport et media queries

### La meta viewport (rappel §35) — sans elle, pas de responsive

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

Sans elle, le mobile affiche la page « zoomée arrière » (viewport virtuel ~980 px).

### Media queries : adapter selon l'écran

```css
/* Base : mobile (mobile-first, §66) */
.carte { padding: 1rem; }

/* À partir de 768 px : tablette */
@media (min-width: 768px) {
  .carte { padding: 2rem; }
}

/* À partir de 1024 px : desktop */
@media (min-width: 1024px) {
  .grille { grid-template-columns: repeat(3, 1fr); }
}
```

| Syntaxe | Sens |
|---------|------|
| `(min-width: 768px)` | À partir de 768 px (mobile-first ✅) |
| `(max-width: 767px)` | Jusqu'à 767 px (desktop-first) |
| `(orientation: landscape)` | Paysage |
| `(prefers-color-scheme: dark)` | Dark mode OS |
| `(prefers-reduced-motion: reduce)` | Animations réduites |

**Opérateurs :**

```css
@media (min-width: 768px) and (max-width: 1023px) { /* tablette uniquement */ }
@media (min-width: 768px), (orientation: landscape) { /* OU logique */ }
@media not (prefers-reduced-motion: reduce) { /* sauf… */ }
```

---

## 66. Mobile-first et breakpoints

### Mobile-first : la méthode

1. Écris le CSS pour **mobile** d'abord (1 colonne, navigation simplifiée).
2. Ajoute des `min-width` pour **enrichir** sur grand écran.

```css
/* 1. Mobile : une colonne */
.dashboard { display: grid; gap: 1rem; }

/* 2. Tablette : deux colonnes */
@media (min-width: 768px) {
  .dashboard { grid-template-columns: repeat(2, 1fr); }
}

/* 3. Desktop : sidebar + contenu */
@media (min-width: 1024px) {
  .dashboard { grid-template-columns: 240px repeat(3, 1fr); }
}
```

**Pourquoi mobile-first ?**

- Le CSS mobile est le plus simple → base saine.
- `min-width` s'empile naturellement (pas de conflits d'annulation).
- Performances : le mobile charge le CSS de base, pas les surcharges desktop.

### Breakpoints : pas de valeurs magiques

| Usage courant | Breakpoint |
|---------------|------------|
| Mobile | < 768 px (base) |
| Tablette | ≥ 768 px |
| Desktop | ≥ 1024 px |
| Grand écran | ≥ 1440 px |

> 🎯 Mieux : des breakpoints **au contenu** (« quand ma carte devient trop étroite ») plutôt qu'aux appareils. Et `auto-fit`/`minmax` (§61) évitent souvent toute media query.

---

## 67. Images et médias responsives (CSS)

```css
/* L'image ne dépasse JAMAIS de son conteneur */
img, video, svg {
  max-width: 100%;
  height: auto;   /* garde les proportions (avec width/height HTML, §15) */
}

/* Image de fond qui couvre son cadre */
.hero {
  background-image: url("../img/local.jpg");
  background-size: cover;       /* couvre tout, rogne si besoin */
  background-position: center; /* centre le point d'intérêt */
  min-height: 40vh;
}
/* alternatives background-size : contain (tout visible), auto */
```

### `<picture>` vs CSS : qui fait quoi ?

| Besoin | Solution |
|--------|----------|
| Même image, taille adaptée | `srcset`/`sizes` (§17) + `max-width: 100%` |
| Image **différente** selon l'écran | `<picture>` (§17) |
| Image décorative de fond | CSS `background-image` + media queries |

```css
/* Fond différent sur mobile (plus léger) */
.hero { background-image: url("../img/hero-mobile.jpg"); }
@media (min-width: 768px) {
  .hero { background-image: url("../img/hero-desktop.jpg"); }
}
```

> ⚠️ `background-image` n'a **pas de `alt`** : réservé au décoratif. Toute image informative = `<img>` avec `alt`.

---

## 68. Variables CSS (custom properties)

```css
:root {
  --couleur-primaire: #2563eb;
  --couleur-danger: #dc2626;
  --fond-carte: #ffffff;
  --rayon: 0.5rem;
  --espacement: 1rem;
}

.carte {
  background: var(--fond-carte);
  border-radius: var(--rayon);
  padding: var(--espacement);
  border-left: 4px solid var(--couleur-primaire);
}

.carte.danger {
  --couleur-primaire: var(--couleur-danger); /* surcharge LOCALE */
}
```

### Pourquoi c'est puissant

- **Thèmes** : change les variables, toute la page suit (dark mode §73).
- **Valeur de repli** : `var(--inconnue, #333)` → `#333` si non définie.
- **Cascade** : redéfinissable par composant (exemple `.danger` ci-dessus).
- **Pilotable en JS** : `element.style.setProperty('--couleur-primaire', '#16a34a')`.

```css
/* Thème par attribut data- */
:root, [data-theme="clair"] { --fond: #fff; --texte: #111; }
[data-theme="sombre"]       { --fond: #111; --texte: #f5f5f5; }
body { background: var(--fond); color: var(--texte); }
```

```html
<button onclick="document.documentElement.dataset.theme='sombre'">🌙</button>
```

> 💡 Convention : déclare toutes tes variables dans `:root`, documente-les en commentaire. C'est ton « design system » miniature.

---

## 69. Pseudo-classes : les états

```css
/* Liens : L-O-V-E H-A-T-E (ordre mnémotechnique) */
a:link    { color: #2563eb; }   /* non visité */
a:visited { color: #6d28d9; }   /* visité */
a:hover   { text-decoration: underline; }  /* survolé */
a:active  { color: #dc2626; }   /* pendant le clic */
```

### Les plus utiles au quotidien

| Pseudo-classe | Cible |
|---------------|-------|
| `:hover` | Survol souris |
| `:focus` / `:focus-visible` | Focus clavier (§46) |
| `:first-child` / `:last-child` | Premier/dernier enfant |
| `:nth-child(2n)` | 1 ligne sur 2 (tableaux zébrés) |
| `:not(.actif)` | Tout sauf… |
| `:checked` | Checkbox/radio cochée |
| `:disabled` / `:enabled` | Champs (in)actifs |
| `:required` / `:optional` | Champs (non) obligatoires |
| `:valid` / `:invalid` | Validation (§26) |
| `:empty` | Élément sans contenu |

```css
/* Tableau zébré */
tbody tr:nth-child(even) { background: #f8fafc; }
tbody tr:hover { background: #e0f2fe; }

/* Bouton désactivé */
button:disabled { opacity: .5; cursor: not-allowed; }

/* Case cochée : label mis en avant */
input:checked + label { font-weight: bold; }
```

> ⚠️ `:hover` seul = inutilisable au tactile et au clavier. Tout effet `:hover` doit avoir un équivalent `:focus-visible`.

---

## 70. Pseudo-éléments : ::before / ::after

Ils créent des **boîtes virtuelles** sans toucher au HTML — parfait pour la décoration.

```css
/* Icône externe après les liens sortants (§52) */
a[href^="http"]::after {
  content: " ↗";
  font-size: .8em;
}

/* Puce custom devant chaque item */
.liste-puces li::before {
  content: "⚡";
  margin-right: .5rem;
}

/* Grand guillemet décoratif */
blockquote::before {
  content: "“";
  font-size: 3rem;
  color: #cbd5e1;
}
```

| Règle | Détail |
|-------|--------|
| `content` | **Obligatoire** (même `content: ""`) sinon rien ne s'affiche |
| `::before` / `::after` | Syntaxe moderne à 2 deux-points |
| Accessibilité | Le `content` **n'est pas lu** par (la plupart des) lecteurs d'écran → n'y mets que du décoratif |

**Autres pseudo-éléments utiles :**

```css
p::first-line { font-weight: bold; }       /* première ligne */
p::first-letter { font-size: 2em; }        /* lettrine */
::selection { background: #bfdbfe; }      /* texte sélectionné */
input::placeholder { color: #94a3b8; }     /* placeholder */
dialog::backdrop { background: rgb(0 0 0 / .5); } /* fond de <dialog> */
```

---

## 71. Transitions : animer en douceur

```css
.btn {
  background: #2563eb;
  color: #fff;
  padding: .75rem 1.5rem;
  border-radius: .5rem;
  /* ce qui s'anime, durée, courbe, délai */
  transition: background-color .2s ease, transform .2s ease;
}

.btn:hover {
  background: #1d4ed8;
  transform: translateY(-2px);  /* soulève légèrement */
}

.btn:active {
  transform: translateY(0);      /* renfonce au clic */
}
```

| Propriété | Rôle |
|-----------|------|
| `transition-property` | Quoi animer (`background-color`, `transform`, `all`…) |
| `transition-duration` | Durée (`.2s`) |
| `transition-timing-function` | Courbe : `ease`, `linear`, `ease-in-out`, `cubic-bezier(…)` |
| `transition-delay` | Délai avant démarrage |

**Bonnes pratiques :**

- Anime `transform` et `opacity` (performants, GPU) — **évite** `width`, `height`, `top` (recalculent toute la mise en page).
- Durées : 150–300 ms pour les micro-interactions.
- Toujours le bloc `@media (prefers-reduced-motion: reduce)` (§49).

> 💡 `transition: all` = facile mais coûteux (le navigateur surveille tout). Préfère lister les propriétés.

---

## 72. Animations `@keyframes`

Pour les animations **complexes ou en boucle** (spinner de chargement, pulse d'alerte).

```css
/* Spinner de chargement */
.spinner {
  width: 40px; height: 40px;
  border: 4px solid #e2e8f0;
  border-top-color: #2563eb;
  border-radius: 50%;
  animation: rotation 1s linear infinite;
}

@keyframes rotation {
  to { transform: rotate(360deg); }
}

/* Pulse d'alerte */
.alerte-critique {
  animation: pulse 2s ease-in-out infinite;
}
@keyframes pulse {
  0%, 100% { opacity: 1; }
  50%      { opacity: .4; }
}
```

| Propriété | Rôle |
|-----------|------|
| `animation-name` | Nom du `@keyframes` |
| `animation-duration` | Durée d'un cycle |
| `animation-iteration-count` | `infinite` ou nombre |
| `animation-direction` | `alternate` (aller-retour) |
| `animation-fill-mode` | `forwards` : garde l'état final |

```html
<div class="spinner" role="status">
  <span class="sr-only">Chargement des tickets…</span>
</div>
```

> ⚠️ Une animation en boucle + `prefers-reduced-motion` (§49) = **obligatoire**. Et un spinner sans texte accessible = invisible aux lecteurs d'écran : ajoute `role="status"` + texte sr-only.

---

## 73. Dark mode propre avec les variables

```css
/* 1. Palette en variables */
:root {
  --fond: #ffffff;
  --fond-carte: #f8fafc;
  --texte: #1e293b;
  --texte-doux: #64748b;
  --bordure: #e2e8f0;
  --primaire: #2563eb;
  color-scheme: light; /* scrollbars & contrôles natifs en clair */
}

/* 2. Version sombre : on ne change QUE les variables */
@media (prefers-color-scheme: dark) {
  :root {
    --fond: #0f172a;
    --fond-carte: #1e293b;
    --texte: #f1f5f9;
    --texte-doux: #94a3b8;
    --bordure: #334155;
    --primaire: #60a5fa;
    color-scheme: dark;
  }
}

/* 3. Les composants utilisent les variables → les deux thèmes gratuits */
body { background: var(--fond); color: var(--texte); }
.carte {
  background: var(--fond-carte);
  border: 1px solid var(--bordure);
  color: var(--texte);
}
```

**Pièges du dark mode :**

- Les **images** ne s'inversent pas : prévois des visuels qui marchent sur fond sombre (ou `filter: brightness(.9)`).
- `box-shadow` quasi invisible sur fond sombre → utilise des **bordures**.
- Teste les **contrastes** dans les deux thèmes (§47) : un gris qui passe sur blanc peut casser sur noir.

> 💡 Bonus : un toggle manuel (`data-theme`, §68) + `localStorage` pour laisser l'utilisateur choisir, avec le media query comme défaut.

---

## 74. Print CSS : des pages qui s'impriment bien

Tes procédures et rapports seront imprimés. Prévois-le :

```css
@media print {
  /* On masque ce qui ne sert pas sur papier */
  header nav, footer, .actions, .no-print { display: none; }

  /* Lisible sur papier */
  body { font-size: 12pt; color: #000; background: #fff; }
  a { color: #000; text-decoration: none; }
  /* Affiche les URL des liens (utile sur papier !) */
  a[href^="http"]::after { content: " (" attr(href) ")"; }

  /* Évite les coupures moches */
  h1, h2, h3 { break-after: avoid; }
  table, figure, pre { break-inside: avoid; }

  /* Chaque grande section sur sa page */
  .page-break { break-before: page; }
}
```

| Propriété | Effet |
|-----------|-------|
| `break-before: page` | Saut de page avant |
| `break-inside: avoid` | Pas de coupure à l'intérieur |
| `break-after: avoid` | Pas de coupure juste après (titre orphelin) |

**Checklist print :**

- [ ] Fond blanc, texte noir (économie d'encre).
- [ ] Navigation / boutons / pubs masqués.
- [ ] Tableaux : répète l'en-tête (`thead { display: table-header-group; }`).
- [ ] Teste avec `Ctrl+P` → aperçu avant impression.

---

## 75. Reset CSS et méthodologie BEM

### Reset moderne minimal

Les navigateurs ont des styles par défaut incohérents. Ce reset les harmonise :

```css
/* 1. Box model sain */
*, *::before, *::after { box-sizing: border-box; }

/* 2. Marges par défaut supprimées (on les remet où besoin) */
* { margin: 0; }

/* 3. Médias responsives */
img, video, svg { max-width: 100%; height: auto; display: block; }

/* 4. Formulaires : héritent la typo */
input, button, textarea, select { font: inherit; }

/* 5. Body : base saine */
body {
  line-height: 1.6;
  font-family: system-ui, sans-serif;
  min-height: 100vh;
}
```

### BEM : nommer ses classes sans chaos

**BEM** = Bloc __ Élément -- Modificateur. Parfait pour des dashboards maintenus à plusieurs.

```html
<article class="carte carte--alerte">
  <h2 class="carte__titre">UPS-02 — Défaut batterie</h2>
  <p class="carte__meta">Datacenter · 26/09/2026</p>
  <button class="carte__action carte__action--primaire" type="button">
    Voir le ticket
  </button>
</article>
```

```css
.carte { /* bloc : la carte */ }
.carte__titre { /* élément : le titre DANS la carte */ }
.carte--alerte { /* modificateur : variante alerte */ }
.carte__action--primaire { /* bouton primaire de la carte */ }
```

**Avantages :** pas de conflits de noms, spécificité basse (que des classes), on comprend le HTML sans ouvrir le CSS.

---

## 76. Performance web : l'essentiel

| Levier | Action | Gain |
|--------|--------|------|
| Images | WebP/AVIF, `srcset`, `loading="lazy"`, dimensions | Souvent **-70 %** du poids |
| CSS/JS | 1 fichier chacun, minifiés, `defer` pour JS | Moins de requêtes |
| Polices | `system-ui` ou `font-display: swap` | Pas de texte invisible |
| Iframes/vidéos | `loading="lazy"`, `preload="metadata"` | Charge au besoin |
| Cache | En-têtes serveur (avec ton admin sys 😉) | Visites suivantes instantanées |

```html
<!-- Ordre de chargement optimal -->
<link rel="stylesheet" href="css/style.css">
<script src="js/main.js" defer></script>  <!-- defer : exécuté après le HTML -->
```

**Mesurer :**

- DevTools → **Network** : poids total, requêtes, temps.
- **Lighthouse** : score + recommandations concrètes.
- Objectifs raisonnables (outil interne) : < 1 Mo par page, < 2 s de chargement.

```css
/* Évite le CLS (sauts de mise en page) : réserve la place des images */
img { aspect-ratio: attr(width) / attr(height); }
/* ou directement : */
.vignette { aspect-ratio: 16 / 9; object-fit: cover; width: 100%; }
```

> 🎯 Le plus rentable : **optimiser les images**. Sur la plupart des pages, elles font 60–80 % du poids.

---

## 77. Bonnes pratiques HTML

- [ ] `<!DOCTYPE html>` + `<html lang="fr">` + `<meta charset>` + viewport (§7).
- [ ] **Un seul** `<h1>`, hiérarchie sans saut (§11).
- [ ] Balises **sémantiques** au lieu de `<div>` partout (§32–33).
- [ ] Chaque `<img>` a un `alt` pertinent (§42) + `width`/`height` (§15).
- [ ] Chaque champ de formulaire a un `<label>` (§20).
- [ ] Liens explicites (jamais « cliquez ici ») (§14).
- [ ] `rel="noopener"` avec `target="_blank"` (§14).
- [ ] Attributs entre guillemets, minuscules, balises fermées dans l'ordre (§9).
- [ ] Pas de style inline `style=""` (sauf dynamique JS) — tout en CSS (§50).
- [ ] Pas de balise obsolète (§34), pas de tableau de mise en page (§18).
- [ ] `id` uniques dans la page.
- [ ] Validation W3C à zéro erreur avant mise en prod (§6).
- [ ] Commentaires pour structurer, **jamais** d'infos sensibles dedans (§10).

---

## 78. Bonnes pratiques CSS

- [ ] `box-sizing: border-box` global (§56).
- [ ] Variables CSS pour la palette et les espacements (§68).
- [ ] Classes plutôt qu'ID ; **zéro `!important`** (§53).
- [ ] Nommage cohérent (BEM ou équivalent) (§75).
- [ ] `rem` pour les textes, `%`/`fr` pour les largeurs fluides (§55).
- [ ] Mobile-first : base mobile + `min-width` (§66).
- [ ] `:focus-visible` toujours visible (§46) ; jamais de `:hover` sans équivalent clavier (§69).
- [ ] `prefers-reduced-motion` dès qu'il y a animation (§49).
- [ ] Dark mode via variables (§73) si pertinent.
- [ ] Print CSS pour les documents (§74).
- [ ] Un seul fichier CSS par site (ou découpage logique importé), minifié en prod.
- [ ] Contrastes AA minimum (§47).

---

## 79. Erreurs classiques des débutants (12)

### 1. Oublier le doctype
→ Quirks mode, box model cassé (§8). **Toujours** `<!DOCTYPE html>` en ligne 1.

### 2. `<button>` sans `type` dans un formulaire
→ Il soumet le formulaire par défaut (§23). Toujours `type="button|submit|reset"`.

### 3. Placeholder au lieu de label
→ Inaccessible, disparaît à la saisie (§20). Label visible **+** placeholder en complément.

### 4. « Cliquez ici » comme texte de lien
→ Incompréhensible hors contexte (§14). Décris la destination.

### 5. Images sans dimensions
→ Sauts de mise en page (CLS) (§15, §76). Toujours `width` + `height`.

### 6. Chemins absolus Windows dans `src`
→ `C:\Users\…` ne marche que sur ton PC (§16). Chemins relatifs au projet.

### 7. Sauter des niveaux de titres (`h1` → `h3`)
→ Casse la navigation des lecteurs d'écran (§11). Ordre strict.

### 8. `display: none` vs `visibility: hidden` confondus
→ Le second garde l'élément focusable = focus fantôme (§58).

### 9. `outline: none` sans remplacement
→ Plus aucun repère clavier = site inutilisable sans souris (§46). Toujours un `:focus-visible` visible.

### 10. ID en double
→ `document.getElementById` ne trouve que le premier, ancres cassées, HTML invalide. Un ID = **un seul** élément.

### 11. `z-index: 9999` qui « ne marche pas »
→ Problème de **contexte d'empilement**, pas de valeur (§64). Remonte au parent.

### 12. Oublier `enctype` pour l'upload
→ Sans `enctype="multipart/form-data"`, le fichier n'est jamais transmis (§28).

---

## 80. Cas pratique 1 : page dashboard (commentée)

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="robots" content="noindex, nofollow"><!-- outil interne -->
  <title>Supervision énergie — Dashboard</title>
  <link rel="stylesheet" href="css/dashboard.css">
</head>
<body>
  <!-- Skip link : accès direct au contenu (§43) -->
  <a class="skip-link" href="#contenu">Aller au contenu</a>

  <div class="page">
    <header class="topbar">
      <p class="logo">⚡ Supervision Énergie</p>
      <nav aria-label="Navigation principale">
        <ul>
          <li><a href="/" aria-current="page">Dashboard</a></li>
          <li><a href="/tickets">Tickets</a></li>
          <li><a href="/docs">Docs</a></li>
        </ul>
      </nav>
      <!-- Zone live : annonces des changements d'état (§44) -->
      <p class="horloge" aria-live="off">Maj : <time datetime="2026-09-26T23:30">23:30</time></p>
    </header>

    <main id="contenu" class="contenu">
      <h1>État des équipements</h1>

      <!-- Cartes KPI : <ul> car c'est une LISTE de données -->
      <ul class="kpis">
        <li class="kpi kpi--ok">
          <p class="kpi__valeur">42</p>
          <p class="kpi__label">Équipements en ligne</p>
        </li>
        <li class="kpi kpi--warn">
          <p class="kpi__valeur">3</p>
          <p class="kpi__label">Avertissements</p>
        </li>
        <li class="kpi kpi--ko">
          <p class="kpi__valeur">1</p>
          <p class="kpi__label">Défaut critique</p>
        </li>
      </ul>

      <!-- Tableau accessible (§18-19) dans conteneur scrollable -->
      <section aria-labelledby="t-equipements">
        <h2 id="t-equipements">Détail par équipement</h2>
        <div class="table-scroll">
          <table>
            <caption>État temps réel du parc</caption>
            <thead>
              <tr>
                <th scope="col">Équipement</th>
                <th scope="col">Type</th>
                <th scope="col">Charge</th>
                <th scope="col">Statut</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <th scope="row">UPS-01</th>
                <td>Onduleur 30 kVA</td>
                <td>42 %</td>
                <!-- Statut : couleur + TEXTE (§47) -->
                <td><span class="statut statut--ok"><span aria-hidden="true">●</span> En ligne</span></td>
              </tr>
              <tr>
                <th scope="row">UPS-02</th>
                <td>Onduleur 60 kVA</td>
                <td>—</td>
                <td><span class="statut statut--ko"><span aria-hidden="true">●</span> Défaut batterie</span></td>
              </tr>
            </tbody>
          </table>
        </div>
      </section>
    </main>

    <footer class="pied">
      <p>Service Systèmes & Énergies — usage interne</p>
    </footer>
  </div>
</body>
</html>
```

```css
/* css/dashboard.css — extraits clés */
*, *::before, *::after { box-sizing: border-box; }
* { margin: 0; }

:root {
  --fond: #f8fafc; --carte: #fff; --texte: #1e293b;
  --ok: #16a34a; --warn: #d97706; --ko: #dc2626;
  --rayon: .75rem;
}

/* Layout : grid areas (§62) */
.page {
  display: grid;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}
.topbar {
  display: flex; align-items: center; gap: 1.5rem;  /* flexbox §59 */
  padding: 1rem 1.5rem; background: #0f172a; color: #fff;
}
.topbar ul { display: flex; gap: 1rem; list-style: none; padding: 0; }
.topbar a { color: #e2e8f0; }
.topbar a[aria-current="page"] { color: #fff; font-weight: 700; }
.horloge { margin-left: auto; color: #94a3b8; font-size: .875rem; }

.contenu { padding: 1.5rem; max-width: 1200px; width: 100%; margin: 0 auto; }
.contenu h1 { margin-bottom: 1rem; }

/* KPIs : grid auto-responsive (§61) */
.kpis {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
  gap: 1rem; list-style: none; padding: 0; margin-bottom: 2rem;
}
.kpi {
  background: var(--carte); border-radius: var(--rayon);
  padding: 1rem; border-left: 5px solid var(--ok);
}
.kpi--warn { border-color: var(--warn); }
.kpi--ko   { border-color: var(--ko); }
.kpi__valeur { font-size: 2rem; font-weight: 800; }
.kpi__label  { color: #64748b; font-size: .875rem; }

/* Statuts : pastille + texte (jamais couleur seule) */
.statut { font-weight: 600; white-space: nowrap; }
.statut--ok   { color: var(--ok); }
.statut--warn { color: var(--warn); }
.statut--ko   { color: var(--ko); }

/* Tableau */
.table-scroll { overflow-x: auto; }
table { border-collapse: collapse; width: 100%; min-width: 640px; background: var(--carte); }
caption { text-align: left; font-weight: 700; padding: .5rem 0; }
th, td { padding: .75rem; border-bottom: 1px solid #e2e8f0; text-align: left; }
tbody tr:nth-child(even) { background: #f8fafc; }  /* zébré §69 */
tbody tr:hover { background: #e0f2fe; }

.pied { text-align: center; padding: 1rem; color: #64748b; font-size: .875rem; }

/* Focus visible partout (§46) */
:focus-visible { outline: 3px solid #2563eb; outline-offset: 2px; }

/* Skip link (§43) */
.skip-link { position: absolute; left: -9999px; background: #fff; padding: .5rem 1rem; z-index: 300; }
.skip-link:focus { left: 1rem; top: 1rem; }
```

> 💡 Ce dashboard coche : sémantique, skip link, `aria-current`, zone live prête, statuts texte+couleur, tableau scrollable, grid responsive sans breakpoint, focus visible.

---

## 81. Cas pratique 2 : formulaire de ticket (commenté)

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="robots" content="noindex, nofollow">
  <title>Nouveau ticket — Service Systèmes & Énergies</title>
  <link rel="stylesheet" href="css/ticket.css">
</head>
<body>
  <main class="form-page">
    <h1>Créer un ticket d'intervention</h1>
    <p>* Champs obligatoires</p>

    <!-- novalidate retiré : on garde la validation native (§26) -->
    <form action="/api/tickets" method="post" enctype="multipart/form-data">

      <fieldset>
        <legend>Demandeur</legend>
        <div class="champ">
          <label for="nom">Nom *</label>
          <input type="text" id="nom" name="nom" required autocomplete="name"
                 minlength="2" maxlength="60">
        </div>
        <div class="champ">
          <label for="email">E-mail professionnel *</label>
          <input type="email" id="email" name="email" required autocomplete="email"
                 aria-describedby="aide-email">
          <p class="aide" id="aide-email">La confirmation sera envoyée à cette adresse.</p>
        </div>
        <div class="champ">
          <label for="tel">Téléphone (10 chiffres)</label>
          <input type="tel" id="tel" name="tel" inputmode="numeric"
                 pattern="[0-9]{10}" title="10 chiffres sans espaces">
        </div>
      </fieldset>

      <fieldset>
        <legend>Équipement concerné</legend>
        <div class="champ">
          <label for="famille">Famille *</label>
          <select id="famille" name="famille" required>
            <option value="">— Choisir —</option>
            <optgroup label="Énergie">
              <option value="ups">Onduleur</option>
              <option value="batterie">Batteries</option>
              <option value="ge">Groupe électrogène</option>
              <option value="tgbt">TGBT / armoire</option>
            </optgroup>
            <optgroup label="Climatisation">
              <option value="clim">Climatiseur</option>
            </optgroup>
          </select>
        </div>
        <div class="champ">
          <label for="ref">Référence (format ABC-12) *</label>
          <input type="text" id="ref" name="ref" required
                 pattern="[A-Z]{3}-[0-9]{2}" title="Ex : UPS-02">
        </div>
        <div class="champ">
          <label for="date">Date souhaitée *</label>
          <!-- min = demain : pas d'intervention dans le passé -->
          <input type="date" id="date" name="date" required min="2026-09-27">
        </div>
      </fieldset>

      <fieldset>
        <legend>Criticité</legend>
        <!-- radios : même name = choix unique (§21) -->
        <label class="choix"><input type="radio" name="crit" value="basse"> Basse</label>
        <label class="choix"><input type="radio" name="crit" value="normale" checked> Normale</label>
        <label class="choix"><input type="radio" name="crit" value="haute"> Haute</label>
        <label class="choix"><input type="radio" name="crit" value="critique"> Critique</label>
      </fieldset>

      <fieldset>
        <legend>Description</legend>
        <div class="champ">
          <label for="desc">Décrivez le problème *</label>
          <textarea id="desc" name="desc" rows="5" required minlength="20"
                    placeholder="Ex : alarme batterie depuis hier 14 h, tension floating à 12,1 V…"></textarea>
        </div>
        <div class="champ">
          <label for="pj">Photo du défaut (JPG/PNG, optionnel)</label>
          <input type="file" id="pj" name="pj" accept="image/jpeg,image/png">
        </div>
      </fieldset>

      <div class="actions">
        <!-- type="button" : ne soumet PAS (§23) -->
        <button type="button" id="btn-brouillon">Enregistrer en brouillon</button>
        <button type="submit" class="primaire">Envoyer le ticket</button>
      </div>
    </form>
  </main>
</body>
</html>
```

```css
/* css/ticket.css — extraits */
.form-page { max-width: 640px; margin: 2rem auto; padding: 0 1rem; }
fieldset {
  border: 1px solid #e2e8f0; border-radius: .75rem;
  padding: 1.25rem; margin-bottom: 1.5rem;
}
legend { font-weight: 700; padding: 0 .5rem; }
.champ { margin-bottom: 1rem; }
.champ label { display: block; font-weight: 600; margin-bottom: .25rem; }
.champ input, .champ select, .champ textarea {
  width: 100%; padding: .6rem .75rem;
  border: 1px solid #cbd5e1; border-radius: .5rem;
}
.aide { font-size: .875rem; color: #64748b; margin-top: .25rem; }
.choix { display: inline-flex; align-items: center; gap: .4rem; margin-right: 1rem; }

/* Feedback validation (§26) */
input:required, select:required, textarea:required { border-left: 3px solid #f59e0b; }
input:invalid:not(:placeholder-shown),
textarea:invalid:not(:placeholder-shown) { border-color: #dc2626; }
input:valid:not(:placeholder-shown) { border-color: #16a34a; }

.actions { display: flex; gap: 1rem; justify-content: flex-end; }
button { padding: .75rem 1.25rem; border-radius: .5rem; border: 1px solid #cbd5e1; cursor: pointer; }
button.primaire { background: #2563eb; color: #fff; border-color: #2563eb; }
button:disabled { opacity: .5; cursor: not-allowed; }
:focus-visible { outline: 3px solid #2563eb; outline-offset: 2px; }
```

> 💡 Points forts : fieldsets légendés, `autocomplete`, `pattern` + `title`, feedback visuel `:valid`/`:invalid`, boutons typés, `enctype` pour l'upload, `min` sur la date.

---

## 82. Cas pratique 3 : page de documentation interne

```html
<article class="doc">
  <header>
    <!-- Fil d'Ariane : nav secondaire labellisée (§44) -->
    <nav aria-label="Fil d'Ariane">
      <ol class="ariane">
        <li><a href="/docs">Documentation</a></li>
        <li><a href="/docs/ups">Onduleurs</a></li>
        <li aria-current="page">Maintenance préventive</li>
      </ol>
    </nav>
    <h1>Maintenance préventive des onduleurs</h1>
    <p class="meta">Mis à jour le <time datetime="2026-09-26">26/09/2026</time>
      · Service Systèmes & Énergies</p>
  </header>

  <!-- Sommaire auto : ancres vers les sections -->
  <nav aria-label="Sommaire">
    <h2>Sommaire</h2>
    <ul>
      <li><a href="#periodicites">Périodicités</a></li>
      <li><a href="#valeurs">Valeurs de référence</a></li>
      <li><a href="#procedure">Procédure</a></li>
    </ul>
  </nav>

  <section id="periodicites">
    <h2>Périodicités</h2>
    <table>
      <caption>Fréquence des contrôles</caption>
      <thead>
        <tr><th scope="col">Contrôle</th><th scope="col">Fréquence</th></tr>
      </thead>
      <tbody>
        <tr><td>Inspection visuelle</td><td>Trimestrielle</td></tr>
        <tr><td>Test d'impédance batteries</td><td>Annuelle</td></tr>
        <tr><td>Thermographie</td><td>Annuelle</td></tr>
      </tbody>
    </table>
  </section>

  <section id="valeurs">
    <h2>Valeurs de référence</h2>
    <dl>
      <dt>Tension de floating / bloc 12 V</dt>
      <dd>13,5 – 13,6 V à 20 °C</dd>
      <dt>Charge maximale conseillée</dt>
      <dd>80 % de la puissance nominale</dd>
    </dl>
  </section>

  <section id="procedure">
    <h2>Procédure</h2>
    <ol>
      <li>Consigner l'armoire (voir <a href="/docs/consignation">procédure de consignation</a>).</li>
      <li>Mesurer la tension du bus DC.</li>
      <li>…
        <!-- Accordéon natif pour le détail (§33) -->
        <details>
          <summary>Détail de la mesure du bus DC</summary>
          <p>Utiliser un multimètre CAT III…</p>
        </details>
      </li>
    </ol>
  </section>

  <footer>
    <p>Document interne — <a href="mailto:energie@entreprise.fr">signaler une erreur</a></p>
  </footer>
</article>
```

> 💡 Structure idéale d'une doc : fil d'Ariane + `aria-current`, sommaire à ancres, sections titrées, `<dl>` pour les valeurs, `<details>` pour les approfondissements, JSON-LD `TechArticle` (§39) dans le head pour la doc publique.

---

## 83. Cas pratique 4 : carte équipement réutilisable

Composant **BEM** (§75) : la même carte sert sur le dashboard, la recherche, les favoris.

```html
<article class="eq-card eq-card--ko">
  <div class="eq-card__entete">
    <h3 class="eq-card__titre">UPS-02</h3>
    <span class="eq-card__statut"><span aria-hidden="true">●</span> Défaut batterie</span>
  </div>
  <dl class="eq-card__specs">
    <div>
      <dt>Puissance</dt><dd>60 kVA</dd>
    </div>
    <div>
      <dt>Local</dt><dd>Datacenter</dd>
    </div>
    <div>
      <dt>Charge</dt><dd>—</dd>
    </div>
    <div>
      <dt>Maintenance</dt><dd><time datetime="2026-11-02">02/11/2026</time></dd>
    </div>
  </dl>
  <div class="eq-card__actions">
    <a class="btn" href="/tickets/nouveau?eq=UPS-02">Créer un ticket</a>
    <a class="btn btn--secondaire" href="/docs/ups-02">Documentation</a>
  </div>
</article>
```

```css
.eq-card {
  --accent: #16a34a; /* variante par défaut */
  background: #fff; border: 1px solid #e2e8f0; border-radius: .75rem;
  border-top: 4px solid var(--accent);
  padding: 1.25rem;
  display: flex; flex-direction: column; gap: 1rem; /* flex vertical §59 */
}
.eq-card--ko   { --accent: #dc2626; }
.eq-card--warn { --accent: #d97706; }

.eq-card__entete { display: flex; justify-content: space-between; align-items: baseline; }
.eq-card__titre { font-size: 1.25rem; }
.eq-card__statut { font-weight: 700; color: var(--accent); white-space: nowrap; }

.eq-card__specs {
  display: grid; grid-template-columns: repeat(2, 1fr); gap: .5rem 1rem;
  margin: 0;
}
.eq-card__specs div { display: contents; } /* dt/dd restent dans la grille */
.eq-card__specs dt { color: #64748b; font-size: .875rem; }
.eq-card__specs dd { margin: 0; font-weight: 600; }

.eq-card__actions { display: flex; gap: .75rem; margin-top: auto; }
.btn {
  display: inline-block; padding: .6rem 1rem; border-radius: .5rem;
  background: #2563eb; color: #fff; text-decoration: none; text-align: center;
  transition: background-color .2s ease;
}
.btn:hover { background: #1d4ed8; }
.btn--secondaire { background: #f1f5f9; color: #1e293b; }
```

> 💡 La variante (`--ko`) ne change qu'une **variable** : le reste suit. C'est la puissance du combo BEM + custom properties.

---

## 84. Checklist de mise en production

Avant de publier une page (même interne), passe cette checklist :

**HTML**
- [ ] Doctype + `lang` + charset + viewport présents.
- [ ] Validateur W3C : zéro erreur.
- [ ] Un seul `<h1>`, hiérarchie sans saut.
- [ ] Toutes les images ont un `alt` (éventuellement vide).
- [ ] Tous les champs ont un `<label>`.
- [ ] Liens explicites, `rel="noopener"` sur les `_blank`.

**Accessibilité**
- [ ] Navigation complète au clavier (test sans souris).
- [ ] Focus visible partout.
- [ ] Contrastes AA vérifiés.
- [ ] Test lecteur d'écran rapide (NVDA gratuit / VoiceOver intégré).
- [ ] `prefers-reduced-motion` géré si animations.

**SEO / metas** (si page publique)
- [ ] `<title>` et `description` uniques et soignés.
- [ ] Open Graph + image 1200×630.
- [ ] JSON-LD validé (Rich Results Test).
- [ ] `canonical` si doublons possibles.

**Performance**
- [ ] Poids total < 1 Mo, images optimisées + `lazy`.
- [ ] Lighthouse ≥ 90 (perf + accessibilité).
- [ ] Test mobile (émulateur + vrai téléphone si possible).

**Contenu**
- [ ] Pas d'info sensible dans le code ou les commentaires.
- [ ] Dates, chiffres, références vérifiés.
- [ ] Page 404 personnalisée si site public.

---

## 85. Pense-bête de poche

### Squelette HTML5
```html
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>…</title>
<link rel="stylesheet" href="style.css">
</head>
<body>
</body>
</html>
```

### Balises à connaître par cœur
`header nav main article section aside footer` · `h1–h6 p ul ol li dl dt dd` ·
`a img figure figcaption` · `table caption thead tbody tfoot tr th td` ·
`form label input select textarea button fieldset legend` · `video audio iframe` ·
`details summary dialog` · `strong em mark code kbd time`

### Formulaire express
```html
<form action="…" method="post">
  <label for="x">Nom</label>
  <input type="text" id="x" name="x" required>
  <button type="submit">OK</button>
</form>
```

### Sélecteurs CSS express
```css
.class #id element [attr="v"]   /* base */
parent enfant  parent > enfant   /* descendant / direct */
a + b  a ~ b                     /* adjacent / suivants */
a:hover a:focus-visible          /* états */
li:first-child li:nth-child(2n)  /* position */
::before ::after                 /* pseudo-éléments */
```

### Flexbox express
```css
.c { display:flex; justify-content:center; align-items:center; gap:1rem; }
/* justify = axe principal · align = axe secondaire */
```

### Grid express
```css
.g { display:grid; grid-template-columns:repeat(auto-fit,minmax(250px,1fr)); gap:1rem; }
```

### Centrage express
```css
.centrer { display:flex; justify-content:center; align-items:center; }
```

### Media query express
```css
@media (min-width:768px){ … }
```

### Variables express
```css
:root{ --p:#2563eb; }  .x{ color:var(--p); }
```

### Commandes utiles
| Action | Commande / touche |
|--------|-------------------|
| Valider HTML | https://validator.w3.org |
| Inspecter | `F12` ou `Ctrl+Maj+I` |
| Émulateur mobile | `Ctrl+Maj+M` (dans DevTools) |
| Audit | Onglet Lighthouse |
| Squelette Emmet | `!` + `Tab` (VS Code) |
| Formater (Prettier) | `Maj+Alt+F` (VS Code) |

---

## 86. Glossaire

| Terme | Définition |
|-------|------------|
| **Accessibilité (a11y)** | Pratiques rendant le web utilisable par tous (WCAG, RGAA) |
| **Ancre** | Lien vers un `id` de la page (`href="#section"`) |
| **ARIA** | Attributs enrichissant l'accessibilité (`role`, `aria-*`) |
| **Attribut** | Paramètre d'une balise (`href`, `src`, `class`) |
| **Balise** | Marqueur HTML (`<p>`, `<div>`) — ouvrante/fermante ou orpheline |
| **BEM** | Convention de nommage CSS : Bloc__Élément--Modificateur |
| **Box model** | Modèle de boîte : contenu + padding + border + margin |
| **Breakpoint** | Largeur d'écran déclenchant une media query |
| **Cascade** | Règles de priorité entre déclarations CSS |
| **Charset** | Jeu de caractères (`utf-8`) |
| **CLS** | Cumulative Layout Shift : sauts de mise en page au chargement |
| **DOM** | Arbre d'objets construit par le navigateur depuis le HTML |
| **Doctype** | Déclaration `<!DOCTYPE html>` (évite le quirks mode) |
| **Emmet** | Abréviations générant du HTML/CSS (intégré à VS Code) |
| **Entité HTML** | Code pour caractère spécial (`&lt;`, `&amp;`) |
| **Favicon** | Icône d'onglet du site |
| **Flexbox** | Mise en page CSS sur un axe |
| **Grid** | Mise en page CSS sur deux axes |
| **Héritage** | Transmission de propriétés CSS parent → enfant |
| **JSON-LD** | Données structurées pour les moteurs (schema.org) |
| **Landmark** | Repère sémantique (header, nav, main…) pour lecteurs d'écran |
| **Lazy loading** | Chargement différé (`loading="lazy"`) |
| **Lighthouse** | Outil d'audit (perf, accessibilité, SEO) intégré à Chrome |
| **Media query** | Règle CSS conditionnelle (`@media (min-width: …)`) |
| **Mobile-first** | Concevoir mobile d'abord, enrichir ensuite |
| **Open Graph** | Metas pour les aperçus de partage (réseaux sociaux, Teams…) |
| **Quirks mode** | Mode de compatibilité buggé sans doctype |
| **RGAA** | Référentiel d'accessibilité français |
| **Responsive** | Mise en page s'adaptant à toutes les tailles d'écran |
| **Sémantique** | Balises décrivant le sens (`article`, `nav`…) vs `<div>` générique |
| **SEO** | Optimisation pour les moteurs de recherche |
| **Skip link** | Lien « aller au contenu » pour la navigation clavier |
| **Spécificité** | Score déterminant quelle règle CSS s'applique |
| **Srcset** | Attribut fournissant plusieurs résolutions d'image |
| **Viewport** | Zone visible de la page (meta indispensable au responsive) |
| **W3C** | Organisme de standardisation du web (+ son validateur) |
| **WCAG** | Règles internationales d'accessibilité web |
| **WHATWG** | Groupe maintenant le standard HTML vivant |
| **z-index** | Ordre d'empilement des éléments positionnés |

---

## 87. Quiz : 10 questions + réponses

**Q1.** Où doit se trouver `<!DOCTYPE html>` et pourquoi ?
> **R.** En toute première ligne du document. Sans lui, le navigateur passe en quirks mode (box model faussé, comportements incohérents).

**Q2.** Quelle est la différence entre `<strong>` et `<b>` ?
> **R.** `<strong>` indique une importance forte (les lecteurs d'écran insistent dessus) ; `<b>` n'est que visuel. Même logique pour `<em>` vs `<i>`.

**Q3.** Pourquoi ce bouton pose problème dans un formulaire : `<button>Fermer</button>` ?
> **R.** Sans `type`, un `<button>` dans un formulaire vaut `type="submit"` : cliquer « Fermer » enverrait le formulaire. Il faut `type="button"`.

**Q4.** Un champ avec seulement `placeholder="Nom"` : quel est le problème ?
> **R.** Le placeholder disparaît à la saisie et n'est pas un label fiable pour les lecteurs d'écran. Il faut un vrai `<label>` ; le placeholder n'est qu'un indice complémentaire.

**Q5.** Calcule la spécificité : `nav ul li a` vs `.menu a`. Laquelle gagne ?
> **R.** `nav ul li a` = (0,0,0,4) ; `.menu a` = (0,0,1,1). Une classe bat 4 éléments : **`.menu a` gagne**.

**Q6.** Que change `box-sizing: border-box` ?
> **R.** `width`/`height` incluent padding et border (au lieu du seul contenu). Fini les largeurs qui dépassent : `width: 300px` = 300 px affichés.

**Q7.** Flexbox ou Grid pour une barre de navigation ? Pour un dashboard complet ?
> **R.** Flexbox pour la nav (un seul axe), Grid pour le dashboard (lignes + colonnes). Les deux se combinent.

**Q8.** Cite 3 attributs indispensables d'un `<img>` bien écrit.
> **R.** `src`, `alt` (texte alternatif, éventuellement vide si décoratif), `width` + `height` (évitent les sauts de mise en page). Bonus : `loading="lazy"`.

**Q9.** À quoi sert `rel="noopener"` ?
> **R.** Avec `target="_blank"`, il empêche la page cible d'accéder à `window.opener` (la page d'origine) — faille de sécurité dite « tabnabbing ».

**Q10.** Ton menu passe sous un bandeau malgré `z-index: 9999`. Diagnostic ?
> **R.** Problème de **contexte d'empilement** (§64), pas de valeur : le parent du menu crée un contexte (ex. `opacity`, `transform`, `position`+`z-index`) qui l'enferme sous le bandeau. Solution : restructurer/remonter le menu dans le DOM ou isoler avec `isolation: isolate`.

---

## 88. Pour aller plus loin

### Approfondir HTML/CSS
- **MDN Web Docs** (developer.mozilla.org) — LA référence, en français, avec compatibilités navigateurs.
- **web.dev** (Google) — guides perf, accessibilité, responsive.
- **W3C Validator** — valider en continu.
- **Can I Use** (caniuse.com) — vérifier le support d'une propriété CSS.

### Accessibilité
- **WCAG 2.2** (w3.org) — les règles officielles.
- **RGAA** (accessibilite.numerique.gouv.fr) — le référentiel français.
- **NVDA** (gratuit) — tester au lecteur d'écran sous Windows.
- **axe DevTools** (extension) — audits automatisés.

### S'entraîner
- **Flexbox Froggy** / **Grid Garden** — apprendre flexbox/grid en jouant.
- **CSS Diner** — les sélecteurs en s'amusant.
- Refais les 4 cas pratiques (§80–83) **sans regarder**, puis compare.

### Étapes suivantes (JavaScript)
1. Manipuler le DOM (`querySelector`, `addEventListener`).
2. Valider les formulaires côté client + messages custom (§27).
3. `fetch()` : envoyer le formulaire de ticket vers ton API.
4. Stocker le thème dark/light en `localStorage`.

### Feuille de route suggérée (4 semaines, 3 h/sem.)
- **S1** : sections 1–35 → squelette + formulaires + tableaux.
- **S2** : sections 50–62 → CSS, flexbox, grid → clone le dashboard §80.
- **S3** : sections 41–49 → accessibilité → audite tes pages à l'axe.
- **S4** : cas pratiques + checklist §84 → une vraie page interne en prod.

> 🎯 Dernier conseil : le HTML/CSS s'apprend en **tapant du code**, pas en lisant. Chaque section lue = un exemple testé dans VS Code + Live Server. Bon build, chef ⚡

---

*Fin du guide — v1.0, 26/09/2026. Bon courage pour tes pages internes, dashboards et docs !*
