# IA générative : image, vidéo, recherche

> Guide terrain pour Zelef — chef de service systèmes & énergies, sysadmin, constructeur de son propre RAG.
> **Périmètre :** outils de génération d'image, de vidéo et de recherche IA, avec prix, API, licences et pièges.
> **Ce guide ne duplique pas** les volumes existants (`ia_modeles_chine.md`, `ia_modeles_occident.md`, `ia_modeles_specialises.md`) : des renvois explicites sont faits quand un sujet y est déjà couvert.
> **Dates :** toutes les informations datées ont été vérifiées le **27/09/2026** sauf mention contraire. Les prix et versions marqués « à vérifier » n'ont pas pu être confirmés de source fiable à cette date.

---

## 1. Comment lire ce guide

Ce guide est un manuel d'atelier, pas une brochure. Chaque outil est traité avec la même grille :

1. **Ce que c'est** (éditeur, positionnement, version actuelle au 27/09/2026)
2. **Comment y accéder** (web, API officielle, API tierces, local)
3. **Combien ça coûte** (prix vérifiés le 27/09/2026 ou « à vérifier »)
4. **Licence et droits d'usage commercial** (le point que tout le monde oublie)
5. **Limites honnêtes** (ce que l'outil ne fait pas bien)
6. **Exemple concret** (prompt ou appel API copiable)

Conventions utilisées dans tout le document :

- **« vérifié le 27/09/2026 »** : information confirmée par au moins une source web consultée ce jour.
- **« à vérifier »** : information plausible mais non confirmée par une source fiable au 27/09/2026.
- **« RUMEUR non confirmée »** : bruit de couloir, aucune annonce officielle.
- Les prix sont en USD sauf mention contraire, hors taxes, et **changent vite** : toujours re-vérifier la page tarifaire officielle avant d'engager un budget.
- **Aucune clé API réelle** n'apparaît dans ce guide : les exemples utilisent `VOTRE_CLE_API` ou des variables d'environnement.

## 2. Carte du territoire

Le paysage fin septembre 2026 se résume en trois blocs :

| Bloc | Leaders (sept 2026) | Accès local possible ? |
|------|---------------------|------------------------|
| Image | Midjourney V8.2, GPT Image 2, Imagen 4 / Nano Banana Pro, FLUX.2, Ideogram 4.0, Adobe Firefly | Oui : FLUX (poids ouverts), SD 3.5, Qwen-Image, Z-Image |
| Vidéo | Kling 3.0, Veo 3.1, Runway Gen-4.5, Luma Ray 3.2, Hailuo H3, Seedance 2.5 | Partiel : Wan 2.x, LTX 2.3 (poids ouverts), MiniMax H3 (licence restrictive) |
| Recherche IA | Perplexity (+ API Sonar), Phind, You.com, Kimi, Copilot | Non (services cloud) |

Le point structurant de la période : **l'API vidéo Sora 2 d'OpenAI a été fermée le 24/09/2026** (annonce le 24/03/2026, appli fermée le 26/04/2026, sans remplaçant annoncé). Leçon à retenir pour tout le guide : ne jamais bâtir un pipeline de production sur une API sans plan B. Voir section 210.

## 3. Renvois vers les volumes IA existants

Pour éviter les doublons avec les trois volumes déjà livrés :

- **Modèles de langage chinois** (Qwen, DeepSeek, Kimi/GLM...) → voir `ia_modeles_chine.md`. Ce guide n'y revient que pour **Kimi** en tant qu'outil de recherche (section 104).
- **Modèles occidentaux** (GPT, Claude, Gemini, Llama...) → voir `ia_modeles_occident.md`. Ce guide n'y revient que pour leurs **capacités image/vidéo/recherche** (GPT Image, Veo, Gemini image).
- **Modèles spécialisés** → voir `ia_modeles_specialises.md`.
- **MiniMax H3** : licence territoriale restrictive signalée dans nos recherches du 27/09/2026 (voir volume Chine) — rappel section 96.

## 4. Le vocabulaire minimal avant de commencer

- **Text-to-image (T2I)** : image générée depuis un texte.
- **Image-to-image / image-to-video (I2I / I2V)** : on part d'une image existante qu'on transforme ou qu'on anime.
- **Text-to-video (T2V)** : vidéo générée depuis un texte.
- **Inpainting** : régénérer une zone masquée d'une image. **Outpainting** : étendre l'image au-delà de ses bords.
- **Upscale** : augmenter la résolution après génération (souvent facturé à part).
- **Poids ouverts (open weights)** : les fichiers du modèle sont téléchargeables. ≠ open source : la licence peut interdire l'usage commercial (cas de FLUX Dev).
- **Crédit** : unité de facturation interne d'une plateforme. Un crédit n'a pas de valeur universelle : toujours convertir en USD par génération.

## 5. La règle d'or du budget : coût par livrable accepté

Ne compare jamais « prix par image » ou « prix par seconde » entre outils. Compare le **coût par livrable accepté** :

```
coût_par_livrable = (dépenses_totales_génération) / (livrables_validés)
```

Les dépenses totales incluent : les essais ratés, les re-générations, l'upscale, le montage, et le temps humain de tri. Un outil à 0,03 $/image dont tu jettes 9 images sur 10 coûte plus cher qu'un outil à 0,13 $/image dont tu gardes 1 sur 2. Tiens un compteur simple (tableur ou log d'API) pendant tes deux premières semaines d'usage : c'est le seul chiffre qui compte pour décider.

---

# PARTIE A — KLING (Kuaishou) : la vidéo

## 6. Kling : ce que c'est

**Kling AI** est le générateur vidéo de **Kuaishou Technology** (géant chinois de la vidéo courte, concurrent de TikTok/Douyin). Lancé en accès restreint en juin 2024 (plus d'un million de demandes, 300 000 accès initiaux), ouvert à tous ensuite, il s'est imposé comme l'une des références mondiales de la vidéo générative. Revendications de l'éditeur : 60 millions de créateurs, 30 000 clients entreprise (vérifié le 27/09/2026 via la presse spécialisée — chiffres éditeur, à prendre avec recul).

Positionnement : **le meilleur rapport qualité/prix du marché pour la vidéo IA commerciale**, avec l'audio natif et la 4K native comme différenciateurs. Disponible dans le monde entier, interface en anglais (et chinois).

## 7. Kling : versions au 27/09/2026

La numérotation a beaucoup bougé ; état vérifié le 27/09/2026 :

| Version | Statut | Notes |
|---------|--------|-------|
| **Kling 3.0** | **Version phare actuelle** (sortie février 2026) | 4K native, audio natif, transfert de mouvement, multi-prompt |
| Kling 2.6 / 2.5 | Versions précédentes, encore proposées (mode Turbo économe) | Moins chères, qualité légèrement inférieure |
| **Kling O1** | Endpoint séparé « identité » | Travail sur références : verrouiller un personnage/produit |
| **Kling O3** | Endpoint « édition » | Reference-to-video, édition vidéo |
| Kling Image 3.0 / Omni | Génération d'image (moteur « Kolors ») | 1 crédit/image en text-to-image |

Règle pratique : pour un nouveau projet, pars sur **Kling 3.0** sauf contrainte budgétaire forte (auquel cas 2.5/2.6 Turbo en brouillon, puis 3.0 pour le master).

## 8. Kling 3.0 : fonctionnalités clés

- **Text-to-video et image-to-video** : 3 à 15 secondes par génération.
- **Résolution jusqu'à 4K native** (le plafond le plus haut du marché grand public en sept 2026).
- **Audio natif** : génération du son en même temps que l'image, avec **lip-sync multilingue** et dialogues multi-personnages. L'audio coûte environ +50 % sur le tarif API (voir section 11).
- **Motion transfer** (transfert de mouvement) : tu fournis une vidéo de référence, Kling extrait le motif de mouvement et l'applique à un autre sujet. Différenciateur unique début 2026, toujours distinctif en septembre.
- **Multi-prompt** : jusqu'à 15 secondes découpées en segments temporisés, chacun avec son propre prompt (512 caractères max par segment), rendus en un seul plan continu. Idéal pour les mini-scénarios.
- **Start frame + end frame** : tu imposes la première et la dernière image ; Kling 3.0 remplit l'intervalle. (Pas d'images de référence intermédiaires sur 3.0 : pour le verrouillage d'identité, passer par Kling O1.)
- **AI Director** : planification automatique de l'enchaînement des plans.
- **Rendu de texte à l'écran** : le meilleur du marché pour garder lisibles pancartes, logos, étiquettes de prix dans la vidéo — point décisif pour la vidéo produit/e-commerce.
- **Cohérence du sujet** : jusqu'à 4 images de référence pour verrouiller un personnage (selon le mode/endpoint).

Limites : Kling 3.0 prend **plus de libertés créatives** que certains concurrents (Seedance 2.x) : moins fiable quand le brief exige un mouvement précis et prédéterminé. Files d'attente > 30 min aux heures de pointe sur l'interface web (l'API priorise mieux).

## 9. Kling : prise en main (interface web)

1. Aller sur `klingai.com`, créer un compte (e-mail ou Google).
2. Le plan **gratuit** donne **66 crédits/jour** (sans report), sorties **watermarkées**, **usage non commercial uniquement**.
3. Choisir le mode : Text-to-Video ou Image-to-Video.
4. Régler : modèle (3.0 / 2.6...), mode (Standard / Professional), durée (5 ou 10 s, extensible), résolution (720p / 1080p / 4K), audio natif ou non.
5. Écrire le prompt (voir sections 113-122 pour la méthode), lancer, attendre, télécharger.

Le plan gratuit suffit pour évaluer la qualité, pas pour produire : watermark + interdiction d'usage commercial (voir section 10).

## 10. Kling : offres et prix web (vérifiés le 27/09/2026)

| Plan | Prix mensuel | Prix « remisé » 1er abonnement | Crédits/mois | Notes |
|------|--------------|-------------------------------|--------------|-------|
| Free | 0 $ | — | 66/jour | Watermark, **non commercial** |
| Standard | 10 $ | **6,99 $** | 660 | Sans watermark, modèles standard |
| Pro | 37 $ | **25,99 $** | 3 000 | File prioritaire, tous modèles |
| Premier | 92 $ | **64,99 $** | 8 000 | Mode professionnel |
| Ultra | 180 $ | **127,99 $** | 26 000 | Priorité max |

- Facturation annuelle : **-34 %** environ sur Standard/Pro/Premier. Ultra : pas d'option annuelle (vérifié mi-2026).
- **Les crédits payants expirent en fin de période, sans report.** C'est le piège n°1 : ne surdimensionne pas ton plan.
- Coûts de génération constatés (interface web) :
  - 5 s : **10 crédits** (Standard) / **35 crédits** (Professional)
  - 10 s : **20 crédits** (Standard) / **70 crédits** (Professional)
  - Avec audio natif : coût multiplié par ~2,5 à 5 selon version (ex. 5 s : 50 crédits Standard / 100 crédits Pro sur la grille v2.6 ; sur 3.0 compter **6 à 12 crédits/seconde** en résolution standard, **30 crédits/seconde** en 4K native).
- Génération d'image (Kolors) : 1 crédit/image en text-to-image, 2 en restyle, 5 en essayage virtuel.

**Droit d'usage commercial :** tous les plans payants l'autorisent. Le plan gratuit l'interdit explicitement (mention sur la page tarifaire officielle).

## 11. Kling : l'API développeur (oui, elle existe)

Accès : portail développeur **`kling.ai/dev`**. Points essentiels, vérifiés le 27/09/2026 :

- **Facturation totalement séparée de l'abonnement web.** Un plan Pro web ne donne aucun crédit API. Les packs API ne servent pas sur l'interface web.
- **Packs prépayés** (pas d'abonnement API, pas de facturation à l'usage pur) :

| Pack | Prix | Unités | Validité |
|------|------|--------|----------|
| Trial (petit) | 9,80 $ | 100 | 30 jours |
| Trial (grand) | 98 $ | 1 000 | 30 jours |
| Standard 1 | 700 $ | 5 000 | 180 jours |
| Standard 2 | 2 100 $ | 15 000 | 180 jours |
| Standard 3 | 4 200 $ | 30 000 | 180 jours |
| Large | 7 560 $ | 60 000 | 180 jours |

- **Sans report, sans extension** à l'expiration. Un pack de 700 $ non consommé en 180 jours = 700 $ perdus. Pour un usage ponctuel, le pack Trial à 9,80 $ est le seul rationnel.
- Tarif facial : **0,14 $/unité**. Tarifs par seconde constatés (sept 2026, page développeur) :
  - Kling 3.0, 720p, sans audio : **0,084 $/s** ; avec audio : **0,126 $/s**
  - Kling 3.0, 1080p, sans audio : **0,112 $/s** ; avec audio : **0,168 $/s**
  - Kling 3.0 Turbo + audio : 0,112 $/s (720p) / 0,140 $/s (1080p)
- Fonctionnement : **asynchrone**. Tu POSTes la demande → tu reçois un `job_id` → tu interroges (polling) jusqu'à `completed` (compter ~2 min par clip en pratique). Les **callbacks/webhooks** sont supportés : préfère-les au polling en production (moins d'appels, même coût, meilleure hygiène).
- **Accès tiers** : Kling 3.0 est aussi exposé via des agrégateurs (**fal.ai**, Replicate, Krea...) avec leur propre facturation (ex. fal : ~0,112 $/s audio off, 0,168 $/s audio on, 0,196 $/s avec contrôle vocal — tarifs fal, pas tarifs Kling officiels). Intérêt : un seul compte/clé pour plusieurs modèles, facturation à l'usage sans pack prépayé. Inconvénient : marge de l'agrégateur, conditions propres.

## 12. Kling : exemple d'appel API (schéma)

```python
import os, time, requests

API_KEY = os.environ["KLING_API_KEY"]          # jamais en dur, jamais commitée
BASE = "https://api.klingai.com"               # hôte illustratif : voir la doc dev officielle

headers = {"Authorization": f"Bearer {API_KEY}", "Content-Type": "application/json"}

# 1. Lancer une génération text-to-video (Kling 3.0, 10 s, 1080p, sans audio)
job = requests.post(f"{BASE}/v1/videos/text-to-video", headers=headers, json={
    "model": "kling-3.0",
    "prompt": "travelling avant lent dans une salle serveurs, rangées de baies "
              "19 pouces avec LEDs bleues, sol technique gris, lumière froide, "
              "ambiance data center professionnel, photoréaliste",
    "duration": 10,
    "resolution": "1080p",
    "audio": False,
}).json()
job_id = job["job_id"]

# 2. Polling (en prod : préférer le webhook / callback)
while True:
    st = requests.get(f"{BASE}/v1/videos/{job_id}", headers=headers).json()
    if st["status"] == "completed":
        print("Vidéo prête :", st["video_url"])
        break
    if st["status"] == "failed":
        raise RuntimeError(f"Échec génération : {st.get('error')}")
    time.sleep(10)
```

> Note d'honnêteté : les chemins d'endpoint exacts évoluent ; **la doc officielle `kling.ai/dev` fait foi**. Le schéma ci-dessus (job asynchrone + polling) est le pattern stable de toutes les API vidéo.

## 13. Kling : prompts vidéo efficaces — méthode

Un bon prompt vidéo = **sujet + action + caméra + lumière + ambiance + style**, dans cet ordre. Exemple décortiqué :

> « [sujet] un technicien en tenue de travail bleu marine [action] remplace un module d'onduleur dans une armoire électrique ouverte [caméra] plan moyen, léger travelling latéral gauche-droite [lumière] lumière d'atelier froide, reflets métalliques [ambiance] calme professionnel, gestes précis [style] photoréaliste, 24 i/s, netteté documentaire »

Règles :
- **Une action principale par clip** (5-10 s). Deux actions = deux clips.
- Décrire le **mouvement de caméra** explicitement (travelling, panoramique, zoom, plan fixe, drone).
- L'audio natif : décrire le son souhaité (« Audio : ronronnement grave de ventilation, clics métalliques ») — Kling 3.0 le génère avec l'image.
- Le **multi-prompt** pour les mini-scénarios : un prompt par segment temporel, avec continuité du sujet (« même technicien, même armoire »).
- Ce que Kling fait mal : texte long à l'écran (au-delà de quelques mots), gestes ultra-précis (mains), chorégraphies exactes.

## 14. Kling : cas d'usage pro (angle Zelef)

1. **Vidéo de présentation d'équipement** (onduleur, baie, groupe électrogène) : image-to-video depuis une photo réelle du matériel + prompt de caméra lente + audio natif (ronronnement). Usage : formation interne, page intranet, réponse à appel d'offres.
2. **Clip de sensibilisation sécurité** : scénario multi-prompt (ex. consignation électrique en 3 plans de 5 s). Bien moins cher qu'un tournage.
3. **Visite virtuelle de site** : enchaîner 4-6 clips image-to-video depuis des photos du local technique → montage simple → visite guidée pour un client qui ne se déplace pas.
4. **Contenu formation** : animer des schémas (plan unifilaire, synoptique) pour expliquer un cheminement électrique. Attention : le texte dans l'image source peut se déformer à l'animation — tester d'abord.

## 15. Kling : limites honnêtes

- **Files d'attente** sur le web aux heures de pointe (jusqu'à 30+ min constatées). L'API est plus prévisible.
- **Générations ratées facturées** : un clip raté consomme les crédits quand même, sur le web comme (selon l'agrégateur) via API. Budgéter 2 à 3 essais par clip final.
- **Liberté créative** : Kling interprète plus qu'il n'exécute au pied de la lettre. Pour un mouvement imposé, Seedance ou le motion transfer sont meilleurs.
- **Personnes** : visages et mains perfectibles, surtout en 720p. Pour un visage récurrent, passer par l'endpoint O1 (références).
- **Données** : tout ce que tu uploades (photos d'installations, plans) transite par les serveurs de Kuaishou. Pas d'option on-prem. Pour du confidentiel : flouter/anonymiser avant envoi, ou ne pas utiliser.
- **Pas de contrôle fin au photogramme** : ce n'est pas un logiciel de montage. Le montage final se fait ailleurs (DaVinci, Premiere...).

---

# PARTIE B — FLUX (Black Forest Labs) : l'image

## 16. FLUX : ce que c'est

**FLUX** est la famille de modèles d'image de **Black Forest Labs (BFL)**, startup allemande fondée par d'anciens de l'équipe Stable Diffusion. Positionnement : **le meilleur compromis du marché entre qualité, coût et déploiement local**. Particularité stratégique : BFL publie des **poids ouverts** pour une partie de la gamme, ce qui en fait le seul acteur « flagship » qu'on peut faire tourner **on-premise / air-gap** — argument décisif pour du travail confidentiel (plans, installations client).

Architecture (rappel technique) : transformeur de diffusion à flux rectifié (rectified flow), ~12 Md de paramètres pour FLUX.1, **32 Md** pour FLUX.2 Dev (associé à un modèle vision-langage Mistral-3), double encodeur texte T5 + CLIP sur la génération 1.

## 17. FLUX : la famille au 27/09/2026

### FLUX.2 (génération actuelle, sortie novembre 2025)

| Variante | Positionnement | Licence / accès |
|----------|---------------|-----------------|
| **FLUX.2 [pro]** | Meilleure qualité, hébergé | API BFL + partenaires |
| **FLUX.2 [max]** | Qualité maximale, le plus cher | API BFL |
| **FLUX.2 [flex]** | Paramètres réglables (steps, guidance) : compromis vitesse/qualité | API BFL + Playground |
| **FLUX.2 [dev]** | **Poids ouverts téléchargeables** (32 Md), qualité proche du pro | **Non-commercial** ; licence commerciale à demander à BFL |
| **FLUX.2 [klein] 4B** | Petit, rapide (~1 s sur RTX 5090), **Apache 2.0** | **Usage commercial autorisé**, déploiement on-prem |
| **FLUX.2 [klein] 9B** | Meilleur ratio qualité/latence | Poids ouverts, **non-commercial** |

Le VAE (auto-encodeur variationnel) de FLUX.2 est publié sous **Apache 2.0** : il définit l'espace latent commun, ce qui permet l'interopérabilité entre tes pipelines internes et les API BFL.

Points forts de FLUX.2 : **multi-références** (jusqu'à ~6-10 images de référence pour la cohérence d'un personnage/objet sans fine-tuning), **rendu de texte** très amélioré (infographies, UI, multilingue), contrôle de pose direct, sortie jusqu'à **4 mégapixels**.

### FLUX.1 (génération précédente, toujours pertinente)

| Variante | Positionnement | Licence |
|----------|---------------|---------|
| FLUX.1 [pro] / 1.1 | API, haute qualité | Propriétaire (API) |
| FLUX.1 [pro] Ultra | 4 MP (2048×2048+) | API |
| **FLUX.1 [dev]** | Poids ouverts 12 Md, 20-50 steps | **Non-commercial** |
| **FLUX.1 [schnell]** | 1-4 steps, ultra-rapide | **Apache 2.0** (commercial OK) |
| FLUX.1 [fill] | Inpainting/outpainting | API (+ variantes ouvertes communautaires) |
| FLUX.1 [kontext] (Max/Pro/Dev) | **Édition d'image sans masque** par instruction texte | Dev : poids ouverts 12 Md non-commerciaux |
| FLUX.1 [canny]/[depth], [redux] | ControlNet, variations | Écosystème ouvert |

**FLUX.1 Kontext** mérite une mention spéciale : c'est l'éditeur « sans masque » le plus abouti — tu décris la modification en langage naturel (« remplace le câble rouge par un câble bleu ») et le modèle édite en conservant le reste, avec une excellente **cohérence de personnage** entre scènes sans ré-entraînement.

## 18. FLUX : les licences, sans détour

C'est le point le plus mal compris de l'écosystème. Tableau de vérité :

| Variante | Poids téléchargeables ? | Usage commercial ? |
|----------|------------------------|--------------------|
| FLUX.2 [klein] **4B** | Oui | **Oui (Apache 2.0)** |
| FLUX.1 [schnell] | Oui | **Oui (Apache 2.0)** |
| FLUX.2 VAE | Oui | **Oui (Apache 2.0)** |
| FLUX.2 [dev] 32B | Oui | **Non — licence commerciale à obtenir auprès de BFL** |
| FLUX.2 [klein] 9B | Oui | **Non (non-commercial)** |
| FLUX.1 [dev] 12B | Oui | **Non (non-commercial)** |
| FLUX.1 [kontext-dev] | Oui | **Non (non-commercial)** |
| Versions [pro]/[max]/[flex] | Non (API uniquement) | Oui (via l'API, selon les CGU) |

**Piège classique** : générer avec FLUX.2 [dev] en local « parce que c'est gratuit » puis utiliser les visuels dans une doc client ou un appel d'offres → **violation de licence**. Pour un usage pro local : **klein 4B (Apache 2.0)** ou licence commerciale BFL pour dev. Pour un usage pro via API : les CGU de l'API couvrent l'usage commercial.

## 19. FLUX : l'API officielle BFL

- Endpoints : `https://api.bfl.ai` (global, bascule auto), `https://api.eu.bfl.ai` (**résidence UE — à privilégier pour des données européennes / RGPD**), `https://api.us.bfl.ai` (résidence US).
- Auth : clé API (`x-key`), compte en self-service sur `dashboard.bfl.ai`, paiement Stripe, **aucune entité juridique requise**.
- **Modèle d'appel asynchrone** : POST → `id` de tâche → GET `/v1/get_result` jusqu'au résultat ; **webhooks** supportés pour la prod.
- Facturation : **1 crédit = 0,01 $**, au **mégapixel** (entrée + sortie comptent) pour FLUX.2 ; au forfait par image pour FLUX.1.

Tarifs API BFL vérifiés le 27/09/2026 (1er mégapixel / mégapixels suivants) :

| Modèle | Endpoint | 1er MP | +MP | ~1 MP T2I |
|--------|----------|--------|-----|-----------|
| FLUX.2 [klein] 4B | `/v1/flux-2-klein-4b` | 1,4 ¢ | 0,1 ¢ | **0,014 $** |
| FLUX.2 [klein] 9B | `/v1/flux-2-klein-9b` | 1,5 ¢ | 0,2 ¢ | **0,015 $** |
| FLUX.2 [pro] | `/v1/flux-2-pro` | 3 ¢ | 1,5 ¢ | **0,030 $** |
| FLUX.2 [flex] | `/v1/flux-2-flex` | 5 ¢ | 5 ¢ | **0,050 $** |
| FLUX.2 [max] | `/v1/flux-2-max` | 7 ¢ | 3 ¢ | **0,070 $** |

FLUX.1 (forfait) : 1.1 [pro] **0,04 $**, Ultra **0,06 $**, Kontext [pro] **0,04 $**, Kontext [max] **0,08 $**, Fill [pro] **0,05 $**.

Ordre de grandeur : une image 1024×1024 en FLUX.2 [pro] = **3 centimes**. C'est ~4× moins cher que Nano Banana Pro à qualité comparable sur beaucoup d'usages (voir section 78). Pour du volume (centaines d'images de doc), l'écart est massif.

Exemple d'appel (schéma officiel BFL) :

```python
import os, time, requests

BFL_KEY = os.environ["BFL_API_KEY"]
EU = "https://api.eu.bfl.ai"   # résidence UE : privilégier pour données européennes

# 1. Soumettre la génération (FLUX.2 Pro, ~1 MP)
r = requests.post(f"{EU}/v1/flux-2-pro",
    headers={"x-key": BFL_KEY, "Content-Type": "application/json"},
    json={"prompt": "photographie documentaire d'une armoire électrique industrielle "
                    "ouverte, borniers et disjoncteurs modulaires alignés, "
                    "étiquettes lisibles, lumière d'atelier neutre, "
                    "style photo technique, très net",
          "width": 1024, "height": 1024}).json()
task_id = r["id"]

# 2. Attendre le résultat (en prod : webhook)
while True:
    res = requests.get(f"{EU}/v1/get_result",
                       headers={"x-key": BFL_KEY},
                       params={"id": task_id}).json()
    if res["status"] == "Ready":
        print("Image :", res["result"]["sample"])   # URL temporaire de l'image
        break
    time.sleep(2)
```

## 20. FLUX via API tierces : Replicate, fal.ai et les autres

BFL n'est pas le seul guichet. FLUX est exposé chez **Replicate, fal.ai, Together AI, Fireworks AI, Runware, DeepInfra, Cloudflare**, avec leur propre grille (marge variable). Intérêts :

- **Un seul compte, une seule clé, une seule facture** pour FLUX + Kling + Veo + autres (fal.ai et Replicate hébergent des dizaines de modèles).
- Facturation **à l'usage pur**, sans crédit BFL à pré-charger.
- Exemples de tarifs constatés (sept 2026, **tarifs agrégateurs, pas BFL**) : FLUX [schnell] ~**0,003 $/image** sur fal.ai (idéal pour les brouillons jetables), Nano Banana ~0,04 $, Nano Banana Pro ~0,15-0,20 $.

Quand choisir quoi :
- **Volume régulier, un seul modèle** → API officielle BFL (moins cher, relation directe, endpoint UE).
- **Multi-modèles, prototypes, volumes irréguliers** → fal.ai / Replicate (simplicité > prix unitaire).
- Toujours comparer le **coût total à volume égal** avant de t'engager, et noter le fournisseur dans ton suivi (les prix bougent indépendamment).

## 21. FLUX en local : pourquoi c'est un cas à part

C'est **le** différenciateur stratégique de FLUX pour un sysadmin :

- **Confidentialité totale** : plans, photos d'installations clients, schémas réseau ne quittent jamais ta machine. Le seul flagship 2026 qui le permet avec des poids ouverts.
- **Coût marginal nul** après l'investissement GPU : des milliers d'images sans compteur.
- **Reproductibilité** : seed fixe, version de modèle figée, pas de « mise à jour silencieuse » côté fournisseur.
- **Air-gap** : possible (télécharger les poids une fois, travailler déconnecté).

Inconvénients : investissement GPU, consommation électrique, maintenance (drivers, CUDA), vitesse inférieure au cloud, et **licence** (voir section 18 : klein 4B ou schnell en Apache 2.0 pour du commercial).

## 22. FLUX en local : prérequis matériels (vérifiés sept 2026)

| Modèle | VRAM min (pratique) | Config type | Disque |
|--------|---------------------|-------------|--------|
| FLUX.2 [dev] 32B (FP8) | **24 Go** | RTX 3090 / 4090 / 5090 | ~75 Go |
| FLUX.2 [dev] 32B (BF16) | ~44-64 Go | RTX 6000 Ada / pro | ~90 Go |
| FLUX.2 [dev] (GGUF Q4_K_S) | ~19-24 Go | RTX 4090 | ~40 Go |
| FLUX.2 [klein] 9B (FP8) | **16 Go** | RTX 4080+ | ~30 Go |
| FLUX.2 [klein] 4B | **12 Go** | RTX 4070 / 3060 12 Go | ~15 Go |
| FLUX.1 [dev]/[schnell] 12B (FP8) | **16 Go** | RTX 4090 confortable | ~30 Go |

La quantification **FP8** (optimisée avec NVIDIA pour FLUX.2) divise la VRAM par ~2 avec une perte de qualité faible : c'est le réglage standard en 2026. Sans GPU correct, la location d'un GPU cloud (RunPod, etc.) reste l'option réaliste — pas de chemin CPU viable pour un modèle 32B.

## 23. FLUX en local via ComfyUI : installation pas à pas

**ComfyUI** est l'orchestrateur de workflows de génération d'image le plus utilisé (interface par nœuds, reproductible, scriptable via API). Procédure pour FLUX.2 [dev] en FP8 (sept 2026) :

```bash
# 1. Installer ComfyUI (portable)
git clone https://github.com/comfyanonymous/ComfyUI.git
cd ComfyUI
python -m venv venv && source venv/bin/activate
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu128
pip install -r requirements.txt

# 2. Récupérer les poids (ex. ~71 Go au total en FP8/BF16 mixte)
pip install -U "huggingface_hub[cli]"
hf download Comfy-Org/flux2-dev split_files/diffusion_models/flux2_dev_fp8mixed.safetensors \
  --local-dir models/diffusion_models        # ~35,5 Go : le modèle (UNET)
hf download Comfy-Org/flux2-dev split_files/text_encoders/mistral_3_small_flux2_fp8.safetensors \
  --local-dir models/text_encoders           # version FP8 du CLIP : moitié moins lourde
hf download black-forest-labs/FLUX.2-small-decoder full_encoder_small_decoder.safetensors \
  --local-dir models/vae                     # ~1 Go : le VAE

# 3. Lancer
python main.py --listen 127.0.0.1 --port 8188
# Interface : http://127.0.0.1:8188
```

Dans l'interface : charger le **template officiel FLUX.2** (menu Templates de ComfyUI), vérifier que les trois chargeurs pointent vers les bons fichiers, régler **20-30 steps** pour les tests (jusqu'à 50 pour le master), sampler `euler`, guidance ~3,5, résolution 1024×1024 pour commencer (monter prudemment : la VRAM explose au-delà).

**Astuce VRAM** : le nœud « purge VRAM » entre les grosses générations, et le weight streaming (déchargement partiel vers la RAM système) activé par les builds récents rendent FLUX.2 utilisable même quand ça coince.

## 24. FLUX en local : premier workflow et variante légère

Workflow minimal text-to-image dans ComfyUI (nœuds) :

1. `Load Diffusion Model` → `flux2_dev_fp8mixed.safetensors`
2. `Load CLIP` → `mistral_3_small_flux2_fp8.safetensors`
3. `Load VAE` → le VAE FLUX.2
4. `CLIP Text Encode (Prompt)` → ton prompt (positif) ; un second pour le négatif (`floou, déformé, filigrane, texte illisible`)
5. `Empty Latent Image` → 1024×1024
6. `KSampler` → steps 25, cfg 3.5, sampler `euler`, scheduler `simple`, seed fixe (ex. 42) pour reproductibilité
7. `VAE Decode` → `Save Image`

**Variante légère (klein 4B, Apache 2.0, usage commercial OK)** : même workflow, modèle 4B, ~8-12 Go de VRAM, génération en ~1 s sur RTX 5090. Parfait pour : itérer sur les compositions, faire tourner un service interne de visuels, ou équiper un poste modeste. La qualité est inférieure à dev/pro mais largement suffisante pour des schémas, illustrations de doc et maquettes.

## 25. FLUX via Diffusers (Python, sans ComfyUI)

Pour intégrer la génération dans un script ou un pipeline (ex. générer les visuels d'une doc automatiquement) :

```python
import torch
from diffusers import FluxPipeline   # FLUX.1 ; voir la doc diffusers pour FLUX.2

pipe = FluxPipeline.from_pretrained(
    "black-forest-labs/FLUX.1-schnell",   # Apache 2.0 : commercial OK
    torch_dtype=torch.bfloat16,
).to("cuda")

image = pipe(
    "schéma isométrique d'un local technique avec deux onduleurs 40 kVA, "
    "armoire électrique et climatisation de précision, style illustration "
    "technique épurée, fond clair",
    height=1024, width=1024,
    num_inference_steps=4,                 # schnell : 1 à 4 steps suffisent
    guidance_scale=0.0,
    generator=torch.Generator("cuda").manual_seed(7),
).images[0]
image.save("local_technique.png")
```

Points d'attention : `schnell` est ultra-rapide mais moins fidèle que `dev` ; pour FLUX.2 via diffusers, vérifier la classe de pipeline supportée dans la version installée de `diffusers` (le support suit les releases BFL avec quelques semaines de décalage — **à vérifier** au moment où tu lis ceci).

## 26. FLUX : cas d'usage pro (angle Zelef)

1. **Illustrations de documentation technique** : schémas de principe (synoptique onduleur, cheminement câbles), illustrations pour procédures. Générer en local (klein 4B / schnell) = 0 coût marginal, confidentialité totale.
2. **Photos d'ambiance pour supports** : salle serveurs, atelier, armoire électrique — quand tu n'as pas le droit de photographier un site client ou que la photo réelle est inexploitable.
3. **Maquettes avant intervention** : visualiser un réaménagement de local technique (nouvelle baie, nouvel onduleur) avec Kontext en édition d'une photo réelle : « ajoute une deuxième armoire à droite ».
4. **Cohérence de personnage/mascotte** : multi-références FLUX.2 pour garder le même technicien/équipement sur une série de visuels de formation.
5. **Schémas avec texte** : FLUX.2 et Ideogram sont les meilleurs pour le texte dans l'image — utile pour des étiquettes, mais **toujours relire** : une étiquette « 400 V » devenue « 4000 V » dans une doc est un incident.

## 27. FLUX : limites honnêtes

- **Le photoréalisme « parfait »** : Nano Banana Pro et Imagen 4 Ultra restent devant pour la peau, les portraits, la photo produit haut de gamme (tests indépendants juin 2026).
- **Petite typographie** : les longs passages de texte finissent par baver ; pour une affiche avec beaucoup de texte, composer le texte **après** en PAO, pas dans le prompt.
- **Le « look propre » par défaut** : FLUX sort des images très nettes, parfois trop lisses (« AI sheen »). Pour un grain documentaire, le post-traitement ou un prompt explicite (« grain photographique, lumière naturelle imparfaite ») aident.
- **Vitesse en local** : 1-2 min/image en 1024² sur RTX 4090 (FLUX.2 dev, 20 steps). Pour du volume, le cloud ou klein sont préférables.
- **Édition conversationnelle** : Kontext est excellent en édition par instruction, mais le pattern « chat multi-tours » de Nano Banana est plus naturel pour itérer en dialogue.

---

# PARTIE C — PERPLEXITY : la recherche IA

## 28. Perplexity : ce que c'est (« plexplxity » = coquille)

**Perplexity AI** est un **moteur de réponse** (answer engine) : tu poses une question en langage naturel, il cherche sur le web en temps réel et te répond avec **des citations sources cliquables**. La différence avec un chatbot classique : chaque affirmation importante est ancrée à une source que tu peux vérifier. Pour un métier où une valeur fausse coûte cher (dimensionnement, norme, procédure), c'est le point décisif.

État au 27/09/2026 : produit mature, modèle économique **100 % abonnement/API** — la régie publicitaire lancée en octobre 2025 a été **arrêtée en février 2026** (confiance utilisateurs et précision des réponses). Perplexity a aussi signé un partenariat compute avec Microsoft Azure (annoncé janvier 2026, ~750 M$ selon la presse).

## 29. Perplexity : offres et prix (vérifiés le 27/09/2026)

| Offre | Prix | Contenu essentiel |
|-------|------|-------------------|
| **Free** | 0 $ | Recherches basiques illimitées + ~5 recherches Pro/jour, citations |
| **Pro** | **20 $/mois** (200 $/an, ~16,67 $/mois) | Recherches Pro illimitées, ~20 Deep Research/jour (jusqu'à 500/jour selon sources), choix du modèle (GPT-5.x, Claude, Gemini 3.x, Grok, Sonar), upload de fichiers, Spaces, génération d'image/vidéo, **5 $/mois de crédit API inclus** |
| **Max** | **200 $/mois** (2 000 $/an) | Tout Pro + Labs illimités, Deep Research illimité, Perplexity Computer (10 000 crédits/mois), vidéo cinématique, accès prioritaire aux nouveautés |
| **Education Pro** | 10 $/mois | Pro pour étudiants vérifiés |
| **Enterprise Pro** | 40 $/siège/mois (400 $/an) | Admin, SSO, Spaces partagés, sécurité renforcée |
| **Enterprise Max** | 325 $/siège/mois | Limites maximales |
| **Comet** (navigateur) | **Gratuit** depuis octobre 2025 | Navigateur avec assistant intégré (Windows, Mac, Android, iOS) |
| **Comet Plus** | 5 $/mois | Contenus éditeurs premium |

Fonctionnalités notables : **Spaces** (espaces de travail collaboratifs avec tâches planifiées qui rafraîchissent une réponse périodiquement — parfait pour une veille), **Labs** (génère tableurs, rapports, mini-apps), **Model Council** (pose la même question à plusieurs modèles frontières et compare), **Deep Research** (recherche approfondie multi-sources).

## 30. L'API Sonar : oui, elle existe (et c'est le vrai sujet pro)

L'**API Sonar** expose la recherche web temps réel + génération avec citations dans tes propres outils. Caractéristiques vérifiées le 27/09/2026 :

- **Endpoint** : `https://api.perplexity.ai` — **compatible OpenAI** (tu peux réutiliser tes clients/SDK existants en changeant l'URL de base et la clé).
- **Modèles** : `sonar` (léger/rapide), `sonar-pro` (recherche approfondie), `sonar-reasoning`, `sonar-deep-research`, plus une **Search API** (résultats web bruts, sans génération).
- **Pas d'offre gratuite permanente** côté API (des crédits d'essai ~25-50 $ pour les nouveaux comptes selon les sources ; les abonnés Pro reçoivent **5 $/mois** de crédit API).
- **Tarifs indicatifs** (grille publique, vérifiée sept 2026) :
  - `sonar` : **~1 $ / 1 M tokens** (entrée et sortie)
  - `sonar-pro` : **~3 $ / 1 M tokens en entrée, ~15 $ / 1 M en sortie**
  - **Frais de requête** : **5 $ à 14 $ pour 1 000 requêtes** selon le modèle (la Search API brute : 5 $/1 000 recherches)
  - Tokens de citation ~2 $/1M, tokens de raisonnement ~3-8 $/1M selon les docs tierces (**à vérifier** sur la page officielle)
- SDK officiels **Python et TypeScript**, serveur **MCP** (Cursor, VS Code, Claude Desktop), programme startups (6 mois d'Enterprise Pro + 5 000 $ de crédits API selon les sources — **à vérifier**).
- Les modèles Sonar sont **propriétaires** (fine-tunés sur des bases ouvertes type Llama 3.3 / DeepSeek R1 selon les analyses, mais poids et infra de recherche fermés).

Exemple minimal (compatible OpenAI) :

```python
import os
from openai import OpenAI   # client OpenAI standard, réutilisé tel quel

client = OpenAI(
    api_key=os.environ["PERPLEXITY_API_KEY"],
    base_url="https://api.perplexity.ai",
)

resp = client.chat.completions.create(
    model="sonar-pro",
    messages=[{
        "role": "user",
        "content": "Quelle est la tension de floating recommandée pour des batteries "
                   "VRLA AGM d'onduleur en 2026 ? Donne les valeurs par élément et "
                   "cite tes sources.",
    }],
)
print(resp.choices[0].message.content)
# La réponse inclut des citations [1], [2]... : les récupérer et les vérifier,
# surtout pour des valeurs d'ingénierie.
```

## 31. Perplexity : cas d'usage pro (angle Zelef)

1. **Veille normative et documentaire** : « évolutions NF C 15-100 / NF C 18-510 en 2026 », « nouveaux onduleurs 60 kVA comparatif » — avec sources, en 2 minutes au lieu de 30.
2. **Recherche de documentation obscure** : manuels de service, firmware, notes d'application (le prolongement naturel de ton pipeline RAG : Perplexity trouve, ton scraper archive).
3. **Préparation d'intervention** : « procédure de remplacement des condensateurs bus DC sur Easy UPS 3S » + citations → tu croises avec la doc constructeur.
4. **Veille automatisée via l'API Sonar** : un script qui interroge chaque semaine (« nouvelles failles CVE onduleurs connectés », « rappels produits Schneider/Eaton ») et t'envoie un digest. Voir le cas pratique section 163.
5. **Spaces partagés** : un Space « Veille énergie » avec tâches planifiées, partagé à l'équipe — chacun y pioche, personne ne refait la recherche.

## 32. Perplexity vs recherche classique : comparatif honnête

| Critère | Perplexity (Pro) | Google / Bing classique |
|---------|------------------|------------------------|
| Vitesse de synthèse | **Excellente** : réponse rédigée en ~10 s | Tu ouvres 5 onglets et synthétises toi-même |
| Citations | Intégrées, cliquables | Tu dois les trouver toi-même |
| Fraîcheur | Web temps réel | Web temps réel |
| Contrôle des sources | Choix du périmètre (web, académique, social) mais pas de liste blanche perso fine | Opérateurs avancés (`site:`, `-`, guillemets), total |
| Coût | 20 $/mois ou API au token | Gratuit |
| Biais de synthèse | **Le modèle peut mal résumer ou sur-interpréter une source** | Tu lis la source toi-même |
| Recherche obscure / long tail | Bonne, mais parfois « répond à côté » | Souvent meilleure avec les bons opérateurs |
| Confidentialité | Ta question part chez Perplexity (+ Microsoft Azure) | Ta question part chez Google |

**Règle d'usage** : Perplexity pour **dégrossir vite** (80 % du travail en 20 % du temps), puis **toujours ouvrir les 2-3 sources clés** avant d'utiliser une valeur dans un dimensionnement, un devis ou une procédure. Une citation n'est pas une preuve : c'est un pointeur. La preuve, c'est la source lue.

## 33. Perplexity : les pièges spécifiques

- **La synthèse confiante** : Perplexity écrit bien, avec des citations, même quand il a compris de travers. Pour les chiffres (tensions, couples, normes), **vérifier la source primaire**.
- **Sources de qualité variable** : un blog SEO peut être cité au même niveau qu'une doc constructeur. Regarder *qui* est cité, pas seulement *que* c'est cité.
- **Le contentieux éditeurs** : Perplexity est en litige avec des éditeurs de presse (News Corp, Nikkei/Asahi — procédures en cours en 2025-2026). Conséquence possible : retrait ou dégradation de certaines sources. Ne pas en dépendre comme source unique d'actualité.
- **L'API n'est pas gratuite** : le compteur tourne à chaque appel (tokens + frais de requête). Un script de veille mal calibré (boucle trop fréquente, `sonar-pro` au lieu de `sonar`) fait grimper la facture. Commencer par `sonar`, monter en gamme si besoin.
- **Données sensibles** : ne pas coller dans Perplexity (ni son API) des infos confidentielles client, des plans internes, des identifiants. C'est un service cloud américain.
- **Deep Research ≠ expertise** : excellent pour cartographier un sujet, insuffisant pour trancher un point de sécurité électrique. L'expert reste toi.

## 34. Perplexity : Space de veille — exemple concret

Un Space « Veille onduleurs & énergie » utile au quotidien :

1. Créer un Space, ajouter comme instructions : « Sources prioritaires : documentations constructeurs (Schneider Electric, Eaton, Vertiv, Riello, Socomec, Huawei), normes (UTE, IEC), presse spécialisée énergie. Signaler la date de chaque source. »
2. Ajouter une **tâche planifiée hebdomadaire** : « Nouveautés de la semaine : onduleurs triphasés 10-120 kVA, batteries Li-ion pour UPS, réglementation batteries, CVE touchant des équipements d'énergie connectés. Format : puces avec lien source et une ligne de résumé. »
3. Partager le Space à l'équipe (Enterprise) ou exporter le digest.

Coût : inclus dans Pro/Max. Via API, le même digest hebdomadaire coûte quelques dizaines de centimes en `sonar` (voir cas pratique section 163).

---

# PARTIE D — PANORAMA IMAGE : les autres acteurs

## 35. Pourquoi un panorama (et pas un seul outil)

Aucun générateur d'image n'est le meilleur partout. Le choix dépend de ce que tu optimises : **esthétique** (Midjourney), **fidélité aux instructions** (GPT Image), **photoréalisme** (Imagen/Nano Banana Pro), **coût + local** (FLUX), **texte dans l'image** (Ideogram, FLUX.2, Nano Banana Pro), **sécurité juridique** (Adobe Firefly). Le tableau comparatif est en section 43.

## 36. Midjourney : la référence esthétique

- **Modèle actuel au 27/09/2026 : V8.2** (défaut depuis le 24 juillet 2026 ; V8.1 par défaut depuis le 11 juin 2026). Images natives **2K** sans upscaling, ~5× plus rapide que V7 selon l'éditeur.
- Accès : **midjourney.com** (interface web ; Discord devenu optionnel). **Pas d'offre gratuite** (l'essai gratuit a été supprimé en mars 2023 et n'est jamais revenu).
- Fonctionnalités 2026 : **Edit Model** (édition par instruction, jusqu'à 4 références, inpainting/outpainting — en test août 2026), **personnalisation** (profil esthétique appris de tes notations), moodboards et style references, **génération vidéo** (courts clips SD/HD selon le plan), mode Draft pour itérer vite, paramètres `--p`, `--tile 2.0`, `--weird`, `--stylize`, `--hd`.
- Le point fort historique, inchangé : le **« look Midjourney »** — rendu pictural/cinématographique léché **sans effort de prompt**. Pour des visuels « beaux » rapidement, imbattable. Revers : ce style signature est reconnaissable, parfois indésirable pour du documentaire technique (préférer FLUX dans ce cas).

## 37. Midjourney : prix (vérifiés le 27/09/2026)

Facturation au **temps GPU**, pas à l'image. ~20 % de remise en annuel.

| Plan | Mensuel | Annuel (équiv./mois) | GPU rapides | Relax (illimité lent) | Stealth (privé) |
|------|---------|----------------------|-------------|----------------------|-----------------|
| Basic | 10 $ | 8 $ | 3,3 h (~200-250 images) | Non | Non |
| Standard | 30 $ | 24 $ | 15 h (~900 images) | Oui | Non |
| Pro | 60 $ | 48 $ | 30 h (~1 800 images) | Oui (+ vidéo SD) | **Oui** |
| Mega | 120 $ | 96 $ | 60 h (~3 600 images) | Oui (+ vidéo SD) | **Oui** |

- Une image standard V8 = ~0,8 min GPU ; une image HD 2048px = ~1,3 min GPU. Heure GPU supplémentaire : **4 $**.
- **Droits commerciaux** : inclus sur tous les plans ; **les entreprises à plus d'1 M$ de revenus annuels doivent être en Pro ou Mega**.
- **Pas d'API publique** (une offre Enterprise sur mesure existerait selon des sources tierces — **à vérifier** ; en pratique, pas d'intégration programmatique standard).
- Images **publiques par défaut** : sans Stealth (Pro/Mega), tes générations sont visibles par la communauté. Pour du confidentiel : à proscrire, ou passer en Pro.

## 38. OpenAI : GPT Image (DALL-E nouvelle génération)

OpenAI a fait basculer sa génération d'image vers la famille **GPT Image** (le modèle derrière le générateur de ChatGPT), DALL-E 3 restant disponible via API :

| Modèle | Prix indicatif / image (API) | Notes |
|--------|------------------------------|-------|
| **gpt-image-2** | fourchette ~0,006-0,211 $ selon taille/qualité (sources tierces, **à vérifier** sur la page officielle) | Le plus récent, édition native, tailles libres |
| gpt-image-1.5 | ~0,009-0,200 $ | Version allégée |
| gpt-image-1-mini | ~0,005-0,052 $ | Économique |
| gpt-image-1 | 0,011 $ (low) / 0,042 $ (med) / 0,167 $ (high) en 1024² | Référence 2025 |
| **DALL-E 3** | 0,040 $ (standard) / 0,080 $ (HD) | Stable, prévisible |
| DALL-E 2 | 0,016-0,020 $ | Legacy |

Points forts : **fidélité aux instructions** (le modèle comprend des consignes complexes et littérales), **édition d'image native** (inpainting au masque, fond transparent), intégration à l'écosystème OpenAI/ChatGPT (abonnés Plus/Pro : génération incluse dans l'abonnement chat). Points faibles : moins « beau » par défaut que Midjourney, moins photoréaliste que les meilleurs Google/FLUX.2, pas d'usage local.

Exemple d'appel API (endpoint images) :

```python
import os
from openai import OpenAI
client = OpenAI(api_key=os.environ["OPENAI_API_KEY"])

img = client.images.generate(
    model="gpt-image-1",          # ou "dall-e-3", "gpt-image-2" (tarifs à vérifier)
    prompt="pictogramme vectoriel minimaliste d'un onduleur triphasé, "
           "style ligne bleue sur fond blanc, sans texte",
    size="1024x1024",
    quality="medium",
)
print(img.data[0].b64_json[:60], "...")   # base64 : décoder puis sauvegarder
```

## 39. Google : Imagen 4 et Nano Banana

Google propose **deux gammes** via la Gemini API et Vertex AI (facturation au token / à l'image, **pas de palier gratuit pour l'image en API** — l'UI gratuite d'AI Studio existe avec quotas) :

| Modèle | Prix / image (API) | Positionnement |
|--------|-------------------|----------------|
| **Imagen 4 Fast** | **0,02 $** | Volume, brouillons, A/B tests |
| **Imagen 4 Standard** | **0,04 $** | Équilibre qualité/prix (80-90 % du premium) |
| **Imagen 4 Ultra** | **0,06 $** | Photorealisme max, 2K natif, zéro artefact |
| **Nano Banana** (gemini-2.5-flash-image) | **0,039 $** | Édition rapide originale |
| **Nano Banana 2** (gemini-3.1-flash-image) | 0,045-0,067 $ selon résolution | Haut du leaderboard, rapide |
| **Nano Banana Pro** (gemini-3-pro-image) | **0,134 $** (1-2K), **0,24 $** (4K) | Photorealisme extrême, contrôles pro (lumière, focale, colorimétrie) |

- **Batch API : -50 %** sur tous ces tarifs (0,01 $ l'Imagen 4 Fast en batch : imbattable pour du volume planifiable).
- Abonnements Google AI : AI Pro (~19,99 $/mois ≈ 100 images NB Pro/jour), AI Ultra (~30 $/mois ≈ 1 000/jour) — ordres de grandeur, **à vérifier**.
- Points forts : **photoréalisme** (Imagen 4 Ultra préféré à 78 % pour les portraits lors de tests indépendants juin 2026), **rendu de texte**, intégration à l'écosystème Google (même clé/facturation que Gemini texte).
- Points faibles : peaux parfois « plastiques », raisonnement spatial perfectible, **pas de cohérence de personnage native** au niveau de FLUX Kontext, pas d'usage local.

## 40. Ideogram 4.0 : le spécialiste du texte dans l'image

**Ideogram** (startup, modèle 4.0 en 2026) s'est fait un nom sur un point précis : **le texte long et lisible intégré à l'image** (affiches, visuels marketing, miniatures). Tarifs API : **~0,03-0,10 $/image** selon qualité, ou abonnement. À évaluer quand ton besoin n°1 est « une affiche avec un vrai titre lisible » — là où Midjourney et FLUX.1 peinent encore sur les longs passages.

## 41. Adobe Firefly (Image Model 5) : l'option « juridiquement sûre »

- Abonnement **9,99 $ à 199,99 $/mois** (crédits ; les modèles partenaires/premium consomment plus).
- Argument unique : modèle entraîné sur du contenu **licencié** (Adobe Stock et domaine public), intégré à **Photoshop, Illustrator, Premiere**. Pour une entreprise qui veut minimiser le risque de contentieux sur les données d'entraînement, c'est l'option la plus défendable.
- Qualité correcte mais en retrait des meilleurs sur le photoréalisme pur ; l'intérêt est l'intégration au workflow Adobe existant, pas le benchmark.

## 42. Les poids ouverts image (rappel)

Pour mémoire (détaillés pour FLUX sections 16-27) :

| Modèle | Licence | Intérêt |
|--------|---------|---------|
| FLUX.1 [schnell] | Apache 2.0 | Rapide, commercial OK |
| FLUX.2 [klein] 4B | Apache 2.0 | Qualité 2025+, commercial OK, 12 Go VRAM |
| Stable Diffusion 3.5 (Large/Medium) | Stability Community License (gratuit < 1 M$ revenus) | Écosystème immense, 10 Go VRAM (Medium) |
| Qwen-Image 2.0 (Alibaba, 20B) | Apache 2.0 | Typographie pro, édition |
| Z-Image (Alibaba) | Ouverte (**licence à vérifier**) | Alternative légère |

## 43. Tableau comparatif image (sept 2026)

| Critère | Midjourney V8.2 | GPT Image 2 | Imagen 4 Ultra | Nano Banana Pro | FLUX.2 Pro (API) | FLUX local (klein 4B) | Ideogram 4.0 | Firefly |
|---------|----------------|-------------|----------------|-----------------|------------------|----------------------|--------------|---------|
| Qualité esthétique | ★★★★★ | ★★★★ | ★★★★ | ★★★★ | ★★★★ | ★★★ | ★★★ | ★★★ |
| Photoréalisme | ★★★★ | ★★★ | ★★★★★ | ★★★★★ | ★★★★ | ★★★ | ★★★ | ★★★ |
| Fidélité au prompt | ★★★★ | ★★★★★ | ★★★★ | ★★★★ | ★★★★ | ★★★★ | ★★★★ | ★★★ |
| Texte dans l'image | ★★★ | ★★★★ | ★★★★★ | ★★★★★ | ★★★★ | ★★★ | ★★★★★ | ★★★ |
| Cohérence personnage | ★★★★ | ★★★ | ★★ | ★★★ | ★★★★★ (multi-ref) | ★★★★ | ★★★ | ★★★ |
| Prix / image (ordre) | abo (~0,03-0,05 $ eff.) | 0,01-0,21 $ | 0,06 $ | 0,134 $ | **0,03 $** | ~0 $ (GPU) | 0,03-0,10 $ | abo |
| API | Non (officiel) | Oui | Oui | Oui | Oui | Locale (ComfyUI) | Oui | Oui (Adobe) |
| Usage local | Non | Non | Non | Non | Non | **Oui** | Non | Non |
| Usage commercial | Oui (Pro/Mega si >1 M$) | Oui (CGU API) | Oui (API payante) | Oui (API payante) | Oui (CGU API) | **Oui (Apache 2.0)** | Oui (payant) | Oui (licencié) |

Lecture : pas de gagnant absolu. **Stack recommandée pour un usage pro mixte** : FLUX (local ou API BFL) en base quotidienne + Nano Banana Pro ou Imagen 4 Ultra pour le photoréalisme critique + Ideogram pour les affiches à texte + Midjourney si l'esthétique « belle image » prime.

## 44. Image : prompts efficaces — la méthode commune

Tous les modèles image 2026 partagent la même grammaire de prompt :

```
[sujet précis] + [contexte/environnement] + [lumière] + [point de vue/cadrage]
+ [style/rendu] + [détails techniques] + [ce qu'on NE veut pas]
```

Exemple « doc technique » (à adapter par modèle) :

> « photographie technique d'un onduleur triphasé 40 kVA en armoire 19 pouces, voyants verts en façade, câbles d'arrivée par le bas, local technique propre, lumière neutre 5000K, cadrage frontal légèrement en contre-plongée, netteté maximale, style documentation constructeur, -- pas de personnes, pas de texte flou, pas de filigrane »

Notes par modèle :
- **Midjourney V8** : le langage naturel narratif marche mieux que la liste de mots-clés (« un technicien fatigué qui... » > « technicien, fatigué, néons »). Paramètres `--style raw` pour moins d'embellissement.
- **FLUX** : prompts descriptifs denses, guillemets pour le texte à afficher (« panneau avec le texte "LOCAL TGBT" »), négatif explicite utile.
- **GPT Image** : instructions littérales et structurées très bien suivies (« à gauche..., à droite..., exactement trois... »).
- **Imagen/Nano Banana** : bon avec la description d'appareil photo (« 35mm, f/2.8, lumière dorée »).

---

# PARTIE E — PANORAMA VIDÉO : les autres acteurs

## 45. Sora : l'avertissement (API fermée le 24/09/2026)

Rappel factuel, vérifié le 27/09/2026 :

- **24/03/2026** : OpenAI annonce l'arrêt (6 mois de préavis, politique standard de dépréciation).
- **26/04/2026** : fermeture de l'appli et du web Sora.
- **24/09/2026** : **fin de la Videos API** — modèles `sora-2`, `sora-2-pro` et tous les snapshots datés **retirés de l'API, sans remplaçant annoncé** (colonne « remplacement recommandé » : vide).
- Les contenus devaient être exportés avant la fermeture (suppression définitive après la fenêtre d'export).

**Ce que ça t'apprend** : (1) pinner les versions d'API et suivre les pages de dépréciation ; (2) isoler l'appel vidéo derrière une interface interne pour pouvoir changer de fournisseur ; (3) ne jamais laisser des assets uniquement chez le fournisseur. Les alternatives de migration citées par la presse : **Veo 3.1** (même écosystème Google si tu y es déjà) et **Kling 3.0** (qualité/prix).

## 46. Google Veo 3.1 : le haut de gamme

- Accès : **Gemini API** et **Vertex AI** (même SDK/facturation que le reste de Google — gros avantage si tu es déjà client GCP).
- Points forts : **réalisme cinématographique**, **audio natif synchronisé**, dialogues (12 langues de script selon les tests tiers), contrôles de caméra. Le choix « premium » quand le budget le permet.
- Tarifs API vérifiés (sept 2026, par seconde) :

| Variante | 720p / 1080p | 4K |
|----------|--------------|-----|
| Standard (+audio) | **0,40 $/s** | 0,60 $/s |
| Fast | 0,10 $ / 0,12 $/s | 0,30 $/s |
| Lite | 0,05 $ / 0,08 $/s | — |

Un clip 8 s 1080p : **3,20 $** (Standard), 0,96 $ (Fast), 0,64 $ (Lite). Les variantes sont des modèles différents, pas des paliers de qualité équivalente : Fast/Lite sont visiblement en retrait.
- Clips de **8 s** max. Pas d'offre gratuite API (l'accès passe par les abonnements Google AI côté grand public).
- Idéal pour : le clip « vitrine » (présentation d'entreprise, vidéo d'appel d'offres), là où la qualité justifie 3 $ les 8 secondes.

## 47. Runway Gen-4.5 : la plateforme des pros de l'image

- Positionnement : **suite de production** plus que simple générateur — Gen-4.5 (génération), **Aleph** (montage/édition IA), **Act-Two** (transfert de jeu d'acteur), **Motion Brush 2.0**, Director Mode, voix custom.
- Tarifs (vérifiés sept 2026) : **Standard 15 $/mois** (625 crédits), **Pro 35 $/mois** (2 250 crédits), **Unlimited 95 $/mois** (2 250 crédits + générations relax illimitées sur certains outils) ; annuel ≈ -20 % (12/28/76 $).
- Consommation : **Gen-4.5 ≈ 25 crédits/seconde** → le plan Standard ne produit qu'**~25 secondes** de Gen-4.5 par mois. Un clip 10 s Gen-4 + upscale 4K ≈ 140 crédits. **C'est le plus cher du panorama à qualité égale** — le prix de la plateforme et de la régularité (le moins de re-rolls nécessaires selon les comparatifs).
- **API développeur** : existe (modèles Runway + modèles tiers comme Veo 3.1 au catalogue, facturation au crédit). Attention : changer de modèle dans l'API ne rend pas les paramètres interchangeables — chaque modèle a ses formats/durées propres.
- Plan gratuit : 125 crédits **one-shot** (non renouvelés), et Gen-4 exclu du gratuit (on teste avec Gen-3 Alpha Turbo : trompeur pour évaluer le produit actuel).

## 48. Luma Ray 3.2 (Dream Machine) : l'atmosphérique

- Points forts : **effets atmosphériques** (fumée, feu, eau, brouillard) — le meilleur du marché sur ce créneau selon les comparatifs 2026 ; mouvement naturel ; **Brainstorm mode** ; start/end frames historiques.
- Points faibles : **pas d'audio natif** (à ajouter à part), plus de variance d'un prompt à l'autre (davantage de re-rolls), cohérence de personnage en retrait de Kling.
- Tarifs 2026 (le produit a été restructuré : gamme **Luma Agents** + legacy Dream Machine) : **Plus 30 $/mois** (10 000 crédits), **Pro 90 $/mois** (40 000), **Ultra 300 $/mois** (150 000) ; legacy Dream Machine : Lite 9,99 $, Plus 29,99 $, Unlimited 94,99 $. **Usage commercial à partir de Plus** ; le gratuit/pas cher = watermark + usage personnel uniquement.
- Repère de coût : ~**170 crédits pour 5 s**, ~800 crédits pour 10 s (variable selon résolution/modèle). **Les crédits mensuels n'expirent pas en report** : non consommés = perdus (seuls les packs Top-Up, dès 4 $/1 200 crédits, sont valables 12 mois).
- Luma Agents regroupe sous un même compteur **Veo 3.1, Kling 3.0, Seedance et ElevenLabs** : intéressant si tu veux jongler entre moteurs sans multiplier les abonnements.

## 49. Hailuo / MiniMax H3 : le brouillon rapide et pas cher

- **MiniMax** (éditeur chinois, produit **Hailuo**) : le positionnement « **rapide et économique** ».
- Version 2026 : **H3 (Hailuo 3.0)** — tarif API officiel **forfaitaire : 0,26 $/seconde** (5 s = 1,30 $, 15 s = 3,90 $), résolution 2K, durées 5-15 s ; mode ref2v : +0,08 $/s par image de référence au-delà de 5. Génération en 5-15 s chrono : le plus rapide du marché.
- Abonnement : **Standard 14,99 $/mois** (des offres à 9,99 $/mois ont existé — **à vérifier**).
- Usage : **itération rapide**, storyboards animés, brouillons avant de masteriser sur Kling/Veo. La communauté l'utilise en « draft » et Kling/Runway en « final ».
- **Licence** : nos recherches du 27/09/2026 (volume Chine) signalent pour MiniMax H3 une **licence territoriale restrictive** — **à vérifier** dans le texte de licence avant tout usage commercial hors périmètre autorisé. Versions précédentes (2.3) : 512p-1080p, 6-10 s, ~0,045 $/s via agrégateurs.

## 50. Seedance (ByteDance) : le précis

- **Seedance** est le modèle vidéo de **ByteDance** (maison-mère de TikTok). Version marquante : **2.0 (février 2026)**, puis **2.5**.
- Points forts : **précision du mouvement** (danse, action, expressions — l'ADN TikTok), **entrée multimodale** (texte + images + vidéo + audio combinés, jusqu'à 12 fichiers), **clips longs : jusqu'à 30 s** en un appel (contre 15 s chez Kling), audio natif.
- Tarifs API (2.5, sept 2026) : **0,134 $/s à 480p**, **~0,29 $/s à 720p** (10 s 720p ≈ 2,90 $ ; 30 s ≈ 8,70 $). Moins « beau » que Kling/Veo à réglages égaux, mais **plus obéissant** sur le mouvement demandé.
- Accès : API BytePlus/Seedance + agrégateurs (fal.ai : ~0,30 $/s en 720p). Écosystème API plus jeune que Kling/Runway/Veo.

## 51. Les poids ouverts vidéo : Wan, LTX

- **Wan 2.x (Alibaba)** : famille **open weights** (2.5/2.6/2.7), 1080p, 10-15 s, avec audio sur certaines versions. Le choix « FLUX de la vidéo » pour du local/confidentiel. Via agrégateurs : dès ~0,03-0,07 $/s.
- **LTX 2.3 (Lightricks)** : ouvert, rapide, 20 s max, pensé pour le temps réel et l'itération.
- **MiniMax H3** : évoqué comme partiellement ouvert par certaines sources (**licence restrictive — à vérifier**, voir section 49).
- En local, la vidéo générative reste **très gourmande** (24 Go VRAM minimum pour du court, temps de rendu en minutes) : le cloud reste la norme, le local l'exception motivée (confidentialité, volume).

## 52. Tableau comparatif vidéo (sept 2026)

| Critère | Kling 3.0 | Veo 3.1 | Runway Gen-4.5 | Luma Ray 3.2 | Hailuo H3 | Seedance 2.5 |
|---------|-----------|---------|----------------|--------------|-----------|--------------|
| Durée max / appel | 15 s | 8 s | ~10 s | ~10 s | 15 s | **30 s** |
| Résolution max | **4K native** | 4K (Standard) | 4K (upscale) | 4K (upscale) | 2K | 1080p |
| Audio natif | Oui (+50 %) | Oui (inclus) | Limité | **Non** | Oui | Oui |
| Coût indicatif / 10 s 1080p | ~1,1-1,7 $ (API) | 4,0 $ (Standard) / 1,2 $ (Fast) | ~5-6 $ (équiv.) | ~2-3 $ (équiv.) | 2,60 $ | ~2,9 $ |
| API officielle | Oui (packs prépayés) | Oui (Gemini/Vertex) | Oui (crédits) | Oui | Oui | Oui (BytePlus) |
| Usage local | Non | Non | Non | Non | Partiel (**licence à vérifier**) | Non |
| Point fort | Qualité/prix, motion transfer | Réalisme premium | Régularité, suite pro | Atmosphérique | Vitesse, prix | Mouvement précis, 30 s |
| Usage commercial | Oui (plans payants) | Oui (API) | Oui (plans payants) | Oui (dès Plus) | Oui (**licence à vérifier**) | Oui (CGU) |

## 53. Vidéo : prompts efficaces — spécificités

Par rapport à l'image, ajouter systématiquement : **la durée ressentie** (« en 8 secondes »), **le mouvement de caméra**, **le rythme** (« lent », « dynamique »), **le son** si audio natif. Exemple « présentation d'équipement » :

> « plan séquence de 10 secondes : travelling avant lent vers un onduleur triphasé dans un local technique, LEDs vertes qui clignotent, léger flou d'arrière-plan sur les baies voisines, lumière froide industrielle, ambiance feutrée professionnelle. Audio : ronronnement grave et régulier de ventilation, pas de voix. Style documentaire corporate, photoréaliste. »

Et en **image-to-video** (souvent meilleur pour du pro) : partir d'une **vraie photo** du matériel + prompt minimal de mouvement (« léger travelling avant, les voyants clignotent doucement, 8 secondes »). Le réel ancre la crédibilité ; l'IA n'ajoute que le mouvement.

---

# PARTIE F — PANORAMA RECHERCHE IA : les alternatives à Perplexity

## 54. Pourquoi comparer

Perplexity est le mieux packagé, pas le seul. Selon le besoin (code, vie privée, personnalisation, écosystème chinois, intégration Microsoft), une alternative peut être meilleure. Toutes les suivantes existent et sont actives en sept 2026.

## 55. Phind : la recherche pour développeurs

**Phind** reste en 2026 le moteur de recherche IA **orienté code** : questions techniques, réponses avec extraits de code et citations de documentation. Pour un sysadmin qui cherche « exemple nftables rate-limit SSH » ou « script Python Netmiko sauvegarde configs Huawei », c'est souvent plus direct que Perplexity généraliste. Positionnement : moins « moteur de réponse grand public », plus « pair programmer qui cite la doc ».

## 56. You.com : la recherche personnalisable

**You.com** : plateforme de recherche IA **personnalisable** (sources préférées, apps intégrées, agents). Intérêt : construire une recherche « à ta main » (ex. prioriser la doc constructeur). En retrait de Perplexity en notoriété, mais vivant et utile pour des workflows de recherche répétitifs et paramétrables.

## 57. Kimi (Moonshot AI) : l'outsider chinois à suivre

- **Kimi**, assistant de **Moonshot AI** (Pékin) : recherche web intégrée, raisonnement profond, contexte ultra-long.
- **16/07/2026** : sortie de **Kimi K3** (modèle revendiqué à 2 800 Md de paramètres, **poids ouverts** sous licence Kimi K3 — le plus gros modèle ouvert jamais publié ; ~1,4 To de stockage, usage self-host réservé aux grosses infra).
- Fait marquant : **suspension des nouveaux abonnements** après le lancement (saturation des GPU — la demande a dépassé la capacité), IPO de Hong Kong en préparation (~3 Md$ visés selon Reuters, sept 2026).
- API : `platform.kimi.com` (compatible Chat Completions). Pertinent pour : veille sur l'écosystème chinois (docs Huawei, Xiaomi...), alternative géopolitique aux clouds US.

## 58. Microsoft Copilot : l'intégré

**Copilot** (ex-Bing Chat) : recherche IA **intégrée à l'écosystème Microsoft** (Edge, Windows, M365). Si ton poste et ta messagerie sont Microsoft, c'est la recherche IA « sans friction » : pas d'abonnement séparé, données dans ton tenant (selon ton contrat). Moins bon en citation rigoureuse que Perplexity, meilleur en intégration bureautique.

## 59. Les autres cités en 2026

**Brave Leo** (vie privée, navigateur Brave), **DuckDuckGo Search Assist** (vie privée), **Komo** (catégories ciblées), **Andi** (résultats visuels, gratuit), **ChatGPT avec recherche** et **Gemini / AI Mode de Google** (les assistants généralistes font aussi de la recherche web temps réel — voir volumes existants). La tendance : **la recherche IA devient une fonctionnalité partout**, plus un produit isolé.

## 60. Tableau comparatif recherche IA

| Critère | Perplexity | Phind | You.com | Kimi | Copilot | Google/Bing classique |
|---------|-----------|-------|---------|------|---------|----------------------|
| Citations sources | ★★★★★ | ★★★★ | ★★★ | ★★★★ | ★★★ | ★ (c'est toi qui cherches) |
| Synthèse rédigée | ★★★★★ | ★★★★ | ★★★★ | ★★★★ | ★★★★ | Non |
| Orientation code | ★★★ | ★★★★★ | ★★★ | ★★★★ | ★★★ | ★★★ (docs brutes) |
| API développeur | ★★★★★ (Sonar) | Limitée (**à vérifier**) | Oui (**à vérifier**) | Oui | Via Azure | Non (SERP payantes) |
| Vie privée | ★★ | ★★ | ★★★ | ★★ | ★★★ (tenant MS) | ★ |
| Prix entrée | 0 $ / 20 $ | Freemium (**à vérifier**) | Freemium (**à vérifier**) | Freemium / suspendu | Inclus M365 | 0 $ |
| Point fort | Recherche sourcée | Code + doc | Personnalisation | Contexte énorme, Chine | Intégration MS | Contrôle total, gratuit |

## 61. Quelle recherche pour quel besoin (pense-bête)

- **Vérifier un fait technique vite** → Perplexity (puis ouvrir la source).
- **Chercher un exemple de code/config** → Phind.
- **Veille automatisée / digest d'équipe** → Perplexity Spaces ou API Sonar.
- **Écosystème chinois / docs constructeurs CN** → Kimi.
- **Déjà dans M365, zéro friction** → Copilot.
- **Recherche obscure, opérateurs fins, gratuit** → Google/Bing classique (ça reste un outil pro).

---

# PARTIE G — JEV : le nom mystérieux, identifié

## 62. « jev » : identification (recherche du 27/09/2026)

Bonne nouvelle : **« jev » n'est pas une coquille**. C'est **Jev**, le premier modèle public de **TypeSafe AI** (San Francisco, fondée en 2024), sorti de la discrétion (**stealth**) le **15/09/2026**, avec une levée seed de **40 M$** (valorisation ~200 M$ selon Forbes). Vérifié le 27/09/2026 via la presse tech et la documentation développeur.

Point crucial : **Jev n'est PAS un outil de génération d'image ou de vidéo.** C'est un modèle de **décision** — ce que TypeSafe appelle « System One » (référence à Kahneman, *Thinking, Fast and Slow*) : là où un LLM « parle » (System 2 : lent, verbal), Jev **juge** (System 1 : rapide, pré-verbal). Il ne génère aucun texte, aucune image : il répond à des questions fermées par des **probabilités**.

## 63. Jev : ce qu'il fait, techniquement

- **Entrée** : un `state` (texte ou JSON : le contexte à juger) + une **liste de questions** nommées, évaluées **en parallèle**.
- **Trois types de questions, rien d'autre** :

| Type | Question | Retour |
|------|----------|--------|
| `noul` | jugement oui/non | P(oui) entre 0 et 1 |
| `choice` | choisir parmi ≤ 255 options | option + distribution de probabilités + confiance |
| `score` | noter selon des niveaux ordonnés | score + probabilités + confiance |

- **Sortie garantie valide par construction** : 0 % de JSON malformé (le cauchemar du « demande à un LLM de classer et renvoie du JSON » disparaît).
- **Latence** : 70 à 500 ms de bout en bout. **Texte uniquement** : pas d'image, pas d'audio, pas de vidéo en entrée.
- **Endpoint** : `POST https://api.typesafe.ai/v1/systemone`, clé Bearer. SDKs **Python et JavaScript**, intégration **Vercel AI Gateway**. Aliases `jev-latest` / `jev-preview` (pointent vers la même version ; **pinner la version** `jev-1.13.0` en prod pour éviter les changements silencieux).
- **Contexte** : 64k tokens par requête (dont 32k pour `state` + la plus longue question).

## 64. Jev : prix et accès (vérifiés le 27/09/2026)

- **0,042 $ par million de tokens d'entrée ; tokens de sortie gratuits** (il ne génère pas de texte : logique).
- Rate limits affichés : 250 000 tokens/s, 1 200 req/min (ajustés selon la demande).
- Accès : **API hébergée, liste d'attente early-access**. **Pas de poids ouverts, pas de self-host, pas de nombre de paramètres publié.**

Ordre de grandeur : qualifier 1 000 tickets ou visuels coûte **quelques centimes** (les benchmarks éditeur annoncent ~0,0004 $/cas — chiffre éditeur, à prendre avec recul, mais l'ordre de grandeur « quasi-gratuit » est crédible vu le prix d'entrée).

## 65. Jev : à quoi ça sert dans TON pipeline (angle Zelef)

Jev ne génère pas tes visuels, mais il peut **garder** ton pipeline de génération :

1. **Contrôle qualité automatique** : après génération d'une série d'images (schémas, visuels de doc), envoyer chaque image *décrite en texte* (légende + prompt + métadonnées) à Jev : « le schéma montre-t-il un onduleur triphasé ? (noul) », « quel défaut potentiel ? (choice: texte illisible / mains déformées / aucun) ». Rejeter sous un seuil de confiance.
2. **Routage** : « ce ticket demande-t-il une image, une vidéo, ou une recherche ? (choice) » → aiguillage automatique vers le bon outil.
3. **Garde-fous** : « ce prompt contient-il une marque identifiable / une personne réelle ? (noul) » avant génération — filtre anti-risque juridique à 0,00004 $ l'appel.
4. **Triage de veille** : sur un digest Perplexity Sonar, scorer la pertinence de chaque item avant de l'envoyer à l'équipe.

Exemple (schéma) :

```python
import os, requests
JEV_KEY = os.environ["JEV_API_KEY"]
r = requests.post("https://api.typesafe.ai/v1/systemone",
    headers={"Authorization": f"Bearer {JEV_KEY}"},
    json={
        "model": "jev-1.13.0",          # pinner la version, pas l'alias
        "state": "Image générée pour la doc 'Remplacement module UPS' : "
                 "prompt='armoire électrique ouverte, borniers alignés', "
                 "modèle=flux-2-pro, seed=42",
        "questions": {
            "montre_borniers": {"type": "noul",
                "question": "La description indique-t-elle des borniers visibles et alignés ?"},
            "defaut": {"type": "choice",
                "question": "Quel est le défaut le plus probable de ce visuel ?",
                "options": ["texte_illisible", "cablage_incoherent", "aucun_defaut"]},
        },
    }).json()
print(r)  # ex. {"montre_borniers": {"p_yes": 0.93}, "defaut": {"choice": "aucun_defaut", "confidence": 0.88}}
```

## 66. Jev : limites honnêtes

- **Ne génère rien** : ni texte, ni image, ni vidéo, ni raisonnement visible. Pour une phrase, un LLM ; pour une décision, Jev.
- **Pas de multimodalité** : il ne « voit » pas tes images — il juge leur **description textuelle**. Le contrôle qualité visuel réel reste humain (ou un modèle vision).
- **Jeune produit** (sorti de stealth le 15/09/2026) : écosystème naissant, prix et limites susceptibles d'évoluer vite, **pas d'option on-prem**.
- **Anglais d'abord** : les autres langues sont documentées comme moins précises — écrire les `state`/`questions` en anglais en prod.
- Ne pas lui demander : calculs exacts, dates, raisonnement multi-étapes, appels d'outils — ce n'est pas son métier.

---

# PARTIE H — COÛTS RÉELS : budgéter sans se faire piéger

## 67. La méthode : partir du livrable, pas du prix facial

Pour chaque besoin, estimer dans l'ordre :

1. **Volume de livrables/mois** (ex. 40 visuels de doc, 6 clips vidéo, 1 digest de veille hebdo).
2. **Taux de rejet** réaliste (×2 à ×3 générations par livrable au début, ×1,5 quand tu maîtrises).
3. **Coût unitaire tout compris** (génération + essais + upscale éventuel).
4. **Frais fixes** (abonnements) vs **variables** (API).
5. Comparer **3 scénarios** : tout-abonnement / tout-API / hybride.

## 68. Exemple chiffré 1 : 40 visuels/mois pour de la doc technique

Hypothèses : 40 images 1024² validées, taux de rejet ×2 (80 générations), usage commercial, confidentialité souhaitée.

| Scénario | Calcul | Coût / mois |
|----------|--------|-------------|
| **FLUX local (klein 4B)** | 0 $ marginal (GPU déjà là, élec. négligeable à cette échelle) | **~0 $** |
| API BFL (FLUX.2 pro) | 80 × 0,03 $ | **2,40 $** |
| fal.ai (schnell brouillon + pro final : 60×0,003 + 20×0,03) | 0,18 + 0,60 | **0,78 $** |
| Midjourney Basic | abonnement | **10 $** (illimité relax : non, Basic n'a pas le relax) |
| Nano Banana Pro (API) | 80 × 0,134 $ | **10,72 $** |

Verdict : pour de la doc technique interne, **le local ou l'API BFL écrasent tout**. Midjourney ne se justifie que si l'esthétique « belle image » est le besoin.

## 69. Exemple chiffré 2 : 6 clips vidéo/mois (présentation d'équipement)

Hypothèses : 6 clips 10 s 1080p validés, ×3 essais (18 générations), audio natif souhaité.

| Scénario | Calcul | Coût / mois |
|----------|--------|-------------|
| Kling web **Pro** (25,99 $) | 18 gén × ~35-70 crédits (pro 10 s ± audio) ≈ 630-1 260 crédits < 3 000 inclus | **25,99 $** (marge restante) |
| Kling **API** (pack Trial 98 $/1 000 u) | 18 × 10 s × 0,168 $/s (1080p+audio) ≈ 30 $ | **~30 $** sur le pack (validité 30 j !) |
| Veo 3.1 Standard (API) | 18 × 10 s... 8 s max → 18 × 8 × 0,40 $ | **57,60 $** |
| Veo 3.1 Fast | 18 × 8 × 0,12 $ | **17,28 $** |
| Runway Standard (15 $) | 625 crédits ; 10 s Gen-4.5 ≈ 250 crédits → 2 clips max | **insuffisant** → Pro 35 $ |

Verdict : **Kling Pro web** est le sweet spot pour ce volume ; l'API Kling ne devient intéressante qu'au-delà (ou via fal.ai à l'usage pur, sans pack qui expire).

## 70. Exemple chiffré 3 : veille automatisée (API Sonar)

Hypothèses : 1 digest hebdomadaire = ~20 questions de veille (4 semaines = 80 requêtes/mois).

| Scénario | Calcul | Coût / mois |
|----------|--------|-------------|
| `sonar` | 80 × (~2 000 tokens à 1 $/1M + frais req. ~5 $/1K) | **~0,50 $** |
| `sonar-pro` | 80 × (~5 000 tokens à 3-15 $/1M + frais req. ~14 $/1K) | **~2-5 $** |
| Abonnement Pro (20 $) + Spaces | inclus | **20 $** (mais tu as tout le reste) |

Verdict : la veille automatisée via API coûte **quelques dollars par mois**. L'abonnement Pro se justifie par l'usage interactif quotidien, pas par la veille seule.

## 71. Le vrai coût caché : les packs qui expirent

Récapitulatif des « dates de péremption » (vérifié sept 2026) :

| Fournisseur | Ce qui expire | Délai |
|-------------|--------------|-------|
| Kling **API** (packs) | Unités non consommées | **30 ou 180 jours** selon le pack, sans report |
| Kling **web** (abonnement) | Crédits mensuels | Fin de période, sans report |
| Luma | Crédits mensuels | Fin de période (seuls les Top-Up durent 12 mois) |
| Midjourney | Heures GPU | Fin de période, sans report |
| BFL (API) | Crédits pré-chargés | Selon CGU (**à vérifier** : pas de durée trouvée au 27/09/2026) |
| fal.ai / Replicate | Facturation à l'usage | **Pas d'expiration** (tu paies ce que tu consommes) |

**Règle** : usage ponctuel → **facturation à l'usage** (fal.ai, Replicate) ou petit pack Trial. Usage régulier et prévisible → abonnement/API officielle. Jamais de gros pack prépayé « au cas où ».

## 72. Budget annuel type « service systèmes & énergies »

Proposition réaliste pour un service qui produit doc + vidéos + veille :

| Poste | Choix | Coût annuel |
|-------|-------|-------------|
| Images doc (volume) | FLUX local (klein 4B/dev) ou API BFL | 0-50 $ |
| Images premium ponctuelles | fal.ai (Nano Banana Pro / Imagen) à l'usage | ~100 $ |
| Vidéo | Kling Pro web (25,99 $/mois, annuel -34 %) | ~205 $ |
| Vidéo premium ponctuelle | Veo 3.1 Fast via API à l'usage | ~100 $ |
| Recherche interactive | Perplexity Pro (200 $/an) | 200 $ |
| Veille automatisée | API Sonar (`sonar`) | ~10 $ |
| Contrôle qualité / routage | Jev (API) | ~5 $ |
| **Total** | | **~670 $/an** |

Soit **moins de 60 $/mois** pour un outillage complet. Le poste qui dérape toujours : la vidéo (multiplier les essais, l'audio, la 4K). Plafonner par un quota mensuel par personne.

## 73. Dix règles anti-dérapage budgétaire

1. **Toujours convertir les crédits en USD/livrable** avant de comparer.
2. **Ne jamais acheter un gros pack prépayé** sans historique de consommation sur 2-3 mois.
3. **Séparer les budgets** : brouillon (modèles pas chers : schnell, klein, Hailuo, Veo Lite) vs master (pro, 4K).
4. **Noter le fournisseur + le modèle + la date** à côté de chaque comparaison de prix (les grilles bougent indépendamment).
5. **Activer les alertes de dépense** sur chaque console API dès le premier jour.
6. **Une clé API par usage** (dev/prod/veille) pour suivre qui dépense quoi ; rotation en cas de fuite.
7. **Tester à petit format** : 480p/brouillon d'abord, 1080p/4K ensuite (Seedance, Veo et Kling facturent au format).
8. **Couper l'audio natif** quand il n'apporte rien (jusqu'à +50 % à ×5 du coût).
9. **Réutiliser** : une bonne image de référence (Kling O1, FLUX multi-ref) vaut 10 re-générations.
10. **Revue mensuelle** : 15 min pour vérifier le coût/livrable réel vs estimé, et ajuster.

---

# PARTIE I — LICENCES & DROITS : ce que tu as le droit de faire

## 74. Tableau des droits d'usage commercial (vérifié le 27/09/2026)

| Outil | Usage commercial autorisé ? | Conditions / limites |
|-------|----------------------------|----------------------|
| Kling (plans payants) | **Oui** | Plan gratuit : **interdit** (watermark + clause) |
| Kling API | Oui | Selon contrat dev |
| FLUX API (BFL) | Oui | Selon CGU API |
| FLUX.2 [klein] 4B local | **Oui** | Apache 2.0 |
| FLUX.1 [schnell] local | **Oui** | Apache 2.0 |
| FLUX.2 [dev] / klein 9B local | **Non** | Licence commerciale à obtenir auprès de BFL |
| Midjourney | Oui | **>1 M$ de revenus/an → Pro ou Mega obligatoire** ; images publiques par défaut sans Stealth |
| OpenAI (GPT Image / DALL-E, API) | Oui | CGU OpenAI (droits cédés à l'utilisateur) |
| Google (Imagen / Nano Banana, API payante) | Oui | CGU Google Cloud / AI Studio |
| Ideogram (payant) | Oui | Selon plan |
| Adobe Firefly | Oui | Modèle entraîné sur contenu licencié : le plus sûr juridiquement |
| Runway (payant) | Oui | Dès le plan Standard |
| Luma (payant) | Oui | Dès le plan Plus |
| Hailuo / MiniMax | Oui a priori | **Licence territoriale restrictive signalée — à vérifier** |
| Perplexity (contenu des réponses) | Réponses : oui ; **sources citées : leurs propres licences** | Ne pas republier des extraits longs sans vérifier |
| Jev | Oui (sortie = tes décisions) | CGU TypeSafe |

## 75. Le principe juridique de base (pas un avis d'avocat)

En droit français/européen, une image **100 % générée par IA sans apport créatif humain** ne bénéficie en principe pas du droit d'auteur classique (l'originalité suppose une personne physique). Conséquences pratiques :

- Tes visuels IA sont **difficilement protégeables** contre la copie.
- En revanche, **l'assemblage créatif** (direction artistique, sélection, montage, texte ajouté) peut être protégé.
- Pour une doc d'entreprise, ce n'est généralement pas un problème ; pour une identité visuelle de marque, si.
- **Ceci est un principe général, pas un avis juridique** : pour un enjeu réel (marque, contentieux), consulter un juriste.

## 76. Deepfakes, personnes réelles et consentement

- **Ne jamais générer** une personne réelle identifiable (collaborateur, client, personnalité) sans son **consentement écrit**, même « pour une blague de service ». En France, le droit à l'image s'applique ; les montages trompeurs peuvent relever du pénal.
- L'**EU AI Act** impose des **obligations de transparence** pour les contenus générés par IA (mention du caractère artificiel, marquage). Les modalités fines évoluent : **à vérifier** au moment de publier, surtout pour du contenu externe.
- En interne : mentionner « visuel généré par IA » sur les docs qui en contiennent — c'est de l'hygiène, pas de la paranoïa (ça évite qu'on prenne un schéma illustratif pour une photo réelle d'installation).
- Les plateformes (Kling, Luma, Midjourney...) filtrent les contenus sensibles ; les contournements volontaires violent leurs CGU et peuvent faire bannir le compte.

## 77. Données d'entraînement et risque de contentieux

- Des éditeurs attaquent les fournisseurs d'IA (ex. **News Corp, Nikkei/Asahi contre Perplexity**, procédures en cours en 2025-2026). Le risque pèse d'abord sur le **fournisseur**, mais un usage intensif de contenus litigieux dans ta prod n'est pas neutre.
- L'option la plus défendable juridiquement pour une entreprise : **Adobe Firefly** (entraînement licencié) ou des modèles **open weights à licence claire** (Apache 2.0).
- Éviter de générer dans le **style signature d'un artiste vivant nommé** (« à la manière de X ») pour un usage commercial : zone grise juridique et éthique.

## 78. Confidentialité des données envoyées aux API

Règle simple, trois niveaux :

1. **Public / non sensible** → n'importe quelle API.
2. **Interne entreprise** (photos de locaux, schémas, docs) → API avec **résidence UE** quand elle existe (BFL : `api.eu.bfl.ai`), contrats enterprise (SSO, DPA), ou **local**.
3. **Confidentiel / secret** (plans détaillés, données clients, infra critique) → **local uniquement** (FLUX klein 4B/schnell). Aucune API publique.

Ne jamais envoyer : identifiants, clés, données personnelles non nécessaires, documents couverts par un NDA client — sauf validation contractuelle explicite avec le fournisseur.

## 79. Watermarks et métadonnées

- Les offres gratuites **watermarkent** (Kling free, Luma free) : un watermark oublié sur un livrable client = incident d'image.
- Certaines plateformes ajoutent des **métadonnées C2PA** (provenance du contenu). Les conserver : c'est ta preuve de bonne foi en cas de question.
- **Ne jamais retirer un watermark** d'un tiers pour faire passer un visuel gratuit pour un visuel payé : violation de CGU.

## 80. Check-list juridique avant publication d'un visuel/vidéo IA

- [ ] Plan payant / licence qui autorise l'usage commercial ? (vérifié, pas supposé)
- [ ] Aucune personne réelle identifiable sans consentement ?
- [ ] Aucune marque/logo d'un tiers reproduit ?
- [ ] Mention « généré par IA » si diffusion externe (ou interne sensible) ?
- [ ] Source des éléments repris (si montage avec du réel) : droits OK ?
- [ ] Watermark du plan gratuit absent ?
- [ ] Pour FLUX local : variante **Apache 2.0** (klein 4B / schnell), pas dev ?

---

# PARTIE J — PIÈGES TRANSVERSAUX : 20 erreurs à ne pas commettre

## 81. Piège n°1 : les crédits qui expirent en silence

Kling API (30/180 j), Kling web (fin de mois), Luma (fin de mois), Midjourney (fin de mois) : partout, le non-consommé est perdu. **Symptôme** : tu paies 700 $ de pack « pour l'année » et tu en utilises 200 $. **Parade** : commencer petit, mesurer 2 mois, puis dimensionner. Voir section 71.

## 82. Piège n°2 : web ≠ API, deux caisses séparées

Chez Kling (et c'est fréquent ailleurs), l'abonnement web ne crédite pas l'API et réciproquement. **Symptôme** : « je suis Pro à 25,99 $, pourquoi l'API me dit crédit insuffisant ? » **Parade** : décider dès le départ si ton usage est manuel (web) ou programmé (API), et ne payer qu'une caisse.

## 83. Piège n°3 : les générations ratées sont facturées

Un clip avec six doigts, un schéma avec du texte en charabia : tu as payé quand même. Avec un taux de rejet de ×3 au démarrage, ton coût réel est le triple du prix facial. **Parade** : brouillons sur modèles pas chers (schnell, klein, Hailuo, Veo Lite), master sur le bon modèle ; fixer un quota d'essais par livrable.

## 84. Piège n°4 : l'audio natif qui multiplie la facture

Sur Kling, l'audio natif peut multiplier le coût par 2,5 à 5 (selon version et mode). **Symptôme** : la facture triple le mois où « on a juste coché la case son ». **Parade** : ne l'activer que si le son apporte vraiment (dialogue, ambiance) ; sinon ajouter une bande-son en montage (gratuit, contrôlable).

## 85. Piège n°5 : la 4K systématique

La 4K native (Kling 3.0 : 30 crédits/s ; Veo : 0,60 $/s) coûte 3 à 5× le 1080p. Pour de l'intranet, de la formation projetée en salle ou du web, le 1080p suffit dans 90 % des cas. **Parade** : master en 1080p, 4K uniquement pour diffusion grand écran / print vidéo.

## 86. Piège n°6 : la licence « open weights » confondue avec « open source »

FLUX.2 [dev] se télécharge librement mais **interdit l'usage commercial** sans licence BFL. **Symptôme** : des visuels de doc client générés « gratuitement » en infraction. **Parade** : klein 4B ou schnell (Apache 2.0) pour le commercial local ; lire la licence avant, pas après. Voir section 18.

## 87. Piège n°7 : le fournisseur qui ferme (leçon Sora)

Le 24/09/2026, l'API Sora a disparu avec 6 mois de préavis et **zéro remplaçant**. **Parade** : isoler chaque appel IA derrière une interface interne (une fonction `generer_video()` à toi, pas des appels Kling en dur partout), pinner les versions, exporter les assets, suivre les pages de dépréciation, toujours avoir un fournisseur B identifié.

## 88. Piège n°8 : la version qui change sous tes pieds

`jev-latest`, les snapshots de modèles, les « améliorations silencieuses » : un pipeline qui marchait peut dériver sans que ton code change. **Parade** : pinner les versions exactes (`jev-1.13.0`, `flux-2-pro`, snapshot daté), logger modèle+version à chaque génération, tester en non-régression après chaque upgrade.

## 89. Piège n°9 : les données confidentielles dans le prompt

Envoyer à une API publique : une photo d'installation client, un schéma réseau annoté, un plan de local technique. **Parade** : classification en 3 niveaux (section 78), endpoint UE quand il existe, local pour le confidentiel. Et relire ce que contient une image **avant** de l'uploader (les EXIF aussi).

## 90. Piège n°10 : Perplexity pris pour une source primaire

Une réponse bien citée n'est pas une preuve. Les modèles **résument parfois de travers**, et une citation peut pointer vers un blog qui lui-même se trompe. **Parade** : pour toute valeur d'ingénierie (tension, couple, norme), ouvrir la source primaire. Perplexity = le pointeur, pas la preuve.

## 91. Piège n°11 : le texte dans l'image qui ment

« 400 V » devenu « 4000 V », un logo approximatif, une étiquette en charabia. Dans une doc technique, c'est un **risque de sécurité**, pas un défaut esthétique. **Parade** : relire chaque texte généré caractère par caractère ; pour les schémas cotés, générer l'image **sans** texte et ajouter le texte en PAO/DAO après.

## 92. Piège n°12 : l'incohérence d'un visuel à l'autre

Le même onduleur qui change de couleur, le technicien qui change de visage entre deux clips. **Parade** : FLUX.2 multi-références, Kling O1 (identité), seeds fixes, prompts avec description verrouillée du sujet (« même armoire grise RAL 7035, voyants verts »). Pour une série, valider le « personnage » d'abord, décliner ensuite.

## 93. Piège n°13 : le watermark oublié

Sortie du plan gratuit (Kling, Luma) utilisée telle quelle dans une présentation client. **Parade** : bannir les exports gratuits des dossiers « livrables » ; nommer les dossiers `brouillon/` vs `final/` ; check-list de publication (section 80).

## 94. Piège n°14 : la personne réelle générée sans consentement

Le collègue « incrusté » dans une vidéo de formation pour la blague, le client reconnaissable dans un visuel. **Parade** : consentement écrit ou anonymisation (visages non identifiables, silhouettes). Le droit à l'image ne rigole pas, et l'EU AI Act durcit la transparence.

## 95. Piège n°15 : le style d'un artiste vivant pour du commercial

« À la manière de [artiste] » pour une affiche d'entreprise : zone grise juridique et éthique, et un style reconnaissable qui vieillit mal. **Parade** : décrire le style par ses caractéristiques (« affiche rétro, aplats, typographie bold ») plutôt que par un nom propre.

## 96. Piège n°16 : la file d'attente le jour J

Présentation client à 14h, génération lancée à 13h30 sur le plan web gratuit/standard : file de 30 min. **Parade** : générer la veille ; pour l'événementiel, plan avec file prioritaire (Kling Pro/Premier) ou API ; toujours avoir un plan B (slides statiques).

## 97. Piège n°17 : l'API sans garde-fous budgétaires

Un script de veille en boucle, un batch d'images sans limite, une clé API qui fuite sur GitHub. **Parade** : alertes de dépense dès le jour 1, quotas par clé, une clé par usage, jamais de clé dans le code (variables d'environnement / gestionnaire de secrets), rotation immédiate en cas d'exposition.

## 98. Piège n°18 : le « tout gratuit » qui coûte en temps

Enchaîner les offres gratuites (66 crédits/jour ici, 8 vidéos/mois là) pour « ne rien payer » : des heures perdues en files d'attente, watermarks, qualité brouillon. **Parade** : chiffrer ton temps (ton taux horaire × heures perdues) — 26 $/mois de Kling Pro sont rentabilisés en une heure gagnée.

## 99. Piège n°19 : l'over-reliance — l'IA comme béquille

Générer au lieu de comprendre : un schéma IA « à peu près juste » dans une procédure de consignation, une réponse Perplexity recopiée dans un rapport sans vérification. **Parade** : l'IA produit le **brouillon**, l'expert **valide**. Pour le critique (sécurité, conformité), la validation est non négociable et c'est toi qui signes.

## 100. Piège n°20 : oublier que les prix bougent

Toutes les grilles de ce guide datent du **27/09/2026**. Les fournisseurs ajustent tous les trimestres (exemples récents : restructuration Luma, packs Kling, batch -50 % chez Google). **Parade** : re-vérifier les 3-4 tarifs qui comptent pour toi **chaque trimestre** (15 min), et dater tes comparaisons internes.

---

# PARTIE K — CAS PRATIQUES COMMENTÉS

## 101. Cas n°1 : visuels pour une documentation technique

**Besoin** : illustrer une procédure « Remplacement d'un module de puissance sur onduleur 40 kVA » — 12 visuels (vues d'ensemble, détails borniers, gestes).

**Démarche** :
1. **Inventaire** : lister les 12 visuels, classer en « schéma » (6) et « photo d'ambiance » (6).
2. **Choix d'outil** : FLUX local (klein 4B, Apache 2.0 — commercial OK, 0 $ marginal, confidentiel). Si pas de GPU : API BFL (FLUX.2 pro, 0,03 $/image).
3. **Série cohérente** : fixer une description verrouillée (« armoire RAL 7035, voyants verts, sol technique gris ») + seed de base, déclinée par prompt.
4. **Génération** : 12 prompts, ×2 essais → ~24 générations, ~30 min en local.
5. **Relecture critique** : vérifier chaque texte/étiquette (piège n°11), chaque incohérence électrique (un disjoncteur « à l'envers », un câble impossible).
6. **Finition** : ajouter flèches/numéros/légendes en DAO (pas dans l'IA), mention « visuels d'illustration générés par IA » en pied de doc.
7. **Archivage** : prompts + seeds + modèle dans le dossier de la doc (reproductibilité pour la v2).

**Coût** : ~0 $ (local) ou ~0,72 $ (API BFL). **Temps** : 2-3 h tout compris.

## 102. Cas n°2 : vidéo de présentation d'un équipement (onduleur)

**Besoin** : clip 30 s pour présenter un onduleur 60 kVA sur l'intranet / à un client.

**Démarche** :
1. **Storyboard** : 3 plans de 10 s (1. travelling vers l'équipement, 2. détail façade/voyants, 3. plan large du local).
2. **Source** : photographier le vrai matériel (ou un équivalent) → **image-to-video** (crédibilité maximale, piège n°12 évité).
3. **Génération** : Kling 3.0, 1080p, **sans audio natif** (on ajoutera une voix off + musique libre en montage — moins cher et contrôlable). Multi-prompt si besoin de continuité narrative.
4. **Essais** : budgéter ×3 par plan (9 générations ≈ 9 × 10 s × ~0,35-0,70 $ en crédits web Pro, ou ~1,1 $/clip en API).
5. **Montage** : assembler (DaVinci/Premiere), ajouter cartons de texte **en montage** (pas générés), voix off, mention « séquences générées par IA ».
6. **Validation** : faire relire par un collègue qui connaît l'équipement (un détail absurde grille la crédibilité).

**Coût** : < 10 $ sur Kling Pro web (crédits inclus dans les 25,99 $/mois). **Temps** : une demi-journée.

## 103. Cas n°3 : veille automatisée hebdomadaire (API Sonar + cron)

**Besoin** : chaque lundi 7h, un digest « onduleurs, batteries, normes, CVE énergie » dans ta boîte mail.

**Démarche** :
1. **Clé API** Perplexity (compte à part « veille », alerte de dépense à 5 $/mois).
2. **Script Python** (modèle `sonar`, pas `sonar-pro` : 10× moins cher pour de la veille) :

```python
import os, smtplib
from email.message import EmailMessage
from openai import OpenAI

client = OpenAI(api_key=os.environ["PERPLEXITY_API_KEY"],
                base_url="https://api.perplexity.ai")

SUJETS = [
    "nouveautés onduleurs triphasés 10-120 kVA cette semaine",
    "batteries lithium pour UPS : nouveautés et rappels produits",
    "CVE ou failles de sécurité touchant des onduleurs ou équipements d'énergie connectés",
    "évolution normes NF C 15-100, NF C 18-510, IEC 62040",
]

blocs = []
for s in SUJETS:
    r = client.chat.completions.create(
        model="sonar",   # économique ; passer à sonar-pro si la qualité suit mal
        messages=[{"role": "user",
                   "content": f"Actualités des 7 derniers jours : {s}. "
                              "Réponds en 5 puces max, chaque puce avec la source "
                              "(titre + URL + date). Si rien de notable : dis-le."}])
    blocs.append(f"## {s}\n{r.choices[0].message.content}")

msg = EmailMessage()
msg["Subject"] = "Veille énergie — hebdo"
msg["From"] = "veille@exemple.fr"
msg["To"] = "zelef@exemple.fr"          # adresse d'exemple : adapter
msg.set_content("\n\n".join(blocs))
with smtplib.SMTP("localhost") as smtp:  # ou ton relais SMTP
    smtp.send_message(msg)
```

3. **Cron** : `0 7 * * 1 /usr/bin/python3 /opt/veille/veille_hebdo.py` (adapter le chemin).
4. **Garde-fous** : timeout, log des appels, Jev en option pour scorer la pertinence avant envoi (section 65).
5. **Coût** : ~80 requêtes/mois en `sonar` ≈ **0,50 $/mois**.

**Variante sans code** : Space Perplexity avec tâche planifiée (section 34) — 0 ligne de code, inclus dans Pro.

## 104. Cas n°4 : série de visuels cohérents (identité produit)

**Besoin** : 20 visuels d'un même équipement (ex. une gamme d'onduleurs) pour un catalogue interne, sous plusieurs angles et décors, **sans** pouvoir photographier chaque configuration.

**Démarche** :
1. **Référence** : 3-4 vraies photos de l'équipement sous différents angles.
2. **Outil** : **FLUX.2 multi-références** (jusqu'à ~6-10 images) ou **Kling O1** côté vidéo — le verrouillage d'identité sans fine-tuning.
3. **Protocole** : valider UN visuel maître (angle 3/4, décor neutre), puis décliner (décor atelier, décor data center, détail façade) en gardant les mêmes références + description verrouillée.
4. **Contrôle** : passer chaque visuel dans la check-list (logo correct ? proportions ? texte lisible ?) ; rejeter sans pitié (le coût unitaire est de quelques centimes).
5. **Alternative lourde** (si l'identité doit être parfaite sur 200+ visuels) : fine-tuning LoRA sur FLUX en local — investissement initial (dizaines d'images d'entraînement, heures de GPU), rentabilisé au volume.

**Coût** : quelques dollars en API, ou 0 $ en local. **Leçon** : la cohérence se **conçoit** (références + protocole), elle ne s'obtient pas en relançant le même prompt en espérant mieux.

---

# PARTIE L — CHECK-LISTS & PENSE-BÊTE TERRAIN

## 105. Pense-bête : quel outil pour quel besoin (1 page)

```
BESOIN IMAGE
├── Beau visuel, affiche, créa ................. Midjourney V8.2
├── Doc technique, schéma, volume .............. FLUX (local klein 4B / API BFL)
├── Photoréalisme critique (portrait, produit) . Imagen 4 Ultra / Nano Banana Pro
├── Texte long lisible dans l'image ............ Ideogram 4.0 / Nano Banana Pro
├── Instructions complexes suivies au pied ..... GPT Image 2
├── Juridiquement sûr (entreprise frileuse) .... Adobe Firefly
└── Confidentiel / air-gap ..................... FLUX local (Apache 2.0)

BESOIN VIDÉO
├── Rapport qualité/prix, usage régulier ....... Kling 3.0 (Pro web)
├── Clip vitrine premium ....................... Veo 3.1 Standard
├── Régularité + suite de prod ................. Runway Gen-4.5
├── Ambiance (fumée, eau, feu) ................. Luma Ray 3.2
├── Brouillon rapide pas cher .................. Hailuo H3 / Veo Lite
├── Mouvement précis, clip 30 s ................ Seedance 2.5
└── Intégration par API au volume .............. Kling API / Veo API / fal.ai

BESOIN RECHERCHE
├── Fait technique sourcé, vite ............... Perplexity (+ ouvrir les sources)
├── Code / config .............................. Phind
├── Veille auto d'équipe ....................... Perplexity Spaces / API Sonar
├── Écosystème chinois ......................... Kimi
└── Déjà dans M365 ............................. Copilot

DÉCISION AUTO (pas de génération)
└── Classer / scorer / router / garde-fou ...... Jev (TypeSafe)
```

## 106. Check-list : avant de générer (2 min)

- [ ] Le besoin est-il bien de la **génération** (vs une vraie photo, un vrai schéma DAO) ?
- [ ] Le bon outil pour ce besoin (pense-bête section 105) ?
- [ ] Brouillon sur modèle **pas cher** d'abord ?
- [ ] Rien de **confidentiel** dans le prompt / l'image source ? (section 78)
- [ ] Aucune **personne réelle** / marque sans droit ?
- [ ] Budget : coût estimé × taux de rejet, dans le quota du mois ?
- [ ] Licence : usage commercial autorisé pour ce que je vais en faire ? (section 74)

## 107. Check-list : avant de publier un livrable IA (5 min)

- [ ] Textes dans l'image **relus caractère par caractère** (tensions, étiquettes, logos)
- [ ] Cohérence vérifiée (couleurs, équipement, personnage identiques dans la série)
- [ ] Pas de watermark / pas d'export « gratuit »
- [ ] Mention « généré par IA » si diffusion externe ou doc sensible
- [ ] Métadonnées de provenance conservées (C2PA si présentes)
- [ ] Prompts + seeds + modèle **archivés** avec le livrable (reproductibilité)
- [ ] Pour la vidéo : visionnée **en entier**, audio écouté (pas de mot fantôme / bruit bizarre)

## 108. Check-list : mise en prod d'une API IA (pour ton RAG / tes scripts)

- [ ] Clé dans variable d'environnement / gestionnaire de secrets, **jamais** dans le code
- [ ] Une clé par usage (dev / prod / batch), alertes de dépense activées
- [ ] Version du modèle **pinnée** (pas d'alias flottant)
- [ ] Timeouts + retries bornés + logs (modèle, version, coût par appel)
- [ ] Webhooks plutôt que polling quand supportés
- [ ] Plan B fournisseur identifié (leçon Sora, section 87)
- [ ] Page de dépréciation du fournisseur surveillée (trimestriel)
- [ ] Données envoyées : classification OK (section 78), rien d'interdit par NDA/client

## 109. Kit de survie : commandes et liens utiles

```bash
# Suivi des dépenses (exemples, adapter)
# BFL : console https://dashboard.bfl.ai  (crédits, historique)
# Kling : https://kling.ai/dev            (packs, consommation API)
# Perplexity : console API                (usage tokens + requêtes)
# fal.ai / Replicate : dashboard           (facturation à l'usage, par modèle)

# Hygiène des clés
export BFL_API_KEY="..." PERPLEXITY_API_KEY="..." KLING_API_KEY="..."
# Ne JAMAIS : git add d'un .env ; copier une clé dans un chat ; la laisser dans l'historique shell partagé
```

## 110. Anti-sèche tarifs (au 27/09/2026 — re-vérifier trimestriellement)

```
IMAGE / génération 1024² : FLUX schnell ~0,003 $ | FLUX.2 pro 0,03 $ | Imagen 4 Std 0,04 $
  | DALL-E 3 0,04 $ | Nano Banana Pro 0,134 $ | GPT Image high 0,167 $
VIDÉO / 10 s 1080p : Kling ~1,1-1,7 $ (API) | Hailuo H3 2,60 $ | Seedance 2,5 ~2,9 $
  | Veo Fast 1,2 $ | Veo Standard 4 $ | Runway ~5-6 $
RECHERCHE : Perplexity Pro 20 $/mois | Sonar API ~1 $/1M tok | Sonar Pro ~3/15 $/1M + 5-14 $/1K req
DÉCISION : Jev 0,042 $/1M tokens entrée, sortie gratuite
```

---

# PARTIE M — GLOSSAIRE

## 111. Air-gap

Machine ou réseau **physiquement isolé** d'Internet. Seuls les modèles en local (FLUX poids ouverts) permettent un vrai workflow air-gap pour des données sensibles.

## 112. Apache 2.0

Licence open source **permissive** : usage commercial autorisé, y compris modification et redistribution, avec obligation de conserver la notice de licence. Dans ce guide : FLUX.1 [schnell], FLUX.2 [klein] 4B, VAE FLUX.2, Qwen-Image 2.0.

## 113. Batch API

Mode de traitement **différé** (file d'attente, rendu en heures) avec **~50 % de remise** (Google : -50 % sur Imagen/Nano Banana ; OpenAI : -50 % sur plusieurs API). Idéal pour le volume planifiable, inutilisable pour l'interactif.

## 114. C2PA

Standard de **provenance des contenus** (métadonnées signées : « cette image a été générée par tel outil à telle date »). De plus en plus intégré par les plateformes. À conserver : c'est ta traçabilité.

## 115. Checkpoint / poids (weights)

Fichiers contenant le modèle entraîné (ex. `flux2_dev_fp8mixed.safetensors`, ~35 Go). « Poids ouverts » = téléchargeables ; la **licence** dit ce que tu as le droit d'en faire.

## 116. CFG / guidance scale

Paramètre qui règle à quel point le modèle **suit le prompt** (valeurs typiques 3-7). Trop haut : artefacts, image « brûlée ». Trop bas : le modèle ignore ton prompt.

## 117. ComfyUI

Orchestrateur **open source** de workflows de génération d'image/vidéo par **nœuds** (interface web locale). Standard de fait pour FLUX en local : reproductible, scriptable via API, immense écosystème d'extensions.

## 118. Crédit (plateforme)

Unité de facturation interne. **Piège** : 1 crédit Kling ≠ 1 crédit Luma ≠ 1 crédit Runway. Toujours convertir en **USD par livrable**.

## 119. Deep Research

Mode de recherche IA **approfondie** : le système enchaîne des dizaines de recherches et de lectures avant de synthétiser (Perplexity, Gemini...). Puissant pour cartographier un sujet, lent (minutes), à vérifier comme le reste.

## 120. Diffusion (modèle de)

Famille de modèles génératifs qui partent du **bruit** et le « débruitent » pas à pas vers une image/vidéo (principe de FLUX, Stable Diffusion, Kling, Veo...). D'où les paramètres « steps » et « sampler ».

## 121. Distillation

Technique qui compresse un gros modèle en un **petit modèle rapide** qui l'imite (FLUX [schnell], klein 4B). Moins fidèle, beaucoup plus rapide et léger.

## 122. Fine-tuning / LoRA

Ré-entraînement (complet ou léger via **LoRA**, quelques Mo) d'un modèle sur **tes** images pour lui apprendre un style, un équipement, un personnage. Rentable au volume (catalogue de 200 visuels cohérents), overkill pour 10 images.

## 123. FP8 / quantification

Réduction de la **précision des poids** (32 bits → 8 bits) pour diviser la VRAM et accélérer, avec une perte de qualité faible. Le réglage standard 2026 pour FLUX.2 en local (24 Go au lieu de 64).

## 124. Guidance-distilled

Modèle distillé pour fonctionner avec peu de steps **sans** réglage fin de guidance (cas de FLUX.1 [dev]). En pratique : moins de boutons à tourner, qualité stable.

## 125. Inpainting / outpainting

**Inpainting** : régénérer une zone masquée d'une image (effacer un défaut, changer un détail). **Outpainting** : étendre l'image au-delà de ses bords (passer du carré au 16:9).

## 126. Kontexte (FLUX.1 Kontext)

Modèle d'**édition d'image par instruction** (« remplace X par Y ») **sans masque**, avec conservation du reste de l'image et cohérence de personnage. Le plus abouti de sa catégorie en 2026.

## 127. Lip-sync

Synchronisation **lèvres/parole** dans la vidéo générée (Kling 3.0, Veo 3.1, Seedance : audio natif avec dialogues). À tester dans la langue cible : la qualité varie selon les langues.

## 128. LoRA

Voir 122. Fichier léger (~Mo) qui **spécialise** un modèle de base (un équipement, un style). S'échange et se combine facilement dans l'écosystème ouvert.

## 129. Motion transfer

Extraire le **motif de mouvement** d'une vidéo de référence pour l'appliquer à un autre sujet (différenciateur Kling 3.0). Utile quand tu as la « bonne » gestuelle sous les yeux mais pas le bon sujet.

## 130. Multi-prompt

Découper une génération vidéo en **segments temporels**, chacun avec son prompt (Kling 3.0 : jusqu'à 15 s, 512 caractères/segment). La façon de raconter une mini-histoire en un plan continu.

## 131. Nano Banana

Surnom communautaire des modèles **image de Google** (Gemini 2.5 Flash Image = Nano Banana, 3.1 = Nano Banana 2, 3 Pro = Nano Banana Pro). Photorealisme et édition de haut niveau via l'API Gemini.

## 132. Negative prompt

Ce qu'on **ne veut pas** (« flou, déformé, filigrane, texte illisible, six doigts »). Très utile sur FLUX/Midjourney pour éliminer les défauts récurrents.

## 133. Polling / webhook

Deux façons de suivre une génération **asynchrone** : **polling** = interroger régulièrement « c'est prêt ? » ; **webhook** = le fournisseur t'appelle quand c'est prêt. Le webhook est la bonne pratique en prod.

## 134. Prompt

La **consigne** en langage naturel. En 2026, les bons prompts sont **descriptifs et structurés** (sujet + action + caméra + lumière + style), pas des listes de mots-clés — sauf en recherche où les opérateurs classiques restent rois.

## 135. Quantification

Voir 123 (FP8).

## 136. RAG (rappel)

**Retrieval-Augmented Generation** : ton pipeline — une base documentaire interrogée par un LLM qui cite ses sources. Ce guide t'aide à choisir les **outils de recherche** (Perplexity/Sonar) et de **génération de visuels** qui gravitent autour de ton RAG, pas à le reconstruire (voir tes volumes existants).

## 137. Rectified flow

Technique d'entraînement des modèles FLUX (v2) : trajectoire de « débruitage » plus directe que la diffusion classique → **moins de steps, meilleure fidélité**. Détail technique, mais c'est ce qui rend FLUX rapide à qualité égale.

## 138. Relax mode

File d'attente **lente et illimitée** (Midjourney Standard/Pro/Mega, Luma Unlimited) : tu ne consommes pas ton quota, tu attends. Idéal pour le volume non urgent.

## 139. Seed

**Graine aléatoire** : à prompt et modèle identiques, même seed = même image. La base de la **reproductibilité** (noter le seed de chaque visuel validé).

## 140. Sonar

Famille de modèles **API de Perplexity** : recherche web temps réel + génération avec citations, facturée au token + à la requête. Le pont entre « je cherche à la main » et « ma veille tourne toute seule ».

## 141. Stealth mode

Générations **privées** (non visibles par la communauté) — Midjourney Pro/Mega uniquement. Sans ça, tes prompts et images sont publics par défaut. Pour du pro : indispensable ou rédhibitoire.

## 142. Steps (pas de diffusion)

Nombre d'itérations de débruitage. Plus = mieux (jusqu'à un plateau) mais plus lent. Repères : FLUX.1 schnell 1-4, FLUX.1/2 dev 20-50.

## 143. System One / System Two

Référence à Kahneman reprise par **TypeSafe** : System 2 = le LLM qui raisonne en parlant (lent) ; **System One = Jev**, qui juge vite par probabilités. Deux outils complémentaires, pas concurrents.

## 144. Text-to-video / image-to-video

**T2V** : vidéo depuis un texte. **I2V** : animation d'une image existante — souvent **meilleur pour le pro** (le réel ancre la crédibilité, l'IA n'ajoute que le mouvement).

## 145. Token

Unité de facturation des API de langage/recherche (~0,75 mot en anglais). Les API Sonar facturent **tokens + requêtes** : surveiller les deux compteurs.

## 146. Upscale

Augmentation de la **résolution après génération** (ex. 1024 → 4096). Souvent facturée à part (Runway, BFL au mégapixel). À n'appliquer que sur les visuels **validés**.

## 147. VAE

**Auto-encodeur variationnel** : le module qui compresse/décompresse l'image dans l'espace latent du modèle. Celui de FLUX.2 est **Apache 2.0** : tu peux l'utiliser librement dans tes pipelines.

## 148. VRAM

Mémoire de la carte **GPU**. Le facteur limitant n°1 du local : 12 Go (klein 4B), 16 Go (FLUX.1 dev FP8), 24 Go (FLUX.2 dev FP8).

## 149. Watermark (filigrane)

Marque incrustée par les **offres gratuites** (Kling, Luma...). Bannie des livrables : passer au payant ou ne pas utiliser.

## 150. Weights

Voir 115 (checkpoint).

---

# PARTIE N — QUIZ : 10 questions pour valider

## 151. Questions

**Q1.** Quelle est la version phare de Kling au 27/09/2026, et quelles sont ses deux différenciateurs majeurs face à la concurrence ?
**Q2.** Tu génères des visuels en local avec FLUX.2 [dev] pour une documentation client facturée. Problème ?
**Q3.** Quelle est la différence de facturation entre l'abonnement web Kling et l'API Kling ?
**Q4.** Cite trois choses qui expirent / ne se reportent pas chez les fournisseurs de ce guide.
**Q5.** Pourquoi faut-il « pinner » la version d'un modèle dans un pipeline de production ? (Deux raisons.)
**Q6.** L'API Sora 2 d'OpenAI : que s'est-il passé le 24/09/2026, et quelle leçon d'architecture en tires-tu ?
**Q7.** Dans quel cas l'audio natif de Kling 3.0 est-il un mauvais calcul économique ?
**Q8.** Perplexity te répond avec des citations sur une valeur de réglage (ex. tension de floating). Que fais-tu avant de l'utiliser dans une note de calcul ?
**Q9.** Qu'est-ce que Jev (TypeSafe), et pourquoi ne peut-il pas générer tes visuels ?
**Q10.** Calcule le coût API de 200 images 1024² en FLUX.2 [pro] via l'API BFL, et compare avec 200 images Nano Banana Pro via l'API Gemini.

## 152. Réponses

**R1.** **Kling 3.0** (sortie février 2026). Différenciateurs : le **motion transfer** (appliquer le mouvement d'une vidéo de référence à un autre sujet) et le **rendu de texte à l'écran** le plus lisible du marché, plus la 4K native et l'audio natif avec lip-sync multilingue.
**R2.** **Oui, problème** : FLUX.2 [dev] est en **licence non-commerciale**. Pour du commercial en local : FLUX.2 [klein] 4B ou FLUX.1 [schnell] (Apache 2.0), ou licence commerciale BFL.
**R3.** Ce sont **deux facturations totalement séparées** : l'abonnement web (crédits mensuels, interface) ne crédite pas l'API, et les packs API prépayés (30/180 jours de validité) ne servent pas sur le web.
**R4.** Au choix : crédits mensuels Kling web (fin de période), unités des packs API Kling (30/180 j), crédits Luma mensuels, heures GPU Midjourney, crédits mensuels non consommés en général.
**R5.** (1) Éviter les **changements silencieux** de comportement quand l'alias ou le snapshot bouge ; (2) garantir la **reproductibilité** et le débogage (savoir exactement quelle version a produit quoi).
**R6.** **Fermeture définitive** de la Videos API (sora-2, sora-2-pro, snapshots), annoncée le 24/03/2026, **sans remplaçant**. Leçon : isoler chaque appel IA derrière une **interface interne** (fonction à toi), exporter les assets, suivre les dépréciations, toujours un fournisseur B.
**R7.** Quand le son n'apporte rien au livrable (ex. clip muet d'intranet, ambiance ajoutée de toute façon en montage) : l'audio natif peut **multiplier le coût par 2,5 à 5** pour zéro valeur ajoutée.
**R8.** **Ouvrir les sources citées** et vérifier la valeur dans la **source primaire** (doc constructeur, norme). Perplexity est un pointeur, pas une preuve — surtout pour une valeur d'ingénierie.
**R9.** Jev est un modèle de **décision** (TypeSafe AI, « System One ») : il répond à des questions fermées (oui/non, choix, score) par des **probabilités**, en ~100-500 ms, pour ~0,042 $/1M tokens. Il **ne génère rien** (ni texte, ni image, ni vidéo) : il juge, il ne crée pas. Usage : contrôle qualité, routage, garde-fous.
**R10.** FLUX.2 [pro] : 200 × 0,03 $ = **6 $**. Nano Banana Pro : 200 × 0,134 $ = **26,80 $**. Soit **~4,5× moins cher** avec FLUX.2 pro à qualité souvent comparable — d'où son intérêt pour le volume documentaire.

---

# PARTIE O — POUR ALLER PLUS LOIN

## 153. Documentations officielles (faire foi en cas de conflit avec ce guide)

- API Black Forest Labs : `https://docs.bfl.ai` — tarifs : `https://bfl.ai/pricing` (**à vérifier** : URLs de documentation, je n'ai pas ouvert chaque page le 27/09/2026 ; la page pricing est citée par la presse spécialisée).
- Kling développeurs : `https://kling.ai/dev` (packs, modèles, polling/webhooks).
- Perplexity API : `https://docs.perplexity.ai` — tarifs : `https://docs.perplexity.ai/docs/getting-started/pricing`.
- OpenAI (images) : `https://platform.openai.com/docs` (grille tarifaire officielle).
- Google (Imagen / Gemini image / Veo) : `https://ai.google.dev/gemini-api/docs/pricing` et console Vertex AI.
- Midjourney : `https://docs.midjourney.com` (modèles, paramètres, CGU).
- TypeSafe Jev : `https://docs.typesafe.ai` (**à vérifier** — produit sorti de stealth le 15/09/2026, la doc évolue vite).
- ComfyUI : `https://docs.comfy.org` ; templates FLUX.2 dans l'appli.

## 154. Pour creuser côté sysadmin

- **Automatiser la génération** : ComfyUI expose une **API HTTP** (file de prompts en JSON) — piloter tes batches d'images depuis un script, avec seeds et logs.
- **RAG + visuels** : indexer tes visuels générés (avec prompts/seeds en métadonnées) dans ton RAG : « retrouve le visuel de la procédure X » devient une recherche.
- **Supervision des coûts** : un export hebdo des consoles (BFL, Kling, Perplexity, fal.ai) vers un tableur ; alerte si coût/livrable dérive de >20 %.
- **GPU local** : pour FLUX.2 dev en FP8, une RTX 4090/5090 24 Go d'occasion ou un GPU cloud à l'heure (RunPod & co) selon ton volume — calculer le seuil de rentabilité (section 68).

## 155. Pour creuser côté métier (systèmes & énergies)

- **Banque de visuels d'équipements** : constituer ta bibliothèque de références (photos réelles + prompts validés) par famille (onduleurs, TGBT, groupes, clim) — c'est un actif qui s'amortit à chaque doc.
- **Gabarits vidéo** : un template de montage (intro 3 s, cartons, outro) réutilisé pour chaque équipement — la génération IA ne fait que les plans.
- **Veille réglementaire** : Space Perplexity « Normes & énergie » partagé à l'équipe (section 34) + digest API hebdo (section 103).
- **Formation** : clips courts (Kling/Hailuo) pour illustrer les gestes sécurité — plus mémorables qu'un paragraphe, moins chers qu'un tournage.

---

# PARTIE P — À VENIR : annonces vérifiées au 27/09/2026

## 156. Annonces officielles récentes (sourcées)

- **24/09/2026 — OpenAI ferme la Videos API (Sora 2).** Annonce initiale le 24/03/2026 (6 mois de préavis), appli fermée le 26/04/2026, API coupée le 24/09/2026, **aucun remplaçant annoncé**. Source : page de dépréciation officielle OpenAI (developers.openai.com), reprise par la presse tech le 24-25/09/2026.
- **15/09/2026 — TypeSafe AI sort Jev de stealth.** Premier modèle public `jev-1.13.0` (« System One »), levée seed 40 M$ (Forbes), API `api.typesafe.ai`. Source : presse tech (ADTmag 23/09/2026, The Rundown) et documentation développeur.
- **16/07/2026 — Moonshot AI publie Kimi K3** (poids ouverts, 2 800 Md de paramètres revendiqués), puis **suspend les nouveaux abonnements** (saturation GPU, 19-20/07/2026, Reuters). IPO de Hong Kong en préparation (~3 Md$ visés, Reuters 03/09/2026).
- **Février 2026 — Kling 3.0** (4K native, audio natif, motion transfer) et **Seedance 2.0** (ByteDance) : les deux lancements qui ont structuré le marché vidéo 2026.
- **Février 2026 — Perplexity coupe sa régie publicitaire** (lancée octobre 2025) : modèle 100 % abonnement/API.
- **Novembre 2025 — FLUX.2** (Black Forest Labs) : génération actuelle, avec klein 4B Apache 2.0.

## 157. Rumeurs (non confirmées — à ne pas prendre pour des faits)

- **RUMEUR non confirmée** : évolutions futures de la numérotation Kling (au-delà de 3.0), de Midjourney (au-delà de V8.2) ou de Veo (au-delà de 3.1) — **rien d'officialisé au 27/09/2026**.
- **RUMEUR non confirmée** : une API publique Midjourney grand public (seule une offre Enterprise sur mesure est évoquée par des sources tierces).
- **RUMEUR non confirmée** : réouverture des abonnements Kimi (dépend des capacités GPU, aucune date annoncée).
- Principe : tant qu'il n'y a pas d'annonce officielle (blog éditeur, page de dépréciation, presse vérifiée), c'est une rumeur. Les prix et versions de ce guide sont un **instantané du 27/09/2026**, pas une promesse.

## 158. Signaux faibles à surveiller (pas des annonces)

- **FLUX 3 vidéo** : une documentation tierce (sept 2026) fait état d'une offre vidéo BFL au compteur (~0,17 $/s en HD text/image-to-video) — **non confirmé par une annonce officielle BFL au 27/09/2026, à vérifier**. Si ça se confirme, BFL deviendrait un acteur vidéo à part entière.
- **Agrégateurs multi-modèles** (fal.ai, Replicate, Krea, Luma Agents) : la tendance est au **guichet unique** — un seul compteur pour Kling + Veo + Seedance + FLUX. À terme, ça peut changer l'équation « API officielle vs tiers ».
- **Contentieux éditeurs vs IA** (Perplexity et autres) : l'issue (accords de licence ? retraits de sources ?) impactera la qualité des moteurs de réponse. À suivre trimestriellement.

---

# PARTIE Q — NOMS NON IDENTIFIÉS

## 159. Verdict d'identification (recherche du 27/09/2026)

Deux noms étaient incertains dans la demande. Après recherche web sérieuse :

| Nom demandé | Verdict |
|-------------|---------|
| **« plexplxity »** | **Coquille pour Perplexity** — traité en Partie C (sections 28-34). Aucun outil distinct trouvé sous ce nom. |
| **« jev »** | **Identifié : Jev, modèle de décision de TypeSafe AI** (« System One »), sorti de stealth le 15/09/2026 — traité en Partie G (sections 62-66). **Attention** : ce n'est pas un outil d'image/vidéo. |

**Aucun nom n'est resté non identifié.** Il n'y a donc pas de section « NON TROUVÉ » à proprement parler — les deux pistes ont abouti. (Si tu rencontres d'autres noms obscurs, la méthode est la même : recherche web ciblée + vérification de l'éditeur avant d'inventer quoi que ce soit.)

---

# PARTIE R — SOURCES & DATES DE VÉRIFICATION

## 160. Comment ce guide a été vérifié

- **Date de vérification unique : 27/09/2026.** Recherche web ciblée par outil (existence, version, features, prix, API, licence).
- **Règle appliquée** : tout nom introuvable après recherche sérieuse aurait été marqué « NON TROUVÉ (sept 2026) » ; **aucune spec, aucun prix, aucune version n'a été inventé** — les points non confirmés sont marqués « à vérifier » ou « RUMEUR non confirmée ».
- **Limite assumée** : les pages tarifaires officielles changent vite et certaines n'ont été consultées qu'à travers la presse spécialisée et des documentations tierces récentes (sept 2026). **Avant d'engager un budget, re-vérifier la page officielle.**
- **Aucune clé API réelle** dans ce guide : tous les exemples utilisent des variables d'environnement.
- Volumes à croiser : `ia_modeles_chine.md`, `ia_modeles_occident.md`, `ia_modeles_specialises.md` (modèles de langage et spécialisés — non dupliqués ici).

## 161. Journal des vérifications (27/09/2026)

| Outil | Version confirmée | Prix confirmés | API confirmée | Licence vérifiée |
|-------|-------------------|----------------|---------------|------------------|
| Kling | 3.0 (fév. 2026) | Web : 0/6,99/25,99/64,99/127,99 $ ; API : packs 9,80-7 560 $ | Oui (`kling.ai/dev`, packs prépayés) | Commercial : plans payants uniquement |
| FLUX | FLUX.2 (pro/max/flex/dev/klein) ; FLUX.1 (pro/dev/schnell/kontext) | API BFL : 0,014-0,07 $/MP ; FLUX.1 : 0,04-0,08 $ | Oui (`api.bfl.ai`, `api.eu.bfl.ai`, `api.us.bfl.ai`) | klein 4B & schnell : Apache 2.0 ; dev : non-commercial |
| Perplexity | Offres Free/Pro/Max/Enterprise, Comet | Pro 20 $, Max 200 $, API Sonar ~1-15 $/1M + 5-14 $/1K req | Oui (Sonar, OpenAI-compatible) | Réponses : oui ; sources : leurs licences |
| Midjourney | V8.2 (défaut 24/07/2026) | 10/30/60/120 $ (GPU time) | Non (pas d'API publique) | Commercial oui ; >1 M$ → Pro/Mega |
| OpenAI image | gpt-image-2/1.5/1-mini/1, DALL-E 3 | 0,005-0,25 $/image selon modèle | Oui (Images API) | Oui (CGU) |
| Google image | Imagen 4 (Fast/Std/Ultra), Nano Banana (1/2/Pro) | 0,02-0,24 $/image ; batch -50 % | Oui (Gemini API / Vertex) | Oui (API payante) |
| Sora | API **fermée le 24/09/2026** | — | **Non (coupée)** | — |
| Veo 3.1 | 3.1 (Standard/Fast/Lite) | 0,05-0,60 $/s | Oui (Gemini/Vertex) | Oui |
| Runway | Gen-4.5 | 15/35/95 $ (crédits) | Oui (crédits) | Oui (payant) |
| Luma | Ray 3.2 / Luma Agents | 30/90/300 $ (Agents) | Oui | Oui (dès Plus) |
| Hailuo/MiniMax | H3 | 0,26 $/s forfaitaire ; abo ~14,99 $ | Oui | **Territoriale restrictive — à vérifier** |
| Seedance | 2.0/2.5 (ByteDance) | ~0,134-0,29 $/s | Oui (BytePlus + agrégateurs) | Oui (CGU) |
| Phind / You.com | Actifs en 2026 | Freemium (**à vérifier**) | Limitée/**à vérifier** | — |
| Kimi | K3 (16/07/2026, poids ouverts) | Abonnements **suspendus** | Oui (`platform.kimi.com`) | Licence Kimi K3 |
| Jev (TypeSafe) | jev-1.13.0 (stealth levé 15/09/2026) | 0,042 $/1M in, out gratuit | Oui (`api.typesafe.ai`) | CGU TypeSafe |

---

# PARTIE S — PATTERNS API COMPARÉS : le code qui marche en prod

## 162. Le pattern universel des API génératives

Toutes les API de ce guide partagent le même squelette (à l'exception des API synchrones simples) :

```
1. Authentification  → clé dans header (x-key, Bearer, ?key=)
2. Soumission        → POST avec prompt + paramètres → job_id / task_id
3. Attente           → polling GET ou webhook (préférer webhook en prod)
4. Récupération      → URL temporaire → télécharger VITE (elle expire !)
5. Journalisation    → modèle, version, seed, coût, durée
```

Les URL de téléchargement **expirent** (souvent 1h à 24h) : toujours rapatrier le fichier sur ton stockage immédiatement, jamais « le lien dans un tableur ».

## 163. Pattern : génération d'image synchrone (fal.ai, exemple)

fal.ai expose un endpoint synchrone pratique pour les prototypes (pas de polling) :

```python
import os, requests

FAL_KEY = os.environ["FAL_API_KEY"]
headers = {"Authorization": f"Key {FAL_KEY}", "Content-Type": "application/json"}

# FLUX schnell : brouillons à ~0,003 $/image
r = requests.post("https://fal.run/fal-ai/flux/schnell",
                  headers=headers,
                  json={"prompt": "armoire électrique industrielle, borniers alignés, "
                                  "photo technique nette, lumière neutre",
                        "image_size": "square_hd",
                        "num_inference_steps": 4}).json()
url = r["images"][0]["url"]
print("Image :", url)   # télécharger immédiatement (URL temporaire)
```

> Hôte et chemins illustratifs : la doc fal.ai fait foi. Le principe (clé `Key`, POST synchrone, `images[].url`) est stable.

## 164. Pattern : BFL asynchrone robuste (avec timeout et erreurs)

Version « prod » de l'exemple de la section 19 : timeouts, erreurs typées, téléchargement immédiat.

```python
import os, time, requests

BFL_KEY = os.environ["BFL_API_KEY"]
EU = "https://api.eu.bfl.ai"
H = {"x-key": BFL_KEY, "Content-Type": "application/json"}

def generer_image_bfl(prompt, width=1024, height=1024, model="flux-2-pro",
                      timeout_s=300):
    # 1. Soumission
    sub = requests.post(f"{EU}/v1/{model}", headers=H,
                        json={"prompt": prompt, "width": width, "height": height},
                        timeout=30).json()
    task_id = sub["id"]
    # 2. Attente bornée
    t0 = time.time()
    while time.time() - t0 < timeout_s:
        res = requests.get(f"{EU}/v1/get_result", headers=H,
                           params={"id": task_id}, timeout=30).json()
        st = res["status"]
        if st == "Ready":
            url = res["result"]["sample"]
            # 3. Rapatriement immédiat
            img = requests.get(url, timeout=60).content
            with open(f"bfl_{task_id}.png", "wb") as f:
                f.write(img)
            # 4. Journalisation (modèle, version, paramètres, coût estimé)
            print(f"OK {model} {width}x{height} -> bfl_{task_id}.png")
            return f"bfl_{task_id}.png"
        if st in ("Error", "Failed"):
            raise RuntimeError(f"BFL échec {task_id} : {res}")
        time.sleep(3)
    raise TimeoutError(f"BFL timeout {task_id} après {timeout_s}s")

# Exemple : coût ~0,03 $ (1 MP, flux-2-pro)
generer_image_bfl("salle serveurs, rangées de baies 19 pouces, LEDs bleues, "
                  "photo documentaire, lumière froide")
```

## 165. Pattern : Kling 3.0 via agrégateur (sans pack prépayé)

Quand tu veux de l'API Kling **sans** acheter un pack de 700 $ : passer par un agrégateur à l'usage pur. Schéma (fal.ai, tarifs constatés sept 2026 : ~0,112 $/s sans audio, ~0,168 $/s avec audio — **tarifs agrégateur, pas Kling officiel**) :

```python
import os, requests, time

FAL_KEY = os.environ["FAL_API_KEY"]
headers = {"Authorization": f"Key {FAL_KEY}", "Content-Type": "application/json"}

# Image-to-video : animer une vraie photo d'équipement
job = requests.post("https://fal.run/fal-ai/kling-video/v3.0/pro/image-to-video",
    headers=headers,
    json={
        "image_url": "https://ton-stockage/photo-onduleur.jpg",  # ton asset
        "prompt": "léger travelling avant, les voyants verts clignotent doucement, "
                  "ambiance feutrée de local technique, 10 secondes",
        "duration": "10",
        "aspect_ratio": "16:9",
    }).json()
print("Résultat :", job)  # selon l'agrégateur : synchrone ou job à suivre
```

Arbitrage : agrégateur = **0 $ d'engagement**, marge de ~20-40 % sur le tarif officiel ; API officielle = **moins cher à l'unité**, pack prépayé qui expire. Seuil de bascule typique : usage hebdomadaire régulier.

## 166. Pattern : Perplexity Sonar avec gestion du budget

```python
import os, time
from openai import OpenAI

client = OpenAI(api_key=os.environ["PERPLEXITY_API_KEY"],
                base_url="https://api.perplexity.ai")

BUDGET_REQUETES_MOIS = 500
compteur = {"n": 0}

def sonar_ask(question, model="sonar", max_tokens=800):
    """Interroge Sonar avec garde-fous : modèle économe par défaut, compteur."""
    if compteur["n"] >= BUDGET_REQUETES_MOIS:
        raise RuntimeError("Budget mensuel Sonar atteint — augmenter ou attendre.")
    t0 = time.time()
    r = client.chat.completions.create(
        model=model,   # "sonar" par défaut ; "sonar-pro" si besoin de profondeur
        messages=[{"role": "system",
                   "content": "Réponds en français, de façon concise, avec les sources "
                              "(titre + URL + date) après chaque affirmation importante."},
                  {"role": "user", "content": question}],
        max_tokens=max_tokens,
    )
    compteur["n"] += 1
    usage = r.usage
    print(f"[Sonar] req {compteur['n']}/{BUDGET_REQUETES_MOIS} | "
          f"{usage.prompt_tokens} in / {usage.completion_tokens} out | "
          f"{time.time()-t0:.1f}s")
    return r.choices[0].message.content

print(sonar_ask("Quelle est la durée de vie typique des condensateurs du bus DC "
                "d'un onduleur double conversion ? Cite les sources."))
```

## 167. Pattern : Gemini image (Nano Banana Pro) via l'API

```python
import os
from google import genai   # SDK google-genai (nom du package : à vérifier à l'install)

client = genai.Client(api_key=os.environ["GEMINI_API_KEY"])

# Nano Banana Pro : ~0,134 $/image en 1-2K
result = client.models.generate_content(
    model="gemini-3-pro-image-preview",
    contents=["Photographie produit d'un onduleur triphasé 60 kVA en armoire, "
              "fond gris clair studio, lumière douce, ultra net, style catalogue "
              "constructeur"],
)
# Sauvegarder les parties image de la réponse (structure selon SDK : à vérifier)
for i, part in enumerate(result.parts):
    if getattr(part, "as_bytes", None):
        with open(f"nbpro_{i}.png", "wb") as f:
            f.write(part.as_bytes)
        print(f"Image sauvegardée : nbpro_{i}.png (~0,134 $)")
```

> Le SDK `google-genai` évolue vite : vérifier le nom du modèle (`gemini-3-pro-image-preview`) et la structure de réponse dans la doc au moment de l'usage.

## 168. Pattern : Jev en garde-fou de pipeline (cascade vérifiée)

Le pattern recommandé par l'écosystème Jev : **LLM qui produit, Jev qui vérifie** (verified cascade).

```python
import os, requests

JEV_KEY = os.environ["JEV_API_KEY"]
JEV_URL = "https://api.typesafe.ai/v1/systemone"
JH = {"Authorization": f"Bearer {JEV_KEY}", "Content-Type": "application/json"}

def jev_check(state, questions, model="jev-1.13.0", seuil=0.75):
    """Retourne (ok: bool, détails). ok=False si une proba < seuil."""
    r = requests.post(JEV_URL, headers=JH, timeout=20,
                      json={"model": model, "state": state,
                            "questions": questions}).json()
    verdicts = {}
    for nom, q in questions.items():
        rep = r[nom]
        if q["type"] == "noul":
            p = rep["p_yes"]; ok = p >= seuil
        else:  # choice / score : on seuille la confiance
            p = rep.get("confidence", 0); ok = p >= seuil
        verdicts[nom] = {"ok": ok, "p": round(p, 3), "brut": rep}
    return all(v["ok"] for v in verdicts.values()), verdicts

# Exemple : valider un visuel avant publication (coût : une fraction de centime)
ok, details = jev_check(
    state="Visuel pour doc 'TGBT atelier' : prompt='armoire électrique, borniers', "
          "model=flux-2-klein-4b, seed=11. Relecture humaine : textes OK, câblage OK.",
    questions={
        "commercial_ok": {"type": "noul",
            "question": "La licence du modèle (Apache 2.0) autorise-t-elle l'usage commercial ?"},
        "risque": {"type": "choice",
            "question": "Quel est le risque résiduel de ce visuel ?",
            "options": ["aucun", "texte_douteux", "personne_visible", "marque_visible"]},
    })
print("PUBLIER :" if ok else "BLOQUER :", details)
```

## 169. Gestion d'erreurs commune : ce qu'il faut toujours coder

- **429 (rate limit)** : backoff exponentiel + jitter, jamais de retry bourrin (risque de bannissement, cf. leçon support.huawei.com de ton pipeline : ne pas bourriner).
- **Timeouts** : toujours bornés (30 s connexion, 300 s génération) ; un job vidéo peut durer plusieurs minutes, c'est normal.
- **Idempotence** : générer un `idempotency_key` par livrable pour ne pas payer deux fois en cas de retry.
- **Journal** : chaque appel loggue `{fournisseur, modèle, version, prompt_hash, seed, coût_estimé, statut}`. C'est ce journal qui permet la revue mensuelle des coûts (section 73, règle 10).
- **Secrets** : rotation immédiate si une clé apparaît dans un log, un chat ou un commit.

---

# PARTIE T — COMFYUI AVANCÉ : du prototype au batch

## 170. Piloter ComfyUI par API (batch de visuels)

ComfyUI expose une API HTTP : tu peux envoyer des workflows en JSON et récupérer les images — idéal pour générer une série de visuels documentaires sans cliquer.

```python
import json, os, requests, uuid

COMFY = "http://127.0.0.1:8188"

def queue_prompt(workflow: dict):
    r = requests.post(f"{COMFY}/prompt",
                      json={"prompt": workflow,
                            "client_id": str(uuid.uuid4())}).json()
    return r["prompt_id"]

# Exemple : on charge un workflow exporté (format API) et on change le prompt + seed
with open("workflow_flux2_api.json") as f:
    wf = json.load(f)

visuels = [
    ("Vue d'ensemble du local technique avec deux onduleurs 40 kVA", 101),
    ("Détail des borniers d'arrivée, câbles repérés", 102),
    ("Façade onduleur, voyants verts, afficheur", 103),
]
for texte, seed in visuels:
    wf["6"]["inputs"]["text"] = (  # nœud CLIP Text Encode : adapter l'id à ton workflow
        f"photographie technique documentaire : {texte}, lumière neutre "
        f"d'atelier, netteté maximale, style documentation constructeur")
    wf["3"]["inputs"]["seed"] = seed          # nœud KSampler
    pid = queue_prompt(wf)
    print(f"En file : {texte[:40]}... (job {pid}, seed {seed})")
# Récupérer ensuite via /history/{prompt_id} puis /view
```

Méthode : construire le workflow une fois dans l'UI, l'exporter en **format API** (menu), puis le paramétrer en Python. Chaque visuel : seed noté, prompt archivé → reproductibilité totale.

## 171. Workflow inpainting : corriger un défaut sans tout refaire

Le schéma est bon mais une étiquette est en charabia ? Pas besoin de régénérer :

1. Charger l'image dans ComfyUI (`Load Image`).
2. Peindre un **masque** sur la zone à corriger (clic droit sur l'image → « Open in MaskEditor »).
3. Brancher : image + masque → nœud d'inpainting du modèle (FLUX.1 [fill] ou le pipeline inpaint de ton modèle).
4. Prompt ciblé : « étiquette métallique vierge, brossé, sans texte » (le texte sera ajouté en DAO après — piège n°11).
5. Denoise ~0,7-0,85 : assez pour corriger, pas assez pour dénaturer le reste.

En local c'est gratuit et instantané comparé à une re-génération complète — et ça préserve tout ce qui était déjà bon.

## 172. Workflow upscale propre (pas juste « agrandir »)

Chaîne recommandée pour passer un visuel validé en haute résolution :

1. `Load Image` (le visuel validé en 1024²).
2. `Upscale Image (using Model)` → modèle `4x-UltraSharp.pth` (ou équivalent ESRGAN, ~67 Mo).
3. (Option) second passage avec un upscaler « créatif » à faible denoise (0,2-0,3) pour restaurer les micro-détails — attention : à fort denoise, le modèle **invente** des détails (dangereux sur un schéma technique : rester en upscale « fidèle »).
4. `Save Image`.

Règle : **upscaler seulement le visuel validé**, jamais les brouillons (l'upscale coûte du temps GPU et ne sauve pas une mauvaise composition).

## 173. LoRA : spécialiser FLUX sur tes équipements

Si tu dois produire **des dizaines de visuels** d'un même équipement (gamme d'onduleurs, armoires types) :

1. Constituer un **dataset** : 20-50 photos réelles de l'équipement (angles variés, bonne lumière, arrière-plans variés).
2. Légender chaque image (captioning : description courte et factuelle).
3. Entraîner un **LoRA** (outils : kohya_sd, ai-toolkit — quelques heures sur RTX 4090).
4. Utiliser : charger le LoRA dans ComfyUI (`Load LoRA`, force 0,7-1,0) + mot-déclencheur dans le prompt.

Coût : une demi-journée de setup + électricité. Gain : une **identité visuelle verrouillée** sur des centaines de générations, sans payer de multi-références à chaque appel. Pour < 20 visuels, le multi-référence FLUX.2 suffit (section 104).

## 174. ComfyUI : hygiène d'exploitation

- **Versionner les workflows** (git) : un workflow = un fichier JSON versionné avec la doc qu'il illustre.
- **Épingler les versions** : ComfyUI + custom nodes + poids — noter les versions dans un `ENVIRONNEMENT.md` ; une mise à jour peut casser un workflow (même leçon que section 88).
- **File et concurrence** : une génération à la fois par GPU ; pour du batch nocturne, une file (script section 170) + `--lowvram` si besoin.
- **Sauvegarde des poids** : 70-90 Go — les sauvegarder (disque externe), re-télécharger à chaque fois est une perte de temps.
- **Sécurité** : ComfyUI écoute en local par défaut (`127.0.0.1`) — ne pas l'exposer tel quel sur le réseau sans authentification (l'API permet l'exécution de code via custom nodes).

## 175. Dix prompts image commentés (copiables, à adapter)

**P1 — Photo documentaire d'armoire (FLUX, doc technique)**
> « photographie technique d'une armoire électrique industrielle ouverte, borniers à vis alignés sur rail DIN, repérage par étiquettes, câbles gris et bleus en goulottes, lumière neutre d'atelier, cadrage frontal, netteté maximale, style documentation constructeur »
*Pourquoi ça marche : sujet précis + environnement + lumière + cadrage + style. Le « style documentation constructeur » verrouille le registre.*

**P2 — Salle serveurs (ambiance)**
> « travelling photographique : allée centrale d'un data center, rangées de baies 19 pouces noires avec LEDs bleues et vertes, sol technique gris clair, plafond avec chemins de câbles, lumière froide, profondeur de champ, photoréaliste, 35mm »
*Le détail « 35mm » et la lumière guident le rendu photo ; les LEDs donnent la vie.*

**P3 — Schéma de principe (style épuré)**
> « illustration technique épurée en isométrique d'un local énergie : onduleur 40 kVA, armoire TGBT, climatisation de précision, flèches de flux d'air bleu et rouge, fond blanc cassé, lignes fines bleu marine, sans texte, style manuel d'ingénierie »
*« sans texte » : on ajoute les légendes en DAO après (piège n°11).*

**P4 — Détail macro connectique**
> « macrophotographie d'un bornier de puissance avec câbles 25mm² sertis, cosses tubulaires, repérage jaune-vert de la terre, métal cuivré net, profondeur de champ courte, lumière latérale d'atelier »
*Le macro cache la complexité globale : parfait pour illustrer un geste précis.*

**P5 — Avant/après réaménagement (Kontext, édition)**
> Image source : photo réelle du local. Instruction : « ajoute une deuxième armoire électrique grise identique à droite de l'existante, conserve le sol, la lumière et la perspective »
*Kontext excelle là où la génération from-scratch dériverait.*

**P6 — Visuel formation sécurité**
> « illustration pédagogique : technicien avec EPI (gants isolants, visière) devant une armoire consignée, cadenas et pancarte de consignation, style affiche de prévention, couleurs franches, composition lisible de loin »
*Penser « lisible de loin » : une affiche de sécurité se lit à 3 mètres.*

**P7 — Photo produit catalogue (Nano Banana Pro / Imagen Ultra)**
> « photographie produit studio d'un onduleur tour 10 kVA noir, façade avec afficheur LCD bleu, fond gris clair dégradé, lumière douce à 45 degrés, reflet discret au sol, ultra net, style catalogue constructeur »
*Pour le premium, le modèle premium : la différence se voit sur les matières.*

**P8 — Affiche avec titre (Ideogram)**
> « affiche A3 portrait pour une journée portes ouvertes 'ÉNERGIE & MAINTENANCE', titre en grandes lettres blanches en haut, illustration d'un technicien devant des baies serveurs au centre, bandeau bleu marine en bas avec la date, style corporate moderne »
*Le texte long : Ideogram ou Nano Banana Pro ; toujours relire le titre généré.*

**P9 — Pictogramme / icône (GPT Image)**
> « pictogramme minimaliste en ligne bleue sur fond blanc : onduleur avec éclair, style flat design, traits de 3px, aucune ombre, aucun texte »
*GPT Image suit bien les contraintes « aucun/aucune ».*

**P10 — Moodboard projet (Midjourney)**
> « série de 4 visuels d'ambiance pour un data center éco-responsable : lumière naturelle, végétalisation discrète, baies blanches, atmosphère calme et lumineuse --style raw »
*Midjourney pour l'intention esthétique ; `--style raw` pour calmer l'embellissement.*

## 176. Dix prompts vidéo commentés (copiables, à adapter)

**V1 — Présentation équipement (Kling I2V, 10 s)**
> Photo réelle de l'onduleur + « léger travelling avant vers la façade, les voyants verts clignotent doucement, reflet subtil sur le sol technique, 10 secondes, lumière froide stable »
*L'I2V depuis le réel : la crédibilité est déjà là, l'IA n'ajoute que le mouvement.*

**V2 — Geste technique (Kling T2V, 5 s)**
> « gros plan sur des mains gantées qui serrent un bornier avec un tournevis dynamométrique, geste lent et précis, atelier lumineux, 5 secondes, photoréaliste »
*Une action, un plan, 5 secondes : la règle d'or.*

**V3 — Visite de site (multi-prompt Kling, 3×5 s)**
> S1 : « plan large du local technique, porte qui s'ouvre, lumière qui s'allume » / S2 : « travelling le long des baies, LEDs qui défilent » / S3 : « arrêt sur l'onduleur, zoom lent sur l'afficheur »
*Le multi-prompt raconte ; garder « même local, même lumière » dans chaque segment.*

**V4 — Ambiance data center (Luma, atmosphérique)**
> « allée de data center dans une légère brume froide, nappes de lumière bleue, mouvement d'air subtil, caméra qui avance lentement, cinématographique »
*Le créneau Luma : l'atmosphère. Ne pas lui demander un geste précis.*

**V5 — Clip vitrine premium (Veo 3.1 Standard, 8 s)**
> « lever de soleil sur un site industriel, travelling aérien vers le bâtiment technique, reflets dorés sur les façades, musique non incluse, cinématographique, 8 secondes »
*8 secondes à 3,20 $ : on ne les dépense que pour du vitrine.*

**V6 — Brouillon storyboard (Hailuo H3, 6 s)**
> « schéma animé simplifié : flux d'énergie du réseau vers l'onduleur puis vers les baies, flèches lumineuses, fond sombre, style motion design »
*Hailuo pour valider l'idée à 0,70 $ avant de masteriser ailleurs.*

**V7 — Mouvement précis (Seedance 2.5, 10 s)**
> « technicien qui déroule un câble depuis un touret, geste ample et régulier, caméra fixe en plan moyen, atelier, 10 secondes »
*Seedance quand le geste doit être juste, pas juste « beau ».*

**V8 — Détail produit e-commerce (Kling, motion transfer)**
> Vidéo de référence : rotation produit sur plateau. Sujet : ton onduleur (photo). « applique le mouvement de rotation de la vidéo de référence au produit de l'image »
*Le motion transfer : le mouvement pro sans le studio.*

**V9 — Sensibilisation sécurité (Kling multi-prompt, 2×5 s)**
> S1 : « technicien qui pose un cadenas de consignation sur un disjoncteur, geste net » / S2 : « pancarte 'NE PAS MANŒUVRER' accrochée, zoom lent »
*Deux gestes, deux plans : la sécurité se montre, elle ne se raconte pas.*

**V10 — Teaser événement (Runway, Aleph + Gen-4.5)**
> Générer 3 plans (Gen-4.5), puis assembler et rythmer dans l'éditeur (Aleph) avec titrage.
*Runway se justifie quand tu restes dans sa suite de bout en bout.*

---

# PARTIE U — FAQ : 20 questions qu'on pose vraiment

## 177. Questions générales

**F1. Par quoi commencer si je n'ai jamais rien généré ?**
Par **Perplexity gratuit** (recherche, zéro risque) puis **FLUX.1 [schnell] en local ou via fal.ai** (~0,003 $/image) pour l'image, et le **plan gratuit de Kling** (66 crédits/jour) pour la vidéo. Tu auras tout testé pour moins d'1 $.

**F2. Lequel choisir entre abonnement et API ?**
Usage **manuel et régulier** → abonnement web. Usage **programmé / en volume / irrégulier** → API (officielle si volume, agrégateur si ponctuel). Voir sections 67-73.

**F3. C'est vraiment utilisable pour du travail sérieux, ou c'est du gadget ?**
Les deux. Gadget : générer « pour voir ». Sérieux : **doc technique illustrée, clips de formation, veille automatisée, maquettes avant travaux** — les 4 cas de la Partie K sont du travail réel, pas de la démo.

**F4. Combien de temps pour être autonome ?**
Une **demi-journée** par outil pour les bases (prompts + réglages + export), une **semaine d'usage réel** pour les réflexes (taux de rejet, coûts, limites). ComfyUI en local : compter une journée d'installation et de prise en main.

**F5. Et si je n'ai pas de GPU ?**
Image : API (BFL dès 0,014 $/image, fal.ai dès 0,003 $). Vidéo : 100 % cloud de toute façon. Recherche : 100 % cloud. Le GPU local n'est **obligatoire pour rien** — c'est un choix de confidentialité et de coût au volume.

## 178. Questions image

**F6. Midjourney ou FLUX pour mes docs techniques ?**
**FLUX** : plus neutre/documentaire, texte mieux géré (v2), local possible, 10× moins cher. Midjourney si tu veux du « beau » (com, affiches) plutôt que du « juste ».

**F7. Pourquoi mes textes générés sont-ils en charabia ?**
Parce que tu demandes un **long texte fin** à un modèle qui le « dessine » au lieu de l'écrire. Solutions : texte court entre guillemets, modèle fort en typo (Ideogram, Nano Banana Pro, FLUX.2), ou **texte ajouté en PAO après** (la vraie solution pro).

**F8. Comment garder le même équipement d'une image à l'autre ?**
FLUX.2 **multi-références** (3-6 photos de l'équipement en entrée), description verrouillée, seed noté. Au-delà de ~20 visuels : **LoRA** (section 173).

**F9. Les images gratuites sont-elles utilisables au travail ?**
**Non** : watermarks + interdiction d'usage commercial sur les plans gratuits (Kling, Luma...). Le gratuit sert à **évaluer**, pas à produire.

**F10. Quelle résolution pour quel usage ?**
Web/intranet : 1024² suffit. Présentation projetée : 2048². Print (affiche A3) : générer grand ou **upscaler** (section 172). Ne jamais générer en 4K « au cas où » (coût ×3-5).

## 179. Questions vidéo

**F11. Text-to-video ou image-to-video ?**
**Image-to-video** dès que tu as une bonne photo de départ (équipement réel, site réel) : plus crédible, moins d'aléas. Text-to-video pour l'imaginaire (ambiances, concepts).

**F12. Pourquoi ma vidéo est-elle « bizarre » (mains, visages, physique) ?**
Limites connues : gestes fins, mains, foules, textes longs, mouvements imposés au millimètre. Contourner : plans larges, actions simples, **une action par clip**, I2V depuis le réel, Seedance pour le geste précis.

**F13. L'audio natif vaut-il le coup ?**
Pour un dialogue ou une ambiance impossible à refaire : oui. Sinon **non** : +50 % à ×5 du coût pour un son que tu referas mieux en montage (voix off, bruitages libres).

**F14. Quelle durée par clip ?**
5-10 s : le standard (un geste, un plan). 15 s : mini-scénario (multi-prompt). 30 s : Seedance en un appel, ou **montage** de 3-4 clips (mieux : rythme contrôlé, plans variés).

**F15. Où monter les clips ?**
Pas dans le générateur : **DaVinci Resolve** (gratuit, pro), Premiere, ou même CapCut pour du simple. Le générateur fait les **plans**, le monteur fait le **film** (titres, voix, musique, rythme).

## 180. Questions recherche & API

**F16. Perplexity peut-il remplacer Google ?**
**Non** : il le **complète**. Perplexity dégrossit vite avec des sources ; Google reste roi pour la recherche obscure, les opérateurs fins (`site:`, dates, forums) et le gratuit. Les pros utilisent les deux.

**F17. L'API Sonar est-elle rentable pour une petite structure ?**
Oui : une veille hebdo coûte **~0,50 $/mois** en `sonar` (section 103). Le coût n'est jamais le problème ; la **qualité du prompt de veille** et la **relecture** le sont.

**F18. Jev remplace-t-il un LLM dans mon pipeline ?**
**Non** : il le **complète**. Le LLM produit (texte, code, résumé), Jev **vérifie/route/score** en 100-500 ms pour des fractions de centime. Pattern : cascade vérifiée (section 168).

**F19. Que faire quand un fournisseur change ses prix ?**
Re-calculer ton **coût par livrable** (section 5), comparer les 2-3 alternatives de ce guide, et migrer si l'écart dépasse ~30 %. Ne jamais migrer « parce que c'est moins cher au tarif facial » sans mesurer le taux de rejet.

**F20. Comment rester à jour sans y passer mes soirées ?**
Rituel trimestriel (1 h) : re-vérifier les 4-5 tarifs que tu utilises, lire les notes de version des modèles, parcourir la section « À venir » de ce guide. Le reste du temps : **utiliser** les outils, pas les suivre.

---

# PARTIE V — SCRIPTS COMPLETS PRÊTS À ADAPTER

## 181. Script : batch d'images doc via l'API BFL (avec suivi des coûts)

```python
#!/usr/bin/env python3
"""Génère une série de visuels documentaires via l'API BFL (UE), avec journal des coûts.
Usage : BFL_API_KEY=... python3 batch_bfl.py
Coût estimé : 0,03 $/image en flux-2-pro (1 MP) — voir section 19.
"""
import csv, os, time, requests
from datetime import datetime

BFL_KEY = os.environ["BFL_API_KEY"]
EU = "https://api.eu.bfl.ai"   # résidence UE
H = {"x-key": BFL_KEY, "Content-Type": "application/json"}
COUT_PAR_IMAGE = 0.03          # flux-2-pro, ~1 MP : adapter au modèle

JOBS = [
    # (nom_fichier, prompt, seed)
    ("tgbt_vue_ensemble",
     "photographie technique documentaire : armoire TGBT industrielle ouverte, "
     "disjoncteurs modulaires alignés, borniers, lumière neutre d'atelier, "
     "cadrage frontal, netteté maximale, style documentation constructeur", 201),
    ("tgbt_detail_borniers",
     "macrophotographie : borniers à vis sur rail DIN, câbles repérés par "
     "étiquettes, cuivre net, lumière latérale d'atelier, style documentation "
     "constructeur", 202),
    ("local_energie_isometrique",
     "illustration technique épurée en isométrique : local énergie avec onduleur "
     "40 kVA, armoire TGBT, climatisation de précision, fond clair, lignes fines "
     "bleu marine, sans texte, style manuel d'ingénierie", 203),
]

def generer(nom, prompt, seed, timeout_s=300):
    sub = requests.post(f"{EU}/v1/flux-2-pro", headers=H, timeout=30,
                        json={"prompt": prompt, "width": 1024, "height": 1024,
                              "seed": seed}).json()
    tid = sub["id"]
    t0 = time.time()
    while time.time() - t0 < timeout_s:
        res = requests.get(f"{EU}/v1/get_result", headers=H,
                           params={"id": tid}, timeout=30).json()
        if res["status"] == "Ready":
            url = res["result"]["sample"]
            data = requests.get(url, timeout=60).content
            path = f"{nom}.png"
            with open(path, "wb") as f:
                f.write(data)
            return path
        if res["status"] in ("Error", "Failed"):
            raise RuntimeError(f"Échec {tid} : {res}")
        time.sleep(3)
    raise TimeoutError(tid)

journal = []
for nom, prompt, seed in JOBS:
    try:
        path = generer(nom, prompt, seed)
        journal.append([datetime.now().isoformat(), nom, "flux-2-pro", seed,
                        "OK", COUT_PAR_IMAGE, path, prompt[:80]])
        print(f"OK  {nom} -> {path}")
    except Exception as e:   # on continue le batch malgré un échec
        journal.append([datetime.now().isoformat(), nom, "flux-2-pro", seed,
                        f"ECHEC: {e}", 0, "", prompt[:80]])
        print(f"KO  {nom} : {e}")

with open("journal_batch.csv", "a", newline="") as f:
    csv.writer(f).writerows(journal)
total = sum(float(l[5]) for l in journal)
print(f"Terminé : {len(journal)} jobs, coût estimé {total:.2f} $ — voir journal_batch.csv")
# Rappel : relire chaque visuel (textes !) avant usage — piège n°11.
```

## 182. Script : digest de veille multi-sources (Sonar + déduplication)

```python
#!/usr/bin/env python3
"""Digest hebdo : interroge Sonar sur N sujets, déduplique les URLs déjà vues.
Usage : PERPLEXITY_API_KEY=... python3 veille.py
État : urls_vues.txt conserve l'historique (ne jamais alerter 2x sur la même URL).
"""
import os, re
from openai import OpenAI

client = OpenAI(api_key=os.environ["PERPLEXITY_API_KEY"],
                base_url="https://api.perplexity.ai")
VU = "urls_vues.txt"
vues = set(open(VU).read().splitlines()) if os.path.exists(VU) else set()

SUJETS = [
    "nouveautés onduleurs triphasés 10 à 120 kVA",
    "batteries lithium-ion pour onduleurs : sécurité et rappels",
    "failles CVE équipements d'énergie et building management connectés",
    "normes électriques NF C 15-100 / NF C 18-510 / IEC 62040 : évolutions",
]

def ask(s):
    r = client.chat.completions.create(
        model="sonar",
        messages=[{"role": "user",
                   "content": f"Actualités des 7 derniers jours : {s}. "
                              "Format : une puce par info, avec titre, 1 ligne de "
                              "résumé, et l'URL source entre chevrons <url>. "
                              "Maximum 6 puces. Si rien de notable : 'RAS'."}])
    return r.choices[0].message.content

nouvelles_urls = set()
sortie = []
for s in SUJETS:
    texte = ask(s)
    urls = set(re.findall(r"<(https?://[^>]+)>", texte))
    fraiches = urls - vues
    nouvelles_urls |= urls
    if fraiches:
        sortie.append(f"## {s}\n" + "\n".join(
            l for l in texte.splitlines()
            if not any(u in l for u in (urls - fraiches)) or "RAS" in l))
    else:
        sortie.append(f"## {s}\nRAS ou déjà signalé cette semaine.")

with open(VU, "a") as f:
    f.write("\n".join(sorted(nouvelles_urls - vues)) + "\n")

print("VEILLE ÉNERGIE — " + __import__("datetime").date.today().isoformat())
print("\n\n".join(sortie))
print(f"\n---\nURLs déjà connues ignorées : {len(vues)} en historique.")
# À brancher sur cron le lundi 7h + envoi mail (voir section 103).
```

## 183. Script : contrôle qualité d'un dossier de visuels (Jev)

```python
#!/usr/bin/env python3
"""Passe chaque visuel d'un dossier au crible Jev (métadonnées textuelles).
Usage : JEV_API_KEY=... python3 qc_visuels.py ./livrables
Principe : Jev ne voit pas l'image, il juge sa fiche (prompt, modèle, relecture).
"""
import json, os, sys, requests

JEV = "https://api.typesafe.ai/v1/systemone"
H = {"Authorization": f"Bearer {os.environ['JEV_API_KEY']}",
     "Content-Type": "application/json"}
SEUIL = 0.70

def check(fiche):
    r = requests.post(JEV, headers=H, timeout=20, json={
        "model": "jev-1.13.0",
        "state": fiche,
        "questions": {
            "licence_ok": {"type": "noul", "question":
                "La licence du modèle cité autorise-t-elle l'usage commercial ?"},
            "risque": {"type": "choice", "question":
                "Quel est le risque principal de ce visuel ?",
                "options": ["aucun", "texte_douteux", "personne_reconnaissable",
                            "marque_visible", "contenu_sensible"]},
        }}).json()
    lic = r["licence_ok"]["p_yes"] >= SEUIL
    risq = r["risque"]
    ok = lic and risq["choice"] == "aucun" and risq["confidence"] >= SEUIL
    return ok, {"licence_p": round(r["licence_ok"]["p_yes"], 3),
                "risque": risq["choice"],
                "confiance": round(risque_conf(risque), 3)}

def risque_conf(x):
    return x.get("confidence", 0)

dossier = sys.argv[1]
# Convention : chaque visuel a sa fiche JSON {prompt, modele, licence, seed, relecture}
for fn in sorted(os.listdir(dossier)):
    if not fn.endswith(".json"):
        continue
    fiche = json.load(open(os.path.join(dossier, fn)))
    texte = (f"Visuel {fn}: prompt='{fiche['prompt']}', modèle={fiche['modele']} "
             f"(licence {fiche['licence']}), seed={fiche['seed']}. "
             f"Relecture humaine : {fiche['relecture']}")
    ok, det = check(texte)
    print(("PASS " if ok else "REVIEW ") + fn, det)
# Coût : ~0,0004 $/visuel. Les REVIEW repartent en relecture humaine.
```

## 184. Matrice de décision fournisseur (à remplir pour TON cas)

Recopie ce tableau, mets tes chiffres, décide en 10 minutes :

| Critère (poids) | Option A : ______ | Option B : ______ | Option C : ______ |
|-----------------|-------------------|-------------------|-------------------|
| Coût / livrable accepté (×3) | | | |
| Qualité sur mon besoin (×3) | | | |
| Confidentialité des données (×2) | | | |
| Vitesse / file d'attente (×1) | | | |
| Intégration API (×2) | | | |
| Licence commerciale claire (×2) | | | |
| Risque fournisseur (×1) | | | |
| **Score pondéré** | | | |

Règle : le « coût / livrable accepté » se **mesure** (2 semaines de test avec compteur), il ne se lit pas sur une page tarifaire. Le gagnant du tableau n'est valable que pour **ton** besoin : refaire l'exercice par besoin (doc, vidéo, veille), pas une fois pour tout.

## 185. Fiche réflexe imprimable (1 page — à afficher)

```
┌─────────────────────────────────────────────────────────┐
│  IA GÉNÉRATIVE — FICHE RÉFLEXE (v1.0, 27/09/2026)       │
├─────────────────────────────────────────────────────────┤
│ AVANT DE GÉNÉRER                                        │
│ □ Bon outil ? (pense-bête §105)  □ Brouillon pas cher   │
│ □ Rien de confidentiel (§78)     □ Budget OK (×3 essais)│
│ □ Licence commerciale OK (§74)   □ Pas de personne réelle│
│                                                         │
│ AVANT DE PUBLIER                                        │
│ □ Textes relus caractère par caractère                  │
│ □ Cohérence de la série vérifiée                        │
│ □ Pas de watermark gratuit                              │
│ □ Mention "généré par IA" si externe                    │
│ □ Prompts + seeds archivés                              │
│                                                         │
│ EN PROD (API)                                           │
│ □ Clé en variable d'env, 1 clé/usage, alertes ON        │
│ □ Version pinnée, timeouts, logs (modèle+coût)          │
│ □ Webhook > polling  □ Plan B fournisseur identifié     │
│                                                         │
│ TARIFS REPÈRES (§110) : image 0,003-0,134 $ · vidéo     │
│ 10 s 1080p 1-4 $ · Sonar ~1 $/1M · Jev 0,042 $/1M      │
│                                                         │
│ URGENCES : clé fuitée → rotation immédiate. Facture     │
│ anormale → couper la clé, lire les logs, comprendre.    │
└─────────────────────────────────────────────────────────┘
```

## 186. Mot de la fin : l'outillage ne remplace pas le métier

Ce guide t'a donné les prix, les API, les licences et les pièges de l'IA générative image/vidéo/recherche au 27/09/2026. Mais l'outil ne fait pas l'électricien, pas plus que la perceuse ne fait le menuisier :

- **L'IA propose, tu disposes** : chaque visuel, chaque clip, chaque valeur sourcée passe par ta validation. C'est toi qui signes la doc, pas le modèle.
- **Le coût se mesure, il ne se lit pas** : deux semaines de compteur valent mieux que dix comparatifs.
- **La confidentialité ne se négocie pas** : local pour le sensible, UE pour l'interne, cloud US pour le public — et jamais l'inverse.
- **Le fournisseur peut disparaître** (Sora, 24/09/2026) : interface interne, versions pinnées, plan B.

Avec ~670 $/an (section 72), un GPU correct et les réflexes de ce guide, ton service produit des docs illustrées, des clips de formation et une veille automatique — sans dépendre d'un prestataire, sans exposer tes données, et sans mauvaise surprise sur la facture. Le reste, c'est de la pratique : génère, rate, mesure, recommence.

---

# PARTIE W — APPROFONDISSEMENTS

## 187. Kling vs Veo vs Runway : qui gagne, selon le scénario

| Scénario | Gagnant | Pourquoi |
|----------|---------|----------|
| 20 clips/mois pour intranet/formation | **Kling 3.0 (Pro web)** | 25,99 $/mois, qualité suffisante, audio natif en option |
| 1 clip vitrine pour appel d'offres | **Veo 3.1 Standard** | 3,20 $ les 8 s : le réalisme se paie, une fois |
| Série avec montage, effets, voix | **Runway** | Suite complète (Aleph, voix), même si le €/seconde pique |
| Ambiance (brume, eau, feu) | **Luma Ray 3.2** | Personne ne fait mieux l'atmosphérique |
| Geste technique précis à montrer | **Seedance 2.5** | Le plus obéissant sur le mouvement |
| Valider une idée en 2 min | **Hailuo H3** | 0,70 $ les 6 s, rendu en quelques secondes |
| Pipeline 100 % API, multi-modèles | **fal.ai / Replicate** | Un compteur, zéro pack prépayé |

## 188. Dictionnaire des paramètres (ceux qui changent vraiment le résultat)

**Image (FLUX/Midjourney)** : `steps` (20-50 dev, 1-4 schnell), `guidance/CFG` (3-7 : fidélité au prompt), `seed` (reproductibilité), `aspect_ratio` (choisir **avant**, pas après — recadrer une image générée, c'est la dénaturer), `negative_prompt` (défauts à bannir), `--style raw` (Midjourney : moins d'embellissement), `--stylize` (plus ou moins de « patte » MJ).

**Vidéo (Kling/Veo/Runway)** : `duration` (5/8/10/15 s : **une action par clip**), `resolution` (720p brouillon → 1080p master → 4K exception), `audio` (natif ou non : voir le coût), `camera` (dans le prompt : travelling, fixe, drone), `start/end frame` (I2V : ancrage), `multi_prompt` (segments temporisés).

**Recherche (Perplexity/Sonar)** : `model` (sonar éco vs sonar-pro profond), `search_recency` (filtrer par fraîcheur quand l'API le permet), `max_tokens` (borner le coût), périmètre de sources (web / académique / social).

**Décision (Jev)** : `model` pinné (`jev-1.13.0`), `seuil` de probabilité/confiance (0,70-0,80 selon le risque), `state` en anglais, questions **atomiques** (une question = un jugement).

## 189. Dix erreurs de prompting et leurs corrections

1. **Trop vague** : « un onduleur » → **précis** : « onduleur triphasé 40 kVA en armoire 19 pouces, façade avec afficheur LCD bleu, voyants verts ».
2. **Deux actions** : « le technicien ouvre l'armoire puis remplace le module » → **deux clips** (ou multi-prompt).
3. **Pas de caméra** : ajouter « plan moyen, léger travelling latéral » (vidéo) / « cadrage frontal, 35mm » (image).
4. **Pas de lumière** : « lumière neutre d'atelier 5000K » change tout.
5. **Style absent** : « style documentation constructeur » vs « photoréaliste » vs « illustration épurée » — trois résultats radicalement différents.
6. **Texte long dans l'image** : réduire à 2-3 mots entre guillemets, ou passer en PAO.
7. **Négatif oublié** : ajouter « pas de texte flou, pas de filigrane, pas de mains déformées ».
8. **Contradictions** : « de nuit » + « lumière du jour éclatante » → le modèle choisit au hasard. Relire son prompt.
9. **Unités floues** : « grand » → « 2 mètres de haut » ; les modèles comprennent mal les dimensions absolues mais mieux que le vague.
10. **On ne décrit pas l'évident** : le modèle ne sait pas ce qu'est « un TGBT normal » — décrire comme à un stagiaire de première semaine.

## 190. Roadmap d'adoption 90 jours (service systèmes & énergies)
**Semaines 1-2 — Découverte (budget : ~10 $)**
- Perplexity gratuit : 5 recherches Pro/jour sur tes sujets métier ; ouvrir systématiquement les sources.
- Kling gratuit : 3-4 clips tests (I2V depuis tes photos) ; noter le taux de rejet.
- fal.ai : 20 images FLUX schnell (~0,06 $) pour sentir le prompting.

**Semaines 3-4 — Cadrage**
- Choisir 1 besoin pilote (ex. illustrer une procédure réelle).
- Tester FLUX local (si GPU) ou API BFL ; mesurer le coût/livrable réel.
- Rédiger ta fiche réflexe (section 185) adaptée à ton équipe.

**Mois 2 — Production pilote**
- Produire la doc pilote complète (visuels + relecture + archivage prompts/seeds).
- Si vidéo : Kling Pro 1 mois (25,99 $), 3-6 clips, montage, diffusion intranet.
- Mettre en place la veille Sonar hebdo (script section 182 ou Space).

**Mois 3 — Industrialisation**
- Batch ComfyUI ou script BFL pour les séries ; LoRA si besoin d'identité verrouillée.
- Revue des coûts : coût/livrable vs estimé, ajustement des abonnements.
- Partage : Space Perplexity d'équipe, bibliothèque de prompts validés, rituel trimestriel de re-vérification des tarifs.

**Jalon 90 jours** : 1 doc illustrée livrée, 3+ clips en ligne, 12 digests de veille envoyés, coût total < 150 $. Si ces 4 cases sont cochées, l'outillage est rentré dans les mœurs — sinon, c'est le besoin pilote qui était mal choisi, pas les outils.

## 191. Cinq projets types budgétés de bout en bout

Tous les montants : tarifs vérifiés le 27/09/2026, hypothèse taux de rejet ×3 sur les brouillons.

**Projet A — Illustrer une procédure de 20 pages (15 visuels documentaires)**
- Brouillons : 45 images FLUX schnell via fal.ai ≈ 0,14 $
- Masters : 15 images flux-2-pro via BFL ≈ 0,45 $
- Relecture/rejetons PAO : 2 h humaines
- **Total : < 1 $ + 2 h.** Oui, moins d'un dollar.

**Projet B — Clip de formation de 2 minutes (8 clips de 10 s assemblés)**
- Brouillons : 24 clips Kling Standard ≈ 24 × 0,35 $ ≈ 8,40 $
- Masters : 8 clips Kling Pro 1080p ≈ 8 × 1,12 $ ≈ 9 $
- Montage + voix off : 1 journée humaine (DaVinci Resolve)
- **Total : ~18 $ + 1 jour.**

**Projet C — Veille hebdomadaire pendant 1 an (52 digests)**
- Sonar éco : 4 requêtes × 52 semaines ≈ 208 requêtes ≈ **~2-5 $/an**
- Envoi mail : 0 $ (script + SMTP existant)
- Relecture : 15 min/semaine
- **Total : < 5 $/an.** Le poste le moins cher de tout ce guide.

**Projet D — Catalogue produit : 30 visuels « identité verrouillée »**
- Option multi-références : 90 images Nano Banana Pro ≈ 90 × 0,134 $ ≈ 12 $
- Option LoRA local : 1 journée de setup GPU + électricité, puis 0 $/image
- **Seuil** : au-delà de ~30 visuels récurrents du même équipement, le LoRA local gagne.

**Projet E — Bilan annuel d'un service outillé (ordre de grandeur)**
- Perplexity Pro : 240 $/an · Kling Pro : 312 $/an · Veille Sonar : ~5 $/an
- Images API (500 masters/an) : ~15 $/an · 2-3 clips Veo premium : ~10 $/an
- **Total : ~580-670 $/an** — cohérent avec la section 72.

## 192. Dernière checklist : avant de fermer ce guide

- [ ] J'ai noté les **4-5 tarifs** que j'utiliserai vraiment (sections 19, 44, 46, 57, 66, 103, 110).
- [ ] J'ai choisi mon **résidence de données** par niveau de sensibilité (section 78).
- [ ] Mes clés API sont en **variables d'environnement**, 1 clé par usage, alertes activées (section 89).
- [ ] J'ai un **budget mensuel** et une règle d'arrêt (sections 73, 166).
- [ ] Je sais quoi faire quand un fournisseur **change ses prix ou ferme** (sections 88, 168-169).
- [ ] Mon premier besoin pilote est **défini et mesurable** (section 190, semaines 3-4).
- [ ] Je sais que les prix bougent : **re-vérification fin décembre 2026** inscrite à l'agenda.
- [ ] J'ai imprimé la **fiche réflexe** (section 185) pour l'afficher au bureau.

*Si les 8 cases sont cochées : tu es prêt. Sinon, retourne aux sections indiquées — c'est pour ça qu'elles existent.*

## 193. À venir — annonces vérifiées au 27/09/2026

Cette section est la synthèse « radar » du guide : tout ce qui est listé ci-dessous a été **vérifié le 27/09/2026** via recherche web. Le détail sourcé est en PARTIE P (sections 156-158) ; ici, l'essentiel actionnable.

**Annonces confirmées (impact direct sur tes choix)**

1. **Kling 3.0 est la version de référence** (sortie 15/09/2026) : la famille 2.x reste en API mais la doc et les nouveautés (audio natif, motion transfer, multi-prompt) portent sur la 3.0. Si tu démarres aujourd'hui, démarre en 3.0.
2. **FLUX.2 est la génération courante** (Pro/Max/Flex/Dev/Klein), avec Klein 4B sous Apache 2.0 pour le local gratuit. FLUX.1 reste pertinent en local léger (Schnell).
3. **Sora 2 fermé le 24/09/2026** (Videos API incluse) : aucun nouveau projet ne doit en dépendre ; les projets existants doivent migrer (Kling, Veo 3.1, Runway).
4. **Nano Banana Pro (Gemini 3 Pro Image)** : le nouveau haut de gamme image Google (~0,134 $/image), avec génération de texte dans l'image au niveau design.
5. **Jev / System One (TypeSafe AI)** : sorti de stealth le 15/09/2026 (`jev-1.13.0`) — décision sous la seconde à 0,042 $/1M, à intégrer comme **garde-fou** de tes pipelines (sections 119, 168, 183).
6. **Hailuo H3** : MiniMax a divisé ses prix par ~3 (0,70 $ les 6 s) — le brouillon vidéo le moins cher du marché au 27/09/2026.
7. **Runway Gen-4.5** (10/09/2026) : nouveau standard de la suite ; offre Entreprise/Éducation sans tarif public — devis uniquement.
8. **Veo 3.1** (août 2026) : Fast à 0,40 $/8 s pour les brouillons, Standard à 3,20 $/8 s pour le premium — la fourchette la plus lisible du marché vidéo.

**Rumeurs explicitement signalées comme telles (ne pas décider dessus)**

- **FLUX 3 vidéo** : signal faible uniquement, aucune annonce. Si confirmé, il rebattrait les cartes image+vidéo en local.
- **Midjourney V8.2** : attendu d'après le rythme historique (~6 mois après V7), non confirmé.
- **GPT Image 2 / DALL-E 4** : rien d'officiel ; le positionnement actuel d'OpenAI image reste l'édition dans l'écosystème.
- **Réouverture d'une API vidéo OpenAI** : rien n'indique un remplaçant de Sora 2 à court terme.

**Ce que ça change pour ta roadmap (section 190)**

- Verrouille tes choix 2026 sur : **Kling 3.0** (vidéo), **FLUX.2** (image), **Sonar** (veille), **Jev** (contrôle).
- Garde une **ligne budgétaire « prime au premium »** (~10-15 $/an) pour 2-3 visuels/clips Veo/Nano Banana Pro par an : c'est là que le haut de gamme se justifie.
- Re-vérification des tarifs : **fin décembre 2026** — c'est le seul rendez-vous calendaire que ce guide t'impose.

---

*Fin du guide — IA générative : image, vidéo, recherche (v1.0, 27/09/2026).*
*Prochaine re-vérification conseillée des tarifs : fin décembre 2026.*

---

## Note de version

- **v1.0 — 27/09/2026** : version initiale. 193 sections, 7 scripts Python prêts à adapter, 20 prompts commentés, 20 pièges, 20 questions FAQ, quiz 10 questions, 5 projets budgétés.
- Tous les tarifs et versions ont été vérifiés par recherche web le **27/09/2026** ; les points incertains sont marqués **« à vérifier »** dans le texte.
- Historique des modifications : à tenir ici à chaque re-vérification trimestrielle (date, ce qui a changé, sections impactées).
