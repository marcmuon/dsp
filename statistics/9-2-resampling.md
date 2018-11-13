[Think Stats Chapter 9 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2010.html#toc90) (resampling)

>> ### Ch 9 Ex 2 - Resampling
Hypothesis testing calculation of p-value on two different methods: permutation vs resampling. In this case the output of both tests is nearly identical: we could expect observations like this or more extreme about 15-17% of the time due to chance alone. Thus we shouldn't reject the null hypothesis that the pregancy length of first babies vs other babies is of equal length


```python
import thinkstats2
import first
import numpy as np
```


```python
class DiffMeansPermute(thinkstats2.HypothesisTest):
    """From ThinkStats2 Chapter 9"""
    
    def TestStatistic(self, data):
        group1, group2 = data
        test_stat = abs(group1.mean() - group2.mean())
        return test_stat

    def MakeModel(self):
        group1, group2 = self.data
        self.n, self.m = len(group1), len(group2)
        self.pool = np.hstack((group1, group2))

    def RunModel(self):
        np.random.shuffle(self.pool)
        data = self.pool[:self.n], self.pool[self.n:]
        return data
```


```python
live, firsts, others = first.MakeFrames()
data = firsts.prglngth.values, others.prglngth.values
ht = DiffMeansPermute(data)
pvalue = ht.PValue()
pvalue  # p-value output from permutation test
```




    0.166




```python
class DiffMeansResample(DiffMeansPermute):
    def RunModel(self):
        group1 = np.random.choice(self.pool, self.n, replace=True)
        group2 = np.random.choice(self.pool, self.m, replace=True)
        return group1, group2
```


```python
live, firsts, others = first.MakeFrames()
data = firsts.prglngth.values, others.prglngth.values
ht = DiffMeansResample(data)
pvalue = ht.PValue()
pvalue  # p-value output from permutation test
```




    0.152



