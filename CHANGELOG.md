# Changelog

All notable changes to keep-or-rebuild are listed here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.2] - 2026-10-28

First public release.

### Added

- A "Cheaper option" line in the output. The skill now checks the product's own free or cheaper plan and the free plan of the best-known hosted competitor, quotes the line that shows each feature, and makes a plan that covers everything the first step.

### Changed

- When the latest release is older than a license change, the skill gives the license of the release and of the main branch separately.
- The first step is one action, and it doesn't advise changing your billing period.
- Sources list only pages the skill opened, not search results.

### Known issues

- When a cheaper hosted plan covers everything, the skill can still label it REPLACE, which it defines as a move to open source.
- It can put a release's license next to the main branch's license without separating them, when the release is older than a license change.
- It reads pricing tables as text, so it can miss check marks drawn as icons.
- The first step can pack several actions into one line.
- Under "You'd lose" it can list features the cheaper plan also has, and miss one it lacks (in the demo: a mobile app listed, branding removal missed).
- So far we've tested it only in Claude Code.

## [0.1.1] - 2026-10-25

Not released publicly.

### Changed

- The license guardrails in the skill's instructions now say that for mixed licenses the badge and the API's license field can show NOASSERTION or only the root license. They also say that a rebuild reusing GPL or AGPL code must use the same license when someone distributes it (for AGPL, also when others use it over a network).

### Fixed

- The skill's instructions now tell it to open the product's help or docs page on plan limits when the pricing page doesn't say whether a plan has a feature you use, and to list that page under Sources.
- When a pricing page has a monthly/yearly switch, the instructions now ask it to compare your price with the one for the same billing period and say which price it saw.

## [0.1.0] - 2026-10-24

Not released publicly. This is the version that produced the first demo run.

### Added

- The `keep-or-rebuild` skill for Claude Code. You name one product, its plan and platform, and it answers KEEP, REPLACE or REBUILD.
- Under the verdict: the features you use, the best open-source alternative and up to 3 others it checked, their licenses quoted from the LICENSE file, what you'd lose, your cost (with today's public price if it differs), its assumptions, a first step and the pages it opened.
- Guardrails in the skill's instructions: respect the terms of service and the license, no cracking or decompiling, data out only through the official export or the documented API, and no savings estimates.
- Install with one command from the `thenikacode` marketplace in Claude Code 2.1.275 or later (two commands on older versions), or with `npx skills add thenikacode/keep-or-rebuild`.
- A demo run with a made-up Calendly input, and its unedited output, in the README.

### Known issues

- Two lines of the demo output (24 Oct 2026) don't match the live pages. Calendly's pricing page shows $12 for Standard with "Billed monthly" selected, and Cal.com's help page on workflows says you can build workflows on the Free plan. For plan limits the skill read only the pricing pages. v0.1.1 changes the skill so it also opens help pages on plan limits and compares your price with the same billing period.
- So far we've tested it only in Claude Code.

[Unreleased]: https://github.com/thenikacode/keep-or-rebuild/compare/v0.1.2...HEAD
[0.1.2]: https://github.com/thenikacode/keep-or-rebuild/releases/tag/v0.1.2
