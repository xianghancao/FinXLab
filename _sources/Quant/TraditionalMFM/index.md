#多因子模型

[TOC]

Version: 2.0 Oct, 2019



## 资本资产定价模型CAPM

Reference:

- Black, F., M. C. Jensen, and M. Scholes (1972). The Capital Asset Pricing Model: Some empirical Tests. In *Michael Jensen, Ed., Studies in the Theory of Capital Markets*, Praeger, New York NY.

## 多因子模型MFM

APT (Arbitrage Pricing Theory) model believes that arbitrage behaviour is a decisive factor in the formation of modern effective market (namely market equilibrium price state). If the market does not reach equilibrium state, there will be risk-free arbitrage opportunities in the market, and arbitrage behaviour will make the market return to equilibrium state.

APT（Arbitrage Pricing Theory）模型认为，套利行为是现代有效市场(即市场均衡价格)形成的一个决定因素，如果市场未达到均衡状态的话，市场上就会存在无风险套利机会，套利行为会使得市场重新回到均衡状态。

According to Wikipedia, "In [mathematical finance](https://en.wikipedia.org/wiki/Mathematical_finance), **multiple factor models** are [asset pricing](https://en.wikipedia.org/wiki/Valuation_(finance)) models that can be used to estimate the [discount rate](https://en.wikipedia.org/wiki/Discounted_cash_flow) for the valuation of financial assets. They are generally extensions of the single-factor [capital asset pricing model](https://en.wikipedia.org/wiki/Capital_asset_pricing_model)(CAPM)".

The multi-factor model quantifies the linear relationship between the expected stock return rate and the stock's factor load (risk exposure) on each factor, as well as the factor return rate per unit factor load (risk exposure) on each factor. For stock j:

多因子模型定量刻画了股票预期收益率与股票在每个因子上的因子载荷(风险敞口)，以及每个因子每单位因子载荷(风险敞口)的因子收益率之间的线性关系。对于股票$j$:
$$
\widetilde{r}_j = \sum^k_{k=1}X_{jk}*\widetilde{f}_k+\widetilde{u}_j\\
X_{jk}:股票j在因子k上的因子暴露（因子载荷）\\
\widetilde{f}_k:因子k的因子收益\\
\widetilde{u}_j：股票j的残差收益率
$$
The essence of the multi-factor model is to convert the return - risk prediction for N stocks into the return - risk prediction for K factors, and convert the covariance matrix for estimating the return rate of a single stock into the covariance matrix for estimating the return rate of factors.

多因子模型的本质是将对N只股票的收益-风险预测转变为对于K个因子的收益-风险预测，将估算个股收益率的协方差矩阵转换为估算因子收益率的协方差阵。

假设一个投资组合由N个股票组成，它们在组合中的权重分别是$h_{P1},h_{P2},\cdots,h_{PN}$，则组合的收益率为
$$
\widetilde{r}_P = \sum^k_{k=1}X_{Pk}*\widetilde{f}_k+\sum_{j=1}^Nh_{Pj}*\widetilde{u}_j\\
其中，X_{Pk}=\sum_{j=1}^Nh_{Pj}*X_{jK}
$$

### 历史上有哪些主流的多因子模型？

依据简约法则，学术界主流的多因子模型包括以下几个（按时间顺序、排名不分先后，不完整 list），它们的因子个数均在 3 到 5 个之间：

- **Fama-French 三因子模型（Fama and French 1993）：**多因子模型的开山鼻祖，包括 MKT，HML 以及 SMB 三因子。
- **Carhart 四因子模型（Carhart 1997）：**在 Fama-French 三因子模型上加上了动量 MOM 因子。
- **Novy-Marx 四因子模型（Novy-Marx 2013）：**包含 MKT，HML，MOM 以及 PMU 四个因子；其中 PMU 所用的财务指标是 Gross Profit-to-Asset，代表 profitability 维度。
- **Fama-French 五因子模型（Fama and French 2015）：**Fama 和 French 在其三因子模型的基础上加入了 CMA 和 RMW 两个因子，分别代表 investment 和 profitability 两个维度。
- **Hou-Xue-Zhang 四因子模型（Hou, Xue and Zhang 2015）：**包含 MKT，SMB，IVA 以及 ROE；其中 IVA 是 total assets 的年增长率，代表 investment 维度。
- **Stambaugh-Yuan 四因子模型（Stambaugh and Yuan 2016）：**包含 MKT，SMB，MGMT 和 PERF 四个因子。MGMT 和 PERF 分别使用了 6 个和 5 个指标，代表和 management 以及 performance 相关的两个 mispricing 因子。虽然该模型只有四个因子，但它用到的基本面和量价指标多达 12 个。
- **Daniel-Hirshleifer-Sun 三因子模型（Daniel, Hirshleifer and Sun 2018）：**在 MKT 的基础上，使用 PEAD 和 FIN 两个指标作为短期和长期行为因子（behavioral factors）的代理指标，构建了三因子模型。该模型由于包括了传统的 MKT 风险因子，又包括行为因子，故称为复合模型。

### 如何比较不同的多因子模型？



https://zhuanlan.zhihu.com/p/56614427



### 多因子模型的风险

对于一个包含N只股票和K个因子的系统，多因子模型本质上是将对于N只股票的收益-风险预测转变成了对于K个因子的收益—风险预测。
$$
\begin{bmatrix}
{\widetilde{r_1}}\\
{\widetilde{r_2}}\\
{\vdots}\\
{\widetilde{r_N}}\end{bmatrix}=
\begin{bmatrix}
{X_{11}}&{X_{12}}&{\cdots}&{X_{1k}}\\
{X_{21}}&{X_{22}}&{\cdots}&{X_{2k}}\\
{\vdots}&{\vdots}&{\ddots}&{\vdots}\\
X_{N1}&X_{N2}&{\dots}&X_{NK}\end{bmatrix}
\begin{bmatrix}
{\widetilde{f_1}}\\
{\widetilde{f_2}}\\
{\vdots}\\
{\widetilde{f_K}}\end{bmatrix}+
\begin{bmatrix}
{\widetilde{u_1}}\\
{\widetilde{u_2}}\\
{\vdots}\\
{\widetilde{u_N}}\end{bmatrix}
$$
多因子模型极大的降低了预测工作量，以一个1000只股票和20个因子组成系统而言，预测从1000只股票的预期收益和风险转换为对20个因子的预期收益和风险的预测。随着预测复杂程度的降低，预测的精度大幅提升。特别是对于风险的预测，前面已经提到过，若对1000只股票估计协方差矩阵，我们需要预测𝑁 ∗ (𝑁 − 1)/2 = 4950个相关系数。协方差矩阵中包含的独立参数太多，如果采用历史数据的样本方差和协方差，估计值既不稳定也不合理。因为采用历史数据进行估计，采样时间长度为𝑇，要求𝑇 > 𝑁(即𝑇 > 1000)。按照多因子模型最常规的月度频率，需要的数据超过80年，这显然不现实，同时也不合理，因为公司基本面数据是在不断发生变化的。

#### 多因子模型风险分解

1. 市场风险(Market Risk)： 所有的股票都会受到市场整体供需的影响而呈现出同涨同
   跌的现象，即我们所说的牛市和熊市。这是所有类别的风险中波及面最广，影响最大的
   风险;
2. 行业风险(Sector Risk):从事相同或者相似业务公司的股票，由于受到共同的产业
   景气周期影响、或者共同的产业政策冲击、抑或是其他宏观环境的影响，在市场上也会
   表现出较高的相关性;
3. 风格风险(Style Risk):风格风险是指剔除掉市场风险和行业风险之后，股票市场的
   结构表现在一定的时期内会呈现出很强烈的风格特征，比如小市值股票表现更优的小市
   值风格，前期收益低的股票近期收益更高的反转风格，成长性高的股票表现更好地成长
   风格，或者是低估值股票表现更好地低估值风格等等。主要的风格因子暂时分成十二大类:估值因子(Value Factor)、成长因子(Growth Factor)、财务质量因子(Financial Quality Factor)、杠杆因子(Leverage Factor)、规模因子(SizeFactor)、动量因子(Momentum Factor)、波动率因子(Volatility Factor)、换手率因子(Turnover Factor)、改进的动量因子(Modified Momentum Factor)、分析师情绪因子(Sentiment Factor)、股东因子(Shareholder Factor)和技术因子(Technical Factor)。

### 什么是Alpha?

Alpha用来衡量一个在风险调整下由投资所产生的"主动收益"（超过市场收益）。如今，任何投资者都可以通过购买指数型基金实现与整体市场相近的回报，而且只需要支付非常低的手续费，同时投资者无需做投资分析和市场研究。

[Richard Tortoriello] 认为一个略微超越市场甚至与之具有相同回报率的投资策略是没有投资价值的。此外，由于统计检验结果会随测试时间段的变化而产生比较显著的误差率，所以一个在少于60%的时间段内略微战胜/输给市场的测试结果是非常值得怀疑的。

[Barra Handbook USE3]定义卓越的投资表现为

- 收益预测:形成合理并且有效的收益预期;
- 风险控制:在谨慎的前提下捕捉市场机会;
- 过程控制:监控整个投资过程以保持投资生产上的一致性;
- 成本控制:避免过度或者无效率的交易侵蚀投资利润。

![image-20190614154226138](/Users/Hans/Library/Application%20Support/typora-user-images/image-20190614154226138.png)

在MFM框架下，Alpha 和 Beta 是相辅相成的，分别是使用线性回归将组合收益率分解为与业绩基准相关的部分和业绩基准不相关的残差部分。如果$r_P(t)$是投资组合在时点𝑡 = 1, 2, 3, ⋯ , 𝑇上的超额收益率，$r_B(t)$是业绩基准在同时期的超额收益率，那么回归模型为:
$$
r_p(t) = \alpha_P+\beta_P*r_B(t)+\varepsilon_P(t)
$$
利用回归分析计算出来的$\beta_P$和$\alpha_P$的估计值称为实现的或者历史的 Beta 和 Alpha。组合P的残差收益率是： 
$$
\theta_P(t) = \alpha_P + \varepsilon_P(t)
$$
$\alpha_P$是平均残差收益率，$\varepsilon_P(t)$残差收益率中均值为零的随机项。

根据定义，业绩基准组合的残差收益率总是等于零，即𝜃𝐵 = 0总是成立。因此，业绩基准组合的 Alpha 必然等于零，即𝛼𝐵 = 0。为了保证𝛼𝐵 = 0，我们要求股票层面的 Alpha 列向量满足业绩基准中性的约束。

随着不断发现新的风险因子，投资组合收益中越来越多的部分被划分为 ![公式](https://www.zhihu.com/equation?tex=%5Cbeta) 部分；相应的 ![公式](https://www.zhihu.com/equation?tex=%5Calpha) 部分就越来越少。如下图所示，自风险因子流行以来的 30 年里，投资组合（或者个股）的收益率的很大一部分已经被市场 ![公式](https://www.zhihu.com/equation?tex=%5Cbeta) （包括国家和行业）、风格因子 ![公式](https://www.zhihu.com/equation?tex=%5Cbeta) （包括价值因子、质量因子、动量因子等）、以及策略因子 ![公式](https://www.zhihu.com/equation?tex=%5Cbeta) （即通过有效配置风格因子）来解释。

![img](https://pic1.zhimg.com/80/v2-0a02cd709e86e44f1da90c3f0c4289e0_hd.jpg)



### 多因子模型的构建

### 因子的准备

#### 数据采集

基础数据采集:首先需要确定原始因子集合，然后按照原始因子集合逐个进行因子
原始数据的采集和计算工作;

估值因子(Value Factor)、成长因子(Growth Factor)、财务质量因子(Financial Quality Factor)、杠杆因子(Leverage Factor)、规模因子(Size Factor)、动量因子(Momentum Factor)、波动率因子(Volatility Factor)、换手率因子(Turnover Factor)、改进的动量因子(Modified Momentum Factor)、分析师情绪因子(Sentiment Factor)、股东因子(Shareholder Factor)和技术因子(Technical Factor)

#### 数据标准化

首先将因子载荷原始值转换为排序值，然后再进行标准化

#### 有效因子

仅纳入行业因子，而将市场因子包含在行业因子中。
$$
\widetilde{r}_j^t = \sum^s_{s=1}X_{js}^t*\widetilde{f_s^t}+X_{jk}^t*\widetilde{f_k^t}+\widetilde{u_j^t}\\
\widetilde{r}_j^t:股票j在第t期的收益率\\
X_{js}^t:股票j在第t期在行业s上的暴露\\
\widetilde{f_s^t}:行业s在第t期的收益率\\
X_{jk}^t:股票j在第t期在因子k上的暴露\\
\widetilde{f_k^t}:因子k在第t期的收益率\\
\widetilde{u_j^t}:股票j的残差收益率
$$
$X{js}^t$ 是一个0 − 1哑变量，即如果股票$j$属于行业$s$，则暴露度为1，否则为0。在有的模型中，会对公司所属行业进行拆分，比如公司𝑗的业务50%属于行业𝑎，30%属于行业𝑏，20%属于行业𝑐，则股票𝑗在行业𝑎的暴露度为0.5，在行业𝑏的暴露度为0.3，在行业𝑐的暴露度为0.2。

#### 因子收益率序列$t$检验

$\widetilde{f_s^t}$是因子k在第t期的因子收益，为确定因子k在第t期是否和股票收益率显著相关，即$\widetilde{f_s^t}$是否显著不等于0，我们需要对$\widetilde{f_s^t}$进行t检验：
$$
t=\frac{\overline{x}-u}{\sigma/\sqrt{n-1}}\\
t:x的t统计量\\
\overline{x}:样本均值\\
u:总体的均值\\
\sigma:样本的标准差\\
n:样本的容量
$$
$t$值绝对值序列的均值：对于每一期的截面回归，都可以得到一个因子收益率$\widetilde{f_k^t}$的t值。对于t值序列，首先取绝对值，然后计算$|t|$的均值，$|t|$是判断因子是否为有效因子的重要指标。

和收益率存在很明显相关性的因子，满足前面$t$的第一点和第二点。根据第三点，将有效因子分成收益类因子和风险类因子。

- 收益类因子:即因子收益率$\widetilde{f_k^t}$ 序列的$t$值显著不等于0，因子收益率的方向性相对明确，这类型的因子，用历史序列对下一期的因子收益进行预测时，相对比较准确，所以称之为收益类因子。
- 风险类因子:即因子收益率$\widetilde{f_k^t}$ 序列的$t$值在0附近，因子收益率的方向性相对不明确，这类型的因子，用历史序列对下一期的因子收益进行预测时，风险比较大，所以称之为风险类因子。收益类因子是多因子模型超额收益的主要来源，在模型中是需要风险暴露相对多的因子。而风险类因子也需要重点关注，因为风险类因子是进行风险控制的重点，需要风险暴露尽量少。

#### 因子IC值

在实际计算中，因子𝑘的 IC 值一般是指个股第𝑇期在因子𝑘上的暴露度与𝑇 + 1期的收益率的 相关系数。因子 IC 值反映的是个股下期收益率和本期因子暴露度的线性相关程度，是使用 该因子进行收益率预测的稳健性;而回归法中计算出的因子收益率本质上是一个斜率，反映 的是从该因子可能获得的收益的大小，这并不能代表任何关于稳健性的信息。 举个例子，票池里5只个股第𝑇期在动量因子上的暴露度为−2、−1、0、1、2，假设它们第𝑇 + 1 期收益率为−0.2、−0.1、0、0.1、0.2，则因子 IC 值为1，因子收益率为0.1;假设它们第𝑇 + 1期收 益率为−0.4、−0.2、0、0.2、0.4，则因子 IC 值为1，因子收益率为0.2。而因子𝑡值某种程度 上反映的也是稳健性信息，在上述举例的两种简单情形下，因子𝑡值都是正无穷。但是在更 复杂的包括其它因子和行业哑变量的多元线性回归模型中，因子𝑡值和 IC 的关系也随之变得 复杂，无法用确定的公式表示，只能说它们之间具有某种正相关关系。在我们的后续多因子 报告中会有更为详细的数学推导论述，欢迎继续关注。
 在利用 IC 值评价因子有效性时，可以预先对因子进行提纯，排除行业、市值等重要因素的 影响，使结果更明晰。具体来说，就是在因子标准化处理之后，在每个截面期上用其做因变 量对市值因子及行业因子等做线性回归，取残差作为因子值的一个替代，这种做法可以消除 因子在行业、板块、市值等方面的偏离。例如，股息率因子较高的个股可能较多分布在电力 及公用事业、汽车、商贸零售等行业以及大市值板块，经过因子提纯之后，股息率因子较高 的个股就会平均分布在各行业及板块了。
 当得到各因子 IC 值序列后，我们可以仿照上一小节𝑡检验的分析方法进行计算: 

1. IC 值序列的均值及绝对值均值:判断因子有效性;
2. IC 值序列的标准差:判断因子稳定性;
3. IC 值序列大于零(或小于零)的占比:判断因子效果的一致性。 

#### 因子打分法回测

依照因子值对股票进行打分，构建投资组合回测，是最直观的衡量指标优劣的手段。具体来 说，在某个截面期上，可以根据一个或几个因子值对个股进行打分，将所有个股依照分数进 行排序，然后分为𝑁个投资组合，进行回测。
构建方法详细说明如下: 

1. 股票池、回溯区间、截面期(换仓期)可均与回归法相同;
2. 选取一个基准组合(比如沪深300)，将所有个股在各个行业内按照得分进行排序， 每个行业内按得分从高到低分成𝑁个组合，每个行业内的每个组合中股票按流通市值配 比，然后将各行业的𝑁个组合中序数相同的组合结合在一起(最后一共形成𝑁个组合)， 组合内行业间权重按沪深300配比。以上这种构造方法得到的𝑁个组合为行业中性组合。 也可以选择不做行业中性，直接在全股票池中不分行业按得分高低分成𝑁个组合，每个 组合中的股票等权配比或按流通市值配比。
3. 评价方法:回测年化收益率、年化波动率、夏普比率、最大回撤、胜率(分时间、分 行业胜率)等。 

一般来说，对于比较有效的因子(如市净率)，分成3~5层进行回测，各个投资组合的最终 净值一般可以保序。分成𝑁层(𝑁 > 5)进行回测时，可以用最终净值的秩相关系数来衡量 因子的优劣(秩相关系数的绝对值越接近1时效果越好)。 

#### 共线性的问题

多因子模型强调因子本身的经济含义和实证有效性两个方面。在因子搜集的时候就会根据因
子的具体经济含义对因子进行大类划分，但是同类型的因子可能存在较强的相关性，多元线
性回归的时候会造成多重共线性(Multicollinearity)，多重共线性是指回归模型中的解释变
量之间由于存在精确相关关系或高度相关性而使模型估计失真或者难以估计准确。
所以在有效因子筛选出来之后，我们首先需要根据大类对因子的相关性进行𝑡检验，对于相
关性较高的因子，要么舍弃显著性较低的因子，要么进行因子合成。

step 1：同类型因子的相关性检验

同类型的K候选因子，向前选取M个月的数据作为样本：

1. 按月计算出因子载荷之间的相关系数矩阵和每个因子的因子收益率；
   $$
   \rho^t=
   \begin{bmatrix}
   {1}&{\rho_{12}^t}&{\cdots}&{\rho_{1k}^t}\\
   {\rho_{21}^t}&{1}&{\cdots}&{{\cdots}}\\
   {\vdots}&{\vdots}&{\ddots}&{\vdots}\\
   \rho_{K1}^t&\dots&{\dots}&1\end{bmatrix}
   $$

   $$
   f^t =
   \begin{bmatrix}
   {\widetilde{f_1^t}}\\
   {\widetilde{f_2^t}}\\
   {\vdots}\\
   {\widetilde{f_K^t}}\end{bmatrix}
   $$

   

2. 然后根据𝑀个月的相关系数进行检验，检验的方法包括相关系数绝对值的均值、中位数、𝑡检验等方式。

$$
\sum^M_{t=1}|\rho_{ij}^t|/M\\
median(|\rho_{ij}^t|), t=1,2,\cdots,M\\
t=\frac{\overline{|\rho^t_{ij}|}-u}{\sigma/\sqrt{M-1}}
$$

step2: 筛选因子

对于相关性较高的因子集合，采取两种方式处理：

1. 因子本身的有效性进行排序，挑选最有效的因子进行保留，删除其他因子；
2. 合成并保留有效因子信息；
   - 等权重法
   - 收益率加权法
   - 信息系数IC加权法

#### 残差异方差分析

线性回归模型的一个重要假定：总体回归函数中的随机误差项满足同方差性，否则线性回归模型存在异方差性。
$$
\widetilde{r_j}=\sum_{K=1}^KX_{jK}*\widetilde{f_K}+\widetilde{u_j}
$$
如果残差项的条件方差相同，即$Var(\widetilde{u_j}|X_{j1},X_{j2},\cdots,X_{jK})=\sigma^2$，则称为同方差性;
如果残差项的条件方差不相同，即$Var(\widetilde{u_j}|X_{j1},X_{j2},\cdots,X_{jK})=\sigma_j^2$，则称为异方差性。

异方差性对普通最小二乘法(Ordinary Least Square，OLS)估计的影响，主要有三点: 

1. 回归系数的 OLS 估计量仍然是无偏的、一致的、并且不影响𝑅2和调整的𝑅2。
2. 回归标准差的估计不再是无偏的，从而回归系数 OLS 估计量的方差不再是无偏的， OLS 估计量不再是有效的和渐近有效的。
3. 𝑡统计量不服从𝑡分布，𝐹统计量也不服从𝐹分布，从而无法进行假设检验和区间估计， 也无法进行区间预测。 

对于模型是否存在异方差的检验，可以采用 Breusch-Pagan test 或者 White test 两种方法

#### 多元线性回归

做完异方差分析处理之后，就可以正式进行多元线性回归，估计每期因子的收益序列。
$$
\widetilde{r}_j^t = \sum^s_{s=1}X_{js}*\widetilde{f_s^t}+X_{jk}^t*\widetilde{f_k^t}+\widetilde{u_j^t}\\
\widetilde{r}_j^t:股票j在第t期的收益率\\
X_{js}^t:股票j在第t期在行业s上的暴露\\
\widetilde{f_s^t}:行业s在第t期的收益率\\
X_{jk}^t:股票j在第t期在因子k上的暴露\\
\widetilde{f_k^t}:因子k在第t期的收益率\\
\widetilde{u_j^t}:股票j的残差收益率
$$
经典回归模型的基本假设:

1. 参数的线性性:回归模型对于参数而言是线性的;
2. 样本的随机性:样本是从总体中随机抽样得到的;
3. 不存在完全共线性:每个解释变量具有一定变异并且自变量之间不存在完全的线性相
   关关系;
4. 零条件均值:$E({u_j}|X_{j1},X_{j2},\cdots,X_{jK})=\sigma^2$;
5. 同方差性:$Var({u_j}|X_{j1},X_{j2},\cdots,X_{jK})=\sigma^2$;
6. 正态性:$u_j$独立于所有变量，并且$u_j\sim{}N(0,\sigma^2)$。



#### 估计因子预期收益率

多元线性回归，我们可以得到所有因子的历史收益率序列（$\widetilde{f_k^t}, k=1,2,3,\cdots,K$;$t=1,2,3,\cdots,T$):
$$
\rho^t=
\begin{bmatrix}
{\widetilde{f_1^1}}&{\widetilde{f_2^1}}&{\cdots}&{\widetilde{f_K^1}}\\
{\widetilde{f_1^2}}&{\widetilde{f_2^2}}&{\cdots}&{{\cdots}}\\
{\vdots}&{\vdots}&{\ddots}&{\vdots}\\
{\widetilde{f_1^T}}&\dots&{\dots}&{\widetilde{f_K^T}}\end{bmatrix}
$$
对于$T+1$期因子的预期收益率的估计，有以下几种方法：

1. 历史均值法：用前𝑁期因子历史收益率的均值作为𝑇 + 1期因子的预期收益率， 
   $$
   \widetilde{f_K^{T+1}} = \frac{\sum^T_{t=T-N+1}f_K^t}{N}
   $$
   一般情况下，𝑁的取值为36或者60，即前36个月或者60个月的均值;

2. 时间序列预测方法：

   AR(q)，自回归模型(Auto-regress, AR): 变量在时刻$t$的取值$x(t)$依赖于变量$q$个历史取值${x(t-1),x(t-2),\cdots,x(t-q)}$的加总和以及一个随机输入项$e(t)$:
   $$
   x(t) = a_0+a_1*x(t-1)+\cdots+a_q*x(t-q)+e(t)
   $$
   𝑀𝐴(𝑝)，移动平均模型(MovingAverage，MA):变量在时刻𝑡的取值等于𝑝+1个 (独立的)随机输入𝑒(𝑡), 𝑒(𝑡 − 1), ⋯ , 𝑒(𝑡 − 𝑝)的加权平均之和: 
   $$
   x(t) = e(t)+c_1*e(t-1)+\cdots+c_p*e(t-p)+c_0
   $$
    𝐴𝑅𝑀𝐴(𝑞, 𝑝)，自回归移动平均模型(Auto-Regress Moving Average，ARMA): 是𝐴𝑅(𝑞)和𝑀𝐴(𝑝)的组合。 
   $$
   x(t) = a_0 + a_1*(t-1)+\cdots+a_q*x(t-q)\\+e(t)+c_1*e(t-1)+\cdots+c_p*e(t-p)
   $$
   自回归积分滑动平均模型(Autoregressive Integrated Moving Average Model, ARIMA)，是 ARMA 模型在时间序列一阶差分上的应用; 

   

#### 计算股票预期收益

估算出T+1期的因子收益率向量($\widetilde{f_1^{T+1}}, \widetilde{f_2^{T+1}},\cdots,\widetilde{f_K^{T+1}}$)后，以及计算出$T+1$期的因子载荷矩阵：
$$
X^{T+1}=
\begin{bmatrix}
{{X_{11}^{T+1}}}&{{X_{12}^{T+1}}}&{\cdots}&{{X_{1K}^{T+1}}}\\
{{X_{21}^{T+1}}}&{{X_{22}^{T+1}}}&{\cdots}&{{\cdots}}\\
{\vdots}&{\vdots}&{\ddots}&{\vdots}\\
{{X_{N1}^{T+1}}}&\dots&{\dots}&{{X_{NK}^{T+1}}}\end{bmatrix}
$$
接下来可以计算出:
$$
\widetilde{r_j^{T+1}}=\sum_{K=1}^K{X_{jk}^{T+1}}*\widetilde{f_K^{T+1}}
$$
计算出T+1期每只股票的预期收益率向量($\widetilde{r_1^{T+1}}, \widetilde{r_2^{T+1}},\cdots,\widetilde{r_N^{T+1}}$).



#### 多因子模型的风险分解 

多因子风险模型的主要观点是，股票的收益率可以被一组共同因子和一个仅与该股票有关的 特异因子解释，即任何股票的收益率来自两个方面:共同(因子)部分，特异部分。多因子 模型通过对于共同(因子)部分的定量建模，将投资的聚焦点从股票转移至因子，即从原来 的对股票的收益和风险管理，变成对于因子的收益和风险管理。 

从组合的角度而言，如果设置有业绩基准，那么组合收益率可以分成: 

1. 业绩基准收益率; 
2. 主动收益率(主动超额收益率)。 

主动收益率可以进一步细分成: 

1. 因子主动收益率(共同部分); 
2. 特定主动收益率(特异部分)。 

因子主动收益率可以再细分成: 

1. 市场因子收益率; 
2. 行业因子收益率; 
3. 风格因子收益率。 

在多因子模型中，股票对于市场因子的因子暴露统一为1，对于行业因子的因子暴露是一个 0 − 1哑变量(如果股票属于某一行业为1，否则为0)。由于多因子模型本质上是一个统计套 利模型，不适合对市场因子和行业因子进行收益预测和风险管理。

因此目前国内市场上多因 子模型最流行的用法是: 

1. 通过股指期货对冲组合的市场风险(市值对冲);
2. 通过行业中性对冲组合的行业风险(以业绩基准的行业权重为基准进行对齐，即组合
   在每个行业上的权重分配与业绩基准一致)。

### 投资组合风险预测

## 优化模型

### 二次规划

第𝑇 + 1期的股票预期收益、因子收益协方差矩阵、预期残差风险，都计算出来之后，关于
股票的预期风险和收益的基础数据就全部得到了。接下来需要做的就是在这些数据的基础上，
结合投资组合的风险-收益目标，以及各种约束条件，进行股票选择和权重分配。

对于投资组合的优化问题，一般可以采用二次规划的方法构建符合目标的投资组合。
$$
\min_H\frac{1}{2}*H^T*Q*H+H^T*c\\
s.t.A^T*H\le{b}\\
H:目标向量\\
Q:最优化问题的二次项系数的对称半正定矩阵\\
c:线性目标方程有关的系数向量\\
A:为约束等式与非等式的系数矩阵\\
b:约束值的向量矩阵
$$
二次与线性最优化的问题都可以通过一般二次规划最优化程序来解决。对于线性最优化问题，
只要令𝑄 = 0，则问题就变成一个线性规划问题。对于二次最优化而言，要使用恰当的𝑄。

### 收益目标和风险目标

站在投资者的角度，都是希望收益越高越好，风险越低越好。但是投资的收益和风险是两个 矛盾的目标，无法同时满足。实际操作中有两种形式: 

1. 将预期风险控制在一定水平之下，选择投资组合使得期望收益最大; 

2. 在预期收益不低于某一特定水平的条件下，选择投资组合使得预期风险最小。 

要求解的目标是投资组合𝑃的权重向量h𝑃 ，组合的预期收益率;
$$
\widetilde{r_P^{T+1}} = \sum^N_{j=1}{r_j^{T+1}}*h_j
$$
组合的预期风险为：
$$
\sigma^2=x_P^T*F*x_P+h_P^T*\Delta*h_P=h_P^T*V*h_P
$$
股票权重总和为1:
$$
\sum_{j=1}^Nh_j=1,h_j\ge0,j=1,2,\cdots,N
$$


控制风险，最大化收益:
$$
\max_{h_j}\sum^N_{j=1}{r_j^{T+1}}*h_j\\
s.t. h_P^T*V*h_P\le\sigma^2\\
$$
保证收益，最小化风险:
$$
\min_{h_p}h_P^T*V*h_P\\
s.t.\sum_{j=1}^N\widetilde{r_j^{T+1}}*h_j\ge{r}
$$


个股上下限约束：
$$
\sum_{j=1}^Nh_{PAj}=0,h_j^{upper}\ge{h_{PAj}}\ge-h_{Bj},j=1,2,\cdots,N
$$
对于任意股票，其行业哑变量的值要么为0，要么为1，所有股票的哑变量矩阵S：
$$
行业哑变量矩阵S =
\begin{bmatrix}
{S_{11}}&{S_{12}}&{\cdots}&{S_{1S}}\\
{S_{21}}&{S_{22}}&{\cdots}&{\cdots}\\
{\vdots}&{\vdots}&{\ddots}&{\vdots}\\
{S_{N1}}&\dots&{\dots}&{S_{NS}}\end{bmatrix}
$$
行业权重约束：
$$
\sum_{j=1}^Nh_{PAj}*S_{ji}=0,j=1,2,\cdots,N
$$
因子k暴露上线为$x_k$：
$$
|\sum^N_{j=1}{h_{PA}}*X_{jk}|\le{x_K}
$$



## Reference

Richard Tortoriello, Quantitative Strategies for Achieving Alpha, Charpter 1

investopedia.com, https://www.investopedia.com/terms/m/multifactor-model.asp

Joseph Cernigli, Frank Fabozzi, Academic, Practitioner, and Investor Perspectives on Factor Investing, 􏱦􏱺􏱻􏲍2018􏲎 

股票多因子模型的回归检验, https://zhuanlan.zhihu.com/p/40984029

 