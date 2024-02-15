# 20170501-光大证券-择时系列报告之一：基于阻力支撑相对强度（RSRS）的市场择时
这篇写的很好，且原理通俗好懂，很值得一读
## 主要原理
支撑位即是指标的价格在下跌时可能遇到的支撑，是交易者认为买方力量开始反超卖方使得价格在此止跌或反弹上涨的价位；阻力位则是指在标的价格上涨时可能遇到的压力，是交易者认为卖方力量开始反超买方而使得价格难以继续上涨或就此回调下跌的价位。

包括均线策略、布林带策略在内，这些策略都是讲支撑位与阻力位视为定值。其优点是在大趋势中能获得很好的收益，缺点是在震荡行情中会出现滞后性。

现将阻力位与支撑位视为一个变量。阻力位与支撑位实质上反应了交易者对目前市场状态顶底的一种预期判断。从直觉上看，如果这种预期判断极易改变，则表明支撑位或阻力位的强度小，有效性弱；而如果众多交易者预期较为一致、变动不大，则表明支撑位或阻力位强度高，有效性强。

如果支撑位的强度小，作用弱于阻力位，则表明市场参与者对于支撑位的分歧大于对于阻力位的分歧，市场接下来更倾向于向熊市转变。而如果支撑位的强度大，作用强于阻力位，则表示市场参与者对于支撑位的认可度更高于对于阻力位的认可度，市场更倾向于在牛市转变。   **（why？）**

每日的最高价与最低价就是一种阻力位与支撑位，它是当日全体市场参与者的交易行为所认可的阻力与支撑。**（好精彩的想法！）**这里我们并非用支撑位与阻力位作突破或反转交易的阈值，而是更关注市场参与者们对于阻力位与支撑位的定位一致性。当日最高价与最低价能迅速反应近期市场对于阻力位与支撑位态度的性质，是我们使用最高价与最低价的最重要原因。

我们用相对位置变化强度来描述阻力支撑相对强度。考虑对近N个数据点限定回归来得到信噪比较高的相对变化强度，得到：high = alpha + beta*low + epsilon， epsilon ~ N(0,sigma)

这里beta就是需要的斜率。其中 N 的取法不能太小，不然不能过滤掉足够多的噪音；但也不能太大，因为我们希望得到的是体现目前市场的支撑阻力相对强度，若取值太大，则滞后性太高。

当斜率值很大时->最高价变动更加迅速->支撑强度强于阻力强度。
![1696905268320](https://github.com/Marcotong21/Quant/assets/125079176/4139d367-ab1d-4175-9a79-0ce70315b9db)
上图分别代表继续走强的上涨牛市，以及下跌势头渐止的下跌熊市。（最低价变动缓慢->支撑点更加稳固->上涨趋势）
![1696905569444](https://github.com/Marcotong21/Quant/assets/125079176/9cccb37b-41b6-449f-85d3-6aadc16b7b48)
这两张图则相反，代表有下跌倾向的牛熊市。<br>
总的来说，斜率beta越大，越有上涨趋势; beta越小，越有下跌趋势。

## 构建RSRS指标
斜率指标与标准分指标，比较后发现后者效果更好（标准分指标增加了两个自由度，周期M和阈值S）。标准分指标策略如下：<br>
1.取前M日（例如M=600）的序列，计算当日的标准分z<br>
2.对近N日（例如N=18）的标准分z线性回归，得到当日的斜率标准分<br>
3.如果斜率标准分大于S（例如S=0.7）则买入，小于-S则卖出<br>
观察到无论是斜率指标还是标准分指标都在09年至14年的震荡区间净值较稳定增长，这正是RSRS 指标优秀左侧预测能力的体现。<br>
回测结果：
![image](https://github.com/Marcotong21/Quant/assets/125079176/dfff2be0-e3f7-4c80-91db-638dcc39d620)

## 敏感性测试
当数据个数N选用为20时，策略效果有一个明显的下跌。可能的原因是：20是许多技术指标（如均线计算）的默认常用参数，因此当大量交易者按照以20 这个周期计算的技术指标进行交易，就会影响到 20 这个周期的最高价最低价数据信息分布。

## 优化
1.研究标准分分布对未来市场收益的预测性。得到结果是，右侧标准分（z>0）与收益率有尚可的正相关性，是较好的牛市预测指标。而在左侧几乎失去任何预测性。<br>
![1697423002183](https://github.com/Marcotong21/Quant/assets/125079176/92610a00-b89f-4921-895f-aa9f7b19f5a6)
2.**改进：**
我们将标准分值与线性拟合的R方值相乘得到修正的标准分。以此降低绝对值很大，但实际拟合效果很差的标准分对策略的影响。修正的标准分在偏度上有明显的向正态修正的效果。研究修正标准分与未来市场收益的预测性，发现整体相关性得到了改善，尤其是左侧标准分的表现。如右图所示。<br>
![1697423009605](https://github.com/Marcotong21/Quant/assets/125079176/2465dbfd-41ce-475c-a725-e00146a5e451)
观察指标值域的改变，一个大胆的想法是，是否右侧数据值域越广，其对未来收益率的预测越好？是否左侧数据值域越窄越好？能否通过改变标准分的分布来达到改善整体指标预测性的目的？<br>
3.**再次改进：**
右偏标准分=修正标准分x斜率 (斜率本身几乎一定是正值，故无偏分布x右偏分布为右偏）<br>
得到相关性有小幅增强。<br>
![1697423287625](https://github.com/Marcotong21/Quant/assets/125079176/69252267-be17-4d78-9b23-074d36e6d64f)
对比三个指标的收益率：
![1697423405818](https://github.com/Marcotong21/Quant/assets/125079176/cd7ddceb-2ef9-42a5-a412-387d7be9f9b6)
理论上，收益率应该是标准分<修正标准分<右偏标准分。然而实际上修正标准分的收益反而小于标准分。<br>
原因是，在股票中一般只考虑做多策略，因此左侧标准分的预测能力对于择时策略没有很大的帮助。修正标准分对于预测能力的提升主要体现在左侧，在右侧的预测能力反而下降了一些。因此其收益也就下降了一些。<br>
回测结果：
![image](https://github.com/Marcotong21/Quant/assets/125079176/538693ec-8ae4-4fb9-b247-b1711e4c4dc9)


总的来说，在这一节我们对标准分进行了改进，其效果小幅提高。然而，熊市开仓信号误判或过早左侧开仓造成巨大回撤，这个缺点依然没有解决。

## 量价数据优化
### 基于市场趋势的优化
我们尝试在开仓时加入一个对目前市场状态的判断，过滤掉下跌行情中的开仓。<br>
价格优化交易策略：<br>
1.计算标准分指标买卖信号<br>
2.若发出买入信号，且同时满足MA(20)大于前三日的MA(20)的值(向上趋势)，则买入。<br>
优化后的策略表现显著提高，成功避免了在大熊市的惨重损失。<br>
![1697503301247](https://github.com/Marcotong21/Quant/assets/125079176/f603bb6b-04c5-4713-86c0-75dba93ccf07)
虽然右偏标准分的夏普比率不如前两个，但它较小的交易次数使得成本较低，在考虑成本的情况下相对收益更高。

### 基于交易量的优化
我们尝试用交易量与修正标准分之间的相关性来过滤误判的信号。我们认为相关行为正的信号才是好的信号。<br>
交易量优化交易策略：<br>
1。计算标准分指标买卖信号<br>
2.如果发出买入信号，同时前10日交易量与修正标准分的相关性为正，则买入<br
优化的策略过滤了更多的交易，也避开了熊市市场。<br>
![1697503801804](https://github.com/Marcotong21/Quant/assets/125079176/82b06f33-716c-4a5f-b2df-a95bec545021)
此时的右偏策略有最大的胜利与平均盈亏比，但回撤很大且夏普不高。<br>
回测结果：
![image](https://github.com/Marcotong21/Quant/assets/125079176/08becdba-a356-46e6-9dcf-c30ac4ca80ca)
![image](https://github.com/Marcotong21/Quant/assets/125079176/6fef5fef-92fc-434d-8a1f-293ed1816f51)
![image](https://github.com/Marcotong21/Quant/assets/125079176/297b5981-9e06-4e83-b1f8-6182685f22ba)
三张图依次代表标准分、修正标准分、右偏标准分的回测结果，蓝色代表原策略，红色代表经过MA(20)优化后的策略。优化后的策略收益确实有了明显的提升，也很好的避开了2008年的熊市。

总的来说，量价数据使得策略表现有所提高，其中标准分策略表现最好。<br>
对周期M进行敏感性分析，发现M在超过600之后表现开始递减。可能的原因是太久远的数据有不一样的市场基本面，以及M太大的话前M天就没有足够的数据来计算标准分。策略在前期由于时间窗长度不同，标准分会波动很大，对策略产生影响。
## 结论
RSRS指标对于市场未来收益的择时效果显著，有较强预判市场顶底的能力。。在大牛市中能抓住大部分的趋势获得巨额收益。在震荡市场中也能有效抓取小上涨，避开小慢跌，从而在震荡中缓慢稳定盈利。
![1697516589312](https://github.com/Marcotong21/Quant/assets/125079176/342afd26-c43e-4602-bfc7-39a1f5cf0242)