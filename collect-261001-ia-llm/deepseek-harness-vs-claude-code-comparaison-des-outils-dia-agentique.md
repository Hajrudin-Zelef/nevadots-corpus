Cursus

La plupart des comparatifs d’agents de code se résument à une course à la vitesse, au prix, au nombre d’outils ou aux scores de benchmark. Ce cadrage passe à côté de la vraie question qui distingue ces deux approches : jusqu’où un développeur peut-il remplacer les composants du runtime de l’agent ?

DeepSeek Harness et Claude Code répondent à cette question à des niveaux différents. Harness expose les adaptateurs de modèle, le stockage, les bacs à sable et la boucle d’agent sous forme de plugins remplaçables. Claude Code embarque sa propre boucle autour de Claude et prévoit des points d’extension pour le workflow. Ici, cette frontière compte davantage qu’une simple comparaison des modèles DeepSeek et Claude.

J’ai donné aux deux outils le même dépôt cassé et le même modèle Claude pour observer ce que change le runtime. Une étude de cas ne suffit pas à départager les produits, mais elle montre pourquoi le harness n’est pas un détail d’arrière-plan. Je me concentre sur les différences de choix de modèle, de configuration, de vérification, de journaux d’exécution et de coût.

## À retenir

- Si le runtime fait partie du travail : Harness expose l’adaptateur de modèle, le stockage, le bac à sable et la boucle d’agent comme plugins remplaçables, avec prise en charge de plusieurs fournisseurs de modèles.
- Si l’essentiel est le code applicatif : Claude Code embarque davantage de runtime et étend son workflow via des Skills, des hooks, MCP et fonctionnalités associées.
- Test avec le même modèle : les deux ont produit un patch identique à l’octet et passé la suite de tests d’origine. Dans cet essai sur Windows, Claude Code a annoncé 55,0 secondes ; Harness a enregistré 125,4 secondes après trois demandes d’approbation.
- Coût : DeepSeek Harness n’a pas de frais de licence ; l’usage du modèle est facturé par le fournisseur choisi, avec d’éventuels coûts d’infrastructure. Claude Code est inclus dans les offres Claude payantes.
- Règle simple : commencez avec Claude Code pour les travaux applicatifs courants. Optez pour Harness lorsque modifier ou inspecter le runtime fait partie du mandat.

## Introduction aux agents d'intelligence artificielle

## DeepSeek Harness vs Claude Code : comparaison rapide

| Dimension | DeepSeek Harness | Claude Code | 
|---|---|---|
| Ce que c’est | Runtime d’agent à composants remplaçables | Agent de code packagé | 
| Licence et statut | MIT, aperçu développeur, pas de version stable | Propriétaire, produit en production | 
| Modèles | DeepSeek, Anthropic, OpenAI, Bedrock, Vertex, Azure, local | Claude, en direct ou via Bedrock et Vertex | 
| Boucle remplaçable | Oui, point d’extension plugin substituable | Non, des extensions se greffent autour | 
| Unité d’extension | Plugin Cordis, toute capacité du runtime | Plugin regroupant skills, hooks, agents, MCP | 
| Historique d’exécution | Journal d’événements typés en ajout seul, relecturable | Transcription JSONL, checkpoints, OpenTelemetry | 
| Surfaces | Interface web locale, CLI headless, SDK Python | Terminal, IDE, desktop, web, Slack, CI | 
| Configuration | Nécessite la configuration du fournisseur et la maîtrise des profils | Configuration par défaut plus légère ; permissions, hooks, MCP et projet en option | 

## Qu’est-ce que DeepSeek Harness ?

DeepSeek Harness est un harness d’agent open source actuellement en aperçu développeur. Il fournit le runtime dans lequel les modèles utilisent des outils et du contexte, et il peut exécuter des modèles d’autres fournisseurs que DeepSeek. Son README avertit que des mises à jour peuvent casser des configurations existantes.

Si vous souhaitez l’essayer, commencez par notre tutoriel DeepSeek Harness.

### Comment fonctionne DeepSeek Harness

La boucle d’agent est implémentée comme un plugin. Harness utilise Cordis pour composer les adaptateurs de modèle, les outils, les sessions, le stockage, les bacs à sable et la boucle.

Cordis permet aux plugins de découvrir des services et d’échanger des événements. Les changements de dépendances chargent ou déchargent des plugins ; le rechargement à chaud nettoie les écouteurs obsolètes et les tâches en arrière-plan.

Le profil par défaut expose la boucle comme une configuration. `dsh --profile headless --dump-default-config` inclut cette entrée :

```
- id: agent-loop
  name: '@deepseek-ai/dsh-agent-loop'
  config:
    agents: []
```
Cette entrée peut être remplacée via `cordis.patch.yml`, preuve que la boucle elle-même n’est pas figée.

Panneau des plugins affichant les composants du runtime. Vidéo de l’auteur.

### Les quatre modes d’exécution

DeepSeek Harness propose quatre modes d’exécution distincts.

- **Standard** : mode agent de code complet
- **PTC** /**Code** : combine plusieurs appels d’outils en un programme TypeScript
- **Minimal** : ne conserve que bash persistant et str_replace_editor
- **Creator** : pour inspecter et tester le runtime

DeepSeek a utilisé le mode *Minimal* pour ses benchmarks.

## Qu’est-ce que Claude Code ?

Claude Code est l’agent de code propriétaire d’Anthropic et un harness agentique pour un usage local ou managé via le terminal, l’IDE, le desktop, le web, Slack et le CI.

La CLI se lance avec `claude` dans le répertoire du projet. Ce répertoire devient le périmètre de fichiers par défaut, et la session lit les instructions du projet depuis `CLAUDE.md`. Les utilisateurs peuvent élargir l’accès aux fichiers ou ajouter des services externes ensuite.

Anthropic présente ces interfaces comme des moyens d’accéder à Claude Code. Les sessions locales s’exécutent sur votre machine. Les sessions cloud tournent dans des environnements managés ou sur des serveurs opérés par votre organisation. Le Remote Control permet de piloter un travail local depuis un navigateur.

Les meilleurs points de départ sont notre tutoriel Claude Code et le guide des bonnes pratiques Claude Code.

### Boucle intégrée et extensions

Anthropic décrit Claude Code comme le harness agentique autour de Claude, avec une boucle répétée de contexte, action et vérification. Les utilisateurs peuvent l’orienter pendant l’exécution.

Claude Code stocke les sessions en local et compacte les anciens contextes à mesure que la fenêtre se remplit. `CLAUDE.md` et la mémoire automatique conservent des instructions et détails de projet sélectionnés d’une session à l’autre. Les sous-agents utilisent des fenêtres de contexte séparées et renvoient des synthèses à la session parente.

### Comment fonctionne la couche d’extension de Claude Code

Claude Code prend en charge plusieurs mécanismes d’extension. `CLAUDE.md`, skills, hooks, MCP, subagents, plugins, Agent Teams et l’Agent SDK étendent tous son workflow.

Ces extensions opèrent autour de la boucle intégrée de Claude Code. Les hooks peuvent imposer des règles d’appels d’outils, tandis que l’Agent SDK fournit des outils et la gestion du contexte dans le code.

## DeepSeek Harness vs Claude Code : architecture et contrôle

DeepSeek Harness expose des éléments de runtime de plus bas niveau, remplaçables. Claude Code conserve sa boucle intégrée et prend en charge des extensions autour d’elle.

### Runtime remplaçable vs agent packagé

DeepSeek Harness considère le runtime comme une infrastructure configurable : adaptateurs de modèle, stockage, bacs à sable et boucle peuvent être remplacés via des entrées de profil. Claude Code garde sa boucle intégrée fixe et étend le workflow autour.

Harness peut utiliser ce mécanisme pour échanger les fournisseurs de modèles ou d’autres parties du runtime. Le mot « plugin » n’a pas la même portée ici :

- Les plugins Claude Code agrègent des fonctionnalités autour de la boucle.
- Les plugins DeepSeek Harness peuvent définir des pans du runtime lui-même.

### Prise en charge des modèles et choix du fournisseur

DeepSeek Harness prend en charge davantage de fournisseurs de modèles. Il exécute DeepSeek, Anthropic, OpenAI, des clouds et des endpoints locaux compatibles. Claude Code exécute… Claude. En d’autres termes, DeepSeek Harness peut héberger Claude, mais Claude Code ne peut pas héberger DeepSeek.

C’est ce qui m’a permis de garder le même modèle lors des tests. Comparer un modèle DeepSeek dans Harness à Claude dans Claude Code change deux variables à la fois.

Le choix du fournisseur implique la configuration des identifiants et de l’endpoint, y compris les IDs de modèle exacts. Claude Code contrôle la famille de modèles et la plupart des paramètres de requête.

Changer de fournisseur ne garantit pas un comportement identique. Les modèles peuvent différer par le format d’outillage, la taille de contexte ou les contrôles de raisonnement. Harness garde la configuration des plugins accessible, mais l’adaptateur fournisseur doit rester compatible avec l’endpoint choisi.

### Journaux de session et traçabilité

DeepSeek Harness enregistre invites, injections de contexte, appels d’outils et décisions de permission dans un flux d’événements append-only. Sa vue « trajectory » montre l’origine de chaque enregistrement et prend en charge la reprise, le fork, la recherche et la relecture.

Claude Code stocke des transcriptions JSONL, prend en charge reprise et fork, et émet des traces OpenTelemetry.

Les deux exposent l’historique de session, mais DeepSeek Harness met davantage le chemin d’exécution en avant dans l’UI.

Vue « trajectory » reconstruisant une exécution complète. Vidéo de l’auteur.

### Environnements d’exécution et permissions

DeepSeek Harness utilise bubblewrap ou Landlock sous Linux, Seatbelt sous macOS, et un jeton restreint par ACL sous Windows. Si le bac à sable ne peut pas démarrer, Harness bloque la commande. Claude Code propose des modes de permission ainsi que des règles allow, ask et deny. Dans ce run, le bac à sable Windows a influencé le chemin de vérification et le timing.

Les libellés d’accès diffèrent, rendant impossible un alignement strict des réglages.

- 
Les préréglages de permissions de DeepSeek Harness incluent `read-only` ,`workspace-write` et`danger-full-access` .
- 
Claude Code sépare les modifications automatiques, l’approbation manuelle, la planification et l’accès aux commandes par règles. Il prend aussi des snapshots des fichiers avant modification pour un rollback sans Git.

Sur les sessions Pro, Max et Team actuelles dans le terminal et VS Code, le mode Auto est le mode de démarrage intégré de Claude Code. Un classifieur révise les actions en arrière-plan. Le test a explicitement utilisé `acceptEdits` ; son comportement en matière de permissions reflète donc la configuration de test plutôt que le défaut actuel.

Cette nuance compte lorsqu’on compare le nombre d’approbations.

Le lieu d’exécution diffère aussi. Harness s’exécute principalement sur la machine qui héberge son interface web ou un processus headless. Claude Code peut tourner en local, dans des environnements cloud managés par Anthropic, ou sur une infrastructure autohébergée. Remote Control ajoute une interface navigateur tandis que l’exécution reste locale.

## Test pratique DeepSeek Harness vs Claude Code

Pour vérifier si ces différences de runtime affectaient l’exécution, j’ai confié aux deux outils le même bug TypeScript d’une ligne. Si vous voulez uniquement le résultat chronométré, c’est la section à lire.

### Configuration du test : même modèle, même dépôt, même prompt

J’ai écrit une bibliothèque TypeScript de « habit-streak » avec une ligne fautive. Elle mesurait les jours calendaires en arrondissant les millisecondes écoulées.

- Une validation à 23:50 suivie d’une autre à 00:10 semblait appartenir au même jour.
- Une validation à 08:00 suivie de 20:00 le lendemain semblait indiquer un jour manqué.

Dix tests figés révélaient trois échecs.

Chaque nouveau clone avait ses dépendances installées avant le chronométrage. Les deux agents ont reçu la même instruction et utilisé Claude Sonnet 5. Le dépôt et le prompt étaient fixes ; les outils, permissions et comportements de bac à sable restaient spécifiques à chaque produit.

Les configurations de permissions n’étaient pas équivalentes. DeepSeek Harness a démarré en `workspace-write`, mais son bac à sable Windows n’a pas pu se lancer. L’interface web a demandé une approbation, et j’ai accordé `danger-full-access` à trois reprises. Claude Code utilisait `accept-edits` et ne pouvait exécuter que la commande de test via bash.

Cette tâche ciblée n’utilisait que du code et des tests locaux. Les agents devaient trouver et corriger une ligne défectueuse, puis exécuter la suite. Les dépendances étaient préinstallées avant le chrono, bien que Harness ait choisi de relancer `npm install`.

### Résultats : patch identique, chemins d’approbation différents

Claude Code a indiqué 55,0 secondes. Il a exécuté la suite, trouvé le bug, modifié un fichier source et relancé les tests. Dix sur dix ont réussi.

Dans ce test Windows, DeepSeek Harness a pris 125,4 secondes. Il a trouvé le même helper `daysBetween` inutilisé, appliqué la même correction et exécuté avec succès la suite d’origine.

Les diffs étaient identiques à l’octet :

```
-import { DAY_MS } from './dates.js';
+import { daysBetween } from './dates.js';
 
 function gapInDays(earlier: number, later: number): number {
-  return Math.round((later - earlier) / DAY_MS);
+  return daysBetween(earlier, later);
 }
```
Le parcours d’approbation et les répétitions côté shell expliquent l’essentiel de l’écart de temps.

### Pourquoi DeepSeek Harness a été plus lent sous Windows

Le bac à sable `workspace-write` de DeepSeek Harness n’a pas pu démarrer sous Windows, si bien que l’interface web a demandé à trois reprises un mode de permission plus large. Après approbation, Harness a exécuté la suite d’origine, modifié `src/streak.ts`, puis relancé la suite. Dix tests sur dix ont réussi.

Ce résultat ne dit rien du comportement de Harness sous Linux ou macOS. Comme les deux exécutions utilisaient le même ID de modèle, l’écart de temps ne peut pas venir d’une différence de famille de modèles. Les permissions et chemins d’outils distincts ont toutefois façonné chaque run.

Les deux exécutions atteignent dix tests réussis. Image de l’auteur.

### Ce que le test avec le même modèle isole

Partager le même ID de modèle n’a pas rendu les requêtes identiques. Chaque produit a fourni son propre prompt système, ses descriptions d’outils, son contexte et ses règles de permission. Chacun de ces apports peut influencer la réponse suivante du modèle.

La configuration contrôle donc mieux la famille de modèles qu’une comparaison « modèle DeepSeek vs Claude », sans pour autant isoler toutes les variables.

Je n’ai réalisé qu’un essai chronométré par configuration. Des répétitions, dans un ordre aléatoire, seraient nécessaires avant de considérer les temps ou les comptes d’outils comme des mesures de performance stables.

## DeepSeek Harness vs Claude Code : coûts, benchmarks et limites des tests

Une étude de cas sous Windows ne peut pas trancher la comparaison générale de performance ou de prix. Elle montre cependant pourquoi les détails de benchmark comptent.

### Avertissement sur les benchmarks : DeepSeek a utilisé le mode Minimal

Les scores publiés par DeepSeek utilisaient le mode Minimal, et non le mode Standard utilisé dans cette comparaison. Des tests indépendants avec un autre harness ont rapporté des résultats inférieurs. Je ne les ai pas reproduits. Les résultats en mode Minimal n’établissent pas les performances du mode Standard et ne mesurent pas Claude Code.

### Coût : appels d’API vs abonnements Claude

DeepSeek Harness n’a pas de frais de licence, mais les coûts de modèles et d’infrastructure s’appliquent. L’API de DeepSeek utilise des tarifs de pointe et hors pointe. Claude Code est inclus dans les offres Claude payantes. Consultez la page tarifs d’Anthropic avant de budgéter.

L’exécution avec Harness a coûté environ 0,11 $ aux tarifs publics de l’API Anthropic. Claude Code affichait 0,349 $ tout en utilisant des identifiants d’abonnement. Comme ces chiffres proviennent de systèmes de facturation différents, ce n’est pas une comparaison directe de prix.

Les offres individuelles de Claude listent Pro à 20 $ par mois et Max à 100 $ ou 200 $ par mois, tandis que DeepSeek Harness n’a pas d’abonnement équivalent ; c’est le fournisseur de modèle qui facture. Les modèles locaux éliminent les frais d’API mais mobilisent tout de même les ressources de la machine.

Les équipes doivent également prendre en compte les bacs à sable hébergés et le calcul autohébergé lorsque ces services sont facturés en dehors du modèle. Ces coûts n’apparaissent pas dans la licence Harness.

### Stabilité : aperçu développeur vs produit en production

Harness était encore en release candidate lors des tests. Les équipes devraient figer les versions des paquets, car des mises à jour peuvent casser des profils ou des sessions enregistrées ; Claude Code est déjà en production.

## Limites de DeepSeek Harness vs Claude Code

Aucun des deux systèmes ne couvre tous les workflows. Leurs limites proviennent de choix de conception différents.

### Limites de DeepSeek Harness

Harness expose davantage de composants du runtime, ce qui laisse aussi plus de configuration et de tests à la charge de l’utilisateur :

- 
Le projet reste en aperçu développeur, et des mises à jour peuvent casser des profils ou des sessions enregistrées
- 
Les identifiants fournisseur, IDs de modèles et règles d’endpoint nécessitent une configuration manuelle
- 
Son bac à sable `workspace-write` n’a pas démarré dans cette étude de cas Windows et a exigé trois approbations`danger-full-access`
- 
Le SDK Python actuel n’a pas de wheel Windows

Le constat sur le bac à sable vaut uniquement pour l’exécution Windows décrite plus haut. Je n’ai pas répété le test sous Linux ou macOS.

Les notes de version v0.1.0-rc.7 mentionnent une correction de latence Bash persistante et une mise à jour de `node-pty` pour une meilleure compatibilité PTY. Cette observation est donc à la fois spécifique à la version et à Windows.

### Limites de Claude Code

Claude Code prend en charge davantage de décisions de runtime, mais ses frontières fixes empêchent certains types d’expérimentations :

- 
Il exécute des modèles Claude et ne fournit pas de voie pour d’autres familles de modèles
- 
Sa boucle de session intégrée n’est pas exposée comme composant remplaçable
- 
Claude Code nécessite un accès payant ou des crédits d’API facturés séparément, avec des limites d’usage
- 
Les longues sessions peuvent compacter l’ancien contexte ; placez donc les règles persistantes dans `CLAUDE.md`

Ces limites comptent surtout lorsqu’une tâche requiert un autre fournisseur de modèles ou une boucle personnalisée.

## DeepSeek Harness vs Claude Code : lequel choisir ?

Mettez les noms des modèles de côté un instant. Votre tâche consiste-t-elle à terminer le code applicatif, ou à modifier le runtime qui fait le travail ?

Choisissez DeepSeek Harness si le runtime lui-même est le projet. Il convient aux tests multi-fournisseurs, aux boucles ou bacs à sable sur mesure, et aux travaux nécessitant des traces d’exécution détaillées. Son statut de préversion implique aussi d’assumer les changements de version et la validation de configuration.

Choisissez Claude Code si vous travaillez sur une base de code existante et souhaitez que l’agent explore les fichiers, modifie le code et exécute les contrôles du projet avec moins de configuration de runtime. Dans ce test Windows, il a exécuté la suite d’origine sans les trois approbations d’accès total exigées par Harness.

Utilisez les deux si cette répartition reflète votre réalité. Une équipe peut utiliser Claude Code pour le quotidien et Harness pour expérimenter sur les prompts, les outils ou la boucle d’agent.

## Conclusion

Les deux outils savent inspecter un dépôt, modifier des fichiers et exécuter des commandes. La différence tient à la maîtrise du runtime : Harness expose ses composants sous forme de plugins, tandis que Claude Code packe la boucle intégrée et vous permet d’étendre le workflow autour.

Si je devais choisir un point de départ sur la base de ce test, je commencerais par Claude Code pour le travail applicatif courant. Je partirais sur Harness quand il s’agit de modifier ou d’inspecter le runtime, parce que c’est l’objet même de la tâche. Il s’agit d’une étude de cas Windows avec Harness en préversion, pas d’un classement définitif.

Pour aller plus loin, consultez nos articles sur Grok Build, Cursor, OpenCode, Codex et d’autres alternatives à Claude Code.

## DeepSeek Harness vs Claude Code : FAQ

### DeepSeek Harness peut-il réutiliser les instructions de projet de Claude Code ?

**Oui. Harness lit `CLAUDE.md` et `AGENTS.md`, de sorte que les instructions du dépôt peuvent être réutilisées. Les Skills de Claude Code restent séparées.**

### DeepSeek Harness peut-il exécuter Claude Code ou Codex comme sous-agent ?

**Oui. Les builds de préversion incluent des bundles de fournisseur de sous-agent optionnels pour Claude Code et Codex. Figez la version de Harness avant de vous reposer sur leur configuration.**

### Claude Code permet-il de remplacer le modèle par un autre ?

Non, il exécute uniquement Claude. Harness peut héberger Claude, mais Claude Code ne peut pas héberger DeepSeek.

### Quelles plateformes prennent actuellement en charge le SDK Python de DeepSeek Harness ?

**La wheel actuelle prend en charge Linux en x64 ou arm64 ainsi que les versions récentes de macOS arm64. Il n’existe pas de wheel Windows, et l’exemple de composition utilise un accès complet au système de fichiers.**

### Claude Code a-t-il des restrictions de plateforme comme le SDK Python de Harness ?

Pas exactement. Il n’a pas de bac à sable Windows natif, donc il nécessite WSL2 pour `/sandbox` sous Windows, mais la CLI elle-même fonctionne sous macOS, Linux et Windows.

### Le mode Code de DeepSeek Harness peut-il exécuter des programmes Python ?

**Pas avec le backend fourni. Le mode PTC (aussi appelé Code) reconnaît les langages de programmation, mais le runtime documenté propose actuellement un backend TypeScript.**

### DeepSeek Harness peut-il exécuter des modèles locaux ?

**Oui. Ajoutez un fournisseur personnalisé exposant un endpoint compatible OpenAI, tel qu’un serveur local Ollama ou vLLM, puis enregistrez l’ID de modèle dans DeepSeek Harness.**

Je suis ingénieur de données et créateur de communautés. Je travaille sur les pipelines de données, le cloud et les outils d'IA, tout en rédigeant des tutoriels pratiques et percutants pour DataCamp et les développeurs émergents.