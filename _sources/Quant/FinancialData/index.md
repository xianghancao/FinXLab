# Financial Data

## Data Types

| Fundamental Data | Market Data           | Analytics               | Alternative Data | Macro Data |
| ---------------- | --------------------- | ----------------------- | ---------------- | ---------- |
| assets           | price                 | analyst recommendations | satellite        |            |
| sales            | volume                | credit ratings          | CCTV/sensor      |            |
| liabilities      | yield                 | earnings expectations   | google searches  |            |
| costs/earnings   | implied volatility    | news sentiment          | twitter          |            |
|                  | dividend/ coupons     |                         |                  |            |
|                  | quotes/ cancellations |                         |                  |            |
|                  |                       |                         |                  |            |
|                  |                       |                         |                  |            |

### 基本面数据（Fundamental Data）

基本面数据主要是季度财报数据，这类数据发布时间比统计区间晚（比如第一季度财报可能到5月中旬才发布）。使用时要注意不能使用未发布的数据，即如果对1-3月做研究，就不能用5月发布的财报数据。很多论文用的财报数据常犯这样的错误，导致这些论文的结果无法复现。

基本面数据经常被回填（backfilled）和恢复（reinstated）：前者指数据中的缺失值会被赋值；后者指某个数据发布后可能会被多次修改，数据提供方往往简单地用新值覆盖原值，但显然这些新值在数据发布时还不存在。

基本面数据非常系统而低频，由于其极易被市场获取，数据中的价值有限，需要配合其他数据类型一起分析。



### 市场数据（Market Data）

市场数据包括所有在交易所发生的交易行为。理性情况下你可以拿到未经结构化处理的原始数据流，如 [FIX](https://link.zhihu.com/?target=https%3A//baike.baidu.com/item/fix/19660545) 数据、BWIC（bids wanted in competition）数据。每个市场参与者都会在市场中留下脚印，只要耐心分析，你就能预测对手的下一步行动。

与基本面数据不同，FIX 数据非常庞大，每天大概有 10TB 量级的数据产生，处理起来很不简单，这正是它吸引人的原因。



### 分析数据（Analytics）

分析数据是对其他数据处理后的衍生数据，一般来自投行、研究公司，还有些公司会卖对分析数据的统计，例如从新闻报道和社交平台上提取的情绪。分析数据的优点是它已经被提取好了，缺点是这些数据很贵、其计算逻辑可能有误，以及不止一个人使用这些数据。



### 替代数据（Alternative Data）

替代数据可产生于个人（社交媒体、网页搜索）、商业过程、传感器（卫星、定位、天气），比如卫星图像数据很受欢迎，包括对油罐、管道活动、停车场的监控图像。替代数据是“原始数据”（primary information），举个栗子，在埃克森美孚公司报告盈利增加、股价增长的几个月前，其油罐、钻井和管道布局定会有所变化。替代数据有两点不足：一是监控成本很高，二是可能会受到被监控公司的反对。

采用替代数据意味着要面对独特、难处理的数据，但越难存储、操作的数据越有价值，因为你的同行可能因为同样的原因放弃使用这些数据。



## Bars

为了将 ML 用于非结构化数据上，我们需要解析并以系统化的格式存储数据。存储数据的常见形式为表格，一般称表格的行为 "bars"，常用的 bar 方法有标准 bar 方法、信息驱动（information-driven） bar 方法两种，前者多见于学术期刊而后者更高级。

### Standard Bars

构建 Bar 是为了将非等间距产生的数据序列（inhomogeneous series）通过有规律采样变为同质数据（homogeneous series）。有些构建 Bar 的方法非常流行，以至几乎所有数据供应商的 API 都会提供它们。



1. Time Bars 
指的是以固定的时间区间对数据进行取样（如每分钟一次）后得到的数据。 尽管Time bars 是实践中最流行使用的处理方式，但这里我们指出它的两个不足：第一，市场交易信息的数量在时间上的分布并不是均匀的。开盘后的一小时内交易通常会比午休前的一小时活跃许多。因此，使用Time bars 会导致交易活跃的时间区间的欠采样，以及交易冷清的时间区间的过采样。第二，根据时间采样的序列通常呈现出较差的统计特征，包括序列相关、异方差等。 
2. Tick Bars 
指每隔固定的（如1000次）交易次数提取上述的变量信息。一些研究发现这一取样方法得到的数据更接近独立正态同分布 [Ane and Geman 2000]，从而能更好地进行统计推断。 使用Tick Bars 还需注意异常值 (outliers) 的处理。一些交易所会在开盘和收盘时进行集中竞价，在竞价结束后以统一价格进行撮合。 
3. Volume Bars & Dollar Bars 
Volume Bars 是指每隔固定的成交量提取上述的变量信息。Dollar Bars 则使用了成交额。 使用 Dollar Bars 相对而言是有一定优势的。假设一只股票在一定时间区间内股价翻倍，期初10000元可以购买的股票将会是期末10000元可购买股票手数的两倍。在股价有巨大波动的情况下，*Tick Bars*以及*Volume Bars*每天的数量都会随之有较大的波动。除此之外，增发、配股、回购等事件也会导致*Tick Bars*以及*Volume Bars*每天数量的波动



使用 Time Bar 会有缺点，相比之下，Volume Clock表现更好。使用固定 bar size 时，每日Dollar/Volume/Tick Bar 的数量如下图所示，可见 Dollar Bars 是更加稳健的方法。

![img](https://pic1.zhimg.com/80/v2-bc644231808bcc54b14ee761d2487168_hd.jpg)

### Information-Driven Bars

信息驱动 bar 的思路很简单：当市场中出现新信息时更频繁地采样。这里“信息”是市场微观结构中的概念（详见19章），比如我们会更重视信息优势者的行为。通过对信息优势者行为的等间距采样，我们就能在价格达到新的均衡水平前做出决策。

**1. Tick Imbalance Bars（TIBs）**

考虑量价序列 ![[公式]](https://www.zhihu.com/equation?tex=%5C%7B%28p_t%2C+v_t%29%5C%7D_%7Bt%3D1%2C+...%2C+T%7D) ，其中 ![[公式]](https://www.zhihu.com/equation?tex=p_t) 为价格序列， ![[公式]](https://www.zhihu.com/equation?tex=v_t+) 为交易量序列。（记号下同）

定义序列 ![[公式]](https://www.zhihu.com/equation?tex=%5C%7Bb_t%5C%7D) ：![[公式]](https://www.zhihu.com/equation?tex=b_t+%3D++%5Cbegin%7Bcases%7D+b_%7Bt-1%7D+%26+%5Ctext%7B+if+%7D+%5CDelta+p_t+%3D+0+%5C%5C+%5Cfrac%7B%5CDelta+p_t%7D%7B%7C%5CDelta+p_t%7C%7D+%26+%5Ctext%7B+if+%7D+%5CDelta+p_t+%5Cneq+0+%5Cend%7Bcases%7D) ， ![[公式]](https://www.zhihu.com/equation?tex=b_t+%5Cin+%5C%7B-1%2C+1%5C%7D)

TIBs 的思想是当 tick imbalance 超过期望值时采样一次，下面讨论如何确定采样时间间隔 ![[公式]](https://www.zhihu.com/equation?tex=T)

定义 ![[公式]](https://www.zhihu.com/equation?tex=T) 时刻的 tick imbalance 为 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta_T+%3D+%5Ctextstyle%5Csum_%7Bt%3D1%7D%5E%7BT%7D+b_t) ；初始时 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta_T) 的期望为![[公式]](https://www.zhihu.com/equation?tex=E_0%5B%5Ctheta_T%5D+%3D++E_0%5BT%5D%28P%5Bb_t%3D1%5D-P%5Bb_t+%3D+-1%5D%29+%3D+E_0%5BT%5D%282P%5Bb_t%3D1%5D-1%29+) ：其中 ![[公式]](https://www.zhihu.com/equation?tex=E_0%5BT%5D) 为 tick bar 长度的期望，实践中可以用 ![[公式]](https://www.zhihu.com/equation?tex=T) 的指数加权移动平均（Exponentially weighted moving average）来估计； ![[公式]](https://www.zhihu.com/equation?tex=P%5Bb_t%3D%5Cpm1%5D) 代表价格上涨 / 下跌的一笔交易， ![[公式]](https://www.zhihu.com/equation?tex=%282P%5Bb_t%3D1%5D-1%29) 可以用 ![[公式]](https://www.zhihu.com/equation?tex=b_t) 的指数加权移动平均来估计。

![[公式]](https://www.zhihu.com/equation?tex=T%5E%2A+%3D+%5Carg+%5Cmin_T+%5C%7B+%7C%5Ctheta_T%7C+%5Cgeq+E_0%5BT%5D+%5Ccdot+%7C+2P%5Bb_t%3D1%5D-1+%7C+%5C%7D) 为一个 TIB 间隔。当 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta_T) 偏离期望值——市场中有知情交易者触发大量单边交易时， ![[公式]](https://www.zhihu.com/equation?tex=T%5E%2A) 较小，采样更频繁。实际上，可认为相邻 TIB 之间包含一样多的信息量。



**2. Volume/Dollar Imbalance Bars（VIBs / DIBs）**

同样，VIBs / DIBs 的思想是当 Volume / Dollar 的不平衡度超过期望值时采样一次。定义 ![[公式]](https://www.zhihu.com/equation?tex=T) 时刻的不平衡度（imbalance）为 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta_T+%3D+%5Ctextstyle%5Csum_%7Bt%3D1%7D%5E%7BT%7D+b_t+v_t) ， ![[公式]](https://www.zhihu.com/equation?tex=v_t) 代表 Volume / Dollar 。接着我们计算初始时 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta_T) 的期望

![[公式]](https://www.zhihu.com/equation?tex=%5Cbegin%7Balign%7D+E_0%5B%5Ctheta_T%5D+%26+%3D+E_0%5Cleft%5B%5Csum_%7Bt%3D1%7D%5E%7BT%7D+v_t+%7B%5Cbf%7B1%7D%7D%5C%7Bb_t+%3D+1%5C%7D+%5Cright%5D+-+E_0%5Cleft%5B%5Csum_%7Bt%3D1%7D%5E%7BT%7D+v_t+%7B%5Cbf%7B1%7D%7D%5C%7Bb_t+%3D-+1%5C%7D+%5Cright%5D+%5C%5C+%26+%3D+E_0%5BT%5D%5Cleft%28P%5Bb_t+%3D+1%5DE_0%5Bv_t%7Cb_t+%3D+1%5D-+P%5Bb_t+%3D+-1%5DE_0%5Bv_t%7Cb_t+%3D+-1%5D++%5Cright%29+%5C%5C+%26+%3D+E_0%5BT%5D%28v%5E%2B-v%5E-%29%3D+E_0%5BT%5D%282v%5E%2B-E_0%5Bv_t%5D%29+%5Cend%7Balign%7D)

其中 ![[公式]](https://www.zhihu.com/equation?tex=v%5E%2B+%3D+P%5Bb_t+%3D+1%5DE_0%5Bv_t%7Cb_t+%3D+1%5D) ， ![[公式]](https://www.zhihu.com/equation?tex=v%5E-+%3D+P%5Bb_t+%3D+-1%5DE_0%5Bv_t%7Cb_t+%3D+-1%5D+) ，满足 ![[公式]](https://www.zhihu.com/equation?tex=v%5E%2B+%2B+v%5E-+%3D++E_0%5Bv_t%5D+)

实践中，![[公式]](https://www.zhihu.com/equation?tex=E_0%5BT%5D) 可以用 ![[公式]](https://www.zhihu.com/equation?tex=T) 的指数加权移动平均来估计；![[公式]](https://www.zhihu.com/equation?tex=%282v%5E%2B-E_0%5Bv_t%5D%29) 可以用 ![[公式]](https://www.zhihu.com/equation?tex=b_tv_t) 的指数加权移动平均来估计。

![[公式]](https://www.zhihu.com/equation?tex=T%5E%2A+%3D+%5Carg+%5Cmin_T+%5C%7B+%7C%5Ctheta_T%7C+%5Cgeq+E_0%5BT%5D+%5Ccdot+%7C+2v%5E%2B-E_0%5Bv_t%5D+%7C+%5C%7D) 为一个 VIB / DIB 间隔，同理当 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta_T) 偏离期望值时， ![[公式]](https://www.zhihu.com/equation?tex=T%5E%2A) 较小，采样更频繁

**3. Tick Runs Bars**

TIBs、VIBs、DIBs 用来监控下单的不平衡。庄家会掩饰其下单，如用冰山单（iceberg order）或将大单拆成若干小单，但这些行为都会留下脚印，因此，一个有效的做法是监控总交易量中的买单，当序列偏离期望值时采样。

首先定义 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta_T+%3D+%5Cmax+%5Cleft%5C%7B+%5Csum_%7Bt%3D1%7D%5ET+%7B%5Cbf+1%7D%5C%7Bb_t+%3D+1%5C%7D%2C+%5Csum_%7Bt%3D1%7D%5ET+%7B%5Cbf+1%7D%5C%7Bb_t+%3D+-1%5C%7D+%5Cright%5C%7D)

则 ![[公式]](https://www.zhihu.com/equation?tex=E_0%5B%5Ctheta_T%5D+%3D+E_0%5BT%5D+%5Ccdot+%5Cmax%5C%7BP%5Bb_t+%3D+1%5D%2C+1-+P%5Bb_t+%3D+1%5D+%5C%7D+) 。实践中，![[公式]](https://www.zhihu.com/equation?tex=E_0%5BT%5D) 可以用 ![[公式]](https://www.zhihu.com/equation?tex=T) 的指数加权移动平均来估计；![[公式]](https://www.zhihu.com/equation?tex=P%5Bb_t+%3D1+%5D) 可以用 buy ticks 占所有 ticks 比重的指数加权移动平均来估计。

![[公式]](https://www.zhihu.com/equation?tex=T%5E%2A+%3D+%5Carg+%5Cmin_T%5Cleft%5C%7B+%5Ctheta_T+%5Cgeq+E_0%5BT%5D+%5Ccdot+%5Cmax%5C%7BP%5Bb_t+%3D+1%5D%2C+1-+P%5Bb_t+%3D+1%5D+%5C%7D++%5Cright%5C%7D+) 为一个 TRB 间隔。与 TIBs 相比，TRBs 分别统计了买卖两边的 tick 数，而不会抵消某一边，这样定义的效果会更好。（只能说两种定义都有道理，bob未经实践不好评价，附上原文～）

> Instead of measuring the length of the longest sequence, we count the number of ticks of each side, without offsetting them (no imbalance). In the context of forming bars, this turns out to be a more useful definition than measuring sequence lengths.

 **Volume/Dollar Runs Bars**

VRBs、DRBs 的思想和定义与1-3类似：当买卖双方某一方累计成交量 / 成交额超过预期值时采样一次。

首先定义 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta_T+%3D+%5Cmax+%5Cleft%5C%7B+%5Csum_%7Bt%3D1%7D%5ET+v_t+%7B%5Cbf+1%7D%5C%7Bb_t+%3D+1%5C%7D%2C+%5Csum_%7Bt%3D1%7D%5ET+v_t+%7B%5Cbf+1%7D%5C%7Bb_t+%3D+-1%5C%7D+%5Cright%5C%7D)

则 ![[公式]](https://www.zhihu.com/equation?tex=E_0%5B%5Ctheta_T%5D+%3D+E_0%5BT%5D+%5Ccdot+%5Cmax%5C%7BP%5Bb_t+%3D+1%5DE_0%5Bv_t%7Cb_t+%3D+1%5D%2C+1-+P%5Bb_t+%3D+1%5DE_0%5Bv_t%7Cb_t+%3D+-1%5D+%5C%7D+) 。
实践中，![[公式]](https://www.zhihu.com/equation?tex=E_0%5BT%5D) 可以用 ![[公式]](https://www.zhihu.com/equation?tex=T) 的指数加权移动平均来估计；![[公式]](https://www.zhihu.com/equation?tex=P%5Bb_t+%3D1+%5D) 可以用 buy ticks 占所有 ticks 比重的指数加权移动平均来估计； ![[公式]](https://www.zhihu.com/equation?tex=E_0%5Bv_t%7Cb_t+%3D+1%5D) 可用买方交易量的指数加权移动平均来估计。

![[公式]](https://www.zhihu.com/equation?tex=T%5E%2A+%3D+%5Carg+%5Cmin_T%5Cleft%5C%7B+%5Ctheta_T+%5Cgeq+E_0%5BT%5D+%5Ccdot+%5Cmax%5C%7BP%5Bb_t+%3D+1%5DE_0%5Bv_t%7Cb_t+%3D+1%5D%2C+%5C%5C+%281-+P%5Bb_t+%3D+1%5D%29E_0%5Bv_t%7Cb_t+%3D+-1%5D+%5C%7D++%5Cright%5C%7D+)

为一个 TRB 间隔

## 2.5 Sampling Features

到目前为止，我们学习了如何由一组非结构化数据集构造连续、同性质的结构化数据集。在这个数据集上应用 ML 算法总体上还不合适，因为有以下不利因素：一是一些 ML 算法 “do not scale well with sample size”；二是 ML 算法只有从相关样本（relevant examples）中学习才能实现最高精确率。这一部分我们要讨论如何对 bars 采样来产生相关训练样本。

**1. 下采样（Sampling for Reduction）**

如前所述，对特征进行采样的一个原因是减少用于 ML 训练的数据（仅选择相关样本），即下采样。下采样可以通过等间距序列采样（linspace sampling）或依均匀分布随机采样（uniform sampling）。

linspace sampling 的优点是简单，缺点是其间距设定过于主观（arbitrary），且结果可能依赖于第一个 bar 的选取；而 uniform sampling 在全样本集上随机采样，解决了这些问题。然而两种方式都不保证最具有预测力 / 信息量 的样本被采样出来。

**2. Event-Based Sampling**

资产组合经理通常会在某些事件发生后做决策，这些事件包括宏观统计结果的发布、波动率突然上升、价差 / 息差（spread）显著偏离均衡水平等。我们将这些描述为显著事件，并让 ML 学习这些显著事件中是否包含精确规律。结果往往是 No，但没关系，我们可以重新定义“显著事件”，或者换一个 feature 再试一次。

举个 Event-Based Sampling 的栗子 🌰—— [CUSUM](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/CUSUM) Filter。CUSUM（**cu**mulative **sum**）Filter 是一种质量控制方法，旨在检测某个量相对目标值的偏移。考虑由局部平稳过程产生的一组 IID 观测值 ![[公式]](https://www.zhihu.com/equation?tex=%5C%7By_t%5C%7D_%7Bt%3D1%2C+...%2C+T%7D) 定义 CUSUM：

![[公式]](https://www.zhihu.com/equation?tex=S_t+%3D+%5Cmax+%5C%7B+0%2C+S_%7Bt-1%7D+%2B+y_t+-+E_%7Bt-1%7D%5By_t%5D+%5C%7D%2C+S_0+%3D+0)

则满足 ![[公式]](https://www.zhihu.com/equation?tex=S_t+%5Cgeq+h) 的第一个 ![[公式]](https://www.zhihu.com/equation?tex=t) 会被采样。注意到当 ![[公式]](https://www.zhihu.com/equation?tex=+y_t+%5Cleq+E_%7Bt-1%7D%5By_t%5D+-S_%7Bt-1%7D+) 时 ![[公式]](https://www.zhihu.com/equation?tex=S_t+%3D+0) ，这意味着我们会跳过向下的偏移， ![[公式]](https://www.zhihu.com/equation?tex=S_t) 被用来检测向上的偏移。（“the filter is set up to identify a sequence of upside divergences from any reset level zero”）

![[公式]](https://www.zhihu.com/equation?tex=S_t+%5Cgeq+h+%5Ciff+%5Cexists+%5Ctau+%5Cin+%5B1%2C+t%5D+%5Ctext%7B+s.t.+%7D+%5Csum_%7Bi+%3D+%5Ctau%7D%5Et+%28y_i+-+E_%7Bi-1%7D%5By_i%5D%29+%5Cgeq+h)

CUSUM filter 可扩展为对称形式，即

![[公式]](https://www.zhihu.com/equation?tex=S_t%5E%2B+%3D+%5Cmax+%5C%7B+0%2C+S_%7Bt-1%7D%5E%2B+%2B+y_t+-+E_%7Bt-1%7D%5By_t%5D+%5C%7D%2C+S_0%5E%2B+%3D+0+%5C%5C+S_t%5E-+%3D+%5Cmin+%5C%7B+0%2C+S_%7Bt-1%7D%5E-+%2B+y_t+-+E_%7Bt-1%7D%5By_t%5D+%5C%7D%2C+S_0%5E-+%3D+0+%5C%5C+S_t+%3D+%5Cmax+%5C%7B+S_t%5E%2B+%2C+S_t%5E-+%5C%7D)

若 ![[公式]](https://www.zhihu.com/equation?tex=y_t) 为价格序列，![[公式]](https://www.zhihu.com/equation?tex=E_%7Bt-1%7D%5By_t%5D+%3D+y_%7Bt-1%7D) （*说实话 ![[公式]](https://www.zhihu.com/equation?tex=E_%7Bt-1%7D%5By_t%5D) 的含义我不大明白*），我们可以得到如下采样结果：

![img](https://pic2.zhimg.com/80/v2-b080927a181e8770fd3362e4e58a4891_hd.jpg)

```
import pandas as pd

AAPL = pd.read_csv("/Users/bob/Desktop/美股/EOD-AAPL.csv") // 近300天的 AAPL 数据
dif = AAPL.Adj_Close.diff(1).values

S_pos, S_neg, event = 0, 0, []
h = 10
for i in range(1, 300):
    S_pos = max(0, S_pos + dif[i])
    S_neg = min(0, S_neg + dif[i])
    if S_pos > h:
        S_pos = 0
        event.append(i)
    elif S_neg < -h:
        S_neg = 0
        event.append(i)
// 接着把价格序列画出来，根据 event 描点
```

## Reference
