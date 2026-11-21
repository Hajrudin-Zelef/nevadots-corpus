---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-granary-v2-datasets-at-hugging-face
title: "nvidia/Granary-v2 · Datasets at Hugging Face"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["speech", "cc-by-4", "hours", "labels", "source", "pipeline", "voxpopuli", "granary-v2"]
source: https://huggingface.co/datasets/nvidia/Granary-v2?ref=explainx
source_anchor: ""
source_lines: [1, 282]
section: "NVIDIA_Hardware"
---
**Granary-v2** is NVIDIA's second release of generated labels and instruction pairs
for multilingual speech. It re-processes a broad set of source corpora through
an entirely new labelling pipeline that produces not just transcripts and translations, but
inverse text normalization, captioning, code-switching and speech-QA targets, plus
ready-to-train instruction pairs.

**The pipeline source is already public.** All of it lives in
NeMo Curator.
The full end-to-end runs and the per-stage scripts will follow soon.


- **48 languages** across**16 source corpora**
- Labels for **419,974 hours** of source speech
- **Quality-filtered** : utterances the pipeline flagged for removal (hallucinations, wrong
language, empty transcripts after cleaning, abnormal speaking rate) are not included;`high_quality` is a separate, stricter gate on what remains
- **154,961 hours** flagged high-quality by the pipeline's own gate
- **89,353 hours** of X→English speech translation
- **39,317 hours** of English→X speech translation, each record fanned out
to 31 targets at once (`ar` ,`bg` ,`cs` ,`da` ,`de` ,`el` ,`es` ,`et` ,`fi` ,`fr` ,`he` ,`hi` ,`hr` ,`hu` ,`it` ,`ja` ,`ko` ,`lt` ,`lv` ,`mt` ,`nl` ,`pl` ,`pt` ,`ro` ,`ru` ,`sk` ,`sl` ,`sv` ,`th` ,`uk` ,`zh` )
- **Six text targets per utterance** : normalized transcript, ITN, captioning,
code-switched, speech-QA, and translation

**Update, November 2026.** The first upload of this release also contained 5,740,042 utterances (43,088 hours) that the pipeline had flagged for removal, with no flag in the schema to tell them apart. Most are English speech inside non-English VoxPopuli leaves; the rest failed language-ID, hallucination, empty-transcript or speaking-rate checks. They have been removed and every number on this card describes the cleaned release. The earlier files remain available at revision `81402b75`.


Arabic, Assamese, Bengali, Bulgarian, Chinese, Croatian, Czech, Danish, Dutch, English, Estonian, Filipino, Finnish, French, German, Greek, Gujarati, Hebrew, Hindi, Hungarian, Indonesian, Italian, Japanese, Kannada, Korean, Latvian, Lithuanian, Malayalam, Maltese, Marathi, Odia, Persian, Polish, Portuguese, Punjabi, Romanian, Russian, Slovak, Slovenian, Spanish, Swedish, Tamil, Telugu, Thai, Turkish, Ukrainian, Urdu, Vietnamese.

| Corpus | Languages | Hours | High-quality h | AST (X→en) h | AST (en→X) h | Source corpus license | 
|---|---|---|---|---|---|---|
| **VoxPopuli (MOSEL labels)** (`voxpopuli` ) | 23 | 338,011 | 112,877 | 81,961 | 8,974 | CC0 / CC-BY-4.0 labels | 
| **LibriLight** (`librilight` ) | 1 | 49,283 | 25,898 | 0 | 25,724 | MIT | 
| **People's Speech** (`people_speech` ) | 1 | 8,593 | 3,807 | 0 | 826 | CC BY ( `clean` partition) | 
| **Multilingual LibriSpeech** (`mls` ) | 8 | 7,773 | 3,070 | 1,844 | 103 | CC-BY-4.0 | 
| **FalAR** (`falar` ) | 1 | 4,703 | 1,605 | 1,602 | — | CC-BY-4.0 | 
| **Common Voice 26.0** (`mcv26` ) | 39 | 3,834 | 2,832 | 2,365 | 495 | CC0-1.0 | 
| **National Speech Corpus** (`nsc` ) | 1 | 2,723 | 1,647 | 0 | 1,650 | Singapore Open Data Licence | 
| **VoxPopuli (re-labelled)** (`voxpopuli_improved` ) | 16 | 1,270 | 526 | 561 | 160 | CC0 / CC-BY-4.0 labels | 
| **Golos** (`golos` ) | 1 | 1,111 | 715 | 696 | — | Custom (Salute Developers) | 
| **Europarl-ASR** (`europarl` ) | 1 | 1,020 | 605 | 0 | 613 | CC-BY-4.0 | 
| **LibriSpeech-PC** (`librispeech_pc` ) | 1 | 953 | 953 | 0 | 686 | CC-BY-4.0 | 
| **FLEURS** (`fleurs` ) | 48 | 391 | 209 | 223 | 5 | CC-BY-4.0 | 
| **Dusha** (`dusha` ) | 1 | 147 | 104 | 102 | — | Custom (Salute Developers) | 
| **VCTK** (`vctk` ) | 1 | 82 | 76 | 0 | 76 | ODC-By 1.0 | 
| **AMI Meeting Corpus** (`ami` ) | 1 | 69 | 24 | 0 | 4 | CC-BY-4.0 | 
| **Google Speech Commands** (`google_speech_commands` ) | 1 | 12 | 12 | 0 | 1 | CC-BY-4.0 | 
| **Total** | **48** | **419,974** | **154,961** | **89,353** | **39,317** |  | 

Totals describe only the manifests in this public release. ASR hours and translation into English are calculated from the reported corpus durations. Translation targets do not add audio hours.

The table lists source-corpus licenses. The released labels and instruction pairs are governed by OpenMDW-1.1, except for the `people_speech` subset, which is governed by CC BY-SA 4.0. See License for the subset mapping.

This repository contains label manifests only. Obtain audio from the linked source distributors under their applicable licenses and access terms.

The commands below obtain source corpora from their distributors. Check the source licenses and access terms before downloading. Loading `nvidia/Granary-v2` downloads label manifests only; obtain source audio separately.

This repository ships **manifests only** — no audio travels with it. Every
`audio_filepath` is instead a self-describing identifier:

```
data_<lang>_<corpus>_<source path, with "/" replaced by "_">
```
So `data_de_mcv26_cv-corpus-26.0-2026-06-12_de_clips_common_voice_de_21137340.opus`
is `cv-corpus-26.0-2026-06-12/de/clips/common_voice_de_21137340.opus` in the Common
Voice 26.0 release. Fetch a corpus from its own distributor, then join on that
suffix.

Source audio remains subject to its distributor's terms. See the Source corpus license column in Hours by corpus. See License for the terms that apply to the released labels and instruction pairs.

The four routes below are ordered by how much work the download takes. Open the one your corpus is in.

| **1 · One HF command**`people_speech` · 8,593 h`falar` · 4,703 h`fleurs` · 391 h | **2 · Direct download**`librilight` · 49,283 h`mls` · 7,773 h`golos` · 1,111 h`librispeech_pc` · 953 h`dusha` · 147 h`vctk` · 82 h`google_speech_commands` · 12 h | **3 · Corpus tooling**`voxpopuli` · 338,011 h`voxpopuli_improved` · 1,270 h`europarl` · 1,020 h`ami` · 69 h | **4 · Gated access**`mcv26` · 3,834 h`nsc` · 2,723 h | 

## **1 · One Hugging Face command**

```
pip install -U "huggingface_hub[cli]" datasets
```
| Corpus | Hours | How | 
|---|---|---|
| **People's Speech** (`people_speech` ) | 8,593 | `hf download MLCommons/peoples_speech --repo-type dataset --include "clean/*"` — we release labels for the government-origin subset of`clean` . The released manifests are governed by CC BY-SA 4.0. Obtain source audio under the distributor's terms. | 
| **FalAR** (`falar` ) | 4,703 | `load_dataset("inesc-id/FalAR")` — the audio is embedded in the Parquet shards, so load it rather than fetching files | 
| **FLEURS** (`fleurs` ) | 391 | `load_dataset("google/fleurs", "ar_eg")` — the config is`<lang>_<region>` | 

## **2 · Direct download**

| Corpus | Hours | Archive | 
|---|---|---|
| **LibriLight** (`librilight` ) | 49,283 | `https://dl.fbaipublicfiles.com/librilight/data/large.tar` (3.06 TB) — we use the**`large`** 60k-hour segmentation;`small` and`medium` sit alongside it in the repo | 
| **Multilingual LibriSpeech** (`mls` ) | 7,773 | `https://dl.fbaipublicfiles.com/mls/mls_<lang>.tar.gz` for`english` ,`german` ,`dutch` ,`french` ,`spanish` ,`italian` ,`portuguese` ,`polish` — an`_opus` variant of each exists; full index at OpenSLR 94 | 
| **Golos** (`golos` ) | 1,111 | `https://cdn.chatwm.opensmodel.sberdevices.ru/golos/train_crowd{0..9}.tar` (127 GB total) — we use the**crowd** partition, and`crowd9` carries the manifests. Whole set in Opus:`golos_opus.tar` | 
| **LibriSpeech-PC** (`librispeech_pc` ) | 953 | The audio is plain LibriSpeech: `https://www.openslr.org/resources/12/train-{clean-100,clean-360,other-500}.tar.gz` . OpenSLR 145 adds only the punctuated and capitalised text | 
| **Dusha** (`dusha` ) | 147 | `https://cdn.chatwm.opensmodel.sberdevices.ru/dusha/crowd.tar` (30 GB) —**crowd only** ; the podcast partition ships no audio for licensing reasons and we use none of it | 
| **VCTK** (`vctk` ) | 82 | `https://datashare.ed.ac.uk/bitstream/handle/10283/3443/VCTK-Corpus-0.92.zip` (11.7 GB) | 
| **Google Speech Commands** (`google_speech_commands` ) | 12 | `http://download.tensorflow.org/data/speech_commands_v0.02.tar.gz` (2.4 GB) | 

## **3 · Needs the corpus's own tooling**

**VoxPopuli** — the two VoxPopuli leaves come from the same repository but
**different subsets**, so they take different commands. `voxpopuli` (338,011 h) is
the **v2 unlabelled** release, carrying the MOSEL pseudo-labels that seeded ours
(`FBK-MT/mosel`); `voxpopuli_improved`
(1,270 h) is the transcribed ASR subset, re-labelled by us.

Mind the `_v2` suffix. `--subset bg` is the v1 release and is roughly **4× smaller**
than the `bg_v2` our manifests were built from.


```
git clone https://github.com/facebookresearch/voxpopuli && cd voxpopuli
pip install -r requirements.txt
# `voxpopuli` -> the v2 UNLABELLED subset, per language ("400k" fetches all 23 at once)
python -m voxpopuli.download_audios     --root ./data --subset bg_v2
python -m voxpopuli.get_unlabelled_data --root ./data --subset bg_v2
# `voxpopuli_improved` -> the transcribed ASR subset
python -m voxpopuli.download_audios --root ./data --subset asr
python -m voxpopuli.get_asr_data    --root ./data --lang cs
```
**Europarl-ASR** (`europarl`, 1,020 h) — clone the MLLP repository and run its fetch
scripts:

```
git clone https://www.mllp.upv.es/git-pub/ggarces/Europarl-ASR.git
```
**AMI** (`ami`, 70 h) — download through the
Lhotse recipe
with the **IHM** (individual headset microphone) setting; every record we ship is an
individual headset channel, so SDM/MDM audio will not align.

```
pip install lhotse && lhotse download ami --mic ihm ./ami
```
## **4 · Gated access**

| Corpus | Hours | What it takes | 
|---|---|---|
| **Common Voice 26.0** (`mcv26` ) | 3,834 | Download the per-language `cv-corpus-26.0-2026-06-12` archive from the Common Voice portal (email-gated); the`mozilla-foundation/common_voice_*` mirrors on the Hub lag the latest release | 
| **National Speech Corpus** (`nsc` ) | 2,723 | Request access from IMDA Singapore — there is no direct download | 

```
nvidia/Granary-v2/
└── <language>/                 # ISO 639-1 codes
    └── <corpus>/
        └── <lang>_asr.jsonl.gz
```
One or more manifest files per (language, corpus). Large manifests can be split into shards.

Unlike Granary-v1, there is no separate `<lang>_ast-en.jsonl`: every translation travels
inline in each record's `translations` field, alongside its quality-estimation score. The
manifest files for each (language, corpus) carry both the ASR and the AST targets.

`translations` is keyed by target language, and the release runs in **both directions**.
Non-English audio is translated into English, which is the `AST (X→en) h` column. English
audio is translated the other way, into 31 targets at once (`ar`, `bg`, `cs`, `da`, `de`, `el`, `es`, `et`, `fi`, `fr`, `he`, `hi`, `hr`, `hu`, `it`, `ja`, `ko`, `lt`, `lv`, `mt`, `nl`, `pl`, `pt`, `ro`, `ru`, `sk`, `sl`, `sv`, `th`, `uk`, `zh`) — that is the
`AST (en→X) h` column, and it is why corpora like LibriLight and VCTK show no X→English
hours yet still carry translation.

Coverage is per-record, and a present key is **not** a usable translation: the pipeline
writes a stub with an empty `translation_raw` where its quality estimator rejected the
candidate. **Test `translation_raw` for non-emptiness** rather than assuming a key means
a translation. Both table columns count distinct audio, once per record however many
targets it carries, so neither double-counts and the two never sum past a corpus's hours.

These examples load the released label manifests. Obtain the source audio separately as described in Getting the audio. The `people_speech` records retain their CC BY-SA 4.0 license when loaded through the combined `en` configuration; see License.

```
from datasets import load_dataset
# 🌍 Language-level access (every corpus for that language)
ds = load_dataset("nvidia/Granary-v2", "de")
# 🎯 Corpus-specific access, named <lang>_<corpus>
ds = load_dataset("nvidia/Granary-v2", "de_mcv26")
ds = load_dataset("nvidia/Granary-v2", "en_librilight")
ds = load_dataset("nvidia/Granary-v2", "ja_mcv26")
# 📡 Streaming for large configs
ds = load_dataset("nvidia/Granary-v2", "en", streaming=True)
```
```
{
    "audio_filepath": str,            # Identifier used to locate source audio
    "duration": float,                # Duration in seconds
    "corpus": str,                    # source corpus name
    "source_lang": str,               # Source language code, e.g. "de"
    "tn_raw": str,                    # Primary transcript, text-normalized with P&C
    "itn_raw": str,                   # Inverse-text-normalized transcript
    "itn_clean": str,                 # ITN transcript after cleanup
    "captioning_text": str,           # Natural-language description of the audio
    "code_switched_text": str,        # Transcript with code-switching preserved
    "speech_qa_text": str,            # Q/A pair grounded in the audio ("SKIP" if N/A)
    "context_asr": dict | None,       # Coarse/fine context terms and entity categories
    "translations": dict,             # {"en": {"translation_raw": str,
                                      #          "translation_quality_score": float}}
    "preference_instructions": list,  # [{"prompt": str, "target": str, "tags": {}}, ...]
    "high_quality": bool | None,      # pipeline quality gate -- see note below
}
```
The pipeline uses 16 kHz mono audio for labeling. Source distributors can provide audio in other formats.

**On `high_quality`.** The flag records the pipeline's quality check.
154,961 hours in this public release pass it. Context and
translation coverage vary by record. Early translation runs also included
records outside the high-quality subset. Check each target for a usable value.


`preference_instructions` is a **derived training view**, not a core target: it packs
the fields above into ready-to-train prompt/target pairs for instruction tuning. The
pairs add no information beyond the other fields, so ignore them if you are training a
plain ASR or AST model.

This dataset is governed by the OpenMDW License Agreement, version 1.1, except for the People_Speech subset, which is governed by the Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0).

- **OpenMDW-1.1:** See LICENSE and the official agreement.
- **People_Speech exception:** See CC BY-SA 4.0 and the official license.

`People_Speech` refers to the released `people_speech` corpus, stored at `en/people_speech/en_asr.jsonl.gz` and available through configuration `en_people_speech`. CC BY-SA 4.0 also applies to these records when they are accessed through the combined `en` configuration. OpenMDW-1.1 applies to the other released manifests. See NOTICE for the subset mapping.

Obtain source audio separately from its distributors under the applicable upstream terms. The licenses above describe the dataset materials released in this repository.

If you use Granary-v2, please cite the NVIDIA technical report below. The report describes the curation system and distinguishes the full processed collection from the public release subset.

```
@techreport{koluguri2026granaryv2,
  title       = {Granary-v2: Scalable Multilingual Speech Data Curation for Predictable and Controllable Model Outputs},
  author      = {Nithin Rao Koluguri and Meline Mkrtchyan and Nune Tadevosyan and Aaftab V and Sushmitha Deva and Fan Qian and Davit Karamyan and
                 Kunal Dhawan and Taejin Park and Piotr {\.Z}elasko and Arushi Dhingra and Viraj Karandikar and Shuoyang Ding and Slyne Deng and
                 Onur Yilmaz and Praateek Mahajan and Utkarsh Vaidya and Nikolay Karpov and Jagadeesh Balam and Boris Ginsburg},
  institution = {NVIDIA},
  type        = {Technical Report},
  year        = {2026},
  url         = {https://research.nvidia.com/labs/nemotron/files/granary-v2.pdf}
}
```
For background on Granary-v1, see the Granary-v1 paper in the References section.

Granary-v2 provides synthetic text labels and instruction pairs for multilingual speech. The repository contains compressed JSONL manifests. Obtain the source audio from the distributors listed in the Hours by corpus table. The Overview and data sample sections above describe the available targets and fields.

NVIDIA Corporation

October 28, 2026

Previous Version(s): Granary-v1 (1.0).

Relationship to Previous Version(s): Granary-v2 replaces the labelling pipeline and expands the release to 48 languages and 16 source entries. It adds inverse text normalization, captioning, code-switching, speech-QA, context terms, and instruction pairs. Translations are stored in each record's `translations` field, keyed by target language, and run in both directions: non-English audio into English, and English audio into 31 target languages.

This dataset is governed by the OpenMDW License Agreement, version 1.1, except for the People_Speech subset, which is governed by the Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0).

See License, LICENSE, CC BY-SA 4.0, and NOTICE for the license texts and subset mapping. Obtain source audio separately from its distributors under their applicable terms.

Use the dataset for automatic speech recognition, speech translation in both directions (non-English audio into English, and English audio into 31 target languages), and speech instruction tuning. Additional targets support inverse text normalization, audio captioning, code-switching, speech question answering, and contextual speech recognition. Target availability varies by record; see the Data Sample Structure section above.

- **Data Collection Method:** Manually-Collected. This release uses existing source corpora. The repository distributes manifests; obtain the audio from the source distributors.
- **Labeling Method:** Synthetic. The pipeline produces labels with automatic speech and text models.

The repository contains compressed JSONL manifests (`.jsonl.gz`). Each line contains one utterance record. The manifest files for each language and corpus contain the available ASR and speech translation targets. Translations are stored in the `translations` field, keyed by target language.

See Repository Structure, Quick Start, and Data Sample Structure above for file paths, loading examples, and record fields.

| Measure | Value | 
|---|---|
| Languages | 48 | 
| Source entries | 16 | 
| ASR hours | 419,974 | 
| Hours marked high-quality by the pipeline | 154,961 | 
| Speech translation hours (X→English) | 89,353 | 
| Speech translation hours (English→X) | 39,317 | 

Speech translation uses audio already included in the ASR total. See Hours by corpus above for the corpus distribution.

The Hours by corpus table lists all 16 source entries, with links to their sources and their source-corpus licenses. This repository distributes manifests. Obtain the audio from each source distributor under that corpus's terms.

- Nithin Rao Koluguri, Meline Mkrtchyan, Nune Tadevosyan, et al. (2026). *Granary-v2: Scalable Multilingual Speech Data Curation for Predictable and Controllable Model Outputs* . NVIDIA technical report.
- Granary-v1 dataset.
- Granary-v1 paper: Speech Recognition and Translation Dataset in 25 European Languages — describes the v1 pipeline, which this release supersedes.
- Granary-v2 curation pipeline in NeMo Curator.
- NVIDIA NeMo Toolkit — for training on these manifests.

The Citation section above provides the Granary-v2 NVIDIA technical report citation. The Granary-v1 paper describes the earlier release.

NVIDIA believes Trustworthy AI is a shared responsibility and we have established policies and practices to enable development for a wide array of AI applications. Developers should work with their internal developer teams to ensure this dataset meets requirements for the relevant industry and use case and addresses unforeseen product misuse.

Please report quality, risk, security vulnerabilities or NVIDIA AI Concerns here.

- Downloads last month
- 10,617
