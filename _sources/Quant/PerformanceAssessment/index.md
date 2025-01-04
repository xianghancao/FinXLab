# Performance

## PROM

悲观收益率PROM（*Pessimistic return on margin*）是一个年化收益率估计，它保守得考虑到实盘交易的表现会比历史数据回测要差。作为一个鲁棒性良好的策略评价指标，它同时考虑了总盈利，总亏损，平均盈利，平均亏损，盈利次数，亏损次数。

盈利的交易次数减去平方根，意思是用标准误(standard error)调整。这样调整后的交易次数再乘以平均盈利金额，得到调整后的总盈利(adjusted gross profit)。对于亏损的交易，用交易次数加上其标准误来进行调整，再乘以平均亏损金额，得到调整后的总亏损(adjusted gross loss)。


PROM = (调整后的总盈利 - 调整后的总亏损)/准备金

上述例子清晰得说明了这种方法为什么是悲观的。它假设交易系统在实盘的时候，并没有回测的盈利次数多，亏损次数反而不会减少。这是一种保守估计。

这种保守估计有什么好处呢？计算过程中使用标准误，PROM惩罚了那些交易次数少的策略。交易次数越少，平方根越大。因为这种原因，如果策略的交易次数越少，PROM越悲观。


更严格的一种衍生形式是PROM计算中剔除最大一笔交易。这个对于股票市场来说很重要，当回测期间存在黑天鹅时间的时候，交易系统也会趁机发一笔横财（windfall profit）。但是会严重抬高了平均盈利金额，从保守估计的角度，不考虑这种交易。

## sharpe ratio

The Sharpe ratio calculation assumes that returns are normally distributed. However, in practice, the distribution of returns has "fatter tails" and extreme events are more likely to occur than a Gaussian distribution would lead us to believe. Hence, the Sharpe ratio is poor at characterising tail risk.

The Sharpe Ratio S is defined by the following relation:
$$
S = \frac{E(R - R_b)}{\sqrt{Var(R-R_b)}}\\
$$
where R is return of strategy, $R_b$ is the period return of benchmark.

The annualized Sharpe is caluculated as follows:
$$
S_A =  \sqrt{N}\frac{E(R - R_b)}{\sqrt{Var(R-R_b)}}
$$
where $N = 252$, $R$ and $R_b$ are daily returns. 

The Sharpe ratio generally utilises the risk-free rate and often, for US equities strategies, this is based on 10-year government Treasury bills. 

Although this point might seem obvious to some, transaction costs MUST be included in the calculation of Sharpe ratio in order for it to be realistic. There are countless examples of trading strategies that have high Sharpes (and thus a likelihood of great profitability) only to be reduced to low Sharpe, low profitability strategies once realistic costs have been factored in. This means making use of the *net returns* when calculating in excess of the benchmark. Hence, transaction costs must be factored in *upstream* of the Sharpe ratio calculation.

## Drawdown & Drawdown Period



```python
def drawdown(cpnl):
    _cpnl = cpnl * np.ones((cpnl.shape[0], cpnl.shape[0]))
    _cpnl = np.tril(_cpnl)
    max_cpnl = np.nanmax(_cpnl, axis=1)
    return cpnl - max_cpnl
```

drawdown period

```python
def drawdown_period(cpnl):
    _cpnl = cpnl * np.ones((cpnl.shape[0], cpnl.shape[0]))
    _cpnl = np.tril(_cpnl)
    _cpnl = np.hstack((np.zeros((cpnl.shape[0],1)), _cpnl))
    max_cpnl = np.argmax(_cpnl, axis=1)
    return np.arange(len(cpnl)) - max_cpnl + 1



```

## Rsquare

```python

def Rsquared(y):
    # return R^2 where x and y are array-like
    from scipy.stats import linregress
    x = np.arange(len(y))
    slope, intercept, r_value, p_value, std_err = linregress(x, y)
    return r_value**2, x*slope + intercept
```

## ICIR

```python
class ICIR(): 
   def stat_IC(self):
        # alpha 的 Infomation Coefficience(这里用RankIC，spearman rank) 即某时点某因子在全部股票因子暴露值排名与其下期回报排名的截面相关系数。
        
        def rankdata(x):
            import scipy.stats as st
            tmp_x = x.copy() 
            idx = np.sum(~np.isnan(tmp_x), axis=1) == 0  #某行全为nan值
            tmp_x[idx, :] = 0
            #sz = np.sum(~np.isnan(x), axis=1)
            res = st.mstats.rankdata(tmp_x, axis=1)   #.T/sz.astype(float)).T
            res[np.isnan(x)] = np.nan
            return res

        if np.isnan(self.sharpe) or self.sharpe == 0: 
            self.IC = np.nan
            self.IC_IR = np.nan
            self.IC_arr = np.nan * np.zeros_like(self.resample_dates)
            return 
        IC_arr = []
        rank_wgts = rankdata(self.scaled_resample_wgts)
        rank_return = rankdata(self.resample_return)
        #from scipy.stats import linregress
        for i in xrange(self.scaled_resample_wgts.shape[0]):
            index_ = ~np.isnan(rank_wgts[i]) * ~np.isnan(rank_return[i])
            ic_val = np.corrcoef(rank_wgts[i][index_], rank_return[i][index_])[0, 1]
            #x = rank_wgts[i][index_] 
            #y = rank_return[i][index_]
            #slope, intercept, r_value, p_value, std_err = linregress(x, y) 
            IC_arr.append(ic_val)

        self.IC_arr = np.nan_to_num(IC_arr)
        self.IC = np.nanmean(IC_arr)
        self.IC_IR = np.nanmean(np.nan_to_num(IC_arr))/np.nanstd(np.nan_to_num(IC_arr))
```



## Reference

The evaluation and optimization of trading strategies.* Robert Pardo