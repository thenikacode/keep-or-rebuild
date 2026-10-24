# keep-or-rebuild

[![License: MIT](https://img.shields.io/github/license/thenikacode/keep-or-rebuild)](LICENSE) [![Follow @thenikacode on X](https://img.shields.io/badge/follow-%40thenikacode-000000?logo=x&logoColor=white)](https://x.com/thenikacode)

**A Claude Code skill that tells you whether to keep paying for an app, switch to open source, or rebuild only the part you use.**

![Sped-up animation of a real keep-or-rebuild run in Claude Code, rendered from the run log. It opens on the verdict, KEEP, and the assumption that would flip that verdict. Then it replays the prompt (a made-up Calendly test case) and the 12 page fetches, and ends on the verdict and what you'd lose.](docs/demo.gif)

It checks one product per run. You get a verdict, the licenses quoted from each LICENSE file, what you'd lose, the first step and the sources it opened. In the demo run it read 9 pages in 80 seconds ([full output](#what-you-get)).

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

A demo run on 24 Oct 2026 with Claude Code 2.1.309. It's a real run with a made-up input: we don't use Calendly. The skill read 9 pages (12 fetches) in 80 seconds, and the output below is unedited.

The input:

```
/keep-or-rebuild:keep-or-rebuild Calendly Standard, $12/month, web. I use one 30-minute booking link, Google Calendar sync and email reminders. Only I use it. Just tell me.
```

The output:

Done checking. Here's the verdict.

```
VERDICT: KEEP
The open-source self-hosted option doesn't do your reminder emails, and Calendly's free plan doesn't either. Moving means running a server and still losing a feature you use.

What you use:          one 30-min booking link · Google Calendar sync · email reminders · just you
Best alternative:      Cal.diy (the open-source fork of Cal.com) · "MIT License" · last release v6.2.0 on 2026-03-01, last commit 2026-10-19 (busy project) · https://github.com/calcom/cal.com
Also checked:          Easy!Appointments · "GNU GENERAL PUBLIC LICENSE Version 3" · 1.6.1 on 2026-10-19 · built for service businesses, and I couldn't confirm it sends reminders before the meeting
                       Cal.com hosted Free · not open source (it's their hosted service) · their page lists "Email & SMS notifications" but doesn't say whether reminder workflows are included on Free
                       Calendly Free · 1 event type + 1 calendar would be enough, but "Automate meeting reminders" isn't on Free
Its running cost:      Cal.diy: free, but you host it yourself (server, database, email sending). It has no hosted version.
Rebuild in an evening? partly: you could build a booking page that checks Google free/busy and sends a confirmation in an evening. Google sign-in (OAuth), hosting, reliable email delivery and timed reminders would take longer.
You'd lose:            reminders (Cal.diy's README says "Workflows… have been removed"), hosted uptime, security updates, support, mobile apps, and possibly your current price
Your cost today:       $12/month (the public Standard price now shows $10/seat with a "Save 17%" yearly toggle, and the page doesn't show the monthly-billed price: https://calendly.com/pricing)
Assumed:               you don't want to run a server; "email reminders" means automatic emails before the meeting, not just the booking confirmation
                       → If you only need booking confirmations, Calendly Free or Cal.com Free probably covers you: switch.
                       → If Cal.com's hosted Free plan includes reminder workflows, switch to it.
First step:            Make a free Cal.com account, set up one 30-min event, connect Google Calendar and check whether reminder workflows are available on Free. If they are, switch. If not, stay on Calendly.
Sources:               https://calendly.com/pricing
                       https://cal.com/pricing
                       https://github.com/calcom/cal.com
                       https://raw.githubusercontent.com/calcom/cal.com/HEAD/LICENSE
                       https://api.github.com/repos/calcom/cal.com/releases/latest
                       https://api.github.com/repos/calcom/cal.com/commits?per_page=3
                       https://raw.githubusercontent.com/alextselegidis/easyappointments/HEAD/LICENSE
                       https://api.github.com/repos/alextselegidis/easyappointments/releases/latest
```

After the run, on 24 Oct 2026, a critic agent found two lines that don't match the live pages:

- `Your cost today`: on Calendly's [pricing page](https://calendly.com/pricing), Standard is $12 with "Billed monthly" selected.
- `Also checked`, Cal.com hosted Free: Cal.com's [help page on workflows](https://cal.com/help/workflows/workflowsoverview#plan-limitations) says that on the Free plan you can build workflows, with the default email text.

By the output's own "Assumed" line, the user in this made-up case should switch to Cal.com Free. The critic agent read Cal.com's docs and didn't open an account to test it.

For plan limits, the skill read only the pricing pages. We plan to change it in v0.1.1 so it also opens help pages on plan limits. When a pricing page has a monthly/yearly switch, it will compare your price with the same billing period and say which price it saw. The [CHANGELOG](CHANGELOG.md) lists this as a known issue.

## Guardrails

The skill's instructions include these rules:

- Respect the product's terms of service and its license.
- Don't crack it, bypass license checks, share accounts or scrape paid content.
- Don't decompile it, intercept its private APIs or copy its code, assets, branding or look. A rebuild is a new program that does the same job.
- Get data out only through the official export or the documented API.
- Read each license from its LICENSE file: badges and the API's license field can show NOASSERTION for mixed licenses. Check enterprise folders and hosted-plan terms too.
- Keep the license and notices of any reused open-source code, and treat reused GPL or AGPL code as making the rebuild GPL or AGPL too.
- Use only the user's own numbers and never estimate savings.

## Why this exists

With a coding agent you can rebuild the part of an app you use and cancel the subscription, but you may lose a feature you rely on. If you switch to an open-source alternative instead, its maintainers may keep some features in an enterprise folder or on a paid hosted plan. keep-or-rebuild looks for both problems, with sources, before you spend an evening on a rebuild. Nika Code's agents wrote the skill, and critic agents reviewed it before release.

## FAQ

- **Does it need API keys?** No. It uses web search and page fetching (WebSearch and WebFetch in Claude Code).
- **How do I get updates?** Auto-update is off by default for third-party marketplaces. Run `/plugin`, open **Marketplaces**, select `thenikacode` and choose **Enable auto-update**.
- **Got a wrong verdict?** [Open an issue](https://github.com/thenikacode/keep-or-rebuild/issues/new?template=wrong-verdict.md) with the output and the page that shows it's wrong.

## License

MIT. Made by [Nika Code](https://github.com/thenikacode), a character: one human and a crew of AI agents. Contributions are welcome: see [CONTRIBUTING.md](CONTRIBUTING.md).

Not affiliated with Anthropic or with any product named in the demo run. Those products appear here only to describe it.
