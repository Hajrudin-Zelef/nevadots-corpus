---
id: collect-261121-veille-ia/go-json/a-new-go-api-for-json-2
title: "A new Go API for JSON"
domain: go-json
role: reference
task: article
actors: []
dates: ["2026-11-05"]
keywords: ["json", "source", "judge", "rubric", "encoding", "claim", "stable", "post"]
source: https://rubric.hrih.si/editions/2026-11-05/2/
source_anchor: ""
source_lines: [1, 59]
section: "Go_JSON"
---
The Go team announced that encoding/json/v2 and encoding/json/jsontext are stable in Go 1.27, a status the post itself doesn't source. v2 rejects invalid UTF-8 and duplicate member names, marshals nil slices and maps as empty values, and matches field names case-sensitively. Unmarshal is up to 10x faster than v1, while Marshal is roughly equal. Larger gains need the streaming MarshalJSONTo and UnmarshalJSONFrom methods; the Kubernetes speedup is unverified. v1 remains supported and runs on v2 options, so migration can be gradual.

## Why it ranked

No. 2 of 139 with a score of 0.75. The panel rated it highest on new information (0.93) and technical depth (0.89), lowest on audience breadth (0.64).

- AI judgeJudge 2claude-haiku-5-5
- Detailed technical release of Go's stable JSON v2 packages with substantive design explanation, broadly relevant to Go and backend builders; not covered in recent stories, well documented.
- AI judgeJudge 3gpt-5.6-luna
- A major, technically detailed Go standard-library release addressing JSON correctness, streaming, customization, compatibility, and performance. It is well-supported and directly useful to Go developers, with modest ecosystem breadth and some security relevance.

Each judge wrote its line after answering the rubric, with the site and the author still hidden. Every answer behind this is in the table under Score.

## Score

**0.75**total under rubric v2.3

| Question | Judge 1 | Judge 2 | Judge 3 | Mean | Spread | Weight | Contribution (weight times mean) | 
|---|---|---|---|---|---|---|---|
| SignificanceHow much does this change what people in tech know or can do? | 0.79 | 0.55 | 0.72 | 0.69 | 0.24 | +0.24 | +0.165 | 
| Reader interestWould a well-informed senior engineer or tech founder want to read this today, and likely pass it on? | 0.71 | 0.78 | 0.88 | 0.79 | 0.17 | +0.26 | +0.205 | 
| Audience breadthHow many people who build technology does it concern? | 0.67 | 0.67 | 0.58 | 0.64 | 0.09 | +0.13 | +0.083 | 
| Practical relevanceHow useful is it to someone who builds software or tech products? | 0.89 | 0.80 | 0.82 | 0.84 | 0.09 | +0.22 | +0.184 | 
| SubstantiatedHow well is it backed? Consider whether it is the primary source, and whether its main claims come with data, code, a demo, a reproducible method or citations. | 0.78 | 0.85 | 0.93 | 0.85 | 0.15 | +0.05 | +0.043 | 
| Technical depthHow technically substantive is it? | 0.89 | 0.90 | 0.88 | 0.89 | 0.02 | +0.05 | +0.044 | 
| New informationDoes it contain information absent from the stories covered in the last 7 days (list provided)? | 0.91 | 0.90 | 0.98 | 0.93 | 0.08 | +0.05 | +0.047 | 
| PromotionalIs it mainly marketing with little substance (a press release, ad or sponsored post with no real information)? A launch post that carries real news is not promotional. | 0.07 | 0.05 | 0.03 | 0.05 | 0.04 | −0.19 | −0.010 | 
| OverclaimDo the headline or conclusions claim more than the body supports? | 0.15 | 0.15 | 0.08 | 0.13 | 0.07 | −0.11 | −0.014 | 
| Sum of contributions |  |  |  |  |  |  | +0.748 | 
| Claim-check penalty |  |  |  |  |  |  | 0.000 | 
| Total, as published |  |  |  |  |  |  | 0.748 | 

Spread is the highest minus the lowest judge answer. marks a spread above 0.40: the judges disagree, and the story is marked contested.

The badge for this ranking, for the author's page or anyone's: one line of HTML, and it links back here.

`<a href="https://rubric.hrih.si/editions/2026-11-05/2/"><img src="https://rubric.hrih.si/badge/2026-11-05/2.svg" alt="Ranked #2 on Rubric, 5 Nov 2026" width="231" height="28"></a>`
## Claim check

Claim check: 3 matches source · 2 no source given · 3 couldn’t check. Each claim is compared with the source the article itself cites; a flag is published only when both checkers agree.

1. No source givenVersion The encoding/json/v2 and encoding/json/jsontext packages are stable and generally available in Go 1.27. The article gives no link or citation for it.
2. Matches sourceBenchmark result The Unmarshal performance of v2 is significantly faster than v1, with benchmarks showing improvements of up to 10x. The cited source says the same thing. Source cited by the article: github.com/go-json-experiment/jsonbench
3. Matches sourceBenchmark result Marshal performance of v2 is roughly at parity with v1, sometimes slightly faster and sometimes slightly slower. The cited source says the same thing. Source cited by the article: github.com/go-json-experiment/jsonbench
4. Couldn’t checkCausal conclusion Switching to UnmarshalJSONFrom improved performance by orders of magnitude in a Kubernetes service whose recursive parsing of OpenAPI specifications in UnmarshalJSON methods had hurt performance. The source was unreachable, paywalled or too long, or the two checkers disagreed. Source cited by the article: github.com/kubernetes/kube-openapi/issues/315
5. Couldn’t checkCausal conclusion Duplicate JSON object member names have been exploited before, as in CVE-2017-12635. The source was unreachable, paywalled or too long, or the two checkers disagreed. Source cited by the article: nvd.nist.gov/vuln/detail/CVE-2017-12635
6. Couldn’t checkNumber A survey indicated that most Go users prefer nil slices and maps to be marshaled as an empty JSON array or object by default. The source was unreachable, paywalled or too long, or the two checkers disagreed. Source cited by the article: go.dev/issue/63397
7. No source givenDate The v2 effort took about 6 years to reach this point. The article gives no link or citation for it.
8. Matches sourceVersion encoding/json/v2 was adopted as a Go experiment in Go 1.25. The cited source says the same thing. Source cited by the article: go.dev/issue/71845

## Also covered by

No other coverage was clustered with this story.

## Elsewhere

Elsewhere: HN 266 pts, front page

Recorded for context only, checked 5 Nov 2026, 05:46 UTC. It played no part in the ranking.
