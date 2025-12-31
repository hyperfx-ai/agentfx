
# Skill: Arbitrage Detection & Execution (v3.0)

**Layer**: 03 - Strategies
**Objective**: To provide the agent with a high-frequency, opportunistic protocol for identifying and executing risk-free arbitrage trades.

---

## 1. Core Principle: Opportunistic Profit

While scalp trading is the agent's primary strategy, arbitrage is its secondary, opportunistic strategy. It is the process of finding and exploiting market inefficiencies to lock in a guaranteed, risk-free profit. This skill runs as a constant background process, ensuring the agent never misses out on "free money."

## 2. The Arbitrage Detection Loop

This is a high-frequency loop that runs independently of the news-driven scalp trading logic.

1.  **Data Source**: The loop is fed by the **Real-Time Price Streamer**, which provides a live feed of the order books for all active markets.
2.  **The Core Calculation**: For every market, the Arbitrage Detector continuously performs one simple check:
    -   Fetch the `best_yes_ask` (the lowest price someone is willing to sell a YES share for).
    -   Fetch the `best_no_ask` (the lowest price someone is willing to sell a NO share for).
    -   Calculate the `bundle_cost = best_yes_ask + best_no_ask`.
3.  **The Trigger Condition**: If `bundle_cost < (1.00 - min_profit_threshold)`, an `OnArbitrageOpportunity` event is fired.
    -   The `min_profit_threshold` is a configurable parameter that accounts for potential fees and ensures the trade is worthwhile (e.g., 0.005 or 0.5 cents).

## 3. The Arbitrage Execution Protocol

When the Strategy & Execution Engine receives the `OnArbitrageOpportunity` event, it executes the following protocol immediately.

### Step 1: Pre-Trade Verification

-   **Verify Depth**: Is there enough volume available at the `best_yes_ask` and `best_no_ask` to execute a meaningful position size? If the depth is only for a few shares, the opportunity may not be worth the gas fees.
-   **No Analysis Needed**: Unlike scalp trading, there is no need for sentiment analysis or magnitude assessment. This is a purely mathematical trade.

### Step 2: Define the Arbitrage Position

-   **Strategy**: `binary_arbitrage`
-   **Assets**: One YES share and one NO share.
-   **Entry**: Place two simultaneous limit orders to buy the YES and NO shares at their respective best ask prices.
-   **Exit Strategy**: This is key. The exit is **not** to wait for resolution. The exit is to **sell the bundle** as soon as possible for a price close to $1.00. The exit condition is `ARBITRAGE_COMPLETE`.

### Step 3: Execution and Closure

1.  The entry orders are placed. The position becomes `OPEN`.
2.  The agent immediately places two corresponding `SELL` limit orders for the YES and NO shares at prices that would lock in the profit (e.g., selling the YES at its current `bid` and the NO at its current `bid`).
3.  As soon as these sell orders are filled, the position is `CLOSED`, and the profit is realized.

## 4. Why This is a Background Task

Arbitrage opportunities are fleeting, often lasting only for a few seconds. They are also purely mechanical and require no complex decision-making. By running this as a constant, high-frequency background task, the agent can:

-   **Not be distracted** from its primary task of analyzing news and executing scalp trades.
-   **Instantly react** to mathematical inefficiencies the moment they appear.
-   **Generate a consistent, low-risk stream of profit** that complements the higher-risk, higher-reward scalp trades.

This two-pronged approach—a primary, news-driven strategy and a secondary, opportunistic arbitrage strategy—creates a more robust and profitable trading agent.
