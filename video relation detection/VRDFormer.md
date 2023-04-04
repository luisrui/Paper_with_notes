# VRDFormer: End-to-End Video Visual Relation Detection with Transformers

基于VidVRD（video visual relation detection）模型的，基于transformer-based模型的一个模型

relation instances: a subject, an object and their relationship(spatial and temporal locations of the subject and the object)

##### 现有的框架：

![1680426814663](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680426814663.png)

存在问题：

+  在tracklet pair的生成中，时空上下文没有得到很好的利用 
+ 目标检测，跟踪和关系分类是高度相关的，但是在之前的工作中这些任务是分开训练的

## Methodology

每一个关系实例被记录为一个五元组：![1680507550593](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680507550593.png)

triplet classes of subject, relation and object while the last two denote tracklets of the subject and the object between the start and end timestamp $t_1$ and $t_n$ in the video

后面两个是包含了n个时刻subject或object的bbox位置

### Framework

+ video encoding module
  + encodes a video into a sequence of frame-level feature maps
  + CNN backbone and a multi-layer transformer CNN 负责提取局部信息，trans负责融合全局信息

+ query-based relation instance generation module
  + process the encoded feature maps frame-by-frame and generate relation instances in a autoregressive manner
    + frame-level object pair detection
    + tracklet pair updating
    + relation classification
  + overall pipeline: 

![1680524824042](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680524824042.png)

#### Frame-level object pair detection

+ ###### 静止的循环的query:

  + 循环的收集时空内容在之后的帧数中去跟踪先前的物体对： static queries and recurrent queries
    + static queries: 在每一帧的transformer层中固定长度
    + recurrent queries: 在每一帧的transformer层中动态长度
    + 两种queries结合在一起，输入进decoder

+ ###### Transformer Decoder:

  + multi-layer transformer with attentions: self- attention and cross-attention(这里感觉不是很懂)

+ ###### Prediction Heads:

  + 五个MLP检测头，subject bbox, object bbox, subject class probability, object class probability, interactiveness probability

#### Tracklet Pair updating

###### Initialization of new tracklet pairs:

(问题：这个置信度是根据每个静止query来计算的，那就是一开始输入的阶段？)

我的理解是第一层transformer输出的tracket pairs， 计算一个置信度，把置信度高于阈值的queries的预测结果和相应的embedding结果初始化在memory bank中的tracklet pairs中， 输出的这些embedding会作为循环query存到下一个帧中

###### Expansion of Existing Tracklet Pairs 

对于每个循环的query，都能先计算他们的置信度，把低于阈值的置信度，(<font color='red'>那如果是subject class不一样呢</font>)或者预测的object class和本身对应的tracklet pair不一样，inactivate这个query. 有一个暂存的机制， 等待$T_{re}$帧数在丢弃，在这个暂缓中，如果一个静止的query预测的so类别和这个暂存的query一致，而且IoU位置大于一个阈值，那就恢复这个循环query，放到下一帧中，用线性插值把暂缓时间这个query对应的predictions填充

#### Relation Classification

参考了 《Graph R-CNN for Scene Graph Generation》 

用两个transformer去整合$D_t$,用一个learnable的token整合到$D_t$里面去，添加时间位置的编码，把这个添加的token放入MLP中预测一个关系概率$p_t^r$

## Training Process

### Object detection

每次在短时段内sample两帧， 用bipartite matching的方法比较predictions和gt

具体方法：

在这个任务中，gt为每帧被标注的物体对

使用Hungarian Algorithm找到

![1680588074471](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680588074471.png)

![1680588115827](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680588115827.png)

![1680588164258](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680588164258.png)

