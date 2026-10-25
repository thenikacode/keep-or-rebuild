---
name: keep-or-rebuild
description: Use when someone asks whether to keep paying for a piece of software, switch to an open-source alternative, or rebuild the part they use with Claude Code. Returns KEEP, REPLACE or REBUILD with evidence, the licenses read from the source, what they would lose, and the first step.
argument-hint: "<product, plan and platform, e.g. 'my notes app, Plus plan, macOS'>"
allowed-tools: WebSearch WebFetch
---

# Keep or rebuild

Product: $ARGUMENTS. If no product is named, ask for one.

Rebuilding the software you pay for is popular right now. Often it's the wrong move. This skill decides, for one product at a time: **keep it, replace it with something open source, or rebuild only the part you use.**

## Ask first (one round, at most 3 questions, only what's missing)

- Which product and plan, and on which platform (macOS, Windows, Linux, web, phone)?
- The 3–5 features they really use.
- Does anyone else depend on it, and can the data be exported?

If a product is named, you may search first and give a provisional verdict, then ask only the questions that would change it. If they don't answer or say "just tell me", go ahead: list each assumption under the verdict and say which answer would flip it.

## Then check, with sources

0. **Cheaper ways to keep the job done.** Check the product's own free or cheaper plan, and the free plan of the best-known hosted competitor. For each one, quote the line from its pricing or help page that says whether it has each feature they use. If one covers everything, put it on the Cheaper option line and make it the First step. These plans are not open source: keep them out of Best alternative.
1. **Open-source alternatives.** Search the web. For each candidate, record:
   - the license of the parts they would actually use, read from the LICENSE (or COPYING, LICENSE.md) file itself, for example `https://raw.githubusercontent.com/<owner>/<repo>/HEAD/LICENSE`. Don't trust the badge or the API's license field: for mixed licenses they can show NOASSERTION, or only the root license. Quote the line that names the license. Check for `ee/`, `enterprise/` or `pro/` folders, dual or commercial licensing, and the terms of the official hosted plan: features behind a paid license are not open source. "Source-available" is not open source. If the latest release is older than a license change, give the license of the release and the license of the main branch separately;
   - the date of the last release or commit, and how active the project looks;
   - how it runs: hosted by someone, self-hosted, desktop;
   - what it costs to run, from its pricing page, with the link;
   - how the data gets in: official import, documented API, or by hand.
2. **Rebuild size.** Only for the features they use. Rebuild only if it's small enough for one evening with Claude Code, and say exactly what that version would not have.
3. **What they'd lose.** Sync, mobile apps, security updates, backups, support, compliance, integrations, sharing with others.
4. **Their real cost.** Use only the numbers they gave you, and never estimate savings. If the public price today is different from theirs, show it next to theirs with the link, and add "may lose an older price" under You'd lose.
   If the pricing page has a monthly/yearly switch, compare their number with the price for the same billing period: monthly, unless they said they pay yearly. Show the other price only next to it, and say which is which. If the page text you fetched shows only one of the two, say which one you saw. Don't write that the page doesn't show the other.
5. **Plan limits.** When the verdict depends on whether a plan has a feature they use (a free or cheaper plan, or an alternative's hosted plan), check the pricing page first. If it doesn't answer, or answers only with a footnote, open the product's help or docs page on plan limits (search `<product> <feature> plan limits` or `<product> help <feature>`) and quote the line that answers. Write "doesn't say" only after you've checked both, and list both pages under Sources.

## Legal guardrails (always)

- Respect the product's terms of service as well as its license and the law.
- Never help to crack, bypass a license check, share accounts or scrape paid content.
- Never decompile it, intercept or replay its private API or network traffic, or automate its web app to rebuild it.
- Get data out only through the official export or the documented API.
- Never copy proprietary code, assets, fonts, icons, names or branding, and don't recreate its look from screenshots: your own layout, your own names, your own icons. A rebuild is a new program that does the same job.
- If the rebuild reuses open-source code, keep its license and notices. If it reuses GPL or AGPL code and the user distributes it (or, for AGPL, lets others use it over a network), it must use the same license: GPL for GPL code, AGPL for AGPL code.
- Reverse engineering only where the law, the license and the terms allow it. When unsure, say so and recommend REPLACE or KEEP.

## Verdict

```
VERDICT: KEEP | REPLACE | REBUILD
<one line on why>

What you use:          <the 3–5 features>
Best alternative:      <name> · <license, quoted from its LICENSE file> · last release <date> · <link>
Also checked:          <name> · <license> · <date> · <why not>   (one line each, at most 3)
Cheaper option:        <a free or cheaper plan that covers what they use, their product's or a competitor's · the line from its pricing or help page that shows it · link>, or "none found"
Its running cost:      <from its pricing page, with link; or "free, self-hosted"; or "unknown">
Rebuild in an evening? <yes / partly / no>: <what it would and wouldn't do>
You'd lose:            <list>
Your cost today:       <their number, or "not given">
Assumed:               <assumptions, or "none">
First step:            <one concrete action>
Sources:               <pages you opened with WebFetch>
```

- **KEEP** when the product does more than they'd want to maintain, the data is hard to move, or others depend on it.
- **REPLACE** when a maintained open-source project covers what they use and the license fits.
- **REBUILD** when what they use is small and personal, and nothing maintained fits.
- **Tie-break:** KEEP if others depend on it or the data can't be exported cleanly; otherwise REPLACE before REBUILD.
- **First step:** one action. Don't advise changing their billing period: show the other price only next to theirs.
- **Sources:** list only pages you opened with WebFetch. A search result you didn't open is not a source.

Be blunt and short. One product per run.
