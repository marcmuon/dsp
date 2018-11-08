[Think Stats Chapter 6 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2007.html#toc60) (household income)

>> 
### Ch 6 Ex 1 - Sampling Distribution: Skewness and CDF Probability Calc


```python
import hinc
import hinc2
import numpy as np
```


```python
df = hinc.ReadData()  # income df
```


```python
sample = hinc2.InterpolateSample(df, log_upper=6.0)
sample = np.power(10, sample)
```

#### First find Mean and Median for use in skew calcs


```python
samp_median = np.median(sample)  # Compute new sample median
samp_median
```




    51226.93306562372




```python
samp_mean = np.mean(sample)  # Compute new sample mean
samp_mean  # Mean > Median, suggests right-tailed skewness
```




    74278.7075311872



#### Skewness Calculation following by Pearson Median Skewness


```python
from scipy.stats import moment

samp_std = np.std(sample)
third = moment(sample, moment = 3)
skew = third / samp_std**3  # Compute skewness
skew  # Suggests right tailed skewness
```




    4.949920244429585




```python
pskew = 3*(samp_mean - samp_median) / samp_std  # Compute Pearson Median Skewness
pskew  # Suggests right-tailed skewness
```




    0.7361105192428792



#### Create CDF


```python
n = len(sample)
x = np.sort(sample)
y = np.arange(1, len(x)+1) / n
```

#### Find where Mean is on the x-axis, get probability for `X<=x` from y-axis


```python
x2 = x.astype(int)
itemindex = np.where(x2==74279)
y[itemindex]
```




    array([0.66001405])



#### How is this influenced by upper bound?
The upper bound is arbitratily set in this interpolated sampling example to 1 million dollars. Since we don't know the actual upper bound, we can't make a conclusion about the real skewness

