# Google Ads Skills

10 Google Ads skills for Claude Code and other AI agents.

## Install

```bash
npx skills add eliasmalm/google-ads-skills
```

## Skills Included

| Skill | Description |
|-------|-------------|
| `google-ads-search` | Search campaigns, RSAs, Quality Score, Ad Rank, extensions |
| `google-ads-pmax` | Performance Max campaigns, asset groups, audience signals |
| `google-ads-shopping` | Merchant Center, feed optimization, Shopping campaigns |
| `google-ads-display` | GDN remarketing and prospecting, RDAs, placements |
| `google-ads-video` | YouTube ads, skippable/bumper/in-feed formats, sequencing |
| `google-ads-keywords` | Keyword research, match types, negatives, search term analysis |
| `google-ads-bidding` | Smart Bidding, tCPA, tROAS, bid adjustments, portfolio strategies |
| `google-ads-audiences` | RLSA, Customer Match, in-market, custom intent, lookalikes |
| `google-ads-conversion-tracking` | Google Tag, GTM, enhanced conversions, offline import |
| `google-ads-scripts` | JavaScript automation, budget monitoring, keyword pausing, reporting |

## Skills Are Interconnected

These skills reference each other — e.g., `google-ads-conversion-tracking` is a prerequisite for Smart Bidding in `google-ads-bidding`, and audience lists built in `google-ads-audiences` feed into `google-ads-pmax` signals.

## Compatible With

- Claude Code
- Cursor
- Windsurf
- Codex
- Gemini CLI
