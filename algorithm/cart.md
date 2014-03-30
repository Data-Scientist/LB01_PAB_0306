ʹ��cart�����лع����--ˮ��Ũ��Ԥ��
========================================================
���Ƚ�һ�¹���cart���ļ������
--------------------------------------------------------
### ����

���˼��������Breiman�����������rpart��cart����һ��ʵ���㷨����Ĭ���ǣ�rpart������ֺ����cart����Ϊ����֪������˵������㷨��Ӱ������

rpart�����Ļع�/����ģ�Ϳ�����һ�����������򵥱�ʾ��������ˮ��Ũ��Ԥ��ģ�����棬�������յõ���ģ�Ϳ����������ģ�


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

![plot of chunk cart��ʾ��](figure/cart��ʾ��.png) 

������˵�����Ĺ��췽ʽ��������:
- Ѱ��һ�����԰����� *��õ�* �ֳ�2���ֵı�����*��õ�* ���������
- �ڷֺõ�2���Ӽ��ϣ��ظ�����Ĳ��衣

### Notations
��˵��rpart�㷨֮ǰ�������ȶ��弸������

$$\begin{eqnarray}
& \pi_{i} & i = 1,3,...C \,��ʾÿ�������ֵ�������� \\
& L(i,j) & i = 1,3,...C \,�����i��j��ֵ���ʧ������L(i,i) = 0  \\
& A & ��ʾ����ĳ���ڵ� \\
& \tau(x) & ����x����ȷ���ȡֵ��Χ��1,2,...C \\
& \tau(A) & �ڵ�A����Ϊ�ĸ��࣬���A��Ҷ�ӽڵ� \\
& n_{i}, n_{A} & class\,i�ͽڵ�A�е������� \\
& P(A) & �ڵ�A �ĸ���\\
&  & = \Sigma^{C}_{i=1}\pi_{i}P\{x \in A | \tau(x) = i\} \\ 
&  & \approx \Sigma^{C}_{i=1}\pi_{i}{n_{iA}}/{n_i} \\
& P(i|A) &\, P(\tau(x) = i | x \in A) \\
&  & = \pi_i P(x \in A | \tau(x) = i)/P(x \in A) \\   
&  & \approx \pi_i (n_iA/n_i)/\Sigma\pi_i (n_{iA}/n_i) \\
& R(A) & �ڵ� A ��risk \\
& & = \Sigma^C_i p(i|A)L(i, \tau(A)) \\
& R(T) &  ��T ��risk \\ 
& & = \Sigma^k_{j=1} P(A_j)R(A_j)\, A_j��ʾҶ�ӽڵ� 
\end{eqnarray}$$  


### ����CART��
#### ����׼��

���Ҫ�ѽڵ�$A$���ѳ�$A_L��A_R$�����Ǻ���Ȼ���뵽��
$$P(A_L)R(A_L) + P(A_R)R(A_R) < P(A)R(A)$$
�����뷨��ѡ��ʹ�÷�����С�����ַ��ѷ�ʽ�����������뷨����ȱ�ݵģ����磺

������ʧ��������1����ǰ�ڵ�����80%��class 1��20%��class 0����һ�ַ��ѷ�ʽ�õ���$A_L$��100%����class 1��$A_R$��54%��class 1����Ȼ��ÿ���ӽڵ��ϣ�Ϊ��ʹ������С��$\tau(A_L) = \tau(A_R) = 1$�����������ܷ��ղ�û�м��٣�ԭ���ִ�Ļ��Ƿִ��ˣ����������ϵ�ѡȡ��׼���ܾͲ���ѡ���ַ��ѷ�ʽ�����������ķ�����������ģ����ܼ򵥷�����ʵ������У��ڷ��ѵ�һ���ڵ�ʱ�����Ҳ��������ַ��Ѹ��õķ�ʽ��

����Ҫ���ǣ����շ����½�ȥѡ��ķ����������ܲ���������Ҫ�ġ�������һ������ʹ��2���ڵ�Ĵ��ȷֱ�Ϊ85%-50%������һ������70%-70%��������С�Ļ����ǿ��ܻ�ȥѡ����ߣ���ʵ�����Ǹ�Ը��ѡ��ǰ�ߡ���Ϊ��������һ���ڵ���Ч�����á�

������û����㷨��������ȫ�������ĵ��ǣ��ҵ����ŷ��ѡ������������ں�ʱ��rpart����ĳ�֡�������(impurity)��������ÿ���ڵ㡣���õ����ֲ����ȶ����ǡ���(information index)��: $f(p) = -p log(p)$�͡�Gini index����$f(p) = p(1 - p)$�������ȵļ��㷽ʽ���ǣ�
$$I(A) = \Sigma_{i=1}^C f(p_{iA})$$
������rpart�У�ʹ�������½����ķ��Ѿ������ŷ��ѷ�ʽ
$$\Delta I = P(A)I(A) - P(A_L)I(A_L) - P(A_R)I(A_R)$$

#### ������ʧ����
����ղ���������ʾ������ֻ����ʧ�������������������ȫ������ʧ����Ҳ���á���2��������ʧ�������ַ���generalized Gini index��altered priors����rpart�в��ú�������չԭGini�����ȵļ��㡣

1.generalized Gini index

������Ҫ������ôһ������������һ������A���㲻֪����ôָ��ÿ�����������������ֻ��������µķ�����ÿ������������鵽�ĸ�����$p_i$���������ָ�����ʱҲ����${p_1,\, p_2,\, ... p_C}$�����ĸ���ȥָ������ô��ʱ��ġ�������ʧ�����ǣ�
$$G(A) = \sum_i \sum_j L(i,j)p_{iA} p_{jA}$$
��Ҳ���԰�������һ��generalized�Ĳ����ȡ���ʵ�ϣ���$C = 2$ʱ��$G(A) = L(0, 1)p(1 - p) + L(1, 0)p(1 - p)$�����$L(0, 1) == L(1, 0)$����ʱ���˻���ԭ����gini�������ˡ�

�����ڵ�$G(A)$ȥ�滻ԭ����$I(A)$�����ǾͰ���ʧ�����ӵ��˲����ȼ����С�

2.Altered priors
��Generalized Gini index������ͬ��������ͨ���޸��������$\pi_i$�ļ��㷽ʽ�����´���$P(A)$������ʧ������չ�������ȼ����еġ�
���Ȼ���һ��$R(A)$�Ķ��壺
$$\begin{eqnarray}
& R(A) &= \sum^C_{i=1} p(i|A)L(i, \tau(A)) \\
& & = \sum^C_{i=1} \pi_i L(i, \tau(A)) (n_{iA}/n_i)(n/n_A)
\end{eqnarray}$$

�������$\tilde{\pi}��\tilde{L}$��ʹ��
$$\begin{eqnarray}
\tilde{\pi_i}\tilde{L}(i,j) = \pi_i L(i, j) & & \forall i, j \in C
\end{eqnarray}$$
���Ҫ��$\tilde{L}$��0-1��ʧ���������ȣ���ô$\tilde{\pi}$���ܹ���ʾ�ɣ�
$$\tilde{\pi} = \frac{\pi_i L_i}{\tilde L(i, j)}$$
��$\tilde{\pi_i}$����$P(A)$�е�${\pi_i}$�����¼��㲻���ȣ��ͽ���ʧ������չ�������ȼ������ˡ�

### ��cart��/rpart�㷨Ԥ��ˮ��Ũ��

���ȣ����ǹ���ˮ��a1�Ļع���ģ�ͣ�

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

![plot of chunk a1-cart��ʾ��](figure/a1-cart��ʾ��.png) 

�������ľ���������£�

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

ÿ����ÿ�еĺ������£�

1. node)���ڵ���
2. split����������
3. n���ڸ��ڵ��У����Ϸ�����������������Ҳ��֮Ϊ�ýڵ㸲�ǵ�������������root�ڵ㸲�ǵ�����������������������ߵ�һ���ڵ������������PO4>=43.818����������147����
4. deviance��$deviance = \Sigma_i(y_i - \bar{y})^2, \,\,y_i$��ʾÿ��������yֵ
5. yval����ʾÿ���ڵ��Ԥ��ֵ�����Ǹýڵ㸲��������yֵƽ��

��rpart�����У�����3��������ֹͣ������������ֹ�������ع���ϣ��ֱ��ǣ������rpart.control����

1. cp��complexity parameter����devianceΪ�������$deviance(A)*cp >= deviance(A) - deviance(A_L) - deviance(A_R)$����ô�ڵ�A��ֹͣ����
2. minsplit���ڵ㸲�ǵ�������������������������ֵ���ڵ�ֹͣ����
3. maxdepth������������

���⣬��rpart�У�������cross-validation�����ݣ����ǿ��Ը�����Щ���ݣ�ͨ��1-se��������ȡ���ŵ�cp������


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


���У�"rel error"���ǵ�ǰ����deviance���Ե�һ��������deviance����"xerror"��"xstd"����10-fold������֤��ƽ��deviance�ͷ��ˮ�����ʵ����һ���ܺõ����ӣ���Ϊ��������չʾ��"rel error"��"xerror"����Сֵ����ͬһ�����ϵ��������˵������"rel error"��С�����ϣ�Ҳ����������������ϣ������˹���ϣ�����ѵ�����ϱ��ֺܺã����Ƿ����Խϲ��ô�����ҵ��Ƚ�׼ȷ��ͬʱ������Ҳ�ȽϺõ����أ�"1-se"�����ṩ������һ��˼·��

1. ��cptable��Ѱ��xerror��С��һ����
2. ��������� xerror + se*xstd ��Ϊ�����̵���С��tol err
3. ���ϵ���Ѱ��xerrorС��tol err�ĵ�һ������Ҳ������򵥵��ǿţ������������cp������Ϊ�ü���cp����
4. ���øղŻ�õ�cp�����ü�ԭ������

����ʵ���㷨���£�

```r
rt.prune <- function(tree, se = 1, verbose = T, ...) {
    if (ncol(tree$cptable) < 5) 
        tree else {
        lin.min.err <- which.min(tree$cptable[, 4])
        if (verbose & lin.min.err == nrow(tree$cptable)) 
            warning("Minimal Cross Validation Error is obtained  at the largest tree.\n Further tree growth   (achievable through smaller ��cp�� parameter value),\ncould produce more accurate tree.\n")
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

������Ԥ��ģ�;ͽ����ˡ�����6��ˮ���Ԥ��Ҳ����ˡ�
