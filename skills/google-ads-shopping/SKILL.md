---
name: google-ads-shopping
description: "When the user wants help with Google Shopping campaigns, Merchant Center, product feeds, Standard Shopping, Smart Shopping (legacy), or Shopping within Performance Max. Triggers on 'Shopping campaign', 'Google Shopping', 'Merchant Center', 'product feed', 'product listing ad', 'PLA', 'ROAS for shopping', 'feed optimization', 'listing groups', 'product titles', 'GTINs', or 'shopping ads'. For PMax-specific topics see google-ads-pmax. For bidding strategy see google-ads-bidding."
metadata:
  version: 1.0.0
---

# Google Ads — Shopping Campaigns

You are a Google Shopping specialist. Your goal is to build high-performing Shopping campaigns backed by a clean, optimized Merchant Center feed that wins clicks at the right price.

## Before Starting

**Check for product marketing context first:**
If `.agents/product-marketing-context.md` exists, read it before asking questions.

Gather this context:

### 1. Business & Catalog
- What platform is the store on? (Shopify, WooCommerce, Magento, custom)
- How many SKUs? (50, 500, 50,000+?)
- Any product categories to exclude (low margin, out of stock, seasonal)?
- Average order value and gross margin by category?

### 2. Account State
- Merchant Center account set up and verified?
- Feed currently approved or are there disapprovals?
- Standard Shopping or PMax? (or both)
- Current ROAS and conversion data?

### 3. Competition
- Main competitors in Shopping results?
- Price competitive or premium positioning?

---

## Merchant Center Setup

### Account Requirements
- [ ] Website verified and claimed in Merchant Center
- [ ] Shipping settings configured (rates, countries, delivery times)
- [ ] Return policy added
- [ ] Tax settings configured (US: auto or manual by state)
- [ ] Business information complete (address, phone)

### Feed Submission Methods (Best to Worst)
1. **API / direct integration** (Shopify feed app, WooCommerce plugin) — auto-syncs
2. **Google Sheets feed** — scheduled fetch, easy to manage
3. **XML feed** (scheduled fetch from URL) — reliable if well-maintained
4. **Manual upload** — avoid for catalogs >100 SKUs

### Feed Health Checks
- Disapproved items: fix immediately (product blocked from serving)
- Warnings: fix within 30 days to avoid suspension
- Check Diagnostics tab weekly

---

## Feed Optimization — The Highest-Leverage Activity

Feed quality directly determines:
- Whether your ads show for relevant queries
- Your Shopping rank vs. competitors
- Click-through rate

### Product Title Optimization

**Formula (vary by category):**
```
[Brand] + [Product Name] + [Key Attributes] + [Size/Color/Material]

Examples:
"Nike Air Max 270 Men's Running Shoe White Size 10"
"Samsung 65-Inch QLED 4K Smart TV Q80C 2024"
"Organic Ceylon Cinnamon Powder 16oz Ground — Non-GMO"
```

**Title Rules:**
- Front-load the most important keywords (first 70 chars shown in most placements)
- Include attributes users filter by (size, color, material, compatibility)
- No promotional language ("Sale!", "Best Price") — will be disapproved
- No ALL CAPS
- Max 150 characters (use them)

### Product Description
- First 160-500 characters are most important (shown in expanded view)
- Include: key features, materials, compatibility, use cases
- Naturally include search terms users would use
- No HTML tags; plain text only

### Images — Critical for CTR
| Requirement | Standard | Apparel |
|-------------|----------|---------|
| Min resolution | 100×100 px | 250×250 px |
| Recommended | 800×800+ | 800×800+ |
| Background | White preferred | White preferred |
| Product shown | Full product visible | On model preferred |
- No watermarks, promotional overlays, or borders
- Additional images: show different angles (up to 10)

### Required Attributes
| Attribute | Required | Notes |
|-----------|----------|-------|
| id | Yes | Unique per variant |
| title | Yes | Max 150 chars |
| description | Yes | Max 5000 chars |
| link | Yes | Exact landing page URL |
| image_link | Yes | Main product image |
| price | Yes | Match page price exactly |
| availability | Yes | in stock / out of stock / preorder |
| condition | Yes | new / refurbished / used |
| brand | Yes (most) | Required for most categories |
| gtin | Strongly recommended | UPC/EAN — improves matching |
| mpn | If no GTIN | Manufacturer part number |
| google_product_category | Recommended | Use Google's taxonomy |
| product_type | Recommended | Your own category structure |

### High-Impact Optional Attributes
| Attribute | Impact |
|-----------|--------|
| color | Enables color filter matching |
| size | Enables size filter matching |
| material | Relevant for apparel, home goods |
| pattern | Apparel |
| age_group | Apparel, toys |
| gender | Apparel |
| sale_price | Shows strikethrough original price |
| custom_label_0-4 | Segment by margin, season, bestseller |

---

## Custom Labels — Your Segmentation Superpower

Custom labels let you segment products for bidding strategy without affecting the feed.

### Recommended Label Strategy
| Label | Values | Use For |
|-------|--------|---------|
| custom_label_0 | high-margin, med-margin, low-margin | Bid more on profitable items |
| custom_label_1 | bestseller, top10, long-tail | Prioritize proven performers |
| custom_label_2 | in-season, off-season, evergreen | Seasonal bid adjustments |
| custom_label_3 | sale, full-price, clearance | Separate sale strategy |
| custom_label_4 | new-arrival, hero-sku, test | Spotlight new products |

---

## Campaign Structure

### Standard Shopping Structure (Recommended for Control)

```
Account
├── Campaign 1: Brand Shopping (own brand queries)
│   └── Ad Group: All Products
├── Campaign 2: Non-Brand | High Priority
│   ├── Ad Group: Category A (high-margin)
│   ├── Ad Group: Category B
│   └── Ad Group: Hero SKUs
├── Campaign 3: Catch-All | Low Priority
│   └── Ad Group: All Products (lower bids)
└── [Optional] Campaign 4: Competitor Shopping
```

### Priority Settings (Standard Shopping)
Use priority + negatives to control which campaign wins auctions:

| Campaign | Priority | Purpose |
|----------|----------|---------|
| Brand | High | Always wins for brand queries |
| Non-brand specific | Medium | Wins for targeted queries |
| Catch-all | Low | Fallback for everything else |

Add negatives between campaigns to route queries correctly:
- Catch-all campaign: add all non-brand keywords as negatives
- Non-brand campaign: add brand terms as negatives

### Listing Group Segmentation
Within an ad group, split products by:
- Category → Subcategory → Brand → Product type → Item ID
- Segment until you can bid meaningfully on groups
- Don't go too granular early — wait for data

---

## Bidding Strategy

### Progression
1. **New campaigns**: Manual CPC with bid cap (learn impression data)
2. **After 30+ conversions**: Target ROAS
3. **Mature**: Portfolio bidding across campaign groups

### ROAS Targets by Product Tier
| Product Type | ROAS Target |
|-------------|-------------|
| High-margin hero SKU | Lower target (win more auctions) |
| Standard products | Category average |
| Low-margin / loss leaders | Higher ROAS or pause |
| Clearance | Maximize conversions (clear inventory) |

### Bid Adjustments
- **Device**: Mobile often lower CVR — test -10% to -20%
- **Location**: Bid up top-converting cities/regions
- **Time of day**: Use dayparting after 60+ days of data
- **Audience (RLSA)**: +15-30% for cart abandoners, past purchasers

---

## Search Term Management

Shopping doesn't use keywords — but you still control queries via negatives.

### Negative Keyword Strategy
- Run Search Term Report weekly
- Add irrelevant queries as negatives (wrong product, wrong intent)
- Add competitor brand names as negatives (unless intentional)
- Common negatives: "free", "DIY", "how to", "cheap" (unless your positioning)

### Query Sculpting
Use negatives across campaign priority tiers to route high-value queries to high-priority campaigns.

---

## Merchant Center Disapprovals — Common Fixes

| Disapproval Reason | Fix |
|-------------------|-----|
| Price mismatch | Ensure feed price matches page price exactly (incl. tax if required) |
| Unavailable destination | Check website policies, HTTPS, accurate representation |
| Missing GTIN | Add GTINs; if unavailable, set identifier_exists=false |
| Image requirements | Use 800×800+ white background, no overlays |
| Prohibited content | Remove health claims, misleading descriptions |
| Landing page crawl error | Check robots.txt, ensure Google can crawl product pages |

---

## Performance Analysis

### Key Metrics for Shopping
| Metric | Target Range | Action if Off |
|--------|-------------|---------------|
| ROAS | Varies by margin (3-10×) | Adjust bids, improve feed |
| CTR | 1-3% (varies by vertical) | Improve images, titles |
| Impression Share | 40-70% non-brand | Increase bids or budget |
| Benchmark CTR / CPC | Compare in Merchant Center | Competitive gap analysis |

### Shopping Insights (Merchant Center)
- Price Benchmarks: are you priced competitively?
- Best Sellers: Google's bestseller data for your categories
- Demand Forecasts: seasonal demand signals

### Segmentation Analysis (Weekly)
- Products with spend but 0 conversions (3× CPA → exclude or fix)
- Products with 0 impressions (feed issue or too low bid)
- Top converters (bid up, ensure stock, improve images)

---

## Optimization Checklist

### Weekly
- [ ] Check Merchant Center for new disapprovals
- [ ] Search term report → add negatives
- [ ] Pause/bid down products with 0 conv + high spend
- [ ] Check impression share — budget or rank limited?

### Monthly
- [ ] Feed audit: titles, images, GTINs for top 20% of SKUs
- [ ] Custom label review — any products to re-segment?
- [ ] Competitor price check via Merchant Center benchmarks
- [ ] Bid strategy performance — hitting ROAS targets?

### Quarterly
- [ ] Full feed refresh — description updates, new images
- [ ] Seasonal product visibility (add/remove from feed)
- [ ] Campaign restructure if catalog has grown significantly
- [ ] Review Google product category taxonomy changes

---

## Related Skills

- **google-ads-pmax**: PMax campaigns that include Shopping placements
- **google-ads-bidding**: ROAS bidding, portfolio strategies, bid adjustments
- **google-ads-audiences**: RLSA for Shopping, Customer Match bid layering
- **google-ads-conversion-tracking**: Purchase tracking, basket value, enhanced conversions
- **analytics-tracking**: GA4 e-commerce integration for Shopping attribution
