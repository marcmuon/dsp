[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

>> ### Chapter 2 Exercise 4
Using the variable `totalwgt_lb`, investigate whether first babies
are lighter or heavier than others. Compute Cohen’s d to quantify the
difference between the groups. How does it compare to the difference in
pregnancy length?


```python
import nsfg
import pandas as pd
import math
```

#### Creating total weight in pounds Series for first born babies vs. other babies


```python
df = nsfg.ReadFemPreg()
live = df[df['outcome'] == 1]
```


```python
first_babies = live[live['birthord'] == 1]
other_babies = live[live['birthord'] != 1]

first_weight = first_babies['totalwgt_lb']
others_weight = other_babies['totalwgt_lb']
```

Note that the mean birth weights of the two groups are quite close, and we'll investigate further by standardizing with Cohen's d.


```python
print('first baby mean weight:', first_weight.mean())
print('other baby mean weight:', others_weight.mean())
```

    first baby mean: 7.201094430437772
    other baby mean: 7.325855614973262


#### Homogeneity of Variance Assumption
If the standard deviation of the different test groups are very different, then it isn't appropriate to use pooled standard deviation in the effect size calculation for Cohen's d. In that case, the homogeneity of variance assumption would be violated, but here we're fine. 


```python
print('first baby std. dev. weight:', first_weight.std())
print('other baby std. dev. weight:', others_weight.std())
```

    first baby std. dev. weight: 1.4205728777207374
    other baby std. dev. weight: 1.3941954762143138



```python
def cohen_d(series1, series2):
    obs_effect = series1.mean() - series2.mean()
    def pooled_sd(series1,series2):
        pooled_var_num = len(series1)*series1.var() + len(series2)*series2.var()
        pooled_var_denom = len(series1) + len(series2)
        return math.sqrt(pooled_var_num/pooled_var_denom)
    return obs_effect / pooled_sd(series1,series2)
```

#### Cohen's d
The cohen's d effect size for first born babies vs. other babies is quite small at d = -0.08867. Cohen himself initially advised that .2 is a small effect size for d, and this is less than half that .2 value with an absolute value of our finding of d = .08867. While arbitrarily compared to some value of 'small = .2' won't always be appropriate based on context, in this case I feel it's a relevant baseline to think about. I looked it up and Cohen's initial recommendation of .2 for a small effect was based on CDC type data of female heights. In his example, he found d = .2 corresponded to the difference in heights between a 15 year old and 16 year old girl. 


```python
cohen_d(first_weight, others_weight)
```




    -0.088672927072602



#### Cohen's d calculation for pregancy length
To take the comparison of d back to the exercise question and to a d in the same data set: d for pregancy length of the two groups is .02887. So while the total weight d has a larger magnitude, they're still both quite small. Another note is that in addition to recommending d=.2 for a small size, Cohen recommended d=.5 for a medium effect and d=.8 for a large effect. Both of the d's for total weight and pregancy length are well below those thresholds.


```python
first_preglength = first_babies['prglngth']
others_preglength = other_babies['prglngth']
```


```python
cohen_d(first_preglength, others_preglength)
```




    0.028879044654449883



#### A note on the pooled SD calculation
Other sources online and in textbooks I referenced calculate Cohen's d differently than the Downey does in ThinkStats2. It looks like Downey's calculation is a slight derivation of what others specify for Hedges g:
http://www.polyu.edu.hk/mm/effectsizefaqs/effect_size_equations2.html
https://www.itl.nist.gov/div898/software/dataplot/refman2/auxillar/hedgeg.htm
Geoff Cumming's 'Understanding the New Statistics' text mentions that the choice of d (and its standardizer) varies quite frequently in the scientific literature in any case.
