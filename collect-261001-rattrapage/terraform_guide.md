# Terraform — Guide complet : Infrastructure as Code en production

> **Public** : Zelef, chef de service systèmes & énergies, sysadmin orienté production.
> **Objectif** : maîtriser Terraform de l'installation au déploiement industriel, avec un focus sur Proxmox VE (provider bpg) et le cloud-init.
> **Versions couvertes** : Terraform ≥ 1.5 (vérifié avec 1.9/1.10), OpenTofu ≥ 1.8.
> **Règle d'or** : *on relit TOUJOURS le plan avant `apply`.*

---

## Sommaire

1. L'Infrastructure as Code : pourquoi Terraform
2. Déclaratif vs impératif
3. Idempotence : le pilier de la fiabilité
4. Installation de Terraform sur Debian/Ubuntu
5. Installation d'OpenTofu (alternative)
6. Configuration du shell et des outils
7. Vérification, versions et gestion multi-versions
8. Structure d'un projet Terraform
9. Le workflow de base : init → plan → apply
10. destroy : détruire proprement
11. La CLI Terraform : commandes essentielles
12. HCL : le langage — blocs, arguments, identifiants
13. HCL : types de valeurs et expressions
14. HCL : les fonctions intégrées les plus utiles
15. Variables : déclaration et typage
16. Variables : validation des entrées
17. Variables : les fichiers .tfvars et la précédence
18. Outputs : exposer des valeurs
19. Locals : factoriser la logique
20. Data sources : lire l'existant
21. count : répétition simple
22. for_each : répétition robuste par clé
23. Blocs dynamic : générer des sous-blocs
24. Conditions et boucles en expressions
25. Fonctions avancées : templatefile, fileset, jsonencode
26. terraform fmt et validate : l'hygiène du code
27. Providers : principe et installation
28. Version pinning : figer les versions
29. Le provider Proxmox bpg : installation et configuration
30. Les ressources : anatomie d'un bloc resource
31. Dépendances implicites et graphe
32. depends_on : dépendances explicites
33. Le bloc lifecycle : meta-arguments
34. prevent_destroy : le garde-fou anti-catastrophe
35. create_before_destroy : zéro interruption
36. ignore_changes : ignorer les dérives voulues
37. replace_triggered_by : forcer le remplacement
38. taint et -replace : marquer une ressource à recréer
39. Refresh et plan -refresh-only
40. Le state : pourquoi il existe
41. State local vs state distant
42. Backend S3 + verrouillage DynamoDB
43. Backend Terraform Cloud / HCP Terraform
44. Sauvegarde et protection du state
45. Manipulation du state : mv, rm, pull, push
46. Workspaces : un state par environnement
47. Séparation des environnements : dev / staging / prod
48. Modules : créer son premier module
49. Modules : utiliser un module (registry, git, local)
50. Bonnes pratiques de conception de modules
51. Import de ressources existantes (import block)
52. Provisioners : pourquoi les éviter
53. Alternatives aux provisioners (cloud-init, Ansible, packer)
54. Sécurité : jamais de secrets en clair
55. Chiffrer les tfvars : SOPS + age
56. Détection de dérive (drift detection)
57. CI/CD : pipeline GitLab de revue et d'apply
58. Revue de plan en équipe
59. Debug : TF_LOG et les traces
60. 10+ erreurs classiques et leurs solutions
61. Mise à jour des providers
62. Mise à jour de Terraform / OpenTofu
63. OpenTofu : différences avec Terraform
64. Migration Terraform → OpenTofu
65. Cas pratique 1 : parc de 10 VM Proxmox depuis du code
66. Cas pratique 2 : reprise après sinistre
67. Cas pratique 3 : réseau complet avec VLAN et cloud-init
68. Cas pratique 4 : cluster à 3 nœuds avec anti-affinité
69. Supervision et alerting autour de Terraform
70. Tester son code : terraform test et check blocks
71. Documenter : terraform-docs
72. Estimer les coûts : Infracost
73. Conventions de nommage et style d'équipe
74. Checklist de mise en production
75. Pense-bête de poche (cheat sheet)
76. Glossaire
77. Quiz : 10 questions (énoncés)
78. Quiz : réponses commentées
79. Pour aller plus loin
80. Annexe : fichiers de référence complets

> ⚠️ **Avertissement de version** : les exemples ont été écrits pour Terraform ≥ 1.5 / OpenTofu ≥ 1.8 et le provider `bpg/proxmox` ≥ 0.66. Vérifiez toujours la documentation du provider pour votre version exacte : les noms d'arguments changent parfois (ex. `proxmox_virtual_environment_vm`).

---

## 1. L'Infrastructure as Code : pourquoi Terraform

L'Infrastructure as Code (IaC) consiste à décrire son infrastructure (VM, réseaux, DNS, pare-feu…) dans des fichiers versionnés, au lieu de cliquer dans des interfaces ou de lancer des scripts ad hoc.

| Avant (manuel) | Après (Terraform) |
|---|---|
| Clics dans l'UI Proxmox, non reproductibles | Code versionné en Git, reproductible à l'identique |
| "Qui a créé cette VM ?" sans réponse | `git blame` répond en 10 secondes |
| Dérive de configuration invisible | `terraform plan` révèle toute dérive |
| Reprise après sinistre = réinstallation manuelle | `terraform apply` reconstruit le parc |

Terraform, créé par HashiCorp, est l'outil IaC déclaratif le plus répandu. Il gère le **cycle de vie complet** : création, mise à jour, suppression. Pour un chef de service systèmes, c'est l'assurance que le parc documenté = le parc réel.

---

## 2. Déclaratif vs impératif

- **Impératif** (Ansible ad hoc, scripts bash) : on décrit *comment* faire, étape par étape. Si une étape échoue à moitié, l'état est inconnu.
- **Déclaratif** (Terraform) : on décrit *l'état final souhaité*. Terraform calcule lui-même le chemin (créer, modifier, supprimer) pour y arriver.

```hcl
# Déclaratif : "je veux une VM nommée web-01 avec 4 vCPU"
resource "proxmox_virtual_environment_vm" "web_01" {
  name      = "web-01"
  node_name = "pve1"
  cpu {
    cores = 4
  }
}
# Terraform décide seul : créer la VM si absente, modifier si différente.
```

Avantage clé : **le code est la documentation vivante** de l'infrastructure.

---

## 3. Idempotence : le pilier de la fiabilité

Une opération idempotente peut être répétée sans effet de bord supplémentaire. `terraform apply` lancé 5 fois de suite ne crée qu'**une** VM.

```
1er apply  → 1 VM créée
2e apply   → "No changes. Your infrastructure matches the configuration."
3e apply   → idem
```

C'est ce qui rend Terraform sûr en production et en CI/CD : relancer un pipeline ne duplique rien. L'idempotence repose sur le **state** (section 40) : Terraform compare l'état désiré (code), l'état réel (fournisseur) et l'état mémorisé (state).

---

## 4. Installation de Terraform sur Debian/Ubuntu

Méthode officielle : dépôt APT HashiCorp (clé GPG vérifiée).

```bash
# 1. Prérequis
sudo apt update && sudo apt install -y gnupg software-properties-common curl

# 2. Clé GPG officielle HashiCorp
wget -O- https://apt.releases.hashicorp.com/gpg \
  | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

# 3. Vérification de l'empreinte (doit afficher 798A EC65...)
gpg --no-default-keyring \
    --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg --fingerprint

# 4. Dépôt stable
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" \
  | sudo tee /etc/apt/sources.list.d/hashicorp.list

# 5. Installation
sudo apt update && sudo apt install -y terraform

# 6. Vérification
terraform version
# Terraform v1.10.x on linux_amd64
```

> 💡 **Alternative sans dépôt** : télécharger le binaire sur `releases.hashicorp.com`, dézipper dans `/usr/local/bin`. Pratique sur serveur sans accès au dépôt.

---

## 5. Installation d'OpenTofu (alternative)

OpenTofu est le fork communautaire open source de Terraform (licence MPL-2.0), né après le changement de licence de Terraform (BSL) en 2023. Compatibilité quasi totale avec le langage HCL.

```bash
# Dépôt officiel OpenTofu (Debian/Ubuntu)
curl -fsSL https://get.opentofu.org/opentofu.gpg \
  | sudo tee /etc/apt/keyrings/opentofu.gpg >/dev/null
curl -fsSL https://packages.opentofu.org/opentofu/tofu/any-any/stable/tofu.list \
  | sudo tee /etc/apt/sources.list.d/opentofu.list >/dev/null
sudo chmod a+r /etc/apt/keyrings/opentofu.gpg /etc/apt/sources.list.d/opentofu.list
sudo apt update && sudo apt install -y tofu

# Vérification
tofu version
# OpenTofu v1.9.x
```

| Critère | Terraform | OpenTofu |
|---|---|---|
| Licence | BSL (source disponible, usage restreint) | MPL-2.0 (vraiment open source) |
| Registre | registry.terraform.io | Compatible registry Terraform + le sien |
| Chiffrement du state | Non natif | Oui (natif, section 42) |
| Gouvernance | HashiCorp | Linux Foundation / communauté |

> 💡 En 2026, les deux coexistent. OpenTofu est le choix prudent pour éviter tout risque de licence. La migration est couverte section 64.

---

## 6. Configuration du shell et des outils

```bash
# Complétion bash (à ajouter dans ~/.bashrc)
complete -C /usr/bin/terraform terraform
# Pour OpenTofu :
complete -C /usr/bin/tofu tofu

# Alias pratiques
alias tf='terraform'
alias tfp='terraform plan'
alias tfa='terraform apply'

# Éditeur : VS Code + extension "HashiCorp Terraform"
# (coloration HCL, fmt à la sauvegarde, validation)
```

Fichiers à ignorer dans Git (`.gitignore`) :

```gitignore
# Terraform
.terraform/
*.tfstate
*.tfstate.*
*.tfvars
!example.tfvars
.terraform.lock.hcl
override.tf
override.tf.json
*_override.tf
*_override.tf.json
crash.log
crash.*.log

# OpenTofu
.tofu/
```

---

## 7. Vérification, versions et gestion multi-versions

```bash
terraform version          # version installée
terraform version -json   # format JSON (utile en CI)

# Figer la version requise dans le code (OBLIGATOIRE en prod) :
```

```hcl
terraform {
  required_version = ">= 1.9.0, < 1.11.0"

  required_providers {
    proxmox = {
      source  = "bpg/proxmox"
      version = "~> 0.66"
    }
  }
}
```

**Gestion multi-versions** : `tfenv` (ou `tenv`, qui gère aussi tofu) :

```bash
# tenv : gère terraform + tofu + terragrunt
curl -sL https://github.com/tofuutils/tenv/releases/latest/download/tenv_Linux_x86_64.tar.gz \
  | tar xz -C /tmp && sudo mv /tmp/tenv /usr/local/bin/
tenv terraform install 1.10.5
tenv terraform use 1.10.5
```

> ⚠️ En équipe, **tout le monde utilise la même version** (via `.terraform-version` ou la CI). Un state écrit par une version majeure différente peut poser problème.

---

## 8. Structure d'un projet Terraform

```
infra-proxmox/
├── README.md                  # but du projet, prérequis
├── versions.tf                # required_version + required_providers
├── providers.tf               # configuration des providers
├── backend.tf                 # configuration du backend (state distant)
├── variables.tf               # déclarations de variables
├── terraform.tfvars.example   # exemple de valeurs (JAMAIS de secrets)
├── locals.tf                  # valeurs locales calculées
├── main.tf                    # ressources principales
├── outputs.tf                 # sorties
├── modules/
│   └── vm/                    # module local "vm"
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
└── envs/
    ├── dev.tfvars
    ├── staging.tfvars
    └── prod.tfvars
```

Règles :
- **Un dossier = une stack** (un state). Ne jamais mélanger prod et dev dans le même dossier.
- Noms de fichiers en minuscules, suffixe `.tf`.
- `main.tf` ne doit pas faire 2000 lignes : découper par thème (`network.tf`, `vms.tf`, `storage.tf`).

---

## 9. Le workflow de base : init → plan → apply

```bash
cd infra-proxmox/

# 1. Initialisation : télécharge providers + modules, configure le backend
terraform init

# 2. Plan : calcule les actions SANS rien modifier (lecture seule)
terraform plan -out=tfplan

# 3. RELIRE LE PLAN (toujours !) puis appliquer exactement ce plan
terraform apply tfplan
```

Le plan affiche :

```
Terraform will perform the following actions:

  # proxmox_virtual_environment_vm.web_01 will be created
  + resource "proxmox_virtual_environment_vm" "web_01" {
      + name      = "web-01"
      + node_name = "pve1"
      ...
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```

Symboles : `+` créer, `~` modifier, `-` détruire, `-/+` remplacer (détruire puis recréer), `<=` lire (data source).

> 🚨 **Règle d'or** : `terraform apply` sans `-out`/`tfplan` recalcule un plan à la volée — acceptable en dev, **interdit en prod**. En prod : `plan -out=tfplan` → relecture humaine → `apply tfplan`.

---

## 10. destroy : détruire proprement

```bash
# Détruire TOUTE la stack (demande confirmation)
terraform destroy

# Détruire avec plan préalable (recommandé en prod)
terraform plan -destroy -out=tfplan-destroy
terraform apply tfplan-destroy

# Détruire une seule ressource (ciblée)
terraform destroy -target=proxmox_virtual_environment_vm.web_01
```

> ⚠️ `-target` est un outil de dépannage, pas un mode de fonctionnement normal : il crée des dépendances partielles et peut laisser le state incohérent. À utiliser avec parcimonie.

**Protection** : combinez `prevent_destroy` (section 34) sur les ressources critiques pour rendre `destroy` impossible par accident.

---

## 11. La CLI Terraform : commandes essentielles

| Commande | Usage |
|---|---|
| `terraform init` | Initialise le dossier (providers, backend, modules) |
| `terraform plan` | Affiche les changements prévus |
| `terraform apply` | Applique les changements |
| `terraform destroy` | Détruit l'infrastructure gérée |
| `terraform fmt` | Reformate le code HCL |
| `terraform validate` | Vérifie la syntaxe/cohérence (sans provider) |
| `terraform show` | Affiche le state ou un plan |
| `terraform output` | Affiche les outputs |
| `terraform state list` | Liste les ressources du state |
| `terraform import` | Importe une ressource existante |
| `terraform taint` | Marque une ressource à recréer |
| `terraform workspace` | Gère les workspaces |
| `terraform providers` | Liste les providers requis |
| `terraform graph` | Génère le graphe de dépendances (DOT) |
| `terraform console` | REPL pour tester des expressions |
| `terraform force-unlock` | Libère un verrou bloqué (avec l'ID !) |

```bash
# Exemples utiles au quotidien
terraform output -json | jq .                    # outputs en JSON
terraform state list | grep proxmox_virtual_environment_vm
terraform console <<< 'format("%s-%02d", "web", 1)'   # tester une expression
```

---

## 12. HCL : le langage — blocs, arguments, identifiants

HCL (HashiCorp Configuration Language) : syntaxe `bloc "type" "nom" { argument = valeur }`.

```hcl
# Bloc "resource" : type = proxmox_virtual_environment_vm, nom local = web_01
resource "proxmox_virtual_environment_vm" "web_01" {
  name      = "web-01"   # argument : chaîne
  node_name = "pve1"     # argument : chaîne
  cpu {                  # sous-bloc imbriqué
    cores = 4            # argument : nombre
  }
  tags = ["web", "prod"] # argument : liste
}
```

- Le **nom local** (`web_01`) n'existe que dans le code : c'est l'identifiant pour les références (`proxmox_virtual_environment_vm.web_01.name`).
- Le **nom réel** (`web-01`) est l'argument `name` vu par Proxmox.
- Commentaires : `#` ou `//` (ligne), `/* ... */` (bloc).
- Indentation : 2 espaces (`terraform fmt` l'impose).

## 13. HCL : types de valeurs et expressions

| Type | Exemple |
|---|---|
| `string` | `"web-01"` |
| `number` | `4`, `3.14` |
| `bool` | `true`, `false` |
| `list(string)` | `["web", "prod"]` |
| `map(string)` | `{ env = "prod", role = "web" }` |
| `object({...})` | `{ name = string, cores = number }` |
| `tuple` | `["web", 4, true]` (types mixtes) |
| `set(string)` | `toset(["a", "b"])` (sans doublons, sans ordre) |
| `null` | valeur absente (laisse le défaut du provider) |

```hcl
# Interpolation et expressions
name = "${var.prefix}-web-01"          # interpolation (souvent implicite)
name = "${var.prefix}-web-01"          # équivalent moderne :
name = "${var.prefix}-web-01"

# Opérateurs
cores    = var.ha_enabled ? 8 : 4      # ternaire
disk_gb  = var.base_disk + 20          # arithmétique
is_prod  = var.env == "prod"           # comparaison
combined = concat(["a"], ["b"])        # fonctions
```

> 💡 Depuis Terraform 0.12, l'interpolation `"${...}"` autour d'une expression seule est inutile : `name = var.prefix` suffit. On garde `"${}"` uniquement pour assembler des chaînes.

---

## 14. HCL : les fonctions intégrées les plus utiles

```hcl
# Chaînes
upper("prod")                    # "PROD"
lower("Web-01")                  # "web-01"
format("web-%02d", 1)            # "web-01"
formatlist("%s-prod", ["a","b"]) # ["a-prod", "b-prod"]
join(",", ["a", "b"])            # "a,b"
split(",", "a,b")                # ["a", "b"]
replace("web-01", "-", "_")      # "web_01"
trimspace("  x  ")               # "x"

# Listes / maps
length(["a", "b"])               # 2
element(["a", "b"], 0)           # "a"
concat(["a"], ["b"])             # ["a", "b"]
distinct(["a", "a", "b"])        # ["a", "b"]
flatten([["a"], ["b", "c"]])     # ["a", "b", "c"]
toset(["a", "b"])                # set
keys({a = 1, b = 2})             # ["a", "b"]
values({a = 1, b = 2})           # [1, 2]
merge({a = 1}, {b = 2})          # {a = 1, b = 2}
lookup({a = 1}, "b", 0)          # 0 (défaut)

# Nombres / logique
max(1, 5, 3)                     # 5
min(1, 5, 3)                     # 1
ceil(4.2)                        # 5
floor(4.8)                       # 4
coalesce("", "defaut")           # "defaut" (1er non-vide/non-null)
coalescelist([], ["x"])          # ["x"]

# Fichiers / encodage
file("${path.module}/init.sh")   # contenu d'un fichier
templatefile("${path.module}/cloud-init.tftpl", { hostname = "web-01" })
jsonencode({a = 1})              # "{\"a\":1}"
jsondecode("{\"a\":1}")          # {a = 1}
base64encode("secret")           # "c2VjcmV0"
yamlencode({a = 1})              # "a: 1\n"

# Réseau / IP
cidrhost("10.0.0.0/24", 10)      # "10.0.0.10"
cidrsubnet("10.0.0.0/16", 8, 1)  # "10.0.1.0/24"

# Tester dans le REPL :
# terraform console <<< 'cidrhost("10.0.0.0/24", 10)'
```

---

## 15. Variables : déclaration et typage

```hcl
# variables.tf
variable "env" {
  description = "Environnement cible (dev, staging, prod)"
  type        = string
  default     = "dev"
}

variable "vm_count" {
  description = "Nombre de VM web"
  type        = number
  default     = 2
}

variable "tags" {
  description = "Tags Proxmox"
  type        = list(string)
  default     = ["terraform"]
}

variable "vm_spec" {
  description = "Gabarit d'une VM"
  type = object({
    cores  = number
    memory = number   # en Mo
    disk   = number   # en Go
  })
  default = {
    cores  = 2
    memory = 4096
    disk   = 40
  }
}

variable "node_affinities" {
  description = "Nœud préféré par rôle"
  type        = map(string)
  default = {
    web = "pve1"
    db  = "pve2"
  }
}
```

Règles :
- `description` **toujours** renseignée (c'est de la documentation).
- `type` **toujours** déclaré : Terraform valide les entrées avant tout appel provider.
- Pas de `default` = variable **obligatoire** (Terraform demandera la valeur en interactif — à bannir en CI : utilisez `-var` ou `.tfvars`).

---

## 16. Variables : validation des entrées

```hcl
variable "env" {
  description = "Environnement cible"
  type        = string
  default     = "dev"

  validation {
    condition     = contains(["dev", "staging", "prod"], var.env)
    error_message = "env doit valoir dev, staging ou prod."
  }
}

variable "vm_count" {
  type    = number
  default = 2

  validation {
    condition     = var.vm_count >= 1 && var.vm_count <= 50
    error_message = "vm_count doit être entre 1 et 50."
  }
}

variable "cidr" {
  type    = string
  default = "10.10.0.0/24"

  validation {
    # Vérifie que c'est un CIDR valide
    condition     = can(cidrhost(var.cidr, 0))
    error_message = "cidr doit être un bloc CIDR valide (ex. 10.10.0.0/24)."
  }
}

variable "hostname" {
  type    = string
  default = "web-01"

  validation {
    condition     = can(regex("^[a-z0-9-]+$", var.hostname))
    error_message = "hostname : minuscules, chiffres et tirets uniquement."
  }
}
```

> 💡 La validation s'exécute **avant** le plan : c'est le premier rempart contre les erreurs de saisie, bien avant tout appel à Proxmox.

---

## 17. Variables : les fichiers .tfvars et la précédence

```hcl
# envs/prod.tfvars
env      = "prod"
vm_count = 10
tags     = ["terraform", "prod"]
vm_spec = {
  cores  = 4
  memory = 8192
  disk   = 80
}
```

```bash
terraform plan -var-file=envs/prod.tfvars
terraform plan -var-file=envs/prod.tfvars -var="vm_count=12"  # surcharge ponctuelle
```

**Ordre de précédence** (du plus faible au plus fort) :

1. `default` dans la déclaration
2. `terraform.tfvars` / `*.auto.tfvars` (chargés automatiquement, ordre alphabétique)
3. `-var-file=...` (ordre de la ligne de commande)
4. `-var 'nom=valeur'`
5. Variable d'environnement `TF_VAR_nom`

```bash
export TF_VAR_env="staging"   # pratique en CI
```

> 🚨 **Sécurité** : les `.tfvars` contenant des secrets ne vont **jamais** dans Git (voir sections 54-55). Versionnez uniquement `terraform.tfvars.example`.

---

## 18. Outputs : exposer des valeurs

```hcl
# outputs.tf
output "vm_ipv4_addresses" {
  description = "Adresses IPv4 des VM web"
  value       = [for vm in proxmox_virtual_environment_vm.web : vm.ipv4_addresses]
}

output "vm_ids" {
  description = "IDs Proxmox des VM"
  value       = { for k, vm in proxmox_virtual_environment_vm.web : k => vm.vm_id }
}

output "db_password" {
  description = "Mot de passe généré pour la BDD"
  value       = random_password.db.result
  sensitive   = true   # masqué dans les logs et la sortie console
}
```

```bash
terraform output                  # affiche les outputs non sensibles
terraform output -json            # format JSON (pour scripts/Ansible)
terraform output -raw vm_ids      # valeur brute d'un output simple
```

> 💡 `sensitive = true` masque la valeur à l'écran, mais elle reste **en clair dans le state** (section 44). Ce n'est pas du chiffrement.

---

## 19. Locals : factoriser la logique

Les `locals` sont des valeurs calculées une fois et réutilisées : nommage, tags communs, listes dérivées.

```hcl
# locals.tf
locals {
  # Convention de nommage centralisée
  name_prefix = "${var.env}-${var.project}"

  # Tags communs à toutes les ressources
  common_tags = concat(var.tags, ["env:${var.env}", "managed-by:terraform"])

  # Liste des VM à créer : dérivée des variables
  web_nodes = [
    for i in range(var.vm_count) : {
      name = format("%s-web-%02d", local.name_prefix, i + 1)
      ip   = cidrhost(var.cidr, 10 + i)
    }
  ]

  # Map indexée par nom (pratique pour for_each)
  web_nodes_map = { for n in local.web_nodes : n.name => n }
}
```

Différences `locals` vs `variable` vs `output` :

| | Source | Modifiable par l'appelant | Visible hors module |
|---|---|---|---|
| `variable` | entrée externe | oui (`-var`, tfvars) | non (entrée) |
| `locals` | calcul interne | non | non |
| `output` | valeur exposée | non | oui |

---

## 20. Data sources : lire l'existant

Les data sources **lisent** des informations sans les gérer : un template existant, un nœud, un datastore.

```hcl
# providers.tf — configuration du provider bpg/proxmox
provider "proxmox" {
  endpoint  = var.proxmox_endpoint   # ex. https://pve1.exemple.lan:8006/
  api_token = var.proxmox_api_token # format "USER@REALM!TOKENID=SECRET" (voir §54)
  insecure  = false                 # true seulement avec CA autosignée interne
}
```

```hcl
# Lire le template Debian 12 existant (ne le crée pas, ne le modifie pas)
data "proxmox_virtual_environment_vms" "templates" {
  # selon la version du provider ; alternative : référencer un template connu
}

# Exemple générique avec un autre provider (concept identique) :
# data "proxmox_virtual_environment_datastores" ...
```

Exemple concret et portable avec le provider bpg :

```hcl
# On suppose un template cloud-init "debian-12-cloudinit" déjà présent (VMID 9000)
locals {
  template_vm_id = 9000
}

resource "proxmox_virtual_environment_vm" "web" {
  name      = "web-01"
  node_name = "pve1"
  vm_id     = 101

  clone {
    vm_id = local.template_vm_id   # clone depuis le template existant
    full  = true
  }
  # ...
}
```

> 💡 Règle : **data source = lecture**, **resource = gestion**. Si Terraform doit pouvoir le détruire, c'est une `resource`.

---

## 21. count : répétition simple

```hcl
resource "proxmox_virtual_environment_vm" "web" {
  count     = var.vm_count          # ex. 3
  name      = format("web-%02d", count.index + 1)  # web-01, web-02, web-03
  node_name = "pve1"
  vm_id     = 101 + count.index     # 101, 102, 103
  # ...
}
```

Références : `proxmox_virtual_environment_vm.web[0]`, `[1]`, `[2]`.

⚠️ **Limite de `count`** : les instances sont indexées par position. Supprimer l'élément du milieu décale tout :

```
web[0], web[1], web[2]  →  on passe count de 3 à 2
Terraform détruit web[2]... mais si on voulait retirer web[1],
c'est web[2] qui est détruit et web[1] est "modifié" en ex-web[2].
```

> 💡 Pour des VM nommées/identifiées durablement, préférez **`for_each`** (section 22).

---

## 22. for_each : répétition robuste par clé

```hcl
locals {
  vms = {
    "web-01" = { node = "pve1", cores = 4, memory = 8192, ip = "10.10.0.11" }
    "web-02" = { node = "pve1", cores = 4, memory = 8192, ip = "10.10.0.12" }
    "db-01"  = { node = "pve2", cores = 8, memory = 16384, ip = "10.10.0.21" }
  }
}

resource "proxmox_virtual_environment_vm" "nodes" {
  for_each  = local.vms
  name      = each.key               # "web-01", "web-02", "db-01"
  node_name = each.value.node
  # ...
  initialization {
    ip_config {
      ipv4 {
        address = "${each.value.ip}/24"
        gateway = "10.10.0.1"
      }
    }
  }
}
```

Références : `proxmox_virtual_environment_vm.nodes["web-01"]`.

| | `count` | `for_each` |
|---|---|---|
| Clé d'instance | index numérique | clé de map / valeur de set |
| Suppression d'un élément | décale les suivants (risque) | supprime uniquement cet élément |
| Idéal pour | N copies strictement identiques | objets nommés, hétérogènes |

> 🚨 On ne mélange pas `count` et `for_each` dans le même bloc (erreur de validation).

---

## 23. Blocs dynamic : générer des sous-blocs

Quand le nombre de sous-blocs varie (disques, interfaces réseau), `dynamic` les génère par boucle.

```hcl
variable "extra_disks" {
  type = list(object({
    size      = number
    datastore = string
  }))
  default = [
    { size = 50, datastore = "ceph-vm" },
    { size = 100, datastore = "ceph-vm" },
  ]
}

resource "proxmox_virtual_environment_vm" "db" {
  name      = "db-01"
  node_name = "pve2"

  disk {
    datastore_id = "local-lvm"
    size         = 40
    interface    = "scsi0"
  }

  # Un bloc disk par élément de var.extra_disks
  dynamic "disk" {
    for_each = var.extra_disks
    content {
      datastore_id = disk.value.datastore
      size         = disk.value.size
      interface    = "scsi${disk.key + 1}"   # scsi1, scsi2
    }
  }
}
```

- `disk.key` : index (liste) ou clé (map) de l'itération.
- `disk.value` : l'élément courant.
- Le nom du bloc `dynamic "disk"` doit correspondre exactement au nom du sous-bloc attendu par le provider.

---

## 24. Conditions et boucles en expressions

```hcl
locals {
  # Condition : choisir le datastore selon l'environnement
  vm_datastore = var.env == "prod" ? "ceph-vm" : "local-lvm"

  # Boucle for sur liste : construire des noms
  vm_names = [for i in range(3) : format("web-%02d", i + 1)]
  # => ["web-01", "web-02", "web-03"]

  # Boucle for sur map : filtrer
  prod_vms = {
    for name, spec in local.vms : name => spec
    if spec.env == "prod"
  }

  # Splat : extraire un attribut de chaque instance
  all_ips = proxmox_virtual_environment_vm.nodes[*].ipv4_addresses
  # Équivalent explicite :
  all_ips2 = [for vm in proxmox_virtual_environment_vm.nodes : vm.ipv4_addresses]
}
```

```hcl
# Ressource conditionnelle : count ternaire (0 = absente)
resource "proxmox_virtual_environment_vm" "bastion" {
  count     = var.env == "prod" ? 1 : 0
  name      = "bastion"
  node_name = "pve1"
  # ...
}
```

> 💡 Astuce : pour une ressource optionnelle avec `for_each`, utilisez `for_each = var.enabled ? toset(["x"]) : toset([])`.

## 25. Fonctions avancées : templatefile, fileset, jsonencode

```hcl
# cloud-init.tftpl (fichier template à côté du .tf)
#cloud-config
hostname: ${hostname}
manage_etc_hosts: true
users:
  - name: ${admin_user}
    sudo: ALL=(ALL) NOPASSWD:ALL
    ssh_authorized_keys:
      - ${ssh_key}
```

```hcl
# Utilisation
resource "proxmox_virtual_environment_vm" "web" {
  # ...
  initialization {
    user_data_file_id = proxmox_virtual_environment_file.cloud_config["web-01"].id
  }
}

# Générer un fichier cloud-init par VM depuis un template
resource "proxmox_virtual_environment_file" "cloud_config" {
  for_each     = local.vms
  content_type = "snippets"
  datastore_id = "local"
  node_name    = each.value.node

  source_raw {
    file_name = "${each.key}-cloud-config.yaml"
    data = templatefile("${path.module}/templates/cloud-init.tftpl", {
      hostname   = each.key
      admin_user = var.admin_user
      ssh_key    = var.ssh_public_key
    })
  }
}
```

```hcl
# fileset : lister des fichiers (ex. générer des snippets en masse)
locals {
  snippet_files = fileset("${path.module}/snippets", "*.yaml")
}

# jsonencode : passer une structure à une API
locals {
  metadata = jsonencode({
    env     = var.env
    owner   = "infra-team"
    created = timestamp()
  })
}
```

> ⚠️ `timestamp()` change à chaque plan → drift permanent. Ne l'utilisez jamais dans une ressource suivie, uniquement dans des `locals` éphémères ou des outputs informatifs.

---

## 26. terraform fmt et validate : l'hygiène du code

```bash
terraform fmt            # reformate tous les .tf du dossier (récursif avec -recursive)
terraform fmt -check     # vérifie sans modifier (idéal en CI : échoue si mal formaté)
terraform fmt -diff      # montre les différences

terraform validate       # vérifie la cohérence interne (types, références)
                         # SANS contacter les providers : rapide, à lancer avant plan
```

Pipeline minimal de qualité (à mettre en CI, section 57) :

```bash
terraform fmt -check -recursive
terraform init -backend=false
terraform validate
```

---

## 27. Providers : principe et installation

Un **provider** est un plugin (binaire) qui traduit le HCL en appels d'API : Proxmox, AWS, DNS, etc. Terraform Core orchestre ; les providers exécutent.

```hcl
terraform {
  required_providers {
    proxmox = {
      source  = "bpg/proxmox"   # <namespace>/<nom> sur le registry
      version = "~> 0.66"
    }
    random = {
      source  = "hashicorp/random"
      version = "~> 3.6"
    }
  }
}
```

```bash
terraform init   # télécharge les providers dans .terraform/
```

- Registre officiel : `registry.terraform.io` (Terraform) — OpenTofu sait aussi l'utiliser.
- Les binaires sont vérifiés par signature (`.terraform.lock.hcl` fige les hashes).
- **Ne jamais** commiter `.terraform/` (binaires lourds) — voir `.gitignore` section 6.

---

## 28. Version pinning : figer les versions

Contraintes de version (opérateurs) :

| Contrainte | Signification |
|---|---|
| `= 0.66.0` | exactement cette version |
| `~> 0.66` | `>= 0.66.0, < 0.67.0` (recommandé : correctifs + mineurs compatibles) |
| `>= 0.66, < 0.70` | intervalle explicite |
| `>= 1.9.0, < 2.0.0` | pour `required_version` de Terraform lui-même |

```hcl
terraform {
  required_version = ">= 1.9.0, < 1.11.0"
  required_providers {
    proxmox = {
      source  = "bpg/proxmox"
      version = "~> 0.66"     # fige la branche 0.66.x
    }
  }
}
```

`.terraform.lock.hcl` (versionné en Git !) fige le **hash exact** du binaire :

```bash
terraform init -upgrade   # autorise la montée de version dans la contrainte
terraform providers lock  # régénère le lock file
```

> 🚨 En prod : contrainte `~>` + lock file versionné + `init` sans `-upgrade` en CI. Une montée de provider ne se fait que volontairement, avec relecture du changelog.

---

## 29. Le provider Proxmox bpg : installation et configuration

Le provider communautaire `bpg/proxmox` est la référence pour Proxmox VE (API native, pas de dépendance SSH).

```hcl
# versions.tf
terraform {
  required_version = ">= 1.9.0, < 1.11.0"
  required_providers {
    proxmox = {
      source  = "bpg/proxmox"
      version = "~> 0.66"
    }
  }
}
```

```hcl
# providers.tf
variable "proxmox_endpoint" {
  description = "URL API Proxmox, ex. https://pve1.lan:8006/"
  type        = string
}
variable "proxmox_api_token" {
  description = "Token API 'USER@REALM!TOKENID=SECRET'"
  type        = string
  sensitive   = true
}

provider "proxmox" {
  endpoint  = var.proxmox_endpoint
  api_token = var.proxmox_api_token
  insecure  = var.proxmox_insecure_tls  # true si CA interne/autosignée
  # ssh { ... }  # optionnel : uniquement pour certaines opérations (ex. upload ISO)
  tmp_dir   = "/var/tmp"
}
```

**Créer le token API côté Proxmox** (privilège minimal) :

```bash
# Sur le nœud Proxmox, en root :
pveum user add terraform@pve --comment "Terraform automation"
pveum usertoken add terraform@pve tf-token --privsep 0
# Noter : terraform@pve!tf-token=<SECRET>

# Rôle minimal (ajuster selon besoins : VM.Allocate, VM.Clone, Datastore.AllocateSpace...)
pveum role add TerraformRole -privs "VM.Allocate VM.Clone VM.Config.CDROM VM.Config.CPU VM.Config.Disk VM.Config.HWType VM.Config.Memory VM.Config.Network VM.Config.Options VM.Audit VM.PowerMgmt Datastore.AllocateSpace Datastore.Audit Sys.Audit"
pveum acl modify / -user terraform@pve -role TerraformRole
```

> 💡 Le token s'injecte via `TF_VAR_proxmox_api_token` ou un `.tfvars` **hors Git** (sections 54-55). Jamais en clair dans le code.

---

## 30. Les ressources : anatomie d'un bloc resource

```hcl
resource "proxmox_virtual_environment_vm" "web_01" {
  # ── Identité ──
  name      = "web-01"
  node_name = "pve1"        # nœud d'hébergement
  vm_id     = 101           # VMID Proxmox (unique par cluster)
  tags      = ["terraform", "prod", "web"]

  # ── Matériel ──
  cpu {
    cores = 4
    type  = "host"          # ou x86-64-v2-AES pour la portabilité
  }
  memory {
    dedicated = 8192        # Mo
  }
  disk {
    datastore_id = "ceph-vm"
    file_format  = "raw"
    interface    = "scsi0"
    size         = 40        # Go
  }
  network_device {
    bridge = "vmbr0"
    vlan_id = 10
  }

  # ── Démarrage ──
  started = true
  on_boot = true

  # ── Cloud-init ──
  initialization {
    datastore_id = "local-lvm"
    ip_config {
      ipv4 {
        address = "10.10.0.11/24"
        gateway = "10.10.0.1"
      }
    }
  }
}
```

Attributs **calculés** (connus après création) : `ipv4_addresses`, `vm_id` (si auto), etc. Ils sont utilisables dans d'autres ressources et outputs — c'est le chaînage.

---

## 31. Dépendances implicites et graphe

Terraform construit un **graphe de dépendances** à partir des références entre ressources : si B utilise un attribut de A, B est créé **après** A, et détruit **avant** A.

```hcl
# Le snippet cloud-init doit exister AVANT la VM qui le référence :
# dépendance implicite via proxmox_virtual_environment_file.cloud_config[...].id
resource "proxmox_virtual_environment_vm" "web" {
  for_each = local.vms
  # ...
  initialization {
    user_data_file_id = proxmox_virtual_environment_file.cloud_config[each.key].id
  }
}
```

Visualiser le graphe :

```bash
terraform graph | dot -Tpng > graph.png   # nécessite graphviz
terraform graph -type=plan | dot -Tsvg > plan.svg
```

> 💡 95 % des dépendances doivent être **implicites** (par référence). C'est plus lisible et moins fragile que `depends_on`.

---

## 32. depends_on : dépendances explicites

À n'utiliser que quand il n'y a **aucune référence d'attribut** mais un ordre requis (ex. attendre qu'un script côté Proxmox ait préparé un stockage).

```hcl
resource "proxmox_virtual_environment_vm" "db" {
  name      = "db-01"
  node_name = "pve2"
  # ...

  # N'a de sens QUE sans référence directe entre les deux ressources
  depends_on = [
    proxmox_virtual_environment_vm.nas,  # le NAS doit exister avant la BDD
  ]
}
```

> ⚠️ `depends_on` crée un ordre strict mais **n'exporte aucune donnée**. Si vous avez besoin d'un attribut de l'autre ressource, utilisez une référence directe (dépendance implicite) plutôt que `depends_on`.

---

## 33. Le bloc lifecycle : meta-arguments

```hcl
resource "proxmox_virtual_environment_vm" "db" {
  # ...
  lifecycle {
    prevent_destroy       = true   # §34
    create_before_destroy = true   # §35
    ignore_changes        = [tags]  # §36
    replace_triggered_by  = [proxmox_virtual_environment_file.cloud_config["db-01"].id]  # §37
  }
}
```

| Meta-argument | Effet |
|---|---|
| `prevent_destroy = true` | `apply`/`destroy` échoue si la ressource doit être détruite |
| `create_before_destroy = true` | crée le remplaçant avant de détruire l'ancien |
| `ignore_changes = [...]` | ignore certaines dérives au plan |
| `replace_triggered_by = [...]` | force le remplacement quand la référence change |

---

## 34. prevent_destroy : le garde-fou anti-catastrophe

```hcl
resource "proxmox_virtual_environment_vm" "db_prod" {
  name      = "db-prod-01"
  node_name = "pve2"
  # ...

  lifecycle {
    prevent_destroy = true
  }
}
```

Si un plan veut détruire cette VM :

```
Error: Instance cannot be destroyed
  on main.tf line 42:
  42: resource "proxmox_virtual_environment_vm" "db_prod" {
Resource ... has lifecycle.prevent_destroy set, but the plan calls for this
resource to be destroyed.
```

**À mettre sur** : BDD de prod, NAS, tout ce qui est difficilement reconstructible.
**Retrait volontaire** : enlever le bloc, `apply`, puis détruire — jamais en urgence, toujours relu.

---

## 35. create_before_destroy : zéro interruption

Par défaut, un remplacement = détruire puis recréer (**coupure**). Avec `create_before_destroy`, Terraform crée d'abord le remplaçant.

```hcl
resource "proxmox_virtual_environment_vm" "web" {
  # ...
  lifecycle {
    create_before_destroy = true
  }
}
```

⚠️ **Contraintes** :
- Le nom/ID doit pouvoir coexister temporairement : avec Proxmox, deux VM ne peuvent pas partager le même `vm_id` ni le même nom → prévoyez un nom avec suffixe ou un `vm_id` dynamique.
- Doublonne temporairement les ressources (capacité à prévoir).

```hcl
# Exemple : nom unique par génération pour permettre la coexistence
resource "proxmox_virtual_environment_vm" "web" {
  name = "web-${random_id.gen.hex}"
  lifecycle { create_before_destroy = true }
}
resource "random_id" "gen" {
  byte_length = 2
  keepers = { config_version = var.config_version }  # change => nouvelle VM
}
```

---

## 36. ignore_changes : ignorer les dérives voulues

Quand un attribut est modifié **hors Terraform** volontairement (ex. tags posés par une autre équipe, nombre de vCPU ajusté à chaud), on évite que chaque plan propose de le réécrire.

```hcl
resource "proxmox_virtual_environment_vm" "web" {
  # ...
  lifecycle {
    ignore_changes = [
      tags,            # les tags gérés hors Terraform sont ignorés
      cpu[0].cores,    # ajustement CPU à chaud ignoré (adressage d'un sous-bloc)
    ]
  }
}
```

> ⚠️ À utiliser avec parcimonie : chaque `ignore_changes` est un angle mort du drift detection (section 56). Documentez **pourquoi** dans un commentaire.

## 37. replace_triggered_by : forcer le remplacement

Quand une ressource dépend d'un fichier externe (cloud-init, script) que Terraform ne "voit" pas comme un changement de la ressource elle-même :

```hcl
resource "proxmox_virtual_environment_file" "cloud_config" {
  for_each     = local.vms
  content_type = "snippets"
  datastore_id = "local"
  node_name    = each.value.node
  source_raw {
    file_name = "${each.key}-cloud-config.yaml"
    data      = templatefile("${path.module}/templates/cloud-init.tftpl", {
      hostname = each.key
      # ...
    })
  }
}

resource "proxmox_virtual_environment_vm" "nodes" {
  for_each = local.vms
  # ...
  lifecycle {
    # Si le cloud-init change, recréer la VM (le provider bpg ne le fait pas seul)
    replace_triggered_by = [
      proxmox_virtual_environment_file.cloud_config[each.key].id
    ]
  }
}
```

> 💡 Alternative moins brutale : versionner le nom du fichier (`cloud-config-v2.yaml`) pour tracer les générations.

---

## 38. taint et -replace : marquer une ressource à recréer

Une VM est corrompue (disque endommagé, OS vérolé) mais sa config Terraform est bonne : on la marque à recréer.

```bash
# Ancienne méthode (deprecated mais encore vue) :
terraform taint 'proxmox_virtual_environment_vm.nodes["web-01"]'

# Méthode moderne (planifie explicitement le remplacement) :
terraform plan -replace='proxmox_virtual_environment_vm.nodes["web-01"]' -out=tfplan
terraform apply tfplan

# Annuler un taint :
terraform untaint 'proxmox_virtual_environment_vm.nodes["web-01"]'
```

Le plan affiche alors `-/+` (remplacement) pour cette seule ressource.

---

## 39. Refresh et plan -refresh-only

```bash
# Synchronise le state avec la réalité (sans modifier l'infra)
terraform refresh

# Aperçu des dérives SANS proposer de corrections (lecture seule)
terraform plan -refresh-only -out=refresh.plan
```

Cas d'usage : quelqu'un a modifié une VM dans l'UI Proxmox → `plan -refresh-only` montre ce qui a dérivé, sans proposer de tout écraser. C'est la base du **drift detection** (section 56).

> ⚠️ `terraform refresh` seul modifie le state : préférez `plan -refresh-only` + `apply` du refresh plan pour garder une trace.

---

## 40. Le state : pourquoi il existe

Le **state** (`terraform.tfstate`, JSON) est la mémoire de Terraform : il mappe chaque `resource` du code vers l'objet réel (VMID Proxmox, adresse IP…).

```json
{
  "version": 4,
  "resources": [
    {
      "type": "proxmox_virtual_environment_vm",
      "name": "web_01",
      "instances": [{ "attributes": { "vm_id": 101, "name": "web-01" } }]
    }
  ]
}
```

Sans state, Terraform ne saurait pas que `web_01` = VMID 101 : il voudrait tout recréer. Le state contient aussi les **outputs** et les **dépendances**.

> 🚨 Le state peut contenir des **secrets** (mots de passe en clair dans les attributs). Il doit être **chiffré au repos, accès restreint, jamais dans Git** (sauf repo privé avec git-crypt — déconseillé).

---

## 41. State local vs state distant

| | Local (`terraform.tfstate`) | Distant (S3, Terraform Cloud…) |
|---|---|---|
| Stockage | disque du poste | service partagé |
| Travail en équipe | ❌ (conflits, écrasements) | ✅ |
| Verrouillage | ❌ (risque de corruption si 2 apply) | ✅ (state locking) |
| Secrets | en clair sur le poste | chiffrables au repos |
| Sauvegarde | manuelle | versioning natif |

**Règle** : state local = POC solo sur son poste. Dès qu'on est 2 ou qu'on automatise → **backend distant obligatoire**.

---

## 42. Backend S3 + verrouillage DynamoDB

Le classique : state dans un bucket S3 (versionné, chiffré), verrou dans DynamoDB.

```hcl
# backend.tf
terraform {
  backend "s3" {
    bucket         = "infra-terraform-state-prod"
    key            = "proxmox/prod/terraform.tfstate"
    region         = "eu-west-3"
    encrypt        = true
    dynamodb_table = "terraform-state-locks"
    # use_lockfile = true   # alternative moderne (S3 natif, sans DynamoDB)
  }
}
```

```bash
# Création du bucket + table (une fois, à la main ou via une stack bootstrap)
aws s3api create-bucket --bucket infra-terraform-state-prod --region eu-west-3 \
  --create-bucket-configuration LocationConstraint=eu-west-3
aws s3api put-bucket-versioning --bucket infra-terraform-state-prod \
  --versioning-configuration Status=Enabled
aws s3api put-bucket-encryption --bucket infra-terraform-state-prod \
  --server-side-encryption-configuration \
  '{"Rules":[{"ApplyServerSideEncryptionByDefault":{"SSEAlgorithm":"AES256"}}]}'
aws dynamodb create-table --table-name terraform-state-locks \
  --attribute-definitions AttributeName=LockID,AttributeType=S \
  --key-schema AttributeName=LockID,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST --region eu-west-3
```

**Alternatives on-prem** (sans AWS) :
- **Backend HTTP** maison (simple serveur avec verrou) ;
- **Terraform Cloud / HCP Terraform** (section 43) ;
- **OpenTofu** : chiffrement natif du state (`tofu` + `key_provider`, section 63) ;
- MinIO (compatible S3) auto-hébergé : même config, `endpoints = { s3 = "https://minio.lan" }`.

> ⚠️ Changer de backend = `terraform init -migrate-state` (Terraform propose de copier l'ancien state). Toujours sauvegarder avant.

---

## 43. Backend Terraform Cloud / HCP Terraform

```hcl
terraform {
  cloud {
    organization = "mon-entreprise"
    workspaces {
      name = "proxmox-prod"
    }
  }
}
```

Avantages : state hébergé + verrouillé + versionné, historique des runs, revue de plan dans l'UI, variables sensibles chiffrées, intégration VCS (plan auto sur PR).
Inconvénients : dépendance à un SaaS externe, offre gratuite limitée.

```bash
terraform login          # authentification (token stocké en clair dans ~/.terraform.d/credentials.tfrc.json !)
terraform init           # migre le state local vers le cloud
```

> 💡 Pour un usage 100 % on-prem, préférez S3/MinIO + DynamoDB ou le backend HTTP.

---

## 44. Sauvegarde et protection du state

Checklist de protection du state distant :

- [ ] **Versioning** activé sur le bucket (restauration d'une version antérieure en 1 clic)
- [ ] **Chiffrement au repos** (SSE-S3 ou SSE-KMS)
- [ ] **Accès restreint** : IAM limité à l'équipe infra + rôle CI (pas de `s3:*` large)
- [ ] **MFA Delete** sur le bucket (optionnel mais fort)
- [ ] **Sauvegarde externe** : `terraform state pull > backup-$(date +%F).tfstate` chiffré (gpg) vers un stockage séparé
- [ ] **Jamais dans Git** en clair

```bash
# Sauvegarde manuelle chiffrée
terraform state pull | gpg --encrypt --recipient infra@entreprise.lan \
  > /srv/backups/terraform/prod-$(date +%F).tfstate.gpg

# Restauration d'urgence (ATTENTION : écrase le state distant)
gpg --decrypt prod-2026-09-26.tfstate.gpg | terraform state push -
```

> 🚨 `state push` écrase sans verrou collaboratif : à n'utiliser qu'en dernier recours, équipe prévenue.

---

## 45. Manipulation du state : mv, rm, pull, push

```bash
# Lister
terraform state list
terraform state show 'proxmox_virtual_environment_vm.nodes["web-01"]'

# Renommer une ressource dans le code SANS la recréer
# (ex. web_01 -> web dans le .tf) :
terraform state mv 'proxmox_virtual_environment_vm.web_01' \
                   'proxmox_virtual_environment_vm.nodes["web-01"]'

# Retirer du state SANS détruire l'objet réel (Terraform l'oublie)
terraform state rm 'proxmox_virtual_environment_vm.old'

# Réimporter ensuite si besoin
terraform import 'proxmox_virtual_environment_vm.old' pve1:qemu/109
```

> 💡 La méthode moderne pour les renommages : le bloc **`moved`** dans le code (traçable en Git) :
>
> ```hcl
> moved {
>   from = proxmox_virtual_environment_vm.web_01
>   to   = proxmox_virtual_environment_vm.nodes["web-01"]
> }
> ```

---

## 46. Workspaces : un state par environnement

```bash
terraform workspace new dev
terraform workspace new prod
terraform workspace list
terraform workspace select prod
terraform plan -var-file=envs/prod.tfvars
```

- Chaque workspace = un **state séparé** dans le même backend (`key` suffixé par le workspace).
- `${terraform.workspace}` utilisable dans le code :

```hcl
locals {
  name_prefix = "${terraform.workspace}-${var.project}"
}
```

⚠️ **Limites** : les workspaces partagent le même code et les mêmes variables par défaut — faciles à confondre (`apply` sur le mauvais workspace = catastrophe). Pour des environnements bien séparés, préférez des **dossiers distincts** (section 47). Workspaces = pratique pour dev/test éphémères, pas pour prod sérieuse.

---

## 47. Séparation des environnements : dev / staging / prod

Architecture recommandée :

```
infra/
├── modules/vm/                 # module partagé
├── envs/
│   ├── dev/
│   │   ├── main.tf             # appelle le module, petites VM
│   │   ├── backend.tf          # key = "proxmox/dev/terraform.tfstate"
│   │   └── terraform.tfvars
│   ├── staging/
│   │   ├── main.tf
│   │   ├── backend.tf          # key = "proxmox/staging/terraform.tfstate"
│   │   └── terraform.tfvars
│   └── prod/
│       ├── main.tf
│       ├── backend.tf          # key = "proxmox/prod/terraform.tfstate"
│       └── terraform.tfvars
```

| Principe | Mise en œuvre |
|---|---|
| 1 dossier = 1 state = 1 env | `envs/prod` ne touche jamais le state de dev |
| Code partagé | modules locaux ou registry |
| Différences | uniquement via `.tfvars` et variables |
| Promotion | `dev` → `staging` → `prod`, même code, plan relu à chaque étape |
| Accès | token Proxmox et droits S3 distincts par env |

```hcl
# envs/prod/main.tf : exemple minimal
module "web_farm" {
  source   = "../../modules/vm"
  env      = "prod"
  vm_count = 10
  # ...
}
```

---

## 48. Modules : créer son premier module

Un module = un dossier avec des `.tf`, une interface (`variables.tf` / `outputs.tf`).

```
modules/vm/
├── main.tf        # ressources
├── variables.tf   # interface d'entrée
├── outputs.tf     # interface de sortie
└── README.md      # documentation (but, exemple)
```

```hcl
# modules/vm/variables.tf
variable "name"      { description = "Nom de la VM"; type = string }
variable "node_name" { description = "Nœud Proxmox"; type = string }
variable "vm_id"     { description = "VMID"; type = number }
variable "cores"     { description = "vCPU"; type = number; default = 2 }
variable "memory"    { description = "RAM en Mo"; type = number; default = 4096 }
variable "ip_address" { description = "IP avec CIDR, ex. 10.10.0.11/24"; type = string }
variable "gateway"   { description = "Passerelle"; type = string; default = "10.10.0.1" }

# modules/vm/main.tf
resource "proxmox_virtual_environment_vm" "this" {
  name      = var.name
  node_name = var.node_name
  vm_id     = var.vm_id
  tags      = ["terraform", "module-vm"]

  cpu { cores = var.cores }
  memory { dedicated = var.memory }
  disk {
    datastore_id = "local-lvm"
    interface    = "scsi0"
    size         = 40
  }
  network_device { bridge = "vmbr0" }
  initialization {
    datastore_id = "local-lvm"
    ip_config {
      ipv4 { address = var.ip_address; gateway = var.gateway }
    }
  }
}

# modules/vm/outputs.tf
output "vm_id" { description = "VMID"; value = proxmox_virtual_environment_vm.this.vm_id }
output "name"  { description = "Nom";  value = proxmox_virtual_environment_vm.this.name }
```

## 49. Modules : utiliser un module (registry, git, local)

```hcl
# Module local
module "web_01" {
  source     = "../../modules/vm"
  name       = "web-01"
  node_name  = "pve1"
  vm_id      = 101
  cores      = 4
  memory     = 8192
  ip_address = "10.10.0.11/24"
}

# Module depuis le registry public
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "~> 5.0"
  # ...
}

# Module depuis Git (tag figé !)
module "vm" {
  source = "git::https://git.entreprise.lan/infra/tf-modules.git//vm?ref=v1.2.0"
  # ...
}

# Accès aux outputs d'un module
output "web_01_id" {
  value = module.web_01.vm_id
}
```

```bash
terraform init -upgrade   # met à jour les modules vers la contrainte
terraform get             # télécharge les modules sans init complet
```

> 🚨 Toujours figer `version` (registry) ou `?ref=` (git). Un module flottant sur `main` = surprise garantie un lundi matin.

---

## 50. Bonnes pratiques de conception de modules

- [ ] **Une responsabilité** par module (ex. `vm`, pas `infra-complete`).
- [ ] Interface minimale : peu de variables obligatoires, des `default` sensés.
- [ ] `description` sur chaque variable et output.
- [ ] Pas de provider configuré dans un module partagé (le provider est configuré à la racine ; le module hérite).
- [ ] Pas de backend dans un module.
- [ ] Versionner (tag Git `v1.0.0`) + CHANGELOG.
- [ ] `README.md` avec exemple d'appel copiable.
- [ ] Tester avec `terraform plan` sur un exemple (`examples/`).

```
modules/vm/
├── main.tf
├── variables.tf
├── outputs.tf
├── README.md
├── CHANGELOG.md
└── examples/
    └── basic/
        └── main.tf     # exemple minimal, testé en CI
```

---

## 51. Import de ressources existantes (import block)

Une VM créée à la main dans Proxmox peut entrer sous gestion Terraform **sans être recréée**.

```hcl
# 1. Déclarer la ressource cible (config aussi proche que possible du réel)
resource "proxmox_virtual_environment_vm" "legacy_db" {
  name      = "db-legacy"
  node_name = "pve2"
  vm_id     = 200
  # ... (le reste sera ajusté après le premier plan)
}

# 2. Bloc d'import (Terraform ≥ 1.5) : traçable en Git, réapplicable
import {
  to = proxmox_virtual_environment_vm.legacy_db
  id = "pve2:qemu/200"   # format d'ID selon le provider (voir sa doc)
}
```

```bash
terraform plan -out=tfplan   # montre ce que Terraform voudrait CHANGER après import
# Ajuster le .tf jusqu'à obtenir "No changes" (ou des changements acceptés)
terraform apply tfplan
```

> 💡 Méthode : importez, lancez `plan`, ajustez la config pour coller au réel, recommencez jusqu'à zéro diff non voulu. Ne jamais importer en prod sans relecture du plan.

---

## 52. Provisioners : pourquoi les éviter

```hcl
# ❌ À ÉVITER : fragile, non idempotent, pas de gestion d'erreur fine
resource "proxmox_virtual_environment_vm" "web" {
  # ...
  provisioner "remote-exec" {
    inline = ["apt update", "apt install -y nginx"]
    connection {
      type        = "ssh"
      user        = "admin"
      private_key = file("~/.ssh/id_ed25519")
      host        = self.ipv4_addresses[0]
    }
  }
}
```

Problèmes :
- S'exécute **après** création, hors du modèle déclaratif (pas de drift detection dessus).
- Échec réseau/SSH = ressource "créée mais pas configurée" (état bâtard, `taint` manuel).
- Secrets de connexion dans le code/state.
- `local-exec` dépend de la machine qui lance Terraform (non portable en CI).

> 🚨 Règle d'équipe : **aucun provisioner dans le code partagé**. Ils restent un outil de dernier recours, documenté comme tel.

---

## 53. Alternatives aux provisioners (cloud-init, Ansible, Packer)

| Besoin | Solution propre |
|---|---|
| Config initiale (user, SSH, réseau, paquets) | **cloud-init** via `user_data_file_id` (natif Proxmox) |
| Configuration applicative | **Ansible** après `terraform apply` (inventaire généré depuis `terraform output -json`) |
| Image durcie/répétable | **Packer** pour builder le template, Terraform pour cloner |
| Script one-shot fiable | `null_resource` + `local-exec` avec `triggers` (explicite, versionné) |

```bash
# Chaînage Terraform -> Ansible (classique et robuste)
terraform apply -auto-approve tfplan
terraform output -json > /tmp/tf-outputs.json
ansible-playbook -i inventory_terraform.py site.yml
```

```hcl
# Exemple cloud-init complet (templates/cloud-init.tftpl)
#cloud-config
hostname: ${hostname}
manage_etc_hosts: true
package_update: true
packages: [qemu-guest-agent, curl, vim]
users:
  - name: ${admin_user}
    sudo: ALL=(ALL) NOPASSWD:ALL
    shell: /bin/bash
    ssh_authorized_keys: [${ssh_key}]
runcmd:
  - systemctl enable --now qemu-guest-agent
```

---

## 54. Sécurité : jamais de secrets en clair

Règles non négociables :

1. **Aucun secret dans les `.tf`** (ni token, ni mot de passe, ni clé privée).
2. **Aucun secret dans Git** — même en repo privé. L'historique Git n'oublie jamais.
3. Variables sensibles : `sensitive = true` + injection via environnement ou fichier chiffré.
4. Le state contient des secrets en clair → backend chiffré + accès restreint (section 44).

```hcl
variable "proxmox_api_token" {
  type      = string
  sensitive = true
  # PAS de default avec un vrai secret !
}
```

```bash
# Injection par environnement (CI ou poste admin)
export TF_VAR_proxmox_api_token="terraform@pve!tf-token=xxxxxxxx"
terraform plan
```

```bash
# Vérifier qu'aucun secret ne traîne (à mettre en pre-commit)
git secrets --scan
# ou : truffleHog / gitleaks
gitleaks detect --source . --verbose
```

> 💡 Si un secret a fuité dans Git : le **révoquer immédiatement** (rotation), puis nettoyer l'historique (`git filter-repo`). Le nettoyage seul ne suffit jamais.

---

## 55. Chiffrer les tfvars : SOPS + age

[SOPS](https://github.com/getsops/sops) chiffre les fichiers de variables avec `age` (simple, sans serveur).

```bash
# Installation
sudo apt install -y age
# sops : binaire depuis les releases GitHub
curl -sL https://github.com/getsops/sops/releases/latest/download/sops-v3.x.x.linux.amd64 \
  -o /tmp/sops && sudo install /tmp/sops /usr/local/bin/sops

# Générer une clé age (1 par admin, publique versionnée, privée gardée)
age-keygen -o ~/.config/sops/age/keys.txt
# Public key: age1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

```yaml
# .sops.yaml (à la racine, versionné)
creation_rules:
  - path_regex: envs/prod/.*\.tfvars$
    age: age1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

```bash
# Chiffrer le tfvars de prod
sops --encrypt --in-place envs/prod/secrets.tfvars

# Utilisation : déchiffrer à la volée (jamais de fichier clair sur disque)
sops --decrypt envs/prod/secrets.tfvars > /tmp/secrets.tfvars
terraform plan -var-file=envs/prod.tfvars -var-file=/tmp/secrets.tfvars
shred -u /tmp/secrets.tfvars
```

> 💡 Alternative : **Vault** (HashiCorp) via le provider `vault` + data sources, ou variables d'environnement injectées par le gestionnaire de secrets de la CI.

---

## 56. Détection de dérive (drift detection)

La dérive = la réalité s'écarte du code (modif manuelle dans l'UI Proxmox, script externe).

```bash
# Job planifié (cron ou CI nightly) : plan en lecture seule
terraform plan -refresh-only -detailed-exitcode -out=/tmp/drift.plan
# Codes de sortie : 0 = pas de changement, 1 = erreur, 2 = changements détectés !
```

Script de détection + alerte :

```bash
#!/bin/bash
# /usr/local/bin/terraform-drift-check.sh
set -euo pipefail
cd /opt/infra/envs/prod
terraform init -input=false >/dev/null
if terraform plan -refresh-only -detailed-exitcode -out=/tmp/drift.plan >/tmp/drift.log 2>&1; then
  echo "OK: aucune dérive"
else
  code=$?
  if [ $code -eq 2 ]; then
    echo "ALERTE: dérive détectée sur prod" | \
      mail -s "[Terraform] Drift prod" infra@entreprise.lan < /tmp/drift.log
    # ou webhook vers le canal d'astreinte
  else
    echo "ERREUR terraform (code $code)" >&2; exit 1
  fi
fi
```

```
# Cron : vérification quotidienne à 6h
0 6 * * * /usr/local/bin/terraform-drift-check.sh
```

> 💡 En CI (GitLab nightly, section 57), le même plan peut ouvrir une issue automatique. L'objectif : **zéro changement hors Terraform**, ou documenté via `ignore_changes`.

---

## 57. CI/CD : pipeline GitLab de revue et d'apply

```yaml
# .gitlab-ci.yml
stages: [validate, plan, apply]

variables:
  TF_IN_AUTOMATION: "true"

fmt-validate:
  stage: validate
  image: hashicorp/terraform:1.10
  script:
    - terraform fmt -check -recursive
    - terraform init -backend=false
    - terraform validate
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"

plan-prod:
  stage: plan
  image: hashicorp/terraform:1.10
  script:
    - cd envs/prod
    - terraform init -input=false
    - terraform plan -input=false -out=tfplan
    - terraform show -json tfplan > plan.json
  artifacts:
    paths: [envs/prod/tfplan, envs/prod/plan.json]
    expire_in: 1 week
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"

apply-prod:
  stage: apply
  image: hashicorp/terraform:1.10
  script:
    - cd envs/prod
    - terraform init -input=false
    - terraform apply -input=false -auto-approve tfplan
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
  when: manual            # apply prod = validation humaine obligatoire
  environment: production
```

Points clés :
- `TF_IN_AUTOMATION=true` : sorties adaptées aux logs CI.
- Le **plan est un artefact** relu dans la MR, puis appliqué tel quel.
- `apply` en `manual` sur `main` uniquement : traçabilité totale.
- Secrets via variables CI masquées/protégées (`TF_VAR_proxmox_api_token`).

---

## 58. Revue de plan en équipe

Checklist du relecteur avant d'approuver un plan :

- [ ] Le plan correspond-il à l'intention de la MR (pas de changement surprise) ?
- [ ] Nombre de `to add / to change / to destroy` cohérent ?
- [ ] **Aucun `to destroy`** inattendu (surtout avec `-/+` = remplacement) ?
- [ ] Les `~` (modifications) touchent-ils des attributs sensibles (disque, réseau, `vm_id`) ?
- [ ] Les ressources avec `prevent_destroy` ne sont pas contournées ?
- [ ] Backend/state : le bon environnement (prod vs staging) ?
- [ ] Secrets : rien en clair dans le plan JSON (`terraform show -json tfplan | grep -i password`) ?
- [ ] Version des providers conforme au lock file ?

```bash
# Inspecter un plan en détail
terraform show tfplan                    # lisible humain
terraform show -json tfplan | jq '.resource_changes[] | {address, actions: .change.actions}'
```

> 💡 Astuce : commentez le résumé du plan dans la MR (bot ou copier-coller). Un plan non relu = un `apply` non relu.

---

## 59. Debug : TF_LOG et les traces

```bash
# Niveaux : TRACE, DEBUG, INFO, WARN, ERROR (défaut: désactivé)
export TF_LOG=DEBUG
export TF_LOG_PATH=/tmp/terraform-debug.log
terraform plan

# Tracer uniquement un provider (moins verbeux)
export TF_LOG_PROVIDER=DEBUG

# Désactiver
unset TF_LOG TF_LOG_PATH
```

```bash
# Autres leviers de debug
terraform plan -refresh=false     # plan sans interroger Proxmox (rapide, state seul)
terraform apply -parallelism=1    # sérialise (isole les problèmes de concurrence)
TF_CLI_ARGS_plan="-lock-timeout=60s" terraform plan   # attend le verrou au lieu d'échouer
```

> ⚠️ Les logs `TRACE`/`DEBUG` peuvent contenir des **secrets** (tokens, mots de passe). Ne les publiez jamais tels quels, purgez-les après usage.

---

## 60. 10+ erreurs classiques et leurs solutions

### Erreur 1 — `Error: Failed to install provider` / checksum mismatch
**Cause** : lock file corrompu ou miroir compromis.
**Solution** : `rm .terraform.lock.hcl && terraform init` (puis `git diff` du nouveau lock avant commit).

### Erreur 2 — `Error: acquiring the state lock`
**Cause** : un `apply` précédent a planté en laissant le verrou, ou deux apply concurrents.
**Solution** : vérifier qu'aucun apply ne tourne vraiment, puis `terraform force-unlock <ID>` (l'ID est dans le message d'erreur). Jamais à l'aveugle.

### Erreur 3 — `Error: Instance cannot be destroyed` (prevent_destroy)
**Cause** : le plan veut détruire une ressource protégée.
**Solution** : c'est le garde-fou qui fait son travail. Vérifier pourquoi la ressource est détruite (renommage ? `moved` oublié ?), corriger le code.

### Erreur 4 — `Provider produced inconsistent result after apply`
**Cause** : le provider retourne après création une valeur différente du plan (souvent un défaut côté API).
**Solution** : mettre à jour le provider ; contournement temporaire : `ignore_changes` sur l'attribut fautif.

### Erreur 5 — `Error: Duplicate resource` / VMID déjà utilisé
**Cause** : `vm_id` en dur déjà pris par une VM hors Terraform.
**Solution** : importer la VM existante (section 51) ou choisir un VMID libre (`pvesh get /cluster/nextid`).

### Erreur 6 — `Error: Reference to undeclared resource`
**Cause** : faute de frappe dans le nom local, ou ressource dans un module non préfixée par `module.`.
**Solution** : `terraform state list` pour voir les vrais noms ; corriger la référence.

### Erreur 7 — Cycle : `Error: Cycle: ...`
**Cause** : A dépend de B qui dépend de A (souvent via deux `depends_on` croisés).
**Solution** : `terraform graph` pour visualiser, supprimer la dépendance artificielle, passer par des data sources si besoin.

### Erreur 8 — `Error: Invalid for_each argument` (valeur non connue au plan)
**Cause** : `for_each` sur un attribut calculé (connu seulement après apply).
**Solution** : `for_each` exige des clés connues au plan. Restructurer (map statique en variable) ou découper en deux `apply`.

### Erreur 9 — Dérive permanente sur un attribut (ex. `tags` réordonnés)
**Cause** : le provider normalise différemment (ordre, casse).
**Solution** : `ignore_changes`, ou normaliser côté code (`sort()`), ou monter de version du provider.

### Erreur 10 — `terraform plan` interminable / timeouts API Proxmox
**Cause** : trop de ressources rafraîchies, API lente.
**Solution** : `-refresh=false` pour un plan rapide (à n'utiliser qu'en lecture), `-parallelism` réduit, vérifier la charge du nœud Proxmox.

### Erreur 11 — `Error: Failed to query available provider packages` (registry injoignable)
**Cause** : pas d'accès internet ou proxy non configuré.
**Solution** : `export HTTPS_PROXY=...`, ou miroir de providers local (`terraform init -backend=false` + `provider_installation` dans `.terraformrc`).

### Erreur 12 — Secrets visibles dans `terraform show` / logs CI
**Cause** : output sans `sensitive = true`, ou `TF_LOG=TRACE` en CI.
**Solution** : marquer `sensitive`, purger les logs, rotation du secret exposé.

## 61. Mise à jour des providers

```bash
# Voir les versions disponibles (registry)
terraform init -upgrade   # monte à la dernière version autorisée par la contrainte ~>

# Monter explicitement de branche (ex. 0.66 -> 0.70) :
# 1. Éditer versions.tf : version = "~> 0.70"
# 2. Lire le CHANGELOG du provider (breaking changes !)
# 3. terraform init -upgrade
# 4. terraform plan  → relire TOUT (un upgrade peut proposer des diffs)
# 5. Tester sur dev/staging avant prod
```

Procédure d'équipe :
1. Une MR par montée de provider (jamais mélangée avec du fonctionnel).
2. `terraform plan` sur **chaque environnement** (dev → staging → prod).
3. Ne jamais `-upgrade` en CI prod sans relecture humaine.

---

## 62. Mise à jour de Terraform / OpenTofu

```bash
# Vérifier la version requise par le projet
grep required_version versions.tf

# Mettre à jour le binaire (ex. via tenv)
tenv terraform install 1.10.5 && tenv terraform use 1.10.5

# Après changement de version MINEURE/MAJEURE :
terraform init
terraform plan   # le format du state peut migrer (sauvegarde auto en .backup)
```

| Saut de version | Risque | Conduite |
|---|---|---|
| Patch (1.10.1 → 1.10.2) | faible | direct, plan de contrôle |
| Mineur (1.9 → 1.10) | moyen | lire l'upgrade guide, tester dev/staging |
| Majeur (1.x → 2.x) | élevé | plan de migration, fenêtre de maintenance |

> ⚠️ `required_version` bloque l'équipe sur une version non prévue : mettez-la à jour **volontairement** dans la même MR que le test.

---

## 63. OpenTofu : différences avec Terraform

| Fonctionnalité | Terraform | OpenTofu |
|---|---|---|
| Licence | BSL | MPL-2.0 |
| `terraform {}` / CLI | `terraform` | `tofu` (alias `terraform` possible) |
| Chiffrement du state au repos | ❌ (délégué au backend) | ✅ natif (`key_provider`) |
| `removed` block | ❌ | ✅ (retirer proprement sans détruire) |
| Variables d'itération `for_each`/`count` dans `import` | partiel | ✅ (`for_each` sur blocs `import`) |
| Early evaluation / `static` | ❌ | ✅ (évaluation anticipée) |
| Registre par défaut | registry.terraform.io | idem (compatible) |

Exemple : chiffrement natif du state avec OpenTofu :

```hcl
# versions.tf (OpenTofu)
terraform {
  encryption {
    key_provider "pbkdf2" "main" {
      passphrase = var.state_passphrase   # via TF_VAR_state_passphrase
    }
    method "aes_gcm" "main" {
      keys = key_provider.pbkdf2.main
    }
    state {
      method = method.aes_gcm.main
    }
    plan {
      method = method.aes_gcm.main
    }
  }
}
```

---

## 64. Migration Terraform → OpenTofu

La migration est conçue pour être **sans douleur** (même langage, même state) :

```bash
# 1. Sauvegarde du state (obligatoire)
terraform state pull > /srv/backups/pre-tofu-$(date +%F).tfstate

# 2. Installer tofu (section 5), puis dans le projet :
tofu init          # lit le même backend, le même lock file

# 3. Vérifier : le plan doit être VIDE (aucun changement)
tofu plan
# "No changes. Your infrastructure matches the configuration."

# 4. Basculer la CI sur l'image opentofu/opentofu:latest
# 5. Optionnel : renommer terraform {} -> mêmes blocs (compatibles),
#    activer le chiffrement du state (section 63)
```

Points d'attention :
- `required_version` : le remplacer par la contrainte tofu si vous figez (`tofu` ignore `required_version` ? non — il l'honore ; utilisez la version tofu correspondante).
- Les providers du registry Terraform fonctionnent tels quels.
- Gardez le binaire `terraform` sous la main le temps de la transition (rollback = `terraform init` + `plan` vide).

---

## 65. Cas pratique 1 : parc de 10 VM Proxmox depuis du code

Objectif : 10 VM web identiques, réparties sur 2 nœuds, IP automatiques, cloud-init, tags.

```hcl
# versions.tf
terraform {
  required_version = ">= 1.9.0, < 1.11.0"
  required_providers {
    proxmox = { source = "bpg/proxmox", version = "~> 0.66" }
  }
  backend "s3" {
    bucket = "infra-terraform-state-prod"
    key    = "proxmox/web-farm/terraform.tfstate"
    region = "eu-west-3"
    encrypt = true
    dynamodb_table = "terraform-state-locks"
  }
}
```

```hcl
# variables.tf
variable "proxmox_endpoint"  { type = string }
variable "proxmox_api_token" { type = string, sensitive = true }
variable "vm_count"    { type = number, default = 10 }
variable "base_vmid"   { type = number, default = 200 }
variable "subnet"      { type = string, default = "10.20.0.0/24" }
variable "gateway"     { type = string, default = "10.20.0.1" }
variable "nodes"       { type = list(string), default = ["pve1", "pve2"] }
variable "template_id" { type = number, default = 9000 }  # template debian-12-cloudinit
variable "ssh_key"     { type = string, sensitive = true }
```

```hcl
# main.tf
provider "proxmox" {
  endpoint  = var.proxmox_endpoint
  api_token = var.proxmox_api_token
  insecure  = true   # ⚠️ labo avec CA autosignée ; false en prod avec CA valide
}

locals {
  vms = {
    for i in range(var.vm_count) : format("web-%02d", i + 1) => {
      node = var.nodes[i % length(var.nodes)]       # alternance pve1/pve2
      vmid = var.base_vmid + i
      ip   = cidrhost(var.subnet, 11 + i)
    }
  }
}

resource "proxmox_virtual_environment_file" "cloud_config" {
  for_each     = local.vms
  content_type = "snippets"
  datastore_id = "local"
  node_name    = each.value.node
  source_raw {
    file_name = "${each.key}-cloud-config.yaml"
    data = templatefile("${path.module}/templates/cloud-init.tftpl", {
      hostname   = each.key
      admin_user = "admin"
      ssh_key    = var.ssh_key
    })
  }
}

resource "proxmox_virtual_environment_vm" "web" {
  for_each  = local.vms
  name      = each.key
  node_name = each.value.node
  vm_id     = each.value.vmid
  tags      = ["terraform", "prod", "web", "farm"]

  clone {
    vm_id = var.template_id
    full  = true
  }
  cpu { cores = 4 }
  memory { dedicated = 8192 }
  disk {
    datastore_id = "ceph-vm"
    interface    = "scsi0"
    size         = 40
  }
  network_device { bridge = "vmbr0", vlan_id = 20 }

  initialization {
    datastore_id      = "local-lvm"
    user_data_file_id = proxmox_virtual_environment_file.cloud_config[each.key].id
    ip_config {
      ipv4 { address = "${each.value.ip}/24", gateway = var.gateway }
    }
  }

  lifecycle {
    replace_triggered_by = [proxmox_virtual_environment_file.cloud_config[each.key].id]
  }
}
```

```hcl
# outputs.tf
output "vm_inventory" {
  description = "Nom, nœud, VMID et IP de chaque VM"
  value = {
    for k, vm in proxmox_virtual_environment_vm.web : k => {
      node = vm.node_name
      vmid = vm.vm_id
      ip   = local.vms[k].ip
    }
  }
}
```

```bash
terraform init
terraform plan -out=tfplan        # vérifier : 10 VM + 10 snippets = 20 ajouts
terraform apply tfplan
terraform output -json vm_inventory | jq .
```

---

## 66. Cas pratique 2 : reprise après sinistre

Scénario : le nœud `pve1` est perdu (incendie). Les VM critiques doivent renaître sur `pve2`/`pve3` depuis le code + les sauvegardes.

**Pré-requis (à préparer AVANT le sinistre)** :
- [ ] Code Terraform versionné (Git) décrivant tout le parc
- [ ] State distant (S3 versionné) — **hors du site sinistré** si possible
- [ ] Sauvegardes VM (Proxmox Backup Server) répliquées hors site
- [ ] `terraform.tfvars` de prod sauvegardé chiffré (SOPS)

**Procédure** :

```bash
# 1. Sur le poste de secours, cloner le code
git clone https://git.entreprise.lan/infra/proxmox-prod.git && cd proxmox-prod/envs/prod

# 2. Restaurer les variables chiffrées
sops --decrypt secrets.tfvars.enc > /tmp/secrets.tfvars

# 3. Pointer vers le cluster de secours (nouveau endpoint)
export TF_VAR_proxmox_endpoint="https://pve2-secours.lan:8006/"
export TF_VAR_proxmox_api_token="..."

# 4. Init + plan : Terraform détecte les VM manquantes et propose de les recréer
terraform init
terraform plan -out=tfplan   # RELIRE : que des créations attendues

# 5. Restaurer les données depuis PBS AVANT ou APRÈS selon la stratégie :
#    Option A (simple) : Terraform recrée des VM vierges, puis restauration PBS par-dessus
#    Option B : adapter le code pour cloner depuis les sauvegardes si le provider le permet
terraform apply tfplan
```

**Leçons** :
- Le code seul ne restaure pas les **données** : la sauvegarde (PBS) reste indispensable.
- Tester la procédure 1×/an (exercice PRA) : un plan jamais testé est un vœu pieux.
- `prevent_destroy` sur les BDD : en cas de vrai sinistre, c'est une protection, pas un obstacle — on le retire consciemment si besoin.

---

## 67. Cas pratique 3 : réseau complet avec VLAN et cloud-init

```hcl
# VMs sur 3 VLANs : 10 (DMZ web), 20 (applicatif), 30 (BDD)
locals {
  vlans = { web = 10, app = 20, db = 30 }
  tiers = {
    "web-01" = { vlan = "web", ip = "10.10.10.11/24", node = "pve1" }
    "web-02" = { vlan = "web", ip = "10.10.10.12/24", node = "pve2" }
    "app-01" = { vlan = "app", ip = "10.10.20.11/24", node = "pve1" }
    "db-01"  = { vlan = "db",  ip = "10.10.30.11/24", node = "pve2" }
  }
}

resource "proxmox_virtual_environment_vm" "tiers" {
  for_each  = local.tiers
  name      = each.key
  node_name = each.value.node
  tags      = ["terraform", "prod", each.value.vlan]

  clone { vm_id = var.template_id, full = true }
  cpu { cores = each.value.vlan == "db" ? 8 : 4 }
  memory { dedicated = each.value.vlan == "db" ? 16384 : 8192 }
  disk {
    datastore_id = "ceph-vm"
    interface    = "scsi0"
    size         = each.value.vlan == "db" ? 100 : 40
  }
  network_device {
    bridge  = "vmbr0"
    vlan_id = local.vlans[each.value.vlan]
  }
  initialization {
    datastore_id = "local-lvm"
    ip_config {
      ipv4 {
        address = each.value.ip
        gateway = "10.10.${local.vlans[each.value.vlan]}.1"
      }
    }
  }
}
```

> 💡 La passerelle est dérivée du VLAN (`10.10.<vlan>.1`) : un seul endroit à maintenir. Les bridges VLAN-aware (`bridge-vlan-aware yes`) doivent être configurés côté Proxmox au préalable.

---

## 68. Cas pratique 4 : cluster à 3 nœuds avec anti-affinité

Pour la HA, les 3 VM d'un service ne doivent pas être sur le même nœud Proxmox :

```hcl
locals {
  ha_nodes = ["pve1", "pve2", "pve3"]
  cluster_vms = {
    for i in range(3) : format("kafka-%02d", i + 1) => {
      node = local.ha_nodes[i % length(local.ha_nodes)]
      vmid = 300 + i
      ip   = cidrhost("10.30.0.0/24", 11 + i)
    }
  }
}

resource "proxmox_virtual_environment_vm" "kafka" {
  for_each  = local.cluster_vms
  name      = each.key
  node_name = each.value.node   # pve1, pve2, pve3 : anti-affinité naturelle
  vm_id     = each.value.vmid
  tags      = ["terraform", "prod", "kafka", "ha"]

  clone { vm_id = var.template_id, full = true }
  cpu { cores = 4 }
  memory { dedicated = 8192 }
  disk { datastore_id = "ceph-vm", interface = "scsi0", size = 60 }
  network_device { bridge = "vmbr0", vlan_id = 30 }

  initialization {
    datastore_id = "local-lvm"
    ip_config { ipv4 { address = "${each.value.ip}/24", gateway = "10.30.0.1" } }
  }

  # Garde-fou : jamais de destruction accidentelle du cluster
  lifecycle { prevent_destroy = true }
}

output "kafka_placement" {
  value = { for k, vm in proxmox_virtual_environment_vm.kafka : k => vm.node_name }
}
```

> 💡 Complétez avec les groupes HA de Proxmox (`ha-manager`) côté cluster pour le redémarrage automatique en cas de panne de nœud — Terraform pose les VM, Proxmox HA les protège à l'exécution.

---

## 69. Supervision et alerting autour de Terraform

Terraform ne supervise pas l'infra à l'exécution : il faut l'entourer.

| Couche | Outil | Rôle |
|---|---|---|
| Drift | `plan -refresh-only` nightly (section 56) | détecter les changements hors code |
| Métriques Proxmox | Prometheus + `pve-exporter` | CPU/RAM/disque des nœuds et VM |
| Logs | Loki / journald centralisé | corréler un apply avec un incident |
| Alerting | Alertmanager / mail | dérive, échec de pipeline, verrou bloqué |
| Audit | `TF_LOG` + logs CI archivés | qui a appliqué quoi, quand |

```bash
# Exemple : alerte si un verrou de state dure trop longtemps (DynamoDB)
aws dynamodb scan --table-name terraform-state-locks --region eu-west-3 \
  --query 'Items[?attribute_exists(LockID)]' | jq length
```

> 💡 Ajoutez un **dashboard "IaC"** : dernier apply par env, résultat du drift check, version des providers. C'est ce que l'astreinte regarde en premier.

---

## 70. Tester son code : terraform test et check blocks

```hcl
# tests/web.tftest.hcl — tests natifs (Terraform ≥ 1.6)
mock_provider "proxmox" {}   # simule le provider, aucun appel réel

run "naming_convention" {
  command = plan
  variables {
    vm_count = 3
  }
  # Vérifie que les noms suivent web-XX
  assert {
    condition     = alltrue([for k in keys(proxmox_virtual_environment_vm.web) : can(regex("^web-[0-9]{2}$", k))])
    error_message = "Les VM doivent se nommer web-XX."
  }
}
```

```hcl
# check blocks : assertions évaluées à chaque plan/apply (Terraform ≥ 1.5)
check "vm_count_coherent" {
  assert {
    condition     = var.vm_count == length(local.vms)
    error_message = "Incohérence entre vm_count et la map des VM."
  }
}
```

```bash
terraform test          # exécute les .tftest.hcl
```

---

## 71. Documenter : terraform-docs

```bash
# Installation
curl -sL https://github.com/terraform-docs/terraform-docs/releases/latest/download/terraform-docs-v0.x.x-linux-amd64.tar.gz \
  | tar xz -C /tmp && sudo mv /tmp/terraform-docs /usr/local/bin/

# Génère la doc du module (variables, outputs) en Markdown
terraform-docs markdown table modules/vm/ > modules/vm/README.md
```

Extrait généré :

```markdown
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| name | Nom de la VM | `string` | n/a | yes |
| cores | vCPU | `number` | `2` | no |
```

> 💡 En CI : `terraform-docs --check` échoue si le README n'est pas à jour → documentation toujours synchronisée.

---

## 72. Estimer les coûts : Infracost

Infracost estime le coût mensuel d'un plan (utile surtout pour le cloud public ; pour Proxmox on-prem, adaptez avec des coûts internes au kWh/To).

```bash
# Installation
curl -fsSL https://raw.githubusercontent.com/infracost/infracost/master/scripts/install.sh | sh

# Estimation sur un plan
terraform plan -out=tfplan
terraform show -json tfplan > plan.json
infracost breakdown --path plan.json
```

Pour l'on-prem, une approche simple : un `locals` de coût interne exposé en output :

```hcl
locals {
  # Coûts internes mensuels (exemples, à ajuster)
  cost_vcpu_gb_ram = 2.5   # €/mois par vCPU
  cost_gb_disk     = 0.08  # €/mois par Go Ceph
}
output "cout_mensuel_estime" {
  value = sum([
    for k, vm in proxmox_virtual_environment_vm.web :
    4 * local.cost_vcpu_gb_ram + 40 * local.cost_gb_disk
  ])
}
```

---

## 73. Conventions de nommage et style d'équipe

```hcl
# Fichiers : minuscules, par thème
# versions.tf, providers.tf, backend.tf, variables.tf, locals.tf,
# main.tf (ou vms.tf, network.tf), outputs.tf, checks.tf

# Ressources : snake_case, nom local explicite
resource "proxmox_virtual_environment_vm" "web_prod_01" { ... }  # ✅
resource "proxmox_virtual_environment_vm" "vm1" { ... }          # ❌

# Noms réels : <env>-<role>-<nn>
name = "prod-web-01"   # ✅  (tri, filtrage, inventaire)

# Tags : toujours env + managed-by + rôle
tags = ["terraform", "prod", "web"]

# Variables : noms explicites, description obligatoire
variable "proxmox_endpoint" { ... }   # ✅
variable "endpoint" { ... }           # ❌ trop vague
```

Règles d'équipe (à mettre dans `CONTRIBUTING.md`) :
- `terraform fmt` avant chaque commit (pre-commit hook).
- Pas de valeur en dur : tout ce qui varie par env → variable.
- Un `README.md` par stack expliquant : but, prérequis, `init/plan/apply`, environnements.

## 74. Checklist de mise en production

Avant le premier `apply` sur la prod :

**Code**
- [ ] `terraform fmt -check` OK, `terraform validate` OK
- [ ] `required_version` + `required_providers` figés, lock file versionné
- [ ] Aucun secret en clair (`gitleaks detect` OK)
- [ ] `prevent_destroy` sur les ressources critiques (BDD, NAS)
- [ ] Variables validées (`validation` blocks), descriptions renseignées

**State & backend**
- [ ] Backend distant configuré (S3 versionné + chiffrement + DynamoDB, ou équivalent)
- [ ] Sauvegarde du state testée (pull → restore sur un backend de test)
- [ ] Accès IAM restreints (équipe infra + rôle CI uniquement)

**Processus**
- [ ] Pipeline CI : fmt → validate → plan sur MR, apply manuel sur main
- [ ] Drift check nightly en place + alerte
- [ ] Procédure de reprise après sinistre écrite et testée
- [ ] `CONTRIBUTING.md` / conventions d'équipe diffusées
- [ ] Plan relu par un pair avant apply (jamais seul sur prod)

**Exploitation**
- [ ] Monitoring (Prometheus/pve-exporter) + alerting sur le parc
- [ ] Sauvegardes PBS configurées et répliquées hors site
- [ ] Documentation (`terraform-docs`) à jour

---

## 75. Pense-bête de poche (cheat sheet)

```bash
# ── Quotidien ──
terraform init                          # 1ère fois / nouveau module/provider
terraform plan -out=tfplan              # plan relu, sauvegardé
terraform apply tfplan                  # apply EXACTEMENT ce plan
terraform output -json | jq .           # voir les outputs

# ── Dépannage ──
terraform validate                      # cohérence du code
terraform plan -refresh-only            # voir la dérive sans toucher
terraform state list                    # que contient le state ?
terraform force-unlock <ID>             # libérer un verrou (ID du message)
TF_LOG=DEBUG terraform plan             # debug (purger les logs après)

# ── Recréer / importer ──
terraform plan -replace='<addr>' -out=tfplan   # recréer une ressource
terraform import '<addr>' '<id-provider>'      # importer l'existant

# ── Workspaces ──
terraform workspace new dev && terraform workspace select dev

# ── OpenTofu ──
tofu plan / tofu apply                  # mêmes commandes, binaire tofu
```

| Symbole du plan | Signification |
|---|---|
| `+` | création |
| `~` | modification en place |
| `-` | destruction |
| `-/+` | remplacement (destroy puis create) |
| `<=` | lecture (data source) |

---

## 76. Glossaire

| Terme | Définition |
|---|---|
| **IaC** | Infrastructure as Code : décrire l'infra dans des fichiers versionnés |
| **HCL** | HashiCorp Configuration Language, le langage de Terraform |
| **Provider** | Plugin traduisant le HCL en appels API (Proxmox, AWS…) |
| **Resource** | Objet géré par Terraform (VM, disque, DNS…) |
| **Data source** | Lecture d'un objet existant, sans le gérer |
| **State** | Fichier JSON mémorisant le mapping code ↔ objets réels |
| **Backend** | Où est stocké le state (local, S3, Terraform Cloud…) |
| **Plan** | Calcul des actions nécessaires, sans rien modifier |
| **Apply** | Exécution des actions du plan |
| **Drift** | Écart entre le réel et le code (modif hors Terraform) |
| **Taint** | Marquer une ressource comme à recréer |
| **Module** | Paquet réutilisable de configuration Terraform |
| **Workspace** | Instance isolée du state dans un même backend |
| **Lock** | Verrou du state distant empêchant deux apply concurrents |
| **Idempotence** | Propriété : réappliquer ne change rien |
| **for_each / count** | Meta-arguments de répétition de ressources |
| **Dynamic block** | Génération de sous-blocs par boucle |
| **Lifecycle** | Meta-bloc contrôlant création/destruction (prevent_destroy…) |
| **Provisioner** | Script exécuté à la création (à éviter) |
| **cloud-init** | Standard de configuration initiale des VM cloud |
| **OpenTofu** | Fork open source (MPL-2.0) de Terraform |
| **SOPS** | Outil de chiffrement de fichiers (avec age) |
| **TF_LOG** | Variable d'activation des logs de debug |

---

## 77. Quiz : 10 questions (énoncés)

1. Quelle commande affiche les changements prévus **sans rien modifier** ?
2. Pourquoi faut-il relire le plan avant `terraform apply` en production ?
3. Quelle est la différence entre `count` et `for_each` pour créer 10 VM ?
4. Où sont stockés les secrets si on les met dans une variable sans précaution ? (2 endroits)
5. À quoi sert `terraform init` ? Citez 3 choses qu'il fait.
6. Que se passe-t-il si deux personnes lancent `terraform apply` en même temps avec un state local ?
7. Expliquez `prevent_destroy` et donnez un cas d'usage.
8. Comment importer une VM créée manuellement dans Proxmox sans la recréer ?
9. Pourquoi les provisioners `remote-exec` sont-ils déconseillés ? Citez 2 alternatives.
10. Quelle est la principale différence de licence entre Terraform et OpenTofu ?

---

## 78. Quiz : réponses commentées

1. **`terraform plan`** (idéalement `plan -out=tfplan`). C'est une opération de lecture seule : aucun changement n'est appliqué.
2. Parce que le plan est le **contrat d'exécution** : il liste créations, modifications et surtout **destructions**. Relire évite de détruire une BDD de prod par une coquille dans le code.
3. `count` indexe par position (0, 1, 2…) : retirer une VM du milieu décale les suivantes et peut provoquer des remplacements en cascade. `for_each` indexe par **clé** (`"web-01"`) : chaque VM a une identité stable, la suppression est chirurgicale.
4. Dans le **state** (`terraform.tfstate`, en clair dans les attributs) et potentiellement dans les **logs** (`TF_LOG=TRACE`, sortie `plan`/`apply`). D'où : backend chiffré + `sensitive = true` + jamais de secret en clair dans Git.
5. `terraform init` : (1) télécharge les **providers** requis, (2) télécharge les **modules**, (3) configure/initialise le **backend** (et migre le state si besoin).
6. **Corruption du state** : sans verrouillage (state locking), les deux apply écrivent en concurrence et le state devient incohérent. D'où l'obligation d'un backend distant avec verrou en équipe.
7. `lifecycle { prevent_destroy = true }` fait **échouer** tout plan qui voudrait détruire la ressource. Cas d'usage : BDD de production, NAS, toute ressource difficilement reconstructible.
8. Avec le bloc `import { to = ...; id = "..." }` (Terraform ≥ 1.5) ou `terraform import`, après avoir déclaré la `resource` correspondante dans le code, puis ajustement jusqu'à obtenir un plan sans diff non voulu.
9. Ils sont **hors modèle déclaratif** (pas de drift detection), fragiles (SSH/réseau), non idempotents, et stockent des secrets de connexion. Alternatives : **cloud-init** pour la config initiale, **Ansible** après l'apply pour la configuration applicative.
10. Terraform est sous **BSL** (Business Source License, usage restreint par HashiCorp) tandis qu'OpenTofu est sous **MPL-2.0**, une vraie licence open source gouvernée par la communauté (Linux Foundation).

---

## 79. Pour aller plus loin

| Ressource | Pourquoi |
|---|---|
| `registry.terraform.io/providers/bpg/proxmox` | Documentation officielle du provider Proxmox (arguments exacts par version) |
| `developer.hashicorp.com/terraform/docs` | Référence du langage et de la CLI |
| `opentofu.org/docs` | Documentation OpenTofu (chiffrement du state, `removed` blocks) |
| `cloudinit.readthedocs.io` | Référence cloud-init (modules, exemples) |
| `pve.proxmox.com/wiki` | API et concepts Proxmox VE (prérequis au provider) |
| `terraform-docs` | Génération de documentation des modules |
| `infracost` | Estimation de coûts sur plan |
| `checkov` / `tfsec` | Analyse statique de sécurité du code Terraform |
| `terragrunt` | Surcouche DRY pour gérer backend/variables sur N environnements |
| `packer` | Construction de templates Proxmox versionnés (avec le plugin Proxmox) |

**Feuille de route suggérée** :
1. Labo : déployer 3 VM avec le cas pratique §65 sur un Proxmox de test.
2. Mettre le state sur MinIO/S3 + verrou, brancher le drift check nightly.
3. Écrire le module `vm` d'équipe + pipeline GitLab (§57).
4. Exercice PRA : tout détruire et reconstruire depuis le code (§66).
5. Évaluer OpenTofu (§63-64) pour les nouveaux projets.

---

## 80. Annexe : fichiers de référence complets

### A. `backend.tf` — S3 + DynamoDB (prod)

```hcl
terraform {
  backend "s3" {
    bucket         = "infra-terraform-state-prod"
    key            = "proxmox/prod/terraform.tfstate"
    region         = "eu-west-3"
    encrypt        = true
    dynamodb_table = "terraform-state-locks"
  }
}
```

### B. `.gitignore` Terraform/OpenTofu

```gitignore
.terraform/
.tofu/
*.tfstate
*.tfstate.*
*.tfvars
!*.tfvars.example
.terraform.lock.hcl
override.tf
override.tf.json
*_override.tf
*_override.tf.json
crash.log
terraform.tfstate.backup
```

### C. `templates/cloud-init.tftpl` — template complet

```yaml
#cloud-config
hostname: ${hostname}
manage_etc_hosts: true
package_update: true
package_upgrade: false
packages:
  - qemu-guest-agent
  - curl
  - vim
  - htop
users:
  - name: ${admin_user}
    sudo: ALL=(ALL) NOPASSWD:ALL
    shell: /bin/bash
    ssh_authorized_keys:
      - ${ssh_key}
ntp:
  servers: [10.10.0.1, 1.1.1.1]
runcmd:
  - systemctl enable --now qemu-guest-agent
  - timedatectl set-timezone Europe/Paris
final_message: "Cloud-init termine pour ${hostname}"
```

### D. Script `drift-check.sh` complet (cron quotidien)

```bash
#!/bin/bash
# /usr/local/bin/terraform-drift-check.sh — à adapter par environnement
set -euo pipefail
ENV_DIR="/opt/infra/envs/prod"
ALERT_MAIL="infra@entreprise.lan"
cd "$ENV_DIR"
terraform init -input=false >/dev/null 2>&1
set +e
terraform plan -refresh-only -detailed-exitcode -out=/tmp/drift.plan >/tmp/drift.log 2>&1
code=$?
set -e
if [ "$code" -eq 0 ]; then
  echo "$(date -Iseconds) OK: aucune derive" | tee -a /var/log/terraform-drift.log
elif [ "$code" -eq 2 ]; then
  echo "$(date -Iseconds) ALERTE: derive detectee" | tee -a /var/log/terraform-drift.log
  mail -s "[Terraform] Derive detectee sur prod" "$ALERT_MAIL" < /tmp/drift.log
else
  echo "$(date -Iseconds) ERREUR terraform code=$code" | tee -a /var/log/terraform-drift.log
  exit 1
fi
```

---

*Fin du guide — bon courage, et rappelez-vous : **on relit toujours le plan avant `apply`**.* 🚀
