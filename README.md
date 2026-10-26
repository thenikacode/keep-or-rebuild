# keep-or-rebuild

[![License: MIT](https://img.shields.io/github/license/thenikacode/keep-or-rebuild)](LICENSE) [![Follow @thenikacode on X](https://img.shields.io/badge/follow-%40thenikacode-000000?logo=x&logoColor=white)](https://x.com/thenikacode)

**A Claude Code skill that tells you whether to keep paying for an app, switch to open source, or rebuild only the part you use.**

![Sped-up animation of a real keep-or-rebuild run in Claude Code, rendered from the run log. It opens on the verdict, REPLACE: Cal.com's free hosted plan covers the booking link, Google sync and reminders, and needing custom reminder text would flip it to KEEP. Then it replays the prompt (a made-up Calendly test case), the 2 web searches and the 7 pages it read, and ends on the verdict and what you'd lose.](docs/demo.gif)

It checks one product per run. You get a verdict, a cheaper plan if one covers what you use, the licenses quoted from each LICENSE file, what you'd lose, the first step and the sources it opened. In the demo run it read 7 pages in 71 seconds ([full output](#what-you-get)).

## Install

In Claude Code 2.1.275 or later:

```
/plugin install keep-or-rebuild --marketplace thenikacode/marketplace
```

<details>
<summary>Older Claude Code versions</summary>

```
/plugin marketplace add thenikacode/marketplace
/plugin install keep-or-rebuild@thenikacode
```

</details>

For other agents that read skills, use the skills CLI:

```
npx skills add thenikacode/keep-or-rebuild
```

So far we've tested it only in Claude Code. The agent you use needs web search and page fetching.

## Use

```
/keep-or-rebuild:keep-or-rebuild "my screenshot app, $8/month, macOS"
/keep-or-rebuild:keep-or-rebuild "my notes app, Plus plan, web"
```

If you installed it with `npx skills add`, in Claude Code the command is `/keep-or-rebuild`.

## What you get

A demo run on 25 Oct 2026 with keep-or-rebuild v0.1.2 and Claude Code 2.1.309. It's a real run with a made-up input: we don't use Calendly. The skill ran 2 web searches and read 7 pages in 71 seconds, and the output below is unedited.

The input:

```
/keep-or-rebuild:keep-or-rebuild Calendly Standard, $12/month, web. I use one 30-minute booking link, Google Calendar sync and email reminders. Only I use it. Just tell me.
```

The output (the skill wrote its last line under the code block):

```
VERDICT: REPLACE
Cal.com's free hosted plan covers your link, Google sync and reminders. It's a hosted plan, not the open-source code.

What you use:          one 30-min event type · Google Calendar sync · email reminders · single user
Best alternative:      Cal.diy (calcom/cal.com repo, self-hosted) · "MIT License" · last release v6.2.0, "1 March" (the page doesn't show the year) · https://github.com/calcom/cal.com
                       The README says "Teams, Organizations, Insights, Workflows, SSO/SAML, and other EE-only features have been removed". Without Workflows there are no reminder emails, so it doesn't cover what you use.
Also checked:          none. Cal.diy is the obvious open-source pick, and it's missing the feature you need.
Cheaper option:        Cal.com Free (hosted) · pricing: "Free forever", "Unlimited event types & calendars", "Email & SMS notifications" · help: "you can build workflows, but email steps use the default template text and can't be edited" · https://cal.com/pricing
                       (Calendly Free has "One event type" and "One calendar connection", but "Automate meeting reminders" is marked not included. Calendly's blog says "Workflows are available on all paid Calendly plans.")
Its running cost:      $0 (Cal.com Free, hosted)
Rebuild in an evening? partly: you could build a booking page that checks Google free/busy and creates events. Reliable reminder emails, OAuth upkeep, hosting and spam protection wouldn't be finished in one evening. Not worth it.
You'd lose:            custom reminder text (Free uses the default template) · Calendly's mobile app and its integrations · your current booking URL (you'll need to share the new link)
Your cost today:       $12/month. Calendly's pricing page now shows Standard at "$10 /seat/mo" under "Billed yearly". That's the only price I saw on the page.
Assumed:               the default reminder text is fine · you only send reminders to invitees · no other plans or people depend on the Calendly link. If you need custom reminder text, the answer flips to KEEP.
First step:            Sign up for Cal.com Free, connect Google Calendar, create the 30-minute event, turn on the default reminder workflow, and book a test meeting with yourself before you cancel Calendly.
Sources:               https://calendly.com/pricing · https://calendly.com/blog/workflows · https://cal.com/pricing · https://cal.com/help/workflows/workflowsoverview · https://raw.githubusercontent.com/calcom/cal.com/HEAD/LICENSE · https://github.com/calcom/cal.com · https://github.com/calcom/cal.com/releases/latest
```

Cal.com's pricing page doesn't name Google Calendar for the Free plan. It only says "Unlimited event types & calendars". You'll see whether sync works during the test booking.

We checked 24 statements in this output against the live pages on 25 Oct 2026. 16 hold, 5 are wrong, and 3 are partly right, unverified or a judgment call. The run opened all 7 pages it lists under Sources. The 5 that need a correction:

- REPLACE here means "move to Cal.com's hosted Free plan". That plan is not open source: Cal.com went closed source in April 2026. The skill defines REPLACE as a move to open source, so here it stretches its own rule. The open-source option it checked, Cal.diy, has no reminder emails.
- Release v6.2.0 (1 Mar 2026) predates Cal.diy and ships under AGPLv3, with a commercial license for its ee folders. The main branch has been MIT since 15 Apr 2026, and Cal.diy hasn't published a release since. The skill should have listed the two licenses separately.
- The output's last line, the one under its code block, is wrong. Cal.com's [pricing page](https://cal.com/pricing) lists "Google Calendar, Outlook, Apple Calendar, Zoho Calendar + more" in its feature table, with a check for the Free plan. The run read the page as text and missed the check marks.
- Under "You'd lose": Cal.com Free has its own mobile app and lists 100+ integrations. You do lose one thing the output misses: Calendly Standard can remove Calendly branding, and Cal.com Free can't remove Cal.com branding.
- The first step packs five actions into one line. The skill asks for one.

With "Billed monthly" selected, Calendly shows Standard at $12, the same as your price. The [CHANGELOG](CHANGELOG.md) lists all five points as known issues.

<details>
<summary>Earlier demo runs</summary>

v0.1.0 (24 Oct 2026) and v0.1.1 (25 Oct 2026) ran the same input and both said KEEP. Critic agents checked both runs against the live pages and found that Cal.com's hosted Free plan covers all three features. In v0.1.2 we added the Cheaper option line to catch that case.

</details>

## Guardrails

The skill's instructions include these rules:

- Respect the product's terms of service and its license.
- Don't crack it, bypass license checks, share accounts or scrape paid content.
- Don't decompile it, intercept its private APIs or copy its code, assets, branding or look. A rebuild is a new program that does the same job.
- Get data out only through the official export or the documented API.
- Read each license from its LICENSE file: for mixed licenses, badges and the API's license field can show NOASSERTION or only the root license. Check enterprise folders and hosted-plan terms too.
- Keep the license and notices of any reused open-source code. A rebuild that reuses GPL or AGPL code must use the same license when someone distributes it (for AGPL, also when others use it over a network).
- Use only the user's own numbers and never estimate savings.

## Why this exists

With a coding agent you can rebuild the part of an app you use and cancel the subscription, but you may lose a feature you rely on. If you switch to an open-source alternative instead, its maintainers may keep some features in an enterprise folder or on a paid hosted plan. keep-or-rebuild looks for both problems, with sources, before you spend an evening on a rebuild. Nika Code built it with Claude Code agents, and critic agents reviewed it before release.

## FAQ

- **Does it need API keys?** No. It uses web search and page fetching (WebSearch and WebFetch in Claude Code).
- **How do I get updates?** Auto-update is off by default for third-party marketplaces. Run `/plugin`, open **Marketplaces**, select `thenikacode` and choose **Enable auto-update**.
- **Got a wrong verdict?** [Open an issue](https://github.com/thenikacode/keep-or-rebuild/issues/new?template=wrong-verdict.md) with the output and the page that shows it's wrong.

## License

MIT. Made by [Nika Code](https://github.com/thenikacode) with Claude Code. Contributions are welcome: see [CONTRIBUTING.md](CONTRIBUTING.md).

Not affiliated with Anthropic or with any product named in the demo run. Those products appear here only to describe it.
