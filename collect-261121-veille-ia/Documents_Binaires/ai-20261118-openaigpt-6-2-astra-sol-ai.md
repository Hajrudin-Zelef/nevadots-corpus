---
id: collect-261121-veille-ia/documents-binaires/ai-20261118-openaigpt-6-2-astra-sol-ai
title: "アメリカAIニュース速報 2026年11月18日 OpenAIがGPT-6.2 Astra／Solを公開｜AI瓦版@カワラワークス"
domain: documents-binaires
role: reference
task: article
actors: []
dates: []
keywords: ["astra", "openaigpt-6", "openai", "gpt-6", "sol", "greg", "google", "claude"]
source: https://note.com/kawara_works/n/nc29d4c1dc445
source_anchor: ""
source_lines: [1, 81]
section: "Documents_Binaires"
---
### 1. OpenAIがGPT-6.2 Astra／Solを公開

- **発言者:** Greg Brockman、Thibault “Tibo” Sottiaux
- **投稿日時:** 11月18日 3:02〜3:09（日本時間）
- **情報区分:** OpenAI関係者による公式発表

GPT-6.2 Astraは複雑な専門業務・科学研究向け、Solは日常的なコーディングやエージェント作業向けです。ChatGPT WorkとCodexではPlus、Pro、Enterprise、Eduに提供され、APIでは`gpt-6.2-astra`と`gpt-6.2-sol`を利用できます。

主な変更は次のとおりです。

- AstraのAPI料金は100万トークン当たり入力8ドル、キャッシュ入力0.40ドル、出力40ドル。旧Astraより入出力が20%、キャッシュ入力が60%安価
- Solは入力2ドル、キャッシュ入力0.10ドル、出力10ドルで6.1 Solと同額
- Pro 200でもSolのUltrafastが利用可能に
- 長い会話ではプロンプトの前半を変更せず、後ろへ指示を追加する方法を推奨
- ツール呼び出しにはResponses APIが必要

OpenAIのシステムカードでは、両モデルがサイバーセキュリティ能力で「Critical」、生物・化学分野で「High」に達し、Astraは外部公開モデルとして初めてAI自己改善能力でも「High」に達したと評価されています。この分類は危険性が実証されたという意味ではなく、OpenAIのPreparedness Framework上で追加対策が必要な能力水準を示します。OpenAI

**なぜ重要か:**

単なる性能向上ではなく、「高性能モデルの1タスク当たり費用を下げる」更新です。同時にサイバー・自己改善能力も上がっており、性能と安全対策の両面で重要な世代交代です。

### 2. Geminiのタスク実行機能を日本を含む6か国へ無料展開

- **発言者:** Koray Kavukcuoglu
- **投稿日時:** 11月18日 3:47
- **情報区分:** Google DeepMind幹部による公式発表の紹介

Gemini 4 Hydrogenを使うタスク実行機能が、日本、米国、韓国、カナダ、オーストラリア、インドへ無料展開されます。

Geminiアプリの「Chat／Task」を切り替え、予約、請求書分析、買い物、カレンダー登録などの複数段階作業をバックグラウンドで実行できます。各タスクは利用者専用のハードウェア分離VMで処理され、購入などの重要操作では確認を求める設計です。

ただし、電話代行、メール転送、RCS、Google Pay連携など一部の高度機能は、まず米国から提供されます。日本で最初からすべての例が利用できるとは限りません。blog.google

**なぜ重要か:**

Googleがエージェント型タスク実行を有料上位層に限定せず、日本を含む一般利用者へ広げます。Geminiが「質問に答えるアプリ」から、Googleアカウント内の情報を使って仕事を完了する実行基盤へ移行する更新です。

### 3. Isomorphic Labsが50億ドルを調達

- **発言者:** Demis Hassabis
- **投稿日時:** 11月17日 21:06
- **情報区分:** 共同創業者による公式発表

AI創薬企業Isomorphic LabsがシリーズCで50億ドルを調達しました。GreenoaksとSequoia Capitalが共同主導し、a16z、Alphabet、既存投資家などが参加しています。

資金は独自の創薬エンジン「IsoDDE」と、AIによって設計する治療候補パイプラインの拡大に使われます。Hassabisの「Solve All Disease」は企業ミッションを表す表現であり、実現済みの研究成果ではありません。Isomorphic Labs

**なぜ重要か:**

基盤モデル企業への投資ではなく、AIを使って実際の医薬品候補を開発する企業への非常に大規模な資金投入です。AI創薬が研究支援から臨床候補の創出へ進むための体制強化といえます。

### 4. OpenAI DaybreakをNATOのサイバー防衛へ導入

- **発言者:** Greg Brockman
- **投稿日時:** 11月18日 2:16
- **情報区分:** OpenAIによる公式発表

OpenAIはNATO通信情報機関と連携し、NATOの中核組織へ高度なサイバーセキュリティモデルを提供しています。利用規模は月最大3,000億の入出力トークンとされています。

Daybreakはソフトウェア調査、疑わしい活動の分析、脆弱性検証、修正案の開発・試験に使用。2026年のCrossed Swords演習では、参加者がAIエージェントを構築・運用・評価する訓練も実施されました。OpenAI

**なぜ重要か:**

フロンティアモデルが国家・同盟レベルの実運用環境へ入り始めた事例です。一方で、攻撃にも転用可能な能力を扱うため、利用者の認証、アクセス範囲、人間による監督が重要になります。

### 5. コーディングエージェント用「外部サンドボックス」を公開

- **発言者:** Simon Willison
- **投稿日時:** 11月17日 10:21
- **情報区分:** 独立開発者による技術紹介・試作公開

Claude Code、Codex、Pi本体はMac上で動かしつつ、コマンドやファイル操作を別のLinux環境へ送る「external sandbox」方式が公開されました。

WebAssembly上のv86、Apple Container、Microsoft Quicksand＋QEMUという3実装があり、共通プロトコルを使います。公開するフォルダ、読み書き権限、接続可能なホストを制限し、APIキーをエージェントから見えない場所で注入できます。blog.primeradiant.com

ただしClaude Codeでは、Bashコマンドだけがサンドボックスへ送られ、標準のRead／EditツールはMac側で動くという制約があります。Codexではプロセス・ファイル操作と`apply_patch`を外部環境へ送れますが、現時点では試作段階です。

**なぜ重要か:**

モデルによる「安全かどうか」の判断に加え、OSレベルで被害範囲を限定する決定論的な防御を導入できます。AI生成コードを試験機や開発機で自律実行させる際に実用性の高い構成です。
