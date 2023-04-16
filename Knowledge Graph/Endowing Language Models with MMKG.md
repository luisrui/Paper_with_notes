# Endowing Language Models with MMKG Representations 

paper address: https://arxiv.org/pdf/2206.13163.pdf

这篇文章比较了tuple-based 和 graph-based representation learning的编码方法

使用VisualSem KG数据集，在NER(Entity Recognition)和VSD(crosslingual visual verb sense disambiguation)

### Hybrid method

在这篇文章中，使用一种混合方式把基于元组的编码方式和基于图的编码方式混合：

对于一个triplet，给定从图神经网络生成的隐藏状态$h_i$, $h_j$，使用DistMult的方式计算三元组的分数

![1681485735753](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681485735753.png)

replacing $v_i$, $v_j$ with $h_i$, $h_j$

### Multimodal Node Features

##### 对文本信息的编码

<font color='red'>T5</font>使用LASER网络对语义的gloss进行编码(每个语义信息编码长度为1024)，对于一个结点的整体文本编码用所有gloss编码的均值来表示![1681620292971](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681620292971.png)

##### 对图片的编码

<font color='red'>SWINT</font>使用在ImageNet上预训练的ResNet-152进行编码(图片信息编码长度为2048)，对于一个结点的整体图片编码用所有的图片编码的均值来表示![1681620780712](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681620780712.png)

##### 在图中的Neural Message Passaging 

<font color='red'>(Simple HGN)</font>

###### Node Gating

分为单独的文本编码，单独的图像编码，和同时的文本和图像编码

###### Edge Gating

分为单独的文本编码，单独的图像编码，和同时的文本和图像编码

#### 实验部分：

下游evaluation任务：Link Prediction

下游任务没怎么看懂，两个任务

