# Guide Rust ultra-complet — Langage système sûr, rapide et moderne

> **Pour qui ?** Sysadmins et chefs de service systèmes & énergies qui veulent écrire des outils
> système fiables, des CLI performantes et des binaires portables sans sacrifier la sécurité mémoire.
> **Niveau :** de zéro à autonome sur projets réels. **Langue :** français.
> **Édition Rust visée :** 2021 et 2024 (les différences sont signalées explicitement).
> **Ton :** direct, tutoriel + référence, dense. Zéro remplissage.

---

## 1. Pourquoi Rust quand on est sysadmin

- **Binaires uniques, statiques par défaut** : un outil compilé en Rust s'exécute sur une machine
  cible sans runtime, sans interpréteur, sans dépendances dynamiques (ou presque). Idéal pour
  déployer sur des serveurs, des appliances, des conteneurs minimaux.
- **Vitesse du C, sécurité en plus** : le *borrow checker* garantit à la compilation l'absence
  de *use-after-free*, de double `free`, de data races. Fini les segfaults en prod à 3h du matin.
- **Gestion d'erreurs explicite** : pas d'exceptions qui surgissent de nulle part.
  `Result<T, E>` force à traiter les pannes (disque plein, réseau coupé) là où elles arrivent.
- **Excellent outillage** : `cargo` (build, dépendances, tests, doc, publish) fait en une commande
  ce qui demande ailleurs un Makefile + pip + venv + pytest + sphinx.
- **Cas d'usage typiques** : CLI d'inventaire, agents de supervision, exporters Prometheus,
  outils de sauvegarde, parsers de logs, daemons réseau, utilitaires de migration.

> Pense-bête : si ton script Python devient critique, lent ou fragile → candidat à la réécriture en Rust.

---

## 2. Installation avec rustup (la seule méthode recommandée)

Ne passe JAMAIS par le gestionnaire de paquets de la distro en premier choix (`apt install rustc`
fournit souvent une version ancienne). Utilise **rustup**, le gestionnaire de chaînes d'outils officiel.

```bash
# Linux / macOS
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
# Choisir l'option 1 (installation par défaut), puis :
source "$HOME/.cargo/env"

# Vérification
rustc --version   # ex : rustc 1.82.0 (f20d54c8a 2024-10-17)
cargo --version
rustup --version
```

```powershell
# Windows : télécharger rustup-init.exe depuis https://rustup.rs
# puis dans PowerShell :
rustc --version
cargo --version
```

### Toolchains : stable, beta, nightly

| Canal | Usage |
|---|---|
| `stable` | Production, par défaut. Mises à jour toutes les 6 semaines. |
| `beta` | Prévisualisation de la prochaine stable. |
| `nightly` | Fonctionnalités expérimentales (`#![feature(...)]`). Instable par définition. |

```bash
rustup toolchain install nightly          # ajouter nightly
rustup default stable                      # toolchain par défaut
rustup override set nightly                # nightly pour le dossier courant
rustup update                              # tout mettre à jour
rustup component add rustfmt clippy        # formateur + linter (indispensables)
rustup component add rust-analyzer         # serveur de langage (IDE)
rustup target add x86_64-unknown-linux-musl  # cible pour binaires 100 % statiques
```

> **Édition** : depuis Rust 1.56 (édition 2021), `cargo new` crée des projets en édition 2021.
> L'édition 2024 est stable depuis Rust 1.85. On la choisit explicitement (voir §62).

---

## 3. Premier programme et anatomie d'un projet Cargo

```bash
cargo new mon_outil        # crée mon_outil/ (binaire)
cargo new --lib ma_lib     # crée ma_lib/ (bibliothèque)
cd mon_outil
cargo run                  # compile + exécute
cargo build                # compile (debug)
cargo build --release      # compile optimisé
```

Arborescence générée :

```
mon_outil/
├── Cargo.toml      # manifeste : nom, version, édition, dépendances
├── Cargo.lock      # versions exactes résolues (à committer pour les binaires)
└── src/
    └── main.rs     # point d'entrée
```

`src/main.rs` minimal :

```rust
fn main() {
    println!("Hello, infra !");
}
```

`Cargo.toml` typique :

```toml
[package]
name = "mon_outil"
version = "0.1.0"
edition = "2021"          # ou "2024"

[dependencies]
# serde = "1.0"           # exemple de dépendance (crates.io)

[profile.release]
opt-level = 3
lto = true                # Link Time Optimization : binaire plus petit/rapide
strip = true              # supprime les symboles de debug
```

Checklist premier projet :

- [ ] `rustup component add rustfmt clippy` exécuté
- [ ] Édition choisie consciemment dans `Cargo.toml`
- [ ] `cargo run` fonctionne
- [ ] Éditeur configuré avec rust-analyzer

---

## 4. `cargo` en détail : le couteau suisse

| Commande | Rôle |
|---|---|
| `cargo new` / `cargo init` | Créer un projet |
| `cargo build` / `cargo build --release` | Compiler |
| `cargo run [--release] [-- <args>]` | Compiler + exécuter (args après `--`) |
| `cargo check` | Vérification rapide sans produire de binaire (itération rapide) |
| `cargo test` | Lancer les tests (unitaires, intégration, doc) |
| `cargo doc --open` | Générer/ouvrir la documentation |
| `cargo fmt` | Formater le code (rustfmt) |
| `cargo clippy` | Linter : détecte les maladresses |
| `cargo add <crate>` | Ajouter une dépendance (nécessite `cargo-edit`, ou édition manuelle) |
| `cargo update` | Mettre à jour les dépendances (respecte `Cargo.lock`) |
| `cargo tree` | Arbre des dépendances |
| `cargo audit` | Vulnérabilités connues (via `cargo install cargo-audit`) |
| `cargo install <crate>` | Installer un binaire Rust (ex : `cargo install ripgrep`) |
| `cargo clean` | Supprimer les artefacts de build |

```bash
cargo run -- --help              # passer des arguments au programme
cargo test -- --nocapture        # voir les println! dans les tests
cargo build --target x86_64-unknown-linux-musl   # binaire statique
```

> Astuce sysadmin : `cargo check` est 3 à 5× plus rapide que `cargo build` pendant le
> développement. Réserve `build --release` à la livraison.

---

## 5. Syntaxe de base : variables, mutabilité, constantes

```rust
fn main() {
    let x = 5;            // immutable par défaut (choix de sécurité)
    // x = 6;             // ERREUR : cannot assign twice to immutable variable
    let mut y = 5;        // mutable explicite
    y = 6;

    let z = 5;
    let z = z + 1;        // shadowing : redéclare, peut changer de type
    let z = "texte";      // légal grâce au shadowing

    const MAX_RETRY: u32 = 3;                 // constante : type obligatoire, SCREAMING_CASE
    static COMPTEUR: u32 = 0;                 // variable statique (rare ; voir Mutex pour mutabilité)
}
```

| Mot-clé | Caractéristique |
|---|---|
| `let` | Liaison immutable par défaut |
| `let mut` | Liaison mutable |
| Shadowing (`let x = ...` répété) | Nouvelle variable, type modifiable, portée limitée |
| `const` | Constante évaluée à la compilation, toujours typée, inlinée |
| `static` | Emplacement mémoire fixe, durée de vie `'static` |

---

## 6. Types primitifs : le tableau de référence

| Catégorie | Types | Détails |
|---|---|---|
| Entiers signés | `i8` `i16` `i32` `i64` `i128` `isize` | `i32` par défaut ; `isize` = taille d'un pointeur |
| Entiers non signés | `u8` `u16` `u32` `u64` `u128` `usize` | `usize` pour indexer les collections |
| Flottants | `f32` `f64` | `f64` par défaut (IEEE 754) |
| Booléen | `bool` | `true` / `false`, 1 octet |
| Caractère | `char` | 4 octets, Unicode scalaire (`'é'`, `'🦀'`) |
| Unité | `()` | Type « rien », retour des fonctions sans valeur |
| Jamais | `!` | Type « ne retourne jamais » (`panic!`, boucles infinies) |

```rust
fn main() {
    let a: i32 = -42;
    let b = 42u8;                       // suffixe de type sur littéral
    let c = 0xFF_FF;                    // hexadécimal, _ comme séparateur
    let d = 0b1010_0110;                // binaire
    let e = 1_000_000;                  // lisibilité
    let f = 3.14f32;
    let g: bool = true;
    let h: char = '🦀';                 // la mascotte de Rust : Ferris le crabe

    // Conversions : explicites, jamais implicites
    let petit: u8 = 200;
    let grand: u32 = petit as u32;      // `as` : transtypage (tronque si dépassement)
    let sur: i32 = petit as i32;

    // Dépassement d'entier : panic en debug, wrap en release !
    // let boom: u8 = 255 + 1;          // panic en debug
    let wrap = 255u8.wrapping_add(1);    // 0, explicite
    let sat = 255u8.saturating_add(1);  // 255, explicite
    let chk = 255u8.checked_add(1);     // None, explicite
    println!("{wrap} {sat} {chk:?}");
}
```

> ⚠️ En `debug`, un dépassement arithmétique fait paniquer le programme.
> En `release`, il s'enroule silencieusement (wrapping). Utilise les méthodes
> `wrapping_*` / `saturating_*` / `checked_*` / `overflowing_*` quand le dépassement est possible.

---

## 7. Tuples, tableaux, slices

```rust
fn main() {
    // Tuple : taille fixe, types hétérogènes
    let srv: (&str, u16, bool) = ("srv-web-01", 443, true);
    let nom = srv.0;                    // accès par index
    let (hote, port, _actif) = srv;     // déstructuration

    // Tableau : taille fixe, type homogène, sur la pile
    let ports: [u16; 4] = [80, 443, 8080, 8443];
    let zeros = [0u8; 16];              // [valeur; taille]
    println!("{}", ports[1]);           // indexation (panic si hors bornes)

    // Slice : vue sur une séquence contiguë (taille inconnue à la compilation)
    let tranche: &[u16] = &ports[1..3]; // [443, 8080]
    println!("{:?}", tranche);

    // Chaînes : &str (vue) vs String (possédée) — voir § ownership
    let s1: &str = "immuable, dans le binaire";
    let mut s2 = String::from("modifiable, sur le tas");
    s2.push_str(" !");
}
```

| Structure | Taille | Où | Copiable ? |
|---|---|---|---|
| Tuple | Fixe (connue à la compilation) | Pile (si ses éléments y sont) | Si tous les éléments sont `Copy` |
| `[T; N]` | Fixe | Pile | Si `T: Copy` |
| `&[T]` (slice) | Dynamique (fat pointer : ptr + len) | Vue sur pile/tas/statique | Oui (la vue, pas les données) |
| `Vec<T>` | Dynamique | Tas | Non (ownership) |

---

## 8. Contrôle de flux : if, boucles, labels

```rust
fn main() {
    // if est une EXPRESSION : retourne une valeur
    let charge = 78;
    let statut = if charge > 90 { "critique" } else if charge > 70 { "alerte" } else { "ok" };

    // loop : boucle infinie, `break` peut retourner une valeur
    let mut n = 0;
    let resultat = loop {
        n += 1;
        if n == 10 { break n * 2; }      // break avec valeur
    };

    // while classique
    while n > 0 { n -= 1; }

    // for sur itérateur (pas d'index manuel = pas d'erreur off-by-one)
    let ports = [80, 443, 8080];
    for p in ports {                     // `ports` est Copy ([u16; 3]) : pas de move ici
        println!("port {p}");
    }
    for (i, p) in ports.iter().enumerate() {
        println!("{i}: {p}");
    }
    for i in 1..=5 {                     // 1..=5 inclusif ; 1..5 exclusif
        println!("{i}");
    }

    // Labels pour sortir de boucles imbriquées
    'exterieur: loop {
        loop {
            break 'exterieur;            // sort des DEUX boucles
        }
    }
}
```

> En Rust, `if`/`match`/`loop` sont des expressions : on écrit `let x = if ... { ... } else { ... };`.
> Les blocs retournent la dernière expression **sans point-virgule**.

---

## 9. Fonctions : signatures, retours, expressions

```rust
// snake_case obligatoire (clippy râle sinon)
fn ping(hote: &str, timeout_ms: u64) -> bool {
    // pas de `return` nécessaire : dernière expression = valeur de retour
    !hote.is_empty() && timeout_ms > 0
}

fn afficher(b: bool) {                  // -> () implicite
    println!("joignable : {b}");
}

// Fonctions divergentes : ne retournent jamais
fn mourir(msg: &str) -> ! {
    eprintln!("FATAL : {msg}");
    std::process::exit(1);
}

fn main() {
    let ok = ping("10.0.0.1", 500);
    afficher(ok);
    // mourir("test");                   // le programme s'arrête ici
}
```

Règles d'or :

- Les paramètres et le retour sont **toujours typés** (pas d'inférence sur les signatures).
- Le corps est un bloc d'expressions ; `return` existe mais on le réserve aux sorties anticipées.
- Point-virgule final = la fonction retourne `()` au lieu de la valeur. C'est l'erreur n°1 des débutants.

---

## 10. Ownership : le concept central (pas à pas)

L'ownership répond à UNE question : **qui libère la mémoire, et quand ?**
En C tu appelles `free`, en Go/Java un GC s'en charge, en Rust **le compilateur** insère la
libération automatiquement grâce à 3 règles.

### Règle 1 — Chaque valeur a exactement un propriétaire

```rust
fn main() {
    let s = String::from("données");  // s est propriétaire de la String
}                                    // fin de portée : s est "droppée", mémoire libérée
```

### Règle 2 — Un seul propriétaire à la fois (move)

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;              // MOVE : s1 n'est plus valide
    // println!("{s1}");      // ERREUR : borrow of moved value
    println!("{s2}");         // OK
}
```

Pourquoi ? `String` = (pointeur, longueur, capacité) sur la pile + octets sur le tas.
Copier bêtement le pointeur donnerait deux propriétaires → double `free` → faille.
Rust **invalide l'ancien propriétaire** au lieu de copier : c'est le *move*.

### Les types `Copy` : l'exception

```rust
fn main() {
    let a: i32 = 5;
    let b = a;                // COPIE bit à bit, a reste valide
    println!("{a} {b}");      // OK : i32 est Copy (entiers, flottants, bool, char, tuples de Copy...)
}
```

`Copy` = données entièrement sur la pile, copie triviale. `String`, `Vec<T>`, `Box<T>` ne sont PAS `Copy`.

### Règle 3 — Quand le propriétaire sort de portée, la valeur est droppée

```rust
fn prendre(s: String) {       // s devient propriétaire (move depuis l'appelant)
    println!("{s}");
}                             // s droppée ici

fn main() {
    let s = String::from("x");
    prendre(s);               // move
    // println!("{s}");       // ERREUR : valeur déplacée
}
```

Pour **prêter** sans transférer : les références, §11.

> 🧠 Modèle mental : une valeur = un jeton physique. Tu peux le donner (move), le prêter
> (borrow), ou le cloner explicitement (`s.clone()` = copie profonde, coûteuse mais explicite).

---

## 11. Borrowing : références immuables et mutables

```rust
fn longueur(s: &String) -> usize {   // &String : emprunt immuable
    s.len()
}                                   // la référence meurt, RIEN n'est libéré

fn ajouter(s: &mut String) {         // &mut String : emprunt mutable
    s.push_str(" + suffixe");
}

fn main() {
    let s = String::from("hello");
    println!("len = {}", longueur(&s));   // on prête, on garde la propriété
    println!("{s}");                      // s toujours valide

    let mut m = String::from("hello");
    ajouter(&mut m);
    println!("{m}");
}
```

### Les deux règles d'emprunt (le cœur du borrow checker)

1. **OU** un nombre quelconque de références immuables `&T`,
2. **OU** exactement **une** référence mutable `&mut T`.

Jamais les deux en même temps. C'est ce qui élimine les data races **à la compilation**.

```rust
fn main() {
    let mut s = String::from("hello");
    let r1 = &s;              // OK : emprunt immuable
    let r2 = &s;              // OK : plusieurs immuables
    // let r3 = &mut s;       // ERREUR : cannot borrow as mutable (r1, r2 actifs)
    println!("{r1} {r2}");     // r1, r2 utilisés ici...
    let r3 = &mut s;          // OK maintenant (NLL : les emprunts meurent à leur dernier usage)
    r3.push('!');
}
```

> **NLL (Non-Lexical Lifetimes**, édition 2018+) : un emprunt se termine à son **dernier usage**,
> pas à la fin du bloc. Le code ci-dessus compile grâce à ça.

---

## 12. Lifetimes : quand le compilateur a besoin d'aide

La plupart du temps, les durées de vie sont **élidées** (déduites). Elles deviennent explicites
quand une fonction **retourne une référence** liée à ses paramètres.

```rust
// Le compilateur refuse : de quel paramètre vient la référence retournée ?
// fn plus_long(x: &str, y: &str) -> &str {
//     if x.len() > y.len() { x } else { y }
// }

// On annote : 'a = "vit au moins aussi longtemps que x ET y"
fn plus_long<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}

fn main() {
    let a = String::from("court");
    let b = String::from("beaucoup plus long");
    let r = plus_long(&a, &b);
    println!("{r}");
}
```

### Les 3 règles d'élision (quand on peut omettre `'a`)

1. Chaque paramètre référence reçoit sa propre lifetime.
2. S'il n'y a **qu'un** paramètre référence en entrée, sa lifetime est donnée à la sortie.
3. Si un paramètre est `&self`/`&mut self` (méthode), sa lifetime est donnée à la sortie.

```rust
fn premier_mot(s: &str) -> &str {   // OK par la règle 2 : fn premier_mot<'a>(s: &'a str) -> &'a str
    s.split_whitespace().next().unwrap_or("")
}
```

### `'static` : vit jusqu'à la fin du programme

```rust
let s: &'static str = "compilé dans le binaire";   // littéraux de chaîne : toujours 'static
```

> En pratique : tu écris rarement des lifetimes à la main sauf pour des fonctions qui
> retournent des références, des structs qui contiennent des références (§13), et du code async.

---

## 13. Structs : données structurées

```rust
// Struct classique (champs nommés)
struct Serveur {
    hostname: String,
    ip: String,
    port: u16,
    actif: bool,
}

// Tuple struct (champs anonymes)
struct Coordonnees(f64, f64);

// Unit struct (marqueur, zéro octet)
struct Supervision;

// Struct avec lifetime : contient une référence empruntée
struct Vue<'a> {
    extrait: &'a str,
}

impl Serveur {
    // Méthode associée (constructeur idiomatique) — pas de `self`
    fn new(hostname: &str, ip: &str, port: u16) -> Self {
        Self { hostname: hostname.to_string(), ip: ip.to_string(), port, actif: true }
    }

    // Méthode : &self = emprunt immuable
    fn adresse(&self) -> String {
        format!("{}:{}", self.ip, self.port)
    }

    // Méthode : &mut self = emprunt mutable
    fn desactiver(&mut self) {
        self.actif = false;
    }
}

fn main() {
    let mut srv = Serveur::new("web-01", "10.0.0.11", 443);
    println!("{}", srv.adresse());
    srv.desactiver();

    // Syntaxe de mise à jour struct (struct update syntax)
    let srv2 = Serveur { hostname: "web-02".into(), ..srv };  // srv partiellement déplacé !
    // println!("{}", srv.hostname);  // ERREUR : hostname a été déplacé dans srv2
    println!("{}", srv.ip);           // ERREUR aussi : `..srv` déplace le reste
}
```

| Forme | Usage typique |
|---|---|
| `struct S { champ: T }` | Données métier (99 % des cas) |
| `struct S(T, U)` | Newtype pattern : `struct Port(u16)` pour typer fort |
| `struct S;` | Marqueur de type, zéro coût |

> **Newtype pattern** : `struct Metres(f64);` vs `struct Secondes(f64);` — le compilateur
> t'empêche de mélanger des unités. Précieux pour configs réseau/énergie.

---

## 14. Enums : le couteau suisse de la modélisation

```rust
// Enum simple (comme en C)
enum Statut { Ok, Alerte, Critique }

// Enum avec données : chaque variante est un "constructeur" différent
enum Evenement {
    Ping { hote: String, latence_ms: u64 },  // variante struct
    Log(String),                              // variante tuple
    Arret,                                    // variante unit
    Metrique(String, f64),
}

// Enum générique : le célèbre Option<T>
enum MonOption<T> { Some(T), None }

// Méthodes sur enum
impl Evenement {
    fn resume(&self) -> String {
        match self {
            Evenement::Ping { hote, latence_ms } => format!("ping {hote}: {latence_ms}ms"),
            Evenement::Log(msg) => format!("log: {msg}"),
            Evenement::Arret => "arrêt".to_string(),
            Evenement::Metrique(nom, val) => format!("{nom}={val}"),
        }
    }
}
```

> Contrairement au C, une enum Rust peut porter des données **et** le compilateur connaît
> toujours la variante active (tag implicite). C'est la base de `Option` et `Result`.

---

## 15. Pattern matching : `match` et `if let`

`match` est **exhaustif** : le compilateur exige de traiter TOUS les cas. Oublier un cas = erreur
de compilation, pas bug en prod.

```rust
enum Alarme { Temperature(f64), Disque(u8), Reseau(String) }

fn traiter(a: Alarme) {
    match a {
        Alarme::Temperature(t) if t > 80.0 => println!("CRITIQUE : {t}°C"),
        Alarme::Temperature(t) => println!("temp : {t}°C"),
        Alarme::Disque(pct) => println!("disque à {pct}%"),
        Alarme::Reseau(msg) => println!("réseau : {msg}"),
        // aucun `_` nécessaire ici : tous les cas sont couverts
    }
}

fn main() {
    // match comme expression
    let code = 2;
    let txt = match code {
        0 => "ok",
        1 | 2 => "avertissement",      // plusieurs motifs avec |
        3..=5 => "erreur",             // plage inclusive
        _ => "inconnu",                // joker (catch-all)
    };

    // if let : sucre syntaxique pour un seul cas intéressant
    let opt: Option<String> = Some("srv-01".to_string());
    if let Some(nom) = opt {
        println!("hôte : {nom}");
    }

    // while let : boucle tant que le motif correspond
    let mut pile = vec![1, 2, 3];
    while let Some(x) = pile.pop() {
        println!("dépilé : {x}");
    }

    // let-else (édition 2021+, Rust 1.65) : extraction ou sortie anticipée
    let entree: Option<u16> = Some(8080);
    let port = match entree { Some(p) => p, None => return };
    // équivalent :
    // let Some(port) = entree else { return };
    println!("port {port} {txt}");
    traiter(Alarme::Temperature(85.5));
}
```

| Construction | Quand l'utiliser |
|---|---|
| `match` | Tous les cas comptent, ou valeur de retour voulue |
| `if let` | Un seul cas intéressant, les autres ignorés |
| `while let` | Consommer jusqu'à épuisement (`pop`, itérateurs, channels) |
| `let-else` | Extraire ou sortir (`return`, `break`, `continue`) |

---

## 16. `Option<T>` : l'absence de valeur, sans `null`

Rust **n'a pas de `null`**. L'absence est explicite via `Option<T>` : `Some(valeur)` ou `None`.
Impossible d'oublier le cas « pas de valeur » : le compilateur l'exige.

```rust
fn trouver_port(services: &[(&str, u16)], nom: &str) -> Option<u16> {
    services.iter().find(|(n, _)| *n == nom).map(|(_, p)| *p)
}

fn main() {
    let services = [("http", 80), ("https", 443)];

    // Méthodes chaînables (le cœur du style Rust)
    let p = trouver_port(&services, "https")
        .map(|p| p + 1000)          // transforme si Some
        .filter(|p| *p < 2000)      // garde si prédicat vrai
        .unwrap_or(8080);           // valeur par défaut si None
    println!("port : {p}");

    // unwrap_or_else : défaut calculé paresseusement
    let q = trouver_port(&services, "smtp").unwrap_or_else(|| {
        eprintln!("smtp introuvable, défaut 25");
        25
    });

    // ok_or : convertir Option -> Result
    let r: Result<u16, &str> = trouver_port(&services, "dns").ok_or("service inconnu");
    println!("{q} {r:?}");
}
```

Méthodes essentielles de `Option<T>` :

| Méthode | Effet |
|---|---|
| `.unwrap()` | Panic si `None` — **à éviter** hors tests/prototypes |
| `.expect("msg")` | Panic avec message — acceptable si l'invariant est prouvé |
| `.unwrap_or(défaut)` / `.unwrap_or_default()` / `.unwrap_or_else(f)` | Repli sans panic |
| `.map(f)` / `.and_then(f)` | Transformation chaînée |
| `.filter(prédicat)` | `Some` → `None` si faux |
| `.ok_or(err)` / `.ok_or_else(f)` | Vers `Result` |
| `.is_some()` / `.is_none()` | Test |

> Règle d'équipe : `unwrap()` dans du code applicatif = code smell. `expect()` avec un message
> qui explique l'invariant = tolérable. Valeur par défaut ou propagation `?` = idéal.

---

## 17. `Result<T, E>` : les erreurs sont des valeurs (pas d'exceptions)

Pas d'exceptions en Rust. Une fonction qui peut échouer retourne `Result<T, E>` :
`Ok(valeur)` ou `Err(erreur)`. **L'appelant DOIT traiter les deux cas** (sinon warning
`unused_must_use`, et clippy hurle).

```rust
use std::fs;
use std::io;

fn lire_config(chemin: &str) -> Result<String, io::Error> {
    fs::read_to_string(chemin)   // retourne déjà un Result : on le propage tel quel
}

fn main() {
    // Style 1 : match exhaustif
    match lire_config("/etc/mon_outil.conf") {
        Ok(contenu) => println!("config : {} octets", contenu.len()),
        Err(e) => eprintln!("impossible de lire la config : {e}"),
    }

    // Style 2 : propagation avec `?` (dans une fonction qui retourne Result)
    if let Err(e) = demarrer() {
        eprintln!("échec démarrage : {e}");
        std::process::exit(1);
    }
}

// L'opérateur `?` : si Err, retourne immédiatement l'erreur à l'appelant
fn demarrer() -> Result<(), io::Error> {
    let cfg = lire_config("/etc/mon_outil.conf")?;   // <- le `?` magique
    println!("démarrage avec {} octets de config", cfg.len());
    Ok(())
}
```

**Pourquoi pas d'exceptions ?** Parce qu'une exception rend invisible le chemin d'erreur :
tu ne sais pas, en lisant une signature, ce qui peut casser. Avec `Result`, la signature
`fn f() -> Result<T, E>` **documente** l'échec. Le `?` garde le code lisible sans cacher l'erreur.

---

## 18. Traits : le polymorphisme à la Rust

Un **trait** définit un comportement partagé (comme une interface, mais avec super-pouvoirs :
méthodes par défaut, implémentation pour types existants, génériques).

```rust
trait Resume {
    fn resume(&self) -> String;
    // Méthode par défaut : implémentation fournie, surchargeable
    fn resume_court(&self) -> String {
        let r = self.resume();
        r.chars().take(40).collect()
    }
}

struct Onduleur { modele: String, charge_pct: u8 }
struct GroupeElectrogene { modele: String, carburant_l: f64 }

impl Resume for Onduleur {
    fn resume(&self) -> String {
        format!("UPS {} : {}%", self.modele, self.charge_pct)
    }
}
impl Resume for GroupeElectrogene {
    fn resume(&self) -> String {
        format!("GE {} : {} L", self.modele, self.carburant_l)
    }
}

// Fonction générique bornée par le trait
fn afficher<T: Resume>(equipement: &T) {
    println!("{}", equipement.resume());
}

// Syntaxe moderne équivalente (impl Trait)
fn afficher2(equipement: &impl Resume) {
    println!("{}", equipement.resume_court());
}

// Plusieurs bornes : T: Resume + Clone
// where clause pour lisibilité :
fn comparer<T>(a: &T, b: &T) -> bool
where T: Resume + PartialEq {
    a == b && a.resume() == b.resume()
}
```

### Traits dérivables (derive)

```rust
#[derive(Debug, Clone, PartialEq)]
struct Config {
    hote: String,
    port: u16,
}

fn main() {
    let c1 = Config { hote: "srv".into(), port: 443 };
    let c2 = c1.clone();            // Clone explicite (jamais implicite !)
    println!("{c1:?}");             // Debug : {:#?} pour pretty-print
    println!("égaux : {}", c1 == c2);
}
```

| Derive | Rôle |
|---|---|
| `Debug` | Affichage développeur `{:?}` |
| `Clone` | Copie explicite `.clone()` |
| `Copy` | Copie implicite (types simples uniquement) |
| `PartialEq` / `Eq` | `==` / `!=` |
| `PartialOrd` / `Ord` | `<`, tri |
| `Hash` | Clé de `HashMap` |
| `Default` | `Config::default()` |

---

## 19. Génériques : écrire une fois, typer fort

```rust
// Fonction générique : le type est décidé à l'appel, vérifié à la compilation
fn premier<T>(liste: &[T]) -> Option<&T> {
    liste.first()
}

// Struct générique avec contrainte
struct Pile<T> {
    elements: Vec<T>,
}

impl<T> Pile<T> {
    fn new() -> Self { Self { elements: Vec::new() } }
    fn empiler(&mut self, v: T) { self.elements.push(v); }
    fn depiler(&mut self) -> Option<T> { self.elements.pop() }
}

fn main() {
    let nombres = [10, 20, 30];
    println!("{:?}", premier(&nombres));       // Option<&i32>
    let mots = ["a", "b"];
    println!("{:?}", premier(&mots));          // Option<&&str>

    let mut p: Pile<String> = Pile::new();     // monomorphisation : un code par type utilisé
    p.empiler("hello".to_string());
    println!("{:?}", p.depiler());
}
```

> **Monomorphisation** : le compilateur génère une version spécialisée par type concret.
> Zéro coût à l'exécution (pas de boxing, pas de dispatch dynamique), binaire un peu plus gros.
> Pour du dispatch dynamique, voir les *trait objects* (`dyn Trait`, §66).

---

## 20. Collections : Vec, HashMap, String et consorts

```rust
use std::collections::HashMap;

fn main() {
    // ---- Vec<T> : tableau dynamique ----
    let mut v = Vec::new();
    v.push("srv-01");
    v.push("srv-02");
    let mut v2 = vec![80u16, 443, 8080];       // macro vec!
    v2.push(8443);
    println!("2e : {}", v2[1]);                 // indexation (panic si hors bornes)
    println!("sûr : {:?}", v2.get(99));         // get -> Option, jamais de panic

    // Itération sans consommer / en consommant / mutable
    for x in &v2 { println!("lecture {x}"); }
    for x in &mut v2 { *x += 1; }
    let somme: u16 = v2.iter().sum();

    // ---- HashMap<K, V> ----
    let mut latences = HashMap::new();
    latences.insert("srv-01", 12u64);
    latences.insert("srv-02", 45u64);
    // entry : insérer seulement si absent (évite double lookup)
    latences.entry("srv-03").or_insert(99);
    if let Some(l) = latences.get("srv-01") {
        println!("latence srv-01 : {l} ms");
    }
    for (hote, lat) in &latences {
        println!("{hote} -> {lat} ms");
    }

    // ---- String vs &str ----
    let mut s = String::new();
    s.push('a');                 // un char
    s.push_str("bc");            // une tranche
    let s2 = format!("{s}-suffixe");  // format! retourne String
    // let bad = s + "x";        // &str + &str n'existe PAS ; String + &str existe
    let ok = s + "x";            // String + &str -> String (move de s)
    println!("{ok} somme={somme}");
}
```

| Collection | Quand l'utiliser |
|---|---|
| `Vec<T>` | Liste ordonnée, taille dynamique (le défaut) |
| `VecDeque<T>` | File double-ended (push/pop aux deux bouts en O(1)) |
| `HashMap<K,V>` | Association clé→valeur (clés `Hash + Eq`) |
| `BTreeMap<K,V>` | Comme HashMap mais trié par clé (`Ord`) |
| `HashSet<T>` / `BTreeSet<T>` | Ensemble, unicité |
| `String` | Texte possédé, modifiable (UTF-8 garanti) |
| `&str` | Vue texte empruntée |

> ⚠️ `String` est **toujours** de l'UTF-8 valide. L'indexation `s[0]` est interdite
> (un « caractère » = nombre variable d'octets). Utilise `.chars()`, `.bytes()`, ou les slices
> sur frontières UTF-8 (`.get(0..2)` retourne `Option`).

---

## 21. Itérateurs : la puissance fonctionnelle sans surcoût

Les itérateurs Rust sont **paresseux** (lazy) et **zero-cost** : `map`/`filter` ne font rien
tant qu'on ne consomme pas, et le code généré égale une boucle manuelle.

```rust
fn main() {
    let logs = vec![
        "INFO démarrage ok",
        "WARN disque 82%",
        "ERROR connexion refusée",
        "INFO cycle terminé",
        "ERROR timeout 10.0.0.5",
    ];

    // Pipeline : iterator adaptors (paresseux) + consumer (déclenche)
    let erreurs: Vec<&str> = logs
        .iter()                       // -> impl Iterator<Item = &&str>
        .map(|l| l.trim())            // adaptateur : transforme
        .filter(|l| l.starts_with("ERROR"))  // adaptateur : filtre
        .collect();                   // consommateur : exécute et collecte
    println!("{erreurs:?}");

    // Autres consommateurs courants
    let nb_erreurs = logs.iter().filter(|l| l.starts_with("ERROR")).count();
    let total: usize = logs.iter().map(|l| l.len()).sum();
    let max = logs.iter().max_by_key(|l| l.len());

    // Chaînage, zip, enumerate, take/skip
    let pairs: Vec<(usize, &&str)> = logs.iter().enumerate().collect();
    let premiers: Vec<&&str> = logs.iter().take(2).collect();

    // into_iter() consomme le Vec (prend possession des éléments)
    let possedes: Vec<String> = vec!["a".to_string(), "b".to_string()]
        .into_iter()
        .map(|s| s.to_uppercase())
        .collect();
    println!("{nb_erreurs} {total} {max:?} {pairs:?} {premiers:?} {possedes:?}");
}
```

| Adaptateur (paresseux) | Consommateur (exécute) |
|---|---|
| `map`, `filter`, `filter_map` | `collect`, `sum`, `count` |
| `take`, `skip`, `enumerate` | `for_each`, `fold`, `reduce` |
| `zip`, `chain`, `flat_map` | `find`, `any`, `all`, `position` |
| `map_while`, `take_while` | `min/max`, `min_by_key`, `partition` |

> `collect()` a besoin du type cible : `let v: Vec<_> = ...` ou `let s: String = ...`.
> Le `_` laisse l'inférence faire le reste.

---

## 22. Gestion d'erreurs avancée : thiserror et anyhow

Deux crates complémentaires (écosystème standard de facto) :

| Crate | Rôle | Quand |
|---|---|---|
| `thiserror` | **Définir** des types d'erreur propres (bibliothèques) | Tu exposes une API |
| `anyhow` | **Propager** des erreurs avec contexte (applications) | Ton binaire, ton CLI |

```toml
[dependencies]
thiserror = "2"
anyhow = "1"
```

```rust
use thiserror::Error;

// -- Côté bibliothèque : erreur typée, exhaustive --
#[derive(Error, Debug)]
pub enum InventaireErreur {
    #[error("hôte injoignable : {0}")]
    Injoignable(String),
    #[error("fichier {chemin} illisible")]
    FichierIllisible { chemin: String },
    #[error(transparent)]                 // délègue le Display à l'erreur source
    Io(#[from] std::io::Error),           // #[from] génère le From<> pour `?`
}

// -- Côté application : anyhow + contexte --
use anyhow::{Context, Result};

fn charger_inventaire(chemin: &str) -> Result<Vec<String>> {
    let contenu = std::fs::read_to_string(chemin)
        .with_context(|| format!("lecture de l'inventaire {chemin}"))?;
    Ok(contenu.lines().map(|l| l.to_string()).collect())
}

fn main() -> Result<()> {                  // main peut retourner Result !
    let hotes = charger_inventaire("/etc/inventaire.txt")?;
    println!("{} hôtes chargés", hotes.len());
    Ok(())
}
```

Sortie d'erreur avec contexte :

```
Error: lecture de l'inventaire /etc/inventaire.txt

Caused by:
    No such file or directory (os error 2)
```

> `fn main() -> Result<()>` : si `Err`, Rust affiche l'erreur via `Debug` et sort avec code 1.
> Avec `anyhow`, tu obtiens la chaîne de contextes. Pour un CLI soigné, ajoute une crate
> comme `miette` (rapports d'erreur enrichis).

---

## 23. Modules et crates : organiser un projet

```rust
// src/main.rs
mod inventaire;          // charge src/inventaire.rs (ou src/inventaire/mod.rs)
mod reseau;

use inventaire::Hote;    // import dans la portée
use reseau::ping;

fn main() {
    let h = Hote::new("srv-01");
    println!("{} joignable : {}", h.nom, ping("10.0.0.1"));
}
```

```rust
// src/inventaire.rs
pub struct Hote {         // `pub` : visible hors du module
    pub nom: String,      // champ public
    ip: String,           // champ privé au module
}

impl Hote {
    pub fn new(nom: &str) -> Self {
        Self { nom: nom.to_string(), ip: String::new() }
    }
    pub(crate) fn definir_ip(&mut self, ip: &str) {  // visible dans toute la crate
        self.ip = ip.to_string();
    }
}
```

Visibilité, du plus restreint au plus large :

| Mot-clé | Portée |
|---|---|
| (privé, défaut) | Module courant + descendants |
| `pub(crate)` | Toute la crate |
| `pub(super)` | Module parent |
| `pub(in crate::chemin)` | Chemin précis |
| `pub` | Partout (API publique) |

> Une **crate** = une unité de compilation (un projet). Un **package** Cargo peut contenir
> plusieurs crates (binaire + lib). `crates.io` = le registre public (~150 000 crates).

---

## 24. Entrées-sorties et fichiers : le terrain du sysadmin

```rust
use std::fs;
use std::io::{self, BufRead, BufReader, Write};
use std::path::{Path, PathBuf};

fn main() -> io::Result<()> {
    // --- Lecture/écriture "tout d'un coup" (fichiers petits/moyens) ---
    let contenu = fs::read_to_string("/etc/hostname")?;
    fs::write("/tmp/test.txt", "hello")?;

    // --- Lecture ligne à ligne (logs !) : BufReader indispensable ---
    let fichier = fs::File::open("/var/log/syslog")?;
    let lecteur = BufReader::new(fichier);
    let mut nb_warn = 0;
    for ligne in lecteur.lines() {
        let ligne = ligne?;                       // chaque ligne est un io::Result
        if ligne.contains("WARN") { nb_warn += 1; }
    }

    // --- Écriture bufferisée ---
    let mut sortie = fs::File::create("/tmp/rapport.txt")?;
    writeln!(sortie, "avertissements : {nb_warn}")?;

    // --- Parcours de répertoire ---
    for entree in fs::read_dir("/etc")? {
        let entree = entree?;
        let chemin = entree.path();
        if chemin.extension().and_then(|e| e.to_str()) == Some("conf") {
            println!("{}", chemin.display());
        }
    }

    // --- Path / PathBuf : manipuler sans String ---
    let mut p = PathBuf::from("/etc");
    p.push("mon_outil");                          // gère les séparateurs selon l'OS
    p.set_extension("conf");
    println!("{} existe={}", p.display(), Path::new(&p).exists());
    Ok(())
}
```

> `writeln!` vers un fichier retourne un `Result` : ne l'ignore pas (`?` ou `let _ =`).
> Pour des logs volumineux, `BufReader` + `.lines()` évite de charger des Go en RAM.

---

## 25. Arguments CLI, variables d'environnement, codes de sortie

```rust
use std::env;
use std::process::ExitCode;

fn main() -> ExitCode {
    // --- Arguments : env::args() (le 1er = nom du programme) ---
    let args: Vec<String> = env::args().collect();
    // let args: Vec<String> = env::args().skip(1).collect();  // sans le nom

    if args.len() < 2 {
        eprintln!("usage : {} <fichier> [--verbose]", args[0]);
        return ExitCode::from(2);                  // 2 = erreur d'usage (convention)
    }

    // --- Variables d'environnement ---
    let verbose = env::var("VERBOSE").is_ok();     // présence
    let timeout: u64 = env::var("TIMEOUT_MS")      // valeur parsée
        .ok()
        .and_then(|v| v.parse().ok())
        .unwrap_or(1000);
    // env::var("OBLIGATOIRE").expect("...")       // panic si absente

    println!("fichier={} verbose={verbose} timeout={timeout}", args[1]);
    ExitCode::SUCCESS                              // 0
}
```

> Pour un vrai CLI (sous-commandes, `--help` auto, validation), utilise **clap**
> (dérive : `#[derive(Parser)]`). `env::args` suffit pour les outils internes simples.
> Convention Unix : stdout = données, stderr = logs/erreurs, code 0 = succès.

---

## 26. Réseau : TCP/UDP avec la std

La std couvre TCP/UDP bloquant — suffisant pour sondes, petits serveurs, outils.

```rust
use std::io::{Read, Write};
use std::net::{TcpListener, TcpStream, UdpSocket};
use std::time::Duration;

fn mini_serveur() -> std::io::Result<()> {
    let listener = TcpListener::bind("0.0.0.0:7878")?;
    println!("écoute sur 7878");
    for flux in listener.incoming() {          // itérateur de connexions
        let mut stream = flux?;               // TcpStream
        let mut buf = [0u8; 1024];
        let n = stream.read(&mut buf)?;
        stream.write_all(b"HTTP/1.1 200 OK\r\n\r\npong")?;
        println!("requête : {} octets", n);
    }
    Ok(())
}

fn sonde_tcp(hote: &str, port: u16, timeout_ms: u64) -> bool {
    let addr = format!("{hote}:{port}");
    let timeout = Duration::from_millis(timeout_ms);
    match addr.parse::<std::net::SocketAddr>() {
        Ok(sa) => TcpStream::connect_timeout(&sa, timeout).is_ok(),
        Err(_) => false,
    }
}

fn envoi_udp() -> std::io::Result<()> {
    let sock = UdpSocket::bind("0.0.0.0:0")?;     // port éphémère
    sock.send_to(b"metrique=1", "10.0.0.9:8125")?; // ex : DogStatsD
    Ok(())
}

fn main() {
    println!("sonde : {}", sonde_tcp("10.0.0.1", 443, 500));
    let _ = envoi_udp();
    // mini_serveur().unwrap();
}
```

> Pour du HTTP, du TLS, de l'async ou des milliers de connexions : crates `reqwest`,
> `tokio`, `rustls` (§27, §45). La std reste parfaite pour sondes et prototypes.

---

## 27. HTTP client/serveur : l'écosystème (reqwest, axum)

```toml
[dependencies]
reqwest = { version = "0.12", features = ["json", "rustls-tls"] }
tokio = { version = "1", features = ["full"] }
serde = { version = "1", features = ["derive"] }
axum = "0.7"
```

```rust
use serde::{Deserialize, Serialize};

#[derive(Debug, Deserialize)]
struct Sante { statut: String }

#[derive(Serialize)]
struct Rapport { hote: String, ok: bool }

// Client HTTP (bloquant possible avec feature "blocking", async par défaut)
async fn verifier(url: &str) -> Result<bool, reqwest::Error> {
    let client = reqwest::Client::builder()
        .timeout(std::time::Duration::from_secs(5))
        .build()?;
    let rep: Sante = client.get(url).send().await?.json().await?;
    Ok(rep.statut == "ok")
}

// Serveur HTTP minimal avec axum
async fn lancer_serveur() {
    use axum::{routing::get, Json, Router};
    async fn sante() -> Json<Rapport> {
        Json(Rapport { hote: hostname::get().unwrap().to_string_lossy().into(), ok: true })
    }
    let app = Router::new().route("/sante", get(sante));
    let listener = tokio::net::TcpListener::bind("0.0.0.0:3000").await.unwrap();
    axum::serve(listener, app).await.unwrap();
}
```

> `reqwest` avec `rustls-tls` : TLS en pur Rust, pas de dépendance OpenSSL système.
> Binaire plus portable (musl !). `axum` = framework web moderne bâti sur `tokio`/`hyper`.

---

## 28. Tests unitaires : intégrés au langage

```rust
// Fonctions à tester
fn addition(a: i32, b: i32) -> i32 { a + b }
fn diviser(a: f64, b: f64) -> Option<f64> {
    if b == 0.0 { None } else { Some(a / b) }
}

// Les tests vivent dans le même fichier, module cfg(test)
#[cfg(test)]
mod tests {
    use super::*;                          // importe le code testé

    #[test]
    fn addition_basique() {
        assert_eq!(addition(2, 3), 5);
    }

    #[test]
    fn division_par_zero() {
        assert_eq!(diviser(1.0, 0.0), None);
    }

    #[test]
    #[should_panic(expected = "dépassement")]
    fn panique_attendue() {
        panic!("dépassement volontaire");
    }

    #[test]
    fn avec_resultat() -> Result<(), String> {   // test qui retourne Result : élégant
        if addition(2, 2) == 4 { Ok(()) } else { Err("mauvais calcul".into()) }
    }
}
```

```bash
cargo test                  # tous les tests
cargo test addition         # filtre par nom
cargo test -- --nocapture   # affiche les println!
cargo test --release        # en mode optimisé
```

Macros d'assertion :

| Macro | Usage |
|---|---|
| `assert!(cond)` | Condition booléenne |
| `assert_eq!(a, b)` / `assert_ne!` | Égalité (nécessite `PartialEq + Debug`) |
| `panic!("msg")` / `unreachable!()` / `unimplemented!()` | Échecs explicites |

---

## 29. Tests d'intégration et tests de documentation

```
mon_outil/
├── src/
│   └── lib.rs          # la lib (fonctions publiques)
├── src/main.rs         # le binaire (utilise la lib)
└── tests/
    └── integration.rs  # tests d'intégration : crate externe
```

```rust
// tests/integration.rs — compile comme une crate séparée
use mon_outil::addition;   // n'a accès qu'à l'API PUBLIQUE

#[test]
fn via_api_publique() {
    assert_eq!(addition(40, 2), 42);
}
```

```rust
// Test de documentation : le code dans /// est COMPILÉ et EXÉCUTÉ
/// Additionne deux entiers.
///
/// ```
/// assert_eq!(mon_outil::addition(2, 3), 5);
/// ```
pub fn addition(a: i32, b: i32) -> i32 { a + b }
```

> Les doctests garantissent que **tes exemples de doc restent vrais**. `cargo test` les lance.
> Stratégie : unitaires pour la logique interne, intégration pour le comportement public,
> doctests pour les exemples.

---

## 30. Debug : eprintln!, dbg!, gdb/lldb

```rust
fn main() {
    let config = vec!["a", "b", "c"];

    // dbg! : affiche fichier:ligne + valeur, REND la valeur (pratique en pipeline)
    let n = dbg!(config.len());            // [src/main.rs:4] config.len() = 3

    // eprintln! : vers stderr (ne pollue pas stdout = les données)
    eprintln!("DEBUG : {} éléments", config.len());

    // pretty-print
    eprintln!("{config:#?}");

    // traçage conditionnel via variable d'env (sans crate)
    if std::env::var("DEBUG").is_ok() {
        eprintln!("[debug] étape 1 ok");
    }
}
```

Debug natif :

```bash
# Symboles déjà inclus en debug. Lancer sous gdb :
rust-gdb ./target/debug/mon_outil
# ou lldb :
rust-lldb ./target/debug/mon_outil
```

> Pour du vrai logging structuré : crates `log` + `env_logger` (façade + implémentation,
> niveau via `RUST_LOG=debug`), ou `tracing` (async-aware, standard moderne).
> `dbg!` = provisoire ; ne jamais le laisser en prod (il écrit sur stderr).

---

## 31. Clippy et rustfmt : qualité automatique

```bash
cargo fmt --check        # vérifie le formatage (CI)
cargo fmt                # reformate tout
cargo clippy             # ~700 lints : maladresses, perfs, style
cargo clippy -- -D warnings   # les warnings deviennent des erreurs (CI stricte)
```

Exemples de ce que clippy détecte :

```rust
fn avant(hotes: &Vec<String>) -> usize {   // clippy: &Vec -> &[String] (ptr_arg)
    let mut total = 0;
    for h in hotes {                        // clippy ok ici, mais...
        total += h.len();
    }
    return total;                           // clippy: needless_return
}

fn apres(hotes: &[String]) -> usize {
    hotes.iter().map(|h| h.len()).sum()     // clippy: suggère l'itérateur
}
```

Fichier `clippy.toml` / attributs :

```rust
#![warn(clippy::pedantic)]        // en tête de crate : niveau exigeant
#[allow(clippy::too_many_arguments)]  // exception locale justifiée
fn f(a: u8, b: u8, c: u8, d: u8, e: u8, f2: u8, g: u8, h: u8) -> u8 {
    a + b + c + d + e + f2 + g + h
}
```

> Règle d'équipe : `cargo fmt --check` + `cargo clippy -- -D warnings` dans la CI.
> Zéro warning toléré sur le code neuf.

---

## 32. Bonnes pratiques & style : le contrat d'équipe

1. **`cargo fmt` systématique** avant chaque commit (hook pre-commit ou CI).
2. **Zéro `unwrap()`** dans les chemins applicatifs ; `expect("invariant: ...")` si prouvé.
3. **Nomme explicitement** : `hote`, `latence_ms`, pas `x`, `tmp` (hors boucles triviales).
4. **Petites fonctions** : une fonction = une responsabilité ; < 50 lignes en général.
5. **Erreurs typées** (`thiserror`) dans les libs, `anyhow` + contexte dans les binaires.
6. **Pas de `panic!` pour une erreur récupérable** (fichier manquant, réseau coupé).
7. **Documentation** : `///` sur toute fonction publique, avec exemple (doctest).
8. **`&str` en paramètre, `String` en retour** quand c'est possible (flexibilité d'appel).
9. **`&[T]` plutôt que `&Vec<T>`** en paramètre (accepte slices et Vec).
10. **Évite les clones défensifs** : préfère les emprunts ; clone quand la sémantique l'exige.
11. **Tests** : chaque correction de bug = un test de non-régression.
12. **Édition** : choisis 2021 ou 2024 et reste cohérent dans le workspace.

---

## 33. Performance : mesurer avant d'optimiser

```bash
cargo build --release              # toujours mesurer en release !
```

```rust
use std::time::Instant;

fn main() {
    // Mesure artisanale
    let debut = Instant::now();
    let v: Vec<u64> = (0..10_000_000).map(|x| x * 2).collect();
    println!("collect : {:?} ({} éléments)", debut.elapsed(), v.len());

    // Comparer deux approches : écrire un bench...
}
```

Benchmarks sérieux :

```toml
[dev-dependencies]
criterion = "0.5"

[[bench]]
name = "mon_bench"
harness = false
```

```rust
// benches/mon_bench.rs
use criterion::{black_box, criterion_group, criterion_main, Criterion};

fn bench_filtrage(c: &mut Criterion) {
    let donnees: Vec<u64> = (0..100_000).collect();
    c.bench_function("filtre pair", |b| {
        b.iter(|| donnees.iter().filter(|x| *x % 2 == 0).count())
    });
    // black_box empêche le compilateur d'optimiser le calcul hors de la mesure
    c.bench_function("avec black_box", |b| {
        b.iter(|| black_box(&donnees).iter().filter(|x| *x % 2 == 0).count())
    });
}
criterion_group!(benches, bench_filtrage);
criterion_main!(benches);
```

Leviers classiques :

| Levier | Effet typique |
|---|---|
| `cargo build --release` (opt-level=3) | Base obligatoire |
| Éviter les allocations en boucle (`String::with_capacity`) | Moins de pression sur l'allocateur |
| Itérateurs plutôt que boucles indexées | Même perf, moins d'erreurs |
| `lto = true`, `codegen-units = 1` | Binaire plus rapide, compilation plus lente |
| `jemalloc` / `mimalloc` (crates) | Allocateur plus rapide selon workload |
| Éviter `Mutex` en section chaude | Contention = poison de la perf |

> Règle : profile d'abord (`perf`, `flamegraph` via `cargo install flamegraph`), optimise ensuite.
> 90 % du temps passé dans 10 % du code.

---

## 34. Sécurité mémoire : ce que le compilateur garantit (et pas)

**Garanti à la compilation** (code safe) :

- Pas de déréférencement de pointeur nul (pas de `null`).
- Pas de *use-after-free* (ownership).
- Pas de double libération (move).
- Pas de data race (règles d'emprunt + `Send`/`Sync`).
- Pas de dépassement de buffer non vérifié (indexation contrôlée, slices avec longueur).

**NON garanti** (à toi de gérer) :

| Risque | Mitigation |
|---|---|
| Panic par `unwrap` / index hors bornes | `get()`, `?`, valeurs par défaut |
| Dépassement arithmétique silencieux en release | `checked_*` / `saturating_*` |
| Fuite mémoire (`mem::forget`, cycles `Rc`) | Rare ; `Weak` pour casser les cycles |
| Logique métier fausse | Tests, types forts (newtype) |
| Bloc `unsafe` | Audit obligatoire (voir §35) |
| Dépendances vulnérables | `cargo audit`, `cargo deny` en CI |

> « Fearless concurrency » n'est pas un slogan : si ça compile, les data races sont exclus
> (hors `unsafe`). C'est LA raison d'écrire un agent multi-thread en Rust plutôt qu'en C.

---

## 35. `unsafe` : quand et comment (avec parcimonie)

`unsafe` ne désactive pas le borrow checker : il autorise 5 opérations normalement interdites :

1. Déréférencer un pointeur brut (`*const T`, `*mut T`).
2. Appeler une fonction `unsafe` (dont les FFI C).
3. Accéder/muter une variable `static mut` (déconseillé ; préférer `Atomic*`/`Mutex`).
4. Implémenter un trait `unsafe` (`Send`, `Sync` manuels).
5. Accéder aux champs d'une `union`.

```rust
// Exemple canonique : FFI vers libc (voir §46)
extern "C" {
    fn getpid() -> i32;
}

fn main() {
    let pid = unsafe { getpid() };   // l'appel FFI est unsafe : l'appelant assume le contrat C
    println!("pid = {pid}");
}
```

Contrat d'équipe pour `unsafe` :

- [ ] Encapsuler dans une **abstraction sûre** : fonction safe qui contient un petit bloc unsafe.
- [ ] Documenter l'invariant avec `// SAFETY: ...` (convention officielle).
- [ ] Justifier pourquoi le safe ne suffit pas.
- [ ] `cargo geiger` / audit en revue de code.
- [ ] Miri (`cargo +nightly miri test`) pour traquer les UB dans les tests.

```rust
/// Retourne le pid du processus.
///
/// # Safety
/// Aucune : fonction 100 % sûre, l'unsafe est encapsulé.
fn mon_pid() -> i32 {
    // SAFETY: getpid ne peut pas échouer et ne touche pas de mémoire partagée.
    unsafe { getpid() }
}
```

---

## 36. Erreurs classiques des débutants (10+, avec corrections)

### Erreur 1 — `cannot borrow as mutable` : deux emprunts incompatibles

```rust
// ❌ NE COMPILE PAS
// let mut v = vec![1, 2, 3];
// let premier = &v[0];
// v.push(4);                    // push peut réallouer -> invaliderait `premier`
// println!("{premier}");

// ✅ Copier la valeur (types Copy) ou terminer l'emprunt d'abord
let mut v = vec![1, 2, 3];
let premier = v[0];              // i32 est Copy : pas d'emprunt
v.push(4);
println!("{premier}");
```

### Erreur 2 — `borrow of moved value`

```rust
// ❌ let s = String::from("x"); let t = s; println!("{s}");
// ✅
let s = String::from("x");
let t = s.clone();               // copie explicite si on veut garder s
println!("{s} {t}");
// ✅ ou : passer par référence
fn affiche(s: &str) { println!("{s}"); }
let s2 = String::from("y");
affiche(&s2);
println!("{s2}");                // toujours valide
```

### Erreur 3 — Oublier que `if`/`match` sans `;` retourne une valeur

```rust
fn statut(charge: u8) -> &'static str {
    if charge > 90 {
        "critique";   // ❌ le `;` transforme en statement -> la fonction retourne ()
    } else {
        "ok";         // ❌ idem
    }
}
// ✅ retirer les points-virgules finaux
fn statut_ok(charge: u8) -> &'static str {
    if charge > 90 { "critique" } else { "ok" }
}
```

### Erreur 4 — Comparer `String` et `&str` avec `==`… ça marche, mais l'addition non

```rust
let a = "hello".to_string();
// let b = a + " world";         // ❌ : String + &str existe, mais &str + &str NON
let b = a + " world";            // ✅ en fait ça compile : String + &str -> String
let c = "hello";
// let d = c + " world";         // ❌ vraiment interdit : &str + &str n'existe pas
let d = format!("{c} world");    // ✅ format! pour concaténer des &str
println!("{b} {d}");
```

### Erreur 5 — `Vec` déplacé par `for x in v`

```rust
let v = vec![1, 2, 3];
// for x in v { }                 // ❌ into_iter : v est déplacé (sauf si Copy... non, Vec ne l'est pas)
// println!("{:?}", v);           // ❌ v n'existe plus
// ✅
let v = vec![1, 2, 3];
for x in &v { println!("{x}"); } // emprunt : v survit
println!("{:?}", v);
```

### Erreur 6 — Lifetime manquante sur struct avec référence

```rust
// ❌ struct Config { chemin: &str }   // missing lifetime specifier
// ✅
struct Config<'a> { chemin: &'a str }
// Mieux : posséder plutôt qu'emprunter quand c'est possible
struct ConfigPossedee { chemin: String }   // zéro annotation, zéro souci
```

### Erreur 7 — `unwrap()` sur `Result` dans `main` sans réfléchir

```rust
// ❌ std::fs::read_to_string("/etc/fichier").unwrap();  // panic si absent
// ✅
fn main() -> anyhow::Result<()> {
    let c = std::fs::read_to_string("/etc/fichier")?;     // erreur propre + code 1
    println!("{c}");
    Ok(())
}
```

### Erreur 8 — Muter via une référence immuable

```rust
// ❌ let v = vec![1]; v.push(2);   // cannot borrow as mutable
// ✅
let mut v = vec![1];
v.push(2);
```

### Erreur 9 — `&String` vs `&str` : accepter trop étroit

```rust
// ❌ fn f(s: &String)  — refuse les littéraux &str
// ✅
fn f(s: &str) { println!("{s}"); }   // accepte &String (deref coercion) ET &str
```

### Erreur 10 — Ignorer le `Result` retourné (`unused_must_use`)

```rust
use std::io::Write;
// ❌ std::io::stdout().write_all(b"x");   // warning : unused Result
// ✅
let _ = std::io::stdout().write_all(b"x"); // assumé explicitement
// ✅ ou : std::io::stdout().write_all(b"x")?;
```

### Erreur 11 — Pattern `match` non exhaustif

```rust
enum S { A, B, C }
// ❌ match s { S::A => {}, S::B => {} }   // non-exhaustive patterns: `C` not covered
// ✅ ajouter S::C => {} ou _ => {}
```

### Erreur 12 — Shadowing involontaire qui masque un bug

```rust
let mut total = 0;
let total = total + 5;   // ⚠️ shadowing : le `mut` initial ne sert plus ; souvent un oubli
println!("{total}");
```

---

## 37. Cas pratique 1 — Analyseur de logs (commenté)

Objectif : compter les niveaux de log d'un fichier, avec gestion d'erreurs propre.

```rust
use std::collections::HashMap;
use std::env;
use std::fs::File;
use std::io::{BufRead, BufReader};
use std::process::ExitCode;

/// Extrait le niveau (INFO/WARN/ERROR/...) en tête de ligne.
/// Retourne None si la ligne ne commence pas par un niveau connu.
fn niveau(ligne: &str) -> Option<&str> {
    let premier = ligne.split_whitespace().next()?;
    match premier {
        "INFO" | "WARN" | "ERROR" | "DEBUG" => Some(premier),
        _ => None,
    }
}

fn analyser(chemin: &str) -> Result<HashMap<String, u64>, Box<dyn std::error::Error>> {
    let fichier = File::open(chemin)?;                    // ? propage io::Error
    let mut compteurs: HashMap<String, u64> = HashMap::new();
    for ligne in BufReader::new(fichier).lines() {
        let ligne = ligne?;
        if let Some(n) = niveau(&ligne) {
            *compteurs.entry(n.to_string()).or_insert(0) += 1;  // entry : idiome canonique
        }
    }
    Ok(compteurs)
}

fn main() -> ExitCode {
    let chemin = env::args().nth(1).unwrap_or_else(|| "/var/log/syslog".to_string());
    match analyser(&chemin) {
        Ok(c) => {
            let mut cles: Vec<_> = c.keys().collect();
            cles.sort();                                  // affichage déterministe
            for k in cles {
                println!("{k}: {}", c[k]);
            }
            ExitCode::SUCCESS
        }
        Err(e) => {
            eprintln!("erreur : {e}");
            ExitCode::FAILURE
        }
    }
}
```

Points à noter :

- `Box<dyn Error>` : type d'erreur générique pour un petit outil (anyhow ferait pareil, en mieux).
- `entry().or_insert()` : l'idiome compteur, à connaître par cœur.
- Tri des clés : sortie déterministe = diffable, scriptable.

---

## 38. Cas pratique 2 — Sonde réseau parallèle (threads + channels)

Objectif : pinger N hôtes en parallèle, agréger les résultats via un channel.

```rust
use std::net::TcpStream;
use std::sync::mpsc;
use std::thread;
use std::time::Duration;

#[derive(Debug)]
struct ResultatSonde {
    hote: String,
    joignable: bool,
    latence_ms: u128,
}

fn sonder(hote: &str, port: u16) -> ResultatSonde {
    let debut = std::time::Instant::now();
    let joignable = TcpStream::connect_timeout(
        &format!("{hote}:{port}").parse().unwrap(),
        Duration::from_millis(800),
    )
    .is_ok();
    ResultatSonde { hote: hote.to_string(), joignable, latence_ms: debut.elapsed().as_millis() }
}

fn main() {
    let hotes = vec!["10.0.0.1", "10.0.0.2", "10.0.0.3", "10.0.0.4"];
    let (tx, rx) = mpsc::channel();       // multiple producers, single consumer

    for hote in hotes {
        let tx = tx.clone();              // un émetteur par thread
        let hote = hote.to_string();      // posséder les données envoyées au thread
        thread::spawn(move || {           // `move` : le thread possède son environnement
            let r = sonder(&hote, 443);
            let _ = tx.send(r);           // envoi (Result ignoré si rx fermé)
        });
    }
    drop(tx);                             // ferme l'émetteur d'origine : rx se termine

    for r in rx {                         // itère jusqu'à fermeture du channel
        let statut = if r.joignable { "OK  " } else { "KO  " };
        println!("{statut} {:12} {} ms", r.hote, r.latence_ms);
    }
}
```

> `move` sur la closure : obligatoire, car le thread peut survivre à la fonction.
> Le borrow checker garantit qu'aucune donnée ne meurt pendant que le thread l'utilise.

---

## 39. Cas pratique 3 — Mini inventaire JSON (serde)

```toml
[dependencies]
serde = { version = "1", features = ["derive"] }
serde_json = "1"
anyhow = "1"
```

```rust
use anyhow::{Context, Result};
use serde::{Deserialize, Serialize};
use std::fs;

#[derive(Debug, Serialize, Deserialize)]
struct Equipement {
    hostname: String,
    ip: String,
    #[serde(default = "port_defaut")]      // valeur par défaut si champ absent
    port: u16,
    #[serde(default)]
    tags: Vec<String>,                     // défaut = Vec::new()
}

fn port_defaut() -> u16 { 22 }

#[derive(Debug, Serialize, Deserialize)]
struct Inventaire {
    version: u8,
    equipements: Vec<Equipement>,
}

fn charger(chemin: &str) -> Result<Inventaire> {
    let brut = fs::read_to_string(chemin)
        .with_context(|| format!("lecture {chemin}"))?;
    let inv: Inventaire = serde_json::from_str(&brut)
        .with_context(|| format!("parse JSON {chemin}"))?;
    Ok(inv)
}

fn main() -> Result<()> {
    // Exemple de JSON accepté :
    // {"version":1,"equipements":[{"hostname":"sw-01","ip":"10.0.1.2","tags":["core"]}]}
    let inv = charger("inventaire.json")?;
    println!("{} équipements (v{})", inv.equipements.len(), inv.version);
    let json = serde_json::to_string_pretty(&inv)?;   // sérialisation inverse
    fs::write("inventaire.pretty.json", json)?;
    Ok(())
}
```

> `serde` = LE pilier de l'écosystème (JSON, YAML, TOML, CSV…). `#[serde(default)]`,
> `rename`, `skip_serializing_if` : à connaître pour des formats de config tolérants.

---

## 40. Pense-bête de poche (une page)

```
--- Ownership ---
let s = String::from("x");   // s possède
let t = s;                   // MOVE : s invalide
let u = t.clone();           // copie profonde explicite
fn f(s: &str) {}             // emprunt : le prêteur garde la propriété

--- Emprunts ---
&r      immuable, N autorisés
&mut r  mutable, UN seul, pas de & en même temps

--- Erreurs ---
fn f() -> Result<T, E>       // pas d'exceptions
val?                         // propage Err, ou unwrap le Ok
opt.ok_or("msg")?            // Option -> Result

--- Match ---
match v { P1 => .., P2(x) => .., _ => .. }   // exhaustif !
if let Some(x) = opt { .. }
let Some(x) = opt else { return; };

--- Collections ---
vec![1,2,3]   v.push(x)   v.get(i) -> Option
map.entry(k).or_insert(v)    // idiome compteur/insertion
s.chars() / s.bytes()        // jamais s[i]

--- Itérateurs ---
.iter() .iter_mut() .into_iter()
.map .filter .collect::<Vec<_>>() .sum() .count()

--- Cargo ---
cargo check   (rapide)      cargo clippy -- -D warnings
cargo fmt     (toujours)    cargo test -- --nocapture

--- Conventions ---
&str en param, String en retour | &[T] pas &Vec<T>
unwrap() interdit en prod     | eprintln! pour les logs
```

---

## 41. Glossaire

| Terme | Définition |
|---|---|
| Ownership | Système de possession unique : chaque valeur a un propriétaire qui la libère |
| Borrow checker | Analyse statique du compilateur qui valide emprunts et durées de vie |
| Move | Transfert de propriété ; l'ancien propriétaire devient invalide |
| Borrow (emprunt) | Prêt temporaire via `&T` / `&mut T`, sans transfert de propriété |
| Lifetime (`'a`) | Durée de vie d'une référence, souvent élidée |
| NLL | Non-Lexical Lifetimes : l'emprunt meurt à son dernier usage |
| Trait | Contrat de comportement partagé (interface + méthodes par défaut) |
| Générique | Code paramétré par type, monomorphisé à la compilation |
| Crate | Unité de compilation ; `crates.io` = registre public |
| Cargo | Gestionnaire de build, dépendances, tests, doc |
| rustup | Gestionnaire de toolchains Rust |
| `Result<T,E>` | `Ok(T)` ou `Err(E)` : erreur = valeur, pas d'exception |
| `Option<T>` | `Some(T)` ou `None` : remplace `null` |
| Panic | Arrêt brutal sur bug irrécupérable (débordement, unwrap sur None) |
| `unsafe` | Bloc autorisant 5 opérations sensibles, sous contrat du programmeur |
| RAII | La libération suit la portée (drop automatique) |
| Zero-cost abstraction | Abstraction sans surcoût à l'exécution (itérateurs, génériques) |
| Monomorphisation | Génération d'une version par type concret pour les génériques |
| FFI | Foreign Function Interface : appeler du C (ou être appelé) |
| Édition | Version du langage (2015/2018/2021/2024) : évolutions sans casser l'existant |
| Shadowing | Redéclaration `let` qui masque la précédente (type modifiable) |
| Deref coercion | Conversion auto `&String`→`&str`, `&Vec<T>`→`&[T]` |
| Send / Sync | Traits marqueurs : transférable entre threads / partageable entre threads |
| Tokio | Runtime async de référence |
| Miri | Interpréteur (nightly) détectant les comportements indéfinis |

---

## 42. Quiz : 10 questions (réponses en §43)

1. Que se passe-t-il quand le propriétaire d'une `String` sort de portée ?
2. Peut-on avoir deux `&mut` sur la même valeur en même temps ? Pourquoi ?
3. `let x = vec![1,2]; let y = x;` — que vaut `x` après ? Et si c'était un `i32` ?
4. À quoi sert l'opérateur `?` et dans quel type de fonction peut-on l'utiliser ?
5. Pourquoi Rust n'a-t-il pas d'exceptions ? Quel type les remplace ?
6. `match` doit être exhaustif : qu'est-ce que ça garantit concrètement ?
7. Différence entre `&String` et `&str` en paramètre de fonction : lequel préférer ?
8. Que signifie `#[derive(Debug, Clone)]` au-dessus d'un struct ?
9. `String` est-elle `Copy` ? Pourquoi (où sont ses données) ?
10. À quoi sert `cargo check` par rapport à `cargo build` ?

---

## 43. Quiz : réponses

1. La valeur est **droppée** : `Drop` libère la mémoire du tas automatiquement (RAII).
   Aucun `free` manuel, aucun GC.
2. **Non.** Règle d'emprunt : soit N `&T`, soit un seul `&mut T`. Ça élimine les data races
   à la compilation.
3. `x` est **déplacé** (move) : invalide. Avec `i32` (type `Copy`), `x` reste valide
   (copie bit à bit).
4. `?` **propage l'erreur** : sur `Err(e)`/`None`, retourne immédiatement `e` à l'appelant
   (avec conversion `From`). Utilisable dans toute fonction retournant `Result` ou `Option`.
5. Les exceptions rendent les chemins d'erreur **invisibles** dans les signatures.
   `Result<T, E>` rend l'échec **explicite et obligatoire à traiter**.
6. Le compilateur **refuse d'oublier un cas** (ajout d'une variante d'enum = erreur de
   compilation aux endroits à mettre à jour, pas bug silencieux).
7. Préférer **`&str`** : accepte `&String` (deref coercion) ET les littéraux, donc plus général.
8. Génère automatiquement les implémentations `Debug` (affichage `{:?}`) et `Clone` (`.clone()`).
9. **Non** : ses octets sont sur le tas (pointeur + len + cap sur la pile). La copie serait
   un double pointeur → double free. D'où le move.
10. `cargo check` **vérifie** (types, emprunts) sans générer de code machine : bien plus
    rapide en itération. `cargo build` produit le binaire.

---

## 44. Pour aller plus loin

| Ressource | Type | Note |
|---|---|---|
| *The Rust Programming Language* (le « Book ») | Livre officiel gratuit | La référence, en ligne |
| *Rust by Example* | Tutoriels | Apprendre par l'exemple |
| docs.rs | Documentation | Doc de toutes les crates |
| Rustlings | Exercices CLI | `cargo install rustlings` : petits exercices corrigés |
| *The Rustonomicon* | Livre | L'unsafe en profondeur |
| *Asynchronous Programming in Rust* | Livre officiel | L'async/await |
| This Week in Rust | Newsletter | Suivi de l'écosystème |
| crates.io | Registre | Chercher des crates (trier par téléchargements) |
| users.rust-lang.org | Forum | Questions/réponses |
| `cargo audit`, `cargo deny` | Outils | Sécurité des dépendances en CI |

Prochaines étapes conseillées pour un sysadmin :

1. Réécrire un script Python d'inventaire en Rust (serde + clap).
2. Écrire un exporter Prometheus minimal (axum).
3. Packager en binaire statique musl + image `scratch` Docker.
4. Ajouter `cargo audit` à ta CI.

---

## 45. Concurrence : threads, Arc, Mutex, channels

```rust
use std::sync::{mpsc, Arc, Mutex};
use std::thread;
use std::time::Duration;

fn main() {
    // --- thread::spawn : threads OS, `move` obligatoire ---
    let poignee = thread::spawn(|| {
        thread::sleep(Duration::from_millis(100));
        42
    });
    println!("résultat thread : {}", poignee.join().unwrap());  // join -> Result

    // --- Arc<Mutex<T>> : état partagé entre threads ---
    // Arc = Atomic Reference Counted (compteur thread-safe) ; Mutex = exclusion mutuelle
    let compteur = Arc::new(Mutex::new(0u64));
    let mut poignees = vec![];
    for _ in 0..10 {
        let c = Arc::clone(&compteur);       // clone le POINTEUR, pas la donnée
        poignees.push(thread::spawn(move || {
            let mut n = c.lock().unwrap();   // MutexGuard : verrou jusqu'à fin de portée
            *n += 1;
        }));
    }
    for p in poignees { p.join().unwrap(); }
    println!("compteur = {}", *compteur.lock().unwrap());   // 10

    // --- Channels : "ne communique pas en partageant, partage en communiquant" ---
    let (tx, rx) = mpsc::channel();
    thread::spawn(move || {
        for i in 0..3 { tx.send(format!("msg-{i}")).unwrap(); }
    });
    for recu in rx {                       // se termine quand tous les tx sont droppés
        println!("reçu : {recu}");
    }
}
```

| Primitive | Rôle |
|---|---|
| `thread::spawn` | Thread OS, closure `move`, `JoinHandle::join()` |
| `mpsc::channel()` | Multi-producteur, mono-consommateur |
| `Arc<T>` | Propriété partagée thread-safe (compteur atomique) |
| `Mutex<T>` / `RwLock<T>` | Exclusion mutuelle / lecteurs-multiples |
| `AtomicU64`… | Compteurs sans verrou (`Ordering::SeqCst` par défaut sûr) |
| `thread::scope` (Rust 1.63+) | Threads scopés : emprunter la pile parente sans `Arc` ! |

```rust
// Scoped threads : plus besoin d'Arc pour emprunter des données locales
fn main2() {
    let donnees = vec![1, 2, 3, 4];
    let mut somme = 0;
    thread::scope(|s| {
        let (g, d) = donnees.split_at(2);
        let h1 = s.spawn(|| g.iter().sum::<i32>());
        let h2 = s.spawn(|| d.iter().sum::<i32>());
        somme = h1.join().unwrap() + h2.join().unwrap();
    });   // tous les threads sont joints ici, garanti
    println!("somme = {somme}");
}
```

> **Send / Sync** : `Send` = peut traverser les threads (presque tout) ; `Sync` = `&T`
> partageable (`Mutex<T>` est `Sync` si `T: Send`). `Rc<T>` et `Cell<T>` ne sont PAS `Send` :
> le compilateur refuse de les envoyer dans un thread. C'est la garantie anti-data-race.

---

## 46. Async / Tokio : introduction

L'async Rust = concurrence **coopérative** sur peu de threads (idéal I/O : milliers de
connexions, sondes réseau, scrapers). `async fn` ne fait rien tant qu'on ne l'**exécute pas**
dans un runtime (`tokio` = standard de facto).

```toml
[dependencies]
tokio = { version = "1", features = ["full"] }
```

```rust
use std::time::Duration;

// async fn = fonction qui retourne une Future (paresseuse !)
async fn sonder_async(hote: &str) -> bool {
    // Ici on simule ; en vrai : tokio::net::TcpStream::connect(...).await
    tokio::time::sleep(Duration::from_millis(50)).await;  // .await = point de suspension
    !hote.is_empty()
}

#[tokio::main]                       // macro : construit le runtime et bloque dessus
async fn main() {
    // Exécution séquentielle
    println!("a : {}", sonder_async("10.0.0.1").await);

    // Exécution CONCURRENTE : join! attend les deux
    let (r1, r2) = tokio::join!(
        sonder_async("10.0.0.1"),
        sonder_async("10.0.0.2"),
    );
    println!("{r1} {r2}");

    // spawn : tâche détachée (comme un thread, mais légère)
    let tache = tokio::spawn(sonder_async("10.0.0.3"));
    println!("spawn : {}", tache.await.unwrap());

    // Timeout : combinateur indispensable en supervision
    let res = tokio::time::timeout(Duration::from_millis(10), sonder_async("x")).await;
    println!("timeout écoulé : {}", res.is_err());
}
```

| Concept | À retenir |
|---|---|
| `Future` | Calcul paresseux, pollé par le runtime |
| `.await` | Suspend la tâche SANS bloquer le thread |
| `tokio::spawn` | Tâche `'static` (posséder ses données, comme les threads) |
| `tokio::join!` | Attendre plusieurs futures en parallèle |
| `tokio::select!` | Premier arrivé gagne (course, timeout, annulation) |
| `Send` across `.await` | Les données conservées entre deux `.await` doivent être `Send` |

> ⚠️ **Ne jamais bloquer** (`std::thread::sleep`, I/O bloquante) dans du code async :
> ça fige tout le runtime. Utiliser `tokio::time::sleep`, `tokio::fs`, `tokio::task::spawn_blocking`
> pour le code bloquant legacy.

---

## 47. FFI et binaires système : parler au C et à l'OS

```rust
use std::ffi::{CStr, CString};
use std::os::raw::c_char;

// Déclarer une fonction C existante
extern "C" {
    fn strlen(s: *const c_char) -> usize;
}

fn longueur_c(texte: &str) -> usize {
    let c_texte = CString::new(texte).expect("pas de \\0 intérieur");
    // SAFETY: c_texte est une C-string valide terminée par \0.
    unsafe { strlen(c_texte.as_ptr()) }
}

// Exposer une fonction Rust au C
#[no_mangle]                          // garde le nom du symbole
pub extern "C" fn addition_c(a: i32, b: i32) -> i32 {
    a + b
}

fn main() {
    println!("len C : {}", longueur_c("hello"));
    // Lire une chaîne C (ex : retour d'API) :
    // let s: &str = unsafe { CStr::from_ptr(ptr).to_str().unwrap() };
}
```

Types de pont :

| Rust | C | Note |
|---|---|---|
| `CString` / `CStr` | `char*` nul-terminé | `CString::new` échoue si `\0` intérieur |
| `*const T` / `*mut T` | pointeurs bruts | Pas de garantie : `unsafe` pour déréférencer |
| `std::os::raw::{c_int, c_char…}` | types C | Tailles selon plateforme |
| `#[repr(C)] struct` | `struct` C | Layout compatible garanti |

Binaire système portable :

```bash
rustup target add x86_64-unknown-linux-musl
cargo build --release --target x86_64-unknown-linux-musl
file target/x86_64-unknown-linux-musl/release/mon_outil   # statically linked
```

> Crate `libc` : bindings bruts des appels système. Pour du code portable, préfère la std
> (`std::process`, `std::fs`) ou des crates (`nix` pour l'Unix idiomatique).

---

## 48. Les erreurs du borrow checker les plus fréquentes (décryptées)

### E0502 — `cannot borrow as mutable because it is also borrowed as immutable`

```rust
let mut v = vec![1, 2, 3];
let r = &v;
// v.push(4);            // ❌ E0502 : r emprunte encore v
println!("{r}");         // dernier usage de r
v.push(4);               // ✅ OK : r est mort (NLL)
```

### E0505 — `cannot move out because it is borrowed`

```rust
let v = vec![String::from("a")];
let r = &v[0];
// let s = v;            // ❌ E0505 : v est emprunté par r
println!("{r}");
let s = v;               // ✅ après dernier usage de r
println!("{} {}", s[0], r.len());  // ❌ r est mort ici en fait... voir correction :
// ✅ version propre :
let v2 = vec![String::from("a")];
let s2 = v2;             // move direct, sans emprunt actif
println!("{}", s2[0]);
```

### E0515 — `cannot return reference to local variable`

```rust
// ❌ fn bad() -> &str { let s = String::from("x"); &s }  // s meurt à la fin !
 // ✅ retourner une valeur possédée :
fn good() -> String {
    String::from("x")
}
// ✅ ou emprunter un paramètre (lifetime) :
fn premier<'a>(s: &'a str) -> &'a str { s }
```

### E0382 — `borrow of moved value` (le classique)

```rust
let s = String::from("hello");
prend(s);                            // move dans la fonction
// println!("{s}");                   // ❌ E0382
fn prend(_s: String) {}
// ✅ passer &s si la fonction n'a pas besoin de posséder :
fn emprunte(s: &str) { println!("{s}"); }
let s2 = String::from("hello");
emprunte(&s2);
println!("{s2}");                    // ✅ toujours valide
```

### E0499 — `cannot borrow as mutable more than once`

```rust
let mut x = 5;
let a = &mut x;
// let b = &mut x;       // ❌ E0499
*a += 1;
println!("{a}");         // dernier usage de a
let b = &mut x;          // ✅ OK maintenant
*b += 1;
```

### E0597 — `does not live long enough`

```rust
// ❌ let r: &str; { let s = String::from("x"); r = &s; }  // s meurt avant r
// ✅ faire vivre le propriétaire assez longtemps :
let s = String::from("x");
let r: &str = &s;
println!("{r}");
```

> Méthode de debug : lis le **dernier usage** de chaque emprunt. 90 % des erreurs E05xx
> se résolvent en **réordonnant** le code (utiliser l'emprunt avant la mutation) ou en
> **clonant** explicitement quand le partage de propriété est voulu (`Rc`/`Arc`).

---

## 49. Éditions 2021 et 2024 : ce qui change

| Sujet | Édition 2021 | Édition 2024 (Rust 1.85+) |
|---|---|---|
| `cargo new` | Édition 2021 par défaut | Préciser `edition = "2024"` dans Cargo.toml |
| Closures | Capturent les variables entières | Capturent **champ par champ** (plus précis, moins d'erreurs d'emprunt) |
| `gen` | Identifiant normal | Mot-clé réservé (générateurs) → renommer ou `r#gen` |
| `unsafe extern` | `extern` simple | `unsafe extern` requis pour déclarer des FFI |
| Résolution `match` ergonomique | Déjà là (2018) | Inchangée, mais match sur `&Option` encore adouci |
| Prelude | `TryInto`, `FromIterator`… | Ajouts : `Future`, `IntoFuture` dans le prelude |

Choisir dans `Cargo.toml` :

```toml
[package]
edition = "2024"
```

> Les éditions **ne cassent jamais** le code existant : un projet 2018 compile avec un
> compilateur 2024. Migrer = `cargo fix --edition` + revue manuelle. Pour un projet neuf
> en 2026 : **édition 2024** directement.

---

## 50. Workspaces : plusieurs crates, un seul build

```toml
# Cargo.toml à la racine
[workspace]
members = ["agent", "exporter", "commun"]
resolver = "2"          # résolveur moderne (recommandé depuis 2021)
```

```
mon_projet/
├── Cargo.toml          # [workspace]
├── Cargo.lock          # unique pour tout le workspace
├── target/             # build partagé
├── agent/
│   ├── Cargo.toml      # name = "agent" ; commun = { path = "../commun" }
│   └── src/main.rs
├── exporter/
│   ├── Cargo.toml
│   └── src/main.rs
└── commun/
    ├── Cargo.toml      # [lib]
    └── src/lib.rs
```

```bash
cargo build -p agent            # compiler un membre
cargo run -p exporter -- --help
cargo test --workspace          # tout tester
```

> Dépendance `path` entre membres = pas de version à gérer, recompilation incrémentale.
> Idéal : binaire agent + lib commune + exporter, tous versionnés ensemble.

---

## 51. Features Cargo : compilation conditionnelle propre

```toml
# Cargo.toml de la crate
[features]
default = ["json"]
json = ["dep:serde_json"]        # syntaxe moderne : dépendance optionnelle
yaml = ["dep:serde_yaml"]
tls = []

[dependencies]
serde_json = { version = "1", optional = true }
serde_yaml = { version = "0.9", optional = true }
```

```rust
// src/lib.rs
#[cfg(feature = "json")]
pub fn vers_json<T: serde::Serialize>(v: &T) -> String {
    serde_json::to_string(v).unwrap()
}

#[cfg(feature = "tls")]
pub fn client_tls() -> &'static str { "client TLS activé" }
```

```bash
cargo build                        # features default
cargo build --no-default-features   # sans rien
cargo build --features yaml,tls     # sélection
cargo build --all-features          # tout (CI)
```

> Les features doivent être **additives** : activer une feature ne doit jamais casser
> un code qui compile sans elle. Convention respectée par tout l'écosystème.

---

## 52. Macros : `macro_rules!` et derive procédurales

```rust
// Macro déclarative : pattern matching sur du code
macro_rules! hashmap {
    ( $( $cle:expr => $val:expr ),* $(,)? ) => {{
        let mut m = std::collections::HashMap::new();
        $( m.insert($cle, $val); )*
        m
    }};
}

fn main() {
    let seuils = hashmap! { "cpu" => 90, "disque" => 85, "ram" => 95 };
    println!("seuil cpu : {}", seuils["cpu"]);
}
```

| Type de macro | Usage | Exemples connus |
|---|---|---|
| Déclarative (`macro_rules!`) | Petits DSL, répétitions | `vec!`, `hashmap!` maison |
| Derive procédurale | Génère du code depuis un struct | `#[derive(Serialize)]`, `Parser` de clap |
| Attribut procédural | Transforme une fonction | `#[tokio::main]`, `#[test]` |
| Fonction procédurale | Appel `ma_macro!(...)` | `sqlx::query!` (vérifié à la compilation !) |

> Règle : **n'écris pas de macro** tant qu'une fonction générique + trait suffit.
> Les macros procédurales = crates séparées (`proc-macro = true`), à réserver aux cas
> où la génération de code apporte une vraie valeur (serde, clap, sqlx).

---

## 53. Sérialisation : serde en profondeur

```rust
use serde::{Deserialize, Serialize};

#[derive(Debug, Serialize, Deserialize)]
struct Interface {
    nom: String,
    #[serde(rename = "adresseIP")]          // nom différent dans le JSON
    ip: String,
    #[serde(default)]                       // absent -> Default::default()
    mtu: u16,
    #[serde(skip_serializing_if = "Option::is_none")]
    description: Option<String>,            // omis si None
    #[serde(skip)]                          // jamais sérialisé
    #[serde(default)]
    cache_interne: String,
}

#[derive(Debug, Serialize, Deserialize)]
#[serde(tag = "type")]                      // enum à tag interne
enum EquipementReseau {
    Switch { ports: u16 },
    Routeur { bgp: bool },
}
// JSON : {"type":"Switch","ports":48}

#[derive(Debug, Deserialize)]
struct ConfigSouple {
    #[serde(default)]
    hotes: Vec<String>,
    #[serde(flatten)]                       // champs inconnus capturés
    extra: std::collections::HashMap<String, serde_json::Value>,
}

fn main() -> Result<(), serde_json::Error> {
    let json = r#"{"nom":"eth0","adresseIP":"10.0.0.1","description":"uplink"}"#;
    let itf: Interface = serde_json::from_str(json)?;
    println!("{itf:?}");                    // mtu=0 (défaut), cache_interne=""
    let eq = EquipementReseau::Switch { ports: 48 };
    println!("{}", serde_json::to_string(&eq)?);
    Ok(())
}
```

> `#[serde(deny_unknown_fields)]` : strict sur les configs (détecte les fautes de frappe).
> `flatten` + `HashMap<String, Value>` : tolérant sur les formats qui évoluent.

---

## 54. CLI sérieuses avec clap (derive)

```toml
[dependencies]
clap = { version = "4", features = ["derive"] }
anyhow = "1"
```

```rust
use clap::{Parser, Subcommand};

/// Sonde réseau parallèle — exemple d'outil sysadmin
#[derive(Parser, Debug)]
#[command(name = "sonde", version, about = "Sonde TCP multi-hôtes")]
struct Cli {
    /// Hôtes à sonder (IP ou nom)
    #[arg(required = true)]
    hotes: Vec<String>,

    /// Port TCP à tester
    #[arg(short, long, default_value_t = 443)]
    port: u16,

    /// Timeout en millisecondes
    #[arg(long, default_value_t = 1000)]
    timeout_ms: u64,

    /// Sortie JSON
    #[arg(long)]
    json: bool,

    #[command(subcommand)]
    commande: Option<Commandes>,
}

#[derive(Subcommand, Debug)]
enum Commandes {
    /// Liste les hôtes sans sonder
    Lister,
}

fn main() -> anyhow::Result<()> {
    let cli = Cli::parse();   // --help et --version générés automatiquement
    println!("{cli:?}");
    Ok(())
}
```

```bash
cargo run -- --help
cargo run -- 10.0.0.1 10.0.0.2 -p 22 --timeout-ms 500 --json
cargo run -- lister --help
```

> clap v4 derive : un struct annoté = un CLI complet (help, erreurs, complétion shell
> via `clap_complete`). Pour les scripts internes simples, `std::env::args` suffit (§25).

---

## 55. Logging et tracing en production

```toml
[dependencies]
tracing = "0.1"
tracing-subscriber = { version = "0.3", features = ["env-filter", "json"] }
```

```rust
use tracing::{debug, error, info, instrument, warn};

#[instrument]                                  // span auto : nom + args de la fonction
fn sonder(hote: &str, port: u16) -> bool {
    debug!(hote, port, "début de sonde");
    let ok = !hote.is_empty() && port != 0;
    if ok { info!(hote, "joignable"); } else { warn!(hote, "injoignable"); }
    ok
}

fn main() {
    // Format JSON + niveau via RUST_LOG (ex : RUST_LOG=debug)
    tracing_subscriber::fmt()
        .json()
        .with_env_filter("sonde=info")
        .init();

    if !sonder("10.0.0.1", 443) {
        error!("sonde critique échouée");
        std::process::exit(1);
    }
}
```

| Niveau | Usage |
|---|---|
| `error!` | Panne nécessitant une action |
| `warn!` | Dégradation, à surveiller |
| `info!` | Événements métier normaux |
| `debug!` | Détail pour diagnostiquer |
| `trace!` | Très verbeux (boucles, paquets) |

> `tracing` > `log` pour l'async (spans suivent les tâches). En JSON + journald/ELK :
> tes logs deviennent requêtables. `RUST_LOG` en prod, jamais de niveau codé en dur.

---

## 56. Configuration : layered config (fichier + env + CLI)

Ordre de priorité idiomatique : **CLI > variables d'env > fichier > défauts**.

```toml
[dependencies]
config = "0.14"
serde = { version = "1", features = ["derive"] }
```

```rust
use serde::Deserialize;

#[derive(Debug, Deserialize)]
struct Config {
    #[serde(default = "defaut_hote")]
    hote: String,
    #[serde(default = "defaut_port")]
    port: u16,
    #[serde(default)]
    verbose: bool,
}
fn defaut_hote() -> String { "127.0.0.1".to_string() }
fn defaut_port() -> u16 { 8080 }

fn charger() -> Result<Config, config::ConfigError> {
    config::Config::builder()
        .add_source(config::File::with_name("/etc/sonde/config").required(false))
        .add_source(config::Environment::with_prefix("SONDE"))  // SONDE_PORT=9090
        .build()?
        .try_deserialize()
}

fn main() {
    match charger() {
        Ok(c) => println!("config : {c:?}"),
        Err(e) => { eprintln!("config invalide : {e}"); std::process::exit(2); }
    }
}
```

> Valide la config **au démarrage** et échoue vite (fail fast) avec un message qui dit
> QUELLE clé pose problème. Un daemon qui démarre à moitié configuré = incident garanti.

---

## 57. Gestion des processus : spawn, signaux, exit codes

```rust
use std::process::{Command, Stdio};

fn main() -> std::io::Result<()> {
    // Lancer un processus et capturer sa sortie
    let sortie = Command::new("ip")
        .args(["-brief", "addr"])
        .output()?;                                   // attend + capture stdout/stderr
    println!("code : {}", sortie.status.code().unwrap_or(-1));
    println!("{}", String::from_utf8_lossy(&sortie.stdout));

    // Succès/échec explicite
    let statut = Command::new("ping")
        .args(["-c", "1", "-W", "1", "10.0.0.1"])
        .stdout(Stdio::null())
        .status()?;
    println!("ping ok : {}", statut.success());

    // Pipeline : echo hello | wc -c (via deux Command chaînés, ou shell -c si besoin)
    // Préférer l'API directe au shell pour éviter l'injection :
    let _ = Command::new("systemctl")
        .args(["is-active", "sshd"])
        .status()?;
    Ok(())
}
```

Signaux (Unix) : la std ne les gère pas → crate `tokio::signal` ou `signal-hook`.

```rust
// Avec tokio :
// tokio::signal::ctrl_c().await?;   // attend Ctrl-C proprement pour shutdown gracieux
```

> **Jamais** de `format!("ping {user_input}")` passé à `sh -c` : injection de commande.
> `Command::new` + `.args()` = pas d'interprétation shell, arguments sûrs par construction.

---

## 58. Temps, durées, sleep : std::time et chrono

```rust
use std::time::{Duration, Instant, SystemTime, UNIX_EPOCH};

fn main() {
    // Mesure d'intervalle (monotonique) : Instant
    let debut = Instant::now();
    std::thread::sleep(Duration::from_millis(120));
    println!("écoulé : {:?}", debut.elapsed());          // jamais affecté par NTP

    // Horloge murale : SystemTime (peut reculer !)
    let maintenant = SystemTime::now();
    let epoch = maintenant.duration_since(UNIX_EPOCH).unwrap().as_secs();
    println!("epoch : {epoch}");

    // Durées : arithmétique sûre
    let timeout = Duration::from_secs(5);
    let total = timeout.checked_add(Duration::from_secs(10)).unwrap();
    println!("{}s", total.as_secs());
}
```

```toml
[dependencies]
chrono = "0.4"
```

```rust
// Dates humaines : chrono (l'écosystème ; `time` est l'alternative)
fn exemple_chrono() {
    use chrono::{Local, Utc};
    let utc = Utc::now();
    println!("{}", utc.to_rfc3339());              // 2026-09-26T23:34:28+00:00
    println!("{}", Local::now().format("%d/%m/%Y %H:%M"));
}
```

> `Instant` pour mesurer, `SystemTime`/`chrono` pour horodater. Ne jamais calculer
> une durée avec l'horloge murale (NTP, DST, ajustements manuels).

---

## 59. Expressions régulières et parsing de texte

```toml
[dependencies]
regex = "1"
```

```rust
use regex::Regex;

fn main() {
    // Parser des lignes de log : extraire IP et code
    let re = Regex::new(r#"^(?P<ip>\d+\.\d+\.\d+\.\d+).*?"code":(?P<code>\d+)"#).unwrap();
    let ligne = r#"10.0.0.5 - - [26/Sep/2026] "GET /" "code":200"#;
    if let Some(caps) = re.captures(ligne) {
        println!("ip={} code={}", &caps["ip"], &caps["code"]);
    }

    // Valider un hostname (simple)
    let re_host = Regex::new(r"^[a-z0-9]([a-z0-9-]{0,61}[a-z0-9])?$").unwrap();
    println!("valide : {}", re_host.is_match("srv-web-01"));

    // Splitter sur plusieurs séparateurs
    let re_sep = Regex::new(r"[;,\s]+").unwrap();
    let champs: Vec<&str> = re_sep.split("a;b, c  d").collect();
    println!("{champs:?}");
}
```

> Compile tes `Regex` **une fois** (coûteux) : `static` + `OnceLock` (std depuis 1.80),
> ou `lazy_static`/`once_cell` sur anciennes versions. Pour du parsing structuré
> (protocoles, formats), préfère `nom` ou `winnow` aux regex géantes.

---

## 60. Lecture/écriture binaire et endianness

```rust
fn main() {
    // Entiers <-> octets : endianness explicite (indispensable en réseau/protocoles)
    let n: u32 = 0x01020304;
    let be = n.to_be_bytes();      // big-endian (ordre réseau)
    let le = n.to_le_bytes();      // little-endian
    println!("be={be:?} le={le:?}");
    assert_eq!(u32::from_be_bytes(be), n);

    // Construire un paquet : en-tête fixe + payload
    let mut paquet = Vec::with_capacity(8);
    paquet.extend_from_slice(&0xCAFEu16.to_be_bytes());  // magic
    paquet.extend_from_slice(&(42u16).to_be_bytes());    // longueur
    paquet.extend_from_slice(b"data");                  // payload (4 octets)
    println!("paquet : {paquet:02X?}");

    // Parser : slices + from_be_bytes (jamais de cast sauvage)
    let magic = u16::from_be_bytes(paquet[0..2].try_into().unwrap());
    let len = u16::from_be_bytes(paquet[2..4].try_into().unwrap());
    println!("magic={magic:#X} len={len}");
}
```

> `try_into().unwrap()` sur slice de taille fixe = conversion vérifiée.
> Pour des protocoles complexes : crates `binrw` / `deku` (dérive déclarative).

---

## 61. Documentation : rustdoc comme un pro

```rust
/// Sonde TCP avec timeout.
///
/// # Arguments
///
/// * `hote` - IP ou nom DNS (ex : `"10.0.0.1"`).
/// * `port` - Port TCP (ex : `443`).
///
/// # Exemple
///
/// ```
/// assert!(mon_outil::sonder("127.0.0.1", 22));
/// ```
///
/// # Erreurs
///
/// Ne retourne pas d'erreur : `false` = injoignable ou timeout.
///
/// # Panics
///
/// Ne panique jamais.
pub fn sonder(hote: &str, port: u16) -> bool {
    !hote.is_empty() && port != 0
}
```

Sections conventionnelles : `# Arguments`, `# Returns`, `# Errors`, `# Panics`,
`# Safety` (si unsafe), `# Examples`. Le code des exemples = **doctests** (§29).

```bash
cargo doc --open --no-deps     # doc de TA crate uniquement
```

> Une fonction publique sans `///` = dette. `cargo doc` + `#![warn(missing_docs)]`
> en tête de lib pour l'exiger.

---

## 62. Édition 2024 en pratique : ce qui change vraiment ton code

Au-delà du tableau §49, les impacts concrets :

```rust
// 1. Captures de closures précises (édition 2024)
// Avant (2021) : la closure capturait `config` ENTIER par référence...
// Après (2024) : elle ne capture que `config.port` -> moins de conflits d'emprunt
struct Config { port: u16, hote: String }
fn exemple_ed2024() {
    let mut config = Config { port: 8080, hote: "x".into() };
    let lire_port = || config.port;      // 2024 : capture config.port uniquement
    println!("{}", lire_port());
    config.hote = "y".into();            // ✅ OK en 2024 (conflit en 2021 !)
}

// 2. `gen` est réservé : utiliser r#gen si besoin
// let r#gen = 5;

// 3. unsafe extern pour les FFI (édition 2024)
unsafe extern "C" {
    fn getpid() -> i32;
}
```

> Migration 2021 → 2024 : `cargo fix --edition`, puis `cargo test`.
> Les gains (captures précises) valent le coup sur du code concurrent.

---

## 63. Dépendances : choisir, auditer, verrouiller

```bash
cargo tree                    # qui dépend de quoi
cargo tree -i serde            # qui amène serde ?
cargo tree --depth 1
cargo outdated                 # versions plus récentes (cargo install cargo-outdated)
```

`Cargo.lock` :

- **Binaires** : committer TOUJOURS (reproductibilité du déploiement).
- **Bibliothèques** : ne PAS committer (laisser le résolveur choisir ; convention crates.io).

Sécurité supply chain :

```bash
cargo install cargo-audit cargo-deny
cargo audit                   # CVE connues dans tes dépendances
cargo deny check               # licences + advisories + sources (CI)
```

```toml
# deny.toml minimal
[advisories]
vulnerability = "deny"
[licenses]
allow = ["MIT", "Apache-2.0", "BSD-3-Clause", "ISC"]
```

> Checklist avant d'ajouter une crate : maintenance active ? téléchargements ?
> `unsafe` dedans (`cargo geiger`) ? licence compatible ? alternative dans la std ?

---

## 64. Cross-compilation et release : livrer un binaire

```bash
# Cibles utiles au sysadmin
rustup target add x86_64-unknown-linux-musl      # Linux statique (containers scratch)
rustup target add aarch64-unknown-linux-gnu      # ARM64 (Raspberry Pi, serveurs ARM)
rustup target add x86_64-pc-windows-gnu          # Windows depuis Linux

cargo build --release --target x86_64-unknown-linux-musl
```

`Cargo.toml` : profil release soigné :

```toml
[profile.release]
opt-level = 3
lto = true
codegen-units = 1
strip = true          # ou "debuginfo" pour garder les symboles séparés
panic = "abort"       # binaire plus petit (pas de unwinding) — attention : pas de catch
```

Dockerfile multi-stage typique :

```dockerfile
FROM rust:1.82 AS build
WORKDIR /app
COPY . .
RUN cargo build --release --target x86_64-unknown-linux-musl

FROM scratch
COPY --from=build /app/target/x86_64-unknown-linux-musl/release/mon_outil /mon_outil
ENTRYPOINT ["/mon_outil"]
```

> Binaire musl statique + image `scratch` = ~5 Mo, zéro CVE d'OS, déploiement trivial.
> Vérifie `ldd` / `file` après build pour confirmer le statique.

---

## 65. Déboguer le borrow checker : méthode pas à pas

Quand le compilateur dit non, applique cette procédure :

1. **Lis l'erreur EN ENTIER** : Rust donne la ligne, la cause, et souvent la solution
   (`help: ...`). Les E05xx sont les plus pédagogiques du marché.
2. **Identifie les 3 acteurs** : le propriétaire, l'emprunt actif, l'opération refusée.
3. **Trouve le dernier usage** de l'emprunt : peux-tu réordonner (utiliser avant de muter) ?
4. **Questionne le besoin** : faut-il vraiment partager ? Souvent on peut :
   - cloner explicitement (petites données),
   - passer par valeur puis retourner (transfert aller-retour),
   - restructurer (calculer d'abord, muter ensuite).
5. **Si partage réel** : `Rc<T>` (mono-thread) / `Arc<T>` (multi-thread) + `RefCell`/`Mutex`
   pour la mutabilité intérieure.
6. **Ne contourne JAMAIS** avec `unsafe` pour faire taire le borrow checker.

Exemple de restructuration (le pattern le plus rentable) :

```rust
// ❌ calcul + mutation entremêlés
// let mut cache = std::collections::HashMap::new();
// let v = cache.get("cle");          // emprunt immuable
// cache.insert("cle", 1);            // ❌ E0502
// println!("{v:?}");

// ✅ séparer : phase de lecture, puis phase d'écriture
let mut cache = std::collections::HashMap::new();
let existe = cache.contains_key("cle");   // lecture terminée (bool : Copy)
if !existe {
    cache.insert("cle", 1);               // écriture, plus d'emprunt actif
}
println!("{existe}");
```

---

## 66. Trait objects (`dyn Trait`) : dispatch dynamique

Quand le type concret n'est pas connu à la compilation (plugins, handlers hétérogènes) :

```rust
trait Sondable {
    fn sonder(&self) -> bool;
    fn nom(&self) -> &str;
}

struct Ping { hote: String }
struct Http { url: String }

impl Sondable for Ping {
    fn sonder(&self) -> bool { !self.hote.is_empty() }
    fn nom(&self) -> &str { &self.hote }
}
impl Sondable for Http {
    fn sonder(&self) -> bool { self.url.starts_with("https") }
    fn nom(&self) -> &str { &self.url }
}

fn main() {
    // Vec de types DIFFÉRENTS via trait object (taille connue via Box)
    let sondes: Vec<Box<dyn Sondable>> = vec![
        Box::new(Ping { hote: "10.0.0.1".into() }),
        Box::new(Http { url: "https://srv".into() }),
    ];
    for s in &sondes {
        println!("{} : {}", s.nom(), s.sonder());
    }
}
```

| | Génériques (`T: Trait`) | Trait objects (`dyn Trait`) |
|---|---|---|
| Dispatch | Statique (monomorphisation) | Dynamique (vtable) |
| Coût | Zéro à l'exécution | Indirection légère |
| Types hétérogènes en collection | Non | Oui |
| Taille connue à la compilation | Oui | Non → `Box`/`&` |

> Règle : génériques par défaut (perf) ; `dyn Trait` quand l'hétérogénéité l'exige.
> Un trait objet-safe = pas de méthodes génériques, pas de `Self` en retour.

---

## 67. Intériorité mutabilité : Cell, RefCell, OnceLock

Parfois la propriété est partagée (`&T`) mais on doit muter : **mutabilité intérieure**,
vérifiée à l'exécution plutôt qu'à la compilation.

```rust
use std::cell::{Cell, RefCell};
use std::sync::OnceLock;

// Cell<T: Copy> : get/set sans emprunt
let c = Cell::new(0);
c.set(c.get() + 1);

// RefCell<T> : emprunt vérifié à l'EXÉCUTION (panique si règles violées)
let rc = RefCell::new(vec![1, 2]);
rc.borrow_mut().push(3);          // panic si déjà emprunté (mut ou immut)
// RefCell n'est PAS Send/Sync : mono-thread uniquement !

// OnceLock<T> : initialisation paresseuse thread-safe (std depuis 1.80)
static CONFIG: OnceLock<String> = OnceLock::new();
fn config() -> &'static str {
    CONFIG.get_or_init(|| {
        std::env::var("APP_CONFIG").unwrap_or_else(|_| "/etc/app.conf".into())
    })
}

fn main() {
    println!("{}", c.get());
    println!("{:?}", rc.borrow());
    println!("{}", config());
}
```

| Type | Thread-safe | Vérification | Usage |
|---|---|---|---|
| `Cell<T: Copy>` | Non | — | Compteurs simples mono-thread |
| `RefCell<T>` | Non | Exécution (panic si abus) | Partage `&` + mutation mono-thread |
| `Mutex<T>` | Oui | Exécution (blocage) | Partage multi-thread |
| `OnceLock<T>` | Oui | — | Global initialisé une fois |

---

## 68. Smart pointers : Box, Rc, Arc

```rust
use std::rc::Rc;
use std::sync::Arc;

fn main() {
    // Box<T> : allocation unique sur le tas, propriétaire unique
    let b = Box::new([0u8; 1024]);       // gros tableau : évite de saturer la pile
    println!("len {}", b.len());

    // Rc<T> : propriété PARTAGÉE mono-thread (reference counted)
    let partage = Rc::new(String::from("config"));
    let a = Rc::clone(&partage);         // compteur += 1 (pas cher)
    let b2 = Rc::clone(&partage);
    println!("{} refs = {}", a, Rc::strong_count(&partage));  // 3

    // Arc<T> : comme Rc mais atomique -> threads (voir §45)
    let partage_t = Arc::new(42);
    let _c = Arc::clone(&partage_t);

    // Rc/Arc + RefCell/Mutex pour partager ET muter :
    let mutable_partage = Rc::new(RefCell::new(0));
    *mutable_partage.borrow_mut() += 1;
    println!("{}", mutable_partage.borrow());
}
```

> `Rc::clone(&x)` plutôt que `x.clone()` : signale visuellement qu'on ne clone pas la donnée.
> Cycles `Rc` → fuite : casser avec `Weak::new()`.

---

## 69. Erreurs FFI/libc courantes et garde-fous

- Oublier `CString` : passer `&str` brut à du C = octets non terminés → UB.
- `CStr::from_ptr` sur pointeur nul ou non terminé → UB. Vérifier le contrat de l'API C.
- `String::from_raw_parts` : à réserver aux cas où TU as alloué via l'API Rust.
- Thread C qui rappelle du Rust : déclarer `extern "C" fn` côté Rust, attention aux panics
  (un panic qui traverse la FFI = UB → `catch_unwind` aux frontières).

---

## 70. Patterns utiles : builder, newtype, type state

```rust
// --- Builder : construction lisible d'objets complexes ---
#[derive(Debug)]
struct Sonde { hote: String, port: u16, timeout_ms: u64, retries: u8 }

impl Sonde {
    fn builder(hote: &str) -> SondeBuilder {
        SondeBuilder { hote: hote.to_string(), port: 443, timeout_ms: 1000, retries: 3 }
    }
}

struct SondeBuilder { hote: String, port: u16, timeout_ms: u64, retries: u8 }
impl SondeBuilder {
    fn port(mut self, p: u16) -> Self { self.port = p; self }
    fn timeout_ms(mut self, t: u64) -> Self { self.timeout_ms = t; self }
    fn retries(mut self, r: u8) -> Self { self.retries = r; self }
    fn build(self) -> Sonde {
        Sonde { hote: self.hote, port: self.port, timeout_ms: self.timeout_ms, retries: self.retries }
    }
}

// --- Newtype : unités impossibles à mélanger ---
struct Millisecondes(u64);
struct Pourcent(u8);
fn attendre(d: Millisecondes) { std::thread::sleep(std::time::Duration::from_millis(d.0)); }

fn main() {
    let s = Sonde::builder("10.0.0.1").port(22).retries(5).build();
    println!("{s:?}");
    attendre(Millisecondes(200));
    // attendre(Pourcent(50));   // ❌ ne compile pas : type différent !
}
```

> Builder = quand > 3 paramètres ou beaucoup d'optionnels. Newtype = quand une confusion
> d'unités peut casser du matériel (ms vs s, W vs kW, % vs ratio).

---

## 71. Optimiser les allocations : le guide express

```rust
fn main() {
    // 1. Pré-allouer quand la taille est connue
    let mut v = Vec::with_capacity(10_000);   // évite les réallocations successives
    let mut s = String::with_capacity(1024);

    // 2. Réutiliser au lieu de recréer (boucles chaudes)
    let mut tampon = String::new();
    for i in 0..1000 {
        tampon.clear();                        // garde la capacité allouée
        tampon.push_str(&format!("ligne {i}"));
    }

    // 3. Emprunter plutôt que cloner dans les signatures
    // fn f(s: &str) plutôt que fn f(s: String) quand on ne stocke pas

    // 4. Cow : clone uniquement si mutation nécessaire
    use std::borrow::Cow;
    fn normaliser(entree: &str) -> Cow<str> {
        if entree.chars().all(|c| !c.is_whitespace()) {
            Cow::Borrowed(entree)              // zéro allocation
        } else {
            Cow::Owned(entree.split_whitespace().collect::<Vec<_>>().join(" "))
        }
    }
    println!("{}", normaliser("hello"));
    println!("{}", normaliser("hel lo"));

    // 5. Éviter format! en boucle chaude : write! dans un buffer réutilisé
    use std::fmt::Write as _;
    let mut buf = String::with_capacity(64);
    write!(buf, "val={}", 42).unwrap();
    println!("{buf} cap={}", v.capacity());
    let _ = s;
}
```

---

## 72. Panics : quand c'est légitime (et comment les contenir)

Légitime :

- Invariant interne violé = bug (`unreachable!`, `assert!` sur préconditions internes).
- Échec d'initialisation irrécupérable au démarrage (config absente → exit propre plutôt).
- Tests et prototypes.

Illégitime : fichier manquant, réseau coupé, entrée utilisateur invalide → `Result`.

```rust
fn main() {
    // catch_unwind : dernier recours (frontière FFI, plugin, worker isolé)
    let resultat = std::panic::catch_unwind(|| {
        // code qui pourrait paniquer (dépendance fragile)
        let v = vec![1, 2, 3];
        v[10]   // panic : index out of bounds
    });
    match resultat {
        Ok(val) => println!("ok : {val}"),
        Err(_) => eprintln!("le worker a paniqué, on continue"),
    }

    // assert! avec message : documente l'invariant
    let seuil = 90u8;
    assert!(seuil <= 100, "seuil {seuil} incohérent : doit être <= 100");
}
```

> En lib : ne panique jamais sur des entrées externes. En binaire : `panic = "abort"`
> en release si tu veux des binaires plus petits ET aucun unwinding à travers du C.

---

## 73. Données et fichiers : CSV, TOML, YAML

```toml
[dependencies]
csv = "1"
serde = { version = "1", features = ["derive"] }
toml = "0.8"
```

```rust
use serde::{Deserialize, Serialize};

#[derive(Debug, Deserialize, Serialize)]
struct Mesure {
    horodatage: String,
    equipement: String,
    watts: f64,
}

fn lire_csv(chemin: &str) -> Result<Vec<Mesure>, csv::Error> {
    let mut lecteur = csv::Reader::from_path(chemin)?;
    lecteur.deserialize().collect()   // Vec<Mesure> via FromIterator<Result>
}

fn ecrire_csv(mesures: &[Mesure]) -> Result<(), csv::Error> {
    let mut ecrivain = csv::Writer::from_writer(std::io::stdout());
    for m in mesures {
        ecrivain.serialize(m)?;
    }
    ecrivain.flush()?;
    Ok(())
}

#[derive(Debug, Deserialize)]
struct ConfToml {
    general: General,
}
#[derive(Debug, Deserialize)]
struct General {
    site: String,
    #[serde(default)]
    verbose: bool,
}

fn lire_toml(texte: &str) -> Result<ConfToml, toml::de::Error> {
    toml::from_str(texte)
}

fn main() {
    // CSV : mesures.csv -> horodatage,equipement,watts
    // TOML :
    //   [general]
    //   site = "datacenter-nord"
    let cfg = lire_toml("[general]\nsite = \"datacenter-nord\"\n").unwrap();
    println!("site : {}", cfg.general.site);
    let _ = (lire_csv, ecrire_csv);
}
```

---

## 74. Base de données : SQL avec sqlx (intro)

```toml
[dependencies]
sqlx = { version = "0.8", features = ["runtime-tokio", "sqlite"] }
tokio = { version = "1", features = ["full"] }
```

```rust
#[derive(Debug)]
struct EquipementDb { id: i64, hostname: String }

async fn demo() -> Result<(), sqlx::Error> {
    // SQLite en mémoire : parfait pour tests/outils embarqués
    let pool = sqlx::SqlitePool::connect("sqlite::memory:").await?;

    sqlx::query("CREATE TABLE equipements (id INTEGER PRIMARY KEY, hostname TEXT NOT NULL)")
        .execute(&pool).await?;

    // Requête VÉRIFIÉE À LA COMPILATION avec query! (nécessite DATABASE_URL au build)
    // let eq = sqlx::query_as!(EquipementDb,
    //     "SELECT id, hostname FROM equipements WHERE id = ?", 1u32)
    //     .fetch_one(&pool).await?;

    // Version dynamique (sans vérification compile-time) :
    sqlx::query("INSERT INTO equipements (hostname) VALUES (?)")
        .bind("srv-01")
        .execute(&pool).await?;
    let rows: Vec<EquipementDb> = sqlx::query_as("SELECT id, hostname FROM equipements")
        .fetch_all(&pool).await
        .map(|v| v.into_iter().map(|(id, hostname): (i64, String)|
            EquipementDb { id, hostname }).collect())?;

    println!("{rows:?}");
    Ok(())
}

#[tokio::main]
async fn main() {
    if let Err(e) = demo().await { eprintln!("db : {e}"); }
}
```

> `query!` vérifie le SQL **à la compilation** contre une vraie base (via `DATABASE_URL`) :
> colonne renommée = erreur de build, pas incident de prod. Postgres/MySQL/SQLite supportés.

---

## 75. Checklist de revue de code Rust (avant chaque merge)

- [ ] `cargo fmt --check` passe
- [ ] `cargo clippy -- -D warnings` passe
- [ ] Aucun `unwrap()`/`expect()` injustifié dans le code applicatif
- [ ] Aucun `panic!` sur erreur récupérable (I/O, réseau, parsing)
- [ ] Erreurs avec contexte (`anyhow`) côté binaire, typées (`thiserror`) côté lib
- [ ] `&str`/`&[T]` en paramètres plutôt que `&String`/`&Vec<T>`
- [ ] Pas de clone défensif sans raison (emprunts privilégiés)
- [ ] Lifetimes explicites uniquement où nécessaire (retour de référence)
- [ ] `unsafe` encapsulé + commentaire `// SAFETY:`
- [ ] Tests : nouveau comportement = nouveau test ; bug = test de non-régression
- [ ] Doc `///` sur les items publics (+ doctests qui passent)
- [ ] `Cargo.lock` committé (binaire) ; features additives
- [ ] `cargo audit` sans vulnérabilité bloquante
- [ ] Logs sur stderr / données sur stdout ; codes de sortie conventionnels
- [ ] Mesuré en `--release` si perf revendiquée

---

## 76. Anti-patterns : ce qu'il ne faut plus faire (venu d'autres langages)

| Habitude d'ailleurs | Équivalent Rust idiomatique |
|---|---|
| `null` / `None` checks partout (Python) | `Option<T>` + combinateurs (`map`, `?`) |
| Exceptions (Java/Python) | `Result<T, E>` + `?` + thiserror/anyhow |
| Héritage de classes (Java/C++) | Traits + composition (pas d'héritage en Rust) |
| Getters/setters systématiques | Champs `pub` si trivial, méthodes si logique |
| Variables globales mutables (C) | `OnceLock` / `Mutex` / paramètres explicites |
| `char*` et `strcpy` (C) | `String`/`&str` (UTF-8 garanti) |
| Threads + mémoire partagée sans garde (C) | `Arc<Mutex<T>>` ou channels (vérifié à la compilation) |
| `void*` générique (C) | Génériques `T` ou `dyn Trait` |
| Macros préprocesseur `#define` (C) | `const`, `macro_rules!`, generics |
| GC implicite (Go/Java) | Ownership : libération déterministe, zéro GC |

---

## 77. Ressources système : aller plus loin côté sysadmin

Pistes de projets pour ancrer les réflexes :

1. **Exporter de métriques** : lire `/proc`, exposer en Prometheus (axum + endpoint `/metrics`).
2. **Agent d'inventaire** : SSH/parallèle (threads), sortie JSON (serde), config TOML.
3. **Rotate de logs** : surveiller un fichier, compresser, retenir N archives (fs + chrono).
4. **Watchdog de service** : boucle de sondes, alertes, backoff exponentiel.
5. **Outil de sauvegarde** : parcours récursif (`walkdir`), hash (sha2), manifest JSON.

Crates utiles au sysadmin :

| Crate | Usage |
|---|---|
| `clap` | CLI |
| `serde` / `serde_json` / `toml` / `csv` | Formats de données |
| `tokio` / `reqwest` | Async + HTTP |
| `tracing` | Logs structurés |
| `anyhow` / `thiserror` | Erreurs |
| `sysinfo` | CPU/RAM/disques/processus (cross-platform) |
| `walkdir` | Parcours récursif |
| `notify` | Watch de fichiers (inotify) |
| `cron` (crate `cron`) | Planification façon cron |
| `nix` | Appels Unix idiomatiques |
| `prometheus` | Métriques Prometheus |

---

*Fin du guide. Le borrow checker est ton ami le plus exigeant : s'il râle, c'est qu'il vient
de t'éviter un incident de prod. Bon code, et que tes binaires soient statiques et tes
panics absentes.* 🦀
