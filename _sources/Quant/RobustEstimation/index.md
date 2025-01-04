# Robust Estimation

## 要解决的问题

- 输入(历史数据,策略参数)的微小变动
- 订单被跳过

## 蒙特卡洛模拟

- 置信水平 95%
- 仿真次数, 10 ~ 100次

## 测试种类:

- 随机交易订单

Exact variation -  只是 shuffles orders, 不影响总的利润, 但可以观察最大回撤

Resampling variation - 高级重采样方法包括允许重复采样, 允许部分不在采样的样本集, 总利润会发生改变, 这是一种极端测试

- 随机跳过订单

真实情况可能miss 一些订单, 或者订单没有成交, 观察一些订单被跳过对于曲线的影响

- 随机策略参数

策略中的指标, 常数等的敏感性测试, 比如参数的10%改变, 就会在原始值上下10%随机取值

- 随机初始点

好的策略应该是初始点不敏感

- 随机历史价格

策略太依赖于价格数据, 过拟合的一种, 对于OHLC等做随机变化处理, 举例使用ATR作为变动参考, 变动10%, 价格数据变动 = 10%*ATR

- 随机spread, slippage, min distance of stop order from price

Ref:

SQ help documents

## multi-markets test and multi-periods test

The optimization of the model is carried out in a diversified market basket to obtain a measure of the universality and robustness of the trading model. The more markets a model can trade in, the more useful it is. Moreover, the more diverse the market the strategy can trade in, the more robust the strategy will be. In addition, this broader assessment provides an additional measure of statistical validity. A model that works well in only one market, unless that's what it's designed to do, can be a red flag.[1]

[1]Robert Pardo, *The Evaluation and Optimization of Trading Strategies*, [Chapter 10](/Credit/Sources/The Evaluation and Optimization of Trading Strategies/第十章 优化)