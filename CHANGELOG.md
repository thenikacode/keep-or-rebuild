# Changelog

All notable changes to keep-or-rebuild are listed here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.0] - 2026-10-28

### Added

- The `keep-or-rebuild` skill for Claude Code. You name one product, its plan and platform, and it answers KEEP, REPLACE or REBUILD.
- Under the verdict: the features you use, the best open-source alternative and up to 3 others it checked, their licenses quoted from the LICENSE file, what you'd lose, your cost (with today's public price if it differs), its assumptions, a first step and the pages it opened.
- Guardrails in the skill's instructions: respect the terms of service and the license, no cracking or decompiling, data out only through the official export or the documented API, and no savings estimates.
- Install with one command from the `thenikacode` marketplace in Claude Code 2.1.275 or later (two commands on older versions), or with `npx skills add thenikacode/keep-or-rebuild`.
- A demo run with a made-up Calendly input, and its unedited output, in the README.

### Known issues

- Two lines of the demo output (24 Oct 2026) don't match the live pages. Calendly's pricing page shows $12 for Standard with "Billed monthly" selected, and Cal.com's help page on workflows says you can build workflows on the Free plan. For plan limits the skill read only the pricing pages. We plan to change the skill in v0.1.1 so it also opens help pages on plan limits and compares your price with the same billing period.
- So far we've tested it only in Claude Code.

[Unreleased]: https://github.com/thenikacode/keep-or-rebuild/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/thenikacode/keep-or-rebuild/releases/tag/v0.1.0
