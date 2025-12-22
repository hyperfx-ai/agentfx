'''# Polymarket AI Agent: Core Framework (v3.0 - Real-Time News & Scalp Trading)

**Version**: 3.0
**Author**: Manus AI
**Core Philosophy**: The agent is a **news-reactive scalp trader** that profits from short-term price movements caused by new information. It treats Polymarket as a high-frequency financial market, not a long-term betting platform. Arbitrage is a secondary, opportunistic strategy.

---

## 1. The Event-Driven Architecture

This framework abandons the simple, fixed-interval loops of previous versions. It is built on an **event-driven architecture**. The agent does not just run on a schedule; it **reacts** to incoming events from multiple sources in real-time. This is essential for scalp trading where speed is the primary edge.

**The Core Events:**

1.  `OnNewHeadline(headline_data)`: Triggered when a new piece of information is detected.
2.  `OnPriceUpdate(price_data)`: Triggered every time the price of a monitored market changes.
3.  `OnArbitrageOpportunity(arb_data)`: Triggered when a risk-free arbitrage is detected.

## 2. Revised Component Architecture

The agent is a system of specialized, interconnected components designed for speed and reactivity.

| Component | Primary Function | Triggers On... |
| :--- | :--- | :--- |
| **News Ingestion Pipeline** | Continuously monitors external sources (APIs, RSS, Telegram, Twitter) for new information. | (Always Running) |
| **Market Impact Engine** | Maps incoming headlines to specific, tradable Polymarket markets. | `OnNewHeadline` |
| **Real-Time Price Streamer**| Maintains a persistent WebSocket connection to Polymarket for live price data. | (Always Running) |
| **Strategy & Execution Engine**| The core decision-maker. Decides whether to trade based on an event and executes the order. | `OnNewHeadline`, `OnPriceUpdate`, `OnArbitrageOpportunity` |
| **Position Manager** | Tracks all open positions, calculates real-time P&L, and enforces exit conditions. | `OnPriceUpdate` |
| **Learning & Adaptation Engine**| Periodically analyzes the database of closed trades to refine strategies and parameters. | (Scheduled Task) |

---

## 3. The Information Flow: From Headline to Trade

This is the primary execution path for the agent's main strategy: news-driven scalp trading.

1.  **Ingestion**: The **News Ingestion Pipeline** detects a new headline: *"SEC Chair to make unscheduled announcement at 2 PM EST."*

2.  **Impact Analysis**: The **Market Impact Engine** receives this headline.
    -   It uses NLP and keyword matching to search its database of active Polymarket markets.
    -   It identifies a potential match: *"Will an Ethereum ETF be approved by May 31st?"*
    -   It determines the potential impact is **HIGH**.

3.  **Pre-Trade Analysis**: The **Strategy & Execution Engine** is triggered.
    -   It immediately fetches the current order book for the matched market via the **Real-Time Price Streamer**.
    -   It sees the price is currently stable at 40¢.
    -   It loads the `scalp-trading.md` skill and determines its entry condition: *Buy on news catalyst before the market reacts.*

4.  **Execution**: The engine decides to **OPEN** a position.
    -   It places a `BUY` order for YES shares at 40¢.
    -   A new `Position` object is created and passed to the **Position Manager**.

5.  **Active Management**: The position is now live.
    -   The **Position Manager** receives every `OnPriceUpdate` event from the Price Streamer.
    -   The price moves: 41¢... 43¢... 48¢... 55¢.
    -   The Position Manager continuously updates the position's `unrealized_pnl`.
    -   The price hits the pre-defined `take_profit` target of 55¢.

6.  **Closure**: The **Position Manager** triggers an exit.
    -   It instructs the **Strategy & Execution Engine** to **CLOSE** the position.
    -   The engine places a `SELL` order for the YES shares at 55¢.
    -   The trade is complete. The `realized_pnl` is calculated and the closed position is logged to the database.

7.  **Learning**: Days later, the **Learning Engine** analyzes this trade. It notes that the trade was profitable and that the news source was reliable, increasing that source's credibility score.

---

## 4. The Secondary Loop: Opportunistic Arbitrage

While the agent is primarily focused on news, it runs a constant, high-frequency background task.

-   The **Real-Time Price Streamer** feeds prices into a dedicated **Arbitrage Detector**.
-   This detector continuously checks the `YES + NO < 1.00` condition for all markets.
-   If an opportunity is found, it fires an `OnArbitrageOpportunity` event.
-   The **Strategy & Execution Engine** receives this event, loads the `arbitrage.md` skill, and executes the risk-free trade.

This ensures the agent never misses out on "free money" while it hunts for larger, news-driven scalp trades.

## 5. Why This Framework is Superior

-   **Speed**: It is designed to react to information in milliseconds, which is the primary edge in scalp trading.
-   **Specialization**: Each component has a single, clear responsibility, making the system more robust and easier to debug.
-   **Realism**: This architecture mirrors how high-frequency trading firms operate: they ingest data, find signals, and execute with speed.
-   **Flexibility**: It can execute multiple strategies (scalping, arbitrage) in parallel without them interfering with each other.

This event-driven, news-reactive framework is the correct and necessary foundation for an agent that aims to profit from the fast-paced, information-driven environment of Polymarket.'''
