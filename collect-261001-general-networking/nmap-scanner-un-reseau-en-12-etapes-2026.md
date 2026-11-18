Un scan Nmap mal ciblé peut faire tomber un onduleur industriel, déclencher une alerte au SOC d’un client ou, pire, vous exposer aux trois ans de prison prévus par l’article 323-1 du Code pénal. Et pourtant, Nmap reste en 2026 l’outil que la quasi-totalité des analystes réseau ouvrent en premier pour cartographier une infrastructure. La version 7.991, sortie début août 2026, vient encore renforcer sa base cryptographique. Ce tutoriel vous emmène de l’installation jusqu’à un projet complet d’audit réseau automatisé, avec le cadre légal français clarifié dès le départ.

**Avertissement d’usage :** ce guide s’adresse aux administrateurs systèmes, aux équipes sécurité et aux étudiants qui scannent leur propre infrastructure ou un périmètre couvert par une autorisation écrite. Scanner un système tiers sans accord expose à des poursuites pénales, détaillées plus bas. Tech Insider ne cautionne aucun usage non autorisé de **Nmap**.

## Qu’est-ce que Nmap et pourquoi il reste incontournable en 2026

**Nmap** (Network Mapper) est un scanner réseau open source créé en 1997 par Gordon Lyon, alias Fyodor. Sa mission n’a pas changé depuis : découvrir les hôtes actifs sur un réseau, lister leurs ports ouverts, identifier les services qui tournent dessus et, si besoin, deviner le système d’exploitation. Ce qui a changé, en revanche, c’est l’échelle et la profondeur de l’outil. La dernière version stable, **Nmap 7.991**, publiée début août 2026, embarque OpenSSL 3.0.21 et corrige plusieurs failles de la bibliothèque libssh2 qu’elle intègre (CVE-2025-15661, CVE-2026-7598, CVE-2026-55199, CVE-2026-55200, CVE-2026-58050 et CVE-2026-58051), toutes patchées dans une branche interne suivie sous le nom 1.11.1_NMAP1.

Plusieurs guides spécialisés publiés entre 2025 et 2026 classent encore **Nmap** parmi les cinq à dix outils les plus utilisés par les professionnels de la cybersécurité, aux côtés de Wireshark, Metasploit et Burp Suite. Le paquet Python `python3-nmap`, qui permet de piloter l’outil depuis des scripts d’automatisation, cumulait environ 190 000 téléchargements sur le mois de juin 2026 selon PyPI Stats, un signal net de l’usage massif de Nmap dans des pipelines automatisés plutôt qu’en ligne de commande isolée. Sur Windows, un seul miroir de téléchargement (Uptodown) recensait plus de 150 000 installations fin août 2025.

Concrètement, un administrateur réseau utilise Nmap pour trois choses : inventorier les machines connectées à un sous-réseau qu’il ne maîtrise pas totalement, vérifier qu’aucun port sensible n’est resté ouvert après un déploiement, et détecter des services obsolètes ou mal configurés avant qu’un attaquant ne le fasse à sa place. C’est un outil défensif autant qu’offensif : les deux usages emploient exactement la même commande.

Ce qui distingue Nmap de la plupart des scanners commerciaux, c’est sa communauté. Le projet publie son code source ouvertement, documente chaque option dans un manuel consultable en ligne et laisse des centaines de contributeurs indépendants écrire des scripts NSE pour des cas d’usage très spécifiques : détection de ransomware sur des partages SMB, audit de configuration TLS, énumération de bases de données exposées. Cette ouverture explique pourquoi Nmap reste enseigné dans la quasi-totalité des cursus de cybersécurité en France, du BTS SIO aux formations RSSI, et pourquoi il figure systématiquement dans la boîte à outils livrée avec des distributions comme Kali Linux ou Parrot OS.

## Nmap est-il légal en France ? Ce que dit précisément le Code pénal

C’est la question que devrait se poser quiconque tape `nmap` suivi d’une adresse IP qui n’est pas la sienne. En France, l’infraction de référence est l’**article 323-1 du Code pénal**, qui réprime « le fait d’accéder ou de se maintenir, frauduleusement, dans tout ou partie d’un système de traitement automatisé de données » (STAD). La peine de base est de **trois ans d’emprisonnement et 100 000 € d’amende**. Si le scan entraîne une suppression ou une altération de données, la peine grimpe à cinq ans et 150 000 €. Et si le système visé est un STAD contenant des données à caractère personnel géré par l’État, la sanction peut atteindre sept ans d’emprisonnement et 300 000 € d’amende, selon les chiffres actualisés cités par le cabinet ACI et le rapport 2026 de l’ICLG sur le droit de la cybersécurité en France.

La nuance juridique porte sur le scan de ports lui-même. Des analyses de l’OSSIR datant d’un colloque JSSI, encore citées dans la doctrine actuelle, expliquent qu’un scan SYN classique n’établit jamais de connexion TCP complète : le paquet SYN part, la cible répond par SYN/ACK, mais Nmap coupe la poignée de main avant le dernier ACK. Techniquement, il n’y a donc pas toujours d’« accès » au sens strict de l’article 323-1. Cela dit, plusieurs analyses juridiques plus récentes, dont un rapport 2026 sur la législation cyber française, rappellent que scanner un système tiers sans autorisation peut être retenu comme un **acte préparatoire** à une intrusion, ce qui suffit à justifier des poursuites, surtout si le scan est suivi d’une tentative d’exploitation.

En pratique, la règle à retenir est simple : ne scannez jamais un périmètre que vous ne possédez pas ou pour lequel vous n’avez pas une **autorisation écrite** (contrat, lettre de mission, ordre de mission pentest). Pour vos propres tests, restez sur votre réseau domestique, un labo virtualisé (Metasploitable, DVWA) ou un environnement cloud dédié aux tests d’intrusion autorisés. Le site de Cybermalveillance.gouv.fr publie des rappels réguliers sur ce cadre légal pour les professionnels comme pour les particuliers.

## Prérequis : versions, matériel et environnement de test

Avant de lancer la première commande, vérifiez que votre environnement correspond à ces prérequis. Ils sont volontairement modestes : Nmap tourne aussi bien sur un Raspberry Pi que sur un poste de travail professionnel.

| Élément | Version / configuration recommandée | Remarque | 
|---|---|---|
| Nmap | 7.991 (août 2026) ou 7.99 minimum | Versions antérieures à 7.98 exposées à CVE-2025-43715 sur l’installeur Windows | 
| Système d’exploitation | Linux (Ubuntu 24.04+, Debian 12+), Windows 10/11, macOS 14+ | Comportement légèrement différent selon l’OS pour les scans SYN | 
| Droits d’exécution | root / sudo (Linux, macOS), administrateur (Windows) | Obligatoire pour les scans SYN (-sS) et la détection d’OS (-O) | 
| Zenmap (interface graphique) | zenmap-7.991-py3 | Nécessite Python 3 ; facultatif si vous restez en CLI | 
| Cible de test | Machine virtuelle Metasploitable2/3 ou votre propre réseau local | Ne jamais cibler un tiers sans autorisation écrite | 
| Espace disque | 200 Mo minimum | Plus si vous stockez des exports XML volumineux | 

Comptez environ 30 minutes pour l’installation et les premiers scans, et jusqu’à 90 minutes si vous allez au bout du projet complet d’audit automatisé présenté à l’étape 12.

## Étape 1 : installer Nmap sur Linux, Windows et macOS

L’installation varie selon la plateforme, mais reste triviale partout. Sur les distributions Linux basées sur Debian ou Ubuntu :

```
sudo apt update
sudo apt install nmap -y
nmap --version
```
Sur Fedora, RHEL ou CentOS :

`sudo dnf install nmap -y`
Sur macOS, la méthode la plus fiable passe par Homebrew :

`brew install nmap`
Sur Windows, téléchargez l’installeur officiel depuis nmap.org (version 7.991), qui embarque désormais Npcap avec le pilote de capture réseau nécessaire aux scans bruts. L’installeur a été reconstruit avec NSIS 3.11 depuis la version 7.98, ce qui corrige la faille d’escalade de privilèges CVE-2025-43715 présente dans les anciens installeurs NSIS exécutés en tant que SYSTEM. Ne réutilisez donc jamais un installeur Nmap téléchargé avant août 2025.

Pour ceux qui préfèrent l’interface graphique, Zenmap reste disponible en package Python 3 (zenmap-7.991-py3) et permet de visualiser une topologie réseau sans mémoriser la syntaxe complète.

## Étape 2 : vérifier l’installation et lancer le premier scan

Une fois l’installation terminée, vérifiez la version exacte et lancez un scan minimal sur votre propre machine (localhost) pour confirmer que tout fonctionne :

```
nmap --version
# Nmap version 7.991 ( https://nmap.org )
# Platform: x86_64-pc-linux-gnu
# Compiled with: liblua-5.4.7 openssl-3.0.21 libssh2-1.11.1_NMAP1 ...
nmap 127.0.0.1
```
La sortie typique ressemble à ceci sur un poste Linux fraîchement installé :

```
Starting Nmap 7.991 ( https://nmap.org ) at 2026-08-25 14:02 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.000035s latency).
Not shown: 998 closed tcp ports (reset)
PORT     STATE SERVICE
22/tcp   open  ssh
631/tcp  open  ipp
Nmap done: 1 IP address (1 host up) scanned in 0.19 seconds
```
Si vous obtenez ce type de résultat, votre installation est fonctionnelle et vous pouvez passer aux scans réseau réels, toujours sur un périmètre autorisé.

## Étape 3 : découvrir les hôtes actifs sur le réseau

Avant de scanner des ports, il faut savoir quelles machines répondent. C’est le rôle du **ping scan**, déclenché avec l’option `-sn` (anciennement `-sP`), qui désactive le scan de ports et se contente de la découverte d’hôtes :

`nmap -sn 192.168.1.0/24`
Cette commande balaie les 254 adresses possibles du sous-réseau et renvoie la liste des hôtes qui répondent à un ping ICMP, une requête ARP (sur réseau local) ou une sonde TCP sur les ports 80 et 443. Sur un réseau domestique typique, le résultat prend quelques secondes et liste la box internet, les objets connectés, les ordinateurs et les smartphones actifs. C’est souvent la première surprise d’un audit : la plupart des foyers découvrent deux à trois fois plus d’appareils connectés qu’ils ne l’imaginaient.

## Étape 4 : scanner les ports TCP, SYN contre Connect

Une fois les hôtes actifs identifiés, l’étape suivante consiste à scanner leurs ports ouverts. Nmap propose deux techniques principales pour le TCP :

- **Scan SYN (-sS)** : envoie un paquet SYN et attend la réponse sans terminer la connexion. Rapide, discret, mais nécessite les droits root/administrateur pour manipuler les paquets bruts.
- **Scan Connect (-sT)** : utilise l’appel système`connect()` standard, termine la connexion TCP normalement. Plus lent et plus visible dans les journaux, mais ne demande aucun privilège particulier.

```
# Scan SYN, nécessite sudo
sudo nmap -sS 192.168.1.10
# Scan Connect, sans privilège root
nmap -sT 192.168.1.10
```
Pour cibler l’ensemble des 65 535 ports plutôt que les 1 000 ports les plus courants scannés par défaut, ajoutez `-p-` :

`sudo nmap -sS -p- 192.168.1.10`
Ce scan complet prend nettement plus de temps (plusieurs minutes contre quelques secondes) mais révèle des services qui tournent sur des ports non standards, une technique de dissimulation fréquente chez les attaquants qui déplacent un service SSH du port 22 vers un port à quatre chiffres.

## Étape 5 : scanner les ports UDP et composer avec les pare-feu

Le protocole UDP, utilisé par le DNS, le SNMP ou encore les services VoIP, se scanne différemment avec l’option `-sU`. Contrairement au TCP, l’absence de réponse ne signifie pas forcément qu’un port est ouvert : UDP n’accuse pas systématiquement réception.

`sudo nmap -sU --top-ports 100 192.168.1.10`
Un scan UDP complet sur 65 535 ports peut prendre plusieurs heures selon la configuration du pare-feu cible, c’est pourquoi on se limite en général aux ports les plus courants (`--top-ports`) en contexte d’audit régulier. Si la cible se trouve derrière un pare-feu qui bloque les échos ICMP, Nmap peut la considérer comme éteinte alors qu’elle répond bien. L’option `-Pn` force le scan sans phase de découverte préalable :

`sudo nmap -Pn -sS 192.168.1.10`
## Étape 6 : détecter les services et leurs versions

Savoir qu’un port est ouvert ne dit rien sur ce qui tourne dessus. L’option `-sV` interroge chaque port ouvert pour identifier le service exact et sa version :

`sudo nmap -sV 192.168.1.10````
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.5
80/tcp  open  http    nginx 1.26.1
3306/tcp open  mysql   MySQL 8.0.39
```
Cette information est précieuse pour croiser les versions détectées avec des bases de vulnérabilités connues : un OpenSSH ou un MySQL non patché depuis des mois est le premier signal qu’un auditeur va relever. Combinez `-sV` avec un niveau d’intensité plus élevé (`--version-intensity 9`) si les sondes par défaut ne suffisent pas à identifier un service exotique.

## Étape 7 : identifier le système d’exploitation

L’option `-O` déclenche la détection d’OS, basée sur l’analyse des caractéristiques de la pile TCP/IP de la cible (taille de fenêtre, options TCP, TTL initial). Elle nécessite les droits root :

`sudo nmap -O 192.168.1.10`
La commande combinée la plus utilisée par les praticiens en 2026, selon plusieurs cheat sheets publiés cette année, reste `-A`, qui active en une seule fois la détection d’OS, la détection de version, les scripts NSE par défaut et un traceroute :

`sudo nmap -A 192.168.1.10`
Gardez à l’esprit que la détection d’OS reste probabiliste : Nmap indique un pourcentage de certitude et peut se tromper sur des systèmes durcis, des conteneurs ou des machines derrière un pare-feu qui normalise les paquets sortants.

## Étape 8 : ajuster la vitesse avec les modèles de timing

Nmap propose six profils de vitesse, de `-T0` (paranoïaque, un paquet toutes les cinq minutes) à `-T5` (insane, aussi rapide que possible au risque de fausses négatives). Le profil par défaut est `-T3`.

| Template | Nom | Cas d’usage typique | 
|---|---|---|
| -T0 | Paranoid | Contournement d’IDS très sensible, scan sur plusieurs jours | 
| -T1 | Sneaky | Scan discret sur réseau surveillé | 
| -T2 | Polite | Limite la charge réseau sur des liens fragiles | 
| -T3 | Normal | Profil par défaut, équilibre vitesse/fiabilité | 
| -T4 | Aggressive | Réseau local rapide, audit interne planifié | 
| -T5 | Insane | Labo de test uniquement, risque de résultats incomplets | 

En environnement professionnel, `-T4` est le choix le plus courant pour un audit interne planifié en dehors des heures de production, tandis que `-T2` convient mieux à un scan mené en journée sur une infrastructure sensible où la latence compte.

## Étape 9 : automatiser l’audit avec le moteur de scripts NSE

Le **Nmap Scripting Engine (NSE)** transforme Nmap en véritable plateforme d’audit. Les scripts sont regroupés en catégories : `default` (sûrs, activés par `-sC`), `vuln` (vérification active de vulnérabilités connues), `auth`, `brute`, `intrusive` et `exploit`. Pour lancer une recherche de vulnérabilités documentées sur une cible :

`sudo nmap -sV --script vuln 192.168.1.10`
Le script `vulners` pousse la logique plus loin en interrogeant la base Vulners.com pour chaque service identifié, et renvoie directement les identifiants CVE et les scores CVSS correspondants :

`sudo nmap -sV --script vulners 192.168.1.10`
Parmi les scripts les plus utilisés en audit défensif en 2025-2026 : `smb-vuln-ms17-010` (détection d’EternalBlue), `ssl-heartbleed`, `ssl-enum-ciphers` (audit de la configuration TLS), `http-enum` (énumération de répertoires web) et `dns-zone-transfer` (détection de transferts de zone DNS mal configurés). La documentation complète est disponible sur le portail NSEDoc officiel.

## Cartographier un environnement Windows et Active Directory

Un cas d’usage très fréquent en entreprise consiste à cartographier un domaine Active Directory avant un audit ou une migration. Nmap dispose de plusieurs scripts NSE dédiés au protocole SMB qui permettent d’identifier les contrôleurs de domaine, les partages accessibles et les comptes exposés sans authentification :

`sudo nmap -p 88,389,445,3268 --script smb-os-discovery,smb-enum-shares 192.168.1.0/24`
Cette commande cible les ports caractéristiques d’un environnement Windows : Kerberos (88), LDAP (389), SMB (445) et le catalogue global (3268). Le script `smb-os-discovery` révèle la version exacte de Windows Server installée sur chaque machine répondante, une information précieuse quand on sait que des failles critiques comme Netlogon (CVE visant les contrôleurs de domaine non patchés) continuent de circuler activement sur des parcs mal maintenus. Le script `smb-enum-shares`, de son côté, liste les partages réseau accessibles, y compris ceux ouverts par erreur à des utilisateurs non authentifiés, un défaut de configuration que l’on retrouve encore régulièrement lors d’audits menés en 2026.

Sur ce type de scan, restez particulièrement prudent : certains scripts de la catégorie `brute` ou `intrusive` peuvent verrouiller des comptes Active Directory après plusieurs tentatives d’authentification échouées, avec un impact direct sur la production. Limitez-vous aux catégories `default`, `discovery` et `safe` lors d’un premier passage, et réservez les scripts plus agressifs à une fenêtre de maintenance validée avec les équipes infrastructure.

## Nmap et les obligations de la directive NIS2

Depuis l’entrée en application de la **directive NIS2** pour les entités essentielles et importantes en France et dans l’Union européenne, la cartographie régulière du parc réseau n’est plus seulement une bonne pratique : c’est une composante attendue de la gestion des risques cyber documentée dans le référentiel de conformité. Un audit Nmap planifié, avec ses rapports XML horodatés et archivés, constitue une preuve tangible de la démarche de gouvernance exigée par le texte, que nous détaillons dans notre guide complet sur la mise en conformité NIS2.

Concrètement, un scan Nmap ne suffit jamais à lui seul à démontrer une conformité NIS2 complète : il doit s’insérer dans un cycle plus large associant gestion des vulnérabilités, plan de réponse à incident et formation des équipes. Mais il reste souvent le point de départ le plus simple à mettre en place pour un service informatique qui découvre ses obligations : avant de documenter des risques, encore faut-il savoir précisément quelles machines et quels services composent le système d’information.

## Étape 10 : exporter et exploiter les résultats de scan

Un scan ponctuel dans un terminal n’a de valeur que sur l’instant. Pour construire un historique ou alimenter un autre outil, Nmap propose plusieurs formats de sortie :

```
# Format normal, lisible
nmap -oN scan_resultat.txt 192.168.1.0/24
# Format XML, exploitable par des scripts et des SIEM
nmap -oX scan_resultat.xml 192.168.1.0/24
# Format "greppable", pratique avec grep/awk
nmap -oG scan_resultat.gnmap 192.168.1.0/24
# Les trois formats en une seule commande
nmap -oA audit_complet 192.168.1.0/24
```
Le format XML (`-oX`) est celui que consomment la majorité des intégrations tierces, y compris la bibliothèque Python `python-nmap` utilisée dans les pipelines SIEM. C’est celui que nous utiliserons dans le projet complet ci-dessous.

## Étape 11 : intégrer Nmap à un SIEM, l’exemple Wazuh

En 2026, la tendance forte n’est plus le scan manuel isolé mais l’intégration de Nmap dans un pipeline de surveillance continue. **Wazuh**, la plateforme SIEM/XDR open source déjà couverte dans notre tutoriel Wazuh, illustre bien ce pattern grâce à sa fonctionnalité de *command monitoring*. On ajoute dans le fichier `ossec.conf` de l’agent Wazuh un bloc qui exécute un script Nmap à intervalle régulier :

```
<localfile>
  <log_format>full_command</log_format>
  <command>python3 /home/audit/nmap_scan.py</command>
  <frequency>604800</frequency> <!-- hebdomadaire -->
</localfile>
```
Le script Python, généralement bâti sur la bibliothèque `python-nmap`, exécute le scan, convertit les résultats en JSON et les ajoute au fichier `/var/ossec/logs/active-responses.log`. Wazuh ingère ensuite ce flux et applique des règles personnalisées pour générer des alertes dès qu’un nouveau port apparaît sur un hôte surveillé. C’est exactement le pattern documenté par l’équipe Wazuh dans son article sur l’audit de sécurité automatisé, qui combine scan Nmap planifié et enrichissement des résultats. À l’inverse, Wazuh peut aussi **détecter des scans Nmap hostiles** dirigés contre vos propres machines, via une règle qui repère les signatures caractéristiques dans les journaux réseau :

```
<rule id="100100" level="10">
  <if_sid>576</if_sid>
  <match>Nmap|Ncat|Nping</match>
  <description>Outil de scan de ports détecté</description>
</rule>
```
Ce double usage, sonde active d’un côté et signal de détection de l’autre, résume bien pourquoi Nmap reste central dans une stratégie de sécurité réseau en 2026 : le même outil sert autant à l’attaque simulée qu’à la défense. Plus de détails dans notre article dédié à l’intégration Nmap et Wazuh.

## Étape 12 : projet complet, un script d’audit réseau automatisé avec alerte

Voici un projet fonctionnel de bout en bout, à adapter à votre propre réseau domestique ou labo de test. L’objectif : scanner un sous-réseau chaque nuit, comparer le résultat avec le scan précédent, et alerter par e-mail si un nouveau port ouvert apparaît, un signe classique de compromission ou de mauvaise configuration.

Créez d’abord le script Python `audit_reseau.py` :

```
#!/usr/bin/env python3
import nmap
import json
import smtplib
from email.mime.text import MIMEText
from pathlib import Path
RESEAU_CIBLE = "192.168.1.0/24"
FICHIER_HISTORIQUE = Path("/var/log/audit_nmap/dernier_scan.json")
EXPEDITEUR = "[email protected]"
DESTINATAIRE = "[email protected]"
def lancer_scan():
    scanner = nmap.PortScanner()
    scanner.scan(hosts=RESEAU_CIBLE, arguments="-sS -sV -T4 --top-ports 200")
    resultat = {}
    for hote in scanner.all_hosts():
        ports_ouverts = []
        if "tcp" in scanner[hote]:
            for port, info in scanner[hote]["tcp"].items():
                if info["state"] == "open":
                    ports_ouverts.append({"port": port, "service": info.get("name", "?")})
        resultat[hote] = ports_ouverts
    return resultat
def comparer_et_alerter(nouveau_scan):
    ancien_scan = {}
    if FICHIER_HISTORIQUE.exists():
        ancien_scan = json.loads(FICHIER_HISTORIQUE.read_text())
    nouveaux_ports = []
    for hote, ports in nouveau_scan.items():
        anciens_ports = {p["port"] for p in ancien_scan.get(hote, [])}
        for p in ports:
            if p["port"] not in anciens_ports:
                nouveaux_ports.append(f"{hote}:{p['port']} ({p['service']})")
    if nouveaux_ports:
        envoyer_alerte(nouveaux_ports)
    FICHIER_HISTORIQUE.parent.mkdir(parents=True, exist_ok=True)
    FICHIER_HISTORIQUE.write_text(json.dumps(nouveau_scan, indent=2))
def envoyer_alerte(nouveaux_ports):
    contenu = "Nouveaux ports détectés depuis le dernier audit :\n\n" + "\n".join(nouveaux_ports)
    message = MIMEText(contenu)
    message["Subject"] = "[Audit Nmap] Nouveaux ports ouverts détectés"
    message["From"] = EXPEDITEUR
    message["To"] = DESTINATAIRE
    with smtplib.SMTP("localhost") as serveur:
        serveur.send_message(message)
if __name__ == "__main__":
    scan_actuel = lancer_scan()
    comparer_et_alerter(scan_actuel)
```
Installez la dépendance et testez le script manuellement avant de l’automatiser :

```
pip install python-nmap
sudo python3 audit_reseau.py
```
Une fois validé, planifiez son exécution nocturne via cron :

```
sudo crontab -e
# Ajouter la ligne suivante pour un scan chaque nuit à 2h15
15 2 * * * /usr/bin/python3 /home/audit/audit_reseau.py
```
Vous disposez maintenant d’un projet complet et fonctionnel : un scan différentiel automatisé qui vous alerte uniquement sur les changements, plutôt que de vous noyer sous un rapport complet chaque matin. C’est exactement la logique qu’appliquent les intégrations Wazuh-Nmap en production, à plus grande échelle.

## Bonnes pratiques après un scan

Un scan Nmap n’a de valeur que si ses résultats déclenchent une action. Une fois le rapport en main, fermez systématiquement les ports non justifiés par un besoin métier documenté, mettez à jour les services dont la version détectée correspond à une CVE connue, et si vous avez utilisé `--script vuln`, traitez en priorité tout résultat marqué `VULNERABLE (Exploitable)`. Conservez vos rapports XML dans un dossier daté : c’est votre seule preuve de conformité en cas d’audit NIS2, un sujet que nous détaillons dans notre guide sur la directive NIS2.

Il est également recommandé de documenter chaque campagne de scan dans un registre interne : date, périmètre exact, personne à l’origine du scan et référence de l’autorisation écrite. Ce registre sert deux objectifs distincts. D’abord, il évite les confusions internes lorsqu’une équipe SOC détecte un scan Nmap dans ses journaux et doit déterminer en quelques minutes s’il s’agit d’un audit légitime ou d’une activité suspecte à traiter en urgence. Ensuite, il constitue une pièce de dossier utile en cas de contrôle réglementaire, notamment dans les secteurs soumis à NIS2 ou à des exigences sectorielles comme celles de la santé ou de la finance.

## Nmap face aux autres scanners de réseau

Nmap n’est pas le seul outil de reconnaissance réseau disponible, même s’il reste le plus polyvalent. Le choix entre ces outils dépend rarement d’une préférence technique isolée : il dépend surtout du livrable attendu. Un rapport d’audit destiné à un comité de direction ou à un organisme de certification a besoin d’une sortie structurée avec scoring de risque, ce que fournissent nativement des scanners de vulnérabilités comme OpenVAS. Un diagnostic rapide en pleine intervention sur incident, en revanche, tolère mal la lourdeur de ces plateformes et privilégie la vitesse d’exécution de Nmap. Voici comment il se positionne face à trois alternatives fréquemment citées en 2026.

| Outil | Type | Point fort | Limite | 
|---|---|---|---|
| Nmap | Scanner de ports + NSE | Gratuit, scriptable, référence du secteur | Ligne de commande, courbe d’apprentissage réelle | 
| Zenmap | Interface graphique pour Nmap | Visualisation topologique, profils prédéfinis | Ne couvre pas toutes les options CLI avancées | 
| OpenVAS / Greenbone | Scanner de vulnérabilités | Base CVE intégrée, rapports de conformité | Plus lourd à déployer, orienté audit planifié | 
| Angry IP Scanner | Scanner d’hôtes basique | Ultra simple, multiplateforme | Pas de détection de service ni de scripting | 

En pratique, beaucoup d’équipes sécurité combinent les deux approches : Nmap pour la découverte rapide et l’automatisation légère, Trivy pour le scan de conteneurs et un scanner de vulnérabilités dédié comme OpenVAS pour les audits de conformité approfondis nécessitant un rapport formel.

## 5 pièges fréquents à éviter avec Nmap

La majorité des incidents liés à Nmap ne viennent pas d’une mauvaise intention, mais d’une commande mal calibrée lancée sans réfléchir à son impact réseau. Voici les erreurs qui reviennent le plus souvent, aussi bien chez les débutants que chez des administrateurs expérimentés pressés par le temps.

- **Lancer un scan SYN sans sudo.** Nmap bascule silencieusement sur un scan Connect (-sT) moins discret et plus lent, sans toujours prévenir clairement l’utilisateur débutant.
- **Scanner une plage IP trop large sans autorisation.** Un`/16` lancé par erreur au lieu d’un`/24` peut toucher des milliers de machines hors périmètre autorisé.
- **Confondre port filtré et port fermé.** Un port « filtered » signifie qu’un pare-feu bloque la réponse, pas que le service est absent : le traiter comme fermé fausse tout l’audit.
- **Oublier -Pn derrière un pare-feu strict.** Si les échos ICMP sont bloqués, Nmap déclare l’hôte injoignable alors qu’il répond parfaitement sur ses ports TCP.
- **Lancer –script vuln en pleine production sans fenêtre de maintenance.** Certains scripts intrusifs peuvent perturber des services fragiles ou déclencher des blocages automatiques côté pare-feu applicatif.

## Dépannage : 8 problèmes courants et leurs solutions

Même avec une installation propre, Nmap produit régulièrement des résultats surprenants pour qui découvre l’outil. La plupart de ces situations s’expliquent par le comportement des pare-feux modernes ou par une mauvaise combinaison d’options, et se résolvent en une seule commande une fois le diagnostic posé.

- **« nmap: command not found »** après installation : la variable PATH n’a pas été rechargée. Fermez et rouvrez le terminal, ou lancez`hash -r` sous bash.
- **« You requested a scan type which requires root privileges »** : ajoutez`sudo` devant la commande sous Linux/macOS, ou lancez le terminal en administrateur sous Windows.
- **Le scan reste bloqué indéfiniment sur une IP** : ajoutez`--host-timeout 5m` pour forcer un abandon après cinq minutes et passer à l’hôte suivant.
- **Tous les ports apparaissent « filtered »** : un pare-feu intermédiaire bloque probablement les réponses RST. Essayez un scan ACK (`-sA` ) pour distinguer les règles de filtrage.
- **L’hôte semble éteint alors qu’il répond via navigateur** : le ping ICMP est bloqué. Utilisez`-Pn` pour forcer le scan sans phase de découverte.
- **Un script NSE dépasse le temps imparti** : ajoutez`--script-timeout 30s` pour limiter la durée de chaque script individuellement.
- **Le scan est anormalement lent sur un grand réseau** : augmentez le parallélisme avec`--min-parallelism 50` ou passez à un template de timing plus agressif (`-T4` ).
- **Détection de version incorrecte ou incomplète** : montez l’intensité des sondes avec`--version-intensity 9` , la valeur par défaut (7) n’interroge pas toujours les services les plus rares.

## Astuces avancées pour aller plus loin

Une fois les bases maîtrisées, plusieurs options méritent d’être explorées pour professionnaliser vos audits. La combinaison `-sV --version-intensity 9 --script default,vuln` donne un bon compromis entre exhaustivité et temps d’exécution pour un audit hebdomadaire planifié. Pensez aussi à exclure systématiquement les hôtes critiques d’un scan agressif avec `--exclude 192.168.1.1,192.168.1.254` pour éviter de perturber une passerelle ou un contrôleur industriel fragile. Pour les environnements IPv6, ajoutez simplement `-6` à votre commande : la couverture IPv6 de Nmap s’est nettement améliorée depuis l’introduction du résolveur DNS parallèle en version 7.96, qui a permis de résoudre un million de noms d’hôtes en environ une heure contre 49 heures auparavant selon le changelog officiel du projet.

Enfin, pour les équipes qui gèrent plusieurs segments réseau, envisagez de déployer un scanner Nmap par segment (via des agents Wazuh distincts par exemple), plutôt qu’un scan centralisé unique qui traverse des pare-feux internes et fausse les résultats de latence et de filtrage. Pour la documentation complète des options, référez-vous au manuel officiel de Nmap.

Dernier conseil, souvent négligé : versionnez vos scripts d’audit Nmap dans un dépôt Git privé, au même titre que du code de production. Un script d’automatisation qui scanne l’ensemble d’un système d’information mérite une revue de code, un historique de modifications et une procédure de restauration en cas d’erreur de configuration, exactement comme n’importe quel outil interne critique.

## FAQ : vos questions sur Nmap

**Nmap est-il gratuit ?**

Oui, Nmap est un logiciel open source distribué sous une licence propre inspirée de la GPL, entièrement gratuit pour un usage personnel comme professionnel. Le code source est disponible sur nmap.org.

**Puis-je utiliser Nmap sans les droits administrateur ?**

Oui pour un scan Connect (-sT) ou un scan de découverte simple, mais les scans SYN (-sS) et la détection d’OS (-O) nécessitent des privilèges élevés pour manipuler les paquets réseau bruts.

**Quelle est la différence entre Nmap et Zenmap ?**

Zenmap est simplement l’interface graphique officielle de Nmap. Il exécute les mêmes commandes en arrière-plan mais facilite la visualisation des résultats et la sauvegarde de profils de scan.

**Scanner ma propre box internet est-il légal ?**

Oui, scanner votre propre réseau domestique ou une infrastructure que vous possédez ne pose aucun problème légal en France. La question se pose uniquement pour des systèmes appartenant à des tiers.

**Nmap peut-il détecter tous les appareils IoT sur mon réseau ?**

Dans la majorité des cas oui, via le ping scan (-sn) et la détection de service, mais certains objets connectés à très faible consommation ne répondent qu’à des sondes spécifiques et peuvent nécessiter des scripts NSE dédiés.

**Combien de temps prend un scan complet d’un réseau /24 ?**

Avec un template -T4 et les ports par défaut, comptez entre 2 et 10 minutes selon le nombre d’hôtes actifs. Un scan complet des 65 535 ports par hôte peut prendre plusieurs heures.

**Nmap peut-il être détecté par un pare-feu ou un antivirus ?**

Oui, la plupart des solutions de sécurité réseau modernes, dont Wazuh, Suricata ou les pare-feux nouvelle génération, détectent les signatures de scan Nmap et peuvent déclencher une alerte ou un blocage automatique de l’adresse IP source.

**Faut-il préférer Nmap ou un scanner de vulnérabilités comme OpenVAS ?**

Ce ne sont pas des concurrents directs. Nmap excelle en découverte et cartographie réseau rapide, tandis qu’OpenVAS ou Greenbone conviennent mieux à un audit de conformité formel avec base CVE intégrée. Beaucoup d’équipes utilisent les deux, l’un après l’autre.

**Nmap fonctionne-t-il sur un réseau IPv6 ?**

Oui, il suffit d’ajouter l’option `-6` à votre commande. La prise en charge d’IPv6 s’est nettement améliorée ces dernières versions, notamment grâce au nouveau résolveur DNS parallèle introduit en 7.96, mais certaines techniques de scan (comme la détection d’OS) restent moins fiables qu’en IPv4.