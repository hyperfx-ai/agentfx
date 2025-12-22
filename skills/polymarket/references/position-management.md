# Skill: Real-Time Price Monitoring & Position Management (v3.0)

**Layer**: 05 - Execution
**Objective**: To provide the agent with a robust, high-frequency protocol for tracking the real-time value of all open positions and executing pre-defined exit conditions with mechanical precision.

---

## 1. Core Principle: You Don't Make Money Until You Sell

An open position is just an unrealized gain or loss. The profit is only locked in when the position is closed. This skill ensures that the agent closes its positions in a disciplined, rules-based manner, which is the key to long-term profitability.

This skill is the primary responsibility of the **Position Manager** component in the agent framework.

## 2. The Real-Time Price Monitoring Loop

The Position Manager is driven by the `OnPriceUpdate` event, which is fired by the Real-Time Price Streamer every time the price of any market changes.

**For every open position in its portfolio, the Position Manager must perform the following actions upon receiving a relevant `OnPriceUpdate` event:**

1.  **Update Current Value**: Recalculate the position’s `current_value` based on the new market price.
2.  **Update Unrealized P&L**: Recalculate the `unrealized_pnl` (`current_value - initial_cost`).
3.  **Check Exit Conditions**: Check the new price against the position’s pre-defined `exit_conditions`.

## 3. The Exit Condition Protocol

This is a strict, first-come-first-served protocol. The first exit condition to be met triggers an immediate closure of the position. There is no re-evaluation or waiting.

**The agent must check for these conditions in the following order of priority:**

### 1. Stop-Loss (Highest Priority)
-   **Trigger**: `current_price <= stop_loss_price`.
-   **Action**: Immediately instruct the Execution Engine to close the position with a **market order**. The goal is to get out and stop the bleeding, not to get the best price.
-   **Rationale**: This is the agent’s primary risk management tool. It is non-negotiable.

### 2. Take-Profit
-   **Trigger**: `current_price >= take_profit_price`.
-   **Action**: Instruct the Execution Engine to close the position. A **limit order** can be used here to ensure the target price is met, but a market order is acceptable for speed.
-   **Rationale**: This enforces the discipline of taking profits. In scalp trading, holding on for more profit often leads to the edge evaporating.

### 3. Time-Based Exit
-   **Trigger**: `current_timestamp - entry_timestamp > max_holding_time`.
-   **Action**: Close the position with a market order.
-   **Rationale**: The information edge in scalp trading decays extremely quickly. If the expected price move hasn’t happened within a few minutes, the thesis is likely invalid, and the capital should be redeployed.

## 4. The `Position` Object: The Single Source of Truth

Every open position is represented by a `Position` object that contains all the necessary information for this skill to function:

```python
class Position:
    # ... (other fields)
    initial_cost: float
    current_value: float
    unrealized_pnl: float
    status: Literal["OPEN", "CLOSING", "CLOSED"]
    exit_conditions: {
        "stop_loss": float,      # e.g., 0.37
        "take_profit": float,    # e.g., 0.45
        "max_holding_time": int # e.g., 300 seconds
    }
```

When the Position Manager detects that an exit condition has been met, it updates the position’s status to `CLOSING` and hands it off to the Execution Engine. Once the closing trade is confirmed, the status is moved to `CLOSED`, the `realized_pnl` is calculated, and the object is archived for the Learning Engine.

This mechanical, high-frequency monitoring and management of open positions is what separates a professional trading agent from a simple betting bot. It is the core of disciplined, profitable trading.
