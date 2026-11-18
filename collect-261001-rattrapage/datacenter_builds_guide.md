# Datacenter Builds — Le guide des BOMs

**Builds serveurs complets par workload + montage d'un datacenter.**
Chaque build = une BOM complète et chiffrée : châssis, CPU, RAM, stockage, GPU,
NICs, alimentation — avec ordre de prix total et consommation électrique estimée.
Puis : rack, PDU, câblage, refroidissement, onduleurs, mise en service, PME vs hyperscaler.

**Public :** Zelef, chef de service systèmes & énergies. Ce guide parle BOM, watts et euros.
**Langue :** français. **Date de référence des prix : 27/09/2026.**

> **Avertissement honnête.** Les prix serveur sont des prix publics indicatifs relevés
> sur le web le 27/09/2026 (boutiques européennes, configurateurs, snapshots de marché).
> Un vrai devis intégrateur varie de ±15 à 30 % selon volumes, remises et délais.
> Les TDP sont des ordres de grandeur : vérifiez la fiche constructeur exacte avant de
> dimensionner l'électrique. Quand une info n'a pas pu être vérifiée, c'est marqué
> « à vérifier ». Quand un composant est introuvable, c'est marqué
> « non trouvé au 27/09/2026 ». Rien n'est inventé.

---

## 1. Comment utiliser ce guide

1. Lisez la **partie A (sections 2 à 16)** : l'état du marché et les formules de
   dimensionnement. C'est le socle : sans ça, les BOMs ne sont que des listes de courses.
2. Pour chaque workload, prenez la BOM **S** (petit) ou **M** (moyen), ou les variantes
   **double GPU / quad GPU** quand c'est pertinent.
3. Additionnez les consommations estimées : c'est l'entrée de la **partie D**
   (rack, PDU, refroidissement, onduleurs).
4. La **partie E** donne la méthode pour chiffrer seul, la **partie F** le montage
   datacenter complet, la **partie G** l'écart PME/hyperscaler.
5. Glossaire (30+ termes), quiz (10 questions + réponses) et pièges terrain en fin.

Toutes les BOMs sont en **euros HT indicatifs**, sauf mention contraire.
Taux de change retenu : 1 $ ≈ 0,92 € (ordre de grandeur, septembre 2026 — à vérifier
au jour du devis).

---

## 2. Conventions de prix et mentions de vérification

| Mention | Signification |
|---|---|
| **vérifié le 27/09/2026** | Prix ou référence relevé sur le web à cette date (source citée) |
| **à vérifier** | Ordre de grandeur plausible, pas de source directe ce jour-là |
| **non trouvé au 27/09/2026** | Recherché, introuvable publiquement |
| **TDP indicatif** | Valeur constructeur arrondie ; vérifier la fiche exacte |

Règle de lecture des BOMs : colonne « Prix » = prix unitaire public constaté ;
« Total » = quantité × prix. Les totaux sont arrondis. La main-d'œuvre d'intégration
n'est pas incluse (compter 5 à 10 % du matériel pour un montage + burn-in pro).

---

## 3. État du marché au 27/09/2026 — ce qui a changé

Trois faits structurent tous les chiffrages de ce guide.

**1. La DRAM serveur a explosé.** Prix contrat d'un module DDR5 RDIMM 64 Go :
**≈ 1 500 $ en septembre 2026**, soit 5,5× le niveau d'un an plus tôt (272 $) ;
spot jusqu'à 3 100 $ (TrendForce via presse coréenne, vérifié le 27/09/2026).
Cause : Samsung, SK hynix et Micron allouent leurs capacités au HBM, à la DDR5
serveur et aux SSD entreprise — l'IA agentique tire la demande. Conséquence
directe : **la RAM est redevenue le premier poste de coût d'un serveur**
(devant le CPU sur beaucoup de builds). Dimensionnez la RAM au plus juste,
privilégiez 1 DIMM par canal, et négociez les gros volumes tôt.

**2. Les GPU restent sous tension d'allocation.** Snapshot de marché mars 2026
(vérifié le 27/09/2026) : L40S 48 Go à 8 610–8 900 $, RTX PRO 6000 96 Go à
9 450–9 800 $, H100 80 Go SXM neuf à 25 000–35 000 $ (occasion 18 000–22 000 $),
H200 141 Go à 30 000–40 000 $, B200 à 30 000–40 000 $. Délais Blackwell PRO :
3 à 7 mois début 2026. Le MI300X AMD (192 Go HBM3) se traite autour de
10 000–12 000 $ pièce en standalone — l'alternative mémoire/prix pour l'inférence
gros modèles.

**3. Le refroidissement liquide est devenu mainstream.** Enquête AFCOM 2026 :
36 % des opérateurs ont déployé du liquid cooling, 28 % prévoient de le faire sous
12–24 mois ; 37 % ont adopté ou envisagent le rear-door heat exchanger (RDHx),
forme dominante car la moins invasive. Le direct-to-chip monophasé représente
≈ 55 % du marché DTC 2026. Repères de densité : air OK jusqu'à ~20–30 kW/rack,
hybride 50–100 kW, DTC obligatoire au-delà de 100 kW/rack. Côté puissance, le
**800VDC** arrive : produits commerciaux Vertiv, Schneider Electric, Eaton, Delta
attendus au **2ᵉ semestre 2026** (vérifié le 27/09/2026).

---

## 4. CPU : AMD EPYC domine le rack en 2026

Pour les workloads de ce guide, **AMD EPYC 9005 « Turin »** (socket SP5, DDR5
12 canaux, PCIe 5.0) est le choix par défaut : densité de cœurs, lanes PCIe
(128 en mono-socket) et prix/cœur. Intel Xeon 6 (Granite Rapids) reste pertinent
quand l'écosystème l'impose (certains appliances, AMX pour l'inférence CPU),
mais en 2026 le rapport prix/perf du rack généraliste est chez AMD.
Prix publics constatés sur configurateur européen (vérifiés le 27/09/2026) :

| CPU | Cœurs/Threads | TDP indicatif | Prix constaté |
|---|---|---|---|
| EPYC 9124 (Genoa) | 16/32 | 200 W | ≈ 750 € |
| EPYC 9115 (Turin) | 16/32 | 200 W | ≈ 185 € (prix promo constaté — à vérifier, anomalie possible) |
| EPYC 9275F | 24/48 | 320 W | ≈ 2 784 € |
| EPYC 9355P | 32/64 | 280 W | ≈ 2 891 € |
| EPYC 9375F | 32/64 | 320 W | ≈ 3 893 € |
| EPYC 9455P | 48/96 | 300 W | ≈ 3 406 € |
| EPYC 9475F | 48/96 | 360 W | ≈ 4 790 € |
| EPYC 9555P | 64/128 | 360 W | ≈ 4 830 € |
| EPYC 9575F | 64/128 | 400 W | ≈ 6 731 € |
| EPYC 9655P | 96/192 | 400 W | ≈ 5 208 € |
| EPYC 9825 | 144/288 | 390 W | ≈ 7 774 € |
| EPYC 9845 | 160/320 | 390 W | ≈ 8 764 € |

Règles terrain :
- **Suffixe P** = mono-socket optimisé (moins cher, le bon choix en 1P).
  **Suffixe F** = fréquence haute (bases de données, VDI, latence).
- Au-delà de 64 cœurs, vérifiez que votre workload scale : beaucoup d'applis
  paient des cœurs qu'elles n'utilisent pas. Le sweet spot 2026 : **32–64 cœurs**.
- En bi-socket, la bande passante mémoire double mais la latence NUMA apparaît :
  réservez le 2P au HPC, aux grosses DB et à la virtualisation dense.

---

## 5. RAM DDR5 : le poste qui fait mal en 2026

Repères (vérifiés le 27/09/2026) :
- RDIMM DDR5-5600 ECC 64 Go : **≈ 1 500 $ le module** en contrat (≈ 1 380 €),
  spot jusqu'à ≈ 3 100 $ en pointe. Il y a un an : 272 $.
- RDIMM DDR5 128 Go : ≈ 2 800–3 000 $ constatés sur configurateurs (≈ 2 600–2 750 €).
- RDIMM DDR5 96 Go : position intermédiaire, ≈ 2 100 $ (à vérifier).

Règles de dimensionnement :
1. **1 DIMM par canal, 12 canaux remplis** sur EPYC = pleine bande passante.
   12 × 64 Go = 768 Go est la configuration standard 2026.
2. Ne mélangez ni capacités ni rangs sur un même canal si vous voulez la
   fréquence nominale (le BIOS dégrade sinon).
3. ECC Registered obligatoire en serveur. Le prix au Go chute avec la densité
   du module… sauf en 2026 où tout est cher : comparez 12×64 Go vs 12×96 Go
   au moment du devis, l'écart varie vite.
4. Pour les DB : RAM = working set + 30 % de marge (section 33).
   Pour Ceph OSD : 4–5 Go de RAM par To de stockage + base OS (section 43).

**Astuce budget :** sur un nœud Ceph ou backup, 384 Go (12×32 Go) suffisent
souvent — n'achetez pas 768 Go « au cas où » à 1 500 $ le module.

---

## 6. Stockage : NVMe U.2, le standard du datacenter

Le 2,5" U.2 NVMe PCIe 4.0/5.0 est le format de référence. Prix constatés
(vérifiés le 27/09/2026) :

| SSD | Capacité | Endurance | Prix constaté |
|---|---|---|---|
| Samsung PM9A3 U.2 | 960 Go | 1 DWPD | ≈ 877 $ |
| Samsung PM9A3 U.2 | 3,84 To | 1,3 DWPD | ≈ 3 300 $ |
| Samsung PM9A3 U.2 | 7,68 To | 1 DWPD | ≈ 4 256 $ |
| Samsung PM9A3 U.2 | 15,36 To | 1 DWPD | ≈ 2 900–3 000 $ (marché gris, à vérifier) |

Choix d'endurance (critique, voir sections 34 et 48) :
- **Read-intensive (1 DWPD)** : Ceph, backup, boot. Le PM9A3 convient.
- **Mixed-use (3 DWPD)** : DB, virtualisation dense. Gamme PM9A5 / équivalent
  Micron 7450 PRO — ≈ +40 à 60 % vs read-intensive (à vérifier au devis).
- **Write-intensive (10+ DWPD)** : ZIL/SLOG, journaux. Petit volume, cher.

Formats :
- **U.2 2,5"** : standard, hot-swap, le choix par défaut.
- **E3.S / EDSFF** : densité supérieure (1U 24+ baies), remplace progressivement
  l'U.2 sur les plateformes 2025–2026. Vérifiez la dispo du châssis.
- **M.2** : boot uniquement (2× en RAID 1), jamais pour la donnée en prod.

---

## 7. NICs : 25/100/400 GbE, OCP 3.0

| NIC | Usage | Prix indicatif |
|---|---|---|
| 2× 25 GbE SFP28 (Intel E810 / Broadcom) | Compute, Ceph front | ≈ 350–500 € (à vérifier) |
| 2× 100 GbE QSFP28 (Mellanox CX6-DX) | Ceph backend, HPC | ≈ 900–1 300 € (à vérifier) |
| 2× 400 GbE QSFP112 (CX7) | Inter-GPU, backbone IA | ≈ 2 500–4 000 € (non trouvé au 27/09/2026 — devis requis) |
| OCP 3.0 2× 25 GbE | Standard serveurs récents | inclus ou ≈ 250 € |

Règles :
- **RoCE v2** (RDMA sur Ethernet) suffit pour Ceph et le stockage ; **InfiniBand**
  reste roi pour le HPC/GPU à grande échelle (section 21).
- Toujours **2 NICs ou 1 dual-port** : séparez front/back sur Ceph, management
  sur port dédié BMC.
- Comptez les **optiques et DAC** dans la BOM : 30–80 € le DAC 25G 3 m,
  150–400 € l'optique 100G SR4 (à vérifier). Sur 40 serveurs, c'est un budget
  à part entière.

---

## 8. GPU : panorama d'achat 2026

| GPU | VRAM | TDP | Prix constaté (mars–sept 2026) |
|---|---|---|---|
| NVIDIA L40S | 48 Go GDDR6 | 350 W | 8 610–8 900 $ (MSRP 8 352 $) |
| NVIDIA RTX PRO 6000 Blackwell | 96 Go GDDR7 | 600 W | 9 450–9 800 $ (MSRP 7 673 $ workstation) |
| NVIDIA RTX 6000 Ada | 48 Go GDDR6 | 300 W | 7 400–7 800 $ |
| NVIDIA H100 SXM 80 Go | 80 Go HBM2e | 700 W | 25 000–35 000 $ neuf (18 000–22 000 $ occasion) |
| NVIDIA H200 141 Go | 141 Go HBM3e | 700 W | 30 000–40 000 $ |
| NVIDIA B200 | 192 Go HBM3e | 1 000 W | 30 000–40 000 $ |
| AMD MI300X | 192 Go HBM3 | 750 W | 10 000–12 000 $ (standalone) |
| AMD MI325X | 256 Go HBM3e | 750 W | non trouvé au 27/09/2026 (prix public) |
| AMD MI355X | 288 Go HBM3e | 1 000 W | non trouvé au 27/09/2026 (prix public) |
| NVIDIA L4 | 24 Go GDDR6 | 72 W | 2 500–8 000 $ |

Tous prix vérifiés le 27/09/2026 sauf mentions. Le marché est en allocation :
**ne promettez jamais un délai GPU sans écrit du distributeur.**

---

## 9. Alimentation serveurs : Titanium, redondance, 200–240 V

- Standard : **2× PSU redondantes 80 PLUS Titanium** (96 %+ de rendement à 50 %).
- Puissances courantes : 1 600 W (1U/2U compute), 2 000–2 600 W (2U 4-GPU),
  3 000 W+ (nœuds 8-GPU HGX).
- **Branchez toujours en 220–240 V** : un PSU 2 600 W ne sort que 1 000–1 200 W
  en 110 V. En France c'est du 230 V mono ou 400 V tri — pas de sujet, mais
  vérifiez les PDU US si vous importez du matériel.
- Dimensionnement PSU : P_max_serveur × 1,25, réparti sur 2 PSU (N+1).
  Exemple : serveur 1 400 W → 2× 1 600 W (chaque PSU doit pouvoir porter seule
  la charge en cas de panne de l'autre… en pratique on tolère N+1 à 80 %).

---

## 10. Formule n° 1 — IOPS : le dimensionnement stockage

```
IOPS_requis = (IOPS_lecture × %_lecture) + (IOPS_écriture × %_écriture × pénalité_RAID)
```

Pénalités d'écriture : miroir = 2, RAID 5/6 ≈ 4, Ceph réplica 3 ≈ 3 (chaque
écriture client = 3 écritures disques + overhead réseau).

Exemple terrain — 200 VMs bureautiques :
- 50 IOPS/VM en pic (boot storm : 100) → 200 × 50 = 10 000 IOPS mixtes 70/30.
- Écritures : 3 000 × 3 (Ceph replica 3) = 9 000 IOPS disques.
- Total disques : 7 000 + 9 000 = 16 000 IOPS.
- Un NVMe PM9A3 fait ≈ 900 k IOPS lecture / 200 k écriture : **2 NVMe suffisent
  en IOPS**, mais la capacité dictera le nombre réel (section 12).

**Leçon : en 2026, avec le NVMe, c'est presque toujours la capacité qui
dimensionne, pas les IOPS.** Sauf DB OLTP extrême et VDI en boot storm.

---

## 11. Formule n° 2 — Débit réseau et disques

```
Débit_requis = volume / fenêtre_temps × marge
```

Exemple — backup de 20 To en 8 h :
- 20 To = 160 Tb ; / 28 800 s = 5,5 Gb/s théoriques.
- × 1,5 (marge, overhead, dédup) ≈ **8–10 Gb/s** → NIC 25 GbE recommandée.

Exemple — rebuild Ceph : un OSD de 7,68 To qui rebuild à 500 Mo/s met
≈ 4 h 20. Pendant ce temps le cluster est dégradé : prévoyez la bande
passante backend en conséquence (2× 25 G mini, 2× 100 G confortable).

Tableau de conversion rapide :

| Débit | To/heure |
|---|---|
| 1 Gb/s | 0,45 To/h |
| 10 Gb/s | 4,5 To/h |
| 25 Gb/s | 11 To/h |
| 100 Gb/s | 45 To/h |

---

## 12. Formule n° 3 — RAM : le working set + 30 %

```
RAM = working_set_chaud × 1,3 + OS/hyperviseur + marge_croissance
```

- **DB** : working set = taille des tables+index fréquemment lus. 80 % des
  requêtes touchent 20 % des données : dimensionnez pour ces 20 %.
- **Virtualisation** : somme des RAM VMs × (1 − taux_déduplication_KSM ≈ 0,85)
  + 16 Go hyperviseur.
- **Ceph OSD** : 4 Go de base + 1 Go/To (BlueStore), arrondi à 5 Go/To.
- **VDI** : 4–8 Go par utilisateur selon profil (section 58).

Avec la DRAM à 1 500 $/64 Go, **chaque Go inutile coûte ≈ 23 $**. Mesurez le
working set réel (pg_stat, monitoring) avant d'acheter.

---

## 13. Formule n° 4 — Watts : la plus importante du guide

```
P_serveur ≈ Σ TDP_CPU + Σ TDP_GPU + P_RAM + P_disques + P_NICs + P_base
```

Avec :
- P_RAM ≈ 8–10 W par DIMM DDR5 (12 DIMM ≈ 110 W).
- P_disque NVMe ≈ 12–20 W en charge (× nombre).
- P_base (carte mère, BMC, ventilos) ≈ 80–150 W selon châssis.
- **Facteur de charge réel** : un serveur ne tourne jamais à 100 % de tous ses
  TDP simultanément. Appliquez un **facteur de coïncidence de 0,7–0,8** pour
  l'électrique, mais dimensionnez les PSU et le refroidissement à 1,0.

Exemple — serveur 2× EPYC 9655P + 12 NVMe + 2× 100G :
- 2× 400 + 12× 15 + 2× 25 + 110 + 120 = **1 260 W** max théorique.
- Charge réaliste : ≈ 900 W. PSU : 2× 1 600 W. PDU : compter 1,3 kW.

**C'est cette formule qui alimente tout le dimensionnement onduleur
(partie D, sections 155–160).** Faites le tableau une fois, réutilisez-le partout.

---

## 14. Formule n° 5 — TCO simplifié sur 5 ans

```
TCO_5ans = CAPEX_matériel + 5 × (électricité + maintenance + licences)
```

- Électricité : P_moyenne_kW × 8 760 h × prix_kWh (0,15–0,25 €/kWh pro en France
  en 2026 — à vérifier sur votre contrat).
- Exemple : serveur à 900 W moyens, 0,20 €/kWh → 900 × 8,76 × 0,20 ≈
  **1 577 €/an**, soit **7 885 € sur 5 ans**. À comparer au CAPEX : sur un
  serveur à 15 000 €, l'énergie représente **un tiers du TCO**.
- Maintenance : 8–12 % du CAPEX/an en contrat constructeur NBD, ou pièces
  détachées en stock pour l'autonomie.

**Un serveur « pas cher » qui consomme 400 W de plus coûte 3 500 € de plus
en électricité sur 5 ans.** Le TCO tue les fausses économies.

---

## 15. Règles de marge terrain (à appliquer à toutes les BOMs)

1. **Capacité** : +30 % de croissance à 3 ans minimum sur stockage et RAM.
2. **Puissance** : +25 % sur PSU et PDU vs P_max calculée.
3. **Réseau** : un uplink ne dépasse jamais 70 % en nominal.
4. **IOPS** : +50 % vs pic mesuré (les pics non mesurés existent).
5. **Délais** : commandez GPU et DRAM 3–6 mois avant le besoin en 2026.
6. **Pièces** : 1 disque et 1 PSU de rechange par grappe homogène de 10 nœuds.

---

## 16. Châssis de référence utilisés dans ce guide

| Châssis | Format | Usage guide | Prix nu constaté |
|---|---|---|---|
| ASUS RS501A-E12 / Supermicro AS-1115CS-TNR | 1U 1P EPYC | Compute, firewall, Ceph MON | ≈ 4 500–5 000 € (vérifié le 27/09/2026) |
| 2U 8–12 baies (type Supermicro 2029P) | 2U 2P EPYC | DB, virtualisation, backup | ≈ 3 500–4 500 € (à vérifier) |
| 2U 24 baies NVMe (type ASRock Rack 2U24E) | 2U 2P EPYC | Ceph NVMe | ≈ 4 300 $ (relevé ancien — à vérifier) |
| ASUS ESC4000A-E12 | 2U 4× GPU double-slot | Inférence 2–4 GPU | ≈ 4 320 £ (~5 000 €) nu (vérifié le 27/09/2026) |
| 4U 36 baies 3,5" (type Supermicro 847) | 4U | Ceph hybride, backup | ≈ 4 000–5 500 € (à vérifier) |
| 8-GPU HGX (Supermicro/Dell XE) | 8U | Inférence lourde | devis uniquement (non trouvé au 27/09/2026 en prix public) |

> Les BOMs ci-dessous utilisent ces châssis comme base « nu » (sans CPU/RAM/disques).

---

# PARTIE B — BOMs PAR WORKLOAD

---

## 17. Workload 1 — Compute / HPC : principes

Le « compute » générique (virtualisation, Kubernetes, batch) se dimensionne au
**ratio cœurs/RAM/bande passante** :
- Virtualisation/K8s : 4–8 Go de RAM par cœur.
- HPC : 2–4 Go par cœur + interconnect rapide (InfiniBand ou RoCE).
- 1× EPYC 64 cœurs + 512 Go–1 To RAM couvre 80 % des besoins PME.

Deux tailles ci-dessous : **S** (mono-socket 32 cœurs, le cheval de labour) et
**M** (bi-socket 96 cœurs, nœud HPC/virt dense).

---

## 18. Compute S — BOM complète (1× EPYC 32 cœurs, 384 Go RAM)

| Composant | Référence type | Qté | Prix unit. | Total |
|---|---|---|---|---|
| Châssis 1U 1P EPYC 9005, 10 baies NVMe | Supermicro AS-1115CS-TNR nu | 1 | 4 585 € | 4 585 € |
| CPU AMD EPYC 9355P (32c/64t, 280 W) | 100-000001694 (P = 1P) | 1 | 2 891 € | 2 891 € |
| RAM DDR5-5600 ECC RDIMM 32 Go | 12× 32 Go = 384 Go | 12 | ≈ 750 € | 9 000 € |
| SSD boot M.2 NVMe 960 Go (RAID 1) | 2× 960 Go entreprise | 2 | 180 € | 360 € |
| SSD data U.2 NVMe 3,84 To (PM9A3) | MZQL23T80 | 4 | ≈ 3 035 € | 12 140 € |
| NIC 2× 25 GbE SFP28 | Intel E810-XXVDA2 | 1 | 450 € | 450 € |
| DAC 25G 3 m + optiques | — | 4 | 50 € | 200 € |
| PSU 2× 1 600 W Titanium redondantes | incluses châssis | — | — | — |
| Rails + kit | — | 1 | 120 € | 120 € |
| **TOTAL** | | | | **≈ 29 750 €** |

Prix vérifiés le 27/09/2026 sauf RAM/SSD/NIC (ordres de grandeur 2026 — à vérifier
au devis, la DRAM bouge vite).

---

## 19. Compute S — consommation électrique estimée

| Poste | Calcul | Watts |
|---|---|---|
| CPU EPYC 9355P | 1× 280 W TDP | 280 |
| RAM 12 DIMM | 12 × 9 W | 108 |
| 4× NVMe data + 2× M.2 | 6 × 15 W | 90 |
| NIC 25G | 1 × 25 W | 25 |
| Base (carte mère, BMC, ventilos 1U) | — | 120 |
| **P_max théorique** | | **≈ 623 W** |
| **P_réaliste (facteur 0,75)** | | **≈ 470 W** |

PSU : 2× 1 600 W → largement couvertes. **Compter 0,65 kW par nœud** pour la PDU.
Sur 10 nœuds : 6,5 kW/rack — de l'air cooling standard.

---

## 20. Compute M — BOM complète (2× EPYC 96 cœurs, 768 Go RAM)

| Composant | Référence type | Qté | Prix unit. | Total |
|---|---|---|---|---|
| Châssis 2U 2P EPYC, 12 baies | 2U 12× 3,5"/2,5" nu | 1 | 4 000 € | 4 000 € |
| CPU AMD EPYC 9655P (96c/192t, 400 W) | 2× | 2 | 5 208 € | 10 416 € |
| RAM DDR5-5600 ECC RDIMM 64 Go | 12× 64 Go = 768 Go | 12 | ≈ 1 380 € | 16 560 € |
| SSD boot 2× M.2 960 Go RAID 1 | — | 2 | 180 € | 360 € |
| SSD data U.2 NVMe 7,68 To (PM9A3) | MZ-QL27T600 | 6 | ≈ 3 915 € | 23 490 € |
| NIC 2× 25 GbE SFP28 (front) | Intel E810-XXVDA2 | 1 | 450 € | 450 € |
| NIC 2× 100 GbE QSFP28 (stockage/HPC) | Mellanox CX6-DX | 1 | 1 100 € | 1 100 € |
| PSU 2× 2 000 W Titanium | incluses | — | — | — |
| Câbles/optiques | — | lot | — | 600 € |
| **TOTAL** | | | | **≈ 56 975 €** |

Note : la RAM (16 560 €) coûte plus cher que les 2 CPU réunis. Bienvenue en 2026.

---

## 21. Compute M — consommation et comparatif S/M

| Poste | Calcul | Watts |
|---|---|---|
| 2× CPU EPYC 9655P | 2 × 400 W | 800 |
| RAM 12 DIMM 64 Go | 12 × 10 W | 120 |
| 6× NVMe 7,68 To | 6 × 18 W | 108 |
| NICs (25G + 100G) | 25 + 2 × 30 W | 85 |
| Base 2U | — | 150 |
| **P_max théorique** | | **≈ 1 263 W** |
| **P_réaliste (0,75)** | | **≈ 950 W** |

| Critère | Compute S | Compute M |
|---|---|---|
| Cœurs / threads | 32 / 64 | 192 / 384 |
| RAM | 384 Go | 768 Go |
| Stockage data | 15,4 To bruts | 46 To bruts |
| Prix | ≈ 29 750 € | ≈ 56 975 € |
| P_max | 0,62 kW | 1,26 kW |
| €/cœur | ≈ 930 € | ≈ 297 € |
| Usage type | Virt légère, K8s, services | Virt dense, HPC, build |

**Le M est 3× moins cher au cœur.** Si vous virtualisez, le M gagne presque
toujours au TCO — sauf si vous n'avez besoin que de quelques VMs.

---

## 22. HPC petite grappe : l'interconnect décide de tout

Pour 4–8 nœuds HPC, le réseau est plus important que le CPU :
- **InfiniBand NDR (400 Gb/s)** : latence ≈ 1 µs, le choix MPI sérieux.
  Switch 8–16 ports NDR : 15 000–30 000 € (à vérifier — devis requis).
- **RoCE v2 sur 100/200 GbE** : latence 2–5 µs, 3× moins cher, suffisant pour
  80 % des codes (et pour Ceph).
- Ne faites pas de HPC sérieux sur du 25 GbE : le réseau devient le goulot
  avant les CPU.

BOM nœud HPC type (×4) : Compute M (section 20) + NIC IB NDR 400G
(≈ 2 500 €, à vérifier) − NIC 100G. Soit ≈ 58 500 €/nœud + switch IB.
**Total grappe 4 nœuds : ≈ 260 000 €**, P_max ≈ 5,5 kW (un demi-rack, air OK).

---

## 23. Cas chiffré : grappe Proxmox VE 3 nœuds HA

Hypothèses : 60 VMs bureautiques (4 vCPU / 16 Go), stockage Ceph intégré
ou ZFS répliqué, HA Proxmox.

| Poste | Détail | Total |
|---|---|---|
| 3× Compute M (section 20) | 56 975 € × 3 | 170 925 € |
| Switch 2× 25G + 2× 100G (ToR, redondant) | 2× ≈ 8 000 € | 16 000 € |
| Câblage (DAC, optiques, PDU déjà comptées) | lot | 2 500 € |
| Licences Proxmox VE (Community→Standard, 3 CPU-socket/an) | 3× ≈ 340 €/an | ≈ 1 020 €/an |
| **CAPEX** | | **≈ 189 500 €** |
| P_max grappe | 3 × 1,26 kW | **≈ 3,8 kW** |

Densité : 60 VMs × 16 Go = 960 Go RAM utile ; 3 nœuds × 768 Go = 2 304 Go bruts,
soit N+1 avec marge (perte d'un nœud : 1 536 Go restants > 960 × 1,15).
C'est le dimensionnement à retenir : **toujours N+1 sur la ressource critique**.

---

## 24. Kubernetes bare-metal : build worker type

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 1U 1P EPYC | AS-1115CS-TNR nu | 1 | 4 585 € |
| EPYC 9455P (48c/96t) | — | 1 | 3 406 € |
| RAM 12× 64 Go = 768 Go | DDR5 ECC | 12 | 16 560 € |
| 2× NVMe 3,84 To (OS + ephemeral) | PM9A3 | 2 | 6 070 € |
| NIC 2× 25 GbE | E810 | 1 | 450 € |
| **TOTAL/worker** | | | **≈ 31 070 €** |
| P_max / réaliste | | | **≈ 0,85 kW / 0,64 kW** |

Ratio 2026 constaté : **8 Go RAM par cœur** pour du K8s générique
(microservices Java/Go). Montez à 12–16 Go/cœur pour l'IA et la data.

---

## 25. Pièges terrain — Compute / HPC

1. **Acheter des cœurs sans bande passante mémoire** : 160 cœurs avec 384 Go
   de RAM = famine mémoire. Ratio plancher : 4 Go/cœur.
2. **Bi-socket pour « la sécurité »** : en 2026, un mono-socket 96 cœurs bat un
   bi-socket 2× 32 cœurs en perf/prix/watt. Le 2P ne se justifie que par la RAM
   (> 1,5 To) ou les lanes PCIe.
3. **Oublier la licence par socket/cœur** : VMware, certaines DB commerciales
   se paient au cœur — un 192-cœurs peut coûter plus cher en licences qu'en
   matériel. Vérifiez avant de commander.
4. **HPC sur Ethernet 10G « parce qu'on a déjà les switchs »** : 90 % des
   déceptions MPI viennent du réseau, pas du CPU.
5. **Ventilos 1U à 15 000 tr/min dans un bureau** : 70 dB. Le 1U va en salle
   ventilée, jamais sous un bureau.

---

## 26. Check-list d'achat Compute (à joindre au devis)

- [ ] CPU : suffixe P (1P) ou F (fréquence) — justifié par écrit
- [ ] RAM : 12 DIMM identiques, 1 DPC, ECC Registered, fréquence cible
- [ ] Stockage : endurance DWPD adaptée (1 / 3 / 10)
- [ ] NICs : 2 ports min, optiques/DAC chiffrés dans le devis
- [ ] PSU : 2× redondantes, dimensionnées à P_max × 1,25
- [ ] BMC/IPMI : licence incluse (iDRAC Enterprise / iLO Advanced se paient !)
- [ ] Garantie : 3 ans NBD mini, 5 ans pour l'amortissement comptable
- [ ] Firmware : version livrée notée, plan de MAJ au burn-in

---

## 27. Workload 2 — Bases de données : principes

Une DB se dimensionne sur **trois axes** : RAM (working set), IOPS écriture
(endurance), latence (NVMe local, pas de SAN lent).
- **OLTP** (PostgreSQL, MySQL) : fréquence CPU haute (suffixe F), RAM = working
  set + 30 %, NVMe mixed-use 3 DWPD, fsync rapide.
- **Data warehouse / OLAP** : cœurs nombreux, RAM énorme, débit séquentiel.
- Règle d'or : **shared_buffers = 25 % RAM**, le reste au cache OS (Linux fait
  très bien le cache).
- Toujours **2 nœuds mini** (primaire + réplica synchrone) : une DB seule est
  une panne en attente.

---

## 28. PostgreSQL OLTP S — BOM (500 Go actifs, 5 000 TPS)

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 2U 1P EPYC, 8 baies NVMe | 2U 1P nu | 1 | 3 800 € |
| EPYC 9375F (32c, 3,85 GHz, 320 W) | fréquence pour OLTP | 1 | 3 893 € |
| RAM 12× 64 Go = 768 Go | DDR5 ECC | 12 | 16 560 € |
| 2× M.2 960 Go RAID 1 (OS) | — | 2 | 360 € |
| 4× NVMe 3,84 To mixed-use 3 DWPD (data) | PM9A5 ou équiv. | 4 | ≈ 4 200 € × 4 = 16 800 € |
| 2× NVMe 1,92 To mixed-use (WAL dédié) | — | 2 | ≈ 2 400 € × 2 = 4 800 € |
| NIC 2× 25 GbE (réplication) | E810 | 1 | 450 € |
| PSU 2× 1 600 W | incluses | — | — |
| **TOTAL** | | | **≈ 46 665 €** |
| P_max / réaliste | 320+120+150+50+150 | | **≈ 0,79 kW / 0,6 kW** |

Le WAL sur disques dédiés : +20 à 40 % de TPS sur écritures intenses.
C'est le meilleur euro/perf d'une DB.

---

## 29. PostgreSQL OLTP M — BOM (2 To actifs, 20 000 TPS)

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 2U 2P EPYC, 12 baies | 2U 2P nu | 1 | 4 200 € |
| EPYC 9475F (48c, 3,65 GHz, 360 W) | 2× | 2 | 4 790 € × 2 = 9 580 € |
| RAM 24× 64 Go = 1,5 To (12/canaux × 2 CPU) | DDR5 ECC | 24 | 33 120 € |
| 2× M.2 960 Go RAID 1 (OS) | — | 2 | 360 € |
| 8× NVMe 7,68 To mixed-use 3 DWPD (data) | — | 8 | ≈ 5 500 € × 8 = 44 000 € |
| 2× NVMe 3,84 To (WAL) | — | 2 | ≈ 4 200 € × 2 = 8 400 € |
| NIC 2× 100 GbE (réplication + backup) | CX6-DX | 1 | 1 100 € |
| PSU 2× 2 000 W | incluses | — | — |
| **TOTAL** | | | **≈ 100 760 €** |
| P_max / réaliste | 720+240+240+80+150 | | **≈ 1,43 kW / 1,05 kW** |

---

## 30. PostgreSQL Data Warehouse — BOM (20 To, analytique)

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 2U 2P EPYC, 24 baies NVMe | type 2U24E nu | 1 | ≈ 3 970 € |
| EPYC 9655P (96c, 400 W) | 2× | 2 | 5 208 € × 2 = 10 416 € |
| RAM 24× 128 Go = 3 To | DDR5 ECC | 24 | ≈ 2 750 € × 24 = 66 000 € |
| 16× NVMe 7,68 To read-intensive (PM9A3) | MZ-QL27T600 | 16 | 3 915 € × 16 = 62 640 € |
| NIC 2× 100 GbE | CX6-DX | 1 | 1 100 € |
| PSU 2× 2 000 W | incluses | — | — |
| **TOTAL** | | | **≈ 144 125 €** |
| P_max / réaliste | 800+240+290+60+150 | | **≈ 1,54 kW / 1,1 kW** |

Capacité brute : 123 To. En production analytique (compression colonne
≈ 3–5×), cela porte **300–500 To logiques**. L'OLAP se paie en RAM, pas en CPU.

---

## 31. MySQL / MariaDB : ce qui change

Même BOM que PostgreSQL S/M, avec 3 différences :
1. **InnoDB buffer pool = 70–80 % RAM** (vs 25 % shared_buffers + cache OS
   pour PG) : prévoyez +15 % de RAM à working set égal.
2. Le WAL s'appelle redo log : mêmes disques dédiés recommandés.
3. Réplication : un réplica asynchrone suffit souvent (vs synchrone en PG
   pour le RPO zéro) — divisez le coût du 2ᵉ nœud par 1,5 si l'async est
   acceptable métier.

---

## 32. Dimensionnement RAM d'une DB — la formule qui évite le sur-achat

```
RAM_cible = (taille_tables_chaudes + taille_index_chauds) × 1,3
```

Méthode :
1. `pg_total_relation_size` trié par accès (`pg_stat_user_tables`) : identifiez
   les 20 % de tables qui servent 80 % des requêtes.
2. Additionnez tables + index de ce top 20 %.
3. × 1,3 (marge + OS + connexions : 10 Mo par connexion, 500 connexions = 5 Go).

Exemple : 300 Go chauds → 390 Go → **384 Go suffisent**, pas 768 Go.
Économie 2026 : 384 Go = 6 modules × 1 380 € ≈ **8 280 € économisés**.
Mesurez d'abord, achetez ensuite.

---

## 33. Stockage DB : l'endurance décide du modèle de SSD

| Profil écriture | DWPD requis | Modèle type | Surcoût vs 1 DWPD |
|---|---|---|---|
| Lecture 90 % (reporting) | 1 | PM9A3 | base |
| OLTP mixte 70/30 | 3 | PM9A5 / Micron 7450 PRO | +40–60 % (à vérifier) |
| WAL / redo seul | 3–10 | Optane-like / Z-NAND (non trouvé au 27/09/2026 en neuf — marché de niche) | — |
| TempDB / tri | 3 | mixed-use | +40–60 % |

Calcul d'endurance : `DWPD_requis = (Go_écrits/jour) / capacité_To / 1000`.
Exemple : 2 To écrits/jour sur 4× 3,84 To = 15,4 To → 0,13 DWPD → 1 DWPD suffit
largement. **La plupart des DB n'ont pas besoin de 3 DWPD** : calculez avant
de payer le surcoût.

---

## 34. Vector DB pour l'IA — en bref (pgvector, Qdrant, Milvus)

Pour le RAG de Zelef et les applis IA : pas besoin d'un build dédié au début.

| Option | Quand | Sizing indicatif |
|---|---|---|
| **pgvector** (extension PG) | < 5 M vecteurs, RAG classique | Même serveur que la DB, +20 % RAM |
| **Qdrant** | 5–100 M vecteurs, filtrage riche | 1 nœud Compute S (section 18) par 20 M vecteurs 768-d |
| **Milvus** | > 100 M vecteurs, distribué | 3 nœuds Compute S minimum |

Règle : **1 M vecteurs 768 dimensions ≈ 3 Go RAM** (float32) / 1,5 Go en
quantifié. 10 M vecteurs = 30 Go → tient dans n'importe quel serveur du guide.
Ne construisez une infra vectorielle dédiée qu'au-delà de 50 M vecteurs.

---

## 35. Cas chiffré : PostgreSQL 2 To actifs, HA synchrone

| Poste | Détail | Total |
|---|---|---|
| 2× PG OLTP M (section 29) | 100 760 € × 2 | 201 520 € |
| 1× témoin/quorum (petit 1U) | — | 8 000 € |
| Switch 25G redondant (déjà au §23 si mutualisé) | — | — |
| Licences | 0 € (open source) | 0 € |
| **CAPEX** | | **≈ 209 500 €** |
| P_max | 2 × 1,43 kW + 0,3 | **≈ 3,2 kW** |
| Coût/To actif utile | 209 500 / 2 | **≈ 105 k€/To** |

C'est le prix de la donnée critique : la redondance synchrone double le coût,
mais le RPO zéro n'a pas de prix en production.

---

## 36. Sauvegarde des DB : ne l'oubliez pas dans la BOM

- **pgBackRest / Percona XtraBackup** vers le serveur backup (section 97+) :
  full hebdo + incr quotidienne + WAL archivés en continu.
- Dimensionnement : taille_base × (1 + 6 × taux_incr + 1) × rétention.
  Exemple : base 2 To, incr 5 %/j, rétention 30 j → 2 × (1 + 0,3 + 1) ≈ 4,6 To
  sur le repo backup. **Chiffrez toujours le repo** (le backup contient toute
  la donnée en clair sinon).
- Restaurez **tous les mois** en test : un backup non testé n'existe pas.

---

## 37. Monitoring DB : les 5 métriques qui comptent

1. **Cache hit ratio** (> 99 % en OLTP, sinon RAM insuffisante).
2. **Checkpoints** : fréquence et durée (WAL/disques trop lents si > 30 s).
3. **Bloat** : tables/index gonflés → VACUUM/OPTIMIZE planifié.
4. **Réplication lag** : < 1 s en synchrone, alerte au-delà.
5. **Température SSD** : les NVMe throttlent à 70–75 °C — vérifiez le flux
   d'air du châssis (les baies avant des 2U denses chauffent).

---

## 38. Pièges terrain — Bases de données

1. **Mettre la DB sur Ceph sans QoS** : la latence réseau tue les TPS.
   DB critique = NVMe local, toujours.
2. **RAID 5/6 sur SSD pour une DB** : la pénalité d'écriture ×4 + le garbage
   collection = latence imprévisible. Miroir ou RAID 10.
3. **Oublier le WAL dédié** : le meilleur euro/perf, et le plus oublié.
4. **Sur-RAM « au cas où »** en 2026 : 384 Go inutiles = 8 280 € brûlés.
5. **Tester la perf sur des données vides** : un benchmark sur table vide ne
   vaut rien. Testez à volumétrie réelle.
6. **Réplica synchrone sur un lien > 2 ms** : chaque commit attend le réplica —
   la latence applicative explose. Synchrone = même salle, fibre courte.

---

## 39. Tableau récapitulatif — Builds DB

| Build | Cible | Prix | P_max | RAM | Stockage |
|---|---|---|---|---|---|
| PG OLTP S | 500 Go / 5 kTPS | ≈ 46,7 k€ | 0,79 kW | 768 Go | 15,4 To MU + WAL |
| PG OLTP M | 2 To / 20 kTPS | ≈ 100,8 k€ | 1,43 kW | 1,5 To | 61 To MU + WAL |
| PG Warehouse | 20 To analytique | ≈ 144,1 k€ | 1,54 kW | 3 To | 123 To RI |
| HA synchrone (2× M) | RPO zéro | ≈ 209,5 k€ | 3,2 kW | — | — |

MU = mixed-use 3 DWPD, RI = read-intensive 1 DWPD.

---

## 40. Ce qu'il faut retenir — DB

- CPU **F** (fréquence), RAM = working set × 1,3, WAL dédié, NVMe local.
- L'endurance se **calcule** (Go écrits/jour) — ne payez pas 3 DWPD par défaut.
- En 2026, la RAM domine le prix : mesurez avant d'acheter.
- Vector DB : commencez par pgvector, Qdrant au-delà de 5 M vecteurs.

---

## 41. Workload 3 — Ceph full NVMe : principes

Ceph full NVMe = le stockage unifié (bloc RBD, objet S3, fichier CephFS) à
haute performance. Règles d'architecture :
- **Minimum 3 nœuds OSD** (5+ recommandé pour la résilience), + 3 MON/MGR
  (colocalisés ou dédiés).
- **Réplica 3** par défaut (survie à 1 panne) ; **EC 4+2** pour l'objet froid
  (économe en capacité, pas pour le bloc à latence faible).
- **Réseau séparé** : front (clients) et back (réplication/rebuild) — le back
  doit encaisser 2× le débit d'un rebuild complet.
- 1 OSD par SSD (pas de partitionnement), BlueStore.

---

## 42. Nœud OSD NVMe S — BOM (8× 7,68 To = 61 To bruts)

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 2U 12 baies NVMe, 1P EPYC | 2U 12× U.2 nu | 1 | 4 200 € |
| EPYC 9355P (32c) — Ceph aime les cœurs | — | 1 | 2 891 € |
| RAM 12× 32 Go = 384 Go (5 Go/To × 61 To + base) | DDR5 ECC | 12 | 9 000 € |
| 2× M.2 960 Go RAID 1 (OS) | — | 2 | 360 € |
| 8× NVMe 7,68 To PM9A3 (OSD) | MZ-QL27T600 | 8 | 3 915 € × 8 = 31 320 € |
| NIC 2× 25 GbE (front) | E810 | 1 | 450 € |
| NIC 2× 100 GbE (back/rebuild) | CX6-DX | 1 | 1 100 € |
| PSU 2× 1 600 W | incluses | — | — |
| **TOTAL** | | | **≈ 49 320 €** |
| P_max / réaliste | 280+110+150+85+150 | | **≈ 0,78 kW / 0,6 kW** |

---

## 43. Nœud OSD NVMe M — BOM (16× 7,68 To = 123 To bruts)

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 2U 24 baies NVMe, 2P EPYC | type 2U24E nu | 1 | ≈ 3 970 € |
| EPYC 9555P (64c) | 2× | 2 | 4 830 € × 2 = 9 660 € |
| RAM 24× 32 Go = 768 Go | DDR5 ECC | 24 | 18 000 € |
| 2× M.2 960 Go RAID 1 (OS) | — | 2 | 360 € |
| 16× NVMe 7,68 To PM9A3 (OSD) | MZ-QL27T600 | 16 | 3 915 € × 16 = 62 640 € |
| NIC 2× 25 GbE (front) | E810 | 1 | 450 € |
| NIC 2× 100 GbE (back) | CX6-DX | 1 | 1 100 € |
| PSU 2× 2 000 W | incluses | — | — |
| **TOTAL** | | | **≈ 96 180 €** |
| P_max / réaliste | 720+240+290+85+150 | | **≈ 1,49 kW / 1,1 kW** |

---

## 44. Nœuds MON/MGR — BOM (×3)

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 1U 1P EPYC, 4 baies | AS-1115CS-TNR nu | 1 | 4 585 € |
| EPYC 9124 (16c, 200 W) | — | 1 | 749 € |
| RAM 6× 32 Go = 192 Go | DDR5 ECC | 6 | 4 500 € |
| 2× NVMe 1,92 To (RocksDB MON) | — | 2 | ≈ 1 600 € × 2 = 3 200 € |
| NIC 2× 25 GbE | E810 | 1 | 450 € |
| **TOTAL / nœud** | | | **≈ 13 485 €** |
| **× 3 nœuds** | | | **≈ 40 455 €** |
| P_max / nœud | | | **≈ 0,45 kW** |

Les MON stockent RocksDB : mettez du NVMe correct, pas du SATA. Un MON lent =
cluster lent (les OSD attendent les maps).

---

## 45. Réseau Ceph : le dimensionner sans se tromper

| Lien | Débit recommandé | Pourquoi |
|---|---|---|
| Front (clients → OSD) | 2× 25 GbE par nœud | pics clients |
| Back (réplication) | 2× 100 GbE par nœud (M) / 2× 25 GbE (S) | rebuild × N OSD |
| MON | 2× 10/25 GbE | faible |

Règle : **bande passante back ≥ (débit_rebuild_1_OSD × nb_OSD_en_rebuild)**.
Avec 16 OSD à 500 Mo/s = 8 Go/s = 64 Gb/s → 2× 100 GbE = 200 Gb/s, confortable.
En 25 GbE × 2 = 50 Gb/s : le rebuild sature le back — acceptable en S, pas en M.

Switches : 2× ToR 100G 32 ports (redondance MLAG) ≈ 12 000–18 000 € pièce
(à vérifier). Comptez aussi **un réseau out-of-band** pour les MON.

---

## 46. Formule capacité utile Ceph

```
Utile_réplica3 = brut × 0,85 (marge remplissage) / 3
Utile_EC4+2    = brut × 0,85 × 4/6
```

Exemples :
- 5× nœuds S (5 × 61 To = 307 To bruts) en réplica 3 → **87 To utiles**.
- 5× nœuds M (5 × 123 To = 615 To bruts) en réplica 3 → **174 To utiles**.
- Mêmes 615 To en EC 4+2 (pool objet) → **348 To utiles**.

Ne remplissez jamais un pool au-delà de **85 %** : au-delà, Ceph ne peut plus
rééquilibrer et les perfs s'effondrent. C'est une limite dure, pas un conseil.

---

## 47. Cas chiffré : 500 To utiles full NVMe, réplica 3

Besoin brut : 500 / 0,85 × 3 = **1 765 To bruts** → 15 nœuds M (15 × 123 To).

| Poste | Détail | Total |
|---|---|---|
| 15× nœuds OSD M | 96 180 € × 15 | 1 442 700 € |
| 3× MON/MGR | 40 455 € | 40 455 € |
| 2× switch 100G 32p (back) | 15 000 € × 2 | 30 000 € |
| 2× switch 25G 48p (front) | 8 000 € × 2 | 16 000 € |
| Câblage/optiques | lot | 15 000 € |
| **CAPEX** | | **≈ 1 544 000 €** |
| P_max | 15 × 1,49 + 3 × 0,45 + 4 × 0,4 | **≈ 25 kW** |
| **Coût/To utile** | 1 544 000 / 500 | **≈ 3 090 €/To** |

25 kW = **un rack haute densité** (air limite) ou 2 racks standard.
C'est ici que le refroidissement (partie D) entre en jeu.

---

## 48. Cas chiffré : 150 To utiles, 5 nœuds S

| Poste | Détail | Total |
|---|---|---|
| 5× nœuds OSD S | 49 320 € × 5 | 246 600 € |
| 3× MON/MGR (mutualisables si petit) | 40 455 € | 40 455 € |
| 2× switch 25G/100G | — | 20 000 € |
| Câblage | lot | 5 000 € |
| **CAPEX** | | **≈ 312 000 €** |
| P_max | 5 × 0,78 + 3 × 0,45 + 0,8 | **≈ 6 kW** |
| **Coût/To utile** (87 To, §46) | 312 000 / 87 | **≈ 3 590 €/To** |

Le petit cluster coûte **plus cher au To** (MON fixes, switches fixes) :
l'effet d'échelle joue dès 8–10 nœuds.

---

## 49. E3.S vs U.2 : le format de 2026

| Critère | U.2 2,5" | E3.S (EDSFF) |
|---|---|---|
| Densité 1U | 10–12 baies | 24–32 baies |
| Hot-swap | oui | oui |
| Prix SSD | référence | ≈ +10–15 % (à vérifier) |
| Dispo châssis 2026 | large | croissante (Supermicro, Dell) |
| Conseil | valeur sûre | si densité 1U requise |

Pour un nouveau build Ceph 2026 : **regardez l'E3.S** si le châssis existe chez
votre intégrateur, sinon restez en U.2 sans regret.

---

## 50. Scrub, deep-scrub et conso cachée

- Le **scrub hebdo** et le **deep-scrub mensuel** lisent tous les disques :
  prévoyez +10–15 % de charge disques/réseau en fenêtre de scrub.
- Planifiez-les **hors heures de backup** : scrub + backup + rebuild
  simultanés = la recette d'un incident.
- Les NVMe consomment 2× plus en écriture qu'au repos : la P_réaliste de la
  section 42/43 suppose 50 % d'écriture — en scrub (lecture), comptez −15 %.

---

## 51. Pièges terrain — Ceph NVMe

1. **Mélanger des SSD de tailles différentes** dans un même pool : Ceph
   répartit par OSD, le plus petit disque limite tout le monde.
2. **Back réseau en 10G « pour commencer »** : le premier rebuild sature tout
   et les clients rament. 25G mini, 100G dès 8 nœuds.
3. **Oublier les MON dédiés en NVMe** : des MON sur SATA = latence de cluster.
4. **Remplir à 90 %+** « temporairement » : il n'y a rien de plus permanent que
   du temporaire en stockage.
5. **Firmware SSD hétérogènes** : un lot avec un vieux firmware peut faire du
   garbage-collection au mauvais moment. Uniformisez au burn-in.
6. **Sous-dimensionner la RAM** : BlueStore + RocksDB mangent 5 Go/To. En
   dessous, c'est le swap et la mort lente.

---

## 52. Ce qu'il faut retenir — Ceph NVMe

- 5 nœuds OSD mini en prod, réplica 3, 85 % max de remplissage.
- Back réseau = 2× le débit de rebuild ; 100G dès que ça compte.
- Coût 2026 : ≈ 3 000–3 600 €/To utile (la DRAM et les SSD dominent).
- 25 kW pour 500 To utiles : prévoyez le refroidissement dès la BOM.

---

## 53. Workload 4 — Ceph hybride : principes

Le Ceph hybride combine **HDD 3,5" haute capacité** (données froides/tièdes) et
**NVMe** (cache, DB/WAL BlueStore, pools chauds). C'est l'architecture
prix/capacité pour l'objet, la vidéo, le backup secondaire.
- Ratio classique : **1 To de NVMe pour 10–20 To de HDD**.
- Les HDD sont en 2026 des **20–24 To CMR** (Seagate Exos / WD Ultrastar).
- Comptez **120–180 IOPS par HDD** : le dimensionnement IOPS limite le nombre
  de HDD par nœud avant la capacité.

---

## 54. Nœud OSD hybride S — BOM (12× 20 To HDD + 4× 3,84 To NVMe)

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 4U 36 baies 3,5", 1P EPYC | type 4U36 nu | 1 | 4 800 € |
| EPYC 9355P (32c) | — | 1 | 2 891 € |
| RAM 12× 32 Go = 384 Go | DDR5 ECC | 12 | 9 000 € |
| 2× M.2 960 Go RAID 1 (OS) | — | 2 | 360 € |
| 12× HDD 20 To CMR (Exos X20) | — | 12 | ≈ 380 € × 12 = 4 560 € |
| 4× NVMe 3,84 To (cache/WAL) | PM9A3 | 4 | 3 035 € × 4 = 12 140 € |
| NIC 2× 25 GbE (front) | E810 | 1 | 450 € |
| NIC 2× 25 GbE (back) | E810 | 1 | 450 € |
| PSU 2× 1 600 W | incluses | — | — |
| **TOTAL** | | | **≈ 34 650 €** |
| P_max / réaliste | 280+110+60+90+50+150 | | **≈ 0,74 kW / 0,55 kW** |

Capacité brute : 240 To HDD + 15 To NVMe. Les HDD consomment peu (≈ 8–10 W
chacun en charge) mais **vibrent** : ne mélangez jamais HDD et NVMe sans
isolation mécanique correcte (le châssis 4U est conçu pour).

---

## 55. Nœud OSD hybride M — BOM (24× 20 To HDD + 6× 7,68 To NVMe)

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 4U 36 baies, 2P EPYC | 4U36 2P nu | 1 | 5 500 € |
| EPYC 9555P (64c) | 2× | 2 | 4 830 € × 2 = 9 660 € |
| RAM 24× 32 Go = 768 Go | DDR5 ECC | 24 | 18 000 € |
| 2× M.2 960 Go RAID 1 (OS) | — | 2 | 360 € |
| 24× HDD 20 To CMR | Exos X20 | 24 | 380 € × 24 = 9 120 € |
| 6× NVMe 7,68 To (cache/WAL) | PM9A3 | 6 | 3 915 € × 6 = 23 490 € |
| NIC 2× 25 GbE (front) | E810 | 1 | 450 € |
| NIC 2× 100 GbE (back) | CX6-DX | 1 | 1 100 € |
| PSU 2× 2 000 W | incluses | — | — |
| **TOTAL** | | | **≈ 67 680 €** |
| P_max / réaliste | 720+240+190+140+85+150 | | **≈ 1,53 kW / 1,1 kW** |

Capacité brute : 480 To HDD + 46 To NVMe par nœud.

---

## 56. Ratios de cache : combien de NVMe pour combien de HDD

| Workload | Ratio NVMe:HDD | Exemple (nœud M) |
|---|---|---|
| Objet S3 froid (backup, archives) | 1:20 | 24 To NVMe / 480 To HDD ✓ |
| Objet tiède (VM images) | 1:10 | 48 To NVMe / 480 To HDD |
| RBD bloc actif | 1:5 ou full NVMe | — (prenez du full NVMe, §41) |

Le NVMe sert à : BlueStore DB/WAL (10–30 Go par OSD HDD), cache Ceph
(read/write), et pools « chauds » séparés. **Ne mettez jamais le WAL BlueStore
sur le HDD lui-même** : c'est −50 % de perfs d'écriture.

---

## 57. Cas chiffré : 2 Po utiles hybrides, EC 4+2

Besoin brut : 2 000 / 0,85 / (4/6) = **3 530 To bruts** → 8 nœuds M
(8 × 480 = 3 840 To HDD).

| Poste | Détail | Total |
|---|---|---|
| 8× nœuds hybrides M | 67 680 € × 8 | 541 440 € |
| 3× MON/MGR (section 44) | 40 455 € | 40 455 € |
| 2× switch 25G/100G | — | 25 000 € |
| Câblage | lot | 8 000 € |
| **CAPEX** | | **≈ 614 900 €** |
| P_max | 8 × 1,53 + 3 × 0,45 + 1 | **≈ 14,6 kW** |
| **Coût/To utile** | 614 900 / 2 000 | **≈ 307 €/To** |

**307 €/To vs 3 090 €/To en full NVMe : un facteur 10.** C'est pour ça que
l'hybride existe. Réservez le NVMe à ce qui a besoin de latence.

---

## 58. Comparatif coût NVMe vs hybride (€/To utile, 2026)

| Architecture | €/To utile | Latence typique | Usage |
|---|---|---|---|
| Full NVMe réplica 3 | ≈ 3 100 € | < 1 ms | RBD, DB, VM |
| Full NVMe EC 4+2 | ≈ 1 600 € (à vérifier) | 1–3 ms | Objet perf |
| Hybride EC 4+2 | ≈ 310 € | 5–20 ms | Objet froid, backup |
| HDD seul EC 8+3 (à vérifier) | ≈ 180 € | 10–30 ms | Archives |

---

## 59. Pièges terrain — Ceph hybride

1. **HDD SMR « pas cher »** : le SMR (shingled) est inutilisable en Ceph
   (réécritures catastrophiques). **CMR uniquement** — vérifiez la fiche.
2. **Rebuild HDD de 20 To** : à 200 Mo/s = **28 heures** par disque. Pendant ce
   temps, un 2ᵉ disque qui lâche = perte de données en réplica 3 dégradé.
   Prévoyez des spares et surveillez le SMART.
3. **Vibrations** : 24 HDD qui vibrent dans un rack mal fixé tuent les perfs
   de tous les disques. Serrez les rails, évitez les racks bas de gamme.
4. **Température HDD** : 25–40 °C idéal, 50 °C = vieillissement accéléré.
   Les 4U 36 baies ont besoin d'un flux d'air sérieux en façade.
5. **Mélanger CMR et NVMe dans le même CRUSH bucket** sans device class :
   Ceph répartit au hasard → perfs imprévisibles. **Device classes
   `hdd`/`ssd` obligatoires.**

---

## 60. Ce qu'il faut retenir — Ceph hybride

- 1 To NVMe pour 10–20 To HDD, WAL BlueStore sur NVMe, CMR uniquement.
- ≈ 310 €/To utile en EC 4+2 : 10× moins cher que le full NVMe.
- Le rebuild d'un 20 To prend 28 h : spares + surveillance SMART.
- Device classes Ceph : séparez `hdd` et `ssd` dès le jour 1.

---

## 61. Workload 5 — VDI : principes

La VDI se dimensionne **par utilisateur**, pas par serveur :
- **CPU** : 2–4 vCPU par utilisateur (bureautique 2, CAO 4–8).
- **RAM** : 4–8 Go par utilisateur (bureautique 4, CAO 16).
- **IOPS** : 20–50 par utilisateur, **100+ au boot storm** (8h55, tout le monde
  allume son poste en même temps).
- **GPU** : vGPU NVIDIA (vPC pour bureau, vWS pour CAO) quand l'appli l'exige.

Deux builds : **S** (200 users bureautiques) et **M** (400 users dont CAO avec vGPU).

---

## 62. Tableau des profils utilisateurs VDI

| Profil | vCPU | RAM | IOPS pic | GPU | Exemples |
|---|---|---|---|---|---|
| Tâche (call center) | 1–2 | 4 Go | 20 | non | saisie, téléphonie |
| Bureautique | 2 | 6–8 Go | 50 | non | Office, navigateur |
| Développeur | 4 | 16 Go | 80 | optionnel | IDE, builds |
| CAO/DAO | 4–8 | 16–32 Go | 100 | vWS 4–8 Go | AutoCAD, Revit |

Règle CPU : **surallocation 4:1** max en bureautique (4 vCPU pour 1 pCPU),
2:1 en CAO. Au-delà, la latence se ressent au clavier.

---

## 63. Serveur VDI S — BOM (200 users bureautiques)

Cible : 200 × 2 vCPU = 400 vCPU (ratio 4:1 → 100 pCPU), 200 × 8 Go = 1,6 To RAM.

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 2U 2P EPYC | 2U 2P nu | 1 | 4 200 € |
| EPYC 9655P (96c) — 192 cœurs au total | 2× | 2 | 5 208 € × 2 = 10 416 € |
| RAM 24× 64 Go = 1,5 To | DDR5 ECC | 24 | 33 120 € |
| 2× M.2 960 Go RAID 1 (hyperviseur) | — | 2 | 360 € |
| 4× NVMe 7,68 To (images, linked clones) | PM9A3 | 4 | 3 915 € × 4 = 15 660 € |
| NIC 2× 25 GbE | E810 | 1 | 450 € |
| PSU 2× 2 000 W | incluses | — | — |
| **TOTAL / serveur (100 users)** | 2 serveurs requis | | **≈ 64 205 € × 2 = 128 410 €** |
| P_max / serveur | | | **≈ 1,35 kW** |

Note : 1,5 To RAM pour 100 users = 15 Go/user brut, soit 8 Go/user + marge N+1.
La RAM est **le poste n° 1** de la VDI en 2026.

---

## 64. Serveur VDI M — BOM (200 users CAO avec vGPU)

Cible : 200 × 4 vCPU, 200 × 16 Go = 3,2 To RAM, vGPU 4 Go/user.

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 2U 4× GPU double-slot | ASUS ESC4000A-E12 nu | 1 | ≈ 5 000 € |
| EPYC 9555P (64c) | 2× | 2 | 4 830 € × 2 = 9 660 € |
| RAM 24× 128 Go = 3 To | DDR5 ECC | 24 | ≈ 2 750 € × 24 = 66 000 € |
| 2× M.2 960 Go RAID 1 | — | 2 | 360 € |
| 4× NVMe 7,68 To | PM9A3 | 4 | 15 660 € |
| 4× NVIDIA L40S 48 Go (vWS) | — | 4 | ≈ 8 000 € × 4 = 32 000 € |
| NIC 2× 25 GbE | E810 | 1 | 450 € |
| PSU 2× 2 600 W | incluses châssis | — | — |
| **TOTAL / serveur (100 users CAO)** | 2 serveurs | | **≈ 129 130 € × 2 = 258 260 €** |
| P_max / serveur | 720+240+120+1400+50+150 | | **≈ 2,68 kW** |

4× L40S = 192 Go VRAM → 48 users à 4 Go par GPU, 192 users par serveur à
vWS 4 Go. **2,68 kW par serveur : c'est du rack haute densité** (air limite,
RDHx recommandé — section 147).

---

## 65. vGPU NVIDIA : licences et découpage

| Licence | Usage | Prix indicatif |
|---|---|---|
| vPC | Bureau virtualisé | ≈ 50–100 €/user/an (à vérifier) |
| vWS | CAO/3D | ≈ 200–300 €/user/an (à vérifier) |
| vApps | RDSH applicatif | ≈ 30–50 €/user/an (à vérifier) |

- Découpage : L40S 48 Go → 12× vGPU 4 Go (vWS) ou 24× vGPU 2 Go (vPC).
- **Alternative sans licence** : GPU passthrough 1:1 (1 GPU = 1 VM puissante)
  ou AMD MxGPU/SR-IOV (pas de licence, écosystème plus restreint).
- Comptez les licences sur 5 ans dans le TCO : 200 users × 250 € × 5 ans =
  **250 000 €** — plus cher que les GPU eux-mêmes.

---

## 66. Stockage VDI : le boot storm décide

- 200 users × 100 IOPS pic = **20 000 IOPS** pendant 10 minutes à 8h55.
- 4× NVMe PM9A3 = 800 k IOPS écriture : largement couvert.
- **Linked clones / instant clones** : 1 image mère + deltas → divise le
  stockage par 5–10. Sans ça, 200 × 60 Go = 12 To par serveur.
- Tiering : images sur NVMe local (pas sur Ceph — latence), profils utilisateurs
  sur CephFS/NAS.

---

## 67. Cas chiffré : 300 postes bureautiques VDI complets

| Poste | Détail | Total |
|---|---|---|
| 3× serveurs VDI S (N+1 : 2+1) | 64 205 € × 3 | 192 615 € |
| Licences VMware Horizon ou Citrix (300 users) | ≈ 200 €/user/an | 60 000 €/an |
| Stockage profils (NAS 20 To) | — | 15 000 € |
| Switches 25G (mutualisés) | — | — |
| Clients légers (300× 400 €) | thin clients | 120 000 € |
| **CAPEX année 1** | | **≈ 327 600 € + 60 k€/an** |
| P_max serveurs | 3 × 1,35 kW | **≈ 4 kW** |
| **Coût/poste/an (5 ans)** | (327 600 + 300 000) / 300 / 5 | **≈ 418 €** |

À comparer à un PC fixe à 800 € renouvelé tous les 5 ans (160 €/an) : la VDI
ne se justifie **que** par la sécurité, le nomadisme ou la CAO centralisée.
Ne vendez jamais la VDI comme « moins cher » — vendez-la comme « plus sûr ».

---

## 68. Réseau VDI : le protocole d'affichage

- **Blast/PCoIP/HDX** : 150–500 kb/s par session bureautique, 2–5 Mb/s en CAO.
- 300 sessions × 300 kb/s = 90 Mb/s : un lien 1G suffit… mais la **latence**
  doit être < 50 ms (RTT). Au-delà, les utilisateurs détestent.
- Séparez le trafic d'affichage du trafic de management (VLAN dédié).

---

## 69. Pièges terrain — VDI

1. **Sous-dimensionner la RAM** : la VDI est un workload RAM. Le CPU peut
   attendre, la RAM non.
2. **Oublier le boot storm** : tout marche en test à 14h, tout s'écroule à
   8h55 le lundi. Testez à 100 % de connexions simultanées.
3. **vGPU sans licences comptées** : le coût des licences dépasse le matériel
   sur 5 ans.
4. **Profils itinérants de 20 Go** : le login prend 10 minutes. FSLogix ou
   équivalent, quotas stricts.
5. **VDI sur Wi-Fi d'hôtel** : 200 ms de latence = inutilisable. Prévoyez un
   mode dégradé ou des PC locaux pour les nomades extrêmes.

---

## 70. Ce qu'il faut retenir — VDI

- 8 Go RAM + 2 vCPU par user bureautique, 16 Go + 4 vCPU + vGPU en CAO.
- 100 IOPS/user au boot storm : NVMe local obligatoire.
- Coût 2026 : ≈ 420 €/poste/an tout compris — la sécurité, pas le prix.
- 2,7 kW par serveur vGPU : prévoyez le refroidissement (RDHx).

---

## 71. Workload 6 — SOC : principes

Un SOC se dimensionne **au volume de logs par jour** (Go/j ou EPS —
événements/seconde). La chaîne : collecte → parsing/normalisation → indexation
→ détection → rétention.
- **1 To/j ≈ 12 000 EPS** en moyenne (ordre de grandeur, dépend des sources).
- Rétention typique : 90 j chauds (recherche rapide) + 1 an tiède/froid.
- Les SIEM open source (Wazuh, ELK) se dimensionnent comme les commerciaux,
  la licence en moins.

---

## 72. Tableau volumétrie → sizing

| Volume/jour | EPS moyen | Nœuds indexation | Stockage 90 j chauds | Profil |
|---|---|---|---|---|
| 100 Go | 1 200 | 1 | 9 To | PME |
| 500 Go | 6 000 | 2–3 | 45 To | ETI |
| 1 To | 12 000 | 3–4 | 90 To | Grand compte |
| 2 To | 24 000 | 6–8 | 180 To | SOC mutualisé |
| 5 To | 60 000 | 12–16 | 450 To | Opérateur |

Stockage chaud = volume × 90 j × facteur_index (≈ 1,1–1,5 selon parsing).
Le tiède/froid (objet S3/Ceph) coûte 10× moins cher : **ne gardez en chaud que
ce que vous recherchez vraiment**.

---

## 73. Serveur SIEM S — BOM (500 Go/j, Wazuh/ELK)

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 2U 1P EPYC, 12 baies | 2U 1P nu | 1 | 3 800 € |
| EPYC 9455P (48c) — l'indexation aime les cœurs | — | 1 | 3 406 € |
| RAM 12× 64 Go = 768 Go | DDR5 ECC | 12 | 16 560 € |
| 2× M.2 960 Go RAID 1 (OS) | — | 2 | 360 € |
| 8× NVMe 7,68 To (hot tier 61 To) | PM9A3 | 8 | 3 915 € × 8 = 31 320 € |
| NIC 2× 25 GbE | E810 | 1 | 450 € |
| PSU 2× 1 600 W | incluses | — | — |
| **TOTAL / nœud** | | | **≈ 55 895 €** |
| P_max / réaliste | | | **≈ 0,95 kW / 0,7 kW** |

---

## 74. Serveur SIEM M — BOM (2 To/j, cluster 3 nœuds)

Par nœud (×3, N+1) :

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 2U 2P EPYC, 24 baies | type 2U24E nu | 1 | ≈ 3 970 € |
| EPYC 9655P (96c) | 2× | 2 | 5 208 € × 2 = 10 416 € |
| RAM 24× 64 Go = 1,5 To | DDR5 ECC | 24 | 33 120 € |
| 2× M.2 960 Go RAID 1 (OS) | — | 2 | 360 € |
| 16× NVMe 7,68 To (123 To bruts/nœud) | PM9A3 | 16 | 62 640 € |
| NIC 2× 100 GbE (ingest + réplication) | CX6-DX | 1 | 1 100 € |
| PSU 2× 2 000 W | incluses | — | — |
| **TOTAL / nœud** | | | **≈ 111 605 €** |
| **× 3 nœuds** | | | **≈ 334 815 €** |
| P_max / nœud | | | **≈ 1,6 kW** |

Capacité chaude cluster : 3 × 123 To × 0,85 ≈ 313 To → **≈ 170 j de rétention
chaude à 2 To/j** (avec réplica 2 du SIEM). Confortable.

---

## 75. Stockage tiède/froid : l'objet Ceph

- Au-delà de 90 j, basculez vers un **pool objet Ceph EC 4+2** (section 57) :
  ≈ 310 €/To utile vs 3 100 €/To en NVMe chaud.
- 2 To/j × 275 j (reste de l'année) = 550 To/an → **≈ 170 000 €** en hybride
  vs 1,7 M€ en NVMe. Le tiering n'est pas une option, c'est une obligation.
- Cycle de vie S3 (ILM) : automatisez la bascule chaud → tiède à J+90.

---

## 76. Réseau de capture : SPAN, TAP, NIC

| Besoin | Solution | Prix indicatif |
|---|---|---|
| Capture 1–10G | SPAN sur switch existant | 0 € (mais perte de paquets en surcharge) |
| Capture 10G fiable | TAP optique passif | ≈ 500–1 500 €/lien (à vérifier) |
| Sonde NDR 25G | NIC avec timestamp hardware | ≈ 1 500 € (à vérifier) |
| Agrégation | Packet broker (Garland, Cubro) | 10 000 €+ (à vérifier) |

Règle : **SPAN pour le dépannage, TAP pour la sécurité**. Un attaquant qui
sature le switch fait disparaître les paquets du SPAN — exactement ceux que
vous vouliez voir.

---

## 77. Cas chiffré : SOC 1 To/j complet (collecte + SIEM + froid)

| Poste | Détail | Total |
|---|---|---|
| 3× SIEM S (N+1, 500 Go/j chacun) | 55 895 € × 3 | 167 685 € |
| 2× collecteurs/syslog (1U légers) | 12 000 € × 2 | 24 000 € |
| Stockage froid 1 an (365 To utiles → Ceph hybride) | ≈ 310 €/To | 113 150 € |
| TAP + packet broker | lot | 15 000 € |
| Licences (Wazuh = 0 € ; commercial ≈ 100–200 k€/an) | open source | 0 € |
| **CAPEX** | | **≈ 319 800 €** |
| P_max | 3 × 0,95 + 2 × 0,4 + stockage | **≈ 5 kW** |

Avec un SIEM commercial (Splunk/QRadar), ajoutez **150–300 k€/an de licences**
au volume 1 To/j — souvent plus que le matériel sur 5 ans.

---

## 78. Rétention légale : ce que la loi impose (France)

- Logs de connexion (LCEN) : **1 an** pour les opérateurs/hébergeurs.
- Journaux de sécurité : pas de durée légale unique, mais l'ANSSI recommande
  6–13 mois pour la détection d'intrusions lentes (dwell time médian > 200 j).
- **Chiffrez les archives**, contrôlez l'accès (données personnelles), et
  documentez la politique de rétention (RGPD : minimisation).

---

## 79. Pièges terrain — SOC

1. **Collecter sans parser** : 1 To/j de logs bruts inexploitables = 1 To/j
   de coût disque pour zéro détection. Le parsing est le vrai travail.
2. **Rétention chaude infinie** : le NVMe coûte 10× l'objet. Tiering à J+90,
   sans exception.
3. **EPS pic vs moyen** : un ransomware génère 10× l'EPS normal. Si la file
   d'ingest déborde pendant l'attaque, vous perdez exactement les logs de
   l'attaque. Surdimensionnez l'ingest ×3.
4. **Horloges désynchronisées** : 1 seconde d'écart entre sources = corrélation
   impossible. NTP/Chrony partout, monitoré.
5. **Tester la restauration des archives froides** : un objet Ceph illisible à
   J+300 = rétention légale non conforme.

---

## 80. Ce qu'il faut retenir — SOC

- Dimensionnez à l'EPS et au Go/j, pas « au nombre de serveurs ».
- 90 j chauds NVMe + 1 an froid objet : le tiering fait le budget.
- Ingest ×3 vs moyenne, TAP plutôt que SPAN, NTP partout.
- Open source (Wazuh/ELK) : 0 € de licence ; commercial : 150–300 k€/an à 1 To/j.

---

## 81. Workload 7 — Inférence AMD : pourquoi, quand

Le MI300X (192 Go HBM3, 750 W) est l'arme **mémoire/prix** de 2026 :
- 192 Go sur une carte = un **70B en FP8/FP16 sur 1–2 cartes** au lieu de 4–8.
- Prix standalone ≈ 10 000–12 000 $ (vérifié le 27/09/2026) vs H100 à
  25 000–35 000 $ : **2–3× moins cher au Go de HBM**.
- Location cloud : ≈ 1,99 $/GPU/h (DigitalOcean, vérifié le 27/09/2026).
- **ROCm en 2026** : mature pour vLLM/SGLang sur les modèles courants
  (Llama, Qwen, Mistral). Vérifiez toujours la compatibilité de VOTRE modèle
  avant d'acheter — quelques architectures exotiques restent CUDA-only.

Quand choisir AMD : inférence de gros modèles, budget contraint, pas de besoin
CUDA propriétaire. Quand rester NVIDIA : training, écosystème CUDA verrouillé,
NVLink multi-nœuds.

---

## 82. Serveur 2× MI300X — BOM

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 2U 2× GPU OAM/HGX AMD | plateforme MI300X 2-GPU (à vérifier — devis intégrateur) | 1 | ≈ 25 000 € |
| 2× AMD Instinct MI300X 192 Go | — | 2 | ≈ 10 000 € × 2 = 20 000 € |
| CPU EPYC 9555P (64c) | — | 1–2 | ≈ 4 830 € |
| RAM 12× 64 Go = 768 Go | DDR5 ECC | 12 | 16 560 € |
| 2× NVMe 3,84 To (modèles + cache) | PM9A3 | 2 | 6 070 € |
| NIC 2× 100 GbE (front) | CX6-DX | 1 | 1 100 € |
| PSU 3 000 W redondantes | incluses | — | — |
| **TOTAL** | | | **≈ 77 560 €** |
| P_max / réaliste | 2×750+360+120+60+60+150 | | **≈ 2,3 kW / 1,8 kW** |

384 Go HBM au total : un **70B FP8 (~70 Go) + KV cache confortable** sur les
2 cartes, ou 2× 32B en parallèle.

---

## 83. Serveur 4× MI300X — BOM (« quad »)

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 4U 4× MI300X | plateforme 4-GPU (devis intégrateur) | 1 | ≈ 40 000 € |
| 4× MI300X 192 Go | — | 4 | 10 000 € × 4 = 40 000 € |
| 2× EPYC 9555P (64c) | — | 2 | 9 660 € |
| RAM 24× 64 Go = 1,5 To | DDR5 ECC | 24 | 33 120 € |
| 4× NVMe 7,68 To (modèles) | PM9A3 | 4 | 15 660 € |
| 2× NIC 200/400 GbE (scale-out) | — | 2 | ≈ 3 000 € × 2 = 6 000 € |
| PSU 4× 3 000 W (N+1) | incluses | — | — |
| **TOTAL** | | | **≈ 144 440 €** |
| P_max / réaliste | 4×750+720+240+120+120+200 | | **≈ 4,4 kW / 3,5 kW** |

768 Go HBM : **405B FP8 (~405 Go) sur un seul nœud**, ou 4× 70B en parallèle.
4,4 kW dans 4U : **refroidissement liquide ou RDHx obligatoire** (section 145+).

---

## 84. MI325X / MI355X : positionnement (prix non publics)

| GPU | HBM | TDP | Statut prix au 27/09/2026 |
|---|---|---|---|
| MI300X | 192 Go HBM3 | 750 W | 10 000–12 000 $ standalone ✓ |
| MI325X | 256 Go HBM3e | 750 W | non trouvé (prix public) |
| MI355X | 288 Go HBM3e | 1 000 W | non trouvé (prix public) — location ≈ 3 $/h |

Le MI355X vise le segment H200/B200. Sans prix public, **ne le chiffrez pas
dans une BOM ferme** : demandez un devis ou louez à l'heure pour valider.

---

## 85. VRAM vs taille de modèle — tableau de choix

| Modèle | FP16 | FP8 | Config mini recommandée |
|---|---|---|---|
| 8B | 16 Go | 8 Go | 1× L40S / 1× MI300X (large) |
| 32B | 64 Go | 32 Go | 1× L40S 48 Go (FP8) / 1× MI300X |
| 70B | 140 Go | 70 Go | 1× MI300X / 2× L40S / 1× RTX PRO 6000 (FP8) |
| 405B | 810 Go | 405 Go | 3× MI300X (FP8) / 6× H100 / 8× B200 |

Règle : **poids + 30 % (KV cache, overhead)** ≤ VRAM totale. Le FP8 divise par
2 vs FP16 avec une perte qualité faible sur les modèles récents — c'est le
format d'inférence par défaut en 2026.

---

## 86. vLLM / SGLang : sizing pratique

- **Débit** : un 70B FP8 sur 2× MI300X sort ≈ 100–200 tok/s en batch modeste
  (à vérifier sur votre modèle — mesurez avec un benchmark).
- **Parallélisme** : tensor-parallel sur les GPU du nœud, data-parallel entre
  réplicas pour la concurrence.
- **Quantification** : AWQ/GPTQ (poids seuls) ou FP8 (poids+activations) —
  le FP8 natif Blackwell/MI300 est le plus simple.
- **RAM CPU** : 2× la taille du modèle pour le chargement/déchargement
  (70B → 140 Go RAM CPU mini).

---

## 87. Cas chiffré : servir un 70B FP8 à 50 utilisateurs

| Poste | Détail | Total |
|---|---|---|
| 1× serveur 2× MI300X (section 82) | — | 77 560 € |
| Switch 100G (partagé) | quote-part | 5 000 € |
| Électricité (1,8 kW × 8 760 h × 0,20 €) | /an | 3 150 €/an |
| **CAPEX** | | **≈ 82 500 €** |
| **Coût/1k tokens** (10 M tok/j) | 3 150 / 365 / 10 000 | **≈ 0,0009 €** |

À comparer à l'API cloud : 70B à ≈ 0,50–0,90 $/M tokens → 10 M tok/j =
**5 000–9 000 $/j**. Le serveur est amorti en **moins de 3 semaines** à ce
rythme. **L'on-premise gagne dès que l'usage est soutenu** — c'est le calcul
à refaire pour chaque projet IA.

---

## 88. Refroidissement nœud MI300X

- 2× MI300X (2,3 kW) : air possible en 2U bien ventilé, RDHx conseillé.
- 4× MI300X (4,4 kW) : **DLC ou RDHx obligatoire** — l'air seul ne suit plus
  dans un datacenter à 25 °C.
- Les HBM sont sensibles à la température : **T_junction < 95 °C**,
  throttling au-delà. Surveillez via `rocm-smi`.

---

## 89. Pièges terrain — Inférence AMD

1. **Acheter avant de tester le modèle** : 5 % des architectures ne passent
   pas sous ROCm. Louez 10 h à 1,99 $/h pour valider AVANT la BOM.
2. **Oublier la RAM CPU** : le chargement d'un 405B demande 800 Go de RAM
   système — sinon OOM au démarrage.
3. **NVLink vs Infinity Fabric** : la bande passante inter-GPU AMD (Infinity
   Fabric) est bonne en intra-nœud, mais l'écosystème multi-nœuds reste
   derrière NVLink. Restez en intra-nœud pour l'inférence.
4. **Pilotes ROCm** : épinglez la version (6.x validée avec votre vLLM),
   ne suivez pas « latest » en prod.

---

## 90. Ce qu'il faut retenir — Inférence AMD

- 192 Go HBM à 10–12 k$ : le meilleur €/Go VRAM du marché 2026.
- 2× MI300X = 70B FP8 pour ≈ 78 k€ ; 4× = 405B FP8 pour ≈ 144 k€.
- Validez le modèle à l'heure en cloud avant d'acheter.
- 4,4 kW en 4U : prévoyez le liquide dès la commande.

---

## 91. Workload 8 — Inférence NVIDIA : panorama 2026

| GPU | VRAM | TDP | Prix constaté | Positionnement |
|---|---|---|---|---|
| L40S | 48 Go | 350 W | 8 610–8 900 $ | Le standard inférence PCIe |
| RTX PRO 6000 Blackwell | 96 Go | 600 W | 9 450–9 800 $ | 70B sur 1 carte |
| RTX 6000 Ada | 48 Go | 300 W | 7 400–7 800 $ | Alternative L40S (occasion intéressante) |
| H100 SXM | 80 Go | 700 W | 25–35 k$ neuf | Haute concurrence, SLA prod |
| H200 | 141 Go | 700 W | 30–40 k$ | Gros modèles, training |
| B200 | 192 Go | 1 000 W | 30–40 k$ | Génération Blackwell, délais 3–7 mois |

Tous prix vérifiés le 27/09/2026 (snapshot mars 2026 + MSRP). L'écosystème
CUDA reste l'argument n° 1 : tout tourne, tout est documenté.

---

## 92. Serveur 2× L40S — BOM

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 2U 4× GPU double-slot | ASUS ESC4000A-E12 nu | 1 | ≈ 5 000 € |
| EPYC 9455P (48c) | — | 1 | 3 406 € |
| RAM 12× 64 Go = 768 Go | DDR5 ECC | 12 | 16 560 € |
| 2× NVMe 3,84 To (modèles) | PM9A3 | 2 | 6 070 € |
| 2× NVIDIA L40S 48 Go | — | 2 | ≈ 8 000 € × 2 = 16 000 € |
| NIC 2× 25 GbE (+ 2× 100 GbE option) | E810 | 1 | 450 € |
| PSU 2× 2 600 W | incluses | — | — |
| **TOTAL** | | | **≈ 47 485 €** |
| P_max / réaliste | 300+120+60+700+50+150 | | **≈ 1,38 kW / 1,05 kW** |

96 Go VRAM : 70B en FP8 sur les 2 cartes (tensor-parallel), ou 2× 32B
indépendants.

---

## 93. Serveur 4× L40S — BOM (« quad »)

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 2U 4× GPU double-slot | ASUS ESC4000A-E12 nu | 1 | ≈ 5 000 € |
| EPYC 9555P (64c) | 2× | 2 | 4 830 € × 2 = 9 660 € |
| RAM 24× 64 Go = 1,5 To | DDR5 ECC | 24 | 33 120 € |
| 4× NVMe 7,68 To (modèles) | PM9A3 | 4 | 15 660 € |
| 4× NVIDIA L40S 48 Go | — | 4 | 8 000 € × 4 = 32 000 € |
| NIC 2× 100 GbE | CX6-DX | 1 | 1 100 € |
| PSU 2× 2 600 W | incluses | — | — |
| **TOTAL** | | | **≈ 96 540 €** |
| P_max / réaliste | 720+240+120+1400+60+150 | | **≈ 2,69 kW / 2,0 kW** |

192 Go VRAM : 2× 70B FP8 en parallèle, ou 1× 70B FP16 confortable.

---

## 94. Serveur 2× RTX PRO 6000 96 Go — BOM

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 2U 4× GPU (600 W/GPU !) | plateforme 600W-capable (devis — à vérifier) | 1 | ≈ 6 500 € |
| EPYC 9455P (48c) | — | 1 | 3 406 € |
| RAM 12× 64 Go = 768 Go | DDR5 ECC | 12 | 16 560 € |
| 2× NVMe 3,84 To | PM9A3 | 2 | 6 070 € |
| 2× RTX PRO 6000 96 Go | — | 2 | ≈ 8 800 € × 2 = 17 600 € |
| NIC 2× 25 GbE | E810 | 1 | 450 € |
| PSU 2× 3 000 W | — | — | incluses |
| **TOTAL** | | | **≈ 50 585 €** |
| P_max / réaliste | 300+120+60+1200+50+150 | | **≈ 1,88 kW / 1,4 kW** |

**Attention : 600 W par GPU** — vérifiez que le châssis et le refroidissement
sont qualifiés 600 W/GPU. Tous les 2U 4-GPU ne le sont pas.

---

## 95. Serveur 4× RTX PRO 6000 96 Go — BOM (« quad »)

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 4U 4× GPU 600 W | plateforme qualifiée (devis) | 1 | ≈ 9 000 € |
| 2× EPYC 9555P (64c) | — | 2 | 9 660 € |
| RAM 24× 64 Go = 1,5 To | DDR5 ECC | 24 | 33 120 € |
| 4× NVMe 7,68 To | PM9A3 | 4 | 15 660 € |
| 4× RTX PRO 6000 96 Go | — | 4 | 8 800 € × 4 = 35 200 € |
| NIC 2× 100 GbE | CX6-DX | 1 | 1 100 € |
| PSU 4× 3 000 W (N+1) | incluses | — | — |
| **TOTAL** | | | **≈ 103 740 €** |
| P_max / réaliste | 720+240+120+2400+60+200 | | **≈ 3,74 kW / 2,8 kW** |

384 Go VRAM sur 4 cartes : 4× 70B FP8 indépendants, ou 405B FP8 en
tensor-parallel sur le nœud. 3,74 kW : **DLC/RDHx obligatoire**.

---

## 96. H100 / H200 : quand les choisir

| Critère | L40S / RTX PRO 6000 | H100 / H200 |
|---|---|---|
| Prix/GPU | 8–10 k$ | 25–40 k$ |
| Bande passante mémoire | 864–1 792 Go/s | 2 000–4 800 Go/s |
| Concurrence (req/s) | moyenne | très haute |
| NVLink | non (PCIe) | oui (SXM) |
| Quand | inférence classique | SLA prod, batch énorme |

Règle : **tant que la concurrence < 50 req/s sur un 70B, le PCIe suffit**.
Le H100 ne se justifie que par la bande passante HBM ou le NVLink 8-GPU.

---

## 97. Tableau de choix GPU inférence 2026

| Besoin | Choix prix/perf | Budget | P_max |
|---|---|---|---|
| 8–32B, dev | 2× L40S | ≈ 47 k€ | 1,4 kW |
| 70B FP8, 1 carte | 1–2× RTX PRO 6000 96 Go | ≈ 35–50 k€ | 1,2–1,9 kW |
| 70B, petit prix | 2× MI300X (AMD) | ≈ 78 k€ | 2,3 kW |
| 70B haute concurrence | 4× L40S | ≈ 97 k€ | 2,7 kW |
| 405B FP8 | 4× MI300X ou 4× RTX PRO 6000 | ≈ 104–144 k€ | 3,7–4,4 kW |

---

## 98. Cas comparé : 70B FP8 — 1× RTX PRO 6000 vs 2× L40S

| Critère | 1× RTX PRO 6000 96 Go | 2× L40S 48 Go |
|---|---|---|
| VRAM totale | 96 Go (1 carte) | 96 Go (2 cartes) |
| Prix GPU | ≈ 8 800 € | ≈ 16 000 € |
| Serveur total | ≈ 35 000 € | ≈ 47 500 € |
| P_max GPU | 600 W | 700 W |
| Simplicité | 1 carte, pas de TP | tensor-parallel requis |
| Verdict | **gagne** si le modèle tient en 96 Go | utile si déjà en parc |

---

## 99. Pièges terrain — Inférence NVIDIA

1. **Le 600 W du RTX PRO 6000** : beaucoup de châssis 4-GPU sont qualifiés
   350 W/GPU (génération L40S). Vérifiez la qualification thermique par écrit.
2. **PCIe 4.0 vs 5.0** : un GPU PCIe 5.0 dans un slot 4.0 perd peu en inférence,
   mais vérifiez quand même la négociation (GPU-Z/nvidia-smi).
3. **ECC VRAM** : activé par défaut sur les datacenter (L40S), pas sur les
   workstation. Pour de la prod, activez-le (perte ~6 % VRAM, fiabilité).
4. **Délais Blackwell** : 3–7 mois début 2026. Ne planifiez pas une mise en
   prod à J+30 avec du RTX PRO 6000.
5. **Refroidissement des 600 W** : à 30 °C ambiants, un 600 W air-cooled
   throttle. 24 °C max en salle, ou liquide.

---

## 100. Ce qu'il faut retenir — Inférence NVIDIA

- L40S = standard 48 Go (≈ 47 k€ le serveur 2×) ; RTX PRO 6000 = 70B sur
  1 carte (≈ 35 k€ le serveur 1×).
- 600 W/GPU : qualification thermique du châssis obligatoire.
- H100/H200 uniquement pour haute concurrence ou NVLink.
- Le PCIe suffit jusqu'à ~50 req/s sur 70B.

---

## 101. Workload 9 — Backup : principes

La règle **3-2-1** : 3 copies, 2 supports différents, 1 hors site.
Le serveur backup se dimensionne à :
1. **La volumétrie source** × taux de dédup × rétention (formule §104).
2. **La fenêtre de sauvegarde** → débit réseau/disques (formule §11).
3. **La volumétrie de restauration** → le restore doit tenir dans le RTO.

Logiciels : Veeam, Proxmox Backup Server (déduplication excellente),
Bareos, restic/Borg pour le fichier.

---

## 102. Serveur backup S — BOM (50 To utiles)

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 2U 12 baies 3,5", 1P EPYC | 2U 12× 3,5" nu | 1 | 3 800 € |
| EPYC 9355P (32c) — la dédup aime les cœurs | — | 1 | 2 891 € |
| RAM 12× 32 Go = 384 Go (cache dédup) | DDR5 ECC | 12 | 9 000 € |
| 2× M.2 960 Go RAID 1 (OS) | — | 2 | 360 € |
| 2× NVMe 3,84 To (landing zone, cache) | PM9A3 | 2 | 6 070 € |
| 10× HDD 12 To CMR (RAID 60 ou ZFS raidz2) | Exos | 10 | ≈ 280 € × 10 = 2 800 € |
| NIC 2× 25 GbE | E810 | 1 | 450 € |
| PSU 2× 1 600 W | incluses | — | — |
| **TOTAL** | | | **≈ 25 370 €** |
| P_max / réaliste | | | **≈ 0,7 kW / 0,5 kW** |

Capacité : 10× 12 To en RAID 60 (2× raidz2 5 disques) ≈ **72 To utiles**.
La landing zone NVMe absorbe les pics d'écriture nocturnes.

---

## 103. Serveur backup M — BOM (200 To utiles)

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Châssis 4U 24 baies 3,5", 1P EPYC | 4U24 nu | 1 | 4 500 € |
| EPYC 9455P (48c) | — | 1 | 3 406 € |
| RAM 12× 64 Go = 768 Go | DDR5 ECC | 12 | 16 560 € |
| 2× M.2 960 Go RAID 1 (OS) | — | 2 | 360 € |
| 4× NVMe 7,68 To (landing zone) | PM9A3 | 4 | 15 660 € |
| 20× HDD 20 To CMR (ZFS raidz2 ×2) | Exos X20 | 20 | 380 € × 20 = 7 600 € |
| NIC 2× 25 GbE | E810 | 1 | 450 € |
| PSU 2× 1 600 W | incluses | — | — |
| **TOTAL** | | | **≈ 48 535 €** |
| P_max / réaliste | | | **≈ 0,85 kW / 0,65 kW** |

Capacité : 20× 20 To en 2× raidz2 (8+2) ≈ **320 To bruts → ~290 To utiles**.
La RAM (768 Go) sert de cache ARC ZFS : le restore des petits fichiers est
transformé.

---

## 104. Dimensionnement à la volumétrie — la formule

```
Capacité = sources × (1 + incr_quot × jours + full_sup) × (1 + rétention_mois/12) / taux_dédup
```

Exemple : 100 To sources, incr 3 %/j, full hebdo, rétention 3 mois, dédup 2:1
(PBS sur VMs : 2–4× réaliste) :
- 100 × (1 + 0,03×6 + 1) × 1,25 / 2 = **136 To utiles**.
→ Serveur S (72 To) trop petit, **serveur M (290 To) avec marge**.

Taux de dédup réalistes 2026 :
- VMs bureautiques (PBS) : 3–5×.
- DB (données uniques) : 1,2–1,5×.
- Fichiers bureautiques : 1,5–2×.
- **Ne comptez jamais plus de 2× sans mesure sur VOS données.**

---

## 105. Fenêtre de sauvegarde → débit (exemple)

100 To en 8 h (voir §11) : **≈ 10 Gb/s** → NIC 25 GbE.
Règles :
- Full hebdo le week-end (fenêtre 48 h), incr la nuit (8 h).
- **Backup et Ceph scrub jamais en même temps** (§50).
- Immuabilité (object lock S3 / WORM) contre le ransomware : le backup doit
  survivre à un attaquant avec les droits admin. **Sans immuabilité, le 3-2-1
  est une illusion.**

---

## 106. Cas chiffré : 100 To sources, 3-2-1 complet

| Poste | Détail | Total |
|---|---|---|
| Serveur backup M local | §103 | 48 535 € |
| Copie 2 (NAS secondaire ou Ceph objet) | 150 To utiles | 25 000 € |
| Copie 3 hors site (S3 immuable, 150 To) | ≈ 20 €/To/mois (à vérifier) | 36 000 €/an |
| Logiciel (PBS = 0 € ; Veeam ≈ 15 k€/an) | open source | 0 € |
| **CAPEX année 1** | | **≈ 73 500 € + 36 k€/an** |
| P_max local | | **≈ 1 kW** |

---

## 107. Pièges terrain — Backup

1. **Backup non testé** : restaurez 1 VM par mois en exercice. Le jour du
   sinistre n'est pas le jour de la découverte.
2. **Dédup promise par le commercial** : mesurez sur vos données (PBS a un
   mode dry-run).
3. **Fenêtre dépassée** : le backup qui finit à 10h ralentit la prod. Alertez
   sur la durée, pas seulement sur l'échec.
4. **Chiffrement des clés perdues** : un repo chiffré sans escrow des clés =
   perte définitive. 2 copies des clés, 2 lieux.
5. **Ransomware** : immuabilité obligatoire (object lock). Testez la
   restauration depuis la copie immuable.

---

## 108. Workload 10 — Firewall OPNsense : dimensionner AU DÉBIT

Un firewall se dimensionne sur **4 métriques** : débit firewall (Gbps), PPS,
débit IPsec, débit avec IDS/IPS activé (Suricata divise par 5–10).
Le tableau ci-dessous donne le **CPU/NIC minimal** par palier de débit
Internet réel (pas le débit du port — le débit facturé par l'opérateur).

---

## 109. Tableau débit → CPU/NIC (bare-metal OPNsense)

| Débit Internet | CPU minimal | NIC | RAM | Build type |
|---|---|---|---|---|
| ≤ 1 Gb/s | 4c Atom/Celeron récent (N100+) | 2× 2,5 GbE Intel i226 | 8 Go | Mini-PC |
| 2–5 Gb/s | 4–6c Core i3/i5 ou Xeon D-1700 | 2× 10G SFP+ (X710) | 16 Go | 1U léger |
| 10 Gb/s | 8c Xeon D-1733NT / EPYC 9124 | 2–4× 10G SFP+ | 32 Go | 1U (DEC3862 / Netgate 8300) |
| 25 Gb/s | 16c EPYC 9124/9355P | 2× 25G SFP28 (E810) | 32–64 Go | 1U EPYC |
| 40–100 Gb/s | 32c+ EPYC 9355P/9555P | 2× 100G QSFP28 | 64 Go | 1U/2U EPYC |

Repères constructeur (vérifiés le 27/09/2026) :
- Deciso DEC3842 : 14,4 Gbps firewall, 2,3 Gbps IPsec (AES-256-GCM).
- Netgate 8300 (Xeon D-1733NT 8c) : 36,7 Gbps L3, 26,8 Gbps firewall,
  14,6 Gbps IPsec — **3 599 $** base.
- **Avec Suricata IPS** : divisez ces chiffres par 5 environ.

---

## 110. Build firewall 1G — BOM (PME, 1 Gb/s)

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| Mini-PC 4× 2,5 GbE Intel i226, N100+ | barebone | 1 | ≈ 350 € |
| RAM 16 Go DDR5 | — | 1 | 80 € |
| SSD M.2 256 Go | — | 1 | 40 € |
| **TOTAL** | ×2 pour HA | | **≈ 470 € × 2 = 940 €** |
| Conso | | | **≈ 15 W / unité** |

---

## 111. Build firewall 10G — BOM (ETI, 10 Gb/s)

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| 1U EPYC 8c, 4× SFP+ | Deciso DEC3862 ou 1U EPYC nu | 1 | ≈ 2 500 € (appliance) |
| RAM 32 Go ECC | — | 1 | ≈ 750 € (prix 2026 !) |
| SSD 512 Go | — | 1 | 80 € |
| **TOTAL** | ×2 pour HA | | **≈ 3 330 € × 2 = 6 660 €** |
| Conso | | | **≈ 80–120 W / unité** |

Note : 32 Go de RAM à 750 € — la flambée DRAM frappe aussi les firewalls.

---

## 112. Build firewall 25/40G — BOM (datacenter)

| Composant | Référence type | Qté | Prix |
|---|---|---|---|
| 1U EPYC 9124 (16c), 2× 25G SFP28 | AS-1115CS-TNR nu | 1 | 4 585 € |
| EPYC 9124 | — | 1 | 749 € |
| RAM 2× 32 Go = 64 Go ECC | DDR5 | 2 | 1 500 € |
| SSD M.2 512 Go (miroir) | 2× | 2 | 160 € |
| NIC 2× 25G SFP28 (si non incluse) | E810 | 1 | 450 € |
| **TOTAL** | ×2 pour HA | | **≈ 7 445 € × 2 = 14 890 €** |
| Conso | | | **≈ 250 W / unité** |

---

## 113. HA firewall : l'architecture qui ne tombe pas

```
        Internet
            |
     +------+------+
     |             |
  FW-1 (CARP)  FW-2 (CARP)   ← sync états pfsync (lien dédié 10G)
     |             |
     +------+------+
            |
      Switch LAN
```

- **2 nœuds mini**, CARP (OPNsense) ou équivalent, sync des états sur lien
  dédié (pas sur le LAN de prod).
- Testez le **failover réel** (débranchez FW-1) tous les 6 mois.
- Les 2 FW doivent avoir la **même version OPNsense** : MAJ l'un après
  l'autre, jamais les deux ensemble.

---

## 114. Pièges terrain — Firewall

1. **Dimensionner au débit du port** (10G) au lieu du débit réel (2 Gb/s
   opérateur) : on sur-paie ×5. Dimensionnez au débit facturé + 50 %.
2. **Oublier la chute Suricata** : 10 Gb/s annoncés → 2 Gb/s avec IPS.
   Testez AVEC votre ruleset.
3. **NIC Realtek** : pas de pilote stable sous FreeBSD. **Intel uniquement**
   (i226, X710, E810).
4. **HA sans lien de sync dédié** : la sync sur le LAN sature et le failover
   perd les états.
5. **Pas de console série** : quand le FW ne boot plus, l'IPMI/serial est le
   seul accès. Vérifiez avant la mise en rack.

---

## 115. Workload 11 — HSM : principes (pas de BOM interne)

Un HSM (Hardware Security Module) est une **appliance réseau** : on ne la
construit pas, on l'intègre. Rôle : génération, stockage et usage des clés
cryptographiques dans un boîtier inviolable (tamper-evident/resistant),
certifié **FIPS 140-3 niveau 3**.
- Acteurs : **Thales Luna** (leader), **Entrust nShield**, Utimaco, Atos.
- Modèles 2026 : **Thales Luna 8** (annoncé 2026, FIPS 140-3 L3, support
  post-quantique — vérifié le 27/09/2026), Entrust nShield (gammes XC).
- Prix : **sur devis uniquement** (non trouvé au 27/09/2026 en prix public ;
  ordre de grandeur historique : 15 000–50 000 €/appliance selon perfs —
  à vérifier).

---

## 116. Intégration au rack — prérequis

| Prérequis | Détail |
|---|---|
| Format | 1U rackable, 2× alim redondantes |
| Conso | 100–300 W par appliance (à vérifier selon modèle) |
| Réseau | 2× 1/10 GbE dédiés (réseau crypto isolé, VLAN ou physiquement séparé) |
| Redondance | **2 appliances mini** (cluster), jamais une seule |
| Accès | Console série + réseau de management isolé |
| Environnement | 0–35 °C, comme un serveur standard |

Comptez dans la PDU : **2× 0,3 kW** pour la paire HSM, et 2U de rack.

---

## 117. Mise en service HSM : la cérémonie des clés

1. **Cérémonie d'initialisation** : génération de la clé maîtresse avec
   quorum (ex : 3 porteurs sur 5). Filmez/documentez, témoins requis.
2. **Sauvegarde chiffrée** des clés (tokens/clés de récupération) dans **2
   coffres géographiques distincts**.
3. **Intégration applicative** : PKCS#11, JCE, ou API REST selon modèle.
   Prévoyez 2–5 j d'intégration par application.
4. **Plan de rotation** : clés applicatives tous les 1–2 ans, documenté.
5. **Fin de vie** : procédure de destruction (zéroïsation) écrite avant la
   mise en prod.

Un HSM sans cérémonie documentée = un HSM dont personne ne peut prouver
l'intégrité devant un auditeur. **C'est un sujet conformité, pas juste
technique.**

---

## 118. Pièges terrain — HSM

1. **Un seul HSM** : panne = toutes les applis crypto à l'arrêt. Toujours 2.
2. **Clés de récupération dans le même bâtiment** : incendie = perte totale.
3. **Oublier la montée en charge** : un HSM fait 1 000–20 000 op/s selon
   modèle — dimensionnez aux pics (TLS, signatures).
4. **Post-quantique** : en 2026, exigez le support des algo PQC (ML-KEM,
   ML-DSA) — le Luna 8 l'annonce. Un HSM sans PQC est un investissement
   à durée de vie courte.

---

## 119. Workload 12 — Méthode : chiffrer soi-même (template)

Feuille de calcul type (une ligne par serveur, colonnes ci-dessous) :

| Serveur | CPU (TDP) | RAM (Go) | Disques | GPU (TDP) | NIC | P_max | P_réaliste | Prix |
|---|---|---|---|---|---|---|---|---|
| srv-01 | 280 W | 384 | 4×NVMe | — | 25G | 0,62 kW | 0,47 kW | 29 750 € |
| ... | | | | | | | | |

Puis :
1. **Σ P_réaliste** → dimensionnement onduleur + refroidissement (§155, §146).
2. **Σ P_max × 1,25** → dimensionnement PDU + PSU.
3. **Σ prix × 1,1** → budget (10 % intégration/câblage).
4. **TCO 5 ans** : + électricité (§14) + maintenance (10 %/an).

---

## 120. Exemple complet : rack de 8 serveurs mixtes

| Serveur | Type | P_réaliste | Prix |
|---|---|---|---|
| 2× Compute M | §20 | 2 × 0,95 = 1,9 kW | 114 k€ |
| 2× PG OLTP S | §28 | 2 × 0,6 = 1,2 kW | 93 k€ |
| 2× Ceph OSD S | §42 | 2 × 0,6 = 1,2 kW | 99 k€ |
| 1× Backup S | §102 | 0,5 kW | 25 k€ |
| 1× Firewall 10G (paire = 2×) | §111 | 2 × 0,1 = 0,2 kW | 6,7 k€ |
| 2× switch 25G | — | 2 × 0,3 = 0,6 kW | 16 k€ |
| **TOTAL rack** | 11 serveurs + 2 switch | **≈ 5,6 kW** | **≈ 354 k€** |

5,6 kW dans un 42U : **air cooling standard**, 1 PDU 3× 32 A suffit
(22 kW). C'est le rack « PME dense » type.

---

## 121. Marge et croissance : la règle des 3 ans

- Achetez la capacité **J+36 mois**, pas J+0 : un serveur vit 5 ans, le
  remplir à 90 % le jour 1 = racheter dans 18 mois.
- Mais n'achetez pas J+60 mois : la DRAM/GPU de 2029 sera moins chère et
  meilleure. **Le juste milieu : 30 % de marge.**
- Prévoyez **4U libres par rack** et **30 % de ports switch libres**.

---

## 122. Devis : ce qu'il faut exiger du fournisseur

- [ ] Références exactes (CPU stepping, RAM part-number, SSD modèle + firmware)
- [ ] Prix unitaires **et** remise par volume, valables 90 j
- [ ] Délais **écrits** par ligne (surtout GPU et DRAM en 2026)
- [ ] Garantie : durée, NBD vs 4 h, pièces sur site
- [ ] Burn-in inclus ? (24–72 h, rapport fourni)
- [ ] Firmware livré : versions notées
- [ ] Reprise de l'ancien matériel (DEEE)

---

## 123. Benchmark avant achat : les 4 tests

1. **CPU** : `stress-ng --cpu 0 --timeout 3600` + mesure conso à la PDU.
2. **RAM** : `memtester` ou memtest86+ (1 passe complète mini).
3. **Disques** : `fio` randread/randwrite 4k QD32 — comparez à la fiche
   (PM9A3 : ≈ 900 k/200 k IOPS).
4. **Réseau** : `iperf3` bidirectionnel sur chaque NIC à la vitesse nominale.

Un serveur qui ne passe pas ces 4 tests **ne va pas en prod**. Point.

---

## 124. Coûts unitaires de pilotage (€/vCPU, €/To, €/token)

| Métrique | Calcul (exemples du guide) | Valeur 2026 |
|---|---|---|
| €/cœur (compute) | 56 975 € / 192 cœurs | ≈ 297 € |
| €/To utile NVMe | §47 | ≈ 3 090 € |
| €/To utile hybride | §57 | ≈ 307 € |
| €/poste VDI/an | §67 | ≈ 418 € |
| €/M tokens (70B on-prem) | §87 | ≈ 0,9 € |

Suivez ces 5 métriques par trimestre : elles disent si votre infra reste
compétitive vs cloud.

---

## 125. Tableau récapitulatif — consommation de toutes les BOMs

| Build | Section | P_max | P_réaliste |
|---|---|---|---|
| Compute S | §18–19 | 0,62 kW | 0,47 kW |
| Compute M | §20–21 | 1,26 kW | 0,95 kW |
| PG OLTP S | §28 | 0,79 kW | 0,60 kW |
| PG OLTP M | §29 | 1,43 kW | 1,05 kW |
| PG Warehouse | §30 | 1,54 kW | 1,10 kW |
| Ceph OSD NVMe S | §42 | 0,78 kW | 0,60 kW |
| Ceph OSD NVMe M | §43 | 1,49 kW | 1,10 kW |
| Ceph hybride S | §54 | 0,74 kW | 0,55 kW |
| Ceph hybride M | §55 | 1,53 kW | 1,10 kW |
| VDI S (100 users) | §63 | 1,35 kW | 1,00 kW |
| VDI M vGPU (100 users) | §64 | 2,68 kW | 2,00 kW |
| SIEM S | §73 | 0,95 kW | 0,70 kW |
| SIEM M | §74 | 1,60 kW | 1,20 kW |
| 2× MI300X | §82 | 2,30 kW | 1,80 kW |
| 4× MI300X | §83 | 4,40 kW | 3,50 kW |
| 2× L40S | §92 | 1,38 kW | 1,05 kW |
| 4× L40S | §93 | 2,69 kW | 2,00 kW |
| 2× RTX PRO 6000 | §94 | 1,88 kW | 1,40 kW |
| 4× RTX PRO 6000 | §95 | 3,74 kW | 2,80 kW |
| Backup S / M | §102–103 | 0,70 / 0,85 kW | 0,50 / 0,65 kW |
| Firewall 1G / 10G / 25G | §110–112 | 0,02 / 0,12 / 0,25 kW | idem |
| HSM (paire) | §116 | 0,60 kW | 0,40 kW |

**C'est LE tableau d'entrée du dimensionnement électrique (partie D).**

---

## 126. Synthèse budgets par workload (ordre de grandeur)

| Workload | Build d'entrée | Budget CAPEX | kW/rack typique |
|---|---|---|---|
| Compute | S : 30 k€ | 90–190 k€ (3–6 nœuds) | 3–6 kW |
| DB HA | 2× S : 93 k€ | 100–210 k€ | 2–4 kW |
| Ceph NVMe 150 To | 5× S : 312 k€ | 300–500 k€ | 6–10 kW |
| Ceph hybride 2 Po | 8× M : 615 k€ | 600–800 k€ | 12–18 kW |
| VDI 300 postes | 328 k€ + 60 k€/an | 350–500 k€ | 4–6 kW |
| SOC 1 To/j | 320 k€ | 300–450 k€ | 5–8 kW |
| Inférence 70B | 35–97 k€ | 40–150 k€ | 2–5 kW/serveur |
| Backup 100 To (3-2-1) | 74 k€ + 36 k€/an | 75–120 k€ | 1–2 kW |
| Firewall 10G HA | 6,7 k€ | 7–15 k€ | 0,3 kW |
| HSM (paire) | devis (15–50 k€/u) | 30–100 k€ | 0,6 kW |

---

# PARTIE C — MONTAGE DATACENTER COMPLET

---

## 127. Rack : le choix du 42U

| Critère | Recommandation PME/datacenter | Pourquoi |
|---|---|---|
| Hauteur | 42U (rarement 47U) | standard, pièces détachées |
| Largeur | 600 mm (800 mm si câblage dense) | 800 mm = gestion verticale des câbles |
| Profondeur | **1 100–1 200 mm** | les serveurs GPU font 800+ mm + câbles |
| Charge statique | ≥ 1 500 kg | un rack plein pèse 800–1 200 kg |
| Charge dynamique | ≥ 1 000 kg | déplacement chargé (à éviter quand même) |
| Portes | perforées ≥ 80 % (avant+arrière) | le plein panneau tue le flux d'air |
| Prix | ≈ 2 700 $ (APC NetShelter AR3100, vérifié le 27/09/2026) | 1 500–3 000 € selon options |

**Ne prenez jamais un rack de 1 000 mm de profondeur** pour des serveurs :
avec les câbles et les PDU 0U, il vous manquera 15 cm. C'est l'erreur n° 1.

---

## 128. Charge au sol : le calcul qu'on oublie

- Rack plein : ≈ 1 000 kg sur 0,72 m² (600×1200) = **1 390 kg/m²**.
- Dalle technique standard : 500–1 000 kg/m² → **insuffisant** !
- Solutions : répartiteurs de charge, racks sur dalle béton (rez-de-chaussée),
  ou étude structurelle. **Faites vérifier par un BE** au-delà de 800 kg/rack.
- Sismique : ancrage au sol dans les zones concernées.

---

## 129. PDU : metered vs switched, bien choisir

| Type | Mesure | Pilotage prises | Prix indicatif 22 kW 3-ph | Usage |
|---|---|---|---|---|
| Basic | non | non | ≈ 300–500 € | à éviter en prod |
| Metered | globale (+ par phase) | non | ≈ 2 050 £ (~2 400 €, vérifié le 27/09/2026) | standard mini |
| Metered per-outlet | par prise | non | ≈ 3 000–4 000 € (à vérifier) | facturation, debug |
| Switched | globale | oui (on/off distant) | ≈ 3 782 $ (~3 480 €, vérifié le 27/09/2026) | redémarrage distant |
| Switched per-outlet | par prise | oui | ≈ 5 200 $ (~4 800 €, vérifié le 27/09/2026) | le top |

**Recommandation : 2× PDU metered par rack** (une par arrivée A/B), switched
si vous n'avez pas d'IPMI fiable partout. La mesure par prise paie son
surcoût dès le premier debug de surcharge.

---

## 130. Tableau PDU : quelle puissance par rack

| PDU | Tension | Courant | Puissance | Prix indicatif |
|---|---|---|---|---|
| Mono 16 A | 230 V | 16 A | 3,7 kW | ≈ 400–600 € |
| Mono 32 A | 230 V | 32 A | 7,4 kW | ≈ 600–900 € |
| Tri 16 A | 400 V | 3× 16 A | 11 kW | ≈ 1 500–2 000 € |
| Tri 32 A | 400 V | 3× 32 A | 22 kW | ≈ 2 400 € (metered, §129) |
| Tri 63 A | 400 V | 3× 63 A | 43,5 kW | ≈ 4 000–5 000 € (à vérifier) |

Règle : **PDU chargée à 80 % max** en nominal. Un rack à 5,6 kW (§120) →
PDU tri 16 A (11 kW) confortable, ou tri 32 A pour la croissance.

---

## 131. Répartition des charges par phase — exemple chiffré

Rack §120 (5,6 kW) sur PDU tri 16 A (3× 16 A = 11 kW) :
- Phase L1 : 2× Compute M (1,9 kW) + 1 switch (0,3) = **2,2 kW (9,6 A)**
- Phase L2 : 2× PG S (1,2 kW) + 2× Ceph S (1,2 kW) = **2,4 kW (10,4 A)**
- Phase L3 : backup (0,5) + FW (0,2) + switch (0,3) = **1,0 kW (4,3 A)**
- Déséquilibre : 10,4 vs 4,3 A → **rééquilibrez** : déplacez un Ceph S en L3.
- Cible : **écart < 20 %** entre phases (neutre non surchargé, disjoncteur
  différentiel content).

**Les 2 arrivées A/B** : chaque serveur double-alimenté prend 1 cordon sur
chaque PDU. En cas de perte d'une arrivée, l'autre porte 100 % : donc chaque
PDU doit pouvoir porter **le rack entier** (pas la moitié !).

---

## 132. Schéma ASCII — rack 42U type (§120)

```
+--------------------------------------------------+
| 42                                               |  <- panneau de brassage
| 41  [ Patch panel fibre 24 ports ]               |
| 40                                               |
| 39  [ Switch ToR-1 25/100G ]                     |  0,3 kW (A/B)
| 38  [ Switch ToR-2 25/100G ]                     |  0,3 kW
| 37                                               |
| 36  [ FW-1 ]  [ FW-2 ]  (1U côte à côte)          |  0,2 kW
| 35                                               |
| 34  [ Backup S 2U ]                              |  0,5 kW
| 33                                               |
| 32  [ Ceph OSD S 2U ]                            |  0,6 kW
| 31                                               |
| 30  [ Ceph OSD S 2U ]                            |  0,6 kW
| 29                                               |
| 28  [ PG OLTP S 2U ]                             |  0,6 kW
| 27                                               |
| 26  [ PG OLTP S 2U ]                             |  0,6 kW
| 25                                               |
| 24  [ Compute M 2U ]                             |  0,95 kW
| 23                                               |
| 22  [ Compute M 2U ]                             |  0,95 kW
| 21                                               |
| 20  [ KVM / console ]                            |
| 19                                               |
|  1-18  LIBRE (croissance : 18U = 40 % du rack)   |
+--------------------------------------------------+
  | PDU-A (gauche, 0U) | | PDU-B (droite, 0U) |
```

Règles de placement : switchs en haut (câbles courts vers le brassage),
serveurs lourds en bas (stabilité), 1U libre entre groupes pour l'air.

---

## 133. Rails, gestion verticale, accessoires

| Accessoire | Prix indicatif | Pourquoi |
|---|---|---|
| Rails coulissants (par serveur) | 100–150 € | inclus souvent — vérifiez |
| Gestionnaire de câbles vertical 0U | 80–150 € | sans ça, c'est le chaos en 6 mois |
| Panneau obturateur 1U (×10) | 50 € | **obligatoire** : l'air passe par les trous sinon |
| Brosse passe-câbles | 30 € | étanchéité + passage |
| Tablette 1U | 40 € | clavier/écran crash-cart |

**Les panneaux obturateurs sont le meilleur investissement thermique du rack**
(50 € pour −3 °C en entrée serveur). Les racks « gruyère » surchauffent par le
haut.

---

## 134. KVM, IPMI, console : l'accès hors-bande

- Chaque serveur : **port BMC/IPMI dédié** sur un VLAN/switch de management
  séparé (pas sur le LAN de prod).
- Licences : iDRAC Enterprise / iLO Advanced / XCC Enterprise se paient
  (200–400 €/serveur — à vérifier). **Négociez-les dans le devis**, pas après.
- 1× console série (OpenGear, Digi) par rangée de racks : quand le réseau
  est down, le série reste. ≈ 1 500–3 000 € (à vérifier).
- Documentez **tous** les mots de passe BMC dans le coffre (HSM §117 ou
  équivalent) — pas sur un post-it.

---

## 135. Sécurité physique du rack

- Serrures à badge ou code (pas la clé universelle constructeur).
- Caméra sur l'allée, détection d'ouverture de porte (contact sec → SNMP).
- **Personne ne travaille seul** dans la salle (règle des 2 personnes pour
  l'électrique).
- Registre d'accès : qui, quand, pourquoi. En cas d'audit, c'est demandé.

---

## 136. Pièges terrain — Rack / PDU

1. **Rack 1 000 mm** pour des serveurs de 800 mm + PDU 0U : ça ne ferme pas.
2. **Une seule PDU** : la maintenance électrique = arrêt total. Toujours A+B.
3. **Phases déséquilibrées** : le neutre chauffe, le différentiel saute.
4. **Pas de panneaux obturateurs** : recyclage d'air chaud, +5 °C en haut.
5. **Câbles d'alim qui pendent devant les ventilos** : −30 % de flux d'air.
6. **Charge au sol non vérifiée** : 1 390 kg/m² sur une dalle à 500 kg/m².

---

## 137. Check-list réception rack

- [ ] Profondeur ≥ 1 100 mm vérifiée au mètre
- [ ] 2× PDU (A+B), type metered mini, ampérage ≥ P_max × 1,25 / 0,8
- [ ] Panneaux obturateurs sur tous les U libres
- [ ] Rails : 1 jeu par serveur, montés
- [ ] Ancrage au sol si > 800 kg ou zone sismique
- [ ] Étiquette du rack (nom, PDU A/B, phases)

---

## 138. Budget rack complet (vide → prêt)

| Poste | Prix |
|---|---|
| Rack 42U 600×1200 | ≈ 2 500 € |
| 2× PDU metered tri 32 A | 2 × 2 400 € = 4 800 € |
| Accessoires (obturateurs, gestion câbles) | 500 € |
| Brassage + jarretières (cuivre+fibre) | 1 500 € |
| **TOTAL / rack** | **≈ 9 300 €** |

À multiplier par le nombre de racks — et à ne pas oublier dans le budget
global (c'est 10–15 % du CAPEX qui disparaît si on l'oublie).

---

## 139. Câblage : cuivre vs fibre — tableau de choix

| Lien | Cuivre (DAC/RJ45) | Fibre (optique) | Conseil 2026 |
|---|---|---|---|
| 1G management | RJ45 Cat6 | — | cuivre |
| 10G < 5 m | DAC SFP+ (~30 €) | — | DAC |
| 25G < 5 m | DAC SFP28 (~40 €) | — | DAC |
| 25G > 5 m | — | SR (~80 €) | fibre |
| 100G < 3 m | DAC QSFP28 (~80 €) | — | DAC |
| 100G > 3 m | — | SR4 (~250 €) | fibre |
| 400G | — | SR4 (~800 €, à vérifier) | fibre |

Règle : **DAC en intra-rack** (pas cher, pas de panne d'optique), **fibre dès
que ça sort du rack**. Un DAC qui traverse 3 racks est un incident en attente.

---

## 140. AOC vs DAC vs optique+jarretière

| Solution | Prix 100G 5 m | Avantages | Inconvénients |
|---|---|---|---|
| DAC | ≈ 80 € | pas cher, fiable | rigide, ≤ 5 m |
| AOC (câble optique actif) | ≈ 200 € (à vérifier) | souple, 30 m | électronique aux 2 bouts |
| Optique + jarretière | ≈ 250 € + 30 € | standard, réparable | 2 points de panne |

En 2026, l'AOC gagne du terrain pour le ToR→serveur quand le DAC est trop
court : un seul composant, pas de nettoyage de connecteurs.

---

## 141. Chemins de câbles : l'infra invisible

- **Chemin de câbles** au-dessus des racks (300–600 mm de large) : cuivre d'un
  côté, fibre de l'autre (séparation EMI + rayon de courbure).
- **Fibre** : rayon de courbure mini 30 mm (OM4) — un angle à 90° sec = fibre
  morte dans 2 ans.
- **Descente** dans le rack par le haut (pas par le bas — l'eau et la
  poussière montent moins bien).
- Prévoyez **30 % de place libre** dans les chemins : le recâblage d'un chemin
  plein est un cauchemar.

---

## 142. Étiquetage : la norme maison (à écrire et afficher)

Format proposé : `RACK-SWITCH-PORT → RACK-SERVEUR-PORT`

```
R01-TOR1-ETH25 → R01-SRV03-ETH0
R01-TOR1-ETH26 → R01-SRV03-ETH1
```

- Étiquettes **des 2 côtés** de chaque câble, lisibles sans débrancher.
- Code couleur : **bleu** = data, **jaune** = management/IPMI,
  **rouge** = arrivée A, **noir** = arrivée B (ou votre code — mais UN code).
- Imprimante d'étiquettes pro (Brady/Dymo Rhino) : 200–400 € — rentabilisée
  au premier dépannage nocturne.

---

## 143. Brassage : l'architecture ToR

```
    [ Core / Agg switches ]
           | 100G (MLAG)
    +------+------+
    | ToR-1      ToR-2 |   ← 1 paire par rack (ou par 2 racks)
    +------+------+
     | 25G DAC | 25G DAC
  [srv-01] [srv-02] ...
```

- **ToR (Top of Rack)** : 1–2 switchs par rack, uplinks en MLAG vers
  l'agrégation. Pas de switch « fin de rangée » en 2026 sauf très gros site.
- Chaque serveur : **2 liens** (1 vers chaque ToR) en LACP/bond actif-passif.
- Management : 1 switch 1G 48 ports par 2–4 racks, réseau isolé.

---

## 144. Schéma ASCII — câblage d'un serveur type

```
Façade arrière serveur (2U) :

 [PSU-A]====rouge====>[ PDU-A ]   (alim redondante)
 [PSU-B]====noir=====>[ PDU-B ]

 [BMC ]-----jaune---->[ SW-MGMT ]  (IPMI, VLAN 99)
 [ETH0]-----bleu----->[ TOR1-ETH25 ]  (DAC 25G)
 [ETH1]-----bleu----->[ TOR2-ETH25 ]  (DAC 25G)
 [ETH2]~~~~~fibre~~~~>[ TOR1-ETH1 ]  (100G, si double NIC)
 [ETH3]~~~~~fibre~~~~>[ TOR2-ETH1 ]
```

`====` = cordon d'alim C13/C19, `-----` = cuivre/DAC, `~~~~~` = fibre.

---

## 145. Longueurs et stocks : le kit du câbleur

| Article | Stock conseillé (par rack) |
|---|---|
| DAC 25G 1 m / 3 m | 10 / 10 |
| DAC 100G 3 m | 4 |
| Jarretières OM4 LC-LC 3 m / 10 m | 10 / 4 |
| Cordons d'alim C13 2 m / C19 2 m | 10 / 4 |
| Étiquettes + ruban | 1 rouleau |
| Attaches velcro (pas de colliers !) | 1 paquet |

**Velcro, jamais de colliers de serrage** : un collier trop serré écrase le
cuivre et casse la fibre. Le jour où il faut recâbler, le velcro se rouvre.

---

## 146. Pièges terrain — Câblage

1. **DAC de 7 m** : n'existe pas en fiable. Fibre au-delà de 5 m.
2. **Fibre pliée à 90°** dans le gestionnaire : micro-cassures, erreurs CRC
   dans 6 mois.
3. **Une seule étiquette** (ou pire, au marqueur) : illisible dans 2 ans.
4. **Cuivre et fibre dans le même chemin sans séparation** : le jour où on
   tire un câble, on arrache les connecteurs LC.
5. **Pas de plan de câblage** : le « on s'en souvient » ne survit pas au
   premier départ en retraite.
6. **Optiques non codées** pour le switch : un transceiver « générique » non
   reconnu = lien down. Vérifiez la compatibilité (FS.com code sur demande).

---

## 147. Plan de câblage : le document

Un tableur (ou NetBox — voir guide netbox) avec : câble ID, de, vers, type,
longueur, couleur, date. **Chaque câble posé = 1 ligne.** 30 minutes de saisie
par rack évitent 4 h de debug par incident.

---

## 148. Budget câblage par rack

| Poste | Prix |
|---|---|
| 20× DAC 25G | 800 € |
| 4× DAC/optiques 100G | 800 € |
| Jarretières + cordons | 400 € |
| Étiquettes, velcro, divers | 200 € |
| **TOTAL** | **≈ 2 200 €** |

Comptez-le : sur 10 racks, c'est 22 000 € « invisibles ».

---

## 149. Refroidissement : les seuils qui décident de tout

| Densité/rack | Solution | Statut 2026 |
|---|---|---|
| < 15 kW | Air seul (allées chaude/froide) | standard |
| 15–30 kW | Air optimisé + confinement | limite haute de l'air |
| 30–50 kW | **Rear-door heat exchanger (RDHx)** | dominant (37 % adoption/envisagé) |
| 50–100 kW | RDHx + DLC partiel (hybride) | mainstream IA |
| > 100 kW | **DLC direct-to-chip obligatoire** | racks NVL72 > 120 kW |
| > 240 kW | Immersion (émergent) | hyperscalers |

Repères vérifiés le 27/09/2026 (AFCOM 2026, analyses industrie) : 36 % des
opérateurs ont déployé du liquide, 28 % prévoient sous 12–24 mois ; le DTC
monophasé = 55 % du marché direct-to-chip. **Au-delà de 20 kW/rack, le liquide
doit être étudié ; au-delà de 50 kW, il n'y a plus de débat.**

---

## 150. Calcul thermique simplifié : kW → débit d'air

```
Débit_air (m³/h) = P(kW) × 860 / ΔT(°C) / 1,2
```

Avec ΔT = 12 °C (entrée 24 °C → sortie 36 °C, standard) :
- **1 kW ≈ 300 m³/h** d'air.
- Rack 5,6 kW (§120) → **≈ 1 700 m³/h**.
- Rack 25 kW (Ceph §47) → **≈ 7 500 m³/h** — c'est énorme : 2 ventilos
  industriels. D'où le liquide.

Vérifiez que la **CTA/clim** de la salle fournit ce débit PAR RACK, pas au
total de la salle. L'erreur classique : une clim de 50 kW pour 4 racks de
15 kW = 60 kW → ça ne passe pas.

---

## 151. Rear-door heat exchanger (RDHx) : la porte qui refroidit

- Principe : porte arrière du rack avec batterie à eau — l'air chaud des
  serveurs est refroidi **avant** de sortir dans la salle. La salle reste à
  température normale.
- **Actif** (avec ventilos) ou passif ; capacité 20–50 kW/porte.
- Avantages : pas de modification des serveurs, retrofit possible, pas de
  risque d'eau DANS le serveur.
- Inconvénients : il faut une **boucle d'eau** (CDU ou eau glacée du bâtiment),
  +1 porte = +15 cm de profondeur.
- Prix : 8 000–20 000 €/porte (à vérifier). **Le meilleur rapport €/kW pour
  passer de 15 à 40 kW/rack.**

---

## 152. DLC direct-to-chip : le standard > 100 kW

- Principe : **cold plates** en cuivre sur CPU/GPU, eau glycolée (75 % eau /
  25 % glycol) en circuit fermé, raccords **quick-disconnect** anti-goutte.
- Le rack NVL72 (> 120 kW) ne fonctionne QUE comme ça.
- Il faut : CDU (Coolant Distribution Unit) par rangée, manifold par rack,
  boucle bâtiment (dry cooler ou groupe froid).
- Les composants non refroidis (RAM, disques, alims) restent à l'air → les
  racks DLC gardent souvent un **RDHx en complément** (hybride).
- Fournisseurs : Supermicro (DLC intégré, 3 000+ racks/mois en 2026), Dell
  (XE9780L + rRDHx), Vertiv/Schneider (pods modulaires).

---

## 153. CDU et boucles : l'hydraulique du datacenter

```
[ Dry cooler / groupe froid ] ← boucle bâtiment (eau glacée)
            |
      [ CDU redondante ]  ← échangeur plaques, sépare les boucles
            |
   [ Manifold rack 1 ] [ Manifold rack 2 ] ...  ← boucle techno (eau pure)
            |
   [ Cold plates CPU/GPU ] (quick-disconnect)
```

- **2 boucles séparées** : la boucle bâtiment (sale, traitée) n'entre jamais
  dans les serveurs ; la boucle techno (eau déminéralisée) reste propre.
- Détection de fuite : câble détecteur au sol + vannes automatiques.
  **Testez la détection** (un seau d'eau suffit pour le test).
- Qualité d'eau : conductivité < 5 µS/cm, traitement anti-légionelles sur la
  boucle bâtiment.

---

## 154. Immersion : l'état en 2026

- Principe : serveurs plongés dans un **diélectrique** (mono ou diphasique).
- Le diphasique (ébullition) est freiné par la réglementation **PFAS**
  (reporting EPA dès janvier 2027, bans locaux) → le **monophasé** domine les
  nouveaux projets.
- Usage 2026 : hyperscalers, mining reconverti, quelques HPC. **Pas encore
  pour la PME** : maintenance (sortir un serveur du bain), compatibilité
  composants, coût du fluide.
- À surveiller : si vos racks dépassent 200 kW un jour, l'immersion reviendra
  sur la table (section 179).

---

## 155. Cas chiffré : salle de 100 kW IT

| Poste | Calcul | Résultat |
|---|---|---|
| Charge IT | 4 racks × 25 kW | 100 kW |
| Climatisation (PUE 1,4) | 100 × 0,4 | 40 kW froid |
| Débit d'air équivalent | 100 × 300 m³/h | 30 000 m³/h |
| Solution | 4× RDHx 30 kW + boucle eau | — |
| Budget froid | 4 portes × 15 k€ + CDU 30 k€ + boucle 50 k€ | **≈ 140 k€** |
| Électricité froid/an | 40 kW × 8 760 × 0,20 € | **70 k€/an** |

Le froid coûte **autant que les serveurs** sur 5 ans. C'est pour ça que le
PUE est la métrique reine (voir guide onduleurs/énergie).

---

## 156. Free cooling : le climat comme allié

- En France, **8–10 mois/an** l'air extérieur suffit à refroidir (économiseur).
- Dry cooler + free cooling : PUE 1,15–1,25 atteignable (vs 1,6–2,0 en
  détente directe classique).
- Contrainte : humidité contrôlée (pas d'air extérieur direct sur les
  serveurs — échangeur obligatoire).
- **Dimensionnez le free cooling dès la conception** : l'ajouter après coûte 3×.

---

## 157. Surveillance thermique : les sondes qui sauvent

| Sonde | Où | Seuil d'alerte |
|---|---|---|
| Température entrée rack (bas/milieu/haut) | 3/rack | > 27 °C |
| Température sortie rack | 1/rack | > 40 °C |
| Humidité salle | 2/salle | < 20 % ou > 60 % |
| Débit boucle eau / pression | CDU | variation > 10 % |
| Détection fuite | sol + manifold | contact |

Remontez tout en SNMP vers Zabbix (voir guide zabbix) : une dérive lente de
+1 °C/mois = filtre encrassé ou porte mal fermée.

---

## 158. Pièges terrain — Refroidissement

1. **Clim dimensionnée « au total »** et pas par rack : le rack du fond
   surchauffe pendant que celui de devant gèle.
2. **Allées chaude/froide non respectées** : un serveur monté à l'envers
   recycle son air chaud → emballement thermique local.
3. **RDHx sans boucle d'eau prévue** : la porte arrive, l'eau n'existe pas.
   L'hydraulique se décide AVANT les serveurs.
4. **Eau dans la salle sans détection de fuite** : un raccord qui goutte sur
   un PDU = court-circuit. Câble détecteur obligatoire.
5. **Filtres jamais changés** : −20 % de débit en 6 mois en environnement
   poussiéreux.
6. **Oublier la redondance du froid** : N+1 sur les groupes froids comme sur
   les onduleurs. Une clim en panne en août = arrêt.

---

## 159. Tableau de décision rapide

| Votre rack fait… | Faites… |
|---|---|
| < 10 kW | Air, allées chaude/froide, obturateurs |
| 10–20 kW | Air + confinement d'allée |
| 20–40 kW | **RDHx** |
| 40–100 kW | RDHx + DLC partiel (GPU) |
| > 100 kW | DLC complet + CDU |

---

## 160. Lien avec l'énergie : le PUE

```
PUE = énergie_totale_datacenter / énergie_IT
```

- PUE 2,0 : pour 1 kW de serveurs, 1 kW de froid/pertes → **l'énergie double**.
- PUE 1,3 (free cooling) : +30 % seulement.
- Sur 100 kW IT à 0,20 €/kWh : PUE 2,0 = 350 k€/an ; PUE 1,3 = 228 k€/an.
  **122 k€/an d'écart** — le refroidissement EST un sujet financier.
  (Détail dans le guide onduleurs de Zelef — lien §161.)

---

## 161. Onduleurs : lien avec le guide onduleurs de Zelef (pas de doublon)

Ce guide ne duplique PAS le guide `onduleurs_ups_guide.md` (topologies VFI/VI/VFD,
batteries VRLA/Li-ion, maintenance, contrats — tout y est). Ici : **la méthode
pour dimensionner la chaîne onduleur à partir des BOMs ci-dessus**.

Rappel utile : un onduleur se dimensionne en **kVA et en kW** (kW = kVA × fp,
fp ≈ 0,9–1,0 en 2026). C'est le kW qui compte pour des serveurs à PFC actif.

---

## 162. Méthode : des BOMs à l'onduleur en 5 étapes

1. **Σ P_réaliste** de tous les serveurs + réseau + froid critique
   (tableau §125). Exemple §120 : 5,6 kW IT + 0,6 kW réseau = **6,2 kW**.
2. **× 1,25** (marge + croissance) → 7,75 kW.
3. **+ froid secouru** si la clim doit tenir sur onduleur (souvent non en PME —
   les serveurs tiennent 10 min sans froid, le temps d'un arrêt propre).
4. Choisissez l'onduleur : **P_onduleur_kW ≥ résultat**, en N+1 si critique
   (2 onduleurs en parallèle redondant).
5. **Autonomie** : batteries pour 10–15 min (arrêt propre) ou 1–2 h (tenir
   une coupure) — voir §164.

---

## 163. Cas chiffré : 40 kW IT (salle de 6–8 racks)

| Étape | Calcul | Résultat |
|---|---|---|
| Σ P_réaliste | 40 kW | 40 kW |
| × 1,25 | — | 50 kW |
| Onduleur | 60 kVA / 60 kW (fp=1), N+1 → 2× 60 kW | 2× 60 kW |
| Prix indicatif | Easy UPS 3M 60 kW ≈ 25–35 k€ pièce (à vérifier) | **≈ 60 k€** |
| Batteries | 15 min à 50 kW | incluses ou +15 k€ |

Architecture : **2 onduleurs en parallèle redondant** (si l'un tombe, l'autre
prend tout), bypass de maintenance externe, 2 arrivées A/B derrière.

---

## 164. Autonomie : le calcul batteries

```
Énergie (kWh) = P(kW) × autonomie(h) / rendement_onduleur
```

- 50 kW × 0,25 h (15 min) / 0,95 = **13,2 kWh** → 1 armoire batteries.
- 50 kW × 2 h / 0,95 = **105 kWh** → 4–6 armoires (cher !).
- **Règle PME** : 15 min d'autonomie + groupe électrogène si > 30 min requis.
  Les batteries pour 2 h coûtent plus cher que le groupe.
- Li-ion vs VRLA : Li-ion = 2× le prix, 3× la durée de vie, 2× moins de place
  (détail : guide onduleurs).

---

## 165. ATS et redondance des arrivées

- Chaque rack : **2 PDU (A+B)** sur 2 onduleurs (ou 2 jeux de batteries)
  différents.
- Équipements mono-alimentation (vieux switch, console) : **ATS** (Automatic
  Transfer Switch) 16 A ≈ 1 300 € (vérifié le 27/09/2026, gamme APC).
- **Testez le basculement** : coupez A un dimanche, vérifiez que tout tient
  sur B. Puis l'inverse. Tous les ans.

---

## 166. Pièges terrain — Énergie

1. **Dimensionner en kVA au lieu de kW** : un 60 kVA à fp 0,8 = 48 kW. Avec
   des serveurs à 50 kW, ça disjoncte. **Toujours en kW.**
2. **Oublier le courant d'appel** : les PSU appellent 2× au démarrage. Un
   redémarrage simultané de 20 serveurs fait déclencher l'onduleur. Démarrage
   séquencé (PDU switched, §129).
3. **Batteries jamais testées** : test mensuel auto + test de décharge annuel.
   Une batterie morte = 0 minute d'autonomie.
4. **Pas de délestage** : en fin d'autonomie, arrêtez proprement (NUT —
   voir guide Proxmox, scénario d'arrêt auto). Un arrêt brutal corrompt.

---

## 167. Mise en service pas à pas

| Étape | Action | Durée |
|---|---|---|
| 1. Réception | Check-list §137 + §168, photos des cartons | 1 j |
| 2. Montage | Rails, serveurs (lourds en bas), PDU, câblage §144 | 1–2 j/rack |
| 3. Électrique | Phases §131, test A/B, serrage | 0,5 j |
| 4. Burn-in | §169, 48–72 h | 3 j |
| 5. Firmware | BMC, BIOS, NIC, SSD — versions notées | 1 j |
| 6. Réseau | iperf3, VLAN, LACP, management | 1 j |
| 7. OS/hyperviseur | Install, durcissement (voir guides OS) | 1–2 j |
| 8. Applicatif | Ceph, DB, VDI… par workload | variable |
| 9. As-built | §172 | 1 j |
| 10. Prod | progressive, §173 | 1–2 sem |

---

## 168. Check-list de réception matériel

- [ ] Colis : nombre, état, n° de série vs bon de livraison
- [ ] Chaque serveur : modèle, CPU, RAM (quantité + part-number), disques
- [ ] Firmware notés (BMC/BIOS/NIC/SSD) — photo des étiquettes
- [ ] Câbles/optiques : quantités, types
- [ ] PDU : ampérage, type (metered/switched)
- [ ] Garantie : enregistrée, date de fin notée
- [ ] Photos : rack vide, rack plein, câblage arrière

---

## 169. Burn-in : le protocole 72 h

```
J1 : memtest86+ (1 passe) + smartctl (tous disques)
J1-2 : stress-ng --cpu 0 --vm 4 (24 h) + surveillance temp/conso
J2 : fio randwrite 4k sur TOUS les SSD (2 h) — révèle les SSD faibles
J2-3 : iperf3 24 h sur chaque NIC (débit + 0 perte)
J3 : reboot × 5 (POST, BMC, boot) + re-vérification firmware
```

- **Tout défaut pendant le burn-in = retour fournisseur**, pas « on verra ».
- Rapport de burn-in archivé par n° de série (utile pour la garantie).
- Les 3XS et intégrateurs sérieux le font d'usine (24 h) — refaites 48 h
  quand même : le transport secoue.

---

## 170. Firmware : l'ordre des MAJ

1. **BMC/IPMI** d'abord (un BMC planté = plus d'accès distant).
2. BIOS/UEFI (notez les réglages : NUMA, virtualization, power profile).
3. NIC (firmware + option ROM).
4. SSD (firmware — voir §51 sur l'uniformité).
5. Backplane/expandeurs.

**Ne mettez jamais à jour tous les nœuds d'un cluster en même temps.**
Un par un, avec validation entre chaque (rolling upgrade).

---

## 171. Tests réseau à la mise en service

- [ ] iperf3 : débit nominal sur chaque lien (25G → ≥ 23 Gb/s utiles)
- [ ] 0 % de perte sur 24 h (sinon : câble, optique, ou port)
- [ ] LACP : débranchez un lien, le trafic continue
- [ ] VLAN : isolez, vérifiez qu'il n'y a pas de fuite
- [ ] Latence : ping < 0,2 ms en intra-rack (sinon : buffer, duplex)

---

## 172. Documentation as-built : le livrable

- Plan de rack (qui est où, §132) + plan de câblage (§147).
- Tableau électrique : phases, PDU, disjoncteurs, onduleur.
- Versions firmware par n° de série.
- Mots de passe (coffre, §134).
- Procédures : arrêt/démarrage, failover FW, restore backup.
- **Sans as-built, la salle n'est pas réceptionnée.** C'est contractuel.

---

## 173. Mise en production progressive

1. Semaine 1 : 10 % de la charge, surveillance renforcée.
2. Semaine 2 : 50 %, premier test de failover réel (coupez un nœud).
3. Semaine 3 : 100 %, exercice de restauration backup.
4. Mois 2 : revue thermique (caméra IR) + resserrage électrique.
5. **Ne déclarez jamais « c'est fini » avant le premier incident géré.**

---

## 174. Pièges terrain — Mise en service

1. **Sauter le burn-in** « pour gagner 3 jours » : le disque qui meurt à J+15
   en prod coûte 10× plus cher.
2. **Firmware hétérogènes** : 3 versions de BIOS sur 10 nœuds = comportements
   différents. Uniformisez.
3. **Mots de passe par défaut** sur les BMC : le premier scan Shodan les
   trouve. Changez AVANT de brancher le réseau.
4. **As-built « on le fera plus tard »** : plus tard = jamais.
5. **Mise en prod un vendredi** : non. Mardi ou mercredi matin, équipe complète.

---

## 175. PME vs hyperscaler : ce qui change à l'échelle

| Sujet | PME (1–10 racks) | Hyperscaler (1 000+ racks) |
|---|---|---|
| Serveurs | Intégrateurs (Supermicro, Dell) | Design propre (OCP) |
| Refroidissement | Air + RDHx | DLC généralisé, immersion |
| Puissance/rack | 5–30 kW | 100–240 kW (Rubin : 100–240 kW/rack) |
| Distribution élec | 400 V AC → PDU | **800VDC** (produits H2 2026) |
| Réseau | ToR 25/100G | 400/800G, topologies Clos |
| Achat | Devis, remise volume | Contrats pluriannuels directs |
| Staff | 1–5 personnes | Équipes dédiées par domaine |
| PUE cible | 1,4–1,6 | 1,1–1,2 |

---

## 176. 800VDC : l'état au 27/09/2026

- Constat : à 54 VDC (standard actuel in-rack), 1 MW = **18 500 A** → des
  barres de cuivre qui occuperaient 64U. À 800 VDC : **1 250 A**, −45 % de
  cuivre, rendement 83 % → 92 %+ (vérifié le 27/09/2026).
- Produits commerciaux (Vertiv, Schneider Electric, Eaton, Delta) :
  **2ᵉ semestre 2026**, alignés sur le rack NVIDIA Kyber.
- Pour la PME : **pas avant 2027–2028** en pratique. Mais prévoyez des
  chemins de câbles et des arrivées électriques **surdimensionnés** : le
  retrofit 800VDC passera par là.

---

## 177. OCP et designs ouverts : ce que la PME peut copier

- **Open Compute Project** : designs de racks, serveurs et PDU en open source.
- À copier : les principes (barres de puissance, airflow, gestion câbles),
  pas les pièces (pas de supply chain OCP en petite série).
- Les intégrateurs « OCP-inspired » (Supermicro notamment) apportent 80 %
  des gains pour 20 % de l'effort.

---

## 178. Ce que la PME doit copier / ne pas copier

**À copier :**
- Le monitoring exhaustif (tout est mesuré chez les hyperscalers).
- L'automatisation (provisioning, firmware — Ansible, voir guide ansible).
- Le burn-in systématique et les procédures écrites.
- Le PUE comme KPI financier.

**À NE PAS copier :**
- La densité extrême (100 kW/rack) : sans l'équipe et l'infra qui vont avec,
  c'est un incendie en attente.
- Le hardware custom : pas de pièces de rechange, pas de support.
- L'optimisme des délais GPU : eux ont des allocations, vous non.

---

## 179. Section « À venir » — annonces vérifiées au 27/09/2026

> Uniquement des annonces officielles ou des faits sourcés. Le reste est
> marqué « non trouvé » ou « à vérifier ».

1. **800VDC commercial** : Vertiv, Schneider Electric, Eaton, Delta —
   produits attendus H2 2026 (source : analyses industrie, §176).
2. **Thales Luna 8** : HSM FIPS 140-3 niveau 3, support post-quantique,
   disponible EMEA en 2026 (annonce Thales — vérifié le 27/09/2026).
3. **Densités Rubin** : racks 100–240 kW en préparation (trajectoire
   documentée par les équipementiers — à vérifier au cas par cas).
4. **Immersion monophasée** : progression chez les hyperscalers, le diphasique
   freiné par la réglementation PFAS (reporting EPA janvier 2027).
5. **DLC monophasé 55 % du marché DTC** : le standard de facto pour les
   nouveaux builds IA (vérifié le 27/09/2026).
6. **Délais GPU** : normalisation H100 (2–3 mois), tension persistante sur
   Blackwell PRO (3–7 mois début 2026 — à revérifier au devis).
7. **Prix DRAM** : TrendForce projette +13 à 18 % au T3 2026 sur la DRAM
   serveur — **achetez la RAM tôt** (vérifié le 27/09/2026).

Non trouvé au 27/09/2026 (prix publics) : MI325X/MI355X standalone, serveurs
8-GPU HGX en prix catalogue, PDU 800VDC en vente libre.

---

## 180. Veille : la méthode

- **Trimestrielle** : prix DRAM (TrendForce), GPU (snapshots marché),
  annonces refroidissement (AFCOM, Data Center World).
- **À chaque achat** : 3 devis, délais écrits, firmware notés.
- **Ce guide** : relisez la section 3 avant chaque budget annuel — le marché
  2026 bouge par trimestre, pas par an.

---

## Glossaire (35 termes)

| Terme | Définition |
|---|---|
| **BOM** | Bill of Materials : liste complète des composants d'un build, avec quantités et prix. |
| **TDP** | Thermal Design Power : puissance thermique max d'un composant, base du dimensionnement électrique et froid. |
| **PUE** | Power Usage Effectiveness : énergie totale / énergie IT. 1,0 = parfait, 1,5 = correct, 2,0 = à améliorer. |
| **DWPD** | Drive Writes Per Day : endurance d'un SSD (1 = on peut réécrire tout le disque 1×/jour pendant la garantie). |
| **RDIMM** | Mémoire serveur ECC à registres (buffered). Obligatoire en serveur. |
| **DPC** | DIMM Per Channel : nombre de barrettes par canal mémoire (1 DPC = pleine vitesse). |
| **NUMA** | Non-Uniform Memory Access : en bi-socket, la mémoire de l'autre CPU est plus lente. |
| **U.2** | Format SSD 2,5" NVMe hot-swap, standard datacenter. |
| **E3.S / EDSFF** | Format SSD nouvelle génération, plus dense que l'U.2. |
| **OCP 3.0** | Format de carte réseau mezzanine standard serveurs récents. |
| **RoCE** | RDMA over Converged Ethernet : accès mémoire distant sur Ethernet, pour Ceph/HPC. |
| **DAC** | Direct Attach Cable : câble cuivre avec transceivers intégrés, ≤ 5–7 m. |
| **AOC** | Active Optical Cable : câble optique actif, souple, jusqu'à 30 m+. |
| **ToR** | Top of Rack : switch en haut du rack, 1–2 par rack. |
| **MLAG** | Multi-Chassis Link Aggregation : 2 switchs vus comme un seul (redondance sans STP). |
| **PDU** | Power Distribution Unit : multiprise de rack intelligente (metered/switched). |
| **ATS** | Automatic Transfer Switch : bascule auto entre 2 arrivées électriques. |
| **N+1** | Redondance : N équipements pour la charge + 1 de secours. |
| **CARP** | Common Address Redundancy Protocol : VIP partagée entre 2 firewalls (HA). |
| **RDHx** | Rear Door Heat Exchanger : porte arrière à eau qui refroidit l'air sortant du rack. |
| **DLC** | Direct Liquid Cooling : refroidissement liquide direct des puces (cold plates). |
| **CDU** | Coolant Distribution Unit : échangeur qui sépare boucle bâtiment et boucle techno. |
| **PFAS** | « Polluants éternels » : freinent le refroidissement diphasique (réglementation 2027). |
| **EC 4+2** | Erasure Coding : 4 fragments data + 2 parité (économe en capacité, Ceph). |
| **RBD** | RADOS Block Device : volumes bloc Ceph. |
| **BlueStore** | Moteur de stockage Ceph (remplace FileStore), avec DB/WAL sur NVMe. |
| **OSD** | Object Storage Daemon : processus Ceph par disque. |
| **MON/MGR** | Moniteurs et managers Ceph (cluster, placement, métriques). |
| **vGPU** | GPU virtualisé NVIDIA (vPC/vWS) : partage d'un GPU entre VMs. |
| **EPS** | Événements Par Seconde : métrique de dimensionnement SIEM. |
| **HSM** | Hardware Security Module : boîtier inviolable pour les clés cryptographiques. |
| **PQC** | Post-Quantum Cryptography : algos résistants à l'ordinateur quantique (ML-KEM, ML-DSA). |
| **TCO** | Total Cost of Ownership : CAPEX + 5 ans d'OPEX (énergie, maintenance, licences). |
| **Burn-in** | Test de vieillissement accéléré (48–72 h) avant mise en prod. |
| **As-built** | Dossier « tel que construit » : plans, versions, mots de passe, procédures. |

---

## Quiz — 10 questions + réponses

**Q1.** Un module DDR5 RDIMM 64 Go coûtait ≈ 1 500 $ en contrat en septembre 2026.
Quel était son prix un an plus tôt, et quelle en est la conséquence sur les BOMs ?
**R1.** ≈ 272 $ (5,5× moins). Conséquence : la RAM est redevenue le premier poste
de coût d'un serveur — on la dimensionne au working set mesuré, pas « au cas où ».

**Q2.** Un serveur affiche : 2× CPU 400 W, 12 DIMM, 6 NVMe, 2 NIC 100G, base 150 W.
Calculez P_max avec la formule §13.
**R2.** 800 + 120 + 108 + 85 + 150 ≈ 1 260 W (proche du Compute M, §21).

**Q3.** Pourquoi le WAL PostgreSQL va-t-il sur des disques dédiés ?
**R3.** Les écritures séquentielles du WAL ne sont pas perturbées par les lectures
aléatoires des données : +20 à 40 % de TPS sur charges à écritures intenses (§28).

**Q4.** 5 nœuds Ceph de 123 To bruts en réplica 3 : combien de To utiles ?
**R4.** 615 × 0,85 / 3 ≈ 174 To (§46). Ne jamais dépasser 85 % de remplissage.

**Q5.** Un rebuild d'un HDD 20 To à 200 Mo/s prend combien de temps, et quel est
le risque pendant ce temps ?
**R5.** ≈ 28 h. Risque : un 2ᵉ disque qui lâche pendant le rebuild = perte de
données si le placement group n'a plus de réplica sain (§59).

**Q6.** 200 users VDI bureautiques : RAM totale et IOPS au boot storm ?
**R6.** 200 × 8 Go = 1,6 To RAM ; 200 × 100 IOPS = 20 000 IOPS pendant ~10 min (§66).

**Q7.** Un 70B en FP8 tient-il sur 1× RTX PRO 6000 96 Go ? Et sur 2× L40S ?
**R7.** 70B FP8 ≈ 70 Go + 30 % KV cache ≈ 91 Go ≤ 96 Go : oui, sur 1 carte.
Sur 2× L40S (96 Go au total) : oui aussi, en tensor-parallel (§85, §98).

**Q8.** Un rack de 25 kW : air seul ou liquide ? Justifiez avec le calcul d'air.
**R8.** Liquide (RDHx mini). 25 kW × 300 m³/h = 7 500 m³/h d'air — irréaliste en
air seul (§149–150).

**Q9.** Pourquoi dimensionne-t-on un onduleur en kW et pas en kVA ?
**R9.** Les serveurs à PFC actif consomment des watts réels ; un 60 kVA à fp 0,8
ne fournit que 48 kW — sous-dimensionné, il disjoncte (§166).

**Q10.** Citez 3 éléments obligatoires du dossier as-built.
**R10.** Plan de rack + plan de câblage, versions firmware par n° de série,
procédures d'arrêt/démarrage et de failover (§172).

---

## Top 15 des pièges terrain — récapitulatif

1. **RAM « au cas où » en 2026** : 384 Go inutiles = 8 280 € brûlés (§32).
2. **DB sur Ceph sans QoS** : la latence réseau tue les TPS — NVMe local (§38).
3. **RAID 5/6 SSD pour une DB** : pénalité ×4 + GC = latence imprévisible (§38).
4. **Ceph rempli à > 85 %** : plus de rééquilibrage, perfs en chute libre (§46).
5. **Back Ceph en 10G** : le premier rebuild sature tout (§51).
6. **HDD SMR en Ceph** : inutilisable — CMR uniquement (§59).
7. **VDI sans test boot storm** : tout s'écroule le lundi 8h55 (§69).
8. **Licences vGPU oubliées** : 250 k€ sur 5 ans pour 200 users (§65).
9. **Acheter le GPU avant de tester le modèle** : louez 10 h d'abord (§89).
10. **Châssis non qualifié 600 W/GPU** pour RTX PRO 6000 (§99).
11. **Rack 1 000 mm** : les serveurs + PDU ne ferment pas (§136).
12. **Une seule PDU / phases déséquilibrées** : pas de maintenance sans arrêt,
    neutre qui chauffe (§136).
13. **Clim « au total » pas par rack** : le rack du fond surchauffe (§158).
14. **Onduleur en kVA** : 60 kVA ≠ 60 kW, ça disjoncte (§166).
15. **Burn-in sauté** : le disque qui meurt à J+15 coûte 10× plus cher (§174).

---

## Conclusion — la méthode en une page

1. **Mesurez** (working set, EPS, débit, IOPS) avant d'acheter.
2. **Calculez** (les 5 formules §10–14) — la RAM et les watts d'abord.
3. **Choisissez** la BOM S/M/quad du workload correspondant.
4. **Additionnez** les P_réaliste (§125) → onduleur (§162) + froid (§150).
5. **Vérifiez** les prix et délais **le jour du devis** (marché 2026 volatil).
6. **Burn-in 72 h**, firmware uniformes, as-built complet.
7. **Suivez** les 5 métriques €/unité (§124) chaque trimestre.

Un datacenter n'est pas une collection de serveurs : c'est une **chaîne
électrique, thermique et réseau** dimensionnée d'un seul tenant. Les BOMs de
ce guide sont les maillons ; les parties C et D sont la chaîne.

*Prix et références vérifiés le 27/09/2026 sauf mentions « à vérifier ».*
*Guide rédigé pour Zelef — chef de service systèmes & énergies.*

---

# PARTIE D — APPROFONDISSEMENTS

---

## 181. Cas chiffré global : salle de 8 racks, 60 kW IT

Hypothèse : PME/ETI qui construit sa salle. Contenu : 40 serveurs mixtes
(compute, DB, Ceph, backup, VDI), 2 racks réseau, onduleurs, froid.

| Lot | Détail | Budget |
|---|---|---|
| Serveurs (40×, mix §120 ×5) | 354 k€ × 5 | 1 770 000 € |
| 8× racks 42U équipés (§138) | 9 300 € × 8 | 74 400 € |
| Réseau : 16× ToR + 2× agg 100G | — | 220 000 € |
| Câblage inter-racks + fibre | lot | 35 000 € |
| Froid : 8× RDHx + CDU + boucle | §155 adapté | 220 000 € |
| Onduleurs 2× 80 kW N+1 + batteries 15 min | — | 90 000 € |
| Groupe électrogène 200 kVA + inverseur | §183 | 55 000 € |
| Sécurité incendie (gaz, détection) | §190 | 40 000 € |
| Supervision (sondes, DCIM) | §188 | 25 000 € |
| Main-d'œuvre intégration (10 %) | — | 250 000 € |
| **CAPEX total** | | **≈ 2 780 000 €** |
| Électricité/an (60 kW × 8 760 × 0,20 × PUE 1,4) | — | **≈ 147 000 €/an** |
| Maintenance/an (8 % CAPEX) | — | ≈ 222 000 €/an |

**Le bâtiment et le génie civil ne sont pas inclus** (dalle, locaux, TGBT :
compter 200–500 k€ selon l'existant — à chiffrer avec un BET).

---

## 182. Plan de salle ASCII — 8 racks

```
+----------------------------------------------------------+
|  ENTREE (porte CF 1h)                                    |
|                                                          |
|  [Rack R1] [Rack R2] [Rack R3] [Rack R4]   ALLÉE FROIDE  |
|                                                          |
|  [Rack R5] [Rack R6] [Rack R7] [Rack R8]   ALLÉE FROIDE  |
|                                                          |
|  +--------+  +--------+                                  |
|  | UPS-1  |  | UPS-2  |   local batteries (ventilé)      |
|  +--------+  +--------+                                  |
|  [TGBT] [Groupe: extérieur, local technique]             |
|  [Baie brassage général] [Console]                       |
+----------------------------------------------------------+
  Allées chaudes entre les rangées (confinement)
  Distance rack/mur : ≥ 1 m (maintenance arrière)
  Chemins de câbles au plafond, boucle d'eau en caniveau
```

Règles : allées froides face aux façades avant, allées chaudes confinées vers
les RDHx/CTA, **1 m libre** derrière chaque rack (sortie d'un serveur 2U en
rails).

---

## 183. Groupe électrogène : dimensionnement

```
P_groupe(kVA) = (P_IT + P_froid_secouru) × 1,5 / 0,8
```

- 60 kW IT + 20 kW froid secouru = 80 kW → 80 × 1,5 / 0,8 = **150 kVA** →
  standard **200 kVA**.
- Prix : ≈ 40 000–60 000 € posé (à vérifier), + inverseur de source 15 k€.
- **Test mensuel en charge** (banc de charge ou délestage réel) : un groupe
  qui ne tourne jamais ne démarre jamais.
- Fioul : 200 L pour 8 h à 75 % de charge (ordre de grandeur — voir fiche
  groupe). Contrat de livraison fioul en astreinte.

---

## 184. Disjoncteurs et TGBT : le dimensionnement

| Circuit | Protection | Courbe | Pourquoi |
|---|---|---|---|
| PDU rack 32 A tri | 32 A tétrapolaire | C ou D | appel PSU |
| Onduleur 80 kW | 160 A | D | fort appel |
| Groupe | 250 A | D | — |
| Prises de maintenance | 16 A | C + 30 mA | personnel |

- **Courbe D** pour les charges à fort appel (onduleurs, groupes, gros
  serveurs) : la courbe C déclenche au démarrage simultané.
- **Sélectivité** : l'aval doit déclencher avant l'amont — étude à faire par
  l'électricien (pas au doigt mouillé).
- Terre : **< 5 Ω** (voir guide onduleurs), contrôle annuel.

---

## 185. TCO on-premise vs cloud — comparatif 3 ans chiffré

Hypothèse : 100 VMs (4 vCPU / 16 Go), 50 To stockage, 3 ans.

| Poste | On-premise (6× Compute M) | Cloud (ordre de grandeur) |
|---|---|---|
| CAPEX | 6 × 57 k€ = 342 k€ | 0 |
| Électricité 3 ans | 6 × 0,95 kW × 8 760 × 0,20 × 3 = 30 k€ | inclus |
| Maintenance 3 ans | 82 k€ | incluse |
| Licences (Proxmox) | 3 k€ | — |
| **Total 3 ans** | **≈ 457 k€** | — |
| Cloud équivalent | — | ≈ 25–35 k€/mois → **900 k€–1,26 M€ / 3 ans** |

**L'on-premise coûte 2–3× moins cher à 3 ans** pour une charge stable et
prévisible. Le cloud gagne quand : charge variable, besoin < 18 mois,
pas d'équipe infra. Refaites ce calcul à chaque projet — c'est le tableau
que la direction comprend.

---

## 186. Niveau Tier : ce que ça veut dire (et coûte)

| Tier (Uptime Institute) | Dispo | Redondance | Surcoût vs Tier I |
|---|---|---|---|
| I (basique) | 99,671 % | N | base |
| II (composants redondants) | 99,741 % | N+1 partiel | +15–25 % |
| III (maintenable sans arrêt) | 99,982 % | N+1, 2 chemins (1 actif) | +40–60 % |
| IV (tolérant aux pannes) | 99,995 % | 2N | +80–120 % |

Une PME vise **Tier II+** : onduleurs N+1, 2 PDU, froid N+1, groupe.
Le Tier III (tout maintenable sans arrêt) double presque le lot électrique —
à réserver aux applis qui le justifient vraiment.

---

## 187. PUE par design : les leviers

| Levier | Gain PUE | Coût |
|---|---|---|
| Allées confinées | −0,1 à −0,2 | faible |
| Free cooling | −0,3 à −0,5 | moyen (conception) |
| Température +3 °C (24→27 °C) | −0,05 à −0,1 | 0 € (ASHRAE le permet) |
| RDHx vs sur-clim | −0,1 | moyen |
| Onduleurs ecolo-mode (VFD) | −0,02 | 0 € (réglage) |

ASHRAE 2021 : **18–27 °C** en entrée serveur, classe A1. Faire tourner à
24 °C au lieu de 21 °C ne tue aucun serveur et économise 10–15 % de froid.
Le 21 °C « parce qu'on a toujours fait comme ça » est une habitude chère.

---

## 188. Supervision et DCIM : le minimum vital

| Outil | Rôle | Coût |
|---|---|---|
| Zabbix (voir guide zabbix) | métriques serveurs, PDU, sondes | 0 € |
| NetBox (voir guide netbox) | inventaire, câblage, IP | 0 € |
| NUT | arrêt auto onduleur (voir guide proxmox) | 0 € |
| Caméra thermique (FLIR One) | audit annuel | ≈ 300 € |
| DCIM commercial | capacity planning | 10–30 k€ (à vérifier) |

**Alertes critiques** (SMS/astreinte) : température > 27 °C, PDU > 80 %,
onduleur sur batterie, fuite d'eau, porte ouverte hors heures. Tout le reste
en mail.

---

## 189. Fibre inter-racks et inter-salles

- **OM4** jusqu'à 100 m en 100G (SR4), **OS2** au-delà (LR4).
- Prévoyez des **tiroirs de brassage** avec 30 % de ports libres.
- **2 chemins physiques** pour les liens critiques (A et B ne passent pas
  dans le même caniveau — un coup de pelle coupe les deux sinon).
- Testez chaque lien au **photomètre/OTDR** à la réception (pas « ça ping,
  c'est bon »).

---

## 190. Sécurité incendie : gaz, pas d'eau

- **Gaz inerte (IG-541, Novec)** : extinction sans résidu, sans danger pour
  l'électronique. Prix : 30 000–50 000 € pour 100 m² (à vérifier).
- Détection **aspiration (VESDA)** : détecte à l'état de pré-combustion.
- **Jamais de sprinkler à eau** sur les racks (sauf pré-action à double
  détection si le code l'impose).
- Exercice d'évacuation annuel, coupure électrique d'urgence identifiée et
  **testée** (bouton coup de poing).

---

## 191. Contrôle d'accès et traçabilité

- Badge nominatif, zones (salle serveurs ≠ local batteries ≠ TGBT).
- **Registre** : toute intervention notée (qui, quoi, quand).
- Caméras : allées + entrée, rétention 30 j mini.
- Fournisseurs : accompagnés, jamais seuls. **Sans exception.**

---

## 192. Exploitation : la check-list mensuelle

- [ ] Températures (min/max du mois) — dérive ?
- [ ] PDU : pic de charge < 80 % ?
- [ ] Onduleur : test batterie OK, autonomie mesurée
- [ ] Groupe : test en charge 30 min
- [ ] Disques : SMART, 0 erreur non corrigée
- [ ] Sauvegardes : 1 restore test
- [ ] Firmware : CVE critiques ?
- [ ] Consommables : filtres, étiquettes, DAC de rechange
- [ ] Registre d'accès : relu, anomalies ?

---

## 193. Négociation fournisseurs : les 7 leviers

1. **3 devis minimum**, même intégrateur habituel (le concurrent fait baisser).
2. **Fin de trimestre** : les commerciaux ont des objectifs — −5 à 10 %.
3. **Paiement comptant** vs 60 j : −2 à 3 % si vous pouvez.
4. **Garantie 5 ans** négociée au lieu de 3 : coûte moins cher à l'achat
   qu'en extension plus tard.
5. **Pièces détachées** : 1 lot (disques, PSU, ventilos) offert dès 10 nœuds.
6. **Burn-in + rapport** inclus par écrit.
7. **Reprise DEEE** de l'ancien parc : 0 € de coût caché.

---

## 194. Occasion et reconditionné : quand c'est pertinent

| Composant | Occasion intéressante ? | Prix constaté |
|---|---|---|
| H100 80 Go SXM | oui (garantie vendeur) | 18 000–22 000 $ (vérifié le 27/09/2026) |
| RTX 6000 Ada 48 Go | oui | ≈ 5 000–6 000 $ (à vérifier) |
| A100 80 Go | oui si budget serré | 12 000–18 000 $ (vérifié le 27/09/2026) |
| Serveurs complets N−1 | oui (garantie 1 an) | −40 à −60 % vs neuf |
| SSD d'occasion | **non** (endurance inconnue) | — |
| Batteries onduleur | **non** | — |

Règle : occasion = **GPU et serveurs** (testés, garantis), jamais = SSD,
batteries, câbles fibre (fragiles).

---

## 195. Fin de vie : DEEE et effacement

- **Effacement certifié** (NIST 800-88) avant toute sortie : disques, SSD
  (les SSD s'effacent mal — destruction physique pour le très sensible).
- DEEE : filière agréée, bordereau de suivi (obligation légale FR).
- Données : le disque qui part au recyclage **sans** effacement = fuite de
  données. Procédure écrite, registre d'effacement.

---

## 196. Retours terrain — 8 histoires vraies (anonymisées, ordres de grandeur)

1. **Le rack qui ne fermait pas.** 12 serveurs 2U commandés avec un rack
   1 000 mm « en promo ». Les PDU 0U + les bras de câbles dépassaient de
   12 cm. Solution : racheter 2 racks 1 200 mm (5 000 €) et 2 jours de
   déménagement. **Leçon : §127.**
2. **Le neutre qui chauffait.** 3 racks sur PDU tri 32 A, phases à 28/12/9 A.
   Le neutre à 65 °C, odeur de chaud un vendredi soir. Rééquilibrage le
   week-end, 0 incident. **Leçon : §131.**
3. **Le boot storm du lundi.** 250 VDI sur Ceph HDD (pas NVMe). À 8h55,
   latence 8 s, helpdesk saturé. Migration des images sur NVMe local :
   problème disparu. Coût : 12 000 € de NVMe. **Leçon : §66.**
4. **Le firmware qui divergeait.** 8 nœuds Ceph, 3 versions de BIOS. Un nœud
   rebootait aléatoirement (bug C-state corrigé dans la version récente).
   3 semaines de debug pour 20 min de MAJ. **Leçon : §170.**
5. **La clim « au total ».** Salle de 60 kW avec 2× 40 kW de clim… mais tout
   le froid soufflé d'un côté. Le rack du fond à 34 °C en entrée, throttling
   CPU. Ajout d'un confinement : −8 °C. **Leçon : §158.**
6. **L'onduleur en kVA.** 50 kW de serveurs sur 60 kVA fp 0,8 = 48 kW.
   Premier test de basculement : surcharge, bypass. Échange standard contre
   80 kW. **Leçon : §166.**
7. **Le backup jamais restauré.** 2 ans de backups Veeam « verts ». Le jour
   du ransomware : le repo n'était pas immuable, chiffré aussi. 3 semaines
   de reconstruction. **Leçon : §105, §107.**
8. **Le GPU non qualifié.** 4× RTX PRO 6000 dans un châssis 350 W/GPU.
   Throttling à 2 400 W dissipés au lieu de 2 400 W… en fait 600 W × 4 que le
   châssis ne pouvait pas extraire : −30 % de perfs. Changement de châssis.
   **Leçon : §99.**

---

## 197. Scripts — burn-in CPU/RAM

```bash
# CPU : 24 h, tous les cœurs
stress-ng --cpu 0 --cpu-method matrixprod --timeout 86400 --metrics-brief

# RAM : 1 passe complète (8 h pour 768 Go environ)
memtester 700G 1

# Surveillance pendant le test (autre terminal)
watch -n 5 'sensors | grep -E "Package|Tdie"; ipmitool sdr | grep -i fan'
```

---

## 198. Scripts — burn-in disques (fio)

```bash
# NVMe : 4k aléatoire, QD32, 2 h — comparez à la fiche (PM9A3: ~900k/200k)
fio --name=randread --ioengine=libaio --rw=randread --bs=4k \
    --iodepth=32 --numjobs=4 --runtime=7200 --time_based \
    --filename=/dev/nvme0n1 --direct=1 --group_reporting

fio --name=randwrite --ioengine=libaio --rw=randwrite --bs=4k \
    --iodepth=32 --numjobs=4 --runtime=7200 --time_based \
    --filename=/dev/nvme0n1 --direct=1 --group_reporting

# ATTENTION : randwrite use le SSD — ne le faites qu'au burn-in, jamais en prod.
# Vérifiez l'endurance restante après : nvme smart-log /dev/nvme0n1
```

---

## 199. Scripts — burn-in réseau (iperf3)

```bash
# Serveur (sur la cible)
iperf3 -s -D

# Client : 24 h, bidirectionnel, 4 flux
iperf3 -c <IP> -t 86400 -P 4 --bidir --logfile iperf.log

# Objectif 25G : ≥ 23 Gb/s utiles, 0 retransmit anormal
# Surveillez : ethtool -S eth0 | grep -i -E "drop|error|crc"
```

---

## 200. Scripts — inventaire rapide d'un nœud

```bash
#!/bin/bash
# inventaire.sh — à lancer sur chaque nœud, sortie vers l'as-built
echo "=== $(hostname) $(date -I) ==="
dmidecode -t processor | grep -E "Version|Core Count"
dmidecode -t memory | grep -E "Size|Speed|Part Number" | head -40
lsblk -d -o NAME,SIZE,MODEL | grep -v loop
lspci | grep -i -E "ethernet|vga|3d"
ipmitool fru | head -20
nvme list 2>/dev/null | awk '{print $1, $2, $13}'
ethtool eth0 2>/dev/null | grep -E "Speed|Duplex"
```

---

## 201. Gabarit — BOM vierge (à copier par projet)

```
PROJET : _______________  DATE : ___________  VERSION : ___
| # | Composant | Référence exacte | Qté | Prix unit. | Total | Délai | Fournisseur |
|---|-----------|------------------|-----|------------|-------|-------|-------------|
| 1 |           |                  |     |            |       |       |             |
...
|   | SOUS-TOTAL MATÉRIEL |            |       |             |
|   | Intégration (10 %)  |            |       |             |
|   | TOTAL PROJET        |            |       |             |
P_max calculée : _____ kW | P_réaliste : _____ kW | PDU : _________
Refroidissement : ________________ | Onduleur : ________________
```

---

## 202. Gabarit — plan de phases PDU

```
RACK : _____  PDU : _____ (A/B)  Date : _____
| Phase | Équipements | P (kW) | I (A) | % charge |
|-------|-------------|--------|-------|----------|
| L1    |             |        |       |          |
| L2    |             |        |       |          |
| L3    |             |        |       |          |
| TOTAL |             |        |       |          |
Écart max entre phases : ____ % (cible < 20 %)
```

---

## 203. Gabarit — plan de câblage (extrait NetBox)

```
Câble ID | De | Vers | Type | Longueur | Couleur | Date | Par
R01-C001 | R01-TOR1-E25 | R01-SRV03-E0 | DAC 25G | 3 m | bleu | ... | ...
```

---

## 204. Tableau de conversion — unités

| De | Vers | Facteur |
|---|---|---|
| kW | BTU/h | × 3 412 |
| kW | kcal/h | × 860 |
| To | Tio | × 0,9095 |
| Gb/s | Go/s | / 8 |
| 1 To à 10 Gb/s | temps de transfert | ≈ 15 min |
| 1 To à 25 Gb/s | temps de transfert | ≈ 6 min |
| 1 kW pendant 1 an | kWh | × 8 760 |
| 1 kW à 0,20 €/kWh/an | €/an | 1 752 € |

---

## 205. Dimensionnement rapide — l'antisèche du chef

```
RAM DB        = working set × 1,3
RAM Ceph      = 5 Go × To bruts + 4 Go
IOPS          = pic mesuré × 1,5
Réseau        = débit / 0,7 (jamais > 70 %)
PSU/PDU       = P_max × 1,25, chargée à 80 %
Onduleur (kW) = Σ P_réaliste × 1,25
Froid (m³/h)  = kW × 300
Batteries     = P × autonomie / 0,95
Rack          = 42U, 1 200 mm, 2 PDU A/B
Marge         = +30 % capacité à 3 ans
```

---

## 206. Normes et docs à connaître

| Référence | Sujet |
|---|---|
| ASHRAE TC 9.9 | Environnement thermique datacenter (18–27 °C) |
| Uptime Institute Tier | Niveaux de disponibilité |
| EN 50600 | Conception datacenter (Europe) |
| ISO 27001 A.11 | Sécurité physique |
| NIST 800-88 | Effacement des médias |
| FIPS 140-3 | Certification HSM |
| RGPD art. 32 | Sécurité des traitements (logs, rétention) |

---

## 207. Index des BOMs du guide

| Build | Section BOM | Section conso | Prix | P_max |
|---|---|---|---|---|
| Compute S | §18 | §19 | ≈ 29 750 € | 0,62 kW |
| Compute M | §20 | §21 | ≈ 56 975 € | 1,26 kW |
| PG OLTP S | §28 | §28 | ≈ 46 665 € | 0,79 kW |
| PG OLTP M | §29 | §29 | ≈ 100 760 € | 1,43 kW |
| PG Warehouse | §30 | §30 | ≈ 144 125 € | 1,54 kW |
| Ceph OSD NVMe S | §42 | §42 | ≈ 49 320 € | 0,78 kW |
| Ceph OSD NVMe M | §43 | §43 | ≈ 96 180 € | 1,49 kW |
| MON/MGR (×3) | §44 | §44 | ≈ 40 455 € | 1,35 kW |
| Ceph hybride S | §54 | §54 | ≈ 34 650 € | 0,74 kW |
| Ceph hybride M | §55 | §55 | ≈ 67 680 € | 1,53 kW |
| VDI S (100 users) | §63 | §63 | ≈ 64 205 € | 1,35 kW |
| VDI M vGPU (100 users) | §64 | §64 | ≈ 129 130 € | 2,68 kW |
| SIEM S | §73 | §73 | ≈ 55 895 € | 0,95 kW |
| SIEM M | §74 | §74 | ≈ 111 605 € | 1,60 kW |
| 2× MI300X | §82 | §82 | ≈ 77 560 € | 2,30 kW |
| 4× MI300X | §83 | §83 | ≈ 144 440 € | 4,40 kW |
| 2× L40S | §92 | §92 | ≈ 47 485 € | 1,38 kW |
| 4× L40S | §93 | §93 | ≈ 96 540 € | 2,69 kW |
| 2× RTX PRO 6000 | §94 | §94 | ≈ 50 585 € | 1,88 kW |
| 4× RTX PRO 6000 | §95 | §95 | ≈ 103 740 € | 3,74 kW |
| Backup S | §102 | §102 | ≈ 25 370 € | 0,70 kW |
| Backup M | §103 | §103 | ≈ 48 535 € | 0,85 kW |
| Firewall 1G HA | §110 | §110 | ≈ 940 € | 0,03 kW |
| Firewall 10G HA | §111 | §111 | ≈ 6 660 € | 0,24 kW |
| Firewall 25G HA | §112 | §112 | ≈ 14 890 € | 0,50 kW |
| HSM (paire) | §115–116 | §116 | devis | 0,60 kW |
| Rack 42U équipé | §138 | — | ≈ 9 300 € | — |
| Salle 8 racks 60 kW | §181 | §181 | ≈ 2,78 M€ | 60 kW |

---

## 208. Sources des prix (27/09/2026)

- CPU EPYC, châssis 1U/2U, RAM configurateurs : rect.coreto.de / rect.coreto-europe.com,
  ahead-it.eu, scan.co.uk (relevés 24–27/09/2026).
- GPU : snapshot Fusionww mars 2026 (via sourcebyspec.com), MSRP NVIDIA
  (TechRadar), prix cloud RunPod/Spheron/DigitalOcean (relevés 09/2026).
- SSD Samsung PM9A3 : tech-america.com, hssl.us (relevés 09/2026).
- DRAM : TrendForce via sedaily.com (16/09/2026).
- PDU APC, racks : dell.com, superwarehouse.com (relevés 09/2026).
- Firewall : shop.opnsense.com (fiches DEC), info.netgate.com (comparatif
  janvier 2026).
- Refroidissement/800VDC : datacentremagazine.com, upsite.com (AFCOM 2026),
  techtimes.com, finance.biggo.com (relevés 09/2026).
- HSM : itbrief.co.uk (annonce Thales Luna 8, 09/2026), peerspot.com.

---

## 209. Ce que ce guide ne couvre pas (volontairement)

- Le câblage électrique amont (TGBT, transfo) : domaine de l'électricien.
- Le génie civil (dalle, désenfumage) : bureau d'études.
- Les licences logicielles détaillées (VMware, Veeam) : devis éditeurs.
- La cybersécurité applicative : voir guides wazuh/fail2ban/ssh.
- L'administratif (ICPE, déclarations) : selon votre pays et puissance.

---

## 210. Historique des versions du guide

| Version | Date | Changements |
|---|---|---|
| 1.0 | 27/09/2026 | Version initiale : 12 workloads, 26 BOMs, datacenter complet |

---

## 211. Dernier mot — l'ordre des priorités

Si vous ne retenez que cinq choses :

1. **La RAM se mesure** (working set × 1,3) — à 1 500 $/64 Go, l'à-peu-près
   coûte des dizaines de milliers d'euros.
2. **Les watts s'additionnent** (§125) — tout le dimensionnement électrique
   et froid en découle.
3. **Le réseau se dimensionne au pic** (rebuild, boot storm, backup) —
   pas à la moyenne.
4. **Le backup se teste** (restore mensuel, immuabilité) — sinon il n'existe pas.
5. **L'as-built se livre** — une salle sans documentation n'est pas une salle,
   c'est un risque.

Bon build. Et quand le commercial vous dit « c'est standard », sortez la
check-list §122.

---

## 212. Comparatif CPU détaillé — EPYC Turin vs Xeon 6 (2026)

| Critère | AMD EPYC 9005 (Turin) | Intel Xeon 6 (Granite Rapids) |
|---|---|---|
| Cœurs max | 160 (9845) | 128 (6980P) |
| Canaux mémoire | 12× DDR5 | 12× DDR5 / MRDIMM |
| Lanes PCIe | 128 (5.0) en 1P | 96 (5.0) |
| TDP max | ~500 W | ~500 W |
| Prix/cœur | le plus bas du marché | +20–40 % (à vérifier) |
| Atouts | densité, prix, écosystème large | AMX (IA), QAT (crypto), certains ISV |
| Verdict guide | **choix par défaut** | si l'appli l'exige |

Le Xeon 6 se justifie pour : appliances certifiées Intel, inférence CPU avec
AMX (jusqu'à 2× vs sans), QAT pour le chiffrement/IPsec à haut débit.
Sinon, EPYC.

---

## 213. Quel EPYC pour quel workload — tableau de choix

| Workload | CPU recommandé | Pourquoi |
|---|---|---|
| Virtualisation dense | 9655P (96c) | cœurs pas chers |
| DB OLTP | 9375F / 9475F (F) | fréquence, latence |
| HPC | 9655P / 9845 | cœurs + bande passante |
| Ceph OSD | 9355P / 9555P | cœurs/€, lanes PCIe |
| VDI | 9655P (96c) | vCPU par € |
| Inférence | 9455P / 9555P | équilibre, lanes pour GPU |
| Firewall 25G+ | 9124 (16c) | suffit, économe |
| Backup | 9355P (32c) | dédup multithread |

---

## 214. Comparatif SSD NVMe entreprise (2026)

| Modèle | Format | Endurance | Lecture | Prix/To indicatif |
|---|---|---|---|---|
| Samsung PM9A3 | U.2 | 1 DWPD | 6 900 Mo/s | ≈ 555 $/To (7,68 To) |
| Samsung PM9A5 | U.2 | 3 DWPD | 6 900 Mo/s | ≈ 800 $/To (à vérifier) |
| Micron 7450 PRO | U.2/E1.S | 1–3 DWPD | 6 800 Mo/s | ≈ 600–900 $/To (à vérifier) |
| Kioxia CM7 | U.2/E3.S | 1–3 DWPD | 14 000 Mo/s (PCIe 5) | ≈ 700–1 000 $/To (à vérifier) |
| Solidigm D7-P5520 | U.2 | 1–3 DWPD | 7 100 Mo/s | ≈ 600 $/To (à vérifier) |

Le PM9A3 reste la référence prix/perf en read-intensive. Pour du PCIe 5.0
plein débit (14 Go/s), Kioxia CM7 et équivalents — mais vérifiez que le
châssis et le CPU négocient bien en Gen5 (sinon vous payez pour rien).

---

## 215. Comparatif NIC 25/100G

| NIC | Ports | Débit | RDMA | Prix indicatif |
|---|---|---|---|---|
| Intel E810-XXVDA2 | 2× 25G | 25G | RoCE | ≈ 450 € |
| Broadcom P225P | 2× 25G | 25G | RoCE | ≈ 350 € (à vérifier) |
| Mellanox CX6-DX | 2× 100G | 100G | RoCE v2 | ≈ 1 100 € |
| Mellanox CX7 | 2× 400G | 400G | RoCE v2 | ≈ 3 000 € (à vérifier) |

Pour Ceph : CX6-DX en 100G = le standard. L'E810 suffit en 25G front.
**Vérifiez la compatibilité OCP vs PCIe** avec votre châssis avant commande.

---

## 216. Cas chiffré : Ceph NVMe 1 Po utile, réplica 3

Besoin brut : 1 000 / 0,85 × 3 = 3 530 To → 29 nœuds M (29 × 123 To).

| Poste | Détail | Total |
|---|---|---|
| 29× nœuds OSD M | 96 180 € × 29 | 2 789 220 € |
| 5× MON/MGR (cluster large) | 13 485 € × 5 | 67 425 € |
| 2× switch 100G 64p (back) | 25 000 € × 2 | 50 000 € |
| 2× switch 25G 48p (front) | 8 000 € × 2 | 16 000 € |
| Câblage/optiques | lot | 30 000 € |
| **CAPEX** | | **≈ 2 953 000 €** |
| P_max | 29 × 1,49 + 5 × 0,45 + 2 | **≈ 47 kW** |
| **Coût/To utile** | 2 953 000 / 1 000 | **≈ 2 950 €/To** |

47 kW = **2 racks à 24 kW** (RDHx) ou 1 rangée DLC. À cette échelle,
négociez les SSD par lot de 500 : −10 à 15 % réaliste.

---

## 217. Cas chiffré : VDI 1 000 postes bureautiques

| Poste | Détail | Total |
|---|---|---|
| 10× serveurs VDI S (N+1 : 9+1) | 64 205 € × 10 | 642 050 € |
| Licences VDI (1 000 users) | ≈ 200 €/user/an | 200 000 €/an |
| Stockage profils (CephFS 60 To) | — | 80 000 € |
| 1 000× thin clients | 400 € × 1 000 | 400 000 € |
| Réseau (quote-part) | — | 40 000 € |
| **CAPEX année 1** | | **≈ 1 162 000 € + 200 k€/an** |
| P_max | 10 × 1,35 kW | **≈ 13,5 kW** |
| **Coût/poste/an (5 ans)** | (1 162 000 + 1 000 000) / 1000 / 5 | **≈ 432 €** |

---

## 218. Cas chiffré : PostgreSQL 10 To actifs, HA + backup

| Poste | Détail | Total |
|---|---|---|
| 2× PG Warehouse (adapté 10 To) | 144 125 € × 2 | 288 250 € |
| 1× témoin | — | 8 000 € |
| Repo backup (30 To utiles) | serveur S adapté | 30 000 € |
| **CAPEX** | | **≈ 326 000 €** |
| P_max | 2 × 1,54 + 0,5 | **≈ 3,6 kW** |

---

## 219. Dimensionnement disjoncteurs — exemple TGBT complet

Salle §181 (60 kW IT) :

| Départ | Calibre | Courbe | Section câble |
|---|---|---|---|
| 8× PDU rack (tri 32 A) | 8× 32 A tétra | C | 5G6 mm² |
| 2× onduleurs 80 kW | 2× 160 A tétra | D | 4× 70 mm² |
| Groupe 200 kVA | 250 A tétra | D | 4× 120 mm² |
| Climatisation | 3× 63 A tétra | D | 5G16 mm² |
| Éclairage + prises | 2× 20 A | C + 30 mA | 3G2,5 mm² |
| Arrivée générale | 400 A tétra | — | TGBT 400 A |

**Étude par un électricien qualifié obligatoire** — ce tableau est une base
de discussion, pas un schéma d'exécution.

---

## 220. Tableau — temps de transfert utiles

| Volume | 1 Gb/s | 10 Gb/s | 25 Gb/s | 100 Gb/s |
|---|---|---|---|---|
| 1 To | 2 h 30 | 15 min | 6 min | 1 min 30 |
| 10 To | 25 h | 2 h 30 | 1 h | 15 min |
| 100 To | 10 j | 25 h | 10 h | 2 h 30 |
| 1 Po | 104 j | 10 j | 4 j | 25 h |

À coller au mur de la salle : c'est ce tableau qui fait comprendre pourquoi
le 25G est le minimum en 2026 pour tout ce qui brasse de la donnée.

---

## 221. Erreurs de devis les plus coûteuses (vécu)

| Erreur | Surcoût typique |
|---|---|
| RAM commandée en 2 DPC au lieu d'1 (perte 20 % bande passante) | refonte partielle : 5–10 k€ |
| NIC PCIe au lieu d'OCP (ne rentre pas) | retour + port : 1–2 k€ |
| SSD SATA commandés au lieu de NVMe U.2 (mauvais backplane) | incompatibles : 10–30 k€ |
| Onduleur sans bypass externe | arrêt pour maintenance : coût d'indispo |
| PDU 16 A pour un rack à 9 kW | déclenchements : 2 k€ de PDU |
| Garantie oubliée (restée à 1 an) | extension après-coup : +30 % |

**Relisez chaque devis ligne par ligne avec la check-list §122.**
Le copier-coller d'un commercial pressé coûte plus cher que votre temps.

---

## 222. Garantie et pièces détachées — la politique

| Parc | Politique recommandée |
|---|---|
| < 10 serveurs | Garantie constructeur 5 ans NBD |
| 10–50 serveurs | 3 ans NBD + stock pièces (2 disques, 1 PSU, 1 ventilo) |
| > 50 serveurs | 3 ans + stock 5 % + contrat 4 h sur le critique |

Un disque de rechange **sur étagère** vaut mieux qu'une garantie NBD quand
le rebuild prend 28 h (§59) : le NBD ne raccourcit pas le rebuild, le spare
si.

---

## 223. Câbles d'alimentation : C13 vs C19 vs C21

| Connecteur | Intensité | Usage |
|---|---|---|
| C13/C14 | 10 A | serveurs 1U/2U standard |
| C19/C20 | 16 A | serveurs GPU, gros 2U/4U |
| C21/C22 | 20 A | très haute densité (à vérifier) |

**Vérifiez le connecteur du serveur AVANT de commander les PDU** : une PDU
full C13 ne branche pas un serveur GPU en C19. Les PDU modernes mixtes
(C13/C19/C21, §129) évitent le problème.

---

## 224. Températures de fonctionnement — limites réelles

| Composant | Limite conseillée | Throttling/danger |
|---|---|---|
| CPU EPYC | < 75 °C | > 95 °C (Tjmax) |
| GPU L40S/MI300X | < 80 °C | > 90–95 °C |
| NVMe | < 60 °C | > 70–75 °C |
| HDD | 25–40 °C | > 50 °C |
| Entrée rack (ASHRAE A1) | 18–27 °C | > 32 °C (classe A2+) |

Le NVMe est le plus fragile : à 75 °C il throttle et vos IOPS s'effondrent.
**Surveillez les températures SSD** (smartctl), surtout dans les 2U denses.

---

## 225. Check-list annuelle d'exploitation

- [ ] Revue thermique caméra IR (connexions électriques chaudes ?)
- [ ] Resserrage des bornes TGBT/PDU (dilatation thermique)
- [ ] Test groupe en charge 1 h + analyse fioul
- [ ] Test autonomie onduleur (décharge réelle mesurée)
- [ ] Exercice restore backup complet (1 appli critique)
- [ ] Failover firewall réel
- [ ] Revue des accès (badges, comptes BMC)
- [ ] Mise à jour as-built (tout ce qui a changé dans l'année)
- [ ] Renégociation contrats maintenance
- [ ] Plan de capacité : à 70 % d'une ressource, on commande

---

## 226. Plan de branchement PDU — exemple par serveur

```
Serveur srv-03 (Compute M, 2 PSU) :
  PSU-A (C19) → PDU-A prise 12 (phase L1) — cordon ROUGE
  PSU-B (C19) → PDU-B prise 12 (phase L1) — cordon NOIR

Règle : même numéro de prise sur A et B pour un serveur donné.
Le technicien trouve la prise en 10 secondes, même à 3 h du matin.
Documenté dans : plan de câblage (§203) + étiquette sur le cordon.
```

---

## 227. Chemins de câbles — dimensionnement

| Câbles | Largeur chemin | Charge |
|---|---|---|
| 50 DAC + 20 fibres | 300 mm | ~15 kg |
| 150 câbles mixtes | 600 mm | ~40 kg |
| Fibre inter-salles | goulotte fermée 100 mm | — |

- Charge admissible : vérifiez la fiche (un chemin plein de cuivre pèse lourd).
- **Séparation** : cuivre / fibre / électrique dans 3 chemins ou 3
  compartiments. Le 230 V à côté du DAC 25G = diaphonie et danger.
- Mise à la terre du chemin métallique (obligatoire).

---

## 228. Consignation électrique (LOTO) — procédure

1. **Identifier** le circuit (étiquette, plan TGBT).
2. **Séparer** : ouvrir le disjoncteur, verrouiller avec cadenas personnel.
3. **Condamner** : étiquette « NE PAS MANŒUVRER » nominative.
4. **Vérifier** : VAT (vérificateur d'absence de tension) sur chaque phase.
5. **Travailler**, puis déconsigner en sens inverse.

**Un seul cadenas par intervenant.** Le chef ne retire jamais le cadenas d'un
autre. C'est la règle qui sauve des vies — pas une option.

---

## 229. Gestion des changements (change management)

| Changement | Procédure |
|---|---|
| Standard (MAJ firmware planifiée) | ticket, fenêtre, rollback |
| Normal (nouveau serveur) | demande, validation, as-built MAJ |
| Urgent (panne) | intervention, ticket a posteriori sous 24 h |

**Fenêtres de maintenance** : 1×/mois, nuit ou week-end, communiquées 1 sem
avant. **Jamais de changement non tracé en prod** — le « petit fix rapide »
est la cause n° 1 des incidents majeurs.

---

## 230. Faut-il secourir la climatisation sur onduleur ?

| Scénario | Réponse |
|---|---|
| Coupure < 15 min + arrêt auto (NUT) | **Non** : les serveurs tiennent thermiquement 10–15 min sans froid |
| Groupe électrogène présent | **Non** : le groupe reprend le froid en < 2 min |
| Pas de groupe, autonomie > 30 min | **Oui** : sinon la salle surchauffe avant la fin des batteries |
| Salle GPU dense (> 20 kW/rack) | **Oui** : l'inertie thermique est de quelques minutes seulement |

Règle : **l'inertie thermique d'une salle air = 10–20 min**, d'une salle
GPU dense = 3–5 min. Dimensionnez en conséquence (§164).

---

## 231. Scénario blackout — la procédure

```
T+0    : coupure. Onduleurs sur batteries, alerte SMS.
T+2    : groupe démarre (auto). Vérifier prise de charge.
T+5    : si groupe OK → rien à faire, surveillance.
T+5    : si groupe KO → délestage : arrêt NUT des non-critiques.
T+10   : si toujours pas de groupe → arrêt propre général (runbook).
T+30   : batteries vides. Tout est arrêté proprement. On attend.
Retour : groupe ou réseau → démarrage séquencé (§166), vérif services.
```

**Testez ce scénario 1×/an** (un dimanche). Le runbook non testé est un vœu
pieux. Chronométrez chaque étape.

---

## 232. Connecteurs fibre — tableau

| Connecteur | Usage | Remarque |
|---|---|---|
| LC duplex | 10/25/100G SR4 (via MPO→LC) | standard 2026 |
| MPO-12 | 100/400G SR4 | 1 connecteur = 4–8 fibres |
| SC | ancien, management | en voie de disparition |
| E2000 | laser haute puissance | rare en datacenter |

**Nettoyage** : stylo de nettoyage ou cassette avant CHAQUE connexion.
Une fibre sale = −3 dB = lien instable. Le microscope d'inspection (200 €)
est rentabilisé au premier lien douteux.

---

## 233. Brassage fibre — bonnes pratiques

- Tiroirs coulissants avec **gestion du mou** (jamais de fibre tendue).
- Étiquette à chaque extrémité : `R01-T1-P03 → R02-T1-P03`.
- **Ne regardez jamais** dans une fibre active (laser 850/1310 nm, invisible,
  dangereux pour la rétine).
- Stock : 10 % de jarretières de rechange, 2 cassettes de nettoyage.

---

## 234. Glossaire complémentaire (12 termes)

| Terme | Définition |
|---|---|
| **LOTO** | Lockout/Tagout : consignation électrique par cadenas personnel. |
| **VAT** | Vérificateur d'Absence de Tension : test avant intervention. |
| **TGBT** | Tableau Général Basse Tension : arrivée électrique du bâtiment. |
| **THD** | Taux de distorsion harmonique : les PSU à PFC le maintiennent < 5 %. |
| **Cos φ / fp** | Facteur de puissance : kW/kVA. 1,0 = parfait (PFC actif). |
| **MPO** | Multi-fiber Push On : connecteur 12/24 fibres pour 100/400G. |
| **OTDR** | Réflectomètre : mesure les défauts d'une fibre (distance, perte). |
| **VESDA** | Détection incendie par aspiration : alerte précoce. |
| **IG-541** | Gaz inerte d'extinction (azote/argon/CO2), sans résidu. |
| **MTBF** | Mean Time Between Failures : fiabilité théorique (ex : 2 Mh SSD). |
| **RPO / RTO** | Perte de données max / durée d'arrêt max acceptées. |
| **DEEE** | Déchets d'équipements électriques et électroniques : filière obligatoire. |

---

## 235. La phrase à retenir par partie

| Partie | Phrase |
|---|---|
| A — Méthode | Mesurez d'abord, la DRAM 2026 ne pardonne pas l'à-peu-près. |
| B — BOMs | Chaque workload a sa BOM ; le tableau §125 les relie à l'électrique. |
| C — Datacenter | 2 PDU A/B, phases équilibrées, rack 1 200 mm, obturateurs partout. |
| Câblage | DAC en rack, fibre dehors, velcro toujours, étiquettes des 2 côtés. |
| Froid | kW × 300 = m³/h ; au-delà de 20 kW/rack, le liquide s'impose. |
| Énergie | Onduleur en kW, batteries testées, groupe testé en charge. |
| MES | Burn-in 72 h, firmware uniformes, as-built livré. |
| Échelle | Copiez les méthodes des hyperscalers, pas leurs densités. |

---

## 236. Fin du guide

Ce guide est un **outil de travail**, pas une lecture : imprimez l'antisèche
(§205), la check-list devis (§122) et le top 15 des pièges. Le reste vit dans
le classeur de la salle, à côté de l'as-built.

Prochaines étapes suggérées : faire chiffrer 3 devis sur la BOM Compute S
(§18) pour étalonner VOS prix réels — c'est le test qui valide tout le reste.

*— Fin du guide datacenter_builds — 27/09/2026.*

---

## 237. FAQ express — les questions qu'on pose toujours

**Faut-il du bi-socket en 2026 ?**
Rarement. Un EPYC 9655P 96 cœurs mono-socket bat la plupart des bi-socket
d'hier. Le 2P ne se justifie que par > 1,5 To RAM ou > 96 lanes PCIe utiles.

**AMD ou Intel ?**
AMD par défaut (prix/cœur, lanes). Intel si l'appli/l'appliance l'exige
(AMX, QAT, certification).

**Combien de RAM par cœur ?**
4 Go mini (HPC), 8 Go standard (virt/K8s), 12–16 Go (IA/data).

**SATA a-t-il encore un sens ?**
Pour le boot en miroir (bon marché) et l'archivage froid. Jamais en perf.

**Ceph ou ZFS ?**
Ceph = distribué, multi-nœuds, S3. ZFS = local, 1–2 nœuds, simple.
En dessous de 100 To sur 3 nœuds, ZFS répliqué (Proxmox) est souvent plus
simple que Ceph.

**Cloud ou on-premise ?**
Charge stable > 18 mois → on-premise (2–3× moins cher, §185).
Charge variable ou < 18 mois → cloud.

**Quelle température en salle ?**
24–27 °C en entrée rack (ASHRAE A1). 21 °C = habitude chère.

**Faut-il un groupe électrogène ?**
Oui dès que l'indisponibilité coûte plus cher que 55 k€ + fioul.
En pratique : oui pour toute salle > 20 kW.

**Onduleur : quelle autonomie ?**
15 min + arrêt auto (NUT). Plus = groupe, pas des batteries.

**Par où commencer ?**
Un rack §120 (5,6 kW, 354 k€) : c'est le « datacenter minimum viable ».

---

## 238. Où acheter — les circuits (France/Europe)

| Circuit | Pour | Remarque |
|---|---|---|
| Intégrateurs (Serveur/boutiques pro) | serveurs complets | devis, burn-in, garantie — le circuit recommandé |
| Distributeurs (Tech Data, Ingram) | volumes, licences | via revendeur |
| Constructeurs direct (Dell, HPE) | parc homogène | cher mais support intégré |
| Occasion pro (garantie 1 an) | GPU, serveurs N−1 | §194 — jamais SSD/batteries |
| Grossistes câble/fibre | câblage | FS.com et équivalents (optiques codées) |

**Toujours 3 devis** (§193). Le prix public de ce guide sert d'étalon pour
détecter les devis anormaux (±30 % = question à poser).

---

## 239. Lexique anglais-français rapide

| EN | FR |
|---|---|
| Bill of Materials | nomenclature |
| Burn-in | test d'endurance |
| Hot-swap | remplaçable à chaud |
| Cold plate | plaque froide (DLC) |
| Rear door heat exchanger | échangeur porte arrière |
| Top of Rack | haut de baie |
| Power distribution unit | unité de distribution |
| Uninterruptible power supply | onduleur / ASI |
| Erasure coding | codage à effacement |
| Scrubbing | contrôle d'intégrité |
| Failover | bascule |
| As-built | dossier de récolement |

---

## 240. Les 10 chiffres à connaître par cœur

1. 1 500 $ — le 64 Go DDR5 en contrat (09/2026).
2. 300 m³/h — le débit d'air par kW (ΔT 12 °C).
3. 85 % — le remplissage max d'un pool Ceph.
4. 80 % — la charge max d'une PDU en nominal.
5. 20 % — l'écart max entre phases.
6. 1 752 € — le coût annuel d'1 kW à 0,20 €/kWh.
7. 28 h — le rebuild d'un HDD 20 To.
8. 72 h — le burn-in avant prod.
9. 15 min — l'autonomie onduleur standard.
10. 3 — le nombre de devis minimum.

---

## 241. Pour aller plus loin (guides liés de Zelef)

- `onduleurs_ups_guide.md` — le détail onduleurs/batteries/groupes.
- `proxmox_guide.md` — grappe Proxmox + NUT (arrêt auto).
- `zabbix_guide.md` — supervision (sondes, PDU, températures).
- `netbox_guide.md` — inventaire et plan de câblage.
- `debian_ubuntu_guide.md` — OS des serveurs.
- `ansible_guide.md` — automatisation du provisionning.

---

## 242. Crédits et méthode de recherche

Prix relevés sur le web public le 27/09/2026 : configurateurs européens
(rect.coreto.de, ahead-it.eu, scan.co.uk), snapshots de marché GPU
(sourcebyspec.com, gpusmith.com), boutiques (tech-america.com, dell.com,
superwarehouse.com), constructeurs (shop.opnsense.com, info.netgate.com),
presse spécialisée (sedaily.com/TrendForce, datacentremagazine.com,
upsite.com/AFCOM 2026). Les prix « à vérifier » sont des ordres de grandeur
2026 sans source directe ce jour-là. Aucun prix n'a été inventé.

---

## 243. Note de maintenance du guide

- Relire la section 3 (marché) avant chaque budget annuel : DRAM, GPU et
  délais bougent par trimestre en 2026.
- Mettre à jour le tableau §125 à chaque nouvel achat : c'est la référence
  électrique de la salle.
- Archiver chaque devis reçu avec sa date : c'est l'historique de prix qui
  rend les négociations suivantes crédibles.
- Ce guide ne remplace ni l'électricien (TGBT, sélectivité), ni le BET
  (structure, froid), ni le devis écrit du fournisseur (délais GPU/DRAM).
- En cas de doute sur un dimensionnement : surdimensionnez la marge, pas le
  matériel — +30 % de place libre coûte moins cher qu'un rack à refaire.

*Document de travail — version 1.0 — 27/09/2026.*
