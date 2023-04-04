# Feature Pyramid Networks

这是一个特征提取算法

Input: a single-scale image of an arbitrary size

Output: proportionally sized feature maps at multiple levels

## Bottom-up pathway

Feed-forward computation of the backbone ConvNet

用来计算一个包含不同大小的特征图的特征层次(scaling step of 2)

 C1，C2，C3和C4分别代表骨干网络不同层级的特征图 

1. C1是骨干网络最底层的特征图，通常大小为原始输入图像的1/4或1/8，用于检测小目标。
2. C2是骨干网络中第二层的特征图，通常大小为原始输入图像的1/8或1/16，用于检测中等大小的目标。
3. C3是骨干网络中第三层的特征图，通常大小为原始输入图像的1/16或1/32，用于检测大目标。
4. C4是骨干网络中第四层的特征图，通常大小为原始输入图像的1/32或1/64，用于检测最大的目标。

## Top-down pathway and lateral connections

自顶向下的路径和横向连接

首先以最底层的特征图C5作为起点，通过一个1×1卷积层生成一个粗略的特征图P5，作为最高分辨率的特征图。然后，从最高分辨率的特征图P5开始，通过反卷积（upsampling）将特征图的空间分辨率增加一倍（使用最近邻插值方法），得到一个更细粒度的特征图。接着，将上采样后的特征图与底层特征图C4通过元素级别的加法进行融合，得到更高分辨率、更具有语义信息的特征图P4。然后，继续对特征图P4进行上采样、融合操作，得到更高分辨率、更具有语义信息的特征图P3，以此类推，直到得到最精细的特征图P2。在每个融合操作之后，还会添加一个3×3的卷积层，以减少上采样带来的锯齿状失真和毛刺现象。 

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1679920018679.png" alt="1679920018679" style="zoom:100%;" />

问题(add extra levels没看，留一个坑，关于RetinaNet的)：

在整个前向传播的逻辑中，输入的是左边金字塔输入的C1：5构成的list，先通过lateral connection(把上一层的放到和下一层一样大)实现方框中操作，然后输出的是重新通过fpn_convs层的结果作为prediction，<font color='red'>这里我并没有看到实现那个1*1 conv的部分？直接上一层和下一层相加了</font>

