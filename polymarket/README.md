# Polymarket Trading Skills

A suite of skills for algorithmic trading on prediction markets. Built on an event-driven architecture for speed and efficiency.

## Overview

These skills enable you to trade prediction markets like Polymarket using a systematic, data-driven approach. The core philosophy: treat prediction markets as financial markets, not betting platforms. Your primary edge is **speed of information**.

## Skills in This Category

### [news-monitoring](./news-monitoring)
Monitors real-time information sources and identifies market-moving news relevant to prediction markets. Extracts keywords, assesses impact magnitude, and triggers trading opportunities.

**When to use**: Setting up information pipelines, building an information edge

### [scalp-trading](./scalp-trading)
Executes short-term directional trades based on new information. Opens positions before markets reprice, then closes moments later for profit.

**When to use**: Trading on breaking news, capturing repricing opportunities

### [arbitrage-scanner](./arbitrage-scanner)
Continuously scans prediction markets for risk-free arbitrage opportunities where YES + NO shares cost less than $1.00.

**When to use**: Running as a background process, finding guaranteed profit opportunities

### [position-management](./position-management)
Tracks real-time value of all open positions and executes pre-defined exit conditions with mechanical precision.

**When to use**: Managing open positions, enforcing risk management rules

## Core Architecture

The skills work together in an event-driven architecture:

```
┌─────────────────┐
│ News Sources    │ (Twitter, APIs, Telegram)
│ (Tier 1-4)      │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ News Monitoring │ ← OnNewHeadline
│ Skill           │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Scalp Trading   │ ← OnPriceUpdate
│ Skill           │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Position        │ ← OnPriceUpdate
│ Management Skill│
└────────┬────────┘
         │
         ▼
    PROFIT / LOSS


┌─────────────────┐
│ Price Streamer  │ (WebSocket to Polymarket)
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Arbitrage       │ ← OnArbitrageOpportunity
│ Scanner Skill   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Position        │
│ Management Skill│
└─────────────────┘
```

## Primary Strategy: News-Driven Scalp Trading

1. **Monitor News**: news-monitoring skill continuously ingests data
2. **Analyze Impact**: Maps headlines to tradable markets
3. **Execute Trade**: scalp-trading skill opens position at incorrect price
4. **Manage Position**: position-management skill monitors in real-time
5. **Exit Trade**: Closes when stop-loss, take-profit, or time limit hits

**Timeline**: Entire process takes seconds to minutes

## Secondary Strategy: Opportunistic Arbitrage

While monitoring news, arbitrage-scanner runs as a background process finding risk-free opportunities where YES + NO < $1.00.

## Getting Started

### 1. Prerequisites

- API keys for news sources (AP, Reuters, Twitter, etc.)
- Polymarket API access
- WebSocket connection for real-time prices
- Position tracking database

### 2. Recommended Workflow

**Step 1**: Set up news-monitoring skill
- Configure your information sources
- Set up credibility scoring
- Test headline detection

**Step 2**: Implement position-management skill
- Define your risk parameters
- Set up stop-loss/take-profit rules
- Test exit execution

**Step 3**: Start with scalp-trading skill
- Begin with small position sizes
- Monitor performance closely
- Refine entry/exit rules

**Step 4**: Add arbitrage-scanner as background process
- Run continuously alongside scalp trading
- Capture opportunistic profits
- Track fee impact on profitability

### 3. Risk Management

Critical rules across all skills:

- **Always use stop-losses** - Non-negotiable
- **Define exits before entry** - Every position needs stop-loss, take-profit, and time exit
- **Execute mechanically** - No emotional overrides
- **Limit position sizes** - Never risk more than X% of capital on single trade
- **Track credibility** - Learn which news sources are reliable

## Example: Full Trading Cycle

**T+0:00** - Breaking news detected:
- news-monitoring: *"SEC Chair announces Ethereum ETF approval"*
- Keywords extracted: `SEC`, `Ethereum`, `ETF`, `approval`
- Market matched: *"Will an Ethereum ETF be approved by May 31st?"*
- Impact: Positive sentiment, HIGH magnitude
- Trigger sent to scalp-trading skill

**T+0:03** - Trade executed:
- scalp-trading: Buys YES shares at current price 40¢
- Position size: 1000 shares
- Entry cost: $400
- Exit rules: Stop-loss 37¢, Take-profit 55¢, Time limit 5 minutes
- Position handed to position-management skill

**T+0:15** - Price moves:
- position-management: Monitoring price updates
- 40¢ → 42¢ → 45¢ → 50¢ → 55¢
- Take-profit at 55¢ triggered
- Immediate market sell order placed

**T+0:17** - Trade closed:
- Exit price: 55¢
- Exit cost: $550
- Realized profit: $150
- Return: 37.5%
- Hold time: 17 seconds

Meanwhile, arbitrage-scanner detected:
- Different market: YES 62¢ + NO 36¢ = 98¢
- Bought bundle for $0.98, sold for $1.00
- Profit: $0.02 per bundle
- 500 bundles traded = $10 profit

**Total session profit**: $160

## Performance Tracking

Key metrics to monitor:

- **Win rate**: % of profitable trades
- **Average profit per trade**
- **Average hold time**
- **Max drawdown**: Largest losing streak
- **Source reliability**: Which news sources predict profitable moves
- **Arbitrage capture rate**: % of detected opportunities executed

## Advanced Topics

See these reference files in the category root:

- **framework.md**: Complete technical architecture
- **learning.md**: Performance analysis and strategy refinement
- **LICENSE.txt**: Usage terms

## Risk Warning

⚠️ **Important**: Prediction market trading carries significant risk. These skills are for educational and research purposes. 

- Start with small position sizes
- Test thoroughly in paper trading first
- Understand the markets you're trading
- Never risk more than you can afford to lose
- Be aware of regulatory requirements in your jurisdiction

## Contributing

Found ways to improve these skills? Contributions welcome:

- Improved entry/exit rules
- Additional news sources
- Better impact assessment models
- Performance optimizations

## License

See LICENSE.txt for complete terms.

---

hyperfx.ai


