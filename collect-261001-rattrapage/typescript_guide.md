# TypeScript — Guide ultra-complet

> **Public visé** : sysadmin / chef de service systèmes & énergies qui développe des outils internes, des CLI et des dashboards.
> **Ton** : direct, tutoriel + référence. Chaque section contient du code exécutable et des tableaux de synthèse.
> **Version couverte** : TypeScript 5.x (les syntaxes dépendant d'une version sont signalées). Node.js 20+.

---

## 1. Pourquoi TypeScript quand on vient du scripting

En administration système, on écrit beaucoup de scripts : inventaires, collecte SNMP, parsing de logs, glue entre API. JavaScript suffit pour un script jetable de 50 lignes. Dès que l'outil devient critique (un dashboard utilisé par l'équipe, un CLI de provisioning), les types deviennent un filet de sécurité :

- **Les erreurs sont détectées à la compilation**, pas à 3h du matin en astreinte.
- **L'autocomplétion** dans VS Code connaît la forme exacte de vos objets (une réponse d'API REST, une ligne de CSV parsée).
- **Le refactoring est sûr** : renommer un champ `hostname` en `fqdn` dans 40 fichiers devient une opération mécanique.
- **La documentation est vivante** : une interface `Onduleur` décrit le contrat mieux qu'un commentaire.

TypeScript = JavaScript + un système de types statique qui **disparaît à la compilation** (aucun surcoût au runtime).

```typescript
// Sans types : l'erreur n'apparaît qu'à l'exécution
function puissanceTotale(onduleurs) {
  return onduleurs.reduce((s, o) => s + o.puissance, 0);
  // si "puissance" s'appelle "kva" dans les données : NaN silencieux
}

// Avec types : l'erreur est signalée avant même d'exécuter
interface Onduleur { nom: string; kva: number; }

function puissanceTotale(onduleurs: Onduleur[]): number {
  return onduleurs.reduce((s, o) => s + o.kva, 0);
}
```

**Checklist mentale** — adoptez TypeScript quand :
- [ ] le script dépasse ~200 lignes ou est partagé avec l'équipe ;
- [ ] il manipule des données structurées (JSON d'API, CSV, YAML) ;
- [ ] il doit tourner en production / en cron sans surveillance ;
- [ ] plusieurs personnes vont le maintenir.

---

## 2. Installation et tooling

Prérequis : Node.js 20+ (`node -v`). Trois façons d'installer TypeScript :

```bash
# 1. Globale (pratique pour essayer, déconseillé en projet)
npm install -g typescript
tsc --version   # ex : Version 5.6.3

# 2. Par projet (recommandé : version figée par projet)
mkdir mon-outil && cd mon-outil
npm init -y
npm install -D typescript @types/node
npx tsc --version

# 3. Vérifier ce qui est installé
npx tsc --version
```

**Outils compagnons à installer d'emblée :**

| Outil | Rôle | Installation |
|---|---|---|
| `typescript` | Compilateur `tsc` | `npm i -D typescript` |
| `@types/node` | Types de l'API Node (fs, process…) | `npm i -D @types/node` |
| `tsx` | Exécution directe de TS (dev) | `npm i -D tsx` |
| `vitest` | Tests unitaires | `npm i -D vitest` |
| `eslint` + `prettier` | Qualité + formatage | voir section 58 |

**VS Code** (éditeur recommandé) : l'extension TypeScript est intégrée. Réglez la version du workspace pour utiliser celle du projet : `Ctrl+Shift+P` → *TypeScript: Select TypeScript Version* → *Use Workspace Version*.

**Structure de projet conseillée :**

```
mon-outil/
├── src/
│   ├── index.ts        # point d'entrée
│   ├── inventory.ts
│   └── types.ts        # interfaces partagées
├── tests/
│   └── inventory.test.ts
├── dist/               # sortie compilée (généré, à ignorer via .gitignore)
├── package.json
└── tsconfig.json
```

---

## 3. Premier programme : de `hello.ts` au JavaScript

```typescript
// src/hello.ts
const message: string = "Bonjour depuis TypeScript";
console.log(message);
```

Compilation et exécution :

```bash
npx tsc src/hello.ts --outDir dist --target es2022 --module nodenext
node dist/hello.js
```

Points clés :
- `tsc` **transpile** : il enlève les annotations de types et produit du JS pur.
- Par défaut, `tsc fichier.ts` compile **sans vérification stricte**. Activez toujours `--strict` (ou un `tsconfig.json`, section 4).
- Les types n'existent qu'à la compilation : `typeof maVariable` au runtime ne verra jamais une interface.

**Erreurs ≠ blocage** : par défaut, `tsc` émet quand même le JS même s'il y a des erreurs de types. Pour interdire ça en CI : `"noEmitOnError": true` dans le tsconfig.

```bash
# Vérifier sans émettre (idéal en CI / pre-commit)
npx tsc --noEmit
```

---

## 4. tsconfig.json : la référence complète

Le `tsconfig.json` est le centre de contrôle. Voici un modèle **production-ready** pour un outil Node, commenté option par option :

```jsonc
{
  "compilerOptions": {
    /* --- Sortie --- */
    "target": "ES2022",              // version JS générée (ES2022 = Node 18+ OK)
    "module": "NodeNext",            // système de modules (NodeNext = ESM/CJS natif)
    "moduleResolution": "NodeNext",  // comment résoudre les imports
    "outDir": "dist",                // dossier du JS compilé
    "rootDir": "src",                // dossier des sources
    "declaration": true,             // génère les .d.ts (utile si package publié)
    "sourceMap": true,              // .map pour debugger le TS d'origine
    "removeComments": false,

    /* --- Rigueur (voir section 5) --- */
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,

    /* --- Modules --- */
    "esModuleInterop": true,         // import express from "express"
    "allowSyntheticDefaultImports": true,
    "resolveJsonModule": true,       // import config from "./config.json"
    "isolatedModules": true,         // compatibilité esbuild/tsx/swc

    /* --- Sécurité de compilation --- */
    "noEmitOnError": true,           // ne produit rien si erreur de types
    "forceConsistentCasingInFileNames": true,
    "skipLibCheck": true,            // ignore les erreurs dans les .d.ts tiers

    /* --- Style --- */
    "noUnusedLocals": true,          // erreur si variable locale inutilisée
    "noUnusedParameters": true,      // erreur si paramètre inutilisé
    "noFallthroughCasesInSwitch": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist", "tests"]
}
```

**Tableau des options les plus importantes :**

| Option | Effet | Recommandé |
|---|---|---|
| `target` | Version JS émise (`ES2022`, `ESNext`…) | `ES2022` (Node 20) |
| `module` / `moduleResolution` | Format des modules | `NodeNext`/`NodeNext` |
| `strict` | Active tout le bloc strict | `true` **toujours** |
| `outDir` / `rootDir` | Sépare sources et build | `dist` / `src` |
| `declaration` | Émet les fichiers `.d.ts` | `true` si lib publiée |
| `sourceMap` | Permet de debugger le `.ts` | `true` |
| `noEmitOnError` | Bloque la sortie en cas d'erreur | `true` en CI |
| `resolveJsonModule` | `import` de fichiers JSON | `true` |
| `esModuleInterop` | Compatibilité CommonJS | `true` |
| `skipLibCheck` | Ignore les erreurs des `.d.ts` tiers | `true` (build plus rapide) |
| `isolatedModules` | Chaque fichier compilable seul | `true` (requis par tsx/esbuild) |

> **Piège classique** : `"module": "NodeNext"` exige `"moduleResolution": "NodeNext"`. Si vous mettez l'un sans l'autre, `tsc` proteste.

---

## 5. Le mode strict, option par option

`"strict": true` est un **interrupteur général** qui active 7 vérifications. Les comprendre une par une évite de le désactiver par frustration.

| Sous-option | Ce qu'elle interdit | Exemple d'erreur évitée |
|---|---|---|
| `strictNullChecks` | `null`/`undefined` assignés n'importe où | Appeler `.length` sur une valeur peut-être `null` |
| `noImplicitAny` | Paramètre au type `any` implicite | `function f(x) {}` → `x` est `any` caché |
| `strictFunctionTypes` | Contravariance des paramètres de fonction | Assigner un callback incompatible |
| `strictBindCallApply` | `bind`/`call`/`apply` non vérifiés | Mauvais arguments via `.call()` |
| `strictPropertyInitialization` | Propriété de classe non initialisée | `this.host` utilisé avant affectation |
| `noImplicitThis` | `this: any` implicite | `this` dans une fonction détachée |
| `alwaysStrict` | Émet `"use strict"` dans le JS | Oubli du mode strict JS |

**Deux options strictes hors du pack `strict` (à activer à la main) :**

```typescript
// "noUncheckedIndexedAccess": true
const tab: string[] = ["a", "b"];
const x = tab[10]; // type: string | undefined (au lieu de string)
// → vous force à gérer le cas undefined. Précieux quand on parse des données.

// "exactOptionalPropertyTypes": true
interface Config { timeout?: number; }
const c: Config = { timeout: undefined }; // ERREUR : undefined n'est pas assignable
// → distingue "propriété absente" de "propriété = undefined".
```

**Règle d'or** : commencez tout nouveau projet avec `"strict": true`. Sur un projet existant, migrez progressivement (`// @ts-nocheck` en haut des fichiers non migrés, puis fichier par fichier).

---

## 6. Types primitifs : le socle

```typescript
const nom: string = "onduleur-01";
const puissance: number = 40;          // pas de int/float : tout est number
const enLigne: boolean = true;
const gros: bigint = 9007199254740993n; // grands entiers (target ES2020+)
const id: symbol = Symbol("id");        // identifiants uniques
const rien: null = null;
const indefini: undefined = undefined;
```

**Inférence** : TypeScript devine le type quand c'est évident. N'annotez que quand ça apporte de l'information :

```typescript
let compteur = 0;            // inféré: number — pas besoin d'annoter
let etat: "ok" | "ko" = "ok"; // annotation utile : union de littéraux
```

**`number` et ses pièges** (le type ne protège pas de tout) :

```typescript
console.log(0.1 + 0.2);      // 0.30000000000000004 — arithmétique flottante
Number.isSafeInteger(2 ** 53); // false → passer à bigint si besoin
```

> Pense-bête : `string`, `number`, `boolean` s'écrivent **en minuscules**. `String`, `Number`, `Boolean` (majuscules) sont les objets wrappers — ne les utilisez jamais comme types.

---

## 7. string, number, boolean : les subtilités utiles

**Template literals typés** (TS 4.1+) : des strings dont la forme est vérifiée.

```typescript
type Fqdn = `${string}.${string}`;      // "srv-01.lan" OK
type IpV4 = `${number}.${number}.${number}.${number}`;

function ping(hote: Fqdn): void { /* ... */ }
ping("srv-01.lan");   // OK
ping("srv-01");       // Erreur : ne matche pas `${string}.${string}`
```

**Conversions explicites** (TypeScript ne convertit jamais implicitement) :

```typescript
const portStr: string = "8080";
const port: number = Number(portStr);        // "8080" -> 8080
const ok: boolean = portStr === "8080";     // comparaison stricte

// Depuis une variable d'environnement (toujours string | undefined !)
const portEnv: number = Number(process.env.PORT ?? "3000");
```

**Opérateur `??` vs `||`** — capital en scripting :

```typescript
const timeout = config.timeout ?? 5000;  // défaut seulement si null/undefined
const timeout2 = config.timeout || 5000; // défaut aussi si 0 ! dangereux
// Avec ||, un timeout configuré à 0 serait écrasé. Préférez ??.
```

---

## 8. Arrays typés

```typescript
const hosts: string[] = ["srv-01", "srv-02"];
const ports: Array<number> = [22, 80, 443]; // syntaxe équivalente

hosts.push("srv-03");   // OK
hosts.push(42);         // Erreur : number non assignable à string
```

**Tableaux en lecture seule** (évite les mutations accidentelles) :

```typescript
const LISTE: readonly string[] = ["a", "b"];
LISTE.push("c"); // Erreur : push n'existe pas sur readonly
```

**Itérations typées** — l'inférence fait le travail :

```typescript
const onduleurs = [
  { nom: "ups-01", charge: 42 },
  { nom: "ups-02", charge: 67 },
];
// onduleurs: { nom: string; charge: number }[]

for (const o of onduleurs) {
  console.log(o.nom.toUpperCase()); // o.nom est string : autocomplétion OK
}

const charges = onduleurs.map((o) => o.charge * 2); // number[]
const critiques = onduleurs.filter((o) => o.charge > 80); // même type d'objet[]
```

> Avec `noUncheckedIndexedAccess` (section 5), `onduleurs[0]` vaut `{...} | undefined` : pensez au test ou au `!` assumé.

---

## 9. Tuples : des tableaux à forme fixe

Un tuple = un tableau dont **chaque position a son type**. Idéal pour les paires clé/valeur, les coordonnées, les retours multiples.

```typescript
type EntreeInventaire = [nom: string, ip: string, enLigne: boolean];
const srv: EntreeInventaire = ["srv-01", "10.0.0.1", true];
// Les labels (nom:, ip:) sont purement documentaires.

srv[0] = "srv-02"; // OK : string
srv[1] = 123;      // Erreur : number non assignable à string

// Destructuration typée
const [nom, ip, enLigne] = srv; // string, string, boolean

// Tuple à longueur variable (reste typé)
type LigneCsv = [string, ...number[]];
const ligne: LigneCsv = ["ups-01", 42, 67, 12]; // OK
```

**Cas d'usage sysadmin** : retour de fonction multi-valeurs sans créer d'interface.

```typescript
function parseLigneInventaire(ligne: string): [string, string] | null {
  const [nom = "", ip = ""] = ligne.split(";");
  if (!nom || !ip) return null;
  return [nom, ip];
}
```

---

## 10. Union types : « soit l'un, soit l'autre »

```typescript
type Statut = "en-ligne" | "hors-ligne" | "maintenance";
let s: Statut = "en-ligne";
s = "inconnu"; // Erreur

type Id = string | number;
function afficher(id: Id): void {
  console.log(`ID: ${id}`); // OK : les deux ont toString via template
}
```

**Unions d'objets** — la base des machines à états :

```typescript
type ResultatPing =
  | { ok: true; latenceMs: number }
  | { ok: false; erreur: string };

function traiter(r: ResultatPing): void {
  if (r.ok) {
    console.log(r.latenceMs); // OK : narrowing automatique (section 24)
  } else {
    console.log(r.erreur);    // OK
  }
}
```

**Règle** : sur une union, vous ne pouvez utiliser que les propriétés **communes** à tous les membres sans narrowing préalable.

```typescript
declare const r: ResultatPing;
r.latenceMs; // Erreur : n'existe pas sur le membre { ok: false }
```

---

## 11. Intersection types : « à la fois l'un et l'autre »

```typescript
type Identifiable = { id: string };
type Horodate = { creeLe: Date };

type Equipement = Identifiable & Horodate;
// { id: string; creeLe: Date }

const ups: Equipement = { id: "ups-01", creeLe: new Date() };
```

**Cas d'usage** : composer des comportements (mixins), enrichir un type tiers sans le modifier :

```typescript
import type { Request } from "express";

type RequeteAuthentifiee = Request & { utilisateur: { login: string; roles: string[] } };
// On ajoute l'utilisateur injecté par un middleware d'auth.
```

> **Union vs intersection** : `A | B` = "l'un OU l'autre" (au moins un), `A & B` = "l'un ET l'autre" (les deux). L'intersection de types incompatibles donne `never` : `string & number` est inhabitable.

---

## 12. Types littéraux : des valeurs comme types

```typescript
type NiveauLog = "debug" | "info" | "warn" | "error";
type CodeSortie = 0 | 1 | 2;

function log(niveau: NiveauLog, msg: string): void {
  if (niveau === "error") console.error(msg);
  else console.log(`[${niveau}] ${msg}`);
}
log("info", "démarrage");  // OK
log("fatal", "x");         // Erreur
```

**`const` + littéraux** : une variable `const` garde son littéral, `let` s'élargit.

```typescript
const niveau = "info"; // type: "info" (littéral conservé)
let niveau2 = "info";  // type: string (élargi)
```

**`as const`** (assertion const, section 23) fige un objet entier en littéraux — très utile pour les tables de correspondance :

```typescript
const SEUILS = {
  critique: 90,
  warning: 75,
} as const;
// { readonly critique: 90; readonly warning: 75 }

type Seuil = keyof typeof SEUILS; // "critique" | "warning"
```

---

## 13. Enums : valeurs nommées (et leurs alternatives)

```typescript
enum StatutEquipement {
  EnLigne,      // 0
  HorsLigne,    // 1
  Maintenance,  // 2
}

enum Protocole {
  Snmp = "snmp",
  Ssh = "ssh",
  Http = "http",
}

const p: Protocole = Protocole.Snmp; // "snmp"
```

**Tableau comparatif : enum vs union de littéraux :**

| Critère | `enum` | Union de littéraux (`"a" \| "b"`) |
|---|---|---|
| Valeur au runtime | Objet réel généré | Disparaît (zéro coût) |
| Itération (`Object.values`) | Possible | Impossible |
| Reverse mapping (numérique) | Oui (`Statut[0] === "EnLigne"`) | Non |
| Interop JSON/API | Verbeux | Naturel (ce sont des strings) |
| Tree-shaking | Mauvais | Parfait |

**Recommandation moderne** : pour des API/JSON, préférez les unions de littéraux + `as const`. Gardez `enum` quand vous avez besoin d'itérer ou d'un mapping bidirectionnel.

```typescript
// Alternative recommandée : objet const
const PROTOCOLE = {
  Snmp: "snmp",
  Ssh: "ssh",
} as const;
type Protocole = (typeof PROTOCOLE)[keyof typeof PROTOCOLE]; // "snmp" | "ssh"
// Itérable via Object.values(PROTOCOLE), zéro surcoût, JSON-friendly.
```

> **Signal de version** : les `const enum` (zéro coût runtime) sont interdits avec `isolatedModules: true` (donc avec tsx/esbuild). Évitez-les.

---

## 14. Interfaces : les bases

Une interface décrit la **forme** d'un objet. C'est le type le plus courant dans le code TypeScript.

```typescript
interface Serveur {
  nom: string;
  ip: string;
  roles: string[];
  enLigne: boolean;
}

const srv: Serveur = {
  nom: "srv-01",
  ip: "10.0.0.1",
  roles: ["hyperviseur", "dns"],
  enLigne: true,
};
```

**Propriétés optionnelles** (`?`) et **readonly** :

```typescript
interface ConfigCollecte {
  readonly sondeId: string;  // assignable une seule fois
  intervalleSec?: number;     // optionnel
  destinations: string[];
}

const cfg: ConfigCollecte = { sondeId: "sonde-01", destinations: ["influx"] };
cfg.sondeId = "x"; // Erreur : readonly
```

**Interfaces de fonctions et d'index** :

```typescript
// Signature de fonction
interface PingFn {
  (hote: string, timeoutMs?: number): Promise<number>;
}

// Index signature : dictionnaire à clés dynamiques
interface Inventaire {
  [nom: string]: Serveur; // toute clé string -> Serveur
}
const inv: Inventaire = { "srv-01": srv };
```

> **Excès de propriétés** : un objet littéral avec une propriété inconnue est rejeté (frappe), mais pas un objet passé via variable. C'est une protection contre les fautes de frappe, pas une étanchéité.

```typescript
const s2: Serveur = { nom: "x", ip: "y", roles: [], enLigne: true, cpu: 4 };
// Erreur : 'cpu' n'existe pas dans Serveur (object literal excess check)
```

---

## 15. Type aliases : `type`

`type` crée un alias pour **n'importe quel** type : union, tuple, fonction, primitif, objet.

```typescript
type Ip = string;
type Handler = (event: string) => void;
type Matrice = number[][];
type Reponse<T> = { data: T; erreur?: string }; // générique (section 33)
```

**Objet via `type`** (équivalent à l'interface pour la forme simple) :

```typescript
type ServeurT = {
  nom: string;
  ip: string;
};
```

Différences techniques interface/type (détaillées section 16) : l'interface supporte la **déclaration fusionnée** et `extends`/`implements` ; le `type` supporte les unions, tuples et types calculés.

---

## 16. Interfaces vs Types : quand utiliser quoi

**Règle pratique (recommandation officielle TS) :**

| Situation | Privilégier |
|---|---|
| Forme d'un objet / d'une classe (contrat) | `interface` |
| Union, intersection, tuple, fonction, primitif | `type` |
| Type calculé (mapped, conditional) | `type` (obligatoire) |
| API publique d'une bibliothèque | `interface` (extensible par l'utilisateur) |
| Extension d'un type existant | `interface extends` |

```typescript
// ✅ interface : contrat extensible
interface Equipement { nom: string; }
interface Onduleur extends Equipement { kva: number; }

// ✅ type : composition impossible en interface
type Charge = number | "inconnue";
type Paire = [string, number];

// ✅ type : utilitaires (section 36-38)
type Partiel = Partial<Equipement>;
```

**Declaration merging** (interfaces uniquement) : deux déclarations du même nom **fusionnent**. Utile pour étendre des types tiers, dangereux si accidentel :

```typescript
interface Fenetre { titre: string; }
interface Fenetre { largeur: number; }
// Fenetre = { titre: string; largeur: number }
```

> En pratique d'équipe : **interfaces pour les modèles métier** (`Serveur`, `Onduleur`, `Alerte`), **types pour tout le reste**.

---

## 17. Optionnel, readonly, index signatures : le trio du modelage

```typescript
interface Sondage {
  readonly id: string;          // écriture unique
  hote: string;
  port?: number;                // peut être absent
  tags: readonly string[];      // tableau non modifiable
  meta: Record<string, string>; // dictionnaire (section 37)
  [clef: string]: unknown;       // attrape-tout (attention : affaiblit le type)
}
```

**Tableau de décision :**

| Besoin | Syntaxe |
|---|---|
| Champ facultatif | `port?: number` |
| Champ non réassignable | `readonly id: string` |
| Tableau non modifiable | `readonly string[]` |
| Dictionnaire homogène | `Record<string, T>` ou `{ [k: string]: T }` |
| Rendre tout optionnel | `Partial<T>` |
| Rendre tout requis | `Required<T>` |

**`?.` et `!` à l'usage** :

```typescript
declare const s: Sondage;
const p = s.port ?? 443;        // ✅ valeur par défaut propre
const p2 = s.port!.toFixed();   // ⚠️ non-null assertion : "je sais qu'il est défini"
```

> Le `!` (non-null assertion) désactive la vérification : à n'utiliser que quand vous avez une **garantie externe** (ex : validé juste avant). En `strict`, préférez le narrowing.

---

## 18. Fonctions : signatures et paramètres

```typescript
// Annotation complète
function ping(hote: string, timeoutMs: number): Promise<number> {
  // ...
  return Promise.resolve(12);
}

// Inférence du retour (possible, mais annotez les fonctions publiques)
function addition(a: number, b: number) {
  return a + b; // inféré: number
}
```

**Pourquoi annoter le retour des fonctions exportées** :
1. Le compilateur vérifie que tous les chemins retournent le bon type.
2. Le `.d.ts` généré est explicite.
3. Évite les fuites de types internes dans l'API publique.

**Paramètres typés et `this`** :

```typescript
// this typé explicitement (utile avec noImplicitThis)
function afficher(this: Serveur): void {
  console.log(this.nom);
}
```

---

## 19. Paramètres optionnels, par défaut et rest

```typescript
function connecter(
  hote: string,
  port: number = 22,        // défaut → type number, optionnel à l'appel
  options?: { timeout?: number }, // vraiment optionnel
  ...tags: string[]          // rest : 0..n strings
): void {
  console.log(hote, port, options?.timeout, tags);
}

connecter("srv-01");                    // OK
connecter("srv-01", 2222);              // OK
connecter("srv-01", 22, { timeout: 5 }); // OK
connecter("srv-01", 22, undefined, "prod", "critique"); // OK
```

**Ordre obligatoire** : requis → défaut/optionnel → rest. Un paramètre requis ne peut pas suivre un optionnel :

```typescript
function f(a?: string, b: string) {} // Erreur : b requis après a optionnel
```

---

## 20. Surcharge de fonctions (overloads)

Quand une fonction accepte **des formes d'appel différentes**, les overloads décrivent chaque forme :

```typescript
// Signatures publiques (ce que l'appelant voit)
function requete(url: string): Promise<string>;
function requete(url: string, format: "json"): Promise<unknown>;
function requete(url: string, format: "text"): Promise<string>;
// Implémentation (invisible de l'extérieur)
function requete(url: string, format: "json" | "text" = "text"): Promise<unknown> {
  // ...
  return fetch(url).then((r) => (format === "json" ? r.json() : r.text()));
}

const t = await requete("http://x");          // Promise<string>
const j = await requete("http://x", "json");  // Promise<unknown>
```

**Règles** :
- L'implémentation doit être compatible avec **toutes** les signatures.
- L'implémentation n'est pas appelable directement avec sa propre signature large.
- Alternative moderne : une seule signature avec union + narrowing interne (souvent plus simple).

---

## 21. Fonctions fléchées et callbacks typés

```typescript
// Callback typé inline
const hosts = ["srv-01", "srv-02"];
hosts.forEach((h, index) => {
  console.log(index, h.toUpperCase()); // h: string, index: number (inférés)
});

// Type de callback réutilisable
type AlerteHandler = (alerte: { niveau: string; message: string }) => void;

function surveiller(handler: AlerteHandler): void {
  handler({ niveau: "critique", message: "UPS en surcharge" });
}
```

**`void` vs `undefined` en retour** : un callback typé `( ) => void` accepte une fonction qui retourne quelque chose (la valeur est ignorée). C'est voulu : `forEach` n'utilise pas le retour.

```typescript
const f: () => void = () => 42; // OK : 42 est ignoré
```

**Async comme callback** : attention, `forEach` n'attend pas les promesses (section 43).

---

## 22. unknown, any, never, void : le quatuor à maîtriser

| Type | Signification | Assignable à | Opérations permises |
|---|---|---|---|
| `any` | « je renonce au typage » | tout (contamine) | tout, sans vérification |
| `unknown` | « je ne sais pas encore » (sûr) | rien sans narrowing | narrowing obligatoire |
| `never` | « impossible » | tout | aucune valeur possible |
| `void` | « pas de retour utile » | `undefined` seul | rien |

```typescript
// any : la trappe (à bannir via eslint @typescript-eslint/no-explicit-any)
declare const sale: any;
sale.nimporteQuoi(); // aucune erreur... jusqu'au runtime

// unknown : le any sûr — typique des données externes
async function lireJson(chemin: string): Promise<unknown> {
  const txt = await import("node:fs/promises").then((fs) => fs.readFile(chemin, "utf8"));
  return JSON.parse(txt); // JSON.parse retourne any → on le remonte en unknown
}
const data: unknown = await lireJson("conf.json");
if (typeof data === "object" && data !== null && "port" in data) {
  console.log((data as { port: number }).port); // narrowing puis usage
}

// never : fonction qui ne revient jamais
function panique(msg: string): never {
  throw new Error(msg);
}
// never : exhaustiveness checking (section 26)

// void : fonction sans retour significatif
function notifier(msg: string): void {
  console.log(msg);
}
```

**Règle d'équipe** : `unknown` pour les entrées externes (JSON, argv, API), `never` pour l'impossible, `any` interdit sauf interop legacy documentée.

---

## 23. Assertions de type et `as const`

L'assertion dit au compilateur : « fais-moi confiance, c'est ce type ». Elle ne convertit rien au runtime.

```typescript
// Depuis unknown / any
const data: unknown = JSON.parse('{"port": 8080}');
const port = (data as { port: number }).port;

// Syntaxe alternative (évitez-la en .tsx : conflit avec le JSX)
const port2 = (<{ port: number }>data).port;
```

**`as const`** : fige en littéraux readonly (déjà vu section 12) :

```typescript
const SEUILS = { critique: 90, warning: 75 } as const;
const NIVEAUX = ["debug", "info", "error"] as const;
type Niveau = (typeof NIVEAUX)[number]; // "debug" | "info" | "error"
```

**`satisfies`** (TS 4.9+) : vérifie la conformité **sans élargir** le type — le meilleur des deux mondes :

```typescript
const config = {
  hote: "srv-01",
  port: 22,
  proto: "ssh",
} satisfies { hote: string; port: number; proto: string };
// config.proto garde le type littéral "ssh" (pas string) ET est vérifié.
```

> **Avertissement** : une assertion fausse = un mensonge au compilateur. `JSON.parse` + `as MonType` sans validation runtime (Zod, section 68) est la source n°1 de bugs « impossibles » en TypeScript.

---

## 24. Narrowing : réduire une union avec typeof, instanceof, in

Le **narrowing** = le compilateur suit vos tests et affine le type dans chaque branche.

```typescript
function formater(valeur: string | number): string {
  if (typeof valeur === "string") {
    return valeur.toUpperCase(); // ici: string
  }
  return valeur.toFixed(2);      // ici: number
}
```

**Les 4 gardes natives :**

| Garde | Usage | Exemple |
|---|---|---|
| `typeof x === "..."` | primitifs | `"string"`, `"number"`, `"boolean"`, `"undefined"`, `"bigint"`, `"function"`, `"symbol"` |
| `x instanceof C` | classes | `err instanceof Error`, `d instanceof Date` |
| `"clef" in obj` | propriété présente | `"latenceMs" in resultat` |
| `Array.isArray(x)` | tableaux | `Array.isArray(donnees)` |

```typescript
function traiter(entree: string[] | string): void {
  if (Array.isArray(entree)) {
    entree.forEach((e) => console.log(e)); // string[]
  } else {
    console.log(entree.split(","));        // string
  }
}

// Garde "in" sur union d'objets
type R = { ok: true; latence: number } | { ok: false; erreur: string };
function log2(r: R): void {
  if ("latence" in r) console.log(r.latence); // { ok: true, ... }
  else console.log(r.erreur);
}
```

**Vérité/faux (truthiness)** : `if (x)` narrow `"" | 0 | null | undefined | NaN` hors du type. Pratique mais attention : `0` et `""` valides sont exclus — préférez `x != null` ou `x !== undefined` quand 0/"" sont légitimes.

---

## 25. Type guards personnalisés (type predicates)

Quand la logique de test est réutilisable, extrayez-la dans un **prédicat** `x is T` :

```typescript
interface Onduleur { type: "onduleur"; kva: number; }
interface Serveur2 { type: "serveur"; cpu: number; }
type Equipement2 = Onduleur | Serveur2;

// Prédicat : le retour "e is Onduleur" informe le compilateur
function estOnduleur(e: Equipement2): e is Onduleur {
  return e.type === "onduleur";
}

function puissance(e: Equipement2): number {
  if (estOnduleur(e)) {
    return e.kva * 0.8; // e: Onduleur ici
  }
  return 0;
}
```

**Assertion functions** (TS 3.7+) : des guards qui lancent au lieu de retourner false :

```typescript
function assertEstDefini<T>(v: T | null | undefined): asserts v is T {
  if (v === null || v === undefined) throw new Error("Valeur manquante");
}

declare const p: number | undefined;
assertEstDefini(p);
p.toFixed(2); // OK : p est number après l'assertion
```

> Les prédicats sont la façon propre de valider des données d'API avant usage — alternative légère à Zod pour des cas simples.

---

## 26. Discriminated unions & exhaustiveness checking

Une **union discriminée** = union d'objets partageant un champ discriminant littéral (souvent `type` ou `kind`). C'est le pattern n°1 pour les machines à états, les événements, les résultats d'opérations.

```typescript
type Evenement =
  | { type: "ping"; hote: string }
  | { type: "alerte"; niveau: "warn" | "critique"; message: string }
  | { type: "fin"; code: number };

function traiterEvenement(e: Evenement): void {
  switch (e.type) {
    case "ping":
      console.log(`ping ${e.hote}`); // e: { type: "ping"; hote: string }
      break;
    case "alerte":
      console.log(`[${e.niveau}] ${e.message}`);
      break;
    case "fin":
      console.log(`code ${e.code}`);
      break;
    default:
      // Exhaustiveness : si on ajoute un variant sans case, ça casse ici
      const impossible: never = e;
      throw new Error(`Événement non géré: ${JSON.stringify(impossible)}`);
  }
}
```

Le `const impossible: never = e` est le **contrôle d'exhaustivité** : si un jour vous ajoutez `{ type: "reboot" }` à l'union sans ajouter de `case`, `e` ne sera plus `never` → **erreur de compilation**. Inestimable pour les handlers d'événements et les reducers.

---

## 27. Classes : les bases

```typescript
class Sonde {
  // Propriétés déclarées (strictPropertyInitialization les exige initialisées)
  nom: string;
  private dernierPing: number | null = null;

  constructor(nom: string) {
    this.nom = nom;
  }

  ping(): string {
    this.dernierPing = Date.now();
    return `pong ${this.nom}`;
  }
}

const s = new Sonde("sonde-dc1");
s.ping();
s.dernierPing; // Erreur : private
```

**Rappel important** : les modificateurs `private`/`public` sont **compile-time uniquement** — au runtime, tout reste accessible en JS. Pour une vraie encapsulation runtime : `#champPrive` (champs privés ECMAScript).

```typescript
class Coffre {
  #secret = "s3cret"; // vraiment privé au runtime
}
```

---

## 28. Constructeurs et parameter properties

Les **parameter properties** déclarent + assignent en une ligne :

```typescript
class Equipement3 {
  constructor(
    public readonly nom: string,   // déclare this.nom (public, readonly)
    private ip: string,            // déclare this.ip (private)
    public enLigne = true,         // avec valeur par défaut
  ) {}
  // Pas de corps nécessaire !
}

const e = new Equipement3("srv-01", "10.0.0.1");
e.nom; // "srv-01"
```

**Surcharge de constructeur** (même syntaxe que les overloads, section 20) :

```typescript
class Plage {
  constructor(debut: string, fin: string);
  constructor(cidr: string);
  constructor(a: string, b?: string) {
    // ...
  }
}
```

> `strictPropertyInitialization` : toute propriété non optionnelle doit être assignée dans le constructeur ou à la déclaration. Échappatoire assumée : `propriete!: string` (definite assignment assertion) quand l'init est faite par un framework.

---

## 29. Modificateurs : public, private, protected, readonly, static

```typescript
class Compteur {
  static instances = 0;          // partagé par la classe (pas l'instance)
  readonly id: string;           // assignable une fois (constructeur)
  private valeur = 0;            // classe uniquement (compile-time)
  protected nom: string;         // classe + sous-classes

  constructor(nom: string) {
    this.nom = nom;
    this.id = `c-${Compteur.instances++}`;
  }

  incrementer(): void { this.valeur++; }
  get lecture(): number { return this.valeur; } // getter
}

class CompteurNomme extends Compteur {
  etiquette(): string { return this.nom; } // OK : protected visible ici
}
```

| Modificateur | Visible depuis | Runtime |
|---|---|---|
| `public` (défaut) | partout | normal |
| `private` | la classe seule | encore accessible en JS ! |
| `protected` | classe + héritiers | idem |
| `#prive` | la classe seule | **vraiment privé** |
| `readonly` | lecture partout, écriture au constructeur | normal |
| `static` | via `Classe.membre` | normal |

---

## 30. Héritage et override

```typescript
class EquipementReseau {
  constructor(public nom: string, public ip: string) {}

  decrire(): string {
    return `${this.nom} (${this.ip})`;
  }
}

class Commutateur extends EquipementReseau {
  constructor(nom: string, ip: string, public nbPorts: number) {
    super(nom, ip); // obligatoire avant d'utiliser this
  }

  override decrire(): string {  // "override" exigé si noImplicitOverride
    return `${super.decrire()} - ${this.nbPorts} ports`;
  }
}
```

**Points d'attention** :
- `super(...)` **avant** tout usage de `this` dans le constructeur dérivé.
- `override` explicite : activez `noImplicitOverride: true` dans le tsconfig pour forcer le mot-clé (évite les fautes de frappe qui créent une méthode au lieu d'en surcharger une).
- Préférez la **composition** à l'héritage quand il n'y a pas de vraie relation « est-un ».

---

## 31. Classes abstraites

Une classe abstraite définit un **squelette** : elle ne s'instancie pas, elle s'étend.

```typescript
abstract class Collecteur {
  abstract source(): string;          // à implémenter obligatoirement
  abstract collecter(): Promise<Record<string, number>>;

  // Méthode concrète partagée (template method pattern)
  async cycle(): Promise<void> {
    const debut = Date.now();
    const mesures = await this.collecter();
    console.log(`[${this.source()}] ${Object.keys(mesures).length} mesures en ${Date.now() - debut}ms`);
  }
}

class CollecteurSnmp extends Collecteur {
  source(): string { return "snmp"; }
  async collecter(): Promise<Record<string, number>> {
    return { charge: 42 }; // ... vraie implémentation
  }
}

// new Collecteur(); // Erreur : classe abstraite
await new CollecteurSnmp().cycle();
```

> Abstrait vs interface : l'abstrait peut porter du **code partagé** et un état ; l'interface ne porte que le contrat. Pour un plugin système (collecteurs, exporters), l'abstrait + template method est idéal.

---

## 32. Implémenter des interfaces avec des classes

```typescript
interface Pingable {
  hote: string;
  ping(): Promise<number>;
}

class ServeurPing implements Pingable {
  constructor(public hote: string) {}
  async ping(): Promise<number> {
    return 12; // ... vraie implémentation
  }
}
```

**`implements` ne vérifie que la forme publique** : il ne contrôle pas les types internes. Et une classe peut implémenter plusieurs interfaces :

```typescript
class MultiSonde implements Pingable, CollecteurDeMetriques { /* ... */ }
```

> Astuce : `implements` + `satisfies`-like via `implements` sur des object literals n'existe pas — pour un objet littéral conforme à une interface, annotez-le (`const x: MonInterface = {...}`) ou utilisez `satisfies` (section 23).

---

## 33. Génériques : les fondamentaux

Les génériques paramètrent un type par **un autre type**. Sans eux, on perd l'information (retour `any`) ou on duplique.

```typescript
// Sans générique : le type de retour est perdu
function premierAny(tab: any[]): any { return tab[0]; }

// Avec générique : le type circule
function premier<T>(tab: T[]): T | undefined {
  return tab[0];
}

const n = premier([1, 2, 3]);       // T = number → number | undefined
const s = premier(["a", "b"]);      // T = string → string | undefined
const srv = premier([{ nom: "x" }]); // { nom: string } | undefined
```

**Génériques sur interfaces, classes, alias** :

```typescript
interface ReponseApi<T> {
  data: T;
  erreur?: string;
  dureeMs: number;
}

class File<T> {
  private items: T[] = [];
  enfiler(x: T): void { this.items.push(x); }
  defiler(): T | undefined { return this.items.shift(); }
}

type Paire<K, V> = [K, V];
```

**Inférence vs explicite** : TypeScript infère `T` depuis les arguments. Précisez-le quand l'inférence se trompe ou pour contraindre l'appel :

```typescript
const x = premier<number>([1, 2, 3]); // explicite, rarement nécessaire
```

---

## 34. Contraintes (extends) et valeurs par défaut

`extends` sur un paramètre générique = **contrainte** : « T doit au moins avoir cette forme ».

```typescript
function nomDe<T extends { nom: string }>(e: T): string {
  return e.nom.toUpperCase();
}
nomDe({ nom: "srv-01", ip: "x" }); // OK
nomDe({ ip: "x" });                // Erreur : pas de 'nom'
```

**Valeur par défaut** :

```typescript
interface Cache<T = string> { // T vaut string si non précisé
  get(clef: string): T | undefined;
}
const c1: Cache = {} as Cache;        // Cache<string>
const c2: Cache<number> = {} as Cache<number>;
```

**Contrainte `keyof`** — le duo gagnant avec les objets :

```typescript
function lireProp<Objet, Clef extends keyof Objet>(obj: Objet, clef: Clef): Objet[Clef] {
  return obj[clef];
}
const srvX = { nom: "srv-01", port: 22 };
const p = lireProp(srvX, "port"); // number (pas string | number !)
lireProp(srvX, "inexistant");     // Erreur de compilation
```

---

## 35. keyof, typeof et l'accès aux types

Trois opérateurs pour **dériver des types de valeurs existantes** — la base du DRY typé.

```typescript
const CONFIG = {
  hote: "srv-01",
  port: 22,
  tls: true,
} as const;

type ClefConfig = keyof typeof CONFIG;      // "hote" | "port" | "tls"
type Port = (typeof CONFIG)["port"];        // 22 (littéral, grâce à as const)

function getConfig<K extends ClefConfig>(k: K): (typeof CONFIG)[K] {
  return CONFIG[k];
}
getConfig("port");    // 22
getConfig("inconnu"); // Erreur
```

**Tableau récapitulatif :**

| Opérateur | Opère sur | Produit |
|---|---|---|
| `typeof v` | une valeur | son type |
| `keyof T` | un type objet | union de ses clés |
| `T[K]` | un type objet + clé | type de la propriété |
| `typeof import("...")` | un module | son namespace de types |

> Pattern sysadmin typique : définir **une seule** source de vérité (objet `as const` ou interface) et dériver tout le reste (clés, unions, validateurs).

---

## 36. Types utilitaires : Partial, Required, Readonly

TypeScript fournit des **utility types** qui transforment des types existants. Les trois premiers rendent les propriétés optionnelles / requises / readonly **récursivement au premier niveau**.

```typescript
interface ServeurComplet {
  nom: string;
  ip: string;
  port: number;
  tags: string[];
}

// Tout optionnel : parfait pour les PATCH / mises à jour partielles
type ServeurPatch = Partial<ServeurComplet>;
// { nom?: string; ip?: string; port?: number; tags?: string[] }

function majServeur(nom: string, patch: ServeurPatch): void {
  // ... UPDATE ... SET ... avec seulement les champs fournis
}
majServeur("srv-01", { port: 2222 }); // OK

// Tout requis (inverse de Partial)
type ServeurRequis = Required<ServeurComplet>;

// Tout readonly (premier niveau)
type ServeurFige = Readonly<ServeurComplet>;
```

> `Partial` est **shallow** (premier niveau uniquement). Pour du deep-partial, il faut un mapped type récursif (section 39).

---

## 37. Types utilitaires : Pick, Omit, Record

```typescript
interface ServeurComplet {
  nom: string; ip: string; port: number; tags: string[]; secret: string;
}

// Sous-ensemble : Pick (inclusion)
type ServeurPublic = Pick<ServeurComplet, "nom" | "ip" | "port">;
// { nom: string; ip: string; port: number }

// Soustraction : Omit (exclusion) — idéal pour masquer des secrets
type ServeurSansSecret = Omit<ServeurComplet, "secret">;

// Dictionnaire : Record<Clés, Valeur>
type Inventaire2 = Record<string, ServeurPublic>;
const inv2: Inventaire2 = {
  "srv-01": { nom: "srv-01", ip: "10.0.0.1", port: 22 },
};

// Record avec union de clés : exhaustif !
type Niveau = "warn" | "critique";
const seuils: Record<Niveau, number> = { warn: 75, critique: 90 };
// Oublier "critique" → erreur. Le compilateur vous force à tout couvrir.
```

**Cas d'usage par utilitaire :**

| Utilitaire | Quand l'utiliser |
|---|---|
| `Partial<T>` | Body PATCH, options de fonction, formulaires partiels |
| `Required<T>` | Forcer la complétude après validation |
| `Readonly<T>` | Config figée, constantes partagées |
| `Pick<T, K>` | DTO public, projection de champs |
| `Omit<T, K>` | Retirer secrets / champs internes avant export JSON |
| `Record<K, V>` | Dictionnaires, tables de correspondance exhaustives |

---

## 38. Types utilitaires : ReturnType, Parameters, Awaited…

Ces utilitaires **extraient** des informations de fonctions existantes — fini la duplication de signatures.

```typescript
async function fetchSonde(id: string): Promise<{ charge: number }> {
  return { charge: 42 };
}

type SondeData = Awaited<ReturnType<typeof fetchSonde>>;
// { charge: number } — le type "déballe" la promesse

type ArgsFetch = Parameters<typeof fetchSonde>;
// [id: string]

function creer(...args: ArgsFetch): void { /* réutilise la signature */ }

// Autres utiles :
type Ctor = ConstructorParameters<typeof Date>; // params du constructeur
type Inst = InstanceType<typeof Date>;           // type d'instance
```

**Tableau complet des utilitaires natifs :**

| Utilitaire | Effet |
|---|---|
| `Partial<T>` / `Required<T>` / `Readonly<T>` | Modifie l'optionnalité / readonly |
| `Pick<T,K>` / `Omit<T,K>` | Sélection / exclusion de clés |
| `Record<K,V>` | Dictionnaire |
| `Exclude<U,E>` / `Extract<U,E>` | Soustrait / garde des membres d'une union |
| `NonNullable<T>` | Retire `null`/`undefined` |
| `ReturnType<F>` / `Parameters<F>` | Retour / paramètres d'une fonction |
| `Awaited<T>` | Déballe les `Promise` imbriquées |
| `ConstructorParameters` / `InstanceType` | Sur les constructeurs |

```typescript
type Statut2 = "ok" | "ko" | "inconnu";
type SansInconnu = Exclude<Statut2, "inconnu">; // "ok" | "ko"
```

---

## 39. Mapped types : transformer des types par programme

Un **mapped type** applique une transformation à chaque clé d'un type — c'est une boucle `for` au niveau des types.

```typescript
// Rendre optionnel "à la main" (= Partial)
type MonPartial<T> = {
  [K in keyof T]?: T[K];
};

// Rendre nullable
type Nullable<T> = {
  [K in keyof T]: T[K] | null;
};

// Modificateurs : -? retire l'optionnel, -readonly retire le readonly
type FigéEnMutable<T> = {
  -readonly [K in keyof T]: T[K];
};
```

**Exemple concret : tous les champs d'un formulaire avec leurs erreurs** :

```typescript
interface Formulaire { hote: string; port: number; }
type Erreurs<T> = { [K in keyof T]?: string };

const erreurs: Erreurs<Formulaire> = { port: "Le port doit être entre 1 et 65535" };
```

**`as` dans les mapped types** (TS 4.1+) : renommer/filtrer les clés :

```typescript
type Getters<T> = {
  [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
};
type G = Getters<{ nom: string }>;
// { getNom: () => string }
```

---

## 40. Conditional types & infer : la logique dans les types

Un **conditional type** = un `if` au niveau des types : `T extends U ? A : B`.

```typescript
type EstString<T> = T extends string ? true : false;
type A = EstString<"hello">; // true
type B = EstString<42>;      // false

// Aplatir un tableau : T[] -> T, sinon T
type Aplatir<T> = T extends (infer E)[] ? E : T;
type C = Aplatir<string[]>; // string
type D = Aplatir<number>;   // number
```

**`infer`** capture un morceau du type dans la branche `true`. C'est ainsi que `ReturnType` et `Awaited` sont implémentés en interne :

```typescript
// Simplifié : voilà comment ReturnType fonctionne vraiment
type MonReturnType<F> = F extends (...args: never[]) => infer R ? R : never;
```

**Cas sysadmin** : extraire le type de données d'un collecteur générique :

```typescript
type DonneesDe<C> = C extends { collecter(): Promise<infer D> } ? D : never;

declare class CollecteurSnmp2 {
  collecter(): Promise<{ charge: number }>;
}
type D2 = DonneesDe<CollecteurSnmp2>; // { charge: number }
```

> Les conditionals sont **distributifs** sur les unions : `Aplatir<string[] | number>` = `string | number`. Pour désactiver la distributivité, enveloppez : `[T] extends [U]`.

---

## 41. Template literal types : des strings calculées

Les template literals existent aussi **au niveau des types** (TS 4.1+) :

```typescript
type EvenementSonde = `sonde:${string}:${"up" | "down"}`;
const e1: EvenementSonde = "sonde:dc1:up";    // OK
const e2: EvenementSonde = "sonde:dc1:panic"; // Erreur

// Générer des noms de méthodes/event handlers
type HandlerName<E extends string> = `on${Capitalize<E>}`;
type H = HandlerName<"connect">; // "onConnect"
```

**Parsing de strings au niveau des types** avec `infer` :

```typescript
type ExtraireHote<S> = S extends `${string}://${infer Hote}/${string}` ? Hote : never;
type H2 = ExtraireHote<"https://srv-01.lan/api">; // "srv-01.lan"
```

**Cas d'usage** : typer des clés de métriques, des topics MQTT, des routes d'API :

```typescript
type Route = `/api/${"v1" | "v2"}/${"sondes" | "alertes"}`;
const r: Route = "/api/v2/sondes"; // OK
```

> **Signal de version** : template literal types = TS 4.1+. `Capitalize`/`Uncapitalize`/`Uppercase`/`Lowercase` = utilitaires natifs TS 4.8-.

---

## 42. Branded types : des types nominaux dans un monde structurel

TypeScript est **structurel** : deux types de même forme sont interchangeables. Parfois on veut distinguer `UserId` de `string` :

```typescript
// Brand : un champ fantôme qui n'existe qu'au niveau des types
type HostId = string & { readonly __brand: "HostId" };

function creerHostId(s: string): HostId {
  if (!s) throw new Error("id vide");
  return s as HostId; // seule porte d'entrée (validée)
}

function pingHost(id: HostId): void { /* ... */ }

pingHost(creerHostId("srv-01")); // OK
pingHost("srv-01");              // Erreur ! string ≠ HostId
```

**Variante objet** (plus lisible dans les messages d'erreur) :

```typescript
interface KiloWatt { readonly __unite: "kW"; valeur: number; }
```

> À utiliser pour : identifiants (évite de mélanger `userId` et `hostId`), unités (kW vs kVA — critique en énergie !), valeurs validées (email vérifié vs string brute).

---

## 43. async/await typé

```typescript
async function mesurerLatence(hote: string): Promise<number> {
  const debut = Date.now();
  await attendre(50); // simulation
  return Date.now() - debut; // number → Promise<number> automatique
}

function attendre(ms: number): Promise<void> {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

// À l'appel : await déballe
const latence: number = await mesurerLatence("srv-01");
```

**Règles d'or** :
1. Une fonction `async` retourne **toujours** une `Promise` (même `return 42` → `Promise<number>`).
2. `await` n'est autorisé que dans une fonction `async` (ou au top-level d'un module ESM — Node 14.8+).
3. **Ne jamais** passer une fonction `async` à `forEach`/`map` sans gérer les promesses (voir point 5).

```typescript
// ❌ Piège classique : forEach n'attend pas
hosts.forEach(async (h) => { await pingHost2(h); });
console.log("fini"); // s'affiche AVANT la fin des pings !

// ✅ Boucle séquentielle
for (const h of hosts) { await pingHost2(h); }

// ✅ Parallèle contrôlé
await Promise.all(hosts.map((h) => pingHost2(h)));
```

---

## 44. Promesses : typage avancé

```typescript
// Promise.all : tuple typé position par position
const [latence, charge] = await Promise.all([
  mesurerLatence("srv-01"),   // Promise<number>
  Promise.resolve("42%"),     // Promise<string>
]);
// latence: number, charge: string

// Promise.allSettled : n'échoue jamais globalement
const resultats = await Promise.allSettled(hosts.map((h) => pingHost2(h)));
for (const r of resultats) {
  if (r.status === "fulfilled") console.log(r.value);  // narrowing natif
  else console.error(r.reason);
}

// Premier arrivé gagne
const rapide = await Promise.race([
  fetchDonnees("http://a"),
  timeout(2000).then(() => { throw new Error("timeout"); }),
]);
```

**Typer un timeout/race proprement** :

```typescript
function avecTimeout<T>(p: Promise<T>, ms: number, message = "Timeout"): Promise<T> {
  let timer: NodeJS.Timeout;
  const garde = new Promise<never>((_, reject) => {
    timer = setTimeout(() => reject(new Error(`${message} après ${ms}ms`)), ms);
  });
  return Promise.race([p, garde]).finally(() => clearTimeout(timer));
}

const data3 = await avecTimeout(fetch("http://sonde/api").then((r) => r.json()), 3000);
```

> `Promise<never>` = une promesse qui ne se résout jamais positivement : parfaite pour les gardes de timeout.

---

## 45. Modules ESM : import / export

TypeScript utilise les modules ES standard. **Chaque fichier est un module** dès qu'il a un `import`/`export`.

```typescript
// src/types.ts
export interface Alerte { niveau: string; message: string; date: Date; }
export const SEUIL_CRITIQUE = 90;
export default class Moteur { /* ... */ } // un seul default par fichier

// src/alertes.ts
import Moteur, { Alerte, SEUIL_CRITIQUE } from "./types.js";
import type { Alerte as AlerteType } from "./types.js"; // import de type seul
```

**Points critiques avec `module: NodeNext`** :
1. **Extension obligatoire** : `from "./types.js"` (même si le fichier source est `types.ts`). C'est l'exigence Node ESM.
2. `import type` : import **effacé à la compilation** — zéro impact runtime, zéro risque d'import circulaire de valeurs. Activez `verbatimModuleSyntax: true` (TS 5.0+) pour forcer la distinction.
3. Évitez `export default` dans le code partagé : un seul par fichier, refactoring fragile, interop CJS compliquée. Préférez les exports nommés.

```typescript
// ✅ Recommandé
export function ping() {}
export interface Config {}

// Import namespace
import * as utils from "./utils.js";
utils.ping();
```

---

## 46. Résolution de modules, paths et interop

**`paths`** : des alias pour éviter les `../../../` :

```jsonc
// tsconfig.json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@types/*": ["src/types/*"]
    }
  }
}
```

```typescript
import { Alerte } from "@/types.js"; // au lieu de ../../../types.js
```

> ⚠️ **Attention** : `paths` n'est qu'une indication pour `tsc`. Au **runtime**, Node ne connaît pas `@/`. Solutions : utiliser un bundler, ou le plugin `tsc-alias`, ou des imports relatifs en prod. Avec `tsx` en dev, ça marche via son resolver.

**Interop CommonJS** (`esModuleInterop: true`) :

```typescript
import express from "express";        // CJS vu comme default
import * as fs from "node:fs";        // namespace
import { readFile } from "node:fs/promises"; // nommé
```

**Ré-exports (barrel files)** :

```typescript
// src/index.ts — point d'entrée public du package
export * from "./types.js";
export * from "./alertes.js";
export { Moteur } from "./moteur.js";
```

> Les barrel files simplifient les imports mais peuvent ralentir la compilation et créer des cycles. Dans un gros projet, importez les modules directement.

---

## 47. Namespaces et declaration merging

Les `namespace` sont l'ancien mécanisme d'organisation interne (pré-modules). **En code moderne : préférez les modules ESM.** Ils restent utiles pour **étendre des types tiers** (declaration merging) :

```typescript
// Étendre les variables d'environnement typées (process.env est string | undefined par défaut)
declare global {
  namespace NodeJS {
    interface ProcessEnv {
      readonly SONDE_API_URL: string;
      readonly PORT?: string;
    }
  }
}
// À placer dans un fichier src/env.d.ts
// → process.env.SONDE_API_URL est maintenant string (pas string | undefined)
```

**Declaration merging utile** : ajouter une méthode à une interface tierce sans la modifier :

```typescript
declare module "express-serve-static-core" {
  interface Request {
    utilisateur?: { login: string };
  }
}
```

> Règle : `namespace` uniquement pour de l'**ambient** (déclarations `.d.ts`, extensions globales). Jamais pour structurer votre code applicatif.

---

## 48. Fichiers .d.ts et @types : les types sans le code

Un fichier `.d.ts` ne contient **que des déclarations de types** — aucun code exécutable. Usages :

1. **Typer du JS existant** sans le réécrire.
2. **Typer les variables globales** (`env.d.ts` ci-dessus).
3. **Publier les types** d'une bibliothèque (`declaration: true` génère les `.d.ts`).

```typescript
// src/legacy.d.ts — typer un vieux module JS sans types
declare module "vieux-client-snmp" {
  export function get(oid: string): Promise<string>;
  export const VERSION: string;
}
```

**@types/node** : le package qui type toute l'API Node (`fs`, `process`, `path`…). Sans lui, `import fs from "node:fs"` est `any` implicite → erreur en `strict`.

```bash
npm i -D @types/node   # types Node
npm i -D @types/express # types d'express (si pas de types natifs)
```

> De plus en plus de packages embarquent leurs propres types (`"types"` dans leur package.json) : vérifiez avant d'installer un `@types/*`.

---

## 49. Node.js : fs, path, os — les essentiels typés

```typescript
import { readFile, writeFile, mkdir, readdir } from "node:fs/promises";
import path from "node:path";
import os from "node:os";

// Chemins multi-plateformes
const dossier = path.join(os.homedir(), "inventaires");
const fichier = path.resolve(dossier, "srv-01.json");
path.extname(fichier); // ".json"
path.basename(fichier, ".json"); // "srv-01"

// Infos système
os.hostname();        // string
os.cpus().length;     // nombre de cœurs
os.freemem();         // octets libres
os.uptime();          // secondes
```

**Lire un dossier et filtrer** :

```typescript
const entrees = await readdir(dossier, { withFileTypes: true });
const jsons = entrees
  .filter((e) => e.isFile() && e.name.endsWith(".json"))
  .map((e) => path.join(dossier, e.name));
```

> `node:` prefix (ex : `node:fs/promises`) : explicite, évite les collisions avec des packages npm du même nom. Recommandé depuis Node 16+.

---

## 50. Lire et écrire des fichiers et du JSON

```typescript
import { readFile, writeFile } from "node:fs/promises";

interface InventaireServeur { nom: string; ip: string; }

// Lecture JSON typée (le chaînon manquant : JSON.parse → unknown, section 22)
async function lireInventaire(chemin: string): Promise<InventaireServeur[]> {
  const brut: unknown = JSON.parse(await readFile(chemin, "utf8"));
  if (!Array.isArray(brut)) throw new Error(`Format invalide: ${chemin}`);
  // Validation minimale champ par champ (ou Zod, section 68)
  return brut.map((e) => {
    if (typeof e !== "object" || e === null || !("nom" in e) || !("ip" in e)) {
      throw new Error(`Entrée invalide dans ${chemin}`);
    }
    const { nom, ip } = e as { nom: unknown; ip: unknown };
    if (typeof nom !== "string" || typeof ip !== "string") throw new Error("Types invalides");
    return { nom, ip };
  });
}

// Écriture
async function ecrireInventaire(chemin: string, data: InventaireServeur[]): Promise<void> {
  await writeFile(chemin, JSON.stringify(data, null, 2) + "\n", "utf8");
}

// Import JSON direct (resolveJsonModule: true) — pour la config statique
import config from "./config.json" with { type: "json" };
// Note : la syntaxe "with" (import attributes) = Node 20.10+ / TS 5.3+
```

**Streaming pour les gros fichiers** (logs multi-Go : ne jamais tout charger en RAM) :

```typescript
import { createReadStream } from "node:fs";
import { createInterface } from "node:readline";

async function* lignes(chemin: string): AsyncGenerator<string> {
  const rl = createInterface({ input: createReadStream(chemin), crlfDelay: Infinity });
  for await (const ligne of rl) yield ligne;
}

let erreurs = 0;
for await (const ligne of lignes("/var/log/app.log")) {
  if (ligne.includes("ERROR")) erreurs++;
}
console.log(`${erreurs} erreurs`);
```

---

## 51. CLI : process.argv, stdin/stdout, codes de sortie

```typescript
// args.ts — parsing minimal sans dépendance
const args = process.argv.slice(2); // ["--hote", "srv-01", "--port", "22"]

function lireOption(nom: string, defaut?: string): string | undefined {
  const i = args.indexOf(nom);
  if (i === -1) return defaut;
  return args[i + 1];
}

const hote = lireOption("--hote") ?? "localhost";
const port = Number(lireOption("--port", "22"));
```

**Conventions CLI à respecter** :

| Élément | Convention |
|---|---|
| Sortie normale | `console.log` → stdout |
| Erreurs / diagnostics | `console.error` → stderr |
| Succès | `process.exit(0)` (implicite) |
| Échec | `process.exit(1)` (ou 2 pour usage invalide) |
| `--help` / `-h` | Toujours implémenté |

```typescript
function aide(): never {
  console.log(`Usage: inventaire --hote <nom> [--port <n>]`);
  process.exit(2);
}
if (args.includes("--help") || args.includes("-h")) aide();

// Lecture stdin (pipe)
async function lireStdin(): Promise<string> {
  const morceaux: Buffer[] = [];
  for await (const chunk of process.stdin) morceaux.push(chunk as Buffer);
  return Buffer.concat(morceaux).toString("utf8");
}
// Usage : cat hosts.txt | mon-outil
```

> Pour un vrai CLI (sous-commandes, validation, help auto), utilisez **commander** ou **yargs** — typés nativement.

---

## 52. Réseau : fetch typé

Node 18+ a `fetch` global. Le typage de la **réponse** reste à votre charge (`response.json()` retourne `Promise<any>`) :

```typescript
interface SondageApi { charge: number; temperature: number; }

async function getSondage(baseUrl: string, id: string): Promise<SondageApi> {
  const res = await fetch(`${baseUrl}/api/sondes/${id}`, {
    headers: { "Accept": "application/json" },
  });
  if (!res.ok) {
    throw new Error(`HTTP ${res.status} ${res.statusText} sur ${id}`);
  }
  const data: unknown = await res.json();
  // Validation (ici manuelle, voir Zod section 68)
  if (typeof data !== "object" || data === null) throw new Error("Réponse invalide");
  return data as SondageApi;
}
```

**POST JSON typé** :

```typescript
async function postAlerte(url: string, alerte: { niveau: string; message: string }): Promise<void> {
  const res = await fetch(url, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(alerte),
  });
  if (!res.ok) throw new Error(`POST ${url}: HTTP ${res.status}`);
}
```

> `fetch` ne rejette **que** sur erreur réseau (DNS, connexion refusée). Un HTTP 500 **ne rejette pas** : toujours tester `res.ok`.

---

## 53. HTTP avancé : timeouts, retry, erreurs propres

```typescript
// Timeout via AbortController (le standard fetch)
async function fetchAvecTimeout(url: string, ms: number): Promise<Response> {
  const ctrl = new AbortController();
  const timer = setTimeout(() => ctrl.abort(new Error(`Timeout ${ms}ms`)), ms);
  try {
    return await fetch(url, { signal: ctrl.signal });
  } finally {
    clearTimeout(timer);
  }
}

// Retry avec backoff exponentiel — typé générique, réutilisable
export async function retry<T>(
  fn: () => Promise<T>,
  tentatives = 3,
  delaiMs = 500,
): Promise<T> {
  let derniere: unknown;
  for (let i = 0; i < tentatives; i++) {
    try {
      return await fn();
    } catch (e) {
      derniere = e;
      if (i < tentatives - 1) await new Promise((r) => setTimeout(r, delaiMs * 2 ** i));
    }
  }
  throw derniere;
}

// Usage : 3 tentatives, 500ms / 1s / 2s
const res = await retry(() => fetchAvecTimeout("http://sonde/api", 3000), 3);
```

**Hiérarchie d'erreurs métier** (mieux que des `Error` génériques) :

```typescript
class HttpError extends Error {
  constructor(public status: number, message: string) {
    super(message);
    this.name = "HttpError";
  }
}
class TimeoutError extends Error {
  constructor(public ms: number) { super(`Timeout après ${ms}ms`); this.name = "TimeoutError"; }
}

// Discrimination au catch (section 54)
try {
  await getSondage("http://x", "s1");
} catch (e) {
  if (e instanceof TimeoutError) { /* replanifier */ }
  else if (e instanceof HttpError && e.status === 404) { /* sonde inconnue */ }
  else throw e;
}
```

---

## 54. Gestion d'erreurs : try/catch moderne

Depuis TS 4.4, en `strict` (`useUnknownInCatchVariables`), la variable de `catch` est **`unknown`** — pas `any`. C'est une feature, pas un bug :

```typescript
try {
  await operationRisquee();
} catch (e) {
  // e: unknown → narrowing obligatoire
  if (e instanceof Error) {
    console.error(`Échec: ${e.message}`);
  } else {
    console.error(`Échec non-Error: ${String(e)}`);
  }
}
```

**Ne jamais laisser une promesse sans catch dans un CLI** :

```typescript
// main.ts — point d'entrée blindé
async function main(): Promise<void> {
  // ... logique
}

main().catch((e: unknown) => {
  console.error(e instanceof Error ? e.message : e);
  process.exit(1);
});
```

**Erreurs à ne pas avaler** : un `catch` vide ou un `catch { return null; }` masque les pannes. Logguez toujours, même brièvement. En cron, un script qui échoue **silencieusement** est pire qu'un script qui plante bruyamment.

---

## 55. Le pattern Result : gérer les erreurs sans exceptions

Alternative aux exceptions pour les erreurs **prévisibles** (validation, HTTP 404) : retourner un objet `Result` — le succès ou l'échec est explicite dans le type.

```typescript
type Result<T, E = Error> =
  | { ok: true; valeur: T }
  | { ok: false; erreur: E };

function diviser(a: number, b: number): Result<number, string> {
  if (b === 0) return { ok: false, erreur: "Division par zéro" };
  return { ok: true, valeur: a / b };
}

const r = diviser(10, 0);
if (r.ok) {
  console.log(r.valeur); // number
} else {
  console.error(r.erreur); // string — impossible d'oublier le cas d'échec
}
```

**Helpers** :

```typescript
function ok<T>(valeur: T): Result<T, never> { return { ok: true, valeur }; }
function ko<E>(erreur: E): Result<never, E> { return { ok: false, erreur }; }

// Convertir une promesse en Result (plus de try/catch au call-site)
async function tenter<T>(p: Promise<T>): Promise<Result<T>> {
  try { return ok(await p); }
  catch (e) { return ko(e instanceof Error ? e : new Error(String(e))); }
}

const res2 = await tenter(fetch("http://sonde/api"));
if (!res2.ok) { console.error("Sonde injoignable:", res2.erreur.message); process.exit(1); }
```

> Exceptions vs Result : exceptions pour l'**exceptionnel** (bugs, infra), Result pour l'**attendu** (validation, 404, timeout métier). Les deux cohabitent très bien.

---

## 56. Décorateurs : métaprogrammation déclarative

Les décorateurs (TS 5.0+, standard ECMAScript) ajoutent un comportement à une classe/méthode **déclarativement**. Cas d'usage : logging, mesure de temps, retry, validation.

```typescript
// tsconfig : "experimentalDecorators" n'est PLUS requis pour les décorateurs
// standard TS 5.0+ (il reste requis pour l'ancienne syntaxe expérimentale).

function Mesurer(
  _cible: unknown,
  nom: string,
  descripteur: PropertyDescriptor,
): PropertyDescriptor {
  const original = descripteur.value as (...args: unknown[]) => unknown;
  descripteur.value = function (this: unknown, ...args: unknown[]): unknown {
    const debut = Date.now();
    try {
      return original.apply(this, args);
    } finally {
      console.log(`${nom}: ${Date.now() - debut}ms`);
    }
  };
  return descripteur;
}

class Collecteur3 {
  @Mesurer
  collecter(hote: string): string {
    return `donnees de ${hote}`;
  }
}
```

**Décorateur de classe** (ex : enregistrer automatiquement un plugin) :

```typescript
const REGISTRE: Array<new () => unknown> = [];

function Plugin(nom: string) {
  return function (classe: new () => unknown): void {
    REGISTRE.push(classe);
    console.log(`Plugin enregistré: ${nom}`);
  };
}

@Plugin("snmp")
class CollecteurSnmpPlugin {}
```

> **Signal de version** : syntaxe standard = TS 5.0+. L'ancienne syntaxe (`experimentalDecorators: true`) reste supportée mais diverge du standard — ne mélangez pas les deux.

---

## 57. ts-node, tsx et l'exécution directe

Compiler puis exécuter (`tsc` + `node`) est lourd en développement. Trois exécuteurs directs :

| Outil | Moteur | Vérification des types | Usage |
|---|---|---|---|
| `ts-node` | tsc | Oui (lent, `--transpileOnly` pour aller vite) | `npx ts-node src/index.ts` |
| `tsx` | esbuild | **Non** (transpile seule) | `npx tsx src/index.ts` |
| `node --experimental-strip-types` | natif | Non | Node 22.6+ |

**Recommandation** : `tsx` en dev (instantané), `tsc --noEmit` en parallèle ou en CI pour la vérification :

```jsonc
// package.json
{
  "scripts": {
    "dev": "tsx watch src/index.ts",  // relance à chaque sauvegarde
    "build": "tsc",
    "start": "node dist/index.js",
    "check": "tsc --noEmit",
    "test": "vitest run"
  }
}
```

> ⚠️ `tsx` **ne vérifie pas les types** : une erreur de type passe silencieusement à l'exécution. Le `npm run check` en CI est donc obligatoire, pas optionnel.

---

## 58. ESLint + Prettier : setup complet

Prettier **formate**, ESLint **analyse**. Les deux ensemble = code homogène sans débats.

```bash
npm i -D eslint @eslint/js typescript-eslint prettier eslint-config-prettier
```

```javascript
// eslint.config.js (format "flat", ESLint 9+)
import js from "@eslint/js";
import tseslint from "typescript-eslint";
import prettier from "eslint-config-prettier";

export default tseslint.config(
  js.configs.recommended,
  ...tseslint.configs.recommendedTypeChecked, // règles avec vérification de types
  prettier, // désactive les règles de style en conflit avec Prettier
  {
    languageOptions: {
      parserOptions: {
        projectService: true,
        tsconfigRootDir: import.meta.dirname,
      },
    },
    rules: {
      "@typescript-eslint/no-explicit-any": "error",   // bannit any
      "@typescript-eslint/no-unused-vars": "error",
      "no-console": "off", // on est en CLI, console.* est légitime
    },
  },
);
```

```jsonc
// .prettierrc
{ "semi": true, "singleQuote": false, "printWidth": 100, "trailingComma": "all" }
```

```jsonc
// package.json
{ "scripts": { "lint": "eslint src tests", "format": "prettier --write src tests" } }
```

**Règles à activer en priorité** : `no-explicit-any`, `no-floating-promises` (promesse non awaitée = bug en puissance), `no-misused-promises`.

---

## 59. Tests unitaires avec Vitest

Vitest = runner moderne, compatible API Jest, support TS natif, rapide.

```bash
npm i -D vitest
```

```typescript
// src/seuils.ts
export function niveauAlerte(charge: number): "ok" | "warn" | "critique" {
  if (charge >= 90) return "critique";
  if (charge >= 75) return "warn";
  return "ok";
}

// tests/seuils.test.ts
import { describe, it, expect } from "vitest";
import { niveauAlerte } from "../src/seuils.js";

describe("niveauAlerte", () => {
  it("retourne ok sous 75", () => {
    expect(niveauAlerte(42)).toBe("ok");
  });

  it("gère les bornes exactes", () => {
    expect(niveauAlerte(75)).toBe("warn");
    expect(niveauAlerte(90)).toBe("critique");
  });

  it.each([
    [0, "ok"],
    [74.9, "ok"],
    [75, "warn"],
    [89.9, "warn"],
    [90, "critique"],
    [100, "critique"],
  ])("charge %i → %s", (charge, attendu) => {
    expect(niveauAlerte(charge)).toBe(attendu);
  });
});
```

```bash
npx vitest run          # une fois (CI)
npx vitest              # mode watch (dev)
npx vitest run --coverage # couverture (nécessite @vitest/coverage-v8)
```

**Matchers essentiels** : `toBe` (égalité stricte), `toEqual` (égalité profonde), `toThrow`, `toContain`, `resolves`/`rejects` pour les promesses.

---

## 60. Mocks, fixtures et tests async

```typescript
// src/sonde.ts
export async function chargeSonde(fetcher: (url: string) => Promise<{ charge: number }>, id: string) {
  const data = await fetcher(`http://sondes/${id}`);
  return data.charge;
}

// tests/sonde.test.ts — injection de dépendance : pas de mock framework requis
import { it, expect, vi } from "vitest";
import { chargeSonde } from "../src/sonde.js";

it("remonte la charge", async () => {
  const fauxFetcher = vi.fn(async (_url: string) => ({ charge: 66 }));
  await expect(chargeSonde(fauxFetcher, "s1")).resolves.toBe(66);
  expect(fauxFetcher).toHaveBeenCalledWith("http://sondes/s1");
});

it("propage les erreurs réseau", async () => {
  const enPanne = vi.fn(async (_url: string): Promise<{ charge: number }> => {
    throw new Error("connexion refusée");
  });
  await expect(chargeSonde(enPanne, "s1")).rejects.toThrow("connexion refusée");
});
```

**Bonnes pratiques de test** :
- Testez les **fonctions pures** en priorité (seuils, parsers, formateurs) : rapides, déterministes.
- **Injectez** les dépendances I/O (fetch, fs) en paramètres plutôt que de mocker des modules.
- Un test qui dépend du réseau ou de l'heure est un test **fragile** : isolez l'horloge (`vi.useFakeTimers()`).
- Visez les **cas limites** : bornes, vide, `null`, unicode, timeouts.

---

## 61. Debug : VS Code, source maps, inspector

**launch.json** — debugger le TypeScript source directement (grâce à `sourceMap: true`) :

```jsonc
// .vscode/launch.json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Debug src/index.ts",
      "runtimeExecutable": "npx",
      "runtimeArgs": ["tsx", "--inspect", "src/index.ts"],
      "args": ["--hote", "srv-01"],
      "console": "integratedTerminal"
    }
  ]
}
```

**Techniques de debug par situation :**

| Situation | Outil |
|---|---|
| Script qui plante en dev | Breakpoints VS Code + `tsx` |
| Plantage en prod (dist/) | `node --enable-source-maps dist/index.js` + stack trace mappée |
| Fuite mémoire / perf | `node --inspect` + Chrome DevTools (onglet Memory/Profiler) |
| Promesse qui ne se termine pas | `--trace-warnings`, `process.on("unhandledRejection")` |
| Comportement étrange des types | Survolez le type dans VS Code (`Ctrl+K, Ctrl+I`) |

**Garde-fou global** (à mettre dans `main.ts`) :

```typescript
process.on("unhandledRejection", (raison) => {
  console.error("Promesse non gérée:", raison);
  process.exit(1);
});
```

> En prod, les logs structurés (JSON) valent mieux que `console.log` : envisagez `pino` (rapide, typé).

---

## 62. React + TypeScript : l'essentiel

Même si votre cœur de métier est sysadmin, un **dashboard interne** en React+TS est un cas classique. Les 20% qui couvrent 80% des besoins :

```typescript
// Props typées
interface CarteSondeProps {
  nom: string;
  charge: number;
  onAlerte?: (nom: string) => void; // callback optionnel
}

function CarteSonde({ nom, charge, onAlerte }: CarteSondeProps) {
  return (
    <div onClick={() => onAlerte?.(nom)}>
      {nom} — {charge}%
    </div>
  );
}

// useState typé (inférence suffit souvent)
import { useState, useEffect } from "react";

function ListeSondes() {
  const [sondes, setSondes] = useState<string[]>([]); // annotation requise (init vide)
  const [erreur, setErreur] = useState<string | null>(null);

  useEffect(() => {
    let actif = true;
    fetch("/api/sondes")
      .then((r) => r.json())
      .then((d: unknown) => { if (actif) setSondes(d as string[]); })
      .catch((e: unknown) => { if (actif) setErreur(String(e)); });
    return () => { actif = false; }; // cleanup anti setState après unmount
  }, []);

  if (erreur) return <div>Erreur: {erreur}</div>;
  return <ul>{sondes.map((s) => <li key={s}>{s}</li>)}</ul>;
}
```

**Règles React+TS** :
- `useState<string[]>([])` : annotez quand l'initial est vide/ambigu.
- Événements : `(e: React.ChangeEvent<HTMLInputElement>) => ...`, `(e: React.FormEvent) => ...`.
- `useRef<HTMLInputElement>(null)` pour le DOM.
- **Ne mettez jamais `any`** dans les props : le composant devient un piège pour ses utilisateurs.

---

## 63. Les 15 erreurs du compilateur les plus fréquentes (expliquées + corrigées)

| # | Code | Message typique | Cause | Correction |
|---|---|---|---|---|
| 1 | TS2322 | `Type 'X' is not assignable to type 'Y'` | Affectation incompatible | Aligner les types ou narrower avant |
| 2 | TS2339 | `Property 'x' does not exist on type 'Y'` | Faute de frappe ou union non narrowée | Corriger le nom / ajouter un narrowing |
| 3 | TS2345 | `Argument of type 'X' is not assignable to parameter of type 'Y'` | Mauvais argument | Vérifier l'ordre et le type des args |
| 4 | TS18048 | `'x' is possibly 'undefined'` | `strictNullChecks` + accès non gardé | `if (x)`, `?.`, `??`, assertion |
| 5 | TS2531 | `Object is possibly 'null'` | Idem avec `null` | Narrowing explicite |
| 6 | TS7006 | `Parameter 'x' implicitly has an 'any' type` | `noImplicitAny` | Annoter le paramètre |
| 7 | TS2454 | `Variable 'x' is used before being assigned` | Assignation conditionnelle non prouvée | Initialiser ou restructurer |
| 8 | TS2564 | `Property 'x' has no initializer` | `strictPropertyInitialization` | Initialiser, `?`, ou `!` assumé |
| 9 | TS2571 | `Object is of type 'unknown'` | Usage direct d'un `unknown` | Narrowing (typeof/in/instanceof) |
| 10 | TS2741 | `Property 'x' is missing in type ... but required` | Objet littéral incomplet | Ajouter la propriété ou la rendre optionnelle |
| 11 | TS2769 | `No overload matches this call` | Aucune surcharge ne correspond | Vérifier les combinaisons d'args |
| 12 | TS2349 | `This expression is not callable` | Appeler quelque chose qui n'est pas une fonction (`string \| (() => void)` non narrowé) | Narrower avec `typeof x === "function"` |
| 13 | TS2304 | `Cannot find name 'x'` | Faute de frappe / import manquant / mauvaise portée | Importer ou corriger |
| 14 | TS1192 | `Module has no default export` | `import X from` sans default | Utiliser l'import nommé |
| 15 | TS2307 | `Cannot find module './x'` | Mauvais chemin / extension manquante (NodeNext exige `.js`) | Corriger le chemin + extension |

**Exemples corrigés :**

```typescript
// ❌ TS18048: 'port' is possibly 'undefined'
declare const cfg: { port?: number };
const p1 = cfg.port.toFixed(); // Erreur

// ✅ Corrections possibles
const p2 = cfg.port?.toFixed();          // string | undefined
const p3 = (cfg.port ?? 443).toFixed();  // string — valeur par défaut
if (cfg.port !== undefined) {
  const p4 = cfg.port.toFixed();          // string — narrowing
}

// ❌ TS2339 sur union
declare const r: { ok: true; v: number } | { ok: false; e: string };
console.log(r.v); // Erreur : 'v' n'existe pas sur le 2e membre
// ✅
if (r.ok) console.log(r.v); else console.error(r.e);

// ❌ TS7006
const dbl = [1, 2].map((x) => x * 2); // OK ici (x inféré), mais :
function traiter2(x) { return x; }     // TS7006 en strict
// ✅
function traiter2(x: unknown): unknown { return x; }
```

**Méthode de lecture d'une erreur TS** : 1) le code (TSxxxx) → 2) *ce que* le compilateur attendait → 3) *pourquoi* votre valeur ne convient pas → 4) corrigez la **cause** (le type), pas le symptôme (pas de `as any` pour faire taire).

---

## 64. Erreurs classiques des débutants (12, avec corrections)

### 1. Utiliser `any` comme rustine
```typescript
const data: any = JSON.parse(brut); // ❌ contamine tout le fichier
const data2: unknown = JSON.parse(brut); // ✅ puis narrowing / Zod
```

### 2. Oublier que `fetch` ne rejette pas sur HTTP 500
Toujours tester `res.ok` (section 52).

### 3. `forEach` + `async` sans await
`forEach` n'attend pas : utilisez `for...of` ou `Promise.all` (section 43).

### 4. Comparer avec `==` au lieu de `===`
```typescript
if (port == "22") {} // ❌ true par coercition — en TS on veut du strict
if (port === 22) {}  // ✅
```

### 5. Muter un paramètre par défaut partagé
```typescript
function f(tags: string[] = []): string[] { // ✅ OK : nouveau tableau à chaque appel
  tags.push("x"); return tags;
}
// ⚠️ En revanche, un objet mutable en variable module = état global caché.
```

### 6. Confondre `interface` et valeur au runtime
```typescript
interface S { nom: string; }
const x = new S(); // ❌ Impossible : une interface n'existe pas au runtime
// Les interfaces sont effacées. Pour instancier : class. Pour valider : Zod.
```

### 7. Oublier l'extension `.js` dans les imports (module NodeNext)
```typescript
import { x } from "./utils";   // ❌ TS2307 avec moduleResolution NodeNext
import { x } from "./utils.js"; // ✅
```

### 8. Croire que `private` protège au runtime
`private` est compile-time. Donnée vraiment sensible → `#privé` ou ne pas l'exposer.

### 9. Retourner `undefined` implicitement dans tous les chemins
```typescript
function chercher(nom: string): Serveur { // ❌ TS: pas de retour dans tous les cas
  if (nom) { /* ... */ return srv; }
}
function chercher2(nom: string): Serveur | undefined { // ✅ type honnête
  if (nom) { /* ... */ return srv; }
  return undefined;
}
```

### 10. Négliger `noUncheckedIndexedAccess`
`tableau[i]` peut être `undefined` : en prod, c'est un crash `Cannot read properties of undefined`.

### 11. Mettre la logique métier dans le `catch`
Le `catch` gère l'échec, il ne continue pas le traitement nominal. Structure : try = nominal, catch = dégradé/log, finally = nettoyage.

### 12. Ne pas typer les variables d'environnement
`process.env.PORT` est `string | undefined` : validez au démarrage (section 47 + Zod) au lieu de découvrir le `undefined` en plein run.

---

## 65. Bonnes pratiques & style

**Checklist qualité avant chaque commit :**
- [ ] `npm run check` (tsc --noEmit) passe
- [ ] `npm run lint` passe, zéro `any` explicite
- [ ] `npm run test` passe
- [ ] Prettier appliqué (`npm run format`)
- [ ] Pas de `console.log` de debug oublié (ou logger structuré)

**Conventions de nommage :**

| Élément | Convention | Exemple |
|---|---|---|
| Variables, fonctions | camelCase | `nbTentatives`, `lireInventaire()` |
| Classes, interfaces, types, enums | PascalCase | `Serveur`, `NiveauAlerte` |
| Constantes globales | UPPER_SNAKE | `SEUIL_CRITIQUE` |
| Fichiers | kebab-case | `collecte-snmp.ts` |
| Booléens | préfixe `is/has/peut` | `isEnLigne`, `hasErreur` |

**Principes de design :**
1. **Types honnêtes** : si ça peut être `undefined`, écrivez `| undefined`. Le type est un contrat.
2. **Petites fonctions pures** : parse, formate, calcule → testables sans mock.
3. **I/O aux frontières** : `main()` orchestre ; la logique ne touche ni `fs` ni `fetch` directement (injection, section 60).
4. **Échec bruyant** : jamais de `catch` vide ; exit code ≠ 0 en cas d'échec (cron/supervision).
5. **Un seul `as`** par validation, jamais en chaîne : `as unknown as T` = aveu d'échec du typage.
6. **TSDoc** sur les fonctions publiques (`/** ... */` avec `@param`, `@returns`, `@throws`).

```typescript
/**
 * Calcule le niveau d'alerte à partir d'une charge en %.
 * @param charge - Charge mesurée (0-100)
 * @returns "ok" | "warn" | "critique"
 * @throws {RangeError} Si charge hors de [0, 100]
 */
export function niveauAlerte(charge: number): "ok" | "warn" | "critique" {
  if (charge < 0 || charge > 100) throw new RangeError(`Charge invalide: ${charge}`);
  if (charge >= 90) return "critique";
  if (charge >= 75) return "warn";
  return "ok";
}
```

---

## 66. Performance : compilation et runtime

**Compilation plus rapide :**
- `skipLibCheck: true` (ignore les `.d.ts` tiers) — gain majeur.
- `isolatedModules: true` + `tsx`/esbuild en dev (pas de vérification, transpile seule).
- Évitez les types **récursifs profonds** et les unions géantes (>10k membres) : le checker peut exploser.
- `tsc --noEmit` en CI ; en dev, laissez l'IDE vérifier à la volée.

**Runtime : les types coûtent zéro** (effacés). Les vrais leviers :
- **Éviter les `await` séquentiels** quand l'ordre n'importe pas : `Promise.all` (section 44).
- **Streamer** les gros fichiers au lieu de `readFile` (section 50).
- **Limiter la concurrence** sur les I/O massives (ne pas lancer 10 000 fetch en parallèle) :

```typescript
// Parallélisme borné : simple et robuste
export async function paralleleBorne<T, R>(
  items: T[],
  limite: number,
  fn: (item: T) => Promise<R>,
): Promise<R[]> {
  const resultats: R[] = new Array(items.length);
  let index = 0;
  async function worker(): Promise<void> {
    while (index < items.length) {
      const i = index++;
      resultats[i] = await fn(items[i] as T);
    }
  }
  await Promise.all(Array.from({ length: Math.min(limite, items.length) }, worker));
  return resultats;
}

// 500 hôtes, 20 pings simultanés max
const latences = await paralleleBorne(hosts, 20, (h) => mesurerLatence(h));
```

- `for...of` > `.forEach` + closure en boucle chaude (micro-opt, rarement décisif).
- JSON : `JSON.parse` sur des payloads énormes = synchrone et bloquant ; pour du très gros, streamer (ndjson).

---

## 67. Sécurité : valider à la frontière

TypeScript **ne protège que le code que vous écrivez**. Tout ce qui vient de l'extérieur (argv, env, JSON, API, stdin) est **hostile jusqu'à validation**.

**Checklist sécurité d'un outil interne :**
- [ ] Valider `process.env` au démarrage (présence + format), échouer vite si invalide.
- [ ] Valider tout JSON entrant (fichier, API) avec Zod ou des guards (section 68).
- [ ] Ne jamais interpoler une entrée utilisateur dans une commande shell → utilisez `execFile` (args séparés), jamais `exec` avec concaténation.
- [ ] Timeouts sur tout appel réseau (section 53) — un hang = un cron bloqué.
- [ ] Ne jamais logger de secrets (tokens, mots de passe) ; masquez-les (`***`).
- [ ] `npm audit` régulier ; `npm ci` (pas `npm install`) en CI/prod pour figer le lockfile.
- [ ] Fichiers sensibles (clés) : permissions `0o600`, jamais dans le repo.

```typescript
import { execFile } from "node:child_process";
import { promisify } from "node:util";
const execFileAsync = promisify(execFile);

// ✅ Arguments séparés : pas d'injection shell possible
async function pingSys(hote: string): Promise<string> {
  if (!/^[a-zA-Z0-9.-]+$/.test(hote)) throw new Error(`Hôte invalide: ${hote}`);
  const { stdout } = await execFileAsync("ping", ["-c", "1", "-W", "2", hote]);
  return stdout;
}

// ❌ JAMAIS : `exec(`ping -c 1 ${hote}`)` — injection si hote = "x; rm -rf /"
```

---

## 68. Zod : validation runtime + types inférés

Zod (ou Valibot/ArkType) résout le dilemme « `as` menteur vs validation manuelle verbeuse » : **un schéma = validation runtime + type statique inféré**.

```bash
npm i zod
```

```typescript
import { z } from "zod";

// 1. Schéma (runtime)
const ServeurSchema = z.object({
  nom: z.string().min(1),
  ip: z.string().ip({ version: "v4" }),
  port: z.number().int().min(1).max(65535).default(22),
  tags: z.array(z.string()).default([]),
});

// 2. Type inféré (statique) — source unique de vérité !
type ServeurValide = z.infer<typeof ServeurSchema>;
// { nom: string; ip: string; port: number; tags: string[] }

// 3. Validation à la frontière
function parseInventaire(brut: unknown): ServeurValide[] {
  return z.array(ServeurSchema).parse(brut); // throw ZodError si invalide
}

// Version non-throw (pattern Result, section 55)
const res4 = z.array(ServeurSchema).safeParse(brut);
if (!res4.success) {
  console.error(res4.error.issues.map((i) => `${i.path.join(".")}: ${i.message}`));
  process.exit(1);
}
const serveurs: ServeurValide[] = res4.data; // typé et VALIDÉ
```

**Schémas utiles en sysadmin :**

```typescript
const EnvSchema = z.object({
  PORT: z.coerce.number().int().min(1).max(65535).default(3000),
  SONDE_API_URL: z.string().url(),
  LOG_LEVEL: z.enum(["debug", "info", "warn", "error"]).default("info"),
});
export const ENV = EnvSchema.parse(process.env); // échoue vite au démarrage
// z.coerce.number() convertit "3000" -> 3000 : parfait pour les env vars
```

> **Règle** : Zod (ou équivalent) à **chaque frontière** : argv, env, fichiers JSON, réponses HTTP, webhooks. À l'intérieur, TypeScript suffit.

---

## 69. Publier un package typé sur npm

Pour qu'un package soit agréable à consommer en TS :

**1. tsconfig de build (lib)** :
```jsonc
{
  "compilerOptions": {
    "target": "ES2022", "module": "NodeNext", "moduleResolution": "NodeNext",
    "outDir": "dist", "rootDir": "src",
    "declaration": true,          // ← génère les .d.ts
    "declarationMap": true,       // ← map vers les sources
    "sourceMap": true,
    "strict": true, "noEmitOnError": true,
    "verbatimModuleSyntax": true  // ← force import type / import valeur
  },
  "include": ["src"]
}
```

**2. package.json** — les champs `types`/`exports` :
```jsonc
{
  "name": "@mon-org/outils-reseau",
  "version": "1.0.0",
  "type": "module",
  "main": "./dist/index.js",
  "types": "./dist/index.d.ts",
  "exports": {
    ".": { "types": "./dist/index.d.ts", "default": "./dist/index.js" }
  },
  "files": ["dist"],
  "scripts": {
    "build": "tsc",
    "prepublishOnly": "npm run build && npm run check && npm run test"
  }
}
```

**3. Checklist publication :**
- [ ] `declaration: true` → les consommateurs ont l'autocomplétion sans `@types`.
- [ ] `verbatimModuleSyntax` → pas d'imports de types fantômes au runtime.
- [ ] README avec exemples typés + matrice de compatibilité Node.
- [ ] `npm publish --dry-run` pour inspecter le contenu avant envoi.
- [ ] Versionnage semver strict : un changement de type public = **breaking** (majeur) ou au minimum mineur documenté.
- [ ] `attw --pack` (package `@arethetypeswrong/cli`) pour vérifier que les types se résolvent correctement côté consommateur.

> Alternative sans publication : un **monorepo** avec `paths`/`references` TypeScript (`tsc -b`) pour partager le code typé en interne sans npm.

---

## 70. Cas pratique 1 : CLI d'inventaire réseau

**Besoin** : lire un CSV d'équipements, pinger chacun (parallélisme borné), sortir un JSON + un résumé. Tout ce qu'on a vu, assemblé.

```typescript
// src/inventaire.ts
import { readFile, writeFile } from "node:fs/promises";
import { z } from "zod";
import { paralleleBorne } from "./concurrence.js"; // section 66
import { pingSys } from "./reseau.js";             // section 67 (execFile)

// --- 1. Modèle validé à la frontière (Zod) ---
const EquipementSchema = z.object({
  nom: z.string().min(1),
  ip: z.string().ip({ version: "v4" }),
});
type Equipement = z.infer<typeof EquipementSchema>;

// --- 2. Parsing CSV : string -> unknown -> validé ---
function parseCsv(contenu: string): Equipement[] {
  const lignes = contenu.split("\n").map((l) => l.trim()).filter(Boolean);
  return lignes.map((ligne, i) => {
    const [nom = "", ip = ""] = ligne.split(";");
    const res = EquipementSchema.safeParse({ nom: nom.trim(), ip: ip.trim() });
    if (!res.success) {
      throw new Error(`Ligne ${i + 1} invalide: ${res.error.issues[0]?.message}`);
    }
    return res.data;
  });
}

// --- 3. Résultat typé en union discriminée ---
type ResultatSonde =
  | { nom: string; ip: string; ok: true; latenceMs: number }
  | { nom: string; ip: string; ok: false; erreur: string };

async function sonder(e: Equipement): Promise<ResultatSonde> {
  try {
    const debut = Date.now();
    await pingSys(e.ip);
    return { nom: e.nom, ip: e.ip, ok: true, latenceMs: Date.now() - debut };
  } catch (err) {
    return {
      nom: e.nom, ip: e.ip, ok: false,
      erreur: err instanceof Error ? err.message : String(err),
    };
  }
}

// --- 4. Orchestration ---
export async function inventaire(entreeCsv: string, sortieJson: string): Promise<void> {
  const equipements = parseCsv(await readFile(entreeCsv, "utf8"));
  console.error(`Sondage de ${equipements.length} équipements…`);

  const resultats = await paralleleBorne(equipements, 20, sonder);

  await writeFile(sortieJson, JSON.stringify(resultats, null, 2) + "\n");

  // Résumé avec narrowing sur l'union discriminée
  const ok = resultats.filter((r) => r.ok);
  const ko = resultats.filter((r) => !r.ok);
  console.log(`OK: ${ok.length} | KO: ${ko.length}`);
  for (const r of ko) {
    if (!r.ok) console.log(`  ✗ ${r.nom} (${r.ip}): ${r.erreur}`);
  }
  if (ko.length > 0) process.exitCode = 1; // échec bruyant pour la supervision
}

// src/index.ts
import { inventaire } from "./inventaire.js";

const [csv = "equipements.csv", json = "resultats.json"] = process.argv.slice(2);
inventaire(csv, json).catch((e: unknown) => {
  console.error(e instanceof Error ? e.message : e);
  process.exit(1);
});
```

**Ce que ce cas pratique démontre** : validation Zod à la frontière, union discriminée + narrowing, parallélisme borné, `execFile` sécurisé, exit codes propres, séparation parsing / métier / orchestration.

---

## 71. Cas pratique 2 : client API de supervision avec cache

**Besoin** : interroger une API de supervision (métriques d'onduleurs), mettre en cache avec TTL, gérer timeout + retry. Réutilisable dans un dashboard.

```typescript
// src/client-supervision.ts
import { z } from "zod";
import { retry } from "./retry.js";           // section 53
import { avecTimeout } from "./timeout.js";   // section 44

// --- Schémas : le contrat de l'API, validé au runtime ---
const MesureSchema = z.object({
  horodatage: z.string().datetime(),
  chargePct: z.number().min(0).max(100),
  tensionV: z.number().positive(),
  autonomieMin: z.number().nonnegative(),
});
const ReponseSchema = z.object({ mesures: z.array(MesureSchema) });

type Mesure = z.infer<typeof MesureSchema>;

interface OptionsClient {
  baseUrl: string;
  token: string;
  timeoutMs?: number;
  ttlCacheMs?: number;
}

export class ClientSupervision {
  private cache = new Map<string, { expire: number; data: Mesure[] }>();
  private timeoutMs: number;
  private ttl: number;

  constructor(private opts: OptionsClient) {
    this.timeoutMs = opts.timeoutMs ?? 5000;
    this.ttl = opts.ttlCacheMs ?? 60_000;
  }

  private async get<T>(chemin: string, schema: z.ZodType<T>): Promise<T> {
    const url = `${this.opts.baseUrl}${chemin}`;
    const res = await retry(
      () =>
        avecTimeout(
          fetch(url, { headers: { Authorization: `Bearer ${this.opts.token}` } }),
          this.timeoutMs,
        ),
      3,
    );
    if (res.status === 401) throw new Error("Token invalide (401)");
    if (!res.ok) throw new Error(`HTTP ${res.status} sur ${chemin}`);
    return schema.parse(await res.json()); // throw ZodError si contrat rompu
  }

  async mesures(onduleurId: string): Promise<Mesure[]> {
    const enCache = this.cache.get(onduleurId);
    if (enCache && enCache.expire > Date.now()) return enCache.data;

    const data = await this.get(
      `/api/onduleurs/${encodeURIComponent(onduleurId)}/mesures`,
      ReponseSchema,
    );
    this.cache.set(onduleurId, { expire: Date.now() + this.ttl, data: data.mesures });
    return data.mesures;
  }

  async derniereCharge(onduleurId: string): Promise<number | null> {
    const mesures = await this.mesures(onduleurId);
    return mesures.at(-1)?.chargePct ?? null; // .at(-1) : ES2022
  }

  invalider(onduleurId: string): void {
    this.cache.delete(onduleurId);
  }
}
```

**Points à noter** :
- `encodeURIComponent` sur les segments d'URL (injection évitée).
- Le token ne transite que dans le header `Authorization`, jamais dans l'URL (les URLs finissent dans les logs).
- Cache en `Map` avec TTL : simple, suffisant pour un outil interne (pas besoin de Redis à cette échelle).
- `z.ZodType<T>` rend `get` générique sur n'importe quel schéma.

---

## 72. Cas pratique 3 : parseur de logs avec alertes

**Besoin** : analyser un fichier de log en streaming (gros volume), détecter des motifs, émettre des alertes typées, sortir un rapport.

```typescript
// src/parse-logs.ts
import { lignes } from "./streaming.js"; // section 50 (AsyncGenerator)

// --- Événements : union discriminée (section 26) ---
type EvenementLog =
  | { type: "erreur"; heure: string; service: string; message: string }
  | { type: "timeout"; heure: string; service: string; dureeMs: number }
  | { type: "auth"; heure: string; utilisateur: string; succes: boolean };

// Format attendu : "2026-09-26T10:00:00Z [svc-ups] ERROR: surcharge détectée"
const REGEX = /^(\S+) \[([^\]]+)\] (ERROR|TIMEOUT \d+ms|AUTH (OK|KO) \S+): (.*)$/;

function parseLigne(ligne: string): EvenementLog | null {
  const m = REGEX.exec(ligne);
  if (!m) return null;
  const [, heure = "", service = "", code = "", message = ""] = m;
  if (code === "ERROR") return { type: "erreur", heure, service, message };
  if (code.startsWith("TIMEOUT")) {
    return { type: "timeout", heure, service, dureeMs: Number(code.split(" ")[1]?.replace("ms", "")) || 0 };
  }
  const ok = code.includes("OK");
  return { type: "auth", heure, utilisateur: message.trim(), succes: ok };
}

export interface Rapport {
  totalLignes: number;
  erreurs: number;
  timeouts: number;
  echecsAuth: number;
  topServicesEnErreur: Array<{ service: string; nb: number }>;
}

export async function analyser(chemin: string): Promise<Rapport> {
  let totalLignes = 0, erreurs = 0, timeouts = 0, echecsAuth = 0;
  const parService = new Map<string, number>();

  for await (const ligne of lignes(chemin)) {
    totalLignes++;
    const evt = parseLigne(ligne);
    if (!evt) continue;
    switch (evt.type) {
      case "erreur":
        erreurs++;
        parService.set(evt.service, (parService.get(evt.service) ?? 0) + 1);
        break;
      case "timeout":
        timeouts++;
        break;
      case "auth":
        if (!evt.succes) echecsAuth++;
        break;
      default: {
        const impossible: never = evt; // exhaustivité (section 26)
        throw new Error(`Non géré: ${JSON.stringify(impossible)}`);
      }
    }
  }

  const topServicesEnErreur = [...parService.entries()]
    .map(([service, nb]) => ({ service, nb }))
    .sort((a, b) => b.nb - a.nb)
    .slice(0, 5);

  return { totalLignes, erreurs, timeouts, echecsAuth, topServicesEnErreur };
}
```

**Pourquoi ce design tient la route en prod** : streaming (mémoire constante même sur 10 Go de logs), regex ancrée (`^...$`, pas de catastrophic backtracking ici car pas de quantificateurs imbriqués), exhaustivité garantie par `never`, rapport sérialisable en JSON tel quel.

---

## 73. Cas pratique 4 : agrégateur de métriques pour dashboard

**Besoin** : agréger des mesures de plusieurs onduleurs (min/max/moyenne par tranche de 5 min) pour alimenter un dashboard. Pur, testable, sans I/O.

```typescript
// src/agregation.ts — 100% pur : testable sans mock
export interface MesureBrute {
  onduleur: string;
  horodatage: string; // ISO
  chargePct: number;
}

export interface Tranche {
  debut: string; // ISO, arrondi à 5 min
  nbMesures: number;
  min: number;
  max: number;
  moyenne: number;
}

export function arrondir5min(iso: string): string {
  const d = new Date(iso);
  d.setSeconds(0, 0);
  d.setMinutes(d.getMinutes() - (d.getMinutes() % 5));
  return d.toISOString();
}

export function agreger(mesures: MesureBrute[]): Tranche[] {
  const groupes = new Map<string, number[]>();
  for (const m of mesures) {
    const cle = arrondir5min(m.horodatage);
    const g = groupes.get(cle);
    if (g) g.push(m.chargePct);
    else groupes.set(cle, [m.chargePct]);
  }
  return [...groupes.entries()]
    .map(([debut, vals]) => ({
      debut,
      nbMesures: vals.length,
      min: Math.min(...vals),
      max: Math.max(...vals),
      moyenne: Math.round((vals.reduce((s, v) => s + v, 0) / vals.length) * 10) / 10,
    }))
    .sort((a, b) => a.debut.localeCompare(b.debut));
}

// tests/agregation.test.ts
import { it, expect } from "vitest";
import { agreger } from "../src/agregation.js";

it("agrège par tranche de 5 minutes", () => {
  const tranches = agreger([
    { onduleur: "ups-01", horodatage: "2026-09-26T10:01:00Z", chargePct: 40 },
    { onduleur: "ups-01", horodatage: "2026-09-26T10:03:00Z", chargePct: 60 },
    { onduleur: "ups-01", horodatage: "2026-09-26T10:07:00Z", chargePct: 80 },
  ]);
  expect(tranches).toHaveLength(2);
  expect(tranches[0]).toMatchObject({ min: 40, max: 60, moyenne: 50, nbMesures: 2 });
  expect(tranches[1]).toMatchObject({ min: 80, max: 80, moyenne: 80, nbMesures: 1 });
});
```

**Leçon** : séparer le **calcul pur** (testé en millisecondes) de la **collecte I/O** (ClientSupervision, section 71). Le dashboard (React, section 62) consomme les `Tranche[]` via une API qui expose ce résultat.

---

## 74. Migrer un projet JavaScript vers TypeScript

**Stratégie incrémentale** (ne jamais tout réécrire d'un coup) :

**Étape 1 — cohabitation** : autorisez le JS dans le projet TS.
```jsonc
// tsconfig.json
{ "compilerOptions": { "allowJs": true, "checkJs": false, "strict": true } }
```
Renommez `index.js` → `index.ts` progressivement, en commençant par les feuilles (utilitaires sans dépendances).

**Étape 2 — JSDoc d'abord** : sur les fichiers encore en `.js`, ajoutez des annotations JSDoc — `tsc` les comprend avec `checkJs: true` :
```javascript
// @ts-check
/**
 * @param {string} hote
 * @returns {Promise<number>}
 */
async function ping(hote) { /* ... */ }
```

**Étape 3 — renommage par vagues** : `.js` → `.ts`, corrigez les erreurs fichier par fichier. Mettez `// @ts-nocheck` en haut des fichiers pas encore migrés pour garder un build vert.

**Étape 4 — durcissement** : une fois tout en `.ts`, retirez `allowJs`, activez `noUncheckedIndexedAccess`, puis les règles ESLint strictes.

**Checklist migration :**
- [ ] Un seul fichier migré à la fois, tests verts entre chaque vague
- [ ] Les frontières (I/O, API) typées en premier — c'est là que les bugs se cachent
- [ ] `any` temporaires marqués `// TODO(ts-migration): typer` pour ne pas les oublier
- [ ] Pas de changement fonctionnel pendant la migration (même comportement, juste des types)

---

## 75. Pense-bête de poche

### Commandes
```bash
npx tsc --noEmit          # vérifier les types (CI)
npx tsc                   # compiler vers dist/
npx tsx src/index.ts      # exécuter direct (dev, sans vérif)
npx tsx watch src/index.ts # dev avec relance auto
npx vitest run            # tests une fois
npx eslint src tests      # lint
```

### Snippets à copier
```typescript
// Main CLI blindé
async function main(): Promise<void> { /* ... */ }
main().catch((e: unknown) => {
  console.error(e instanceof Error ? e.message : e);
  process.exit(1);
});

// Env validée
import { z } from "zod";
const ENV = z.object({
  PORT: z.coerce.number().default(3000),
  API_URL: z.string().url(),
}).parse(process.env);

// fetch avec timeout + retry (sections 53-54)
const res = await retry(() => fetchAvecTimeout(url, 3000), 3);
if (!res.ok) throw new Error(`HTTP ${res.status}`);

// Dictionnaire exhaustif
const SEUILS: Record<"warn" | "critique", number> = { warn: 75, critique: 90 };

// Union discriminée + exhaustivité
switch (e.type) {
  case "a": /* ... */ break;
  default: { const x: never = e; throw new Error("non géré"); }
}

// Narrowing unknown (données externes)
if (typeof v === "object" && v !== null && "port" in v) { /* ... */ }

// Parallélisme borné
await paralleleBorne(items, 20, (x) => traiter(x));
```

### Tableau des opérateurs de types
| Syntaxe | Nom | Exemple |
|---|---|---|
| `A \| B` | Union | `string \| number` |
| `A & B` | Intersection | `{a} & {b}` |
| `T[]` / `Array<T>` | Tableau | `string[]` |
| `[A, B]` | Tuple | `[string, number]` |
| `keyof T` | Clés | `keyof Config` |
| `T[K]` | Accès indexé | `Config["port"]` |
| `typeof v` | Type d'une valeur | `typeof CONFIG` |
| `T extends U ? A : B` | Conditionnel | `Awaited<T>` |
| `` `a${T}` `` | Template literal | `` `sonde:${string}` `` |
| `infer R` | Capture | `ReturnType<F>` |
| `Partial/Pick/Omit/Record` | Utilitaires | `Partial<Config>` |

---

## 76. Glossaire

| Terme | Définition |
|---|---|
| **Annotation** | Type écrit explicitement (`: string`) |
| **Inférence** | Type deviné par le compilateur |
| **Narrowing** | Affinage d'une union via un test (`typeof`, `in`, `instanceof`) |
| **Type guard** | Fonction prédicat `x is T` qui narrow |
| **Assertion** | `as T` : forcer un type (sans contrôle runtime) |
| **Union discriminée** | Union d'objets avec champ discriminant commun |
| **Générique** | Type paramétré (`Array<T>`, `Promise<T>`) |
| **Utility type** | Type utilitaire natif (`Partial`, `Pick`…) |
| **Mapped type** | Type construit en itérant sur des clés (`{ [K in keyof T]: … }`) |
| **Conditional type** | `T extends U ? A : B` |
| **Declaration merging** | Fusion de plusieurs déclarations `interface` du même nom |
| **Ambient** | Déclaration `.d.ts` : types sans implémentation |
| **Branded type** | `string & { __brand }` : distinguer des types structurellement identiques |
| **Exhaustiveness** | Vérifier via `never` que tous les cas d'une union sont traités |
| **Transpilation** | Conversion TS → JS (les types sont effacés) |
| **Erasure** | Les types n'existent pas au runtime |
| **Soundness** | Garantie qu'un typage est correct (TS est *unsound* par endroits : `as`, tableaux covariants — assumé pour la praticité) |
| **Variance** | Sens de compatibilité des sous-types (covariance/contravariance) |
| **Overload** | Plusieurs signatures pour une implémentation |
| **Barrel file** | `index.ts` qui ré-exporte des modules |
| **Triple-slash** | `/// <reference path="..." />` : directive d'inclusion (legacy, préférez les imports) |

---

## 77. Quiz : 10 questions + réponses

**Q1.** Que produit `tsc` à partir d'une `interface` dans le JavaScript généré ?
> **R :** Rien. Les interfaces (comme tous les types) sont effacées à la compilation — zéro trace au runtime.

**Q2.** Quelle est la différence entre `any` et `unknown` ?
> **R :** `any` désactive toute vérification (tout est permis, contamination). `unknown` impose un narrowing avant usage : c'est le « any sûr », à utiliser pour les données externes.

**Q3.** Pourquoi `fetch` peut-il vous piéger avec un HTTP 500 ?
> **R :** `fetch` ne rejette que sur erreur réseau. Un statut 500 résout normalement la promesse : il faut tester `res.ok` (ou `res.status`) explicitement.

**Q4.** Dans `hosts.forEach(async (h) => { await ping(h); })`, les pings sont-ils attendus avant la suite du programme ?
> **R :** Non. `forEach` ignore les promesses retournées. Utilisez `for...of` + `await` (séquentiel) ou `Promise.all(hosts.map(...))` (parallèle).

**Q5.** À quoi sert le contrôle `const x: never = e` dans le `default` d'un `switch` sur une union discriminée ?
> **R :** À garantir l'exhaustivité : si un nouveau variant est ajouté à l'union sans `case` correspondant, `e` n'est plus `never` et la compilation échoue.

**Q6.** Que change `"noUncheckedIndexedAccess": true` ?
> **R :** L'accès `tableau[i]` devient `T | undefined` au lieu de `T`, forçant à gérer l'index hors limites — source fréquente de crashs en prod.

**Q7.** Pourquoi faut-il écrire `from "./utils.js"` (et non `"./utils"`) avec `moduleResolution: NodeNext` ?
> **R :** Node ESM exige l'extension complète à l'exécution. TypeScript aligne la résolution sur ce comportement : l'import doit refléter le fichier JS réellement chargé.

**Q8.** Quelle est la différence entre `??` et `||` pour une valeur par défaut ?
> **R :** `??` ne s'applique que si `null`/`undefined` ; `||` s'applique aussi à `0`, `""`, `false`, `NaN`. Pour un timeout configuré à `0`, `||` écraserait à tort la valeur.

**Q9.** `private` en TypeScript protège-t-il vraiment un champ au runtime ?
> **R :** Non, c'est une vérification compile-time uniquement — le champ reste accessible en JS. Pour une vraie encapsulation runtime, utilisez `#champ`.

**Q10.** Quel est l'intérêt de Zod par rapport à un simple `as MonType` après `JSON.parse` ?
> **R :** `as` est un mensonge potentiel sans contrôle runtime. Zod valide réellement les données **et** infère le type statique (`z.infer`) : un schéma = validation + typage, source unique de vérité.

---

## 78. Pour aller plus loin

**Documentation officielle (en anglais, la référence) :**
- *TypeScript Handbook* : typescriptlang.org/docs/handbook — le lire en entier une fois vaut tous les tutoriels.
- *TSConfig Reference* : chaque option expliquée avec exemples.
- *TypeScript Playground* : typescriptlang.org/play — tester un type en 10 secondes, partager via URL.

**Outils à explorer ensuite :**
- **tRPC** : API typées de bout en bout (le type du serveur devient le type du client — fini les contrats dupliqués).
- **Prisma / Drizzle** : ORM avec types générés depuis le schéma SQL.
- **Hono / Fastify** : frameworks HTTP rapides et bien typés pour vos API internes.
- **Effect** : programmation fonctionnelle avancée (gestion d'erreurs, retry, ressources) — quand `Result` ne suffit plus.
- **tsx + chokidar / pm2** : superviser vos outils TS en production.

**Pratique délibérée (4 exercices) :**
1. Réécrire un script bash/python d'inventaire en CLI TypeScript (section 70) avec tests.
2. Typer un client d'API existant avec Zod (section 71) et mesurer les bugs attrapés par `tsc --noEmit`.
3. Migrer un vieux script JS avec la méthode de la section 74.
4. Implémenter un mini-dashboard React+TS (section 62) qui consomme l'agrégateur de la section 73.

**Communautés :** le Discord TypeScript, r/typescript, et le dépôt DefinitelyTyped pour contribuer des `@types`.

---

*Fin du guide — bon code, et que vos `tsc --noEmit` restent verts.* ✅
