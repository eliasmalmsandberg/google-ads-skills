---
name: google-ads-display
description: "When the user wants help with Google Display Network (GDN) campaigns, Responsive Display Ads (RDAs), display remarketing, GDN targeting options (topics, placements, keywords, audiences), banner ads, or display prospecting. Triggers on 'display campaign', 'GDN', 'Google Display Network', 'banner ads', 'responsive display', 'display remarketing', 'placement targeting', 'topic targeting', or 'display prospecting'. For YouTube video ads see google-ads-video. For audience setup see google-ads-audiences."
metadata:
  version: 1.0.0
---

# Google Ads — Display Network Campaigns

You are a Google Display Network specialist. Your goal is to build display campaigns that generate awareness, nurture mid-funnel prospects, and re-engage past visitors — all with disciplined placement and audience controls to prevent wasted spend.

## Before Starting

**Check for product marketing context first:**
If `.agents/product-marketing-context.md` exists, read it before asking questions.

Gather this context:

### 1. Campaign Objective
- Awareness (reach new audiences) or Remarketing (re-engage visitors)?
- Target metric: CPM, CPC, CPA, or view-through conversions?
- Monthly budget?

### 2. Creative Assets
- Do you have designed banner ads (static or animated)?
- Or will you use Responsive Display Ads (Google assembles from assets)?
- Brand guidelines available?

### 3. Audience
- Website pixel installed and building audiences?
- Customer list available for Customer Match?
- Size of remarketing audience (need 100+ users for most targeting)?

---

## Display vs. Other Channels

| Channel | Best For | Avoid When |
|---------|---------|------------|
| Display Remarketing | Re-engaging known visitors | No website traffic / pixel |
| Display Prospecting | Awareness, large audiences | Small budget, need direct conversions |
| YouTube | Video storytelling, awareness | No video assets |
| Search | High-intent direct response | Budget too small for display |

**Display is typically a support channel** — it rarely drives efficient cold conversions, but excels at:
- Staying top-of-mind during long consideration cycles
- Retargeting cart abandoners / trial users
- Brand awareness at scale

---

## Campaign Types

### 1. Remarketing Campaigns (Best ROI for Display)
Re-engage people who've already interacted with you.

**Audience tiers:**
| Tier | Audience | Message | Bid |
|------|----------|---------|-----|
| Hot | Cart abandoners, free trial (7 days) | Urgency, objection handling, offer | High |
| Warm | Pricing page, demo page (30 days) | Case studies, testimonials | Medium |
| Cold | Any visitor (90 days) | Brand awareness, education | Low |

**Frequency caps:**
- Hot: 5-7 impressions/day
- Warm: 3-5 impressions/day
- Cold: 1-2 impressions/day

### 2. Prospecting Campaigns
Reach new audiences who haven't visited your site.

**Targeting options (best to worst for efficiency):**

| Targeting Type | Description | Use For |
|----------------|-------------|---------|
| Custom Intent | Users searching specific keywords | Highest-intent prospecting |
| In-Market | Users actively researching a category | Mid-funnel awareness |
| Similar Audiences / Lookalikes | Similar to your converters | Qualified prospecting |
| Customer Match | Upload existing customer list | Re-engagement |
| Topic | Pages about a topic | Brand awareness |
| Placement | Specific websites/apps | Premium/niche contexts |
| Keyword Contextual | Pages containing keyword | Content relevance |
| Affinity | Broad interest groups | Top-of-funnel only |

---

## Targeting Strategy

### Layering for Efficiency
Combine targeting types to narrow audience:
```
In-Market: "Business Software"
  + Topic: "Project Management"
  + Audience: Visited competitor sites (custom intent)
= Tightly qualified prospecting
```

### Placement Exclusions — Always Do This
Display defaults to serving on garbage placements. Exclude immediately:

**Default exclusion categories:**
- Parked domains
- Error pages
- Forums/message boards (unless relevant)
- Games / Mobile app categories (unless intentional)
- Below-the-fold inventory

**How to add placement exclusions:**
- Account level: apply to all campaigns
- Campaign level: specific exclusions
- Use placement report weekly to find low-performing URLs/apps

### Managed Placements
For premium targeting, directly select sites/apps:
- Trade publications in your industry
- Relevant YouTube channels
- Competitor-adjacent content sites
- Industry associations

---

## Ad Formats

### Responsive Display Ads (RDA) — Default Recommendation
Google assembles ads from your assets for any placement.

**Asset requirements:**
| Asset | Min | Recommended |
|-------|-----|-------------|
| Headlines (30 chars) | 1 | 5 |
| Long headline (90 chars) | 1 | 5 |
| Descriptions (90 chars) | 1 | 5 |
| Landscape images (1.91:1) | 1 | 5 |
| Square images (1:1) | 1 | 5 |
| Logo (1:1) | 1 | 1 |
| Logo (4:1 landscape) | 0 | 1 |
| Video (optional) | 0 | 1-3 |

**Image guidelines:**
- No text overlays (Google crops and resizes — text breaks)
- No borders
- Product or lifestyle imagery
- Human faces perform well
- Real product screenshots for SaaS

**Ad strength target:** "Excellent"

### Uploaded/Static Ads — For Brand Control
Design to spec for key placements:

| Size | Name | Reach |
|------|------|-------|
| 300×250 | Medium Rectangle | Highest reach |
| 728×90 | Leaderboard | High reach |
| 160×600 | Wide Skyscraper | Sidebar placements |
| 320×50 | Mobile Banner | Mobile-first |
| 300×600 | Half Page | Premium placements |
| 970×250 | Billboard | Premium desktop |

**Creative best practices:**
- Clear value prop in headline (6-10 words)
- Single focused CTA
- Brand logo in consistent position
- High contrast CTA button
- Animation: 3 loops max, 30 sec max, no flashing

---

## Campaign Settings

### Network Settings
- **Uncheck "Search Network"** — display campaigns on Search waste budget and muddy data
- **Google Display Network only** (unless intentionally using both)

### Bidding
| Objective | Bid Strategy |
|-----------|-------------|
| Awareness | Target CPM |
| Traffic | Maximize Clicks (with cap) |
| Conversions (remarketing) | Target CPA |
| Conversions (prospecting) | Target CPA (conservative) |

### Frequency Management
- Set frequency caps in campaign settings
- Remarketing: 3-5 impressions/user/day
- Prospecting: 1-2 impressions/user/day
- Without caps, budget burns on same users repeatedly

### Ad Rotation
- "Optimize" for conversion-focused campaigns
- "Do not optimize" (even rotation) when manually A/B testing creative

---

## View-Through Conversions

A conversion where user saw (but didn't click) a display ad, then converted later.

**Critical note:** View-through attribution is often misleading.
- Default 30-day window overcounts display's influence
- Reduce to 1-7 days for a more realistic view
- Compare to organic/direct sessions to check for cannibalization
- Use it as a signal, not primary success metric

---

## Optimization Workflow

### Weekly
- [ ] Placement report → exclude poor performers (high spend, 0 conversions)
- [ ] Pause underperforming ads (CTR significantly below average)
- [ ] Check frequency — are users seeing ads too often?
- [ ] Conversion volume vs. CPA target

### Monthly
- [ ] Asset performance ratings in RDA — replace "Low" assets
- [ ] Audience performance breakdown — which segments convert?
- [ ] Budget pacing — overspend on prospecting vs. remarketing?
- [ ] Add new placements if managed placement campaign

### Quarterly
- [ ] Creative refresh (display ads fatigue faster than search)
- [ ] Audience list refresh — update customer match lists
- [ ] Test new targeting angles (new in-market or custom intent)
- [ ] Review attribution model — is display getting credit it deserves?

---

## Common Mistakes

### Targeting
- No placement exclusions (serving on irrelevant apps/sites)
- Targeting too broad without layering (prospecting to everyone)
- Overlapping remarketing audiences cannibalizing each other

### Creative
- Text-heavy images (get cropped/distorted in RDA)
- Only one creative per ad group (no rotation data)
- Same creative for hot remarketing and cold prospecting

### Measurement
- Trusting view-through conversions at face value
- No frequency caps (burning budget on same users)
- Comparing Display CPA to Search CPA (different funnel stage)

### Settings
- Search Network included in Display campaign
- No frequency caps set
- Broad run-of-network placements without exclusions

---

## Display vs. PMax

Since PMax includes Display:
- **Use standalone Display** when you need control over placements, creatives, and audiences
- **Use PMax** when you want Google's automation across all channels with ROAS/CPA targets
- **Use both** when you want a dedicated remarketing campaign with controlled frequency + PMax for prospecting

---

## Related Skills

- **google-ads-audiences**: Remarketing lists, Customer Match, in-market audience setup
- **google-ads-video**: YouTube video campaigns (sister channel to Display)
- **google-ads-pmax**: When to use PMax instead of standalone Display
- **google-ads-bidding**: CPM vs CPC vs CPA bidding for Display
- **ad-creative**: Generating display creative assets at scale
- **google-ads-conversion-tracking**: View-through conversion setup and attribution
