[Think Stats Chapter 5 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2006.html#toc50) (blue men)

>> ### Chapter 5 Exercise 1
In the BRFSS (see Section 5.4), the distribution of heights is
roughly normal with parameters µ = 178 cm and σ = 7.7 cm for men, and
µ = 163 cm and σ = 7.3 cm for women.
In order to join Blue Man Group, you have to be male between 5’10” and
6’1” (see http://bluemancasting.com). What percentage of the U.S. male
population is in this range? Hint: use scipy.stats.norm.cdf.


```python
from scipy.stats import norm as norm
```

#### Difference of points on the normal CDF
The goal here is the following: `P(5'10 < X < 6'1)`, where `X` is normally distributed with mean 178cm and standard deviation 7.7cm. One way to think of this visually is integrating the area under the curve defined by the normal PDF. In this case, integrating between 177.8cm and 185.4cm on the normal curve with mean 178cm and std.dev. 7.7cm. This is equivalent to finding the length of the 'rise' of the CDF bounded by points `(x=177.8, y=CDF(177.8))` --> `(x=185.4, y=CDF(185.4))`. That follows since PDFs are derivatives of CDFs, hence you can integrate PDFs to get back to CDFs. The answer is 34.2% of the male population via the subtraction below.


```python
p = norm.cdf(185.4, 178, 7.7) - norm.cdf(177.8, 178, 7.7)
p  # 34.2% of the male population is in this range
```




    0.3420946829459531



#### Equivalent z-score calculation
Note that another way to do this is to run standardized versions of the random variables through the standard normal CDF. The standardization is `z = X - µ / σ`, and note the parameters of the cdf below have changed to µ = 0 and σ = 1.


```python
p_z = norm.cdf(.96103, 0, 1) - norm.cdf(-.0259, 0, 1)
p_z
```




    0.3420629080317316


