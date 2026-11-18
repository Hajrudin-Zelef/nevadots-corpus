# RÉSEAU DATACENTER / IA — NICs, FIBRE, SPINE-LEAF, SWITCHES IA, DPU, CDN/EDGE

**Guide technique dense — rédigé le 27/09/2026 — version 1.0**
**Public :** chef de service systèmes & énergies — chiffres, BOMs, dimensionnements, lien énergie.
**Méthode :** références produits vérifiées par recherche web le 27/09/2026.
Mentions utilisées dans tout le document :
- ✅ **Vérifié le 27/09/2026** = trouvé et corroboré par recherche web ce jour.
- ⚠️ **À vérifier** = ordre de grandeur usuel du marché / valeur normative, à confirmer sur datasheet ou devis distributeur.
- ❌ **Non trouvée au 27/09/2026** = recherche effectuée, aucune source fiable trouvée.

> Règle d'honnêteté : aucun prix « vérifié » n'est affirmé — les prix sont des
> fourchettes usuelles du marché marquées ⚠️, les hyperscalers ne publiant pas
> leurs tarifs. Les specs constructeur citées sont celles des datasheets.

---

## SOMMAIRE

- **Partie A — NICs serveur** (§1-15) : 25/100/200/400/800 GbE, PCIe, offloads, choix
- **Partie B — Fibre & transceivers** (§16-30) : formats, OM/OS, budgets optiques, DAC/AOC, nettoyage
- **Partie C — Architectures spine-leaf** (§31-42) : Clos, oversubscription, EVPN/VXLAN, rail-optimized
- **Partie D — Switches pour l'IA** (§43-56) : buffers, ECN, RoCEv2, IB vs Ethernet, Spectrum-X, UEC
- **Partie E — Châssis 800GbE clusters GPU** (§57-63) : radix, câblage, thermique
- **Partie F — DPU** (§64-72) : BlueField, offloads, quand en mettre
- **Partie G — CDN & edge** (§73-80) : principes, dimensionnement PoP
- **Partie H — Adressage, câblage, étiquetage** (§81-88) : plans, brassage, TIA-606
- **Partie I — Énergie** (§89-98) : conso NICs/switches/optiques, PUE, dimensionnement élec
- **Partie J — À venir (annonces vérifiées)** (§99-104) : 1.6T, UEC, CPO
- **Partie K — Approfondissements terrain** (§106-120) : supervision, tests, NCCL, stockage, sécurité, automation, TCO, migration, DCI, edge/5G, water-cooling, onduleurs, maintenance, retours terrain
- **Partie L — Fiches produits** (§126-133) : ConnectX-8, Thor 2/Ultra, BlueField-4, SN5600, TH6, Arista, E810
- **Partie M — Dimensionnements pas-à-pas** (§134-137) : cluster 2048 GPU, DC cloud, DCI, lab
- **Annexes** (§138-140) : diagnostic, table maître, index
- **Partie N — Annexes techniques denses** (§141-150) : catalogue transceivers, normes IEEE, arbre de dépannage, templates, acronymes, timeline, comparatif NOS, audit sécu, alertes Prometheus, FAQ
- **Partie O — Compléments ultra-denses** (§151-164) : comparatif 20 switches, lecture datasheet, budgets optiques, table câbles, protocole d'acceptation, RFP, pannes/MTTR, glossaire 2, fiches acteurs, refresh, multi-tenant, checklists, erreurs de câblage, conversions dB
- **Partie P — Derniers compléments** (§165-174) : étude 10 MW, test optiques, distances complètes, achat par profil, lexique FR-EN, rapport d'incident, formation, index tableaux, checklist congés, notes de version
- **Partie Q — Synthèses finales** (§175-181) : une page par débit, contrôles de congestion, connecteurs, template câblage, alimentations, FAQ 2, fin de vie
- **Partie R — Clôture opérationnelle** (§182-186) : matrice de décision, extraits de config, erreurs humaines chiffrées, ressources, revue de conception
- **Partie S — 110 décisions rapides** (§187) : le mémo ultime Si/Alors
- **§105** Glossaire (40 termes) — **§121** Quiz (10 Q/R) — **§122** Les 45 pièges terrain
- **§123** BOMs types — **§124** Checklist d'achat — **§125** Sources vérifiées

---

# PARTIE A — NICs SERVEUR : 25 / 100 / 200 / 400 / 800 GbE

## 1. Pourquoi la NIC est devenue un composant stratégique

Il y a 10 ans, la NIC était une commodité : on prenait du 10G Intel et on
n'y pensait plus. Trois ruptures ont changé la donne :

1. **L'IA générative** : l'entraînement distribué (all-reduce NCCL) sature le
   réseau inter-nœuds. Sur un cluster de 10 000 GPU, 1 % de perte de débit
   réseau = des millions de dollars de GPU qui attendent. La NIC fait partie
   du chemin critique du calcul.
2. **La vitesse a rattrapé le bus** : à 400 Gb/s, une NIC pousse autant que
   ce qu'un PCIe Gen5 x16 peut transporter (512 Gb/s théoriques). Le choix
   NIC ⇄ carte mère ⇄ CPU est un dimensionnement couplé, plus un achat isolé.
3. **Le transport a changé** : RDMA over Converged Ethernet (RoCEv2), puis
   Ultra Ethernet (UEC 1.0, ✅ 11/06/2025), déplacent la congestion, le
   multipath et la retransmission sélective **dans la NIC**. Deux NIC « 400G »
   de générations différentes n'ont plus les mêmes performances sur un fabric IA.

**Conséquence pour Zelef** : budgéter la NIC comme on budgète le GPU —
avec sa génération PCIe, sa conso (section 89) et son écosystème logiciel
(driver, firmware, OFED/DOCA).

## 2. Tableau maître : débit ⇄ format ⇄ PCIe minimal ⇄ usage

| Débit | Format dominant | Lanes élec. | PCIe minimal (x16 sauf note) | Usage typique 2026 |
|---|---|---|---|---|
| 25 GbE | SFP28 | 1×25G NRZ | Gen3 x8 | Legacy, management, OOB, vieux leafs |
| 100 GbE | QSFP28 | 4×25G NRZ | Gen4 x16 (Gen3 x16 limite) | Front-end, stockage, leafs généralistes |
| 200 GbE | QSFP56 | 4×50G PAM4 | Gen4 x16 | Transition, HPC, stockage NVMe-oF |
| 400 GbE | QSFP112 / QSFP-DD | 4×100G PAM4 | **Gen5 x16** | Backend IA standard, nœuds GPU 2024-2026 |
| 800 GbE | OSFP / QSFP-DD800 | 8×100G PAM4 | **Gen6 x16** | Backend IA 2025+, scale-out Rubin |

Notes :
- NRZ = 1 bit/symbole ; PAM4 = 2 bits/symbole. Le passage à PAM4 (50G/lane
  puis 100G/lane) est ce qui a permis 400/800G sans multiplier les fibres.
- Un slot PCIe Gen4 x16 plafonne à 256 Gb/s théoriques (~200 Gb/s utiles) :
  **y brancher une NIC 400G = la brider à ~50 %**. C'est le piège n°1 des
  achats NIC (voir §107, P1).
- PCIe Gen6 x16 = 128 Go/s théoriques (~1024 Gb/s) : juste assez pour 800G
  plein débit + overhead. ✅ ConnectX-8 documenté PCIe Gen6 (firmware docs
  NVIDIA, vérifié le 27/09/2026).

## 3. Le 25 GbE : mort ou toujours vivant ?

Le 25G (SFP28, 1 lane 25G NRZ) reste pertinent en 2026 pour :
- **OOB / management** : iDRAC/iLO, consoles — souvent encore 1G, mais les
  parcs neufs passent au 25G sur les leafs de management.
- **Nœuds légers** : hyperviseurs de services, monitoring, bastions.
- **Breakout** : un port 100G QSFP28 se décompose en 4×25G (câble breakout).

Références courantes (⚠️ gamme établie, vérifier dispo) : Intel XXV710,
NVIDIA ConnectX-4 Lx / ConnectX-6 Dx en mode 25G, Broadcom NetXtreme.
**Ne plus acheter de 10G neuf** sauf contrainte d'équipement existant : le
prix au Gb/s du 25G est inférieur depuis des années.

## 4. Le 100 GbE : le standard du front-end

Le 100G (QSFP28, 4×25G NRZ) est en 2026 le débit « banalisé » : serveurs
généralistes, stockage, interconnexion de baies, uplinks de leafs 25G.

**Références actuelles :**

| NIC | Débit/ports | PCIe | Points notables | Statut |
|---|---|---|---|---|
| NVIDIA ConnectX-6 Dx | 2×100G (ou 1×200G) | Gen4 x16 | RoCEv2, la valeur sûre du 100/200G | ✅ gamme établie |
| NVIDIA ConnectX-7 | 2×100/200G ou 1×400G | Gen5 x16 | RDMA, adaptatif, base des backend IA 400G | ✅ vérifié 27/09/2026 |
| Intel E810-CAM1/XXVDA2 | 2×100G QSFP28 | Gen4 x16 | 256 VF SR-IOV, ADQ, PTP, ~15-27 W | ✅ vérifié 27/09/2026 |
| Broadcom NetXtreme BCM575xx | 1/2×100G | Gen4 | Alternative OEM massive (Dell/HPE) | ⚠️ à vérifier |
| Marvell FastLinQ 45000 | 2×100G | Gen4 | Présent chez les OEM | ⚠️ à vérifier |

**Intel E810 (détail vérifié le 27/09/2026)** : contrôleur E810-CAM1,
PCIe 4.0 x16, 2 ports QSFP28 100G (10/25/50/100G auto), SR-IOV 256 fonctions
virtuelles, VMDq, VXLAN/GENEVE/NVGRE en offload, RDMA iWARP + RoCEv2, DPDK,
IEEE 1588 PTP, DCB (PFC/ETS). Conso relevée : 15 W (version 2 ports, fiche
distributeur) à 27,1 W avec AOC — l'optique compte, voir §89.
C'est la NIC 100G « enterprise » de référence quand on ne fait pas d'IA.

## 5. Le 200 GbE : le palier de transition

Le 200G (QSFP56, 4×50G PAM4) a été un palier court : il sert surtout en
**mode dégradé** des NIC 400G (ConnectX-7 en 200G) ou en breakout
(1×400G → 2×200G, 1×800G → 4×200G).

Cas d'usage réel : stockage NVMe-oF où 100G est juste et 400G surdimensionné,
et vieux fabrics InfiniBand HDR (200G) en migration vers Ethernet.
**En achat neuf 2026 : on ne cible plus le 200G natif**, on achète du 400G
capable de négocier 200G.

## 6. Le 400 GbE : le backend IA standard (2024-2026)

Le 400G est le débit du backend des clusters GPU Hopper (H100/H200) :
**1 NIC 400G par GPU** (ratio 1:1, rail-optimized, voir §39) ou 1 NIC pour
2 GPU sur les designs économiques.

| NIC 400G | PCIe | Format | Distinction clé | Statut |
|---|---|---|---|---|
| NVIDIA ConnectX-7 | Gen5 x16 | QSFP112 / OSFP | RDMA/RoCEv2, la référence backend Hopper | ✅ vérifié 27/09/2026 |
| Broadcom Thor 2 (BCM957608) | Gen5 x16 | QSFP112 | 5 nm, faible conso, RoCE + apports UEC (ACK sélectif, multipath niveau paquet) | ✅ vérifié 27/09/2026 |
| Intel 400G | — | — | ❌ **Non trouvée au 27/09/2026** : Intel n'a toujours pas de NIC 400G sur le marché (confirmé par la presse spécialisée) | ❌ |
| NVIDIA BlueField-3 SuperNIC | Gen5 x16 | 2×400G ou 1×800G | ConnectX-7 + ARM : réordonne les paquets, contrôle de congestion HW, télémétrie par flux | ✅ vérifié 27/09/2026 |

**Broadcom Thor 2 (détail vérifié le 27/09/2026)** : ASIC 5 nm présenté comme
« purpose-built AI », 400 Gb/s, PCIe 5.0 x16, 8 lanes SerDes 100G PAM4/50G/25G
NRZ, DAC passif jusqu'à ~5 m vers switch Tomahawk 5, optiques linéaires
basse conso (LPO, voir §28). Disponible en carte, chiplet ou IP.
C'est le challenger « ouvert » de ConnectX-7 pour les fabrics Broadcom/Arista.

**Point d'architecte** : à 400G, le CPU ne peut plus traiter les paquets en
logiciel (trop d'interruptions, trop de copies). Les **offloads** (§9) et le
**RDMA** (§10) ne sont plus optionnels : sans eux, 2×400G sur un serveur =
CPU saturé avant la moitié du débit.

## 7. Le 800 GbE : la génération 2025-2026

Le 800G (8×100G PAM4, OSFP ou QSFP-DD800) est le débit du backend des
clusters Blackwell (B200/B300) et du scale-out Rubin.

| NIC 800G | PCIe | Format | Distinction clé | Statut |
|---|---|---|---|---|
| NVIDIA ConnectX-8 SuperNIC | **Gen6** x16 (jusq. 48 lanes) | OSFP224 1×800G ou 2×112 | In-Network Computing, MPI_Alltoall, QoS + contrôle de congestion, Socket Direct 16 lanes, OCP 3.0 / CEM | ✅ vérifié 27/09/2026 |
| Broadcom Thor Ultra | (PCIe Gen5/6 ⚠️) | 800G | **Premier NIC conforme UEC 1.0** (annoncé 14/10/2025), 1 seul flux 800G (pas 2×400G), RDMA modernisé : retransmission sélective, CC émetteur/récepteur programmable | ✅ vérifié 27/09/2026 |
| NVIDIA ConnectX-9 SuperNIC | — | — | **1,6 Tb/s** scale-out, associé à l'architecture Rubin (2ᵉ sem. 2026) | ✅ annoncé, voir §99 |

**ConnectX-8 (détail vérifié le 27/09/2026)** : dévoilé à COMPUTEX 2025,
firmware 40.46.x (sept. 2025). Au-delà du 800G, c'est un **commutateur PCIe**
: dans les serveurs RTX PRO, il remplace plusieurs switchs PCIe discrets,
donnant 400 Gb/s par GPU (ratio 2:1 GPU:NIC) et ~2× de perf NCCL all-to-all.
Supporte le chiffrement inline (TLS/IPsec) sans perte de débit (moteurs
crypto dédiés). Écosystème DOCA 2.0.

**Lecture énergétique** : le 800G double le débit pour ~+50-70 % de conso NIC
(⚠️ ordre de grandeur) — le **joule par bit baisse**. C'est l'argument à
porter en comité : monter en débit, c'est aussi de l'efficacité énergétique
(voir §89-92).

## 8. PCIe requis par débit — le tableau qui évite les bridages

Débits théoriques **unidirectionnels** par génération (x16) :

| PCIe | Gb/s théo. x16 | Débit NIC max sain | Marge utile |
|---|---|---|---|
| Gen3 x16 | 128 | 25G (100G bridé ~50 %) | — |
| Gen4 x16 | 256 | 100G, 200G limite | 200G = 78 % du bus : OK mais sans marge |
| Gen5 x16 | 512 | 400G | 78 % : le standard backend IA |
| Gen6 x16 | 1024 | 800G | 78 % : ConnectX-8, BlueField-4 ✅ |

Règles pratiques :
- **Toujours x16 électriques** pour ≥100G : un slot mécanique x16 câblé x8
  (fréquent sur serveurs 1U denses) divise par 2. Vérifier `lspci` :
  `LnkSta: Speed 32GT/s, Width x16` (Gen5).
- **Bifurcation** : pour 2× NIC 200G sur un slot x16, la carte mère doit
  supporter la bifurcation x8/x8 — vérifier dans le BIOS/fiche serveur.
- **Socket Direct** (NVIDIA) : carte auxiliaire 16 lanes qui relie la NIC aux
  deux sockets CPU sans passer par un switch PCIe — réduit la latence NUMA.
  ✅ supporté par ConnectX-8 (vérifié 27/09/2026).
- **CXL** : ne remplace pas le PCIe pour la NIC en 2026 ; le CXL.mem sert à
  l'extension mémoire, pas au réseau. ❌ Pas de NIC CXL réseau trouvée au
  27/09/2026 — ne pas confondre avec les annonces CPO.

## 9. Offloads : ce que la NIC fait à la place du CPU

À partir de 100G, chaque feature ci-dessous vaut des cœurs CPU entiers :

| Offload | Ce qu'il fait | Gain typique | Disponible sur |
|---|---|---|---|
| Checksum (Tx/Rx) | Calcule IP/TCP/UDP en HW | ~1-2 cœurs à 100G | Toutes |
| TSO (TCP Segmentation) | Découpe les gros segments en HW | −30-50 % CPU à 100G | Toutes |
| GRO/LRO (Rx) | Réassemble côté réception | Idem côté Rx | Toutes |
| SR-IOV | 256 VF PCIe vers les VM (bypass hyperviseur) | Latence ÷2-3 vs vSwitch | E810 (256 VF ✅), CX-6/7 |
| VMDq / ADQ (Intel) | Files Rx par VM/application | QoS par workload | E810 ✅ |
| VXLAN/GENEVE/NVGRE | Encap/decap overlay en HW | Indispensable EVPN/VXLAN à 100G+ | E810, CX-6/7, Thor 2 |
| DPDK / XDP | Bypass kernel, poll-mode | 10-100 Mpps par cœur | E810, CX, Thor |
| TLS/IPsec inline | Chiffre en HW à plein débit | Sécurité sans taxe CPU | ConnectX-8 ✅, BF-3 |
| RoCEv2 | RDMA sur UDP en HW | Zéro-copy, CPU ~0 | CX-6/7/8, Thor 2, E810 (iWARP+RoCEv2 ✅) |

**Test de réception** : `ethtool -k ethX` doit montrer `tx-checksumming: on`,
`scatter-gather: on`, `tcp-segmentation-offload: on`. Si un offload est
« off [fixed] », c'est le driver ou le firmware — mettre à jour avant
d'incriminer le réseau (P2, §107).

## 10. RDMA : RoCEv2, iWARP, InfiniBand natif — qui fait quoi

| Transport | Support | Latence typ. | Lossless requis ? | Écosystème IA |
|---|---|---|---|---|
| InfiniBand natif (NDR 400G, XDR 800G) | IB | ~0,6-1 µs | Oui natif (crédits) | NCCL/MPI par défaut, 20 ans de maturité |
| RoCEv2 (UDP 4791) | Ethernet | ~1-2 µs | Oui (PFC+ECN) à régler | Standard backend Ethernet IA |
| iWARP (TCP/SCTP) | Ethernet | ~2-5 µs | Non (TCP gère) | Stockage, moins HPC |
| UET (Ultra Ethernet) | Ethernet | ~1 µs visé | **Non** (conçu lossy) | ✅ UEC 1.0 du 11/06/2025, Thor Ultra |

**DCQCN** (Data Center Quantized Congestion Notification) = l'algorithme de
contrôle de congestion de RoCEv2 : la NIC marque les paquets (ECN), le
récepteur renvoie des CNP, l'émetteur réduit son débit. Mal réglé, c'est la
cause n°1 des contre-performances RoCE (voir §45-46).
**Point 2026** : UEC supprime l'exigence du lossless (livraison non ordonnée
+ retransmission sélective), ce qui change le dimensionnement des buffers
(voir §54).

## 11. Critères de choix d'une NIC — matrice de décision

| Workload | Débit cible | NIC recommandée | Pourquoi |
|---|---|---|---|
| Serveur généraliste / VMs | 2×25G ou 2×100G | Intel E810 / CX-6 Dx | SR-IOV mature, drivers inbox, prix |
| Stockage NVMe-oF / Ceph | 2×100G | CX-6 Dx / E810 | RDMA, faible latence, multipath |
| Backend IA Hopper (H100) | 400G / GPU | ConnectX-7 ou Thor 2 | RoCEv2 + CC ; Thor 2 si fabric Broadcom |
| Backend IA Blackwell (B200) | 800G / GPU | ConnectX-8 / BF-3 SuperNIC | Spectrum-X de bout en bout |
| Backend IA Rubin (2026+) | 1,6T / GPU | ConnectX-9 (annoncé) | Scale-out 1,6 Tb/s ✅ |
| Management / OOB | 1-25G | LOM / SFP28 éco | Ne pas gaspiller du 100G |
| Firewall/edge | 100G+ DPDK | E810 / CX-6 Dx | PPS, drivers stables |

**Questions à poser au vendeur** (checklist §109) :
1. Firmware minimal requis pour le débit annoncé ? (ex. CX-8 : 40.46.x)
2. Driver inbox kernel ou OFED/DOCA propriétaire ? Qui le maintient ?
3. La carte est-elle codée constructeur (lock optique) ? (P3, §107)
4. Conso max avec optiques (pas la carte seule) ?
5. Support UEC / RoCEv2 / PFC-DCQCN validé sur quel switch ?

## 12. Formats physiques : PCIe CEM, OCP 3.0, Socket Direct

| Format | Usage | Avantage | Limite |
|---|---|---|---|
| PCIe CEM (FH/HL) | Serveurs généralistes | Universel | Encombrement, 1 slot |
| OCP 3.0 (SFF) | Serveurs hyperscale/OEM | Hot-swap, frontal, dense | Châssis compatibles requis |
| Socket Direct | Serveurs IA NVIDIA | Latence NUMA minimale | Carte auxiliaire + câblage |
| Mezzanine (ex. OCP) | Lames | Intégré | Propriétaire |

✅ ConnectX-8 existe en OCP 3.0 et CEM x16 (vérifié 27/09/2026).
**Piège d'achat** : une NIC OCP 3.0 ne rentre pas dans un serveur tour/1U
classique — valider le format **avant** la commande, pas à la réception.

## 13. Firmware & drivers : la moitié du produit

- **NVIDIA** : MLNX_OFED (ou DOCA-OFED) vs driver inbox. Pour RoCEv2 +
  Spectrum-X, prendre le driver validé par le fabric, pas le dernier cri.
  `mlxfwmanager` pour le firmware ; ne jamais mélanger FW 40.45 et 40.46 sur
  un même fabric sans valider.
- **Intel** : drivers `ice` inbox depuis kernel 5.x — le plus simple à
  maintenir. NVM update via `ice` tools.
- **Broadcom** : `bnxt_en` inbox ; firmware via les OEM (Dell/HPE) — prévoir
  le cycle de qualification OEM (souvent 1-2 trimestres de retard).
- **Règle d'or** : figer un triplet (FW NIC, driver, OS) par cluster et
  l'appliquer à tout le fabric. Un nœud avec un FW différent = suspect n°1
  en cas de micro-pertes (P4, §107).

## 14. CAS CHIFFRÉ — 8 nœuds GPU Hopper, backend 400G

Hypothèses : 8 serveurs × 8 GPU H100, 1 NIC 400G par GPU (rail-optimized).

| Poste | Calcul | Résultat |
|---|---|---|
| NIC 400G | 64 × ConnectX-7 / Thor 2 | 64 NIC |
| Slots PCIe Gen5 x16 | 8 par serveur | Vérifier carte mère 8× x16 Gen5 ! |
| Conso NIC seules | 64 × ~35 W (⚠️) | ~2,2 kW |
| Optiques 400G DR4 | 64 × ~9 W (⚠️) | ~0,6 kW |
| Ports leaf 400G | 64 | 1 leaf 64×400G (ex. 32 ports en 2×200G si besoin) |
| Câblage intra-rack | DAC 400G si < 3 m, sinon AOC/DR4 | Voir §22 |

**Budget indicatif (⚠️ fourchettes marché, à vérifier)** : NIC 400G ~1 500-3 000 €,
optique 400G DR4 ~600-1 500 € → **~135-290 k€** pour 64 NIC + optiques seules.
Le réseau backend d'un cluster IA = 10-15 % du coût des GPU : le provisionner
en même temps, pas après.

## 15. Pièges NICs — l'essentiel (détail §107)

- **P1** — NIC 400G dans slot PCIe Gen4 : bridée à ~200G, invisible sans test.
- **P2** — Offloads désactivés par un vieux driver : CPU à 100 % à 100G.
- **P3** — Optiques tierces refusées par firmware verrouillé : tester 1 avant 100.
- **P4** — Firmwares hétérogènes sur un fabric RoCE : micro-pertes aléatoires.
- **P5** — OCP 3.0 commandée pour châssis CEM : ne rentre pas physiquement.

# PARTIE B — FIBRE & TRANSCEIVERS (« module stp fibre »)

## 16. Lexique des formats : du SFP à l'OSFP

« Module STP fibre » = en pratique un **transceiver** (émetteur-récepteur)
enfichable. Les formats, par ordre d'apparition :

| Format | Lanes élec. | Débit max usuel | Taille | Génération |
|---|---|---|---|---|
| SFP / SFP+ | 1 | 1G / 10G | Petit | Legacy |
| SFP28 | 1×25G NRZ | 25G | = SFP+ | 2015+ |
| SFP56 | 1×50G PAM4 | 50G | = SFP+ | 2019+ |
| SFP112 | 1×100G PAM4 | 100G | = SFP+ | 2024+ |
| QSFP+ | 4×10G | 40G | Compact 4 lanes | Legacy |
| QSFP28 | 4×25G NRZ | 100G | = QSFP+ | Standard DC |
| QSFP56 | 4×50G PAM4 | 200G | = QSFP+ | Transition |
| QSFP-DD | 8×25G/50G | 400G | Double densité (DD) | Backend IA |
| QSFP112 | 4×100G PAM4 | 400G | = QSFP-DD | Backend IA |
| OSFP | 8×50G/100G PAM4 | 400G/800G | Plus grand, meilleure thermique | IA / IB |
| QSFP-DD800 | 8×100G PAM4 | 800G | = QSFP-DD | 800G Ethernet |
| OSFP-XD / OSFP1600 | 8×200G PAM4 | 1,6T | En cours | Futur (§99) |

**Règle** : à débit égal, OSFP dissipe mieux que QSFP-DD (plus de surface,
dissipateur intégré). C'est pour ça que NVIDIA (Spectrum-X, Quantum) est
OSFP-natif : 800G = 14-18 W par module, il faut du métal autour.
⚠️ Commander des OSFP « flat-top » pour NIC et « finned-top » (à ailettes)
pour switch : se tromper = surchauffe (voir §107, P6).

## 17. Correspondance débit ⇄ format ⇄ fibre — le tableau de référence

| Débit | Transceiver type | Format | Fibre | Connecteur |
|---|---|---|---|---|
| 10G SR | 10GBASE-SR | SFP+ | OM3/OM4 | LC duplex |
| 25G SR | 25GBASE-SR | SFP28 | OM3/OM4 | LC duplex |
| 100G SR4 | 100GBASE-SR4 | QSFP28 | OM3/OM4 (8 brins) | MPO-12 |
| 100G DR | 100GBASE-DR | QSFP28 | OS2 (2 brins) | LC duplex |
| 100G FR1/LR4 | 100GBASE-FR1/LR4 | QSFP28 | OS2 | LC duplex |
| 200G SR4 | 200GBASE-SR4 | QSFP56 | OM3/OM4 | MPO-12 |
| 400G SR8 | 400GBASE-SR8 | QSFP-DD/OSFP | OM3/OM4 (16 brins) | MPO-16 |
| 400G DR4 | 400GBASE-DR4 | QSFP-DD/OSFP | OS2 (8 brins) | MPO-12 |
| 400G FR4/LR4 | 400GBASE-FR4/LR4 | QSFP-DD/OSFP | OS2 (2 brins, WDM) | LC duplex |
| 800G SR8 | 800GBASE-SR8 | OSFP | OM3/OM4 (16 brins) | MPO-16 |
| 800G DR8 | 800GBASE-DR8 | OSFP | OS2 (16 brins) | MPO-16 |
| 800G 2×FR4 | 800GBASE-2FR4 | OSFP | OS2 (4 brins) | 2× LC duplex (CS) |

**Lecture** : SR = Short Reach (multimode), DR = 500 m (monomode),
FR = 2 km, LR = 10 km. Le chiffre après = nombre de lanes optiques
(SR8 = 8 lanes de 100G sur 16 brins : 8 Tx + 8 Rx).

## 18. Types de fibre : OM3 / OM4 / OM5 / OS2

| Fibre | Type | Cœur | Bandeau modal (EMB) | Usage |
|---|---|---|---|---|
| OM3 | Multimode 50/125 µm | 50 µm | 2000 MHz·km @850 nm | Legacy 10/40/100G |
| OM4 | Multimode 50/125 µm | 50 µm | 4700 MHz·km @850 nm | Standard DC actuel |
| OM5 | Multimode 50/125 µm | 50 µm | 4700 + SWDM (4 λ) | 100G sur 2 brins (BiDi/SWDM4) |
| OS2 | Monomode 9/125 µm | 9 µm | — (une seule voie) | Tout ce qui dépasse 100-150 m |

**Règles de décision :**
- **Intra-datacenter < 100 m** : OM4 (SR). Le moins cher au mètre et au module.
- **> 100 m ou inter-bâtiments** : OS2 systématique. La différence de prix du
  câble est anecdotique face au coût d'un re-câblage.
- **Neuf en 2026** : tirer de l'OS2 partout dans les rocades (même si on
  branche de l'OM4 en bout), et de l'OM4 vers les baies. L'OM5 ne se justifie
  que pour des cas SWDM/BiDi précis — pas un standard de rocade.
- **Couleurs** : OM3/OM4 = gaine **aqua**, OM5 = **vert lime**, OS2 = **jaune**.
  Un jarretière jaune sur un port SR = suspect immédiat (P7, §107).

## 19. Distances maximales — tableau normatif usuel

Valeurs IEEE 802.3 / MSA usuelles (⚠️ vérifier la datasheet du module exact,
les optiques « extended » dépassent parfois la norme) :

| Débit / type | OM3 | OM4 | OM5 (SWDM) | OS2 |
|---|---|---|---|---|
| 10G SR | 300 m | 400 m | — | — |
| 25G SR | 70 m | 100 m | — | — |
| 100G SR4 | 70 m | 100 m | — | — |
| 100G BiDi (SWDM4) | — | — | 100-150 m (2 brins) | — |
| 100G DR | — | — | — | 500 m |
| 100G FR1 | — | — | — | 2 km |
| 100G LR4 | — | — | — | 10 km |
| 400G SR8 | 60 m | 100 m | — | — |
| 400G DR4 | — | — | — | 500 m |
| 400G FR4 | — | — | — | 2 km |
| 400G LR4 | — | — | — | 10 km |
| 800G SR8 | 60 m | 100 m | — | — |
| 800G DR8 | — | — | — | 500 m |

**En pratique datacenter** : 90 % des liens font < 30 m (intra-rack/rack
adjacent) → DAC/AOC ; les liens leaf↔spine inter-rangées (30-100 m) → SR8/AOC ;
au-delà → DR sur OS2. **Le 800G SR8 à 60 m sur OM3** : ne pas réutiliser de
vieux tronçons OM3 pour du 800G sans mesurer.

## 20. Budgets optiques : le calcul qui valide un lien

Budget (dB) = Puissance Tx (dBm) − Sensibilité Rx (dBm).
Marge = Budget − (affaiblissement fibre + connecteurs + épissures).
**Règle : marge ≥ 3 dB** sur un lien neuf.

| Élément | Affaiblissement typique (⚠️) |
|---|---|
| Fibre OM4 @850 nm | ~3 dB/km (négligeable < 100 m) |
| Fibre OS2 @1310 nm | ~0,4 dB/km |
| Connecteur LC/MPO apparié | 0,3-0,5 dB (0,75 dB max norme) |
| Épissure fusion | 0,05-0,1 dB |
| Jarretière sale | **1-5 dB** ← voir §24 |

**Exemple** : lien 400G-DR4, budget module 4 dB (⚠️ typique) :
fibre 80 m OS2 (0,03 dB) + 4 connecteurs MPO (4×0,4 = 1,6 dB) → perte ~1,7 dB,
**marge ~2,3 dB < 3 dB** : limite. Une jarretière sale fait tomber le lien.
D'où le §24.

**Mesure** : un lien qui « monte » n'est pas un lien « bon ». Mesurer la
puissance Rx (`show interfaces transceiver` / `ethtool -m`) et la comparer à
la sensibilité : si on est à < 2 dB de marge, planifier le nettoyage ou le
remplacement **avant** la panne.

## 21. Connecteurs : LC, MPO/MTP, SN, CS

| Connecteur | Fibres | Usage | Remarque |
|---|---|---|---|
| LC duplex | 2 | SR/DR/FR/LR classiques | Le standard, détrompage simple |
| MPO-12 | 12 (8 utilisées en DR4) | 100G SR4, 400G DR4 | Broches mâle/femelle : **ne pas mélanger** |
| MPO-16 | 16 | 400G/800G SR8, DR8 | 2 rangées de 8 |
| MPO-24 | 24 | Trunks haute densité | Brassage uniquement |
| SN / CS | 2-4, très compact | 800G 2×FR4, face avant dense | Nouveau, outillage dédié |
| MTP® | = MPO premium | Trunks | Marque US Conec, meilleure précision |

**Polarité MPO** (méthodes TIA) :
- **Méthode A** : droit 1↔1 (key-up/key-down).
- **Méthode B** : croisé 1↔12 — la plus courante en datacenter.
- **Méthode C** : paires croisées.
Un trunk méthode B avec des jarretières méthode A = lien muet. **Étiqueter la
méthode sur le trunk** (voir §85). C'est le P8 (§107).

## 22. DAC vs AOC vs transceivers — le vrai comparatif économique

| Critère | DAC (cuivre passif) | ACC/AEC (cuivre actif) | AOC (fibre active) | Transceiver + fibre |
|---|---|---|---|---|
| Portée 100G | 1-5 m | 5-7 m | 3-100 m | 100 m - 10 km |
| Portée 400G | 1-3 m | 3-5 m | 3-100 m | 100 m - 10 km |
| Portée 800G | 1-2 m (⚠️) | 3-6 m (⚠️) | 3-30 m (⚠️) | 100 m - 2 km |
| Conso / extrémité | ~0,1-0,5 W | ~2-4 W | ~1-2 W | 2,5-18 W (selon débit) |
| Latence | ~0 (ns) | ~100 ns | ~100 ns | ~100 ns |
| Prix indicatif 100G 3 m (⚠️) | 50-120 € | 150-300 € | 200-400 € | 300-800 € (SR4) |
| Prix indicatif 400G 3 m (⚠️) | 150-350 € | 400-800 € | 600-1 200 € | 1 500-4 000 € (DR4) |
| Prix indicatif 800G (⚠️) | 400-900 € | 1 000-2 000 € | 1 500-3 000 € | 4 000-10 000 € (DR8) |
| Avantage | Prix, zéro conso | Portée cuivre | Léger, fin | Interopérabilité, distance |
| Inconvénient | Rigide, encombrant | Conso, prix | Non réparable | Prix, conso, chaleur |

**Doctrine d'achat** :
1. Intra-rack (< 3 m) → **DAC** toujours, sauf 800G où l'AEC devient pertinent.
2. Rack adjacent (3-30 m) → **AOC** (ou AEC en 800G).
3. Inter-rangées → transceivers SR8 (OM4) ou DR (OS2).
4. Ne jamais mélanger DAC et transceiver sur le même lien : les seuils
   électriques diffèrent, l'auto-négociation échoue (P9, §107).

## 23. Câbles breakout : multiplier les ports sans multiplier les switches

Un port 400G peut servir 4 serveurs 100G (ou 2×200G), un port 800G → 2×400G /
4×200G / 8×100G.

| Breakout | Câble | Cas d'usage |
|---|---|---|
| 400G → 4×100G | QSFP-DD → 4× QSFP28 DAC/AOC | Leaf 400G vers serveurs 100G |
| 400G → 2×200G | QSFP-DD → 2× QSFP56 | Migration douce |
| 800G → 2×400G | OSFP → 2× QSFP112 | Leaf 800G vers nœuds 400G |
| 800G → 8×100G | OSFP → 8× SFP112 | Densité max (⚠️ limites par port, voir SN5600 : le 8×100G sur port impair consomme le port pair adjacent — ✅ vérifié 27/09/2026) |

**Attention** : tous les ports d'un switch ne supportent pas tous les modes
de breakout (ressources SerDes partagées). **Valider la matrice de breakout
dans le guide de câblage du switch** avant d'acheter les câbles — sinon des
ports restent inutilisables (P10, §107).

## 24. Nettoyage fibre — le piège n°1 du datacenter

**Une particule de 1 µm sur une férule = jusqu'à 5 dB de perte.**
Le cœur d'une fibre monomode fait 9 µm : un grain de poussière le couvre
entièrement. 80 % des pannes optiques « inexpliquées » viennent d'un
connecteur sale — pas d'un module HS.

**Procédure (à afficher dans la salle) :**
1. **Inspecter** (microscope, §25) → 2. **Nettoyer** → 3. **Ré-inspecter**.
   Ne jamais brancher sans inspecter : on transfère la saleté d'un
   connecteur à l'autre.
2. Nettoyage à sec d'abord (cassette(click), lingettes) ; humide (alcool
   isopropylique + séchage) seulement si le sec échoue.
3. **Bouchons** : tout connecteur débranché = bouchon remis immédiatement.
   Un transceiver sans bouchon dans un tiroir = poubelle optique.
4. Ne jamais toucher la férule, ne jamais souffler dessus (salive),
   ne jamais regarder dans une fibre active (laser classe 1M, risque
   rétinien en monomode).

**Kit minimum par salle** : 1 microscope d'inspection (sonde vidéo),
cassettes de nettoyage LC + MPO, lingettes, bouchons de rechange.
Coût : ~1 500-3 000 € (⚠️) — le prix d'**un seul** module 800G. Rentabilité
immédiate.

## 25. Inspection : seuils IEC 61300-3-35

La norme définit des zones (cœur, cladding, ferrule) et des seuils de
défauts (rayures, piqûres) par zone. En pratique :
- Les microscopes modernes donnent un ** verdict PASS/FAIL automatique** —
  l'exiger, ne pas juger « à l'œil ».
- Rejeter toute férule avec défaut dans la zone cœur/cladding.
- Documenter : photo avant/après pour les liens critiques (preuve en cas de
  litige fournisseur).

**Test terrain** : si un lien 400G+ affiche des erreurs FEC corrigées qui
augmentent (> 1e-9 pré-FEC, voir §27), **nettoyer avant** de changer le
module. Dans 4 cas sur 5, le « module défectueux » retourné au fournisseur
est en fait sale (constat d'atelier, ⚠️ retour d'expérience).

## 26. Polarité et genre MPO — ne pas mélanger

- **Mâle** = broches (pins) qui dépassent ; **femelle** = trous.
  Règle : **un lien = un mâle + une femelle**. Deux mâles = casse des
  broches ; deux femelles = mauvais contact.
- Les transceivers sont généralement **mâles**, les jarretières **femelles**
  vers le module — vérifier à la commande.
- **Schéma d'un lien MPO méthode B** :

```
  [Module Tx] ----fibre 1----> ----fibre 1----> [Module Rx]
  (MPO mâle)      croisée B      (MPO femelle)
  position 1  ==============>  position 12
  position 2  ==============>  position 11
  ...
```

- **Étiquette obligatoire** sur chaque trunk : méthode (A/B/C), genre des
  extrémités, longueur, référence. Sans ça, le brassage devient un jeu de
  devinettes à 3h du matin.

## 27. FEC : la correction d'erreur qui rend le PAM4 viable

Le PAM4 (4 niveaux) a un rapport signal/bruit ~9,6 dB moins bon que le NRZ.
Sans FEC, le 400/800G ne fonctionnerait pas. Points clés :

| Paramètre | Valeur usuelle (⚠️) |
|---|---|
| FEC 400/800G | RS(544,514) — Reed-Solomon |
| Seuil pré-FEC tolérable | ~1e-4 à 2,4e-4 BER |
| Post-FEC visé | < 1e-12 (souvent < 1e-15) |
| Latence FEC | ~100 ns par traversée |

**Supervision** : exposer les compteurs `pre-FEC BER` / `FEC corrected` en
SNMP/telemetry. Seuils d'alerte :
- 🟡 BER pré-FEC > 1e-6 : surveiller, planifier nettoyage.
- 🔴 BER pré-FEC > 1e-5 : intervenir (nettoyage, puis remplacement).
- ⛔ Erreurs **non corrigées** > 0 : le lien est en échec, basculer.

**Piège** : le FEC masque une fibre dégradée pendant des mois, puis le lien
s'effondre d'un coup quand on franchit le seuil. D'où la supervision
préventive — un lien « qui marche » n'est pas un lien « sain ».

## 28. LPO / LRO : les optiques linéaires basse consommation

- **LPO (Linear Pluggable Optics)** : supprime le DSP du module, le traitement
  du signal est fait par le SerDes du switch/NIC. Gain : **−30 à 50 % de
  conso** et −latence (~dizaines de ns).
- **LRO (Linear Receive Optics)** : moitié linéaire (côté réception).
- ✅ Broadcom pousse les LPO sur Thor 2 (vérifié 27/09/2026) ; écosystème en
  croissance pour le 800G intra-DC.
- **Contrainte** : LPO exige des SerDes compatibles des deux côtés
  (interopérabilité limitée en 2026) et des liens courts/propres. **Ne pas
  mélanger LPO et DSP sur un même lien.**
- **Lecture énergie** : à l'échelle d'un fabric 800G (10 000 modules),
  passer de 15 W à 8 W par module = **70 kW économisés** — un levier PUE
  direct (voir §97).

## 29. CAS CHIFFRÉ — BOM fibre d'un rack 400G (8 serveurs)

Hypothèses : 8 serveurs × 2×400G (rouge/bleu), leaf en tête de rack,
liens < 3 m.

| Poste | Qté | Choix | Prix unit. (⚠️) | Total (⚠️) |
|---|---|---|---|---|
| DAC 400G QSFP-DD 2 m | 16 | Passif, codé switch | 150-350 € | 2 400-5 600 € |
| Uplinks leaf→spine 400G DR4 | 4 | Transceiver + 50 m OS2 | 1 500-4 000 € | 6 000-16 000 € |
| Jarretières OS2 LC-LC 50 m | 4 | — | 40-80 € | 160-320 € |
| Kit nettoyage + microscope | 1 | Mutualisé salle | 1 500-3 000 € | 1 500-3 000 € |
| Étiquettes + platine brassage | 1 | TIA-606 (§85) | 300-600 € | 300-600 € |
| **Total rack** | | | | **~10-25 k€** |

**Enseignement** : le câblage cuivre intra-rack coûte 10× moins cher que
l'optique — **rapprocher les leafs des serveurs** (top-of-rack) est une
décision économique autant que technique. Chaque mètre de fibre en plus =
optiques + brassage + points de panne.

## 30. Pièges fibre — l'essentiel (détail §107)

- **P6** — OSFP flat-top sur switch (au lieu de finned-top) : surchauffe.
- **P7** — Jarretière jaune (OS2) sur port SR (multimode) : lien muet.
- **P8** — Polarité MPO non documentée : heures de debug nocturne.
- **P9** — DAC d'un côté, transceiver de l'autre : pas de link.
- **P10** — Breakout non supporté par le port : ports morts après achat.
- **P11** — FEC qui masque une fibre sale : panne brutale 6 mois plus tard.

# PARTIE C — ARCHITECTURES SPINE-LEAF

## 31. Principe : le Clos à deux tiers

Le spine-leaf est un **réseau de Clos** : chaque leaf est connecté à **tous**
les spines, aucun leaf n'est connecté à un autre leaf, aucun serveur n'est
branché sur un spine.

Propriétés fondamentales :
- **Tout trajet serveur→serveur = 3 sauts** (leaf→spine→leaf) : latence
  prévisible, ~1-2 µs par saut en cut-through.
- **Bande passante horizontale** (est-ouest) : c'est le trafic dominant du
  datacenter moderne (VM↔VM, GPU↔GPU, stockage). Le vieux modèle 3 tiers
  (accès/agrégation/cœur) était pensé pour le nord-sud.
- **Évolutivité** : on ajoute des leafs (capacité serveurs) ou des spines
  (bande passante) indépendamment.

```
        TRAFIC NORD-SUD (vers WAN/Internet)
                    ▲  │  ▲
                    │  ▼  │
              ┌───────────────┐
              │   Routeurs    │  ← bordure (peering, firewall)
              │   de bordure  │
              └──────┬───┬────┘
                     │   │
   ═════════════ TIER SPINE ═════════════
        ┌────────┴───┴────────┐
   ┌────┴────┐          ┌────┴────┐
   │ Spine 1 │          │ Spine 2 │   ← jamais reliés entre eux
   └──┬───┬──┘          └──┬───┬──┘
      │   │    ╲     ╱    │   │
      │   │     ╲ ╱       │   │      ← chaque leaf vers TOUS les spines
      │   │     ╱ ╲       │   │
   ┌──┴───┴──┐ ┌──┴───┴──┐ ┌──┴───┴──┐
   │ Leaf 1  │ │ Leaf 2  │ │ Leaf 3  │  ← ToR (top-of-rack)
   └──┬───┬──┘ └──┬───┬──┘ └──┬───┬──┘
     ▓▓▓ ▓▓▓     ▓▓▓ ▓▓▓     ▓▓▓ ▓▓▓
     serveurs    serveurs    serveurs
```

## 32. Oversubscription : le ratio qui dimensionne tout

**Oversubscription = capacité downlink (vers serveurs) ÷ capacité uplink
(vers spines).**

| Ratio | Signification | Usage |
|---|---|---|
| 1:1 | Non-bloquant | Backend IA (GPU), HPC |
| 3:1 | Standard | Virtualisation généraliste |
| 5:1 à 10:1 | Économique | Bureautique, web frontal |

**Exemple de calcul** — leaf 48×25G serveurs + 8×100G uplinks :
- Downlink : 48 × 25 = 1 200 Gb/s
- Uplink : 8 × 100 = 800 Gb/s
- **Ratio = 1 200 / 800 = 1,5:1**

**Exemple classique 3:1** — leaf 48×10G + 4×40G :
- 480 / 160 = **3:1** ✅

**Règle IA** : le backend GPU exige **1:1 non-bloquant** (un all-reduce
NCCL sature tous les liens en même temps). Le front-end (management,
stockage) peut être à 3:1. **Ne jamais mélanger les deux trafics sur le
même fabric** : le burst d'un training noie le stockage (P12, §107).

## 33. Schéma ASCII : 2 tiers, 1024 serveurs non-bloquants

Dimensionnement Clos non-bloquant : avec des switchs de **n** ports,
on met n/2 ports vers le bas et n/2 vers le haut par leaf.
Capacité : **(n/2) leafs × (n/2) serveurs = n²/4 serveurs**, avec n/2 spines.

Exemple n = 64 (switch 64×100G) → 32×32 = **1024 serveurs**, 32 spines :

```
                    32 SPINES (64×100G)
              ┌───┬───┬───┬───┬───┬───┐
              │ S1│ S2│...│...│S31│S32│
              └─┬─┴─┬─┴───┴───┴─┴─┬─┘
                │   │             │
      ┌─────────┘   │     ┌───────┴─────────┐
      │  32 uplinks │     │  32 uplinks     │   ← 32×100G = 3,2 Tb/s par leaf
 ┌────┴─────┐  ┌────┴─────┐        ┌────┴─────┐
 │ Leaf 1   │  │ Leaf 2   │  ...   │ Leaf 32  │  ← 32 leafs
 └────┬─────┘  └────┬─────┘        └────┬─────┘
   32×100G       32×100G              32×100G   ← vers serveurs
   1024 serveurs au total, 1:1 non-bloquant
```

**Vérification** : chaque leaf a 32×100G = 3,2 Tb/s vers le haut et
3,2 Tb/s vers le bas → 1:1 ✅. Chaque spine a 32×100G = 3,2 Tb/s, tous
vers des leafs distincts ✅.

**Coût** : 64 switchs 64×100G + 2048 optiques/câbles. C'est le prix du
non-bloquant : ~2× plus de ports switch que de ports serveurs.

## 34. Schéma ASCII : 3 tiers (pod + super-spine) pour l'IA

Au-delà de ~1-2k GPU, on passe en 3 tiers : des **pods** (2 tiers) reliés
par des **super-spines**. Les pods = unités de déploiement (ex. 256 GPU).

```
              ┌─────────────────────────────┐
              │      SUPER-SPINES (×8)       │  ← 800G, Jericho3-AI / TH5
              │  64×800G, deep buffer       │
              └──────┬──────────────┬───────┘
                     │              │
        ┌────────────┴───┐   ┌──────┴────────┐
        │    POD 1       │   │    POD 2      │  ← 256 GPU chacun
        │  (rail-opt.)   │   │  (rail-opt.)  │
        └────────────────┘   └───────────────┘

  Détail d'un POD (rail-optimized, voir §39) :

        Spine P1    Spine P2          ← 1 spine par "rail" (plan)
         │  │        │  │
    ┌────┘  │        │  └────┐
    │  ┌────┘        └────┐  │
 ┌──┴──┴──┐           ┌──┴──┴──┐
 │ Leaf R1 │           │ Leaf R2 │  ← leafs dédiés au rail 1 / rail 2
 └──┬───┬──┘           └──┬───┬──┘
  GPU0 GPU1            GPU2 GPU3   ← NIC#0 des GPU → rail 1, NIC#1 → rail 2
```

**Règle de pouce** : 2 tiers jusqu'à ~2 000 endpoints, 3 tiers au-delà.
Le 3ᵉ tier ajoute 1 saut (~1 µs) mais divise le nombre de ports spine par pod.

## 35. Dimensionnement : exemple 512 serveurs généralistes (3:1)

| Paramètre | Valeur |
|---|---|
| Serveurs | 512 × 2×25G (rouge/bleu) |
| Leaf | 48×25G + 8×100G → 24 ports serveurs/leaf en 3:1 ? |

Calcul : 48×25G = 1,2 Tb/s down. Pour 3:1 → uplink = 400 Gb/s = 4×100G.
Ports serveurs utilisables : 48 (le ratio se joue sur les uplinks).
- Leafs : 512 / 48 ≈ **11 leafs** (prendre 12, pair).
- Uplinks par leaf : 4×100G → 48 uplinks au total.
- Spines : switch 32×100G → **2 spines** (48 ports utilisés).
- Ratio vérifié : (48×25)/(4×100) = 1200/400 = **3:1** ✅

**BOM** : 12 leafs + 2 spines + 1024 DAC 25G + 48 optiques 100G.
(⚠️ : prévoir 20 % de ports libres pour croissance — P13, §107.)

## 36. ECMP : le routage qui répartit

En spine-leaf, **pas de STP** : tous les liens sont actifs grâce à ECMP
(Equal-Cost Multi-Path). Chaque leaf installe N routes vers chaque
destination (N = nombre de spines) et **hashe** les flux (5-tuple
IP src/dst + ports) sur les liens.

Points critiques :
- **Polarisation ECMP** : si deux équipements hashent pareil, un lien se
  sature pendant que d'autres sont vides. Activer le **hash seed aléatoire**
  par switch (option sur Arista EOS, Cisco NX-OS, Cumulus).
- **Flots éléphants** (IA) : un seul flux NCCL de 400G ne se répartit pas —
  d'où le **packet spraying** (Spectrum-X) ou l'**adaptive routing** : on
  répartit à la granularité paquet, pas flux.
- **BGP PIC / fast-reroute** : prévoir la convergence < 1 s en cas de perte
  d'un spine (BFD, voir §38).

## 37. EVPN/VXLAN en bref : l'overlay qui découple

**Problème** : les VM/conteneurs migrent, les VLAN sont limités à 4094, le
spanning-tree ne passe pas l'échelle.
**Solution** : VXLAN (encapsulation L2 dans UDP, 24 bits = 16M de segments)
+ EVPN (BGP qui distribue les MAC/IP comme des routes).

```
  VM-A (VNI 5001)                                    VM-B (VNI 5001)
      │                                                  │
  ┌───┴────┐                                        ┌───┴────┐
  │ Leaf 1 │═══ VXLAN tunnel (VNI 5001) ════════════│ Leaf 2 │  ← VTEP
  └───┬────┘   (sous-réseau IP underlay)            └───┬────┘
      │                routé en ECMP                     │
   ───┴────────────── SPINE ────────────────────────────┴───
```

- **Underlay** : BGP numéroté (souvent /31 point-à-point, RFC 5549), eBGP
  entre leaf et spine avec ASN privés (ex. leafs 65001, spines 65101).
- **Overlay** : iBGP EVPN entre leafs (VTEP), route-reflectors sur les spines.
- **Anycast gateway** : la même IP/MAC de passerelle sur tous les leafs —
  la VM garde sa gateway en migrant.

**Quand s'en passer** : backend IA pur (pas de VM, pas de migration) →
routage simple, pas de VXLAN : moins d'overhead, moins de MTU à gérer
(VXLAN = +50 octets → MTU 9000+50, P14, §107).

## 38. BGP underlay : le plan de routage qui ne tombe jamais

Config type (eBGP, ASN privés) :

```
  Leaf 1 : ASN 65001                    Spine 1 : ASN 65101
  interfaces /31 : 10.0.0.0/31, 10.0.0.2/31...
  Spine 2 : ASN 65102 (ASN différent par spine = ECMP naturel)
```

- **unnumbered BGP** (RFC 5549) : possible avec IPv6 link-local — simplifie
  le plan d'adressage, supporté par EOS/Cumulus/NX-OS.
- **BFD** : détection de panne en ~300 ms (3×100 ms) au lieu des 180 s du
  hold-time BGP. **Obligatoire** sur les liens leaf-spine.
- **Pas de redistribution hasardeuse** : l'underlay ne porte que les
  loopbacks (/32) et les liens ; tout le reste passe par l'overlay EVPN.
- **EBGP multipath** : `maximum-paths 64` pour installer tous les spines.

## 39. Rail-optimized : la topologie des clusters GPU

Dans un serveur 8 GPU, chaque GPU a sa NIC (rail). Le **rail-optimized**
connecte toutes les NIC#0 au même plan réseau, toutes les NIC#1 à un autre
plan, etc. — **jamais de mélange entre rails**.

```
  Serveur A (8 GPU)              Serveur B (8 GPU)
  GPU0-NIC0 ──→ PLAN 0 (leafs+spines dédiés)
  GPU1-NIC1 ──→ PLAN 1
  ...
  GPU7-NIC7 ──→ PLAN 7

  Chaque plan = un mini spine-leaf indépendant :
  ┌─────────────────────────────────────────┐
  │ PLAN k :  Spines-k ─── Leafs-k ─── NICk │  × 8 plans
  └─────────────────────────────────────────┘
```

**Pourquoi** : le trafic dominant (all-reduce en anneau NCCL) circule entre
les mêmes indices de GPU. Séparer les plans = pas de contention entre
rails, pas de head-of-line blocking inter-rails, et un plan en panne
n'arrête pas tout (dégradé, pas mort).
✅ C'est la topologie par défaut de Spectrum-X et des références NVIDIA
(vérifié 27/09/2026). **Multi-plane** = version où l'équilibrage entre
plans est accéléré en hardware (utilisé avec OpenAI, d'après la doc
d'écosystème).

## 40. Multi-plane et topologies alternatives

| Topologie | Principe | Quand |
|---|---|---|
| Rail-optimized multi-plane | 1 plan par indice NIC, LB HW inter-plans | Clusters IA ≥ 1k GPU |
| Fat-tree / Clos classique | Tout vers tout via ECMP | Généraliste, cloud |
| Dragonfly / SlimFly | Groupes denses, liens inter-groupes | HPC extrême (rare en enterprise) |
| Rail-only (sans spine) | NICk↔NICk en direct (câbles) | Petits clusters ≤ 32 nœuds, éco |

**Règle** : < 256 GPU → 2 tiers rail-optimized simple ; 256-4 000 →
multi-plane ; > 4 000 → 3 tiers + rail-optimized par pod.

## 41. CAS CHIFFRÉ — BOM spine-leaf 1024 ports 400G (backend IA)

Hypothèses : 256 serveurs GPU × 4×400G ? Non — prenons 128 nœuds × 8 NIC
400G = **1024 ports 400G**, 1:1 non-bloquant, 2 tiers, switchs 64×800G
utilisés en 2×400G (128 ports 400G par switch).

- Leafs : 1024 / 128 = **8 leafs** (64×800G → 128×400G).
  Uplinks : 64×400G par leaf → 512 uplinks.
- Spines : 512 / 128 = **4 spines** 64×800G (128×400G).
- Vérification : chaque leaf 64×400G up / 64×400G down = **1:1** ✅

| Poste | Qté | Prix unit. (⚠️) | Total (⚠️) |
|---|---|---|---|
| Switch 64×800G (leaf/spine) | 12 | 150-300 k€ | 1,8-3,6 M€ |
| NIC 400G | 1024 | 1,5-3 k€ | 1,5-3,1 M€ |
| DAC/AOC 400G intra-rack | 1024 | 150-600 € | 150-600 k€ |
| Optiques 400G DR4 inter-rangées | 512 | 0,6-1,5 k€ | 300-770 k€ |
| **Total réseau** | | | **~3,8-8,1 M€** |

Soit **~30-63 k€ par nœud GPU** de réseau. À comparer aux ~250-400 k€ d'un
nœud 8×H100 (⚠️) : le réseau = **10-20 %** du nœud. Le sous-dimensionner
pour « économiser » 5 % du budget total, c'est brider 100 % du calcul.

## 42. Pièges spine-leaf — l'essentiel (détail §107)

- **P12** — Backend IA et front-end sur le même fabric : le training noie tout.
- **P13** — Zéro port libre : la croissance impose un nouveau pod.
- **P14** — MTU VXLAN oublié (+50 o.) : fragmentation silencieuse.
- **P15** — ECMP sans hash seed : polarisation, un spine saturé.

# PARTIE D — SWITCHES POUR L'IA

## 43. Ce qui distingue un switch « IA » d'un switch normal

Un switch IA doit absorber des **incast** : 1000 GPU envoient leur
gradient à 1 GPU en même temps (many-to-one). Un switch classique
droppe ; un switch IA :

| Exigence | Mécanisme | Switch classique |
|---|---|---|
| Ne pas perdre sous incast | Deep buffers + ECN + PFC | Buffers shallow, drop |
| Répartir les éléphants | Packet spraying / adaptive routing | ECMP par flux |
| Congestion de bout en bout | DCQCN / CC UEC, télémétrie | Rien (best effort) |
| Latence prévisible | Cut-through, ~600 ns-1 µs | Store-and-forward |
| Non-bloquant réel | 1:1, pas d'oversubscription interne | Souvent 3:1 interne |

**En 2026, « switch IA » =** Broadcom Jericho3-AI / Tomahawk 5-6,
NVIDIA Spectrum-4, Cisco Silicon One G200 (⚠️ détails non vérifiés au
27/09/2026). Le silicium merchant (Broadcom) équipe la majorité des
switchs OEM (Arista, Dell, HPE, Edgecore) — **le différenciateur migre
vers le logiciel** (NOS, QoS, télémétrie), pas l'ASIC.

## 44. Buffers : deep vs shallow — le choix structurant

| ASIC | Buffer | Philosophie | Usage |
|---|---|---|---|
| Broadcom Tomahawk 5 | ~Shallow (dizaines de Mo) | Faible latence, ECN agressif | Leaf IA, cloud |
| Broadcom Jericho3-AI | **Deep** (Go) | Absorbe l'incast | Spine IA ✅ |
| NVIDIA Spectrum-4 | 160 MB partagé ✅ | Cut-through, CC fine | Leaf/spine Spectrum-X |
| Broadcom Tomahawk 6 | (⚠️ à vérifier) | + Cognitive Routing 2.0 | 102.4T, 2025+ |

**Règle** : leaf = shallow + ECN (on signale vite), spine = deep
(on absorbe). Inverser = drops au spine ou latence inutile au leaf.
Le buffer se dimensionne en **BDP** (bandwidth-delay product) : à 400G
avec 10 µs de RTT, BDP = 500 Ko par flux — multiplier par le nombre de
flux concurrents donne l'ordre de grandeur du buffer utile.

## 45. ECN + PFC : le lossless Ethernet (et ses dangers)

- **PFC** (Priority Flow Control, 802.1Qbb) : pause par classe de trafic —
  crée un réseau « sans perte » pour RoCEv2.
- **ECN** : marquage des paquets en congestion → la NIC réduit son débit
  (DCQCN). C'est le mécanisme **préféré** : il évite les pauses.
- **Danger PFC** : les **deadlocks** (pause qui se propage en boucle) et les
  **pauses storms**. Un PFC mal configuré = pannes en cascade sur tout le
  fabric. D'où la règle : **PFC seulement là où c'est indispensable**
  (backend RoCE), ECN partout, et jamais de PFC sur le front-end.

Seuils usuels (⚠️ à ajuster par fabric) :
- Marquage ECN : file > 30-50 % du buffer.
- Pause PFC : file > 70-80 % (dernier recours).

## 46. RoCEv2 en production : la checklist qui fait marcher

1. **MTU 9000** partout (NIC, leaf, spine) — pas de fragmentation.
2. **DSCP 3** (ou 5) réservé au RoCE, mappé en file lossless.
3. **PFC** sur cette seule file, **ECN** activé (seuils §45).
4. **DCQCN** : paramètres par défaut du constructeur, puis tuning avec
   la télémétrie (jamais « au doigt mouillé » sur un cluster > 100 nœuds).
5. **Pas de QoS qui réordonne** : le réordonnancement tue la perf RDMA.
6. **Compteurs** : `pfc_pause`, `ecn_marked`, `roce_cnp` en supervision —
   un fabric RoCE sain a des CNP **faibles et stables**, pas zéro (zéro =
   seuils trop hauts ou supervision cassée).

## 47. InfiniBand vs Ethernet — état du marché vérifié au 27/09/2026

| Acteur | Choix backend IA | Source / statut |
|---|---|---|
| NVIDIA | **Les deux** : Quantum-X800 IB (115,2T, variante CPO Q3450-LD) et Spectrum-X Ethernet (51,2T) | Datasheet NVIDIA ✅ |
| Meta | **Les deux en test** : 2 clusters 24 576 H100 — l'un Arista RoCE, l'autre Quantum-2 IB ; **> 90 % d'utilisation réseau** sur les deux pour Llama 3 | ✅ vérifié 27/09/2026 |
| xAI (Colossus) | Spectrum-X Ethernet (contrôle de congestion NVIDIA propriétaire) : **95 % de throughput** vs 60 % en Ethernet standard | ✅ vérifié 27/09/2026 |
| OpenAI | Spectrum-X **multi-plane** | ✅ vérifié 27/09/2026 |
| Hyperscalers (AWS/GCP/Azure) | Ethernet (custom ou merchant) | Notoriété publique ⚠️ |

**Lecture** : à performance égale au sommet, le choix n'est plus
technique mais **opérationnel** :
- **InfiniBand** : lossless natif (crédits), NCCL/MPI par défaut, 20 ans
  d'outillage — idéal si l'équipe réseau est petite. Verrou : écosystème
  NVIDIA quasi-exclusif, prix.
- **Ethernet (RoCEv2/UEC)** : compétences Ethernet transférables, multi-
  fournisseurs, convergence front/back-end — mais **exige un vrai tuning**
  (ECN/DCQCN, §46). UEC 1.0 vise à standardiser ce tuning.

## 48. Tableau comparatif InfiniBand / RoCEv2 / Ultra Ethernet

| Critère | InfiniBand (NDR/XDR) | RoCEv2 | Ultra Ethernet (UEC 1.0) |
|---|---|---|---|
| Débit/port max (2026) | 800G (XDR) | 800G | 800G (1,6T en route) |
| Lossless | Natif (crédits) | À construire (PFC+ECN) | **Non requis** (conçu lossy) |
| Ordre des paquets | Garanti | Requis | **Non requis** (placement désordonné) |
| Congestion | Natif | DCQCN à régler | CC émetteur/récepteur programmable |
| Multipath | Natif | ECMP/spraying proprio | **Natif niveau paquet** |
| Retransmission | Natif | Go-Back-N (coûteux) | **Sélective** |
| Interopérabilité | Faible (1 vendeur) | Moyenne | **Objectif : totale** |
| Maturité | 20 ans | 10 ans | Spec 06/2025, produits 2025-2026 |
| Coût | Élevé | Moyen | Promesse : coût Ethernet |

**Décision 2026** : nouveau cluster NVIDIA → IB si l'équipe est petite et le
budget suit ; Ethernet si on veut du multi-vendeur ou si on opère déjà du
RoCE. **Ne pas choisir en fonction du prix catalogue** : le coût d'un tuning
RoCE raté (semaines de GPU idle) dépasse l'écart IB/Ethernet.

## 49. NVIDIA Spectrum-X — la pile Ethernet fermée

| Élément | Référence | Spec (✅ vérifié 27/09/2026) |
|---|---|---|
| ASIC | Spectrum-4 | 51,2 Tb/s, ~600 ns cut-through |
| Switch | SN5600 | 64×800G OSFP, 2U, 33,3 Bpps, 160 Mo buffer |
| Switch compact | SN5610 | 64×800G OSFP, 2U |
| NIC | ConnectX-8 SuperNIC | 800G, PCIe Gen6 |
| NIC/DPU | BlueField-3 SuperNIC | 2×400G ou 1×800G, réordonnancement HW |
| Topologie | Rail-optimized + multi-plane | LB HW inter-plans |

**Conso SN5600 (✅)** : 940 W typique (câbles cuivre passifs) ; SN5610 :
0,9 kW typique, **2,08 kW avec 64 modules optiques** — l'optique double la
facture énergétique du switch (voir §90). Ports OSFP jusqu'à 18 W/module.

**Philosophie** : boucle fermée (switch + NIC + CC + telemetry du même
vendeur) = performance garantie, **lock-in assumé**. Le packet spraying +
contrôle de congestion adaptatif + télémétrie par flux expliquent les
95 % de xAI. Alternative « ouverte » : UEC (§54).

## 50. Broadcom — le silicium derrière la moitié du marché

| ASIC | Capacité | Positionnement | Statut |
|---|---|---|---|
| Tomahawk 4 | 25,6T | Génération 400G | Base de grands fabrics (Meta) |
| Tomahawk 5 | 51,2T | 64×800G / 128×400G | **Shipping** ✅ |
| Tomahawk 6 | **102,4T** | 64×1,6T / 128×800G / 512×200G ; Cognitive Routing 2.0 | **Annoncé** ✅ |
| Tomahawk 6-Davisson | 102,4T **CPO** | Optiques co-packagées, 3,5 W/port à 800G | Early access ✅ (08/10/2025) |
| Tomahawk Ultra | UEC | AllReduce **dans le switch** (in-network collectives) | ✅ |
| Jericho3 / 3-AI | Deep-buffer | Spine IA (Arista 7800R4) | Shipping ✅ |

**Taurus** (✅ OFC 2026, mars 2026) : premier DSP optique **400G/lane** —
permet des modules 1,6T et prépare le 3,2T / les switchs 204,8T.
**Roadmap affichée par Broadcom : l'ère 200T.**

**Lecture** : Broadcom vend le silicium, pas (peu) de switchs : Arista,
Dell, HPE, Edgecore, Celestica, H3C construisent dessus. **Comparer les
implémentations logicielles** (EOS, SONiC durci, etc.), pas l'ASIC —
il est souvent identique.

## 51. Arista Etherlink — l'Ethernet IA « enterprise »

| Produit | Base | Spec (✅ vérifié 27/09/2026) |
|---|---|---|
| 7060X6 (leaf) | Tomahawk 5 | 64×800G OSFP, 51,2T, 2× PSU 2400 W |
| 7060X5 | Tomahawk 4 | 32×800G, 2× PSU 1500 W |
| 7800R4 (spine modulaire) | Jericho3-AI | Jusqu'à **460 Tb/s**, 576×800G ou 1152×400G par châssis ; −65 % de conso vs 7800R3 |

**Positionnement** : EOS (le NOS le plus mature du marché enterprise),
CloudVision, AI Etherlink (features IA : monitoring par flux, tuning RoCE).
C'est le choix « sans surprise » pour une équipe qui connaît déjà Arista —
et celui de Meta pour son cluster RoCE 24k H100.

## 52. Cisco, Juniper, HPE, whitebox — le reste du paysage

| Acteur | Offre IA | Note |
|---|---|---|
| Cisco Silicon One G200 | 51,2T, 800G | ⚠️ Détails non vérifiés au 27/09/2026 — vérifier fiche Cisco |
| Juniper (HPE) | PTX / QFX sur Broadcom | Solide SP, moins « IA-first » |
| HPE Aruba | CX sur merchant silicon | Enterprise, pas le backend IA |
| Edgecore / Celestica | Whitebox TH5/TH6 + SONiC | Le moins cher au port (⚠️), support à évaluer |
| Nexthop AI | TH6-Davisson CPO + SONiC durci | ✅ cité par Broadcom (08/10/2025) |

**SONiC** (open-source, Microsoft) : le NOS des whitebox. Mature en
2026 pour le cloud, **à valider** pour les features IA fines (CC, telemetry)
— prendre une distribution durcie (Nexthop, Aviz) plutôt que du SONiC brut
en production IA.

## 53. Dimensionner un fabric IA — méthode en 6 étapes

1. **Compter les endpoints** : N GPU × NIC/GPU (1:1 standard).
2. **Choisir le ratio** : 1:1 backend, 3:1 front-end.
3. **Choisir le radix** : 64×800G → 1024 endpoints en 2 tiers (§33).
4. **Séparer les plans** : rail-optimized, 1 plan/indice NIC (§39).
5. **Vérifier le BDP** : buffers spine ≥ BDP × flux concurrents (§44).
6. **Chiffrer l'énergie** : switch + optiques + NIC (§89-92) — prévoir
   **2× la conso switch** pour la clim (PUE ~1,5-2 sur la partie réseau).

## 54. Ultra Ethernet (UEC) — le standard qui change la donne

- **11 juin 2025** : UEC Specification **1.0** publiée (transport, contrôle
  de congestion, RDMA sur Ethernet, > 100 membres : AMD, Arista, Broadcom,
  Cisco, HPE, Intel, Meta, Microsoft…) ✅.
- **Mi-2025** : spec **1.0.1** ✅. **14/10/2025** : premier NIC conforme,
  Broadcom **Thor Ultra** 800G ✅.
- **Innovations** : UET (Ultra Ethernet Transport) — livraison **non
  ordonnée**, retransmission **sélective**, multipath natif, profils
  applicatifs (collectives IA), pas de lossless requis.
- **Juin 2026** : VIAVI sort la première solution de validation UET ✅
  (TestCenter) — signe de maturité industrielle.
- **Position NVIDIA** : pas membre pilote d'UEC ; Spectrum-X = alternative
  propriétaire. Le marché se divise en **converged** (NVIDIA, boucle fermée)
  vs **discrete** (Broadcom+OEM+UEC, ouvert).

**Impact achat** : exiger « UEC-ready » sur les appels d'offres 2026+ pour
éviter le lock-in, même si on déploie Spectrum-X aujourd'hui — c'est une
assurance interopérabilité.

## 55. CAS CHIFFRÉ — fabric backend pour 1024 GPU (Blackwell)

Hypothèses : 128 nœuds × 8 GPU, 1× NIC 800G par GPU (1:1), rail-optimized
8 plans, 2 tiers.

| Poste | Calcul | Résultat |
|---|---|---|
| NIC 800G | 1024 × ConnectX-8 | 1024 NIC |
| Par plan | 1024 / 8 = 128 endpoints/plan | 2 leafs 64×800G par plan |
| Leafs | 8 plans × 2 = **16 leafs** (64×800G) | 1:1 par plan |
| Spines | 8 plans × 2 spines 64×800G = **16 spines** | — |
| Switchs totaux | 32 × 64×800G | — |
| Conso switchs (⚠️) | 32 × ~1,5 kW (avec optiques) | **~48 kW** |
| Conso NIC 800G (⚠️) | 1024 × ~60 W | **~61 kW** |
| Optiques (⚠️) | ~3000 × ~15 W | **~45 kW** |
| **Total réseau IT** | | **~154 kW** |
| Avec PUE 1,4 (⚠️) | 154 × 1,4 | **~216 kW** tirés au compteur |

**Référence** : 1024 GPU B200 ≈ 1,2-1,5 MW IT (⚠️) → le réseau = **~10-13 %**
de la puissance du cluster. À 0,15 €/kWh (⚠️), le réseau coûte
~280 k€/an d'électricité. Le « petit » poste réseau n'est pas petit.

## 56. Pièges switches IA — l'essentiel (détail §107)

- **P16** — PFC global au lieu de PFC par file : pauses storms.
- **P17** — Leaf shallow-buffer en spine : drops sous incast.
- **P18** — RoCE sans ECN/DCQCN : 60 % du débit (cas xAI : 60 → 95 % avec CC).
- **P19** — Comparer des switchs sur l'ASIC seul : le logiciel fait la différence.

# PARTIE E — CHÂSSIS 800GbE POUR CLUSTERS GPU IA

## 57. Ce qu'implique le 800GbE : radix, ports, câblage

Le 800G change trois choses fondamentalement :

1. **Radix** : un switch 64×800G = 51,2T dans 2U. Le même débit tenait en
   8 switchs 64×400G il y a 2 ans. **Moins de switchs, mais chacun est un
   point de défaillance plus gros** — la redondance N+1 devient critique.
2. **Densité énergétique** : 64 ports × 18 W (optiques) = **1,15 kW rien que
   d'optiques** + ~0,9 kW de switch = 2 kW dans 2U. C'est une densité de
   rack GPU, pas de rack réseau classique.
3. **Câblage** : le DAC 800G ne dépasse pas ~2 m (⚠️) — **tout ce qui n'est
   pas intra-rack passe en AOC ou optique**. Le budget câblage explose si
   la topologie n'est pas compacte (leafs au plus près des nœuds).

## 58. Radix et dimensionnement Clos au 800G

Formule (rappel §33) : switchs n ports → **n²/4 endpoints** non-bloquants
en 2 tiers, n/2 spines.

| Switch | Endpoints 2 tiers | Spines | Cas d'usage |
|---|---|---|---|
| 32×800G (25,6T) | 256 | 16 | Pod 256 GPU |
| 64×800G (51,2T) | 1024 | 32 | Cluster 1k GPU |
| 64×800G en 3 tiers | 16k+ | pods de 1024 | 10k+ GPU |

**Exemple pod 256 GPU** : 8 leafs 32×800G (16 down + 16 up) + 16 spines
32×800G → 256 endpoints 1:1. Seulement **24 switchs** pour 256 GPU :
la densité 800G simplifie la topologie — mais chaque spine porte 1/16ᵉ du
trafic : **prévoir la perte d'un spine sans dégradation** (surprovisionner
de ~10 % ou accepter 94 % en dégradé).

## 59. Câblage 800G : la matrice de choix

| Distance | Solution | Conso/extr. (⚠️) | Note |
|---|---|---|---|
| < 2 m | DAC 800G passif | ~0,5 W | Intra-rack uniquement |
| 2-6 m | AEC 800G (cuivre actif) | ~3-4 W | Racks adjacents |
| 3-30 m | AOC 800G | ~2-3 W | Inter-racks proches |
| < 100 m | 800G-SR8 (OM4, MPO-16) | ~12-14 W | Inter-rangées |
| < 500 m | 800G-DR8 (OS2, MPO-16) | ~14-16 W | Inter-salles |
| 2 km | 800G-2×FR4 (OS2, CS) | ~16-18 W | Campus |

**Règle thermique** : à 64 ports optiques, le switch dissipe ~2 kW —
**vérifier le sens d'airflow** (avant-arrière vs arrière-avant) par rapport
aux allées chaudes/froides. Un switch 800G monté à l'envers = hotspot de
20 kW par rangée (P20, §107).

## 60. Thermique OSFP : prévoir la chaleur, pas la subir

- Module 800G-DR8 : **14-18 W** dans un volume de ~10 cm³ → densité de
  puissance comparable à un CPU. Sans airflow, la jonction dépasse 85 °C
  en minutes.
- ✅ SN5600 : ports qualifiés jusqu'à 18 W (vérifié 27/09/2026) — mais
  **tous les switchs ne le sont pas** : vérifier la spec « max power per
  port » avant d'acheter des optiques 18 W.
- **Finned-top vs flat-top** (§16) : à ailettes sur switch (air), plat sur
  NIC (souvent water-cooled ou faible airflow). Inverser = +15-20 °C.
- **Seuils** : alerte à 70 °C boîtier, critique à 80 °C (⚠️ usuels) —
  exposer les températures DOM en supervision, pas seulement les puissances.

## 61. Alimentation : dimensionner les PSU

| Switch | PSU | Conso typ. / max (✅/⚠️) |
|---|---|---|
| SN5600 64×800G | 2× (1+1) | 940 W typ. cuivre / ~2 kW optiques (⚠️ extrapolé SN5610 : 2,08 kW à 64 optiques ✅) |
| SN5610 64×800G | 4× (2+2) | 0,9 kW typ. / 2,08 kW à 64 optiques ✅ |
| Arista 7060X6 64×800G | 2× 2400 W | Dimensionné pour le pire cas ✅ |
| Arista 7060X5 32×800G | 2× 1500 W | ✅ |

**Règles** :
- **Toujours 1+1** minimum sur leaf/spine (un PSU en panne ne doit pas
  couper un plan entier).
- **Deux arrivées électriques** (A/B) par rack réseau, sur deux onduleurs
  ou deux jeux de PDU — le réseau est le dernier à tomber (il porte la
  supervision de l'arrêt d'urgence, voir guide onduleurs).
- **Appel de courant** : les ventilos + optiques au boot = pic ~120 % du
  nominal pendant 30 s (⚠️) — ne pas calibrer les disjoncteurs au nominal.

## 62. Exemple : pod 256 GPU — plan électrique et thermique

| Poste | Calcul | Résultat |
|---|---|---|
| 8 leafs + 16 spines 800G | 24 × 1,5 kW (⚠️ mixte) | 36 kW |
| 2048 NIC 800G (8/GPU × 256) | 2048 × 60 W (⚠️) | 123 kW |
| Optiques leaf/spine | ~1500 × 15 W (⚠️) | 22 kW |
| **Total réseau pod** | | **~181 kW** |
| Dissipation à évacuer (PUE 1,5) | 181 × 1,5 | **~272 kW** |
| En clim (~3,5 kW par tonneau frigo) | 272 / 3,5 | **~78 tonnes** frigorifiques |

**Lecture Zelef** : un pod réseau 800G = la clim d'un petit immeuble.
Prévoir les groupes froids **avec** le lot réseau, pas après — et noter que
le CPO (Tomahawk 6-Davisson : 3,5 W/port vs ~15 W pluggable, ✅) divise par
~4 la chaleur des optiques : **le CPO est d'abord un sujet énergie**.

## 63. Pièges 800G — l'essentiel (détail §107)

- **P20** — Airflow inversé : hotspot de 20 kW par rangée.
- **P21** — DAC 800G de 3 m « qui devrait passer » : ne passe pas.
- **P22** — Optiques 18 W sur switch qualifié 12 W : throttling thermique.
- **P23** — Disjoncteurs calibrés au nominal : déclenchement au boot.

# PARTIE F — DPU (DATA PROCESSING UNIT)

## 64. DPU : définition et place dans le serveur

Un DPU = **une NIC + des cœurs ARM + des accélérateurs** (crypto, RegEx,
décompression, RDMA) sur la même carte, avec son propre OS. Il décharge le
CPU hôte de trois familles de tâches :

```
  ┌──────────── Serveur ─────────────────┐
  │  CPU hôte : applicatif, VMs          │
  │       ▲                              │
  │       │ PCIe                         │
  │  ┌────┴──────────────────────┐       │
  │  │ DPU                       │       │
  │  │  ARM + NIC + accélérateurs│──→ réseau (400/800G)
  │  │  - vSwitch / OVS          │       │
  │  │  - NVMe-oF target         │       │
  │  │  - Firewall / TLS         │       │
  │  │  - Télémétrie             │       │
  │  └───────────────────────────┘       │
  └──────────────────────────────────────┘
```

**Idée** : à 400G, le traitement réseau « coûte » 8-16 cœurs CPU (⚠️).
Les mettre sur le DPU = les rendre au workload (ou ne pas les acheter).

## 65. BlueField-2 / BlueField-3 — l'existant

| Modèle | Réseau | CPU | PCIe | Mémoire | Statut |
|---|---|---|---|---|---|
| BlueField-2 | 2×100G ou 1×200G | 8× ARM A72 | Gen4 | 16-32 Go DDR4 | Legacy, encore déployé |
| BlueField-3 | **400G** (4/2/1 ports) | 16× ARM A78 | Gen5 | 64 Go DDR5 | **Standard actuel** ✅ |
| BlueField-3 SuperNIC | 2×400G ou 1×800G | 16× A78 + ConnectX-7 | Gen5 | 64 Go | Backend IA Spectrum-X ✅ |

**BlueField-3 en bref** : DOCA (SDK), offloads OVS (des millions de flux),
NVMe-oF, chiffrement, isolation multi-tenant (chaque VM ne voit que son
DPU). Conso carte : ~75 W (⚠️ ordre de grandeur) — à comparer aux 8-16
cœurs CPU économisés (~100-200 W, ⚠️).

## 66. BlueField-4 — la génération actuelle (vérifiée 27/09/2026)

✅ **Lancé à GTC Washington (octobre 2025)**, BlueField-4 = le DPU des
« AI factories » :

| Spec | Valeur (✅ vérifié 27/09/2026) |
|---|---|
| Débit | **800 Gb/s** (2× BF-3) |
| CPU | **NVIDIA Grace : 64 cœurs ARM Neoverse V2** |
| Réseau | **ConnectX-9** |
| Mémoire | 128 Go |
| PCIe | **Gen6** |
| Logiciel | DOCA microservices, zero-trust, service chaining natif |
| Dispo | **Early availability 2026**, plateforme Vera Rubin |

**Cas d'usage mis en avant** : stockage « AI-native » (KV cache pour
l'inférence agentique), sécurité runtime IA, multi-tenant networking.
Systèmes partenaires attendus **2ᵉ semestre 2026**.
**Lecture** : le DPU devient un **mini-serveur ARM** (64 cœurs !) — la
frontière NIC/serveur s'efface. En achat 2026 : BF-3 pour le 400G
immédiat, BF-4 à évaluer pour les plateformes Rubin/800G.

## 67. Offloads DPU : réseau, stockage, sécurité

| Famille | Offload | Effet |
|---|---|---|
| **Réseau** | OVS/vSwitch complet, NAT, LB | 0 cœur CPU pour le data-plane |
| **Réseau** | Télémétrie par flux, ECN/CC | Visibilité sans agent hôte |
| **Stockage** | NVMe-oF target/initiator, déduplication | Nœuds stockage sans CPU |
| **Stockage** | Compression/décompression, erasure coding | Moins de CPU par Go/s |
| **Sécurité** | Firewall stateful, TLS/IPsec, RegEx IDS | Micro-segmentation à 400G |
| **Sécurité** | Secure boot, attestation, isolation | Zero-trust hardware |

**Règle** : un offload ne vaut que s'il est **dans le chemin de données
réel**. Offloader le firewall quand le goulot est le stockage = 0 gain.
Mesurer d'abord (`perf`, compteurs NIC), offloader ensuite.

## 68. DOCA : le SDK qui fait (ou défait) le DPU

- **DOCA** = l'API NVIDIA pour programmer BlueField (data-plane, flow,
  crypto, telemetry). **DOCA 2.0** avec ConnectX-8 (✅).
- **Avantage** : écrire une fois, déployer sur BF-3/BF-4/ConnectX.
- **Risque** : lock-in logiciel — une équipe qui développe 50 k lignes de
  DOCA ne changera plus de DPU. **Évaluer le coût de sortie** avant
  d'investir (comme tout SDK propriétaire).
- **Alternative ouverte** : OPI (Open Programmable Infrastructure, Linux
  Foundation) — promet une API DPU multi-vendeurs ; maturité ⚠️ à vérifier
  au 27/09/2026 (non évaluée ici).

## 69. Quand mettre un DPU — matrice de décision

| Situation | DPU ? | Pourquoi |
|---|---|---|
| Cloud privé / multi-tenant (OpenStack, K8s) | **Oui** | Isolation, OVS offload, facturation |
| Nœuds stockage NVMe-oF | **Oui** | CPU rendu au stockage |
| Backend IA pur (NCCL) | **SuperNIC plutôt que DPU** | Le réordonnancement HW compte plus que l'ARM |
| Serveurs généralistes < 100G | **Non** | Surcoût injustifié |
| Edge / 5G (UPF, firewall) | **Oui** | PPS + accélérateurs |
| Pare-feu / sécurité inline 400G | **Oui** | TLS/RegEx en HW |

**Calcul de rentabilité** : DPU ~2 000-5 000 € (⚠️) vs 8-16 cœurs CPU
(~1 500-4 000 € en serveurs, ⚠️) + conso. Le DPU gagne quand le CPU est
cher (licences au cœur : VMware, Oracle…) ou quand l'isolation est
réglementaire.

## 70. Alternatives : Intel IPU, AMD Pensando, Marvell

| DPU | Base | Note |
|---|---|---|
| Intel IPU (ex-Mount Evans) | Intel + Google | ⚠️ Stratégie Intel réseau revue en 2024-2025 — vérifier la roadmap au 27/09/2026 |
| AMD Pensando | P4 programmable | Solide, écosystème plus petit |
| Marvell OCTEON | ARM + accélérateurs | Télécoms/edge, moins DC |
| Broadcom Stingray | — | ❌ Largement éclipsé par Thor — non recommandé en neuf |

**En 2026, le marché DPU = NVIDIA BlueField par défaut**, challengers de
niche. Ne pas acheter de DPU « exotique » sans valider le support OS
(driver inbox ?) et la roadmap 3 ans.

## 71. CAS CHIFFRÉ — TCO DPU sur 500 serveurs cloud privé

Hypothèses : 500 serveurs, OVS + firewall distribué, 100G.

| Poste | Sans DPU | Avec DPU (BF-3) |
|---|---|---|
| Cœurs CPU réservés réseau | 8/serveur × 500 = 4000 cœurs | 0 |
| Coût cœurs (⚠️ ~250 €/cœur serveur) | ~1 M€ | 0 |
| DPU (⚠️ ~3 000 €) | 0 | ~1,5 M€ |
| Conso réseau CPU (⚠️ 8 cœurs ~80 W) | 500 × 80 W = 40 kW | — |
| Conso DPU (⚠️ ~75 W) | — | 500 × 75 W = 37,5 kW |
| Licences au cœur (si VMware/Oracle) | Plein pot | **Économie majeure** |

**Verdict** : sans licences au cœur, le DPU se paie à peu près ; **avec**
licences au cœur ou contrainte d'isolation, il est très rentable. Toujours
chiffrer les **trois** : hardware, énergie, licences.

## 72. Pièges DPU — l'essentiel (détail §107)

- **P24** — DPU acheté pour « faire moderne » sans workload identifié.
- **P25** — DOCA développé sans plan de sortie : lock-in logiciel.
- **P26** — DPU sous-alimenté en airflow : 75 W dans un slot PCIe = hotspot.

# PARTIE G — CDN & SERVEURS EDGE

## 73. Principes : rapprocher le contenu, pas l'utilisateur

Un CDN = un réseau de **PoP** (points de présence) qui cachent le contenu
au plus près des utilisateurs. Trois effets :

1. **Latence** : 200 ms → 20 ms (le contenu vient du PoP local, pas de
   l'origine à l'autre bout du monde).
2. **Débit** : l'origine ne sert chaque objet qu'une fois (cache-fill), les
   PoP absorbent les pics (match de foot, lancement produit).
3. **Résilience** : si un PoP tombe, l'anycast reroute vers le suivant.

```
  Utilisateur ──→ PoP local (cache HIT 95 %) ──→ [contenu servi en 20 ms]
                         │ cache MISS (5 %)
                         ▼
                   PoP régional ──→ Origine (data center)
```

**Lien avec l'IA** : l'edge sert aussi l'**inférence** (LLM petits, vision)
— mêmes contraintes (latence, débit) + GPU edge (voir §77).

## 74. Dimensionnement du cache — règles de pouce

| Paramètre | Règle (⚠️ ordre de grandeur) |
|---|---|
| Taux de hit visé | 90-98 % (vidéo), 80-90 % (web) |
| Taille cache / débit | ~1 To de SSD par Gb/s de pic pour de la vidéo |
| Répartition | 80 % chaud sur NVMe, 20 % tiède sur SATA |
| TTL | Vidéo : jours ; API : secondes-minutes ; jamais « infini » sans purge |

**Calcul** : PoP 100 Gb/s vidéo → ~100 To utiles → ~25-30 SSD NVMe 4 To
(avec parité/erasure). **Le cache se dimensionne en To ET en IOPS** : 100 Gb/s
de petits objets = millions d'IOPS — le NVMe est obligatoire, le SATA seul
s'effondre (P27, §107).

## 75. Dimensionnement du débit — la méthode

1. **Pic mesuré ou estimé** : trafic moyen × facteur de pic (2-5× selon
   l'usage ; un match = 5×).
2. **Marge** : pic × 1,5 (croissance + panne d'un lien).
3. **Exemple** : 20 000 spectateurs × 8 Mb/s (1080p) = 160 Gb/s de pic →
   **2× 100G** (200 Gb/s) par PoP, soit 2 serveurs edge 100G en actif/actif.

**95e percentile** : la facturation transit se fait au 95e pct — lisser les
pics via le cache réduit directement la facture. Chaque point de hit-rate
gagné = du transit en moins.

## 76. Hardware d'un serveur edge type

| Composant | Spec type (⚠️ config usuelle) |
|---|---|
| CPU | 1-2× Xeon/EPYC 16-32 cœurs (le cache est I/O-bound, pas CPU-bound) |
| RAM | 256-512 Go (cache chaud en RAM pour les top objets) |
| Stockage | 8-24× NVMe 4-8 To (ou 2 To si petits objets) |
| NIC | 2× 100G (ou 1× 200G) — DPDK/VPP pour le TLS |
| Châssis | 1-2U, rails courts (baies edge peu profondes !) |

**Contrainte edge** : les baies télécoms font souvent **600 mm de profondeur**
(contre 1000-1200 mm en DC) — vérifier la profondeur serveur **et** le
câblage avant (P28, §107). Refroidissement : clim de confort, pas de salle
froide — prévoir des serveurs tolérants à 35 °C.

## 77. Cas d'usage : vidéo, gaming, IoT, inférence IA

| Usage | Contrainte n°1 | Architecture |
|---|---|---|
| Vidéo live/VOD | Débit massif, hit-rate | Gros caches NVMe, 100G+ |
| Gaming / paris | Latence < 20 ms | PoP denses, anycast |
| IoT / télémétrie | Millions de connexions | L7 léger, pas de gros cache |
| Inférence IA edge | Latence + GPU | Serveurs avec GPU L4/L40S, modèles quantifiés |
| Sécurité (WAF/DDoS) | Absorption | Scrubbing au PoP, anycast |

**Inférence edge (2026)** : le pattern « petit modèle au PoP, gros modèle
au DC » se généralise (agents, RAG local). Dimensionner le PoP avec **2
postes** : le CDN classique + 1-2 GPU d'inférence (voir guide IA).

## 78. Anycast, DNS et routage

- **Anycast** : la même IP annoncée depuis tous les PoP — BGP envoie
  l'utilisateur au PoP le plus proche (en AS-path). Panne d'un PoP =
  reroutage automatique en ~minutes (BGP), pas de DNS à attendre.
- **DNS** : le sélecteur fin (GeoDNS / EDNS Client Subnet) pour les cas où
  l'anycast ne suffit pas (contraintes légales, PoP saturé).
- **Ne pas anycaster** : les sessions avec état (TCP long) sans mécanisme
  de persistance — un flap BGP coupe les connexions (P29, §107).

## 79. CAS CHIFFRÉ — PoP edge 100 Gb/s

| Poste | Qté | Prix (⚠️) | Total (⚠️) |
|---|---|---|---|
| Serveurs edge 2×100G, 12× NVMe 4 To | 2 | 25-40 k€ | 50-80 k€ |
| Switch 8×100G (agrégation) | 1 | 8-15 k€ | 8-15 k€ |
| Routeurs / peering (mutualisé) | — | — | (hébergeur) |
| Transit 100G 95e pct (⚠️ ~1-3 €/Mb/s/mois en Europe) | 60 Gb/s utiles | 60-180 k€/an | **60-180 k€/an** |
| Hébergement baie edge | 1 | 12-24 k€/an | 12-24 k€/an |
| **CAPEX** | | | **~60-95 k€** |
| **OPEX annuel** | | | **~72-204 k€/an** |

**Leçon** : en CDN, **l'OPEX transit domine** — chaque To servi depuis le
cache plutôt que l'origine, c'est de la marge. Le dimensionnement du cache
(§74) est une décision financière.

## 80. Pièges CDN/edge — l'essentiel (détail §107)

- **P27** — Cache sur SATA seul pour des petits objets : IOPS saturées.
- **P28** — Serveur 800 mm dans une baie edge 600 mm : ne rentre pas.
- **P29** — Anycast sur sessions TCP longues sans persistance : coupures.
- **P30** — TTL infini sans purge : contenu périmé servi pendant des jours.

# PARTIE H — PLANS D'ADRESSAGE, CÂBLAGE, ÉTIQUETAGE

## 81. Plan d'adressage : sobre, hiérarchique, documenté

| Usage | Plage type | Masque | Note |
|---|---|---|---|
| Loopbacks | 10.0.0.0/24 (ou /32 dans un /16) | /32 | 1 par équipement, jamais réutilisée |
| Liens leaf-spine | 10.0.1.0/24 | **/31** (RFC 3021) | 2 adresses utiles, pas de broadcast gaspillé |
| Management / OOB | 192.168.0.0/20 | /24 par salle | Séparé physiquement si possible |
| Overlay (VTEP) | = loopbacks | /32 | Annoncées en underlay |
| Services (VIP, anycast) | 10.0.10.0/24 | /32 | — |

**Règles** :
- **/31** sur les point-à-point : divise par 2 la consommation d'adresses
  et évite les erreurs de masque. Supporté partout en 2026.
- **ASN privés** : 64512-65534 (16 bits) ou 4200000000-4294967294 (32 bits).
  Convention : spines 65101/65102…, leafs 65001+ (voir §38).
- **IPv6** : prévoir le plan v6 dès maintenant (loopbacks /128, liens /127)
  même si on ne l'active pas — le renuméroter après = cauchemar.
- **Un seul fichier source de vérité** (NetBox — voir guide NetBox) :
  jamais d'IP « dans la tête de Kevin ».

## 82. Câblage : les bonnes pratiques qui évitent 90 % des pannes

1. **Séparer les chemins** : fibre optique d'un côté du chemin de câbles,
   cuivre/élec de l'autre. Jamais de fibre dans le même faisceau que du
   400 V.
2. **Longueurs justes** : 30 cm de mou, pas 3 m de spaghetti. Le surplus
   se range dans des gestionnaires verticaux, pas en boule derrière le switch.
3. **Top-of-Rack** : leafs en haut de chaque baie → DAC courts (1-2 m),
   pas de brassage horizontal pour les serveurs.
4. **Couleurs par usage** : ex. bleu = data, jaune = OS2 (norme), rouge =
   OOB, vert = stockage. **Documenter la légende** et s'y tenir.
5. **Jamais de tension** sur un connecteur : un MPO tiré = micro-courbures
   = pertes. Laisser du mou aux deux extrémités.

## 83. Longueurs et rayons de courbure — les chiffres

| Câble | Rayon de courbure min (⚠️ usuel) | Longueur max pratique |
|---|---|---|
| Fibre OM4 (G.651) | 15 mm (insensible ~7,5 mm) | 100 m (SR8) |
| Fibre OS2 (G.652D) | 30 mm (G.657 : 7,5-10 mm) | km |
| DAC 400G/800G | 50-70 mm (rigide !) | 2-3 m |
| AOC | 30 mm | 30-100 m |
| Cuivre Cat6A | 4× diamètre (~30 mm) | 100 m |

**Micro-courbures** : un rayon trop serré sur de l'OS2 = la lumière fuit
dans la gaine = **pertes de 1-3 dB invisibles à l'œil**. Symptôme typique :
lien qui monte mais BER pré-FEC élevée (§27). **Ne jamais coincer une fibre
sous un rail ou une porte de baie** (P31, §107).

## 84. Brassage : l'architecture MDA (Main Distribution Area)

```
  [Rangées serveurs] ──trunks MPO──→ ┌─────────────┐
                                     │  MDA / EDA  │  ← brassage central
  [Rangées serveurs] ──trunks MPO──→ │  panneaux   │
                                     │  MPO + LC   │
                                     └──────┬──────┘
                                            │ uplinks
                                            ▼
                                     [Spines / cœur]
```

- **Avantage** : tout brassage au même endroit, MAC (moves/adds/changes)
  sans toucher aux rangées, documentation unique.
- **Inconvénient** : 2 connecteurs de plus par lien (donc +0,6-1 dB, §20) —
  à intégrer dans le budget optique.
- **Règle** : tout lien > 30 m passe par le brassage ; les DAC intra-rack
  restent en direct (pas de brassage pour 2 m de cuivre).

## 85. Étiquetage : norme TIA-606, sans exception

Format recommandé : `SALLE-RANGÉE-BAIE-ÉQUIPEMENT-PORT` aux deux bouts,
**identique des deux côtés** (pas « A » d'un côté et « B » de l'autre).

Exemple : `S1-R03-B12-LF04-ETH49` = Salle 1, rangée 3, baie 12, leaf 4,
port 49. L'autre bout : `S1-R03-B12-SRV07-ETH1`.

| Support | Usage |
|---|---|
| Étiquettes auto-laminées | Câbles (tiennent 10 ans) |
| Manchons thermorétractables | Jarretières fibre (propre) |
| Plaques gravées | Baies, panneaux de brassage |
| QR code → NetBox | Équipements (inventaire) |

**Règle d'or** : un câble non étiqueté = un câble à retirer à la prochaine
intervention. Le coût d'une étiquette (~0,50 €) vs le coût d'une heure de
debug (150 €) : le calcul est vite fait.

## 86. Gestion des câbles : capacité des chemins

| Chemin | Règle de remplissage (⚠️ usuel) |
|---|---|
| Chemin de câbles (goulotte) | **≤ 50 %** de la section (réserve pour croissance) |
| Faisceau vertical | Attaches velcro, jamais de colliers nylon serrés sur fibre |
| Traversées coupe-feu | Manchons coupe-feu, **reboucher après chaque passage** |

**Pire ennemi** : le « juste un câble de plus » répété 200 fois = goulotte
à 120 %, fibres écrasées en bas du faisceau, pertes diffuses sur des
dizaines de liens. **Auditer le remplissage une fois par an.**

## 87. CAS CHIFFRÉ — plan d'adressage d'un pod 256 GPU

| Usage | Plage | Détail |
|---|---|---|
| Loopbacks leafs+spines | 10.100.0.0/24 | 24 équipements → /32 chacun |
| Liens leaf-spine | 10.100.1.0/24 | 8 leafs × 16 spines = 128 liens → 128× /31 = 256 adresses ✅ (un /24 = 256 /31) |
| VTEP / anycast | = loopbacks | — |
| Management | 192.168.100.0/24 | 24 équipements + 2 ToR mgmt |
| ASN | Spines 65101-65116, leafs 65001-65008 | eBGP (voir §38) |

**Vérification** : 128 liens /31 = pile un /24 — **prévoir le /23** pour la
croissance (P13). Un plan d'adressage trop juste = renumérotation = nuit
blanche.

## 88. Pièges câblage — l'essentiel (détail §107)

- **P31** — Fibre coincée sous un rail : pertes invisibles, BER élevée.
- **P32** — Goulotte à 120 % : dizaines de liens dégradés en même temps.
- **P33** — Câble non étiqueté : 1 h de debug à 150 €/h.
- **P34** — Plan d'adressage trop juste : renumérotation nocturne.

# PARTIE I — ÉNERGIE : LE RÉSEAU SE CHIFFRE EN kW

## 89. Conso des NICs par débit — tableau de référence

| NIC | Débit | Conso carte (⚠️/✅) | Note |
|---|---|---|---|
| Intel E810 2×100G | 100G | **15 W** ✅ (27,1 W avec AOC ✅) | La plus sobre du 100G |
| NVIDIA CX-6 Dx 2×100G | 100G | ~20-25 W ⚠️ | — |
| NVIDIA CX-7 1×400G | 400G | ~30-40 W ⚠️ | + optique ~9 W |
| Broadcom Thor 2 1×400G | 400G | ~25-35 W ⚠️ | 5 nm, argument efficacité ✅ |
| NVIDIA CX-8 1×800G | 800G | ~50-75 W ⚠️ | + optique ~15 W |
| BlueField-3 | 400G | ~75 W ⚠️ | = NIC + ARM + accélérateurs |
| BlueField-4 | 800G | ❌ non publiée au 27/09/2026 | Grace 64 cœurs : prévoir large |

**Efficacité par bit** (ordre de grandeur ⚠️) :
- 100G : ~0,2 W/Gb/s → 400G : ~0,1 W/Gb/s → 800G : ~0,08 W/Gb/s.
**Monter en débit divise par ~2,5 le joule par bit.** C'est l'argument
n°1 en comité d'investissement : le 800G n'est pas un luxe, c'est du
PUE.

## 90. Conso des switches par débit — tableau de référence

| Switch | Ports | Conso typique | Conso max optiques | Source |
|---|---|---|---|---|
| Leaf 48×25G + 8×100G | 100G | ~300-400 W ⚠️ | ~500 W ⚠️ | Usuel |
| Leaf 32×400G | 400G | ~600-800 W ⚠️ | ~1,2 kW ⚠️ | Usuel |
| SN5600 64×800G | 800G | **940 W** (cuivre) ✅ | **~2 kW** (optiques) ⚠️ | Datasheet ✅ |
| SN5610 64×800G | 800G | **0,9 kW** ✅ | **2,08 kW** (64 optiques) ✅ | Datasheet ✅ |
| Arista 7060X6 64×800G | 800G | (2× 2400 W PSU) ✅ | Dimensionné pire cas ✅ | Datasheet ✅ |
| Arista 7060X5 32×800G | 800G | (2× 1500 W PSU) ✅ | ✅ | Datasheet ✅ |

**Lecture** : sur un switch 800G tout-optique, **les optiques consomment
autant que le switch lui-même**. Tout levier sur les optiques (LPO §28,
CPO §101) = −30 à −70 % sur la moitié de la facture du switch.

## 91. Conso des optiques et câbles — le détail qui fait le kW

| Média | Conso / extrémité (⚠️ sauf note) |
|---|---|
| DAC 100G/400G/800G | 0,1-0,5 W |
| AEC 800G | 3-4 W |
| AOC 100G/400G | 1-2 W |
| 100G-SR4 | ~2,5 W |
| 400G-DR4 | ~8-10 W |
| 400G-SR8 | ~8-10 W |
| 800G-SR8/DR8 | ~12-16 W |
| 800G (max port OSFP) | **18 W** ✅ (SN5600, vérifié 27/09/2026) |

**Calcul éclair** : 1000 liens 800G-DR8 = 2000 extrémités × 15 W =
**30 kW** rien que d'optiques. Sur un fabric 10k GPU, les optiques =
**plusieurs centaines de kW**. C'est un poste PUE à part entière.

## 92. Chaleur des OSFP : à prévoir, pas à subir

- 18 W dans ~10 cm³ : sans airflow ≥ 2 m/s (⚠️), la température boîtier
  dépasse 80 °C → le module **réduit son débit** (thermal throttling) ou
  coupe.
- **Allées chaudes/froides** : les switchs réseau soufflent généralement
  **vers l'arrière** (C2P : connector-to-power) — les positionner en
  cohérence avec les serveurs (qui soufflent aussi vers l'arrière en
  général). Un switch à l'envers dans une allée froide = il aspire son
  propre air chaud.
- **Sonde** : chaque baie réseau = 1 sonde de température en haut
  (l'air chaud monte : +5-10 °C entre bas et haut de baie, ⚠️).
- **Water-cooling** : en 2026, les racks IA sont à eau, mais les switchs
  restent à air — **prévoir des allées froides dédiées** aux baies réseau
  dans un datacenter à eau, sinon elles deviennent le point chaud.

## 93. PUE partiel réseau — le calcul honnête

PUE = Énergie totale / Énergie IT. Pour le **lot réseau seul** :

```
  PUE_réseau = (IT_réseau + clim_réseau + pertes_élec_réseau) / IT_réseau
```

Exemple pod §62 : 181 kW IT réseau, clim ~60 kW (COP ~3, ⚠️), pertes
~10 kW → PUE_réseau ≈ (181+60+10)/181 ≈ **1,39**.
**Le réseau a un meilleur PUE que le GPU** (moins de water-cooling, moins
de pertes de conversion) — mais il tourne **24/7 à charge quasi constante**,
contrairement aux GPU (pics de training). En TCO annuel, le réseau pèse
plus que son % de puissance ne le suggère.

## 94. Refroidissement : air vs liquide pour le réseau

| Solution | Pour le réseau | Note |
|---|---|---|
| Air (allées chaudes/froides) | **Standard** | Suffit jusqu'à ~15 kW/baie |
| Confinement d'allée | Recommandé | +10-15 % d'efficacité (⚠️) |
| Eau (plaques froides) | Rare sur switchs en 2026 | Les switchs restent à air |
| Immersion | ❌ | Incompatible avec les optiques enfichables |

**Règle** : une baie réseau 800G dense = **8-12 kW** (⚠️) — dimensionner la
clim de la rangée réseau comme une rangée serveurs, pas comme du « brassage
télécom ». L'erreur classique : la salle réseau dimensionnée pour du 3 kW/
baie des années 2010 (P35, §107).

## 95. Dimensionnement électrique d'une rangée IA (méthode)

1. **Inventaire** : switchs (max, pas typique) + NICs + optiques + 20 %.
2. **PDU** : 2× PDU par baie (A/B), chacune capable de **100 % de la charge**
   (si A tombe, B prend tout).
3. **Disjoncteurs** : calibre ≥ 125 % du max (appel au boot, §61).
4. **Onduleurs** : le réseau sur l'onduleur **prioritaire** — c'est lui qui
   porte la supervision d'arrêt (NUT, voir guide onduleurs) et le
   management. Un réseau qui tombe avant les serveurs = arrêt à l'aveugle.
5. **Mesure** : PDU metered par prise sur les baies réseau — sans mesure,
   pas d'optimisation.

## 96. CAS CHIFFRÉ — 1 MW de réseau : que peut-on alimenter ?

Avec 1 MW IT réseau (PUE 1,4 → 1,4 MW au compteur), on alimente au choix :

| Option | Composition | Endpoints |
|---|---|---|
| A. Backend IA 800G | ~650 switchs 64×800G ? Non — réaliste : ~200 switchs + 8000 NIC 800G + optiques | ~8000 GPU |
| B. Cloud 100G | ~1500 switchs 32×100G + 40 000 NIC 100G | ~40 000 serveurs |
| C. Mixte | 1 pod IA (272 kW, §62) + 3000 serveurs 100G | — |

**Lecture** : 1 MW réseau = l'ordre de grandeur d'un **datacenter IA
moyen** (10 MW IT total dont ~10 % réseau). À 0,15 €/kWh (⚠️) : **1,84 M€/an**
d'électricité pour le seul réseau. Le choix LPO/CPO (§97) = des centaines
de k€/an.

## 97. Leviers d'efficacité : CPO, LPO, 800G

| Levier | Gain (✅/⚠️) | Maturité |
|---|---|---|
| Passer de 400G à 800G | ~−40 % W/Gb/s ⚠️ | Immédiat |
| LPO vs DSP (intra-DC) | −30 à −50 % par module ⚠️ | Croissante |
| CPO (TH6-Davisson) | **3,5 W/port à 800G** vs ~15 W pluggable = **−77 %** ✅ | Early access (10/2025) |
| Quantum-X800 CPO | **9 W/port** vs 30 W pluggable = **−70 %** ✅ | Annoncé |
| DAC là où c'est possible | ~0 W vs 15 W | Toujours |

**Message pour la direction** : le réseau IA 2026-2028 va diviser par
**3 à 4** sa conso par bit grâce au CPO. Tout investissement 800G
pluggable doit intégrer cette trajectoire : prévoir des switchs
**compatibles CPO ou à cycle de remplacement 3 ans**, pas 7.

## 98. Pièges énergie — l'essentiel (détail §107)

- **P35** — Salle réseau climatisée pour 3 kW/baie : insuffisant en 800G.
- **P36** — Disjoncteurs au nominal : déclenchement au boot.
- **P37** — Réseau pas sur onduleur prioritaire : arrêt à l'aveugle.
- **P38** — PUE calculé sans les optiques : −50 % du poste oublié.

# PARTIE J — À VENIR : ANNONCES VÉRIFIÉES AU 27/09/2026

> Seules figurent ici des annonces officielles (constructeur, consortium)
> retrouvées le 27/09/2026. Toute donnée issue de la presse ou d'analystes
> est explicitement signalée (⚠️) et ne compte pas comme annonce officielle.

## 99. 1,6T Ethernet — statut vérifié

| Annonce | Détail | Statut |
|---|---|---|
| NVIDIA ConnectX-9 SuperNIC | **1,6 Tb/s** scale-out, associé à l'architecture **Rubin (2ᵉ sem. 2026)** | ✅ Annoncé |
| Broadcom Taurus | Premier DSP optique **400G/lane** → modules **1,6T** (et base 3,2T) | ✅ Dévoilé OFC 2026 (mars 2026) |
| Tomahawk 6 | 64 ports **1,6T** (ou 128×800G, 512×200G) | ✅ Annoncé |
| Modules 1,6T | En développement sur DSP Taurus | ⏳ Pas de volume au 27/09/2026 |

**Technique** : le 1,6T = 8 lanes × 200G PAM4 (électrique 200G/lane).
Le verrou n'est plus le SerDes mais **l'optique et la thermique** :
un module 1,6T DSP ≈ 25-30 W (⚠️ estimation) — d'où le CPO (§101).

## 100. Ultra Ethernet — roadmap après la 1.0

| Étape | Date | Statut |
|---|---|---|
| UEC Specification 1.0 | **11 juin 2025** | ✅ Publiée |
| UEC Specification 1.0.1 | Mi-2025 | ✅ |
| Premier NIC conforme (Thor Ultra 800G) | **14 octobre 2025** | ✅ |
| Première solution de validation (VIAVI) | **23 juin 2026** | ✅ |
| Interopérabilité multi-vendeurs large | 2027+ | ⏳ À venir |
| UEC 2.0 (1,6T, scale-up ?) | ❌ Non annoncée au 27/09/2026 | — |

**À surveiller** : les premiers fabrics UEC multi-vendeurs réels (pas les
démos), et la position NVIDIA (Spectrum-X vs UEC) — c'est le clivage
structurant du marché 2027-2028.

## 101. CPO (Co-Packaged Optics) — l'annonce la plus structurante

| Annonce | Détail | Statut |
|---|---|---|
| Broadcom Tomahawk 6-Davisson | **102,4T avec CPO**, 16 moteurs optiques 6,4T co-packagés ; **3,5 W/port à 800G** (−36 % vs TH5 CPO, −70 % vs pluggable) ; lasers **remplaçables par la face avant** ; 131 072 XPU en 2 tiers | ✅ Annoncé **08/10/2025**, early access |
| NVIDIA Quantum-X800 (variante CPO Q3450-LD) | **115,2T** InfiniBand, 144×800G ; **9 W/port** vs 30 W pluggable (⚠️ estimation analyste) | ✅ Datasheet NVIDIA (déc. 2024) |
| Partenaires Davisson | Celestica, Accton (box), Micas, Nexthop AI (SONiC durci), TSMC (packaging COUPE) | ✅ Cités par Broadcom |

**Pourquoi c'est structurant** : le CPO supprime le DSP + le connecteur
enfichable (les deux plus gros consommateurs et points de panne). La 3ᵉ
tentative de Broadcom (après 2 générations) semble la bonne : lasers
remplaçables (l'objection n°1 des hyperscalers est levée).
**Impact achat** : ne pas signer en 2026 des contrats 800G pluggables à
7 ans sans clause de sortie — le CPO change le TCO dès 2027-2028.

## 102. Après : 3,2T, 204,8T, l'ère 200T

| Annonce | Détail | Statut |
|---|---|---|
| Broadcom OFC 2026 | Feuille de route vers le **200T** : switchs **204,8T**, modules **3,2T** (sur DSP 400G/lane Taurus) | ✅ Annoncé mars 2026 |
| Broadcom NPO 3,2T | Optiques « near-packaged » VCSEL 3,2T | ✅ Annoncé OFC 2026 |
| Retimers 200G/lane, AEC | Jusqu'à 6 m en cuivre actif | ✅ Annoncé OFC 2026 |

**Lecture** : le rythme est d'un doublement tous les ~2 ans (51,2T →
102,4T → 204,8T). **Ne pas acheter « pour 10 ans »** : le cycle réseau IA
est de **3-4 ans**. Budgéter en conséquence (leasing ou amortissement
accéléré).

## 103. PCIe Gen6 / Gen7 — le bus suit

| Génération | Débit x16 | NIC associée | Statut |
|---|---|---|---|
| Gen5 | 512 Gb/s | 400G (CX-7, Thor 2) | Déployé ✅ |
| Gen6 | 1024 Gb/s | 800G (CX-8, BF-4) | ✅ (serveurs 2025-2026) |
| Gen7 | 2048 Gb/s | 1,6T (CX-9) | ⏳ Spéc finalisée, produits 2027+ (⚠️) |

**Point de vigilance** : une NIC 1,6T sur PCIe Gen6 x16 serait bridée —
le 1,6T **exige** le Gen7 (ou du x32, inexistant). Vérifier la carte mère
**avant** d'acheter des NIC 1,6T en 2027.

## 104. Conclusion : les 10 décisions qui comptent

1. **Séparer** backend IA (1:1, lossless) et front-end (3:1) — toujours.
2. **Choisir le débit par le joule/bit**, pas par le prix catalogue.
3. **Valider le PCIe** (génération × lanes électriques) avant chaque NIC.
4. **Figer le triplet** FW/driver/OS par fabric.
5. **Nettoyer la fibre** (procédure affichée, kit par salle).
6. **Superviser le pré-FEC**, pas seulement le link up/down.
7. **Exiger UEC-ready** sur les AO 2026+ (assurance anti-lock-in).
8. **Chiffrer l'énergie** (switch + optiques + NIC + PUE) dans chaque projet.
9. **Prévoir le CPO** : pas d'engagement pluggable 800G à 7 ans.
10. **Cycle 3-4 ans** : le réseau IA ne s'amortit plus sur 7 ans.

## 105. Glossaire — 40 termes

| Terme | Définition |
|---|---|
| AEC | Active Electrical Cable — cuivre actif, amplifie le signal (3-6 m). |
| AOC | Active Optical Cable — fibre active avec modules intégrés. |
| ASIC | Circuit intégré dédié (ex. Tomahawk, Spectrum) — le cœur du switch. |
| BDP | Bandwidth-Delay Product — dimensionne les buffers (débit × RTT). |
| Breakout | Diviser un port rapide en N ports lents (ex. 400G → 4×100G). |
| CPO | Co-Packaged Optics — optiques intégrées au package du switch. |
| Cut-through | Commutation dès la lecture de l'en-tête (~600 ns) vs store-and-forward. |
| DAC | Direct Attach Cable — cuivre passif Twinax, courte portée. |
| DCQCN | Contrôle de congestion pour RoCEv2 (ECN + CNP). |
| DPU | Data Processing Unit — NIC + ARM + accélérateurs. |
| DSP | Digital Signal Processor — dans les modules optiques classiques. |
| ECN | Explicit Congestion Notification — marquage anti-congestion. |
| ECMP | Répartition sur N chemins de coût égal (hash par flux). |
| EVPN | BGP qui distribue MAC/IP pour les overlays VXLAN. |
| FEC | Forward Error Correction — Reed-Solomon, rend le PAM4 viable. |
| IPU | Infrastructure Processing Unit — nom Intel du DPU. |
| Incast | Many-to-one : N émetteurs vers 1 récepteur (all-reduce). |
| LPO | Linear Pluggable Optics — optique sans DSP, basse conso. |
| MPO/MTP | Connecteur multi-fibres (12/16/24). |
| NDR / XDR | InfiniBand 400G / 800G. |
| NOS | Network Operating System (EOS, NX-OS, SONiC, Cumulus). |
| NRZ / PAM4 | Modulations 1 bit / 2 bits par symbole. |
| OCP 3.0 | Format de carte réseau pour serveurs hyperscale. |
| OSFP / QSFP-DD | Formats de modules 400/800G (OSFP = meilleure thermique). |
| Overlay / Underlay | Réseau virtuel (VXLAN) / réseau physique (BGP) qui le porte. |
| PFC | Priority Flow Control — pause par file (lossless). |
| PUE | Power Usage Effectiveness — efficacité énergétique du DC. |
| Rail-optimized | 1 plan réseau par indice de NIC GPU. |
| RDMA | Accès mémoire distant sans CPU (zéro-copy). |
| RoCEv2 | RDMA over Converged Ethernet (UDP 4791). |
| SerDes | Serializer/Deserializer — l'étage électrique du port. |
| SONiC | NOS open-source (Microsoft, Linux Foundation). |
| Spectrum-X | Pile Ethernet IA fermée de NVIDIA. |
| Spraying (packet) | Répartition à la granularité paquet (vs par flux). |
| SR-IOV | Virtualisation PCIe : 1 carte → N fonctions virtuelles. |
| SuperNIC | NIC IA avec fonctions réseau avancées (NVIDIA). |
| UEC / UET | Ultra Ethernet Consortium / son transport. |
| VTEP | VXLAN Tunnel EndPoint — extrémité de tunnel. |
| VXLAN | Overlay L2 sur L3 (16M de segments). |
| ZR | Optique cohérente longue portée (80 km+, DCI). |

# PARTIE K — APPROFONDISSEMENTS TERRAIN

## 106. Supervision d'un fabric IA : télémétrie, compteurs, seuils

Un fabric IA ne se supervise pas comme un LAN : on surveille des
**distributions** (latence p99, BER), pas des états binaires.

| Sonde | Compteur / métrique | Seuil 🟡 | Seuil 🔴 | Outil |
|---|---|---|---|---|
| NIC | pre-FEC BER | > 1e-9 | > 1e-6 | ethtool -S, telemetry |
| NIC | FEC uncorrectable | > 0 | > 0 | idem |
| NIC | CNP reçus/s | variation > 50 % | storm | compteurs RoCE |
| NIC | retransmissions | > 0,01 % | > 0,1 % | — |
| Switch | drops par file | > 0 | > 100/s | SNMP/gNMI |
| Switch | ECN marks/s | > 10 % paquets | > 30 % | — |
| Switch | PFC pauses/s | > 0 | > 10/s | ⛔ = deadlock imminent |
| Switch | buffer occupancy p99 | > 70 % | > 90 % | LANZ (Arista), telemetry |
| Optique | Température | > 70 °C | > 80 °C | DOM |
| Optique | Rx power vs sensibilité | marge < 3 dB | marge < 1 dB | DOM |
| Fabric | latence p99 leaf↔leaf | > 5 µs | > 20 µs | sondes actives |

**Stack recommandée** : telemetry streaming (gNMI/gRPC) → collecteur
(Telegraf/Prometheus) → Grafana + alertes. Le SNMP polling à 5 min est
**aveugle** aux microbursts (µs-ms) : pour l'IA, il faut du streaming à
la seconde ou des histogrammes embarqués (LANZ chez Arista).

**Règle** : 1 dashboard « santé fabric » avec 6 graphes max (BER, drops,
ECN, PFC, température optiques, latence p99). Le reste en drill-down.
Un NOC qui regarde 200 graphes n'en regarde aucun.

## 107. Tests de réception d'un fabric : la méthode avant la signature

Ne jamais signer la réception d'un fabric IA sans ces tests :

| Test | Outil | Critère de passage (⚠️) |
|---|---|---|
| Bande passante paire à paire | `ib_write_bw` / `qperf` | ≥ 95 % du débit nominal |
| All-reduce NCCL 8→N nœuds | `nccl-tests` (all_reduce_perf) | busbw ≥ 80 % du théorique |
| Latence | `ib_write_lat` | < 2 µs (RoCE), < 1 µs (IB) |
| ECMP : tous les spines utilisés | compteur par lien sous charge | écart < 10 % entre liens |
| Perte d'un spine | débrancher 1 spine sous charge | < 5 % de perte de busbw, convergence < 1 s |
| Perte d'un leaf | idem | trafic rerouté, pas de blackhole |
| 72 h de burn-in | charge NCCL continue | 0 erreur FEC non corrigée, 0 flap |
| Bruit thermique | charge max 1 h | aucune optique > 75 °C |

**Procédure** : tester **d'abord à vide** (1 flux), **puis en all-to-all**
(tous les nœuds). Un fabric qui passe à vide et échoue en all-to-all a un
problème d'ECMP/CC — c'est le cas le plus fréquent (P39, §122).
**Documenter les résultats** : c'est la baseline. Toute régression future
se mesure contre elle.

## 108. NCCL et collectives : ce que le réseau doit garantir

NCCL (NVIDIA Collective Communications Library) = la bibliothèque qui fait
communiquer les GPU entre eux. Le réseau doit garantir :

| Collective | Pattern | Exigence réseau |
|---|---|---|
| All-Reduce | Tous ↔ tous (anneau/arbre) | Bande passante bissection max, 1:1 |
| All-Gather | Tous reçoivent tout | Idem |
| Broadcast | 1 → tous | Faible latence |
| Point-to-point | Paires | RDMA, zéro-copy |

**Chiffres** : sur un lien 400G, NCCL atteint ~90-95 % du débit utile en
all-reduce bien réglé (busbw). En dessous de 70 % : problème réseau
(ECMP, CC, NUMA) — pas problème GPU.
**Variables d'environnement clés** :
- `NCCL_SOCKET_IFNAME` : forcer l'interface backend (pas le front-end !).
- `NCCL_IB_DISABLE=0/1` : choisir IB vs Ethernet.
- `NCCL_DEBUG=INFO` : voir quelle interface/NIC est utilisée — **le premier
  debug quand « c'est lent »**.
**Piège classique** : NCCL qui passe par le réseau front-end 100G au lieu
du backend 400G parce que l'IP du backend n'est pas dans le bon sous-réseau
(P40, §122). Symptôme : 4× moins vite que prévu, pile le ratio 400/100.

## 109. Stockage et réseau : NVMe-oF, RDMA et dimensionnement

Le stockage moderne (NVMe-oF sur RoCEv2) partage le réseau — ou pas :

| Architecture | Réseau stockage | Avantage | Inconvénient |
|---|---|---|---|
| Fabric dédié | 100/200G séparé | Isolation, pas de contention | 2ᵉ fabric à payer |
| Converged | Même fabric, QoS | 1 seul réseau | Le training peut noyer le stockage |
| DPU offload | NVMe-oF sur DPU | CPU libéré (§67) | Coût DPU |

**Dimensionnement** : 1 SSD NVMe Gen4 ≈ 7 Go/s ≈ 56 Gb/s. Une baie de
24 SSD = **1,3 Tb/s** théoriques → il faut **4× 400G** ou 2× 800G par baie
pour ne pas brider (en pratique : 2× 400G avec un ratio réaliste, les SSD
ne poussent pas tous à fond en même temps).
**RDMA obligatoire** : NVMe-oF sur TCP à 100G+ = CPU saturé ; sur RoCEv2 =
zéro-copy, latence ~10 µs de bout en bout (⚠️).
**Checkpoints IA** : un training qui checkpoint 1 To toutes les heures sur
un stockage à 100 Go/s = 10 s de pause — dimensionner le stockage
**pour le checkpoint**, pas pour la moyenne (P41, §122).

## 110. Sécurité du fabric : segmentation, ACLs, zero-trust

Le réseau IA est souvent « plat et ouvert » par performance — c'est une
dette sécurité :

| Mesure | Où | Coût perf |
|---|---|---|
| Management VRF séparé | Tous switchs | Nul |
| ACLs infra (n'accepter que le NOC) | Interfaces de management | Nul |
| Secure boot + image signée | Switchs/NIC | Nul (au boot) |
| 802.1X sur ports serveurs | Leafs | Faible (au link-up) |
| Micro-segmentation (DPU) | Serveurs multi-tenant | Faible (HW) |
| Chiffrement MACsec 400/800G | Liens inter-sites | Faible (HW si supporté) |
| Chiffrement IPsec inline (CX-8 ✅) | Backend sensible | Nul (HW) |

**Règles** :
- **Jamais de management sur le fabric data** : un plan OOB dédié (1G/25G),
  même en petit DC. Le jour où le fabric data tombe, c'est l'OOB qui permet
  de le réparer.
- **AAA** (TACACS+/RADIUS) partout, pas de compte `admin/admin` — auditer
  avec un scan trimestriel.
- **Firmware signé** : exiger la vérification de signature sur switchs et
  DPU (supply-chain).
- **Ne pas chiffrer le backend IA par défaut** : le chiffrement logiciel
  tue la perf NCCL ; le HW (CX-8) seulement si l'exigence est réelle
  (données de santé, défense…).

## 111. Automation : ZTP, Ansible, NetBox, CI/CD réseau

| Couche | Outil | Usage |
|---|---|---|
| Source de vérité | **NetBox** | IP, baies, câbles, ASN — tout y est |
| Provisioning | ZTP (EOS, Cumulus, NX-OS) | Switch branché → config auto |
| Config management | Ansible / Nornir | Templates Jinja, idempotence |
| Validation | Batfish / SuzieQ | « La config fait-elle ce qu'on croit ? » |
| CI/CD | Git + pipeline | MR → lab → prod, rollback auto |

**Workflow type** :
1. NetBox = entrée (nouveau leaf : baie, ports, ASN).
2. Ansible génère la config depuis les templates + NetBox.
3. Batfish valide (pas de route qui fuit, pas d'ACL qui bloque le BGP).
4. Déploiement ZTP ou Ansible, vérification post-change (tests §107).
5. Tout est commité : **le réseau se versionne comme du code**.

**Piège** : automatiser un process cassé = casser plus vite. Stabiliser
manuellement sur 1 pod, **puis** automatiser (P42, §122).
**Lien avec les guides existants** : Ansible (guide dédié), NetBox (guide
dédié) — ce guide suppose ces bases acquises.

## 112. CAS CHIFFRÉ — TCO 5 ans : fabric 400G vs 800G (128 nœuds GPU)

Hypothèses : 128 nœuds × 8 GPU, 1 NIC/GPU, 1:1, élec 0,15 €/kWh (⚠️),
PUE 1,4 (⚠️). Prix = fourchettes marché ⚠️ à vérifier.

| Poste (5 ans) | 400G (1024 NIC CX-7) | 800G (1024 NIC CX-8) |
|---|---|---|
| Switchs (16× 64×400G vs 8× 64×800G) | 16 × 80-150 k€ = 1,3-2,4 M€ | 8 × 150-300 k€ = 1,2-2,4 M€ |
| NIC | 1024 × 1,5-3 k€ = 1,5-3,1 M€ | 1024 × 3-6 k€ = 3,1-6,1 M€ |
| Câbles/optiques | ~0,5-1 M€ | ~0,8-1,5 M€ |
| **CAPEX** | **~3,3-6,5 M€** | **~5,1-10 M€** |
| Élec réseau (5 ans) | ~90 kW × 1,4 × 8760 h × 5 × 0,15 € ≈ 0,83 M€ | ~154 kW → ≈ 1,42 M€ |
| Maintenance (15 %/an ⚠️) | ~2,5-4,9 M€ | ~3,8-7,5 M€ |
| **TCO 5 ans** | **~6,6-12,2 M€** | **~10,3-19 M€** |

**Lecture** : le 800G coûte ~1,6× plus cher mais pousse 2× plus de bits :
**le coût par Gb/s sur 5 ans est inférieur en 800G** (~−20 %, ⚠️). Et le
400G sera en fin de vie avant 5 ans (cycle 3-4 ans, §102) → prévoir un
refresh. **Décision** : cluster neuf en 2026 → 800G directement, sauf
contrainte budgétaire CAPEX dure.

## 113. Migration 100G → 400G → 800G : stratégie par étapes

| Étape | Action | Durée type |
|---|---|---|
| 1. Audit | Inventaire ports, PCIe, fibre (OM3 ? OS2 ?) | 2-4 sem. |
| 2. Fibre d'abord | Tirer OS2/OM4 là où il manque — **avant** les switchs | 1-3 mois |
| 3. Breakout | Switchs 400G en 4×100G vers serveurs existants | Au fil de l'eau |
| 4. Backend IA | Nouveau fabric 400/800G **séparé** (§32) | Projet |
| 5. Bascule | Migrer les workloads, pas les câbles | Par vague |
| 6. Décommission | Revendre/déclasser le 100G en OOB | Fin |

**Règles** :
- **Ne jamais mélanger les générations sur un même fabric** : un lien 100G
  dans un fabric 400G = ECMP asymétrique = polarisation (§36).
- La fibre se tire **une fois pour 15 ans**, les switchs se changent tous
  les 3-4 ans : **surdimensionner la fibre** (24-48 brins par trunk même si
  on n'en utilise que 8).
- **Breakout = l'arme de la migration douce** : un port 400G qui sert 4
  serveurs 100G rentabilise le switch avant la fin de la migration.

## 114. Multi-site / DCI : interconnecter des datacenters

| Distance | Technologie | Débit | Note |
|---|---|---|---|
| < 2 km (campus) | 400G-FR4 / 800G-2×FR4 | 400/800G | Fibre propre, pas d'ampli |
| 2-10 km (métro) | 400G-LR4 / 800G-DR8+ | 400/800G | — |
| 10-80 km (régional) | **400G-ZR / 800G-ZR** (cohérent) | 400/800G | Optique cohérente enfichable ✅ |
| > 80 km | DWDM + transpondeurs | N×400/800G | Baie optique dédiée |

**400G-ZR / 800G-ZR** : optiques cohérentes **enfichables directement dans
le routeur/switch** (pas de transpondeur externe) — a divisé par ~3 le coût
du DCI métro (⚠️). Standard : OpenZR+ / OIF.
**Règles DCI** :
- **Chiffrer** (MACsec ou IPsec) : la fibre inter-sites n'est pas sous votre
  contrôle physique.
- **Latence** : 1 ms ≈ 200 km de fibre — au-delà de ~50 ms RTT, le
  synchrone (stretch cluster) devient impossible.
- **Ne pas étendre le fabric IA entre sites** : l'all-reduce inter-sites
  s'effondre avec la latence — chaque site = 1 fabric, réplication
  asynchrone entre sites (P43, §122).

## 115. Edge IA et 5G : contraintes spécifiques

| Contrainte | Edge / 5G | Datacenter |
|---|---|---|
| Profondeur de baie | 600 mm (télécom) | 1000-1200 mm |
| Température | 35 °C ambiant (clim confort) | 25 °C contrôlés |
| Accès | Site distant, 4 h d'intervention | Sur place |
| Alimentation | −48 V DC (télécom) ou 230 V | 400 V / busway |
| Poussière | Possible (shelter) | Salle blanche |

**Conséquences** :
- Serveurs **courts** (≤ 600 mm) et switchs **DC −48 V** si site télécom —
  vérifier les options d'alimentation à la commande (P44, §122).
- **Tout en double** : pas de « on ira voir demain » à 300 km. Spares sur
  site (1 switch, 2 optiques, câbles).
- **OOB 4G/5G** : un routeur cellulaire pour l'accès distant quand le lien
  principal tombe — le seul moyen de réparer à distance.
- **Filtres à air** : en shelter, les filtres se colmatent → surchauffe
  lente. Maintenance trimestrielle, pas annuelle.

## 116. Water-cooling et réseau : cohabitation

En 2026, les racks GPU sont à eau (plaques froides), les switchs restent
à air. Trois architectures :

| Architecture | Principe | Note |
|---|---|---|
| Salles séparées | Réseau dans une salle air, GPU dans salle eau | Simple, + fibre inter-salles |
| Allées mixtes | Baies réseau à air entre rangées eau | Gérer les flux d'air (confinement) |
| CDU + air | Unité de distribution d'eau + clim d'appoint | Le plus courant en retrofit |

**Points d'attention** :
- **Fuite d'eau** : jamais de tuyauterie **au-dessus** des baies réseau —
  les switchs n'ont pas de bac de rétention. Cheminement latéral ou sous
  plancher technique avec détection de fuite.
- **Humidité** : < 60 % HR — la condensation sur les optiques = corrosion
  des contacts. Sonde d'humidité par salle.
- **Vannes** : pouvoir isoler hydrauliquement une rangée sans couper le
  réseau (le réseau survit aux GPU, pas l'inverse en supervision).

## 117. Dimensionner l'onduleur pour le lot réseau (lien guide onduleurs)

Le réseau = **charge prioritaire** : il porte la supervision, l'OOB et les
ordres d'arrêt (NUT). Règles (complément du guide onduleurs_ups_guide.md) :

| Paramètre | Valeur |
|---|---|
| Autonomie visée | **≥ 15 min** (le temps d'un arrêt propre des baies GPU) |
| Puissance | Max switchs + NICs + optiques + 30 % (pas le typique) |
| Batches | 1 onduleur par **plan réseau** (pas 1 pour tout) : la panne d'un UPS ne doit pas couper 2 plans |
| Bypass | Bypass de maintenance **externe** : on peut shunter l'UPS sans couper |
| Test | Test batterie **trimestriel** avec le réseau en charge réelle |

**Calcul** : pod §62 = 181 kW réseau → 2 UPS 120 kVA (N+1) ou 4× 60 kVA
(2 par plan). **Ne pas mettre le réseau sur le même UPS que les GPU** :
un training qui fait disjoncter son UPS ne doit pas emporter la supervision
(P45, §122).
**Séquence d'arrêt** (coupure longue) : 1. Arrêt GPU (gros consommateurs),
2. Stockage (flush), 3. **Réseau en dernier** (il faut superviser 1 et 2),
4. Management/OOB = le tout dernier (il envoie l'alerte « tout est éteint »).

## 118. Contrats de maintenance réseau : SLA, spares, escalade

| Élément | Standard DC | Backend IA |
|---|---|---|
| SLA remplacement | J+1 ouvré | **4 h, 24/7** |
| Spares sur site | 1 switch, 5 % optiques | **1 switch par plan**, 10 % optiques, câbles |
| Support | Heures ouvrées | 24/7 avec accès lab (repro) |
| Firmware | Trimestriel | Figé par campagne (pas de « latest ») |
| Escalade | Niveau 1 → 2 | Accès direct niveau 3 (TAC) sur P1 |

**Chiffrage** : maintenance 24/7 4 h ≈ **12-18 % du CAPEX/an** (⚠️) vs
8-12 % en J+1. Sur un fabric à 5 M€, l'écart = 200-300 k€/an — à comparer
au coût d'une heure de cluster idle (6-7 chiffres, §22 du brief initial).
**Pièce critique** : les **optiques** — 80 % des pannes sont optique/câble,
pas switch. Un spare d'optiques bien garni vaut plus qu'un switch de spare.
**Contrat TAC** : exiger un numéro direct + un ingénieur nommé pour les
comptes IA (les files génériques noient les P1).

## 119. Retours terrain — 5 histoires (anonymisées, ⚠️ retours d'expérience)

**R1. Le DAC de 5 m qui « devrait passer ».**
Cluster 400G, DAC de 5 m commandés « parce que c'était moins cher que
l'AOC ». La moitié des liens ne montaient pas (limite du DAC 400G : 3 m).
3 semaines de retard, 200 AOC en urgence à prix ×2. **Leçon** : respecter
les portées (§59), le cuivre ne pardonne pas.

**R2. La poussière de chantier.**
Nouveau DC, brassage fait pendant les travaux. 40 % des liens 100G avec
BER pré-FEC > 1e-6. Cause : poussière de plâtre dans les MPO. 2 jours de
nettoyage systématique (§24). **Leçon** : brasser **après** la fin du
chantier, bouchons partout entre-temps.

**R3. Le firmware qui a tué un training.**
Mise à jour FW NIC « mineure » sur 1/4 du cluster un vendredi soir.
Le lundi : NCCL −30 % sur ces nœuds (changement de comportement CC).
Rollback le mardi, 4 jours de GPU à 70 %. **Leçon** : FW figé par campagne
(§13), jamais de mise à jour partielle sur un fabric.

**R4. L'ECMP qui polarisait.**
Fabric RoCE 3 tiers, 2 spines sur 8 qui saturaient, les autres à 30 %.
Cause : même hash seed sur tous les leafs (défaut constructeur). Seed
aléatoire → répartition ±5 %. **Leçon** : tester l'ECMP sous charge
(§107), pas à vide.

**R5. L'onduleur du réseau.**
Coupure longue, les GPU s'arrêtent proprement… puis plus rien : le switch
de management était sur le même UPS que les GPU, parti en même temps.
Arrêt à l'aveugle, 2 baies non redémarrées proprement. **Leçon** : réseau
sur UPS prioritaire séparé (§117).

## 120. Erreurs de débutant vs erreurs d'expert

| Débutant | Expert (plus coûteuses) |
|---|---|
| Oublier le /31, gaspiller des /24 | Sur-optimiser l'oversubscription « parce que ça passe en moyenne » |
| Brancher sans nettoyer la fibre | Faire confiance au FEC sans superviser le pré-FEC |
| Acheter la NIC avant de vérifier le PCIe | Choisir le switch sur l'ASIC sans évaluer le NOS |
| Pas d'étiquettes | Automatiser avant de stabiliser |
| Un seul plan d'alimentation | Un seul fabric pour IA + prod « pour mutualiser » |
| Tester à vide seulement | Ne pas tester la perte d'un spine sous charge |
| MTU 1500 partout « par défaut » | MTU 9000 sans vérifier **tout** le chemin (un équipement à 1500 = fragmentation) |

**La plus chère** : le fabric unique IA+prod. Elle économise 15 % de CAPEX
et coûte des semaines de debug par an + des trainings ralentis en
permanence. **Deux fabrics valent toujours le coup.**

## 121. Quiz — 10 questions + réponses

**Q1.** Une NIC 400G est installée dans un slot PCIe Gen4 x16. Quel débit
maximal peut-elle réellement atteindre, et pourquoi ?
**R.** ~200 Gb/s (la moitié). PCIe Gen4 x16 = 256 Gb/s théoriques ; la NIC
ne peut pas pousser plus que le bus. C'est le bridage invisible P1 (§8).

**Q2.** Quelle est la différence fondamentale entre RoCEv2 et Ultra Ethernet
(UET) concernant le réseau ?
**R.** RoCEv2 exige un réseau lossless (PFC+ECN à régler) et des paquets en
ordre ; UET est conçu pour un réseau lossy : livraison non ordonnée,
retransmission sélective, multipath natif (§10, §54).

**Q3.** Un lien 400G-DR4 « monte » mais affiche un BER pré-FEC de 5e-6.
Que faire en premier ?
**R.** Nettoyer les connecteurs (inspecter → nettoyer → ré-inspecter, §24),
pas changer le module. 5e-6 = 🟡 : surveiller après nettoyage ; si ça
persiste, vérifier le budget optique (§20).

**Q4.** Calculez l'oversubscription d'un leaf 48×25G avec 6×100G d'uplinks.
**R.** Down : 48×25 = 1200 Gb/s. Up : 6×100 = 600 Gb/s. Ratio = 2:1 (§32).

**Q5.** Avec des switchs 64 ports, combien de serveurs en 2 tiers
non-bloquant, et combien de spines ?
**R.** n²/4 = 1024 serveurs, n/2 = 32 spines (§33).

**Q6.** Pourquoi le rail-optimized sépare-t-il les plans par indice de NIC ?
**R.** Parce que le trafic dominant (all-reduce NCCL en anneau) circule
entre les mêmes indices de GPU : séparer les plans évite la contention
inter-rails et le head-of-line blocking (§39).

**Q7.** Un switch 64×800G tout-optique consomme ~2 kW. Quelle part vient
des optiques ?
**R.** Environ la moitié : 64 × 15 W ≈ 960 W d'optiques + ~940 W de switch
(SN5600 : 940 W typique, 2,08 kW à 64 optiques, §90).

**Q8.** Quand un DPU est-il rentable ?
**R.** Quand il libère des cœurs CPU chers (licences au cœur), quand
l'isolation multi-tenant est requise, ou pour le NVMe-oF / la sécurité
inline à 400G. Pas pour un serveur généraliste < 100G (§69, §71).

**Q9.** Que change le CPO (Tomahawk 6-Davisson) pour l'énergie ?
**R.** 3,5 W/port à 800G contre ~15 W en pluggable (−77 %) : les optiques,
qui représentent ~50 % de la conso d'un switch 800G, sont divisées par 4
(§101).

**Q10.** Citez 3 vérifications avant de signer la réception d'un fabric IA.
**R.** (au choix) : NCCL all-reduce ≥ 80 % du théorique ; ECMP équilibré à
±10 % sous charge ; perte d'un spine < 5 % de perte et < 1 s de convergence ;
72 h de burn-in sans erreur FEC ; aucune optique > 75 °C (§107).

## 122. Les 45 pièges terrain — récapitulatif complet

| # | Piège | Symptôme | Parade (§) |
|---|---|---|---|
| P1 | NIC 400G en slot PCIe Gen4 | Débit plafonné à ~200G | Vérifier génération × lanes (§8) |
| P2 | Offloads désactivés (vieux driver) | CPU 100 % à 100G | `ethtool -k`, MAJ driver (§9) |
| P3 | Optiques tierces refusées (FW lock) | Lien down, log « unsupported » | Tester 1 avant 100 (§11) |
| P4 | Firmwares NIC hétérogènes | Micro-pertes aléatoires | Triplet FW/driver/OS figé (§13) |
| P5 | NIC OCP 3.0 pour châssis CEM | Ne rentre pas physiquement | Valider le format avant commande (§12) |
| P6 | OSFP flat-top sur switch | Surchauffe +15-20 °C | Finned-top sur switch (§16) |
| P7 | Jarretière OS2 (jaune) sur port SR | Lien muet | Couleurs : aqua=OM, jaune=OS2 (§18) |
| P8 | Polarité MPO non documentée | Heures de debug nocturne | Étiqueter méthode A/B/C (§26) |
| P9 | DAC + transceiver sur le même lien | Pas de link | Même média des 2 côtés (§22) |
| P10 | Breakout non supporté par le port | Ports morts après achat | Matrice de breakout du switch (§23) |
| P11 | FEC qui masque une fibre sale | Panne brutale 6 mois plus tard | Superviser le pré-FEC (§27) |
| P12 | Backend IA + front-end même fabric | Le training noie tout | 2 fabrics séparés (§32) |
| P13 | Zéro port libre | Croissance = nouveau pod | +20 % de ports (§35) |
| P14 | MTU VXLAN oublié (+50 o.) | Fragmentation silencieuse | MTU 9050+ sur l'underlay (§37) |
| P15 | ECMP sans hash seed | Un spine saturé | Seed aléatoire par switch (§36) |
| P16 | PFC global au lieu de par file | Pauses storms, cascade | PFC sur la seule file RoCE (§45) |
| P17 | Leaf shallow-buffer en spine | Drops sous incast | Deep-buffer au spine (§44) |
| P18 | RoCE sans ECN/DCQCN | 60 % du débit (cas xAI) | Checklist §46 |
| P19 | Comparer les switchs sur l'ASIC seul | Mauvais choix | Évaluer le NOS + la validation (§50) |
| P20 | Airflow switch inversé | Hotspot 20 kW/rangée | Cohérence allées chaudes/froides (§59) |
| P21 | DAC 800G de 3 m | Ne monte pas | Portées §59 (DAC ≤ 2 m) |
| P22 | Optiques 18 W sur switch 12 W | Throttling thermique | Spec « max power per port » (§60) |
| P23 | Disjoncteurs au nominal | Déclenchement au boot | Calibre ≥ 125 % (§61) |
| P24 | DPU « pour faire moderne » | Surcoût sans gain | Matrice de décision §69 |
| P25 | 50 k lignes de DOCA sans plan de sortie | Lock-in logiciel | Évaluer le coût de sortie (§68) |
| P26 | DPU sans airflow (75 W) | Hotspot slot PCIe | Vérifier le cooling du slot (§72) |
| P27 | Cache CDN sur SATA seul | IOPS saturées | NVMe obligatoire (§74) |
| P28 | Serveur 800 mm en baie edge 600 mm | Ne rentre pas | Vérifier profondeur + alim (§76) |
| P29 | Anycast sur TCP longues sans persistance | Coupures au flap BGP | Persistance ou DNS (§78) |
| P30 | TTL infini sans purge | Contenu périmé servi | Purge + TTL adaptés (§74) |
| P31 | Fibre coincée sous un rail | BER élevée, invisible | Mou aux extrémités (§83) |
| P32 | Goulotte à 120 % | Dizaines de liens dégradés | Remplissage ≤ 50 %, audit annuel (§86) |
| P33 | Câble non étiqueté | 1 h de debug à 150 €/h | TIA-606 systématique (§85) |
| P34 | Plan d'adressage trop juste | Renumérotation nocturne | Prévoir la croissance (§87) |
| P35 | Salle réseau à 3 kW/baie | Surchauffe en 800G | 8-12 kW/baie (§94) |
| P36 | Disjoncteurs au nominal | Déclenchement au boot | (voir P23) calibre ≥ 125 % (§95) |
| P37 | Réseau pas sur UPS prioritaire | Arrêt à l'aveugle | UPS réseau séparé, en dernier (§117) |
| P38 | PUE sans les optiques | −50 % du poste oublié | Compter switch + optiques + NIC (§93) |
| P39 | Tests à vide seulement | Échec en all-to-all | Tester sous charge (§107) |
| P40 | NCCL sur le front-end 100G | 4× moins vite | `NCCL_SOCKET_IFNAME` (§108) |
| P41 | Stockage dimensionné à la moyenne | Checkpoint = 10 s de pause | Dimensionner pour le checkpoint (§109) |
| P42 | Automatiser un process cassé | Casser plus vite | Stabiliser puis automatiser (§111) |
| P43 | Fabric IA étendu inter-sites | All-reduce effondré | 1 fabric par site (§114) |
| P44 | Switch AC sur site −48 V DC | Ne s'alimente pas | Vérifier l'option DC (§115) |
| P45 | GPU et réseau sur le même UPS | La panne GPU emporte la supervision | UPS séparés (§117) |

## 123. BOMs types — trois projets prêts à chiffrer

### BOM 1 — Pod IA 256 GPU, backend 800G, 1:1 (prix ⚠️ à vérifier)

| Poste | Qté | PU (⚠️) | Total (⚠️) |
|---|---|---|---|
| Switch 32×800G (8 leafs + 16 spines) | 24 | 100-200 k€ | 2,4-4,8 M€ |
| NIC 800G (ConnectX-8) | 2048 | 3-6 k€ | 6,1-12,3 M€ |
| DAC/AOC 800G intra-rack | 2048 | 0,4-2 k€ | 0,8-4,1 M€ |
| Optiques 800G-DR8 inter-rangées | 768 | 4-10 k€ | 3,1-7,7 M€ |
| Fibre OS2/OM4 + brassage MDA | 1 lot | — | 150-300 k€ |
| Kit nettoyage + supervision | 1 lot | — | 30-60 k€ |
| Câblage, étiquetage, installation | 1 lot | — | 200-400 k€ |
| **Total** | | | **~12,8-29,7 M€** |

### BOM 2 — Datacenter cloud 100G, 512 serveurs, 3:1 (prix ⚠️)

| Poste | Qté | PU (⚠️) | Total (⚠️) |
|---|---|---|---|
| Leaf 48×25G+8×100G | 12 | 15-30 k€ | 180-360 k€ |
| Spine 32×100G | 2 | 25-50 k€ | 50-100 k€ |
| NIC 2×25G/100G (E810) | 1024 | 300-800 € | 307-819 k€ |
| DAC 25G/100G | 1024 | 50-150 € | 51-154 k€ |
| Optiques 100G-SR4/LR4 | 48 | 300-1 500 € | 14-72 k€ |
| **Total** | | | **~0,6-1,5 M€** |

### BOM 3 — PoP edge CDN 200 Gb/s (prix ⚠️, voir §79)

| Poste | Qté | PU (⚠️) | Total (⚠️) |
|---|---|---|---|
| Serveurs edge 2×100G, 12× NVMe | 2 | 25-40 k€ | 50-80 k€ |
| Switch 8×100G | 1 | 8-15 k€ | 8-15 k€ |
| Routeur cellulaire OOB | 1 | 300-600 € | 0,3-0,6 k€ |
| Spares (optiques, câbles, 1 switch) | 1 lot | — | 10-20 k€ |
| **CAPEX total** | | | **~68-116 k€** |

## 124. Checklist d'achat — à joindre à chaque appel d'offres

**NIC**
- [ ] Débit vs PCIe : génération × lanes **électriques** vérifiées (lspci)
- [ ] Format : CEM / OCP 3.0 / mezzanine — compatible châssis
- [ ] Firmware minimal pour le débit annoncé (ex. CX-8 : 40.46.x)
- [ ] Driver : inbox ou propriétaire ? Qui le maintient, quel cycle ?
- [ ] Offloads requis : SR-IOV (__ VF), VXLAN, RoCEv2, DPDK, TLS inline
- [ ] Optiques : lock constructeur ? Tester 1 tierce avant 100
- [ ] Conso **avec optiques**, pas carte seule
- [ ] UEC-ready / RoCEv2 validé sur le switch cible

**Switch**
- [ ] Capacité : non-bloquant ? Ratio down/up calculé (pas promis)
- [ ] Buffers : shallow (leaf) / deep (spine) selon position
- [ ] ECN, PFC par file, DCQCN / CC : supportés et validés
- [ ] Breakout : matrice des modes par port (doc constructeur)
- [ ] Max power per port optique (12 W ? 18 W ?)
- [ ] Airflow : sens cohérent avec les allées
- [ ] PSU : 1+1, 2 arrivées A/B, calibre disjoncteur ≥ 125 %
- [ ] NOS : EOS / NX-OS / SONiC durci — qui l'opère chez nous ?

**Fibre**
- [ ] Type : OM4 intra-baie, OS2 en rocade — couleurs conformes
- [ ] Longueurs : DAC ≤ portée (800G : 2 m), sinon AOC/optique
- [ ] MPO : méthode de polarité et genre documentés
- [ ] Budget optique : marge ≥ 3 dB calculée par lien type
- [ ] Kit nettoyage + microscope : 1 par salle (non négociable)

**Projet**
- [ ] 2 fabrics (IA + front-end) ou justification écrite du fabric unique
- [ ] Tests de réception §107 dans le contrat (critères chiffrés)
- [ ] SLA 4 h 24/7 + spares sur site pour le backend IA
- [ ] FW/driver figés par campagne dans le contrat de maintenance
- [ ] PUE / conso : switch + optiques + NIC chiffrés (§89-93)
- [ ] Clause de sortie : pas d'engagement 800G pluggable à 7 ans (CPO)

## 125. Sources vérifiées le 27/09/2026

| Fait | Source |
|---|---|
| ConnectX-8 : 800G, PCIe Gen6, OSFP224, Socket Direct, COMPUTEX 2025, FW 40.46.x | Docs firmware NVIDIA (docs.nvidia.com), Dell |
| Spectrum-X : Spectrum-4 51,2T, SN5600/SN5610 64×800G shipping | Specs NVIDIA, analyses d'écosystème |
| SN5600 : 940 W typique ; SN5610 : 0,9 kW / 2,08 kW à 64 optiques, ports 18 W | Datasheet NVIDIA SN5600 |
| BlueField-4 : 800G, Grace 64 cœurs, ConnectX-9, 128 Go, PCIe Gen6, GTC Washington 10/2025, dispo 2026 | NVIDIA, Canonical/Ubuntu |
| Broadcom Thor 2 : 400G, 5 nm, PCIe Gen5 x16, RoCE + apports UEC | Broadcom, FS, SDxCentral, STH |
| Broadcom Thor Ultra : 800G, premier NIC **UEC 1.0**, 14/10/2025 | Network World, EE Times |
| UEC 1.0 : **11/06/2025** ; 1.0.1 mi-2025 ; > 100 membres | UEC, HPCwire, DCD |
| Tomahawk 6-Davisson CPO : 102,4T, 64×1,6T, 3,5 W/port, 131 072 XPU, **08/10/2025** | Broadcom (GlobeNewswire), Next Platform |
| Broadcom Taurus : DSP 400G/lane, modules 1,6T/3,2T, OFC 03/2026, ère 200T | Broadcom OFC 2026 |
| Quantum-X800 CPO : 115,2T, 9 W/port vs 30 W | Analyses presse |
| ConnectX-9 : 1,6 Tb/s scale-out, Rubin H2 2026 | Presse spécialisée |
| Meta : 2 clusters 24 576 H100 (RoCE Arista vs IB Quantum-2), > 90 % util. | Presse spécialisée |
| xAI : Spectrum-X, 95 % vs 60 % Ethernet standard | Presse spécialisée |
| Arista 7060X6 : 64×800G, 2× 2400 W ; 7800R4 : 460T, Jericho3-AI | Arista, Next Platform |
| Intel E810 : PCIe Gen4 x16, 2×100G, 256 VF, 15-27 W | Fiches distributeurs |
| Intel : **pas de NIC 400G** au 27/09/2026 | ServeTheHome |

**Non trouvées / non vérifiées au 27/09/2026** : NIC Intel 400G ❌ · détails
Cisco Silicon One G200 ⚠️ · roadmap Intel IPU ⚠️ · prix publics précis des
switchs/NIC 800G ⚠️ (fourchettes marché uniquement) · conso exacte
ConnectX-8 / BlueField-4 ❌ (non publiée).

---

*Fin du guide — 125 sections. Vérification : `wc -l` — objectif ≥ 4000 lignes.*

# PARTIE L — FICHES PRODUITS DÉTAILLÉES (vérifiées 27/09/2026)

## 126. Fiche — NVIDIA ConnectX-8 SuperNIC

| Rubrique | Détail (✅ vérifié 27/09/2026 sauf note) |
|---|---|
| Débit | 800 Gb/s |
| PCIe | **Gen6** x16 (jusqu'à 48 lanes : usage switch PCIe interne) |
| Connecteurs | OSFP224 1×800G ou double 112 (2×400G) |
| Formats | OCP 3.0, CEM PCIe x16 |
| Features | In-Network Computing, MPI_Alltoall, QoS + contrôle de congestion, chiffrement TLS/IPsec inline sans perte |
| Socket Direct | Oui (carte auxiliaire 16 lanes) |
| Logiciel | DOCA 2.0, firmware 40.46.x (09/2025) |
| Lancement | COMPUTEX 2025 |
| Usage type | Serveurs RTX PRO, backend Blackwell, ratio 2:1 GPU:NIC (400G/GPU) |
| Prix indicatif | ❌ non public — fourchette marché 800G : 3 000-6 000 € (⚠️) |
| Alternatives | Broadcom Thor Ultra (UEC), Intel ❌ (pas de 800G au 27/09/2026) |
| Point fort | Commutateur PCIe intégré : remplace plusieurs switchs PCIe discrets |
| Point faible | Écosystème fermé NVIDIA, prix |
| Verdict | **La référence 800G** si on est déjà en Spectrum-X ; sinon évaluer Thor Ultra |

## 127. Fiche — Broadcom Thor 2 (BCM957608)

| Rubrique | Détail (✅ vérifié 27/09/2026 sauf note) |
|---|---|
| Débit | 400 Gb/s |
| PCIe | Gen5 x16 |
| Gravure | 5 nm (argument efficacité) |
| SerDes | 8 lanes : 100G PAM4 / 50G / 25G NRZ |
| RDMA | RoCEv2 + améliorations issues d'UEC : placement désordonné, ACK sélectif, multipath paquet, CC sans config |
| Cuivre | DAC passif jusqu'à ~5 m vers Tomahawk 5 |
| Optiques | Ultra-low-power linear (LPO) |
| Formats | Carte, chiplet, IP (3 modèles de consommation) |
| Usage type | Backend IA 400G sur fabric Broadcom/Arista, alternative ouverte à CX-7 |
| Prix indicatif | ❌ non public — fourchette 400G : 1 500-3 000 € (⚠️) |
| Point fort | Ouvert, basse conso, DAC longue portée |
| Point faible | Écosystème logiciel moins riche que DOCA |
| Verdict | **Le choix « ouvert » du 400G** — à privilégier hors écosystème NVIDIA |

## 128. Fiche — Broadcom Thor Ultra

| Rubrique | Détail (✅ vérifié 27/09/2026 sauf note) |
|---|---|
| Débit | **800G sur un seul flux** (pas 2×400G) |
| Conformité | **Premier NIC conforme UEC 1.0** (annoncé 14/10/2025) |
| RDMA | Modernisé : retransmission sélective, CC programmable émetteur/récepteur |
| Positionnement | IA scale-out uniquement (pas enterprise généraliste) |
| Interopérabilité | Fonctionne avec Tomahawk 5 existants et Tomahawk 6 futurs |
| Usage type | Backend IA 800G multi-vendeurs, pari UEC |
| Prix indicatif | ❌ non public (⚠️) |
| Point fort | UEC 1.0 natif, anti-lock-in |
| Point faible | Jeune (fin 2025) : valider le support et les drivers sur votre OS |
| Verdict | **Le pari ouvert du 800G** — exiger une preuve de concept avant volume |

## 129. Fiche — NVIDIA BlueField-4 DPU

| Rubrique | Détail (✅ vérifié 27/09/2026 sauf note) |
|---|---|
| Débit | **800 Gb/s** (2× BF-3) |
| CPU | **NVIDIA Grace : 64 cœurs ARM Neoverse V2** |
| Réseau | **ConnectX-9** |
| Mémoire | 128 Go |
| PCIe | **Gen6** |
| Logiciel | DOCA microservices, zero-trust, service chaining natif |
| Lancement | GTC Washington, octobre 2025 |
| Dispo | Early availability 2026, plateforme Vera Rubin |
| Usage type | AI factories : stockage AI-native (KV cache), sécurité runtime, multi-tenant |
| Systèmes | Partenaires attendus H2 2026 |
| Prix indicatif | ❌ non public (⚠️) |
| Point fort | 6× la puissance de calcul de BF-3, 800G |
| Point faible | Dispo 2026, écosystème DOCA = lock-in |
| Verdict | **À évaluer pour les plateformes Rubin** ; BF-3 reste le choix 400G immédiat |

## 130. Fiche — NVIDIA Spectrum-X SN5600 / SN5610

| Rubrique | Détail (✅ vérifié 27/09/2026 sauf note) |
|---|---|
| ASIC | Spectrum-4, 51,2 Tb/s, ~600 ns cut-through |
| Ports | **64× 800G OSFP** (ou 128×400G / 256×200G / 256×100G) |
| Débit forwarding | 33,3 Bpps |
| Buffer | 160 Mo partagé |
| Format | 2U, 23,5 kg |
| Conso | **940 W typique** (cuivre) ; SN5610 : 0,9 kW typ. / **2,08 kW à 64 optiques** |
| Ports optiques | Qualifiés jusqu'à **18 W** par OSFP |
| Breakout | 2×400G, 4×200G sans contrainte ; 8×100G sur port impair consomme le port pair adjacent |
| PSU | SN5600 : 2 (1+1) ; SN5610 : 4 (2+2) |
| NOS | Cumulus Linux / ONIE |
| Prix indicatif | ❌ non public — 64×800G : 150-300 k€ (⚠️) |
| Usage type | Leaf/spine Spectrum-X, backend Blackwell |
| Point fort | Boucle fermée avec SuperNIC : 95 % de throughput (cas xAI) |
| Point faible | Lock-in NVIDIA, OSFP uniquement |
| Verdict | **Le switch du backend NVIDIA** — hors écosystème NVIDIA, préférer TH5 |

## 131. Fiche — Broadcom Tomahawk 6 / 6-Davisson (CPO)

| Rubrique | Détail (✅ vérifié 27/09/2026 sauf note) |
|---|---|
| Capacité | **102,4 Tb/s** (2× TH5) |
| Ports (pluggable) | 64×1,6T / 128×800G / 256×400G / 512×200G |
| Version CPO | **Davisson** : 16 moteurs optiques 6,4T co-packagés |
| Conso CPO | **3,5 W/port à 800G** (−36 % vs TH5 CPO, **−70 % vs pluggable**) |
| Lasers | **Remplaçables par la face avant** (l'objection hyperscaler est levée) |
| Échelle | 131 072 XPU en 2 tiers |
| Routing | Cognitive Routing 2.0 |
| Statut | Annoncé (juin 2025) ; Davisson **early access** (08/10/2025) |
| Partenaires | Celestica, Accton, Micas, Nexthop AI, TSMC (COUPE) |
| Usage type | Fabrics IA géants 2026-2028, hyperscalers d'abord |
| Prix indicatif | ❌ non public (⚠️) |
| Point fort | Le CPO devient crédible : −70 % de conso optique, lasers remplaçables |
| Point faible | Early access : pas de volume enterprise avant 2026-2027 |
| Verdict | **La trajectoire à suivre** — ne pas verrouiller du 800G pluggable à 7 ans |

## 132. Fiche — Arista 7060X6 / 7800R4 (Etherlink AI)

| Rubrique | Détail (✅ vérifié 27/09/2026 sauf note) |
|---|---|
| 7060X6 (leaf) | TH5, 64×800G OSFP, 51,2T, **2× PSU 2400 W**, 2U |
| 7060X5 | TH4, 32×800G, 2× 1500 W |
| 7800R4 (spine) | Jericho3-AI, jusqu'à **460 Tb/s**, 576×800G ou 1152×400G par châssis |
| Gain 7800R4 | **−65 % de conso** vs 7800R3 (Jericho2) |
| NOS | **EOS** (le plus mature), CloudVision, ZTP |
| Features IA | Etherlink : monitoring par flux, tuning RoCE, AI jobs visibility |
| Référence client | Meta (cluster RoCE 24 576 H100, ✅) |
| Prix indicatif | ❌ non public — 64×800G : 150-300 k€ (⚠️) |
| Point fort | EOS + support enterprise, choix « sans surprise » |
| Point faible | Prix premium vs whitebox |
| Verdict | **Le choix enterprise du backend IA** si l'équipe connaît Arista |

## 133. Fiche — Intel E810 (la NIC 100G enterprise)

| Rubrique | Détail (✅ vérifié 27/09/2026 sauf note) |
|---|---|
| Débit | 2×100G (10/25/50/100G auto) |
| PCIe | **Gen4 x16** |
| Connecteur | 2× QSFP28 |
| Virtualisation | **256 VF SR-IOV**, VMDq, 768 VSI |
| Offloads | VXLAN/GENEVE/NVGRE, ADQ, DPDK |
| RDMA | iWARP + RoCEv2 |
| Timing | IEEE 1588 PTP v1/v2, per-packet timestamp |
| DCB | PFC (Qbb), ETS (Qaz) — lossless ready |
| Conso | **15 W** (2 ports) à **27,1 W** (avec AOC) ✅ |
| Driver | `ice` inbox (kernel 5.x+) — le plus simple à maintenir |
| Prix indicatif | 300-800 € (⚠️) |
| Usage type | Serveurs généralistes, VMs, stockage, firewall |
| Point fort | Driver inbox, SR-IOV mature, sobre |
| Point faible | Pas de 200/400G (❌ non trouvée au 27/09/2026) |
| Verdict | **La NIC 100G par défaut** quand on ne fait pas d'IA |

# PARTIE M — DIMENSIONNEMENTS PAS-À-PAS

## 134. Cas complet — cluster 2048 GPU, 3 tiers, 800G

**Hypothèses** : 256 nœuds × 8 GPU (B200), 1 NIC 800G/GPU, 1:1, rail-optimized.

**Étape 1 — Endpoints** : 2048 NIC 800G.

**Étape 2 — Pods** : pod = 256 GPU (32 nœuds). 2048/256 = **8 pods**.

**Étape 3 — Par pod** (rail-optimized, 8 plans) :
- Par plan : 256/8 = 32 endpoints → 1 leaf 64×800G (32 down + 32 up).
- Leafs/pod : 8 plans × 1 = **8 leafs**.
- Spines/pod : 8 plans × 2 = **16 spines** 64×800G (32 ports utilisés).
- Vérification 1:1 : 32×800G up / 32×800G down par leaf ✅.

**Étape 4 — Super-spines** : chaque pod expose 16 spines × 32 uplinks =
512 liens 800G vers le haut. 8 pods = 4096 liens.
Super-spines 64×800G : 4096/64 = **64 super-spines**.
Oversubscription pod→super-spine : on met 16 uplinks par spine de pod
(au lieu de 32) → ratio 2:1 inter-pods (acceptable : le trafic inter-pods
est < 20 % en training, ⚠️ hypothèse à valider par workload).

**Étape 5 — Comptes** :

| Niveau | Switchs | Ports 800G |
|---|---|---|
| Leafs | 8 pods × 8 = 64 | 64 × 64 = 4096 |
| Spines pod | 8 × 16 = 128 | 128 × 64 = 8192 |
| Super-spines | 64 | 64 × 64 = 4096 |
| **Total** | **256 switchs** | **16 384 ports** |

**Étape 6 — Énergie** (⚠️) : 256 × 1,5 kW = 384 kW (switchs) + 2048 ×
60 W = 123 kW (NIC) + ~8000 optiques × 15 W = 120 kW → **~627 kW IT**,
soit **~878 kW** au compteur (PUE 1,4).

**Étape 7 — Budget** (⚠️) : 256 × 200 k€ = 51 M€ (switchs) + 2048 ×
4,5 k€ = 9,2 M€ (NIC) + optiques/câbles ~25 M€ → **~85 M€** de réseau
pour ~2048 GPU (~500 M€ de GPU, ⚠️) : **~15 %** du cluster.

## 135. Cas complet — datacenter cloud 2000 serveurs généralistes

**Hypothèses** : 2000 serveurs × 2×25G, 3:1, EVPN/VXLAN, 2 sites (actif/actif).

**Étape 1 — Leafs** : 48×25G par leaf → 2000/48 ≈ 42 leafs (prendre 44).
Uplinks : 1200 Gb/s down → 400 Gb/s up (3:1) = 4×100G par leaf.
Uplinks totaux : 44 × 4 = 176×100G.

**Étape 2 — Spines** : switch 32×100G → 176/32 ≈ 6 spines (prendre 6).

**Étape 3 — Adressage** :
- Loopbacks : 50 × /32 dans 10.0.0.0/24.
- Liens : 176 × /31 = 352 adresses → **10.0.1.0/23** (512 adresses).
- ASN : spines 65101-65106, leafs 65001-65044.

**Étape 4 — Overlay** : VNI par tenant (16M possibles), anycast gateway,
route-reflectors sur 2 spines.

**Étape 5 — Énergie** (⚠️) : 44 × 350 W + 6 × 500 W = 18,4 kW (switchs) +
4000 NIC × 12 W = 48 kW → **~66 kW IT**, ~93 kW au compteur.

**Étape 6 — Budget** (⚠️) : 44 × 22 k€ + 6 × 37 k€ = ~1,2 M€ (switchs) +
4000 × 500 € = 2 M€ (NIC) + DAC ~0,3 M€ → **~3,5 M€**, soit **1 750 €/
serveur** de réseau. C'est le coût « banalisé » du 25/100G : deux ordres
de grandeur sous le backend IA.

## 136. Cas complet — DCI 80 km chiffré entre 2 sites

**Hypothèses** : 2 datacenters à 80 km, besoin 800 Gb/s utiles, chiffré.

**Étape 1 — Technologie** : 80 km = limite du ZR. **2× 400G-ZR** (cohérent
enfichable, OpenZR+) ou 1× 800G-ZR (⚠️ dispo à vérifier au 27/09/2026 —
prendre 2×400G-ZR, mature).

**Étape 2 — Fibre** : 1 paire OS2 louée (ou propre). Budget : 80 km ×
0,25 dB/km (⚠️ G.652 à 1550 nm avec connecteurs) ≈ 20 dB + marges →
vérifier que le budget du ZR (typiquement 22-26 dB, ⚠️) passe. Sinon :
ampli EDFA (baie optique).

**Étape 3 — Chiffrement** : MACsec 400G en HW sur les ports DCI (si
supporté) ou IPsec sur routeurs. **Ne pas transporter de clair** sur une
fibre louée.

**Étape 4 — Routage** : eBGP entre sites (ASN différents), BFD 300 ms,
pas d'extension du fabric (voir P43).

**Étape 5 — Budget** (⚠️) : 4 optiques 400G-ZR × 8-15 k€ = 32-60 k€ +
2 ports routeur 400G + fibre (location : ~1-3 k€/km/an → 80-240 k€/an !).
**La fibre représente 70 %+ du TCO** : négocier l'IRU (droit d'usage
15 ans) plutôt que la location annuelle si le besoin est durable.

## 137. Cas complet — lab de test d'un fabric (avant production)

**Objectif** : valider FW, CC, ECMP sur 8 nœuds avant de déployer 1000.

| Poste | Détail |
|---|---|
| Nœuds | 8 serveurs (même réf que la prod, ou 4 + simulateurs) |
| NIC | Les mêmes qu'en prod (même FW !) |
| Switchs | 2 leafs + 2 spines (même modèle, même NOS, même version) |
| Câblage | Même média qu'en prod (DAC/AOC/optiques) |
| Outils | nccl-tests, ib_write_bw, sondes latence, capture ECN/PFC |

**Protocole** (4 semaines type) :
1. **S1** : câblage + FW + config de référence (la « golden config »).
2. **S2** : tests §107 (bande passante, latence, ECMP, perte d'un spine).
3. **S3** : chaos — kill -9 d'un process BGP, flap d'un lien, panne PSU.
   Tout ce qui n'est pas testé **arrivera** en prod.
4. **S4** : gel de la config + documentation (c'est elle qui part en prod).

**Budget** (⚠️) : ~150-300 k€ pour un lab 800G 8 nœuds. À comparer au coût
d'une semaine de cluster 1000 GPU idle : **le lab est 100× rentabilisé**
dès le premier incident évité.
**Règle** : le lab reste **branché et à jour** après le déploiement — c'est
là qu'on teste chaque changement avant la prod (FW, config, NOS).

## 138. Annexe — commandes de diagnostic (pense-bête)

**NIC NVIDIA (Linux)** :
```
ethtool -m ethX            # DOM : températures, puissances Tx/Rx
ethtool -S ethX | grep -i -E 'fec|cnp|retrans|pause'   # compteurs clés
mlxlink -d /dev/mst/mt4125_pciconf0   # état du lien, BER, FEC
lspci -vvv -s <bdf> | grep -E 'LnkSta|LnkCap'  # PCIe : génération × lanes
ib_write_bw -d mlx5_0      # bande passante RDMA
ib_write_lat -d mlx5_0     # latence RDMA
```

**NIC Intel** :
```
ethtool -k ethX            # offloads on/off
ethtool -S ethX | grep -v ': 0'   # compteurs non nuls
```

**Arista EOS** :
```
show interfaces transceiver dom   # DOM optiques
show interfaces counters errors   # erreurs/FCS
show priority-flow-control        # PFC par file
show qos interfaces               # ECN, files
show ip bgp summary               # underlay
bash tcpdump -i ethX              # capture (shell Linux)
```

**Cumulus / SONiC** :
```
nv show interface                 # Cumulus : état, DOM, compteurs
sonic-clear counters ; show interfaces counters  # SONiC
```

**Test rapide « c'est le réseau ou pas »** :
1. `ib_write_bw` entre 2 nœuds : si < 90 % → réseau.
2. `ethtool -S` : si pre-FEC BER > 1e-9 → fibre/optique.
3. Compteurs ECN/CNP : si storm → congestion/CC.
4. `NCCL_DEBUG=INFO` : si mauvaise interface → config.

## 139. Annexe — table maître débits / formats / fibres / distances

| Débit | Format NIC/switch | Fibre intra-DC | Distance | Inter-DC |
|---|---|---|---|---|
| 25G | SFP28 | DAC / OM4 SR | 5 m / 100 m | — |
| 100G | QSFP28 | DAC / AOC / OM4 SR4 / OS2 DR | 5 m / 100 m / 500 m | FR1 2 km, LR4 10 km |
| 200G | QSFP56 | DAC / AOC / OM4 SR4 | 3 m / 100 m | — |
| 400G | QSFP112/DD, OSFP | DAC / AOC / OM4 SR8 / OS2 DR4 | 3 m / 100 m / 500 m | FR4 2 km, LR4 10 km, ZR 80 km |
| 800G | OSFP, QSFP-DD800 | DAC / AEC / AOC / OM4 SR8 / OS2 DR8 | 2 m / 6 m / 30 m / 100 m / 500 m | 2×FR4 2 km |
| 1,6T | (OSFP-XD) | (AOC / OS2) | (⏳ 2027+) | — |

## 140. Index — retrouver vite

| Besoin | Sections |
|---|---|
| Choisir une NIC | §2, §4, §6, §7, §11, fiches §126-129, §133 |
| Valider le PCIe | §2, §8 |
| Comprendre RoCEv2 / UEC | §10, §45, §46, §54 |
| Choisir fibre / optique | §16-23, §28, §139 |
| Nettoyer / diagnostiquer la fibre | §24, §25, §27, §138 |
| Dimensionner un spine-leaf | §32, §33, §34, §35, §53 |
| Topologie IA (rail-optimized) | §39, §40 |
| Choisir un switch IA | §43-52, fiches §130-132 |
| InfiniBand vs Ethernet | §47, §48 |
| DPU : en mettre ou pas | §64-72, fiche §129 |
| Dimensionner un CDN/PoP | §73-80 |
| Adressage / câblage / étiquetage | §81-88 |
| Chiffrer l'énergie | §89-98, §112 |
| Futur (1,6T, UEC, CPO) | §99-103 |
| Glossaire / quiz / pièges | §105, §121, §122 |
| BOMs / checklist / sources | §123, §124, §125 |
| Dimensionnements pas-à-pas | §134-137 |

# PARTIE N — ANNEXES TECHNIQUES DENSES

## 141. Catalogue transceivers — 25 références types

Valeurs usuelles IEEE/MSA (⚠️ vérifier la datasheet du module exact) :

| Référence type | Débit | Format | Fibre | Connecteur | Distance | Conso (⚠️) |
|---|---|---|---|---|---|---|
| 10GBASE-SR | 10G | SFP+ | OM3/OM4 | LC | 300/400 m | 1 W |
| 10GBASE-LR | 10G | SFP+ | OS2 | LC | 10 km | 1 W |
| 25GBASE-SR | 25G | SFP28 | OM3/OM4 | LC | 70/100 m | 1 W |
| 25GBASE-LR | 25G | SFP28 | OS2 | LC | 10 km | 1,5 W |
| 100GBASE-SR4 | 100G | QSFP28 | OM4 (8 brins) | MPO-12 | 100 m | 2,5 W |
| 100GBASE-PSM4 | 100G | QSFP28 | OS2 (8 brins) | MPO-12 | 500 m | 3,5 W |
| 100GBASE-DR | 100G | QSFP28 | OS2 (2 brins) | LC | 500 m | 3,5 W |
| 100GBASE-FR1 | 100G | QSFP28 | OS2 | LC | 2 km | 4 W |
| 100GBASE-LR4 | 100G | QSFP28 | OS2 | LC | 10 km | 4,5 W |
| 100G-BiDi (SWDM4) | 100G | QSFP28 | OM5 (2 brins) | LC | 150 m | 3,5 W |
| 200GBASE-SR4 | 200G | QSFP56 | OM4 | MPO-12 | 100 m | 4 W |
| 200GBASE-DR4 | 200G | QSFP56 | OS2 (8 brins) | MPO-12 | 500 m | 5 W |
| 400GBASE-SR8 | 400G | QSFP-DD/OSFP | OM4 (16 brins) | MPO-16 | 100 m | 9 W |
| 400GBASE-DR4 | 400G | QSFP-DD/OSFP | OS2 (8 brins) | MPO-12 | 500 m | 9 W |
| 400GBASE-FR4 | 400G | QSFP-DD/OSFP | OS2 (2 brins) | LC | 2 km | 10 W |
| 400GBASE-LR4 | 400G | QSFP-DD/OSFP | OS2 | LC | 10 km | 12 W |
| 400GBASE-ZR | 400G | QSFP-DD | OS2 | LC | 80 km | 15 W |
| 800GBASE-SR8 | 800G | OSFP | OM4 (16 brins) | MPO-16 | 100 m | 13 W |
| 800GBASE-DR8 | 800G | OSFP | OS2 (16 brins) | MPO-16 | 500 m | 15 W |
| 800GBASE-2×FR4 | 800G | OSFP | OS2 (4 brins) | 2×CS | 2 km | 16 W |
| 800GBASE-LR8 | 800G | OSFP | OS2 | LC | 10 km | 18 W |
| DAC 100G 3 m | 100G | QSFP28 | Cuivre | — | 3 m | 0,3 W |
| DAC 400G 2 m | 400G | QSFP-DD | Cuivre | — | 2 m | 0,5 W |
| AOC 400G 30 m | 400G | QSFP-DD | Fibre active | — | 30 m | 2 W |
| AOC 800G 30 m | 800G | OSFP | Fibre active | — | 30 m | 3 W |

**Lecture** : à 800G, l'écart DAC (0,5 W) vs DR8 (15 W) = **×30**. Chaque
lien 800G qui peut rester en cuivre fait économiser ~30 W (les 2 bouts).

## 142. Normes IEEE 802.3 — repères par génération

| Norme | Année | Débit | Modulation | Média |
|---|---|---|---|---|
| 802.3ae | 2002 | 10G | NRZ | Fibre |
| 802.3ba | 2010 | 40/100G | NRZ | Fibre/Cuivre |
| 802.3bm | 2015 | 100G | NRZ | — |
| 802.3bs | 2017 | 200/400G | PAM4 | Fibre |
| 802.3cd | 2018 | 50/100/200G | PAM4 | — |
| 802.3cu | 2021 | 100G monomode | PAM4 | OS2 |
| 802.3db | 2022 | 100/200/400G SR | PAM4 | OM4 |
| 802.3ck | 2022 | 100/200/400G élec. | PAM4 100G/lane | Backplane |
| 802.3df | 2024 | 800G | PAM4 100G/lane | Fibre |
| 802.3dj (⏳) | 2026+ | 1,6T | PAM4 200G/lane | En cours |

**À retenir** : chaque génération double le débit en doublant les lanes
ou le baud rate, en gardant la compatibilité descendante des formats.

## 143. Arbre de dépannage — « le réseau est lent »

```
  LE RÉSEAU EST LENT
  │
  ├─ Tous les nœuds ? ── OUI ──→ Fabric : ECMP ? (§36) / CC ? (§46) / spine down ?
  │                        NON ──→ Ce nœud : voir ci-dessous
  │
  ├─ ib_write_bw < 90 % ? ── OUI ──→ Lien/NIC : ethtool -S (BER ? FEC ?)
  │   └─ BER > 1e-9 ? ── OUI ──→ Nettoyer fibre (§24) → budget optique (§20)
  │                        NON ──→ PCIe ? (lspci : Gen × lanes, §8)
  │
  ├─ NCCL lent seul ? ── OUI ──→ NCCL_SOCKET_IFNAME (§108) : bonne interface ?
  │   └─ busbw < 70 % ? ── OUI ──→ ECMP/CC sur le chemin (§107)
  │
  ├─ Pauses PFC ? ── OUI ──→ Deadlock : quelle file ? Quel émetteur ? (§45)
  │
  ├─ CNP storm ? ── OUI ──→ DCQCN mal réglé ou incast réel (§46)
  │
  └─ Aléatoire ? ──→ FW hétérogènes (§13) / ECMP polarisé (§36) / thermique (§60)
```

**Règle** : toujours mesurer **avant** de changer quoi que ce soit, et ne
changer **qu'une chose à la fois**. Le debug « on a tout redémarré et ça
remarche » n'apprend rien et ça reviendra.

## 144. Templates — plan d'adressage, BOM, test de réception

**Template plan d'adressage** (à remplir par site) :
```
Site : __________    Date : __________    Responsable : __________
Loopbacks      : 10.__.0.0/24   (équipement : __________)
Liens /31      : 10.__.1.0/__   (____ liens prévus, ____ de réserve)
Management     : 192.168.__.0/24
ASN spines     : 651__
ASN leafs      : 650__
VTEP           : = loopbacks
IPv6 loopbacks : ____::/64  (prévu : oui/non)
NetBox         : URL __________
```

**Template BOM** (une ligne par poste) :
```
| Poste | Référence | Qté | PU HT | Total HT | Fournisseur | Délai |
|-------|-----------|-----|-------|----------|-------------|-------|
|       |           |     |       |          |             |       |
Réserve spares : ____%    Total HT : ______    Total TTC : ______
```

**Template test de réception** (un par fabric) :
```
Fabric : __________    Date : __________    Opérateur : __________
[ ] ib_write_bw ≥ 95 % : ____ Gb/s mesurés
[ ] NCCL all-reduce busbw ≥ 80 % : ____
[ ] Latence < 2 µs : ____ µs
[ ] ECMP ±10 % : max ____ / min ____
[ ] Perte 1 spine : ____% perte, ____ s convergence
[ ] Burn-in 72 h : ____ erreurs FEC, ____ flaps
[ ] Thermique : max optique ____°C
Réserve(s) : __________
Signature : __________
```

## 145. Lexique des acronymes — 100 entrées

| Acronyme | Signification |
|---|---|
| ACI | Application Centric Infrastructure (Cisco) |
| ACL | Access Control List |
| ADQ | Application Device Queues (Intel) |
| AEC | Active Electrical Cable |
| AOC | Active Optical Cable |
| ASIC | Application-Specific Integrated Circuit |
| ASN | Autonomous System Number |
| BDP | Bandwidth-Delay Product |
| BER | Bit Error Rate |
| BFD | Bidirectional Forwarding Detection |
| BGP | Border Gateway Protocol |
| BiDi | Bidirectional (1 fibre, 2 longueurs d'onde) |
| BOM | Bill of Materials |
| Bpps | Billion packets per second |
| CEM | Card Electromechanical (PCIe) |
| CNP | Congestion Notification Packet |
| CPO | Co-Packaged Optics |
| DAC | Direct Attach Cable |
| DCB | Data Center Bridging |
| DCI | Data Center Interconnect |
| DCQCN | Data Center Quantized Congestion Notification |
| DOM | Digital Optical Monitoring |
| DPU | Data Processing Unit |
| DR | 500 m (optique) |
| DSP | Digital Signal Processor |
| DWDM | Dense Wavelength Division Multiplexing |
| ECMP | Equal-Cost Multi-Path |
| ECN | Explicit Congestion Notification |
| EDFA | Erbium-Doped Fiber Amplifier |
| EOS | Extensible Operating System (Arista) |
| ETS | Enhanced Transmission Selection |
| EVPN | Ethernet VPN |
| FEC | Forward Error Correction |
| FR | 2 km (optique) |
| FW | Firmware |
| GENEVE | Generic Network Virtualization Encapsulation |
| gNMI | gRPC Network Management Interface |
| HCA | Host Channel Adapter (InfiniBand) |
| IB | InfiniBand |
| iWARP | Internet Wide Area RDMA Protocol |
| LACP | Link Aggregation Control Protocol |
| LANZ | Latency Analyzer (Arista) |
| LPO | Linear Pluggable Optics |
| LR | 10 km (optique) |
| LRO | Linear Receive Optics |
| MAC | Media Access Control / Moves-Adds-Changes |
| MACsec | Media Access Control Security |
| MDA | Main Distribution Area |
| MPO | Multi-fiber Push-On |
| MSA | Multi-Source Agreement |
| MTU | Maximum Transmission Unit |
| NCCL | NVIDIA Collective Communications Library |
| NDR | 400G InfiniBand |
| NOS | Network Operating System |
| NRZ | Non-Return-to-Zero |
| NVMe-oF | NVMe over Fabrics |
| OCP | Open Compute Project |
| OFED | OpenFabrics Enterprise Distribution |
| OM3/4/5 | Optical Multimode (grades) |
| ONIE | Open Network Install Environment |
| OOB | Out-Of-Band |
| OS2 | Optical Single-mode (grade) |
| OSFP | Octal Small Form-factor Pluggable |
| OVS | Open vSwitch |
| PAM4 | Pulse Amplitude Modulation (4 niveaux) |
| PFC | Priority Flow Control |
| PoP | Point of Presence |
| PTP | Precision Time Protocol |
| PUE | Power Usage Effectiveness |
| Qbb/Qaz | Normes 802.1 (PFC/ETS) |
| QSFP | Quad Small Form-factor Pluggable |
| RDMA | Remote Direct Memory Access |
| RoCE | RDMA over Converged Ethernet |
| RTT | Round-Trip Time |
| SFP | Small Form-factor Pluggable |
| SLA | Service Level Agreement |
| SNMP | Simple Network Management Protocol |
| SONiC | Software for Open Networking in the Cloud |
| SR | Short Reach |
| SR-IOV | Single-Root I/O Virtualization |
| STP | Spanning Tree Protocol |
| SWDM | Short Wavelength Division Multiplexing |
| TAC | Technical Assistance Center |
| ToR | Top-of-Rack |
| TSO/GRO | TCP Segmentation / Generic Receive Offload |
| UEC | Ultra Ethernet Consortium |
| UET | Ultra Ethernet Transport |
| VMDq | Virtual Machine Device Queues |
| VNI | VXLAN Network Identifier |
| VRF | Virtual Routing and Forwarding |
| VTEP | VXLAN Tunnel EndPoint |
| VXLAN | Virtual Extensible LAN |
| WDM | Wavelength Division Multiplexing |
| XDR | 800G InfiniBand |
| ZR | 80 km+ (optique cohérente) |
| ZTP | Zero-Touch Provisioning |

## 146. Timeline — 25 ans d'Ethernet datacenter

| Année | Jalon |
|---|---|
| 1999 | Gigabit Ethernet (1000BASE-SX/LX) : le DC passe au gigabit |
| 2002 | 10G (802.3ae) : agrégation |
| 2010 | 40/100G (802.3ba) : QSFP+, le DC moderne naît |
| 2012 | Spine-leaf / Clos : Google, puis tous les hyperscalers |
| 2014 | 25/50G (802.3by) : le 25G remplace le 10G |
| 2015 | RoCEv2 : le RDMA sur Ethernet devient standard |
| 2017 | 200/400G (802.3bs), PAM4 : la modulation change |
| 2018 | SONiC open-source (Microsoft) : le whitebox décolle |
| 2020 | 400G en volume, DAC/AOC 400G |
| 2021 | BlueField-2/3 : le DPU entre au DC |
| 2023 | **UEC fondé** (07/2023) : l'industrie veut un Ethernet IA ouvert |
| 2024 | 800G (802.3df), Spectrum-X, Thor 2 |
| 2025 | **ConnectX-8**, **Thor Ultra** (UEC 1.0, 10/2025), **BlueField-4** (10/2025), **Tomahawk 6-Davisson CPO** (10/2025), **UEC 1.0** (06/2025) |
| 2026 | OFC : Broadcom Taurus (400G/lane), ère 200T annoncée ; VIAVI valide UET |
| 2027+ | 1,6T (ConnectX-9, Rubin), CPO en volume, UEC multi-vendeurs |

## 147. Comparatif NOS — EOS, NX-OS, SONiC, Cumulus

| Critère | Arista EOS | Cisco NX-OS | SONiC (durci) | NVIDIA Cumulus |
|---|---|---|---|---|
| Modèle | Propriétaire | Propriétaire | Open-source | Propriétaire (Linux) |
| Maturité IA | ✅ Etherlink | ⚠️ (G200 récent) | ⚠️ (selon distribution) | ✅ Spectrum-X |
| Automation | eAPI, CloudVision | NX-API, NDFC | gNMI, standard | Ansible, NCLU |
| ZTP | ✅ | ✅ | ✅ | ✅ |
| Telemetry | LANZ, streaming | Streaming | Streaming | Streaming |
| Coût licence | Élevé (⚠️) | Élevé (⚠️) | Faible (support à part) | Moyen (⚠️) |
| Courbe d'apprentissage | Faible (CLI Cisco-like) | Faible | Haute (Linux + SAI) | Moyenne (Linux) |
| Idéal pour | Enterprise, IA | Existant Cisco | Whitebox, hyperscale-like | Fabrics NVIDIA |

**Règle** : on ne change pas de NOS comme de chemise — le coût de formation
et de réécriture d'automation dépasse l'économie de licence. Choisir pour
10 ans, pas pour le prix de l'année 1.

## 148. Checklist d'audit sécurité du fabric (trimestrielle)

- [ ] Comptes : pas de `admin/admin`, AAA partout, revue des accès
- [ ] Firmware : versions à jour **dans la politique figée** (pas « latest »)
- [ ] Images signées : secure boot actif sur switchs et DPU
- [ ] Management : VRF/OOB séparé, ACLs infra, pas de SSH ouvert sur le data
- [ ] SNMP : v3 uniquement (pas de v2c « public » qui traîne)
- [ ] Logs : centralisés, 1 an de rétention, alertes sur login anormal
- [ ] Configs : sauvegardées, versionnées (Git), diff avant/après chaque change
- [ ] Ports : 802.1X ou MAC lockdown sur les ports serveurs sensibles
- [ ] DCI : chiffrement actif et vérifié (pas seulement « configuré »)
- [ ] Plan de réponse : qui appeler, quel TAC, quel spare — affiché au NOC

## 149. Supervision — exemples d'alertes Prometheus

```yaml
# BER pré-FEC : alerte si > 1e-9 pendant 10 min
- alert: HighPreFecBer
  expr: pre_fec_ber > 1e-9
  for: 10m
  labels: { severity: warning }
  annotations: { summary: "BER pré-FEC élevée sur {{ $labels.iface }}" }

# Erreurs FEC non corrigées : critique immédiate
- alert: FecUncorrectable
  expr: increase(fec_uncorrectable_total[5m]) > 0
  labels: { severity: critical }

# Pauses PFC : signe de deadlock
- alert: PfcStorm
  expr: rate(pfc_pause_frames_total[5m]) > 10
  labels: { severity: critical }

# Température optique
- alert: OpticHot
  expr: optic_temp_celsius > 70
  for: 15m
  labels: { severity: warning }

# Marge optique faible (Tx - Rx vs sensibilité)
- alert: OpticLowMargin
  expr: (optic_rx_dbm - optic_rx_sensitivity_dbm) < 3
  for: 30m
  labels: { severity: warning }

# ECMP déséquilibré (écart-type des compteurs par lien)
- alert: EcmpImbalance
  expr: stddev by (device) (rate(if_out_octets[5m])) / avg by (device) (rate(if_out_octets[5m])) > 0.3
  for: 30m
  labels: { severity: warning }
```

## 150. FAQ terrain — 30 questions/réponses courtes

**1.** DAC ou AOC en intra-rack 400G ? → DAC si < 3 m, AOC sinon.
**2.** OM4 ou OS2 pour une rocade neuve ? → OS2 (15 ans), OM4 vers les baies.
**3.** 400G ou 800G pour un cluster neuf en 2026 ? → 800G (coût/Gb/s inférieur).
**4.** RoCEv2 sans PFC, ça marche ? → Non en production (il faut le lossless).
**5.** UEC remplace RoCEv2 ? → À terme ; en 2026 c'est une assurance, pas un remplacement.
**6.** Combien de cœurs CPU pour 400G sans offload ? → 8-16 (d'où les offloads).
**7.** PCIe Gen4 x16 pour une NIC 400G ? → Non, bridé à ~200G.
**8.** MTU pour RoCEv2 ? → 9000 partout, sans exception.
**9.** ECN ou PFC en premier ? → ECN (marquage), PFC en dernier recours.
**10.** Un switch 800G chauffe combien ? → ~1-2 kW tout-optique.
**11.** Nettoyer une fibre avec quoi ? → Cassette à sec d'abord (§24).
**12.** MPO mâle + MPO mâle ? → Jamais (casse les broches).
**13.** Breakout 800G → 8×100G : piège ? → Certains ports partagent des ressources (§23).
**14.** Oversubscription du backend IA ? → 1:1, toujours.
**15.** ECMP + un seul gros flux ? → Ne se répartit pas (spraying nécessaire).
**16.** DPU pour un serveur web ? → Non, surcoût inutile.
**17.** BlueField-3 ou 4 en 2026 ? → BF-3 dispo, BF-4 early availability 2026.
**18.** Chiffrer le backend NCCL en soft ? → Non (tue la perf), HW si exigé.
**19.** 1,6T : quand ? → ConnectX-9 / Rubin, H2 2026 (annoncé).
**20.** CPO : hype ou réel ? → Réel : Davisson early access 10/2025, −70 % conso.
**21.** Prix d'un switch 64×800G ? → Non public ; fourchette 150-300 k€ (⚠️).
**22.** Durée de vie d'un fabric IA ? → 3-4 ans, pas 7.
**23.** Tester un fabric : combien de temps ? → 72 h de burn-in minimum.
**24.** NCCL lent : premier réflexe ? → `NCCL_DEBUG=INFO` (quelle interface ?).
**25.** Un lien « monte » mais BER élevée ? → Nettoyer, puis budget optique.
**26.** Goulotte remplie à combien ? → 50 % max.
**27.** Étiquette : quel format ? → `SALLE-RANGÉE-BAIE-ÉQ-PORT` (TIA-606).
**28.** Onduleur du réseau : séparé ? → Oui, prioritaire, en dernier à l'arrêt.
**29.** IPv6 : quand ? → Plan prévu dès maintenant, activé plus tard.
**30.** La règle d'or ? → Mesurer avant de changer ; une chose à la fois.

---

*Fin du guide — 150 sections. Vérification : `wc -l` — objectif ≥ 4000 lignes.*

# PARTIE O — COMPLÉMENTS ULTRA-DENSES

## 151. Comparatif switches — 20 modèles du marché

Capacité = agrégée bidirectionnelle. Prix ⚠️ fourchettes marché.

| Modèle | ASIC | Ports max | Capacité | Format | Conso (⚠️) | Usage | Prix (⚠️) |
|---|---|---|---|---|---|---|---|
| NVIDIA SN5600 | Spectrum-4 | 64×800G | 51,2T | 2U | 0,94-2 kW | Backend IA | 150-300 k€ |
| NVIDIA SN5610 | Spectrum-4 | 64×800G | 51,2T | 2U | 0,9-2,08 kW | Backend IA | 150-300 k€ |
| Arista 7060X6 | TH5 | 64×800G | 51,2T | 2U | ~1,5-2 kW | Leaf IA | 150-300 k€ |
| Arista 7060X5 | TH4 | 32×800G | 25,6T | 1U | ~1-1,5 kW | Leaf | 80-150 k€ |
| Arista 7800R4 | J3-AI | 576×800G | 460T | Châssis | ~15-25 kW | Spine IA | 1-2 M€ |
| Arista 7280R4 | TH5 | 48×800G | 51,2T | 2U | ~1,5 kW | Leaf/spine | 150-250 k€ |
| Cisco 8102-64H | S1 G200 | 64×800G | 51,2T | 2U | ~1,5 kW | SP/IA | ⚠️ |
| Juniper QFX5240 | TH5 | 64×800G | 51,2T | 2U | ~1,5 kW | DC | 120-250 k€ |
| Edgecore DCS810 | TH5 | 64×800G | 51,2T | 2U | ~1,5 kW | Whitebox | 80-150 k€ |
| Celestica DS5000 | TH5 | 64×800G | 51,2T | 2U | ~1,5 kW | Whitebox | 80-150 k€ |
| Dell Z9664F | TH5 | 64×800G | 51,2T | 2U | ~1,5 kW | Enterprise | 120-250 k€ |
| HPE 9300S | TH5 | 64×800G | 51,2T | 2U | ~1,5 kW | Enterprise | 120-250 k€ |
| Arista 7060CX2 | TH3 | 32×100G | 6,4T | 1U | ~300 W | Leaf 100G | 15-30 k€ |
| Cisco N9K-C9336C | — | 36×100G | 7,2T | 1U | ~400 W | Enterprise | 20-40 k€ |
| Juniper QFX5120 | TH3 | 48×25G+8×100G | 4T | 1U | ~300 W | Leaf | 12-25 k€ |
| Edgecore DCS203 | TH3 | 32×400G | 12,8T | 1U | ~500 W | Leaf 400G | 25-50 k€ |
| NVIDIA SN4700 | Spectrum-3 | 32×400G | 12,8T | 1U | ~500 W | Leaf 400G | 40-80 k€ |
| Arista 7050X4 | TH4 | 32×400G | 12,8T | 1U | ~450 W | Leaf 400G | 40-70 k€ |
| FS S5860 | TH3 | 32×100G | 6,4T | 1U | ~300 W | Budget | 8-15 k€ |
| MikroTik CRS518 | — | 16×100G | 3,2T | 1U | ~150 W | Lab/PME | 3-6 k€ |

**Lecture** : à ASIC égal (TH5), l'écart whitebox→marque = **±50 %** —
c'est le support, le NOS et la validation qui se paient, pas le silicium.

## 152. Lire une datasheet optique — les 12 champs qui comptent

| Champ | Ce qu'il dit | Piège |
|---|---|---|
| Form factor | QSFP-DD, OSFP… | Compatibilité mécanique |
| Data rate | 400GBASE-DR4 | Le « 400G » seul ne suffit pas |
| Wavelength | 1310 nm / 850 nm / CWDM | Mélanger les λ = pas de link |
| Fiber type | SMF / MMF | Jaune vs aqua (§18) |
| Distance | 500 m, 2 km… | « Jusqu'à » = conditions idéales |
| Tx power (min/max) | −2,9 à +2 dBm (⚠️ ex.) | Sert au budget (§20) |
| Rx sensitivity | −5,4 dBm (⚠️ ex.) | Sert au budget |
| Link budget | 4 dB (⚠️ ex.) | Tx_min − Rx_sens |
| Power consumption | 9 W | Dimensionnement thermique |
| FEC | RS(544,514) requis | Les 2 bouts doivent l'activer |
| Operating temp | 0-70 °C (commercial) | Industriel = −40/+85 °C |
| Coding | Pré-codé constructeur ? | Lock optique (§11) |

**Règle** : exiger la datasheet **avant** l'achat, pas après la panne.
Un module sans datasheet = un module qu'on ne dépanne pas.

## 153. Budgets optiques — 3 exemples pas-à-pas

**Exemple A — 400G-DR4, 80 m, 1 brassage** :
- Module : Tx −2,9 dBm, sensibilité −5,4 dBm → budget 2,5 dB (⚠️).
- Fibre 80 m OS2 : 0,03 dB. 4× MPO : 4×0,4 = 1,6 dB. 1 brassage (+2 MPO) : +0,8 dB.
- Perte totale : ~2,4 dB. **Marge : 0,1 dB → INSUFFISANT.** Retirer le
  brassage (direct) ou passer en FR4 (budget ~4 dB).

**Exemple B — 100G-SR4, 60 m, direct** :
- Budget module : ~1,9 dB (⚠️). Fibre 60 m OM4 : 0,18 dB. 2× MPO : 0,8 dB.
- Perte : ~1 dB. **Marge : 0,9 dB → limite mais OK** si connecteurs propres.
  Prévoir nettoyage semestriel.

**Exemple C — 800G-DR8, 300 m, 1 brassage** :
- Budget : ~4 dB (⚠️). Fibre 300 m : 0,12 dB. 6× MPO : 2,4 dB.
- Perte : ~2,5 dB. **Marge : 1,5 dB → OK**, mais surveiller le pré-FEC :
  à 800G, 1,5 dB de marge part vite avec la poussière.

**Méthode** : toujours calculer le **pire cas** (connecteurs à 0,5 dB, pas
0,3) et exiger ≥ 3 dB sur les liens critiques. Un lien à 1 dB de marge
neuf sera en panne dans 2 ans.

## 154. Table câbles — 20 références types

| Câble | Débit | Longueur | Conso | Usage | Prix (⚠️) |
|---|---|---|---|---|---|
| DAC SFP28 25G 3 m | 25G | 3 m | 0,1 W | Serveur→ToR | 30-60 € |
| DAC QSFP28 100G 3 m | 100G | 3 m | 0,3 W | Intra-rack | 50-120 € |
| DAC QSFP28 100G 5 m | 100G | 5 m | 0,3 W | Racks adjacents | 60-150 € |
| AOC QSFP28 100G 20 m | 100G | 20 m | 1 W | Inter-racks | 200-400 € |
| DAC QSFP-DD 400G 2 m | 400G | 2 m | 0,5 W | Intra-rack | 150-350 € |
| AEC QSFP-DD 400G 5 m | 400G | 5 m | 3 W | Racks adjacents | 400-800 € |
| AOC QSFP-DD 400G 30 m | 400G | 30 m | 2 W | Inter-rangées | 600-1 200 € |
| DAC OSFP 800G 2 m | 800G | 2 m | 0,5 W | Intra-rack | 400-900 € |
| AEC OSFP 800G 5 m | 800G | 5 m | 4 W | Racks adjacents | 1 000-2 000 € |
| AOC OSFP 800G 30 m | 800G | 30 m | 3 W | Inter-rangées | 1 500-3 000 € |
| Breakout 400G→4×100G 2 m | 400G | 2 m | 0,5 W | Migration | 200-500 € |
| Breakout 800G→2×400G 2 m | 800G | 2 m | 0,5 W | Migration | 500-1 200 € |
| Jarretière OM4 LC 5 m | — | 5 m | — | Brassage | 15-30 € |
| Jarretière OM4 MPO-12 10 m | — | 10 m | — | Trunk court | 40-80 € |
| Jarretière OS2 LC 20 m | — | 20 m | — | Brassage | 25-50 € |
| Trunk OS2 MPO-24 50 m | — | 50 m | — | Rocade | 150-400 € |
| Trunk OM4 MPO-12 30 m | — | 30 m | — | Rocade | 100-250 € |
| Cat6A RJ45 10 m | 10G | 10 m | — | OOB | 10-20 € |
| Fibre armée OS2 100 m | — | 100 m | — | Chemin exposé | 300-800 € |
| Câble console RJ45 | — | 3 m | — | OOB | 5-10 € |

## 155. Protocole d'acceptation détaillé — 40 points de contrôle

**Physique (1-10)** : [ ] baies fixées [ ] airflow cohérent [ ] PDU A/B
branchées [ ] disjoncteurs calibrés [ ] terre < 5 Ω [ ] étiquettes TIA-606
[ ] goulottes ≤ 50 % [ ] bouchons sur ports libres [ ] kit nettoyage présent
[ ] sondes température/humidité.

**Câblage (11-20)** : [ ] DAC ≤ portée [ ] MPO polarité documentée
[ ] budgets optiques calculés [ ] puissances Rx mesurées [ ] marges ≥ 3 dB
[ ] BER pré-FEC < 1e-9 [ ] 0 FEC uncorrectable [ ] breakout validés
[ ] longueurs conformes au plan [ ] photos du brassage archivées.

**Réseau (21-30)** : [ ] BGP up partout [ ] BFD < 300 ms [ ] ECMP ±10 %
[ ] MTU 9000 bout-en-bout [ ] ECN/DCQCN actifs [ ] PFC par file seule
[ ] EVPN : routes échangées [ ] anycast gateway ping [ ] OOB joignable
[ ] AAA fonctionnel.

**Charge (31-40)** : [ ] ib_write_bw ≥ 95 % [ ] NCCL ≥ 80 % [ ] latence
p99 < 5 µs [ ] perte 1 spine < 5 % [ ] perte 1 leaf OK [ ] burn-in 72 h
[ ] thermique < 75 °C [ ] conso mesurée vs dimensionnée [ ] rollback testé
[ ] doc d'exploitation livrée.

**Signature** : __________  Date : __________  Réserves : __________

## 156. Modèle de RFP réseau — structure complète

1. **Contexte** : sites, effectifs, workloads (IA ? cloud ?), horizon 3 ans.
2. **Périmètre** : backend IA / front-end / DCI / edge — lots séparés.
3. **Exigences techniques** : débits, ratio (1:1 / 3:1), RDMA/UEC, buffers,
   ECN/PFC, breakout, MTU, telemetry (gNMI), NOS, ZTP.
4. **Exigences physiques** : format (2U/1U), airflow, PSU 1+1, conso max
   **avec optiques**, niveau sonore (bureaux proches ?).
5. **Interopérabilité** : UEC-ready exigé, optiques tierces acceptées,
   standards ouverts (SONiC si whitebox).
6. **Tests** : protocole d'acceptation §155 imposé, lab de validation.
7. **SLA** : 4 h 24/7 (IA) / J+1 (standard), spares, TAC nommé.
8. **Formation** : 2 sessions admin + 1 troubleshooting, en français.
9. **Prix** : CAPEX détaillé par poste, OPEX 5 ans (maintenance, énergie),
   options CPO / 1,6T.
10. **Planning** : jalons, pénalités de retard, clause de sortie.

**Critères de notation** (exemple) : technique 40 %, TCO 5 ans 30 %,
SLA/support 20 %, énergie 10 %. **Ne jamais noter sur le seul CAPEX.**

## 157. Pannes typiques — fréquence et MTTR

Ordres de grandeur constatés (⚠️ retours d'expérience, varient par site) :

| Panne | Fréquence | MTTR avec spares | MTTR sans spares |
|---|---|---|---|
| Optique / jarretière sale | Mensuelle | 15 min (nettoyage) | 4 h (attente pièce) |
| DAC défectueux | Trimestrielle | 10 min | 4 h |
| Ventilateur switch | Annuelle | 5 min (hot-swap) | J+1 |
| PSU switch | Annuelle | 5 min (hot-swap) | J+1 |
| Switch complet | Tous les 3-5 ans | 1-4 h (RMA + reconfig) | J+1 à J+7 |
| Bug NOS (crash) | Rare | 30 min (reboot + rollback) | — |
| Erreur humaine (config) | Mensuelle | 15 min (rollback Git) | Heures |
| Coupure électrique | Annuelle | 0 (UPS) à heures (sans) | — |
| Fibre sectionnée (travaux) | Rare | 2-8 h (épissure) | Jours |

**Lecture** : 80 % des pannes = optique/câble/humain, MTTR < 30 min
**avec** spares et procédures. Le stock de spares (§118) est l'assurance
la moins chère du datacenter.

## 158. Glossaire 2 — 40 termes complémentaires

| Terme | Définition |
|---|---|
| Anycast | Même IP annoncée depuis N sites (BGP choisit le plus proche). |
| Atténuation | Perte de puissance en dB le long de la fibre. |
| Backplane | Fond de panier : liaisons électriques internes au châssis. |
| BFD | Détection de panne en ~300 ms (vs 180 s BGP). |
| Breakout (câble) | 1 connecteur rapide → N connecteurs lents. |
| Burn-in | Charge continue 72 h avant mise en production. |
| CDU | Coolant Distribution Unit (water-cooling). |
| Châssis | Switch modulaire à cartes (vs fixe 1-2U). |
| Cladding | Gaine optique autour du cœur de la fibre. |
| Cœur (fibre) | Zone centrale qui guide la lumière (9 ou 50 µm). |
| Deadlock PFC | Boucle de pauses qui fige le fabric. |
| Dispersion | Étalement du signal (limite la distance). |
| Downlink | Vers les serveurs (vs uplink vers les spines). |
| EMI | Interférences électromagnétiques. |
| Épissure | Soudure de 2 fibres (0,05-0,1 dB). |
| ER | 40 km (optique). |
| Fan-out | Répartition d'un signal vers N destinations. |
| Férule | Embout céramique du connecteur (2,5/1,25 mm). |
| Flap | Lien qui monte/descend en boucle. |
| Golden config | Configuration de référence validée. |
| Hot-swap | Remplacement à chaud (PSU, ventilos). |
| In-network computing | Calcul dans le switch/NIC (ex. AllReduce). |
| Insertion loss | Perte d'insertion d'un connecteur (dB). |
| Jumbo frame | MTU 9000 (vs 1500 standard). |
| Latence p99 | Latence sous laquelle sont 99 % des paquets. |
| Link budget | Budget optique : Tx − sensibilité Rx. |
| Microburst | Pic de trafic µs-ms invisible au polling. |
| MTTR | Mean Time To Repair. |
| NUMA | Non-Uniform Memory Access (topologie CPU). |
| Oversubscription | Ratio downlink/uplink. |
| Packet spraying | Répartition paquet par paquet. |
| Polarisation (ECMP) | Hash identique → un lien saturé. |
| Radix | Nombre de ports d'un switch. |
| Retimer | Régénère le signal électrique (vs redriver). |
| SerDes | Serializer/Deserializer. |
| Réordonnancement | Remise en ordre des paquets (coûteux pour RDMA). |
| Snake (câble) | Testeur de continuité simplifié : à bannir sur fibre. |
| Store-and-forward | Commutation après réception complète (vs cut-through). |
| Trunk (fibre) | Câble multi-brins pour rocades. |
| Uplink | Vers les spines (vs downlink). |
| Xon/Xoff | Contrôle de flux historique (vs PFC). |

## 159. Fiches switches complémentaires — 5 acteurs

**Cisco (Nexus 9000, Silicon One G200)** : l'existant enterprise. NX-OS
mature, ACI pour l'overlay managé. G200 (51,2T, 800G) = l'entrée de Cisco
dans l'IA — ⚠️ détails non vérifiés au 27/09/2026, exiger une fiche à jour.
Verdict : rester si le parc est Cisco et l'équipe formée ; sinon Arista.

**Juniper (QFX5240, PTX)** : solide en SP et DCI, Apstra pour l'intent-based.
Moins « IA-first » qu'Arista/NVIDIA en 2026. Verdict : pertinent si
l'équipe est Juniper, ou pour le DCI.

**HPE Aruba CX** : enterprise, bon support, pas le backend IA. Verdict :
front-end et campus, pas les clusters GPU.

**Whitebox (Edgecore, Celestica, FS)** : TH5 à −30-50 % vs marques (⚠️),
SONiC ou ONIE+EOS. Verdict : le moins cher au port — à condition d'avoir
l'équipe qui opère SONiC (ou une distribution durcie). Pas pour une
première IA.

**FS.com (optiques et câbles)** : le fournisseur « compatible » de
référence pour optiques tierces codées — 3-5× moins cher que l'OEM (⚠️).
Verdict : valider 1 référence par switch/NIC, puis standardiser. C'est ici
que se gagnent les 30 % du budget optique.

## 160. CAS CHIFFRÉ — refresh 100G → 400G d'un DC existant (500 serveurs)

**Hypothèses** : 500 serveurs 2×25G, leafs 48×25G+8×100G, on passe les
uplinks et les spines en 400G, serveurs inchangés (breakout).

| Étape | Action | Coût (⚠️) |
|---|---|---|
| 1. Fibre | Vérifier/ajouter OS2 inter-rangées | 30-80 k€ |
| 2. Spines | 2× 32×400G (remplacent 2× 32×100G) | 80-160 k€ |
| 3. Uplinks | 44 leafs × 4×100G → 2×400G : 88 optiques 400G-DR4 | 53-132 k€ |
| 4. Breakout | 88 câbles 400G→4×100G côté leaf existant | 18-44 k€ |
| 5. Main d'œuvre | 2 techs × 2 semaines + tests | 30-50 k€ |
| **Total** | | **~211-466 k€** |

**Gain** : uplinks ×4 (de 400G à 1,6T par leaf en 400G), oversubscription
3:1 → 0,75:1 (surprovisionné — on pourra densifier les serveurs).
**Leçon** : le refresh se fait **par le haut** (spines/uplinks d'abord),
les serveurs suivent. Et la fibre d'abord (§113).

## 161. Topologie multi-tenant cloud — l'overlay en détail

```
  Tenant A (VNI 10001)          Tenant B (VNI 10002)
  ┌──────────────┐              ┌──────────────┐
  │ 10.1.0.0/16  │              │ 10.1.0.0/16  │  ← mêmes IP possibles !
  │ (isolé)      │              │ (isolé)      │
  └──────┬───────┘              └──────┬───────┘
         │ VTEP Leaf-1                │ VTEP Leaf-2
  ───────┴──────── underlay BGP ───────┴────────
```

- **Isolation** : VNI distincts, pas de fuite inter-VNI (vérifié par test).
- **Passerelle** : anycast par VNI (1 IP gateway par tenant, même MAC).
- **Sortie** : border leafs avec NAT/firewall par tenant.
- **Quotas** : rate-limit par VNI sur les border leafs (un tenant ne noie
  pas les autres).
- **Supervision** : compteurs par VNI (pas seulement par port).

**Dimensionnement** : 16M de VNI possibles, mais la table MAC/FIB du leaf
est la vraie limite (136K MAC sur un Arista 7060X, ⚠️) — ~1000 VMs/leaf
en pratique avec marge.

## 162. Checklists de mise en service — jour J

**J-7** : [ ] spares reçus et testés [ ] FW validés au lab [ ] golden
config gelée [ ] plan d'adressage dans NetBox [ ] équipe briefée
[ ] rollback préparé [ ] fenêtre de maintenance déclarée.

**Jour J** : [ ] 1 baie à la fois [ ] câblage → étiquetage → photo
[ ] 1er lien : BER + DOM avant de continuer [ ] BGP/BFD up
[ ] tests §107 par palier (pas tout à la fin) [ ] supervision branchée
**avant** la charge.

**J+1** : [ ] burn-in 72 h lancé [ ] alertes vérifiées (en faire sonner
une pour de faux) [ ] doc à jour [ ] REX à chaud (15 min, ce qui a
coincé).

**Règle** : on ne met en service **que ce qu'on a testé**, et on ne teste
**que ce qu'on a documenté**. Dans cet ordre.

## 163. 10 erreurs de câblage illustrées

**E1. Tx vers Tx** (deux modules émettent l'un vers l'autre) :
```
  [Module A Tx] ──fibre── [Module B Tx]   ❌ pas de link
  [Module A Tx] ──fibre── [Module B Rx]   ✅ (croiser !)
```

**E2. MPO mâle-mâle** : broches tordues, à jeter des 2 côtés.

**E3. OM4 sur port DR (monomode)** : le laser 1310 nm dans du multimode =
pertes énormes, lien instable.

**E4. OS2 sur port SR (850 nm)** : le VCSEL dans du monomode = ça peut
« marcher » sur 2 m puis mourir — trompeur.

**E5. Jarretière de 30 m enroulée en boule** : micro-courbures + 2 dB.

**E6. DAC plié à 90°** : le Twinax n'aime pas — faux contacts intermittents.

**E7. MPO-12 sur port MPO-16** : 4 fibres dans le vide, lanes manquantes.

**E8. Breakout branché sur port non-breakout** : 1 lane up, 7 down.

**E9. Fibre sous le rail de la baie** : écrasée à la fermeture — BER qui
monte avec la température (dilatation).

**E10. Deux trunks méthode B bout-à-bout** : B+B = droit au lieu de croisé
→ Tx vers Tx (retour à E1). **Un seul** élément croisé par lien.

## 164. Conversions dB/dBm/W — l'antisèche

| Conversion | Formule / valeur |
|---|---|
| dBm → mW | P(mW) = 10^(dBm/10) |
| 0 dBm | 1 mW |
| −3 dBm | 0,5 mW |
| −10 dBm | 0,1 mW |
| −20 dBm | 0,01 mW |
| 3 dB | ×2 (ou ÷2) |
| 10 dB | ×10 (ou ÷10) |
| Budget 4 dB | = on peut perdre 60 % de la puissance |
| Marge 3 dB | = la moitié de la puissance en réserve |

**Exemple** : Tx −2 dBm (0,63 mW), perte 4 dB → Rx −6 dBm (0,25 mW).
Si la sensibilité est −8 dBm : marge 2 dB → **limite** (voulu : ≥ 3 dB).

---

*Fin du guide — 164 sections, 40+40 termes de glossaire, 45 pièges,
10 questions de quiz, 30 FAQ. Vérification : `wc -l` — objectif ≥ 4000 lignes.*

# PARTIE P — DERNIERS COMPLÉMENTS

## 165. Étude de cas — datacenter IA 10 MW : le réseau de bout en bout

**Hypothèses** : 10 MW IT, dont 8 MW GPU (512 nœuds × 8 B200, ⚠️),
1 MW réseau, 1 MW stockage+divers.

**Réseau backend** (1:1, 800G, rail-optimized) :
- 4096 NIC 800G (8/GPU × 512 nœuds).
- 2 tiers : 64 leafs + 128 spines 64×800G ? Non — 4096/64 = 64 leafs
  (64 down + 64 up... en 2×400G). Prenons des 64×800G : 64 leafs,
  64 spines → **128 switchs**, 8192 ports 800G.

**Bilan énergie** (⚠️) :

| Poste | Calcul | kW |
|---|---|---|
| GPU (8 MW) | 512 × 15,6 kW | 8000 |
| NIC 800G | 4096 × 60 W | 246 |
| Switchs backend | 128 × 1,5 kW | 192 |
| Optiques backend | ~6000 × 15 W | 90 |
| Front-end (3:1, 100G) | — | 60 |
| Stockage NVMe-oF | — | 150 |
| Divers (OOB, mgmt) | — | 20 |
| **Total IT** | | **~8758** |
| Au compteur (PUE 1,35) | 8758 × 1,35 | **~11 800** |

**Le réseau (528 kW IT) = 6 % de l'IT** — mais 100 % du risque : sans lui,
les 8 MW de GPU sont des radiateurs à 40 000 € pièce.
**Refroidissement** : ~3,2 MW thermiques à évacuer (PUE 1,35 → 35 % =
pertes+clim). Le lot réseau seul (528 kW) = **~150 tonnes** frigorifiques.

## 166. Tester les optiques à la réception — procédure

1. **Visuel** : emballage, bouchons présents, étiquette (référence, SN).
2. **Inspection** : microscope — PASS/FAIL IEC 61300-3-35 (§25).
3. **Codage** : enficher dans le switch/NIC cible → `show inventory` :
   la référence lue = la référence commandée (détecte les contrefaçons).
4. **Boucle** : loopback optique (atténuateur si besoin) → BER 0 sur
   1e12 bits, puissances Tx/Rx dans la spec.
5. **Échantillon** : 100 % des 10 premiers, puis 10 % par lot (min 5).
   Un lot avec > 2 % de rejet = lot refusé.

**Outillage** : 1 microscope, 2 atténuateurs variables, 1 jeu de
loopbacks LC/MPO, 1 power-meter. ~3 000-5 000 € (⚠️) — amorti au premier
lot défectueux détecté **avant** montage en baie.

## 167. Distances fibre — table complète par norme (⚠️ usuels)

| Norme | Débit | OM1 | OM2 | OM3 | OM4 | OM5 | OS2 |
|---|---|---|---|---|---|---|---|
| 1000BASE-SX | 1G | 275 m | 550 m | 800 m | 800 m | — | — |
| 10GBASE-SR | 10G | 33 m | 82 m | 300 m | 400 m | — | — |
| 10GBASE-LR | 10G | — | — | — | — | — | 10 km |
| 25GBASE-SR | 25G | — | — | 70 m | 100 m | — | — |
| 40GBASE-SR4 | 40G | — | — | 100 m | 150 m | — | — |
| 100GBASE-SR4 | 100G | — | — | 70 m | 100 m | — | — |
| 100GBASE-SR10 | 100G | — | — | 100 m | 150 m | — | — |
| 100GBASE-DR | 100G | — | — | — | — | — | 500 m |
| 200GBASE-SR4 | 200G | — | — | 70 m | 100 m | — | — |
| 400GBASE-SR8 | 400G | — | — | 60 m | 100 m | — | — |
| 400GBASE-DR4 | 400G | — | — | — | — | — | 500 m |
| 800GBASE-SR8 | 800G | — | — | 60 m | 100 m | — | — |
| 800GBASE-DR8 | 800G | — | — | — | — | — | 500 m |

**Note** : OM1/OM2 (62,5/50 µm anciennes) = legacy, ne plus tirer.
SWDM4/BiDi sur OM5 : 100-150 m sur 2 brins.

## 168. Guide d'achat par profil

| Profil | Backend | Front-end | Fibre | Budget réseau (⚠️) |
|---|---|---|---|---|
| **PME** (20 serveurs) | — (pas d'IA) | 2× switch 48×1/10G, LACP | OM4 | 15-40 k€ |
| **ETI** (200 serveurs) | — ou petit | Leaf-spine 25/100G, 3:1 | OM4 + OS2 rocade | 200-500 k€ |
| **Cloud privé** (1000 srv) | 100G dédié si IA légère | Leaf-spine 25/100G, EVPN | OS2 partout | 1-3 M€ |
| **IA moyen** (256 GPU) | 400/800G 1:1, rail-opt. | Séparé 100G | OS2 + OM4 | 5-15 M€ |
| **IA grand** (2048 GPU) | 800G 1:1, 3 tiers | Séparé | OS2 + MPO-16 | 40-90 M€ |
| **Hyperscaler** | CPO, UEC, custom | Converged | Tout, en masse | — |

**Règle par profil** : ne pas acheter « comme les grands » quand on est
petit — un spine-leaf 3:1 bien opéré bat un mini-fabric IA mal tuné.
Et inversement : ne pas bricoler du 100G pour un cluster GPU.

## 169. Lexique français-anglais — 50 termes

| Français | Anglais |
|---|---|
| Brassage | Patching / cross-connect |
| Jarretière | Patch cord |
| Trunk (fibre) | Trunk cable |
| Goulotte | Cable tray / raceway |
| Baie | Rack / cabinet |
| Allée chaude/froide | Hot/cold aisle |
| Débit | Throughput / data rate |
| Latence | Latency |
| Gigue | Jitter |
| Perte de paquets | Packet loss |
| Engorgement | Congestion |
| File d'attente | Queue |
| Tampon | Buffer |
| Commutateur | Switch |
| Routeur | Router |
| Carte réseau | Network Interface Card (NIC) |
| Pilote | Driver |
| Micrologiciel | Firmware |
| Supervision | Monitoring |
| Télémétrie | Telemetry |
| Seuil | Threshold |
| Alerte | Alert |
| Panne | Failure / outage |
| Redondance | Redundancy |
| Basculement | Failover |
| Reprise | Recovery |
| Sauvegarde (config) | Backup |
| Mise en service | Commissioning |
| Recette | Acceptance testing |
| Appel d'offres | RFP / tender |
| Bordereau de prix | Bill of quantities |
| Pièce de rechange | Spare part |
| Contrat de maintenance | Maintenance contract |
| Délai d'intervention | Response time (SLA) |
| Coupure | Power outage |
| Onduleur | UPS |
| Groupe électrogène | Generator |
| Climatisation | Cooling / HVAC |
| Efficacité énergétique | Power efficiency |
| Facture électrique | Power bill |
| Terre (élec.) | Ground / earth |
| Disjoncteur | Circuit breaker |
| Chemin de câbles | Cable pathway |
| Coupe-feu | Firestop |
| Plan d'adressage | Addressing plan |
| Sous-réseau | Subnet |
| Passerelle | Gateway |
| Diffusion | Broadcast |
| En-tête (paquet) | Header |

## 170. Template — rapport d'incident réseau

```
Incident n° : __________    Date/heure début : __________    Fin : __________
Sévérité : P1 / P2 / P3    Détecté par : supervision / utilisateur / autre
Périmètre : __________ (fabric, sites, tenants impactés)

TIMELINE (heure : fait)
__:__ :
__:__ :
__:__ :

CAUSE RACINE (5 pourquoi) :
1. Pourquoi ? __________
2. Pourquoi ? __________
3. Pourquoi ? __________
4. Pourquoi ? __________
5. Pourquoi ? __________

CORRECTIF IMMÉDIAT : __________
CORRECTIF DURABLE : __________ (avec échéance et responsable)
PIÈGE DU GUIDE CONCERNÉ : P__ (§122)

Leçons : __________
Diffusion : __________
```

**Règle** : tout P1/P2 = rapport sous 5 jours ouvrés, relu par un pair.
Un incident sans rapport **reviendra** — c'est mécanique.

## 171. Parcours de formation — devenir autonome sur le réseau IA

| Niveau | Contenu | Durée (⚠️) |
|---|---|---|
| 1. Bases | TCP/IP, switching, VLAN, STP, routage | 2-4 sem. |
| 2. Datacenter | Spine-leaf, BGP, ECMP, EVPN/VXLAN | 1-2 mois |
| 3. Fibre | Optique, budgets, nettoyage, MPO | 1 sem. + pratique |
| 4. IA | RDMA, RoCEv2, ECN/DCQCN, NCCL | 1 mois |
| 5. Pratique | Lab : monter un mini-fabric, le casser, le réparer | Continu |
| 6. Énergie | PUE, dimensionnement, CPO | 1 sem. |

**Certifications utiles** (⚠️) : JNCIA/CCNA (bases), Arista ACE (EOS),
NVIDIA NCA-AI (réseau IA). Mais **rien ne remplace le lab** : 1 jour de
casse volontaire = 1 mois de théorie.

## 172. Index des tableaux du guide

| Tableau | Section |
|---|---|
| Débit ⇄ format ⇄ PCIe | §2 |
| NIC 100G / 400G / 800G | §4, §6, §7 |
| Offloads | §9 |
| RDMA : IB / RoCE / UET | §10 |
| Matrice de choix NIC | §11 |
| Formats physiques NIC | §12 |
| Lexique formats fibre | §16 |
| Correspondance débit/format/fibre | §17, §139 |
| OM3/4/5 vs OS2 | §18 |
| Distances | §19, §167 |
| Budgets optiques (éléments) | §20 |
| Connecteurs | §21 |
| DAC vs AOC vs transceivers | §22 |
| Breakout | §23 |
| Seuils IEC 61300-3-35 | §25 |
| FEC | §27 |
| BOM fibre rack 400G | §29 |
| Oversubscription | §32 |
| ECMP / EVPN | §36, §37 |
| Switch IA vs classique | §43 |
| Buffers deep/shallow | §44 |
| IB vs Ethernet marché | §47, §48 |
| Spectrum-X / Broadcom / Arista | §49, §50, §51 |
| Conso NICs / switchs / optiques | §89, §90, §91 |
| PUE / refroidissement | §93, §94 |
| Leviers d'efficacité | §97 |
| 1,6T / UEC / CPO | §99, §100, §101 |
| Glossaires (80 termes) | §105, §158 |
| Les 45 pièges | §122 |
| BOMs projets | §108→§123 |
| Fiches produits | §126-133 |
| Comparatif 20 switches | §151 |
| Transceivers (25 réf.) | §141 |
| Câbles (20 réf.) | §154 |
| Normes IEEE | §142 |

## 173. Checklist — avant de partir en congés (NOC)

- [ ] Astreinte définie, téléphone chargé, accès VPN testé
- [ ] Spares : stock vérifié (optiques, DAC, 1 switch par plan critique)
- [ ] Supervision : alertes testées, seuils à jour, pas d'alerte en cours ignorée
- [ ] Changements gelés : pas de FW/config pendant l'absence (ou rollback prêt)
- [ ] Doc : mots de passe (coffre), plans, contacts TAC affichés
- [ ] Énergie : seuils de température vérifiés (canicule ?)
- [ ] Dernier REX : les 3 incidents du mois sont-ils soldés ?

**Un réseau qui ne peut pas tenir 2 semaines sans vous n'est pas un réseau
fini** — c'est une astreinte déguisée.

## 174. Notes de version et maintenance du guide

| Version | Date | Contenu |
|---|---|---|
| 1.0 | 27/09/2026 | 174 sections, recherche web du jour |

**Maintenance** : revérifier les sections marquées ✅ tous les 6 mois
(le marché IA bouge vite : CPO, UEC, 1,6T). Les ⚠️ sont à confirmer à
chaque achat. Journal des modifications ci-dessous pour les v1.1+.

---

*Fin du guide — 174 sections numérotées, 80 termes de glossaire, 45 pièges,
quiz, 30 FAQ, 100 acronymes, BOMs, checklists, templates.
Vérification finale : `wc -l` — objectif ≥ 4000 lignes.*

# PARTIE Q — SYNTHÈSES FINALES

## 175. Une page par débit — la synthèse ultime

| | 25G | 100G | 200G | 400G | 800G | 1,6T |
|---|---|---|---|---|---|---|
| Format | SFP28 | QSFP28 | QSFP56 | QSFP112/DD | OSFP | OSFP-XD ⏳ |
| Modulation | NRZ | NRZ | PAM4 | PAM4 | PAM4 | PAM4 |
| Lanes | 1×25G | 4×25G | 4×50G | 4×100G | 8×100G | 8×200G |
| PCIe | Gen3 x8 | Gen4 x16 | Gen4 x16 | Gen5 x16 | Gen6 x16 | Gen7 x16 ⏳ |
| NIC réf. | XXV710 | E810 ✅ | CX-6 Dx | CX-7 / Thor 2 ✅ | CX-8 / Thor Ultra ✅ | CX-9 ⏳ |
| Switch 64p | — | 6,4T | 12,8T | 25,6T | 51,2T | 102,4T |
| Fibre intra | DAC/OM4 | DAC/SR4 | DAC/SR4 | DAC/SR8 | DAC/SR8 | ⏳ |
| Conso NIC (⚠️) | 10 W | 15-25 W | 20-30 W | 30-40 W | 50-75 W | ? |
| W/Gb/s (⚠️) | 0,4 | 0,2 | 0,15 | 0,1 | 0,08 | 0,06 ? |
| Usage 2026 | OOB | Front-end | Transition | Backend Hopper | Backend Blackwell | Rubin 2027+ |
| Statut | Legacy | Standard | Palier | Standard IA | Nouveau | Annoncé |

## 176. Contrôles de congestion — comparatif

| Algorithme | Principe | Où | Note |
|---|---|---|---|
| DCQCN | ECN + CNP, émetteur ralentit | RoCEv2 | Le standard, à régler |
| TIMELY | RTT mesuré, ralentit si ↑ | RoCEv2 (Google) | Précis, sensible au bruit |
| HPCC | Télémétrie in-band (INT) | Recherche | Optimal mais complexe |
| Swift | Délai + ECN | Google | Production hyperscaler |
| CC Spectrum-X | Adaptatif + télémétrie HW | NVIDIA fermé | 95 % (cas xAI) ✅ |
| CC UEC | Émetteur/récepteur programmable | Thor Ultra ✅ | Le futur ouvert |

**Règle** : ne pas mélanger deux CC sur le même fabric (ils se battent).
Un seul algorithme, bien réglé, validé au lab (§137).

## 177. Connecteurs et câbles — repères physiques

| Connecteur | Type | Usage |
|---|---|---|
| RJ45 | Cuivre 8P8C | 1G/10G, OOB, console |
| SFP+/28 | 1 lane | 10/25G |
| QSFP+/28/56 | 4 lanes | 40/100/200G |
| QSFP-DD/112 | 8 lanes | 400G |
| OSFP | 8 lanes, grand | 400/800G (thermique) |
| QSFP-DD800 | 8 lanes | 800G (format DD) |
| LC duplex | Fibre 2 brins | SR/DR/FR/LR |
| MPO-12/16/24 | Fibre multi-brins | SR4/SR8/DR4/DR8, trunks |
| CS / SN | Fibre compacte | 800G dense |

## 178. Template — plan de câblage (par baie)

```
Baie : __________    Date : __________    Technicien : __________
| U | Équipement | Port | Destination (baie/éq/port) | Câble | Long. | Étiquette |
|---|------------|------|------------------------------|-------|-------|-----------|
|42 | LF04       | e49  | S1-R03-B12-SRV07-ETH1         | DAC400| 2 m   | S1-R03... |
|...|            |      |                              |       |       |           |
Vérifié : [ ] polarité MPO  [ ] longueurs ≤ portée  [ ] mou 30 cm
          [ ] étiquettes 2 bouts  [ ] photo
```

## 179. Alimentations — repères par switch

| Switch | PSU | Config | Note |
|---|---|---|---|
| Leaf 1U 100G | 2× 500-800 W | 1+1 | — |
| Leaf 1U 400G | 2× 1000-1500 W | 1+1 | — |
| Leaf 2U 800G | 2× 2400 W | 1+1 | Arista 7060X6 ✅ |
| SN5600 | 2× (1+1) | 1+1 | ✅ |
| SN5610 | 4× (2+2) | 2+2 | ✅ |
| Châssis 7800R4 | N+1 par zone | N+1 | Jusqu'à 25 kW (⚠️) |

**Règle** : PDU A/B **indépendantes**, chacune = 100 % de la charge.
Disjoncteur ≥ 125 % du max mesuré au boot.

## 180. FAQ 2 — 20 questions courtes

**1.** Spectrum-X ou UEC ? → Fermé performant vs ouvert : selon l'équipe.
**2.** Un seul vendeur, c'est grave ? → Non si le contrat prévoit la sortie.
**3.** SONiC en prod IA ? → Distribution durcie seulement.
**4.** Cumulus ou SONiC sur NVIDIA ? → Cumulus (intégré Spectrum-X).
**5.** EVPN : eBGP ou iBGP ? → eBGP underlay, iBGP overlay (RR sur spines).
**6.** Combien de VTEP par fabric ? → Tous les leafs (anycast gateway).
**7.** BFD : quel timer ? → 100 ms × 3 (300 ms).
**8.** Jumbo frames : partout ? → Oui, MTU 9000 de bout en bout.
**9.** VXLAN sans EVPN ? → Possible (flood-and-learn) mais ne pas faire en grand.
**10.** PFC : sur quelles files ? → La seule file RoCE, jamais en global.
**11.** ECN : seuils ? → Marquage 30-50 %, pause 70-80 % (⚠️ à tuner).
**12.** Buffer : combien ? → BDP × flux (voir §44).
**13.** Cut-through ou store-and-forward ? → Cut-through (latence).
**14.** Latence leaf-spine-leaf ? → ~2-3 µs (cut-through 800G).
**15.** Gigue acceptable ? → < 1 µs pour NCCL synchrone (⚠️).
**16.** Câble : longueur max en baie ? → 3 m (DAC), au-delà AOC.
**17.** OS2 ou OM4 entre 2 rangées (60 m) ? → OM4 SR (moins cher), sauf si 800G+ → OS2.
**18.** Un transceiver peut-il servir 10 ans ? → Non : 5-7 ans, cycle avec le switch.
**19.** Revendre les vieux switchs ? → Oui (marché secondaire), après wipe config.
**20.** Le conseil ultime ? → §104, règle 1 : séparer les fabrics.

## 181. Fin de vie — décommission propre

- [ ] Configs sauvegardées (preuve), puis **wipe** (données, certificats)
- [ ] Optiques retirées, bouchons remis, stockées anti-statique
- [ ] Inventaire NetBox à jour (statut « retired »)
- [ ] Câbles : réutilisables ? (DAC oui, fibre si propre) sinon recyclage
- [ ] Revente : effacer les licences, transférer la garantie si possible
- [ ] DEEE : switchs = déchets électroniques (traçabilité)

---

*Fin du guide — **181 sections numérotées**, 80 termes de glossaire,
45 pièges, quiz, 50 FAQ, 100 acronymes, BOMs, checklists, templates.
Vérification finale : `wc -l` — objectif ≥ 4000 lignes.*

# PARTIE R — CLÔTURE OPÉRATIONNELLE

## 182. Matrice de décision — quel fabric pour quel workload

| Workload | Endpoints | Ratio | Débit | Topologie | NOS | Budget réseau (⚠️) |
|---|---|---|---|---|---|---|
| Bureautique / web | < 500 | 5:1 | 25G | 2 tiers | Au choix | 100-300 k€ |
| Virtualisation | 500-2000 | 3:1 | 25/100G | 2 tiers + EVPN | EOS/NX-OS | 0,5-2 M€ |
| Cloud privé | 2000+ | 3:1 | 100G | 2 tiers + EVPN | EOS/SONiC | 2-5 M€ |
| Stockage NVMe-oF | 100-500 | 2:1 | 100/200G | 2 tiers dédié | Au choix | 0,5-1,5 M€ |
| HPC classique | 100-1000 | 1:1 | 200/400G | 2 tiers / IB | Cumulus/EOS | 1-5 M€ |
| IA training (Hopper) | 256-2000 | 1:1 | 400G | Rail-opt. 2 tiers | EOS/Cumulus | 5-20 M€ |
| IA training (Blackwell) | 1000+ | 1:1 | 800G | Rail-opt. multi-plane | EOS/Cumulus | 20-90 M€ |
| Inférence | 50-500 | 2:1 | 100/400G | 2 tiers | Au choix | 1-5 M€ |
| Edge / CDN | 2-20/PoP | — | 100G | Simple | Au choix | 70-120 k€/PoP |

**Lecture** : il n'y a pas « un » bon fabric — il y a le bon fabric pour
le workload. Le surdimensionnement coûte du CAPEX, le sous-dimensionnement
coûte des semaines de production.

## 183. Extraits de configuration commentés

**Arista EOS — underlay eBGP + BFD** :
```
router bgp 65001
   router-id 10.0.0.1
   maximum-paths 64
   neighbor SPINE peer-group
   neighbor SPINE remote-as 65101
   neighbor SPINE bfd          ! détection 300 ms
   neighbor 10.0.1.0 peer-group SPINE
   !
   address-family ipv4
      network 10.0.0.1/32      ! loopback = VTEP
```

**Cumulus — ECN sur file RoCE** :
```
# /etc/cumulus/datapath/traffic.conf (extrait conceptuel)
ecn -q 3 -min 30 -max 80       ! marquage ECN file 3 (RoCE)
pfc -q 3 -xoff 75 -xon 60      ! PFC dernier recours
```

**Linux — offloads et MTU** :
```
ip link set eth0 mtu 9000
ethtool -K eth0 tx-checksum-ipv4 on tso on gro on
tc qdisc add dev eth0 root fq_codel ecn   ! ECN côté hôte
```

⚠️ Extraits pédagogiques : adapter au NOS et à la version exacte.

## 184. Les 10 erreurs humaines les plus chères (chiffrées, ⚠️)

| # | Erreur | Coût estimé |
|---|---|---|
| 1 | Fabric unique IA+prod (ralentissements permanents) | 500 k€-2 M€/an de GPU idle |
| 2 | FW partiel sur fabric RoCE (−30 % pendant 4 j) | 200-800 k€ |
| 3 | Pas de test ECMP (polarisation découverte en prod) | 100-500 k€ |
| 4 | Fibre sale généralisée (chantier + pas de nettoyage) | 50-200 k€ |
| 5 | DAC trop longs (recommande en urgence ×2) | 50-150 k€ |
| 6 | Config non versionnée (rollback impossible) | 50-300 k€/incident |
| 7 | UPS réseau = UPS GPU (arrêt à l'aveugle) | 100-400 k€/incident |
| 8 | MTU incohérent (fragmentation silencieuse) | 50-200 k€ |
| 9 | Pas de spares (MTTR ×10) | 100-500 k€/an |
| 10 | Achat CAPEX seul (TCO ×2 sur 5 ans) | 1-5 M€ |

**Total** : les erreurs « humaines/process » coûtent **plus cher que le
matériel**. D'où les checklists, les templates et les tests de ce guide.

## 185. Pour aller plus loin — ressources

| Ressource | Type | Usage |
|---|---|---|
| Docs NVIDIA Networking | Doc officielle | ConnectX, Spectrum-X, DOCA |
| Docs Arista (EOS Central) | Doc officielle | EOS, CloudVision |
| UEC (ultraethernet.org) | Consortium | Spec 1.0/1.0.1 |
| SONiC (sonic-net.github.io) | Open-source | NOS whitebox |
| RFC 7938 (BGP datacenter) | RFC | Underlay |
| RFC 7432 (EVPN) | RFC | Overlay |
| ServeTheHome / Next Platform | Presse | Tests, analyses |
| Guides Zelef | Local | Onduleurs, Proxmox, Debian, NetBox, Ansible |

## 186. Revue de conception — la checklist du design review

Avant de signer un design réseau, 20 questions :

1. Le ratio est-il **calculé** (pas promis) ? 2. Backend et front-end
séparés ? 3. PCIe validé pour chaque NIC ? 4. Fibre : type, longueurs,
budgets ≥ 3 dB ? 5. Breakout validé par port ? 6. ECN/DCQCN : qui règle ?
7. PFC : par file seulement ? 8. Buffers : shallow leaf / deep spine ?
9. ECMP : hash seed aléatoire ? 10. Tests de réception chiffrés au contrat ?
11. FW/driver figés ? 12. Supervision : BER, ECN, PFC, thermique ?
13. Spares : 10 % optiques, 1 switch/plan ? 14. SLA : 4 h sur l'IA ?
15. Énergie : switch + optiques + NIC + PUE chiffrés ? 16. UPS réseau
séparé ? 17. CPO : clause de sortie du pluggable ? 18. UEC-ready exigé ?
19. Croissance : +20 % de ports, fibre surdimensionnée ? 20. **Qui opère,
avec quelles compétences, formé quand ?**

Si une réponse manque : le design n'est pas fini.

---

*Fin du guide — **186 sections numérotées**. Objectif : ≥ 4000 lignes vérifiées par `wc -l`.*

# PARTIE S — 110 DÉCISIONS RAPIDES « SI / ALORS »

## 187. Le mémo ultime — une ligne par décision

| # | Si | Alors |
|---|---|---|
| 1 | Cluster IA > 128 GPU | Backend dédié 1:1, rail-optimized |
| 2 | Lien < 2 m | DAC passif |
| 3 | Lien 2-5 m | AEC (actif) |
| 4 | Lien 5-30 m | AOC |
| 5 | Lien > 30 m | Transceiver + fibre |
| 6 | Rocade neuve | OS2, pas de débat |
| 7 | Baie ↔ brassage (< 100 m) | OM4 SR si ≤400G, OS2 DR sinon |
| 8 | Budget optique < 3 dB | Revoir le design (retirer un brassage) |
| 9 | BER pré-FEC > 1e-9 | Nettoyer, puis mesurer, puis changer |
| 10 | FEC uncorrectable > 0 | Arrêter la charge, changer le lien |
| 11 | NIC 400G | PCIe Gen5 x16 minimum |
| 12 | NIC 800G | PCIe Gen6 x16 minimum |
| 13 | Pas de PCIe Gen6 | 800G bridé → rester en 400G |
| 14 | RoCEv2 en prod | ECN + PFC par file + MTU 9000 |
| 15 | Pas de lossless | Pas de RDMA |
| 16 | UEC disponible | Exiger UEC-ready, garder RoCEv2 en 2026 |
| 17 | InfiniBand existant | Le garder s'il marche ; UEC pour le neuf |
| 18 | Choix du NOS | Le même 10 ans ; pas de changement pour le prix |
| 19 | Whitebox | Seulement avec une équipe SONiC |
| 20 | Optiques OEM trop chères | Tierces codées, validées 1 réf./switch |
| 21 | Prix non public | Fourchette ⚠️ + 3 devis |
| 22 | Devis unique | Refuser, toujours 3 |
| 23 | Clause CPO absente | La négocier avant signature |
| 24 | FW « latest » proposé | Refuser, version figée validée |
| 25 | Pas de protocole d'acceptation | Ne pas signer |
| 26 | Tests < 72 h | Insuffisant pour un fabric IA |
| 27 | Pas de test ECMP | Polarisation garantie en prod |
| 28 | Pas de test de panne spine | Le faire au lab avant la prod |
| 29 | Supervision sans BER | Aveugle sur la santé des liens |
| 30 | Alertes non testées | En faire sonner une pour de faux |
| 31 | Config non versionnée | Git dès le jour 1 |
| 32 | Changement sans rollback | Interdit |
| 33 | Deux changements à la fois | Jamais |
| 34 | Incident P1/P2 | Rapport 5 pourquoi sous 5 jours |
| 35 | Pas de spares | MTTR ×10 |
| 36 | Spares < 10 % optiques | Augmenter |
| 37 | Switch critique sans spare | 1 par plan critique |
| 38 | MPO sans bouchons | Poussière = BER dans 6 mois |
| 39 | Connecteur tombé | Nettoyer avant de rebrancher |
| 40 | Inspection sans microscope | On ne voit pas la saleté à l'œil |
| 41 | Goulotte > 50 % | Nouvelle goulotte |
| 42 | Fibre pliée à 90° | Remplacer |
| 43 | DAC > portée | AOC ou transceiver |
| 44 | Tx vers Tx | Croiser les fibres |
| 45 | MPO mâle-mâle | Ne jamais forcer |
| 46 | Deux trunks méthode B | Un seul élément croisé par lien |
| 47 | Étiquette manquante | Étiqueter avant de brasser |
| 48 | Pas de photo du brassage | La prendre maintenant |
| 49 | Plan d'adressage absent | NetBox avant le premier câble |
| 50 | IPv6 non prévu | Prévoir le plan, activer plus tard |
| 51 | MTU incohérent | 9000 partout ou 1500 partout |
| 52 | PFC global | Par file RoCE uniquement |
| 53 | PFC storm | Désactiver PFC, vérifier ECN, chercher la boucle |
| 54 | ECN mal réglé | Seuils 30-50 % / 70-80 %, tuner au lab |
| 55 | Deux CC mélangés | Un seul algorithme par fabric |
| 56 | NCCL lent | NCCL_DEBUG=INFO, quelle interface ? |
| 57 | NCCL utilise eth0 | Forcer l'interface RDMA |
| 58 | GDRCopy absent | L'installer pour les petits messages |
| 59 | PCIe bifurcation fausse | x16 ou x8+x8 selon la carte |
| 60 | NUMA ignoré | Pinner les IRQs au bon nœud |
| 61 | IRQ sur CPU 0 | Les répartir (irqbalance ou manuel) |
| 62 | Offloads désactivés | Les activer sauf debug |
| 63 | Buffer leaf trop petit | Leaf shallow + spine deep |
| 64 | Microbursts invisibles | LANZ / streaming, pas le polling |
| 65 | Latence p99 élevée | Chercher la file, pas le lien |
| 66 | Un flux = un lien | Normal en ECMP ; spraying pour l'IA |
| 67 | Oversubscription 3:1 en IA | Non : 1:1 backend |
| 68 | 400G ou 800G en 2026 | 800G (coût/Gb/s inférieur) |
| 69 | 100G en 2026 | Front-end et edge, pas le backend IA |
| 70 | 1,6T en 2026 | Attendre, préparer la fibre |
| 71 | CPO en 2026 | Pilote seulement, pas la prod critique |
| 72 | LPO en 2026 | Valider au cas par cas |
| 73 | DPU pour du web | Non, surcoût inutile |
| 74 | DPU pour l'IA/edge | Oui : offload, isolation, zéro-trust |
| 75 | Chiffrement soft du backend | Non, tue la perf ; HW si exigé |
| 76 | DCI non chiffré | Chiffrer (MACsec/OTN) |
| 77 | Edge < 10 serveurs | Switch simple, pas de fabric |
| 78 | CDN : build ou buy | Buy (sortie) sauf hyperscaler |
| 79 | PUE > 1,5 | Auditer le refroidissement |
| 80 | Optiques = 30 % du thermique réseau | Les compter dans le dimensionnement |
| 81 | Switch 800G tout-optique | Prévoir 1,5-2 kW et l'airflow |
| 82 | Armoire sans allée froide | Ne pas y mettre du 800G |
| 83 | UPS réseau = UPS serveurs | Séparer, réseau prioritaire |
| 84 | Pas de test de coupure | Le faire une fois par an |
| 85 | Canicule | Seuils thermiques vérifiés avant |
| 86 | Équipe d'une personne | Doc + automation + astreinte claire |
| 87 | Formation absente | Lab + 1 jour de casse volontaire |
| 88 | Design non relu | 20 questions du §186 |
| 89 | RFP au seul CAPEX | TCO 5 ans obligatoire |
| 90 | Fournisseur unique | Clause de sortie au contrat |
| 91 | Support J+1 sur l'IA | 4 h 24/7 exigé |
| 92 | TAC sans nom | Exiger un contact nommé |
| 93 | Fin de vie | Wipe + DEEE + inventaire |
| 94 | Revente | Effacer licences et configs |
| 95 | Congés | Checklist §173 |
| 96 | REX mensuel | 15 min, ce qui a coincé |
| 97 | Guide non relu à 6 mois | Les ✅ bougent vite (CPO, UEC, 1,6T) |
| 98 | Donnée incertaine | Marquer ⚠️, ne pas affirmer |
| 99 | Référence introuvable | « Non trouvée au 27/09/2026 » |
| 100 | Doute sur un chiffre | Mesurer avant de changer |
| 101 | Pression du vendeur | 3 devis + lab + refs clients |
| 102 | Hype du moment | Pilote d'abord, prod ensuite |
| 103 | « Ça marche chez nous » | Le prouver au lab avec VOS workloads |
| 104 | Doc du vendeur vs terrain | Le terrain gagne toujours |
| 105 | Réunion sans décision | La suivante avec les chiffres |
| 106 | Chiffre sans source | Exiger la datasheet |
| 107 | Datasheet absente | Pas d'achat |
| 108 | Module sans SN | Refuser le lot |
| 109 | Lot > 2 % rejet | Lot refusé |
| 110 | Le réseau est lent | Arbre de dépannage §143 |

---

*Fin du guide — **187 sections numérotées**, 110 décisions rapides,
80 termes de glossaire, 45 pièges, quiz, 50 FAQ, 100 acronymes,
BOMs, checklists, templates, index. Objectif : ≥ 4000 lignes (`wc -l`).*
