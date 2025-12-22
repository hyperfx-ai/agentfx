---
name: polymarket
description: Perform high-frequency scalp trading on the Polymarket prediction market based on real-time news analysis, with a secondary, opportunistic arbitrage strategy. Use this skill when trading on Polymarket or analyzing prediction market opportunities.
license: Complete terms in LICENSE.txt
---

# Polymarket Trading Skill

Use this skill to perform high-frequency trading on the Polymarket prediction market. Trade as a **news-reactive scalp trader** that profits from short-term price movements caused by new information.

## Core Philosophy

Treat Polymarket as a financial market, not a betting platform. Your primary edge is **speed of information**. Identify market-moving news, open a position before the market reprices, and close the position for a small profit moments later.

## Core Architecture

Operate on an **event-driven architecture** designed for speed. For a complete overview of the architecture, see [references/framework.md](references/framework.md).

## Primary Strategy: News-Driven Scalp Trading

1.  **Monitor News**: Continuously ingest data from real-time news sources.
2.  **Analyze Impact**: When a headline breaks, map it to a tradable Polymarket market.
3.  **Execute Trade**: If the news has a high potential impact, open a position at the current (incorrect) price.
4.  **Manage Position**: Monitor the price in real-time.
5.  **Exit Trade**: Close the position when a pre-defined `take_profit` or `stop_loss` is hit, or after a short time window.

For the detailed protocol, see [references/scalp-trading.md](references/scalp-trading.md).

## Secondary Strategy: Opportunistic Arbitrage

While hunting for news, run a constant background process to find and execute risk-free arbitrage opportunities (`YES` + `NO` shares costing less than $1.00).

For the detailed protocol, see [references/arbitrage.md](references/arbitrage.md).

## Reference Protocols

This skill includes several protocols detailed in the `references/` directory:

-   **[references/framework.md](references/framework.md)**: The core event-driven architecture.
-   **[references/news-monitoring.md](references/news-monitoring.md)**: How to ingest and analyze real-time news.
-   **[references/scalp-trading.md](references/scalp-trading.md)**: The protocol for executing news-driven scalp trades.
-   **[references/arbitrage.md](references/arbitrage.md)**: The protocol for executing opportunistic arbitrage trades.
-   **[references/position-management.md](references/position-management.md)**: How to manage open positions and execute exits.
-   **[references/learning.md](references/learning.md)**: The framework for analyzing performance and adapting over time.

## Usage

When using this skill, you should:

1.  **Configuration**: Ensure you have API keys for news sources and access to the Polymarket API.
2.  **Load References**: Load all associated reference documents for detailed protocols.
3.  **Execute**: Follow the event-driven architecture to monitor news, analyze opportunities, and execute trades.
