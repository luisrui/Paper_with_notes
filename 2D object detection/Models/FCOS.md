# FCOS: Fully Convolutional One-Stage Object Detection

Main point: proposal free network

## Reformulate object detection in a per-pixel prediction fashion

问题：

<font color='red'>在fcos中如果是一个negative sample，类别标记为0类背景类，这个背景类在那些检测模型中是必须的，在哪些是不必须的</font>

整体pipeline：

![1680090559135](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680090559135.png)

和anchor-based模型重要区别：

在anchor-based模型中，正样本是一个锚点和gt bbox的IoU大于一定阈值，才被认为是前景样本

在point-based模型中，像素点如果位于gt bbox中就被认为是前景样本，否则为背景样本

定义了一个4D real vector t* = $(l^*, t^*, r^*, b^*)$ 每一位分别代表从当前位置到bbox四条边的长度

![1680090415205](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680090415205.png)

一个预测位置如果落入多个bbox中，认为是ambiguous sample,选择最小的bbox区域作为模型的回归目标

foreground samples: 前景样本，图像中包含目标物体的样本

在原文中没有训练一个多类的分类器，而是对于每个类，都分别训练了一个二分类分类器

损失函数设计： focal loss + IoU loss

![1680100243483](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680100243483.png)

## Multi-level Prediction with FPN for FCOS

+ Fcos可以解决CNN提取特征最后一层大stride导致的最大可能召回率(BPR)较小的问题
+ 在gt bbox中重合方框的问题

FPN的多层级预测：

![1680102921238](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680102921238.png)

P5, P4, P3就是正常的FPN网络，P6, P7是P5经过一层卷积网络，stride为2依次生成的

判断逻辑（判定一个像素点是否为负样本）

 we firstly compute the regression targets l ∗ , t ∗ , r∗ and b ∗ for each location on all feature levels. Next, if a location satisfies max(l ∗ , t∗ , r∗ , b∗ ) > mi or max(l ∗ , t ∗ , r∗ , b∗ ) < mi−1, it is set as a negative sample and is thus not required to regress a bounding box anymore. Here mi is the maximum distance that feature level i needs to regress. 

<font color='blue'>利用不同分辨率的特征图来减少歧义性样本的数量</font>。 具体来说，对于特定的目标，由于其大小和形状的不同，它可能会在多个特征图级别上出现。如果在所有级别上都对目标进行检测，将会产生许多重叠的检测结果，这些结果之间存在很高的重合度。这些重叠的检测结果可能会导致多个正样本预测同一个目标，从而增加了模型的误差。 

 <font color='blue'>为了避免这种情况，FCOS使用了多级预测。对于每个目标，模型仅在与目标大小相匹配的最小级别上预测。 </font>

为了让不同的feature level回归的方框发小固定在不同的限制区域，把exp(x)改成了exp(si*x), 自动调整每一层的输出概率指数函数，Si可以学的

## Center-ness for Fcos

为了去除离gt bbox中心较远的中心生成的pred bbox，设计centerness方法

在分类层网络旁添加一个平行的centerness层，计算一个潜在像素点的中心程度

![1680104615161](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680104615161.png)

这个分数叠成一个BCEloss，加在原来的loss function上面，并且计算bbox分数是cls分数乘以这个中心度

## Coding Part

fcos_head.py

在代码中可学习的尺度变换层被设置成Scale

![1680178578649](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680178578649.png)

##### 这个FP16是什么处理？

 FP16（Half-Precision Floating Point）是指使用16位浮点数来表示浮点数的方法，相较于FP32（Single-Precision Floating Point）使用32位浮点数来表示浮点数，可以减小内存占用，提升计算速度。在深度学习中，由于模型参数较多，内存占用和计算速度是比较重要的考虑因素，因此使用FP16可以在一定程度上提高模型的训练速度。 

##### 没怎么搞懂每个尺度上的点是怎么获取的

 MlvlPointGenerator  这个函数去生成的

代码和论文有点差异 背景类指定的代码中是num.classes + 1, 论文中是0