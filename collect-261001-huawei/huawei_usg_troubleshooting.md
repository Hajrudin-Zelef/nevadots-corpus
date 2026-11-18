# Huawei USG — Guide de dépannage (troubleshooting récurrent)

> Les pannes les plus fréquentes sur USG6000 (V500/V600), avec pour chacune :
> symptômes, causes probables, commandes de diagnostic et corrections.
> Méthode générale : toujours dépanner **de bas en haut** —
> 1. Physique / interfaces, 2. Zones, 3. Security-policy,
> 4. NAT, 5. Routage, 6. UTM / applicatif.
> La majorité des « pannes » USG sont des oublis de **security-policy**
> ou de **NAT** : le pare-feu refuse par défaut.

---

## 1. Réflexes de base avant tout diagnostic

```shell
display version                    # version logicielle (noter le VxxxRxxx)
display device                     # état matériel, cartes
display cpu-usage                  # CPU > 80 % = suspect
display memory-usage               # mémoire saturée = sessions/fuites
display interface brief            # UP/DOWN de toutes les interfaces
display ip interface brief
display logbuffer                  # erreurs récentes
display trapbuffer                 # alarmes
display clock                      # une horloge fausse casse les logs/VPN !
```

- Vérifiez **l'heure** en premier sur les problèmes VPN/certificats/licence.
- `save` **après** chaque correction validée, jamais avant.
- En cas de doute sur une règle : `display security-policy rule all`
  puis regardez l'ordre des règles (la première qui matche gagne).

---

## 2. Pas d'accès Internet depuis le LAN

**Symptômes** : les postes n'ouvrent aucune page, `ping 8.8.8.8` échoue
depuis un PC du LAN.

**Checklist ordonnée** :

```shell
# a) L'interface LAN est-elle UP avec la bonne IP ?
display interface GigabitEthernet 0/0/1
display ip interface brief

# b) Le PC a-t-il une passerelle ? (depuis le USG)
ping -a 192.168.1.1 192.168.1.50
display arp all | include 192.168.1.50

# c) La route par défaut existe-t-elle ?
display ip routing-table 0.0.0.0

# d) Le NAT source est-il configuré ET matche-t-il ?
display nat-policy rule all
display nat session all | include 192.168.1.50

# e) La security-policy autorise-t-elle trust -> untrust ?
display security-policy rule all
display firewall session table source-ip 192.168.1.50

# f) Le DNS fonctionne-t-il ?
nslookup www.google.com
# (depuis le PC : ping 8.8.8.8 OK mais pas de navigation = DNS)
```

**Corrections typiques** :
- Interface DOWN : câble, `undo shutdown` sur l'interface.
- Pas de route par défaut : `ip route-static 0.0.0.0 0.0.0.0 <passerelle>`.
- NAT oublié : ajouter la règle `nat-policy` trust→untrust
  (`action source-nat easy-ip`, voir guide CLI §8).
- Règle security-policy manquante ou **mal ordonnée** (une règle `deny`
  placée avant la règle `permit`).
- Interface WAN pas dans la zone `untrust` (`firewall zone untrust` →
  `add interface ...`).
- PPPoE : `display pppoe-client session summary` — session absente =
  identifiants faux ou ligne KO.

---

## 3. Le NAT ne fonctionne pas

**Symptômes** : `display nat session all` vide alors que le trafic part ;
ou certains protocoles (FTP, SIP, jeux) cassés.

```shell
display nat-policy rule all          # la règle existe ?
display nat address-group            # le pool a des IP libres ?
display nat session all              # sessions traduites visibles ?
display nat session source-ip 192.168.1.50
```

**Causes fréquentes** :
- La règle NAT ne matche pas : `source-zone` / `destination-zone`
  inversées, ou `source-address` trop restrictif.
- Pool épuisé : trop de sessions pour trop peu d'IP publiques
  (élargir le pool ou passer en `easy-ip`).
- Protocoles à ports dynamiques (FTP actif, SIP) : activer l'**ALG**
  correspondant (web UI : Network > ALG, ou vérifier qu'il n'est pas
  désactivé).
- Double NAT en amont (box opérateur) : le USG reçoit déjà une IP
  privée → préférer le mode bridge/DMZ de la box, ou accepter le
  double NAT (casse IPsec sans NAT-T).
- Pour tester sans la security-policy : créez une règle `permit` large
  temporaire, validez le NAT, puis resserrez.

---

## 4. Une règle security-policy bloque (ou laisse passer) à tort

```shell
# Voir les sessions réellement établies pour une IP
display firewall session table source-ip 192.168.1.50
display firewall session table destination-port 443

# Voir l'ordre et le contenu des règles
display security-policy rule all

# Statistiques globales
display firewall statistics
```

**Méthode** :
1. Reproduisez le flux, puis regardez `display firewall session table` :
   session absente = bloqué par une politique (ou pas de route/NAT).
2. Remontez les règles **dans l'ordre** : la première règle dont
   (source-zone, destination-zone, adresses, service, schedule)
   correspondent décide.
3. Pièges classiques :
   - règle `deny any any` placée **avant** les permits ;
   - `destination-zone local` oubliée pour administrer le USG ;
   - `schedule time-range` expiré (la règle ne s'applique plus !) ;
   - objet `address-set` vide ou mal renseigné ;
   - trafic retour bloqué : normalement inutile (stateful), mais si la
     session a expiré (`display firewall session table verbose`),
     augmentez le timeout du service.

**Fix** : réordonner (`move rule`), corriger les objets, ou ajouter une
règle de log temporaire (`action permit` + session-logging) pour voir.

---

## 5. Serveur publié inaccessible depuis Internet (NAT server)

**Symptômes** : `nat server` configuré, mais pas de connexion entrante.

```shell
display nat server
display security-policy rule all     # règle untrust -> dmz/trust ?
display firewall session table destination-ip 192.168.50.10
```

**Checklist** :
1. La règle security-policy **untrust → dmz** (ou trust) existe avec le
   bon `service` (port) et matche l'IP **privée** destination
   (le NAT server s'applique avant la politique : la règle voit l'IP
   interne !).
2. Le serveur a bien le USG comme **passerelle par défaut** (sinon le
   retour part ailleurs → session asymétrique, paquets jetés).
3. Pas de conflit : le port public n'est pas déjà utilisé par un autre
   `nat server` ou par un service du USG lui-même.
4. FAI qui bloque le port (25, 80, 443 courants) : testez depuis
   l'extérieur avec un autre port.
5. ARP : si l'IP publique n'est pas celle de l'interface, le USG répond
   en proxy-ARP par défaut ; sinon déclarez-la.
6. Depuis le LAN, l'accès via l'IP **publique** ne marche pas sans
   hairpin : utilisez le DNS interne ou l'IP privée en interne.

---

## 6. VPN IPsec : le tunnel ne monte pas

**Symptômes** : `display ike sa` vide.

```shell
display ike sa
display ike proposal
display ipsec proposal
# Côté réseau :
ping <ip-publique-distante>
display ip routing-table <ip-publique-distante>
```

**Phase 1 (IKE) — checklist** :
1. Connectivité : les deux pairs se pinguent en public ?
2. `remote-address` correct des deux côtés (attention aux IP
   dynamiques : utilisez alors l'identification par FQDN ou aggressive
   mode côté dynamique).
3. **Pre-shared-key identique** (erreur n°1, invisible dans les logs).
4. Propositions IKE compatibles : encryption / DH group / auth-algo
   doivent avoir une intersection (même `ike proposal` des 2 côtés =
   le plus simple).
5. UDP 500/4500 et protocole ESP (50) **autorisés** dans la
   security-policy entre `local` et `untrust` dans les deux sens.
6. Horloge : un décalage > quelques minutes peut faire échouer
   l'authentification (surtout avec certificats).
7. NAT-T : si un NAT est sur le chemin, les deux côtés doivent
   l'accepter (automatique en général, UDP 4500).

**Phase 2 (IPsec) — `ike sa` OK mais `ipsec sa` vide** :
1. Propositions IPsec compatibles (ESP auth/encryption).
2. **ACL miroir** : `permit ip source LAN_A destination LAN_B` d'un
   côté, `permit ip source LAN_B destination LAN_A` de l'autre.
   C'est l'erreur n°1 de la phase 2.
3. PFS (Perfect Forward Secrecy) : même groupe DH des deux côtés
   (ou désactivé des deux côtés).

```shell
display ipsec sa
display acl 3000        # vérifier le contenu de l'ACL
```

---

## 7. VPN monté mais pas de trafic

**Symptômes** : `display ipsec sa` montre des SA actifs, mais les LAN ne
se pinguent pas.

```shell
display ipsec sa                         # encaps/décapsulent ?
display firewall session table           # sessions visibles ?
```

**Causes** :
1. Security-policy : le trafic entre zones (trust→untrust ou vers la
   zone du tunnel) n'est pas autorisé. **Le VPN ne bypass pas la
   politique !**
2. Routage : il manque la route vers le LAN distant via le tunnel
   (en mode policy-based, le routage suit l'ACL ; en route-based,
   route statique vers l'interface Tunnel).
3. NAT qui « mange » le trafic VPN : exclure le trafic IPsec du NAT
   source (règle `nat-policy` avec `action no-nat` placée **avant**
   la règle de SNAT, ou ACL de NAT qui exclut les LAN distants).
4. MTU : les paquets ESP fragmentés sont parfois jetés → tester avec
   `ping -s 1400` puis ajuster (`mtu`, `tcp adjust-mss`).
5. Chevauchement d'adressage : les deux LAN utilisent le même
   sous-réseau → il faut du NAT avant chiffrement (rare, à éviter).

---

## 8. Lenteurs / CPU ou mémoire à 100 %

```shell
display cpu-usage
display memory-usage
display firewall session table | count   # nombre de sessions (approx)
display device
display logbuffer
```

**Causes fréquentes** :
- **Table de sessions saturée** (attaque ou P2P) : identifiez les
  tops parleurs (`display firewall session table` trié), bloquez en
  `firewall blacklist`, réduisez les timeouts.
- **UTM sur tout le trafic** : l'IPS/AV sur des flux lourds (sauvegardes,
  vidéo) coûte cher → excluez les flux de confiance des profils, ou
  passez l'IPS en détection seule d'abord.
- **Logs excessifs** : session-logging sur une règle très sollicitée =
  I/O disque/CPU → ne loguez que les règles sensibles.
- **Debugging oublié** : `undo debugging all` + `undo terminal debugging`
  (un debug resté actif plombe les performances).
- Boucle réseau en amont : broadcast storm → `display interface`
  (erreurs/overruns en forte hausse).
- Version logicielle buggée : comparez avec les release notes Huawei.

**Fix d'urgence** : `reset firewall session table` (casse les sessions
établies !) pour respirer, puis traitez la cause.

---

## 9. Problèmes DNS

**Symptômes** : `ping 8.8.8.8` OK mais aucun nom ne se résout.

```shell
nslookup www.google.com
display dns server          # selon version
display ip routing-table 8.8.8.8
```

- `dns resolve` oublié dans la config.
- Serveur DNS injoignable ou filtré par la security-policy
  (autoriser UDP 53 trust→untrust).
- Sur le LAN : le DHCP distribue-t-il les bons DNS ?
  (`display ip pool`, `dns-list`).
- DNS hijack de l'opérateur : testez avec `8.8.8.8` en direct.

---

## 10. Le DHCP ne distribue pas d'adresses

```shell
display ip pool name LAN_POOL
# (used/total : pool plein ?)
display interface GigabitEthernet 0/0/1   # dhcp select global présent ?
```

- `dhcp enable` oublié (erreur classique).
- Interface pas en `dhcp select global` (ou `interface`).
- Pool épuisé : élargissez le `network` ou réduisez le `lease`.
- Conflit : une IP statique dans la plage du pool.
- Le client est dans une autre VLAN/zone sans relay DHCP.

---

## 11. Impossible d'administrer le USG (web/SSH/ping)

```shell
# En console (toujours accessible) :
display interface brief
display zone
display security-policy rule all
```

- **Règle vers la zone `local` manquante** : c'est le cas n°1 après un
  reset ou une nouvelle config (voir guide CLI §6, règle ADMIN_TO_FW).
- Web : `display web-manager` — service désactivé ou port changé ?
  URL = `https://<ip>:8443`.
- SSH : `display ssh server status`, `display ssh user-information`.
  Protocole VTY restreint à telnet ? (`protocol inbound ssh`).
- Compte verrouillé après échecs : attendez ou débloquez en console.
- Vous êtes dans la zone `untrust` : l'admin depuis Internet est
  (heureusement) refusé par défaut → passez par VPN.

---

## 12. HA : pas de bascule / split-brain

```shell
display hrp state verbose
display hrp interface
display vrrp
```

- Lien heartbeat DOWN : câble, VLAN, ou IP `remote` fausse dans
  `hrp interface ... remote ...`.
- `hrp enable` oublié sur un des deux nœuds.
- Versions logicielles **différentes** : la synchro échoue.
- Les deux nœuds en `active` (split-brain) : heartbeat coupé des deux
  côtés → rétablissez le lien, un seul doit rester actif.
- Config non synchronisée : `hrp auto-sync config` puis vérifiez
  `display hrp state verbose` (state = `normal`).
- VGMP : si un groupe VRRP ne bascule pas avec les autres, vérifiez
  son rattachement au groupe VGMP.

---

## 13. OSPF / BGP qui ne s'établit pas

```shell
display ospf peer
display ospf interface
display bgp peer
display ip routing-table
```

- Interfaces pas dans la même zone avec politique autorisée : **OSPF/BGP
  entre zones passe par la security-policy** (trust→local / local→trust
  pour les paquets à destination du USG lui-même).
- `network` mal déclaré (wildcard inversé en OSPF).
- MTU mismatch (voisin reste en ExStart) : `display ospf peer verbose`.
- Authentification MD5 : mdp différent des deux côtés.
- BGP : `as-number` du peer faux, ou `ebgp` sans `peer ebgp-max-hop`
  sur plusieurs sauts.
- Router-ID dupliqué dans la zone OSPF.

---

## 14. L'UTM bloque à tort (faux positifs)

**Symptômes** : une appli métier cassée depuis l'activation IPS/AV/URL.

1. Identifiez le moteur : désactivez les profils **un par un** dans la
   règle (d'abord URL-filter, puis AV, puis IPS) jusqu'à trouver le
   coupable.
2. IPS : passez le profil en **alerte seule** d'abord, analysez les logs
   (`display logbuffer`), puis créez des exceptions pour les signatures
   fautives.
3. URL-filter : mettez le domaine métier en liste blanche.
4. AV : excluez les types de fichiers internes volumineux.
5. Vérifiez que la **base de signatures est à jour** : une base
   périmée = faux positifs + trous de sécurité (`display utm`,
   Update Center).
6. HTTPS : sans inspection SSL, l'UTM est aveugle sur le chiffré ;
   avec, un certificat mal déployé casse la navigation → vérifiez le
   CA sur les postes.

---

## 15. Licence / signatures expirées

```shell
display license
```

- Licence UTM/IPS expirée : les moteurs passent en mode dégradé ou
  s'arrêtent → renouvelez via le portail Huawei (fichier `.dat`,
  import en web UI).
- Après import : `reboot` parfois nécessaire selon le type de licence.
- Signatures non mises à jour depuis des mois : planifiez la mise à
  jour automatique (Update Center > Schedule).

---

## 16. Trafic asymétrique (le tueur silencieux)

**Symptômes** : sessions qui se montent puis tombent, applis qui
« rament », VPN instables.

- Le USG est **stateful** : si le retour ne repasse pas par lui
  (double chemin, deux FAI sans PBR), il jette les paquets.
- Diagnostic : `display firewall session table` montre des sessions
  à moitié établies ; `tracert` aller vs retour divergent.
- Fix : symétrisez les chemins (PBR, routage), ou en dernier recours
  désactivez la vérification d'état sur le flux concerné (à éviter en
  production).

---

## 17. Table de sessions pleine / attaque en cours

```shell
display firewall session table | include <ip-suspecte>
display firewall statistics
display logbuffer | include attack
```

- En cas de flood : `firewall blacklist <ip>` immédiatement.
- Vérifiez `Attack Defense` (SYN flood, UDP flood) : seuils trop hauts ?
- Rate-limitez en amont si le lien est saturé avant le USG.
- Après l'attaque : `reset firewall session table` pour nettoyer
  (coupure brève des sessions légitimes).

---

## 18. Échec d'upgrade / boot en boucle

1. Ne paniquez pas : le USG garde l'ancienne image (`display startup`
   montre les images primaire/secours).
2. En BootWare (Ctrl+B au démarrage) : choisissez l'image précédente.
3. Causes d'échec : image corrompue (revérifiez le hash/MD5 après TFTP),
   espace flash insuffisant (`dir` : nettoyez les vieux fichiers),
   coupure de courant pendant l'écriture (d'où l'onduleur).
4. Après upgrade : `display version` + `display device` pour valider,
   puis `save`.

---

## 19. Mot de passe perdu

- **Console** : au démarrage, Ctrl+B → menu BootWare →
  « Clear password for console user ».
- **Web/SSH** : via un compte admin valide restant, ou en console :
  `aaa` → `local-user <nom> password irreversible-cipher <nouveau>`.
- Dernier recours : `reset saved-configuration` + reboot = retour
  usine (perte de config ! sauvegardez avant toute manip risquée).

---

## 20. Fiche réflexe : la panne Internet en 5 minutes

```shell
# 1. Interfaces
display interface brief
# 2. Passerelle vue ?
ping -a <ip-lan-usg> <ip-pc>
# 3. Route par défaut ?
display ip routing-table 0.0.0.0
# 4. NAT ?
display nat session all
# 5. Politique ?
display security-policy rule all
display firewall session table source-ip <ip-pc>
# 6. DNS ?
nslookup www.google.com
# 7. WAN ?
ping <passerelle-fai>
display pppoe-client session summary   # si PPPoE
```

Si les 7 points sont verts et que ça ne marche toujours pas :
regardez les **logs** (`display logbuffer`) et désactivez
temporairement l'UTM sur la règle pour isoler.

---

## 21. Commandes à connaître par cœur

| Besoin | Commande |
|---|---|
| Sessions actives | `display firewall session table` |
| Sessions NAT | `display nat session all` |
| Règles de sécurité | `display security-policy rule all` |
| Règles NAT | `display nat-policy rule all` |
| Table de routage | `display ip routing-table` |
| Voisins OSPF/BGP | `display ospf peer` / `display bgp peer` |
| Tunnels VPN | `display ike sa` / `display ipsec sa` |
| État HA | `display hrp state verbose` |
| Charge | `display cpu-usage` / `display memory-usage` |
| Logs | `display logbuffer` / `display trapbuffer` |
| Nettoyer sessions | `reset firewall session table` |
| Nettoyer NAT | `reset nat session all` |
| Debug (temporaire !) | `terminal debugging` + `debugging ...` puis `undo debugging all` |

---

*Fin du guide — ~550 lignes. Règle d'or : 80 % des pannes USG =
security-policy ou NAT. Vérifiez toujours dans cet ordre :
interface → zone → politique → NAT → routage → UTM.*
