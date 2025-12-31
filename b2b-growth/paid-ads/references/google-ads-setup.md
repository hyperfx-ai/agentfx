# Google Ads Campaign Setup

## Campaign 1: Search Foundation

### Campaign Settings
```
Campaign Type: Search
Campaign Goal: Conversions
Bidding: Maximize conversions
Budget: $100-500/day (start)
Networks: Google Search only (disable partners)
Location: [YOUR_TARGET_GEO]
Languages: English
```

### Ad Group Structure

**Ad Group 1: Primary Keyword**
- Keywords (Phrase Match):
  - "[primary keyword]"
  - "[primary keyword] software"
  - "[primary keyword] tool"

**Ad Copy Template:**
```
Headline 1: [PRIMARY_KEYWORD] Software
Headline 2: [MAIN_BENEFIT] in [TIMEFRAME]
Headline 3: Used by [SOCIAL_PROOF]

Description 1: [PRIMARY_KEYWORD] software that helps [TARGET_ROLE]s [OUTCOME]. [CASE_STUDY] saw [METRIC] in [TIMEFRAME]. Try free.

Description 2: Get [BENEFIT_1], [BENEFIT_2], and [BENEFIT_3]. No credit card. [TIME_TO_VALUE] setup.

Display Path: [yourdomain].com/[keyword]
Final URL: [landing page matching keyword]
```

### Conversion Tracking

**Required Events (Google Tag Manager):**

Event 1: Signup
```javascript
gtag('event', 'sign_up', {
  'method': 'website',
  'value': 0
});
```

Event 2: Purchase
```javascript
gtag('event', 'purchase', {
  'transaction_id': '[ORDER_ID]',
  'value': [ORDER_VALUE],
  'currency': 'USD'
});
```

---

## Campaign 2: Performance Max (After Search Data)

### Prerequisites
- Run Search campaign for 2-4 weeks
- Export search terms that drove purchases
- Have 30+ conversions minimum

### Campaign Settings
```
Campaign Type: Performance Max
Campaign Goal: Conversions
Bidding: Maximize conversion value
Budget: 2x your Search budget
Conversion Goal: Purchase (primary), Signup (secondary)
```

### Audience Signals
```
Custom Segments:
- Search terms from Campaign 1 that drove purchases
  Example: "marketing analytics", "ga4 alternative", "data visualization tool"

Intent Audiences:
- In-market: Business Software
- Custom Intent: [your specific keywords]

Demographics:
- Job Function: [target roles]
- Company Size: [your ICP]
```

### Asset Groups

**Headlines (15 required):**
```
1. [PRIMARY_BENEFIT] for [TARGET_ROLE]s
2. [PRODUCT_NAME] - [MAIN_VALUE_PROP]
3. Get [OUTCOME] in [TIMEFRAME]
4. Used by [SOCIAL_PROOF]
5. [METRIC] Better Than [ALTERNATIVE]
6. Try [PRODUCT] Free
7. [SPECIFIC_FEATURE] Made Easy
8. [PAIN_POINT]? We Solved It
9. Join [NUMBER] [ROLE]s Using [PRODUCT]
10. [BENEFIT] Without [COMMON_PAIN]
11. Free [TIMEFRAME] Trial
12. [CASE_STUDY] Saw [RESULT]
13. Built for [TARGET_AUDIENCE]
14. [OUTCOME] Starts Here
15. [COMPETITOR] Alternative
```

**Descriptions (5 required):**
```
1. [PRODUCT] helps [TARGET_ROLE]s [MAIN_OUTCOME]. [FEATURE_1], [FEATURE_2], and [FEATURE_3] included. Try free for [TIMEFRAME].

2. Join [NUMBER] teams using [PRODUCT] to [OUTCOME]. Get [BENEFIT] in [TIME_TO_VALUE]. No credit card required.

3. [CASE_STUDY] went from [BEFORE] to [AFTER] using [PRODUCT]. Get [MAIN_BENEFIT] without [PAIN_POINT]. Start free trial.

4. Stop [OLD_WAY]. Start [NEW_WAY] with [PRODUCT]. Used by [SOCIAL_PROOF]. [METRIC] improvement average. Try it free.

5. [PRODUCT] is the [CATEGORY] tool that actually works. [SPECIFIC_CAPABILITY] that [COMPETITORS] can't match. Start now.
```

**Images:**
- Product screenshots (5 min)
- Team photos (2)
- Customer logos (3)
- Result charts/graphs (3)

**Videos:**
- Product demo (30s)
- Customer testimonial (15s)
- Founder story (30s)

---

## Campaign 3: Competitor Targeting

### Campaign Settings
```
Campaign Type: Search
Campaign Goal: Conversions
Bidding: Target CPA
Target CPA: 1.5x your normal CPA
Budget: $100-300/day
```

### Ad Group: Competitor Brand

**Keywords (Exact Match Only):**
```
[competitor name]
[competitor name] alternative
[competitor name] vs
[competitor name] pricing
[competitor name] review
```

**Ad Copy:**
```
Headline 1: [COMPETITOR] Alternative
Headline 2: Why Teams Switch to [YOUR_PRODUCT]
Headline 3: Try Free - No Credit Card

Description 1: Former [COMPETITOR] users love [YOUR_PRODUCT]. Get [BENEFIT_THEY_DON'T_HAVE] plus [UNIQUE_FEATURE]. [METRIC] better performance.

Description 2: Switch in [TIMEFRAME]. Import your [DATA_TYPE] from [COMPETITOR]. Try free for [TRIAL_PERIOD].

Final URL: [yourdomain].com/vs/[competitor]
```

**Landing Page Must Have:**
- Side-by-side feature comparison
- Price comparison
- Migration guide
- Customer testimonials from switchers

---

## Campaign 4: Brand Defense

### Campaign Settings
```
Campaign Type: Search
Campaign Goal: Conversions
Bidding: Target impression share
Target: 100% absolute top
Budget: $50-200/day
```

**Keywords (Exact Match):**
```
[your brand name]
[your product name]
[your brand] pricing
[your brand] login
[your brand] review
```

**Ad Copy:**
```
Headline 1: [YOUR_BRAND] - Official Site
Headline 2: [MAIN_VALUE_PROP]
Headline 3: Start Free Trial Today

Description 1: The official [YOUR_PRODUCT] website. [BENEFIT_1], [BENEFIT_2]. Used by [NUMBER] [ROLE]s. Try free.

Description 2: [LATEST_FEATURE] just launched. [OUTCOME] in [TIMEFRAME]. No credit card. [TIME_TO_VALUE] setup.
```

---

## Campaign 5: LinkedIn Text Ads (Awareness)

### Campaign Settings
```
Campaign Type: Text Ads
Bidding: Manual CPC
Max CPC: $2-3
Budget: $500/month
Audience: [YOUR_ICP]
```

**Ad Format:**
```
Headline (25 chars): [PRODUCT] for [ROLE]s
Description (75 chars): [MAIN_BENEFIT]. Try free. [social proof]
Image: 50x50px logo
```

**Strategy:**
- Run indefinitely
- Goal: Cheap impressions ($0.10-0.50 CPM)
- Retarget same audience as other channels
- Build awareness without budget burn

---

## Budget Allocation (At Scale)

**Month 1: Testing ($5k)**
- Search: $3k
- Pmax: $2k

**Month 2: Optimization ($10k)**
- Search: $3k
- Pmax: $6k
- Competitor: $1k

**Month 3+: Scaling ($20k+)**
- Pmax: 40% ($8k)
- Search: 30% ($6k)
- Competitor: 20% ($4k)
- Brand: 10% ($2k)

---

## Success Metrics

**Good Performance:**
- Search CPA: $150-300
- Pmax CPA: $50-150
- Competitor CPA: $200-400
- Brand CPA: $50-100

**What to Track Weekly:**
- CPA by campaign
- Search terms report (mine for Pmax)
- Conversion rate by landing page
- ROAS (if tracking revenue)

**When to Scale:**
- CPA is 50% below target for 2 weeks
- Conversion rate stable or improving
- ROAS above target threshold

