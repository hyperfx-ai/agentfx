---
name: polymarket
description: An autonomous AI agent that performs high-frequency scalp trading on the Polymarket prediction market based on real-time news analysis, with a secondary, opportunistic arbitrage strategy.
license: Complete terms in LICENSE.txt
---

# Polymarket Trading Agent

This skill transforms the agent into a high-frequency trading bot for the Polymarket prediction market. It is designed to be a **news-reactive scalp trader** that profits from short-term price movements caused by new information.

## Core Philosophy

The agent treats Polymarket as a financial market, not a betting platform. The primary edge is **speed of information**. The goal is to identify market-moving news, open a position before the market reprices, and close the position for a small profit moments later.

## Core Architecture

The agent operates on an **event-driven architecture** designed for speed. For a complete overview of the architecture, see [references/framework.md](references/framework.md).

## Primary Strategy: News-Driven Scalp Trading

1.  **Monitor News**: Continuously ingest data from real-time news sources.
2.  **Analyze Impact**: When a headline breaks, map it to a tradable Polymarket market.
3.  **Execute Trade**: If the news has a high potential impact, open a position at the current (incorrect) price.
4.  **Manage Position**: Monitor the price in real-time.
5.  **Exit Trade**: Close the position when a pre-defined `take_profit` or `stop_loss` is hit, or after a short time window.

For the detailed protocol, see [references/scalp-trading.md](references/scalp-trading.md).

## Secondary Strategy: Opportunistic Arbitrage

While hunting for news, the agent runs a constant background process to find and execute risk-free arbitrage opportunities (`YES` + `NO` shares costing less than $1.00).

For the detailed protocol, see [references/arbitrage.md](references/arbitrage.md).

## Required Skills & Protocols

This master skill orchestrates several sub-protocols, which are detailed in the `references/` directory:

-   **[references/framework.md](references/framework.md)**: The core event-driven architecture of the agent.
-   **[references/news-monitoring.md](references/news-monitoring.md)**: How to ingest and analyze real-time news.
-   **[references/scalp-trading.md](references/scalp-trading.md)**: The protocol for executing news-driven scalp trades.
-   **[references/arbitrage.md](references/arbitrage.md)**: The protocol for executing opportunistic arbitrage trades.
-   **[references/position-management.md](references/position-management.md)**: How to manage open positions and execute exits.
-   **[references/learning.md](references/learning.md)**: The framework for analyzing performance and adapting over time.

## Getting Started

1.  **Configuration**: The agent must be configured with API keys for news sources and a connection to the Polymarket API.
2.  **Load Skills**: Load this skill and all associated reference documents.
3.  **Run**: Start the agent's main event loop.
