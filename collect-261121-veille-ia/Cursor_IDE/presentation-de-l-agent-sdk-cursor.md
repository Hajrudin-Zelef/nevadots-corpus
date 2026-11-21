---
id: collect-261121-veille-ia/cursor-ide/presentation-de-l-agent-sdk-cursor
title: "Présentation de l'Agent SDK · Cursor"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["agent", "equipe", "agents", "code", "chaque", "votre", "cursor", "creer"]
source: https://cursor.com/blog/agent-sdk
source_anchor: ""
source_lines: [1, 52]
section: "Cursor_IDE"
---
Les équipes de développement s'appuient sur des agents définis sous forme de code pour prendre en charge les tâches récurrentes tout au long du cycle de vie du développement. Ces agents se connectent aux outils de l'équipe et interviennent dès que des événements pertinents se produisent, qu'il s'agisse d'examiner une pull request, d'exécuter un workflow de conformité ou d'enquêter sur un incident.

Les agents les plus utiles sont ceux qui s'adaptent au fonctionnement propre à chaque équipe. Jusqu'à présent, en créer un impliquait généralement d'adopter un produit conçu autour du workflow de quelqu'un d'autre, ou de repartir de zéro.

Aujourd'hui, nous lançons l'Agent SDK, le framework que nous utilisons pour créer nos propres agents gérés, comme Bugbot, l'Agent de revue de sécurité et Rollouts. L'Agent SDK vous permet de définir des agents dans le code et de les gérer comme n'importe quel autre projet logiciel. Partez d'un modèle ou indiquez notre documentation à votre agent de codage. Une fois l'agent prêt, déployez-le sur l'infrastructure de Cursor en une seule commande : `agent-sdk deploy`.

L'Agent SDK vous permet de créer et de déployer rapidement des agents qui travaillent comme votre équipe. Ils réagissent aux événements que votre équipe gère déjà et suivent des politiques que votre équipe peut inspecter et modifier.

La revue de code en est un exemple. Chaque équipe a ses propres normes pour déterminer ce qui mérite de l'attention et qui doit s'en charger. Avec l'Agent SDK, vous pouvez créer un agent qui réagit aux événements de pull request provenant de GitHub, GitLab ou Bitbucket. L'agent peut évaluer le risque d'une modification, l'attribuer à la bonne équipe et approuver les modifications conformes à votre politique.

Par exemple, une équipe de Cursor avait constitué une base de connaissances volumineuse et en pleine croissance, recensant les enseignements tirés de son expérience, et souhaitait que chaque pull request soit vérifiée à partir de celle-ci. L'objectif : que les décisions passées de l'équipe restent valables à mesure que la base de code évolue. Avec l'Agent SDK, ils ont créé kb-reviewer en une journée environ. Il s'exécute désormais sur chaque PR de leur base de code principale, signale les modifications qui contredisent une leçon consignée, cite la note à l'origine de chaque résultat et incite les ingénieurs à résoudre les incohérences relevées.

Pour les tâches plus complexes, les agents peuvent confier une tâche à un autre agent spécialisé, doté de ses propres outils et sessions. Ils peuvent aussi répartir le travail entre plusieurs sous-agents qui opèrent en parallèle.

L'Agent SDK prend également en charge nativement les évaluations et la relecture d'événements, ce qui permet de développer rapidement tout en ayant la garantie que les flux de travail critiques s'exécutent comme prévu. Le PGA Tour s'appuie sur l'Agent SDK pour faire tourner son usine de développement logiciel. Grâce aux évaluations, ses équipes gardent un rythme soutenu.

L'Agent SDK a permis à la PGA de construire chaque composant de notre usine logicielle sur l'infrastructure de Cursor. Il nous offre la flexibilité nécessaire pour créer des agents qui pilotent notre processus de développement. Et grâce à la prise en charge intégrée des évaluations, nous pouvons itérer rapidement sur de nouvelles idées et optimiser de manière fiable les capacités existantes.


Un agent conçu avec l'Agent SDK s'organise comme un petit projet logiciel. Les instructions qui le guident côtoient le code de ses outils, ses connexions à des services externes et les évaluations qui servent à le tester.

Chaque fichier représente quelque chose que l'agent sait ou peut faire. Les requêtes sont rédigées en Markdown, et les outils sont définis sous forme de fonctions TypeScript. Puisque tout est du code, les modifications peuvent suivre le même processus de gestion de versions et de revue que le reste de votre logiciel.

Lorsque vous êtes prêt à mettre un agent en service, vous pouvez le déployer sur Cursor en exécutant `agent-sdk deploy` depuis le répertoire du projet. La commande crée l’agent, lui attribue une URL et le démarre sur notre infrastructure. Vous gérez les secrets et les intégrations depuis le tableau de bord, tout en continuant à utiliser les mêmes journaux, évaluations et playground qu’en local.

Vous pouvez également exécuter l’agent sur votre propre infrastructure. Chaque agent s’exécute sous la forme d’un unique processus Node, qui peut être empaqueté dans un conteneur.

Prendre en main l’Agent SDK, c’est un peu comme démarrer n’importe quel autre projet logiciel. Créez un répertoire de projet, puis lancez le playground local :

```
import { homedir } from "node:os";
import { join } from "node:path";
import { defineAgent } from "@cursor/agent-sdk";
export default defineAgent({
  name: "Robo",
  model: {
    id: "composer-2.5",
    params: [
      { id: "effort", value: "high" },
      { id: "fast", value: "true" },
    ],
  },
  local: {
    cwd: join(homedir(), ".cache", "factory", "robo"),
  },
})
```
Vous pouvez ensuite itérer en local et exécuter des évaluations avant de déployer avec `agent-sdk deploy`.

L'Agent SDK est également livré avec des skills qui guident les agents de codage tout au long du processus de développement. Vous pouvez orienter un agent vers la documentation et lui demander de vous aider à créer le bot.

L'Agent SDK est en disponibilité générale dès aujourd'hui. Consultez la documentation pour commencer.
