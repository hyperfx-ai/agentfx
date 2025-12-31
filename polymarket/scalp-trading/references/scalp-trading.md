
# Skill: Scalp Trading (v3.0)

**Layer**: 03 - Strategies
**Objective**: To provide the agent with a clear, rules-based protocol for executing short-term directional trades based on new information, capturing profit from the subsequent price movement.

---

## 1. Core Principle: Profit from Repricing

This is the agent's primary profit-generating strategy. It is based on a simple premise: when new, market-moving information becomes public, the market's current price is temporarily "wrong." The agent's goal is to open a position at the old, incorrect price and close it moments later at the new, correct price.

This is a **speed and execution** game, not a long-term prediction game.

## 2. The Scalp Trading Protocol

This protocol is triggered by the Market Impact Engine when it identifies a high-impact news event related to a specific market.

### Step 1: Pre-Trade Analysis (Milliseconds Matter)

Upon receiving the trigger, the Strategy & Execution Engine must immediately:

1.  **Fetch the Live Order Book**: Get the current bid-ask spread for the target market from the Real-Time Price Streamer.
2.  **Verify Liquidity**: Is there enough depth on the order book to enter a position of the desired size without significant slippage? If not, abort.
3.  **Check for Existing Exposure**: Does the agent already have a position in this market? If so, this new information may trigger an exit, not an entry. (This is handled by the Position Manager).
4.  **Define the Trade Parameters**: Based on the trigger's `sentiment` and `magnitude`, define the parameters for this specific trade.

### Step 2: Defining the Trade (The `Position` Object)

-   **Direction**: If `sentiment` is `Positive`, the direction is `BUY YES`. If `Negative`, it's `BUY NO`.
-   **Entry Price**: The best available price on the opposite side of the order book (e.g., the lowest `ask` for a `BUY` order).
-   **Position Size**: A pre-configured amount, adjusted for the `magnitude` of the news (e.g., `HIGH` magnitude = 1.5x standard size).
-   **Exit Conditions**: This is critical. A scalp trade must have pre-defined, non-negotiable exit targets.
    -   `take_profit`: A target price based on the expected repricing. For a `HIGH` magnitude event, this might be a 10-15 cent move (e.g., from 40¢ to 55¢).
    -   `stop_loss`: A tight, mandatory exit to cut losses if the market moves against the trade. Typically 3-5 cents below the entry price.
    -   `time_based_exit`: A maximum holding time (e.g., 5 minutes). If neither the take-profit nor the stop-loss is hit within this window, the position is closed automatically. The information edge decays rapidly.

### Step 3: Execution

-   **Action**: The engine places the entry order (e.g., `BUY YES @ 40¢`).
-   **Action**: A `Position` object is created with all the defined parameters and is passed to the Position Manager for active monitoring.

## 3. The Exit: The Most Important Part of the Trade

Once the position is `OPEN`, the agent's full attention is on the exit. The Position Manager is responsible for this, and it is a simple, brutal process:

-   The manager continuously receives `OnPriceUpdate` events.
-   It checks the current market price against the position's `take_profit` and `stop_loss` levels.
-   **The first level to be touched triggers an immediate market order to close the position.** There is no hesitation or re-evaluation.

This mechanical, emotionless execution is the key to successful scalp trading.

## 4. Example: The 40¢ to 45¢ Trade

Let's walk through your example:

1.  **Trigger**: News breaks that is positive for a market currently trading at 40¢.
2.  **Analysis**: The agent's Impact Engine identifies the opportunity. The Execution Engine defines the trade:
    -   Direction: `BUY YES`
    -   Entry: 40¢
    -   Take Profit: 45¢
    -   Stop-Loss: 37¢
3.  **Execution**: The agent buys YES shares at 40¢. The position is now `OPEN`.
4.  **Management**: The Position Manager monitors the price.
    -   Price moves to 42¢... 44¢...
    -   Price hits 45¢.
5.  **Exit**: The `take_profit` is triggered. The Position Manager immediately places a `SELL YES` market order. The position is closed, and the 5¢ profit per share is realized.

This entire process might take less than a minute. That is the essence of this skill.
