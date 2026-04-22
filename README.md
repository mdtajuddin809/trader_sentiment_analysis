# Trader Performance vs Market Sentiment
**Primetrade.ai — Data Science Intern Assignment**

## Setup
```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

## How to Run
1. Place `historical_data.csv` and `fear_greed_index.csv` in the same folder as the notebook
2. Run: `jupyter notebook trader_sentiment_analysis.ipynb`
3. Execute all cells top to bottom

## Datasets
| File | Rows | Description |
|------|------|-------------|
| historical_data.csv | 211,224 | Hyperliquid trades (2023–2025) |
| fear_greed_index.csv | 2,644 | Daily Bitcoin Fear/Greed index |

After merging by date: **184,263 matched trade records**

---

## Methodology
1. **Data Cleaning:** No missing values found. Timestamps converted from milliseconds epoch to date. Both datasets aligned at daily granularity.
2. **Metrics Created:** Daily PnL per account, win rate, trade count, avg size, long/short ratio.
3. **Segmentation:** Traders classified into Winner/Mixed/Loser (by win rate) and Low/Mid/High (by trade frequency).
4. **Sentiment Mapping:** 5-class F&G index collapsed to 3 groups — Fear, Neutral, Greed.
5. **Bonus:** KMeans clustering (k=4) on win_rate, trade_count, avg_size, total_pnl.

---

## Key Insights

### Insight 1 — Fear Days Dominate Volume But Greed Days Reward Winners More
- **73% of all trades** occur on Fear days, yet average daily PnL on Fear days ($209K) >> Greed ($91K) — driven by sheer volume.
- Win rate is highest on Fear days (41.6%) vs Greed (36.9%), suggesting fear-driven dips offer better entry points.

### Insight 2 — Low-Frequency Traders Are the Most Efficient
- Low-frequency traders earn **$139/trade on Fear days** and **$395/trade on Greed days**.
- High-frequency traders earn only $25/trade on Fear days — more trades ≠ better returns.

### Insight 3 — "Winner" Segment Outperforms on All Sentiments Except Fear
- On Greed days, Winners earn $101/trade avg vs Losers at $64 — consistent skill amplified in bullish conditions.
- Surprisingly, on Fear days, Winners and Losers earn nearly identically (~$47), suggesting fear erases skill edges.

---

## Strategy Recommendations

### Rule 1 — Fear Days: Low-frequency traders should hold their strategy
> On Fear days (F&G < 40): Low-frequency traders should **not reduce activity**. Their selectivity pays off. High-frequency traders should **reduce trade count by 30–40%** to avoid noise-driven losses.

### Rule 2 — Greed Days: Winners can size up; Mid-frequency traders must use stop-losses
> On Greed days (F&G > 60): Consistent winners (win_rate > 55%) can **increase position size by ~20%**. Mid-frequency traders go negative on average — apply strict stop-losses as overconfidence bias peaks.

---

## Output Charts
| File | Description |
|------|-------------|
| chart1_pnl_dist.png | Daily PnL distribution by sentiment |
| chart2_behavior.png | Trade count, size, long ratio by sentiment |
| chart3_heatmap.png | PnL heatmap: Winner segment × Sentiment |
| chart4_winrate.png | Win rate by sentiment |
| chart5_freq_heatmap.png | PnL heatmap: Frequency segment × Sentiment |
| chart6_clusters.png | KMeans trader archetypes |
