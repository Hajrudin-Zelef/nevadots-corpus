---
id: collect-261121-veille-ia/divers-tech/a-round-is-not-a-review
title: "A round is not a review"
domain: divers-tech
role: reference
task: article
actors: []
dates: ["2026-10-20"]
keywords: ["review", "code", "most", "more", "round", "claude", "person", "author"]
source: https://www.dustcrawl.com/blog/agentic/2026-10-20/
source_anchor: ""
source_lines: [1, 325]
section: "Divers_Tech"
---
### The fourth pass found the old lines.

              
                On 15 October 2026 the
                Claude Code team published
                how they review code once
                agents are writing most of
                it. They say Claude writes
                about 90 percent of Claude
                Code’s own code. From June
                to September, merged pull
                requests to that codebase
                more than doubled, and the
                share that had to be
                reverted fell by more than
                40 percent. They did not
                remove review. They moved
                most of the reading to
                machines, and spent the
                saved attention on making
                mistakes fast to undo.
              

              
                The line they want people
                to stop using is “did a
                human read the code?” That
                is a weak stand-in for
                “what could go wrong with
                this change, and which
                check is the right one to
                catch it?” Review now runs
                while the change is being
                written, more like
                spellcheck than a gate at
                the end. Most changes have
                already been reviewed and
                fixed by Claude once before
                a person is asked to look.
                The goal they state is that
                most pull requests merge
                with at most one human in
                the loop, who can be the
                author, and that reverts
                and incidents do not rise.
                If those rates rise, human
                time was not saved. It was
                moved into the cleanup.
              

              
                The failure mode they hit
                is the one that shows up
                when the author is also an
                agent. A person can wave
                off a nit in seconds. An
                agent fixes it, which means
                another commit and another
                round. One CLI pull request
                went through more than
                fifty rounds of review and
                fixes overnight before its
                author stopped it. When
                they ran git blame on what
                the reviewer kept flagging,
                about 59 percent of it was
                code that had been there
                since the first pass. The
                reviewer was rarely finding
                new bugs. It was finding a
                few more old ones each
                round.
              

              
                They changed re-review.
                After three rounds, the
                reviewer asks the author to
                step back and reconsider
                the approach. Later passes
                look at the new diff, not
                the whole file again.
                Findings already posted are
                not repeated. The reviewer
                flags the class of bug, so
                the fix is every instance,
                not one line. Style,
                diagnostics, and cleanup
                comments get dropped before
                a person sees them.
                Findings are voted on by a
                separate group of agents
                and posted only if enough
                of them agree the finding
                is real. Each one is tagged
                Important, Nit, or
                Pre-existing.
              

              
                An agent’s approval never
                counts as a person’s. The
                person who prompted the
                change counts as one of the
                required reviewers, the
                same as if they had typed
                the code. Risk comes from
                the change, not the author.
                Low-risk work — docs, copy,
                internal-only features,
                additive changes behind a
                flag — can merge with no
                extra human once automated
                review and security checks
                are green, and only for
                authors on the trusted
                list. Ordinary product
                changes need one person.
                The highest-risk code, plus
                a small set of paths that
                always need two people,
                needs two. CI workflow
                files, and anything owners
                have listed as sensitive,
                always need a person.
                Approval covers one
                specific commit. An agent
                push after that, or a merge
                someone resolved by hand,
                needs the approval again.
              

              
                They are explicit about
                what machines do not own.
                People still decide whether
                to build the thing, still
                read risky code such as
                billing, the auto-updater,
                and CI, still review
                architecture that other
                changes will copy, and
                still own the outcome.
                Reading the evidence is
                part of that job: test
                output, the command that
                ran, a screenshot, the
                findings. If you cannot say
                what the checks would miss,
                they want a real review,
                not a quick approval. They
                also do not yet have
                circuit breakers that
                tighten approval when
                revert numbers move.
                Engineers there want those
                before more code moves to
                zero-human merges.
              

              
                A round that repeats is
                not a deeper review. Cap
                it. Keep a person on the
                decision the checks cannot
                make, and on the outcome
                after it ships.
              

              
                **Source:**
                claude.dev — How the Claude Code team does code review
                ·
                Simon Willison — quote, 16 October 2026
              

            
            
              ### The model name expired.

              
                On 19 October 2026 GitHub
                deprecated a set of Copilot
                models across most
                experiences, including
                Copilot Chat, inline edits,
                ask and agent modes, and
                code completions. The
                retirement was announced on
                18 September. No action is
                required to remove the old
                models from the product.
                Action is required anywhere
                a workflow still names one.
              

              
                The retired names, and the
                alternatives GitHub lists:
                Gemini 3.7 Flash moves to
                Gemini 3.8 Flash. GPT-5.5
                and GPT-5.4 move to GPT-6.1
                Sol. GPT-5.4 mini and GPT-5
                mini move to GPT-6 Luna.
                Grok 4.5 moves to Grok 4.7.
                Copilot Enterprise
                administrators may need to
                enable the replacement
                through model policies
                before it shows up in the
                Chat model selector in VS
                Code and on github.com.
              

              
                “No action required to
                remove” is about the
                picker, not about your
                pin. A workflow, a skill
                file, or an agent config
                that still says GPT-5.5 is
                calling a name the product
                retired yesterday. Update
                the pin, and confirm the
                replacement is enabled in
                policy, before the next
                unattended run.
              

              
                **Source:**
                GitHub Changelog — Copilot models deprecated on October 19, 2026
              

            
            
              ### The policy is an endpoint.

              
                The same day, GitHub made
                GET and PATCH generally
                available for Copilot model
                policies at four levels:
                enterprise
                (`/enterprises/{enterprise}/copilot/model-policies`),
                enterprise teams
                (`/enterprises/{enterprise}/teams/{team_slug}/copilot/model-policies`),
                enterprise apps
                (`/enterprises/{enterprise}/copilot/apps/model-policies`),
                and organization
                (`/orgs/{org}/copilot/model-policies`).
                Enterprise policies can
                delegate model control to
                organizations, or to teams
                and apps, depending on the
                mode.
              

              
                That is the check that
                belongs next to a model
                pin. The selector you saw
                last week is not the
                policy the agent will hit
                tonight. Read the endpoint
                for the scope that
                actually governs the run.
                If the replacement model
                is not enabled there,
                retargeting the workflow
                only moves the failure
                from a retired name to a
                disabled one.
              

              
                **Source:**
                GitHub Changelog — Copilot model policy API for enterprise, orgs, and teams
              

            
            
              ### The updater learned the new manager.

              
                On 20 October 2026
                Dependabot added support
                for npm 12, pnpm 11 and 12,
                and Bun 1.4. The support is
                available to all github.com
                users, and GitHub says it
                will be included in GitHub
                Enterprise Server 3.24.
                Until that server release,
                a GHES instance on an older
                version does not get this
                line just because
                github.com did.
              

              
                Agents like to bump the
                package manager because the
                lockfile command failed on
                a syntax the old manager
                did not speak. That bump is
                not done when
                `npm install`
                succeeds. It is done when
                the updater that opens your
                dependency pull requests
                still knows how to read the
                ecosystem. As of today,
                those four versions are on
                the github.com list. Check
                the supported-ecosystems
                table before you treat a
                newer bump as safe, and do
                not assume GHES already
                matches.
              

              
                **Source:**
                GitHub Changelog — Dependabot now supports npm 12, pnpm 11 and 12, and Bun 1.4
