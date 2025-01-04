# Overfitting

Overfitting is optimization performed incorrectly. The overfitting of a trading strategy is the identification of parameters that produce good trading performance on in-sample price history but produce poor trading performance on out-of-sample price history.

It is helpful to understand that overfitting in the field of statistics means to fit a statistical model with too many parameters. The statistician continued to add variable after variable to his model to produce a closer and closer fit to past data with no effort to validate its robustness.

There are also degress of overfitting. A mildly overfit trading strategy may still produce real-time profit. But a massively overfit trading strategy is unlikely to do so.

Overfitting occurs when the fine line between proper and improper optimization is crossed. This line is easy to miss, and that is why it is essential to rigorously follow correct testing and optimization procedures.

The closeness of fitting and accuracy of prediction are attributed to two important factors: the complexity of modeling process and the degree of non-random movement of data[1]. Predictive models can only make valid predictions in the presence of nonrandom (predictable) behavior in the data. Models that operate within their capabilities are appropriate for the part of the model that is not subject to random price changes. When the model overadapts to the random part of the price change, the model will be inappropriate or overfitting.

The causes of overfitting

1. Insufficient degress of freedom
2. Inadequate data and trade sample
3. Incorrect optimization methods
4. Big win in a small trade sample

## Degrees of Freedom

[The evaluation and optimization of trading strategies] Each data point in the sample represents "one degree of freedom". A degree of freedom then is said to be consumed or used by each trading rule and by every data point necessary to calculate indicators.

## Reference

Robert Pardo, Optimization, *The Evaluation and Optimization of Trading Strategies*