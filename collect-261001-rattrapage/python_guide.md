# Guide Python complet — De l'installation aux scripts sysadmin

> **Public :** sysadmins, techniciens systèmes & réseaux, curieux de l'automatisation.
> **Angle :** pratique. Moins de théorie abstraite, plus de scripts qui marchent.
> **Version de référence :** Python 3.10+ (3.11/3.12/3.13 recommandés). Les notes "Python 3.10+" indiquent les syntaxes dépendantes de la version.

---

## Sommaire

1. Installation & tooling
2. pip et venv : l'environnement propre
3. Votre premier script
4. Syntaxe de base : indentation, commentaires, variables
5. Types fondamentaux
6. Nombres et opérations
7. Chaînes de caractères
8. Listes
9. Tuples
10. Dictionnaires
11. Sets (ensembles)
12. Conversions de types
13. Conditions (if/elif/else)
14. Boucles (for, while)
15. break, continue, else de boucle
16. Comprehensions
17. Fonctions : bases
18. Fonctions : *args, **kwargs, paramètres avancés
19. lambda, map, filter, sorted
20. Portée des variables (scope)
21. Gestion d'erreurs : try/except
22. Lever ses propres exceptions
23. Modules et imports
24. Packages et `__init__.py`
25. Fichiers : lecture/écriture
26. pathlib : manipuler les chemins
27. os, sys, shutil, subprocess
28. Entrées-sorties avancées (stdin/stdout/stderr)
29. Programmation orientée objet : classes
30. Héritage et polymorphisme
31. Méthodes spéciales (dunder)
32. Dataclasses
33. Décorateurs
34. Générateurs et itérateurs
35. Context managers (`with`)
36. Annotations de type (typing)
37. Expressions régulières (regex)
38. JSON, YAML, CSV
39. datetime : dates et heures
40. argparse : scripts en ligne de commande
41. logging : journaliser proprement
42. requests : HTTP(S) simplifié
43. Réseau : sockets, ping, inventaire
44. sqlite3 : base de données embarquée
45. Tests unitaires : unittest et pytest
46. Débogage : pdb et techniques
47. Asyncio : programmation asynchrone
48. Threading et multiprocessing
49. Performance : mesurer et optimiser
50. Packaging : pyproject.toml
51. Intro à FastAPI
52. Bonnes pratiques & PEP 8
53. Sécurité : écrire du code sûr
54. Scripts sysadmin n°1 : inventaire réseau
55. Scripts sysadmin n°2 : parsing de logs
56. Scripts sysadmin n°3 : sauvegarde et rotation
57. Scripts sysadmin n°4 : surveillance disque et alertes
58. Scripts sysadmin n°5 : requêtes API automatisées
59. Scripts sysadmin n°6 : traitement CSV/Excel
60. Scripts sysadmin n°7 : scan de ports et services
61. Erreurs classiques des débutants (20 pièges)
62. Cas pratiques commentés
63. Checklist "script de production"
64. Pense-bête de poche
65. Glossaire
66. Quiz (10 questions + réponses)
67. Pour aller plus loin

---

## 1. Installation & tooling

### Vérifier l'installation

```bash
python3 --version      # doit afficher 3.10 ou plus
python3 -m pip --version
```

### Installer Python

| OS | Méthode recommandée |
|---|---|
| Debian/Ubuntu | `sudo apt update && sudo apt install python3 python3-pip python3-venv` |
| RHEL/CentOS | `sudo dnf install python3 python3-pip` |
| Windows | Installeur python.org — **cocher "Add python.exe to PATH"** |
| macOS | `brew install python` |

> Sur Debian/Ubuntu récents, le système protège son Python : `pip install` hors venv refuse avec `externally-managed-environment`. C'est normal, utilisez un venv (section 2).

### Éditeurs et IDE

| Outil | Usage |
|---|---|
| VS Code + extension Python | Le plus courant, gratuit, débogueur intégré |
| PyCharm Community | IDE complet, excellent pour les gros projets |
| Vim/Neovim, nano | Édition rapide sur serveur |

### Vérifications rapides après installation

```bash
python3 -c "import sys; print(sys.version)"
python3 -m venv /tmp/testvenv && echo "venv OK"
```

### Checklist installation

- [ ] `python3 --version` >= 3.10
- [ ] `pip` fonctionnel (`python3 -m pip --version`)
- [ ] `venv` disponible (`python3 -m venv --help`)
- [ ] Éditeur configuré avec l'interpréteur correct

---

## 2. pip et venv : l'environnement propre

### Pourquoi un environnement virtuel ?

Chaque projet a ses dépendances (ex. `requests==2.31`). Sans venv, tout s'installe dans le Python système : conflits de versions garantis entre projets.

### Créer et activer un venv

```bash
python3 -m venv ~/venvs/projet1
source ~/venvs/projet1/bin/activate   # Linux/macOS
# Windows : ~\venvs\projet1\Scripts\activate
```

Une fois activé, le prompt affiche `(projet1)`. `pip` installe alors **dans le venv uniquement**.

### Commandes pip essentielles

```bash
pip install requests              # installer un paquet
pip install requests==2.31.0      # version précise
pip install -r requirements.txt   # depuis un fichier
pip freeze > requirements.txt     # figer les versions installées
pip list                          # paquets installés
pip show requests                 # détails d'un paquet
pip install --upgrade requests    # mettre à jour
pip uninstall requests            # désinstaller
```

### requirements.txt : exemple

```text
requests==2.31.0
pyyaml==6.0.1
rich==13.7.0
```

### Quitter / supprimer un venv

```bash
deactivate                        # quitter le venv
rm -rf ~/venvs/projet1            # supprimer = effacer le dossier
```

### Tableau récapitulatif venv

| Action | Commande |
|---|---|
| Créer | `python3 -m venv <nom>` |
| Activer (Linux) | `source <nom>/bin/activate` |
| Activer (Windows) | `<nom>\Scripts\activate` |
| Désactiver | `deactivate` |
| Lister les paquets | `pip list` |
| Figer | `pip freeze > requirements.txt` |

> **Réflexe pro :** un projet = un venv = un `requirements.txt` (ou `pyproject.toml`, section 50). Ne jamais `pip install` en root sur le Python système.

---

## 3. Votre premier script

Créez `hello.py` :

```python
#!/usr/bin/env python3
"""Mon premier script : affiche un message et la version de Python."""

import sys

def main():
    print("Bonjour, sysadmin !")
    print(f"Python {sys.version.split()[0]}")

if __name__ == "__main__":
    main()
```

Exécution :

```bash
chmod +x hello.py
./hello.py
# ou
python3 hello.py
```

### Anatomie d'un script

| Élément | Rôle |
|---|---|
| `#!/usr/bin/env python3` | Shebang : rend le script exécutable directement |
| `"""..."""` | Docstring : documentation du module |
| `def main():` | Point d'entrée logique |
| `if __name__ == "__main__":` | N'exécute `main()` que si le fichier est lancé directement (pas importé) |

> Le bloc `if __name__ == "__main__":` est une convention quasi obligatoire : il permet d'importer vos fonctions dans un autre script sans exécuter tout le fichier.

---

## 4. Syntaxe de base : indentation, commentaires, variables

### L'indentation remplace les accolades

```python
if True:
    print("indenté de 4 espaces")
    print("toujours 4 espaces")
print("hors du if")
```

- **4 espaces** par niveau (convention PEP 8). Pas de tabulations.
- Un mélange espaces/tabulations = `TabError`.

### Commentaires

```python
# Ceci est un commentaire sur une ligne

"""
Ceci est une docstring :
commentaire multi-lignes, utilisé pour documenter
modules, classes et fonctions.
"""
```

### Variables : pas de déclaration de type

```python
hostname = "srv-web-01"   # str
port = 22                 # int
actif = True              # bool
charge = 0.75             # float
```

- Le nom doit commencer par une lettre ou `_`, puis lettres/chiffres/`_`.
- Convention : `snake_case` pour variables et fonctions.
- Constantes : `MAJUSCULES` (convention, pas d'obligation technique).

```python
TIMEOUT = 30          # constante par convention
MAX_RETRIES = 3
```

### Affectations multiples

```python
a, b = 1, 2            # a=1, b=2
x = y = 0              # x=0 et y=0
host, port = "srv", 22 # déballage (unpacking)
```

### print() et f-strings

```python
nom = "srv-web-01"
print("Serveur :", nom)              # virgule = espace auto
print(f"Serveur : {nom}")            # f-string (Python 3.6+)
print(f"Port + 1 = {22 + 1}")        # expressions autorisées
print(f"Charge : {0.756:.1%}")       # formatage : 75,6%
```

### input() : lire au clavier

```python
reponse = input("Nom du serveur ? ")   # retourne toujours une str
port = int(input("Port ? "))           # conversion manuelle
```

---

## 5. Types fondamentaux

| Type | Exemple | Description |
|---|---|---|
| `int` | `42`, `-7` | Entiers, précision illimitée |
| `float` | `3.14`, `1e-3` | Nombres décimaux |
| `str` | `"texte"` | Chaînes (immutables) |
| `bool` | `True`, `False` | Booléens (attention à la majuscule) |
| `None` | `None` | Absence de valeur (`NoneType`) |
| `list` | `[1, 2]` | Liste modifiable |
| `tuple` | `(1, 2)` | Tuple immutable |
| `dict` | `{"a": 1}` | Dictionnaire clé→valeur |
| `set` | `{1, 2}` | Ensemble sans doublons |

```python
type(42)          # <class 'int'>
isinstance("x", str)   # True — préférez isinstance() à type() ==
```

### Valeurs "fausses" (falsy)

```python
bool(0), bool(0.0), bool(""), bool([]), bool({}), bool(None), bool(False)
# → tous False
```

Tout le reste est "vrai" (truthy). Utile pour tester rapidement :

```python
resultats = []
if not resultats:          # liste vide = falsy
    print("Aucun résultat")
```

---

## 6. Nombres et opérations

```python
7 + 3    # 10  addition
7 - 3    # 4   soustraction
7 * 3    # 21  multiplication
7 / 3    # 2.333... division (toujours float)
7 // 3   # 2   division entière
7 % 3    # 1   modulo (reste)
7 ** 3   # 343 puissance
-7       # négation
abs(-7)  # 7   valeur absolue
```

### Opérateurs d'affectation combinés

```python
compteur = 0
compteur += 1   # compteur = compteur + 1
compteur *= 2
```

### Fonctions numériques utiles

```python
round(3.14159, 2)   # 3.14
min(4, 1, 9)        # 1
max(4, 1, 9)        # 9
sum([1, 2, 3])      # 6
divmod(7, 3)        # (2, 1) → (quotient, reste)
pow(2, 10)          # 1024
```

### Précision des flottants (piège classique)

```python
0.1 + 0.2           # 0.30000000000000004 (!)
round(0.1 + 0.2, 2) # 0.3
```

> Pour de la monnaie ou des calculs exacts : module `decimal`.

```python
from decimal import Decimal
Decimal("0.1") + Decimal("0.2")   # Decimal('0.3')
```

### Conversions

```python
int("42")      # 42
int(3.9)       # 3 (tronque, n'arrondit pas)
float("3.14")  # 3.14
str(42)        # "42"
hex(255)       # '0xff'
bin(10)        # '0b1010'
```

---

## 7. Chaînes de caractères

### Bases

```python
s = "Bonjour"
s2 = 'Bonjour'          # ' ou " : équivalent
s3 = """Texte
sur plusieurs
lignes"""
```

### Indexation et slicing

```python
s = "srv-web-01"
s[0]      # 's'   premier caractère
s[-1]     # '1'   dernier caractère
s[0:3]    # 'srv' slice [début:fin[ (fin exclue)
s[4:]     # 'web-01'
s[:3]     # 'srv'
s[::-1]   # '10-bew-vrs' inversion
```

### Méthodes essentielles

```python
"  srv-01  ".strip()          # 'srv-01' (retire espaces)
"srv-01".upper()              # 'SRV-01'
"srv-01".lower()              # 'srv-01'
"srv-web-01".split("-")       # ['srv', 'web', '01']
"-".join(["srv", "01"])       # 'srv-01'
"srv-01".replace("srv", "db") # 'db-01'
"srv-01".startswith("srv")    # True
"log.txt".endswith(".txt")    # True
"ip=10.0.0.1".find("=")       # 2 (position, -1 si absent)
"a,b,c".split(",")            # ['a', 'b', 'c']
```

### f-strings : le formatage moderne

```python
host, port = "srv-01", 8080
f"{host}:{port}"              # 'srv-01:8080'
f"{port:05d}"                 # '08080' (zéro-padding)
f"{0.8567:.1%}"               # '85.7%'
f"{1234567:,}"                # '1,234,567'
f"{host!r}"                   # repr() : 'srv-01' avec guillemets
```

### Concaténation et répétition

```python
"srv" + "-" + "01"   # 'srv-01'
"ab" * 3             # 'ababab'
```

> Les `str` sont **immutables** : chaque opération crée une nouvelle chaîne. Pour construire un gros texte en boucle, accumulez dans une liste puis `""join(...)`.

```python
# Lent (crée N chaînes) :
texte = ""
for i in range(10000):
    texte += str(i)

# Rapide :
morceaux = [str(i) for i in range(10000)]
texte = "".join(morceaux)
```

---

## 8. Listes

```python
serveurs = ["srv-web-01", "srv-web-02", "srv-db-01"]
vide = []
mixte = [1, "deux", 3.0]      # possible mais déconseillé
```

### Accès et modification

```python
serveurs[0]          # 'srv-web-01'
serveurs[-1]         # dernier
serveurs[1] = "srv-web-03"   # modification (mutable !)
serveurs[0:2]        # ['srv-web-01', 'srv-web-03']
```

### Méthodes principales

```python
serveurs.append("srv-cache-01")   # ajoute à la fin
serveurs.insert(0, "srv-lb-01")   # insère à la position 0
serveurs.extend(["a", "b"])       # fusionne une autre liste
serveurs.remove("srv-lb-01")      # retire par valeur (1ère occurrence)
dernier = serveurs.pop()          # retire et retourne le dernier
premier = serveurs.pop(0)         # retire et retourne l'index 0
serveurs.sort()                   # tri en place
serveurs.reverse()                # inverse en place
serveurs.count("a")               # occurrences
serveurs.index("srv-db-01")       # position (ValueError si absent)
serveurs.clear()                  # vide la liste
```

### Fonctions utiles sur les listes

```python
len(serveurs)                  # taille
"srv-db-01" in serveurs        # True (test d'appartenance)
sorted(serveurs)               # trié, sans modifier l'original
reversed(serveurs)             # itérateur inversé
```

### Copier une liste (piège !)

```python
a = [1, 2, 3]
b = a          # b POINTE vers la même liste ! Modifier b modifie a.
b = a.copy()   # vraie copie (superficielle)
b = a[:]       # équivalent
import copy
b = copy.deepcopy(a)   # copie profonde (listes imbriquées)
```

### Parcours avec index

```python
for i, srv in enumerate(serveurs):
    print(f"{i}: {srv}")

for srv in serveurs:
    print(srv)
```

---

## 9. Tuples

Tuples = listes **immutables**. Idéals pour des données fixes (coordonnées, paires host/port).

```python
paire = ("srv-01", 22)
paire[0]          # 'srv-01'
paire[0] = "x"    # TypeError : immutable

# Tuple à 1 élément : la virgule est obligatoire
t = (42,)         # tuple
pas_tuple = (42)  # int !
```

### Déballage (unpacking)

```python
host, port = ("srv-01", 22)
a, b, *reste = (1, 2, 3, 4, 5)   # a=1, b=2, reste=[3, 4, 5]

# Échange de variables sans temporaire
a, b = b, a
```

### Pourquoi des tuples ?

- Clés de dictionnaire possibles (les listes ne le sont pas).
- Retour multiple de fonction : `return host, port` retourne un tuple.
- Protection contre la modification accidentelle.

---

## 10. Dictionnaires

```python
srv = {"hostname": "srv-web-01", "ip": "10.0.0.11", "port": 22}
vide = {}
```

### Accès

```python
srv["hostname"]              # 'srv-web-01'
srv["os"]                    # KeyError !
srv.get("os")                # None (pas d'erreur)
srv.get("os", "linux")       # 'linux' (valeur par défaut)
```

### Modification

```python
srv["os"] = "debian"         # ajout / modification
srv["port"] = 2222
del srv["port"]              # suppression
val = srv.pop("ip")          # retire et retourne
srv.update({"ram": "16G"})   # fusion
srv.setdefault("os", "linux")# n'écrase pas si existe déjà
```

### Parcours

```python
for cle in srv:                    # clés
    print(cle)
for cle, valeur in srv.items():    # paires
    print(f"{cle} = {valeur}")
for valeur in srv.values():        # valeurs
    print(valeur)
```

### Méthodes et tests

```python
"hostname" in srv      # True
len(srv)               # nombre de paires
list(srv.keys())       # ['hostname', 'ip', 'port']
```

### dict en compréhension (Python 3.x)

```python
carres = {x: x**2 for x in range(5)}   # {0: 0, 1: 1, 2: 4, ...}
```

> Depuis Python 3.7, les dicts **conservent l'ordre d'insertion**. On peut s'y fier.

---

## 11. Sets (ensembles)

Ensembles non ordonnés, **sans doublons**. Parfaits pour dédupliquer et comparer.

```python
ips = {"10.0.0.1", "10.0.0.2", "10.0.0.1"}  # {'10.0.0.1', '10.0.0.2'}
vide = set()          # {} crée un DICT vide, pas un set !
```

### Opérations ensemblistes

```python
a = {"web", "db", "cache"}
b = {"db", "lb"}

a | b   # union : {'web', 'db', 'cache', 'lb'}
a & b   # intersection : {'db'}
a - b   # différence : {'web', 'cache'}
a ^ b   # différence symétrique : {'web', 'cache', 'lb'}
```

### Méthodes

```python
ips.add("10.0.0.3")
ips.discard("10.0.0.9")   # pas d'erreur si absent
ips.remove("10.0.0.1")    # KeyError si absent
```

### Cas d'usage sysadmin : dédupliquer des IPs de logs

```python
with open("/var/log/auth.log") as f:
    ips = {ligne.split()[10] for ligne in f if "Failed" in ligne}
print(f"{len(ips)} IP(s) distinctes en échec d'authentification")
```

---

## 12. Conversions de types

| Depuis → Vers | Code | Note |
|---|---|---|
| str → int | `int("42")` | `ValueError` si non numérique |
| str → float | `float("3.14")` | |
| nombre → str | `str(42)` | |
| list → set | `set([1,1,2])` | déduplique |
| set → list | `list({1,2})` | ordre non garanti |
| list → tuple | `tuple([1,2])` | |
| liste de paires → dict | `dict([("a",1)])` | |
| str → list de lignes | `"a\nb".splitlines()` | |
| str → bool | `s.lower() == "true"` | pas de `bool("False")` ! |

> **Piège :** `bool("False")` vaut `True` (chaîne non vide = truthy). Pour parser du booléen depuis un fichier de config, comparez explicitement.

```python
def str_to_bool(s):
    return s.strip().lower() in ("1", "true", "yes", "on")
```

---

## 13. Conditions (if/elif/else)

```python
charge_cpu = 85

if charge_cpu > 90:
    print("CRITIQUE")
elif charge_cpu > 75:
    print("Attention")
else:
    print("OK")
```

### Opérateurs de comparaison

| Opérateur | Sens |
|---|---|
| `==` | égal |
| `!=` | différent |
| `>`, `<`, `>=`, `<=` | ordre |
| `is` | même objet (identité, pas égalité) |
| `is not` | objets différents |
| `in` | appartenance |

```python
# Comparaisons chaînées (lisible !)
if 0 <= charge_cpu <= 100:
    print("valeur plausible")
```

### Opérateurs logiques

```python
if disque_plein and not maintenance:
    alerter()
if port == 22 or port == 2222:
    print("SSH")
if not reponse:
    print("vide")
```

> `and`/`or` sont **court-circuités** : `or` s'arrête au premier truthy. Pratique pour des valeurs par défaut : `timeout = timeout or 30`.

### Ternaire (expression conditionnelle)

```python
etat = "CRITIQUE" if charge > 90 else "OK"
```

### `is` vs `==`

```python
a = [1, 2]; b = [1, 2]
a == b   # True  (même contenu)
a is b   # False (objets différents)

x = None
if x is None:   # toujours 'is' pour None, jamais '=='
    ...
```

---

## 14. Boucles (for, while)

### for : parcours d'itérables

```python
for srv in ["web-01", "web-02"]:
    print(f"Ping {srv}...")

for i in range(5):          # 0,1,2,3,4
    print(i)

for i in range(2, 10, 2):   # 2,4,6,8 (début, fin exclue, pas)
    print(i)

for cle, val in config.items():
    print(cle, val)
```

### while : tant qu'une condition est vraie

```python
tentatives = 0
while tentatives < 3:
    if ping(host):
        break
    tentatives += 1
    time.sleep(5)
```

> **Danger :** boucle infinie si la condition ne devient jamais fausse. Prévoyez toujours une porte de sortie (`break`, compteur, timeout).

```python
# Boucle infinie contrôlée (supervision)
while True:
    verifier_services()
    time.sleep(60)
```

---

## 15. break, continue, else de boucle

```python
for srv in serveurs:
    if srv == "srv-hs":
        continue        # saute ce serveur, passe au suivant
    if ping(srv):
        print(f"{srv} OK")
        break           # sort de la boucle au premier succès
else:
    # Exécuté SEULEMENT si la boucle n'a pas rencontré de break
    print("Aucun serveur joignable")
```

| Instruction | Effet |
|---|---|
| `break` | Sort immédiatement de la boucle |
| `continue` | Saute à l'itération suivante |
| `else` (de boucle) | Exécuté si pas de `break` |

> Le `else` de boucle surprend : pensez "pas de break → else". Très utile pour les recherches ("trouvé ? sinon...").

---

## 16. Comprehensions

Façon concise et rapide de construire des collections.

### Listes en compréhension

```python
carres = [x**2 for x in range(10)]

# Avec filtre
pairs = [x for x in range(20) if x % 2 == 0]

# Équivalent verbeux :
pairs = []
for x in range(20):
    if x % 2 == 0:
        pairs.append(x)
```

### Dict et set en compréhension

```python
longueurs = {srv: len(srv) for srv in serveurs}
ports_uniques = {s["port"] for s in serveurs_list}
```

### Exemple sysadmin : filtrer des lignes de log

```python
erreurs = [l for l in open("/var/log/syslog") if "error" in l.lower()]
ips_suspectes = {l.split()[0] for l in open("access.log") if " 404 " in l}
```

### Règles de bon usage

- 1 compréhension = 1 transformation simple. Au-delà, écrivez une boucle normale.
- Pas de double `for` imbriqué illisible : la lisibilité prime.
- Les générateurs `(x for x in ...)` (parenthèses) ne construisent rien en mémoire — voir section 34.

---

## 17. Fonctions : bases

```python
def ping_host(host, timeout=5):
    """Ping un hôte. Retourne True si joignable.

    Args:
        host: nom ou IP de l'hôte.
        timeout: délai en secondes.
    """
    # ... code ...
    return True
```

### Appels

```python
ping_host("srv-01")                 # timeout=5 par défaut
ping_host("srv-01", timeout=2)       # argument nommé
ping_host(host="srv-01", timeout=2)
```

### return

```python
def diviser(a, b):
    if b == 0:
        return None        # sortie anticipée
    return a / b

resultat = diviser(10, 2)  # 5.0
```

> Une fonction sans `return` retourne `None`. `return` seul = `return None`.

### Docstrings : documentez toujours

Convention : 1ère ligne = résumé impératif, puis détails. Outils comme `help()` et les IDE les affichent.

```python
help(ping_host)   # affiche la docstring
```

---

## 18. Fonctions : *args, **kwargs, paramètres avancés

```python
def journaliser(niveau, *messages, **metadonnees):
    print(f"[{niveau}]", *messages, metadonnees)

journaliser("INFO", "démarrage", "ok", host="srv-01")
# messages = ("démarrage", "ok"), metadonnees = {"host": "srv-01"}
```

### Ordre des paramètres

```python
def f(positionnel, *args, defaut=10, **kwargs):
    ...
```

1. Paramètres positionnels
2. `*args` (tuple des positionnels supplémentaires)
3. Paramètres nommés avec défaut
4. `**kwargs` (dict des nommés supplémentaires)

### Déballage à l'appel

```python
args = ["srv-01", 22]
ping_host(*args)                 # ping_host("srv-01", 22)

opts = {"timeout": 2}
ping_host("srv-01", **opts)     # ping_host("srv-01", timeout=2)
```

### Paramètres positionnels-only et nommés-only (Python 3.8+)

```python
def connecter(host, port, /, *, timeout=5):
    # host, port : positionnels uniquement ; timeout : nommé uniquement
    ...
connecter("srv-01", 22, timeout=2)   # OK
```

---

## 19. lambda, map, filter, sorted

### lambda : petite fonction anonyme

```python
double = lambda x: x * 2
double(21)   # 42
```

Utile comme argument court :

```python
serveurs = [{"nom": "b", "charge": 80}, {"nom": "a", "charge": 30}]
tries = sorted(serveurs, key=lambda s: s["charge"])
# [{'nom': 'a', ...}, {'nom': 'b', ...}]
```

### map / filter

```python
list(map(str.upper, ["a", "b"]))        # ['A', 'B']
list(filter(lambda x: x > 2, [1, 2, 3]))  # [3]

# En général, préférez les comprehensions (plus lisibles) :
[x.upper() for x in ["a", "b"]]
[x for x in [1, 2, 3] if x > 2]
```

### sorted avec key

```python
# Trier des IPs correctement (tri numérique, pas alphabétique !)
ips = ["10.0.0.11", "10.0.0.2"]
sorted(ips)  # ['10.0.0.11', '10.0.0.2']  ← FAUX ordre réseau
sorted(ips, key=lambda ip: tuple(int(p) for p in ip.split(".")))
# ['10.0.0.2', '10.0.0.11']  ← correct
```

> `sorted()` retourne une **nouvelle** liste ; `list.sort()` trie **en place** et retourne `None`.

---

## 20. Portée des variables (scope)

```python
COMPTEUR = 0          # globale (module)

def incrementer():
    global COMPTEUR   # déclare qu'on modifie la globale
    COMPTEUR += 1

def exemple():
    x = 10            # locale : invisible dehors
    print(x)
```

### Règles LEGB

1. **L**ocal : dans la fonction
2. **E**nclosing : fonction englobante (closures)
3. **G**lobal : module
4. **B**uiltins : `print`, `len`...

```python
def externe():
    total = 0
    def interne():
        nonlocal total   # modifie la variable de 'externe'
        total += 1
    interne()
    return total
```

### Bonnes pratiques

- Évitez `global` : préférez passer des arguments et retourner des valeurs.
- Les fonctions pures (mêmes entrées → mêmes sorties, pas d'effet de bord) sont plus faciles à tester.

---

## 21. Gestion d'erreurs : try/except

```python
try:
    with open("config.ini") as f:
        contenu = f.read()
except FileNotFoundError:
    print("Fichier introuvable, utilisation des défauts")
    contenu = ""
except PermissionError:
    print("Droits insuffisants")
    raise                       # re-lève l'exception après log
except OSError as e:            # attrape les autres erreurs OS
    print(f"Erreur OS : {e}")
else:
    print("Lecture OK")         # exécuté si PAS d'exception
finally:
    print("Toujours exécuté")   # nettoyage garanti
```

### Hiérarchie d'exceptions courantes

```
BaseException
 └─ Exception
     ├─ ValueError        (int("abc"))
     ├─ TypeError         ("a" + 1)
     ├─ KeyError          (dict["absent"])
     ├─ IndexError        (liste[99])
     ├─ FileNotFoundError
     ├─ PermissionError
     ├─ TimeoutError
     └─ RuntimeError
```

### Bonnes pratiques

| À faire | À éviter |
|---|---|
| Attraper l'exception la plus précise | `except:` nu (attrape tout, même Ctrl+C) |
| Logger le contexte | `pass` silencieux |
| `raise` pour re-propager | écraser l'info d'origine |

```python
# À éviter absolument :
try:
    risky()
except:        # attrape même KeyboardInterrupt et SystemExit !
    pass
```

---

## 22. Lever ses propres exceptions

```python
class ConfigError(Exception):
    """Erreur de configuration de l'application."""
    pass

def charger_config(chemin):
    if not chemin.endswith((".ini", ".yaml")):
        raise ConfigError(f"Format non supporté : {chemin}")
```

### raise ... from (chaînage)

```python
try:
    port = int(valeur)
except ValueError as e:
    raise ConfigError(f"Port invalide : {valeur!r}") from e
```

Le `from` conserve la cause d'origine dans la traceback — indispensable pour déboguer.

### assert : vérifications internes

```python
def pourcentage(valeur, total):
    assert total > 0, "total doit être > 0"
    return valeur / total * 100
```

> `assert` = garde-fou pendant le développement. Ne pas s'en servir pour valider des entrées utilisateur en production (désactivable avec `python -O`).

---

## 23. Modules et imports

Un module = un fichier `.py`. L'importer exécute le fichier une fois, puis le met en cache (`sys.modules`).

```python
import os                    # import simple
import os.path as osp        # alias
from pathlib import Path     # import ciblé
from collections import defaultdict, Counter
```

### Ce qu'il faut éviter

```python
from module import *   # À ÉVITER : pollue le namespace, masque les noms
```

### Où Python cherche les modules (`sys.path`)

1. Dossier du script lancé
2. `PYTHONPATH` (variable d'environnement)
3. Dossiers d'installation (site-packages du venv...)

```python
import sys
print("\n".join(sys.path))
```

### Imports relatifs (dans un package)

```python
from . import utils        # même package
from ..config import PARAM # package parent
```

> Les imports relatifs ne marchent qu'à l'intérieur d'un package importé, pas dans un script lancé directement.

---

## 24. Packages et `__init__.py`

```
mon_projet/
├── pyproject.toml
├── main.py
└── outils/
    ├── __init__.py
    ├── reseau.py
    └── logs.py
```

```python
# main.py
from outils.reseau import ping_host
from outils import logs
```

- `__init__.py` (même vide) marque le dossier comme package importable.
- Depuis Python 3.3, les *namespace packages* marchent sans `__init__.py`, mais le garder reste la pratique claire.

### `__all__` : contrôler `from x import *`

```python
# outils/reseau.py
__all__ = ["ping_host", "scan_port"]
```

### Exécuter un module comme script

```bash
python3 -m outils.reseau   # exécute outils/reseau.py avec le bon sys.path
```

> Préférez `python -m package.module` à `python package/module.py` : les imports relatifs et `sys.path` sont corrects.

---

## 25. Fichiers : lecture/écriture

### La règle d'or : `with open(...)`

```python
# Lecture
with open("serveurs.txt", encoding="utf-8") as f:
    contenu = f.read()            # tout le fichier (str)

with open("serveurs.txt", encoding="utf-8") as f:
    lignes = f.readlines()        # liste des lignes

with open("serveurs.txt", encoding="utf-8") as f:
    for ligne in f:               # itération ligne à ligne (mémoire OK)
        print(ligne.rstrip("\n"))
```

Le `with` **ferme le fichier automatiquement**, même en cas d'exception.

### Écriture

```python
with open("rapport.txt", "w", encoding="utf-8") as f:   # "w" écrase !
    f.write("Rapport du 2026-09-26\n")

with open("journal.log", "a", encoding="utf-8") as f:   # "a" ajoute
    f.write("nouvelle entrée\n")
```

### Modes d'ouverture

| Mode | Effet |
|---|---|
| `"r"` | Lecture (défaut), erreur si absent |
| `"w"` | Écriture, **écrase** le fichier |
| `"a"` | Ajout en fin de fichier |
| `"x"` | Création exclusive (erreur si existe) |
| `"r+"` | Lecture + écriture |
| `"b"` | Binaire (`"rb"`, `"wb"`) : images, zip... |

### Toujours préciser `encoding="utf-8"`

Sans `encoding`, Python utilise l'encodage locale du système (sur Windows : cp1252 → caractères accentués cassés).

### Gros fichiers : ne pas tout charger

```python
# Bien : streaming ligne par ligne
with open("gros.log", encoding="utf-8", errors="replace") as f:
    for ligne in f:
        traiter(ligne)

# errors="replace" : remplace les octets invalides par � au lieu de planter
```

---

## 26. pathlib : manipuler les chemins

`pathlib` = l'approche moderne (orientée objet). Préférez-la à `os.path`.

```python
from pathlib import Path

p = Path("/var/log/syslog")
p.name        # 'syslog'
p.parent      # Path('/var/log')
p.suffix      # ''
p.stem        # 'syslog'
p.exists()    # True/False
p.is_file()
p.is_dir()

# Construction multi-OS (gère / vs \)
dossier = Path("/srv") / "backups" / "2026-09-26"
```

### Opérations courantes

```python
Path("rapport.txt").read_text(encoding="utf-8")     # lit tout
Path("rapport.txt").write_text("hello", encoding="utf-8")

# Lister un dossier
for f in Path("/var/log").glob("*.log"):
    print(f)

for f in Path("/srv").rglob("*.conf"):   # récursif
    print(f)

Path("nouveau/dossier").mkdir(parents=True, exist_ok=True)
```

### Tableau os.path vs pathlib

| Besoin | os.path (ancien) | pathlib (moderne) |
|---|---|---|
| Joindre | `os.path.join(a, b)` | `Path(a) / b` |
| Nom du fichier | `os.path.basename(p)` | `Path(p).name` |
| Existe ? | `os.path.exists(p)` | `Path(p).exists()` |
| Lister | `os.listdir(d)` | `Path(d).iterdir()` |

> `pathlib` accepte les `~` via `Path.home()` et `Path("~/x").expanduser()`.

---

## 27. os, sys, shutil, subprocess

### os : système d'exploitation

```python
import os
os.getcwd()                 # dossier courant
os.listdir("/tmp")          # contenu (noms simples)
os.environ.get("HOME")      # variable d'environnement
os.environ.get("TIMEOUT", "30")
os.cpu_count()              # nombre de CPU
os.uname()                  # infos système (Linux/macOS)
```

### sys : interpréteur

```python
import sys
sys.argv                    # arguments CLI : ["script.py", "arg1", ...]
sys.exit(1)                 # quitte avec code de retour 1
sys.version_info            # (3, 11, ...)
sys.platform                # 'linux', 'win32', 'darwin'
sys.stderr.write("erreur\n")
```

### shutil : opérations fichiers de haut niveau

```python
import shutil
shutil.copy("a.txt", "b.txt")          # copie fichier
shutil.copytree("src", "dst")          # copie arborescence
shutil.move("a.txt", "archives/")      # déplace/renomme
shutil.rmtree("vieux_dossier")         # supprime récursivement (!)
shutil.disk_usage("/").free            # espace libre en octets
shutil.which("docker")                 # trouve un exécutable dans le PATH
```

### subprocess : lancer des commandes externes

```python
import subprocess

# Méthode moderne et sûre :
res = subprocess.run(
    ["ping", "-c", "2", "10.0.0.1"],
    capture_output=True, text=True, timeout=10,
)
print(res.returncode)   # 0 = succès
print(res.stdout)       # sortie standard

# Vérifier le code de retour automatiquement :
subprocess.run(["systemctl", "is-active", "nginx"], check=True)
# lève CalledProcessError si returncode != 0
```

> **Sécurité :** passez la commande en **liste**, jamais en chaîne avec `shell=True` (injection de commandes — voir section 53).

```python
# DANGEREUX si 'host' vient d'un utilisateur :
subprocess.run(f"ping -c1 {host}", shell=True)

# SÛR :
subprocess.run(["ping", "-c1", host])
```

---

## 28. Entrées-sorties avancées (stdin/stdout/stderr)

```python
import sys

# Lire depuis un pipe : cat log.txt | python3 script.py
for ligne in sys.stdin:
    traiter(ligne)

# Écrire sur stderr (les erreurs ne polluent pas le pipe)
print("Échec du ping", file=sys.stderr)

# Codes de retour : 0 = OK, autre = erreur
sys.exit(0)
sys.exit(2)   # convention : 2 = usage incorrect
```

### Script "filtre" Unix typique

```python
#!/usr/bin/env python3
"""Filtre les lignes contenant ERREUR (usage : cat f.log | python3 filtre.py)."""
import sys

def main():
    for ligne in sys.stdin:
        if "ERREUR" in ligne.upper():
            sys.stdout.write(ligne)

if __name__ == "__main__":
    main()
```

```bash
cat /var/log/syslog | python3 filtre.py > erreurs.txt
echo "code retour : $?"
```

---

## 29. Programmation orientée objet : classes

```python
class Serveur:
    """Représente un serveur supervisé."""

    def __init__(self, hostname, ip, port=22):
        self.hostname = hostname
        self.ip = ip
        self.port = port
        self.en_ligne = None   # inconnu au départ

    def ping(self, timeout=2):
        """Simule un ping. Retourne True/False."""
        # ... vrai code ici ...
        self.en_ligne = True
        return self.en_ligne

    def __str__(self):
        return f"{self.hostname} ({self.ip})"
```

```python
srv = Serveur("web-01", "10.0.0.11")
print(srv)            # web-01 (10.0.0.11)  → via __str__
srv.ping()
```

### Vocabulaire

| Terme | Exemple |
|---|---|
| Classe | `Serveur` (le moule) |
| Instance / objet | `srv` (l'exemplaire) |
| Attribut | `srv.hostname` (donnée) |
| Méthode | `srv.ping()` (fonction de l'objet) |
| `self` | l'instance elle-même (1er paramètre, obligatoire) |
| `__init__` | constructeur, appelé à la création |

### Attributs de classe vs d'instance

```python
class Compteur:
    total = 0              # attribut de CLASSE : partagé
    def __init__(self):
        self.n = 0         # attribut d'INSTANCE : propre à chacune
        Compteur.total += 1
```

---

## 30. Héritage et polymorphisme

```python
class Equipement:
    def __init__(self, nom, ip):
        self.nom = nom
        self.ip = ip

    def decrire(self):
        return f"{self.nom} @ {self.ip}"

class Switch(Equipement):          # Switch hérite d'Equipement
    def __init__(self, nom, ip, nb_ports):
        super().__init__(nom, ip)  # appelle le constructeur parent
        self.nb_ports = nb_ports

    def decrire(self):             # surcharge (override)
        return f"Switch {super().decrire()} - {self.nb_ports} ports"

class Onduleur(Equipement):
    def __init__(self, nom, ip, puissance_kva):
        super().__init__(nom, ip)
        self.puissance_kva = puissance_kva
```

```python
parc = [Switch("sw-01", "10.0.0.2", 48), Onduleur("ups-01", "10.0.0.3", 20)]
for eq in parc:
    print(eq.decrire())   # polymorphisme : même appel, comportements différents
```

### Quand hériter, quand composer ?

- Héritage = relation **"est un"** (un Switch *est un* Équipement).
- Composition = relation **"a un"** (un Serveur *a une* CarteReseau) → souvent préférable, plus souple.

> En pratique sysadmin, on utilise l'OOP avec modération : des classes pour modéliser le parc, des fonctions pour les traitements.

---

## 31. Méthodes spéciales (dunder)

Les méthodes `__x__` permettent d'intégrer vos objets au langage.

```python
class PlageIP:
    def __init__(self, debut, fin):
        self.debut, self.fin = debut, fin

    def __len__(self):                 # len(obj)
        return self.fin - self.debut + 1

    def __contains__(self, ip):        # ip in obj
        return self.debut <= ip <= self.fin

    def __repr__(self):                # représentation "officielle"
        return f"PlageIP({self.debut}, {self.fin})"

    def __eq__(self, autre):           # obj1 == obj2
        return (self.debut, self.fin) == (autre.debut, autre.fin)
```

| Méthode | Déclenchée par |
|---|---|
| `__init__` | création `C(...)` |
| `__str__` | `print(obj)`, `str(obj)` |
| `__repr__` | `repr(obj)`, affichage en console |
| `__len__` | `len(obj)` |
| `__eq__` | `==` |
| `__lt__` | `<` (permet `sorted()`) |
| `__contains__` | `in` |
| `__iter__` | `for x in obj` |

> Définissez toujours `__repr__` pour vos classes métier : le débogage devient 10x plus lisible.

---

## 32. Dataclasses

Pour les classes qui sont surtout des conteneurs de données (Python 3.7+).

```python
from dataclasses import dataclass, field

@dataclass
class Interface:
    nom: str
    ip: str
    masque: str = "255.255.255.0"
    active: bool = True
    tags: list = field(default_factory=list)  # obligatoire pour les mutables !
```

Équivaut à écrire `__init__`, `__repr__`, `__eq__` à la main :

```python
eth0 = Interface("eth0", "10.0.0.11")
print(eth0)
# Interface(nom='eth0', ip='10.0.0.11', masque='255.255.255.0', active=True, tags=[])
```

### Options utiles

```python
@dataclass(frozen=True)     # immutable (comme un namedtuple amélioré)
class Config:
    host: str
    port: int = 22

@dataclass(order=True)      # génère <, <=, >, >= (tri possible)
class Alarme:
    niveau: int
    message: str
```

> **Piège :** ne jamais mettre `tags: list = []` en défaut mutable — toutes les instances partageraient la même liste. Utilisez `field(default_factory=list)`.

---

## 33. Décorateurs

Un décorateur enveloppe une fonction pour ajouter un comportement (log, cache, retry...).

```python
import functools
import time

def chronometrer(fonction):
    @functools.wraps(fonction)   # conserve nom et docstring
    def enveloppe(*args, **kwargs):
        debut = time.perf_counter()
        resultat = fonction(*args, **kwargs)
        duree = time.perf_counter() - debut
        print(f"{fonction.__name__} : {duree:.3f}s")
        return resultat
    return enveloppe

@chronometrer
def sauvegarde(dossier):
    # ... code long ...
    return "ok"
```

### Décorateur avec paramètres : retry automatique

```python
def reessayer(tentatives=3, delai=2):
    def decorateur(fonction):
        @functools.wraps(fonction)
        def enveloppe(*args, **kwargs):
            for i in range(tentatives):
                try:
                    return fonction(*args, **kwargs)
                except Exception as e:
                    if i == tentatives - 1:
                        raise
                    print(f"Tentative {i+1} échouée ({e}), nouvel essai...")
                    time.sleep(delai)
        return enveloppe
    return decorateur

@reessayer(tentatives=3, delai=5)
def requete_api(url):
    ...
```

### Décorateurs standards utiles

| Décorateur | Usage |
|---|---|
| `@functools.lru_cache` | Mémorise les résultats (cache) |
| `@property` | Attribut calculé (voir ci-dessous) |
| `@staticmethod` / `@classmethod` | Méthodes sans instance |

```python
from functools import lru_cache

@lru_cache(maxsize=128)
def resolution_dns(hostname):
    # coûteux → mis en cache automatiquement
    ...
```

```python
class Serveur:
    def __init__(self, ip):
        self.ip = ip

    @property
    def reseau(self):
        return ".".join(self.ip.split(".")[:3]) + ".0/24"

srv = Serveur("10.0.5.11")
print(srv.reseau)   # 10.0.5.0/24 — appelé comme un attribut !
```

---

## 34. Générateurs et itérateurs

Un générateur produit les valeurs **à la demande**, sans tout stocker en mémoire.

```python
def lignes_erreurs(chemin):
    with open(chemin, encoding="utf-8", errors="replace") as f:
        for ligne in f:
            if "error" in ligne.lower():
                yield ligne        # pause ici, reprend au prochain next()

# Usage : ne charge JAMAIS tout le fichier en RAM
for ligne in lignes_erreurs("/var/log/syslog"):
    print(ligne, end="")
```

### Générateur en compréhension

```python
total = sum(int(l.split()[3]) for l in open("tailles.txt"))
# ^ parenthèses = générateur : 1 ligne en mémoire à la fois
```

### itertools : la boîte à outils

```python
import itertools

itertools.islice(range(10**9), 5)        # les 5 premiers, sans tout générer
itertools.chain([1, 2], [3, 4])          # chaîner des itérables
itertools.groupby(donnees_triees, key=...)  # regrouper (données triées !)
itertools.batched(range(10), 3)          # par paquets de 3 (Python 3.12+)
```

> Pour traiter des logs de plusieurs Go : générateurs + `itertools` = mémoire constante.

---

## 35. Context managers (`with`)

Le `with` garantit le nettoyage (fermeture, verrou, connexion...).

```python
with open("f.txt") as f:      # ferme le fichier à la sortie
    ...

import threading
verrou = threading.Lock()
with verrou:                   # libère le verrou à la sortie
    ...
```

### Créer son context manager (2 façons)

```python
# Façon 1 : classe avec __enter__ / __exit__
class Chrono:
    def __enter__(self):
        self.debut = time.perf_counter()
        return self
    def __exit__(self, exc_type, exc, tb):
        print(f"Durée : {time.perf_counter() - self.debut:.2f}s")
        return False   # False = propage l'exception éventuelle

with Chrono():
    traitement_lourd()

# Façon 2 : décorateur @contextmanager (plus simple)
from contextlib import contextmanager

@contextmanager
def connexion_bd(chemin):
    conn = ouvrir(chemin)
    try:
        yield conn          # le 'as conn' reçoit ceci
    finally:
        conn.close()        # toujours exécuté

with connexion_bd("parc.db") as conn:
    ...
```

---

## 36. Annotations de type (typing)

Les annotations documentent les types attendus. Python ne les vérifie **pas** à l'exécution (sauf outils externes comme `mypy`).

```python
def ping(host: str, timeout: float = 5.0) -> bool:
    ...

# Variables
compteur: int = 0
serveurs: list[str] = []            # Python 3.9+ (sinon List[str] de typing)
config: dict[str, str] = {}
```

### Types courants

```python
from typing import Optional, Union

def trouver(nom: str) -> Optional[str]:
    # retourne str ou None
    ...

# Python 3.10+ : syntaxe moderne avec |
def trouver(nom: str) -> str | None:
    ...

def traiter(valeur: int | str) -> None: ...
```

### Pourquoi typer ?

- L'IDE détecte les erreurs avant l'exécution (autocomplétion, refactoring).
- La signature devient une documentation vivante.
- `mypy` en CI attrape des bugs : `mypy mon_script.py`.

> Pour un sysadmin : typez au moins les signatures des fonctions partagées/réutilisées. Pas besoin de tout typer dans un script jetable.

---

## 37. Expressions régulières (regex)

```python
import re

log = "2026-09-26 10:15:32 FAILED login from 192.168.1.25 port 22"

# Recherche
m = re.search(r"from (\d+\.\d+\.\d+\.\d+)", log)
if m:
    print(m.group(1))   # 192.168.1.25

# Toutes les occurrences
re.findall(r"\d+\.\d+\.\d+\.\d+", log)

# Remplacement
re.sub(r"port \d+", "port XXX", log)

# Découpage
re.split(r"\s+", "a  b   c")   # ['a', 'b', 'c']
```

### Toujours des raw strings `r"..."`

Sans `r`, `\d` devient un problème d'échappement. **Réflexe : regex = `r"..."`.**

### Méta-caractères essentiels

| Motif | Sens |
|---|---|
| `\d` | chiffre |
| `\w` | lettre/chiffre/`_` |
| `\s` | espace |
| `.` | n'importe quel caractère |
| `*` / `+` / `?` | 0+, 1+, 0-1 répétitions |
| `{2,4}` | entre 2 et 4 répétitions |
| `^` / `$` | début / fin de chaîne |
| `(...)` | groupe capturant |
| `[...]` | classe : `[0-9a-f]` |
| `\|` | alternative |

### Exemple sysadmin : parser des logs SSH

```python
import re

MOTIF = re.compile(
    r"(?P<date>\S+ \S+) \S+ sshd\[\d+\]: Failed password for (invalid user )?(?P<user>\S+) from (?P<ip>\S+)"
)

with open("/var/log/auth.log", encoding="utf-8", errors="replace") as f:
    for ligne in f:
        m = MOTIF.search(ligne)
        if m:
            print(m.group("date"), m.group("user"), m.group("ip"))
```

> `re.compile()` : pré-compile le motif quand on l'utilise en boucle (performance + lisibilité). Groupes nommés `(?P<nom>...)` = code auto-documenté.

### Pièges regex

- `.*` est **glouton** : `"<a>1</a><a>2</a>"` avec `<.*>` capture tout. Utilisez `<.*?>` (non-glouton).
- Valider un email "parfaitement" en regex est un mythe ; contentez-vous d'un motif raisonnable.

---

## 38. JSON, YAML, CSV

### JSON (bibliothèque standard)

```python
import json

# Python → JSON
config = {"host": "srv-01", "ports": [22, 80]}
texte = json.dumps(config, indent=2, ensure_ascii=False)
Path("config.json").write_text(texte, encoding="utf-8")

# JSON → Python
config = json.loads(Path("config.json").read_text(encoding="utf-8"))
```

### YAML (paquet externe `pyyaml`)

```bash
pip install pyyaml
```

```python
import yaml

# Lecture sûre : TOUJOURS safe_load (pas load !)
with open("inventaire.yaml", encoding="utf-8") as f:
    data = yaml.safe_load(f)

texte = yaml.safe_dump(data, allow_unicode=True, sort_keys=False)
```

> `yaml.load` sans loader = exécution de code arbitraire possible. `safe_load` systématique.

### CSV (bibliothèque standard)

```python
import csv

# Lecture
with open("parc.csv", newline="", encoding="utf-8") as f:
    for ligne in csv.DictReader(f):     # dict par ligne (via l'en-tête)
        print(ligne["hostname"], ligne["ip"])

# Écriture
with open("export.csv", "w", newline="", encoding="utf-8") as f:
    w = csv.DictWriter(f, fieldnames=["hostname", "ip"])
    w.writeheader()
    w.writerow({"hostname": "srv-01", "ip": "10.0.0.11"})
```

> `newline=""` à l'ouverture : évite les lignes vides parasites sous Windows.

---

## 39. datetime : dates et heures

```python
from datetime import datetime, date, time, timedelta, timezone

maintenant = datetime.now()                    # heure locale (naïve)
utc = datetime.now(timezone.utc)               # heure UTC (aware) ← préférez
aujourdhui = date.today()                      # 2026-09-26

datetime(2026, 9, 26, 10, 30)                  # construction
```

### Parsing et formatage

```python
# Chaîne → datetime
dt = datetime.strptime("2026-09-26 10:30:00", "%Y-%m-%d %H:%M:%S")

# datetime → chaîne
dt.strftime("%d/%m/%Y à %Hh%M")     # '26/09/2026 à 10h30'
dt.isoformat()                       # '2026-09-26T10:30:00'

# Python 3.11+ : parse l'ISO directement
datetime.fromisoformat("2026-09-26T10:30:00+00:00")
```

### Codes de format courants

| Code | Sens | Exemple |
|---|---|---|
| `%Y` | année 4 chiffres | 2026 |
| `%m` | mois | 09 |
| `%d` | jour | 26 |
| `%H` | heure (24h) | 10 |
| `%M` | minute | 30 |
| `%S` | seconde | 00 |

### Calculs avec timedelta

```python
expiration = datetime.now(timezone.utc) + timedelta(days=90)
il_y_a_1h = datetime.now(timezone.utc) - timedelta(hours=1)

duree = expiration - il_y_a_1h
print(duree.days, duree.seconds)

# Timestamp Unix
ts = datetime.now(timezone.utc).timestamp()     # float
dt = datetime.fromtimestamp(ts, tz=timezone.utc)
```

### Exemple : rotation de logs par date

```python
from datetime import date
nom = f"sauvegarde-{date.today().isoformat()}.tar.gz"  # sauvegarde-2026-09-26.tar.gz
```

> **Règle d'or :** stockez/manipulez en UTC (`timezone.utc`), convertissez en heure locale uniquement pour l'affichage.

---

## 40. argparse : scripts en ligne de commande

```python
#!/usr/bin/env python3
"""Ping une liste d'hôtes."""
import argparse

def main():
    p = argparse.ArgumentParser(
        description="Ping des hôtes et affiche un résumé.",
        epilog="Exemple : %(prog)s -t 2 srv-01 srv-02",
    )
    p.add_argument("hosts", nargs="+", help="hôtes à pinger")
    p.add_argument("-t", "--timeout", type=float, default=2.0,
                   help="timeout en secondes (défaut : %(default)s)")
    p.add_argument("-v", "--verbose", action="store_true",
                   help="mode verbeux")
    p.add_argument("--fichier", "-f", type=argparse.FileType("r"),
                   help="fichier contenant des hôtes (un par ligne)")
    args = p.parse_args()

    if args.verbose:
        print(f"Timeout : {args.timeout}s")
    for h in args.hosts:
        print(f"Ping {h}...")

if __name__ == "__main__":
    main()
```

```bash
python3 ping.py srv-01 srv-02 -t 1 -v
python3 ping.py --help     # aide générée automatiquement
```

### Types d'arguments

| Paramètre | Effet |
|---|---|
| `nargs="+"` | 1 ou + valeurs (liste) |
| `nargs="*"` | 0 ou + valeurs |
| `action="store_true"` | flag booléen |
| `type=int` | conversion auto (+ erreur claire) |
| `choices=["a","b"]` | valeurs autorisées |
| `required=True` | option obligatoire |
| `default=...` | valeur par défaut |

---

## 41. logging : journaliser proprement

Oubliez `print()` pour les scripts sérieux : `logging` gère niveaux, fichiers, formats.

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] %(message)s",
    handlers=[
        logging.FileHandler("script.log", encoding="utf-8"),
        logging.StreamHandler(),          # console aussi
    ],
)

logging.debug("détail technique")
logging.info("Sauvegarde démarrée")
logging.warning("Espace disque < 20%")
logging.error("Échec de connexion à srv-01")
logging.critical("Base de données inaccessible")
```

### Niveaux

| Niveau | Valeur | Usage |
|---|---|---|
| DEBUG | 10 | détails de développement |
| INFO | 20 | déroulement normal |
| WARNING | 30 | anomalie non bloquante |
| ERROR | 40 | échec d'une opération |
| CRITICAL | 50 | panne grave |

### Logger nommé (recommandé dans les modules)

```python
logger = logging.getLogger(__name__)

def traiter():
    logger.info("Traitement de %s", nom)   # lazy % : pas de formatage si niveau filtré
```

### Rotation automatique des logs

```python
from logging.handlers import RotatingFileHandler

handler = RotatingFileHandler(
    "app.log", maxBytes=5_000_000, backupCount=5, encoding="utf-8"
)  # app.log, app.log.1 ... app.log.5
```

> **Anti-piège :** ne jamais logger de secrets (mots de passe, tokens). Si une URL contient un token, masquez-le avant de logger.

---

## 42. requests : HTTP(S) simplifié

```bash
pip install requests
```

```python
import requests

# GET simple
r = requests.get("https://api.exemple.com/equipements", timeout=10)
r.status_code          # 200
r.json()               # dict/list si la réponse est du JSON
r.text                 # corps en texte

# Avec paramètres et en-têtes
r = requests.get(
    "https://api.exemple.com/equipements",
    params={"site": "paris"},
    headers={"Authorization": "Bearer VOTRE_TOKEN_ICI"},
    timeout=10,
)

# POST JSON
r = requests.post(
    "https://api.exemple.com/equipements",
    json={"hostname": "srv-99", "ip": "10.0.0.99"},
    timeout=10,
)
r.raise_for_status()   # lève une exception si code 4xx/5xx
```

### Toujours `timeout=` et `raise_for_status()`

Sans timeout, une API qui ne répond pas bloque votre script **indéfiniment**.

### Sessions : connexions réutilisées + auth par défaut

```python
s = requests.Session()
s.headers.update({"Authorization": "Bearer VOTRE_TOKEN_ICI"})
s.get("https://api.exemple.com/a", timeout=10)
s.get("https://api.exemple.com/b", timeout=10)   # connexion réutilisée
```

### Gestion d'erreurs réseau

```python
try:
    r = requests.get(url, timeout=10)
    r.raise_for_status()
except requests.exceptions.Timeout:
    print("Délai dépassé")
except requests.exceptions.ConnectionError:
    print("Connexion impossible")
except requests.exceptions.HTTPError as e:
    print(f"Erreur HTTP : {e}")
```

### Télécharger un fichier

```python
with requests.get(url, stream=True, timeout=30) as r:
    r.raise_for_status()
    with open("fichier.iso", "wb") as f:
        for chunk in r.iter_content(chunk_size=8192):
            f.write(chunk)
```

---

## 43. Réseau : sockets, ping, inventaire

### Tester un port TCP (sans dépendance externe)

```python
import socket

def port_ouvert(host, port, timeout=2.0):
    """True si le port TCP accepte une connexion."""
    try:
        with socket.create_connection((host, port), timeout=timeout):
            return True
    except OSError:
        return False

port_ouvert("10.0.0.11", 22)    # True/False
port_ouvert("10.0.0.11", 3389)
```

### Résolution DNS

```python
import socket
socket.gethostbyname("srv-web-01")            # '10.0.0.11'
socket.gethostbyaddr("10.0.0.11")             # (nom, alias, ips)
socket.getaddrinfo("srv-01", 22)              # IPv4 + IPv6
```

### Petit serveur TCP (exemple pédagogique)

```python
import socket

with socket.socket() as srv:
    srv.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    srv.bind(("127.0.0.1", 9999))
    srv.listen(1)
    print("En écoute sur 9999...")
    conn, addr = srv.accept()
    with conn:
        print("Client :", addr)
        conn.sendall(b"OK\n")
```

### Inventaire réseau : balayer un /24 sur le port 22

```python
import ipaddress
from concurrent.futures import ThreadPoolExecutor
import socket

def ssh_ouvert(ip, timeout=1.0):
    try:
        with socket.create_connection((str(ip), 22), timeout=timeout):
            return str(ip)
    except OSError:
        return None

reseau = ipaddress.ip_network("10.0.0.0/24")
with ThreadPoolExecutor(max_workers=50) as pool:
    trouves = [ip for ip in pool.map(ssh_ouvert, reseau.hosts()) if ip]

print(f"{len(trouves)} hôtes avec SSH ouvert :")
print("\n".join(trouves))
```

> `ipaddress` (stdlib) : manipule IPv4/IPv6 proprement — `ip_network`, `ip_address`, tests d'appartenance (`ip in reseau`).

---

## 44. sqlite3 : base de données embarquée

Zéro serveur, un simple fichier. Parfait pour un inventaire local.

```python
import sqlite3

conn = sqlite3.connect("parc.db")
conn.row_factory = sqlite3.Row      # accès par nom de colonne
cur = conn.cursor()

cur.execute("""
    CREATE TABLE IF NOT EXISTS equipements (
        id INTEGER PRIMARY KEY,
        hostname TEXT UNIQUE NOT NULL,
        ip TEXT NOT NULL,
        type TEXT,
        site TEXT
    )
""")

# Insertion SÉCURISÉE (paramètres ? : anti-injection SQL)
cur.execute(
    "INSERT OR IGNORE INTO equipements (hostname, ip, type, site) VALUES (?, ?, ?, ?)",
    ("srv-web-01", "10.0.0.11", "serveur", "paris"),
)
conn.commit()

# Lecture
for row in cur.execute("SELECT hostname, ip FROM equipements WHERE site = ?", ("paris",)):
    print(row["hostname"], row["ip"])

conn.close()
```

### Avec context manager (commit/rollback auto)

```python
with sqlite3.connect("parc.db") as conn:
    conn.execute("INSERT INTO equipements ... ", (...))
    # commit automatique à la sortie ; rollback si exception
```

### Bonnes pratiques sqlite3

- **Toujours** des requêtes paramétrées (`?`), jamais de f-string dans le SQL.
- `conn.commit()` après les écritures (ou context manager).
- Un seul writer à la fois : SQLite verrouille le fichier en écriture.
- Sauvegarde = copier le fichier `.db` (ou `VACUUM INTO 'backup.db'`).

---

## 45. Tests unitaires : unittest et pytest

### Pourquoi tester ?

Un script sysadmin qui tourne à 3h du matin doit être fiable. Les tests vérifient que vos fonctions font ce qu'elles annoncent, **avant** la production.

### pytest (recommandé)

```bash
pip install pytest
```

```python
# test_reseau.py
from reseau import est_ip_privee, normaliser_mac

def test_ip_privee():
    assert est_ip_privee("10.0.0.1") is True
    assert est_ip_privee("8.8.8.8") is False

def test_mac():
    assert normaliser_mac("AA:BB:CC:DD:EE:FF") == "aa:bb:cc:dd:ee:ff"
```

```bash
pytest -v              # exécute les tests
pytest -x              # s'arrête à la 1ère erreur
pytest -k "mac"        # filtre par nom
```

### Fixtures et paramétrisation

```python
import pytest

@pytest.mark.parametrize("ip,attendu", [
    ("10.0.0.1", True),
    ("172.16.5.4", True),
    ("192.168.1.1", True),
    ("8.8.8.8", False),
])
def test_ip_privee_param(ip, attendu):
    assert est_ip_privee(ip) is attendu

@pytest.fixture
def base_test(tmp_path):
    db = tmp_path / "test.db"
    initialiser_base(str(db))
    return str(db)

def test_insertion(base_test):
    inserer_equipement(base_test, "srv-test", "10.9.9.9")
    assert compter(base_test) == 1
```

### unittest (stdlib, sans dépendance)

```python
import unittest

class TestReseau(unittest.TestCase):
    def test_ip(self):
        self.assertTrue(est_ip_privee("10.0.0.1"))
        self.assertEqual(normaliser_mac("AA"), "aa")

if __name__ == "__main__":
    unittest.main()
```

> Convention : fichiers `test_*.py` ou `*_test.py`, fonctions `test_*`. Visez 80 % de couverture sur le code critique (`pytest --cov` avec `pytest-cov`).

---

## 46. Débogage : pdb et techniques

### print() ciblé (le classique)

```python
print(f"DEBUG: host={host!r} port={port}")   # !r = repr, voit les espaces
```

### pdb : le débogueur intégré

```python
# Point d'arrêt dans le code :
breakpoint()   # Python 3.7+ : équivaut à import pdb; pdb.set_trace()
```

```bash
python3 -m pdb script.py     # lance sous pdb
```

| Commande pdb | Effet |
|---|---|
| `n` (next) | ligne suivante (sans entrer dans les fonctions) |
| `s` (step) | entre dans la fonction |
| `c` (continue) | reprend jusqu'au prochain breakpoint |
| `p var` | affiche une variable |
| `l` | montre le code autour |
| `b 42` | breakpoint ligne 42 |
| `q` | quitte |

### Techniques rapides

```python
import traceback

try:
    operation_risquee()
except Exception:
    traceback.print_exc()      # affiche la pile complète dans les logs
    # ou : logger.exception("Échec")  ← avec logging
```

- `python3 -i script.py` : console interactive après exécution (inspectez les variables).
- Lisez la traceback **de bas en haut** : la dernière ligne = l'erreur réelle.

---

## 47. Asyncio : programmation asynchrone

`asyncio` = faire des milliers d'opérations d'attente (réseau) **en parallèle** dans un seul thread. Idéal pour interroger 500 équipements.

```python
import asyncio

async def interroger(host):
    await asyncio.sleep(0.5)          # simule une attente réseau
    return f"{host}: OK"

async def main():
    hosts = [f"srv-{i:02d}" for i in range(10)]
    resultats = await asyncio.gather(*(interroger(h) for h in hosts))
    print(resultats)

asyncio.run(main())   # point d'entrée
```

### Concepts clés

| Notion | Sens |
|---|---|
| `async def` | définit une coroutine |
| `await` | "attends ici, fais autre chose en attendant" |
| `asyncio.gather(...)` | lance plusieurs coroutines en parallèle |
| `asyncio.run(...)` | exécute le programme async |

### Exemple réaliste : requêtes HTTP parallèles (avec aiohttp)

```bash
pip install aiohttp
```

```python
import asyncio, aiohttp

async def fetch(session, url):
    async with session.get(url, timeout=10) as r:
        return url, r.status

async def main(urls):
    async with aiohttp.ClientSession() as session:
        resultats = await asyncio.gather(*(fetch(session, u) for u in urls))
    return resultats

urls = [f"https://srv-{i:02d}.lan/health" for i in range(50)]
print(asyncio.run(main(urls)))
```

### Limiter la concurrence (ne pas DoS ses propres serveurs !)

```python
sem = asyncio.Semaphore(20)   # max 20 requêtes simultanées

async def fetch_limite(session, url):
    async with sem:
        return await fetch(session, url)
```

> `asyncio` ≠ threads : un seul thread, pas de parallélisme CPU. Pour du calcul lourd, voir multiprocessing (section 48).

---

## 48. Threading et multiprocessing

### Choisir le bon outil

| Situation | Outil |
|---|---|
| Beaucoup d'attentes réseau/disque (I/O bound) | `threading` ou `asyncio` |
| Calculs lourds (CPU bound) | `multiprocessing` |
| Code simple, parallélisme modéré | `concurrent.futures` |

### concurrent.futures : l'API simple (recommandée)

```python
from concurrent.futures import ThreadPoolExecutor, as_completed
import subprocess

def ping(host):
    r = subprocess.run(["ping", "-c", "1", "-W", "1", host],
                       capture_output=True)
    return host, r.returncode == 0

hosts = [f"10.0.0.{i}" for i in range(1, 51)]

with ThreadPoolExecutor(max_workers=20) as pool:
    futurs = {pool.submit(ping, h): h for h in hosts}
    for fut in as_completed(futurs):
        host, ok = fut.result()
        print(f"{host}: {'OK' if ok else 'KO'}")
```

### multiprocessing : contourner le GIL

```python
from concurrent.futures import ProcessPoolExecutor

def calcul_lourd(n):
    return sum(i * i for i in range(n))

if __name__ == "__main__":      # OBLIGATOIRE sous Windows/macOS
    with ProcessPoolExecutor() as pool:
        print(list(pool.map(calcul_lourd, [10**7, 10**7, 10**7, 10**7])))
```

> **Le GIL** (Global Interpreter Lock) : un seul thread Python exécute du bytecode à la fois. Les threads n'accélèrent donc **pas** le calcul pur — mais ils sont parfaits pour les I/O.

### threading bas niveau (verrous)

```python
import threading

verrou = threading.Lock()
compteur = 0

def incrementer():
    global compteur
    with verrou:          # protège la section critique
        compteur += 1
```

---

## 49. Performance : mesurer et optimiser

### Mesurer d'abord, optimiser ensuite

```python
import time

debut = time.perf_counter()   # horloge précise (pas time.time !)
traitement()
print(f"{time.perf_counter() - debut:.3f} s")
```

```bash
# timeit : micro-benchmark fiable
python3 -m timeit "'-'.join(str(i) for i in range(1000))"
python3 -m timeit --setup "import mon_module" "mon_module.f()"
```

### cProfile : où passe le temps ?

```bash
python3 -m cProfile -s cumulative script.py | head -30
```

```python
import cProfile, pstats
cProfile.run("traitement()", "stats.prof")
p = pstats.Stats("stats.prof")
p.sort_stats("cumulative").print_stats(15)
```

### Règles d'optimisation (par ordre d'impact)

1. **Algorithme** : une recherche en `set`/`dict` (O(1)) bat une liste (O(n)).
2. **I/O** : parallélisez (threads/asyncio) — c'est là que dorment 90 % des scripts lents.
3. **Éviter le travail inutile** : cache (`lru_cache`), streaming (générateurs).
4. **Micro-optimisations** en dernier : f-strings > `%` > `+`, variables locales en boucle chaude.

```python
# Lent : 'in' sur une liste (O(n) par test)
autorises = ["a", "b", ...]      # 10 000 éléments
if x in autorises: ...

# Rapide : set (O(1))
autorises = {"a", "b", ...}
if x in autorises: ...
```

---

## 50. Packaging : pyproject.toml

Pour distribuer un outil (ou juste l'installer proprement), décrivez le projet.

```
mon_outil/
├── pyproject.toml
├── README.md
└── src/
    └── mon_outil/
        ├── __init__.py
        └── cli.py
```

```toml
[build-system]
requires = ["setuptools>=61"]
build-backend = "setuptools.build_meta"

[project]
name = "mon-outil"
version = "1.0.0"
description = "Inventaire réseau pour sysadmins"
requires-python = ">=3.10"
dependencies = ["requests>=2.28", "pyyaml"]
authors = [{name = "Zelef"}]

[project.scripts]
inventaire = "mon_outil.cli:main"   # crée la commande 'inventaire'

[project.optional-dependencies]
dev = ["pytest", "ruff"]
```

```bash
pip install -e .          # installe en mode éditable (dev)
pip install ".[dev]"      # avec les extras dev
python3 -m build         # construit sdist + wheel (pip install build)
```

> Après `pip install -e .`, la commande `inventaire` est disponible dans le venv. C'est la façon propre de "publier" un script interne.

---

## 51. Intro à FastAPI

FastAPI = framework web moderne pour créer des **API REST** rapidement, avec validation automatique.

```bash
pip install fastapi uvicorn
```

```python
# api.py
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel

app = FastAPI(title="API Parc informatique")

class Equipement(BaseModel):
    hostname: str
    ip: str
    type: str = "serveur"

PARC: dict[str, Equipement] = {}

@app.get("/equipements")
def lister():
    return list(PARC.values())

@app.post("/equipements", status_code=201)
def ajouter(eq: Equipement):
    PARC[eq.hostname] = eq
    return eq

@app.get("/equipements/{hostname}")
def detail(hostname: str):
    if hostname not in PARC:
        raise HTTPException(status_code=404, detail="Introuvable")
    return PARC[hostname]
```

```bash
uvicorn api:app --reload      # serveur de dev
# Docs auto : http://127.0.0.1:8000/docs
```

### Pourquoi c'est intéressant pour un sysadmin

- Exposer l'inventaire/la supervision via une API propre en 30 lignes.
- Validation Pydantic : les données invalides sont rejetées avec un message clair.
- Documentation OpenAPI générée **automatiquement** (`/docs`).

---

## 52. Bonnes pratiques & PEP 8

### PEP 8 : le style officiel (résumé)

| Règle | Exemple |
|---|---|
| 4 espaces d'indentation | pas de tabulations |
| Lignes ≤ 79-88 caractères | (88 = défaut de `black`) |
| `snake_case` fonctions/variables | `def ping_host():` |
| `PascalCase` classes | `class Serveur:` |
| `MAJUSCULES` constantes | `TIMEOUT = 30` |
| 2 lignes vides entre fonctions top-level | |
| Espaces autour des opérateurs | `x = 1`, pas `x=1` |
| Imports en haut, groupés | stdlib → tiers → locaux |

### Outils automatiques

```bash
pip install ruff black
ruff check .        # lint : erreurs + style
ruff check --fix .  # corrige automatiquement
black .             # reformate le code
```

> `ruff` remplace flake8/pylint en 10x plus rapide. `black` tranche les débats de formatage : on ne discute plus, on lance `black`.

### Principes généraux

1. **Lisibilité > astuce.** Code relu à 3h du matin pendant une astreinte.
2. **Une fonction = une chose.** Si le nom contient "et", découpez.
3. **Noms explicites** : `delai_reconnexion_s` > `d`.
4. **Pas de nombres magiques** : `TIMEOUT_SSH = 10` en constante nommée.
5. **Échouez vite, échouez fort** : validez les entrées en début de fonction.
6. **Ne répétez pas** (DRY) : 3e copier-coller = fonction.
7. **Docstrings** sur tout ce qui est réutilisé.
8. **Gestion d'erreurs explicite** : pas de `except: pass` silencieux.

---

## 53. Sécurité : écrire du code sûr

### 1. Injection de commandes (subprocess)

```python
# DANGEREUX — si host = "x; rm -rf /" :
subprocess.run(f"ping -c1 {host}", shell=True)

# SÛR — liste d'arguments, pas de shell :
subprocess.run(["ping", "-c1", host])
```

### 2. Injection SQL

```python
# DANGEREUX :
cur.execute(f"SELECT * FROM eq WHERE hostname = '{nom}'")

# SÛR — requête paramétrée :
cur.execute("SELECT * FROM eq WHERE hostname = ?", (nom,))
```

### 3. Secrets : jamais en dur dans le code

```python
import os

# Bien : variable d'environnement
token = os.environ.get("API_TOKEN")
if not token:
    raise SystemExit("API_TOKEN non défini")

# Bien : fichier protégé (chmod 600)
token = Path.home().joinpath(".secrets/api_token").read_text().strip()

# MAL : token = "ghp_xxxx" dans le script (finira sur git un jour)
```

> Utilisez un gestionnaire de secrets (Vault, gestionnaire d'équipe) en production. Au minimum : variables d'environnement + `.gitignore`.

### 4. YAML, pickle, eval : l'exécution de code cachée

| À éviter | Alternative sûre |
|---|---|
| `yaml.load(f)` | `yaml.safe_load(f)` |
| `pickle.loads(donnees_inconnues)` | `json` |
| `eval(input_utilisateur)` | `ast.literal_eval()` (littéraux uniquement) |

```python
import ast
ast.literal_eval("{'a': 1}")   # OK : dict
ast.literal_eval("__import__('os')")  # ValueError : refusé
```

### 5. Chemins : path traversal

```python
from pathlib import Path

BASE = Path("/srv/rapports")

def lire_rapport(nom):
    chemin = (BASE / nom).resolve()
    if not chemin.is_relative_to(BASE):   # Python 3.9+
        raise ValueError("Chemin interdit")
    return chemin.read_text()
```

### 6. TLS : ne pas désactiver la vérification

```python
requests.get(url, verify=False)   # À ÉVITER (sauf test local conscient)
```

### Checklist sécurité

- [ ] Aucun secret en dur (ni dans le code, ni dans git)
- [ ] Commandes shell en liste, pas de `shell=True` avec données variables
- [ ] SQL paramétré partout
- [ ] `yaml.safe_load`, pas de `pickle` sur données externes
- [ ] Validation des chemins de fichiers
- [ ] TLS vérifié (`verify=True` par défaut)

---

## 54. Scripts sysadmin n°1 : inventaire réseau

```python
#!/usr/bin/env python3
"""inventaire.py — balaye un réseau, teste SSH/HTTP et exporte en CSV.

Usage : python3 inventaire.py 10.0.0.0/24 -o inventaire.csv
"""
import argparse
import csv
import ipaddress
import socket
from concurrent.futures import ThreadPoolExecutor

PORTS = {"ssh": 22, "http": 80, "https": 443}

def sonder(ip):
    """Retourne un dict avec l'état des ports pour une IP."""
    resultat = {"ip": str(ip)}
    for nom, port in PORTS.items():
        try:
            with socket.create_connection((str(ip), port), timeout=1.0):
                resultat[nom] = "ouvert"
        except OSError:
            resultat[nom] = "fermé"
    try:
        resultat["hostname"] = socket.gethostbyaddr(str(ip))[0]
    except OSError:
        resultat["hostname"] = ""
    return resultat

def main():
    p = argparse.ArgumentParser(description="Inventaire réseau rapide")
    p.add_argument("reseau", help="ex : 10.0.0.0/24")
    p.add_argument("-o", "--output", default="inventaire.csv")
    p.add_argument("-w", "--workers", type=int, default=50)
    args = p.parse_args()

    reseau = ipaddress.ip_network(args.reseau, strict=False)
    hotes = list(reseau.hosts())
    print(f"Balayage de {len(hotes)} hôtes...")

    with ThreadPoolExecutor(max_workers=args.workers) as pool:
        resultats = list(pool.map(sonder, hotes))

    actifs = [r for r in resultats if r["ssh"] == "ouvert"]
    with open(args.output, "w", newline="", encoding="utf-8") as f:
        w = csv.DictWriter(f, fieldnames=["ip", "hostname", "ssh", "http", "https"])
        w.writeheader()
        w.writerows(resultats)

    print(f"{len(actifs)} hôtes avec SSH ouvert → {args.output}")

if __name__ == "__main__":
    main()
```

---

## 55. Scripts sysadmin n°2 : parsing de logs

```python
#!/usr/bin/env python3
"""top_ips.py — top 10 des IPs en échec d'authentification SSH.

Usage : python3 top_ips.py /var/log/auth.log [-n 20] [--seuil 5]
"""
import argparse
import re
from collections import Counter

MOTIF = re.compile(r"Failed password .* from (?P<ip>\S+) port")

def analyser(chemin):
    compteur = Counter()
    with open(chemin, encoding="utf-8", errors="replace") as f:
        for ligne in f:
            m = MOTIF.search(ligne)
            if m:
                compteur[m.group("ip")] += 1
    return compteur

def main():
    p = argparse.ArgumentParser()
    p.add_argument("fichier")
    p.add_argument("-n", type=int, default=10, help="nombre d'IPs à afficher")
    p.add_argument("--seuil", type=int, default=5,
                   help="seuil d'alerte (tentatives)")
    args = p.parse_args()

    compteur = analyser(args.fichier)
    print(f"{'IP':<16} {'Tentatives':>10}  Alerte")
    print("-" * 40)
    for ip, nb in compteur.most_common(args.n):
        alerte = " !!!" if nb >= args.seuil else ""
        print(f"{ip:<16} {nb:>10}{alerte}")

    total = sum(compteur.values())
    print(f"\nTotal : {total} échecs, {len(compteur)} IP(s) distinctes")

if __name__ == "__main__":
    main()
```

---

## 56. Scripts sysadmin n°3 : sauvegarde et rotation

```python
#!/usr/bin/env python3
"""backup.py — archive un dossier en tar.gz daté + rotation (garde N dernières).

Usage : python3 backup.py /srv/data /srv/backups --keep 7
"""
import argparse
import logging
import shutil
import tarfile
from datetime import date
from pathlib import Path

logging.basicConfig(level=logging.INFO, format="%(asctime)s %(message)s")
log = logging.getLogger(__name__)

def archiver(source: Path, dest_dir: Path) -> Path:
    nom = f"{source.name}-{date.today().isoformat()}.tar.gz"
    cible = dest_dir / nom
    log.info("Archivage de %s → %s", source, cible)
    with tarfile.open(cible, "w:gz") as tar:
        tar.add(source, arcname=source.name)
    taille_mo = cible.stat().st_size / 1024**2
    log.info("Archive créée : %.1f Mo", taille_mo)
    return cible

def rotation(dest_dir: Path, prefixe: str, garder: int):
    archives = sorted(dest_dir.glob(f"{prefixe}-*.tar.gz"))
    for vieille in archives[:-garder] if len(archives) > garder else []:
        log.info("Suppression ancienne archive : %s", vieille.name)
        vieille.unlink()

def main():
    p = argparse.ArgumentParser(description="Sauvegarde avec rotation")
    p.add_argument("source", type=Path)
    p.add_argument("destination", type=Path)
    p.add_argument("--keep", type=int, default=7)
    args = p.parse_args()

    if not args.source.is_dir():
        raise SystemExit(f"Source introuvable : {args.source}")
    args.destination.mkdir(parents=True, exist_ok=True)

    archiver(args.source, args.destination)
    rotation(args.destination, args.source.name, args.keep)

    libre_go = shutil.disk_usage(args.destination).free / 1024**3
    log.info("Espace libre sur la destination : %.1f Go", libre_go)

if __name__ == "__main__":
    main()
```

---

## 57. Scripts sysadmin n°4 : surveillance disque et alertes

```python
#!/usr/bin/env python3
"""disk_watch.py — alerte si un point de montage dépasse un seuil.

Usage : API_TOKEN=xxx python3 disk_watch.py --seuil 85 --webhook https://...
"""
import argparse
import logging
import os
import shutil
import sys
import requests

logging.basicConfig(level=logging.INFO, format="%(asctime)s %(levelname)s %(message)s")
log = logging.getLogger(__name__)

def usage_points(points):
    resultats = []
    for pt in points:
        u = shutil.disk_usage(pt)
        pct = u.used / u.total * 100
        resultats.append((pt, pct, u.free / 1024**3))
    return resultats

def alerter(webhook, message):
    try:
        requests.post(webhook, json={"text": message}, timeout=10).raise_for_status()
    except requests.RequestException as e:
        log.error("Échec envoi alerte : %s", e)

def main():
    p = argparse.ArgumentParser()
    p.add_argument("--seuil", type=float, default=85.0)
    p.add_argument("--webhook", default=os.environ.get("ALERT_WEBHOOK"))
    p.add_argument("points", nargs="*", default=["/"])
    args = p.parse_args()

    probleme = False
    for pt, pct, libre_go in usage_points(args.points):
        etat = "ALERTE" if pct >= args.seuil else "ok"
        log.info("%s : %.1f%% utilisé (%.1f Go libres) [%s]", pt, pct, libre_go, etat)
        if pct >= args.seuil:
            probleme = True
            if args.webhook:
                alerter(args.webhook,
                        f"⚠ Disque {pt} à {pct:.1f}% ({libre_go:.1f} Go libres)")

    sys.exit(1 if probleme else 0)   # code retour exploitable par cron/nagios

if __name__ == "__main__":
    main()
```

> Astuce : `sys.exit(1)` permet à cron, Nagios, Zabbix ou systemd de détecter l'alerte via le code de retour.

---

## 58. Scripts sysadmin n°5 : requêtes API automatisées

```python
#!/usr/bin/env python3
"""glpi_tickets.py — récupère les tickets ouverts via l'API (exemple générique).

Usage : API_URL=https://glpi.lan/apirest.php API_TOKEN=xxx python3 glpi_tickets.py
"""
import os
import sys
import requests

API_URL = os.environ.get("API_URL")
TOKEN = os.environ.get("API_TOKEN")

def session_api():
    if not API_URL or not TOKEN:
        raise SystemExit("API_URL et API_TOKEN requis en variables d'environnement")
    s = requests.Session()
    s.headers.update({"Authorization": f"Bearer {TOKEN}",
                      "Content-Type": "application/json"})
    return s

def tickets_ouverts(session):
    r = session.get(f"{API_URL}/tickets", params={"status": "open"}, timeout=15)
    r.raise_for_status()
    return r.json()

def main():
    s = session_api()
    try:
        tickets = tickets_ouverts(s)
    except requests.RequestException as e:
        raise SystemExit(f"Erreur API : {e}")
    print(f"{len(tickets)} ticket(s) ouvert(s)")
    for t in sorted(tickets, key=lambda x: x.get("priority", 0), reverse=True):
        print(f"  #{t['id']} [P{t.get('priority')}] {t.get('title')}")

if __name__ == "__main__":
    main()
```

---

## 59. Scripts sysadmin n°6 : traitement CSV/Excel

```python
#!/usr/bin/env python3
"""fusion_parc.py — fusionne deux exports CSV d'inventaire (déduplique par hostname).

Usage : python3 fusion_parc.py export1.csv export2.csv -o parc_fusionne.csv
"""
import argparse
import csv

CHAMPS = ["hostname", "ip", "type", "site", "source"]

def lire(chemin, source):
    with open(chemin, newline="", encoding="utf-8-sig") as f:  # utf-8-sig : BOM Excel
        for ligne in csv.DictReader(f):
            ligne["source"] = source
            yield ligne

def main():
    p = argparse.ArgumentParser()
    p.add_argument("fichiers", nargs=2)
    p.add_argument("-o", "--output", required=True)
    args = p.parse_args()

    parc = {}
    for chemin in args.fichiers:
        for ligne in lire(chemin, source=chemin):
            cle = ligne["hostname"].strip().lower()
            parc.setdefault(cle, ligne)   # le 1er fichier gagne en cas de doublon

    with open(args.output, "w", newline="", encoding="utf-8") as f:
        w = csv.DictWriter(f, fieldnames=CHAMPS, extrasaction="ignore")
        w.writeheader()
        w.writerows(parc.values())

    print(f"{len(parc)} équipements uniques → {args.output}")

if __name__ == "__main__":
    main()
```

> Pour lire/écrire du vrai Excel (`.xlsx`) : `pip install openpyxl` puis `openpyxl.load_workbook(...)`.

---

## 60. Scripts sysadmin n°7 : scan de ports et services

```python
#!/usr/bin/env python3
"""scan.py — scan rapide des ports courants d'un hôte + bannière.

Usage : python3 scan.py srv-01 [--ports 22,80,443] [--timeout 1.5]
"""
import argparse
import socket

PORTS_COURANTS = [21, 22, 23, 25, 53, 80, 110, 143, 443, 445,
                  3306, 3389, 5432, 5900, 6379, 8080, 8443]

def parse_ports(texte):
    return [int(p) for p in texte.split(",")]

def scanner_port(host, port, timeout):
    try:
        with socket.create_connection((host, port), timeout=timeout) as s:
            s.settimeout(1.0)
            try:
                banniere = s.recv(1024).decode("utf-8", errors="replace").strip()
            except OSError:
                banniere = ""
            return True, banniere.split("\n")[0][:60]
    except OSError:
        return False, ""

def main():
    p = argparse.ArgumentParser(description="Scan de ports simple")
    p.add_argument("host")
    p.add_argument("--ports", default=",".join(map(str, PORTS_COURANTS)),
                   help="liste séparée par des virgules")
    p.add_argument("--timeout", type=float, default=1.0)
    args = p.parse_args()

    print(f"Scan de {args.host}...\n{'PORT':<8}{'ÉTAT':<10}BANNIÈRE")
    for port in parse_ports(args.ports):
        ouvert, banniere = scanner_port(args.host, port, args.timeout)
        etat = "OUVERT" if ouvert else "fermé"
        print(f"{port:<8}{etat:<10}{banniere}")

if __name__ == "__main__":
    main()
```

---

## 61. Erreurs classiques des débutants (20 pièges)

### 1. `=` vs `==`

```python
if x = 5:    # SyntaxError : affectation dans un test
if x == 5:   # comparaison
```

### 2. Oublier les deux-points

```python
if x > 0     # SyntaxError
    print(x)
```

### 3. Mélanger tabulations et espaces → `TabError`

Configurez l'éditeur : "insérer des espaces quand on appuie sur Tab".

### 4. Comparer avec `is` au lieu de `==`

```python
a == b   # égalité de contenu ✓ (sauf None : utilisez 'is None')
```

### 5. Argument mutable par défaut

```python
# BUG : la même liste est partagée entre tous les appels !
def ajouter(x, liste=[]):
    liste.append(x)
    return liste

# CORRECT :
def ajouter(x, liste=None):
    if liste is None:
        liste = []
    liste.append(x)
    return liste
```

### 6. Modifier une liste pendant qu'on l'itère

```python
# BUG : éléments sautés
for srv in serveurs:
    if not ping(srv):
        serveurs.remove(srv)

# CORRECT : itérer sur une copie
for srv in serveurs[:]:
    ...
# ou construire une nouvelle liste :
serveurs = [s for s in serveurs if ping(s)]
```

### 7. `except:` nu qui masque tout

```python
try:
    ...
except Exception as e:      # au minimum, pas de 'except:' nu
    logger.exception("contexte : %s", e)
    raise
```

### 8. Oublier `return` (la fonction retourne `None`)

```python
def double(x):
    x * 2          # calculé puis jeté !
double(21)         # None
```

### 9. `bool("False")` vaut `True`

Voir section 12 : comparez explicitement les chaînes.

### 10. Division entière surprise (Python 2 vs 3)

En Python 3, `7 / 2 = 3.5` (float). Si vous voyez `3`, c'est `//` ou un vieux Python 2.

### 11. Encodage : oublier `encoding="utf-8"`

Accents cassés sur Windows, `UnicodeDecodeError` sur des logs exotiques → `encoding="utf-8", errors="replace"`.

### 12. Chemins Windows en dur

```python
"C:\nouveau\dossier"   # \n = saut de ligne ! BUG
r"C:\nouveau\dossier"  # raw string ✓
Path("C:/nouveau/dossier")  # ou pathlib ✓
```

### 13. `sorted()` ne trie pas en place

```python
serveurs = sorted(serveurs)   # ✓ (nouvelle liste)
serveurs.sort()               # ✓ (en place)
sorted(serveurs)              # ✗ trié puis jeté !
```

### 14. Copie de liste par référence

```python
b = a       # même objet ! → b = a.copy()
```

### 15. Variable de boucle qui fuit

```python
for i in range(5):
    ...
print(i)   # 4 : 'i' existe encore après la boucle (pas de scope de bloc)
```

### 16. `range()` : fin exclue

`range(1, 10)` = 1..9, pas 1..10. Source classique d'erreurs off-by-one.

### 17. Comparer des types incompatibles

```python
"10" > 9    # TypeError en Python 3 (pas de comparaison implicite)
int("10") > 9  # ✓
```

### 18. Import circulaire

`a.py` importe `b.py` qui importe `a.py` → `ImportError`. Solution : restructurer, ou import local dans la fonction.

### 19. Ne pas fermer les ressources (sans `with`)

Fichier, connexion, socket : utilisez toujours un context manager.

### 20. `time.sleep()` dans du code asyncio

```python
async def tache():
    await asyncio.sleep(1)   # ✓ non bloquant (dans une coroutine)
    # time.sleep(1)          # ✗ bloque TOUTE la boucle événementielle
```

---

## 62. Cas pratiques commentés

### Cas n°1 : nettoyer des noms d'hôtes

```python
def normaliser_hostname(brut: str) -> str:
    """' SRV-Web_01 ' → 'srv-web-01'."""
    propre = brut.strip().lower()          # retire espaces, minuscules
    propre = propre.replace("_", "-")     # uniformise le séparateur
    # garde lettres, chiffres, tirets uniquement
    propre = "".join(c for c in propre if c.isalnum() or c == "-")
    return propre

for h in [" SRV-Web_01 ", "db#02", "cache_03"]:
    print(f"{h!r:18} → {normaliser_hostname(h)}")
# ' SRV-Web_01 '     → srv-web-01
# 'db#02'            → db02
# 'cache_03'         → cache-03
```

### Cas n°2 : calculer un taux de disponibilité depuis un log

```python
from datetime import datetime

def disponibilite(chemin, service="nginx"):
    """% de lignes 'started' vs 'failed' pour un service dans un journal."""
    ok = ko = 0
    with open(chemin, encoding="utf-8", errors="replace") as f:
        for ligne in f:
            if service not in ligne:
                continue
            if "started" in ligne:
                ok += 1
            elif "failed" in ligne:
                ko += 1
    total = ok + ko
    return ok / total * 100 if total else 0.0

print(f"Disponibilité nginx : {disponibilite('journal.log'):.2f}%")
```

### Cas n°3 : renommer des fichiers en masse

```python
from pathlib import Path

dossier = Path("/srv/exports")
for f in dossier.glob("rapport_*.csv"):
    # rapport_2026-01-05.csv → 2026-01-05_rapport.csv
    nouveau_nom = f"{f.stem[8:]}_{f.stem[:8]}{f.suffix}"
    f.rename(f.with_name(nouveau_nom))
    print(f"{f.name} → {nouveau_nom}")
```

### Cas n°4 : vérifier l'expiration de certificats (idée d'implémentation)

```python
import socket, ssl
from datetime import datetime, timezone

def expiration_certificat(hostname, port=443):
    """Retourne la date d'expiration du certificat TLS."""
    ctx = ssl.create_default_context()
    with socket.create_connection((hostname, port), timeout=5) as sock:
        with ctx.wrap_socket(sock, server_hostname=hostname) as tls:
            cert = tls.getpeercert()
    fin = datetime.strptime(cert["notAfter"], "%b %d %H:%M:%S %Y %Z")
    return fin.replace(tzinfo=timezone.utc)

exp = expiration_certificat("exemple.com")
jours = (exp - datetime.now(timezone.utc)).days
print(f"Expire dans {jours} jours ({exp.date()})")
```

### Cas n°5 : mini tableau de bord texte

```python
import shutil

def barre(pct, largeur=20):
    remplis = int(pct / 100 * largeur)
    return "█" * remplis + "░" * (largeur - remplis)

for point in ["/", "/var", "/srv"]:
    u = shutil.disk_usage(point)
    pct = u.used / u.total * 100
    print(f"{point:<6} {barre(pct)} {pct:5.1f}%  ({u.free/1024**3:.1f} Go libres)")
```

---

## 63. Checklist "script de production"

Avant de mettre un script en cron ou sur un serveur :

- [ ] Shebang `#!/usr/bin/env python3` + `chmod +x`
- [ ] Bloc `if __name__ == "__main__":` présent
- [ ] `argparse` pour les paramètres (pas de valeurs en dur)
- [ ] `logging` au lieu de `print()` (fichier + niveau configurable)
- [ ] Gestion d'erreurs : `try/except` ciblés, pas de `except: pass`
- [ ] Timeouts sur tout ce qui est réseau (`requests`, `socket`)
- [ ] Codes de retour explicites (`sys.exit(0/1/2)`)
- [ ] Secrets via variables d'environnement, jamais en dur
- [ ] `encoding="utf-8"` sur les ouvertures de fichiers texte
- [ ] Testé avec `python3 -m py_compile` (syntaxe) + un jeu de test réel
- [ ] `requirements.txt` ou `pyproject.toml` à jour
- [ ] Testé depuis un venv propre (pas seulement votre poste)
- [ ] Documenté : docstring + `--help` clair + exemple d'usage
- [ ] Idempotent si possible (relançable sans dégâts)
- [ ] Journalisé : on sait ce qu'il a fait (et quand il a échoué)

---

## 64. Pense-bête de poche

### Lancement & environnement

```bash
python3 script.py arg1 --opt      # exécuter
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
python3 -m py_compile script.py  # vérif syntaxe rapide
python3 -i script.py             # console après exécution
```

### Structures

```python
[x for x in l if cond]           # compréhension filtrante
{x: f(x) for x in l}             # dict en compréhension
a, b = b, a                      # échange
for i, x in enumerate(l): ...    # index + valeur
for a, b in zip(l1, l2): ...     # parcours parallèle
```

### Fichiers & chemins

```python
from pathlib import Path
Path("f.txt").read_text(encoding="utf-8")
Path("f.txt").write_text("...", encoding="utf-8")
list(Path(".").glob("*.log"))
```

### Réseau & système

```python
subprocess.run(["cmd", "arg"], capture_output=True, text=True, check=True)
socket.create_connection((host, port), timeout=2)
requests.get(url, timeout=10).raise_for_status()
```

### Debug express

```text
breakpoint()                     # stoppe ici (pdb)
f"{var=}"                        # f-string debug : affiche nom=valeur (3.8+)
python3 -m pdb script.py
python3 -m cProfile -s cumulative script.py
```

### Datetime express

```python
from datetime import datetime, timezone, timedelta
datetime.now(timezone.utc).isoformat()
datetime.strptime(s, "%Y-%m-%d %H:%M:%S")
```

---

## 65. Glossaire

| Terme | Définition |
|---|---|
| Argument | Valeur passée à une fonction à l'appel |
| Attribut | Variable attachée à un objet/classe |
| Bytecode | Code intermédiaire exécuté par la VM Python (`.pyc`) |
| Classe | Moule définissant objets (attributs + méthodes) |
| Closure | Fonction qui capture des variables de son contexte |
| Compréhension | Construction concise de collection `[x for x in ...]` |
| Coroutine | Fonction `async def` exécutable par `asyncio` |
| Décorateur | Fonction qui enveloppe une autre (`@deco`) |
| Dict | Dictionnaire : mapping clé → valeur |
| Docstring | Chaîne de documentation (`"""..."""`) |
| Dunder | Méthode spéciale `__nom__` (double underscore) |
| Exception | Erreur levée à l'exécution, attrapable via `try` |
| f-string | Chaîne formatée `f"..."` (Python 3.6+) |
| Générateur | Fonction avec `yield` : produit à la demande |
| GIL | Verrou global : 1 seul thread exécute du bytecode à la fois |
| Idempotent | Relançable sans effet de bord supplémentaire |
| Instance | Objet créé à partir d'une classe |
| Itérable | Objet parcourable par `for` / `in` |
| Kwargs | Arguments nommés supplémentaires (`**kwargs`) |
| Lambda | Petite fonction anonyme |
| Linter | Outil d'analyse statique (ruff, flake8) |
| Méthode | Fonction définie dans une classe |
| Module | Fichier `.py` importable |
| Mutable / Immutable | Modifiable (`list`) / non (`tuple`, `str`) |
| Namespace | Espace de noms (module, fonction...) |
| Package | Dossier de modules (`__init__.py`) |
| Paramètre | Variable déclarée dans la signature d'une fonction |
| PEP 8 | Guide de style officiel Python |
| Pickle | Sérialisation Python native (dangereuse sur données externes) |
| Polymorphisme | Même interface, comportements différents selon l'objet |
| REPL | Console interactive (`python3` sans argument) |
| Scope | Portée d'une variable (local/global...) |
| Shebang | `#!/usr/bin/env python3` en 1ère ligne |
| Slice | Sous-séquence `seq[début:fin:pas]` |
| Thread | Fil d'exécution (partage la mémoire) |
| Traceback | Pile d'appels affichée lors d'une exception |
| Tuple | Séquence immutable `(a, b)` |
| Type hint | Annotation de type (`def f(x: int) -> bool`) |
| venv | Environnement virtuel isolé |
| Wheel | Format de distribution binaire (`.whl`) |

---

## 66. Quiz (10 questions + réponses)

**Q1.** Que vaut `bool("")`, `bool([])`, `bool(0)` ?
> `False` pour les trois : ce sont des valeurs "falsy".

**Q2.** Quelle est la différence entre `a = b` et `a = b.copy()` pour une liste ?
> `a = b` : `a` pointe vers le **même** objet (modifier l'un modifie l'autre). `.copy()` crée une vraie copie indépendante (superficielle).

**Q3.** Pourquoi ce code est-il bogué ?
> ```text
> def ajouter(x, historique=[]):
>     historique.append(x)
>     return historique
> ```
> Le défaut `[]` est créé **une seule fois** à la définition : tous les appels partagent la même liste. Corriger avec `historique=None` puis création dans le corps.

**Q4.** `sorted(maliste)` vs `maliste.sort()` : différence ?
> `sorted()` retourne une **nouvelle** liste triée (original intact). `.sort()` trie **en place** et retourne `None`.

**Q5.** Dans quel ordre Python cherche-t-il un nom de variable (règle LEGB) ?
> **L**ocal → **E**nclosing → **G**lobal → **B**uiltins.

**Q6.** Pourquoi faut-il préférer `subprocess.run(["ping", host])` à `subprocess.run(f"ping {host}", shell=True)` ?
> Avec `shell=True` et une chaîne, un `host` malveillant (`"x; rm -rf /"`) provoque une **injection de commandes**. La liste d'arguments évite le shell.

**Q7.** Que fait le `else` d'une boucle `for` ?
> Il s'exécute **si la boucle s'est terminée sans `break`**. Utile pour "recherché mais non trouvé".

**Q8.** Pourquoi `0.1 + 0.2 != 0.3` en Python ?
> Les `float` sont en base 2 (IEEE 754) : `0.1` n'est pas représentable exactement. Utilisez `round()` ou `decimal.Decimal` pour des calculs exacts.

**Q9.** Threads ou multiprocessing pour : (a) pinger 200 hôtes, (b) chiffrer 4 gros fichiers ?
> (a) **Threads** (ou asyncio) : tâche I/O-bound, on attend le réseau. (b) **Multiprocessing** : tâche CPU-bound, le GIL empêche les threads de paralléliser le calcul.

**Q10.** Que manque-t-il à ce script pour être "production-ready" ?
> ```text
> import requests
> r = requests.get("https://api.lan/data")
> print(r.json())
> ```
> Au minimum : un `timeout=` (sinon blocage infini), `raise_for_status()` ou test du code HTTP, et un `try/except requests.RequestException`.

---

## 67. Pour aller plus loin

### Bibliothèques à connaître (sysadmin & réseau)

| Domaine | Bibliothèque | Usage |
|---|---|---|
| HTTP | `requests`, `httpx` (sync+async) | API, web |
| SSH | `paramiko`, `netmiko`, `nornir` | automatisation d'équipements réseau |
| Parsing | `pyyaml`, `openpyxl`, `pandas` | configs, Excel, data |
| CLI jolies | `rich`, `typer` | tableaux, barres de progression |
| Scheduling | `schedule`, `APScheduler` | tâches périodiques en Python |
| Chiffrement | `cryptography` | TLS, certificats, hash |
| Tests | `pytest`, `pytest-cov` | qualité |

### Sujets d'approfondissement

1. **Nornir / Netmiko** : automatiser des parcs de switches/routeurs en SSH.
2. **Ansible** (écrit en Python) : modules personnalisés en Python.
3. **Packaging avancé** : publier sur un index privé (devpi, Artifactory).
4. **mypy strict** : typer tout un projet et l'intégrer en CI.
5. **asyncio avancé** : `asyncio.Queue`, timeouts (`asyncio.wait_for`), `aiohttp`.
6. **C extensions / Cython** : quand le Python pur ne suffit plus.
7. **Contribuer** : lire le code source de `requests` ou `rich` — excellentes écoles de style.

### Ressources

- **Docs officielles** : https://docs.python.org/3/ (le tutoriel puis la référence de la bibliothèque)
- **PEP 8** : https://peps.python.org/pep-0008/
- **Real Python** : tutoriels de qualité (realpython.com)
- **`python -m pydoc`** : documentation intégrée en console
- **Exercices** : Exercism (exercism.org, track Python, mentorat gratuit)

> Dernier conseil : le meilleur moyen de progresser est d'automatiser une vraie corvée de votre quotidien. Prenez la tâche répétitive qui vous énerve le plus cette semaine — et scriptez-la.

---

*Fin du guide — bon code ! 🐍*
