�����ھ�Ļ�������
========================================================

�ⲿ�����ݽ���Ҫ����һЩ�����ھ�Ļ���������**����Ԥ����̽�������ݷ�����Ԥ��ģ��**�ȡ�Ϊ���о��ķ��㣬������һ��ʵ�ʵ����ݼ�Ϊ�����о���ķ������������������ؼ�[linked phrase](http://www.dcc.fc.up.pt/~ltorgo/DataMiningWithR/datasets2.html)��

**Preparation for running, Loading the Data into R:**


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
data(algae)
```

*setwd* ���ù���Ŀ¼��ԭ���ݵ�����ԡ�data.frame������ʽ���ڣ�����*col.names* Ϊԭ���ݼ���ÿһ������˱������ƣ����������ݼ��е�δֵ֪��XXXXXXX�����ԡ�NA�����б�ʶ�����⣬R����һЩ�����ķ������Զ�ȡ�ı���ʽ�����ݡ�

**Data Visualization and Summarization��**


```r
summary(algae)
```

```
##     season       size       speed         mxPH           mnO2      
##  autumn:40   large :45   high  :84   Min.   :5.60   Min.   : 1.50  
##  spring:53   medium:84   low   :33   1st Qu.:7.70   1st Qu.: 7.72  
##  summer:45   small :71   medium:83   Median :8.06   Median : 9.80  
##  winter:62                           Mean   :8.01   Mean   : 9.12  
##                                      3rd Qu.:8.40   3rd Qu.:10.80  
##                                      Max.   :9.70   Max.   :13.40  
##                                      NA's   :1      NA's   :2      
##        Cl             NO3             NH4             oPO4      
##  Min.   :  0.2   Min.   : 0.05   Min.   :    5   Min.   :  1.0  
##  1st Qu.: 11.0   1st Qu.: 1.30   1st Qu.:   38   1st Qu.: 15.7  
##  Median : 32.7   Median : 2.67   Median :  103   Median : 40.1  
##  Mean   : 43.6   Mean   : 3.28   Mean   :  501   Mean   : 73.6  
##  3rd Qu.: 57.8   3rd Qu.: 4.45   3rd Qu.:  227   3rd Qu.: 99.3  
##  Max.   :391.5   Max.   :45.65   Max.   :24064   Max.   :564.6  
##  NA's   :10      NA's   :2       NA's   :2       NA's   :2      
##       PO4             Chla              a1              a2       
##  Min.   :  1.0   Min.   :  0.20   Min.   : 0.00   Min.   : 0.00  
##  1st Qu.: 41.4   1st Qu.:  2.00   1st Qu.: 1.50   1st Qu.: 0.00  
##  Median :103.3   Median :  5.47   Median : 6.95   Median : 3.00  
##  Mean   :137.9   Mean   : 13.97   Mean   :16.92   Mean   : 7.46  
##  3rd Qu.:213.8   3rd Qu.: 18.31   3rd Qu.:24.80   3rd Qu.:11.38  
##  Max.   :771.6   Max.   :110.46   Max.   :89.80   Max.   :72.60  
##  NA's   :2       NA's   :12                                      
##        a3              a4              a5              a6       
##  Min.   : 0.00   Min.   : 0.00   Min.   : 0.00   Min.   : 0.00  
##  1st Qu.: 0.00   1st Qu.: 0.00   1st Qu.: 0.00   1st Qu.: 0.00  
##  Median : 1.55   Median : 0.00   Median : 1.90   Median : 0.00  
##  Mean   : 4.31   Mean   : 1.99   Mean   : 5.06   Mean   : 5.96  
##  3rd Qu.: 4.92   3rd Qu.: 2.40   3rd Qu.: 7.50   3rd Qu.: 6.92  
##  Max.   :42.80   Max.   :44.60   Max.   :44.40   Max.   :77.60  
##                                                                 
##        a7      
##  Min.   : 0.0  
##  1st Qu.: 0.0  
##  Median : 1.0  
##  Mean   : 2.5  
##  3rd Qu.: 2.4  
##  Max.   :31.6  
## 
```


*summary* �������԰������ǵõ����ݼ�����������һЩ����ͳ�����������������������Ƶ���ֲ���������˳������������ֵ����λ�����ٷ�������ֵ�ȡ���Щͳ�������ܹ������Ƕ��ڱ����ķֲ���һ���������˽⣬���磬���ǿ��Թ۲쵽�����ռ����������＾�ռ��Ķ࣬����oPO4�ľ�ֵ������λ�������ܻ������ƫ�ֲ�������Щ��Ϣ������ͼ�θ��õ�չ�ֳ�����

**Data inspection by graphes:**
- **Histograms**


```r
hist(algae$mxPH, prob = T, main = "Histogram of maximum pH value", xlab = "mxPH", 
    ylab = "Density", ylim = 0:1)
lines(density(algae$mxPH, na.rm = T))
rug(jitter(algae$mxPH))
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 


*hist* �����趨����*prob=T* �õ�����mxPH�ĸ���ֱ��ͼ������*line* ������ֱ��ͼ����ƽ�������Կ���mxPH��ȡֵ���Ƴ���̬�ֲ���Ϊ�˽�һ���鿴���ݵ���ʵ�ֲ��������*rug* ��mxPH��ʵ��ֵ��ӵ�ͼ�У������ܱ�ʶ�����۲�õ�����С��6����������Զ�������������ݣ���Щ���ݿ��ܴ���һЩ���⣬��ʾ�����ں�����������Ҫע�⡣

- **Boxplots**


```r
boxplot(algae$oPO4, boxew = 0.15, ylab = "Orthophosphate(oPO4)")
rug(jitter(algae$oPO4), side = 2)
abline(h = mean(algae$oPO4, na.rm = T), lty = 2, col = "red")
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4.png) 


*boxplot* ������oPO4������ͼ��Ϊ�˽�һ���������ݵķֲ������������oPO4������ֵ��ӵ�����ͼ�У�ͬʱ��*abline* ������ͼ����ӹ���oPO4��ֵ���Ժ�ɫ��ǵ�ˮƽ���ߡ���ֵ��λ����λ�ߵ��Ϸ�������ϱ�Ե�ߡ����ķ�λ�ߡ���λ�ߡ����ķ�λ�߼��±�Ե��֮��ľ��룬���Կ����󲿷�������oPO4ֵ��ƫ�͵ģ�����һЩ�۲�ֵ�Ƚϸߣ������������ˮƽ��ʹ�����������ƫ�ֲ�������ЩoPO4ֵ�쳣�ߵ������������쳣�¼���
> ��Щʱ����Ҫ�о�һ�������ķֲ�������һ�������������ԣ�����ͨ������ͼ�ν���չʾ��R��ͼ�ΰ�*lattice* �ṩ�˺ܶ໭ͼ�߼����ߡ�

- **Conditioned Boxplots**


```r
library(lattice)
bwplot(mxPH ~ size, data = algae, xlab = "River Size", ylab = " maximum pH value ")
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5.png) 


*bwplot* ��size�Ĳ�ͬ�������ֳɲ�ͬ����ٷֱ��������mxPH ������ͼ�������ȽϷ������Է������Խϴ������������mxPHֵ��Ը���һЩ����������ͼ�β��ǽ���������������ߵ����������ӣ��������£�


```r
library(Hmisc)
```

```
## Error: there is no package called 'Hmisc'
```

```r
minO2 <- equal.count(na.omit(algae$mnO2), number = 4, overlap = 1/5)
stripplot(season ~ mxPH | minO2, data = algae[!is.na(algae$mnO2), ])
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6.png) 


���ȶ������������зֽ⣬*equal.count* ������mnO2ƽ���ֳ�4�飬������֮����1/5���ص��������е�ȱʧֵ��*na.omit* ����ȥ�����ٶ�ÿ����������չʾmxPHֵ�ڲ�ͬ���ڵķֲ������ȱʧֵ��Ҫȥ�����������·������·������Ϸ������Ϸ���˳�򣬲�ͬ�����������mnO2ֵ�������ӡ����Թ۲켾����mnO2�������ض�mxPH��Ӱ�죺����autumn��������mnO2ƫ�ߣ���mxPH�ڲ�ͬ���ڵķֲ�״��δ��mnO2��Ӱ�죬mxPH�ڲ�ͬ���ڵı仯Ҳ����˵�����������ؿ�����mxPH�޹ء�


**Outliers identification**


```r
plot(algae$mxPH, xlab = "maximum pH value")
abline(h = mean(algae$mxPH, na.rm = T), lty = 1)
abline(h = mean(algae$mxPH, na.rm = T) + sd(algae$mxPH, na.rm = T), lty = 2)
abline(h = median(algae$mxPH, na.rm = T), lty = 3)
clicked.lines <- identify(algae$mxPH)
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7.png) 

```r
# algae[clicked.lines,]
```


*plot* ����������mxPH��ɢ��ͼ��*abline* �����ֱ��Թ۲�ֵ�ľ�ֵ����ֵ+��׼���λ����ˮƽ�ߣ����������ݽ��л��֡����������ͼ�е�����㣬*identify* �����ܹ�ʹR��¼������ݵ��λ�ò����浽����clicked.lines�У����������ݵ��ȫ����Ϣ�����ṩ��һ�ֽ���ʽ��Ѱ���쳣��ķ�����

**Strategies for Unknown values**

-*Removing the observations with unknown values* 
```
algae <- na.omit(algae)
```

-*Filling in the unknowns with the most frequent val-ues* 

```
algae[48,��mxPH��] <- mean(algae$mxPH,na.rm=T)
```
��48������algae[48,]��mxPH����ֵ�ǿհ׵ģ�������mxPH�����������еľ�ֵ���档���⣬����һЩ�����Ĵ���ʽ�����£�

-*Filling in the unknown values by exploring correlations*
-*Filling in the unknown values by exploring similari-ties between cases*

**Obtaining prediction models** 
�ع���ģ��Ԥ��
--------------------------------------------------------

```r
library(rpart)  #����rpart��Ӱ������лع���ģ�͵�ʵ��
library(DMwR)  #����DMwR�������溬������data
data(algae)
algae <- algae[-manyNAs(algae), ]
rt.a1 <- rpart(a1 ~ ., data = algae[, 1:12])  #rpart�������ڻ�ȡ�ع���(Regression Tree)ģ��
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

�������ݵ�ÿһ�зֱ����(�ڵ��ţ��ڵ��֧������������ڵ��ϵ��������������ƽ��ֵ��ƫ�����a1�ĳ���Ƶ��)
```
For example,
2) PO4>=43.818 147 31279.120  8.979592
�ڵ���Ϊ2��
��root�ڵ���PO4>=43.818�ľ͵��ڵ�2��ߣ�
�ڵ�2����147��ˮ����
����a1���������(PO4>=43.8)�����ƽ��ֵƫ��Ϊ31279.120��
����a1����������³��ֵ�ƽ��Ƶ����7.49

��������뽨��һ���ع�����Ԥ��ĳ��ˮ����Ƶ�ʣ�ֻҪ�Ӹ���㿪ʼ���ݶԸ�ˮ������Ľ����׷��ĳ����֧��ֱ��Ҷ�ӽڵ㡣Ҷ���Ŀ�������ƽ��ֵ��������Ԥ��ֵ,����ͼ��ʾ
```


```r
prettyTree(rt.a1)  #�õ��ع�����ͼ�α�ʾ
```

![plot of chunk plotRT](figure/plotRT.png) 


```
����ʹ��rpart�������������ڹ������Ĺ����У���������������ʱ�������̾�ֹͣ�������е�����������
1) ƫ��ļ���С��ĳһ�������Ľ���ֵ
2) ������е���������С��ĳ����������ʱ
3) ��������ȴ���һ�������Ľ���ֵ
�����������ֱ���rpart����������������ȷ��(cp,minsplit,maxdepth)��Ĭ��ֵ�ֱ���0.01��20��30
```

ģ�͵�����
-------------------------------------

һ�ֶ����ķ���������ƽ���������(MAE)
  * ��һ������ȡ��Ҫ����ģ��Ԥ�����ܵ�Ԥ��ֵ����һ����predict()����Ԥ��   

```r
rt.predictions.a1 <- predict(rt.a1, algae)
```

  * �ڶ�����������ƽ���������


```r
(mae.a1.rt <- mean(abs(rt.predictions.a1 - algae[, "a1"])))
```

```
## [1] 8.481
```


�����������ֵ�󣬻��ǲ�֪����ô֪��ʲô�÷ֱȽϺã�ʲô�÷ֱȽϲ��������������Ҫһ����ֵ��   
���Ǳ�׼�����ƽ���������(NMSE)��   
ͨ������Ŀ�������ƽ��ֵ����Ϊ��׼ģ��:

```r
(nmse.a1.rt <- mean((rt.predictions.a1 - algae[, "a1"])^2)/mean((mean(algae[, 
    "a1"]) - algae[, "a1"])^2))
```

```
## [1] 0.3546
```

```r
# NMSE��һ����ֵ����ȡֵ����ͨ��Ϊ0~1�����ģ�ͱ������������ģ�͵Ļ�׼Ԥ�⡣��
# NMSE������С��1������˵���ֵԽСģ��Խ�á�����1�Ļ�˵����ƽ��ֵģ�ͻ���
```


���ӻ��鿴ģ�͵�Ԥ��ֵ
-----------------------------------   
��������ɢ��ͼ   


```r
plot(rt.predictions.a1, algae[, "a1"], main = "Regression Tree", xlab = "Predictions", 
    ylab = "True Values")
# abline������һ��y=x������
abline(0, 1, lty = 2)
```

![plot of chunk plotError](figure/plotError.png) 

```r
# ������һ���������y=x��ֱ�ߣ�������е�Ԥ�ⶼ��ȷ�����е�Ȧ��Ӧ�������������ϡ�
# ��ʾtrue value = predicted value.
```


Cross Validation ѡ����ʵ�ģ��
------------------------------------------

��Ϊ����֮ǰ��Ԥ�ⶼ����ѵ�����Ͻ��еģ������õ���ģ���п��ܻ������ϣ����������Ҫ���ģ����λ��������Ԥ�����ܵĸ��ӿɿ��Ĺ��ơ�   
���Ȼ�ȡk��ͬ����С�����ѵ�������Ӽ�������ÿһ���Ӽ����ó�ȥ��֮�������k-1���Ӽ�����ģ�ͣ��õ�k���Ӽ����������ģ�ͣ����洢ģ������ָ�ꡣ   
```
�Ƚϳ��õ�kֵ��10
������ʱ�������������������k-fold cross validation�ܶ�δӶ���ȡ���ӿɿ���Ԥ��
�������Ƚ�������ģ�ͺ���
```


```r
cv.rpart <- function(form, train, test, ...) {
    m <- rpartXse(form, train, ...)  # �õ��ع���ģ��
    p <- predict(m, test)  # �����ģ���ڲ��Լ��Ͻ���Ԥ��
    mse <- mean((p - resp(form, test))^2)  # ����mse(�������)��Ϊ������
    c(nmse = mse/mean((mean(resp(form, train)) - resp(form, test))^2))
}

cv.lm <- function(form, train, test, ...) {
    m <- lm(form, train, ...)  # ���Իع�ģ�ͻ�ȡ
    p <- predict(m, test)  # ģ���ڲ��Լ���Ԥ��
    mse <- mean((p - resp(form, test))^2)  # ͬ��ʹ��mse��Ϊ��������ָ��
    c(nmse = mse/mean((mean(resp(form, train)) - resp(form, test))^2))
}
```

��R�����ṩexperimentalComparison�ĺ����������ǽ���cross validation�Ͳ�ͬģ��֮��ıȽϣ����´�����ʾ   

```r
# �����ĵ�һ��������ʾ��Ҫ�õ���dataset
# �ڶ���������һ�������������е�ÿ�������������ǵ�ģ��
# variant�����þ��ǲ������ǵ�ģ�ͱ���, se��ֵ��ʾ�������ֲ�ͬ�Ļع���ģ��
# ���һ������������Ҫ�ܶ��ٱ�cross-validation,������3�Σ�Ȼ��ÿ�η�10�ݣ��ֵ�ʱ��randomly�֣�������1234
res <- experimentalComparison(c(dataset(a1 ~ ., algae[, 1:12], "a1")), c(variants("cv.lm"), 
    variants("cv.rpart", se = c(0, 0.5, 1))), cvSettings(3, 10, 1234))
```

```
## 
## 
## #####  CROSS VALIDATION  EXPERIMENTAL COMPARISON #####
## 
## ** DATASET :: a1
## 
## ++ LEARNER :: cv.lm  variant ->  cv.lm.v1 
## 
##  3 x 10 - Fold Cross Validation run with seed =  1234 
## Repetition  1 
## Fold:  1  2  3  4  5  6  7  8  9  10
## Repetition  2 
## Fold:  1  2  3  4  5  6  7  8  9  10
## Repetition  3 
## Fold:  1  2  3  4  5  6  7  8  9  10
## 
## 
## ++ LEARNER :: cv.rpart  variant ->  cv.rpart.v1 
## 
##  3 x 10 - Fold Cross Validation run with seed =  1234 
## Repetition  1 
## Fold:  1  2  3  4  5  6  7  8  9  10
## Repetition  2 
## Fold:  1  2  3  4  5  6  7  8  9  10
## Repetition  3 
## Fold:  1  2  3  4  5  6  7  8  9  10
## 
## 
## ++ LEARNER :: cv.rpart  variant ->  cv.rpart.v2 
## 
##  3 x 10 - Fold Cross Validation run with seed =  1234 
## Repetition  1 
## Fold:  1  2  3  4  5  6  7  8  9  10
## Repetition  2 
## Fold:  1  2  3  4  5  6  7  8  9  10
## Repetition  3 
## Fold:  1  2  3  4  5  6  7  8  9  10
## 
## 
## ++ LEARNER :: cv.rpart  variant ->  cv.rpart.v3 
## 
##  3 x 10 - Fold Cross Validation run with seed =  1234 
## Repetition  1 
## Fold:  1  2  3  4  5  6  7  8  9  10
## Repetition  2 
## Fold:  1  2  3  4  5  6  7  8  9  10
## Repetition  3 
## Fold:  1  2  3  4  5  6  7  8  9  10
```

Ȼ�����ǶԽ������һ��summary,�����Ϳ��Կ���nmse��ֵ��

```r
summary(res)
```

```
## 
## == Summary of a  Cross Validation  Experiment ==
## 
##  3 x 10 - Fold Cross Validation run with seed =  1234 
## 
## * Data sets ::  a1
## * Learners  ::  cv.lm.v1, cv.rpart.v1, cv.rpart.v2, cv.rpart.v3
## 
## * Summary of Experiment Results:
## 
## 
## -> Datataset:  a1 
## 
## 	*Learner: cv.lm.v1 
##           nmse
## avg     0.8676
## std     0.2770
## min     0.4421
## max     1.6051
## invalid 0.0000
## 
## 	*Learner: cv.rpart.v1 
##           nmse
## avg     0.7617
## std     0.3019
## min     0.2550
## max     1.3212
## invalid 0.0000
## 
## 	*Learner: cv.rpart.v2 
##           nmse
## avg     0.7888
## std     0.3195
## min     0.1529
## max     1.3212
## invalid 0.0000
## 
## 	*Learner: cv.rpart.v3 
##           nmse
## avg     0.7806
## std     0.3393
## min     0.1701
## max     1.4599
## invalid 0.0000
```

��ֱ�۵����ǿ��Ի���ͼ��Ϊ���ӻ������

```r
plot(res)
```

![plot of chunk plotres](figure/plotres.png) 


