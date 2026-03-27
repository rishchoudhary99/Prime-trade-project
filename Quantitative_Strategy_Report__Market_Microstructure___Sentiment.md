# Quantitative Strategy Report: Market Microstructure & Sentiment 
**Prepared For:** Primetrade.ai Trading Desk  
**Subject:** Extracting Alpha and Managing Risk using the Fear & Greed Index

## Overview & Methodology
This repository contains the end-to-end Data Science assignment submission. The core objective was to move beyond static data correlations and uncover actionable trading intelligence. 

**Data Engineering Methodology:**
1. **Time-Series Alignment:** Execution timestamps (ms) were standardized and merged with daily Fear/Greed index classifications via an inner join.
2. **Transition Engineering:** The sentiment data was shifted by 1 period to capture the previous day's sentiment (t-1), allowing us to calculate day-over-day "Sentiment Transitions" (e.g., Fear → Greed).
3. **Unsupervised Machine Learning:** Standardized trader metrics (trade size, daily frequency, win rate) were fed into a **K-Means Clustering** algorithm to mathematically segment the user base into distinct behavioral archetypes.

---

## Part A: Alpha Generation (Where the Edge Lives)

### 1. The "Contrarian Multiplier"
Trading against the prevailing market sentiment provides a massive, measurable mathematical edge compared to trend-following.
* **Contrarian Positioning** (Buying in Fear / Selling in Greed) yields significantly higher Expected Value (EV) per trade compared to **Momentum Positioning** (Buying in Greed / Selling in Fear). 
* **Actionable Rule:** Systematic strategies should apply a "Contrarian Multiplier" to position sizing. Algorithms must be granted wider risk parameters specifically when generating buy signals during "Extreme Fear" days.

### 2. Time-Series Transition Traps
Analyzing sentiment as a fluid time-series reveals that the highest-value trades occur when sentiment *changes*, not when it remains static.
* **The `Fear -> Greed` Catalyst:** Days where sentiment formally shifts from Fear to Greed produce the highest average PnL per trade across the entire dataset.
* **The `Neutral -> Greed` Trap:** Conversely, days shifting from Neutral directly to Greed are structurally toxic. This represents a "false momentum trap" where retail traders overextend on the long side before the market confirms the trend, resulting in heavily suppressed win rates.
* **Actionable Rule:** Halt all high-frequency, long-biased algorithms immediately upon a `Neutral -> Greed` transition.

---

## Part B: Risk Management (Microstructure Vulnerabilities)

### 1. Profit Factor Collapse
Win rates are notoriously deceptive; measuring the "Profit Factor" (Gross Profit / Gross Loss) exposes true systemic regime risk.
* In standard "Greed," the system's Profit Factor is highly robust (**~$7.20** gained for every $1.00 lost).
* When sentiment shifts to "Extreme Greed," the Profit Factor mathematically collapses by over 50% down to **~$3.20**.
* **Actionable Rule:** Risk engines must treat "Extreme Greed" as a high-risk regime, automatically cutting allowable leverage limits in half to protect capital from the collapsing Profit Factor.

### 2. Execution Decay ("Fee Bleed")
In sideways, momentum-less markets (Neutral classifications), the lack of volatility causes transaction costs to consume an outsized portion of trader edge.
* In volatile "Fear" regimes, exchange fees consume roughly **~2.1%** of gross PnL.
* In "Neutral" regimes, this fee bleed spikes to **~5.5%** of gross PnL. 
* **Actionable Rule:** Algorithms must widen acceptable spread limits and decrease trade frequency thresholds during "Neutral" days to prevent execution decay.

---

## Bonus: Unsupervised ML (Behavioral Archetypes)
Using K-Means Clustering on the aggregated account metrics, the trader base mathematically separated into three distinct profiles. Analyzing how they trade during sentiment extremes reveals "Smart Money" vs "Dumb Money" positioning:

1. **Whales (Market Makers):** High volume, high frequency, ~88% win rate. They maintain a rigid ~50/50 Long/Short bias across *all* regimes. They do not predict direction; they strictly harvest volatility.
2. **Retail Grinders:** Low volume, high frequency. Highly susceptible to market euphoria. During "Extreme Greed," they pivot to an aggressive **73% Long Bias** (trend chasing at the top).
3. **High-Conviction Swing Traders:** Highest volume, lowest frequency (~61% win rate). They aggressively fade the retail crowd, shifting to an **85% Short Bias** during "Extreme Greed" tops.

**Final Strategic Takeaway:** The Primetrade desk should dynamically adjust internal copy-trade parameters to *fade* Retail Grinders during Extreme Greed, and *mimic* Swing Trader short signals when the market is euphoric.

---
## Reproducibility
1. Clone this repository.
2. Install dependencies: `pip install pandas numpy matplotlib seaborn scikit-learn`
3. Ensure `bitcoin_sentiment.csv` and `historical_data.csv` are in the root directory.
4. Execute `analysis.ipynb` cell-by-cell. Output plots and CSVs will generate in the `/outputs` folder.