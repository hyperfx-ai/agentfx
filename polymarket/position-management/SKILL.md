---
name: position-management
description: This skill tracks real-time value of all open positions and executes pre-defined exit conditions with mechanical precision. This skill should be used when managing open trading positions in real-time, monitoring positions against exit conditions, or enforcing risk management rules automatically.
category: "Data & Analysis"
keywords: ["position management", "risk management", "exit strategy", "stop-loss", "take-profit", "trading discipline"]
license: MIT
---

# Real-Time Price Monitoring & Position Management

Provides a robust, high-frequency protocol for tracking the real-time value of all open positions and executing pre-defined exit conditions with mechanical precision.

## When to Use This Skill

This skill should be used when:
- Managing open trading positions in real-time with continuous monitoring
- Monitoring positions against pre-defined exit conditions systematically
- Enforcing risk management rules automatically without manual intervention
- Ensuring disciplined, emotion-free trading execution

## Core Principle: Profit Realization Through Disciplined Exits

An open position represents only an unrealized gain or loss. Profit is locked in exclusively when the position is closed. This skill ensures positions are closed in a disciplined, rules-based manner—the fundamental requirement for long-term profitability.

## How to Use This Skill

### The Real-Time Price Monitoring Loop

Driven by `OnPriceUpdate` events fired every time any market price changes.

**For every open position, perform these actions:**

1. **Update Current Value**: Recalculate the position's `current_value` based on the new market price
2. **Update Unrealized P&L**: Recalculate `unrealized_pnl` = (`current_value` - `initial_cost`)
3. **Check Exit Conditions**: Check new price against position's pre-defined exit conditions

### The Exit Condition Protocol

**Strict, first-come-first-served protocol.** The first exit condition met triggers immediate closure. No re-evaluation or waiting.

Check conditions in this priority order:

#### 1. Stop-Loss (Highest Priority)
- **Trigger**: `current_price <= stop_loss_price`
- **Action**: Immediately close with **market order** (get out now, stop the bleeding)
- **Rationale**: Primary risk management tool. Non-negotiable.

#### 2. Take-Profit
- **Trigger**: `current_price >= take_profit_price`
- **Action**: Close with limit or market order
- **Rationale**: Enforces discipline of taking profits. In scalp trading, holding for more often leads to edge evaporating.

#### 3. Time-Based Exit
- **Trigger**: `current_timestamp - entry_timestamp > max_holding_time`
- **Action**: Close with market order
- **Rationale**: Information edge decays quickly. If expected move hasn't happened within minutes, thesis is likely invalid.

### The Position Object: Single Source of Truth

Every open position is represented by a `Position` object:

```python
class Position:
    position_id: str
    market_id: str
    direction: Literal["BUY_YES", "BUY_NO"]
    entry_price: float
    position_size: int
    initial_cost: float
    current_value: float
    unrealized_pnl: float
    status: Literal["OPEN", "CLOSING", "CLOSED"]
    entry_timestamp: int
    exit_conditions: {
        "stop_loss": float,        # e.g., 0.37
        "take_profit": float,      # e.g., 0.45
        "max_holding_time": int    # e.g., 300 seconds
    }
```

## Example Usage

**Scenario:** Managing a scalp trade during a news event

**Initial State:**
- Position: `BUY YES @ 40¢`
- Size: 1000 shares
- Initial cost: $400
- Exit conditions:
  - Stop-loss: 37¢
  - Take-profit: 45¢
  - Max holding time: 300 seconds (5 minutes)

**Minute 0:00** - Position opens at 40¢
- Current value: $400
- Unrealized P&L: $0

**Minute 0:30** - Price updates to 42¢
- Current value: $420
- Unrealized P&L: +$20
- Check exits: None triggered
- **Continue monitoring**

**Minute 1:15** - Price updates to 44¢
- Current value: $440
- Unrealized P&L: +$40
- Check exits: None triggered
- **Continue monitoring**

**Minute 1:47** - Price updates to 45¢
- Current value: $450
- Unrealized P&L: +$50
- **Take-profit triggered!**
- **Action**: Immediately place `SELL YES` market order
- Position status: `CLOSING`

**Minute 1:49** - Order confirmed
- Position status: `CLOSED`
- Realized P&L: +$50
- Return: 12.5%
- Holding time: 109 seconds

**Result**: Disciplined exit at target, emotion-free execution

## Alternative Scenario: Stop-Loss Triggered

**Minute 0:45** - Price drops to 38¢
- Current value: $380
- Unrealized P&L: -$20
- Check exits: None triggered
- **Continue monitoring**

**Minute 1:12** - Price drops to 37¢
- Current value: $370
- Unrealized P&L: -$30
- **Stop-loss triggered!**
- **Action**: Immediately place `SELL YES` market order (priority: speed, not price)
- Position status: `CLOSING`

**Result**: Loss cut at -7.5%, preventing larger losses

## Alternative Scenario: Time-Based Exit

**Minute 5:00** - Price still at 41¢
- Current value: $410
- Unrealized P&L: +$10
- Neither stop-loss nor take-profit hit
- **Time-based exit triggered!** (300 seconds elapsed)
- **Action**: Close with market order

**Result**: Small profit taken, capital freed for next opportunity

## Critical Rules for Mechanical Execution

1. **First condition met = immediate action** - Execute without hesitation or re-evaluation
2. **Stop-loss is absolute** - Avoid moving or removing stop-loss levels
3. **Use market orders for exits** - Prioritize execution speed over price optimization
4. **Never override the system** - Prevent emotional interference with systematic execution
5. **Track every position** - Update position values on every price change

## Position Tracking Dashboard

Monitor these metrics in real-time:

| Position ID | Market | Entry | Current | P&L | Status | Time Held | Exit Conditions |
|-------------|--------|-------|---------|-----|--------|-----------|-----------------|
| POS-001 | ETH ETF | 40¢ | 44¢ | +$40 | OPEN | 1:15 | SL:37¢ TP:45¢ T:5m |
| POS-002 | Election | 62¢ | 60¢ | -$20 | OPEN | 2:34 | SL:59¢ TP:68¢ T:5m |

## Common Pitfalls to Avoid

❌ **Moving stop-losses** - Adjusting stops "for more room" destroys systematic discipline
❌ **Waiting for "better" exits** - First exit condition hit requires immediate execution
❌ **Ignoring time exits** - Theses that haven't validated within minutes typically won't
❌ **Emotional overrides** - Feelings about price movement must not interfere with rules
❌ **Not tracking positions** - Every open position requires constant monitoring

## Advanced: Portfolio-Level Risk Management

Track these metrics across all positions:

- **Total Capital at Risk**: Sum of all unrealized losses
- **Max Concurrent Positions**: Limit to avoid over-concentration
- **Max Single Position Size**: Cap individual position risk
- **Daily Loss Limit**: Stop trading if total losses hit threshold

## Reference Resources

See `references/position-management.md` for:
- Position sizing formulas
- Exit condition calculation tables
- Risk management guidelines
- Performance tracking templates

## Prerequisites

- Real-time price feed (WebSocket)
- Position tracking database
- Automated order execution
- Alert system for manual review (optional)

## Keywords

position management, risk management, exit strategy, stop-loss, take-profit, trading discipline, automated trading, portfolio management, real-time monitoring

