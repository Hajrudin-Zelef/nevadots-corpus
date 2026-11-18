# Guide Huawei NetEngine AR720 — Routeurs d'entreprise PME/Agences

> **Guide de terrain, pas une brochure.** Ce document couvre le Huawei NetEngine AR720 de A à Z :
> hardware, démarrage, WAN, LAN, NAT, routage, VPN, sécurité, QoS, supervision, dépannage terrain.
> Rédigé en français pour un usage opérationnel (chef de service systèmes & énergies et son équipe).
>
> **Contexte de vérification :** les valeurs constructeur citées viennent de la fiche datasheet
> « Huawei NetEngine AR700 Series Enterprise Router Data Sheet » (AR720 / AR730).
> La version VRP exacte du firmware varie selon le lot et le fournisseur — **à vérifier sur la fiche
> du modèle exact** livré (commande `display version` une fois le routeur en main).
> Quand une valeur est incertaine ou dépend de la version, c'est écrit explicitement.

---

## Sommaire

- [Partie A — Prise en main](#partie-a--prise-en-main)
- [Partie B — WAN : accès Internet](#partie-b--wan--accès-internet)
- [Partie C — LAN et services locaux](#partie-c--lan-et-services-locaux)
- [Partie D — NAT](#partie-d--nat)
- [Partie E — Routage](#partie-e--routage)
- [Partie F — VPN](#partie-f--vpn)
- [Partie G — Sécurité](#partie-g--sécurité)
- [Partie H — QoS](#partie-h--qos)
- [Partie I — Supervision](#partie-i--supervision)
- [Partie J — Exploitation : sauvegarde, firmware, licences](#partie-j--exploitation--sauvegarde-firmware-licences)
- [Partie K — Dépannage terrain (15+ cas)](#partie-k--dépannage-terrain-15-cas-détaillés)
- [Partie L — Maintenance préventive](#partie-l--maintenance-préventive)
- [Partie M — Durcissement](#partie-m--durcissement)
- [Partie N — Cas pratiques complets commentés](#partie-n--cas-pratiques-complets-commentés)
- [Partie O — Pense-bête de poche](#partie-o--pense-bête-de-poche)
- [Partie P — Glossaire](#partie-p--glossaire)
- [Partie Q — Quiz](#partie-q--quiz-10-questions)
- [Partie R — Pour aller plus loin](#partie-r--pour-aller-plus-loin)

---

# Partie A — Prise en main

## 1. Positionnement du produit : c'est quoi, l'AR720 ?

Le Huawei NetEngine AR720 est un **routeur de branche d'entreprise** (segment PME / agences /
boutiques / établissements scolaires), dans la série NetEngine AR700.
Il intègre sur une seule plateforme 1U : routage, commutation, firewall, VPN, NAT, contrôleur
Wi-Fi (gestion d'AP Huawei via CAPWAP), et options 4G/5G/voix via cartes d'extension.

Concrètement, sur le terrain : c'est la boîte qu'on pose dans une agence pour sortir sur
Internet, monter des tunnels VPN vers le siège, faire du NAT, du DHCP, du filtrage, et
éventuellement basculer sur une 4G en secours. Le tout piloté par **VRP** (Versatile
Routing Platform), le même OS que les gros routeurs Huawei — donc la syntaxe CLI qu'on
apprend ici se réutilise sur du NetEngine 8000.

## 2. Caractéristiques hardware officielles (datasheet AR700)

| Élément | Valeur constructeur |
|---|---|
| Processeur | ARM64 4 cœurs, 1,4 GHz |
| Performance de commutation | 9 à 25 Mpps |
| Débit sortant (forwarding) | 4 Gbit/s |
| Nombre de terminaux connectés (capacité) | 700 |
| Ports WAN fixes | 2 x GE combo (RJ45 cuivre / SFP optique) |
| Ports LAN fixes | 8 x GE électriques RJ45 (configurables en ports WAN) |
| Slots SIC (Smart Interface Card) | 2 |
| Slot WSIC | 0 par défaut / 1 maximum |
| Slot MIC / XSIC | 0 / 0 (non supportés sur l'AR720) |
| Port console | 1 x RJ45 console série |
| Ports USB | 2 x USB 2.0 |
| Mémoire RAM | 4 Go |
| Flash | 1 Go (512 Mo utilisables client, note datasheet) |
| Hot swapping | Supporté |
| Châssis | 1U, 44,4 x 442,0 x 220,4 mm, 3,2 kg |
| Alimentation | AC intégrée, 100–240 V, 50/60 Hz (plage max 90–264 V, 47–63 Hz) |
| Consommation | 33 W max (28 W typique) |
| Ventilation | Module ventilateur intégré non extractible (built-in unpluggable), flux gauche → droite |
| Bruit | 52,3 dB(A) |
| Température de fonctionnement | 0 °C à 45 °C |
| Humidité | 5 % à 95 % sans condensation |
| Alimentation redondante | Non supportée |
| PoE | Non supporté |

Note terrain : 4 Gbit/s de débit sortant, c'est **la capacité de la plateforme**, pas un débit
garanti avec tous les services activés. Dès qu'on active firewall + IPS + VPN + QoS en même
temps, le débit utile réel chute. Dimensionner avec de la marge (viser 50–60 % de la capacité
en charge nominale).

## 3. Ce que le « combo » WAN veut dire en pratique

Un port « combo » = une paire RJ45 cuivre + cage SFP qui partagent **la même interface logique**.
On ne peut utiliser qu'un seul des deux médias à la fois pour un port donné.

- `GE0/0/0` : WAN1 combo (RJ45 ou SFP).
- `GE0/0/1` : WAN2 combo (RJ45 ou SFP).
- `GE0/0/2` à `GE0/0/9` : les 8 ports LAN RJ45.

La commutation du média sur un port combo se fait en général en CLI (`combo enable` ou
équivalent selon la version VRP — **à vérifier sur la fiche du modèle exact**, la syntaxe
varie selon les versions V300).

Piège classique : brancher un RJ45 sur le port WAN1 alors que le SFP est déjà inséré et actif —
ou l'inverse. Si le lien ne monte pas, vérifier d'abord quel média est actif :

```
display interface GigabitEthernet 0/0/0
```

## 4. Les slots d'extension SIC : à quoi ça sert

Les 2 slots SIC (Smart Interface Card) acceptent des cartes filles. Typiquement sur la série
AR (à valider avec le catalogue du fournisseur pour l'AR720, **à vérifier sur la fiche du
modèle exact**) :

- cartes 4G LTE / 5G (modem cellulaire avec slot SIM) — très utile en backup WAN ;
- cartes E1/T1 (liaisons louées classiques) ;
- cartes voix (FXS/FXO) ;
- cartes GE supplémentaires ou modules fibre.

Le hot swapping est supporté : on peut insérer/retirer une carte à chaud. Règle de prudence
terrain : on ne fait jamais ça en pleine production sans fenêtre de maintenance, même si le
constructeur l'autorise. Un retrait brutal peut faire tomber la carte et perturber le slot.

Le slot WSIC (0 par défaut, 1 max) correspond aux cartes Wi-Fi intégrées (l'AR720 peut
embarquer un point d'accès Wi-Fi via carte WSIC selon la référence commerciale — **à vérifier
sur la fiche du modèle exact**).

## 5. Face avant / face arrière : ce qu'on y trouve

**Face avant (panneau de ports) :**

- 2 ports combo WAN (étiquetés WAN1 / WAN2, chacun avec RJ45 + cage SFP).
- 8 ports LAN RJ45 (étiquetés LAN1…LAN8 ou GE0/0/2…GE0/0/9).
- 1 port console RJ45.
- 2 ports USB (USB 2.0).
- LED d'état : SYS (système), PWR (alimentation), et LED par port (link/act).

**Face arrière :**

- Prise d'alimentation AC (cordons standard).
- Sortie d'air (le flux va de gauche à droite — ne pas coller le routeur contre un mur à gauche,
  laisser de l'espace pour l'entrée d'air).

**Lecture des LED (convention Huawei classique) :**

| LED | État | Signification |
|---|---|---|
| PWR | Vert fixe | Alimentation OK |
| PWR | Éteinte | Pas d'alimentation |
| SYS | Vert clignotant lentement | Démarrage en cours / fonctionnement normal |
| SYS | Rouge | Panne système ou boot échoué |
| Port | Vert fixe | Lien établi |
| Port | Vert clignotant | Lien + trafic |
| Port | Éteinte | Pas de lien |

Règle d'or du premier diagnostic : **SYS rouge + aucun port allumé = problème de boot**
(firmware corrompu, clé USB de secours à préparer). SYS vert + port éteint = problème de
câble / média combo / négociation auto.

## 6. Alimentation et environnement : les contraintes à respecter

- Alimentation AC intégrée unique, **non redondante**. Sur un site critique, on protège en
  amont : onduleur (voir le guide onduleurs/UPS) + parafoudre. Le routeur ne fait que 33 W
  max : un petit UPS 1 kVA tient largement.
- Plage de tension large (90–264 V) : tolère bien les réseaux instables, mais pas les
  micro-coupures — d'où l'onduleur.
- Température 0–45 °C : dans une armoire technique sans clim en zone chaude, ça se surveille.
  Prévoir une sonde ou au minimum la supervision SNMP de température si disponible.
- Humidité 5–95 % sans condensation : éviter les locaux avec climatisation qui condense
  directement sur l'équipement.
- Flux d'air gauche → droite : dans un rack, ne pas mettre un équipement qui souffle chaud
  directement sur l'entrée d'air gauche de l'AR720.

## 7. La console : câble, paramètres, premier contact

Le port console est un RJ45 série. Il faut un câble console RJ45 ↔ DB9 (ou USB-série selon
le PC). Paramètres du terminal :

| Paramètre | Valeur |
|---|---|
| Débit | 9600 bauds |
| Bits de données | 8 |
| Parité | Aucune |
| Bits de stop | 1 |
| Contrôle de flux | Aucun |

Outils : PuTTY (Windows), `screen /dev/ttyUSB0 9600` ou `minicom` (Linux).

Au premier boot, VRP demande la configuration initiale (assistant). **Ne jamais laisser le
routeur avec le mot de passe par défaut ou sans mot de passe**, même 5 minutes : en DHCP
client sur le WAN, il peut récupérer une IP publique et être scanné dans la minute.

## 8. Architecture logicielle : VRP en deux mots

VRP (Versatile Routing Platform) est l'OS de Huawei, équivalent d'IOS chez Cisco ou JunOS
chez Juniper. Concepts de base :

- **Vues (views)** : la CLI est hiérarchique. `<AR720>` = vue utilisateur (user view),
  `[AR720]` = vue système (system view), puis des sous-vues par fonction
  (`[AR720-GigabitEthernet0/0/0]`, `[AR720-ike-proposal-1]`, etc.).
- `system-view` : passer en vue système (depuis `<AR720>`, taper `system-view` ou `sys`).
- `quit` : remonter d'un niveau. `return` : revenir directement en vue utilisateur
  (raccourci `Ctrl+Z`).
- `display` : commandes d'affichage (équivalent `show` chez Cisco). Ex. `display
  current-configuration`.
- `?` : aide contextuelle partout. `Tab` : complétion.
- La configuration se fait en **mode candidat implicite** : les commandes prennent effet
  immédiatement mais ne sont **pas sauvegardées** tant qu'on n'a pas fait `save`.
  Oublier `save` = perdre la config au reboot. C'est l'erreur n°1 des débutants.

## 9. Premier boot : procédure pas à pas

1. Brancher le câble console, ouvrir le terminal (9600 8N1).
2. Brancher l'alimentation. Observer les LED : PWR vert, SYS clignote puis se stabilise.
3. À l'invite, répondre à l'assistant de configuration initiale **ou** le quitter pour
   configurer à la main (recommandé : plus de contrôle).
4. Définir le nom d'hôte :
   ```
   <AR720>system-view
   [AR720]sysname AGENCE-DAKAR-AR720
   ```
5. Configurer le mot de passe console + activer l'authentification.
6. Configurer l'utilisateur admin pour SSH/telnet (voir section 14).
7. Configurer la date/heure (`clock timezone`, `clock datetime` ou NTP — voir Partie I).
8. Configurer le WAN (Partie B), le LAN (Partie C), le NAT (Partie D).
9. **`save`** puis vérifier avec `display saved-configuration` vs `display
   current-configuration`.

## 10. Sécuriser l'accès console et le mot de passe enable

```
[AGENCE-DAKAR-AR720]user-interface console 0
[AGENCE-DAKAR-AR720-ui-console0]authentication-mode password
[AGENCE-DAKAR-AR720-ui-console0]set authentication password cipher MotDePasseConsoleFictif123
[AGENCE-DAKAR-AR720-ui-console0]idle-timeout 10 0
[AGENCE-DAKAR-AR720-ui-console0]quit
```

- `idle-timeout 10 0` : déconnexion après 10 minutes d'inactivité (minutes, secondes).
- Toujours `cipher` (mot de passe chiffré dans la config), jamais en clair.
- Mot de passe = fictif dans ce guide, évidemment. En production : long, unique par site,
  stocké dans le coffre de l'équipe.

## 11. Créer l'utilisateur admin (SSH)

```
[AGENCE-DAKAR-AR720]aaa
[AGENCE-DAKAR-AR720-aaa]local-user admin password cipher MotDePasseAdminFictif456
[AGENCE-DAKAR-AR720-aaa]local-user admin privilege level 15
[AGENCE-DAKAR-AR720-aaa]local-user admin service-type ssh telnet terminal
[AGENCE-DAKAR-AR720-aaa]quit
[AGENCE-DAKAR-AR720]stelnet server enable
[AGENCE-DAKAR-AR720]ssh user admin authentication-type password
[AGENCE-DAKAR-AR720]user-interface vty 0 4
[AGENCE-DAKAR-AR720-ui-vty0-4]authentication-mode aaa
[AGENCE-DAKAR-AR720-ui-vty0-4]protocol inbound ssh
[AGENCE-DAKAR-AR720-ui-vty0-4]idle-timeout 10 0
[AGENCE-DAKAR-AR720-ui-vty0-4]quit
```

Note : `telnet` dans `service-type` est montré pour compatibilité, mais **désactiver telnet
en production** (voir Partie M). SSH uniquement.

## 12. Nom d'hôte, fuseau horaire, horloge

```
[AGENCE-DAKAR-AR720]sysname AGENCE-DAKAR-AR720
[AGENCE-DAKAR-AR720]clock timezone Dakar add 00:00
[AGENCE-DAKAR-AR720]clock datetime 10:30:00 2026-09-27
```

Le fuseau horaire s'écrit avec un nom libre + décalage. Pour une synchro fiable, configurer
NTP (voir section 70). Une horloge fausse = logs inexploitables, certificats VPN qui
échouent, dépannage impossible. C'est un détail qui coûte cher.

## 13. Les fichiers du système : vrpcfg.zip, firmware, USB

En vue utilisateur :

```
<AGENCE-DAKAR-AR720>dir
<AGENCE-DAKAR-AR720>display version
<AGENCE-DAKAR-AR720>display startup
```

- `display version` : version VRP, uptime, modèle, mémoire. **Première commande à donner
  quand on ouvre un ticket** ou qu'on demande de l'aide.
- `display startup` : quel fichier de config et quel firmware sont chargés au boot.
- `dir` : liste les fichiers (config `vrpcfg.zip`, image système `.cc`).
- Les 2 ports USB servent à : mettre à jour le firmware depuis une clé, sauvegarder la
  config, ou booter en secours si la flash est corrompue.

Convention de nommage des firmwares Huawei : `AR720-V300R024C00SPC100.cc` (format
indicatif — **à vérifier sur la fiche du modèle exact** et sur le portail de téléchargement
avec le numéro de série).

## 14. Upgrade initiale : mettre le firmware à niveau avant la mise en service

Règle : **on ne met jamais en production un équipement avec le firmware d'usine** sans
vérifier qu'il n'y a pas une version corrective connue. Procédure type :

1. Télécharger la version cible depuis le portail Huawei (compte + contrat de support
   requis en général).
2. Copier sur clé USB (FAT32), insérer dans le routeur.
3. En vue utilisateur :
   ```
   <AGENCE-DAKAR-AR720>dir usb0:
   <AGENCE-DAKAR-AR720>copy usb0:/AR720-V300R0XXCXX.cc flash:/
   ```
4. Déclarer la nouvelle image comme image de démarrage :
   ```
   <AGENCE-DAKAR-AR720>startup system-software AR720-V300R0XXCXX.cc
   ```
5. Vérifier : `display startup`.
6. **`save`** la configuration actuelle avant tout reboot.
7. `reboot` (confirmer). Observer le boot en console.
8. Après reboot : `display version` pour confirmer, puis re-tester WAN/LAN/VPN.

**Rollback** : garder l'ancienne image en flash (il y a 512 Mo utilisables, ça passe en
général pour 2 images). Si la nouvelle version pose problème :
`startup system-software <ancienne-image>` + `reboot`. Ne jamais effacer l'ancienne image
avant d'avoir validé la nouvelle en production pendant au moins 48 h.

## 15. Sauvegarder la configuration (la base de tout)

```
<AGENCE-DAKAR-AR720>save
```

Répondre `y` aux deux questions (écraser le fichier de config). Vérification :

```
<AGENCE-DAKAR-AR720>display saved-configuration
<AGENCE-DAKAR-AR720>display current-configuration
```

Les deux doivent être identiques après un `save`. Pour une sauvegarde externe :

```
<AGENCE-DAKAR-AR720>copy vrpcfg.zip usb0:/backup-AGENCE-DAKAR-2026-09-27.zip
```

Ou via TFTP/FTP vers un serveur (voir Partie J). Fréquence minimale : après chaque
changement + sauvegarde hebdomadaire automatisée.

---
---

# Partie B — WAN : accès Internet

## 16. Comprendre les interfaces WAN de l'AR720

- `GigabitEthernet0/0/0` et `GigabitEthernet0/0/1` : les 2 ports combo WAN.
- `GigabitEthernet0/0/2` à `GigabitEthernet0/0/9` : les 8 ports LAN, **reconfigurables en
  WAN** si besoin (utile pour un 3e lien ou un lien dédié).
- Chaque interface physique peut porter des **sous-interfaces** (VLAN tagging) :
  `GigabitEthernet0/0/0.10`.

Convention d'adressage dans ce guide :

| Usage | Interface | IP / mode |
|---|---|---|
| WAN principal (fibre/ADSL) | GE0/0/0 | PPPoE ou DHCP ou statique selon FAI |
| WAN secours (4G ou 2e FAI) | GE0/0/1 | DHCP client |
| LAN | GE0/0/2…9 (ou Vlanif) | 192.168.10.0/24 |

Adapter à chaque site. Noter le plan d'adressage **avant** de configurer.

## 17. WAN en PPPoE (client) — le cas ADSL/fibre avec identifiants FAI

Le PPPoE se configure via une **interface Dialer**. Étapes :

1. Créer l'interface Dialer avec login/mot de passe.
2. Lier l'interface physique au Dialer (`pppoe-client dial-bundle-number`).
3. NAT sortant sur le Dialer + route par défaut.

```
[AGENCE-DAKAR-AR720]dialer-rule
[AGENCE-DAKAR-AR720-dialer-rule]dialer-rule 1 ip permit
[AGENCE-DAKAR-AR720-dialer-rule]quit
[AGENCE-DAKAR-AR720]interface Dialer 1
[AGENCE-DAKAR-AR720-Dialer1]dialer user agence.dakar@fai.exemple
[AGENCE-DAKAR-AR720-Dialer1]dialer-group 1
[AGENCE-DAKAR-AR720-Dialer1]dialer bundle 1
[AGENCE-DAKAR-AR720-Dialer1]ppp chap user agence.dakar@fai.exemple
[AGENCE-DAKAR-AR720-Dialer1]ppp chap password cipher MotDePasseFAIFictif
[AGENCE-DAKAR-AR720-Dialer1]ppp pap local-user agence.dakar@fai.exemple password cipher MotDePasseFAIFictif
[AGENCE-DAKAR-AR720-Dialer1]ip address ppp-negotiate
[AGENCE-DAKAR-AR720-Dialer1]nat outbound 2000
[AGENCE-DAKAR-AR720-Dialer1]quit
[AGENCE-DAKAR-AR720]interface GigabitEthernet 0/0/0
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/0]pppoe-client dial-bundle-number 1
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/0]quit
[AGENCE-DAKAR-AR720]ip route-static 0.0.0.0 0.0.0.0 Dialer 1
```

Points d'attention :

- `dialer user` + `ppp chap user` : certains FAI n'utilisent que CHAP, d'autres que PAP.
  Configurer les deux ne gêne pas (négociation).
- `ip address ppp-negotiate` : l'IP est attribuée par le FAI.
- Si le FAI impose un VLAN (ex. fibre avec VLAN 10 sur l'ONT) : créer une sous-interface
  `GE0/0/0.10` avec `dot1q termination vid 10` et y attacher le client PPPoE.
- Vérification : `display pppoe-client session summary`, `display ip interface brief`.

## 18. PPPoE sur sous-interface VLAN (fibre avec tag FAI)

```
[AGENCE-DAKAR-AR720]interface GigabitEthernet 0/0/0.10
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/0.10]dot1q termination vid 10
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/0.10]pppoe-client dial-bundle-number 1
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/0.10]arp broadcast enable
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/0.10]quit
```

Ne pas oublier `arp broadcast enable` sur les sous-interfaces dot1q, sinon certains flux
ne passent pas. C'est un classique des tickets « ça marche en direct sur l'ONT mais pas
derrière le routeur ».

## 19. WAN en DHCP client (box FAI / modem en mode pont+DHCP)

```
[AGENCE-DAKAR-AR720]interface GigabitEthernet 0/0/0
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/0]ip address dhcp-alloc
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/0]nat outbound 2000
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/0]quit
```

La route par défaut est en général poussée par le serveur DHCP du FAI. Vérifier :

```
display ip routing-table
```

Si la route par défaut n'apparaît pas, l'ajouter à la main (voir section suivante).
Astuce : noter l'IP/gateway obtenues (`display ip interface brief`) pour le dossier du site.

## 20. WAN en IP statique (liaison louée / IP fixe FAI)

```
[AGENCE-DAKAR-AR720]interface GigabitEthernet 0/0/1
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/1]ip address 197.155.10.34 255.255.255.252
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/1]nat outbound 2000
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/1]quit
[AGENCE-DAKAR-AR720]ip route-static 0.0.0.0 0.0.0.0 197.155.10.33
```

(IP fictives — remplacer par celles du FAI.) Avec une IP statique, on maîtrise le NAT
entrant (NAT server) et les tunnels VPN : c'est le mode préféré pour un site qui héberge
des services ou termine des VPN.

## 21. Multi-WAN avec basculement : le principe

Deux liens WAN = un principal + un secours. Le basculement repose sur :

1. **Deux routes par défaut** avec des préférences différentes (la plus petite préférence
   gagne).
2. **NQA** (sonde de connectivité) qui détecte la panne réelle du lien principal
   (pas seulement la perte du lien physique).
3. **Track** qui lie la route statique au résultat NQA : si la sonde échoue, la route
   principale est retirée et la route de secours prend le relais.

Sans NQA, une panne « lien physique OK mais pas d'Internet » (panne chez le FAI) ne serait
pas détectée : le routeur garderait la route principale et tout serait coupé. C'est LE
point à ne pas rater.

## 22. Multi-WAN : configuration complète (PPPoE principal + 4G/DHCP secours)

```
[AGENCE-DAKAR-AR720]nqa test-instance admin pppoe-check
[AGENCE-DAKAR-AR720-nqa-admin-pppoe-check]test-type icmp
[AGENCE-DAKAR-AR720-nqa-admin-pppoe-check]destination-address ipv4 8.8.8.8
[AGENCE-DAKAR-AR720-nqa-admin-pppoe-check]frequency 10
[AGENCE-DAKAR-AR720-nqa-admin-pppoe-check]probe-count 3
[AGENCE-DAKAR-AR720-nqa-admin-pppoe-check]timeout 2
[AGENCE-DAKAR-AR720-nqa-admin-pppoe-check]start now
[AGENCE-DAKAR-AR720-nqa-admin-pppoe-check]quit
[AGENCE-DAKAR-AR720]track 1 nqa admin pppoe-check
[AGENCE-DAKAR-AR720]ip route-static 0.0.0.0 0.0.0.0 Dialer 1 track 1
[AGENCE-DAKAR-AR720]ip route-static 0.0.0.0 0.0.0.0 192.168.8.1 preference 100
```

Explications ligne par ligne :

- `nqa test-instance admin pppoe-check` : crée une sonde nommée.
- `test-type icmp` + `destination-address 8.8.8.8` : ping régulier vers une cible fiable.
  (En production, préférer une cible du FAI ou du siège plutôt qu'un DNS public.)
- `frequency 10` : un test toutes les 10 s. `probe-count 3` : 3 pings par test.
- `track 1 nqa ...` : l'objet track 1 suit l'état de la sonde.
- Route principale via Dialer 1 **liée au track** : si la sonde tombe, la route est retirée.
- Route de secours via la passerelle du lien 2 (ici 192.168.8.1, la box 4G) avec
  préférence 100 (moins prioritaire que la préférence par défaut 60).

Vérification :

```
display nqa results test-instance admin pppoe-check
display track 1
display ip routing-table
```

## 23. Basculement : tester avant de déclarer « ça marche »

Procédure de test obligatoire en fenêtre de maintenance :

1. Noter la route active : `display ip routing-table | include 0.0.0.0`.
2. Débrancher le câble du WAN principal (ou couper la session PPPoE :
   `reset pppoe-client session` — attention, commande en vue utilisateur).
3. Attendre ~30 s, vérifier que la route de secours est active et qu'un ping sort.
4. Rebrancher, vérifier le retour sur le principal (le track remonte, la route
   préférée reprend).
5. Mesurer le temps de coupure réel avec des pings continus depuis un poste.

Noter le temps de basculement dans le dossier du site. Objectif raisonnable : < 60 s.
Si c'est plus long, resserrer `frequency`/`probe-count` (au prix de plus de trafic de
sonde).

## 24. WAN 4G/5G via carte SIC : principes

Si l'AR720 est équipé d'une carte cellulaire SIC (modem 4G LTE ou 5G — **à vérifier sur la
fiche du modèle exact** pour la référence de carte compatible) :

- L'interface apparaît en général comme `Cellular0/0/0` (slot 0) ou similaire.
- Insérer la carte SIM (routeur éteint de préférence, même si le hot swap est supporté).
- Configurer le profil APN du opérateur, le code PIN si nécessaire, puis une interface
  Dialer ou une IP négociée selon le mode du modem.
- Le lien cellulaire sert quasi toujours de **secours** (coût au volume, latence variable).

Exemple de trame (noms d'interface à adapter au modèle de carte réel) :

```
[AGENCE-DAKAR-AR720]interface Cellular 0/0/0
[AGENCE-DAKAR-AR720-Cellular0/0/0]apn-profile apn-internet
[AGENCE-DAKAR-AR720-Cellular0/0/0]quit
[AGENCE-DAKAR-AR720]apn-profile apn-internet
[AGENCE-DAKAR-AR720-apn-profile-apn-internet]apn internet.operateur.exemple
[AGENCE-DAKAR-AR720-apn-profile-apn-internet]quit
```

Vérifier le signal : `display cellular 0/0/0` (niveau RSRP/SINR — viser un montage
d'antenne correct, éviter de laisser le routeur au fond d'une armoire métallique fermée).

## 25. MTU/MSS sur le WAN : le détail qui casse les VPN

Le PPPoE réduit la MTU utile (1492 au lieu de 1500, à cause de l'en-tête PPPoE). Sans
ajustement, certains sites web ne chargent pas et les tunnels VPN rament.

```
[AGENCE-DAKAR-AR720]interface Dialer 1
[AGENCE-DAKAR-AR720-Dialer1]tcp adjust-mss 1400
[AGENCE-DAKAR-AR720-Dialer1]quit
```

Règle pratique : MSS = MTU du lien − 100 environ (1400 pour PPPoE, 1440 pour de l'IP
classique derrière NAT). À ajuster si des applications spécifiques coincent.

---
---

# Partie C — LAN et services locaux

## 26. Adressage du LAN : plan et configuration

Plan type d'une agence (à adapter, mais **documenter et ne plus changer**) :

| Réseau | Usage | Interface |
|---|---|---|
| 192.168.10.0/24 | LAN bureautique | GE0/0/2 (ou Vlanif10) |
| 192.168.20.0/24 | Wi-Fi invités | Vlanif20 |
| 192.168.30.0/24 | Téléphonie / imprimantes | Vlanif30 |
| 192.168.99.0/24 | Management | Vlanif99 |

Configuration de base :

```
[AGENCE-DAKAR-AR720]interface GigabitEthernet 0/0/2
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/2]ip address 192.168.10.1 255.255.255.0
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/2]quit
```

Ou en mode VLAN (recommandé dès qu'il y a plusieurs réseaux) :

```
[AGENCE-DAKAR-AR720]vlan 10
[AGENCE-DAKAR-AR720-vlan10]quit
[AGENCE-DAKAR-AR720]interface Vlanif 10
[AGENCE-DAKAR-AR720-Vlanif10]ip address 192.168.10.1 255.255.255.0
[AGENCE-DAKAR-AR720-Vlanif10]quit
[AGENCE-DAKAR-AR720]interface GigabitEthernet 0/0/2
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/2]port link-type access
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/2]port default vlan 10
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/2]quit
```

## 27. Serveur DHCP : pool de base

```
[AGENCE-DAKAR-AR720]dhcp enable
[AGENCE-DAKAR-AR720]ip pool lan-bureautique
[AGENCE-DAKAR-AR720-ip-pool-lan-bureautique]gateway-list 192.168.10.1
[AGENCE-DAKAR-AR720-ip-pool-lan-bureautique]network 192.168.10.0 mask 255.255.255.0
[AGENCE-DAKAR-AR720-ip-pool-lan-bureautique]dns-list 192.168.10.1 8.8.8.8
[AGENCE-DAKAR-AR720-ip-pool-lan-bureautique]excluded-ip-address 192.168.10.1 192.168.10.20
[AGENCE-DAKAR-AR720-ip-pool-lan-bureautique]lease day 1 hour 0 minute 0
[AGENCE-DAKAR-AR720-ip-pool-lan-bureautique]quit
[AGENCE-DAKAR-AR720]interface Vlanif 10
[AGENCE-DAKAR-AR720-Vlanif10]dhcp select global
[AGENCE-DAKAR-AR720-Vlanif10]quit
```

- `dhcp enable` : activer le service DHCP **globalement** (oubli classique).
- `excluded-ip-address` : réserver la plage des équipements fixes (imprimantes, serveurs).
- `lease day 1` : bail d'un jour (réseau bureautique stable). Pour un Wi-Fi invités :
  bail court (quelques heures).

## 28. DHCP : réservations (baux statiques) pour imprimantes et équipements

Les copieurs/imprimantes réseau et les équipements fixes doivent avoir une IP stable.
Deux méthodes : réservation DHCP (recommandé, centralisé) ou IP fixe sur l'équipement.

```
[AGENCE-DAKAR-AR720]ip pool lan-bureautique
[AGENCE-DAKAR-AR720-ip-pool-lan-bureautique]static-bind ip-address 192.168.10.21 mac-address aaaa-bbbb-cccc
[AGENCE-DAKAR-AR720-ip-pool-lan-bureautique]quit
```

Format MAC Huawei : `aaaa-bbbb-cccc`. Vérifier les baux attribués :

```
display ip pool name lan-bureautique used
```

Astuce terrain : tenir un tableau (dans le dossier du site) : IP / MAC / équipement /
emplacement. Quand l'imprimante « disparaît » du réseau, c'est le premier document qu'on
ouvre.

## 29. DHCP : options avancées (option 43, 66, 150…)

Les options DHCP servent à pousser des paramètres spécifiques : serveur TFTP pour des
téléphones IP (option 66/150), contrôleur Wi-Fi (option 43), etc.

```
[AGENCE-DAKAR-AR720]ip pool lan-telephonie
[AGENCE-DAKAR-AR720-ip-pool-lan-telephonie]option 66 ip-address 192.168.30.5
[AGENCE-DAKAR-AR720-ip-pool-lan-telephonie]quit
```

Pour des options brutes (hexadécimal), utiliser `option <code> hex <valeur>` — **syntaxe
à vérifier selon la version VRP**, les options constructeur (ex. option 43 Huawei pour
les AP) ont des formats spécifiques documentés dans le guide WLAN.

## 30. DNS : le routeur comme relais + DNS statiques

Faire du routeur le DNS du LAN simplifie la vie (un seul point à changer si les DNS du
FAI changent) :

```
[AGENCE-DAKAR-AR720]dns resolve
[AGENCE-DAKAR-AR720]dns server 8.8.8.8
[AGENCE-DAKAR-AR720]dns server 1.1.1.1
[AGENCE-DAKAR-AR720]interface Vlanif 10
[AGENCE-DAKAR-AR720-Vlanif10]dhcp select global
```

(avec `dns-list 192.168.10.1 ...` dans le pool, cf. section 27 : les clients interrogent
le routeur, qui relaie.)

Entrées statiques utiles (intranet, imprimantes) :

```
[AGENCE-DAKAR-AR720]ip host imprimante-accueil 192.168.10.21
[AGENCE-DAKAR-AR720]ip host intranet-siege 10.0.0.10
```

Vérification : `display dns dynamic-host` / `ping imprimante-accueil`.

## 31. Relais DHCP (quand le serveur est ailleurs)

Si le serveur DHCP est au siège (ou sur un serveur Windows du site), le routeur fait
relais :

```
[AGENCE-DAKAR-AR720]dhcp enable
[AGENCE-DAKAR-AR720]interface Vlanif 20
[AGENCE-DAKAR-AR720-Vlanif20]dhcp select relay
[AGENCE-DAKAR-AR720-Vlanif20]dhcp relay server-ip 10.0.0.5
[AGENCE-DAKAR-AR720-Vlanif20]quit
```

Cas typique : Wi-Fi invités avec portail captif hébergé ailleurs, ou centralisation des
baux DHCP au siège pour audit.

## 32. VLAN : segmentation minimale recommandée

Même sur une petite agence, segmenter en 3 VLAN change tout pour la sécurité :

| VLAN | Nom | Réseau | Règle |
|---|---|---|---|
| 10 | Bureautique | 192.168.10.0/24 | Accès Internet + VPN siège |
| 20 | Invités | 192.168.20.0/24 | Internet seul, isolé du reste |
| 30 | Équipements | 192.168.30.0/24 | Imprimantes, caméras : pas d'Internet direct |

```
[AGENCE-DAKAR-AR720]vlan batch 10 20 30
[AGENCE-DAKAR-AR720]interface GigabitEthernet 0/0/3
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/3]port link-type trunk
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/3]port trunk allow-pass vlan 10 20 30
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/3]quit
```

Le filtrage inter-VLAN se fait ensuite avec les zones de sécurité (Partie G).

## 33. STP et boucles : le minimum vital

Sur les ports d'accès vers les PC, activer **STP edge** (portfast) pour une montée de
lien rapide, et **BPDU protection** pour couper net tout port qui reçoit des BPDU
(quelqu'un qui branche un petit switch sauvage) :

```
[AGENCE-DAKAR-AR720]stp enable
[AGENCE-DAKAR-AR720]interface GigabitEthernet 0/0/2
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/2]stp edged-port enable
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/2]quit
[AGENCE-DAKAR-AR720]stp bpdu-protection
```

Une boucle de switching = tempête de broadcast = réseau mort en quelques secondes.
Sur le terrain, 90 % des « le réseau est lent d'un coup » inexpliqués sont des boucles
(câble branché deux fois, petit switch de bureau rebouclé).

## 34. Agrégation et storm-control (protection broadcast)

Limiter les tempêtes sur les ports d'accès :

```
[AGENCE-DAKAR-AR720]interface GigabitEthernet 0/0/2
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/2]storm-control broadcast min-rate 1000 max-rate 2000
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/2]quit
```

(valeurs en kbps, à ajuster selon la taille du site). Ça ne remplace pas la suppression
de la boucle, mais ça évite que tout le site tombe en attendant l'intervention.

---
---

# Partie D — NAT

## 35. NAT : les trois usages sur l'AR720

1. **NAPT (outbound)** : tout le LAN sort sur Internet avec l'IP du WAN. Le cas de base,
   obligatoire.
2. **NAT server (port mapping)** : exposer un serveur interne (caméra, VPN, applicatif)
   depuis Internet.
3. **NAT statique 1:1** : mapper toute une IP publique vers une IP privée (DMZ).

Sur VRP, le NAT se configure sur l'interface de sortie (outbound) avec une ACL qui
désigne le trafic à translater.

## 36. NAPT sortant : la configuration de base

```
[AGENCE-DAKAR-AR720]acl number 2000
[AGENCE-DAKAR-AR720-acl-basic-2000]rule 5 permit source 192.168.10.0 0.0.0.255
[AGENCE-DAKAR-AR720-acl-basic-2000]rule 10 permit source 192.168.20.0 0.0.0.255
[AGENCE-DAKAR-AR720-acl-basic-2000]rule 15 permit source 192.168.30.0 0.0.0.255
[AGENCE-DAKAR-AR720-acl-basic-2000]quit
[AGENCE-DAKAR-AR720]interface Dialer 1
[AGENCE-DAKAR-AR720-Dialer1]nat outbound 2000
[AGENCE-DAKAR-AR720-Dialer1]quit
```

- L'ACL 2000 (basique) liste les réseaux autorisés à sortir. **Tout réseau oublié ici =
  pas d'Internet** (cause fréquente du « le VLAN invités n'a pas Internet »).
- `nat outbound 2000` sans adresse = NAPT avec l'IP de l'interface (le mode par défaut
  et le plus courant).
- Répéter `nat outbound 2000` sur **chaque** interface WAN (Dialer 1, GE0/0/1…).

Vérification :

```
display nat outbound
display nat session table
```

## 37. NAPT avec pool d'adresses (plusieurs IP publiques)

Si le FAI fournit un bloc d'IP publiques (ex. /29) :

```
[AGENCE-DAKAR-AR720]nat address-group 1 197.155.10.35 197.155.10.38
[AGENCE-DAKAR-AR720]interface Dialer 1
[AGENCE-DAKAR-AR720-Dialer1]nat outbound 2000 address-group 1
[AGENCE-DAKAR-AR720-Dialer1]quit
```

Le routeur répartit les sessions sur le pool. Réserver une IP du bloc pour le NAT server
(ne pas l'inclure dans l'address-group si elle sert à exposer un serveur).

## 38. NAT server : exposer un serveur interne (port mapping)

Exemple : un serveur de vidéosurveillance (192.168.30.10) accessible depuis Internet sur
le port 8080, et un accès VPN L2TP (ports UDP 500/4500/1701 — voir Partie F).

```
[AGENCE-DAKAR-AR720]interface Dialer 1
[AGENCE-DAKAR-AR720-Dialer1]nat server protocol tcp global current-interface 8080 inside 192.168.30.10 8080
[AGENCE-DAKAR-AR720-Dialer1]quit
```

- `global current-interface` : utilise l'IP courante de l'interface (pratique en PPPoE/
  DHCP où l'IP change). Avec une IP fixe, on peut écrire l'IP en dur.
- `protocol tcp ... 8080 inside ... 8080` : port externe 8080 → port interne 8080.
  On peut faire du port mapping (externe 8080 → interne 80).

**Règle de sécurité** : chaque `nat server` doit s'accompagner d'une politique de
sécurité WAN→LAN qui n'autorise QUE ce port (Partie G). Un NAT server sans filtre =
porte ouverte.

## 39. NAT statique 1:1 (DMZ)

```
[AGENCE-DAKAR-AR720]interface Dialer 1
[AGENCE-DAKAR-AR720-Dialer1]nat static global 197.155.10.36 inside 192.168.30.10
[AGENCE-DAKAR-AR720-Dialer1]quit
```

Toute l'IP publique 197.155.10.36 est mappée vers le serveur interne. À réserver aux
vrais besoins (serveur avec beaucoup de ports). Vérification : `display nat static`.

## 40. NAT et VPN : l'exclusion du trafic tunnelé

Quand un tunnel IPSec transporte du trafic LAN→LAN entre sites, ce trafic **ne doit pas
être NATé** (sinon le tunnel ne le reconnaît plus). On l'exclut avec une ACL :

```
[AGENCE-DAKAR-AR720]acl number 3000
[AGENCE-DAKAR-AR720-acl-adv-3000]rule 5 deny ip source 192.168.10.0 0.0.0.255 destination 192.168.0.0 0.0.255.255
[AGENCE-DAKAR-AR720-acl-adv-3000]rule 10 permit ip source 192.168.10.0 0.0.0.255
[AGENCE-DAKAR-AR720-acl-adv-3000]quit
[AGENCE-DAKAR-AR720]interface Dialer 1
[AGENCE-DAKAR-AR720-Dialer1]nat outbound 3000
[AGENCE-DAKAR-AR720-Dialer1]quit
```

La règle `deny` en premier : le trafic vers le réseau du siège (192.168.0.0/16) n'est
pas NATé, tout le reste oui. **Oublier cette exclusion = tunnel IPSec qui monte mais
aucun trafic ne passe** (classique absolu, voir cas n°1 du dépannage).

## 41. ALG : quand le NAT casse les protocoles (FTP, SIP…)

Certains protocoles embarquent des adresses IP dans la charge utile (FTP actif, SIP).
Le NAT de base ne les réécrit pas → ça casse. VRP propose des ALG (Application Level
Gateway) :

```
[AGENCE-DAKAR-AR720]nat alg ftp enable
[AGENCE-DAKAR-AR720]nat alg sip enable
```

N'activer que ce qui sert. Pour la ToIP, préférer un SBC ou un VPN vers le PABX plutôt
que du SIP en NAT direct si possible — le SIP à travers NAT reste une source de tickets.

## 42. Vérifier et purger les sessions NAT

```
display nat session table verbose
display nat statistics
```

Si une translation est « coincée » (vieille session qui bloque un port) :

```
reset nat session table
```

(En vue utilisateur. Attention : ça coupe les sessions actives — à faire en fenêtre de
maintenance ou quand le problème est avéré.)

---
---

# Partie E — Routage

## 43. Routage statique : les bases VRP

```
[AGENCE-DAKAR-AR720]ip route-static 10.0.0.0 255.255.0.0 192.168.10.254
[AGENCE-DAKAR-AR720]ip route-static 10.0.0.0 255.255.0.0 192.168.10.253 preference 100
```

- Le premier paramètre après le réseau = **next-hop** (IP du routeur suivant).
- `preference` : distance administrative (60 par défaut en statique). Plus c'est bas,
  plus c'est prioritaire.
- On peut aussi spécifier l'interface de sortie : `ip route-static 0.0.0.0 0.0.0.0
  Dialer 1` (obligatoire pour les interfaces point-à-point/PPPoE où il n'y a pas de
  next-hop IP fixe).

Vérification : `display ip routing-table`, `display ip routing-table statistics`.

## 44. Route flottante : le secours manuel simple

Deux routes vers la même destination, préférences différentes :

```
[AGENCE-DAKAR-AR720]ip route-static 10.0.0.0 255.255.0.0 172.16.0.1
[AGENCE-DAKAR-AR720]ip route-static 10.0.0.0 255.255.0.0 172.16.1.1 preference 120
```

Tant que le next-hop principal répond (et que l'interface est up), la route de secours
reste inactive. Simple, mais sans NQA ça ne détecte que la perte du lien local —
combiner avec le track/NQA de la Partie B pour un vrai basculement.

## 45. OSPF : pourquoi et quand l'utiliser

Dès qu'on a 3+ sites interconnectés (ou un LAN avec plusieurs routeurs), le statique
devient ingérable. OSPF (dynamique, état de liens) :

- converge seul en cas de panne,
- s'authentifie (MD5),
- supporte le multi-zone (backbone area 0).

Sur l'AR720, OSPF est supporté en IPv4 et OSPFv3 en IPv6 (selon datasheet).

## 46. OSPF : configuration minimale entre deux sites

Côté agence (Router-ID = IP de loopback, à créer) :

```
[AGENCE-DAKAR-AR720]interface LoopBack 0
[AGENCE-DAKAR-AR720-LoopBack0]ip address 10.255.0.11 255.255.255.255
[AGENCE-DAKAR-AR720-LoopBack0]quit
[AGENCE-DAKAR-AR720]ospf 1 router-id 10.255.0.11
[AGENCE-DAKAR-AR720-ospf-1]area 0
[AGENCE-DAKAR-AR720-ospf-1-area-0.0.0.0]network 192.168.10.0 0.0.0.255
[AGENCE-DAKAR-AR720-ospf-1-area-0.0.0.0]network 10.255.0.11 0.0.0.0
[AGENCE-DAKAR-AR720-ospf-1-area-0.0.0.0]quit
[AGENCE-DAKAR-AR720-ospf-1]quit
```

Points clés :

- `router-id` explicite et **unique** par routeur (sinon conflits d'élection).
- Les `network` utilisent des **wildcard masks** (inverses du masque).
- Ne déclarer en OSPF que les interfaces qui doivent parler OSPF. L'interface WAN vers
  Internet : `silent-interface` (voir section 48).

Vérification :

```
display ospf peer
display ospf routing
display ospf interface
```

## 47. OSPF sur tunnel GRE/IPSec : attention au multicast

OSPF utilise du multicast (224.0.0.5/6). Sur un tunnel GRE ça passe (GRE transporte le
multicast). Sur de l'IPSec pur en mode tunnel, il faut vérifier que la crypto ACL couvre
le multicast ou passer par du GRE over IPSec (recommandé, voir section 58).

Pour forcer l'adjacence sur un lien point-à-point (tunnel) :

```
[AGENCE-DAKAR-AR720]interface Tunnel 0/0/1
[AGENCE-DAKAR-AR720-Tunnel0/0/1]ospf network-type p2p
[AGENCE-DAKAR-AR720-Tunnel0/0/1]quit
```

Ça évite l'élection DR/BDR inutile et accélère la montée.

## 48. OSPF : sécuriser et optimiser

```
[AGENCE-DAKAR-AR720]ospf 1 router-id 10.255.0.11
[AGENCE-DAKAR-AR720-ospf-1]silent-interface GigabitEthernet 0/0/0
[AGENCE-DAKAR-AR720-ospf-1]silent-interface Dialer 1
[AGENCE-DAKAR-AR720-ospf-1]area 0
[AGENCE-DAKAR-AR720-ospf-1-area-0.0.0.0]authentication-mode md5 1 cipher CleOSPFfictive
[AGENCE-DAKAR-AR720-ospf-1-area-0.0.0.0]quit
[AGENCE-DAKAR-AR720-ospf-1]quit
```

- `silent-interface` : ne pas envoyer de Hello OSPF vers Internet (sécurité + CPU).
- `authentication-mode md5` : authentifier les voisins (même clé des deux côtés).
- Les timers Hello/Dead par défaut (10 s / 40 s) conviennent ; ne les toucher que si le
  lien est instable (satellite, 4G) — et **identiques des deux côtés**.

## 49. Redistribution : injecter les statiques dans OSPF

Pour annoncer la route par défaut (ou des statiques) aux voisins OSPF :

```
[AGENCE-DAKAR-AR720]ospf 1
[AGENCE-DAKAR-AR720-ospf-1]default-route-advertise always
[AGENCE-DAKAR-AR720-ospf-1]import-route static
[AGENCE-DAKAR-AR720-ospf-1]quit
```

- `default-route-advertise always` : annonce 0.0.0.0/0 même si le routeur n'a pas lui-même
  de défaut (utile sur un concentrateur). Sans `always`, il ne l'annonce que s'il a une
  route par défaut active.
- Filtrer avec une route-policy pour ne redistribuer que ce qu'il faut (voir section 50).

## 50. Route-policy : filtrer proprement

```
[AGENCE-DAKAR-AR720]ip ip-prefix PLAGE-AGENCES index 10 permit 192.168.0.0 16 greater-equal 24 less-equal 24
[AGENCE-DAKAR-AR720]route-policy FILTRE-OSPF permit node 10
[AGENCE-DAKAR-AR720-route-policy]if-match ip-prefix PLAGE-AGENCES
[AGENCE-DAKAR-AR720-route-policy]quit
[AGENCE-DAKAR-AR720]ospf 1
[AGENCE-DAKAR-AR720-ospf-1]import-route static route-policy FILTRE-OSPF
[AGENCE-DAKAR-AR720-ospf-1]quit
```

Principe : `ip-prefix` = la liste de ce qu'on autorise, `route-policy` = le filtre
appliqué à la redistribution. Sans filtre, on risque d'injecter des routes parasites
(ou des boucles) dans tout le domaine.

## 51. Exemple multi-site simple : agence ↔ siège via VPN

Topologie :

- Siège : 192.168.0.0/24, routeur 10.255.0.1.
- Agence Dakar : 192.168.10.0/24, AR720 10.255.0.11.
- Tunnel GRE over IPSec entre les deux (section 58), OSPF par-dessus.

Côté agence, après avoir monté le tunnel :

```
[AGENCE-DAKAR-AR720]ospf 1 router-id 10.255.0.11
[AGENCE-DAKAR-AR720-ospf-1]area 0
[AGENCE-DAKAR-AR720-ospf-1-area-0.0.0.0]network 192.168.10.0 0.0.0.255
[AGENCE-DAKAR-AR720-ospf-1-area-0.0.0.0]network 172.16.100.0 0.0.0.3
[AGENCE-DAKAR-AR720-ospf-1-area-0.0.0.0]quit
[AGENCE-DAKAR-AR720-ospf-1]quit
```

où 172.16.100.0/30 est le réseau du tunnel GRE. Le siège déclare son LAN + le même
réseau de tunnel. Résultat : chaque site apprend les routes de l'autre dynamiquement,
et l'ajout d'une 3e agence ne demande qu'à la configurer elle (pas à toucher les autres).

## 52. PBR (Policy-Based Routing) : router selon la source

Parfois on veut qu'un VLAN sorte par un WAN différent (ex. invités → WAN2, bureautique
→ WAN1). C'est du routage par politique :

```
[AGENCE-DAKAR-AR720]acl number 3001
[AGENCE-DAKAR-AR720-acl-adv-3001]rule 5 permit ip source 192.168.20.0 0.0.0.255
[AGENCE-DAKAR-AR720-acl-adv-3001]quit
[AGENCE-DAKAR-AR720]policy-based-route INVITES permit node 5
[AGENCE-DAKAR-AR720-policy-based-route-INVITES-5]if-match acl 3001
[AGENCE-DAKAR-AR720-policy-based-route-INVITES-5]apply ip-address next-hop 192.168.8.1
[AGENCE-DAKAR-AR720-policy-based-route-INVITES-5]quit
[AGENCE-DAKAR-AR720]interface Vlanif 20
[AGENCE-DAKAR-AR720-Vlanif20]ip policy-based-route INVITES
[AGENCE-DAKAR-AR720-Vlanif20]quit
```

Le PBR s'applique **avant** la table de routage. Attention : le trafic PBRé ne suit plus
les routes dynamiques — à combiner avec prudence avec le basculement NQA.

---
---

# Partie F — VPN

## 53. Vue d'ensemble : quel VPN pour quel besoin

| Besoin | Solution | Section |
|---|---|---|
| Relier 2 sites (LAN à LAN) | IPSec site-à-site | 54–57 |
| Relier 2 sites + OSPF dynamique | GRE over IPSec | 58 |
| Utilisateur nomade (PC/téléphone) | L2TP over IPSec | 59–60 |
| Accès distant simple (secours) | L2TP pur (sans IPSec) — déconseillé | 59 |

Règle : **IPSec en mode tunnel** pour du site-à-site simple ; **GRE over IPSec** dès qu'on
veut du routage dynamique ou du multicast par-dessus.

## 54. IPSec site-à-site : les briques (IKE phase 1 et 2)

- **Phase 1 (IKE)** : les deux routeurs s'authentifient et négocient un canal sécurisé
  (SA ISAKMP). Paramètres : version IKE (v1/v2), chiffrement, hash, DH, PSK ou certificat.
- **Phase 2 (IPSec)** : négociation des SA de données (ESP). Paramètres : transform-set
  (ESP-AES + SHA), PFS, durée de vie.
- **Crypto ACL** : définit **quel trafic** passe dans le tunnel (le « interesting traffic »).

Les deux côtés doivent avoir des paramètres **miroir** : ce que l'un propose, l'autre
doit l'accepter. 80 % des échecs viennent d'un paramètre qui diffère.

## 55. IPSec site-à-site IKEv2 + PSK : configuration complète — côté agence (AR720)

Hypothèses : agence 192.168.10.0/24, IP WAN 197.155.10.34 (Dialer 1) ; siège
192.168.0.0/24, IP WAN 197.155.20.10. PSK fictive `CleIPSecFictive2026!`.

```
[AGENCE-DAKAR-AR720]ike proposal 10
[AGENCE-DAKAR-AR720-ike-proposal-10]encryption-algorithm aes-256
[AGENCE-DAKAR-AR720-ike-proposal-10]dh group14
[AGENCE-DAKAR-AR720-ike-proposal-10]authentication-algorithm sha2-256
[AGENCE-DAKAR-AR720-ike-proposal-10]integrity-algorithm hmac-sha2-256
[AGENCE-DAKAR-AR720-ike-proposal-10]quit
[AGENCE-DAKAR-AR720]ike peer SIEGE
[AGENCE-DAKAR-AR720-ike-peer-SIEGE]ike-version 2
[AGENCE-DAKAR-AR720-ike-peer-SIEGE]remote-address 197.155.20.10
[AGENCE-DAKAR-AR720-ike-peer-SIEGE]pre-shared-key cipher CleIPSecFictive2026!
[AGENCE-DAKAR-AR720-ike-peer-SIEGE]proposal 10
[AGENCE-DAKAR-AR720-ike-peer-SIEGE]quit
[AGENCE-DAKAR-AR720]ipsec proposal TRANSFO-AGENCE
[AGENCE-DAKAR-AR720-ipsec-proposal-TRANSFO-AGENCE]esp authentication-algorithm sha2-256
[AGENCE-DAKAR-AR720-ipsec-proposal-TRANSFO-AGENCE]esp encryption-algorithm aes-256
[AGENCE-DAKAR-AR720-ipsec-proposal-TRANSFO-AGENCE]quit
[AGENCE-DAKAR-AR720]acl number 3100
[AGENCE-DAKAR-AR720-acl-adv-3100]rule 5 permit ip source 192.168.10.0 0.0.0.255 destination 192.168.0.0 0.0.0.255
[AGENCE-DAKAR-AR720-acl-adv-3100]quit
[AGENCE-DAKAR-AR720]ipsec policy CARTE-SIEGE 10 isakmp
[AGENCE-DAKAR-AR720-ipsec-policy-isakmp-CARTE-SIEGE-10]security acl 3100
[AGENCE-DAKAR-AR720-ipsec-policy-isakmp-CARTE-SIEGE-10]ike-peer SIEGE
[AGENCE-DAKAR-AR720-ipsec-policy-isakmp-CARTE-SIEGE-10]proposal TRANSFO-AGENCE
[AGENCE-DAKAR-AR720-ipsec-policy-isakmp-CARTE-SIEGE-10]pfs dh-group14
[AGENCE-DAKAR-AR720-ipsec-policy-isakmp-CARTE-SIEGE-10]quit
[AGENCE-DAKAR-AR720]interface Dialer 1
[AGENCE-DAKAR-AR720-Dialer1]ipsec policy CARTE-SIEGE
[AGENCE-DAKAR-AR720-Dialer1]quit
```

Et ne pas oublier l'exclusion NAT (section 40) avec une ACL qui nie le trafic vers
192.168.0.0/24 avant le `permit` général.

## 56. IPSec site-à-site : configuration miroir — côté siège

Le siège fait exactement le miroir (crypto ACL inversée, remote-address = IP de l'agence).
Si le siège est aussi un Huawei VRP, la config est symétrique :

```
[SIEGE]ike proposal 10
[SIEGE-ike-proposal-10]encryption-algorithm aes-256
[SIEGE-ike-proposal-10]dh group14
[SIEGE-ike-proposal-10]authentication-algorithm sha2-256
[SIEGE-ike-proposal-10]integrity-algorithm hmac-sha2-256
[SIEGE-ike-proposal-10]quit
[SIEGE]ike peer AGENCE-DAKAR
[SIEGE-ike-peer-AGENCE-DAKAR]ike-version 2
[SIEGE-ike-peer-AGENCE-DAKAR]remote-address 197.155.10.34
[SIEGE-ike-peer-AGENCE-DAKAR]pre-shared-key cipher CleIPSecFictive2026!
[SIEGE-ike-peer-AGENCE-DAKAR]proposal 10
[SIEGE-ike-peer-AGENCE-DAKAR]quit
[SIEGE]ipsec proposal TRANSFO-SIEGE
[SIEGE-ipsec-proposal-TRANSFO-SIEGE]esp authentication-algorithm sha2-256
[SIEGE-ipsec-proposal-TRANSFO-SIEGE]esp encryption-algorithm aes-256
[SIEGE-ipsec-proposal-TRANSFO-SIEGE]quit
[SIEGE]acl number 3100
[SIEGE-acl-adv-3100]rule 5 permit ip source 192.168.0.0 0.0.0.255 destination 192.168.10.0 0.0.0.255
[SIEGE-acl-adv-3100]quit
[SIEGE]ipsec policy CARTE-AGENCE 10 isakmp
[SIEGE-ipsec-policy-isakmp-CARTE-AGENCE-10]security acl 3100
[SIEGE-ipsec-policy-isakmp-CARTE-AGENCE-10]ike-peer AGENCE-DAKAR
[SIEGE-ipsec-policy-isakmp-CARTE-AGENCE-10]proposal TRANSFO-AGENCE
[SIEGE-ipsec-policy-isakmp-CARTE-AGENCE-10]pfs dh-group14
[SIEGE-ipsec-policy-isakmp-CARTE-AGENCE-10]quit
[SIEGE]interface GigabitEthernet 0/0/0
[SIEGE-GigabitEthernet0/0/0]ipsec policy CARTE-AGENCE
[SIEGE-GigabitEthernet0/0/0]quit
```

Checklist de symétrie avant de tester : même IKE version, même proposal (AES/SHA/DH),
même PSK, ACL miroir, PFS identique, politique appliquée sur l'interface WAN des deux
côtés.

## 57. Vérifier un tunnel IPSec : les commandes qui comptent

```
display ike sa
display ipsec sa
display ike peer
```

- `display ike sa` : phase 1. On doit voir l'état `ESTABLISHED` (ou équivalent selon
  version). Rien ici = problème phase 1 (connectivité UDP 500/4500, PSK, proposal).
- `display ipsec sa` : phase 2. Des SA avec compteurs qui augmentent = trafic chiffré
  qui passe.
- Test applicatif : `ping -a 192.168.10.1 192.168.0.1` (forcer la source dans le LAN
  pour que le trafic matche la crypto ACL).

Si le tunnel ne monte pas : voir cas n°1 et n°2 du dépannage (Partie K).

## 58. GRE over IPSec : le tunnel qui accepte OSPF et le multicast

GRE encapsule n'importe quel protocole (dont OSPF multicast) dans de l'IP ; IPSec
chiffre le tout. Configuration côté agence (tunnel 172.16.100.0/30) :

```
[AGENCE-DAKAR-AR720]interface Tunnel 0/0/1
[AGENCE-DAKAR-AR720-Tunnel0/0/1]ip address 172.16.100.2 255.255.255.252
[AGENCE-DAKAR-AR720-Tunnel0/0/1]tunnel-protocol gre
[AGENCE-DAKAR-AR720-Tunnel0/0/1]source Dialer 1
[AGENCE-DAKAR-AR720-Tunnel0/0/1]destination 197.155.20.10
[AGENCE-DAKAR-AR720-Tunnel0/0/1]ospf network-type p2p
[AGENCE-DAKAR-AR720-Tunnel0/0/1]quit
[AGENCE-DAKAR-AR720]acl number 3101
[AGENCE-DAKAR-AR720-acl-adv-3101]rule 5 permit gre source 197.155.10.34 destination 197.155.20.10
[AGENCE-DAKAR-AR720-acl-adv-3101]quit
```

Puis une `ipsec policy` dont la `security acl` est l'ACL 3101 (trafic GRE entre les deux
IP publiques), appliquée sur l'interface WAN. Côté siège : miroir (source/destination
inversées, IP tunnel 172.16.100.1).

Avantages : OSPF passe, on peut router dynamiquement, le MTU se règle une fois sur le
tunnel (`mtu 1400` + `tcp adjust-mss 1360` recommandés sur l'interface Tunnel).

## 59. L2TP : serveur d'accès distant pour les nomades

L2TP permet à un PC/telephone avec un client VPN natif de se connecter au réseau de
l'agence/du siège. Configuration serveur L2TP over IPSec (recommandé) côté AR720 :

```
[AGENCE-DAKAR-AR720]l2tp enable
[AGENCE-DAKAR-AR720]l2tp-group 1
[AGENCE-DAKAR-AR720-l2tp-group-1]tunnel password cipher MotDePasseTunnelFictif
[AGENCE-DAKAR-AR720-l2tp-group-1]allow l2tp virtual-template 1 remote client1
[AGENCE-DAKAR-AR720-l2tp-group-1]quit
[AGENCE-DAKAR-AR720]interface Virtual-Template 1
[AGENCE-DAKAR-AR720-Virtual-Template1]ppp authentication-mode chap
[AGENCE-DAKAR-AR720-Virtual-Template1]ip address 192.168.50.1 255.255.255.0
[AGENCE-DAKAR-AR720-Virtual-Template1]quit
[AGENCE-DAKAR-AR720]aaa
[AGENCE-DAKAR-AR720-aaa]local-user nomade1 password cipher MotDePasseNomadeFictif
[AGENCE-DAKAR-AR720-aaa]local-user nomade1 service-type ppp
[AGENCE-DAKAR-AR720-aaa]quit
[AGENCE-DAKAR-AR720]ip pool vpn-nomades
[AGENCE-DAKAR-AR720-ip-pool-vpn-nomades]network 192.168.50.0 mask 255.255.255.0
[AGENCE-DAKAR-AR720-ip-pool-vpn-nomades]gateway-list 192.168.50.1
[AGENCE-DAKAR-AR720-ip-pool-vpn-nomades]quit
[AGENCE-DAKAR-AR720]interface Virtual-Template 1
[AGENCE-DAKAR-AR720-Virtual-Template1]ppp ipcp dns 192.168.10.1
[AGENCE-DAKAR-AR720-Virtual-Template1]remote address pool vpn-nomades
[AGENCE-DAKAR-AR720-Virtual-Template1]quit
```

Côté client (Windows/Android/iOS) : VPN L2TP/IPSec avec PSK + login/mot de passe PPP.
Ports à ouvrir sur le WAN : UDP 500, UDP 4500, UDP 1701 (voir NAT server section 38 +
politique WAN→Local section 66).

## 60. L2TP : vérification et sessions actives

```
display l2tp tunnel
display l2tp session
display ppp user
```

On y voit les tunnels montés, les sessions PPP et les utilisateurs connectés. Pour
déconnecter un utilisateur : `cut connection` (syntaxe exacte selon version — **à
vérifier sur la fiche du modèle exact**) ou désactiver son compte AAA.

## 61. IPSec avec certificats : principe et étapes

Le PSK partagé, c'est bien pour 2–3 sites. Au-delà (ou pour une exigence de sécurité),
on passe aux certificats X.509 :

1. Déployer ou utiliser une CA (interne : AD CS, ou appliance ; jamais d'auto-signé
   bricolé sans procédure).
2. Générer une clé RSA sur l'AR720 (`pki` / `rsa local-key-pair create` — syntaxe selon
   version VRP).
3. Créer une demande de certificat (CSR), la faire signer par la CA.
4. Importer le certificat + la CA sur le routeur.
5. Dans le `ike peer`, remplacer `pre-shared-key` par l'authentification par certificat
   (`certificate` + `rsa-signature` selon version).

**Avertissement version** : les commandes PKI varient sensiblement entre VRP V200 et
V300. Suivre le guide « PKI Configuration » de la version exacte (`display version`).
Points critiques : horloge exacte (NTP obligatoire, sinon le certificat paraît expiré),
et renouvellement suivi (un certificat expiré = tous les tunnels tombent en même temps).

## 62. Dépannage tunnel : la méthode en 5 minutes

Ordre de diagnostic, toujours le même :

1. **Connectivité IP** : `ping` l'IP publique distante. Si ça ne répond pas, le problème
   n'est pas IPSec.
2. **Phase 1** : `display ike sa`. Vide → vérifier UDP 500/4500 ouverts, PSK identique,
   proposals compatibles, `ike-version` identique.
3. **Phase 2** : `display ipsec sa`. Phase 1 OK mais pas de phase 2 → crypto ACL non
   miroir, transform-set incompatible, PFS différent.
4. **Trafic** : compteurs SA à 0 → exclusion NAT oubliée, route manquante vers le
   réseau distant, politique de sécurité qui bloque.
5. **Logs** : `display logbuffer` + debug ciblé (`debugging ike`, `debugging ipsec` —
   avec parcimonie, voir section 100).

## 63. Keepalive et DPD : détecter un pair mort

```
[AGENCE-DAKAR-AR720]ike peer SIEGE
[AGENCE-DAKAR-AR720-ike-peer-SIEGE]dpd type periodic
[AGENCE-DAKAR-AR720-ike-peer-SIEGE]quit
```

Le DPD (Dead Peer Detection) envoie des sondes périodiques : si le siège ne répond plus,
la SA est nettoyée et renégociée au prochain trafic. Sans DPD, un tunnel peut rester
« à moitié monté » indéfiniment après une coupure. Recommandé sur tous les tunnels,
surtout avec des liens 4G instables.

---
---

# Partie G — Sécurité

## 64. Le modèle de zones Huawei : comprendre avant de configurer

VRP utilise des **zones de sécurité** : chaque interface appartient à une zone, et le
trafic est filtré **entre zones** (interzone), pas par interface. Zones par défaut :

| Zone | Usage typique |
|---|---|
| `trust` | LAN interne |
| `untrust` | WAN / Internet |
| `dmz` | Serveurs exposés |
| `local` | Le routeur lui-même |

Principe : par défaut, **tout est refusé entre zones** sauf ce qu'on autorise
explicitement via des `security-policy`. C'est un firewall stateful : le retour d'une
connexion autorisée est automatiquement accepté.

## 65. Assigner les interfaces aux zones

```
[AGENCE-DAKAR-AR720]firewall zone trust
[AGENCE-DAKAR-AR720-zone-trust]add interface Vlanif 10
[AGENCE-DAKAR-AR720-zone-trust]add interface Vlanif 30
[AGENCE-DAKAR-AR720-zone-trust]quit
[AGENCE-DAKAR-AR720]firewall zone untrust
[AGENCE-DAKAR-AR720-zone-untrust]add interface Dialer 1
[AGENCE-DAKAR-AR720-zone-untrust]add interface GigabitEthernet 0/0/1
[AGENCE-DAKAR-AR720-zone-untrust]quit
[AGENCE-DAKAR-AR720]firewall zone dmz
[AGENCE-DAKAR-AR720-zone-dmz]add interface Vlanif 20
[AGENCE-DAKAR-AR720-zone-dmz]quit
```

(Ici les invités sont mis en DMZ pour illustrer : Internet seul, pas d'accès au trust.)

## 66. Politique trust → untrust : laisser sortir le LAN

```
[AGENCE-DAKAR-AR720]security-policy
[AGENCE-DAKAR-AR720-policy-security]rule name LAN-vers-Internet
[AGENCE-DAKAR-AR720-policy-security-rule-LAN-vers-Internet]source-zone trust
[AGENCE-DAKAR-AR720-policy-security-rule-LAN-vers-Internet]destination-zone untrust
[AGENCE-DAKAR-AR720-policy-security-rule-LAN-vers-Internet]source-address 192.168.10.0 24
[AGENCE-DAKAR-AR720-policy-security-rule-LAN-vers-Internet]action permit
[AGENCE-DAKAR-AR720-policy-security-rule-LAN-vers-Internet]quit
[AGENCE-DAKAR-AR720-policy-security]quit
```

## 67. Politique untrust → trust : n'ouvrir que le nécessaire

Par défaut : rien n'est autorisé d'Internet vers le LAN. On n'ouvre que les ports
explicitement exposés via NAT server :

```
[AGENCE-DAKAR-AR720]security-policy
[AGENCE-DAKAR-AR720-policy-security]rule name WAN-vers-ServeurVideo
[AGENCE-DAKAR-AR720-policy-security-rule-WAN-vers-ServeurVideo]source-zone untrust
[AGENCE-DAKAR-AR720-policy-security-rule-WAN-vers-ServeurVideo]destination-zone trust
[AGENCE-DAKAR-AR720-policy-security-rule-WAN-vers-ServeurVideo]destination-address 192.168.30.10 32
[AGENCE-DAKAR-AR720-policy-security-rule-WAN-vers-ServeurVideo]service protocol tcp destination-port 8080
[AGENCE-DAKAR-AR720-policy-security-rule-WAN-vers-ServeurVideo]action permit
[AGENCE-DAKAR-AR720-policy-security-rule-WAN-vers-ServeurVideo]quit
[AGENCE-DAKAR-AR720-policy-security]quit
```

Chaque `nat server` = une règle miroir ici. **Audit trimestriel** : lister les NAT server
et vérifier qu'il y a une règle pour chacun, et inversement.

## 68. Politique vers la zone local : protéger le routeur lui-même

La zone `local` = le routeur. Autoriser : SSH d'administration depuis le LAN, SNMP depuis
le superviseur, NTP. Refuser : tout le reste depuis untrust.

```
[AGENCE-DAKAR-AR720]security-policy
[AGENCE-DAKAR-AR720-policy-security]rule name Admin-SSH-LAN
[AGENCE-DAKAR-AR720-policy-security-rule-Admin-SSH-LAN]source-zone trust
[AGENCE-DAKAR-AR720-policy-security-rule-Admin-SSH-LAN]destination-zone local
[AGENCE-DAKAR-AR720-policy-security-rule-Admin-SSH-LAN]service protocol tcp destination-port 22
[AGENCE-DAKAR-AR720-policy-security-rule-Admin-SSH-LAN]action permit
[AGENCE-DAKAR-AR720-policy-security-rule-Admin-SSH-LAN]quit
[AGENCE-DAKAR-AR720-policy-security]rule name IKE-vers-local
[AGENCE-DAKAR-AR720-policy-security-rule-IKE-vers-local]source-zone untrust
[AGENCE-DAKAR-AR720-policy-security-rule-IKE-vers-local]destination-zone local
[AGENCE-DAKAR-AR720-policy-security-rule-IKE-vers-local]service protocol udp destination-port 500
[AGENCE-DAKAR-AR720-policy-security-rule-IKE-vers-local]service protocol udp destination-port 4500
[AGENCE-DAKAR-AR720-policy-security-rule-IKE-vers-local]action permit
[AGENCE-DAKAR-AR720-policy-security-rule-IKE-vers-local]quit
[AGENCE-DAKAR-AR720-policy-security]quit
```

Sans la règle IKE vers local, les tunnels IPSec ne montent jamais (le routeur jette les
paquets IKE). C'est la cause n°2 des « le tunnel ne monte pas » après la PSK.

## 69. Vérifier les politiques et les sessions

```
display security-policy all
display firewall session table
display firewall statistics
```

`display firewall session table` montre les connexions actives (utile pour voir si un
flux est bien autorisé/NATé). Si une règle ne matche pas, vérifier l'ordre des règles
et les zones source/destination (l'erreur la plus fréquente : zones inversées).

---
---

# Partie H — QoS

## 70. NTP : la base (rappel — sans horloge juste, rien n'est fiable)

```
[AGENCE-DAKAR-AR720]ntp-service unicast-server 10.0.0.5
[AGENCE-DAKAR-AR720]ntp-service unicast-server 192.168.10.5 prefer
```

Vérification : `display ntp-service status` (stratum, offset). En agence sans serveur
local : pointer vers le serveur NTP du siège via le tunnel VPN.

## 71. QoS : les concepts en 2 minutes

- **Classification** : repérer le trafic (ACL, DSCP, port).
- **Marquage** : poser un DSCP/802.1p.
- **CAR (Committed Access Rate)** : limiter un débit (policing).
- **Files d'attente** : prioriser (LLQ pour la voix, CBWFQ pour le reste).
- **Shaping** : lisser en sortie.

Sur un lien WAN asymétrique (cas général), la QoS utile se fait **en sortie** (upload),
là où on contrôle la file. En entrée, on ne peut que policer (jeter).

## 72. CAR : limiter le débit des invités (exemple)

Limiter le VLAN invités à 20 Mbit/s en sortie vers le WAN :

```
[AGENCE-DAKAR-AR720]acl number 3002
[AGENCE-DAKAR-AR720-acl-adv-3002]rule 5 permit ip source 192.168.20.0 0.0.0.255
[AGENCE-DAKAR-AR720-acl-adv-3002]quit
[AGENCE-DAKAR-AR720]traffic classifier INVITES
[AGENCE-DAKAR-AR720-classifier-INVITES]if-match acl 3002
[AGENCE-DAKAR-AR720-classifier-INVITES]quit
[AGENCE-DAKAR-AR720]traffic behavior LIMITE-20M
[AGENCE-DAKAR-AR720-behavior-LIMITE-20M]car cir 20000 pir 22000 green pass yellow pass red discard
[AGENCE-DAKAR-AR720-behavior-LIMITE-20M]quit
[AGENCE-DAKAR-AR720]traffic policy QOS-WAN
[AGENCE-DAKAR-AR720-trafficpolicy-QOS-WAN]classifier INVITES behavior LIMITE-20M
[AGENCE-DAKAR-AR720-trafficpolicy-QOS-WAN]quit
[AGENCE-DAKAR-AR720]interface Dialer 1
[AGENCE-DAKAR-AR720-Dialer1]traffic-policy QOS-WAN outbound
[AGENCE-DAKAR-AR720-Dialer1]quit
```

- `cir 20000` : débit garanti/limite en kbps (20 Mbit/s).
- `pir` : pic toléré. `green/yellow pass, red discard` : on jette au-delà du pic.

## 73. Prioriser la voix (ToIP) : file prioritaire

```
[AGENCE-DAKAR-AR720]acl number 3003
[AGENCE-DAKAR-AR720-acl-adv-3003]rule 5 permit udp destination-port eq 5060
[AGENCE-DAKAR-AR720-acl-adv-3003]rule 10 permit udp destination-port range 10000 20000
[AGENCE-DAKAR-AR720-acl-adv-3003]quit
[AGENCE-DAKAR-AR720]traffic classifier VOIX
[AGENCE-DAKAR-AR720-classifier-VOIX]if-match acl 3003
[AGENCE-DAKAR-AR720-classifier-VOIX]quit
[AGENCE-DAKAR-AR720]traffic behavior PRIORITE-VOIX
[AGENCE-DAKAR-AR720-behavior-PRIORITE-VOIX]queue pq
[AGENCE-DAKAR-AR720-behavior-PRIORITE-VOIX]quit
[AGENCE-DAKAR-AR720]traffic policy QOS-WAN
[AGENCE-DAKAR-AR720-trafficpolicy-QOS-WAN]classifier VOIX behavior PRIORITE-VOIX
[AGENCE-DAKAR-AR720-trafficpolicy-QOS-WAN]quit
```

- `queue pq` : file prioritaire (Priority Queuing) — la voix passe avant tout.
- Adapter les ports RTP (10000–20000 ici, fictifs : prendre la plage du PABX réel).
- **Règle d'or** : la file prioritaire ne doit représenter qu'une petite part du lien
  (10–30 %). Si on y met tout, elle ne priorise plus rien.

## 74. Marquage DSCP en entrée du LAN

Marquer la voix dès l'entrée pour que le marquage suive dans le tunnel VPN :

```
[AGENCE-DAKAR-AR720]traffic classifier VOIX-IN
[AGENCE-DAKAR-AR720-classifier-VOIX-IN]if-match acl 3003
[AGENCE-DAKAR-AR720-classifier-VOIX-IN]quit
[AGENCE-DAKAR-AR720]traffic behavior MARQUE-EF
[AGENCE-DAKAR-AR720-behavior-MARQUE-EF]remark dscp ef
[AGENCE-DAKAR-AR720-behavior-MARQUE-EF]quit
[AGENCE-DAKAR-AR720]traffic policy MARQUAGE-LAN
[AGENCE-DAKAR-AR720-trafficpolicy-MARQUAGE-LAN]classifier VOIX-IN behavior MARQUE-EF
[AGENCE-DAKAR-AR720-trafficpolicy-MARQUAGE-LAN]quit
[AGENCE-DAKAR-AR720]interface Vlanif 30
[AGENCE-DAKAR-AR720-Vlanif30]traffic-policy MARQUAGE-LAN inbound
[AGENCE-DAKAR-AR720-Vlanif30]quit
```

## 75. Vérifier la QoS

```
display traffic policy applied-record
display traffic-policy statistics interface Dialer 1 outbound
```

Les statistiques montrent les paquets par classe et les paquets jetés par le CAR.
Si la voix saccade malgré la file prioritaire : vérifier que le classifier matche bien
(`display` des compteurs), et que le lien montant n'est pas saturé à 100 % en permanence
(la QoS ne crée pas de débit).

---
---

# Partie I — Supervision

## 76. SNMP : les bases (v2c puis v3)

Activer SNMP et déclarer la communauté (v2c, lecture seule, restreinte au superviseur) :

```
[AGENCE-DAKAR-AR720]snmp-agent
[AGENCE-DAKAR-AR720]snmp-agent sys-info version v2c v3
[AGENCE-DAKAR-AR720]snmp-agent community read cipher CommunauteFictive123 acl 2001
[AGENCE-DAKAR-AR720]snmp-agent sys-info contact admin-reseau@exemple.sn
[AGENCE-DAKAR-AR720]snmp-agent sys-info location Agence-Dakar-Siege-social
```

Avec l'ACL 2001 qui n'autorise que l'IP du superviseur :

```
[AGENCE-DAKAR-AR720]acl number 2001
[AGENCE-DAKAR-AR720-acl-basic-2001]rule 5 permit source 10.0.0.100 0
[AGENCE-DAKAR-AR720-acl-basic-2001]quit
```

## 77. SNMPv3 : la version à utiliser en production

```
[AGENCE-DAKAR-AR720]snmp-agent
[AGENCE-DAKAR-AR720]snmp-agent sys-info version v3
[AGENCE-DAKAR-AR720]snmp-agent mib-view included iso-view iso
[AGENCE-DAKAR-AR720]snmp-agent group v3 supervision-group privacy read-view iso-view
[AGENCE-DAKAR-AR720]snmp-agent usm-user v3 supervision-user group supervision-group acl 2001
[AGENCE-DAKAR-AR720]snmp-agent usm-user v3 supervision-user authentication-mode sha2-256 MotDePasseAuthFictif privacy-mode aes-256 MotDePassePrivFictif
```

SNMPv3 = authentification + chiffrement. Ne jamais exposer SNMP en v1/v2c sur le WAN.
**Avertissement version** : les algorithmes proposés (sha2-256, aes-256) dépendent de la
version VRP — `sha`/`aes128` sont les valeurs sûres historiques ; vérifier avec `?` dans
la CLI du modèle exact.

## 78. Traps SNMP : être prévenu au lieu de constater

```
[AGENCE-DAKAR-AR720]snmp-agent trap enable
[AGENCE-DAKAR-AR720]snmp-agent target-host trap address udp-domain 10.0.0.100 params securityname supervision-user v3 privacy
```

Traps utiles à vérifier : linkup/linkdown, changements OSPF (neighbor down), basculement
de route, température. Tester en débranchant/rebranchant un câble et en regardant le
superviseur recevoir le trap.

## 79. Syslog : centraliser les logs

```
[AGENCE-DAKAR-AR720]info-center enable
[AGENCE-DAKAR-AR720]info-center loghost 10.0.0.101
[AGENCE-DAKAR-AR720]info-center source default channel loghost log level informational
```

- `10.0.0.101` : serveur syslog du siège (via le tunnel VPN).
- Niveau `informational` en production ; `debugging` uniquement en dépannage ciblé et
  temporaire (ça remplit les disques et charge le CPU).
- Vérifier : `display info-center` et regarder les logs arriver sur le serveur.

## 80. NetStream : voir qui consomme la bande passante

NetStream (l'équivalent NetFlow de Huawei) exporte les flux vers un collecteur :

```
[AGENCE-DAKAR-AR720]ip netstream
[AGENCE-DAKAR-AR720]ip netstream export host 10.0.0.102 9996
[AGENCE-DAKAR-AR720]ip netstream export version 9
[AGENCE-DAKAR-AR720]interface Dialer 1
[AGENCE-DAKAR-AR720-Dialer1]ip netstream inbound
[AGENCE-DAKAR-AR720-Dialer1]ip netstream outbound
[AGENCE-DAKAR-AR720-Dialer1]quit
```

Ça répond à « qui sature le lien ? » en 2 minutes au lieu de 2 heures. Collecteur type :
nfdump/nfsen, ou le module du superviseur.

## 81. NQA/SLA : sonder en continu les liens et les tunnels

Au-delà du basculement (section 22), NQA sert à mesurer la qualité (latence, perte,
gigue) vers le siège ou vers Internet :

```
[AGENCE-DAKAR-AR720]nqa test-instance admin sla-siege
[AGENCE-DAKAR-AR720-nqa-admin-sla-siege]test-type icmp
[AGENCE-DAKAR-AR720-nqa-admin-sla-siege]destination-address ipv4 192.168.0.1
[AGENCE-DAKAR-AR720-nqa-admin-sla-siege]frequency 60
[AGENCE-DAKAR-AR720-nqa-admin-sla-siege]start now
[AGENCE-DAKAR-AR720-nqa-admin-sla-siege]quit
```

Résultats : `display nqa results test-instance admin sla-siege` (min/avg/max RTT, pertes).
Historiser via SNMP ou syslog pour prouver au FAI que « ça rame depuis mardi ».

## 82. Ce qu'il faut superviser en priorité (checklist)

| Sonde | Seuil d'alerte | Pourquoi |
|---|---|---|
| Ping du routeur (LAN + WAN) | perte | Savoir qu'il est vivant |
| État interfaces WAN | down | Panne lien |
| CPU (`display cpu-usage`) | > 80 % prolongé | Sous-dimensionnement / attaque |
| Mémoire (`display memory`) | > 85 % | Fuite / config trop grosse |
| Température | > 40 °C | Ventilation / local |
| État tunnel IPSec (SNMP) | down | VPN coupé |
| NQA vers siège | perte > 5 % | Qualité lien |
| Traps linkup/linkdown | — | Câbles, FAI |

---
---

# Partie J — Exploitation : sauvegarde, firmware, licences

## 83. Sauvegarde : la procédure complète

1. `save` sur le routeur (config en flash).
2. Copie externe immédiate après chaque changement important :
   ```
   <AGENCE-DAKAR-AR720>tftp 10.0.0.103 put vrpcfg.zip backup-AGENCE-DAKAR-20260927.zip
   ```
   (ou `ftp` / clé USB : `copy vrpcfg.zip usb0:/...`).
3. Versionner les fichiers (Git, ou dossier daté) — jamais un seul fichier « config.cfg »
   écrasé en boucle.
4. Tester la restauration **une fois** sur un équipement de spare ou en labo avant d'en
   avoir besoin pour de vrai.

## 84. Restauration : remettre une config

```
<AGENCE-DAKAR-AR720>tftp 10.0.0.103 get backup-AGENCE-DAKAR-20260927.zip vrpcfg.zip
<AGENCE-DAKAR-AR720>startup saved-configuration backup-AGENCE-DAKAR-20260927.zip
<AGENCE-DAKAR-AR720>reboot
```

Vérifier après reboot que la config chargée est la bonne (`display current-configuration
| include sysname`). En cas de doute, la console est le filet de sécurité : toujours
faire une restauration avec quelqu'un en console (ou un accès KVM), jamais en aveugle
à distance.

## 85. Mise à jour firmware : procédure détaillée + rollback

Reprise et détails de la section 14, en procédure d'exploitation :

1. Fenêtre de maintenance déclarée, utilisateurs prévenus.
2. `save` + sauvegarde externe de la config (section 83).
3. `display version` et `display device` : noter la version actuelle.
4. Copier la nouvelle image en flash, vérifier la taille (`dir`).
5. `startup system-software <nouvelle-image>` puis `display startup` pour confirmer.
6. `reboot`, suivre en console.
7. Post-reboot : `display version`, tester WAN → LAN → VPN → téléphone.
8. Laisser tourner 48 h avant d'effacer l'ancienne image.
9. **Rollback** si problème : `startup system-software <ancienne-image>` + `reboot`.

Ne jamais couper l'alimentation pendant un upgrade. Sur un site avec coupures fréquentes :
onduleur obligatoire + ne pas lancer l'upgrade un jour d'orage.

## 86. Fichiers de démarrage : comprendre `display startup`

```
<AGENCE-DAKAR-AR720>display startup
```

Affiche : image système de démarrage, fichier de config de démarrage, patch éventuel.
Après chaque upgrade ou restauration, c'est LA commande de contrôle. Une erreur ici =
le routeur redémarre sur l'ancienne version ou sans config.

## 87. Licences : ce qu'il faut savoir

Sur la gamme AR, certaines fonctions avancées (contrôleur WLAN avec beaucoup d'AP,
fonctions de sécurité avancées type IPS/AV avec signatures, parfois le nombre de
tunnels) peuvent nécessiter des licences ou des abonnements de signatures — **à vérifier
sur la fiche du modèle exact** et le devis du fournisseur.

Procédure type (quand applicable) :

```
display license
```

- Noter les dates d'expiration des licences/signatures dans le planning de maintenance.
- Une signature IPS/AV expirée = protection qui ne se met plus à jour (silencieusement).
- Renouveler **avant** expiration : prévoir 30 jours de marge dans le suivi.

## 88. Mot de passe perdu : procédure de récupération

Si plus personne ne connaît le mot de passe admin :

1. Accès physique + console obligatoires.
2. Au boot, interrompre la séquence (touche selon version, souvent `Ctrl+B`) pour entrer
   dans le BootROM.
3. Choisir l'option « skip current configuration » / ignorer la config au démarrage
   (le libellé exact dépend de la version — **à vérifier sur la fiche du modèle exact**).
4. Le routeur démarre sans config : reconfigurer les accès, puis recharger la config
   sauvegardée (d'où l'importance de la sauvegarde externe, section 83).
5. `save` avec les nouveaux mots de passe.

C'est pour ça que l'accès physique au local technique doit être contrôlé : quiconque a
la console peut réinitialiser les mots de passe.

---
---

# Partie K — Dépannage terrain (15 cas détaillés)

> Méthode générale : **1)** qualifier le symptôme (quoi, quand, qui est impacté),
> **2)** vérifier la couche basse (LED, lien, IP), **3)** remonter les couches
> (routage → NAT → firewall → applicatif), **4)** une hypothèse à la fois,
> **5)** noter la cause et la solution dans le dossier du site.

## 89. Cas n°1 — Le tunnel IPSec ne monte pas : phase 1 KO

**Symptômes :** `display ike sa` vide, aucun trafic vers le site distant.

**Diagnostic :**

```
display ike sa
display ike peer
ping <ip-publique-distante>
display security-policy all | include IKE
```

**Causes fréquentes → solutions :**

| Cause | Indice | Solution |
|---|---|---|
| Pas de connectivité IP | ping KO | Régler le WAN d'abord (Partie B) |
| UDP 500/4500 bloqués | ping OK, IKE muet | Ouvrir dans la politique untrust→local (section 68) + box FAI en face |
| PSK différente | logs IKE « authentication failed » | Ressaisir la même PSK des 2 côtés |
| Proposal incompatible | échec de négociation | Aligner chiffrement/hash/DH/ike-version |
| `ike-version` différente (v1 vs v2) | pas de SA du tout | Mettre la même version des 2 côtés |
| remote-address erronée | — | Vérifier l'IP publique distante (elle a peut-être changé en DHCP) |

## 90. Cas n°2 — Phase 1 OK, phase 2 KO (pas de SA IPSec)

**Symptômes :** `display ike sa` montre une SA établie, mais `display ipsec sa` est vide.

**Diagnostic :** `display ipsec sa`, `display acl 3100` des deux côtés.

**Causes → solutions :**

- **Crypto ACL non miroir** : l'ACL locale dit 192.168.10.0→192.168.0.0 mais la distante
  dit autre chose. Réécrire les deux ACL en miroir exact (sections 55–56).
- **Transform-set incompatible** (AES-256 d'un côté, 3DES de l'autre) : aligner.
- **PFS différent** (dh-group14 vs none) : aligner.
- **Politique appliquée sur la mauvaise interface** : `display ipsec policy` et vérifier
  qu'elle est bien sur l'interface WAN active.

## 91. Cas n°3 — Tunnel monté mais aucun trafic ne passe

**Symptômes :** SA IPSec établies, compteurs à 0 ou qui n'augmentent pas, ping KO.

**Diagnostic :**

```
display ipsec sa
display ip routing-table | include 192.168.0.0
display nat session table
display firewall session table
```

**Causes → solutions :**

1. **Exclusion NAT oubliée** (le grand classique) : le trafic vers le site distant est
   NATé avant d'entrer dans le tunnel. Ajouter la règle `deny` dans l'ACL du `nat
   outbound` (section 40).
2. **Route manquante** vers le réseau distant : ajouter la route statique ou vérifier
   OSPF (`display ospf peer`).
3. **Politique de sécurité** : autoriser trust→untrust et le retour (les flux IPSec
   eux-mêmes sont gérés, mais le trafic clair GRE/interne doit être autorisé selon la
   version).
4. **MTU** : paquets qui passent dans un sens mais pas l'autre → régler `tcp adjust-mss`
   sur le tunnel (section 58).

## 92. Cas n°4 — Le PPPoE décroche en boucle

**Symptômes :** Internet coupe et revient toutes les quelques minutes ; `display
pppoe-client session summary` montre des sessions qui se renégocient.

**Diagnostic :**

```
display pppoe-client session summary
display logbuffer | include PPP
display interface Dialer 1
```

**Causes → solutions :**

- Ligne FAI instable (marge SNR faible) : faire tester la ligne par le FAI, vérifier le
  câblage cuivre, les filtres, l'ONT.
- Négociation PAP/CHAP : forcer le bon mode si le FAI est strict.
- `dialer-group` / `dialer-rule` mal configurée : le Dialer ne redémarre pas tout seul.
  Vérifier `dialer bundle 1` et la règle `permit`.
- MTU : tester avec `ping -s 1472 -c 5` (ne pas confondre perte MTU et décrochage).

## 93. Cas n°5 — Le NAT ne translate pas (pas d'Internet malgré WAN OK)

**Symptômes :** le routeur ping Internet, les PC du LAN non.

**Diagnostic :**

```
display nat outbound
display acl 2000
display ip interface brief
```

**Causes → solutions :**

1. `nat outbound` oublié sur l'interface WAN (ou sur la **bonne** interface : Dialer et
   pas GE0/0/0 quand on est en PPPoE !).
2. Le réseau LAN oublié dans l'ACL 2000 (ajouter la règle `permit`).
3. Le `nat outbound` est sur l'interface physique alors que l'IP est sur le Dialer
   (PPPoE) : le NAT doit être sur l'interface qui porte l'IP publique.
4. Politique de sécurité trust→untrust manquante (section 66).

## 94. Cas n°6 — OSPF neighbor bloqué (ExStart, Exchange, Loading)

**Symptômes :** `display ospf peer` montre un voisin qui ne dépasse pas un état
intermédiaire.

**Diagnostic :** `display ospf peer verbose`, `display ospf error`.

**Causes → solutions :**

| État bloqué | Cause typique | Solution |
|---|---|---|
| Init | Hello non reçu en retour | ACL/firewall qui bloque le multicast, `silent-interface` d'un côté |
| ExStart | MTU différent | Aligner la MTU des deux interfaces |
| Exchange/Loading | Router-ID dupliqué | Changer le router-id (unique !) |
| — | Authentification MD5 différente | Même clé, même mode des 2 côtés |
| — | Timers Hello/Dead différents | Aligner |
| — | Area différente | Même area des 2 côtés |

## 95. Cas n°7 — Débit asymétrique ou très inférieur au contrat FAI

**Symptômes :** speedtest à 40 Mbit/s sur un lien 200 Mbit/s, ou montant correct mais
descendant effondré.

**Diagnostic :**

```
display interface GigabitEthernet 0/0/0
display cpu-usage
display traffic-policy statistics interface Dialer 1 outbound
```

**Causes → solutions :**

- **Duplex/négociation** : `display interface` montre des erreurs CRC, collisions →
  forcer `duplex full` + `speed 1000` des deux côtés (ou laisser l'auto-négociation si
  elle converge).
- **CPU à 100 %** : trop de services (IPS + VPN + QoS) → alléger ou accepter la limite
  plateforme.
- **CAR trop agressif** : vérifier les traffic-policy appliquées.
- **Câble** : un câble abîmé négocie parfois en 100 Mbit/s sans erreur franche →
  changer le câble, tester.
- Tester **au routeur** d'abord (`ping` + transfert depuis le routeur) pour isoler :
  problème WAN ou problème LAN ?

## 96. Cas n°8 — DHCP qui ne distribue plus

**Symptômes :** les nouveaux PC restent en 169.254.x.x.

**Diagnostic :**

```
display ip pool name lan-bureautique used
display dhcp server statistics
```

**Causes → solutions :**

1. `dhcp enable` désactivé (oublié après un `undo` ou une restauration partielle).
2. Pool épuisé : `used` = taille du pool → élargir le réseau ou raccourcir le bail.
3. `dhcp select global` oublié sur l'interface (ou `dhcp select relay` mal pointé).
4. Conflit : une IP du pool configurée en statique sur un équipement → `excluded-ip-address`
   + réservation propre (section 28).

## 97. Cas n°9 — Boucle réseau : tout rame d'un coup

**Symptômes :** latence énorme, pertes, LED des ports qui clignotent frénétiquement,
parfois le routeur inaccessible.

**Diagnostic :** regarder les LED, `display interface brief` (compteurs d'erreurs qui
explosent), `display cpu-usage`.

**Actions immédiates :**

1. Débrancher les câbles suspects un par un jusqu'à ce que ça revienne.
2. Chercher le petit switch de bureau rebouclé ou le câble branché deux fois.
3. Remède durable : `stp bpdu-protection` + `storm-control` (sections 33–34).

Ne pas chercher midi à quatorze heures : une tempête de broadcast, ça se voit aux LED.

## 98. Cas n°10 — Plus d'accès SSH après un changement

**Symptômes :** timeout SSH après avoir touché à la config.

**Causes → solutions :**

1. Politique trust→local qui n'autorise plus le port 22 (ou zones inversées).
2. `protocol inbound ssh` remplacé par erreur, ou VTY en `authentication-mode password`
   sans mot de passe défini.
3. ACL sur le VTY (`acl` sous `user-interface vty`) qui bloque l'IP admin.
4. On a changé l'IP du LAN sans mettre à jour sa propre config → passer par la console.

**Prévention** : toujours garder une session console ouverte quand on touche aux accès
distants, et faire `save` **après** avoir vérifié que le nouvel accès fonctionne (pas avant).

## 99. Cas n°11 — Le basculement WAN ne se fait pas

**Symptômes :** WAN principal en panne (plus d'Internet) mais la route de secours ne
prend pas.

**Diagnostic :**

```
display nqa results test-instance admin pppoe-check
display track 1
display ip routing-table
```

**Causes → solutions :**

- La sonde NQA ping une cible **à travers le lien principal** : si le lien tombe, la
  sonde tombe aussi… mais le track ne bascule que si la sonde est configurée pour.
  Vérifier que le `track` est bien lié à la route (`display ip routing-table verbose`).
- Route de secours avec une préférence **plus basse** (plus prioritaire) que la principale
  → elle prend toujours le dessus. Vérifier les préférences.
- Le NAT n'est pas configuré sur l'interface de secours → le trafic sort sans être
  traduit. Ajouter `nat outbound` sur GE0/0/1.
- Politique de sécurité : l'interface de secours est-elle bien en zone `untrust` ?

## 100. Cas n°12 — L2TP : le client ne se connecte pas

**Symptômes :** le client VPN reste en « connexion… » puis échoue.

**Diagnostic :** `display l2tp tunnel`, `display logbuffer | include L2TP`.

**Causes → solutions :**

1. Ports UDP 500/4500/1701 non ouverts (NAT server + politique untrust→local).
2. Le client est derrière un NAT avec IPSec NAT-T mal traversé : vérifier que le
   NAT-T est actif des deux côtés.
3. Login PPP erroné ou `service-type ppp` oublié sur l'utilisateur AAA.
4. Pool d'adresses VPN épuisé ou mal lié au Virtual-Template.

## 101. Cas n°13 — DNS qui ne résout plus sur le LAN

**Symptômes :** les IP passent (ping 8.8.8.8 OK) mais aucun nom ne résout.

**Diagnostic :** `nslookup exemple.sn 192.168.10.1` depuis un PC, `display dns server`.

**Causes → solutions :**

- `dns-list` du pool DHCP pointe vers une IP qui ne répond plus → mettre le routeur
  (192.168.10.1) + un public en second.
- `dns resolve` désactivé sur le routeur.
- Politique de sécurité qui bloque le DNS (UDP 53) du LAN vers le routeur (zone local)
  ou vers Internet.
- Le FAI qui intercepte/mente les DNS : forcer des DNS publics connus pour tester.

## 102. Cas n°14 — Le routeur reboot en boucle / ne démarre plus

**Symptômes :** SYS rouge ou reboot cyclique, pas d'invite.

**Actions :**

1. Vérifier l'alimentation (autre prise, autre cordon) — un bloc qui s'effondre en
   charge donne exactement ce symptôme.
2. BootROM : interrompre le boot (`Ctrl+B`) → vérifier `display startup` : image
   corrompue ?
3. Démarrer sur l'ancienne image (si conservée) ou réinstaller depuis USB :
   option de mise à jour via clé USB dans le BootROM.
4. Si la flash semble morte : SAV constructeur/fournisseur (noter le numéro de série :
   `display device` / étiquette sous le châssis).

**Prévention** : ne jamais effacer l'ancienne image avant 48 h de validation (section 85).

## 103. Cas n°15 — CPU à 100 % / routeur lent à répondre

**Symptômes :** CLI très lente, pertes de paquets, parfois tunnels qui tombent.

**Diagnostic :**

```
display cpu-usage
display cpu-usage history
display memory
```

**Causes → solutions :**

- Debug oublié actif (`debugging ...` laissé tourner) → `undo debugging all`.
- Tempête de broadcast / boucle (cas n°9).
- Trop de sessions NAT (attaque ou P2P) : `display nat session table` → identifier la
  source, filtrer.
- Logs en `debugging` vers le syslog : repasser en `informational`.
- Si le CPU est structurellement haut en production : la plateforme est sous-dimensionnée
  pour les services activés → revoir le dimensionnement (ou monter en gamme AR730,
  **à valider avec le fournisseur**).

## 104. Cas n°16 — Certificats VPN expirés : tout tombe en même temps

**Symptômes :** tous les tunnels IPSec à certificats tombent simultanément, `display ike
sa` vide partout.

**Diagnostic :** `display pki certificate` (dates de validité), `display clock`
(l'horloge est-elle juste ?).

**Solution :** renouveler les certificats via la CA, les réimporter, renégocier.
**Prévention** : supervision des dates d'expiration (inventaire + alerte à J-60), NTP
obligatoire (une horloge fausse invalide un certificat valide).

## 105. Cas n°17 — Après une coupure électrique, plus rien ne marche

**Symptômes :** au retour du courant, le routeur démarre mais la config semble partielle
ou les tunnels ne remontent pas.

**Causes → solutions :**

1. `save` oublié avant la coupure : la config en cours n'était pas sauvegardée →
   reconfigurer ce qui manque, puis `save`.
2. La flash a été corrompue par une coupure pendant une écriture → restaurer depuis la
   sauvegarde externe (section 84).
3. L'ONT/box FAI met plus longtemps à redémarrer que le routeur : le PPPoE échoue au
   boot puis ne retente pas → vérifier les paramètres de redial du Dialer, ou
   temporiser le démarrage.
4. **Leçon** : onduleur + `save` systématique après chaque changement.

---
---

# Partie L — Maintenance préventive

## 106. Plan de maintenance : le calendrier type

| Fréquence | Actions |
|---|---|
| Mensuelle | Vérifier CPU/mémoire/température, relire les logs d'alerte, contrôler l'état des tunnels et du basculement WAN |
| Trimestrielle | Sauvegarde + vérification hors-ligne, audit des règles firewall/NAT server, test du basculement WAN, dépoussiérage |
| Semestrielle | Revue des licences/signatures (expiration), test de restauration sur spare, revue du plan d'adressage |
| Annuelle | Bilan capacitaire (le lien suffit-il encore ?), mise à jour firmware planifiée, test onduleur, révision du dossier de site |

## 107. Checklist mensuelle (15 minutes par site)

```
[ ] display cpu-usage / display memory            -> < 70 %
[ ] display device temperature (si supporté)      -> < 40 °C
[ ] display ike sa / display ipsec sa             -> tunnels UP
[ ] display nqa results ...                       -> pertes < 5 %
[ ] display logbuffer                             -> pas d'erreur récurrente
[ ] display interface brief                       -> pas d'interface down anormale
[ ] Vérifier que la sauvegarde externe est à jour
```

Cocher, dater, archiver. Une checklist non archivée = une checklist non faite.

## 108. Dépoussiérage et environnement

- Le flux d'air va de gauche à droite : dépoussiérer les grilles d'entrée (gauche) au
  moins une fois par trimestre dans un local poussiéreux.
- Ne jamais poser d'objets sur le routeur (il dissipe par le châssis aussi).
- Contrôler que le local ne dépasse pas 45 °C en pointe (été, clim en panne).
- Vérifier le serrage des cordons d'alimentation et l'état des câbles RJ45 (languettes
  cassées = faux contacts intermittents = tickets fantômes).

## 109. Gestion des pièces de rechange

Pour un parc d'agences : **1 routeur de spare pour ~10 sites** (minimum 1 au total).
Le spare est pré-configuré avec un template, stocké avec sa console-câble et une clé USB
contenant les firmwares validés. Procédure de remplacement : noter le numéro de série,
restaurer la config du site (adapter hostname/IP), tester WAN/LAN/VPN, `save`.

## 110. Suivi des firmwares et des failles

- S'abonner aux bulletins de sécurité Huawei pour la gamme AR.
- Ne pas appliquer chaque version le jour de sa sortie : attendre les retours, tester en
  labo/spare, puis déployer par vagues (1 site pilote → 30 % → 100 %).
- Tenir un registre : site / version actuelle / version cible / date de passage.

---
---

# Partie M — Durcissement

## 111. Principe : réduire la surface d'attaque

Un routeur de branche exposé sur Internet est scanné en permanence. Le durcissement =
désactiver tout ce qui ne sert pas, restreindre tout ce qui sert, journaliser le reste.

## 112. Désactiver les services inutiles

```
[AGENCE-DAKAR-AR720]undo telnet server enable
[AGENCE-DAKAR-AR720]undo ftp server enable
[AGENCE-DAKAR-AR720]undo http server enable
```

Ne garder que : SSH (admin), SNMP (supervision, v3), NTP (client). Le serveur web
d'administration : à n'activer que si l'équipe l'utilise vraiment, et jamais exposé sur
le WAN (politique untrust→local : refuser les ports 80/443).

## 113. SSH durci : clés plutôt que mots de passe (quand possible)

```
[AGENCE-DAKAR-AR720]ssh user admin authentication-type all
[AGENCE-DAKAR-AR720]ssh server-source -i Vlanif 10
```

- Préférer l'authentification par clé (selon support de la version) ; à défaut, mot de
  passe long + `authentication-mode aaa`.
- `ssh server-source -i Vlanif 10` : le serveur SSH n'écoute que sur l'interface LAN
  (pas sur le WAN). Vérifier la syntaxe exacte sur la version (`?` dans la CLI).
- Changer le port SSH par défaut ? Ça réduit le bruit des scans mais n'est pas une
  sécurité en soi. Le vrai gain = pas de SSH joignable depuis Internet du tout
  (administration via le tunnel VPN).

## 114. ACL d'administration : qui a le droit de se connecter

```
[AGENCE-DAKAR-AR720]acl number 2005
[AGENCE-DAKAR-AR720-acl-basic-2005]rule 5 permit source 192.168.10.0 0.0.0.255
[AGENCE-DAKAR-AR720-acl-basic-2005]rule 10 permit source 10.0.0.0 0.0.255.255
[AGENCE-DAKAR-AR720-acl-basic-2005]quit
[AGENCE-DAKAR-AR720]user-interface vty 0 4
[AGENCE-DAKAR-AR720-ui-vty0-4]acl 2005 inbound
[AGENCE-DAKAR-AR720-ui-vty0-4]quit
```

Seuls le LAN de l'agence et le réseau du siège peuvent ouvrir une session VTY.

## 115. SNMPv3 uniquement, jamais en clair sur le WAN

Rappel section 77 : `snmp-agent sys-info version v3` seul (retirer v2c/v1), communauté
supprimée, traps vers le superviseur via le tunnel. Vérifier qu'aucune politique
untrust→local n'autorise UDP 161.

## 116. Protection du plan de contrôle (CPU)

Limiter ce que le CPU accepte pour éviter qu'une attaque ou une tempête ne le mette à
genoux :

- Activer les protections anti-attaque intégrées (selon version : `anti-attack` /
  `cpu-defend policy` — **syntaxe à vérifier sur la fiche du modèle exact**).
- Principe : définir des seuils (pps) par protocole vers la zone local (ARP, ICMP, SSH…).
- Tester après activation : une politique trop stricte coupe l'administration légitime.

## 117. Journaliser les accès et les refus

```
[AGENCE-DAKAR-AR720]security-policy
[AGENCE-DAKAR-AR720-policy-security]rule name Log-Refus-WAN
[AGENCE-DAKAR-AR720-policy-security-rule-Log-Refus-WAN]source-zone untrust
[AGENCE-DAKAR-AR720-policy-security-rule-Log-Refus-WAN]destination-zone local
[AGENCE-DAKAR-AR720-policy-security-rule-Log-Refus-WAN]action deny
[AGENCE-DAKAR-AR720-policy-security-rule-Log-Refus-WAN]quit
[AGENCE-DAKAR-AR720-policy-security]quit
```

+ `info-center` vers le syslog (section 79). Relire périodiquement : des refus SSH en
boucle depuis la même IP = scan → bannir à la main ou via le superviseur.

## 118. Mots de passe : politique minimale

- Longueur ≥ 12, uniques par site et par usage (console ≠ admin ≠ PSK VPN ≠ SNMP).
- `cipher` partout (jamais en clair dans la config).
- Rotation : à chaque départ d'un technicien qui les connaissait, et au moins une fois
  par an pour les comptes partagés.
- Stockage : coffre de l'équipe, jamais dans un tableur partagé ni dans un e-mail.

## 119. Checklist de durcissement avant mise en service

```
[ ] Telnet/FTP/HTTP désactivés (ou justifiés)
[ ] SSH seul, restreint par ACL, pas joignable du WAN
[ ] SNMP v3 uniquement, communauté v2c supprimée
[ ] Politique untrust->local : uniquement IKE (500/4500) si VPN, rien d'autre
[ ] Politique untrust->trust : uniquement les NAT server documentés
[ ] Mots de passe en cipher, uniques par site
[ ] info-center vers syslog actif
[ ] CPU-defend / anti-attaque activé et testé
[ ] save + sauvegarde externe
```

---
---

# Partie N — Cas pratiques complets commentés

## 120. Cas pratique n°1 — Agence standard : config complète commentée

Scénario : agence de 30 personnes. WAN principal PPPoE (fibre FAI, login
`agence.dakar@fai.exemple`), WAN secours box 4G (192.168.8.1). LAN 192.168.10.0/24,
invités 192.168.20.0/24, équipements 192.168.30.0/24. Tunnel IPSec vers le siège
(192.168.0.0/24). Toutes les valeurs d'exemple sont fictives.

```
# --- Identité et base ---
[AR720]sysname AGENCE-DAKAR-AR720
# Nom explicite : site + modèle. On sait où on est dans chaque terminal.

[AGENCE-DAKAR-AR720]clock timezone Dakar add 00:00
# Fuseau pour des logs exploitables. NTP configuré plus bas.

# --- Accès console ---
[AGENCE-DAKAR-AR720]user-interface console 0
[AGENCE-DAKAR-AR720-ui-console0]authentication-mode password
[AGENCE-DAKAR-AR720-ui-console0]set authentication password cipher MotDePasseConsoleFictif123
[AGENCE-DAKAR-AR720-ui-console0]idle-timeout 10 0
[AGENCE-DAKAR-AR720-ui-console0]quit

# --- Admin SSH ---
[AGENCE-DAKAR-AR720]aaa
[AGENCE-DAKAR-AR720-aaa]local-user admin password cipher MotDePasseAdminFictif456
[AGENCE-DAKAR-AR720-aaa]local-user admin privilege level 15
[AGENCE-DAKAR-AR720-aaa]local-user admin service-type ssh terminal
[AGENCE-DAKAR-AR720-aaa]quit
[AGENCE-DAKAR-AR720]stelnet server enable
[AGENCE-DAKAR-AR720]ssh user admin authentication-type password
[AGENCE-DAKAR-AR720]user-interface vty 0 4
[AGENCE-DAKAR-AR720-ui-vty0-4]authentication-mode aaa
[AGENCE-DAKAR-AR720-ui-vty0-4]protocol inbound ssh
[AGENCE-DAKAR-AR720-ui-vty0-4]acl 2005 inbound
[AGENCE-DAKAR-AR720-ui-vty0-4]quit
# SSH uniquement, filtré par ACL (définie plus bas).

# --- VLAN et LAN ---
[AGENCE-DAKAR-AR720]vlan batch 10 20 30
[AGENCE-DAKAR-AR720]interface Vlanif 10
[AGENCE-DAKAR-AR720-Vlanif10]ip address 192.168.10.1 255.255.255.0
[AGENCE-DAKAR-AR720-Vlanif10]dhcp select global
[AGENCE-DAKAR-AR720-Vlanif10]quit
[AGENCE-DAKAR-AR720]interface Vlanif 20
[AGENCE-DAKAR-AR720-Vlanif20]ip address 192.168.20.1 255.255.255.0
[AGENCE-DAKAR-AR720-Vlanif20]dhcp select global
[AGENCE-DAKAR-AR720-Vlanif20]quit
[AGENCE-DAKAR-AR720]interface Vlanif 30
[AGENCE-DAKAR-AR720-Vlanif30]ip address 192.168.30.1 255.255.255.0
[AGENCE-DAKAR-AR720-Vlanif30]quit
[AGENCE-DAKAR-AR720]interface GigabitEthernet 0/0/2
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/2]port link-type access
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/2]port default vlan 10
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/2]stp edged-port enable
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/2]quit
# GE0/0/2 en accès VLAN 10 ; les autres ports à répartir selon le brassage.

# --- DHCP ---
[AGENCE-DAKAR-AR720]dhcp enable
[AGENCE-DAKAR-AR720]ip pool lan-bureautique
[AGENCE-DAKAR-AR720-ip-pool-lan-bureautique]gateway-list 192.168.10.1
[AGENCE-DAKAR-AR720-ip-pool-lan-bureautique]network 192.168.10.0 mask 255.255.255.0
[AGENCE-DAKAR-AR720-ip-pool-lan-bureautique]dns-list 192.168.10.1 8.8.8.8
[AGENCE-DAKAR-AR720-ip-pool-lan-bureautique]excluded-ip-address 192.168.10.1 192.168.10.20
[AGENCE-DAKAR-AR720-ip-pool-lan-bureautique]lease day 1 hour 0 minute 0
[AGENCE-DAKAR-AR720-ip-pool-lan-bureautique]static-bind ip-address 192.168.10.21 mac-address aaaa-bbbb-cccc
[AGENCE-DAKAR-AR720-ip-pool-lan-bureautique]quit
[AGENCE-DAKAR-AR720]ip pool lan-invites
[AGENCE-DAKAR-AR720-ip-pool-lan-invites]gateway-list 192.168.20.1
[AGENCE-DAKAR-AR720-ip-pool-lan-invites]network 192.168.20.0 mask 255.255.255.0
[AGENCE-DAKAR-AR720-ip-pool-lan-invites]dns-list 192.168.20.1 8.8.8.8
[AGENCE-DAKAR-AR720-ip-pool-lan-invites]lease day 0 hour 6 minute 0
[AGENCE-DAKAR-AR720-ip-pool-lan-invites]quit
# Bail court pour les invités (6 h), long pour la bureautique (1 j).

# --- WAN PPPoE ---
[AGENCE-DAKAR-AR720]dialer-rule
[AGENCE-DAKAR-AR720-dialer-rule]dialer-rule 1 ip permit
[AGENCE-DAKAR-AR720-dialer-rule]quit
[AGENCE-DAKAR-AR720]interface Dialer 1
[AGENCE-DAKAR-AR720-Dialer1]dialer user agence.dakar@fai.exemple
[AGENCE-DAKAR-AR720-Dialer1]dialer-group 1
[AGENCE-DAKAR-AR720-Dialer1]dialer bundle 1
[AGENCE-DAKAR-AR720-Dialer1]ppp chap user agence.dakar@fai.exemple
[AGENCE-DAKAR-AR720-Dialer1]ppp chap password cipher MotDePasseFAIFictif
[AGENCE-DAKAR-AR720-Dialer1]ppp pap local-user agence.dakar@fai.exemple password cipher MotDePasseFAIFictif
[AGENCE-DAKAR-AR720-Dialer1]ip address ppp-negotiate
[AGENCE-DAKAR-AR720-Dialer1]tcp adjust-mss 1400
[AGENCE-DAKAR-AR720-Dialer1]quit
[AGENCE-DAKAR-AR720]interface GigabitEthernet 0/0/0
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/0]pppoe-client dial-bundle-number 1
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/0]quit

# --- WAN secours ---
[AGENCE-DAKAR-AR720]interface GigabitEthernet 0/0/1
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/1]ip address dhcp-alloc
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/1]quit
# La box 4G donne 192.168.8.0/24, passerelle 192.168.8.1.

# --- NAT ---
[AGENCE-DAKAR-AR720]acl number 2000
[AGENCE-DAKAR-AR720-acl-basic-2000]rule 5 permit source 192.168.10.0 0.0.0.255
[AGENCE-DAKAR-AR720-acl-basic-2000]rule 10 permit source 192.168.20.0 0.0.0.255
[AGENCE-DAKAR-AR720-acl-basic-2000]quit
[AGENCE-DAKAR-AR720]acl number 3000
[AGENCE-DAKAR-AR720-acl-adv-3000]rule 5 deny ip source 192.168.10.0 0.0.0.255 destination 192.168.0.0 0.0.0.255
[AGENCE-DAKAR-AR720-acl-adv-3000]rule 10 permit ip source 192.168.10.0 0.0.0.255
[AGENCE-DAKAR-AR720-acl-adv-3000]rule 15 permit ip source 192.168.20.0 0.0.0.255
[AGENCE-DAKAR-AR720-acl-adv-3000]quit
[AGENCE-DAKAR-AR720]interface Dialer 1
[AGENCE-DAKAR-AR720-Dialer1]nat outbound 3000
[AGENCE-DAKAR-AR720-Dialer1]quit
[AGENCE-DAKAR-AR720]interface GigabitEthernet 0/0/1
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/1]nat outbound 3000
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/1]quit
# ACL 3000 : le trafic vers le siège n'est PAS naté (tunnel IPSec), le reste oui.
# NAT appliqué sur les DEUX interfaces WAN (sinon pas d'Internet en secours).

# --- Basculement NQA/track ---
[AGENCE-DAKAR-AR720]nqa test-instance admin pppoe-check
[AGENCE-DAKAR-AR720-nqa-admin-pppoe-check]test-type icmp
[AGENCE-DAKAR-AR720-nqa-admin-pppoe-check]destination-address ipv4 8.8.8.8
[AGENCE-DAKAR-AR720-nqa-admin-pppoe-check]frequency 10
[AGENCE-DAKAR-AR720-nqa-admin-pppoe-check]probe-count 3
[AGENCE-DAKAR-AR720-nqa-admin-pppoe-check]start now
[AGENCE-DAKAR-AR720-nqa-admin-pppoe-check]quit
[AGENCE-DAKAR-AR720]track 1 nqa admin pppoe-check
[AGENCE-DAKAR-AR720]ip route-static 0.0.0.0 0.0.0.0 Dialer 1 track 1
[AGENCE-DAKAR-AR720]ip route-static 0.0.0.0 0.0.0.0 192.168.8.1 preference 100

# --- Zones de sécurité ---
[AGENCE-DAKAR-AR720]firewall zone trust
[AGENCE-DAKAR-AR720-zone-trust]add interface Vlanif 10
[AGENCE-DAKAR-AR720-zone-trust]add interface Vlanif 30
[AGENCE-DAKAR-AR720-zone-trust]quit
[AGENCE-DAKAR-AR720]firewall zone untrust
[AGENCE-DAKAR-AR720-zone-untrust]add interface Dialer 1
[AGENCE-DAKAR-AR720-zone-untrust]add interface GigabitEthernet 0/0/1
[AGENCE-DAKAR-AR720-zone-untrust]quit
[AGENCE-DAKAR-AR720]firewall zone dmz
[AGENCE-DAKAR-AR720-zone-dmz]add interface Vlanif 20
[AGENCE-DAKAR-AR720-zone-dmz]quit

# --- Politiques ---
[AGENCE-DAKAR-AR720]security-policy
[AGENCE-DAKAR-AR720-policy-security]rule name LAN-vers-Internet
[AGENCE-DAKAR-AR720-policy-security-rule-LAN-vers-Internet]source-zone trust
[AGENCE-DAKAR-AR720-policy-security-rule-LAN-vers-Internet]destination-zone untrust
[AGENCE-DAKAR-AR720-policy-security-rule-LAN-vers-Internet]source-address 192.168.10.0 24
[AGENCE-DAKAR-AR720-policy-security-rule-LAN-vers-Internet]action permit
[AGENCE-DAKAR-AR720-policy-security-rule-LAN-vers-Internet]quit
[AGENCE-DAKAR-AR720-policy-security]rule name Invites-vers-Internet
[AGENCE-DAKAR-AR720-policy-security-rule-Invites-vers-Internet]source-zone dmz
[AGENCE-DAKAR-AR720-policy-security-rule-Invites-vers-Internet]destination-zone untrust
[AGENCE-DAKAR-AR720-policy-security-rule-Invites-vers-Internet]source-address 192.168.20.0 24
[AGENCE-DAKAR-AR720-policy-security-rule-Invites-vers-Internet]action permit
[AGENCE-DAKAR-AR720-policy-security-rule-Invites-vers-Internet]quit
[AGENCE-DAKAR-AR720-policy-security]rule name IKE-vers-local
[AGENCE-DAKAR-AR720-policy-security-rule-IKE-vers-local]source-zone untrust
[AGENCE-DAKAR-AR720-policy-security-rule-IKE-vers-local]destination-zone local
[AGENCE-DAKAR-AR720-policy-security-rule-IKE-vers-local]service protocol udp destination-port 500
[AGENCE-DAKAR-AR720-policy-security-rule-IKE-vers-local]service protocol udp destination-port 4500
[AGENCE-DAKAR-AR720-policy-security-rule-IKE-vers-local]action permit
[AGENCE-DAKAR-AR720-policy-security-rule-IKE-vers-local]quit
[AGENCE-DAKAR-AR720-policy-security]rule name Admin-SSH-LAN
[AGENCE-DAKAR-AR720-policy-security-rule-Admin-SSH-LAN]source-zone trust
[AGENCE-DAKAR-AR720-policy-security-rule-Admin-SSH-LAN]destination-zone local
[AGENCE-DAKAR-AR720-policy-security-rule-Admin-SSH-LAN]service protocol tcp destination-port 22
[AGENCE-DAKAR-AR720-policy-security-rule-Admin-SSH-LAN]action permit
[AGENCE-DAKAR-AR720-policy-security-rule-Admin-SSH-LAN]quit
[AGENCE-DAKAR-AR720-policy-security]quit
# Les invités (dmz) vont sur Internet mais n'ont AUCUNE règle vers trust : isolés.

# --- Supervision ---
[AGENCE-DAKAR-AR720]ntp-service unicast-server 192.168.0.5
# Serveur NTP du siège, joignable via le tunnel (configuré ci-dessous).
[AGENCE-DAKAR-AR720]snmp-agent
[AGENCE-DAKAR-AR720]snmp-agent sys-info version v3
[AGENCE-DAKAR-AR720]snmp-agent group v3 supervision-group privacy read-view iso-view
[AGENCE-DAKAR-AR720]snmp-agent mib-view included iso-view iso
[AGENCE-DAKAR-AR720]snmp-agent usm-user v3 supervision-user group supervision-group acl 2001
[AGENCE-DAKAR-AR720]snmp-agent usm-user v3 supervision-user authentication-mode sha MotDePasseAuthFictif privacy-mode aes128 MotDePassePrivFictif
[AGENCE-DAKAR-AR720]info-center enable
[AGENCE-DAKAR-AR720]info-center loghost 10.0.0.101
[AGENCE-DAKAR-AR720]acl number 2001
[AGENCE-DAKAR-AR720-acl-basic-2001]rule 5 permit source 10.0.0.100 0
[AGENCE-DAKAR-AR720-acl-basic-2001]quit
[AGENCE-DAKAR-AR720]acl number 2005
[AGENCE-DAKAR-AR720-acl-basic-2005]rule 5 permit source 192.168.10.0 0.0.0.255
[AGENCE-DAKAR-AR720-acl-basic-2005]rule 10 permit source 10.0.0.0 0.0.255.255
[AGENCE-DAKAR-AR720-acl-basic-2005]quit

# --- Durcissement de base ---
[AGENCE-DAKAR-AR720]undo telnet server enable
[AGENCE-DAKAR-AR720]dns resolve
[AGENCE-DAKAR-AR720]dns server 8.8.8.8
[AGENCE-DAKAR-AR720]dns server 1.1.1.1

# --- SAUVEGARDE ---
# <AGENCE-DAKAR-AR720>save
# Ne jamais oublier. Vérifier ensuite display saved-configuration.
```

Ordre de vérification après application : `display pppoe-client session summary` →
`display ip routing-table` → ping 8.8.8.8 depuis un PC → `display ike sa` /
`display ipsec sa` → `save`.

## 121. Cas pratique n°2 — Site avec tunnel GRE over IPSec + OSPF

Scénario : deux sites qui doivent router dynamiquement. On reprend l'agence ci-dessus et
on ajoute le tunnel GRE chiffré par IPSec, avec OSPF par-dessus.

Côté agence (complément à la config du cas n°1) :

```
# --- Tunnel GRE ---
[AGENCE-DAKAR-AR720]interface Tunnel 0/0/1
[AGENCE-DAKAR-AR720-Tunnel0/0/1]ip address 172.16.100.2 255.255.255.252
[AGENCE-DAKAR-AR720-Tunnel0/0/1]tunnel-protocol gre
[AGENCE-DAKAR-AR720-Tunnel0/0/1]source Dialer 1
[AGENCE-DAKAR-AR720-Tunnel0/0/1]destination 197.155.20.10
[AGENCE-DAKAR-AR720-Tunnel0/0/1]mtu 1400
[AGENCE-DAKAR-AR720-Tunnel0/0/1]tcp adjust-mss 1360
[AGENCE-DAKAR-AR720-Tunnel0/0/1]ospf network-type p2p
[AGENCE-DAKAR-AR720-Tunnel0/0/1]quit
# MTU réduite : GRE (24 o) + IPSec (~60 o) mangent de la place.

# --- IPSec qui chiffre le GRE ---
[AGENCE-DAKAR-AR720]acl number 3101
[AGENCE-DAKAR-AR720-acl-adv-3101]rule 5 permit gre source 197.155.10.34 destination 197.155.20.10
[AGENCE-DAKAR-AR720-acl-adv-3101]quit
# (ike proposal/peer, ipsec proposal et ipsec policy : voir sections 55-56,
#  avec security acl 3101 au lieu de 3100, appliquée sur Dialer 1.)

# --- OSPF par-dessus ---
[AGENCE-DAKAR-AR720]interface LoopBack 0
[AGENCE-DAKAR-AR720-LoopBack0]ip address 10.255.0.11 255.255.255.255
[AGENCE-DAKAR-AR720-LoopBack0]quit
[AGENCE-DAKAR-AR720]ospf 1 router-id 10.255.0.11
[AGENCE-DAKAR-AR720-ospf-1]silent-interface Dialer 1
[AGENCE-DAKAR-AR720-ospf-1-area-0.0.0.0]area 0
[AGENCE-DAKAR-AR720-ospf-1-area-0.0.0.0]network 192.168.10.0 0.0.0.255
[AGENCE-DAKAR-AR720-ospf-1-area-0.0.0.0]network 172.16.100.0 0.0.0.3
[AGENCE-DAKAR-AR720-ospf-1-area-0.0.0.0]network 10.255.0.11 0.0.0.0
[AGENCE-DAKAR-AR720-ospf-1-area-0.0.0.0]authentication-mode md5 1 cipher CleOSPFfictive
[AGENCE-DAKAR-AR720-ospf-1-area-0.0.0.0]quit
[AGENCE-DAKAR-AR720-ospf-1]quit
```

Vérifications : `display ipsec sa` (trafic GRE chiffré) → `display ospf peer` (voisin
Full) → `display ip routing-table protocol ospf` (routes du siège apprises).

## 122. Cas pratique n°3 — Exposer un serveur : NAT server + politique

Scénario : caméra/NVR interne 192.168.30.10 à rendre accessible depuis Internet sur le
port 8080, WAN en IP statique 197.155.10.34.

```
[AGENCE-DAKAR-AR720]interface GigabitEthernet 0/0/1
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/1]ip address 197.155.10.34 255.255.255.252
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/1]nat server protocol tcp global 197.155.10.34 8080 inside 192.168.30.10 8080
[AGENCE-DAKAR-AR720-GigabitEthernet0/0/1]quit
[AGENCE-DAKAR-AR720]security-policy
[AGENCE-DAKAR-AR720-policy-security]rule name WAN-vers-NVR
[AGENCE-DAKAR-AR720-policy-security-rule-WAN-vers-NVR]source-zone untrust
[AGENCE-DAKAR-AR720-policy-security-rule-WAN-vers-NVR]destination-zone trust
[AGENCE-DAKAR-AR720-policy-security-rule-WAN-vers-NVR]destination-address 192.168.30.10 32
[AGENCE-DAKAR-AR720-policy-security-rule-WAN-vers-NVR]service protocol tcp destination-port 8080
[AGENCE-DAKAR-AR720-policy-security-rule-WAN-vers-NVR]action permit
[AGENCE-DAKAR-AR720-policy-security-rule-WAN-vers-NVR]quit
[AGENCE-DAKAR-AR720-policy-security]quit
```

Test : depuis un téléphone en 4G (jamais depuis le LAN — le NAT hairpin peut fausser le
test), ouvrir `http://197.155.10.34:8080`. Vérifier `display nat server` et
`display firewall session table`.

## 123. Cas pratique n°4 — QoS voix sur lien contraint

Scénario : lien montant 20 Mbit/s, 10 téléphones IP (VLAN 30), bureautique VLAN 10.
Objectif : la voix ne saccade jamais, même quand quelqu'un envoie un gros fichier.

```
[AGENCE-DAKAR-AR720]acl number 3003
[AGENCE-DAKAR-AR720-acl-adv-3003]rule 5 permit udp source 192.168.30.0 0.0.0.255 destination-port range 10000 20000
[AGENCE-DAKAR-AR720-acl-adv-3003]quit
[AGENCE-DAKAR-AR720]traffic classifier VOIX
[AGENCE-DAKAR-AR720-classifier-VOIX]if-match acl 3003
[AGENCE-DAKAR-AR720-classifier-VOIX]quit
[AGENCE-DAKAR-AR720]traffic classifier DATA
[AGENCE-DAKAR-AR720-classifier-DATA]if-match any
[AGENCE-DAKAR-AR720-classifier-DATA]quit
[AGENCE-DAKAR-AR720]traffic behavior PRIORITE-VOIX
[AGENCE-DAKAR-AR720-behavior-PRIORITE-VOIX]queue pq
[AGENCE-DAKAR-AR720-behavior-PRIORITE-VOIX]quit
[AGENCE-DAKAR-AR720]traffic behavior LIMITE-DATA
[AGENCE-DAKAR-AR720-behavior-LIMITE-DATA]car cir 16000 pir 18000 green pass yellow pass red discard
[AGENCE-DAKAR-AR720-behavior-LIMITE-DATA]quit
[AGENCE-DAKAR-AR720]traffic policy QOS-VOIX
[AGENCE-DAKAR-AR720-trafficpolicy-QOS-VOIX]classifier VOIX behavior PRIORITE-VOIX
[AGENCE-DAKAR-AR720-trafficpolicy-QOS-VOIX]classifier DATA behavior LIMITE-DATA
[AGENCE-DAKAR-AR720-trafficpolicy-QOS-VOIX]quit
[AGENCE-DAKAR-AR720]interface Dialer 1
[AGENCE-DAKAR-AR720-Dialer1]traffic-policy QOS-VOIX outbound
[AGENCE-DAKAR-AR720-Dialer1]quit
```

Logique : la voix passe en priorité absolue ; le reste est plafonné à 16 Mbit/s sur un
lien de 20, ce qui laisse toujours de la place à la voix. **Ordre des classifiers** :
le plus spécifique d'abord (VOIX avant DATA), sinon tout matche `if-match any`.

---
---

# Partie O — Pense-bête de poche

## 124. Les 20 commandes à connaître par cœur

```
display version                    # modèle, VRP, uptime
display current-configuration      # config en cours
display saved-configuration        # config sauvegardée
save                               # sauvegarder !
display ip interface brief         # état + IP des interfaces
display interface GE 0/0/0         # détail un port
display ip routing-table           # table de routage
display ospf peer                  # voisins OSPF
display ike sa                     # phase 1 IPSec
display ipsec sa                   # phase 2 IPSec
display pppoe-client session summary
display nat session table          # sessions NAT actives
display nat outbound               # règles NAT sortant
display firewall session table     # sessions firewall
display security-policy all        # politiques
display cpu-usage / display memory
display logbuffer                  # derniers logs
display clock / display ntp-service status
reboot                             # (vue utilisateur)
```

## 125. Séquence de mise en service (ordre chronologique)

1. Console → hostname → mots de passe → `save`.
2. Firmware : vérifier/valider la version, upgrade si besoin.
3. WAN (PPPoE/DHCP/statique) → tester le ping Internet depuis le routeur.
4. LAN + DHCP → tester depuis un PC.
5. NAT (`nat outbound` + ACL) → tester la navigation.
6. Zones + politiques → re-tester (le firewall peut tout casser).
7. VPN → `display ike sa` / `display ipsec sa` → ping inter-sites.
8. QoS si besoin, supervision (NTP/SNMP/syslog), durcissement.
9. `save` + sauvegarde externe + dossier de site à jour.

## 126. Dossier de site : ce qu'il doit contenir

- Plan d'adressage (VLAN, IP, DHCP, réservations).
- Identifiants FAI (dans le coffre, pas dans le dossier partagé).
- IP WAN, passerelles, DNS.
- PSK VPN (coffre), paramètres IKE/IPSec.
- Version firmware, numéro de série, date d'achat, garantie.
- Sauvegarde de config datée.
- Résultats des tests (débit, basculement, temps de coupure).
- Contacts FAI + contrat.

## 127. Les 10 erreurs qui reviennent tout le temps

1. Oublier `save` → config perdue au reboot.
2. `nat outbound` sur la mauvaise interface (GE au lieu du Dialer).
3. Exclusion NAT oubliée pour le trafic VPN.
4. Politique untrust→local sans IKE → tunnel qui ne monte pas.
5. Crypto ACL non miroir → phase 2 KO.
6. `dhcp enable` oublié → DHCP muet.
7. Réseau oublié dans l'ACL du NAT → VLAN sans Internet.
8. `silent-interface` oublié → Hello OSPF vers Internet.
9. Horloge fausse → certificats/logs inutilisables.
10. Pas de `tcp adjust-mss` sur PPPoE → sites qui ne chargent pas.

---
---

# Partie P — Glossaire

## 128. Glossaire (A–Z)

- **ACL (Access Control List)** : liste de règles qui filtrent le trafic (ici : qui peut
  être NATé, quoi mettre dans le tunnel…).
- **AR720** : routeur de branche Huawei, série NetEngine AR700, 2 WAN combo + 8 LAN,
  2 slots SIC.
- **BPDU** : trames d'échange du protocole STP entre switchs.
- **CAR (Committed Access Rate)** : mécanisme de limitation de débit (policing).
- **CHAP/PAP** : protocoles d'authentification PPP (CHAP = challenge, plus sûr ; PAP =
  mot de passe en clair).
- **Combo (port)** : port RJ45 + cage SFP partageant la même interface logique.
- **Dialer (interface)** : interface logique VRP pour le PPPoE.
- **DMZ** : zone réseau intermédiaire pour les équipements exposés ou isolés (ici aussi
  les invités).
- **DPD (Dead Peer Detection)** : détection d'un pair IPSec mort.
- **DSCP** : marquage de qualité de service dans l'en-tête IP.
- **GRE** : protocole de tunnelisation (encapsulation IP dans IP).
- **IKE (Internet Key Exchange)** : protocole de négociation des clés IPSec (phase 1) ;
  v1 et v2.
- **IPSec** : suite de protocoles de chiffrement/authentification au niveau IP (ESP/AH).
- **L2TP** : protocole de tunnel de niveau 2, souvent combiné à IPSec pour les nomades.
- **LLQ/PQ** : file d'attente prioritaire (voix).
- **MSS (Maximum Segment Size)** : taille max d'un segment TCP ; à ajuster sous la MTU.
- **MTU** : taille maximale d'un paquet sur un lien.
- **NAPT** : NAT avec translation de ports (plusieurs privés → une publique).
- **NAT server** : redirection d'un port public vers un serveur interne (port mapping).
- **NetStream** : export de flux façon NetFlow (Huawei).
- **NQA** : sondes de qualité/disponibilité (ping, jitter…) + objet `track`.
- **OSPF** : protocole de routage dynamique à état de liens.
- **PBR (Policy-Based Routing)** : routage selon des critères (source…) au lieu de la
  table de routage.
- **PFS (Perfect Forward Secrecy)** : renouvellement des clés Diffie-Hellman en phase 2.
- **PPPoE** : PPP sur Ethernet (authentification FAI sur ADSL/fibre).
- **PSK (Pre-Shared Key)** : clé partagée pour authentifier un tunnel IPSec.
- **SA (Security Association)** : association de sécurité négociée (IKE phase 1 / IPSec
  phase 2).
- **SIC (Smart Interface Card)** : carte d'extension (4G, E1, voix…) des slots AR720.
- **SNMP** : protocole de supervision ; v3 = version chiffrée/authentifiée.
- **STP** : protocole anti-boucle de niveau 2.
- **Track** : objet VRP qui suit l'état d'une sonde NQA et pilote les routes.
- **VLAN** : réseau local virtuel (segmentation logique).
- **Vlanif** : interface virtuelle VRP portant l'IP d'un VLAN.
- **VRP (Versatile Routing Platform)** : système d'exploitation des routeurs Huawei.
- **VTY** : lignes virtuelles pour l'accès distant (SSH/telnet).
- **WSIC** : slot pour carte Wi-Fi intégrée (option selon référence).

---
---

# Partie Q — Quiz (10 questions)

## 129. Questions

1. Quelle commande sauvegarde la configuration en cours pour qu'elle survive au reboot ?
2. Sur l'AR720, combien y a-t-il de ports WAN combo fixes et de ports LAN fixes ?
3. Pourquoi faut-il appliquer le `nat outbound` sur l'interface Dialer (et pas sur
   GE0/0/0) quand le WAN est en PPPoE ?
4. Un tunnel IPSec monte en phase 1 mais pas en phase 2. Citez deux causes probables.
5. À quoi sert l'exclusion NAT (règle `deny` dans l'ACL du `nat outbound`) dans un
   contexte VPN site-à-site ?
6. Que fait la commande `track 1 nqa admin pppoe-check` liée à une route statique ?
7. Pourquoi configure-t-on `tcp adjust-mss` sur une interface Dialer PPPoE ?
8. Quelle politique de sécurité faut-il impérativement pour que l'IKE aboutisse au
   routeur ?
9. Un voisin OSPF reste bloqué en ExStart. Quelle est la cause la plus probable ?
10. Pourquoi ne faut-il jamais exposer SNMP v1/v2c sur le WAN, et que faut-il utiliser
    à la place ?

## 130. Réponses

1. `save` (en vue utilisateur `<AR720>`). Sans ça, la config est perdue au reboot.
2. 2 ports WAN combo (RJ45/SFP) + 8 ports LAN RJ45 (reconfigurables en WAN).
3. Parce que c'est l'interface Dialer qui porte l'IP publique négociée en PPP ; le NAT
   doit s'appliquer sur l'interface de sortie qui a l'adresse publique.
4. Crypto ACL non miroir entre les deux sites ; transform-set (chiffrement/hash)
   incompatible ; PFS différent ; politique IPSec appliquée sur la mauvaise interface.
5. À empêcher que le trafic destiné au site distant soit traduit (NATé) avant d'entrer
   dans le tunnel — sinon le tunnel ne reconnaît plus le trafic et rien ne passe.
6. Elle retire automatiquement la route statique (ici la route par défaut via le WAN
   principal) quand la sonde NQA échoue, permettant à la route de secours de prendre
   le relais.
7. Parce que l'en-tête PPPoE réduit la MTU utile (1492) ; sans ajustement du MSS TCP,
   certains flux se fragmentent ou se bloquent (pages web qui ne chargent pas).
8. Une règle `untrust → local` qui autorise UDP 500 et UDP 4500 (IKE / NAT-T) vers le
   routeur lui-même.
9. Une MTU différente entre les deux interfaces (classique), ou un router-id dupliqué.
10. Parce que la communauté transite en clair (interception + usurpation faciles) ;
    utiliser SNMPv3 (authentification + chiffrement), restreint par ACL au superviseur.

---
---

# Partie R — Pour aller plus loin

## 131. Monter en gamme et autour du produit

- **Grand frère AR730** : mêmes slots, 6 Gbit/s sortants, 1 port 10GE optique en plus,
  1200 terminaux — si l'AR720 sature (CPU, débit), c'est la marche suivante logique
  (**à valider avec le fournisseur** selon les prix et la dispo).
- **SD-WAN Huawei** : pour un parc de dizaines d'agences, l'orchestration centralisée
  (iMaster NCE) change la vie par rapport au CLI site par site.
- **Contrôleur WLAN intégré** : l'AR720 peut gérer des AP Huawei (CAPWAP) — pratique
  pour une agence avec 2–5 bornes sans contrôleur dédié.

## 132. Documentation officielle à garder sous la main

- Datasheet « Huawei NetEngine AR700 Series Enterprise Router Data Sheet » (source des
  valeurs hardware de ce guide).
- Guide d'installation hardware AR720 (câblage, rack, mise à la terre).
- « VRP Configuration Guide » de la version exacte (`display version`) : la référence
  pour chaque fonction (IPSec, OSPF, QoS…).
- Notes de version (release notes) avant chaque upgrade : comportements modifiés,
  failles corrigées.

## 133. Sujets à approfondir après ce guide

1. IPv6 sur l'AR720 (adressage, DHCPv6, OSPFv3) — le dual-stack arrive vite.
2. BGP pour les sites multi-opérateurs ou les interconnexions partenaires.
3. MPLS L3VPN si le cœur du réseau opérateur l'exige.
4. PKI complète et renouvellement automatisé des certificats VPN.
5. Scripts d'exploitation : backup automatisé via expect/Ansible (module `community.network`
   ou scripts SSH), parsing de `display` en Python.
6. Intégration au superviseur (Zabbix/Prometheus SNMP) avec templates Huawei.

## 134. Dernier mot : la discipline qui fait la différence

Le routeur n'est jamais le problème le plus fréquent : c'est l'exploitation qui fait la
différence. `save` systématique, sauvegardes externes datées, dossier de site à jour,
checklist mensuelle faite pour de vrai, firmware suivi, mots de passe uniques. Un AR720
bien exploité tourne des années sans un ticket. Bon courage sur le terrain.

---

*Fin du guide — Huawei NetEngine AR720. Rédigé le 2026-09-27. Valeurs hardware issues de
la datasheet AR700 Series ; vérifier la version VRP exacte (`display version`) et la fiche
du modèle livré pour les points marqués « à vérifier ».*
