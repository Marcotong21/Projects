# Projects
**Record some of the projects I've done** :smiley:

(These are papers I've backtested and reproduced. There are some other papers that I have merely read in the 中文版(Chinese version) folder.)

## Statistical Arbitrage Strategy Based on PCA and Mean Regression (Ongoing)
**2024.2 -** 

from the paper "Statistical Arbitrage Strategy Based on PCA and Mean Regression" (2008)<br>

## Light Refraction Strategies at Integer Prices
**2024.1, Personal Research**

Imporved from the paper "MEASURING THE PRESSURE OF PRICES-INTEGER VALUES, OVER THE STOCK TREND" (2010)<br>
1. Constructed and Replicated a light refraction strategy that predicts the direction and slope of a stock’s move as it crosses an integer price. The classification of refraction and reflection is determined by the expected attitude of the market.
2. Improved the strategy of the original paper based on my own thinking by adding some optimization features, including regression fitting and stock price normalization. Backtesting on China's SSE index, finally obtained a better preformance than the original strategy.

## Timing Strategy Based on Relative Strength of Resistance Support (RSRS)
**2023.10 - 2023.11, Personal Research** 

From two research reports from China Everbright Securities:<br>
"20170501-光大证券-择时系列报告之一：基于阻力支撑相对强度（RSRS）的市场择时" and "20191117-光大证券-技术指标系列报告之六：RSRS择时_回顾与改进"

The daily high and low price is a kind of resistance and support level, it is the day all market participants of the trading behavior recognized resistance and support. Changes in the day's high and low prices can quickly reflect the recent changes in market attitudes towards resistance and support levels. Based on this principle, the strategy steps are as follows:

1. Linear regression of the highest and lowest prices of the index for the last n days, and we obtain two slopes $beta_{high}$ and $beta_{low}$
2. $beta = beta_{high} / beta_{low}$, which reflects the change in market attitudes over the past n days. The higher the beta, the more positive attitudes

This is the basic idea of the strategy. This strategy also includes a number of optimizations, including z-score standard scores, goodness-of-fit screening, right-biased standard scores, and so on.

## Market Making Strategy for Crypto Market
**2023.10 - 2023.12, course project of Fin 556 at UIUC**

The original project goes here: https://gitlab.engr.illinois.edu/fin556_algo_market_micro_fall_2023/fin556_algo_fall_2023_group_05/group_05_project
