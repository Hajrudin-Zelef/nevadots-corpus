# Passerelles et infra LLM — OpenRouter, Groq, Cerebras, gratuit, self-hosting, TEI, HF, location GPU

> Guide terrain pour Zelef — chef de service systèmes & énergies, sysadmin, RAG personnel (embeddings `text-embedding-3-small` d'OpenAI).
> Objectif : accéder aux LLM au meilleur coût (idéalement 0 €), comprendre les passerelles, savoir héberger soi-même, et dimensionner l'infra.
> Données vérifiées par recherche web le **27/09/2026**. Tout prix non re-vérifié ce jour est marqué « à vérifier ». Aucune clé réelle : exemples fictifs (`sk-...FAKE`).
> Conventions : prix en $/1M tokens sauf mention ; TPS = tokens/seconde ; ctx = fenêtre de contexte.

---

## 1. Comment lire ce guide

1. **Parties A–C** : les trois passerelles rapides (OpenRouter, Groq, Cerebras) — fiches complètes : principe, modèles vérifiés sept 2026, prix, exemples d'appels, limites.
2. **Partie D** : le comparatif qui tranche — quand utiliser quoi, avec ton angle budget.
3. **Partie E** : `freellmapi` (identifié, c'est un vrai projet) + tout l'écosystème **gratuit réel** en 2026, limites chiffrées.
4. **Partie F** : `DeepSeek Harness` (identifié, vrai outil sorti août 2026) + **self-hosting DeepSeek/LLM local** en profondeur : Ollama, vLLM, llama.cpp, quantization, dimensionnement VRAM.
5. **Partie G** : TEI (Text Embeddings Inference) — servir tes embeddings en local, lien direct avec ton RAG.
6. **Partie H** : Hugging Face (Hub, Inference Providers, transformers, datasets, Spaces) — trouver et tester un modèle pour ton RAG.
7. **Partie I** : Vast.ai et RunPod — louer du GPU au lieu d'acheter, pas à pas, comparatif, pièges.
8. **Parties J–L** : stratégie budget, noms non identifiés (honnêteté), glossaire, quiz, pièges, cas pratiques.

## 2. Le paysage en une page (sept 2026)

| Couche | Acteurs | Coût typique | Quand |
|---|---|---|---|
| Passerelle multi-modèles | OpenRouter | 0 à $10/$50 /1M selon modèle | Comparer N modèles, fallback auto |
| Inférence ultra-rapide | Groq (LPU), Cerebras (WSE) | Gratuit (quotas) puis $0.05–$0.85 /1M in | Temps réel, agents, voix |
| Agrégateur de gratuit | freellmapi (self-hosted) | 0 (tiers gratuits empilés) | Expérimentation perso |
| API directes | OpenAI, Anthropic, DeepSeek, Mistral, Google | $0.10 à $50 /1M | Qualité max, SLA, support |
| Local | Ollama, vLLM, llama.cpp | 0 (ton élec + GPU) | Confidentialité, volume, hors-ligne |
| Embeddings locaux | TEI + bge/jina | 0 | Ton RAG sans facture OpenAI |
| GPU à la demande | Vast.ai, RunPod | $0.13–$3.89 /h | Fine-tune, batch, tests lourds |

## 3. Règle d'or du budget (ta contrainte)

1. Commence toujours par le **gratuit** : Groq free tier + Gemini Flash + OpenRouter `:free` + Cerebras free tier couvrent 95 % des usages d'étude et de dev.
2. Ne paie que ce que le gratuit ne fait pas : gros contextes, modèles fermés (GPT-6, Claude), SLA.
3. Mesure avant d'optimiser : logge tokens in/out par appel dès le jour 1 (section 133).
4. Le local (Ollama/TEI) bat tout le monde au-delà d'un certain volume — le seuil se calcule (section 134).

---

# PARTIE A — OPENROUTER

## 4. OpenRouter : présentation

OpenRouter (openrouter.ai) est une **passerelle unifiée** : une seule clé API, un seul endpoint compatible OpenAI (`https://openrouter.ai/api/v1`), et derrière **plus de 300 modèles** de dizaines de fournisseurs (OpenAI, Anthropic, Google, DeepSeek, Meta, Mistral, Qwen, Z.ai, NVIDIA, xAI…).
Tu changes de modèle en changeant une chaîne de caractères — aucun changement de code. C'est l'outil idéal pour **comparer des modèles** sur ton cas d'usage réel et pour mettre en place des **fallbacks** automatiques.
Modèle économique : prépayé (crédits), facturation au token, petite marge au-dessus du prix fournisseur. Pas d'abonnement obligatoire.

## 5. OpenRouter : principe technique

- Endpoint : `POST https://openrouter.ai/api/v1/chat/completions` — même format que l'API OpenAI.
- Auth : `Authorization: Bearer <ta-clé>`.
- Le nom du modèle suit le format `fournisseur/modele` : `anthropic/claude-sonnet-4`, `deepseek/deepseek-v4.1-flash`, `openai/gpt-oss-20b`, `google/gemma-4-31b-it`.
- Variante `:free` : certains modèles ont une route gratuite (`z-ai/glm-5.2:free`), soumise à quotas.
- Route spéciale `openrouter/free` : routeur qui choisit un modèle gratuit au hasard par requête.
- Routage fournisseur : par défaut OpenRouter choisit le fournisseur (le plus rapide / le moins cher) ; tu peux forcer ou ordonner les fournisseurs via le paramètre `provider`.
- Liste des modèles en direct : `GET https://openrouter.ai/api/v1/models` (pas de clé requise) — c'est LA source de vérité pour prix et disponibilité.

## 6. OpenRouter : modèles disponibles vérifiés (sept 2026)

Relevé de sources tierces ayant interrogé l'API live fin août / sept 2026 (prix susceptibles d'évoluer — vérifier sur `openrouter.ai/models/<id>`) :

| Modèle (id) | Contexte | Prix in/out par 1M (vérifié fin août 2026) | Note |
|---|---|---|---|
| `deepseek/deepseek-v4.1-flash` | 1M | $0.15 / $0.60 (hors peak) | Sorti 10/09/2026 ; peak en semaine 01h–04h et 06h–10h UTC : $0.30/$1.20 |
| `deepseek/deepseek-v4-flash-vision-exp` | 1M | $0.22 / $0.66 | Vision expérimentale |
| `qwen/qwen3.8-flash` | 1M | $0.15 / $0.47 | |
| `z-ai/glm-5.3-flash` | 1,3M | **$0.075 / $0.25** (promo lancement) | Prix cassé temporaire |
| `z-ai/glm-5.2` | 1M | $0.95 / $3.00 | |
| `z-ai/glm-4.6` | 203K | $0.50 / $2.00 | |
| `inception/mercury-2.5` | 260K | $0.04 / $0.15 (promo 80 % au lancement) | Tarif normal $0.20/$0.75 — promo ≠ permanent |
| `google/gemma-4-26b-a4b-it` | 262K | $0.06 / $0.33 (route dekallm/bf16) | |
| `mistralai/mistral-small-4` | ctx à vérifier | $0.15 / $0.60 | Bon support JSON/schema |
| `nvidia/nemotron-3.5-lightning` | 1M | $0.08 / $0.20 (route BF16) | |
| `anthropic/claude-sonnet-4` | 200K | $3 / $15 | Référence code |
| `openai/gpt-4o` | 128K | $2.50 / $10 | Ancien mais stable |
| `meta-llama/llama-3.1-405b` | 128K | $3 / $3 | Poids ouverts, gros |

Points d'attention : les prix **varient selon le fournisseur routé** et selon l'heure (DeepSeek a des fenêtres peak). Les promos de lancement (GLM, Mercury) expirent.

## 7. OpenRouter : modèles gratuits (`:free`) vérifiés

Relevé live du 21/08/2026 (`pricing.prompt == "0"`) — la liste bouge, re-vérifier via l'API :

| id | Contexte | Note |
|---|---|---|
| `nvidia/nemotron-3-ultra-550b-a55b:free` | 1M | 550B MoE, le plus gros modèle gratuit du catalogue |
| `z-ai/glm-5.2:free` | 256K | |
| `nvidia/nemotron-3.5-lightning:free` | 1M | |
| `openai/gpt-oss-20b:free` | 131K | Poids ouverts OpenAI |
| `google/gemma-4-31b-it:free` | 262K | |
| `cohere/north-mini-code:free` | 256K | Code |
| `poolside/laguna-s-2.1:free` | 262K | Code |
| `dots-studio/dots-3-note-preview:free` | 512K | |
| `openrouter/free` | 200K | Routeur aléatoire entre modèles gratuits |
| `stealth/ox-alpha` | 1M | Reasoning obligatoire |

**Quotas free** (vérifié sept 2026, sources concordantes) : **50 requêtes/jour** sur les modèles gratuits, **20 req/min** ; passe à **1 000 req/jour** si le compte a déjà acheté au moins $10 de crédits (achat unique, effet permanent). Limite au niveau du compte, tous modèles gratuits confondus.

## 8. OpenRouter : tarification et facturation

- Prépaiement par carte : tu charges des crédits ($5 min constaté — à vérifier), la conso décrémente.
- **Zéro Data Retention (ZDR)** disponible au niveau organisation : aucun entraînement sur tes données, utile en contexte pro.
- **Garde-fous budgétaires** : limites de dépense par clé (quotidienne/hebdo/mensuelle, reset auto) — la limite journalière sert de coupe-circuit anti-dérapage.
- Chaque réponse API inclut la génération et le coût ; le dashboard détaille par modèle.
- Marge OpenRouter : petite surcouche vs prix direct fournisseur — à volume élevé, comparer avec l'API directe.
- BYOK : tu peux apporter tes propres clés fournisseurs via OpenRouter (1M requêtes/mois gratuites puis +5 %) — utile pour centraliser sans changer de facturation.

## 9. OpenRouter : routage et providers

Par défaut, OpenRouter route vers le fournisseur le plus pertinent (vitesse/prix/disponibilité). Contrôle fin via le champ `provider` :

```json
{
  "model": "meta-llama/llama-3.1-70b-instruct",
  "messages": [{"role": "user", "content": "Bonjour"}],
  "provider": {
    "order": ["Groq", "Together", "DeepInfra"],
    "allow_fallbacks": true,
    "require_parameters": true,
    "data_collection": "deny"
  }
}
```

- `order` : fournisseurs préférés dans l'ordre.
- `allow_fallbacks: true` : si le premier tombe, bascule auto sur le suivant — **le cœur du pattern fallback**.
- `require_parameters: true` : n'utilise que des fournisseurs supportant les paramètres demandés (JSON mode, tools…).
- `data_collection: "deny"` : exclut les fournisseurs qui entraînent sur les données.
- `GET /api/v1/models/<id>/endpoints` : liste les fournisseurs réels derrière un modèle, avec prix et latence — indispensable avant de figer un choix.

## 10. OpenRouter : exemple d'appel (curl)

```bash
curl https://openrouter.ai/api/v1/chat/completions \
  -H "Authorization: Bearer sk-or-v1-FAKEFAKEFAKEFAKE" \
  -H "Content-Type: application/json" \
  -H "HTTP-Referer: https://mon-lab.local" \
  -H "X-Title: test-rag" \
  -d '{
    "model": "z-ai/glm-5.2:free",
    "messages": [{"role": "user", "content": "Explique le routage OSPF en 3 phrases."}],
    "provider": {"allow_fallbacks": true}
  }'
```

`HTTP-Referer` / `X-Title` : optionnels, servent au classement public des apps. En Python, le client `openai` suffit : `base_url="https://openrouter.ai/api/v1"`.

## 11. OpenRouter : exemple Python (comparer 3 modèles)

```python
from openai import OpenAI

client = OpenAI(
    base_url="https://openrouter.ai/api/v1",
    api_key="sk-or-v1-FAKEFAKEFAKEFAKE",
)

MODELES = [
    "z-ai/glm-5.2:free",
    "nvidia/nemotron-3.5-lightning:free",
    "deepseek/deepseek-v4.1-flash",
]
question = "Donne la commande VRP Huawei pour créer un VLAN 10."

for m in MODELES:
    r = client.chat.completions.create(
        model=m,
        messages=[{"role": "user", "content": question}],
        provider={"allow_fallbacks": True},
    )
    print("=" * 60)
    print(m, "->", r.choices[0].message.content[:300])
```

Même code, trois moteurs : c'est exactement le cas d'usage « comparer des modèles » pour ton RAG (qualité des réponses générées à partir de tes chunks).

## 12. OpenRouter : cas d'usage recommandés

1. **Banc d'essai de modèles** : tester 10 modèles sur 50 questions de ton domaine (réseau, onduleurs) et mesurer qualité/coût avant de figer.
2. **Fallback production** : modèle principal + 2 secours via `allow_fallbacks` — ton app survit à une panne fournisseur.
3. **Routage par complexité** : petit modèle gratuit pour le triage/classification, gros modèle payant pour la synthèse finale.
4. **Veille modèles** : l'API `/models` te dit qui sort quoi et à quel prix, sans créer 15 comptes.

## 13. OpenRouter : limites et pièges

1. **Latence ajoutée** : un saut réseau de plus vs l'API directe (quelques dizaines de ms) — négligeable sauf voix temps réel.
2. **Prix variables** : le même `model` peut coûter différemment selon le provider routé et l'heure (cf. DeepSeek peak). Fixe `provider.order` si tu veux de la prédictibilité.
3. **Disponibilité des `:free`** : les modèles gratuits sont les premiers délestés en surcharge ; prévois un fallback payant pas cher.
4. **Contexte effectif** : le contexte annoncé est celui du modèle, mais certains providers le tronquent — vérifier via `/info`.
5. **Données** : par défaut, certains providers peuvent logger ; `data_collection: "deny"` + ZDR pour du contenu sensible.
6. **50 req/jour** en gratuit : c'est un quota d'essai, pas une infra — pour du volume gratuit, empile avec Groq/Cerebras/Gemini (partie E).

## 14. Checklist OpenRouter (mise en route en 10 min)

- [ ] Créer un compte, générer une clé (`sk-or-v1-...`).
- [ ] Charger $10 de crédits (débloque 1 000 req/jour sur les `:free`).
- [ ] Tester `GET /api/v1/models` et filtrer `pricing.prompt == "0"`.
- [ ] Lancer le script de comparaison (section 11) sur 5 questions de ton domaine.
- [ ] Fixer `provider.order` + `allow_fallbacks` pour ton modèle de prod.
- [ ] Poser une limite de dépense quotidienne par clé (garde-fou).
- [ ] Activer ZDR si données pro.

---

# PARTIE B — GROQ

## 15. Groq : présentation (ne pas confondre avec Grok de xAI)

Groq (groq.com, fondée 2016) ne fabrique pas de modèles : elle conçoit des **puces d'inférence**, les **LPU (Language Processing Unit)**, et vend l'inférence dessus via GroqCloud. Positionnement : **la vitesse**. Là où un GPU fait ~30–80 tok/s sur un 70B, les LPU annoncent **275 à 1 000 tok/s** selon le modèle.
API 100 % compatible OpenAI : `https://api.groq.com/openai/v1`. Inscription sans carte bancaire, **free tier permanent** (rate-limité, jamais facturé : au-delà, erreur 429, pas de facture surprise).

## 16. Groq : pourquoi c'est rapide (LPU)

- Architecture **déterministe** : le compilateur connaît le graphe du modèle à l'avance, pas d'ordonnanceur dynamique comme sur GPU → latence ultra-faible et **débit prévisible**.
- Mémoire SRAM énorme sur la puce, bande passante interne très élevée → le décodage auto-régressif (memory-bound sur GPU) s'envole.
- Conséquence pratique : **time-to-first-token** de l'ordre de la centaine de ms et génération à 500–1 000 tok/s sur les petits modèles.
- Revers : catalogue limité aux modèles **open-weight** compilés pour LPU (pas de GPT-6 ni de Claude ici) ; fenêtres de contexte parfois réduites vs l'original.

## 17. Groq : modèles servis vérifiés (sept 2026)

État relevé fin août 2026 (le catalogue tourne vite — vérifier sur `console.groq.com/docs/models`) :

| Model ID exact | Contexte | Complétion max | Vitesse constatée | Prix payant /1M (vérifié mai–août 2026) |
|---|---|---|---|---|
| `openai/gpt-oss-120b` | 131 072 | 65 536 | ~500 tok/s | $0.15 / $0.60 |
| `openai/gpt-oss-20b` | 131 072 | 65 536 | ~1 000 tok/s | $0.075 / $0.30 |
| `qwen/qwen3.6-27b` | 131 072 | 16 384 | à vérifier | $0.60 / $3.00 |
| `qwen/qwen3.8-27b` | 131 042 | 16 384 | à vérifier | $0.80 / $4.00 (preview) |
| `openai/gpt-oss-safeguard-20b` | 131 072 | 65 536 | à vérifier | Modération de contenu |
| `groq/compound` | 131 072 | 8 192 | système agentique | Système, pas un simple chat |
| `groq/compound-mini` | 131 072 | 8 192 | système agentique | Version légère |
| `whisper-large-v3` | audio | — | ~200× temps réel | $0.111 / heure transcrite |
| `whisper-large-v3-turbo` | audio | — | ~220× temps réel | $0.04 / heure transcrite |
| `canopylabs/orpheus-v1-english` | 4 000 | 50 000 (audio) | TTS | preview |

**Dépréciés** (ne plus utiliser dans du neuf) : `llama-3.3-70b-versatile`, `llama-3.1-8b-instant`, Mixtral 8x7B (déc. 2025). Remplacements conseillés par Groq : `openai/gpt-oss-120b` et `qwen/qwen3.6-27b`.
Réductions cumulables (vérifié 2026) : **input caché −50 %**, **batch −50 %**, **tier développeur −25 %**.

## 18. Groq : free tier — limites vérifiées

- **Toujours gratuit avec limites**, sans carte bancaire. Jamais facturé : au-delà du quota → HTTP 429.
- Ordres de grandeur (varient par modèle, limites **au niveau de l'organisation** — multiplier les clés ne contourne pas) :
  - **30 req/min** (RPM) sur les modèles texte courants ;
  - **1 000 à 14 400 req/jour** selon le modèle (ex. gpt-oss-120b : 30/1 000/8K/200K en RPM/RPD/TPM/TPD d'après le relevé du 27/08/2026) ;
  - Whisper : 20 RPM, 2 000 req/jour.
- Le tier payant (pay-as-you-go) multiplie les limites par ~10 et ajoute le batch à −50 %.
- Données : pas d'entraînement sur tes prompts ; rétention jusqu'à 30 jours (ZDR disponible selon l'offre — à vérifier dans les réglages du compte).

## 19. Groq : exemple d'appel

```bash
curl https://api.groq.com/openai/v1/chat/completions \
  -H "Authorization: Bearer gsk_FAKEFAKEFAKEFAKE" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "openai/gpt-oss-20b",
    "messages": [{"role": "user", "content": "Résume ce log en une ligne : ..."}],
    "temperature": 0.2,
    "max_tokens": 200
  }'
```

```python
from openai import OpenAI
client = OpenAI(base_url="https://api.groq.com/openai/v1", api_key="gsk_FAKEFAKEFAKEFAKE")
r = client.chat.completions.create(
    model="openai/gpt-oss-120b",
    messages=[{"role": "system", "content": "Tu es un assistant réseau."},
              {"role": "user", "content": "Diagnostique : ..."}],
)
print(r.choices[0].message.content)
```

## 20. Groq : cas d'usage (où il bat tout le monde)

1. **Agents temps réel / voix** : 500–1 000 tok/s → une réponse de 200 tokens arrive en < 1 s. Idéal pour un assistant vocal d'atelier.
2. **RAG rapide** : génération de la réponse finale sur tes chunks — la récupération (embeddings) est locale, la génération est instantanée.
3. **Classification / triage à haut volume** : `gpt-oss-20b` à 1 000 tok/s en gratuit, parfait pour étiqueter des tickets ou des logs.
4. **Transcription** : Whisper turbo à $0.04/h (ou gratuit dans les quotas) — comptes-rendus de réunions d'équipe.
5. **Dev itératif** : boucle code→test→fix sans attendre.

## 21. Groq : limites

1. **Contexte** : 131K max sur les modèles phares, souvent moins en pratique sur le free tier — pas de 1M tokens ici.
2. **Catalogue fermé aux poids ouverts** : pas de GPT-6, pas de Claude, pas de Gemini. Pour le raisonnement le plus dur, prévoir un second provider.
3. **Quotas gratuits modestes en TPM** (8K–30K/min selon modèle) : les gros batchs passent au payant.
4. **Modèles dépréciés sans préavis long** : épingler les IDs exacts et surveiller `console.groq.com/docs/models`.
5. **Pas de vision** sur la plupart des modèles texte (à vérifier par modèle).

## 22. Checklist Groq

- [ ] Compte sur `console.groq.com`, clé `gsk_...` (pas de CB).
- [ ] Tester `openai/gpt-oss-20b` (rapide) et `openai/gpt-oss-120b` (costaud).
- [ ] Mesurer le temps de réponse réel sur tes prompts (objectif < 2 s).
- [ ] Coder la gestion du 429 (retry avec backoff) — obligatoire en free tier.
- [ ] Si volume : comparer le pay-as-you-go (−25 % tier dev) vs OpenRouter.

---

# PARTIE C — CEREBRAS

## 23. Cerebras : présentation et Wafer-Scale Engine

Cerebras (cerebras.ai) est, comme Groq, un **fondeur d'inférence** : sa puce, le **Wafer-Scale Engine (WSE)**, est le plus grand processeur jamais gravé — **un wafer entier** (pas une puce découpée), avec des dizaines de Go de SRAM sur la puce et une bande passante mémoire de l'ordre de **21 Po/s sur le wafer** (contre ~80 To/s par rack côté LPU Groq et ~3,3 To/s par GPU H100 — ordres de grandeur constatés dans les comparatifs 2026, à prendre comme tels).
Résultat : **1 800 à 3 000 tokens/s** mesurés sur les petits modèles (Llama 3.1 8B à 1 800 tok/s dès 2024), et **2 522 tok/s** relevés sur Llama 4 Maverick en 2026. Cerebras reste en **précision 16 bits de bout en bout** (pas de sacrifice de précision pour la vitesse, d'après leurs benchmarks vérifiés par Artificial Analysis).
API compatible OpenAI : `https://api.cerebras.ai/v1`. **Free tier permanent sans carte** : 1M tokens/jour.

## 24. Cerebras : modèles servis vérifiés (sept 2026)

| Modèle | Contexte | Vitesse constatée | Prix payant /1M in/out (relevé sept 2026, sources tierces — à reconfirmer) |
|---|---|---|---|
| `gpt-oss-120b` | 131K | ~2 000 tok/s | $0.35 / $0.75 |
| `llama-3.1-8b` | 128K | ~1 800–3 000 tok/s | $0.10 / $0.10 |
| `llama-3.3-70b` | 128K | ~450–1 000 tok/s | $0.85 / $1.20 |
| `qwen-3-32b` | 128K | à vérifier | à vérifier |
| `gemma-4-31b` | à vérifier | à vérifier | à vérifier |
| `llama-4-maverick` | à vérifier | 2 522 tok/s (relevé tiers) | à vérifier |

Catalogue **plus étroit** que Groq/OpenRouter : poids ouverts uniquement (Llama, Qwen, gpt-oss, Gemma), **pas de vision**, pas de modèles fermés. Les fine-tunes communautaires exotiques mettent plus de temps à arriver (compilation spécifique au wafer).

## 25. Cerebras : free tier et tarification vérifiés

- **Free tier permanent, sans carte bancaire** : **1 000 000 tokens/jour** (reset quotidien), ~5 RPM, 30K–100K TPM selon le modèle (relevés convergents août–sept 2026).
- **Limite temporaire constatée** : contexte plafonné à **8K tokens** sur le free tier (annoncé comme temporaire par la communauté — à re-vérifier, bloquant pour du RAG à gros chunks).
- Tier développeur : dépôt min ~$10, facturation à l'usage (prix du tableau section 24).
- $5 de crédits d'essai à l'inscription constatés sur certaines pages (à vérifier — l'offre bouge).
- Documentation vie privée plus légère que chez les concurrents : pour des données sensibles, préférer un provider avec ZDR explicite.

## 26. Cerebras vs Groq : tableau comparatif

| Critère | Cerebras (WSE) | Groq (LPU) |
|---|---|---|
| Techno | Wafer entier, SRAM on-chip massive | Puces LPU déterministes |
| Débit max constaté | **1 800–3 000 tok/s** (8B), 2 522 tok/s (Maverick) | 500–1 000 tok/s |
| Latence premier token | Très faible | Très faible (~100 ms) |
| Free tier | **1M tokens/jour**, sans CB | 30 RPM, 1K–14,4K req/jour, sans CB |
| Contexte free | **8K cap** (temporaire, à surveiller) | 131K (selon modèle) |
| Prix payant (ex. 120B) | $0.35 / $0.75 | **$0.15 / $0.60** |
| Prix petit modèle | $0.10 / $0.10 (8B) | **$0.075 / $0.30** (20B) |
| Catalogue | Étro
...[truncated 12332 chars]
## 29. Exemple d'appel Cerebras

```bash
curl https://api.cerebras.ai/v1/chat/completions \
  -H "Authorization: Bearer csk-FAKEFAKEFAKEFAKE" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "llama-3.1-8b",
    "messages": [{"role": "user", "content": "Liste les étapes de consignation électrique avant intervention sur un onduleur."}],
    "max_tokens": 500,
    "temperature": 0.3
  }'
```

Même client Python OpenAI, `base_url="https://api.cerebras.ai/v1"`.

---

# PARTIE D — COMPARATIF : QUAND UTILISER QUOI

## 30. Tableau de synthèse prix / latence / choix (prix vérifiés le 27/09/2026 sauf mention)

| Fournisseur | Modèle repère | In / Out par 1M | Latence typique | Free tier | Idéal pour |
|---|---|---|---|---|---|
| OpenRouter | `deepseek-v4.1-flash` | $0.15 / $0.60 | Moyenne (routage) | 50 req/j | Comparer, fallback |
| OpenRouter | `glm-5.3-flash` (promo) | $0.075 / $0.25 | Moyenne | via `:free` | Pas cher + contexte 1,3M |
| Groq payant | `gpt-oss-20b` | $0.075 / $0.30 | **~1 000 tok/s** | 30 RPM | Temps réel, triage |
| Groq payant | `gpt-oss-120b` | $0.15 / $0.60 | ~500 tok/s | 30 RPM | Agent rapide costaud |
| Cerebras payant | `llama-3.1-8b` | $0.10 / $0.10 | **~2 000 tok/s** | 1M tok/j | Haut volume simple |
| Cerebras payant | `gpt-oss-120b` | $0.35 / $0.75 | ~2 000 tok/s | 1M tok/j | Vitesse max gros modèle |
| OpenAI direct | `gpt-6-luna` | $0.10 / $0.50 | Moyenne | Non | Volume raisonné fermé |
| OpenAI direct | `gpt-6-sol` | $2 / $10 | Moyenne | Non | Code / raisonnement |
| OpenAI direct | `gpt-6-astra` | $10 / $50 | Moyenne | Non | Frontière |
| Anthropic direct | `claude-haiku-4.5` | $1 / $5 | Faible | Non | Rapide + économe fermé |
| Anthropic direct | `claude-sonnet-5` | $2 / $10 | Moyenne | Non | Code agentique |
| Anthropic direct | `claude-opus-5.5` | $4 / $20 | Moyenne-haute | Non | Raisonnement dur |
| DeepSeek direct | `deepseek-v4.1-flash` | $0.15 / $0.60 | Moyenne | 5M tokens offerts (inscription) | Rapport qualité/prix |
| Local (Ollama) | `qwen3:32b` Q4 | $0 (élec) | 10–60 tok/s (RTX 4090) | ∞ | Confidentiel, volume |

## 31. Arbre de décision (angle budget Zelef)

1. **Question simple / triage / résumé** → Groq free (`gpt-oss-20b`) ou Cerebras free (`llama-3.1-8b`). Coût : 0.
2. **Besoin de comparer des modèles** → OpenRouter, script section 11. Coût : quasi 0 avec `:free`.
3. **Raisonnement difficile, code complexe** → DeepSeek V4.1 (direct ou OpenRouter, $0.15/$0.60) d'abord ; si insuffisant, `gpt-6-sol` ($2/$10) ou `claude-sonnet-5` ($2/$10).
4. **Temps réel / voix** → Groq ou Cerebras (free puis payant).
5. **Gros volume répétitif** (> 50M tokens/mois) → calculer le seuil du local (section 134) : souvent Ollama/vLLM sur RTX 4090.
6. **Données sensibles / hors-ligne** → local obligatoire, pas de débat.
7. **Embeddings** → TEI local (partie G), pas l'API OpenAI.

## 32. Le pattern « routeur intelligent » (recommandé pour ton RAG)

```python
def choisir_modele(question: str, budget: str = "eco") -> tuple[str, str]:
    """Retourne (base_url, model). Logique : gratuit d'abord, payant si besoin."""
    if budget == "eco":
        # 1. Triage / simple -> Groq gratuit
        return ("https://api.groq.com/openai/v1", "openai/gpt-oss-20b")
    # 2. Standard -> Cerebras gratuit (1M tok/j)
    # 3. Complexe -> OpenRouter DeepSeek V4.1 ($0.15/$0.60)
    # 4. Très complexe -> gpt-6-sol ou claude-sonnet-5
```

En pratique : un petit classifieur (lui-même sur modèle gratuit) estime la difficulté, puis route. Les systèmes multi-fournisseurs sérieux ajoutent : **cache sémantique** (même question → réponse stockée, coût 0), **déduplication**, et **limites par utilisateur/jour**.

## 33. Coût réel d'une question RAG (exemple chiffré)

Hypothèses : 5 chunks de 800 tokens récupérés + prompt système 500 tokens + question 100 tokens = **4 600 tokens in**, réponse **400 tokens out**.

| Fournisseur / modèle | Coût par question | 1 000 questions/jour | 30 jours |
|---|---|---|---|
| Groq free / Cerebras free | $0 | $0 | $0 |
| DeepSeek V4.1 ($0.15/$0.60) | $0.00093 | $0.93 | ~$28 |
| gpt-6-luna ($0.10/$0.50) | $0.00066 | $0.66 | ~$20 |
| gpt-6-sol ($2/$10) | $0.0132 | $13.20 | ~$396 |
| claude-opus-5.5 ($4/$20) | $0.0264 | $26.40 | ~$792 |
| Local Ollama (élec 300W à 0.20 €/kWh) | ~$0.00006 | $0.06 | ~$2 |

Lecture : le gratuit absorbe des milliers de questions/jour ; le saut vers un modèle frontière se justifie uniquement si la qualité l'exige **mesurée** (éval, pas intuition).

## 34. Comparer honnêtement : la méthode

1. Fige **20–50 questions réelles** de ton domaine (pas des benchmarks génériques).
2. Pour chaque modèle : même prompt système, même `temperature=0`, même contexte.
3. Note : exactitude (barème 0/1/2), hallucinations, format respecté, tokens consommés, latence.
4. Calcule le **coût par bonne réponse** = coût total / nombre de réponses correctes. C'est le seul KPI qui compte.
5. Re-teste trimestriellement : les modèles et prix bougent vite (cf. promos GLM/Mercury).

---

# PARTIE E — FREELLMAPI ET L'ÉCOSYSTÈME GRATUIT RÉEL

## 35. « freellmapi » : identification — C'EST UN VRAI PROJET

Bonne nouvelle : `freellmapi` existe. C'est **FreeLLMAPI** (github.com/tashfeenahmed/freellmapi, licence MIT, site freellmapi.co) : un **proxy auto-hébergé compatible OpenAI** qui **empile les tiers gratuits de ~28 fournisseurs** (~1,7 à 7 Md tokens/mois cumulés selon la version du catalogue) derrière **un seul endpoint `/v1`**.
Principe : tu y enregistres tes clés gratuites (Google, Groq, Cerebras, Mistral, OpenRouter, GitHub Models, Cohere, Cloudflare, HuggingFace, Z.ai, NVIDIA, OVH AI Endpoints, Pollinations, LLM7, Kilo, OpenCode Zen, AI Horde…), le routeur choisit le meilleur modèle dispo par requête, **bascule automatiquement** au suivant en cas de rate-limit (429), et suit la conso par clé pour rester sous chaque plafond gratuit. Clés stockées **chiffrées**. Tu peux aussi brancher tes endpoints locaux (Ollama, llama.cpp, vLLM).
Modèle : open source gratuit en self-hosted (image Docker `ghcr.io/freellmapi`) ; offre hébergée « premium » à **$19/an** (catalogue live auto-à-jour). Mention légale du projet : **« personal experimentation only »** — pas pour de la prod commerciale.

## 36. FreeLLMAPI : installation (Docker)

```bash
# 1. Récupérer l'image (vérifié : ghcr.io/freellmapi via le README officiel)
docker pull ghcr.io/hatch/freellmapi 2>/dev/null || docker pull ghcr.io/freellmapi/freellmapi
# En pratique, suivre le README du repo pour le nom exact d'image (il a pu changer) :
# https://github.com/tashfeenahmed/freellmapi

# 2. Lancer (exemple type — adapter aux variables du README)
docker run -d --name freellmapi \
  -p 8787:8787 \
  -v freellmapi-data:/data \
  --restart unless-stopped \
  ghcr.io/freellmapi/freellmapi:latest

# 3. Ouvrir l'UI locale, y coller tes clés gratuites (Groq, Gemini, Cerebras...)
# 4. Appeler comme une API OpenAI :
curl http://localhost:8787/v1/chat/completions \
  -H "Authorization: Bearer flm_FAKEFAKE" \
  -H "Content-Type: application/json" \
  -d '{"model": "auto", "messages": [{"role": "user", "content": "Bonjour"}]}'
```

Le catalogue de modèles se met à jour via un flux signé (pas besoin de `git pull` pour les nouveaux modèles gratuits). Vérifie toujours le README pour les variables d'environnement exactes — le projet évolue vite.

## 37. FreeLLMAPI : intérêts et limites pour toi

**Intérêts** : un seul endpoint pour tout ton gratuit ; failover auto quand Groq te met un 429 à 14h ; suivi de conso par provider ; tu peux y brancher ton Ollama local comme « provider » de plus.
**Limites** : c'est un projet communautaire (pas de SLA) ; les quotas gratuits restent ceux des fournisseurs (il ne les augmente pas, il les **additionne**) ; « personal experimentation only » → pas pour servir ton équipe en prod ; chaque clé gratuite a ses propres **conditions d'utilisation des données** (section 42) — le proxy ne les efface pas.

## 38. L'écosystème gratuit réel en 2026 (vérifié le 27/09/2026)

Tableau des tiers **permanents, sans carte bancaire** (les chiffres bougent — re-vérifier avant de dimensionner) :

| Fournisseur | Ce qui est gratuit | Limites constatées (sept 2026) | Modèles notables |
|---|---|---|---|
| **Groq** | Tous les modèles du catalogue | 30 RPM ; 1K–14,4K req/j ; 8K–30K TPM selon modèle | gpt-oss 120B/20B, Qwen3.6-27b, Whisper |
| **Cerebras** | Tous les modèles | **1M tokens/jour** ; ~5 RPM ; 8K ctx cap (temporaire) | gpt-oss-120b, llama-3.1-8b |
| **Google AI Studio (Gemini)** | Famille Flash / Flash-Lite | ~15–30 RPM ; ~1 500 req/j (Flash) ; 1M TPM | Gemini 3.x Flash, Gemma |
| **OpenRouter** | Modèles `:free` | 50 req/j (1 000/j si $10 déjà chargés) ; 20 RPM | Nemotron 3 Ultra 550B, GLM 5.2, gpt-oss-20b |
| **Mistral La Plateforme** | Plan « Experiment » | ~1 Md tokens/mois mais ~2 RPM ; TPM 25K–20M selon modèle | Mistral Large, Medium 3.5, Codestral |
| **Mistral Codestral** | Clé dev Codestral | 30 RPM, 2 000 req/j | Codestral (code) |
| **NVIDIA NIM** | Build / API hosted | ~40 RPM ; crédits à l'inscription (~1 000) | Llama, Nemotron, Qwen, GLM |
| **Cohere** | Clé d'essai | 20 RPM ; ~1 000 req/mois ; usage non commercial | Command A/R, Aya |
| **Cloudflare Workers AI** | Edge inference | 10 000 neurons/jour | Llama 3.x, Mistral 7B, Qwen, **BGE embeddings** |
| **Hugging Face** | Inference Providers | $0.10/mois de crédits (Free) ; $2/mois (PRO) | 200+ modèles via partenaires |
| **GitHub Models** | Playground + API sandbox | Rate-limité ; **pas de prod** | GPT-5.x, Llama, Mistral, Qwen |
| **DeepSeek** | Inscription | **5M tokens offerts** (one-shot) | DeepSeek V4.1 |
| **SambaNova** | Free tier | 20–480 RPM ; $5 de crédit d'essai | Llama 4 Maverick, DeepSeek R1 |
| **OVH AI Endpoints** | Bêta | 2 RPM anonyme / 400 RPM authentifié | Qwen3, 20+ poids ouverts |
| **Together / Fireworks** | Crédits d'essai | ~$1–5 à l'inscription | 200+ poids ouverts |
| **Chutes.ai** | Communautaire | Selon dispo (réseau Bittensor) | DeepSeek-R1, Llama 70B |

**Pas de free tier permanent** : OpenAI et Anthropic (crédits d'essai temporaires uniquement).

## 39. Gemini gratuit : ce qu'il faut savoir (sept 2026)

- Depuis **avril 2026**, seuls les modèles **Flash / Flash-Lite** sont gratuits ; la série **Pro est payante**.
- Quotas constatés : ~15–30 RPM, ~1 500 req/jour sur Flash, jusqu'à 1M TPM ; **fenêtre 1M tokens** sur Flash — le plus grand contexte gratuit du marché.
- Depuis **juin 2026**, les clés API « sans restriction » sont bloquées : passer par AI Studio avec restrictions d'usage.
- **Point de vigilance majeur** : les prompts du free tier **peuvent servir à améliorer les produits Google** (revue humaine possible). Ne jamais y envoyer de données pro sensibles.
- Cas d'usage roi : résumer de très longs documents (1M ctx gratuit), multimodal (images/PDF) gratuit.

## 40. Mistral gratuit : plan Experiment

- Le plan **Experiment** de La Plateforme couvre **tous les modèles** (y compris Large et Codestral), de l'ordre d'**1 Md tokens/mois**, mais bridé à **~2 req/min** — c'est fait pour tester, pas pour du temps réel.
- **Données** : Mistral peut **entraîner sur les données du free tier, avec opt-out** dans les réglages — à désactiver explicitement si tu testes avec du contenu sensible. Rétention configurable (jamais → 1 an).
- **Codestral** (le modèle code) a sa propre clé dev gratuite : 30 RPM / 2 000 req/j — excellent pour de l'autocomplétion perso.
- Vérification téléphone requise sur les deux.

## 41. Autres gratuits notables (détails)

- **GitHub Models** : avec n'importe quel compte GitHub, playground + API sandbox sur 100+ modèles (GPT-5.x, Llama, Mistral, Qwen). **Interdit en production**, quotas journaliers par palier. Parfait pour tester un modèle fermé avant de payer.
- **NVIDIA NIM** : ~40 RPM après vérification téléphone, crédits à l'inscription ; catalogue de gros poids ouverts (Nemotron, Kimi, GLM). Endpoint `https://integrate.api.nvidia.com/v1` compatible OpenAI.
- **Cohere** : 1 000 req/mois, **usage non commercial uniquement** sur la clé d'essai — intéressant pour tester le **reranking** (modèles Rerank) et les embeddings multilingues avant d'acheter.
- **Cloudflare Workers AI** : 10 000 neurons/jour, ~80 modèles dont **BGE pour les embeddings** — un backend d'embeddings gratuit potentiel pour ton RAG (à évaluer qualité/latence).
- **SambaNova** : puces RDU propriétaires, free tier 20–480 RPM + $5 d'essai ; Llama 4 Maverick, DeepSeek R1 hébergés.

## 42. Le coût caché du gratuit : TES DONNÉES (à lire absolument)

| Fournisseur | Entraînement sur tes prompts ? | Rétention |
|---|---|---|
| Groq | Non | Jusqu'à 30 jours (ZDR selon offre) |
| Cerebras | Non communiqué clairement (sept 2026) | À vérifier |
| Google AI Studio (free) | **Oui possible** (amélioration produits, revue humaine) | Selon CGU |
| Mistral Experiment | **Oui, avec opt-out** | Configurable (jamais → 1 an) |
| OpenRouter | Selon le provider final (`data_collection: "deny"` pour filtrer) | Selon provider |
| DeepSeek (API) | **Oui, avec opt-out** | « Aussi longtemps que nécessaire » |
| OpenAI / Anthropic (payant) | **Non par défaut** | 30 jours |

**Règle** : gratuit = tu es le produit (tes prompts). Données perso/projet sensible → local (Ollama) ou offre payante avec ZDR. C'est non négociable en contexte d'entreprise.

## 43. Stratégie d'empilement « $0 » (concrète)

Ordre d'appel pour une appli perso :

1. **Cache local** (SQLite/Redis) : question déjà vue → 0 appel.
2. **Groq free** (`gpt-oss-20b`) : triage, résumés, extractions.
3. **Cerebras free** (`llama-3.1-8b`, 1M tok/j) : volume simple.
4. **Gemini Flash free** : gros documents (1M ctx), multimodal.
5. **OpenRouter `:free`** (`nemotron-3-ultra-550b`, `glm-5.2`) : quand il faut du muscle gratuit.
6. **Mistral Experiment** : modèles FR / Mistral Large en test.
7. **Payant ciblé** : uniquement ce qui échoue aux étapes 1–6.

Avec FreeLLMAPI (section 36), les étapes 2–6 tiennent derrière **un seul endpoint** avec failover auto. Pour ton RAG : embeddings en local (TEI, partie G) + génération sur cette pile = **facture mensuelle proche de 0**.

## 44. Checklist écosystème gratuit

- [ ] Créer les comptes : Groq, Cerebras, Google AI Studio, OpenRouter, Mistral, NVIDIA, GitHub.
- [ ] Noter chaque quota dans un tableau (RPM / req/j / TPM) + date de vérification.
- [ ] Déployer FreeLLMAPI en Docker, y enregistrer les clés, tester le failover (couper une clé → vérifier la bascule).
- [ ] Coder le **retry 429 avec backoff exponentiel** (obligatoire partout en gratuit).
- [ ] Désactiver l'entraînement sur données là où l'opt-out existe (Mistral, DeepSeek).
- [ ] Marquer les données sensibles « local only » dans ton code (garde-fou).

---

# PARTIE F — DEEPSEEK HARNESS + SELF-HOSTING LLM LOCAL

## 45. « Harness » : les deux sens (important)

Le mot **harness** a deux sens qu'il ne faut pas mélanger :
1. **Sens générique** (industrie) : le « harnais » autour d'un modèle — scaffolding, outils, gestion du contexte, retries, évals, permissions, observabilité. « Le modèle se loue, le harnais se construit. » C'est ce que tu construis autour de ton RAG.
2. **DeepSeek Harness** (outil précis, sorti le **13 août 2026**) : l'agent de code open source de DeepSeek, commande `dsh`. Les deux sections suivantes couvrent l'outil ; le reste de la partie F couvre le **self-hosting** (faire tourner DeepSeek / un LLM ouvert en local), qui était l'autre interprétation possible — les deux sont traitées, rien n'est laissé dans le flou.

## 46. DeepSeek Harness : identification et état (sept 2026)

- **Repo** : `github.com/deepseek-ai/deepseek-harness` — **réel, vérifié le 27/09/2026**.
- **Licence** : MIT. **Statut** : *developer preview* (changements cassants possibles — ne pas baser une prod dessus aujourd'hui).
- **Lancement** : 13 août 2026, en même temps que DeepSeek V4 Pro. ~95k stars en 2 jours, > 225k mi-septembre 2026 (d'après la presse tech — ordre de grandeur).
- **Nature** : gros monorepo **TypeScript**, architecture **« everything is a plugin »** (tout est un plugin : UI, providers, outils), motorisé par **Cordis** (framework de composabilité).
- **Ce n'est PAS un hébergeur de modèle** : c'est un **agent de code** (concurrent d'OpenCode/Claude Code) avec UI web locale, runner headless, sandbox d'exécution, sous-agents, recherche web, plugins.
- **Multi-provider** : route DeepSeek native + adaptateur multi-fournisseurs (compatible avec d'autres APIs/gateways) — tu peux le brancher sur OpenRouter, Groq, ou ton Ollama local.

## 47. DeepSeek Harness : installation et démarrage

Prérequis : **Node.js 22.19+ ou ≥ 24** (la 23.x n'est **pas** supportée — dépend de `node:sqlite` et du type-stripping natif).

```bash
# Option 1 : sans installation (recommandé pour tester)
npx @deepseek-ai/dsh web

# Option 2 : global
npm install -g @deepseek-ai/dsh
dsh web

# Option 3 : depuis les sources
git clone https://github.com/deepseek-ai/deepseek-harness.git
cd deepseek-harness
pnpm install && pnpm run build
pnpm dsh web
```

- L'UI web démarre sur **`http://127.0.0.1:3080`** (s'ouvre dans le navigateur).
- Config/API key : dans les réglages de l'UI (clé DeepSeek ou autre provider).
- Données locales : `~/.dsh` (plugins, profils, historique de sessions).
- **Sécurité** : l'agent tourne avec **tous tes droits utilisateur** (lecture fichiers, exécution commandes) → lance-le dans un dossier poubelle d'abord, lis `SAFETY.md` du repo.

## 48. DeepSeek Harness : usages et service systemd

- **Mode web** : `dsh web` — assistant de code dans le navigateur, sous-agents, outils terminaux.
- **Mode headless** : une tâche sans UI (à brancher sur CI/automation — vérifier la syntaxe exacte dans la doc, elle bouge en preview).
- **Plugins** : topic GitHub `dsh-plugin` ; extension VS Code communautaire existante (`deepseek-harness-for-vscode`).
- Service utilisateur systemd (toujours actif) :

```ini
# ~/.config/systemd/user/dsh-web.service
[Unit]
Description=DeepSeek Harness Web UI
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
ExecStart=/usr/bin/dsh web --no-open
Restart=always
RestartSec=3

[Install]
WantedBy=default.target
```

```bash
systemctl --user daemon-reload
systemctl --user enable --now dsh-web.service
loginctl enable-linger $USER   # optionnel : actif même sans session ouverte
```

## 49. DeepSeek Harness vs alternatives (sept 2026)

| Outil | Éditeur | Langage | Prix | Point fort |
|---|---|---|---|---|
| DeepSeek Harness (`dsh`) | DeepSeek (MIT) | TypeScript | Gratuit (+ coût API du modèle) | Architecture tout-plugin, multi-provider |
| OpenCode | SST (MIT) | TypeScript/Go | Gratuit (+ API) | Léger, terminal |
| Claude Code | Anthropic | — | Abonnement/API | Intégration Claude |
| Codex CLI | OpenAI | TypeScript | Abonnement/API | Intégration GPT |

Pour toi : `dsh` est intéressant comme **labo d'agents** (comprendre l'architecture d'un harness réel), pas comme choix de prod aujourd'hui (preview). Le vrai gain, c'est d'étudier son découpage plugin pour ton propre scaffolding RAG.

## 50. Self-hosting : panorama des options

| Outil | Idéal pour | GPU requis | API OpenAI-compatible |
|---|---|---|---|
| **Ollama** | Démarrer vite, dev, laptop | Non (CPU ok) | Oui (`:11434/v1`) |
| **llama.cpp** (`llama-server`) | CPU, embarqué, contrôle fin | Non | Oui |
| **vLLM** | Servir en prod, débit | Oui (NVIDIA) | Oui (`:8000/v1`) |
| **TEI** | Embeddings uniquement | Optionnel | Oui (`/v1/embeddings`) |
| **TGI** | Alternative HF au serving | Oui | Partiel |

Règle : **Ollama pour expérimenter**, **vLLM pour servir** (débit, batching), **llama.cpp quand pas de GPU NVIDIA**.

## 51. Ollama : installation

```bash
# Linux (script officiel)
curl -fsSL https://ollama.com/install.sh | sh

# Vérifier
ollama --version
systemctl status ollama   # service actif par défaut

# macOS / Windows : installateur sur ollama.com
# Docker :
docker run -d --gpus=all -v ollama:/root/.ollama -p 11434:11434 \
  --name ollama --restart unless-stopped ollama/ollama
```

Par défaut Ollama écoute sur `127.0.0.1:11434`. Pour l'exposer sur le LAN : `OLLAMA_HOST=0.0.0.0:11434` (variable d'environnement du service — attention, **pas d'auth native** : ne l'expose jamais sur Internet sans reverse-proxy + auth).

## 52. Ollama : modèles DeepSeek et autres (commandes réelles)

```bash
# DeepSeek R1 distillés (raisonnement) — tailles vérifiées sur le catalogue Ollama
ollama run deepseek-r1:1.5b    # ~1,1 Go, CPU ok
ollama run deepseek-r1:7b      # ~4,7 Go
ollama run deepseek-r1:8b      # ~4,9 Go
ollama run deepseek-r1:14b     # ~9 Go
ollama run deepseek-r1:32b     # ~20 Go
ollama run deepseek-r1:70b     # ~43 Go

# Autres utiles pour un sysadmin
ollama run qwen3:32b           # bon équilibre FR/code (vérifier le tag exact : ollama.com/library/qwen3)
ollama run gemma3:27b          # Google, multilingue
ollama run mistral:7b          # léger
ollama run nomic-embed-text    # embeddings locaux 768 dim (alternative à TEI, simple)

# Lister / supprimer
ollama list
ollama rm deepseek-r1:1.5b
```

Note : `deepseek-r1:671b` (le modèle complet) existe au catalogue mais demande un cluster (~400 Go+ de VRAM) — pas pour un poste. Les distillés 7b→32b sont le sweet spot local.

## 53. Ollama : API locale et Python

```bash
# Génération (API native)
curl http://localhost:11434/api/generate -d '{
  "model": "qwen3:32b",
  "prompt": "Explique la différence entre LACP actif et passif.",
  "stream": false
}'

# Chat OpenAI-compatible (même code que Groq/OpenAI, autre base_url)
curl http://localhost:11434/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model": "qwen3:32b", "messages": [{"role": "user", "content": "Bonjour"}]}'
```

```python
from openai import OpenAI
client = OpenAI(base_url="http://localhost:11434/v1", api_key="ollama")  # clé ignorée en local
r = client.chat.completions.create(model="qwen3:32b",
    messages=[{"role": "user", "content": "Commande VRP pour un trunk ?"}])
print(r.choices[0].message.content)
```

→ **Ton RAG peut pointer sa variable `LLM_BASE_URL` vers Ollama** : 0 €, 0 donnée sortante, même code que le cloud.

## 54. Ollama : Modelfile (personnaliser)

```dockerfile
# Modelfile
FROM qwen3:32b
PARAMETER temperature 0.2
PARAMETER num_ctx 16384
SYSTEM """Tu es un assistant expert réseau et systèmes pour un chef de service.
Réponds en français, direct, avec les commandes exactes. Si tu ne sais pas, dis-le."""
```

```bash
ollama create reseau-assistant -f Modelfile
ollama run reseau-assistant
```

`num_ctx` règle la fenêtre de contexte (défaut souvent 4K–8K selon le modèle) — à monter pour du RAG (16K–32K si la VRAM suit).

## 55. vLLM : installation et serving OpenAI-compatible

vLLM = le serveur de prod (PagedAttention, batching continu, débit). NVIDIA recommandé.

```bash
# Installer (dans un venv ou conteneur CUDA)
pip install vllm

# Servir un modèle (téléchargé depuis le Hub HF)
vllm serve Qwen/Qwen3-32B --port 8000 --gpu-memory-utilization 0.9

# Avec quantization AWQ (4-bit, 2× moins de VRAM)
vllm serve Qwen/Qwen3-32B-AWQ --quantization awq --port 8000

# Multi-GPU (70B sur 2× RTX 4090 par ex.)
vllm serve meta-llama/Llama-3.3-70B-Instruct \
  --tensor-parallel-size 2 \
  --gpu-memory-utilization 0.92 \
  --max-model-len 32768 \
  --port 8000
```

Endpoints : `http://localhost:8000/v1/chat/completions`, `/v1/completions`, `/v1/embeddings` (modèles d'embedding supportés), `/metrics` (Prometheus), `/health`.

Paramètres clés : `--max-model-len` (contexte), `--gpu-memory-utilization` (0–0.95, marge pour le système), `--tensor-parallel-size` (= nombre de GPU), `--quantization` (awq, gptq, fp8), `--enable-prefix-caching` (**indispensable en RAG** : le prompt système + chunks récurrents ne sont ré-encodés qu'une fois).

## 56. vLLM : docker-compose de production

```yaml
services:
  vllm:
    image: vllm/vllm-openai:latest
    command: >
      --model Qwen/Qwen3-32B-AWQ
      --quantization awq
      --port 8000
      --gpu-memory-utilization 0.9
      --max-model-len 32768
      --enable-prefix-caching
    ports:
      - "8000:8000"
    volumes:
      - hf-cache:/root/.cache/huggingface
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: 1
              capabilities: [gpu]
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
      interval: 30s
      timeout: 10s
      retries: 3

volumes:
  hf-cache:
```

Prérequis : **nvidia-container-toolkit** installé sur l'hôte (`nvidia-smi` doit fonctionner dans le conteneur). Premier démarrage = téléchargement des poids (plusieurs Go) → prévoir le temps et le volume persistant.

## 57. llama.cpp : quand et comment

- **Quand** : pas de GPU NVIDIA (CPU, AMD, Mac, embarqué), ou besoin du contrôle le plus fin (offload partiel couches CPU/GPU).
- **Serveur** : `llama-server -m modele.gguf -ngl 99 -c 16384 --port 8080` (`-ngl` = couches sur GPU, `99` = tout si ça tient).
- **API** : compatible OpenAI sur `/v1/chat/completions`.
- **GGUF** : LE format local standard (quantizations `Q4_K_M`, `Q5_K_M`, `Q8_0`…) — c'est ce qu'Ollama utilise en interne.
- Perf CPU réaliste : 7B Q4 à ~10–20 tok/s sur un bon CPU moderne — suffisant pour du batch, pas pour du temps réel.

## 58. Quantization : GGUF vs AWQ vs GPTQ (l'essentiel)

| Format | Bits | Où ça tourne | Qualité (perte vs FP16) | Usage |
|---|---|---|---|---|
| FP16 / BF16 | 16 | GPU (gros) | Référence (0) | Entraînement, qualité max |
| FP8 | 8 | GPU récents (Hopper/Ada) | Quasi nulle | vLLM `--quantization fp8` |
| **Q8_0 (GGUF)** | 8 | CPU/GPU | Négligeable (~0,1 %) | « Presque FP16 » en 2× moins lourd |
| **Q4_K_M (GGUF)** | ~4,5 | CPU/GPU | Faible (perplexité +1–3 %) | **Standard local** : le meilleur compromis |
| **AWQ** | 4 | GPU uniquement | Faible | vLLM, inférence GPU 4-bit |
| **GPTQ** | 4 (ou 3/8) | GPU uniquement | Faible à moyenne (selon calibration) | Alternative à AWQ |
| Q2/Q3 (GGUF) | 2–3 | CPU/GPU | **Forte** (dégradation visible) | Dernier recours, petits modèles |

Règles pratiques :
1. **Q4_K_M par défaut** : divise la VRAM par ~3,5 pour une perte quasi invisible sur Q&A et code.
2. Sous Q4 (Q3/Q2), les gros modèles (70B) restent souvent meilleurs que les petits en FP16 — à tester sur TON éval (section 34).
3. AWQ > GPTQ en facilité sous vLLM en 2026 ; les deux exigent un GPU.
4. La quantization **n'affecte presque pas la vitesse** sur GPU moderne (le goulot = la bande passante mémoire, réduite par la quantization → souvent plus rapide).

## 59. Dimensionnement VRAM : la formule (à connaître par cœur)

**Poids du modèle** ≈ `nb_paramètres × octets_par_paramètre` :
- FP16/BF16 : **2 octets** → 7B = 14 Go, 70B = 140 Go
- Q8 : **1 octet** → 7B = 7 Go, 70B = 70 Go
- Q4 : **~0,5 octet** → 7B ≈ 3,9 Go, 70B ≈ 40 Go

**KV cache** (mémoire du contexte, EN PLUS des poids) ≈ `2 × couches × têtes_KV × dim_tête × ctx × octets` :
- Ex. Llama 3.3 70B (80 couches, 8 têtes KV, dim 128) en FP16 : **~320 Ko par token** → 32K ctx ≈ **10 Go** de KV cache.
- Le KV cache grandit avec le **contexte ET le batch** : en RAG avec 32K de contexte, prévoir +30–50 % au-delà des poids.

**Tableau de cadrage** (poids seuls, ajouter KV cache + ~2 Go système/CUDA) :

| Modèle | FP16 | Q8 | Q4_K_M | GPU minimal réaliste (Q4 + ctx 16K) |
|---|---|---|---|---|
| 1.5B | 3 Go | 1,5 Go | ~1,1 Go | CPU / iGPU |
| 7B | 14 Go | 7 Go | ~4,7 Go | RTX 3060 12 Go |
| 8B | 16 Go | 8 Go | ~4,9 Go | RTX 4060 Ti 16 Go |
| 13–14B | 28 Go | 14 Go | ~9 Go | RTX 4070 Ti Super 16 Go (limite) |
| 27B | 54 Go | 27 Go | ~17 Go | RTX 4090 24 Go |
| 32B | 64 Go | 32 Go | ~20 Go | RTX 4090 24 Go |
| 70B | 140 Go | 70 Go | ~40 Go | 2× RTX 4090 ou 1× RTX 6000 Ada 48 Go |
| 671B (DeepSeek R1 full, MoE) | ~1 340 Go (FP8 ~670 Go) | — | ~370 Go (Q4) | Cluster 8× H100 / 8× MI300 — pas du local |

Lecture : sur une **RTX 4090 24 Go**, le sweet spot = **32B en Q4** (20 Go poids + KV cache 16K ≈ 3–4 Go). Le 70B Q4 tient à 40 Go + cache → il faut 48 Go (2× 4090 ou A6000/6000 Ada).

## 60. Quel GPU pour quoi (repères 2026, prix indicatifs neufs — à vérifier)

| GPU | VRAM | Prix ordre de grandeur (neuf, 2026) | Idéal LLM local |
|---|---|---|---|
| RTX 3060 12 Go | 12 | ~300 € | 7–8B Q4, embeddings |
| RTX 4070 Super | 12 | ~650 € | 8B Q4 + TEI |
| RTX 4070 Ti Super | 16 | ~850 € | 14B Q4 |
| RTX 4090 | 24 | ~1 800–2 000 € | **32B Q4** — le choix sysadmin |
| RTX 5090 | 32 | ~2 200–2 500 € (dispo variable) | 32B Q8 / 70B Q4 limite |
| RTX 6000 Ada | 48 | ~8 000 €+ | 70B Q4 confortable |
| 2× RTX 4090 | 48 (2×24, tensor-parallel) | ~3 800 € | 70B Q4 via vLLM |
| Mac Studio (Unified 64–128 Go) | partagée | ~2 500–5 000 € | 70B Q4 en unified memory (lent mais ça passe) |

Note pro : en entreprise, une 4090 « gaming » dans un serveur n'a pas de garantie adaptée ; pour de la prod, viser gammes pro (Ada/Ampere pro) ou du cloud (partie I).

## 61. Docker + GPU : le prérequis unique

```bash
# Ubuntu/Debian — NVIDIA Container Toolkit (1 fois par hôte)
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | \
  sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
  sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
  sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
sudo apt update && sudo apt install -y nvidia-container-toolkit
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker

# Test (doit afficher la sortie de nvidia-smi du HOST)
docker run --rm --gpus all nvidia/cuda:12.6.0-base-ubuntu22.04 nvidia-smi
```

Si ce test échoue, rien d'autre (vLLM, Ollama GPU, TEI GPU) ne marchera : c'est le premier diagnostic.

## 62. Exposer ton LLM local en API « prod perso »

Architecture recommandée (tout sur ton serveur, derrière un reverse proxy) :

```
[Clients] --https--> [Caddy/Nginx :443 + basic auth] --> [vLLM :8000]
                                                      --> [Ollama :11434]
                                                      --> [TEI   :8080]
```

```bash
# Exemple Caddy (Caddyfile) — TLS auto + auth basique
llm.maison.lan {
    basic_auth {
        zelef $2a$14$HASH_FAKE_BCRYPT
    }
    reverse_proxy localhost:8000
}
```

Règles : **jamais** d'Ollama/vLLM exposé brut sur Internet (pas d'auth native) ; toujours TLS + mot de passe ou tailscale/WireGuard ; logge les accès.

## 63. Lien avec ton RAG : servir un LLM local pour ton app

Ton RAG a deux appels : **embeddings** (indexation + requête) et **génération** (réponse). Les deux peuvent être locaux :

```python
# config.py — un seul interrupteur pour basculer cloud <-> local
import os
USE_LOCAL = os.getenv("USE_LOCAL", "1") == "1"

if USE_LOCAL:
    LLM_BASE_URL = "http://localhost:8000/v1"      # vLLM (Qwen3-32B-AWQ)
    LLM_MODEL    = "Qwen/Qwen3-32B-AWQ"
    EMB_URL      = "http://localhost:8080"          # TEI (bge-m3)
else:
    LLM_BASE_URL = "https://api.groq.com/openai/v1" # gratuit cloud
    LLM_MODEL    = "openai/gpt-oss-120b"
    EMB_URL      = "https://api.openai.com/v1"      # text-embedding-3-small
    EMB_MODEL    = "text-embedding-3-small"
```

Stratégie hybride réaliste : **embeddings toujours locaux** (TEI, coût 0, qualité équivalente — partie G), **génération locale par défaut** (vLLM/Ollama) avec **fallback cloud gratuit** (Groq) si le serveur est éteint, et **escalade payante** (DeepSeek/gpt-6-sol) sur les questions ratées.

## 64. Checklist self-hosting

- [ ] `nvidia-smi` OK + test Docker GPU (section 61).
- [ ] Ollama installé, `qwen3:32b` (ou équivalent) testé sur 10 questions de ton domaine.
- [ ] Modelfile avec ton prompt système (section 54).
- [ ] vLLM en docker-compose si besoin de débit / multi-utilisateurs (section 56).
- [ ] `--enable-prefix-caching` activé (RAG).
- [ ] Reverse proxy + auth si accès LAN (section 62).
- [ ] Éval qualité local vs cloud sur tes 20–50 questions (section 34) avant de figer.

---

# PARTIE G — TEI (TEXT EMBEDDINGS INFERENCE)

## 65. TEI : présentation et état (sept 2026)

**TEI** (github.com/huggingface/text-embeddings-inference, Apache-2.0) est le serveur d'inférence officiel de Hugging Face pour les **embeddings** (et aussi reranking `/rerank` et classification `/predict`). Écrit en **Rust** (moteur Candle), il fait le **batching dynamique par tokens**, expose des métriques **Prometheus** + OpenTelemetry, et propose des images Docker **CPU et GPU** (par compute capability : `cpu-`, `turing-`, `86-` pour Ampere 86, `89-` pour Ada, `hopper-`…).
État sept 2026 : **projet actif et maintenu** (versions 1.8.x / 1.9.x constatées), utilisé en prod par de nombreuses équipes RAG. C'est **l'alternative locale directe à `text-embedding-3-small`** pour ton RAG : mêmes dimensions paramétrables, coût 0, données qui ne sortent pas.

## 66. TEI : installation Docker (CPU et GPU)

```bash
# --- CPU (suffit pour bge-small/base et l'indexation en batch) ---
docker run -d --name tei \
  -p 8080:80 \
  -v tei-data:/data \
  --restart unless-stopped \
  ghcr.io/huggingface/text-embeddings-inference:cpu-1.8.3 \
  --model-id BAAI/bge-m3

# --- GPU (Ampere 86 = RTX 30xx/A40 ; 89 = Ada/RTX 40xx ; hopper = H100) ---
docker run -d --name tei \
  --gpus all -p 8080:80 \
  -v tei-data:/data \
  --restart unless-stopped \
  ghcr.io/huggingface/text-embeddings-inference:86-1.8.3 \
  --model-id BAAI/bge-m3
```

Le premier démarrage télécharge les poids (~2 Go pour bge-m3) dans `/data` (volume persistant → un seul téléchargement). Santé : `curl localhost:8080/health` → 200. Infos modèle : `curl localhost:8080/info | jq`.

## 67. TEI : modèles d'embeddings recommandés (vérifiés)

| Modèle (`--model-id`) | Dim. | Langues | Contexte | Poids | Pourquoi |
|---|---|---|---|---|---|
| `BAAI/bge-m3` | 1024 | **Multilingue (100+, FR excellent)** | 8 192 | ~2,2 Go | **Choix par défaut pour ton RAG FR** |
| `BAAI/bge-small-en-v1.5` | 384 | Anglais | 512 | ~130 Mo | Ultra-léger, CPU instantané |
| `BAAI/bge-base-en-v1.5` | 768 | Anglais | 512 | ~440 Mo | Bon compromis EN |
| `BAAI/bge-large-en-v1.5` | 1024 | Anglais | 512 | ~1,3 Go | Qualité max EN classique |
| `jinaai/jina-embeddings-v3` | 1024 | Multilingue | 8 192 | ~2,3 Go | Alternative multilingue, Matryoshka |
| `nomic-ai/nomic-embed-text-v1.5` | 768 | Anglais (+multilingue partiel) | 8 192 | ~550 Mo | Long contexte, léger |
| `nomic-ai/nomic-embed-text-v2-moe` | 768 | Multilingue | 8 192 | MoE | Version 2026, à évaluer |
| `Qwen/Qwen3-Embedding-0.6B` (famille) | 1024 | Multilingue | 32K | ~2,4 Go | Long contexte, famille Qwen3 |

**Attention licence** : BGE = MIT (usage commercial OK) ; Jina = Apache-2.0/CC-BY-NC selon version — **vérifier la fiche du modèle exact** avant usage pro. Pour ton RAG FR : **`bge-m3` en premier**, `jina-embeddings-v3` en challenger.

## 68. TEI : configuration (batching, dimensions, truncation)

```bash
docker run -d --name tei \
  -p 8080:80 -v tei-data:/data \
  ghcr.io/huggingface/text-embeddings-inference:cpu-1.8.3 \
  --model-id BAAI/bge-m3 \
  --max-client-batch-size 256 \     # requêtes concurrentes max
  --max-batch-tokens 32768 \         # budget tokens par batch (le batching dynamique s'auto-règle)
  --max-batch-requests 128 \
  --auto-truncate true               # tronque au lieu d'erreur si texte > contexte
```

- **Batching dynamique par tokens** : TEI remplit chaque batch au mieux — pas de calcul manuel `batch × seq²`.
- `--auto-truncate true` : indispensable en indexation (un PDF avec une page monstrueuse ne doit pas faire planter le batch).
- **Dimensions** : `/v1/embeddings` accepte `dimensions` (Matryoshka) sur les modèles qui le supportent (jina-v3, nomic) → ex. 512 au lieu de 1024 = 2× moins de stockage FAISS/Chroma.
- **Normalisation** : TEI renvoie des vecteurs normalisés par défaut selon le modèle — à vérifier pour ta similarité cosinus.

## 69. TEI : appels API (natif + OpenAI-compatible)

```bash
# Natif : un vecteur par input
curl -s localhost:8080/embed -H 'Content-Type: application/json' \
  -d '{"inputs": "Le protocole OSPF utilise Dijkstra."}' | jq '.[0] | length'
# -> 1024  (bge-m3)

# Natif : batch
curl -s localhost:8080/embed -H 'Content-Type: application/json' \
  -d '{"inputs": ["texte 1", "texte 2", "texte 3"]}' | jq 'length'
# -> 3

# OpenAI-compatible : même code que ton RAG actuel !
curl -s localhost:8080/v1/embeddings -H 'Content-Type: application/json' \
  -d '{"input": ["texte 1", "texte 2"], "model": "bge-m3"}' | jq '.data | length'
```

```python
# Python : drop-in replacement de text-embedding-3-small
from openai import OpenAI
emb = OpenAI(base_url="http://localhost:8080/v1", api_key="tei")
v = emb.embeddings.create(input=["panne onduleur", "batterie VRLA"], model="bge-m3")
print(len(v.data[0].embedding))  # 1024
```

→ Migration de ton RAG : changer `base_url` + `model` + `dimensions` (1536 → 1024) et **réindexer** (les vecteurs ne sont pas compatibles entre modèles).

## 70. TEI : reranking (le bonus qui change tout)

TEI sert aussi les **cross-encoders** (`BAAI/bge-reranker-*`) sur `/rerank` — l'étage qui manque à la plupart des RAG maison :

```bash
docker run -d --name tei-rerank -p 8082:80 -v tei-data:/data \
  ghcr.io/huggingface/text-embeddings-inference:cpu-1.8.3 \
  --model-id BAAI/bge-reranker-v2-m3 --auto-truncate false

curl -s localhost:8082/rerank -H 'Content-Type: application/json' -d '{
  "query": "durée de vie batterie onduleur",
  "texts": ["Les batteries VRLA durent 3 à 5 ans à 20°C.",
            "Le protocole OSPF calcule le plus court chemin."]
}' | jq
# -> scores triés : le texte batterie en premier
```

Pipeline RAG pro : **récupération large** (top-50 en cosinus, pas cher) → **rerank** (top-5, précis) → génération. Le rerank coûte ~10 ms/doc en local et améliore la pertinence plus que n'importe quel changement de modèle d'embedding.

## 71. TEI : bench vs API OpenAI text-embedding-3-small

| Critère | TEI local (`bge-m3`) | OpenAI `text-embedding-3-small` |
|---|---|---|
| Prix (vérifié) | **$0** (élec) | **$0.02 / 1M tokens** (prix public stable) |
| Dimensions | 1024 (bge-m3) | 1536 (réductible Matryoshka) |
| Contexte | 8 192 tokens | 8 191 tokens |
| Multilingue FR | **Excellent** (bge-m3 entraîné multilingue) | Très bon |
| Latence (batch indexation) | ~5–20 ms/doc en CPU, ~1–2 ms en GPU | ~100–300 ms + réseau |
| Débit indexation | Milliers de docs/min (batch) | Limité par RPM/TPM du compte |
| Données | **Ne sortent jamais** | Envoyées à OpenAI (pas d'entraînement par défaut, 30 j rétention) |
| Qualité (MTEB) | bge-m3 : top multilingue historique | Référence solide, légèrement derrière les SOTA 2026 |
| Opérabilité | Un conteneur à superviser | Zéro |

**Calcul** : 1M tokens d'indexation = **$0.02** chez OpenAI. Ça paraît rien — mais à 500M tokens de corpus + réindexations, c'est $10 par passe, et chaque requête coûte aussi. En local : 0, pour toujours. **Pour ton RAG : TEI + bge-m 
...[truncated 12084 chars]
## 75. TEI : service systemd (prod perso)

```ini
# /etc/systemd/system/tei-embed.service
[Unit]
Description=TEI embeddings (bge-m3)
Requires=docker.service
After=docker.service network-online.target
Wants=network-online.target

[Service]
ExecStartPre=-/usr/bin/docker rm -f tei-embed
ExecStart=/usr/bin/docker run --rm --name tei-embed \
  -p 8080:80 \
  -v tei-data:/data \
  ghcr.io/huggingface/text-embeddings-inference:cpu-1.8.3 \
  --model-id BAAI/bge-m3 --port 80 \
  --max-client-batch-size 256 --auto-truncate true
ExecStop=/usr/bin/docker stop tei-embed
Restart=always
RestartSec=5
TimeoutStartSec=600

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now tei-embed
# Premier boot : le téléchargement des poids peut prendre plusieurs minutes
journalctl -u tei-embed -f
```

## 76. Checklist TEI

- [ ] Conteneur TEI + `bge-m3` OK (`/health` → 200, `/info` correct).
- [ ] Test `/embed` et `/v1/embeddings` depuis ton code RAG.
- [ ] Décision dimensions : 1024 natives ou réduites (Matryoshka si modèle supporté).
- [ ] Conteneur reranker `bge-reranker-v2-m3` + test `/rerank`.
- [ ] Pipeline : top-50 cosinus → rerank top-5 → génération (section 74).
- [ ] Réindexation complète du corpus avec le nouveau modèle (jamais de mélange d'embeddings).
- [ ] Éval : comparer précision@5 avant/après sur 50 questions FR (section 34 adaptée).
- [ ] systemd + monitoring (latence p95, file de batch via `/metrics`).

---

# PARTIE H — HUGGING FACE (LE HUB ET SON ÉCOSYSTÈME)

## 77. Hugging Face : ce que c'est (en 2026)

Hugging Face (huggingface.co) = **le GitHub des modèles IA** : ~1M+ de modèles, datasets et démos au catalogue en 2026. Quatre piliers pour toi :
1. **Le Hub** : trouver/télécharger des modèles (LLM, embeddings, rerankers, vision, audio).
2. **Inference Providers** : API serverless unifiée (état sept 2026 : actif, voir section 79).
3. **transformers** : LA librairie Python pour charger et utiliser les modèles.
4. **Datasets + Spaces** : données d'éval et démos web gratuites pour tester avant d'installer.

## 78. Le Hub : trouver un modèle pour ton RAG (méthode)

1. Aller sur `huggingface.co/models`, filtrer : tâche (`feature-extraction` pour embeddings, `text-generation` pour LLM), langue (`french`), licence (`mit`, `apache-2.0`).
2. Lire la **fiche modèle** : dimensions, contexte, licence, benchmarks, « Use this model » (snippet prêt à copier).
3. Vérifier les signaux de confiance : téléchargements/mois, likes, organisation vérifiée (BAAI, nomic-ai, Qwen, jinaai…), date de mise à jour.
4. Tester **sans rien installer** : widget « Inference » sur la fiche, ou un **Space** de démo (section 83).
5. Pour le local : vérifier la présence de fichiers **GGUF** (onglet Files → quantizations) ou la compatibilité **TEI/vLLM/Ollama**.

Exemple de recherche ciblée : `sentence-similarity french` → bge-m3, `jina-embeddings-v3` ; `text-generation qwen3 32b gguf` → variantes quantifiées prêtes pour Ollama/llama.cpp.

## 79. Inference Providers : état vérifié (sept 2026)

- **Actif** : endpoint unifié **`https://router.huggingface.co/v1`** (compatible OpenAI), routage vers **18+ partenaires** (Groq, Fireworks, Mistral, Cohere, Nebius, SambaNova…) — **200+ modèles**, **sans surcoût** (HF répercute le prix fournisseur).
- **Crédits mensuels** (vérifié 2026) : **$0.10/mois** (compte Free), **$2/mois** (PRO) — c'est un filet d'essai, pas une infra.
- **BYOK** : tu peux brancher ta propre clé d'un partenaire et payer le partenaire directement (les crédits HF ne s'appliquent plus, mais tu gardes la facturation directe).
- Le backend historique `hf-inference` (ex-« Inference API serverless ») existe toujours, recentré sur **CPU et petits modèles** (embeddings, classification, BERT/GPT-2) ; les gros LLM routent vers les partenaires.
- Limite serverless : modèles **< 10 Go** (quelques exceptions populaires).
- Verdict pour toi : utile pour **tester un modèle du Hub via API** avant de le self-héberger, pas comme backend de prod (crédits trop faibles).

```python
from openai import OpenAI
client = OpenAI(base_url="https://router.huggingface.co/v1", api_key="hf_FAKEFAKEFAKE")
r = client.chat.completions.create(
    model="Qwen/Qwen3-32B:fireworks-ai",   # syntaxe modele:provider (vérifier la doc)
    messages=[{"role": "user", "content": "Bonjour"}])
```

## 80. transformers : les bases d'usage (indispensable)

```bash
pip install transformers torch --index-url https://download.pytorch.org/whl/cu126
pip install sentence-transformers   # surcouche pratique pour les embeddings
```

```python
# --- Embeddings en 5 lignes (sans TEI, pour prototyper) ---
from sentence_transformers import SentenceTransformer
model = SentenceTransformer("BAAI/bge-m3")
vecs = model.encode(["panne onduleur", "batterie VRLA"], normalize_embeddings=True)
print(vecs.shape)  # (2, 1024)

# --- LLM en local (petit modèle, test) ---
from transformers import pipeline
gen = pipeline("text-generation", model="Qwen/Qwen2.5-3B-Instruct", device_map="auto")
print(gen("Explique le VLAN en une phrase.", max_new_tokens=60)[0]["generated_text"])

# --- Tokenizer seul (compter les tokens avant d'appeler une API payante) ---
from transformers import AutoTokenizer
tok = AutoTokenizer.from_pretrained("Qwen/Qwen3-32B")
print(len(tok("Ton texte ici")["input_ids"]), "tokens")
```

`sentence-transformers` suffit pour prototyper tes embeddings ; **passe à TEI** dès que tu veux du débit (section 65). Le tokenizer local = ton **compteur de tokens gratuit** pour estimer les coûts API avant envoi.

## 81. Datasets : évaluer ton RAG avec des données réelles

```python
from datasets import load_dataset
# Exemple : charger un jeu de questions FR pour tester ton RAG
ds = load_dataset("antoinelouis/colbertv2.0", split="train")  # exemple — adapter
```

Usages : construire ton **jeu d'éval** (50 questions + réponses attendues + docs sources), mesurer précision@k, versionner les jeux de test comme du code. Le Hub héberge aussi des corpus FR utiles pour enrichir un RAG technique (Wikipedia FR, etc.).

## 82. Licences : le piège n°1 du Hub (à lire avant usage pro)

| Licence | Usage commercial | Ce que ça implique |
|---|---|---|
| MIT / Apache-2.0 | **Oui** | Libre, attribution requise |
| BGE (MIT) | Oui | OK pour ton RAG, même pro |
| **Llama Community** | Oui sous conditions | Restrictions (pas d'entraînement de concurrents, etc.) — lire le texte |
| **Qwen** (selon version) | Oui sous conditions | Vérifier la version exacte |
| CC-BY-NC / Non-commercial | **Non** | Interdit en entreprise |
| Gated (accès sur demande) | Selon | Accepter les conditions sur la fiche avant téléchargement |

**Règle** : avant d'intégrer un modèle à ton infra pro, **lire la fiche licence du repo exact** (pas « la famille en général »). Les licences changent entre versions.

## 83. Spaces : tester sans installer

Les **Spaces** = démos Gradio/Streamlit hébergées gratuitement. Devant un modèle inconnu : cherche son Space, teste 10 prompts de ton domaine, décide ensuite si tu le télécharges. C'est l'équivalent d'un essai routier — 0 install, 0 GPU.

## 84. Workflow complet : « trouver et tester un modèle pour mon RAG »

1. **Besoin** : ex. « reranker FR léger pour CPU ».
2. **Hub** : filtre `text-reranking` + langue FR → candidats (`bge-reranker-v2-m3`…).
3. **Fiche** : licence OK ? dimensions ? contexte ?
4. **Space / widget** : test rapide de pertinence sur 5 paires query/doc de ton domaine.
5. **Inference Providers** : test API en 10 lignes (section 79) pour mesurer latence/format.
6. **Local** : TEI (embeddings/rerank) ou Ollama (LLM) avec le modèle exact.
7. **Éval** : tes 50 questions, métrique précision@5 (section 34).
8. **Décision** : figer `model-id` + version + hash dans ta doc (reproductibilité).

## 85. `huggingface_hub` : la CLI utile

```bash
pip install huggingface_hub
hf download BAAI/bge-m3                       # télécharger un modèle en cache local
hf download Qwen/Qwen3-32B-GGUF --include "*Q4_K_M*"  # ne prendre qu'une quantization
huggingface-cli login                         # token hf_... pour les modèles gated
```

Le cache `~/.cache/huggingface` est partagé avec TEI (volume `/data`), vLLM et transformers : **télécharge une fois, sers partout**.

## 86. Checklist Hugging Face

- [ ] Compte + token `hf_...` (lecture) créé, stocké dans le gestionnaire de secrets.
- [ ] Savoir filtrer par tâche/langue/licence sur le Hub.
- [ ] Tester un modèle via Space + Inference Providers avant tout téléchargement.
- [ ] Lire la licence du repo exact avant usage pro.
- [ ] Cache HF partagé entre TEI/vLLM/transformers (un seul volume).
- [ ] Épingler les `model-id` + révisions dans ta doc d'infra.

---

# PARTIE I — LOUER DU GPU : VAST.AI ET RUNPOD

## 87. Pourquoi louer plutôt qu'acheter (le calcul)

Acheter une RTX 4090 ≈ 1 900 €. Louer ≈ $0.34–0.69/h.
- **Seuil de rentabilité** : 1 900 € / 0.50 €/h ≈ **3 800 h** ≈ 158 jours de 24h. Si ton besoin annuel < ~1 000 h, **la location gagne**.
- La location donne aussi accès à du **H100/A100** impossible à acheter pour un particulier, et à la **facturation à la seconde** (tu paies 20 min, pas le mois).
- En revanche : pas de données sensibles (machine d'un tiers), disponibilité variable (spot), egress parfois facturé.

## 88. Vast.ai : principe

**Marketplace pair-à-pair** : des particuliers/datacenters louent leurs GPU, tu enchéris. Deux modes :
- **Interruptible (spot)** : le moins cher (~30–50 % sous l'on-demand), mais l'hôte peut reprendre la machine (préempter). Pour batch/fine-tune avec checkpoints fréquents.
- **On-demand** : prix fixe plus élevé, pas de préemption.
Facturation **à la seconde**, pas de minimum. Catalogue : de la RTX 3060 au H100 8×. Egress : ~$0.01–0.02/Go (vérifié avril 2026 — à re-vérifier, c'est le coût caché classique).

## 89. Vast.ai : prix vérifiés (ordres de grandeur, avril–sept 2026)

| GPU | Spot (interruptible) | On-demand | Usage type |
|---|---|---|---|
| RTX 3090 24 Go | ~$0.13/h | ~$0.22/h | 32B Q4 limite, embeddings |
| RTX 4090 24 Go | ~$0.29–0.34/h | ~$0.45–0.59/h | 32B Q4, LoRA 8B |
| A100 80 Go | ~$0.75/h | ~$1.10–1.45/h | 70B Q4, fine-tune sérieux |
| L40S 48 Go | ~$1.20/h | ~$1.75/h (à vérifier) | Inférence 70B |
| H100 80 Go | ~$1.65–2.60/h | ~$2.40–2.90/h | Gros fine-tune, vLLM 70B |

Prix **variables par hôte** (enchères) : ce sont des médianes constatées, pas un tarif officiel. Toujours trier par prix/fiabilité (`reliability > 98 %`, `cuda_vers` compatible).

## 90. Vast.ai : louer pas à pas (CLI)

```bash
# 1. Installer la CLI (SDK + CLI en un)
pip install vastai
vastai set api-key TA_CLE_VAST_FAKE

# 2. Chercher une 4090 fiable et pas chère
vastai search offers \
  'gpu_name=RTX 4090 num_gpus=1 reliability>0.98 cuda_max_good>=12.4 disk_space>60' \
  -o 'dph_total-'

# 3. Créer l'instance (image PyTorch de base + port SSH + Jupyter)
vastai create instance <OFFER_ID> \
  --image pytorch/pytorch:2.5.1-cuda12.4-cudnn9-devel \
  --disk 60 \
  --ssh \
  --jupyter-dir /

# 4. Suivre le démarrage
vastai show instances
vastai logs <INSTANCE_ID>

# 5. SSH (la CLI affiche la commande exacte : port haute, ex. 23456)
ssh -p 23456 root@<IP_HOTE> -i ~/.ssh/id_ed25519

# 6. Détruire à la fin (LE réflexe à automatiser — section 98)
vastai destroy instance <INSTANCE_ID>
```

**Template d'image** : pars de `pytorch/pytorch:...-devel` (toolkit CUDA complet) ou d'une image avec vLLM/Ollama préinstallé. Monte un volume persistant si tu veux garder les poids entre deux locations.

## 91. RunPod : principe

RunPod = **cloud GPU orienté devs**, deux offres :
- **Community Cloud** : machines de particuliers (comme Vast.ai), les moins chères, dispo variable.
- **Secure Cloud** : infra RunPod directe, SLA, plus chère (~1,7×).
- **Serverless** : facturation à la milliseconde, scale-to-zero — pour exposer une API d'inférence sans gérer de machine.
- **Templates** : images 1-clic (PyTorch, vLLM, Ollama, Stable Diffusion…) — le gros avantage vs Vast.ai pour démarrer vite.
- **Egress gratuit**, facturation fine (ms), API Python/GraphQL + CLI `runpodctl` de qualité.

## 92. RunPod : prix vérifiés (avril–sept 2026)

| GPU | Community | Secure Cloud | Note |
|---|---|---|---|
| RTX 4090 24 Go | ~$0.34/h | ~$0.59–0.74/h | Le standard dev |
| RTX 3090 24 Go | ~$0.22/h | n/a | Budget |
| A100 80 Go | ~$1.19/h | ~$1.89–2.49/h | 70B |
| L40S 48 Go | ~$1.90/h | ~$2.49/h | Inférence pro 70B |
| H100 80 Go | ~$2.49/h | ~$2.99–3.89/h | Haut de gamme |

Egress : **$0** (vérifié). À comparer avec Vast.ai ($0.01–0.02/Go) quand tu rapatries des checkpoints de 40–140 Go.

## 93. RunPod : louer pas à pas (UI + API)

**Via l'UI** (le plus simple) :
1. `runpod.io` → Deploy → choisir **Community** (budget) ou **Secure** (fiabilité).
2. GPU : RTX 4090. Template : **« vLLM »** ou **« PyTorch 2.x »** (1-clic).
3. Disque conteneur : 60 Go min (les poids + le cache). Volume réseau si persistance voulue.
4. Exposer : HTTP 8000 (vLLM), TCP 22 (SSH). **Mettre une clé SSH, pas de mot de passe.**
5. Deploy → attendre `RUNNING` → noter l'URL/ports.

**Via l'API Python** (reproductible) :
```python
import runpod
runpod.api_key = "RUNPOD_API_KEY_FAKE"
gpu = runpod.get_gpus()
# Choisir une 4090 community, créer le pod avec le template vLLM :
pod = runpod.create_pod(
    name="vllm-32b",
    image_name="runpod/pytorch:2.4.0-py3.11-cuda12.4.1-devel-ubuntu22.04",
    gpu_type_id="NVIDIA GeForce RTX 4090",
    cloud_type="COMMUNITY",
    volume_in_gb=60,
)
print(pod["id"], pod["desiredStatus"])
```

**Templates à connaître** : `runpod/pytorch` (base), images communautaires **vLLM**, **Ollama** (chercher « ollama » dans les templates), **Jupyter**. Un template = image + ports + variables d'env : fork-le pour figer ta config (ex. ton Modelfile pré-chargé).

## 94. Vast.ai vs RunPod : tableau comparatif

| Critère | Vast.ai | RunPod |
|---|---|---|
| Modèle | Marketplace P2P (enchères) | Cloud dev (community + secure) |
| RTX 4090 typique | **$0.29–0.59/h** (spot→on-demand) | $0.34/h (community) / $0.59–0.74/h (secure) |
| A100 80 Go | ~$0.75–1.45/h | ~$1.19–2.49/h |
| H100 80 Go | ~$1.65–2.90/h | ~$2.49–3.89/h |
| Facturation | **À la seconde** | À la milliseconde (serverless) / heure |
| Egress | **$0.01–0.02/Go** (à re-vérifier) | **Gratuit** |
| Fiabilité | Variable (noter l'hôte, `reliability`) | Secure > Community |
| Démarrage | CLI `vastai` (bien) | UI + templates 1-clic (mieux) |
| API | CLI/SDK correct | **Python SDK + GraphQL + runpodctl** (excellent) |
| Serverless | Non (instances) | **Oui** (scale-to-zero) |
| Idéal pour | **Prix plancher**, batch, fine-tune | Dev rapide, API serverless, équipe |

**Verdict** : Vast.ai pour le **prix le plus bas** (batch, LoRA, tests) ; RunPod pour la **simplicité** (templates, serverless, egress gratuit). Les deux battent AWS/GCP (~2–3× moins cher) et écrasent l'achat si < 1 000 h/an.

## 95. Cas d'usage location GPU (concrets pour toi)

1. **Fine-tune LoRA d'un 8B sur tes docs** (8 h sur 4090) : ~$2.80 (Vast spot) à $5.92 (RunPod secure). Le prix d'un sandwich pour un modèle à ta sauce.
2. **Batch d'embeddings massif** : 500M tokens sur TEI GPU loué 2 h ≈ $1 — vs $10 chez OpenAI par passe.
3. **Test 70B Q4** : 2 h sur A100 80 Go ≈ $2.40 — pour décider si ça vaut l'achat d'une 2ᵉ 4090.
4. **Inférence API temporaire** : RunPod serverless vLLM pour une démo équipe, scale-to-zero après.
5. **Entraînement complet 70B** : 24 h sur 8× H100 ≈ $420–630 — à ne faire qu'avec un vrai besoin (et des checkpoints fréquents).

## 96. Pièges Vast.ai / RunPod (15 points de vigilance)

1. **Oublier de détruire l'instance** : n°1 des factures surprises → script d'auto-destruction (section 98).
2. **Spot préempté en plein fine-tune** : checkpoints toutes les N steps + reprise auto, sinon tu paies pour rien.
3. **Egress Vast.ai** : rapatrier un checkpoint 70B (140 Go) ≈ $1.40–2.80 — prévoir, ou rester chez RunPod (gratuit).
4. **Fiabilité hôte** : sur Vast.ai, filtrer `reliability > 0.98` et éviter les offres anormalement pas chères (surchargées).
5. **Disque trop petit** : 60 Go min (poids + cache + datasets) ; le resize après coup est galère.
6. **CUDA mismatch** : image `cu12.4` sur hôte driver 12.1 → vérifier `cuda_max_good` dans l'offre.
7. **SSH par mot de passe** : interdit — clé uniquement, et firewall sur les ports exposés.
8. **Données sensibles** : machine d'un tiers → jamais de secrets/clés réelles, jamais de données clients ; instance jetable.
9. **Scripts de minage** : certains templates communautaires sont douteux → images officielles ou tes propres Dockerfiles.
10. **Facturation à la seconde ≠ gratuite à l'arrêt** : « stopped » ≠ « destroyed » — seul `destroy`/`terminate` stoppe la facture (le disque peut continuer à coûter).
11. **Volume persistant oublié** : le network volume RunPod / Vast.ai se facture même sans instance.
12. **Ports exposés par défaut** : Jupyter sans token sur Internet = cadeau — token + auth systématiques.
13. **Timezone/fuseau** : les jobs longs planifiés en heures creuses (moins de concurrence sur le spot).
14. **Double facturation régions** : vérifier la devise et les taxes (VAT) sur la facture.
15. **Pas de SLA en community/spot** : jamais de prod client critique dessus.

## 97. Template RunPod « RAG-lab » (exemple)

```dockerfile
# Dockerfile — image perso à pousser sur Docker Hub puis utiliser comme template
FROM runpod/pytorch:2.4.0-py3.11-cuda12.4.1-devel-ubuntu22.04
RUN pip install vllm transformers sentence-transformers
# Pré-télécharger les poids à la construction (pas à chaque location)
RUN python -c "from huggingface_hub import snapshot_download; \
    snapshot_download('Qwen/Qwen3-32B-AWQ')"
COPY start.sh /start.sh
EXPOSE 8000 22
CMD ["/start.sh"]
```

```bash
# start.sh
#!/bin/bash
vllm serve Qwen/Qwen3-32B-AWQ --quantization awq \
  --host 0.0.0.0 --port 8000 --enable-prefix-caching &
sshd -D
```

→ Template RunPod pointant sur cette image : **pod vLLM prêt en ~2 min**, coût ≈ $0.34/h en community.

## 98. Auto-destruction : le garde-fou obligatoire

```bash
# À la fin de TON script de job (fine-tune, batch...) — Vast.ai
vastai destroy instance $VAST_INSTANCE_ID

# RunPod : via l'API dans le script Python de fin de job
import runpod, os
runpod.api_key = os.environ["RUNPOD_API_KEY_FAKE"]
runpod.terminate_pod(os.environ["RUNPOD_POD_ID"])

# Ceinture + bretelles : cron local qui tue les instances de plus de X heures
# (ex. toutes les 30 min, tue les pods de test de plus de 4 h)
```

Et côté fournisseur : **plafonds de dépense** (RunPod : limites dans les réglages ; Vast.ai : solde prépayé = limite naturelle — ne charger que ce qu'il faut).

## 99. Quand louer plutôt qu'acheter (décision)

| Situation | Verdict |
|---|---|
| < 1 000 h GPU/an, besoins ponctuels | **Louer** |
| Tests avant achat (70B ? 2× 4090 ?) | **Louer** pour décider |
| Fine-tune occasionnel, batch mensuel | **Louer** (spot) |
| Inférence 24/7 pour ton RAG perso | **Acheter** (seuil ~3 800 h) ou… gratuit cloud |
| Données sensibles / secret industriel | **Acheter** (ou on-prem pro) — jamais de P2P |
| Équipe, SLA, prod | **Secure cloud** (RunPod Secure, Lambda) ou on-prem |

## 100. Checklist location GPU

- [ ] Compte Vast.ai + RunPod, solde prépayé modeste (pas de CB à débit auto si possible).
- [ ] Clé SSH dédiée (`~/.ssh/id_gpu`) ajoutée aux deux plateformes.
- [ ] Template/image perso prêt (section 97) — ne jamais partir d'une image inconnue.
- [ ] Script de job avec **checkpoint + auto-destruction** (sections 95, 98).
- [ ] Estimation du coût AVANT (`prix/h × heures`) notée quelque part.
- [ ] Vérifier l'egress si rapatriement de poids (Vast.ai).
- [ ] Aucun secret réel sur l'instance (variables d'env jetables).

---

# PARTIE J — STRATÉGIE BUDGET ET OPÉRATIONS

## 101. Architecture cible « facture ~0 € » pour ton RAG

```
┌─────────────┐     ┌──────────────┐     ┌────────────────┐
│ Indexation  │────▶│ TEI (bge-m3) │────▶│ Chroma/pgvector│  (local, 0 €)
│ (batch)     │     │  :8080       │     │                │
└─────────────┘     └──────────────┘     └───────┬────────┘
                                                │ top-50
                                         ┌──────▼────────┐
                                         │ Reranker TEI  │  (local, 0 €)
                                         │  :8082        │
                                         └──────┬────────┘
                                                │ top-5 + question
                    ┌───────────────────────────▼───────────────────────────┐
                    │ Génération : ordre de tentative                        │
                    │ 1. vLLM local (Qwen3-32B-AWQ) — 0 €                   │
                    │ 2. Groq free (gpt-oss-120b) — 0 €                      │
                    │ 3. Cerebras free (1M tok/j) — 0 €                      │
                    │ 4. OpenRouter DeepSeek V4.1 ($0.15/$0.60) — ~0 €       │
                    │ 5. gpt-6-sol / sonnet-5 — payant, cas durs uniquement  │
                    └───────────────────────────────────────────────────────┘
```

## 102. Le routeur de génération (code complet)

```python
import os, time
from openai import OpenAI

PROVIDERS = [
    ("http://localhost:8000/v1", "Qwen/Qwen3-32B-AWQ", "vllm-local"),
    ("https://api.groq.com/openai/v1", "openai/gpt-oss-120b", "groq"),
    ("https://api.cerebras.ai/v1", "gpt-oss-120b", "cerebras"),
    ("https://openrouter.ai/api/v1", "deepseek/deepseek-v4.1-flash", "openrouter"),
]
KEYS = {
    "groq": os.environ.get("GROQ_API_KEY_FAKE", "gsk_FAKE"),
    "cerebras": os.environ.get("CEREBRAS_API_KEY_FAKE", "csk_FAKE"),
    "openrouter": os.environ.get("OPENROUTER_API_KEY_FAKE", "sk-or-v1_FAKE"),
    "vllm-local": "local",
}

def generer(messages, max_tokens=800):
    last_err = None
    for base_url, model, name in PROVIDERS:
        try:
            client = OpenAI(base_url=base_url, api_key=KEYS[name], timeout=60)
            r = client.chat.completions.create(
                model=model, messages=messages,
                max_tokens=max_tokens, temperature=0.2)
            log_usage(name, model, r.usage)   # section 103
            return r.choices[0].message.content, name
        except Exception as e:
            last_err = e
            time.sleep(1)  # backoff simple ; en prod : exponentiel + jitter
    raise RuntimeError(f"Tous les providers ont échoué : {last_err}")
```

## 103. Logger chaque token (la base du contrôle budgétaire)

```python
import sqlite3, time
db = sqlite3.connect("llm_usage.db")
db.execute("""CREATE TABLE IF NOT EXISTS usage(
  ts REAL, provider TEXT, model TEXT,
  prompt_tok INTEGER, completion_tok INTEGER)""")

def log_usage(provider, model, usage):
    db.execute("INSERT INTO usage VALUES (?,?,?,?,?)",
        (time.time(), provider, model,
         usage.prompt_tokens, usage.completion_tokens))
    db.commit()

# Coût estimé sur 30 jours par provider (prix en dur, à maintenir)
PRIX = {  # (in, out) $/1M — vérifiés le 27/09/2026
    "deepseek/deepseek-v4.1-flash": (0.15, 0.60),
    "openai/gpt-oss-120b": (0.15, 0.60),
    "gpt-oss-120b": (0.35, 0.75),
}
```

Dashboard minimal : une requête SQL par semaine → coût par provider, dérive éventuelle, top modèles. **Sans mesure, pas d'optimisation.**

## 104. Cache sémantique : le multiplicateur de gratuit

```python
import hashlib, json
def cache_key(messages):
    return hashlib.sha256(json.dumps(messages).encode()).hexdigest()

# Avant d'appeler generer() : chercher en SQLite/Redis.
# Après : stocker (question_hash -> réponse, provider, date).
# TTL : 7 jours pour la doc technique (change peu), 1 h pour l'actu.
```

En pratique sur un RAG d'équipe : **20–40 % des questions sont des doublons** (FAQ, procédures). Le cache les sert à coût 0 et latence ~5 ms.

## 105. Prompt engineering économique (5 techniques)

1. **Raccourcir le prompt système** : chaque token système est payé à chaque appel — 500 tokens × 10K appels = 5M tokens « invisibles ».
2. **Limiter `max_tokens`** : une réponse de 200 tokens coûte 4× moins qu'une de 800.
3. **Baisser la température** (0–0.2) : réponses plus courtes et déterministes → moins de tokens, cache plus efficace.
4. **Choisir le contexte utile** : top-5 rerankés > top-20 bruts (moins de tokens in, meilleure qualité).
5. **Structurer la sortie** (JSON) : évite les pavés verbeux, parse direct.

## 106. Batch et files d'attente (quand le temps réel n'est pas requis)

- Indexation, résumés de logs, étiquetage : **pas besoin de temps réel** → batcher la nuit sur le moins cher (local, ou batch API −50 % chez Groq).
- File simple : table SQLite `jobs(status, payload)`, worker qui dépile avec le provider le moins cher disponible.
- Exemple : 100K tickets à classifier → Groq free `gpt-oss-20b` à 30 RPM ≈ 56 h… ou **vLLM local** en 2 h à coût élec. Le local gagne dès que le volume est là.

## 107. Monitoring : ce qu'il faut surveiller

| Signal | Où | Alerte si |
|---|---|---|
| Coût / jour par provider | `llm_usage.db` (section 103) | > seuil (ex. 5 €/j) |
| Taux de 429 | logs routeur | > 5 % → changer de provider/ordre |
| Latence p95 génération | logs | > 10 s → fallback trop lent |
| Santé TEI/vLLM/Ollama | `/health`, systemd | down → bascule cloud auto |
| Quotas free restants | dashboard fournisseurs | < 20 % → prévenir |
| Dérive qualité | éval hebdo (20 questions) | score < baseline → rollback modèle |

## 108. Sécurité des clés API (règles dures)

1. **Jamais** de clé en dur dans le code / Git — variables d'environnement ou gestionnaire de secrets (Vault, 1Password CLI, `pass`).
2. Fichier `.env` : dans `.gitignore` dès le premier commit (vérifier avec `git log --all -p | grep sk-` de temps en temps).
3. Une clé **par usage** (dev, prod, tests) avec **limites de dépense** (OpenRouter le permet nativement).
4. Rotation : si une clé fuite (log, screenshot, repo public) → révoquer immédiatement, pas « demain ».
5. En local (Ollama/vLLM/TEI) : pas de clé du tout — un argument de plus pour le local.
6. Sur instance louée (Vast/RunPod) : clés **jetables** à faible plafond, révoquées après le job.

## 109. Sauvegarde et reproductibilité de l'infra

- Versionner : `docker-compose.yml`, `Modelfile`, `.env.example` (sans secrets), scripts de provisionning.
- Épingler : versions d'images (`:cpu-1.8.3`, pas `:latest` en prod), `model-id` + révision HF, versions de drivers.
- Sauvegarder : la base de vecteurs (ou le script de réindexation — moins lourd), `llm_usage.db`, les Modelfiles.
- Documenter : un `INFRA.md` avec le schéma (section 101), les endpoints, les quotas et leurs dates de vérification.

## 110. Coûts électriques du local (ne pas l'oublier)

- RTX 4090 en charge ≈ 300–450 W ; PC complet ≈ 400–600 W.
- À 0.20 €/kWh : 500 W × 24 h = 12 kWh/j ≈ **2.40 €/jour** en 24/7, soit ~72 €/mois.
- En usage intermittent (8 h/j, 5 j/sem) : ~16 €/mois.
- **Le local 24/7 n'est « gratuit » que si la machine tourne déjà** (serveur Proxmox existant — ton cas). Sinon, comparer avec le gratuit cloud (0 €, 0 W).

---

# PARTIE K — NOMS NON IDENTIFIÉS (HONNÊTETÉ)

## 111. Noms recherchés mais NON TROUVÉS au 27/09/2026

Recherche web sérieuse effectuée ; ces noms n'ont pas pu être confirmés comme des produits/services réels à cette date. **Ne pas les utiliser** dans des décisions ; re-vérifier avant toute action :

| Nom | Statut | Détail |
|---|---|---|
| **Claude Sonnet 4.8** | **NON TROUVÉ (sept 2026)** | Opus 4.8 existe (mentionné par Anthropic/GitHub), Sonnet 4.8 : aucune annonce trouvée. Ne pas confondre. |
| **Claude Haiku 5 / 5.5** | Partiellement | Haiku 4.5 confirmé ($1/$5). « Haiku 5.5 » évoqué comme « à venir » par la presse — pas de fiche produit trouvée. |
| **Gemini 3.5 Pro / Gemini 4** | **NON TROUVÉ (sept 2026)** | Famille Gemini 3.x (Flash) confirmée ; pas de « 3.5 Pro » ni « 4 » dans les résultats. |
| **Veo 4** | **NON TROUVÉ (sept 2026)** | Aucune annonce trouvée (vidéo Google). |
| **DeepSeek V4.1 Pro (annonce)** | **NON TROUVÉ (sept 2026)** | V4.1 Flash (10/09/2026) et V4 Pro (13/08/2026) confirmés ; pas de « V4.1 Pro ». |
| **GPT-6 Terra** | **NON CONFIRMÉ (sept 2026)** | « gpt-5.6-terra » ($2/$12) apparaît dans un comparatif tiers ; « GPT-6 Terra » : recherche antérieure infructueuse. À re-vérifier sur la page prix OpenAI. |
| **Llama 4 Behemoth** | **NON TROUVÉ (sept 2026)** | Llama 4 Scout/Maverick confirmés ; pas de Behemoth. |
| **Poolside Malibu** | **NON TROUVÉ (sept 2026)** | Poolside Laguna S 2.1 confirmé (OpenRouter :free) ; pas de « Malibu ». |
| **mxbai-rerank-v3.1-listwise** | **NON TROUVÉ (sept 2026)** | Famille mxbai-rerank existe en principe, cette version exacte non confirmée. |
| **ctxl-rerank / ReasonRank** | **NON TROUVÉ (sept 2026)** | Aucune fiche produit trouvée. |
| **Mythos (Anthropic)** | Existe mais **non public** | Évoqué comme modèle interne (~40 orgs) — pas d'accès public, inutile pour ton usage. |

**Noms en revanche IDENTIFIÉS** (contrairement au doute initial) : `freellmapi` = FreeLLMAPI (section 35) ; `harness` (DeepSeek) = DeepSeek Harness `dsh` (section 46).

---

# PARTIE L — GLOSSAIRE

## 112. Glossaire (A–M)

- **AWQ** : quantization 4-bit GPU (Activation-aware Weight Quantization).
- **Batching (dynamique/continu)** : regrouper les requêtes pour remplir le GPU ; le batching continu (vLLM) insère les nouvelles requêtes sans attendre la fin du batch.
- **BF16 / FP16** : flottants 16 bits — précision standard d'inférence.
- **Candle** : moteur Rust de Hugging Face (utilisé par TEI).
- **Context window (ctx)** : nombre max de tokens (entrée + sortie) par requête.
- **Embeddings** : vecteurs numériques représentant le sens d'un texte — la base du RAG.
- **Fallback** : bascule automatique sur un provider/modèle de secours.
- **GGUF** : format de modèle quantifié pour llama.cpp/Ollama.
- **GPTQ** : quantization 4-bit GPU (plus ancienne qu'AWQ).
- **KV cache** : mémoire des clés/valeurs d'attention — grandit avec contexte × batch.
- **LPU** : Language Processing Unit — puce d'inférence de Groq.
- **Matryoshka (MRL)** : embeddings à dimensions emboîtées — on peut tronquer le vecteur (1024→512) sans réentraîner.
- **MoE** : Mixture of Experts — seuls quelques « experts » sont actifs par token (ex. 671B total, ~37B actifs chez DeepSeek).

## 113. Glossaire (N–Z)

- **PagedAttention** : gestion mémoire paginée du KV cache (vLLM) — moins de gaspillage.
- **Prefix caching** : réutiliser le calcul du début de prompt (système + chunks RAG récurrents).
- **Quantization** : réduire la précision des poids (16→8→4 bits) pour diviser la mémoire.
- **Rate limit** : quota (req/min, tokens/min, req/jour) — au-delà : HTTP 429.
- **Reranker** : modèle qui re-classe des documents candidats par pertinence fine (cross-encoder).
- **RPM / RPD / TPM / TPD** : requêtes ou tokens par minute / jour.
- **Serverless (inférence)** : API sans machine à gérer, facturée à l'usage.
- **Spot / interruptible** : instance louée moins chère, préemptible par l'hôte.
- **TEI** : Text Embeddings Inference — serveur d'embeddings de Hugging Face.
- **Tiers** : niveau d'offre (free tier, pay-as-you-go…).
- **Token** : unité de texte (~4 caractères en anglais, ~2–3 en français) — unité de facturation.
- **TPS** : tokens par seconde — mesure de débit.
- **TTFT** : time to first token — latence perçue.
- **vLLM** : serveur d'inférence LLM haute performance (UC Berkeley).
- **WSE** : Wafer-Scale Engine — puce géante de Cerebras.
- **ZDR** : Zero Data Retention — garantie de non-conservation des données.

---

# QUIZ (10 questions + réponses)

## 114. Quiz — questions

1. Quelle est la différence fondamentale entre OpenRouter d'une part, Groq et Cerebras d'autre part ?
2. Cite les ordres de grandeur du free tier Groq (RPM) et Cerebras (tokens/jour).
3. Pourquoi le free tier de Google AI Studio est-il déconseillé pour des données professionnelles sensibles ?
4. Que signifie `provider: {"allow_fallbacks": true}` dans un appel OpenRouter ?
5. Quelle VRAM faut-il prévoir (ordre de grandeur) pour servir un modèle 32B en Q4_K_M avec 16K de contexte sur un seul GPU ?
6. Quelle est la différence entre AWQ et GGUF Q4_K_M en termes de matériel requis ?
7. Pourquoi faut-il activer `--enable-prefix-caching` dans vLLM pour un RAG ?
8. Qu'est-ce que FreeLLMAPI et quelle est sa limite d'usage affichée par le projet ?
9. Quel est l'intérêt d'ajouter un reranker (ex. bge-reranker-v2-m3) entre la récupération et la génération dans un RAG ?
10. Sur Vast.ai, quelle est l'erreur n°1 qui fait exploser la facture, et comment l'éviter ?

## 115. Quiz — réponses

1. OpenRouter est une **passerelle logicielle** (route vers des fournisseurs tiers, 300+ modèles) ; Groq et Cerebras sont des **fondeurs** (puces LPU / WSE) qui vendent leur propre inférence ultra-rapide sur des poids ouverts.
2. Groq : **~30 req/min** (quotas par modèle, 1K–14,4K req/jour) ; Cerebras : **1M tokens/jour** (~5 RPM).
3. Parce que Google peut **utiliser les prompts du free tier pour améliorer ses produits** (revue humaine possible) — banni pour du sensible.
4. Si le fournisseur routé échoue ou est rate-limité, OpenRouter **bascule automatiquement** sur un autre fournisseur du modèle — sans changer ton code.
5. Poids Q4 ≈ 20 Go + KV cache 16K ≈ 3–5 Go + marge système ≈ **24 Go** → une RTX 4090 24 Go (juste) ; confortable à 32 Go.
6. **AWQ = GPU NVIDIA uniquement** (4-bit, via vLLM) ; **GGUF Q4_K_M = CPU et GPU** (llama.cpp/Ollama, offload partiel possible).
7. Parce que le début du prompt (instructions système + chunks récurrents) est identique d'un appel à l'autre : le prefix caching **évite de le ré-encoder**, ce qui divise latence et calcul.
8. Un **proxy open source auto-hébergé** qui agrège les tiers gratuits de ~28 fournisseurs derrière un endpoint OpenAI-compatible avec failover auto ; le projet précise **« personal experimentation only »** (pas de prod).
9. La similarité cosinus sur embeddings est rapide mais grossière ; le cross-encoder **relit vraiment** query+document et re-classe finement → top-5 bien meilleur pour un coût local de ~10 ms/doc.
10. **Oublier de détruire l'instance** (`stopped` ≠ `destroyed`) → script d'auto-destruction en fin de job + solde prépayé limité.

---

# PIÈGES RÉCAPITULATIFS (15+)

## 116. Les 18 pièges à ne jamais oublier

1. **Confondre Groq et Grok** (xAI) — deux entreprises sans rapport.
2. **Promos prises pour des prix** : GLM 5.3 Flash ($0.075/$0.25), Mercury 2.5 ($0.04/$0.15) sont des promos de lancement.
3. **Fenêtres peak DeepSeek** : 2× le prix en semaine 01h–04h / 06h–10h UTC sur la route officielle.
4. **50 req/jour OpenRouter free** : quota d'essai, pas une infra — empiler les gratuits.
5. **429 non géré** : sans retry + backoff, ton appli meurt dès que le free tier sature.
6. **Données dans le gratuit** : Google/Mistral/DeepSeek peuvent entraîner sur tes prompts (opt-out à activer ou à éviter).
7. **Mélanger des embeddings** de deux modèles dans le même index — résultats faux silencieusement.
8. **Oublier `--auto-truncate`** en indexation TEI : un doc trop long fait échouer tout le batch.
9. **Q2/Q3 sans éval** : la dégradation qualité est réelle, à mesurer sur tes questions.
10. **KV cache oublié** dans le dimensionnement : +30–50 % au-delà des poids en RAG à gros contexte.
11. **Ollama exposé sans auth** sur le réseau : pas d'authentification native — reverse proxy obligatoire.
12. **Clé API dans Git** : une fois poussée, elle est compromise — rotation immédiate.
13. **Instance Vast.ai non détruite** : la facture tourne même « stopped ».
14. **Egress Vast.ai** : $0.01–0.02/Go — 140 Go de checkpoint = $2.80 à rapatrier.
15. **Licence du modèle** : CC-BY-NC ou Llama Community mal lue = risque juridique en entreprise.
16. **`:latest` en prod** : épingler versions d'images et `model-id` (reproductibilité).
17. **DeepSeek Harness en preview** : changements cassants possibles — labo, pas prod.
18. **Comparer sans éval** : le « meilleur modèle » n'existe pas hors de TON jeu de questions.

---

# CAS PRATIQUES

## 117. Cas 1 — « Je veux un RAG perso à 0 € »

1. TEI + `bge-m3` en Docker sur ton Proxmox (section 66) → embeddings.
2. Indexer ton corpus (scripts existants), top-50 → reranker `bge-reranker-v2-m3` → top-5.
3. Génération : Ollama `qwen3:32b` si tu as une 4090, sinon routeur section 102 (Groq → Cerebras → OpenRouter free).
4. Cache SQLite des Q/R (section 104).
5. Coût mensuel : **0 €** (+ élec existante). Qualité : à éval
...[truncated 8112 chars]
---

# PENSE-BÊTE (UNE PAGE)

## 123. Pense-bête : endpoints et commandes

```
OpenRouter   https://openrouter.ai/api/v1              clé sk-or-v1-...   GET /api/v1/models = vérité prix
Groq         https://api.groq.com/openai/v1            clé gsk_...        30 RPM free, jamais facturé (429)
Cerebras     https://api.cerebras.ai/v1                clé csk-...        1M tok/j free, 8K ctx cap free
HF Providers https://router.huggingface.co/v1          token hf_...       $0.10/mois free
Ollama       http://localhost:11434/v1                 (pas de clé)       ollama run qwen3:32b
vLLM         http://localhost:8000/v1                  (pas de clé)       --enable-prefix-caching
TEI embed    http://localhost:8080/v1/embeddings       (pas de clé)       bge-m3, 1024 dim
TEI rerank   http://localhost:8082/rerank              (pas de clé)       bge-reranker-v2-m3
DeepSeek H.  http://127.0.0.1:3080                     npx @deepseek-ai/dsh web
```

```
# Santé express de toute la stack locale
for p in 11434 8000 8080 8082; do
  curl -s -o /dev/null -w "port $p : %{http_code}\n" http://localhost:$p/health 2>/dev/null \
    || curl -s -o /dev/null -w "port $p : %{http_code}\n" http://localhost:$p/v1/models
done
nvidia-smi --query-gpu=name,memory.used,memory.total,utilization.gpu --format=csv
```

## 124. Pense-bête : VRAM

```
Poids ≈ params × octets  (FP16: 2 | Q8: 1 | Q4: 0,5)
+ KV cache ≈ 320 Ko/token (70B) → 32K ctx ≈ 10 Go
7B Q4 ≈ 5 Go  |  14B Q4 ≈ 9 Go  |  32B Q4 ≈ 20 Go  |  70B Q4 ≈ 40 Go
4090 24 Go → 32B Q4 + 16K ctx = sweet spot
```

## 125. Pense-bête : gratuit 2026 (quotas à re-vérifier)

```
Groq 30 RPM | Cerebras 1M tok/j | Gemini Flash ~1500 req/j (données réutilisables !)
OpenRouter :free 50 req/j (1000 si $10 chargés) | Mistral Experiment ~1 Md tok/mois @ 2 RPM
Codestral 30 RPM/2000 j | NVIDIA NIM ~40 RPM | Cohere 1000 req/mois (non commercial)
Cloudflare 10K neurons/j | HF $0.10/mois | DeepSeek 5M tokens offerts (one-shot)
```

## 126. Pense-bête : règles d'or

```
1. Gratuit d'abord, payant sur échec mesuré.      4. Jamais de secret réel sur instance louée.
2. 429 → retry + backoff, toujours.               5. Embeddings : jamais deux modèles dans un index.
3. Données sensibles → local ou ZDR, jamais free. 6. Tout prix : date de vérification notée.
```

---

# POUR ALLER PLUS LOIN

## 127. Feuille de route 4 semaines

- **Semaine 1** : comptes gratuits (Groq, Cerebras, Gemini, OpenRouter, Mistral) + script de comparaison (section 11) sur 20 questions de ton domaine.
- **Semaine 2** : TEI + bge-m3 en Docker, réindexation d'un échantillon, éval précision@5 vs text-embedding-3-small.
- **Semaine 3** : Ollama `qwen3:32b` (ou vLLM si GPU), routeur multi-provider (section 102), cache SQLite.
- **Semaine 4** : reranker TEI dans le pipeline, logging des coûts (section 103), décision : quoi en local, quoi en gratuit, quoi en payant.

## 128. Ressources officielles (vérifiées le 27/09/2026)

- OpenRouter : docs `openrouter.ai/docs`, modèles `openrouter.ai/models`, API `openrouter.ai/api/v1/models`
- Groq : `console.groq.com/docs` (modèles), `groq.com/pricing`
- Cerebras : `inference.cerebras.ai` (docs), `cerebras.ai/pricing` (à vérifier — montants relevés via sources tierces)
- FreeLLMAPI : `github.com/tashfeenahmed/freellmapi`, `freellmapi.co`
- DeepSeek Harness : `github.com/deepseek-ai/deepseek-harness`, doc `deepseek-harness.github.io/deepseek-harness`
- Ollama : `ollama.com`, catalogue `ollama.com/library`
- vLLM : `docs.vllm.ai`
- TEI : `github.com/huggingface/text-embeddings-inference`, spec API `huggingface.github.io/text-embeddings-inference`
- Hugging Face : `huggingface.co/docs/inference-providers`, `huggingface.co/pricing`
- Vast.ai : `vast.ai`, CLI `pip install vastai`
- RunPod : `runpod.io/docs`

## 129. Sujets à creuser ensuite (proposés)

1. **Fine-tuning LoRA** de Qwen3-32B sur tes docs d'onduleurs/réseau (Unsloth/LLaMA-Factory) — le niveau au-dessus du RAG.
2. **Éval RAG automatisée** (RAGAS) branchée sur ton pipeline TEI + reranker.
3. **Agents** : tester DeepSeek Harness sur une tâche réelle (génération de doc technique).
4. **Observabilité** : Prometheus + Grafana sur `/metrics` (vLLM, TEI) et coûts (`llm_usage.db`).
5. **Sécurité** : durcir l'exposition LAN (WireGuard + Caddy) pour l'équipe.

## 130. Historique des vérifications (ce guide)

| Date | Action |
|---|---|
| 27/09/2026 | Recherche web : OpenRouter, Groq, Cerebras, freellmapi, DeepSeek Harness, TEI, HF Inference Providers, Vast.ai, RunPod, free tiers, prix OpenAI/Anthropic |
| 27/09/2026 | Rédaction v1 — 130 sections |
| À faire (trimestriel) | Re-vérifier prix et quotas : ils bougent vite (promos, dépréciations de modèles) |

*Suite du guide — voir Partie M, sections 131–158.*

---

# PARTIE M — APPROFONDISSEMENTS

## 131. LiteLLM : la passerelle open source à héberger soi-même

En plus d'OpenRouter (SaaS) et FreeLLMAPI (gratuit), il existe **LiteLLM** (github.com/BerriAI/litellm, MIT) : un **proxy unifié open source** qui expose 100+ providers derrière une API OpenAI-compatible, avec **fallbacks, retry, cache (Redis), budgets par clé, et logs**.
C'est le choix « entreprise » du self-hosted : tu gardes le contrôle, tu branches tes clés (Groq, OpenAI, ton vLLM local…).

```yaml
# litellm_config.yaml
model_list:
  - model_name: rapide
    litellm_params:
      model: groq/openai/gpt-oss-120b
      api_key: os.environ/GROQ_API_KEY_FAKE
  - model_name: local
    litellm_params:
      model: openai/Qwen/Qwen3-32B-AWQ   # ton vLLM
      api_base: http://localhost:8000/v1
      api_key: local
router_settings:
  fallbacks: [{"rapide": ["local"]}]
```

```bash
pip install 'litellm[proxy]'
litellm --config litellm_config.yaml --port 4000
# Puis : base_url=http://localhost:4000, model="rapide"
```

Pour toi : LiteLLM = le **routeur central** de ton labo (un seul endpoint pour tout ton code), avec cache Redis et budgets. FreeLLMAPI = orienté gratuit ; LiteLLM = orienté contrôle/SLA.

## 132. LiteLLM vs OpenRouter vs FreeLLMAPI (tableau)

| Critère | LiteLLM (self-hosted) | OpenRouter (SaaS) | FreeLLMAPI (self-hosted) |
|---|---|---|---|
| Hébergement | Chez toi (Docker/pip) | Cloud | Chez toi (Docker) |
| Catalogue | 100+ providers (tes clés) | 300+ modèles (routés) | Tiers gratuits (~28 providers) |
| Fallback / retry | Oui (config) | Oui (`allow_fallbacks`) | Oui (auto) |
| Cache | Oui (Redis) | Non (côté provider) | Non |
| Budgets par clé | Oui | Oui (garde-fous) | Suivi conso |
| Coût | 0 (+ ta machine) | Marge sur tokens | 0 |
| Idéal | Labo/équipe, contrôle total | Simplicité, comparaison | Empiler le gratuit |

## 133. Structured outputs : forcer du JSON propre

Indispensable quand un LLM pilote du code (inventaire, tickets). Trois voies :

```python
# 1. OpenAI / Groq / Cerebras (support variable — vérifier par modèle)
r = client.chat.completions.create(
    model="openai/gpt-oss-120b",
    messages=[{"role": "user", "content": "Liste 2 onduleurs : nom, puissance_kva"}],
    response_format={"type": "json_object"},
)

# 2. JSON Schema strict (modèles compatibles)
r = client.chat.completions.create(
    model="deepseek/deepseek-v4.1-flash",
    messages=[{"role": "user", "content": "..."}],
    response_format={"type": "json_schema",
        "json_schema": {"name": "ond", "schema": {
            "type": "object",
            "properties": {"nom": {"type": "string"},
                           "puissance_kva": {"type": "number"}},
            "required": ["nom", "puissance_kva"],
            "additionalProperties": False}}},
)

# 3. vLLM local : guided decoding (grammaire garantie, moteur xgrammar)
# vllm serve ... --guided-decoding-backend xgrammar
# puis extra_body={"guided_json": {...schema...}}
```

Sur OpenRouter, ajoute `"provider": {"require_parameters": true}` pour ne router que vers des fournisseurs qui supportent le JSON Schema.

## 134. Le seuil du local : calcul de rentabilité (méthode)

```
Coût cloud mensuel  = tokens_mois/1e6 × prix_1M
Coût local mensuel  = (amortissement GPU / mois) + élec + ton temps

Exemple : 200M tokens/mois sur gpt-oss-120b @ $0.15/$0.60
  ≈ 150M in + 50M out → 150×0.15 + 50×0.60 = $22.5 + $30 = $52.5/mois
Local : RTX 4090 (1900 € / 36 mois ≈ 53 €/mois) + élec 16 € = ~69 €/mois
→ À ce volume, le cloud GAGNE encore. Le local gagne quand :
  - la machine existe déjà (serveur Proxmox allumé 24/7) → coût marginal ~0
  - le volume dépasse ~500M tokens/mois
  - ou la confidentialité l'exige (alors le prix ne compte plus)
```

**Règle** : ne pas acheter de GPU « pour le RAG » tant que le gratuit + un petit payant couvrent le besoin. Acheter quand le besoin est prouvé par les logs (section 103).

## 135. Prompt caching : diviser la facture par 2 à 10

Le prompt système + les chunks RAG se répètent : le **cache de prompt** évite de les re-facturer plein pot.

| Provider | Cache read /1M | Cache write /1M | Note (vérifié sept 2026) |
|---|---|---|---|
| Anthropic Opus 5.5 | **$0.20** | $5 | −60 % vs Opus 5 |
| Anthropic Sonnet 5 | $0.20 | $2.50 | |
| OpenAI gpt-6-astra | $1.00 | $12.50 | Contexte court |
| Groq | **−50 %** input caché | — | Réduction simple |
| vLLM local | 0 (prefix caching) | — | `--enable-prefix-caching` |

En pratique : ordre stable des messages (système → chunks → question), et le cache fait le reste. Sur un agent qui porte 50K tokens de contexte sur 20 tours, le cache **divise la facture par ~5**.

## 136. Batch API : −50 % quand le temps réel ne compte pas

Groq et d'autres offrent le **batch à −50 %** : tu soumets un fichier de requêtes, réponse en heures creuses. Idéal : indexation d'annotations, étiquetage de 100K tickets, évals massives. Workflow : `jobs` en SQLite (section 106) → soumission batch la nuit → résultats au matin. Le local (vLLM) reste imbattable si tu as le GPU, mais le batch cloud évite d'immobiliser ta machine.

## 137. Ollama avancé : variables d'environnement utiles

```bash
# /etc/systemd/system/ollama.service.d/override.conf (ou EnvironmentFile)
OLLAMA_HOST=0.0.0.0:11434        # exposer sur le LAN (avec reverse proxy !)
OLLAMA_NUM_PARALLEL=4            # requêtes simultanées (défaut 1 → à monter en équipe)
OLLAMA_MAX_LOADED_MODELS=2       # modèles gardés en VRAM
OLLAMA_KEEP_ALIVE=30m            # garder le modèle chargé 30 min après usage
OLLAMA_MAX_QUEUE=512
```

```bash
ollama ps                        # modèles chargés + VRAM utilisée
ollama show qwen3:32b            # paramètres, template, quantization
```

`OLLAMA_KEEP_ALIVE` évite le rechargement (10–30 s) entre deux questions espacées — essentiel pour un usage interactif sans latence de cold start.

## 138. vLLM avancé : ce qui change la donne en prod

- **`--quantization fp8`** : 2× moins de VRAM que FP16, perte quasi nulle sur GPU récents (Ada/Hopper) — le meilleur choix 2026 si ton GPU le supporte.
- **Tool calling** : vLLM supporte le function calling OpenAI-compatible sur les modèles adaptés (Qwen3, Llama 3.x) — ton agent local peut appeler des outils.
- **`/metrics`** : exporter vers Prometheus → Grafana (latence, file d'attente, KV cache utilisé).
- **Multi-modèles** : un vLLM = un modèle ; pour 2 modèles, 2 conteneurs (ou Ollama qui multiplexe).
- **Versioning** : épingler `vllm/vllm-openai:vX.Y.Z` — les régressions entre versions existent.

## 139. Stockage des vecteurs : Chroma vs pgvector vs Qdrant

| Solution | Install | Perf | Quand |
|---|---|---|---|
| **Chroma** | `pip install chromadb` (embarqué) | OK < 1M vecteurs | Proto, RAG perso |
| **pgvector** (Postgres) | Extension PG | Bon, SQL + vecteurs | Tu as déjà Postgres ; filtres métier en SQL |
| **Qdrant** | Docker 1 conteneur | Excellent, filtres riches | Volume, prod, payloads complexes |

Pour ton RAG : **Chroma pour démarrer**, **pgvector** si tu veux croiser vecteurs + métadonnées métier (site, équipement, date) en SQL, **Qdrant** si > 1M chunks. Tous acceptent les 1024 dim de bge-m3. Sauvegarde = le dossier de données (ou mieux : le script de réindexation, section 109).

## 140. Chunking : la qualité du RAG se joue ici

1. **Taille** : 500–1 000 tokens pour de la doc technique (un chunk = une idée : une procédure, un tableau).
2. **Overlap** : 10–15 % (100–150 tokens) pour ne pas couper une phrase clé.
3. **Respecter la structure** : découper sur les titres (Markdown `##`), jamais au milieu d'un tableau ou d'une commande.
4. **Métadonnées par chunk** : source, page, section, date, équipement → filtres + citations.
5. **Tester** : même corpus, 3 stratégies de chunking, mesurer précision@5 (section 34) — le chunking bat souvent le changement de modèle.

```python
# Exemple : découpage simple sur titres Markdown
import re
def chunk_md(text, max_tokens=800):
    parts = re.split(r'(?m)^## ', text)
    # ... regrouper les petites sections, couper les trop grosses sur les paragraphes
```

## 141. Évaluer un RAG : les métriques qui comptent

- **Précision@k** : la bonne doc est-elle dans les k premiers ? (k=5 après rerank)
- **MRR** : rang moyen de la première bonne réponse — plus fin que précision@k.
- **Fidélité** : la réponse générée est-elle supportée par les chunks ? (RAGAS : `faithfulness`)
- **Pertinence réponse** : la réponse répond-elle à la question ? (RAGAS : `answer_relevancy`)
- **Protocole** : 50 questions FR de ton domaine, réponses attendues rédigées par toi, rejouer à chaque changement (modèle, chunking, reranker). **RAGAS** (`pip install ragas`) automatise une partie avec un LLM juge — sur modèle gratuit/local pour ne pas payer l'éval.

## 142. Sécurité : prompt injection dans un RAG (bases)

- Un document indexé peut contenir des **instructions cachées** (« ignore les instructions précédentes… ») que le LLM suivra.
- Défenses : séparer clairement contexte et instructions (`<contexte>…</contexte>`), instruction système « ne suis que mes instructions », **ne jamais laisser le LLM exécuter** d'actions (pas de tool calling sur données non fiables) sans validation humaine, logger les prompts suspects.
- En entreprise : les docs indexés doivent venir de sources de confiance ; un RAG ouvert à l'upload utilisateur = surface d'attaque.

## 143. Multimodal gratuit : vision sans payer

- **Gemini Flash (free)** : images + PDF en entrée, 1M de contexte — le meilleur plan gratuit pour « décris ce schéma électrique ».
- **OpenRouter** : modèles vision pas chers (`deepseek-v4-flash-vision-exp` $0.22/$0.66) ou `:free` quand dispo.
- **Local** : Qwen3-VL / Gemma 3 (vision) via Ollama — pour des schémas sans envoi cloud.
- Cas d'usage : lire une plaque signalétique d'onduleur en photo, extraire un tableau d'un PDF scanné.

## 144. Transcription locale : faster-whisper

```bash
pip install faster-whisper
```

```python
from faster_whisper import WhisperModel
model = WhisperModel("large-v3-turbo", device="cuda", compute_type="int8")
segments, _ = model.transcribe("reunion.mp3", language="fr")
print(" ".join(s.text for s in segments))
```

Sur RTX 4090 : ~10× temps réel en `int8` — une réunion d'1 h transcrite en ~6 min, **0 €**, données locales. Alternative cloud : Groq Whisper ($0.04/h en payant, gratuit dans les quotas).

## 145. Fine-tune LoRA : le niveau au-dessus (aperçu)

Quand le RAG ne suffit pas (style, vocabulaire métier très spécifique) : **LoRA** sur un 7–8B avec **Unsloth** (le plus rapide en 2026).

```bash
pip install unsloth
```

```python
from unsloth import FastLanguageModel
model, tok = FastLanguageModel.from_pretrained("unsloth/Qwen3-8B",
    max_seq_length=4096, load_in_4bit=True)
model = FastLanguageModel.get_peft_model(model, r=16,
    target_modules=["q_proj","k_proj","v_proj","o_proj"],
    lora_alpha=16, lora_dropout=0)
# ... entraînement SFTTrainer sur tes paires instruction/réponse
```

Budget : 8 h sur RTX 4090 ≈ **$3–6** en location (section 95). 100K paires d'instructions suffisent pour un style métier. **Le LoRA ne remplace pas le RAG** (connaissances factuelles) : il apprend le *comment répondre*, le RAG apporte le *quoi*.

## 146. Vast.ai avancé : gérer le spot comme un pro

```bash
# Chercher avec filtres stricts (prix + fiabilité + bande passante disque)
vastai search offers \
  'gpu_name=RTX 4090 num_gpus=1 reliability>0.985 inet_down>500 disk_space>80' \
  -o 'dph_total-'

# Script de job robuste : checkpoint + reprise + auto-destruction
# train.sh :
#   1. restaure le dernier checkpoint depuis le volume persistant (si existe)
#   2. entraîne avec sauvegarde toutes les 500 steps
#   3. à la fin (ou sur SIGTERM de préemption) : sync + vastai destroy instance $ID
trap 'sync_checkpoints; vastai destroy instance $VAST_ID' SIGTERM
```

**Volumes persistants** Vast.ai : les poids/checkpoints survivent à la destruction de l'instance — indispensable avec du spot (la préemption devient un non-événement).

## 147. RunPod serverless : ton API d'inférence sans serveur

```python
# handler.py — RunPod serverless (scale-to-zero : 0 € quand inutilisé)
import runpod
from openai import OpenAI
client = OpenAI(base_url="http://localhost:8000/v1", api_key="local")  # vLLM côte à côte

def handler(job):
    prompt = job["input"]["prompt"]
    r = client.chat.completions.create(model="Qwen/Qwen3-32B-AWQ",
        messages=[{"role": "user", "content": prompt}], max_tokens=500)
    return {"reponse": r.choices[0].message.content}

runpod.serverless.start({"handler": handler})
```

Cas d'usage : endpoint privé pour ton équipe, facturé à la milliseconde, **zéro quand personne ne l'appelle**. Le chaînon manquant entre « mon PC » et « une infra ».

## 148. Comparer les clouds GPU (rappel élargi, sept 2026)

| Provider | RTX 4090 | A100 80G | H100 80G | Egress | Spot |
|---|---|---|---|---|---|
| Vast.ai | $0.29–0.59 | $0.75–1.45 | $1.65–2.90 | $0.01–0.02/Go | Oui (P2P) |
| RunPod | $0.34 / $0.59–0.74 | $1.19 / $1.89–2.49 | $2.49 / $2.99–3.89 | **Gratuit** | Non (community fixe) |
| Lambda Labs | n/a | $1.29–1.89 | $2.49–3.29 | Gratuit | Non |
| AWS EC2 | n/a (A10G $1.21) | $4.10 | $5.16 | $0.05–0.09/Go | Oui (complexe) |

Prix constatés avril–sept 2026 via comparatifs tiers — **à re-vérifier**, surtout le spot (enchères).

## 149. Réseau et accès distant : Tailscale/WireGuard pour ton labo

Plutôt que d'exposer tes ports (11434, 8000, 8080) : **Tailscale** (ou WireGuard pur) crée un VPN mesh entre ton PC, ton serveur Proxmox et tes instances cloud. Tes API locales restent sur `127.0.0.1`/LAN, accessibles uniquement via le VPN — **zéro surface Internet**, zéro TLS à gérer pour l'interne. C'est la bonne réponse au piège n°11 (Ollama sans auth).

## 150. Sauvegarde de la stack : le script minimal

```bash
#!/bin/bash
# backup-llm.sh — hebdo via cron
DATE=$(date +%F)
tar czf /backup/chroma-$DATE.tgz /data/chroma
sqlite3 llm_usage.db ".backup '/backup/usage-$DATE.db'"
cp docker-compose.yml Modelfile litellm_config.yaml /backup/
# Rétention : garder 4 semaines
find /backup -name "*.tgz" -mtime +28 -delete
```

Ce qui compte n'est pas la base de vecteurs (reconstructible) mais : **les configs, les Modelfiles, les logs d'usage, et le script de réindexation**.

## 151. Mises à jour : politique de MAJ de la stack IA

- **Mensuel** : images Docker TEI/Ollama/vLLM (lire le changelog — les régressions existent).
- **Trimestriel** : re-vérifier prix/quotas des providers (ce guide a une date de péremption, section 130) ; re-tester les modèles gratuits (le catalogue tourne).
- **Jamais en aveugle** : snapshot Proxmox avant MAJ, éval de 20 questions après MAJ (non-régression).
- **Épinglage** : en prod, `image:tag` fixe + `model-id` + révision — `latest` interdit (piège n°16).

## 152. Coûts cachés : la liste finale

1. Électricité du local 24/7 (section 110).
2. Egress Vast.ai / AWS (sections 96, 148).
3. Volumes persistants facturés sans instance (piège n°11).
4. Réindexations répétées (API embeddings payante).
5. Prompts système trop longs (section 105).
6. Modèles dépréciés → migration en urgence (épingler + surveiller).
7. Données d'entraînement : le « gratuit » qui coûte en confidentialité (section 42).
8. Ton temps : une stack locale, ça se maintient — le valoriser dans le calcul (section 134).

## 153. Décider en 5 minutes : la fiche modèle

Avant de figer un modèle, remplir ceci (10 min, évite des semaines de dette) :

```
Modèle : ________________  Provider : ________________  Date vérif : __________
Prix in/out/1M : ________  Contexte : ________  Latence p95 : ________
Free tier ? ____ Quotas : ____  Données entraînées ? ____ ZDR ? ____
Licence (si local) : ________  VRAM Q4 : ________
Score sur mes 30 questions : ____/30  vs baseline : ____/30
Décision : [ ] adopter  [ ] rejeter (motif : ________)
```

## 154. Lexique des erreurs HTTP à connaître

| Code | Signification | Réaction |
|---|---|---|
| 401 | Clé invalide/absente | Vérifier la clé, sa révocation |
| 402 | Crédits épuisés | Recharger (OpenRouter) ou changer de provider |
| 404 | Modèle inconnu | ID exact ? Modèle déprécié ? |
| 429 | Rate limit / quota | **Backoff exponentiel**, failover (c'est le régime normal du gratuit) |
| 500/502/503 | Provider en panne | Failover auto (sections 9, 102) |
| 529 | Surcharge (Anthropic) | Retry + fallback |

Le 429 n'est pas une erreur, c'est un **signal de pilotage** : il dit « change de provider ».

## 155. Backoff exponentiel : l'implémentation propre

```python
import random, time
def appel_avec_retry(fn, essais=5):
    for i in range(essais):
        try:
            return fn()
        except RateLimitError:          # 429 — adapter à ton client HTTP
            if i == essais - 1: raise
            delai = min(2 ** i + random.uniform(0, 1), 60)
            time.sleep(delai)           # 1s, 2s, 4s, 8s, 16s + jitter
        except ServerError:             # 5xx
            time.sleep(5)
```

Jitter obligatoire : sans lui, N clients re-tentent en même temps et re-saturent le provider (thundering herd).

## 156. Ce que ce guide ne couvre pas (périmètre honnête)

- L'entraînement complet (pre-training) — hors de portée d'un labo perso.
- Le fine-tuning avancé (DPO, RLHF) — aperçu LoRA seulement (section 145).
- La conformité réglementaire (AI Act UE, etc.) — à traiter avec ton DPO/juriste avant usage pro des modèles.
- Les détails d'optimisation CUDA/Triton — niveau ingénierie ML.
- Les agents multimodaux complexes — DeepSeek Harness en preview (section 49).

## 157. Vérification finale avant mise en prod (checklist)

- [ ] Éval 30+ questions : score ≥ baseline, 0 hallucination critique.
- [ ] Fallback testé : couper le provider principal, vérifier la bascule < 5 s.
- [ ] 429 simulé : le retry/backoff fonctionne, pas de boucle infinie.
- [ ] Budgets : limites par clé, alertes de coût, dashboard hebdo.
- [ ] Secrets : aucune clé dans Git, rotation documentée.
- [ ] Données : classification (sensible → local/ZDR uniquement).
- [ ] Santé : `/health` de chaque service + alerte down.
- [ ] Backup : configs + script de réindexation testés (restauration essayée une fois).
- [ ] Coût projeté : calculé sur 3 mois avec les logs réels (pas au doigt mouillé).
- [ ] Doc : `INFRA.md` à jour (endpoints, quotas, dates de vérification).

## 158. Le mot de la fin : ta doctrine

1. **Mesure d'abord** (tokens, coûts, qualité) — tout le reste en découle.
2. **Gratuit + local** couvrent 95 % d'un usage perso/étude — le payant est un scalpel, pas un marteau.
3. **Un seul endpoint** dans ton code (LiteLLM, routeur maison, ou FreeLLMAPI) — jamais de provider en dur.
4. **Éval continue** : les modèles changent tous les mois, tes 30 questions sont ton ancre.
5. **Données** : le vrai coût du gratuit — traite tes prompts comme des données de prod.

*Fin de la partie principale du guide — 158 sections. La section 159 « À venir » suit ci-dessous ; son contenu est daté et doit être recontrôlé à chaque changement de mois.*
## 159. À venir — annonces vérifiées au 27/09/2026

Cette section est la seule du guide qui parle du futur, et elle le fait avec des règles strictes : **uniquement des sorties officiellement annoncées**, avec la source et la date d'annonce. Tout le reste est marqué « rien d'officialisé » ou « RUMEUR non confirmée ». Relis cette section à chaque début de mois : les annonces ci-dessous seront déjà périmées pour la plupart.

### 159.1. Règles de lecture (à appliquer avant de citer quoi que ce soit)

1. **ANNONCÉ** = le nom du modèle/produit apparaît dans une communication officielle de l'éditeur (changelog, pricing page, communiqué, thread de lancement officiel) ou dans les propos d'un membre identifié de l'équipe technique cités par plusieurs sources.
2. **RUMEUR** = information rapportée par la presse ou des fuites, non reprise par l'éditeur. Marquée explicitement « RUMEUR non confirmée ».
3. **RIEN D'OFFICIALISÉ** = aucune annonce trouvée au 27/09/2026 après recherche web. Ce n'est pas « ça n'existe pas », c'est « l'éditeur ne l'a pas annoncé ».
4. Un prix, un benchmark ou une date qui ne viennent pas d'une source vérifiable restent « à vérifier ».
5. Ne cite jamais un nom de modèle futur dans une config de prod (LiteLLM, FreeLLMAPI, scripts) : un nom annoncé n'est pas un endpoint qui répond.

### 159.2. DeepSeek V4.1 Pro — ANNONCÉ, non sorti (vérifié le 27/09/2026)

Le cas d'école de l'annonce qui existe mais du produit qui n'existe pas encore.

| Point | État au 27/09/2026 |
|---|---|
| Annonce | **Le 09/09/2026**, un membre de l'équipe technique DeepSeek (pseudonyme `@tianyi`) a expliqué que, une fois DeepSeek V4.1 Flash officiel, les requêtes adressées à `deepseek-v4-pro` seraient reroutées vers V4.1 Flash aux tarifs Flash « jusqu'au lancement de DeepSeek V4.1 Pro ». Le nom « V4.1 Pro » a donc été employé **une seule fois** côté vendeur. |
| Sortie | **Non sorti.** Le plan de reroutage du 14/09/2026 a été **retiré** sous la pression des développeurs : DeepSeek a ensuite indiqué sur sa page pricing que le service V4 Pro continue. |
| Model card / poids open-weight | **Aucun.** Le dépôt le plus récent de l'organisation `deepseek-ai` sur Hugging Face au 27/09/2026 est `DeepSeek-V4.1-Flash` (créé le 10/09/2026). Rien nommé « V4.1 Pro » n'existe, ni sur le Hub ni ailleurs. |
| Prix | **Aucun publié.** Pour l'échelle : V4.1 Flash = $0.15/$0.60 par million de tokens hors pic ($0.30/$1.20 en pic), V4 Pro = $0.66/$1.98 hors pic ($1.32/$3.96 en pic) — prix vérifiés le 27/09/2026 via les pages pricing DeepSeek relayées par des tiers. Un successeur serait logiquement attendu près de la colonne Flash, c'est précisément pour ça que le plan de retrait existait. |
| Endpoints API | **Aucun.** L'API DeepSeek expose exactement deux noms : `deepseek-flash` (V4.1 Flash) et `deepseek-v4-pro` (V4 Pro). Aucun endpoint V4.1 Pro, aucune limite de débit publiée pour un tel modèle. |
| Ce qui est sorti à la place | **DeepSeek V4.1 Flash**, le 10/09/2026 : architecture Causal Encoder-Decoder (552B MoE, 8B actifs en entrée / 16B en sortie), entrée image native, contexte 1M tokens, jusqu'à ~384K tokens de sortie, identifiant API `deepseek-flash`. Les anciens IDs `deepseek-v4-flash` et `deepseek-v4-flash-vision-exp` sont retirés mais toujours acceptés et routés vers V4.1 Flash. Les alias historiques `deepseek-chat` et `deepseek-reasoner` sont arrivés à leur date de retrait le 24/07/2026. |

Sources (vérifiées le 27/09/2026) : changelog officiel DeepSeek (`api-docs.deepseek.com`, note du 10/09/2026), page pricing DeepSeek (continuité du service V4 Pro), synthèse d'orcarouter.ai (« DeepSeek V4.1 Pro Leak: A Window That Closes Sept 30 »), note de recherche codex-router du 11/09/2026, benchlm.ai (historique des versions).

**Leçon infra pour Zelef :** c'est exactement pour ce genre de situation que la section 142 (versions épinglées) existe. Le 10/09/2026, `deepseek-v4-pro` a failli changer de modèle sous-jacent du jour au lendemain, puis ne l'a pas fait. Si ton routeur maison utilise des alias vendeur au lieu d'IDs datés, tu subis ce genre de décision. Épingle, loggue le modèle réel retourné, et mets une alerte sur les changements de changelog.

### 159.3. Anthropic — Sonnet 5.5 et Haiku 5.5 ANNONCÉS (sortie « dans les prochaines semaines »)

| Point | État au 27/09/2026 |
|---|---|
| Annonce | **Semaine du 19–25/09/2026** : Anthropic indique que **Sonnet 5.5 et Haiku 5.5 arrivent dans les prochaines semaines** (digest hebdomadaire EveryDev.ai, citant Anthropic). |
| Contexte déjà sorti | **Claude Opus 5.5** est sorti la semaine du 22/09/2026 : $4/$20 par million de tokens (entrée/sortie) contre $5/$25 pour Opus 5, lectures de cache à $0.20/M (contre $0.50), vitesse de sortie annoncée +30 % par rapport à Opus 5. Performances comparables au haut de gamme Fable 5.1 selon Anthropic, à coût d'exploitation inférieur. Prix vérifiés le 27/09/2026 via le digest EveryDev.ai citant Anthropic. |
| Autre fait officiel récent | **Claude Fable 5.1 et Claude Mythos 5.1**, lancés le **01/09/2026**. Mythos 5.1 est réservé aux institutions vérifiées via des programmes d'accès de confiance (cybersécurité, sciences de la vie) — c'est l'exemple type du « capability-tiered gating » de septembre 2026 : les capacités les plus risquées sont cloisonnées par l'éditeur, pas par l'utilisateur. |
| Ce qui n'est pas annoncé | **Sonnet 4.8** : rien d'officialisé au 27/09/2026 (reste dans la liste des « non trouvés » du guide). **Haiku 5** (sans le « .5 ») : rien d'officialisé ; seul Haiku 5.5 a été évoqué comme à venir. |

Sources (vérifiées le 27/09/2026) : EveryDev.ai « Weekly AI Dev News Digest: September 19 - 25, 2026 », TechTarget (24/09/2026, analyse des lancements OpenAI/Anthropic), wowtale.net (05/09/2026, gating par capacités des quatre labs).

### 159.4. OpenAI — GPT-6 Sol et Luna sortis le 22/09/2026 ; teaser DevDay ; GPT-6 Terra non annoncé

| Point | État au 27/09/2026 |
|---|---|
| Sorti le 22/09/2026 | **GPT-6 Sol** (code et agents, ~2× moins d'erreurs que son prédécesseur selon OpenAI, fiabilité niveau Astra) et **GPT-6 Luna** (volume bureautique : résumés, extraction de champs, questions courtes), **à moitié prix de la série 5.6** — économie attribuée par OpenAI à des améliorations de caching et d'inférence. Les prix par token ne figurent pas dans l'annonce. |
| Sorti le 03/09/2026 | **GPT-6 Astra**, décrit par OpenAI comme son système le plus capable et le plus aligné à ce jour. |
| Teaser officiel | OpenAI (via « Thibault », équipe OpenAI) a teasé des annonces **de niveau DevDay** pour la semaine du 16/09/2026 (digest AI Builders du 16/09/2026). Au 27/09/2026 : teaser confirmé, contenu non détaillé dans les sources consultées — à vérifier sur le blog OpenAI. |
| Non annoncé | **GPT-6 Terra** : rien d'officialisé au 27/09/2026. Le nom circule dans les listes de rumeurs mais n'apparaît dans aucune annonce officielle trouvée. **Ne pas ajouter ce modèle** (consigne explicite du guide). |
| Accès restreint | Les capacités de cybersécurité d'Astra ayant franchi le seuil « Critical » du Preparedness Framework d'OpenAI sont réservées aux entreprises vérifiées via le programme Daybreak (sur dossier). |

Sources (vérifiées le 27/09/2026) : EveryDev.ai (digest 19–25/09/2026, citant TechCrunch et OpenAI), startupfortune.com (semaine des quatre lancements), digest GitHub to-real/ai-builder-digest du 16/09/2026.

### 159.5. Meta — plus haut niveau de raisonnement RETENU (annoncé), Muse Spark 1.3 sorti

| Point | État au 27/09/2026 |
|---|---|
| Annoncé | Meta **retient son plus haut niveau de raisonnement** en attendant la fin de tests de sécurité supplémentaires (wowtale.net, 05/09/2026, dans le cadre de la couverture de la semaine des quatre lancements). C'est une annonce de roadmap : le niveau existe, sa diffusion générale est conditionnée aux tests. |
| Sorti le 02/09/2026 | **Muse Spark 1.3** (Meta), dans la vague de lancements de la semaine du 31/08 au 03/09/2026. |
| Non annoncé | **Llama 4 Behemoth** : rien d'officialisé au 27/09/2026. |

### 159.6. xAI — Grok 4.7 sorti ; rien d'annoncé au-delà

| Point | État au 27/09/2026 |
|---|---|
| Sorti | **Grok 4.7**, semaine du 19–25/09/2026. Tarification à deux étages, vérifiée le 27/09/2026 (digest EveryDev.ai citant xAI) : **en dessous de 200K tokens de prompt** : $2 entrée / $0.50 entrée en cache / $6 sortie par million ; **au-dessus de 200K** : $4 / $1 / $12. Fenêtre de contexte inchangée à 500K (identique à Grok 4.6) — seul le bout lointain de la fenêtre coûte plus cher. Aucune annonce de benchmark plaçant 4.7 devant Claude ou GPT-6 publiée par xAI. |
| Non annoncé | Aucune version ultérieure officialisée au 27/09/2026. |

### 159.7. Google — Gemini 3.8 Flash sorti ; Gemini 4 NON ANNONCÉ

| Point | État au 27/09/2026 |
|---|---|
| Sorti | **Gemini 3.8 Flash**, semaine du 31/08 au 03/09/2026 (vague des quatre lancements). Google a couplé son modèle orienté code à une variante cybersécurité distincte, réservée aux opérateurs gouvernementaux et d'infrastructures de confiance via le programme Fairwind. |
| Non annoncé | **Gemini 4** : rien d'officialisé au 27/09/2026. **Ne pas ajouter ce modèle** (consigne explicite du guide, comme GPT-6 Terra). |
| Non annoncé | **Veo 4** : rien d'officialisé au 27/09/2026. |

### 159.8. Chine : Alibaba, Moonshot, Zhipu, ByteDance — rien d'officialisé au-delà du sorti

| Éditeur | État au 27/09/2026 |
|---|---|
| Alibaba (Qwen) | Rien d'officialisé au-delà des modèles déjà sortis (Qwen 3.6/3.8, HY3/Hunyuan 3 en open-weight Apache 2.0). |
| Moonshot (Kimi) | Rien d'officialisé au-delà des modèles déjà sortis (K2.5). |
| Zhipu (GLM) | GLM 5.3 sorti (cité dans les comparatifs de benchmarks de septembre 2026). Rien d'officialisé au-delà. |
| ByteDance (Doubao) | **Doubao Seed 2.1 Pro et Turbo** sortis le **24/06/2026** (conférence Volcano Engine FORCE, 256K de contexte, Pro = deep-thinking, Turbo = moitié prix et basse latence). Tarifs Volcano Engine vérifiés le 27/09/2026 : Seed 2.1 Pro ¥6/¥30 par million (entrée/sortie), Turbo ¥3/¥15, entrée en cache ¥1.20 ; Seed 2.0 Pro ¥3.20/¥16 (entrées ≤ 32K). Via l'agrégateur ofox.ai (USD, vérifié le 27/09/2026) : Pro $0.884/$4.42, cache $0.177 ; Turbo $0.442/$2.212, cache $0.085. Rien d'officialisé au-delà de Seed 2.1 au 27/09/2026. |

### 159.9. RUMEURS non confirmées — à ne jamais mettre en prod

1. **RUMEUR non confirmée — Anthropic préparerait un nouveau modèle pour répondre à GPT-6 Astra.** Reuters a rapporté le 18/09/2026 qu'Anthropic « envisageait » un nouveau modèle, alors qu'Astra capterait ~13 % des dépenses IA business suivies par Ramp (contre ~8 % pour Claude Fable). Anthropic n'a rien confirmé. C'est une rumeur de presse, pas une annonce. (Sources : daylila.com 23/09/2026 citant Reuters ; aiagentsdirectory.com 20/09/2026.)
2. **RUMEUR non confirmée — fenêtre de sortie de DeepSeek V4.1 Pro.** Le billet orcarouter.ai évoque une « fenêtre qui se ferme le 30 septembre », mais DeepSeek n'a publié ni date ni fenêtre officielle. Le nom n'apparaît ni dans le changelog, ni sur la page pricing, ni dans un papier. Ne pas planifier de migration dessus.
3. **RUMEUR non confirmée — Poolside Malibu.** Nom circulant dans les listes de modèles non trouvés ; aucune annonce officielle au 27/09/2026.

### 159.10. Tableau récapitulatif — ce qui est annoncé vs ce qui ne l'est pas

| Nom | Statut au 27/09/2026 | Source de l'annonce |
|---|---|---|
| DeepSeek V4.1 Pro | **ANNONCÉ, non sorti** (09/09/2026, équipe DeepSeek) | api-docs.deepseek.com (news 10/09/2026), pricing page |
| Claude Sonnet 5.5 | **ANNONCÉ, « dans les prochaines semaines »** | Anthropic (via digest EveryDev.ai 19–25/09/2026) |
| Claude Haiku 5.5 | **ANNONCÉ, « dans les prochaines semaines »** | Anthropic (via digest EveryDev.ai 19–25/09/2026) |
| Meta — plus haut tier de raisonnement | **ANNONCÉ, retenu jusqu'aux tests de sécurité** | wowtale.net 05/09/2026 |
| OpenAI — annonces « niveau DevDay » | **Teaser officiel**, contenu à vérifier | Équipe OpenAI (digest 16/09/2026) |
| Sonnet 4.8 | Rien d'officialisé | — |
| Haiku 5 (sans .5) | Rien d'officialisé (seul Haiku 5.5 évoqué) | — |
| Gemini 4 | Rien d'officialisé — **ne pas ajouter** | — |
| Veo 4 | Rien d'officialisé | — |
| GPT-6 Terra | Rien d'officialisé — **ne pas ajouter** | — |
| Llama 4 Behemoth | Rien d'officialisé | — |
| Poolside Malibu | Rien d'officialisé (RUMEUR) | — |
| Qwen / Kimi / GLM / Doubao (générations suivantes) | Rien d'officialisé | — |

### 159.11. Comment suivre les annonces sans te faire piéger (méthode)

```bash
# 1. Les changelogs officiels d'abord (exemples à bookmarker)
#    - https://api-docs.deepseek.com/news/
#    - https://platform.openai.com/docs/changelog  (ou le blog OpenAI)
#    - https://docs.anthropic.com/en/release-notes
#    - https://cloud.google.com/vertex-ai/generative-ai/docs/release-notes (Gemini)
#    - https://docs.x.ai/docs/changelog
# 2. Les pages pricing : une annonce y apparaît AVANT les blogs tiers
#    (cf. DeepSeek : la continuité de V4 Pro s'est lue sur la page pricing,
#     pas dans un communiqué).
# 3. Règle des 3 sources : un nom de modèle n'entre dans ton routeur que si
#    (a) l'éditeur le nomme, (b) un endpoint ou un ID d'API existe,
#    (c) le prix est publié. Il en manque un ? C'est « à vérifier ».
# 4. Dans ton RAG : taggue chaque fiche modèle avec "vérifié le <date>" et
#    une date d'expiration de 30 jours. Au-delà, la fiche passe en
#    « à recontrôler » automatiquement (script section 128).
```

Script de veille minimal (à adapter) :

```python
# watch_models.py — alerte quand un changelog officiel bouge
import hashlib, urllib.request, json, os
WATCH = {
    "deepseek_news": "https://api-docs.deepseek.com/news/news260910",
    "anthropic_releases": "https://docs.anthropic.com/en/release-notes",
}
state_file = os.path.expanduser("~/.cache/model_watch.json")
state = json.load(open(state_file)) if os.path.exists(state_file) else {}
changed = []
for name, url in WATCH.items():
    try:
        data = urllib.request.urlopen(url, timeout=20).read()
        h = hashlib.sha256(data).hexdigest()
        if state.get(name) and state[name] != h:
            changed.append(name)
        state[name] = h
    except Exception as e:
        print(f"[{name}] erreur de lecture : {e} (à vérifier manuellement)")
json.dump(state, open(state_file, "w"))
print("CHANGÉ :", changed if changed else "rien — relire manuellement quand même")
```

### 159.12. Checklist anti-rumeur (à coller dans ton INFRA.md)

- [ ] Le nom apparaît-il dans une communication **de l'éditeur** (pas un blog tiers) ?
- [ ] Un **ID d'API ou un endpoint** existe-t-il (pas seulement un nom) ?
- [ ] Le **prix** est-il publié sur une page officielle ?
- [ ] La **date d'annonce** est-elle connue et notée ?
- [ ] Si une seule de ces cases est vide : le modèle est « à vérifier », pas « disponible ».
- [ ] Interdiction de coder un nom de modèle futur en dur dans LiteLLM/FreeLLMAPI/scripts.
- [ ] Toute fiche RAG sur un modèle futur porte « vérifié le … » + expiration 30 jours.

### 159.13. Ce que ça change pour ton RAG (Zelef)

1. **Les 30 questions d'éval (section 127)** doivent inclure 2–3 questions « piège temporel » du type : « DeepSeek V4.1 Pro est-il disponible ? » — la bonne réponse au 27/09/2026 est « annoncé le 09/09/2026, non sorti, aucun prix ni endpoint publiés ». Ça teste si ton RAG date ses connaissances.
2. **Le tag « vérifié le »** (section 159.11) est la seule parade contre les guides qui pourrissent : un prix de septembre cité en décembre sans re-vérification est un mensonge poli.
3. **Ne réindexe pas sur rumeur** : ajoute une fiche « à venir » seulement pour les ANNONCÉ, jamais pour les RUMEUR.

### 159.14. Chronologie officielle de septembre 2026 (le mois le plus dense de l'année)

Pour situer les annonces « à venir » dans leur contexte : septembre 2026 a vu quatre labs sortir un flagship en quatre jours, puis une seconde vague trois semaines plus tard.

| Date | Événement | Source de l'annonce |
|---|---|---|
| 01/09/2026 | Anthropic lance **Claude Fable 5.1** (grand public) et **Claude Mythos 5.1** (institutions vérifiées, cybersécurité/sciences de la vie) | Anthropic ; relayé par startupfortune.com et wowtale.net |
| 02/09/2026 | Meta sort **Muse Spark 1.3** | Meta ; relayé par startupfortune.com |
| 03/09/2026 | OpenAI sort **GPT-6 Astra** (« système le plus capable et le plus aligné » selon OpenAI) ; Google sort **Gemini 3.8 Flash** (+ variante cybersécurité via le programme Fairwind) | OpenAI / Google ; relayés par startupfortune.com, wowtale.net |
| 06/09/2026 | CNBC parle de « model fatigue » : les acheteurs n'arrivent plus à suivre le tableau des scores. Le PDG de Runpod (Zhen Lu) : le marché est devenu si bruyant que les entreprises doivent faire du bruit pour exister. Sam Altman : les labs « passent tous à des cadences plus rapides ». | CNBC via startupfortune.com |
| 09/09/2026 | Un membre de l'équipe DeepSeek (@tianyi) nomme **DeepSeek V4.1 Pro** comme successeur à venir (voir 159.2) | Équipe DeepSeek ; relayé par orcarouter.ai |
| 10/09/2026 | DeepSeek sort **V4.1 Flash** (nouvelle architecture Causal Encoder-Decoder, 552B MoE, entrée image native, 1M de contexte) | Changelog officiel DeepSeek (news260910) |
| 12/09/2026 | Dario Amodei (Anthropic) écrit que l'industrie de l'IA **doit ralentir** ; Sam Altman (OpenAI) et Elon Musk soutiennent cet appel | daylila.com 23/09/2026 |
| 14/09/2026 | Date prévue du reroutage `deepseek-v4-pro` → V4.1 Flash — **plan retiré** sous la pression des développeurs ; le service V4 Pro continue | Page pricing DeepSeek ; benchlm.ai |
| 18/09/2026 | Reuters : Anthropic « envisagerait » un nouveau modèle pour répondre à GPT-6 Astra (**RUMEUR**, voir 159.9). Le même jour, **quatre clients poursuivent les labs** au sujet du pacte de ralentissement | Reuters via daylila.com |
| 22/09/2026 | Anthropic sort **Claude Opus 5.5** ($4/$20/M, cache $0.20/M, +30 % de vitesse) ; OpenAI sort **GPT-6 Sol et GPT-6 Luna** (moitié prix de la série 5.6) | Anthropic / OpenAI ; relayés par EveryDev.ai, TechTarget |

**Pourquoi c'est dans ton guide infra :** un mois comme celui-là casse les hypothèses figées. Le 1er septembre, ton routeur avait un « meilleur modèle » ; le 22, il en a trois autres, avec des prix divisés par deux à qualité comparable. Sans éval continue (section 127) et sans versions épinglées (section 142), tu paies le prix fort pour le modèle d'hier.

### 159.15. Le « capability-tiered gating » : la vraie annonce structurelle de septembre 2026

Au-delà des noms de modèles, septembre 2026 a officialisé une doctrine d'accès qui change la façon de consommer les API (wowtale.net, 05/09/2026) :

| Lab | Mécanisme de cloisonnement (annoncé) |
|---|---|
| OpenAI | Capacités cybersécurité d'Astra ayant franchi le seuil « Critical » du Preparedness Framework → réservées aux entreprises vérifiées via le programme **Daybreak** (sur dossier) |
| Anthropic | **Mythos 5.1** séparé de Fable 5.1 → accès aux institutions vérifiées via programmes d'accès de confiance (cybersécurité, sciences de la vie) |
| Google | Variante cybersécurité de Gemini 3.8 Flash → programme **Fairwind** (gouvernements et opérateurs d'infrastructures de confiance) |
| Meta | Plus haut niveau de raisonnement **retenu** jusqu'à la fin des tests de sécurité supplémentaires |

**Conséquence pratique pour ton infra :** « le modèle est sorti » ne veut plus dire « tu y as accès ». Quand tu évalues un nouveau modèle, vérifie trois choses dans l'ordre : (1) l'annonce, (2) ton éligibilité au programme d'accès, (3) le prix. Un benchmark sur un modèle auquel tu n'as pas accès est du temps perdu.

### 159.16. Template de fiche RAG « à venir » (à copier tel quel)

Chaque modèle annoncé mais non sorti (ou sorti mais non vérifié par toi) mérite une fiche datée dans ton RAG. Modèle à copier :

```markdown
# FICHE MODÈLE — <nom>
- Statut : ANNONCÉ / SORTI / RUMEUR (rayer les mentions inutiles)
- Date d'annonce : <JJ/MM/AAAA> — source : <URL ou nom de l'éditeur>
- ID d'API : <id exact> ou « aucun publié au <date> »
- Prix vérifiés le : <date> — <montants> ou « aucun publié »
- Endpoint testé : oui/non — date du test : <date>
- Éligibilité : accès libre / programme <nom> / non éligible
- Expiration de la fiche : <date + 30 jours> → à recontrôler
- Décision : en prod / en éval / en attente / écarté — motif : <une ligne>
```

Exemple rempli au 27/09/2026 :

```markdown
# FICHE MODÈLE — DeepSeek V4.1 Pro
- Statut : ANNONCÉ (09/09/2026, membre de l'équipe DeepSeek @tianyi)
- ID d'API : aucun publié au 27/09/2026
- Prix vérifiés le : aucun publié au 27/09/2026
- Endpoint testé : non
- Éligibilité : inconnue
- Expiration de la fiche : 27/10/2026 → à recontrôler
- Décision : en attente — motif : aucun endpoint ni prix, ne pas coder en dur
```

### 159.17. Journal des modifications de la section 159 (à tenir à jour)

| Date de relecture | Changements constatés | Action |
|---|---|---|
| 27/09/2026 | Création de la section. V4.1 Pro annoncé non sorti ; Sonnet/Haiku 5.5 annoncés « dans les prochaines semaines » ; Opus 5.5, GPT-6 Sol/Luna, Grok 4.7 sortis. | Référence initiale |
| __/__/____ | (à compléter à chaque relecture mensuelle) | |

### 159.18. Plan d'action « lundi matin » — que faire de cette section concrètement

1. **Épingle tes modèles aujourd'hui** (section 142). Septembre 2026 a prouvé qu'un alias vendeur peut changer de modèle sous-jacent en une nuit (cas `deepseek-v4-pro` le 14/09, plan retiré in extremis). Tes configs LiteLLM/FreeLLMAPI/scripts n'utilisent que des IDs datés.
2. **Teste Opus 5.5 et GPT-6 Sol sur tes 30 questions d'éval** (section 127) : ce sont les deux sorties « prix cassés » du 22/09/2026 (Opus 5.5 : $4/$20/M contre $5/$25 ; Sol : moitié prix de la série 5.6). Si ton RAG tourne sur Opus 5 ou GPT-5.6, tu paies potentiellement 20 à 50 % trop cher pour la même qualité.
3. **Vérifie ton éligibilité aux programmes d'accès** (159.15) avant de benchmarker : Daybreak (OpenAI), trusted-access (Anthropic/Mythos), Fairwind (Google). Un score sur un modèle auquel tu n'as pas accès ne sert à rien.
4. **Crée les fiches « à venir »** (template 159.16) pour : DeepSeek V4.1 Pro, Sonnet 5.5, Haiku 5.5. Statut : en attente. Expiration : 27/10/2026.
5. **Programme la relecture mensuelle** : un cron le 1er de chaque mois qui t'affiche le journal 159.17 et te force à re-vérifier chaque ligne du tableau 159.10. Exemple :
   ```bash
   # cron mensuel — relecture section 159
   0 9 1 * * grep -A 40 '159.10. Tableau récapitulatif' ~/workspace/user/files/llm_gateways_infra.md | head -45
   ```
6. **Règle d'or** : aucun nom de cette section n'entre dans une config de prod tant que les trois cases ne sont pas cochées — annonce officielle de l'éditeur, ID d'API ou endpoint existant, prix publié (checklist 159.12).

### 159.19. Prix cités dans cette section — tableau consolidé (tous vérifiés le 27/09/2026)

Tous les montants ci-dessous viennent des recherches web du 27/09/2026 citées dans les sous-sections. Ils expirent vite : re-vérifie sur les pages pricing officielles avant toute décision budgétaire.

| Modèle | Entrée /M | Sortie /M | Cache (entrée) /M | Notes |
|---|---|---|---|---|
| Claude Opus 5.5 | $4 | $20 | $0.20 (lectures) | Baisse vs Opus 5 ($5/$25, cache $0.50) ; +30 % de vitesse annoncée |
| GPT-6 Sol | non publié dans l'annonce | non publié | — | Annoncé « moitié prix de la série 5.6 » par OpenAI (22/09/2026) |
| GPT-6 Luna | non publié dans l'annonce | non publié | — | Idem, moitié prix de la série 5.6 |
| Grok 4.7 (< 200K prompt) | $2 | $6 | $0.50 | Identique à Grok 4.6 sur cette tranche |
| Grok 4.7 (> 200K prompt) | $4 | $12 | $1 | Nouvel étage tarifaire ; contexte 500K inchangé |
| DeepSeek V4.1 Flash (hors pic) | $0.15 | $0.60 | — | Pic : $0.30/$1.20 |
| DeepSeek V4 Pro (hors pic) | $0.66 | $1.98 | — | Pic : $1.32/$3.96 ; service maintenu après le 14/09/2026 |
| Doubao Seed 2.1 Pro (Volcano) | ¥6 | ¥30 | ¥1.20 | Contexte 256K |
| Doubao Seed 2.1 Turbo (Volcano) | ¥3 | ¥15 | — | Moitié prix de Pro |
| Doubao Seed 2.1 Pro (ofox.ai, USD) | $0.884 | $4.42 | $0.177 | Agrégateur tiers, sans compte Volcano |
| Doubao Seed 2.1 Turbo (ofox.ai, USD) | $0.442 | $2.212 | $0.085 | Agrégateur tiers |
| Doubao Seed 2.0 Pro (Volcano) | ¥3.20 | ¥16 | — | Entrées ≤ 32K ; remplacé en flagship par Seed 2.1 |

Ligne de conduite : quand un nouveau modèle annoncé arrive avec un prix « moitié moins cher » (Sol, Luna, Opus 5.5), ne migre pas à l'aveugle — repasse tes 30 questions d'éval (section 127) sur l'ancien et le nouveau, compare qualité ET coût réel par tâche, puis bascule le routeur. Un prix divisé par deux avec 5 % de qualité en moins sur tes cas critiques n'est pas une économie.

### 159.20. Mini-FAQ — les questions que cette section va te poser

**Q : DeepSeek V4.1 Pro est-il disponible ?**
R : Non au 27/09/2026. Annoncé le 09/09/2026 par un membre de l'équipe DeepSeek comme successeur à venir, mais aucun endpoint, aucun prix, aucun poids publié. Le plan de transition du 14/09/2026 a été annulé.

**Q : Puis-je utiliser « GPT-6 Terra » ou « Gemini 4 » dans mon routeur ?**
R : Non. Rien d'officialisé au 27/09/2026 pour ces deux noms — ce sont des rumeurs ou des extrapolations. Les coder en dur, c'est garantir une erreur 404 le jour J.

**Q : Sonnet 5.5 / Haiku 5.5 sont-ils sortis ?**
R : Non au 27/09/2026. Anthropic les annonce « dans les prochaines semaines » (semaine du 19–25/09/2026). Fiche « en attente », à recontrôler.

**Q : Que faire si un nom de cette section sort officiellement demain ?**
R : Appliquer la checklist 159.12 (annonce éditeur + ID d'API + prix publié), créer la fiche RAG datée (template 159.16), passer les 30 questions d'éval, puis seulement envisager la prod.

**Q : Cette section sera-t-elle encore valable dans 3 mois ?**
R : Non — et c'est assumé. C'est la seule section du guide avec une date de péremption explicite : relis-la à chaque début de mois via le journal 159.17.

**Q : Où signaler une annonce que cette section aurait ratée ?**
R : Dans ton INFRA.md (section 157), rubrique « veille » : note la date, la source officielle et l'ID d'API. À la relecture mensuelle, la fiche rejoint le tableau 159.10 si les trois cases de la checklist 159.12 sont cochées.

> **Rappel de doctrine :** une annonce n'est pas une disponibilité, une disponibilité n'est pas un prix, un prix n'est pas une éligibilité. Les quatre doivent être vérifiés séparément.

*Fin du guide — 159 sections. Section 159 rédigée le 27/09/2026 : seules les annonces avec source et date y figurent ; tout le reste est « rien d'officialisé » ou « RUMEUR non confirmée ». Prix « vérifiés le 27/09/2026 » par recherche web ; le reste « à vérifier » sur les pages officielles avant toute décision budgétaire.*

## 160. Doubao (ByteDance) : le challenger chinois qui compte

**En une phrase :** l'assistant et la famille de modèles de ByteDance (TikTok), exposés via la plateforme cloud Volcano Engine — le rival direct de DeepSeek sur le segment « API pas chère et agents », avec des prix publiés en yuans et un flagship, Seed 2.1 Pro, qui vise le créneau « coding + agents long-chain ».

### 160.1. Positionnement

- Doubao = la marque IA grand public de ByteDance (app mobile gratuite, leader en Chine en 2026). Les modèles s'appellent **Seed** (Doubao Seed 2.1 Pro / Turbo au 27/09/2026).
- Distribution API : **Volcano Engine** (la branche cloud de ByteDance), runtime **Ark**. C'est l'équivalent de « DeepSeek Platform » chez ByteDance : console, endpoints, facturation au token, compatible OpenAI.
- Positionnement prix : volontairement agressif, avec une particularité utile pour ton RAG — le **cache d'entrée facturé très bas** (¥1,20/M sur Seed 2.1 Pro, soit 5x moins cher que l'entrée normale). Si tes prompts RAG réutilisent le même gros préfixe système (instructions + quelques exemples), le cache change la facture.
- Particularité administrative : un compte Volcano Engine « Chine » exige **vérification d'identité réelle** (real-name verification) et recharge en CNY. Il existe une console internationale (Volcano Engine overseas) avec carte bancaire internationale — sinon, les agrégateurs tiers (ex. ofox.ai) exposent Seed 2.1 en USD sans compte Volcano, avec une marge au passage.

### 160.2. Modèles (état au 27/09/2026 — vérifié)

Annoncés le **24/06/2026** à la conférence Volcano Engine FORCE. Deux variantes, contexte **256K** sur les deux, texte + image en entrée, texte en sortie.

| Modèle | ID d'API (Volcano) | Positionnement |
|---|---|---|
| **Doubao Seed 2.1 Pro** | `doubao-seed-2.1-pro` | Flagship « deep thinking » : code complexe, agents long-chain, multimodal. ByteDance revendique des scores de tête sur Terminal Bench 2.1, SWE-Pro, SciCode (code) et OSWorld, MobileWorld, MMMU-Pro (agents) — chiffres vendeur, à vérifier sur tes évals |
| **Doubao Seed 2.1 Turbo** | `doubao-seed-2.1-turbo` | Moitié prix de Pro : trafic entreprise haute fréquence, latence faible |

Génération précédente toujours servie : Seed 2.0 Lite/Mini, Doubao 1.5 Pro 32K, Doubao 1.5 Lite (le moins cher de la gamme). Seed 2.0 Pro a été retiré du catalogue phare le 23/06/2026, remplacé par Seed 2.1 Pro — ne le code plus en dur.

### 160.3. Prix vérifiés le 27/09/2026 (Volcano Engine, CNY/M tokens)

| Modèle | Entrée | Sortie | Cache entrée | Contexte |
|---|---|---|---|---|
| Seed 2.1 Pro | ¥6 | ¥30 | **¥1,20** | 256K |
| Seed 2.1 Turbo | ¥3 | ¥15 | ¥0,60 | 256K |
| Seed 2.0 Pro (≤ 32K) | ¥3,20 | ¥16 | — | 32K (ancienne génération) |
| Doubao 1.5 Lite | ¥0,30 | ¥0,60 | — | le moins cher |

Points d'attention :
- Doubao facture **par tranche de longueur d'entrée** (le tarif Pro couvre 0–256K ; vérifie la page pricing si tu dépasses — les longs contextes peuvent avoir un étage tarifaire).
- Tier gratuit : **500K tokens par modèle** après vérification d'identité, valables 30 jours — suffisant pour un benchmark sérieux sans sortir la carte.
- Via agrégateur tiers ofox.ai (USD, sans compte Volcano, vérifié le 27/09/2026) : Pro $0,884/$4,42/M (cache $0,177), Turbo $0,442/$2,212/M (cache $0,085). Pratique pour tester, mais **ce sont des prix de revendeur** : la référence reste la page pricing Volcano.

### 160.4. Appel API — compatible OpenAI

Volcano Ark expose un endpoint OpenAI-compatible. Schéma minimal (clé fictive) :

```python
from openai import OpenAI

client = OpenAI(
    base_url="https://ark.cn-beijing.volces.com/api/v3",  # région à adapter
    api_key="sk-EXEMPLE-FICTIF",
)
resp = client.chat.completions.create(
    model="doubao-seed-2.1-pro",   # ou "doubao-seed-2.1-turbo"
    messages=[{"role": "user", "content": "Résume ce rapport de maintenance en 5 points."}],
)
print(resp.choices[0].message.content)
```

Règles pratiques :
- Les IDs de modèle côté Ark sont des **endpoints à créer dans la console** (chaque déploiement = un endpoint ID). En production, épingle l'endpoint ID, pas le nom marketing.
- Latence Chine → ton infra : si ton RAG tourne hors de Chine, mesure le TTFB réel avant de router du trafic synchrone dessus — un prix bas avec 800 ms de latence réseau ne convient pas à un chat interactif.

### 160.5. Quand l'utiliser dans ta stack

1. **Volume + budget** : si ton RAG génère beaucoup de réponses longues en français technique, Turbo à ¥3/¥15 vaut un benchmark contre DeepSeek V4.1 Flash et GPT-6 Sol sur tes 30 questions d'éval (section 127).
2. **Agents** : Seed 2.1 Pro vise explicitement les agents multi-étapes — à tester là où tu utiliserais Claude ou GPT-6 pour de l'orchestration.
3. **Cache agressif** : ton prompt système RAG est stable → active le cache d'entrée, c'est le poste où Doubao est le plus compétitif.
4. **À ne pas faire** : traiter les benchmarks vendeur (vs Claude Opus 4.6) comme une vérité ; ignorer la contrainte real-name verification si tu dois facturer à une entité hors Chine ; oublier que les prix sont en CNY (taux ~6,79 CNY/USD au 27/09/2026 — recalcule à chaque devis).

### 160.6. Embeddings et modèles annexes : ne pas oublier la brique vectorielle

Ton RAG vit ou meurt par l'embedding (tu es sur text-embedding-3-small). Doubao propose ses propres modèles d'embedding sur Volcano Ark (`doubao-embedding` — plusieurs générations, dont des versions « large » et des modèles vision). Deux usages :
- **Benchmark d'embedding** : si tu testes Doubao comme LLM de génération, teste aussi son embedding sur ton corpus FR technique — un embedding entraîné avec du chinois/anglais peut se comporter différemment sur ton français industriel. Mesure avec tes métriques (section 141 : recall@k, MRR), pas au feeling.
- **Reranker** : Volcano propose aussi des rerankers — l'étage « rerank » après la récupération vectorielle est souvent le gain qualité/coût le plus rentable d'un RAG (quelques centimes pour +10 points de précision).

Règle : ne mélange pas les embeddings entre index et requêtes — tout le corpus doit être réindexé si tu changes de modèle d'embedding. C'est l'opération la plus chère d'un changement de stack RAG : compte tes tokens d'indexation AVANT (section 167.5).

### 160.7. Doubao vs DeepSeek vs GPT-6 Sol : comment arbitrer (sept 2026)

| Critère | Doubao Seed 2.1 Pro | DeepSeek V4 Pro | GPT-6 Sol |
|---|---|---|---|
| Prix entrée /M | ¥6 (~$0,88) | ¥0,66 (~$0,10) hors pic | « moitié prix série 5.6 », non publié |
| Prix sortie /M | ¥30 (~$4,42) | ¥1,98 hors pic | non publié |
| Cache entrée | ¥1,20 (5x moins cher) | — | à vérifier |
| Contexte | 256K | 256K (V4 Pro) | à vérifier |
| Positionnement | Agents long-chain, code | Raisonnement brut, prix cassé | Écosystème OpenAI, prix agressif |
| Accès | Volcano (real-name) / agrégateurs | API directe simple | API OpenAI directe |
| Français technique | À benchmarker | À benchmarker | À benchmarker |

Lecture : DeepSeek reste imbattable au prix ; Doubao se place entre DeepSeek et les occidentaux avec un argument « agents + cache » ; Sol est le joker si son prix publié confirme la promesse. **Aucun ne gagne sans passer tes 30 questions d'éval en français** — le prix ne dit rien de la qualité sur tes procédures d'onduleurs.

### 160.8. Pièges opérationnels avant la prod

1. **Facturation CNY** : ta compta veut de l'EUR/USD — prévois la conversion et la TVA applicable aux services cloud chinois ; un agrégateur USD simplifie la facture mais ajoute sa marge.
2. **Souveraineté des données** : prompts envoyés sur des serveurs en Chine (région `cn-beijing` par défaut). Pour des docs d'exploitation internes, valide avec ta politique de sécurité — sinon, réserve Doubao aux usages non sensibles.
3. **Endpoint ≠ modèle** : côté Ark, tu déploies un modèle sur un **endpoint** (avec son propre ID et son propre débit alloué). En pic, c'est le débit de TON endpoint qui limite, pas le modèle — dimensionne-le comme tu dimensionnerais une VM.
4. **Dérive des IDs** : ByteDance a retiré Seed 2.0 Pro du flagship en 3 mois (juin 2026). Épingle des IDs datés et garde la section 159 à jour — un alias qui change de modèle sous-jacent, tu as déjà vu le cas DeepSeek le 14/09.

### 160.9. Intégrer Doubao dans ton routeur LiteLLM

```yaml
# litellm config — Doubao via Ark (endpoint OpenAI-compatible)
model_list:
  - model_name: doubao-seed-2-1-pro
    litellm_params:
      model: openai/doubao-seed-2-1-pro
      api_base: https://ark.cn-beijing.volces.com/api/v3
      api_key: os.environ/VOLC_ARK_API_KEY   # clé Ark fictive en exemple
  - model_name: doubao-seed-2-1-turbo
    litellm_params:
      model: openai/doubao-seed-2-1-turbo
      api_base: https://ark.cn-beijing.volces.com/api/v3
      api_key: os.environ/VOLC_ARK_API_KEY

router_settings:
  routing_strategy: cost-based  # Turbo pour le volume, Pro pour les agents
```

Stratégie de routage suggérée : questions RAG simples → Turbo ; tâches agentiques multi-étapes ou code → Pro ; et un fallback `doubao-seed-2-1-pro → deepseek-v4-pro → gpt-6-sol` pour la résilience. Mesure le TTFB réel depuis ton infra avant de mettre Doubao sur un chemin synchrone exposé aux utilisateurs — la latence réseau Chine→Europe peut annuler l'avantage prix sur un chat interactif.

### 160.10. Veille prix : le rituel mensuel (car ça bouge vite)

Doubao a augmenté ses prix flagship entre Seed 2.0 et 2.1 (¥3,20 → ¥6 l'entrée) tout en divisant par deux via Turbo — la gamme se repositionne chaque semestre. Rituel à caler sur ta relecture mensuelle (section 159.17) :

1. Ouvre la page pricing Volcano (docs.volcengine.com/docs/82379/1544106) — note les prix Pro/Turbo/cache.
2. Compare avec ton coût réel du mois (tes logs LiteLLM : tokens × prix).
3. Si l'écart avec DeepSeek/GPT-6 Sol dépasse 30 % sur ton mix réel entrée/sortie, relance un benchmark d'éval.
4. Mets à jour le tableau 160.3 avec la date — un prix sans date dans ce guide est un prix périmé.

---

## 161. LM Studio : l'alternative desktop à Ollama (avec GUI)

**En une phrase :** le même moteur que tout le monde (llama.cpp) mais avec une vraie application desktop : navigateur de modèles Hugging Face intégré, historique de chat persistant, comparaisons côte à côte, serveur OpenAI-compatible en un clic — gratuit, propriétaire, y compris pour usage commercial.

### 161.1. Ce que c'est (état : v0.4.16, juin 2026)

- App native **Windows 10+, macOS (Apple Silicon et Intel), Linux (AppImage)**. Sous le capot : llama.cpp, accélération GPU (NVIDIA CUDA, Apple Metal/MLX, AMD Vulkan/ROCm).
- Installation : téléchargeur sur lmstudio.ai → installe → l'onboarding te propose un modèle recommandé. **Temps jusqu'à la première inférence : ~10–15 min** (install + téléchargement d'un modèle 8B).
- Licence : propriétaire, **gratuite pour tout usage y compris commercial** (l'obligation de licence business a été supprimée en 2025). Ce n'est pas open source — à noter si ta politique interne l'exige.
- Nouveautés 2026 notables : **v0.4.14** — décodage spéculatif MTP (Multi-Token Prediction) ; **v0.4.15** — parallélisme tensoriel multi-GPU en un clic ; **v0.4.16 (04/06/2026)** — app iPhone/iPad **Locally** + **LM Link** : ton téléphone devient un terminal chiffré (mesh Tailscale, E2E) vers les modèles de ton Mac — rien ne transite par un relais cloud, seul un annuaire de découverte touche les serveurs LM Studio. Lancement iPhone/iPad uniquement.
- Panneau **Mission Control** (2026) : gestion headless — les modèles se chargent à la demande sans UI, utile pour un poste de dev qui sert aussi de backend local.

### 161.2. Installation et usage

```text
1. Télécharge depuis lmstudio.ai (version du site officiel uniquement)
2. Installe (Windows : Setup.exe / macOS : glisser dans Applications / Linux : AppImage)
3. Onglet Search : cherche un modèle (ex. "Qwen3"), filtre par ton matériel —
   LM Studio détecte ta RAM/VRAM et indique quelles quantifications passent
4. Clique Download (un 8B en Q4 ≈ 5 Go, compte ~10 min)
5. Onglet Chat : sélectionne le modèle dans la liste, discute
```

**Mode serveur** (le pont vers tes outils) : onglet *Local Server* → *Start* → API OpenAI-compatible sur `http://localhost:1234`. N'importe quel outil qui accepte `OPENAI_BASE_URL` s'y branche :

```bash
# Exemple : pointer un client OpenAI vers LM Studio (clé fictive, valeur ignorée en local)
export OPENAI_BASE_URL="http://localhost:1234/v1"
export OPENAI_API_KEY="lm-studio"
```

Cas d'usage concrets : comparer plusieurs quantifications d'un même modèle (Q4 vs Q5 vs Q8) pour ton matériel ; tester un modèle vision (LLaVA, Qwen-VL) en glissant une image dans le chat ; servir de backend local à Continue.dev / Open WebUI sans toucher au terminal.

### 161.3. LM Studio vs Ollama — tableau comparatif (vérifié sept 2026)

| Critère | LM Studio 0.4.16 | Ollama 0.24.0 |
|---|---|---|
| Interface | GUI native complète (chat, sliders température/top-p, comparaisons) | CLI (`ollama run`) ; GUI via tiers (Open WebUI) |
| Historique de chat | Oui, persistant | Non en natif |
| Découverte de modèles | Navigateur Hugging Face intégré + indicateur compatibilité matérielle | `ollama pull` + catalogue ollama.com (~100+ modèles) |
| Formats | GGUF (tout Hugging Face) | GGUF via registre Ollama (curé) |
| API REST | OpenAI-compatible, port **1234** (l'app doit tourner) | OpenAI-compatible, port **11434** (daemon système) |
| Headless / serveur | Non — desktop requis (Mission Control = semi-headless) | Oui — daemon, idéal serveurs |
| Multi-GPU | Parallélisme tensoriel en 1 clic (0.4.15) | Oui, configurable |
| Mobile | Locally (iOS) + LM Link via Tailscale | Non (tiers uniquement) |
| macOS Apple Silicon | Metal + MLX | Metal + MLX (depuis 0.24.0, plus rapide) |
| Mises à jour | Auto in-app | Auto en arrière-plan |
| Licence | Propriétaire, gratuit (y c. commercial) | MIT (open source) |
| Temps 1re inférence | ~12–15 min | ~5 min |

### 161.4. Doctrine d'usage pour ton labo

- **Les deux cohabitent** : ports différents (11434 vs 1234), aucun conflit. Ollama = backend API/headless de référence ; LM Studio = l'outil d'évaluation interactive (« ce nouveau modèle vaut-il quelque chose ? » en 5 minutes, avec historique et comparaison côte à côte).
- Pour Zelef : LM Studio est ce que tu donnes à un collègue non-technique qui veut « essayer un modèle local » sans installer Python ni apprendre ce que veut dire GGUF. Ollama reste ton choix pour tout ce qui est scripté, conteneurisé ou servi (RAG local, Vast.ai, RunPod).
- Limite honnête : LM Studio ne remplace ni vLLM ni SGLang (section 163) pour servir en production — c'est un outil desktop, pas un serveur d'inférence à haut débit.

### 161.5. Quelle quantification pour ton matériel : le guide express

Le navigateur de modèles de LM Studio affiche un indicateur de compatibilité, mais voici la logique derrière — à connaître pour ne pas télécharger 40 Go pour rien :

| Ta VRAM/RAM | Quantification à viser (modèle 7B–8B) | Taille fichier | Qualité |
|---|---|---|---|
| 8 Go VRAM | Q4_K_M | ~5 Go | Très bonne (défaut recommandé) |
| 12–16 Go VRAM | Q5_K_M ou Q6_K | ~6–7 Go | Proche du FP16 |
| 24 Go VRAM | Q8_0, voire 2 modèles Q4 en parallèle | ~8–9 Go | Quasi-identique au modèle d'origine |
| CPU uniquement (16 Go RAM) | Q4_K_M, modèles ≤ 8B | ~5 Go | Lente (~5–15 tok/s) mais utilisable |
| Apple Silicon unifié (24 Go+) | Q4/Q5, modèles 8B–32B selon RAM | variable | Excellente (bande passante unifiée) |

Règles : **Q4_K_M = le défaut raisonnable** dans 90 % des cas ; au-delà de Q6_K, les gains qualité sont marginaux pour un usage chat/RAG ; un 70B en Q4 demande ~40 Go — c'est du multi-GPU ou du Mac Studio, pas un laptop. LM Studio te dit « passe / limite / ne passe pas » AVANT le téléchargement : fais-lui confiance.

### 161.6. LM Link + Locally : le détail qui change tout (v0.4.16)

Le montage exact, vérifié juin 2026 :
1. Ton **Mac** (moteur de calcul — Apple Silicon recommandé) fait tourner LM Studio avec le modèle chargé.
2. **LM Link** établit un lien via un mesh **Tailscale** chiffré de bout en bout entre ton Mac et ton iPhone/iPad.
3. L'app **Locally** (iOS/iPadOS uniquement au lancement) devient un terminal : tu chattes depuis ton téléphone, l'inférence tourne sur le Mac.

Ce qui ne transite JAMAIS par le cloud : tes prompts, tes réponses, tes modèles. Ce qui touche les serveurs LM Studio : uniquement la liste de découverte des appareils (pour que le téléphone trouve le Mac). Pour un chef de service qui veut interroger ses docs depuis le terrain sans exposer de données : c'est l'architecture « private inference » la plus simple du marché — à condition d'avoir le Mac allumé au bureau.

### 161.7. Le CLI `lms` : LM Studio sans la souris

LM Studio embarque un CLI pour les allergiques à la GUI (utile pour scripter) :

```bash
# Lister / télécharger / charger un modèle
lms ls
lms get qwen3-8b              # télécharge (équivalent du clic dans Search)
lms load qwen3-8b --gpu 1.0   # charge en VRAM (gpu 0..1 = fraction offloadée)

# Démarrer le serveur OpenAI-compatible (port 1234 par défaut)
lms server start
lms server stop

# Chat direct au terminal
lms chat qwen3-8b
```

Cas d'usage : un script qui charge le modèle le matin (`lms load`), démarre le serveur, et l'éteint le soir — ton poste devient un mini-backend local sans intervention.

### 161.8. LM Studio dans la chaîne « modèle local » complète

```
Hugging Face (poids GGUF)
   → LM Studio : découverte, test interactif, comparaison de quants
   → export/décision : « ce modèle vaut le coup en prod ? »
   → Ollama (serveur léger) ou vLLM/SGLang (serveur lourd) ou NIM (clé en main)
   → ton routeur LiteLLM
```

LM Studio n'est pas le maillon de prod — c'est le **maillon d'évaluation**. Sa valeur = réduire à 10 minutes la question « ce nouveau modèle open-weight mérite-t-il une place dans ma stack ? », avec historique et comparaisons à l'appui au lieu d'impressions.

### 161.9. Modèles recommandés 2026 pour ton usage (à tester dans LM Studio)

| Usage | Modèle GGUF à chercher | Pourquoi |
|---|---|---|
| Chat technique FR généraliste | Qwen3-8B / 14B instruct (Q4_K_M) | Bon FR, tool calling correct, rapide |
| Raisonnement / debug | Qwen3-32B (si 24 Go+ VRAM) ou DeepSeek-R1-Distill 8B | Le distill R1 donne du raisonnement visible pas cher |
| RAG local : embeddings | nomic-embed-text / bge-m3 (GGUF) | Pour tester le « tout local » avant de décider |
| Vision (lire une plaque onduleur) | Qwen2.5-VL / Qwen3-VL (GGUF) | Glisser-déposer d'image dans le chat LM Studio |
| Code sysadmin | Qwen3-Coder / Devstral | Scripts bash, Ansible, debug |

Protocole d'évaluation express (30 min) : charge 2 candidats → même 5 questions métier FR → compare côte à côte (fonction native) → note qualité/vitesse/VRAM → le gagnant part en test sur Ollama/vLLM. Ce rituel t'évite d'ajouter un modèle à ta stack sur un simple effet de mode Hugging Face.

---

## 162. Unsloth : le fine-tuning efficace (2x plus rapide, 2x moins de VRAM)

**En une phrase :** une surcouche open source (Apache 2.0) à Hugging Face TRL qui rend le fine-tuning LoRA/QLoRA accessible sur une seule GPU consommatrice — grâce à des kernels Triton écrits à la main et un moteur de backprop optimisé, sans perte de précision revendiquée.

### 162.1. Principe

Le fine-tuning classique d'un LLM est gourmand : un 7B en 16 bits demande ~60 Go de VRAM pour l'entraînement complet (poids + gradients + états d'optimiseur). Unsloth attaque le problème sur deux axes :

1. **LoRA / QLoRA** : au lieu de réentraîner tous les poids, on gèle le modèle et on n'entraîne que de petites matrices d'adaptation (rank `r` = 16 typiquement, soit < 1 % des paramètres). En QLoRA, le modèle de base est en plus quantifié en 4 bits → un 7B tient dans ~5 Go + les adaptateurs.
2. **Kernels Triton manuels** : Unsloth remplace les opérations coûteuses (attention, backward des couches LoRA, RoPE, cross-entropy) par des kernels écrits à la main, avec un moteur de backpropagation manuel. Résultat revendiqué : **~2x plus rapide, ~70 % de VRAM en moins, 0 % d'erreur d'approximation** (pas de raccourci mathématique, juste du code mieux optimisé).

En pratique : fine-tuner un 7B–8B instruct sur tes données métier (tickets de maintenance, procédures onduleurs, docs Huawei) devient faisable sur une **RTX 4090 24 Go**, voire une 3090 — là où le fine-tuning naïf exigerait du datacenter.

### 162.2. Installation (vérifiée sept 2026)

```bash
# Méthode recommandée (Linux ou WSL2 — pas de support Windows natif)
pip install unsloth

# Avec vLLM (utile pour le GRPO : l'inférence rapide des rollouts)
pip install uv && uv pip install unsloth vllm

# Docker : tout pré-installé, avec Jupyter
docker run -d --gpus all -p 8888:8888 \
  -e JUPYTER_PASSWORD="mot-de-passe-fictif" \
  -v $(pwd)/work:/workspace/work \
  unsloth/unsloth
```

Prérequis : **GPU NVIDIA, capacité CUDA 7.0+** (T4, V100, RTX 20/30/40/50, A100, H100, L40), Python 3.10–3.13. Support AMD/Intel en cours (partiel — vérifie la doc pour ta carte). Sur Colab/Kaggle : fonctionne, c'est d'ailleurs le chemin le plus courant pour un premier essai gratuit (T4 gratuit = QLoRA 7B possible, lentement).

Piège classique : Unsloth gère l'attention en interne — **n'installe pas `flash-attn` manuellement** avec Unsloth, il utilise SDPA en interne sur les modèles récents et ça entre en conflit.

### 162.3. Cas d'usage pour ton RAG (et ton métier)

1. **Adaptation au domaine** : SFT (supervised fine-tuning) sur tes paires question/réponse métier — ex. 500–2000 Q/R sur les onduleurs, les CLI Huawei, les procédures de maintenance. Le modèle apprend ton vocabulaire, ton format de réponse, tes gammes de produits.
2. **Format de sortie contraint** : entraîner le modèle à toujours répondre en JSON structuré, ou avec les citations au format que ton RAG attend.
3. **GRPO / DPO** : au-delà du SFT, Unsloth supporte le reinforcement learning (GRPO, DPO, GSPO) — utile quand tu as une **fonction de récompense** plutôt que des exemples parfaits (ex. : récompenser les réponses qui citent la bonne section de doc).
4. **Export universel** : après entraînement, exporte en **GGUF** (Ollama, LM Studio — section 161), fusionne en 16 bits, ou sers via **vLLM / SGLang** (section 163). Le LoRA reste compatible avec l'écosystème transformers.

Ordres de grandeur (7B–8B, QLoRA r=16, séquences 2048) : ~8–14 Go de VRAM, quelques heures sur une 4090 pour 1–3 époques sur quelques milliers d'exemples. C'est du « fine-tuning du dimanche », pas du pré-entraînement.

### 162.4. Exemple minimal (SFT, correct et testable)

```python
# pip install unsloth  (Linux/WSL2 + GPU NVIDIA)
from unsloth import FastLanguageModel
from trl import SFTTrainer, SFTConfig
from datasets import Dataset

# 1. Jeu de données jouet au format instruction (remplace par tes vraies données)
data = Dataset.from_list([
    {"instruction": "Quelle est la tension de floating d'un bloc VRLA 12 V ?",
     "output": "13,5 à 13,6 V par bloc à 20 °C, avec compensation -3 mV/°C/cellule."},
    {"instruction": "Que signifie le code C6000 sur un copieur Kyocera ?",
     "output": "Rupture de chauffe (fuser) : vérifier thermistance, thermostat et lampe."},
])

def to_chatml(ex):
    return {"text":
        f"<|im_start|>user\n{ex['instruction']}<|im_end|>\n"
        f"<|im_start|>assistant\n{ex['output']}<|im_end|>"}
data = data.map(to_chatml)

# 2. Chargement du modèle en 4 bits
model, tokenizer = FastLanguageModel.from_pretrained(
    model_name="unsloth/Qwen3-8B",   # version pré-patchée par Unsloth ; adapte à ton modèle
    max_seq_length=2048,
    dtype=None,            # auto : float16 (T4/V100) ou bfloat16 (Ampere+)
    load_in_4bit=True,     # QLoRA : ~5 Go pour un 8B
)

# 3. Greffe des adaptateurs LoRA
model = FastLanguageModel.get_peft_model(
    model,
    r=16,                  # rank : 8/16/32 ; 16 = bon défaut
    lora_alpha=16,
    target_modules=["q_proj", "k_proj", "v_proj", "o_proj",
                    "gate_proj", "up_proj", "down_proj"],
    lora_dropout=0,        # 0 = optimisé dans Unsloth
    bias="none",
    use_gradient_checkpointing=True,  # "unsloth" : économise la VRAM au prix d'un peu de vitesse
    random_state=3407,
)

# 4. Entraînement
trainer = SFTTrainer(
    model=model,
    train_dataset=data,
    processing_class=tokenizer,
    args=SFTConfig(
        output_dir="./output",
        per_device_train_batch_size=2,
        gradient_accumulation_steps=4,
        num_train_epochs=3,
        learning_rate=2e-4,
        packing=True,      # 2 à 5x plus rapide sur séquences de longueurs mixtes
    ),
)
trainer.train()

# 5. Export GGUF pour Ollama / LM Studio
model.save_pretrained_gguf("mon-modele-metier", tokenizer, quantization_method="q4_k_m")
```

### 162.5. Limites honnêtes

- **Le fine-tuning ne remplace pas le RAG** : il apprend un style et un domaine, il n'apprend pas *tes* documents. Pour des faits qui changent (prix, versions firmware), le RAG reste supérieur — le modèle fine-tuné « sait » ce qu'il a vu à l'entraînement et hallucine le reste avec assurance.
- **Qualité des données > quantité** : 500 paires propres valent mieux que 10 000 paires bruitées. Prévois du temps de curation (c'est 80 % du travail).
- **Unsloth OSS = DDP uniquement** : une réplique complète par GPU, pas de FSDP/ZeRO — le modèle doit tenir sur **une seule GPU**. Pour du multi-GPU sérieux, c'est DeepSpeed/FSDP manuel ou un framework pro.
- **Évalue avant/après** : repasse tes 30 questions d'éval (section 127) sur le modèle de base puis fine-tuné. Un fine-tuning raté dégrade le modèle (catastrophic forgetting sur les capacités générales).

### 162.6. GRPO : le fine-tuning par récompense (exemple minimal)

Le SFT apprend des exemples parfaits. Le GRPO apprend d'une **fonction de récompense** — idéal quand tu sais dire « c'est bien / c'est mal » mais que tu n'as pas 2000 exemples parfaits. Exemple : récompenser les réponses qui citent une référence documentaire.

```python
from unsloth import FastLanguageModel
from trl import GRPOConfig, GRPOTrainer
import re

model, tokenizer = FastLanguageModel.from_pretrained(
    model_name="unsloth/Qwen3-8B",
    max_seq_length=2048,
    load_in_4bit=True,
)
model = FastLanguageModel.get_peft_model(model, r=16)

def reward_citation(completions, **kwargs):
    """+2 si la réponse cite une référence type [DOC-xxx] ou §x.y, 0 sinon."""
    scores = []
    for c in completions:
        text = c[0]["content"] if isinstance(c, list) else str(c)
        has_ref = bool(re.search(r"\[DOC-[\w-]+\]|§\s?\d+\.\d+|U\d{3,4}|C\d{4}", text))
        scores.append(2.0 if has_ref else 0.0)
    return scores

trainer = GRPOTrainer(
    model=model,
    processing_class=tokenizer,
    args=GRPOConfig(
        output_dir="./grpo-out",
        num_generations=4,     # 4 réponses candidates par prompt
        beta=0.04,             # garde-fou : ne pas trop s'éloigner du modèle de base
        learning_rate=5e-6,    # LR très faible : le RL est instable par nature
        max_prompt_length=1024,
        max_completion_length=512,
    ),
    train_dataset=dataset_prompts,   # juste des prompts, PAS de réponses attendues
    reward_funcs=[reward_citation],
)
trainer.train()
```

Notes : le GRPO consomme plus de VRAM que le SFT (plusieurs générations en parallèle) — prévois large ou réduis `num_generations` à 2 sur 24 Go. Et la fonction de récompense, c'est 80 % du résultat : une récompense mal conçue produit un modèle qui « game » la métrique (ex. : il cite des références inventées pour toucher le bonus — d'où le regex strict sur des formats vérifiables).

### 162.7. VRAM : combien pour quel modèle (QLoRA r=16, seq 2048 — ordres de grandeur)

| Modèle | Poids 4 bits | Entraînement (SFT) | Recommandation GPU |
|---|---|---|---|
| 1B–3B | ~2 Go | ~6–8 Go | RTX 3060 12 Go, T4 Colab |
| 7B–8B | ~5 Go | ~10–14 Go | RTX 3090/4090 24 Go |
| 13B–14B | ~8 Go | ~18–22 Go | 4090 24 Go (limite), A100 40 Go confort |
| 30B–32B | ~18 Go | ~40–48 Go | A100 80 Go, 2×4090 (non — DDP = 1 réplique/GPU, donc 1× GPU ≥ 48 Go) |
| 70B | ~40 Go | ~80 Go+ | H100 80 Go (limite) |

Rappel : Unsloth OSS = **une réplique complète par GPU** (DDP, pas de sharding FSDP/ZeRO) — le modèle + l'entraînement doivent tenir sur **UNE** carte. Si ça ne passe pas : réduis `max_seq_length`, `r`, le batch, ou passe au FP8 (RTX 40+/H100 : -60 % VRAM, 1,4x plus vite selon Unsloth).

### 162.8. Workflow complet : du dataset au modèle servi

```
1. Curation : 500–2000 paires Q/R métier au format ChatML/ShareGPT/Alpaca
   (tickets résolus, procédures validées — PAS du scraping brut)
2. Split : 90 % train / 10 % éval (tes 30 questions d'éval, section 127)
3. SFT Unsloth : 1–3 époques, LR 2e-4, r=16, packing=True
4. Mesure : éval avant/après — si pas de gain net, jette l'adaptateur
5. (Optionnel) GRPO : affine le comportement avec une fonction de récompense
6. Export : GGUF q4_k_m → Ollama / LM Studio (test humain)
7. Prod : LoRA mergé en 16 bits → vLLM/SGLang avec --enable-lora (hot-swap d'adaptateurs)
```

Le hot-swap LoRA de vLLM/SGLang mérite d'être connu : un seul modèle de base servi, plusieurs adaptateurs métier commutables par requête — ton « modèle onduleurs » et ton « modèle copieurs » sur la même GPU, sans recharger les poids.

---

## 163. SGLang : le serveur d'inférence qui monte (état sept 2026)

**En une phrase :** le challenger open source de vLLM pour servir des LLM en production — son arme : **RadixAttention** (cache de préfixes partagés entre requêtes) et un support day-one agressif des gros MoE (DeepSeek, Qwen, Kimi), avec des gains massifs sur le trafic à préfixes répétés… et presque aucun sur le trafic à prompts uniques.

### 163.1. Présentation

- Projet open source (Université de Berkeley / LMSYS à l'origine), licence Apache 2.0, comme vLLM.
- Architecture : **RadixAttention** — un arbre radix des KV caches qui réutilise automatiquement les préfixes communs entre requêtes (prompt système, few-shot, historique multi-tours, documents RAG réinjectés). Là où vLLM recalcule le préfixe à chaque requête (sauf cache explicite), SGLang le partage.
- **Langage de frontend structuré** : SGLang propose aussi un DSL pour la génération contrainte (JSON schema, choix multiples, raisonnement multi-étapes avec `gen`, `select`) — utile pour des agents qui doivent produire du structuré fiable.
- Installation typique :

```bash
pip install sglang
# Serveur OpenAI-compatible
python -m sglang.launch_server --model-path Qwen/Qwen3-8B --port 30000
# curl http://localhost:30000/v1/chat/completions  (même format qu'OpenAI)
```

### 163.2. État au 27/09/2026 (vérifié)

- Ligne de version : **SGLang 0.5.x** (été 2026). **vLLM 0.24.0** (août 2026) — attention au breaking change : vLLM ne définit plus `CUDA_VISIBLE_DEVICES` en interne, un nouvel argument `device_ids` le remplace ; les déploiements qui comptaient sur l'ancien comportement silencieux changent de binding GPU après upgrade.
- **MoE** : SGLang travaille en direct avec DeepSeek/Qwen/Kimi/Z AI sur des implémentations optimisées — sur DeepSeek V3, SGLang revendique **3,1x plus vite que vLLM** ; sur GB200 NVL72, **25x** (février 2026, chiffres vendeur/écosystème).
- **TPU** : backend SGLang-Jax natif — SGLang tourne sur TPU Google, pas seulement NVIDIA.
- **Disaggregation prefill/decode** : devenue une fonctionnalité de production (pas expérimentale) : jusqu'à 6,4x de débit et 20x moins de variance de latence sur les déploiements qui la configurent — vLLM et SGLang la proposent tous deux.
- Point de vigilance : les benchmarks « SGLang écrase vLLM » sont **dépendants du trafic**. Mesures indépendantes 2026 :
  - Trafic à préfixes partagés (agents, multi-tours, RAG) : SGLang ~+29 % de débit vs vLLM sur H100 (classe Llama-3-8B, début 2026) ; multi-tours jusqu'à ~1,75x dans certains setups.
  - **Trafic à prompts uniques** (génération créative one-shot, requêtes de recherche indépendantes) : l'avantage s'effondre à **~1–4 %** (bruit de mesure). RadixAttention n'a rien à réutiliser quand rien n'est partagé.
  - Sur petite GPU consommateur (8 Go, Qwen3-0.6B) : vLLM jusqu'à 2,35x plus rapide *out of the box* dans un test — l'écart venait d'une optimisation GPU que vLLM tenait en mémoire et pas SGLang. Moralité : **benchmarke sur TON matériel et TON trafic**, pas sur des slides.

### 163.3. vLLM vs SGLang — tableau comparatif (sept 2026)

| Critère | vLLM 0.24.0 | SGLang 0.5.x |
|---|---|---|
| Licence | Apache 2.0 | Apache 2.0 |
| Idée centrale | PagedAttention (gestion mémoire KV efficace) | RadixAttention (partage des préfixes entre requêtes) |
| Point fort | Le plus simple à adopter, **support modèles/matériel le plus large** (NVIDIA, AMD, Intel, TPU) | **MoE et gros déploiements multi-nœuds** ; trafic à préfixes répétés |
| API | OpenAI-compatible (`/v1/chat/completions`), port configurable | OpenAI-compatible (port 30000 par défaut) + DSL structuré natif |
| Tool calling | Via `--tool-call-parser` | Natif |
| Génération structurée | Oui (guidance/xgrammar selon version) | Oui, DSL intégré (`gen`, `select`, JSON) |
| Multimodal (« Omni ») | Large support | En retrait sur certains modèles vision |
| Maturité écosystème | La référence : intégrations partout (TGI-like, LiteLLM, etc.) | En forte croissance, support day-one des MoE chinois |
| Cas d'usage idéal | Serveur généraliste « ça marche avec (presque) tout » | Agents, RAG à gros prompts système partagés, serving de MoE |
| Piège 2026 | 0.24 : `device_ids` remplace le `CUDA_VISIBLE_DEVICES` implicite | Avantage nul sur prompts uniques ; perfs sensibles au tuning |

### 163.4. Que choisir pour ton RAG ?

- **Règle simple** : si ton trafic a un **gros préfixe partagé** (prompt système RAG + instructions + few-shot identiques à chaque requête), SGLang mérite un benchmark — c'est exactement son terrain de jeu, et ton RAG est un cas d'école (même system prompt, documents variables).
- Si tu sers des modèles variés, du multimodal, ou si tu veux le chemin le plus documenté : **vLLM** reste le choix par défaut.
- Les deux exposent une API OpenAI-compatible → ton routeur LiteLLM/FreeLLMAPI ne voit pas la différence : tu peux **A/B tester en changeant une URL**, mesurer tok/s et TTFT sur ton trafic réel, et décider avec des chiffres.
- Alternative « je ne veux rien tuner » : **NVIDIA NIM** (section 165) choisit le moteur (vLLM, TensorRT-LLM ou SGLang selon le profil) à ta place.

### 163.5. Génération structurée : le DSL SGLang en 10 lignes

Là où vLLM s'appuie sur des libs externes (xgrammar, guidance), SGLang intègre un mini-langage pour forcer la structure des sorties — décisif quand ton RAG doit produire du JSON valide à tous les coups :

```python
import sglang as sgl

@sgl.function
def diagnostic(s, symptome):
    s += sgl.user(f"Symptôme onduleur : {symptome}. Réponds en JSON strict.")
    s += sgl.assistant(sgl.gen("reponse",
                               max_tokens=256,
                               regex=r'\{"cause": ".+", "gravite": "(critique|majeure|mineure)", "action": ".+"\}'))
    # le regex est CONTRAINT pendant la génération : la sortie matche toujours

# Exécution contre ton serveur SGLang local
state = diagnostic.run(symptome="bypass statique en défaut, charge sur réseau",
                       backend=sgl.Runtime("http://localhost:30000"))
print(state["reponse"])  # JSON garanti valide, ex: {"cause": "...", "gravite": "majeure", "action": "..."}
```

Autres primitives : `sgl.select` (choix parmi une liste fermée — classification sans risque de sortie libre), `sgl.gen` avec `choices`, chaînage multi-étapes avec variables. Pour un RAG qui alimente des tickets GLPI ou des rapports : la génération contrainte élimine toute une classe de bugs de parsing en aval.

### 163.6. Prefill/decode disaggregation : l'idée en 30 secondes

L'inférence a deux phases aux besoins opposés : le **prefill** (lire tout le prompt d'un coup — gourmand en calcul) et le **decode** (générer token par token — gourmand en mémoire/bande passante). Les servir sur les mêmes GPU, c'est faire cohabiter deux workloads qui se gênent.

La disaggregation = des GPU dédiées au prefill, d'autres au decode, avec transfert du KV cache entre les deux. Gains mesurés (2026) : jusqu'à **6,4x de débit** et **20x moins de variance de latence** sur les bons setups. Le prix : de la complexité réseau (transfert KV cache rapide exigé — NVLink/InfiniBand, pas du 1 GbE) et un dimensionnement à deux pools. **Pour ton usage** : pertinent si tu sers un RAG à fort trafic avec de gros prompts (chunks longs) ; overkill pour un prototype ou un usage interne modéré. vLLM et SGLang la proposent tous deux en 2026 — ce n'est plus un argument différenciant, c'est une option d'archi.

### 163.7. Mise en route Docker et benchmark minimal

```bash
# Serveur SGLang via Docker (adapte le tag à la version du moment)
docker run -d --gpus all \
  -p 30000:30000 \
  -v ~/.cache/huggingface:/root/.cache/huggingface \
  --name sglang \
  lmsysorg/sglang:latest \
  python -m sglang.launch_server \
    --model-path Qwen/Qwen3-8B \
    --port 30000 \
    --enable-prefix-caching   # RadixAttention : le flag qui fait la différence

# Attendre la dispo
curl http://localhost:30000/v1/models

# Benchmark de TON trafic : le script officiel, avec TES prompts
python -m sglang.bench_serving \
  --backend sglang \
  --dataset-name random \
  --random-input-len 3000 --random-output-len 300 \
  --num-prompts 100 --max-concurrency 32
# Refais pareil côté vLLM (benchmark_serving.py), compare tok/s, TTFT p50/p99, VRAM.
# Règle : même GPU, même modèle, même prompts, 3 runs — sinon le benchmark ne vaut rien.
```

Flags utiles : `--tp 2` (tensor parallel sur 2 GPU), `--quantization fp8` (si supporté par ton modèle/GPU), `--tool-call-parser qwen25` (tool calling natif). La doc de référence : docs.sglang.ai — vérifie les flags à chaque upgrade mineur, ça bouge vite.

### 163.8. Check-list : dois-tu migrer de vLLM vers SGLang ?

Ne migre pas sur un benchmark lu en ligne. Migre si tu coches au moins 3 cases :

- [ ] Ton trafic a un **gros préfixe partagé** (prompt système RAG > 1K tokens, identique à chaque requête)
- [ ] Tu sers un **MoE** (DeepSeek V3, Qwen3-235B, Kimi K2…) où SGLang a le support day-one
- [ ] Tu as besoin de **génération structurée intensive** (JSON garanti, classification fermée)
- [ ] Ton benchmark maison (même GPU, même modèle, même prompts, 3 runs) montre **> 15 %** de gain
- [ ] Tu acceptes un écosystème plus jeune (moins de réponses StackOverflow, doc qui bouge)

Si tu ne coches rien : reste sur vLLM, ton temps vaut plus que 3 % de débit. Si tu coches 3+ : fais tourner les deux en parallèle derrière ton routeur pendant 2 semaines (SGLang sur 20 % du trafic), compare TTFT p99 et taux d'erreur réels, puis décide. Le coût de la migration = zéro changement de code côté appelant (API OpenAI-compatible des deux côtés) — c'est un changement d'URL et de monitoring, pas un rewrite.

---

## 164. PyTorch : les bases pour l'IA (tensors, inférence, écosystème)

**En une phrase :** la fondation sur laquelle tournent (presque) tous les modèles que tu utilises — transformers, vLLM, SGLang, Unsloth ne sont que des couches au-dessus de `torch`. Comprendre les tensors et l'inférence te rend autonome quand ça casse.

### 164.1. État au 27/09/2026

- Version stable : **PyTorch 2.13** (08/07/2026) ; **2.14** sortie le 02/09/2026. Cycle : une mineure tous les ~2 mois. Les images RunPod listent 2.9 → 2.13 ; CUDA supporté : 12.8/12.9/13.0.
- Points à connaître : `torch.compile` est mature (accélère l'inférence en compilant le graphe), l'export ONNX utilise désormais `dynamo=True` par défaut (pipeline `torch.export`), et `torchaudio.io` a été supprimé au profit de `torchcodec` (depuis 2.9).
- Installation standard (adapte `cu130` à ton driver — `nvidia-smi` te dit la version CUDA max) :

```bash
pip install torch --index-url https://download.pytorch.org/whl/cu130
python -c "import torch; print(torch.__version__, torch.cuda.is_available())"
```

### 164.2. Les tensors : 5 minutes pour comprendre

Un tensor = un tableau n-dimensionnel typé, sur CPU ou GPU. Tout le reste (modèles, gradients, KV cache) n'est que des tensors et des opérations dessus.

```python
import torch

# Création
x = torch.tensor([[1.0, 2.0], [3.0, 4.0]])          # 2x2, CPU, float32
w = torch.randn(2, 2, device="cuda")                # aléatoire, sur GPU
print(x.shape, x.dtype, x.device)                   # torch.Size([2, 2]) torch.float32 cpu

# Opérations : broadcasting, matmul (le cœur des transformers)
y = x @ x.T                                         # multiplication matricielle
z = torch.softmax(y / 8 ** 0.5, dim=-1)             # softmax — oui, c'est l'attention en miniature

# Le gradient : la base de l'entraînement (et de rien d'autre en inférence)
p = torch.tensor(2.0, requires_grad=True)
loss = (p ** 2 - 4) ** 2
loss.backward()
print(p.grad)                                       # d(loss)/dp — l'optimiseur s'en sert pour mettre à jour p
```

Trois règles d'or :
1. **dtype** : `float32` (précis, lent, 4 octets), `float16`/`bfloat16` (2 octets — le standard inférence/entraînement moderne ; `bfloat16` préféré sur Ampere+ car plus stable), `int8`/`int4` (quantification — section 162).
2. **device** : un tensor GPU et un tensor CPU ne se mélangent pas — `x.to("cuda")` avant de calculer. L'erreur `Expected all tensors to be on the same device` = 90 % des bugs débutants.
3. **`.item()` / `.detach()`** : pour sortir un scalaire vers Python ou couper le graphe de gradient (utile en debug, jamais en prod chaude).

### 164.3. L'inférence : le pattern à connaître par cœur

```python
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer

model_id = "Qwen/Qwen3-8B"
tok = AutoTokenizer.from_pretrained(model_id)
model = AutoModelForCausalLM.from_pretrained(
    model_id,
    torch_dtype=torch.bfloat16,   # 2 octets/poids : ~16 Go pour un 8B
    device_map="auto",            # répartit sur GPU(s) dispo, sinon CPU
)
model.eval()                      # désactive dropout & co — OBLIGATOIRE en inférence

prompt = "Explique la différence entre kVA et kW pour un onduleur."
inputs = tok(prompt, return_tensors="pt").to(model.device)

with torch.no_grad():             # pas de graphe de gradient : 2 à 3x moins de VRAM
    out = model.generate(**inputs, max_new_tokens=200, do_sample=False)

print(tok.decode(out[0], skip_special_tokens=True))
```

Ce que ce code t'apprend, transposable partout :
- `torch.no_grad()` + `model.eval()` : le duo de l'inférence. Les oublier = VRAM qui explose + résultats non déterministes.
- `torch_dtype=torch.bfloat16` : divise la VRAM par 2 vs float32, sans perte visible sur un LLM.
- `device_map="auto"` (de la lib `accelerate`) : le pont entre « ça tourne sur mon laptop » et « ça tourne sur 2 GPU ».
- `torch.compile(model)` (une ligne, après le chargement) : peut accélérer l'inférence de 10–30 % après un warmup — gratuit, à tester.

Estimation VRAM d'un modèle : `params × octets/param` + KV cache. Un 8B en bfloat16 ≈ 16 Go de poids ; le KV cache ajoute ~1–2 Mo par token de contexte (d'où l'importance de la section 167 sur les tokens).

### 164.4. PyTorch dans ton écosystème : qui utilise quoi

```
PyTorch (tensors, autograd, torch.compile)
 ├── transformers (Hugging Face) : chargement/inférence/fine-tuning — la porte d'entrée
 │    └── TRL : SFTTrainer, GRPOTrainer (utilisé par Unsloth, section 162)
 ├── vLLM / SGLang : serveurs d'inférence (section 163) — kernels CUDA custom AU-DESSUS de torch
 ├── Unsloth : kernels Triton + backprop manuelle au-dessus de torch/TRL
 ├── NVIDIA NIM (section 165) : conteneur qui embarque TensorRT-LLM/vLLM/SGLang compilés
 └── llama.cpp / Ollama / LM Studio : monde C++ à part (GGUF) — PAS du PyTorch
```

Traduction opérationnelle : quand ton RAG rame, la question n'est presque jamais « PyTorch est-il lent ? » mais « quel étage de la pile est mal configuré ? » — dtype, batching, KV cache, quantification. Et quand tu dois debugger un script d'embedding ou de fine-tuning, c'est `torch.cuda.memory_summary()` et `nvidia-smi` qui te disent la vérité, pas les logs.

### 164.5. Debugger la VRAM : les 3 commandes qui disent la vérité

```python
import torch

# 1. Vue d'ensemble : alloué / réservé / libre
print(torch.cuda.memory_summary(device=0, abbreviated=True))

# 2. Le coupable classique : le cache allocator garde de la VRAM "réservée"
#    Si reserved >> allocated : torch.cuda.empty_cache() (ne corrige pas une fuite,
#    mais libère ce que l'allocator retient)
torch.cuda.empty_cache()

# 3. Trouver QUI mange la mémoire : snapshot (à activer AVANT le run)
torch.cuda.memory._record_memory_history(max_entries=100000)
# ... ton code d'inférence ...
torch.cuda.memory._dump_snapshot("vram.pkl")  # à ouvrir dans https://pytorch.org/memory_viz
torch.cuda.memory._record_memory_history(enabled=None)
```

Causes habituelles d'OOM en inférence : oubli de `torch.no_grad()` (le graphe de gradient reste en mémoire), batch trop gros, `max_new_tokens` délirant (le KV cache grandit avec la génération), deux modèles chargés « pour comparer » sur la même carte. En entraînement : ajoute le gradient checkpointing et la mixed precision (`torch.amp`).

### 164.6. Embeddings avec PyTorch : le chaînon vers ton RAG

Ton RAG indexe avec text-embedding-3-small (API). Le pendant local — utile pour chiffrer le « tout local » ou debugger — tient en 15 lignes avec `sentence-transformers` (qui n'est qu'un wrapper PyTorch + transformers) :

```python
# pip install sentence-transformers
from sentence_transformers import SentenceTransformer
import torch

model = SentenceTransformer("BAAI/bge-m3",  # multilingue, bon sur le FR — à benchmarker vs ton embedding actuel
                            model_kwargs={"torch_dtype": torch.float16})
docs = [
    "La tension de floating d'un bloc VRLA 12 V est de 13,5 à 13,6 V à 20 °C.",
    "Le code C6000 Kyocera signale une rupture de chauffe du four.",
    "Un onduleur double conversion isole totalement la charge du réseau.",
]
emb = model.encode(docs, normalize_embeddings=True)  # (3, 1024) — des tensors numpy
print(emb.shape, emb.dtype)

# Similarité cosinus = produit scalaire sur vecteurs normalisés
import numpy as np
q = model.encode(["Quelle tension pour un bloc batterie 12 V ?"], normalize_embeddings=True)
scores = q @ emb.T
print(scores)  # le doc 0 doit gagner — sinon, ton embedding ou ton chunking a un problème
```

Ce script est aussi ton **test de fumée d'embedding** : si la similarité ne classe pas correctement 10 paires évidentes de ton domaine, inutile d'indexer 10 000 documents — change d'embedding ou de chunking d'abord (sections 140–141).

### 164.7. `torch.compile` et l'inférence rapide : ce qu'il faut en retenir

```python
model = AutoModelForCausalLM.from_pretrained(model_id, torch_dtype=torch.bfloat16, device_map="auto")
model.eval()
model = torch.compile(model, mode="reduce-overhead")  # une ligne — compile le graphe

# Premier appel = compilation (lent, 1 à 5 min) ; les suivants = +10 à 30 %
with torch.no_grad():
    out = model.generate(**inputs, max_new_tokens=200)
```

Limites honnêtes : le warmup de compilation est réel (prévois-le au démarrage du service, pas à la première requête utilisateur) ; certains modèles exotiques « cassent » le graphe (graph breaks) et le gain s'évapore — mesure avant/après. vLLM/SGLang font déjà ce genre d'optimisations (et bien plus) en interne : `torch.compile` manuel sert surtout pour tes scripts maison (embedding batch, reranking, prototypes).

---

## 165. NVIDIA NIM : l'inférence en microservice clé en main

**En une phrase :** un conteneur Docker par modèle, maintenu par NVIDIA, qui embarque poids optimisés + moteur d'inférence (TensorRT-LLM, vLLM ou SGLang selon le profil) + API OpenAI-compatible — tu `docker run`, ça sert.

### 165.1. Principe

NIM (NVIDIA Inference Microservice) répond à une question simple : « que faut-il pour servir un LLM comme n'importe quel service de prod ? » La réponse de NVIDIA : un conteneur qui fait tout, tout seul.

```
docker run ...
      ↓
NIM inspecte ton GPU
      ↓
choisit le profil optimal pour ce GPU (moteur + quantification validés par NVIDIA)
      ↓
télécharge les poids depuis NGC (ou utilise le cache local)
      ↓
démarre le moteur avec une config validée
      ↓
expose une API OpenAI-compatible sur le port 8000
```

Dans le conteneur, toujours les mêmes briques : poids optimisés par NVIDIA · moteur d'inférence (vLLM, TensorRT-LLM ou SGLang selon profil) · serveur API `/v1/chat/completions` · sondes `/v1/health/live` et `/v1/health/ready` · métriques Prometheus sur `/metrics`. Ce ne sont pas des options : c'est le contrat de chaque NIM.

Positionnement dans la gamme NVIDIA : **NeMo** = entraîner/fine-tuner · **NIM** = servir · **TensorRT-LLM** = le moteur d'optimisation sous le capot. Tu ne fine-tunes pas avec NIM, tu sers avec.

### 165.2. Déploiement (vérifié sept 2026)

Prérequis : GPU NVIDIA (datacenter, workstation ou RTX récent — il existe des NIM « RTX » pour postes locaux, produit distinct de l'offre cloud), driver récent, Docker + nvidia-container-toolkit, et une **clé API NGC** (gratuite, depuis ton compte NVIDIA) pour tirer les conteneurs du registre `nvcr.io`.

```bash
# 1. Login au registre NVIDIA (clé API NGC — mets la vraie dans un secret, jamais en clair)
echo "$NGC_API_KEY" | docker login nvcr.io --username '$oauthtoken' --password-stdin

# 2. Lancer un NIM (exemple : Llama 3.1 8B instruct — adapte le tag à ton modèle)
docker run -d --name nim-llama \
  --gpus all \
  -p 8000:8000 \
  -e NGC_API_KEY="$NGC_API_KEY" \
  -v "$HOME/.cache/nim:/opt/nim/.cache" \
  nvcr.io/nim/meta/llama-3.1-8b-instruct:latest

# 3. Attendre que le modèle soit prêt (téléchargement + optimisation au 1er lancement)
curl http://localhost:8000/v1/health/ready

# 4. Inférence — même format qu'OpenAI
curl http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "meta/llama-3.1-8b-instruct",
    "messages": [{"role": "user", "content": "Qu'est-ce qu'un bypass de maintenance sur un onduleur ?"}],
    "max_tokens": 300
  }'
```

En Kubernetes (Helm, vérifié sur des déploiements 2026) :

```bash
helm repo add nim https://helm.ngc.nvidia.com/nim
helm install nim-llama nim/meta-llama3-3-70b-instruct \
  --namespace nim --create-namespace \
  --set model.ngcAPIKey="$NGC_API_KEY"
# Le pod expose /v1/chat/completions : ton code existant ne change pas
```

Notes de prod :
- **Premier démarrage = long** : téléchargement des poids + construction du moteur TensorRT-LLM (plusieurs minutes à dizaines de minutes selon modèle/GPU). Monte un volume de cache (`/opt/nim/.cache`) pour ne payer ce coût qu'une fois.
- Catalogue : des dizaines de modèles packagés (Llama, Mistral, Qwen, Gemma, embeddings, rerankers, vision…) sur `build.nvidia.com` — vérifie que *ton* modèle y existe avant d'architecturer autour de NIM.
- NIM existe aussi en version **workstation/RTX** (Windows via WSL2) pour faire tourner des modèles localement sur un PC RTX — pratique pour une démo ou un poste de dev, pas pour servir une équipe.

### 165.3. Cas d'usage — et quand NE PAS l'utiliser

| Cas | NIM pertinent ? | Pourquoi |
|---|---|---|
| Servir 1–N modèles en prod sur GPU NVIDIA, sans tuner de moteur | ✅ Oui | C'est exactement le produit : profil validé, API standard, sondes/métriques incluses |
| Équipe qui veut une API OpenAI-compatible interne rapidement | ✅ Oui | `docker run` + clé NGC, pas de code de serving à écrire |
| Kubernetes avec autoscaling | ✅ Oui | Chart Helm officiel, HPA sur les métriques `/metrics` |
| Fine-tuner un modèle | ❌ Non | C'est NeMo (ou Unsloth, section 162) |
| GPU non-NVIDIA (AMD, Apple Silicon, CPU) | ❌ Non | NIM = NVIDIA uniquement ; vLLM/SGLang/Ollama sinon |
| Modèle exotique non packagé par NVIDIA | ❌ Non | Pas de NIM officiel = retour à vLLM/SGLang manuel |
| Budget serré / spot Vast.ai | ⚠️ Réfléchir | Le conteneur est lourd (plusieurs Go) et le 1er boot coûte cher en temps ; sur du spot préempté souvent, vLLM léger peut être plus agile |

Ordre de grandeur perf (tiers, 2026) : Llama 3.3 70B en NIM sur H100 ≈ 2 400 tok/s vs ~1 200 tok/s en vLLM vanille sur le même hardware — l'écart vient du backend TensorRT-LLM compilé spécifiquement pour ce couple modèle/GPU. C'est le deal NIM : tu échanges du contrôle (choix du moteur, flags fins) contre de la perf validée et du temps d'ingénierie économisé.

### 165.4. NIM dans ta stack RAG

Scénario réaliste : ton routeur LiteLLM pointe `/v1` vers un NIM local pour les modèles open-weight (zéro coût marginal au token, données on-prem), et vers les API cloud (OpenAI/Anthropic/DeepSeek/Doubao) pour le reste. Le jour où tu changes de modèle, tu changes de conteneur — l'API ne bouge pas. Pour ton cas (données techniques internes, contrainte de confidentialité potentielle), c'est l'option « LLM local en prod sans devenir expert CUDA » la plus crédible du marché en sept 2026.

### 165.5. Catalogue et clés : le parcours sans surprise

1. **build.nvidia.com** : le catalogue des NIM — chaque fiche modèle indique les GPU supportées, la précision (FP8/FP16…), le débit attendu. **Vérifie la tienne avant tout** : un NIM Llama-70B exige ~2×H100 ou équivalent, pas ta 4090.
2. **Clé NGC** : gratuite sur org.ngc.nvidia.com (compte NVIDIA) — elle sert à la fois à tirer le conteneur (`docker login nvcr.io`) ET à télécharger les poids au premier démarrage. Mets-la dans un secret (Docker secret, Vault, variable d'env du compose — jamais en clair dans un repo).
3. **Versionnement** : épingle le tag du conteneur (`:1.8.3`, pas `:latest`) — un NIM qui se met à jour tout seul un dimanche soir, c'est une loterie de perf et de comportement.
4. **Licence des poids** : le conteneur est de NVIDIA, mais les poids restent sous la licence du modèle (Llama Community, etc.) — vérifie la compatibilité avec ton usage commercial.

### 165.6. Supervision : les métriques que tu branches à ton monitoring

Chaque NIM expose `/metrics` au format Prometheus — branche-le à ta stack existante (tu as déjà Zabbix/Prometheus/Grafana dans tes guides) :

```yaml
# prometheus.yml — scrape du NIM
scrape_configs:
  - job_name: 'nim-llm'
    static_configs:
      - targets: ['nim-serveur:8000']
    metrics_path: '/metrics'
```

Métriques à alerter : **time-to-first-token** (dégradation = GPU saturée ou KV cache plein), **tokens/s**, **nombre de requêtes en file** (queue depth qui monte = sous-dimensionné), **santé** via `/v1/health/ready` (sonde Kubernetes `readinessProbe` — le pod ne reçoit du trafic que quand le modèle est chargé). Sans ces 4 signaux, tu pilotes un service d'inférence à l'aveugle — inacceptable en prod, même interne.

### 165.7. NIM vs « vLLM à la main » : l'arbitrage honnête

| Critère | NIM | vLLM / SGLang manuel |
|---|---|---|
| Temps jusqu'au 1er token en prod | ~1 h (pull + run) | 1–3 jours (tuning, tests) |
| Perf | Validée par NVIDIA pour ce couple modèle/GPU | À toi de la trouver (flags, quants, batching) |
| Contrôle fin | Faible (profils imposés) | Total |
| Poids du conteneur | Lourd (plusieurs Go + poids) | Léger (image Python + poids HF) |
| Modèles supportés | Catalogue NVIDIA uniquement | Tout Hugging Face |
| GPU non-NVIDIA | Non | Oui (vLLM : AMD/Intel/CPU) |
| Coût d'expérimentation | Faible | Faible aussi |
| Coût de sortie | Changer de moteur = changer de produit | Tu restes maître de ta stack |

Verdict : NIM quand tu veux un **résultat** (une API qui sert vite et bien) ; vLLM/SGLang manuel quand tu veux une **compétence** (comprendre et contrôler toute la chaîne). Les deux ne s'excluent pas : NIM pour le service stable, vLLM en labo pour tester les nouveautés du catalogue HF avant qu'elles n'arrivent en NIM.

---

## 166. NotebookLM (Google) : le RAG-as-a-product — et ce qu'il t'apprend pour ton RAG

**En une phrase :** le carnet de recherche de Google où tu déposes tes sources (PDF, Docs, liens, YouTube) et obtiens des réponses **ancrées avec citations cliquables** — un RAG clé en main, gratuit, excellent comme référence UX… mais qui ne remplace pas ton pipeline.

### 166.1. Présentation (état au 27/09/2026 — vérifié)

- **Nouveau nom depuis le 16/07/2026 : « Gemini Notebook »** — l'ancienne adresse notebooklm.google redirige vers notebook.google. Même produit, rattachement marketing à l'app Gemini.
- Principe : tu crées un carnet, tu ajoutes des sources, tu interroges. Chaque réponse pointe vers des **citations = extraits textuels directs** de tes sources (survol → texte complet, clic → saut au passage). Si la réponse n'est pas dans le corpus, il s'abstient — c'est le comportement que ton RAG devrait viser.
- Nouveautés 2026 : **exécution de code** dans chaque carnet (un ordinateur cloud sécurisé pour analyser tes données) ; **fonctionnalités d'étude** (15/09/2026 : conversations vocales temps réel, enregistreur de cours sur mobile) ; **Audio/Video Overviews** (le « podcast » généré depuis tes sources, 80+ langues), mind maps, flashcards, quiz, rapports, infographies, slide decks.
- Modèle sous le capot : Gemini (fenêtre 1M tokens côté chat du carnet).

### 166.2. Limites et quotas (sept 2026 — chiffres tiers, à vérifier sur l'aide officielle)

Google a remplacé les plafonds journaliers fixes par un **budget de calcul qui se recharge toutes les 5 heures** (02/09/2026) — plus flexible, mais rend les volumes prévisibles difficiles à anticiper. Ordres de grandeur publiés par des tiers (pas de page officielle unique) :

| Élément | Ordre de grandeur |
|---|---|
| Carnets | ~100 (gratuit) à 500 (payant) par utilisateur |
| Sources par carnet | ~50 (gratuit) à 300 (payant) |
| Taille par source | **500 Mo ou 500 000 mots** (seul plafond documenté par Google) |
| Chats | ~500 / jour / utilisateur |
| Audio/Video Overviews, mind maps, rapports | ~20 / jour chacun |
| Infographies | ~3 / jour observés en gratuit (non officiel) |

- **Tarif** : gratuit pour un compte Google standard (« free forever ») ; quotas élevés via Google One AI Premium (~$19,99/mois) ou Workspace/Éducation.
- **Confidentialité** : pour les comptes Workspace, Google indique que les uploads, requêtes et réponses ne sont ni relus par des humains ni utilisés pour entraîner les modèles. En compte grand public gratuit, pars du principe inverse pour des documents sensibles — **ne dépose jamais de doc interne confidentiel** sans validation de ta politique de sécurité.

### 166.3. Lien direct avec ton RAG : l'utiliser comme banc d'essai

NotebookLM est un excellent **étalon** pour ton pipeline, pas un remplaçant :

1. **Référence UX** : les citations inline avec extraits + abstention hors-corpus, c'est le gold standard. Si tes réponses RAG n'atteignent pas ce niveau de traçabilité, c'est ta spec d'amélioration.
2. **Benchmark qualité** : dépose le même corpus (ex. 20 PDF de docs Huawei/onduleurs) dans NotebookLM et dans ton RAG, pose les mêmes 30 questions d'éval (section 127), compare. Ça te donne un plafond « RAG industriel gratuit » à battre.
3. **Prototypage rapide** : avant d'ingérer un nouveau corpus dans ton pipeline, teste en 10 minutes dans NotebookLM si le corpus répond bien aux questions — ça évite d'indexer des documents inutiles.
4. **Idées de features** : Audio Overview = une façon de « consommer » tes guides ; les mind maps = une visualisation de la structure de ton corpus.

### 166.4. Limites honnêtes — pourquoi tu gardes ton RAG

- **Pas d'API publique officielle** : tout est web UI. Il existe des libs tierces non officielles (notebooklm-py, open-notebooklm) qui pilotent l'interface — fragiles par construction (un changement de RPC côté Google et ça casse), à proscrire en prod.
- **Zéro contrôle sur le pipeline** : tu ne choisis ni le chunking (section 140), ni le modèle d'embedding (tu es sur text-embedding-3-small, lui utilise l'embedding de Gemini), ni le reranking, ni la stratégie de récupération. Quand la qualité baisse, tu ne peux pas debugger — tu subis.
- **Tes données quittent ton infra** : cloud Google, juridiction US. Pour des docs d'exploitation internes, c'est un non-starter sans accord DPO.
- **Qualité = qualité des sources** : « answers depend on what you upload » — un corpus mal curé donne des réponses mal ancrées, exactement comme ton RAG. L'outil ne remplace pas la curation.
- **Quotas imprévisibles** : le budget de calcul rend les gros batchs (réindexer 500 docs, générer 200 quiz) aléatoires en gratuit.
- **Enfermement** : pas d'export structuré propre du graphe de connaissances ; tes carnets vivent chez Google.

**Doctrine** : NotebookLM = labo d'idées et étalon qualité, jamais une brique de ton pipeline. Ton RAG reste chez toi : tes embeddings, ton chunking, tes modèles, tes coûts maîtrisés (sections 140–141, 167).

### 166.5. Workflow concret : ton premier carnet « docs onduleurs » en 15 minutes

```text
1. notebook.google → Nouveau carnet → nomme-le « Onduleurs – base documentaire »
2. Ajouter des sources : glisse 5–10 PDF (manuels Easy UPS 3S, ton guide onduleurs_ups_guide.md
   exporté en PDF, 2-3 fiches constructeurs). Formats acceptés : PDF, Google Docs/Slides,
   TXT/MD, liens web, vidéos YouTube (transcription), audio.
3. Attends l'indexation (quelques minutes). Vérifie le compteur de sources.
4. Pose tes 5 questions les plus fréquentes du support, ex. :
   « Quelle section décrit la procédure de bypass de maintenance ? »
5. Pour chaque réponse : CLIQUE les citations. Si la citation ne contient pas vraiment
   la réponse → note-le : c'est exactement le test « fidélité » à appliquer à ton RAG.
6. Génère un Audio Overview (2–3 min) : écoute ce que « ton corpus raconté » donne —
   révélateur des trous et redondances du corpus.
```

Ce que tu en tires pour ton RAG : la liste des questions où NotebookLM échoue te dit où TON pipeline doit être meilleur (ou au moins aussi bon) ; les citations te donnent le format de réponse cible ; l'audio te fait « entendre » la qualité de ton corpus.

### 166.6. Les features 2026 qui comptent vraiment pour un tech

- **Exécution de code** : chaque carnet a un environnement Python cloud. Cas d'usage : « à partir de ces 3 rapports de maintenance en PDF, sors-moi un tableau des pannes par mois » — sans exporter les fichiers. Pour toi : prototype d'analyse documentaire avant d'automatiser dans ton pipeline.
- **Data Tables** : extraction structurée multi-sources (« compare les caractéristiques des Easy UPS 3S/3M, Eaton 9PX et Vertiv Liebert à partir de ces fiches ») → tableau exportable. C'est du RAG + structuration en un clic.
- **Deep Research** : le carnet peut chercher ET croiser tes sources avec le web — utile pour une veille (« qu'est-ce qui a changé sur la norme IEC 62040 depuis mon manuel de 2023 ? »), mais attention : dès qu'il sort de tes sources, la garantie d'ancrage tombe.
- **Quiz / flashcards** : génère des QCM depuis tes guides — recyclable pour former ton équipe (tes 72 sections Kyocera → 50 questions d'habilitation, par exemple).

### 166.7. Check-list gouvernance avant d'y mettre un doc d'entreprise

1. **Classification** : public / interne / confidentiel — seul le « public » et l'« interne non sensible » vont dans un compte Google standard. Le confidentiel : Workspace avec garanties contractuelles, ou rien.
2. **Compte dédié** : n'utilise pas ton compte perso — un compte de service/équipe, avec 2FA, dont l'accès est révocable.
3. **Pas de données personnelles** : les rapports d'intervention contiennent des noms, des sites clients — anonymise avant upload (c'est aussi une bonne hygiène pour ton RAG).
4. **Traçabilité** : note quel corpus est dans quel carnet (un simple `CORPUS.md`) — dans 6 mois, tu dois savoir d'où vient une réponse que tu as citée.
5. **Sortie** : tout ce qui est produit dans NotebookLM (notes, rapports) et réutilisé ailleurs doit être re-vérifié — l'outil ne signe pas ses affirmations, les citations si.

### 166.8. NotebookLM vs ton RAG maison : le comparatif honnête

| Critère | NotebookLM (Gemini Notebook) | Ton RAG (pipeline maison) |
|---|---|---|
| Mise en route | 15 min, zéro code | Jours/semaines de dev |
| Citations | Inline, cliquables, extraits — excellent | À construire (ton chantier) |
| Contrôle chunking/embedding | Aucun | Total (sections 140–141) |
| Choix du LLM | Gemini imposé | Ton routeur (LiteLLM) |
| API programmable | Non (libs tierces fragiles) | Oui, natif |
| Données | Cloud Google | Chez toi (on-prem possible) |
| Coût | Gratuit (quotas) | Tokens + infra, prévisible |
| Quotas | Budget de calcul, imprévisible en pic | Tes limites à toi |
| Audit/debug | Boîte noire | Logs, métriques, reproductible |

Lecture : NotebookLM gagne sur le time-to-value et l'UX des citations ; ton RAG gagne sur tout le reste dès que c'est sérieux (prod, confidentialité, automatisation). Utilise le premier pour **apprendre** (UX cible, test de corpus), le second pour **servir**.

### 166.9. Pour ton équipe : former avec les quiz générés

Cas d'usage sous-estimé pour un chef de service : tes guides (Kyocera, onduleurs, Huawei — des milliers de lignes) → carnet NotebookLM par thème → génération de **quiz/flashcards** → 15 min de formation hebdo pour l'équipe, notée. Avantages : questions ancrées dans TES docs (pas de QCM générique), génération en 2 minutes, révision facile quand une procédure change. Limite : relis chaque quiz généré — le modèle peut produire des questions ambiguës ou des réponses discutables sur des points techniques fins (couple de serrage, seuils d'alarme). Le quiz est un brouillon, pas une habilitation.

---

## 167. Token / tokenisation : l'unité qui drive ton coût et ta fenêtre de contexte

**En une phrase :** tout ce que tu paies et tout ce qui tient en mémoire se compte en tokens, pas en mots ni en caractères — et le français coûte plus cher que l'anglais à contenu égal. Cette section te donne les algorithmes, les outils et les vrais chiffres.

### 167.1. Ce qu'est un token

Un token = l'unité atomique que le modèle lit et génère. Ce n'est ni un mot, ni un caractère : c'est un **morceau de mot** appris statistiquement. Exemples mesurés le 27/09/2026 avec `tiktoken` (encodeur `cl100k_base`, celui de GPT-3.5/4) :

```python
import tiktoken
enc = tiktoken.get_encoding("cl100k_base")

enc.encode("Hello world")          # [9906, 1917]  → 2 tokens
enc.encode("Le rapport de maintenance du groupe électrogène doit être archivé avant vendredi.")
# → 18 tokens (81 caractères, soit 4,5 car/token)
enc.encode("The generator maintenance report must be archived before Friday.")
# → 10 tokens (64 caractères, soit 6,4 car/token)
```

Même sens, **18 tokens en français contre 10 en anglais** : à contenu égal, ton corpus français coûte ~1,5 à 1,8x plus cher en tokens que l'équivalent anglais sur un tokenizer entraîné majoritairement sur de l'anglais. C'est mécanique, pas un complot : le vocabulaire contient des mots anglais entiers (« maintenance » = 1 token) mais découpe le français en sous-morceaux.

Vocabulaires mesurés : `cl100k_base` = **100 277** tokens ; `o200k_base` (nouvelle génération OpenAI) = **200 019** tokens. Plus de vocabulaire = mots rares mieux couverts = moins de fragmentation.

### 167.2. Les algorithmes : BPE, WordPiece, Unigram

**BPE (Byte-Pair Encoding)** — le standard (GPT-2/3/4, RoBERTa, LLaMA, Qwen…) :
1. Vocabulaire de départ = caractères (ou octets) individuels.
2. Trouve la paire de symboles adjacents la plus fréquente dans le corpus d'entraînement.
3. Fusionne-la en un nouveau token, ajoute au vocabulaire.
4. Répète jusqu'à la taille de vocabulaire cible (ex. 100K, 200K).

Résultat : les séquences fréquentes (« maintenance », « https:// », « \n\n ») deviennent des tokens uniques ; les mots rares se décomposent en sous-mots connus. Jamais de mot « inconnu » : au pire, ça retombe sur les octets.

| Algorithme | Principe | Utilisé par |
|---|---|---|
| **BPE** | Fusions gloutonnes par fréquence | GPT, LLaMA, Qwen, Mistral, RoBERTa |
| **WordPiece** | Fusions par vraisemblance (pas fréquence brute) | BERT, DistilBERT |
| **Unigram / SentencePiece** | Part d'un gros vocabulaire, élague par probabilité ; traite le texte brut sans pré-découpe | T5, ALBERT, LLaMA (SentencePiece), Gemma |
| **tiktoken** | Implémentation BPE open source d'OpenAI (Rust) | GPT-3.5/4/4o, o-series, embeddings OpenAI |

Point crucial : **chaque modèle a SON tokenizer**. Compter avec `cl100k_base` pour dimensionner un appel à Qwen ou Llama donne une approximation (±10–20 %), pas le chiffre exact. Pour le chiffre exact, utilise le tokenizer du modèle (`AutoTokenizer.from_pretrained` — voir 167.4).

### 167.3. La librairie `tokenizers` (Hugging Face)

L'implémentation de référence : cœur **Rust**, bindings Python/Node — ~10 à 100x plus rapide que du Python pur, < 20 s pour tokeniser 1 Go de texte sur CPU. C'est ce que `transformers` utilise en interne quand tu charges un `AutoTokenizer`.

```bash
pip install tokenizers          # seul
pip install tokenizers transformers   # avec l'écosystème HF
```

```python
from tokenizers import Tokenizer
from tokenizers.models import BPE
from tokenizers.trainers import BpeTrainer
from tokenizers.pre_tokenizers import ByteLevel

# Entraîner un mini-tokenizer BPE sur TON corpus (utile pour mesurer,
# ou pour un tokenizer métier — voir l'encadré RAG plus bas)
tok = Tokenizer(BPE(unk_token="[UNK]"))
tok.pre_tokenizer = ByteLevel()
trainer = BpeTrainer(vocab_size=30000,
                     special_tokens=["[UNK]", "[CLS]", "[SEP]", "[PAD]", "[MASK]"])
tok.train(["corpus_maintenance.txt"], trainer)
tok.save("tokenizer-metier.json")

out = tok.encode("Remplacer le kit de fusion FK-475")
print(out.tokens)   # ex. ['Remplacer', ' le', ' kit', ' de', ' fusion', ' FK', '-', '475']
print(out.ids)      # les IDs numériques envoyés au modèle
print(len(out.ids)) # le compteur qui fait ta facture
```

Pipeline interne d'un encode : `texte brut → normalisation → pré-tokenisation → modèle (BPE/…) → post-processing (tokens spéciaux) → IDs`. Quand un découpage te surprend, c'est presque toujours la pré-tokenisation (espaces, ponctuation, accents) qui en est la cause.

### 167.4. Compter ses tokens AVANT d'appeler : les 3 patterns

**Pattern 1 — tiktoken (API OpenAI / compatible) :**

```python
import tiktoken

def count_openai(text: str, model: str = "gpt-4o") -> int:
    enc = tiktoken.encoding_for_model(model)  # choisit o200k_base pour gpt-4o
    return len(enc.encode(text))

# Garde-fou budgétaire : tronquer AVANT l'appel, pas après la facture
MAX_TOKENS = 8000
prompt = system_prompt + "\n\n" + "\n\n".join(chunks)
if (n := count_openai(prompt)) > MAX_TOKENS:
    raise ValueError(f"Prompt trop long : {n} tokens > {MAX_TOKENS}")
```

**Pattern 2 — le tokenizer du modèle local (le chiffre exact) :**

```python
from transformers import AutoTokenizer

tok = AutoTokenizer.from_pretrained("Qwen/Qwen3-8B")  # le VRAI tokenizer du modèle servi
n = len(tok.encode("Le rapport de maintenance doit être archivé avant vendredi."))
print(n, "/", tok.model_max_length)   # tokens / fenêtre de contexte du modèle
```

**Pattern 3 — estimation rapide sans lib (règle du pouce) :**

```text
Anglais : 1 token ≈ 4 caractères ≈ 0,75 mot
Français (cl100k_base, texte varié) : 1 token ≈ 4,5 caractères  → ×1,5 à ×1,8 vs anglais
Français technique RÉPÉTITIF : ~10 caractères/token (mesuré : 292 tokens pour ~3000 car.
de procédure copieur répétée 3× — la répétition se re-tokenize efficacement)
```

### 167.5. Impact direct n°1 : le coût

Toutes les API du guide facturent **au million de tokens**, entrée et sortie séparément. Exemple chiffré avec ton cas RAG (prix « vérifiés le 27/09/2026 », section 160) :

```text
Question RAG typique sur Doubao Seed 2.1 Pro (¥6 entrée / ¥30 sortie par M) :
- prompt système :            ~500 tokens
- 5 chunks FR de ~1000 car. : ~5 × 100 tokens = ~500 tokens   (mesuré §167.4)
- question utilisateur :      ~30 tokens
- total entrée :              ~1030 tokens → ¥0,00618  (~0,0009 $)
- réponse générée (300 mots FR) : ~600 tokens → ¥0,018  (~0,0027 $)
- COÛT TOTAL par question :   ~¥0,024  (moins d'un demi-centime d'euro)
```

À 1 000 questions/jour : ~¥24/jour (~3,5 $/jour). Le poste qui explose n'est jamais UNE question, c'est le volume × la taille des chunks × l'absence de cache. D'où :
- **compte tes chunks** : un chunk de 2000 caractères FR ≈ 200–450 tokens selon répétitivité — diviser la taille des chunks par 2 divise presque par 2 ton coût d'entrée ;
- **active le cache de prompt** quand l'API le propose (Doubao : ¥1,20/M en cache vs ¥6 — ton prompt système RAG est le candidat idéal) ;
- **l'embedding aussi se paie au token** : text-embedding-3-small facture ses tokens d'entrée (prix public OpenAI ~$0,02/M, à vérifier) — indexer 1 Go de docs FR ≈ ~100–220 M tokens ≈ $2–4,5 d'indexation. Négligeable une fois, à surveiller si tu réindexe souvent.

### 167.6. Impact direct n°2 : la fenêtre de contexte et la VRAM

La fenêtre de contexte se paie deux fois : en **prix** (tokens d'entrée) et en **mémoire** (KV cache).

```text
256K tokens, ça représente quoi en français ?
  256 000 tokens ÷ ~700 tokens/page A4  ≈  366 pages A4
128K tokens ≈ 183 pages A4. Ton corpus Huawei complet ne tient PAS dans un prompt —
  d'où le RAG (section 140) : on n'envoie que les bons chunks, pas tout.
```

KV cache (ordre de grandeur, à mesurer sur ton setup avec `torch.cuda.memory_summary()`) : pour un 8B en bfloat16, compte ~1–2 Mo de VRAM **par token de contexte**. 32K tokens de contexte ≈ 32–64 Go de KV cache — plus que les 16 Go des poids eux-mêmes. C'est pour ça que :
- les serveurs (vLLM/SGLang, section 163) gèrent le KV cache au cordeau (PagedAttention, RadixAttention) ;
- un prompt système de 10K tokens « parce qu'on a la place » est une mauvaise idée même quand la fenêtre le permet : tu paies le cache à chaque requête ;
- la règle d'or du RAG reste : **le plus petit contexte qui répond bien** (top-k serré, chunks denses — section 140).

### 167.7. Check-list token pour ton pipeline

1. **Un compteur par brique** : `tiktoken` pour les API OpenAI-compatibles, `AutoTokenizer` du modèle pour le local. Jamais d'estimation « au mot » dans du code de prod.
2. **Garde-fou avant chaque appel** : `if tokens > MAX: tronquer/alerter` — une boucle qui accumule des chunks sans compter, c'est une facture surprise ou un 400 Bad Request.
3. **Mesure ton corpus une fois** : script qui tokenize tout ton corpus avec le tokenizer de ton modèle d'embedding → tu connais ton coût d'indexation AVANT de payer, et tu repères les documents pathologiques (un PDF de 500 pages scannées en OCR bruité peut exploser le compteur).
4. **Français = budget ×1,5** : quand tu lis un benchmark de coût en anglais (« $X pour 1M tokens »), multiplie par ~1,5–1,8 pour ton usage FR réel.
5. **Tokens spéciaux** : les templates de chat (`<|im_start|>`, etc.) ajoutent des tokens invisibles — compte le prompt **après** application du template, pas avant.
6. **Ne pas optimiser prématurément** : entraîner ton propre tokenizer (167.3) ne sert que si tu entraînes aussi ton modèle. Pour un RAG sur modèles existants, le tokenizer est imposé — ton levier, c'est la taille et la densité des chunks.

### 167.8. Quel tokenizer pour quel modèle : le tableau de correspondance

| Modèle / API | Tokenizer à utiliser pour compter | Vocabulaire | Remarque |
|---|---|---|---|
| GPT-4o, o-series, embeddings OpenAI | `tiktoken` → `o200k_base` | 200 019 | `tiktoken.encoding_for_model("gpt-4o")` |
| GPT-3.5 / GPT-4 | `tiktoken` → `cl100k_base` | 100 277 | L'ancienne génération |
| Qwen3, Qwen2.5 | `AutoTokenizer.from_pretrained("Qwen/Qwen3-8B")` | ~152K | BPE byte-level, bon sur le FR |
| Llama 3.x | `AutoTokenizer.from_pretrained("meta-llama/Llama-3.1-8B")` | 128K | Logique proche de tiktoken |
| Mistral / Mixtral | Tokenizer Mistral (`mistral-common`) | 32K (v1/v2) | Vocabulaire petit → FR plus fragmenté |
| Gemma 3 | `AutoTokenizer.from_pretrained("google/gemma-3-4b-it")` | 256K | Gros vocabulaire, SentencePiece |
| DeepSeek V3/V4 | Tokenizer DeepSeek (`deepseek-ai/DeepSeek-V3`) | ~128K | Proche de l'esprit Llama |
| Doubao Seed | Tokenizer Volcano/Doubao (via doc Ark) | à vérifier | À vérifier sur la doc — ne pas supposer cl100k |

Erreur classique : compter avec `cl100k_base` puis appeler Qwen — l'écart de ±10–20 % suffit à faire dépasser une fenêtre ou à fausser un devis sur gros volumes. **Le compteur doit matcher le modèle servi.** Dans ton routeur, associe chaque modèle à son compteur (un dict `model_id → fonction de comptage`).

### 167.9. Exercice chiffré : dimensionner le budget tokens de ton RAG

Prenons ton cas réel — RAG FR technique, 10 000 documents d'~5 000 caractères chacun (PDF de manuels), chunks de 1 000 caractères avec 200 de recouvrement :

```text
1. Indexation (une fois) :
   10 000 docs × 5 000 car. = 50 M caractères
   FR technique : ~100 tokens / 1 000 car. (mesuré §167.4, texte répétitif)
   → ~5 M tokens d'embedding
   text-embedding-3-small ~$0,02/M (prix public, à vérifier) → ~$0,10 d'indexation
   → L'indexation est quasi gratuite. La RÉINDEXATION fréquente, non.

2. Requête type (Doubao Seed 2.1 Pro, §167.5) :
   entrée ~1 030 tokens → ¥0,0062 | sortie ~600 tokens → ¥0,018
   → ~¥0,024 / question (~0,0035 €)

3. À 500 questions/jour ouvrés (équipe support) :
   500 × 22 jours × ¥0,024 ≈ ¥264/mois ≈ 38 €/mois
   → Le LLM de génération : 38 €/mois. Le VRAI coût : ton temps de curation.

4. Piège : chunks ×2 (2 000 car.) sans rerank
   → entrée ~2 000 tokens/question → ~76 €/mois pour une qualité pas forcément meilleure.
   → D'où la section 140 : des chunks DENSES et un top-k serré valent mieux que du volume.
```

Moralité chiffrée : sur un RAG interne, le coût tokens est un bruit de fond (dizaines d'euros/mois) — **l'optimisation rentable n'est pas le prix au token, c'est la précision** (moins de questions reformulées, moins de chunks inutiles envoyés). Le jour où tu exposes le RAG à 500 utilisateurs, refais ce calcul : c'est là que le cache de prompt (Doubao ¥1,20/M) et le choix du modèle (Turbo vs Pro) changent la donne.

### 167.10. Tokens et sécurité : deux angles morts

1. **Saturation par document piégé** : un attaquant qui contrôle un document ingéré peut y glisser des milliers de tokens invisibles (texte blanc sur blanc, div cachées) pour saturer ta fenêtre ou gonfler ta facture. Contre-mesures : plafond de tokens PAR SOURCE à l'ingestion, détection des écarts (un chunk « normal » fait 100–400 tokens ; un chunk à 8 000 tokens mérite un examen).
2. **Usurpation de rôle via les délimiteurs** : les templates de chat ajoutent des marqueurs (`<|im_start|>`…) — si ton RAG concatène naïvement des contenus non fiables SANS passer par le template du modèle, un document peut forger un faux marqueur système. Toujours appliquer le chat template du tokenizer (`tok.apply_chat_template`) sur des messages STRUCTURÉS, jamais sur du texte brut concaténé.

> **Rappel de doctrine :** le token est l'unité de compte de toute ta stack IA — facturation API, fenêtre de contexte, VRAM du KV cache, coût d'indexation. Celui qui ne compte pas ses tokens pilote à l'aveugle. Un compteur de 10 lignes (167.4) dans ton routeur vaut tous les tableaux de prix du monde.

*Fin du guide — 167 sections. Extension du 27/09/2026 : sections 160 (Doubao), 161 (LM Studio), 162 (Unsloth), 163 (SGLang), 164 (PyTorch), 165 (NVIDIA NIM), 166 (NotebookLM), 167 (tokenisation). Prix « vérifiés le 27/09/2026 » par recherche web ; chiffres de tokenisation mesurés localement avec tiktoken (cl100k_base) le 27/09/2026 ; le reste « à vérifier » sur les pages officielles avant toute décision budgétaire. Rappel : « gemini 4 », « GPT-6 Terra » et assimilés n'existent pas au 27/09/2026 — ne pas les coder en dur (cf. section 159).*
