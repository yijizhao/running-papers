# On Large-Batch Training for Deep Learning: Generalization Gap and Sharp Minima

Nitish Shirish Keskar, Dheevatsa Mudigere, Jorge Nocedal, Mikhail Smelyanskiy, Ping Tak Peter Tang

Northwestern University, Intel Corporation

# Intro

DL经常是解决一个non-convex的优化问题。mini-batch based SGD 可以理解为使用SGD，但加入了噪声。$$B$$通常远小于data point的个数，比如$$\{32, 64, ..., 512\}$$.

# Drawbacks of Large-Batch Methods

许多研究都发现了使用large-batch methods进行训练的时候，generalization gap会增加。但是large-batch和small-batch的方法通常产生相同的结果。有好几种可能的原因：

1. large-batch overfit
2. large-batch 收敛到了saddle point
3. large-batch方法缺少small-batch方法的探究性，并且会跑到和initial point非常接近的minimizer
4. large-和small-batch会converge到不同的点，其中generalization properties不同

这篇文章证明了最后两点。large-batch method会converge到sharp minimizers，而small-batch method会converge到flat minimizer。sharp minimizer的特点是Hessian的eigenvalue更大，所以generalize更差。flat minimum与sharp minimizer相比，lower precision，但better generalization。

比较了几个image recognition经典模型，发现使用small-batch的generalization error小很多。这篇paper还提到了一个很有意思的观点，这个generalization gap不是由于overfitting引起的，而是由于network模型的选择引起的。（不过在这里不是重点）

## Parameteric Plots

让$$x^*_s$$和$$x^*_l$$分别表示使用small和big batch-size的答案。对于$$\alpha \in [-1, 2]$$，plot $$f(\alpha x^*_s + (1-\alpha) x^*_l )$$，并且叠加classification accuracy。从图2发现large batch minima sharper。

## Sharpness of Minima

appendix中附录一个paper。minimizer的sharpness可以通过Hessian特征值的magnitude来表示。但是计算特征值计算需求较大，应用一个sensitivity measure。

$$A \in \mathbb{R}^{n \times p}$$，定义一个constraint set $$C_\epsilon$$，需要引入A的pseudo-inverse。

那么sensitivity/sharpness measure定义如下：

$$\phi_{x,f}(\epsilon, A) = \frac{max_{y\in C_\epsilon}(f(x+Ay)) - f(x)}{1 + f(x)} \times 100$$

一个问题是这里的$$A \in \mathbb{R}^{n \times p}$$如何构造。论文说按照identity matrix，还存有疑惑。

使用L-BFGS求解这个最大优化问题。而最后的结果证明了LB比SB的sharpness高1到2个数量级。

# Success of Small-Batch Methods

Small-batch相比于Large-batch，有一个缺点是很多noisy gradient。而正是因为考虑了这些noisy gradient，才能使得迭代跑出了sharp minimizer basin，到了更加flat minimizer。论文为了证明这一点，使用一个SB作为热启动，然后使用LB作为最后的训练。然后可以发现，一开始的epoch如果是使用SB进行的训练，最终使用LB训练之后，能够converge到和SB一样的结果，也就是flat minimizer。

最后的证明主要是基于实验的sharpness plotting。（但我个人觉得sharpness measure的定义有点一拍脑门的味道）

# Appendix

Sepp Hochreiter, Jurgen Schmidhuber, Flat minima. 1997