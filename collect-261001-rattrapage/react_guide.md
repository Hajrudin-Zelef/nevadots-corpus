# Guide React — Le manuel complet

> **Public visé :** Zelef, chef de service systèmes & énergies — dashboards internes, outils web de supervision.
> **Ton :** direct, tutoriel + référence. **Version :** React 18+ (notes React 19 signalées quand pertinent).
> **Règle d'or de ce guide :** tout le code est moderne (composants fonctionnels, hooks). Les classes ne sont mentionnées que pour comprendre le code hérité.

---

## Sommaire

1. Pourquoi React pour un SI interne
2. Prérequis : Node.js, npm, éditeur
3. Créer un projet avec Vite
4. Anatomie d'un projet Vite + React
5. JSX — la syntaxe au cœur de React
6. JSX — expressions, attributs, enfants
7. JSX — les 3 règles de base
8. Composants fonctionnels
9. Props en détail
10. Composition vs héritage
11. L'état avec useState
12. useState — pièges et bonnes pratiques
13. Rendu conditionnel
14. Événements en React
15. Formulaires contrôlés
16. Listes et keys
17. useEffect — dépendances et cleanup
18. useEffect — les patterns courants
19. useRef — au-delà du DOM
20. useMemo — mémoriser un calcul
21. useCallback — mémoriser une fonction
22. React.memo — éviter les re-rendus inutiles
23. useContext et la Context API
24. useReducer — l'état complexe
25. Hooks personnalisés
26. Les règles des hooks (linter)
27. Data fetching — patterns loading / error
28. Data fetching — exemple complet
29. Error Boundaries — attraper les plantages
30. Modules — organiser son code
31. React Router — routes de base
32. React Router — params, navigation, layout
33. Vite — configurer et builder
34. Debug avec React DevTools
35. Debug — stratégies et console
36. Tests avec Vitest + Testing Library
37. Tests — exemples commentés
38. TypeScript + React
39. Sécurité — XSS et injections
40. Bonnes pratiques & style de code
41. Performance — comprendre le rendu
42. Performance — lazy, Suspense, code splitting
43. Performance — virtualisation des longues listes
44. Intro Next.js — SSR et SSG
45. Erreurs classiques des débutants (15)
46. Les erreurs React les plus fréquentes — corrections
47. Cas pratique 1 — dashboard de supervision
48. Cas pratique 2 — formulaire de ticket d'intervention
49. Cas pratique 3 — liste d'équipements filtrable
50. Checklist — mettre une app en production
51. Pense-bête de poche
52. Glossaire
53. Quiz — 10 questions + réponses
54. Pour aller plus loin

---

## 1. Pourquoi React pour un SI interne

React est une bibliothèque JavaScript (Meta) pour construire des interfaces utilisateur par **composants réutilisables**. Pour un service systèmes & énergies, les cas d'usage typiques sont :

- **Dashboards de supervision** : onduleurs, températures, états d'équipements, alertes temps réel.
- **Outils internes** : formulaires d'intervention, inventaires, suivi de tickets, GMAO simplifiée.
- **Portails** : pages d'accueil intranet, documentation, tableaux de bord KPI.

Pourquoi React plutôt qu'un site "classique" en HTML/JS vanilla ou PHP ?

| Critère | React | JS vanilla / jQuery |
|---|---|---|
| Mise à jour du DOM | Déclarative : on décrit l'état, React met à jour | Impérative : on manipule le DOM à la main |
| Réutilisabilité | Composants (bouton, carte, tableau) | Copier-coller de code |
| Écosystème | Énorme (charts, tables, formulaires) | Fragmenté |
| Tests | Vitest + Testing Library, standardisés | Artisanal |
| Courbe d'apprentissage | Moyenne (JSX, hooks) | Faible au début, chaotique ensuite |

**Le modèle mental à adopter :** en React, l'UI est une **fonction de l'état**. `UI = f(state)`. Quand l'état change (nouvelle mesure d'un onduleur), React recalcule et met à jour uniquement ce qui a changé. On ne touche (presque) jamais au DOM directement.

**Quand NE PAS utiliser React :** une page statique de documentation, un simple formulaire de contact → un site statique ou un template serveur suffit. React se justifie dès qu'il y a de l'interactivité et de l'état.

---

## 2. Prérequis : Node.js, npm, éditeur

React s'écrit en JavaScript moderne (ES2015+). Avant tout :

### 2.1 Installer Node.js

Node.js permet d'exécuter JavaScript hors navigateur et fournit **npm** (gestionnaire de paquets).

- Télécharger la version **LTS** sur https://nodejs.org (vérifiée : LTS conseillée pour la stabilité).
- Vérifier l'installation :

```bash
node -v   # v20.x.x ou v22.x.x attendu
npm -v    # 10.x.x attendu
```

> Sur un poste Windows d'entreprise, préférer l'installeur MSI officiel. Éviter les versions "Current" en production.

### 2.2 L'éditeur : VS Code

Extensions indispensables :

| Extension | Rôle |
|---|---|
| ES7+ React/Redux/React-Native snippets | Snippets (`rafce` → composant) |
| ESLint | Détecte les erreurs, **dont les règles des hooks** |
| Prettier | Formatage automatique |
| Error Lens | Affiche les erreurs inline |

### 2.3 JavaScript moderne requis

Si ces syntaxes sont floues, les revoir avant de continuer :

```js
// Déstructuration
const { nom, puissance } = onduleur;

// Spread
const copie = { ...onduleur, puissance: 120 };

// Arrow functions
const double = (x) => x * 2;

// Template literals
const msg = `UPS ${nom} : ${puissance} kVA`;

// Modules ES
import { useState } from 'react';
export function Carte() { /* ... */ }

// Optional chaining + nullish coalescing
const ville = site?.adresse?.ville ?? 'Inconnue';
```

**Checklist prérequis :**
- [ ] Node LTS installé, `node -v` OK
- [ ] VS Code + ESLint + Prettier
- [ ] À l'aise avec les arrow functions, la déstructuration, les modules ES

---

## 3. Créer un projet avec Vite

**Vite** est l'outil de build officiellement recommandé pour démarrer un projet React (Create React App est abandonné depuis 2023 — ne plus l'utiliser).

```bash
# Créer le projet (répondre aux questions : React, JavaScript ou TypeScript)
npm create vite@latest supervision-dashboard -- --template react

cd supervision-dashboard
npm install     # installe les dépendances
npm run dev     # lance le serveur de dev → http://localhost:5173
```

Commandes du quotidien :

| Commande | Effet |
|---|---|
| `npm run dev` | Serveur de développement (rechargement instantané) |
| `npm run build` | Build de production dans `dist/` |
| `npm run preview` | Prévisualise le build de production en local |
| `npm install <paquet>` | Ajoute une dépendance |

> **Pourquoi Vite et pas autre chose ?** Démarrage en < 1 s, rechargement à chaud (HMR) quasi instantané même sur gros projet, config simple. Pour du SSR (rendu serveur), voir la section 44 (Next.js).

**Variante TypeScript** (recommandée pour un projet d'équipe) :

```bash
npm create vite@latest supervision-dashboard -- --template react-ts
```

---

## 4. Anatomie d'un projet Vite + React

```
supervision-dashboard/
├── index.html              # point d'entrée HTML (Vite l'utilise directement)
├── package.json            # dépendances + scripts
├── vite.config.js          # config Vite
├── src/
│   ├── main.jsx            # point d'entrée JS : monte <App/> dans le DOM
│   ├── App.jsx             # composant racine
│   ├── App.css
│   ├── index.css           # styles globaux
│   └── assets/             # images, etc.
```

**`src/main.jsx`** — le bootstrap, on n'y touche presque jamais :

```jsx
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import App from './App.jsx';
import './index.css';

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

**`src/App.jsx`** — le composant racine, c'est ici que tout commence :

```jsx
export default function App() {
  return <h1>Supervision UPS</h1>;
}
```

**Points clés :**
- `StrictMode` : en dev, il double l'invocation de certains rendus pour révéler les effets de bord. **Ne pas le retirer** : un bug visible uniquement sans StrictMode est un bug réel.
- `createRoot` : l'API React 18+ (l'ancien `ReactDOM.render` est déprécié).
- Le fichier `index.html` contient `<div id="root"></div>` et le script `/src/main.jsx`.

**Convention de nommage :** fichiers de composants en `PascalCase` (`CarteOnduleur.jsx`), un composant par fichier, extension `.jsx` (ou `.tsx`).

---

## 5. JSX — la syntaxe au cœur de React

JSX est une extension de syntaxe qui permet d'écrire du HTML dans du JavaScript :

```jsx
const titre = <h1>État des onduleurs</h1>;
```

Ce n'est ni une string ni du HTML : c'est du **sucre syntaxique** pour `React.createElement('h1', null, 'État des onduleurs')`. Le navigateur ne comprend pas JSX — Vite le transpile (via esbuild) en JavaScript pur au build.

```jsx
// Ce que vous écrivez
const carte = <div className="carte">UPS-01</div>;

// Ce que Vite produit (simplifié)
const carte = React.createElement('div', { className: 'carte' }, 'UPS-01');
```

**Conséquences pratiques :**
1. On peut mélanger logique JS et markup dans le même fichier — c'est voulu, c'est la **colocalisation**.
2. `class` devient `className` (mot réservé JS), `for` devient `htmlFor`.
3. Tout attribut HTML standard existe en camelCase : `tabIndex`, `onClick`, `readOnly`, `maxLength`.

**Pense-bête JSX :**
- [ ] `class` → `className`
- [ ] Attributs en camelCase
- [ ] Les balises doivent être fermées : `<img />`, `<br />`, `<input />`
- [ ] Commentaires : `{/* commentaire */}`, pas `<!-- -->`

---

## 6. JSX — expressions, attributs, enfants

Les accolades `{}` permettent d'injecter **n'importe quelle expression JavaScript** dans le JSX :

```jsx
const ups = { nom: 'UPS-01', puissance: 40, enLigne: true };

function CarteUps() {
  return (
    <div className="carte">
      <h2>{ups.nom}</h2>
      <p>Puissance : {ups.puissance} kVA</p>
      <p>Double : {ups.puissance * 2} kVA</p>
      <p>Statut : {ups.enLigne ? 'En ligne' : 'Hors ligne'}</p>
      <p>Prochaine maintenance : {prochaineDate().toLocaleDateString('fr-FR')}</p>
    </div>
  );
}
```

**Ce qui est autorisé dans `{}` :** variables, appels de fonctions, ternaires, calculs, accès à des tableaux/objets.

**Ce qui est INTERDIT :** les instructions (`if`, `for`, `while`, `return` au milieu). Pour de la logique complexe, la calculer **avant** le `return` :

```jsx
function Badge({ niveau }) {
  // Logique AVANT le return
  const couleur = niveau > 80 ? 'rouge' : niveau > 50 ? 'orange' : 'vert';
  const libelle = `Charge ${niveau} %`;

  return <span style={{ color: couleur }}>{libelle}</span>;
}
```

**Styles inline :** un objet JS, clés en camelCase :

```jsx
<div style={{ backgroundColor: '#111', padding: '16px', fontSize: 14 }}>
```

> Note : `fontSize: 14` → React ajoute automatiquement `px` pour les propriétés numériques qui l'acceptent.

---

## 7. JSX — les 3 règles de base

### Règle 1 : un seul élément parent

Un composant doit retourner **un seul** élément racine. Sinon, erreur de syntaxe.

```jsx
// ❌ ERREUR : deux racines
return (
  <h1>Titre</h1>
  <p>Texte</p>
);

// ✅ OK : envelopper dans une div…
return (
  <div>
    <h1>Titre</h1>
    <p>Texte</p>
  </div>
);

// ✅ MIEUX : Fragment (n'ajoute pas de div inutile dans le DOM)
return (
  <>
    <h1>Titre</h1>
    <p>Texte</p>
  </>
);
```

`<>...</>` est le raccourci de `<React.Fragment>...</React.Fragment>`.

### Règle 2 : fermer toutes les balises

```jsx
<img src="ups.png" alt="Onduleur" />   // ✅
<br />                                 // ✅
<input type="text" />                  // ✅
```

### Règle 3 : camelCase pour les attributs

`class` → `className`, `onclick` → `onClick`, `tabindex` → `tabIndex`.

---

## 8. Composants fonctionnels

Un composant est une **fonction JavaScript qui retourne du JSX**. Son nom commence par une majuscule (sinon React le traite comme une balise HTML).

```jsx
// Composant simple
function Titre() {
  return <h1>Supervision énergétique</h1>;
}

// Utilisation (comme une balise HTML personnalisée)
function App() {
  return (
    <div>
      <Titre />
      <Titre />
    </div>
  );
}
```

**Anatomie complète d'un composant :**

```jsx
import { useState } from 'react';

export default function CarteOnduleur({ nom, puissance }) {
  const [allume, setAllume] = useState(true);

  return (
    <article className="carte">
      <h2>{nom}</h2>
      <p>{puissance} kVA — {allume ? 'ON' : 'OFF'}</p>
      <button onClick={() => setAllume(!allume)}>Basculer</button>
    </article>
  );
}
```

**Règles d'un composant :**
- Nom en `PascalCase`.
- Retourne du JSX (ou `null` pour ne rien afficher).
- Ne modifie jamais ses props (elles sont en lecture seule).
- Peut contenir de l'état (hooks), des effets, des fonctions internes.

**Composition :** les composants s'imbriquent comme des briques :

```jsx
function Dashboard() {
  return (
    <main>
      <Entete titre="Supervision" />
      <Grille>
        <CarteOnduleur nom="UPS-01" puissance={40} />
        <CarteOnduleur nom="UPS-02" puissance={60} />
      </Grille>
      <PiedDePage />
    </main>
  );
}
```

> **Héritage (classes) :** l'ancien modèle à base de `class MonComposant extends React.Component` existe encore dans du code ancien. En React 18+, on écrit **100 % fonctionnel**. Les classes ne sont détaillées ici que pour lire du code legacy.

---

## 9. Props en détail

Les **props** (propriétés) sont les arguments d'un composant : le mécanisme pour faire passer des données du parent vers l'enfant. Elles sont **en lecture seule** dans l'enfant.

```jsx
// Parent : passe les props comme des attributs HTML
<CarteOnduleur nom="UPS-01" puissance={40} critique={true} />

// Enfant : les reçoit dans un objet en premier paramètre
function CarteOnduleur(props) {
  return <h2>{props.nom} — {props.puissance} kVA</h2>;
}

// Version idiomatique : déstructuration directe
function CarteOnduleur({ nom, puissance, critique = false }) {
  return (
    <h2 className={critique ? 'critique' : ''}>
      {nom} — {puissance} kVA
    </h2>
  );
}
```

**Types de props courants :**

| Type | Exemple | Usage |
|---|---|---|
| String | `nom="UPS-01"` | Texte simple |
| Nombre | `puissance={40}` | Accolades obligatoires |
| Booléen | `critique` (= `critique={true}`) | Flag |
| Objet / tableau | `mesures={[1, 2, 3]}` | Données |
| Fonction | `onAlerte={gererAlerte}` | Callback enfant → parent |
| Élément | `icone={<IconeUps />}` | Composition |

**Props spéciales :**
- `children` : le contenu entre les balises ouvrante/fermante.
- `key` : réservée à React pour les listes (section 16), **non accessible** dans le composant.

```jsx
function Cadre({ titre, children }) {
  return (
    <section className="cadre">
      <h2>{titre}</h2>
      <div className="contenu">{children}</div>
    </section>
  );
}

// Utilisation
<Cadre titre="Batteries">
  <p>Tension : 432 V</p>
  <p>Autonomie : 12 min</p>
</Cadre>;
```

**Valeurs par défaut** (déstructuration, syntaxe moderne) :

```jsx
function Badge({ texte, couleur = 'gris', taille = 'm' }) { /* ... */ }
```

> L'ancienne syntaxe `Composant.defaultProps = {...}` est dépréciée — utiliser les valeurs par défaut de la déstructuration.

---

## 10. Composition vs héritage

React **n'utilise pas l'héritage** entre composants : on compose. C'est un choix de design assumé.

### Containment (conteneur générique)

```jsx
function Panneau({ titre, children }) {
  return (
    <div className="panneau">
      <header>{titre}</header>
      {children}
    </div>
  );
}

<Panneau titre="Alertes">
  <ListeAlertes />
</Panneau>;
```

### Spécialisation (variante d'un composant générique)

```jsx
function Bouton({ variante, children, ...reste }) {
  return <button className={`btn btn-${variante}`} {...reste}>{children}</button>;
}

// Spécialisations par composition, pas par héritage
function BoutonDanger(props) {
  return <Bouton variante="danger" {...props} />;
}
function BoutonPrimaire(props) {
  return <Bouton variante="primaire" {...props} />;
}
```

### Slots multiples (plusieurs `children` nommés)

```jsx
function MiseEnPage({ entete, lateral, children }) {
  return (
    <div className="layout">
      <header>{entete}</header>
      <aside>{lateral}</aside>
      <main>{children}</main>
    </div>
  );
}

<MiseEnPage
  entete={<BarreNavigation />}
  lateral={<MenuSites />}
/>;
```

**Règle :** si vous êtes tenté d'écrire `class X extends Y` pour des composants → utilisez la composition. L'héritage de composants n'existe quasiment jamais en React moderne.

---

## 11. L'état avec useState

Le **state** (état) est la mémoire d'un composant : des données qui, quand elles changent, provoquent un nouveau rendu.

```jsx
import { useState } from 'react';

function Compteur() {
  // [valeur actuelle, fonction pour la modifier] = useState(valeur initiale)
  const [compteur, setCompteur] = useState(0);

  return (
    <div>
      <p>Clics : {compteur}</p>
      <button onClick={() => setCompteur(compteur + 1)}>+1</button>
    </div>
  );
}
```

**Exemple métier — bascule d'un équipement :**

```jsx
function InterrupteurEquipement({ nom }) {
  const [actif, setActif] = useState(true);

  return (
    <div>
      <span>{nom} : {actif ? '🟢 En service' : '🔴 Hors service'}</span>
      <button onClick={() => setActif(!actif)}>
        {actif ? 'Mettre hors service' : 'Remettre en service'}
      </button>
    </div>
  );
}
```

**Points fondamentaux :**
1. `useState` retourne **toujours un tableau de 2 éléments** : `[état, setÉtat]`.
2. Appeler `setÉtat` **ne modifie pas la variable immédiatement** : il planifie un nouveau rendu.
3. Chaque instance de composant a son **propre état isolé** (deux `<Compteur />` ne partagent rien).
4. L'état initial n'est évalué qu'au **premier rendu** (pour un calcul coûteux : `useState(() => calculCouteux())` — initialisation paresseuse).

**Types d'état courants :**

```jsx
const [nom, setNom] = useState('');           // string
const [charge, setCharge] = useState(0);      // number
const [enLigne, setEnLigne] = useState(true); // boolean
const [ups, setUps] = useState(null);         // null en attendant les données
const [mesures, setMesures] = useState([]);   // tableau
const [config, setConfig] = useState({ seuil: 80 }); // objet
```

---

## 12. useState — pièges et bonnes pratiques

### Piège 1 : l'état est immuable — ne jamais le muter

```jsx
const [mesures, setMesures] = useState([72, 75, 71]);

// ❌ INTERDIT : mutation directe → React ne détecte pas le changement
mesures.push(78);
setMesures(mesures);

// ✅ Créer un NOUVEAU tableau
setMesures([...mesures, 78]);

// ❌ INTERDIT : mutation d'objet
config.seuil = 90;
setConfig(config);

// ✅ Nouvel objet
setConfig({ ...config, seuil: 90 });
```

**Tableau récapitulatif des mises à jour immuables :**

| Opération | ❌ Mutant | ✅ Immuable |
|---|---|---|
| Ajouter | `push()` | `[...t, x]` |
| Retirer | `splice()` | `t.filter(...)` |
| Modifier un élément | `t[i] = x` | `t.map((e, i) => ...)` |
| Trier | `sort()` | `[...t].sort()` |
| Changer une clé | `o.k = v` | `{...o, k: v}` |
| Objet imbriqué | `o.a.b = v` | `{...o, a: {...o.a, b: v}}` |

### Piège 2 : le batching — l'état n'est pas mis à jour immédiatement

```jsx
function doubleIncrement() {
  setCompteur(compteur + 1);
  setCompteur(compteur + 1);
  // ❌ compteur n'augmente que de 1 : les deux appels voient l'ANCIENNE valeur
}

// ✅ Forme fonctionnelle : reçoit la valeur à jour
function doubleIncrement() {
  setCompteur((c) => c + 1);
  setCompteur((c) => c + 1); // +2 garanti
}
```

**Règle :** dès que la nouvelle valeur **dépend de l'ancienne**, utiliser la forme `setÉtat((prev) => ...)`.

### Piège 3 : état redondant (dérivable)

```jsx
// ❌ Mauvais : nomComplet est dérivable de prenom + nom
const [prenom, setPrenom] = useState('Jean');
const [nom, setNom] = useState('Dupont');
const [nomComplet, setNomComplet] = useState(''); // risque de désynchro !

// ✅ Calculer pendant le rendu
const nomComplet = `${prenom} ${nom}`;
```

**Ne mettre dans l'état que ce qui ne peut pas être calculé** à partir des props ou d'un autre état.

---

## 13. Rendu conditionnel

Plusieurs techniques pour afficher ou non du contenu :

```jsx
function StatutEquipement({ enLigne, enAlarme }) {
  // 1. Ternaire (if/else)
  const badge = enLigne
    ? <span className="ok">En ligne</span>
    : <span className="ko">Hors ligne</span>;

  return (
    <div>
      {badge}

      {/* 2. && pour "si vrai, afficher" */}
      {enAlarme && <AlerteCritique />}

      {/* 3. Ternaire inline */}
      {enLigne ? <DetailsTechniques /> : <p>Équipement injoignable</p>}
    </div>
  );
}
```

**Attention au piège du `&&` avec `0` :**

```jsx
// ❌ Si nbAlertes vaut 0, React affiche "0" !
{nbAlertes && <p>{nbAlertes} alertes</p>}

// ✅ Comparaison explicite
{nbAlertes > 0 && <p>{nbAlertes} alertes</p>}
```

**Retourner `null` pour ne rien afficher :**

```jsx
function BanniereMaintenance({ maintenanceEnCours }) {
  if (!maintenanceEnCours) return null;
  return <div className="banniere">⚠️ Maintenance planifiée ce soir 22h</div>;
}
```

**Tableau de choix :**

| Besoin | Technique |
|---|---|
| if / else simple | Ternaire `? :` |
| Afficher si vrai | `&&` (avec garde anti-`0`) |
| Logique complexe | `if` avant le `return`, variable intermédiaire |
| Ne rien afficher | `return null` |

---

## 14. Événements en React

Les événements se déclarent en camelCase (`onClick`, `onChange`, `onSubmit`) et reçoivent une **fonction**, pas une string.

```jsx
function PanneauControle() {
  const demarrer = () => console.log('Démarrage…');
  const arreter = (e) => {
    e.preventDefault(); // comme en JS classique
    console.log('Arrêt…');
  };

  return (
    <div>
      {/* Référence de fonction : sans parenthèses */}
      <button onClick={demarrer}>Démarrer</button>

      {/* Arrow inline pour passer un argument */}
      <button onClick={() => arreter('manuel')}>Arrêt manuel</button>

      {/* ❌ NE PAS faire : appelle la fonction pendant le rendu ! */}
      {/* <button onClick={demarrer()}>Démarrer</button> */}
    </div>
  );
}
```

**L'objet événement :** React utilise des événements synthétiques (wrapper cross-browser). Les propriétés utiles : `e.target.value` (inputs), `e.preventDefault()`, `e.stopPropagation()`.

```jsx
function Formulaire() {
  const envoyer = (e) => {
    e.preventDefault(); // empêche le rechargement de la page
    // … logique d'envoi
  };
  return <form onSubmit={envoyer}>…</form>;
}
```

**Événements courants :**

| Événement | Déclencheur |
|---|---|
| `onClick` | Clic |
| `onChange` | Champ modifié (texte, select, checkbox) |
| `onSubmit` | Formulaire soumis |
| `onKeyDown` / `onKeyUp` | Touche clavier |
| `onFocus` / `onBlur` | Focus gagné/perdu |

---

## 15. Formulaires contrôlés

En React, un **formulaire contrôlé** stocke la valeur de chaque champ dans l'état. C'est le pattern standard (vs "non contrôlé" avec `useRef`, réservé à des cas simples).

```jsx
import { useState } from 'react';

function FormulaireTicket() {
  const [titre, setTitre] = useState('');
  const [priorite, setPriorite] = useState('normale');
  const [urgent, setUrgent] = useState(false);

  const envoyer = (e) => {
    e.preventDefault();
    console.log({ titre, priorite, urgent });
    // → POST vers l'API (section 28)
  };

  return (
    <form onSubmit={envoyer}>
      <label>
        Titre :
        <input
          type="text"
          value={titre}
          onChange={(e) => setTitre(e.target.value)}
        />
      </label>

      <label>
        Priorité :
        <select value={priorite} onChange={(e) => setPriorite(e.target.value)}>
          <option value="basse">Basse</option>
          <option value="normale">Normale</option>
          <option value="haute">Haute</option>
        </select>
      </label>

      <label>
        <input
          type="checkbox"
          checked={urgent}
          onChange={(e) => setUrgent(e.target.checked)}
        />
        Urgent
      </label>

      <button type="submit">Créer le ticket</button>
    </form>
  );
}
```

**Points clés :**
- `value` + `onChange` pour texte/select ; `checked` + `onChange` pour checkbox (`e.target.checked`).
- Sans `onChange`, un champ avec `value` est **en lecture seule** (warning React).
- Un seul objet d'état pour un gros formulaire (voir cas pratique 2, section 48).

**Validation simple :**

```jsx
const [erreurs, setErreurs] = useState({});

const valider = () => {
  const e = {};
  if (titre.trim().length < 5) e.titre = 'Titre trop court (min 5 caractères)';
  setErreurs(e);
  return Object.keys(e).length === 0;
};
```

---

## 16. Listes et keys

Pour afficher un tableau, on le transforme avec `.map()` en éléments JSX. Chaque élément **doit** avoir une prop `key` unique et stable.

```jsx
const onduleurs = [
  { id: 'ups-01', nom: 'UPS-01', puissance: 40 },
  { id: 'ups-02', nom: 'UPS-02', puissance: 60 },
];

function ListeOnduleurs() {
  return (
    <ul>
      {onduleurs.map((ups) => (
        <li key={ups.id}>
          {ups.nom} — {ups.puissance} kVA
        </li>
      ))}
    </ul>
  );
}
```

### Pourquoi `key` est critique

React utilise `key` pour **identifier chaque élément entre deux rendus** : réordonner, ajouter, supprimer sans tout re-créer (et sans perdre l'état local des lignes, ex. un champ en cours de saisie).

```jsx
// ❌ JAMAIS l'index comme key si la liste peut changer d'ordre / filtrée
{items.map((item, i) => <Ligne key={i} {...item} />)}

// ✅ Un identifiant stable issu des données
{items.map((item) => <Ligne key={item.id} {...item} />)}
```

**Conséquence d'une mauvaise key :** avec l'index, si on supprime la première ligne, React croit que c'est la *dernière* qui a disparu → états des champs mélangés, bugs "fantômes".

### Filtrage + tri avant le rendu

```jsx
function ListeFiltree({ equipements, recherche }) {
  const visibles = equipements
    .filter((e) => e.nom.toLowerCase().includes(recherche.toLowerCase()))
    .sort((a, b) => a.nom.localeCompare(b.nom));

  if (visibles.length === 0) return <p>Aucun équipement trouvé.</p>;

  return (
    <ul>
      {visibles.map((e) => (
        <li key={e.id}>{e.nom}</li>
      ))}
    </ul>
  );
}
```

> `key` n'est **pas** transmise au composant comme prop. Besoin de l'id dans l'enfant ? Le passer explicitement : `<Ligne key={e.id} id={e.id} />`.

---

## 17. useEffect — dépendances et cleanup

`useEffect` exécute du code **après** le rendu, pour tout ce qui est un **effet de bord** : appels API, timers, abonnements (WebSocket), manipulation du DOM, `document.title`.

```jsx
import { useState, useEffect } from 'react';

function Horloge() {
  const [heure, setHeure] = useState(new Date());

  useEffect(() => {
    // 1. L'EFFET : s'exécute après chaque rendu (selon dépendances)
    const timer = setInterval(() => setHeure(new Date()), 1000);

    // 2. Le CLEANUP : exécuté avant le prochain effet + au démontage
    return () => clearInterval(timer);
  }, []); // 3. Le TABLEAU DE DÉPENDANCES

  return <p>Il est {heure.toLocaleTimeString('fr-FR')}</p>;
}
```

### Le tableau de dépendances — les 3 cas

| Dépendances | Comportement |
|---|---|
| `[]` (vide) | L'effet tourne **une seule fois** (montage) |
| `[a, b]` | L'effet tourne au montage + **quand `a` ou `b` change** |
| *(absent)* | L'effet tourne **après chaque rendu** (rarement voulu) |

```jsx
// ❌ Sans tableau : boucle infinie potentielle (setState → rendu → effet → setState…)
useEffect(() => { setCompteur(compteur + 1); });

// ✅ Avec dépendance : se relance quand l'id change (ex. page détail d'un équipement)
useEffect(() => {
  fetch(`/api/ups/${id}`).then(/* ... */);
}, [id]);
```

### Le cleanup — quand et pourquoi

Le cleanup s'exécute : **avant** chaque ré-exécution de l'effet, et au **démontage** du composant.

```jsx
useEffect(() => {
  const ws = new WebSocket('wss://supervision.local/mesures');
  ws.onmessage = (e) => setMesures(JSON.parse(e.data));
  return () => ws.close(); // ferme l'ancienne connexion avant d'en ouvrir une nouvelle
}, [siteId]);
```

**Sans cleanup :** timers qui continuent après navigation, WebSockets orphelines, `setState` sur composant démonté (warning + fuite mémoire).

> **React 19 :** `useEffect` existe toujours. La nouveauté est le hook `use()` pour consommer des promesses pendant le rendu (Suspense), mais `useEffect` reste le standard pour les effets impératifs.

---

## 18. useEffect — les patterns courants

### Pattern 1 : chargement de données au montage

```jsx
useEffect(() => {
  let ignore = false; // garde anti "setState après démontage"

  async function charger() {
    const res = await fetch('/api/onduleurs');
    const data = await res.json();
    if (!ignore) setOnduleurs(data);
  }
  charger();

  return () => { ignore = true; };
}, []);
```

> Note : on ne peut pas rendre la fonction d'effet `async` directement (elle doit retourner soit rien, soit une fonction de cleanup). On définit une fonction async **à l'intérieur**.

### Pattern 2 : synchroniser le titre du document

```jsx
useEffect(() => {
  document.title = `(${nbAlertes}) Supervision`;
}, [nbAlertes]);
```

### Pattern 3 : s'abonner / se désabonner

```jsx
useEffect(() => {
  const onResize = () => setLargeur(window.innerWidth);
  window.addEventListener('resize', onResize);
  return () => window.removeEventListener('resize', onResize);
}, []);
```

### Pattern 4 : effet conditionné par une valeur

```jsx
// Recharge les mesures quand l'équipement sélectionné change
useEffect(() => {
  if (!equipementId) return;
  chargerMesures(equipementId);
}, [equipementId]);
```

### Anti-patterns useEffect

| ❌ À éviter | ✅ À la place |
|---|---|
| `useEffect` pour calculer une valeur dérivée | Calcul direct pendant le rendu (section 12) |
| Chaîne d'effets qui se déclenchent entre eux | Un seul effet, ou un gestionnaire d'événement |
| `fetch` sans garde `ignore` / AbortController | Annuler proprement (voir section 28) |
| Dépendances manquantes (warning ESLint) | Ajouter toutes les valeurs utilisées, ou `useCallback` |

**Checklist useEffect :**
- [ ] Tableau de dépendances toujours renseigné (même `[]`)
- [ ] Cleanup pour tout timer / abonnement / connexion
- [ ] Pas de fonction `async` directement en callback d'effet
- [ ] Warning `react-hooks/exhaustive-deps` d'ESLint = à corriger, pas à ignorer

---

## 19. useRef — au-delà du DOM

`useRef` retourne un objet `{ current: ... }` **persistant entre les rendus**, dont la modification **ne provoque pas** de nouveau rendu. Deux usages :

### Usage 1 : accéder à un élément du DOM

```jsx
import { useRef } from 'react';

function ChampRecherche() {
  const inputRef = useRef(null);

  const focus = () => inputRef.current?.focus();

  return (
    <div>
      <input ref={inputRef} type="search" placeholder="Rechercher…" />
      <button onClick={focus}>Focus</button>
    </div>
  );
}
```

### Usage 2 : une "boîte" mutable qui survit aux rendus

```jsx
function CompteurRendus() {
  const nbRendus = useRef(0);
  const timerRef = useRef(null);

  useEffect(() => {
    nbRendus.current += 1; // pas de re-rendu → pas de boucle infinie
    console.log('Rendus :', nbRendus.current);
  });

  const demarrer = () => {
    timerRef.current = setInterval(() => console.log('tick'), 1000);
  };
  const arreter = () => clearInterval(timerRef.current);
  // …
}
```

**useRef vs useState :**

|  | `useState` | `useRef` |
|---|---|---|
| Modif → re-rendu | ✅ Oui | ❌ Non |
| Lecture pendant le rendu | ✅ | ⚠️ Éviter (valeur "en retard" possible) |
| Cas typique | Données affichées | Timers, éléments DOM, valeur précédente |

**Piège :** ne pas lire/écrire `ref.current` **pendant** le rendu (sauf initialisation) — le faire dans des effets ou des gestionnaires d'événements.

---

## 20. useMemo — mémoriser un calcul

`useMemo` mémorise le **résultat** d'un calcul coûteux et ne le recalcule que si ses dépendances changent.

```jsx
import { useMemo } from 'react';

function Statistiques({ mesures }) {
  // Sans useMemo : ce tri+calcul tourne à CHAQUE rendu (même si mesures inchangé)
  const stats = useMemo(() => {
    console.log('Calcul des stats…');
    const triees = [...mesures].sort((a, b) => a - b);
    return {
      min: triees[0],
      max: triees[triees.length - 1],
      moyenne: triees.reduce((s, v) => s + v, 0) / triees.length,
    };
  }, [mesures]);

  return <p>Min {stats.min} — Max {stats.max} — Moy {stats.moyenne.toFixed(1)}</p>;
}
```

**Quand l'utiliser :**
- Calcul réellement coûteux (tri/filtrage de milliers de lignes, agrégations).
- Valeur passée à un composant mémorisé (`React.memo`) ou à un `useEffect` en dépendance (référence stable).

**Quand NE PAS l'utiliser :** pour un calcul trivial — `useMemo` a lui-même un coût. **Ne pas optimiser prématurément** : d'abord mesurer (React DevTools Profiler, section 34).

---

## 21. useCallback — mémoriser une fonction

`useCallback` mémorise une **fonction** : même référence entre les rendus tant que les dépendances ne changent pas.

```jsx
import { useState, useCallback } from 'react';

function Liste({ items }) {
  const [selection, setSelection] = useState(new Set());

  // Sans useCallback : nouvelle fonction à chaque rendu → LigneItem (memo) re-rend inutilement
  const basculer = useCallback((id) => {
    setSelection((prev) => {
      const next = new Set(prev);
      next.has(id) ? next.delete(id) : next.add(id);
      return next;
    });
  }, []); // setSelection est stable → pas de dépendance

  return items.map((item) => (
    <LigneItem key={item.id} item={item} onBasculer={basculer} />
  ));
}

const LigneItem = React.memo(function LigneItem({ item, onBasculer }) {
  return <li onClick={() => onBasculer(item.id)}>{item.nom}</li>;
});
```

**`useCallback(fn, deps)` ≈ `useMemo(() => fn, deps)`** — c'est du sucre syntaxique.

**Règle pratique :** `useCallback` n'est utile que si la fonction est passée à un composant **mémorisé** ou utilisée comme **dépendance d'un effet**. Sinon, une fonction inline suffit.

---

## 22. React.memo — éviter les re-rendus inutiles

Par défaut, quand un parent se re-rend, **tous ses enfants se re-rendent aussi** (même si leurs props n'ont pas changé). `React.memo` mémorise le rendu d'un composant : il ne se re-rend que si ses props changent (comparaison superficielle).

```jsx
import { memo } from 'react';

// Ne se re-rend que si `ups` (référence) change
const CarteOnduleur = memo(function CarteOnduleur({ ups }) {
  console.log('Rendu', ups.nom);
  return <div>{ups.nom} — {ups.charge} %</div>;
});
```

**Comparaison personnalisée** (rarement nécessaire) :

```jsx
const Ligne = memo(function Ligne({ data }) { /* ... */ },
  (prev, next) => prev.data.id === next.data.id // true = skip le re-rendu
);
```

**Limites importantes :**
- Comparaison **superficielle** : si le parent recrée un objet à chaque rendu (`data={{...}}`), `memo` ne sert à rien → combiner avec `useMemo`/`useCallback`.
- Ne pas enrober **tous** les composants : la comparaison a un coût. Cibler les composants coûteux ou très souvent re-rendus (lignes de tableau, listes longues).
- `memo` ne protège pas contre un changement de `children` recréé à chaque rendu.

**Ordre d'optimisation :** 1) structure correcte (état au bon niveau), 2) `memo` ciblé, 3) `useMemo`/`useCallback` en support.

---

## 23. useContext et la Context API

Le **contexte** permet de partager des données à toute une sous-arborescence **sans prop drilling** (passer des props à travers 5 niveaux de composants qui n'en ont pas besoin).

Cas typiques : thème, utilisateur connecté, langue, configuration.

```jsx
import { createContext, useContext, useState } from 'react';

// 1. Créer le contexte (avec une valeur par défaut)
const ThemeContext = createContext('clair');

// 2. Fournir la valeur en haut de l'arbre
function App() {
  const [theme, setTheme] = useState('clair');
  return (
    <ThemeContext.Provider value={theme}>
      <Entete />
      <button onClick={() => setTheme(theme === 'clair' ? 'sombre' : 'clair')}>
        Basculer le thème
      </button>
    </ThemeContext.Provider>
  );
}

// 3. Consommer où on veut, sans props intermédiaires
function Entete() {
  const theme = useContext(ThemeContext);
  return <header className={`theme-${theme}`}>Supervision</header>;
}
```

### Pattern "contexte + hook personnalisé" (recommandé)

```jsx
// contexts/AuthContext.jsx
import { createContext, useContext, useState } from 'react';

const AuthContext = createContext(null);

export function AuthProvider({ children }) {
  const [utilisateur, setUtilisateur] = useState(null);
  const connecter = (u) => setUtilisateur(u);
  const deconnecter = () => setUtilisateur(null);

  return (
    <AuthContext.Provider value={{ utilisateur, connecter, deconnecter }}>
      {children}
    </AuthContext.Provider>
  );
}

// Hook qui masque useContext + vérifie le provider
export function useAuth() {
  const ctx = useContext(AuthContext);
  if (!ctx) throw new Error('useAuth doit être utilisé dans <AuthProvider>');
  return ctx;
}

// Utilisation
function Profil() {
  const { utilisateur, deconnecter } = useAuth();
  // …
}
```

**Avertissements :**
- Tout changement de `value` re-rend **tous** les consommateurs → mémoriser la valeur (`useMemo`) si elle contient des objets recréés à chaque rendu.
- Ne pas mettre dans un contexte des données qui changent très souvent (ex. position souris) — préférer un état local ou une librairie dédiée.
- Le contexte ne remplace pas un vrai store (Zustand, Redux) pour un état global complexe.

---

## 24. useReducer — l'état complexe

`useReducer` est une alternative à `useState` quand l'état a **plusieurs sous-valeurs liées** ou que les transitions sont complexes. Inspiré de Redux : un **reducer** pur `(état, action) => nouvelÉtat`.

```jsx
import { useReducer } from 'react';

const initial = { charge: 72, enLigne: true, alertes: [] };

function reducer(etat, action) {
  switch (action.type) {
    case 'MESURE':
      return { ...etat, charge: action.valeur };
    case 'PANNE':
      return { ...etat, enLigne: false, alertes: [...etat.alertes, action.alerte] };
    case 'REPARER':
      return { ...etat, enLigne: true };
    case 'RESET':
      return initial;
    default:
      throw new Error(`Action inconnue : ${action.type}`);
  }
}

function SupervisionUps() {
  const [etat, dispatch] = useReducer(reducer, initial);

  return (
    <div>
      <p>Charge : {etat.charge} % — {etat.enLigne ? 'En ligne' : 'En panne'}</p>
      <button onClick={() => dispatch({ type: 'MESURE', valeur: 75 })}>
        Simuler mesure
      </button>
      <button onClick={() => dispatch({ type: 'PANNE', alerte: 'Surchauffe' })}>
        Simuler panne
      </button>
    </div>
  );
}
```

**useState vs useReducer :**

|  | `useState` | `useReducer` |
|---|---|---|
| État simple (1-2 valeurs) | ✅ Idéal | Overkill |
| Plusieurs valeurs liées | Devient verbeux | ✅ Idéal |
| Transitions nommées/testables | ❌ | ✅ Le reducer est une fonction pure |
| Logique réutilisable | ❌ | ✅ Reducer exportable |

**Bonnes pratiques :** actions avec `type` en SNAKE_CASE, payloads explicites, reducer **pur** (pas de mutation, pas d'appels API dedans).

---

## 25. Hooks personnalisés

Un **hook personnalisé** est une fonction qui encapsule de la logique à base de hooks pour la **réutiliser** entre composants. Convention : nom préfixé par `use`.

```jsx
// hooks/useCompteur.js
import { useState, useCallback } from 'react';

export function useCompteur(initial = 0) {
  const [valeur, setValeur] = useState(initial);
  const incrementer = useCallback(() => setValeur((v) => v + 1), []);
  const decrementer = useCallback(() => setValeur((v) => v - 1), []);
  const reset = useCallback(() => setValeur(initial), [initial]);
  return { valeur, incrementer, decrementer, reset };
}

// Utilisation : chaque composant a son état indépendant
function PanneauA() {
  const { valeur, incrementer } = useCompteur(0);
  return <button onClick={incrementer}>A : {valeur}</button>;
}
```

**Exemple métier — polling d'une API :**

```jsx
// hooks/usePolling.js
import { useState, useEffect, useRef } from 'react';

export function usePolling(url, intervalleMs = 5000) {
  const [data, setData] = useState(null);
  const [erreur, setErreur] = useState(null);
  const [chargement, setChargement] = useState(true);

  useEffect(() => {
    let ignore = false;
    let timer;

    async function tick() {
      try {
        const res = await fetch(url);
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        const json = await res.json();
        if (!ignore) { setData(json); setErreur(null); }
      } catch (e) {
        if (!ignore) setErreur(e.message);
      } finally {
        if (!ignore) setChargement(false);
      }
    }

    tick();
    timer = setInterval(tick, intervalleMs);
    return () => { ignore = true; clearInterval(timer); };
  }, [url, intervalleMs]);

  return { data, erreur, chargement };
}

// Utilisation : 3 lignes pour un polling complet
function WidgetUps() {
  const { data, erreur, chargement } = usePolling('/api/ups/ups-01', 5000);
  if (chargement) return <p>Chargement…</p>;
  if (erreur) return <p>Erreur : {erreur}</p>;
  return <p>Charge : {data.charge} %</p>;
}
```

**Checklist hook personnalisé :**
- [ ] Nom en `useQuelqueChose`
- [ ] Retourne ce que le composant consomme (objet ou tableau)
- [ ] Chaque appel = état indépendant (pas de partage magique)
- [ ] Testable : logique pure extraite si possible

---

## 26. Les règles des hooks (linter)

Trois règles **non négociables**, vérifiées par le plugin ESLint `react-hooks` (inclus dans les templates Vite) :

### Règle 1 : appeler les hooks uniquement au niveau racine

```jsx
// ❌ INTERDIT : hook dans une condition / boucle / fonction imbriquée
function Mauvais({ actif }) {
  if (actif) {
    const [x, setX] = useState(0); // ordre des hooks instable !
  }
}

// ✅ Toujours au top-level, dans le même ordre à chaque rendu
function Bon({ actif }) {
  const [x, setX] = useState(0);
  useEffect(() => {
    if (actif) { /* ... */ } // la condition est DANS le hook
  }, [actif]);
}
```

**Pourquoi ?** React identifie les hooks par leur **ordre d'appel**. Un hook conditionnel décale tout l'ordre → états mélangés.

### Règle 2 : appeler les hooks uniquement dans des composants React ou des hooks personnalisés

Pas dans des fonctions utilitaires classiques, pas dans des classes.

### Règle 3 : respecter les dépendances (`exhaustive-deps`)

Le warning ESLint *"React Hook useEffect has a missing dependency"* signale une vraie source de bugs (valeur obsolète capturée). Le corriger, pas le masquer avec `// eslint-disable-next-line`.

**Pour aller plus loin :** le compilateur React (React 19, "React Compiler") mémorise automatiquement — mais les règles des hooks restent valables.

---

## 27. Data fetching — patterns loading / error

Le trio d'états standard pour tout appel réseau : **`data` / `erreur` / `chargement`**.

```jsx
import { useState, useEffect } from 'react';

function ListeEquipements() {
  const [data, setData] = useState(null);
  const [chargement, setChargement] = useState(true);
  const [erreur, setErreur] = useState(null);

  useEffect(() => {
    const controleur = new AbortController(); // pour annuler la requête

    async function charger() {
      try {
        setChargement(true);
        const res = await fetch('/api/equipements', { signal: controleur.signal });
        if (!res.ok) throw new Error(`Erreur HTTP ${res.status}`);
        setData(await res.json());
      } catch (e) {
        if (e.name !== 'AbortError') setErreur(e.message);
      } finally {
        setChargement(false);
      }
    }
    charger();

    return () => controleur.abort(); // annule si démontage / changement
  }, []);

  // Rendu selon l'état — TOUJOURS dans cet ordre
  if (chargement) return <p>⏳ Chargement des équipements…</p>;
  if (erreur) return <p className="erreur">❌ {erreur}</p>;
  if (!data || data.length === 0) return <p>Aucun équipement.</p>;

  return (
    <ul>
      {data.map((e) => <li key={e.id}>{e.nom}</li>)}
    </ul>
  );
}
```

**Les 4 états d'une requête — checklist :**
- [ ] `chargement` : skeleton ou spinner (pas de page blanche)
- [ ] `erreur` : message compréhensible + bouton "Réessayer"
- [ ] `vide` : "Aucun résultat" plutôt qu'une liste vide silencieuse
- [ ] `succès` : les données

**`AbortController` :** annule la requête si le composant se démonte ou si les dépendances changent → évite les `setState` sur composant démonté et les réponses "en retard" qui écrasent les fraîches (race condition).

---

## 28. Data fetching — exemple complet

Un composant de détail qui recharge quand l'`id` change, avec annulation et retry :

```jsx
import { useState, useEffect, useCallback } from 'react';

function DetailOnduleur({ id }) {
  const [ups, setUps] = useState(null);
  const [chargement, setChargement] = useState(true);
  const [erreur, setErreur] = useState(null);

  const charger = useCallback(async (signal) => {
    setChargement(true);
    setErreur(null);
    try {
      const res = await fetch(`/api/onduleurs/${id}`, { signal });
      if (!res.ok) throw new Error(`HTTP ${res.status} — vérifiez l'ID`);
      setUps(await res.json());
    } catch (e) {
      if (e.name !== 'AbortError') setErreur(e.message);
    } finally {
      setChargement(false);
    }
  }, [id]);

  useEffect(() => {
    const controleur = new AbortController();
    charger(controleur.signal);
    return () => controleur.abort();
  }, [charger]);

  if (chargement) return <p>⏳ Chargement de {id}…</p>;
  if (erreur) return (
    <div className="erreur">
      <p>❌ {erreur}</p>
      <button onClick={() => charger()}>Réessayer</button>
    </div>
  );
  if (!ups) return null;

  return (
    <article>
      <h2>{ups.nom}</h2>
      <dl>
        <dt>Puissance</dt><dd>{ups.puissance} kVA</dd>
        <dt>Charge</dt><dd>{ups.charge} %</dd>
        <dt>Batterie</dt><dd>{ups.batterieV} V</dd>
      </dl>
    </article>
  );
}
```

**Envoi de données (POST) :**

```jsx
async function creerTicket(ticket) {
  const res = await fetch('/api/tickets', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(ticket),
  });
  if (!res.ok) {
    const detail = await res.json().catch(() => ({}));
    throw new Error(detail.message || `HTTP ${res.status}`);
  }
  return res.json();
}
```

**Alternatives à `fetch` natif :** pour du cache, retry, déduplication → **TanStack Query** (ex React Query) ou **SWR**. Pour un dashboard avec polling, TanStack Query change la vie (voir section 55).

---

## 29. Error Boundaries — attraper les plantages

En React, une erreur JS **non rattrapée** pendant le rendu démonte **toute** l'application (écran blanc). Les **Error Boundaries** sont des composants qui capturent les erreurs de leurs enfants et affichent une UI de secours.

> ⚠️ Il n'existe **pas** de hook `useErrorBoundary` en natif : les Error Boundaries s'écrivent encore en **classe** (la seule exception légitime aux composants fonctionnels).

```jsx
import { Component } from 'react';

class ErrorBoundary extends Component {
  constructor(props) {
    super(props);
    this.state = { aErreur: false, erreur: null };
  }

  static getDerivedStateFromError(erreur) {
    return { aErreur: true, erreur }; // affiche le fallback au prochain rendu
  }

  componentDidCatch(erreur, infos) {
    // Ici : log vers Sentry / console / API interne
    console.error('Erreur capturée :', erreur, infos.componentStack);
  }

  render() {
    if (this.state.aErreur) {
      return this.props.fallback ?? (
        <div className="erreur">
          <h2>⚠️ Un module a planté</h2>
          <p>{this.state.erreur?.message}</p>
          <button onClick={() => this.setState({ aErreur: false, erreur: null })}>
            Réessayer
          </button>
        </div>
      );
    }
    return this.props.children;
  }
}

// Utilisation : isoler les zones à risque
function Dashboard() {
  return (
    <main>
      <ErrorBoundary>
        <GraphiqueTemperatures />  {/* si ça plante, le reste survit */}
      </ErrorBoundary>
      <ErrorBoundary>
        <CarteOnduleurs />
      </ErrorBoundary>
    </main>
  );
}
```

**Ce que ça capture :** erreurs de rendu, des méthodes de cycle de vie, des constructeurs des enfants.

**Ce que ça NE capture PAS :** erreurs dans les gestionnaires d'événements (try/catch manuel), le code asynchrone (`setTimeout`, promesses), l'Error Boundary elle-même, le rendu SSR partiel.

**Stratégie :** placer des boundaries **par zone fonctionnelle** (un widget qui plante ne doit pas tuer le dashboard), pas une seule au sommet.

---

## 30. Modules — organiser son code

### Imports / exports ES

```jsx
// utils/format.js
export function formatKva(v) { return `${v} kVA`; }
export function formatPct(v) { return `${v.toFixed(1)} %`; }
export default function formatDate(d) { /* ... */ }

// Composant.jsx
import formatDate, { formatKva, formatPct } from './utils/format.js';
```

|  | Export nommé | Export default |
|---|---|---|
| Syntaxe | `export function …` / `export const …` | `export default …` |
| Import | `import { nom } from '…'` (nom exact) | `import NimporteQuelNom from '…'` |
| Par fichier | Autant qu'on veut | Un seul |
| Convention React | Utilitaires, hooks | **Composants** (un par fichier) |

### Arborescence conseillée pour un projet qui grandit

```
src/
├── main.jsx
├── App.jsx
├── pages/            # écrans liés au routeur (Dashboard, Tickets…)
├── components/       # composants réutilisables (Bouton, Carte, Tableau…)
│   └── ui/
├── hooks/            # usePolling, useLocalStorage…
├── contexts/         # AuthContext, ThemeContext…
├── services/         # appels API (api.js : fetch centralisé)
├── utils/            # formatage, constantes
└── styles/
```

**Règles :**
- Un composant = un fichier = nom du fichier.
- Les appels `fetch` **jamais** en dur dans les composants : les centraliser dans `services/api.js` (base URL, headers, gestion d'erreurs commune).
- Imports absolus via alias `@` → config Vite (section 33) pour éviter `../../../../`.

```js
// services/api.js — client HTTP centralisé
const BASE = import.meta.env.VITE_API_URL ?? 'http://localhost:3000';

async function request(path, options = {}) {
  const res = await fetch(`${BASE}${path}`, {
    headers: { 'Content-Type': 'application/json' },
    ...options,
  });
  if (!res.ok) throw new Error(`API ${res.status} sur ${path}`);
  return res.status === 204 ? null : res.json();
}

export const api = {
  listerOnduleurs: () => request('/api/onduleurs'),
  detailOnduleur: (id) => request(`/api/onduleurs/${id}`),
  creerTicket: (t) => request('/api/tickets', { method: 'POST', body: JSON.stringify(t) }),
};
```

---

## 31. React Router — routes de base

**React Router** est le routeur standard : il associe des URL à des composants (navigation sans rechargement de page = SPA).

```bash
npm install react-router-dom
```

```jsx
// main.jsx
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom';
import App from './App.jsx';

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </StrictMode>
);
```

```jsx
// App.jsx
import { Routes, Route, Link } from 'react-router-dom';
import Dashboard from './pages/Dashboard.jsx';
import Tickets from './pages/Tickets.jsx';
import NonTrouve from './pages/NonTrouve.jsx';

export default function App() {
  return (
    <div>
      <nav>
        {/* Link = navigation côté client (pas de rechargement) */}
        <Link to="/">Dashboard</Link>
        <Link to="/tickets">Tickets</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Dashboard />} />
        <Route path="/tickets" element={<Tickets />} />
        {/* Route 404 : toujours en dernier */}
        <Route path="*" element={<NonTrouve />} />
      </Routes>
    </div>
  );
}
```

**`<Link>` vs `<a>` :** `<a href>` recharge toute la page (on perd l'état React). `<Link to>` navigue côté client. **Toujours `<Link>`** pour la navigation interne.

> **Note version :** React Router v6/v7 : API `createBrowserRouter` (data routers) recommandée pour les nouveaux projets. La syntaxe `<Routes>` ci-dessus reste valable et plus simple pour débuter.

---

## 32. React Router — params, navigation, layout

### Routes avec paramètres

```jsx
<Routes>
  <Route path="/onduleurs" element={<ListeOnduleurs />} />
  <Route path="/onduleurs/:id" element={<DetailOnduleur />} />
</Routes>;

// pages/DetailOnduleur.jsx
import { useParams } from 'react-router-dom';

function DetailOnduleur() {
  const { id } = useParams(); // { id: 'ups-01' } pour /onduleurs/ups-01
  // … charger les données avec id (section 28)
}
```

### Navigation programmatique

```jsx
import { useNavigate } from 'react-router-dom';

function FormulaireTicket() {
  const navigate = useNavigate();

  const envoyer = async (e) => {
    e.preventDefault();
    const ticket = await api.creerTicket(donnees);
    navigate(`/tickets/${ticket.id}`); // redirige vers le ticket créé
  };
}
```

### Layout partagé (routes imbriquées)

```jsx
import { Outlet } from 'react-router-dom';

function Layout() {
  return (
    <div className="app">
      <BarreLaterale />
      <main>
        <Outlet /> {/* la page enfant s'affiche ici */}
      </main>
    </div>
  );
}

<Routes>
  <Route element={<Layout />}>
    <Route path="/" element={<Dashboard />} />
    <Route path="/tickets" element={<Tickets />} />
  </Route>
  <Route path="/login" element={<Login />} /> {/* sans layout */}
</Routes>;
```

**Autres hooks utiles :** `useLocation()` (URL actuelle), `useSearchParams()` (query string `?tri=nom`).

```jsx
const [params, setParams] = useSearchParams();
const tri = params.get('tri') ?? 'nom';
// changer : setParams({ tri: 'puissance' })
```

---

## 33. Vite — configurer et builder

### Fichier `vite.config.js`

```js
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: { '@': '/src' }, // imports absolus : import X from '@/components/X'
  },
  server: {
    port: 5173,
    proxy: {
      // Redirige /api vers le backend en dev → évite les problèmes CORS
      '/api': { target: 'http://localhost:3000', changeOrigin: true },
    },
  },
});
```

### Variables d'environnement

- Fichier `.env` à la racine : `VITE_API_URL=https://api.interne.local`
- **Obligatoirement** préfixées par `VITE_` pour être exposées au navigateur.
- Accès : `import.meta.env.VITE_API_URL` (jamais `process.env`, c'est Node).

```bash
# .env.development
VITE_API_URL=http://localhost:3000

# .env.production
VITE_API_URL=https://api.supervision.interne
```

> ⚠️ Tout ce qui est préfixé `VITE_` est **embarqué dans le JS envoyé au navigateur** : jamais de secret/mot de passe dedans.

### Build de production

```bash
npm run build    # → dossier dist/ (HTML + JS + CSS minifiés, hashés)
npm run preview  # teste le build en local
```

Le contenu de `dist/` est **statique** : à servir avec Nginx, Apache, ou copié sur un serveur de fichiers interne. Pas besoin de Node en production pour une SPA Vite.

**Checklist build :**
- [ ] `npm run build` passe sans erreur ni warning bloquant
- [ ] Variables `VITE_*` définies pour la cible
- [ ] Tester avec `npm run preview` avant déploiement

---

## 34. Debug avec React DevTools

**React DevTools** : extension navigateur (Chrome/Firefox/Edge) indispensable. Deux onglets :

### Onglet Components

- Arborescence des composants (noms, pas divs anonymes).
- **Props et hooks** de chaque composant inspectés en direct (valeurs de `useState`, dépendances).
- *"Highlight updates"* : surligne les composants qui se re-rendent → repère les rendus en cascade.
- On peut **modifier une prop ou un état** à la volée pour tester.

### Onglet Profiler

- Enregistre une session → montre **quel composant a rendu, combien de fois, combien de temps**.
- Le workflow d'optimisation : *mesurer d'abord* avec le Profiler, *ensuite* `memo`/`useMemo` ciblés.

**Bonnes pratiques :**
- Nommer les composants (pas de `export default () => …` anonyme : le DevTools affiche "Anonymous").
- En dev, `StrictMode` double certains effets : un double appel API au montage en dev est **normal** (pas en prod).

---

## 35. Debug — stratégies et console

### La méthode en 5 étapes

1. **Reproduire** : le plus petit cas qui déclenche le bug.
2. **Lire l'erreur** : la console donne le composant + la stack. Les erreurs React ont un lien vers la doc.
3. **Isoler** : commenter / extraire le composant suspect.
4. **Vérifier les hypothèses** : `console.log` des props et de l'état aux points clés.
5. **Corriger à la racine** : pas de rustine qui masque le symptôme.

### Console utile

```jsx
console.log('props reçues :', props);
console.table(tableauDeMesures); // affichage tableau
console.time('calcul'); /* … */ console.timeEnd('calcul');
```

### Erreurs typiques et lecture

| Message | Signification |
|---|---|
| *Objects are not valid as a React child* | On essaie d'afficher un objet `{...}` directement → afficher une propriété ou `JSON.stringify` |
| *Cannot read properties of undefined* | Donnée pas encore chargée → garde `if (!data)` ou `?.` |
| *Too many re-renders* | `setState` appelé **pendant** le rendu (ex. `onClick={setX(1)}`) |
| *Each child in a list should have a unique "key"* | `key` manquante dans un `.map()` |

### Debugger VS Code

Avec l'extension "JavaScript Debugger", on peut poser des **breakpoints** directement dans le `.jsx` (source maps de Vite). Plus précis que les `console.log` en série.

---

## 36. Tests avec Vitest + Testing Library

**Vitest** : runner de tests ultra-rapide, natif pour Vite. **Testing Library** (`@testing-library/react`) : teste les composants **comme un utilisateur** (clics, saisie) plutôt que l'implémentation.

```bash
npm install -D vitest @testing-library/react @testing-library/jest-dom jsdom
```

```js
// vite.config.js — ajouter la section test
export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',       // simule le DOM du navigateur
    setupFiles: './src/test-setup.js',
    globals: true,
  },
});
```

```js
// src/test-setup.js
import '@testing-library/jest-dom'; // matchers : toBeInTheDocument(), …
```

```json
// package.json — scripts
{
  "scripts": {
    "test": "vitest",          // mode watch
    "test:run": "vitest run"   // une seule passe (CI)
  }
}
```

**Philosophie Testing Library :** on ne teste pas l'état interne ni les détails d'implémentation ; on teste ce que **voit** et **fait** l'utilisateur : texte affiché, bouton cliquable, formulaire remplissable.

**Les requêtes (par ordre de préférence) :**

| Requête | Usage |
|---|---|
| `getByRole('button', { name: 'Envoyer' })` | ✅ Rôle accessible — préférée |
| `getByLabelText('Titre')` | ✅ Champ de formulaire |
| `getByText('En ligne')` | ✅ Texte visible |
| `getByTestId('…')` | ⚠️ Dernier recours (`data-testid`) |

---

## 37. Tests — exemples commentés

```jsx
// components/Badge.test.jsx
import { render, screen, fireEvent } from '@testing-library/react';
import { describe, it, expect } from 'vitest';
import Badge from './Badge.jsx';

describe('Badge', () => {
  it('affiche le texte fourni', () => {
    render(<Badge texte="En ligne" />);
    expect(screen.getByText('En ligne')).toBeInTheDocument();
  });

  it('applique la classe critique quand niveau > 80', () => {
    const { container } = render(<Badge texte="Charge" niveau={95} />);
    expect(container.firstChild).toHaveClass('critique');
  });
});
```

```jsx
// components/Compteur.test.jsx — interaction utilisateur
import { render, screen, fireEvent } from '@testing-library/react';
import Compteur from './Compteur.jsx';

it('incrémente au clic', () => {
  render(<Compteur />);
  const bouton = screen.getByRole('button', { name: '+1' });
  fireEvent.click(bouton);
  expect(screen.getByText('Clics : 1')).toBeInTheDocument();
});
```

**Tester un appel API (mock de fetch) :**

```jsx
import { render, screen, waitFor } from '@testing-library/react';
import { vi } from 'vitest';
import ListeEquipements from './ListeEquipements.jsx';

it('affiche les équipements chargés', async () => {
  global.fetch = vi.fn().mockResolvedValue({
    ok: true,
    json: async () => [{ id: '1', nom: 'UPS-01' }],
  });

  render(<ListeEquipements />);

  // waitFor : attend l'update asynchrone
  await waitFor(() => {
    expect(screen.getByText('UPS-01')).toBeInTheDocument();
  });
  vi.restoreAllMocks();
});
```

**Que tester en priorité :** logique métier (calculs, formatage), formulaires (validation, soumission), états loading/error. **Ne pas tester** : le style CSS, les détails d'implémentation, les librairies tierces.

---

## 38. TypeScript + React

TypeScript ajoute le **typage statique** : l'éditeur détecte les erreurs avant l'exécution. Recommandé pour tout projet d'équipe ou durable.

```bash
npm create vite@latest mon-app -- --template react-ts
```

### Typer les props

```tsx
// CarteOnduleur.tsx
interface CarteOnduleurProps {
  nom: string;
  puissance: number;
  critique?: boolean;          // optionnelle
  onAlerte?: (msg: string) => void;
}

export default function CarteOnduleur({ nom, puissance, critique = false, onAlerte }: CarteOnduleurProps) {
  return (
    <div>
      <h2>{nom}</h2>
      <p>{puissance} kVA</p>
      {critique && <button onClick={() => onAlerte?.('Surchauffe')}>Alerter</button>}
    </div>
  );
}
```

### Typer l'état et les événements

```tsx
const [charge, setCharge] = useState<number>(0);
const [ups, setUps] = useState<Onduleur | null>(null); // peut être null avant chargement

interface Onduleur { id: string; nom: string; puissance: number; charge: number; }

const gererChangement = (e: React.ChangeEvent<HTMLInputElement>) => {
  setNom(e.target.value);
};
const envoyer = (e: React.FormEvent) => {
  e.preventDefault();
  // …
};
```

### Types React utiles

| Type | Usage |
|---|---|
| `React.ReactNode` | `children` quelconque |
| `React.FC<Props>` | Type de composant (optionnel, controversé) |
| `React.ChangeEvent<HTMLInputElement>` | `onChange` d'un input |
| `React.FormEvent` | `onSubmit` |

**Conseil :** typer les **frontières** (props, retours d'API, fonctions exportées) ; laisser l'inférence faire le reste. Un `any` occasionnel et assumé vaut mieux qu'un typage mensonger.

---

## 39. Sécurité — XSS et injections

### React protège par défaut contre le XSS

Tout ce qui passe dans `{}` est **échappé** automatiquement : impossible d'injecter du HTML par une variable.

```jsx
const nomSaisi = '<img src=x onerror=alert(1)>';
<p>{nomSaisi}</p>
// ✅ Affiche le texte littéralement — le script NE s'exécute PAS
```

### Le danger : `dangerouslySetInnerHTML`

```jsx
// ☠️ À ÉVITER sauf contenu 100 % maîtrisé
<div dangerouslySetInnerHTML={{ __html: htmlBrut }} />
```

**Si on doit afficher du HTML riche** (ex. description d'équipement avec mise en forme) : le **sanitizer** avec une librairie comme **DOMPurify** :

```bash
npm install dompurify
```

```jsx
import DOMPurify from 'dompurify';

<div dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(htmlBrut) }} />
```

### Checklist sécurité front

- [ ] Jamais de `dangerouslySetInnerHTML` sur du contenu utilisateur (ou + DOMPurify)
- [ ] Pas de secret dans le code front ni dans les variables `VITE_*` (visibles par tous)
- [ ] Valider aussi côté **serveur** : le front n'est jamais une barrière de sécurité
- [ ] Liens externes : `target="_blank"` **toujours** avec `rel="noopener noreferrer"` (évite le tabnabbing)
- [ ] `npm audit` régulier ; dépendances à jour (faille = souvent une lib tierce)
- [ ] Échapper les URL construites : `encodeURIComponent(id)` dans les `fetch`

```jsx
// ✅ Lien externe sécurisé
<a href="https://doc.interne" target="_blank" rel="noopener noreferrer">
  Documentation
</a>
```

---

## 40. Bonnes pratiques & style de code

### Organisation

- **Un composant = un fichier**, nommé comme le composant (`CarteOnduleur.jsx`).
- **Colocalisation** : le style, les tests et le composant vivent ensemble (`CarteOnduleur.jsx`, `.css`, `.test.jsx`).
- Extraire un composant dès qu'un bloc de JSX est **réutilisé 2 fois** ou dépasse ~50 lignes.

### Nommage

| Élément | Convention | Exemple |
|---|---|---|
| Composant | PascalCase | `CarteOnduleur` |
| Hook personnalisé | camelCase, préfixe `use` | `usePolling` |
| Fonction / variable | camelCase | `chargerMesures` |
| Constante globale | SNAKE_CASE | `SEUIL_CRITIQUE` |
| Gestionnaire d'événement | `gerer…` / `on…` (props) | `gererClic`, prop `onAlerte` |

### Règles d'or

1. **Ne jamais muter** l'état ou les props (section 12).
2. **État au plus bas niveau possible** : remonter ("lifter") uniquement si plusieurs composants en ont besoin.
3. **Dériver plutôt que dupliquer** : pas d'état calculable depuis un autre.
4. **Pas de logique métier dans le JSX** : extraire des fonctions / utilitaires.
5. **Keys stables** sur les listes (section 16).
6. **Nettoyer les effets** (section 17).
7. **Prévenir plutôt que guérir** : ESLint + TypeScript + tests sur les chemins critiques.
8. **Accessibilité de base** : `<button>` pour les actions (pas de `<div onClick>`), `label` pour chaque input, `alt` sur les images.

### Exemple de composant "propre"

```jsx
import { usePolling } from '@/hooks/usePolling';
import { formatPct } from '@/utils/format';
import './CarteUps.css';

const SEUIL_CRITIQUE = 80;

export default function CarteUps({ id }) {
  const { data: ups, erreur, chargement } = usePolling(`/api/ups/${id}`, 5000);

  if (chargement) return <div className="carte">⏳…</div>;
  if (erreur) return <div className="carte erreur">❌ {erreur}</div>;

  const critique = ups.charge > SEUIL_CRITIQUE;

  return (
    <article className={`carte ${critique ? 'critique' : ''}`}>
      <h2>{ups.nom}</h2>
      <p>Charge : {formatPct(ups.charge)}</p>
      {critique && <p className="alerte">⚠️ Charge critique</p>}
    </article>
  );
}
```

---

## 41. Performance — comprendre le rendu

### Le cycle de rendu

1. **Déclencheur** : un `setState` (ou nouvelles props du parent).
2. **Render** : React ré-exécute le composant et construit un arbre virtuel (Virtual DOM).
3. **Réconciliation** : React compare (diff) avec l'arbre précédent.
4. **Commit** : seules les différences sont appliquées au vrai DOM.

Le Virtual DOM n'est pas "magique" : un composant qui rend 10 000 lignes à chaque frappe clavier **restera lent**. Les optimisations :

| Levier | Quand |
|---|---|
| État au bon niveau | Toujours (évite les re-rendus en cascade) |
| `React.memo` | Composant coûteux re-rendu avec mêmes props |
| `useMemo` / `useCallback` | Calcul coûteux / props de composants mémorisés |
| `lazy` + `Suspense` | Code splitting par page |
| Virtualisation | Listes de centaines/milliers de lignes |
| Pagination côté serveur | Listes très longues (mieux que tout) |

### Mesurer avant d'optimiser

1. Onglet **Profiler** de React DevTools : enregistrer une interaction, repérer les composants lents / trop souvent rendus.
2. *"Highlight updates"* : visualiser les re-rendus en cascade.
3. `console.time` autour d'un calcul suspect.

**Règle :** 95 % des "lenteurs" React viennent d'un **état placé trop haut** (ex. le champ de recherche dans le même composant que le tableau de 5 000 lignes → chaque frappe re-rend tout). Descendre l'état ou séparer les composants règle le problème sans `memo`.

---

## 42. Performance — lazy, Suspense, code splitting

`React.lazy` charge un composant **à la demande** (chunk JS séparé) au lieu de l'inclure dans le bundle initial. `Suspense` affiche un fallback pendant le chargement.

```jsx
import { lazy, Suspense } from 'react';
import { Routes, Route } from 'react-router-dom';

// Ces pages ne sont téléchargées que quand on les visite
const Dashboard = lazy(() => import('./pages/Dashboard.jsx'));
const Rapports = lazy(() => import('./pages/Rapports.jsx')); // gros graphiques

function App() {
  return (
    <Suspense fallback={<p>⏳ Chargement de la page…</p>}>
      <Routes>
        <Route path="/" element={<Dashboard />} />
        <Route path="/rapports" element={<Rapports />} />
      </Routes>
    </Suspense>
  );
}
```

**Effet :** le bundle initial passe par ex. de 800 Ko à 200 Ko → première page affichée bien plus vite, crucial sur un intranet avec des postes modestes.

**Bonnes pratiques :**
- Découper **par route** en priorité (le plus rentable).
- Découper les **gros composants rarement utilisés** (éditeur riche, export PDF, graphiques lourds).
- Le `fallback` doit être léger (spinner CSS, pas un composant lourd).
- Combiner avec une Error Boundary : un chunk qui échoue à charger (déploiement entre-temps) ne doit pas écran-blanc l'app.

> **Note :** `React.lazy` ne fonctionne qu'avec les **exports default**. Avec Vite, on peut aussi utiliser l'import dynamique manuel pour précharger : `import('./pages/Rapports.jsx')` au survol d'un lien.

---

## 43. Performance — virtualisation des longues listes

Afficher 10 000 lignes d'historique d'alarmes dans le DOM = lent (mémoire + layout). La **virtualisation** ne rend que les lignes **visibles** (+ une marge).

Librairie standard : **TanStack Virtual** (ex `react-virtual`) ou `react-window`.

```bash
npm install @tanstack/react-virtual
```

```jsx
import { useRef } from 'react';
import { useVirtualizer } from '@tanstack/react-virtual';

function JournalAlertes({ alertes }) { // alertes = tableau de 10 000 entrées
  const parentRef = useRef(null);

  const virtualizer = useVirtualizer({
    count: alertes.length,
    getScrollElement: () => parentRef.current,
    estimateSize: () => 40, // hauteur estimée d'une ligne (px)
    overscan: 10,           // lignes de marge hors viewport
  });

  return (
    <div ref={parentRef} style={{ height: 400, overflow: 'auto' }}>
      <div style={{ height: virtualizer.getTotalSize(), position: 'relative' }}>
        {virtualizer.getVirtualItems().map((ligne) => {
          const alerte = alertes[ligne.index];
          return (
            <div
              key={alerte.id}
              style={{
                position: 'absolute', top: 0, left: 0, width: '100%',
                height: ligne.size, transform: `translateY(${ligne.start}px)`,
              }}
            >
              {alerte.date} — {alerte.message}
            </div>
          );
        })}
      </div>
    </div>
  );
}
```

**Quand virtualiser :** à partir de quelques centaines de lignes complexes, ou milliers de lignes simples. En dessous, un `.map()` classique suffit.

**Alternative souvent meilleure :** la **pagination côté serveur** (`?page=2&limit=50`) — moins de données transférées ET moins de DOM.

---

## 44. Intro Next.js — SSR et SSG

**Next.js** est le framework React de Vercel : routage basé sur les fichiers, rendu côté serveur, build optimisé. À connaître même si on reste sur Vite.

### Les 3 modes de rendu

| Mode | Où le HTML est généré | Cas d'usage |
|---|---|---|
| **CSR** (Client-Side Rendering) | Dans le navigateur (Vite/SPA classique) | Dashboards internes, apps derrière login |
| **SSR** (Server-Side Rendering) | Sur le serveur à chaque requête | Données fraîches + SEO |
| **SSG** (Static Site Generation) | Au build, une fois | Documentation, pages publiques, SEO max |

**Pour un dashboard interne :** le CSR (Vite) suffit largement — pas besoin de SEO, l'utilisateur est authentifié. Next.js se justifie pour un portail public, de la doc indexée, ou des performances premier affichage critiques.

### Concepts clés Next.js (App Router, v13+)

```
app/
├── page.jsx            # route "/"
├── tickets/
│   └── page.jsx        # route "/tickets"
├── layout.jsx          # layout partagé
└── onduleurs/[id]/
    └── page.jsx        # route dynamique "/onduleurs/ups-01"
```

- **Server Components** (par défaut) : s'exécutent sur le serveur, peuvent appeler la BDD directement, n'envoient pas leur JS au client.
- **Client Components** (`'use client'` en haut du fichier) : pour l'interactivité (`useState`, `onClick`) — c'est le React qu'on connaît.

```jsx
// app/onduleurs/page.jsx — Server Component : fetch direct, zéro JS client
export default async function Onduleurs() {
  const res = await fetch('https://api.interne/onduleurs');
  const data = await res.json();
  return (
    <ul>{data.map((u) => <li key={u.id}>{u.nom}</li>)}</ul>
  );
}
```

**Verdict pragmatique :** maîtriser React + Vite d'abord. Next.js quand un besoin SSR/SSG réel apparaît.

---

## 45. Erreurs classiques des débutants (15)

1. **Oublier les accolades pour une valeur non-string** : `puissance="40"` passe la *string* `"40"`, pas le nombre. → `puissance={40}`.
2. **`class` au lieu de `className`** : warning + style non appliqué.
3. **Appeler la fonction dans le gestionnaire** : `onClick={maFonction()}` l'exécute au rendu. → `onClick={maFonction}` ou `onClick={() => maFonction(arg)}`.
4. **Mutations directes de l'état** : `tableau.push(x)` puis `setTableau(tableau)` → rien ne se met à jour. → créer un nouveau tableau/objet.
5. **Lire l'état juste après `setState`** : la valeur n'est pas encore à jour (batching). → utiliser la valeur calculée localement ou un effet.
6. **`useEffect` sans tableau de dépendances** : tourne à chaque rendu → boucles infinies avec `setState` dedans.
7. **Oublier le cleanup** : `setInterval` / WebSocket qui survivent à la navigation → fuites mémoire.
8. **L'index comme `key`** dans une liste modifiable → états des lignes mélangés.
9. **Props drilling extrême** : passer des props sur 6 niveaux → penser Context ou recomposition.
10. **Tout mettre dans un seul composant géant** : 400 lignes de JSX → découper en sous-composants.
11. **`<a href>` pour la navigation interne** : recharge la page, tue l'état React. → `<Link>`.
12. **Champ contrôlé sans `onChange`** : `value={x}` seul = champ figé + warning.
13. **Afficher un objet brut** : `<p>{monObjet}</p>` → crash *"Objects are not valid as a React child"*. → `{monObjet.nom}` ou `JSON.stringify`.
14. **Le piège du `&&` avec `0`** : `{compte && <X/>}` affiche `"0"`. → `{compte > 0 && <X/>}`.
15. **Fetch dans le corps du composant** (hors `useEffect`) : requête relancée à chaque rendu → boucle. → `useEffect` + dépendances.

---

## 46. Les erreurs React les plus fréquentes — corrections

### "Too many re-renders"

```jsx
// ❌ setCompteur exécuté PENDANT le rendu → boucle infinie
<button onClick={setCompteur(compteur + 1)}>+1</button>

// ✅ Fonction passée, exécutée AU CLIC
<button onClick={() => setCompteur(compteur + 1)}>+1</button>
```

Autre cause : `setState` appelé directement dans le corps du composant (hors gestionnaire/effet).

### "Cannot read properties of undefined (reading 'nom')"

```jsx
// ❌ ups est null pendant le chargement
<h2>{ups.nom}</h2>

// ✅ Garde ou optional chaining
<h2>{ups?.nom ?? 'Chargement…'}</h2>
if (!ups) return <p>Chargement…</p>;
```

### "Each child in a list should have a unique key"

→ Ajouter `key={item.id}` stable (section 16). Si vraiment aucun id : `key={item.nom + index}` en dernier recours, jamais l'index seul sur liste mutable.

### "Objects are not valid as a React child"

```jsx
// ❌
<p>{erreur}</p>              // erreur est un objet Error !
// ✅
<p>{erreur.message}</p>
```

### Warning `exhaustive-deps`

```jsx
useEffect(() => {
  console.log(filtre); // 'filtre' utilisé mais absent des dépendances
}, []); // ⚠️ valeur obsolète capturée

// ✅
}, [filtre]);
```

### "X is not a function" sur une prop callback

La prop n'a pas été passée par le parent → valeur par défaut ou garde :

```jsx
function Enfant({ onValider = () => {} }) { /* ... */ }
// ou
onValider?.();
```

---

## 47. Cas pratique 1 — dashboard de supervision

Un mini-dashboard : cartes d'onduleurs avec polling, badge critique, temps réel simulé. Fichiers : `services/api.js`, `hooks/usePolling.js` (section 25), composants ci-dessous.

```jsx
// components/Dashboard.jsx
import { usePolling } from '../hooks/usePolling.js';
import CarteUps from './CarteUps.jsx';
import ResumeParc from './ResumeParc.jsx';

export default function Dashboard() {
  const { data: parc, erreur, chargement } = usePolling('/api/parc', 10000);

  if (chargement) return <p>⏳ Connexion à la supervision…</p>;
  if (erreur) return (
    <div className="erreur">
      <p>❌ Supervision injoignable : {erreur}</p>
      <p>Vérifiez le réseau et le service d'acquisition.</p>
    </div>
  );

  return (
    <main className="dashboard">
      <h1>Supervision énergétique</h1>
      <ResumeParc onduleurs={parc.onduleurs} />
      <div className="grille">
        {parc.onduleurs.map((ups) => (
          <CarteUps key={ups.id} ups={ups} />
        ))}
      </div>
      <p className="maj">Dernière mise à jour : {new Date(parc.horodatage).toLocaleTimeString('fr-FR')}</p>
    </main>
  );
}
```

```jsx
// components/ResumeParc.jsx — agrégats calculés pendant le rendu
import { useMemo } from 'react';

export default function ResumeParc({ onduleurs }) {
  const resume = useMemo(() => {
    const total = onduleurs.length;
    const enAlarme = onduleurs.filter((u) => u.charge > 80 || !u.enLigne).length;
    const chargeMoy = total
      ? onduleurs.reduce((s, u) => s + u.charge, 0) / total
      : 0;
    return { total, enAlarme, chargeMoy };
  }, [onduleurs]);

  return (
    <section className="resume">
      <div>🔌 {resume.total} onduleurs</div>
      <div>⚠️ {resume.enAlarme} en alarme</div>
      <div>📊 Charge moyenne : {resume.chargeMoy.toFixed(1)} %</div>
    </section>
  );
}
```

```jsx
// components/CarteUps.jsx — carte individuelle mémorisée
import { memo } from 'react';

const CarteUps = memo(function CarteUps({ ups }) {
  const critique = !ups.enLigne || ups.charge > 80;

  return (
    <article className={`carte ${critique ? 'critique' : ''}`}>
      <h2>{ups.nom}</h2>
      <p className="statut">{ups.enLigne ? '🟢 En ligne' : '🔴 Hors ligne'}</p>
      <dl>
        <dt>Charge</dt><dd>{ups.charge} %</dd>
        <dt>Batterie</dt><dd>{ups.batterieV} V</dd>
        <dt>Autonomie est.</dt><dd>{ups.autonomieMin} min</dd>
      </dl>
      {critique && <p className="alerte">⚠️ Intervention requise</p>}
    </article>
  );
});

export default CarteUps;
```

**Ce que ce cas illustre :** polling via hook personnalisé, états loading/error, `useMemo` pour les agrégats, `memo` sur les cartes, `key` stables, rendu conditionnel.

---

## 48. Cas pratique 2 — formulaire de ticket d'intervention

Formulaire complet : un seul objet d'état, validation, envoi POST, retour utilisateur.

```jsx
// pages/NouveauTicket.jsx
import { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import { api } from '../services/api.js';

const INITIAL = {
  titre: '',
  site: '',
  equipement: '',
  priorite: 'normale',
  description: '',
  urgent: false,
};

export default function NouveauTicket() {
  const [form, setForm] = useState(INITIAL);
  const [erreurs, setErreurs] = useState({});
  const [envoi, setEnvoi] = useState(false);
  const [erreurApi, setErreurApi] = useState(null);
  const navigate = useNavigate();

  // Un seul gestionnaire pour tous les champs (name → clé d'état)
  const gererChangement = (e) => {
    const { name, value, type, checked } = e.target;
    setForm((f) => ({ ...f, [name]: type === 'checkbox' ? checked : value }));
  };

  const valider = () => {
    const err = {};
    if (form.titre.trim().length < 5) err.titre = 'Titre trop court (5 caractères min).';
    if (!form.site) err.site = 'Site obligatoire.';
    if (!form.equipement) err.equipement = 'Équipement obligatoire.';
    if (form.description.trim().length < 10) err.description = 'Décrivez le problème (10 caractères min).';
    setErreurs(err);
    return Object.keys(err).length === 0;
  };

  const envoyer = async (e) => {
    e.preventDefault();
    setErreurApi(null);
    if (!valider()) return;
    setEnvoi(true);
    try {
      const ticket = await api.creerTicket(form);
      navigate(`/tickets/${ticket.id}`); // vers le ticket créé
    } catch (err) {
      setErreurApi(err.message);
    } finally {
      setEnvoi(false);
    }
  };

  const Champ = ({ label, ...props }) => (
    <label>
      {label}
      <input {...props} name={props.name} value={form[props.name]} onChange={gererChangement} />
      {erreurs[props.name] && <span className="err">{erreurs[props.name]}</span>}
    </label>
  );

  return (
    <form onSubmit={envoyer} className="form-ticket">
      <h1>Nouveau ticket d'intervention</h1>

      <Champ label="Titre" name="titre" type="text" />
      <Champ label="Site" name="site" type="text" placeholder="Ex. Datacenter Paris-Nord" />
      <Champ label="Équipement" name="equipement" type="text" placeholder="Ex. UPS-01" />

      <label>
        Priorité
        <select name="priorite" value={form.priorite} onChange={gererChangement}>
          <option value="basse">Basse</option>
          <option value="normale">Normale</option>
          <option value="haute">Haute</option>
        </select>
      </label>

      <label>
        Description
        <textarea name="description" value={form.description} onChange={gererChangement} rows={4} />
        {erreurs.description && <span className="err">{erreurs.description}</span>}
      </label>

      <label className="check">
        <input type="checkbox" name="urgent" checked={form.urgent} onChange={gererChangement} />
        Intervention urgente
      </label>

      {erreurApi && <p className="err">❌ Échec de l'envoi : {erreurApi}</p>}

      <button type="submit" disabled={envoi}>
        {envoi ? 'Envoi en cours…' : 'Créer le ticket'}
      </button>
    </form>
  );
}
```

**Points à retenir :** état objet unique + handler générique via `name`, validation avant envoi, bouton désactivé pendant l'envoi (anti double-clic), redirection après succès.

---

## 49. Cas pratique 3 — liste d'équipements filtrable

Recherche + filtre par statut + tri, avec état local bien placé.

```jsx
// pages/Parc.jsx
import { useState, useMemo } from 'react';
import { usePolling } from '../hooks/usePolling.js';
import { Link } from 'react-router-dom';

export default function Parc() {
  const { data: equipements, chargement, erreur } = usePolling('/api/equipements', 15000);
  const [recherche, setRecherche] = useState('');
  const [statut, setStatut] = useState('tous'); // tous | ligne | alarme | hs
  const [tri, setTri] = useState('nom');

  const visibles = useMemo(() => {
    if (!equipements) return [];
    const r = recherche.toLowerCase();
    const filtres = equipements.filter((e) => {
      const okRecherche = e.nom.toLowerCase().includes(r) || e.site.toLowerCase().includes(r);
      const okStatut = statut === 'tous' ||
        (statut === 'ligne' && e.enLigne && e.charge <= 80) ||
        (statut === 'alarme' && e.enLigne && e.charge > 80) ||
        (statut === 'hs' && !e.enLigne);
      return okRecherche && okStatut;
    });
    return [...filtres].sort((a, b) =>
      tri === 'charge' ? b.charge - a.charge : a.nom.localeCompare(b.nom)
    );
  }, [equipements, recherche, statut, tri]);

  if (chargement) return <p>⏳ Chargement du parc…</p>;
  if (erreur) return <p className="erreur">❌ {erreur}</p>;

  return (
    <main>
      <h1>Parc d'équipements</h1>

      <div className="filtres">
        <input
          type="search"
          placeholder="Rechercher (nom, site)…"
          value={recherche}
          onChange={(e) => setRecherche(e.target.value)}
          aria-label="Rechercher un équipement"
        />
        <select value={statut} onChange={(e) => setStatut(e.target.value)} aria-label="Filtrer par statut">
          <option value="tous">Tous statuts</option>
          <option value="ligne">En ligne</option>
          <option value="alarme">En alarme</option>
          <option value="hs">Hors service</option>
        </select>
        <select value={tri} onChange={(e) => setTri(e.target.value)} aria-label="Trier par">
          <option value="nom">Tri : nom</option>
          <option value="charge">Tri : charge ↓</option>
        </select>
      </div>

      <p>{visibles.length} équipement(s) affiché(s)</p>

      {visibles.length === 0 ? (
        <p>Aucun équipement ne correspond aux critères.</p>
      ) : (
        <table>
          <thead>
            <tr><th>Nom</th><th>Site</th><th>Charge</th><th>Statut</th></tr>
          </thead>
          <tbody>
            {visibles.map((e) => (
              <tr key={e.id}>
                <td><Link to={`/equipements/${e.id}`}>{e.nom}</Link></td>
                <td>{e.site}</td>
                <td>{e.charge} %</td>
                <td>{e.enLigne ? (e.charge > 80 ? '⚠️ Alarme' : '🟢 En ligne') : '🔴 HS'}</td>
              </tr>
            ))}
          </tbody>
        </table>
      )}
    </main>
  );
}
```

**Ce que ce cas illustre :** `useMemo` pour filtrer/trier (recalcul uniquement si les critères changent), état des filtres **local** au composant (pas besoin de le remonter), `key` sur les lignes, message "aucun résultat".

---

## 50. Checklist — mettre une app en production

### Avant le build
- [ ] `StrictMode` conservé (aucun warning en console dev)
- [ ] ESLint : zéro erreur (les warnings `react-hooks/*` corrigés)
- [ ] `console.log` de debug supprimés
- [ ] Variables `VITE_*` documentées (`.env.example` fourni)
- [ ] Aucun secret / mot de passe dans le code ou les `.env` front

### Build & tests
- [ ] `npm run build` OK, tester avec `npm run preview`
- [ ] `npm run test:run` : tous les tests passent
- [ ] Taille du bundle vérifiée (découper avec `lazy` si > 500 Ko initial)

### Robustesse
- [ ] Error Boundaries sur les zones à risque (graphiques, widgets)
- [ ] Tous les appels API gèrent loading / error / empty
- [ ] Page 404 (`path="*"`) définie
- [ ] Formulaires : validation + anti double-submit

### Sécurité
- [ ] Pas de `dangerouslySetInnerHTML` non sanitizé
- [ ] `rel="noopener noreferrer"` sur les `target="_blank"`
- [ ] `npm audit` sans vulnérabilité critique

### Déploiement (SPA statique)
- [ ] Servir `dist/` via Nginx/Apache
- [ ] **Fallback SPA** : toute route inconnue → `index.html` (sinon refresh sur `/tickets` = 404 serveur). Ex. Nginx : `try_files $uri /index.html;`
- [ ] En-têtes cache : assets hashés = cache long ; `index.html` = `no-cache`

---

## 51. Pense-bête de poche

### Créer & lancer
```bash
npm create vite@latest mon-app -- --template react
npm install && npm run dev
```

### Composant minimal
```jsx
export default function MonComposant({ titre }) {
  return <h1>{titre}</h1>;
}
```

### Les 8 hooks essentiels
| Hook | Usage en une phrase |
|---|---|
| `useState` | Mémoire du composant → re-rend quand ça change |
| `useEffect` | Effet de bord après rendu (API, timer, abonnement) |
| `useRef` | Réf DOM ou boîte mutable sans re-rendu |
| `useMemo` | Mémorise un **calcul** coûteux |
| `useCallback` | Mémorise une **fonction** (props stables) |
| `useContext` | Lit un contexte (sans prop drilling) |
| `useReducer` | État complexe à transitions nommées |
| `useId` | (bonus) Génère un id unique accessible |

### Les 5 réflexes
1. `UI = f(state)` — décrire l'état, pas manipuler le DOM.
2. Immuable : `[...t, x]`, `{...o, k: v}` — jamais `push` / assignation directe.
3. `useEffect` : toujours un tableau de dépendances + cleanup si abonnement.
4. Listes : `key` stable et unique (jamais l'index sur liste mutable).
5. Dériver pendant le rendu plutôt que dupliquer dans l'état.

### Snippets VS Code (extension ES7+)
- `rafce` → composant fonctionnel avec export
- `useState` → `const [x, setX] = useState()`
- `useEffect` → squelette d'effet

---

## 52. Glossaire

| Terme | Définition |
|---|---|
| **Babel / esbuild** | Transpilers : convertissent JSX/TS en JS compris par le navigateur |
| **Batching** | Regroupement de plusieurs `setState` en un seul re-rendu |
| **Bundler** | Outil qui assemble les modules en fichiers optimisés (Vite utilise Rollup en build) |
| **Cleanup** | Fonction retournée par `useEffect`, exécutée avant ré-exécution et au démontage |
| **Client Component** | (Next.js) Composant interactif exécuté dans le navigateur (`'use client'`) |
| **Code splitting** | Découpage du bundle en morceaux chargés à la demande (`React.lazy`) |
| **Composant contrôlé** | Champ de formulaire dont la valeur est pilotée par l'état React |
| **CSR** | Client-Side Rendering : le HTML est construit dans le navigateur |
| **Dépendance (effet)** | Valeur du tableau de `useEffect` qui redéclenche l'effet quand elle change |
| **Hydratation** | (SSR) React "réhydrate" le HTML serveur en y attachant l'interactivité |
| **HMR** | Hot Module Replacement : rechargement à chaud sans perdre l'état (Vite) |
| **Hook** | Fonction `use*` qui branche un composant aux fonctionnalités React (état, effets…) |
| **Immuabilité** | Ne jamais modifier un objet/tableau existant : en créer un nouveau |
| **JSX** | Extension de syntaxe : écrire du HTML-like dans du JavaScript |
| **Key** | Identifiant stable aidant React à suivre les éléments d'une liste |
| **Lifting state up** | Remonter l'état vers le parent commun quand plusieurs enfants le partagent |
| **Memoïsation** | Mettre en cache un résultat pour éviter de le recalculer (`memo`, `useMemo`) |
| **Prop drilling** | Passer des props à travers des composants intermédiaires qui n'en ont pas besoin |
| **Props** | Données passées du parent à l'enfant (lecture seule) |
| **Réconciliation** | Algorithme de diff entre deux arbres virtuels |
| **Server Component** | (Next.js) Composant exécuté sur le serveur, sans JS envoyé au client |
| **SPA** | Single Page Application : une seule page HTML, navigation côté client |
| **SSG** | Static Site Generation : HTML généré une fois au build |
| **SSR** | Server-Side Rendering : HTML généré sur le serveur à chaque requête |
| **State** | État local d'un composant ; tout changement déclenche un re-rendu |
| **StrictMode** | Mode dev qui révèle les effets de bord (double invocation) |
| **SyntheticEvent** | Wrapper d'événement normalisé par React |
| **Virtual DOM** | Représentation mémoire du DOM utilisée pour calculer les différences |
| **XSS** | Cross-Site Scripting : injection de script via des données non échappées |

---

## 53. Quiz — 10 questions + réponses

**Q1. Que se passe-t-il quand on appelle `setCompteur(compteur + 1)` deux fois de suite ?**
> Le compteur n'augmente que de 1 : les deux appels capturent l'ancienne valeur (batching). Solution : forme fonctionnelle `setCompteur(c => c + 1)`.

**Q2. Pourquoi ce code est-il interdit ? `if (actif) { const [x] = useState(0); }`**
> Les hooks doivent être appelés dans le même ordre à chaque rendu, au niveau racine. Un hook conditionnel casse cet ordre → états mélangés. Mettre la condition *dans* le hook.

**Q3. À quoi sert le tableau de dépendances de `useEffect` ?**
> À déclarer les valeurs dont dépend l'effet : `[]` = une seule fois au montage ; `[a, b]` = au montage + quand `a` ou `b` change ; absent = après chaque rendu (rarement voulu).

**Q4. Pourquoi ne faut-il pas utiliser l'index comme `key` dans une liste modifiable ?**
> React identifie les éléments par leur `key`. Avec l'index, une suppression/réordonnancement fait croire à React que c'est un autre élément qui a changé → états locaux des lignes mélangés, bugs fantômes.

**Q5. Quelle est la différence entre `useMemo` et `useCallback` ?**
> `useMemo` mémorise le *résultat* d'un calcul ; `useCallback` mémorise une *fonction* (référence stable). `useCallback(fn, deps)` équivaut à `useMemo(() => fn, deps)`.

**Q6. Pourquoi ce champ est-il figé ? `<input type="text" value={nom} />`**
> Champ contrôlé sans `onChange` : React impose `value` à chaque rendu, l'utilisateur ne peut rien saisir. Ajouter `onChange={(e) => setNom(e.target.value)}`.

**Q7. Comment éviter qu'une erreur dans un widget ne fasse écran blanc sur tout le dashboard ?**
> Envelopper chaque zone à risque dans une **Error Boundary** (composant classe avec `getDerivedStateFromError` / `componentDidCatch`) qui affiche une UI de secours.

**Q8. `const [v, setV] = useState(0); setV(1); console.log(v);` — qu'affiche le log ?**
> `0` : `setV` ne modifie pas la variable immédiatement, il planifie un nouveau rendu où `v` vaudra 1.

**Q9. Quand utiliser `useReducer` plutôt que `useState` ?**
> Quand l'état comporte plusieurs sous-valeurs liées ou des transitions complexes : le reducer centralise les transitions nommées (`{ type: 'PANNE' }`), testables car fonction pure.

**Q10. Pourquoi `<a href="/tickets">` est-il déconseillé pour la navigation interne ?**
> Il recharge toute la page : on perd l'état React et on retélécharge le bundle. Utiliser `<Link to="/tickets">` de React Router (navigation côté client).

---

## 54. Pour aller plus loin

### Librairies incontournables pour un SI interne
| Besoin | Librairie | Pourquoi |
|---|---|---|
| Data fetching + cache | **TanStack Query** | Cache, retry, polling, déduplication — remplace 80 % des `useEffect` de fetch |
| Formulaires complexes | **React Hook Form** | Performant (non contrôlé), validation intégrée (Zod) |
| Graphiques | **Recharts** / **ECharts** | Courbes de charge, températures, historiques |
| Tableaux avancés | **TanStack Table** | Tri, filtres, pagination, virtualisation |
| État global | **Zustand** | Simple, sans boilerplate (alternative à Redux) |
| Dates | **date-fns** | Léger, immutable, tree-shakable |

### Sujets à creuser ensuite
- **TanStack Query** en détail : `useQuery`, `useMutation`, invalidation du cache.
- **Zustand** : store global en ~20 lignes pour les préférences / l'utilisateur connecté.
- **React Hook Form + Zod** : validation typée des formulaires.
- **Tests E2E** : Playwright pour les parcours critiques (créer un ticket de bout en bout).
- **PWA** : transformer le dashboard en app installable (mode hors-ligne du technicien sur site).
- **Accessibilité (a11y)** : ARIA, navigation clavier, contrastes — obligatoire pour un outil utilisé quotidiennement.
- **i18n** : `react-i18next` si l'outil doit exister en FR/EN.

### Ressources officielles
- https://react.dev — la documentation officielle (tutoriels, référence des hooks) — **le point de départ n°1**
- https://vite.dev — documentation Vite
- https://reactrouter.com — documentation React Router
- https://tanstack.com — TanStack Query / Table / Virtual

### Feuille de route conseillée (4 semaines, à temps partiel)
- **Semaine 1** : sections 1→18 de ce guide + mini-app "liste d'équipements" (useState, props, map/keys).
- **Semaine 2** : sections 19→30 + dashboard avec `usePolling` et Error Boundaries.
- **Semaine 3** : React Router + formulaire de ticket + tests Vitest sur les utilitaires.
- **Semaine 4** : TypeScript (convertir un composant), TanStack Query, build + déploiement Nginx.

---

*Fin du guide — bon code, et que vos dashboards restent verts. 🟢*
