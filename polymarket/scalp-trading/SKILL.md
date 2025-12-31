---
name: scalp-trading
description: This skill executes short-term directional trades on prediction markets based on new information. This skill should be used when new market-moving information becomes public, current market price is temporarily incorrect due to slow repricing, or trading on millisecond-to-minute timeframes with an information edge.
category: "Data & Analysis"
keywords: ["scalp trading", "prediction markets", "high-frequency trading", "position management", "short-term trading"]
license: MIT
---

# Scalp Trading

Provides a clear, rules-based protocol for executing short-term directional trades based on new information, capturing profit from the subsequent price movement.

## When to Use This Skill

This skill should be used when:
- New market-moving information becomes publicly available
- Current market price is temporarily incorrect due to slow repricing
- An information edge exists before the broader market reacts
- Trading on millisecond-to-minute timeframes with speed advantages

## Core Principle: Profit from Repricing

When new, market-moving information becomes public, the market's current price is temporarily "wrong." The goal is to open a position at the old, incorrect price and close it moments later at the new, correct price.

This is a **speed and execution** game, not a long-term prediction game.

## How to Use This Skill

### Step 1: Pre-Trade Analysis (Milliseconds Matter)

Upon receiving a trading signal from news monitoring, immediately:

1. **Fetch the Live Order Book**: Get the current bid-ask spread for the target market
2. **Verify Liquidity**: Is there enough depth to enter a position without significant slippage? If not, abort.
3. **Check for Existing Exposure**: Do you already have a position in this market? New information may trigger an exit, not an entry.
4. **Define the Trade Parameters**: Based on the signal's `sentiment` and `magnitude`, define parameters for this specific trade.

### Step 2: Define the Trade

Create a position with these parameters:

**Direction**:
- If `sentiment` is `Positive` → `BUY YES`
- If `sentiment` is `Negative` → `BUY NO`

**Entry Price**:
- Best available price on the opposite side of the order book
- For a `BUY` order → the lowest `ask`

**Position Size**:
- Pre-configured amount, adjusted for the `magnitude` of news
- `HIGH` magnitude = 1.5x standard size
- `MEDIUM` magnitude = 1.0x standard size

**Exit Conditions** (Critical - Non-Negotiable):
- **`take_profit`**: Target price based on expected repricing
  - `HIGH` magnitude = 10-15 cent move (e.g., 40¢ → 55¢)
  - `MEDIUM` magnitude = 5-8 cent move
- **`stop_loss`**: Tight, mandatory exit to cut losses if market moves against you
  - Typically 3-5 cents below entry price
- **`time_based_exit`**: Maximum holding time (e.g., 5 minutes)
  - If neither take-profit nor stop-loss hits, close automatically
  - Information edge decays rapidly

### Step 3: Execution

1. Place the entry order (e.g., `BUY YES @ 40¢`)
2. Create a `Position` object with all defined parameters
3. Pass position to Position Manager for active monitoring

### Step 4: The Exit (Most Important Part)

Once the position is `OPEN`, full attention is on the exit:

1. Position Manager continuously receives price updates
2. Checks current market price against `take_profit` and `stop_loss` levels
3. **The first level to be touched triggers an immediate market order to close**
4. No hesitation or re-evaluation

This mechanical, emotionless execution is the key to successful scalp trading.

## Example Usage

**Scenario:** News breaks that is positive for a market currently trading at 40¢

**1. Trigger**: 
- News: *"SEC approves Ethereum ETF filing"*
- Signal: Market *"ETH ETF by May 31st?"*, Sentiment: Positive, Magnitude: HIGH

**2. Pre-Trade Analysis**:
- Current price: 40¢ (stable)
- Order book depth: Sufficient liquidity
- No existing position in this market
- Go ahead with trade

**3. Define Trade Parameters**:
- Direction: `BUY YES`
- Entry: 40¢
- Take Profit: 55¢ (HIGH magnitude = 15¢ move)
- Stop-Loss: 37¢ (3¢ below entry)
- Time Exit: 5 minutes

**4. Execution**:
- Buy YES shares at 40¢
- Position now `OPEN`

**5. Management & Exit**:
- Price moves: 42¢ → 44¢ → 48¢ → 52¢ → 55¢
- Take-profit at 55¢ is hit
- Immediately place `SELL YES` market order
- Position closed with 15¢ profit per share

**Time elapsed**: Less than 2 minutes

**Result**: 37.5% return on capital deployed

## Critical Rules

1. **Never hold without exit conditions** - Define stop-loss, take-profit, and time exit before entry
2. **Execute exits mechanically** - First condition triggered requires immediate close without hesitation
3. **Information edge decays fast** - Exit if expected move fails to materialize within minutes
4. **Stop-loss is non-negotiable** - Cut losses immediately rather than hoping for recovery
5. **Take profits when hit** - Avoid greed by exiting at predetermined profit targets

## Reference Resources

See `references/scalp-trading.md` for:
- Entry criteria tables
- Exit rule configurations
- Position sizing formulas
- Risk management guidelines

## Prerequisites

- Real-time price feed (WebSocket connection)
- Order book access
- Position management system
- Automated exit execution

## Keywords

scalp trading, prediction markets, high-frequency trading, position management, short-term trading, market making, repricing, information edge

