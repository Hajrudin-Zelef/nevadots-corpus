---
id: collect-261121-veille-ia/claude-anthropic/tutoriel-mcp-server-13-etapes-typescript-et-python-2026
title: "Tutoriel MCP Server : 13 Étapes TypeScript et Python [2026]"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-07-28"]
keywords: ["typescript", "python", "protocole", "juillet", "server", "support", "protocol", "microsoft"]
source: https://tech-insider.org/fr/tutoriel-mcp-server-typescript-python-anthropic-2026/
source_anchor: ""
source_lines: [1, 573]
section: "Claude_Anthropic"
---
Le **Model Context Protocol** (MCP) est devenu, en moins de deux ans, le standard de facto pour connecter les modèles d’intelligence artificielle à des outils, fichiers et bases de données. Depuis la publication initiale par Anthropic le 5 novembre 2024 jusqu’à la toute dernière spécification stable **2026-07-28** – publiée fin juillet 2026 comme cinquième révision majeure du protocole –, l’écosystème a explosé : plus de 2 000 serveurs MCP publics sont désormais recensés sur npm, Microsoft a même fait passer son propre Microsoft Learn MCP Server en disponibilité générale le 7 novembre 2025, et les clients Claude Desktop, Cursor, Windsurf, VS Code et Cline prennent tous en charge le protocole nativement. Ce tutoriel détaille la construction d’un **serveur MCP** complet, en TypeScript puis en Python, avec authentification OAuth 2.1, transports stdio et Streamable HTTP, et déploiement en production. Les commandes, extraits de code et configurations présentés fonctionnent avec le SDK TypeScript officiel `@modelcontextprotocol/sdk` v1.29.0 et le package Python `mcp` (FastMCP), les quatre SDK Tier 1 (TypeScript, Python, Go, C#) ayant tous livré le support de la spec 2026-07-28 dès le 28 juillet 2026 – contenu à jour au 19 août 2026.

À la fin des treize étapes, vous disposerez d’un serveur MCP réutilisable, exposant des *tools*, *resources* et *prompts*, testé avec **MCP Inspector**, connecté à Claude Desktop et Cursor, empaqueté pour npm et PyPI, déployé sur Cloudflare Workers, et prêt à intégrer des agents LangGraph ou CrewAI. Les pièges classiques (désynchronisation du handshake JSON-RPC, fuites de jetons OAuth, schémas Zod incohérents) sont traités en détail dans la section dédiée.

## Qu’est-ce que Model Context Protocol (MCP) en 2026

Le Model Context Protocol est un **protocole ouvert JSON-RPC 2.0** qui standardise la manière dont les applications LLM (hosts) communiquent avec des sources de données et des outils externes via des serveurs dédiés. Son inspiration directe est le Language Server Protocol (LSP) de Microsoft, qui a unifié les éditeurs et les analyseurs statiques depuis 2016. De la même façon, MCP évite aux développeurs d’écrire N × M intégrations propriétaires entre chaque modèle (Claude, GPT-5, Gemini, DeepSeek) et chaque système (GitHub, Slack, Postgres, Google Drive). Un seul serveur MCP est consommable par tous les clients compatibles, sans modification.

La spécification stable active au 19 août 2026 est **2026-07-28**, cinquième révision majeure du protocole. Son *release candidate* a été verrouillé le 21 mai 2026, laissant dix semaines de validation aux SDK Tier 1 avant publication fin juillet 2026 ; elle rend le cœur du protocole davantage stateless et instaure une fenêtre de dépréciation de douze mois pour les anciennes versions à compter de juillet 2026. Les quatre SDK Tier 1 (TypeScript, Python, Go, C#) ont tous livré ce support dès le 28 juillet 2026, et le SDK Rust a ajouté un support bêta de cette même spec le même mois, en tant que première implémentation hors Tier 1. La version précédente, **2025-11-25** – quatrième révision majeure publiée en novembre 2025 –, avait introduit la toute première politique de dépréciation formelle, ainsi que le support d’OpenID Connect Discovery, les métadonnées d’icônes pour tools/resources/prompts, le consentement incrémental de scopes, l’élicitation en mode URL, le *sampling tool calling*, les OAuth Client ID metadata documents et un support expérimental des *Tasks*. Anthropic a transféré la gouvernance de MCP à la Linux Foundation en 2025, ce qui a ouvert le protocole à la contribution de Microsoft, Google, AWS et OpenAI, via des Spec Enhancement Proposals (SEPs) et des Working Groups publics.

Techniquement, un serveur MCP expose trois primitives : des **tools** (fonctions exécutables, comme « envoyer un email »), des **resources** (données en lecture, comme un fichier ou une requête SQL) et des **prompts** (gabarits réutilisables pour les interactions LLM). Chacune est décrite par un schéma JSON validé côté serveur, ce qui permet au modèle d’appeler l’outil en toute sécurité, avec typage fort. Les transports supportés sont `stdio` (processus local), `SSE` (Server-Sent Events, déprécié au profit de Streamable HTTP), `Streamable HTTP` (transport web moderne unifié) et `WebSocket` (bidirectionnel, utilisé pour le sampling).

## Architecture MCP : Hosts, Clients et Serveurs

L’architecture MCP est stateful et suit un modèle trois rôles clairement délimités. Le **host** est l’application qui héberge le LLM (Claude Desktop, Cursor, un script LangGraph). Il instancie un ou plusieurs **clients**, un par serveur distant, qui maintiennent chacun une connexion JSON-RPC persistante. Chaque **serveur** MCP expose ses capacités via des méthodes comme `tools/list`, `tools/call`, `resources/read` ou `prompts/get`. Le host orchestre, le client dialogue, le serveur répond.

Le handshake d’initialisation est critique. À la connexion, le client envoie `initialize` avec la version de protocole supportée et ses capabilities (e.g. `roots`, `sampling`). Le serveur répond avec la version négociée, son nom, sa version et ses capabilities (`tools`, `resources`, `prompts`, `logging`). Le client confirme avec `notifications/initialized`, puis la session démarre. Tout appel avant ce triptyque échoue avec `-32002 (Server not initialized)`.

La table ci-dessous résume les rôles et flux du protocole dans la version 2025-11-25 :

| Composant | Rôle | Méthodes principales | Transport typique | 
|---|---|---|---|
| Host (Claude Desktop, Cursor) | Exécute le LLM et consomme les capacités | – | gère les clients | 
| Client (par serveur) | Maintient la session JSON-RPC | `initialize` ,`tools/call` | stdio, Streamable HTTP | 
| Serveur MCP | Expose tools, resources, prompts | `tools/list` ,`resources/read` | stdio, HTTP, WebSocket | 
| Transport stdio | Communication locale par flux | JSON-RPC sur stdin/stdout | fork de processus | 
| Transport Streamable HTTP | Communication distante moderne | POST /mcp + SSE | HTTPS + OAuth 2.1 | 

## Prérequis du tutoriel : versions, outils et comptes

Avant d’écrire la moindre ligne, vérifiez l’environnement. Un serveur MCP moderne exige des runtimes récents pour supporter `fetch` natif, les modules ES et les décorateurs asynchrones – d’autant plus depuis que les quatre SDK Tier 1 (TypeScript, Python, Go, C#) ont basculé sur le support de la spécification 2026-07-28 le 28 juillet 2026. Les versions minimales et recommandées au 19 août 2026 sont :

| Outil | Version minimale | Version recommandée | Commande de vérification | 
|---|---|---|---|
| Node.js | 18.0 | 22 LTS | `node --version` | 
| npm | 9.0 | 10.x | `npm --version` | 
| Python | 3.10 | 3.12 | `python3 --version` | 
| uv (gestionnaire Python) | 0.4 | 0.5+ | `uv --version` | 
| TypeScript | 5.2 | 5.6+ | `tsc --version` | 
| @modelcontextprotocol/sdk | 1.20 | 1.29.0 | `npm list @modelcontextprotocol/sdk` | 
| mcp (PyPI) | 1.12 | 1.15+ | `uv pip show mcp` | 
| Claude Desktop | 0.8 | 1.2+ | Menu → À propos | 

Créez également un compte **GitHub** pour tester le serveur MCP GitHub officiel en fin de tutoriel, installez `git` 2.40+, et prévoyez un dossier de travail propre : `mkdir -p ~/mcp-lab && cd ~/mcp-lab`. Si vous comptez déployer sur Cloudflare, créez un compte gratuit et installez `wrangler` 4.x via `npm install -g wrangler`. Pour déboguer, vous utiliserez **MCP Inspector**, livré via `npx @modelcontextprotocol/inspector`.

## Étape 1 – Initialiser un projet MCP TypeScript avec le SDK 1.29

Créez un dossier `mcp-ts-server`, générez un `package.json` en ESM strict et installez le SDK officiel ainsi que Zod pour la validation des schémas. ESM est obligatoire : le SDK TypeScript n’exporte plus de CommonJS depuis la v1.10.

```
mkdir mcp-ts-server && cd mcp-ts-server
npm init -y
npm pkg set type="module"
npm install @modelcontextprotocol/[email protected] [email protected]
npm install -D [email protected] @types/node@22 [email protected]
```
Ajoutez un fichier `tsconfig.json` minimal. Le `moduleResolution` doit être `NodeNext` sinon les imports `.js` du SDK échoueront :

```
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "outDir": "dist",
    "rootDir": "src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  },
  "include": ["src/**/*"]
}
```
Complétez les scripts npm pour le build, le run en développement (via `tsx`) et le lancement via MCP Inspector :

```
npm pkg set scripts.build="tsc"
npm pkg set scripts.dev="tsx watch src/index.ts"
npm pkg set scripts.start="node dist/index.js"
npm pkg set scripts.inspector="npx @modelcontextprotocol/inspector node dist/index.js"
```
Créez `src/index.ts` avec un squelette `McpServer`. Le constructeur prend un objet `{ name, version }` et déclare les capabilities. Sans `tools: {}`, la méthode `tools/list` renverra une erreur `-32601 (Method not found)`.

```
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
const server = new McpServer(
  { name: "ti-demo-server", version: "1.0.0" },
  { capabilities: { tools: {}, resources: {}, prompts: {} } }
);
async function main() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
  console.error("MCP server ready on stdio");
}
main().catch((err) => { console.error(err); process.exit(1); });
```
Compilez avec `npm run build` et lancez `npm run inspector`. Une interface web s’ouvre sur `http://localhost:5173` et montre le handshake réussi. Si rien ne s’affiche, vérifiez que vous écrivez bien vos logs sur `stderr` (`console.error`) et jamais sur `stdout` – ce dernier est réservé au protocole JSON-RPC.

## Étape 2 – Créer votre premier Tool avec Zod et schémas JSON

Un tool MCP est une fonction exposée au LLM. Elle est décrite par un *input schema* Zod, converti automatiquement en JSON Schema par le SDK, et un *handler* asynchrone qui reçoit les arguments typés et renvoie un tableau de `content`. Ajoutons un outil `calculer_tva` qui applique les taux français de TVA.

```
import { z } from "zod";
server.tool(
  "calculer_tva",
  "Calcule la TVA française pour un montant HT donné",
  {
    montant_ht: z.number().positive().describe("Montant hors taxes en euros"),
    taux: z.enum(["20", "10", "5.5", "2.1"]).describe("Taux de TVA applicable"),
  },
  async ({ montant_ht, taux }) => {
    const t = parseFloat(taux) / 100;
    const ttc = montant_ht * (1 + t);
    const tva = montant_ht * t;
    return {
      content: [
        {
          type: "text",
          text: `HT: ${montant_ht.toFixed(2)} € · TVA (${taux}%) : ${tva.toFixed(2)} € · TTC: ${ttc.toFixed(2)} €`,
        },
      ],
    };
  }
);
```
La signature `server.tool(name, description, inputShape, handler)` est la forme canonique du SDK 1.29. Les trois erreurs les plus courantes : (1) passer un schéma Zod au lieu du *shape* brut (le SDK wrap lui-même) ; (2) retourner un objet au lieu de `{ content: [...] }` ; (3) oublier de décrire les paramètres avec `.describe()`, ce qui réduit nettement la qualité d’appel du LLM.

Rebuildez, relancez l’Inspector, et dans l’onglet *Tools*, testez avec `{ "montant_ht": 1000, "taux": "20" }`. Vous obtiendrez `HT: 1000,00 € · TVA (20%) : 200,00 € · TTC: 1200,00 €`. Le LLM reçoit exactement ce texte comme contenu d’appel outil, formaté prêt à être cité.

## Étape 3 – Exposer Resources et Prompts pour l’agent

Les *resources* exposent des données en lecture, identifiées par une URI. Contrairement aux tools, elles ne sont pas « appelées » : le client les référence et le host décide quand les injecter dans le contexte du LLM. Deux formes existent : statique (une URI fixe) ou dynamique via `ResourceTemplate` avec paramètres.

```
import { ResourceTemplate } from "@modelcontextprotocol/sdk/server/mcp.js";
// Ressource statique
server.registerResource(
  "config-entreprise",
  "config://entreprise",
  { title: "Configuration entreprise", mimeType: "application/json" },
  async () => ({
    contents: [{ uri: "config://entreprise", text: JSON.stringify({ pays: "FR", devise: "EUR" }) }],
  })
);
// Ressource templatée
server.registerResource(
  "facture",
  new ResourceTemplate("factures://{id}", { list: undefined }),
  { title: "Facture par ID" },
  async (uri, { id }) => ({
    contents: [{ uri: uri.href, text: `Facture ${id} – statut : payée` }],
  })
);
```
Les *prompts* sont des gabarits réutilisables avec des arguments. Ils apparaissent dans le menu *slash commands* de Claude Desktop (`/`). Un prompt bien conçu évite à l’utilisateur de réécrire une amorce complexe et sert aussi aux agents automatisés.

```
server.prompt(
  "audit_rgpd",
  "Génère un audit RGPD pour un fichier de traitement",
  { fichier: z.string().describe("Chemin du fichier à auditer") },
  async ({ fichier }) => ({
    messages: [
      {
        role: "user",
        content: {
          type: "text",
          text: `Réalise un audit RGPD complet du fichier ${fichier}. Liste : finalité, base légale, DPO, durée de conservation, risques identifiés, actions correctives.`,
        },
      },
    ],
  })
);
```
Dans MCP Inspector, l’onglet *Resources* liste `config://entreprise` et permet de tester `factures://INV-2026-001`. L’onglet *Prompts* affiche `audit_rgpd` et accepte l’argument. Si une ressource ne s’affiche pas, vérifiez que `capabilities.resources = {}` est bien déclaré dans le constructeur du serveur.

## Étape 4 – Configurer le transport stdio pour Claude Desktop

Le transport `stdio` reste le plus simple pour un serveur MCP local. Claude Desktop le lance comme sous-processus, dialogue en JSON-RPC via stdin/stdout, et ferme le flux à la déconnexion. C’est aussi le transport par défaut pour Cursor, Windsurf et VS Code avec Copilot MCP.

Le fichier de configuration Claude Desktop dépend du système d’exploitation :

| Système | Chemin du fichier de configuration | 
|---|---|
| macOS | `~/Library/Application Support/Claude/claude_desktop_config.json` | 
| Windows | `%APPDATA%\Claude\claude_desktop_config.json` | 
| Linux | `~/.config/Claude/claude_desktop_config.json` | 

Ajoutez votre serveur dans la clé `mcpServers`. Le chemin du binaire `node` doit être absolu sous macOS (pas d’alias `nvm` car Claude Desktop ne charge pas votre shell) et les arguments doivent pointer sur le `dist/index.js` compilé :

```
{
  "mcpServers": {
    "ti-demo": {
      "command": "/usr/local/bin/node",
      "args": ["/Users/vous/mcp-lab/mcp-ts-server/dist/index.js"],
      "env": {
        "NODE_ENV": "production"
      }
    }
  }
}
```
Redémarrez Claude Desktop (`Cmd+Q` puis relance). L’icône 🔌 dans la barre d’entrée indique que le serveur est connecté. Cliquez dessus pour lister les tools exposés. En cas d’échec, consultez les logs dans `~/Library/Logs/Claude/mcp-server-ti-demo.log` sous macOS. Les erreurs les plus fréquentes : chemin Node inexistant, permissions d’exécution manquantes (`chmod +x dist/index.js`), ou écriture parasite sur stdout (`console.log` au lieu de `console.error`).

## Étape 5 – Passer au Streamable HTTP avec Express et SSE

Pour un serveur distant consommé par plusieurs hosts, stdio ne suffit pas. Le transport **Streamable HTTP**, introduit dans la spec 2025-06-18 et stabilisé en 2025-11-25, remplace l’ancien SSE. Il utilise une seule URL `/mcp` gérant POST (requêtes) et GET (flux SSE pour notifications serveur).

```
import express from "express";
import { randomUUID } from "node:crypto";
import { StreamableHTTPServerTransport } from "@modelcontextprotocol/sdk/server/streamableHttp.js";
const app = express();
app.use(express.json());
const transports: Record<string, StreamableHTTPServerTransport> = {};
app.post("/mcp", async (req, res) => {
  const sessionId = req.headers["mcp-session-id"] as string | undefined;
  let transport = sessionId ? transports[sessionId] : undefined;
  if (!transport) {
    transport = new StreamableHTTPServerTransport({
      sessionIdGenerator: () => randomUUID(),
    });
    await server.connect(transport);
    transports[transport.sessionId!] = transport;
  }
  await transport.handleRequest(req, res, req.body);
});
app.get("/mcp", async (req, res) => {
  const sessionId = req.headers["mcp-session-id"] as string;
  const transport = transports[sessionId];
  if (!transport) return res.status(400).send("Session inconnue");
  await transport.handleRequest(req, res);
});
app.listen(3000, () => console.error("MCP HTTP sur http://localhost:3000/mcp"));
```
Trois points d’attention : (1) l’en-tête `Mcp-Session-Id` est obligatoire après le premier `initialize` ; (2) activez CORS explicitement pour les clients navigateur (`cors({ exposedHeaders: ["Mcp-Session-Id"] })`) ; (3) prévoyez un timeout de connexion SSE à 30 secondes minimum, sinon les notifications longues sont coupées. Testez depuis Inspector en mode « HTTP » avec l’URL `http://localhost:3000/mcp`.

## Étape 6 – Construire un serveur MCP en Python avec FastMCP

Le SDK Python officiel `mcp` expose la classe `FastMCP`, inspirée de FastAPI, qui génère automatiquement les schémas JSON à partir des annotations de type et des docstrings. Créez un nouveau projet avec `uv` (gestionnaire recommandé) :

```
mkdir mcp-py-server && cd mcp-py-server
uv init --python 3.12
uv add "mcp[cli]>=1.15"
uv add httpx
```
Créez `server.py` avec un serveur qui interroge l’API gouvernementale française `entreprise.data.gouv.fr` pour récupérer un SIREN. La fonction est typée et documentée en français : FastMCP extrait la description et les types pour le LLM.

```
import httpx
from mcp.server.fastmcp import FastMCP
mcp = FastMCP("ti-siren-server")
@mcp.tool()
async def rechercher_siren(siren: str) -> dict:
    """Recherche les informations publiques d'une entreprise française par SIREN.
    Args:
        siren: Le numéro SIREN à 9 chiffres.
    """
    url = f"https://recherche-entreprises.api.gouv.fr/search?q={siren}"
    async with httpx.AsyncClient(timeout=10) as client:
        r = await client.get(url)
        r.raise_for_status()
        data = r.json()
        if not data.get("results"):
            return {"erreur": "SIREN inconnu"}
        e = data["results"][0]
        return {
            "nom": e.get("nom_complet"),
            "siren": e.get("siren"),
            "naf": e.get("activite_principale"),
            "effectif": e.get("tranche_effectif_salarie"),
            "siege": e.get("siege", {}).get("adresse"),
        }
@mcp.resource("sirets://{siren}")
def profil_entreprise(siren: str) -> str:
    return f"Profil synthétique pour SIREN {siren}."
if __name__ == "__main__":
    mcp.run(transport="stdio")
```
Testez via `uv run mcp dev server.py`, qui lance automatiquement MCP Inspector sur le serveur Python. Pour un transport HTTP, appelez `mcp.run(transport="streamable-http", host="0.0.0.0", port=8000)`. Les décorateurs `@mcp.tool()`, `@mcp.resource()` et `@mcp.prompt()` suivent la même ergonomie : typage, docstring, retour JSON-sérialisable.

## Étape 7 – Sécuriser avec OAuth 2.1 et PKCE (spec 2025-11-25)

La spec 2025-11-25 mandate **OAuth 2.1 avec PKCE** pour tout serveur MCP distant. Le serveur est classifié comme OAuth Resource Server (RFC 9728), et expose un `/.well-known/oauth-protected-resource` qui pointe vers l’Authorization Server. Les clients utilisent la Dynamic Client Registration (RFC 7591) ou les nouveaux OAuth Client ID metadata documents pour s’enregistrer sans interaction humaine.

```
import { requireBearerAuth } from "@modelcontextprotocol/sdk/server/auth/middleware/bearerAuth.js";
const authMiddleware = requireBearerAuth({
  verifier: {
    verifyAccessToken: async (token) => {
      const response = await fetch("https://auth.exemple.fr/introspect", {
        method: "POST",
        headers: { "Content-Type": "application/x-www-form-urlencoded" },
        body: new URLSearchParams({ token }),
      });
      const data = await response.json();
      if (!data.active) throw new Error("Token invalide");
      return {
        token,
        clientId: data.client_id,
        scopes: data.scope?.split(" ") ?? [],
      };
    },
  },
  requiredScopes: ["mcp:read", "mcp:call"],
});
app.post("/mcp", authMiddleware, async (req, res) => { /* ... */ });
```
Côté client, le flow est : (1) le client découvre `/.well-known/oauth-protected-resource` ; (2) il récupère le `authorization_server` ; (3) il lance un Authorization Code flow avec PKCE et `resource` indicator (RFC 8707) pointant sur le serveur MCP ; (4) l’access token obtenu est présenté en `Authorization: Bearer` à chaque requête. Le consentement incrémental de scopes, nouveauté de la spec 2025-11-25, évite de tout demander d’un coup : le serveur peut renvoyer un `403` avec un `insufficient_scope`, déclenchant une nouvelle phase d’autorisation granulaire.

Trois impératifs de sécurité : utilisez TLS 1.3 en production, stockez les tokens chiffrés (AES-GCM, jamais en clair dans une base), et n’acceptez jamais `audience` absent ou vide – c’est le vecteur classique d’exfiltration de jetons entre serveurs MCP.

## Étape 8 – Déboguer avec MCP Inspector

MCP Inspector est l’outil officiel de débogage. Il se lance via `npx @modelcontextprotocol/inspector <commande>`, par exemple `npx @modelcontextprotocol/inspector node dist/index.js` ou `npx @modelcontextprotocol/inspector uv run server.py`. L’interface web expose cinq onglets : *Connection*, *Tools*, *Resources*, *Prompts*, *Sampling*.

Dans *Connection*, vérifiez la version de protocole négociée et les capabilities. Un écart de version entre client et serveur (par exemple 2025-06-18 vs 2025-11-25) provoque un fallback vers la plus ancienne supportée ; si aucune ne matche, la connexion échoue. Pour forcer une version, passez `--protocol-version 2025-11-25` en ligne de commande.

Dans *Tools*, chaque outil est listé avec son input schema en JSON. Un bouton *Call* permet d’exécuter l’outil avec des valeurs de test et d’afficher la réponse brute ainsi que les erreurs JSON-RPC. Le panneau *Request Log* à droite montre toute la trace JSON-RPC en temps réel, incluant les `notifications/message` pour le logging structuré.

```
# Forcer une version et lancer l'inspector en mode HTTP
npx @modelcontextprotocol/inspector \
  --protocol-version 2025-11-25 \
  --transport streamable-http \
  --server-url http://localhost:3000/mcp
```
Astuce : activez le logging côté serveur avec `server.server.sendLoggingMessage({ level: "info", data: "..." })`. Chaque message apparaît dans l’Inspector sous *Server Logs*. Si rien ne s’affiche, vérifiez que la capability `logging: {}` est déclarée et que le client a appelé `logging/setLevel`.

## Étape 9 – Connecter à Cursor, Windsurf et VS Code

L’écosystème d’éditeurs supportant MCP a doublé entre 2025 et 2026. Cursor a ajouté le support natif en v0.43, Windsurf dans sa version Cascade 1.0, et Microsoft a intégré MCP dans GitHub Copilot Agent Mode sur VS Code 1.99+. La configuration est similaire à Claude Desktop mais chaque éditeur utilise son propre fichier.

| Éditeur | Fichier de configuration | Mode agent | 
|---|---|---|
| Claude Desktop | `claude_desktop_config.json` | Chat + slash commands | 
| Cursor | `~/.cursor/mcp.json` ou`.cursor/mcp.json` (projet) | Composer Agent | 
| Windsurf | `~/.codeium/windsurf/mcp_config.json` | Cascade | 
| VS Code (Copilot Agent) | `.vscode/mcp.json` (projet) | Agent Mode | 
| Cline | `~/.config/cline/mcpServers.json` | Autonomous Coder | 

Pour Cursor, créez `.cursor/mcp.json` à la racine du projet avec la même structure que Claude Desktop. L’onglet *MCP* des préférences Cursor permet d’activer/désactiver chaque serveur et de voir les outils disponibles. Pour VS Code, le fichier `.vscode/mcp.json` supporte en plus des *inputs* sécurisés pour les secrets (API keys stockés dans VS Code Secret Storage, jamais en clair) :

```
{
  "inputs": [
    {
      "type": "promptString",
      "id": "github-token",
      "description": "GitHub Personal Access Token",
      "password": true
    }
  ],
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "${input:github-token}" }
    }
  }
}
```
## Étape 10 – Packager et déployer en production

Pour distribuer votre serveur MCP TypeScript sur npm, préparez le `package.json` avec un `bin`, un `files` restreint et une condition de publication. Ajoutez un shebang `#!/usr/bin/env node` en tête du `dist/index.js` (le SDK propose l’helper `chmod` via un script postbuild).

```
{
  "name": "@votreorg/ti-demo-mcp",
  "version": "1.0.0",
  "type": "module",
  "bin": { "ti-demo-mcp": "dist/index.js" },
  "files": ["dist"],
  "scripts": {
    "build": "tsc && chmod +x dist/index.js",
    "prepublishOnly": "npm run build"
  },
  "dependencies": {
    "@modelcontextprotocol/sdk": "1.29.0",
    "zod": "3.24.0"
  }
}
```
Publiez avec `npm publish --access public`. Les utilisateurs l’installeront en un appel dans Claude Desktop : `"command": "npx", "args": ["-y", "@votreorg/ti-demo-mcp"]`. Pour Python, publiez sur PyPI via `uv build && uv publish`. Pour une distribution Docker, une image basée sur `node:22-alpine` suffit en moins de 100 Mo.

Pour un déploiement **Cloudflare Workers**, utilisez `wrangler` 4.x. Cloudflare a publié en 2025 un adaptateur officiel `@modelcontextprotocol/sdk-cloudflare` qui remplace Express par Workers runtime et gère l’OAuth 2.1 via Workers KV. Un déploiement sur AWS Bedrock MCP Gateway, Google Cloud Vertex AI Agent Builder ou Azure AI Studio est également possible depuis début 2026.

```
# Déploiement Cloudflare Workers
npm install -g wrangler@4
wrangler init mcp-worker
# Ajouter votre serveur MCP puis
wrangler deploy
```
## Étape 11 – Tester, monitorer et journaliser

Les tests unitaires d’un serveur MCP ciblent d’abord les handlers (logique métier des tools) et ensuite le handshake complet. Vitest est le framework recommandé pour TypeScript, `pytest` pour Python. Le SDK propose `InMemoryTransport` pour tester sans I/O réelle.

```
import { describe, it, expect } from "vitest";
import { Client } from "@modelcontextprotocol/sdk/client/index.js";
import { InMemoryTransport } from "@modelcontextprotocol/sdk/inMemory.js";
describe("ti-demo-server", () => {
  it("calcule la TVA à 20 %", async () => {
    const [ct, st] = InMemoryTransport.createLinkedPair();
    await server.connect(st);
    const client = new Client({ name: "test", version: "1.0.0" });
    await client.connect(ct);
    const res = await client.callTool({
      name: "calculer_tva",
      arguments: { montant_ht: 100, taux: "20" },
    });
    expect(res.content[0].text).toContain("120,00");
  });
});
```
En production, exposez des métriques Prometheus (`prom-client`) sur `/metrics` : nombre d’appels par tool, latence P50/P95/P99, taux d’erreur JSON-RPC, sessions actives. Un dashboard Grafana de base montre ces quatre séries et identifie immédiatement un outil lent ou une fuite de sessions. Le logging structuré (pino pour Node, structlog pour Python) vers un agrégateur comme Loki ou Datadog complète le dispositif.

## Étape 12 – Intégrer à LangGraph et CrewAI pour agents multi-LLM

Un serveur MCP peut être consommé par n’importe quel framework d’agents. LangGraph (LangChain) expose depuis 2025 un adaptateur `langchain-mcp-adapters` qui convertit les tools MCP en LangChain Tools ; CrewAI propose `crewai-tools-mcp`, et AutoGen ajoute un `MCPToolAdapter` natif. Ces adaptateurs gèrent le handshake, la découverte des tools et la conversion des schémas.

```
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client
from langchain_mcp_adapters.tools import load_mcp_tools
from langgraph.prebuilt import create_react_agent
from langchain_anthropic import ChatAnthropic
async def run_agent():
    params = StdioServerParameters(command="node", args=["dist/index.js"])
    async with stdio_client(params) as (r, w):
        async with ClientSession(r, w) as session:
            await session.initialize()
            tools = await load_mcp_tools(session)
            agent = create_react_agent(
                ChatAnthropic(model="claude-opus-4-7"),
                tools,
            )
            out = await agent.ainvoke({"messages": "Calcule la TVA de 1250 € à 20 %."})
            print(out["messages"][-1].content)
```
L’agent ReAct interroge la liste des tools exposés par votre serveur, sélectionne `calculer_tva`, passe les arguments extraits de la phrase utilisateur, reçoit la réponse et produit une réponse finale en langage naturel. Tout cela sans écrire une seule ligne de code d’intégration spécifique au modèle.

## Étape 13 – Projet complet : serveur MCP Filesystem d’entreprise

Récapitulons avec un projet de bout en bout. Le serveur `ti-filesystem-mcp` expose deux tools (`lire_fichier`, `lister_dossier`), une resource templatée (`file://{path}`) et un prompt (`resume_code`). Il filtre les chemins via une allowlist pour éviter les escapes, et supporte stdio local ainsi que HTTP distant.

```
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";
import { readFile, readdir } from "node:fs/promises";
import path from "node:path";
const ALLOWED_ROOTS = (process.env.MCP_ALLOWED_ROOTS ?? "").split(",").filter(Boolean);
function checkPath(p: string) {
  const abs = path.resolve(p);
  if (!ALLOWED_ROOTS.some((r) => abs.startsWith(path.resolve(r)))) {
    throw new Error(`Chemin refusé : ${abs}`);
  }
  return abs;
}
const server = new McpServer(
  { name: "ti-filesystem-mcp", version: "1.0.0" },
  { capabilities: { tools: {}, resources: {}, prompts: {} } }
);
server.tool(
  "lire_fichier",
  "Lit un fichier texte dans un dossier autorisé",
  { chemin: z.string() },
  async ({ chemin }) => {
    const abs = checkPath(chemin);
    const contenu = await readFile(abs, "utf-8");
    return { content: [{ type: "text", text: contenu }] };
  }
);
server.tool(
  "lister_dossier",
  "Liste les entrées d'un dossier autorisé",
  { chemin: z.string() },
  async ({ chemin }) => {
    const abs = checkPath(chemin);
    const entries = await readdir(abs, { withFileTypes: true });
    const lines = entries.map((e) => `${e.isDirectory() ? "DIR " : "FILE"} ${e.name}`);
    return { content: [{ type: "text", text: lines.join("\n") }] };
  }
);
server.prompt(
  "resume_code",
  "Résume un fichier de code",
  { fichier: z.string() },
  async ({ fichier }) => ({
    messages: [{ role: "user", content: { type: "text", text: `Résume clairement le rôle, les dépendances et les risques du fichier ${fichier}.` } }],
  })
);
async function main() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
  console.error("ti-filesystem-mcp ready");
}
main();
```
Lancez avec `MCP_ALLOWED_ROOTS="/Users/vous/projets" node dist/index.js`. Le serveur refuse toute requête hors de l’allowlist avec une erreur explicite. Testez-le dans Claude Desktop ou Cursor : demandez « Liste le contenu du dossier projets et résume le fichier `main.ts` », le modèle chaîne automatiquement `lister_dossier`, `lire_fichier` puis `resume_code`.

## 8 pièges courants et comment les éviter

La communauté MCP a identifié huit pièges récurrents qui cassent les intégrations débutantes. Les connaître évite des heures de débogage stérile.

| Piège | Symptôme | Solution | 
|---|---|---|
| Écriture sur stdout | Handshake échoue, Claude Desktop déconnecte | Logger sur stderr uniquement ( `console.error` ) | 
| Versions protocole désynchronisées | Erreur « unsupported version » | Mettre à jour SDK et spec à 2025-11-25 | 
| Session HTTP sans header | 400 Bad Request sur 2e appel | Renvoyer l’en-tête `Mcp-Session-Id` | 
| Schémas Zod nested non supportés | Appel outil ignoré | Aplatir les schémas, éviter les unions complexes | 
| Chemins relatifs sous Claude Desktop | ENOENT au démarrage | Utiliser des chemins absolus | 
| Token OAuth sans audience | Risque d’exfiltration silencieuse | Valider le `aud` claim à chaque requête | 
| Timeouts longs non gérés | Connexion SSE fermée au bout de 10 s | Heartbeats toutes les 25 s | 
| Permissions Linux 644 | `Permission denied` | `chmod +x dist/index.js` post-build | 

Deux pièges avancés méritent un mot supplémentaire. D’abord, la **fuite de sessions** côté serveur HTTP : si vous stockez les transports dans un objet global sans purge, la mémoire grimpe linéairement. Ajoutez un `transport.onclose = () => delete transports[sid]` et un TTL de 15 minutes par défaut. Ensuite, la **réentrance des tools** : un tool qui appelle un autre tool via le client interne peut boucler si le LLM enchaîne des requêtes. Protégez avec un sémaphore par session.

## Astuces avancées pour serveurs MCP en production

Au-delà de la base, quelques pratiques distinguent les serveurs MCP sérieux. Première astuce : utilisez **structured content** (spec 2025-06-18) plutôt que du texte brut. Un tool peut retourner `{ content: [{ type: "text", text }], structuredContent: { champs... } }`, qui est consommé directement par le LLM sans parsing. Deuxième astuce : exposez un endpoint `/.well-known/mcp-metadata` pour que les hosts découvrent automatiquement vos capabilities sans ouvrir une session complète – pratique pour les marketplaces de serveurs MCP.

Troisième astuce : implémentez le **sampling**. Un serveur peut, au lieu de répondre lui-même, demander au client d’appeler son propre LLM (c’est la primitive inverse). Utile pour des tools qui nécessitent une génération (traduction, résumé) : le client paie l’inférence, pas le serveur. Quatrièmement, exploitez l’**elicitation** : le serveur peut requérir un input utilisateur en cours d’exécution d’un tool (confirmation, valeur manquante), via une URL ou un prompt dédié – très puissant pour les workflows humains-dans-la-boucle.

Cinquième astuce : profilez avec `0x` (Node) ou `py-spy` (Python) avant toute optimisation. Un tool qui paraît lent est souvent bloqué sur un appel HTTP externe, pas sur son code – un simple cache LRU de 30 secondes sur les appels API gouv.fr ou GitHub réduit la latence médiane de 400 ms à moins de 10 ms.

Sixième astuce : exploitez les **notifications/cancelled** pour annuler proprement un tool long. Le client peut envoyer un message pour interrompre une requête ; côté serveur, écoutez l’événement via `signal.onabort` et propagez l’annulation à vos appels `fetch` ou requêtes SQL. Sans cela, un utilisateur qui ferme son IDE laisse tourner des handlers fantômes pendant plusieurs minutes. Septième astuce : paginez systématiquement les ressources volumineuses. Le SDK prend en charge `nextCursor` dans les réponses `tools/list` et `resources/list` ; implémentez-le dès que votre serveur dépasse 50 entrées pour éviter les payloads de plusieurs mégaoctets qui saturent le contexte LLM.

Huitième astuce : adoptez une convention de nommage claire pour vos tools (`verbe_objet` en snake_case, descriptions commençant par un verbe d’action, e.g. « Calcule », « Liste », « Crée »). Les LLM modernes sélectionnent mieux un outil dont le nom et la description reflètent exactement son intention métier. Testez empiriquement la reconnaissance avec le framework `mcp-eval` (en open source depuis fin 2025), qui mesure le taux d’appel correct d’un tool sur un benchmark de 500 requêtes utilisateur réelles.

## FAQ sur les serveurs MCP en 2026

### MCP remplace-t-il OpenAI Function Calling ?

Non, ils sont complémentaires. Function Calling est un format d’API propriétaire OpenAI (et Anthropic), destiné à invoquer une fonction dans un prompt unique. MCP est un protocole de session persistante entre hôte et serveur, supportant tools, resources, prompts, sampling et elicitation. En pratique, un serveur MCP traduit ses tools en Function Calls vers n’importe quel modèle.

### Faut-il apprendre TypeScript ou Python pour MCP ?

Les deux SDK sont au même niveau de maturité en avril 2026. TypeScript bénéficie du plus large écosystème de serveurs publics (plus de 2 000 paquets npm dans la communauté), Python convient mieux aux serveurs qui s’appuient sur scikit-learn, PyTorch ou pandas. Pour Java, C# et Kotlin, des SDK officiels sont disponibles depuis 2025.

### Quels sont les serveurs MCP publics les plus populaires ?

Les incontournables maintenus par la communauté `modelcontextprotocol` incluent `filesystem`, `github`, `slack`, `postgres`, `sqlite`, `memory`, `fetch`, `brave-search`, `google-drive` et `puppeteer`, tous disponibles via `npx -y @modelcontextprotocol/server-<nom>`. Du côté des éditeurs, le **Microsoft Learn MCP Server** illustre la maturité atteinte par les serveurs officiels : lancé le 12 juin 2025 avec trois outils (search, fetch, code sample search), il a reçu son outil de récupération de documentation le 6 août 2025 – convertissant chaque page en Markdown pour les modèles –, puis son outil de recherche d’échantillons de code le 24 septembre 2025, avant de passer en disponibilité générale le 7 novembre 2025 avec un point de terminaison compatible OpenAI dédié à la recherche approfondie.

### MCP fonctionne-t-il avec GPT-5, Gemini et DeepSeek ?

Oui. OpenAI a ajouté le support MCP côté client dans ChatGPT début 2026, Google Cloud Vertex AI Agent Builder l’intègre depuis mars 2026, et DeepSeek ainsi que Mistral l’exposent via leurs propres clients. Côté serveur, peu importe le modèle consommateur : le protocole est neutre.

### Comment sécuriser un serveur MCP distant ?

Trois couches indispensables : TLS 1.3, OAuth 2.1 avec PKCE et resource indicator (RFC 8707), et rate limiting strict par session (par exemple 60 appels/minute). Ajoutez une allowlist de scopes, validez systématiquement le claim `aud` du token et journalisez chaque appel outil pour l’audit.

### Peut-on monétiser un serveur MCP ?

Oui, via OAuth scopes payants et metered billing. Plusieurs éditeurs (Linear, Notion, Asana) facturent désormais l’accès MCP à leurs API comme un add-on. Les marketplaces Cloudflare, AWS Bedrock et Azure AI Studio proposent depuis Q1 2026 un catalogue commercial de serveurs MCP certifiés.

### Quelle est la différence entre stdio et Streamable HTTP ?

Le transport stdio est local (sous-processus), sans chiffrement, idéal pour un serveur exécuté sur la machine de l’utilisateur (filesystem, accès à un outil CLI). Streamable HTTP fonctionne à distance, avec TLS et OAuth, pour multi-utilisateur ou cloud. L’ancien transport SSE est déprécié depuis la spec 2025-06-18.

### Comment publier un serveur MCP sur le registre officiel ?

Le registre communautaire est hébergé sur GitHub sous `modelcontextprotocol/servers`. Ouvrez une pull request avec votre serveur dans `src/`, en respectant la convention de nommage `server-<nom>`, une suite de tests et un README. Pour un serveur commercial, privilégiez la publication directe sur npm/PyPI et un référencement dans un marketplace cloud.

### Related Coverage

*Sources officielles : Spécification MCP 2025-11-25, TypeScript SDK, Python SDK, Registre officiel de serveurs, Annonce Anthropic, MCP Inspector. Article mis à jour le 23 avril 2026.*
