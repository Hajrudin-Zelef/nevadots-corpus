# JavaScript — Guide ultra-complet

> **Pour Zelef** — chef de service systèmes & énergies, sysadmin.
> Objectif : maîtriser JavaScript pour les scripts système, les outils web internes et Node.js.
> Ton : direct, dense, tutoriel + référence. Chaque section contient du code exécutable, des tableaux et des pense-bête.
> Version ciblée : **ES2024** (syntaxes récentes signalées quand elles dépendent de la version).

---

## Sommaire

1. [Installation & tooling](#1-installation--tooling)
2. [Syntaxe de base](#2-syntaxe-de-base)
3. [var / let / const](#3-var--let--const)
4. [Types primitifs](#4-types-primitifs)
5. [Opérateurs](#5-opérateurs)
6. [Structures de contrôle](#6-structures-de-contrôle)
7. [Fonctions — les fondamentaux](#7-fonctions--les-fondamentaux)
8. [Closures](#8-closures)
9. [Paramètres avancés des fonctions](#9-paramètres-avancés-des-fonctions)
10. [Tableaux](#10-tableaux)
11. [Objets](#11-objets)
12. [Destructuration, spread, rest](#12-destructuration-spread-rest)
13. [Chaînes de caractères](#13-chaînes-de-caractères)
14. [Nombres et Math](#14-nombres-et-math)
15. [Dates](#15-dates)
16. [this — les 4 règles](#16-this--les-4-règles)
17. [Prototypes](#17-prototypes)
18. [Classes](#18-classes)
19. [Héritage et polymorphisme](#19-héritage-et-polymorphisme)
20. [Gestion d'erreurs](#20-gestion-derreurs)
21. [Event loop : call stack, micro/macro-tâches](#21-event-loop--call-stack-micromacro-tâches)
22. [Callbacks et callback hell](#22-callbacks-et-callback-hell)
23. [Promises en profondeur](#23-promises-en-profondeur)
24. [async / await en profondeur](#24-async--await-en-profondeur)
25. [Fetch : GET, POST, erreurs](#25-fetch--get-post-erreurs)
26. [JSON](#26-json)
27. [Timers](#27-timers)
28. [Modules ESM vs CommonJS](#28-modules-esm-vs-commonjs)
29. [npm et package.json](#29-npm-et-packagejson)
30. [Node.js — process, env, CLI](#30-nodejs--process-env-cli)
31. [Node.js — fs et path](#31-nodejs--fs-et-path)
32. [Node.js — http](#32-nodejs--http)
33. [Node.js — child_process](#33-nodejs--child_process)
34. [Node.js — events et streams](#34-nodejs--events-et-streams)
35. [Manipulation du DOM](#35-manipulation-du-dom)
36. [Événements et délégation](#36-événements-et-délégation)
37. [Formulaires](#37-formulaires)
38. [Stockage navigateur : localStorage, cookies](#38-stockage-navigateur--localstorage-cookies)
39. [Web APIs utiles](#39-web-apis-utiles)
40. [RegExp](#40-regexp)
41. [Map, Set, WeakMap, WeakSet](#41-map-set-weakmap-weakset)
42. [Itérateurs et générateurs](#42-itérateurs-et-générateurs)
43. [Symbols et BigInt](#43-symbols-et-bigint)
44. [Proxy et Reflect](#44-proxy-et-reflect)
45. [Programmation fonctionnelle](#45-programmation-fonctionnelle)
46. [Tests unitaires](#46-tests-unitaires)
47. [Debug](#47-debug)
48. [Bonnes pratiques & style](#48-bonnes-pratiques--style)
49. [Performance](#49-performance)
50. [Sécurité](#50-sécurité)
51. [Erreurs classiques des débutants (15)](#51-erreurs-classiques-des-débutants-15)
52. [Cas pratique 1 — parser de logs Apache/nginx](#52-cas-pratique-1--parser-de-logs-apachenginx)
53. [Cas pratique 2 — client API REST avec retry](#53-cas-pratique-2--client-api-rest-avec-retry)
54. [Cas pratique 3 — mini-serveur HTTP maison](#54-cas-pratique-3--mini-serveur-http-maison)
55. [Cas pratique 4 — script d'inventaire réseau](#55-cas-pratique-4--script-dinventaire-réseau)
56. [Cas pratique 5 — dashboard web interne](#56-cas-pratique-5--dashboard-web-interne)
57. [Cas pratique 6 — automatisation de fichiers](#57-cas-pratique-6--automatisation-de-fichiers)
58. [Cas pratique 7 — planificateur avec file de tâches](#58-cas-pratique-7--planificateur-avec-file-de-tâches)
59. [Pense-bête de poche](#59-pense-bête-de-poche)
60. [Glossaire](#60-glossaire)
61. [Quiz — 10 questions + réponses](#61-quiz--10-questions--réponses)
62. [Pour aller plus loin](#62-pour-aller-plus-loin)
63. [Checklist de mise en production](#63-checklist-de-mise-en-production)
64. [Annexe — tableau de compatibilité ES](#64-annexe--tableau-de-compatibilité-es)
65. [Annexe — recettes express sysadmin](#65-annexe--recettes-express-sysadmin)

---

## 1. Installation & tooling

### 1.1 Installer Node.js (LTS recommandée)

| Méthode | Commande / action |
|---|---|
| Windows | Télécharger le MSI LTS sur nodejs.org, ou `winget install OpenJS.NodeJS.LTS` |
| Debian/Ubuntu | `curl -fsSL https://deb.nodesource.com/setup_22.x \| sudo -E bash - && sudo apt install -y nodejs` |
| Gestionnaire de versions (recommandé) | `nvm` (Linux/macOS) ou `fnm` (multi-OS, rapide) |

```bash
# Avec fnm (recommandé, rapide, multi-shell)
curl -fsSL https://fnm.vercel.app/install | bash
fnm install 22
fnm default 22

# Vérifications
node --version   # v22.x.x
npm --version    # 10.x.x
```

### 1.2 L'environnement de travail

- **Éditeur** : VS Code + extensions ESLint, Prettier, "Error Lens".
- **REPL** : `node` en terminal pour tester vite ; `.help`, `.exit`, `_` = dernier résultat.
- **Navigateur** : console DevTools (F12) pour le DOM et le web.
- **Deno / Bun** : alternatives à Node (Bun = très rapide, compatible npm).

### 1.3 Premier script

```js
// hello.js
console.log("Bonjour depuis Node.js !");
console.log(`Version Node : ${process.version}`);
```

```bash
node hello.js
```

### 1.4 Checklist tooling

- [ ] Node LTS installé + vérifié (`node -v`)
- [ ] VS Code + ESLint + Prettier
- [ ] `npm init -y` compris (section 29)
- [ ] Savoir lancer `node script.js` et `node --watch script.js` (rechargement auto, Node 18.11+)
- [ ] `"type": "module"` vs CommonJS compris (section 28)

---

## 2. Syntaxe de base

```js
// Commentaire sur une ligne
/* Commentaire
   sur plusieurs lignes */

// Point-virgule : optionnel (ASI) mais recommandé en scripts sérieux
let a = 1;
let b = 2

// Template literals (backticks) : interpolation
const nom = "Zelef";
console.log(`Bonjour ${nom}, il est ${new Date().getHours()}h`);

// console : l'outil n°1
console.log("info");        // standard
console.error("erreur");    // stderr
console.warn("attention");  // avertissement
console.table([{a:1},{a:2}]); // tableau lisible
console.time("t"); /* ... */ console.timeEnd("t"); // chrono
```

### Mots-clés réservés (à ne pas utiliser comme identifiants)

`break case catch class const continue debugger default delete do else export extends finally for function if import in instanceof new return super switch this throw try typeof var void while with yield` (+ `let static enum implements package protected interface private public`, `await` en module).

### Conventions de nommage

| Usage | Convention | Exemple |
|---|---|---|
| Variables, fonctions | camelCase | `nbUtilisateurs`, `calculerTotal()` |
| Constantes globales | SNAKE_UPPER | `MAX_RETRY` |
| Classes | PascalCase | `ServeurHttp` |
| Fichiers | kebab-case | `parse-logs.js` |

---

## 3. var / let / const

C'est **le** point à comprendre en premier : la portée (scope) et la remontée (hoisting).

```js
// var : portée FONCTION, hoisting, réassignable, redéclarable
function demoVar() {
  console.log(x); // undefined (hoisting : déclaration remontée, pas la valeur)
  var x = 5;
  var x = 10; // OK avec var (à éviter !)
}

// let : portée BLOC, pas d'accès avant déclaration (TDZ), réassignable
function demoLet() {
  // console.log(y); // ReferenceError : zone morte temporelle (TDZ)
  let y = 5;
  y = 6; // OK
  // let y = 7; // SyntaxError : déjà déclaré
}

// const : portée BLOC, non réassignable, MAIS objet mutable !
const PI = 3.14159;
// PI = 3; // TypeError
const cfg = { host: "localhost" };
cfg.host = "10.0.0.1"; // OK : on mute le contenu, pas la référence
```

### Tableau comparatif

|  | `var` | `let` | `const` |
|---|---|---|---|
| Portée | fonction | bloc `{}` | bloc `{}` |
| Hoisting | oui (→ `undefined`) | TDZ (erreur si accès avant) | TDZ |
| Redéclaration | autorisée | interdite | interdite |
| Réassignation | autorisée | autorisée | interdite |
| Usage recommandé | **jamais** (code legacy) | variable qui change | **par défaut** |

### Le piège classique avec `var` dans les boucles

```js
// BUG classique
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log("var:", i), 10); // 3, 3, 3 !
}
// CORRIGÉ
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log("let:", i), 10); // 0, 1, 2
}
```

> **Pense-bête** : `const` par défaut, `let` si ça change, `var` jamais.

---

## 4. Types primitifs

JavaScript a 7 types primitifs + `object`. `typeof` renvoie le type (avec le bug historique `typeof null === "object"`).

```js
typeof "hello"      // "string"
typeof 42           // "number"
typeof 42n          // "bigint"
typeof true         // "boolean"
typeof undefined    // "undefined"
typeof Symbol("s")  // "symbol"
typeof null         // "object"  <-- bug historique, à connaître
typeof {}           // "object"
typeof []           // "object"  (utiliser Array.isArray)
typeof function(){} // "function"
```

### Détails par type

```js
// string : immuable, guillemets simples/doubles/backticks équivalents
const s = "Cisco";
s[0];            // "C"
s.length;        // 5
// s[0] = "c";   // silencieux en sloppy, TypeError en strict : immuable !

// number : TOUS les nombres sont des flottants double précision (IEEE 754)
0.1 + 0.2;       // 0.30000000000000004  <-- piège !
Number.MAX_SAFE_INTEGER; // 9007199254740991 (2^53 - 1)

// bigint (ES2020) : entiers arbitraires, suffixe n
const big = 9007199254740993n;
big + 1n;        // 9007199254740994n
// big + 1;      // TypeError : ne pas mélanger bigint et number

// boolean : truthy / falsy
// Falsy : false, 0, -0, 0n, "", null, undefined, NaN  (7 valeurs, à connaître par cœur)
// Tout le reste est truthy, y compris [] et {} !
Boolean([]);     // true  <-- piège classique
Boolean("0");    // true  (chaîne non vide)

// undefined vs null
let x;           // undefined : "pas encore de valeur" (moteur)
let y = null;    // null : "volontairement vide" (programmeur)

// symbol (ES2015) : identifiant unique, voir section 43
const id = Symbol("id");
```

### Conversions

```js
// Explicites (recommandées)
Number("42");    // 42
Number("");      // 0
Number("abc");   // NaN
String(42);      // "42"
Boolean(0);      // false
parseInt("42px", 10); // 42  (toujours préciser la base !)
parseFloat("3.14");   // 3.14

// Implicites (pièges)
"5" + 1;         // "51"  (concaténation !)
"5" - 1;         // 4     (- force la conversion numérique)
"5" * "2";       // 10
true + 1;        // 2
null + 1;        // 1
undefined + 1;   // NaN
```

---

## 5. Opérateurs

### 5.1 Comparaison : `==` vs `===`

```js
// == : égalité LÂCHE (avec coercition de type) — À ÉVITER
0 == "0";        // true
0 == [];         // true  (!!)
"" == false;     // true
null == undefined; // true (seul cas "utile")

// === : égalité STRICTE (type + valeur) — TOUJOURS utiliser
0 === "0";       // false
NaN === NaN;     // false  <-- NaN n'est jamais égal à lui-même !
Object.is(NaN, NaN); // true (Object.is : comparaison "SameValue")
Object.is(0, -0);    // false (=== dirait true)
```

> **Règle d'or** : toujours `===` et `!==`. Pour tester NaN : `Number.isNaN(x)`.

### 5.2 Logiques et courts-circuits

```js
// && et || retournent un OPÉRANDE, pas forcément un booléen
const port = process.env.PORT || 3000;   // valeur par défaut
const user = input && input.trim();      // garde si input truthy

// ?? : nullish coalescing (ES2020) — ne réagit qu'à null/undefined
const p1 = 0 || 3000;    // 3000 (0 est falsy — bug potentiel !)
const p2 = 0 ?? 3000;    // 0 (correct)

// ?. : optional chaining (ES2020) — navigation sécurisée
const ip = srv?.reseau?.ip;  // undefined si un maillon est null/undefined
srv?.demarrer?.();           // n'appelle que si la méthode existe

// ! et !!
const connecte = !!token;    // force en booléen
```

### 5.3 Arithmétiques et autres

```js
let n = 10;
n++; n--;            // incrément / décrément
2 ** 10;             // 1024 (exponentiation, ES2016)
17 % 5;              // 2 (modulo)
10 / 0;              // Infinity
-10 / 0;             // -Infinity
0 / 0;               // NaN

// Affectation composée
n += 5; n -= 2; n *= 3; n **= 2; n %= 4;

// Opérateurs logiques d'affectation (ES2021)
let a2; a2 ??= 5;    // a2 = 5 (si null/undefined)
let b2 = 0; b2 ||= 9; // b2 = 9 (si falsy)
let c2 = 1; c2 &&= 9; // c2 = 9 (si truthy)

// typeof, instanceof, in, delete, void
"host" in {host: "x"};  // true
const o = {a: 1}; delete o.a;
void 0;              // undefined (idiome historique)
```

---

## 6. Structures de contrôle

```js
// if / else if / else
const charge = 82;
if (charge > 90) console.log("CRITIQUE");
else if (charge > 70) console.log("WARNING");
else console.log("OK");

// Ternaire (pour des valeurs, pas pour des effets de bord complexes)
const etat = charge > 90 ? "CRITIQUE" : charge > 70 ? "WARNING" : "OK";

// switch (comparaison stricte ===)
const os = "linux";
switch (os) {
  case "linux": console.log("apt/dnf"); break;
  case "windows": console.log("winget"); break;
  default: console.log("inconnu");
}

// Boucles
for (let i = 0; i < 3; i++) { /* ... */ }

for (const el of [10, 20, 30]) { console.log(el); }      // valeurs (itérables)
for (const k in {a: 1, b: 2}) { console.log(k); }         // clés (objets) — éviter sur tableaux !

let j = 0;
while (j < 3) { j++; }
do { j--; } while (j > 0);   // exécuté au moins une fois

// break / continue avec labels (rare mais utile pour boucles imbriquées)
boucleExterne:
for (const hote of hotes) {
  for (const port of ports) {
    if (port === 22) continue boucleExterne;
  }
}
```

### Choisir la bonne boucle

| Besoin | Construction |
|---|---|
| Compteur classique | `for (let i = 0; ...)` |
| Parcourir un tableau (valeurs) | `for...of` ou `.forEach/.map` |
| Parcourir les clés d'un objet | `for...in` ou `Object.keys()` |
| Condition d'arrêt inconnue | `while` |
| Au moins une exécution | `do...while` |
| Transformer un tableau | `.map/.filter/.reduce` (section 10) |

---

## 7. Fonctions — les fondamentaux

```js
// 1. Déclaration (hoisted : appelable avant sa définition)
function addition(a, b) { return a + b; }

// 2. Expression (non hoisted)
const soustraction = function(a, b) { return a - b; };

// 3. Fléchée (ES2015) — concise, pas de `this` propre, pas de `arguments`
const multiplication = (a, b) => a * b;          // return implicite
const carre = x => x * x;                        // 1 param : parenthèses optionnelles
const ping = () => console.log("pong");
const fabrique = (hote) => ({ hote, port: 22 }); // objet : parenthèses obligatoires

// Fonctions = objets de première classe
function executer(fn, ...args) { return fn(...args); }
executer(addition, 2, 3); // 5
```

### return implicite vs bloc

```js
const f1 = x => x * 2;        // retourne x*2
const f2 = x => { x * 2; };   // retourne undefined ! (bloc sans return)
const f3 = x => { return x * 2; }; // explicite
```

### Hoisting des fonctions

```js
saluer(); // OK
function saluer() { console.log("salut"); }

// aurevoir(); // TypeError : pas une fonction (undefined au moment de l'appel)
const aurevoir = function() { console.log("bye"); };
```

---

## 8. Closures

Une **closure** = une fonction qui "capture" les variables de la portée où elle a été **définie** (pas appelée). C'est le mécanisme derrière les callbacks, les fabriques et l'encapsulation.

```js
function creerCompteur() {
  let n = 0; // variable "privée", capturée
  return {
    incrementer() { n++; return n; },
    valeur() { return n; }
  };
}
const c1 = creerCompteur();
c1.incrementer(); // 1
c1.incrementer(); // 2
const c2 = creerCompteur();
c2.valeur();      // 0 — chaque appel a SA propre closure
```

### Cas d'usage n°1 : fabrique paramétrée

```js
function creerLogger(prefixe) {
  return (message) => console.log(`[${new Date().toISOString()}] [${prefixe}] ${message}`);
}
const logReseau = creerLogger("RESEAU");
const logSys = creerLogger("SYSTEME");
logReseau("lien up");   // [2026-...] [RESEAU] lien up
```

### Cas d'usage n°2 : encapsulation (données privées avant les champs #)

```js
function creerSession(utilisateur) {
  let token = Math.random().toString(36).slice(2); // privé
  return {
    utilisateur,
    getToken: () => token.slice(0, 4) + "****",  // accès contrôlé
    regenerer() { token = Math.random().toString(36).slice(2); }
  };
}
```

### Cas d'usage n°3 : mémoïsation

```js
function memoiser(fn) {
  const cache = new Map(); // capturé par la closure
  return (...args) => {
    const cle = JSON.stringify(args);
    if (cache.has(cle)) return cache.get(cle);
    const res = fn(...args);
    cache.set(cle, res);
    return res;
  };
}
const dnsLent = memoiser((host) => { /* résolution coûteuse */ return host; });
```

> **Piège mémoire** : une closure retient TOUTE sa portée. Ne pas capturer de gros objets inutilement dans des callbacks longue durée (timers, listeners).

---

## 9. Paramètres avancés des fonctions

```js
// Valeurs par défaut (ES2015) — évaluées à chaque appel
function connecter({ host = "localhost", port = 22, timeout = 5000 } = {}) {
  console.log(`ssh ${host} -p ${port} (timeout ${timeout}ms)`);
}
connecter();                    // ssh localhost -p 22 (timeout 5000ms)
connecter({ host: "10.0.0.5" });

// Rest : regroupe les arguments restants en tableau
function somme(...nombres) { return nombres.reduce((a, b) => a + b, 0); }
somme(1, 2, 3, 4); // 10

// arguments (fonctions classiques uniquement) : objet array-like
function legacy() { return Array.from(arguments).join("-"); }

// Paramètres nommés via destructuration (idiome sysadmin : options lisibles)
async function requete(url, { methode = "GET", headers = {}, timeoutMs = 10000, retries = 3 } = {}) {
  /* ... */
}
```

### Bonnes pratiques

- Max ~3 paramètres positionnels ; au-delà → objet d'options.
- Valeurs par défaut pour tout ce qui a une valeur "normale".
- Ne jamais muter les paramètres d'entrée (effet de bord surprise).

---

## 10. Tableaux

```js
const srv = ["web1", "web2", "db1"];

// Accès / bases
srv[0]; srv.length; srv.at(-1); // "db1" (.at() ES2022 : index négatif)
```

### Méthodes mutantes vs non-mutantes

| Mutantes (modifient le tableau) | Non-mutantes (retournent un nouveau) |
|---|---|
| `push`, `pop`, `shift`, `unshift` | `map`, `filter`, `slice`, `concat` |
| `splice`, `sort`, `reverse`, `fill` | `toSorted`, `toReversed`, `toSpliced` (ES2023) |

```js
const nums = [3, 1, 4, 1, 5];

// Transformation (chaînables — le cœur du JS moderne)
nums.filter(n => n > 2)   // [3, 4, 5]
    .map(n => n * 10)     // [30, 40, 50]
    .sort((a, b) => a - b);

nums.reduce((acc, n) => acc + n, 0); // 14 (somme)
nums.find(n => n > 3);    // 4 (premier match)
nums.findIndex(n => n > 3);// 2
nums.some(n => n > 4);    // true (au moins un)
nums.every(n => n > 0);   // true (tous)
nums.includes(4);         // true (avec SameValueZero : trouve NaN !)
nums.indexOf(1);          // 1 (premier index)

// Aplatir / grouper
[[1, 2], [3]].flat();              // [1, 2, 3]
[1, 2, 3].flatMap(x => [x, x * 2]); // [1,2,2,4,3,6]
Object.groupBy(srv, s => s.startsWith("web") ? "web" : "db"); // ES2024

// Tri : TOUJOURS avec comparateur pour les nombres !
[10, 2, 30].sort();            // [10, 2, 30] — tri LEXICOGRAPHIQUE, bug classique
[10, 2, 30].sort((a, b) => a - b); // [2, 10, 30]

// Copie
const copie = [...nums];          // spread (superficielle)
const copie2 = structuredClone(obj); // profonde (navigateur + Node 17+)
```

### Cas sysadmin : pipeline de traitement

```js
const lignes = [
  "10.0.0.1 - - [26/Sep/2026:10:00:01] \"GET /api/status 200\"",
  "10.0.0.2 - - [26/Sep/2026:10:00:02] \"GET /api/data 500\"",
];
const erreurs = lignes
  .map(l => l.match(/"(GET|POST) (\S+) (\d{3})"/))
  .filter(Boolean)
  .filter(([, , , code]) => code.startsWith("5"))
  .map(([, methode, url]) => ({ methode, url }));
console.log(erreurs); // [{ methode: 'GET', url: '/api/data' }]
```

---

## 11. Objets

```js
const routeur = {
  hostname: "R1",
  ip: "10.0.0.1",
  "nom avec espace": "core",   // clé entre guillemets si spéciale
  interfaces: ["ge-0/0/0"],
  redemarrer() { console.log(`${this.hostname} reboot...`); }, // méthode
  ["vlan" + 10]: "users",      // clé calculée (ES2015)
};

// Accès
routeur.hostname;      // notation point (préférée)
routeur["ip"];         // notation crochet (clé dynamique)
const cle = "hostname";
routeur[cle];          // "R1"

// Ajout / suppression
routeur.os = "JunOS";  // ajout
delete routeur.os;     // suppression

// Parcourir
Object.keys(routeur);    // ["hostname", "ip", ...]
Object.values(routeur);  // valeurs
Object.entries(routeur); // [[clé, valeur], ...]
for (const [k, v] of Object.entries(routeur)) { /* ... */ }

// Fusion / copie superficielle
const clone = { ...routeur, ip: "10.0.0.2" };
const fusion = Object.assign({}, routeur, { site: "Paris" });

// Descripteurs : verrouiller un objet
const CONSTANTES = Object.freeze({ SEUIL_CRITIQUE: 90, SEUIL_WARN: 70 });
// CONSTANTES.SEUIL_CRITIQUE = 95; // silencieux / TypeError en strict
```

### Getters / setters

```js
const sonde = {
  _temp: 21,
  get temp() { return `${this._temp}°C`; },
  set temp(v) { if (v < -50 || v > 80) throw new RangeError("plage invalide"); this._temp = v; }
};
sonde.temp = 25;
console.log(sonde.temp); // "25°C"
```

### Vérifier l'existence d'une propriété

```js
"ip" in routeur;                    // true (chaîne de prototype incluse)
routeur.hasOwnProperty("ip");       // true (propre uniquement)
Object.hasOwn(routeur, "ip");       // true (ES2022, recommandé)
routeur.ip !== undefined;           // approximatif (échoue si valeur = undefined)
```

---

## 12. Destructuration, spread, rest

```js
// Tableaux
const [premier, deuxieme, ...reste] = ["a", "b", "c", "d"];
// premier="a", deuxieme="b", reste=["c","d"]
const [x = 1, y = 2] = [];  // valeurs par défaut : x=1, y=2
let p = 1, q = 2; [p, q] = [q, p]; // swap sans variable temporaire

// Objets (+ renommage + défauts + imbriqué)
const { hostname: nom, ip = "0.0.0.0", reseau: { masque } = {} } = routeur;

// Paramètres de fonction
function afficher({ hostname, ip }) { console.log(hostname, ip); }

// Spread : étaler
const t1 = [1, 2], t2 = [3, 4];
const t3 = [...t1, ...t2];          // [1,2,3,4]
const o2 = { ...routeur };          // copie superficielle
Math.max(...[4, 9, 2]);             // 9 (spread en appel)

// Rest : regrouper (inverse du spread)
const { hostname: h, ...autres } = routeur; // autres = tout sauf hostname
```

> **Attention** : spread/destructuration = copie **superficielle** (les objets imbriqués restent partagés).

---

## 13. Chaînes de caractères

```js
const log = "  ERR  connexion timeout sur eth0  ";

// Nettoyage / inspection
log.trim();                 // "ERR  connexion timeout sur eth0"
log.toLowerCase();          // minuscules
log.includes("timeout");    // true
log.startsWith("  ERR");    // true
log.endsWith("eth0  ");     // true
log.indexOf("timeout");     // position ou -1

// Extraction
log.slice(2, 5);            // "ERR" (indices négatifs OK)
log.substring(2, 5);        // "ERR" (pas de négatifs)
"eth0".padStart(8, "0");    // "0000eth0"
"42".padEnd(5, ".");        // "42..."

// Remplacement
"aaa".replace("a", "b");    // "baa" (première occurrence)
"aaa".replaceAll("a", "b"); // "bbb" (ES2021)
"ERR 12".replace(/(\d+)/, "[$1]"); // "ERR [12]" (regex + groupe)

// Découpage / assemblage
"a,b,c".split(",");         // ["a","b","c"]
["a","b"].join("-");        // "a-b"

// Template literals : multiligne + interpolation + taggés
const lignes = `
  ligne1
  ligne2 ${2 + 2}
`;
```

### Cas sysadmin : normaliser des sorties

```js
function parseLigneDf(ligne) {
  // "/dev/sda1       100G   60G   40G  60% /"
  const [fs, taille, used, dispo, pct, mount] = ligne.trim().split(/\s+/);
  return { fs, taille, used, dispo, usage: parseInt(pct), mount };
}
```

---

## 14. Nombres et Math

```js
// Littéraux
const a = 1_000_000;   // séparateur (ES2021) : lisibilité
const b = 0xFF;        // 255 hexadécimal
const c = 0b1010;      // 10 binaire
const d = 0o755;       // 493 octal

// Arrondis
Math.floor(4.9);  // 4    Math.ceil(4.1);   // 5
Math.round(4.5);  // 5    Math.trunc(-4.9); // -4 (ES2015, vers zéro)
(4.567).toFixed(2);      // "4.57" (chaîne !)
Number("4.57").toPrecision(3);

// Aléatoire
Math.floor(Math.random() * 100); // 0..99
// crypto.getRandomValues() pour du vrai aléatoire (tokens, voir section 50)

// Min/Max, puissance, racine
Math.max(3, 9, 1);  Math.min(...[3, 9, 1]);
Math.pow(2, 10);  2 ** 10;
Math.sqrt(144);   Math.cbrt(27);

// Vérifications
Number.isInteger(4);       // true
Number.isNaN("abc" / 1);   // true (ne coerce pas, contrairement au global isNaN)
Number.isFinite(1 / 0);    // false
Number.EPSILON;            // pour comparer des flottants
Math.abs(0.1 + 0.2 - 0.3) < Number.EPSILON; // true
```

---

## 15. Dates

```js
const maintenant = new Date();
maintenant.toISOString();      // "2026-09-26T23:34:27.000Z" (UTC, idéal logs)
maintenant.toLocaleString("fr-FR"); // "26/09/2026 23:34:27" (affichage)
maintenant.getTime();          // timestamp ms depuis 1970-01-01

// Construction : ATTENTION mois 0-indexé !
new Date(2026, 8, 26);         // 26 septembre 2026 (mois 8 = septembre !)
new Date("2026-09-26T10:00:00Z"); // ISO : recommandé
Date.now();                    // timestamp ms (le plus rapide)

// Calculs : travailler en ms, jamais en "jours" naïfs (DST !)
const UN_JOUR_MS = 24 * 60 * 60 * 1000;
const demain = new Date(Date.now() + UN_JOUR_MS);

// Formater proprement : Intl (ES2015+)
new Intl.DateTimeFormat("fr-FR", {
  dateStyle: "full", timeStyle: "short", timeZone: "Europe/Paris"
}).format(maintenant); // "samedi 26 septembre 2026 à 23:34"

// Durée entre deux dates
const debut = Date.now();
// ... traitement ...
const dureeMs = Date.now() - debut;

// Temporal (API moderne, en cours de standardisation — signalée, pas encore partout)
// const zdt = Temporal.Now.zonedDateTimeISO(); // futur remplaçant de Date
```

### Pièges Date

| Piège | Explication |
|---|---|
| Mois 0-indexé | `new Date(2026, 8, 1)` = septembre, pas août |
| `getDay()` vs `getDate()` | jour semaine (0=dimanche) vs jour du mois |
| Fuseaux | `new Date("2026-09-26")` = minuit **UTC** ; `new Date(2026,8,26)` = minuit **local** |
| Mutable | les setters (`setHours`...) modifient l'objet |

---

## 16. this — les 4 règles

`this` dépend du **site d'appel**, pas du site de définition. 4 règles, par ordre de priorité :

```js
// RÈGLE 1 — new : `this` = nouvel objet
function Serveur(host) { this.host = host; }
const s = new Serveur("10.0.0.1"); // this → s

// RÈGLE 2 — appel méthode : `this` = objet avant le point
const obj = {
  nom: "R1",
  qui() { return this.nom; }
};
obj.qui(); // "R1" — this → obj

// RÈGLE 3 — appel explicite : call / apply / bind
function presenter(role) { return `${this.nom} (${role})`; }
presenter.call({ nom: "Zelef" }, "admin");   // "Zelef (admin)"
presenter.apply({ nom: "Zelef" }, ["admin"]); // idem, args en tableau
const f = presenter.bind({ nom: "Zelef" }); // this figé
f("admin");

// RÈGLE 4 — défaut : undefined (strict) ou globalThis (sloppy)
function seule() { return this; }
seule(); // undefined en module/strict ; globalThis en sloppy
```

### Fonctions fléchées : PAS de `this` propre

```js
const equipe = {
  nom: "Systèmes",
  membres: ["a", "b"],
  lister() {
    // `this` fléchée = celui de lister() → equipe. Parfait pour les callbacks !
    this.membres.forEach(m => console.log(`${this.nom} : ${m}`));
  },
  // MAUVAIS : méthode fléchée → this = portée englobante, pas l'objet
  // mauvais: () => this.nom
};
```

### Cas concret : perdre `this` dans un callback

```js
class Sonde {
  constructor() { this.valeur = 0; }
  demarrer() {
    // setInterval(function() { this.valeur++; }, 1000); // BUG : this = timeout/global
    setInterval(() => { this.valeur++; }, 1000); // OK : fléchée hérite du this
    // ou : setInterval(function() { this.valeur++; }.bind(this), 1000);
  }
}
```

> **Pense-bête** : dans une méthode, callback → fléchée. Pour fixer un `this` → `bind`.

---

## 17. Prototypes

Avant les classes (ES2015), JS utilisait les prototypes. Les classes ne sont que du sucre syntaxique par-dessus — comprendre le prototype explique 80 % des bizarreries.

```js
function Animal(nom) { this.nom = nom; }
// Méthode sur le PROTOTYPE : partagée par toutes les instances (1 seule copie en mémoire)
Animal.prototype.parler = function() { return `${this.nom} fait un bruit`; };

const rex = new Animal("Rex");
rex.parler(); // "Rex fait un bruit"

// Chaîne de prototype : rex → Animal.prototype → Object.prototype → null
Object.getPrototypeOf(rex) === Animal.prototype; // true
rex.hasOwnProperty("nom");        // true (propre)
rex.hasOwnProperty("parler");     // false (hérité du prototype)

// __proto__ : accesseur historique (préférer Object.getPrototypeOf / Object.create)
const chien = Object.create(Animal.prototype);
Animal.call(chien, "Médor"); // simule le constructeur
```

### Pourquoi c'est important

- `Array.prototype.map`, `String.prototype.trim`… : toutes les méthodes natives vivent sur des prototypes.
- Ajouter une méthode au prototype = disponible sur toutes les instances existantes.
- **Ne jamais** modifier `Object.prototype` (pollution globale) ni les prototypes natifs en lib partagée.

---

## 18. Classes

```js
class Equipement {
  // Champ public (ES2022)
  statut = "inconnu";
  // Champ privé (ES2022) : inaccessible de l'extérieur
  #serie = "SN-" + Math.random().toString(36).slice(2, 8).toUpperCase();

  constructor(hostname, ip) {
    this.hostname = hostname;
    this.ip = ip;
  }

  // Méthode d'instance (sur le prototype)
  ping() { return `ping ${this.ip}...`; }

  // Getter / setter
  get fiche() { return `${this.hostname} [${this.ip}]`; }
  set fiche(v) { [this.hostname, this.ip] = v.split("|"); }

  // Méthode statique (sur la classe, pas les instances)
  static type() { return "équipement réseau"; }

  // Méthode privée
  #audit() { return this.#serie; }
  serieMasquee() { return this.#audit().slice(0, 5) + "****"; }
}

const sw1 = new Equipement("SW1", "10.0.0.2");
sw1.ping();            // "ping 10.0.0.2..."
sw1.fiche;             // "SW1 [10.0.0.2]" (getter, sans parenthèses)
Equipement.type();     // "équipement réseau"
// sw1.#serie;         // SyntaxError : privé
```

### Tableau : champs et méthodes

| Syntaxe | Accès | Exemple |
|---|---|---|
| `champ = v` | public, par instance | `statut = "ok"` |
| `#champ` | privé (classe uniquement) | `#serie` |
| `static champ` | sur la classe | `static VERSION = 2` |
| `methode()` | prototype → instances | `ping()` |
| `static methode()` | classe uniquement | `type()` |
| `get` / `set` | propriété calculée | `get fiche()` |

---

## 19. Héritage et polymorphisme

```js
class Equipement {  // voir section 18
  constructor(hostname, ip) { this.hostname = hostname; this.ip = ip; }
  decrire() { return `${this.hostname} (${this.ip})`; }
}

class Switch extends Equipement {
  constructor(hostname, ip, nbPorts) {
    super(hostname, ip);   // OBLIGATOIRE avant d'utiliser this
    this.nbPorts = nbPorts;
  }
  // Surcharge (override) : polymorphisme
  decrire() { return `${super.decrire()} — ${this.nbPorts} ports`; }
}

class Routeur extends Equipement {
  constructor(hostname, ip, as) { super(hostname, ip); this.as = as; }
  decrire() { return `${super.decrire()} — AS${this.as}`; }
}

const parc = [new Switch("SW1", "10.0.0.2", 48), new Routeur("R1", "10.0.0.1", 64512)];
for (const e of parc) console.log(e.decrire()); // polymorphisme : chaque classe répond à sa façon
parc[0] instanceof Switch;      // true
parc[0] instanceof Equipement;  // true (chaîne d'héritage)
```

### Composition > héritage (principe)

```js
// Plutôt que d'hériter, composer : plus souple
const Supervisable = (obj) => ({
  ...obj,
  superviser() { return `supervision de ${this.hostname}`; }
});
const sw = Supervisable(new Switch("SW1", "10.0.0.2", 48));
```

---

## 20. Gestion d'erreurs

```js
// Lancer
function diviser(a, b) {
  if (b === 0) throw new RangeError("division par zéro");
  return a / b;
}

// Capturer : try / catch / finally
let resultat;
try {
  resultat = diviser(10, 0);
} catch (err) {
  console.error(`${err.name}: ${err.message}`); // "RangeError: division par zéro"
  // err.stack : pile d'appels (debug)
  resultat = Infinity;
} finally {
  console.log("nettoyage : toujours exécuté");
}
```

### Types d'erreurs natives

| Classe | Quand |
|---|---|
| `Error` | générique |
| `SyntaxError` | code invalide (compilation) |
| `ReferenceError` | variable inexistante / TDZ |
| `TypeError` | mauvais type (`null.foo()`, réassigner `const`) |
| `RangeError` | valeur hors limites |
| `URIError` | `encodeURI` invalide |
| `AggregateError` (ES2021) | plusieurs erreurs (`Promise.any`) |

### Erreurs métier personnalisées

```js
class HttpError extends Error {
  constructor(status, message) {
    super(message);
    this.name = "HttpError";
    this.status = status;
  }
}
// catch (e) { if (e instanceof HttpError && e.status === 404) {...} }
```

### Bonnes pratiques

- Capturer là où on peut **agir** (retry, fallback, message utilisateur) ; sinon laisser remonter.
- En async : `try/catch` autour de `await`, ou `.catch()` sur la promise.
- Ne jamais `catch {}` vide silencieux — au minimum logger.
- `finally` pour libérer les ressources (fermer fichier, connexion).
- Node : écouter `process.on("uncaughtException")` et `unhandledRejection` (section 30).

---

## 21. Event loop : call stack, micro/macro-tâches

JavaScript est **mono-thread** : une seule pile d'exécution (call stack). L'event loop orchestre l'asynchrone.

```
┌─────────────────────────────┐
│         CALL STACK          │  ← exécution synchrone (LIFO)
└──────────────┬──────────────┘
               │ vide ?
               ▼
┌─────────────────────────────┐
│   MICROTÂCHES (prioritaires)│  ← promises (.then), queueMicrotask, await
└──────────────┬──────────────┘
               │ vides ?
               ▼
┌─────────────────────────────┐
│   MACROTÂCHES               │  ← setTimeout, setInterval, I/O, setImmediate
└─────────────────────────────┘
        ↺ boucle infinie
```

```js
console.log("1. synchrone");

setTimeout(() => console.log("4. macrotâche (setTimeout)"), 0);

Promise.resolve().then(() => console.log("3. microtâche (promise)"));
queueMicrotask(() => console.log("3b. microtâche explicite"));

console.log("2. synchrone");
// Ordre : 1, 2, 3, 3b, 4
// Même avec un délai de 0, setTimeout passe APRÈS toutes les microtâches.
```

### Démonstration : les microtâches affament les macrotâches

```js
// DANGER : boucle de microtâches = event loop bloquée, timers jamais exécutés
function boucleInfinie() {
  Promise.resolve().then(boucleInfinie); // ne jamais faire ça
}
```

### Ce qui bloque / ne bloque pas

| Bloque l'event loop | Ne bloque pas |
|---|---|
| Boucle `for` de 10M d'itérations | `await fetch(...)` |
| `JSON.parse` d'un fichier de 500 Mo | `fs.readFile` (async) |
| `crypto.pbkdf2Sync` | `setTimeout` |
| Regex catastrophique | Workers threads |

> **Règle sysadmin** : en Node, jamais de synchrone lourd dans le chemin d'une requête/API. Déporter vers worker threads ou processus fils.

---

## 22. Callbacks et callback hell

```js
// Callback : fonction passée en argument, appelée "quand c'est prêt"
const fs = require("fs"); // CommonJS, voir section 28
fs.readFile("/etc/hosts", "utf8", (err, data) => {
  if (err) { console.error(err); return; } // convention error-first !
  console.log(data);
});
```

### Le callback hell (pyramide de la mort)

```js
// À NE PAS FAIRE — illisible, erreurs mal propagées
connexion(db, (err, conn) => {
  if (err) return gerer(err);
  requete(conn, "SELECT ...", (err, rows) => {
    if (err) return gerer(err);
    traiter(rows, (err, res) => {
      if (err) return gerer(err);
      envoyer(res, (err) => {
        if (err) return gerer(err);
        console.log("fini");
      });
    });
  });
});
```

### Sorties du callback hell (historique → moderne)

1. **Nommer les fonctions** au lieu d'imbriquer des anonymes.
2. **Promisifier** : `const { promisify } = require("util"); const readFile = promisify(fs.readFile);`
3. **Promises / async-await** (sections 23-24) : la solution moderne.

```js
// Promisification manuelle (à connaître pour les vieilles APIs)
function delai(ms) {
  return new Promise((resolve) => setTimeout(() => resolve(ms), ms));
}
```

---

## 23. Promises en profondeur

Une Promise = valeur **future** avec 3 états : `pending` → `fulfilled` (valeur) ou `rejected` (raison). État **définitif** une fois résolu.

```js
const promesse = new Promise((resolve, reject) => {
  // exécuteur appelé IMMÉDIATEMENT (synchrone !)
  setTimeout(() => {
    Math.random() > 0.5 ? resolve("OK") : reject(new Error("KO"));
  }, 100);
});

promesse
  .then(valeur => { console.log(valeur); return valeur + "!"; }) // chaînage
  .then(v2 => console.log(v2))
  .catch(err => console.error("capturé :", err.message))  // attrape tout le chaînage
  .finally(() => console.log("toujours exécuté"));
```

### Création rapide

```js
Promise.resolve(42);           // déjà fulfilled
Promise.reject(new Error("x"));// déjà rejected
// await 42;                   // await accepte aussi les non-promises
```

### Combinateurs

```js
const p1 = fetch("https://api1.example.com");
const p2 = fetch("https://api2.example.com");

// Tous doivent réussir — rejette dès le premier échec
await Promise.all([p1, p2]);

// Tous se terminent (succès ou échec) — idéal pour bilans
const resultats = await Promise.allSettled([p1, p2]);
// [{status:"fulfilled", value:...}, {status:"rejected", reason:...}]

// Le premier qui se termine (succès ou échec)
await Promise.race([p1, timeout(5000)]);

// Le premier SUCCÈS — rejette seulement si TOUS échouent (AggregateError, ES2021)
await Promise.any([miroir1, miroir2, miroir3]);
```

### Timeout sur une promise (pattern indispensable)

```js
function avecTimeout(promise, ms, message = "timeout") {
  const t = new Promise((_, reject) =>
    setTimeout(() => reject(new Error(`${message} après ${ms}ms`)), ms));
  return Promise.race([promise, t]);
}
await avecTimeout(fetch(url), 5000);
```

### Retry avec backoff exponentiel (pattern sysadmin)

```js
async function retry(fn, { tentatives = 3, delaiMs = 1000, facteur = 2 } = {}) {
  let derniereErreur;
  for (let i = 1; i <= tentatives; i++) {
    try { return await fn(); }
    catch (err) {
      derniereErreur = err;
      if (i < tentatives) {
        const attente = delaiMs * facteur ** (i - 1);
        console.warn(`Tentative ${i}/${tentatives} échouée, nouvel essai dans ${attente}ms`);
        await new Promise(r => setTimeout(r, attente));
      }
    }
  }
  throw derniereErreur;
}
// await retry(() => fetch("https://api.instable.example.com/status"));
```

### Pièges promises

- Oublier `return` dans un `.then` → le chaînage reçoit `undefined`.
- `new Promise` sans `catch` final → `unhandledRejection` (crash possible en Node).
- L'exécuteur est **synchrone** : `new Promise(() => { throw ... })` rejette immédiatement.
- `.then` retourne toujours une nouvelle promise (chaînage infini possible).

---

## 24. async / await en profondeur

`async` = fonction qui retourne **toujours** une promise. `await` = pause jusqu'à résolution (sans bloquer le thread).

```js
async function recupererStatut(url) {
  try {
    const reponse = await fetch(url);          // pause ici
    if (!reponse.ok) throw new HttpError(reponse.status, reponse.statusText);
    const data = await reponse.json();         // pause ici
    return data;
  } catch (err) {
    console.error("échec :", err.message);
    throw err; // re-propagation si l'appelant doit gérer
  }
}
```

### Séquentiel vs parallèle

```js
// SÉQUENTIEL (lent : 3 × 1s = 3s) — seulement si dépendances entre appels
const a = await tache(1);
const b = await tache(2);

// PARALLÈLE (rapide : max(1s) = 1s) — indépendants
const [ra, rb] = await Promise.all([tache(1), tache(2)]);

// Parallèle avec limite de concurrence (pattern sysadmin : ne pas DoS la cible)
async function pool(taches, limite) {
  const resultats = [];
  const enCours = new Set();
  for (const t of taches) {
    const p = t().then(r => { enCours.delete(p); return r; });
    enCours.add(p); resultats.push(p);
    if (enCours.size >= limite) await Promise.race(enCours);
  }
  return Promise.all(resultats);
}
// await pool(urls.map(u => () => fetch(u)), 5); // 5 requêtes max en parallèle
```

### async dans les boucles — LE piège

```js
const urls = ["https://a.example.com", "https://b.example.com"];

// BUG : forEach n'attend pas — les promises partent en "fire and forget"
urls.forEach(async (u) => { await fetch(u); console.log("fini", u); });
console.log("ceci s'affiche AVANT les fetch");

// CORRECT séquentiel : for...of
for (const u of urls) { await fetch(u); }

// CORRECT parallèle : map + Promise.all
await Promise.all(urls.map(async (u) => { await fetch(u); }));
```

### Top-level await (ES2022, modules uniquement)

```js
// En module ESM : await hors fonction autorisé
const config = await fetch("/config.json").then(r => r.json());
```

### Fonction async = toujours une promise

```js
async function f() { return 42; }
f() instanceof Promise; // true
f().then(console.log);  // 42
```

---

## 25. Fetch : GET, POST, erreurs

`fetch` = API native (navigateur + Node 18+). Retourne une promise vers un objet `Response`.

```js
// GET simple
const res = await fetch("https://api.example.com/serveurs");
console.log(res.status); // 200
console.log(res.ok);     // true si 200-299
console.log(res.headers.get("content-type"));
const data = await res.json();  // ou .text(), .blob(), .arrayBuffer(), .formData()
```

### L'erreur n°1 : fetch ne rejette PAS sur 404/500

```js
// fetch ne rejette que sur échec RÉSEAU (DNS, connexion refusée, CORS...)
// Un 404 ou 500 donne une promise FULFILLED avec ok=false !
const r = await fetch("https://api.example.com/inexistant");
if (!r.ok) throw new HttpError(r.status, `HTTP ${r.status} sur ${r.url}`);
```

### POST JSON

```js
const reponse = await fetch("https://api.example.com/equipements", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "Authorization": "Bearer " + process.env.API_TOKEN, // jamais en dur !
  },
  body: JSON.stringify({ hostname: "SW9", ip: "10.0.0.9" }),
  signal: AbortSignal.timeout(8000), // timeout natif (Node 18+ / navigateurs récents)
});
if (!reponse.ok) throw new HttpError(reponse.status, await reponse.text());
const cree = await reponse.json();
```

### Annulation explicite

```js
const ctrl = new AbortController();
const timer = setTimeout(() => ctrl.abort(), 5000); // annule après 5s
try {
  const r = await fetch(url, { signal: ctrl.signal });
} catch (e) {
  if (e.name === "AbortError") console.log("requête annulée");
} finally { clearTimeout(timer); }
```

### Client robuste complet (GET + erreurs + timeout + retry)

```js
class HttpError extends Error {
  constructor(status, message) { super(message); this.name = "HttpError"; this.status = status; }
}

async function apiGet(url, { timeoutMs = 8000, retries = 2 } = {}) {
  let err;
  for (let i = 0; i <= retries; i++) {
    try {
      const res = await fetch(url, { signal: AbortSignal.timeout(timeoutMs) });
      if (!res.ok) throw new HttpError(res.status, `${res.status} ${res.statusText}`);
      return await res.json();
    } catch (e) {
      err = e;
      const rejouable = e.name === "AbortError" || e.name === "TypeError" ||
                        (e instanceof HttpError && e.status >= 500);
      if (!rejouable || i === retries) throw e;
      await new Promise(r => setTimeout(r, 1000 * 2 ** i)); // backoff
    }
  }
  throw err;
}
```

### Tableau des méthodes Response

| Méthode | Usage |
|---|---|
| `res.json()` | corps JSON → objet |
| `res.text()` | corps → chaîne |
| `res.blob()` | fichier binaire (navigateur) |
| `res.arrayBuffer()` | binaire brut |
| `res.formData()` | formulaire multipart |

---

## 26. JSON

```js
const obj = { hostname: "R1", ports: [22, 443], actif: true };

// Sérialiser
JSON.stringify(obj);                    // '{"hostname":"R1",...}'
JSON.stringify(obj, null, 2);            // indenté (lisible, logs/fichiers)
JSON.stringify(obj, ["hostname"]);       // whitelist de clés
JSON.stringify(obj, (k, v) => k === "mdp" ? "***" : v); // replacer : filtrer les secrets !

// Désérialiser — TOUJOURS dans try/catch (données externes = hostiles)
try {
  const data = JSON.parse(texteRecu);
} catch (e) {
  console.error("JSON invalide :", e.message);
}
```

### Limites de JSON

- Pas de `undefined`, fonctions, `Symbol` (ignorés ou `null` dans les tableaux).
- `Date` → chaîne ISO (re-parser à la main si besoin).
- `BigInt` → **TypeError** (`JSON.stringify(10n)` échoue !).
- Références circulaires → TypeError.
- `NaN`/`Infinity` → `null`.

```js
// Contourner BigInt
JSON.stringify({ id: 10n }, (k, v) => typeof v === "bigint" ? v.toString() : v);
```

---

## 27. Timers

```js
// Exécution différée
const id1 = setTimeout(() => console.log("une fois"), 1000);
clearTimeout(id1); // annuler

// Répétition
const id2 = setInterval(() => console.log("tick"), 5000);
clearInterval(id2);

// Immédiat (différé au prochain tour de boucle)
setImmediate(() => console.log("tour suivant")); // Node uniquement
queueMicrotask(() => console.log("microtâche")); // avant les timers !

// Timers promisifiés (Node)
const { setTimeout: attendre } = require("timers/promises");
await attendre(1000); // await sleep(1000)
```

### Dérive de setInterval

`setInterval` ne garantit pas l'heure exacte (dérive si le callback est long). Pour des sondes régulières, préférer le `setTimeout` récursif :

```js
function sonde() {
  const debut = Date.now();
  // ... mesure ...
  const duree = Date.now() - debut;
  setTimeout(sonde, Math.max(0, 5000 - duree)); // compense la durée d'exécution
}
sonde();
```

### Node : ne pas bloquer la sortie

```js
const t = setInterval(collecte, 60000);
t.unref(); // le processus peut se terminer même si le timer est actif
```

---

## 28. Modules ESM vs CommonJS

Deux systèmes coexistent. Node historiquement = CommonJS ; le standard moderne = ESM.

### CommonJS (CJS) — `require` / `module.exports`

```js
// maths.cjs
function somme(a, b) { return a + b; }
const PI = 3.14159;
module.exports = { somme, PI };
// ou : exports.somme = somme;

// app.cjs
const { somme, PI } = require("./maths.cjs");
const path = require("path"); // modules natifs Node
```

Caractéristiques : **synchrone**, `require` appelable partout (même conditionnel), `__dirname`/`__filename` disponibles, extension `.cjs` (ou `.js` sans `"type": "module"`).

### ESM (ES2015+) — `import` / `export`

```js
// maths.mjs
export function somme(a, b) { return a + b; }
export const PI = 3.14159;
export default class Calculatrice { /* ... */ }

// app.mjs
import { somme, PI } from "./maths.mjs";       // import nommé (extension OBLIGATOIRE en Node)
import Calculatrice from "./maths.mjs";        // import défaut
import * as maths from "./maths.mjs";          // namespace
const { somme: s } = await import("./maths.mjs"); // import dynamique (conditionnel !)
```

Caractéristiques : **asynchrone**, imports statiques en tête (tree-shaking), strict mode implicite, pas de `__dirname` (utiliser `import.meta.url`), top-level await.

### Tableau comparatif

|  | CommonJS | ESM |
|---|---|---|
| Syntaxe | `require` / `module.exports` | `import` / `export` |
| Chargement | synchrone | asynchrone |
| Strict mode | non (sauf directive) | toujours |
| Tree-shaking | non | oui |
| `__dirname` | oui | non (`import.meta.url` + `fileURLToPath`) |
| Top-level await | non | oui |
| Recommandé pour | legacy, scripts rapides | **tout nouveau code** |

### Choisir dans Node

```json
// package.json
{ "type": "module" }   // .js = ESM (recommandé pour les nouveaux projets)
```

```js
// Équivalent __dirname en ESM
import { fileURLToPath } from "url";
import { dirname, join } from "path";
const __filename = fileURLToPath(import.meta.url);
const __dirname = dirname(__filename);
```

### Interopérabilité

- ESM peut importer du CJS : `import pkg from "vieux-package-cjs"`.
- CJS peut importer de l'ESM **uniquement** via `await import()` dynamique (Node 22 : `require(esm)` encore expérimental/limité).
- Règle pratique : publier/écrire en ESM ; si une dépendance n'existe qu'en CJS, l'importer en défaut.

---

## 29. npm et package.json

```bash
npm init -y                 # crée package.json avec défauts
npm install express         # dépendance de prod (+ package-lock.json)
npm install -D nodemon      # dépendance de dev
npm install -g pm2          # installation globale (CLI système)
npm uninstall lodash
npm update                  # met à jour selon les plages semver
npm outdated                # quoi mettre à jour
npm audit                   # vulnérabilités
npm audit fix
npx cowsay salut            # exécute un paquet SANS l'installer
```

### package.json — l'essentiel

```json
{
  "name": "outils-reseau",
  "version": "1.0.0",
  "type": "module",
  "description": "Scripts d'exploitation réseau",
  "main": "index.js",
  "bin": { "scan-reseau": "./bin/scan.js" },
  "scripts": {
    "start": "node src/index.js",
    "dev": "node --watch src/index.js",
    "test": "node --test",
    "lint": "eslint .",
    "inventaire": "node scripts/inventaire.js --sous-reseau 10.0.0.0/24"
  },
  "engines": { "node": ">=20" },
  "dependencies": { "commander": "^12.0.0" },
  "devDependencies": { "eslint": "^9.0.0" }
}
```

### Semver : `^` vs `~`

| Plage | Signification | `1.2.3` accepte |
|---|---|---|
| `^1.2.3` | compatible mineur | `1.x.x` (pas `2.0.0`) |
| `~1.2.3` | compatible patch | `1.2.x` (pas `1.3.0`) |
| `1.2.3` | exact | `1.2.3` uniquement |

### Scripts npm (le "Makefile" du JS)

```bash
npm run inventaire        # lance le script
npm start / npm test      # raccourcis (run implicite)
```

Astuce : chaîner avec `&&`, passer des args avec `--` : `npm run scan -- --rapide`.

### Bonnes pratiques npm

- Commiter `package.json` **et** `package-lock.json` (reproductibilité).
- Ne jamais commiter `node_modules/` (`.gitignore`).
- `npm ci` en CI/prod (installe exactement le lockfile, plus rapide et sûr que `install`).
- Fixer `engines.node` pour éviter les surprises de version.

---

## 30. Node.js — process, env, CLI

```js
// process : infos et contrôle du processus
process.version;          // "v22.x.x"
process.platform;         // "linux" | "win32" | "darwin"
process.arch;             // "x64", "arm64"
process.pid;              // PID
process.cwd();            // répertoire de travail
process.uptime();         // secondes depuis le démarrage
process.memoryUsage();    // { rss, heapTotal, heapUsed, ... }
process.exit(0);          // 0 = OK, 1+ = erreur (vide les stdout avant en sync)

// Variables d'environnement — JAMAIS de secrets en dur !
const token = process.env.API_TOKEN;
if (!token) { console.error("API_TOKEN manquant"); process.exit(1); }

// Arguments CLI : process.argv
// node scan.js --sous-reseau 10.0.0.0/24 --rapide
// argv = ["/usr/bin/node", "/chemin/scan.js", "--sous-reseau", "10.0.0.0/24", "--rapide"]
const args = process.argv.slice(2);

// stdin / stdout / stderr (streams)
process.stdout.write("progression...\r");
process.stdin.on("data", (d) => console.log("reçu :", d.toString().trim()));
```

### Parser d'arguments minimal (sans dépendance)

```js
function parseArgs(argv = process.argv.slice(2)) {
  const opts = { _: [] };
  for (let i = 0; i < argv.length; i++) {
    const a = argv[i];
    if (a.startsWith("--")) {
      const [k, v] = a.slice(2).split("=");
      opts[k] = v ?? argv[++i] ?? true;
    } else if (a.startsWith("-") && a.length === 2) {
      opts[a[1]] = argv[++i] ?? true;
    } else opts._.push(a);
  }
  return opts;
}
// node scan.js --sous-reseau 10.0.0.0/24 -v scan
// → { _: ["scan"], "sous-reseau": "10.0.0.0/24", v: true }
```

> Pour un vrai CLI : paquet `commander` (robuste, help auto).

### Signaux et erreurs globales

```js
// Arrêt propre sur Ctrl+C
process.on("SIGINT", () => { console.log("\nArrêt..."); server.close(() => process.exit(0)); });
process.on("SIGTERM", () => process.exit(0)); // systemd/docker

// Filet de sécurité (logguer, PAS masquer les bugs)
process.on("uncaughtException", (err) => { console.error("FATAL:", err); process.exit(1); });
process.on("unhandledRejection", (raison) => { console.error("Promise non gérée:", raison); process.exit(1); });
```

### Variables d'environnement en pratique

```bash
# .env (jamais commité !) — chargé via paquet `dotenv` ou --env-file (Node 20.6+)
API_TOKEN=xxx
PORT=3000
```

```bash
node --env-file=.env app.js   # Node 20.6+ : natif, sans dépendance
```

---

## 31. Node.js — fs et path

```js
import { readFile, writeFile, appendFile, mkdir, readdir, stat, rm } from "fs/promises";
import { join, resolve, dirname, basename, extname } from "path";
import { existsSync, createReadStream } from "fs";

// Lire / écrire (promises — à privilégier)
const contenu = await readFile("/etc/hosts", "utf8");
await writeFile("/tmp/rapport.txt", contenu);
await appendFile("/var/log/app.log", `[${new Date().toISOString()}] démarrage\n`);
await mkdir("/tmp/a/b", { recursive: true });

// Lister et filtrer
const fichiers = await readdir("/var/log", { withFileTypes: true });
const logs = fichiers.filter(f => f.isFile() && f.name.endsWith(".log")).map(f => f.name);

// Infos fichier
const infos = await stat("/var/log/syslog");
infos.size; infos.mtime; infos.isFile(); infos.isDirectory();

// path : construire des chemins portables (jamais de concaténation à la main !)
join("/var", "log", "app.log");       // "/var/log/app.log"
resolve("relatif", "fichier.txt");    // absolu depuis cwd
basename("/var/log/app.log");         // "app.log"
extname("archive.tar.gz");            // ".gz"
dirname("/var/log/app.log");          // "/var/log"

// Supprimer (moderne)
await rm("/tmp/vieux", { recursive: true, force: true });
```

### Gros fichiers : streams (ne jamais tout charger en RAM)

```js
import { createReadStream } from "fs";
import readline from "readline";

// Compter les lignes d'un log de 2 Go sans exploser la RAM
async function compterLignes(chemin) {
  let n = 0;
  const rl = readline.createInterface({ input: createReadStream(chemin), crlfDelay: Infinity });
  for await (const ligne of rl) { n++; } // for await...of sur un stream (section 42)
  return n;
}
```

### Sync vs Async

|  | Usage |
|---|---|
| `fs/promises` (`await readFile`) | **par défaut** — ne bloque pas l'event loop |
| `fs.readFileSync` | init au démarrage, scripts jetables uniquement |
| streams | gros fichiers, temps réel |

> **Jamais** de `*Sync` dans un serveur ou une boucle chaude.

---

## 32. Node.js — http

```js
import http from "http";

const server = http.createServer(async (req, res) => {
  const url = new URL(req.url, `http://${req.headers.host}`);
  console.log(`${req.method} ${url.pathname}`);

  if (url.pathname === "/sante" && req.method === "GET") {
    res.writeHead(200, { "Content-Type": "application/json" });
    res.end(JSON.stringify({ statut: "ok", uptime: process.uptime() }));
    return;
  }

  if (url.pathname === "/echo" && req.method === "POST") {
    let corps = "";
    for await (const chunk of req) corps += chunk; // lire le body en stream
    res.writeHead(200, { "Content-Type": "application/json" });
    res.end(JSON.stringify({ recu: JSON.parse(corps) }));
    return;
  }

  res.writeHead(404, { "Content-Type": "application/json" });
  res.end(JSON.stringify({ erreur: "introuvable" }));
});

const PORT = process.env.PORT || 3000;
server.listen(PORT, () => console.log(`Écoute sur http://localhost:${PORT}`));
```

### Client HTTP natif (alternative à fetch)

```js
// fetch suffit dans 95 % des cas (Node 18+). http.request pour le contrôle fin :
import https from "https";
function getJson(url) {
  return new Promise((resolve, reject) => {
    https.get(url, { timeout: 5000 }, (res) => {
      let data = "";
      res.on("data", c => data += c);
      res.on("end", () => res.statusCode === 200 ? resolve(JSON.parse(data)) : reject(new Error(`HTTP ${res.statusCode}`)));
    }).on("error", reject).on("timeout", function() { this.destroy(new Error("timeout")); });
  });
}
```

> En pratique : **fetch** pour les clients, **Express/Fastify** pour les vraies APIs (routage, middlewares).

---

## 33. Node.js — child_process

Exécuter des commandes système depuis Node : le pont sysadmin.

```js
import { exec, execFile, spawn } from "child_process";
import { promisify } from "util";
const execAsync = promisify(exec);

// 1. exec : commande shell complète (simple, mais RISQUE D'INJECTION)
const { stdout } = await execAsync("uptime");
console.log(stdout.trim());

// 2. execFile : binaire + args SANS shell — SÉCURISÉ, à préférer
const execFileAsync = promisify(execFile);
const { stdout: ip } = await execFileAsync("ip", ["-brief", "addr"]);

// 3. spawn : streaming temps réel (ping, tail -f, builds)
const ping = spawn("ping", ["-c", "4", "8.8.8.8"]);
ping.stdout.on("data", d => process.stdout.write(`[ping] ${d}`));
ping.stderr.on("data", d => process.stderr.write(`[ping:err] ${d}`));
const code = await new Promise(res => ping.on("close", res));
console.log("code de sortie :", code);
```

### Tableau : quelle fonction choisir

| Fonction | Shell ? | Streaming | Usage |
|---|---|---|---|
| `exec` | oui | non (buffer) | commande simple, sortie petite |
| `execFile` | non | non (buffer) | **sécurisé**, args dynamiques |
| `spawn` | non (opt `shell:true`) | oui | sortie longue / temps réel |
| `fork` | non | IPC | sous-processus Node (workers) |

### SÉCURITÉ : injection de commande

```js
// DANGEREUX si `hote` vient de l'utilisateur :
await execAsync(`ping -c 1 ${hote}`); // hote = "8.8.8.8; rm -rf /" → catastrophe

// SÛR : execFile sans shell, args séparés
await execFileAsync("ping", ["-c", "1", hote]); // ";" traité comme simple argument
```

### Timeout et kill

```js
const proc = spawn("sleep", ["60"], { timeout: 5000 }); // tué après 5s
proc.on("error", e => console.error(e.message));
// ou manuel : proc.kill("SIGTERM");
```

---

## 34. Node.js — events et streams

### EventEmitter : le cœur de Node

```js
import { EventEmitter } from "events";

class Superviseur extends EventEmitter {}
const sup = new Superviseur();

sup.on("panne", (equipement) => console.log(`ALERTE: ${equipement} en panne`));
sup.once("demarrage", () => console.log("premier démarrage")); // une seule fois
sup.emit("panne", "SW1");
sup.emit("panne", "SW1"); // les .on persistent, les .once non

sup.off("panne", handler);      // désabonner (référence de fonction requise)
sup.removeAllListeners("panne");
```

### Streams : traiter les données par morceaux

Quatre types : **Readable** (lecture), **Writable** (écriture), **Duplex**, **Transform**.

```js
import { createReadStream, createWriteStream } from "fs";
import { Transform, pipeline } from "stream/promises";
import { createGzip } from "zlib";

// Compresser un gros log SANS le charger en RAM
const filtreErreurs = new Transform({
  transform(chunk, enc, cb) {
    const lignes = chunk.toString().split("\n").filter(l => /error|crit/i.test(l));
    cb(null, lignes.join("\n") + "\n");
  }
});

await pipeline(
  createReadStream("/var/log/syslog"), // Readable
  filtreErreurs,                        // Transform
  createGzip(),                         // Transform
  createWriteStream("/tmp/erreurs.log.gz") // Writable
);
console.log("pipeline terminé");
// pipeline() gère les erreurs et ferme les streams — TOUJOURS préférer à .pipe()
```

---

## 35. Manipulation du DOM

Le DOM = représentation objet de la page HTML. `document` = point d'entrée.

```js
// Sélection
const el = document.getElementById("statut");      // par id (unique)
const btns = document.querySelectorAll(".btn");    // tous les .btn (NodeList)
const premier = document.querySelector("#menu a"); // premier match (sélecteur CSS)

// Contenu
el.textContent = "En ligne";        // texte brut (SÛR contre XSS)
el.innerHTML = "<b>En ligne</b>";   // HTML interprété (DANGEREUX si données externes !)
el.innerHTML = "";                  // vider

// Attributs / classes / styles
el.setAttribute("data-ip", "10.0.0.1");
el.getAttribute("data-ip");
el.classList.add("ok"); el.classList.remove("ko"); el.classList.toggle("actif");
el.style.color = "green"; // style inline (préférer les classes)

// Créer / insérer
const li = document.createElement("li");
li.textContent = "SW1 — 10.0.0.2";
document.getElementById("liste").appendChild(li);
// Moderne : append(), prepend(), before(), after(), remove()
li.remove();

// Parcourir
el.parentElement; el.children; el.closest(".carte"); // ancêtre le plus proche
```

### NodeList vs Array

```js
const items = document.querySelectorAll("li"); // NodeList (itérable, .forEach OK)
const tableau = [...items];                    // spread → vrai tableau (map/filter...)
```

### Performance DOM

- Regrouper les insertions via `DocumentFragment` (un seul reflow).
- Éviter `innerHTML +=` en boucle (re-parse tout à chaque fois).
- Mettre en cache les sélections hors des boucles.

```js
const frag = document.createDocumentFragment();
for (const srv of serveurs) {
  const li = document.createElement("li");
  li.textContent = `${srv.nom} — ${srv.ip}`;
  frag.appendChild(li);
}
document.getElementById("liste").appendChild(frag); // un seul reflow
```

---

## 36. Événements et délégation

```js
const btn = document.getElementById("btn-scan");

// addEventListener : la méthode moderne (plusieurs handlers, options)
btn.addEventListener("click", (e) => {
  console.log("cliqué !", e.target, e.clientX, e.clientY);
}, { once: true }); // once : exécuté une seule fois

btn.removeEventListener("click", handler); // nécessite la référence

// Empêcher le comportement par défaut / la propagation
form.addEventListener("submit", (e) => {
  e.preventDefault();   // ne pas recharger la page
  // ... validation + fetch ...
});
```

### Propagation : capture → cible → bubbling

```js
// Par défaut les handlers s'exécutent au BUBBLING (remontée vers document)
document.getElementById("externe").addEventListener("click", () => console.log("externe"));
document.getElementById("interne").addEventListener("click", (e) => {
  console.log("interne");
  e.stopPropagation(); // stoppe la remontée
});
```

### Délégation d'événements (pattern clé)

Un seul listener sur le parent gère tous les enfants, **même ajoutés dynamiquement** :

```js
document.getElementById("liste-serveurs").addEventListener("click", (e) => {
  const bouton = e.target.closest("button[data-action]");
  if (!bouton) return; // clic hors bouton : ignorer
  const ligne = bouton.closest("tr");
  const ip = ligne.dataset.ip;
  if (bouton.dataset.action === "ping") pinger(ip);
  if (bouton.dataset.action === "supprimer") ligne.remove();
});
```

> Sans délégation : N listeners (fuites mémoire, rien sur les éléments futurs). Avec : 1 seul.

### Événements utiles

| Événement | Déclencheur |
|---|---|
| `click`, `dblclick` | souris |
| `input`, `change` | champ modifié (`input` = chaque frappe) |
| `submit` | formulaire |
| `keydown` / `keyup` | clavier (`e.key`) |
| `DOMContentLoaded` | DOM prêt (avant images) |
| `load` | page complète |

---

## 37. Formulaires

```js
const form = document.getElementById("form-equipement");

form.addEventListener("submit", async (e) => {
  e.preventDefault();

  // FormData : récupère tout le formulaire d'un coup
  const fd = new FormData(form);
  const equipement = Object.fromEntries(fd.entries());
  // { hostname: "SW9", ip: "10.0.0.9", site: "Paris" }

  // Validation native HTML5 : required, type="email", pattern, min/max...
  if (!form.checkValidity()) { form.reportValidity(); return; }

  const res = await fetch("/api/equipements", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(equipement),
  });
  if (!res.ok) { /* afficher l'erreur */ return; }
  form.reset(); // vider
});
```

### Lire les champs individuellement

```js
const ip = document.getElementById("ip").value.trim();
const actif = document.getElementById("actif").checked; // checkbox
const site = document.querySelector('input[name="site"]:checked')?.value; // radio
```

### Validation custom

```js
const champIp = document.getElementById("ip");
champIp.addEventListener("input", () => {
  const ok = /^(?:\d{1,3}\.){3}\d{1,3}$/.test(champIp.value);
  champIp.setCustomValidity(ok ? "" : "IPv4 invalide (ex : 10.0.0.1)");
});
```

---

## 38. Stockage navigateur : localStorage, cookies

```js
// localStorage : ~5 Mo, persistant, SYNCHRONE (petites données uniquement)
localStorage.setItem("theme", "sombre");
localStorage.getItem("theme");            // "sombre" (null si absent)
localStorage.removeItem("theme");

// Stocker un objet → JSON
const prefs = { theme: "sombre", refresh: 30 };
localStorage.setItem("prefs", JSON.stringify(prefs));
const chargees = JSON.parse(localStorage.getItem("prefs") ?? "{}");

// sessionStorage : idem mais vidé à la fermeture de l'onglet
```

### Tableau des stockages

|  | localStorage | sessionStorage | Cookies |
|---|---|---|---|
| Capacité | ~5 Mo | ~5 Mo | ~4 Ko |
| Durée | persistant | onglet | selon `Expires`/`Max-Age` |
| Envoyé au serveur | non | non | **oui** (chaque requête) |
| Usage | préférences UI, cache | état temporaire | sessions (HttpOnly !) |

### Cookies côté JS

```js
document.cookie = "theme=sombre; Max-Age=31536000; Path=/; SameSite=Lax";
// Lire : parser document.cookie ("a=1; b=2")
const cookies = Object.fromEntries(document.cookie.split("; ").map(c => c.split("=")));
```

> Cookies de session/auth : `HttpOnly` + `Secure` + `SameSite` → **inaccessibles en JS** (protection XSS). Les poser côté serveur.

---

## 39. Web APIs utiles

```js
// Presse-papiers
await navigator.clipboard.writeText("10.0.0.1");
const texte = await navigator.clipboard.readText();

// Géolocalisation (avec permission utilisateur)
navigator.geolocation.getCurrentPosition(
  (pos) => console.log(pos.coords.latitude, pos.coords.longitude),
  (err) => console.error(err.message)
);

// Notifications (avec permission)
if (await Notification.requestPermission() === "granted") {
  new Notification("Supervision", { body: "SW1 ne répond plus" });
}

// Fetch + AbortController : voir section 25
// WebSocket : temps réel
const ws = new WebSocket("wss://supervision.example.com/live");
ws.onmessage = (e) => afficher(JSON.parse(e.data));
ws.send(JSON.stringify({ action: "abonner", cible: "SW1" }));

// IntersectionObserver : lazy-load / infinite scroll
const obs = new IntersectionObserver((entrees) => {
  for (const e of entrees) if (e.isIntersecting) chargerPlus();
});
obs.observe(document.getElementById("sentinelle"));

// URL et URLSearchParams : parser/construire des URLs proprement
const u = new URL("https://api.example.com/equipements?site=paris&page=2");
u.searchParams.get("site");      // "paris"
u.searchParams.set("page", "3");
u.toString();
```

---

## 40. RegExp

```js
// Littéral /.../flags  ou  new RegExp("...", "flags")
const reIp = /^(?:\d{1,3}\.){3}\d{1,3}$/;
reIp.test("10.0.0.1");   // true (test rapide booléen)

"err: timeout sur eth0".match(/timeout sur (\w+)/); // ["timeout sur eth0", "eth0"] (groupe)
// matchAll (ES2020) : tous les matchs avec groupes
for (const m of "a1 b2".matchAll(/([a-z])(\d)/g)) console.log(m[1], m[2]);
```

### Flags

| Flag | Effet |
|---|---|
| `g` | global (tous les matchs) |
| `i` | insensible à la casse |
| `m` | `^`/`$` par ligne |
| `s` | `.` matche aussi `\n` (dotAll, ES2018) |
| `u` | unicode (ES2015) |
| `y` | sticky (ES2015) |

### Syntaxes essentielles

| Motif | Signifie |
|---|---|
| `\d` `\w` `\s` | chiffre, mot `[A-Za-z0-9_]`, espace |
| `\D` `\W` `\S` | inverses |
| `^` `$` | début / fin (de chaîne ou ligne avec `m`) |
| `*` `+` `?` `{2,4}` | 0+, 1+, 0/1, répétitions |
| `*?` `+?` | versions **non-gourmandes** (lazy) |
| `(...)` `(?:...)` | groupe capturant / non-capturant |
| `(?<nom>...)` | groupe nommé (ES2018) : `m.groups.nom` |
| `(?<=...)` `(?=...)` | lookbehind / lookahead (ES2018) |
| `[abc]` `[^abc]` | classe / négation |
| `a\|b` | alternative |
| `\.` `\*` `\\` | caractères spéciaux échappés |

### Recettes sysadmin

```js
const IPV4 = /^(?:(?:25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)\.){3}(?:25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)$/;
const MAC = /^([0-9A-Fa-f]{2}[:-]){5}([0-9A-Fa-f]{2})$/;
// Extraire les IP d'un texte
const ips = texte.match(/(?:\d{1,3}\.){3}\d{1,3}/g) ?? [];
// Parser une ligne de log Apache (combined)
const RE_APACHE = /^(\S+) \S+ \S+ \[([^\]]+)\] "(\S+) (\S+) \S+" (\d{3}) (\d+|-) "[^"]*" "([^"]*)"/;
```

### Piège : regex avec `g` et `.test()` en boucle

```js
const re = /a/g;
re.test("a"); // true  — lastIndex avance !
re.test("a"); // false — repart de lastIndex=1
// Solution : ne pas réutiliser une regex /g/ avec test(), ou reset re.lastIndex = 0
```

---

## 41. Map, Set, WeakMap, WeakSet

```js
// Map : clés de TOUT type (objets !), ordre d'insertion, .size
const inventaire = new Map();
inventaire.set("SW1", { ip: "10.0.0.2", ports: 48 });
inventaire.set("SW1", { ip: "10.0.0.3", ports: 24 }); // écrase (clé unique)
inventaire.get("SW1");       // { ip: "10.0.0.3", ports: 24 }
inventaire.has("R1");        // false
inventaire.delete("SW1");
inventaire.size;             // 0
for (const [nom, fiche] of inventaire) { /* ... */ }

// Set : valeurs UNIQUES
const ipsVues = new Set();
ipsVues.add("10.0.0.1"); ipsVues.add("10.0.0.1"); // doublon ignoré
ipsVues.has("10.0.0.1");     // true
[...new Set([1, 2, 2, 3])];   // [1, 2, 3] — dédupliquer un tableau !

// Opérations ensemblistes (ES2025)
const a = new Set([1, 2, 3]), b = new Set([2, 3, 4]);
a.union(b);            // Set {1,2,3,4}
a.intersection(b);     // Set {2,3}
a.difference(b);       // Set {1}

// WeakMap / WeakSet : clés OBJETS, références faibles (pas d'empêchement GC)
const cache = new WeakMap();
cache.set(objetEquipement, donneesCalculees); // libéré si objetEquipement est GC
// Pas de .size, pas d'itération — usage : caches, métadonnées privées
```

### Map vs Object

|  | `Map` | `Object` |
|---|---|---|
| Clés | tout type | string/symbol (coercition !) |
| Ordre | insertion garanti | quasi (entiers d'abord triés) |
| Taille | `.size` | `Object.keys(o).length` |
| Itération | directe | via `Object.entries` |
| Perf ajout/suppr fréquents | meilleure | — |
| JSON | non natif | natif |

> Clés dynamiques non-string, ajouts/suppressions fréquents → `Map`. Config/objet métier sérialisable → `Object`.

---

## 42. Itérateurs et générateurs

```js
// Tout objet avec [Symbol.iterator] est itérable (for...of, spread, ...)
const iterable = {
  *[Symbol.iterator]() { yield 1; yield 2; yield 3; }
};
[...iterable]; // [1, 2, 3]

// Générateur : fonction* — exécution PARESSEUSE, état interne
function* scanSousReseau(base) {
  for (let i = 1; i < 255; i++) {
    yield `${base}.${i}`; // pause ici, reprend au prochain next()
  }
}
const gen = scanSousReseau("10.0.0");
gen.next(); // { value: "10.0.0.1", done: false }

// Cas sysadmin : générer des IP sans allouer un tableau de 65000 entrées
function* plageIp(cidrBase, debut, fin) {
  for (let i = debut; i <= fin; i++) yield `${cidrBase}.${i}`;
}
for (const ip of plageIp("192.168.1", 1, 254)) {
  // await ping(ip); // traitement à la volée, mémoire constante
  if (ip === "192.168.1.10") break;
}

// Générateur async (for await...of)
async function* lireLignes(chemin) {
  const rl = readline.createInterface({ input: createReadStream(chemin) });
  for await (const ligne of rl) yield ligne;
}
```

---

## 43. Symbols et BigInt

```js
// Symbol : identifiant UNIQUE — clés "cachées", pas de collision
const CLE_PRIVEE = Symbol("cle");
const obj = { [CLE_PRIVEE]: "secret interne", public: "visible" };
Object.keys(obj);              // ["public"] — le symbol n'apparaît pas
obj[CLE_PRIVEE];               // "secret interne"

// Symboles connus (well-known) : customiser le comportement natif
class Plage {
  constructor(debut, fin) { this.debut = debut; this.fin = fin; }
  *[Symbol.iterator]() { for (let i = this.debut; i <= this.fin; i++) yield i; }
  get [Symbol.toStringTag]() { return "Plage"; }
}
[...new Plage(1, 3)]; // [1, 2, 3]

// BigInt : entiers au-delà de 2^53-1 (ES2020)
const compteur = 9007199254740993n;
typeof compteur;              // "bigint"
compteur + 1n;                // OK
// compteur + 1;              // TypeError : conversion explicite requise
Number(compteur);             // 9007199254740992 (perte de précision !)
// Usage : compteurs, timestamps ns, crypto, identifiants 64 bits
```

---

## 44. Proxy et Reflect

`Proxy` = intercepter les opérations sur un objet (validation, logs, valeurs par défaut).

```js
const config = new Proxy({ port: 3000 }, {
  get(cible, prop) {
    if (!(prop in cible)) console.warn(`Config inconnue : ${String(prop)}`);
    return cible[prop];
  },
  set(cible, prop, valeur) {
    if (prop === "port" && (valeur < 1 || valeur > 65535))
      throw new RangeError("port invalide");
    cible[prop] = valeur;
    return true;
  }
});
config.port = 99999; // RangeError
console.log(config.hote); // warn + undefined

// Reflect : mêmes opérations en version fonctionnelle
Reflect.get(config, "port");
Reflect.has(config, "port");
```

> Usage réel : validation de config, ORM, observabilité. À manier avec parcimonie (coût perf, debug plus dur).

---

## 45. Programmation fonctionnelle

```js
// Fonctions pures : même entrée → même sortie, pas d'effet de bord
const ajouterTva = (prix, taux = 0.2) => prix * (1 + taux); // pure ✓
// let total = 0; const ajouter = (p) => total += p; // impure ✗ (état externe)

// Immuabilité : créer plutôt que muter
const srv1 = { nom: "SW1", ports: 48 };
const srv2 = { ...srv1, ports: 24 }; // srv1 intact

// Composition : enchaîner des petites fonctions
const pipe = (...fns) => (x) => fns.reduce((v, f) => f(v), x);
const normaliser = pipe(
  s => s.trim(),
  s => s.toLowerCase(),
  s => s.replace(/\s+/g, "-")
);
normaliser("  SW 1 Core  "); // "sw-1-core"

// Curryfication : spécialiser progressivement
const requete = (base) => (chemin) => (params) =>
  fetch(`${base}${chemin}?${new URLSearchParams(params)}`);
const apiInterne = requete("https://api.interne.example.com");
const getEquipements = apiInterne("/equipements");
await getEquipements({ site: "paris" });
```

### Méthodes fonctionnelles de tableau (référence)

| Méthode | Retourne | Usage |
|---|---|---|
| `map` | tableau transformé | extraire un champ |
| `filter` | sous-ensemble | garder les erreurs |
| `reduce` | une valeur | somme, groupement, agrégation |
| `find` / `findIndex` | élément / index | premier match |
| `some` / `every` | booléen | au moins un / tous |
| `flatMap` | aplati | 1 → N |
| `sort` (avec comparateur) | trié (mutant !) | trier |

---

## 46. Tests unitaires

### Avec le runner natif Node (Node 18+, `--test`)

```js
// maths.js
export const somme = (a, b) => a + b;
export const diviser = (a, b) => {
  if (b === 0) throw new RangeError("division par zéro");
  return a / b;
};

// maths.test.js
import { describe, it } from "node:test";
import assert from "node:assert/strict";
import { somme, diviser } from "./maths.js";

describe("maths", () => {
  it("somme additionne", () => {
    assert.equal(somme(2, 3), 5);
  });
  it("diviser lève une erreur sur zéro", () => {
    assert.throws(() => diviser(1, 0), RangeError);
  });
  it("async supporté", async () => {
    const data = await Promise.resolve(42);
    assert.deepEqual(data, 42);
  });
});
```

```bash
node --test                 # lance tous les *.test.js
node --test --test-reporter=spec
```

```json
// package.json
{ "scripts": { "test": "node --test" } }
```

### Assertions essentielles (`node:assert/strict`)

| Assertion | Vérifie |
|---|---|
| `assert.equal(a, b)` | `==` (éviter) |
| `assert.strictEqual(a, b)` | `===` |
| `assert.deepStrictEqual(a, b)` | structure profonde |
| `assert.throws(fn, Erreur)` | lève bien l'erreur |
| `assert.rejects(promise, Erreur)` | promise rejetée |
| `assert.ok(valeur)` | truthy |
| `assert.match(str, regex)` | regex |

### Bonnes pratiques de test

- Nommage : `fichier.test.js` à côté du fichier.
- Tester les **comportements** (contrats), pas l'implémentation.
- Un test = un comportement ; nom explicite ("rejette les ports hors plage").
- Mocker les I/O (fs, réseau, temps) — un test unitaire ne touche ni disque ni réseau.
- Viser les cas limites : vide, `null`, zéro, bornes, erreurs.
- Alternatives : **Vitest** (rapide, moderne), **Jest** (écosystème historique).

---

## 47. Debug

### Console avancée

```js
console.log("valeur :", x);
console.log("objet : %o", objet);           // format objet
console.table(tableauObjets);               // tableau lisible
console.dir(objet, { depth: null });        // profondeur illimitée
console.trace("où suis-je ?");              // stack trace
console.count("appel");                     // compteur par label
console.time("requete"); /* ... */ console.timeEnd("requete");
console.group("section"); /* ... */ console.groupEnd();
```

### Debugger (le vrai outil)

```js
function calculer(x) {
  debugger; // point d'arrêt : pause ici si DevTools / --inspect attaché
  return x * 2;
}
```

```bash
node --inspect script.js        # DevTools Chrome : chrome://inspect
node --inspect-brk script.js    # pause dès le démarrage
```

Dans VS Code : F5 avec une config `launch.json` → breakpoints visuels, variables, pile d'appels, évaluation d'expressions.

### Stratégie de debug

1. **Reproduire** avec le cas minimal.
2. **Lire l'erreur** : `message` + 3 premières lignes de `stack`.
3. **Isoler** : `console.log`/`debugger` en dichotomie (moitié du code).
4. **Vérifier les hypothèses** : `typeof`, valeurs réelles vs supposées.
5. Pièges fréquents : async non attendu, `this` perdu, mutation surprise, cache.

### Erreurs Node courantes : décodage

| Message | Cause probable |
|---|---|
| `Cannot read properties of undefined (reading 'x')` | objet `undefined` (chaînage `?.` ou garde) |
| `x is not a function` | mauvaise import, écrasement, `this` perdu |
| `Unexpected token` | syntaxe invalide / JSON malformé |
| `Cannot find module './x'` | chemin/extension faux (ESM exige l'extension) |
| `require is not defined` | fichier ESM (`"type": "module"`) |
| `await is only valid in async` | `await` hors fonction async / hors module |
| `Maximum call stack size exceeded` | récursion infinie |
| `EADDRINUSE` | port déjà occupé |
| `ENOENT` | fichier/chemin inexistant |

---

## 48. Bonnes pratiques & style

### Les 12 règles d'or

1. `const` par défaut, `let` si réassigné, jamais `var`.
2. `===` toujours, jamais `==` (sauf `x == null` idiome, à connaître).
3. Fonctions courtes, un seul niveau d'abstraction, un seul "travail".
4. Noms explicites : `recupererEquipementsEnPanne()` > `getData2()`.
5. Retour anticipé (early return) plutôt qu'imbrications.
6. Ne jamais muter les arguments d'entrée.
7. Gérer les erreurs là où on peut agir ; logger avec contexte.
8. Async/await plutôt que `.then` imbriqués ; `Promise.all` pour le parallèle.
9. Valider les entrées externes (API, CLI, fichiers) — ne jamais faire confiance.
10. Pas de secrets en dur : variables d'environnement.
11. Commenter le **pourquoi**, pas le **quoi**.
12. Linter (ESLint) + formateur (Prettier) dès le premier jour.

### Early return

```js
// MAL : pyramide
function traiter(req) {
  if (req) {
    if (req.body) {
      if (req.body.ip) { /* ... */ }
    }
  }
}
// BIEN : gardes
function traiter(req) {
  if (!req?.body?.ip) return { erreur: "IP manquante" };
  /* ... logique principale au premier niveau ... */
}
```

### ESLint + Prettier (setup minimal)

```bash
npm init -y && npm install -D eslint prettier
npx eslint --init   # ou config manuelle
```

```js
// eslint.config.js (flat config, ESLint 9+)
export default [
  { rules: {
    "no-var": "error",
    "eqeqeq": ["error", "always"],
    "no-unused-vars": "warn",
    "prefer-const": "error",
  }}
];
```

### Style : points qui comptent

- Point-virgule : choisir (avec/sans) et laisser Prettier trancher.
- Guillemets simples en JS, backticks pour l'interpolation.
- `camelCase` variables/fonctions, `PascalCase` classes, `SNAKE_UPPER` constantes.
- Fichiers : un module = une responsabilité ; `kebab-case.js`.

---

## 49. Performance

### Mesurer d'abord

```js
console.time("boucle");
// ... code ...
console.timeEnd("boucle"); // "boucle: 12.345ms"

// Précis : performance.now()
const t0 = performance.now();
// ...
console.log(`${(performance.now() - t0).toFixed(2)} ms`);

// Benchmark sérieux : répéter + médiane (bruit de mesure)
function bench(fn, iterations = 10000) {
  const temps = [];
  for (let i = 0; i < iterations; i++) {
    const t0 = performance.now(); fn(); temps.push(performance.now() - t0);
  }
  temps.sort((a, b) => a - b);
  return temps[Math.floor(temps.length / 2)]; // médiane
}
```

### Règles de performance

| Règle | Pourquoi |
|---|---|
| Éviter le travail dans les boucles chaudes | hoister les invariants, pré-calculer |
| `for` > `forEach`/`map` en boucle ultra-chaude | moins d'overhead (micro-opt, à mesurer) |
| `Map`/`Set` pour recherches fréquentes | O(1) vs O(n) de `Array.includes`/`find` |
| Concaténer avec `push` + `join`, pas `+=` en boucle géante | chaînes immuables = copies |
| `JSON.parse/stringify` : coûteux sur gros volumes | streamer si possible |
| Regex pré-compilées hors boucle | `new RegExp` à chaque itération = lent |
| Éviter les fuites : listeners, timers, closures sur gros objets | mémoire qui ne redescend jamais |
| `structuredClone` > `JSON.parse(JSON.stringify())` | plus rapide et plus fidèle |

### Mémoire Node

```bash
node --max-old-space-size=4096 app.js  # augmenter le heap si besoin
node --inspect app.js                   # heap snapshots dans DevTools
```

```js
// Surveiller
const { heapUsed } = process.memoryUsage();
console.log(`heap : ${(heapUsed / 1024 / 1024).toFixed(1)} Mo`);
```

### Anti-patterns perf

```js
// MAL : N+1 requêtes séquentielles
for (const id of ids) { await fetch(`/api/eq/${id}`); } // N allers-retours
// BIEN : parallèle borné (voir pool section 24) ou endpoint batch

// MAL : JSON.parse/stringify pour cloner en boucle chaude
// BIEN : structuredClone, ou mieux : ne pas cloner (immuabilité ciblée)

// MAL : regex recompilée
for (const l of lignes) { if (/error/i.test(l)) n++; } // en fait /.../ littéral = pré-compilé, OK
for (const l of lignes) { if (new RegExp(motif).test(l)) n++; } // MAL : compilation à chaque tour
```

---

## 50. Sécurité

### Checklist sécurité (à appliquer systématiquement)

- [ ] Aucun secret en dur (code, git) → variables d'environnement / vault
- [ ] `execFile`/`spawn` avec args séparés, **jamais** d'interpolation dans `exec`
- [ ] Validation de toutes les entrées externes (schéma, plages, types)
- [ ] `textContent` plutôt que `innerHTML` avec des données externes
- [ ] Cookies auth : `HttpOnly; Secure; SameSite=Lax` (posés serveur)
- [ ] Dépendances : `npm audit` régulier, lockfile commité
- [ ] Erreurs : messages génériques côté client, détails côté logs
- [ ] `npm ci` en prod, pas `npm install`
- [ ] Principe du moindre privilège (utilisateur dédié, pas root)
- [ ] Timeouts sur toutes les I/O réseau

### Injection de commande (rappel section 33)

```js
// INTERDIT avec entrée utilisateur
exec(`ping -c 1 ${hote}`);
// AUTORISÉ
execFile("ping", ["-c", "1", hote]);
```

### XSS (navigateur)

```js
// DANGEREUX si `nom` vient d'un utilisateur/API
el.innerHTML = `<b>${nom}</b>`;
// SÛR
el.textContent = nom;
// ou échapper manuellement si HTML nécessaire :
const echapper = (s) => s.replace(/[&<>"']/g, c =>
  ({ "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;", "'": "&#39;" }[c]));
```

### Prototype pollution

```js
// DANGEREUX : fusion récursive naïve d'objets externes
// { "__proto__": { "isAdmin": true } } peut polluer Object.prototype !
// Défenses :
const cible = Object.create(null); // objet sans prototype
// ou : valider les clés ("__proto__", "constructor", "prototype" interdites)
// ou : structuredClone / lib de merge sûre
```

### Secrets et tokens

```js
// Générer un token sûr (jamais Math.random() pour la sécurité !)
import { randomBytes } from "crypto";
const token = randomBytes(32).toString("hex"); // 64 caractères hex

// Comparer des secrets en temps constant (anti timing-attack)
import { timingSafeEqual } from "crypto";
const ok = timingSafeEqual(Buffer.from(a), Buffer.from(b)); // mêmes longueurs requises
```

### Validation d'entrée (exemple)

```js
function validerEquipement(e) {
  const erreurs = [];
  if (typeof e.hostname !== "string" || !/^[A-Za-z0-9-]{1,32}$/.test(e.hostname))
    erreurs.push("hostname invalide");
  if (!/^(?:\d{1,3}\.){3}\d{1,3}$/.test(e.ip ?? ""))
    erreurs.push("ip invalide");
  if (e.port !== undefined && (!Number.isInteger(e.port) || e.port < 1 || e.port > 65535))
    erreurs.push("port invalide");
  if (erreurs.length) throw new Error("Validation : " + erreurs.join("; "));
  return e;
}
```

---

## 51. Erreurs classiques des débutants (15)

### 1. `==` au lieu de `===`
```js
if (code == "200") // true aussi pour 200 (number) — ambigu
if (code === 200)  // strict, prévisible
```

### 2. `NaN !== NaN`
```js
// MAL : if (x === NaN)
// BIEN :
if (Number.isNaN(x)) { /* ... */ }
```

### 3. Oublier le comparateur de `sort`
```js
[10, 2, 30].sort(); // [10, 2, 30] — tri alphabétique !
[10, 2, 30].sort((a, b) => a - b); // [2, 10, 30]
```

### 4. `var` dans une boucle + closure
```js
for (var i = 0; i < 3; i++) setTimeout(() => console.log(i), 0); // 3,3,3
for (let i = 0; i < 3; i++) setTimeout(() => console.log(i), 0); // 0,1,2
```

### 5. Muter un tableau/objet partagé
```js
const defaut = { port: 22 };
function connecter(opts = defaut) { opts.port = 2222; } // mute le défaut pour tous !
connecter(); connecter(); // 2e appel : port déjà à 2222
// BIEN : function connecter(opts = {}) { const cfg = { port: 22, ...opts }; }
```

### 6. `async` dans `forEach`
```js
// forEach n'attend pas les callbacks async → voir section 24
// BIEN : for...of ou Promise.all(urls.map(...))
```

### 7. Oublier que `fetch` ne rejette pas sur HTTP 4xx/5xx
```js
const r = await fetch(url);
if (!r.ok) throw new Error(`HTTP ${r.status}`); // TOUJOURS vérifier !
```

### 8. `this` perdu dans un callback
```js
class A { constructor(){ this.x = 1; } go(){ setTimeout(function(){ console.log(this.x); }, 0); } }
// undefined — BIEN : setTimeout(() => console.log(this.x), 0)
```

### 9. Comparer des objets avec `===`
```js
{ a: 1 } === { a: 1 }; // false — références différentes !
// BIEN : comparer champ à champ ou assert.deepStrictEqual
```

### 10. Hoisting / TDZ
```js
console.log(x); // ReferenceError (TDZ)
let x = 5;
```

### 11. `parseInt` sans base
```js
parseInt("08");      // 8 aujourd'hui, mais...
parseInt("08", 10);  // 8 — TOUJOURS préciser la base
```

### 12. Flottants : `0.1 + 0.2 !== 0.3`
```js
// BIEN : travailler en centimes (entiers) ou comparer avec EPSILON
Math.abs((0.1 + 0.2) - 0.3) < Number.EPSILON; // true
```

### 13. `Array` vide truthy
```js
if ([]) console.log("vrai"); // s'affiche ! — tester .length
if (resultats.length === 0) { /* ... */ }
```

### 14. Oublier `return` dans `.then` / fonction fléchée à bloc
```js
const f = () => { 42; }; // undefined !
const g = () => 42;      // 42
```

### 15. Bloquer l'event loop avec du sync lourd
```js
// const data = fs.readFileSync("2go.log"); // bloque TOUT le serveur
// BIEN : await readFile / streams (sections 31, 34)
```

---

## 52. Cas pratique 1 — parser de logs Apache/nginx

Objectif : lire un access log, compter les codes HTTP, top 10 des IP, détecter les 5xx. Streaming (gros fichiers OK).

```js
// parse-access-log.js — usage : node parse-access-log.js /var/log/nginx/access.log
import { createReadStream } from "fs";
import readline from "readline";

const RE = /^(\S+) \S+ \S+ \[([^\]]+)\] "(\S+) (\S+) \S+" (\d{3}) (\d+|-) "[^"]*" "([^"]*)"/;

async function analyser(chemin) {
  const parCode = new Map();   // "200" -> n
  const parIp = new Map();     // ip -> n
  const erreurs5xx = [];
  let total = 0, ignorees = 0;

  const rl = readline.createInterface({
    input: createReadStream(chemin),
    crlfDelay: Infinity,
  });

  for await (const ligne of rl) {
    const m = ligne.match(RE);
    if (!m) { ignorees++; continue; }
    const [, ip, , methode, url, code] = m;
    total++;
    parCode.set(code, (parCode.get(code) ?? 0) + 1);
    parIp.set(ip, (parIp.get(ip) ?? 0) + 1);
    if (code.startsWith("5")) erreurs5xx.push({ ip, methode, url, code });
  }

  const top = (map, n) => [...map.entries()].sort((a, b) => b[1] - a[1]).slice(0, n);

  console.log(`Lignes : ${total} (ignorées : ${ignorees})`);
  console.log("\nPar code HTTP :");
  for (const [code, n] of [...parCode.entries()].sort())
    console.log(`  ${code} : ${n} (${(n / total * 100).toFixed(1)}%)`);
  console.log("\nTop 10 IP :");
  for (const [ip, n] of top(parIp, 10)) console.log(`  ${ip} : ${n}`);
  console.log(`\nErreurs 5xx : ${erreurs5xx.length}`);
  for (const e of erreurs5xx.slice(0, 20))
    console.log(`  ${e.code} ${e.methode} ${e.url} ← ${e.ip}`);
}

const fichier = process.argv[2];
if (!fichier) { console.error("Usage : node parse-access-log.js <fichier>"); process.exit(1); }
await analyser(fichier);
```

---

## 53. Cas pratique 2 — client API REST avec retry

Objectif : interroger une API d'inventaire avec timeout, retry backoff, et gestion d'erreurs propre.

```js
// api-client.js
class HttpError extends Error {
  constructor(status, message) { super(message); this.name = "HttpError"; this.status = status; }
}

const sleep = (ms) => new Promise(r => setTimeout(r, ms));

export async function apiRequest(url, {
  method = "GET", body = null, token = process.env.API_TOKEN,
  timeoutMs = 8000, retries = 3,
} = {}) {
  let lastError;
  for (let attempt = 1; attempt <= retries + 1; attempt++) {
    try {
      const res = await fetch(url, {
        method,
        headers: {
          "Content-Type": "application/json",
          ...(token ? { Authorization: `Bearer ${token}` } : {}),
        },
        ...(body ? { body: JSON.stringify(body) } : {}),
        signal: AbortSignal.timeout(timeoutMs),
      });
      if (!res.ok) throw new HttpError(res.status, `${method} ${url} → HTTP ${res.status}`);
      const text = await res.text();
      return text ? JSON.parse(text) : null;
    } catch (err) {
      lastError = err;
      const retryable = err.name === "AbortError" || err.name === "TypeError" ||
        (err instanceof HttpError && (err.status >= 500 || err.status === 429));
      if (!retryable || attempt > retries) throw err;
      const wait = 1000 * 2 ** (attempt - 1);
      console.warn(`[${attempt}/${retries + 1}] ${err.message} — nouvel essai dans ${wait}ms`);
      await sleep(wait);
    }
  }
  throw lastError;
}

// Utilisation
const equipements = await apiRequest("https://inventaire.example.com/api/equipements?site=paris");
const cree = await apiRequest("https://inventaire.example.com/api/equipements",
  { method: "POST", body: { hostname: "SW9", ip: "10.0.0.9" } });
```

---

## 54. Cas pratique 3 — mini-serveur HTTP maison

Objectif : API interne de supervision sans dépendance (santé, liste d'équipements, réception d'alertes).

```js
// mini-api.js — node mini-api.js (PORT=3000)
import http from "http";

const equipements = new Map([
  ["SW1", { ip: "10.0.0.2", statut: "ok" }],
  ["R1",  { ip: "10.0.0.1", statut: "ok" }],
]);
const alertes = [];

function json(res, code, data) {
  res.writeHead(code, { "Content-Type": "application/json; charset=utf-8" });
  res.end(JSON.stringify(data));
}

async function lireBody(req) {
  let corps = "";
  for await (const chunk of req) {
    corps += chunk;
    if (corps.length > 1e6) throw new Error("body trop volumineux"); // anti-DoS basique
  }
  return corps ? JSON.parse(corps) : {};
}

const server = http.createServer(async (req, res) => {
  try {
    const url = new URL(req.url, `http://${req.headers.host}`);
    const route = `${req.method} ${url.pathname}`;

    if (route === "GET /sante") return json(res, 200, { statut: "ok", uptime: Math.round(process.uptime()) });
    if (route === "GET /equipements") return json(res, 200, [...equipements.entries()].map(([nom, f]) => ({ nom, ...f })));
    if (route === "GET /alertes") return json(res, 200, alertes.slice(-50));

    if (route === "POST /alertes") {
      const a = await lireBody(req);
      if (!a.equipement || !a.message) return json(res, 400, { erreur: "equipement et message requis" });
      const alerte = { ...a, recuLe: new Date().toISOString() };
      alertes.push(alerte);
      console.log(`[ALERTE] ${a.equipement} : ${a.message}`);
      return json(res, 201, alerte);
    }

    const m = url.pathname.match(/^\/equipements\/([\w-]+)$/);
    if (m && req.method === "PATCH") {
      const fiche = equipements.get(m[1]);
      if (!fiche) return json(res, 404, { erreur: "inconnu" });
      Object.assign(fiche, await lireBody(req));
      return json(res, 200, { nom: m[1], ...fiche });
    }

    return json(res, 404, { erreur: "route inconnue" });
  } catch (err) {
    console.error(err);
    return json(res, 500, { erreur: "erreur interne" }); // message générique côté client
  }
});

const PORT = Number(process.env.PORT) || 3000;
server.listen(PORT, () => console.log(`API sur http://localhost:${PORT}`));
process.on("SIGTERM", () => server.close(() => process.exit(0)));
process.on("SIGINT", () => server.close(() => process.exit(0)));
```

---

## 55. Cas pratique 4 — script d'inventaire réseau

Objectif : pinger une plage d'IP en parallèle borné, sortir un JSON d'inventaire.

```js
// inventaire.js — node inventaire.js 10.0.0 1 254
import { execFile } from "child_process";
import { promisify } from "util";
import { writeFile } from "fs/promises";
const execFileAsync = promisify(execFile);

const sleep = (ms) => new Promise(r => setTimeout(r, ms));

async function ping(ip, timeoutSec = 1) {
  try {
    await execFileAsync("ping", ["-c", "1", "-W", String(timeoutSec), ip], { timeout: 5000 });
    return true;
  } catch { return false; }
}

// Parallélisme borné : ne pas inonder le réseau
async function pool(taches, limite) {
  const resultats = [];
  const enCours = new Set();
  for (const t of taches) {
    const p = t().then(r => { enCours.delete(p); return r; });
    enCours.add(p); resultats.push(p);
    if (enCours.size >= limite) await Promise.race(enCours);
  }
  return Promise.all(resultats);
}

function* plageIp(base, debut, fin) {
  for (let i = debut; i <= fin; i++) yield `${base}.${i}`;
}

const [base = "10.0.0", debut = "1", fin = "254"] = process.argv.slice(2);
console.log(`Scan de ${base}.${debut} → ${base}.${fin} (50 parallèles max)...`);
const t0 = Date.now();

const joignables = await pool(
  [...plageIp(base, Number(debut), Number(fin))].map(ip => async () => {
    const ok = await ping(ip);
    if (ok) console.log(`  ✓ ${ip}`);
    return ok ? ip : null;
  }),
  50
).then(r => r.filter(Boolean));

const rapport = {
  genereLe: new Date().toISOString(),
  plage: `${base}.${debut}-${fin}`,
  dureeSec: Math.round((Date.now() - t0) / 1000),
  joignables,
  total: joignables.length,
};
await writeFile("inventaire.json", JSON.stringify(rapport, null, 2));
console.log(`\n${rapport.total} hôtes joignables en ${rapport.dureeSec}s → inventaire.json`);
```

---

## 56. Cas pratique 5 — dashboard web interne

Objectif : page HTML + JS qui interroge la mini-API (section 54) et affiche les équipements avec auto-refresh.

```html
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="utf-8">
<title>Supervision interne</title>
<style>
  body { font-family: system-ui, sans-serif; margin: 2rem; }
  table { border-collapse: collapse; width: 100%; }
  th, td { border: 1px solid #ccc; padding: .5rem; text-align: left; }
  .ok { color: green; font-weight: bold; } .ko { color: red; font-weight: bold; }
  #erreur { color: red; }
</style>
</head>
<body>
<h1>Supervision</h1>
<p id="erreur" hidden></p>
<table>
  <thead><tr><th>Nom</th><th>IP</th><th>Statut</th></tr></thead>
  <tbody id="corps"></tbody>
</table>
<p>Dernière mise à jour : <span id="maj">—</span></p>

<script>
const API = "http://localhost:3000";
const corps = document.getElementById("corps");
const maj = document.getElementById("maj");
const erreur = document.getElementById("erreur");

async function rafraichir() {
  try {
    const res = await fetch(`${API}/equipements`, { signal: AbortSignal.timeout(5000) });
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    const eqs = await res.json();
    erreur.hidden = true;
    const frag = document.createDocumentFragment(); // un seul reflow
    for (const e of eqs) {
      const tr = document.createElement("tr");
      // textContent : pas de XSS même si l'API renvoie du louche
      const tdNom = document.createElement("td"); tdNom.textContent = e.nom;
      const tdIp = document.createElement("td"); tdIp.textContent = e.ip;
      const tdStatut = document.createElement("td");
      tdStatut.textContent = e.statut;
      tdStatut.className = e.statut === "ok" ? "ok" : "ko";
      tr.append(tdNom, tdIp, tdStatut);
      frag.appendChild(tr);
    }
    corps.replaceChildren(frag);
    maj.textContent = new Date().toLocaleString("fr-FR");
  } catch (e) {
    erreur.hidden = false;
    erreur.textContent = `Échec de rafraîchissement : ${e.message}`;
  }
}

rafraichir();
const timer = setInterval(rafraichir, 30000);
timer.unref?.(); // navigateur : unref n'existe pas, le ?. évite l'erreur
</script>
</body>
</html>
```

---

## 57. Cas pratique 6 — automatisation de fichiers

Objectif : renommer/organiser des exports (ex : `export_2026-09-26.csv` → archivage par mois), avec log.

```js
// organiser-exports.js — node organiser-exports.js /chemin/exports
import { readdir, mkdir, rename, stat } from "fs/promises";
import { join, basename, extname } from "path";

const racine = process.argv[2] ?? "./exports";
const journal = [];

for (const f of await readdir(racine)) {
  const chemin = join(racine, f);
  if (!(await stat(chemin)).isFile()) continue;
  // export_SW1_2026-09-26.csv → dossier 2026-09/
  const m = f.match(/^export_(.+?)_(\d{4}-\d{2})-\d{2}(\.\w+)$/);
  if (!m) { journal.push({ fichier: f, action: "ignoré (nom non reconnu)" }); continue; }
  const [, nom, mois, ext] = m;
  const dest = join(racine, mois, `${nom}${ext}`);
  await mkdir(join(racine, mois), { recursive: true });
  await rename(chemin, dest);
  journal.push({ fichier: f, action: `archivé → ${mois}/${basename(dest)}` });
}

console.table(journal);
await import("fs/promises").then(fs =>
  fs.appendFile(join(racine, "journal.txt"),
    `[${new Date().toISOString()}] ${journal.length} fichiers traités\n`));
```

---

## 58. Cas pratique 7 — planificateur avec file de tâches

Objectif : file FIFO persistée en JSON, exécution séquentielle, reprise après crash.

```js
// file-taches.js — node file-taches.js ajouter "sauvegarde SW1" / node file-taches.js traiter
import { readFile, writeFile } from "fs/promises";

const FICHIER = "./file.json";

async function charger() {
  try { return JSON.parse(await readFile(FICHIER, "utf8")); }
  catch { return { enAttente: [], terminees: [] }; }
}
const sauver = (etat) => writeFile(FICHIER, JSON.stringify(etat, null, 2));

const [, , commande, ...reste] = process.argv;

if (commande === "ajouter") {
  const etat = await charger();
  etat.enAttente.push({ id: Date.now(), tache: reste.join(" "), ajouteeLe: new Date().toISOString() });
  await sauver(etat);
  console.log("Tâche ajoutée.");
} else if (commande === "lister") {
  const etat = await charger();
  console.table(etat.enAttente);
} else if (commande === "traiter") {
  const etat = await charger();
  while (etat.enAttente.length > 0) {
    const tache = etat.enAttente.shift(); // FIFO
    console.log(`▶ ${tache.tache}`);
    try {
      await new Promise(r => setTimeout(r, 500)); // ← remplacer par le vrai traitement
      etat.terminees.push({ ...tache, termineeLe: new Date().toISOString(), statut: "ok" });
    } catch (e) {
      etat.terminees.push({ ...tache, statut: "echec", erreur: e.message });
    }
    await sauver(etat); // persisté après CHAQUE tâche → reprise après crash
  }
  console.log("File vide.");
} else {
  console.log("Usage : node file-taches.js <ajouter|lister|traiter> [tâche]");
}
```

---

## 59. Pense-bête de poche

### Syntaxe express

```js
const c = 1; let v = 2;                       // déclarations
const f = (a, b = 1) => a + b;                // fléchée + défaut
const { ip, port = 22 } = srv;                // destructuration
const copie = { ...srv, ip: "x" };            // spread objet
const [p, ...reste] = liste;                  // rest tableau
`ssh ${user}@${ip}`;                          // template literal
obj?.champ?.methode?.();                      // optional chaining
const x = val ?? defaut;                      // nullish coalescing
```

### Async express

```js
await fetch(url);                             // pause sans bloquer
const [a, b] = await Promise.all([f1(), f2()]); // parallèle
for (const x of xs) await traiter(x);         // séquentiel
await Promise.all(xs.map(x => traiter(x)));   // parallèle (map)
try { await f(); } catch (e) { /* ... */ }    // erreurs async
```

### Les 7 falsy (par cœur)

`false` `0` `-0` `0n` `""` `null` `undefined` `NaN` — tout le reste est truthy (y compris `[]` et `{}`).

### Vérifications express

```js
Array.isArray(x);            // vrai tableau ?
Number.isNaN(x);             // NaN ?
x == null;                   // null ou undefined (idiome toléré)
typeof x === "string";       // type primitif
x instanceof MaClasse;       // instance
Object.hasOwn(o, "cle");     // propriété propre (ES2022)
```

### Node express

```bash
node script.js                 # exécuter
node --watch script.js         # rechargement auto
node --test                    # tests natifs
node --env-file=.env app.js    # variables d'environnement
node --inspect app.js          # debug DevTools
npm init -y && npm i <pkg>     # projet + dépendance
npx <pkg>                      # exécuter sans installer
```

### Commandes qui sauvent

```js
console.table(data);           // inspecter un tableau d'objets
console.dir(obj, {depth:null});// tout voir
JSON.stringify(o, null, 2);    // pretty-print
structuredClone(o);            // copie profonde
[...new Set(t)];               // dédupliquer
t.sort((a,b) => a-b);          // tri numérique
new URL(url).searchParams;     // parser query string
AbortSignal.timeout(5000);     // timeout fetch natif
```

---

## 60. Glossaire

| Terme | Définition |
|---|---|
| ASI | Automatic Semicolon Insertion : le moteur ajoute les `;` manquants (ne pas s'y fier aveuglément) |
| Callback | fonction passée en argument pour être appelée plus tard |
| Closure | fonction + les variables capturées de sa portée de définition |
| Coercition | conversion implicite de type (`"5" - 1` → `4`) |
| Debounce / Throttle | limiter la fréquence d'appels (recherche en direct, resize) |
| Event loop | boucle qui orchestre l'exécution : stack → microtâches → macrotâches |
| Hoisting | remontée des déclarations (`var`, `function`) en haut de la portée |
| IIFE | Immediately Invoked Function Expression : `(function(){...})()` |
| Polyfill | code qui apporte une API moderne aux vieux environnements |
| Promise | valeur future (pending → fulfilled / rejected) |
| Prototype | objet dont hérite une instance (chaîne de recherche des propriétés) |
| Scope | portée : zone où une variable est visible (bloc, fonction, module, globale) |
| Shadowing | variable locale qui masque une variable externe du même nom |
| Strict mode | `"use strict"` : erreurs sur les pratiques dangereuses (implicite en modules/classes) |
| TDZ | Temporal Dead Zone : `let`/`const` inaccessibles avant leur déclaration |
| Thunk | fonction sans argument qui retarde un calcul : `() => couteux()` |
| Tree-shaking | élimination du code inutilisé au build (possible grâce aux imports ESM statiques) |
| Truthy / Falsy | valeurs évaluées à `true`/`false` dans un contexte booléen |

---

## 61. Quiz — 10 questions + réponses

**Q1.** Que vaut `typeof null` et pourquoi ?
> `"object"` — bug historique du moteur, conservé pour compatibilité. Tester `x === null` explicitement.

**Q2.** Quelle est la différence entre `==` et `===` ? Lequel utiliser ?
> `==` compare avec coercition de type (`0 == "0"` → true), `===` compare type + valeur sans conversion. Toujours `===`.

**Q3.** Que se passe-t-il ici et comment corriger ?
> ```js
> for (var i = 0; i < 3; i++) setTimeout(() => console.log(i), 0);
> ```
> Affiche `3, 3, 3` : `var` est partagé par les 3 closures (même variable mutée). Corriger avec `let` (une variable par itération).

**Q4.** Pourquoi `fetch` vers une URL qui répond 500 n'entre-t-il pas dans le `catch` ?
> `fetch` ne rejette que sur échec réseau. Un statut HTTP 500 donne une promise *fulfilled* avec `ok === false`. Il faut tester `res.ok` / `res.status` manuellement.

**Q5.** `urls.forEach(async u => { await fetch(u); })` — quel est le problème ?
> `forEach` n'attend pas les callbacks : les requêtes partent en fire-and-forget et le code suivant s'exécute avant leur fin. Utiliser `for...of` (séquentiel) ou `Promise.all(urls.map(...))` (parallèle).

**Q6.** Citer les 4 règles de `this` par ordre de priorité.
> 1. `new` → le nouvel objet ; 2. appel méthode `obj.m()` → `obj` ; 3. `call`/`apply`/`bind` → l'objet passé ; 4. défaut → `undefined` (strict) ou objet global. Les fléchées n'ont pas de `this` propre (hérité lexicalement).

**Q7.** Dans l'event loop, qui passe en premier : le callback de `setTimeout(..., 0)` ou celui de `Promise.resolve().then(...)` ?
> La microtâche (promise) : l'event loop vide toujours la file des microtâches avant de prendre la prochaine macrotâche (timer).

**Q8.** `const cfg = { port: 22 }; cfg.port = 2222;` — erreur ou pas ? Pourquoi ?
> Pas d'erreur : `const` fige la *référence*, pas le contenu. L'objet reste mutable. Pour figer : `Object.freeze()`.

**Q9.** Comment copier un objet en profondeur, et pourquoi `JSON.parse(JSON.stringify(o))` est-il imparfait ?
> `structuredClone(o)` (moderne). Le hack JSON perd les `Date` (→ chaînes), `undefined`/fonctions/`Symbol`, échoue sur `BigInt` et les références circulaires.

**Q10.** Écrire une fonction `sleep(ms)` retournant une promise, puis l'utiliser pour attendre 2 s.
> ```js
> const sleep = (ms) => new Promise(resolve => setTimeout(resolve, ms));
> await sleep(2000);
> ```

---

## 62. Pour aller plus loin

### Documentation de référence
- **MDN Web Docs** (developer.mozilla.org) — LA référence JS : chaque méthode avec exemples et compatibilité.
- **node.js.org/docs** — API Node officielle.
- **javascript.info** — tutoriel moderne, excellent sur les subtilités.

### Approfondir par sujet
| Sujet | Ressource |
|---|---|
| Event loop visuelle | Conf "What the heck is the event loop?" (Philip Roberts, YouTube) |
| Async moderne | MDN "Using promises" + "async function" |
| Node en prod | "Node.js Best Practices" (goldbergyoni, GitHub) |
| Regex | regex101.com (tester en direct), regexr.com |
| Sécurité | OWASP Cheat Sheets |
| TypeScript | typescriptlang.org — le successeur typé (recommandé pour projets > quelques fichiers) |

### Prochaines étapes concrètes pour Zelef
1. Convertir un script shell récurrent (sauvegarde, rapport) en script Node CLI avec `commander`.
2. Exposer un endpoint `/sante` sur un outil interne existant (section 54).
3. Ajouter des tests `node --test` sur les fonctions de parsing (section 46).
4. Passer un projet à TypeScript quand la base dépasse ~1000 lignes.
5. Conteneuriser un outil Node avec Docker (multi-stage build, user non-root).

### Frameworks (quand le besoin vient)
- **API** : Express (simple), Fastify (rapide), NestJS (structuré).
- **Front** : pas besoin de framework pour un dashboard interne (section 56 suffit) ; sinon Vue ou Svelte.
- **CLI** : `commander`, `yargs`.
- **Temps réel** : `ws` (WebSocket), Socket.IO.

---

## 63. Checklist de mise en production

### Code
- [ ] ESLint passe sans erreur, Prettier appliqué
- [ ] `node --test` : tous les tests verts
- [ ] Aucun `console.log` de debug restant (logger structuré : `pino`/`winston`)
- [ ] Gestion d'erreurs : `try/catch` aux frontières (I/O, réseau, parsing)
- [ ] `unhandledRejection` / `uncaughtException` gérés (log + exit 1)

### Config & secrets
- [ ] Secrets uniquement via variables d'environnement / vault — rien en dur, rien dans git
- [ ] `.env` dans `.gitignore`, `.env.example` commité (sans valeurs)
- [ ] `package-lock.json` commité, `npm ci` utilisé au déploiement
- [ ] `engines.node` fixé dans package.json

### Exploitation
- [ ] Timeouts sur toutes les I/O réseau et `child_process`
- [ ] Arrêt propre : `SIGTERM`/`SIGINT` → fermer serveur/connexions
- [ ] Supervision : process manager (`pm2`, systemd) avec restart auto
- [ ] Logs avec timestamps ISO, niveau (info/warn/error), contexte (qui/quoi)
- [ ] Santé : endpoint `/sante` + alerte si down
- [ ] Utilisateur système dédié, jamais root ; principe du moindre privilège
- [ ] Sauvegarde des données persistées (fichiers JSON, etc.)

---

## 64. Annexe — tableau de compatibilité ES

| Version | Année | Apports majeurs | Node min |
|---|---|---|---|
| ES2015 (ES6) | 2015 | `let`/`const`, fléchées, classes, modules, promises, template literals, destructuration, spread | 6+ |
| ES2016 | 2016 | `**`, `Array.includes` | 7+ |
| ES2017 | 2017 | `async/await`, `Object.entries/values`, `padStart/padEnd` | 8+ |
| ES2018 | 2018 | rest/spread objets, groupes nommés regex, lookbehind | 10+ |
| ES2019 | 2019 | `flat`/`flatMap`, `Object.fromEntries`, `trimStart/End`, `?.` catch sans param | 12+ |
| ES2020 | 2020 | `?.`, `??`, `BigInt`, `Promise.allSettled`, `matchAll`, `globalThis` | 14+ |
| ES2021 | 2021 | `replaceAll`, `??=`/`&&=`/`\|\|=`, séparateurs `_`, `Promise.any` | 16+ |
| ES2022 | 2022 | top-level await, `.at()`, champs privés `#`, `Object.hasOwn`, `Error.cause` | 18+ |
| ES2023 | 2023 | `toSorted`/`toReversed`/`toSpliced`, `findLast`, shebang | 20+ |
| ES2024 | 2024 | `Object.groupBy`, `Promise.withResolvers` | 22+ |
| ES2025 | 2025 | `Set` union/intersection/difference, `RegExp.escape` | 22+ |

> Node 22 couvre confortablement ES2024. Pour les navigateurs : vérifier sur caniuse.com si parc ancien.

---

## 65. Annexe — recettes express sysadmin

```js
// 1. Lire un fichier ligne par ligne (stream)
import { createReadStream } from "fs";
import readline from "readline";
const rl = readline.createInterface({ input: createReadStream("/var/log/syslog"), crlfDelay: Infinity });
for await (const ligne of rl) { if (/error/i.test(ligne)) console.log(ligne); }

// 2. Ping rapide d'un hôte (sécurisé)
import { execFile } from "child_process";
import { promisify } from "util";
const ok = await promisify(execFile)("ping", ["-c", "1", "-W", "1", "10.0.0.1"]).then(() => true).catch(() => false);

// 3. Requête API JSON avec timeout (Node 18+)
const data = await fetch("https://api.example.com/status", { signal: AbortSignal.timeout(5000) })
  .then(r => { if (!r.ok) throw new Error(`HTTP ${r.status}`); return r.json(); });

// 4. Horodater un log
const ts = () => new Date().toISOString();
console.log(`[${ts()}] sauvegarde terminée`);

// 5. Charger une config JSON avec valeurs par défaut
import { readFile } from "fs/promises";
const cfg = { port: 3000, hote: "localhost", ...JSON.parse(await readFile("config.json", "utf8").catch(() => "{}")) };

// 6. Lister les fichiers récents d'un dossier
import { readdir, stat } from "fs/promises";
const fichiers = await readdir("/var/log");
const recents = [];
for (const f of fichiers) {
  const s = await stat(`/var/log/${f}`).catch(() => null);
  if (s?.isFile() && Date.now() - s.mtimeMs < 24 * 3600e3) recents.push(f);
}

// 7. Sleep / attendre
const sleep = (ms) => new Promise(r => setTimeout(r, ms));

// 8. Exécuter N tâches en parallèle borné (voir pool() section 24/55)

// 9. Générer un mot de passe/token sûr
import { randomBytes } from "crypto";
const secret = randomBytes(24).toString("base64url"); // Node 15.7+

// 10. Valider une IPv4
const estIpv4 = (s) => /^(?:(?:25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)\.){3}(?:25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)$/.test(s);

// 11. Top N des occurrences (comptage)
const top = (arr, n = 10) => [...arr.reduce((m, x) => m.set(x, (m.get(x) ?? 0) + 1), new Map())]
  .sort((a, b) => b[1] - a[1]).slice(0, n);

// 12. Échapper pour le shell — NE PAS FAIRE soi-même : utiliser execFile (section 33)

// 13. Dédupliquer et trier des lignes
const uniques = [...new Set((await readFile("hosts.txt", "utf8")).split("\n").map(l => l.trim()).filter(Boolean))].sort();

// 14. Timeout global sur un script
const TIMEOUT_MS = 60000;
setTimeout(() => { console.error("timeout global"); process.exit(2); }, TIMEOUT_MS).unref();

// 15. Arguments CLI simples
const args = Object.fromEntries(process.argv.slice(2).map(a => a.replace(/^--/, "").split("=")));
// node script.js --hote=10.0.0.1 --port=22 → { hote: "10.0.0.1", port: "22" }
```

---

*Fin du guide — bon code, et que tes scripts ne plantent jamais un vendredi soir.*
