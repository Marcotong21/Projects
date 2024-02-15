# Market Making Strategy for Crypto Market
- [Market Making Strategy for Crypto Market](#market-making-strategy-for-crypto-market)
  - [People](#people)
  - [Introduction](#introduction)
    - [Background](#background)
    - [Project Description](#project-description)
  - [Technologies](#technologies)
    - [Programming Languages](#programming-languages)
    - [Packages](#packages)
    - [Strategy Studio](#strategy-studio)
  - [Data](#data)
    - [Crypto Data](#crypto-data)
    - [Data Formatting](#data-formatting)
  - [Strategy Implementation](#strategy-implementation)
    - [Version 0 Strategy](#version-0-strategy)
      - [Basic Ideas](#basic-ideas)
      - [Strategy Steps](#strategy-steps)
      - [Explanations](#explanations)
      - [Backtesting 0](#backtesting-0)
    - [Version 1 Strategy](#version-1-strategy)
      - [Basic Ideas 1](#basic-ideas-1)
      - [Strategy Steps 1](#strategy-steps-1)
      - [Backtesting 1](#backtesting-1)
  - [Analysis and Summaries](#analysis-and-summaries)
  - [Reflections](#reflections)
    - [Vishesh Prasad](#vishesh-prasad)
    - [Zhicheng Tang](#zhicheng-tang)
    - [Yibang (Marco) Tong](#yibang-marco-tong)
    - [Harsh Agarwal](#harsh-agarwal)
  - [References](#references)

## People
**Vishesh Prasad (Team Leader)**<br>
Hi! I am Vishesh Prasad and I am currently a junior studying Computer Engineering at the University of Illinois at Urbana-Champaign with minors in 
Math, Statistics, and Econometrics. I am a U.S. citizen, graduating in May 2025, and am interested in quantitative trading and quantitative research roles. I have experience working on statistical anlysis, research, and machine learning with a passion for financial markets. I am proficient in languages such as C++, Python, and others and have experience thorugh previous roles as an Undergraduate Research Assistant and a Software Developer Intern.
You can reach me through email or [Linkedin](https://www.linkedin.com/in/visheshprasad/)

**Zhicheng Tang**<br>
I am Zhicheng Tang,  currently pursuing a Master's degree in Industrial Engineering at the University of Illinois at Urbana-Champaign. My university study covers math, statistics, computer science, economics and finance. I have practical experiences in data analysis, quantitative research, signal generating, machine learning, and arbitrage strategy development. I am proficient in multiple programming languages such as Python, C++, R, SQL and MATLAB. Presently, I am gaining hands-on experiences as a quant developer intern at a California-based hedge fund. Prior to this, I have had the opportunity to work with leading tech companies in China including Huawei, Baidu and Tencent. 

I am seeking opporunities for May 2024 graduation. Feel free to reach out to me through email or [LinkedIn](https://www.linkedin.com/in/zhichengtang/)

**Yibang (Marco) Tong**<br>
I am Yibang Tong, a junior student at uiuc and major in Mathematics. My estimated graduate time is Dec 2024, and currently seeking internship opportunities for quant trader/researcher. Linkedin profile: [LinkedIn](https://www.linkedin.com/in/yibang-tong/)

**Harsh Agarwal**<br>
I am a senior majoring in Computer Science. I am passionate about software development, particularly developing low latency systems for use in finaicial markets. I am well versed with core software development tools including data structures, algorithms, system design, distributed systems, and computer architecture. You can find me here on LinkedIn - https://www.linkedin.com/in/harshagl2002/

## Introduction

### Background
Market makers are entities that facilitate trading in financial markets by providing liquidity. They play an essential role in helping ensure that the traded volume is large enough for trades to be completed seamlessly. Market makers earn money by facilitating a high volume of trades and exploiting the bid-ask spread. They sell shares at a marginally higher price than what they bought them for, and with enough volume of these trades, they can make significant income. With the high volume that market makers trade at and by placing orders on the book (orders not executed immediately), market makers are also rewarded with maker fees (rebates) for their participation (maker and taker fees are highly dependent on the exchange and the market-making firm). For market makers to be successful, they are highly reliant on being the fastest to get market updates from the exchange and the quickest to provide orders to the exchange. Since markets are very efficient if market makers are not the fastest in the market, they will get killed (if we are putting it lightly). This is hugely more complicated in crypto markets (which we will be focusing on for our project) since crypto markets are primarily based on the cloud, where there are not many technological advancements a firm can invest in that can provide an edge over firms in terms of speed. Because of this interesting situation, we decided to implement a crypto market-making strategy to understand better how market-making strategies work with crypto data.

### Project Description
In this project, our group will implement an active market-making strategy in the crypto market. We have data from a Level 2 book and use Python to format it to meet Strategy Studio's requirements. Since the data was extensive, we first connected to the professor's virtual machine to backtest. Then, we used Strategy Studio's interface to write our basic market-making strategy in C++. Finally, we analyze the results of the backtesting and suggest potential improvements. <br>
Here is the outline of our strategy, Version 0 strategy:
![Outline](Final_Report/Outline.png)
## Technologies
### Programming Languages
- Python
    - We used Python for basic data analysis, format conversion, and to analyze the results of the Strategy Studio output
- C++
    - Strategy Studio provides the api interface in C++ and allows us to implement various strategies in C++
### Packages
- Strategy Studio Includes
    - Strategy Studio contains libraries defined in its software for developing strategies.
- Other Packages
    - numpy
    - pandas
    - matplotlib
    - plotly
    - datetime
    - argparse
    - gzip

### Strategy Studio
We are very thankful to RCM for sponsoring us to use Strategy Studio for this class. we can input our market data and strategy to Strategy Studio and it will give us the PnL result of our strategy, as well as the details of the order filling.
## Data
### Crypto Data
Thanks to one of our team members, Zhicheng, we acquired Binance crypto data from the company where he is interning. This made our task a lot easier. We chose Bitcoin's data because it is the best-known and most liquid of the cryptocurrencies. In our data, we decided to use incremental_book_L2, which has Level 2 order book data for every order book price level update. The Level 2 order book allows us to construct the total Bitcoin order book at every price level and provides us with every price level book update, allowing us to implement a more well-thought-out strategy and make the backtest more accurate. Our Level 2 order book data is given in single-day batches, so the first few thousand entries in our original data have the is_snapshot flag set to TRUE (is_snapshot = TRUE if that update is filling the order book with the previous day's order book state). The rest of the columns in the data are self explanatory. A screenshot of the L2 book：
![L2 book](Final_Report/L2_screenshot.png)

Most of our original data is stored in Docker, and we need to manually download it and manually upload it to the Professor's Server, which will take a lot of time. Therefore, we chose a small range of data to run backtesting first in this project. We decided on the initial range 2023-08-08 to 2023-08-21, which is a very volatile period. The time to upload and reformat these two weeks of data took several days. If we can handle this extreme period successfully, we have reason to believe that our strategy can also work well in other common periods.

### Data Formatting
To be able to backtest on Strategy Studio using the data we collected, we had to format it using Strategy Studio's requirements for the input data. We wrote Python scripts to reformat our data into the required formatting for Depth Update by Price (OrderBook data) to accomplish this. This reformatting correctly formatted the data with collection time, source time, side, price, size, and other optional and non-optional fields. The exact specifics of this formatting requirement are omitted for proprietary reasons.
## Strategy Implementation
### Version 0 Strategy
#### Basic Ideas
As a market maker, our bread and butter is by exploiting the spread between buy and sell orders. Ideally, both our buy and sell orders are executed, and we make a profit. However, the reality of the market is that it changes rapidly, and a quote on one side of the market may not be executed. This creates an inventory position that can be risky for market makers. Therefore, we hope to adjust the quote scale and quantity to control inventory position risk while making markets. Note that we would prefer to wait to have these orders executed immediately and have them rest on the book to add liquidity to the market and gain maker fees instead of reducing the liquidity by executing immediately and being hit with taker fees. <br>
In our project we decided to implement an active market making strategy, which is providing quotes to provide liquidity to the market at every moment in time.<br>
We did multiple basic analysis to the order book data, include spread, imbalance, liquidity, etc. Finally we made the decision that our quoting strategy would be primarily based on BBO's spread. We select the data of 2019-11-19 as an example to extract and analyze the spread distribution of BBO from book_ticker.csv:
![BBO_spread_example](Final_Report/BBO_spread.png)
The spread has been kind of large for a significant portion of this time, which is probably due to the fact that Bitcoin is not yet very liquid in 2019.
#### Strategy Steps
1. Set fixed basic spread $`S = x \cdot \tau`$, where $`\tau`$ is the tick size and x is a parameter
2. Bid and ask price
   1. Let best ask price be $`P_{ask^*}`$, best bid price be $`P_{bid^*}`$ and mid price be $`P_{mid}`$.
   2. Let $`\tau`$ denote the tick size, $`I_{\%}`$ denote the percentage of max inventory and $`c`$ is a scaling factor. Inventory adjustment $`\delta = I_{\%} \cdot c \cdot \tau`$.
   3. The bid price we submit is $`P_{bid} = min(P_{mid} - \dfrac{S}{2} - \delta, P_{ask^*}-\epsilon)`$.
   4. The ask price we submit is $`P_{ask} = max(P_{mid} + \dfrac{S}{2} - \delta, P_{bid^*}+\epsilon)`$.
3. Order quantity = fixed base quantity $`y`$% of the maximum position + quantity that makes net open quantity plus inventory equals 0
4. Handle Orders: use GTD orders to set a lifetime of open orders.
5. Inventory risk: when $`I_{\%}`$ greater than or smaller than a threshold, we use take orders to gradually reduce it.
#### Explanations
1. In this version we simply set a fixed spread S, and this S is the spread at which we profit. We can try different S by changing the parameter x to test how well it works. We can also try dynamic S in later version.
2. The basic idea of bid and ask price is really simple: <br>
$$bid\_price = mid\_price - \frac{S}{2}$$ <br>
$$ask\_price = mid\_price + \frac{S}{2}$$ <br>
After this, we made two improvements：
    1. Inventory Adjustment. How inventory position is handled is a very important issue and we want to minimize the risk by changing the quotes. Assume our current inventory position is $`I_{\%}`$ (I is positive in this example), then we want to move prices of our next quotes down to try to close the inventory position. By moving prices down, we will have a more competitive sell quote, and a less competitive buy quote. In this case our buy quote is more likely to be executed, which is a useful way to minimize the inventory position. And if our inventory position is negative, we will move our prices up, to have a more competitive buy quote. Our adjustment term is $`\delta = I_{\%} \cdot c \cdot \tau`$
![Example of quotes](Final_Report/quotes.jpg)
2. Non-crossover Contrain. Our buy quotes should not exceed the best_ask price, and our sell quotes should not be less then the best_bid price. If this happens, we will change the quote to $`P_{ask^*}-\epsilon`$ or $`P_{bid^*}+\epsilon`$.
3. The logic of our order quantity is also similar to our bid/ask prices. We also set a basic quantity, and add an adjustment according to inventory. The goal is trying to make the inventory position close to 0 at every time.
4. GTD(Good Till Date) orders are widely used in trading, especially in crypto markets where we have perpentual contracts. Here we want to use GTD orders to deal with unexecuted quotes. Image in one moment, only one side of our quotes is executed, then we will have inventory position as well as an unexecuted quote. We have developed some adjustment strategies to deal with inventory positions, but have not considered unexecuted quotes. The first thought was just leave the quote there. But image when Bitcoin just rises rapidly, and at each moment our buy quotes will not be executed. In this case we will have a huge amount of unexecuted quotes left, which is definitely an unexpected uncertainty. So we decided to GTD orders, withdrawal unexecuted orders to eliminate the uncertainty directly.
5. Although we have some adjustments to minimize inventory position, there are still special circumstances reamin. Just like the example in 4, when the price rises rapidly in a straight line, our buy quotes might never be filled. Since our adjustments to our quotes were relatively minor (fluctuating only around the BBO), we might not be able to follow the fast move of the market. Then we will have a large negative inventory position. So we decided to set a threshold, and close our position manually when the inventory exceeds this threshold. This might cause some loss in a short time, but this is acceptable compared to the risk of extremely large inventory position.
#### Backtesting 0
Return: 335298595117.53107

![Version 0 PnL](Final_Report/version_0_all_dates.png)

Divide the period into sections to see it a little more clearly:
![Aug 10 - Aug 14](Final_Report/backtesting0-1.png)
![Aug 15 - Aug 17](Final_Report/backtesting0-2.png)
![Aug 18 - Aug 21](Final_Report/backtesting0-3.png)

It looks like our strategy wasn't too bad for Aug 10 - Aug 14 and Aug 18 - Aug 21. However, our strategy had a huge pullback in the Aug 15 - Aug 17 timeframe. A large part of this is due to the sudden market crash on August 17th, which caused us to accumulate a large inventory position, which in turn caused a large loss when we liquidated the position. Therefore our strategy does not handle extreme market conditions well and other improvements are needed.

### Version 1 Strategy
#### Basic Ideas 1
We implemented two improvements to our strategy:
1. One point we noticed is that our BASIC SPREAD is fixed, which cannot be resized according to the real recent market conditions. Therefore, we consider setting a dynamic spread, which should be closely related to the recent market conditions.<br>
First, select the order book data of the previous day and divide it into 24 groups according to hours. For each group, compute the mean value of the spread in that interval. <br>
Then for today's trade, our basic spread for each hour will refer to the average spread of the previous day's hour. For example, yesterday's average spread from 0:00 to 1:00 was 0.004, so we set today's basic spread from 0:00 to 1:00 to be 0.004.<br>
We originally wanted to use the average spread of the previous full day as a basis, but the crypto market can be very different in terms of activity during the day and night. Therefore, it would be more appropriate to use the hourly spread as a reference.
2. Another thing is that the quantity of our quotes can also be adjusted slightly according to the probability of risk. If our basic spread is large, we have a larger possible profit and a larger risk of not being executed. So we can consider reducing the quantity of quotes to minimize the risk when the basic spread is large. So the basic quantity y% can become like this:
$`y_{1}\% = \dfrac{C}{S_{1}}`$, where C is a constant and S is basic spread. So here quantity is inversely related to basic spread. Constant C here is actually the expected percentage of profit on each market making, as $`C = y_{1}\% \cdot S_{1} `$= quantity * spread. Note that this is an adjustment to the basic quantity, and the following adjustment will still need to be made according to the inventory position as in Version 0.
#### Strategy Steps 1
1. Dynamic spread $`S_{1} = `$ average spread of this hour a day ago.
2. Bid and ask price
   1. Let best ask price be $`P_{ask^*}`$, best bid price be $`P_{bid^*}`$ and mid price be $`P_{mid}`$.
   2. Let $`\tau`$ denote the tick size, $`I_{\%}`$ denote the percentage of max inventory and $`c`$ is a scaling factor. Inventory adjustment $`\delta = I_{\%} \cdot c \cdot \tau`$.
   3. The bid price we submit is $`P_{bid} = min(P_{mid} - \dfrac{S}{2} - \delta, P_{ask^*}-\epsilon)`$.
   4. The ask price we submit is $`P_{ask} = max(P_{mid} + \dfrac{S}{2} - \delta, P_{bid^*}+\epsilon)`$.
3. Order quantity = base quantity $`y_{1}`$% of the maximum position + quantity that makes net open quantity plus inventory equals 0, and here $`y_{1}\% = \dfrac{C}{S_{1}}`$
4. Handle Orders: use GTD orders to set a lifetime of open orders.
5. Inventory risk: when $`I_{\%}`$ greater than or smaller than a threshold, we use take orders to gradually reduce it.
And the outline is shown below:
![Version 1.0 Outline](Final_Report/Outline_1.0.png)
The purple boxes represent the improvements made in this Version.
#### Backtesting 1
Return: -228653118456172.44

![Aug 10 - Aug 17](Final_Report/backtesting1-1.png)
![Aug 18 - Aug 21](Final_Report/backtesting1-2.png)

We believe that the backtesting of the second half of our strategy has been problematic, so we are leaving it out for now. If we only compare the results of the backtesting of the first third of the strategy (Aug 10 - Aug 14), we will see that Version 1 does show some improvement over Version 0.

## Analysis and Summaries
We will do some analysis to our backtesting results to measure the effectiveness of the strategy. Here are some indices that are widely used to measure a strategy:  <br>
**Sharpe Ratio** <br>
$$Sharpe Ratio = \frac{actual\ or\ expected\ return\ of\ portfolio−risk\ free\ rate}{standard\ deviation\ of\ the\ portfolio's\ excess\ return}$$ <br>
**Sortino Ratio** <br>
$$Sortino Ratio = \frac{actual\ or\ expected\ return\ of\ portfolio - risk\ free\ rate}{standard\ deviation\ of\ the\ downside}$$ <br>
Although Sharpe Ratio is the most widely used return/risk metric, it penalizes upside volatility (which means that large gains in certain data points can lower the Sharpe Ratio). And Sortino Ratio is a similar metric to the Sharpe Ratio, but it only penalizes downside volatility. Therefore, we can use these two indicators to better evaluate our strategy.

**Maximum Drawdown** <br>
$$Max\ drawdown = \frac{Peak Value - Trough Value}{Peak Value}$$ 

The analysis of our strategies is:

|Version 0 Strategy| Value |
|----|-------|
|Return | 335298595117.53107|
|Sharpe Ratio | 1.7177644939591924e-05|
|Sortino Ratio | 2.288810881510311e-05|
|Maximum Drawdown | -126466689285510.72|
|Start Time of Maximum Drawdown | 2023-Aug-10 05:56:06.635000|
|End Time of Maximum Drawdown | 2023-Aug-15 14:39:39.230000|
|Number of orders filled | 6300002|
|Number of orders canceled| 35157|
|Percentage of orders canceled | 0.00558047441889701|
|The total transaction fee | 5221036834.175667|

|Version 1 Strategy| Value |
|----|-------|
|Return| -228653118456172.44|
|Sharpe Ratio| -0.009132476779958244|
|Sortino Ratio| -0.008650292969139514|
|Maximum Drawdown:| -228658283261934.1|
|Start Time of Maximum Drawdown| 2023-Aug-09 08:12:14.342000|
|End Time of Maximum Drawdown | 2023-Aug-21 10:09:22.692000|
|Number of orders filled | 6300002|
|Number of orders canceled | 30437|
|Percentage of orders canceled | 0.0048312683075338704|
|The total transaction fee is | 30184856.76306799|

We can see that from the analysis of our version 0, we have a relatively low return relative to the risk taken (low Sharpe Ratio), and a low risk-adjusted return (low Sortino Ratio). We can also see that we have a relatively large number of orders filled and low percentage of orders canceled, which are good signs for market makers.  

From our analysis of our version 1, we have a suboptimal risk-adjusted performance (negative Sharpe Ratio), and a return that is not sufficient to cover the downside risk (negative Sortino Ratio). We can also see that we have a relatively large number of orders filled and low percentage of orders canceled, which are good signs for market makers. 


We also thought of some other potential improvements:
1. Splitting our quotes and changing to consecutive quotes. This is a very useful approach if we consider gaming in the market. For example, if our original buy quote was $10.05 with quantity 1, we can now change it to $10.07 with quantity 0.2, $10.06 with quantity 0.2, $10.05 with quantity 0.2, $10.04 with quantity 0.2, $10.03 with quantity 0.2. Split quotes are a very common method in real markets. This is because if one firm submits a larger quote, other rivals in the market will recognize the order and respond to it in turn. Therefore, each firm would like their own quotes not to be recognized by the other counterparties, in which case splitting to consecutive quotes is a very effective measure.
2. Constructing some signals to support decision making. Some potential signals include large spread, liquidity, volatility, orderbook imbalance, etc. We can use these signals to determine what the market has been doing recently and adjust our quoting strategy accordingly, like to be more aggressive or more conservative.
3. Using some other complex mathematical models. For example, the Avellaneda-Stoikov (AS) model is a famous and widely-used market making model. This model is designed to optimize bid and ask spreads dynamically by considering factors such as inventory levels, market volatility, and the trader's risk preferences. By adapting to market conditions in real time, the AS model helps in aligning the market making strategy with the prevailing market dynamics, thereby potentially improving profitability and managing risks more effectively. This is a good model if we want to learn more complex and practical things about market making.

## Reflections
### Vishesh Prasad
**1. What did you specifically do individually for this project?**

- I proposed the idea of doing market making on crypto data and set up the initial proposal and plan.
- I did the initial analysis on order book imbalance. 
- I wrote the inital script to convert the crypto data we have into the format required by Strategy Studio.
- I converted our Version 0 market making strategy idea into C++ code with Starategy Studio API integration so that it could be used by the backtester.
- I converted our Version 1 market making strategy idea into C++ code with Starategy Studio API integration so that it could be used by the backtester.
- I helped set up the scripts and makefile to run the backtest, and debug problems with our backtesting
- I backtested both strategy versions on our crypto data and obtained the analysis output
- I set up the outline for the final report and helped write and edit it.

**2. What did you learn as a result of doing your project?**

This project gave me my first experience with algorithmic trading and specifics about market making. I also got an introduction to the crypto market and how it differs from traditional stock markets. This project also taught me more about the strengths (ability to test strategies without having to build a backtesting machine myself) and weaknesses (inability to get dynamic fee structuring, inaccuracy in the latency data feeds, etc.) of backtesting used in the industry. This project also introduced me to centralized virtual machines, Yubikeys, and general scalable computing practices.

**3. If you had a time machine and could go back to the beginning, what would you have done differently?**

If I had a time machine, I would go back and get the problems with our data and virtual machine sorted sooner by focusing on setting everything up instead of splitting up the work. The issues we faced with our data and the inability to backtest correctly pushed our work timeline back and limited our bandwidth to explore further market-making strategies such as market book imbalance, momentum-based signals, and machine learning techniques.

**4. If you were to continue working on this project, what would you continue to do to improve it, how, and why?**

If I continue working on this project, I would first experiment around with different fee structures and latency settings in order to understand how those variables would change the backtesting output. This would improve the project by outputting a more accurate (but still not very accurate) backtesting result of the strategy. I would also explore more signals and compare a more complicated and computationally heavy strategy (using many basic and machine learning signals and predictors) to a simple strategy focusing on low latency coding. However, for this comparison, I would have to use a different backtesting software that isn't event-driven and instead simulates the actual market timing.

**5. What advice do you offer to future students taking this course and working on their semester long project (be sides “start earlier”… everyone ALWAYS says that). Providing detailed thoughtful advice to future students will be weighed heavily in evaluating your responses.**

For future students, I would highly recommend having an idea about what you want to learn the most about or areas that interest you the most. You don't need to have an idea to take the class (I didn't), but having the mindset to learn as much as you can will help you better understand how everything fits together and prepare you well for after the class. I also recommend learning as much as you can from your peers. There are brilliant people in this class, and just listening to the questions they ask will help you in numerous ways. Being attentive, curious, and willing to get your hands dirty and lose some sleep (mainly towards the end) will take you a long way, not just in this class but also in other endeavors you may take on.

### Zhicheng Tang
**1. What did you specifically do individually for this project?**

- I read paper [High-frequency trading in a limit order book](https://math.nyu.edu/~avellane/HighFrequencyTrading.pdf) to fish ideas for the strategy. 
- I wrote the second version of data conversion script that converted the compressed csv incremental order book data to the compressed txt format.
- I fetched the Binance sample data for backtesting and introduced the details of the data to other group members.
- I formulated the strategy and wrote the pseudo code in the [issue](https://gitlab.engr.illinois.edu/fin556_algo_market_micro_fall_2023/fin556_algo_fall_2023_group_05/group_05_project/-/issues/27).

​	**Other cooperative tasks**

- Finalized every detail of the market making strategy with Yibang.
- Finalized the data format and data conversion with Vishesh.
- Set up the backtesting environment with Vishesh and Harsh, including fetching and modifying make files and shell scripts from group1, modifying the config of strategy studio and examining the data format.

**2. What did you learn as a result of doing your project?**

It's my first time opportunity to develop a market making strategy, though it's a very simple one. We didn't implement a complex strategy due to lack of time, but in the process of designing strategies, I read lots of research papers and realized the most important part of such strategies is not how to predict the market, most of people focus on the inventory management. I also learned how limited the resources online.

**3. If you had a time machine and could go back to the beginning, what would you have done differently?**

I would ask professor to start the project earlier so that we have more time to set up the backtest environment. I would also try to convince my group members to select other types of strategies, instead of such a difficult one. Market making is facinating and big companies use it to dominate the exchange. However, there is are reasons they could achieve that with such strategies.

**4. If you were to continue working on this project, what would you continue to do to improve it, how, and why?**

I will spend more time on improving the strategy because we spent too much time on the backtesting and didn't have enough time to improve it. To be honest, try to develop such kind of strategy using online public resources is difficult. Those academic papers, at most cases,  are trying to apply complex mathematical models to manage the inventory and the router of the orders. A better idea is to seek advice from someone in the indsutry and start with a more practial model.

**5. What advice do you offer to future students taking this course and working on their semester long project (be sides “start earlier”… everyone ALWAYS says that). Providing detailed thoughtful advice to future students will be weighed heavily in evaluating your responses.**

- Be more active in asking questions about setting up strategy studio.
- Spend more time at the beginning to choose a relatively easier strategy then improve it iteratively. Also, schedule meetings with professor to discuss the strategy.
- When fishing the ideas, try to start with twitter, podcast, forum and blogs, instead of diving into research papers. The followings are some useful resources: 
  - https://www.thinknewfound.com/
  - https://www.elitetrader.com/et/
  - Reddit
  - https://www.wealth-lab.com/Strategy
  - @bennpeifert
- If you have no idea of algo trade, read the book Inside the Black Box.

### Yibang (Marco) Tong
**1. What did you specifically do individually for this project?**

- I initiated basic analysis of the order book, including specefically BBO spread analysis.<br>
- I came up with the rough outline of the market making strategy at first, then dicussed with Zhicheng and finalized our Version 0 strategy. 
- I also thought of some potential improvements and wrote the version 1 strategy.<br>
- I helped roughly check the initial strategy code written by Vishesh. <br>
- I wrote the python script to analyze the backtesting result of Strategy Studio, e.g. Sharpe ratio, Max drawdown, Return, etc.<br>
- I wrote most of the final report.

**2. What did you learn as a result of doing your project?**

This is when I first got specific about market making strategies. Prior to this I only knew that market makers could provide liquidity to the market, but not the details of market making. After this project I gained experience with market making strategies and a better understanding of the order book structure.

**3. If you had a time machine and could go back to the beginning, what would you have done differently?**

I think we should have settled on the general direction of the project earlier. We never decided on the strategy we wanted to do at the beginning, and only re-decided to do a market making strategy after we wrote the project proposal. This wasted a lot of our time. So we should have decided on a direction earlier and stuck to it once we had decided.

**4. If you were to continue working on this project, what would you continue to do to improve it, how, and why?**

We have listed a number of potential strategy improvements in Summary, which are worthwhile points of improvement. Perhaps we could also test more hyperparameters to see how sensitive our strategy is to different parameters.

**5. What advice do you offer to future students taking this course and working on their semester long project (be sides “start earlier”… everyone ALWAYS says that). Providing detailed thoughtful advice to future students will be weighed heavily in evaluating your responses.**

It's a good thing to have a general idea of what you want to do with your project by mid-semester, e.g. what strategies to do? In which market? What spot will be traded? As well as other details, such as the need for backtesting hyperparameters? Backtesting latency?
And if your group wants to use your own data (e.g. crypto data), you should be very careful about the formatting; Strategy Studio has certain formatting requirements and you need to try to convert your data earlier.
One more thing, I personally think offline meetings will be more efficient than online meetings. So I highly recommend offline meetings in the second half of the semester, especially if the progress is a bit slow.

### Harsh Agarwal
**1. What did you specifically do individually for this project?**

- I looked at generating signals and indicators that could used in our market making strategy. 
- I developed a thorough analysis to highlight two major indicators which could be essential in developing the market making strategy. We however, did not end up using it. 
- I thoruoughly went over the strategy studio documentations to help convert our market making strategy to C++ using required classes and functions. 
- I helped in implementing our Version 0 market making strategy idea into C++ code with Starategy Studio API integration so that it could be used by the backtester.
- I designed the scripts and makefiles to run the backtest, and debug problems with our backtesting.
- I helped on the final report by writing and editing it.


**2. What did you learn as a result of doing your project?**

One great learning curve that I had was to actually have hands on experience with market making and algorithmic trading. I learnt the different dynamics involved in market making and how C++ software engineers actually impelement and test out strategies which is used by real world trading companies. The project was also unique in essence as it covered the crypto market. Coming in with no prior knowledge of the crypto market, I learnt a lot about how the exchanges work and how the data is first parsed to use it in building strategies

**3. If you had a time machine and could go back to the beginning, what would you have done differently?**

I would have first tried to understand the different dynamics associated with the crypto exhcnages and how the data is different from regular IEX data which strategy studio. This would have helped us save tremendous time which could then have been channeled into implementing additional improvements to the strategy

**4. If you were to continue working on this project, what would you continue to do to improve it, how, and why?**

I would contiue to improve the basics of our strategy to adjust for additonal parameters and indicators that I had intially generated. With these new variables, the strategy will be a stronger fit into predicting the movement of the market, thus helping generate higher profits. 

**5. What advice do you offer to future students taking this course and working on their semester long project (be sides “start earlier”… everyone ALWAYS says that). Providing detailed thoughtful advice to future students will be weighed heavily in evaluating your responses.**

One advice I would give to students is to not be intimidated by the breath of the project. It is very open ended which makes it challenging yet exciting. It is always important to be inquisitive and have a genuine knack for the subject. It is diffuclt to succeed in this class without being genuinely passionate about learning and implementing and researching. Also, please dont hesitate to ask questons!!


## References
https://math.nyu.edu/~avellane/HighFrequencyTrading.pdf
