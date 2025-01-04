# Barra MFM model

[TOC]



## What is factor?

"In the realm of investing, a factor is any characteristic that can explain the risk and return performance of an asset. "[About factors by MSCI](https://www.msci.com/factor-investing)

## What is factor exposure?

Factor exposure is the value of the stock on the characteristics represented by the factor. For example, if the P/E of a stock is 15.9, its factor exposure to the P/E factor is 15.9.

## What is factor return?

For a given factor, according to a certain weight portfolio of all stocks form a portfolio built on this factor, the portfolio return is defined as the return of this factor.

## How do you build a factor portfolio for a given factor?

The common practice is to standardize the exposure of all stocks to this factor on a cross section. Then all the stocks will be ranked from good to bad according to the business logic of the factor, according to the value of the factor exposure. Finally, let's say you want to build a zero investment portfolio by going long the top 10% or 20% of the stocks and short the bottom 10% or 20% of the stocks.

## What is "pure" factor model of Barra MSCI?

Barra proposed the pure factor model, which can ensure that when constructing a factor portfolio on a cross section, each factor portfolio has 1 unit of exposure to the target factor, while the exposure to other factors is 0. A pure factor portfolio is constructed from a pure mathematical perspective in order to correctly quantify the benefits and risks of factors. No investability requirements were taken into account at the time of establishment, so the investability of a pure factor portfolio was very low. It meets the requirement that there is 1 unit exposure to the target factor and no exposure to other factors, so the validity of the factor can be measured correctly



## What does the investability refers?

Investability refers to whether the stock (long or short) position in the portfolio is reasonable, whether the turnover rate and transaction cost of the portfolio are actual, whether the stock into the portfolio has enough liquidity, the amount of capital that the portfolio can bear (i.e. the capacity of the portfolio) is large enough, etc.

## What doe Barra pure factor model aims for?

At the heart of Barra's risk factor model is risk analysis. Specifically, there are two purposes:

1. Calculate the correlation coefficient between the returns of individual stocks. The number of individual stocks in the market is very large. If the return sequence of individual stocks is used to calculate the correlation coefficient, the sequence length of return sequence should not be lower than the number of individual stocks. Otherwise, the return matrix is not full rank, so it is irreversible. Since this requirement is difficult to realize in reality, people want to decompose the return rate of a single stock into some common factors and then deduce the return rate of a single stock by solving the correlation coefficient of the return rate of a single stock.
2. Make a risk attribution for a given asset or portfolio. For an asset or portfolio, we want to understand what factors explain the volatility of its return.



## What is CNE5(Barra China Factor Model)?

The model considers one country factor, multiple industry factors and multiple style factors. Suppose the market has N stocks, P industries, and Q style factors. At any given point in time, build the model using factor exposure and stocks yields cross-section regression (cross - sectional regression) is as follows:

$$
\begin{bmatrix}
{r_1-r_f}\\
{r_2-r_f}\\
{\vdots}\\
{r_N-r_f}
\end{bmatrix}=
\begin{bmatrix}
{1}\\
{1}\\
{\vdots}\\
1\end{bmatrix}f_C+
\begin{bmatrix}
{X_1^{I_1}}\\
{X_2^{I_1}}\\
{\vdots}\\
{X_N^{I_1}}\end{bmatrix}f_{I_1}+\dots+
\begin{bmatrix}
{X_1^{I_P}}\\
{X_2^{I_P}}\\
{\vdots}\\
{X_N^{I_P}}\end{bmatrix}f_{I_P}+
\begin{bmatrix}
{X_1^{S_1}}\\
{X_2^{S_1}}\\
{\vdots}\\
{X_N^{S_1}}\end{bmatrix}f_{S_1}+\dots+
\begin{bmatrix}
{X_1^{S_Q}}\\
{X_2^{S_Q}}\\
{\vdots}\\
{X_N^{S_Q}}\end{bmatrix}f_{S_Q}+
\begin{bmatrix}
{\mu_1}\\
{\mu_2}\\
{\vdots}\\
{\mu_N}
\end{bmatrix}
$$


其中$r_{N}$是第 N 支股票的收益率， $r_f$是无风险收益率。$X_N^{I_P}$ 是股票 n 在行业 ![[公式]](https://www.zhihu.com/equation?tex=I_p) 的暴露，如果假设一个公司只能属于一个行业，那么 ![[公式]](https://www.zhihu.com/equation?tex=X_n%5E%7BI_p%7D) 的取值为 0（代表该股票不属于这个行业）或者 1（代表该股票属于这个行业）。![[公式]](https://www.zhihu.com/equation?tex=X_n%5E%7BS_q%7D) 是股票 n 在风格因子 ![[公式]](https://www.zhihu.com/equation?tex=S_q) 的暴露，它的取值经过了某种标准化（标准化的方法会在下文说明）。![[公式]](https://www.zhihu.com/equation?tex=u_n) 为股票 n 的超额收益中无法被因子解释的部分，因此也被称为该股票的特异性收益。![[公式]](https://www.zhihu.com/equation?tex=f_C) 为国家因子的因子收益率（所有股票在国家因子上的暴露都是1）；![[公式]](https://www.zhihu.com/equation?tex=f_%7BI_p%7D) 为行业 ![[公式]](https://www.zhihu.com/equation?tex=I_p) 因子的因子收益率；![[公式]](https://www.zhihu.com/equation?tex=f_%7BS_q%7D) 为风格因子 ![[公式]](https://www.zhihu.com/equation?tex=S_q) 的因子收益率。

对于给定某一期截面数据（记为 T 期），在截面回归时，Barra 采用**期初**的因子暴露取值（等价于 T - 1 期期末的因子暴露取值）和股票在 T **期内的收益率**进行截面回归。

## What is country factor?

In the market combination, the stock is by the circulation market value weight allocation, we use to express the return of the market combination. As mentioned above, is the weight of stock n's current market value. The following relationship can be obtained by substituting this set of weights into the factor model of CNE5. The left side is the market return, and the right side is the decomposition of country factor, industry factor, style factor and individual stock specific rate of return.

![[公式]](https://www.zhihu.com/equation?tex=%5Cbegin%7Bequation%7D+%5Clabel%7Beq%3Acountry+portfolio+model%7D+%5Cbegin%7Barray%7D%7Brll%7D+r_M+%26%3D%26%5Cdisplaystyle+f_C+%2B+%5Csum_%7Bn%3D1%7D%5E%7BN%7D%5Csum_%7Bp%3D1%7D%5EPs_nX_%7Bn%7D%5E%7BI_p%7Df_%7BI_p%7D%2B%5Csum_%7Bn%3D1%7D%5E%7BN%7D%5Csum_%7Bq%3D1%7D%5EQs_nX_%7Bn%7D%5E%7BS_q%7Df_%7BS_q%7D%2B%5Csum_%7Bn%3D1%7D%5E%7BN%7Ds_n+u_%7Bn%7D%5C%5C%5C%5C+%26%3D%26%5Cdisplaystyle+f_C+%2B+%5Csum_%7Bp%3D1%7D%5EP%5Cleft%28%5Csum_%7Bn%3D1%7D%5E%7BN%7Ds_nX_%7Bn%7D%5E%7BI_p%7D%5Cright%29f_%7BI_p%7D%2B%5Csum_%7Bq%3D1%7D%5EQ%5Cleft%28%5Csum_%7Bn%3D1%7D%5E%7BN%7Ds_nX_%7Bn%7D%5E%7BS_q%7D%5Cright%29f_%7BS_q%7D%2B%5Csum_%7Bn%3D1%7D%5E%7BN%7Ds_n+u_%7Bn%7D%5C%5C%5C%5C+%26%3D%26%5Cdisplaystyle+f_C+%2B+%5Csum_%7Bp%3D1%7D%5EPs_%7BI_p%7Df_%7BI_p%7D%2B%5Csum_%7Bq%3D1%7D%5EQ0%5Ctimes+f_%7BS_q%7D%2B%5Csum_%7Bn%3D1%7D%5E%7BN%7Ds_n+u_%7Bn%7D%5C%5C%5C%5C+%26%3D%26%5Cdisplaystyle+f_C+%2B+0%2B0%2B%5Csum_%7Bn%3D1%7D%5E%7BN%7Ds_n+u_%7Bn%7D%5C%5C%5C%5C+%26%3D%26%5Cdisplaystyle+f_C+%2B+%5Csum_%7Bn%3D1%7D%5E%7BN%7Ds_n+u_%7Bn%7D%5C%5C%5C%5C+%26%5Capprox%26%5Cdisplaystyle+f_C+%5Cend%7Barray%7D+%5Cend%7Bequation%7D)

The last term in the above formula is the sum of all stock specific returns, which is so small (close to 0) that it is ignored in the last step of the derivation. The heart of the derivation is how the middle two terms in the last to last step get to 0. For the first 0, it USES the constraint that the return rate of the industry factor is weighted to 0 according to the market value of the industry to exclude the collinearity between the industry and the country factor. For the second 0, it is based on the definition that the style factor is standardized using the weight of current market value. It can be seen that, under the definition of CNE5 model, the country factor rate of return does approximately represent the rate of return of market portfolio, so the combination of country factors is (approximately) the market portfolio. It is necessary to add this item in the new multi-factor model.



### Why country factor has collinear relationship with industrial factors exposure? How to solve this?

国家因子的因子暴露向量可以表达为 P 个行业因子因子暴露向量的线性组合。这会造成上式的解不唯一。为此，对行业因子的因子收益率作如下限制：

![[公式]](https://www.zhihu.com/equation?tex=s_%7BI_1%7Df_%7BI_1%7D%2Bs_%7BI_2%7Df_%7BI_2%7D%2B%5Cdots%2Bs_%7BI_P%7Df_%7BI_P%7D%3D0)

其中 ![[公式]](https://www.zhihu.com/equation?tex=s_%7BI_p%7D) 是所有属于行业 ![[公式]](https://www.zhihu.com/equation?tex=I_p) 的股票的按流通市值计算出的权重之和。

## What is the style factors?

### How to standardize the style factors?

在使用截面回归求解时，必须对风格因子的因子暴露进行标准化（国家和行业因子的因子暴露不需要标准化）。令 ![[公式]](https://www.zhihu.com/equation?tex=s_n) 表示股票 n 的流通市值权重。对风格因子的因子暴露进行标准化的初衷是这样的：**按照股票的流通市值权重构建的投资组合等同于整个市场，而市场对所有的风格因子都应该是中性的。**因此，按流通市值权重构建的股票投资组合在所有风格因子上的暴露必须是 0。这意味着经过标准化后的风格因子暴露 ![[公式]](https://www.zhihu.com/equation?tex=X_n%5E%7BS_q%7D) 必须满足：

![[公式]](https://www.zhihu.com/equation?tex=%5Cdisplaystyle%5Csum_%7Bn%3D1%7D%5ENs_%7Bn%7DX_%7Bn%7D%5E%7BS_q%7D%3D0%2C+%5Cmbox%7B+%7Dq%3D1%2C%5Cdots%2CQ)

此外，我们还必须对风格因子的因子暴露进行**标准差**的标准化，即要求对每一个风格因子 ![[公式]](https://www.zhihu.com/equation?tex=S_q)，![[公式]](https://www.zhihu.com/equation?tex=X_n%5E%7BS_q%7D) 的标准差为 1。这样便完成了对风格因子的因子暴露的标准化。https://zhuanlan.zhihu.com/p/38280638

## What is the different between the characteristics of country, style, industrial factors?

**国家纯因子投资组合**

由 ![[公式]](https://www.zhihu.com/equation?tex=f_C+%E2%89%88+r_M) 可知，国家纯因子投资组合就是近似的市场组合，它是**纯多头**组合：

- **国家纯因子是满额投资的（fully invested）。**国家纯因子的投资组合中所有股票（近似）按流通市值取权重，因此全部大于 0，即均为做多，不存在做空任何个股的情况。该投资组合使用了 100% 的资金。
- **国家纯因子投资组合对行业的暴露不为 0。**由定义可知，该投资组合在行业 I_p 的因子暴露为：![[公式]](https://www.zhihu.com/equation?tex=%5Csum_%7Bn%3D1%7D%5EN+s_%7Bn%7DX_%7Bn%7D%5E%7BI_p%7D%3Ds_%7BI_p%7D)
- 由于每个行业都包括一些股票（即对任何一个行业 ![[公式]](https://www.zhihu.com/equation?tex=I_p) ，总有一些股票满足 ![[公式]](https://www.zhihu.com/equation?tex=X_n%5E%7BI_p%7D+%3D+1) ），且股票的权重 ![[公式]](https://www.zhihu.com/equation?tex=s_n+%3E+0) ，因此上式大于 0。事实上，国家纯因子投资组合按照行业的市值权重暴露于不同的行业之中。
- **国家纯投资组合在所有风格因子上的暴露均为 0。**



**行业纯因子投资组合**

行业因子的纯因子投资组合是一个**多空**组合，它满足以下特征：

- **行业纯因子投资组合是零额投资（dollar-neutral）。**在这个投资组合中，我们做空一部分股票，然后用卖出股票的钱来做多另外一部分股票，因此整体来看我们的绝对投资额度为 0。
- **行业纯因子投资组合的本质是 100%****做多该行业，并 100%****做空国家纯因子组合（市场组合）。**由于国家纯因子组合对所有行业都有暴露，因此行业纯因子对自身行业有正的暴露，对其他所有行业有负的暴露。行业纯因子投资组合是 100% 做多该行业 100% 做空市场，**因此从业务上解释，这个组合就是认为该行业可以跑赢市场，该组合对应的就是该行业相对于市场的超额收益。**
- **行业纯因子投资组合对所有风格因子的暴露为 0。**该投资组合赚取的仅仅是行业相对市场的超额收益，这个超额收益不来自对任何风格因子的风险暴露（因为该组合对任何风格因子的风险暴露为 0）。



**风格纯因子投资组合**

风格因子的纯因子投资组合同样是一个**多空**组合，它满足下列特征：

- **风格纯因子投资组合是零额投资（dollar-neutral）。**在这个投资组合中，我们做空一部分股票，然后用卖出股票的钱来做多另外一部分股票，绝对投资额度为 0。
- **风格纯因子投资组合对该因子有 1 个单位的暴露。**
- **风格纯因子投资组合对自身风格因子外的其他所有因子、包括国家因子、行业因子和其他风格因子，的暴露都是 0。**从业务上解释，该投资组合是靠仅仅暴露于该因子来赚取这个风险因子的超额收益。

https://zhuanlan.zhihu.com/p/38280638

## What is risk factors? 

如果我们从整体上看好市场，那么只需要持有国家因子的纯因子组合（即近似的市场组合）；如果我们看好了某些行业，那么只需要持有那些特定行业的行业纯因子组合，从而赚取行业相对于市场的超额收益；如果我们看好了某个风格因子（比如小市值、价值等），那么只需要持有这些因子的纯因子组合，去赚取通过暴露于这些因子的超额收益。**它可以针对我们喜欢的因子（无论是市场、行业或是风格），构建出纯粹的仅仅针对于那些因子的投资组合，从而捕捉这些因子的风险收益。** **该投资组合如果赚钱，那么靠的是该投资组合在该风险因子上的单位暴露，靠的是该风险因子在时间维度上所带来的有效而稳定的风险溢价。

美国的 AQR 基金写过一篇文章来分析巴菲特的选股能力（Frazzini et al. 2013)。结果显示，巴菲特选股的收益率几乎可以完全被 1 个市场因子和 5 个风格因子的收益率来解释。它说明巴菲特的投资组合能赚钱是因为它以一定的权重有效的暴露在了这 6 个因子之中，长期稳定地赚取了这 6 个因子的风险溢价。巴菲特有一个科学的价值投资框架来保证它的投资组合对最合理的风险因子有着最合理的风险暴露，这些风险因子的风险溢价为他带来了年复一年的优秀收益。

- Frazzini, A., D. Kabiller, and L. H. Pedersen (2013). Buffett's alpha. Working paper 19681, National Bureau of Economic Research.

### How to deffentiate a good or bad return/risk factors?

![img](https://pic1.zhimg.com/80/v2-4f88536312ade3f222f432665e32d42c_hd.jpg)

上图展示了 4 种典型的因子收益率在时间维度上的统计特征：

1. 在左上角的第一幅图中，因子收益率在大部分时间为正，但波动较大。这说明该因子虽然可以贡献超额收益，但是其自身波动也带来了它对应的系统性风险。
2. 在右上角的第二幅图中，因子收益率在大部分时间为正，且波动很小。这说明该因子不但可以稳定的贡献超额收益，其自身的系统风险也非常低。**这在理论上是最优秀的收益因子。**
3. 在左下角的第三幅图中，因子收益率时正时负，波动很大，在统计上无法贡献非0的超额收益。因此，该因子无法带来超额收益，但是它可以显著的描述某种系统性风险。**因此这个因子是一个优秀的风险因子，但它不是收益因子。**
4. 在右下角的第四幅图中，因子收益率在过去显著为正，可以贡献稳定的超额收益，但是在最近不再有效，转变为纯粹的风险因子，无法贡献超额收益，仅能产生系统性风险。

在评价一个风险因子时，应按照正确的方法得到每个时间截面的纯因子投资组合，进而算出每一期的因子收益率。然后，通过对因子收益率的时间序列进行统计分析，最终判定该因子能否在长期稳定的贡献超额收益。同时，对因子收益率的统计分析也可以得到因子收益率之间的协方差矩阵，它是推导个股之间的协方差矩阵的必要条件之一。https://zhuanlan.zhihu.com/p/38280638



### What is **因子收益的驱动力**?

**系统性风险**和**系统性错误**是这些风险因子获取超额收益的两大主要驱动力。此外，机构投资者的投资限制也是造成某些风险因子有效的原因。驱动这些风险因子获得收益的原因都是可以持续的。因此我们可以乐观的预期那些在历史上被证明有效的因子，在今后会依然有效。https://zhuanlan.zhihu.com/p/38280638

首先，**风险因子反映的是一揽子股票所共同承担的某个特定的系统性风险，因此必须通过对应的风险溢价来补偿。**举例来说，小市值股票可能有较差的流动性以及较低的透明度。因此小市值股票的风险较市场整体水平更高。又或者价值因子之所以有效，是因为低估值的公司可能有更高的财务杠杆和未来的不确定性；相对于成长股，它们对于经济的冲击更加敏感。因此，价值因子的风险溢价就源自价值股票暴露于宏观经济风险的补偿。https://zhuanlan.zhihu.com/p/38280638

其次，**行为金融学认为，人们在投资的时候有不可避免的认知偏差和情感弱点**，**这些导致了投资行为偏差，产生了系统性的错误，从而给一些因子带来了收益。**仍以价值因子为例。厌恶损失（loss aversion）是一种著名的认知偏差。由于这种偏差，人们往往对于过去挣到钱的股票有更高的容忍。换句话说，如果股票 A 在最近一段时间为我们挣钱了，那么即便它未来下跌我们也不觉着怎么样，我们会把它看作风险较低的股票，要求较低的风险补偿；如果股票 B 在最近一段时间让我们亏钱了，我们则会把它看作风险较高的股票，希望它在未来会有更高的收益来补偿我们所感知的高风险。https://zhuanlan.zhihu.com/p/38280638

另一个驱动风险因子收益的原因是**机构投资者的投资限制**。以动量因子为例，如果一些股票的负面基本面消息对股价造成了冲击，持有这些股票的机构投资者可能会被迫的抛售他们的持仓。抛售会加剧股价的下跌。一旦资金从这些股票持续出逃，那么股价将会继续下跌，即下跌的股票在未来仍会下跌，这便贡献了动量因子的收益率。注意，在对这些股票的抛售中，机构投资者的行为并非非理性，而仅仅是因为机构必须遵守的条条框框的限制。https://zhuanlan.zhihu.com/p/38280638

### What is factor allocation?

价值、动量等风险因子虽然在历史上可以获得长期的超额收益，但必须说明的是，所有这些因子都是有周期性的。它们能否或的相对基准（市场指数）的超额收益和宏观经济的大环境有关。

特别的，**价值、动量、规模因子在经济上升、利率和通胀上行的宏观经济环境下较好；质量和低波动率因子则在经济下行的时候有比较好的表现。虽然因子收益率存在周期性，但不同因子间的收益率存在较低的相关性，这使得它们在一定程度上互补，“东边不亮西边亮”。**下图显示了使用中证 500 成分股计算的常见因子收益率的结果，可以清晰的验证不同因子收益率之间的低相关性。此外，MSCI 基于他们选定的六大风险因子所构建的因子指数的收益率的相关系数普遍在 -0.3 和 0.3 之间，也体现了低相关性。

![img](https://pic3.zhimg.com/80/v2-155803f452dddccb5efde1773064848a_hd.jpg)

为更好地理解因子收益的周期性，近几年国际上一些学者已经开始研究宏观经济参数（如 GDP，CPI，利率，利差等）如何影响不同因子的收益率。另一方面，因子收益率之间的低相关性则更是可以被我们直接利用的。把不同风险因子的投资组合按照它们的风险收益比配合在一起，得到更优秀的投资组合，这便是**因子配置（factor allocation）**。https://zhuanlan.zhihu.com/p/38280638

## What is the limitation for factor investment in China?

定性来看，美国的市场 ![[公式]](https://www.zhihu.com/equation?tex=%5Cbeta) 、因子 ![[公式]](https://www.zhihu.com/equation?tex=%5Cbeta) 、以及 ![[公式]](https://www.zhihu.com/equation?tex=%5Calpha) 的关系大致如下图所示。

![img](https://pic2.zhimg.com/80/v2-8e1c36245e26e27fae7d71e48a68d09d_hd.jpg)

在过去的 100 年中，除去几次股灾，美股的几大指数均呈现温和上涨的慢牛行情，通过暴露于市场风险，投资者可以获得稳定的长期收益。在因子 ![[公式]](https://www.zhihu.com/equation?tex=%5Cbeta) 方面，美国的 AQR 基金写过一篇文章来分析巴菲特的投资组合。结果显示，巴菲特投资组合的收益几乎可以完全被 1 个市场因子和 5 个风格因子解释。这说明该投资组合能赚钱是因为它以一定的权重有效的暴露在了这 6 个因子之中，长期稳定地赚取了这 6 个因子的风险溢价。**可见，因子** ![[公式]](https://www.zhihu.com/equation?tex=%5Cbeta) **在美国市场足够吸引人。此外，美国的对冲机制和投资手段也十分丰富，这些为因子投资的发展奠定了坚实的基础。**

再来看看中国，市场 ![[公式]](https://www.zhihu.com/equation?tex=%5Cbeta) 、因子 ![[公式]](https://www.zhihu.com/equation?tex=%5Cbeta) 、以及 ![[公式]](https://www.zhihu.com/equation?tex=%5Calpha) 的关系大致如下图所示。

![img](https://pic3.zhimg.com/80/v2-6ceb7eb7f45de0854b8a40c7f2e59616_hd.jpg)

中国和美国存在显著不同，大量实证显示**发达市场中主流并且有效的风格因子在国内市场发挥的空间不大、实际贡献的** ![[公式]](https://www.zhihu.com/equation?tex=%5Cbeta) **很小。**换个角度想，这说明还有一些适用于国内的未知因子等待挖掘，比如“私募基金”因子。2016 年赚钱效应极强的事件驱动型策略，比如定增，就完全可以理解为一个“风险”因子。基金通过这类策略赚钱，靠的正是其投资组合在这类风险因子上的暴露。另外，不能否认的是，由于监管不完善和个别机构投资者的道德败坏，国内的收益率中还存在**不少黑心** ![[公式]](https://www.zhihu.com/equation?tex=%5Calpha) **，它们也压榨了因子** ![[公式]](https://www.zhihu.com/equation?tex=%5Cbeta) **的生存空间。另一方面，国内缺乏有效的对冲和做空机制，纯多头的因子投资组合也要被迫暴露于市场风险之中，承受因子之外的风险。最后国内缺少实现因子投资的工具，如相应的 ETF。这些问题阻碍了因子投资在国内的发展。**https://zhuanlan.zhihu.com/p/38280638



### Factors exposure VS investability

**因子暴露 vs 可投资性**

第 2 节和第 3 节的论述说明一些风险因子可以在长期获得超额收益；此外，虽然因子收益率有周期性，但是我们可以充分利用不同因子间的低相关性将它们科学的配置在一起，从而得到风险收益比更好的投资组合。

这听起来确实令人振奋。**但在现实的投资世界中，满足各种限制的可投资的因子投资组合如何构建呢？回测中令人振奋的因子投资组合是否仅仅是理论上的数学游戏？因子的投资组合能否在现实投资世界中以指数化的形式被低成本的实现呢？**

**在将因子投资组合指数化时，因子暴露和可投资性（investability）是我们必须考虑的两个因素，而它俩天生矛盾；获得高的因子暴露是通过牺牲可投资性得到的；反之亦然。**我们必须在该投资组合的因子暴露和可投资性两者间权衡、取舍。

**可投资性是指投资组合中股票的仓位是否合理，该组合的换手率和交易成本是否实际，进入该组合的股票是否有足够的流动性、该投资组合能承担的资金量（即投资组合的容量）是否足够大等。**

下面的金字塔图描绘了 5 种因子投资组合指数化的方法。**自下而上，它们的因子暴露越来越高，而可投资性却越来越低。**下面一一说明。

![img](https://pic4.zhimg.com/80/v2-778d6b79b4d7e56345def489a49eae1f_hd.jpg)

**纯因子指数**

**位于金字塔顶端的是因子的纯因子（投资组合）指数。**纯因子投资组合是为了正确量化因子的收益和风险而从纯数学的角度构建的；建立时没有考虑任何可投资性的要求，因此纯因子投资组合的可投资性非常低。它满足对目标因子有 1 个单位的暴露，对其他因子没有暴露，因此可以正确的衡量因子的有效性。（注：纯因子组合的作用是正确计算因子收益率，从而通过模型推导出个股收益率之间的协方差矩阵。因此，虽然纯因子组合没有可投资性，但它在风险管理和业绩归因中有着非常重要的作用。）

**多空因子指数**

**接下来是多空因子指数，著名的就是法马-佛伦奇三因子模型中的那种构建方法**，即将所有股票按照因子暴露的好坏 10 等分，然后等权做多最优秀的那10%，等权做空最差的 10%，以此构建的投资组合。该方法对目标因子仍然有很高的因子暴露，但也不可避免的暴露于其他因子上，但是这种方法构建的投资组合较纯因子组合有一定的可投资性（假设可以做空个股）。

**高暴露因子指数**

**我们将位于金字塔中间的方法称为高暴露因子指数。它的地位说明它在一定程度上达到了因子暴露和可投资性之间的平衡。**在构建该指数时，同时考虑了因子暴露和各种可投资性的约束条件（如低换手率、高流动性、高容量）。通常，为了实现对目标因子的高暴露，此方法选择的股票是所有候选股票的一个子集。在甄选股票时，可按照排名或优化算法选出在该因子上的优秀股票，用它们构建因子指数。（下文会简要介绍排名和优化这两种算法。）较纯因子和多空两种方法，这种方法以牺牲了一定的因子暴露为代价换来了满意的可投资性。

**高容量因子指数**

**再下面一档称为高容量因子指数，它为了获得更好的可投资性而进一步降低了因子暴露。**这种方法并不对股票进行甄选，而是将候选池中的所有股票都包含在这类指数中。在决定股票的权重时，此方法同时考虑股票在该因子上的暴露程度以及股票自身的市值大小。通过这两方面的综合评判来最终决定每支股票在这类指数中的权重。

**市场指数（基准）**

**金字塔的底层是我们的投资基准，即市场指数。**市场指数是按照股票市值加权构建的指数，它是唯一的纯粹的反应被动投资的指数。由于股票按照市值加权，而股票的市值是所有投资者真真切切交易出来的，因此市场指数有着最高的可投资性。因为它作为基准，因此一般认为它对所有风险因子的暴露都是零。

## Reference

**https://zhuanlan.zhihu.com/p/38280638