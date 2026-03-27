# Quantitative Strategy Report: Market Microstructure & Sentiment
**Prepared For:** Primetrade.ai Trading Desk
**Objective:** Analyze the relationship between the Bitcoin Fear/Greed Index and Hyperliquid trader execution to inform systematic trading strategies.

## Methodology
This analysis moved beyond static daily correlations by treating the market as a fluid time-series environment. 
1. **Data Alignment:** Execution timestamps (ms) were standardized and inner-joined with daily Fear/Greed index classifications.
2. **Transition Engineering:** The sentiment index was shifted by 1 period ($t-1$) to isolate day-over-day sentiment transitions (e.g., Fear → Greed), uncovering actionable time-series triggers.
3. **Machine Learning:** Standardized lifetime trader metrics (trade size, daily frequency, win rate) were processed through a K-Means Clustering algorithm to mathematically segment the user base into distinct behavioral archetypes.

## Key Insights
1. **The Profit Factor Collapse:** Win rates remain artificially stable across regimes, but risk profiles do not. The system's "Profit Factor" (Gross Profit / Gross Loss) is robust during standard "Greed" (~$7.20 gained per $1 lost) but mathematically collapses by over 50% during "Extreme Greed" (~$3.20).
2. **Time-Series Transition Traps:** The highest Expected Value (EV) trades occur during sentiment shifts, not static states. The transition from `Fear -> Greed` acts as a massive profitability catalyst. Conversely, a transition from `Neutral -> Greed` acts as a "false momentum trap," resulting in suppressed win rates and negative aggregate PnL.
3. **Behavioral Divergence (Smart vs. Dumb Money):** K-Means clustering identified three distinct archetypes. During market euphoria ("Extreme Greed"), "Retail Grinders" pivot to an aggressive 73% Long bias (chasing the top). In stark contrast, "High-Conviction Swing Traders" aggressively fade the retail crowd, shifting to an 85% Short bias.

## Strategy Recommendations
1. **Apply a "Contrarian Multiplier" to Position Sizing:**
   The data proves that trading against the prevailing sentiment (e.g., buying in Fear, selling in Greed) yields a significantly higher EV per trade than momentum-chasing. Systematic strategies should dynamically increase their capital allocation parameters when generating signals that fade a Fear/Greed extreme.
2. **Implement an "Extreme Greed" Risk Filter:**
   Because the overarching Profit Factor collapses during Extreme Greed, the risk engine must treat this as a distinct, high-danger regime. The firm should automatically cut allowable leverage limits in half during these phases to protect capital from retail overextension.
3. **Halt Long-Bias Algorithms on Neutral-to-Greed Shifts:**
   Algorithms should be programmed to identify the specific `Neutral -> Greed` transition and temporarily pause high-frequency long executions until the trend is structurally confirmed by subsequent daily closes.
