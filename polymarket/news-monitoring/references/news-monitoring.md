
# Skill: News Monitoring & Market Impact Analysis (v3.0)

**Layer**: 02 - Intelligence
**Objective**: To provide the agent with a systematic process for ingesting real-time information, identifying its relevance to Polymarket, and assessing its potential market impact.

---

## 1. Core Principle: Information is the Primary Edge

In a market driven by future events, having access to new information faster and interpreting it more accurately than others is the single greatest advantage. This skill enables the agent to build and leverage that information edge.

## 2. The News Ingestion Pipeline

The agent must be configured to monitor a diverse set of real-time information sources. This is not optional; it is the lifeblood of the scalp trading strategy. The agent must maintain persistent connections to these sources.

**Recommended Source Tiers:**

| Tier | Source Type | Examples | Latency | Reliability |
| :--- | :--- | :--- | :--- | :--- |
| **Tier 1** | Direct Feeds & APIs | Associated Press API, Reuters API, Official Government Feeds | Very Low | Very High |
| **Tier 2** | Social Media (High-Credibility) | Verified journalist Twitter/X lists, Official company/agency accounts | Low | High |
| **Tier 3** | Niche Communities | Specific Telegram channels, Discord servers, specialized forums | Very Low | Variable |
| **Tier 4** | Mainstream Media | Scraping major news websites (CNN, BBC, etc.) | High | High |

**Action**: The agent must be given a configurable list of these sources, including API keys, Twitter handles, and Telegram channel IDs.

## 3. The Market Impact Analysis Protocol

When the News Ingestion Pipeline fires an `OnNewHeadline` event, the Market Impact Engine must execute the following protocol within milliseconds.

### Step 1: Keyword Extraction & Normalization

-   **Action**: Extract key entities (people, organizations, locations), concepts, and action words from the headline.
-   **Example**: *"Federal Reserve Chair Powell hints at Q3 rate cut in Jackson Hole speech."*
    -   Keywords: `Federal Reserve`, `Powell`, `rate cut`, `Q3`

### Step 2: Market Database Search

-   **Action**: The agent must maintain a local, cached database of all active Polymarket market titles and their corresponding `market_id`.
-   **Action**: Perform a high-speed search (e.g., using an inverted index or a vector search) of this database using the extracted keywords.
-   **Example**: The search for `rate cut` matches the market: *"Will the Fed cut rates by the end of Q3?"* (`market_id: 0x123...`)

### Step 3: Impact Assessment

-   **Action**: For each matched market, assess the potential impact of the news.
-   **Sentiment Analysis**: Is the news positive, negative, or neutral for the "YES" outcome of the market?
    -   *"hints at Q3 rate cut"* -> **Positive** for the YES outcome.
-   **Magnitude Assessment**: How significant is this news?
    -   A direct quote from the Fed Chair is **HIGH** magnitude.
    -   A rumor from an unverified source is **LOW** magnitude.

### Step 4: Trigger Downstream Engine

-   **Action**: If a match is found with an impact assessment of `MEDIUM` or `HIGH`, the engine must immediately trigger the Strategy & Execution Engine.
-   **Payload**: The trigger must include:
    -   `market_id`
    -   `sentiment` (Positive/Negative)
    -   `magnitude` (Low/Medium/High)
    -   `source_of_news` (for later credibility analysis)

## 4. The Credibility Scoring System

The agent must learn which sources are reliable. Every information source in the pipeline must have a `credibility_score` (e.g., from 0.0 to 1.0).

-   **Initial Score**: Set based on the source tier (e.g., Tier 1 starts at 0.9, Tier 3 starts at 0.4).
-   **Dynamic Updates**: The Learning & Adaptation Engine is responsible for updating these scores. When a trade based on a news source is closed, the P&L is used to adjust the source's score.
    -   A profitable trade slightly **increases** the score.
    -   A losing trade significantly **decreases** the score.

This ensures that the agent learns over time to trust reliable sources and fade noise from unreliable ones. The `magnitude` assessment in Step 3 should be weighted by this `credibility_score`.

By implementing this skill, the agent moves from being a passive observer to an active, intelligent hunter of information-driven trading opportunities.
