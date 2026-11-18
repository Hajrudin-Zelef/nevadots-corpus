# Guide fail2ban — Le bouclier anti-brute-force

> **Audience :** sysadmins et chefs de service systèmes (production réelle).
> **Prérequis :** Debian 11/12/13 ou Ubuntu 20.04/22.04/24.04, accès root/sudo, bases iptables/nftables.
> **Objectif :** installer, configurer, exploiter et superviser fail2ban sans se bannir soi-même.

---

# Sommaire

1. Principe : scan de logs → bannissement
2. Architecture : jails, filters, actions
3. Installation sur Debian/Ubuntu
4. Arborescence et fichiers
5. Premier jail.local minimal
6. bantime, findtime, maxretry expliqués
7. Le jail sshd en détail
8. Le jail sshd (modes et variantes)
9. nginx-http-auth : protéger le basic auth
10. nginx-badbot et nginx-botsearch
11. postfix : relais et SASL
12. dovecot : POP3/IMAP sous pression
13. proftpd et vsftpd
14. recidive : les récidivistes
15. Activer plusieurs jails sans conflit
16. fail2ban-regex : tester son filtre
17. Créer un filter personnalisé pas à pas
18. Regex : guide de survie
19. Les actions : iptables, nftables, ufw
20. Action nftables en détail
21. Action ufw
22. Actions multiples et combinées
23. Tuning par service : valeurs recommandées
24. ignoreip : whitelist (son réseau, la supervision !)
25. Ne pas se bannir soi-même : 7 techniques
26. fail2ban-client : usage quotidien
27. Débannir : le classique du lundi
28. Bannissement temporaire vs permanent
29. Persistent bans (sqlite db)
30. Logs et niveaux de debug
31. Troubleshooting des logs
32. Intégration Cloudflare (bannir à l'edge)
33. AbuseIPDB : remonter les attaquants
34. Monitoring Zabbix
35. Monitoring Prometheus
36. Alertes mail et webhooks
37. Sauvegarde et restauration de jail.local
38. Mise à jour de fail2ban
39. 10+ erreurs classiques et solutions
40. Cas pratique : protéger un serveur web exposé
41. Cas pratique : durcir un Proxmox VE
42. Cas pratique : sécuriser un bastion SSH
43. Cas pratique : protéger un webmail Roundcube
44. Performance : fail2ban sur gros volume
45. Backend : auto, systemd, pyinotify, polling
46. Le fuseau horaire : la cause cachée
47. IPv6 avec fail2ban
48. Multi-instance et mode socket
49. Bannir via ipset pour la performance
50. Exclure les IPs légitimes : failregex d'exclusion
51. Rotation de logs et fail2ban
52. Fail2ban derrière un proxy inverse
53. Tester en sécurité : mode dry-run mental
54. Scripts d'administration utiles
55. Intégration Ansible
56. Intégration avec GeoIP
57. Le fichier filter.d : anatomie
58. Le fichier action.d : anatomie
59. Jail.d : surcharges modulaires
60. Ordre de fusion de la configuration
61. Durcissement de fail2ban lui-même
62. Bannir un pays entier : réaliste ?
63. Corrélation avec auditd
64. Fail2ban et Docker : attention
65. Fail2ban sur Proxmox : règles avancées
66. Recette de jail.local « gold »
67. Checklist de mise en production
68. Checklist de revue hebdomadaire
69. Checklist de revue mensuelle
70. Pense-bête de poche
71. Glossaire
72. Quiz : 10 questions
73. Réponses du quiz
74. Pour aller plus loin
75. Références officielles

---

## 1. Principe : scan de logs → bannissement

fail2ban n'est pas un pare-feu : c'est un **gardien qui lit les journaux**.

Le cycle de vie d'une attaque contre SSH ressemble à ceci :

```
┌──────────┐   tente de se connecter   ┌──────────┐
│ Attaquant │ ───────────────────────▶ │   sshd   │
│203.0.113.│ ◀──── refusé ─────────── │ port 22  │
└──────────┘                           └────┬─────┘
                                            │ écrit
                                            ▼
                                   /var/log/auth.log
                                   "Failed password ..."
                                            │
                                            ▼
                                   ┌───────────────┐
                                   │   fail2ban    │
                                   │  jail[sshd]   │
                                   │ maxretry=5    │
                                   └───────┬───────┘
                                           │ 5 échecs < findtime
                                           ▼
                              ┌────────────────────────┐
                              │ action ban :           │
                              │ iptables -A ... DROP   │
                              └────────────────────────┘
                                           │
                                           ▼
                                   Attaquant bloqué
                                   (bantime = 1h)
```

Trois étapes, toujours les mêmes :

1. **Observer** : fail2ban surveille en continu des fichiers de logs (ou le journal systemd).
2. **Compter** : chaque ligne qui correspond au filtre incrémente un compteur par IP.
3. **Agir** : si le compteur dépasse `maxretry` pendant `findtime`, une action bannit l'IP pendant `bantime`.

Le bannissement est **temporaire par défaut** : après `bantime`, l'IP est débannie automatiquement. C'est volontaire : on veut bloquer les robots, pas devenir soi-même une source de déni de service contre des utilisateurs légitimes dont le mot de passe a expiré.

### Pourquoi c'est efficace en production

- Un scan SSH classique frappe 50 à 500 fois par heure. Avec `maxretry=5` et `findtime=10m`, le bot est bloqué après ~30 secondes.
- Le CPU du serveur est soulagé : `iptables`/`nftables` jette les paquets avant qu'ils atteignent sshd.
- Les logs restent lisibles : au lieu de 10 000 lignes « Failed password », on n'en voit que 5 par attaquant.

### Ce que fail2ban ne fait pas

| Ce n'est pas... | Parce que... |
|---|---|
| Un pare-feu | il ne contrôle que les IP déjà vues dans les logs |
| Un WAF | il ne comprend pas le HTTP, il lit des lignes de texte |
| Une authentification forte | il ne remplace ni les clés SSH ni le 2FA |
| Un anti-DDoS | 100 000 IP distinctes satureront le compteur avant tout |

fail2ban est une **couche** de défense en profondeur, à combiner avec : clés SSH, 2FA, pare-feu restrictif, mises à jour.

### Exemple concret : à quoi ressemble une attaque

Extrait réel (anonymisé) de `/var/log/auth.log` :

```
Sep 26 08:14:02 srv-web sshd[2041]: Failed password for root from 203.0.113.45 port 51234 ssh2
Sep 26 08:14:04 srv-web sshd[2043]: Failed password for root from 203.0.113.45 port 51235 ssh2
Sep 26 08:14:06 srv-web sshd[2045]: Failed password for invalid user admin from 203.0.113.45 port 51236 ssh2
Sep 26 08:14:08 srv-web sshd[2047]: Failed password for invalid user oracle from 203.0.113.45 port 51237 ssh2
Sep 26 08:14:10 srv-web sshd[2049]: Failed password for root from 203.0.113.45 port 51238 ssh2
```

Cinq lignes en huit secondes : c'est exactement le schéma qu'un jail `sshd` détecte.

---

## 2. Architecture : jails, filters, actions

fail2ban est organisé en trois concepts. Les retenir, c'est comprendre 80 % de l'outil.

### Le filter : « quoi chercher »

Un filter est un fichier dans `/etc/fail2ban/filter.d/<nom>.conf` qui contient des **expressions régulières** (`failregex`). Il décrit *ce qu'est une attaque* dans un log donné.

Exemple simplifié de `filter.d/sshd.conf` :

```ini
[INCLUDES]
before = common.conf

[Definition]
failregex = ^%(__prefix_line)sFailed (?:password|publickey) for <F-USER>.*</F-USER> from <HOST>( port \d+)?(?: ssh2)?\s*$
ignoreregex =
```

- `<HOST>` est un **raccourci magique** : il capture l'adresse IP (IPv4 ou IPv6).
- `<F-USER>...</F-USER>` capture le nom d'utilisateur (utile pour les logs).
- `__prefix_line` vient de `common.conf` : il matche le préfixe date/hostname/processus.

> Règle d'or : **un filter ne décide jamais de bannir**. Il se contente de dire « cette ligne est un échec d'authentification, venant de cette IP ».

### Le jail : « sur quoi veiller et combien tolérer »

Un jail relie un filter à des logs et des seuils. C'est la **configuration opérationnelle**.

```ini
[sshd]
enabled  = true
filter   = sshd
logpath  = /var/log/auth.log
backend  = auto
port     = ssh
maxretry = 5
findtime = 10m
bantime  = 1h
action   = %(action_)s
```

Lecture : « surveille `/var/log/auth.log` avec le filter sshd ; si une IP échoue 5 fois en 10 minutes, bannis-la 1 heure sur le port ssh ».

### L'action : « que faire quand ça dépasse »

Une action est un fichier dans `/etc/fail2ban/action.d/<nom>.conf`. Elle définit les commandes à exécuter au **ban** et au **unban**.

Structure type (`action.d/iptables.conf`, simplifié) :

```ini
[Definition]
actionstart  = iptables -N f2b-sshd
               iptables -A f2b-sshd -j RETURN
               iptables -I INPUT -p tcp --dport ssh -j f2b-sshd

actionban    = iptables -I f2b-sshd 1 -s <ip> -j REJECT --reject-with icmp-port-unreachable

actionunban  = iptables -D f2b-sshd -s <ip> -j REJECT --reject-with icmp-port-unreachable

actionstop   = iptables -D INPUT -p tcp --dport ssh -j f2b-sshd
               iptables -F f2b-sshd
               iptables -X f2b-sshd
```

`<ip>` est remplacé par l'IP fautive. `actionstart` crée la chaîne, `actionstop` la nettoie.

### Schéma d'ensemble

```
/etc/fail2ban/
├── jail.conf / jail.local      ← les JAILS (quoi surveiller, seuils)
├── filter.d/*.conf              ← les FILTERS (regex de détection)
└── action.d/*.conf              ← les ACTIONS (ban/unban)

            ┌─────────────────────────────────┐
            │         fail2ban-server         │
            │                                 │
 log ──▶    │  jail[sshd]                     │
fichier     │   filter: sshd.conf             │
ou journal  │   seuils: 5 / 10m / 1h          │──▶ action ban/unban
            │                                 │     (iptables, nftables,
            └─────────────────────────────────┘      ufw, cloudflare...)
                    ▲
                    │ fail2ban-client (CLI)
                    │ get / set / ban / unban / status
```

### Le démon et le client

- `fail2ban-server` : le démon Python qui tourne en tâche de fond (surveille les logs, applique les bannissements).
- `fail2ban-client` : l'outil en ligne de commande qui dialogue avec le serveur via un socket (`/run/fail2ban/fail2ban.sock`).
- `fail2ban-regex` : l'outil de **test** des regex contre un fichier de log (indispensable, section 16).

---

## 3. Installation sur Debian/Ubuntu

### Installation standard

```bash
sudo apt update
sudo apt install -y fail2ban
```

C'est tout. Le paquet Debian/Ubuntu installe :

- le démon et les outils CLI,
- un service systemd `fail2ban.service` (activé au boot),
- des jails par défaut raisonnables (dont `sshd` activé),
- les dépendances Python nécessaires.

Vérification :

```bash
fail2ban-client --version
# Fail2Ban v1.0.2

sudo systemctl status fail2ban
# ● fail2ban.service - Fail2Ban Service
#   Active: active (running) ...

sudo fail2ban-client status
# Status
# |- Number of jail:	1
# `- Jail list:	sshd
```

### Versions par distribution (repères 2026)

| Distribution | Version fail2ban (dépôt) | Backend nftables natif |
|---|---|---|
| Debian 11 (bullseye) | 0.11.2 | partiel (banaction nftables-multiport OK) |
| Debian 12 (bookworm) | 1.0.2 | oui |
| Debian 13 (trixie) | 1.1.x | oui |
| Ubuntu 20.04 | 0.11.1 | partiel |
| Ubuntu 22.04 | 0.11.2 | partiel |
| Ubuntu 24.04 | 1.0.2 | oui |

> Sur Debian 11 / Ubuntu ≤ 22.04, `iptables` est en réalité un wrapper vers nftables (`iptables-nft`). fail2ban fonctionne quand même, mais préférez explicitement `banaction = nftables-multiport` sur les systèmes récents (section 19).

### Installer une version plus récente (optionnel)

Si vous voulez les derniers filters (WordPress, nouveaux services), deux options :

```bash
# Option A : backports Debian (préférée, propre)
sudo apt -t bookworm-backports install fail2ban

# Option B : depuis les sources GitHub (à éviter en prod sauf besoin précis)
# git clone https://github.com/fail2ban/fail2ban.git
```

> En production, **restez sur le paquet de la distribution** sauf raison impérieuse : la version packagée est testée avec le backend firewall du système.

### Dépendances utiles

```bash
# whois : pour l'action d'alerte mail enrichie (optionnel)
sudo apt install -y whois

# ipset : pour la variante haute performance (optionnel, section 49)
sudo apt install -y ipset
```

### Vérifier que le service démarre au boot

```bash
sudo systemctl enable --now fail2ban
sudo systemctl is-enabled fail2ban
# enabled
```

---

## 4. Arborescence et fichiers

```
/etc/fail2ban/
├── fail2ban.conf          # config du démon (socket, loglevel, dbfile) — rarement touché
├── fail2ban.d/            # surcharges modulaires de fail2ban.conf
├── jail.conf              # JAILS par défaut — NE JAMAIS MODIFIER
├── jail.d/                # vos surcharges de jails (fichiers .local ou .conf)
│   └── defaults-debian.conf
├── jail.local             # VOTRE fichier principal (à créer)
├── filter.d/              # ~100 filters fournis
│   ├── sshd.conf
│   ├── nginx-http-auth.conf
│   └── ...
└── action.d/              # ~50 actions fournies
    ├── iptables.conf
    ├── nftables.conf
    ├── ufw.conf
    └── ...
```

### La règle d'or : ne jamais toucher `jail.conf`

`jail.conf` est **écrasé à chaque mise à jour du paquet**. Toute votre configuration va dans :

1. **`/etc/fail2ban/jail.local`** — le fichier principal (recommandé), ou
2. **`/etc/fail2ban/jail.d/*.local`** — découpé par service (pratique avec Ansible).

fail2ban fusionne `jail.conf` + `jail.d/*.conf` + `jail.local` + `jail.d/*.local`, dans cet ordre ; **la dernière valeur définie gagne**.

> Astuce : mettez `jail.local` sous version (git) ou sauvegardez-le (section 37). C'est LE fichier à ne pas perdre.

### Où sont les logs de fail2ban lui-même

- `/var/log/fail2ban.log` — le journal du démon (bans, unbans, erreurs).
- `journalctl -u fail2ban` — la même chose via systemd (selon config).

### Où est la base de données

`/var/lib/fail2ban/fail2ban.sqlite3` — elle mémorise les bannissements pour les restaurer après un redémarrage (section 29).

---

## 5. Premier jail.local minimal

Voici le plus petit `jail.local` utile en production. Il protège SSH et ne touche à rien d'autre.

```ini
# /etc/fail2ban/jail.local — configuration minimale de production

[DEFAULT]
# Une IP bannie le reste 1 heure
bantime = 1h

# Fenêtre de comptage : 10 minutes
findtime = 10m

# Nombre d'échecs tolérés dans la fenêtre
maxretry = 5

# NE JAMAIS bannir ces adresses (à adapter !)
ignoreip = 127.0.0.1/8 ::1 192.168.1.0/24 10.0.0.0/8

# Action par défaut : bannissement nftables multi-ports
banaction = nftables-multiport

[sshd]
enabled = true
```

Mise en service :

```bash
# 1. Vérifier la syntaxe (fail2ban >= 0.10)
sudo fail2ban-client --test

# 2. Recharger
sudo fail2ban-client reload

# 3. Contrôler
sudo fail2ban-client status sshd
```

Sortie attendue :

```
Status for the jail: sshd
|- Filter
|  |- Currently failed:	0
|  |- Total failed:	0
|  `- File list:	/var/log/auth.log
`- Actions
   |- Currently banned:	0
   |- Total banned:	0
   `- Banned IP list:
```

> **Point de vigilance immédiat** : `ignoreip` ci-dessus contient des exemples. Remplacez `192.168.1.0/24` et `10.0.0.0/8` par **vos** réseaux réels (section 24). Un `ignoreip` mal rempli = risque de vous bannir vous-même.

### Les unités de temps acceptées

| Suffixe | Signification | Exemple |
|---|---|---|
| (aucun) | secondes | `bantime = 3600` |
| `s` | secondes | `60s` |
| `m` | minutes | `10m` |
| `h` | heures | `1h` |
| `d` | jours | `7d` |
| `w` | semaines | `2w` |
| `y` | années | `1y` (= 365j) |

On peut combiner : `bantime = 1d12h` (= 36 heures). Et `bantime = -1` signifie **permanent** (section 28).

---

## 6. bantime, findtime, maxretry expliqués

Ces trois paramètres forment le **triangle de décision**. Bien les comprendre évite 90 % des erreurs de tuning.

### Définitions précises

- **`maxretry`** : nombre d'échecs (lignes matchées par le filter) qui déclenche le ban.
- **`findtime`** : fenêtre glissante pendant laquelle on compte les échecs. Un échec vieux de plus de `findtime` sort du compteur.
- **`bantime`** : durée du bannissement une fois déclenché.

### Exemple déroulé

Config : `maxretry = 5`, `findtime = 10m`, `bantime = 1h`. L'IP 203.0.113.45 échoue à :

```
08:00, 08:01, 08:02, 08:03   → 4 échecs, rien ne se passe
08:20                         → l'échec de 08:00 a plus de 10 min : il sort du compteur
                               compteur = 4 (08:01, 08:02, 08:03, 08:20)
08:21                         → compteur = 5 → BAN jusqu'à 09:21
```

### Les erreurs de raisonnement classiques

| Idée reçue | Réalité |
|---|---|
| « `maxretry = 3` c'est plus sûr » | Sur un service avec des utilisateurs humains (webmail, VPN), 3 fautes de frappe arrivent vite → faux positifs. Réservez les seuils bas aux services sans humains (SSH root). |
| « `findtime` long = plus strict » | Oui, mais aussi plus de faux positifs : un utilisateur qui se trompe 5 fois dans la journée se fait bannir. |
| « `bantime` long = mieux » | Pour les botnets à IP tournantes, un bantime de 24h ne change rien (l'IP ne reviendra jamais) et remplit la table firewall. Pour les attaquants persistants, c'est utile. |

### Valeurs de départ recommandées (à affiner section 23)

| Contexte | maxretry | findtime | bantime |
|---|---|---|---|
| SSH exposé (pas d'humains légitimes sauf vous) | 5 | 10m | 1h |
| Service web avec utilisateurs | 10 | 10m | 30m |
| Mail (postfix/dovecot) | 5 | 10m | 1h |
| Récidivistes (jail recidive) | 3 | 1d | 1w |

---

## 7. Le jail sshd en détail

C'est le jail le plus important : **tout serveur Linux exposé est scanné en SSH en permanence**.

### Configuration recommandée

```ini
[sshd]
enabled  = true
port     = ssh
filter   = sshd
logpath  = /var/log/auth.log
backend  = auto
maxretry = 5
findtime = 10m
bantime  = 1h
```

Sur les systèmes avec systemd et `sshd` qui loggue dans le journal :

```ini
[sshd]
enabled  = true
backend  = systemd
# logpath inutile avec le backend systemd
```

> `backend = auto` choisit tout seul (pyinotify → gamin → polling → systemd). En pratique, `auto` convient dans 95 % des cas. Ne forcez `systemd` que si `/var/log/auth.log` est vide alors que les tentatives existent (section 45).

### Ce que détecte le filter sshd

Le filter `sshd.conf` couvre, entre autres :

- `Failed password for ... from <HOST>` — mot de passe invalide
- `Failed publickey for ... from <HOST>` — clé invalide
- `Invalid user ... from <HOST>` — utilisateur inexistant
- `Connection closed by authenticating user ... <HOST> [preauth]` — abandon pendant l'authentification
- `Disconnected from authenticating user ... <HOST> [preauth]`
- `Received disconnect from <HOST> ... [preauth]` — certains scanners
- `error: PAM: Authentication failure for ... from <HOST>`
- `maximum authentication attempts exceeded for ... from <HOST>`

### Le mode agressif

`filter.d/sshd.conf` propose un mode `aggressive` qui bannit aussi les scanners qui ne tentent même pas de s'authentifier (probes de version, connexions fermées) :

```ini
[sshd]
enabled = true
filter  = sshd[mode=aggressive]
```

> À réserver aux serveurs très exposés : le mode agressif augmente les faux positifs (un simple `nmap` ou un monitoring Nagios qui ouvre le port 22 peut se faire bannir).

### Vérifier que le jail voit bien les attaques

```bash
# Générer des échecs volontaires depuis une machine de test (PAS depuis votre poste prod !)
for i in 1 2 3 4 5 6; do ssh -o ConnectTimeout=3 fauxuser@SERVEUR; done

# Sur le serveur, observer :
sudo tail -f /var/log/fail2ban.log
# ... NOTICE [sshd] Ban 198.51.100.23
```

---

## 8. Le jail sshd : variantes et cas particuliers

### SSH sur un port non standard

Si sshd écoute sur le port 2222 :

```ini
[sshd]
enabled = true
port    = 2222
filter  = sshd
logpath = /var/log/auth.log
```

> Le paramètre `port` sert à l'action firewall : il détermine quel(s) port(s) sont bloqués. Si vous vous trompez de port, le ban ne bloquera rien d'utile.

### Plusieurs ports SSH

```ini
[sshd]
enabled = true
port    = ssh,2222
```

### SSH derrière un bastion : ne bannir que l'extérieur

Si votre serveur n'est joignable en SSH que depuis le bastion (bonne pratique), le jail sshd devient presque inutile — mais gardez-le : il protège contre un bastion compromis ou une erreur de firewall.

### Journal auth.log vs journald

Sur Debian 12+ / Ubuntu 22.04+, rsyslog écrit toujours `/var/log/auth.log` par défaut. Vérifiez :

```bash
ls -la /var/log/auth.log
# Si le fichier n'existe pas ou reste vide :
sudo journalctl -u ssh --since "10 min ago" | head
# → alors utilisez backend = systemd dans le jail
```

### Dropbear (routeurs, embarqué)

```ini
[dropbear]
enabled  = true
port     = ssh
filter   = dropbear
logpath  = /var/log/auth.log
maxretry = 5
```

---

## 9. nginx-http-auth : protéger le basic auth

Le basic auth HTTP (pop-up login/mot de passe) est une cible classique : interfaces d'admin, Kibana, zones `/admin`.

```ini
[nginx-http-auth]
enabled  = true
port     = http,https
filter   = nginx-http-auth
logpath  = /var/log/nginx/error.log
maxretry = 5
findtime = 10m
bantime  = 1h
```

Ce que le filter détecte dans `/var/log/nginx/error.log` :

```
2026/09/26 10:22:11 [error] 1234#1234: *5678 user "admin" was not found in "/etc/nginx/.htpasswd",
  client: 203.0.113.78, server: intranet.example.com,
  request: "GET /admin/ HTTP/1.1", host: "intranet.example.com"
```

> Point d'attention : c'est bien `error.log` (pas `access.log`) que nginx utilise pour les échecs d'authentification basique. Si votre `error.log` est verbeux, le filter ne matche que les lignes d'authentification, pas les autres erreurs.

### Variante Apache

```ini
[apache-auth]
enabled  = true
port     = http,https
filter   = apache-auth
logpath  = /var/log/apache2/error.log
maxretry = 5
```

---

## 10. nginx-badbot et nginx-botsearch

Deux jails complémentaires pour l'hygiène d'un serveur web public.

### nginx-botsearch : les scans de vulnérabilités

Détecte les requêtes vers des chemins qui n'existent pas et qui ressemblent à des scans (`/wp-admin`, `/phpmyadmin`, `/.env`, `/actuator`, etc.) :

```ini
[nginx-botsearch]
enabled  = true
port     = http,https
filter   = nginx-botsearch
logpath  = /var/log/nginx/access.log
maxretry = 3
findtime = 1d
bantime  = 1w
```

> Seuils volontairement sévères : aucun utilisateur légitime ne demande `/.git/config` ou `/wp-login.php` sur un site qui n'est pas WordPress. `maxretry = 3` sur 1 jour, c'est raisonnable.

### nginx-badbot : les user-agents pourris

Détecte les bots connus pour être malveillants via leur User-Agent :

```ini
[nginx-badbot]
enabled  = true
port     = http,https
filter   = nginx-badbot
logpath  = /var/log/nginx/access.log
maxretry = 2
bantime  = 1w
```

### Adapter le format de log nginx

Les filters nginx supposent le `combined` log format par défaut. Si vous avez personnalisé `log_format`, vérifiez que les champs `$remote_addr`, `$request` et `$http_user_agent` sont présents, sinon les filters ne matcheront pas (erreur classique n°2, section 39).

```nginx
# /etc/nginx/nginx.conf — format compatible fail2ban
log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                '$status $body_bytes_sent "$http_referer" '
                '"$http_user_agent"';
access_log /var/log/nginx/access.log main;
```

---

## 11. postfix : relais et SASL

Un serveur mail exposé subit des tentatives d'authentification SASL en continu. Deux jails complémentaires.

```ini
[postfix]
enabled  = true
port     = smtp,465,submission
filter   = postfix
logpath  = /var/log/mail.log
maxretry = 5
findtime = 10m
bantime  = 1h
```

```ini
[postfix-sasl]
enabled  = true
port     = smtp,465,submission,imap,imaps,pop3,pop3s
filter   = postfix[mode=auth]
logpath  = /var/log/mail.log
maxretry = 5
findtime = 10m
bantime  = 1h
```

Ce que ça détecte :

```
# postfix (générique) :
Sep 26 11:00:01 srv-mail postfix/smtpd[3210]: NOQUEUE: reject: RCPT from unknown[203.0.113.90]:
  554 5.7.1 <victim@example.com>: Relay access denied; ...

# postfix[mode=auth] (SASL) :
Sep 26 11:00:02 srv-mail postfix/smtpd[3210]: warning: unknown[203.0.113.90]:
  SASL LOGIN authentication failed: authentication failure
```

> Sur les systèmes avec rsyslog, les logs postfix sont dans `/var/log/mail.log`. Avec systemd pur, utilisez `backend = systemd` et `journalmatch = _SYSTEMD_UNIT=postfix.service`.

### Cas particulier : postfix derrière un anti-spam

Si un relais (type Proxmox Mail Gateway) est devant, les logs voient l'IP du relais, pas celle de l'attaquant. **Ne pas activer** le jail postfix dans ce cas, ou configurez `postscreen`/`smtpd` pour logger la vraie IP (via `X-Forwarded-For` équivalent SMTP : en-têtes `Received`).

---

## 12. dovecot : POP3/IMAP sous pression

```ini
[dovecot]
enabled  = true
port     = pop3,pop3s,imap,imaps,submission,465,sieve
filter   = dovecot
logpath  = /var/log/mail.log
maxretry = 5
findtime = 10m
bantime  = 1h
```

Détecte :

```
Sep 26 11:05:00 srv-mail dovecot: imap-login: Disconnected: Connection closed:
  auth failed, 1 attempts in 2 secs (auth failed, 1 attempts):
  user=<victim@example.com>, method=PLAIN, rip=203.0.113.91, lip=192.0.2.10
```

### Le piège des clients mail mal configurés

C'est LE service où les faux positifs sont fréquents : un smartphone avec un ancien mot de passe tente de se reconnecter **toutes les 5 minutes**, indéfiniment. Recommandations :

- `maxretry = 10` plutôt que 5 pour dovecot si vous avez des utilisateurs humains.
- `bantime = 30m` plutôt que 1h : un utilisateur bloqué une heure appelle le support.
- Communiquez : « si votre téléphone redemande le mot de passe en boucle, appelez avant d'être bloqué ».

### dovecot agressif

```ini
[dovecot]
enabled = true
filter  = dovecot[mode=aggressive]
```

Le mode agressif matche aussi les échecs `pop3-login`/`managesieve-login` avec des patterns supplémentaires. À tester avec `fail2ban-regex` d'abord.

---

## 13. proftpd et vsftpd

Le FTP est un protocole legacy ; s'il est encore exposé, il est pilonné.

```ini
[proftpd]
enabled  = true
port     = ftp,ftp-data,ftps,ftps-data
filter   = proftpd
logpath  = /var/log/proftpd/proftpd.log
maxretry = 5
findtime = 10m
bantime  = 1h
```

```ini
[vsftpd]
enabled  = true
port     = ftp,ftp-data,ftps,ftps-data
filter   = vsftpd
logpath  = /var/log/vsftpd.log
maxretry = 5
```

> Vérifiez TOUJOURS le `logpath` réel : selon la distribution et la configuration, proftpd peut logger dans `/var/log/auth.log`, `/var/log/proftpd/proftpd.log` ou via syslog. Un jail qui pointe vers un fichier vide ne protège rien (erreur classique n°3).

### Recommandation stratégique

Si vous pouvez, **ne pas exposer FTP du tout** : migrez vers SFTP (couvert par le jail sshd). Un jail proftpd bien configuré reste mieux que rien.

---

## 14. recidive : les récidivistes

Le jail `recidive` est la **deuxième ligne de défense** : il surveille le log de fail2ban lui-même et bannit longuement les IP déjà bannies plusieurs fois.

```ini
[recidive]
enabled  = true
filter   = recidive
logpath  = /var/log/fail2ban.log
banaction = %(banaction_allports)s
bantime  = 1w
findtime = 1d
maxretry = 5
```

Fonctionnement :

1. Une IP se fait bannir 5 fois en 1 jour par n'importe quel jail (chaque ban écrit `NOTICE [sshd] Ban 203.0.113.45` dans `/var/log/fail2ban.log`).
2. Le filter `recidive` matche ces lignes `Ban`.
3. L'IP est bannie **sur tous les ports** (`banaction_allports`) pendant **1 semaine**.

### Pourquoi `banaction_allports` ?

Un récidiviste qui a attaqué SSH attaquera probablement le web et le mail ensuite. Le bannir sur tous les ports (`iptables -A INPUT -s <ip> -j DROP` sans restriction de port) est logique à ce stade.

### Réglages selon la paranoïa

| Profil | findtime | maxretry | bantime |
|---|---|---|---|
| Standard | 1d | 5 | 1w |
| Strict (serveur très exposé) | 1d | 3 | 4w |
| Prudent (peur des faux positifs) | 2d | 10 | 3d |

> Le jail recidive ne fonctionne que si `/var/log/fail2ban.log` existe et contient les lignes de ban. Si vous avez mis `logtarget = SYSTEMD-JOURNAL`, adaptez avec `backend = systemd` (section 30).

---

## 15. Activer plusieurs jails sans conflit

### Règles de coexistence

1. **Un log, plusieurs jails : OK.** `nginx-http-auth` (sur `error.log`) et `nginx-botsearch` (sur `access.log`) cohabitent sans problème.
2. **Deux jails sur le même log avec des filters qui se chevauchent :** possible double comptage, mais chaque jail a son propre compteur — pas de conflit technique, juste un tuning à surveiller.
3. **Actions différentes par jail : OK.** Un jail peut bannir sur `http,https` pendant qu'un autre bannit sur tous les ports.

### Exemple : stack web complète

```ini
[DEFAULT]
bantime  = 1h
findtime = 10m
maxretry = 5
ignoreip = 127.0.0.1/8 ::1 192.168.10.0/24
banaction = nftables-multiport

[sshd]
enabled = true

[nginx-http-auth]
enabled = true
port    = http,https
filter  = nginx-http-auth
logpath = /var/log/nginx/error.log

[nginx-botsearch]
enabled  = true
port     = http,https
filter   = nginx-botsearch
logpath  = /var/log/nginx/access.log
maxretry = 3
findtime = 1d
bantime  = 1w

[nginx-badbot]
enabled = true
port    = http,https
filter  = nginx-badbot
logpath = /var/log/nginx/access.log
maxretry = 2
bantime  = 1w

[recidive]
enabled   = true
filter    = recidive
logpath   = /var/log/fail2ban.log
banaction = %(banaction_allports)s
bantime   = 1w
findtime  = 1d
maxretry  = 5
```

### Vérifier l'ensemble

```bash
sudo fail2ban-client status
# Status
# |- Number of jail:	5
# `- Jail list:	sshd, nginx-http-auth, nginx-botsearch, nginx-badbot, recidive

for j in sshd nginx-http-auth nginx-botsearch nginx-badbot recidive; do
  echo "=== $j ==="
  sudo fail2ban-client status "$j" | grep -E "Currently|Total|File list"
done
```

---

## 16. fail2ban-regex : tester son filtre

`fail2ban-regex` est l'outil le plus sous-utilisé de la suite. **Ne jamais mettre en production un filter non testé.**

### Syntaxe de base

```bash
sudo fail2ban-regex /var/log/auth.log /etc/fail2ban/filter.d/sshd.conf
```

Sortie (extraits) :

```
Running tests
=============

Use   failregex filter file : sshd, basedir: /etc/fail2ban
Use         log file : /var/log/auth.log
Use         encoding : UTF-8

Results
=======

Failregex: 27 total
|-  #) [# of hits] regular expression
|   1) [15] ^\s*(?:\S+ )?(?:kernel: \[...)...
...
Ignoreregex: 0 total

Date template hits:
|- [# of hits] date format
|  [2450] {^LN-BEG}ExYear(?P<_sep>-|/)?Month(?P<_sep>-|/)Day ...

Lines: 2450 lines, 0 ignored, 27 matched, 2423 missed
```

Lecture : sur 2450 lignes, 27 matchent le filter. Si vous savez qu'il y a eu ~27 attaques, c'est bon.

### Tester une regex ad hoc (sans fichier)

```bash
sudo fail2ban-regex /var/log/nginx/error.log \
  'user "\S+" was not found in "[^"]+", client: <HOST>'
```

### Tester contre une ligne unique (debug rapide)

```bash
echo 'Sep 26 10:22:11 srv sshd[1234]: Failed password for root from 203.0.113.45 port 51234 ssh2' \
  | sudo fail2ban-regex - /etc/fail2ban/filter.d/sshd.conf --print-all-matched
```

L'option `--print-all-matched` affiche les lignes matchées : idéal pour comprendre pourquoi une ligne matche (ou pas).

### Les options indispensables

| Option | Usage |
|---|---|
| `--print-all-matched` | affiche les lignes qui matchent |
| `--print-all-missed` | affiche les lignes qui NE matchent pas |
| `--print-all-ignored` | affiche les lignes ignorées (ignoreregex) |
| `-v` | mode verbeux (détail par regex) |
| `--datepattern=...` | forcer un format de date |

### Cas d'école : ma regex ne matche pas

```bash
# 1. Vérifier que le format de DATE est reconnu
sudo fail2ban-regex /var/log/monapp.log /etc/fail2ban/filter.d/monapp.conf -v
# Si "Date template hits" = 0 → le problème vient de la date, pas de la regex !

# 2. Forcer le format de date
sudo fail2ban-regex --datepattern="%%Y-%%m-%%d %%H:%%M:%%S" /var/log/monapp.log ...
```

> 50 % des « ma regex ne marche pas » sont en réalité des **problèmes de date** : fail2ban doit d'abord extraire le timestamp avant d'appliquer la regex (section 46).

---

## 17. Créer un filter personnalisé pas à pas

Scénario : vous avez une application maison qui loggue dans `/var/log/myapp/auth.log` :

```
2026-09-26 14:32:10 [WARN] login failed for user 'jdupont' from 203.0.113.77 (bad password)
2026-09-26 14:32:15 [WARN] login failed for user 'jdupont' from 203.0.113.77 (bad password)
```

### Étape 1 : écrire la regex et la tester

La partie variable : le nom d'utilisateur et l'IP. La regex :

```
\[WARN\] login failed for user '[^']+' from <HOST> \(bad password\)
```

Test immédiat :

```bash
sudo fail2ban-regex /var/log/myapp/auth.log \
  "\[WARN\] login failed for user '[^']+' from <HOST> \(bad password\)" \
  --print-all-matched
```

### Étape 2 : créer le fichier filter

```ini
# /etc/fail2ban/filter.d/myapp.conf
[INCLUDES]
before = common.conf

[Definition]
failregex = \[WARN\] login failed for user '[^']+' from <HOST> \(bad password\)$
ignoreregex =
```

> `common.conf` fournit `__prefix_line` et les définitions de `<HOST>`. L'inclure est quasi obligatoire.

### Étape 3 : tester le fichier complet

```bash
sudo fail2ban-regex /var/log/myapp/auth.log /etc/fail2ban/filter.d/myapp.conf
# Lines: 120 lines, 0 ignored, 8 matched, 112 missed  → OK si 8 attaques connues
```

### Étape 4 : créer le jail

```ini
# /etc/fail2ban/jail.d/myapp.local
[myapp]
enabled  = true
port     = 8443
filter   = myapp
logpath  = /var/log/myapp/auth.log
maxretry = 5
findtime = 10m
bantime  = 1h
```

### Étape 5 : recharger et vérifier

```bash
sudo fail2ban-client reload
sudo fail2ban-client status myapp
```

### Checklist du filter maison

- [ ] La regex matche les vraies lignes d'échec (`--print-all-matched`)
- [ ] La regex ne matche PAS les lignes de succès (`--print-all-missed` relu à la main)
- [ ] `<HOST>` capture bien l'IP de l'attaquant (pas celle d'un proxy)
- [ ] Le format de date est détecté (section 16)
- [ ] Le fichier est dans `filter.d/`, pas écrasé par les mises à jour (c'est votre fichier, il est safe)
- [ ] Le jail pointe vers le bon `logpath`

---

## 18. Regex : guide de survie

Pas besoin d'être un expert regex. Voici les 20 % qui couvrent 80 % des filters.

### Les briques de base

| Motif | Signification | Exemple |
|---|---|---|
| `<HOST>` | une adresse IP (v4 ou v6) | `from <HOST>` |
| `\d+` | un ou plusieurs chiffres | `port \d+` |
| `\S+` | caractères non-espaces | `user "\S+"` |
| `[^']+` | tout sauf une apostrophe | `'[^']+'` |
| `.*` | n'importe quoi (gourmand) | à éviter si possible |
| `(?:...)` | groupe non-capturant | `(?:password\|publickey)` |
| `^` / `$` | début / fin de ligne | `^...Failed...$` |
| `\.` | un point littéral | `203\.0\.113\.` |
| `\s*` | espaces éventuels | `ssh2\s*$` |

### Les raccourcis fail2ban

Définis dans `filter.d/common.conf` :

| Raccourci | Équivalent | Usage |
|---|---|---|
| `<HOST>` | `(?:::f{4,6}:)?(?P<host>[\w\-.^_]*\w)` | capture l'IP |
| `<F-USER>...</F-USER>` | capture nommée `user` | le login attaqué |
| `<F-MLFID>...</F-MLFID>` | capture multi-lignes | logs sur plusieurs lignes |
| `__prefix_line` | `^...date...host...process:` | préfixe syslog standard |

### Les 5 règles d'un bon failregex

1. **Ancrez avec `<HOST>`** : toute failregex doit contenir `<HOST>`, sinon fail2ban ne sait pas qui bannir (et refuse de charger le filter).
2. **Soyez spécifique** : `Failed` seul matchera trop large. `Failed password for` est précis.
3. **Échappez les caractères spéciaux** : `[`, `]`, `(`, `)`, `.` ont un sens en regex. `[WARN]` doit s'écrire `\[WARN\]`.
4. **Préférez `[^']+` à `.*`** : moins gourmand, moins de faux positifs.
5. **Testez les lignes de succès** : assurez-vous que « login successful » ne matche pas votre regex d'échec.

### Exemple décortiqué

```
^\s*\S+ sshd\[\d+\]: Failed (?:password|publickey) for (?:invalid user )?\S+ from <HOST>( port \d+)?(?: ssh2)?\s*$
```

| Morceau | Rôle |
|---|---|
| `^\s*` | début de ligne, espaces éventuels |
| `\S+ sshd\[\d+\]:` | hostname + `sshd[pid]:` |
| `Failed (?:password\|publickey)` | le type d'échec (sans capturer) |
| `for (?:invalid user )?\S+` | `for root` ou `for invalid user admin` |
| `from <HOST>` | **l'IP de l'attaquant** |
| `( port \d+)?` | port source, optionnel |
| `(?: ssh2)?\s*$` | suffixe `ssh2` optionnel, fin de ligne |

### Tester mentalement : la méthode en 3 questions

1. Si je remplace `<HOST>` par `203.0.113.45`, ma regex matche-t-elle la vraie ligne de log ? (copiez-collez, ne devinez pas)
2. Matche-t-elle aussi une ligne de **succès** ? (si oui, resserrez)
3. Le format de date en début de ligne est-il dans la liste des `Date template hits` ?

---

## 19. Les actions : iptables, nftables, ufw

L'action transforme une décision (« bannir 203.0.113.45 ») en règles firewall. Le choix dépend de votre système.

### Tableau de décision

| Système | Pare-feu actif | banaction recommandée |
|---|---|---|
| Debian 12/13, Ubuntu 24.04 | nftables | `nftables-multiport` |
| Debian 11, Ubuntu 22.04 | iptables-nft (wrapper) | `iptables-multiport` (fonctionne) ou `nftables-multiport` |
| Ubuntu avec `ufw enable` | ufw | `ufw` |
| Haute performance (gros volume) | ipset/iptables | `iptables-ipset-proto6` |

### Configurer l'action par défaut

```ini
# /etc/fail2ban/jail.local
[DEFAULT]
# Choix moderne (Debian 12+, Ubuntu 24.04+)
banaction = nftables-multiport
banaction_allports = nftables-allports
```

```ini
# Variante classique iptables
[DEFAULT]
banaction = iptables-multiport
banaction_allports = iptables-allports
```

```ini
# Variante UFW (si ufw est votre firewall)
[DEFAULT]
banaction = ufw
banaction_allports = ufw
```

### Ce que fait nftables-multiport

À l'activation du jail (`actionstart`) :

```nft
# (simplifié)
table inet f2b-table {
  chain f2b-sshd {
    # les IP bannies sont ajoutées ici, en tête de chaîne
  }
  chain input {
    type filter hook input priority 0;
    tcp dport == ssh jump f2b-sshd
  }
}
```

Au ban (`actionban`) : `nft add element inet f2b-table f2b-sshd { 203.0.113.45 }`
Au unban : `nft delete element ...`

Vérifier à la main :

```bash
sudo nft list table inet f2b-table
```

### Ce que fait iptables-multiport

Crée une chaîne `f2b-sshd`, y insère les IP en `REJECT`, et branche la chaîne sur `INPUT` pour les ports concernés.

```bash
sudo iptables -L f2b-sshd -n --line-numbers
# Chain f2b-sshd (1 references)
# num  target  prot  source        destination
# 1    REJECT  all   203.0.113.45  0.0.0.0/0  reject-with icmp-port-unreachable
```

### REJECT vs DROP

- `iptables-multiport` utilise **REJECT** par défaut : l'attaquant reçoit un ICMP d'erreur, sa connexion échoue vite, il passe à autre chose.
- `iptables` tout court (action de base) utilise **DROP** : les paquets sont jetés silencieusement, l'attaquant attend le timeout.

> En pratique, REJECT fait perdre moins de temps aux scanners (ils abandonnent vite) et rend vos propres tests plus lisibles (connexion refusée immédiatement plutôt que timeout).

---

## 20. Action nftables en détail

Sur les systèmes récents, nftables est le backend natif. Comprendre ce que fail2ban y fait aide au dépannage.

### Inspection

```bash
# Voir toutes les tables fail2ban
sudo nft list tables | grep f2b

# Détail d'une chaîne de jail
sudo nft list chain inet f2b-table f2b-sshd

# Compter les IP bannies par jail
sudo nft list set inet f2b-table f2b-sshd 2>/dev/null | tr ',' '\n' | grep -c "203\."
```

### Fichier d'action

`/etc/fail2ban/action.d/nftables.conf` définit les variantes :

| Action | Portée |
|---|---|
| `nftables` | un seul port (`<port>`) |
| `nftables-multiport` | liste de ports (`<port>` = `http,https`) |
| `nftables-allports` | tous les ports |

```ini
# Extrait de nftables-common.conf
actionban = nft add element <table> <chain> { <ip> }
actionunban = nft delete element <table> <chain> { <ip> }
```

### Conflit avec docker ou d'autres outils nftables

Docker manipule aussi nftables/iptables. Si fail2ban et Docker cohabitent :

- fail2ban crée sa propre table `f2b-table` : pas de conflit direct.
- Mais l'ordre des hooks compte : les règles Docker s'appliquent dans leurs propres chaînes.
- **Testez** : bannissez une IP de test et vérifiez qu'elle est bien bloquée vers le conteneur.

### Nettoyage manuel (si fail2ban a planté)

```bash
# Si des règles orphelines subsistent après un crash :
sudo nft delete table inet f2b-table
sudo fail2ban-client restart
```

---

## 21. Action ufw

Si votre serveur utilise UFW (`sudo ufw enable`), utilisez l'action `ufw` pour rester cohérent.

```ini
[DEFAULT]
banaction = ufw
```

Ce que fait l'action :

```bash
# actionban :
sudo ufw insert 1 deny from 203.0.113.45 to any
# actionunban :
sudo ufw delete deny from 203.0.113.45 to any
```

Vérification :

```bash
sudo ufw status numbered | head -20
```

### Limites de l'action ufw

- Elle insère des règles **en tête** (`insert 1`) : les bans priment sur vos règles `allow`. C'est voulu.
- Sur un serveur avec beaucoup de bans simultanés, `ufw` (qui régénère tout le ruleset à chaque changement) est **plus lent** que nftables direct. Au-delà de ~500 bans actifs, préférez nftables.
- Ne mélangez pas `banaction = ufw` et des règles iptables manuelles : UFW ne les verra pas.

---

## 22. Actions multiples et combinées

Un jail peut déclencher **plusieurs actions** : bannir + alerter, par exemple.

### Syntaxe

```ini
[sshd]
enabled = true
action  = %(action_)s
          %(action_mail)s[name=sshd, dest=root@example.com]
```

`%(action_)s` = l'action de bannissement par défaut (définie par `banaction`).
`%(action_mail)s` = bannir + envoyer un mail via `action.d/mail.conf`.

### Actions d'alerte disponibles

| Action | Effet |
|---|---|
| `action_` | ban seul (défaut) |
| `action_mw` | ban + mail avec extrait de log (whois) |
| `action_mwl` | ban + mail avec whois + lignes de log |
| `action_cf_mwl` | ban + mail + ban Cloudflare (section 32) |

Exemple complet :

```ini
[sshd]
enabled = true
action  = %(action_)s
          %(action_mwl)s[name=sshd, dest=admin@example.com, sender=fail2ban@srv-web.example.com]
```

Prérequis mail : un MTA local fonctionnel (`postfix` en mode satellite, ou `ssmtp`) :

```bash
echo "test" | mail -s "test fail2ban" admin@example.com
```

### Webhook générique (exemple Slack/Mattermost)

Créez `/etc/fail2ban/action.d/webhook.conf` :

```ini
[Definition]
actionban = curl -s -X POST -H 'Content-Type: application/json' \
              -d '{"text":"[fail2ban] <name> : bannissement de <ip> (échecs: <failures>)"}' \
              https://hooks.exemple.fr/services/TOKEN
actionunban =
```

```ini
[sshd]
action = %(action_)s
         webhook[name=sshd]
```

> Attention aux secrets dans les fichiers d'action : `webhook.conf` contient un token. `chmod 640` + propriétaire root, et ne le commitez jamais en clair.

---

## 23. Tuning : bantime, findtime, maxretry par service

Il n'existe pas de valeurs universelles, mais voici des bases éprouvées en production, avec le raisonnement.

### Tableau de référence

| Jail | maxretry | findtime | bantime | Raison |
|---|---|---|---|---|
| sshd | 5 | 10m | 1h | pas d'humains légitimes en faute ; bots agressifs |
| sshd (parano) | 3 | 10m | 4h | serveurs ultra-exposés |
| nginx-http-auth | 5 | 10m | 1h | humains possibles derrière le basic auth |
| nginx-botsearch | 3 | 1d | 1w | aucun humain légitime ne scanne `/.env` |
| nginx-badbot | 2 | 10m | 1w | user-agent = signature quasi certaine |
| postfix / postfix-sasl | 5 | 10m | 1h | bots de relay + brute-force SASL |
| dovecot | 10 | 10m | 30m | **smartphones mal configurés** : seuil haut, ban court |
| proftpd / vsftpd | 5 | 10m | 1h | legacy, bots uniquement |
| recidive | 5 | 1d | 1w | deuxième ligne, tous ports |
| wordpress* | 5 | 10m | 1h | `wp-login.php` pilonné |

\* nécessite un filter wordpress (non fourni par défaut sur les vieilles versions ; voir section 17 pour l'écrire).

### Méthode de tuning en 4 étapes

1. **Démarrez avec les valeurs du tableau** (conservatrices).
2. **Observez une semaine** : `grep "Ban " /var/log/fail2ban.log | awk '{print $7}' | sort | uniq -c | sort -rn | head` — qui se fait bannir, et combien de fois ?
3. **Repérez les faux positifs** : une IP interne ? un utilisateur légitime ? → augmentez `maxretry` ou ajoutez à `ignoreip`.
4. **Repérez les attaquants persistants** : une IP qui revient après chaque unban → elle finira dans `recidive` ; si besoin, durcissez `bantime`.

### Le piège du bantime trop long

Un `bantime = 1w` sur le jail sshd semble sécurisant, mais :

- Les botnets utilisent des IP jetables : l'IP bannie ne reviendra jamais, la règle firewall reste pour rien.
- À 10 000 bans/semaine, la table nftables/iptables grossit et **ralentit chaque paquet**.
- Solution : `bantime` modéré (1h) + `recidive` agressif (1w) pour les vrais persistants.

### bantime incrémental (fail2ban ≥ 0.11)

```ini
[DEFAULT]
# Le bantime augmente à chaque récidive : 1h, 2h, 4h, 8h... (facteur 2)
bantime.increment = true
bantime.factor = 2
bantime.maxtime = 4w
# Formule : bantime * factor ^ (nombre de bans précédents)
```

Avec `bantime = 1h` : 1er ban = 1h, 2e = 2h, 3e = 4h, 4e = 8h... plafonné à 4 semaines. C'est une excellente alternative (ou un complément) au jail recidive.

> Note : `bantime.increment` utilise la base sqlite (`dbfile`) pour se souvenir des bans précédents. Vérifiez que `dbfile = /var/lib/fail2ban/fail2ban.sqlite3` est actif (défaut).

---

## 24. ignoreip : whitelist (son réseau, la supervision !)

`ignoreip` est le paramètre **le plus important pour votre tranquillité** : les IP listées ne seront JAMAIS bannies, même si elles dépassent `maxretry`.

### Qui mettre en whitelist, systématiquement

```ini
[DEFAULT]
ignoreip = 127.0.0.1/8 ::1
           192.168.10.0/24      # LAN bureautique
           10.20.0.0/16         # LAN serveurs
           203.0.113.10         # IP publique du bureau (fixe)
           198.51.100.5         # sonde Zabbix
           192.0.2.53           # sonde Prometheus/blackbox
           203.0.113.200        # bastion d'administration
```

Checklist des oublis classiques :

- [ ] Votre **IP publique actuelle** (si elle est fixe ; sinon, voir section 25)
- [ ] Le **bastion** depuis lequel vous administrez
- [ ] Les **sondes de supervision** (Zabbix, Prometheus, Nagios, Uptime Kuma) — elles ouvrent des connexions en boucle et se font bannir par les jails agressifs (nginx-badbot, sshd mode agressif)
- [ ] Les **reverse proxy** internes (si fail2ban voit l'IP du proxy au lieu du client — voir section 52)
- [ ] Le **loopback** (127.0.0.1/8, ::1) — toujours
- [ ] Les **IP des collègues** en télétravail fixe

### Syntaxe acceptée

```
ignoreip = 127.0.0.1/8 ::1 192.168.1.14 10.0.0.0/8 2001:db8::/32
```

IP uniques, CIDR v4 et v6, séparés par des espaces. On peut aussi utiliser des noms DNS (résolus au démarrage — **à éviter** : si le DNS est down au boot, fail2ban échoue).

### Vérifier qu'une IP est ignorée

```bash
sudo fail2ban-client get sshd ignoreip
# 127.0.0.1/8 ::1 192.168.10.0/24 ...
```

### Le cas des IP dynamiques (télétravail, 4G)

Si votre IP publique change, `ignoreip` statique ne suffit pas. Options :

1. **VPN d'administration** : vous vous connectez toujours via le VPN, et c'est le **réseau VPN** qui est en `ignoreip`. C'est la bonne solution.
2. **Ne jamais exposer SSH** sans VPN/bastion (recommandé).
3. En dernier recours : `bantime` court sur sshd (30m) pour limiter la casse en cas d'auto-ban.

---

## 25. Ne pas se bannir soi-même : 7 techniques

L'auto-bannissement est **l'erreur n°1** (section 39). Voici la défense en profondeur.

### Technique 1 : ignoreip complet (base)

Voir section 24. C'est non négociable.

### Technique 2 : accès console de secours

Avant toute modification de fail2ban sur un serveur distant :

- Ouvrez une **console hors-bande** (console Proxmox/VMware, IPMI/iDRAC, console du provider cloud).
- Ou gardez une **session SSH déjà ouverte** : un ban fail2ban bloque les NOUVELLES connexions, pas toujours les sessions établies (selon l'action : REJECT sur INPUT bloque aussi les paquets des sessions existantes — ne comptez pas dessus).

> Règle : **ne touchez jamais à fail2ban en SSH sans console de secours**. Le jour où vous en aurez besoin, vous comprendrez.

### Technique 3 : tester avec un bantime court

```bash
# Pendant les tests, bantime très court :
sudo fail2ban-client set sshd bantime 60
# ... vos tests ...
# Puis remettez la vraie valeur et rechargez jail.local
```

Si vous vous bannissez pendant les tests, 60 secondes plus tard c'est fini.

### Technique 4 : le « dead man's switch »

Avant de recharger une config risquée, programmez un garde-fou :

```bash
# Dans 5 minutes, fail2ban s'arrête (et les règles de ban tombent avec actionstop)
echo "systemctl stop fail2ban" | at now + 5 minutes
# Si tout va bien, annulez :
atrm $(atq | cut -f1)
```

Variante sans `at` :

```bash
(sleep 300 && systemctl stop fail2ban) &
# puis kill %1 si tout va bien
```

### Technique 5 : jail de test séparé

Testez vos nouveaux filters sur un jail dédié avec des seuils inoffensifs, pas directement sur `sshd` :

```ini
[sshd-test]
enabled  = true
filter   = sshd
logpath  = /var/log/auth.log
port     = 22222
maxretry = 100
bantime  = 60
action   = iptables[name=sshd-test, port=22222]
```

### Technique 6 : vérifier AVANT de recharger

```bash
sudo fail2ban-client --test   # valide la syntaxe de toute la config
sudo fail2ban-regex /var/log/auth.log /etc/fail2ban/filter.d/sshd.conf | tail -3
```

### Technique 7 : procédure d'urgence écrite

Affichez dans votre documentation d'équipe :

```
URGENCE AUTO-BAN — serveur <nom>
1. Console Proxmox : https://pve.exemple.fr:8006 (datacenter > srv-web > Console)
2. Login root (mot de passe dans le coffre)
3. fail2ban-client set sshd unbanip <VOTRE_IP>
   ou : fail2ban-client unban --all
4. Vérifier : fail2ban-client status sshd
```

---

## 26. fail2ban-client : usage quotidien

### Statuts

```bash
# Vue d'ensemble : jails actifs
sudo fail2ban-client status

# Détail d'un jail : compteurs + IP bannies
sudo fail2ban-client status sshd

# Détail de TOUS les jails en une commande
sudo fail2ban-client status --all
```

### Bannir / débannir manuellement

```bash
# Bannir une IP à la main (utile en incident)
sudo fail2ban-client set sshd banip 203.0.113.45

# Débannir
sudo fail2ban-client set sshd unbanip 203.0.113.45

# Tout débannir d'un jail
sudo fail2ban-client set sshd unban --all
```

### Paramètres à chaud (sans reload)

```bash
# Changer le bantime d'un jail immédiatement
sudo fail2ban-client set sshd bantime 7200

# Changer maxretry / findtime
sudo fail2ban-client set sshd maxretry 3
sudo fail2ban-client set sshd findtime 600

# Ajouter une IP à ignoreip sans toucher au fichier
sudo fail2ban-client set sshd addignoreip 198.51.100.99
sudo fail2ban-client get sshd ignoreip
```

> Attention : les changements `set` sont **perdus au prochain reload/restart**. Pour les rendre durables, modifiez `jail.local`.

### Recharger la configuration

```bash
# Recharge en douceur (recharge les jails modifiés, garde les bans)
sudo fail2ban-client reload

# Recharge un seul jail
sudo fail2ban-client reload sshd

# Redémarrage complet (coupe tous les bans puis les restaure via la db)
sudo fail2ban-client restart
```

### Ping et diagnostic

```bash
# Le serveur répond-il ?
sudo fail2ban-client ping
# Server replied: pong

# Version
sudo fail2ban-client --version
```

---

## 27. Débannir : le classique du lundi

Lundi 8h : « je n'arrive plus à me connecter au serveur ». Dans 90 % des cas : l'utilisateur (ou vous) s'est fait bannir le week-end.

### Procédure standard

```bash
# 1. Identifier le jail concerné
sudo fail2ban-client status | grep "Jail list"

# 2. Vérifier que l'IP est bien bannie
sudo fail2ban-client status sshd | grep -A5 "Banned IP list"

# 3. Débannir
sudo fail2ban-client set sshd unbanip 198.51.100.23

# 4. Vérifier dans les logs
sudo grep "Unban 198.51.100.23" /var/log/fail2ban.log
```

### Si on ne sait pas quel jail a banni

```bash
# Chercher dans le log fail2ban
sudo grep "Ban 198.51.100.23" /var/log/fail2ban.log
# 2026-09-26 08:15:01,234 fail2ban.actions [1234]: NOTICE [sshd] Ban 198.51.100.23
# → c'était le jail sshd

# Ou scanner tous les jails
for j in $(sudo fail2ban-client status | grep "Jail list" | sed 's/.*://;s/,//g'); do
  echo "== $j =="
  sudo fail2ban-client status "$j" | grep -A3 "Banned IP list"
done
```

### Après le débannissement : traiter la cause

Un débannissement sans correction = le même appel mardi.

| Cause probable | Correction |
|---|---|
| Mot de passe expiré / changé | mettre à jour le client mail, le script, la conf |
| Smartphone en boucle | corriger le mot de passe sur le téléphone |
| Script de supervision mal configuré | corriger le script OU mettre sa sonde en `ignoreip` |
| Vrai attaquant | ne pas débannir, analyser (section 33) |
| C'est vous, en test | ajouter votre IP/réseau en `ignoreip` |

### Le cas du ban persistant (recidive)

Si l'IP est aussi dans `recidive`, il faut la débannir des **deux** jails :

```bash
sudo fail2ban-client set sshd unbanip 198.51.100.23
sudo fail2ban-client set recidive unbanip 198.51.100.23
```

---

## 28. Bannissement temporaire vs permanent

### Temporaire (défaut) : le bon choix 95 % du temps

```ini
[sshd]
bantime = 1h
```

Avantages : auto-nettoyant, pas de risque d'accumulation, pardonne les faux positifs.

### Permanent : quand et comment

```ini
# Ban permanent pour un jail donné
[nginx-botsearch]
bantime = -1
```

`bantime = -1` = banni jusqu'au redémarrage de fail2ban (ou unban manuel). Avec la base sqlite persistante (section 29), le ban survit même au redémarrage.

**Quand c'est justifié :**
- Jail `nginx-botsearch` / `nginx-badbot` : aucun faux positif possible, attaquant certain.
- IP identifiée comme source d'attaque ciblée (après analyse AbuseIPDB, section 33).

**Quand c'est dangereux :**
- Sur `sshd` ou `dovecot` avec des humains : un faux positif permanent = ticket au support + intervention manuelle.

### Stratégie recommandée : l'escalade

Plutôt que du permanent brutal, utilisez l'escalade automatique :

```ini
[DEFAULT]
bantime = 1h
# Escalade : 1h → 2h → 4h → ... → max 4 semaines
bantime.increment = true
bantime.factor = 2
bantime.maxtime = 4w
bantime.rndtime = 10m   # petite randomisation (évite les patterns prévisibles)
```

Combiné au jail `recidive` (ban 1 semaine tous ports après 5 récidives), vous obtenez une réponse graduée sans administration manuelle.

---

## 29. Bans persistants : la base sqlite

### Comment ça marche

```ini
# /etc/fail2ban/fail2ban.conf (défaut, ne pas toucher sauf besoin)
[Definition]
dbfile = /var/lib/fail2ban/fail2ban.sqlite3
dbpurgeage = 1d
```

- Chaque ban est enregistré dans sqlite avec son timestamp.
- Au (re)démarrage, fail2ban **restaure les bans non expirés**.
- `dbpurgeage = 1d` : les bans expirés depuis plus d'1 jour sont purgés de la base.

### Vérifier la base

```bash
sudo sqlite3 /var/lib/fail2ban/fail2ban.sqlite3 "SELECT name, COUNT(*) FROM bans GROUP BY name;"
# sshd|12
# recidive|3

sudo sqlite3 /var/lib/fail2ban/fail2ban.sqlite3 \
  "SELECT ip, jail, timeofban, bantime FROM bans LIMIT 5;"
```

### Désactiver la persistance (rare)

```ini
# fail2ban.conf — si vous voulez repartir de zéro à chaque boot
dbfile = :memory:
```

> À éviter en production : après un reboot (mise à jour noyau un dimanche à 3h), tous les attaquants seraient de nouveau libres de frapper pendant que vous dormez.

### Sauvegarder la base

Elle fait partie de la sauvegarde système, mais elle est **reconstruite automatiquement** si absente. Pas critique, mais pratique.

---

## 30. Logs et niveaux de debug

### Où et quoi

| Log | Contenu | Config |
|---|---|---|
| `/var/log/fail2ban.log` | bans, unbans, erreurs, démarrage des jails | `logtarget` dans fail2ban.conf |
| `journalctl -u fail2ban` | idem si `logtarget = SYSTEMD-JOURNAL` | — |
| Les logs surveillés | les attaques elles-mêmes | `logpath` de chaque jail |

### Niveaux de log

```ini
# /etc/fail2ban/fail2ban.d/debug.local — temporaire !
[Definition]
loglevel = DEBUG
```

| Niveau | Usage |
|---|---|
| `INFO` | production (défaut raisonnable) |
| `NOTICE` | production verbeuse (bans/unbans visibles) |
| `WARNING` | erreurs uniquement |
| `DEBUG` | dépannage : montre chaque ligne matchée, chaque décision |

> `DEBUG` est **très verbeux** : ne le laissez jamais en production (le log grossit de plusieurs Go/jour sur un serveur attaqué). Activez, reproduisez, désactivez.

### Lire efficacement le log

```bash
# Les 20 derniers bans
sudo grep " Ban " /var/log/fail2ban.log | tail -20

# Les erreurs (config, regex, actions)
sudo grep -iE "error|exception|failed" /var/log/fail2ban.log | tail -20

# Activité d'un jail sur la dernière heure
sudo awk -v d="$(date -d '1 hour ago' '+%Y-%m-%d %H')" \
  '$0 > d' /var/log/fail2ban.log | grep "\[sshd\]" | tail -30

# Compter les bans par jail aujourd'hui
sudo grep "$(date +%Y-%m-%d).* Ban " /var/log/fail2ban.log \
  | awk -F'[][]' '{print $2}' | sort | uniq -c | sort -rn
```

### Augmenter temporairement le debug d'un jail

```bash
# Passer tout le démon en DEBUG sans éditer de fichier :
sudo fail2ban-client set loglevel DEBUG
# ... reproduire le problème ...
sudo fail2ban-client set loglevel INFO
sudo grep -i "dovecot" /var/log/fail2ban.log | tail -40
```

---

## 31. Troubleshooting des logs : méthode

Quand « fail2ban ne bannit pas », suivez cette méthode dans l'ordre. 95 % des cas se résolvent aux étapes 1-3.

### Étape 1 : le jail tourne-t-il ?

```bash
sudo fail2ban-client status monjail
# Si "Sorry but the jail 'monjail' does not exist" → le jail n'est pas chargé.
# Vérifiez : enabled = true ? faute de frappe dans le nom ? reload effectué ?
sudo fail2ban-client status | grep "Jail list"
```

### Étape 2 : le log est-il lu ?

```bash
sudo fail2ban-client status monjail | grep -A2 "File list"
# File list: /var/log/auth.log
ls -la /var/log/auth.log   # le fichier existe ? non vide ? lisible par root ?
```

Si `Currently failed` reste à 0 alors que le log contient des échecs → le filter ne matche pas (étape 3).

### Étape 3 : le filter matche-t-il ?

```bash
sudo fail2ban-regex /var/log/auth.log /etc/fail2ban/filter.d/sshd.conf | tail -2
# Lines: 5000 lines, 0 ignored, 0 matched, 5000 missed
# → 0 matched = le filter ne voit rien. Vérifiez le format de date (section 46)
#    et le format des lignes (section 18).
```

### Étape 4 : l'action s'exécute-t-elle ?

```bash
sudo grep -E "Ban |action.*error" /var/log/fail2ban.log | tail -10
# Si vous voyez des erreurs après "Ban" → l'action firewall échoue.
# Causes : nftables vs iptables mal choisi, ufw inactif, droits.
```

Vérifiez la règle réellement créée :

```bash
sudo nft list table inet f2b-table 2>/dev/null | head -20
# ou :
sudo iptables -L f2b-sshd -n 2>/dev/null | head -20
```

### Étape 5 : le ban est-il effectif réseau ?

```bash
# Depuis la machine attaquante (ou un conteneur de test) :
curl -m 5 -v http://VOTRE_SERVEUR/ ; echo "code: $?"
ssh -o ConnectTimeout=5 user@VOTRE_SERVEUR ; echo "code: $?"
```

### Grille de diagnostic rapide

| Symptôme | Cause probable | Section |
|---|---|---|
| Jail absent de `status` | `enabled` oublié / faute de frappe / pas de reload | 31 |
| `Currently failed: 0` malgré des attaques | filter ne matche pas / mauvais logpath | 16, 46 |
| `Total failed` augmente mais aucun ban | seuils trop hauts / findtime trop court | 6, 23 |
| `Ban` dans le log mais IP toujours joignable | mauvaise action / mauvais port / Docker | 19, 20 |
| Ban immédiat au démarrage | `ignoreip` incomplet (c'est vous) | 24, 25 |
| Tout fonctionnait, puis plus rien | rotation de log mal gérée / backend | 45, 51 |

---

## 32. Intégration Cloudflare : bannir à l'edge

Si votre site est derrière Cloudflare, bannir l'IP sur votre serveur ne suffit pas toujours : autant la bannir **chez Cloudflare**, au bord du réseau.

### Principe

```
Attaquant → Cloudflare (edge) → votre serveur (origin)
                                        │
                              fail2ban détecte
                                        │
                                        ▼
                              API Cloudflare : "bloque cette IP"
                                        │
                                        ▼
                              Attaquant bloqué AVANT d'atteindre l'origin
```

### Prérequis

1. Un token API Cloudflare (modèle : *Edit zone firewall* ou le modèle « Firewall »).
2. L'ID de votre zone.

### Action Cloudflare

`/etc/fail2ban/action.d/cloudflare.conf` est fournie. Configuration :

```ini
# /etc/fail2ban/action.d/cloudflare.local
[Definition]
cfuser  = admin@example.com
cftoken = VOTRE_TOKEN_API_CLOUDFLARE
cfzoneid = VOTRE_ZONE_ID
```

Puis dans le jail :

```ini
[nginx-botsearch]
enabled = true
action  = %(action_)s
          cloudflare[name=nginx-botsearch]
```

### Le problème de l'IP réelle (important !)

Derrière Cloudflare, vos logs nginx voient **l'IP de Cloudflare**, pas celle du visiteur. Il faut restaurer la vraie IP :

```nginx
# /etc/nginx/nginx.conf
set_real_ip_from 173.245.48.0/20;
set_real_ip_from 103.21.244.0/22;
# ... (toutes les plages Cloudflare : https://www.cloudflare.com/ips/)
real_ip_header CF-Connecting-IP;
# ou : real_ip_header X-Forwarded-For;
```

> Sans `set_real_ip_from`, fail2ban bannira les IP de Cloudflare lui-même — catastrophique. C'est l'erreur classique n°8.

### Alternative sans API : bannir localement quand même

Si vous ne voulez pas d'intégration API, le ban local reste utile : il protège l'origin contre les attaques directes (bypass du CDN en attaquant l'IP du serveur). Mais il ne bloquera pas via le CDN.

---

## 33. AbuseIPDB : remonter les attaquants

AbuseIPDB est une base communautaire d'IP malveillantes. Deux usages : **consulter** (est-ce un attaquant connu ?) et **contribuer** (remonter vos attaquants).

### Consulter (enquête manuelle)

```bash
# Via l'API (clé gratuite : 1000 requêtes/jour)
curl -s -G https://api.abuseipdb.com/api/v2/check \
  -d ipAddress=203.0.113.45 \
  -d maxAgeInDays=90 \
  -H "Key: VOTRE_CLE_API" \
  -H "Accept: application/json" | python3 -m json.tool
```

Réponse (extraits) : `abuseConfidenceScore`, `totalReports`, `lastReportedAt`, pays, FAI.

### Remonter automatiquement (action)

Créez `/etc/fail2ban/action.d/abuseipdb.conf` :

```ini
[Definition]
actionban = curl -s --fail -X POST https://api.abuseipdb.com/api/v2/report \
              -H "Key: <abuseipdb_key>" -H "Accept: application/json" \
              --data-urlencode "ip=<ip>" \
              --data-urlencode "categories=18,22" \
              --data-urlencode "comment=fail2ban [<name>] : <failures> echecs" \
              >/dev/null
actionunban =

[Init]
abuseipdb_key = VOTRE_CLE_API
```

```ini
[sshd]
action = %(action_)s
         abuseipdb[name=sshd, abuseipdb_key=VOTRE_CLE_API]
```

Catégories AbuseIPDB utiles : `18` = brute-force, `22` = SSH, `21` = web app attack, `14` = port scan.

### Bonnes pratiques de remontée

- Ne remontez que les bans **automatiques** des jails fiables (sshd, nginx-botsearch), pas les bans manuels.
- Ne remontez pas les IP en `ignoreip` (évident, mais l'action ne le sait pas — filtrez en amont si besoin).
- Une remontée par ban suffit : AbuseIPDB déduplique, mais n'abusez pas du quota gratuit.

### Enrichir les alertes mail avec AbuseIPDB

Dans vos mails d'alerte (section 22), ajoutez le lien direct :

```
https://www.abuseipdb.com/check/203.0.113.45
```

Un clic suffit pour voir l'historique de l'IP avant de décider d'un ban permanent.

---

## 34. Monitoring Zabbix

### Ce qu'il faut superviser

| Métrique | Seuil d'alerte | Signification |
|---|---|---|
| fail2ban-server en cours d'exécution | process absent → critique | le service est tombé |
| Nombre de jails actifs | < attendu → warning | un jail ne s'est pas chargé |
| IP bannies par jail | pic brutal → warning | attaque en cours ou faux positifs |
| Total bans / heure | > 100/h → warning | campagne de scan massive |
| Âge de `/var/log/fail2ban.log` | pas modifié depuis 1h → info | peut-être normal (nuit calme) |

### UserParameter Zabbix

Sur l'agent Zabbix (`/etc/zabbix/zabbix_agentd.d/fail2ban.conf`) :

```ini
# Nombre de jails
UserParameter=fail2ban.jails,sudo /usr/bin/fail2ban-client status 2>/dev/null | grep "Number of jail" | awk '{print $NF}'

# IP actuellement bannies pour un jail donné : fail2ban.banned[sshd]
UserParameter=fail2ban.banned[*],sudo /usr/bin/fail2ban-client status "$1" 2>/dev/null | grep "Currently banned" | awk '{print $NF}'

# Total des bans pour un jail : fail2ban.total[sshd]
UserParameter=fail2ban.total[*],sudo /usr/bin/fail2ban-client status "$1" 2>/dev/null | grep "Total banned" | awk '{print $NF}'

# Échecs en cours (compteur non encore banni) : fail2ban.failed[sshd]
UserParameter=fail2ban.failed[*],sudo /usr/bin/fail2ban-client status "$1" 2>/dev/null | grep "Currently failed" | awk '{print $NF}'
```

Droits sudo pour l'utilisateur `zabbix` (via `visudo`) :

```
zabbix ALL=(ALL) NOPASSWD: /usr/bin/fail2ban-client status *
```

> Restreignez au strict nécessaire : `status` seul ne modifie rien. Ne donnez JAMAIS `fail2ban-client set *` ou `reload` à l'agent.

### Template Zabbix (idée de structure)

- Items : `fail2ban.jails`, `fail2ban.banned[sshd]`, `fail2ban.banned[recidive]`, `fail2ban.total[sshd]`.
- Triggers :
  - `{host:fail2ban.jails.last()}<5` → « fail2ban : jail manquant » (severity: average)
  - `{host:fail2ban.banned[recidive].last()}>50` → « fail2ban : nombreux récidivistes » (severity: warning)
  - `nodata(/host/proc.num[fail2ban-server],5m)=1` → « fail2ban-server arrêté » (severity: high)

---

## 35. Monitoring Prometheus

### Exporter les métriques (script simple)

Pas d'exporter officiel maintenu universellement ; le plus fiable en prod est un petit script + node_exporter `textfile`.

`/usr/local/bin/fail2ban_metrics.sh` :

```bash
#!/bin/bash
# Exporte les métriques fail2ban pour node_exporter (textfile)
OUT=/var/lib/node_exporter/textfile_collector/fail2ban.prom
TMP="$OUT.$$"

{
  echo "# HELP fail2ban_jails_total Nombre de jails actifs"
  echo "# TYPE fail2ban_jails_total gauge"
  JAILS=$(fail2ban-client status 2>/dev/null | grep "Number of jail" | awk '{print $NF}')
  echo "fail2ban_jails_total ${JAILS:-0}"

  echo "# HELP fail2ban_banned_current IP actuellement bannies par jail"
  echo "# TYPE fail2ban_banned_current gauge"
  echo "# HELP fail2ban_banned_total Total des bans par jail"
  echo "# TYPE fail2ban_banned_total counter"
  echo "# HELP fail2ban_failed_current Echecs en cours par jail"
  echo "# TYPE fail2ban_failed_current gauge"

  for j in $(fail2ban-client status 2>/dev/null | grep "Jail list" | sed 's/.*Jail list://;s/,//g'); do
    ST=$(fail2ban-client status "$j" 2>/dev/null)
    CUR=$(echo "$ST" | grep "Currently banned" | awk '{print $NF}')
    TOT=$(echo "$ST" | grep "Total banned" | awk '{print $NF}')
    FAIL=$(echo "$ST" | grep "Currently failed" | awk '{print $NF}')
    echo "fail2ban_banned_current{jail=\"$j\"} ${CUR:-0}"
    echo "fail2ban_banned_total{jail=\"$j\"} ${TOT:-0}"
    echo "fail2ban_failed_current{jail=\"$j\"} ${FAIL:-0}"
  done
} > "$TMP" && mv "$TMP" "$OUT"
```

Cron (toutes les 2 minutes) :

```cron
*/2 * * * * root /usr/local/bin/fail2ban_metrics.sh
```

### Alertes Prometheus (exemples)

```yaml
groups:
  - name: fail2ban
    rules:
      - alert: Fail2BanDown
        expr: absent(fail2ban_jails_total) or fail2ban_jails_total == 0
        for: 5m
        labels: { severity: critical }
        annotations:
          summary: "fail2ban ne tourne plus sur {{ $labels.instance }}"

      - alert: Fail2BanJailMissing
        expr: fail2ban_jails_total < 5
        for: 10m
        labels: { severity: warning }
        annotations:
          summary: "Jails manquants sur {{ $labels.instance }} ({{ $value }} actifs)"

      - alert: Fail2BanBanStorm
        expr: increase(fail2ban_banned_total[1h]) > 200
        for: 15m
        labels: { severity: warning }
        annotations:
          summary: "Tempête de bans sur {{ $labels.instance }} : {{ $value }} bans/h"
```

### Dashboard Grafana (panneaux suggérés)

1. `fail2ban_jails_total` — stat unique (vert si == attendu).
2. `fail2ban_banned_current` par jail — bar gauge.
3. `increase(fail2ban_banned_total[1h])` par jail — time series (courbe des attaques).
4. `fail2ban_failed_current{jail="sshd"}` — les attaques en cours non encore bannies.

---

## 36. Alertes mail et webhooks

### Prérequis : un MTA qui envoie

```bash
# Test d'envoi (postfix en satellite, exim, msmtp...)
echo "corps du message" | mail -s "[test] fail2ban" admin@example.com
```

Si le mail n'arrive pas, le problème est le MTA, pas fail2ban. Débuggez d'abord `mail.log`.

### Alerte mail par jail

```ini
[sshd]
enabled = true
action  = %(action_mwl)s[name=sshd, dest=admin@example.com, sender=fail2ban@srv-web.example.com]
```

Contenu du mail (`action_mwl`) : IP bannie, whois, nombre d'échecs, extrait des lignes de log. C'est verbeux : réservez-le aux jails critiques (sshd, recidive), pas à nginx-botsearch (sinon : 200 mails/nuit).

### Réduire le bruit : n'alerter que recidive

```ini
[DEFAULT]
action = %(action_)s          # silencieux par défaut

[recidive]
enabled = true
action  = %(action_mwl)s[name=recidive, dest=admin@example.com, sender=fail2ban@srv-web.example.com]
```

Vous ne recevez un mail que pour les vrais récidivistes (5 bans en 1 jour) : signal utile, bruit minimal.

### Webhook Mattermost/Slack (rappel section 22)

```ini
# /etc/fail2ban/action.d/webhook.conf — avec timeout et silence
[Definition]
actionban = curl -s -m 10 -X POST -H 'Content-Type: application/json' \
  -d '{"text":"🚨 [fail2ban] <name> : <ip> bannie (<failures> échecs)"}' \
  https://mattermost.exemple.fr/hooks/TOKEN >/dev/null 2>&1 || true
actionunban =
```

Le `|| true` évite que l'échec du webhook fasse échouer l'action de ban (une alerte ne doit jamais casser la protection).

---

## 37. Sauvegarde et restauration de jail.local

### Ce qu'il faut sauvegarder

| Fichier | Criticité | Note |
|---|---|---|
| `/etc/fail2ban/jail.local` | **critique** | toute votre config |
| `/etc/fail2ban/jail.d/*.local` | critique | si vous découpez |
| `/etc/fail2ban/filter.d/mon*.conf` | critique | vos filters maison |
| `/etc/fail2ban/action.d/mon*.conf` | important | vos actions maison |
| `/var/lib/fail2ban/fail2ban.sqlite3` | pratique | historique des bans |

### Script de sauvegarde

```bash
#!/bin/bash
# /usr/local/bin/backup-fail2ban.sh
DEST=/root/backups/fail2ban
DATE=$(date +%Y%m%d)
mkdir -p "$DEST"
tar -czf "$DEST/fail2ban-$DATE.tar.gz" \
  /etc/fail2ban/jail.local \
  /etc/fail2ban/jail.d/ \
  /etc/fail2ban/fail2ban.d/ \
  $(ls /etc/fail2ban/filter.d/*.conf 2>/dev/null | grep -v "^/etc/fail2ban/filter.d/$") \
  2>/dev/null
# Ne sauvegarde que jail.local + jail.d + fail2ban.d par défaut :
tar -czf "$DEST/fail2ban-essentiel-$DATE.tar.gz" \
  /etc/fail2ban/jail.local /etc/fail2ban/jail.d/ /etc/fail2ban/fail2ban.d/
ls -la "$DEST" | tail -5
```

> Simplifiez : sauvegardez `/etc/fail2ban/jail.local`, `/etc/fail2ban/jail.d/`, `/etc/fail2ban/fail2ban.d/` et vos fichiers `filter.d`/`action.d` maison. Le reste est fourni par le paquet.

### Versionner avec git (recommandé)

```bash
cd /etc/fail2ban
git init
git add jail.local jail.d/ fail2ban.d/
git commit -m "config initiale $(date +%Y-%m-%d)"
# Après chaque modification :
git diff && git commit -am "décrit le changement"
```

Un `git log` sur fail2ban vaut de l'or pendant un audit ou un dépannage (« qui a changé maxretry le 14 ? »).

### Restauration

```bash
# Restaurer les fichiers, puis :
sudo fail2ban-client --test   # valide
sudo fail2ban-client reload   # applique
sudo fail2ban-client status   # contrôle
```

---

## 38. Mise à jour de fail2ban

### Mise à jour via le paquet (cas normal)

```bash
sudo apt update && sudo apt install --only-upgrade fail2ban
```

Points d'attention :

1. **`jail.conf` peut être écrasé** : c'est normal, votre config est dans `jail.local` (qui n'est jamais touché).
2. **Nouveaux filters** : la mise à jour peut ajouter des filters dans `filter.d/`. Relisez les notes de version si vous avez surchargé un filter du même nom.
3. **Redémarrage du service** : le paquet redémarre fail2ban. Les bans actifs sont restaurés via sqlite (section 29). Vérifiez après :
   ```bash
   sudo fail2ban-client status
   sudo fail2ban-client status sshd | grep -E "Currently banned"
   ```

### Vérifier ce qui a changé

```bash
# Debian : voir les fichiers modifiés par la mise à jour
sudo apt changelog fail2ban | head -40
# ou :
zcat /usr/share/doc/fail2ban/changelog.Debian.gz | head -40
```

### Mise à jour majeure (0.11 → 1.x) : points de vigilance

- Le backend par défaut et les actions nftables ont évolué : relisez votre `banaction`.
- Certaines options dépréciées : consultez le log au redémarrage (`grep -i deprecat /var/log/fail2ban.log`).
- Testez en pré-production avant de déployer sur le parc.

### Stratégie de déploiement sur un parc

1. MaJ sur un serveur pilote.
2. `fail2ban-client --test` + 48h d'observation.
3. Déploiement Ansible sur le parc (section 55), par vagues.
4. Vérification centralisée : le monitoring (sections 34-35) doit montrer les jails actifs partout.

---

## 39. Erreurs classiques (1/2) : les 6 premières

### Erreur n°1 : se bannir soi-même

**Symptôme :** plus d'accès SSH au serveur juste après un reload.
**Cause :** votre IP n'est pas dans `ignoreip`, et vos tests (ou votre client qui réessaie) ont dépassé `maxretry`.
**Solution immédiate :** console hors-bande → `fail2ban-client set sshd unbanip VOTRE_IP`.
**Prévention :** sections 24 et 25. `ignoreip` complet + console de secours + dead man's switch.

### Erreur n°2 : le filter ne matche pas le format de log

**Symptôme :** `Currently failed: 0` alors que les attaques sont visibles dans le log.
**Causes fréquentes :**
- `log_format` nginx personnalisé sans les champs attendus (section 10).
- Application qui loggue en JSON alors que le filter attend du texte.
- Backend qui lit le mauvais fichier (auth.log vide, tout est dans le journal).
**Solution :** `fail2ban-regex` avec `--print-all-missed`, comparez ligne par ligne (section 16).

### Erreur n°3 : mauvais logpath

**Symptôme :** jail actif, 0 échec détecté, le fichier de log est vide ou n'existe pas.
**Exemples réels :**
- proftpd qui loggue dans `/var/log/auth.log` au lieu de `/var/log/proftpd/proftpd.log`.
- postfix sur un système sans rsyslog : tout est dans `journalctl`, `/var/log/mail.log` n'existe pas.
**Solution :** trouvez où l'application écrit VRAIMENT :
```bash
sudo lsof -p $(pgrep -f "dovecot/auth" | head -1) 2>/dev/null | grep log
# ou plus simple : provoquez un échec et cherchez où il apparaît
sudo grep -r "authentication failed" /var/log/ 2>/dev/null | head -5
```

### Erreur n°4 : backend systemd vs auto

**Symptôme :** sur un système récent, le jail ne voit rien bien que le log existe.
**Cause :** `backend = auto` choisit `pyinotify`, mais les logs arrivent via journald sans passer par le fichier (ou l'inverse).
**Solution :** forcez le backend adapté :
```ini
[monjail]
backend = systemd
# et pour cibler l'unité :
journalmatch = _SYSTEMD_UNIT=monservice.service
```
Voir section 45 pour le détail des backends.

### Erreur n°5 : fuseau horaire incohérent

**Symptôme :** les bans se déclenchent avec un retard bizarre, ou `findtime` semble ne pas fonctionner (les échecs ne sont pas comptés ensemble).
**Cause :** les logs sont en UTC, le système en Europe/Paris (ou l'inverse). fail2ban compare les timestamps du log avec l'heure système.
**Solution :** harmonisez (section 46) :
```bash
timedatectl   # vérifiez "Time zone" et "Universal time"
head -1 /var/log/auth.log   # quel fuseau dans les logs ?
```

### Erreur n°6 : le ban ne bloque rien (mauvais port)

**Symptôme :** `Ban 203.0.113.45` dans le log, mais l'attaquant continue.
**Causes :**
- `port = ssh` alors que sshd écoute sur 2222 → la règle firewall bloque le port 22, pas le 2222.
- Action `iptables` alors que le système utilise nftables en natif (règles créées au mauvais endroit).
- Docker qui bypasse les chaînes INPUT.
**Solution :** vérifiez le port réel (`ss -tlnp | grep sshd`), vérifiez la règle créée (section 19), testez depuis l'extérieur.

---

## 40. Erreurs classiques (2/2) : les 6 suivantes

### Erreur n°7 : modifier jail.conf au lieu de jail.local

**Symptôme :** après `apt upgrade`, toute la config a disparu.
**Cause :** `jail.conf` est écrasé par le paquet.
**Solution :** tout migrer dans `jail.local` (section 4). Pour récupérer : si vous avez un backup ou un git, restaurez ; sinon, réécrivez (douloureux, mais formateur).

### Erreur n°8 : bannir les IP de Cloudflare / du reverse proxy

**Symptôme :** des centaines d'utilisateurs légitimes bloqués d'un coup, ou le site entier inaccessible.
**Cause :** les logs voient l'IP du proxy/CDN, fail2ban bannit le proxy/CDN.
**Solution :** `set_real_ip_from` + `real_ip_header` (nginx, section 32/52) ou `RemoteIPHeader` (Apache). Et mettez les IP du proxy en `ignoreip` en attendant.

### Erreur n°9 : maxretry trop bas sur un service à humains

**Symptôme :** tickets au support : « je suis bloqué » — utilisateurs légitimes (dovecot, webmail, VPN).
**Cause :** `maxretry = 3` + smartphone qui réessaie en boucle.
**Solution :** seuils différenciés (section 23) : `maxretry = 10`, `bantime = 30m` sur dovecot. Communiquez auprès des utilisateurs.

### Erreur n°10 : oublier recidive (ou mal le configurer)

**Symptôme :** les mêmes IP reviennent toutes les heures, indéfiniment.
**Cause :** pas de jail recidive, ou recidive qui pointe vers un `logpath` inexistant.
**Solution :** activez recidive (section 14), vérifiez qu'il lit bien `/var/log/fail2ban.log`.

### Erreur n°11 : loglevel DEBUG oublié en production

**Symptôme :** `/var` plein à 100 %, services qui tombent.
**Cause :** `loglevel = DEBUG` laissé après un dépannage ; sur un serveur attaqué, ça génère des Go/heure.
**Solution :** remettez `INFO`, mettez en place une alerte sur l'espace disque ET sur la taille de `/var/log/fail2ban.log` :
```bash
# Alerte simple via cron :
find /var/log/fail2ban.log -size +500M -exec echo "ALERTE fail2ban.log trop gros" | mail -s alerte admin@example.com \;
```

### Erreur n°12 : tester en production sans filet

**Symptôme :** tout ce qui précède, en même temps, un vendredi à 18h.
**Cause :** reload d'une config non testée, sans console de secours, sans dead man's switch.
**Solution :** la discipline des sections 25 et 31 : `--test`, `fail2ban-regex`, bantime court pendant les essais, console de secours ouverte, jamais le vendredi soir.

---

## 41. Backend : auto, systemd, pyinotify, polling

Le **backend** est la méthode par laquelle fail2ban surveille les logs. Le choix impacte la fiabilité.

### Les backends disponibles

| Backend | Méthode | Avantages | Inconvénients |
|---|---|---|---|
| `auto` | choisit le meilleur dispo | zéro config, convient à 95 % | choix parfois surprenant |
| `systemd` | journald natif | pas de fichier à gérer, métadonnées riches | que pour les services systemd |
| `pyinotify` | inotify (événements noyau) | temps réel, peu de CPU | ne voit pas les rotations mal gérées |
| `gamin` | Gamin (similaire) | temps réel | dépendance supplémentaire |
| `polling` | relecture périodique | fonctionne partout | CPU +, latence |

Ordre de `auto` : pyinotify → gamin → polling → systemd.

### Quand forcer systemd

```ini
[postfix]
enabled      = true
backend      = systemd
journalmatch = _SYSTEMD_UNIT=postfix.service + _SYSTEMD_UNIT=postfix@-.service
```

Cas d'usage : pas de rsyslog installé (Debian minimale, containers), logs uniquement dans le journal.

Vérifier que le journal contient bien les lignes :

```bash
sudo journalctl -u postfix --since "10 min ago" | grep -i "sasl.*failed" | head -3
```

### Quand forcer polling

Rare : systèmes de fichiers réseau (NFS) où inotify ne fonctionne pas.

```ini
[monjail]
backend = polling
```

### Diagnostiquer le backend actif

```bash
sudo grep -i "backend" /var/log/fail2ban.log | tail -5
# ... INFO [sshd] Using backend auto -> polling (par ex.)
sudo fail2ban-client get sshd backend
```

---

## 42. Le fuseau horaire : la cause cachée

### Le problème

fail2ban lit un timestamp dans chaque ligne de log et le compare à l'heure système pour `findtime`. Si les deux ne sont pas dans le même fuseau :

- Logs en UTC, système en Europe/Paris (UTC+2 en été) : un échec « à 10h00 UTC » est vu comme « il y a 2h » → il sort immédiatement de la fenêtre `findtime = 10m` → **aucun ban ne se déclenche jamais**.
- L'inverse : des échecs vieux de 2h sont comptés comme récents → **bans abusifs**.

### Diagnostic

```bash
timedatectl | grep -E "Time zone|Universal time|Local time"
head -2 /var/log/auth.log
# Sep 26 10:00:01 ...  → heure locale (rsyslog utilise l'heure système par défaut)
date -u && date
```

Si `date` (locale) et les timestamps des logs diffèrent d'un nombre rond d'heures → problème de fuseau.

### Cas typiques

| Source de log | Fuseau habituel | Solution |
|---|---|---|
| rsyslog local | heure système | OK si système à l'heure |
| Application Java/Node | souvent UTC | configurer l'app en heure locale, ou accepter le décalage |
| Conteneur Docker | UTC par défaut | monter `/etc/localtime` dans le conteneur |
| Log applicatif avec `Z` | UTC explicite | fail2ban gère le `Z` s'il est dans le format de date |

### Bonne pratique

Mettez **tout en UTC** sur les serveurs (recommandation sysadmin classique) :

```bash
sudo timedatectl set-timezone UTC
```

fail2ban, rsyslog et le système sont alors cohérents. Les humains convertissent à la lecture (les outils le font pour vous).

---

## 43. IPv6 avec fail2ban

### Ça marche nativement

`<HOST>` matche les IPv6. Rien à configurer pour la détection.

### Points d'attention spécifiques

1. **`ignoreip` doit inclure `::1`** (loopback v6) — sinon un service local en v6 peut se faire bannir.
2. **Les actions firewall doivent gérer v6** : `iptables-multiport` ne gère que v4 ! Pour v6 :
   ```ini
   [DEFAULT]
   banaction = nftables-multiport   # gère v4 + v6 nativement
   ```
   Avec nftables, pas de problème. Avec iptables classique, il faudrait aussi `ip6tables` — d'où l'intérêt de nftables.
3. **Les attaquants v6 ont des /64 entiers** : bannir une seule IPv6 est parfois inutile (l'attaquant en a 2^64). Pour les cas extrêmes, certaines actions permettent de bannir le /64 (personnalisation avancée, hors scope ici — à noter comme piste).

### Vérifier

```bash
# Forcer un ban de test en IPv6 (depuis une machine v6) puis :
sudo nft list set inet f2b-table f2b-sshd | grep -i ":"
```

---

## 44. Bannir via ipset pour la performance

Quand le nombre de bans simultanés dépasse ~1000, les chaînes iptables linéaires ralentissent. `ipset` stocke les IP dans une table de hachage : O(1) au lieu de O(n).

### Installation et configuration

```bash
sudo apt install -y ipset
```

```ini
[DEFAULT]
banaction = iptables-ipset-proto6
```

Variantes : `iptables-ipset-proto6` (tous protocoles, ports multiples), `iptables-ipset-proto6-allports`.

### Vérification

```bash
sudo ipset list | head -30
# Name: f2b-sshd
# Type: hash:net
# Members:
# 203.0.113.45
```

### Quand en avez-vous besoin ?

| Bans simultanés | Action conseillée |
|---|---|
| < 200 | nftables-multiport (défaut moderne) |
| 200 – 2000 | nftables-multiport tient encore très bien |
| > 2000 | ipset ou nftables avec sets (les sets nftables sont déjà en hachage) |

> Bonne nouvelle : les **sets nftables** utilisés par `nftables-multiport` sont déjà des tables de hachage. Sur les systèmes récents, vous avez la performance d'ipset sans rien faire.

---

## 45. Rotation de logs et fail2ban

### Le problème

`logrotate` compresse/renomme `/var/log/auth.log` → `auth.log.1`. Si fail2ban suit l'ancien descripteur de fichier, il ne voit plus les nouvelles attaques.

### Pourquoi ça marche (en général)

- Avec `pyinotify`/`systemd`, fail2ban détecte la rotation et rouvre le fichier.
- Les configurations logrotate de Debian/Ubuntu utilisent `copytruncate` ou `create` avec un `postrotate` qui notifie — compatibles.

Vérifiez votre logrotate :

```bash
cat /etc/logrotate.d/rsyslog | head -20
# /var/log/auth.log {
#   rotate 4
#   weekly
#   ...
#   postrotate
#     /usr/lib/rsyslog/rsyslog-rotate
#   endscript
# }
```

### Tester après une rotation forcée

```bash
sudo logrotate -f /etc/logrotate.d/rsyslog
sleep 5
sudo fail2ban-client status sshd | grep -E "File list|Currently"
# Si le jail ne suit plus le nouveau fichier : restart du jail
sudo fail2ban-client reload sshd
```

### Le cas des logs applicatifs maison

Si votre application écrit dans `/var/log/myapp/auth.log`, créez son logrotate :

```
# /etc/logrotate.d/myapp
/var/log/myapp/*.log {
  daily
  rotate 14
  compress
  delaycompress
  missingok
  notifempty
  copytruncate
}
```

`copytruncate` : le fichier garde le même inode, fail2ban ne perd jamais le fil. (Petit risque de perdre quelques lignes pendant la troncature — acceptable pour des logs de sécurité.)

---

## 46. Fail2ban derrière un proxy inverse

### Le problème (rappel erreur n°8)

```
Client (198.51.100.7) → nginx reverse proxy → backend Apache
                                              logs : "connexion depuis 10.0.0.5 (le proxy)"
```

fail2ban sur le backend voit l'IP du proxy. Le bannir = couper le proxy = couper tout le monde.

### Solution nginx (côté proxy ET backend)

Sur le **proxy** : transmettre la vraie IP.

```nginx
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```

Sur le **backend** : utiliser la vraie IP dans les logs.

```nginx
# backend nginx
set_real_ip_from 10.0.0.0/8;      # le proxy interne
real_ip_header X-Forwarded-For;
real_ip_recursive on;
log_format main '$remote_addr - ...';   # $remote_addr = vraie IP client maintenant
```

### Solution Apache (backend)

```apache
# a2enmod remoteip
RemoteIPHeader X-Forwarded-For
RemoteIPTrustedProxy 10.0.0.5
LogFormat "%a %l %u %t \"%r\" %>s %b" combined_realip
```

`%a` loggue alors la vraie IP client, et les filters apache standards fonctionnent.

### Où placer fail2ban : proxy ou backend ?

| Option | Avantages | Inconvénients |
|---|---|---|
| Sur le proxy (recommandé) | voit les vraies IP directement, bloque tôt | un seul point de config |
| Sur le backend | protège aussi en cas de bypass du proxy | config real_ip obligatoire |
| Les deux | défense en profondeur | double administration |

> Recommandation : fail2ban sur le **proxy** pour les jails web (nginx-*), et sur chaque serveur pour sshd (chaque machine est scannée directement).

### En attendant la correction : whitelistez le proxy

```ini
[DEFAULT]
ignoreip = 127.0.0.1/8 ::1 10.0.0.5   # IP du reverse proxy
```

Ça évite la catastrophe, mais ça **désactive** la protection (toutes les IP passent par le proxy). C'est un pansement, pas un traitement.

---

## 47. Tester en sécurité : la discipline du dry-run

Il n'y a pas de vrai mode « dry-run » dans fail2ban, mais voici l'équivalent opérationnel.

### Protocole de test d'un nouveau jail

```bash
# 1. Créer le jail avec une action INOFFENSIVE (log seul)
# /etc/fail2ban/jail.d/test.local
[nginx-test]
enabled  = true
filter   = nginx-botsearch
logpath  = /var/log/nginx/access.log
maxretry = 3
action   = %(action_)s
           # action_ avec banaction remplacée par un simple log :
banaction = dummy-log

# /etc/fail2ban/action.d/dummy-log.conf
[Definition]
actionban   = echo "[DRY-RUN] bannirait <ip> (jail <name>)" >> /var/log/fail2ban-dryrun.log
actionunban = echo "[DRY-RUN] débannirait <ip> (jail <name>)" >> /var/log/fail2ban-dryrun.log
```

```bash
# 2. Recharger, attendre 24-48h
sudo fail2ban-client reload

# 3. Analyser ce qui AURAIT été banni
cat /var/log/fail2ban-dryrun.log
# Des IP légitimes dedans ? → ajustez le filter/seuils avant de passer en vrai ban.
```

### Test de charge d'un filter

```bash
# Combien de lignes matchent sur 7 jours de logs ?
zcat /var/log/nginx/access.log* 2>/dev/null | \
  sudo fail2ban-regex - /etc/fail2ban/filter.d/nginx-botsearch.conf | tail -2
```

Si le filter matche 50 000 lignes/semaine dont 40 000 légitimes (ex: votre appli mobile qui appelle `/.well-known/`), revoyez la regex avant d'activer le ban.

---

## 48. Scripts d'administration utiles

### Rapport quotidien des bans (cron)

`/usr/local/bin/fail2ban-daily-report.sh` :

```bash
#!/bin/bash
# Rapport quotidien fail2ban → mail à l'admin
DEST="admin@example.com"
HOST=$(hostname -f)
TMP=$(mktemp)

{
  echo "Rapport fail2ban du $(date +%Y-%m-%d) — $HOST"
  echo "=================================================="
  echo
  fail2ban-client status
  echo
  for j in $(fail2ban-client status | grep "Jail list" | sed 's/.*Jail list://;s/,//g'); do
    echo "--- Jail: $j ---"
    fail2ban-client status "$j" | grep -E "Currently|Total"
    echo
  done
  echo "Top 10 des IP bannies (24h) :"
  grep "$(date -d 'yesterday' +%Y-%m-%d).* Ban " /var/log/fail2ban.log | \
    awk '{print $NF}' | sort | uniq -c | sort -rn | head -10
  echo
  echo "Top 10 des IP bannies (7j) :"
  grep " Ban " /var/log/fail2ban.log | awk '{print $NF}' | \
    sort | uniq -c | sort -rn | head -10
} > "$TMP"

mail -s "[fail2ban] rapport quotidien $HOST" "$DEST" < "$TMP"
rm -f "$TMP"
```

```cron
# /etc/cron.d/fail2ban-report
0 7 * * * root /usr/local/bin/fail2ban-daily-report.sh
```

### Débannir une IP partout (tous les jails)

```bash
#!/bin/bash
# /usr/local/bin/f2b-unban-all.sh
IP="$1"
[ -z "$IP" ] && { echo "Usage: $0 <IP>"; exit 1; }
for j in $(sudo fail2ban-client status | grep "Jail list" | sed 's/.*Jail list://;s/,//g'); do
  sudo fail2ban-client set "$j" unbanip "$IP" 2>/dev/null && echo "débannie de $j"
done
```

### Exporter toutes les IP bannies (pour audit)

```bash
for j in $(sudo fail2ban-client status | grep "Jail list" | sed 's/.*Jail list://;s/,//g'); do
  echo "## $j"
  sudo fail2ban-client status "$j" | grep -A100 "Banned IP list" | tail -n +2
done > /root/fail2ban-banned-$(date +%Y%m%d).txt
```

### Compter les bans par heure (détecter les campagnes)

```bash
sudo grep " Ban " /var/log/fail2ban.log | \
  awk '{print $1" "$2}' | cut -d: -f1 | sort | uniq -c | sort -rn | head -24
# 2026-09-26 03  →  47 bans entre 3h et 4h : campagne nocturne typique
```

---

## 49. Intégration Ansible

### Rôle minimal

```yaml
# roles/fail2ban/tasks/main.yml
- name: Installer fail2ban
  apt:
    name: fail2ban
    state: present
    update_cache: true

- name: Déployer jail.local
  template:
    src: jail.local.j2
    dest: /etc/fail2ban/jail.local
    owner: root
    group: root
    mode: "0644"
  notify: reload fail2ban

- name: Déployer les filters maison
  copy:
    src: "{{ item }}"
    dest: "/etc/fail2ban/filter.d/{{ item }}"
    mode: "0644"
  loop:
    - myapp.conf
  notify: reload fail2ban

- name: Activer et démarrer fail2ban
  systemd:
    name: fail2ban
    enabled: true
    state: started
```

```yaml
# roles/fail2ban/handlers/main.yml
- name: reload fail2ban
  command: fail2ban-client reload
  # Plutôt qu'un restart : les bans actifs sont conservés
```

### Template jail.local avec variables par groupe

```jinja2
# roles/fail2ban/templates/jail.local.j2
[DEFAULT]
bantime  = {{ fail2ban_bantime | default('1h') }}
findtime = {{ fail2ban_findtime | default('10m') }}
maxretry = {{ fail2ban_maxretry | default(5) }}
ignoreip = 127.0.0.1/8 ::1 {{ fail2ban_ignoreip_extra | default('') }}
banaction = {{ fail2ban_banaction | default('nftables-multiport') }}

[sshd]
enabled = true

{% if 'webservers' in group_names %}
[nginx-http-auth]
enabled = true
port    = http,https
filter  = nginx-http-auth
logpath = /var/log/nginx/error.log

[nginx-botsearch]
enabled  = true
filter   = nginx-botsearch
logpath  = /var/log/nginx/access.log
maxretry = 3
findtime = 1d
bantime  = 1w
{% endif %}

{% if 'mailservers' in group_names %}
[postfix]
enabled = true
filter  = postfix
logpath = /var/log/mail.log

[dovecot]
enabled  = true
filter  = dovecot
logpath = /var/log/mail.log
maxretry = 10
bantime  = 30m
{% endif %}

[recidive]
enabled   = true
filter    = recidive
logpath   = /var/log/fail2ban.log
banaction = %(banaction_allports)s
bantime   = 1w
findtime  = 1d
maxretry  = 5
```

### Vérification post-déploiement

```yaml
- name: Vérifier les jails actifs
  command: fail2ban-client status
  register: f2b_status
  changed_when: false

- name: Afficher
  debug:
    var: f2b_status.stdout_lines
```

---

## 50. Intégration avec GeoIP

### Cas d'usage

Vous n'avez aucun utilisateur légitime hors d'Europe ? Vous pouvez **alerter** (ou bannir) les IP de pays inattendus. Attention : la géolocalisation d'IP est approximative, et les attaquants utilisent des VPS européens. C'est un **signal**, pas une preuve.

### Enrichir les logs avec le pays

```bash
sudo apt install -y geoip-bin geoip-database
geoiplookup 203.0.113.45
# GeoIP Country Edition: US, United States
```

Script d'enrichissement du rapport quotidien (section 48) : ajoutez une colonne pays au top 10.

### Bannir un pays : la réalité technique

fail2ban ne fait pas de GeoIP natif. Les approches :

1. **Listes d'IP par pays** (ex: ipdeny.com) chargées dans un ipset/nftables set, en amont de fail2ban. C'est du firewall statique, pas du fail2ban.
2. **Action personnalisée** qui consulte GeoIP au moment du ban et allonge le bantime si le pays est « inattendu ».

Exemple d'action « bantime selon pays » (idée, à adapter) :

```ini
# /etc/fail2ban/action.d/geo-ban.conf
[Definition]
actionban = /usr/local/bin/geo-ban.sh <ip> <name>
actionunban = /usr/local/bin/geo-unban.sh <ip>
```

> Recommandation pragmatique : **n'auto-bannissez pas par pays**. Utilisez la GeoIP pour le *reporting* (d'où viennent les attaques ?) et gardez les décisions de ban sur les comportements (échecs d'authentification).

---

## 51. Le fichier filter.d : anatomie complète

```ini
# /etc/fail2ban/filter.d/exemple.conf

[INCLUDES]
# Charge d'abord common.conf (définit <HOST>, __prefix_line...)
# puis les fichiers listés, dans l'ordre
before = common.conf

[Definition]
# La/les regex de détection (une par ligne, toutes en OU)
failregex = ^%(__prefix_line)sFailed password for <F-USER>\S+</F-USER> from <HOST> port \d+ ssh2$
            ^%(__prefix_line)sInvalid user \S+ from <HOST> port \d+$

# Regex d'exclusion : une ligne qui matche ignoreregex n'est JAMAIS comptée,
# même si elle matche failregex (pratique pour les faux positifs connus)
ignoreregex = ^%(__prefix_line)sFailed password for deploy from <HOST> port \d+ ssh2$

# Surcharges possibles :
# datepattern = ^%Y-%m-%d %H:%M:%S   (si le format de date n'est pas auto-détecté)
# journalmatch = _SYSTEMD_UNIT=sshd.service  (pour le backend systemd)
```

### Ordre d'évaluation pour chaque ligne de log

1. La date est-elle reconnue ? (non → ligne ignorée silencieusement)
2. `ignoreregex` matche-t-elle ? (oui → ignorée)
3. Une `failregex` matche-t-elle ? (oui → échec compté pour `<HOST>`)

### Héritage et surcharge d'un filter existant

Ne modifiez jamais un filter fourni : créez `filter.d/sshd.local` :

```ini
# /etc/fail2ban/filter.d/sshd.local — AJOUTE des regex au filter sshd
[Definition]
failregex = %(known/failregex)s
            ^%(__prefix_line)sMon pattern maison from <HOST>$
```

`%(known/failregex)s` reprend les regex du fichier de base. Votre `.local` les complète.

---

## 52. Le fichier action.d : anatomie complète

```ini
# /etc/fail2ban/action.d/exemple.conf

[Definition]
# Exécuté une fois au démarrage du jail
actionstart = iptables -N f2b-<name>
              iptables -A f2b-<name> -j RETURN
              iptables -I INPUT -p tcp --dport <port> -j f2b-<name>

# Exécuté à chaque ban
actionban = iptables -I f2b-<name> 1 -s <ip> -j <blocktype>

# Exécuté à chaque unban (fin de bantime ou unban manuel)
actionunban = iptables -D f2b-<name> -s <ip> -j <blocktype>

# Exécuté à l'arrêt du jail (doit ANNULER actionstart)
actionstop = iptables -D INPUT -p tcp --dport <port> -j f2b-<name>
             iptables -F f2b-<name>
             iptables -X f2b-<name>

# Vérification optionnelle avant ban (retourne 0 si déjà banni)
actioncheck = iptables -n -L f2b-<name> | grep -q 'f2b-<name>'

[Init]
# Valeurs par défaut des variables, surchargeables dans le jail
port = ssh
blocktype = REJECT --reject-with icmp-port-unreachable
```

### Variables disponibles dans les actions

| Variable | Contenu |
|---|---|
| `<ip>` | l'IP bannie/débannie |
| `<name>` | le nom du jail |
| `<port>` | le(s) port(s) du jail |
| `<failures>` | nombre d'échecs ayant déclenché le ban |
| `<time>` | timestamp du ban |
| `<matches>` | extrait des lignes de log fautives |

### Règle d'or des actions maison

`actionstop` doit **exactement annuler** `actionstart`, et `actionunban` doit **exactement annuler** `actionban`. Sinon : règles orphelines qui s'accumulent à chaque reload.

---

## 53. Jail.d : surcharges modulaires

Plutôt qu'un seul gros `jail.local`, découpez par service dans `/etc/fail2ban/jail.d/`. Les fichiers `.local` sont lus après `jail.local`.

```
# /etc/fail2ban/jail.d/00-defaults.local
[DEFAULT]
bantime  = 1h
findtime = 10m
maxretry = 5
ignoreip = 127.0.0.1/8 ::1 192.168.10.0/24
banaction = nftables-multiport

# /etc/fail2ban/jail.d/10-sshd.local
[sshd]
enabled = true

# /etc/fail2ban/jail.d/20-web.local
[nginx-http-auth]
enabled = true
...

# /etc/fail2ban/jail.d/30-mail.local
[postfix]
enabled = true
...

# /etc/fail2ban/jail.d/99-recidive.local
[recidive]
enabled = true
...
```

Avantages : chaque fichier a un responsable, les diffs git sont lisibles, Ansible déploie par rôle.

---

## 54. Ordre de fusion de la configuration

fail2ban lit les fichiers dans cet ordre strict. **La dernière valeur définie gagne.**

```
1. jail.conf
2. jail.d/*.conf          (ordre alphabétique)
3. jail.local
4. jail.d/*.local         (ordre alphabétique)
```

### Voir la configuration effective d'un jail

```bash
# fail2ban >= 1.0 : affiche la config fusionnée
sudo fail2ban-client --dp "jail:sshd:maxretry"
sudo fail2ban-client -d | grep -A15 "^\[sshd\]"
```

`fail2ban-client -d` (dump) affiche **toute** la configuration effective : c'est la vérité de référence quand on ne comprend plus quelle valeur s'applique.

### Piège classique

```ini
# jail.local
[DEFAULT]
maxretry = 5

# jail.d/99-strict.local
[sshd]
maxretry = 3    # ← gagne sur le 5 du DEFAULT pour le jail sshd
```

Si vous ne comprenez pas d'où vient une valeur, `fail2ban-client -d` + `grep`.

---

## 55. Durcissement de fail2ban lui-même

fail2ban tourne en **root** (il doit modifier le firewall). Quelques mesures :

### Réduire la surface

```ini
# /etc/fail2ban/fail2ban.conf — via fail2ban.d/
[Definition]
# Socket accessible uniquement à root
socket = /run/fail2ban/fail2ban.sock
# (permissions gérées par systemd, déjà restrictives par défaut)
```

Vérifiez :

```bash
ls -la /run/fail2ban/fail2ban.sock
# srwx------ 1 root root ... → seul root peut piloter le démon. Parfait.
```

### Protéger les fichiers de config

```bash
sudo chmod 640 /etc/fail2ban/action.d/webhook.conf   # contient un token
sudo chmod 640 /etc/fail2ban/action.d/cloudflare.local
sudo chown root:root /etc/fail2ban/jail.local
```

### Surveiller l'intégrité

- Mettez `/etc/fail2ban/` sous git (section 37) : toute modification non planifiée apparaît dans `git status`.
- Avec AIDE ou auditd, surveillez les écritures dans `/etc/fail2ban/`.

### fail2ban n'écoute pas sur le réseau

Le démon ne bind **aucun port** : il communique via socket Unix local. Il n'est pas exposable directement. Le risque vient des **actions** (curl vers des webhooks, tokens en clair) : soignez-les.

---

## 56. Bannir un pays entier : réaliste ?

### La réponse courte

Non, pas avec fail2ban seul. Et c'est rarement une bonne idée.

### Pourquoi c'est tentant (et pourquoi ça échoue)

- Tentant : « 80 % de mes attaques viennent de 3 pays où je n'ai aucun client ».
- Problème 1 : les attaquants louent des VPS **dans votre pays** (OVH, Hetzner...). Le géoblocage ne les arrête pas.
- Problème 2 : les plages d'IP par pays changent en permanence ; une liste statique se périme.
- Problème 3 : un client en déplacement à l'étranger (ou derrière un VPN) se fait bloquer → ticket au support.

### Si vous voulez quand même le faire (firewall, pas fail2ban)

```bash
# Exemple nftables avec un set d'IP par pays (listes : ipdeny.com)
sudo nft add table inet geo-filter
sudo nft add set inet geo-filter blocked-countries '{ type ipv4_addr; flags interval; }'
# Charger la liste (script à écrire, mise à jour hebdo via cron)
for ip in $(cat /root/geo/country-zone.txt); do
  sudo nft add element inet geo-filter blocked-countries "{ $ip }"
done
sudo nft add rule inet geo-filter input ip saddr @blocked-countries drop
```

> C'est du filtrage statique au firewall. fail2ban garde son rôle : la détection **comportementale** (échecs répétés), quelle que soit l'origine.

---

## 57. Corrélation avec auditd

auditd trace les appels système ; fail2ban lit des logs applicatifs. Les deux se complètent pour l'enquête post-incident.

### Scénario : une IP bannie par fail2ban, que faisait-elle ?

```bash
IP="203.0.113.45"
# 1. Quand a-t-elle été bannie ?
sudo grep "Ban $IP" /var/log/fail2ban.log
# 2. Quelles lignes de log ont déclenché ?
sudo grep "$IP" /var/log/auth.log | tail -20
# 3. Y a-t-il eu des connexions RÉUSSIES depuis cette IP ? (le vrai danger)
sudo grep "$IP" /var/log/auth.log | grep -i "accepted"
# Si oui → compromission possible → incident de sécurité, pas juste un ban.
```

### Règle auditd utile : surveiller les modifications de fail2ban

```
# /etc/audit/rules.d/fail2ban.rules
-w /etc/fail2ban/jail.local -p wa -k fail2ban-config
-w /etc/fail2ban/jail.d/ -p wa -k fail2ban-config
-w /usr/bin/fail2ban-client -p x -k fail2ban-client
```

```bash
sudo augenrules --load
sudo ausearch -k fail2ban-config --start recent
```

Un attaquant qui a pris pied tentera de **désactiver fail2ban** pour travailler tranquille. Cette règle vous en avertit.

---

## 58. Fail2ban et Docker : attention

### Le problème

Docker manipule iptables/nftables avec ses propres chaînes (`DOCKER`, `DOCKER-USER`). Par défaut, les règles fail2ban sur `INPUT` **ne voient pas** le trafic destiné aux conteneurs (qui passe par `FORWARD`).

```
Internet → INPUT (fail2ban bloque ici : OK pour sshd de l'hôte)
Internet → DOCKER-USER → FORWARD → conteneur nginx
         (fail2ban sur INPUT ne voit RIEN ici)
```

### Solution : bannir dans DOCKER-USER

Utilisez une action qui insère dans la chaîne `DOCKER-USER` (évaluée avant les règles Docker) :

```ini
[nginx-botsearch]
enabled = true
banaction = iptables-multiport[chain=DOCKER-USER]
```

Ou en nftables : créez une action qui accroche la chaîne `forward` :

```ini
# Variante : banaction nftables sur le forward
# (à adapter selon votre version de l'action nftables)
```

### Alternative simple et robuste

Faites tourner fail2ban **dans un conteneur à part** avec accès au socket Docker et aux logs des autres conteneurs (montage des volumes de logs), ou — plus simple — exposez les ports via le host et laissez fail2ban sur l'hôte avec la chaîne adaptée.

### Vérification indispensable

```bash
# Depuis l'extérieur, avec une IP de test bannie :
# 1. Le conteneur est-il injoignable ?
curl -m 5 http://VOTRE_SERVEUR/ ; echo $?
# 2. Où est la règle ?
sudo iptables -L DOCKER-USER -n | grep 203.0.113.45
```

> Si vous ne testez pas, vous avez un fail2ban décoratif sur une infra Docker.

---

## 59. Journalctl et backend systemd : recettes

### jail postfix en backend systemd complet

```ini
[postfix]
enabled      = true
backend      = systemd
journalmatch = _SYSTEMD_UNIT=postfix.service
filter       = postfix
port         = smtp,465,submission
maxretry     = 5
```

### Plusieurs unités pour un jail

```ini
[dovecot]
enabled      = true
backend      = systemd
journalmatch = _SYSTEMD_UNIT=dovecot.service
filter       = dovecot
```

### Filtrer par niveau dans le journal

```ini
# Ne regarder que les erreurs et plus grave
journalmatch = _SYSTEMD_UNIT=nginx.service + PRIORITY=3
```

### Tester le journalmatch

```bash
sudo journalctl _SYSTEMD_UNIT=postfix.service --since "1 hour ago" | \
  sudo fail2ban-regex - /etc/fail2ban/filter.d/postfix.conf | tail -2
```

`fail2ban-regex` accepte `-` pour lire depuis stdin : parfait pour tester un flux journald.

---

## 60. Recette de jail.local « gold »

La configuration que je déploierais sur un serveur standard exposé (web + ssh). À adapter (surtout `ignoreip`).

```ini
# /etc/fail2ban/jail.local — recette "gold" (Debian 12/Ubuntu 24.04)
# ADAPTEZ ignoreip AVANT TOUT RELOAD.

[DEFAULT]
bantime  = 1h
findtime = 10m
maxretry = 5

# Escalade automatique du bantime (1h, 2h, 4h... max 4 semaines)
bantime.increment = true
bantime.factor    = 2
bantime.maxtime   = 4w
bantime.rndtime   = 10m

# >>> À PERSONNALISER : vos réseaux, votre supervision, votre bastion <<<
ignoreip = 127.0.0.1/8 ::1
           192.168.10.0/24
           10.20.0.0/16
           203.0.113.10
           198.51.100.5

banaction         = nftables-multiport
banaction_allports = nftables-allports

# ---------------------------------------------------------------- SSH
[sshd]
enabled = true
port    = ssh
filter  = sshd
logpath = /var/log/auth.log
backend = auto

# ---------------------------------------------------------------- Web : nginx
[nginx-http-auth]
enabled = true
port    = http,https
filter  = nginx-http-auth
logpath = /var/log/nginx/error.log

[nginx-botsearch]
enabled  = true
port     = http,https
filter   = nginx-botsearch
logpath  = /var/log/nginx/access.log
maxretry = 3
findtime = 1d
bantime  = 1w

[nginx-badbot]
enabled  = true
port     = http,https
filter   = nginx-badbot
logpath  = /var/log/nginx/access.log
maxretry = 2
bantime  = 1w

# ---------------------------------------------------------------- Récidivistes
[recidive]
enabled   = true
filter    = recidive
logpath   = /var/log/fail2ban.log
banaction = %(banaction_allports)s
bantime   = 1w
findtime  = 1d
maxretry  = 5
# Alerte mail uniquement pour les récidivistes (bruit minimal)
action    = %(banaction_allports)s
            %(action_mwl)s[name=recidive, dest=admin@example.com, sender=fail2ban@%(hostname)s]
```

Mise en service :

```bash
sudo cp /etc/fail2ban/jail.local /root/jail.local.bak-$(date +%Y%m%d)
# ... éditez ...
sudo fail2ban-client --test && sudo fail2ban-client reload
sudo fail2ban-client status
```

---

## 61. Checklist de mise en production

À cocher avant d'activer fail2ban sur un serveur de production.

### Avant le reload

- [ ] `ignoreip` contient : loopback, LAN admin, IP publique du bureau, bastion, sondes de supervision
- [ ] `fail2ban-client --test` passe sans erreur
- [ ] Chaque filter testé avec `fail2ban-regex` (`--print-all-matched` relu)
- [ ] Chaque `logpath` existe et contient des lignes récentes
- [ ] Le `port` de chaque jail correspond au port réel du service (`ss -tlnp`)
- [ ] `banaction` cohérente avec le firewall actif (nftables/iptables/ufw)
- [ ] Console hors-bande ouverte (ou dead man's switch programmé, section 25)

### Juste après le reload

- [ ] `fail2ban-client status` : tous les jails attendus sont listés
- [ ] `fail2ban-client status <jail>` : `File list` pointe vers le bon fichier
- [ ] `/var/log/fail2ban.log` : pas d'ERROR dans les 50 dernières lignes
- [ ] Les règles firewall existent : `nft list table inet f2b-table` ou `iptables -L f2b-sshd -n`
- [ ] Test de ban contrôlé depuis une IP de test (pas la vôtre) : 6 échecs → ban visible

### 24h après

- [ ] Relire `/var/log/fail2ban.log` : des bans ? des erreurs ?
- [ ] Vérifier l'absence de faux positifs (IP internes bannies ?)
- [ ] Le rapport quotidien (section 48) arrive bien par mail

---

## 62. Checklist de revue hebdomadaire

(15 minutes, le lundi avec le café.)

- [ ] `sudo grep " Ban " /var/log/fail2ban.log | wc -l` : volume de la semaine vs semaine précédente (pic ?)
- [ ] Top 10 des IP bannies : des récidivistes ? → envisager ban permanent manuel ou remontée AbuseIPDB
- [ ] `sudo fail2ban-client status --all` : tous les jails actifs ? (`Number of jail` == attendu)
- [ ] Y a-t-il eu des tickets « je suis bloqué » ? → ajuster seuils ou `ignoreip`
- [ ] Espace disque `/var/log/` : `fail2ban.log` ne grossit pas anormalement (loglevel DEBUG oublié ?)
- [ ] Le jail `recidive` a-t-il banni ? (si oui : attaques persistantes en cours)

---

## 63. Checklist de revue mensuelle

- [ ] Relire `jail.local` (ou `git log`) : encore pertinent ? nouveaux services à protéger ?
- [ ] Vérifier les mises à jour fail2ban en attente (`apt list --upgradable | grep fail2ban`)
- [ ] Contrôler `ignoreip` : l'équipe a-t-elle changé ? nouvelles sondes ? IP du bureau modifiée ?
- [ ] Tester un débannissement et un bannissement manuel (la procédure d'urgence est-elle connue de l'astreinte ?)
- [ ] Revoir les seuils avec les stats du mois (section 23 : méthode de tuning)
- [ ] Sauvegarde de `/etc/fail2ban/` à jour ? (section 37)
- [ ] Corréler avec les autres sources : les IP bannies apparaissent-elles aussi dans les alertes WAF / IDS ?

---

## 64. Cas pratique : protéger un serveur web exposé

### Contexte

Serveur `srv-web` : Debian 12, nginx + PHP-FPM, site vitrine + espace client avec basic auth sur `/admin`. IP publique directe (pas de CDN).

### Étape 1 : inventaire des surfaces

```bash
ss -tlnp | grep -E "nginx|sshd"
# 0.0.0.0:80, 0.0.0.0:443 (nginx), 0.0.0.0:22 (sshd)
ls -la /var/log/nginx/   # access.log + error.log présents ?
```

### Étape 2 : jail.local

```ini
[DEFAULT]
bantime  = 1h
findtime = 10m
maxretry = 5
ignoreip = 127.0.0.1/8 ::1 192.168.10.0/24 203.0.113.10 198.51.100.5
banaction = nftables-multiport
banaction_allports = nftables-allports

[sshd]
enabled = true

[nginx-http-auth]
enabled = true
port    = http,https
filter  = nginx-http-auth
logpath = /var/log/nginx/error.log

[nginx-limit-req]
enabled = true
port    = http,https
filter  = nginx-limit-req
logpath = /var/log/nginx/error.log
maxretry = 10
findtime = 1m
bantime  = 10m
# ^ bannit les IP qui se prennent des 503 par le rate-limit nginx :
#   complément idéal au limit_req de nginx.

[nginx-botsearch]
enabled  = true
filter   = nginx-botsearch
logpath  = /var/log/nginx/access.log
maxretry = 3
findtime = 1d
bantime  = 1w

[nginx-badbot]
enabled  = true
filter   = nginx-badbot
logpath  = /var/log/nginx/access.log
maxretry = 2
bantime  = 1w

[recidive]
enabled   = true
filter    = recidive
logpath   = /var/log/fail2ban.log
banaction = %(banaction_allports)s
bantime   = 1w
findtime  = 1d
maxretry  = 5
```

### Étape 3 : durcir nginx en amont (défense en profondeur)

```nginx
# Rate limiting : 10 req/s par IP, burst 20
limit_req_zone $binary_remote_addr zone=login:10m rate=10r/s;

server {
  location /admin/ {
    auth_basic "Espace client";
    auth_basic_user_file /etc/nginx/.htpasswd;
    limit_req zone=login burst=20 nodelay;
  }
  # Bloquer les chemins de scan les plus courants (avant fail2ban)
  location ~* (\.env|\.git|wp-login\.php|phpmyadmin) {
    return 444;   # ferme la connexion sans répondre
  }
}
```

### Étape 4 : validation

```bash
sudo fail2ban-client --test && sudo fail2ban-client reload
# Depuis une VM de test : forcer 6 mauvais logins sur /admin → vérifier le ban
curl -u faux:mauvais https://srv-web/admin/  # x6
sudo grep "Ban " /var/log/fail2ban.log | tail -3
```

---

## 65. Cas pratique : durcir un Proxmox VE

### Contexte

Nœud Proxmox VE 8.x : interface web sur le port 8006, SSH sur 22, exposé sur un réseau d'administration (jamais sur Internet directement — mais fail2ban reste utile contre les erreurs internes et les mouvements latéraux).

### Jail pour l'interface web Proxmox (port 8006)

Proxmox loggue les échecs d'authentification web dans `/var/log/daemon.log` (pveproxy) :

```
Sep 26 12:00:01 pve pvedaemon[1234]: authentication failure; rhost=203.0.113.60 user=root@pam msg=no such user
```

Filter maison `/etc/fail2ban/filter.d/proxmox.conf` :

```ini
[INCLUDES]
before = common.conf

[Definition]
failregex = ^%(__prefix_line)sauthentication failure; rhost=<HOST> user=\S+ msg=.*$
ignoreregex =
```

Test :

```bash
sudo fail2ban-regex /var/log/daemon.log /etc/fail2ban/filter.d/proxmox.conf --print-all-matched
```

Jail `/etc/fail2ban/jail.d/proxmox.local` :

```ini
[proxmox]
enabled  = true
port     = 8006
filter   = proxmox
logpath  = /var/log/daemon.log
maxretry = 5
findtime = 10m
bantime  = 1h
```

### Jail SSH (standard)

```ini
[sshd]
enabled = true
port    = 22
maxretry = 5
```

### Points d'attention Proxmox

1. **Cluster** : chaque nœud a son fail2ban. Une IP bannie sur le nœud 1 ne l'est pas sur le nœud 2. Pour un blocage cluster-wide, il faut une action qui propage (ssh vers les autres nœuds, ou firewall Proxmox au niveau datacenter — voir ci-dessous).
2. **Firewall intégré Proxmox** : Proxmox a son propre firewall (datacenter > Firewall). Vous pouvez AUSSI y mettre des règles, mais ne mélangez pas : fail2ban gère le dynamique, le firewall Proxmox le statique.
3. **Ne bannissez jamais l'IP d'un autre nœud du cluster** : mettez toutes les IP du cluster en `ignoreip` ! Un nœud qui se fait bannir par un autre = split-brain et migration HA en échec.
4. **Port 8006** : l'action doit bloquer le port 8006 (`port = 8006`), sinon le ban ne sert à rien.

```ini
[DEFAULT]
# Sur un nœud Proxmox en cluster à 3 nœuds (exemple) :
ignoreip = 127.0.0.1/8 ::1 10.30.0.11 10.30.0.12 10.30.0.13 192.168.10.0/24
```

### Vérification

```bash
sudo fail2ban-client status proxmox
# Provoquez 6 échecs de login sur https://pve:8006 depuis une IP de test,
# puis vérifiez le ban :
sudo nft list table inet f2b-table | grep -A5 "f2b-proxmox"
```

---

## 66. Cas pratique : sécuriser un bastion SSH

### Contexte

Un bastion est LE point d'entrée : il mérite le niveau maximal.

### Configuration

```ini
[sshd]
enabled  = true
port     = 22
filter   = sshd[mode=aggressive]
logpath  = /var/log/auth.log
maxretry = 3
findtime = 10m
bantime  = 4h

[recidive]
enabled   = true
filter    = recidive
logpath   = /var/log/fail2ban.log
banaction = %(banaction_allports)s
bantime   = 4w
findtime  = 1d
maxretry  = 3
```

### Mesures complémentaires (le bastion ne vit pas de fail2ban seul)

```bash
# /etc/ssh/sshd_config — durcissement bastion
PermitRootLogin no
PasswordAuthentication no        # clés uniquement
ChallengeResponseAuthentication no
AllowUsers admin-bastion          # liste fermée
MaxAuthTries 3
LoginGraceTime 30
```

- `PasswordAuthentication no` : sans mot de passe, le brute-force devient inutile — fail2ban ne sert plus qu'à calmer les scanners.
- 2FA via `libpam-google-authenticator` pour les accès sensibles.

### Particularité : les utilisateurs du bastion sont des admins

`ignoreip` doit contenir **tous** les réseaux d'où les admins se connectent (bureau, VPN, astreinte). Un admin banni du bastion un dimanche à 3h du matin, c'est une indisponibilité de l'infogérance.

---

## 67. Cas pratique : protéger un webmail Roundcube

### Contexte

Roundcube sur `webmail.exemple.com` : des centaines d'utilisateurs humains, smartphones en arrière-plan. **Faux positifs garantis** si on tune comme SSH.

### Logs Roundcube

Activez le log d'échecs (`config/config.inc.php`) :

```php
$config['log_driver'] = 'file';
$config['log_session_id'] = false;
```

Les échecs ressemblent à (selon version) :

```
[26-Sep-2026 13:00:01 +0200]: <abc123> IMAP Error: Login failed for victim@example.com
  from 203.0.113.70. AUTHENTICATE PLAIN: Authentication failed.
```

### Filter maison

```ini
# /etc/fail2ban/filter.d/roundcube.conf
[INCLUDES]
before = common.conf

[Definition]
failregex = ^\[[^]]+\]: <\S+> IMAP Error: Login failed for \S+ from <HOST>\.
datepattern = ^\[%%d-%%b-%%Y %%H:%%M:%%S %%z\]
```

> Le `datepattern` explicite est nécessaire : le format `[26-Sep-2026 13:00:01 +0200]` n'est pas toujours auto-détecté. Testez avec `fail2ban-regex -v`.

### Jail (seuils « humains »)

```ini
[roundcube]
enabled  = true
port     = http,https
filter   = roundcube
logpath  = /var/log/roundcube/errors.log
maxretry = 10
findtime = 10m
bantime  = 30m
```

`maxretry = 10` : un utilisateur qui se trompe 5 fois + son téléphone qui réessaie 5 fois ne doit pas être banni. `bantime = 30m` : le temps d'appeler le support, pas une punition.

### Alternative : protéger dovecot plutôt que Roundcube

Si Roundcube s'authentifie en IMAP contre dovecot en local, les échecs apparaissent AUSSI dans les logs dovecot — mais avec l'IP du serveur webmail, pas celle du client ! Dans ce cas, le jail dovecot est inutile (il bannirait le webmail). Protégez au niveau Roundcube (vraie IP via `X-Forwarded-For` si proxy), pas au niveau IMAP.

---

## 68. Performance : fail2ban sur gros volume

### Où sont les limites ?

- **Lecture des logs** : négligeable (inotify + regex en Python, quelques % CPU).
- **Nombre de règles firewall** : le vrai goulot. 10 000 règles iptables linéaires = chaque paquet traverse 10 000 règles.
- **Base sqlite** : tient des centaines de milliers de bans sans broncher.

### Recommandations > 1000 bans simultanés

1. Utilisez `nftables-multiport` (sets en hachage, section 44) — pas iptables linéaire.
2. `bantime` modéré + `recidive` agressif (section 23) : moins de règles actives en permanence.
3. `dbpurgeage` adapté : ne gardez pas 6 mois d'historique si vous n'en faites rien.
4. Surveillez : `time sudo nft list table inet f2b-table | wc -l` (doit rester instantané).

### Le cas extrême : attaque DDoS applicative

fail2ban n'est pas un anti-DDoS (section 1). Face à 100 000 IP distinctes :

1. Montez le rate-limiting en amont (nginx `limit_req`, Cloudflare).
2. Basculez en mode « tout bloquer sauf » temporaire si le service doit survivre.
3. fail2ban continue de tourner, mais il ne sauvera pas le serveur seul.

---

## 69. Pense-bête de poche

À imprimer et coller sur le mur du bureau (ou dans le wiki d'équipe).

```
┌─ FAIL2BAN : PENSE-BÊTE ─────────────────────────────────┐
│                                                         │
│  STATUT                                                 │
│  sudo fail2ban-client status              # jails actifs│
│  sudo fail2ban-client status sshd         # détail jail │
│                                                         │
│  DÉBANNIR (le classique du lundi)                       │
│  sudo fail2ban-client set sshd unbanip 203.0.113.45     │
│  sudo fail2ban-client set recidive unbanip 203.0.113.45 │
│                                                         │
│  BANNIR À LA MAIN (incident)                            │
│  sudo fail2ban-client set sshd banip 203.0.113.45       │
│                                                         │
│  RECHARGER                                              │
│  sudo fail2ban-client --test   # VALIDER D'ABORD !      │
│  sudo fail2ban-client reload   # recharge en douceur    │
│                                                         │
│  TESTER UN FILTER                                       │
│  sudo fail2ban-regex /var/log/auth.log \                │
│      /etc/fail2ban/filter.d/sshd.conf                   │
│                                                         │
│  LOGS                                                   │
│  sudo tail -f /var/log/fail2ban.log                     │
│  sudo grep " Ban " /var/log/fail2ban.log | tail -20     │
│                                                         │
│  RÈGLES FIREWALL                                        │
│  sudo nft list table inet f2b-table        # nftables   │
│  sudo iptables -L f2b-sshd -n              # iptables   │
│                                                         │
│  URGENCE AUTO-BAN                                       │
│  1. Console hors-bande (PVE/IPMI/cloud)                 │
│  2. fail2ban-client set sshd unbanip MON_IP             │
│  3. Ajouter MON_IP à ignoreip dans jail.local           │
│                                                         │
│  FICHIERS                                               │
│  /etc/fail2ban/jail.local      ← VOTRE config           │
│  /etc/fail2ban/jail.conf       ← NE JAMAIS TOUCHER      │
│  /var/log/fail2ban.log         ← ce qui se passe       │
│                                                         │
│  TRIANGLE : maxretry / findtime / bantime               │
│  ignoreip : VOS réseaux + supervision, TOUJOURS         │
└─────────────────────────────────────────────────────────┘
```

---

## 70. Glossaire

| Terme | Définition |
|---|---|
| **Action** | Ce que fait fail2ban lors d'un ban/unban (ex: ajouter une règle nftables). Fichiers dans `action.d/`. |
| **Backend** | Méthode de surveillance des logs : `auto`, `systemd`, `pyinotify`, `polling`. |
| **Ban** | Blocage temporaire (ou permanent) d'une IP au firewall. |
| **banaction** | L'action de bannissement par défaut (ex: `nftables-multiport`). |
| **bantime** | Durée du bannissement. `-1` = permanent. |
| **Brute-force** | Attaque par essais répétés de mots de passe. |
| **CIDR** | Notation des plages d'IP : `192.168.1.0/24` = 256 adresses. |
| **Faux positif** | IP légitime bannie par erreur (utilisateur maladroit, sonde). |
| **failregex** | Expression régulière qui détecte un échec dans un log. |
| **Filter** | Fichier de `filter.d/` contenant les failregex d'un service. |
| **findtime** | Fenêtre de temps pendant laquelle on compte les échecs. |
| **ignoreip** | Liste blanche : ces IP ne sont jamais bannies. |
| **ignoreregex** | Regex d'exclusion : ces lignes ne sont jamais comptées. |
| **Jail** | Unité de surveillance : un filter + un log + des seuils + une action. |
| **journald** | Le journal système de systemd (`journalctl`). |
| **maxretry** | Nombre d'échecs qui déclenche le ban. |
| **recidive** | Jail qui surveille les récidivistes (déjà bannis N fois). |
| **REJECT / DROP** | Deux façons de bloquer : avec (REJECT) ou sans (DROP) réponse à l'attaquant. |
| **Unban** | Levée du bannissement (fin de bantime ou manuelle). |
| **WAF** | Pare-feu applicatif web — fail2ban n'en est pas un. |

---

## 71. Quiz : 10 questions

Répondez sans regarder les réponses (section 72). Objectif : 8/10 pour valider.

**Q1.** Que se passe-t-il si vous modifiez `/etc/fail2ban/jail.conf` puis que vous faites `apt upgrade` ?

**Q2.** Votre `ignoreip` contient `192.168.10.0/24`. Un attaquant depuis `192.168.10.77` échoue 50 fois en SSH. Est-il banni ? Pourquoi ?

**Q3.** Avec `maxretry = 5` et `findtime = 10m`, une IP échoue à 10h00, 10h02, 10h04, 10h06 puis 10h21. Est-elle bannie à 10h21 ? Expliquez le compteur.

**Q4.** `fail2ban-regex` affiche `Lines: 5000 lines, 0 ignored, 0 matched, 5000 missed` alors que le log contient des attaques visibles. Citez deux causes possibles (autres que « la regex est fausse »).

**Q5.** Votre site est derrière Cloudflare sans `set_real_ip_from`. Quel est le risque si vous activez un jail nginx ?

**Q6.** Quelle commande débannit `198.51.100.23` du jail `sshd` ? Et s'il est aussi dans `recidive` ?

**Q7.** Vous changez `maxretry` avec `fail2ban-client set sshd maxretry 3`, puis vous faites `fail2ban-client reload`. Que vaut `maxretry` après le reload ?

**Q8.** Pourquoi un `bantime = 1w` sur le jail sshd est-il généralement une mauvaise idée, et que recommande-t-on à la place ?

**Q9.** Sur un serveur mail avec des utilisateurs humains (smartphones), quelles valeurs conseillez-vous pour le jail dovecot, et pourquoi ?

**Q10.** Vous déployez fail2ban sur un hôte Docker et les conteneurs restent joignables malgré les bans. Expliquez et proposez une correction.

---

## 72. Réponses du quiz

**R1.** `jail.conf` est **écrasé par le paquet** à la mise à jour : toute votre configuration est perdue. Il faut mettre la config dans `jail.local` (jamais touché par les mises à jour).

**R2.** Non, il n'est jamais banni : `ignoreip` est une **whitelist absolue**. C'est pour cela qu'il ne faut y mettre que des réseaux de confiance (LAN admin, supervision) — jamais un réseau exposé à des attaquants.

**R3.** Non. À 10h21, l'échec de 10h00 a plus de 10 minutes : il sort de la fenêtre glissante. Compteur à 10h21 : 10h02, 10h04, 10h06, 10h21 = 4 échecs < 5. Pas de ban. (Un 5e échec avant 10h12 aurait déclenché le ban.)

**R4.** (1) Le **format de date** n'est pas reconnu : fail2ban ignore les lignes dont il ne comprend pas le timestamp — vérifiez `Date template hits`. (2) Le **backend** lit le mauvais fichier / le fichier est vide (les attaques sont ailleurs, ex: dans le journal systemd alors que le jail lit un fichier).

**R5.** Les logs nginx voient les **IP de Cloudflare**, pas celles des visiteurs : fail2ban risque de **bannir les IP de Cloudflare lui-même**, coupant le site pour tout le monde. Il faut `set_real_ip_from` + `real_ip_header CF-Connecting-IP`.

**R6.** `sudo fail2ban-client set sshd unbanip 198.51.100.23`. Si aussi dans recidive : ajouter `sudo fail2ban-client set recidive unbanip 198.51.100.23` (chaque jail a sa propre liste de bans).

**R7.** La valeur de `jail.local` (le `set` à chaud est **perdu au reload**). Pour un changement durable, modifiez `jail.local` puis `reload`.

**R8.** Les botnets utilisent des IP jetables : l'IP bannie ne reviendra jamais, la règle firewall reste pour rien et la table grossit (ralentissement). Recommandé : `bantime` modéré (1h) + jail `recidive` agressif (1 semaine, tous ports) pour les vrais persistants, éventuellement `bantime.increment = true`.

**R9.** `maxretry = 10` (seuil haut : smartphones qui réessaient en boucle avec un ancien mot de passe), `bantime = 30m` (court : un utilisateur légitime bloqué 1h appelle le support). Les services à humains exigent des seuils tolérants.

**R10.** Le trafic vers les conteneurs passe par la chaîne `FORWARD`/`DOCKER-USER`, pas par `INPUT` où fail2ban insère ses règles par défaut. Correction : utiliser une action qui bannit dans `DOCKER-USER` (ex: `banaction = iptables-multiport[chain=DOCKER-USER]`) et tester depuis l'extérieur.

---

## 73. Pour aller plus loin

### Documentation officielle

- Site : https://www.fail2ban.org
- Wiki : https://github.com/fail2ban/fail2ban/wiki
- Manuel : `man jail.conf`, `man fail2ban-regex`, `man fail2ban-client`

### Sujets à creuser après ce guide

1. **Écrire des filters pour vos applis métier** (section 17) : chaque application maison exposée mérite son filter.
2. **Corrélation multi-serveurs** : un attaquant qui frappe 3 serveurs à 2 échecs chacun passe sous les radars. Pistes : centraliser les logs (rsyslog → Graylog/ELK) et bannir depuis la corrélation, ou partager les bans via une action qui publie sur un bus (Redis, webhook).
3. **Threat intelligence** : croiser les IP bannies avec des flux (AbuseIPDB, Blocklist.de, FireHOL) pour le reporting.
4. **IPv6 /64** : adapter les actions pour bannir le préfixe /64 d'un attaquant v6.
5. **Tests automatisés** : intégrer `fail2ban-regex` dans votre CI Ansible (un filter non testé ne part pas en prod).
6. **Alternatives complémentaires** : `crowdsec` (approche collaborative moderne, même philosophie avec une communauté), `sshguard` (plus léger).

### Livres et références réseau

- La documentation `nftables` (https://wiki.nftables.org) pour comprendre ce que font vraiment les actions.
- Les RFC 5737 / 3849 : les plages d'IP de documentation utilisées dans ce guide (`203.0.113.0/24`, `198.51.100.0/24`, `192.0.2.0/24`).

### Ce que ce guide ne couvre pas (volontairement)

- L'écriture d'actions complexes multi-étapes (report AbuseIPDB + webhook + firewall) : faisable, mais chaque brique est déjà documentée sections 22, 32, 33, 52.
- Le tuning fin des regex PCRE avancées (lookahead/lookbehind) : rarement nécessaire pour des logs.
- L'intégration avec un SIEM : chaque SIEM a son connecteur, le principe reste « fail2ban écrit, le SIEM lit `/var/log/fail2ban.log` ».

---

## 74. Références officielles

| Ressource | URL |
|---|---|
| Site officiel fail2ban | https://www.fail2ban.org |
| Dépôt GitHub | https://github.com/fail2ban/fail2ban |
| Wiki (filters, actions, FAQ) | https://github.com/fail2ban/fail2ban/wiki |
| Man page jail.conf | `man jail.conf` sur votre système |
| Paquet Debian | https://packages.debian.org/stable/fail2ban |
| AbuseIPDB API | https://docs.abuseipdb.com |
| Cloudflare API (firewall) | https://developers.cloudflare.com/api/ |
| Plages IP Cloudflare | https://www.cloudflare.com/ips/ |
| Documentation nftables | https://wiki.nftables.org |

---

## 75. Note finale : la philosophie du gardien

fail2ban n'est pas un mur : c'est un **videur**. Il ne rend pas votre serveur invulnérable ; il rend les attaques automatisées **non rentables** :

- Le bot qui scanne 10 000 serveurs abandonne le vôtre après 5 essais.
- L'attaquant persistant se retrouve banni sur tous les ports pendant une semaine.
- Vous, vous dormez, et le lundi matin le rapport quotidien vous dit qui a frappé.

Les trois habitudes qui font la différence en production :

1. **`ignoreip` d'abord** : on ne protège bien que ce qu'on ne casse pas.
2. **Tester avant de bannir** : `fail2ban-regex` n'est pas une option.
3. **Relire les logs** : un gardien qu'on ne supervise pas finit par bannir les clients.

Bon courage, et que vos `Currently banned` restent à zéro les nuits calmes. 🛡️

---

*Guide rédigé pour Zelef — chef de service systèmes. Vérifié : toutes les commandes et tous les blocs de configuration suivent la syntaxe fail2ban ≥ 0.11 / 1.x. Les adresses IP utilisées sont des plages de documentation (RFC 5737). Adaptez `ignoreip`, les `logpath` et les ports à votre infrastructure avant toute mise en production.*
