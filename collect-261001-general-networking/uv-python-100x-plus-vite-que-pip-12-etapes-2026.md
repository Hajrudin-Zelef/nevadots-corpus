*Dernière mise à jour : 19 août 2026.* Si vous écrivez du Python en 2026, le gestionnaire de paquets **uv Python** est probablement l’outil le plus important à adopter cette année. Développé par la société **Astral** – les créateurs du linter Ruff – et écrit intégralement en Rust, uv se présente comme « un gestionnaire de paquets et de projets Python extrêmement rapide ». Concrètement, il remplace à lui seul `pip`, `pip-tools`, `pipx`, `poetry`, `pyenv`, `twine` et `virtualenv`, tout en étant de **10 à 100 fois plus rapide** que pip selon la documentation officielle d’Astral – un écart que le tutoriel UV publié par Han en décembre 2025 chiffre même, sur certains flux de travail, à **10 à 1000 fois plus rapide** que les outils Python traditionnels. La cadence de publication d’Astral s’est encore accélérée depuis : après la 0.12.2 du 5 août 2026, l’éditeur a livré la 0.12.6 le 25 août 2026, puis **uv 0.12.7**, la dernière version en date, le 27 août 2026.

Le sujet est d’autant plus brûlant que, le 19 mars 2026, **OpenAI a annoncé le rachat d’Astral** – et donc de uv, Ruff et de l’outil de typage ty. L’équipe rejoint le projet Codex d’OpenAI, mais s’est engagée à poursuivre le développement de ces outils en open source sous licence permissive, comme le rapporte le développeur Simon Willison. Autrement dit : uv n’est pas près de disparaître, bien au contraire.

Dans ce tutoriel, vous allez installer uv, gérer plusieurs versions de Python, puis **construire un projet Python complet et fonctionnel** de A à Z : une application en ligne de commande baptisée `pypeek` qui interroge l’API publique de PyPI pour afficher la dernière version d’un paquet. Vous verrez les dépendances, le verrouillage reproductible, les tests, le linting, la construction d’un paquet, la publication, la conteneurisation Docker et l’intégration continue. Comptez environ **40 minutes** pour les 12 étapes. Aucune installation préalable de Python n’est nécessaire : uv s’en charge.

## uv, le gestionnaire de paquets Python qui bouscule pip et Poetry

Pendant des années, l’écosystème Python a souffert d’une fragmentation de son outillage. On installait les paquets avec pip, on isolait les environnements avec virtualenv ou venv, on gérait les versions de l’interpréteur avec pyenv, on verrouillait les dépendances avec pip-tools ou Poetry, on lançait les outils CLI avec pipx, et on publiait sur PyPI avec twine. Sept outils, sept syntaxes, sept sources de bugs.

uv unifie tout cela derrière une seule commande. Le dépôt astral-sh/uv sur GitHub dépasse aujourd’hui les **87 000 étoiles**, ce qui en fait l’un des projets d’outillage Python les plus populaires jamais publiés. Il s’installe comme un binaire Rust autonome, sans nécessiter Python ni Rust au préalable, et fonctionne sur macOS, Linux et Windows.

Astral n’est pas un projet amateur : la société a levé **4 millions de dollars** en amorçage en 2023, tour mené par le fonds Accel avec la participation de figures comme Guillermo Rauch (Vercel) et Solomon Hykes (Docker). uv a été testé à grande échelle contre les **10 000 paquets PyPI les plus téléchargés** pour garantir sa compatibilité. C’est cette rigueur qui explique son adoption fulgurante dans les entreprises et les projets open source.

## OpenAI rachète Astral : ce que cela change pour uv (mars 2026)

Le 19 mars 2026, OpenAI a officialisé l’acquisition d’Astral. Le montant de la transaction n’a pas été communiqué. L’équipe d’Astral rejoint la division Codex d’OpenAI, l’assistant de programmation de l’entreprise, ce qui laisse deviner l’ambition : faire de uv et Ruff des briques centrales de l’expérience de développement Python assistée par IA.

La question qui a immédiatement agité la communauté était : uv va-t-il rester libre ? La réponse, réaffirmée par Astral et OpenAI, est oui. Les deux parties se sont engagées à maintenir uv, Ruff et ty en open source, sous une licence permissive qui autorise le fork si nécessaire. Pour les équipes qui ont déjà misé sur **uv Python** en production, il n’y a donc aucun changement de licence à craindre à court terme. Cette continuité est précisément ce qui rend le moment idéal pour adopter l’outil : sa gouvernance est désormais adossée à l’un des acteurs les mieux financés du secteur.

Ce rachat s’inscrit dans une tendance de fond : l’outillage de développement devient un champ de bataille stratégique pour les entreprises d’IA. Après tout, la vitesse d’installation et la reproductibilité des environnements conditionnent directement la productivité des agents de code automatisés. uv, capable de recréer un environnement complet en quelques centaines de millisecondes, est un candidat naturel pour ce rôle.

## uv Python contre pip, Poetry et pyenv : le tableau des remplacements

Avant de plonger dans l’installation, il est utile de cartographier ce que uv remplace. Le tableau ci-dessous met en regard chaque outil traditionnel et son équivalent uv. C’est aussi une bonne feuille de route mentale : si vous connaissez déjà pip ou Poetry, vous retrouverez vos réflexes.

| Outil traditionnel | Rôle | Commande uv équivalente | 
|---|---|---|
| pip | Installer des paquets | `uv add` /`uv pip install` | 
| virtualenv / venv | Créer des environnements isolés | `uv venv` (automatique via`uv run` ) | 
| pyenv | Gérer les versions de Python | `uv python install` /`uv python pin` | 
| pip-tools (pip-compile) | Verrouiller les dépendances | `uv lock` /`uv pip compile` | 
| pipx | Exécuter des outils CLI isolés | `uvx` /`uv tool install` | 
| Poetry / PDM | Gérer projet + dépendances | `uv init` /`uv add` /`uv sync` | 
| twine | Publier sur PyPI | `uv publish` | 
| build | Construire sdist et wheel | `uv build` | 

L’avantage n’est pas seulement la vitesse : c’est la cohérence. Toutes ces commandes partagent le même cache global, le même résolveur de dépendances et le même fichier de verrouillage. Vous n’avez plus à vous demander si Poetry et pip voient les mêmes versions – il n’y a plus qu’une seule source de vérité.

## Benchmarks : à quel point uv est-il plus rapide que pip ?

La promesse d’Astral, « 10 à 100 fois plus rapide que pip », se vérifie dans les mesures. Le facteur exact dépend surtout de l’état du cache : à froid, le gain est déjà net ; à chaud, il devient spectaculaire, car uv déduplique les paquets déjà téléchargés et se contente de créer des liens durs (hardlinks) plutôt que de recopier les fichiers.

| Opération | Outil classique | Accélération avec uv | 
|---|---|---|
| Résolution + installation (cache froid) | pip | 8 à 10× | 
| Réinstallation (cache chaud) | pip | 80 à 115× | 
| Création d’un environnement virtuel | `python -m venv` | ~80× | 
| Création d’un environnement virtuel | virtualenv | ~7× | 

Ces chiffres proviennent des benchmarks publiés par Astral et confirmés par des tests indépendants. En pratique, cela transforme le flux de travail : recréer un environnement propre en intégration continue passe de plusieurs dizaines de secondes à moins d’une seconde. Sur un monorepo avec des centaines de dépendances, l’écart cumulé se compte en heures d’ingénierie économisées chaque mois – une étude de Techplained publiée en avril 2026 chiffre ce gain à environ **58 heures par an** de temps de CI économisé face à pip, sur un échantillon de **10 400 exécutions** avec un cache mixte.

## Prérequis et versions

Ce tutoriel a été rédigé et testé avec la dernière version stable de uv, **0.12.7**, publiée par Astral le 27 août 2026 (comme le relève PythonDataBench). Voici ce dont vous avez besoin avant de commencer. Bonne nouvelle : la liste est courte, car uv n’exige quasiment rien en amont. Le rythme de publication d’Astral reste soutenu – entre la 0.12.2 du 5 août 2026, recensée par Modern Python Weekly, et la 0.12.6 du 25 août 2026, ce ne sont pas moins de trois versions qui sont sorties en l’espace de trois semaines – ce qui garantit des correctifs et améliorations quasi hebdomadaires.

| Élément | Version / exigence | Remarque | 
|---|---|---|
| uv | 0.11.26 (dernière stable) | Binaire Rust autonome | 
| Système d’exploitation | macOS, Linux ou Windows 10+ | Multiplateforme | 
| Python | Aucun préinstallé requis | uv le télécharge à la demande | 
| Terminal | bash, zsh ou PowerShell | Toutes les commandes sont en CLI | 
| Docker (facultatif) | 24+ | Uniquement pour l’étape 12 | 
| Compte PyPI / TestPyPI (facultatif) | – | Uniquement pour la publication | 
| Espace disque | Quelques centaines de Mo | Pour le cache global de uv | 

Si vous suivez d’autres tutoriels de développement sur Tech Insider, sachez que uv s’intègre parfaitement à des piles comme FastAPI ou aux projets d’IA locale de type Ollama. Nous y reviendrons.

## Étape 1 – Installer uv (macOS, Linux, Windows)

La méthode recommandée est l’installateur autonome, qui télécharge le binaire et l’ajoute au PATH – une approche documentée dès novembre 2025 par le tutoriel du CNRS SARI, qui présente uv comme le gestionnaire de projets Python moderne d’Astral et détaille cet installateur basé sur curl. Astral continue d’affiner ces scripts : la version 0.11.28, publiée le 7 juillet 2026, a notamment republié des scripts d’installation précompilés le même mois. Sur macOS et Linux, ouvrez un terminal et exécutez :

```
# macOS et Linux
curl -LsSf https://astral.sh/uv/install.sh | sh
```
Sous Windows, utilisez PowerShell :

```
# Windows (PowerShell)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```
Vous préférez passer par un gestionnaire de paquets existant ? uv est aussi disponible via pip, pipx et Homebrew. Les installateurs shell et PowerShell officiels, eux, existent depuis longtemps – ils ont été formalisés dès la version 0.9.27 du 26 janvier 2026 – et la 0.11.8, publiée le 27 avril 2026, a ajouté de nouveaux binaires d’installation pour davantage de plateformes :

```
# Alternatives
pip install uv        # via pip
pipx install uv       # via pipx
brew install uv       # via Homebrew (macOS/Linux)
```
Vérifiez ensuite que tout fonctionne et mettez uv à jour si besoin :

```
uv --version
# uv 0.11.26
# Mettre a jour uv (si installe via l'installateur autonome)
uv self update
```
Si la commande `uv` reste introuvable après l’installation, redémarrez votre terminal ou rechargez votre shell (`source ~/.bashrc` ou `source ~/.zshrc`). L’installateur ajoute uv à `~/.local/bin`, qui doit figurer dans votre PATH.

## Étape 2 – Gérer les versions de Python avec uv

C’est ici que uv Python remplace pyenv. uv sait télécharger, installer et épingler n’importe quelle version de l’interpréteur, sans compilation ni configuration système. Listez d’abord les versions disponibles et installées :

```
# Voir les versions disponibles et installees
uv python list
# Installer une version precise
uv python install 3.13
# Installer plusieurs versions d'un coup
uv python install 3.11 3.12 3.13
```
uv gère les téléchargements de manière transparente : si un projet réclame une version absente, il l’installe automatiquement au moment de l’exécution. Vous pouvez épingler une version pour un projet donné, ce qui crée un fichier `.python-version` lu par tous les collègues :

```
# Epingler la version pour le repertoire courant
uv python pin 3.13
# Cree un fichier .python-version contenant "3.13"
```
Cette approche élimine la classe entière de bugs « ça marche sur ma machine » liés à des interpréteurs divergents. Le fichier `.python-version` se versionne dans Git, garantissant que toute l’équipe – et la CI – utilise exactement le même Python.

## Étape 3 – Initialiser un projet uv Python

Nous allons créer un paquet distribuable, avec un point d’entrée en ligne de commande. L’option `--package` génère une structure « src layout » complète, prête pour la publication :

```
uv init --package pypeek
cd pypeek
```
uv crée l’arborescence suivante :

```
pypeek/
├── .python-version
├── README.md
├── pyproject.toml
└── src/
    └── pypeek/
        └── __init__.py
```
Le fichier `pyproject.toml` est le cœur du projet. Il contient les métadonnées, les dépendances et la déclaration du point d’entrée CLI. Voici à quoi il ressemble après l’initialisation :

```
[project]
name = "pypeek"
version = "0.1.0"
description = "Affiche la derniere version d'un paquet PyPI"
readme = "README.md"
requires-python = ">=3.13"
dependencies = []
[project.scripts]
pypeek = "pypeek:main"
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```
Notez la section `[project.scripts]` : elle déclare que la commande `pypeek` appellera la fonction `main()` du module. Nous écrirons cette fonction à l’étape 6.

## Étape 4 – Ajouter des dépendances avec uv add

Notre CLI a besoin de deux paquets : `httpx` pour les requêtes HTTP et `typer` pour construire l’interface en ligne de commande. La commande `uv add` les ajoute au `pyproject.toml`, met à jour le fichier de verrouillage `uv.lock` et synchronise l’environnement virtuel – le tout en une fraction de seconde :

`uv add httpx typer`
Sortie typique :

```
Resolved 12 packages in 84ms
Installed 12 packages in 21ms
 + httpx==0.28.1
 + typer==0.16.0
 + click==8.1.8
 + ...
```
La section `dependencies` de votre `pyproject.toml` reflète désormais ces ajouts, avec des contraintes de version minimales :

```
dependencies = [
    "httpx>=0.28.1",
    "typer>=0.16.0",
]
```
Point crucial à retenir : `uv add` n’est pas la même chose que `uv pip install`. Le premier modifie `pyproject.toml` et `uv.lock` – c’est le mode « projet », reproductible. Le second se comporte comme un pip classique et n’écrit rien dans les métadonnées. Pour un projet géré, utilisez toujours `uv add`. Vous n’avez d’ailleurs jamais besoin d’activer manuellement l’environnement : uv crée et gère le dossier `.venv` pour vous.

## Étape 5 – Dépendances de développement et groupes

Les outils de test et de qualité de code ne doivent pas être livrés à vos utilisateurs. uv les range dans un groupe de développement séparé grâce à l’option `--dev` :

`uv add --dev pytest ruff`
Ces dépendances atterrissent dans une section dédiée du `pyproject.toml`, distincte des dépendances de production :

```
[dependency-groups]
dev = [
    "pytest>=8.3.0",
    "ruff>=0.14.0",
]
```
Lors d’un déploiement en production, vous pourrez exclure ce groupe avec `uv sync --no-dev`, ce qui allège considérablement l’image finale. À l’inverse, en local et en CI, `uv sync` installe tout par défaut. Cette séparation nette entre dépendances applicatives et outils de développement est l’une des bonnes pratiques que uv rend triviales, là où un simple `requirements.txt` mélangeait souvent les deux.

## Étape 6 – Écrire l’application et l’exécuter avec uv run

Place au code. Ouvrez `src/pypeek/__init__.py` et remplacez son contenu par notre CLI, qui interroge l’API JSON publique de PyPI :

```
import httpx
import typer
app = typer.Typer(help="Affiche la derniere version d'un paquet PyPI.")
@app.command()
def version(paquet: str) -> None:
    """Recupere la derniere version publiee d'un paquet."""
    url = f"https://pypi.org/pypi/{paquet}/json"
    reponse = httpx.get(url, timeout=10.0)
    if reponse.status_code == 404:
        typer.secho(f"Paquet introuvable : {paquet}", fg=typer.colors.RED)
        raise typer.Exit(code=1)
    reponse.raise_for_status()
    info = reponse.json()["info"]
    typer.secho(f"{info['name']} {info['version']}", fg=typer.colors.GREEN)
    if info.get("summary"):
        typer.echo(info["summary"])
def main() -> None:
    app()
if __name__ == "__main__":
    main()
```
Exécutez l’application avec `uv run`. Cette commande synchronise l’environnement si nécessaire, puis lance votre code dans le bon interpréteur – sans que vous ayez à activer quoi que ce soit :

`uv run pypeek version requests`
Sortie attendue :

```
requests 2.32.3
Python HTTP for Humans.
```
Toujours utiliser `uv run` plutôt que `python` directement : c’est la garantie que le script s’exécute avec les bonnes dépendances et la bonne version de Python, celle épinglée à l’étape 2.

## Étape 7 – Le lockfile universel et les builds reproductibles

Le fichier `uv.lock`, généré et géré exclusivement par uv, est un **lockfile universel** au format TOML lisible par un humain : il capture les versions exactes et les empreintes (hashes) de chaque dépendance, pour toutes les plateformes à la fois. C’est ce qui rend un build reproductible d’une machine à l’autre. Depuis novembre 2025, la documentation d’Astral détaille en outre l’export de ce lockfile vers trois formats – `requirements.txt`, `pylock.toml` et une nomenclature CycloneDX – dont un SBOM CycloneDX 1.5 généré via `uv export --format cyclonedx1.5`, un atout pour la conformité logicielle en entreprise. Vous pouvez régénérer le lockfile explicitement :

```
# (Re)generer le fichier de verrouillage
uv lock
# Synchroniser l'environnement a partir du lock, sans le modifier
uv sync --frozen
# Verifier que le lock est a jour (echoue sinon) – ideal en CI
uv sync --locked
```
Règle d’or : **versionnez `uv.lock` dans Git, mais jamais le dossier `.venv`**. Ajoutez ce dernier à votre `.gitignore`. Le lockfile est la source de vérité ; l’environnement virtuel n’est qu’un artefact reconstructible en une seconde.

### Pourquoi le lockfile universel change la donne

Contrairement à un `requirements.txt` figé pour une seule plateforme, `uv.lock` encode les résolutions conditionnelles (par système d’exploitation, architecture ou version de Python) grâce à ce qu’Astral appelle la **résolution universelle**, un mécanisme que la documentation de résolution d’Astral, mise à jour en septembre 2026, continue de présenter comme le socle du lockfile. Un même fichier fonctionne donc à l’identique sur le portable macOS d’un développeur, sur un runner Linux en CI et dans une image Docker. En cas de conflit Git sur `uv.lock`, ne le résolvez jamais à la main : relancez simplement `uv lock`.

## Étape 8 – Exécuter des outils sans les installer (uvx)

uv remplace pipx pour exécuter des outils Python en ligne de commande. `uvx` (alias de `uv tool run`) lance un outil dans un environnement éphémère, sans polluer votre projet. Par exemple, pour vérifier du code avec Ruff sans même l’ajouter au projet :

```
# Executer un outil de maniere ephemere
uvx ruff check .
# Executer une version precise
uvx [email protected] --version
# Installer un outil de maniere permanente (globale)
uv tool install ruff
```
La distinction est importante : `uvx` convient aux usages ponctuels (une vérification rapide, un générateur de projet), tandis que `uv tool install` place l’exécutable dans votre PATH pour un usage quotidien, à la manière de pipx. Les deux profitent du cache global de uv, ce qui rend les lancements suivants quasi instantanés.

## Étape 9 – Scripts autonomes avec dépendances inline (PEP 723)

uv implémente le standard PEP 723, qui permet d’embarquer les dépendances directement dans l’en-tête d’un script Python isolé. Plus besoin de projet ni de `requirements.txt` pour un utilitaire d’une page. Créez un fichier `rapide.py` :

```
# /// script
# requires-python = ">=3.13"
# dependencies = ["httpx"]
# ///
import httpx
reponse = httpx.get("https://pypi.org/pypi/uv/json", timeout=10.0)
print("Derniere version de uv :", reponse.json()["info"]["version"])
```
Exécutez-le directement : uv lit l’en-tête, crée un environnement temporaire avec httpx, et lance le script – le tout automatiquement :

```
uv run rapide.py
# Derniere version de uv : 0.11.26
```
Vous pouvez aussi ajouter une dépendance à un script via `uv add --script rapide.py rich`, ou fournir des dépendances ad hoc sans modifier le fichier avec `uv run --with rich rapide.py`. Cette fonctionnalité est idéale pour partager des scripts d’automatisation qui « fonctionnent partout » sans instructions d’installation.

## Étape 10 – Tester, linter et formater

Un projet complet a besoin de tests. Créez un dossier `tests/` avec un fichier `test_cli.py` qui vérifie le comportement de notre CLI grâce au `CliRunner` de Typer :

```
from typer.testing import CliRunner
from pypeek import app
runner = CliRunner()
def test_version_paquet_connu():
    resultat = runner.invoke(app, ["version", "pip"])
    assert resultat.exit_code == 0
    assert "pip" in resultat.stdout
def test_paquet_inexistant():
    resultat = runner.invoke(app, ["version", "paquet-qui-nexiste-pas-xyz"])
    assert resultat.exit_code == 1
```
Lancez la suite de tests avec `uv run`, qui utilise le pytest du groupe de développement :

```
uv run pytest -q
# ..                                    [100%]
# 2 passed in 0.42s
```
Enfin, contrôlez la qualité et le formatage avec Ruff – l’autre outil d’Astral, écrit lui aussi en Rust :

```
# Detecter les problemes de style et de logique
uv run ruff check .
# Formater automatiquement le code
uv run ruff format .
```
Puisque uv et Ruff partagent le même éditeur, leur intégration est sans friction. Cette pile Astral (uv + Ruff) est aujourd’hui l’un des socles les plus rapides pour l’outillage Python moderne. Consultez la documentation de Ruff pour affiner vos règles de style.

## Étape 11 – Construire et publier le paquet

uv remplace `build` et `twine`. Pour générer les artefacts de distribution (une archive source « sdist » et un « wheel »), une seule commande suffit :

```
uv build
# Successfully built dist/pypeek-0.1.0.tar.gz
# Successfully built dist/pypeek-0.1.0-py3-none-any.whl
```
Pour la publication, il est fortement recommandé de tester d’abord sur TestPyPI avant de viser le vrai PyPI. uv lit le jeton d’authentification depuis la variable d’environnement `UV_PUBLISH_TOKEN` :

```
# Publier sur TestPyPI pour valider
export UV_PUBLISH_TOKEN="pypi-VOTRE_JETON"
uv publish --publish-url https://test.pypi.org/legacy/
# Puis, une fois valide, sur le vrai PyPI
uv publish
```
En cas d’erreur 403 lors de la publication, vérifiez la validité de votre jeton et que le nom du paquet n’est pas déjà pris sur l’index visé. Le flux `uv build` puis `uv publish` remplace intégralement l’ancien duo `python -m build` + `twine upload`.

## Étape 12 – Conteneuriser avec Docker et automatiser en CI/CD

Pour déployer, rien de tel qu’une image Docker minimale. Astral publie des images officielles contenant le binaire uv. Voici un `Dockerfile` optimisé, qui met en cache les dépendances séparément du code source pour accélérer les reconstructions :

```
FROM ghcr.io/astral-sh/uv:python3.13-bookworm-slim
# Optimisations recommandees par Astral
ENV UV_COMPILE_BYTECODE=1
ENV UV_LINK_MODE=copy
WORKDIR /app
# 1) Installer d'abord les dependances (couche mise en cache)
COPY pyproject.toml uv.lock ./
RUN uv sync --frozen --no-dev --no-install-project
# 2) Copier le code puis installer le projet
COPY . .
RUN uv sync --frozen --no-dev
ENTRYPOINT ["uv", "run", "pypeek"]
```
Construisez et lancez l’image :

```
docker build -t pypeek .
docker run --rm pypeek version fastapi
```
Côté intégration continue, l’action officielle `astral-sh/setup-uv` installe uv et active le cache en une ligne. Voici un workflow GitHub Actions qui installe les dépendances, lance Ruff et exécute les tests :

```
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Installer uv
        uses: astral-sh/setup-uv@v6
        with:
          enable-cache: true
      - name: Synchroniser (build reproductible)
        run: uv sync --locked
      - name: Linter
        run: uv run ruff check .
      - name: Tests
        run: uv run pytest -q
```
Vérifiez la dernière version majeure de l’action `setup-uv` dans sa documentation avant de l’épingler. Combinée au cache, cette configuration ramène le temps d’installation des dépendances en CI à quelques centaines de millisecondes. Si vous conteneurisez d’autres services, notre tutoriel Docker complète parfaitement cette étape.

## Migrer depuis pip et Poetry vers uv

Vous avez un projet existant ? La transition est indolore. Pour importer un ancien `requirements.txt` dans un projet uv, utilisez :

```
# Importer les dependances d'un requirements.txt
uv add -r requirements.txt
# Ou, en mode pip pur, generer un requirements.txt verrouille
uv pip compile pyproject.toml -o requirements.txt
# Installer depuis un requirements.txt (interface pip)
uv pip install -r requirements.txt
```
Depuis Poetry, la structure `pyproject.toml` est largement compatible : dans la plupart des cas, il suffit de déplacer les dépendances vers la section standard `[project]`, de supprimer les sections spécifiques à Poetry, puis de lancer `uv lock`. Beaucoup d’équipes conservent d’ailleurs Poetry et uv en parallèle pendant la période de transition, sans conflit, car uv respecte les standards de packaging Python (PEP 621). Selon le guide 2026 de Pynions, cette bascule est désormais si aboutie qu’un unique fichier `uv.lock` remplace purement et simplement les anciens flux `requirements.txt` et pip-tools pour la gestion des dépendances d’un projet.

## uv contre Poetry, PDM et Rye : lequel choisir en 2026 ?

uv n’est pas seul sur le créneau des gestionnaires de projets Python. Poetry reste très répandu, PDM a des adeptes, et Rye – dont la maintenance a été confiée à Astral – fait figure de précurseur que uv est venu remplacer. Une comparaison indépendante de Techplained, menée en avril 2026 sur uv 0.5+ face à pip 25+, Poetry 2.x et PDM 2.x, a confirmé des accélérations de **5 à 8 fois** par rapport à Poetry. Le tableau suivant résume les différences structurelles qui font aujourd’hui pencher la balance vers uv.

| Critère | uv | Poetry | PDM | pip + venv | 
|---|---|---|---|---|
| Vitesse d’installation | Très élevée (Rust) | Moyenne | Moyenne | Faible | 
| Versions de Python intégrées | Oui | Non | Non | Non | 
| Lockfile universel | Oui | Oui (poetry.lock) | Oui | Non | 
| Remplace pipx / pyenv / twine | Oui | Non | Partiel | Non | 
| Standard PEP 621 | Oui | Oui (récent) | Oui | – | 
| Langage d’implémentation | Rust | Python | Python | Python | 

La différence décisive tient en deux points. D’abord, uv est le seul à intégrer nativement la gestion des versions de Python : nul besoin d’installer pyenv en parallèle. Ensuite, sa vitesse – héritée de Rust et de son résolveur – le place dans une catégorie à part pour les gros projets et la CI. Poetry demeure un excellent choix si votre équipe l’a déjà standardisé et que la vitesse n’est pas critique, mais pour un nouveau projet en 2026, **uv Python** est le point de départ recommandé. Sa reprise par OpenAI ne fait que renforcer la solidité de ce pari à long terme.

## Exemple concret : gérer un service FastAPI avec uv

Notre CLI illustre bien le cycle de vie d’un paquet, mais uv brille tout autant pour les applications web. Voici comment démarrer un microservice FastAPI en quelques secondes, dépendances verrouillées comprises :

```
uv init --package web-api
cd web-api
uv add fastapi "uvicorn[standard]"
```
Dans `src/web_api/__init__.py`, un point d’entrée minimal :

```
from fastapi import FastAPI
app = FastAPI(title="web-api")
@app.get("/sante")
def sante() -> dict[str, str]:
    return {"statut": "ok"}
```
Lancez le serveur de développement, toujours via `uv run`, qui garantit le bon environnement :

```
uv run uvicorn web_api:app --reload
# Uvicorn running on http://127.0.0.1:8000
```
La force de cette approche est la reproductibilité : le `uv.lock` généré capture non seulement FastAPI et Uvicorn, mais aussi toute leur chaîne de dépendances transitives, avec leurs empreintes cryptographiques. Vos collègues, vos serveurs et votre pipeline CI obtiendront tous exactement les mêmes versions. Le même mécanisme s’applique aux projets d’intelligence artificielle : un service qui appelle un modèle local via Ollama ou orchestre des agents avec LangGraph se gère de la même manière, avec les mêmes garanties.

## Structure finale du projet et bonnes pratiques d’équipe

Au terme des douze étapes, votre projet `pypeek` présente une structure propre, standard et reproductible :

```
pypeek/
├── .github/workflows/ci.yml
├── .gitignore
├── .python-version
├── Dockerfile
├── README.md
├── pyproject.toml
├── uv.lock
├── src/
│   └── pypeek/
│       └── __init__.py
└── tests/
    └── test_cli.py
```
Un point souvent négligé : le fichier `.gitignore`. Il doit exclure l’environnement virtuel et le cache, mais surtout **pas** le lockfile ni le fichier de version Python :

```
# .gitignore
.venv/
__pycache__/
dist/
*.egg-info/
# NE PAS ignorer : uv.lock et .python-version (a versionner !)
```
Pour une équipe, quelques règles simples garantissent une expérience fluide. Versionnez toujours `uv.lock` et `.python-version` : ils font partie du contrat du projet. En CI, utilisez systématiquement `uv sync --locked` afin d’échouer explicitement si le lock n’est pas à jour, plutôt que d’installer silencieusement des versions divergentes. Traitez les modifications de `uv.lock` comme du code : elles doivent apparaître dans les revues de pull request, car un changement de dépendance transitive peut avoir des conséquences. Enfin, encouragez chacun à utiliser `uv run` pour toute commande liée au projet, ce qui élimine les différences d’environnement entre les postes. Avec ces habitudes, la promesse de reproductibilité de uv devient une réalité opérationnelle plutôt qu’un simple argument marketing.

## Aide-mémoire des commandes uv essentielles

Gardez ce tableau sous la main : il couvre 90 % de votre usage quotidien de uv Python.

| Commande | Action | 
|---|---|
| `uv init --package nom` | Créer un nouveau projet packagé | 
| `uv add paquet` | Ajouter une dépendance (met à jour le lock) | 
| `uv add --dev paquet` | Ajouter une dépendance de développement | 
| `uv remove paquet` | Retirer une dépendance | 
| `uv run commande` | Exécuter dans l’environnement du projet | 
| `uv lock` | Générer / mettre à jour `uv.lock` | 
| `uv sync --locked` | Installer depuis le lock (CI) | 
| `uv python install 3.13` | Installer une version de Python | 
| `uv python pin 3.13` | Épingler la version du projet | 
| `uvx outil` | Exécuter un outil de façon éphémère | 
| `uv build` | Construire sdist + wheel | 
| `uv publish` | Publier sur PyPI | 
| `uv cache clean` | Vider le cache global | 

## Erreurs fréquentes et pièges à éviter

Voici les pièges les plus courants observés lors de l’adoption de uv, et comment les éviter.

- **Confondre `uv add` et `uv pip install`.** Seul`uv add` met à jour`pyproject.toml` et le lock.`uv pip install` se comporte comme pip et n’assure aucune reproductibilité pour un projet géré.
- **Versionner `.venv` dans Git.** N’ajoutez jamais l’environnement virtuel au dépôt. Versionnez`uv.lock` et`.python-version` , et mettez`.venv` dans`.gitignore` .
- **Oublier `--locked` ou `--frozen` en CI.** Sans ces options, uv peut re-résoudre les dépendances et introduire des versions différentes de celles testées en local.
- **Lancer `python script.py` au lieu de `uv run`.** Vous risquez d’utiliser le mauvais interpréteur ou un environnement global pollué.
- **Mélanger dépendances de production et de développement.** Utilisez`--dev` pour pytest, Ruff et consorts, afin de ne pas les livrer aux utilisateurs.
- **Installer des paquets avec un pip global dans un projet uv.** Cela contourne le lock et brise la cohérence de l’environnement géré.

## Dépannage : 8 problèmes courants et leurs solutions

Cette section rassemble huit situations fréquentes rencontrées en production, avec la marche à suivre.

- **1. `uv: command not found` après installation.** Le binaire est dans`~/.local/bin` ; redémarrez le terminal ou rechargez votre shell (`source ~/.bashrc` ) pour rafraîchir le PATH.
- **2. « No solution found » lors de la résolution.** Deux contraintes de version sont incompatibles. Assouplissez une borne dans`pyproject.toml` ou inspectez le conflit affiché par uv.
- **3. Conflit Git sur `uv.lock`.** Ne fusionnez pas le fichier à la main : relancez`uv lock` pour le régénérer proprement.
- **4. Erreur de certificat SSL en entreprise.** Ajoutez l’option`--native-tls` , ou définissez`SSL_CERT_FILE` pour pointer vers le certificat racine de votre organisation.
- **5. Mauvaise version de Python utilisée.** Vérifiez le fichier`.python-version` et exécutez`uv python pin` pour forcer la bonne version.
- **6. `uv sync` supprime un paquet installé à la main.** C’est normal : l’environnement reflète strictement le lock. Ajoutez la dépendance avec`uv add` au lieu d’un pip manuel.
- **7. Publication qui échoue en 403.** Jeton invalide ou nom de paquet déjà pris. Régénérez`UV_PUBLISH_TOKEN` et testez d’abord sur TestPyPI.
- **8. Build Docker lent.** Copiez`pyproject.toml` et`uv.lock` avant le code source pour profiter du cache de couches, et activez`UV_LINK_MODE=copy` .

## Astuces avancées pour aller plus loin

Une fois les bases maîtrisées, ces fonctionnalités avancées font passer votre usage de uv à la vitesse supérieure.

### Workspaces pour les monorepos

À la manière de Cargo (l’outil de Rust), uv gère des **workspaces** : plusieurs paquets dans un même dépôt, partageant un unique `uv.lock`. Déclarez-les dans le `pyproject.toml` racine :

```
[tool.uv.workspace]
members = ["packages/*"]
```
### Cache, mode hors ligne et builds figés dans le temps

Le cache global vit dans `~/.cache/uv`. Nettoyez-le avec `uv cache clean` ou élaguez les entrées obsolètes avec `uv cache prune`. Pour des builds strictement reproductibles, l’option `--exclude-newer 2026-06-01` ignore toute version publiée après une date donnée. Enfin, `--offline` force uv à n’utiliser que le cache, utile en environnement isolé.

### Index privés et registres d’entreprise

Pour tirer des paquets depuis un miroir interne, définissez la variable `UV_INDEX_URL` ou déclarez un index dans le `pyproject.toml`. uv gère l’authentification par jetons et prend en charge plusieurs index avec priorités, ce qui couvre les besoins des grandes organisations. Ces mécanismes, combinés à l’assistance d’outils comme Claude Code ou GitHub Copilot, forment une chaîne d’outillage Python particulièrement productive en 2026.

## Foire aux questions (FAQ)

### uv est-il gratuit et open source ?

Oui. uv est distribué sous licence permissive (Apache 2.0 ou MIT) et reste gratuit. Le rachat d’Astral par OpenAI en mars 2026 s’est accompagné d’un engagement public à maintenir cette ouverture, avec la possibilité de forker le projet si besoin.

### Dois-je désinstaller pip ou Poetry pour utiliser uv ?

Non. uv coexiste sans problème avec pip et Poetry. Vous pouvez migrer progressivement, projet par projet, sans big bang. uv respecte les standards de packaging Python, ce qui facilite les allers-retours.

### uv fonctionne-t-il sous Windows ?

Oui, uv est multiplateforme : macOS, Linux et Windows 10 et supérieur sont pris en charge. L’installation se fait via un script PowerShell, et toutes les commandes présentées ici fonctionnent à l’identique.

### uv remplace-t-il conda ?

En partie. uv couvre l’installation de paquets, la gestion des versions de Python et l’isolation des environnements. En revanche, pour les piles scientifiques lourdes qui reposent sur des binaires non-Python complexes, conda reste parfois pertinent, même si uv gère la grande majorité des cas via les wheels de PyPI.

### Où est stocké le cache de uv et comment le vider ?

Le cache global se trouve dans `~/.cache/uv` (personnalisable via `UV_CACHE_DIR`). Videz-le avec `uv cache clean`. C’est ce cache partagé, avec sa déduplication par liens durs, qui explique les gains de vitesse à chaud pouvant atteindre 80 à 115×.

### Le fichier uv.lock est-il vraiment multiplateforme ?

Oui. Le lockfile universel de uv encode les résolutions pour différents systèmes, architectures et versions de Python. Un même `uv.lock` garantit des installations identiques sur macOS, Linux et Windows, ce qu’un `requirements.txt` classique ne permet pas.

### uv gère-t-il les monorepos et projets multi-paquets ?

Oui, grâce aux workspaces de style Cargo. Vous déclarez les membres dans le `pyproject.toml` racine, et uv résout l’ensemble avec un seul lockfile partagé – idéal pour les grandes bases de code.

### Puis-je utiliser uv avec un projet requirements.txt existant ?

Absolument. Importez les dépendances avec `uv add -r requirements.txt`, ou restez en mode pip avec `uv pip install -r requirements.txt`. La migration est progressive et sans rupture.

### Pour aller plus loin : articles liés

*Sources et documentation : documentation officielle de uv, dépôt GitHub astral-sh/uv et page PyPI de uv. Toutes les données de version et de performance reflètent l’état de uv 0.12.7 au 27 août 2026.*