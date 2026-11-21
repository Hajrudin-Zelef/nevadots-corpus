---
id: collect-261121-veille-ia/deepseek/deepseek-updates-v4-1-flash-for-complete-projects-and-opens-search-apis-to-developers
title: "DeepSeek Updates V4.1 Flash for Complete Projects and Opens Search APIs to Developers"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-10-17"]
keywords: ["deepseek", "search", "flash", "complete", "developers", "model", "projects", "tools"]
source: https://asiadaily.org/news/17934/
source_anchor: ""
source_lines: [1, 113]
section: "DeepSeek"
---
## A coding upgrade arrives with a new search bill

DeepSeek released V4.1-Flash-1017 on October 17, 2026, updating its website, app and API while opening the search engine behind its services to outside developers. The company says the model can build more complete projects from short instructions, finish tasks faster and produce shorter answers than the previous V4.1 Flash version.

The release combines two parts of an AI developer's workflow: software that writes and revises code, and services that find current information online. DeepSeek's new Search and Fetch interfaces each cost $0.75 per 1,000 successful requests. Its Chinese pricing lists each at 5 yuan per 1,000 successful requests.

Developers using certain tools have a temporary exemption. Search and Fetch calls made through tools running on DeepSeek's servers in DeepSeek Harness, Claude Code and Codex are currently free for DeepSeek API users. Billing begins at 16:00 UTC on October 31, equivalent to midnight Beijing time on November 1, 2026.

DeepSeek's English release announcement says the update improves benchmark performance while reducing task duration and output length. Its practical promise is straightforward: an automated coding task should take less time and generate less billable text.

The company has also made the updated model weights available through Hugging Face and ModelScope. The release extends V4.1 Flash, introduced on September 10, rather than launching a separate model family. Those releases are 37 days apart, putting this update on a relatively short development cycle.

## What building a complete project means

DeepSeek is targeting work that goes beyond answering a programming question or producing a single function. An AI agent can plan a project, create files, run tools, inspect the result and revise its work. A complete project may require many such actions before a user has something usable.

Four demonstrations illustrate the ambition: a 3D model of Beijing's Forbidden City, a racing game played in a browser, a recreation of Portal 2 using the Godot game engine, and Valley of Ages, a changing 3D scene tracing development from prehistoric times to the modern era.

DeepSeek says the demonstrations were created inside DeepSeek Harness with relatively few prompts, limited reference material and few rounds of interaction. The announcement gives no exact prompt counts or completion times for those examples.

The company attributes the gains to reinforcement learning with self feedback. In simple terms, this is training that uses feedback on attempted work to help the model make better choices later. Here, the intended improvement is carrying a task through several connected steps, rather than producing a convincing first answer and stopping.

For someone commissioning a browser game or a visual prototype, the useful change would be fewer rounds spent asking the system to repair missing pieces. DeepSeek's demonstrations focus on that broad project workflow, including visual construction and interactive software.

## Where the benchmark gains appear

DeepSeek's published evaluation figures put the new checkpoint at 77.0 on DeepSWE v1.1, compared with 74.2 for the previous V4.1 Flash. That is an increase of 2.8 points. The evaluation uses the mini-swe-v2 harness, the software environment that runs and checks the tasks.

Terminal-Bench 4.0 shows a larger numerical gain, from 31.2 to 42.3, or 11.1 points. Automation-Bench v1.0 rises from 54.8 to 60.5, while ProgramBench increases from 20.3 to 29.1. These are DeepSeek's own evaluation figures, with different tests examining different kinds of work.

The comparison with competing models varies by task. DeepSeek's table places V4.1-Flash-1017 above Claude Opus 5 on DeepSWE, at 77.0 versus 74.0. Claude Opus 5 scores higher on Terminal-Bench, at 51.8 versus 42.3, and ProgramBench, at 37.0 versus 29.1.

Visual and reasoning evaluations also improve over the prior Flash checkpoint. BabyVision with tools rises from 89.6 to 92.5, while Humanity's Last Exam without tools rises from 36.8 to 42.6. The pattern gives developers a practical reason to test the updated model on their own workloads: the size of the gain depends on the task.

DeepSeek describes the combination of better results and lower costs as a Pareto improvement. Put simply, the company says users receive better performance while spending less, rather than paying more for the improvement. Its Chinese announcement says that relationship holds across several harnesses.

## Why the surrounding software matters

DeepSeek recommends upgrading DeepSeek Harness to version 0.2.1-rc.1 or later. The company also says it has optimized the model for Tencent CodeBuddy and WorkBuddy. A harness manages the files, tools, sessions and actions around a model, turning its decisions into steps a computer can execute.

Harness version 0.2.1-rc1 adds plugin management features, including installation from custom plugin indexes and controls to enable, disable, remove and update plugins. A new entry point lets users ask an agent to create a plugin, starting execution after they submit their request.

Other additions address everyday development work. An experimental Git Worktrees plugin lets an agent create and enter a separate checkout of a repository, giving it an isolated place to work on files. A working_directory tool allows software to inspect or change the folder used for a session.

The release also carries forward experimental support for Claude Code Mods. That work follows the October compatibility tests, when three example mods could run and four others lacked required capabilities. The stated goal remains testing how Claude Code's extension interfaces fit within DeepSeek Harness plugins; full practical compatibility is still outside the experiment's present scope.

For developers, the model upgrade and the harness upgrade address different needs. The model chooses what to do, while the harness supplies the controls needed to manage files, extensions and execution.

## Search finds pages, Fetch reads them

Search and Fetch perform separate jobs. Search accepts a query and returns structured web results. Fetch takes an individual webpage and returns its title and body as Markdown, a plain text format that preserves basic structure such as headings and lists.

A research agent could use Search to find a company's latest announcement, then call Fetch to read the selected page. A coding agent could follow the same pattern to locate documentation and bring the relevant text into its task. Each successful call to either service is counted separately.

Developers can use their existing DeepSeek API credentials. Both interfaces use the same API host and Bearer authentication, meaning a request carries an access token identifying the account. The endpoints are POST /v1/tools/search and POST /v1/tools/fetch.

Search accepts queries of up to 128 characters and returns between one and 50 results, with 10 as the default. Options include requesting page content and using operators such as site:, lang:, after: and before: to narrow results by website, language or date.

Result fields include titles, web addresses, summaries, publication dates where available, and information intended to help a model rank and assess pages. Those fields include a reranking score from zero to three and an authority tier from zero to four. Longer summaries and optional full text give applications more material to work with before making another request.

## What the search charges cover

Search charges apply when a request succeeds and returns a nonempty result list. Fetch charges apply when it successfully returns page content with an HTTP 200 response, the standard status code for a successful web request. Failed requests, empty results and timeouts are not charged.

The number of search results requested does not change the price of a successful Search call. A request returning 50 results therefore carries the same Search fee as one returning 10. Fetching selected pages afterward adds separate successful Fetch calls.

For example, 1,000 successful Search requests and 1,000 successful Fetch requests would cost $1.50 under the dollar schedule, or 10 yuan under the Chinese schedule. That calculation covers the two retrieval services. Any model activity used to plan the searches or read the returned text has its own usage charges.

Each account is limited to five requests per second for Search and five per second for Fetch, with the services counted independently. Fetch also has additional limits by domain. Requests exceeding a limit receive HTTP 429, a response telling the application it is sending requests too quickly.

These limits matter when several agents run at once. Applications need to pace requests and handle temporary failures, especially when many tasks try to retrieve pages from the same website.

## The engine behind the public interfaces

DeepSeek's Chinese release announcement says its search engine already supports the company's website and API, handling hundreds of millions of requests daily from around the world. The newly opened interfaces give developers access to that existing service.

The company says the engine indexes hundreds of billions of webpages and supports searches in multiple languages. It describes its results as selected for relevance, authority and timeliness, with filtering aimed at search manipulation and low quality content generated by AI.

DeepSeek specifically names SEO cheating and GEO manipulation among its targets. SEO means search engine optimization, the practice of making content easier to find through search. GEO refers to attempts to influence what generative AI systems retrieve or include in their answers. Manipulation can involve making a page appear more useful or authoritative than its content warrants.

For an AI application, retrieval quality affects every later step. A model writing an answer or building software may rely on the pages a search service selects. DeepSeek is positioning its filtering and ranking as part of the service developers receive alongside the raw ability to search.

## Open weights and deployment choices

DeepSeek has published V4.1-Flash-1017 model weights on Hugging Face and ModelScope. Weights are the numerical settings learned during training. Making them downloadable allows developers to run the model on infrastructure they control rather than relying entirely on a hosted service.

The release is described as using the MIT license and includes inference code and deployment instructions. Inference is the process of running a trained model to generate an answer. Those materials give organizations a route to operating the model themselves, subject to their computing capacity.

The underlying V4.1 Flash architecture supports text and image input, a context window of one million tokens and outputs of up to 384,000 tokens. Tokens are small units of information, often words or pieces of words. A larger context window lets a model receive more material in one task, such as code, instructions and reference text.

Developers continuing with the hosted API can use the deepseek-flash model name. Downloading the model and using the hosted Search and Fetch services are separate deployment choices: an organization can control where model inference runs while deciding independently how it retrieves current web information.

## The dates developers need to track

DeepSeek's release schedule connects the original Flash launch, the October update and the start of retrieval billing. The main dates are:

- September 10, 2026: V4.1 Flash was released.
- October 3, 2026: DeepSeek Harness v0.2.1-alpha.1 introduced experimental Claude Code Mods compatibility.
- October 17, 2026: V4.1-Flash-1017 and the public Search and Fetch interfaces were announced.
- October 31, 2026, at 16:00 UTC: Billing starts for eligible Search and Fetch calls through tools running on DeepSeek's servers.
- November 1, 2026, at 00:00 Beijing time: The same billing change takes effect in China's local time.

The temporary exemption applies to the specified calls through tools in DeepSeek Harness, Claude Code and Codex. Developers making direct requests should budget using the published successful request prices. The October 17 launch brought the coding upgrade and retrieval services together; the November deadline is the next concrete change for users of the exempt tools.

## Key Points

- V4.1-Flash-1017 updates DeepSeek's website, app and API with a focus on complete project development.
- DeepSeek's evaluations show DeepSWE rising from 74.2 to 77.0 and Terminal-Bench from 31.2 to 42.3.
- DeepSeek recommends Harness version 0.2.1-rc.1 or later.
- Search finds webpages; Fetch retrieves an individual page's title and content.
- Each service costs $0.75, or 5 yuan under Chinese pricing, per 1,000 successful requests. Failures, empty results and timeouts are free.
- Eligible calls through tools remain free until midnight Beijing time on November 1, 2026.
- Updated model weights are available through Hugging Face and ModelScope.
