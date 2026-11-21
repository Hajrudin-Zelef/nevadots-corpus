---
id: collect-261121-veille-ia/google-ecosystem/google-s-stitch-loop-sends-the-redesign-back-as-a-pull-request
title: "Google's Stitch Loop sends the redesign back as a pull request"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-18"]
keywords: ["loop", "google", "stitch", "pull", "request", "design", "product", "options"]
source: https://artdirectiondaily.com/issues/2026-11-18-stitch-loop-redesign-pull-request.html
source_anchor: ""
source_lines: [1, 192]
section: "Google_Ecosystem"
---
**No. 186**Wednesday, 18 November 2026

6 minute read

Google Labs, waitlist open today

# Google's Stitch Loop sends the redesign back as a pull request

A new Labs product agent reads your repo and live metrics, proposes fixes with visual design options, and builds the one you pick. Nothing merges until someone reviews it.

- Hours
- Runs around the clock until the goal is met
- Waitlist
- Open today, all users, worldwide
- First in line
- Eligible Google AI Ultra users
- Price
- Not announced

Received No. 1

Today's art direction

## The Alteration Shop

A neighborhood tailor's landing page, set on marigold felt with a navy cloth band and red thread.

The page borrows the working surfaces of an alterations counter. A cutting-table photograph is the hero ground, a red seal stands in for the masthead, and each section starts with a workroom stamp the way a ticket gets stamped as a garment moves from measured to ready.

The type pairs a compact neo-grotesk, set heavy for signage and labels, with a bookish old-style text serif for reading. The ground is a faint gingham check, the cloth a tailor actually keeps on the shelf.

- Cutting-table hero
- Seal masthead
- Gingham ground
- Workroom stamps
- Pinked swatches
- Services price list
- Sign-off gutter
- Work order slip

Measured No. 2

## Stitch Loop keeps working after you ship

Google Labs today introduced Stitch Loop, a product agent that connects to a GitHub repo and to live product context (logs, user behavior, feedback) through more than 180 integrations, custom MCP servers, or local CLI uploads. Google calls it an experiment and says it keeps working on the product until your goals are met.

Each loop runs four steps. It surfaces suggestions grounded in evidence, offers one or more solutions with tradeoffs and, where they apply, visual design options, then writes the chosen fix in an isolated cloud sandbox and opens a pull request with screenshots or video. After the goal is met it keeps watching in case the metric slips.

Nothing merges until you review the PR.

Justin Toupin, Google Labs, in the launch post
For designers the phrase to watch is *visual design options*. A look change arrives as alternatives attached to a proposal, and the review happens in the pull request, so the PR description and its screenshots become the place where a site's design gets argued over.

### Services on the board

Three loops ship by default; you can add your own.

| Stitch Loop default and custom loops |  |  | 
|---|---|---|
| Security | Scans the codebase for vulnerabilities, flags each with evidence and opens a PR with a proposed fix, drawing on Gemini 4 Carbon and DeepMind's CodeMender. | Included | 
|---|---|---|
| Latency | Named in the launch post as a default loop. No further detail yet. | Included | 
| Accessibility | Also a default. The post gives no further detail. | Included | 
| Your own loop | Any goal you can point it at. Google's example is improving checkout conversion. | Yours to set | 

### How a fitting works

1. **01****Suggestions**Opportunities grounded in evidence from your product.
2. **02****Solutions**One or more options with tradeoffs and scoping, plus visual design options where they apply.
3. You pick one
4. **03****Build**Code in an isolated cloud sandbox, opened as a GitHub pull request with screenshots or video.
5. You review
6. **04****Reflect**Watches live data after the merge, and keeps watching once the goal is met.

The two stamped gutters are the only places the loop waits for a person.

Pinned No. 3

## Kalka draws on a canvas made of real HTML

Kalka is an open-source design editor where every layer is an HTML element, a heading or a button, carrying its classes and CSS as written. Breakpoints export as media queries, and a document is a folder of HTML pages with a stylesheet, tokens and assets, or a single file, so it versions in Git like any other code.

It lists components, variants, variables, auto layout, live collaboration and Figma file import, and exports standalone HTML, Tailwind JSX, Storybook stories and images. A commit pushed today adds running named agents in parallel from an @ mention. It is an early beta from a small repo, so treat it as one to watch rather than one to adopt.

- License
- MIT
- First beta
- v0.2.0-beta.1, 16 November
- Canvas
- HTML elements with classes and CSS kept as written
- Exports
- HTML, Tailwind JSX, Storybook, PNG, JPG, SVG, PDF

Sewn No. 4

## Alloy CSS opens a dialog with no script at all

Alloy CSS is one MIT-licensed stylesheet for server-rendered HTML that builds dialogs and menus from platform features instead of a JavaScript bundle. A dialog is a native dialog element opened by a button carrying the new commandfor and command attributes; menus are popovers, and the README names :has(), color-mix() and light-dark() as the rest of the kit. Version 1.0.8 shipped on November 16.

For agent-built pages that matters more than it looks. Markup a model writes with no build step still produces a working, keyboard-reachable dialog. Closing on an outside click uses closedby, which the README says needs Safari 27.3.

### Also this week: Firefox 160 reads WebKit placeholder styles

Firefox 160, released November 10, treats the WebKit placeholder pseudo-element as an alias of the standard one, so placeholder styling written for Safari and Chrome now renders the same way in Firefox.

The pattern

```
<button commandfor="saved"
        command="show-modal">
  Save
</button>
<dialog id="saved" closedby="any">
  <p>Saved.</p>
  <button commandfor="saved"
          command="close">
    Close
  </button>
</dialog>
```
Fitted No. 5

## Where the loop reports back

The launch post names three places a loop's work turns up besides its own screens.

- ### In SlackMention @stitch for a daily digest of what each loop found, or to start a new loop from a thread.
- ### In other agentsCoding agents can pull Loop's context through the Stitch CLI and MCP, and Google Antigravity can use it as a service layer.
- ### In the pull requestScreenshots and video ride along when relevant, so a design reviewer can judge the change before reading a line of the diff.

Pressed No. 6

Borrow this pattern

## Put the customer's sign-off in the gutter

In any service process, a fitting or a quote or a redesign loop, the provider's steps and the customer's decisions are different kinds of thing. Number the work. Set each approval as a stamp in the gutter between steps, in the one accent color, with a verb the customer owns.

1. Steps get numbers; approvals never do.
2. The accent color is reserved for approvals, so the eye finds them first.
3. Keep the stamp in DOM order between its two steps, so a screen reader hears it where it happens and mobile stacks it in place.
4. Use a real verb: "You pick one" beats "Approval."

CSS

```
.steps{
  display:grid;
  grid-template-columns:
    1fr 150px 1fr 150px 1fr;
  gap:0 12px;
}
.gate span{
  border:3px double var(--red);
  color:var(--red);
  background:var(--cream);
  transform:rotate(-4deg);
}
@media (max-width:980px){
  .steps{grid-template-columns:1fr}
}
```
Ready No. 7

## Prompt Lab

Build a single landing page for a neighborhood tailor and alterations shop. Treat it as a real local service business page a client would pay for: booking, services with prices, hours, and a clear process.
Ground and palette: a vivid marigold page ground (#D99B14) with a faint gingham check made of two crossing 50 percent stripes of navy at about 5 percent opacity on a 44px pitch. Navy wool (#152036) for text and dark bands, ticket cream (#F6EEDC) for cards, thread red (#B8261B) as the single accent, shear brass (#8A5F12) only for small details. No gradients, no glow, no purple.
Type: Host Grotesk at 800 for the headline, section titles and labels; Crimson Pro at 21px with 1.66 line height for body copy; a mono only inside code. Body text never under 18px; keep lines near 65 characters.
Above the fold: a full-bleed overhead photograph of a cutting table as the hero ground, with flat marigold felt on the left third. Over that calm area, a round red seal (cream rim text with the shop name, a number in the middle) acting as the masthead, a solid navy kicker label, a short heavy headline, a two-sentence deck, a red booking button with a hard 3px bottom shadow and a plain text link. A marigold scrim on the left keeps the copy at WCAG AA. Under the hero, a navy hours bar of four labelled cells: hours, walk-ins, rush service, price from.
Sections: each starts with a workroom stamp, a cream label with a 3px double red border, uppercase, rotated about -2.5 degrees, reading Received, Measured, Pinned, Sewn, Fitted, Pressed, Ready. Below it the section title.
Components: a cream services price list as a real table (service, tag such as Standard or Rush, plain description with a price); a full-width navy process band of four numbered steps where the customer's decisions (Choose your fit, Approve the fitting) sit as red stamps in the gutters between steps, never as numbered steps; a palette card with pinked-edge cloth swatches made with a zigzag clip-path; a portrait photo of a pinned jacket on a hanger with a one-line caption; a cream work order card for the booking form with labelled header cells.
Guardrails: real hover and focus states on links and buttons (150ms ease), one restrained stamp-press animation on the seal behind a prefers-reduced-motion guard, cards at 12px radius, no pill shapes except true status dots, no left accent stripes, no fake search fields or toggles, no legible fake text inside photos, every small label checked for AA contrast against its actual background. On mobile the hero photo sits on top at 4:3 with the seal overlapping its lower edge, the process becomes a vertical list with each stamp kept between its two steps, and the price table rows stack.

Works in Claude, v0, Lovable, Bolt, Cursor, Stitch and Beaver Builder AI.

Collected No. 8

## Field note

A tailor never cuts cloth before the customer has stood in the fitting room, and Stitch Loop writes that rule into its pipeline. What nobody knows yet is how many design options a team can review before the PR queue becomes the bottleneck.

## Sources

1. Introducing Stitch Loop: A new way to build and evolve products, Google, 18 November 2026
2. Stitch Loop waitlist, Google Labs
3. Introducing CodeMender, an AI agent for code security, Google DeepMind
4. Kalka, GitHub repository
5. Kalka releases, GitHub
6. Alloy CSS, GitHub repository
7. Alloy CSS documentation, alloyhtml.com
8. Firefox 160 for developers, MDN
