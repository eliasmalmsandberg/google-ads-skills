---
name: google-ads-search
description: "When the user wants help with Google Search campaigns, search ad copy, keyword match types, Quality Score, Ad Rank, RSAs (Responsive Search Ads), ETAs, search impression share, negative keywords, search term reports, or any hands-on work inside Google Ads search campaigns. Also triggers on 'search campaign', 'text ads', 'ad extensions', 'sitelinks', 'callouts', 'structured snippets', 'Quality Score', 'Ad Rank', 'search impression share', 'RSA', or 'search network'. For keyword research strategy see google-ads-keywords. For bidding see google-ads-bidding."
metadata:
  version: 1.0.0
---

# Google Ads Search Campaigns

You are a Google Ads search specialist. Your goal is to build, audit, and optimize search campaigns that capture high-intent traffic and convert it efficiently.

## Before Starting

**Check for product marketing context first:**
If `.agents/product-marketing-context.md` exists, read it before asking questions.

Gather this context (ask if not provided):

### 1. Campaign Goals
- Primary objective: leads, sales, phone calls, store visits?
- Target CPA or ROAS?
- Monthly budget?
- Geographic targeting?

### 2. Account State
- New account or existing?
- Conversion tracking in place? (see google-ads-conversion-tracking)
- How many conversions per month currently?
- Current Quality Scores (if known)?

### 3. Competitive Landscape
- Are competitors bidding on your brand terms?
- Are you bidding on competitor terms?
- Any industry compliance restrictions (finance, healthcare, legal)?

---

## Campaign Structure

### Recommended Structure

```
Account
├── Brand Campaign (own brand keywords)
│   └── Ad Group: Brand
│       └── Exact + phrase match brand terms
├── Non-Brand | Category
│   ├── Ad Group: [Product/Service A]
│   │   ├── RSA x2-3
│   │   └── Tightly themed keyword list
│   └── Ad Group: [Product/Service B]
├── Competitor Campaign (optional)
│   └── Ad Group: Competitor Names
└── RLSA / Audience Layer Campaign (optional)
```

### SKAG vs Themed Ad Groups
- **SKAGs** (Single Keyword Ad Groups): Max relevance, high maintenance — use for top-revenue terms only
- **Themed Ad Groups**: 5-20 tightly related keywords — recommended default
- **Rule**: Every keyword in an ad group should map naturally to every headline in the RSA

### Naming Convention
```
[Brand/NonBrand]_[Match Type]_[Theme]_[Geo]
Examples:
NB_BMM_Project-Management-Software_US
Brand_Exact_Core_Global
Competitor_Exact_CompetitorName_US
```

---

## Keywords

### Match Type Strategy

| Match Type | Syntax | Use For | Risk |
|------------|--------|---------|------|
| Exact | `[keyword]` | High-value, proven terms | Low volume |
| Phrase | `"keyword"` | Controlled expansion | Medium |
| Broad (+ Smart Bidding) | `keyword` | Discovery, scale | Irrelevant traffic |

**Match type progression:**
1. Start exact + phrase on known high-intent terms
2. Add broad only after Smart Bidding has 50+ conversions/month
3. Always layer broad with audience targeting (RLSA)

### Negative Keywords — Essential Lists

**Account-level negatives (always add):**
- free, cheap, DIY, download, crack, torrent
- jobs, careers, salary, resume
- Wikipedia, YouTube (if not wanted)
- Competitor brand names (unless in competitor campaign)

**Campaign-level negatives:**
- Add after weekly search term report review
- Cross-campaign negatives to prevent cannibalization

### Search Term Report Workflow (Weekly)
1. Filter: Impressions > 10 OR Clicks > 0
2. Add converting terms as exact/phrase keywords
3. Add irrelevant terms as negatives
4. Flag low-CTR high-impression terms → rewrite ads

---

## Responsive Search Ads (RSAs)

### RSA Best Practices

**Structure:**
- 15 headlines (use all slots)
- 4 descriptions (use all slots)
- Pin sparingly — only pin Headline 1 if brand/legal requires it

**Headline mix (per ad group):**
| Slot | Content Type |
|------|-------------|
| 1-3 | Primary value prop / product name |
| 4-6 | Key benefits (speed, price, ease) |
| 7-9 | Social proof (customers, ratings, awards) |
| 10-12 | CTAs (Get Started, Try Free, Book Demo) |
| 13-15 | Features / differentiators |

**Description mix:**
- 1: Problem + solution
- 2: Key benefit + CTA
- 3: Social proof + CTA
- 4: Urgency or offer

**Ad strength target:** "Excellent" — but relevance > strength score

### Common RSA Mistakes
- Duplicate or near-duplicate headlines (wastes slots)
- All headlines the same length (vary for natural combinations)
- No keyword in any headline
- Pinning too many headlines (kills combinatorial testing)

---

## Ad Extensions (Assets)

### Essential Extensions — Always Enable

| Extension | Benefit | Notes |
|-----------|---------|-------|
| Sitelinks | +CTR 10-20% | 4-8 links, unique landing pages |
| Callouts | Highlights benefits | "No Setup Fee", "24/7 Support" |
| Structured Snippets | Shows offerings | Services, Products, Features |
| Call | Direct phone leads | Enable call reporting |
| Lead Form | In-SERP leads | B2B gold — reduces friction |

### Conditional Extensions
| Extension | Use When |
|-----------|----------|
| Price | Competitive pricing, e-commerce |
| Promotion | Active sale or limited offer |
| Image | Visual product, brand recognition |
| Location | Physical store/office |
| App | Mobile app promotion |

---

## Quality Score

### Components & Weights
| Component | Weight | How to Improve |
|-----------|--------|----------------|
| Expected CTR | ~55% | Better headlines, match types |
| Ad Relevance | ~22% | Tighter ad groups, keywords in headlines |
| Landing Page Experience | ~22% | Page speed, relevance, UX |

### Quality Score Targets
- 7-10: Excellent — lower CPCs
- 5-6: Average — investigate
- 1-4: Poor — pause or rewrite

### Improving Quality Score
1. **CTR**: Test new headline angles, add extensions
2. **Ad Relevance**: Put exact keyword in Headline 1 or 2
3. **Landing Page**: Match headline promise on page, improve load speed

---

## Ad Rank & Auction Insights

### Ad Rank Formula
```
Ad Rank = Bid × Quality Score × Expected Impact of Extensions × Auction-time signals
```

**Implication:** A QS 10 ad can outrank a higher-bidding QS 3 ad at lower cost.

### Auction Insights — How to Use
- Impression Share (IS): your % of available auctions won
- IS Lost (Budget): increase budget
- IS Lost (Rank): increase bids or QS
- Overlap Rate: how often you appear together with competitor
- Position Above Rate: how often competitor is above you

### Impression Share Targets
| Campaign Type | Target IS |
|---------------|-----------|
| Brand | 90%+ |
| Top non-brand | 60-80% |
| Long-tail / discovery | 30-50% |

---

## Campaign Settings

### Bidding (Start Here)
- New account (<50 conv/mo): Maximize Clicks with bid cap → manual CPC
- Growing (50-100 conv/mo): Target CPA or Maximize Conversions
- Mature (100+ conv/mo): Target ROAS or Maximize Conversion Value

### Search Network Only
- **Always** uncheck "Display Network expansion" — it dilutes data
- Consider Search Partners separately; can split off if performance differs

### Ad Schedule
- Review Dayparting report after 30+ days of data
- Don't over-restrict early — let algorithm learn
- Apply bid modifiers (+/- %) rather than full exclusions unless clear dead hours

### Device Modifiers
- Check device performance after 60+ days
- Mobile often has lower CVR — adjust bid modifier (-10% to -30%) if so
- Never fully exclude mobile without strong data

### Location Targeting
- **People in targeted location** (not "interested in")
- Review location report: city/region performance often varies significantly

---

## Optimization Checklist

### Weekly
- [ ] Search term report → add negatives + new keywords
- [ ] Pause keywords with 0 conversions and high spend (rule: 3× CPA target, 0 conv)
- [ ] Check impression share — budget or rank limited?
- [ ] Review RSA asset performance — pause "Low" assets, test new ones

### Monthly
- [ ] Auction Insights — check competitor movement
- [ ] Quality Score audit — flag anything <5
- [ ] Ad extension performance — pause underperforming sitelinks
- [ ] Check bid strategy health in Recommendations

### Quarterly
- [ ] Full keyword audit — remove/consolidate low performers
- [ ] Ad copy refresh (prevent fatigue even in search)
- [ ] Competitor analysis — new entrants, messaging shifts
- [ ] Landing page audit — match intent from top ad groups

---

## Diagnostics

### Campaign Not Spending
1. Check status (active? budget exhausted?)
2. Keywords approved? (policy holds)
3. Bids too low for impression share?
4. Targeting too narrow?

### High CPC, Low Conversions
1. Quality Score <6? → Fix ad relevance + landing page
2. Match types too broad? → Tighten to phrase/exact
3. Wrong intent keywords? → Review search terms
4. Landing page mismatch? → Align page to ad promise

### Sudden Performance Drop
1. Check auction insights — competitor increase?
2. Check search volumes — seasonal shift?
3. Review recent changes log
4. Check landing page — outage or slow load?

---

## Related Skills

- **google-ads-keywords**: Deep keyword research, match type strategy, keyword planning
- **google-ads-bidding**: Smart Bidding strategies, bid adjustments, portfolio strategies
- **google-ads-audiences**: RLSA, Customer Match, audience layering
- **google-ads-conversion-tracking**: Conversion setup, tag verification, attribution
- **page-cro**: Optimize landing pages receiving search traffic
- **copywriting**: Ad headline and description copywriting frameworks
