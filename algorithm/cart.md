使用cart树进行回归分析--水藻浓度预测
========================================================
首先讲一下关于cart树的几个概念。
--------------------------------------------------------
### 概述

这个思想最早由Breiman教授提出，而rpart是cart树的一种实现算法。幽默的是，rpart这个名字后面比cart更加为人所知，足以说明这个算法的影响力。

rpart构建的回归/分类模型可以用一个二叉树来简单表示，比如在水藻浓度预测模型里面，我们最终得到的模型可能是这样的：


```r
library(DMwR)
```

```
## Loading required package: lattice
## Loading required package: grid
## KernSmooth 2.23 loaded
## Copyright M. P. Wand 1997-2009
```

```r
library(rpart)
data(algae)
algae <- algae[-manyNAs(algae), ]
rt.a1 <- rpart(a1 ~ ., data = algae[, 1:12])
plot(rt.a1, uniform = T, branch = 1, margin = 0.1, cex = 0.9)
text(rt.a1, cex = 0.75)
```

![plot of chunk cart树示例](figure/cart树示例.png) 

大体来说，树的构造方式是这样的:
- 寻找一个可以把数据 *最好的* 分成2部分的变量。*最好的* 定义见下文
- 在分好的2个子集上，重复上面的步骤。

### Notations
在说明rpart算法之前，我们先定义几个变量

$$\begin{eqnarray}
& \pi_{i} & i = 1,3,...C \,表示每个类别出现的先验概率 \\
& L(i,j) & i = 1,3,...C \,把类别i和j错分的损失函数。L(i,i) = 0  \\
& A & 表示树中某个节点 \\
& \tau(x) & 样本x的正确类别，取值范围：1,2,...C \\
& \tau(A) & 节点A被赋为哪个类，如果A是叶子节点 \\
& n_{i}, n_{A} & class\,i和节点A中的样本数 \\
& P(A) & 节点A 的概率\\
&  & = \Sigma^{C}_{i=1}\pi_{i}P\{x \in A | \tau(x) = i\} \\ 
&  & \approx \Sigma^{C}_{i=1}\pi_{i}{n_{iA}}/{n_i} \\
& P(i|A) &\, P(\tau(x) = i | x \in A) \\
&  & = \pi_i P(x \in A | \tau(x) = i)/P(x \in A) \\   
&  & \approx \pi_i (n_iA/n_i)/\Sigma\pi_i (n_{iA}/n_i) \\
& R(A) & 节点 A 的risk \\
& & = \Sigma^C_i p(i|A)L(i, \tau(A)) \\
& R(T) &  树T 的risk \\ 
& & = \Sigma^k_{j=1} P(A_j)R(A_j)\, A_j表示叶子节点 
\end{eqnarray}$$  


### 构建CART树
#### 分类准则

如果要把节点$A$分裂成$A_L和A_R$，我们很自然会想到：
$$P(A_L)R(A_L) + P(A_R)R(A_R) < P(A)R(A)$$
这种想法是选择使得风险最小的那种分裂方式。但是这种想法是有缺陷的，比如：

假设损失函数都是1，当前节点中有80%是class 1，20%是class 0，有一种分裂方式得到的$A_L$中100%都是class 1，$A_R$中54%是class 1。显然在每个子节点上，为了使风险最小，$\tau(A_L) = \tau(A_R) = 1$。所以这样总风险并没有减少（原来分错的还是分错了），按照以上的选取标准可能就不会选这种分裂方式。但是这样的分裂是有意义的，不能简单放弃。实际情况中，在分裂第一个节点时可能找不到比这种分裂更好的方式。

更重要的是，按照风险下降去选择的分裂条件可能不是我们想要的。比如有一个分裂使得2个节点的纯度分别为85%-50%，而另一个则是70%-70%。风险最小的话我们可能会去选择后者，而实际我们更愿意选择前者。因为它至少在一个节点上效果更好。

如果利用回溯算法，可以完全解决上面的担忧，找到最优分裂。但是这样过于耗时。rpart采用某种“不纯度(impurity)”来衡量每个节点。常用的两种不纯度度量是“熵(information index)”: $f(p) = -p log(p)$和“Gini index”：$f(p) = p(1 - p)$，不纯度的计算方式就是：
$$I(A) = \Sigma_{i=1}^C f(p_{iA})$$
所以在rpart中，使不纯度下降最大的分裂就是最优分裂方式
$$\Delta I = P(A)I(A) - P(A_L)I(A_L) - P(A_R)I(A_R)$$

#### 增加损失函数
就如刚才例子中所示，单纯只看损失函数不够，但是如果完全放弃损失函数也不好。有2种增加损失函数的手法，generalized Gini index和altered priors，在rpart中采用后者来扩展原Gini不纯度的计算。

1.generalized Gini index

首先你要想象这么一个场景：给定一个集合A，你不知道怎么指定每个样本的类别。于是你只能用随机猜的方法。每个类的样本被抽到的概率是$p_i$，你在随机指定类别时也按照${p_1,\, p_2,\, ... p_C}$这样的概率去指定。那么此时你的“期望损失”就是：
$$G(A) = \sum_i \sum_j L(i,j)p_{iA} p_{jA}$$
你也可以把它当作一种generalized的不纯度。事实上，当$C = 2$时，$G(A) = L(0, 1)p(1 - p) + L(1, 0)p(1 - p)$，如果$L(0, 1) == L(1, 0)$，此时就退化成原来的gini不纯度了。

用现在的$G(A)$去替换原来的$I(A)$，我们就把损失函数加到了不纯度计算中。

2.Altered priors
与Generalized Gini index方法不同，这里是通过修改先验概率$\pi_i$的计算方式，重新带入$P(A)$来将损失函数扩展到不纯度计算中的。
首先回想一下$R(A)$的定义：
$$\begin{eqnarray}
& R(A) &= \sum^C_{i=1} p(i|A)L(i, \tau(A)) \\
& & = \sum^C_{i=1} \pi_i L(i, \tau(A)) (n_{iA}/n_i)(n/n_A)
\end{eqnarray}$$

假设存在$\tilde{\pi}和\tilde{L}$，使得
$$\begin{eqnarray}
\tilde{\pi_i}\tilde{L}(i,j) = \pi_i L(i, j) & & \forall i, j \in C
\end{eqnarray}$$
如果要求$\tilde{L}$和0-1损失函数成正比，那么$\tilde{\pi}$将能够表示成：
$$\tilde{\pi} = \frac{\pi_i L_i}{\tilde L(i, j)}$$
用$\tilde{\pi_i}$代替$P(A)$中的${\pi_i}$，重新计算不纯度，就将损失函数扩展到不纯度计算中了。

### 用cart树/rpart算法预测水藻浓度

首先，我们构造水藻a1的回归树模型：

```r
library(DMwR)
library(rpart)
data(algae)
set.seed(1234)
algae <- algae[-manyNAs(algae), ]
rt.a1 <- rpart(a1 ~ ., data = algae[, 1:12])
plot(rt.a1, uniform = T, branch = 1, margin = 0.1, cex = 0.9)
text(rt.a1, cex = 0.75)
```

![plot of chunk a1-cart树示例](figure/a1-cart树示例.png) 

其中树的具体参数如下：

```r
rt.a1
```

```
## n= 198 
## 
## node), split, n, deviance, yval
##       * denotes terminal node
## 
##  1) root 198 90400.0 17.000  
##    2) PO4>=43.82 147 31280.0  8.980  
##      4) Cl>=7.806 140 21620.0  7.493  
##        8) oPO4>=51.12 84  3441.0  3.846 *
##        9) oPO4< 51.12 56 15390.0 12.960  
##         18) mnO2>=10.05 24  1249.0  6.717 *
##         19) mnO2< 10.05 32 12500.0 17.650  
##           38) NO3>=3.188 9   257.1  7.867 *
##           39) NO3< 3.188 23 11050.0 21.470  
##             78) mnO2< 8 13  2920.0 13.810 *
##             79) mnO2>=8 10  6371.0 31.440 *
##      5) Cl< 7.806 7  3158.0 38.710 *
##    3) PO4< 43.82 51 22440.0 40.100  
##      6) mxPH< 7.87 28 11450.0 33.450  
##       12) mxPH>=7.045 18  5146.0 26.390 *
##       13) mxPH< 7.045 10  3798.0 46.150 *
##      7) mxPH>=7.87 23  8241.0 48.200  
##       14) PO4>=15.18 12  3048.0 38.180 *
##       15) PO4< 15.18 11  2674.0 59.140 *
```

每行中每列的含义如下：

1. node)，节点编号
2. split，分裂条件
3. n，在父节点中，符合分裂条件的样本数，也称之为该节点覆盖的样本数。比如root节点覆盖的样本数就是所有样本。左边第一个节点就是满足所有PO4>=43.818的样本，共147个。
4. deviance，$deviance = \Sigma_i(y_i - \bar{y})^2, \,\,y_i$表示每个样本的y值
5. yval，表示每个节点的预测值，就是该节点覆盖样本的y值平均

在rpart函数中，以下3个参数会停止树的生长，防止出现严重过拟合，分别是（具体见rpart.control）：

1. cp，complexity parameter，以deviance为例，如果$deviance(A)*cp >= deviance(A) - deviance(A_L) - deviance(A_R)$，那么节点A将停止分裂
2. minsplit，节点覆盖的最少样本数，如果少于这个阈值，节点停止分裂
3. maxdepth，树的最大深度

另外，在rpart中，保存了cross-validation的数据，我们可以根据这些数据，通过1-se方法，获取最优的cp参数。


```r
printcp(rt.a1)
```

```
## 
## Regression tree:
## rpart(formula = a1 ~ ., data = algae[, 1:12])
## 
## Variables actually used in tree construction:
## [1] Cl   mnO2 mxPH NO3  oPO4 PO4 
## 
## Root node error: 90401/198 = 457
## 
## n= 198 
## 
##      CP nsplit rel error xerror xstd
## 1 0.406      0      1.00   1.01 0.13
## 2 0.072      1      0.59   0.70 0.11
## 3 0.031      2      0.52   0.69 0.12
## 4 0.030      3      0.49   0.71 0.12
## 5 0.028      4      0.46   0.73 0.12
## 6 0.028      5      0.43   0.71 0.12
## 7 0.018      6      0.41   0.71 0.12
## 8 0.016      7      0.39   0.73 0.11
## 9 0.010      9      0.35   0.75 0.11
```


其中，"rel error"就是当前树的deviance除以第一个棵树的deviance。而"xerror"和"xstd"则是10-fold交叉验证的平均deviance和方差。水藻这个实验是一个很好的例子，因为它给我们展示了"rel error"和"xerror"的最小值不在同一个树上的情况。这说明，在"rel error"最小的树上，也就是最下面这个树上，出现了过拟合：它在训练集上表现很好，但是泛化性较差。那么怎样找到比较准确，同时泛化性也比较好的树呢，"1-se"方法提供了这样一个思路：

1. 在cptable中寻找xerror最小的一颗树
2. 将这棵树的 xerror + se*xstd 作为可容忍的最小误差：tol err
3. 从上到下寻找xerror小于tol err的第一棵树（也就是最简单的那颗），将这棵树的cp参数作为裁剪的cp参数
4. 利用刚才获得的cp参数裁剪原来的树

具体实现算法如下：

```r
rt.prune <- function(tree, se = 1, verbose = T, ...) {
    if (ncol(tree$cptable) < 5) 
        tree else {
        lin.min.err <- which.min(tree$cptable[, 4])
        if (verbose & lin.min.err == nrow(tree$cptable)) 
            warning("Minimal Cross Validation Error is obtained  at the largest tree.\n Further tree growth   (achievable through smaller ’cp’ parameter value),\ncould produce more accurate tree.\n")
        tol.err <- tree$cptable[lin.min.err, 4] + se * tree$cptable[lin.min.err, 
            5]
        se.lin <- which(tree$cptable[, 4] <= tol.err)[1]
        prune.rpart(tree, cp = tree$cptable[se.lin, 1] + 1e-09)
    }
}
rpartXse <- function(form, data, se = 1, cp = 0, verbose = T, ...) {
    tree <- rpart(form, data, cp = cp, ...)
    if (verbose & ncol(tree$cptable) < 5) 
        warning("No pruning will be carried out because no estimates were obtained.")
    rt.prune(tree, se, verbose)
}
set.seed(1234)
rt2.a1 <- rpartXse(a1 ~ ., data = algae[, 1:12])

rt2.a1
```

```
## n= 198 
## 
## node), split, n, deviance, yval
##       * denotes terminal node
## 
## 1) root 198 90400 17.00  
##   2) PO4>=43.82 147 31280  8.98 *
##   3) PO4< 43.82 51 22440 40.10 *
```

```r
printcp(rt2.a1)
```

```
## 
## Regression tree:
## rpart(formula = form, data = data, cp = cp)
## 
## Variables actually used in tree construction:
## [1] PO4
## 
## Root node error: 90401/198 = 457
## 
## n= 198 
## 
##      CP nsplit rel error xerror xstd
## 1 0.406      0      1.00    1.0 0.13
## 2 0.072      1      0.59    0.7 0.11
```

这样，预测模型就建好了。其他6种水藻的预测也是如此。
