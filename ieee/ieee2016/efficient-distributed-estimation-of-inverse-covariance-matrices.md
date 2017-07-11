# Efficient Distributed Estimation of Inverse Covariance Matrices

Jesus Arroyo, Elizabeth Hou

# Intro

inverse matrix经常用于分析gene expression和brain imaging。尤其是当数据符合Gaussian时候，在inverse covariance matrix非0的边表示一个在Markov random field中有一条边。（也是通过Z-transform之后设置一个threshold）因此有研究价值。

这篇论我研究了一个方法，在数据spread不同机器上的时候，来预估sparse inverse covariance matrix。