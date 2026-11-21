---
id: collect-261121-veille-ia/copilot-microsoft/trying-out-microsoft-s-decision-model-microsoft-decision-1-1-evaluating-its-evolution-from-v1-0-and-
title: "Trying Out Microsoft's Decision Model 'Microsoft-Decision-1.1': Evaluating Its Evolution from v1.0 and Image Input | DevelopersIO"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "ver1", "microsoft-decision-1", "standard", "decision", "input", "foundry", "model"]
source: https://dev.classmethod.jp/en/articles/azure-decision1-1/
source_anchor: ""
source_lines: [1, 545]
section: "Copilot_Microsoft"
---
Hello, I'm Keima.

Microsoft has released a new version of its decision model, "Microsoft-Decision-1.1", and it is now GA.

Microsoft released the preview version Decision-1 about a month ago. This ver1.1 reportedly improves speed and probability accuracy, and also supports image input.

In this article, I ran Microsoft-Decision-1.1 on the same data as ver1.0, compared probability bias, fluctuation, and speed, and also tried the new image input feature (verification date: November 18, 2026).

I hope this is useful for anyone who wants to know what has changed in ver1.1 and how it can be used.

## 1. What is Microsoft-Decision-1.1?

Microsoft-Decision-1.1 is a decision model offered in Microsoft Foundry.

According to the official documentation, it makes focused judgments about text, JSON, and images, and returns numerical decisions rather than prose.

It makes a focused judgment about text, JSON, or images and returns a typed, numerical decision.


The question types are the same three as in ver1.0:

- 
`noul` : A yes/no question. Returns the probability of yes
- 
`choice` : A question that picks one from fixed options. Returns the selected value and the probability of each option
- 
`score` : A question that scores on ordered levels. Returns the probability of each level and a probability-weighted score

### 1.1 What changed from ver1.0

The changes from ver1.0 described in the announcement article are summarized below.

| Item | Decision-1 (ver1.0) | Decision-1.1 | 
|---|---|---|
| Release stage | Preview (October 9, 2026) | GA (November 17, 2026) | 
| Input | Text, JSON | Text, JSON, images | 
| Speed | - | More than 2x ver1.0 | 

| Price (Global Standard, per 1M input tokens) | $0.042 | $0.042 |

Sources: Microsoft-Decision-1.1 is now generally available for fast, low-cost decision making | Microsoft / Microsoft-Decision-1 in Microsoft Foundry | Microsoft / Introducing Microsoft-Decision-1 in Microsoft Foundry | Microsoft Community Hub

According to the announcement article, ver1.1 outperformed Jev 1.14, Gemini Decision 1, and GPT-6 Luna (Decisions).

The base model is Gemma 4 26B A4B, which uses a Mixture of Experts architecture. ver1 was based on Qwen3.5.

### 1.2 Where it can be used

Like ver1.0, ver1.1 can be called not only from Microsoft Foundry but also from OpenRouter and Vercel AI Gateway.

In this article, I deployed it to Microsoft Foundry as **Global Standard** and called it from there.

The deployment types and prices in Foundry are as follows.

| Deployment type | Processing location | Price (per 1M input tokens) | 
|---|---|---|
| Global Standard | Any supported Azure region | $0.042 | 
| Data Zone Standard (US) | Within the US | $0.046 | 
| Data Zone Standard (EU/APAC) | Within the EU or Asia Pacific | $0.05 | 

Output tokens are free for every type.

Data Zone Standard was $0.042 for both the US and EU in ver1.0, so both have become more expensive in ver1.1.

The region setting concerns where the resource is placed, and the location of the caller doesn't matter. The resource in this article was also left in East US (eastus) and called from Japan.

When placing the resource itself in Japan, I was able to choose both Global Standard and Data Zone Standard in either Japan East (japaneast) or Japan West (japanwest).


*The region determines where the resource is placed and where processing happens, regardless of where the caller is*

### 1.3 Deployment steps

From the Foundry portal, I deployed it with the following steps.

1. 
Open Microsoft Foundry and enter your project
2. 
From **"Models"** in the left menu, click**"Deploy base model"**
3. 
Select **"Microsoft-Decision-1.1"** , specify the deployment name and deployment type (Global Standard in this article), and deploy

To deploy from the Azure CLI, run the following command.

```
az cognitiveservices account deployment create \
  --name <YOUR_ACCOUNT_NAME> \
  --resource-group <YOUR_RESOURCE_GROUP> \
  --deployment-name Decision-1.1 \
  --model-name "Microsoft-Decision-1.1" \
  --model-format Microsoft \
  --model-version "1" \
  --sku-name GlobalStandard \
  --sku-capacity 1
```
For `model` in requests, specify the deployment name, not the model name.

## 2. Request and response format

This is an example `choice` request that determines which team should handle a single inquiry.

```
curl "<YOUR_FOUNDRY_ENDPOINT>/providers/microsoft/v1/systemone" \
  -H "Content-Type: application/json" \
  -H "api-key: $AZURE_API_KEY" \
  -d '{
    "model": "Decision-1.1",
    "state": "The API returns 500 on every call.",
    "questions": {
      "team": {
        "type": "choice",
        "instructions": "Which team should handle this ticket?",
        "criteria": {
          "billing": "Charges, invoices, and refunds",
          "engineering": "Bugs, errors, and outages",
          "support": "How-to and account questions"
        }
      }
    }
  }'
```
The meaning of each parameter is as follows.

- 
`model` : The deployment name
- 
`state` : The information used as the basis for the judgment (here, an inquiry saying "calling the API returns a 500 error every time")
- 
`questions.team.instructions` : The question text ("Which team should handle this inquiry?")
- 
`questions.team.criteria` : For each option, a description of which inquiries apply.`billing` is for charges, invoices, and refunds;`engineering` is for bugs, errors, and outages;`support` is for questions about how to use the product or about accounts

The `answers` in the actual response looks like this.

```
{
  "team": {
    "type": "choice",
    "choice": "engineering",
    "confidence": 0.9998599815228657,
    "probabilities": {
      "billing": 3.953596349963996e-05,
      "engineering": 0.9999066543485772,
      "support": 5.380968792313423e-05
    }
  }
}
```
This output shows the following three points.

- 
`choice` : The selected item (engineering)
- 
`confidence` : The confidence level
- 
`probabilities` : The breakdown of the probability of each option

In other words, Decision-1.1 judged this to be an inquiry about a defect and routed it to the engineering team with a probability of 0.9999.

Note that the `model` field of the response contained `microsoft-decision-1.1`, not the deployment name.

When passing an image, include the base64-encoded image data in `assets` and reference it from `state` or the question text in the form `image:<name>`.

```
{
  "model": "Decision-1.1",
  "state": "届いた荷物の写真: image:photo.png",
  "assets": {
    "photo.png": {"type": "image", "format": "png", "data_base64": "<BASE64_IMAGE>"}
  },
  "questions": {
    "damaged": {"type": "noul", "instructions": "写真に破損が写っているか。"}
  }
}
```
## 3. Comparing with ver1.0 on value matching

### 3.1 Data and requests

As in the Gemini Decision 1 cross-matching article, I used 107 test pairs, each consisting of two values of the same field in documents.

In the previous verification, Gemini Decision 1, Jev, and the Decisions API judged the same pairs; Jev recorded the highest accuracy, and Gemini Decision 1 tended to push probabilities to the extremes.

The test pairs deliberately include the following cases that are easy to misjudge.

- 
Differences in date notation ( `Sep 12, 2026` and`12/09/2026` )
- 
Differences in numeric separators ( `1.250,00` and`1,250.00` )
- 
Differences between Japanese and English address notation
- 
OCR misreads (such as the letter O in `1,25O.00` )
- 
Differences in how missing values are expressed (blank, `N/A` ,`Not applicable` )
- 
Matching a total against line items ( `Total 500 KG` and`Details: 500 KG / 500 KG` )

For each pair, I asked two questions in Japanese: "Are they exactly the same as strings?" (exact match) and "Do they refer to the same value even if the notation differs?" (semantic match).

All models answered almost every exact-match question correctly, so this article focuses on the semantic-match results.

I sent the following requests to ver1.0 and ver1.1.

```
{
  "model": "Decision-1.1",
  "state": {"値A": "1,250.00", "値B": "1250"},
  "questions": {
    "exact": {
      "type": "noul",
      "instructions": "値Aと値Bは、文字列として完全に同一か。空白・記号・大文字小文字・全角半角の違いも「同一ではない」と扱う。",
      "criteria": {
        "true": "1文字も違わず完全に同じ",
        "false": "1文字でも違う"
      }
    },
    "semantic": {
      "type": "noul",
      "instructions": "値Aと値Bは、表記が違っていても同じ値を指しているか。これは2つの書類の同じ項目を突合している場面での判断である。片方が明細の羅列で、もう片方が合計値の場合は、明細をすべて足し合わせてから合計値と比べること。",
      "criteria": {
        "true": "書式・言語・単位・略記の違いだけで、指している値は同じ。または明細の合計が合計値と一致する",
        "false": "指している値そのものが違う、または片方にしか値がない。または明細の合計が合計値と一致しない"
      }
    }
  }
}
```
I called ver1.0 and ver1.1 alternately for each case and ran all 107 cases five times each.

Note that the probabilities for Gemini Decision 1 and Jev are quoted from the Gemini Decision 1 cross-matching article, and the probabilities for ver1.0 as of October are quoted from the results of the same verification conducted in October.

### 3.2 Metrics

- 
Correct answers: The number of cases where a yes probability of 0.8 or higher was judged as "same" and matched the correct answer
- 
Slip-throughs: The number of cases where actually different values were wrongly judged as "same" (the error most to be avoided in cross-matching)
- 
Number of cases between 0.2 and 0.8: Cases where the model was unsure. The more there are, the easier it is to detect gray zones through probabilities
- 
Response time: The median time from sending a request to receiving the response, measured from my Mac (including network round-trip time)

### 3.3 Results

|  | Gemini Decision 1 | Jev | Decision-1 (October) | Decision-1.1 | 
|---|---|---|---|---|
| Correct answers | 94/106 | 100/106 | 95/106 | 97/106 | 
| Slip-throughs | 3 | 1 | 6 | 2 | 
| Between 0.2 and 0.8 | 10 | 17 | 2 | 16 | 

The totals cover 106 cases, excluding one case for which the correct answer could not be uniquely defined.


*Probabilities for semantic match (Japanese question text, 106 cases). Each dot is one value pair, and the dashed line is the 0.8 threshold*

In ver1.0, even for actually different values (orange), probabilities stuck to the extremes of 0 or 1, and all six slip-throughs had a high probability of 0.95 or more.

**In contrast, ver1.1, like Jev, now also produces values in the 0.2–0.8 middle band, and slip-throughs dropped from six to two.**

Because intermediate probabilities are now returned, it becomes easier to operate in a way such as "send only gray cases with low confidence to manual checking."

The two cases that still slipped through in ver1.1 are as follows.

| ID | Value A | Value B | Reason they differ | ver1.0 (October) | ver1.1 | 
|---|---|---|---|---|---|
| 3-6 | `1,250.00` | `1,250.00 USD` | One lacks a currency, so it can't be asserted to be the same amount | 1.00 | 0.995 | 
| 7-1 | `INVO1CE` | `INVOICE` | Misreading of I and 1 | 0.999 | 0.972 | 

On the other hand, ver1.1 correctly distinguished them as "different" in cases that ver1.0 had missed, such as the O/0 misreading (`DOC-O012345` and `DOC-0012345`) and address notations of different granularity such as `OSAKA` and `Osaka-shi, Osaka`.

### 3.4 Fluctuation over four runs

I ran the same content four times and checked how much the probabilities fluctuated.

|  | Max probability difference | Cases with difference over 0.01 | Cases where the judgment changed | 
|---|---|---|---|
| Decision-1 (ver1.0) | 0.10 | 18 | 0 | 
| Decision-1.1 | 0 | 0 | 0 | 

ver1.1 returned exactly the same probability in all four runs for all 107 cases.

ver1.0 also showed slight probability fluctuations, but there were no cases where the pass/fail judgment flipped across the 0.8 threshold.

In this verification, ver1.1 returned the same answer every time for the same input.

### 3.5 Response speed

|  | Median response time | 
|---|---|
| Decision-1 (ver1.0) | About 1.09 s (1,093 ms) | 
| Decision-1.1 | About 1.02 s (1,020 ms) | 

Comparing 534 responses each from ver1.0 and ver1.1, ver1.1's response time was about 0.93 times that of ver1.0.

The dramatic "more than 2x" difference mentioned in the announcement article could not be confirmed in the overall response time measured from the client side.

However, the measured time here includes communication from my PC to the East US (eastus) resource and the time for Global Standard routing, so I couldn't separate out the server-side processing time alone.

### 3.6 Japanese and English question text

I changed the cross-matching question text (`instructions` and `criteria`) and the `state` keys (Value A and Value B) to English, and ran the same 107 cases once on ver1.1.

|  | Japanese question text | English question text | 
|---|---|---|
| Decision-1 correct answers | 95/106 | 97/106 | 
| Decision-1.1 correct answers | 97/106 | 95/106 | 

The difference in correct answers between Japanese and English was only 2 out of 106 for both ver1.0 and ver1.1.

ver1.0 got 2 more right in English and ver1.1 got 2 more right in Japanese, so there is no strong trend of one language being favored that holds across versions.

Within the scope of this verification, accuracy doesn't seem to differ much between Japanese and English.

## 4. Trying images

### 4.1 Documents and questions

I used the same five types of document data as the Gemini Decision 1 OCR article (two Japanese receipts and three English documents).

That article compares whether Gemini Decision 1, Jev, and the Decisions API can correctly detect inserted errors when given document images and OCR-extracted text.

| Document | Language | Source / License | 
|---|---|---|
| Receipt from AEON MaxValu Matsuyama store (2019) | Japanese | Wikimedia Commons (public domain) | 
| Receipt from CamelMart Akita Hachimantai store (2022) | Japanese | Wikimedia Commons (Yauchi, CC BY-SA 4.0. Resized to 1400px height) | 
| Invoice for an article publication fee | English | Wikimedia Commons (public domain) | 
| US IRS Form 1040 test filled-in example (page 2) | English | IRS (US government work) | 
| Receipt from a Malaysian bookstore | English | SROIE (ICDAR 2019) (CC BY 4.0) | 


*From the left: receipts from AEON MaxValu Matsuyama, CamelMart Akita Hachimantai, and a Malaysian bookstore*


*From the left: the invoice for an article publication fee and page 2 of the Form 1040 test filled-in example*

From each document, I transcribed the ground-truth text for 5–6 fields and created three patterns: "no errors," "error A," and "error B."

In "error A" and "error B," two fields each were deliberately altered (such as `¥246` → `¥264` and `MaxValu` → `MaxValy`, spelled in Japanese katakana in the original), so the five documents contain a total of 20 errors.

I gave the model a set of the image and the extracted values, and had it judge per field whether the value is "identical to what is printed in the image."

```
{
  "model": "Decision-1.1",
  "state": {
    "task": "OCR でレシートの画像を読み取り、次の項目と値を得た。画像と照らし合わせて確かめる。",
    "extracted": {"store": "マックスバリュ松山店", "total": "¥264"},
    "image": "image:document.jpg"
  },
  "assets": {
    "document.jpg": {"type": "image", "format": "jpeg", "data_base64": "<BASE64_IMAGE>"}
  },
  "questions": {
    "field__total": {
      "type": "noul",
      "instructions": "抽出した「total」の値 \"¥264\" は、画像に印字されている内容と1文字も違わず同じか。空白の数の違いだけは同じとみなす。"
    }
  }
}
```
The evaluation metrics are the following two.

- 
Errors found: Of the 20 inserted errors, the number where the probability of "same as the image" was below 0.8 (correctly detected as errors)
- 
False positives: Of the 61 values that are actually correct, the number where the probability of "same as the image" fell below 0.8 (wrongly judged as errors). The higher this value, the more effort people need to spend rechecking on site

### 4.2 Results

|  | Gemini Decision 1 | Jev | Decisions API | Decision-1.1 | 
|---|---|---|---|---|
| Errors found | 19/20 | 18/20 | 17/20 | 17/20 | 
| Of which Japanese receipts | 8/8 | 8/8 | 7/8 | 8/8 | 
| Of which English documents | 11/12 | 10/12 | 10/12 | 9/12 | 
| False positives | 24/61 | 11/61 | 7/61 | 18/61 | 
| Input tokens (average) | 1,749 | 3,268 | 2,257 | 759 | 

The data for Gemini Decision 1, Jev, and the Decisions API was measured as of November 13, 2026.

The number of errors found didn't differ greatly across the four models, at 17–19.

**What stands out for Decision-1.1 is the false positives: 18 of 61, more than Jev (11) and the Decisions API (7).**

These false positives were concentrated mainly in English documents, such as the invoice number and customer ID of the English invoice and the store name of the Malaysian receipt.

On the other hand, input tokens per document averaged 759, which in terms of cost is about 1/4 of Jev and about 1/7 of the Decisions API.

## 5. Sending video, audio, and PDF

As far as I can tell from the documentation, there is no particular mention of how to input file formats other than images.

The Foundry model card also explicitly states that audio and video are not supported.

So I actually specified the video, audio, and PDF data used in the Gemini Decision 1 article in `assets` and sent requests.

| What I sent | `type` | Result | 
|---|---|---|
| JPEG photo | `image` | Accepted | 
| MP4 video | `video` | `asset 'f' must be an image` | 
| MP4 video | `image` | `asset 'f' format must be png, jpg or jpeg` | 
| MP3/WAV audio | `audio` | `asset 'f' must be an image` | 
|  | `document` /`file` | `asset 'f' must be an image` | 
|  | `image` | `asset 'f' format must be png, jpg or jpeg` | 

Video, audio, and PDF were all rejected with an HTTP 400 error.

I couldn't find the supported image formats in the official documentation, but I confirmed by actually sending them that the three mentioned in the error message, png, jpg, and jpeg, are accepted (GIF and BMP were rejected with the same error).

It was not possible to pass a PDF as-is in `assets`.

## 6. All results

This is a list of semantic-match probabilities for the 106 cross-matching cases (Japanese question text).

Decision-1.1 shows the result of the first trial, and bold indicates cases that did not match the correct answer.

## Table of all results (click to expand)

| ID | Value A | Value B | Correct answer | Gemini Decision 1 | Jev | ver1.0 (October) | ver1.1 | 
|---|---|---|---|---|---|---|---|
| 1-1 | `INV-2026-0912` | `INV-2026-0912` | Same | 0.98 | 0.98 | 1.00 | 1.00 | 
| 1-2 | `1,250.00` | `1,250.00` | Same | 0.98 | 0.98 | 1.00 | 1.00 | 
| 1-3 | `ACME TRADING CO., LTD.` | `ACME TRADING CO., LTD.` | Same | 0.98 | 0.98 | 1.00 | 1.00 | 
| 1-4 | `SEAMLESS CARBON STEEL PIPE, OUTER DIAMETER 50MM, WALL THICKNESS 3.5MM, LENGTH 6000MM, SURFACE BLACK PAINTED, PACKED IN WOODEN CRATE, MANUFACTURED UNDER SPECIFICATION JIS G3454 SGP, LOT CONTROLLED, INSPECTION CERTIFICATE ISSUED BY MILL, ORIGIN JAPAN` | `SEAMLESS CARBON STEEL PIPE, OUTER DIAMETER 50MM, WALL THICKNESS 3.5MM, LENGTH 6000MM, SURFACE BLACK PAINTED, PACKED IN WOODEN CRATE, MANUFACTURED UNDER SPECIFICATION JIS G3454 SGP, LOT CONTROLLED, INSPECTION CERTIFICATE ISSUED BY MILL, ORIGIN JAPAN` | Same | 0.96 | 0.98 | 1.00 | 1.00 | 
| 1-5 | `STEEL PIPE SEAMLESS 50MM` | `STEEL PIPE SEAMLESS 50MM` | Same | 0.98 | 0.98 | 1.00 | 0.99 | 
| 1-6 | `ＡＣＭＥ　ＴＲＡＤＩＮＧ` | `ＡＣＭＥ　ＴＲＡＤＩＮＧ` | Same | 0.97 | 0.97 | 1.00 | 1.00 | 
| 1-7 | `DOC-O0O12345` | `DOC-O0O12345` | Same | 0.97 | 0.97 | 1.00 | 0.90 | 
| 1-8 | `1,250.00` | `1,250.00` | Same | 0.97 | 0.98 | 1.00 | 1.00 | 
| 1-9 | `Sep 12, 2026` | `Sep 12, 2026` | Same | 0.98 | 0.98 | 1.00 | 1.00 | 
| 2-1 | `2026/09/12` | `September 12, 2026 (Japanese kanji notation)` | Same | 0.98 | 0.99 | 1.00 | 1.00 | 
| 2-2 | `12-SEP-2026` | `2026-09-12` | Same | 0.98 | 0.98 | 1.00 | 1.00 | 
| 2-3 | `Sep 12, 2026` | `12/09/2026` | Same | **0.56** | **0.56** | **0.05** | **0.60** | 
| 2-4 | `Reiwa 8, September 12` | `2026-09-12` | Same | 0.98 | 0.89 | 1.00 | 1.00 | 
| 2-5 | `03/04/2026` | `2026-04-03` | Same | **0.20** | 0.80 | 0.96 | **0.57** | 
| 2-7 | `2026/09/12` | `2026/09/13` | Different | 0.05 | 0.04 | 0.00 | 0.00 | 
| 2-8 | `2026/09/12` | `2025/09/12` | Different | 0.03 | 0.02 | 0.00 | 0.01 | 
| 2-9 | `September 12, 2026 (Japanese kanji notation)` | `September 12, 2026 (Japanese kanji notation)` | Same | 0.99 | 0.98 | 1.00 | 1.00 | 
| 2-10 | `12-SEP-2026` | `12-SEP-2026` | Same | 0.98 | 0.98 | 1.00 | 1.00 | 
| 2-11 | `Reiwa 8, September 12` | `Reiwa 8, September 12` | Same | 0.98 | 0.98 | 1.00 | 1.00 | 
| 2-12 | `2026/09/12` | `2026/12/09` | Different | 0.11 | 0.02 | 0.00 | 0.37 | 
| 2-13 | `Reiwa 8, September 12` | `Reiwa 7, September 12` | Different | 0.04 | 0.02 | 0.00 | 0.00 | 
| 3-1 | `1,250.00` | `1250` | Same | 0.96 | 0.98 | 1.00 | 1.00 | 
| 3-2 | `USD 1,250.00` | `$1,250.00` | Same | 0.98 | 0.99 | 1.00 | 1.00 | 
| 3-3 | `1.250,00` | `1,250.00` | Same | **0.55** | 0.86 | 1.00 | 0.97 | 
| 3-4 | `1250.0` | `1250.5` | Different | 0.03 | 0.05 | 0.00 | 0.01 | 
| 3-5 | `12,500` | `1,250` | Different | 0.09 | 0.05 | 0.00 | 0.07 | 
| 3-6 | `1,250.00` | `1,250.00 USD` | Different | **0.88** | **0.91** | **1.00** | **1.00** | 
| 3-7 | `USD 1,250` | `JPY 1,250` | Different | 0.03 | 0.04 | 0.00 | 0.04 | 
| 3-8 | `500 KGS` | `500.000 KG` | Same | 0.98 | 0.93 | 1.00 | 0.93 | 
| 3-9 | `500 KG` | `1102.31 LBS` | Same | 0.94 | 0.86 | **0.11** | **0.65** | 
| 3-10 | `10 CTNS` | `10 CARTONS` | Same | 0.96 | 0.98 | 1.00 | 1.00 | 
| 3-11 | `USD 1,250.00` | `USD 1,250.00` | Same | 0.99 | 0.98 | 1.00 | 1.00 | 
| 3-12 | `1.250,00` | `1.250,00` | Same | 0.98 | 0.98 | 1.00 | 0.98 | 
| 3-13 | `500 KGS` | `500 KGS` | Same | 0.98 | 0.98 | 1.00 | 0.99 | 
| 3-14 | `1,250.00 USD` | `1,250.00 USD` | Same | 0.99 | 0.98 | 1.00 | 1.00 | 
| 3-15 | `1,250.00` | `1,205.00` | Different | 0.03 | 0.02 | 0.00 | 0.11 | 
| 3-16 | `500 KG` | `500 LBS` | Different | 0.03 | 0.03 | 0.00 | 0.17 | 
| 3-17 | `10 CTNS` | `10 PALLETS` | Different | 0.07 | 0.21 | 0.53 | 0.36 | 
| 3-18 | `500.00 KG` | `50.00 KG` | Different | 0.04 | 0.02 | 0.00 | 0.11 | 
| 4-1 | `ACME CO., LTD.` | `Acme Co., Ltd.` | Same | 0.95 | 0.96 | 1.00 | 1.00 | 
| 4-2 | `ACME CO.,LTD.` | `ACME CO., LTD.` | Same | 0.96 | 0.96 | 1.00 | 1.00 | 
| 4-3 | `GLOBEX CORPORATION` | `GLOBEX CORP.` | Same | 0.96 | 0.93 | 1.00 | 1.00 | 
| 4-4 | `Co., Ltd. Sample Trading` | `Sample Trading Co., Ltd.` | Same | 0.95 | 0.86 | **0.38** | 0.98 | 
| 4-5 | `ＡＣＭＥ` | `ACME` | Same | 0.97 | 0.93 | 1.00 | 1.00 | 
| 4-6 | `ACME TRADING CO.` | `ACNE TRADING CO.` | Different | 0.11 | 0.23 | 0.02 | 0.32 | 
| 4-7 | `Initech Inc.` | `Initech Incorporated` | Same | 0.95 | 0.95 | 1.00 | 1.00 | 
| 4-8 | `Co., Ltd. Sample Trading` | `Co., Ltd. Sample Trading` | Same | 0.98 | 0.98 | 1.00 | 1.00 | 
| 4-9 | `ＡＣＭＥ` | `ＡＣＭＥ` | Same | 0.99 | 0.97 | 1.00 | 0.98 | 
| 4-10 | `GLOBEX CORP.` | `GLOBEX CORP.` | Same | 0.99 | 0.97 | 1.00 | 1.00 | 
| 4-11 | `Initech Incorporated` | `Initech Incorporated` | Same | 0.99 | 0.98 | 1.00 | 1.00 | 
| 4-12 | `ACME TRADING CO., LTD.` | `ACME SHIPPING CO., LTD.` | Different | 0.03 | 0.16 | 0.00 | 0.13 | 
| 4-13 | `GLOBEX CORP.` | `GLOBEX HOLDINGS CORP.` | Different | 0.08 | 0.31 | 0.07 | 0.49 | 
| 4-14 | `Co., Ltd. Sample Trading` | `Co., Ltd. Sample Goods` | Different | 0.09 | 0.20 | 0.00 | 0.29 | 
| 4-15 | `ACME CO., LTD.` | `ACME CO., LTD. (Taiwan Branch)` | Different | 0.10 | 0.24 | 0.01 | 0.42 | 
| 5-1 | `TOKYO, JAPAN` | `Tokyo Japan` | Same | 0.97 | 0.98 | 1.00 | 1.00 | 
| 5-2 | `JP` | `JAPAN` | Same | 0.94 | 0.93 | 1.00 | 0.99 | 
| 5-3 | `OSAKA` | `Osaka-shi, Osaka` | Different | 0.67 | 0.67 | **1.00** | 0.77 | 
| 5-4 | `1-1-1 Sample-cho, Chuo-ku, Tokyo` | `Postal 100-0000, 1-1-1 Sample-cho, Chuo-ku, Tokyo` | Same | **0.67** | 0.97 | 1.00 | 0.93 | 
| 5-5 | `SAPPORO` | `SENDAI` | Different | 0.04 | 0.04 | 0.00 | 0.19 | 
| 5-6 | `Postal 100-0000, 1-1-1 Sample-cho, Chuo-ku, Tokyo` | `Postal 100-0000, 1-1-1 Sample-cho, Chuo-ku, Tokyo` | Same | 0.99 | 0.98 | 1.00 | 1.00 | 
| 5-7 | `TOKYO, JAPAN` | `TOKYO, JAPAN` | Same | 0.98 | 0.98 | 1.00 | 1.00 | 
| 5-8 | `JP` | `JP` | Same | 0.97 | 0.97 | 1.00 | 1.00 | 
| 5-9 | `1-1-1 Sample-cho, Chuo-ku, Tokyo` | `1-1-2 Sample-cho, Chuo-ku, Tokyo` | Different | 0.03 | 0.04 | 0.00 | 0.03 | 
| 5-10 | `Postal 100-0000, 1-1-1 Sample-cho, Chuo-ku, Tokyo` | `Postal 101-0000, 1-1-1 Sample-cho, Chiyoda-ku, Tokyo` | Different | 0.03 | 0.12 | 0.00 | 0.02 | 
| 5-11 | `CHUO-KU, TOKYO` | `CHUO-KU, OSAKA` | Different | 0.04 | 0.03 | 0.00 | 0.03 | 
| 6-1 | `STEEL PIPE SEAMLESS 50MM` | `SEAMLESS STEEL PIPE 50MM` | Same | 0.96 | 0.97 | 1.00 | 1.00 | 
| 6-2 | `STEEL PIPE SEAMLESS 50MM` | `STEEL PIPE SEAMLESS 50MM` | Same | 0.97 | 0.97 | 1.00 | 1.00 | 
| 6-3 | `STEEL PIPE (50MM)` | `STEEL PIPE 50MM` | Same | 0.98 | 0.97 | 1.00 | 1.00 | 
| 6-4 | `STEEL PIPE 50MM` | `STEEL PIPE 60MM` | Different | 0.03 | 0.03 | 0.00 | 0.02 | 
| 6-5 | `SEAMLESS CARBON STEEL PIPE, OUTER DIAMETER 50MM, WALL THICKNESS 3.5MM, LENGTH 6000MM, SURFACE BLACK PAINTED, PACKED IN WOODEN CRATE, MANUFACTURED UNDER SPECIFICATION JIS G3454 SGP, LOT CONTROLLED, INSPECTION CERTIFICATE ISSUED BY MILL, ORIGIN JAPAN` | `SEAMLESS CARBON STEEL PIPE, OUTER DIAMETER 50MM, WALL THICKNESS 3.5MM, LENGTH 6000MM, SURFACE BLACK PAINTED, PACKED IN WOODEN CRATE, MANUFACTURED UNDER SPECIFICATION JIS G3454 SGP, LOT CONTROLLED, INSPECTION CERTIFICATE ISSUED BY MILL, ORIGIN KOREA` | Different | 0.06 | 0.09 | 0.11 | 0.02 | 
| 6-6 | `STEEL PIPE (50MM)` | `STEEL PIPE (50MM)` | Same | 0.98 | 0.98 | 1.00 | 1.00 | 
| 6-7 | `SEAMLESS CARBON STEEL PIPE, OUTER DIAMETER 50MM, WALL THICKNESS 3.5MM, LENGTH 6000MM, SURFACE BLACK PAINTED, PACKED IN WOODEN CRATE, MANUFACTURED UNDER SPECIFICATION JIS G3454 SGP, LOT CONTROLLED, INSPECTION CERTIFICATE ISSUED BY MILL, ORIGIN KOREA` | `SEAMLESS CARBON STEEL PIPE, OUTER DIAMETER 50MM, WALL THICKNESS 3.5MM, LENGTH 6000MM, SURFACE BLACK PAINTED, PACKED IN WOODEN CRATE, MANUFACTURED UNDER SPECIFICATION JIS G3454 SGP, LOT CONTROLLED, INSPECTION CERTIFICATE ISSUED BY MILL, ORIGIN KOREA` | Same | 0.95 | 0.98 | 1.00 | 1.00 | 
| 6-8 | `STEEL PIPE SEAMLESS 50MM` | `STEEL PIPE WELDED 50MM` | Different | 0.05 | 0.09 | 0.00 | 0.09 | 
| 6-9 | `STEEL PIPE 50MM BLACK` | `STEEL PIPE 50MM GALVANIZED` | Different | 0.04 | 0.08 | 0.00 | 0.16 | 
| 6-10 | `SEAMLESS STEEL PIPE 50MM x 6000MM` | `SEAMLESS STEEL PIPE 50MM x 6600MM` | Different | 0.03 | 0.04 | 0.00 | 0.01 | 
| 7-1 | `INVO1CE` | `INVOICE` | Different | 0.24 | 0.73 | **1.00** | **0.97** | 
| 7-2 | `DOC-O012345` | `DOC-0012345` | Different | 0.25 | 0.61 | **1.00** | 0.56 | 
| 7-3 | `1,25O.00` | `1,250.00` | Different | **0.86** | 0.73 | **0.99** | 0.57 | 
| 7-4 | `ACMl` | `ACME` | Different | 0.45 | 0.28 | 0.09 | 0.45 | 
| 7-5 | `DOC-O012345` | `DOC-O012345` | Same | 0.99 | 0.97 | 1.00 | 0.99 | 
| 7-6 | `1,25O.00` | `1,25O.00` | Same | 0.97 | 0.87 | 1.00 | 0.82 | 
| 7-7 | `INVO1CE` | `INVO1CE` | Same | 0.97 | 0.94 | 1.00 | 0.94 | 
| 7-8 | `DOC-2026` | `DOC-2028` | Different | 0.05 | 0.04 | 0.00 | 0.22 | 
| 8-1 | `` | `` | Same | **0.16** | **0.37** | **0.04** | **0.01** | 
| 8-2 | `` | `1,250.00` | Different | 0.01 | 0.02 | 0.00 | 0.00 | 
| 8-3 | `N/A` | `` | Same | **0.09** | **0.15** | **0.01** | **0.03** | 
| 8-4 | `-` | `Not applicable` | Same | **0.24** | **0.46** | 0.99 | **0.52** | 
| 8-5 | `N/A` | `N/A` | Same | **0.21** | **0.51** | 1.00 | **0.09** | 
| 8-6 | `Not applicable` | `Not applicable` | Same | **0.37** | 0.91 | 1.00 | 0.89 | 
| 8-7 | `N/A` | `0` | Different | 0.08 | 0.08 | 0.13 | 0.04 | 
| 9-1 | `Total 500 KG` | `Details: 200 KG / 200 KG / 100 KG` | Same | 0.98 | 0.98 | 0.96 | 0.99 | 
| 9-2 | `Total 500 KG` | `Details: 200 KG / 200 KG / 50 KG` | Different | 0.03 | 0.40 | 0.12 | 0.16 | 
| 9-3 | `10 CTNS` | `CTN No.1-10` | Same | 0.91 | 0.88 | 1.00 | 0.92 | 
| 9-4 | `1 box` | `Control No. ABCD1234567 (1 box)` | Same | 0.84 | 0.92 | 1.00 | 0.96 | 
| 9-5 | `Details: 200 KG / 200 KG / 100 KG` | `Details: 200 KG / 200 KG / 100 KG` | Same | 0.99 | 0.98 | 1.00 | 1.00 | 
| 9-6 | `CTN No.1-10` | `CTN No.1-10` | Same | 0.97 | 0.98 | 1.00 | 1.00 | 
| 9-7 | `10 CTNS` | `CTN No.1-9` | Different | 0.04 | 0.35 | 0.12 | 0.19 | 
| 9-8 | `Total 500 KG` | `Details: 500 KG / 500 KG` | Different | **0.97** | 0.56 | **0.95** | 0.49 | 
| 10-1 | `INV-2026-0912` | `INV-2026-0912` | Same | 0.94 | 0.96 | 1.00 | 1.00 | 
| 10-2 | `ACME CO., LTD.` | `ACME CO., LTD.` | Same | 0.90 | 0.95 | 1.00 | 0.97 | 
| 10-3 | `1,250.00` | `1,250.00` | Same | 0.98 | 0.98 | 1.00 | 0.98 | 
| 10-4 | `ACME　CO., LTD.` | `ACME CO., LTD.` | Same | 0.96 | 0.95 | 1.00 | 1.00 | 
| 10-5 | `1,250.00` | `1,250.00` | Same | 0.99 | 0.98 | 1.00 | 0.99 | 
| 10-6 | `INV-2026-0912` | `INV-2026-0912` | Same | 0.94 | 0.98 | 1.00 | 0.99 | 
| 10-7 | `ACME　CO., LTD.` | `ACME　CO., LTD.` | Same | 0.98 | 0.98 | 1.00 | 1.00 | 
| 10-8 | `STEEL PIPE SEAMLESS` | `STEEL PIPE SEAMLESS` | Same | 0.98 | 0.97 | 1.00 | 0.99 | 

## 7. Summary

I verified Microsoft-Decision-1.1 using the same value-matching dataset as ver1.0 and the same document images as the Gemini Decision 1 article.

There were two points I found good in actual use.

The first is that the problem that was prominent in ver1.0, "probabilities sticking to 0 or 1," has been resolved, and, like Jev, it now appropriately outputs scores in the 0.2–0.8 middle band.

The second is that even when the same request was repeated four times, exactly identical probabilities were returned, with no fluctuation in results.

In addition, accuracy with Japanese question text was about the same as with English, so there was no sense that it is hard to use because it's Japanese.

On the other hand, as a caveat, the speed measured from my machine was only about 1.07 times that of ver1.0, and I couldn't feel the "more than 2x" speedup announced officially. That said, it's fast enough, so I don't think it's much of a problem.

Also, although the image recognition itself works, it tends to judge even error-free values as "different from the image," with 18 false positives out of 61.

In terms of pricing, it can be used at the same price as Jev, which is nice and cheap. It would be even better if processing could be completed within Japan. For users who have already adopted Azure, I think it is worth considering Microsoft-Decision-1.1.
