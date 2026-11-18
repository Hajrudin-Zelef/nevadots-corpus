En 2025, GitGuardian a détecté **28,65 millions de nouveaux secrets** (clés API, mots de passe, jetons d’accès) codés en dur dans les dépôts publics de GitHub, soit une hausse de 34 % sur un an, la plus forte progression jamais enregistrée par l’éditeur dans son rapport State of Secrets Sprawl 2026. Pire encore : parmi les identifiants fuités en 2022, 64 % étaient toujours valides en janvier 2026. Autrement dit, la grande majorité des secrets qui fuitent dans le code ne sont jamais révoqués. Pour les équipes techniques françaises et européennes soumises à la directive NIS2 et au Cyber Resilience Act, ce chiffre n’est plus une simple statistique de conférence sécurité : c’est un risque de conformité direct. Ce tutoriel explique comment déployer GitLeaks, l’outil open source de référence pour scanner un dépôt Git à la recherche de secrets exposés, du premier scan jusqu’à l’intégration complète dans une chaîne CI/CD.

Ce sujet n’a rien de théorique pour les organisations françaises. Les incidents récents impliquant des fuites massives de données personnelles ou administratives rappellent régulièrement qu’un identifiant oublié dans un dépôt de code, un fichier de configuration ou un script d’automatisation peut servir de porte d’entrée initiale à un attaquant, bien avant toute exploitation de vulnérabilité logicielle complexe. Les équipes qui gèrent des applications exposées sur Internet, des API publiques ou des pipelines de déploiement automatisés ont donc tout intérêt à traiter le scan de secrets comme un contrôle de base, au même titre qu’un pare-feu applicatif ou une authentification à double facteur.

## Pourquoi le scan de secrets est devenu incontournable en 2026

Le phénomène a un nom dans l’industrie : le “secrets sprawl”, la dissémination incontrôlée de secrets dans le code source. Selon le rapport State of Secrets Sprawl 2026 de GitGuardian, plus de 4 012 054 dépôts publics GitHub contenaient au moins un secret en 2025, avec une moyenne de 0,32 secret par dépôt. La croissance des secrets exposés (+152 % depuis 2021) dépasse largement celle du nombre de développeurs actifs sur la plateforme (+98 % sur la même période). Un facteur aggrave la tendance depuis 2025 : l’essor du code généré par IA. Les clés d’API liées aux services d’intelligence artificielle ont bondi de 81 % en un an, avec plus de 1,27 million de secrets détectés pour ces seuls services. Les outils de complétion de code assistés par IA font fuiter des secrets à un rythme environ deux fois supérieur à la moyenne observée sur l’ensemble de GitHub.

Cette explosion coïncide avec un durcissement réglementaire en Europe. La directive NIS2 impose aux entités essentielles et importantes de mettre en place des mesures techniques de gestion des vulnérabilités sur toute leur chaîne d’approvisionnement logicielle, ce qui inclut la gestion des identifiants dans le code source. Le Cyber Resilience Act, de son côté, exige des pratiques de développement sécurisé et une gestion continue des vulnérabilités pendant tout le cycle de vie d’un produit numérique. Un secret oublié dans un commit peut désormais être qualifié de faille de sécurité au sens réglementaire, avec des obligations de notification et de remédiation associées. Le scan de secrets, longtemps considéré comme une bonne pratique optionnelle, devient une brique de conformité difficile à ignorer pour toute organisation qui publie du code en Europe. L’ENISA, l’agence de l’Union européenne pour la cybersécurité, classe d’ailleurs la gestion des secrets et des identifiants parmi les mesures techniques prioritaires de sécurisation du cycle de développement logiciel dans ses recommandations aux États membres.

## Qu’est-ce que GitLeaks et comment il détecte les secrets

GitLeaks est un outil en ligne de commande, écrit en Go, entièrement gratuit et open source, distribué sous licence MIT via son dépôt GitHub officiel. Il scanne des dépôts Git, des chemins de fichiers arbitraires ou un flux stdin à la recherche de motifs correspondant à des secrets. Son moteur de détection combine deux approches complémentaires. La première repose sur des règles regex prédéfinies, chacune ciblant un format de secret précis : les clés d’accès AWS commencent toujours par un préfixe reconnaissable, les jetons GitHub personnels suivent un format documenté, de même pour les clés d’API Stripe, Slack ou Google Cloud. La seconde approche s’appuie sur l’entropie : GitLeaks calcule le niveau d’aléatoire d’une chaîne de caractères et la signale comme suspecte si elle dépasse un seuil, en particulier lorsqu’elle est assignée à une variable nommée de façon évocatrice comme API_KEY ou SECRET_TOKEN.

La version stable actuelle est GitLeaks v8.30.1, publiée le 21 mars 2026. Toute la configuration de l’outil passe par un fichier au format TOML, généralement nommé .gitleaks.toml, qui définit les règles actives, les listes d’exclusion (allowlists) par fichier, par chemin ou par auteur de commit, ainsi que le format des rapports générés (JSON, SARIF, CSV). GitLeaks embarque une configuration par défaut couvrant plusieurs centaines de types de secrets connus, que chaque équipe peut ensuite étendre avec ses propres règles internes.

## Prérequis : versions et environnement nécessaires

Avant de commencer, vérifiez que votre environnement dispose des éléments suivants. Ce tutoriel a été testé sur Ubuntu 24.04 LTS, macOS 15 et Windows 11 avec WSL2.

- **Git** version 2.30 ou supérieure (nécessaire pour l’historique complet et les hooks)
- **GitLeaks** v8.30.1 (dernière version stable, 21 mars 2026)
- **Go** 1.21 ou supérieur, uniquement si vous compilez GitLeaks depuis les sources
- **Docker** 24.0 ou supérieur, pour l’exécution en conteneur (optionnel mais recommandé en CI)
- **Python** 3.9 ou supérieur et le framework**pre-commit** 3.6 ou supérieur, pour les hooks locaux
- Un compte **GitHub** avec GitHub Actions activé, ou un compte**GitLab** avec GitLab CI/CD activé
- Un accès administrateur au dépôt cible, indispensable si vous devez réécrire l’historique Git
- **git-filter-repo** version 2.45 ou supérieure, pour la purge de l’historique (voir étape 11)

Comptez environ 60 minutes pour parcourir l’intégralité des 13 étapes sur un dépôt de taille moyenne, en incluant les tests d’intégration CI/CD. La réécriture de l’historique sur un très gros dépôt (plusieurs dizaines de milliers de commits) peut prendre nettement plus longtemps.

## Étape 1 : Installer GitLeaks selon votre système

GitLeaks propose plusieurs méthodes d’installation selon votre plateforme. Sur macOS, la méthode la plus rapide passe par Homebrew. Sur Linux, vous pouvez télécharger le binaire précompilé depuis la page de releases GitHub ou utiliser un gestionnaire de paquets tiers. Sur Windows, l’exécutable est également disponible en téléchargement direct, et fonctionne nativement dans WSL2.

```
# macOS via Homebrew
brew install gitleaks
# Linux : téléchargement du binaire v8.30.1
curl -sSL https://github.com/gitleaks/gitleaks/releases/download/v8.30.1/gitleaks_8.30.1_linux_x64.tar.gz -o gitleaks.tar.gz
tar -xzf gitleaks.tar.gz
sudo mv gitleaks /usr/local/bin/
# Vérification de l'installation
gitleaks version
# Alternative via Docker (sans installation locale)
docker pull zricethezav/gitleaks:v8.30.1
```
Une fois l’installation terminée, la commande gitleaks version doit afficher 8.30.1. Si vous préférez une approche totalement isolée sans installer de binaire sur votre machine, l’image Docker officielle zricethezav/gitleaks est mise à jour à chaque release et constitue l’option recommandée pour les environnements CI/CD partagés.

## Étape 2 : Lancer votre premier scan avec gitleaks detect

Placez-vous à la racine d’un dépôt Git existant et lancez la commande de détection. GitLeaks analyse par défaut l’ensemble de l’historique des commits, pas seulement l’état actuel des fichiers, ce qui est essentiel puisqu’un secret supprimé du code reste présent dans l’historique tant qu’il n’est pas purgé.

```
cd mon-projet/
gitleaks detect --source . --verbose
# Pour scanner uniquement l'état actuel (sans historique)
gitleaks detect --source . --no-git --verbose
# Pour générer un rapport au format JSON
gitleaks detect --source . --report-format json --report-path gitleaks-report.json
```
Le code de sortie de la commande est déterminant pour l’automatisation : GitLeaks retourne 0 si aucun secret n’est trouvé, et 1 dès qu’au moins un secret est détecté. C’est ce code de sortie qui permet de bloquer un commit, une pull request ou un pipeline CI en cas de fuite.

## Étape 3 : Lire et interpréter le rapport GitLeaks

Chaque secret détecté génère une entrée détaillée dans le rapport, contenant le fichier concerné, le numéro de ligne, le hash du commit, l’auteur, la date, la règle qui a déclenché l’alerte et un extrait tronqué du secret (jamais affiché en clair dans les logs par défaut, pour éviter une double fuite via les journaux CI). Voici un exemple de sortie console typique lors d’un scan qui trouve une clé AWS oubliée dans un fichier de configuration :

```
Finding:     AKIA************WXYZ
Secret:      AKIA****************
RuleID:      aws-access-token
Entropy:     3.912345
File:        config/settings.py
Line:        42
Commit:      a1b2c3d4e5f6...
Author:      [email protected]
Date:        2026-06-14T09:12:00Z
10:34AM INF 1 commits scanned.
10:34AM INF scan completed in 842ms
10:34AM WRN leaks found: 1
```
Chaque ligne du rapport doit être triée en deux catégories : un vrai secret actif à révoquer immédiatement, ou un faux positif (une clé de test, une donnée d’exemple dans la documentation, un placeholder). C’est cette distinction qui justifie l’étape suivante, la configuration d’une allowlist.

## Étape 4 : Personnaliser la configuration avec .gitleaks.toml

La configuration par défaut de GitLeaks couvre la grande majorité des cas d’usage, mais chaque organisation possède ses propres formats de secrets internes (jetons d’API maison, identifiants de bases de données propriétaires). Créez un fichier .gitleaks.toml à la racine du dépôt pour étendre les règles existantes.

```
title = "Configuration GitLeaks — Projet Interne"
[extend]
useDefault = true
[[rules]]
id = "jeton-interne-acme"
description = "Jeton d'API interne ACME Corp"
regex = '''acme_[a-zA-Z0-9]{32}'''
tags = ["api", "interne"]
[[rules]]
id = "chaine-connexion-postgres"
description = "Chaîne de connexion PostgreSQL avec mot de passe"
regex = '''postgres(?:ql)?:\/\/[^:]+:[^@]+@[^\/\s]+'''
tags = ["database", "credentials"]
```
La directive useDefault = true conserve toutes les règles fournies nativement par GitLeaks tout en ajoutant vos propres motifs. Relancez ensuite gitleaks detect –source . –config .gitleaks.toml pour valider que la nouvelle règle fonctionne, idéalement contre un fichier de test contenant volontairement un faux jeton correspondant au format attendu.

## Étape 5 : Gérer les faux positifs avec une allowlist

Sans allowlist correctement configurée, un projet contenant de la documentation avec des exemples de clés, des fixtures de tests ou des fichiers de vendoring tiers génère rapidement des dizaines de fausses alertes, ce qui use la vigilance des équipes et pousse à ignorer les vrais résultats. GitLeaks permet d’exclure des chemins entiers, des regex spécifiques ou des commits précis.

```
[allowlist]
description = "Exclusions globales du projet"
paths = [
  '''node_modules''',
  '''vendor''',
  '''(.*)_test\.go''',
  '''docs/exemples/'''
]
regexes = [
  '''EXEMPLE_[A-Z0-9]{16}''',
  '''your-api-key-here'''
]
commits = [
  "a1b2c3d4e5f6789012345678901234567890abcd"
]
```
Attention à ne pas transformer l’allowlist en solution de facilité : chaque exclusion doit être documentée et justifiée dans une revue de code, sinon elle devient elle-même un angle mort de sécurité, un vrai secret pouvant se glisser discrètement dans un chemin exclu à tort.

## Étape 6 : Bloquer les secrets avant le commit avec un hook pre-commit

La détection la plus efficace intervient avant même que le secret n’atteigne le dépôt distant. Le framework pre-commit permet d’exécuter GitLeaks automatiquement à chaque tentative de commit local, en scannant uniquement les fichiers modifiés (staged). Installez le framework puis déclarez GitLeaks dans le fichier de configuration.

```
# Installation du framework pre-commit
pip install pre-commit
# Fichier .pre-commit-config.yaml à la racine du projet
cat > .pre-commit-config.yaml << 'EOF'
repos:
  - repo: https://github.com/gitleaks/gitleaks
    rev: v8.30.1
    hooks:
      - id: gitleaks
EOF
# Activation du hook dans le dépôt local
pre-commit install
```
À partir de cet instant, chaque git commit déclenche automatiquement gitleaks protect sur les fichiers indexés. Si un secret est détecté, le commit est bloqué et un message d'erreur détaillé s'affiche dans le terminal, obligeant le développeur à retirer le secret avant de poursuivre.

## Étape 7 : Automatiser le scan dans GitHub Actions

Un hook local peut être contourné avec l'option --no-verify, volontairement ou par mégarde. La ligne de défense suivante s'exécute donc côté serveur, dans le pipeline CI, sur chaque push et chaque pull request. Créez le fichier .github/workflows/gitleaks.yml suivant.

```
name: Scan GitLeaks
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]
jobs:
  gitleaks:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - name: Exécuter GitLeaks
        uses: gitleaks/gitleaks-action@v2
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          GITLEAKS_CONFIG: .gitleaks.toml
```
L'option fetch-depth: 0 est indispensable : sans elle, GitHub Actions ne récupère qu'un clone superficiel du dépôt et GitLeaks ne peut analyser que le dernier commit, laissant passer un secret introduit plus tôt dans la même pull request. Le pipeline échoue automatiquement (statut rouge sur la pull request) dès qu'un secret est détecté, empêchant la fusion tant que le problème n'est pas résolu.

## Étape 8 : Intégrer GitLeaks à GitLab CI

Pour les équipes hébergées sur GitLab, le principe est identique mais la syntaxe change. Ajoutez le job suivant à votre fichier .gitlab-ci.yml, en utilisant l'image Docker officielle pour éviter toute installation manuelle du binaire dans le runner.

```
scan-secrets:
  stage: test
  image:
    name: zricethezav/gitleaks:v8.30.1
    entrypoint: [""]
  script:
    - gitleaks detect --source . --report-format sarif --report-path gitleaks.sarif --exit-code 1
  artifacts:
    reports:
      sast: gitleaks.sarif
    when: always
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
    - if: '$CI_COMMIT_BRANCH == "main"'
```
L'option --exit-code 1 garantit que le job échoue en cas de détection, ce qui bloque la merge request dans les règles de protection de branche si celles-ci exigent un pipeline vert. Le format de sortie SARIF permet également d'alimenter directement le tableau de bord de sécurité intégré à GitLab Ultimate.

## Étape 9 : Scanner tout l'historique Git en profondeur

Un scan CI classique porte généralement sur les commits récents d'une pull request, pas sur l'intégralité de l'historique du dépôt. Or un audit initial, avant la mise en place de GitLeaks, doit couvrir tous les commits depuis la création du projet. Utilisez l'option --log-opts pour cibler une plage précise, ou omettez-la pour scanner tout l'historique.

```
# Scan complet de tout l'historique du dépôt
gitleaks detect --source . --log-opts="--all" --verbose
# Scan limité aux 12 derniers mois
gitleaks detect --source . --log-opts="--since=12.months" --verbose
# Scan d'une branche spécifique uniquement
gitleaks detect --source . --log-opts="feature/nouvelle-api" --verbose
```
Sur un dépôt actif depuis plusieurs années, ce scan initial fait souvent remonter des dizaines, parfois des centaines de résultats. Ne cédez pas à la tentation de tout corriger en urgence : classez les résultats par gravité (secret probablement encore valide, secret révoqué depuis, faux positif) avant d'agir, comme détaillé à l'étape suivante.

## Étape 10 : Réagir face à un secret détecté (rotation immédiate)

Dès qu'un secret est confirmé, la règle de base de l'industrie est simple : considérez-le comme compromis, quelle que soit la probabilité réelle qu'il ait été exploité, surtout s'il a été poussé un jour vers un dépôt distant, même privé. La procédure de remédiation suit toujours le même ordre :

- **Révoquer** immédiatement la clé ou le mot de passe compromis dans la console du fournisseur concerné (AWS IAM, GitHub Settings, tableau de bord Stripe, etc.)
- **Générer** un nouveau secret et mettre à jour toutes les applications et services qui en dépendent
- **Vérifier les journaux d'accès** du fournisseur pour détecter une éventuelle utilisation frauduleuse pendant la fenêtre d'exposition
- **Rechercher les réutilisations** du même secret dans d'autres dépôts ou environnements, une pratique malheureusement fréquente
- **Remplacer** le secret en dur dans le code par une variable d'environnement ou un appel à un gestionnaire de secrets externe comme HashiCorp Vault

La rotation seule ne suffit pas : tant que le secret révoqué reste visible dans l'historique Git, il continue à polluer les scans futurs et peut induire en erreur un auditeur externe. C'est l'objet de l'étape suivante.

## Étape 11 : Purger l'historique avec git-filter-repo

Supprimer un secret du dernier commit ne l'efface pas de l'historique : il reste récupérable via n'importe quel clone antérieur. Deux outils dominent la réécriture d'historique Git : BFG Repo-Cleaner, rapide et écrit en Java, et git-filter-repo, l'outil désormais recommandé officiellement par le projet Git en remplacement de git filter-branch, jugé trop lent et sujet aux erreurs. Le choix entre les deux dépend surtout de l'environnement disponible : git-filter-repo ne nécessite qu'un interpréteur Python, tandis que BFG requiert une machine virtuelle Java installée sur le poste qui effectue la réécriture.

```
# Installation de git-filter-repo
pip install git-filter-repo
# Créer un fichier listant le texte exact du secret à purger
echo "AKIAEXEMPLEDECLEEXPOSEE==>SUPPRIME" > secrets-a-purger.txt
# Réécriture de l'historique complet
git filter-repo --replace-text secrets-a-purger.txt
# Alternative avec BFG Repo-Cleaner (nécessite Java)
java -jar bfg.jar --replace-text secrets-a-purger.txt mon-depot.git
# Forcer la mise à jour du dépôt distant après réécriture
git push origin --force --all
git push origin --force --tags
```
Cette opération réécrit tous les hashes de commit après le point d'insertion du secret, ce qui casse tous les clones locaux existants chez vos collaborateurs. Prévenez systématiquement toute l'équipe avant de forcer le push, et demandez-leur de recloner le dépôt plutôt que de tenter un pull sur leur copie locale devenue incompatible.

## Étape 12 : Centraliser les rapports SARIF dans GitHub Security

Pour les organisations gérant des dizaines de dépôts, consulter chaque rapport individuellement n'est pas tenable. Le format SARIF (Static Analysis Results Interchange Format), standard ouvert utilisé par la plupart des scanners de sécurité statique, permet de centraliser les résultats de GitLeaks directement dans l'onglet Security de GitHub, aux côtés des alertes Dependabot et CodeQL.

```
- name: Générer le rapport SARIF
  run: gitleaks detect --source . --report-format sarif --report-path results.sarif --exit-code 0
- name: Publier dans GitHub Security
  uses: github/codeql-action/upload-sarif@v3
  with:
    sarif_file: results.sarif
```
Notez l'usage de --exit-code 0 dans cette étape spécifique : on souhaite que le job publie le rapport même en présence de secrets, plutôt que d'échouer immédiatement et de sauter l'étape de publication. Le blocage effectif de la pull request est alors géré séparément, en amont, via le job de scan classique décrit à l'étape 7.

## Étape 13 : Documenter la conformité NIS2 et Cyber Resilience Act

Pour les entités concernées par la directive NIS2 ou le Cyber Resilience Act, la mise en place technique de GitLeaks ne suffit pas : il faut également produire une trace documentaire démontrable lors d'un audit. Consignez systématiquement la politique de scan de secrets (fréquence, périmètre, outils), la procédure de remédiation en cas de détection, ainsi que l'historique des incidents traités avec leur délai de résolution. Ces éléments alimentent directement le dossier de conformité NIS2 relatif à la gestion des vulnérabilités de la chaîne d'approvisionnement logicielle, ainsi que la documentation de cycle de développement sécurisé exigée par le Cyber Resilience Act pour tout produit numérique commercialisé dans l'Union européenne.

Un point de vigilance mérite d'être souligné : GitLeaks est un contrôle technique parmi d'autres, pas une solution de conformité à lui seul. Il doit s'inscrire dans une politique plus large de gestion des secrets, incluant idéalement un gestionnaire dédié comme HashiCorp Vault pour éliminer à la racine le besoin de stocker des identifiants en dur dans le code.

## GitLeaks vs TruffleHog vs GitGuardian : lequel choisir

GitLeaks n'est pas le seul outil du marché. TruffleHog, également open source, repose sur une combinaison similaire de règles regex et de détection par entropie, avec un accent mis sur la vérification en direct de la validité de certains types de secrets. GitGuardian, de son côté, est une plateforme SaaS commerciale qui surveille en continu l'intégralité de GitHub public à l'échelle d'Internet, en plus des dépôts privés d'un organisme, et propose un tableau de bord de remédiation centralisé.

| Critère | GitLeaks | TruffleHog | GitGuardian | 
|---|---|---|---|
| Modèle | Open source (MIT) | Open source + offre commerciale | SaaS commercial | 
| Prix | Gratuit | CLI gratuit, plans payants pour la plateforme | Offre gratuite limitée + plans entreprise | 
| Moteur | Regex + entropie, config TOML | Regex + entropie, vérification live | Règles propriétaires + validation | 
| Périmètre | Dépôts Git, fichiers, stdin | Dépôts Git et autres sources | GitHub public + dépôts privés, CI/CD, conteneurs, IaC | 
| Hébergement | Auto-hébergé, CLI | Auto-hébergé ou hébergé | Plateforme cloud centralisée | 
| Usage typique | Pre-commit et CI/CD par dépôt | Pre-commit et CI/CD par dépôt | Surveillance de flotte à l'échelle de l'organisation | 

Dans la pratique, ces outils ne s'excluent pas : une organisation peut très bien déployer GitLeaks localement dans les hooks pre-commit et les pipelines CI de chaque dépôt, tout en s'abonnant à GitGuardian pour une surveillance transversale et une visibilité centralisée sur des dizaines, voire des centaines de dépôts.

## Types de secrets détectés et niveaux de risque

Toutes les catégories de secrets ne présentent pas le même niveau de risque en cas d'exposition. Le tableau suivant résume les types les plus couramment détectés par GitLeaks et leur impact potentiel.

| Type de secret | Exemple de format | Risque en cas de fuite | 
|---|---|---|
| Clé d'accès AWS | Préfixe AKIA suivi de 16 caractères | Prise de contrôle de compte cloud, facturation frauduleuse | 
| Jeton GitHub personnel | Préfixe ghp_ suivi de 36 caractères | Accès et modification de dépôts privés | 
| Clé API service IA | Variable selon le fournisseur | Usage frauduleux facturé au propriétaire du compte | 
| Chaîne de connexion base de données | postgres://user:motdepasse@hôte | Accès direct et exfiltration de données | 
| Clé privée SSH ou TLS | Bloc -----BEGIN PRIVATE KEY----- | Usurpation d'identité de serveur, interception de trafic | 
| Jeton de webhook | URL avec jeton en paramètre | Injection de messages ou d'événements frauduleux | 

## Projet complet : pipeline de scan de secrets de bout en bout

Pour assembler l'ensemble des étapes précédentes en une chaîne de défense cohérente, voici l'architecture complète recommandée pour un projet réel. La défense en profondeur repose sur trois couches successives, chacune rattrapant ce que la précédente pourrait laisser passer.

- **Couche 1, poste de développement :** hook pre-commit avec GitLeaks, bloque un secret avant même qu'il ne quitte la machine du développeur
- **Couche 2, pull request :** job GitHub Actions ou GitLab CI, scanne chaque nouvelle contribution avant fusion, avec publication SARIF vers le tableau de sécurité
- **Couche 3, audit périodique :** scan complet de l'historique programmé chaque semaine via une tâche planifiée (cron), pour détecter tout contournement des deux premières couches

```
# Job planifié GitHub Actions pour l'audit hebdomadaire complet
name: Audit hebdomadaire des secrets
on:
  schedule:
    - cron: '0 3 * * 1'
  workflow_dispatch:
jobs:
  audit-complet:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - name: Scan complet de l'historique
        run: |
          docker run -v $(pwd):/repo zricethezav/gitleaks:v8.30.1 \
            detect --source /repo --log-opts="--all" \
            --report-format json --report-path /repo/audit-secrets.json \
            --exit-code 0
      - name: Archiver le rapport
        uses: actions/upload-artifact@v4
        with:
          name: rapport-audit-secrets
          path: audit-secrets.json
          retention-days: 90
```
Ce pipeline complet, combiné à une politique de rotation systématique des identifiants tous les 90 jours et à la migration progressive vers un gestionnaire de secrets centralisé, constitue une base solide et démontrable pour toute équipe cherchant à réduire durablement son exposition, tout en produisant la documentation attendue dans un dossier de conformité NIS2 ou Cyber Resilience Act.

## Les erreurs les plus fréquentes avec GitLeaks

Plusieurs pièges reviennent systématiquement lors du déploiement de GitLeaks en environnement réel. Les connaître à l'avance permet d'éviter des semaines de frustration.

- **Oublier fetch-depth: 0 dans GitHub Actions :** sans cette option, seul le dernier commit est analysé, laissant passer un secret introduit plus tôt dans la même branche
- **Confondre gitleaks detect et gitleaks protect :** detect scanne l'historique complet du dépôt, protect ne scanne que les changements indexés (staged), un usage inversé produit des résultats incohérents en hook pre-commit
- **Sur-utiliser l'allowlist pour faire taire les alertes :** exclure un chemin entier pour éviter un faux positif ponctuel peut masquer un vrai secret futur dans ce même chemin
- **Croire que la suppression du fichier suffit :** un secret retiré du code reste visible dans l'historique Git tant qu'il n'est pas explicitement purgé avec git-filter-repo ou BFG
- **Négliger la synchronisation d'équipe après une réécriture d'historique :** un force-push après purge casse tous les clones existants, un collaborateur qui fait un simple pull recrée le problème
- **Ne scanner que les nouvelles pull requests :** sans audit initial complet de l'historique, des secrets anciens restent invisibles indéfiniment
- **Ignorer le contexte des secrets liés à l'IA :** les clés d'API de services d'intelligence artificielle sont désormais l'une des catégories qui croissent le plus vite et méritent des règles dédiées

## Dépannage : problèmes courants et leurs solutions

Voici les difficultés les plus fréquemment rencontrées lors de l'exploitation quotidienne de GitLeaks, avec la solution correspondante.

- **GitLeaks ne détecte rien alors qu'un secret est visible dans le code :** vérifiez que le fichier n'est pas exclu par une allowlist héritée, et que la commande utilise bien --source . et non un chemin relatif incorrect
- **Le hook pre-commit ne se déclenche jamais :** confirmez que pre-commit install a bien été exécuté dans le dépôt local, la commande doit créer un fichier exécutable dans .git/hooks/pre-commit
- **Le job GitHub Actions passe au vert malgré un secret présent :** vérifiez le code de sortie utilisé, --exit-code 0 masque volontairement l'échec, remplacez-le par --exit-code 1 pour le job de blocage
- **Trop de faux positifs sur des fichiers de test :** ajoutez le motif de chemin correspondant dans la section allowlist.paths du fichier .gitleaks.toml
- **Le scan est extrêmement lent sur un gros dépôt :** limitez la profondeur avec --log-opts="--since=6.months" pour les scans réguliers, réservez le scan complet à l'audit périodique
- **La réécriture d'historique échoue avec git-filter-repo :** l'outil refuse de s'exécuter sur un clone qui n'est pas fraîchement cloné, relancez depuis un clone propre avec l'option --force si nécessaire
- **Le rapport SARIF n'apparaît pas dans l'onglet Security de GitHub :** vérifiez que le dépôt dispose des permissions security-events: write dans le fichier de workflow
- **Le hook Docker en CI échoue avec une erreur de permissions :** montez le volume avec les bons droits utilisateur ou ajoutez --user $(id -u):$(id -g) à la commande docker run
- **Le même secret réapparaît après une correction :** vérifiez que le développeur n'a pas simplement effectué un git revert, qui réintroduit l'ancien commit contenant le secret

## Conseils avancés pour aller plus loin

Une fois les bases posées, plusieurs optimisations permettent de faire mûrir la démarche. Premièrement, combinez GitLeaks avec un scanner de vulnérabilités comme Trivy pour couvrir à la fois les secrets exposés et les dépendances vulnérables dans une même chaîne DevSecOps. Deuxièmement, pour les organisations gérant plus d'une dizaine de dépôts, envisagez de déployer GitLeaks via un modèle de workflow réutilisable (reusable workflow GitHub Actions ou template GitLab CI), plutôt que de dupliquer le fichier de configuration dans chaque projet, ce qui facilite la mise à jour de version en un seul endroit.

Troisièmement, ajoutez une notification automatique (webhook Slack ou Microsoft Teams) déclenchée dès qu'un secret est détecté en CI, avec un lien direct vers le commit incriminé, pour réduire le délai entre détection et remédiation. Rappelez-vous que 64 % des secrets qui fuitaient en 2022 étaient toujours valides en 2026 selon GitGuardian : ce chiffre traduit un problème de processus de remédiation, pas seulement de détection. Enfin, envisagez à moyen terme la migration vers un gestionnaire de secrets centralisé comme HashiCorp Vault, qui élimine structurellement le besoin de coder des identifiants en dur, rendant le scan GitLeaks une simple couche de vérification plutôt que la seule ligne de défense.

Pour approfondir les bonnes pratiques de gestion des secrets au sens large, la fiche de référence OWASP sur la gestion des secrets constitue une base solide, tout comme les recommandations publiées par l'ANSSI sur la sécurisation du cycle de développement logiciel.

## Combien coûte réellement la mise en place d'un programme de scan de secrets

Un argument revient souvent pour justifier de repousser ce chantier : le manque de budget ou de temps disponible côté équipe sécurité. Dans le cas de GitLeaks, cet argument tient mal, puisque l'outil lui-même est entièrement gratuit, sans limite de dépôts, de développeurs ou de scans mensuels. Le coût réel se situe ailleurs : le temps d'ingénierie nécessaire pour intégrer l'outil dans les pipelines existants, traiter le stock initial de faux positifs, et surtout gérer la charge de travail générée par un audit initial qui révèle souvent des dizaines de secrets historiques à révoquer.

Pour une équipe de taille moyenne gérant une dizaine de dépôts actifs, l'expérience de terrain montre qu'il faut généralement compter une demi-journée pour l'installation et la configuration initiale décrite dans ce tutoriel, puis entre une et trois journées supplémentaires pour trier et traiter les résultats du premier audit complet de l'historique, selon l'ancienneté et l'activité du dépôt. Cet investissement ponctuel se rentabilise rapidement : le coût moyen d'une intrusion liée à un identifiant cloud compromis, incluant la remédiation, la communication et les éventuelles amendes réglementaires, dépasse largement de simples journées d'ingénieur. Les organisations qui préfèrent une solution clé en main plutôt qu'un déploiement interne peuvent se tourner vers l'offre gratuite de GitGuardian pour la surveillance de leurs dépôts publics, ou vers ses plans payants pour une couverture complète incluant les dépôts privés et les conteneurs, en complément et non en remplacement de GitLeaks en local.

## Questions fréquentes

**GitLeaks est-il vraiment gratuit pour un usage commercial ?**

Oui. GitLeaks est distribué sous licence MIT, sans restriction d'usage commercial, sans limite de nombre de dépôts scannés et sans version payante cachée. Le projet vit de contributions open source, à la différence de GitGuardian qui propose une plateforme SaaS payante en complément.

**GitLeaks peut-il scanner des dépôts privés hébergés en interne ?**

Oui, GitLeaks fonctionne en local sur n'importe quel dépôt Git accessible depuis la machine ou le runner CI, qu'il soit hébergé sur GitHub, GitLab, Bitbucket, ou sur un serveur Git auto-hébergé de type Gitea ou Forgejo.

**Faut-il utiliser gitleaks detect ou gitleaks protect ?**

gitleaks detect analyse l'historique complet des commits d'un dépôt, adapté à un audit ou un scan CI sur une pull request. gitleaks protect ne scanne que les modifications indexées (staged) non encore commitées, ce qui en fait le choix approprié pour un hook pre-commit local.

**Un secret détecté par GitLeaks doit-il toujours être révoqué ?**

Par précaution, oui, dès lors que le secret a été poussé vers un dépôt distant, même privé. Un dépôt privé peut être exposé par une mauvaise configuration de permissions, un clone local compromis ou un ancien collaborateur ayant toujours accès. La révocation systématique élimine ce risque résiduel.

**GitLeaks ralentit-il significativement les pipelines CI/CD ?**

Un scan ciblé sur les commits d'une pull request s'exécute généralement en quelques secondes. Seul le scan complet de l'historique sur de très gros dépôts (plusieurs dizaines de milliers de commits) peut prendre plusieurs minutes, raison pour laquelle il est recommandé de réserver ce scan complet à une tâche planifiée hebdomadaire plutôt qu'à chaque pull request.

**GitLeaks suffit-il pour être conforme à NIS2 ou au Cyber Resilience Act ?**

Non. GitLeaks est un contrôle technique qui contribue à démontrer une gestion des vulnérabilités de la chaîne d'approvisionnement logicielle, mais la conformité NIS2 et Cyber Resilience Act exige une politique documentée plus large, incluant la gestion des accès, la gestion des correctifs et une procédure de notification d'incident.

**Comment tester GitLeaks sans risquer de casser un dépôt de production ?**

Clonez le dépôt cible dans un répertoire temporaire et lancez gitleaks detect en lecture seule. Cette commande n'écrit jamais dans le dépôt, contrairement à git-filter-repo ou BFG Repo-Cleaner, qui doivent impérativement être testés sur un clone jetable avant toute application sur le dépôt réel.

**Quelle est la différence entre une règle regex et la détection par entropie dans GitLeaks ?**

Une règle regex cible un format de secret connu et documenté, comme le préfixe caractéristique d'une clé AWS. La détection par entropie, elle, repère des chaînes de caractères statistiquement aléatoires sans connaître leur format à l'avance, ce qui permet de détecter des secrets génériques ou propriétaires qu'aucune règle prédéfinie ne couvre.