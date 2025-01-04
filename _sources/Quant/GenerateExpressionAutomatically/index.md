![Screen Shot 2020-10-21 at 11.52.17 AM](logo.png)


AutoMarketFeature(AMF)  aims to speedily generate a portfolio of characteristics based on price and volume in the market.

Its features include:

- Generate features by batch combinations
- Specifically select elements
- Control the probability of the occurrence of each element
- Connect with DataBase

## Conceptions:

elements: price and volume, e.g. close, open, low, high, volume e.g.

operators: math operators, e.g. add(), std();

expression: short for 'expr' means 'var(close, 10)'

layer: nesting levels, e.g. layer of 'var(close, 10)' is 1; layer of 'delay(var(clpse, 10), 1)' is 2;

## Files:

operators_blacklist.yaml: block the occurrence of specific elements;

prob_control.yaml: the probability the occurrence of each element;

## Examples:
```python
import os
os.path.join('/your_path/')
from lib import expr_generator
eg = expr_generator.ExprGenerator()
eg.get_one_expr(layer_num=1)
```
```python
[ExprGenerator] layer:1 expr:ts_min(Volume,60)
{'expr': 'ts_min(Volume,60)',
 'layer': 1,
  'create_date': '2020-10-22 14:38:49'}
```

```python
eg.get_one_expr(layer_num=2)
```

```python
[ExprGenerator] layer:2 expr:stddev(delta2(VolChg,60,100),200)
{'expr': 'stddev(delta2(VolChg,60,100),200)',
 'layer': 2,
 'create_date': '2020-10-22 14:45:37'}
```

generate more:
![image_01](image_01.png)