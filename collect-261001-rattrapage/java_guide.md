# Guide Java — du zéro au production

> **Public visé** : administrateurs systèmes, responsables d'équipe technique et développeurs d'outils internes d'entreprise.
> **Niveau** : débutant à intermédiaire confirmé. **Version cible** : Java 17+ (LTS). Les nouveautés Java 21 sont signalées explicitement.
> **Ton** : direct, tutoriel + référence. Chaque section se termine par un point « À retenir » quand c'est utile.

---

## 1. Pourquoi Java en 2026 ?

Java reste le pilier des applications d'entreprise : banques, opérateurs télécoms, éditeurs de supervision, outils internes critiques. Pourquoi il vous concerne, vous, chef de service systèmes & énergies :

- **Portabilité réelle** : « write once, run anywhere » — un jar tourne sur Linux, Windows, conteneurs Docker sans recompilation.
- **Écosystème mature** : Maven Central contient des millions de bibliothèques (JDBC, SNMP, Modbus, LDAP, etc.).
- **Longévité** : les LTS (8, 11, 17, 21, 25) garantissent des années de support. Un outil interne écrit aujourd'hui tournera dans 10 ans.
- **Performance** : la JVM (JIT, GC modernes) rivalise avec le natif pour la plupart des charges métier.
- **Spring Boot** : l'écosystème dominant pour exposer vos outils en API REST / dashboards web.

**À retenir** : pour des outils internes (inventaires, collecteurs SNMP, batchs d'import, petites API), Java + Spring Boot est un choix sûr et durable.

---

## 2. Installation du JDK

### 2.1 Choisir une distribution

| Distribution | Fournisseur | Licence | Remarque |
|---|---|---|---|
| Temurin | Eclipse Adoptium | Gratuite (GPL+CE) | Choix recommandé, builds testés |
| Oracle JDK | Oracle | Gratuite sous NFTC | OK pour dev, vérifier les conditions en prod |
| Corretto | Amazon | Gratuite | Bien pour AWS |
| Zulu | Azul | Gratuite (communauté) | Large choix de versions |
| GraalVM | Oracle | Gratuite | Compilation native, polyglotte |

**Règle** : en entreprise, privilégiez une LTS : **Java 17** (supportée jusqu'en 2029+) ou **Java 21** (jusqu'en 2031+).

### 2.2 Installation sur Debian/Ubuntu

```bash
# Temurin via le dépôt Adoptium
sudo apt install -y wget apt-transport-https gpg
wget -qO - https://packages.adoptium.net/artifactory/api/gpg/key/public | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/adoptium.gpg > /dev/null
echo "deb https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
sudo apt update && sudo apt install -y temurin-21-jdk

# Vérification
java -version
javac -version
```

### 2.3 Installation sur Windows

1. Téléchargez l'installeur `.msi` Temurin 21 depuis adoptium.net.
2. Cochez « Set JAVA_HOME » et « Add to PATH » pendant l'installation.
3. Vérifiez dans PowerShell : `java -version`.

### 2.4 Variables d'environnement

```bash
# ~/.bashrc ou ~/.profile
export JAVA_HOME=/usr/lib/jvm/temurin-21-jdk-amd64
export PATH="$JAVA_HOME/bin:$PATH"
```

Vérifiez que `javac` pointe bien vers la bonne version : `which javac` puis `javac -version`.

### 2.5 JDK vs JRE

- **JDK** (Java Development Kit) : compilateur `javac` + outils + runtime. **C'est ce qu'il faut installer pour développer.**
- **JRE** : runtime seul (n'existe plus en distribution séparée depuis Java 11 ; on génère un runtime sur mesure avec `jlink`/`jpackage`, voir section 48).

**À retenir** : installez toujours un JDK LTS (17 ou 21), jamais « la dernière version » non-LTS pour de la production.

---

## 3. Tooling : Maven, Gradle, IDE

### 3.1 Maven (recommandé pour débuter)

Maven gère les dépendances et le cycle de build via un fichier `pom.xml`.

```bash
# Installation Debian/Ubuntu
sudo apt install -y maven
mvn -version
```

Créer un projet :

```bash
mvn archetype:generate -DgroupId=com.entreprise -DartifactId=outil-inventaire \
  -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
cd outil-inventaire
mvn compile     # compile
mvn package     # produit target/outil-inventaire-1.0-SNAPSHOT.jar
mvn test        # lance les tests
```

`pom.xml` minimal avec Java 21 :

```xml
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.entreprise</groupId>
  <artifactId>outil-inventaire</artifactId>
  <version>1.0.0</version>
  <properties>
    <maven.compiler.release>21</maven.compiler.release>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>
  <dependencies>
    <dependency>
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter</artifactId>
      <version>5.10.3</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

### 3.2 Gradle (alternative moderne)

```bash
# Via SDKMAN (recommandé)
curl -s "https://get.sdkman.io" | bash
sdk install gradle
gradle init   # assistant interactif
./gradlew build
```

`build.gradle` minimal (syntaxe Kotlin DSL) :

```kotlin
plugins { java }
java { toolchain { languageVersion.set(JavaLanguageVersion.of(21)) } }
repositories { mavenCentral() }
dependencies { testImplementation("org.junit.jupiter:junit-jupiter:5.10.3") }
tasks.test { useJUnitPlatform() }
```

### 3.3 IDE : IntelliJ IDEA (recommandé)

- **IntelliJ IDEA Community** (gratuit) : le meilleur support Java/Maven/Gradle. Refactoring, debug, inspections.
- **Eclipse** : gratuit, historique, encore très présent en entreprise.
- **VS Code** + extension « Extension Pack for Java » : léger, correct pour petits projets.

**Conseil** : prenez IntelliJ IDEA Community. Le gain de productivité (complétion, corrections automatiques) vaut largement l'installation.

### 3.4 Outils en ligne de commande du JDK à connaître

| Outil | Rôle |
|---|---|
| `javac` | Compilateur |
| `java` | Lanceur d'applications |
| `jar` | Manipulation d'archives jar |
| `javadoc` | Génération de documentation |
| `jshell` | REPL interactif (idéal pour tester un bout de code) |
| `jconsole` / `jvisualvm` | Supervision JVM (threads, heap) |
| `jstat`, `jmap`, `jstack` | Diagnostic production |
| `jlink`, `jpackage` | Runtimes sur mesure et installateurs |

Essayez `jshell` dès maintenant : c'est le bac à sable parfait pour ce guide.

```text
$ jshell
|  Welcome to JShell -- Version 21
jshell> int x = 40 + 2;
x ==> 42
jshell> System.out.println("Hello " + x);
Hello 42
```

**À retenir** : JDK Temurin 21 + Maven + IntelliJ IDEA Community = stack de départ saine et gratuite.

---

## 4. Premier programme : compilation et exécution

Fichier `Hello.java` (le nom du fichier **doit** correspondre à la classe publique) :

```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Bonjour, Java !");
        if (args.length > 0) {
            System.out.println("Argument reçu : " + args[0]);
        }
    }
}
```

```bash
javac Hello.java        # produit Hello.class (bytecode)
java Hello              # exécute
java Hello monde        # avec un argument
```

Depuis Java 11, on peut lancer un fichier source directement (pratique pour les scripts) :

```bash
java Hello.java monde
```

**Anatomie** :
- `public class Hello` : tout le code Java vit dans des classes.
- `public static void main(String[] args)` : point d'entrée du programme.
- `System.out.println(...)` : affiche sur la sortie standard.

**À retenir** : un fichier = une classe publique du même nom ; `main` est la porte d'entrée.

---

## 5. Syntaxe de base : le squelette du langage

```java
// Commentaire sur une ligne
/* Commentaire
   sur plusieurs lignes */
/** Commentaire Javadoc (génère de la doc) */

package com.entreprise.outil;   // doit être la 1re instruction (hors commentaires)

import java.util.ArrayList;     // import d'une classe
import java.util.*;             // import d'un package (évitez * en équipe)

public class Syntaxe {
    // Attribut (champ) de classe
    private static final double TVA = 0.20;

    // Méthode
    public static double prixTTC(double prixHT) {
        return prixHT * (1 + TVA);
    }
}
```

Règles de base :
- Instructions terminées par `;`.
- Blocs délimités par `{ }`.
- Sensible à la casse : `prix` ≠ `Prix`.
- Conventions de nommage : `MaClasse` (PascalCase), `maMethode`/`maVariable` (camelCase), `MA_CONSTANTE` (UPPER_SNAKE).

---

## 6. Types primitifs et variables

| Type | Taille | Plage / valeurs | Défaut (attribut) |
|---|---|---|---|
| `byte` | 8 bits | -128 à 127 | 0 |
| `short` | 16 bits | -32 768 à 32 767 | 0 |
| `int` | 32 bits | ±2,1 milliards | 0 |
| `long` | 64 bits | ±9×10¹⁸ | 0L |
| `float` | 32 bits | ~±3,4×10³⁸ | 0.0f |
| `double` | 64 bits | ~±1,8×10³⁰⁸ | 0.0 |
| `char` | 16 bits | caractère Unicode | '\u0000' |
| `boolean` | — | `true` / `false` | false |

```java
int compteur = 0;
long population = 8_000_000_000L;   // _ = séparateur de lisibilité, L obligatoire
double prix = 19.99;
float ratio = 0.5f;                  // f obligatoire, sinon double
char lettre = 'A';
boolean actif = true;

var nom = "Zelef";        // inférence de type (Java 10+) : nom est un String
// var x;                 // INTERDIT : var exige une initialisation immédiate

final int PORT = 8080;    // constante : ne peut plus être réassignée
```

**Pièges** :
- `int / int` = division **entière** : `5 / 2` vaut `2`, pas `2.5`. Écrivez `5 / 2.0`.
- Dépassement silencieux : `int` qui déborde ne lève **aucune** erreur (il « boucle »). Pour des calculs critiques, utilisez `long` ou `Math.addExact` (lève `ArithmeticException` en cas de débordement).
- N'utilisez **jamais** `float`/`double` pour de la monnaie : utilisez `BigDecimal` (voir section 35).

**À retenir** : `int`/`long`/`double`/`boolean` couvrent 95 % des besoins ; `var` allège le code mais n'est pas du typage dynamique.

---

## 7. Opérateurs

```java
int a = 10, b = 3;
System.out.println(a + b);  // 13
System.out.println(a - b);  // 7
System.out.println(a * b);  // 30
System.out.println(a / b);  // 3  (division entière !)
System.out.println(a % b);  // 1  (modulo = reste)

a++; b--;                   // incrémentation / décrémentation
a += 5;                     // a = a + 5 ; idem -= *= /= %=

boolean ok = (a > 5) && (b < 10);  // && et || sont "court-circuit" : la suite n'est pas évaluée si inutile
boolean ko = (a > 5) & (b < 10);   // & évalue toujours les deux côtés (rarement voulu sur des booléens)

int max = (a > b) ? a : b;         // ternaire

// Opérateurs bit à bit (masques, flags, protocoles réseau)
int flags = 0b1010;
int masque = flags & 0b1100;       // ET bit à bit
int ajout  = flags | 0b0001;        // OU bit à bit
int bascule= flags ^ 0b1111;        // OU exclusif
int decale = flags << 2;           // décalage à gauche (= ×4)
```

Priorités à connaître : `*` `/` `%` avant `+` `-` ; comparaisons avant `&&` avant `||`. En cas de doute : **parenthèses**.

---

## 8. Chaînes de caractères : String

`String` est **immutable** : chaque « modification » crée un nouvel objet.

```java
String s = "Bonjour";
String t = "Bonjour";
System.out.println(s == t);        // true ici (pool de String), mais...
System.out.println(s.equals(t));   // true — TOUJOURS comparer avec equals() !

String nom = new String("test");
System.out.println(s == nom);      // false ! (objets différents)
System.out.println(s.equals(nom)); // true

// Concaténation
String msg = "Valeur : " + 42 + " €";   // "Valeur : 42 €"

// Méthodes essentielles
"  hello  ".trim();                // "hello"  (Java 11+ : strip() gère mieux Unicode)
"hello".toUpperCase();             // "HELLO"
"hello".length();                  // 5
"hello".charAt(1);                 // 'e'
"hello".substring(1, 4);           // "ell"
"hello world".contains("world");   // true
"hello".startsWith("he");          // true
"a,b,c".split(",");                // ["a","b","c"]
String.join(", ", "a", "b", "c");  // "a, b, c"
"hello".replace('l', 'L');         // "heLLo"

// Text blocks (Java 15+) : chaînes multilignes propres
String json = """
    {
      "nom": "onduleur-01",
      "puissance_kva": 40
    }
    """;
System.out.println(json);
```

**Concaténer en boucle = piège de performance** : chaque `+` crée un objet. Utilisez `StringBuilder` :

```java
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 10000; i++) {
    sb.append("ligne ").append(i).append('\n');
}
String resultat = sb.toString();
```

**À retenir** : `equals()` pour comparer, jamais `==` ; `StringBuilder` pour construire en boucle ; text blocks pour le multiligne.

---

## 9. Structures de contrôle : if / switch

```java
int note = 14;
if (note >= 16) {
    System.out.println("Très bien");
} else if (note >= 10) {
    System.out.println("Passable");
} else {
    System.out.println("Insuffisant");
}

// Switch expression (Java 14+) : retourne une valeur, sans break oublié
String mention = switch (note / 4) {
    case 5, 4 -> "Excellent";
    case 3 -> "Bien";
    case 2 -> "Passable";
    default -> "Insuffisant";
};

// Pattern matching dans switch (Java 21+)
static String decrire(Object o) {
    return switch (o) {
        case Integer i -> "entier : " + i;
        case String s  -> "chaîne de " + s.length() + " caractères";
        case null      -> "rien (null)";
        default        -> "autre type : " + o.getClass().getSimpleName();
    };
}
```

Notez `case null` : depuis Java 21, un `switch` sur `null` ne lève plus forcément `NullPointerException` si vous gérez le cas.

---

## 10. Boucles

```java
// for classique
for (int i = 0; i < 5; i++) {
    System.out.println("i = " + i);
}

// for-each (à privilégier sur les collections/tableaux)
String[] baies = {"baie-A", "baie-B", "baie-C"};
for (String baie : baies) {
    System.out.println(baie);
}

// while
int n = 3;
while (n > 0) {
    System.out.println(n);
    n--;
}

// do-while : exécuté au moins une fois
int code;
do {
    code = lireCodePin();
} while (code != 1234);

// break / continue avec labels (rare mais utile pour boucles imbriquées)
externe:
for (int i = 0; i < 10; i++) {
    for (int j = 0; j < 10; j++) {
        if (i * j > 50) break externe;
    }
}
```

**Boucle infinie volontaire** (serveur, worker) : `while (true) { ... }` avec une condition de sortie propre.

---

## 11. Tableaux

```java
int[] puissances = new int[4];          // [0,0,0,0]
int[] notes = {10, 20, 30};             // initialisation directe
String[] noms = new String[3];

puissances[0] = 10;
System.out.println(notes.length);       // 3 (attribut, pas méthode !)
System.out.println(notes[notes.length - 1]); // dernier élément : 30

// Parcours
for (int i = 0; i < notes.length; i++) System.out.println(notes[i]);
for (int note : notes) System.out.println(note);

// Tableaux multidimensionnels
int[][] matrice = new int[3][3];
matrice[1][2] = 7;
int[][] triangle = { {1}, {2, 3}, {4, 5, 6} };

// Utilitaires
java.util.Arrays.sort(notes);
System.out.println(java.util.Arrays.toString(notes)); // [10, 20, 30]
int[] copie = java.util.Arrays.copyOf(notes, notes.length);
```

**Limites** : taille fixe à la création ; `notes[5]` sur un tableau de 3 → `ArrayIndexOutOfBoundsException`. Pour une taille dynamique : `ArrayList` (section 25).

---

## 12. Méthodes (fonctions)

```java
public class Calculatrice {

    // Signature : visibilité + (static?) + type de retour + nom + paramètres
    public static int additionner(int a, int b) {
        return a + b;
    }

    // Surcharge (overloading) : même nom, paramètres différents
    public static double additionner(double a, double b) {
        return a + b;
    }

    // Nombre variable d'arguments (varargs)
    public static int somme(int... valeurs) {
        int total = 0;
        for (int v : valeurs) total += v;
        return total;
    }
    // somme(1, 2, 3) -> 6 ; somme() -> 0

    // Passage par valeur : Java passe TOUJOURS par valeur
    public static void essayer(int x) { x = 99; }  // sans effet dehors
    public static void remplir(int[] t) { t[0] = 99; } // ...mais la valeur copiée peut être une référence !

    public static void main(String[] args) {
        int x = 5;
        essayer(x);
        System.out.println(x); // 5 : inchangé
        int[] t = {1, 2};
        remplir(t);
        System.out.println(t[0]); // 99 : le contenu du tableau a changé
    }
}
```

**À retenir** : Java est *pass-by-value* ; pour un objet, c'est la **référence** qui est copiée — d'où la confusion classique.

### Checklist « méthode propre »

- [ ] Nom = verbe d'action (`calculerTotal`, pas `truc`)
- [ ] Une méthode = une responsabilité (viser < 30 lignes)
- [ ] Paramètres ≤ 4 (au-delà : objet dédié ou builder)
- [ ] Retour explicite plutôt que `null` quand c'est possible (`Optional`, liste vide)
- [ ] Javadoc sur les méthodes publiques d'une bibliothèque

---

## 13. Classes et objets : les fondamentaux

La programmation orientée objet repose sur 4 piliers : **encapsulation, héritage, polymorphisme, abstraction**. Tout le reste du guide en découle.

```java
public class Onduleur {
    // Attributs (état)
    private String nom;
    private double puissanceKva;
    private boolean enService;

    // Constructeur (section 15)
    public Onduleur(String nom, double puissanceKva) {
        this.nom = nom;
        this.puissanceKva = puissanceKva;
        this.enService = true;
    }

    // Méthodes (comportement)
    public void mettreHorsService() {
        this.enService = false;
    }

    public String decrire() {
        return nom + " (" + puissanceKva + " kVA) - " +
               (enService ? "en service" : "hors service");
    }

    public static void main(String[] args) {
        Onduleur o1 = new Onduleur("UPS-Baie-A", 40.0);  // instanciation
        System.out.println(o1.decrire());
        o1.mettreHorsService();
        System.out.println(o1.decrire());
    }
}
```

Vocabulaire : **classe** = moule ; **objet** (instance) = exemplaire ; **attribut** = donnée ; **méthode** = action ; `this` = l'instance courante.

---

## 14. Constructeurs

```java
public class Batterie {
    private final String technologie;  // final : assigné une seule fois
    private final int capaciteAh;
    private int cycles;

    // Constructeur principal
    public Batterie(String technologie, int capaciteAh) {
        if (capaciteAh <= 0) throw new IllegalArgumentException("capacité > 0 requise");
        this.technologie = technologie;
        this.capaciteAh = capaciteAh;
    }

    // Constructeur secondaire qui délègue avec this(...)
    public Batterie(int capaciteAh) {
        this("VRLA", capaciteAh);  // doit être la PREMIÈRE instruction
    }

    // Pas de constructeur écrit => Java génère un constructeur par défaut sans argument.
    // Dès qu'on en écrit un, le défaut disparaît !
}
```

**Bonnes pratiques** : validez les arguments (fail-fast avec `IllegalArgumentException`) ; préférez des objets **immutables** (`final` partout) quand c'est possible — ils sont thread-safe par construction.

---

## 15. Encapsulation : private + getters/setters

```java
public class CompteurEnergie {
    private double kwh;   // inaccessible directement de l'extérieur

    public double getKwh() { return kwh; }

    public void ajouter(double delta) {
        if (delta < 0) throw new IllegalArgumentException("delta négatif interdit");
        this.kwh += delta;
    }
    // Pas de setKwh() : l'état ne change que via une règle métier.
}
```

L'encapsulation n'est pas de la paranoïa : c'est ce qui permet de **changer l'implémentation interne sans casser le code client** (ex. passer de `double` à `BigDecimal` sans toucher les appelants). Dans IntelliJ : `Alt+Insert` → Getters/Setters.

---

## 16. Héritage : extends

```java
public class Equipement {
    protected String nom;              // visible par les sous-classes
    public Equipement(String nom) { this.nom = nom; }
    public String decrire() { return "Équipement : " + nom; }
}

public class Onduleur extends Equipement {
    private double puissanceKva;

    public Onduleur(String nom, double puissanceKva) {
        super(nom);                    // appel OBLIGATOIRE au constructeur parent (1re ligne)
        this.puissanceKva = puissanceKva;
    }

    @Override                          // annotation : le compilateur vérifie la redéfinition
    public String decrire() {
        return super.decrire() + " - " + puissanceKva + " kVA";
    }
}
```

Règles :
- Une classe n'hérite que d'**une** seule classe (pas d'héritage multiple de classes).
- `final class` = non héritable ; `final` sur une méthode = non redéfinissable.
- **Préférez la composition à l'héritage** : héritez pour un vrai « est-un » (`Onduleur` est un `Equipement`), composez pour un « a-un » (`Onduleur` *a* une `Batterie`).

---

## 17. Polymorphisme

```java
public class Supervision {
    public static void afficher(Equipement e) {
        // Le bon decrire() est choisi À L'EXÉCUTION selon le type réel : liaison dynamique
        System.out.println(e.decrire());
    }

    public static void main(String[] args) {
        Equipement e1 = new Equipement("Switch");
        Equipement e2 = new Onduleur("UPS-01", 40.0);  // référence parent, objet enfant
        afficher(e1);  // Équipement : Switch
        afficher(e2);  // Équipement : UPS-01 - 40.0 kVA  (version Onduleur !)
    }
}
```

C'est le cœur des architectures extensibles : votre code de supervision manipule `Equipement`, et chaque nouvel équipement n'exige **aucune** modification de ce code (principe ouvert/fermé).

---

## 18. Classes abstraites et interfaces

```java
// Classe abstraite : base incomplète, ne peut pas être instanciée
public abstract class Capteur {
    protected String id;
    public Capteur(String id) { this.id = id; }
    public abstract double lire();          // les enfants DOIVENT l'implémenter
    public String getId() { return id; }    // méthode concrète partagée
}

// Interface : contrat pur (une classe peut en implémenter PLUSIEURS)
public interface Alertable {
    void alerter(String message);
    default void alerterCritique(String message) {   // méthode par défaut (Java 8+)
        alerter("[CRITIQUE] " + message);
    }
}

public class CapteurTemperature extends Capteur implements Alertable {
    public CapteurTemperature(String id) { super(id); }
    @Override public double lire() { return 21.5; /* lecture réelle ici */ }
    @Override public void alerter(String message) { System.out.println(id + " : " + message); }
}
```

| | Classe abstraite | Interface |
|---|---|---|
| Instanciable | Non | Non |
| Héritage multiple | Non (1 seule) | Oui (plusieurs) |
| Attributs d'instance | Oui | Non (constantes uniquement) |
| Constructeur | Oui | Non |
| Cas d'usage | Base commune avec état partagé | Contrat / capacité (`Alertable`, `Comparable`) |

**Depuis Java 8**, les interfaces peuvent avoir des méthodes `default` et `static` ; utilisez-les pour faire évoluer un contrat sans casser les implémentations existantes.

---

## 19. Énumérations (enum)

Bien plus qu'une liste de constantes : une `enum` Java est une vraie classe.

```java
public enum EtatEquipement {
    EN_SERVICE("Opérationnel", 1),
    DEGRADE("Fonctionnement dégradé", 2),
    HORS_SERVICE("Arrêté", 3);

    private final String libelle;
    private final int criticite;

    EtatEquipement(String libelle, int criticite) {
        this.libelle = libelle;
        this.criticite = criticite;
    }

    public String getLibelle() { return libelle; }
    public boolean estCritique() { return criticite >= 3; }
}

// Usage
EtatEquipement etat = EtatEquipement.DEGRADE;
System.out.println(etat.getLibelle());       // Fonctionnement dégradé
System.out.println(etat.ordinal());          // 1 (position, évitez de vous en servir en persistance)
System.out.println(etat.name());             // "DEGRADE"

for (EtatEquipement e : EtatEquipement.values()) System.out.println(e);
EtatEquipement x = EtatEquipement.valueOf("EN_SERVICE"); // depuis une chaîne (lève IllegalArgumentException si inconnu)

// Les enum se comparent avec == sans risque (singletons garantis)
if (etat == EtatEquipement.HORS_SERVICE) { /* ... */ }
```

**À retenir** : remplacez vos constantes `int`/`String` magiques par des enums typées — le compilateur devient votre garde-fou.

---

## 20. Records : des classes de données compactes (Java 16+)

```java
// Équivaut à une classe immutable avec constructeur, getters, equals, hashCode, toString
public record Mesure(String capteurId, double valeur, java.time.Instant horodatage) {}

Mesure m = new Mesure("temp-01", 21.5, java.time.Instant.now());
System.out.println(m.capteurId());  // accesseurs = nom du composant, pas getXxx()
System.out.println(m);              // Mesure[capteurId=temp-01, valeur=21.5, horodatage=...]

// Record avec validation dans le constructeur compact
public record PlageIp(String cidr) {
    public PlageIp {
        if (!cidr.contains("/")) throw new IllegalArgumentException("CIDR invalide : " + cidr);
    }
}
```

Records = parfaits pour DTO, résultats de requêtes, messages, clés de Map. Ils sont **implicitement final** et leurs composants sont **final**.

---

## 21. Packages : organiser le code

```text
src/main/java/
└── com/entreprise/supervision/
    ├── Main.java
    ├── model/
    │   ├── Equipement.java
    │   └── Onduleur.java
    ├── service/
    │   └── SupervisionService.java
    └── util/
        └── ReseauUtil.java
```

```java
package com.entreprise.supervision.model;   // 1re ligne du fichier

import com.entreprise.supervision.util.ReseauUtil;
import java.util.List;                       // les imports suivent le package

public class Equipement { /* ... */ }
```

Conventions : nom de package en minuscules, en **reverse-DNS** (`com.entreprise.projet`) ; un package ≈ un module fonctionnel (`model`, `service`, `dao`, `web`).

---

## 22. Modules Java (JPMS, Java 9+)

Pour les grosses applications : encapsulation au niveau package + dépendances explicites.

```java
// module-info.java à la racine des sources
module com.entreprise.supervision {
    requires java.sql;              // dépendance explicite
    requires spring.boot;           // (nom automatique si pas modulaire)
    exports com.entreprise.supervision.api;   // packages visibles de l'extérieur
    // com.entreprise.supervision.impl reste interne : inaccessible !
}
```

En pratique, beaucoup de projets (dont Spring Boot) s'en passent encore. **Conseil** : commencez sans modules ; adoptez JPMS quand le projet dépasse ~50 classes ou doit exposer une API publique propre.

---

## 23. Modificateurs de visibilité

| Modificateur | Classe | Package | Sous-classe | Monde |
|---|---|---|---|---|
| `private` | ✅ | ❌ | ❌ | ❌ |
| (défaut) | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

Autres modificateurs clés :
- `static` : appartient à la classe, pas à l'instance (méthodes utilitaires, constantes, compteurs partagés).
- `final` : variable = constante ; méthode = non redéfinissable ; classe = non héritable.
- `abstract` : classe non instanciable / méthode sans corps.
- `synchronized` : verrou d'exclusion mutuelle (voir section 40).
- `volatile` : garantit la visibilité inter-threads d'une variable (pas d'atomicité !).

**Règle d'or** : visibilité la plus restrictive possible. `private` par défaut, on élargit si besoin prouvé.

---

## 24. Génériques (Generics)

```java
// Sans génériques (avant Java 5) : casts partout, erreurs à l'exécution
List brute = new ArrayList();
brute.add("texte");
Integer i = (Integer) brute.get(0);  // ClassCastException à l'exécution !

// Avec génériques : vérifié À LA COMPILATION
List<String> noms = new ArrayList<>();
noms.add("UPS-01");
// noms.add(42);  // ERREUR de compilation : c'est le but !
String premier = noms.get(0);       // pas de cast

// Méthode générique
public static <T> T premierOuNull(List<T> liste) {
    return liste.isEmpty() ? null : liste.get(0);
}

// Classe générique
public class Paire<K, V> {
    private final K cle;
    private final V valeur;
    public Paire(K cle, V valeur) { this.cle = cle; this.valeur = valeur; }
    public K getCle() { return cle; }
    public V getValeur() { return valeur; }
}
Paire<String, Double> mesure = new Paire<>("temp-01", 21.5);

// Wildcards : <? extends T> lecture, <? super T> écriture (règle PECS)
public static double somme(List<? extends Number> nombres) {
    double total = 0;
    for (Number n : nombres) total += n.doubleValue();
    return total;
}
somme(List.of(1, 2.5, 3L));  // OK : Integer, Double, Long sont des Number
```

**Type erasure** : les génériques n'existent qu'à la compilation ; à l'exécution, `List<String>` = `List`. Conséquence : impossible de faire `new T()` ou `instanceof List<String>`.

**À retenir** : génériques partout où un conteneur/algorithme est indépendant du type contenu — zéro cast, erreurs détectées tôt.

---

## 25. Collections : vue d'ensemble

| Interface | Implémentations | Ordonné | Doublons | `null` | Accès |
|---|---|---|---|---|---|
| `List` | `ArrayList`, `LinkedList` | Oui (insertion) | Oui | Oui | Index |
| `Set` | `HashSet`, `LinkedHashSet`, `TreeSet` | Selon impl. | Non | 1 (sauf TreeSet) | — |
| `Map` | `HashMap`, `LinkedHashMap`, `TreeMap` | Selon impl. | Clés uniques | Oui (sauf TreeMap) | Clé |
| `Queue`/`Deque` | `ArrayDeque`, `PriorityQueue` | FIFO/priorité | Oui | Non (ArrayDeque) | Tête/queue |

```java
// Fabriques immutables (Java 9+) : parfaites pour constantes et tests
List<String> baies = List.of("A", "B", "C");       // immutable ! add() -> UnsupportedOperationException
Set<String> roles = Set.of("admin", "tech");
Map<String, Integer> ports = Map.of("http", 80, "https", 443);

// Copie défensive
List<String> modifiable = new ArrayList<>(baies);
```

**Choisir la bonne implémentation** : `ArrayList` par défaut ; `HashMap`/`HashSet` par défaut ; `TreeMap`/`TreeSet` si tri naturel nécessaire ; `LinkedHashMap` si ordre d'insertion à préserver (caches LRU).

---

## 26. List : ArrayList en détail

```java
List<String> onduleurs = new ArrayList<>();
onduleurs.add("UPS-01");
onduleurs.add("UPS-02");
onduleurs.add(0, "UPS-00");            // insertion à l'index 0
System.out.println(onduleurs.get(1));  // UPS-01
System.out.println(onduleurs.size()); // 3
onduleurs.set(1, "UPS-01B");           // remplacement
onduleurs.remove("UPS-02");           // par valeur
onduleurs.remove(0);                  // par index
System.out.println(onduleurs.contains("UPS-01B")); // true

// Tri
onduleurs.sort(String::compareTo);              // naturel
onduleurs.sort(Comparator.comparingInt(String::length)); // par longueur

// Conversion tableau <-> liste
String[] tableau = onduleurs.toArray(new String[0]);
List<String> depuis = new ArrayList<>(Arrays.asList(tableau));
```

`ArrayList` = tableau redimensionnable : accès index en O(1), insertion au milieu en O(n). `LinkedList` = liste chaînée : insertion/suppression en tête en O(1), mais accès index en O(n) — **rarement le bon choix** en pratique.

---

## 27. Set : unicité garantie

```java
Set<String> ipsVues = new HashSet<>();
ipsVues.add("10.0.0.1");
ipsVues.add("10.0.0.1");   // ignoré silencieusement
System.out.println(ipsVues.size()); // 1

Set<String> ordonne = new TreeSet<>();        // trié (éléments Comparable)
Set<String> insertion = new LinkedHashSet<>(); // ordre d'insertion préservé

// Opérations ensemblistes
Set<String> a = new HashSet<>(Set.of("1", "2", "3"));
Set<String> b = new HashSet<>(Set.of("3", "4"));
a.retainAll(b);  // intersection : a = {"3"}
```

**Contrat crucial** : si vous mettez vos objets dans un `HashSet`/`HashMap`, redéfinissez **toujours** `equals()` **et** `hashCode()` ensemble (les records le font gratuitement — encore une raison de les utiliser).

---

## 28. Map : HashMap en détail

```java
Map<String, String> dns = new HashMap<>();
dns.put("ups-01", "10.0.0.11");
dns.put("switch-01", "10.0.0.12");
System.out.println(dns.get("ups-01"));          // 10.0.0.11
System.out.println(dns.get("inconnu"));         // null
System.out.println(dns.getOrDefault("inconnu", "0.0.0.0")); // 0.0.0.0

// Parcours
for (Map.Entry<String, String> e : dns.entrySet()) {
    System.out.println(e.getKey() + " -> " + e.getValue());
}
dns.forEach((nom, ip) -> System.out.println(nom + " -> " + ip));

// Méthodes modernes très utiles
dns.putIfAbsent("ups-01", "x");                 // n'écrase pas l'existant
dns.computeIfAbsent("routeur-01", k -> resoudre(k)); // calcule seulement si absent (cache lazy)
dns.merge("compteur", 1, Integer::sum);          // accumulateur : compteur += 1

// Compter des occurrences : l'idiome merge
Map<String, Integer> occurrences = new HashMap<>();
for (String code : codesAlarme) {
    occurrences.merge(code, 1, Integer::sum);
}
```

**Performance** : `HashMap` = O(1) moyen. En concurrence : `ConcurrentHashMap` (jamais `Collections.synchronizedMap` sauf besoin spécifique).

---

## 29. Iterator et ConcurrentModificationException

```java
List<String> liste = new ArrayList<>(List.of("a", "b", "c"));

// ❌ INTERDIT : modifier pendant un for-each
for (String s : liste) {
    if (s.equals("b")) liste.remove(s);  // ConcurrentModificationException !
}

// ✅ Via l'itérateur explicite
Iterator<String> it = liste.iterator();
while (it.hasNext()) {
    if (it.next().equals("b")) it.remove();  // OK
}

// ✅ Ou en Java 8+ : removeIf
liste.removeIf(s -> s.equals("b"));
```

**À retenir** : un for-each = un itérateur caché ; toute modification structurelle pendant l'itération (hors `Iterator.remove()`) explose.

---

## 30. Exceptions : checked vs unchecked

```
Throwable
├── Error               → ne PAS catcher (OutOfMemoryError, StackOverflowError)
└── Exception
    ├── RuntimeException (UNCHECKED) → NullPointerException, IllegalArgumentException...
    └── autres (CHECKED)             → IOException, SQLException : déclaration obligatoire
```

```java
import java.io.IOException;
import java.nio.file.*;

// CHECKED : le compilateur EXIGE try/catch ou throws
public static String lire(Path p) throws IOException {
    return Files.readString(p);
}

// UNCHECKED : pas d'obligation, mais à prévenir par validation
public static double diviser(double a, double b) {
    if (b == 0) throw new IllegalArgumentException("diviseur nul");
    return a / b;
}

public static void main(String[] args) {
    try {
        System.out.println(lire(Path.of("config.txt")));
    } catch (NoSuchFileException e) {
        System.err.println("Fichier absent : " + e.getFile());
    } catch (IOException e) {
        System.err.println("Erreur I/O : " + e.getMessage());
    }
    // Ordre des catch : du PLUS spécifique au PLUS général !
}
```

**Philosophie** : checked = conditions extérieures récupérables (fichier, réseau, SQL) ; unchecked = bugs de programmation (null, argument invalide, état illégal). Ne catchez **jamais** `Exception` ou `Throwable` en bloc « pour être tranquille » — vous masquerez les bugs.

---

## 31. try-with-resources : fermeture automatique (Java 7+)

Toute ressource `AutoCloseable` (fichiers, sockets, connexions JDBC, scanners) **doit** être ouverte ainsi :

```java
// Fermeture garantie, même en cas d'exception, dans l'ordre inverse d'ouverture
try (var lecteur = Files.newBufferedReader(Path.of("journal.log"));
     var ecrivain = Files.newBufferedWriter(Path.of("resume.txt"))) {
    String ligne;
    while ((ligne = lecteur.readLine()) != null) {
        ecrivain.write(ligne.toUpperCase());
        ecrivain.newLine();
    }
} catch (IOException e) {
    System.err.println("Échec : " + e.getMessage());
}
// lecteur et ecrivain sont fermés ici, automatiquement

// Multi-catch
try {
    risquer();
} catch (IOException | IllegalStateException e) {
    System.err.println("Problème : " + e);
}
```

**À retenir** : tout `open()`/`new ...Reader/Writer/Stream/Connection` sans try-with-resources est un bug en puissance (fuite de descripteurs).

---

## 32. Créer ses propres exceptions

```java
// Exception métier checked (récupérable : ex. équipement injoignable)
public class EquipementInjoignableException extends Exception {
    public EquipementInjoignableException(String ip) {
        super("Équipement injoignable : " + ip);
    }
    public EquipementInjoignableException(String ip, Throwable cause) {
        super("Équipement injoignable : " + ip, cause);
    }
}

// Exception métier unchecked (bug/validation : ex. paramètre invalide)
public class ConfigurationInvalideException extends RuntimeException {
    public ConfigurationInvalideException(String message) { super(message); }
}

// Usage : ne jamais "avaler" une exception
try {
    interroger(ip);
} catch (EquipementInjoignableException e) {
    logger.warn("Ping échoué sur {}", ip, e);  // on LOGUE avec la stack trace...
    planifierNouvelEssai(ip);                   // ...et on AGIT
}
```

**Anti-pattern** : `catch (Exception e) {}` vide. Si vous ne savez pas quoi faire, laissez remonter (`throws`) — c'est toujours mieux que le silence.

---

## 33. Lambdas : fonctions anonymes concises (Java 8+)

```java
// Avant : classe anonyme verbeuse
new Thread(new Runnable() {
    @Override public void run() { System.out.println("démarré"); }
}).start();

// Après : lambda
new Thread(() -> System.out.println("démarré")).start();

// Formes
Runnable r = () -> System.out.println("sans paramètre");
Consumer<String> affiche = s -> System.out.println(s);            // 1 param, pas de retour
BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;     // 2 params, retour
Supplier<Double> aleatoire = () -> Math.random();                // pas de param, retour
Predicate<String> nonVide = s -> !s.isBlank();                   // test -> boolean
Function<String, Integer> longueur = String::length;             // référence de méthode

// Références de méthode : les 4 formes
Function<String, Integer> f1 = String::length;        // méthode d'instance (param = receveur)
BiFunction<String, String, Boolean> f2 = String::equals;
Supplier<List<String>> f3 = ArrayList::new;          // constructeur
Function<Integer, int[]> f4 = int[]::new;            // constructeur de tableau

// Les lambdas capturent les variables locales si elles sont "effectively final"
int seuil = 20;
Predicate<Integer> alerte = v -> v > seuil;  // OK tant que seuil n'est plus modifié après
```

Interfaces fonctionnelles clés (`java.util.function`) : `Predicate<T>`, `Function<T,R>`, `Consumer<T>`, `Supplier<T>`, `BiFunction`, `UnaryOperator`, `BinaryOperator`.

---

## 34. Streams : traitement déclaratif des données (Java 8+)

Un stream = **pipeline** : source → opérations intermédiaires (lazy) → opération terminale.

```java
record Alarme(String equipement, String code, int criticite) {}

List<Alarme> alarmes = List.of(
    new Alarme("UPS-01", "BATTERIE_FAIBLE", 3),
    new Alarme("SW-01", "PORT_DOWN", 2),
    new Alarme("UPS-01", "SURCHARGE", 4),
    new Alarme("RT-01", "PORT_DOWN", 2)
);

// Filtrer + transformer + collecter
List<String> critiques = alarmes.stream()
    .filter(a -> a.criticite() >= 3)          // intermédiaire
    .map(Alarme::equipement)                  // intermédiaire
    .distinct()                               // intermédiaire
    .sorted()                                 // intermédiaire
    .toList();                                // terminale -> [UPS-01] (liste immutable)

// Regrouper
Map<String, List<Alarme>> parEquipement = alarmes.stream()
    .collect(Collectors.groupingBy(Alarme::equipement));

// Compter par code
Map<String, Long> parCode = alarmes.stream()
    .collect(Collectors.groupingBy(Alarme::code, Collectors.counting()));

// Statistiques
DoubleSummaryStatistics stats = alarmes.stream()
    .collect(Collectors.summarizingDouble(Alarme::criticite));
System.out.println("max=" + stats.getMax() + " moyenne=" + stats.getAverage());

// Réduction
int criticiteTotale = alarmes.stream().mapToInt(Alarme::criticite).sum();
Optional<Alarme> pire = alarmes.stream().max(Comparator.comparingInt(Alarme::criticite));

// anyMatch / allMatch / noneMatch / findFirst
boolean urgent = alarmes.stream().anyMatch(a -> a.criticite() >= 4);

// Chaînage plat : flatMap
List<String> tousLesCodes = sites.stream()
    .flatMap(site -> site.getAlarmes().stream())
    .map(Alarme::code)
    .toList();
```

**Règles d'or des streams** :
- Une opération intermédiaire ne fait **rien** sans opération terminale (lazy).
- Un stream est **à usage unique** : le réutiliser lève `IllegalStateException`.
- Pas d'effet de bord dans `map`/`filter` (pas de `list.add` dedans !).
- `parallelStream()` : seulement pour gros volumes + opérations coûteuses et sans état — mesurez avant/après, le parallélisme a un coût.

---

## 35. Optional : en finir avec le null (Java 8+)

```java
// ❌ Avant : null qui traîne, NPE à 3h du matin
Equipement e = trouverParNom(nom);
System.out.println(e.getIp());  // NullPointerException si absent !

// ✅ Après : l'absence est explicite dans le type
public Optional<Equipement> trouverParNom(String nom) {
    return equipements.stream().filter(e -> e.getNom().equals(nom)).findFirst();
}

trouverParNom("UPS-01")
    .ifPresent(e -> System.out.println(e.getIp()));            // action si présent

String ip = trouverParNom("UPS-99")
    .map(Equipement::getIp)                                    // transforme si présent
    .orElse("0.0.0.0");                                        // valeur par défaut

String ip2 = trouverParNom("UPS-99")
    .map(Equipement::getIp)
    .orElseThrow(() -> new EquipementInjoignableException("UPS-99")); // ou exception

// Filtrage
trouverParNom("UPS-01")
    .filter(e -> e.getEtat() == EtatEquipement.EN_SERVICE)
    .ifPresent(e -> System.out.println("OK : " + e.getNom()));

// orElse vs orElseGet : orElse ÉVALUE toujours son argument !
String v1 = opt.orElse(calculCouteux());      // calculCouteux() appelé même si opt présent !
String v2 = opt.orElseGet(() -> calculCouteux()); // lazy : appelé seulement si absent
```

**Règles** : `Optional` = type de **retour** (et chaînage), jamais attribut de classe, jamais paramètre de méthode, jamais `Optional.of(null)` (utilisez `ofNullable`). Ne remplace pas la validation d'arguments (`Objects.requireNonNull`).

---

## 36. Dates et heures : java.time (Java 8+)

Oubliez `Date`/`Calendar` (mutables, piégeux). `java.time` = immutable et thread-safe.

```java
import java.time.*;
import java.time.format.DateTimeFormatter;
import java.time.temporal.ChronoUnit;

LocalDate aujourd = LocalDate.now();                    // 2026-09-26
LocalTime midi = LocalTime.of(12, 30);
LocalDateTime releve = LocalDateTime.of(2026, 9, 26, 14, 0);
ZonedDateTime paris = ZonedDateTime.now(ZoneId.of("Europe/Paris"));
Instant instant = Instant.now();                        // timestamp UTC, parfait pour logs/BDD

// Calculs
LocalDate dans30j = aujourd.plusDays(30);
long jours = ChronoUnit.DAYS.between(derniereMaintenance, aujourd);
Duration panne = Duration.between(debut, fin);           // heures/min/sec
Period garantie = Period.ofYears(2);                    // années/mois/jours

// Formatage / parsing (DateTimeFormatter est thread-safe, contrairement à SimpleDateFormat)
DateTimeFormatter fmt = DateTimeFormatter.ofPattern("dd/MM/yyyy HH:mm");
System.out.println(releve.format(fmt));                 // 26/09/2026 14:00
LocalDateTime parse = LocalDateTime.parse("26/09/2026 14:00", fmt);

// Pour la monnaie : BigDecimal, JAMAIS double
import java.math.BigDecimal;
BigDecimal prix = new BigDecimal("19.99");              // constructeur String, pas double !
BigDecimal ttc = prix.multiply(new BigDecimal("1.20"));
System.out.println(ttc.setScale(2, RoundingMode.HALF_UP)); // 23.99
```

**À retenir** : `Instant` pour horodater/stocker, `ZonedDateTime` pour afficher à l'utilisateur, `BigDecimal(String)` pour l'argent.

---

## 37. Entrées-sorties fichiers : Path et Files (NIO.2)

```java
import java.nio.file.*;
import java.io.IOException;
import java.util.List;

Path p = Path.of("data", "inventaire.csv");   // construction portable (séparateurs gérés)
System.out.println(p.toAbsolutePath());
System.out.println(p.getFileName());          // inventaire.csv
System.out.println(p.getParent());            // data

// Lire
String contenu = Files.readString(p);                       // petit fichier texte
byte[] octets = Files.readAllBytes(p);                      // petit fichier binaire
List<String> lignes = Files.readAllLines(p);                // petites listes de lignes
try (var stream = Files.lines(p)) {                         // GROS fichier : stream lazy ligne par ligne
    stream.filter(l -> l.contains("UPS"))
          .forEach(System.out::println);
}

// Écrire
Files.writeString(p, "UPS-01,40\n", StandardOpenOption.CREATE, StandardOpenOption.APPEND);

// Tester / créer
if (Files.notExists(p)) Files.createDirectories(p.getParent());
System.out.println(Files.isRegularFile(p) + " " + Files.size(p));
System.out.println(Files.getLastModifiedTime(p));

// Copier / déplacer / supprimer
Files.copy(p, Path.of("data", "backup.csv"), StandardCopyOption.REPLACE_EXISTING);
Files.move(p, Path.of("archive", "inventaire.csv"));
Files.deleteIfExists(Path.of("tmp.txt"));

// Parcourir une arborescence
try (var walk = Files.walk(Path.of("data"))) {
    walk.filter(Files::isRegularFile)
        .filter(f -> f.toString().endsWith(".csv"))
        .forEach(System.out::println);
}
```

**À retenir** : `Files` + `Path` remplacent `File` dans tout code moderne ; `Files.lines`/`Files.walk` retournent des streams à fermer (try-with-resources).

---

## 38. NIO avancé : buffers, canaux, WatchService

```java
// Copie rapide via canaux (utile pour gros fichiers)
try (var in = FileChannel.open(Path.of("gros.iso"), StandardOpenOption.READ);
     var out = FileChannel.open(Path.of("copie.iso"),
             StandardOpenOption.CREATE, StandardOpenOption.WRITE)) {
    in.transferTo(0, in.size(), out);   // zéro-copy quand l'OS le permet
}

// Surveiller un répertoire (ex. : dossier de dépôt de fichiers d'inventaire)
try (WatchService watcher = FileSystems.getDefault().newWatchService()) {
    Path dir = Path.of("data/inbox");
    dir.register(watcher, StandardWatchEventKinds.ENTRY_CREATE);
    while (true) {
        WatchKey key = watcher.take();  // bloque jusqu'à un événement
        for (WatchEvent<?> ev : key.pollEvents()) {
            System.out.println("Nouveau fichier : " + ev.context());
            // -> déclencher le traitement d'import ici
        }
        if (!key.reset()) break;
    }
}
```

Pour 99 % des outils internes, `Files` suffit ; gardez `WatchService` pour les scénarios « dossier d'échange » (dépôt CSV/exports).

---

## 39. Threads : les bases

```java
// 1. Via Thread + lambda
Thread t = new Thread(() -> {
    System.out.println("Tâche dans " + Thread.currentThread().getName());
});
t.start();          // démarre (NE JAMAIS appeler run() directement !)
t.join();           // attend la fin

// 2. Via Runnable nommé (préférable : sépare la tâche du mécanisme)
Runnable inventaire = () -> scannerSousReseau("10.0.0.0/24");
new Thread(inventaire, "scan-reseau").start();

// 3. Via Callable + Future : tâche qui RETOURNE un résultat
ExecutorService pool = Executors.newFixedThreadPool(4);
Future<Integer> futur = pool.submit(() -> compterEquipements());
System.out.println("Équipements : " + futur.get());  // bloque jusqu'au résultat
pool.shutdown();

// États d'un thread : NEW -> RUNNABLE -> (BLOCKED/WAITING/TIMED_WAITING) -> TERMINATED
```

**Erreurs fréquentes** : appeler `run()` au lieu de `start()` (exécution synchrone dans le thread courant !) ; oublier `shutdown()` sur un pool (la JVM ne se termine pas).

---

## 40. synchronized, volatile, et les pièges classiques

```java
public class Compteur {
    private int valeur = 0;

    // ❌ Sans synchronized : deux threads peuvent lire/écrire en même temps -> comptes perdus
    // ✅ Avec : un seul thread à la fois dans la méthode (verrou = l'instance)
    public synchronized void incrementer() { valeur++; }

    public synchronized int getValeur() { return valeur; }
}

// Bloc synchronisé ciblé (verrouille moins longtemps = mieux)
private final Object verrou = new Object();
public void ajouterMesure(double m) {
    // ... calculs non critiques hors verrou ...
    synchronized (verrou) {
        mesures.add(m);   // section critique courte
    }
}

// volatile : visibilité immédiate inter-threads (mais PAS d'atomicité)
private volatile boolean arretDemande = false;
// Thread worker : while (!arretDemande) { ... }  -> voit le changement sans synchronized
```

**Règle** : `volatile` pour un flag ; `synchronized` (ou `java.util.concurrent`) pour toute opération lire-modifier-écrire (`i++`, `check-then-act`). Pour les compteurs : `AtomicInteger`/`AtomicLong` (sans verrou, plus rapides).

```java
import java.util.concurrent.atomic.AtomicLong;
private final AtomicLong requetes = new AtomicLong();
requetes.incrementAndGet();   // atomique, sans synchronized
```

---

## 41. ExecutorService : la bonne façon de gérer des threads

Ne créez **jamais** des `Thread` à la main en production : utilisez des pools.

```java
import java.util.concurrent.*;
import java.util.List;

ExecutorService pool = Executors.newFixedThreadPool(8);  // 8 threads réutilisés
try {
    List<Callable<String>> taches = List.of(
        () -> ping("10.0.0.11"),
        () -> ping("10.0.0.12"),
        () -> interrogerSnmp("10.0.0.13")
    );
    // invokeAll : lance tout, attend tout (avec timeout global possible)
    List<Future<String>> resultats = pool.invokeAll(taches, 30, TimeUnit.SECONDS);
    for (Future<String> f : resultats) {
        System.out.println(f.isCancelled() ? "TIMEOUT" : f.get());
    }
} finally {
    pool.shutdown();                                  // 1. refuse nouvelles tâches
    if (!pool.awaitTermination(10, TimeUnit.SECONDS)) {
        pool.shutdownNow();                           // 2. force l'arrêt si ça traîne
    }
}

// Planification (cron-like en mémoire)
ScheduledExecutorService planif = Executors.newSingleThreadScheduledExecutor();
planif.scheduleAtFixedRate(() -> releverCompteurs(), 0, 5, TimeUnit.MINUTES);
```

**Choisir la taille du pool** : tâches I/O-bound (réseau, disque) → beaucoup de threads (50-200) ; tâches CPU-bound → `Runtime.getRuntime().availableProcessors()` threads.

---

## 42. Virtual threads : la révolution Java 21+

Problème historique : 1 thread plateforme = ~1 Mo de pile → 10 000 connexions simultanées = 10 Go. Les **virtual threads** sont légers (quelques Ko) et gérés par la JVM.

```java
// AVANT (Java 17) : pool limité, code async complexe pour scaler
// APRÈS (Java 21+) : un thread virtuel par tâche, code BLOQUANT simple qui scale

// Lancement simple
Thread.startVirtualThread(() -> {
    System.out.println("Je suis virtuel : " + Thread.currentThread());
});

// Executor de virtual threads : remplace le pool pour les tâches I/O-bound
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    for (String ip : toutesLesIp) {
        executor.submit(() -> ping(ip));   // 10 000 tâches = OK, code bloquant simple
    }
} // fermeture auto : attend la fin des tâches

// Dans du code existant : remplacez juste la fabrique
ExecutorService pool = Executors.newFixedThreadPool(200);          // avant
ExecutorService vt = Executors.newVirtualThreadPerTaskExecutor(); // après (Java 21+)
```

**Quand les utiliser** : I/O-bound massif (scans réseau, collecteurs SNMP, serveurs HTTP) → idéal. **Quand les éviter** : calcul CPU intensif (aucun gain), code avec `synchronized` bloquant long (peut « pinner » le thread porteur — préférez `ReentrantLock`).

---

## 43. Réseau et HTTP : HttpClient (Java 11+)

```java
import java.net.URI;
import java.net.http.*;
import java.time.Duration;

HttpClient client = HttpClient.newBuilder()
    .connectTimeout(Duration.ofSeconds(5))
    .followRedirects(HttpClient.Redirect.NORMAL)
    .build();

// GET simple
HttpRequest requete = HttpRequest.newBuilder()
    .uri(URI.create("https://api.exemple.com/equipements"))
    .header("Accept", "application/json")
    .timeout(Duration.ofSeconds(10))
    .GET()
    .build();

HttpResponse<String> reponse = client.send(requete, HttpResponse.BodyHandlers.ofString());
System.out.println(reponse.statusCode());
System.out.println(reponse.body());

// POST JSON
String json = """
    {"nom": "UPS-04", "puissance_kva": 60}
    """;
HttpRequest post = HttpRequest.newBuilder()
    .uri(URI.create("https://api.exemple.com/equipements"))
    .header("Content-Type", "application/json")
    .POST(HttpRequest.BodyPublishers.ofString(json))
    .build();
HttpResponse<String> rep = client.send(post, HttpResponse.BodyHandlers.ofString());

// Asynchrone (CompletableFuture)
client.sendAsync(requete, HttpResponse.BodyHandlers.ofString())
      .thenApply(HttpResponse::body)
      .thenAccept(System.out::println);
```

**À retenir** : `HttpClient` est immutable et thread-safe → **une seule instance partagée** dans toute l'application.

---

## 44. Introduction à JDBC : parler aux bases SQL

```java
import java.sql.*;

// PostgreSQL : dépendance Maven -> org.postgresql:postgresql:42.7.x
String url = "jdbc:postgresql://localhost:5432/gmao";
String user = "gmao";            // en vrai : variables d'environnement, jamais en dur !
String pass = System.getenv("DB_PASSWORD");

try (Connection cx = DriverManager.getConnection(url, user, pass)) {

    // INSERT avec paramètres (ANTI-INJECTION SQL : toujours PreparedStatement, jamais de concaténation)
    String sql = "INSERT INTO equipement(nom, puissance_kva, etat) VALUES (?, ?, ?)";
    try (PreparedStatement ps = cx.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS)) {
        ps.setString(1, "UPS-04");
        ps.setDouble(2, 60.0);
        ps.setString(3, "EN_SERVICE");
        ps.executeUpdate();
        try (ResultSet cles = ps.getGeneratedKeys()) {
            if (cles.next()) System.out.println("id=" + cles.getLong(1));
        }
    }

    // SELECT
    try (PreparedStatement ps = cx.prepareStatement("SELECT nom, puissance_kva FROM equipement WHERE etat = ?");
         ) {
        ps.setString(1, "EN_SERVICE");
        try (ResultSet rs = ps.executeQuery()) {
            while (rs.next()) {
                System.out.println(rs.getString("nom") + " : " + rs.getDouble("puissance_kva") + " kVA");
            }
        }
    }
} catch (SQLException e) {
    System.err.println("SQL [" + e.getSQLState() + "] : " + e.getMessage());
}
```

**Indispensable en production** : un pool de connexions (HikariCP — inclus par défaut dans Spring Boot). Ouvrir une connexion TCP par requête = lent et fragile.

**Transaction** :
```java
cx.setAutoCommit(false);
try {
    // ... plusieurs updates ...
    cx.commit();
} catch (SQLException e) {
    cx.rollback();   // tout ou rien
    throw e;
} finally {
    cx.setAutoCommit(true);
}
```

---

## 45. Tests unitaires avec JUnit 5

```java
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

class CalculatriceTest {

    @Test
    @DisplayName("L'addition de deux positifs fonctionne")
    void addition() {
        assertEquals(5, Calculatrice.additionner(2, 3));
    }

    @Test
    void divisionParZeroLeveUneException() {
        assertThrows(IllegalArgumentException.class, () -> Calculatrice.diviser(1, 0));
    }

    @Test
    void sommeVarargs() {
        assertAll(
            () -> assertEquals(6, Calculatrice.somme(1, 2, 3)),
            () -> assertEquals(0, Calculatrice.somme())
        );
    }

    // Test paramétré : le même test avec plusieurs jeux de données
    @ParameterizedTest
    @CsvSource({"2, 3, 5", "0, 0, 0", "-1, 1, 0"})
    void additionParametree(int a, int b, int attendu) {
        assertEquals(attendu, Calculatrice.additionner(a, b));
    }

    @Test
    @Timeout(2)   // échoue si > 2 secondes
    void neDoitPasBoucler() { /* ... */ }
}
```

Cycle de vie : `@BeforeEach` (setup avant chaque test), `@AfterEach`, `@BeforeAll`/`@AfterAll` (static, une fois). **Règle** : un test = un comportement ; nom explicite ; pas de dépendance entre tests ; pas d'accès réseau/disque dans les tests unitaires (mockez !).

---

## 46. Mockito : simuler les dépendances

```java
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

// On teste SupervisionService sans vraie base ni vrai réseau
class SupervisionServiceTest {

    @Test
    void alerteSiBatterieFaible() {
        // 1. Créer le mock
        CapteurBatterie capteur = mock(CapteurBatterie.class);
        // 2. Programmer son comportement
        when(capteur.niveauCharge("UPS-01")).thenReturn(12);  // 12 %
        // 3. Injecter et tester
        SupervisionService service = new SupervisionService(capteur);
        assertTrue(service.batterieCritique("UPS-01"));
        // 4. Vérifier les interactions
        verify(capteur).niveauCharge("UPS-01");
        verify(capteur, never()).niveauCharge("UPS-02");
    }

    @Test
    void relanceEnCasDErreur() {
        CapteurBatterie capteur = mock(CapteurBatterie.class);
        when(capteur.niveauCharge(anyString()))
            .thenThrow(new RuntimeException("timeout"))  // 1er appel
            .thenReturn(80);                             // 2e appel
        SupervisionService service = new SupervisionService(capteur);
        assertFalse(service.batterieCritique("UPS-01")); // le retry a fonctionné
        verify(capteur, times(2)).niveauCharge("UPS-01");
    }
}
```

**Mockito** = `mock()` + `when(...).thenReturn(...)` + `verify()`. Ne mockez que les **frontières** (réseau, BDD, horloge) — jamais la classe testée elle-même.

---

## 47. Debug : méthode et outils

**Méthode en 5 étapes** :
1. **Reproduire** : cas minimal, déterministe si possible.
2. **Lire** : la stack trace du **haut** (votre code) vers le bas ; la 1re ligne « Caused by » est souvent la vraie cause.
3. **Hypothèse** : une seule à la fois.
4. **Vérifier** : breakpoint ou log ciblé, pas 50 `println`.
5. **Corriger + test de non-régression** : ajoutez le test qui aurait attrapé le bug.

**Dans IntelliJ** : clic dans la gouttière = breakpoint ; `F8` pas-à-pas, `F7` entrer dans, `Shift+F8` sortir ; « Evaluate Expression » (`Alt+F8`) pour inspecter ; breakpoints conditionnels (clic droit) pour les boucles.

**Flags JVM utiles au diagnostic** :
```bash
java -Xlog:gc*:file=gc.log          # logs GC
jstack <pid>                         # dump des threads (deadlocks !)
jmap -histo <pid>                    # histogramme du heap (fuites mémoire)
java -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/dump.hprof -jar app.jar
```

**Lire une stack trace** :
```text
Exception in thread "main" java.lang.NullPointerException: Cannot invoke "String.length()" because "nom" is null
        at com.entreprise.App.valider(App.java:42)      <- VOTRE code, ligne 42
        at com.entreprise.App.main(App.java:15)
```
Depuis Java 14, les NPE sont « helpful » : le message dit **quelle** variable est null. (Voir aussi section 53.)

---

## 48. Intro Spring Boot : exposer une API REST

Spring Boot = le standard pour les applications/services Java en entreprise. Il embarque un serveur (Tomcat) : **un jar exécutable suffit**.

`pom.xml` (parent + starter web) :
```xml
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>3.3.5</version>
</parent>
<dependencies>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
</dependencies>
```

```java
package com.entreprise.api;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.*;
import java.util.*;
import java.util.concurrent.ConcurrentHashMap;

@SpringBootApplication
public class ApiApplication {
    public static void main(String[] args) {
        SpringApplication.run(ApiApplication.class, args);  // démarre sur http://localhost:8080
    }
}

// DTO immutable : un record suffit
record EquipementDto(String nom, double puissanceKva, String etat) {}

@RestController
@RequestMapping("/api/equipements")
class EquipementController {

    private final Map<String, EquipementDto> base = new ConcurrentHashMap<>();

    @GetMapping
    public Collection<EquipementDto> lister() {
        return base.values();
    }

    @GetMapping("/{nom}")
    public EquipementDto detail(@PathVariable String nom) {
        EquipementDto e = base.get(nom);
        if (e == null) throw new EquipementIntrouvableException(nom);
        return e;
    }

    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public EquipementDto creer(@RequestBody EquipementDto dto) {
        base.put(dto.nom(), dto);
        return dto;
    }

    @DeleteMapping("/{nom}")
    @ResponseStatus(HttpStatus.NO_CONTENT)
    public void supprimer(@PathVariable String nom) {
        base.remove(nom);
    }
}

// Gestion d'erreur centralisée
@ResponseStatus(HttpStatus.NOT_FOUND)
class EquipementIntrouvableException extends RuntimeException {
    EquipementIntrouvableException(String nom) { super("Équipement introuvable : " + nom); }
}
```

Test :
```bash
curl -X POST localhost:8080/api/equipements -H 'Content-Type: application/json' \
  -d '{"nom":"UPS-04","puissance_kva":60,"etat":"EN_SERVICE"}'
curl localhost:8080/api/equipements/UPS-04
```

**À retenir** : `@RestController` + `@GetMapping`/`@PostMapping` + records DTO = API REST fonctionnelle en 50 lignes. La suite logique : Spring Data JPA (BDD), Spring Security, Actuator (supervision `/actuator/health`).

---

## 49. Packaging : jar, jlink, jpackage

```bash
# 1. Jar exécutable avec Maven (plugin shade : inclut les dépendances)
# pom.xml -> maven-shade-plugin, puis :
mvn package
java -jar target/outil-1.0.0.jar

# MANIFESTE : pour un jar lançable sans plugin, indiquez la classe principale
# src/main/resources/META-INF/MANIFEST.MF :
#   Main-Class: com.entreprise.outil.Main

# 2. Runtime sur mesure (Java 11+) : embarque uniquement les modules JDK nécessaires
jlink --module-path "$JAVA_HOME/jmods" --add-modules java.base,java.sql,java.net.http \
      --output runtime-perso --compress=2 --strip-debug
# -> dossier runtime-perso/bin/java (~40 Mo au lieu de ~300 Mo)

# 3. Installateur natif (Java 14+) : .deb, .msi, .dmg avec runtime embarqué
jpackage --name outil-inventaire --input target/ --main-jar outil-1.0.0.jar \
         --main-class com.entreprise.outil.Main --type deb \
         --dest installateurs/
```

**En pratique entreprise** : jar « fat » (shade) pour les outils internes simples ; image Docker `eclipse-temurin:21-jre` + jar pour les services ; `jpackage` quand il faut un installateur livré à des non-techniciens.

---

## 50. La JVM : comprendre la machine

Le bytecode `.class` s'exécute sur la JVM, qui le compile **à chaud** (JIT) en code natif après quelques milliers d'appels. Conséquences pratiques :

- **Warmup** : les premières secondes/minutes sont plus lentes (montée en charge progressive en prod, pas de benchmark à froid).
- **Heap** : zone mémoire des objets, gérée par le GC. Dimensionnée via `-Xms` (initial) et `-Xmx` (max).
- **Stack** : une pile par thread (appels de méthodes, variables locales). `-Xss` règle sa taille.
- **Metaspace** : définitions des classes.

```bash
# Réglages mémoire typiques pour un service (ex. : 2 Go max, GC G1 par défaut)
java -Xms512m -Xmx2g -jar app.jar

# Java 21+ : dimensionnement automatique raisonnable dans les conteneurs (détecte les cgroups)
java -XX:MaxRAMPercentage=75.0 -jar app.jar   # utilise 75 % de la RAM du conteneur
```

**À retenir** : en conteneur, fixez `-Xmx` ou `MaxRAMPercentage` — sinon la JVM peut se croire sur une machine à 128 Go et se faire tuer par l'OOM-killer.

---

## 51. Garbage Collector : l'essentiel

| GC | Depuis | Profil |
|---|---|---|
| G1 (défaut) | Java 9 | Équilibré, pauses < ~200 ms, bon choix par défaut |
| ZGC | Java 15 (prod) | Pauses < 1 ms, heaps énormes (To) |
| Shenandoah | Java 15 | Pauses courtes, bon sur heaps moyens |
| Parallel | Toujours | Débit max, batchs (pauses longues acceptables) |

```bash
java -XX:+UseZGC -Xmx8g -jar app.jar        # choisir ZGC
java -Xlog:gc*:file=/var/log/app/gc.log:time,uptime,level,tags -jar app.jar
```

**Fuites mémoire en Java ?** Oui : références conservées par erreur (cache sans éviction, listeners jamais désinscrits, `ThreadLocal` oublié, collections static qui grossissent). Symptôme : heap qui monte en dents de scie jusqu'à `OutOfMemoryError`. Diagnostic : `jmap -histo`, heap dump + analyse (Eclipse MAT).

**Bonnes pratiques** :
- Ne faites **jamais** `System.gc()` en production (force un GC complet, sauf cas très particulier).
- Réutilisez les objets coûteux via des pools uniquement si mesuré nécessaire (le GC moderne alloue très vite).
- Préférez les objets **courte durée de vie** : le GC adore ça (collectés jeunes, presque gratuits).

---

## 52. Performance : mesurer avant d'optimiser

**Règle n°1** : pas d'optimisation sans mesure. 97 % du temps, le goulot n'est pas là où vous croyez.

```bash
# Micro-benchmark sérieux : JMH (jamais de boucle System.nanoTime() artisanale !)
# Dépendance : org.openjdk.jmh:jmh-core:1.37
```

Checklist des gains faciles (par ordre d'impact typique) :

1. **I/O** : requêtes SQL N+1, appels HTTP en série → batcher / paralléliser (virtual threads).
2. **Algorithmique** : `List.contains` en O(n) dans une boucle → `HashSet` en O(1).
3. **Concaténation String en boucle** → `StringBuilder` (section 8).
4. **Boxing** : `Stream<Integer>` + `mapToInt` plutôt que `map` (évite `Integer` ↔ `int`).
5. **Logs** : `logger.debug("x=" + couteux())` évalue même si le niveau est INFO → utilisez `logger.debug("x={}", () -> couteux())` ou gardez la concaténation hors du log.
6. **Collections dimensionnées** : `new ArrayList<>(tailleConnue)`, `new HashMap<>(n)` évitent les reallocations.
7. **Regex précompilées** : `Pattern.compile` une fois en `static final`, pas dans la boucle.

```java
// ❌ Lent : boxing + contains O(n) + regex recompilée à chaque tour
for (String ip : ips) {
    if (listeNoire.contains(ip) && ip.matches("\\d+\\.\\d+\\.\\d+\\.\\d+")) alerter(ip);
}

// ✅ Rapide : HashSet O(1) + Pattern précompilé
private static final Pattern IPV4 = Pattern.compile("\\d+\\.\\d+\\.\\d+\\.\\d+");
private final Set<String> listeNoire = new HashSet<>(...);
for (String ip : ips) {
    if (IPV4.matcher(ip).matches() && listeNoire.contains(ip)) alerter(ip);
}
```

---

## 53. Sécurité : les réflexes

| Risque | Contre-mesure |
|---|---|
| Injection SQL | `PreparedStatement` systématique (section 44), jamais de concaténation |
| Injection de commande | `ProcessBuilder` avec liste d'arguments, jamais `Runtime.exec(String)` avec entrée utilisateur |
| Path traversal | Validez que `chemin.normalize().startsWith(racineAutorisee)` |
| XXE (XML) | Désactivez DTD/entités externes sur le parser |
| Désérialisation Java native | **Bannissez** `ObjectInputStream` sur données non fiables (gadget chains) ; préférez JSON |
| Secrets en dur | Variables d'environnement / coffre (Vault), jamais dans le code ni le jar |
| Dépendances vulnérables | `mvn dependency:check` / OWASP Dependency-Check / Dependabot en CI |
| TLS | `HttpsURLConnection`/`HttpClient` vérifient par défaut ; ne désactivez **jamais** la vérif. du certificat « pour tester » en prod |
| Mots de passe | BCrypt/Argon2 (jamais MD5/SHA-1, jamais en clair) |

```java
// ✅ Lecture d'un secret : variable d'environnement
String dbPassword = System.getenv("DB_PASSWORD");
if (dbPassword == null) throw new IllegalStateException("DB_PASSWORD non défini");

// ✅ Path traversal : confinement dans un répertoire
Path racine = Path.of("/srv/rapports").toAbsolutePath();
Path demande = racine.resolve(nomFichier).normalize();
if (!demande.startsWith(racine)) throw new SecurityException("Chemin interdit : " + nomFichier);

// ❌ INTERDIT : désactiver la vérification TLS
// TrustManager qui accepte tout -> faille béante. Ne jamais commiter ça.
```

**Hygiène** : `mvn versions:display-dependency-updates` régulièrement ; image Docker minimale ; principe du moindre privilège (utilisateur non-root dans le conteneur).

---

## 54. Bonnes pratiques & style de code

### Style (Google Java Style / conventions Oracle)

- Indentation 4 espaces (ou 2 — mais **constant** dans le projet) ; accolades même ligne pour les blocs.
- 120 colonnes max ; un import par ligne, pas de `.*` (sauf tests).
- Noms explicites : `equipementsHorsService`, pas `list2`.
- Formateur automatique : **Spotless** ou le formateur IntelliJ appliqué à chaque commit (via pre-commit hook).

### Principes

- **DRY** : factorisez la 3e duplication, pas la 1re.
- **KISS** : la solution simple et lisible bat la solution « intelligente ».
- **Fail fast** : `Objects.requireNonNull(param, "param")` en tête de méthode publique.
- **Immutabilité** : `final` champs, records, collections non modifiables renvoyées (`List.copyOf`).
- **Pas de `null` silencieux** : `Optional`, listes vides, exceptions explicites.
- **Javadoc** sur toute API publique : `@param`, `@return`, `@throws`.

```java
/**
 * Calcule l'autonomie estimée en minutes.
 *
 * @param chargeKva charge actuelle en kVA, doit être &gt; 0
 * @param capaciteAh capacité batterie en Ah, doit être &gt; 0
 * @return autonomie en minutes, arrondie à l'entier inférieur
 * @throws IllegalArgumentException si un paramètre est &lt;= 0
 */
public static long autonomieMinutes(double chargeKva, double capaciteAh) {
    Objects.requireNonNull(chargeKva, "chargeKva"); // pour objets ; ici double -> check manuel
    if (chargeKva <= 0 || capaciteAh <= 0) throw new IllegalArgumentException("paramètres > 0 requis");
    return (long) (capaciteAh * 12 * 60 / (chargeKva * 1000));
}
```

### Checklist revue de code

- [ ] Le code compile sans warning nouveau (`-Xlint:all`)
- [ ] Tests unitaires : cas nominaux + limites + erreurs
- [ ] Pas de secret / chemin en dur
- [ ] Ressources fermées (try-with-resources)
- [ ] `equals`/`hashCode` cohérents si usage en Set/Map
- [ ] Logs utiles (pas de `printStackTrace()`, niveaux cohérents)
- [ ] Complexité raisonnable (méthode < 30 lignes, imbrication < 3)

---

## 55. Erreurs classiques des débutants (et comment les éviter)

### 1. `NullPointerException` (NPE)
```java
String nom = null;
nom.length();  // 💥 NPE
// Remèdes : valider en entrée (Objects.requireNonNull), Optional en retour,
// vérifier les retours de Map.get(), initialiser les champs.
```

### 2. `==` sur les String (et les objets en général)
```java
if (etat == "EN_SERVICE") { }      // ❌ compare les références
if ("EN_SERVICE".equals(etat)) { } // ✅ compare le contenu (et null-safe !)
```

### 3. Division entière involontaire
```java
double moyenne = total / nombre;      // ❌ si int/int -> tronqué AVANT conversion
double moyenne = (double) total / nombre; // ✅
```

### 4. `ConcurrentModificationException`
Modifier une collection pendant son for-each → voir section 29 (`removeIf`, `Iterator.remove()`).

### 5. Oublier `break` dans un switch classique / confondre `=` et `==`
```java
if (etat = "HS") { }  // ❌ compile si etat est boolean... et assigne ! (avec String : erreur de compilation, ouf)
```
Préférez les **switch expressions** (section 9) : pas de `break` à oublier.

### 6. Fuite de ressources (fichier/socket non fermé)
```java
var r = Files.newBufferedReader(p); r.readLine(); // ❌ jamais fermé si exception
// ✅ try-with-resources systématique (section 31)
```

### 7. `ArrayIndexOutOfBoundsException` : boucle `<=` au lieu de `<`
```java
for (int i = 0; i <= t.length; i++) { } // ❌ le dernier index valide est length-1
for (int i = 0; i < t.length; i++) { }  // ✅ (ou for-each)
```

### 8. Comparer des `double` avec `==`
```java
if (0.1 + 0.2 == 0.3) { } // ❌ false ! (0.30000000000000004)
if (Math.abs(a - b) < 1e-9) { } // ✅ epsilon ; ou BigDecimal pour l'exact
```

### 9. Ignorer la valeur de retour des méthodes immutables
```java
s.trim();                    // ❌ ne modifie PAS s (String immutable)
s = s.trim();                // ✅
liste = liste.stream().filter(...).toList(); // idem : le stream ne modifie pas la liste
```

### 10. `static` abusif / tout mettre dans `main`
Le `static` = état global partagé. Un programme « tout static » n'est ni testable ni extensible : créez des objets, injectez les dépendances (constructeur).

### 11. Catcher `Exception` et ne rien faire
```java
try { ... } catch (Exception e) { } // ❌ bug invisible garanti
// ✅ logger + action, ou laisser remonter (section 32)
```

### 12. Confondre `List.of(...)` immutable et `ArrayList`
```java
var l = List.of("a"); l.add("b"); // 💥 UnsupportedOperationException
var l2 = new ArrayList<>(List.of("a")); l2.add("b"); // ✅
```

**À retenir** : 80 % des bugs débutants = null, `==` vs `equals`, ressources non fermées, division entière. La checklist de la section 54 les attrape en revue.

---

## 56. Nouveautés modernes : var, switch, text blocks, pattern matching

Récapitulatif versionné (tout ce qui suit exige la version indiquée minimum) :

```java
// var — inférence locale (Java 10+)
var inventaire = new ArrayList<Equipement>();  // ArrayList<Equipement>
var ligne = Files.readString(path);            // String
// Interdit : var x; / var n = null; / var comme type de champ ou de paramètre

// Switch expression — valeur de retour + flèches (Java 14+)
int joursMaintenance = switch (mois) {
    case JANVIER, MARS, MAI -> 2;
    case FEVRIER -> 1;
    default -> 0;
};

// yield — retour depuis un bloc case (Java 14+)
String niveau = switch (criticite) {
    case 1, 2 -> "info";
    case 3 -> {
        notifierAstrainte();
        yield "warning";
    }
    default -> "critique";
};

// Text blocks — multiligne (Java 15+)
String script = """
    #!/bin/bash
    echo "Sauvegarde du %s"
    """.formatted(nom);

// Pattern matching instanceof (Java 16+) : plus de cast explicite
if (obj instanceof Onduleur o) {           // o est typé et non-null ici
    System.out.println(o.getPuissanceKva());
}
// Avant : if (obj instanceof Onduleur) { Onduleur o = (Onduleur) obj; ... }

// Records (Java 16+), classes scellées (Java 17+) : hiérarchies fermées et exhaustives
sealed interface Alerte permits AlerteInfo, AlerteCritique {}
record AlerteInfo(String msg) implements Alerte {}
record AlerteCritique(String msg, int niveau) implements Alerte {}

// Pattern matching switch exhaustif sur sealed (Java 21+)
String traiter(Alerte a) {
    return switch (a) {   // le compilateur VÉRIFIE l'exhaustivité : pas de default nécessaire
        case AlerteInfo i -> "INFO: " + i.msg();
        case AlerteCritique c -> "CRITIQUE(" + c.niveau() + "): " + c.msg();
    };
}

// Virtual threads (Java 21+), String templates (preview Java 21+, finalisé plus tard)
// SequencedCollections : List.getFirst()/getLast(), Map.firstEntry() (Java 21+)
var premier = onduleurs.getFirst();
```

**Conseil version** : écrivez en « Java 17 » par défaut (le plus répandu en entreprise), adoptez les features 21 (virtual threads, pattern matching switch) quand le runtime cible est 21+.

---

## 57. Expressions régulières

```java
import java.util.regex.*;

// Précompiler en constante static final (voir section 52)
private static final Pattern IPV4 =
    Pattern.compile("^(?:\\d{1,3}\\.){3}\\d{1,3}$");
private static final Pattern MAC =
    Pattern.compile("^([0-9A-Fa-f]{2}:){5}[0-9A-Fa-f]{2}$");

System.out.println(IPV4.matcher("10.0.0.300").matches()); // true (format ; pas la validité 0-255 !)
System.out.println(MAC.matcher("AA:BB:CC:DD:EE:FF").matches()); // true

// Groupes de capture : extraire des champs d'une ligne de log
Pattern LOG = Pattern.compile("(\\S+) \\S+ \\S+ \\[([^]]+)] \"(\\S+) (\\S+)");
Matcher m = LOG.matcher("10.0.0.5 - - [26/Sep/2026:10:00:01] \"GET /api/ups HTTP/1.1\"");
if (m.find()) {
    System.out.println("IP=" + m.group(1) + " méthode=" + m.group(3) + " chemin=" + m.group(4));
}

// Remplacements
String propre = "  UPS-01 ; ; UPS-02  ".trim().replaceAll("\\s*;\\s*", ";");

// Mémento
// .         n'importe quel caractère        \d chiffre  \w [a-zA-Z0-9_]  \s espace
// * 0+  + 1+  ? 0/1  {n,m}                 ^ début  $ fin  | ou  () groupe
// (?i) insensible à la casse               \\. point littéral
```

**Attention** : valider une IPv4 « 0-255 » en regex pure est verbeux ; en pratique, combinez regex de format + `InetAddress.getByName()` ou parsing manuel des octets.

---

## 58. Annotations et réflexion (intro)

```java
import java.lang.annotation.*;

// Définir une annotation
@Retention(RetentionPolicy.RUNTIME)          // visible à l'exécution
@Target(ElementType.METHOD)                  // applicable aux méthodes
public @interface Planifiable {
    String cron() default "0 0 * * *";
    String description() default "";
}

// L'utiliser
public class Taches {
    @Planifiable(cron = "0 */5 * * *", description = "Relève compteurs")
    public void releverCompteurs() { /* ... */ }
}

// La lire par réflexion (c'est comme ça que Spring trouve vos @GetMapping)
for (var methode : Taches.class.getDeclaredMethods()) {
    Planifiable p = methode.getAnnotation(Planifiable.class);
    if (p != null) System.out.println(methode.getName() + " -> " + p.cron());
}
```

Annotations du quotidien : `@Override`, `@Deprecated`, `@SuppressWarnings("unchecked")`, `@FunctionalInterface`. La réflexion est puissante mais lente et casse l'encapsulation : réservez-la aux frameworks/outils, pas au code métier.

---

## 59. JSON : sérialisation avec Jackson

```java
// Dépendance Maven : com.fasterxml.jackson.core:jackson-databind:2.17.x
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.datatype.jsr310.JavaTimeModule;

record EquipementJson(String nom, double puissanceKva, EtatEquipement etat, Instant maj) {}

ObjectMapper mapper = new ObjectMapper();
mapper.registerModule(new JavaTimeModule());  // support java.time
// mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false); // tolérant

// Sérialiser
String json = mapper.writeValueAsString(new EquipementJson("UPS-01", 40.0, EtatEquipement.EN_SERVICE, Instant.now()));
String joli = mapper.writerWithDefaultPrettyPrinter().writeValueAsString(obj);

// Désérialiser
EquipementJson e = mapper.readValue(json, EquipementJson.class);

// Structures libres
JsonNode node = mapper.readTree(json);
System.out.println(node.get("nom").asText());
```

**À retenir** : `ObjectMapper` est thread-safe après configuration → **une instance partagée** (static final). Jackson + records = combo idéal pour les DTO d'API.

---

## 60. Logging : SLF4J + Logback

Ne faites **jamais** `System.out.println` en production : pas de niveaux, pas de fichier, pas de rotation.

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Collecteur {
    private static final Logger log = LoggerFactory.getLogger(Collecteur.class);

    public void interroger(String ip) {
        log.info("Interrogation de {}", ip);                    // {} = placeholder, pas de concaténation
        try {
            // ...
        } catch (Exception e) {
            log.warn("Échec sur {}, nouvel essai planifié", ip, e); // stack trace en dernier arg
        }
        log.debug("Détail trame : {}", () -> trameHexaCouteuse()); // supplier lazy (Logback 1.3+)
    }
}
```

`logback.xml` minimal (console + fichier rotatif) :
```xml
<configuration>
  <appender name="FICHIER" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <file>/var/log/collecteur/app.log</file>
    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
      <fileNamePattern>/var/log/collecteur/app.%d{yyyy-MM-dd}.log</fileNamePattern>
      <maxHistory>30</maxHistory>
    </rollingPolicy>
    <encoder><pattern>%d{HH:mm:ss.SSS} %-5level %logger{36} - %msg%n</pattern></encoder>
  </appender>
  <root level="INFO">
    <appender-ref ref="FICHIER"/>
  </root>
  <logger name="com.entreprise" level="DEBUG"/>
</configuration>
```

Niveaux : `ERROR` (panne) > `WARN` (anormal mais géré) > `INFO` (jalons métier) > `DEBUG` (diagnostic) > `TRACE` (très verbeux).

---

## 61. Cas pratique 1 — Outil CLI : inventaire réseau depuis un CSV

```java
import java.nio.file.*;
import java.util.*;
import java.util.stream.*;

/** Lit un CSV "nom;ip;puissance_kva", ping chaque hôte en parallèle (virtual threads),
 *  et écrit un rapport. Usage : java Inventaire.java inventaire.csv rapport.txt */
public class Inventaire {

    record Equipement(String nom, String ip, double puissanceKva) {}

    public static void main(String[] args) throws Exception {
        if (args.length < 2) {
            System.err.println("Usage : java Inventaire.java <entree.csv> <rapport.txt>");
            System.exit(2);
        }
        List<Equipement> equipements;
        try (Stream<String> lignes = Files.lines(Path.of(args[0]))) {
            equipements = lignes
                .filter(l -> !l.isBlank() && !l.startsWith("#"))
                .map(l -> l.split(";"))
                .filter(c -> c.length == 3)
                .map(c -> new Equipement(c[0].trim(), c[1].trim(), Double.parseDouble(c[2].trim())))
                .toList();
        }

        // Ping parallèle : un virtual thread par hôte (Java 21+)
        Map<Equipement, Boolean> resultats = new ConcurrentHashMap<>();
        try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
            for (Equipement e : equipements) {
                executor.submit(() -> resultats.put(e, ping(e.ip())));
            }
        } // attend la fin de tous les pings

        long joignables = resultats.values().stream().filter(b -> b).count();
        double puissanceTotale = equipements.stream().mapToDouble(Equipement::puissanceKva).sum();

        String rapport = """
            Rapport d'inventaire — %s
            =====================================
            Équipements : %d (joignables : %d, injoignables : %d)
            Puissance totale : %.1f kVA
            Injoignables :
            %s
            """.formatted(
                java.time.LocalDate.now(),
                equipements.size(), joignables, equipements.size() - joignables,
                puissanceTotale,
                resultats.entrySet().stream()
                    .filter(en -> !en.getValue())
                    .map(en -> "  - " + en.getKey().nom() + " (" + en.getKey().ip() + ")")
                    .collect(Collectors.joining("\n")));

        Files.writeString(Path.of(args[1]), rapport);
        System.out.println(rapport);
    }

    private static boolean ping(String ip) {
        try {
            return java.net.InetAddress.getByName(ip).isReachable(2000);
        } catch (Exception e) {
            return false;
        }
    }
}
```

**Ce que ce cas montre** : NIO (`Files.lines`), records, streams, text block `.formatted()`, virtual threads, `ConcurrentHashMap`. Un outil interne réel tient en ~70 lignes.

---

## 62. Cas pratique 2 — Mini client d'API REST avec retry

```java
import java.net.URI;
import java.net.http.*;
import java.time.Duration;

/** Interroge une API de supervision avec retry exponentiel. */
public class ApiClient {

    private static final HttpClient CLIENT = HttpClient.newBuilder()
        .connectTimeout(Duration.ofSeconds(5))
        .build();
    private static final int MAX_ESSAIS = 4;

    public static String getAvecRetry(String url) throws Exception {
        HttpRequest requete = HttpRequest.newBuilder()
            .uri(URI.create(url))
            .timeout(Duration.ofSeconds(10))
            .header("Accept", "application/json")
            .GET().build();

        Exception dernierEchec = null;
        for (int essai = 1; essai <= MAX_ESSAIS; essai++) {
            try {
                HttpResponse<String> rep = CLIENT.send(requete, HttpResponse.BodyHandlers.ofString());
                if (rep.statusCode() == 200) return rep.body();
                if (rep.statusCode() >= 400 && rep.statusCode() < 500)
                    throw new IllegalStateException("Erreur client HTTP " + rep.statusCode());
                dernierEchec = new IllegalStateException("HTTP " + rep.statusCode());
            } catch (java.io.IOException | InterruptedException e) {
                dernierEchec = e;   // réseau : on réessaie
                if (e instanceof InterruptedException) Thread.currentThread().interrupt();
            }
            long attenteMs = (long) (500 * Math.pow(2, essai - 1)); // backoff exponentiel
            Thread.sleep(attenteMs);
        }
        throw new IllegalStateException("Échec après " + MAX_ESSAIS + " essais", dernierEchec);
    }

    public static void main(String[] args) throws Exception {
        System.out.println(getAvecRetry("http://localhost:8080/api/equipements"));
    }
}
```

**Points à noter** : on ne retry **jamais** les 4xx (erreur du client, inutile) ; backoff exponentiel pour ne pas marteler ; `InterruptedException` → restaurer le flag d'interruption.

---

## 63. Cas pratique 3 — Import CSV vers base avec JDBC batch

```java
import java.nio.file.*;
import java.sql.*;
import java.util.stream.Stream;

/** Importe equipements.csv (nom;puissance;etat) en base via batch JDBC. */
public class ImportJdbc {

    public static void main(String[] args) throws Exception {
        String url = System.getenv("JDBC_URL");       // jamais en dur
        String user = System.getenv("JDBC_USER");
        String pass = System.getenv("JDBC_PASSWORD");

        try (Connection cx = DriverManager.getConnection(url, user, pass);
             Stream<String> lignes = Files.lines(Path.of(args[0]))) {

            cx.setAutoCommit(false);
            String sql = "INSERT INTO equipement(nom, puissance_kva, etat) VALUES (?, ?, ?) " +
                         "ON CONFLICT (nom) DO UPDATE SET puissance_kva = EXCLUDED.puissance_kva";
            try (PreparedStatement ps = cx.prepareStatement(sql)) {
                final int[] compteur = {0};
                lignes.filter(l -> !l.isBlank()).map(l -> l.split(";")).forEach(c -> {
                    try {
                        ps.setString(1, c[0].trim());
                        ps.setDouble(2, Double.parseDouble(c[1].trim()));
                        ps.setString(3, c[2].trim());
                        ps.addBatch();
                        if (++compteur[0] % 500 == 0) ps.executeBatch(); // flush tous les 500
                    } catch (SQLException e) { throw new RuntimeException(e); }
                });
                ps.executeBatch();  // reste
                cx.commit();
                System.out.println("Importé : " + compteur[0] + " lignes");
            } catch (Exception e) {
                cx.rollback();
                throw e;
            } finally {
                cx.setAutoCommit(true);
            }
        }
    }
}
```

**Points à noter** : batch (`addBatch`/`executeBatch`) = 10 à 100× plus rapide que des inserts un par un ; transaction unique + rollback ; upsert PostgreSQL (`ON CONFLICT`).

---

## 64. Cas pratique 4 — Planificateur de relevés avec logs et arrêt propre

```java
import java.time.*;
import java.util.concurrent.*;
import org.slf4j.*;

/** Toutes les 5 min : relève les compteurs, logue, s'arrête proprement sur SIGTERM. */
public class Planificateur {

    private static final Logger log = LoggerFactory.getLogger(Planificateur.class);

    public static void main(String[] args) {
        ScheduledExecutorService planif = Executors.newSingleThreadScheduledExecutor(
            r -> { Thread t = new Thread(r, "releve"); t.setDaemon(false); return t; });

        // Arrêt propre : flush + fin du tour en cours (max 30 s)
        Runtime.getRuntime().addShutdownHook(new Thread(() -> {
            log.info("Arrêt demandé, fin du relevé en cours...");
            planif.shutdown();
            try {
                if (!planif.awaitTermination(30, TimeUnit.SECONDS)) planif.shutdownNow();
            } catch (InterruptedException e) {
                planif.shutdownNow();
                Thread.currentThread().interrupt();
            }
            log.info("Arrêt terminé.");
        }, "shutdown-hook"));

        planif.scheduleAtFixedRate(() -> {
            try {
                Instant debut = Instant.now();
                int n = releverCompteurs();   // votre logique métier
                log.info("Relevé OK : {} équipements en {} ms",
                    n, Duration.between(debut, Instant.now()).toMillis());
            } catch (Exception e) {
                log.error("Relevé en échec", e);  // on logue, le planificateur continue
            }
        }, 0, 5, TimeUnit.MINUTES);

        log.info("Planificateur démarré (Ctrl+C pour arrêter).");
    }

    private static int releverCompteurs() {
        // ... SNMP / Modbus / HTTP vers les équipements ...
        return 42;
    }
}
```

**Points à noter** : `addShutdownHook` = indispensable dans les conteneurs (SIGTERM) ; le `catch` dans la tâche empêche le planificateur de mourir silencieusement après une exception ; nommer les threads aide énormément au diagnostic (`jstack`).

---

## 65. Checklists : nouveau projet & mise en production

### Nouveau projet

- [ ] JDK LTS (17/21) + `maven.compiler.release` aligné
- [ ] `.gitignore` (target/, .idea/, *.iml, .DS_Store)
- [ ] `README.md` : prérequis, build (`mvn package`), lancement, config (variables d'env.)
- [ ] Encodage UTF-8 forcé (`project.build.sourceEncoding`)
- [ ] Dépendances : JUnit 5 + Mockito + SLF4J/Logback dès le jour 1
- [ ] Formateur de code configuré (Spotless)
- [ ] `main` minimal qui démarre et logue sa version

### Mise en production

- [ ] `-Xmx` / `MaxRAMPercentage` réglé pour le conteneur
- [ ] Secrets via variables d'environnement ou coffre (jamais dans l'image)
- [ ] Logs vers stdout **ou** fichier rotatif (pas les deux en double), niveau INFO par défaut
- [ ] Healthcheck (`/actuator/health` en Spring Boot, ou endpoint `/health` maison)
- [ ] Shutdown hook (section 64) pour SIGTERM propre
- [ ] Heap dump on OOM (`-XX:+HeapDumpOnOutOfMemoryError`)
- [ ] Sauvegarde/restauration testée si état local (BDD embarquée, fichiers)
- [ ] Versions épinglées (Docker tag précis, pas `latest`)
- [ ] Runbook : où sont les logs, comment redémarrer, qui appeler

---

## 66. Pense-bête de poche

```text
COMPILATION / EXÉCUTION
  javac Main.java && java Main        java Main.java (script, Java 11+)
  mvn compile | mvn package | mvn test
  jshell                              REPL interactif

BASE
  var x = ...;        inférence (init obligatoire)
  "a".equals(b)       comparaison chaînes (jamais ==)
  s = s.trim();       String immutable : réassigner !
  (double) a / b      éviter la division entière
  new BigDecimal("1.10")  monnaie (jamais double, jamais new BigDecimal(1.1))

COLLECTIONS
  List.of / Set.of / Map.of   immutables (Java 9+)
  new ArrayList<>(...)         modifiable
  map.getOrDefault(k, def)     map.merge(k, 1, Integer::sum)
  list.removeIf(p)             suppression sûre pendant parcours

EXCEPTIONS
  try (var r = ...) { }       try-with-resources : TOUJOURS pour I/O, JDBC, sockets
  catch du spécifique au général
  throw new IllegalArgumentException("...")   validation d'arguments

STREAMS
  .stream().filter().map().collect(...)   .toList() = immutable
  Collectors.groupingBy / counting / summarizingDouble
  Optional : map / filter / orElse / orElseGet / orElseThrow / ifPresent

CONCURRENCE
  Executors.newVirtualThreadPerTaskExecutor()  I/O massif (Java 21+)
  Executors.newFixedThreadPool(n)              CPU-bound : n = nb cœurs
  shutdown() + awaitTermination                TOUJOURS arrêter les pools
  volatile = flag visible   synchronized/AtomicX = lire-modifier-écrire

DATES
  Instant.now()  horodatage UTC   LocalDate.now()  ZonedDateTime (affichage)
  DateTimeFormatter.ofPattern("dd/MM/yyyy HH:mm")  (thread-safe)

RÉSEAU / BDD
  HttpClient.newBuilder()...build()   UNE instance partagée
  PreparedStatement + ?               jamais de concat SQL
  HikariCP                            pool de connexions en prod

DEBUG
  jstack <pid>   threads    jmap -histo <pid>   heap
  -XX:+HeapDumpOnOutOfMemoryError
  Lire la stack trace du HAUT + première ligne "Caused by"
```

---

## 67. Glossaire

| Terme | Définition |
|---|---|
| JDK | Kit de développement (compilateur + outils + runtime) |
| JRE | Runtime seul (plus distribué séparément depuis Java 11) |
| JVM | Machine virtuelle qui exécute le bytecode |
| Bytecode | Code intermédiaire des `.class`, indépendant de l'OS |
| JIT | Compilation « juste-à-temps » du bytecode en natif à chaud |
| Heap | Zone mémoire des objets, gérée par le GC |
| GC | Ramasse-miettes (G1, ZGC, Shenandoah, Parallel) |
| LTS | Version à support long (8, 11, 17, 21, 25) |
| Maven/Gradle | Outils de build et gestion de dépendances |
| POJO | Simple objet Java, sans contrainte de framework |
| DTO | Objet de transfert de données (souvent un record) |
| Boxing | Conversion auto `int` ↔ `Integer` (a un coût) |
| Erasure | Effacement des génériques à la compilation |
| Immutable | Objet non modifiable après création (String, records, List.of) |
| Checked exception | Exception que le compilateur force à gérer (`IOException`) |
| Unchecked exception | `RuntimeException` : bug probable, non forcée |
| Stream | Pipeline lazy de traitement de données (≠ `InputStream` !) |
| Lambda | Fonction anonyme `(a, b) -> a + b` |
| Thread virtuel | Thread léger JVM (Java 21+), idéal I/O massif |
| Deadlock | Deux threads qui s'attendent mutuellement (verrous croisés) |
| Race condition | Résultat dépendant de l'ordre d'exécution des threads |
| NPE | `NullPointerException` : déréférencement de `null` |
| JAR | Archive Java exécutable/portable |
| JPMS | Système de modules Java (`module-info.java`) |
| Javadoc | Documentation générée depuis les commentaires `/** */` |
| Réflexion | Inspection/manipulation des classes à l'exécution |
| Backoff | Attente croissante entre retries (exponentiel) |
| Idempotent | Opération répétable sans effet supplémentaire (GET, PUT) |

---

## 68. Quiz : 10 questions + réponses

**Q1.** Que vaut `System.out.println(5 / 2);` et pourquoi ?
<details><summary>Réponse</summary>Affiche <code>2</code> : division entière entre deux <code>int</code>, la partie décimale est tronquée. Pour 2.5 : <code>5 / 2.0</code>.</details>

**Q2.** Pourquoi ce test échoue-t-il parfois : `if (scanner.nextLine() == "oui")` ?
<details><summary>Réponse</summary><code>==</code> compare les références, pas le contenu. Deux String au contenu identique peuvent être des objets différents. Utilisez <code>"oui".equals(scanner.nextLine())</code>.</details>

**Q3.** Quelle exception survient ici, et comment la corriger ?
```java
List<String> l = new ArrayList<>(List.of("a", "b"));
for (String s : l) { if (s.equals("a")) l.remove(s); }
```
<details><summary>Réponse</summary><code>ConcurrentModificationException</code> : modification pendant itération. Corriger avec <code>l.removeIf(s -> s.equals("a"))</code> ou un <code>Iterator</code> explicite et <code>it.remove()</code>.</details>

**Q4.** Checked ou unchecked : `IOException` ? `NullPointerException` ? `SQLException` ?
<details><summary>Réponse</summary><code>IOException</code> et <code>SQLException</code> : checked (gestion ou <code>throws</code> obligatoire). <code>NullPointerException</code> : unchecked (<code>RuntimeException</code>).</details>

**Q5.** Que garantit `try-with-resources` ?
<details><summary>Réponse</summary>La fermeture automatique des ressources <code>AutoCloseable</code> (fichiers, sockets, connexions), même si une exception survient, dans l'ordre inverse d'ouverture.</details>

**Q6.** Quelle est la différence entre `orElse` et `orElseGet` sur un `Optional` ?
<details><summary>Réponse</summary><code>orElse(valeur)</code> évalue son argument <strong>toujours</strong> (même si l'Optional est présent). <code>orElseGet(() -> ...)</code> n'appelle le supplier que si l'Optional est vide — à préférer pour les valeurs coûteuses.</details>

**Q7.** Pourquoi `map.get(cle)` peut-il être dangereux sans précaution ?
<details><summary>Réponse</summary>Il retourne <code>null</code> si la clé est absente → risque de NPE en chaîne. Utilisez <code>getOrDefault</code>, <code>computeIfAbsent</code>, ou testez <code>containsKey</code>.</details>

**Q8.** Dans quel cas préférer un virtual thread (Java 21+) à un thread plateforme ?
<details><summary>Réponse</summary>Tâches I/O-bound nombreuses (milliers de connexions/pings/appels HTTP) avec code bloquant simple. Pas de gain pour du calcul CPU pur.</details>

**Q9.** Pourquoi ce code est-il une faille de sécurité ?
```java
String sql = "SELECT * FROM users WHERE login = '" + login + "'";
Statement st = cx.createStatement();
st.executeQuery(sql);
```
<details><summary>Réponse</summary>Injection SQL : un login comme <code>' OR '1'='1</code> détourne la requête. Corriger avec <code>PreparedStatement</code> et paramètres <code>?</code>.</details>

**Q10.** Que fait ce code et quel est le piège ?
```java
String total = "";
for (String s : lignes) total += s + "\n";
```
<details><summary>Réponse</summary>Concatène les lignes, mais chaque <code>+=</code> crée un nouveau String (O(n²)). Sur gros volume : <code>StringBuilder</code> ou <code>String.join("\n", lignes)</code>.</details>

---

## 69. Pour aller plus loin

### Par priorité pour un responsable systèmes

1. **Spring Boot** — docs.spring.io : votre porte d'entrée vers les API REST, Spring Data JPA (BDD sans SQL manuel), Spring Security, Actuator (health/metrics pour votre supervision).
2. **Concurrence avancée** — *Java Concurrency in Practice* (Goetz) : la référence, toujours d'actualité sur les fondamentaux.
3. **JVM en production** — apprenez `jstat`/`jfr` (Java Flight Recorder : profiling sans surcoût, intégré au JDK).
4. **Protocoles métier** — bibliothèques : SNMP4J (SNMP), j2mod (Modbus), Paho (MQTT) : parfaites pour parler à onduleurs et équipements.
5. **Tests** — Testcontainers (vraie BDD Docker dans les tests), AssertJ (assertions lisibles).

### Feuille de route 6 mois (2 h/semaine)

| Mois | Objectif | Livrable |
|---|---|---|
| 1 | Bases + collections + exceptions | Outil CLI inventaire (cas pratique 1) |
| 2 | Streams, Optional, NIO, JDBC | Import CSV→BDD (cas pratique 3) |
| 3 | JUnit/Mockito, logging | Couverture > 70 % sur vos outils |
| 4 | Spring Boot REST | API d'inventaire exposée (cas pratique 2 + section 48) |
| 5 | Concurrence, virtual threads | Collecteur parallèle d'alarmes |
| 6 | Packaging, Docker, runbook | Outil packagé + déployé avec checklist section 65 |

### Ressources

- Documentation officielle : **docs.oracle.com/javase**
- Tutoriels : **dev.java** (site officiel, moderne)
- Pratique : **exercism.org** (track Java, mentorat gratuit)
- Questions : **stackoverflow.com** (tag java)
- Versions : adoptez une LTS et suivez les notes de version d'**OpenJDK**

---

*Fin du guide — bon code, et que vos NPE soient rares et vos builds verts.* 🍵
