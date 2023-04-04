# Object as Points: Centernet

anchor-free method

![1680347323521](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680347323521.png)

## Preliminary

输入是一个w ** h ** 3的图片，想要输出一个带有关键点的热量图的0,1指标(<font color='red'>这个是方框的置信度吗</font>)

0为背景的中心点

1为detected keypoint

![1680322052267](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680322052267.png)

采用三种方式来生成这样的热量图：(每一篇单独出一个论文精度)

+ a stacked hourglass network
+ ResNet
+ DLA（deep layer aggregation）                                                                                                                            

对于一张train image，每一个gt中的中心点，先低分辨率的整除stride，用一种方式投影到heatmap上：

Gaussian Kernel $Y_{xyc}$

![1680322806960](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680322806960.png)

关于关键点检测的一个loss函数：

![1680323330624](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680323330624.png)

#### Cornernet中的启发

 [目标检测算法之CenterNet_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1r44y1a75j/?spm_id_from=333.337.search-card.all.click&vd_source=d5c3a818b9f95b13d562b149dcf2dc96) 

对于$\sigma_p$来说，如何计算是具有别的anchor-free模型启发的：
$$
\sigma_p = \frac{r}{3}
$$


三种情况

红框为ground truth 绿框为预测值

![1680347984288](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680347984288.png)



![1680347993490](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680347993490.png)

![1680348006113](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680348006113.png)

N为一张图片中中心点个数，alpha和beta都是focal loss中的参数

关于中心点偏移，单独设计了一个离散化误差

![1680323747678](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680323747678.png)

## Objects as Points

预测完了中心点，下一步是预测尺寸

![1680343958461](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680343958461.png)

![1680345285620](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680345285620.png)

## Inference Time

 首先针对每个类别独立地从热力图中提取出峰值点。对于每个位置，如果它的值大于等于相邻8个像素的值，则认为它是一个峰值点。然后，从所有的峰值点中选出置信度最高的前100个点，作为该类别的中心点。 

具有新的参数：

![1680346962754](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680346962754.png)

![1680347041744](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680347041744.png)