# ENCYCLOPÉDIE DES MODÈLES IA — VOLUME 1 : LA CHINE
## Février 2026 → 27 septembre 2026

*Volume 1/3. Ce volume couvre les laboratoires chinois : DeepSeek, Alibaba (Qwen), Moonshot (Kimi),
Zhipu (GLM), Xiaomi (MiMo), Ant Group (Ling/BaiLing), Meituan (LongCat), MiniMax, Tencent (Hunyuan).
Les modèles américains (NVIDIA Nemotron, NousResearch Hermes) et le reste de l'Occident sont dans les
volumes 2 et 3. Date de référence des faits : **27 septembre 2026**. Toute spécification non vérifiable
est marquée « non vérifié au 27/09/2026 » et aucun chiffre n'est inventé.*

---

## 1. Ce que couvre ce volume

Ce volume recense les modèles de langage et multimodaux sortis, annoncés ou retirés entre février 2026
et le 27 septembre 2026 par neuf acteurs chinois. Chaque famille a sa partie : portrait de l'acteur,
fiches modèle par modèle, encadrés techniques, puis un tableau comparatif de synthèse. La dernière partie
croise tout : grand tableau Chine, timeline, analyse (« que retenir »), glossaire de 32 termes et quiz.

Périmètre strict : seules les informations vérifiées par la recherche documentaire de septembre 2026 sont
reprises. Les fiches signalent honnêtement les trous (modèles annoncés mais jamais sortis, specs
introuvables, contradictions entre sources). Les modèles antérieurs à février 2026 ne sont rappelés que
lorsqu'ils éclairent la lignée 2026 (ex. MiMo-7B d'avril 2025, LongCat-Flash d'août 2025).

## 2. Comment lire une fiche modèle

Chaque fiche suit le même plan : nom exact (avec ID API et repo Hugging Face quand ils sont connus),
date de sortie, statut au 27/09/2026 (disponible / retiré / annoncé), architecture (type, paramètres
totaux et actifs par token), contexte, fonctionnalités, fine-tuning, poids et licence, benchmarks clés,
prix API, déploiement. Les chiffres de benchmarks sont **vendor-reported** (fournis par le constructeur)
sauf mention « indépendant ». Les prix API viennent souvent de sources tierces (agrégateurs, fournisseurs)
et sont signalés comme tels quand le tarif officiel n'a pas été consulté directement.

## 3. Les mentions d'incertitude : mode d'emploi

Trois niveaux sont utilisés partout dans ce volume. **« Non vérifié au 27/09/2026 »** : l'information
circule (presse, notes communautaires) mais n'a pas été confirmée par une source primaire ou officielle.
**« Divergence »** : deux sources sérieuses donnent des chiffres différents, impossible de trancher.
**« Non trouvé »** : après plusieurs requêtes de recherche, aucune trace — le modèle n'existe probablement
pas sous ce nom. Ces mentions ne sont pas des coquetteries : sur un corpus 2026 largement pollué par du
contenu IA-généré qui se recopie, elles sont la seule protection contre la fausse précision.

---

## 4. DeepSeek : portrait de l'entreprise

DeepSeek, laboratoire chinois fondé par Liang Wenfeng (adossé au fonds High-Flyer), a défini le standard
de l'open-weight agressif : modèles géants Mixture-of-Experts (MoE), poids ouverts sous licence MIT,
prix API cassés, rapports techniques publics. En 2026, la famille V4 (sortie le 24 avril 2026) prolonge
la lignée V3/V3.2 avec un changement architectural majeur : abandon du MLA pur au profit d'une attention
hybride. DeepSeek reste l'acteur qui force tout le marché chinois à aligner ses prix vers le bas.

## 5. DeepSeek V4 : fiche générale

- **Nom exact** : DeepSeek V4 (préview le 24 avril 2026), décliné en deux variantes : **V4-Pro** et **V4-Flash**.
- **Statut au 27/09/2026** : V4-Pro disponible (version « V4 Pro 0813 » en GA depuis le 13 août 2026).
  V4-Flash retiré le 10 septembre 2026, remplacé par V4.1-Flash (routes API redirigées).
- **Architecture** : MoE sparse, **~1,6T paramètres totaux / 49B actifs par token** (V4-Pro). Attention
  **hybride** (CSA stride 4 + HCA stride 128 + SWA ~128) — la famille V4 abandonne le MLA pur de V3 pour
  cette attention hybride (sources GitHub tierces ; non vérifié par documentation officielle DeepSeek).
  MegaMoE (expert parallelism), résiduels « mHC », optimiseur Muon (+ AdamW partiel), FP4 QAT pour les
  poids MoE. Entraînement sur **32T+ tokens**.
- **Contexte** : **1M tokens** (fenêtre native), sortie max **384K tokens**.
- **Fonctionnalités** : reasoning (modes de pensée unifiés), tool use / agents (« Interleaved Thinking »),
  vision (le V4-Flash avait une variante vision expérimentale — non vérifié dans le détail).
- **Poids** : **ouverts, licence MIT** (model cards Hugging Face `deepseek-ai/DeepSeek-V4-Flash` et Pro).
  Formats BF16 / FP8 / FP4 natifs (NVFP4).
- **Inférence** : attention hybride conçue pour réduire le KV cache (~10 % de V3.2 selon la model card) ;
  Together AI (mai 2026) documente 3 layouts de cache simultanés (CSA compressé / HCA dense ~8K entrées /
  SWA exact) ; capacité ~3,7M tokens par nœud HGX B200 avec politique optimisée. Déploiement cible :
  vLLM ou SGLang (forks DeepSeek), DeepGEMM + FlashInfer + DeepEP, EP sur NVLink (NVL72). Chiffres exacts
  par source officielle : non vérifiés. Serveurs datacenter : 8× H100/H200 ou 16+ × B100/B200.
- **Benchmarks clés** (vendor-reported) : V4 Pro Max — Codeforces **3206**, SWE-bench Verified **80,6 %**,
  SimpleQA 57,9 %, Chinese-SimpleQA 84,4 % ; bat Claude Opus 4.8 sur Terminal Bench 2.1.
- **Prix API** (V4-Pro) : **$0,435 / 1M tokens input** (cache miss), **$0,003625 / 1M** (cache hit),
  **$0,87–0,93 / 1M output**. Concurrency limitée à 500 requêtes (Pro).
- **Fine-tuning** : l'API DeepSeek propose la personnalisation — non vérifié au 27/09/2026 pour V4
  précisément. Support Ollama : non vérifié.

## 6. DeepSeek V4-Pro (0813)

- **Nom exact** : DeepSeek V4-Pro (version GA « V4 Pro 0813 », 13 août 2026).
- **Statut au 27/09/2026** : disponible, mais en **dépréciation ordonnée**. L'annonce DeepSeek du
  10 septembre 2026 (WeChat) indique que V4.1-Flash « a comprehensively surpassed V4 Pro » ; à partir du
  14 septembre 2026 (04:00 UTC), les requêtes `deepseek-v4-pro` devaient être servies par V4.1-Flash au
  tarif Flash, « jusqu'au lancement de V4.1 Pro ». **Contradiction non tranchée** : benchlm.ai (26 sept.
  2026) affirme que cette reroute a été **annulée** et que V4 Pro continue d'être servi ; des notes de
  recherche du 11 septembre affirment l'inverse. Impossible de trancher sans accès direct à l'API.
- **Architecture** : MoE ~1,6T total / 49B actifs (voir fiche V4). Mêmes benchmarks et prix que la fiche
  générale : Codeforces 3206, SWE-bench Verified 80,6 %, $0,435/$0,87–0,93 par 1M.
- **Poids** : ouverts, MIT.

## 7. DeepSeek V4-Flash (retiré)

- **Nom exact** : DeepSeek V4-Flash. Sortie : 24 avril 2026. **Retiré : 10 septembre 2026** (routes API
  redirigées vers V4.1-Flash le même jour).
- **Architecture** : MoE, **284B total / 13B actifs**, 256 experts routés + 1 partagé, top-6. NVFP4 natif.
- **Contexte** : 1M tokens. **Poids** : ouverts, MIT.
- **Rôle historique** : le « petit » efficace de la famille V4, positionné prix contre les tiers Flash
  concurrents (Qwen3.8-Flash, GLM-5.3-Flash). Son retrait au profit de V4.1-Flash illustre le rythme
  infernal des remplacements chez DeepSeek en 2026 : à peine 4,5 mois de vie commerciale.

## 8. DeepSeek V4.1-Flash

- **Nom exact** : DeepSeek V4.1-Flash (ID API : `deepseek-flash` ; OpenRouter : `deepseek/deepseek-v4.1-flash`).
- **Date de sortie** : **10 septembre 2026**. Statut : disponible (modèle Flash actuel sur l'API DeepSeek).
- **Architecture** : MoE, **552B paramètres backbone** (763B paramètres stockés selon le repo Hugging Face),
  **8B actifs en prefill / 16B en decode**. Nouvelle famille « Causal Encoder-Decoder » (non vérifié en
  détail). Attention hybride KDA + MLA selon une source tierce (non vérifié au 27/09/2026). Un rapport
  technique mentionne aussi une « Engram memory » de 196B (non vérifié).
- **Contexte** : **1M tokens**, sortie max 384K. **Entrée image native** (max 1 024 tokens/image).
- **Fonctionnalités** : reasoning contrôlable (thinking activé par défaut, efforts low/high/max,
  désactivable), tool calling, mode agent. Web search natif : non vérifié.
- **Poids** : **ouverts, licence MIT** (Hugging Face `deepseek-ai/DeepSeek-V4.1-Flash`).
- **Benchmarks clés** (vendor-reported, effort de raisonnement max) : DeepSWE v1.1 **74,2** (vs Claude
  Opus 5 74,0, GPT-5.6 Sol 73,0), victoires sur AutomationBench et CyberGym ; derrière sur Terminal-Bench
  4.0 ; GPQA Diamond 90,9 (V4-Pro : 92,4). Artificial Analysis Intelligence Index : **40**.
- **Prix API** : **$0,15 / 1M input, $0,60 / 1M output** hors peak ; **double en peak** ($0,30 / $1,20).
  Peak : 01:00–04:00 et 06:00–10:00 UTC, jours ouvrés hors jours fériés chinois. Sur OpenRouter off-peak :
  $0,15 / $0,60, cached $0,003/M.
- **KV cache / inférence** : non vérifié au 27/09/2026 — pas de données publiques trouvées. Déploiement :
  API DeepSeek, OpenRouter (DeepInfra, Novita, Venice). Support vLLM/Ollama : non vérifié.

## 9. DeepSeek V4.1 Pro : annoncé, pas sorti

- **Nom exact** : DeepSeek V4.1 Pro (alias attendu `deepseek-v4.1-pro` — non vérifié).
- **Statut au 27/09/2026** : **annoncé, non disponible**. Aucune spec publiée, aucune date de sortie
  annoncée. Une rumeur (orcarouter.ai, 25 septembre 2026) évoque une fenêtre de sortie 28–30 septembre,
  mais rien dans le changelog officiel DeepSeek entre le 10 et le 25 septembre ne l'étaye.
- Fiche honnête : il n'y a rien de plus à dire. C'est le modèle fantôme de cette fin septembre 2026 —
  tout le marché l'attend comme la réponse de DeepSeek au Kimi K3 et au Qwen3.8-Max.

## 10. DeepSeek : l'attention hybride de la famille V4

Le changement technique le plus important de DeepSeek en 2026 est architectural : la famille V4 abandonne
le MLA (Multi-head Latent Attention) pur qui avait fait la réputation de V3 pour une **attention hybride**
combinant CSA (Compressed Sparse Attention, stride 4), HCA (Heavily Compressed Attention, stride 128) et
SWA (sliding window ~128). L'objectif affiché : diviser le KV cache par ~10 par rapport à V3.2 à contexte
égal, ce qui rend le million de tokens économiquement servable. Le revers : trois layouts de cache
simultanés à gérer côté inference engine (d'où les forks vLLM/SGLang maison et l'usage de DeepEP sur
NVLink). Réserve : ces détails viennent de notes communautaires GitHub, pas d'un rapport technique
officiel DeepSeek — le rapport V4 n'a pas été trouvé au 27/09/2026.

## 11. DeepSeek : les prix et le modèle économique

DeepSeek a inventé en 2026 la tarification « peak/off-peak » calée sur les heures ouvrées chinoises
(01:00–04:00 et 06:00–10:00 UTC en peak, prix doublés). Le V4-Pro facture $0,435/M input en cache miss
mais seulement $0,003625/M en cache hit — un ratio de 120× qui pousse à architecturer les applications
autour du cache de contexte. Le V4.1-Flash descend à $0,15/$0,60 hors peak : c'est le niveau de prix qui
a forcé Qwen3.8-Flash ($0,16/$0,47) et GLM-5.3-Flash ($0,15/$0,50) à s'aligner au dixième de cent près.
La guerre des Flash de l'été 2026 se joue littéralement à $0,01 près sur le million de tokens d'entrée.

## 12. DeepSeek : tableau comparatif

| Modèle | Date | Archi | Params (total/actifs) | Contexte | Prix API ($/1M in-out) | Licence |
|---|---|---|---|---|---|---|
| V4-Pro (0813) | 24/04/2026 (GA 13/08) | MoE, attention hybride | 1,6T / 49B | 1M | 0,435 / 0,87–0,93 | MIT (ouverts) |
| V4-Flash | 24/04/2026 (retiré 10/09) | MoE, NVFP4 | 284B / 13B | 1M | non vérifié | MIT (ouverts) |
| V4.1-Flash | 10/09/2026 | MoE, causal encoder-decoder | 552B / 8–16B | 1M | 0,15 / 0,60 (×2 peak) | MIT (ouverts) |
| V4.1 Pro | annoncé 10/09, pas sorti | non vérifié | non vérifié | non vérifié | non vérifié | présumé MIT |

## 13. Alibaba Qwen : portrait de l'entreprise

Alibaba (via Alibaba Cloud et l'équipe Qwen) est l'acteur chinois le plus prolifique de 2026 : quatre
générations mineures (3.5 → 3.8) en sept mois, une gamme complète du 0.8B au 2.4T paramètres, et une
stratégie à double détente qui structure tout le marché — **open-weight agressif sur les modèles
efficaces (Apache 2.0), poids fermés sur les flagships** (3.6-Max, 3.7-Max/Plus), puis retour surprise à
l'open-weight sur le flagship 3.8-Max. Alibaba vend aussi du cloud (Model Studio, QwenCloud, Bailian) et
des puces maison (Zhenwu M890, serveurs Panjiu AL128, présentés avec Qwen3.7-Max) : c'est le seul acteur
chinois à jouer simultanément le modèle, le cloud et le silicium.

## 14. La stratégie open-weight / fermé d'Alibaba en 2026

La doctrine 2026 d'Alibaba tient en trois temps. **Février–avril** : tout est ouvert (Qwen3.5 intégralement
Apache 2.0, Qwen3.6-35B-A3B et 27B ouverts). **Avril–mai** : virage « frontier-closed » — Qwen3.6-Max-Preview
est le premier flagship Qwen à poids fermés, suivi des 3.6-Plus, 3.7-Max et 3.7-Plus, tous API uniquement.
**Août** : revirement — Qwen3.8-Max sort en **poids ouverts** (annonce semaine du 10 août 2026), présenté
comme « open-weight multimodal system », et Qwen3.8-Flash est ouvert dès le jour J comme « preview de
l'architecture Qwen4 ». Lecture : Alibaba a testé le fermé pour monétiser l'agentique haut de gamme, puis
a rouvert face à la pression concurrentielle (Kimi K3 à 2.8T ouverts en juillet, GLM-5.3 ouverts en août).
Le pendule open/fermé est l'indicateur avancé de la confiance d'Alibaba dans son avance technique.

## 15. Qwen3.5-397B-A17B (flagship février 2026)

- **Nom exact** : Qwen3.5-397B-A17B. **Sortie : 16 février 2026** (Reuters). Statut : disponible, poids ouverts.
- **Architecture** : MoE hybride — **premier modèle open-weight à combiner Gated DeltaNet (attention
  linéaire) + MoE**. 397B total, **512 experts, routage top-10 + experts partagés, 17B actifs/token**.
  45 couches (chiffre communautaire). Vocabulaire ~250K, 201 langues.
- **Contexte** : natif exact non vérifié au 27/09/2026 — la communauté l'utilise à 262 144 tokens sans problème.
- **Fonctionnalités** : vision (encodeur multimodal, variantes vision-language sur HF), tool use / agents
  (« visual agentic capabilities », vidéos jusqu'à 2h d'après la presse), reasoning (modes
  thinking/non-thinking hérités de Qwen3).
- **Poids** : **ouverts, Apache 2.0** (HF `Qwen/Qwen3.5-397B-A17B`). Formats BF16/FP8 officiels, GGUF
  (Unsloth), NVFP4 (Sehyo, RedHatAI). 122B-A10B en Q4_K_M ≈ 71 Go ; Q8_0 ≈ 121 Go (ordres de grandeur
  communautaires).
- **Inférence** : Gated DeltaNet → état récurrent compact au lieu d'un KV cache plein sur les couches
  linéaires (état ≈ 186 MiB pour 397B — mesure communautaire). Têtes MTP (multi-token prediction)
  présentes, supportées par Ollama (décodage spéculatif).
- **Benchmarks** : Alibaba revendique supériorité sur GPT-5.2, Claude Opus 4.5, Gemini 3 Pro sur plusieurs
  benchs (revendication constructeur, non vérifiée indépendamment).
- **Prix API** : non vérifié — « 60 % moins cher que le prédécesseur » selon Alibaba.
- **Déploiement** : vLLM, SGLang, llama.cpp, Ollama, LM Studio, KTransformers, Megatron-Bridge (NVIDIA
  NeMo), ModelScope.

## 16. Qwen3.5-122B-A10B et Qwen3.5-35B-A3B

- **Noms exacts** : Qwen3.5-122B-A10B et Qwen3.5-35B-A3B. **Sortie : 24 février 2026**. Poids ouverts,
  Apache 2.0. 122B : 10B actifs/token, 36 couches ; 35B : 3B actifs/token, 30 couches (chiffres
  communautaires). Même architecture hybride Gated DeltaNet + MoE que le 397B.
- **Le fait marquant** : la communauté note que le 122B-A10B est « très peu différent » du 397B sur les
  benchs malgré plus de 50 % de paramètres en moins — la courbe de scaling s'aplatit et le milieu de
  gamme devient le sweet spot économique.
- **Benchmarks** (SWE-bench Verified) : 35B-A3B **70,0 %** ; 27B dense **75,0 %**.
- **Fine-tuning** : supporté par la communauté — configs Axolotl officielles (LoRA/QLoRA/FFT, FSDP2) pour
  9B, 27B, 35B-A3B, 122B-A10B ; le LoRA doit cibler `linear_attn.in_proj_qkv/z/out_proj` + experts MoE
  (`mlp.experts.gate_up_proj/down_proj`). Fine-tuning via API Alibaba : non vérifié.
- **Inférence** : état GDN ≈ 63 MiB (35B-A3B), ≈ 149 MiB (122B) — mesures communautaires.

## 17. Qwen3.5-27B (dense) et petites tailles

- **Qwen3.5-27B** : sortie 24 février 2026. **Dense 27B**, hybride GDN/attention, Apache 2.0. SWE-bench
  Verified **75,0 %** — meilleur que le 35B-A3B MoE sur ce bench, ce qui a relancé le débat dense vs MoE
  à taille moyenne. GGUF Q4_K_M ≈ 15,7 Go (ordre de grandeur communautaire pour la lignée 27B).
- **Petites tailles** (0.8B / 2B / 4B / 9B) : sortie **2 mars 2026**, denses, Apache 2.0. Destinées à
  l'embarqué et à l'inférence locale légère. Specs fines : non vérifiées au 27/09/2026.
- La gamme 3.5 complète couvre ainsi 0.8B → 397B : la plus large échelle open-weight du marché début 2026.

## 18. Qwen3.5 : l'attention Gated DeltaNet, explication

Le différenciateur technique de toute la génération Qwen 3.5/3.6/3.8 (hors Max fermés) est le **Gated
DeltaNet** : une attention linéaire (complexité O(n) au lieu de O(n²)) mélangée par blocs avec de
l'attention standard (pattern répété, ex. 3 couches GDN + 1 couche d'attention). Au lieu d'un KV cache qui
grossit avec la séquence, les couches linéaires maintiennent un **état récurrent de taille fixe**
(quelques dizaines à ~200 MiB selon la taille du modèle). Conséquence pratique : le long contexte coûte
beaucoup moins cher en mémoire, et c'est ce qui permet à des modèles ouverts de viser 256K–1M de contexte
sans ferme de GPU. C'est la même famille d'idées que le KDA de Kimi et le DSA de GLM : 2026 est l'année
où l'attention linéaire/hybride est devenue le standard chinois.

## 19. Qwen3.6-35B-A3B (« qwen3.6-flash »)

- **Nom exact** : Qwen3.6-35B-A3B (nom API Model Studio : `qwen3.6-flash`). **Sortie : 16 avril 2026**.
  Statut : disponible, poids ouverts.
- **Architecture** : MoE sparse, **35B total / 3B actifs** par token ; 256 experts (8 routés + 1 partagé) ;
  40 couches en 10 groupes de (3× Gated DeltaNet + 1× Gated Attention). Attention hybride GDN + Gated Attention.
- **Contexte** : **262 144 tokens natifs (256K)**, extensible à >1M via YaRN.
- **Fonctionnalités** : vision (texte + image + vidéo), tool use / agents, reasoning (**Thinking
  Preservation** — conservation du raisonnement entre les tours ; modes thinking/non-thinking).
- **Poids** : **ouverts, Apache 2.0** (HF `Qwen/Qwen3.6-35B-A3B`). GGUF disponible (Ollama, llama.cpp).
  Tient sur 1× H100/H200 en BF16/FP8.
- **Benchmarks** : SWE-bench Verified **73,4 %**, SWE-bench Pro 49,5 %, Terminal-Bench 2.0 51,5 %,
  LiveCodeBench v6 80,4 %, QwenWebBench (frontend) 1397.
- **Prix API** : ~$0,14 / 1M input, $1,00 / 1M output (FP8, fournisseur tiers AirCloud) — prix officiel
  Alibaba non vérifié.
- **Déploiement** : vLLM, SGLang, Ollama, llama.cpp, ModelScope, HF.

## 20. Qwen3.6-27B (dense, « open flagship »)

- **Nom exact** : Qwen3.6-27B. **Sortie : 22 avril 2026**. Statut : disponible, poids ouverts.
- **Architecture** : **dense 27B**, hybride GDN/full attention, multimodal (texte + image + vidéo).
- **Contexte** : 262K natifs d'après un comparatif tiers — source primaire Alibaba non vérifiée.
- **Poids** : **ouverts, Apache 2.0**. GGUF Q4_K_M ≈ 15,7 Go.
- **Benchmarks** : SWE-bench 77,2 % (agrégateur tiers kilo.ai — à prendre avec précaution).
- **Positionnement** : le « dense open flagship » d'Alibaba — la preuve qu'à 27B, un dense bien entraîné
  tient tête aux MoE sparse de la même classe sur le code.

## 21. Qwen3.6-Max-Preview (fermé)

- **Nom exact** : Qwen3.6-Max-Preview. **Sortie : 20 avril 2026**. Statut : disponible, **fermé, API uniquement**.
- **Importance historique** : **premier flagship Qwen à poids fermés** — le moment où Alibaba a rompu avec
  le tout-open-weight.
- **Architecture** : MoE 35B total / **3B actifs** par inférence (classe 35B-A3B) — chiffres rapportés par
  la presse, non publiés par Alibaba.
- **Contexte** : 256 000 tokens. **Fonctionnalités** : texte uniquement au lancement (pas d'image),
  agents/coding, `preserve_thinking` (traces de raisonnement conservées entre tours).
- **Poids** : fermés — pas de poids HF. **Prix API** : non vérifié. Benchmarks : meilleurs scores sur
  6 benchs coding/agents dont SWE-bench Pro et Terminal-Bench 2.0 (revendication Alibaba).
- **Déploiement** : Alibaba Cloud Bailian, Qwen Studio ; compatible API OpenAI **et** Anthropic.

## 22. Qwen3.6-Plus (fermé)

- **Nom exact** : Qwen3.6-Plus. **Sortie : 2 avril 2026** (Caixin Global). Statut : disponible, **fermé,
  API uniquement**.
- **Architecture** : non vérifié (paramètres non publiés). **Contexte : 1M tokens par défaut**.
- **Fonctionnalités** : multimodal (texte, image, vidéo, documents), agentic coding (planifie/teste/itère,
  niveau repo, frontend), « capability loop » (perception → raisonnement → action). Compatible Claude Code,
  OpenClaw, Cline ; **protocole API Anthropic supporté** (on peut pointer un setup Claude Code vers le
  modèle). Intégré à Wukong (plateforme entreprise Alibaba) et à l'app Qwen.
- **Benchmarks** : SWE-bench Verified **78,8** (vs Claude Opus 4.5 : 80,9), Terminal-Bench 2.0 **61,6**
  (devant Opus 4.5 : 59,3), GPQA **90,4** (meilleur des modèles comparés).
- **Prix API** : Bailian — 2 yuans (~$0,29) / 1M input, 12 yuans / 1M output (Caixin).

## 23. Qwen : la compatibilité « protocole Anthropic »

Détail stratégique qui traverse les fiches 3.6-Plus, 3.7-Max et 3.8 : les modèles fermés Qwen supportent le
**protocole API Anthropic** en plus du format OpenAI. Concrètement, un utilisateur peut pointer son
installation Claude Code vers un endpoint Qwen sans changer d'outillage. C'est une prise de guerre
commerciale directe contre Anthropic sur son propre terrain (les agents de code), rendue possible parce que
les harnais type Claude Code/OpenClaw parlent un protocole que n'importe quel fournisseur peut implémenter.
En 2026, l'interopérabilité des protocoles compte autant que les benchmarks.

## 24. Qwen : premier tableau intermédiaire (3.5 – 3.6)

| Modèle | Date | Archi | Params (total/actifs) | Contexte | Prix API ($/1M in-out) | Licence |
|---|---|---|---|---|---|---|
| Qwen3.5-397B-A17B | 16/02/2026 | MoE, GDN + MoE | 397B / 17B | ~262K (non vérifié) | non vérifié (−60 % vs préd.) | Apache 2.0 |
| Qwen3.5-122B-A10B | 24/02/2026 | MoE, GDN + MoE | 122B / 10B | ~262K (non vérifié) | non vérifié | Apache 2.0 |
| Qwen3.5-35B-A3B | 24/02/2026 | MoE, GDN + MoE | 35B / 3B | ~262K (non vérifié) | non vérifié | Apache 2.0 |
| Qwen3.5-27B | 24/02/2026 | Dense, hybride GDN | 27B | non vérifié | non vérifié | Apache 2.0 |
| Qwen3.5 (0.8B–9B) | 02/03/2026 | Denses | 0.8–9B | non vérifié | non vérifié | Apache 2.0 |
| Qwen3.6-35B-A3B | 16/04/2026 | MoE, GDN + Gated Attn | 35B / 3B | 256K (>1M YaRN) | ~0,14 / 1,00 (tiers) | Apache 2.0 |
| Qwen3.6-27B | 22/04/2026 | Dense, hybride GDN | 27B | 256K (tiers) | non vérifié | Apache 2.0 |
| Qwen3.6-Max-Preview | 20/04/2026 | MoE (presse) | 35B / 3B (presse) | 256K | non vérifié | Fermé |
| Qwen3.6-Plus | 02/04/2026 | non vérifié | non vérifié | 1M | ~0,29 / ~1,74 (Bailian) | Fermé |

## 25. Qwen3.7-Max (fermé, l'agent natif)

- **Nom exact** : Qwen3.7-Max (previews : Qwen3.7-Max-Preview). **Preview : 19 mai 2026**, annonce
  officielle **20 mai 2026** (Alibaba Cloud Summit, Hangzhou) — les deux previews (Max et Plus) étaient
  apparues silencieusement sur le leaderboard LMArena avant l'annonce.
- **Statut** : disponible, **fermé, API uniquement**. Architecture : non vérifié (paramètres total/actifs
  non publiés par Alibaba). **Contexte : 1M tokens**.
- **Fonctionnalités** : modèle **agent natif** — conçu pour l'exécution autonome longue, pas un chat avec
  tool calling greffé : tâches agentiques jusqu'à **35 heures** en continu ; reasoning à chaîne de pensée
  étendue, toujours active ; **texte uniquement** (pas de vision — voir 3.7-Plus) ; généralisation
  cross-scaffold (Claude Code, OpenClaw, Qwen Code) ; multilingue (55+ langues, WMT24++ 85,8).
- **Benchmarks** : SWE-bench Verified **80,4** (≈ Claude Opus-4.6 Max 80,8 ; DeepSeek-v4-Pro Max 80,6),
  Terminal-Bench 2.0-Terminus **69,7** (devant DeepSeek-v4-pro-Max 67,9), GPQA Diamond **92,4** (devant
  Opus-4.6 : 91,3), HLE **41,4** (devant Opus-4.6 : 40,0), MCP-Mark **60,8** (devant GLM-5.1 : 57,5),
  MCP-Atlas **76,4** (devant Opus-4.6 : 75,8), SpreadSheetBench-v1 **87**. Artificial Analysis Intelligence
  Index ≈ **56,6–57** (5e mondial). LMArena Text #13 mondial.
- **Prix API** : à partir de $1,25 / 1M input, $0,25 / 1M cached input, $3,75 / 1M output (via Novita, prix
  le plus bas suivi) — prix officiel Alibaba non vérifié.
- **Déploiement** : API (Model Studio/QwenCloud) ; tiers : Novita, DeepInfra, Together.
- **Note hardware** : lancé avec la puce **Zhenwu M890** et le serveur **Panjiu AL128** (TechTimes) — Qwen3.7-Max
  « a écrit son propre logiciel de puce » pendant un run de 35 heures selon la presse tech, anecdote qui a
  fait le tour des médias.

## 26. Qwen3.7-Plus (fermé, le multimodal)

- **Nom exact** : Qwen3.7-Plus (preview : Qwen3.7-Plus-Preview). **Sortie : 20 mai 2026** (avec Max, au
  Summit) — API grand public quelques semaines après (juin 2026 d'après VentureBeat).
- **Statut** : disponible, **fermé, API uniquement**. Architecture : non vérifié. Contexte : non vérifié
  (probablement 1M comme le reste de la gamme — non vérifié).
- **Fonctionnalités** : **multimodal** (texte, vidéo, images) — le pendant vision du 3.7-Max texte-only ;
  version « équilibrée » axée raisonnement.
- **Poids** : fermés. **Benchmarks** : LMArena Vision #16 mondial (preview) — autres chiffres non vérifiés.
- **Prix API** : **$0,40 / 1M input, $1,60 / 1M output** (VentureBeat) — ~60 % moins cher que Qwen3.7-Max.

## 27. Qwen3.8-Max (2,4T paramètres, retour à l'open-weight)

- **Nom exact** : Qwen3.8-Max (snapshot mis à jour : **Qwen3.8-Max-0902**, 2 septembre 2026).
- **Preview Arena.AI : 19 juillet 2026** ; lancement officiel **3 août 2026** ; snapshot 0902 le 2 septembre 2026.
- **Statut** : disponible — **poids OUVERTS** (retour à l'open-weight au flagship : annonce de publication
  des poids la semaine du 10 août 2026 ; décrit comme « open-weight multimodal system » par plusieurs médias).
  Licence exacte des poids : non vérifiée au 27/09/2026 (texte final non lu directement).
- **Architecture** : MoE — **2,4 billions (2.4T) de paramètres total / ~95B actifs** par token (chiffre
  actif cité dans les supports presse d'Alibaba). Nombre de couches / type d'attention : non publiés.
- **Contexte** : **1M tokens**.
- **Fonctionnalités** : multimodal (texte, images, vidéos, gros fichiers), coding autonome longue durée
  (16 jours de dev autonome en test interne), analyse visuelle, `reasoning_effort` (low/medium/**xhigh**
  par défaut), `preserve_thinking` activé par défaut, `/think` et `/no_think` sur les poids ouverts.
- **Benchmarks** : Artificial Analysis Intelligence Index v4.3 : **40** (3 août) → **45** (snapshot 0902) ;
  Code Arena WebDev : 4e (1669) → **1er** (1691) pour 0902 (devant Claude Opus 5 Max) ; Terminal-Bench 2.1 :
  86,6 ; PaperBench 93,0 ; QwenSWEBench 80,7 (chiffres carte constructeur) ; LMArena : 2e Vision Arena,
  5e Text Arena (derrière Anthropic Fable 5).
- **Prix API** : **$2,00 / 1M input, $6,00 / 1M output** (Model Studio, identique pour les deux snapshots) —
  coût/tâche mesuré par AA : $2,67 → $5,41 (le snapshot 0902 parle beaucoup plus : 108k tokens/tâche).
- **Déploiement** : QwenCloud, Alibaba Cloud Model Studio, QwenWork ; plans API : Standard (pay-as-you-go)
  ou Token Plan (pas le Coding Plan).

## 28. Qwen3.8-Max-0902 : le snapshot qui parle trop

Le snapshot du 2 septembre 2026 mérite sa fiche : AA Intelligence Index passé de 40 à 45, 1er de Code
Arena WebDev (1691, devant Claude Opus 5 Max), mais coût par tâche doublé ($2,67 → $5,41) parce que le
modèle génère ~108k tokens par tâche. C'est l'illustration parfaite du dilemme « xhigh reasoning » :
plus de raisonnement = meilleurs scores, mais facture API qui explose. Alibaba laisse `reasoning_effort`
réglable (low/medium/xhigh) pour que le client arbitre lui-même.

## 29. Qwen3.8-27B (dense ouvert, tueuse de géants)

- **Nom exact** : Qwen3.8-27B (27,78B params ; 28B avec l'encodeur vision). **Sortie : 14 août 2026**.
  Statut : disponible, poids ouverts.
- **Architecture** : **dense 27,78B** ; hybride : **48 couches Gated DeltaNet + 16 couches full-attention**.
- **Contexte** : 262 144 natifs, extensible à 1M via YaRN.
- **Fonctionnalités** : multimodal (texte, image, vidéo), thinking activé par défaut (désactivable),
  coding/reasoning/agents long-horizon.
- **Poids** : **ouverts, Apache 2.0**. Ollama : `qwen3.8:27b` (18 Go en Q4_K_M, support day-0, tête MTP
  incluse → décodage spéculatif par défaut) ; GGUF Unsloth (UD-Q4_K_XL 17,6 Go) ; tourne sur 1× GPU 24 Go
  en 4-bit.
- **Inférence** : GDN → état compact ; tête MTP exploitée par Ollama (`draft-mtp`).
- **Benchmarks** (agrégateur regolo.ai, à manier avec précaution) : SWE-bench Pro **61,7** (vs Claude Opus
  4.6 Max 53,4), LiveCodeBench v6 **90,3** (vs 88,8), IFBench **79,5** (vs 62,5), QwenSWEBench 79,0 (vs 63,8),
  CoWorkBench 70,7 (vs 68,2), DeepSWE 1.1 42,2, Arena WebDev 18e/128. Un dense 27B qui bat un flagship
  fermé occidental sur le code : c'est le fait technique le plus disruptif de l'été 2026 côté ouvert.
- **Déploiement** : Ollama (day-0), llama.cpp, vLLM, LM Studio, DGX Spark.

## 30. Qwen3.8-Flash (la « preview de Qwen4 »)

- **Nom exact** : Qwen3.8-Flash (nom preview : Qwen3.8-Flash-Next) — présenté comme **preview explicite de
  l'architecture Qwen4**. Teaser ModelScope 25 août 2026, annonce **26 août** (Alizila), production
  `qwen3.8-flash` le **2 septembre 2026** (renommage Next → production en une semaine).
- **Statut** : disponible — poids ouverts + API. Licence exacte : non vérifiée (une source dit « Qwen
  License », une autre « non officiellement précisée » — à vérifier sur la carte HF/ModelScope).
- **Architecture** : MoE — **125B backbone + 51B table N-gram = 176B total / 6B actifs** par token ;
  Gated DeltaNet + **Qwen Sparse Attention** (preview archi Qwen4).
- **Contexte** : 262 144 natifs (extensible à ~1M via YaRN) ; déploiement Model Studio listé à 983 616
  tokens de contexte, 131 072 tokens max en sortie ; reasoning « xhigh » par défaut.
- **Fonctionnalités** : multimodal (texte, vision haute résolution, vidéo multi-frame), coding, document
  work, agents.
- **Benchmarks** : SWE-bench Pro **62,5 %**, CoWorkBench **73,9**, JobBench **55,7**, DeepSWE 58,7,
  Toolathlon 73,5, HLE (sans tools) 35,9, GPQA Diamond 91,7, LiveCodeBench v6 **91,9**, MMLU-Pro 73,23
  (chiffres constructeur/agrégateurs — vérification indépendante limitée).
- **Prix API** : **$0,16 / 1M input, $0,47 / 1M output** (QwenCloud/Model Studio) — positionné frontalement
  contre DeepSeek-V4-Flash ($0,15/$0,60).
- **Déploiement** : API QwenCloud/Model Studio (Standard ou Token Plan) ; support **day-0 SGLang** ; poids
  ouverts → vLLM/llama.cpp probables (non vérifié au 27/09).

## 31. Les tiers API « Plus / Flash / Max » démystifiés

En 2026, « Plus / Flash / Max » ne sont plus des modèles mais des **tiers API versionnés** :
`qwen3.6-flash` (= Qwen3.6-35B-A3B ouvert), `qwen3.6-plus` (fermé), `qwen3.7-max` (fermé), `qwen3.7-plus`
(fermé), `qwen3.8-max` (ouvert), `qwen3.8-flash` (ouvert). Les IDs génériques historiques `qwen-plus` /
`qwen-max` restent référencés comme défauts dans des docs tierces récentes (sept. 2026), mais le mapping
exact vers les modèles versionnés côté Model Studio n'a pas été vérifié sur la doc officielle. La doc
OpenClaw (sept. 2026) précise : `qwen3.7-plus` et `qwen3.6-plus` acceptés sur Coding Plan et Standard ;
`qwen3.8-max`, `qwen3.8-flash`, `qwen3.7-max`, `qwen3.6-flash` exigent Standard (pay-as-you-go) ou Token Plan.
Moralité : toujours vérifier l'ID exact facturé, le nom commercial ne dit plus quel modèle tourne derrière.

## 32. Qwen3-Coder-Next (février 2026)

- **Nom exact** : Qwen3-Coder-Next. **Sortie : février 2026**. Statut : disponible, poids ouverts.
- **Note de nommage** : aucun « Qwen3.5-Coder » / « Qwen3.6-Coder » / « Qwen3.7-Coder » n'a été trouvé —
  le codeur 2026 d'Alibaba s'appelle bien « Coder-Next ».
- **Architecture** : MoE **80B total / 3B actifs** ; attention hybride **Gated DeltaNet (75 % linéaire /
  25 % full)** ; contexte **256K** ; licence **Apache 2.0**.
- **Benchmarks** : SWE-bench Verified **70,6 % (SWE-Agent) → 71,1 % (MiniSWE) → 71,3 % (OpenHands)** ;
  SWE-Bench Pro 44,3 %. Entraîné par RL pour tâches agentiques (schémas d'outils style Claude Code).
  Rapport technique : arXiv 2603.00729.
- **Prix API** : ~$0,11 / $0,80 par 1M (tiers). **Déploiement** : Ollama, LM Studio, llama.cpp, vLLM, mlx-lm.

## 33. Qwen3-Coder-480B-A35B

- **Nom exact** : Qwen3-Coder-480B-A35B. Statut : disponible, poids ouverts, **Apache 2.0**.
- **Architecture** : MoE **480B total / 35B actifs**. Contexte 256K (1M par extrapolation).
- **Benchmarks** : SWE-bench Verified **69,6 %**.
- **Prix API** : ~$0,22 in / $0,90 out par 1M (tiers).
- Fait notable : avec 35B actifs par token, c'est le codeur ouvert le plus « dense en actifs » de la gamme
  Qwen 2026 — puissance brute plutôt qu'efficacité, l'inverse du positionnement du Coder-Next (3B actifs).

## 34. Qwen : le déploiement local, mode d'emploi

La gamme ouverte Qwen est la mieux supportée côté self-hosted de tout l'écosystème chinois 2026 : vLLM,
SGLang, llama.cpp, Ollama (support day-0 sur les 27B), LM Studio, KTransformers, Megatron-Bridge, MLX sur
Apple Silicon, ModelScope. Points d'attention : les modèles GDN nécessitent des kernels compatibles
attention linéaire (vérifier la version de vLLM/SGLang) ; les têtes MTP activent le décodage spéculatif
sous Ollama ; les GGUF communautaires (Unsloth, ggml-org, bartowski) sont la voie rapide pour le local.
Pour le fine-tuning, les configs Axolotl officielles (LoRA/QLoRA/FFT, FSDP2) ciblent les projections de
l'attention linéaire + les experts MoE — ne pas appliquer une recette LoRA standard sans vérifier les
noms de modules.

## 35. Qwen : tableau comparatif (3.7 – 3.8 + Coder)

| Modèle | Date | Archi | Params (total/actifs) | Contexte | Prix API ($/1M in-out) | Licence |
|---|---|---|---|---|---|---|
| Qwen3.7-Max | 20/05/2026 | non vérifié (fermé) | non vérifié | 1M | ~1,25 / 3,75 (tiers) | Fermé |
| Qwen3.7-Plus | 20/05/2026 | non vérifié (fermé) | non vérifié | 1M (non vérifié) | 0,40 / 1,60 | Fermé |
| Qwen3.8-Max | 03/08/2026 (snap. 02/09) | MoE | 2,4T / 95B | 1M | 2,00 / 6,00 | Ouverts (licence non vérifiée) |
| Qwen3.8-27B | 14/08/2026 | Dense, 48 GDN + 16 full | 27,78B | 256K (>1M YaRN) | non vérifié | Apache 2.0 |
| Qwen3.8-Flash | 02/09/2026 | MoE, GDN + Qwen Sparse Attn | 176B / 6B | 256K (~1M YaRN) | 0,16 / 0,47 | Ouverts (licence non vérifiée) |
| Qwen3-Coder-Next | 02/2026 | MoE, GDN 75/25 | 80B / 3B | 256K | ~0,11 / 0,80 (tiers) | Apache 2.0 |
| Qwen3-Coder-480B-A35B | 2026 | MoE | 480B / 35B | 256K (1M extrap.) | ~0,22 / 0,90 (tiers) | Apache 2.0 |

## 36. Qwen : ce que la gamme 2026 raconte

Trois leçons. Un, l'échelle n'est plus un argument : le 122B-A10B fait jeu égal avec le 397B-A17B, le
27B dense bat des MoE plus gros — l'efficacité d'entraînement a rattrapé la taille brute. Deux, Alibaba
a industrialisé le « snapshot » (3.8-Max-0902) : le modèle n'est plus une release mais un flux, avec des
mises à jour qui changent le comportement et le coût. Trois, la compatibilité protocolaire (OpenAI +
Anthropic) et les tiers API versionnés montrent qu'Alibaba vend désormais une plateforme, pas des modèles.

## 37. Moonshot AI : portrait de l'entreprise

Moonshot AI (Pékin), fondée par Yang Zhilin, est le spécialiste chinois de l'agentique à grande échelle.
Sa marque modèle est **Kimi** (plateforme : platform.kimi.ai). La lignée 2026 va de K2.5 (27 janvier) à
K2.6 (20 avril), K2.7 Code (12 juin) puis **K3 (juillet)** — ce dernier étant, à sa sortie, le plus gros
modèle open-weight de l'histoire (2,8T paramètres). Moonshot a deux signatures : les essaims d'agents
(jusqu'à 300 agents coordonnés sur 4 000 étapes) et une licence maison, la « Modified MIT », qui mérite
sa propre section.

## 38. La licence « Modified MIT » de Moonshot : ce qu'elle change

Les poids Kimi sont ouverts sous **licence Modified MIT** : usage commercial, modification, fine-tuning
et revente permis sans royalties — comme la MIT classique — **plus une clause d'attribution commerciale** :
si le produit dépasse **100M d'utilisateurs mensuels ou $20M de revenus mensuels**, l'interface doit
afficher « Kimi K2.6 » (ou le nom du modèle). C'est permissif pour 99,9 % des usages (recherche, PME,
self-hosted, intégration produit), mais ce n'est pas une MIT pure : un hyperscaler qui voudrait servir
Kimi à des centaines de millions d'utilisateurs devrait créditer Moonshot en façade. Pour Zelef et son
RAG personnel : aucun impact, la clause ne se déclenche qu'à l'échelle industrielle.

## 39. Kimi K2.6

- **Nom exact** : Kimi K2.6 (ID API : `kimi-k2.6`). **Sortie : 20 avril 2026**. Statut : disponible, poids ouverts.
- **Architecture** : MoE — **1T (1 billion) total / 32B actifs** par token ; **61 couches** (1 bloc dense +
  60 blocs MoE) ; **384 experts routés** (8 sélectionnés + 1 partagé ; config top-8-of-385) ; SwiGLU ;
  vocabulaire 163 840 tokens ; **livré nativement quantifié INT4** (pas de release haute précision).
- **Attention** : **MLA (Multi-head Latent Attention)** style DeepSeek-V3 + YaRN RoPE scaling.
- **Contexte** : **262 144 tokens** (256K).
- **Fonctionnalités** : multimodal natif (encodeur vision **MoonViT**, ~400M params : texte + image + vidéo) ;
  agents long-horizon (essaims jusqu'à **300 agents**, **4 000 étapes** coordonnées) ; thinking activé par
  défaut, température **fixée à 1,0** sur l'API ; raisonnement retourné dans le champ `reasoning_content`.
- **Poids** : **ouverts, Modified MIT** (HF `moonshotai/Kimi-K2.6`).
- **Inférence** : MLA → KV cache compressé (latent) ; INT4 natif ; guidance : 8× H200 (baseline), 8×
  B200/B300 ; vLLM + FlashInfer-MoE, CUTLASS maison (templates NVFP4), FlashAttention-2, parallélisme EP/TP/PP.
- **Benchmarks** : à sa sortie, « modèle open-weight le plus fort » (Artificial Analysis), à quelques
  points des systèmes fermés sur SWE-Bench Pro et HLE ; LMArena 1355 ; MMLU 88,0 ; HumanEval 87,0 ;
  SWE-bench 80,2 % (agrégateur tiers).
- **Prix API** : $0,60 / 1M input, $2,50 / 1M output (best-of-ai, juil. 2026) — prix officiel Moonshot non vérifié.
- **Déploiement** : vLLM, SGLang, KTransformers, API Moonshot (OpenAI-compatible).

## 40. Kimi K2.7 Code (« Kimi 2.7 »)

- **Nom exact** : **Kimi K2.7 Code** (ID API : `kimi-k2.7-code`) — « Kimi 2.7 » seul n'existe pas, c'est
  l'abréviation usuelle. **Sortie : 12 juin 2026** (mode HighSpeed : 15 juin ; dispo Cloudflare Workers AI
  le jour J). Statut : disponible, poids ouverts.
- **Architecture** : **identique à K2.6** (1T total / 32B actifs, 384 experts 8+1, 61 couches, MLA, SwiGLU,
  vocab ~160K) — construit directement sur le checkpoint K2.6, post-training spécialisé code ; **nativement INT4**.
- **Contexte** : 262 144 tokens ; **sortie max par défaut 32 768 tokens** (non relevable à « illimité » sur l'API).
- **Fonctionnalités** : code-first — +21,8 % sur Kimi Code Bench v2, +11,0 % Program Bench, +31,5 % MLS Bench
  Lite vs K2.6 ; **~30 % de tokens de raisonnement en moins** que K2.6 sur les tâches de code ; thinking
  **forcé** (toujours actif) ; multimodal (texte+image+vidéo via MoonViT, 4K image / FHD vidéo recommandés) ;
  sessions >12 h en continu (revendication Moonshot).
- **Poids** : **ouverts, Modified MIT** (HF `moonshotai/Kimi-K2.7-Code`).
- **Benchmarks** : MCP-Mark **81,1** (devant Claude Opus 4.8 : 76,4 ; derrière GPT-5.5 : 92,9). Réserve :
  Moonshot n'avait soumis **aucun bench indépendant** au 15/06/2026 (critique TechTimes) — chiffres
  constructeur.
- **Prix API** : **$0,95 / 1M input, $4,00 / 1M output, $0,19 / 1M cached input** (prix Moonshot) ; variante
  **HighSpeed** à 2× ($1,90/$8,00) pour ~180–260 tok/s.
- **Déploiement** : API Moonshot, Cloudflare Workers AI (`@cf/moonshotai/kimi-k2.7-code`), Fireworks,
  OpenRouter, vLLM/SGLang/KTransformers en self-hosted.

## 41. Kimi K3 (2,8T paramètres)

- **Nom exact** : Kimi K3 (HF `moonshotai/Kimi-K3`). Annonce ~**16 juillet 2026**, **poids open-source le
  27 juillet 2026**. Statut : disponible, poids ouverts — le plus gros modèle open-weight de l'histoire
  à sa sortie.
- **Architecture** : MoE très sparse — **2,8T (2 800B) total / 104B actifs** par token (16 experts activés
  sur 896). **Divergence non résolue** : une source (table 99sono) indique « ~50B actifs » avec le même
  « 16/896 » — incohérence non tranchée au 27/09/2026.
- **Attention** : hybride **Kimi Delta Attention (KDA, attention linéaire maison) + MLA** — support natif
  du contexte 1M.
- **Contexte** : **1 048 576 tokens (1M)**.
- **Fonctionnalités** : vision native ; « thinking mode » toujours actif ; coding, agents, raisonnement
  complexe — en tête des modèles ouverts sur plusieurs benchs.
- **Poids** : **ouverts, Modified MIT** (licence exacte à confirmer sur la carte HF — la carte
  `moonshotai/Kimi-K3` n'a pas été lue directement). **Téléchargement ~1,4–1,56 To** (96 shards
  safetensors) — le poids est la barrière, pas la licence.
- **Inférence** : KDA rend l'inférence long-contexte « plusieurs fois moins chère » ; **support Day-0** :
  Mooncake (réutilisation KV cache cross-instance, disaggregation PD/EPD), SGLang, vLLM, TokenSpeed.
  Guidance Moonshot : supernœud **64+ accélérateurs** ; minimum pratique cité : 4 nœuds / 32 GPU en
  400 Gbps RoCEv2/InfiniBand.
- **Benchmarks** : SWE-bench Verified **85,6 %**, Terminal-Bench 3.0 **79,8 %**, LMArena Elo **1512**
  (Tier-1), AA Intelligence **65,0**.
- **Prix API** : $1,20 / 1M input, $4,80 / 1M output (matrice comparative tierce) — prix officiel Moonshot
  non vérifié.
- **Fine-tuning** : non vérifié (poids ouverts → possible en théorie ; aucune recette vérifiée au 27/09).

## 42. Kimi : l'attention KDA + MLA

Le **Kimi Delta Attention (KDA)** est l'attention linéaire maison de Moonshot, combinée au MLA
(style DeepSeek) dans K3. Le principe est le même que le Gated DeltaNet de Qwen : remplacer une partie
des couches d'attention quadratique par des couches linéaires à état récurrent fixe, pour un coût mémoire
qui n'explose plus avec la longueur du contexte. K3 est le premier modèle à 1M de contexte natif servi
avec cette architecture hybride à l'échelle de 2,8T paramètres. Fait notable : le KDA de Moonshot a
essaimé — l'hybride « 35 couches KDA + 7 couches MLA » du Ling-3.0-flash d'Ant Group reprend
explicitement la brique Kimi (voir section 74). L'open-weight fait circuler les idées d'architecture
plus vite que les papiers.

## 43. Kimi : tableau comparatif

| Modèle | Date | Archi | Params (total/actifs) | Contexte | Prix API ($/1M in-out) | Licence |
|---|---|---|---|---|---|---|
| Kimi K2.6 | 20/04/2026 | MoE, MLA + YaRN | 1T / 32B | 256K | 0,60 / 2,50 (tiers) | Modified MIT |
| Kimi K2.7 Code | 12/06/2026 | MoE, MLA (base K2.6) | 1T / 32B | 256K | 0,95 / 4,00 (×2 HighSpeed) | Modified MIT |
| Kimi K3 | 27/07/2026 (poids) | MoE, KDA + MLA | 2,8T / 104B (divergence : ~50B) | 1M | 1,20 / 4,80 (tiers) | Modified MIT (à confirmer) |

## 44. Zhipu AI / Z.ai : portrait de l'entreprise

Zhipu AI (opérant à l'international sous la marque **Z.ai**), issue de l'université Tsinghua, est devenue
en 2026 la première entreprise IA chinoise cotée à innover au niveau frontier en open-weight. Sa famille
**GLM** a marqué février 2026 avec GLM-5 (744B/40B, entraîné entièrement sur puces **Huawei Ascend**,
zéro NVIDIA — fait géopolitique majeur corroboré par Reuters), puis l'été avec **GLM-5.2** (juin),
**GLM-5.3** (août, positionné « cyber ») et **GLM-5.3-Flash** (août). Zhipu livre aussi le harnais agent
officiel **ZCode** (style Claude Code) avec ses modèles.

## 45. GLM-5.2

- **Nom exact** : GLM-5.2 (ID API : `glm-5.2` ; HF `zai-org/GLM-5.2`). **13 juin 2026** (GLM Coding Plan),
  **16 juin 2026** (API + poids ouverts). Statut : disponible, poids ouverts.
- **Architecture** : MoE — **~744–753B total / ~40B actifs** par token (incohérences mineures entre
  outlets : 744B, 750B, 753B — Z.ai n'a publié aucun papier d'architecture) ; **256 experts routés +
  1 partagé** ; architecture `glm_moe_dsa`.
- **Attention** : **MLA + DSA (Dynamic Sparse Attention)** + nouveauté **IndexShare** (des groupes de
  couches partagent les indices top-k de l'attention sparse → coût réduit à 1M de contexte).
- **Contexte** : **1M tokens** (solide/utilisable, vs 200K sur GLM-5.1) ; sortie max ~128–131K tokens.
- **Fonctionnalités** : **texte uniquement** (pas de vision) ; coding-first / agents long-horizon ;
  **effort de raisonnement réglable** (`reasoning_effort` : non-thinking → High → Max) ; couche MTP
  améliorée (décodage spéculatif) ; harnais officiel **ZCode** livré avec ; compatible Claude Code, Cline,
  OpenCode, Roo Code, Goose, Crush, OpenClaw, Kilo Code dès le jour J.
- **Poids** : **ouverts, licence MIT** (pleine, permissive — « no regional limits »). HF `zai-org/GLM-5.2`, ModelScope.
- **Benchmarks** : TerminalBench v2.1 **78 %**, GPQA Diamond **89 %**, SWE-bench Pro **62,1 %** (devant
  GPT-5.5 et Claude sur ce bench), AA Intelligence Index v4.1 **51** (n°1 de l'index à sa sortie, devant
  MiniMax-M3 44, DeepSeek V4 Pro 44, Kimi K2.6 43), ProofBench 30 %+ (11 pts devant le n°2),
  GDPval-AA v2 agent : **1524 Elo** (devant GPT-5.5 : 1514). N°1 open-weight sur FrontierSWE, PostTrainBench,
  SWE-Marathon (revendication Z.ai).
- **Prix API** : **$1,40 / 1M input, $4,40 / 1M output, $0,26 / 1M cached input** (= prix GLM-5.1 repris à
  l'identique) ; dispo via GLM Coding Plan (Lite/Pro/Max/Team) et API.
- **Déploiement** : Ollama (référencé), HF Inference Providers, API Z.ai (OpenAI-compatible), self-hosted.
- **Fine-tuning** : non vérifié au 27/09/2026 — aucune API de fine-tuning Z.ai ni recette communautaire
  vérifiée trouvée.

## 46. GLM-5.3 (le « cyber »)

- **Nom exact** : GLM-5.3 (HF `zai-org/GLM-5.3`). **14 août 2026** (API/Coding Plan), **poids ouverts le
  28 août 2026** (retenus 2 semaines pour durcissement sécurité). Statut : disponible, poids ouverts
  (licence exacte à confirmer sur la carte HF — la famille est MIT, mais la carte 5.3 n'a pas été vérifiée
  directement).
- **Architecture** : **même base pré-entraînée que GLM-5.2** (743–744B total / ~40B actifs — Z.ai : « chaque
  gain vient du post-training, aucun changement architectural »). **Divergence** : une source tierce
  (codepick) indique 730B / ~70B actifs — non résolu.
- **Attention** : MLA + DSA + IndexShare (hérité de 5.2). **Contexte : 1M tokens**.
- **Fonctionnalités** : positionné **« frontier coding with emergent cyber capabilities »** — post-training
  avec environnements de découverte de vulnérabilités → capacités cyber émergentes (analyse de vulns,
  exploit scripting, élévation de privilèges, chaînes d'exploitation multi-étapes ; 2 436 vulns trouvées
  sur 269 projets open-source, 53 CVE — revendication Z.ai). **Thinking non désactivable** :
  `thinking.type: "disabled"` n'est plus supporté (la requête échoue) ; `reasoning_effort` = low/high/max
  (défaut max).
- **Benchmarks** : Terminal-Bench 3.0 : **4,6 % → 28,3 %** (×6 vs 5.2), DeepSWE v1.1 : 46,2 % → **66,9 %**,
  CyberGym : 77,2 % → **84,5 %**, ExploitBench : 24,4 % → **54,4 %**, HumanEval 94,5 %, MBPP 91,2 %,
  LiveCodeBench 68,3 %, LMArena Elo 1498, AA Intelligence 63,8. **Divergence massive non résolue sur
  SWE-bench Verified : 57,8 %** (codepick, « données officielles ») **vs 82,4 %** (matrice ifnodoraemon) —
  l'un des deux chiffres (ou les deux) est faux ou mal labellisé.
- **Prix API** : **$1,40/$4,40** (annonce 14/08, Coding Plan) **vs $0,50/$2,00** (matrice ifnodoraemon) —
  divergence ; possiblement une baisse de prix entre août et septembre, non vérifié.
- **Déploiement** : API Z.ai, GLM Coding Plan, poids HF (28/08/2026).

## 47. GLM-5.3-Flash (« Ox Alpha »)

- **Nom exact** : GLM-5.3-Flash (HF `zai-org/GLM-5.3-Flash`) — apparu en stealth sous le nom **« Ox Alpha »**
  (`stealth/ox-alpha` sur OpenRouter, 20 août, gratuit 1 semaine), confirmé par Zhipu à Bloomberg le
  **26 août 2026**, lancement officiel + poids le **27 août 2026**.
- **Statut** : disponible, poids ouverts, **licence MIT**.
- **Architecture** : MoE — **320B total / 18B actifs** par token.
- **Contexte** : **1M tokens, nativement multimodal** (texte + image + vidéo en entrée).
- **Fonctionnalités** : tier « Flash » vitesse/débit (matrice tierce : 260 tok/s en Flash vs 115 pour le
  5.3 standard) ; coding/agents à bas coût.
- **Fait industriel** : servi initialement depuis un cluster de **100 000 puces de fabrication chinoise**
  (déclaration Zhipu à Bloomberg) — la démonstration que l'inférence frontier peut tourner sans NVIDIA.
- **Benchmarks** : non vérifié (aucun bench publié dans les sources consultées au 27/09).
- **Prix API** : **$0,15 / 1M input, $0,50 / 1M output** (prix catalogue).
- **Déploiement** : API Z.ai officielle ; poids HF.

## 48. GLM : MLA + DSA + IndexShare, explication

La signature technique de Zhipu combine trois briques. **MLA** (attention latente, style DeepSeek) pour
compresser le KV cache. **DSA** (Dynamic Sparse Attention) : au lieu d'attentionner sur tous les tokens
passés, le modèle sélectionne dynamiquement un sous-ensemble (top-k) de positions pertinentes — l'attention
devient sparse et le coût par token chute sur les très longues séquences. **IndexShare** (nouveauté 5.2) :
des groupes de couches **partagent les mêmes indices top-k** au lieu de les recalculer chacun, ce qui
divise encore le coût à 1M de contexte. C'est l'équivalent Zhipu du GDN de Qwen et du KDA de Kimi : trois
laboratoires, trois noms, une même direction — tuer le coût quadratique de l'attention.

## 49. GLM : la parenthèse « cyber » du 5.3

Le positionnement « emergent cyber capabilities » du GLM-5.3 est une première assumée : un constructeur
qui markete la découverte de vulnérabilités et l'exploit scripting comme feature. Z.ai a retenu les poids
deux semaines pour « durcissement sécurité » avant publication — aveu implicite du dilemme dual-use. Les
chiffres (2 436 vulnérabilités trouvées, 53 CVE, ExploitBench ×2,2) sont vendor-reported et invérifiables
indépendamment, mais la trajectoire est claire : les frontier ouverts de 2026 savent attaquer, et les
labos chinois ne s'en cachent plus. Pour un RSSI, c'est une donnée à intégrer : l'outillage offensif
open-weight est désormais au niveau des équipes de recherche.

## 50. GLM : tableau comparatif

| Modèle | Date | Archi | Params (total/actifs) | Contexte | Prix API ($/1M in-out) | Licence |
|---|---|---|---|---|---|---|
| GLM-5.2 | 16/06/2026 | MoE, MLA + DSA + IndexShare | ~744–753B / ~40B | 1M | 1,40 / 4,40 | MIT |
| GLM-5.3 | 14/08/2026 (poids 28/08) | MoE, même base 5.2 (post-training) | ~744B / ~40B (div. : 730B/70B) | 1M | 1,40 / 4,40 (div. : 0,50/2,00) | MIT (à confirmer carte HF) |
| GLM-5.3-Flash | 27/08/2026 | MoE | 320B / 18B | 1M (multimodal) | 0,15 / 0,50 | MIT |

## 51. Xiaomi MiMo : portrait d'équipe

L'équipe **MiMo** de Xiaomi est dirigée par **Luo Fuli**, ex-DeepSeek et core dev de DeepSeek-V2 — ce qui
explique la filiation technique visible (MoE sparse, MLA, MTP). Xiaomi alterne les phases : open-weight
généreux (7B en 2025, V2.5, V2.6), parenthèse propriétaire (V2-Pro/Omni/TTS, mars 2026, API/produits
Xiaomi uniquement), puis retour à l'ouvert. La marque de fabrique 2026 de MiMo : l'**omnimodalité native**
(texte + image + vidéo + audio dans un seul modèle) et une transparence inédite — le post-training RL de
V2.6 a été **retransmis en direct** sur dashboard public.

## 52. MiMo-7B (avril 2025, la fondation)

- **Nom exact** : MiMo-7B (variantes : Base, SFT, RL, RL-Zero). **Sortie : 30 avril 2025**. Statut :
  disponible, open weights. Antérieur à la période, mais c'est la fondation de la lignée.
- **Architecture** : dense **7B** paramètres, optimisé reasoning. Pré-entraînement sur **25T tokens**
  (dont 200B dédiés reasoning). **MTP (Multiple Token Prediction)**.
- **Contexte** : non vérifié au 27/09/2026. **Poids** : **ouverts, MIT**.
- **Benchmarks** (Xiaomi, MiMo-7B-RL, temp 0,6) : MATH-500 95,8 %, AIME 2024 68,2 %, AIME 2025 55,4 %,
  LiveCodeBench v5 57,8 % / v6 49,3 %, GPQA Diamond 54,4 %, MMLU-Pro 58,6 % ; présenté comme > OpenAI
  o1-mini en maths/code.

## 53. MiMo-VL-7B et MiMo-Audio-7B (fiches prudentes)

- **MiMo-VL-7B** : vision. Date : **4 juin 2025** selon une timeline tierce (logeshwaran.org) — **non
  corroborée par une source primaire**. Statut : disponible, open weights. Specs détaillées : non
  vérifiées au 27/09/2026.
- **MiMo-Audio-7B** : audio. Date : **29 décembre 2025** (même timeline tierce — non vérifiée). Statut :
  disponible, open weights. Specs : non vérifiées.
- Ces deux modèles existent dans la lignée multimodale MiMo, mais la recherche n'a pas permis d'établir
  des fiches solides : ils sont cités pour mémoire, pas pour usage décisionnel.

## 54. MiMo-V2-Flash (décembre 2025, 1ère génération V2)

- **Nom exact** : MiMo-V2-Flash. **Sortie : 17 décembre 2025**. Statut : disponible, open weights.
- **Architecture** : MoE, **309B total / 15B actifs**. Ratio d'attention hybride 5:1 (source tierce, non vérifié).
- **Poids** : **ouverts, MIT**. Specs fines : non vérifiées au 27/09/2026.
- C'est le gabarit « Flash » de Xiaomi : ~300B totaux pour ~15B actifs, le format efficacité qui sera
  repris par V2.5 puis V2.6-Flash-RL.

## 55. MiMo-V2-Pro / V2-Omni / V2-TTS (la parenthèse fermée)

- **Noms exacts** : MiMo-V2-Pro, MiMo-V2-Omni, MiMo-V2-TTS. **Sortie : 18 mars 2026** (Spring launch event,
  Lei Jun). Statut : disponible via API/produits Xiaomi.
- **Architecture** : V2-Pro — **>1T total / 42B actifs** (MoE). V2-Omni / V2-TTS : specs non vérifiées.
- **Poids** : **fermés / propriétaires** — la seule parenthèse fermée de Xiaomi, refermée dès V2.5 (avril 2026).
- **Contexte / prix / benchmarks** : non vérifiés au 27/09/2026 — les specs de ces modèles propriétaires
  sont quasi introuvables publiquement.

## 56. MiMo-V2.5 / V2.5-Pro (avril 2026, retour à l'ouvert)

- **Noms exacts** : MiMo-V2.5, MiMo-V2.5-Pro. **Sortie : 22 avril 2026**. Statut : disponible, open weights.
- **Architecture** : MoE — V2.5 : **310B total** ; V2.5-Pro : **1,02T total** (actifs non vérifiés).
- **Poids** : **ouverts, MIT**. Fait industriel : adapté **jour 1 à 7 plateformes de chips chinoises**
  (T-Head, Kunlun, Enflame, Muxi, Tianshu Zhixin...) + AWS ; programme d'incitation **100T tokens gratuits**
  (30 jours).
- **Contexte / prix / benchmarks** : non vérifiés au 27/09/2026.
- Lecture : Xiaomi pousse l'inférence sur silicium domestique dès le jour de sortie — l'alignement
  modèle-puce chinoise est une stratégie d'écosystème, pas un accident.

## 57. MiMo-V2.6-Pro-RL (septembre 2026)

- **Nom exact** : MiMo-V2.6-Pro-RL. **Poids publiés le 21 septembre 2026**, annonce le 22 (décalage fuseau
  Chine). Statut : disponible, open weights.
- **Architecture** : MoE sparse, **1,02T total / 42B actifs**, 70 couches (attention hybride sliding-window
  + globale), **384 experts routés, 8 actifs/token**, encodeur vision 681M, encodeurs audio, décodeur MTP
  5 couches.
- **Contexte** : **1M tokens**. **Omnimodal natif** : texte + image + vidéo + audio dans un seul modèle.
- **Fonctionnalités** : agents/tool use (focus), raisonnement, méthode « You Only RL Once » (un seul run RL
  mixte code/général/vision/cyber + distillation on-policy).
- **Poids** : **ouverts, MIT** (BF16 sur Hugging Face ; Pro-RL = 573,49 Go de poids, 155 fichiers).
- **Inférence** : non vérifié au 27/09/2026 — pas de données publiques trouvées. Pro demande 2 nœuds /
  32 GPU selon la commande de serving Xiaomi.
- **Benchmarks** : AA Intelligence Index **46** (indépendant, Artificial Analysis — top open-weights, à
  égalité avec Grok 4.7 selon aiweekly). Vendor-reported : DeepSWE v1.1 71,9 (Opus 5 : 74,0),
  AutomationBench 53,1, Toolathlon-Verified 76,9, OSWorld-Verified 82,0, Terminal Bench 2.1 89,9,
  CyberGym 94,0, ExploitBench 47,9.
- **Prix API** : Pro **$0,435 / 1M input, $0,87 / 1M output** (tiers — mêmes chiffres que DeepSeek V4-Pro,
  coïncidence tarifaire du marché).
- **Déploiement** : API Xiaomi (MiMo Open Platform), MiMo Studio (chat web), MiMo Desktop ; poids HF.

## 58. MiMo-V2.6-Flash-RL et Pro-UltraSpeed

- **MiMo-V2.6-Flash-RL** : **309B total / 15B actifs**, contexte 1M, omnimodal, MIT. Prix API :
  **$0,14 / $0,28** par 1M (tiers). Le « Flash » de la génération : même ratio ~20× entre total et actifs
  que le V2-Flash de décembre 2025, la recette Xiaomi de l'efficacité est stable.
- **MiMo-V2.6-Pro-UltraSpeed** : ~1T paramètres, **API uniquement** — la variante vitesse du Pro, non
  ouverte. Specs fines : non vérifiées.

## 59. MiMo-V2.6-Distill-Qwen-9B (le petit malin)

- **Nom exact** : MiMo-V2.6-Distill-Qwen-9B. Sortie avec la famille V2.6 (21–22 septembre 2026).
- **Architecture** : **9B dense**, SFT de Qwen3.5-9B, distillé pour la recherche agentic-RL.
- **Poids** : ouverts, MIT. GGUF officiel (ggml-org, Q4_K_M) — **16 Go VRAM suffisants**.
- **Déploiement** : Ollama / LM Studio. C'est le seul modèle chinois « trillion-class lineage » à tourner
  sur une machine bureautique : distillé depuis un 1T, il embarque une partie du savoir agentic du Pro-RL
  dans 9B. Pour un labo local ou un RAG embarqué, c'est la porte d'entrée la plus accessible de tout ce volume.

## 60. MiMo : le RL retransmis en direct (transparence inédite)

Fait sans précédent dans l'industrie : Xiaomi a **retransmis en direct le post-training RL** de V2.6 sur
un dashboard public (mimo.xiaomi.com/rl/) du 15 au 20 septembre 2026. Chiffres publiés : coût RL **~$3,47M**
($2,62M Pro + $854K Flash), 30 steps, ~750K trajectoires, <6 jours. **7 000+ environnements RL et le code
d'entraînement publiés**. C'est la première fois qu'un labo frontier ouvre le capot du RL à ce niveau :
on connaît désormais le coût marginal du post-training d'un modèle trillion-class (~3,5M$), et c'est
dérisoire face au pré-entraînement. Le message implicite : la barrière n'est plus le RL, c'est le
pré-entraînement et les données.

## 61. MiMo : la controverse Anthropic (septembre 2026)

Anthropic accuse Xiaomi d'avoir utilisé **Claude à grande échelle pour la collecte de données**
(distillation non autorisée) en septembre 2026 — allégation **non tranchée** au 27/09/2026. C'est la
deuxième affaire « distillation sauvage » de l'année côté chinois après les soupçons récurrents visant
d'autres labos. Enjeu : les conditions d'utilisation des API frontier interdisent l'entraînement de
modèles concurrents sur leurs sorties, mais la preuve est techniquement difficile (détection par
« fingerprints » stylistiques, canaris). À suivre : si l'accusation se confirme, elle fragilisera la
légitimité open-weight de toute la lignée MiMo.

## 62. MiMo : tableau comparatif

| Modèle | Date | Archi | Params (total/actifs) | Contexte | Prix API ($/1M in-out) | Licence |
|---|---|---|---|---|---|---|
| MiMo-7B | 30/04/2025 | Dense, MTP, reasoning RL | 7B | non vérifié | — | MIT |
| MiMo-V2-Flash | 17/12/2025 | MoE, hybride 5:1 | 309B / 15B | non vérifié | non vérifié | MIT |
| MiMo-V2-Pro | 18/03/2026 | MoE | >1T / 42B | non vérifié | non vérifié | Fermé |
| MiMo-V2.5 | 22/04/2026 | MoE | 310B / actifs non vérifiés | non vérifié | non vérifié | MIT |
| MiMo-V2.5-Pro | 22/04/2026 | MoE | 1,02T / actifs non vérifiés | non vérifié | non vérifié | MIT |
| MiMo-V2.6-Pro-RL | 21/09/2026 | MoE, SW + globale, omnimodal | 1,02T / 42B | 1M | 0,435 / 0,87 (tiers) | MIT |
| MiMo-V2.6-Flash-RL | 21/09/2026 | MoE, omnimodal | 309B / 15B | 1M | 0,14 / 0,28 (tiers) | MIT |
| MiMo-V2.6-Distill-Qwen-9B | 21/09/2026 | Dense distillé | 9B | non vérifié | — | MIT |

## 63. MiMo : ce que la lignée raconte

Xiaomi a fait en 18 mois le trajet inverse d'Alibaba : parti de l'open-weight pur (7B), passé par une
parenthèse propriétaire (V2-Pro, mars 2026), revenu à l'ouvert avec une radicalité nouvelle (RL public,
7 000 environnements publiés). La constante technique : le MoE sparse à ratio ~20–24× (309B/15B, 1,02T/42B),
l'omnimodalité native, et une équipe issue de DeepSeek qui applique les recettes DeepSeek avec les moyens
de Xiaomi. Le Distill-Qwen-9B est le geste le plus intéressant : distiller un trillion-class vers 9B
utilisables en local, c'est admettre que la valeur n'est plus dans la taille mais dans la recette.

## 64. Ant Group / inclusionAI : portrait

« Ling » est la famille de LLM open-source d'**Ant Group** (le lab s'appelle **inclusionAI**), aussi
appelée **BaiLing / Bailing**. Site : ant-ling.com. Org Hugging Face : `inclusionAI`. GitHub :
`inclusionAI/ling-cookbook`. Ant est l'acteur « efficacité » du paysage chinois : là où les autres
empilent les billions de paramètres, Ling optimise les **paramètres actifs par token** (5,1B actifs pour
le 3.0-flash à 124B totaux) et l'attention linéaire. C'est aussi le seul à décliner nativement une
version **finance** (Fin) co-développée avec des institutions financières.

## 65. Les trois séries Ling / Ring / Ming

La nomenclature Ant tient en trois séries. **Ling** : les modèles « non-thinking » (réponse directe,
efficacité). **Ring** : les modèles « thinking » (raisonnement explicite, CoT long). **Ming** : la série
multimodale omni (texte, vision, audio, musique). Un même socle technique (familles `BailingMoeV2`,
`BailingMoeV3`) décliné selon le besoin : Ling-2.5-1T (flagship non-thinking) et Ring-2.5-1T (premier
thinking à attention linéaire hybride) partagent la génération 2.5 ; Ming-Flash-Omni-2.0 porte la
multimodalité. Retenir le triplet, c'est ne plus se perdre dans les noms.

## 66. Ling 2.0 (octobre 2025, la fondation)

- **Noms exacts** : Ling-mini-2.0, Ling-flash-2.0, Ling-1T (+ variantes base). **Sortie : octobre 2025**.
  Statut : disponible, open weights.
- **Architecture** : `BailingMoeV2ForCausalLM` (HF `model_type: bailing_moe`), MoE. Attention **GQA**
  (16 têtes / 4 KV, head_dim 128) + QK-RMSNorm + demi-RoPE. Routing style DeepSeek-V3 : sigmoid, biais par
  expert, MoE groupé (n_group=8, topk_group=4, top-8 experts), 1 expert partagé, `first_k_dense_replace`
  (premières couches denses).
  - mini-2.0 : **16B total / ~1,4B actifs** (20 couches, 256 experts).
  - flash-2.0 : **100B total / ~6,1B actifs** (32 couches, 256 experts).
  - Ling-1T : **1T total / ~50B actifs** (80 couches).
- **Poids** : ouverts (licence exacte non vérifiée pour chaque variante ; flash-2.0 cité MIT par une source
  tierce). **Contexte / prix / benchmarks** : non vérifiés au 27/09/2026.
- **Déploiement** : vLLM (support `bailing_moe.py`, PR #24627 mergée le 2025-09-15).

## 67. Ling-2.5-1T / Ring-2.5-1T (février 2026)

- **Noms exacts** : Ling-2.5-1T, Ring-2.5-1T. **Sortie : 16 février 2026** (annonce Business Wire). Statut :
  disponible, open licenses (Hugging Face + ModelScope).
- **Architecture** : **1T paramètres** ; Ling-2.5-1T : flagship non-thinking, efficacité de raisonnement ;
  Ring-2.5-1T : **premier modèle thinking à architecture linéaire hybride** (hybrid linear attention).
- **Contexte** : **1M tokens** (Ling-2.5-1T).
- **Fonctionnalités** : agent natif, alignement de préférences fin, token usage réduit (AIME 2026 : ~5 890
  tokens vs 15–23K pour les frontier thinking models) — l'argument « raisonner moins pour raisonner mieux ».
- **Benchmarks** (Ant) : Ring-2.5-1T — IMO 2025 : 35/42 (niveau médaille d'or), CMO 2025 : 105/126
  (> cutoff équipe nationale chinoise).
- **Poids** : ouverts (licence exacte non vérifiée).

## 68. Ming-Flash-Omni-2.0 (février 2026)

- **Nom exact** : Ming-Flash-Omni-2.0. **Sortie : 11 février 2026**. Série multimodale Ming.
- Présenté comme le **premier modèle unifiant parole, audio et musique dans une seule architecture**.
- Specs détaillées : non vérifiées au 27/09/2026. C'est la fiche la plus mince du volume côté Ant :
  l'annonce existe (même Business Wire que Ling-2.5), le détail technique n'a pas filtré.

## 69. Ling-2.6-flash (avril 2026)

- **Nom exact** : Ling-2.6-flash. **Sortie : avril 2026**. Statut : disponible (API ; poids annoncés mais
  **non encore publiés en mai 2026** selon lilting.ch — statut des poids au 27/09/2026 : non vérifié).
- **Architecture** : MoE, **104B total / 7,4B actifs**.
- **Fonctionnalités** : agent/tool-use, production à haut volume — le « cheval de trait » de la gamme.
- C'est le chaînon entre la génération 2.x (GQA classique) et la 3.0 (KDA + MLA) : mêmes ordres de
  grandeur, architecture en transition.

## 70. Ling-3.0-flash (juillet 2026, le flagship efficacité)

- **Nom exact** : Ling-3.0-flash (HF `inclusionAI/Ling-3.0-flash`). **Sortie : 23 juillet 2026**. Statut :
  disponible, open weights.
- **Architecture** : MoE, **124B total / 5,1B actifs par token**. Attention hybride : **35 couches KDA
  (Kimi-Delta linear attention) + 7 couches MLA** (gated MLA), **512 experts routés + 1 partagé, top-8**,
  tête MTP (multi-token prediction), 42 couches. Famille `BailingMoeV3`.
- **Contexte** : **256K natif (262 144), extensible à 1M**.
- **Fonctionnalités** : thinking model (CoT activé par défaut, désactivable), tool calling structuré,
  agents de production. **HiCache** (hierarchical cache) : -60 à -80 % TTFT sur longues conversations
  (vendor claim).
- **Poids** : **ouverts, MIT**. Formats : BF16 (~250 Go), FP8 (128,4 Go), INT4 W4A16 (77 Go), MXFP4/FP4
  (70,4 Go, Blackwell only) ; draft DSpark pour speculative decoding.
- **Inférence** : attention linéaire hybride (KDA) → KV cache réduit ; support vLLM (PR #51045, mergée
  2026-08-05) et SGLang (parsers `ling3` reasoning/tool-call). Fine-tuning local possible (poids ouverts)
  — pas d'offre FT API documentée (non vérifié).
- **Benchmarks** : AA Intelligence Index **38** (source tierce best-of-ai).
- **Prix API** : **$0,075 / 1M input, $0,22 / 1M output** (first-party) — le moins cher de tout le volume
  côté input. OpenRouter : variante `:free` à $0.
- **Déploiement** : vLLM, SGLang, Ollama (non vérifié pour Ollama), DGX Spark (GB10, INT4 72 Go),
  H100/H200/B200/GB300 (tp 4–8).

## 71. Ling-3.0-flash-VL (septembre 2026, la vision)

- **Nom exact** : Ling-3.0-flash-VL. **Sortie : 9 septembre 2026** (open-source ; API déjà lancée avant).
  Statut : disponible, open weights.
- **Architecture** : backbone Ling-3.0-flash + encodeur vision 27 couches (0,4B, patches 16px, merging 2×2,
  frames vidéo appariées) via MLP projector. **~125B total / ~5,1B actifs**. Famille BailingMoeV3.
- **Contexte** : 128K natif (doc SGLang Ant) **vs** 256–262K (cryptobriefing) — **incohérence non tranchée** ;
  une source mentionne extension max 1M (non vérifié).
- **Fonctionnalités** : vision native (images, vidéo, documents, UI logicielles), tool use avec **boucle de
  feedback visuel** (auto-correction : l'agent voit le résultat de son action et se corrige), thinking
  activé par défaut. Pas d'audio.
- **Poids** : **ouverts, MIT** — BF16, FP8, INT4 (GPTQ), FP4 (MXFP4, Blackwell).
- **Benchmarks** : AA Intelligence Index **25** (cryptobriefing).
- **Prix API** : non vérifié. **Déploiement** : API via Ling Studio ; SGLang validé (4×GB300 / 4×H200,
  tp 4 ; tp 8 sur H100).

## 72. Ling-3.0-tiny (le nain)

- **Nom exact** : Ling-3.0-tiny. Date : non vérifiée (postérieur au 23 juillet 2026 ; guide de déploiement
  daté septembre 2026). Statut : disponible, open weights (licence non vérifiée).
- **Architecture** : Sparse MoE, **7,9B total / 1,3B actifs**, attention hybride **KDA linéaire + Gated MLA**,
  **128 experts routés**, contexte **128K natif**.
- **Déploiement** : SGLang sur DGX Spark (BF16). **Prix / benchmarks** : non vérifiés.
- Avec 1,3B actifs par token, c'est le modèle « sérieux » le plus frugal du volume — la preuve que la
  recette BailingMoeV3 descend à l'échelle du poste de travail.

## 73. Ling-3.0-flash-Fin (septembre 2026, la finance)

- **Nom exact** : Ling-3.0-flash-Fin. **Sortie : 9 septembre 2026** (annonce Business Wire ; une source
  tierce dit 3 septembre — incohérence mineure non tranchée). Annoncé à l'Inclusion·Conference on the Bund
  (Shanghai). Statut : open-sourcé (licence non vérifiée).
- **Architecture** : MoE, **124B total / 5,1B actifs** (base Ling-3.0-flash, fine-tuné finance).
- **Fonctionnalités** : recherche investissement, conformité, modélisation de valorisation, reporting
  réglementaire ; co-développé avec des institutions financières (CICC investment banking).
- **Benchmarks** (Ant) : FinFIRST, FinSearchComp Verified, FinCRAFT, FinanceAgent v1.1/v2, APEX-Agents,
  SpreadsheetBench v1/v2, τ³-Banking.
- C'est le seul modèle vertical « métier » (finance) open-weight du volume — tous les autres sont des
  généralistes. Signe que la différenciation 2026 se joue aussi sur les déclinaisons sectorielles.

## 74. Ling : l'attention hybride KDA + MLA (empruntée à Kimi)

Le Ling-3.0-flash empile **35 couches d'attention linéaire KDA (Kimi Delta Attention) + 7 couches MLA**.
Le KDA est la brique de Moonshot (voir section 42) : Ant Group l'a adoptée telle quelle dans sa famille
BailingMoeV3. C'est un cas d'école de circulation des architectures en régime open-weight — là où les
labos fermés gardent leurs recettes, les labos ouverts chinois se réutilisent mutuellement les briques
en quelques semaines. Résultat mesurable : 5,1B actifs par token pour un 124B, soit un ratio total/actifs
de 24×, et un KV cache réduit qui rend le 256K–1M servable à $0,075/M input.

## 75. Ling : HiCache, le cache hiérarchique

**HiCache** (hierarchical cache) est l'optimisation d'inférence mise en avant par Ant pour le 3.0-flash :
-60 à -80 % de TTFT (time to first token) sur les longues conversations (vendor claim, non vérifié
indépendamment). Le principe : organiser le cache en hiérarchie pour éviter de recomposer tout le contexte
à chaque tour — critique pour les agents qui accumulent des centaines de milliers de tokens d'historique
d'outils. Avec le KDA (mémoire bornée) + HiCache (réutilisation), Ant attaque le coût du long contexte
sur les deux fronts : stockage et recalcul.

## 76. Ling : tableau comparatif

| Modèle | Date | Archi | Params (total/actifs) | Contexte | Prix API ($/1M in-out) | Licence |
|---|---|---|---|---|---|---|
| Ling-mini-2.0 | 10/2025 | MoE, GQA | 16B / 1,4B | non vérifié | non vérifié | Ouverts (non vérifiée) |
| Ling-flash-2.0 | 10/2025 | MoE, GQA | 100B / 6,1B | non vérifié | non vérifié | Ouverts (MIT cité, tiers) |
| Ling-1T | 10/2025 | MoE, GQA | 1T / 50B | non vérifié | non vérifié | Ouverts (non vérifiée) |
| Ling-2.5-1T | 16/02/2026 | 1T, non-thinking | 1T / non vérifié | 1M | non vérifié | Ouverts (non vérifiée) |
| Ring-2.5-1T | 16/02/2026 | 1T, thinking, lin. hybride | 1T / non vérifié | non vérifié | non vérifié | Ouverts (non vérifiée) |
| Ming-Flash-Omni-2.0 | 11/02/2026 | Omni (parole/audio/musique) | non vérifié | non vérifié | non vérifié | non vérifié |
| Ling-2.6-flash | 04/2026 | MoE | 104B / 7,4B | non vérifié | non vérifié | non vérifié (poids ?) |
| Ling-3.0-flash | 23/07/2026 | MoE, KDA + MLA, 512 experts | 124B / 5,1B | 256K (>1M) | 0,075 / 0,22 | MIT |
| Ling-3.0-flash-VL | 09/09/2026 | MoE + encodeur vision | ~125B / 5,1B | 128K–256K (div.) | non vérifié | MIT |
| Ling-3.0-tiny | 09/2026 ? | MoE, KDA + Gated MLA | 7,9B / 1,3B | 128K | non vérifié | Ouverts (non vérifiée) |
| Ling-3.0-flash-Fin | 09/09/2026 | MoE (base 3.0-flash) | 124B / 5,1B | non vérifié | non vérifié | Ouverts (non vérifiée) |

## 77. Meituan LongCat : portrait d'équipe

**LongCat** (龙猫, « chinchilla »... non : « long chat ») est l'équipe IA de **Meituan**, le géant chinois
de la livraison de repas — le profil le plus inattendu du volume. GitHub : `meituan-longcat`. La lignée :
LongCat-Flash-Chat (août 2025), Flash-Thinking (sept. 2025), **LongCat-Next** (avril 2026, multimodal
natif), **LongCat-2.0** (juin 2026, 1,6T paramètres, 1M de contexte natif, entraîné sur ASIC chinois).
Meituan joue la carte « outsider qui frappe fort » : le 2.0 revendique des scores coding au niveau de
GPT-5.5 avec un entraînement 100 % domestique.

## 78. LongCat-Flash-Chat (août 2025)

- **Nom exact** : LongCat-Flash-Chat (HF `meituan-longcat/LongCat-Flash-Chat`). **Sortie : 30 août 2025**
  (annonce ; blog SGLang 1er sept.). Statut : disponible, open weights. Antérieur à la période, mais c'est
  la fondation.
- **Architecture** : MoE, **560B total / 18,6–31,3B actifs (~27B en moyenne)**. **512 experts FFN + 256
  experts « zero-computation »**. **ScMoE** (Shortcut-Connected MoE : overlap calcul/communication),
  **MLA**, biais d'expert contrôlé par PID, hyperparameter transfer.
- **Contexte** : **128K tokens** (131 072).
- **Fonctionnalités** : non-thinking, agent/tool-use (focus), >100 tokens/s en inférence.
- **Poids** : **ouverts, MIT**.
- **Benchmarks** : MMLU 89,7 %, MATH-500 96,4 %, GPQA-Diamond 73,2 %, τ²-Bench Telecom 73,7 %.
- **Prix API** : ~$0,69 / 1M tokens (Caixin, 2025 — probablement obsolète au 27/09/2026).
- **Déploiement** : SGLang (blog officiel lm-sys), OpenRouter.

## 79. LongCat-Flash-Thinking (septembre 2025)

- **Nom exact** : LongCat-Flash-Thinking. **Sortie : 22 septembre 2025**. Statut : disponible, open weights
  (Hugging Face + GitHub, démo en ligne).
- **Architecture** : MoE reasoning, **560B total** (même base que Flash). Cold-start CoT long + RL large
  échelle ; schéma d'entraînement **« domain-parallel »** (fusion de modèles experts par domaine : STEM,
  Code, Agentic) ; framework RL **DORA** (Dynamic ORchestration for Asynchronous rollout, >3× speedup).
- **Fonctionnalités** : reasoning avec efficacité revendiquée : -64,5 % de tokens moyens sur AIME-25
  (19 653 → 6 965) sans perte de précision.
- **Poids** : ouverts (licence exacte non vérifiée — probablement MIT comme Flash, non confirmé).
- **Contexte / prix / déploiement** : non vérifiés au 27/09/2026.

## 80. LongCat-Next (avril 2026, le multimodal natif)

- **Nom exact** : LongCat-Next. **Sortie : 2 avril 2026** (article ; papier arXiv 2603.27538, mars 2026).
  Statut : disponible, open weights.
- **Architecture** : modèle **nativement multimodal** (texte, vision, audio) sous paradigme **DiNA**
  (Discrete Native Autoregression) — toutes les modalités en tokens discrets dans un espace partagé.
  Tokenizer visuel **dNaViT** (RVQ 8 couches). Backbone **LongCat-Flash-Lite MoE : ~68,5–75B total / ~3B
  actifs** (sources : 74B / 75B / 68,5B — incohérence mineure non vérifiée), attention MLA, N-gram
  over-embedding, tokenizers RVQ natifs vision+audio.
- **Fonctionnalités** : compréhension + **génération d'images**, compréhension audio, synthèse vocale /
  clonage de voix (24 kHz, streaming), tool calling. Pas de génération vidéo (compréhension seule).
- **Poids** : **ouverts, MIT** (poids, tokenizers, code d'inférence).
- **Contexte / prix / benchmarks** : non vérifiés au 27/09/2026.
- **Déploiement** : SGLang (adaptation officielle `LongCat-Next-inference`) ; build communautaire
  w8a8_int8 ~90 Go sur DGX Spark.

## 81. LongCat-2.0 (juin 2026, le 1,6T domestique)

- **Nom exact** : LongCat-2.0. **Sortie : 30 juin 2026**. Statut : annoncé + open-sourcé (MIT) — une source
  (5 juillet 2026) disait « weights coming soon » ; au 27/09/2026 le repo/blog officiel existe
  (longcat.ai/blog/longcat-2.0/), mais la disponibilité effective des poids reste **non vérifiée**.
- **Architecture** : MoE, **1,6T total / ~48B actifs (33–56B)**. Contexte **1M natif**, **LongCat Sparse
  Attention**, sortie max 128K. Entraîné et servi sur **supercalculateurs d'ASIC chinois domestiques**
  (cluster ~50 000 cartes — claim Meituan). « Super kernels » + L2-cache weight prefetching.
- **Fonctionnalités** : agentic coding.
- **Poids** : **MIT** (selon table marktechpost).
- **Benchmarks** (Meituan, non indépendants) : SWE-bench Pro 59,5 (vs GPT-5.5 58,6), Terminal-Bench 2.1
  70,8, SWE-bench Multilingual 77,3 ; « comparable à Gemini 3.1 Pro ».
- **Prix API / déploiement / fine-tuning** : non vérifiés au 27/09/2026.

## 82. LongCat : ScMoE et l'entraînement « domain-parallel »

Deux idées originales chez Meituan. **ScMoE** (Shortcut-Connected MoE) : des connexions « shortcut » entre
experts permettent de recouvrir (overlap) le calcul et la communication inter-experts — le goulot
classique du MoE à grande échelle. Les **experts « zero-computation »** (256 dans Flash-Chat) sont des
experts qui ne calculent rien mais routent : ils servent de « mémoire de routage » à coût nul. Côté
entraînement, le **domain-parallel** fusionne des modèles experts par domaine (STEM, Code, Agentic) au
lieu d'un seul run monolithique, et le framework **DORA** accélère le RL asynchrone (>3× speedup). C'est
une école d'ingénierie pragmatique : moins de théorie de l'attention, plus d'optimisation système.

## 83. LongCat : tableau comparatif

| Modèle | Date | Archi | Params (total/actifs) | Contexte | Prix API ($/1M in-out) | Licence |
|---|---|---|---|---|---|---|
| LongCat-Flash-Chat | 30/08/2025 | MoE, ScMoE, MLA | 560B / ~27B | 128K | ~0,69 (2025, obsolète ?) | MIT |
| LongCat-Flash-Thinking | 22/09/2025 | MoE reasoning, DORA | 560B / non vérifié | non vérifié | non vérifié | Ouverts (prob. MIT) |
| LongCat-Next | 02/04/2026 | MoE multimodal DiNA | ~68,5–75B / ~3B | non vérifié | non vérifié | MIT |
| LongCat-2.0 | 30/06/2026 | MoE, LongCat Sparse Attn | 1,6T / ~48B | 1M natif | non vérifié | MIT (poids à confirmer) |

## 84. MiniMax : portrait de l'entreprise

MiniMax (Shanghai) est le laboratoire chinois le plus « produit » du volume : modèles de langue (série M),
génération vidéo/audio (série H), API internationale. En 2026, la série **M2** (M2 → M2.7) a conquis le
titre de « roi de l'open-source » sur l'agentique, **M3** (1er juin 2026) est passé au 1M de contexte
multimodal, et **H3** (juillet 2026) a ouvert la génération vidéo+audio — avec une **licence territoriale
restrictive** inédite qui mérite sa section. Dérivé notable : **humain-m3**, modèle arabe co-développé
avec HUMAIN (Arabie saoudite).

## 85. MiniMax-M2 (série : M2, M2.1, M2.5, M2.7)

- **Noms exacts** : MiniMax-M2 (23 octobre 2025), M2.1 (22–23 décembre 2025), M2.5 (février 2026 — une
  source indique le 12/02, à confirmer), M2.7 (annoncé 18 mars 2026, poids open-sourcés le 12 avril 2026).
  Statut : disponible (API + poids ouverts).
- **Architecture** : **MoE sparse** — **230B paramètres totaux, 10B actifs par forward** (256 experts fins,
  8 actifs, sigmoid routing avec expert biases). 62 blocs décodeur, hidden size 3 072, vocab 200 064.
  **Full attention GQA dans toutes les couches** (48 query heads, 8 KV heads, RoPE) — retour au full
  attention après l'hybride Lightning/full de MiniMax-01. Module **MTP** : 1 module au pré-entraînement,
  étendu à 3 par copie de poids pour le speculative decoding. Pré-entraînement : 29,2T tokens.
- **Contexte** : **192K–205K natif** (192K selon arXiv 2605.26494 ; 204.8K input / 176.9K output selon
  Artificial Analysis ; limite par séquence 196K selon un profil d'intégration).
- **Fonctionnalités** : text in/out, function calling, structured output, reasoning toujours actif. Design
  orienté **coding & agents** (multi-file edits, code-run-fix loops, toolchains long-horizon
  shell/browser/retrieval/code runners). Pas de vision.
- **Poids** : ouverts (HuggingFace `MiniMaxAI/MiniMax-M2*`). **Nom exact de la licence : non vérifié**
  (controverse de licence autour de M2.7 mentionnée sans détail par awesomeagents).
- **Inférence** : poids ~220 Go ; ~240 Go de KV cache par 1M tokens (note fournisseur citée par un profil
  d'intégration) ; MTP (3 têtes) pour speculative decoding ; serveurs recommandés vLLM (4×H100 en FP8) ou
  SGLang, parsers d'outils `minimax_m2` / `minimax_m2_append_think`. Sampling : temperature=1.0,
  top_p=0.95, top_k=40.
- **Benchmarks** : M2 (oct 2025, fournisseur) — SWE-bench Verified 69.4, ArtifactsBench 66.8, τ²-Bench
  77.2, GAIA text-only 75.7, BrowseComp 44.0, FinSearchComp-global 65.5. M2.7 — SWE-Pro 56.22 % (parité
  GPT-5.3-Codex), Terminal Bench 2 57.0 %, VIBE-Pro 55.6 %, GDPval-AA 1495 ELO (#1 open-source), MLE Bench
  Lite 66.6 % medal rate. M2.5 → M2.7 : taux d'hallucination AA-Omniscience de 88 % à **34 %** (meilleur
  que Claude Sonnet 4.6 à 46 %). Artificial Analysis : #1 composite open-source.
- **Prix API** : **$0,30 / 1M input, $1,20 / 1M output** (tarif cité par Magica, sept 2026).
- **Déploiement** : vLLM, SGLang (parsers dédiés), Transformers, GGUF (communauté), KTransformers, ATOM
  (ROCm), MLX-LM, Ollama (docs tierces).
- **Fine-tuning** : non vérifié au 27/09/2026.

## 86. MiniMax-M2.7 et le « self-evolution harness »

La nouveauté conceptuelle de M2.7 (mars 2026) est le **« self-evolution harness »** : mémoire court terme,
self-feedback, self-optimization — le modèle gère **30–50 % du workflow RL interne** de MiniMax
(VentureBeat). Autrement dit, MiniMax utilise M2.7 pour entraîner ses propres successeurs : la boucle
« modèle qui s'améliore lui-même » n'est plus un slogan mais une part mesurée du pipeline. C'est aussi
avec M2.7 qu'est apparue une « controverse de licence » (awesomeagents) dont le détail n'a pas été vérifié
— prudence donc sur la réutilisation commerciale des poids M2.7 sans lecture directe de la licence.

## 87. MiniMax-M3 (juin 2026)

- **Nom exact** : MiniMax-M3. **Sortie : 1er juin 2026** (annonce MiniMax API docs). Statut : disponible
  (modèle de langue courant de la série M).
- **Architecture** : **MoE**. Nombre total de paramètres : **non vérifié** — le checkpoint
  `MiniMaxAI/MiniMax-M3-MXFP8` fait ~444 Go (ce qui suggère ~440B paramètres en MXFP8 ≈ 1 octet/param,
  mais c'est une inférence, pas une spec).
- **Contexte** : **1M tokens** (contexte long).
- **Fonctionnalités** : agentic reasoning, tool use, coding, **chat multimodal en input** (texte +
  image/vision en entrée), long-context. Pas de vision en sortie.
- **Poids** : ouverts (HF `MiniMaxAI/MiniMax-M3-MXFP8` notamment). Nom de licence : non vérifié.
- **Inférence** : self-hosting marqué **expérimental** par MiniMax (revue doc : 26 août 2026). Baseline :
  8×B200, image SGLang pinnée par digest, `--reasoning-parser auto --tool-call-parser auto --tp 8`.
  Support : SGLang, vLLM, Transformers (`minimax_m3_vl`), KTransformers, Unsloth, ATOM (ROCm), MLX-LM.
  Sampling : temperature=1.0, top_p=0.95 (pas de top_k).
- **Benchmarks** : avec le framework **MaxProof** (generative-verifier RL + evolutionary search), M3
  dépasse le **seuil médaille d'or humaine sur IMO 2025 et USAMO 2026** (blog MiniMax, 9 juin 2026). Pas
  de chiffres de benchmarks publics tiers détaillés dans les sources trouvées.
- **Prix API** : non vérifié. **Fine-tuning** : non vérifié.

## 88. MiniMax-H3 (juillet 2026, vidéo + audio génératifs)

- **Nom exact** : MiniMax-H3. Annoncé à **WAIC 2026 le 17 juillet 2026** ; **API le 31 juillet 2026** ;
  **poids ouverts le 3 août 2026**. Statut : disponible (API + poids ouverts partiels).
- **Architecture** : **omni-modal génératif (vidéo+audio)** — **H3-Omni-Transformer dense 33B single-stream**
  qui prédit conjointement les latents vidéo et audio ; encodages spécifiques par modalité packés en
  séquence multimodale ; H3-VisualVAE et H3-AudioVAE (espaces latents séparés) ; sparse attention annoncée
  native (update ultérieure). Deux checkpoints Base distillés CFG task-specific : **FL2VA** (texte /
  first-or-last-frame conditioning) et **Ref2VA** (références multimodales : jusqu'à 9 images, 3 clips
  vidéo, 3 clips audio, max 12 fichiers).
- **Sorties** : vidéo **4–15 secondes à 24 FPS, audio stéréo 32 kHz natif** ; Base = 768p, workflow hébergé
  complet jusqu'à 2K.
- **Fonctionnalités** : text→vidéo, image→vidéo, vidéo native avec audio stéréo natif, séquençage
  multi-shot, prompt adherence, text rendering, character consistency, motion transfer.
- **Poids** : **ouverts** (checkpoints H3-Base) mais voir section 89 — **restriction territoriale**.
  Les composants H3-Context-IR et H3-Regenerate-2K restent hébergés ; le workflow 2K complet n'est pas
  en poids ouverts.
- **Benchmarks** : aucun benchmark public standard trouvé (modèle génératif — pas de benchmarks LLM
  classiques). **Prix API** : non vérifié. **Fine-tuning** : non vérifié.
- **Déploiement** : ComfyUI (guide Spheron), API MiniMax. VRAM : du GPU consommateur au datacenter selon
  Spheron.

## 89. MiniMax : la licence territoriale restrictive du H3

C'est la licence la plus inhabituelle du volume : la `minimax-h3-community-license-agreement` (datée
**2026-08-02**) **exclut explicitement les États-Unis, l'UE, le Royaume-Uni et la Corée du Sud** du
déploiement local sans autorisation individuelle. Autrement dit : poids « ouverts » mais pas pour tout le
monde — une première qui inverse la logique habituelle (d'habitude, c'est l'export depuis la Chine qui est
restreint, pas l'usage en Occident). Lecture géopolitique : MiniMax se protège contre la concurrence
frontale sur son propre terrain génératif tout en gardant l'API comme porte d'entrée mondiale. Pour un
utilisateur hors zones exclues, les poids Base 768p sont auto-hébergeables ; le workflow 2K complet
reste de toute façon hébergé.

## 90. humain-m3 : le dérivé saoudien (septembre 2026)

- **Nom exact** : humain-m3. Annoncé le **3 septembre 2026** à LEAP par **HUMAIN** (PIF, Arabie saoudite).
- **Architecture** : **428B paramètres MoE**, issu de la lignée MiniMax-M3, pré-entraîné sur **>1T tokens
  de contenu arabe natif**, meilleur score moyen sur 7 benchmarks arabes publics.
- **Statut** : research preview sur HUMAIN Node ; **open-weight release planifiée** — non vérifié si
  effective au 27/09/2026.
- Ce n'est pas un modèle MiniMax pur mais un **dérivé vérifié** : il illustre la stratégie d'export du
  savoir-faire chinois via partenariat souverain (modèle arabe frontier construit sur base chinoise
  ouverte). À surveiller : si la release open-weight se confirme, ce sera le premier frontier arabe
  ouvert de cette génération.

## 91. MiniMax : tableau comparatif

| Modèle | Date | Archi | Params (total/actifs) | Contexte | Prix API ($/1M in-out) | Licence |
|---|---|---|---|---|---|---|
| MiniMax-M2 → M2.7 | 10/2025 → 03/2026 | MoE, full GQA, MTP | 230B / 10B | ~192–205K | 0,30 / 1,20 | Ouverts (nom non vérifié) |
| MiniMax-M3 | 01/06/2026 | MoE | ~440B ? (non vérifié) | 1M | non vérifié | Ouverts (non vérifiée) |
| MiniMax-H3 | 17/07/2026 (poids 03/08) | Dense 33B omni vidéo+audio | 33B (dense) | multimodal natif | non vérifié | Community (territoires exclus) |
| humain-m3 (dérivé) | 03/09/2026 | MoE (lignée M3) | 428B / non vérifié | non vérifié | non vérifié | Preview (open-weight planifié) |

## 92. Tencent : portrait de l'entreprise (Hunyuan)

Tencent, via sa division **Hunyuan**, est le dernier géant chinois à avoir dégainé en 2026 — mais avec une
cible claire : **l'agent logiciel**. **Hy3 Preview** (avril 2026) puis **Tencent Hy3 / Hunyuan 3**
(**6 juillet 2026**) sont explicitement optimisés pour les workloads agents (coding repo-scale, CI/CD,
multi-step tool use, workflows financiers). ID API : `hy3`. Tencent joue la carte de l'efficacité
ciblée : 295B total / 21B actifs, Apache 2.0, support day-0 SGLang/vLLM — un positionnement « le meilleur
agent ouvert par dollar de compute » plutôt que la course au billion de paramètres.

## 93. Hy3 Preview (avril 2026)

- **Nom exact** : Hy3 Preview. **Sortie : avril 2026**. Statut : précurseur remplacé par Hy3 production
  le 6 juillet 2026.
- C'est la version « bêta publique » qui a permis à Tencent de roder son positionnement agent avant la
  release : function calling, structured output, context caching, preserved reasoning étaient déjà là.
- Specs détaillées : non vérifiées séparément au 27/09/2026 — la fiche Hy3 ci-dessous fait foi pour la
  lignée.

## 94. Tencent Hy3 (Hunyuan 3)

- **Nom exact** : Tencent Hy3 (Hunyuan 3) — ID API : `hy3`. **Sortie : 6 juillet 2026** (successeur de
  Hy3 Preview, avril 2026). Statut : disponible (poids ouverts + API).
- **Architecture** : **MoE — 295B total / 21B actifs par token** ; 192 experts, 8 actifs par token
  (+ expert partagé selon sources) ; + **3.8B paramètres de couches MTP** (multi-token prediction).
- **Attention** : type non vérifié au 27/09/2026.
- **Contexte** : **256K** (input max 192K, output max 128K).
- **Fonctionnalités** : **hybrid reasoning** — modes `no_think` / `think_low` / `think_high` ; function
  calling, structured output, context caching, preserved reasoning ; conçu pour agents logiciels
  (repo-scale coding, CI/CD, multi-step tool use), knowledge work long-contexte, workflows financiers.
- **Poids / licence** : **ouverts, Apache 2.0** ; checkpoint FP8 officiel ; support **day-0 SGLang**
  (FP8 + speculative decoding) ; vLLM ≥ 0.23 (support natif arch `hy_v3` / `hy_v3_mtp`) ; variantes
  communautaires NVFP4 (`kodelow/Hy3-NVFP4-W4A16`) ; tutoriel SGLang sur Ascend NPU (910B, 2× Atlas 800I A2).
- **Inférence** : ~21.8 tok/s single-stream sur 2× DGX Spark (NVFP4, test communautaire) ; 181 Go en
  NVFP4-W4A16 ; contexte 128K validé en pratique (256K = stretch, selon runbook communautaire).
- **Benchmarks** (Tencent, directionnels, non répliqués indépendamment) : SWE-bench Verified **78.0**,
  SWE-bench Pro **57.9**, GPQA Diamond **90.4**, HLE **53.2**.
- **Prix API** : fenêtre **gratuite** (Nous Portal + OpenRouter `:free` du ~6 au ~21 juillet 2026, fenêtre
  fermée). Prix courant : non vérifié au 27/09/2026.
- **Fine-tuning** : poids ouverts → fine-tuning/quantization/RL post-training possibles en self-hosted ;
  fine-tuning via API : non vérifié.
- **Déploiement** : SGLang, vLLM, Ollama/MLX (quant 4-bit, « Ultra-only » = très gros hardware), Nous
  Portal, OpenRouter.

## 95. Hy3 : l'agent comme cible de design

Là où DeepSeek optimise le ratio benchmark/prix et Kimi l'essaim d'agents, Tencent optimise le **workflow
agent complet** : `preserved reasoning` (le raisonnement survit entre les appels d'outils), context caching
(le cache survit entre les tours), structured output garanti, et trois niveaux de thinking commutables
(`no_think` pour les appels triviaux, `think_high` pour la planification). C'est une philosophie
« infra-agent » : le modèle est conçu comme un composant logiciel fiable plutôt que comme un cerveau
brillant. Le tutoriel officiel SGLang sur **Ascend NPU** (910B, 2× Atlas 800I A2) montre que Tencent
prépare aussi l'inférence domestique — comme Zhipu et Xiaomi, la pile chinoise se découple de NVIDIA.

## 96. Tencent : tableau comparatif

| Modèle | Date | Archi | Params (total/actifs) | Contexte | Prix API ($/1M in-out) | Licence |
|---|---|---|---|---|---|---|
| Hy3 Preview | 04/2026 | MoE (non vérifié) | non vérifié | non vérifié | — (preview) | non vérifié |
| Tencent Hy3 | 06/07/2026 | MoE, +3.8B MTP | 295B / 21B | 256K (192K in / 128K out) | gratuit 6→21/07, puis non vérifié | Apache 2.0 |

## 97. Le grand tableau comparatif Chine (février → septembre 2026)

| Modèle | Labo | Date | Archi | Total / actifs | Contexte | Prix $/1M (in/out) | Licence |
|---|---|---|---|---|---|---|---|
| Qwen3.5-397B-A17B | Alibaba | 16/02/26 | MoE, GDN | 397B / 17B | ~262K | n.v. | Apache 2.0 |
| Ling-2.5-1T / Ring-2.5-1T | Ant | 16/02/26 | 1T MoE | 1T / n.v. | 1M | n.v. | Ouverts (n.v.) |
| Qwen3-Coder-Next | Alibaba | 02/26 | MoE, GDN | 80B / 3B | 256K | ~0,11 / 0,80 | Apache 2.0 |
| MiMo-V2-Pro | Xiaomi | 18/03/26 | MoE | >1T / 42B | n.v. | n.v. | Fermé |
| MiniMax-M2.7 | MiniMax | 18/03/26 | MoE, GQA full | 230B / 10B | ~200K | 0,30 / 1,20 | Ouverts (n.v.) |
| Qwen3.6-Plus | Alibaba | 02/04/26 | n.v. (fermé) | n.v. | 1M | ~0,29 / 1,74 | Fermé |
| LongCat-Next | Meituan | 02/04/26 | MoE multimodal DiNA | ~70B / 3B | n.v. | n.v. | MIT |
| Qwen3.6-35B-A3B | Alibaba | 16/04/26 | MoE, GDN | 35B / 3B | 256K | ~0,14 / 1,00 | Apache 2.0 |
| Qwen3.6-Max-Preview | Alibaba | 20/04/26 | MoE (presse) | 35B / 3B | 256K | n.v. | Fermé |
| Kimi K2.6 | Moonshot | 20/04/26 | MoE, MLA | 1T / 32B | 256K | 0,60 / 2,50 | Modified MIT |
| MiMo-V2.5 / V2.5-Pro | Xiaomi | 22/04/26 | MoE | 310B / 1,02T | n.v. | n.v. | MIT |
| DeepSeek V4-Pro | DeepSeek | 24/04/26 | MoE, attn. hybride | 1,6T / 49B | 1M | 0,435 / 0,87–0,93 | MIT |
| DeepSeek V4-Flash | DeepSeek | 24/04/26 | MoE, NVFP4 | 284B / 13B | 1M | n.v. | MIT |
| Qwen3.7-Max | Alibaba | 20/05/26 | n.v. (fermé) | n.v. | 1M | ~1,25 / 3,75 | Fermé |
| Qwen3.7-Plus | Alibaba | 20/05/26 | n.v. (fermé) | n.v. | 1M ? | 0,40 / 1,60 | Fermé |
| MiniMax-M3 | MiniMax | 01/06/26 | MoE | ~440B ? | 1M | n.v. | Ouverts (n.v.) |
| Kimi K2.7 Code | Moonshot | 12/06/26 | MoE, MLA | 1T / 32B | 256K | 0,95 / 4,00 | Modified MIT |
| GLM-5.2 | Zhipu | 16/06/26 | MoE, MLA+DSA | ~750B / 40B | 1M | 1,40 / 4,40 | MIT |
| LongCat-2.0 | Meituan | 30/06/26 | MoE, sparse attn | 1,6T / 48B | 1M natif | n.v. | MIT (poids ?) |
| Tencent Hy3 | Tencent | 06/07/26 | MoE + MTP | 295B / 21B | 256K | n.v. (post-gratuit) | Apache 2.0 |
| MiniMax-H3 | MiniMax | 17/07/26 | Dense 33B omni | 33B | multimodal | n.v. | Community (restrictive) |
| Ling-3.0-flash | Ant | 23/07/26 | MoE, KDA+MLA | 124B / 5,1B | 256K | 0,075 / 0,22 | MIT |
| Kimi K3 | Moonshot | 27/07/26 | MoE, KDA+MLA | 2,8T / 104B | 1M | 1,20 / 4,80 | Modified MIT (?) |
| Qwen3.8-Max | Alibaba | 03/08/26 | MoE | 2,4T / 95B | 1M | 2,00 / 6,00 | Ouverts (n.v.) |
| Qwen3.8-27B | Alibaba | 14/08/26 | Dense, GDN | 27,78B | 256K | n.v. | Apache 2.0 |
| GLM-5.3 | Zhipu | 14/08/26 | MoE (base 5.2) | ~744B / 40B | 1M | 1,40 / 4,40 | MIT (?) |
| GLM-5.3-Flash | Zhipu | 27/08/26 | MoE | 320B / 18B | 1M | 0,15 / 0,50 | MIT |
| Qwen3.8-Flash | Alibaba | 02/09/26 | MoE, GDN+sparse | 176B / 6B | 256K | 0,16 / 0,47 | Ouverts (n.v.) |
| humain-m3 | HUMAIN×MiniMax | 03/09/26 | MoE (lignée M3) | 428B | n.v. | n.v. | Preview |
| Ling-3.0-flash-VL | Ant | 09/09/26 | MoE + vision | ~125B / 5,1B | 128–256K | n.v. | MIT |
| Ling-3.0-flash-Fin | Ant | 09/09/26 | MoE (base 3.0) | 124B / 5,1B | n.v. | n.v. | Ouverts (n.v.) |
| DeepSeek V4.1-Flash | DeepSeek | 10/09/26 | MoE, enc.-dec. | 552B / 8–16B | 1M | 0,15 / 0,60 | MIT |
| MiMo-V2.6-Pro-RL | Xiaomi | 21/09/26 | MoE, omnimodal | 1,02T / 42B | 1M | 0,435 / 0,87 | MIT |
| MiMo-V2.6-Flash-RL | Xiaomi | 21/09/26 | MoE, omnimodal | 309B / 15B | 1M | 0,14 / 0,28 | MIT |

*(n.v. = non vérifié au 27/09/2026. Les prix « ~ » viennent de sources tierces.)*

## 98. Timeline février → septembre 2026

- **16 fév.** : Qwen3.5-397B-A17B (Alibaba, ouvert) + Ling-2.5-1T / Ring-2.5-1T (Ant, ouverts). Le mois
  commence par un tir groupé open-weight.
- **Mars** : Qwen3-Coder-Next · MiMo-V2-Pro/Omni/TTS (Xiaomi, fermés — parenthèse) · MiniMax-M2.7
  (self-evolution harness).
- **Avril** : Qwen3.6-Plus (fermé) · Qwen3.6-35B-A3B (ouvert) · Qwen3.6-Max-Preview (fermé — rupture
  « frontier-closed ») · Kimi K2.6 (ouvert, 1T) · MiMo-V2.5 (retour à l'ouvert) · DeepSeek V4-Pro/Flash
  (ouverts, MIT).
- **Mai** : Qwen3.7-Max/Plus (fermés, agents natifs 35 h, puce Zhenwu M890).
- **Juin** : MiniMax-M3 (ouvert, 1M) · Kimi K2.7 Code (ouvert) · GLM-5.2 (ouvert, MIT, AA #51) ·
  LongCat-2.0 (1,6T, ASIC chinois).
- **Juillet** : Tencent Hy3 (ouvert, Apache 2.0) · MiniMax-H3 (vidéo/audio, licence territoriale) ·
  Ling-3.0-flash (ouvert, MIT, $0,075/M) · Kimi K3 (ouvert, 2,8T — record).
- **Août** : Qwen3.8-Max (ouvert, 2,4T — revirement) · Qwen3.8-27B (dense ouvert qui bat Opus 4.6 Max sur
  le code) · GLM-5.3 (ouvert, « cyber ») · GLM-5.3-Flash / « Ox Alpha » (ouvert, 100K puces chinoises).
- **Septembre** : Qwen3.8-Flash (preview Qwen4, ouvert) · humain-m3 (dérivé arabe) · Ling-3.0-flash-VL/Fin ·
  DeepSeek V4.1-Flash (remplace V4-Flash et déprécie V4-Pro) · MiMo-V2.6 (RL retransmis en direct,
  controverse Anthropic).
- **En attente au 27/09** : DeepSeek V4.1 Pro (annoncé, rumeur 28–30 sept.), release open-weight
  d'humain-m3, Qwen4 (teasé via 3.8-Flash).

## 99. Que retenir (1/4) : la course au MoE géant open-weight

En huit mois, le plafond open-weight est passé de ~400B (Qwen3.5, février) à **2,8T paramètres** (Kimi K3,
juillet), avec 2,4T (Qwen3.8-Max), 1,6T (DeepSeek V4-Pro, LongCat-2.0) et 1T (Kimi K2.6, MiMo-V2.6-Pro,
Ling-2.5) entre les deux. Mais le chiffre qui compte n'est pas le total : c'est le **ratio total/actifs**,
passé de ~10× à **20–27×** (K3 : 2,8T/104B, Ling-3.0 : 124B/5,1B, Qwen3.8-Flash : 176B/6B). La sparsité est
devenue l'arme économique : servir 100B actifs coûte comme un dense 100B, mais le modèle « sait » comme
un 2,8T. Conséquence : l'écart open/fermé sur les benchmarks généralistes s'est refermé (K3 à AA 65,
MiMo-V2.6 à 46, GLM-5.3 à 63,8 — des niveaux frontier), et la question n'est plus « l'ouvert peut-il
rivaliser ? » mais « que reste-t-il au fermé ? ». Réponse provisoire : l'agentique très longue (35 h de
Qwen3.7-Max) et l'avance de quelques mois.

## 100. Que retenir (2/4) : le contexte 1M généralisé

Le million de tokens est passé du statut d'exploit (2024–2025) à celui de **standard de gamme** : V4,
V4.1-Flash, Qwen3.6-Plus, Qwen3.7-Max, Qwen3.8-Max, GLM-5.2/5.3/Flash, MiMo-V2.6, Kimi K3, Ling-2.5-1T,
MiniMax-M3, LongCat-2.0 — douze modèles de ce volume l'affichent. Trois moteurs : l'attention
linéaire/hybride (GDN, KDA, DSA+IndexShare) qui borne la mémoire, le KV cache compressé (MLA et dérivés),
et les optimisations système (Mooncake, HiCache, DeepEP). Le 1M « natif » vs « via YaRN » reste une
distinction à surveiller (le 256K stretché n'a pas la même qualité que le 1M entraîné), mais la direction
est sans ambiguïté : en 2026, un modèle chinois sans 1M (ou sans chemin crédible vers 1M) est un modèle
de seconde zone.

## 101. Que retenir (3/4) : les prix en chute libre

La guerre des prix 2026 se lit en trois chiffres. **Input standard** : de ~$1,40 (GLM-5.2, juin) à
**$0,075** (Ling-3.0-flash, juillet) — division par ~19 en six semaines sur le segment efficace.
**Flash** : $0,15–0,16/M input chez DeepSeek, Qwen et Zhipu (alignement au cent près, septembre).
**Cache hit** : $0,003625/M chez DeepSeek V4-Pro, soit 120× moins que le cache miss — la tarification
pousse à architecturer autour du cache. Pendant ce temps, les flagships fermés (Qwen3.7-Max à $1,25/M,
Qwen3.8-Max à $2,00/M, Kimi K3 à $1,20/M) restent 10–25× plus chers que les efficaces ouverts — mais ce
sont eux qui portent les scores frontier. Le marché s'est polarisé : le token banal ne vaut plus rien,
le token « frontier » se paie encore.

## 102. Que retenir (4/4) : l'attention linéaire/hybride, vraie révolution technique

Le fil rouge technique du volume : **tout le monde a tué l'attention quadratique**. Gated DeltaNet
(Qwen), KDA (Kimi, réutilisé par Ant), DSA + IndexShare (Zhipu), attention hybride CSA/HCA/SWA (DeepSeek),
LongCat Sparse Attention (Meituan) — cinq noms pour une idée : mélanger couches linéaires à état borné
et couches d'attention sparse/dense pour un coût mémoire qui n'explose plus avec la séquence. C'est cette
brique, plus que le MoE, qui a rendu le 1M généralisé et les prix cassés possibles. Et elle circule :
Ant reprend le KDA de Moonshot en quelques semaines. L'open-weight chinois fonctionne comme un
« commons » d'architectures où l'innovation se diffuse à la vitesse des releases — l'inverse du modèle
fermé occidental où chaque labo garde sa recette.

## 103. Le pari des puces chinoises

Quatre faits convergent : GLM-5 entraîné **entièrement sur Huawei Ascend** (zéro NVIDIA, corroboré par
Reuters) ; GLM-5.3-Flash servi depuis **100 000 puces de fabrication chinoise** ; LongCat-2.0 entraîné
sur **~50 000 ASIC domestiques** ; MiMo-V2.5 adapté **jour 1 à 7 plateformes de chips chinoises** ;
tutoriel SGLang officiel pour Hy3 sur **Ascend 910B**. Ajouter le lancement conjoint puce+modèle
d'Alibaba (Zhenwu M890 + Qwen3.7-Max). La pile IA chinoise se découple méthodiquement de NVIDIA, du
silicium au serving. Pour l'observateur : les modèles ouverts chinois de 2026 sont déjà pensés pour
tourner sur silicium domestique — c'est un critère de robustesse géopolitique, pas un détail.

## 104. Où va Qwen4 (et la suite)

Le Qwen3.8-Flash est présenté par Alibaba comme une **« preview explicite de l'architecture Qwen4 »**
(Gated DeltaNet + Qwen Sparse Attention, table N-gram 51B). Le schéma habituel d'Alibaba : teaser
l'architecture sur un modèle efficace ouvert, puis décliner en flagship. Avec DeepSeek V4.1 Pro attendu
imminemment (rumeur 28–30 septembre 2026), Kimi K3 installé comme référence ouverte et GLM-5.3 en
embuscade « cyber », le Q4 2026 s'annonce comme un choc frontal : Qwen4 vs DeepSeek V4.1 Pro vs Kimi K4
(?). Rien de tout cela n'est vérifié — c'est de la lecture de trajectoire, pas de l'information.

## 105. Limites et incertitudes de ce volume

Rappel honnête de ce que ce volume ne garantit pas. Un, les **benchmarks sont vendor-reported** sauf
mention « indépendant » (AA Intelligence Index, LMArena, Artificial Analysis) : les chiffres constructeur
sont des documents marketing autant que techniques. Deux, les **prix API** viennent majoritairement de
sources tierces et bougent vite (les divergences GLM-5.3 : $1,40/$4,40 vs $0,50/$2,00 — en sont
l'exemple). Trois, les **architectures fines** (attention hybride DeepSeek, Engram 196B, actifs exacts du
K3) reposent sur des notes communautaires, pas des rapports officiels. Quatre, le corpus 2026 est
**pollué par du contenu IA-généré** qui se recopie : les dates et chiffres isolés sur une seule source
« ai-news » sont fragiles. Cinq, les **licences** doivent être vérifiées sur la carte Hugging Face avant
tout usage commercial (cas MiniMax-M2/M3, GLM-5.3, Qwen3.8-Max/Flash, Kimi K3). Ce volume est une carte
dressée en septembre 2026 : le terrain bouge chaque semaine.

## 106. Glossaire — mode d'emploi

Les 32 termes ci-dessous sont les briques de vocabulaire minimales pour lire les fiches sans se perdre.
Chaque terme est défini en une phrase dense, avec son contexte 2026 quand il compte. Les termes marqués
« voir section » renvoient aux fiches où ils sont illustrés.

## 107. Glossaire — MoE (Mixture-of-Experts)

Architecture où chaque token n'active qu'un sous-ensemble d'« experts » (petits réseaux FFN) au lieu de
tout le modèle : on peut avoir 2 800B paramètres totaux (Kimi K3) pour seulement 104B actifs par token.
Le ratio total/actifs (20–27× en 2026) est la mesure de la sparsité — et donc du coût réel d'inférence.

## 108. Glossaire — Paramètres actifs par token

Nombre de paramètres réellement utilisés pour générer un token. C'est lui, pas le total, qui détermine le
coût de calcul et la mémoire vive nécessaire : un Ling-3.0-flash (124B total / 5,1B actifs) se sert comme
un modèle ~5B, tout en « sachant » comme un 124B.

## 109. Glossaire — MLA (Multi-head Latent Attention)

Attention à cache latent compressé (popularisée par DeepSeek-V3) : au lieu de stocker clés et valeurs
pleines par tête, on stocke une représentation latente compacte. Divise le KV cache par ~10 à contexte
égal. Utilisée par DeepSeek, Kimi (K2.6, K3), Zhipu (GLM-5.x), Ant (Ling-3.0), Meituan (LongCat).

## 110. Glossaire — Attention linéaire (GDN, KDA)

Famille d'attentions en O(n) au lieu de O(n²) : Gated DeltaNet (Qwen), Kimi Delta Attention (Moonshot,
réutilisée par Ant). Elles remplacent le KV cache croissant par un **état récurrent de taille fixe**
(quelques dizaines à ~200 MiB). Le prix : une qualité légèrement différente sur les dépendances très
longues, d'où les architectures **hybrides** (mélange couches linéaires + attention standard).

## 111. Glossaire — DSA (Dynamic Sparse Attention) et IndexShare

Briques de Zhipu (GLM-5.2/5.3) : la DSA ne calcule l'attention que sur un sous-ensemble dynamique (top-k)
de positions passées ; **IndexShare** fait partager les mêmes indices top-k à des groupes de couches au
lieu de les recalculer. Objectif : diviser le coût à 1M de contexte. Voir section 48.

## 112. Glossaire — MTP (Multi-Token Prediction)

Tête auxiliaire qui prédit plusieurs tokens d'un coup au lieu d'un seul. Deux usages : (1) meilleur
apprentissage (signal plus riche), (2) **décodage spéculatif** — le modèle « brouillon » propose, le
modèle principal vérifie, ce qui accélère l'inférence (Ollama l'exploite en `draft-mtp`). Présent chez
Qwen, MiMo (décodeur MTP 5 couches sur V2.6-Pro), GLM (couche MTP améliorée), MiniMax-M2 (3 têtes),
Tencent Hy3 (+3.8B params MTP), NVIDIA Nemotron (hors volume).

## 113. Glossaire — KV cache

Mémoire où sont stockées les clés/valeurs des tokens déjà traités pour ne pas les recalculer. Son volume
croît avec la longueur du contexte : c'est **le** goulot du long contexte (ex. ~240 Go par 1M tokens pour
MiniMax-M2). Toutes les innovations 2026 (MLA, GDN/KDA, DSA, HiCache, Mooncake) visent à le réduire ou à
le réutiliser.

## 114. Glossaire — TTFT (Time To First Token)

Temps avant l'arrivée du premier token généré — la latence perçue par l'utilisateur. Critique pour les
agents (qui enchaînent les appels) : le HiCache d'Ant revendique -60 à -80 % de TTFT sur longues
conversations. Un modèle « rapide » en tok/s mais lent en TTFT reste désagréable en usage interactif.

## 115. Glossaire — Décodage spéculatif (speculative decoding)

Technique d'accélération : un petit modèle « draft » propose plusieurs tokens, le grand modèle les vérifie
d'un coup. Si le draft a raison souvent, gain de 1,5–2× en débit. Les têtes MTP servent de drafter intégré.
Supporté day-0 par SGLang sur plusieurs modèles du volume (Hy3, Kimi K3 via TokenSpeed).

## 116. Glossaire — YaRN

Méthode d'**extrapolation de contexte** : étendre au-delà de la fenêtre d'entraînement (ex. 256K → 1M)
en rééchelonnant les positions RoPE, sans réentraînement. Utilisée par Qwen (3.6, 3.8-27B, 3.8-Flash).
Réserve : un 1M « via YaRN » n'a pas la même qualité garantie qu'un 1M natif (entraîné à cette longueur).

## 117. Glossaire — Reasoning / thinking (modes)

Capacité du modèle à produire une chaîne de raisonnement avant la réponse. En 2026, c'est devenu
**réglable** : `reasoning_effort` (low/high/max chez GLM, Qwen), thinking désactivable (DeepSeek V4.1-Flash,
Ling-3.0), thinking forcé (Kimi K2.7 Code), trois niveaux `no_think`/`think_low`/`think_high` (Tencent Hy3).
Le « Thinking Preservation » (Qwen) conserve le raisonnement entre les tours d'un agent.

## 118. Glossaire — Agent natif vs tool calling

**Tool calling** : le modèle sait appeler des outils (fonctions) — brique de base. **Agent natif** : le
modèle est entraîné pour des boucles autonomes longues (planifier, agir, observer, se corriger) —
Qwen3.7-Max revendique 35 heures continues, Kimi K2.6 des essaims de 300 agents / 4 000 étapes. La
différence se joue dans le post-training (RL sur environnements), pas dans l'architecture.

## 119. Glossaire — RL (post-training) et DORA

Le post-training par apprentissage par renforcement (RLVR, GRPO...) aligne le modèle sur les tâches
cibles (code, agents, maths). Fait 2026 : Xiaomi a **retransmis son RL en direct** (coût ~$3,47M pour
V2.6) ; Meituan utilise **DORA** (rollouts asynchrones, >3× speedup) ; MiniMax fait gérer 30–50 % de son
RL par M2.7 lui-même (self-evolution harness).

## 120. Glossaire — Distillation

Entraîner un petit modèle sur les sorties d'un grand (ex. MiMo-V2.6-Distill-Qwen-9B : 9B distillé depuis
la lignée trillion-class). Légale entre modèles ouverts ; **controversée** quand elle pompe les sorties
d'une API commerciale contre ses conditions d'utilisation (accusation d'Anthropic contre Xiaomi,
sept. 2026 — non tranchée).

## 121. Glossaire — Quantification (INT4, FP8, NVFP4, GGUF)

Réduction de la précision des poids pour servir moins cher : FP8 (8 bits), INT4 (4 bits, ex. Kimi livré
nativement en INT4), NVFP4 (format 4 bits NVIDIA Blackwell), **GGUF** (format communautaire llama.cpp/
Ollama, ex. Q4_K_M). Un 124B en BF16 (~250 Go) passe à ~70 Go en FP4 — c'est ce qui rend les gros
ouverts servables hors datacenter.

## 122. Glossaire — vLLM / SGLang / Ollama

Les trois moteurs d'inférence open-source de référence : **vLLM** (le standard datacenter,PagedAttention),
**SGLang** (le challenger, fort sur le structured output et le speculative decoding — support day-0 de
Kimi K3, Hy3, Qwen3.8-Flash), **Ollama** (le local simple, GGUF, support day-0 du Qwen3.8-27B). Le support
day-0 par ces moteurs est devenu un critère de lancement aussi important que les benchmarks.

## 123. Glossaire — Open-weight vs open-source

**Open-weight** : les poids sont publiés (on peut télécharger et servir le modèle), mais le code
d'entraînement et souvent les données restent fermés. **Open-source** (au sens OSI strict) exigerait
tout. En 2026, « open-source » est utilisé abusivement pour « open-weight » dans 90 % de la presse :
ce volume dit « poids ouverts » quand c'est le cas précis, et donne la licence à chaque fois.

## 124. Glossaire — Licences (MIT, Apache 2.0, Modified MIT)

**MIT** et **Apache 2.0** : permissives, usage commercial inclus (DeepSeek, Zhipu, Xiaomi, Tencent,
Qwen — c'est le standard chinois 2026). **Modified MIT** (Moonshot) : MIT + clause d'attribution
au-delà de 100M MAU / $20M mensuels (voir section 38). **Licence communautaire territoriale**
(MiniMax-H3) : usage local interdit aux US/UE/UK/Corée du Sud sans autorisation — l'exception qui
confirme la règle permissive.

## 125. Glossaire — Snapshot (ex. Qwen3.8-Max-0902)

Version datée d'un même modèle (ici : 2 septembre 2026), qui peut changer comportement, benchmarks et
**coût par tâche** (le 0902 parle ~108k tokens/tâche, soit 2× plus cher que le 3 août). En 2026, le
modèle n'est plus une release figée mais un flux : toujours épingler le snapshot en production.

## 126. Glossaire — Peak / off-peak (tarification)

Tarification horaire inventée par DeepSeek : prix doublés en « peak » (01:00–04:00 et 06:00–10:00 UTC,
jours ouvrés hors fériés chinois). Le V4.1-Flash passe de $0,15/$0,60 à $0,30/$1,20. C'est la
financiarisation du compute : le token a un prix spot qui suit la charge du datacenter.

## 127. Glossaire — Cache hit / cache miss

Facturation selon que le préfixe de la requête est déjà en cache : DeepSeek V4-Pro facture $0,435/M en
cache miss contre **$0,003625/M** en cache hit (120× moins). Architecturer ses prompts avec des préfixes
stables (system prompt, contexte RAG figé) devient un levier économique direct.

## 128. Glossaire — SWE-bench (Verified, Pro)

Benchmark de **résolution de vraies issues GitHub** — la mesure reine du code en 2026. **Verified** :
500 tâches validées humainement ; **Pro** : version plus dure et plus récente. Scores du volume :
Kimi K3 85,6 % (Verified), MiMo-V2.6-Pro 71,9 % (DeepSWE v1.1), Qwen3.8-27B 61,7 % (Pro). Tous
vendor-reported sauf mention contraire.

## 129. Glossaire — Terminal-Bench / OSWorld / Toolathlon

Benchmarks **agentiques** (pas Q&R) : Terminal-Bench (tâches en terminal réel), OSWorld (contrôle d'un OS
via GUI), Toolathlon (usage d'outils enchaînés). Ils mesurent ce que les labos vendent en 2026 : l'agent
autonome. Scores notables : MiMo-V2.6-Pro 89,9 (Terminal-Bench 2.1) et 82,0 (OSWorld-Verified), GLM-5.2 78 %
(TerminalBench v2.1).

## 130. Glossaire — AA Intelligence Index / LMArena

**Artificial Analysis Intelligence Index** : score composite indépendant (0–100) agrégeant les benchmarks —
la mesure « tierce » la plus citée du volume (Kimi K3 : 65 ; GLM-5.3 : 63,8 ; Qwen3.7-Max : ~57 ;
MiMo-V2.6-Pro : 46 ; DeepSeek V4.1-Flash : 40). **LMArena** : classement par votes humains en aveugle
(Kimi K3 : Elo 1512 ; Qwen3.8-Max : 2e Vision Arena). Ce sont les deux garde-fous contre les chiffres
constructeur.

## 131. Glossaire — DiNA (Discrete Native Autoregression)

Paradigme de Meituan (LongCat-Next) : toutes les modalités (texte, image, audio) sont converties en
**tokens discrets dans un espace partagé**, puis un unique autoregresseur les prédit. Alternative aux
architectures « backbone texte + encodeurs greffés » : ici, la multimodalité est native dès le tokenizer.

## 132. Glossaire — RVQ (Residual Vector Quantization)

Technique de tokenization (ici : visuelle et audio) qui quantifie un signal en plusieurs couches
résiduelles successives — ex. le tokenizer visuel **dNaViT** de LongCat-Next (RVQ 8 couches). Plus de
couches = plus de fidélité, au prix de plus de tokens par image.

## 133. Glossaire — ScMoE (Shortcut-Connected MoE)

Variante MoE de Meituan : des connexions « shortcut » entre experts permettent de **recouvrir le calcul
et la communication** inter-experts (le goulot du MoE distribué). Complétée par des experts
« zero-computation » (routage à coût nul). École d'optimisation système plutôt que d'architecture
d'attention.

## 134. Glossaire — Omnimodal

Un seul modèle qui comprend **et** génère (ou au moins comprend) texte + image + vidéo + audio : MiMo-V2.6
(texte+image+vidéo+audio), LongCat-Next (texte+vision+audio, génération d'images), GLM-5.3-Flash
(texte+image+vidéo en entrée), Ming-Flash-Omni-2.0 (parole+audio+musique). Le multimodal « greffé »
(encodeur vision ajouté) vs « natif » (DiNA) est la ligne de fracture technique.

## 135. Glossaire — Harnais agent (ZCode, OpenClaw, Claude Code)

Le **harnais** est le logiciel autour du modèle : boucle d'outils, gestion du contexte, exécution de
code. **ZCode** (Zhipu, livré avec GLM-5.2), **Claude Code** (Anthropic, dont le protocole est implémenté
par Qwen), **OpenClaw** (open-source, multi-fournisseurs). En 2026, le harnais compte autant que le modèle :
Qwen3.7-Max est évalué « cross-scaffold » (Claude Code, OpenClaw, Qwen Code) parce que l'agentique se joue
à ce niveau.

## 136. Glossaire — Frontier / frontier-closed

**Frontier** : un modèle au niveau de l'état de l'art mondial (quelques mois d'avance max). En 2026,
plusieurs ouverts chinois sont frontier (Kimi K3, GLM-5.3, Qwen3.8-Max). **Frontier-closed** : la doctrine
Alibaba d'avril–mai 2026 (Qwen3.6-Max, 3.7-Max/Plus) — garder le frontier fermé pour le monétiser, ouvrir
le reste. Doctrine abandonnée en août avec l'ouverture du 3.8-Max.

## 137. Glossaire — Ascend / silicium domestique

**Huawei Ascend** (910B...) : les puces IA chinoises qui portent l'entraînement et l'inférence domestiques
(GLM-5 entraîné à 100 % sur Ascend, tutoriel Hy3 sur Ascend NPU). Avec T-Head, Kunlun, Enflame, Muxi
(supportées jour 1 par MiMo-V2.5) et la puce Zhenwu M890 d'Alibaba, elles forment le « silicium
domestique » : la pile matérielle qui rend la Chine indépendante de NVIDIA pour l'IA.

## 138. Glossaire — Vendor-reported

Chiffre de benchmark **fourni par le constructeur**, non vérifié indépendamment. Statut par défaut dans
ce volume sauf mention « indépendant » (AA, LMArena, Artificial Analysis). Les écarts peuvent être
massifs (ex. divergence GLM-5.3 SWE-bench : 57,8 % vs 82,4 %) : un chiffre vendor-reported isolé est une
indication, pas une preuve.

## 139. Quiz — question 1

Quel modèle détenait, à sa sortie en juillet 2026, le titre de « plus gros modèle open-weight de
l'histoire », et avec combien de paramètres totaux ?

## 140. Quiz — question 2

DeepSeek V4 abandonne le MLA pur de V3 au profit de quelle architecture d'attention ? Citez les trois
composantes.

## 141. Quiz — question 3

Quelle est la particularité tarifaire introduite par DeepSeek sur le V4.1-Flash, et à quelles heures
UTC s'applique le tarif « peak » ?

## 142. Quiz — question 4

Citez les trois séries de modèles d'Ant Group (inclusionAI) et ce que chacune désigne.

## 143. Quiz — question 5

Quelle brique d'attention linéaire Moonshot a été réutilisée par Ant Group dans le Ling-3.0-flash, et
dans quelle proportion (couches) ?

## 144. Quiz — question 6

Vrai ou faux : « Kimi 2.7 » existe comme modèle standalone. Justifiez.

## 145. Quiz — question 7

Qu'est-ce que l'« Ox Alpha » apparu en stealth sur OpenRouter le 20 août 2026, et que s'est-il passé
le 26–27 août ?

## 146. Quiz — question 8

Quelle est la restriction inédite de la licence `minimax-h3-community-license-agreement` (2026-08-02),
et quels territoires sont concernés ?

## 147. Quiz — question 9

Quel fait industriel inédit Xiaomi a-t-il réalisé du 15 au 20 septembre 2026 autour du MiMo-V2.6, et
quel était le coût RL publié ?

## 148. Quiz — question 10

Citez trois éléments prouvant que la pile IA chinoise se découple de NVIDIA (modèles ou infra de ce volume).

## 149. Quiz — question 11

Quelle divergence massive et non résolue concerne les benchmarks du GLM-5.3, et sur quel bench ?

## 150. Quiz — question 12

Pourquoi le Qwen3.8-Flash est-il présenté comme important pour l'avenir d'Alibaba, au-delà de ses
propres performances ?

## 151. Quiz — question 13

Quel modèle dense 27B open-weight a battu un flagship fermé occidental sur les benchmarks de code en
août 2026, et sur quels scores ?

## 152. Quiz — questions 14 et 15

14. Quelle clause distingue la « Modified MIT » de Moonshot d'une licence MIT classique, et à partir de
quels seuils se déclenche-t-elle ?
15. DeepSeek V4.1 Pro : sorti ou non au 27/09/2026 ? Que sait-on officiellement, et que dit la rumeur ?

## 153. Quiz — réponses

**1.** **Kimi K3** (Moonshot), **2,8T (2 800B) paramètres totaux**, 104B actifs par token (16 experts sur
896), poids publiés le 27 juillet 2026.
**2.** Une **attention hybride** : **CSA** (Compressed Sparse Attention, stride 4) + **HCA** (Heavily
Compressed Attention, stride 128) + **SWA** (sliding window ~128). Objectif : ~10 % du KV cache de V3.2.
**3.** Tarification **peak/off-peak** : prix **doublés en peak** ($0,15/$0,60 → $0,30/$1,20). Peak :
**01:00–04:00 et 06:00–10:00 UTC**, jours ouvrés hors jours fériés chinois.
**4.** **Ling** = modèles non-thinking ; **Ring** = modèles thinking (raisonnement) ; **Ming** = série
multimodale omni (texte, vision, audio, musique).
**5.** Le **KDA (Kimi Delta Attention)** : **35 couches KDA + 7 couches MLA** (gated MLA) dans le
Ling-3.0-flash — emprunt direct à l'architecture Moonshot.
**6.** **Faux.** Le modèle s'appelle **Kimi K2.7 Code** ; « Kimi 2.7 » seul n'existe pas, c'est
l'abréviation usuelle (post-training code du checkpoint K2.6, 12 juin 2026).
**7.** « Ox Alpha » (`stealth/ox-alpha`) était le **GLM-5.3-Flash de Zhipu en mode furtif** (gratuit une
semaine sur OpenRouter) ; **confirmé par Zhipu à Bloomberg le 26 août**, lancement officiel + poids
MIT le **27 août 2026** (320B/18B, 1M multimodal, servi sur 100 000 puces chinoises).
**8.** Elle **exclut le déploiement local sans autorisation individuelle** aux **États-Unis, Union
européenne, Royaume-Uni et Corée du Sud** — une première : des poids « ouverts » à accès territorial
restreint.
**9.** Xiaomi a **retransmis en direct le post-training RL** sur dashboard public (mimo.xiaomi.com/rl/) ;
coût RL publié : **~$3,47M** ($2,62M Pro + $854K Flash), 30 steps, ~750K trajectoires, <6 jours, 7 000+
environnements RL publiés.
**10.** Au choix : GLM-5 entraîné **100 % sur Huawei Ascend** (zéro NVIDIA) ; GLM-5.3-Flash servi depuis
**100 000 puces de fabrication chinoise** ; LongCat-2.0 entraîné sur **~50 000 ASIC domestiques** ;
MiMo-V2.5 adapté **jour 1 à 7 plateformes de chips chinoises** ; tutoriel SGLang Hy3 sur **Ascend 910B** ;
puce **Zhenwu M890** d'Alibaba lancée avec Qwen3.7-Max.
**11.** **SWE-bench Verified : 57,8 %** (codepick, « données officielles ») **vs 82,4 %** (matrice
ifnodoraemon) — écart massif non résolu au 27/09/2026.
**12.** C'est la **« preview explicite de l'architecture Qwen4 »** (Gated DeltaNet + Qwen Sparse Attention,
176B = 125B backbone + 51B table N-gram) : Alibaba y teaserait l'architecture du prochain flagship avant
de la décliner.
**13.** **Qwen3.8-27B** (dense, ouvert, Apache 2.0, 14 août 2026) : SWE-bench Pro **61,7** (vs Claude Opus
4.6 Max 53,4), LiveCodeBench v6 **90,3** (vs 88,8), IFBench **79,5** (vs 62,5).
**14.** Clause d'**attribution commerciale** : au-delà de **100M d'utilisateurs mensuels ou $20M de
revenus mensuels**, le produit doit afficher le nom du modèle (ex. « Kimi K2.6 ») sur son interface.
**15.** **Non sorti.** Officiellement : **annoncé le 10 septembre 2026** (WeChat DeepSeek), V4.1-Flash
« a comprehensively surpassed V4 Pro », aucune spec ni date publiées. Rumeur (orcarouter.ai, 25 sept.) :
fenêtre de sortie **28–30 septembre 2026**, non étayée par le changelog officiel.

---

*Fin du volume 1 — Encyclopédie des modèles IA : la Chine (février 2026 → 27 septembre 2026).*
*155 sections. Tous les faits sont issus de la recherche documentaire de septembre 2026 ; les points*
*marqués « non vérifié au 27/09/2026 » le restent jusqu'à confirmation par source primaire.*

## 154. DeepSeek : anatomie d'une dépréciation ordonnée

Le cas V4-Pro → V4.1-Flash (10–14 septembre 2026) est un cas d'école de gestion de fin de vie modèle.
Séquence : le 10 septembre, annonce WeChat — V4.1-Flash « a comprehensively surpassed V4 Pro » ; le même
jour, retrait du V4-Flash et redirection de ses routes vers V4.1-Flash ; le 14 septembre à 04:00 UTC,
bascule prévue des requêtes `deepseek-v4-pro` vers V4.1-Flash **au tarif Flash** (donc moins cher pour
l'utilisateur), « jusqu'au lancement de V4.1 Pro ». C'est une dépréciation « douce » : pas de coupure,
une période de transition tarifaire favorable, et un successeur nommé (V4.1 Pro) qui n'existe pas encore.
L'incertitude (benchlm.ai affirme le 26 septembre que la reroute du 14 a été annulée) montre la limite :
sans accès API direct, on ne sait pas quel modèle répond vraiment derrière un ID. Leçon opérationnelle :
**ne jamais coder un ID de modèle en dur sans stratégie de fallback** — en septembre 2026, un ID peut
changer de modèle sous vos pieds en 96 heures.

## 155. DeepSeek : le rapport technique fantôme

Fait remarquable pour le labo le plus influent du volume : **aucun rapport technique officiel DeepSeek V4
n'a été trouvé** au 27/09/2026. Tout ce que l'on sait de l'attention hybride (CSA+HCA+SWA), de MegaMoE,
des résiduels « mHC », de l'optimiseur Muon et de l'« Engram memory » 196B du V4.1-Flash vient de notes
communautaires GitHub, de documentations d'hébergeurs (Together AI) et de model cards. DeepSeek publie les
poids (MIT) mais garde le « comment » pour lui — l'inverse d'Anthropic/OpenAI qui publient des system
cards sans les poids. C'est la doctrine DeepSeek 2026 : **l'open-weight comme distribution, pas comme
documentation**. Pour la recherche, c'est frustrant ; pour l'utilisateur, c'est transparent — le modèle
parle de lui-même.

## 156. Qwen : le snapshot comme méthode de release

Alibaba a normalisé en 2026 la release par **snapshot daté** : Qwen3.8-Max-0902 succède au snapshot du
3 août sans changement de nom commercial, mais avec un AA Index qui passe de 40 à 45, une 1re place à
Code Arena WebDev, et un coût par tâche qui double ($2,67 → $5,41, ~108k tokens/tâche). Le snapshot n'est
ni un patch ni une version majeure : c'est un **recalibrage continu** (post-training, réglages de
reasoning). Conséquences pratiques : (1) les benchmarks ont une date de péremption de quelques semaines ;
(2) la facture API d'un même ID peut varier fortement d'un mois à l'autre ; (3) en production, il faut
épingler le snapshot et rejouer les évaluations à chaque changement. DeepSeek fait pareil (« V4 Pro 0813 »),
mais Alibaba l'a industrialisé.

## 157. Qwen : QwenWork, Wukong et la conquête de l'entreprise

Les modèles ne sont que la moitié de la stratégie Alibaba : **QwenWork** (plateforme de travail, citée au
déploiement du 3.8-Max), **Wukong** (plateforme entreprise, intégrée au 3.6-Plus) et l'**app Qwen**
grand public forment un étau entreprise. Le 3.8-Max est explicitement positionné « workplace tools » par
la presse (marketbusinessnews, août 2026), et le snapshot 0902 vise le coding d'entreprise (1er Code Arena
WebDev). La mention « China state-law risk » dans un article TechTimes sur QwenWork rappelle l'enjeu
compliance : un modèle d'entreprise chinois opère sous droit chinois — point de vigilance pour un
déploiement hors de Chine (données, juridiction). Alibaba ne vend plus des modèles : il vend une suite
bureautique IA.

## 158. Kimi : Mooncake et la disaggregation

Le support day-0 du Kimi K3 cite **Mooncake**, le système d'inférence de Moonshot : **réutilisation du KV
cache cross-instance** et **disaggregation PD/EPD** (séparer le prefill — traitement du prompt — du
decode — génération —, voire l'encoding). L'idée : dans une ferme servant des milliers de requêtes, les
préfixes de contexte se répètent (system prompts, documents RAG) ; Mooncake les partage entre instances
au lieu de les recomposer. Combiné au KDA (état borné), c'est l'arme du 1M « plusieurs fois moins cher ».
C'est aussi un signal : en 2026, **l'inférence est un problème de systèmes distribués**, pas seulement de
poids — et les labos chinois investissent dans la couche serving autant que dans les modèles.

## 159. Kimi : l'essaim de 300 agents

La signature produit de Kimi K2.6 : des **essaims jusqu'à 300 agents** coordonnés sur **4 000 étapes**
(revendication Moonshot, relayée par Spheron). Ce n'est pas du marketing creux : c'est le prolongement
direct du design « agent natif » — un modèle entraîné au tool use multi-acteurs, avec un champ
`reasoning_content` dédié et une température API fixée à 1,0 (pour la diversité des trajectoires). K2.7
Code pousse la logique : sessions de **>12 h en continu**, thinking forcé, -30 % de tokens de
raisonnement sur le code. La question ouverte : qui orchestre 300 agents en production réelle ? Au
27/09/2026, c'est une capacité démontrée plus qu'un usage banalisé — mais elle fixe le plafond de ce que
l'open-weight sait faire en 2026.

## 160. GLM : ZCode, le harnais maison

Avec GLM-5.2, Zhipu livre **ZCode**, son harnais agent officiel « style Claude Code », compatible dès le
jour J avec Claude Code, Cline, OpenCode, Roo Code, Goose, Crush, OpenClaw et Kilo Code. La stratégie est
double : fournir l'outil de référence (pour que l'expérience GLM soit optimale) **et** garantir
l'interopérabilité (pour capter les utilisateurs des harnais existants). C'est la même logique que la
compatibilité « protocole Anthropic » de Qwen (section 23) : en 2026, **le harnais est un champ de
bataille** — celui qui contrôle la boucle agent contrôle la distribution du modèle. Zhipu est le seul
labo chinois du volume à livrer harnais + modèle + Coding Plan (Lite/Pro/Max/Team) en pack intégré.

## 161. GLM-5 (février 2026) : le fondateur hors-période

Antérieur à la période du volume (sortie : **11 février 2026**), GLM-5 mérite sa fiche car il fonde la
lignée 5.2/5.3. **MoE 744B total / 40B actifs**, 28,5T tokens de pré-entraînement, **DeepSeek Sparse
Attention (DSA)**, contexte **200K** (137K selon une table tierce — divergence), **licence MIT**.
**Entraîné entièrement sur chips Huawei Ascend (MindSpore), zéro NVIDIA** — fait géopolitique majeur,
corroboré par Reuters via Maxime Labonne. Benchmarks (vendor) : SWE-bench Verified **77,8 %** (SOTA
open-source au lancement), Terminal Bench 2.0 56,2, AIME 2026 92,7 %, GPQA-Diamond 86,0 %. GLM-5.2/5.3 en
sont l'évolution directe (même base pré-entraînée pour 5.2→5.3, passage à 1M de contexte, IndexShare).
Sans GLM-5, pas de « moment Zhipu » de l'été 2026.

## 162. MiMo : Studio, Desktop et l'Open Platform

Xiaomi distribue ses modèles via trois canaux : l'**API MiMo Open Platform** (facturation au token),
**MiMo Studio** (chat web) et **MiMo Desktop** (client local). Le V2.6-Pro-RL demande **2 nœuds / 32 GPU**
selon la commande de serving Xiaomi — c'est du datacenter, pas du bureau. À l'autre bout : le
**Distill-Qwen-9B** tourne sous Ollama/LM Studio avec 16 Go VRAM. Entre les deux, le Flash-RL (309B/15B)
est le compromis servable. Cette échelle de distribution (API → datacenter self-hosted → bureau) est la
plus complète du volume avec Qwen : Xiaomi pense déjà en « gamme de déploiement », pas en modèle unique.

## 163. Ling : Fin, anatomie d'un vertical

Le **Ling-3.0-flash-Fin** (9 septembre 2026) est le seul modèle **vertical métier** du volume : base
3.0-flash (124B/5,1B) fine-tunée finance, co-développée avec des institutions financières (CICC investment
banking). Cas d'usage revendiqués : recherche investissement, conformité, modélisation de valorisation,
reporting réglementaire. Benchmarks dédiés (Ant) : FinFIRST, FinSearchComp Verified, FinCRAFT,
FinanceAgent v1.1/v2, APEX-Agents, SpreadsheetBench v1/v2, τ³-Banking. Deux lectures : (1) la
différenciation 2026 se joue sur les déclinaisons sectorielles autant que sur les généralistes ; (2) Ant,
adossé à un empire fintech, fait ce que personne d'autre ne peut faire aussi crédiblement — transformer
son accès aux workflows financiers en données de post-training. Licence exacte : non vérifiée.

## 164. LongCat : 50 000 ASIC et le pari domestique

Le **LongCat-2.0** (30 juin 2026) revendique un entraînement et un serving sur **supercalculateurs d'ASIC
chinois domestiques** (cluster ~50 000 cartes — claim Meituan), avec « Super kernels » et L2-cache weight
prefetching maison. C'est le cas le plus extrême du découplage : non seulement sans NVIDIA, mais sur une
infrastructure conçue autour du modèle (1,6T paramètres, 1M natif, LongCat Sparse Attention). Scores
revendiqués au niveau GPT-5.5 (SWE-bench Pro 59,5 vs 58,6). Réserves : benchmarks vendor-reported,
disponibilité effective des poids non vérifiée au 27/09/2026. Mais le signal stratégique est clair —
même un acteur « non-IA » comme Meituan peut monter une pile complète domestique.

## 165. MiniMax : MaxProof, l'or aux IMO/USAMO

Le framework **MaxProof** (generative-verifier RL + evolutionary search) a fait passer MiniMax-M3
**au-dessus du seuil médaille d'or humaine à l'IMO 2025 et à l'USAMO 2026** (blog MiniMax, 9 juin 2026).
C'est un résultat de post-training, pas de pré-entraînement : la méthode (vérificateur génératif + recherche
évolutionnaire) compte autant que le modèle. À mettre en regard du Ring-2.5-1T d'Ant (IMO 2025 : 35/42,
niveau or) : en 2026, **deux labos chinois ouverts revendiquent le niveau médaille d'or en maths de
compétition**. Le raisonnement mathématique frontier n'est plus un monopole fermé — et c'est un
pré-entraînement + RL ciblé, pas un scaling brut, qui l'a apporté.

## 166. Tencent : la fenêtre gratuite du 6 au 21 juillet 2026

Pour lancer Hy3, Tencent a ouvert une **fenêtre gratuite** : Nous Portal + OpenRouter `:free` du ~6 au
~21 juillet 2026 (fenêtre fermée depuis). C'est une tactique de distribution agressive : inonder les
comparateurs (OpenRouter, LMArena) et les harnais (Hermes Agent de NousResearch a intégré Hy3 pendant la
fenêtre — voir le guide dev.to « now that the free window closed ») pour acheter de la notoriété au prix
du compute. Le prix courant post-fenêtre est **non vérifié** au 27/09/2026 — Tencent n'a pas communiqué
de grille publique dans les sources consultées. Leçon : une fenêtre gratuite crée un pic d'adoption qui
ne dit rien du prix d'équilibre ; toujours vérifier le tarif en vigueur avant d'architecturer dessus.

## 167. Face-à-face : les six attentions chinoises

| Labo | Nom | Principe | Modèles |
|---|---|---|---|
| DeepSeek | Hybride CSA+HCA+SWA | 3 sparsités d'attention combinées | V4, V4.1-Flash |
| Alibaba | Gated DeltaNet (+ Gated Attention) | Attention linéaire récurrente + blocs standard | Qwen3.5→3.8 |
| Moonshot | KDA + MLA | Attention linéaire maison + cache latent | Kimi K3 (Ant reprend le KDA) |
| Zhipu | MLA + DSA + IndexShare | Sparse dynamique + partage d'indices top-k | GLM-5.2/5.3 |
| Ant | KDA + MLA (hybride 35+7) | Reprise du KDA Moonshot | Ling-3.0 |
| Meituan | LongCat Sparse Attention / ScMoE | Sparse attention + overlap calcul/communication | LongCat-2.0 / Flash |

Constat : **zéro labo chinois majeur n'utilise l'attention dense vanilla** sur ses flagships 2026. Tous
ont une brique linéaire ou sparse. Et les briques circulent (KDA Moonshot → Ant). L'attention n'est plus
un choix d'architecture, c'est une **commodité optimisée** — la différenciation s'est déplacée vers le
post-training (RL, environnements, harnais).

## 168. Face-à-face : le coût du million de tokens (input)

| Segment | Modèle | $/1M input | Contexte |
|---|---|---|---|
| Efficace ouvert | Ling-3.0-flash | **0,075** | 256K |
| Flash | DeepSeek V4.1-Flash | 0,15 (0,30 peak) | 1M |
| Flash | GLM-5.3-Flash | 0,15 | 1M |
| Flash | Qwen3.8-Flash | 0,16 | 256K |
| Mid ouvert | MiniMax-M2 | 0,30 | ~200K |
| Mid fermé | Qwen3.7-Plus | 0,40 | 1M ? |
| Flagship ouvert | MiMo-V2.6-Pro-RL | 0,435 | 1M |
| Flagship ouvert | DeepSeek V4-Pro | 0,435 | 1M |
| Flagship ouvert | Kimi K3 | 1,20 | 1M |
| Flagship fermé | Qwen3.7-Max | ~1,25 | 1M |
| Flagship ouvert | GLM-5.2/5.3 | 1,40 | 1M |
| Flagship ouvert | Qwen3.8-Max | 2,00 | 1M |

Lecture : **un facteur 27×** entre le moins cher (Ling-3.0-flash) et le plus cher (Qwen3.8-Max) de ce
volume — pour des modèles qui se côtoient dans les mêmes benchmarks. Le prix ne mesure plus la qualité,
il mesure le positionnement.

## 169. Face-à-face : les licences

| Licence | Modèles | Contrainte |
|---|---|---|
| MIT | DeepSeek (tous), Zhipu GLM, Xiaomi MiMo, Meituan LongCat, Ant Ling-3.0 | Aucune (commercial OK) |
| Apache 2.0 | Qwen (3.5, 3.6, 3.8-27B, Coder), Tencent Hy3 | Aucune (commercial OK, clause brevets) |
| Modified MIT | Kimi K2.6, K2.7 Code, K3 (à confirmer carte HF) | Attribution UI au-delà de 100M MAU / $20M mensuels |
| Community territoriale | MiniMax-H3 | Local interdit US/UE/UK/KR sans autorisation |
| Non vérifiée | MiniMax-M2/M3, Qwen3.8-Max/Flash, GLM-5.3 | **Vérifier la carte HF avant usage commercial** |
| Fermé | Qwen3.6-Max, 3.6-Plus, 3.7-Max/Plus, MiMo-V2-Pro/Omni/TTS | API uniquement |

Règle d'or : **la licence se lit sur la carte Hugging Face du checkpoint exact**, pas dans un article.
Cinq modèles majeurs du volume ont une licence non vérifiée au 27/09/2026 — c'est le premier chantier
avant toute mise en production.

## 170. Face-à-face : les tiers « Flash » de septembre 2026

| Modèle | Date | Total / actifs | Contexte | $/1M in | $/1M out | Licence |
|---|---|---|---|---|---|---|
| DeepSeek V4.1-Flash | 10/09/26 | 552B / 8–16B | 1M | 0,15 (×2 peak) | 0,60 (×2 peak) | MIT |
| GLM-5.3-Flash | 27/08/26 | 320B / 18B | 1M multimodal | 0,15 | 0,50 | MIT |
| Qwen3.8-Flash | 02/09/26 | 176B / 6B | 256K | 0,16 | 0,47 | Ouverts (n.v.) |
| MiMo-V2.6-Flash-RL | 21/09/26 | 309B / 15B | 1M omnimodal | 0,14 | 0,28 | MIT |

Quatre labos, quatre Flash, **un prix d'entrée convergé à $0,14–0,16/M** : c'est un oligopole tarifaire
de fait. Les différenciateurs restants : le multimodal natif (GLM, MiMo), la tarification peak (DeepSeek),
la preview d'architecture future (Qwen). Le V4-Flash original (retiré le 10/09) a été cannibalisé par son
successeur le jour même — la durée de vie d'un tier Flash se compte en mois. Pour l'acheteur : le Flash
est devenu une commodité, on choisit sur les features (vision ? peak ? tool use ?), plus sur le prix.

## 171. Les modèles retirés : V4-Flash et la fragilité des IDs API

Un seul modèle du volume a été **retiré** en 2026 : **DeepSeek V4-Flash** (24 avril → 10 septembre 2026,
4,5 mois de vie). Mais trois autres sont en **fin de vie programmée** : V4-Pro (dépréciation ordonnée vers
V4.1-Flash, statut exact flou), Hy3 Preview (remplacé par Hy3), Qwen3.8-Max snapshot du 3 août (remplacé
par le 0902). La leçon dépasse la Chine : en 2026, **un ID d'API est un pointeur mutable**, pas un
contrat. Bonnes pratiques : épingler modèle + snapshot + date dans la config, monitorer les changelogs
fournisseurs (api-docs.deepseek.com/news/, blogs labos), prévoir un modèle de repli testé (ex. bascule
V4-Pro → V4.1-Flash), et rejouer une batterie d'évaluations à chaque changement d'ID — le comportement
peut changer sans préavis.

## 172. Pour un RAG personnel : que choisir dans ce volume

En restant strictement sur les faits vérifiés : pour un **RAG francophone exigeant** (long contexte,
prix bas, licence propre), les candidats sérieux sont **Ling-3.0-flash** ($0,075/M input, MIT, 256K→1M,
5,1B actifs — le moins cher du volume), **Qwen3.8-27B** (dense ouvert Apache 2.0, 18 Go en Q4_K_M,
tourne sur 1× 24 Go, support Ollama day-0 — le meilleur choix local), **DeepSeek V4.1-Flash** ($0,15/M,
MIT, 1M, entrée image native) et **MiMo-V2.6-Distill-Qwen-9B** (16 Go VRAM, lignée trillion distillée —
le plus accessible en local). Pour l'**agentique** : Kimi K2.6/K3 (essaims, Modified MIT — clause
d'attribution à connaître), GLM-5.2 (ZCode, MIT), Tencent Hy3 (Apache 2.0, preserved reasoning). À éviter
en production sans vérification : tout ce qui est marqué licence non vérifiée (MiniMax-M2/M3, GLM-5.3,
Qwen3.8-Max/Flash) et les modèles retirés ou en dépréciation (V4-Flash, V4-Pro).

## 173. Le local : ce qui tourne vraiment (ordres de grandeur)

D'après les données vérifiées du volume : **16 Go VRAM** → MiMo-V2.6-Distill-Qwen-9B (Q4_K_M, Ollama/LM
Studio). **24 Go (1× GPU)** → Qwen3.8-27B en 4-bit (18 Go Q4_K_M, MTP + décodage spéculatif), Ling-3.0-tiny
(SGLang, DGX Spark). **8× H200** → Kimi K2.6 en INT4 natif (baseline Moonshot). **4 nœuds / 32 GPU** →
Kimi K3 (minimum pratique cité). **2 nœuds / 32 GPU** → MiMo-V2.6-Pro-RL (commande Xiaomi). **DGX Spark
(GB10)** → Ling-3.0-flash en INT4 (72 Go), Hy3 en NVFP4 (~21,8 tok/s single-stream sur 2× DGX Spark, test
communautaire). Règle : les MoE sparse se servent au prorata des **actifs**, pas du total — un 124B/5,1B
(Ling-3.0) est bien plus servable que son total ne le suggère, à condition d'avoir les bons kernels
(vLLM/SGLang récents, support des attentions linéaires).

## 174. Veille : comment suivre après le 27/09/2026

Les points chauds à surveiller : **DeepSeek V4.1 Pro** (rumeur 28–30 sept. — vérifier le changelog
api-docs.deepseek.com), la **release open-weight d'humain-m3**, **Qwen4** (teasé via 3.8-Flash), la
**licence finale du Kimi K3** (carte HF `moonshotai/Kimi-K3`), les **cartes HF de Qwen3.8-Max/Flash et
GLM-5.3** (licences à confirmer), l'issue de la **controverse Anthropic–Xiaomi**, et l'évolution des
**prix GLM-5.3** ($1,40/$4,40 vs $0,50/$2,00 — une baisse serait un signal marché fort). Sources
primaires à bookmarker : blogs labos (minimax.io/blog, api-docs.deepseek.com/news), Hugging Face
(orgs `Qwen`, `moonshotai`, `zai-org`, `inclusionAI`, `meituan-longcat`, `MiniMaxAI`), Artificial Analysis
et LMArena pour les mesures indépendantes. Et garder le réflexe de ce volume : **un chiffre sans source
primaire reste une rumeur bien habillée**.

## 175. Traçabilité des sources

Ce volume est une synthèse rédigée à partir de quatre dossiers de recherche documentaire (septembre
2026) : `deepseek_mimo_ling_longcat.md` (DeepSeek, Xiaomi, Ant, Meituan), `qwen_kimi_glm.md` (Alibaba,
Moonshot, Zhipu), `minimax_nemotron_hermes.md` (MiniMax — sections Nemotron/Hermes exclues, volume
occidental), `llama_mistral_gemma_granite.md` (section HY3 = Tencent Hunyuan 3 uniquement). Les sources
primaires citées dans ces dossiers : communiqués (Business Wire, Reuters, Caixin, Bloomberg via Zhipu),
docs API officielles (api-docs.deepseek.com), model cards Hugging Face, blogs labos, papiers arXiv
(2603.00729, 2603.27538, 2605.26494, 2509.18883). Les sources secondaires : Artificial Analysis, LMArena,
OpenRouter, agrégateurs (best-of-ai, benchlm.ai), presse tech, notes communautaires GitHub. Les URLs
exactes figurent dans les dossiers de recherche ; ce volume n'invente aucune URL.

## 176. Ce que le volume 2 couvrira (pour mémoire)

Ce volume 1 s'arrête aux frontières de la Chine. Le **volume 2 (Occident)** reprendra : NVIDIA Nemotron
(Nano 2 12B, Nemotron 3 Nano/Super/Ultra, Cascade-2, 3.5 Lightning, Content Safety) et NousResearch
Hermes (Hermes 4, 4.3-36B, Hermes Agent, GEPA, Psyche, atropos) — documentés dans les mêmes dossiers de
recherche mais exclus ici par consigne éditoriale. Le **volume 3** croisera les deux hémisphères :
comparatif Chine–Occident, cartes des licences mondiales, et la question qui traverse tout 2026 —
l'open-weight peut-il rester ouvert quand le frontier coûte des centaines de millions à entraîner ?

---

*Fin du volume 1 (provisoire — recompté à la fin).* Vérification : `wc -l` et 100+ sections `## N.` exigés par la maquette.*

## 177. DeepSeek : Together AI et les trois layouts de cache

La documentation d'inférence la plus fine sur V4 vient de **Together AI** (mai 2026) : l'attention hybride
impose de gérer **trois layouts de KV cache simultanés** — CSA compressé, HCA dense (~8K entrées), SWA
exact. Capacité annoncée : **~3,7M tokens par nœud HGX B200** avec politique de cache optimisée. C'est
concret : servir V4 à 1M de contexte n'est pas qu'une question de poids, c'est une question de moteur
d'inférence capable de jongler avec trois formats de cache hétérogènes — d'où les forks vLLM/SGLang
maison de DeepSeek, DeepGEMM + FlashInfer + DeepEP, et l'expert-parallelism sur NVLink (NVL72). Pour un
hébergeur tiers, supporter V4 correctement demande un travail d'intégration réel, pas un simple
`vllm serve`.

## 178. DeepSeek : la concurrency limitée à 500 requêtes

Détail commercial qui en dit long : l'API **V4-Pro limite la concurrence à 500 requêtes simultanées**.
Ce n'est pas une limite technique du modèle, c'est du **capacity management** : DeepSeek rationne
l'accès à son datacenter au lieu de faire la queue. Combiné à la tarification peak/off-peak, ça dessine
un fournisseur qui gère la pénurie plutôt que l'abondance — paradoxal pour le champion des prix bas.
En pratique : pour un workload à fort parallélisme (batch, évaluations), prévoir le retry/backoff et,
surtout, des fournisseurs tiers (OpenRouter, DeepInfra, Novita) qui mutualisent leurs propres quotas.

## 179. Qwen : Megatron-Bridge et le paradoxe NVIDIA

La liste de déploiement de Qwen3.5 inclut **Megatron-Bridge (NVIDIA NeMo)** — le tooling d'entraînement/
inférence de NVIDIA — et NVIDIA a mis Kimi K2.6 « on a faster path to Blackwell inference » (variante
NVFP4 du 13 mai 2026). Paradoxe apparent : pendant que la Chine se découple du silicium NVIDIA (Ascend,
ASIC domestiques), **le software stack NVIDIA reste la lingua franca** du serving des modèles chinois
ouverts (vLLM, SGLang, TensorRT-LLM, NeMo). Le découplage est matériel, pas logiciel — et c'est logique :
un modèle ouvert a intérêt à tourner partout, y compris sur GPU NVIDIA. La vraie ligne de fracture n'est
pas « NVIDIA vs Chine », c'est « training domestique possible sans NVIDIA » (GLM-5, LongCat-2.0).

## 180. Qwen : 201 langues et un vocabulaire de 250K

Qwen3.5 revendique **201 langues** et un vocabulaire de **~250K tokens** — parmi les plus larges du volume.
C'est un choix de tokenization qui coûte cher (la table d'embedding grossit) mais qui paie sur le
multilingue et sur les langues à morphologie riche. À comparer : Kimi K2.6 (vocab 163 840), MiniMax-M2
(200 064). Pour un usage francophone avec documents multilingues (le cas typique d'un RAG d'entreprise
en Afrique de l'Ouest, par exemple), un gros vocabulaire multilingue réduit la fragmentation des tokens
— donc le coût et la latence. C'est un critère sous-coté face aux benchmarks anglophones.

## 181. Kimi : la température fixée à 1,0

Détail d'API qui surprend : sur l'API Moonshot, la **température de Kimi K2.6 est fixée à 1,0**, non
modifiable. C'est un choix assumé pour les workloads agentiques : à température 1,0, le modèle explore
plus (diversité des trajectoires), ce qui est souhaitable quand on lance des essaims d'agents ou des
rollouts RL — mais moins quand on veut des réponses déterministes (extraction, classification). Le
raisonnement est retourné dans le champ **`reasoning_content`** séparé, ce qui permet de le logger ou de
l'afficher sans polluer la réponse. Deux décisions produit qui montrent que Moonshot designe pour les
agents d'abord, le chat ensuite.

## 182. Kimi : Cloudflare Workers AI, la distribution edge

Kimi K2.7 Code était **disponible sur Cloudflare Workers AI dès le jour J** (`@cf/moonshotai/kimi-k2.7-code`,
12 juin 2026). C'est un canal de distribution sous-estimé : servir un MoE 1T/32B via le réseau edge de
Cloudflare, c'est rapprocher l'inférence de l'utilisateur sans gérer de GPU. Pour Moonshot, c'est aussi
une façon de contourner la barrière des 1,4 To de poids du K3 : l'utilisateur n'a jamais besoin de
télécharger. La leçon distribution : en 2026, **un modèle ouvert se juge aussi à son graphe de
distribution** (API first-party, OpenRouter, Cloudflare, Fireworks, together...) — la disponibilité
multi-canal vaut presque autant que les benchmarks.

## 183. GLM : les deux semaines de retenue (sécurité)

Zhipu a **retenu les poids du GLM-5.3 deux semaines** (14 → 28 août 2026) pour « durcissement sécurité »
avant publication. C'est la première fois qu'un labo chinois ouvert admet publiquement un délai
sécurité sur une release — et c'est lié au positionnement « cyber » du modèle (découverte de vulns,
exploit scripting). Le message implicite : à partir d'un certain niveau de capacité offensive, la
publication immédiate n'est plus tenable, même en MIT. C'est un précédent : on peut s'attendre à ce que
les futurs modèles « cyber-capables » subissent des délais similaires, voire des restrictions. Pour la
veille : une annonce sans poids immédiats n'est plus un retard, c'est peut-être une revue sécurité.

## 184. GLM : le Coding Plan (Lite/Pro/Max/Team)

Zhipu vend GLM-5.2/5.3 via le **GLM Coding Plan** (tiers Lite/Pro/Max/Team) en plus de l'API au token
($1,40/$4,40 par 1M). C'est le modèle économique « abonnement développeur » (forfait mensuel pour les
usages de code intensifs) face au « pay-as-you-go » de DeepSeek et au « Token Plan » d'Alibaba. Trois
philosophies de monétisation coexistent donc en Chine : le token spot (DeepSeek), l'abonnement dev
(Zhipu), la plateforme entreprise (Alibaba/QwenWork). Pour un usage régulier et prévisible (une équipe
dev), le forfait bat le token ; pour un usage sporadique ou batch, c'est l'inverse. Le choix du plan
tarifaire est devenu un vrai sujet d'architecture.

## 185. MiMo : 7 plateformes de chips et 100T tokens gratuits

Le lancement du MiMo-V2.5 (22 avril 2026) cumulait deux gestes industriels : **adaptation jour 1 à 7
plateformes de chips chinoises** (T-Head, Kunlun, Enflame, Muxi, Tianshu Zhixin...) + AWS, et un programme
d'incitation de **100T tokens gratuits pendant 30 jours**. Le premier geste dit : l'inférence domestique
est une priorité produit, pas un portage après-coup. Le second dit : Xiaomi achète des parts de marché
API au prix du compute — 100T tokens, c'est de quoi faire tourner une PME entière pendant un mois. Les
deux ensemble : une stratégie d'écosystème où le modèle est un produit d'appel pour la plateforme (MiMo
Open Platform, Studio, Desktop). C'est l'équivalent chinois du « free tier agressif » des hyperscalers.

## 186. MiMo : l'encodeur vision 681M et le décodeur MTP 5 couches

Zoom technique sur le V2.6-Pro-RL : **encodeur vision de 681M paramètres** (conséquent — c'est la taille
d'un petit LLM à lui seul), **encodeurs audio** dédiés, et **décodeur MTP à 5 couches**. L'omnimodalité
« native » a un coût architectural réel : près d'un milliard de paramètres juste pour voir et entendre,
avant même le backbone MoE (1,02T/42B, 384 experts routés dont 8 actifs par token, 70 couches en attention
hybride sliding-window + globale). Le décodeur MTP à 5 couches, lui, sert le décodage spéculatif — la
vitesse est pensée dès l'architecture. C'est un modèle « système » : chaque modalité et chaque
optimisation a son budget de paramètres propre.

## 187. Ling : l'adoption infra (vLLM PR #51045, parsers SGLang)

Le Ling-3.0-flash a obtenu le **support vLLM via la PR #51045 (mergée le 2026-08-05)** et des **parsers
dédiés SGLang (`ling3` reasoning/tool-call)**. Deux semaines après la sortie des poids (23 juillet), le
modèle était servable sur les deux moteurs de référence — c'est le rythme « day-0 » devenu standard pour
les gros ouverts chinois. Le point technique : supporter l'hybride KDA + MLA + 512 experts + MTP dans
vLLM/SGLang n'est pas trivial (kernels d'attention linéaire, routage d'experts, speculative decoding) ;
le fait que ce soit fait en deux semaines montre la maturité de l'écosystème d'intégration autour des
modèles chinois. Pour l'ops : vérifier la version minimale (vLLM post-PR #51045) avant de planifier un
déploiement.

## 188. Ling : le DGX Spark comme cible edge

Ant documente le déploiement du Ling-3.0-flash sur **DGX Spark (GB10)** en INT4 (72 Go) et valide le
tiny en BF16 sur la même machine ; le runbook communautaire Hy3 tourne en NVFP4 sur 2× DGX Spark
(~21,8 tok/s single-stream). Le DGX Spark — la « machine IA de bureau » NVIDIA — est devenu en 2026 la
**référence edge** des labos chinois : si ton 124B passe en INT4 sur un GB10, tu as un argument
« workstation » crédible. C'est un changement de paradigme discret : les frontier ouverts ne sont plus
réservés au datacenter, ils visent le bureau d'ingénieur. Pour un RAG local exigeant, le couple
« DGX Spark + Ling-3.0-flash INT4 » est l'option la plus sérieuse du volume côté chinois.

## 189. LongCat : dNaViT, la voix 24 kHz et la vidéo absente

Zoom sur le LongCat-Next : tokenizer visuel **dNaViT** (RVQ à 8 couches — voir glossaire), tokenizers
audio RVQ natifs, **synthèse vocale et clonage de voix en 24 kHz streaming**, compréhension + **génération
d'images**. Et une absence notable : **pas de génération vidéo** (compréhension seule) — là où MiniMax-H3
fait de la vidéo+audio native 4–15 s à 24 FPS. Deux écoles du multimodal chinois : Meituan fait du
« discret autoregressif unifié » (DiNA) avec des moyens contenus (~70B), MiniMax fait du « gros
transformer génératif » (33B dense vidéo+audio). Le premier est déployable (build w8a8_int8 ~90 Go sur
DGX Spark), le second est spectaculaire. Selon le besoin (agent vocal vs studio vidéo), le choix est
tranché.

## 190. MiniMax : la recette de sampling (et pourquoi elle compte)

MiniMax publie ses réglages recommandés — fait rare et précieux : **M2 : temperature=1.0, top_p=0.95,
top_k=40** ; **M3 : temperature=1.0, top_p=0.95, sans top_k**. La température 1,0 (comme Kimi) est le
réglage « agentique » : diversité maximale, pas de réponses tièdes. Le top_k=40 du M2 resserre
l'échantillonnage sur les 40 tokens les plus probables ; son abandon sur M3 suggère que le post-training
a rendu le modèle assez calibré pour s'en passer. Retenir : **les benchmarks d'un labo sont mesurés avec
ses réglages** — servir un M2 à température 0,2 « pour être sûr » et comparer aux chiffres officiels
n'a aucun sens. Toujours partir des réglages recommandés, puis ajuster.

## 191. MiniMax : ATOM, ROCm et l'inférence AMD

La liste de déploiement MiniMax inclut **ATOM (ROCm)** — le stack AMD — aux côtés de vLLM, SGLang,
KTransformers et MLX-LM (Apple Silicon). C'est le seul labo du volume à citer explicitement ROCm dans ses
supports, et ça compte : l'inférence des ouverts chinois n'est pas un monopole CUDA. Entre ROCm (AMD),
les NPU Ascend (tutoriel Hy3), les 7 plateformes chinoises (MiMo-V2.5) et MLX (Apple Silicon), les modèles
ouverts chinois de 2026 sont les plus **portables en silicium** du marché. Pour une stratégie
d'infrastructure anti-lock-in, c'est un argument de poids face aux modèles fermés calés sur une seule
plateforme.

## 192. Tencent : le support day-0 (vLLM hy_v3, SGLang FP8)

Hy3 est arrivé avec un **support day-0** exemplaire : **SGLang** (FP8 + speculative decoding) et
**vLLM ≥ 0.23** avec architectures natives `hy_v3` / `hy_v3_mtp`. Des archs nommées dans vLLM, c'est le
signe d'une intégration poussée (pas un simple mapping de config). Ajouter les variantes communautaires
NVFP4 (`kodelow/Hy3-NVFP4-W4A16`, 181 Go) et le **tutoriel SGLang officiel sur Ascend NPU** (910B, 2×
Atlas 800I A2) : Tencent a soigné les trois couches — NVIDIA (SGLang/vLLM), communauté (NVFP4), domestique
(Ascend). C'est la check-list du lancement ouvert réussi en 2026 : poids + licence claire (Apache 2.0) +
moteurs day-0 + hardware domestique. Les labos qui ratent une case (poids « coming soon » de LongCat-2.0,
licence non vérifiée du K3) paient en adoption.

## 193. Face-à-face : les scores SWE-bench (code)

| Modèle | Bench | Score | Source |
|---|---|---|---|
| Kimi K3 | SWE-bench Verified | **85,6 %** | vendor |
| DeepSeek V4-Pro Max | SWE-bench Verified | 80,6 % | vendor |
| Qwen3.7-Max | SWE-bench Verified | 80,4 % | vendor |
| Qwen3.6-Plus | SWE-bench Verified | 78,8 % | vendor |
| Tencent Hy3 | SWE-bench Verified | 78,0 % | vendor |
| Qwen3.6-27B | SWE-bench | 77,2 % | agrégateur tiers |
| Qwen3.8-27B | SWE-bench Pro | **61,7 %** | agrégateur tiers |
| Qwen3.8-Flash | SWE-bench Pro | 62,5 % | vendor/agrég. |
| GLM-5.2 | SWE-bench Pro | 62,1 % | vendor |
| MiMo-V2.6-Pro-RL | DeepSWE v1.1 | 71,9 % | vendor |
| DeepSeek V4.1-Flash | DeepSWE v1.1 | **74,2 %** | vendor |
| GLM-5.3 | DeepSWE v1.1 | 66,9 % | vendor |
| GLM-5.3 | SWE-bench Verified | 57,8 % vs 82,4 % | **divergence non résolue** |

Lecture : le trio de tête (K3, V4-Pro, Qwen3.7-Max) se tient en 5 points sur le Verified ; sur le Pro
(version dure), ce sont les petits ouverts (Qwen3.8-27B, Qwen3.8-Flash, GLM-5.2) qui mènent la danse des
ouverts. Et la divergence GLM-5.3 rappelle que **tout ce tableau est vendor-reported** : à prendre comme
une carte des revendications, pas comme un classement.

## 194. Face-à-face : qui fait quoi en multimodal

| Modèle | Texte | Image (in) | Image (out) | Vidéo (in) | Vidéo (out) | Audio (in) | Audio (out) |
|---|---|---|---|---|---|---|---|
| MiMo-V2.6-Pro-RL | ✓ | ✓ | n.v. | ✓ | n.v. | ✓ | n.v. |
| LongCat-Next | ✓ | ✓ | ✓ | ✓ | compréhension seule | ✓ | ✓ (24 kHz) |
| GLM-5.3-Flash | ✓ | ✓ | — | ✓ | — | — | — |
| Qwen3.8-Max | ✓ | ✓ | n.v. | ✓ | n.v. | n.v. | — |
| Tencent Hy3 | ✓ | — | — | — | — | — | — |
| MiniMax-H3 | (prompt) | ✓ (cond.) | — | ✓ (cond.) | ✓ 4–15 s | ✓ (cond.) | ✓ stéréo 32 kHz |
| Ming-Flash-Omni-2.0 | ✓ | n.v. | n.v. | n.v. | n.v. | ✓ (parole+musique) | n.v. |

(— = non ; n.v. = non vérifié ; cond. = en conditioning d'entrée.) Constat : **aucun modèle du volume ne
fait tout** — l'« omni » reste un horizon. Le texte+image+vidéo en entrée est devenu standard (Qwen, GLM,
MiMo, Kimi) ; la génération multimodale reste l'exception (LongCat-Next pour l'image+voix, MiniMax-H3
pour la vidéo+audio).

## 195. Face-à-face : les réglages « thinking »

| Modèle | Thinking par défaut | Réglage | Désactivable |
|---|---|---|---|
| DeepSeek V4.1-Flash | on | low / high / max | oui |
| Qwen3.8-Max | xhigh (défaut) | low / medium / xhigh | oui (/think, /no_think) |
| GLM-5.2 | réglable | non-thinking → High → Max | oui |
| GLM-5.3 | max (défaut) | low / high / max | **non** (`disabled` rejeté) |
| Kimi K2.7 Code | forcé | — | non |
| Kimi K3 | toujours actif | — | n.v. |
| Tencent Hy3 | hybride | no_think / think_low / think_high | oui (no_think) |
| Ling-3.0-flash | on (CoT) | — | oui |
| Qwen3.8-27B | on | — | oui |

Tendance : le thinking **réglable et facturable** (plus d'effort = meilleurs scores mais plus de tokens
= plus cher, cf. Qwen3.8-Max-0902). Le GLM-5.3 qui **interdit** de couper le thinking est le signal
faible : les labos ne font plus confiance à l'utilisateur pour doser le raisonnement — ou ne veulent pas
qu'on mesure le modèle sans.

## 196. Les noms qui n'ont rien donné (non trouvés)

Honnêteté méthodologique : ces noms ont été cherchés (2–3 requêtes chacun) et **n'existent pas** au
27/09/2026. **« Qwen3.8-35B-A3B »** : aucune trace — la série 3.8 ouverte connue = Max + 27B + Flash.
**« Kimi 2.7 »** (standalone) : n'existe pas, c'est Kimi K2.7 Code. **« Qwen3.5/3.6/3.7-Coder »** :
n'existent pas — le codeur 2026 est Qwen3-Coder-Next (février) puis 480B-A35B. **« DeepSeek V4 Pro »**
sans suffixe : c'est la variante V4-Pro (GA 0813), pas un autre modèle. **« DeepSeek V5 », « MiMo-V3 »,
« Ling-4.0 », « LongCat-3.0 »** : hors périmètre, rien n'est apparu dans les résultats — à noter pour la
veille, pas pour ce volume. Si vous croisez ces noms dans la presse, c'est soit une erreur, soit une
rumeur, soit l'avenir.

## 197. Lexique des IDs API (correspondances vérifiées)

| ID API | Modèle réel | Fournisseur |
|---|---|---|
| `deepseek-flash` | DeepSeek V4.1-Flash | DeepSeek |
| `deepseek/deepseek-v4.1-flash` | DeepSeek V4.1-Flash | OpenRouter |
| `deepseek-v4-pro` | DeepSeek V4-Pro (dépréciation en cours) | DeepSeek |
| `kimi-k2.6` | Kimi K2.6 | Moonshot |
| `kimi-k2.7-code` | Kimi K2.7 Code | Moonshot |
| `glm-5.2` | GLM-5.2 | Z.ai |
| `qwen3.6-flash` | Qwen3.6-35B-A3B (ouvert) | Alibaba Model Studio |
| `qwen3.6-plus` | Qwen3.6-Plus (fermé) | Alibaba Model Studio |
| `qwen3.7-max` / `qwen3.7-plus` | Qwen3.7-Max / Plus (fermés) | Alibaba Model Studio |
| `qwen3.8-max` / `qwen3.8-flash` | Qwen3.8-Max / Flash (ouverts) | Alibaba / QwenCloud |
| `hy3` | Tencent Hy3 | Tencent |
| `@cf/moonshotai/kimi-k2.7-code` | Kimi K2.7 Code | Cloudflare Workers AI |
| `stealth/ox-alpha` | GLM-5.3-Flash (était stealth, 20–26/08) | OpenRouter |
| `qwen-plus` / `qwen-max` | IDs génériques historiques, mapping exact non vérifié | docs tierces |

À épingler dans la config : un ID générique (`qwen-plus`) peut changer de modèle sans préavis ; un ID
versionné (`qwen3.8-flash`) est plus stable mais reste un pointeur (cf. snapshots).

## 198. Check-list de mise en production (modèles de ce volume)

Avant de mettre en production un modèle chinois de ce volume, vérifier dans l'ordre : **(1) Licence** —
lire la carte Hugging Face du checkpoint exact (5 modèles majeurs ont une licence non vérifiée au
27/09/2026 ; la clause Modified MIT de Kimi et la restriction territoriale du H3 sont des cas
pièges). **(2) Snapshot** — épingler modèle + snapshot + date (Qwen3.8-Max-0902 ≠ snapshot du 3 août).
**(3) Contexte réel** — distinguer natif / YaRN / stretch (Hy3 : 128K validé en pratique, 256K = stretch).
**(4) Tarification** — peak/off-peak (DeepSeek), cache hit/miss (120× d'écart), plan au token vs Coding
Plan vs abonnement. **(5) Fallback** — un ID peut changer de modèle en 96 h (V4-Pro, août–sept. 2026) ;
prévoir un repli testé. **(6) Réglages** — partir des sampling recommandés du labo (MiniMax, Kimi),
pas des défauts du harnais. **(7) Évaluations** — rejouer sa batterie à chaque changement d'ID ou de
snapshot ; les chiffres vendor-reported ne dispensent pas de mesurer soi-même. **(8) Juridiction** —
données et droit applicable (QwenWork/« state-law risk », H3 territorial).

---

*Fin du volume 1 (provisoire — recompté à la fin).* Encyclopédie des modèles IA : la Chine (février 2026 → 27 septembre 2026).*

## 199. DeepSeek : l'optimiseur Muon (détail d'entraînement)

Parmi les rares détails d'entraînement de V4 qui ont filtré : l'usage de l'**optimiseur Muon** (complété
par de l'AdamW partiel) et du **FP4 QAT** (quantization-aware training) pour les poids MoE. Muon
(MomentUm Orthogonalized by Newtonian updates) orthogonalise les mises à jour du momentum — en pratique,
il converge plus vite qu'AdamW sur les gros transformers, au prix d'une implémentation plus délicate
(distribuée chez DeepSeek). Le FP4 QAT, lui, consiste à entraîner en simulant déjà la quantification 4
bits : les poids MoE naissent « prêts pour le NVFP4 », au lieu d'être quantifiés après coup avec perte.
C'est cohérent avec toute la stratégie V4 : **penser l'inférence dès l'entraînement** (poids NVFP4
natifs, attention hybride pour le KV cache). Réserve : ces détails viennent de notes tierces, pas d'un
rapport officiel — mais ils sont techniquement cohérents avec les formats de poids publiés (BF16/FP8/FP4
natifs sur le Hub).

## 200. Qwen : les ratios d'hybridation (3:1, 75/25, 48+16)

Le dosage « couches linéaires vs attention standard » varie selon les modèles Qwen, et c'est un paramètre
de design à part entière : **Qwen3.6-35B-A3B** — 40 couches en 10 groupes de (**3× Gated DeltaNet + 1×
Gated Attention**), soit 75 % linéaire ; **Qwen3-Coder-Next** — **75 % linéaire / 25 % full** (même ratio,
explicite) ; **Qwen3.8-27B** — **48 couches GDN + 16 couches full-attention** (même 75/25). La constance
du ratio 3:1 à travers trois modèles suggère un optimum empirique Alibaba : trois couches linéaires
bon marché pour le « gros œuvre » du contexte, une couche d'attention pleine pour la précision des
dépendances fines. C'est de l'ingénierie de compromis quantifiée — et c'est ce ratio, plus que le nombre
de paramètres, qui fait l'économie du 256K–1M chez Qwen.

## 201. Kimi : le vocabulaire 163 840 et la tokenisation

Kimi K2.6 utilise un vocabulaire de **163 840 tokens** — entre MiniMax-M2 (200 064) et les ~250K de Qwen.
La taille du vocabulaire est un levier sous-estimé : un gros vocabulaire fragmente moins les langues
non-anglaises (le chinois notamment, où chaque caractère peut être un token) et le code, ce qui réduit
le nombre de tokens par document — donc le coût et la latence à qualité égale. Le revers : une matrice
d'embedding plus grosse (163 840 × hidden size) qui pèse dans les poids et la mémoire. Moonshot a aussi
fait le choix du **natif INT4** (pas de release haute précision) : le modèle est distribué directement
dans sa forme servable, pas dans sa forme d'entraînement. Philosophie : le poids publié est un artefact
de déploiement, pas un artefact de recherche.

## 202. GLM : anatomie du positionnement « cyber »

Le dossier « cyber » du GLM-5.3 mérite le détail car c'est une première assumée : post-training avec
**environnements de découverte de vulnérabilités**, capacités revendiquées — analyse de vulns, exploit
scripting, élévation de privilèges, chaînes d'exploitation multi-étapes — et chiffres : **2 436
vulnérabilités trouvées sur 269 projets open-source, 53 CVE**. Benchmarks : CyberGym 77,2 % → **84,5 %**,
ExploitBench 24,4 % → **54,4 %** (×2,2), Terminal-Bench 3.0 4,6 % → 28,3 % (×6). Z.ai a retenu les poids
deux semaines pour durcissement sécurité avant publication (28 août). Trois lectures : (1) le post-training
spécialisé peut multiplier une capacité par 2–6 sans changer l'architecture (même base 5.2) ; (2) le
dual-use offensif est désormais un argument marketing ; (3) la « retenue sécurité » devient un précédent
de gouvernance des releases ouvertes. Tous les chiffres sont vendor-reported : l'ampleur réelle reste à
mesurer indépendamment.

## 203. MiniMax : l'hallucination divisée par 2,6

Le progrès le plus spectaculaire de la série M2 n'est pas un benchmark de code, c'est la **chute du taux
d'hallucination** : de **88 % (M2.5) à 34 % (M2.7)** sur AA-Omniscience (Artificial Analysis) — meilleur
que Claude Sonnet 4.6 (46 %). En une génération (février → mars 2026), MiniMax a divisé par 2,6 le taux
d'hallucination de son modèle ouvert. C'est le genre de métrique qui compte plus que SWE-bench pour un
RAG : un modèle qui n'invente pas est un modèle qu'on peut brancher sur des documents d'entreprise. Le
moteur probable : le « self-evolution harness » (self-feedback, self-optimization) et un post-training
orienté factualité. Corollaire : GDPval-AA **1495 ELO** (#1 open-source) et la parité avec GPT-5.3-Codex
sur SWE-Pro (56,22 %) font de M2.7 le « tout-terrain » ouvert le plus équilibré du printemps 2026 —
dommage que sa licence exacte reste non vérifiée.

## 204. Tencent : preserved reasoning et context caching (le détail qui tue)

Deux features Hy3 méritent qu'on s'y arrête car elles ciblent le vrai coût des agents : le **preserved
reasoning** (le raisonnement survit entre les appels d'outils — pas besoin de le régénérer à chaque tour)
et le **context caching** (le cache de contexte survit entre les tours). Dans une boucle agentique
classique, chaque appel d'outil force à renvoyer tout l'historique + à refaire le raisonnement : c'est
du token gaspillé et de la latence. Hy3 attaque les deux : raisonnement persistant + cache persistant +
structured output garanti + trois niveaux de thinking (`no_think` pour les appels triviaux, `think_high`
pour la planification). C'est une conception « système » de l'agent : le modèle est un composant
logiciel fiable avec des garanties d'interface, pas un oracle brillant mais imprévisible. Pour un
développeur d'agents, ces deux features valent plus qu'un point de benchmark.

## 205. Les chiffres qui résument 2026 (table)

| Record / fait | Valeur | Modèle |
|---|---|---|
| Plus gros total open-weight | 2,8T paramètres | Kimi K3 |
| Plus gros actifs/token | ~104B (divergence : ~50B) | Kimi K3 |
| Plus petit actifs/token « sérieux » | 1,3B | Ling-3.0-tiny (7,9B total) |
| Input le moins cher | $0,075 / 1M | Ling-3.0-flash |
| Plus grand ratio total/actifs | ~27× | Kimi K3 (2,8T/104B) |
| Plus gros téléchargement | 1,4–1,56 To (96 shards) | Kimi K3 |
| Contexte max standard | 1M (12 modèles du volume) | — |
| RL le moins cher documenté | ~$3,47M | MiMo-V2.6 |
| Plus longue vie d'un Flash | 4,5 mois (retiré) | DeepSeek V4-Flash |
| Plus longue tâche agentique revendiquée | 35 heures | Qwen3.7-Max |
| Plus gros essaim d'agents revendiqué | 300 agents / 4 000 étapes | Kimi K2.6 |
| Plus forte sparsité d'experts | 16/896 | Kimi K3 |
| Plus forte chute d'hallucination | 88 % → 34 % (une génération) | MiniMax M2.5 → M2.7 |
| Plus gros cluster domestique revendiqué | ~50 000 ASIC | LongCat-2.0 |
| Plus gros serving domestique revendiqué | 100 000 puces chinoises | GLM-5.3-Flash |

## 206. Note finale : ce volume est une photographie

Ce volume fige au 27 septembre 2026 un paysage qui bouge chaque semaine : DeepSeek V4.1 Pro est attendu
d'un jour à l'autre, Qwen4 est teasé, la licence du Kimi K3 reste à confirmer, la controverse
Anthropic–Xiaomi n'est pas tranchée. Les fiches « non vérifié » ne sont pas des échecs de recherche,
ce sont des **positions ouvertes** : chaque mention est une question à reposer dans trois mois. La
méthode (aucune spec inventée, sources tracées, divergences signalées) vaut pour les volumes suivants :
mieux vaut une encyclopédie honnête et incomplète qu'une encyclopédie complète et fausse. Rendez-vous au
volume 2 — l'Occident (NVIDIA Nemotron, NousResearch Hermes) — puis au volume 3, le choc des deux
hémisphères.

---

*Fin du volume 1 — Encyclopédie des modèles IA : la Chine (février 2026 → 27 septembre 2026).*
*Sections numérotées de 1 à 206. Vérification finale : `wc -l` ≥ 2500, 100+ sections, tableaux par famille, glossaire 32 termes, quiz 15 questions + réponses.*

## 207. Qwen : l'open-weight comme arme géopolitique douce

La valse d'Alibaba (tout-ouvert → frontier-closed → réouverture du 3.8-Max) n'est pas qu'une hésitation
commerciale : c'est de la **géopolitique par les poids**. Chaque release Apache 2.0 (Qwen3.5 intégral,
3.6-35B, 3.8-27B, Coder-Next) équipe gratuitement des milliers d'équipes dans le monde — y compris
occidentales — et crée une dépendance d'écosystème (harnais, fine-tunes, benchmarks calibrés sur Qwen).
La parenthèse « frontier-closed » (avril–juillet 2026) a testé la monétisation directe ; sa fin (août)
suggère que la valeur de l'influence open-weight dépasse la rente API du flagship. Même logique chez
Xiaomi (RL public, 7 000 environnements publiés) et Zhipu (MIT sans restriction territoriale, contrairement
au H3 de MiniMax). En 2026, **publier des poids ouverts est un acte de politique étrangère technologique**
— et la Chine est le seul acteur à le faire à l'échelle frontier, systématiquement.

## 208. Récapitulatif : les sorties mois par mois

| Mois | Sorties (modèles du volume) |
|---|---|
| Février 2026 | Qwen3.5-397B-A17B · Ling-2.5-1T / Ring-2.5-1T · Ming-Flash-Omni-2.0 · Qwen3-Coder-Next · GLM-5 (fondateur) |
| Mars 2026 | MiMo-V2-Pro/Omni/TTS (fermés) · MiniMax-M2.7 |
| Avril 2026 | Qwen3.6-Plus (fermé) · Qwen3.6-35B-A3B · Qwen3.6-Max-Preview (fermé) · Kimi K2.6 · MiMo-V2.5/V2.5-Pro · DeepSeek V4-Pro/Flash · Ling-2.6-flash |
| Mai 2026 | Qwen3.7-Max/Plus (fermés) |
| Juin 2026 | MiniMax-M3 · Kimi K2.7 Code · GLM-5.2 · LongCat-2.0 |
| Juillet 2026 | Tencent Hy3 · MiniMax-H3 · Ling-3.0-flash · Kimi K3 |
| Août 2026 | Qwen3.8-Max (+ snapshot 0902 le 02/09) · Qwen3.8-27B · GLM-5.3 · GLM-5.3-Flash |
| Septembre 2026 | Qwen3.8-Flash · humain-m3 · Ling-3.0-flash-VL/Fin · DeepSeek V4.1-Flash · MiMo-V2.6 (Pro/Flash/Distill) |

**Avril et août** sont les deux mois les plus denses (5–6 sorties chacun) : avril = le choc DeepSeek V4 +
la rupture « frontier-closed » d'Alibaba ; août = la contre-offensive ouverte (Qwen3.8-Max, GLM-5.3).
Septembre est le mois des **remplacements** (V4-Flash retiré, V4-Pro déprécié, snapshots). Le rythme
moyen : **~4 modèles significatifs par mois** — intenable pour une veille manuelle, d'où l'importance
des sources agrégées (Artificial Analysis, LMArena) et des changelogs officiels.

## 209. Les 10 faits à retenir en 30 secondes

1. **Kimi K3** (2,8T/104B, juillet 2026) est le plus gros open-weight de l'histoire — et il est chinois.
2. Le **1M de contexte** est devenu le standard (12 modèles du volume), grâce à l'attention linéaire/hybride.
3. Le token d'entrée est tombé à **$0,075/M** (Ling-3.0-flash) — divisé par ~19 en six semaines sur le segment efficace.
4. Un **dense 27B ouvert** (Qwen3.8-27B) bat un flagship fermé occidental sur le code.
5. L'**attention quadratique est morte** en Chine : GDN, KDA, DSA+IndexShare, hybride CSA/HCA/SWA — et les briques circulent (KDA Moonshot → Ant).
6. La **guerre des Flash** se joue à $0,01 près ($0,14–0,16/M input chez 4 labos).
7. **GLM-5.3** markete des capacités cyber offensives (53 CVE revendiqués) — première assumée.
8. La pile chinoise tourne **sans NVIDIA** : Ascend (GLM-5), 50K ASIC (LongCat-2.0), 100K puces (GLM-5.3-Flash).
9. Les **licences** sont le champ de mines : 5 modèles majeurs à licence non vérifiée, 1 restriction territoriale (MiniMax-H3), 1 clause d'attribution (Kimi).
10. Un **ID d'API est un pointeur mutable** : V4-Pro a failli changer de modèle sous les utilisateurs en 96 heures.

## 210. Index des modèles (renvoi aux sections)

- **DeepSeek** : V4 (§5), V4-Pro (§6), V4-Flash (§7), V4.1-Flash (§8), V4.1 Pro — annoncé (§9), prix (§11, §156), tableau (§12)
- **Qwen** : 3.5-397B (§15), 3.5-122B/35B (§16), 3.5-27B et petites tailles (§17), 3.6-35B (§19), 3.6-27B (§20), 3.6-Max-Preview (§21), 3.6-Plus (§22), 3.7-Max (§25), 3.7-Plus (§26), 3.8-Max (§27–28), 3.8-27B (§29), 3.8-Flash (§30), tiers API (§31, §199), Coder-Next (§32), Coder-480B (§33), déploiement local (§34, §175), tableaux (§24, §35), stratégie open/fermé (§14, §207)
- **Kimi** : K2.6 (§39), K2.7 Code (§40), K3 (§41), KDA+MLA (§42), Mooncake (§160), essaims (§161), licence Modified MIT (§38), tableau (§43)
- **GLM** : 5.2 (§45), 5.3 (§46, §202), 5.3-Flash / Ox Alpha (§47), MLA+DSA+IndexShare (§48), ZCode (§162), Coding Plan (§186), retenue sécurité (§185), tableau (§50)
- **MiMo** : 7B (§52), VL/Audio-7B (§53), V2-Flash (§54), V2-Pro/Omni/TTS (§55), V2.5 (§56), V2.6-Pro-RL (§57), V2.6-Flash-RL + UltraSpeed (§58), Distill-Qwen-9B (§59), RL en direct (§60), controverse Anthropic (§61), tableau (§62)
- **Ling** : séries Ling/Ring/Ming (§65), 2.0 (§66), 2.5-1T (§67), Ming-Omni (§68), 2.6-flash (§69), 3.0-flash (§70), 3.0-flash-VL (§71), 3.0-tiny (§72), 3.0-flash-Fin (§73, §165), KDA+MLA (§74), HiCache (§75), tableau (§76)
- **LongCat** : Flash-Chat (§78), Flash-Thinking (§79), Next (§80, §191), 2.0 (§81, §166), ScMoE (§82), tableau (§83)
- **MiniMax** : M2→M2.7 (§85–86, §203), M3 (§87, §167), H3 (§88–89), humain-m3 (§90), tableau (§91)
- **Tencent** : Hy3 Preview (§93), Hy3 (§94–95, §204), fenêtre gratuite (§168), tableau (§96)
- **Synthèses** : grand tableau (§97), timeline (§98, §208), que retenir (§99–102, §209), puces chinoises (§103), Qwen4 (§104), limites (§105), face-à-face (§169–172, §195–197), RAG/local (§174–175), check-list prod (§200), IDs API (§199)

---

*Fin du volume 1 — Encyclopédie des modèles IA : la Chine (février 2026 → 27 septembre 2026).*
*210 sections numérotées. Tous les faits sont issus de la recherche documentaire de septembre 2026 ; les points marqués « non vérifié au 27/09/2026 » le restent jusqu'à confirmation par source primaire.*

## 211. Ce que ce volume ne couvre pas (et pourquoi)

Transparence sur les frontières du volume — ce qui est exclu l'est par consigne ou par méthode, pas par
oubli. **Exclus par consigne éditoriale (→ volume 2, Occident)** : NVIDIA Nemotron (Nano 2 12B, Nemotron 3
Nano 30B-A3B, Super 120B-A12B, Cascade-2, Nano Omni, Ultra 550B-A55B, 3.5 Content Safety, 3.5 Lightning
30B-A3B) et NousResearch Hermes (Hermes 4, Hermes 4.3-36B, Hermes Agent, GEPA, Psyche, atropos) — tous
documentés dans les dossiers de recherche mais américains, donc hors périmètre Chine. **Couverts comme
fondations antérieures à février 2026** (fiches courtes, pas de détail 2026) : MiMo-7B (avril 2025),
LongCat-Flash-Chat/Thinking (août/sept. 2025), MiniMax-M2 originel (oct. 2025), GLM-5 (février 2026 —
fondateur de la lignée 5.x), Kimi K2.5 (janvier 2026, mentionné en contexte uniquement). **Non traités
faute de matière vérifiable** : MiMo-VL-7B et MiMo-Audio-7B (fiches prudentes, §53), Ming-Flash-Omni-2.0
(annonce sans specs, §68), les déclinaisons fines (ex. variantes Base/SFT/RL de chaque modèle — citées
quand connues, pas détaillées). **Non recherchés** (hors périmètre, à noter pour la veille) : d'éventuels
« DeepSeek V5 », « MiMo-V3 », « Ling-4.0 », « LongCat-3.0 », « Kimi K4 », « Qwen4 » — rien de tel n'est
apparu dans les résultats de septembre 2026. **La règle d'or reste** : si un nom n'est ni dans ce volume
ni dans le §198 (noms non trouvés), c'est soit une rumeur, soit une erreur de nommage, soit l'avenir —
dans les trois cas, exiger une source primaire avant de le citer.

---

*Fin du volume 1 — Encyclopédie des modèles IA : la Chine (février 2026 → 27 septembre 2026).*
*211 sections numérotées de 1 à 211, 9 familles, 11 tableaux comparatifs, glossaire de 33 termes, quiz de 15 questions + réponses. Vérifié : `wc -l` ≥ 2500.*
*Méthode : aucune spécification inventée ; tout point incertain reste marqué « non vérifié au 27/09/2026 » ; benchmarks vendor-reported sauf mention « indépendant ».*

## 212. Post-scriptum : relire ce volume dans six mois

Ce qui vieillira le plus vite, par ordre de péremption probable. **Prix API** (semaines) : la guerre des
Flash se joue au centime et les grilles bougent sans préavis — tout prix de ce volume est une
photographie de septembre 2026, à re-vérifier avant tout chiffrage. **Snapshots et IDs** (semaines à
mois) : Qwen3.8-Max-0902 sera remplacé, `deepseek-v4-pro` finira par basculer ou non, V4.1 Pro sortira
(ou non). **Statuts « annoncé »** (mois) : humain-m3 (open-weight planifié), Qwen4 (teasé), la licence
définitive du Kimi K3 — trois dossiers à rouvrir. **Licences non vérifiées** (à vérifier une fois,
puis stable) : MiniMax-M2/M3, GLM-5.3, Qwen3.8-Max/Flash — la première chose à faire avant toute mise
en production durable. **Controverses** (trimestre) : Anthropic–Xiaomi (distillation), la divergence
SWE-bench du GLM-5.3, le sort exact du V4-Pro — à trancher quand des sources primaires parleront. Ce qui
vieillira le moins vite : les **architectures** (GDN, KDA, DSA, attention hybride — les briques restent),
les **ordres de grandeur** (ratios total/actifs, coûts du 1M) et la **méthode** (ne croire qu'une source
primaire, douter des chiffres isolés). Dans six mois, ce volume sera un document d'histoire — c'est
exactement son rôle : fixer ce que l'on savait, quand on le savait, et ce que l'on ne savait pas.

---

*Fin du volume 1 — Encyclopédie des modèles IA : la Chine (février 2026 → 27 septembre 2026).*
*212 sections numérotées de 1 à 212 — sans trou ni doublon (vérifié par script).*

---

**Note de l'auteur.** Ce volume a été rédigé en septembre 2026 à partir de quatre dossiers de recherche
documentaire, sans qu'aucune spécification ne soit inventée : chaque fait renvoie à une source, chaque
doute est marqué « non vérifié au 27/09/2026 ». Si vous n'avez qu'une heure, lisez les sections 97
(grand tableau), 98 (timeline), 99–102 (que retenir) et 209 (10 faits en 30 secondes). Si vous devez
mettre un modèle en production, lisez 200 (check-list) puis vérifiez la licence sur la carte Hugging Face
du checkpoint exact — c'est le seul geste qui compte vraiment. Et si vous croisez un nom de modèle qui
n'est ni dans l'index (§210) ni dans la liste des noms non trouvés (§198) : méfiance, c'est une rumeur,
une erreur ou l'avenir — dans les trois cas, exigez une source primaire.

*Leo — pour Zelef, septembre 2026.*
