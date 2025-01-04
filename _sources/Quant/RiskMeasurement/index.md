## 风险的定义和度量

Harray Markowizt对风险的定义是
$$
Std(\widetilde{r}) = \sqrt{Var(\widetilde{r})}\\
Var(\widetilde{r}) = E[(\widetilde{r}-\overline{r})^2]\\
\widetilde{r}:收益率\\
\overline{r}:预期回报或者平均回报率\\
$$
其他风险定义：

半方差 semivariance: 即认为只有负的收益或者较低的收益才是投资真正的风险。半方差的定义与方差类似，唯一的区别在于半方差仅使用低于均值的收益率样本。

下行风险 downside risk

损失概率 shortfall probability:收益率低于目标值的概率

在险价值 value at risk: 先取定一个目标概率，然后计算与该概率相应的收益率
分位数。

## 投资组合的风险

假设投资组合P由股票A和股票B组成，且各占50%的权重，它们收益率的相关系数为$\rho_{AB}$，那么：
$$
\sigma_P=\sqrt{(0.5*\sigma_A)^2+(0.5*\sigma_B)^2+2*(0.5*\sigma_A)*(0.5*\sigma_B)*\rho_{AB}}\\
\sigma_P \le0.5*\sigma_A+0.5*\sigma_B
$$
考虑一个有𝑁只股票组成的等权重投资组合，每只股票的风险都是$\sigma$，并且股
票之间的收益率互不相关，那么该组合的风险是:
$$
\sigma_P=\frac{\sigma}{\sqrt{N}}
$$
考虑一个有𝑁只股票组成的等权重投资组合，每只股票的风险都是$\sigma$，并且任
意两只股票收益率之间的相关系数都等于$\rho$，那么该组合的风险是:
$$
\sigma_P=\sigma*\sqrt{\frac{1+\rho*(N-1)}{N}}
$$
当组合中的股票数目N很大时，
$$
\sigma_P = \sigma*\sqrt{\rho}
$$

