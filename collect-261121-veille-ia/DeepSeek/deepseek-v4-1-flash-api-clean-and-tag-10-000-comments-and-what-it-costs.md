---
id: collect-261121-veille-ia/deepseek/deepseek-v4-1-flash-api-clean-and-tag-10-000-comments-and-what-it-costs
title: "DeepSeek V4.1 Flash API: Clean and Tag 10,000 Comments, and What It Costs"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-11-09"]
keywords: ["comments", "model", "deepseek", "cost", "chinese", "costs", "flash", "calls"]
source: http://dev.to/everyinfrahq/deepseek-v41-flash-api-clean-and-tag-10000-comments-and-what-it-costs-4kho
source_anchor: ""
source_lines: [1, 509]
section: "DeepSeek"
---
If you tag 10,000 social comments with DeepSeek V4.1 Flash, most of the model bill is output. The labels that the model writes cost 74% to 83% of the model fee. The comments that it reads cost much less. Short codes instead of full JSON, plus off-peak hours, bring the measured model fee in our Chinese and English samples from about $0.34–0.50 at peak down to $0.07–0.14.

We got these numbers from two live runs on November 9, 2026, Singapore time (SGT). From 00:16 to 00:33, 38 calls to the EveryInfra data API returned 907 comments and bullet comments in English, Chinese and Spanish. From 03:21 to 03:30, we sent 602 of the clean comments to deepseek-v4.1-flash-1017 and read the `usage` field of each reply. We priced the tokens with the DeepSeek pricing page. The measured model fee was 20% to 54% higher than our estimate from the doc ratios.

Two more results go against common advice. Cleaning and dedupe removed only 11.8% of rows in all three languages, so the savings are small. Keyword rules could tag only 13.1% to 34.2% of comments with one clear label. The model should do the tagging, and rules should only check it.

**Key points**


- 38 live data calls cost $0.028040 and returned 907 comments. The 12 search calls took $0.013587 of that, almost half. 5 empty results were refunded. One 429 and one 422 cost nothing.
- Cleaning and dedupe removed 107 rows (11.8%). This is below the 15% bar that we set before the test.
- Keyword rules gave exactly one tag to 21.6% of English, 13.1% of Chinese and 34.2% of Spanish comments. Substring bugs inflated the Spanish number. With word boundaries on every keyword, it is 30.2%.
- For 10,000 raw comments, data calls cost $0.0984–0.5828 with search included. DeepSeek V4.1 Flash at peak with full JSON costs $0.3386–0.5383 with measured tokens. Output is 73.8%–82.7% of that model fee.
- We turned thinking off on all 9 model requests. 4 of them still returned 4,041 to 8,010 reasoning tokens, and those batches cost 2 to 3 times more. Read
`reasoning_tokens` in `usage` on every reply.- In our estimate, a batch size of 25 or 400 changes the model fee by about 1.3%. Measured short codes save 45%–59%. Off-peak hours cut the rest in half.

The question here is practical. You follow a public health or food safety topic. You want the public discussion from several platforms. Then a cheap model must clean, dedupe and tag it for a risk communication team.

What does 10,000 comments cost? Which step costs the most? Which savings are real?

Every EveryInfra number in this article comes from a raw API response. On the DeepSeek side, we have no official DeepSeek API key. So we sent the model calls through a third-party OpenAI-compatible relay, with the model name `deepseek-v4.1-flash-1017`. Each reply gave that same name in its `model` field.

Token counts come from the `usage` field. Costs use the DeepSeek list price, so they are not a real DeepSeek invoice. The batch-size table and the Spanish short codes are still estimates from the doc ratios, and they say so. The 10,000-comment totals are extrapolated from the sample.

We fixed the job first, because every number below depends on it. We chose three topics from October and November 2026 with a lot of public talk. Each topic is in one language.

The English topic is H5N1 bird flu in Washington dairy cattle. On October 8, 2026, USDA APHIS confirmed the first two infected dairy herds in Washington (USDA APHIS, 2026-10-08). On November 3, CIDRAP reported two more human cases in Yakima County, and one person died (CIDRAP, 2026-11-03). CIDRAP says this is the first H5N1 death in the US this year.

The Chinese topic is angelica root (bai zhi) that sellers smoke with sulfur. Chinese state TV ran an exposé on November 1. A food safety news site counted the national spot-check bulletins of this year. Bai zhi with sulfur dioxide residue is in 19 of 21 bulletins (China Food Safety News, in Chinese).

The Spanish topic is New World screwworm in Mexico, plus a rumor about avocados. PAHO issued an epidemiological alert on human myiasis on October 16, 2026 (PAHO/WHO, 2026-10-16). On November 4, the Mexican agriculture minister said that larvae in avocados were not the cattle screwworm (The Food Tech, 2026-11-04).

We chose these topics for two reasons. First, each one mixes facts and myths. Some people ask how to spot bad product. Some people blame sellers. Some people say raw milk protects you from bird flu. Some people think that an avocado pest eats cattle.

Rules cannot cover all of that, so the test is fair for a cheap model. Second, each topic lives on different platforms. English talk is on Reddit and YouTube. Chinese talk is on Bilibili, Douyin, Zhihu and Weibo. Spanish talk is almost all on YouTube.

| Topic | Language | Platforms | Comment capabilities | Raw rows | 
|---|---|---|---|---|
| H5N1 in Washington dairy cattle | English | Reddit, YouTube | `reddit/comments` ,`youtube/comments` | 230 | 
| Sulfur-smoked bai zhi | Chinese | Bilibili, Douyin, Zhihu, Weibo | `bilibili/comments` ,`bilibili/danmaku` ,`douyin/comments` ,`zhihu/content_comments` ,`weibo/comments` | 338 | 
| Screwworm and the avocado rumor | Spanish | YouTube | `youtube/comments` | 339 | 

We also fixed the output before the test. Each comment gets 0 to 3 topic tags, such as `raw_milk_pasteurization` or `distrust_conspiracy`. It gets one stance, such as worried, informative or hostile to officials. The Spanish prompt has one more field. It asks whether the comment is about the cattle screwworm, the avocado borer, or a mix of both.

Each comment also gets a `needs_fact_check` flag. The flag marks comments that state a fact about the virus, milk, standards or cases. A person then checks those comments. The tag list is in the prompt, and we did not change it after the test.

The target size is 10,000 comments. The live sample has only a few hundred. We extrapolate with ratios from the sample: rows per call, rows kept after cleaning, and tokens per comment. Every unit price is real. But the sample bias also grows with the total. So each total says "extrapolated," and the limits section says which way the bias can go.

We checked the version first. We read the DeepSeek English pricing page at 00:14 SGT on November 9, 2026 (DeepSeek Models & Pricing). The model name is `deepseek-flash`. Its current model version is DeepSeek-V4.1-Flash-1017. The context is 1M tokens, and the maximum output is 384K.

It supports JSON output and tool calls. Thinking mode is on by default. The Chinese page says the same. The old name `deepseek-v4-flash` still works, but requests with it go to V4.1-Flash-1017 and bill at the Flash price.

This model has two release dates, and people mix them up. V4.1-Flash first went live on September 10, 2026. The release note says that it is a 552B-parameter mixture-of-experts model (DeepSeek release note, 2026-09-10). It uses 8B active parameters for input and 16B for output. Its KV cache needs a quarter of the GPU memory of the last model. The new prices started at 04:00 UTC on September 10.

The 1017 release came on October 17. It has new weights on the same architecture. The note says that it scores higher, finishes tasks faster and writes shorter output. The weights are on Hugging Face (DeepSeek release note, 2026-10-17). The change log entry for October 17 matches (DeepSeek Change Log).

DeepSeek does not explain why the name is not V4.2.

| Item | deepseek-flash (V4.1-Flash-1017) | Source | 
|---|---|---|
| Context / max output | 1M / 384K | Pricing page, read 2026-11-09 | 
| Input, cache hit, per 1M tokens | Peak $0.006, off-peak $0.003 | Pricing page | 
| Input, cache miss, per 1M tokens | Peak $0.30, off-peak $0.15 | Pricing page | 
| Output, per 1M tokens | Peak $1.20, off-peak $0.60 | Pricing page | 
| Peak hours | Mon-Fri 01:00-04:00 and 06:00-10:00 UTC, not on Chinese public holidays | Pricing page footnote | 
| Concurrency per account | 2,500 | Rate limit page | 
| Thinking mode | On by default, effort high | Thinking mode page | 

*The DeepSeek pricing page, read on November 9, 2026. Output costs 4 times the cache-miss input. Off-peak prices are half.* DeepSeek pricing page screenshot

Two ratios in this table matter most for bulk tagging. First, output costs 4 times the cache-miss input. At peak, one million output tokens cost $1.20, and one million input tokens cost $0.30. A comment can be 12 to 44 input tokens. The JSON label for it can be more than 20 output tokens. So the output cost can easily pass the input cost.

Second, off-peak prices are exactly half of peak prices. The pricing page states this as a billing rule, not as a sale. But the same page also says: "Product prices may vary and DeepSeek reserves the right to adjust them." DeepSeek first added peak and off-peak prices with V4-Pro on August 13 (DeepSeek release note, 2026-08-13). The September 10 note said again that off-peak is 50% of peak. For a batch job that can wait a few hours, this is a free 50% discount. Check the pricing page on the day that you schedule the job.

How much cheaper is this than the last Flash model? A Hacker News user listed off-peak prices for three DeepSeek models (Hacker News comment). The older V4 Flash 0731 cost $0.22 per million cache-miss input and $0.66 per million output. V4.1 Flash costs $0.15 and $0.60.

We saved a copy of that comment on November 9. The V4.1 numbers match the current pricing page. We found no official archive of the old prices, so use them only as a guide.

On the EveryInfra side, the job has two steps. First, search finds the posts and videos with the most talk: `youtube/search`, `reddit/search`, `bilibili/search`, `douyin/search`, `zhihu/search` and `weibo/search`. Then the comment calls pull the text: `youtube/comments`, `reddit/comments`, `bilibili/comments`, `bilibili/danmaku`, `douyin/comments`, `zhihu/content_comments` and `weibo/comments`.

We read capability names, parameters and prices from the free catalog first (EveryInfra catalog, read at 00:14 SGT on November 9). All 13 capabilities had `available: true`. Most cost $0.000556 per call. `weibo/search` costs $0.000806, and `douyin/search` costs $0.008333. Douyin comments bill per unit of 20 rows, at $0.000556 per unit.

A request looks like this. The key comes from an environment variable, not from the code:


```
curl -s https://api.everyinfra.com/api/v1/social \
  -H "Authorization: Bearer $EVERYINFRA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"platform":"reddit","action":"comments",
       "params":{"url":"https://www.reddit.com/r/PrepperIntel/comments/1xlm96d/washington_dairy_worker_hospitalized_w_severe/","limit":100}}'
```
The table shows all 38 requests. All times are Singapore time. The English sample came first, then two planned errors, then Chinese and Spanish.

| Time (SGT) | Capability | Calls | Rows returned | Charged | Note | 
|---|---|---|---|---|---|
| 00:16:32-00:16:37 SGT | `reddit/search` ,`youtube/search` | 2 | 9, 20 | $0.001112 | Find English posts and videos | 
| 00:16:57 SGT | `reddit/comments` | 1 | 0 | $0 | 429: this test key allows 10 calls per minute | 
| 00:20:36-00:22:30 SGT | `reddit/comments` | 4 | 58, 29, 28, 9 | $0.002224 | Posts on Washington herds and cases | 
| 00:22:46-00:24:32 SGT | `youtube/comments` | 5 | 100, 0, 6, 0, 0 | $0.001112 | 3 empty results, refunded | 
| 00:25:21-00:26:14 SGT | `weibo/search` ,`bilibili/search` ,`zhihu/search` | 4 | 16, 20, 20, 20 | $0.002474 | Find Chinese videos and answers | 
| 00:26:23-00:27:23 SGT | `youtube/search` ,`reddit/search` | 5 | 20, 20, 0, 0, 20 | $0.001668 | 2 empty Reddit searches, refunded | 
| 00:27:39 SGT | `youtube/comments` | 1 | 0 | $0 | 422: we sent sort=top on purpose | 
| 00:27:46-00:29:21 SGT | `bilibili/comments` ,`bilibili/danmaku` ,`zhihu/content_comments` ,`weibo/comments` | 8 | 3, 3, 10, 57, 8, 100, 16, 1 | $0.004448 | Chinese comments and bullet comments | 
| 00:29:29-00:30:49 SGT | `youtube/comments` | 4 | 100, 100, 100, 39 | $0.002224 | Spanish comments | 
| 00:31:22-00:32:55 SGT | `douyin/search` ,`douyin/comments` | 4 | 10, 49, 60, 31 | $0.012778 | Douyin bills per 20 rows | 

The total is 38 requests and $0.028040. The comment calls returned 230 English, 338 Chinese and 339 Spanish rows, so 907 in all. Search returned 175 more rows. We used them only to pick posts, not as tagging input.

This chart shows the most common error in a cost estimate. People count calls from the `limit` value. But the 6 charged English comment calls returned 38.3 rows on average. The Chinese calls returned 30.7, and only the 4 Spanish calls returned 84.8.

In English, the cause is the posts. A small Reddit thread had only 9 comments, and three YouTube videos had none. In Chinese, part of the cause is the API. In the catalog, `bilibili/comments` has `default_limit` and `max_limit` of 10, so one call returns 10 rows at most. Our three calls did not set `limit`.

Also, search showed 98 and 80 comments on two Bilibili videos, but the API returned only 3 for each. A third video with 4,267 comments returned 10, which is the cap. For volume on Bilibili, use bullet comments, which return up to 200 rows per call. We reported the 3-of-98 result to the API team.

Also count the search calls that find the posts. In the sample, 12 search calls cost $0.013587, which is 48% of the $0.028040 total. One `douyin/search` call alone cost $0.008333. We scale the full sample bill: sample cost divided by sample rows, times 10,000. Then the data fee for 10,000 raw comments is $0.2176 in English, $0.5828 in Chinese and $0.0984 in Spanish.

Comment calls alone cost $0.1451, $0.2636 and $0.0656, so use those as the low end. The real number is usually between the two, because a post with more comments spreads its search cost over more rows. Chinese costs more for one more reason. Douyin bills per 20 rows, so a call on a busy video can cost $0.001667.

Three billing rules matter when you plan for 10,000 comments. First, empty results cost nothing. Three English YouTube videos had comments turned off or empty. The API returned `count: 0`, `charged: false` and `reason: empty_result_refunded`.

Second, Douyin units bill by actual rows. The first `douyin/comments` call asked for `limit: 150`, which is 8 units. It returned 49 rows, so it billed 3 units ($0.001667) and returned the rest. Third, a bad parameter fails before any charge.

| Case in our calls | HTTP | charged_usd | What the script does | 
|---|---|---|---|
| reddit/comments, 58 rows | 200 | 0.000556 | Save rows, add the charge to the log | 
| youtube/comments, comments off | 200 | 0.0 ( `empty_result_refunded` ) | Log the empty post and move on | 
| douyin/comments, limit 150, 49 rows | 200 | 0.001667 (3 of 8 units) | Nothing; unused units come back | 
| youtube/comments, sort=top | 422 | none | Use the value in `did_you_mean` and send again | 
| reddit/comments at the key limit | 429 | none | Wait for `retry_after` , then try again | 

*Three real responses at 00:20, 00:23 and 00:27 SGT on November 9, 2026: a charge, a refund for an empty result, and a free 422. Author names and balance fields are hidden.* EveryInfra API response screenshot

The planned error is useful. We sent `sort: top` to `youtube/comments`. The API returned 422 with code `invalid_value`. The `allowed` field lists `newest` and `popular`, and `did_you_mean` gives `popular`.

The first line in `next_steps` says to change only `sort` and send again, and that nothing was charged. A batch script can fix that by itself. The worst case is an API that ignores a bad value, charges you, and returns the wrong sort order.

The 429 was the same. The response gave `retry_after`, `limit: 10` and `retryable: true`. Our script waited and tried again, and the call worked at 00:20:36. For more on error handling, see how API errors heal themselves. For refund checks, see failure refund reconciliation.

Step one is cleaning, and it costs no model fee. Our Python script does seven things in order. It applies Unicode NFKC to merge full-width and compatible characters (Python unicodedata docs). It removes zero-width characters and collapses whitespace. It drops junk rows: link-only rows, promos with three or more links, emoji-only rows, and empty replies such as "lol" or "first". It drops rows under 3 words in English and Spanish, or under 2 characters in Chinese.

Then it removes exact duplicates. The key is the lowercase text without links, punctuation, spaces and emoji. Last, it removes near duplicates. It cuts Chinese into 3-character pieces and other languages into 3-word pieces. Two comments with a Jaccard score of 0.8 or more count as one.

The near-dupe step compares every pair. A few hundred rows take under a second. At 10,000 rows there are about 50 million pairs, and plain Python takes minutes. MinHash with locality-sensitive hashing makes this close to linear. The open source datasketch library has a MinHash LSH index (datasketch on GitHub; datasketch on PyPI).

Before the test, we set a bar. Our bar was 15% of rows in all three languages. At or above the bar, we would say "clean first to save money." Below the bar, we would say "clean for quality, not for money." Here are the results.

| Sample | Raw rows | Kept | Removed | Main reason | Token change (estimate) | 
|---|---|---|---|---|---|
| English: H5N1 dairy | 230 | 213 | 7.4% | Too short: 12, such as "Phew..." | about 9,323 to 9,107 (-2.3%) | 
| Chinese: sulfur-smoked bai zhi | 338 | 289 | 14.5% | Exact duplicates: 37, almost all bullet-comment spam | about 4,165 to 3,003 (-27.9%) | 
| Spanish: screwworm | 339 | 298 | 12.1% | Emoji only: 21, too short: 19 | about 8,960 to 8,860 (-1.1%) | 
| All | 907 | 800 | 11.8% | - | - | 

The three samples lost 107 rows, or 11.8%. That is below the 15% bar. So the result goes on the fail branch that we wrote first. Cleaning is for quality, not for money. In English and Spanish, most removed rows were very short. They had few tokens, so tokens fell only 2.3% and 1.1%.

The exception is Chinese bullet comments. Bilibili shows these comments over the video. In one video, 22 of 100 bullet comments were removed. In another, 23 of 57 were removed. Most were two fan chants for a pop idol, posted 23 times in total, and each chant was long.

So the Chinese sample lost 14.5% of rows but 27.9% of tokens. The normal Chinese comment sections lost only 4 of 173 rows. The lesson: judge cleaning by source. Spam-heavy sources such as bullet comments save a quarter of input. Normal comment sections save almost nothing.

Money makes this clearer. Our first cost model estimates tokens from the doc ratios. With it, 10,000 raw comments sent to the model with no cleaning cost $0.4442 in English, $0.2984 in Chinese and $0.3877 in Spanish. That is at peak with full JSON. After cleaning, the costs are $0.4176, $0.2502 and $0.3495.

The savings are 6.0%, 16.2% and 9.9%. Most of the savings are output. The model writes a full label for each comment, however short. So when you drop "Phew...", you save more than 20 output tokens, not its 3 input tokens.

Cleaning rules also make mistakes. We read all 107 removed rows. Six of them, about 5.6%, had real content. "HMPV possibly?" asks if the illness is a different virus, but it has two words.

The Spanish rows "Bot flies" and "Los torsalos" name a different parasite fly. A Russian comment says that the larvae are botfly larvae and that garlic does not help. Our script counted only Latin and Chinese letters, so it dropped the Russian text. The other two are "Falso." and "puro monopolio," which show doubt and a price complaint.

Three of the six mention a different parasite fly. That is a clue for the species confusion that the Spanish prompt tries to catch. For public health work, let the model read a few more short rows. Do not drop a question such as "is this a different disease?" If you copy this script, set the short-row limit to 2 words. Run spam dedupe only on bullet-comment sources.

After cleaning, the next idea is to tag easy comments with keyword rules. Then only the hard ones go to the model. We wrote six or seven tag keyword groups per language. A comment that hits exactly one tag is "high confidence," and the rule result is final.

A comment that hits several tags or none goes to the model. Before the test, we set the bar at 40% high-confidence coverage. Only then would we recommend "rules first, model for the rest."

| Sample | Kept rows | Exactly one tag | Several tags | No tag | Top rule tag | 
|---|---|---|---|---|---|
| English | 213 | 46 (21.6%) | 7 (3.3%) | 160 (75.1%) | distrust_conspiracy, 13 | 
| Chinese | 289 | 38 (13.1%) | 8 (2.8%) | 243 (84.1%) | herbal medicine and food use, 11 | 
| Spanish, rule as written | 298 | 102 (34.2%) | 53 (17.8%) | 143 (48.0%) | precio_y_exportacion, 45 | 
| Spanish, after the "usa" fix | 298 | 92 (30.9%) | 27 (9.1%) | 179 (60.1%) | ganado_y_moscas, 38 | 

No language reached 40%. So the result is on the fail branch: the model should tag everything, and rules should only check it. English rules miss irony and hints. "Wonder who has been doing gain of function research" hits a rule.

"Hmmm and what country controls our media?" is the same kind of distrust, but no rule catches it. Chinese is the lowest, because Chinese comments are short and casual. A reader sees at once that "because nobody got executed" is about enforcement. No keyword in the group matches it.

Spanish had the highest coverage but the worst accuracy. We found the reason while we wrote this section. The price-and-export tag had the keyword "usa," which should match "USA." It had no word boundary. So every comment with "gusano" (worm) matched it. "Cómo llegó a México ese maldito gusano" means "how did this damn worm get to Mexico." The rule tagged it as price and export. 70 of the 298 Spanish comments contain "gusano."

With a whole-word match for "usa" only, Spanish high-confidence coverage falls from 34.2% to 30.9%. The price-and-export count falls from 45 to 9. Several-tag rows fall from 53 to 27. But "precio" (price) also matches "preciosa" (precious), so a prayer about "la sangre preciosa" also became a price comment. So we added word boundaries to every keyword and ran the rules again.

Coverage is now 30.2%, and price-and-export keeps 7 rows. We read all 7. None is about price or export. They mention "carne" (flesh, often the flesh that the larvae eat) or "USA" in another sense.

The chart keeps the 34.2% from the rule as written. These errors are the best proof that rules are fragile. One missing boundary made one tag count more than 5 times too large, and the rule gave no error. The fixed number is still below 40%, so the result does not change.

The most useful Spanish label is one that rules cannot give. Is the comment about the cattle screwworm, the avocado seed borer, or a mix of both? That answer tells a health team which myth to correct. So the Spanish prompt has its own `especie` field.

Rules still have value. They are free, they are easy to explain, and they give the same result each time. Use them as a check on the model. When the model says "official response" and a rule says "raw milk," a person should look. Two independent methods are common in content analysis.

A 2023 Zurich study tested ChatGPT against crowd workers on 2,382 tweets (Gilardi et al., arXiv 2303.15056). The model was more accurate on four of five tasks, at under $0.003 per label. The study still kept human checks. Our rule check is a cheap way in.

We wrote this script from the DeepSeek docs. It uses the OpenAI Python SDK with `base_url` set to DeepSeek (openai-python on GitHub; install from openai on PyPI). For our live run, we changed only `base_url`, the key and the model name `deepseek-v4.1-flash-1017`. If you have an official DeepSeek key, use the script as it is.


```
import json, os, time
from openai import OpenAI
ds = OpenAI(api_key=os.environ["DEEPSEEK_API_KEY"], base_url="https://api.deepseek.com")
SYSTEM = open("prompt_en.txt", encoding="utf-8").read()   # tag list + one example; keep it fixed and first for cache hits
def label_batch(rows, retries=3):
    user = "\n".join(f"{i}|{r['text']}" for i, r in enumerate(rows, 1))
    for attempt in range(retries):
        resp = ds.chat.completions.create(
            model="deepseek-flash",
            messages=[{"role": "system", "content": SYSTEM},
                      {"role": "user", "content": user}],
            response_format={"type": "json_object"},
            max_tokens=len(rows) * 40 + 200,                # about 40 output tokens per comment, so JSON is not cut off
            extra_body={"thinking": {"type": "disabled"}},  # tagging needs no chain of thought
        )
        text = (resp.choices[0].message.content or "").strip()
        if text:                                             # JSON mode can return content with only spaces
            labels = json.loads(text)["labels"]
            if len(labels) == len(rows):
                return labels, resp.usage
        time.sleep(2 ** attempt)
    raise RuntimeError("blank or short JSON after retries")
```
Each option comes from one line in the docs. Here is why each one is there.

`response_format={"type": "json_object"}` turns on JSON output. The DeepSeek JSON page lists four notes (DeepSeek JSON Output). The prompt must contain the word "json" and an example. `max_tokens` must be large enough, or the JSON can stop in the middle.

The fourth note matters most. If the output is not valid JSON, the API replaces it with spaces. The `content` can look empty even when `finish_reason` is `stop`. The docs say to handle blank content in code, for example with a retry.

So our code strips spaces before the check. It waits and retries on a blank result. It also checks that the label count equals the comment count. In our 7 full-JSON requests, no reply was blank, and all 602 labels matched the input rows.

But the relay twice sent a plain-text timeout message after 46 and 107 seconds. Its `usage` showed a round 1,000 tokens, and `finish_reason` was still `stop`. A status check alone would call that a success. `json.loads` (Python json docs) and the count check caught it.

*The DeepSeek JSON Output page, read on November 9, 2026. A batch script must handle a response that looks fine but is empty.* DeepSeek JSON Output screenshot

JSON mode gives you valid JSON or blank text. It does not promise the right fields or row count. So keep the count check.

For a stricter structure, DeepSeek tool calls have a `strict` mode (DeepSeek Tool Calls). You use the beta base URL and set `strict: true` on each function. The model then follows the JSON Schema of the function. This feature is in beta, and we did not test it.

`extra_body={"thinking": {"type": "disabled"}}` turns off thinking mode. The DeepSeek thinking page says that `deepseek-flash` has thinking on by default, at effort high (DeepSeek Thinking Mode). The model first writes a chain of thought in `reasoning_content`, then the answer. A short tag does not need long reasoning, and the reasoning text is also model output. In an OpenAI-compatible reply, it shows up as `completion_tokens_details.reasoning_tokens`, and the OpenAI docs say that these tokens bill as output (OpenAI Reasoning models).

On r/DeepSeek, the V4.1 Flash release post had 837 points. The top comment, with 136 points, is four lines of "Hmm" that joke about how the model thinks (r/DeepSeek). We sent the setting that turns thinking off on every request. But 4 of the 9 requests still returned reasoning. The `completion_tokens_details.reasoning_tokens` values were 4,041, 4,046, 6,137 and 8,010. The labels themselves were a few hundred to about 2,600 tokens.

So those 4 requests cost 2 to 3 times the normal fee. At off-peak prices, the 9 requests cost $0.0300, and $0.0133 of that (44%) was reasoning that should not exist. We cannot tell if the relay dropped the setting or if the model ignored it. One rule is clear: read `reasoning_tokens` on every reply. If it is not 0, log it, and resend if you need to.

| Request | Sent (SGT) | Rows | Seconds | prompt_tokens | completion_tokens | reasoning_tokens | Off-peak cost | 
|---|---|---|---|---|---|---|---|
| English, batch 1 | 03:21:27 | 100 | 97 | 4,804 | 3,290 | 0 | $0.002685 | 
| English, batch 2 | 03:23:04 | 100 | 77 | 4,371 | 3,424 | 0 | $0.002700 | 
| English, batch 3 | 03:25:34 | 13 | 45 | 946 | 4,436 | 4,046 | $0.002789 | 
| Chinese, batch 1 | 03:25:34 | 100 | 44 | 2,058 | 6,564 | 4,041 | $0.004240 | 
| Chinese, batch 2 | 03:26:18 | 100 | 60 | 1,966 | 8,779 | 6,137 | $0.005555 | 
| Chinese, batch 3 | 03:27:18 | 89 | 79 | 2,571 | 2,721 | 0 | $0.002009 | 
| Spanish, batch 1 | 03:27:26 | 100 | 86 | 4,570 | 3,961 | 0 | $0.002917 | 
| English, short codes | 03:29:30 | 100 | 72 | 4,810 | 1,297 | 0 | $0.001500 | 
| Chinese, short codes | 03:29:30 | 100 | 56 | 2,063 | 8,860 | 8,010 | $0.005618 | 
| Total | 03:21-03:30 | 802 | - | 28,159 | 43,332 | 22,234 | $0.030013 | 

Last, concurrency and rate limits. The DeepSeek rate limit page sets 2,500 concurrent requests per account for `deepseek-flash` (DeepSeek Rate Limit). The limit is per account, not per key, and excess requests get 429. If inference does not start within 10 minutes, the server closes the connection. While you wait, a non-streaming request gets blank lines.

At 100 comments per batch, 10,000 comments need about 90 requests. That is far below the limit. In our run, a batch of 100 took 44 to 97 seconds, so send requests in parallel. The real bottleneck is data calls. Each EveryInfra key has a per-minute limit, and our test key allows 10 per minute.

So data calls wait in a queue, and model calls can all go at once. The DeepSeek error page lists 402 for no balance, 429 for rate limits, and 500 and 503 for server errors (DeepSeek Error Codes). Retry on 429, 500 and 503. Stop on 402.

If you prefer an AI coding tool, you can connect the EveryInfra remote MCP server. Then the model calls the data tools itself. This is a configuration example. Check the field names in the docs of your tool:


```
{
  "mcpServers": {
    "everyinfra": {
      "type": "http",
      "url": "https://api.everyinfra.com/mcp",
      "headers": { "Authorization": "Bearer ${EVERYINFRA_API_KEY}" }
    }
  }
}
```
For a 10,000-comment batch, we prefer the direct script. Data calls and tagging are fixed steps, and the model makes no choices between them. With a direct script, you can match each charge to a line in your log. For MCP setup, see remote MCP setup and the MCP server page.

Here is one full round trip. First, the script calls EveryInfra `reddit/comments` with `limit: 100`. The URL is a post about a Washington dairy worker in hospital. At 00:20:36 SGT, the API returned HTTP 200 with `count: 58` and `billing.charged_usd: 0.000556`.

The catalog lists eight `response_fields` for `reddit/comments`: `id`, `text`, `author_username`, `like_count`, `community`, `url`, `posted_at` and `platform`. But these 58 rows, and the rows from our other three Reddit comment calls, have no `community` field. They have the other seven. To group by subreddit, take the name from the post URL. We reported this mismatch to the API team.

The first row says "Yes. One of the 3 cases died today." It has 66 likes and a time of 2026-11-02T04:30 UTC. We keep only `id` and `text` for cleaning. We hide author names when we save the file.

Second, the clean comments become one user message, one "line number|text" per row. The fixed system prompt goes first. It lists 9 tags, 6 stances and the fact-check rule, with one input and output example. We sent the first English batch of 100 at 03:21:27 SGT. The reply came 97 seconds later with `prompt_tokens: 4804`, `completion_tokens: 3290` and `reasoning_tokens: 0`.

The real label for "Yes. One of the 3 cases died today." was `{"i":1,"tags":["spread_pandemic_risk"],"stance":"informative","needs_fact_check":true}`. The model saw a claim about cases and asked for a human check, which is what we want. The model flagged 70 of 213 English rows, 65 of 289 Chinese rows and 40 of the first 100 Spanish rows.

Measured per comment, English input is about 47.5 tokens, with the system prompt shared across the batch. The full JSON label is about 33.4 tokens. In Chinese, input is about 22.8 tokens and the label is about 27.3, so the label is longer than the comment.

This chart is the source of the main finding. It is also where the measured numbers differ most from our first estimate. Before the run, we used the DeepSeek ratios (DeepSeek Token & Token Usage). One English character is about 0.3 tokens. One Chinese character is about 0.6 tokens. The real count is in the `usage` field.

After the run, English input matched the estimate (0.997 times). Spanish input was 27% higher, and Chinese input was 70% higher. Emoji, full-width marks and numbers in Chinese comments cost more than 0.6 tokens each. Full JSON labels were longer than the estimate in all three languages. They were 33.4 tokens against 26.4 in English, 27.3 against 21.3 in Chinese, and 39.6 against 25.2 in Spanish. Output also costs 4 times as much as input.

So at peak with full JSON, output is 73.8% of the English model fee, 82.7% of the Chinese fee and 77.6% of the Spanish fee. The estimate said 70.3%, 87.3% and 76.1%. The main finding stays the same. The total changes.

| Sample | Input tokens, measured ÷ estimate | JSON output per comment, estimate → measured | 10,000 comments, peak, full JSON: estimate → measured | 
|---|---|---|---|
| English | 0.997 | 26.4 → 33.4 | $0.4176 → $0.5032 (+20%) | 
| Chinese | 1.70 | 21.3 → 27.3 | $0.2502 → $0.3386 (+35%) | 
| Spanish (100 rows) | 1.27 | 25.2 → 39.6 | $0.3495 → $0.5383 (+54%) | 

Now we add data fees and model fees for 10,000 raw comments. These are the rules for the estimate. The data fee is the full sample bill, search included, divided by sample rows, times 10,000. Comments sent to the model equal 10,000 times the keep rate. Input and output tokens use the measured `usage` averages, without the extra reasoning tokens. We did not measure Spanish short codes, so those cells are empty.

Prices come from the DeepSeek pricing page and the EveryInfra catalog on November 9. The estimate scales prices only. It adds no cases that the sample lacks. For example, the sample has no very long comments, so the total has none.

| Case (10,000 raw comments, extrapolated) | English | Chinese | Spanish | 
|---|---|---|---|
| EveryInfra data, search included (comment calls only) | $0.2176 ($0.1451) | $0.5828 ($0.2636) | $0.0984 ($0.0656) | 
| Comments sent to the model | 9,261 | 8,550 | 8,791 | 
| Flash peak, full JSON (measured tokens) | $0.5032 | $0.3386 | $0.5383 | 
| Flash off-peak, full JSON | $0.2516 | $0.1693 | $0.2691 | 
| Flash peak, short codes (measured tokens) | $0.2781 | $0.1400 | not measured | 
| Flash off-peak, short codes | $0.1391 | $0.0700 | not measured | 
| Total, highest to lowest | $0.7208 to $0.3567 | $0.9214 to $0.6528 | $0.6367 to $0.3675 | 
| For comparison: first estimate, peak, full JSON | $0.4176 | $0.2502 | $0.3495 | 

You can read this table in three ways. First, the total is low. The most expensive case is Chinese at peak with full JSON. It costs $0.9214 for 10,000 comments, from data calls to tags. That is about $0.09 per 1,000 comments.

Gilardi and others put ChatGPT below $0.003 per label in 2023. At that upper bound, 10,000 labels cost $30. In our English sample, the measured Flash model fee is about $0.000054 per comment. That is about one fiftieth of the 2023 bound. The tasks and models are different, so this only shows the change in scale.

Second, the larger cost depends on the language. In Chinese, the data fee of $0.5828 is 1.7 times the peak model fee of $0.3386. Chinese comments are short, so the model is cheap. Chinese posts have few comments, Bilibili comments stop at 10 per call, and one Douyin search cost $0.008333. Spanish is the reverse.

Its data fee is $0.0984, and the model fee is more than 5 times that. So what to fix first depends on your sources. With popular videos and many comments, the model fee is larger. With many small posts, the data fee is larger, and you fix it by picking posts and searching less, not by editing prompts.

Third, the real levers on the model side are output format and time of day. Batch size and cache matter little. On the same 100 comments, short codes cut output from 3,290 to 1,297 tokens in English. In Chinese, they cut it from 2,523 to 850 tokens, without reasoning.

The peak model fee falls from $0.5032 to $0.2781 in English (-44.7%) and from $0.3386 to $0.1400 in Chinese (-58.7%). That is less than our estimate of 60% to 70%, because line numbers, pipes and codes also cost tokens. Off-peak hours then cut it in half. Together, the model fee falls to about 28% of the start in English and 21% in Chinese.

Many batch guides spend a long time on batch size and cache hits. Here are the DeepSeek cache rules (DeepSeek Context Caching). A cache hit needs a full match on a stored prefix unit. The system stores units at request boundaries and at common prefixes that it finds.

The `usage` field shows `prompt_cache_hit_tokens` and `prompt_cache_miss_tokens`. The cache is best effort, with no promise of a hit. Unused cache clears in a few hours to a few days. Our system prompt is fixed and comes first, so from the third request it can hit the cache.

The table below is our first estimate from the doc ratios. Use it only to compare batch sizes. In the live run, 8 of 9 requests had 0 to 102 `cached_tokens`, less than one full system prompt. The one exception was the Spanish batch that we resent after a relay timeout: 984 tokens hit the cache. That agrees with the estimate that cache saves very little, unless a whole prefix repeats.

| Batch size | Requests (English) | Output tokens per request (English) | English peak, cache | English peak, no cache | Chinese peak, cache | Spanish peak, cache | 
|---|---|---|---|---|---|---|
| 25 | 371 | 672 | $0.4220 | $0.4508 | $0.2541 | $0.3538 | 
| 50 | 186 | 1,332 | $0.4190 | $0.4334 | $0.2515 | $0.3510 | 
| 100 | 93 | 2,652 | $0.4176 | $0.4247 | $0.2502 | $0.3495 | 
| 200 | 47 | 5,292 | $0.4168 | $0.4203 | $0.2495 | $0.3488 | 
| 400 | 24 | 10,572 | $0.4165 | $0.4182 | $0.2492 | $0.3485 | 

From 25 to 400 per batch, the English peak fee falls from $0.4220 to $0.4165. That is 1.3%. At 100 per batch, the cache saves $0.0071, about 1.7%. The reason is simple.

Batch size and cache only save repeated input of the system prompt. The prompt is two or three hundred tokens, which is a small part of all input. Neither one changes output, which is the large part.

So choose the batch size for other reasons. A large batch means that one bad JSON reply makes you retry many rows. At 400 per batch, each reply is about 10,000 output tokens. That is far below the 384K limit.

But one blank reply means a full rerun of 400 rows. We suggest 50 to 100 comments per batch. Retries stay small, and the request count stays low.

DeepSeek peak hours are 01:00-04:00 and 06:00-10:00 UTC, Monday to Friday, except Chinese public holidays (DeepSeek pricing page). All other hours are off-peak, including weekends and Chinese public holidays. Off-peak prices are half of peak prices. One video calls this "outside Asian work hours." That is not quite right. In Singapore time, peak is 09:00-12:00 and 14:00-18:00, so the lunch break is off-peak.

*Tagging does not need to be real time. Put the whole batch in off-peak hours, and the model fee is half.*

| Place (time zone) | Peak hours (local, on DeepSeek weekdays) | Suggested batch window | 
|---|---|---|
| UTC | 01:00-04:00, 06:00-10:00 | 10:00 to 01:00 next day | 
| US Pacific (UTC-8 from November) | 17:00-20:00 and 22:00-02:00, starting the evening before | 02:00-17:00 | 
| US Eastern (UTC-5 from November) | 20:00-23:00 and 01:00-05:00, starting the evening before | 05:00-20:00 | 
| Singapore (UTC+8) | 09:00-12:00, 14:00-18:00 | After 18:00, or 12:00-14:00 | 
| Mexico City (UTC-6) | 19:00-22:00 and 00:00-04:00, starting the evening before | 04:00-19:00 | 

For a US team, the easy plan is to run tagging during the working day. Peak hours in the US fall in the evening and night. A team in Washington State can start a job at 09:00 Pacific time and pay off-peak prices. Weekends are all off-peak too. Note the holiday rule. Chinese public holidays are off-peak even on weekdays.

Do you need results at a fixed time, such as a morning meeting? Then run data calls and tagging in an off-peak window the day before. Read the results in the morning. Tagging almost never needs to run at peak.

Output is 74% to 83% of the model fee, so the simple fix is less output. Full JSON repeats key names, quotes and full tag names for each comment. A short code puts the same facts on one pipe-separated line. In English, "123|r,d|w|1" means comment 123, tags raw milk and distrust, stance worried, and fact check needed.

The script expands the codes with a lookup table. Our live short-code prompts used this format. All 100 English lines and all 100 Chinese lines passed the format check.

| Output format | English tokens per comment (measured) | Chinese (measured) | Spanish | Good | Cost | 
|---|---|---|---|---|---|
| Full JSON ( `json_object` ) | 33.4 | 27.3 | 39.6 (measured) | The API returns valid JSON, so parsing is easy | Output is 74%-83% of the model fee | 
| Short codes (plain text) | 13.0 | 8.5 | not measured | 61%-69% less output; 100 of 100 lines valid | No JSON mode, so you check the format | 
| Only rows that need a fact check | Depends on the share | Depends | Depends | Least output | You cannot tell a "no" from a missed row | 

The cost of short codes is the loss of the JSON-mode format promise. We suggest both. Use short codes for the main run. Check each line with a regular expression (Python re docs).

The same module gives the `\b` word boundary for keyword rules. If a batch has the wrong line count or an unknown code, rerun that batch once in full JSON mode. Short codes still cost less as long as you rerun fewer than half of the batches.

The third format looks cheapest, but it is risky in a batch. If the model skips a row, you cannot tell why. Maybe it decided that no check was needed. Maybe it missed the row. In public health work, one missed "raw milk protects you" claim costs more than the few cents that you save.

Short codes also help with retries. Each line has a row number. So the script can find the exact missing rows and resend only those. In full JSON mode, a blank reply means a rerun of the whole batch. That is why short codes are more stable at large batch sizes.

The samples also had findings of their own. They decide how to design the tag list.

| Sample | Top rule tag (exactly one hit) | Most-liked kept comment | Official page to check against | 
|---|---|---|---|
| English | distrust_conspiracy, 13 | A conspiracy claim, 324 likes | USDA APHIS, CDC | 
| Chinese | herbal medicine and food use, 11 | Asks if the video maker was pressured after a factory visit, 673 likes | China Food Safety News | 
| Spanish (word boundaries) | ganado_y_moscas, 38 | Farmers get no help, 90 likes | USDA APHIS | 

In the English sample, the three most-liked comments (324, 221 and 153 likes) all got the rule tag `distrust_conspiracy`. Of the 46 high-confidence rule rows, the model labels agree with the rule on 35. Most of the rest are rows where the rule saw words like "lab" and the model saw talk about spread risk. The `needs_fact_check` flag finds the claims that a team can compare with the USDA and CDC pages (USDA APHIS, 2026-11-04; CDC current situation).

In the Chinese sample, the top rule tag was herbal medicine and food use, with 11 of 38 high-confidence comments. One comment with 157 likes says that all dried herbs and dried foods need checks, not only bai zhi. The model labels include the rule tag on 34 of the 38 rows, the best agreement of the three languages. Many commenters mix up the sulfur dioxide rules for bai zhi as a medicine and as a spice. The fact-check flag must catch that. Check the claims against the source that the news report quotes (China Food Safety News, in Chinese).

In the Spanish sample, with word boundaries, the top rule tag was cattle and flies, with 38. Agreement between rules and the model is lowest here: 15 of 38 high-confidence rows in the first 100. The substring errors above explain most of that. Of the 27 several-tag rows, only 2 hit both cattle and avocado.

Rules almost never see a comment that mixes the two pests. The model marked 14 of the first 100 rows as `confunde_ambos` (mixes both). That is why we added the `especie` field. For the official facts, see these two pages (USDA APHIS screwworm status; The Food Tech, in Spanish).

| Limit | Effect | What we did | 
|---|---|---|
| No official DeepSeek key; live run through a third-party relay | Tokens are real `usage` , but costs use the list price, not a DeepSeek invoice. The relay may affect the thinking setting and timeouts. | We keep each raw request and reply. We log timeouts and reasoning tokens apart. | 
| Only 602 comments and 9 requests measured | Spanish has 100 rows and no short codes. The batch-size table is an estimate. | Each cell says measured or not measured | 
| 10,000 is extrapolated | 907 rows only. The mix of busy and quiet posts changes rows per call and the share of search fees. | Data fees show two numbers: with search and comment calls only | 
| Thinking mode is on by default, and "off" can fail | 4 of 9 requests still returned reasoning, at 2 to 3 times the fee | Read `reasoning_tokens` on every reply | 
| JSON mode can return blanks | It looks fine but has no labels | Strip, count, wait and retry | 
| Cleaning rules drop good rows | Rows like "HMPV possibly?" are lost | Set the short-row limit to 2 words. Dedupe spam only on bullet comments. | 
| No human gold labels | We give agreement with rules, not accuracy | Rules act as a check. Read rows where rules and model disagree. | 
| Some capabilities cap rows per call | Bilibili comments return 10 rows at most, and small posts return 1 to 10 rows | Use search to pick busy posts. On Bilibili, pull bullet comments first. | 

The first limit is the largest. We know the token counts and row counts. We do not know if the official DeepSeek API would also miss the thinking setting or send timeouts as plain text. If you have an official key, run 100 comments first. Compare your `usage` with our measured table, and trust your own numbers if they differ.

The main findings are ratios. Output costs more than input. Short codes are shorter than JSON. Off-peak is half of peak. All three held in the live run. Short codes simply saved less than we estimated.

One more note. This article is about data methods. It does not judge the medical or legal truth of any claim. For facts about H5N1, screwworm or sulfur residue, use the health and farm agency pages that we link.

**How do I get a deepseek v4.1 flash api key?**

Create an account on the DeepSeek platform, add funds, and create an API key. Use `deepseek-flash` as the model name. The DeepSeek error page says that 401 means a wrong key and 402 means no balance (DeepSeek Error Codes). It links to the key and top-up pages.

**Is there a deepseek v4.1 flash api free option?**

The DeepSeek API bills per token, and the pricing page lists no free quota (DeepSeek pricing page). It uses granted balance before paid balance. Third-party platforms with free quotas usually have weekly limits, so they do not suit a 10,000-comment job.

**What is the deepseek v4 1 flash 1017 api cost compared with older versions?**

Off-peak, V4.1 Flash costs $0.15 per million cache-miss input tokens and $0.60 per million output tokens. A Hacker News user lists the older V4 Flash 0731 at $0.22 and $0.66 off-peak (Hacker News comment). We found no official archive of the older prices.

**Does V4.1 Flash make up labels?**

We have no human gold labels, so we cannot give an accuracy number. We can give two checks. All 602 replies had exactly one label per input row, with no extra or missing rows. Take the rows where a rule gave one clear tag. The model agreed on 34 of 38 in Chinese, 35 of 46 in English and 15 of 38 in Spanish.

Spanish is low mostly because of the rule errors. Use keyword rules as an independent check. Have a person check all rows with `needs_fact_check` set to true.

**Does turning thinking off stop reasoning tokens on the bill?**

Not always. All 9 of our requests sent `thinking: disabled`, but 4 still returned 4,041 to 8,010 `reasoning_tokens`. Those batches cost 2 to 3 times the normal fee. The thinking page says that thinking is on by default (DeepSeek Thinking Mode).

The top comment on the r/DeepSeek release post jokes about the model writing "Hmm" again and again (r/DeepSeek). Read `usage.completion_tokens_details.reasoning_tokens` on every reply. If it is not 0, log the cost and consider a resend.

1. Get an EveryInfra API key in the quickstart. Read the free catalog for capabilities and prices. See pricing for an overview.
2. Pick capabilities by platform. For English talk, use YouTube comments and Reddit comments. For Chinese talk, use Bilibili bullet comments, Douyin comments and Zhihu comments. Platform pages: YouTube API, Reddit API, Bilibili API, Douyin API.
3. Pull a sample of 200 to 300 comments. Run the cleaning script, and note the removed share and the tokens per comment.
4. Put the tag list in a fixed system prompt. Run a small batch in full JSON mode to test the tag list, and read `reasoning_tokens` in`usage` . Then switch to short codes.
5. Schedule the full batch in off-peak hours. Handle blank replies, count checks and 429 waits in the script.

- DeepSeek V4 with Claude Code for comment cleaning
- GPT-6.1 structured outputs for review tagging
- Failure refund reconciliation
- How API errors heal themselves

1. Models & Pricing, DeepSeek, read 2026-11-09
2. Change Log, DeepSeek, entry of 2026-10-17
3. DeepSeek-V4.1-Flash-1017 release note, DeepSeek, 2026-10-17
4. DeepSeek-V4.1-Flash release note, DeepSeek, 2026-09-10
5. DeepSeek-V4-Pro GA release note, DeepSeek, 2026-08-13
6. JSON Output, DeepSeek, read 2026-11-09
7. Thinking Mode, DeepSeek, read 2026-11-09
8. Context Caching, DeepSeek, read 2026-11-09
9. Rate Limit & Isolation, DeepSeek, read 2026-11-09
10. Token & Token Usage, DeepSeek, read 2026-11-09
11. Tool Calls, DeepSeek, read 2026-11-09
12. Error Codes, DeepSeek, read 2026-11-09
13. openai-python, OpenAI on GitHub, read 2026-11-09
14. openai, PyPI, read 2026-11-09
15. Social API catalog, EveryInfra, read 2026-11-09
16. DeepSeek-V4.1-Flash-1017 just dropped, Reddit r/DeepSeek, October 2026
17. Price comparison comment for V4.1 Flash, V4 Pro and V4 Flash, Hacker News, September 2026
18. A(H5) Bird Flu: Current Situation, CDC, read 2026-11-09
19. A(H5) Bird Flu Surveillance and Human Monitoring, CDC, 2026-11-06
20. USDA Confirms HPAI in Two Washington Dairy Herds, USDA APHIS, 2026-10-08
21. Update: Genetic Sequencing Results for Washington Dairy Herd, USDA APHIS, 2026-11-04
22. USDA Confirms HPAI in a Dairy Herd in Indiana, USDA APHIS, 2026-11-02
23. HPAI Confirmed Cases in Livestock, USDA APHIS, read 2026-11-09
24. Yakima County reports 2 more avian flu cases, 1 death, CIDRAP, 2026-11-03
25. HPAI (H5N1) in Cattle Producer Resources, Washington State Department of Agriculture, read 2026-11-09
26. Bird Flu (Avian Influenza), Washington State Department of Health, read 2026-11-09
27. Investigation of Avian Influenza A (H5N1) Virus in Dairy Cattle, FDA, read 2026-11-09
28. Influenza (avian and other zoonotic), WHO, 2026-01-21
29. Avian influenza weekly update, WHO Western Pacific, 2026-10-30
30. Announcement on GB 2760-2024 and 46 other national food safety standards (Chinese), National Health Commission of China, 2024-03-12
31. Sulfur-smoked bai zhi still sold under "sulfur-free" labels (Chinese), China Food Safety News, November 2026
32. Xunyang market office seizes 1.5 tonnes of sulfur-smoked bai zhi (Chinese), Xunyang District Government, 2026-11-03
33. Epidemiological Alert: Myiasis by Cochliomyia hominivorax (Spanish), PAHO/WHO, 2026-10-16
34. New World screwworm outbreak (Spanish), CDC, 2026-10-29
35. Current Status of New World Screwworm, USDA APHIS, 2026-11-05
36. USDA Confirms New World Screwworm Detection in Arizona, USDA APHIS, 2026-10-28
37. Screwworm progress bulletin No. 11 (Spanish), SENASICA, 2026-03-27
38. Can screwworm attack fruit? (Spanish), The Food Tech, 2026-11-04
39. ChatGPT Outperforms Crowd-Workers for Text-Annotation Tasks, Gilardi et al., arXiv, 2023-03-27
40. unicodedata, Python docs, read 2026-11-09
41. re, Python docs, read 2026-11-09
42. json, Python docs, read 2026-11-09
43. datasketch, GitHub, read 2026-11-09
44. datasketch, PyPI, read 2026-11-09
45. Reasoning models, OpenAI, read 2026-11-09

*Originally published on the EveryInfra blog: DeepSeek V4.1 Flash API: Clean and Tag 10,000 Comments, and What It Costs. This article was written with AI assistance; the numbers come from the API calls and sources linked above, and were checked before publishing.*

EveryInfra is a pay-per-call data API for AI agents and developers: everyinfra.com.

## Top comments (0)
