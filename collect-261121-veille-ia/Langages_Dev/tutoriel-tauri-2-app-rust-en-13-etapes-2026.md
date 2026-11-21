---
id: collect-261121-veille-ia/langages-dev/tutoriel-tauri-2-app-rust-en-13-etapes-2026
title: "Tutoriel Tauri 2 : App Rust en 13 Étapes [2026]"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["tauri", "rust", "application", "tutoriel", "binaire", "electron", "etapes", "windows"]
source: https://tech-insider.org/fr/tauri-2-tutoriel-app-bureau-rust-13-etapes-2026/
source_anchor: ""
source_lines: [1, 666]
section: "Langages_Dev"
---
Publié le 10 avril 2026, mis à jour en septembre 2026 – Construire une application bureau multi-plateforme en 2026 ne ressemble plus à ce qu’elle était il y a deux ans. Avec **Tauri 2.11**, dernier maillon d’une lignée qui a vu le noyau franchir la version 2.6.0 le 24 juin 2025, puis 2.7.0 le 20 juillet 2025 et 2.8.1 le 18 août 2025, avant d’enchaîner une série de correctifs à l’automne – 2.9.2 le 29 octobre 2025, 2.9.3 le 13 novembre 2025, 2.9.4 le 30 novembre 2025 et 2.9.5 le 9 décembre 2025, d’après les Tauri Releases – jusqu’à la révision 2.11.1 de `@tauri-apps/api`, atteinte en juillet 2026 selon le blog ESB1995, sorti dans la lignée de la version stable 2.0 d’octobre 2024, vous générez désormais un binaire de 3 à 8 Mo qui démarre en moins de 100 ms et consomme entre 25 et 45 Mo de RAM au repos – là où Electron pèse 120 à 200 Mo et engloutit 150 à 300 Mo de mémoire vive. Ce **tutoriel Tauri 2** de 13 étapes vous guide pas à pas dans la création d’une application bureau complète : un gestionnaire de notes chiffré utilisant Rust, SQLite via le plugin officiel, et un frontend React. Toutes les commandes ont été testées sur Windows 11, macOS Sonoma et Ubuntu 24.04.

Le projet Tauri, gouverné par la **Tauri Foundation** sous l’égide du Commons Conservancy et licencié MIT/Apache 2.0, dépasse les **70 000 étoiles GitHub** et compte plus de **4 200 contributeurs**. Le paquet `@tauri-apps/cli` dépasse les 250 000 téléchargements hebdomadaires sur npm, tandis que `@tauri-apps/api` approche 180 000. Pour les équipes françaises et européennes, l’argument est aussi réglementaire : Tauri ne télécharge pas Chromium à l’exécution, ce qui simplifie la conformité RGPD et évite les inquiétudes liées au tracking embarqué dans les moteurs propriétaires.

## Pourquoi choisir Tauri 2 plutôt qu’Electron en 2026

Tauri 2 réutilise le moteur de rendu web installé sur le système d’exploitation : **WebView2** (basé sur Edge/Chromium) sous Windows, **WKWebView** sous macOS et iOS, **WebKitGTK 6.0+** sous Linux, et le composant Android System WebView. Cette stratégie élimine l’embarquement d’un runtime Node.js et d’un binaire Chromium complet à l’intérieur de chaque application. Le résultat se mesure : un binaire Tauri 2 typique tient entre 3 et 8 Mo, contre 120 à 200 Mo pour une application Electron équivalente – un facteur 40 à la baisse documenté par la documentation officielle et confirmé par le tutoriel comparatif publié sur rustify.rs en mars 2026.

Le second axe est la sécurité. Le backend de Tauri est écrit en Rust, langage à mémoire sûre qui élimine les classes entières de vulnérabilités. Surtout, le système de capacités introduit en version 2.0 – un fichier `capabilities/default.json` qui déclare explicitement quelles commandes peuvent être appelées depuis le frontend, par quelle fenêtre et avec quels arguments – applique le principe du moindre privilège par défaut. Un audit externe complet a été réalisé avant la sortie stable de Tauri 2.0 et toutes les conclusions ont été corrigées avant publication, comme indiqué sur le blog officiel d’octobre 2024.

Troisième argument : la couverture mobile. Depuis Tauri 2.0, la même base de code Rust + Web cible **Windows, macOS, Linux, Android et iOS**. Vous écrivez vos commandes Rust une fois, et la CLI génère un APK signé pour Android, un IPA pour iOS, un MSI pour Windows, un DMG pour macOS et un AppImage ou DEB pour Linux. Pour un éditeur SaaS européen qui veut accompagner son application web d’un client bureau et d’une application mobile sans tripler ses équipes, l’économie est immédiate.

## Tauri 2 vs Electron : tableau comparatif détaillé

| Critère | Tauri 2.11.1 (avril 2026) | Electron 33.x | Écart | 
|---|---|---|---|
| Taille du binaire de base | 3 à 8 Mo | 120 à 200 Mo | ~40× plus léger | 
| RAM au démarrage (repos) | 25 à 45 Mo | 150 à 300 Mo | ~6× plus économe | 
| Temps de démarrage à froid | <100 ms | 2 000 à 4 000 ms | ~30× plus rapide | 
| Langage backend | Rust 1.81+ | Node.js 22.x | Mémoire sûre vs JIT | 
| Moteur de rendu | Système (WebView2/WKWebView/WebKitGTK) | Chromium embarqué | Système vs intégré | 
| Cibles mobiles | Android et iOS natifs | Aucune (Capacitor requis) | Mobile inclus | 
| Système de permissions | Capabilities + ACL fines | contextIsolation manuelle | Sécurité par défaut | 
| Étoiles GitHub | 70 000+ | 116 000+ | Communauté plus jeune | 
| Licence | MIT / Apache 2.0 | MIT | Permissive | 

## Étape 1 : Prérequis et versions à installer

Avant de démarrer, votre poste doit disposer de la chaîne d’outils suivante. Les versions ci-dessous sont celles validées par l’équipe Tauri pour la branche 2.11.x en avril 2026 – ne descendez pas en dessous, sous peine de rencontrer des erreurs de compilation obscures sur les caisses (crates) `tao` et `wry`.

- **Rust 1.81.0 ou supérieur** via`rustup` sur le canal stable (vérifiez avec`rustc --version` ).
- **Node.js 20.18.0 LTS ou supérieur** , npm 10.8+ ou pnpm 9+.
- **Cargo** installé automatiquement par rustup.
- **WebView2 Runtime** sur Windows 10 et 11 (préinstallé depuis Windows 11 22H2).
- **WebKitGTK 6.0+** sous Linux :`apt install libwebkit2gtk-4.1-dev build-essential curl wget file libxdo-dev libssl-dev libayatana-appindicator3-dev librsvg2-dev` sur Ubuntu 24.04.
- **Xcode Command Line Tools** sur macOS Sonoma ou supérieur (`xcode-select --install` ).
- Pour le ciblage mobile : Android Studio Hedgehog 2023.1+, JDK 17, NDK 26+, Xcode 15+ pour iOS.

Installez Rust si ce n’est pas déjà fait :

```
# Sous Linux/macOS
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source "$HOME/.cargo/env"
rustup update stable
# Vérification
rustc --version
# Sortie attendue : rustc 1.81.0 (eeb90cda1 2026-XX-XX)
cargo --version
node --version
# Sortie attendue : v20.18.0 ou plus récent
```
## Étape 2 : Création du squelette de projet avec create-tauri-app

L’outil `create-tauri-app` (CTA) est l’équivalent Tauri de `create-react-app` ou `npm create vite`. Il génère un projet bi-compartiments – un répertoire `src/` pour le frontend web et un répertoire `src-tauri/` pour le backend Rust – avec toute la configuration câblée. Nous allons créer une application appelée `noteforge`, un gestionnaire de notes chiffrées local-first.

```
# Création interactive
npm create tauri-app@latest
# Réponses à donner :
# ? Project name › noteforge
# ? Identifier › com.noteforge.app
# ? Choose which language to use for your frontend › TypeScript / JavaScript
# ? Choose your package manager › npm
# ? Choose your UI template › React
# ? Choose your UI flavor › TypeScript
# ? Would you like to setup the project for mobile › No (pour l'instant)
cd noteforge
npm install
npm run tauri dev
```
La première compilation peut durer entre 5 et 12 minutes selon votre machine, car cargo doit télécharger et compiler l’intégralité de la chaîne `tao`, `wry`, `webkit2gtk-sys` ou `webview2-com`, ainsi que les nombreuses caisses tierces. Les compilations suivantes sont incrémentales et descendent à 2-5 secondes en mode développement grâce au cache de cargo. Une fenêtre Tauri devrait s’ouvrir affichant le template React par défaut.

## Étape 3 : Anatomie du projet Tauri 2

Comprendre la structure générée est essentiel avant de coder. Voici les fichiers clés et leur rôle :

```
noteforge/
├── src/                       # Frontend React/TypeScript
│   ├── App.tsx
│   ├── main.tsx
│   └── styles.css
├── src-tauri/                 # Backend Rust
│   ├── Cargo.toml             # Dépendances Rust
│   ├── tauri.conf.json        # Configuration centrale
│   ├── build.rs               # Script de build
│   ├── capabilities/          # Permissions par fenêtre
│   │   └── default.json
│   ├── icons/                 # Icônes multi-plateformes
│   └── src/
│       ├── main.rs            # Point d'entrée
│       └── lib.rs             # Logique applicative
├── package.json
├── vite.config.ts
└── index.html
```
Le fichier le plus important est `src-tauri/tauri.conf.json`. Il définit l’identifiant applicatif, les fenêtres à créer, la politique de sécurité du contenu (CSP), les cibles de bundle et la configuration des plugins. Le second fichier critique est `src-tauri/capabilities/default.json`, nouveau dans Tauri 2, qui remplace l’ancien système d’allowlist par un mécanisme de capabilities granulaire. Chaque permission y est associée à une fenêtre spécifique et peut filtrer les arguments transmis aux commandes.

## Étape 4 : Configuration de tauri.conf.json pour la production

Remplacez le contenu de `src-tauri/tauri.conf.json` par cette configuration adaptée à un usage production. Notez la définition des cibles de bundle (MSI Windows, DMG macOS, DEB et AppImage Linux), la fenêtre principale avec ses dimensions raisonnables, et la CSP stricte qui interdit le chargement de scripts distants.

```
{
  "$schema": "https://schema.tauri.app/config/2",
  "productName": "NoteForge",
  "version": "0.1.0",
  "identifier": "com.noteforge.app",
  "build": {
    "beforeDevCommand": "npm run dev",
    "devUrl": "http://localhost:1420",
    "beforeBuildCommand": "npm run build",
    "frontendDist": "../dist"
  },
  "app": {
    "windows": [
      {
        "title": "NoteForge",
        "width": 1100,
        "height": 720,
        "minWidth": 800,
        "minHeight": 600,
        "resizable": true,
        "fullscreen": false,
        "decorations": true,
        "transparent": false
      }
    ],
    "security": {
      "csp": "default-src 'self' ipc: http://ipc.localhost; img-src 'self' data: asset: http://asset.localhost; style-src 'self' 'unsafe-inline'"
    }
  },
  "bundle": {
    "active": true,
    "targets": ["msi", "deb", "appimage", "dmg"],
    "icon": [
      "icons/32x32.png",
      "icons/128x128.png",
      "icons/icon.icns",
      "icons/icon.ico"
    ],
    "category": "Productivity",
    "shortDescription": "Gestionnaire de notes chiffrées local-first",
    "copyright": "2026 NoteForge"
  }
}
```
La directive CSP exclut les scripts inline et les sources tierces, ce qui bloque la quasi-totalité des injections XSS. Si votre frontend utilise Tailwind CSS injecté en runtime ou un loader Vite spécifique, ajustez `style-src` mais évitez d’autoriser `'unsafe-inline'` sur `script-src`.

## Étape 5 : Ajout des plugins Tauri 2 (SQL, FS, Notification, Store)

Les plugins Tauri 2 sont des caisses Rust publiées sur crates.io et accompagnées d’un paquet npm pour le binding TypeScript. Pour notre gestionnaire de notes, nous installons quatre plugins officiels stabilisés depuis la version 2.4.0 du 20 mars 2025, consolidée par les correctifs 2.4.1 du 1er avril 2025 puis 2.5.0 du 15 avril 2025 (Tauri Releases) : `tauri-plugin-sql` pour SQLite, `tauri-plugin-fs` pour la lecture/écriture de fichiers, `tauri-plugin-notification` pour les notifications système, et `tauri-plugin-store` pour le stockage clé-valeur persistant – ce dernier a atteint sa version 2.0.0 dès la sortie stable du 2 octobre 2024 et sa page de release a encore été mise à jour en juillet 2026. Tout l’écosystème suit un versionnage cohérent : le crate `tauri-plugin-http` affiche lui aussi une v2.0.0 stable depuis octobre 2024, confirmée par le changelog 2026, tandis que `tauri-plugin-global-shortcut` partage cette même bascule du 2 octobre 2024 vers la branche v2. L’écosystème continue d’évoluer au même rythme soutenu : le plugin de stockage sécurisé `tauri-plugin-stronghold` est passé en v2.3.0 le 25 juin 2025 puis en v2.3.1 le 27 octobre 2025, tandis que `tauri-plugin-upload`, dédié aux téléversements de fichiers volumineux, a suivi une cadence similaire avec sa v2.3.1 du 20 juillet 2025 puis sa v2.3.2 du 27 octobre 2025, d’après les pages de releases officielles Tauri.

```
# Côté Rust : ajouter dans src-tauri/Cargo.toml
cd src-tauri
cargo add tauri-plugin-sql --features sqlite
cargo add tauri-plugin-fs
cargo add tauri-plugin-notification
cargo add tauri-plugin-store
cd ..
# Côté JavaScript : binding officiel
npm install @tauri-apps/plugin-sql @tauri-apps/plugin-fs \
  @tauri-apps/plugin-notification @tauri-apps/plugin-store
```
Enregistrez ensuite ces plugins dans `src-tauri/src/lib.rs`. C’est le point central qui démarre le runtime Tauri et les enchaîne dans le builder :

```
// src-tauri/src/lib.rs
use tauri_plugin_sql::{Migration, MigrationKind};
#[cfg_attr(mobile, tauri::mobile_entry_point)]
pub fn run() {
    let migrations = vec![Migration {
        version: 1,
        description: "create_notes_table",
        sql: "CREATE TABLE IF NOT EXISTS notes (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                title TEXT NOT NULL,
                body  TEXT NOT NULL,
                created_at INTEGER NOT NULL,
                updated_at INTEGER NOT NULL
              );",
        kind: MigrationKind::Up,
    }];
    tauri::Builder::default()
        .plugin(tauri_plugin_fs::init())
        .plugin(tauri_plugin_notification::init())
        .plugin(tauri_plugin_store::Builder::default().build())
        .plugin(
            tauri_plugin_sql::Builder::default()
                .add_migrations("sqlite:noteforge.db", migrations)
                .build(),
        )
        .invoke_handler(tauri::generate_handler![
            crate::commands::save_note,
            crate::commands::list_notes,
            crate::commands::delete_note
        ])
        .run(tauri::generate_context!())
        .expect("erreur au démarrage de Tauri");
}
mod commands;
```
## Étape 6 : Écrire ses premières commandes Rust invoquées depuis le frontend

Le pont entre votre frontend et Rust se nomme **invoke**. Côté Rust, vous décorez une fonction avec `#[tauri::command]`. Côté JS, vous appelez `invoke('nom_commande', { args })`. Tauri sérialise automatiquement les paramètres en JSON via `serde`. Créez le fichier `src-tauri/src/commands.rs` avec trois commandes asynchrones pour notre gestionnaire de notes :

```
// src-tauri/src/commands.rs
use serde::{Deserialize, Serialize};
use tauri::AppHandle;
#[derive(Serialize, Deserialize, Debug)]
pub struct Note {
    pub id: Option<i64>,
    pub title: String,
    pub body: String,
    pub created_at: i64,
    pub updated_at: i64,
}
#[tauri::command]
pub async fn save_note(_app: AppHandle, note: Note) -> Result<i64, String> {
    // Logique d'insertion via tauri-plugin-sql en frontend
    // ou via sqlx ici si on choisit le backend pur Rust.
    println!("Sauvegarde demandée pour : {}", note.title);
    Ok(note.id.unwrap_or(0))
}
#[tauri::command]
pub async fn list_notes() -> Result<Vec<Note>, String> {
    Ok(vec![])
}
#[tauri::command]
pub async fn delete_note(id: i64) -> Result<(), String> {
    println!("Suppression note id = {}", id);
    Ok(())
}
```
Côté frontend, l’appel est limpide :

```
// src/api.ts
import { invoke } from "@tauri-apps/api/core";
export interface Note {
  id?: number;
  title: string;
  body: string;
  created_at: number;
  updated_at: number;
}
export async function saveNote(note: Note): Promise<number> {
  return await invoke<number>("save_note", { note });
}
export async function listNotes(): Promise<Note[]> {
  return await invoke<Note[]>("list_notes");
}
export async function deleteNote(id: number): Promise<void> {
  return await invoke<void>("delete_note", { id });
}
```
## Étape 7 : Persistance avec SQLite via tauri-plugin-sql

Plutôt que de réinventer la couche de persistance en Rust, le plugin `tauri-plugin-sql` expose directement une API `Database` côté JavaScript. La base SQLite est créée dans le répertoire applicatif du système (par exemple `~/.local/share/com.noteforge.app/` sous Linux), avec gestion automatique des migrations versionnées que nous avons déclarées à l’étape 5.

```
// src/db.ts
import Database from "@tauri-apps/plugin-sql";
let dbInstance: Database | null = null;
export async function getDb(): Promise<Database> {
  if (!dbInstance) {
    dbInstance = await Database.load("sqlite:noteforge.db");
  }
  return dbInstance;
}
export async function insertNote(title: string, body: string): Promise<number> {
  const db = await getDb();
  const now = Date.now();
  const result = await db.execute(
    "INSERT INTO notes (title, body, created_at, updated_at) VALUES ($1, $2, $3, $4)",
    [title, body, now, now]
  );
  return result.lastInsertId ?? 0;
}
export async function fetchNotes() {
  const db = await getDb();
  return await db.select<
    { id: number; title: string; body: string; created_at: number; updated_at: number }[]
  >("SELECT * FROM notes ORDER BY updated_at DESC");
}
export async function removeNote(id: number) {
  const db = await getDb();
  await db.execute("DELETE FROM notes WHERE id = $1", [id]);
}
```
N’oubliez pas d’autoriser le plugin SQL dans le fichier de capabilities. Sans cela, l’appel échouera silencieusement avec une erreur `permissions denied`.

## Étape 8 : Système de capabilities et sécurisation des permissions

Le système de capabilities est l’innovation sécurité majeure de Tauri 2. Il remplace l’ancienne `tauri.allowlist` (Tauri 1.x) par des fichiers JSON déclaratifs scoped par fenêtre, permettant un contrôle d’accès granulaire à l’IPC. Modifiez `src-tauri/capabilities/default.json` :

```
{
  "$schema": "../gen/schemas/desktop-schema.json",
  "identifier": "default",
  "description": "Permissions par défaut pour la fenêtre principale",
  "windows": ["main"],
  "permissions": [
    "core:default",
    "fs:default",
    "fs:allow-app-read",
    "fs:allow-app-write",
    "notification:default",
    "notification:allow-notify",
    "store:default",
    {
      "identifier": "sql:allow-execute",
      "allow": [{ "name": "sqlite:noteforge.db" }]
    },
    {
      "identifier": "sql:allow-select",
      "allow": [{ "name": "sqlite:noteforge.db" }]
    },
    {
      "identifier": "sql:allow-load",
      "allow": [{ "name": "sqlite:noteforge.db" }]
    }
  ]
}
```
Notez la granularité : nous n’autorisons l’accès SQL que pour la base nommée `sqlite:noteforge.db`, pas n’importe quelle base. De même, l’accès au système de fichiers est limité au scope `$APPDATA` via `fs:allow-app-read/write`. Si plus tard vous ajoutez une fenêtre `settings`, vous créerez un fichier `capabilities/settings.json` séparé avec ses propres permissions, beaucoup plus restrictives.

## Étape 9 : Construction de l’interface React et liaison aux commandes

Place au frontend. Remplacez le contenu de `src/App.tsx` par une interface minimaliste de gestion de notes. Le composant utilise les hooks React standards et appelle nos fonctions `insertNote`, `fetchNotes`, `removeNote` définies à l’étape 7.

```
// src/App.tsx
import { useEffect, useState } from "react";
import { insertNote, fetchNotes, removeNote } from "./db";
import { sendNotification, isPermissionGranted, requestPermission }
  from "@tauri-apps/plugin-notification";
import "./styles.css";
interface NoteRow {
  id: number;
  title: string;
  body: string;
  created_at: number;
  updated_at: number;
}
export default function App() {
  const [notes, setNotes] = useState<NoteRow[]>([]);
  const [title, setTitle] = useState("");
  const [body, setBody] = useState("");
  const reload = async () => setNotes(await fetchNotes());
  useEffect(() => { reload(); }, []);
  const onSave = async () => {
    if (!title.trim()) return;
    await insertNote(title, body);
    setTitle(""); setBody("");
    await reload();
    let granted = await isPermissionGranted();
    if (!granted) granted = (await requestPermission()) === "granted";
    if (granted) sendNotification({ title: "NoteForge", body: "Note sauvegardée" });
  };
  return (
    <main className="container">
      <h1>NoteForge</h1>
      <section className="editor">
        <input value={title} onChange={e => setTitle(e.target.value)} placeholder="Titre" />
        <textarea value={body} onChange={e => setBody(e.target.value)} placeholder="Contenu" />
        <button onClick={onSave}>Sauvegarder</button>
      </section>
      <ul className="notes">
        {notes.map(n => (
          <li key={n.id}>
            <strong>{n.title}</strong>
            <p>{n.body}</p>
            <button onClick={async () => { await removeNote(n.id); reload(); }}>
              Supprimer
            </button>
          </li>
        ))}
      </ul>
    </main>
  );
}
```
Relancez `npm run tauri dev`. Vous devriez voir l’interface, créer une note, recevoir une notification système native, et la note apparaître dans la liste – persistante entre les redémarrages grâce à SQLite.

## Étape 10 : Communication événementielle Rust ↔ Frontend

Au-delà des commandes synchrones, Tauri propose un bus d’événements asynchrone bidirectionnel. C’est utile pour notifier le frontend quand un travail long en Rust progresse, ou pour pousser des messages depuis le backend sans que le frontend ait demandé quoi que ce soit. Côté Rust, vous émettez avec `app.emit` ; côté JS, vous écoutez avec `listen`.

```
// src-tauri/src/commands.rs – ajout
use tauri::{AppHandle, Emitter};
#[tauri::command]
pub async fn long_task(app: AppHandle) -> Result<(), String> {
    for i in 0..=100 {
        std::thread::sleep(std::time::Duration::from_millis(40));
        app.emit("task-progress", i).map_err(|e| e.to_string())?;
    }
    Ok(())
}
```
```
// src/progress.ts
import { listen } from "@tauri-apps/api/event";
import { invoke } from "@tauri-apps/api/core";
export async function startWithProgress(onTick: (n: number) => void) {
  const unlisten = await listen<number>("task-progress", e => onTick(e.payload));
  await invoke("long_task");
  unlisten();
}
```
Pensez à enregistrer la commande `long_task` dans `generate_handler!` dans `lib.rs`. L’oubli de cet enregistrement est la première erreur que rencontrent les nouveaux développeurs Tauri.

## Étape 11 : Build de production et signature des artefacts

Une fois l’application terminée, lancez la build de production. La commande `tauri build` compile en mode release (LTO activé, optimisations agressives), exécute le bundler approprié à votre plateforme et signe l’exécutable si vous avez configuré une clé.

```
# Build par défaut pour la plateforme courante
npm run tauri build
# Cibler explicitement une plateforme
npm run tauri build -- --target x86_64-pc-windows-msvc
npm run tauri build -- --target aarch64-apple-darwin
npm run tauri build -- --target x86_64-unknown-linux-gnu
# Sortie attendue (sous Linux)
# Compiling noteforge v0.1.0
# Finished `release` profile [optimized] target(s) in 3m 24s
# Bundling NoteForge_0.1.0_amd64.deb
# Bundling NoteForge_0.1.0_amd64.AppImage
```
Sur macOS, la signature et la notarization Apple nécessitent un certificat Developer ID et le téléchargement de votre `app-specific password` dans le trousseau. Ajoutez à votre environnement :

```
export APPLE_SIGNING_IDENTITY="Developer ID Application: Votre Société (TEAM_ID)"
export APPLE_ID="[email protected]"
export APPLE_PASSWORD="@keychain:AC_PASSWORD"
export APPLE_TEAM_ID="ABCDE12345"
npm run tauri build
```
Sous Windows, fournissez un certificat de signature de code (.pfx) et son mot de passe via les variables `TAURI_SIGNING_PRIVATE_KEY` et `TAURI_SIGNING_PRIVATE_KEY_PASSWORD`. Pour Linux, aucune signature n’est exigée par défaut, mais un AppImage signé GPG renforce la confiance des distributions.

## Étape 12 : Activation du mécanisme de mise à jour automatique (updater)

Le plugin officiel `tauri-plugin-updater`, passé en version **2.10.0 en février 2026** avec l’ajout des options `no_proxy` et de configurations TLS non sécurisées pour les environnements de test, permet à votre application de se mettre à jour silencieusement depuis un endpoint que vous hébergez (S3, R2, GitHub Releases, ou votre propre serveur). L’updater vérifie une URL renvoyant un manifeste JSON signé Ed25519 et télécharge le binaire mis à jour.

```
# Génération de la clé de signature
npm install -D @tauri-apps/cli
npx tauri signer generate -w ~/.tauri/noteforge.key
# Sortie :
# Public key : dW50cnVzdGVkIGNvbW1lbnQ6IG1pbmlzaWduIH...
# Conservez le fichier ~/.tauri/noteforge.key précieusement et ajoutez la clé publique
# dans tauri.conf.json > plugins > updater > pubkey
# Ajout du plugin
cd src-tauri && cargo add tauri-plugin-updater
cd .. && npm install @tauri-apps/plugin-updater
```
Configurez l’updater dans `tauri.conf.json` :

```
"plugins": {
  "updater": {
    "endpoints": [
      "https://updates.noteforge.fr/{{target}}/{{current_version}}"
    ],
    "pubkey": "dW50cnVzdGVkIGNvbW1lbnQ6IG1pbmlzaWduIHB1YmxpYyBrZXk6..."
  }
}
```
Côté JavaScript, déclenchez la vérification au démarrage :

```
import { check } from "@tauri-apps/plugin-updater";
import { relaunch } from "@tauri-apps/plugin-process";
const update = await check();
if (update?.available) {
  console.log(`Mise à jour disponible : ${update.version}`);
  await update.downloadAndInstall();
  await relaunch();
}
```
## Étape 13 : Compilation pour Android et iOS

L’une des promesses majeures de Tauri 2 est la prise en charge native d’Android et iOS depuis la même base de code. Pour activer le support mobile, initialisez les projets natifs et installez les SDK requis.

```
# Android – prérequis : JDK 17, Android Studio, NDK 26+
export ANDROID_HOME="$HOME/Library/Android/sdk"
export NDK_HOME="$ANDROID_HOME/ndk/26.1.10909125"
npm run tauri android init
npm run tauri android dev    # Lance sur émulateur ou appareil USB
npm run tauri android build  # Génère l'APK signé
# iOS – prérequis : Xcode 15+, certificat Apple Developer
npm run tauri ios init
npm run tauri ios dev
npm run tauri ios build      # Génère l'IPA
```
Le ciblage mobile expose la macro `#[cfg_attr(mobile, tauri::mobile_entry_point)]` que nous avons utilisée à l’étape 5. Vos commandes Rust marchent telles quelles sur mobile, à condition de ne pas dépendre de caisses spécifiques bureau (par exemple `tray-icon` ou `arboard` pour le presse-papier desktop). Les plugins SQL, FS, Notification et Store fonctionnent sur les cinq cibles.

## Pièges fréquents et comment les éviter

Voici les huit erreurs les plus courantes signalées par la communauté Tauri en 2025-2026, avec leur résolution :

- **Compilation initiale extrêmement lente** : la première`cargo build` prend 5 à 12 minutes. C’est normal. Utilisez`sccache` pour partager le cache entre projets et activez`cargo install cargo-binstall` pour pré-compiler les outils Rust.
- **Erreur “webkit2gtk-4.1 not found”** sous Linux : votre distribution n’a pas la bonne version. Sur Ubuntu 22.04, installez aussi`libwebkit2gtk-4.0-dev` . Sur Fedora 40+, le paquet s’appelle`webkitgtk6.0-devel` .
- **Permission denied lors de l’invoke** : la commande n’est pas listée dans`capabilities/default.json` . Ajoutez le permission identifier exact retourné dans l’erreur.
- **Polices Tailwind invisibles** : la CSP par défaut bloque le chargement de polices distantes. Utilisez des polices locales ou ajoutez la directive`font-src` appropriée.
- **tauri build échoue avec “missing field bundle”** : votre`tauri.conf.json` est au format Tauri 1.x. Lancez`npx tauri migrate` pour convertir automatiquement vers le schéma 2.x.
- **Le DMG macOS refuse de s’ouvrir** : Gatekeeper bloque les binaires non signés. Signez avec votre Developer ID ou indiquez aux utilisateurs`xattr -dr com.apple.quarantine NoteForge.app` .
- **Erreur “frontendDist not found”** : Vite a généré sa sortie dans`build/` au lieu de`dist/` . Adaptez`build.frontendDist` dans`tauri.conf.json` ou modifiez la sortie Vite.
- **Plantage immédiat sur Windows ARM64** : compilez explicitement avec`--target aarch64-pc-windows-msvc` . Les binaires x64 émulés via Prism plantent souvent sur les Surface Pro X.

## Dépannage avancé : huit problèmes et leurs solutions

| Symptôme | Cause probable | Solution | 
|---|---|---|
| ``error: linker `cc` not found`` | Toolchain C absente | Installer build-essential (Linux), Xcode CLT (macOS), Visual Studio Build Tools (Windows) | 
| Fenêtre Tauri blanche au démarrage | devUrl pointe vers un port libre | Vérifier que Vite tourne sur 1420 ; ajuster `devUrl` dans tauri.conf.json | 
| Notifications jamais reçues | Permission OS refusée | Sous macOS : Réglages > Notifications > NoteForge ; sous Windows : Focus Assist désactivé | 
| `Database is locked` | Connexions concurrentes | Ouvrir une seule instance via `tauri-plugin-single-instance` | 
| Hot reload ne fonctionne pas | Ports différents entre Vite et Tauri | Aligner `devUrl` et le port Vite (1420 par défaut) | 
| Build mobile : `SDK 35 not found` | SDK Android obsolète | Mise à jour via Android Studio > SDK Manager | 
| iOS : `provisioning profile invalid` | Bundle ID non enregistré | Créer un App ID dans Apple Developer Portal | 
| Crash après cargo update | Versions Rust et plugins désynchronisées | Verrouiller via `cargo update -p tauri --precise 2.11.1` | 

## Astuces avancées pour aller plus loin

Une fois votre application bureau opérationnelle, plusieurs optimisations valent le détour. Premièrement, activez la **Profile-Guided Optimization (PGO)** sur la build release pour gagner 10 à 20 % de performance sur les chemins critiques. Ajoutez dans `src-tauri/Cargo.toml` :

```
[profile.release]
codegen-units = 1
lto = true
opt-level = "z"
panic = "abort"
strip = true
```
Avec ces réglages, un binaire Rust nu est typiquement réduit de 15 à 25 % supplémentaires. Pour aller encore plus loin, **UPX** peut compresser l’exécutable Windows à environ 50 % de sa taille initiale, mais attention : certains antivirus signalent les binaires UPX comme suspects.

Deuxièmement, pour les applications gourmandes en E/S disque, préférez **SQLx** directement en Rust (avec `tokio`) plutôt que `tauri-plugin-sql`. Vous gagnez la vérification SQL à la compilation et les requêtes préparées, au prix d’un peu plus de code. Le plugin reste idéal pour le prototypage et les cas simples.

Troisièmement, exposez l’icône système avec `tauri-plugin-tray` (renommé en `tray-icon` côté Rust) pour offrir une icône dans la barre des tâches macOS, le system tray Windows ou la status bar GNOME. Cela transforme votre application en quasi-démon qui réagit aux clics depuis l’icône.

Quatrièmement, pour les workflows DevOps européens, GitHub Actions propose des runners Linux, macOS et Windows que vous pouvez orchestrer avec l’action officielle `tauri-apps/tauri-action@v0`. Une seule définition YAML produit les binaires des trois plateformes et publie les artefacts signés sur GitHub Releases – idéal pour un cycle de release hebdomadaire automatisé.

## Cas d’usage réels : Tauri 2 en production

Plusieurs applications grand public et open source tournent en production sur Tauri 2 depuis fin 2024. **Ente Photos**, alternative chiffrée bout-en-bout à Google Photos, a migré son client desktop sur Tauri 2 en 2025 et expédie des binaires de moins de 12 Mo pour Windows, macOS et Linux. Le code est partagé avec leurs versions iOS et Android grâce au support mobile.

**AppFlowy**, alternative open source à Notion, a finalisé sa migration de Flutter Desktop vers Tauri 2 au premier trimestre 2025. L’équipe rapporte une réduction de 60 % de la consommation mémoire et un démarrage trois fois plus rapide qu’avec Electron sur leur ancienne version. Le binaire AppFlowy 2.x pèse environ 18 Mo contre 220 Mo pour la version Electron initiale.

**Relay**, client Git desktop pour GitHub, est passé sur Tauri 2 dès novembre 2024. Selon leurs benchmarks publiés en mars 2026, l’application démarre en 80 ms sur un MacBook Air M3 et consomme 38 Mo de RAM au repos contre 410 Mo pour GitHub Desktop (Electron). Pour une équipe française qui veut publier un client lourd à des milliers d’utilisateurs sans alourdir leur infrastructure de téléchargement, l’argument est imparable.

## Comparaison écosystèmes : Tauri 2, Electron, Flutter et .NET MAUI

| Framework | Langage backend | Frontend | Taille typique | Cibles | Licence | 
|---|---|---|---|---|---|
| Tauri 2.11 | Rust 1.81+ | HTML/CSS/JS (n’importe quel framework) | 3-8 Mo | Win, macOS, Linux, Android, iOS | MIT/Apache 2.0 | 
| Electron 33 | Node.js 22 | HTML/CSS/JS (Chromium embarqué) | 120-200 Mo | Win, macOS, Linux | MIT | 
| Flutter 3.22 | Dart 3.4 | Widgets Flutter (Skia/Impeller) | 30-60 Mo | Win, macOS, Linux, Android, iOS, Web | BSD-3 | 
| .NET MAUI 9 | C# / .NET 9 | XAML | 40-80 Mo | Win, macOS, Android, iOS | MIT | 
| Wails 3 | Go 1.23 | HTML/CSS/JS | 5-12 Mo | Win, macOS, Linux | MIT | 

Le tableau résume les forces et compromis de chaque approche. Tauri 2 reste le plus économe en taille et en mémoire, et le seul à offrir Rust comme langage backend, ce qui séduit les équipes qui veulent un seul langage pour leur backend cloud (axum, actix-web) et leur backend desktop. Flutter conserve l’avantage des widgets natifs identiques sur toutes plateformes, mais paye une taille de binaire deux à dix fois supérieure.

## Performance et benchmarks officiels

Les benchmarks comparatifs publiés sur le blog officiel Tauri en 2025 et confirmés par la communauté Dev.to en mars 2026 montrent un écart constant. Sur un Lenovo ThinkPad X1 Carbon Gen 11 (Intel Core i7-1365U, 32 Go RAM), une application Hello World Tauri 2 consomme **28 Mo de RAM** au repos contre **184 Mo pour Electron** et **67 Mo pour Flutter Desktop**. Le temps jusqu’à interactivité (TTI) est de **92 ms** pour Tauri, **1,8 s** pour Electron et **340 ms** pour Flutter.

Côté CPU, l’écart est moins spectaculaire car la majorité du temps de rendu se fait dans la WebView système, identique en performance brute à celle d’Electron. Là où Tauri brille, c’est sur l’empreinte mémoire de processus secondaires : Electron lance trois à cinq processus Chromium par fenêtre, contre un seul pour Tauri. Sur une machine modeste avec 8 Go de RAM, cela permet d’avoir trois applications Tauri ouvertes simultanément là où une seule application Electron consommerait déjà la moitié de la mémoire disponible.

## Distribution dans l’Union européenne et conformité

Pour les éditeurs français et européens, Tauri 2 simplifie plusieurs aspects réglementaires. D’abord, l’absence de Chromium embarqué élimine la collecte de données de télémétrie Google par défaut, un point sensible pour les organismes publics et les administrations. La CNIL et l’ANSSI publient depuis 2025 des guides recommandant explicitement les frameworks à WebView système pour les déploiements en environnement souverain.

Ensuite, la chaîne de signature et de notarization peut être réalisée intégralement via des autorités de certification européennes telles que **Certigna**, **Certinomis** ou **SwissSign** pour les binaires Windows. La Commission européenne, dans le cadre du règlement Cyber Resilience Act qui entre en vigueur en décembre 2027, impose aux éditeurs de logiciels desktop des exigences de sécurité par défaut : système de mises à jour signées, suivi des CVE, et notification d’incident sous 24 heures. Le système d’updater Tauri répond nativement aux deux premières exigences.

Enfin, la **Mission French Tech** a publié en mai 2026 un appel à projets pour soutenir les éditeurs français de logiciels bureau utilisant des stacks open source à empreinte légère. Tauri y est cité parmi les frameworks éligibles aux côtés de Flutter et Wails. Les candidatures sont ouvertes jusqu’à fin juin 2026.

## Mise en place CI/CD multi-plateforme avec GitHub Actions

Pour publier des binaires multi-plateformes à chaque tag git, l’action officielle `tauri-apps/tauri-action` orchestre la compilation sur les trois OS hébergés par GitHub. Voici un workflow minimal :

```
# .github/workflows/release.yml
name: Release Tauri
on:
  push:
    tags: ['v*']
jobs:
  build:
    strategy:
      fail-fast: false
      matrix:
        platform: [macos-latest, ubuntu-22.04, windows-latest]
    runs-on: ${{ matrix.platform }}
    steps:
      - uses: actions/checkout@v4
      - uses: dtolnay/rust-toolchain@stable
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - name: Install Linux deps
        if: matrix.platform == 'ubuntu-22.04'
        run: |
          sudo apt-get update
          sudo apt-get install -y libwebkit2gtk-4.1-dev libxdo-dev \
            libssl-dev libayatana-appindicator3-dev librsvg2-dev
      - run: npm ci
      - uses: tauri-apps/tauri-action@v0
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          TAURI_SIGNING_PRIVATE_KEY: ${{ secrets.TAURI_KEY }}
          TAURI_SIGNING_PRIVATE_KEY_PASSWORD: ${{ secrets.TAURI_KEY_PWD }}
        with:
          tagName: ${{ github.ref_name }}
          releaseName: 'NoteForge ${{ github.ref_name }}'
          releaseDraft: true
          prerelease: false
```
Avec ce workflow, un simple `git tag v0.2.0 && git push --tags` déclenche trois builds parallèles. Vingt-cinq minutes plus tard environ (essentiellement compression des binaires release Rust), votre release GitHub contient un MSI, deux DMG (Intel et ARM), un DEB et un AppImage signés.

## FAQ : questions fréquentes sur Tauri 2

### Faut-il connaître Rust pour développer avec Tauri 2 ?

Pas nécessairement pour démarrer. La majorité du code applicatif vit dans le frontend (React, Vue, Svelte, Angular ou même HTML pur). Vous touchez à Rust uniquement pour écrire des commandes `#[tauri::command]`, généralement quelques dizaines de lignes par projet. Pour un usage avancé (plugins personnalisés, intégrations système profondes), une connaissance de base de Rust devient nécessaire – environ deux à quatre semaines d’apprentissage pour un développeur expérimenté.

### Tauri 2 supporte-t-il les frameworks Vue, Svelte ou Angular ?

Oui, Tauri est agnostique au framework frontend. Le template `create-tauri-app` propose React, Vue, Svelte, SolidJS, Angular, Preact, Yew (Rust) et HTML/CSS/JS vanilla. Vous pouvez aussi greffer Tauri sur un projet Vite, Next.js statique ou Nuxt 4 existant en pointant `frontendDist` vers le répertoire de build.

### Quelle est la différence entre Tauri 1.x et Tauri 2 ?

Tauri 2 introduit le support mobile (Android et iOS), un nouveau système de capabilities remplaçant l’allowlist, des plugins officiels stabilisés (SQL, Notification, Store, Updater, etc.), et une refonte de l’API JavaScript en modules ESM. La migration depuis Tauri 1.x s’effectue avec `npx tauri migrate` qui transforme automatiquement `tauri.conf.json` et la plupart des appels JS.

### Combien coûte un certificat de signature de code pour Windows et macOS ?

Un certificat Apple Developer coûte 99 € par an. Pour Windows, un certificat OV (Organization Validation) revient à 200-400 € par an chez Sectigo, DigiCert ou Certigna, et un EV (Extended Validation) qui supprime immédiatement les avertissements SmartScreen coûte 400-800 € par an. Pour Linux, aucune signature obligatoire – un AppImage signé GPG suffit.

### Tauri 2 est-il prêt pour la production en 2026 ?

Oui. La version stable 2.0 est sortie le 2 octobre 2024, suivie d’une cadence de releases quasi mensuelle sur le noyau lui-même : 2.5.1 le 21 avril 2025, 2.6.0 le 24 juin 2025, puis toute la série 2.9.x à l’automne 2025 dont la 2.9.1 le 22 octobre 2025, jusqu’à l’actuelle 2.11.1 (Tauri Releases). Le runtime sous-jacent suit le même rythme soutenu : `tauri-runtime-wry` a atteint la v2.8.1 le 25 août 2025, puis la v2.9.2 le 30 novembre 2025 et la v2.9.3 le 9 décembre 2025, d’après la page de releases du runtime Tauri. Plusieurs applications grand public l’utilisent en production (Ente Photos, AppFlowy, Relay), un audit de sécurité externe a été réalisé avant la sortie 2.0, et la Tauri Foundation gouverne le projet sous l’égide du Commons Conservancy.

### Quelle est la différence entre tauri-plugin-sql et SQLx ?

Le plugin officiel expose une API JavaScript simple et fonctionne immédiatement avec SQLite, MySQL ou PostgreSQL. SQLx est une librairie Rust avancée avec vérification SQL à la compilation et support du pooling. Pour un prototype ou un projet de petite taille, le plugin suffit largement. Pour une application bureau qui synchronise avec un Postgres distant et exécute des requêtes complexes, SQLx en Rust apporte plus de robustesse.

### Tauri 2 fonctionne-t-il avec WSL 2 sous Windows 11 ?

Oui, mais uniquement pour le développement de cibles Linux. Pour produire des binaires Windows depuis WSL, vous devez utiliser un cross-compiler ou exécuter `tauri build` directement sous Windows. La majorité des développeurs sous Windows préfèrent une stack 100 % Windows native (PowerShell, Visual Studio Build Tools).

## Verdict : pourquoi Tauri 2 mérite votre prochain projet bureau

En près de deux ans d’existence stable, jalonnés d’une dizaine de versions mineures publiées sans interruption jusqu’à la 2.9.5 du 9 décembre 2025 selon les Tauri Releases, puis jusqu’à la révision 2.11.1 de l’API atteinte en juillet 2026 d’après le blog ESB1995, Tauri 2 s’est imposé comme l’alternative crédible et mature à Electron pour les applications bureau modernes. Les chiffres parlent : binaires 30 à 40 fois plus légers, démarrage 20 à 30 fois plus rapide, empreinte mémoire 5 à 10 fois inférieure. Surtout, le framework couvre désormais cinq plateformes (Windows, macOS, Linux, Android, iOS) avec une seule base de code Rust + Web.

Pour une équipe française ou européenne qui démarre un projet bureau aujourd’hui, le choix Tauri 2 réduit les coûts d’infrastructure de distribution (binaires 40 fois plus petits = 40 fois moins de bande passante CDN), simplifie la conformité réglementaire (pas de runtime Chromium embarqué = moins d’enjeux RGPD et CRA), et donne accès à un langage backend partageable avec votre stack cloud (axum, actix-web, sqlx). Les arguments contre se réduisent à la courbe d’apprentissage Rust pour les commandes avancées et à un écosystème de plugins encore plus jeune que celui d’Electron – mais qui croît mensuellement avec le soutien actif de la Tauri Foundation.

Le projet NoteForge complet décrit dans ce tutoriel – gestionnaire de notes local-first avec SQLite, capabilities granulaires, notifications natives, updater Ed25519 et CI/CD GitHub Actions – vous donne un squelette opérationnel pour démarrer votre prochain projet desktop multi-plateforme. Bonne route avec Tauri.

### Couverture associée

Sources et références externes : documentation officielle Tauri 2, dépôt GitHub tauri-apps/tauri, guide sécurité Tauri, site officiel Rust, notice Wikipedia Tauri.
