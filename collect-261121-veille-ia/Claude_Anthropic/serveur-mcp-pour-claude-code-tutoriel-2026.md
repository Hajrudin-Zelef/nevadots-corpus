---
id: collect-261121-veille-ia/claude-anthropic/serveur-mcp-pour-claude-code-tutoriel-2026
title: "Serveur MCP pour Claude Code : Tutoriel 2026"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["serveur", "claude", "code", "outils", "donnees", "tutoriel", "tickets", "comme"]
source: https://tech-insider.org/fr/serveur-mcp-claude-code-2026/
source_anchor: ""
source_lines: [1, 343]
section: "Claude_Anthropic"
---
Le protocole MCP (Model Context Protocol) est devenu, en quelques mois à peine, le langage commun qui permet à Claude Code, Cursor, GitHub Copilot et une bonne partie des outils de codage IA de se connecter aux mêmes sources de données externes : dépôts Git, bases de données, outils de tickets, documentation interne. Lancé par Anthropic comme standard ouvert le 25 novembre 2024, il revendique aujourd’hui plus de 10 000 serveurs publics actifs selon l’annonce d’Anthropic sur sa donation du protocole à l’Agentic AI Foundation — un chiffre déjà dépassé sur le terrain, puisque le registre communautaire PulseMCP en recensait 22 311 en juillet 2026, contre environ 14 000 à peine deux mois plus tôt en mai, selon le suivi publié par ShareuHack — et les kits de développement officiels cumulent désormais plus de 97 millions de téléchargements au total sur l’ensemble des plateformes, toujours d’après ShareuHack, confirmant la tendance déjà repérée début 2026 par digitalapplied.com.

Ce tutoriel détaille la construction d’un serveur MCP fonctionnel, de zéro jusqu’à sa connexion effective à Claude Code : prérequis techniques, installation du kit de développement officiel, écriture des outils et de la ressource exposés par le serveur, tests avec l’Inspecteur MCP, déclaration dans la configuration de l’agent, pièges à éviter, dépannage et astuces avancées pour évoluer vers un serveur distant sécurisé. Treize étapes au total, pour un temps de mise en place d’environ 50 minutes.

Ce guide s’adresse à un développeur qui a déjà pris en main Claude Code, Cursor ou un autre client compatible MCP et qui veut désormais lui connecter ses propres outils, plutôt qu’à quelqu’un qui découvre ces agents pour la première fois. Chaque étape reste valable sur Windows, macOS et Linux, les variantes de commande étant précisées quand elles diffèrent d’un système à l’autre. Les exemples de sortie terminal cités restent illustratifs : le comportement exact peut varier légèrement selon la version du kit de développement installée au moment de la lecture.

## Qu’est-ce que le Model Context Protocol (MCP) ?

MCP est un standard ouvert qui définit comment un modèle de langage, via un client comme Claude Code, peut découvrir et appeler des outils externes, lire des ressources et récupérer des invites prédéfinies, sans que chaque éditeur d’outil ait à développer une intégration sur mesure pour chaque agent IA. Avant MCP, connecter un agent à une base de données ou à un système de tickets supposait souvent un connecteur propriétaire par paire outil/agent. Le protocole standardise cette relation une fois pour toutes : un serveur MCP écrit pour exposer un dépôt Git fonctionne aussi bien avec Claude Code qu’avec un autre client compatible, sans modification.

Concrètement, la différence se voit surtout au quotidien. Avant MCP, un développeur qui voulait que son agent IA tienne compte d’un ticket de support devait ouvrir l’outil de tickets dans un onglet séparé, copier le texte pertinent, puis le coller dans la conversation avec l’agent, en répétant l’opération à chaque nouvelle information nécessaire. Avec un serveur MCP connecté à cet outil de tickets, la même information devient accessible directement depuis la session de l’agent, sur simple demande en langage naturel, sans jamais quitter le terminal ou l’éditeur. Le principe rappelle celui d’un port universel pour les échanges de données entre un modèle et le monde extérieur. La page Wikipédia consacrée au Model Context Protocol retrace la genèse du projet chez Anthropic fin 2024 et son adoption rapide par le reste de l’industrie l’année suivante. Un serveur MCP peut exposer trois types d’éléments : des outils (des fonctions que l’agent peut appeler, comme lister des commits Git), des ressources (des données consultables, comme le contenu d’un fichier ou l’état d’un dépôt) et des invites préconçues (des modèles de requête réutilisables). Le tutoriel qui suit construit un serveur combinant les deux premiers.

### JSON-RPC, stdio et HTTP/SSE : l’architecture en bref

Techniquement, MCP repose sur JSON-RPC 2.0, un format d’échange de messages compact et bien établi, qui sert de base à la communication entre le client (l’agent) et le serveur. Trois transports coexistent désormais, stdio et HTTP restant le duo dominant selon la cartographie de l’écosystème publiée le 16 mai 2026 par Konishi — le registre officiel MCP en dénombrait pour sa part 9 400 en juillet 2026 selon Praxena, un chiffre qui illustre à quel point les méthodes de comptage restent hétérogènes d’une source à l’autre. Le transport stdio fait tourner le serveur comme un simple processus local, l’agent lui parlant via l’entrée et la sortie standard : c’est le mode le plus simple, celui que ce tutoriel utilise de bout en bout. Le transport HTTP, historiquement couplé à des évènements envoyés par le serveur (SSE), permet en revanche d’exposer un serveur MCP à distance, accessible par plusieurs postes ou plusieurs membres d’une équipe à la fois, au prix d’une configuration réseau et d’une authentification plus poussées, abordées plus loin dans la section consacrée aux astuces avancées. La spécification 2026-07-28, publiée fin juillet 2026 par l’Agentic AI Foundation en remplacement de la version 2025-11-25 selon Ottho, impose désormais le transport Streamable HTTP et déprécie officiellement l’ancien couple HTTP+SSE ; ce nouveau transport HTTP devient entièrement sans état (stateless), avec des capacités négociées requête par requête plutôt qu’à l’ouverture d’une session persistante, un changement d’architecture détaillé début août 2026 dans la newsletter de Victor Dibia et déjà commenté par Cloudflare.

Le choix du langage de développement reste ouvert. Les kits officiels couvrent désormais Python et TypeScript en premier lieu, mais aussi C#, publié par Microsoft en partenariat avec Anthropic — un engagement que Microsoft prolonge côté plateforme avec son propre Management MCP Server, passé en aperçu public dès mars 2026 avant d’atteindre la disponibilité générale en avril 2026 dans le cadre de son plan Wave 1 —, et Go, distribué par Block au sein de son agent Goose — les quatre kits reconnus comme SDK « Tier 1 » par le projet. Le 28 juillet 2026, le blog officiel de la spécification MCP a fait basculer ces quatre kits vers la version 2026-07-28 en même temps, gardant ainsi les implémentations Python, TypeScript, C# et Go strictement synchronisées à chaque évolution du protocole. Cette diversité explique en partie pourquoi le protocole s’est répandu aussi vite hors de l’écosystème Anthropic : une équipe déjà organisée autour d’un langage donné n’a pas besoin d’en apprendre un nouveau pour exposer ses propres outils à un agent IA.

## Pourquoi connecter Claude Code à des serveurs MCP en 2026 ?

Un agent de codage comme Claude Code raisonne déjà bien sur le code source qu’il a sous les yeux. Sa limite tient plutôt à tout ce qui vit en dehors du dépôt : un ticket dans un outil de suivi, une conversation dans un canal d’équipe, une table dans une base de données de production. Sans MCP, faire remonter ce contexte suppose des copier-coller manuels entre plusieurs fenêtres. Avec un serveur MCP correctement déclaré, l’agent peut interroger ces systèmes directement pendant une session, ce qui change concrètement la nature des tâches qu’on peut lui confier.

L’adoption du protocole depuis son lancement donne une idée de la vitesse à laquelle ce changement s’est installé dans l’industrie.

| Date | Étape de l’adoption | 
|---|---|
| 25 novembre 2024 | Anthropic publie MCP comme standard ouvert | 
| Mars 2025 | OpenAI adopte MCP dans ChatGPT Desktop, son kit Agents et l’API Responses | 
| Avril 2025 | Google DeepMind confirme la prise en charge de MCP pour ses modèles Gemini | 
| Mai 2025 | Microsoft annonce un support préliminaire à Build 2025, dans Windows 11, Copilot Studio et Azure OpenAI Service | 
| Septembre à novembre 2025 | Le registre de serveurs MCP progresse de 407 %, pour approcher 2 000 entrées | 
| Décembre 2025 | Anthropic transfère la gouvernance de MCP à l’Agentic AI Foundation, hébergée par la Linux Foundation | 
| Début 2026 | 28 % des entreprises du Fortune 500 ont déployé des serveurs MCP en production | 
| Mi-2026 | Plus de 10 000 serveurs MCP publics actifs recensés par Anthropic ; environ 97 millions de téléchargements mensuels cumulés pour les kits Python et TypeScript | 

Ce calendrier explique pourquoi la question ne se limite plus à l’écosystème Anthropic. OpenAI a rallié MCP dès mars 2025 selon l’article de TechCrunch consacré à cette annonce, un choix notable puisqu’il s’agit d’un protocole conçu par un concurrent direct. Nous avions d’ailleurs détaillé le fonctionnement de Codex CLI, l’agent de codage en ligne de commande d’OpenAI, qui bénéficie lui aussi de cette compatibilité croissante. Du côté des chiffres d’usage en entreprise, le rapport State-of-AI 2026 évoque une adoption de MCP dans 74 % des systèmes d’IA en production, avec une réduction du temps d’intégration de nouveaux outils de 73 % et des coûts de maintenance annuels réduits de 62 % par rapport à des connecteurs propriétaires développés en interne, selon les données reprises par l’analyse de WorkOS sur l’état de MCP en 2026.

Ces gains se retrouvent aussi côté déploiement d’agents. Début 2026, 80 % des entreprises du Fortune 500 font tourner des agents IA actifs en production, et 78 % des équipes IA d’entreprise déclarent au moins un agent adossé à MCP parmi ces déploiements. Le nombre exact de serveurs publics varie toujours sensiblement selon la source consultée, mais toutes convergent vers un ordre de grandeur nettement supérieur à celui de 2025 : Anthropic évoquait plus de 10 000 serveurs dans son annonce officielle, un suivi indépendant publié sur dev.to situait la fourchette entre 9 400 et plus de 17 000 serveurs selon les registres pris en compte pour environ 300 applications clientes recensées, le registre communautaire Glama en dénombrait 22 775 dès mai 2026, et PulseMCP en recensait 22 311 en juillet 2026, contre environ 14 000 seulement deux mois plus tôt, d’après ShareuHack. Cet écart tient surtout à la méthode de comptage (registre central unique ou somme de plusieurs registres communautaires), mais l’ordre de grandeur reste le même : le nombre de serveurs MCP publics a été multiplié par plusieurs dizaines depuis le lancement du protocole fin 2024. Cette dispersion des chiffres devrait toutefois s’atténuer : depuis le 9 décembre 2025, la gouvernance de MCP est officiellement passée sous l’Agentic AI Foundation, hébergée par la Linux Foundation, selon Praxena, et le registre officiel doit basculer avant avril 2026 vers une adresse unique, registry.modelcontextprotocol.io, coportée par Anthropic, GitHub, Microsoft et PulseMCP — de quoi, à terme, remplacer la mosaïque actuelle de registres communautaires comme Glama, ShareuHack ou le MCP Server Hub, qui recensait encore des services isolés comme Amap Maps au 30 décembre 2025.

## Prérequis techniques : versions, comptes et outils nécessaires

Avant de lancer la moindre commande, vérifiez les éléments suivants. Un environnement mal préparé reste la cause la plus fréquente des blocages recensés dans la section dépannage plus bas.

| Élément | Exigence minimale | Recommandation | 
|---|---|---|
| Python | Version 3.10 | Version 3.12 ou plus récente | 
| Node.js (pour l’Inspecteur MCP) | Version 18 | Version 20 ou supérieure | 
| Claude Code | Installé et authentifié | Dernière version disponible | 
| Gestionnaire de paquets Python | pip | uv, plus rapide pour isoler l’environnement | 
| Système d’exploitation | Windows 10, macOS 12 ou distribution Linux récente | Dernière version stable de l’OS | 
| Git | Recommandé (utilisé par le projet exemple) | Dernière version stable | 
| Éditeur de code | Facultatif | Utile pour relire le code du serveur | 
| Espace disque | 200 Mo disponibles | 1 Go si plusieurs serveurs testés en parallèle | 

Deux précisions méritent d’être posées avant de continuer. D’abord, Python et Node.js jouent chacun un rôle distinct ici : le serveur lui-même est écrit en Python dans ce tutoriel, tandis que Node.js n’intervient que pour exécuter l’Inspecteur MCP, un outil de test en ligne de commande distribué comme paquet npm. Vous pouvez très bien écrire un serveur MCP entièrement en TypeScript si vous préférez rester dans un seul écosystème, le kit de développement officiel existant dans les deux langages. Ensuite, si Claude Code n’est pas encore installé sur votre machine, notre tutoriel dédié à l’installation de Claude Code en 13 étapes couvre ce prérequis avant de revenir ici.

## Vue d’ensemble du projet : le serveur MCP que nous allons construire

Plutôt qu’un exemple jouet du type “bonjour le monde”, ce tutoriel construit un serveur réellement utile au quotidien : un serveur MCP baptisé `git-journal`, qui donne à Claude Code une vision structurée de l’activité récente d’un dépôt Git. Concrètement, il expose deux outils et une ressource.

Le premier outil, `derniers_commits`, retourne l’historique récent d’un dépôt sous une forme lisible par l’agent (hash court, auteur, date, message). Le second, `chercher_todo`, parcourt le code source à la recherche de commentaires `TODO` et `FIXME` laissés par l’équipe. La ressource, exposée sous l’identifiant `git://status`, donne l’état courant du dépôt, équivalent à un `git status --short`. Une fois ce serveur connecté, une simple demande comme “résume les cinq derniers commits et signale les TODO non traités” devient une requête que Claude Code peut satisfaire seul, sans que vous ayez à copier-coller la sortie de plusieurs commandes Git dans le terminal au préalable.

Ce choix d’exemple n’est pas anodin. La recherche menée en amont de ce tutoriel confirme que le raisonnement multi-fichiers assisté par une intégration Git profonde figure parmi les usages qui distinguent le plus nettement Claude Code d’un simple auto-complétion : plus l’agent dispose d’un contexte fiable sur l’historique et l’état du dépôt, moins il a besoin de deviner ou de redemander une information déjà présente ailleurs dans le projet. Un serveur MCP dédié au Git constitue ainsi un point d’entrée pédagogique idéal avant de s’attaquer à des intégrations plus ambitieuses, vers un outil de tickets ou une base de données interne par exemple.

## Étapes 1 à 3 : préparer l’environnement Python et l’espace de travail

Les trois premières étapes se jouent avant même d’installer le kit de développement MCP. Elles évitent la majorité des erreurs recensées plus loin dans cet article.

- **Étape 1** : vérifiez la version de Python installée sur votre machine.
- **Étape 2** : créez un dossier de projet dédié et un environnement virtuel Python isolé.
- **Étape 3** : vérifiez Node.js et la présence de Claude Code, tous deux nécessaires plus loin dans ce tutoriel.

```
python3 --version
# La sortie doit afficher Python 3.10.x ou une version supérieure
# Exemple de sortie attendue : Python 3.12.4
```
Si la commande renvoie une version antérieure à 3.10 ou une erreur, installez une version récente depuis le site officiel de Python avant de continuer. Une fois la version confirmée, créez le dossier du projet et un environnement virtuel pour isoler les dépendances du serveur du reste de votre système.

```
mkdir serveur-mcp-git-journal && cd serveur-mcp-git-journal
python3 -m venv .venv
source .venv/bin/activate
# Sous Windows (PowerShell) :
# .venv\Scripts\Activate.ps1
```
Terminez par la vérification de Node.js et de Claude Code, tous deux utilisés plus loin, respectivement pour l’Inspecteur MCP et pour la connexion finale du serveur.

```
node --version
# Exemple de sortie attendue : v20.11.0
claude --version
# Exemple de sortie attendue : claude-code/1.x.x darwin-arm64 node-v20.11.0
```
## Étapes 4 à 6 : installer le kit de développement MCP et créer le squelette du serveur

**Étape 4** : installez le kit de développement officiel MCP pour Python, en incluant les outils en ligne de commande utiles au débogage.

```
pip install "mcp[cli]"
# Avec uv, en alternative plus rapide :
# uv add "mcp[cli]"
```
**Étapes 5 et 6** : créez un fichier `server.py` et posez le squelette minimal du serveur, avant d’y ajouter les outils proprement dits à l’étape suivante. Ce squelette suffit déjà à démarrer un serveur MCP valide, même s’il n’expose encore rien d’utile.

```
# server.py
from mcp.server.fastmcp import FastMCP
mcp = FastMCP("git-journal")
if __name__ == "__main__":
    mcp.run()
```
La classe `FastMCP` gère pour vous la partie protocolaire : sérialisation JSON-RPC, annonce des capacités du serveur au client, boucle d’écoute sur l’entrée standard. Le nom passé en argument, ici `git-journal`, sert d’identifiant unique lorsque plusieurs serveurs sont déclarés en parallèle dans la configuration de Claude Code, un point qui revient dans la section dépannage.

## Étapes 7 à 9 : écrire les outils et la ressource du serveur

**Étape 7** : ajoutez l’outil `derniers_commits`, qui interroge l’historique Git du dépôt courant via `subprocess`. Notez l’usage d’une liste d’arguments plutôt que d’une chaîne de commande unique : cela évite tout risque d’injection shell, même si les paramètres d’entrée venaient à contenir des caractères inattendus.

**Étape 8** : ajoutez l’outil `chercher_todo`, qui parcourt les fichiers sources à la recherche de commentaires `TODO` et `FIXME`.

**Étape 9** : ajoutez la ressource `git://status`, qui expose l’état courant du dépôt. Contrairement à un outil, une ressource n’attend pas nécessairement de paramètres : elle représente une donnée que l’agent peut consulter à la demande.

```
import subprocess
from mcp.server.fastmcp import FastMCP
mcp = FastMCP("git-journal")
@mcp.tool()
def derniers_commits(nombre: int = 5) -> str:
    """Retourne les N derniers commits du dépôt Git courant."""
    resultat = subprocess.run(
        ["git", "log", f"-{nombre}", "--pretty=format:%h | %an | %ad | %s", "--date=short"],
        capture_output=True, text=True, check=True
    )
    return resultat.stdout or "Aucun commit trouvé."
@mcp.tool()
def chercher_todo(dossier: str = ".") -> str:
    """Recherche les commentaires TODO et FIXME dans le dossier donné."""
    resultat = subprocess.run(
        ["grep", "-rn", "-E", "TODO|FIXME", dossier,
         "--include=*.py", "--include=*.js", "--include=*.ts"],
        capture_output=True, text=True
    )
    return resultat.stdout or "Aucun TODO ni FIXME trouvé."
@mcp.resource("git://status")
def statut_git() -> str:
    """Expose l'état courant du dépôt (git status --short)."""
    resultat = subprocess.run(
        ["git", "status", "--short"],
        capture_output=True, text=True
    )
    return resultat.stdout or "Aucune modification en attente."
```
La docstring placée sous chaque fonction n’a rien de décoratif : c’est elle que Claude Code lit pour décider quand appeler tel ou tel outil, en complément du nom de la fonction et des types déclarés sur ses paramètres. Un outil mal documenté reste techniquement fonctionnel, mais l’agent peine à deviner dans quel contexte l’utiliser, ce qui se traduit par des appels inutiles ou, à l’inverse, par un outil jamais sollicité alors qu’il aurait été pertinent.

## Étapes 10 et 11 : tester le serveur avec l’Inspecteur MCP

**Étape 10** : avant de connecter quoi que ce soit à Claude Code, testez le serveur isolément avec l’Inspecteur MCP, un outil officiel distribué en paquet npm qui ouvre une interface web locale pour appeler chaque outil manuellement.

```
npx @modelcontextprotocol/inspector python server.py
# L'Inspecteur ouvre une interface web locale, généralement sur
# http://localhost:6274, pour tester chaque outil manuellement
```
Dans cette interface, les deux outils et la ressource déclarés à l’étape précédente doivent apparaître dans une liste, avec la possibilité de renseigner leurs paramètres et de lancer un appel de test. C’est le moment de vérifier que `derniers_commits` renvoie bien un historique lisible et que `git://status` reflète l’état réel du dépôt, avant d’introduire la couche supplémentaire que représente Claude Code.

**Étape 11** : corrigez les erreurs signalées par l’Inspecteur avant d’aller plus loin. L’erreur la plus fréquente à ce stade concerne un binaire manquant dans l’environnement d’exécution.

```
# Erreur fréquente si git n'est pas accessible depuis l'environnement :
# FileNotFoundError: [Errno 2] No such file or directory: 'git'
# Vérifiez que git est bien installé au niveau du système
which git
# Sous Windows : where git
```
## Étapes 12 et 13 : connecter le serveur à Claude Code

**Étape 12** : une fois le serveur validé dans l’Inspecteur, déclarez-le auprès de Claude Code. Deux approches équivalentes existent : la commande dédiée, ou l’édition directe du fichier de configuration du projet.

`claude mcp add git-journal -- python /chemin/complet/vers/server.py`
Cette commande ajoute automatiquement l’entrée correspondante dans le fichier `.mcp.json` à la racine du projet. Vous pouvez aussi l’écrire vous-même directement :

```
{
  "mcpServers": {
    "git-journal": {
      "command": "python",
      "args": ["/chemin/complet/vers/server.py"]
    }
  }
}
```
Utilisez systématiquement un chemin absolu plutôt que relatif dans cette configuration : Claude Code peut être lancé depuis un dossier de travail différent de celui où vit réellement le fichier `server.py`, ce qui casse silencieusement la connexion si le chemin est relatif.

**Étape 13** : ouvrez une nouvelle session Claude Code dans le dossier du projet et vérifiez que le serveur apparaît bien comme actif, avant de lui poser une première question qui mobilise les outils que vous venez d’écrire.

À ce stade, Claude Code doit lister `git-journal` parmi ses serveurs MCP actifs au démarrage. Si ce n’est pas le cas, fermez complètement la session et relancez-la : la configuration n’est lue qu’au démarrage de l’agent, pas en cours de session.

## Exemple de projet complet : le code source intégral du serveur

Voici le fichier `server.py` complet, tel qu’il doit se présenter une fois les treize étapes suivies. Vous pouvez le copier tel quel dans un nouveau projet pour reproduire le tutoriel de bout en bout.

```
# server.py
# Serveur MCP "git-journal" : expose l'activité récente
# d'un dépôt Git à un client compatible MCP (Claude Code, Cursor, etc.)
import subprocess
from mcp.server.fastmcp import FastMCP
mcp = FastMCP("git-journal")
@mcp.tool()
def derniers_commits(nombre: int = 5) -> str:
    """Retourne les N derniers commits du dépôt Git courant."""
    resultat = subprocess.run(
        ["git", "log", f"-{nombre}", "--pretty=format:%h | %an | %ad | %s", "--date=short"],
        capture_output=True, text=True, check=True
    )
    return resultat.stdout or "Aucun commit trouvé."
@mcp.tool()
def chercher_todo(dossier: str = ".") -> str:
    """Recherche les commentaires TODO et FIXME dans le dossier donné."""
    resultat = subprocess.run(
        ["grep", "-rn", "-E", "TODO|FIXME", dossier,
         "--include=*.py", "--include=*.js", "--include=*.ts"],
        capture_output=True, text=True
    )
    return resultat.stdout or "Aucun TODO ni FIXME trouvé."
@mcp.resource("git://status")
def statut_git() -> str:
    """Expose l'état courant du dépôt (git status --short)."""
    resultat = subprocess.run(
        ["git", "status", "--short"],
        capture_output=True, text=True
    )
    return resultat.stdout or "Aucune modification en attente."
if __name__ == "__main__":
    mcp.run()
```
Ce fichier tient volontairement en une quarantaine de lignes, mais la structure se généralise sans effort : chaque nouvel outil est une fonction supplémentaire décorée avec `@mcp.tool()`, chaque nouvelle ressource suit le même principe avec `@mcp.resource()`. Une évolution naturelle de ce serveur consisterait à ajouter un outil `derniers_contributeurs` agrégeant les auteurs de commits sur une période donnée, ou une ressource exposant directement le contenu du fichier `CLAUDE.md` du projet.

## À quoi ressemble une session Claude Code avec MCP actif : exemples de sortie

Voici un exemple simplifié de ce qu’affiche le terminal une fois le serveur `git-journal` connecté, utile pour vérifier que votre propre installation se comporte normalement.

```
$ claude
Claude Code v1.x — connecté en tant que [email protected]
Serveurs MCP actifs : git-journal
> Résume les 5 derniers commits et signale s'il reste des TODO non traités
● Appel de l'outil derniers_commits(nombre=5)
● Appel de l'outil chercher_todo(dossier=".")
● Résultat : 5 commits résumés, 3 occurrences de TODO trouvées
  dans src/api/users.py (lignes 12, 47 et 88)
```
Si votre terminal n’affiche aucune mention de serveur MCP actif au démarrage, ou si l’agent répond sans jamais appeler les outils déclarés alors que la question s’y prête clairement, reportez-vous à la section dépannage : cela indique le plus souvent un chemin de configuration incorrect ou une session lancée depuis le mauvais dossier.

## Les erreurs courantes à éviter avec un serveur MCP

Ces pièges reviennent le plus souvent chez les développeurs qui construisent leur premier serveur MCP. La plupart se corrigent en quelques secondes une fois identifiés, mais ils suffisent à décourager un premier essai si personne ne les a signalés à l’avance, un constat déjà observé sur d’autres outils de codage IA et qui se vérifie tout autant sur la construction d’un serveur MCP maison.

- **Oublier d’activer l’environnement virtuel avant de lancer le serveur.** Claude Code lance la commande déclarée dans`.mcp.json` telle quelle, sans activer automatiquement un environnement virtuel Python à votre place.
- **Utiliser un chemin relatif dans la configuration.** Le dossier de travail au démarrage de l’agent ne correspond pas toujours à celui attendu ; un chemin absolu évite ce piège une fois pour toutes.
- **Négliger les docstrings des outils.** Sans description claire, l’agent choisit moins bien quand appeler un outil, ou l’ignore complètement.
- **Donner un accès trop large à un outil.** Exposer une fonction du type “exécute n’importe quelle commande shell” transforme un serveur pratique en risque de sécurité majeur, en particulier sur un dépôt partagé.
- **Ignorer la gestion des erreurs de `subprocess`.** Sans capture propre des erreurs, une commande Git absente ou un dépôt invalide fait planter le serveur au lieu de renvoyer un message exploitable.
- **Confondre outil et ressource.** Un outil doit modéliser une action paramétrable, une ressource une donnée consultable ; les mélanger complique inutilement la conception du serveur.
- **Ne jamais tester avec l’Inspecteur avant de connecter le serveur à Claude Code.** Sauter cette étape revient à déboguer à l’aveugle, à travers une couche supplémentaire qui masque l’origine réelle d’un problème.

## Dépannage : les problèmes les plus fréquents et leurs solutions

Voici les incidents les plus courants lors de la construction et de la connexion d’un serveur MCP, avec la cause probable et la marche à suivre. Avant de chercher plus loin dans cette liste, un réflexe simple règle une bonne partie des cas : fermer complètement la session Claude Code et le terminal, puis les rouvrir tous les deux, pour éviter de travailler avec une configuration ou un chemin resté en cache depuis avant la dernière modification du serveur.

- **« ModuleNotFoundError: No module named ‘mcp’ ».** L’environnement virtuel n’est pas activé au moment de lancer le serveur, ou la commande déclarée dans`.mcp.json` pointe vers un interpréteur Python différent de celui où le paquet a été installé.
- **Le serveur ne répond pas dans l’Inspecteur.** Vérifiez que`mcp.run()` reste bien la dernière instruction du fichier et qu’aucune erreur silencieuse n’interrompt le script avant d’atteindre cette ligne.
- **Claude Code ne détecte pas le serveur configuré.** Vérifiez le chemin absolu déclaré, puis redémarrez complètement la session : la configuration n’est relue qu’au lancement de l’agent.
- **« FileNotFoundError » pour git ou grep.** Le binaire n’est pas accessible depuis l’environnement d’exécution, un cas fréquent sous Windows sans Git Bash ni WSL installé.
- **Les outils apparaissent mais échouent silencieusement.** Ajoutez une gestion explicite des erreurs autour de chaque appel`subprocess` plutôt que de laisser une exception remonter sans contexte.
- **Conflit de nom entre deux serveurs MCP déclarés.** Deux entrées portant le même identifiant dans`mcpServers` entrent en conflit ; renommez l’une des deux pour lever l’ambiguïté.
- **Permission refusée lors de l’exécution du script.** Vérifiez les droits d’exécution du fichier ou invoquez-le explicitement via l’interpréteur Python plutôt que directement.
- **Le serveur fonctionne en local mais pas une fois déployé à distance.** Le transport HTTP/SSE nécessite une configuration réseau et une authentification distinctes du mode stdio local, détaillées dans la section suivante.
- **Lenteur perçue lors de l’appel d’un outil.** Un outil qui parcourt un dépôt volumineux sans limite peut ralentir sensiblement une session ; bornez le périmètre analysé, par exemple en limitant le nombre de commits ou de fichiers traités.
- **La sortie d’un outil dépasse ce que l’agent peut exploiter utilement.** Tronquez ou paginez la sortie renvoyée par vos fonctions plutôt que de renvoyer un historique Git entier sans limite.

## Astuces avancées : serveurs distants, sécurité et écosystème de serveurs prêts à l’emploi

Une fois le serveur local `git-journal` maîtrisé, plusieurs pistes permettent d’aller plus loin. La première consiste à basculer vers un transport HTTP/SSE plutôt que stdio, ce qui permet d’héberger le serveur sur une machine partagée et de le rendre accessible à toute une équipe plutôt qu’à un seul poste. Ce choix impose en retour une couche d’authentification, généralement fondée sur des jetons, pour éviter qu’un serveur exposé sur le réseau ne devienne un point d’accès non contrôlé aux systèmes qu’il expose.

Le principe de moindre privilège mérite d’être appliqué systématiquement : un outil ne devrait exposer que le strict nécessaire, jamais une commande shell arbitraire. Ajouter une journalisation des appels d’outils, même minimale, facilite également l’audit a posteriori si un comportement inattendu doit être analysé, un réflexe d’autant plus utile à mesure qu’un serveur MCP gagne en périmètre et en nombre d’utilisateurs.

Sur un serveur partagé par plusieurs développeurs, pensez aussi à borner le débit des appels par utilisateur, en particulier si un outil déclenche une requête coûteuse vers une base de données de production ou vers une API tierce facturée à l’appel. Une limite simple, par exemple un nombre maximal de requêtes par minute et par jeton d’authentification, évite qu’une session mal configurée ou qu’une boucle involontaire côté agent ne sature le serveur ou ne génère une facture inattendue. Ce point est souvent négligé lors d’un premier déploiement en HTTP/SSE, alors qu’il se règle en quelques lignes dès la conception du serveur.

### Compatibilité avec Cursor, VS Code et les autres clients MCP

L’un des intérêts pratiques de MCP tient à sa portabilité : le serveur `git-journal` construit dans ce tutoriel fonctionne sans modification avec n’importe quel client compatible, pas seulement Claude Code. Cursor prend en charge le même mécanisme de configuration par serveur, tout comme certaines extensions liées à GitHub Copilot en mode agent. Un seul serveur MCP écrit une fois peut donc être partagé entre plusieurs outils utilisés au sein d’une même équipe, plutôt que de dupliquer la même logique d’intégration pour chaque agent.

Il existe par ailleurs de nombreux serveurs MCP déjà écrits et maintenus, qu’il n’est pas nécessaire de redévelopper soi-même pour des besoins courants. Le mouvement s’est nettement accéléré courant 2026 : le 23 juillet 2026, GitHub a annoncé dans son changelog que son propre serveur MCP prenait déjà en charge la spécification 2026-07-28, avant même le déploiement officiel du 28 juillet, et son serveur officiel github/github-mcp-server est désormais accessible en HTTP distant avec authentification OAuth 2.1 depuis avril 2026. Firecrawl recensait dès le 24 février 2026 huit services majeurs proposant un serveur MCP distant prêt à l’emploi — GitHub, Vercel, Linear, Notion, Supabase, Stripe, Figma et Hugging Face —, une liste à laquelle Snowflake a depuis ajouté son propre serveur MCP géré, entré en avant-première le 2 octobre 2025 pour donner un accès sécurisé aux comptes Snowflake directement depuis une session d’agent. AWS avait ouvert la voie bien plus tôt, le 14 juillet 2025, avec son propre serveur MCP « AWS Price List », qui donne accès en temps réel aux tarifs de centaines de services AWS directement depuis une session d’agent, avant de compléter l’offre dès le 16 juillet 2025 avec AWS Knowledge MCP Server, lancé en préversion publique gratuite mais soumise à des limites de quotas.

| Serveur MCP | Éditeur / mainteneur | Usage principal | 
|---|---|---|
| Filesystem | Anthropic (référence officielle) | Lire et écrire des fichiers locaux de façon contrôlée | 
| GitHub | Communauté open source | Lire les tickets, pull requests et fichiers d’un dépôt distant | 
| Slack | Communauté open source | Lire et publier des messages depuis une session agent | 
| Postgres | Communauté open source | Interroger un schéma de base de données en lecture | 
| Stripe | Stripe | Consulter des données de paiement et de facturation | 
| Notion | Notion | Lire et modifier des pages et bases de données Notion | 

La liste officielle des serveurs MCP de référence, maintenue sur GitHub, reste le point de départ le plus fiable avant d’envisager d’écrire un serveur sur mesure : mieux vaut vérifier qu’un serveur existant ne couvre pas déjà le besoin avant de partir de zéro.

## Foire aux questions sur les serveurs MCP

**Qu’est-ce qu’un serveur MCP, en une phrase ?**

C’est un programme qui expose des outils, des ressources ou des invites à un agent IA compatible, comme Claude Code, selon un format d’échange standardisé plutôt qu’une intégration propriétaire.

**MCP est-il réservé à Claude Code ?**

Non. Bien qu’Anthropic ait créé le protocole, il a été adopté par OpenAI, Google DeepMind et Microsoft, et fonctionne aussi avec Cursor, VS Code et d’autres clients compatibles, comme détaillé dans la section sur la compatibilité plus haut.

**Faut-il savoir coder en Python pour créer un serveur MCP ?**

Le kit de développement officiel existe aussi en TypeScript. Ce tutoriel utilise Python car son kit `FastMCP` réduit le code nécessaire au minimum, mais le choix du langage reste libre.

**Un serveur MCP personnalisé est-il sûr à connecter à mon code source ?**

Oui, à condition de respecter le principe de moindre privilège détaillé plus haut : un outil ne devrait exposer que des actions précises et contrôlées, jamais une exécution de commande arbitraire non filtrée. Sur un dépôt sensible, testez d’abord le serveur sur un projet non critique, exactement comme on le ferait avant d’accorder à Claude Code lui-même des permissions étendues sur un dépôt de production.

**Peut-on utiliser des serveurs MCP tout faits sans en écrire un soi-même ?**

Absolument, et c’est même recommandé pour des besoins courants comme la lecture de fichiers, l’accès à GitHub ou à une base Postgres : la table de serveurs prêts à l’emploi plus haut couvre les cas les plus fréquents.

**Quelle différence entre un serveur MCP local (stdio) et un serveur distant (HTTP) ?**

Un serveur en stdio tourne comme un processus local propre à un poste, simple à mettre en place mais limité à un seul utilisateur. Un serveur en HTTP/SSE tourne sur une machine accessible par le réseau et peut être partagé par une équipe entière, moyennant une authentification adaptée.

**MCP va-t-il remplacer les extensions d’éditeur classiques comme GitHub Copilot ?**

Non, les deux répondent à des besoins différents. Copilot reste centré sur l’auto-complétion dans l’éditeur, tandis que MCP standardise la façon dont un agent accède à des données et outils externes, quel que soit l’agent utilisé par ailleurs. Les deux approches se combinent d’ailleurs très bien : rien n’empêche une équipe d’utiliser Copilot au quotidien pour l’auto-complétion tout en connectant Claude Code à des serveurs MCP pour les tâches qui demandent un contexte plus large que le seul fichier ouvert.

**Où trouver d’autres serveurs MCP prêts à l’emploi ?**

Le dépôt GitHub officiel `modelcontextprotocol/servers` référence les serveurs de démonstration maintenus par Anthropic et par la communauté, un bon point de départ avant d’en écrire un sur mesure.

### Pour aller plus loin

D’autres tutoriels publiés sur tech-insider.org complètent ce guide sur les outils de codage IA :
