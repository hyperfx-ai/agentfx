---
name: news-monitoring
description: This skill monitors real-time information sources (APIs, Twitter, Telegram) and identifies market-moving news relevant to prediction markets. This skill should be used when monitoring real-time news sources for market-moving events, identifying which news affects which markets, or building an information edge for trading decisions.
category: "Data & Analysis"
keywords: ["news monitoring", "real-time data", "market impact", "prediction markets", "information edge"]
license: MIT
---

# News Monitoring & Market Impact Analysis

Provides a systematic process for ingesting real-time information, identifying its relevance to Polymarket, and assessing its potential market impact.

## When to Use This Skill

This skill should be used when:
- Monitoring real-time news sources for market-moving events continuously
- Identifying which news stories affect which specific prediction markets
- Assessing the magnitude and sentiment of news impact on market outcomes
- Building an information edge for informed trading decisions

## Core Principle

In markets driven by future events, accessing new information faster and interpreting it more accurately than others provides the single greatest competitive advantage. This skill enables building and leveraging that information edge systematically.

## How to Use This Skill

### 1. Configure News Sources

Set up monitoring for diverse real-time information sources across multiple tiers:

| Tier | Source Type | Examples | Latency | Reliability |
|------|-------------|----------|---------|-------------|
| **Tier 1** | Direct Feeds & APIs | Associated Press API, Reuters API, Official Government Feeds | Very Low | Very High |
| **Tier 2** | Social Media (High-Credibility) | Verified journalist Twitter/X lists, Official company/agency accounts | Low | High |
| **Tier 3** | Niche Communities | Specific Telegram channels, Discord servers, specialized forums | Very Low | Variable |
| **Tier 4** | Mainstream Media | Major news websites (CNN, BBC, etc.) | High | High |

### 2. Implement the Market Impact Analysis Protocol

When a new headline is detected, execute this protocol within milliseconds:

**Step 1: Keyword Extraction & Normalization**
- Extract key entities (people, organizations, locations), concepts, and action words from the headline
- Example: *"Federal Reserve Chair Powell hints at Q3 rate cut"* → Keywords: `Federal Reserve`, `Powell`, `rate cut`, `Q3`

**Step 2: Market Database Search**
- Maintain a local, cached database of all active prediction market titles and their IDs
- Perform high-speed search using extracted keywords
- Example: Search for `rate cut` matches *"Will the Fed cut rates by the end of Q3?"*

**Step 3: Impact Assessment**
- **Sentiment Analysis**: Is the news positive, negative, or neutral for the "YES" outcome?
  - *"hints at Q3 rate cut"* → **Positive** for YES
- **Magnitude Assessment**: How significant is this news?
  - Direct quote from Fed Chair = **HIGH** magnitude
  - Unverified rumor = **LOW** magnitude

**Step 4: Trigger Downstream Action**
- If a match is found with `MEDIUM` or `HIGH` impact, trigger the trading engine
- Include: `market_id`, `sentiment` (Positive/Negative), `magnitude` (Low/Medium/High), `source_of_news`

### 3. Implement Credibility Scoring

Track the reliability of each information source with a `credibility_score` (0.0 to 1.0):

- **Initial Score**: Set based on source tier (Tier 1 = 0.9, Tier 3 = 0.4)
- **Dynamic Updates**: Adjust scores based on trading outcomes
  - Profitable trade based on source → slightly **increase** score
  - Losing trade based on source → significantly **decrease** score

Weight the magnitude assessment by the credibility score to learn which sources are most reliable.

## Example Usage

**Scenario:** SEC Chair announces emergency press conference

1. **Detection**: News monitoring detects headline: *"SEC Chair to make unscheduled announcement at 2 PM EST"*
2. **Keyword Extraction**: `SEC`, `Chair`, `announcement`, `unscheduled`
3. **Market Search**: Finds match: *"Will an Ethereum ETF be approved by May 31st?"*
4. **Impact Assessment**:
   - Sentiment: Positive (for YES outcome)
   - Magnitude: HIGH (official statement from authority)
   - Source: Tier 1 (Reuters API, credibility 0.92)
5. **Trigger**: Fires trading opportunity with all parameters

**Result**: Trading engine receives actionable signal before market reprices

## Reference Resources

See `references/news-monitoring.md` for detailed information on:
- News API and social media monitoring setup
- Source credibility scoring systems
- Market matching algorithms
- Impact assessment frameworks

## Prerequisites

- API keys for news sources (AP, Reuters, etc.)
- Twitter/X API access for social media monitoring
- Telegram bot for channel monitoring
- Local database for market caching

## Keywords

news monitoring, real-time data, market impact analysis, sentiment analysis, prediction markets, information edge, trading signals

