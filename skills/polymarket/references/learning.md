# Skill: Learning & Adaptation (v3.0)

**Layer**: 04 - Learning
**Objective**: To provide the agent with a framework for periodically analyzing its own performance, identifying the true sources of its profitability (its "edge"), and systematically adapting its parameters to improve future performance.

---

## 1. Core Principle: An Agent That Doesn't Learn is a Static Bot

The market is dynamic. Information sources change in reliability, and strategies that work today may not work tomorrow. An agent that cannot learn and adapt is doomed to fail. This skill transforms the agent from a simple bot into an evolving, intelligent system.

This skill is the primary responsibility of the **Learning & Adaptation Engine** and typically runs as a scheduled, offline task (e.g., once every 24 hours).

## 2. The Foundation: The Trade Database

Every decision, trade, and outcome must be logged to a structured database. This is the agent's memory. For each closed position, the database must record:

-   `trade_id`: Unique identifier for the trade.
-   `market_id`: The market that was traded.
-   `strategy`: The skill that triggered the trade (e.g., `scalp_trading`, `binary_arbitrage`).
-   `entry_timestamp`, `exit_timestamp`: Precise timing of the trade.
-   `entry_price`, `exit_price`: The execution prices.
-   `realized_pnl`: The final profit or loss.
-   `exit_reason`: The condition that triggered the exit (e.g., `stop_loss`, `take_profit`, `time_based`).
-   **`triggering_event_data`**: This is critical. For scalp trades, this should include the `headline`, the `source_of_news`, and the `magnitude` assessment.

## 3. The Analysis Protocol: Asking the Right Questions

Periodically, the Learning Engine runs a series of analytical queries against the trade database. The goal is not just to see if the agent is profitable, but to understand **why**.

### Key Analytical Questions:

1.  **What is my most profitable edge?**
    -   **Query**: `GROUP BY strategy | SUM(realized_pnl)`
    -   **Insight**: Is the agent making more money from high-volume, low-margin arbitrage, or from lower-volume, high-margin scalp trades? This helps allocate capital and processing power more effectively.

2.  **Which news sources are actually making me money?**
    -   **Query**: `FILTER strategy = 'scalp_trading' | GROUP BY triggering_event_data.source_of_news | SUM(realized_pnl)`
    -   **Insight**: This is the core of the **Credibility Scoring System**. Sources that consistently lead to profitable trades are valuable. Sources that lead to losses are just noise.

3.  **What are my optimal exit parameters?**
    -   **Query**: `FILTER strategy = 'scalp_trading' | ANALYZE relationship between take_profit_level and realized_pnl`
    -   **Insight**: Is the agent setting its `take_profit` targets too low and leaving money on the table? Or are they too high, causing profitable trades to revert to losses? The agent can analyze the price action *after* it exited to see what would have happened if it had held longer.

4.  **When am I losing money?**
    -   **Query**: `FILTER realized_pnl < 0 | GROUP BY exit_reason`
    -   **Insight**: If a large percentage of losses are coming from `stop_loss` triggers, are the stops too tight? If they are from `time_based_exit`, was the initial thesis simply wrong? This helps refine the entry conditions.

## 4. The Adaptation Mechanism: Closing the Loop

Analysis is useless without action. The final step of the learning process is for the agent to update its own configuration based on the insights it has generated.

-   **Update Source Credibility**: Based on the analysis of news source profitability, the agent updates the `credibility_score` for each source in its News Ingestion Pipeline configuration.

-   **Tune Strategy Parameters**: Based on the analysis of exit parameters, the agent can make small, incremental adjustments to its default `take_profit` and `stop_loss` percentages.

-   **Blacklist Markets/Keywords**: If the agent consistently loses money on trades related to certain keywords or market types (e.g., it is consistently wrong about sports outcomes), it can learn to assign a lower `magnitude` to or even ignore those triggers in the future.

By implementing this complete loop—**Log -> Analyze -> Adapt**—the agent ensures that every trade, win or lose, is a valuable learning experience that makes it smarter and more profitable over time.
