# Guide Go (Golang) — du script sysadmin au service de production

> Pour Zelef — chef de service systèmes & énergies, sysadmin.
> Angle : pratique, outillage, CLI, automation, déploiement simple.
> Go 1.21+ (certaines syntaxes notées `≥ 1.22`, `≥ 1.23`, etc. le cas échéant).
> Ce guide est dense : tutoriel + référence. Tout le code a été relu pour une syntaxe correcte.

**Plan rapide** : installation → syntaxe → types → structs/interfaces → erreurs →
modules → fichiers → concurrence (goroutines/channels) → réseau/HTTP →
tests → CLI → déploiement → cas pratiques sysadmin → pense-bête → glossaire → quiz.

---

## 1. Pourquoi Go pour un sysadmin ?

| Besoin sysadmin | Réponse Go |
|---|---|
| Scripts d'automation fiables | Binaire compilé, erreurs explicites, pas d'interpréteur à installer |
| Outils CLI distribués aux équipes | Un seul binaire statique, compilation croisée native |
| Supervision / agents / exporters | Concurrence native (goroutines), faible empreinte mémoire |
| Remplacer des scripts shell fragiles | Typage statique, gestion d'erreurs obligatoire, tests intégrés |
| Services internes (API, webhooks) | `net/http` en bibliothèque standard, démarrage en ms |

Go ne remplace pas bash pour un one-liner. Il remplace les scripts de 200+ lignes
qui deviennent critiques, et les outils Python qui exigent un runtime sur chaque machine.

Points d'attention honnêtes :
- Gestion d'erreurs verbeuse (c'est un choix assumé du langage).
- Pas de génériques avant Go 1.18 ; écosystème plus petit que Python pour la data science.
- Le modèle de concurrence est simple mais exige de la discipline (voir sections 37–44).

## 2. Installation sur Linux (Debian/Ubuntu)

Méthode recommandée : archive officielle (les paquets distro sont souvent en retard).

```bash
# 1. Télécharger (vérifier la dernière version sur https://go.dev/dl/)
GO_VERSION="1.23.4"
wget "https://go.dev/dl/go${GO_VERSION}.linux-amd64.tar.gz"

# 2. (Optionnel) vérifier la somme SHA256 publiée sur go.dev/dl
# sha256sum go1.23.4.linux-amd64.tar.gz

# 3. Installer dans /usr/local (supprimer l'ancienne version d'abord)
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go${GO_VERSION}.linux-amd64.tar.gz"

# 4. Ajouter au PATH (persistant)
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
# Pour que "go install" dépose les binaires dans le PATH aussi :
echo 'export PATH=$PATH:$HOME/go/bin' >> ~/.bashrc
source ~/.bashrc

# 5. Vérifier
go version        # go version go1.23.4 linux/amd64
go env GOPATH GOROOT GOCACHE GOENV
```

> Sous Windows/macOS : installeur sur https://go.dev/dl/ ; sous Debian `apt install golang`
> existe mais fournit souvent une version ancienne — à éviter pour un usage sérieux.

Checklist post-installation :
- [ ] `go version` affiche la version attendue
- [ ] `$HOME/go/bin` est dans le PATH
- [ ] `go env GOPROXY` répond (proxy de modules, défaut `https://proxy.golang.org,direct`)
- [ ] Un `hello world` compile (section 3)

## 3. Premier programme : le rituel `hello`

```bash
mkdir -p ~/go/src/hello && cd ~/go/src/hello
go mod init hello          # crée go.mod (voir section 5)
```

`main.go` :
```go
package main

import "fmt"

func main() {
	fmt.Println("Hello, infra !")
}
```

```bash
go run .        # compile + exécute sans écrire de binaire
go build -o hello .   # produit le binaire ./hello
./hello
```

Règles à retenir dès le premier fichier :
- Le point d'entrée est **toujours** `func main()` dans `package main`.
- Un programme = un ou plusieurs fichiers `.go` du même package dans un dossier.
- `go run .` compile en mémoire ; `go build` produit un binaire réel.

## 4. La toolchain `go` : les 10 commandes à connaître

| Commande | Rôle |
|---|---|
| `go run .` | Compile et exécute (dev rapide) |
| `go build -o app .` | Produit le binaire |
| `go vet ./...` | Analyse statique (bugs probables) |
| `gofmt -l .` | Liste les fichiers mal formatés |
| `go test ./...` | Lance les tests |
| `go mod tidy` | Nettoie `go.mod`/`go.sum` |
| `go install tool@version` | Installe un outil (`≥ 1.16`, hors module courant) |
| `go list -m all` | Liste les dépendances |
| `go env -w GOPROXY=...` | Écrit la config persistante (`go env GOENV` pour le fichier) |
| `go doc fmt.Println` | Doc d'un symbole, sans quitter le terminal |

Workflow minimal sain avant chaque commit :
```bash
gofmt -l . && go vet ./... && go test ./...
```

## 5. Modules Go : `go mod`, le gestionnaire de dépendances

Depuis Go 1.16, les modules sont le mode par défaut. Oubliez `GOPATH/src` historique.

```bash
go mod init infra/outils          # nom = chemin du module (souvent URL du repo)
go get github.com/spf13/cobra@latest   # ajoute une dépendance
go mod tidy                      # ajoute ce qui manque, retire l'inutile
```

`go.mod` typique :
```go
module infra/outils

go 1.23

require github.com/spf13/cobra v1.8.1

require (
	github.com/spf13/pflag v1.0.5 // indirect
	golang.org/x/sys v0.20.0 // indirect
)
```

- `go.sum` = empreintes cryptographiques des dépendances (à versionner).
- Versionner `go.mod` **et** `go.sum` dans git. Ne jamais éditer `go.sum` à la main.
- `go mod vendor` : copie les dépendances dans `vendor/` (builds offline / air-gap).

Tableau des commandes modules :

| Commande | Effet |
|---|---|
| `go mod init <nom>` | Crée le module |
| `go get pkg@version` | Ajoute/met à jour (`@latest`, `@v1.2.3`, `@commit`) |
| `go get -u ./...` | Met à jour toutes les déps (avec prudence) |
| `go mod tidy` | Synchronise go.mod/go.sum avec le code |
| `go mod verify` | Vérifie l'intégrité du cache |
| `go mod why pkg` | Explique pourquoi une dépe
...[truncated 10263 chars]
---

---

## 6. Variables : déclaration, `:=`, portée

```go
var nom string = "web-01"   // forme longue
var port = 22               // type inféré
hote := "db-01"             // := déclare + infère (UNIQUEMENT dans une fonction)
var a, b int = 1, 2         // plusieurs
var (
	ip      string = "10.0.0.80"
	actif   bool   = true
	retries int           // 0 par défaut (zéro value, voir section 10)
)

const Timeout = 5           // constante : voir section 7

// Portée : minuscule = package, MAJUSCULE = exporté (voir section 30)
```

Règles :
- `:=` exige **au moins une variable nouvelle** à gauche : `x := 1; x, y := 2, 3` est légal.
- Variable déclarée et **jamais utilisée** = erreur de compilation (le compilateur est strict).
- Préférez `:=` en local, `var` au niveau package ou quand le zéro value suffit.

## 7. Constantes et `iota` : les énumérations Go

```go
const (
	SeuilCritique = 90
	SeuilAlerte   = 70
	Version       = "1.4.0"
)

// iota : compteur auto-incrémenté dans un bloc const
const (
	Lundi = iota // 0
	Mardi        // 1 (iota implicite)
	Mercredi     // 2
	_            // 3 : _ saute une valeur
	Vendredi     // 4
)

// Énumération de statuts typée (idiome)
type Statut int

const (
	StatutInconnu Statut = iota
	StatutOK
	StatutKO
	StatutCritique
)

func (s Statut) String() string {
	return []string{"inconnu", "ok", "ko", "critique"}[s]
}
```

Les constantes sont **non typées** par défaut (`const x = 42` s'adapte au contexte),
sauf si on précise le type. `iota` recommence à 0 dans chaque bloc `const`.

## 8. Types numériques : choisir la bonne taille

| Type | Taille | Usage |
|---|---|---|
| `int` / `uint` | 32 ou 64 bits selon plateforme | **Défaut** : boucles, index, compteurs |
| `int8`…`int64`, `uint8`…`uint64` | Fixe | Protocoles, binaire, perf mémoire |
| `float32` / `float64` | 32 / 64 bits | `float64` par défaut (mesures, ratios) |
| `byte` | alias `uint8` | Octets bruts |
| `rune` | alias `int32` | Point de code Unicode |
| `uintptr` | Pointeur non sûr | Rare : interfaçage bas niveau |

```go
var n int = 42
var f float64 = 3.14
var b byte = 'A'      // 65 : les quotes simples = rune/octet, pas string !
var s string = "A"    // doubles quotes = string

// Conversions EXPLICITES obligatoires (pas de promotion implicite)
var i int = 7
var f2 float64 = float64(i) / 2 // 3.5
// var f3 float64 = i / 2       // ERREUR : type mismatch
```

> Pas de conversion implicite en Go : `int` + `int64` ne compile pas. C'est
> verbeux mais ça élimine toute une classe de bugs silencieux.

## 9. Booléens et chaînes : l'essentiel

```go
var ok bool = true
// Les conditions exigent un booléen : pas de 0/1, pas de pointeur nil
// if port { ... }        // ERREUR : port n'est pas un bool
// if 1 { ... }           // ERREUR

var s string = "hello"
s2 := `chemin brut C:\logs\app.log` // backticks : string brute, sans échappement
multi := `ligne1
ligne2` // multi-lignes autorisées

// Concaténation simple (hors boucle chaude)
msg := "hôte " + hote + " KO"
```

## 10. Zéro values : le "constructeur gratuit"

Chaque type a une valeur zéro utilisable immédiatement — pas de constructeur obligatoire.

| Type | Zéro value |
|---|---|
| `int`, `float` | `0` |
| `string` | `""` |
| `bool` | `false` |
| Pointeur, slice, map, channel, fonction, interface | `nil` |

```go
var s Serveur        // struct zéro : champs à leurs zéro values
fmt.Println(s.Port)  // 0 — utilisable sans initialisation

var compteurs map[string]int // nil : LECTURE ok (retourne zéro), ÉCRITURE = panic
n := compteurs["x"]          // 0, pas de panic
// compteurs["x"] = 1        // PANIC : initialiser avec make d'abord

var liste []int      // slice nil : append fonctionne !
liste = append(liste, 1) // OK

// Idiome : profiter du zéro value pour les options
type Options struct {
	Timeout time.Duration // 0 = à interpréter comme défaut dans le code
	Retries int           // 0 = 1 tentative par défaut, à documenter
}
```

---

## 11. Opérateurs

| Famille | Opérateurs |
|---|---|
| Arithmétiques | `+ - * / %` (division entière si les deux opérandes sont entiers) |
| Affectation | `= += -= *= /= %= &= \|= ^= <<= >>=` |
| Comparaison | `== != < <= > >=` |
| Logiques | `&&` (et, court-circuit) `\|\|` (ou, court-circuit) `!` (non) |
| Bits | `&` (et) `\|` (ou) `^` (ou exclusif / NOT unaire) `&^` (ET-NON : clear) `<<` `>>` |

```go
a, b := 7, 3
fmt.Println(a / b)   // 2  (division entière !)
fmt.Println(a % b)   // 1

x := 0b1100           // littéral binaire (Go 1.13+)
fmt.Printf("%b %d\n", x&^0b0100, x) // clear du bit 2

// Pas d'opérateur ternaire en Go ! On écrit :
max := a
if b > a {
	max = b
}
```

Piège classique : `a / b` avec deux `int` tronque. Pour un quotient décimal,
convertir d'abord : `float64(a) / float64(b)`.

## 12. Contrôle : `if`, `switch`

```go
// if avec initialisation (portée limitée au bloc) — idiome très courant
if err := chargerConfig(); err != nil {
	log.Fatalf("config: %v", err)
}

if charge := cpuCharge(); charge > 90 {
	alert("CPU critique")
} else if charge > 70 {
	alert("CPU élevé")
}

// switch sans expression = switch true
switch {
case charge > 90:
	fmt.Println("critique")
case charge > 70:
	fmt.Println("élevé")
default:
	fmt.Println("ok")
}

// switch sur valeur : pas de 'break' nécessaire (pas de fallthrough implicite)
switch jour {
case "sam", "dim": // plusieurs valeurs par case
	fmt.Println("week-end")
case "lun":
	fmt.Println("lundi")
	fallthrough // explicite si vraiment voulu (rare)
default:
	fmt.Println("semaine")
}

// switch de type (voir section 29)
switch v := x.(type) {
case string:
	fmt.Println("string:", v)
case int:
	fmt.Println("int:", v)
}
```

## 13. Boucles : `for` fait tout (pas de `while`)

```go
// Style while
i := 0
for i < 5 {
	fmt.Println(i)
	i++
}

// Classique
for i := 0; i < 5; i++ {
	fmt.Println(i)
}

// Boucle infinie (serveurs, workers)
for {
	travail()
}

// Parcours avec range (index + valeur)
services := []string{"dns", "dhcp", "ntp"}
for i, s := range services {
	fmt.Printf("%d: %s\n", i, s)
}
for _, s := range services { // _ ignore l'index
	fmt.Println(s)
}

// Parcours d'une map (ordre ALÉATOIRE à chaque exécution)
for nom, ip := range hotes {
	fmt.Println(nom, ip)
}

// break / continue avec label (sortir de boucles imbriquées)
boucle:
	for _, h := range hotes {
		for _, p := range ports {
			if injoignable(h, p) {
				break boucle
			}
		}
	}
```

> `for range` sur un entier (`for i := range 10`) : possible depuis **Go 1.22**.
> Avant, écrire `for i := 0; i < 10; i++`.

## 14. Tableaux : taille fixe, rarement utilisés directement

```go
var codes [4]int              // [0 0 0 0], len = 4
jours := [7]string{"lun", "mar", "mer", "jeu", "ven", "sam", "dim"}
fmt.Println(len(jours))       // 7

// Les tableaux se copient par VALEUR à l'affectation et au passage en paramètre
a := [2]int{1, 2}
b := a
b[0] = 99
fmt.Println(a[0]) // 1 : a inchangé
```

En pratique on utilise des **slices** (section 15), pas des tableaux.
Retenez juste : un tableau `[N]T` a une taille qui fait partie de son type.

## 15. Slices en détail : la structure de données n°1

Un slice = vue sur un tableau sous-jacent : **pointeur + longueur + capacité**.

```go
// Créations
s1 := []int{1, 2, 3}          // littéral
s2 := make([]int, 3)          // len=3, cap=3, zéros
s3 := make([]int, 0, 10)      // len=0, cap=10 (pré-allocation !)
var s4 []int                 // slice nil : len=0, cap=0, utilisable directement

fmt.Println(s4 == nil)        // true — append fonctionne quand même

// Ajout (append peut réallouer : TOUJOURS réaffecter le résultat)
s3 = append(s3, 42)
s3 = append(s3, 1, 2, 3)      // plusieurs d'un coup
s3 = append(s3, s1...)        // ... déplie un slice

// Découpage (partage le tableau sous-jacent !)
t := []int{0, 1, 2, 3, 4, 5}
sous := t[1:4]                // [1 2 3], len=3, cap=5
sous[0] = 99
fmt.Println(t[1])             // 99 : modification visible via t !

// Copie indépendante
copie := make([]int, len(sous))
copy(copie, sous)

// Longueur / capacité
fmt.Println(len(t), cap(t))   // 6 6

// Parcours
for i, v := range t {
	fmt.Println(i, v)
}
```

Tableau récapitulatif des opérations :

| Opération | Code | Note |
|---|---|---|
| Créer vide pré-alloué | `make([]T, 0, n)` | Évite les réallocations en boucle |
| Ajouter | `s = append(s, v)` | Réaffecter impérativement |
| Copier | `copy(dst, src)` | Retourne le nb d'éléments copiés |
| Trier (`≥ 1.21` : paquet `slices`) | `slices.Sort(s)` | Ancien : `sort.Ints(s)` |
| Inverser/chercher | `slices.Reverse`, `slices.Contains`, `slices.Index` | Paquet `slices` (`≥ 1.21`) |
| Vider en gardant cap | `s = s[:0]` | Réutilise le tableau |
| Supprimer l'index i | `s = append(s[:i], s[i+1:]...)` | Idiome à connaître |
| Comparer | `slices.Equal(a, b)` | `==` interdit sur slices |

Pense-bête perf : si vous connaissez la taille finale (lecture de N lignes),
`make([]T, 0, n)` évite des dizaines de réallocations. Mesurez avec les
benchmarks (section 62) avant d'optimiser à l'aveugle.

## 16. Maps en détail : dictionnaires

```go
// Création
inventaire := map[string]string{
	"srv-dns-01": "10.0.0.53",
	"srv-ntp-01": "10.0.0.123",
}
compteurs := make(map[string]int) // pré-allouer si taille connue : make(map[string]int, 100)
var lazy map[string]int           // map nil : LECTURE ok, ÉCRITURE = panic !

// Lecture avec test d'existence (idiome "comma ok")
ip, ok := inventaire["srv-dns-01"]
if !ok {
	fmt.Println("hôte inconnu")
}

// Écriture / suppression
inventaire["srv-web-01"] = "10.0.0.80"
delete(inventaire, "srv-ntp-01")

fmt.Println(len(inventaire)) // nombre de clés

// Parcours : ordre aléatoire !
for nom, ip := range inventaire {
	fmt.Println(nom, "->", ip)
}
```

Points critiques :
- **Les maps ne sont pas thread-safe** : accès concurrents = `sync.Map` ou mutex (section 41).
- L'ordre d'itération est volontairement aléatoire (ne jamais en dépendre).
- Une map `nil` se lit sans paniquer (`v, ok := m[k]` → zéro + false) mais
  l'écriture panique : initialisez avec `make` ou un littéral.
- Clés : tout type **comparable** (`string`, `int`, struct sans slice...). Pas de slice en clé.

Exemple sysadmin : compter des occurrences (lignes de log par code) :
```go
codes := map[string]int{}
for _, ligne := range lignes {
	code := extraireCode(ligne)
	codes[code]++ // zéro implicite si absent : pas besoin de tester !
}
```

## 17. Chaînes, runes et bytes : l'UTF-8 sans douleur

```go
s := "héllo"                  // string = suite d'OCTETS, immuable, UTF-8 par convention
fmt.Println(len(s))           // 6 octets (é = 2 octets), pas 5 caractères !

for i, r := range s {         // range décode l'UTF-8 : r est une rune
	fmt.Printf("pos octet %d : %c (U+%04X)\n", i, r, r)
}

runes := []rune(s)            // conversion en runes pour compter les caractères
fmt.Println(len(runes))       // 5

// Concaténation en boucle : strings.Builder (pas += répété)
var b strings.Builder
for _, h := range hotes {
	b.WriteString(h)
	b.WriteByte('\n')
}
texte := b.String()

// Manipulations courantes
strings.Contains(s, "llo")
strings.HasPrefix(s, "hé")
strings.Split("a,b,c", ",")   // []string{"a","b","c"}
strings.Join([]string{"a","b"}, ",")
strings.TrimSpace("  x  ")
strings.ToUpper(s)
strings.ReplaceAll(s, "l", "L")
strconv.Itoa(42)              // int -> string
n, err := strconv.Atoi("42")  // string -> int
```

Règles d'or :
- Une `string` est immuable : toute "modification" crée une nouvelle chaîne.
- `len(s)` = octets. Pour des caractères : `len([]rune(s))` ou `utf8.RuneCountInString(s).
- En boucle intensive, `strings.Builder` bat la concaténation `+=` (allocations).

## 18. Pointeurs : juste ce qu'il faut

```go
x := 42
p := &x          // p pointe vers x
fmt.Println(*p)  // 42 : déréférencement
*p = 100
fmt.Println(x)   // 100

// Pointeur nil : le déréférencer = panic
var q *int
fmt.Println(q == nil) // true

// Passage par pointeur pour modifier ou éviter une grosse copie
func reset(c *Compteur) { c.valeur = 0 }
```

Ce que vous devez savoir, pas plus :
- `&v` prend l'adresse, `*p` accède à la valeur pointée.
- Go n'a **pas** d'arithmétique de pointeurs (contrairement à C).
- Les slices et maps sont déjà des références : pas besoin de `*[]int` en paramètre.
- Receveurs de méthodes : pointeur `*T` si la méthode modifie le struct (section 26).

## 19. Fonctions : valeurs de retour multiples, nommées

```go
// Deux valeurs de retour : l'idiome (résultat, erreur)
func diviser(a, b float64) (float64, error) {
	if b == 0 {
		return 0, errors.New("division par zéro")
	}
	return a / b, nil
}

// Retours nommés (utiles avec defer, voir section 22)
func ping(hote string) (latence time.Duration, err error) {
	latence = mesurer(hote) // assigné aux variables nommées
	return                 // "return nu" : renvoie latence, err
}

// Nombre variable d'arguments
func somme(nombres ...int) int {
	total := 0
	for _, n := range nombres {
		total += n
	}
	return total
}
somme(1, 2, 3)
somme(slice...) // déplier un slice

// Fonction anonyme + closure (capture les variables environnantes)
compteur := func() func() int {
	n := 0
	return func() int {
		n++
		return n
	}
}()
fmt.Println(compteur(), compteur()) // 1 2
```

## 20. `defer` : le nettoyage garanti

`func` différée : exécutée à la **fin** de la fonction appelante, en LIFO.

```go
func traiter(fichier string) error {
	f, err := os.Open(fichier)
	if err != nil {
		return err
	}
	defer f.Close() // garanti, même en cas de return anticipé ou panic

	// ... lecture ...
	return nil
}

// Ordre LIFO
func demo() {
	defer fmt.Println("1er différé, exécuté en dernier")
	defer fmt.Println("2e différé, exécuté en premier")
	fmt.Println("corps")
}
// Affiche : corps / 2e différé... / 1er différé...
```

Usages sysadmin typiques : `defer f.Close()`, `defer mu.Unlock()`,
`defer resp.Body.Close()` (HTTP client — **obligatoire** pour réutiliser les connexions).

> Astuce : les arguments de `defer` sont évalués **immédiatement**.
> `defer fmt.Println(i)` affiche la valeur de `i` au moment du `defer`, pas à la fin.

## 21. `panic` et `recover` : l'exception qui n'en est pas une

```go
func critiquer(n int) {
	if n < 0 {
		panic("négatif interdit") // arrête la goroutine (et le programme si non récupéré)
	}
}

func securise() {
	defer func() {
		if r := recover(); r != nil {
			fmt.Println("récupéré :", r)
		}
	}()
	critiquer(-1)
	fmt.Println("jamais atteint")
}
```

Doctrine Go :
- `panic` = bug du programmeur ou état irrécupérable (index hors limites, map nil en écriture).
- **Jamais** pour une erreur d'exécution normale (fichier absent, réseau coupé) → `error`.
- `recover` ne sert que dans un `defer`, typiquement aux frontières (serveur HTTP,
  goroutine worker) pour éviter qu'un panic ne tue tout le processus.
- La bibliothèque standard panique rarement ; votre code applicatif ne devrait
  quasiment jamais appeler `panic` directement.
---

## 22. Structs : vos objets métier

```go
type Serveur struct {
	Nom     string   // champ EXPORTÉ (majuscule) : visible hors package
	IP      string
	Port    int
	Tags    []string
	enLigne bool     // champ non exporté : privé au package
}

// Littéraux
s1 := Serveur{Nom: "dns-01", IP: "10.0.0.53", Port: 53} // nommé : recommandé
s2 := Serveur{"dns-01", "10.0.0.53", 53, nil, true}      // positionnel : fragile, à éviter

// Champ par champ
var s3 Serveur
s3.Nom = "ntp-01"

// Struct anonyme (config ponctuelle)
cfg := struct {
	Timeout int
	Retry   int
}{Timeout: 5, Retry: 3}
```

## 23. Méthodes : fonctions attachées à un type

```go
// Receveur valeur : ne modifie pas l'original (ok pour petits structs / lecture)
func (s Serveur) Adresse() string {
	return fmt.Sprintf("%s:%d", s.IP, s.Port)
}

// Receveur pointeur : modifie l'original, évite la copie
func (s *Serveur) MarquerEnLigne() {
	s.enLigne = true
}

s := Serveur{Nom: "dns-01", IP: "10.0.0.53", Port: 53}
fmt.Println(s.Adresse())
s.MarquerEnLigne() // Go prend &s automatiquement
```

Règle de cohérence : si **une** méthode d'un type utilise un receveur pointeur,
utilisez le pointeur pour **toutes** les méthodes du type (recommandation `go vet`
implicite via les linters).

## 24. Interfaces : le polymorphisme à la Go

Petites interfaces, définies **côté consommateur**. L'idiome : `io.Reader`, `io.Writer`.

```go
// Une interface = un contrat de méthodes
type Verifiable interface {
	Verifier() error
}

// Serveur IMPLÉMENTE Verifiable implicitement (pas de "implements")
func (s Serveur) Verifier() error {
	if s.IP == "" {
		return errors.New("IP manquante")
	}
	return nil
}

func controler(v Verifiable) error {
	return v.Verifier() // marche avec TOUT type ayant Verifier() error
}
```

Points clés :
- Implémentation **implicite** : aucun mot-clé, le compilateur vérifie.
- Préférez les petites interfaces (1-2 méthodes). `interface{}` / `any` = absence de contrat.
- `any` est un alias de `interface{}` (Go 1.18+). À utiliser avec parcimonie.
- L'interface vide accepte tout : utile pour le JSON générique, dangereuse partout ailleurs.

## 25. Composition (embedding) : l'alternative à l'héritage

Go n'a pas d'héritage. On compose :

```go
type Base struct {
	Nom string
}

func (b Base) Decrire() string { return "hôte: " + b.Nom }

type ServeurWeb struct {
	Base          // embarqué : ses champs/méthodes sont "promus"
	Port    int
}

w := ServeurWeb{Base: Base{Nom: "web-01"}, Port: 8080}
fmt.Println(w.Nom)      // promu : w.Base.Nom
fmt.Println(w.Decrire()) // méthode promue
```

C'est de la **délégation**, pas de l'héritage : pas de polymorphisme implicite
ni de redéfinition virtuelle. Pour le polymorphisme, voir les interfaces (24).

## 26. Assertions de type et `type switch`

```go
var v any = "hello"

// Assertion avec test (ne panique pas)
s, ok := v.(string)
if ok {
	fmt.Println("c'est une string:", s)
}

// Assertion directe : panique si le type ne correspond pas (à éviter hors certitude)
s2 := v.(string)

// Type switch : aiguillage propre
switch t := v.(type) {
case string:
	fmt.Println("string de longueur", len(t))
case int:
	fmt.Println("int:", t)
case nil:
	fmt.Println("nil")
default:
	fmt.Printf("type inattendu %T\n", t)
}
```

## 27. Génériques (Go 1.18+) : quand s'en servir

```go
// Fonction générique : contrainte via ~
func Min[T int | int64 | float64](a, b T) T {
	if a < b {
		return a
	}
	return b
}

// Contraintes du paquet cmp / slices (~1.21)
import "cmp"
func Max2[T cmp.Ordered](a, b T) T { ... }

// Type générique
type Pile[T any] struct {
	elements []T
}

func (p *Pile[T]) Push(v T) { p.elements = append(p.elements, v) }
func (p *Pile[T]) Pop() (T, bool) {
	if len(p.elements) == 0 {
		var zero T
		return zero, false
	}
	v := p.elements[len(p.elements)-1]
	p.elements = p.elements[:len(p.elements)-1]
	return v, true
}
```

Doctrine : les génériques servent aux **conteneurs et algorithmes** (slices, maps,
sets, files). Pour la logique métier, les interfaces restent l'outil principal.
N'abusez pas : une fonction générique là où `any` + assertion suffisait complique la lecture.

## 28. Gestion d'erreurs explicite : la philosophie Go

Pas d'exceptions. Une fonction qui peut échouer renvoie `error` en **dernière** position.

```go
contenu, err := os.ReadFile("/etc/hostname")
if err != nil {
	// Traiter : logguer, enrichir, propager, ou abandonner — mais JAMAIS ignorer silencieusement
	return fmt.Errorf("lecture hostname: %w", err)
}
fmt.Println(string(contenu))
```

Le motif `if err != nil` représente une part visible du code Go : c'est normal,
c'est documenté, c'est le prix de l'explicite. Les erreurs sont des **valeurs**,
on les inspecte, on les enveloppe, on les compare.

| Réflexe | Exemple |
|---|---|
| Propager en enrichissant | `return fmt.Errorf("connexion %s: %w", hote, err)` |
| Valeur sentinelle | `var ErrTimeout = errors.New("timeout")` puis `errors.Is(err, ErrTimeout)` |
| Erreur typée | `var e *MonErreur; errors.As(err, &e)` |
| Abandonner (main, init) | `log.Fatalf("...: %v", err)` |

## 29. Créer, envelopper, inspecter les erreurs

```go
import (
	"errors"
	"fmt"
)

// 1. Erreur simple
err := errors.New("disque plein")

// 2. Erreur formatée
err = fmt.Errorf("écriture %s: %d octets manquants", chemin, n)

// 3. Envelopper (wrap) avec %w : conserve la chaîne pour errors.Is/As
if err != nil {
	return fmt.Errorf("sauvegarde journalière: %w", err)
}

// 4. Sentinelle + test
var ErrFichierAbsent = errors.New("fichier absent")

func charger(p string) error {
	if _, err := os.Stat(p); err != nil {
		return fmt.Errorf("%w: %s", ErrFichierAbsent, p)
	}
	return nil
}

if errors.Is(charger("/x"), ErrFichierAbsent) {
	fmt.Println("créer le fichier par défaut")
}

// 5. Erreur personnalisée avec contexte
type ErreurReseau struct {
	Hote string
	Op   string
	Err  error
}

func (e *ErreurReseau) Error() string {
	return fmt.Sprintf("%s %s: %v", e.Op, e.Hote, e.Err)
}
func (e *ErreurReseau) Unwrap() error { return e.Err } // dépliage pour Is/As

// 6. Extraire un type précis
var er *ErreurReseau
if errors.As(err, &er) {
	fmt.Println("hôte fautif:", er.Hote)
}
```

Checklist erreurs :
- [ ] Chaque `err != nil` est traité (jamais `_ =` silencieux hors cas justifié et commenté)
- [ ] `%w` (pas `%v`) quand on veut garder la chaîne d'inspection
- [ ] Messages en minuscules, sans ponctuation finale (convention)
- [ ] Pas de `panic` pour une erreur d'exécution (section 21)

## 30. Packages : organisation et visibilité

```
monoutil/
├── go.mod
├── main.go            # package main : le binaire
├── cmd/
│   └── monoutil/
│       └── main.go    # variante : main dans cmd/ (convention CLI)
├── internal/
│   └── inventaire/    # importable UNIQUEMENT par ce module
│       └── inventaire.go  # package inventaire
└── pkg/               # (convention) code réutilisable hors module
    └── reseau/
        └── ping.go    # package reseau
```

Règles :
- **1 dossier = 1 package**. Le nom du package = nom du dossier (sauf `main`).
- Exporté = commence par une **majuscule** (`Serveur`, `Verifier`). Non exporté = minuscule.
- `internal/` : le compilateur **interdit** l'import depuis l'extérieur du module parent.
- Évitez les imports cycliques : le compilateur les refuse. Restructurez (extrayez un package commun).
- Un fichier peut importer un package sous alias : `import inv "infra/outils/internal/inventaire"`.
- Import pour effet de bord seul : `import _ "net/http/pprof"` (init du package).
---

## 31. Entrées-sorties console : `fmt` au quotidien

```go
fmt.Println("texte", 42, true)          // espaces auto, \n final
fmt.Printf("hôte=%s ip=%s charge=%.1f%%\n", nom, ip, charge)
fmt.Sprintf("...")                       // renvoie la string au lieu d'afficher
fmt.Fprintf(os.Stderr, "erreur: %v\n", err) // vers un writer quelconque

// Verbes essentiels
// %v valeur brute | %+v struct avec noms de champs | %#v syntaxe Go
// %T type | %d entier | %s string | %q string quotée | %x hexadécimal
// %f flottant | %.2f 2 décimales | %t booléen | %p pointeur
type S struct{ Nom string; Port int }
fmt.Printf("%+v\n", S{"dns", 53}) // {Nom:dns Port:53}

// Lecture clavier
var nom string
fmt.Print("Nom d'hôte : ")
fmt.Scanln(&nom)
```

## 32. Fichiers avec `os` : lire, écrire, infos

```go
// Lire tout un (petit) fichier
data, err := os.ReadFile("/etc/hosts")
if err != nil {
	log.Fatal(err)
}

// Écrire (crée ou tronque, droits 0644)
err = os.WriteFile("/tmp/test.txt", []byte("hello\n"), 0644)

// Ajouter à la fin : ouvrir avec flags
f, err := os.OpenFile("/var/log/monoutil.log", os.O_APPEND|os.O_CREATE|os.O_WRONLY, 0644)
if err != nil {
	log.Fatal(err)
}
defer f.Close()
f.WriteString("ligne de log\n")

// Infos et existence
info, err := os.Stat("/etc/hostname")
if err != nil {
	if os.IsNotExist(err) {
		fmt.Println("absent")
	}
} else {
	fmt.Println(info.Size(), info.Mode(), info.ModTime())
}

// Créer / renommer / supprimer
os.MkdirAll("/opt/monoutil/data", 0755) // parents inclus
os.Rename("/tmp/a", "/tmp/b")
os.Remove("/tmp/b")          // fichier vide ou dossier vide
os.RemoveAll("/tmp/cache")   // récursif : dangereux, à manier avec précaution

// Variables d'environnement
os.Getenv("HOME")            // "" si absente
os.LookupEnv("MODE")         // (valeur, présente?)
os.Setenv("MODE", "prod")
```

> Droits : `0644`/`0755` en littéral octal (`0` préfixe). Depuis Go 1.13 on peut
> aussi écrire `0o644`. Préférez `0o644` dans le code neuf.

## 33. `bufio` : traiter les gros fichiers ligne par ligne

Ne jamais `os.ReadFile` un log de 2 Go. Scanner ligne par ligne :

```go
f, err := os.Open("/var/log/syslog")
if err != nil {
	log.Fatal(err)
}
defer f.Close()

sc := bufio.NewScanner(f)
sc.Buffer(make([]byte, 1024*1024), 1024*1024) // lignes > 64 Ko si besoin
n := 0
for sc.Scan() {
	ligne := sc.Text()
	if strings.Contains(ligne, "error") {
		n++
	}
}
if err := sc.Err(); err != nil {
	log.Fatalf("scan: %v", err)
}
fmt.Println("lignes d'erreur:", n)

// Écriture bufferisée (flush !)
w := bufio.NewWriter(f2)
w.WriteString("...")
w.Flush() // sans Flush, rien n'est écrit
```

## 34. `path/filepath` : chemins portables

```go
filepath.Join("opt", "monoutil", "data") // "opt/monoutil/data" (séparateur OS)
filepath.Dir("/a/b/c.txt")              // "/a/b"
filepath.Base("/a/b/c.txt")             // "c.txt"
filepath.Ext("archive.tar.gz")          // ".gz"
filepath.Abs("relatif")                 // chemin absolu

// Lister avec motif
fichiers, _ := filepath.Glob("/etc/*.conf")

// Parcours récursif
err := filepath.WalkDir("/opt/data", func(chemin string, d fs.DirEntry, err error) error {
	if err != nil {
		return err
	}
	if !d.IsDir() && strings.HasSuffix(chemin, ".log") {
		fmt.Println(chemin)
	}
	return nil
})
```

## 35. `encoding/json` : le format d'échange sysadmin

```go
type Hote struct {
	Nom     string   `json:"nom"`
	IP      string   `json:"ip"`
	Tags    []string `json:"tags,omitempty"` // omitempty : omis si vide
	Secret  string   `json:"-"`              // jamais sérialisé
	EnLigne bool     `json:"en_ligne"`
}

// Struct -> JSON
h := Hote{Nom: "web-01", IP: "10.0.0.80", EnLigne: true}
data, err := json.MarshalIndent(h, "", "  ")
fmt.Println(string(data))

// JSON -> struct (champs non exportés ignorés silencieusement !)
var h2 Hote
if err := json.Unmarshal(data, &h2); err != nil {
	log.Fatal(err)
}

// Décoder un flux (fichier, réponse HTTP)
f, _ := os.Open("hotes.json")
defer f.Close()
var liste []Hote
dec := json.NewDecoder(f)
if err := dec.Decode(&liste); err != nil {
	log.Fatal(err)
}

// JSON générique quand la structure est inconnue
var gen map[string]any
json.Unmarshal([]byte(`{"a":1,"b":"x"}`), &gen)
fmt.Println(gen["a"].(float64)) // les nombres deviennent float64 !
```

Pièges JSON :
- Champs **non exportés** (minuscule) = ignorés silencieusement dans les deux sens.
- Nombres → `float64` en décodage générique : pour de gros entiers, utilisez
  `json.Number` ou décodez vers une struct typée.
- Tags `json:"-"` pour exclure les secrets des dumps (section 66).

## 36. CSV, YAML, TOML : les autres formats de l'inventaire

```go
// CSV : encoding/csv (stdlib)
f, _ := os.Open("inventaire.csv")
r := csv.NewReader(f)
r.Comma = ';' // CSV "français"
lignes, err := r.ReadAll()

w := csv.NewWriter(os.Stdout)
w.Write([]string{"nom", "ip"})
w.Write([]string{"web-01", "10.0.0.80"})
w.Flush()

// YAML : pas en stdlib — gopkg.in/yaml.v3 (go get)
// TOML : pas en stdlib — github.com/pelletier/go-toml/v2 ou BurntSushi/toml
```

```yaml
# Exemple inventaire.yaml (lu avec yaml.v3)
hotes:
  - nom: web-01
    ip: 10.0.0.80
    roles: [web, prod]
```

```go
// Lecture YAML (après go get gopkg.in/yaml.v3)
import "gopkg.in/yaml.v3"

type Inventaire struct {
	Hotes []Hote `yaml:"hotes"`
}
data, _ := os.ReadFile("inventaire.yaml")
var inv Inventaire
yaml.Unmarshal(data, &inv) // tags yaml: si les noms diffèrent
```

Choix de format : JSON pour les API, YAML pour la config humaine, TOML pour la
config simple, CSV pour l'échange tableur. Viper (section 52) abstrait tout ça.

## 37. Le temps : `time`, durées, timeouts

```go
maintenant := time.Now()
fmt.Println(maintenant.Format("2006-01-02 15:04:05")) // format = date de référence !
fmt.Println(maintenant.Format(time.RFC3339))          // 2026-09-26T23:34:27Z

// La date de référence Go : 02/01/2006 15:04:05 (1,2,3,4,5,6,7)
t, err := time.Parse("2006-01-02", "2026-09-26")

// Durées
d, _ := time.ParseDuration("1h30m")
time.Sleep(500 * time.Millisecond)
deadline := time.Now().Add(5 * time.Second)

// Mesurer
debut := time.Now()
traiter()
fmt.Println("durée:", time.Since(debut))

// Ticker : action périodique (supervision)
ticker := time.NewTicker(60 * time.Second)
defer ticker.Stop()
for {
	select {
	case t := <-ticker.C:
		collecterMetriques(t)
	case <-ctx.Done():
		return
	}
}

// Fuseaux
paris, _ := time.LoadLocation("Europe/Paris")
fmt.Println(maintenant.In(paris).Format("15:04 MST"))
```

> Le layout `2006-01-02 15:04:05` surprend : ce n'est pas un motif `YYYY-MM-DD`,
> c'est **la date de référence** `Mon Jan 2 15:04:05 MST 2006` (1 2 3 4 5 6 7).
> On écrit l'heure qu'il est "le 2 janvier 2006 à 15h04:05".

## 38. Goroutines : la concurrence en un mot-clé

```go
func sonder(hote string) {
	// ... ping / tcp dial ...
}

func main() {
	hotes := []string{"web-01", "db-01", "dns-01"}

	// Lancer une goroutine : préfixer par 'go'
	for _, h := range hotes {
		go sonder(h) // s'exécute en parallèle, ne bloque pas
	}

	time.Sleep(2 * time.Second) // ATTENTION : bricolage, voir WaitGroup section 41
	fmt.Println("fini (peut-être)")
}
```

Réalités :
- Une goroutine coûte ~8 Ko de pile initiale (vs ~1 Mo pour un thread OS).
  On en lance des **milliers** sans sourciller.
- `go f()` ne retourne rien et ne propage pas d'erreur : communiquez via channels (39).
- Le `main` qui se termine **tue** toutes les goroutines : synchronisez (WaitGroup).
- Ordre d'exécution non garanti : ne jamais supposer un ordre entre goroutines.

## 39. Channels : le tuyau typé entre goroutines

```go
// Channel non bufferisé : l'envoi bloque jusqu'à réception (rendez-vous)
ch := make(chan string)
go func() {
	ch <- "résultat" // bloque jusqu'à ce que main reçoive
}()
msg := <-ch // reçoit
fmt.Println(msg)

// Channel bufferisé : l'envoi bloque seulement si plein
file := make(chan int, 10)
file <- 1 // ne bloque pas (place libre)

// Fermer : signale "plus de données" (seul l'ÉMETTEUR ferme)
go func() {
	for i := 0; i < 3; i++ {
		ch2 <- i
	}
	close(ch2)
}()
for v := range ch2 { // boucle jusqu'à fermeture
	fmt.Println(v)
}

// Tester la fermeture
v, ok := <-ch2 // ok == false si fermé ET vide
```

| Type | Création | Sémantique |
|---|---|---|
| Non bufferisé | `make(chan T)` | Rendez-vous : envoi ↔ réception synchrones |
| Bufferisé | `make(chan T, n)` | File de n places, découplage partiel |
| Lecture seule / écriture seule | `chan<- T`, `<-chan T` | Dans les signatures : exprime l'intention |

Règles : on ne ferme que côté émetteur ; envoyer sur un channel fermé = panic ;
recevoir sur un channel `nil` bloque **pour toujours** (utile dans `select`, section 40).

## 40. `select` : l'aiguillage des channels

```go
select {
case msg := <-ch1:
	fmt.Println("ch1:", msg)
case ch2 <- "ping":
	fmt.Println("envoyé sur ch2")
case <-time.After(2 * time.Second): // timeout !
	fmt.Println("timeout")
default: // optionnel : ne bloque pas si aucun cas prêt
	fmt.Println("rien de prêt")
}
```

Motifs indispensables :

```go
// 1. Timeout sur opération
select {
case res := <-resultat:
	traiter(res)
case <-time.After(5 * time.Second):
	return errors.New("timeout après 5s")
}

// 2. Annulation via context (voir section 42)
select {
case <-ctx.Done():
	return ctx.Err()
case v := <-ch:
	fmt.Println(v)
}

// 3. Non-bloquant : envoyer seulement si un lecteur attend
select {
case ch <- v:
default:
	// file pleine / personne n'écoute : on jette ou on compte
}
```

`select` choisit **au hasard** si plusieurs cas sont prêts simultanément :
ne jamais en dépendre pour une priorité (ou ordonner les tests manuellement).

## 41. `sync` : Mutex, WaitGroup, Once

```go
import "sync"

// --- WaitGroup : attendre des goroutines ---
var wg sync.WaitGroup
for _, h := range hotes {
	wg.Add(1)
	go func(hote string) {
		defer wg.Done() // ou wg.Add(-1)
		sonder(hote)
	}(h) // passer h en paramètre : évite la capture de la variable de boucle
}
wg.Wait() // bloque jusqu'à ce que le compteur retombe à 0

// --- Mutex : protéger une ressource partagée ---
var mu sync.Mutex
compteurs := map[string]int{}
go func() {
	mu.Lock()
	compteurs["ok"]++
	mu.Unlock() // ou defer mu.Unlock() juste après Lock
}()

// RWMutex : verrou lecture/écriture (lectures concurrentes ok)
var rwmu sync.RWMutex
rwmu.RLock(); v := cache[k]; rwmu.RUnlock()

// --- Once : initialisation unique thread-safe ---
var (
	cfg     Config
	cfgOnce sync.Once
)
func getConfig() Config {
	cfgOnce.Do(func() { cfg = chargerConfig() })
	return cfg
}

// --- Pool : recycler des objets (buffers) ---
var bufPool = sync.Pool{New: func() any { return make([]byte, 4096) }}
buf := bufPool.Get().([]byte)
defer bufPool.Put(buf)
```

> Depuis **Go 1.22**, la variable de boucle `for _, h := range` est **recréée à
> chaque itération** : `go func() { sonder(h) }()` est sûr. Avant 1.22, il fallait
> passer `h` en paramètre (comme ci-dessus) — gardez l'habitude, elle reste correcte.

Checklist concurrence :
- [ ] `go vet` ne signale pas de copie de `sync.Mutex` (ne jamais copier un Mutex !)
- [ ] Chaque `Lock` a son `Unlock` (defer de préférence)
- [ ] Pas d'accès concurrent à une map sans protection (`go run -race`, section 63)
- [ ] `WaitGroup.Add` **avant** `go`, `Done` en `defer` dans la goroutine

## 42. `context` : annulation et deadlines propres

```go
import "context"

// Timeout : toute l'opération (et ses sous-appels) meurt après 5s
ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
defer cancel() // LIBÈRE les ressources — ne jamais oublier

req, _ := http.NewRequestWithContext(ctx, "GET", url, nil)
resp, err := http.DefaultClient.Do(req)
if err != nil {
	// ctx.DeadlineExceeded ou context.Canceled
}

// Annulation manuelle
ctx2, cancel2 := context.WithCancel(context.Background())
go surveiller(ctx2)
// ... plus tard :
cancel2()

// Valeurs (avec parcimonie : request-scoped uniquement, jamais de secrets !)
ctx3 := context.WithValue(ctx, cleRequeteID, "abc-123")
```

Règles :
- `ctx` est **toujours le premier paramètre** : `func Faire(ctx context.Context, ...)`.
- Ne stockez jamais un `Context` dans un struct ; propagez-le en paramètre.
- `defer cancel()` même si vous pensez ne pas en avoir besoin (fuite de timer sinon).
- `context.Background()` dans `main`/`init`/tests ; `r.Context()` dans les handlers HTTP.
---

## 43. Pattern : worker pool (N workers, file de tâches)

Le classique du sysadmin : sonder 500 hôtes avec 20 connexions parallèles max.

```go
func workerPool(hotes []string, nbWorkers int) {
	taches := make(chan string)
	var wg sync.WaitGroup

	// Lancer les workers
	for i := 0; i < nbWorkers; i++ {
		wg.Add(1)
		go func(id int) {
			defer wg.Done()
			for h := range taches { // jusqu'à fermeture du channel
				if err := sonder(h); err != nil {
					fmt.Printf("worker %d: %s: %v\n", id, h, err)
				}
			}
		}(i)
	}

	// Distribuer
	for _, h := range hotes {
		taches <- h
	}
	close(taches) // signale la fin aux workers
	wg.Wait()
}
```

Variante avec résultats et erreurs via `errgroup` : voir section 46.

## 44. Pattern : fan-in / fan-out

- **Fan-out** : distribuer du travail à plusieurs goroutines (le worker pool ci-dessus).
- **Fan-in** : fusionner plusieurs channels en un seul.

```go
// Fan-in : multiplexe plusieurs sources vers un channel unique
func fusionner(cs ...<-chan Resultat) <-chan Resultat {
	sortie := make(chan Resultat)
	var wg sync.WaitGroup
	for _, c := range cs {
		wg.Add(1)
		go func(c <-chan Resultat) {
			defer wg.Done()
			for r := range c {
				sortie <- r
			}
		}(c)
	}
	go func() {
		wg.Wait()
		close(sortie) // fermer quand toutes les sources sont épuisées
	}()
	return sortie
}

// Usage : 3 sondes parallèles, résultats unifiés
c1 := sonderAsync("web-01")
c2 := sonderAsync("db-01")
c3 := sonderAsync("dns-01")
for r := range fusionner(c1, c2, c3) {
	fmt.Println(r.Hote, r.Ok)
}
```

## 45. Pattern : pipeline à étapes

```go
// Chaque étape : channel en entrée, channel en sortie, fermeture en cascade
func listerFichiers(racine string) <-chan string {
	sortie := make(chan string)
	go func() {
		defer close(sortie)
		filepath.WalkDir(racine, func(p string, d fs.DirEntry, err error) error {
			if err == nil && !d.IsDir() {
				sortie <- p
			}
			return nil
		})
	}()
	return sortie
}

func filtrerLogs(entree <-chan string) <-chan string {
	sortie := make(chan string)
	go func() {
		defer close(sortie)
		for p := range entree {
			if strings.HasSuffix(p, ".log") {
				sortie <- p
			}
		}
	}()
	return sortie
}

// Assemblage lisible
for p := range filtrerLogs(listerFichiers("/var/log")) {
	fmt.Println(p)
}
```

Ajoutez un `ctx` en paramètre de chaque étape pour une annulation propre
(`select` sur `ctx.Done()` dans les boucles d'envoi).

## 46. `errgroup` : goroutines + erreurs + annulation

`golang.org/x/sync/errgroup` — le chaînon manquant de la stdlib.

```go
import "golang.org/x/sync/errgroup"

func controlerTous(ctx context.Context, hotes []string) error {
	g, ctx := errgroup.WithContext(ctx)
	g.SetLimit(20) // max 20 goroutines simultanées (sémaphore intégré)

	for _, h := range hotes {
		h := h
		g.Go(func() error {
			// Si une goroutine échoue, ctx est annulé pour les autres
			return sonderAvecContexte(ctx, h)
		})
	}
	return g.Wait() // première erreur rencontrée (ou nil)
}
```

C'est le pattern à utiliser par défaut pour du travail parallèle avec gestion
d'erreur : plus lisible que WaitGroup + channel d'erreurs bricolé.

## 47. Serveur HTTP avec `net/http` (stdlib, ≥ 1.22)

Depuis Go 1.22, le routeur stdlib gère les **méthodes et paramètres** : fini
l'excuse du framework pour une API interne.

```go
package main

import (
	"encoding/json"
	"log"
	"net/http"
	"time"
)

func main() {
	mux := http.NewServeMux()

	// Routes avec méthode + paramètre {nom}
	mux.HandleFunc("GET /sante", func(w http.ResponseWriter, r *http.Request) {
		w.Header().Set("Content-Type", "application/json")
		json.NewEncoder(w).Encode(map[string]string{"statut": "ok"})
	})
	mux.HandleFunc("GET /hotes/{nom}", func(w http.ResponseWriter, r *http.Request) {
		nom := r.PathValue("nom") // Go 1.22+
		fmt.Fprintf(w, "hôte: %s", nom)
	})
	mux.HandleFunc("POST /hotes", creerHote)

	srv := &http.Server{
		Addr:         ":8080",
		Handler:      avecLogs(mux), // middleware
		ReadTimeout:  5 * time.Second,
		WriteTimeout: 10 * time.Second,
		IdleTimeout:  60 * time.Second,
	}

	// Arrêt gracieux (voir section 55)
	go func() {
		log.Println("écoute sur :8080")
		if err := srv.ListenAndServe(); err != http.ErrServerClosed {
			log.Fatal(err)
		}
	}()
	attendreSignal()
	ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
	defer cancel()
	srv.Shutdown(ctx)
}

// Middleware : log + request ID
func avecLogs(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		debut := time.Now()
		next.ServeHTTP(w, r)
		log.Printf("%s %s %s", r.Method, r.URL.Path, time.Since(debut))
	})
}
```

## 48. Client HTTP : timeouts obligatoires

Le client par défaut **n'a pas de timeout** : une API pendue = goroutine pendue.

```go
client := &http.Client{
	Timeout: 10 * time.Second, // timeout TOTAL de la requête
}

// GET simple
resp, err := client.Get("http://10.0.0.53:8080/sante")
if err != nil {
	log.Fatal(err)
}
defer resp.Body.Close() // OBLIGATOIRE : sinon fuite de connexions

if resp.StatusCode != http.StatusOK {
	log.Fatalf("statut inattendu: %s", resp.Status)
}
corps, _ := io.ReadAll(resp.Body)

// POST JSON
payload, _ := json.Marshal(map[string]string{"nom": "web-02"})
req, _ := http.NewRequest("POST", url, bytes.NewReader(payload))
req.Header.Set("Content-Type", "application/json")
req.Header.Set("Authorization", "Bearer "+os.Getenv("API_TOKEN"))
resp, err = client.Do(req)
```

Réglages fins (pool de connexions, TLS) via `http.Transport` :

```go
tr := &http.Transport{
	MaxIdleConns:        50,
	MaxIdleConnsPerHost: 10,
	IdleConnTimeout:     90 * time.Second,
	TLSClientConfig:     &tls.Config{MinVersion: tls.VersionTLS12},
}
client := &http.Client{Timeout: 10 * time.Second, Transport: tr}
```

## 49. JSON sur HTTP : API complète minimale

```go
type Hote struct {
	Nom string `json:"nom"`
	IP  string `json:"ip"`
}

func creerHote(w http.ResponseWriter, r *http.Request) {
	var h Hote
	dec := json.NewDecoder(r.Body)
	dec.DisallowUnknownFields() // rejette les champs inconnus : config stricte
	if err := dec.Decode(&h); err != nil {
		http.Error(w, "JSON invalide: "+err.Error(), http.StatusBadRequest)
		return
	}
	if h.Nom == "" || h.IP == "" {
		http.Error(w, "nom et ip requis", http.StatusBadRequest)
		return
	}
	w.Header().Set("Content-Type", "application/json")
	w.WriteHeader(http.StatusCreated)
	json.NewEncoder(w).Encode(map[string]string{"id": h.Nom, "statut": "créé"})
}
```

## 50. Logs : `log` puis `slog` (structuré, ≥ 1.21)

```go
// Simple (stdlib historique)
log.Println("démarrage")
log.Printf("hôte %s injoignable: %v", h, err)
log.Fatal(err) // affiche + os.Exit(1) — à réserver à main/init !

// Structuré avec slog (Go 1.21+) : indispensable en prod/supervision
import "log/slog"

logger := slog.New(slog.NewJSONHandler(os.Stdout, &slog.HandlerOptions{
	Level: slog.LevelInfo,
}))
slog.SetDefault(logger)

slog.Info("sonde terminée", "hote", h, "latence_ms", 12, "ok", true)
slog.Warn("retry", "tentative", 2, "hote", h)
slog.Error("échec", "err", err)
// Sortie : {"time":"...","level":"INFO","msg":"sonde terminée","hote":"web-01",...}
```

Niveaux : `Debug < Info < Warn < Error`. En prod : JSON vers stdout,
collecté par l'agent de logs (pas de fichiers de logs gérés à la main :
principe 12-factor, et `logrotate` en plan B).

## 51. Exécuter des commandes externes : `os/exec`

Le pont entre Go et l'existant shell.

```go
// Simple : capturer la sortie
out, err := exec.Command("uptime").Output()
if err != nil {
	if ee, ok := err.(*exec.ExitError); ok {
		fmt.Println("code:", ee.ExitCode(), "stderr:", string(ee.Stderr))
	}
}

// Avec contexte (timeout qui TUE le processus)
ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
defer cancel()
cmd := exec.CommandContext(ctx, "rsync", "-av", "/src/", "/dst/")
cmd.Stdout = os.Stdout // streamer au lieu de bufferiser
cmd.Stderr = os.Stderr
if err := cmd.Run(); err != nil {
	log.Fatalf("rsync: %v", err)
}

// Entrée stdin + shell complet (avec prudence : injection !)
cmd = exec.Command("sh", "-c", "df -h | awk 'NR>1 {print $5, $6}'")
// JAMAIS de concaténation de variable utilisateur dans "sh -c" sans validation.

// Vérifier la présence d'un binaire
if _, err := exec.LookPath("dig"); err != nil {
	log.Fatal("dig introuvable dans le PATH")
}
```

Sécurité : préférez `exec.Command("dig", "+short", hote)` (args séparés, pas de shell)
à `sh -c` avec interpolation. Voir section 66.

## 52. Signaux OS : arrêt propre (SIGTERM/SIGINT)

```go
func attendreSignal() {
	sigs := make(chan os.Signal, 1)
	signal.Notify(sigs, syscall.SIGINT, syscall.SIGTERM)
	sig := <-sigs
	fmt.Println("\nsignal reçu:", sig)
	// puis : cancel(), srv.Shutdown(), wg.Wait()...
}

// Version avec context : la plus propre
func contexteSignal() (context.Context, context.CancelFunc) {
	return signal.NotifyContext(context.Background(), syscall.SIGINT, syscall.SIGTERM)
}

func main() {
	ctx, stop := contexteSignal()
	defer stop()
	// ... serveurs, workers utilisent ctx ...
	<-ctx.Done() // bloqué jusqu'au Ctrl+C / SIGTERM (systemd !)
	fmt.Println("arrêt demandé, nettoyage...")
}
```

Sous systemd, `SIGTERM` est le signal d'arrêt par défaut : un binaire Go qui
l'intercepte s'arrête proprement (flush, close, déréférencement). Voir section 72.

## 53. CLI simple avec `flag` (stdlib)

```go
package main

import (
	"flag"
	"fmt"
	"os"
)

func main() {
	var (
		hote    = flag.String("hote", "localhost", "hôte à sonder")
		port    = flag.Int("port", 22, "port TCP")
		timeout = flag.Duration("timeout", 5*time.Second, "délai max")
		verbeux = flag.Bool("v", false, "mode verbeux")
	)
	flag.Usage = func() {
		fmt.Fprintf(os.Stderr, "Usage: %s [options]\nSonde TCP d'un hôte.\n\n", os.Args[0])
		flag.PrintDefaults()
	}
	flag.Parse()

	if flag.NArg() > 0 {
		fmt.Println("args positionnels:", flag.Args())
	}
	fmt.Printf("sonde %s:%d (timeout %s, verbeux=%v)\n", *hote, *port, *timeout, *verbeux)
}
```

`flag` suffit pour 80 % des outils internes. Pour des sous-commandes (`monoutil
hotes list`), passez à Cobra (section 54).

## 54. CLI avancée : Cobra + Viper

```bash
go get github.com/spf13/cobra@latest
go get github.com/spf13/viper@latest
```

```go
package main

import (
	"fmt"
	"os"

	"github.com/spf13/cobra"
	"github.com/spf13/viper"
)

var rootCmd = &cobra.Command{
	Use:   "monoutil",
	Short: "Boîte à outils infra",
	PersistentPreRun: func(cmd *cobra.Command, args []string) {
		// Viper : config fichier + env + flags, par priorité
		viper.SetConfigName("monoutil")
		viper.AddConfigPath("/etc/monoutil")
		viper.AddConfigPath("$HOME/.monoutil")
		viper.SetEnvPrefix("MONOUTIL")
		viper.AutomaticEnv()
		_ = viper.ReadInConfig() // absent = ok, les flags/env suffisent
	},
}

var sondeCmd = &cobra.Command{
	Use:   "sonde [hote]",
	Short: "Sonde TCP",
	Args:  cobra.ExactArgs(1),
	RunE: func(cmd *cobra.Command, args []string) error {
		port, _ := cmd.Flags().GetInt("port")
		timeout := viper.GetDuration("timeout") // flag > env > config > défaut
		return sonder(args[0], port, timeout)
	},
}

func main() {
	sondeCmd.Flags().Int("port", 22, "port TCP")
	sondeCmd.Flags().Duration("timeout", 5_000_000_000, "délai max") // 5s
	_ = viper.BindPFlag("timeout", sondeCmd.Flags().Lookup("timeout"))
	rootCmd.AddCommand(sondeCmd)
	if err := rootCmd.Execute(); err != nil {
		os.Exit(1)
	}
}
```

Ordre de priorité Viper : **flag > variable d'environnement > fichier config > défaut**.
Idéal : comportement surchargeable sans recompiler.

## 55. Compilation croisée : un binaire par OS/arch

```bash
# Lister les cibles supportées
go tool dist list | grep -E 'linux|windows|darwin' | head

# Compiler pour Windows depuis Linux (CGO désactivé = 100% statique)
GOOS=windows GOARCH=amd64 go build -o monoutil.exe .

# Linux ARM64 (Raspberry Pi, serveurs ARM)
GOOS=linux GOARCH=arm64 go build -o monoutil-arm64 .

# macOS Apple Silicon
GOOS=darwin GOARCH=arm64 go build -o monoutil-mac .

# Binaire allégé : sans symboles de debug ni table des symboles
go build -ldflags="-s -w" -o monoutil .
# -s : strip symbol table | -w : strip DWARF => -20 à -30% de taille

# Injecter version/date à la compilation
go build -ldflags="-s -w -X main.version=$(git describe --tags) -X main.date=$(date -u +%FT%TZ)" .
```

```go
var (
	version = "dev"     // surchargé par -X main.version=...
	date    = "inconnue"
)

func main() {
	if len(os.Args) > 1 && os.Args[1] == "--version" {
		fmt.Printf("monoutil %s (%s)\n", version, date)
		return
	}
	// ...
}
```

> Avec `CGO_ENABLED=0` (défaut si aucun import C), le binaire est **statique** :
> déposable tel quel sur n'importe quelle machine du même OS/arch, y compris
> dans une image `scratch` Docker ou sur un équipement minimal.

## 56. Build tags : code conditionnel par plateforme

```go
//go:build linux
// +build linux   // ligne legacy, garder les deux pour < 1.17 (rare)

package main

import "syscall"

func limites() syscall.Rlimit { /* spécifique Linux */ return syscall.Rlimit{} }
```

```go
//go:build windows

package main

func limites() struct{} { /* stub Windows */ return struct{}{} }
```

Fichier `limites_linux.go` / `limites_windows.go` : le compilateur choisit selon
`GOOS`. Utile pour le code vraiment spécifique (syscall, chemins). Pour le reste,
`runtime.GOOS` suffit :

```go
if runtime.GOOS == "windows" {
	// ...
}
```

## 57. Tests natifs : `testing`, table-driven

Fichier `sonde_test.go` (même package, suffixe `_test.go`) :

```go
package main

import "testing"

// Test table-driven : LE standard Go
func TestAdresse(t *testing.T) {
	tests := []struct {
		nom     string
		ip      string
		port    int
		attendu string
	}{
		{"basique", "10.0.0.53", 53, "10.0.0.53:53"},
		{"port zéro", "10.0.0.1", 0, "10.0.0.1:0"},
	}
	for _, tt := range tests {
		t.Run(tt.nom, func(t *testing.T) {
			s := Serveur{IP: tt.ip, Port: tt.port}
			if got := s.Adresse(); got != tt.attendu {
				t.Errorf("Adresse() = %q, attendu %q", got, tt.attendu)
			}
		})
	}
}

func TestDiviserParZero(t *testing.T) {
	if _, err := diviser(1, 0); err == nil {
		t.Fatal("erreur attendue pour b=0")
	}
}
```

```bash
go test ./...              # tout le module
go test -run TestAdresse . # un seul test
go test -v ./...           # verbeux
go test -race ./...        # détecteur de data races (CI obligatoire)
go test -cover ./...       # couverture
go test -coverprofile=c.out ./... && go tool cover -html=c.out
```

## 58. Benchmarks et exemples

```go
func BenchmarkConcat(b *testing.B) {
	hotes := []string{"a", "b", "c", "d"}
	b.ResetTimer()
	for i := 0; i < b.N; i++ {
		var sb strings.Builder
		for _, h := range hotes {
			sb.WriteString(h)
		}
		_ = sb.String()
	}
}

// Exemple : apparaît dans la doc (go doc) et est TESTÉ
func ExampleServeur_Adresse() {
	s := Serveur{IP: "10.0.0.53", Port: 53}
	fmt.Println(s.Adresse())
	// Output: 10.0.0.53:53
}
```

```bash
go test -bench=. -benchmem ./...   # -benchmem : allocations par opération
```

Sortie typique :
```
BenchmarkConcat-8   5000000   240 ns/op   32 B/op   2 allocs/op
```
Objectif perf : réduire `B/op` et `allocs/op` d'abord (moins d'allocations =
moins de travail pour le GC).

## 59. Fuzzing natif (Go 1.18+)

```go
func FuzzParseLigne(f *testing.F) {
	// Graines : cas connus
	f.Add("web-01,10.0.0.80,up")
	f.Add("malformé")
	f.Fuzz(func(t *testing.T, ligne string) {
		h, err := parseLigne(ligne)
		if err == nil && h.IP == "" {
			t.Fatalf("IP vide sans erreur pour %q", ligne)
		}
		// Invariant : ne doit jamais paniquer
	})
}
```

```bash
go test -fuzz=FuzzParseLigne -fuzztime=30s .
```

Le fuzzer génère des entrées aléatoires pour faire paniquer votre parser :
excellent pour tout ce qui lit des données externes (logs, CSV, réponses d'API).

## 60. Debug : `delve`, `pprof`, traces

```bash
# Delve (debugueur) : go install github.com/go-delve/delve/cmd/dlv@latest
dlv debug .              # lance avec debugueur
# (dlv) break main.go:42 | (dlv) continue | (dlv) print var | (dlv) step

# Profiling intégré : exposer pprof sur un port interne
import _ "net/http/pprof"
go func() { http.ListenAndServe("127.0.0.1:6060", nil) }()

# Capturer 30s de profil CPU puis analyser
go tool pprof http://127.0.0.1:6060/debug/pprof/profile?seconds=30
# (pprof) top10 | (pprof) list maFonction | (pprof) web (graphique)

# Profil mémoire
go tool pprof http://127.0.0.1:6060/debug/pprof/heap

# Trace d'exécution (goroutines, GC, syscalls)
curl -o trace.out http://127.0.0.1:6060/debug/pprof/trace?seconds=5
go tool trace trace.out
```

> N'exposez `pprof` que sur `127.0.0.1` ou un réseau admin : il révèle les
> chemins, une partie de la mémoire et permet de geler le processus.

Réflexes debug sans outils :
- `fmt.Printf("%+v\n", ...)` ciblé (retirer après).
- `go run -race` pour les data races (le bug de concurrence n°1).
- `GODEBUG=gctrace=1` pour voir l'activité du ramasse-miettes.
---

## 61. Performance : les 20 % qui comptent

1. **Mesurez d'abord** : `pprof` avant toute optimisation (section 60).
2. **Allocations** : `strings.Builder`, `make` avec capacité, `sync.Pool` pour les buffers chauds.
3. **Évitez les copies** : receveurs pointeurs, `[]byte` plutôt que `string` en boucle chaude.
4. **I/O > CPU** : en sysadmin, le goulot est le réseau/disque → concurrence (workers), pas micro-optims.
5. **GC** : `GOGC=100` par défaut ; baisser `GOGC` = GC plus fréquent = moins de pics mémoire.

```go
// Mauvais : concaténation en boucle => O(n²) allocations
var s string
for _, h := range hotes {
	s += h + "\n"
}

// Bon : Builder, une seule allocation finale (amortie)
var b strings.Builder
b.Grow(estimOctets) // si estimable
for _, h := range hotes {
	b.WriteString(h)
	b.WriteByte('\n')
}
s = b.String()
```

Tableau des leviers par coût/bénéfice :

| Levier | Coût | Gain typique |
|---|---|---|
| Bufferiser les I/O (`bufio`) | Faible | Énorme sur gros fichiers |
| Worker pool (paralléliser I/O) | Moyen | x10–x50 sur sondes réseau |
| Réduire les allocations | Moyen | -30 % CPU sur chemins chauds |
| `sync.Pool` | Moyen | Visible > 10k obj/s |
| Assembleur / unsafe | Élevé | Rarement justifié : ne pas y toucher |

## 62. Sécurité : checklist du binaire Go

- [ ] **Aucun secret dans le code** : `os.Getenv`, fichiers, Vault — jamais en dur, jamais dans `go.mod`.
- [ ] `go vet ./...` propre ; dépendances à jour (`go list -m -u all`, `go get -u` ciblé).
- [ ] Vulnérabilités connues : `go install golang.org/x/vuln/cmd/govulncheck@latest` puis `govulncheck ./...`.
- [ ] TLS : `MinVersion: tls.VersionTLS12`, vérifier les certificats (ne jamais `InsecureSkipVerify: true` en prod).
- [ ] `os/exec` : args séparés, jamais `sh -c` avec interpolation non validée (section 51).
- [ ] HTTP : timeouts partout (client **et** serveur, sections 47–48), limites de taille (`http.MaxBytesReader`).
- [ ] Fichiers : droits minimaux (`0o600` pour secrets), `MkdirAll` avec `0o750`/`0o700`.
- [ ] `pprof`/debug : bind `127.0.0.1` uniquement (section 60).
- [ ] Binaire : `CGO_ENABLED=0`, build depuis un commit taggé, SBOM si exigé (`go list -m all`).
- [ ] Entrées externes : valider (taille, charset, chemins — pas de `../` vers `/etc`).

```go
// Limiter la taille d'un corps HTTP (anti-DoS mémoire)
r.Body = http.MaxBytesReader(w, r.Body, 1<<20) // 1 Mo max

// Empêcher la traversée de chemin sur un paramètre fichier
nom := r.PathValue("nom")
if strings.Contains(nom, "..") || strings.ContainsRune(nom, '/') {
	http.Error(w, "nom invalide", http.StatusBadRequest)
	return
}
chemin := filepath.Join("/srv/data", filepath.Clean(nom))
```

## 63. Bonnes pratiques & style (Effective Go condensé)

1. **Formatage non négociable** : `gofmt` (ou `goimports`). Zéro débat de style.
2. **Noms courts** en portée courte (`i`, `s`, `err`), explicites en portée large.
3. **Commentaires** : toute fonction/méthode exportée a un commentaire commençant par son nom.
4. **Erreurs** : traitées, enveloppées avec `%w`, messages en minuscules (section 29).
5. **Interfaces petites**, définies côté usage (section 24).
6. **Pas de variables globales mutables** partagées sans synchronisation.
7. **Acceptez des interfaces, retournez des structs** (concret en sortie, abstrait en entrée).
8. **`context.Context` en premier paramètre**, jamais dans un struct (section 42).
9. **Évitez `init()`** sauf enregistrement de plugins/drivers ; préférez des constructeurs explicites `New...`.
10. **Un package = une responsabilité** ; `internal/` pour ce qui ne doit pas fuir.
11. **Tests** : table-driven, `-race` en CI, couverture sur le code critique (pas de course au %).
12. **Logs structurés** (`slog`) plutôt que `fmt.Println` en prod.

Exemple de doc conforme :
```go
// Sonder teste la connectivité TCP vers hote:port avec le timeout donné.
// Il renvoie nil si la connexion réussit avant l'expiration du délai.
func Sonder(ctx context.Context, hote string, port int, timeout time.Duration) error {
	// ...
}
```

## 64. Erreurs classiques des débutants (12 à ne plus faire)

| # | Erreur | Correct |
|---|---|---|
| 1 | Oublier de réaffecter `append` : `append(s, v)` seul | `s = append(s, v)` |
| 2 | Écrire dans une map `nil` | `m := make(map[string]int)` |
| 3 | Ignorer `err` : `f, _ := os.Open(p)` | Toujours tester `err` |
| 4 | Oublier `defer resp.Body.Close()` | Le mettre juste après l'erreur testée |
| 5 | Copier un `sync.Mutex` (struct contenant un mutex passé par valeur) | Receveurs/paramètres pointeurs |
| 6 | Variable de boucle capturée (avant Go 1.22) | Passer en paramètre de la closure |
| 7 | `go` routine sans synchronisation, `main` qui quitte | `sync.WaitGroup` / `errgroup` |
| 8 | Comparer avec `==` des slices ou attendre un ordre de map | `slices.Equal`, trier les clés |
| 9 | Division entière surprise : `7/3 == 2` | Convertir en `float64` d'abord |
| 10 | `panic` pour une erreur normale | Retourner `error` |
| 11 | `select` sans `default`/`timeout` qui bloque pour toujours | Toujours un cas de sortie |
| 12 | Secrets/URLs en dur, `InsecureSkipVerify: true` | Env, fichiers, config (section 62) |

Bonus — le piège du slice partagé :
```go
a := []int{1, 2, 3, 4}
b := a[1:3]  // b partage le tableau de a
b[0] = 99
fmt.Println(a) // [1 99 3 4] : surprise si on l'oublie !
```

## 65. Cas pratique 1 — Sonde TCP parallèle (script sysadmin)

L'outil "premier jour" : 200 hôtes, 20 workers, timeout, sortie triée.

```go
// sonde/main.go
package main

import (
	"context"
	"flag"
	"fmt"
	"net"
	"os"
	"sort"
	"sync"
	"time"

	"golang.org/x/sync/errgroup"
)

type Resultat struct {
	Hote string
	Ok   bool
	Ms   int64
	Err  string
}

func sonder(ctx context.Context, hote string, port int, timeout time.Duration) Resultat {
	debut := time.Now()
	d := net.Dialer{Timeout: timeout}
	conn, err := d.DialContext(ctx, "tcp", fmt.Sprintf("%s:%d", hote, port))
	r := Resultat{Hote: hote, Ms: time.Since(debut).Milliseconds()}
	if err != nil {
		r.Err = err.Error()
		return r
	}
	conn.Close()
	r.Ok = true
	return r
}

func main() {
	port := flag.Int("port", 22, "port TCP")
	workers := flag.Int("w", 20, "parallélisme")
	timeout := flag.Duration("timeout", 3*time.Second, "timeout par hôte")
	flag.Parse()
	hotes := flag.Args()
	if len(hotes) == 0 {
		fmt.Fprintln(os.Stderr, "usage: sonde [-port=22] [-w=20] hote1 hote2 ...")
		os.Exit(2)
	}

	ctx := context.Background()
	g, ctx := errgroup.WithContext(ctx)
	g.SetLimit(*workers)
	resultats := make([]Resultat, len(hotes))
	for i, h := range hotes {
		i, h := i, h
		g.Go(func() error {
			resultats[i] = sonder(ctx, h, *port, *timeout)
			return nil // on ne veut pas annuler les autres sur un échec
		})
	}
	_ = g.Wait()

	sort.Slice(resultats, func(i, j int) bool { return resultats[i].Hote < resultats[j].Hote })
	ko := 0
	for _, r := range resultats {
		statut := "OK"
		if !r.Ok {
			statut = "KO"
			ko++
		}
		fmt.Printf("%-20s %-3s %4d ms %s\n", r.Hote, statut, r.Ms, r.Err)
	}
	fmt.Printf("--- %d/%d joignables ---\n", len(resultats)-ko, len(resultats))
	if ko > 0 {
		os.Exit(1) // code de sortie exploitable par Nagios/Zabbix/scripts
	}
}
```

```bash
go mod init sonde && go get golang.org/x/sync@latest
go build -o sonde .
./sonde -port=443 -w=30 web-01 web-02 db-01
echo $?  # 0 si tout OK, 1 sinon
```

## 66. Cas pratique 2 — Rotation de logs / ménage disque

```go
// menage/main.go : supprime les .log de plus de N jours, avec dry-run
package main

import (
	"flag"
	"fmt"
	"log"
	"os"
	"path/filepath"
	"time"
)

func main() {
	racine := flag.String("dir", "/var/log/monapp", "dossier à nettoyer")
	jours := flag.Int("jours", 30, "âge max en jours")
	dryRun := flag.Bool("dry-run", true, "ne rien supprimer (défaut: true)")
	flag.Parse()

	limite := time.Now().AddDate(0, 0, -*jours)
	var supprimes, conserves int64
	var octets int64

	err := filepath.WalkDir(*racine, func(p string, d os.DirEntry, err error) error {
		if err != nil {
			log.Printf("accès %s: %v", p, err)
			return nil // continuer malgré tout
		}
		if d.IsDir() || filepath.Ext(p) != ".log" {
			return nil
		}
		info, err := d.Info()
		if err != nil {
			return nil
		}
		if info.ModTime().Before(limite) {
			octets += info.Size()
			if *dryRun {
				fmt.Println("[dry-run] supprimerait:", p)
			} else {
				if err := os.Remove(p); err != nil {
					log.Printf("suppression %s: %v", p, err)
				} else {
					fmt.Println("supprimé:", p)
				}
			}
			supprimes++
		} else {
			conserves++
		}
		return nil
	})
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("supprimés: %d, conservés: %d, %.1f Mo libérés\n",
		supprimes, conserves, float64(octets)/1e6)
}
```

```bash
./menage -dir=/var/log -jours=90            # dry-run par défaut : sûr
./menage -dir=/var/log -jours=90 -dry-run=false  # action réelle
```

> Le `dry-run` à `true` par défaut est un garde-fou : tout script destructeur
> devrait exiger un flag explicite pour agir.

## 67. Cas pratique 3 — Exporter Prometheus (agent de supervision)

Un exporter minimal : Go excelle ici (binaire unique, faible empreinte).

```go
// exporter/main.go
package main

import (
	"fmt"
	"log"
	"net/http"
	"os"
	"runtime"
	"time"
)

var demarrage = time.Now()

func metriques(w http.ResponseWriter, r *http.Request) {
	var m runtime.MemStats
	runtime.ReadMemStats(&m)
	fmt.Fprintf(w, `# HELP uptime_secondes Durée de fonctionnement
# TYPE uptime_secondes counter
uptime_secondes %d
# HELP goroutines Nombre de goroutines
# TYPE goroutines gauge
goroutines %d
# HELP memoire_octets Mémoire allouée
# TYPE memoire_octets gauge
memoire_octets %d
`, int64(time.Since(demarrage).Seconds()), runtime.NumGoroutine(), m.Alloc)
}

func main() {
	http.HandleFunc("GET /metrics", metriques)
	http.HandleFunc("GET /sante", func(w http.ResponseWriter, r *http.Request) {
		w.Write([]byte("ok"))
	})
	addr := ":9100"
	log.Println("exporter sur", addr)
	if err := http.ListenAndServe(addr, nil); err != nil {
		log.Fatal(err)
	}
}
```

```bash
go build -o exporter . && ./exporter &
curl localhost:9100/metrics
```

Pour un vrai exporter : `github.com/prometheus/client_golang` (compteurs,
histogrammes, labels). Le format texte ci-dessus suffit pour un besoin ponctuel
et se scrape tel quel par Prometheus.

## 68. Cas pratique 4 — Webhook de déploiement / redémarrage

```go
// webhook/main.go : reçoit un POST signé, exécute une action (ex: reload service)
package main

import (
	"crypto/hmac"
	"crypto/sha256"
	"encoding/hex"
	"io"
	"log"
	"net/http"
	"os"
	"os/exec"
	"time"
)

func verifierSignature(secret, corps []byte, sigHex string) bool {
	mac := hmac.New(sha256.New, secret)
	mac.Write(corps)
	attendu := hex.EncodeToString(mac.Sum(nil))
	return hmac.Equal([]byte(attendu), []byte(sigHex))
}

func main() {
	secret := os.Getenv("WEBHOOK_SECRET")
	if secret == "" {
		log.Fatal("WEBHOOK_SECRET requis")
	}
	http.HandleFunc("POST /redeploy", func(w http.ResponseWriter, r *http.Request) {
		r.Body = http.MaxBytesReader(w, r.Body, 1<<20)
		corps, _ := io.ReadAll(r.Body)
		if !verifierSignature([]byte(secret), corps, r.Header.Get("X-Signature")) {
			http.Error(w, "signature invalide", http.StatusUnauthorized)
			return
		}
		// Exécution asynchrone : répondre vite, agir ensuite
		go func() {
			cmd := exec.Command("systemctl", "reload", "monapp")
			if out, err := cmd.CombinedOutput(); err != nil {
				log.Printf("reload: %v: %s", err, out)
			} else {
				log.Println("service rechargé")
			}
		}()
		w.WriteHeader(http.StatusAccepted)
	})
	srv := &http.Server{Addr: "127.0.0.1:8080", ReadTimeout: 5 * time.Second}
	log.Fatal(srv.ListenAndServe())
}
```

Sécurité du webhook : signature HMAC, bind local + reverse proxy, taille limitée,
pas d'args issus du corps dans `exec` (commande figée ici).

## 69. Cas pratique 5 — Inventaire JSON → rapport

```go
// rapport/main.go : lit inventaire.json, sort un rapport texte trié
package main

import (
	"encoding/json"
	"fmt"
	"log"
	"os"
	"sort"
)

type Hote struct {
	Nom     string   `json:"nom"`
	IP      string   `json:"ip"`
	Roles   []string `json:"roles"`
	EnLigne bool     `json:"en_ligne"`
}

func main() {
	data, err := os.ReadFile("inventaire.json")
	if err != nil {
		log.Fatal(err)
	}
	var hotes []Hote
	if err := json.Unmarshal(data, &hotes); err != nil {
		log.Fatal(err)
	}
	sort.Slice(hotes, func(i, j int) bool { return hotes[i].Nom < hotes[j].Nom })

	parRole := map[string]int{}
	hs := 0
	for _, h := range hotes {
		if !h.EnLigne {
			hs++
		}
		for _, r := range h.Roles {
			parRole[r]++
		}
	}
	fmt.Printf("Hôtes: %d (hors-ligne: %d)\n", len(hotes), hs)
	fmt.Println("Par rôle:")
	roles := make([]string, 0, len(parRole))
	for r := range parRole {
		roles = append(roles, r)
	}
	sort.Strings(roles)
	for _, r := range roles {
		fmt.Printf("  %-10s %d\n", r, parRole[r])
	}
}
```

## 70. Déploiement : binaire statique, systemd, Docker

**Systemd** — `/etc/systemd/system/monoutil.service` :
```ini
[Unit]
Description=Mon outil Go
After=network.target

[Service]
Type=simple
User=monoutil
Environment=MONOUTIL_TIMEOUT=10s
ExecStart=/opt/monoutil/monoutil sonde web-01
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```
```bash
sudo systemctl daemon-reload && sudo systemctl enable --now monoutil
```

**Docker multi-stage** (binaire statique → image ~10 Mo) :
```dockerfile
FROM golang:1.23-alpine AS build
WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -ldflags="-s -w" -o /monoutil .

FROM scratch
COPY --from=build /monoutil /monoutil
# Si HTTPS : COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
ENTRYPOINT ["/monoutil"]
```

Checklist mise en prod :
- [ ] Build reproductible : tag git → `-X main.version=...`
- [ ] `govulncheck ./...` propre
- [ ] Config par env/fichier (jamais recompilé par environnement)
- [ ] Logs JSON sur stdout, niveau réglable (`slog.Level`)
- [ ] Healthcheck (`/sante`) + arrêt gracieux SIGTERM (section 52)
- [ ] Utilisateur dédié, pas root ; droits fichiers minimaux

## 71. Pense-bête de poche

```
go run .                          exécuter
go build -ldflags="-s -w" -o app . compiler léger
go vet ./... && gofmt -l .         hygiène
go test -race -cover ./...         tests stricts
go mod tidy                        nettoyer les déps

var x int = 0      x := 0          déclaration (:= en fonction)
const Pi = 3.14                    constante
s := []int{1,2}                    slice
m := map[string]int{}              map (make ! jamais nil en écriture)
s = append(s, 3)                   TOUJOURS réaffecter
for i, v := range s                boucle
if err != nil { return ... }       gestion d'erreur
defer f.Close()                    nettoyage garanti
go f()                             goroutine
ch := make(chan int, 10)           channel bufferisé
select { case v := <-ch: ... }     multiplexage
ctx, cancel := context.WithTimeout  annulation
defer cancel()
wg.Add(1); go func(){defer wg.Done()}(); wg.Wait()

Serveur{IP: "x"}                   struct (champs nommés)
func (s *Serveur) M()              méthode (pointeur si mutation)
type I interface{ M() error }      interface (implicite)
v.(string) / switch v := v.(type)  assertion / type switch
fmt.Errorf("...: %w", err)         wrap d'erreur
errors.Is / errors.As             inspection
json.Marshal / json.Unmarshal     JSON (champs exportés !)
time.Parse("2006-01-02", s)       date de référence !
GOOS=windows go build             compilation croisée
```

## 72. Glossaire

| Terme | Sens |
|---|---|
| Binaire statique | Exécutable sans dépendance externe (libc incluse via `CGO_ENABLED=0`) |
| Channel | File typée de communication entre goroutines |
| Closure | Fonction capturant les variables de son environnement |
| `defer` | Appel reporté à la fin de la fonction (LIFO) |
| errgroup | Groupe de goroutines avec propagation d'erreur/annulation (`x/sync`) |
| go.mod / go.sum | Manifeste du module / empreintes des dépendances |
| GOROOT / GOPATH / GOCACHE | Install Go / espace de travail / cache de build |
| Goroutine | Fonction s'exécutant en parallèle, légère (~8 Ko) |
| Interface | Contrat de méthodes, implémentation implicite |
| `nil` | Valeur zéro des pointeurs, slices, maps, channels, interfaces |
| Pointeur | Adresse mémoire (`&v` / `*p`), pas d'arithmétique en Go |
| Race (data race) | Accès concurrents non synchronisés — détecté par `-race` |
| Receveur | Le `(s *Serveur)` devant une méthode |
| Rune | Point de code Unicode (alias `int32`) |
| Slice | Vue (ptr+len+cap) sur un tableau sous-jacent |
| Struct | Type composite à champs nommés |
| Vendoring | Copie des déps dans `vendor/` (`go mod vendor`) |
| Vet | Analyse statique de code suspect (`go vet`) |
| Wrap (erreur) | Enrichir en conservant la cause (`%w`, `Unwrap`) |

## 73. Quiz : 10 questions + réponses

**Q1.** Que vaut `fmt.Println(7/3)` et pourquoi ?
> `2` : division entière entre deux `int`, la partie décimale est tronquée.

**Q2.** Quelle est l'erreur dans `append(s, 42)` utilisé seul ?
> `append` peut réallouer : le résultat doit être réaffecté — `s = append(s, 42)`.

**Q3.** Pourquoi ce code panique-t-il : `var m map[string]int; m["a"] = 1` ?
> La map est `nil` : lecture ok, mais l'écriture sur une map nil panique. Initialiser avec `make` ou un littéral.

**Q4.** `defer` : dans quel ordre s'exécutent plusieurs appels différés ?
> LIFO : le dernier `defer` enregistré s'exécute en premier.

**Q5.** Un `main` qui lance `go traiter()` puis se termine immédiatement : que se passe-t-il ?
> La fin de `main` tue toutes les goroutines : `traiter` peut ne jamais s'exécuter. Synchroniser avec `WaitGroup`/`errgroup`.

**Q6.** Comment propager proprement une erreur en ajoutant du contexte ?
> `return fmt.Errorf("contexte: %w", err)` — `%w` conserve la chaîne pour `errors.Is`/`errors.As`.

**Q7.** Channel non bufferisé : que se passe-t-il à `ch <- v` sans récepteur ?
> L'envoi bloque jusqu'à ce qu'un récepteur soit prêt (rendez-vous).

**Q8.** À quoi sert `context.WithTimeout` + `defer cancel()` ?
> Borner la durée d'une opération et libérer les ressources associées (timer, goroutines).

**Q9.** Commande pour compiler pour Windows ARM64 depuis Linux ?
> `GOOS=windows GOARCH=arm64 go build -o app.exe .`

**Q10.** Pourquoi `resp.Body.Close()` est-il obligatoire après un `client.Do` ?
> Sinon la connexion n'est pas rendue au pool : fuite de sockets/fds, puis épuisement.

## 74. Pour aller plus loin

**Documentation officielle**
- https://go.dev/doc/ — tutoriels et guides
- https://pkg.go.dev/ — référence des paquets
- *Effective Go* (go.dev/doc/effective_go) et *Go Wiki*

**Livres**
- *The Go Programming Language* (Donovan & Kernighan) — la référence
- *Concurrency in Go* (Cox-Buday) — goroutines/channels en profondeur
- *Go in Action* / *100 Go Mistakes* (Teiva Harsanyi) — les pièges

**Outils à adopter**
- `golangci-lint` — méta-linter (vet + dizaines de règles)
- `govulncheck` — vulnérabilités des dépendances
- `delve` — debugueur, `go tool pprof` — profiling
- Cobra/Viper (CLI), `client_golang` (Prometheus), Testify (assertions)

**Feuille de route proposée (4 semaines, 30 min/jour)**
1. Semaine 1 : sections 1–21 — réécrire 2 scripts shell en Go
2. Semaine 2 : sections 22–42 — structs, erreurs, fichiers, JSON
3. Semaine 3 : sections 38–46 — concurrence : la sonde parallèle sur le parc réel
4. Semaine 4 : sections 47–56, 70 — API interne + binaire déployé via systemd

> Dernier conseil : Go récompense la simplicité explicite. Quand vous hésitez
> entre une abstraction élégante et du code direct, choisissez le code direct —
> c'est l'esprit du langage, et votre vous-dans-6-mois vous remerciera.
