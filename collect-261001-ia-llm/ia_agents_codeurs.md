# Les agents codeurs IA

**Guide pratique — état des lieux fin septembre 2026.**
Pour Zelef, sysadmin qui construit son RAG et veut maîtriser l'outillage IA/dev.

> Règle d'or de ce guide : tout ce qui suit a été vérifié par recherche web en
> septembre 2026. Les prix et limites changent vite — quand une valeur est
> incertaine, elle est marquée **« à vérifier »** avec la source officielle à
> consulter. Aucun mot de passe réel dans ce document : les secrets sont des
> exemples fictifs.

---

## Sommaire rapide

| Partie | Sections | Contenu |
|---|---|---|
| A — Les CLI agentiques | 1–45 | OpenCode, Claude Code, Codex CLI |
| B — Éditeurs & extensions agentiques | 46–72 | Cursor, Cline, Kilo Code |
| C — L'agent personnel autonome | 73–80 | OpenClaw (le « open clow ») |
| D — Plugins & complétion | 81–95 | Continue, Supermaven, Copilot, Tabnine, Amazon Q, JetBrains AI, Zed, Windsurf, Trae, Aider, Gemini CLI, Kiro… |
| E — Transversal | 96–115 | Comparatif, choix, équipe, sécurité, coûts/tokens, MCP, 20 pièges, cas pratiques, glossaire, quiz, pense-bête, pour aller plus loin |

---

## 1. Comment utiliser ce guide

1. **Lis d'abord les sections 2 à 5** : tu poses le vocabulaire (agent, MCP,
   AGENTS.md, contexte) et la carte du territoire.
2. **Choisis UN outil pour commencer** (section 97 t'aide) et suis son bloc
   d'installation pas à pas. N'installe pas tout le même jour : chaque agent a
   ses réflexes, ses fichiers de config et sa facturation.
3. **Fais le premier workflow commenté** de l'outil choisi, sur un projet
   jetable (`/tmp/lab-agent/`), jamais sur un repo de production.
4. **Reviens aux sections transversales** (96–115) avant d'utiliser un agent
   sur du vrai code : sécurité, coûts, revue du code généré.
5. **Le quiz (section 112)** sert de contrôle : si tu as moins de 8/10,
   relis les sections correspondantes.

Convention de notation dans ce guide :

- `$ commande` → à taper dans ton terminal (shell).
- Les blocs `json`/`toml`/`yaml` sont des fichiers de configuration réels.
- ⚠️ = piège fréquent. 💰 = point de coût. 🔒 = point de sécurité.

## 2. Ce qu'est (vraiment) un agent codeur

Un **agent codeur** n'est pas un chatbot qui répond du code. C'est un programme
qui tourne **en boucle** :

```
objectif (ton prompt)
   → le modèle raisonne
   → il appelle des OUTILS (lire un fichier, exécuter un test, chercher dans le repo)
   → il observe le RÉSULTAT de l'outil
   → il recommence jusqu'à atteindre l'objectif ou abandonner
```

Cette boucle s'appelle la **boucle agentique** (agentic loop). Trois
conséquences pratiques :

1. **Il agit, pas seulement il parle** : il modifie des fichiers, lance des
   commandes, crée des commits. D'où l'importance des garde-fous.
2. **Il consomme du contexte** : chaque tour de boucle envoie l'historique au
   modèle. Une session qui tourne 30 minutes peut brûler des centaines de
   milliers de tokens.
3. **Il peut se tromper en cascade** : une mauvaise lecture au tour 2 produit
   un mauvais patch au tour 5, « validé » par un test mal écrit au tour 8.
   La revue humaine reste obligatoire (section 100).

Distinguer trois familles, car on les confond souvent :

| Famille | Ce qu'elle fait | Exemples |
|---|---|---|
| **Complétion** | Suggère du code pendant que tu tapes | Supermaven, Copilot (mode tab), Tabnine |
| **Chat in-IDE** | Répond à des questions sur ton code | Continue (chat), Copilot Chat |
| **Agent** | Exécute des tâches multi-étapes de façon autonome | Claude Code, Codex CLI, OpenCode, Cline, Cursor Agent, Kilo Code |

Un même produit mélange souvent les trois (Cursor fait les trois). Ce guide
les traite séparément pour que tu saches ce que tu paies et ce que tu
autorises.

## 3. Le vocabulaire minimal (à connaître par cœur)

- **Contexte (context window)** : la quantité de texte (en tokens) que le
  modèle peut « voir » d'un coup. Un repo de 200 000 lignes ne rentre jamais
  en entier : l'agent doit choisir quoi lire. Les fenêtres « 1M tokens »
  existent mais coûtent cher et se dégradent en qualité sur les très longs
  contextes.
- **Token** : unité de facturation. ≈ 4 caractères en anglais, ≈ 2–3 en
  français pour du texte ; le code est plus dense. On paie l'**input**
  (ce qu'on envoie) et l'**output** (ce que le modèle génère), à des tarifs
  différents.
- **MCP (Model Context Protocol)** : standard ouvert (initié par Anthropic)
  pour brancher des **outils** sur un agent : un « serveur MCP » expose des
  fonctions (ex. : interroger ta base, piloter ton navigateur, lire tes
  tickets). En 2026, MCP est le standard de fait : presque tous les agents de
  ce guide le supportent.
- **Skills** : paquets de savoir-faire réutilisables (ex. : « comment écrire
  un playbook Ansible à la mode Zelef »). Claude Code les lit dans
  `.claude/skills/`, OpenClaw dans son ClawHub, Kilo Code dans `.agents/skills/`
  (standard ouvert agentskills.io).
- **AGENTS.md / CLAUDE.md** : fichier d'instructions placé à la racine du
  projet, lu automatiquement par l'agent à chaque session. C'est TA doc de
  cadrage : conventions, commandes de test, choses interdites. Le standard
  `AGENTS.md` (multi-outils) a supplanté les fichiers propriétaires.
- **Plan mode** : l'agent explore et propose un plan **sans écrire** de code.
  Tu valides, puis il exécute. Le réflexe n°1 anti-catastrophe.
- **Sandbox / permissions** : ce que l'agent a le droit de faire sans te
  demander (lire ? écrire ? exécuter du shell ? le réseau ?). Chaque outil a
  son modèle ; tous se règlent.
- **BYOK (Bring Your Own Key)** : tu branches ta propre clé API au lieu de
  payer l'abonnement de l'outil. Moins cher si tu consommes peu, sans plafond
  « fair use », mais facturation au token à surveiller.
- **Subagent** : un agent lancé par l'agent principal pour une sous-tâche
  (ex. : explorer 3 dossiers en parallèle), avec son propre contexte. Très
  puissant, très gourmand en tokens.

## 4. Carte du territoire (septembre 2026)

```
                        ┌─────────────────────────┐
                        │   CE QUE TU VEUX FAIRE  │
                        └────────────┬────────────┘
                                     │
            ┌────────────────────────┼────────────────────────┐
            │                        │                        │
     TERMINAL PUR              DANS L'ÉDITEUR           AUTONOME / PERSO
            │                        │                        │
  ┌─────────┴─────────┐   ┌──────────┴──────────┐    ┌────────┴────────┐
  │ Claude Code       │   │ Cursor (éditeur)    │    │ OpenClaw        │
  │ Codex CLI         │   │ Cline (extension)   │    │ (WhatsApp/      │
  │ OpenCode (TUI)    │   │ Kilo Code (ext.)    │    │  Telegram/      │
  │ Kilo Code (CLI)   │   │ Continue (extension)│    │  cron…)         │
  │ Gemini CLI, Aider │   │ Copilot, Windsurf…  │    │                 │
  └───────────────────┘   └─────────────────────┘    └─────────────────┘
        ▲                         ▲                         ▲
        │        MCP / AGENTS.md / Skills : le socle commun  │
        └─────────────────────────┴─────────────────────────┘
```

Repère simple : **terminal = scriptable et CI-friendly**, **éditeur = confort
visuel et revues de diff**, **OpenClaw = agent qui vit en tâche de fond** et
que tu pilotes depuis ton téléphone.

## 5. Prérequis communs avant d'installer quoi que ce soit

Checklist à cocher une fois pour toutes :

- [ ] **Git installé et configuré** (`git --version` ≥ 2.40). Tous les agents
      travaillent mieux — et plus sûrement — dans un dépôt git.
- [ ] **Node.js 20+** (22+ recommandé pour Codex CLI) si tu installes via npm.
- [ ] **Un dossier de laboratoire** : `mkdir -p /tmp/lab-agent && cd /tmp/lab-agent && git init`.
- [ ] **Une clé API ou un abonnement** selon l'outil (détaillé par outil).
- [ ] **`.gitignore` correct** : aucun `.env`, aucune clé, aucun `id_rsa` ne
      doit traîner dans un dossier où un agent va lire/écrire.
- [ ] **Sauvegarde** : sur un vrai projet, commit propre AVANT de lancer un
      agent. `git status` doit être vide. C'est ton bouton « annuler ».

```bash
# Hygiène de départ : vérifie ce que tu as
git --version && node --version && npm --version && python3 --version

# Crée ton bac à sable
mkdir -p /tmp/lab-agent && cd /tmp/lab-agent && git init -q
cat > .gitignore <<'EOF'
.env
*.pem
*.key
id_rsa*
__pycache__/
node_modules/
EOF
git add .gitignore && git commit -qm "init lab"
echo "bac à sable prêt : $(pwd)"
```

## 6. Le fichier AGENTS.md : ton contrat avec tous les agents

Presque tous les outils de ce guide lisent `AGENTS.md` à la racine du projet
(Claude Code lit aussi `CLAUDE.md`, Kilo Code lit `AGENTS.md` en priorité).
C'est le fichier le plus rentable de tout ton outillage : 30 lignes bien
écrites valent des centaines d'euros de tokens gaspillés.

```markdown
# AGENTS.md — projet: inventaire-parc

## Contexte
App interne de gestion du parc (copieurs, onduleurs). Python 3.12, FastAPI,
PostgreSQL 16. Déploiement sur Debian 12 via Ansible.

## Commandes (à utiliser telles quelles)
- Installer : `pip install -r requirements.txt`
- Tests : `pytest -q`  (doivent passer avant tout commit)
- Lint : `ruff check . && ruff format --check .`
- Lancer en dev : `uvicorn app.main:app --reload`

## Conventions (obligatoires)
- Français pour les messages de commit et la doc, anglais pour le code.
- Pas de nouvelle dépendance sans validation humaine explicite.
- Les secrets passent par variables d'environnement, jamais en dur.
- Schéma SQL : toute migration via Alembic, jamais de SQL à la main en prod.

## Interdictions formelles
- Ne JAMAIS modifier `migrations/` déjà appliquées.
- Ne JAMAIS lancer `pytest` avec `--lf` seul : toujours la suite complète.
- Ne pas toucher à `legacy/` (code gelé, lecture seule).
```

Règles de rédaction :

1. **Impératif, court, vérifiable.** « Les tests doivent passer » > un
   paragraphe sur la qualité.
2. **Commandes copiables.** L'agent exécute bêtement ce que tu écris.
3. **Interdictions explicites.** L'agent ne devine pas tes tabous.
4. **Un AGENTS.md par périmètre** : racine (général) + sous-dossiers
   (`backend/AGENTS.md`, `ansible/AGENTS.md`) pour les règles locales.
5. **Relis-le tous les mois** : un AGENTS.md périmé fait faire n'importe quoi
   très efficacement.

> 💡 Pour ton RAG : un `AGENTS.md` versionné dans chaque repo documentaire te
> servira aussi de contrat quand tu demanderas à un agent de nettoyer ou
> d'indexer tes corpus.

## 7. Grille de lecture des fiches outils

Chaque outil est présenté avec le même plan pour comparer vite :

1. **C'est quoi / pour qui** — en deux phrases.
2. **Installation pas à pas** — commandes réelles, vérifiées sept. 2026.
3. **Premier workflow commenté** — un scénario complet, du prompt au résultat.
4. **Points forts / faiblesses honnêtes** — ce que la doc marketing ne dit pas.
5. **Tarifs vérifiés** — avec la mention « à vérifier » quand ça bouge.
6. **Verdict Zelef** — quand l'outil a du sens pour un sysadmin qui construit
   un RAG (avis assumé, pas sponsorisé).

## 8. Avertissement sécurité (à lire avant la première installation)

Un agent codeur, par construction :

- **lit tes fichiers** (y compris ceux que tu as oubliés : `.env`, dumps,
  clés) ;
- **exécute des commandes** (avec tes droits utilisateur, parfois sudo si tu
  le laisses faire) ;
- **envoie du contenu à un serveur distant** (le modèle) — ton code voyage.

Donc, avant d'installer :

1. 🔒 **Ne mets jamais de secret** dans un dossier de travail d'un agent
   (ni `.env` réel, ni clé API en dur, ni mot de passe). Utilise des `.env.example`.
2. 🔒 **Ne lance pas un agent en root** ni avec un utilisateur qui a les clés
   de la prod.
3. 🔒 **Vérifie le mode réseau** : certains agents ont un mode sandbox sans
   réseau ; active-le quand tu n'en as pas besoin.
4. 🔒 **Lis la politique de données** du fournisseur : ton code peut servir à
   entraîner des modèles (désactivable chez la plupart, mais pas partout —
   voir section 99).

La section 99 détaille tout ça. Si tu ne retiens qu'une phrase : **un agent
est un stagiaire brillant, rapide, et sans aucun sens des conséquences —
tu restes le responsable.**

---

# PARTIE A — LES CLI AGENTIQUES

---

## 9. OpenCode : c'est quoi, pour qui

**OpenCode** (opencode.ai) est un agent codeur **open source (MIT)**,
**agnostique du modèle** : au lieu d'être marié à un seul fournisseur
(Anthropic pour Claude Code, OpenAI pour Codex), il se branche sur **n'importe
quel fournisseur** via clé API — OpenAI, Anthropic, Google, OpenRouter,
Ollama en local, etc.

Pour qui :

- Tu veux **un seul outil** et changer de modèle selon la tâche (un pas cher
  pour explorer, un costaud pour coder).
- Tu veux du **100 % local** possible (Ollama) pour du code sensible.
- Tu refuses le verrouillage fournisseur et tu aimes l'open source
  (le projet dépasse les 95 000 stars GitHub — ordre de grandeur : énorme,
  très actif).
- Tu vis dans le terminal (TUI intégrée) mais tu veux aussi des plugins
  Neovim/VS Code.

Ce n'est PAS l'idéal si tu veux du « zéro configuration » : le choix du
modèle, c'est la liberté ET la responsabilité (qualité très variable selon le
modèle branché).

## 10. OpenCode : installation pas à pas

Prérequis : macOS, Linux ou Windows avec un terminal. Node.js utile mais le
script officiel gère tout.

```bash
# Méthode officielle (macOS / Linux) — vérifiée sept. 2026
curl -fsSL https://opencode.ai/install | bash

# Alternatives équivalentes
npm install -g opencode-ai
# ou : brew install opencode-ai
# Windows : npm install -g opencode-ai

# Vérifier
opencode --version
```

Premier lancement dans ton bac à sable :

```bash
cd /tmp/lab-agent
opencode
```

Au premier démarrage, OpenCode propose de **connecter un fournisseur** :

- **Option simple pour tester** : **OpenCode Zen**, l'offre hébergée
  d'OpenCode (démarrage gratuit selon la doc — à vérifier sur
  opencode.ai/docs, l'offre évolue).
- **Option BYOK** : dans la TUI, commande `/connect`, choisis le fournisseur
  (Anthropic, OpenAI, OpenRouter…) et colle ta clé API.
- **Option locale** : branche Ollama (`http://localhost:11434`) — aucun
  token facturé, qualité dépendant du modèle local.

La clé est stockée dans la config locale (`~/.config/opencode/`), pas dans
le projet. Ne la commite jamais.

Mettre à jour (le projet publie très souvent) :

```bash
# Même commande que l'installation, ou via le gestionnaire utilisé
curl -fsSL https://opencode.ai/install | bash
opencode --version
```

## 11. OpenCode : prise en main de la TUI

La TUI (Text User Interface) est le cœur de l'expérience :

| Touche / commande | Effet |
|---|---|
| `Tab` | Bascule entre l'agent **Plan** (lecture seule) et **Build** (écriture) |
| `@` | Référence un fichier/dossier dans ton prompt (`@app/main.py`) |
| `/init` | Génère un `AGENTS.md` à partir de l'analyse du projet |
| `/connect` | Connecter/configurer un fournisseur de modèle |
| `/login` | Connexion OAuth quand le fournisseur le propose |
| `!` | Préfixe pour lancer une commande shell directement |
| `opencode run "prompt"` | Mode non interactif (scriptable, CI) |
| `opencode run -m <modèle> "prompt"` | Choisir le modèle pour un run |

Exemple de choix de modèle par tâche (à adapter selon tes clés) :

```bash
# Exploration / questions : modèle rapide et pas cher
opencode run -m openrouter/qwen/qwen3-coder-flash "liste les modules et leurs dépendances"

# Implémentation sérieuse : modèle costaud
opencode run -m anthropic/claude-sonnet-4-6 "implémente la fonction dans @app/main.py"
```

> ⚠️ Les identifiants de modèles changent vite (les fournisseurs renomment).
> Liste ce qui est réellement disponible avec ta config avant de copier un
> nom de modèle d'un tuto de 3 mois.

## 12. OpenCode : premier workflow commenté

Scénario : tu veux qu'OpenCode **documente** ton bac à sable puis ajoute une
fonction utilitaire testée. On reste en mode Plan d'abord.

```bash
cd /tmp/lab-agent
opencode
```

Dans la TUI :

```
# 1. Contextualiser le projet (génère AGENTS.md)
> /init
# → OpenCode explore le repo et propose un AGENTS.md. RELIS-LE avant d'accepter :
#    corrige les commandes de test, ajoute tes interdictions (section 6).

# 2. Passer en mode Plan (Tab) pour une exploration sans écriture
> [Tab pour passer en Plan] @. explique l'architecture de ce projet en 10 lignes,
>   sans modifier aucun fichier

# 3. Demander un plan d'implémentation avant d'écrire
> propose un plan pour ajouter une fonction ping_hote(hote) avec retry,
>   sans écrire de code pour l'instant

# 4. Si le plan te convient : Tab pour passer en Build, puis
> implémente le plan validé, ajoute un test pytest, lance les tests
```

Ce que tu observes : l'agent lit les fichiers (`@`), propose, écrit, lance
`pytest`, corrige. **Tu valides chaque étape sensible** : c'est le workflow
« plan d'abord » qui évite 80 % des bêtises.

En mode non interactif (pour un script cron, par ex. génération de doc
nuit) :

```bash
opencode run "génère docs/API.md à partir des docstrings de app/, ne modifie rien d'autre" \
  && git status --porcelain
# Si git status montre des fichiers inattendus → git checkout -- . pour annuler
```

## 13. OpenCode : modèles supportés et configuration

Fichier de config : `~/.config/opencode/opencode.json` (global) ou
`opencode.json` à la racine du projet. Exemple réel :

```json
{
  "$schema": "https://opencode.ai/config.json",
  "agent": {
    "build": { "model": "openrouter/qwen/qwen3-coder-flash" },
    "plan":  { "model": "openrouter/qwen/qwen3-coder-flash" }
  },
  "mcp": {
    "filesystem": {
      "type": "local",
      "command": ["npx", "-y", "@modelcontextprotocol/server-filesystem", "/data/projets"]
    }
  }
}
```

Points clés :

- **Modèles différents par agent** : un modèle pas cher en Plan (exploration),
  un costaud en Build. C'est le levier n°1 d'économie.
- **Fournisseurs** : toute clé compatible OpenAI/Anthropic, OpenRouter
  (agrégateur, pratique pour tester plein de modèles avec une seule clé),
  Ollama local.
- **OpenCode Zen** : l'offre « modèles testés et curatés » d'OpenCode, avec
  un modèle gratuit intégré pour démarrer sans clé (les offres gratuites
  tournent — à vérifier sur opencode.ai/docs).
- **MCP natif** : serveurs MCP déclarés dans la config, outils disponibles
  dans la TUI.
- **Plugins** : écosystème de plugins (hooks, skills) documenté sur
  opencode.ai/docs/plugins.

## 14. OpenCode : prix (vérifiés sept. 2026)

| Poste | Coût |
|---|---|
| Le logiciel | **0 €** — MIT, gratuit, à vie |
| OpenCode Zen / offre hébergée | Formule autour de **~10 $/mois** citée dans les comparatifs (à vérifier sur opencode.ai — l'offre a évolué en 2026) |
| BYOK (ta clé API) | Tu paies le fournisseur au token : ex. quelques $ pour des sessions légères, 20–50 $/mois en usage intensif sur modèles frontier |
| 100 % local (Ollama) | 0 € de tokens, coût = ton électricité et ton GPU |

Règle pratique : **en BYOK avec un modèle « flash/mini » pour le plan et un
gros modèle pour le build, un mois d'usage régulier coûte souvent moins cher
qu'un abonnement fixe à 20 $** — mais sans plafond : mets une alerte de
facturation chez ton fournisseur (section 98).

## 15. OpenCode : points forts / faiblesses honnêtes

**Points forts**

- Liberté totale de modèle : tu n'es marié à personne, tu arbitres
  qualité/prix/confidentialité par tâche.
- Open source MIT : auditable, auto-hébergeable, énorme communauté
  (plugins, skills, docs communautaires).
- TUI soignée + `opencode run` scriptable : aussi à l'aise en interactif
  qu'en cron/CI.
- Plan/Build via `Tab` : le garde-fou est dans le geste, pas dans la doc.
- `/init` → `AGENTS.md` : l'onboarding d'un repo existant prend 2 minutes.

**Faiblesses honnêtes**

- **La qualité dépend du modèle que TU choisis.** Un mauvais modèle =
  un mauvais agent, et OpenCode ne te protège pas de ton propre choix.
- Jeune projet qui bouge très vite : commandes, config et offres changent
  entre deux versions — la doc de 6 mois est déjà périmée.
- Pas de « tout inclus » : pas d'indexation codebase magique offerte, pas de
  cloud agent clé en main comme Cursor.
- L'écosystème plugins/skills est riche mais inégal : à trier.

## 16. OpenCode : bonnes pratiques

1. **Fige tes modèles dans `opencode.json` versionné** : toute l'équipe (ou
   toi dans 3 mois) rejoue avec les mêmes modèles.
2. **Sépare plan/build par modèle** (section 13) : l'économie la plus simple.
3. **Toujours `/init` sur un repo existant**, puis corrige le AGENTS.md généré
   (il invente parfois des commandes de test).
4. **En CI** : `opencode run` avec un modèle fixe, prompt versionné, et
   `git diff --exit-code` après pour détecter les changements inattendus.
5. **MCP avec parcimonie** : chaque serveur MCP ajoute des outils donc des
   tokens à chaque tour. Ne branche que ce qui sert la tâche.

## 17. Kilo Code : c'est quoi, pour qui

**Kilo Code** (kilo.ai) est un agent codeur **open source (MIT)** né en mars
2025 comme **fork de Roo Code** (lui-même dérivé de Cline). En avril 2026, il
a été **reconstruit sur la base d'OpenCode** : le même agent tourne désormais
dans **VS Code, JetBrains et en CLI**, avec une config partagée.

Repères (vérifiés sept. 2026) :

- **Éditeur** : Kilo-Org ; **rachat par Anaconda en juillet 2026**.
- **Adoption** : >1,5 million d'utilisateurs revendiqués, l'un des agents
  open source qui croît le plus vite ; point d'atterrissage naturel des
  utilisateurs de **Roo Code (archivé en mai 2026)**.
- **Modèle éco** : agent gratuit et open source ; tu paies l'usage modèle
  (BYOK sans marge, ou crédits prépayés « Kilo Gateway », ou abonnement
  **Kilo Pass à partir de ~19 $/mois** — à vérifier sur kilo.ai/pricing).
- **Particularités** : modes personnalisés (Architect, Code, Debug, Ask
  hérités de Roo), **autocomplétion inline** (rare pour un agent VS Code),
  **Agent Manager** (sessions parallèles isolées dans des git worktrees
  séparés), marketplace MCP intégré, routage automatique de modèles
  (planification/codage/debug vers le bon modèle selon le palier choisi).

Pour qui : tu veux **un seul agent open source sur tous tes environnements**
(éditeur + terminal), avec une vraie gestion de sessions parallèles, sans
t'enfermer chez un fournisseur.

## 18. Kilo Code : installation pas à pas

**Extension VS Code / JetBrains :**

1. Ouvre le marketplace d'extensions (VS Code : `Ctrl+Shift+X`).
2. Cherche **« Kilo Code »** (éditeur : Kilo Code / Kilo-Org).
3. Installe, puis ouvre la vue Kilo Code dans la barre latérale.
4. Crée un compte ou branche ta clé : au choix **BYOK** (tes clés, sans
   marge), **Kilo Gateway** (crédits prépayés), ou **Kilo Pass** (abonnement).

**CLI (méthode documentée : npm) :**

```bash
npm install -g @kilocode/cli
kilo --version

# Dans un projet
cd /tmp/lab-agent
kilo
```

> La doc officielle du CLI ne documente que la méthode npm (des chemins
> curl/brew sont évoqués sur le site marketing mais non confirmés dans la
> doc — à vérifier sur kilo.ai/docs si tu préfères une autre méthode).

**Config partagée** : depuis la reconstruction sur OpenCode, la config vit
dans un **`kilo.jsonc`** central + dossiers **`.kilo/`** (les anciens
`.kilocode/` sont migrés automatiquement). L'agent lit **`AGENTS.md`**
(en cascade racine → sous-dossiers), plus les skills standard
`.claude/skills/` et `.agents/skills/`.

> ⚠️ La config exacte évolue vite depuis la reconstruction d'avril 2026 :
> pars de `kilo.ai/docs`, pas d'un tuto.

## 19. Kilo Code : premier workflow commenté (modes + Agent Manager)

Scénario : corriger un bug dans un script, en utilisant les **modes** comme
les anciens utilisateurs de Roo Code les aiment.

```
1. Dans VS Code, ouvre la vue Kilo Code, choisis le mode **Architect**.
2. Prompt : « Analyse @scripts/sauvegarde.py : la rotation des archives
   supprime parfois la dernière sauvegarde valide. Propose un plan de
   correction, sans écrire de code. »
   → Architect explore en lecture seule et rend un plan numéroté.
3. Relis le plan. S'il est bon, bascule en mode **Code**.
4. Prompt : « Applique le plan validé ci-dessus. »
   → Kilo édite, avec diff visible dans l'éditeur.
5. Bascule en mode **Ask** pour faire expliquer : « Explique-moi la condition
   limite que tu as corrigée, avec un exemple. »
```

**Agent Manager (sessions parallèles)** — le vrai différenciateur :

```
1. Dans la vue Kilo Code : « New session via Agent Manager ».
2. Kilo crée un **git worktree** séparé par session
   (ex. /tmp/lab-agent/.kilo/worktrees/fix-rotation/).
3. Lance 2 sessions en parallèle : l'une corrige le bug, l'autre écrit
   la doc — elles ne se marchent pas dessus car le worktree isole
   le working directory.
4. À la fin : revue des diffs, merge du worktree retenu, suppression des autres.
```

En CLI, le même réflexe :

```bash
cd /tmp/lab-agent
kilo "en mode plan : liste les risques de la fonction de rotation, sans modifier de fichier"
```

## 20. Kilo Code : prix, forces, faiblesses, verdict

**Tarifs (vérifiés sept. 2026, à re-vérifier sur kilo.ai/pricing) :**

| Poste | Coût |
|---|---|
| Agent (VS Code / JetBrains / CLI) | **0 €** — MIT |
| BYOK | 0 € de marge Kilo, tu paies ton fournisseur au token |
| Kilo Gateway | Crédits prépayés, tarifs = tarifs fournisseurs sans marge (annoncé) |
| Kilo Pass | Abonnement **à partir de ~19 $/mois** |
| Offre gratuite | Palier gratuit avec modèles « free » routés automatiquement (liste tournante, **à vérifier** : les modèles gratuits changent vite et les prompts gratuits peuvent servir à l'entraînement — voir section 99) |
| Teams | **~15 $/utilisateur/mois** (annoncé) |

**Points forts** : un agent, trois surfaces avec la même config ; modes
éprouvés (Architect/Code/Debug/Ask) ; Agent Manager/worktrees pour le
parallélisme réel ; inline autocomplete intégrée ; 500+ modèles, zéro marge
en BYOK ; lecture d'AGENTS.md + skills standards (interopérable avec tes
autres outils).

**Faiblesses honnêtes** : jeune (2025) et déjà reconstruit une fois — la doc
et les chemins de config ont changé en avril 2026 ; l'écosystème est plus
petit que Claude Code/Cursor ; le rachat par Anaconda (juil. 2026) pose la
question classique : quelle indépendance à 2 ans ? Le routage auto de modèles
est pratique mais opaque : vérifie ce qui a réellement tourné si tu dois
justifier un coût.

**Verdict Zelef** : excellent **deuxième agent** après un CLI de référence.
Si tu vis dans VS Code et que tu veux de l'open source sans abonnement
obligatoire, c'est le candidat n°1. Pour du 100 % terminal scripté,
OpenCode/Claude Code restent plus simples.

## 21. Claude Code : c'est quoi, pour qui

**Claude Code** est l'agent codeur officiel d'**Anthropic**. C'est la
référence du marché en 2026 : celui contre lequel tout le monde se compare.
Il ne tourne **que sur les modèles Claude** (Sonnet/Opus/Haiku) — c'est à la
fois sa force (le harnais est co-développé avec le modèle) et sa limite
(verrouillage Anthropic total).

Cinq surfaces, un seul moteur (vérifié juil.–sept. 2026) :

- **Terminal** : `cd ton-projet && claude` — la surface la plus complète.
- **Extension VS Code** (fonctionne aussi dans Cursor) : diffs inline,
  `@`-mentions, revue de plan.
- **Plugin JetBrains** (IntelliJ, PyCharm, WebStorm ; nécessite le CLI à côté).
- **App desktop** (macOS/Windows) : revue visuelle des diffs, sessions
  côte à côte, tâches planifiées.
- **Web** : sessions cloud sans installation locale, récupérables dans le
  terminal via `claude --teleport`.

Pour qui : tu veux **l'agent le plus abouti** et tu acceptes l'écosystème
Anthropic (abonnement ou API). Le choix par défaut pour les longues sessions
autonomes (30+ min) : Sonnet/Opus tiennent le cap mieux que la concurrence
sur les tâches longues, d'après les retours terrain 2026.

## 22. Claude Code : installation pas à pas

```bash
# Méthode officielle — vérifiée sept. 2026
npm install -g @anthropic-ai/claude-code
# (un installeur curl existe aussi : https://claude.ai/install.sh — à vérifier)

# Vérifier
claude --version

# Premier lancement (dans ton bac à sable !)
cd /tmp/lab-agent
claude
```

À l'invite de connexion, deux routes :

**Route A — abonnement Claude (le plus simple) :**

```
/login
```
→ ouvre le navigateur, connecte ton compte Claude **Pro (20 $/mois)** ou
**Max (100 ou 200 $/mois)**. Le plan gratuit **n'inclut pas** Claude Code
(vérifié le 10 sept. 2026).

**Route B — clé API (paiement au token) :**

```bash
export ANTHROPIC_API_KEY="sk-ant-ta-cle-fictive-ici"
claude
```

> 🔒 Ne mets jamais ta vraie clé dans un fichier versionné. `export` dans ton
> `~/.bashrc`/`~/.zshrc`, ou gestionnaire de secrets.

Vérifier que ça marche :

```
> résume en 5 lignes ce que contient ce dossier : @.
```

## 23. Claude Code : commandes essentielles

| Commande | Effet |
|---|---|
| `claude` | Lance la session interactive dans le dossier courant |
| `claude "prompt"` | Exécute un prompt directement (une fois) |
| `claude --teleport` | Récupère une session cloud dans ton terminal |
| `/login`, `/logout` | (Re)connexion au compte / déconnexion |
| `/init` | Génère `CLAUDE.md` à partir du projet |
| `/agents` | Gérer les subagents personnalisés |
| `/skills` | Lister/gérer les skills (`.claude/skills/`) |
| `/mcp` | Gérer les serveurs MCP |
| `/hooks` | Configurer les hooks (ex. : vérif auto après écriture) |
| `/status` | État session, modèle, conso |
| `/cost` | Coût estimé de la session (utile en API !) |
| `/compact` | Résume la session pour libérer du contexte |
| `/clear` | Nouvelle conversation, même dossier |
| `Shift+Tab` | Bascule en **Plan mode** (lit, propose, n'écrit pas) |
| `Esc` | Interrompt l'agent en cours (ton frein d'urgence) |
| `--dangerously-skip-permissions` | ⚠️ Désactive les demandes de permission — **à ne jamais utiliser** sauf sandbox jetable |

Fichier mémoire : **`CLAUDE.md`** à la racine (généré par `/init`), relu à
chaque session. Claude Code lit aussi **`AGENTS.md`** — tu peux n'en
maintenir qu'un et le dupliquer/symlinker selon l'outil dominant de l'équipe.

```bash
# Astuce : un seul fichier source de vérité
ln -s AGENTS.md CLAUDE.md
```

## 24. Claude Code : premier workflow commenté

Scénario : ajouter une commande `inventaire` à un CLI Python, **méthode
plan d'abord**.

```bash
cd /tmp/lab-agent
claude
```

```
# 1. Cadrage du projet (une seule fois par repo)
> /init
# → génère CLAUDE.md. CORRIGE-LE : commandes de test, interdictions (cf. section 6).

# 2. Plan mode : Shift+Tab, puis
> [Plan mode] Voici le besoin : ajouter une sous-commande `inventaire`
> qui liste les équipements depuis un CSV. Explore le code existant et
> propose un plan détaillé (fichiers touchés, structure, tests).
> N'écris aucun code pour l'instant.

# 3. Lis le plan. Demande des ajustements si besoin :
> ton plan prévoit de modifier cli.py directement ; préfère un nouveau
> module inventaire.py importé dans cli.py. Mets à jour le plan.

# 4. Sors du plan mode (Shift+Tab), puis exécution :
> implémente le plan validé. Après chaque fichier modifié, lance
> `pytest -q` et corrige jusqu'au vert.

# 5. Contrôle : /cost pour voir ce que la session a coûté (route API),
#    git diff pour relire chaque ligne avant commit.
```

Réflexes à garder :

- **`Esc`** dès que l'agent part dans une direction bizarre — corriger tôt
  coûte 10× moins cher que corriger tard.
- **`/compact`** quand la session dépasse ~30 min : résume le contexte,
  évite la dérive.
- **Un commit git avant chaque grosse étape** : `git diff` reste le meilleur
  outil de revue d'un agent.

## 25. Claude Code : subagents, skills, hooks, MCP

C'est ici que Claude Code se distingue pour un usage avancé.

**Subagents** (`.claude/agents/`) : des agents spécialisés avec leur propre
prompt système et leurs propres outils autorisés.

```markdown
# .claude/agents/reviewer.md
---
name: reviewer
description: Relit un diff et rend un verdict bloquant/non-bloquant
tools: Read, Grep, Glob   # pas d'écriture, pas de shell
---
Tu es un relecteur exigeant. On te donne un diff.
Rends : 1) verdict BLOQUANT ou OK, 2) la liste des problèmes par sévérité,
3) pour chaque problème bloquant, le patch exact proposé.
Ne modifie jamais de fichier toi-même.
```

Usage : `> utilise le subagent reviewer sur le diff de la branche feature/inventaire`.

**Skills** (`.claude/skills/`) : savoir-faire réutilisables, invoqués par
`/skill` ou automatiquement.

```bash
.claude/skills/ansible-review/SKILL.md   # ta checklist de relecture de playbooks
.claude/skills/rag-ingest/SKILL.md       # ta procédure d'ingestion RAG
```

**Hooks** : scripts shell déclenchés par événements (après écriture d'un
fichier, avant un commit…). Exemple : relancer le lint après chaque
modification Python.

```json
// ~/.claude/settings.json — extrait (schéma indicatif, vérifier la doc)
{
  "hooks": {
    "PostToolUse": [
      { "matcher": "Edit|Write", "command": "ruff check ${file} || true" }
    ]
  }
}
```

**MCP** : Claude Code est **client ET serveur** MCP — le support le plus
mature du marché en 2026.

```bash
# Ajouter un serveur MCP (ex. : filesystem sur ton dossier de docs)
claude mcp add docs -- npx -y @modelcontextprotocol/server-filesystem ~/workspace/user/files
# Lister
claude mcp list
```

> 💡 Pour ton RAG : un serveur MCP « docs » branché sur ton corpus permet à
> l'agent de citer tes propres guides pendant qu'il code. C'est le pont entre
> tes deux chantiers.

## 26. Claude Code : bonnes pratiques (terrain)

1. **Toujours `/init` + relecture du CLAUDE.md** sur un nouveau repo.
2. **Plan mode par défaut** pour toute tâche > 10 minutes. Le réflexe
   `Shift+Tab` doit devenir un automatisme.
3. **Prompts avec critères d'arrêt** : « …et arrête-toi dès que les tests
   passent, résume ce que tu as changé » — sinon l'agent « améliore »
   indéfiniment (et facture).
4. **Donne les chemins, pas des descriptions vagues** : `@app/main.py:120`
   bat « la fonction qui gère le login ».
5. **Interdis explicitement** dans CLAUDE.md : pas de `rm -rf`, pas de
   modification des migrations, pas de nouvelle dépendance sans validation.
6. **Revue systématique** : `git diff` + subagent `reviewer` avant chaque
   commit (section 100).
7. **Sessions courtes** : au-delà de 45–60 min, `/compact` ou nouvelle
   session avec un résumé — la qualité dérive avec la longueur.
8. **Ne colle jamais de secrets dans le prompt** : ils partent chez Anthropic
   et restent dans l'historique de session (`~/.claude/`).

## 27. Claude Code : prix (vérifiés sept. 2026)

| Route | Prix | Notes |
|---|---|---|
| Claude **Pro** | **20 $/mois** (17 $/mois en annuel) | Claude Code **inclus**, usage « raisonnable » |
| Claude **Max** | **100 $/mois** (5× l'usage Pro) ou **200 $/mois** (20×) | Pour usage intensif ; Code + chat partagent le quota |
| Plan gratuit | 0 € | **N'inclut PAS** Claude Code (vérifié 10/09/2026) |
| **API** (clé) | Au token, ex. **Sonnet 4.6 : 3 $ / 15 $ par million de tokens** (input/output) ; cache : ~3,75 $ écriture / 0,30 $ lecture | Tarifs officiels Anthropic ; les familles Opus/Haiku ont leurs propres grilles — **à vérifier** sur platform.claude.com/docs/en/about-claude/pricing |

Repères de coût en API (ordres de grandeur 2026) :

- Petite session (question + 2–3 fichiers) : **quelques centimes**.
- Feature moyenne avec tests : **0,50–3 $**.
- Grosse session autonome d'1 h sur Sonnet : **5–15 $**.
- Sur Opus : **3–5× plus cher** que Sonnet.

> 💰 `/cost` en cours de session + alertes de facturation sur la console
> Anthropic : les deux garde-fous à activer dès la route API.

## 28. Claude Code : points forts / faiblesses honnêtes

**Points forts**

- L'agent le plus **poli et collaboratif** : il pose des questions, admet
  l'incertitude, conteste un besoin flou — comportement de « pair programmer ».
- Tient les **longues sessions** mieux que la concurrence (contexte 1M+
  tokens, moins de dérive).
- Écosystème MCP le plus mature (client + serveur), skills, hooks, subagents.
- Cinq surfaces avec le même moteur (terminal, VS Code, JetBrains, desktop,
  web + `--teleport`).
- Doc et communauté énormes : un problème = quelqu'un l'a déjà eu.

**Faiblesses honnêtes**

- **Verrouillage Anthropic total** : un seul fournisseur, un seul jeu de
  modèles. Panne ou hausse de prix = pas de plan B dans l'outil.
- Le CLI est **fermé** (pas open source) — tu ne peux pas l'auditer ni le
  forker.
- En abonnement, les limites d'usage (« fair use ») sont floues et ont
  changé plusieurs fois en 2026 : un mois tu es large, le suivant tu es
  bridé.
- Tendance à la **verbosité** : il explique beaucoup, ça coûte des tokens
  de sortie. À cadrer (« sois concis » dans CLAUDE.md).
- Ton code transite par les serveurs d'Anthropic : à intégrer dans ton
  analyse de confidentialité (section 99).

## 29. Claude Code : usage en équipe (conventions)

Pour que 3–5 personnes utilisent Claude Code sans chaos :

```bash
# À versionner dans chaque repo
CLAUDE.md                  # ou AGENTS.md + symlink
.claude/
  agents/
    reviewer.md            # relecteur bloquant partagé
    test-writer.md         # génère les tests selon VOS standards
  skills/
    commit-msg/SKILL.md    # format de messages de commit de l'équipe
  settings.json            # hooks partagés (lint auto après écriture)
```

Conventions à écrire noir sur blanc :

1. **Modèle par défaut d'équipe** (ex. : Sonnet pour le quotidien, Opus
   uniquement sur demande motivée — question de coût).
2. **Plan mode obligatoire** au-delà d'une estimation de 15 min de travail.
3. **Aucun commit généré sans `git diff` relu par un humain** (section 100).
4. **Skills d'équipe > prompts individuels** : quand deux personnes
   réécrivent le même long prompt, c'en est un skill.
5. **Journal des sessions** : un `docs/decisions/` où l'agent résume ce qu'il
   a fait et pourquoi (prompté systématiquement en fin de feature).

## 30. Claude Code : verdict Zelef

**Si tu ne dois en choisir qu'un pour commencer : c'est celui-là.** Le plus
abouti, le mieux documenté, le plus prévisible en équipe. Prends la **route
abonnement Pro** si ton usage est régulier (facture fixe, zéro stress), la
**route API** si tu veux contrôler au token près ou brancher d'autres outils
sur la même clé. Pour ton RAG : le serveur MCP + les skills en font le
meilleur « collègue » pour nettoyer/indexer ton corpus documentaire.
Seule vraie réserve : la dépendance à un seul fournisseur — d'où l'intérêt
de garder OpenCode en second pour le jour où tu veux comparer ou basculer.

## 31. Codex (CLI) : c'est quoi, pour qui

**Codex CLI** est l'agent terminal officiel d'**OpenAI** (dépôt open source
**Apache 2.0** : github.com/openai/codex). Comme Claude Code, il lit/édite/
teste ton repo en local ; il tourne sur les modèles **GPT** (GPT-5 par
défaut en 2026).

Positionnement honnête : c'est **le pendant OpenAI de Claude Code**. Mêmes
gestes, philosophie légèrement différente — plus « exécutant » que
« pair collaboratif » : il fonce davantage, demande moins. D'où l'importance
de bien régler ses modes de permission.

Pour qui :

- Tu es déjà abonné **ChatGPT Plus/Pro** : Codex CLI est **inclus**, sans
  surcoût — c'est l'argument massue.
- Tu veux un agent **open source** adossé à OpenAI (auditable, forkable,
  contrairement au CLI fermé de Claude Code).
- Tu veux du **scriptable en CI** : `codex exec` est taillé pour ça.
- Tu travailles dans l'écosystème OpenAI (ou tu veux comparer GPT-5 vs
  Claude sur les mêmes tâches).

## 32. Codex CLI : installation pas à pas

```bash
# Méthode officielle (macOS / Linux) — vérifiée sept. 2026
curl -fsSL https://chatgpt.com/codex/install.sh | sh

# Alternatives
npm install -g @openai/codex
# macOS : brew install --cask codex
# Windows (PowerShell) :
#   powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"
#   (support natif Windows depuis mai 2026 — plus besoin de WSL)

# Vérifier
codex --version
```

Prérequis : **Node.js 22+** pour l'install npm. Sur Windows frais, installe
aussi le **Visual C++ Redistributable** (sinon Codex refuse de démarrer
silencieusement) et **Git for Windows**.

**Connexion — deux routes :**

```bash
cd /tmp/lab-agent
codex
# → choisis « Sign in with ChatGPT » : le navigateur s'ouvre,
#   le token revient dans le terminal.
#   Le login est mis en cache dans ~/.codex/auth.json (fichier à
#   traiter comme un mot de passe !) ou le trousseau de l'OS.

# Alternative : clé API (utile en CI, facturation au token)
printenv OPENAI_API_KEY | codex login --with-api-key

# Machine distante sans navigateur :
codex login --device-auth
# → code à usage unique à saisir ailleurs (bêta, à activer dans les
#   paramètres de sécurité ChatGPT)

# Vérifier l'état
codex login status
```

**Mise à jour** (le CLI évolue vite : ~1 release/semaine en sept. 2026) :

```bash
# Selon ta méthode d'install :
npm install -g @openai/codex
# ou : brew upgrade --cask codex
# ou : codex update   (auto-update si ta version le supporte)
```

## 33. Codex CLI : commandes et modes de permission

```bash
codex                          # session interactive dans le dossier courant
codex "ton prompt"             # exécute un prompt directement
codex exec "prompt"            # mode NON interactif (scripts, CI, cron)
codex exec --json --full-auto "prompt"   # sortie JSON, sans demander
codex login / codex login status
codex update
```

**Les trois modes de permission** — le réglage le plus important :

| Mode | Comportement |
|---|---|
| **Suggest** | L'agent propose, **tu valides chaque action** (défaut prudent) |
| **Auto-edit** | Il édite les fichiers seul, **demande pour exécuter** des commandes |
| **Full-auto** | Il fait tout seul (édition + exécution) — sandboxé, mais tout seul |

```bash
# Choisir le mode au lancement (exemples)
codex --ask-for-approval on-request "prompt"     # proche de Suggest
codex --ask-for-approval on-failure "prompt"
codex --ask-for-approval never "prompt"          # proche de Full-auto ⚠️
```

> 🔒 **Règle** : `Suggest` pour découvrir, `Auto-edit` au quotidien,
> `Full-auto` uniquement dans un bac à sable jetable ou en CI avec un
> périmètre verrouillé (section 99). Le mode Full-auto + un prompt flou =
> la recette classique du « il a tout réécrit ».

**Plan mode** : commande `/plan` dans la session interactive — même
philosophie que les autres : explorer et proposer sans écrire.

Fichier mémoire : **`AGENTS.md`** (le standard multi-outils — pas de fichier
propriétaire chez Codex, bon point d'interopérabilité).

## 34. Codex CLI : premier workflow commenté

Scénario : écrire un script de **vérification d'onduleur** (ton métier !)
avec tests, en mode `codex exec` pour voir la voie scriptable.

```bash
cd /tmp/lab-agent
cat > AGENTS.md <<'EOF'
# AGENTS.md — lab-agent
- Python 3.12. Tests : `pytest -q` (doivent passer).
- Pas de dépendance externe sans validation humaine.
- Ne jamais écrire de secrets en dur ; exemples fictifs uniquement.
EOF

# 1. D'abord en interactif, en plan :
codex
```

```
> /plan
> Besoin : script check_ups.py qui interroge un onduleur en SNMP (hostname,
> communauté en variables d'environnement) et sort un code Nagios
> (0 OK / 1 WARNING / 2 CRITICAL) selon le niveau de batterie.
> Propose un plan sans écrire de code.
```

```
# 2. Plan validé ? On exécute en non-interactif (la voie CI) :
codex exec --full-auto \
  "implémente le plan validé pour check_ups.py : module + tests pytest + README d'usage. \
   Les tests doivent passer. Ne modifie rien d'autre."

# 3. Contrôle systématique :
git status --porcelain
git diff --stat
pytest -q
```

**En CI (GitHub Actions)** — le cas d'usage roi de `codex exec` :

```yaml
# .github/workflows/codex-review.yml — exemple
name: codex-review
on: [pull_request]
jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 22 }
      - run: npm install -g @openai/codex
      - run: printenv OPENAI_API_KEY | codex login --with-api-key
        env: { OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }} }
      - run: >
          codex exec --json --full-auto --skip-git-repo-check
          "relis le diff de cette PR et liste les bugs potentiels par sévérité"
```

> 🔒 En CI : secret via `secrets.OPENAI_API_KEY`, jamais en dur ; périmètre
> `Full-auto` limité à de la **lecture/revue**, jamais d'écriture auto sur
> `main`.

## 35. Codex CLI : cloud, subagents, MCP, SDK

- **Codex cloud** : depuis le terminal, tu peux basculer une tâche vers le
  cloud (`codex cloud`) puis **rappliquer le résultat** dans ton repo local.
  Pratique pour les longues tâches : tu fermes ton laptop, ça continue.
- **Subagents** : Codex peut découper une investigation en agents parallèles
  et consolider leurs résultats dans la session principale.
- **MCP** : support **client** (tu branches des serveurs MCP). Config
  partagée dans `~/.codex/config.toml` — le même fichier est utilisé par le
  CLI, l'app desktop et l'extension IDE (bon point d'unification).

```toml
# ~/.codex/config.toml — extrait (schéma indicatif, vérifier la doc)
# [mcp.servers.docs]
# command = "npx"
# args = ["-y", "@modelcontextprotocol/server-filesystem", "/home/zelef/docs"]
```

- **Codex SDK** : embarque le même agent dans tes propres outils/scripts —
  la voie « je construis mon automatisation » au-delà du CLI.

## 36. Codex CLI : prix (vérifiés sept. 2026)

| Route | Prix | Notes |
|---|---|---|
| ChatGPT **Plus** | **20 $/mois** | Codex CLI **inclus** |
| ChatGPT **Pro** | paliers **5× / 20×** (le 20× a vu ses nouvelles inscriptions **mises en pause** le 10/09/2026 — existants non impactés) | Quotas supérieurs |
| ChatGPT **Free / Go** | inclus avec **limites basses** | Pour tester, pas pour produire |
| **Clé API** | **au token**, tarifs API OpenAI standard | Pas de quota plan, mais pas de fonctions cloud (revue, Slack…) |
| Business / Edu / Entreprise | inclus selon l'offre | **À vérifier** selon ton contrat |

Limites : usage régi par des **fenêtres glissantes de 5 h et hebdomadaires**,
variables selon modèle et tâche — consulte avec `/status` dans le CLI.
Avec une clé API, tu paies au token et tu n'as pas ces quotas, mais tu dois
surveiller la facture (section 98).

Ordres de grandeur API (modèles GPT-5, sept. 2026 — **à vérifier**, ça bouge
vite) : l'input se compte en dizaines de $/M tokens, l'output en centaines
de $/M sur les plus gros modèles. Traduction : **une session d'1 h sur
GPT-5.x peut coûter de quelques $ à 20 $+** selon le modèle exact. En
abonnement Plus, c'est « inclus » dans les 20 $ — imbattable si tu utilises
déjà ChatGPT.

## 37. Codex CLI vs Claude Code : différences franches

| Critère | Claude Code | Codex CLI |
|---|---|---|
| Fournisseur / modèles | Anthropic uniquement (Sonnet/Opus/Haiku) | OpenAI uniquement (GPT) |
| Licence du CLI | **Fermé** | **Open source (Apache 2.0)** |
| Fichier mémoire | `CLAUDE.md` + `AGENTS.md` | `AGENTS.md` |
| Plan mode | `Shift+Tab` | `/plan` |
| Permissions | Default / Auto-accept / `--dangerously-skip-permissions` | **Suggest / Auto-edit / Full-auto** |
| MCP | Client **+ serveur** (mature) | Client (en 2026) |
| Skills | Oui (`.claude/skills/`) | Pas d'équivalent direct en 2026 |
| Scriptable CI | `claude "prompt"` | **`codex exec`** (pensé pour ça : `--json`, `--full-auto`) |
| Cloud | Sessions web + `--teleport` | `codex cloud` + réapplication locale |
| Prix d'entrée | Pro 20 $/mois ou API | **Plus 20 $/mois (inclus)** ou API |
| Personnalité | « Pair collaboratif » (questionne, nuance) | « Exécutant » (fonce, demande moins) |

**Comment choisir** : tu as déjà ChatGPT Plus ? Codex CLI ne te coûte rien de
plus — teste-le en parallèle. Tu veux le meilleur agent « qui réfléchit avec
toi » sur des tâches longues ? Claude Code. Tu veux du CI scriptable et de
l'open source ? Codex CLI. Beaucoup de devs en 2026 **utilisent les deux** :
Claude Code pour concevoir, Codex pour exécuter en boucle.

## 38. Codex CLI : bonnes pratiques

1. **Toujours `AGENTS.md`** à la racine avant la première session — Codex
   n'a pas de `/init` magique équivalent : écris-le toi (section 6).
2. **Reste en Suggest/Auto-edit** tant que tu n'as pas vu l'agent travailler
   10 fois. Le Full-auto se mérite.
3. **`codex exec` + `git diff --exit-code`** : en script, tout changement
   inattendu doit faire échouer le job.
4. **Prompts fermés en non-interactif** : objectif + critères d'arrêt +
   périmètre de fichiers explicites. Jamais de « améliore le projet ».
5. **`/status`** régulièrement pour voir où tu en es des quotas (route
   abonnement).
6. **`~/.codex/auth.json` = secret** : permissions 600, jamais copié,
   jamais dans un backup non chiffré.
7. Sur Windows : vérifie le **VC++ Redist** avant de déclarer « ça ne
   marche pas » (section 32).

## 39. Codex CLI : points forts / faiblesses honnêtes

**Points forts** : inclus dans ChatGPT Plus (coût marginal nul si déjà
abonné) ; **open source Apache 2.0** ; `codex exec` excellent pour CI/scripts ;
`codex cloud` pour les longues tâches ; config unifiée CLI/desktop/IDE ;
maturité OpenAI sur l'exécution.

**Faiblesses honnêtes** : **verrouillage OpenAI** (comme Claude Code avec
Anthropic) ; personnalité « fonceuse » = plus de bêtises si mal cadré ;
MCP moins mature que Claude Code (client seul en 2026) ; pas de système de
skills équivalent ; les quotas d'abonnement sont opaques (fenêtres
glissantes non publiées en détail) ; la doc bouge vite (releases
hebdomadaires).

## 40. Codex CLI : verdict Zelef

**Le meilleur rapport qualité/prix si tu as déjà ChatGPT Plus.** Pour un
sysadmin : `codex exec` dans un cron qui génère un rapport d'inventaire, ou
dans une GitHub Action qui relit tes playbooks Ansible, c'est du pain bénit.
Garde-le en **exécutant** et Claude Code (ou OpenCode) en **concepteur** :
c'est le duo le plus rentable de 2026. Seul bémol structurel : tout repose
sur OpenAI — ne mets pas 100 % de tes automatisations critiques dessus sans
plan B.

---

# PARTIE B — ÉDITEURS & EXTENSIONS AGENTIQUES

---

## 41. Cursor : c'est quoi, pour qui

**Cursor** (cursor.com, édité par **Anysphere**) est un **éditeur de code IA
natif** : un fork de VS Code repensé autour de l'IA, pas « VS Code + un
plugin ». En 2026 c'est l'éditeur IA le plus utilisé (plusieurs centaines de
milliers de développeurs, ARR revendiqué en milliards).

Les trois couches, du plus passif au plus autonome :

1. **Tab** : complétion inline ultra-rapide pendant que tu tapes (le modèle
   `cursor-tab`, renforcé par le rachat de **Supermaven** — voir section 86).
2. **Composer** : tu décris un changement en langage naturel, il l'applique
   **sur plusieurs fichiers** (refacto, feature). En 2026, Cursor a aussi
   lancé son **propre modèle de code « Composer »**, réglé pour la vitesse
   dans l'éditeur.
3. **Agent** : exécution autonome multi-étapes (lit, édite, lance des
   commandes, ouvre des PR), avec **Background Agents** (tournent sans
   bloquer ton éditeur) et **Cloud Agents** (tournent sur l'infra Cursor).

Pour qui : tu vis dans l'éditeur et tu veux **le confort maximal** —
indexation du repo, diffs élégants, règles projet, agents d'arrière-plan —
sans assembler toi-même un puzzle CLI + plugins. Le prix : l'enfermement
dans l'éditeur (ton workflow ne te suit pas dans le terminal) et une
facturation par crédits parfois opaque.

## 42. Cursor : installation pas à pas

```bash
# Téléchargement : https://cursor.com/download
# (Windows, macOS, Linux — installeur graphique, pas de CLI d'install officiel)
```

1. Télécharge et installe depuis **cursor.com/download**.
2. Au premier lancement : importe tes extensions et réglages VS Code
   (proposé automatiquement — tes réflexes clavier suivent).
3. Connecte-toi (compte Cursor) et choisis ton plan (section 45).
4. Ouvre ton bac à sable : `File > Open Folder > /tmp/lab-agent`.
5. Vérifie l'indexation : la barre de statut indique l'état d'indexation du
   repo (l'agent « connaît » ton code via cet index — voir section 99 pour
   l'angle confidentialité).

**Fichiers de pilotage** (l'équivalent du CLAUDE.md, version Cursor) :

```
// .cursorrules  (à la racine — format historique, toujours lu)
// ou le dossier moderne :
.cursor/
  rules/          # règles projet (markdown)
  mcp.json        # serveurs MCP : { "mcpServers": { ... } }
```

Exemple de règle projet :

```markdown
# .cursor/rules/python.mdc
---
description: Standards Python du projet
globs: ["**/*.py"]
---
- Python 3.12, type hints obligatoires sur les fonctions publiques.
- Tests pytest dans tests/, un fichier de test par module.
- Docstrings en français. Pas de print() en prod : logging.
```

## 43. Cursor : Tab, Composer, Agent — mode d'emploi

**Tab (complétion)** — le plus rentable au quotidien :

- Tu tapes, une suggestion grise apparaît, `Tab` pour accepter.
- Réglages : `Cursor Settings > Tab` — active/désactive par langage.
- Astuce : Tab est **d'autant meilleur** que ton code est conventionné
  (noms explicites, types) — l'IA devine la suite logique.

**Composer (`Ctrl+I` / `Cmd+I`)** — le multi-fichiers :

```
1. Ctrl+I → panneau Composer.
2. Prompt : « Ajoute une option --format json à la commande inventaire,
   mets à jour le README et les tests. »
3. Cursor propose les fichiers touchés → tu valides le périmètre.
4. Il génère les diffs fichier par fichier → tu relis (diff agrégée).
5. Apply → les modifs s'appliquent, tu peux checkpoint/annuler.
```

**Agent (`Ctrl+L` puis mode Agent, ou onglet Agent)** — l'autonome :

```
1. Bascule en mode Agent.
2. Prompt avec objectif + garde-fous :
   « Ajoute la persistance SQLite au script d'inventaire.
     Contraintes : ne touche pas à cli.py, tests pytest verts à la fin,
     arrête-toi et résume si un choix d'architecture se présente. »
3. L'agent travaille : tu vois les outils appelés, les diffs se construisent.
4. Review : chaque fichier a un diff à accepter/rejeter.
5. Background : pour les longues tâches, « Run in background » → tu continues
   à coder pendant qu'il bosse.
```

**Checkpoints** : Cursor versionne l'état avant chaque action d'agent —
tu peux revenir en arrière d'un cran (`Restore checkpoint`). C'est ton
`git stash` d'urgence quand tu n'as pas commité.

## 44. Cursor : premier workflow commenté

Scénario : **refactorer** un script monolithique en modules, avec revue.

```
1. Ouvre /tmp/lab-agent dans Cursor. Attends la fin de l'indexation.
2. Crée .cursor/rules/python.mdc (section 42) — 5 lignes de conventions.
3. Sélectionne le gros fichier, Ctrl+I (Composer) :
   « Découpe ce script en 3 modules (collecte, formatage, cli) sans changer
     son comportement. Propose d'abord la découpe, attends ma validation. »
   → Composer propose la découpe. VALIDE ou CORRIGE avant l'exécution.
4. Il génère les diffs → relis chaque diff (clic sur fichier).
5. Passe en Agent pour la suite :
   « Ajoute les tests pytest manquants pour les 3 modules. Lance-les.
     Si un test échoue, corrige le code, pas le test, sauf si le test
     est objectivement faux — dans ce cas explique-moi pourquoi. »
6. Vérifie : terminal intégré → pytest -q. git diff → relecture finale.
```

Ce que ce workflow t'apprend : **Cursor excelle quand tu alternes les
couches** (Composer pour le périmètre précis, Agent pour l'autonomie
cadrée), et il punit le « prompt unique magique » (diffs énormes,
irrelisibles, à jeter).

## 45. Cursor : prix (vérifiés sept. 2026 — À VÉRIFIER, ça a bougé en 2026)

⚠️ **Les grilles Cursor ont été restructurées en 2026** et les sources se
contredisent sur le détail. Chiffres recoupés (sept. 2026), **à confirmer
sur cursor.com/pricing avant tout engagement** :

| Plan | Prix indicatif | Contenu indicatif |
|---|---|---|
| **Hobby** | 0 € | Quota limité (complétions + requêtes premium lentes) — pour tester |
| **Pro** | **~20 $/mois** | Tab illimité, requêtes rapides incluses, agents |
| **Pro+** | **~60 $/mois** | ~3× l'usage Pro |
| **Ultra** | **~200 $/mois** | ~20× l'usage, fonctions prioritaires |
| **Teams/Business** | **~40 $/utilisateur/mois** | Facturation centrale, SSO, analytics, privacy mode |
| **Bugbot** (add-on) | ~40 $/utilisateur/mois | Revue de PR par IA avec règles |

Points d'attention :

- La facturation passe par des **crédits d'usage** : le « Pro à 20 $ »
  inclut une enveloppe d'usage API (~20 $) — au-delà, ça se paie. Surveille
  le dashboard d'usage.
- **Étudiants** : Pro gratuit 1 an (selon l'offre en cours — à vérifier).
- Le modèle **Composer** (maison) est optimisé pour être rapide et
  économique dans l'éditeur : l'utiliser par défaut réduit la facture.
- Certaines sources (fév. 2026) évoquent un plan **Individual à 60 $/mois**
  remplaçant l'ancien Pro à 20 $ — **contradiction non tranchée** :
  vérifie la page pricing le jour où tu t'abonnes.

## 46. Cursor : points forts / faiblesses honnêtes

**Points forts**

- L'expérience éditeur la plus **polie** : indexation repo, diffs agrégées,
  checkpoints, background agents — tout est pensé pour ne pas te ralentir.
- **Tab** excellent (renforcé par Supermaven) : le gain quotidien le plus
  tangible.
- **Composer** (modèle maison) : rapide, bien réglé pour le code in-éditeur.
- Règles projet (`.cursor/rules/`) + MCP : le cadrage d'équipe est simple.
- Écosystème : Bugbot (revue PR), Cloud Agents, intégrations CI.

**Faiblesses honnêtes**

- **Enfermement éditeur** : ton investissement (règles, habitudes) ne te
  suit pas dans le terminal ni ailleurs. Les CLI sont agnostiques de
  l'éditeur.
- **Facturation par crédits** : moins lisible qu'un forfait ou qu'une
  facture API au token. Des utilisateurs se font surprendre.
- **Confidentialité** : ton repo est **indexé sur l'infra Cursor** (cloud
  only, pas d'option on-prem en 2026). Le « privacy mode » promet de ne
  rien stocker, mais le code transite par leurs serveurs — à évaluer
  (section 99), surtout pour du code client sensible.
- Fork VS Code : tu dépends du rythme d'Anysphere pour les nouveautés de
  l'éditeur lui-même.
- Tentation du « tout-Agent » : l'outil rend si facile de déléguer qu'on
  oublie de comprendre — dangereux pour un sysadmin responsable d'une prod.

## 47. Cursor : bonnes pratiques

1. **Règles projet dès le jour 1** (`.cursor/rules/`) : c'est 80 % de la
   qualité des sorties.
2. **Composer pour le précis, Agent pour l'autonome** — ne demande jamais à
   l'Agent ce que Composer fait en 30 secondes.
3. **Valide le périmètre de fichiers AVANT** la génération (Composer le
   propose : ne clique pas « tout accepter » par flemme).
4. **Checkpoints + git** : checkpoint pour l'urgence, commit git pour le
   durable.
5. **Modèle par défaut = Composer** (rapide/pas cher) ; bascule sur
   Claude/GPT pour l'architecture et le debug profond.
6. **Relis les diffs comme si un stagiaire les avait écrites** — parce que
   c'est exactement ce qui s'est passé (section 100).
7. **Dashboard d'usage chaque semaine** tant que tu n'as pas calibré ta
   consommation.

## 48. Cursor : usage en équipe

```bash
.cursor/
  rules/
    python.mdc        # standards partagés (versionnés !)
    ansible.mdc       # tes conventions Ansible d'équipe
    securite.mdc      # interdictions (secrets, prod, sudo…)
  mcp.json            # serveurs MCP d'équipe (docs internes, tickets)
```

Conventions d'équipe :

1. **Les règles sont versionnées et relues** comme du code (PR pour modifier
   `.cursor/rules/`).
2. **Un Bugbot bien réglé** vaut mieux que trois règles floues : encodez vos
   standards de revue dedans.
3. **Privacy mode activé par défaut** + charte : quel code a le droit d'être
   indexé (pas les repos clients sensibles sans accord).
4. **Budget par développeur** : alerte à 80 % de l'enveloppe mensuelle.

## 49. Cursor : verdict Zelef

**Le meilleur choix si tu veux coder dans un éditeur sans te poser de
questions.** Pour un sysadmin qui écrit du Python/Ansible au quotidien,
le duo **Tab + Composer** fait gagner un temps fou sur le boilerplate.
En revanche, pour tes usages « RAG » (ingestion, nettoyage de corpus en
batch), un CLI scriptable (OpenCode/Codex) est plus adapté que Cursor.
Et la question de l'indexation cloud doit être tranchée avant d'y mettre du
code sensible. Idéal en **complément** d'un CLI, pas en remplacement.

## 50. Récapitulatif Partie A+B (les 5 grands)

|  | OpenCode | Claude Code | Codex CLI | Cursor | Kilo Code |
|---|---|---|---|---|---|
| Nature | CLI/TUI open source | CLI officiel Anthropic | CLI officiel OpenAI (OSS) | Éditeur IA | Agent multi-surfaces (OSS) |
| Modèles | Tous (BYOK) | Claude only | GPT only | Multi (dont maison) | Tous (BYOK) |
| Install | `curl .../install \| bash` | `npm i -g @anthropic-ai/claude-code` | `npm i -g @openai/codex` | cursor.com/download | marketplace / `npm i -g @kilocode/cli` |
| Mémoire projet | `AGENTS.md` | `CLAUDE.md` + `AGENTS.md` | `AGENTS.md` | `.cursor/rules/` | `AGENTS.md` |
| Plan mode | `Tab` (Plan/Build) | `Shift+Tab` | `/plan` | périmètre validé | Architect |
| Prix d'entrée | 0 € + ta clé | 20 $/mois ou API | 20 $/mois (Plus, inclus) ou API | ~20 $/mois (à vérifier) | 0 € + ta clé / Pass ~19 $ |
| Scriptable CI | `opencode run` | `claude "…"` | **`codex exec`** | Cloud Agents | CLI `kilo` |
| Verrouillage | Aucun | Anthropic | OpenAI | Anysphere | Aucun (mais jeune) |

## 51. Cline : c'est quoi, pour qui

**Cline** (anciennement « Claude Dev », extension VS Code `saoudrizwan.claude-dev`)
est **l'ancêtre open source (MIT)** de toute la famille : Cline → Roo Code →
Kilo Code. C'est un **agent autonome dans la barre latérale de VS Code** :
il lit/écrit des fichiers, exécute des commandes, utilise même un navigateur,
**toujours avec ta validation** par étape.

Particularités 2026 :

- **Boucle Plan/Act explicite** : rien ne se fait sans ton feu vert par
  défaut — l'agent le plus « transparent » sur ce qu'il fait.
- **BYOK + 200+ modèles via OpenRouter** : agnostique, comme OpenCode.
- **MCP** : oui, config manuelle.
- **CLI 2.0** : mode headless pour CI/CD (les extensions agentiques n'ont
  généralement pas ça).
- Statut août 2026 : **actif et maintenu**, la référence historique — même
  si l'innovation s'est déplacée vers Kilo Code.

Pour qui : tu veux un **agent open source DANS VS Code** (pas un nouvel
éditeur), avec un contrôle fin à chaque étape, et tu acceptes de configurer
toi-même (clés, MCP). Le choix « prudent et transparent ».

## 52. Cline : installation pas à pas

1. VS Code → Extensions (`Ctrl+Shift+X`) → cherche **« Cline »**
   (éditeur : `saoudrizwan.claude-dev`).
2. Installe → l'icône Cline apparaît dans la barre d'activité.
3. Ouvre la vue Cline → **choisis ton fournisseur** :
   - **Clé API directe** (Anthropic, OpenAI, Google…) : colle ta clé.
   - **OpenRouter** : une seule clé pour 200+ modèles (recommandé pour
     tester sans multiplier les comptes).
   - **Modèle local** (Ollama) : gratuit, privé, plus lent/faible.
4. Règle les **auto-approvals** (section 53) avant la première tâche.

> 🔒 Comme toujours : la clé API vit dans le gestionnaire de Cline
> (stockage sécurisé de VS Code), jamais dans un fichier du projet.

## 53. Cline : modes Plan/Act et approbations

Le cœur de Cline, c'est son **contrat de confiance explicite** :

```
┌─────────┐     ┌─────────┐     ┌──────────────┐
│  PLAN   │ ──▶ │ TON AVIS│ ──▶ │     ACT      │
│ lit,    │     │ tu valides│    │ édite, exécute│
│ propose │     │ ou corriges│   │ par étapes   │
└─────────┘     └─────────┘     └──────────────┘
       ▲                               │
       └─────── tu interromps/corriges ─┘
```

**Auto-approve** : par type d'action, tu règles ce qui passe sans demander :

- ✅ Lecture de fichiers : auto (sans risque).
- ⚠️ Écriture/édition : **demander** au début, auto quand tu fais confiance
  au périmètre.
- 🔴 Exécution de commandes : **toujours demander** au début (c'est là que
  vivent les `rm -rf` et les `pip install` sauvages).
- 🌐 Accès navigateur : demander.

Checklist de démarrage (à cocher) :

- [ ] Lecture : auto-approve ON
- [ ] Écriture : OFF (demander) les 2 premières semaines
- [ ] Commandes : OFF, avec limite « ne jamais auto-approuver `sudo`,
      `rm -rf`, `pip install`, `curl | bash` »
- [ ] Chaque session : relire le résumé d'actions avant de fermer

## 54. Cline : premier workflow commenté

Scénario : ajouter du **logging** propre à un script, en gardant la main.

```
1. Ouvre /tmp/lab-agent dans VS Code. Panneau Cline.
2. Prompt (mode Plan implicite) :
   « Analyse check_ups.py et propose un plan pour remplacer les print()
     par du logging structuré (niveau configurable via --log-level).
     Ne modifie rien pour l'instant. »
   → Cline répond avec un plan + les fichiers qu'il lirait.
3. Clique « Act » (ou valide) : Cline exécute étape par étape.
   À CHAQUE action sensible (édition, commande), une carte d'approbation
   apparaît : [Approve] [Reject] (+ « toujours approuver ce type »).
4. Quand il propose `pytest -q`, approuve : tu vois la sortie dans le panneau.
5. À la fin : « Review » du diff intégré, puis commit git manuel.
```

Le point pédagogique : **Cline te montre tout**. C'est plus lent que Cursor,
mais après 2 semaines tu as compris la boucle agentique dans les moindres
détails — une excellente école avant de passer à des agents plus autonomes.

**Mode headless (CLI 2.0)** pour CI :

```bash
# Exemple indicatif (syntaxe exacte : doc Cline en vigueur)
cline --headless "exécute pytest -q et résume les échecs" --approve read-only
```

## 55. Cline : MCP — configuration manuelle

Cline a son **propre fichier de settings MCP**, séparé de celui de VS Code :

| OS | Chemin |
|---|---|
| Linux | `~/.config/Code/User/globalStorage/saoudrizwan.claude-dev/settings/cline_mcp_settings.json` |
| macOS | `~/Library/Application Support/Code/User/globalStorage/saoudrizwan.claude-dev/settings/cline_mcp_settings.json` |
| Windows | `%APPDATA%\Code\User\globalStorage\saoudrizwan.claude-dev\settings\cline_mcp_settings.json` |

Le plus simple : panneau Cline → icône **MCP Servers** → **Configure MCP
Servers** (ouvre le fichier), puis :

```json
{
  "mcpServers": {
    "docs": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/home/zelef/docs"],
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

- `autoApprove: []` = l'agent **demandera** avant d'utiliser les outils de ce
  serveur. Mets des outils en auto-approve uniquement quand ils sont en
  lecture seule et de confiance.
- Redémarre la vue Cline après modification.

## 56. Cline : prix, forces, faiblesses, verdict

**Prix** : extension **gratuite et open source (MIT)**. Tu paies uniquement
l'usage modèle (ta clé API / OpenRouter / local). Le moins cher du marché à
fonctionnalité égale — et le plus transparent sur ce que tu consommes
(puisque tu vois la facture du fournisseur).

**Points forts** : transparence totale (chaque action est montrée et
validée) ; open source MIT ; BYOK multi-fournisseurs ; MCP ; CLI headless
pour CI ; historique et communauté énormes (tutos, modes personnalisés
partagés) ; excellent pour **apprendre** l'agenticité.

**Faiblesses honnêtes** : **pas d'autocomplétion inline** (ce n'est pas son
métier — couple-le avec Supermaven/Copilot si tu veux du Tab) ; config MCP
manuelle (fichier caché, pas de marketplace intégré comme Kilo) ; un seul
agent généraliste (pas de modes Architect/Code/Debug natifs — des modes
personnalisés JSON existent mais c'est du bricolage) ; rythme d'innovation
plus lent que Kilo Code depuis 2026 ; l'innovation s'est déplacée.

**Verdict Zelef** : **installe-le pour apprendre**, même si tu choisis un
autre agent au quotidien. Deux semaines avec Cline en mode « tout valider »
t'apprennent plus sur les agents que deux mois de Cursor en pilote
automatique. Et pour un usage sobre (BYOK + modèle pas cher), ça reste
l'option la plus économique.

## 57. Kilo Code côté éditeur : ce qui change vs Cline

Si tu connais Cline, voici ce que Kilo Code ajoute/changent (vérifié 2026) :

| Aspect | Cline | Kilo Code |
|---|---|---|
| Base de code | Historique (Cline) | **Reconstruit sur OpenCode** (avr. 2026) |
| Modes | Plan/Act (un agent) | **Architect / Code / Debug / Ask** + modes perso |
| Autocomplétion inline | Non | **Oui** |
| Sessions parallèles | Non (expérimental) | **Agent Manager + git worktrees** |
| MCP | Manuel (`cline_mcp_settings.json`) | **Marketplace intégré** |
| Routage modèles | Manuel | **Automatique** (par palier) |
| Config | `cline_mcp_settings.json` global | `kilo.jsonc` + `.kilo/` (partagée CLI/IDE) |
| Fichier d'instructions | Modes JSON perso | **`AGENTS.md`** (standard) |
| IDE | VS Code (+ JetBrains Enterprise) | **VS Code + JetBrains + CLI + Cloud** |

Migration Cline/Roo → Kilo : Kilo lit les `.roomodes` et `.roo/rules/` au
démarrage (compat ascendante annoncée) et migre les `.kilocode/` vers
`.kilo/`. En pratique : installe Kilo à côté de Cline, importe tes clés,
laisse les deux cohabiter une semaine, puis tranche.

## 58. Kilo Code : workflow type côté éditeur

```
1. Ouvre ton projet. La vue Kilo Code propose les modes : Architect, Code,
   Debug, Ask.
2. NOUVELLE FONCTIONNALITÉ :
   a. Mode Architect : « Conçois la fonctionnalité X. Contraintes : […].
      Livre un plan avec fichiers et tests. »
   b. Relis le plan (c'est le moment où tu gagnes de l'argent).
   c. Mode Code : « Implémente le plan validé. »
   d. Mode Debug si les tests échouent : « Les tests échouent avec [log].
      Diagnostique sans tout réécrire. »
3. GROSSE TÂCHE EN PARALLÈLE : Agent Manager → 2 sessions (feature + tests)
   dans 2 worktrees → revue → merge.
4. QUESTION RAPIDE : mode Ask, sans toucher au code.
```

## 59. Extensions agentiques : tableau comparatif

|  | Cline | Kilo Code (ext.) | Cursor (éditeur) | Continue (ext.) |
|---|---|---|---|---|
| Forme | Extension VS Code | Extension VS Code/JetBrains | Éditeur complet | Extension VS Code/JetBrains |
| Licence | MIT (OSS) | MIT (OSS) | Propriétaire | Apache 2.0 (OSS) |
| Modèles | BYOK (200+ via OpenRouter) | BYOK / Gateway / Pass | Multi (dont maison) | BYOK (tout fournisseur) |
| Agent autonome | Oui (Plan/Act) | Oui (modes) | Oui (Agent/Composer) | Agent (plus récent) |
| Tab inline | Non | Oui | Oui (excellent) | Oui |
| MCP | Manuel | Marketplace | Oui | Oui (mode Agent) |
| Prix outil | 0 € | 0 € | ~20 $/mois (à vérifier) | 0 € |
| Idéal pour | Apprendre, contrôle total | Agent OSS multi-surfaces | Confort max, tout intégré | Rester sur VS Code stock, OSS |

## 60. Choisir son extension : arbre de décision

```
Tu veux rester sur VS Code classique ?
├── OUI → Tu veux de l'open source ?
│   ├── OUI → Contrôle étape par étape ? → **Cline**
│   │         Sessions parallèles / modes ? → **Kilo Code**
│   └── NON → **Continue** (l'option OSS pragmatique, section 84)
└── NON (tu acceptes un nouvel éditeur) → **Cursor**
    └── …mais vérifie la question confidentialité (section 99) avant
        d'y mettre du code sensible.
```

## 61. Faut-il cumuler extension + CLI ?

**Oui, et c'est même le setup recommandé en 2026** — mais avec des rôles
distincts :

| Rôle | Outil conseillé | Pourquoi |
|---|---|---|
| Complétion au fil de la frappe | Tab (Cursor) / Supermaven / Copilot | Zéro friction, gain immédiat |
| Refacto cadrée multi-fichiers | Composer (Cursor) / Kilo Code (mode Code) | Diffs relisibles |
| Exploration / plan | CLI (OpenCode/Claude Code) en Plan mode | Moins de distraction, scriptable |
| Exécution longue / CI | `codex exec` / `opencode run` | Non-interactif, JSON, cron-friendly |
| Revue | Subagent reviewer / Bugbot | Deuxième paire d'yeux |

Anti-pattern : **trois agents autonomes sur le même repo en même temps**
(conflits d'édition, factures ×3, aucun responsable). Un seul agent
« écrivain » à la fois par working directory — les autres en lecture.

## 62. Raccourcis et réflexes à ancrer (pense-bête éditeur)

```
PLAN D'ABORD ......... Architect (Kilo) / Plan mode / périmètre validé (Cursor)
JAMAIS SANS DIFF ...... relis chaque diff avant Apply/commit
UN SEUL ÉCRIVAIN ...... un agent actif en écriture par dossier
COMMIT AVANT .......... git commit propre avant chaque session d'agent
SECRETS ............... .env.example, jamais .env réel sous les yeux de l'agent
INTERROMPRE ........... Esc / Stop dès que ça dérive (corriger tôt = 10× moins cher)
SESSIONS COURTES ...... >45 min → /compact ou nouvelle session résumée
```

## 63. Ce que les extensions voient de ton code (rappel vie privée)

Une extension agentique lit **tout ton workspace ouvert** par construction.
Points à vérifier par outil (section 99 détaillée) :

- Où sont traités les prompts (local vs cloud du fournisseur) ?
- Le code sert-il à l'entraînement (opt-in/opt-out) ?
- L'indexation (Cursor) est-elle désactivable par repo ?
- Les logs de session contiennent-ils des extraits de code en clair
  (`~/.claude/`, `~/.codex/`, dossiers de stockage VS Code) ?

## 64. Verdict extensions : la recommandation pragmatique

- **Tu débutes avec les agents** → Cline (2 semaines, tout valider).
- **Tu veux de l'OSS au quotidien dans VS Code** → Kilo Code.
- **Tu veux le confort maximal et tu acceptes l'abonnement + le cloud** →
  Cursor.
- **Tu veux rester sur VS Code stock en OSS** → Continue (section 84).
- Dans tous les cas : **un CLI à côté** (OpenCode ou Claude Code) pour le
  scriptable et les longues sessions.

---

# PARTIE C — L'AGENT PERSONNEL AUTONOME : OPENCLAW

---

## 65. « Open clow » : c'est bien OpenClaw

Le nom entendu (« open clow ») correspond à **OpenClaw** (openclaw.ai,
github.com/openclaw/openclaw) — il n'existe **aucun outil sérieux nommé
« OpenClow »** en septembre 2026. La confusion est fréquente à l'oral :
*claw* (griffe) se prononce comme *clow*.

⚠️ Ne confonds pas non plus avec :

- **Claude** (Anthropic) — le modèle, pas l'agent.
- **Clawdbot / Moltbot** — les **anciens noms** d'OpenClaw (voir section 66).

## 66. OpenClaw : c'est quoi — et son histoire folle

**OpenClaw** est un **agent IA personnel, autonome, auto-hébergé et open
source (MIT)**. La différence fondamentale avec tout ce qui précède : il ne
vit pas dans ton terminal ou ton éditeur, il vit **sur une machine qui
tourne en permanence** (ton serveur, un Mini-PC, un VPS) et tu le pilotes
depuis **tes apps de messagerie** (WhatsApp, Telegram, Discord, Slack,
Signal, iMessage…) ou via le web.

Ce qu'il fait, concrètement : tu lui écris « résume-moi mes mails urgents
et prépare les réponses », il **agit** — lit les mails, rédige, te propose,
envoie si tu l'autorises. Avec des **crons intégrés**, il travaille aussi
**sans qu'on lui demande** (ex. : chaque matin à 7h, briefing du jour).

**L'histoire** (vérifiée, c'est documenté partout en 2026) :

| Date | Événement |
|---|---|
| Nov. 2025 | **Clawdbot** : prototype publié par Peter Steinberger (écrit en ~1 h) |
| Janv. 2026 | Renommé **Moltbot** (plainte marque d'Anthropic — le thème du homard reste) |
| 30 janv. 2026 | Renommé **OpenClaw** (« Moltbot » était imprononçable) |
| Fév. 2026 | **145 000+ stars**, croissance explosive |
| 14 fév. 2026 | Steinberger **rejoint OpenAI** |
| Mars 2026 | **250 000+ stars**, dépasse React sur GitHub — l'un des projets OSS à la croissance la plus rapide de l'histoire |

Écosystème : **ClawHub**, un registre de **skills** (13 000+ skills début
2026 — « le npm des capacités d'agent ») : navigateur, e-mail, fichiers,
API, domotique, etc. 100+ skills préconfigurées à l'install.

⚠️ **Le revers de la viralité** : l'attaque **« ClawHavoc »** (documentée
début 2026) — 800+ skills malveillantes uploadées sur ClawHub (prompt
injection, payloads malveillants), des dizaines de milliers d'instances
exposées (défaut d'écoute sur `0.0.0.0:18789`), plusieurs CVE. Moralité :
**OpenClaw est puissant ET c'est l'outil de ce guide qui exige le plus de
rigueur sécurité** (section 72).

## 67. OpenClaw : installation pas à pas

Prérequis : une machine **qui reste allumée** (serveur Debian, Mini-PC,
VPS), Node.js, et **une clé API** (Anthropic ou OpenAI — c'est « le cerveau »).

```bash
# Méthode officielle — vérifiée sept. 2026
curl -fsSL https://openclaw.ai/install.sh | bash

# Alternative npm
npm install -g openclaw@latest

# Assistant d'onboarding (wizard) + installation en daemon (service)
openclaw onboard --install-daemon
```

Le wizard te demande :

1. **Le fournisseur de modèle** + ta clé API (Anthropic ou OpenAI ; modèles
   locaux via Ollama possibles).
2. **Le canal de messagerie** : Telegram (le plus simple pour débuter —
   crée un bot via @BotFather, colle le token), WhatsApp, Discord, Slack…
3. **Le workspace** : ton répertoire home devient son espace de travail
   (identité, mémoire, config y vivent).

```bash
# Vérifier que le daemon tourne
openclaw status

# Voir les logs
openclaw logs --tail 50
```

> 🔒 **Dès l'install** : ne laisse PAS l'écoute réseau sur `0.0.0.0`
> (c'était le défaut impliqué dans ClawHavoc). Lie l'interface à
> `127.0.0.1` ou protège par firewall + authentification. Voir section 72.

Alternative Docker (si tu préfères) :

```bash
git clone https://github.com/openclaw/openclaw.git
cd openclaw
docker compose up -d
```

## 68. OpenClaw : premier workflow commenté

Scénario : **briefing matinal automatique** + une tâche à la demande.

**Étape 1 — la tâche à la demande (depuis Telegram) :**

```
Toi (Telegram) : « Liste les fichiers modifiés cette semaine dans
                 ~/projets/inventaire et résume les changements. »

OpenClaw : [exécute git log, lit les diffs] « 4 fichiers modifiés :
           - app/collecte.py : ajout du retry SNMP (+2 fonctions)
           - tests/test_collecte.py : 3 nouveaux tests
           - README.md : section installation mise à jour
           - requirements.txt : pysnmp ajouté
           Veux-tu que je génère le changelog ? »
```

**Étape 2 — le cron (le vrai pouvoir d'OpenClaw) :**

```
Toi (Telegram) : « Tous les jours à 7h00, envoie-moi :
                 1) la météo du jour,
                 2) mes 3 rendez-vous du calendrier,
                 3) l'état des sauvegardes (vérifie /var/log/backup.log). »

OpenClaw : « C'est noté. Cron 'briefing-matin' créé : 0 7 * * *.
           Je te l'enverrai ici. »
```

**Étape 3 — la mémoire persistante :**

```
Toi : « Retiens que mes onduleurs sont des Easy UPS 3S et que je veux
       toujours un résumé en français, concis. »

OpenClaw : [écrit dans sa mémoire] « Noté. »
# → Trois jours plus tard, il s'en souvient sans qu'on lui redise.
```

C'est ça, la différence : **continuité** (mémoire), **initiative** (crons),
**profondeur** (il connaît ton contexte) — pas juste un chatbot.

## 69. OpenClaw : skills, ClawHub, MCP

- **Skills** : le format d'extension d'OpenClaw. Une skill = un dossier avec
  une description + les outils qu'elle apporte (naviguer sur le web, envoyer
  un mail, piloter ta domotique…).
- **ClawHub** (clawhub.ai) : le registre communautaire (des milliers de
  skills). Installation d'une skill :

```
Toi (Telegram) : « Installe la skill 'github-releases' depuis ClawHub. »
```

⚠️ **Règle d'or post-ClawHavoc** : n'installe une skill que si (1) elle est
populaire et auditée, (2) tu as lu ce qu'elle fait, (3) l'analyse VirusTotal
— désormais intégrée — est propre. **Jamais de skill obscure en prod.**

- **MCP** : OpenClaw supporte MCP — tu peux brancher les mêmes serveurs que
  tes autres agents (ton serveur « docs » du RAG, par exemple).
- **Modèles locaux** : Ollama/LM Studio supportés — un OpenClaw 100 % local
  (cerveau local + machine locale) est possible pour les usages sensibles,
  au prix d'un cerveau moins brillant.

## 70. OpenClaw : ce qu'il ne faut JAMAIS lui confier (garde-fous)

OpenClaw est l'agent le plus **autonome** de ce guide — donc celui où les
garde-fous comptent le plus :

1. 🔒 **Jamais d'accès direct à la prod** sans validation humaine : pas de
   redémarrage de service, pas de `apt upgrade`, pas de modification réseau
   en autonome.
2. 🔒 **Jamais de secrets en mémoire en clair** au-delà du nécessaire : la
   clé API du « cerveau » et les tokens de messagerie sont le minimum vital ;
   tout le reste (mots de passe, clés SSH) passe par des accès limités.
3. 🔒 **Jamais d'instance exposée sur Internet sans auth** : le défaut
   `0.0.0.0:18789` a causé des prises de contrôle (RCE). Bind local +
   VPN/Tailscale ou reverse proxy avec auth forte.
4. 🔒 **Crons en lecture seule d'abord** : un cron qui *lit* et *résume*,
   oui ; un cron qui *modifie* ou *envoie*, seulement après des semaines de
   comportement sain — et avec notification avant action.
5. 🔒 **Principe du moindre privilège** : l'utilisateur système qui fait
   tourner le daemon n'a ni sudo sans mot de passe, ni accès aux clés de
   prod, ni écriture hors de son workspace.

Checklist de durcissement (à cocher après install) :

- [ ] Écoute réseau liée à 127.0.0.1 ou derrière auth (pas 0.0.0.0 nu)
- [ ] Utilisateur dédié sans sudo
- [ ] Skills installées : liste relue, sources connues
- [ ] Crons : tous en lecture/notification pour commencer
- [ ] Sauvegarde du workspace (mémoire + config) chiffrée

## 71. OpenClaw : prix

| Poste | Coût |
|---|---|
| Le logiciel | **0 €** — MIT, gratuit |
| Le « cerveau » | Ta clé API (Anthropic/OpenAI) au token, ou modèle local gratuit |
| La machine | Ton serveur existant (coût marginal ~nul) ou VPS (~5 €/mois) |
| La messagerie | 0 € (bots Telegram gratuits) |

C'est l'agent **le moins cher** de ce guide en coût logiciel — le coût est
dans les tokens du cerveau et dans **ton temps de durcissement**. Ne
sous-estime pas ce dernier.

## 72. OpenClaw : verdict Zelef

**Le plus excitant ET le plus dangereux de ce guide.** Pour un chef de
service systèmes : un OpenClaw bien durci qui t'envoie le briefing infra du
matin sur Telegram, qui surveille tes logs et te prévient — c'est un vrai
multiplicateur. Mais c'est aussi une **surface d'attaque** ( ClawHavoc l'a
prouvé) et un agent qui agit **sans toi**. Ma recommandation : installe-le
sur une machine dédiée, en **lecture seule + notifications** pendant un
mois, durcis selon la checklist (section 70), et ne lui donne des droits
d'écriture que progressivement. Pour coder, garde les outils des parties A
et B — OpenClaw n'est pas un agent codeur, c'est un **agent de vie**.

---

# PARTIE D — PLUGINS & COMPLÉTION : LE PANORAMA

---

## 73. Les trois métiers des plugins (ne pas confondre)

| Métier | Ce que ça fait | Exemples (sept. 2026) |
|---|---|---|
| **Complétion** | Suggère la suite pendant que tu tapes | Supermaven, Copilot, Tabnine, Codeium, Tabby |
| **Chat in-IDE** | Répond/explique dans l'éditeur | Continue (chat), Copilot Chat, JetBrains AI |
| **Agent en extension** | Agit de façon autonome | Cline, Kilo Code, Continue (mode Agent) |

Un plugin peut cumuler (Continue fait les trois). La règle d'achat : **paie
pour UN métier à la fois** — payer deux complétions concurrentes, c'est
payer deux fois pour des suggestions qui se marchent dessus.

## 74. Continue : l'extension open source pragmatique

**Continue** (continue.dev) est une extension **open source (Apache 2.0)**
pour **VS Code et JetBrains** : chat, complétion, édition multi-fichiers, et
(un plus récent) **mode Agent** — le tout **BYOK** (n'importe quel modèle :
OpenAI, Anthropic, Ollama local…).

Installation :

1. Marketplace VS Code/JetBrains → **« Continue »**.
2. Au premier lancement : choisis ton fournisseur et colle ta clé (ou
   Ollama local pour du 100 % privé).
3. Config dans `~/.continue/config.yaml` (modèles, règles, MCP).

```yaml
# ~/.continue/config.yaml — extrait (schéma indicatif)
models:
  - name: "Mon modèle chat"
    provider: openrouter
    model: qwen/qwen3-coder
    apiKey: ${OPENROUTER_API_KEY}   # variable d'environnement, jamais en dur
tabAutocompleteModel:
  provider: ollama
  model: qwen2.5-coder:1.5b         # petit modèle local pour le Tab : gratuit et privé
```

Points forts : **reste sur ton VS Code stock** (pas de fork), 100 % OSS,
BYOK total, complétion locale possible (Ollama), MCP en mode Agent.
Faiblesses : moins « magique » que Cursor (pas d'indexation cloud poussée),
le mode Agent est plus jeune que Cline/Kilo, la qualité dépend du modèle
branché.

**Verdict** : le meilleur choix si ton cahier des charges dit « VS Code
officiel + open source + je choisis mon modèle ». Excellent couple avec un
petit modèle local pour la complétion (0 €, privé) et un gros modèle cloud
pour le chat/agent.

## 75. Supermaven : la complétion ultra-rapide

**Supermaven** (supermaven.com) est une extension de **complétion IA**
(VS Code, JetBrains, Neovim) réputée pour sa **vitesse** et sa **fenêtre de
contexte énorme** (1M tokens annoncés — elle « voit » très large pour des
suggestions pertinentes).

Repère important 2026 : **Supermaven a été racheté par Anysphere (Cursor)**
— sa technologie alimente désormais le Tab de Cursor. L'extension
standalone existe toujours (à vérifier sur supermaven.com — les offres
évoluent après un rachat).

Pour qui : tu veux **uniquement** de la complétion fulgurante, sans agent,
sans chat, sans changer d'éditeur. Le « métier » pur de la section 73.

## 76. GitHub Copilot : le standard historique

**GitHub Copilot** reste en 2026 **l'outil le plus déployé** (intégré à
VS Code, JetBrains, GitHub lui-même). Il a grandi : complétion + **chat** +
**mode agent** (avec choix parmi quelques modèles).

Tarifs indicatifs (sept. 2026 — à vérifier sur github.com/features/copilot) :

| Plan | Prix |
|---|---|
| Pro | **~10 $/mois** |
| Pro+ | **~39 $/mois** |
| Business / Enterprise | par siège (plus cher) |
| Gratuit | 2 000 complétions/mois + 50 requêtes premium/mois (étudiants/enseignants/mainteneurs OSS : gratuit) |

Points forts : intégration GitHub/VS Code la plus profonde du marché,
disponibilité entreprise (SSO, conformité), prix d'entrée bas, choix de
modèles (limité mais curé). Faiblesses : modèles imposés (pas de BYOK
généralisé), moins bon sur les longues sessions autonomes que les CLI
dédiés, facturation « requêtes premium » parfois confuse.

**Verdict** : si ton entreprise est standardisée GitHub, c'est le choix
évident et le moins cher. Pour un indépendant qui veut le meilleur agent,
les CLI des parties A vont plus loin.

## 77. Tabnine : la complétion « privacy-first »

**Tabnine** (tabnine.com) : complétion IA, 15+ éditeurs, argument historique
= **confidentialité** (modèles locaux, entraînement personnalisé sur ton
codebase sans qu'il quitte ton infra).

Pour qui : entreprise avec une politique « le code ne sort pas » stricte,
ou dev qui veut de la complétion sans cloud. En 2026, l'argument reste
valable mais la concurrence « locale » s'est étoffée (Ollama + Continue,
Tabby…).

## 78. Amazon Q Developer : le choix AWS

**Amazon Q Developer** (ex-CodeWhisperer) : assistant pour VS Code et
JetBrains — complétion, revue de code, génération de doc et de tests, et
sa spécialité : les **montées de version Java** et l'intégration profonde
**AWS** (il connaît les API AWS par cœur).

Tarif indicatif : **~19 $/utilisateur/mois** (Pro), avec un palier gratuit
limité (50 interactions chat/mois…). **À vérifier** sur aws.amazon.com/q.

Pour qui : tu vis dans AWS (CDK, Lambda, IAM…) — aucun autre assistant ne
connaît aussi bien cet écosystème. Sinon, passe ton chemin.

## 79. JetBrains AI : pour les IDE JetBrains

**JetBrains AI** : l'assistant intégré à **tous les IDE JetBrains**
(IntelliJ, PyCharm…). Si tu codes en Python sous PyCharm (ton cas pour les
scripts d'inventaire !), c'est la voie native : complétion, chat, refactos
qui exploitent l'indexation sémantique de l'IDE (meilleure compréhension du
typage que les extensions VS Code).

Note : **Junie** est l'agent autonome de JetBrains (évoqué dans les
comparatifs 2026) — si tu es « team JetBrains », le couple IDE + JetBrains
AI + Junie est cohérent.

## 80. Zed : l'éditeur rapide qui branche tout le monde

**Zed** (éditeur open source, écrit en Rust — rapidité légendaire) mérite sa
place : via l'**ACP (Agent Client Protocol)**, tu peux y brancher **n'importe
quel agent externe** (Claude Code, OpenCode…). Philosophie : le meilleur
éditeur possible + **tes** agents, pas ceux de l'éditeur.

Pour qui : tu veux la vitesse et l'ouverture, et tu as déjà un CLI agentique
préféré. « L'éditeur à surveiller » selon les comparatifs 2026.

## 81. Windsurf, Trae, Antigravity : les éditeurs challengers

- **Windsurf** : fork VS Code « agent-first » (anciennement Codeium). Supporte
  MCP (limite : ~100 outils). Positionné entre Cursor et VS Code+extensions.
- **Trae** (ByteDance) : assistant adaptatif, **offre gratuite généreuse**
  historiquement (accès à des modèles frontier), paliers payants agressifs
  (Lite ~3 $/mois, Pro ~10 $/mois — à vérifier sur trae.ai). Intéressant
  pour tester à moindre coût.
- **Antigravity** (Google) : la plateforme « agent-first » de Google
  (antigravity.google), workspace multi-agents. Paliers Pro ~19,99 $/mois,
  Ultra ~100 $/mois (à vérifier). À suivre si tu es dans l'écosystème Google.

Règle : **ne multiplie pas les éditeurs**. Teste un challenger 2 semaines
maximum, puis tranche — chaque changement d'éditeur coûte des semaines de
productivité.

## 82. Aider : le CLI git-first pour les puristes

**Aider** (aider.chat) : CLI agentique en **Python**, **open source**, pensé
« git-first » : il **committe** automatiquement chaque étape avec des
messages propres, fonctionne avec ta clé API ou des modèles locaux, et
tourne même **entièrement offline**.

Pour qui : le sysadmin/dev qui vit dans le terminal, aime git plus que tout,
et veut un agent **sobre, prévisible, privé**. Moins « magique » que Claude
Code, mais d'une fiabilité appréciable pour des tâches bien définies.
Excellent pour apprendre la boucle agentique côté terminal (pendant de
Cline côté éditeur).

```bash
pip install aider-chat
export ANTHROPIC_API_KEY="sk-ant-fictive"
cd /tmp/lab-agent
aider --model sonnet --message "ajoute une fonction de retry avec backoff exponentiel"
```

## 83. Gemini CLI, Kiro, Devin, Jules : les autres à connaître

- **Gemini CLI** (Google) : le CLI agentique de Google, branché sur Gemini.
  Logique si tu es déjà client Google Cloud / amateur des longs contextes
  Gemini.
- **Kiro** (AWS) : l'IDE agentique d'AWS (fork VS Code) + **Kiro CLI**.
  Argument prix : accès aux modèles Claude via la facturation AWS, annoncé
  comme nettement moins cher que l'abonnement direct (ordre de grandeur
  évoqué : ~1/10 — **à vérifier**, dépend de ton contrat AWS).
- **Devin** (Cognition) : « l'ingénieur logiciel autonome » — pionnier des
  agents qui travaillent seuls des heures. Plans Pro ~20 $/mois, Max
  ~200 $/mois (à vérifier). Positionné « équipe virtuelle » plus
  qu'assistant.
- **Jules** (Google) : agent cloud **asynchrone** — tu lui assignes des
  tâches (web, CLI, API, labels GitHub), il travaille dans une VM Google
  Cloud et livre des **PR**, avec un agent « critique » qui relit avant de
  présenter. Offre gratuite : ~15 tâches/jour (à vérifier). Le « collègue
  virtuel » le plus mature côté cloud.

## 84. Récapitulatif plugins : que choisir selon le besoin

| Besoin | Choix 2026 | Pourquoi |
|---|---|---|
| Complétion pure, rapide | **Supermaven** ou Tab de Cursor | Vitesse, 0 friction |
| Complétion privée (code ne sort pas) | **Tabnine** / Continue + Ollama local | Local-first |
| Rester sur VS Code + OSS | **Continue** | BYOK, Apache 2.0 |
| Entreprise standardisée GitHub | **Copilot** | Intégration, prix, conformité |
| Écosystème AWS | **Amazon Q** | Connaissance AWS native |
| IDE JetBrains (PyCharm…) | **JetBrains AI** | Indexation sémantique |
| Éditeur rapide + mes agents | **Zed** + ton CLI | ACP, Rust, OSS |
| Tester à moindre coût | **Trae** (offre gratuite) | Généreux en free tier |
| CLI git-first sobre | **Aider** | Commits auto, offline possible |

## 85. Stack recommandée « Zelef » (sysadmin + RAG)

Une stack cohérente, sans doublon, calibrée pour ton profil :

```
COMPLÉTION ......... Supermaven (ou Copilot si déjà abonné GitHub)
ÉDITEUR/AGENT ...... Kilo Code (OSS) ou Cursor (confort) — PAS les deux
TERMINAL/CI ........ OpenCode (BYOK) + Codex CLI si ChatGPT Plus
APPRENTISSAGE ...... Cline (2 semaines, tout valider)
VIE PERSO/INFRA .... OpenClaw (durci, lecture seule d'abord)
```

Coût mensuel indicatif de cette stack : **0–40 $** selon les abonnements
déjà détenus (le détail par outil est dans chaque fiche prix).

---

# PARTIE E — TRANSVERSAL : BIEN UTILISER LES AGENTS

---

## 86. Grand comparatif final (septembre 2026)

| Critère | OpenCode | Claude Code | Codex CLI | Cursor | Cline | Kilo Code | OpenClaw |
|---|---|---|---|---|---|---|---|
| Type | CLI/TUI | CLI | CLI | Éditeur | Extension VS Code | Ext.+CLI | Agent perso |
| Licence | MIT | Fermé | Apache 2.0 | Propriétaire | MIT | MIT | MIT |
| Modèles | Tous (BYOK) | Claude only | GPT only | Multi + maison | BYOK | Tous (BYOK) | Tous (BYOK) |
| Prix outil | 0 € | 20 $/mois ou API | Inclus Plus 20 $ ou API | ~20 $/mois* | 0 € | 0 € | 0 € |
| Plan mode | Tab | Shift+Tab | /plan | Périmètre validé | Plan/Act | Architect | N/A (autonome) |
| MCP | Oui | Oui (client+serveur) | Oui (client) | Oui | Oui (manuel) | Oui (marketplace) | Oui |
| Skills | Oui | Oui | Non | Oui | Modes JSON | Oui (standard) | ClawHub |
| Scriptable CI | `opencode run` | `claude "…"` | **`codex exec`** | Cloud Agents | CLI 2.0 | `kilo` | Crons |
| Mémoire projet | AGENTS.md | CLAUDE.md/AGENTS.md | AGENTS.md | .cursor/rules | Modes perso | AGENTS.md | Mémoire persistante |
| Verrouillage | Aucun | Anthropic | OpenAI | Anysphere | Aucun | Aucun | Aucun |
| Idéal pour | Liberté modèle, terminal | Référence généraliste | Déjà abonné Plus, CI | Confort éditeur | Apprendre, contrôle | OSS multi-surfaces | Vie/infra autonome |

\* Cursor : grille restructurée en 2026 — **à vérifier** sur cursor.com/pricing.

## 87. Comment choisir selon le besoin (guide express)

```
BESOIN : écrire du code au quotidien dans un éditeur
└── Je veux le max de confort → Cursor
└── Je veux de l'OSS dans VS Code → Kilo Code (ou Cline pour apprendre)

BESOIN : automatiser en terminal / CI / cron
└── Déjà abonné ChatGPT Plus → Codex CLI (inclus, codex exec)
└── Je veux choisir mes modèles → OpenCode (BYOK)
└── Je veux LA référence, budget OK → Claude Code

BESOIN : comprendre et contrôler (phase d'apprentissage)
└── Côté éditeur → Cline (tout valider, 2 semaines)
└── Côté terminal → Aider (git-first, sobre)

BESOIN : un assistant de vie / une vigie infra
└── OpenClaw (durci, lecture seule d'abord)

BESOIN : juste de la complétion pendant la frappe
└── Supermaven / Copilot / Tabnine (ne paie QUE ça)
```

## 88. Matrice de décision : abonnement vs API (BYOK)

|  | Abonnement (20 $/mois type) | API au token (BYOK) |
|---|---|---|
| Facture | Fixe, prévisible | Variable, à surveiller |
| Plafond | « Fair use » flou, bridage possible | Pas de plafond, tu paies tout |
| Petit usage (< 1 h/sem) | Cher au final | **Gagnant** (quelques $) |
| Usage régulier quotidien | **Gagnant** (amorti vite) | 20–80 $/mois possibles |
| Usage en rafales (sprints) | Bridage possible au mauvais moment | **Gagnant** (paie les pics) |
| Multi-outils, 1 clé | Non (1 abo = 1 outil) | **Gagnant** (1 clé, N outils) |
| Tranquillité mentale | **Gagnant** | Alertes à configurer |

Règle : **commence en abonnement** (prévisible), **passe en BYOK** quand tu
sais mesurer ta conso (section 98) — ou quand tu veux une seule clé pour
plusieurs outils.

## 89. Usage en équipe : la charte minimale

Fais signer (même informellement) ces 10 règles avant de généraliser un
agent dans l'équipe :

1. **AGENTS.md versionné** dans chaque repo, relu comme du code.
2. **Plan mode obligatoire** au-delà de ~15 min de travail estimé.
3. **Aucun commit IA sans relecture humaine du diff** (section 100).
4. **Modèle par défaut d'équipe** + règle d'escalade (qui a le droit à Opus ?).
5. **Interdictions écrites** : secrets, prod, `rm -rf`, dépendances sauvages.
6. **Skills d'équipe** plutôt que prompts individuels qui se dupliquent.
7. **Budget** : enveloppe mensuelle par personne + alerte à 80 %.
8. **Confidentialité** : quels repos ont le droit d'être indexés/envoyés où.
9. **Journal** : l'agent résume chaque feature dans `docs/decisions/`.
10. **Un seul écrivain** : un agent en écriture à la fois par repo.

## 90. Revue du code généré : la méthode

Le code d'un agent se relit **comme celui d'un prestataire brillant mais
junior** : vite, bien, et avec des angles morts.

```
REVUE EN 4 PASSES (10–15 min pour une feature moyenne) :

PASSE 1 — Le diff brut
  $ git diff --stat          # l'ampleur : >500 lignes = méfiance
  $ git diff                 # lire CHAQUE ligne, pas en diagonale

PASSE 2 — Les tests
  $ pytest -q                # vert ? VRAIMENT vert (pas de tests skippés en douce)
  # Cherche : assert True, tests vides, mocks qui mockent le bug

PASSE 3 — Les angles morts de l'IA
  - Gestion d'erreurs : que se passe-t-il si le réseau tombe ? le fichier manque ?
  - Secrets : une clé en dur ? un mot de passe en clair dans un log ?
  - Dépendances : une lib ajoutée sans raison ? (pip freeze avant/après)
  - Sécurité : injection (SQL, shell), permissions de fichiers, sudo

PASSE 4 — Le subagent relecteur (double filet)
  > utilise le subagent reviewer sur cette branche
  # Il voit ce que ton œil fatigué rate. Toi tu tranches.
```

Signaux d'alerte (« à jeter et recommencer ») :

- Le diff touche des fichiers hors périmètre sans justification.
- Des tests ont été **affaiblis** pour passer (seuils baissés, cas supprimés).
- Du code dupliqué au lieu d'une fonction (l'agent a la flemme de refactorer).
- Des commentaires qui décrivent un comportement **différent** du code.

## 91. Conventions : faire écrire l'agent « comme l'équipe »

L'agent n'a pas de goût — il a ton AGENTS.md. Exemple de conventions qui
changent tout (à adapter) :

```markdown
# AGENTS.md — conventions d'équipe (extrait)

## Style
- Python : ruff, 100 colonnes max, type hints obligatoires.
- Nommage : français métier (onduleur, baie), anglais technique (retry, timeout).
- Commentaires : le POURQUOI, jamais le QUOI.

## Messages de commit (conventionnel)
- `feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`
- Ex. : `feat: ajoute le retry SNMP avec backoff exponentiel`

## Tests
- Un fichier test_*.py par module, pytest, couverture > 80 % sur le nouveau code.
- Interdit : `assert True`, tests sans assertion, `time.sleep` dans les tests.

## Doc
- Toute nouvelle fonction publique : docstring + exemple.
- README mis à jour si l'usage change.
```

Astuce : fais générer **par l'agent** la première version de ces conventions
(« propose-moi un AGENTS.md à partir de nos 3 derniers projets »), puis
**corrige-la à la main**. C'est plus rapide que partir de zéro, et la
correction t'oblige à expliciter tes vrais standards.

## 92. Sécurité (1/3) : ce qu'on ne confie JAMAIS à un agent

Liste non négociable — à afficher au-dessus du poste :

1. 🔒 **Les secrets** : mots de passe, clés API, clés SSH, tokens. L'agent
   travaille avec des **exemples fictifs** et des `.env.example`. Un secret
   collé dans un prompt part chez le fournisseur et dort dans les logs de
   session.
2. 🔒 **La production sans garde-fou** : aucun agent ne touche la prod
   directement. Le chemin, c'est : agent → branche → PR → revue humaine →
   CI → déploiement contrôlé. Pas d'exception « c'est urgent ».
3. 🔒 **L'exécution aveugle** : `--dangerously-skip-permissions`,
   `--ask-for-approval never`, Full-auto… ces interrupteurs existent pour
   des sandboxes jetables, pas pour ton poste de travail.
4. 🔒 **Le réseau sans contrôle** : un agent qui peut `curl` peut exfiltrer.
   En phase de test, coupe le réseau (sandbox) quand la tâche ne l'exige pas.
5. 🔒 **Les suppressions** : `rm -rf`, `DROP`, écrasement de fichiers —
   toujours avec validation explicite, jamais en auto.
6. 🔒 **L'installation de dépendances** : `pip install`, `npm install`, PPA
   obscurs… chaque nouvelle dépendance = une chaîne d'approvisionnement à
   valider par un humain (typosquatting, paquets malveillants).
7. 🔒 **Les décisions irréversibles** : rotation de clés, suppression de
   sauvegardes, changement de pare-feu — l'agent **propose**, l'humain
   **exécute**.

## 93. Sécurité (2/3) : durcir chaque outil

| Outil | Réglage clé | Où |
|---|---|---|
| Claude Code | Permissions par défaut (pas `--dangerously-skip-permissions`), hooks de garde | `~/.claude/settings.json`, CLAUDE.md |
| Codex CLI | Rester en **Suggest/Auto-edit**, Full-auto = sandbox | flags `--ask-for-approval` |
| OpenCode | Agent **Plan** par défaut (Tab), modèles figés | `opencode.json` |
| Cursor | Privacy mode, règles `.cursor/rules/securite.mdc` | Settings + repo |
| Cline | Auto-approve : lecture seule auto, **tout le reste manuel** | Panneau Cline |
| Kilo Code | Permissions allow/ask/deny, worktrees pour isoler | `kilo.jsonc` |
| OpenClaw | Bind 127.0.0.1, user sans sudo, crons lecture seule | config + checklist section 70 |
| Tous | `.gitignore` béton, `git status` propre avant session | repo |

Le **fichier `.gitignore` de combat** (à copier dans chaque repo) :

```gitignore
# Secrets — JAMAIS versionnés, JAMAIS sous les yeux d'un agent
.env
.env.*
*.pem
*.key
id_rsa*
*.p12
*credentials*
*secrets*

# Système & build
__pycache__/
node_modules/
.venv/
*.log
```

Et le **bloc « sécurité » à mettre dans TOUS tes AGENTS.md** :

```markdown
## Sécurité (non négociable)
- Ne lis, ne crée, ne modifie JAMAIS de fichier contenant un secret.
  Utilise .env.example et des valeurs fictives (ex. : "motdepasse-fictif").
- Ne lance JAMAIS : rm -rf, sudo, pip install, curl | bash, sans validation
  humaine explicite dans cette session.
- N'envoie JAMAIS de données hors de la machine (pas de curl/post vers
  l'extérieur) sauf demande explicite.
- Avant toute action destructive, affiche ce que tu vas faire et attends "go".
```

## 94. Sécurité (3/3) : confidentialité des données et entraînement

Ton code **voyage** : chaque prompt part chez le fournisseur du modèle. Ce
qu'il faut savoir en 2026 :

- **Claude Code / Anthropic** : politique de non-entraînement sur les
  données API par défaut (l'API n'entraîne pas) — mais vérifie les
  conditions « à vérifier » le jour J, elles ont déjà changé par le passé.
- **Modèles gratuits / tiers gratuits** (offres « free » d'OpenRouter, Kilo,
  Trae…) : la règle quasi générale est que **les prompts gratuits peuvent
  servir à l'entraînement** — c'est le prix du gratuit. Ne jamais y mettre
  de code sensible ou client.
- **Cursor** : indexation du repo sur **son** infrastructure (cloud only).
  Le privacy mode promet « rien stocké », mais le code transite par leurs
  serveurs. Pour du code soumis au secret (client, R&D), tranche
  explicitement avant d'indexer.
- **BYOK via API** : chez la plupart des fournisseurs, l'usage **API payant**
  n'alimente pas l'entraînement — c'est l'argument confidentialité n°1 du
  BYOK (OpenCode, Cline, Continue en API).
- **100 % local (Ollama)** : rien ne sort. Le seul niveau « secret défense »,
  au prix d'un modèle moins brillant.

Checklist confidentialité par projet (à cocher) :

- [ ] Classification du repo : public / interne / sensible / secret
- [ ] « Sensible » et au-dessus : **pas d'offre gratuite**, API payante ou local
- [ ] Indexation cloud (Cursor) : oui/non, décidé et noté
- [ ] Logs de session (`~/.claude/`, `~..codex/`) : contiennent du code en
      clair → exclus des backups non chiffrés, nettoyés périodiquement

## 95. Coût et contrôle des tokens (1/2) : comprendre la facture

**Anatomie d'une session** : chaque tour de boucle agentique renvoie
l'historique + les nouveaux outils au modèle. Une session, c'est :

```
input  = prompt + fichiers lus + sorties d'outils + historique (grossit à chaque tour)
output = raisonnement + code généré
```

Ordres de grandeur (modèles « Sonnet/GPT-5 »-like, sept. 2026 — **à
vérifier**, les tarifs bougent) :

| Session type | Tokens input | Tokens output | Coût API indicatif |
|---|---|---|---|
| Question simple + 2 fichiers | ~10 k | ~2 k | **~0,05 $** |
| Petite feature + tests | ~100 k | ~20 k | **~0,50–1 $** |
| Feature moyenne (30 min) | ~500 k | ~80 k | **~2–5 $** |
| Grosse session autonome (1–2 h) | 2–5 M | 300–800 k | **~10–40 $** |

Le **cache de prompt** change tout : les fournisseurs facturent le
*cache read* ~10× moins cher que l'input normal. Les bons agents (Claude
Code en tête) cachent automatiquement le contexte stable (ton AGENTS.md,
les gros fichiers relus) — d'où l'intérêt de sessions qui **relisent les
mêmes fichiers** plutôt que d'en ouvrir 50 différents.

## 96. Coût et contrôle des tokens (2/2) : les 10 leviers d'économie

1. **Modèle adapté à la tâche** : petit modèle pour explorer/relire, gros
   pour coder. Le levier n°1 (OpenCode/Kilo le font nativement).
2. **Plan mode** : 1 session de plan à 0,50 $ évite 3 sessions de
   réécriture à 5 $.
3. **Prompts avec critères d'arrêt** : « arrête-toi quand les tests passent ».
4. **Donne les chemins exacts** : `@app/main.py:120` plutôt que « trouve où… »
   (chaque recherche exploratoire = des dizaines de k tokens).
5. **Sessions courtes** : `/compact` ou nouvelle session — un contexte
   obèse coûte cher ET dégrade la qualité.
6. **Limite le périmètre** : « ne touche qu'à `app/` » bat « améliore le projet ».
7. **Désactive les MCP inutiles** : chaque serveur = des outils décrits à
   chaque tour = des tokens.
8. **Sois concis dans les instructions système** (« réponds brièvement » dans
   AGENTS.md fait baisser l'output).
9. **Surveille** : `/cost` (Claude Code), `/status` (Codex), dashboard
   fournisseur, **alertes de facturation à 50/80/100 %**.
10. **Abonnement vs BYOK** : recalcule tous les 3 mois avec TES chiffres
    (section 88) — le bon choix change avec ton usage.

**Budget mensuel type** (usage régulier, 1 personne) :

| Profil | Coût mensuel indicatif |
|---|---|
| Léger (questions, petits scripts) | **0–10 $** (BYOK ou Hobby) |
| Régulier (features hebdo) | **20 $** (1 abonnement) |
| Intensif (agent quotidien, CI) | **40–100 $** (abo + API) |
| Équipe de 5 | **150–400 $** (sièges + API) — à piloter (section 89) |

## 97. MCP en pratique : ton premier serveur utile

Le serveur MCP le plus rentable pour toi : **filesystem sur ton corpus de
docs** (tes guides RAG). Tous tes agents pourront alors citer ta doc.

```bash
# Serveur MCP officiel "filesystem" (Node.js)
npx -y @modelcontextprotocol/server-filesystem /home/zelef/docs
```

Branchement par outil :

```bash
# Claude Code
claude mcp add docs -- npx -y @modelcontextprotocol/server-filesystem /home/zelef/docs

# OpenCode — dans opencode.json : clé "mcp" (voir section 13)

# Cline — cline_mcp_settings.json (voir section 55)

# Cursor — .cursor/mcp.json : { "mcpServers": { "docs": { "command": "npx", "args": [...] } } }

# Codex — ~/.codex/config.toml (voir section 35)
```

Idées de serveurs MCP pour un sysadmin (tous existent dans l'écosystème) :

- **filesystem** : tes docs, tes playbooks (lecture).
- **git** : historique, blame, diff sans quitter l'agent.
- **postgres/sqlite** : interroger l'inventaire (« combien d'onduleurs en
  alarme ? » → l'agent fait le SELECT lui-même).
- **snmp/nut** : via un petit serveur maison — l'agent interroge tes
  onduleurs (lecture seule !).
- **prometheus/grafana** : « pourquoi ce pic ? » → l'agent lit les métriques.

> 🔒 MCP = l'agent obtient des **pouvoirs**. Chaque serveur en écriture
> (base de données, API d'admin) est une surface d'attaque : commence par
> des serveurs **lecture seule**, et audite ce que chaque outil fait
> réellement avant de l'auto-approuver.

## 98. Skills : capitaliser ton savoir-faire

Une skill, c'est un **prompt packagé et versionné**. Quand tu te surprends à
réécrire le même long prompt pour la 3ᵉ fois, c'en est une.

```bash
# Claude Code
.claude/skills/revue-ansible/SKILL.md

# Kilo Code (standard ouvert agentskills.io)
.agents/skills/revue-ansible/SKILL.md

# OpenClaw
# via ClawHub ou dossier de skills local
```

Exemple — ta skill « revue Ansible » :

```markdown
# SKILL.md — revue-ansible
---
name: revue-ansible
description: Relit un playbook Ansible selon nos standards avant merge
---

Quand on te demande de relire un playbook :
1. Vérifie : idempotence (chaque tâche rejouable sans effet de bord),
   pas de mot de passe en clair (vault obligatoire), handlers notifiés,
   `become` minimal.
2. Signale tout `shell:` qui pourrait être un module natif.
3. Rends un verdict : BLOQUANT / REMARQUES / OK, avec les lignes concernées.
4. Ne modifie jamais le playbook : tu proposes, l'humain applique.
```

> 💡 Pour ton RAG : tes skills (« comment ingérer un doc », « comment
> nettoyer un corpus ») sont des artefacts versionnables au même titre que
> tes guides. Un jour, ton RAG répondra ET tes skills agiront — même socle.

## 99. Les 20 pièges classiques (et comment les éviter)

**1. La boucle infinie** — l'agent relit, réécrit, relance les tests en
boucle sans progresser.
→ *Antidote* : critères d'arrêt dans le prompt (« max 5 itérations, puis
résume et rends la main ») ; `Esc` dès que tu vois le 3ᵉ tour identique.

**2. L'hallucination de dépendance** — l'agent importe une librairie qui
n'existe pas (ou un paquet typosquatté).
→ *Antidote* : interdiction d'ajouter des dépendances sans validation
(AGENTS.md) ; `pip install` toujours manuel.

**3. Le test qui ment** — l'agent affaiblit le test pour qu'il passe
(seuil baissé, assertion supprimée).
→ *Antidote* : passe 3 de la revue (section 90) ; `git diff` sur les tests
aussi.

**4. Le refactor surprise** — « pendant que j'y étais, j'ai aussi
réorganisé… » 800 lignes hors périmètre.
→ *Antidote* : périmètre explicite dans le prompt ; `git diff --stat`
systématique.

**5. Le secret qui fuit** — clé API collée dans le prompt, `.env` lu et
recopié dans un exemple.
→ *Antidote* : `.env.example` + valeurs fictives ; jamais de secret sous
les yeux de l'agent (section 92).

**6. Le `rm -rf` confiant** — script de nettoyage généré avec un chemin
mal quoté ou une variable vide.
→ *Antidote* : jamais d'auto-approve sur les suppressions ; `set -u` dans
les scripts shell générés.

**7. La prod touchée « pour tester »** — l'agent confond l'inventaire de
test et la base de prod (même nom de variable !).
→ *Antidote* : environnements nommés différemment, connexions prod hors de
portée de l'agent.

**8. Le contexte obèse** — session de 2 h, l'agent oublie le début et
contredit ses propres décisions.
→ *Antidote* : `/compact`, sessions courtes, résumé écrit des décisions.

**9. Le « oui » systématique** — l'agent valide tes mauvaises idées au lieu
de les contester.
→ *Antidote* : demande explicitement la contradiction (« challenge mon
approche avant d'implémenter ») ; Claude Code le fait mieux que les autres.

**10. La doc qui pourrit** — l'agent met à jour le code mais pas le README,
ou écrit une doc qui décrit un comportement imaginaire.
→ *Antidote* : « mets à jour la doc **à partir du code final**, pas de
mémoire » + relecture.

**11. Le copier-coller multiplié** — plutôt que factoriser, l'agent duplique
un bloc 5 fois avec des variantes.
→ *Antidote* : « DRY : factorise avant de dupliquer » dans AGENTS.md ;
passe revue n°3.

**12. L'anglais/français mélangé** — commentaires franglais, messages de
commit incohérents.
→ *Antidote* : règle de langue explicite dans AGENTS.md (section 91).

**13. Le modèle sous-dimensionné** — tâche complexe confiée au modèle
« flash » pour économiser 2 $, résultat à jeter (coût réel : 1 h perdue).
→ *Antidote* : petit modèle = exploration, gros modèle = implémentation.

**14. Le MCP trop puissant** — serveur MCP en écriture auto-approuvé, l'agent
écrit en base « pour tester ».
→ *Antidote* : MCP lecture seule par défaut (section 97).

**15. Le multi-agent qui se marche dessus** — deux agents éditent le même
fichier, le dernier écrase le premier.
→ *Antidote* : un seul écrivain par dossier (section 61) ; worktrees pour
le parallélisme (Kilo).

**16. La facture surprise** — 3 sessions « juste pour voir » sur le gros
modèle en API = 60 $.
→ *Antidote* : alertes de facturation + `/cost` + modèle pas cher par défaut.

**17. Le skill/plugin vérolé** — skill ClawHub ou plugin VS Code malveillant
(prompt injection, exfiltration).
→ *Antidote* : sources connues uniquement, lecture du code/description,
post-ClawHavoc : paranoïa saine (sections 66, 72).

**18. La confiance aveugle dans le vert** — « les tests passent » alors que
l'agent a mocké la fonction testée.
→ *Antidote* : lis les tests générés comme le code (section 90, passe 2).

**19. Le vendor lock-in silencieux** — 6 mois de règles `.cursor/`, de
skills `.claude/` et d'habitudes : changer d'outil coûte une blinde.
→ *Antidote* : **AGENTS.md comme source de vérité** (standard multi-outils),
skills au format ouvert quand possible.

**20. L'atrophie** — à force de déléguer, tu ne sais plus écrire le code que
tu supervises. Pour un chef de service responsable d'une prod, c'est le
piège le plus grave.
→ *Antidote* : impose-toi des plages « sans agent » ; fais expliquer par
l'agent (mode Ask) au lieu de faire faire ; relis vraiment les diffs.

## 100. Cas pratique n°1 : script de supervision d'onduleur (commenté)

Le scénario sysadmin par excellence : un check SNMP d'onduleur, format
Nagios, **avec** les garde-fous.

```bash
mkdir -p /tmp/lab-agent/ups && cd /tmp/lab-agent/ups && git init -q
cat > AGENTS.md <<'EOF'
# AGENTS.md — check_ups
- Python 3.12, zéro dépendance externe (stdlib uniquement).
- SNMP : paramètres via variables d'environnement (UPS_HOST, UPS_COMMUNITY),
  valeurs fictives dans les exemples et les tests.
- Sortie : code retour Nagios (0/1/2/3) + 1 ligne de texte.
- Tests : pytest, 100 % des branches critiques.
- INTERDIT : écrire un vrai hostname ou une vraie communauté, même en exemple.
EOF
```

Prompt (Claude Code, plan mode) :

```
[Plan mode] Besoin : check_ups.py — interroge un onduleur en SNMP
(OID fictifs documentés dans le README), sort un code Nagios selon
l'état batterie (0 OK / 1 WARNING <50 % / 2 CRITICAL <25 % / 3 UNKNOWN
si SNMP injoignable). Contraintes : stdlib uniquement, timeout SNMP
de 5 s, aucun secret en dur, tests pytest avec un faux serveur SNMP.
Propose le plan (fichiers, fonctions, cas de test). N'écris rien.
```

Ce que tu valides dans le plan : le découpage (collecte / évaluation /
sortie), les cas limites (timeout, OID absent), les tests (faux SNMP, pas
de vrai équipement). Puis exécution, puis revue en 4 passes (section 90).
**Le point pédagogique** : sur un sujet que tu maîtrises (les onduleurs),
tu détectes immédiatement les approximations de l'agent — c'est comme ça
qu'on calibre sa confiance.

## 101. Cas pratique n°2 : revue d'un playbook Ansible par l'agent

```bash
cd /tmp/lab-agent && mkdir -p ansible && cd ansible && git init -q
# Place un playbook existant (expurgé de tout secret !) dans playbook.yml
```

Prompt (Codex CLI, non-interactif — la voie CI) :

```bash
codex exec --full-auto \
  "Relis playbook.yml avec la skill implicite suivante : 1) idempotence
   de chaque tâche, 2) aucun secret en clair (signale toute variable
   suspecte), 3) tout 'shell:' remplaçable par un module natif,
   4) handlers correctement notifiés, 5) 'become' minimal.
   Rends un rapport markdown : BLOQUANT / REMARQUES / OK par tâche.
   Ne modifie AUCUN fichier."
```

Ensuite, en CI (GitHub Actions) : ce `codex exec` tourne à chaque PR qui
touche `ansible/` — ton **relecteur automatique** (voir l'exemple YAML
section 34). Le verdict reste humain, mais les oublis bêtes sont attrapés
par la machine.

## 102. Cas pratique n°3 : ingestion RAG assistée (ton chantier)

Ton pipeline RAG (collecte → nettoyage → indexation) est un terrain de jeu
idéal pour un agent **en lecture/écriture cadrée** :

```
Projet : ~/rag-pipeline (fictif, à adapter)
Agent : Claude Code (serveur MCP 'docs' branché sur ton corpus)

Session type :
> [Plan mode] Dans corpus/brut/, identifie les 10 fichiers avec le plus
> de résidus (headers/footers, sommaires, HTML) en échantillonnant.
> Propose une stratégie de nettoyage par type de résidu. N'écris rien.

> Implémente la stratégie validée sous forme de scripts dans tools/clean/,
> un script par type de résidu, avec tests. Ne touche JAMAIS à corpus/brut/
> (lecture seule) : les sorties vont dans corpus/propre/.
```

Garde-fous spécifiques RAG :

- **Source brute intouchable** (lecture seule, idéalement en snapshot).
- **Idempotence** : relancer le nettoyage donne le même résultat.
- **Échantillonnage de contrôle** : l'agent montre 5 exemples avant/après
  avant de traiter 10 000 fichiers.
- **Traçabilité** : chaque fichier propre garde la référence de sa source.

## 103. Cas pratique n°4 : débogage assisté (méthode)

Quand quelque chose casse et que tu veux l'agent en **détective**, pas en
chirurgien :

```
> [Mode Ask / Plan] Le service X échoue au démarrage depuis ce matin.
> Indices : journalctl -u X (je te le colle ci-dessous), config dans
> /etc/X/ (lisible). NE MODIFIE RIEN. Pose-moi des questions si un
> indice manque, puis propose 3 hypothèses classées par probabilité
> avec, pour chacune, LA commande qui la confirmerait.
[coller les logs]
```

Pourquoi ça marche : tu forces l'agent à **raisonner avant d'agir**, tu
gardes la main sur les commandes (c'est toi qui les lances), et tu obtiens
un raisonnement écrit — utile pour ton rapport d'incident.

## 104. Cas pratique n°5 : documentation d'un parc (batch nocturne)

```bash
# Cron hebdo : régénère la doc d'inventaire à partir des données
0 2 * * 0  cd /home/zelef/inventaire && \
  opencode run -m openrouter/qwen/qwen3-coder-flash \
  "à partir de data/inventaire.csv, régénère docs/parc.md (tableaux par
   site, alertes de fin de garantie). Ne modifie rien d'autre." \
  && git diff --exit-code -- docs/ || \
  git commit -am "docs: maj auto parc $(date +%F)"
```

Points de vigilance : modèle **pas cher** (tâche mécanique), `git diff`
pour ne commiter que si ça a changé, **relecture humaine le lundi matin**
(le cron propose, tu disposes). Jamais de push auto vers `main` sans
relecture — le commit local suffit, tu pousses après contrôle.

## 105. Glossaire (les 30 termes à connaître)

| Terme | Définition courte |
|---|---|
| Agent | Programme qui boucle : raisonne → appelle des outils → observe → recommence |
| Boucle agentique | Le cycle raisonnement → action → observation jusqu'à l'objectif |
| Contexte | Tout ce que le modèle « voit » d'un coup (fenêtre limitée, en tokens) |
| Token | Unité de facturation (≈ 4 caractères anglais) ; input et output tarifés séparément |
| MCP | Standard ouvert pour brancher des outils/serveurs sur un agent |
| Skill | Savoir-faire packagé et réutilisable (dossier + instructions) |
| Subagent | Agent lancé par l'agent principal pour une sous-tâche, avec son propre contexte |
| AGENTS.md | Fichier d'instructions projet lu par (presque) tous les agents |
| CLAUDE.md | Équivalent propriétaire chez Claude Code (généré par `/init`) |
| Plan mode | Phase d'exploration/proposition sans écriture, avant exécution |
| BYOK | Bring Your Own Key : tu branches ta clé API au lieu de l'abonnement |
| Sandbox | Environnement d'exécution restreint (pas de réseau, pas d'écriture hors périmètre…) |
| Permission | Ce que l'agent peut faire sans demander (lire/écrire/exécuter) |
| Auto-approve | Validation automatique par type d'action (à régler avec prudence) |
| TUI | Interface texte dans le terminal (ex. : OpenCode) |
| Complétion | Suggestion de code pendant la frappe (Tab) |
| Composer | Mode multi-fichiers de Cursor (décris → il applique sur N fichiers) |
| Background Agent | Agent qui tourne sans bloquer ton éditeur (Cursor) |
| Cloud Agent | Agent qui tourne sur l'infra du fournisseur, pas sur ta machine |
| Worktree | Clone git léger d'une branche dans un dossier séparé (parallélisme) |
| Checkpoint | Point de restauration avant une action d'agent (annulation d'urgence) |
| Cache de prompt | Contexte stable facturé ~10× moins cher (input en cache) |
| Fair use | Limite d'usage « raisonnable » des abonnements, souvent floue |
| Crédits | Unité de facturation interne (Cursor, Kilo Gateway…) |
| ClawHub | Registre communautaire de skills pour OpenClaw |
| Prompt injection | Attaque : des instructions malveillantes cachées dans des données lues par l'agent |
| Typosquatting | Paquet malveillant au nom proche d'un paquet légitime |
| ACP | Agent Client Protocol (Zed) : brancher n'importe quel agent externe à l'éditeur |
| Headless | Mode sans interface, pour scripts/CI (`codex exec`, `opencode run`) |
| Idempotence | Propriété : rejouer l'action ne change rien la 2ᵉ fois (crucial en Ansible) |

## 106. Pense-bête de poche (à imprimer)

```
┌─────────────────────────────────────────────────┐
│           AGENTS CODEURS — PENSE-BÊTE           │
├─────────────────────────────────────────────────┤
│ AVANT CHAQUE SESSION                            │
│ □ git status propre (commit avant)              │
│ □ .env / secrets hors de portée                 │
│ □ AGENTS.md à jour                              │
│                                                 │
│ PENDANT                                         │
│ □ Plan d'abord (Tab / Shift+Tab / /plan)        │
│ □ Un seul écrivain par dossier                  │
│ □ Esc dès que ça dérive                         │
│ □ /cost ou /status de temps en temps            │
│                                                 │
│ APRÈS                                           │
│ □ git diff relu ligne par ligne                 │
│ □ pytest vert (vrai vert)                       │
│ □ Pas de secret, pas de dépendance sauvage      │
│ □ Commit avec message conventionnel             │
│                                                 │
│ INTERDIT (sauf validation explicite)            │
│ ✗ rm -rf / sudo / pip install / curl|bash       │
│ ✗ --dangerously-skip-permissions / Full-auto    │
│ ✗ Secrets dans les prompts                     │
│ ✗ Toucher la prod directement                   │
│                                                 │
│ INSTALLS (sept. 2026)                           │
│ $ curl -fsSL https://opencode.ai/install | bash │
│ $ npm i -g @anthropic-ai/claude-code            │
│ $ npm i -g @openai/codex                        │
│ $ npm i -g @kilocode/cli                        │
│ Extensions : Cline / Kilo Code / Continue       │
│ Éditeur : cursor.com/download                   │
└─────────────────────────────────────────────────┘
```

## 107. Checklist : ta première semaine avec les agents

- [ ] **Jour 1** : prérequis (section 5) + bac à sable `/tmp/lab-agent`.
- [ ] **Jour 1** : installe **UN** outil (recommandé : Claude Code ou OpenCode).
- [ ] **Jour 2** : `/init`, corrige le AGENTS.md/CLAUDE.md généré (section 6).
- [ ] **Jour 2** : workflow plan-d'abord sur une micro-feature (sections 12/24).
- [ ] **Jour 3** : installe **Cline**, travaille 1 journée en « tout valider ».
- [ ] **Jour 4** : configure le pense-bête sécurité (sections 92–93) dans ton AGENTS.md.
- [ ] **Jour 4** : active les **alertes de facturation** chez ton fournisseur.
- [ ] **Jour 5** : premier vrai usage (petit script utilitaire), revue en 4 passes (section 90).
- [ ] **Jour 5** : fais le **quiz** (section 112) — vise 8/10 minimum.
- [ ] **Jour 6–7** : écris ta première **skill** (le prompt que tu as réécrit 3 fois).

## 108. Pour aller plus loin

**Documentation officielle (références à jour — préfère-les aux tutos) :**

- OpenCode : opencode.ai/docs (config, plugins, modèles)
- Claude Code : docs.anthropic.com (CLI, hooks, MCP, skills)
- Codex : github.com/openai/codex (open source — lis le code !) + learn.chatgpt.com/docs (auth, pricing)
- Cursor : cursor.com/docs + cursor.com/pricing (vérifie la grille en vigueur)
- Kilo Code : kilo.ai/docs (kilo.jsonc, Agent Manager, CLI)
- Cline : docs.cline.bot (modes, MCP, auto-approve)
- OpenClaw : openclaw.ai + github.com/openclaw/openclaw (README, sécurité)
- Continue : docs.continue.dev (config.yaml, modèles locaux)
- MCP : modelcontextprotocol.io (le standard, ses serveurs officiels)

**Pour creuser :**

- Le dépôt **openai/codex** (Apache 2.0) : lire le code d'un vrai agent est
  la meilleure école d'architecture agentique.
- Les **awesome-lists** : awesome-ai-coding-tools, awesome-openclaw (panorama
  communautaire, à trier).
- **SWE-bench** : le benchmark de référence des agents codeurs — pour
  comprendre ce que « bon en code » veut dire mesurablement.
- Tes propres **logs de session** (`~/.claude/`, `~/.codex/`) : relis une
  session d'il y a 15 jours, note ce que tu ferais différemment — c'est là
  que naissent tes skills.

**Prochaines étapes pour ton profil :**

1. Serveur MCP « docs » sur ton corpus RAG (section 97) — le pont entre tes
   guides et tes agents.
2. Skill « revue-ansible » versionnée (section 98).
3. `codex exec` en CI sur tes playbooks (sections 34, 101).
4. OpenClaw durci en vigie (lecture seule) sur une machine dédiée (sections
   67–72).

## 109. Ce que ce guide ne couvre pas (honnêteté)

- Les **tarifs exacts au centime** : ils changent tous les trimestres —
  les pages pricing officielles font foi (liens section 108).
- Les **versions précises** : le secteur publie chaque semaine ; un numéro
  de version imprimé serait faux dans un mois.
- Le **fine-tuning** de modèles pour le code : autre sujet, autre guide.
- Les aspects **juridiques** fins (licence du code généré, responsabilité) :
  à voir avec un juriste pour un usage professionnel encadré — ce guide
  donne les bons réflexes, pas un avis juridique.
- Les outils **« NON TROUVÉ »** : aucun dans cette édition — tous les noms
  de la mission ont été identifiés (OpenClaw pour « open clow »).

## 110. Quiz : 10 questions (réponses en fin de section)

**Q1.** Quelle commande installe OpenCode sur Linux/macOS (méthode officielle) ?
**Q2.** Dans Claude Code, que fait `Shift+Tab` et pourquoi est-ce le réflexe n°1 ?
**Q3.** Cite les trois modes de permission de Codex CLI et celui à utiliser en découverte.
**Q4.** Quelle est la différence de licence entre le CLI de Claude Code et celui de Codex ?
**Q5.** Dans Cursor, à quoi servent respectivement Tab, Composer et l'Agent ?
**Q6.** Pourquoi Cline est-il recommandé comme « école » des agents ?
**Q7.** « Open clow » n'existe pas : quel est le vrai nom de l'outil, et quelle attaque de 2026 impose de le durcir ?
**Q8.** Cite trois choses à ne JAMAIS confier à un agent (section 92).
**Q9.** Un abonnement à 20 $/mois ou du BYOK au token : dans quel cas le BYOK gagne-t-il ?
**Q10.** Qu'est-ce que le « test qui ment » (piège n°3) et comment le détecter ?

---

**Réponses :**

**R1.** `curl -fsSL https://opencode.ai/install | bash` (ou `npm install -g opencode-ai`).
**R2.** `Shift+Tab` bascule en **Plan mode** : l'agent explore et propose sans écrire. C'est le réflexe n°1 car valider un plan coûte des centimes, alors que corriger une mauvaise implémentation coûte des dollars et du temps.
**R3.** **Suggest** (propose, tu valides tout) / **Auto-edit** (édite seul, demande pour exécuter) / **Full-auto** (tout seul). En découverte : **Suggest**.
**R4.** Le CLI de Claude Code est **fermé** (propriétaire) ; Codex CLI est **open source Apache 2.0**.
**R5.** **Tab** = complétion inline pendant la frappe ; **Composer** = applique une description sur plusieurs fichiers (avec diffs à valider) ; **Agent** = exécute une tâche multi-étapes de façon autonome (avec background/cloud).
**R6.** Parce que sa boucle **Plan/Act** montre et fait valider **chaque action** par défaut : en deux semaines de « tout valider », on comprend la boucle agentique mieux qu'en deux mois de pilotage automatique.
**R7.** Le vrai nom est **OpenClaw** (ex-Clawdbot/Moltbot). L'attaque **ClawHavoc** (début 2026 : skills malveillantes, instances exposées, RCE) impose bind local, utilisateur sans sudo, skills auditées, crons lecture seule d'abord.
**R8.** (au choix parmi) : les **secrets** ; la **production sans garde-fou** ; l'**exécution aveugle** (skip-permissions/Full-auto) ; le réseau sans contrôle ; les suppressions auto ; l'installation de dépendances non validées ; les décisions irréversibles.
**R9.** Le BYOK gagne quand : usage **faible ou en rafales** (on paie ce qu'on consomme), ou quand on veut **une seule clé pour plusieurs outils**. L'abonnement gagne pour un usage quotidien régulier et prévisible.
**R10.** L'agent **affaiblit le test** au lieu de corriger le code (seuil baissé, assertion supprimée, cas retiré). Détection : passe 2 de la revue — lire le diff **des tests** aussi, traquer `assert True`, tests vides, mocks suspects.

**Score** : 8/10 → tu peux passer à la pratique (section 107). Moins de 8 → relis les sections indiquées entre parenthèses.

## 111. Historique des vérifications (transparence)

| Date | Vérification |
|---|---|
| 27 sept. 2026 | Existence, installation, prix d'OpenCode, Kilo Code, Claude Code, Codex CLI, Cursor, Cline, OpenClaw, Continue, Supermaven — par recherche web |
| 27 sept. 2026 | Points d'incertitude marqués « à vérifier » : grille Cursor (restructuration 2026), offre OpenCode Zen/Go, tarifs API exacts, syntaxes de config mineures |
| — | Prochaine re-vérification conseillée : **janvier 2027** (ou à chaque changement d'abonnement) |

## 112. Le mot de la fin

Un agent codeur ne remplace ni ton expérience de sysadmin, ni ta
responsabilité de chef de service. Il **multiplie** ce que tu sais déjà
faire — et il multiplie aussi tes erreurs si tu le laisses sans cadre.

Le cadre tient en trois artefacts que ce guide t'a fait construire :

1. **`AGENTS.md`** — ton contrat avec la machine, versionné, relu.
2. **La revue en 4 passes** — ton contrôle qualité, systématique.
3. **Le pense-bête** — tes garde-fous, imprimés au-dessus du poste.

Avec ça, tu n'es plus « un mec qui regarde l'IA coder ». Tu es un
**pilote d'agents** : tu fixes le cap (le plan), tu tiens le manche (les
permissions), et tu restes responsable de l'atterrissage (la revue).

Bon code — et surveille tes tokens. 💰

---

*Guide rédigé le 27 septembre 2026. 110+ sections. Vérifications web : voir section 111.*

## 113. À venir — annonces vérifiées au 27/09/2026

> Méthode : recherche web effectuée le 27/09/2026. Seules les
> annonces officielles ou vérifiables figurent ici ; les rumeurs sont
> étiquetées **RUMEUR non confirmée**. Si rien d'annoncé pour un outil,
> c'est écrit noir sur blanc — rien n'est inventé.

### 113.1 Claude Code — Projects repensés en multi-agents (bêta)

Annonce d'Anthropic le **17/09/2026** : les Projects de Claude Code
sont repensés comme un **coordinateur multi-agents**. Tu décris un
objectif, Claude le découpe en **threads parallèles** (chacun sa session
cloud, sa branche, sa copie du dépôt), suit l'avancement depuis la
conversation principale et partage **mémoire + bibliothèque de fichiers**
entre threads. Les threads peuvent ouvrir des PR, lancer des tests et
se subdiviser en subagents/workflows ; les conflits sont gérés comme
des merge conflicts Git classiques.

- **Statut au 27/09/2026 :** bêta, sélection d'abonnés **Pro et Max** en
  sessions cloud uniquement.
- **Annoncé comme à venir :** exécution sur outils/locaux « très
  bientôt », élargissement à Team et Enterprise ensuite (sans date).
- Conséquence pour toi : l'équivalent « multi-agents coordonnés » que ce
  guide fait faire à la main (sessions parallèles, `tmux`, revue en 4
  passes) devient un produit natif — mais **cloud-only au départ**,
  donc hors air-gap. La revue en 4 passes reste indispensable : le
  coordinateur multiplie la production, pas la relecture.
- Sources : *The Decoder*, 17/09/2026 (« Anthropic keeps pushing Claude
  Code toward autonomous coding with new parallel agent workflows ») ;
  *iTechPost*, 18/09/2026 ; *Complete AI Training*, 18/09/2026.

Anthropic a par ailleurs annoncé le **16/09/2026** (Reuters) la fusion
des interfaces chat + **Cowork**, l'intégration de **Claude Design** et
le lancement en bêta de **Claude Docs** et **Claude Slides** (export
Google Docs / Word / PowerPoint / PDF) — d'abord Pro/Max, puis Team et
Free. Impact indirect : les artefacts de projet (docs, slides) rejoignent
la même interface que l'agent de code.

Sortie du **23/09/2026** : Claude Code **v2.1.281** (correctif,
correctifs de stabilité post-Opus 5.5) — correctifs de réparation de
transcripts (« unexpected tool_use_id »), de timeouts MCP en `http`
après ~5 minutes, et de Write refusés sur des paramètres légèrement
erronés. Le **mode autopilot** est désormais le défaut dans Claude Code.

### 113.2 Codex (OpenAI) — CLI 0.156, Sol/Luna, contexte persistant

- **Codex CLI 0.156.0 (22/09/2026)** : voice mode activé par défaut
  (bascule `F8`, réglages `/voice`) ; interface plein écran optionnelle
  `/tui` (recherche dans le transcript, sélection souris, diagrammes
  Mermaid, notation math) ; tableau de bord d'usage `/usage` ; sessions
  worktree activées par défaut. Patch **0.156.1** (même jour) : **GPT-6
  Sol et GPT-6 Luna** ajoutés au sélecteur de modèles, Luna recommandé
  comme repli économique en cas de rate limit.
- **GPT-6 Astra** (début sept. 2026) : OpenAI annonce pour Codex un mode
  de **notes persistantes entre fenêtres de contexte** — les détails
  accumulés survivent sans compaction brutale, et les anciennes fenêtres
  restent interrogeables. Expérimental au lancement, promis comme défaut
  « dans les prochaines semaines ». À surveiller : c'est la première
  réponse officielle du constructeur au problème de la « mémoire de
  session » que ce guide contourne par fichiers (section 106).
- **Feuille de route Codex (annoncée par OpenAI)** : support Windows de
  l'app desktop (aujourd'hui macOS), **Automations** avec déclencheurs
  cloud (agents en tâche de fond permanente, pas seulement PC allumé),
  **plan mode** en lecture seule avant exécution, **personnalités**
  configurables (`/personality`).
- **Codex Cloud nouvelle génération — RUMEUR non confirmée** : analyse
  par reverse-engineering (RuntimeWire, 20/09/2026) du build desktop
  9922 montrant un parcours « Cloud » refondu (feature-gated) séparant
  « Cloud (Legacy) », avec préparation d'environnement pilotée par
  l'agent via **Tailscale**, secrets injectés par proxy et identités
  cloud (Azure/AWS/GCP). Non annoncé par OpenAI : ne pas planifier
  dessus.
- Sources : *Oday Bakkour*, roundup du 24/09/2026 ; *codex-timeline*
  (GitHub, vérifié 23/09/2026) ; *9to5Mac*, 03/09/2026 ; *VentureBeat*
  (roadmap Codex) ; *RuntimeWire*, 20/09/2026.

### 113.3 GitHub Copilot — sandboxing local (public preview)

Annonce GitHub du **23/09/2026** : **sandboxing local** dans l'app
Copilot (public preview). Politiques par projet sur trois axes :
système de fichiers (lecture/écriture, lecture seule, dossiers
interdits), réseau (internet sortant, réseau local), identifiants
(Git HTTPS, GitHub CLI). Désactivé par défaut (`Sandbox new sessions`
par projet, ou `/sandbox on` en session) ; les admins Entreprise
peuvent imposer plus strict ; si l'OS ne peut appliquer la politique,
la session échoue **fermée** plutôt que de tourner sans sandbox.

- Pour ton cadre de sécurité (section 92) : c'est la version « produit »
  du confinement que ce guide fait à la main (utilisateur dédié,
  conteneurs). Limite connue : **sessions locales uniquement**, pas
  cloud/remote pour l'instant.
- La semaine du 21/09/2026 ajoute aussi à Copilot : **Claude Opus 5.5**,
  **GPT-6 Sol**, **GPT-6 Luna**, **Grok 4.7** (selon plans) ;
  **OpenTelemetry** pour tracer l'activité des agents dans tes outils
  de supervision existants ; **assisted approvals** (approbation auto
  des appels à faible risque) en public preview ; édition d'un message
  antérieur avec **rembobinage** de la conversation et des fichiers.
- Sources : *GitHub Changelog*, 23/09/2026 (« Local sandboxing in the
  GitHub Copilot app ») et 25/09/2026 (« Copilot weekly releases —
  September 21 ») ; *Nandann*, guide sandboxing, 24/09/2026.

### 113.4 Cline — Desktop (bêta) et v4.1.21

- **Cline Desktop, bêta macOS/Windows annoncée le 14/09/2026** par le
  fondateur : l'agent devient une app autonome (même moteur SDK que
  l'extension). Fonctions phares : **import de sessions Claude Code /
  Codex** pour continuer avec un autre modèle, **jobs planifiés** (revue
  de PR, scans sécu, doc), marketplace intégré (plugins, serveurs MCP,
  skills). Modèles open-weight gratuits cités au lancement :
  DeepSeek-V4.1-Flash, Musespark-1.3 (offre gratuite tournante, quotas
  journaliers).
- **v4.1.21 (24/09/2026)** : correctif de fiabilité pour les modèles
  locaux (llama.cpp, Ollama, LM Studio) — compaction + un retry au lieu
  d'abandonner la tâche ; nouveau provider « ai& » ; 19 providers sans
  version épinglée basculent leur défaut vers **Claude Opus 5.5**.
- Sources : *RuntimeWire*, 14/09/2026 ; *ExplainX*, 15/09/2026 ;
  *TPS Report*, 24/09/2026 ; *morphllm.com*, leaderboard sept. 2026.

### 113.5 Cursor — agents cloud sur ton infra

Le **update de septembre 2026** (documenté par *developer-tech.com*)
ajoute l'exécution des **Cloud Agents sur infrastructure propre** :
pools de workers dynamiques mutualisés par équipe (au lieu d'un worker
par machine/dépôt), pools séparés par environnement (GPU, Mac iOS),
**opérateur Kubernetes** (warm capacity, rolling updates, rotation de
tokens) et déploiement **Cloud Run** avec autoscaler. L'inférence et la
planification restent dans le cloud Cursor ; l'exécution (terminal,
fichiers, navigateur) se fait chez toi.

- Lien avec le point Coder (section 113.7) : même mouvement de fond —
  les agents cloud deviennent **déployables on-prem**, ce qui les rend
  envisageables dans ton contexte entreprise régulé.
- « Cursor Projects » : une bêta multi-agents évoquée dans des études
  tierces autour du 10-12/09/2026, **sans annonce officielle retrouvée
  au 27/09/2026** — à classer en *RUMEUR non confirmée* en l'état.

### 113.6 Kilo Code — rien d'officialisé pour la suite

Versions en cours : extension VS Code + CLI **v7.7.7** (22/09/2026),
stable JetBrains v7.1.6 (le CLI Kilo est un fork d'OpenCode, ~5 semaines
derrière l'upstream). Rappel : Anaconda a acquis Kilo en **juillet
2026**. **Aucune annonce officielle de roadmap ou de prochaine version
retrouvée au 27/09/2026** — rien d'officialisé.

### 113.7 Coder — Agent Relay (02/09/2026)

Annonce officielle **Coder × SpaceXAI** (GlobeNewswire, 02/09/2026) :
**Coder Agent Relay**, environnement d'exécution auto-hébergé pour
agents cloud — les Cursor Cloud Agents tournent dans des workspaces
Coder sur l'infrastructure du client (code, secrets, services internes
restent sur tes machines). C'est l'argument « entreprise régulée »
(banques, défense, secteur public) : auditabilité + contrôle réseau.
Coder Agents (agent natif auto-hébergé, agnostique de modèle) reste en
bêta.

### 113.8 OpenCode — Desktop en bêta

L'app **OpenCode Desktop** existe en **bêta** (macOS arm64/x64,
Windows x64, Linux deb/rpm/AppImage ; aussi via Homebrew/Scoop) :
multi-sessions parallèles, LSP chargés automatiquement, liens de
partage de sessions, login Claude Pro/Max, 75+ providers via
Models.dev. **Aucune roadmap officielle publiée retrouvée au
27/09/2026** — rien d'officialisé pour la suite.

### 113.9 OpenClaw — directions annoncées par Steinberger

Interview (août 2026, ~4,7 M de téléchargements) : trois axes de
roadmap énoncés par Peter Steinberger — (1) l'agent **toujours actif et
synchronisé** (problème présenté comme économique — les tokens — pas
comme un problème de modèle) ; (2) interfaces **voix et multimodalité**
(démo FaceTime en hack) ; (3) **team server** (sessions visibles entre
collègues, orchestration délégable), OpenClaw se reconstruisant
lui-même. Rappel : Steinberger a rejoint OpenAI (fév. 2026), OpenClaw
reste open source via une fondation. Sources : *BigGo Finance*
(interview) ; *Outlook Business*, fév. 2026.

### 113.10 Calendrier — OpenAI DevDay (29/09/2026)

**DevDay OpenAI prévu le 29/09/2026 à San Francisco**, avec l'IA
agentique annoncée comme axe majeur (Agents API entrée en bêta
publique début septembre, Agents SDK, infrastructure managée,
MCP au programme). Deux jours après la rédaction de ce guide :
c'est le prochain point de contrôle naturel pour mettre à jour
cette section.

### 113.11 Tableau récapitulatif (statut au 27/09/2026)

| Outil | Annoncé, daté | En bêta / preview | Rumeur ou rien |
|---|---|---|---|
| Claude Code | Projects multi-agents (**17/09**), interface unifiée + Docs/Slides (**16/09**), v2.1.281 (**23/09**), autopilot par défaut | Projects (Pro/Max cloud) | — |
| Codex | CLI 0.156.0/0.156.1 (**22/09**), GPT-6 Sol/Luna, notes Astra (début 09) | Automations cloud, plan mode (roadmap annoncée) | Nouveau Codex Cloud (**rumeur**) |
| Copilot | Sandboxing local (**23/09**), modèles sept. (Opus 5.5, Sol, Luna, Grok 4.7) | Sandboxing (public preview), assisted approvals | — |
| Cline | Desktop (**14/09**), v4.1.21 (**24/09**) | Cline Desktop (bêta) | — |
| Cursor | Workers auto-hébergés (sept. 2026) | — | « Projects » (**rumeur**) |
| Kilo Code | — | — | **rien d'officialisé** |
| OpenCode | Desktop (bêta, existant) | Desktop | **rien d'officialisé** (roadmap) |
| OpenClaw | 3 axes roadmap (interview 08/2026) | — | — |
| Écosystème | Coder Agent Relay (**02/09**), DevDay (**29/09**) | Coder Agents | — |

> 🔁 Rituel : relire cette section **début janvier 2027** (ou après
> chaque DevDay / keynote Anthropic / release majeure) et déplacer les
> éléments sortis vers les sections concernées du guide.
