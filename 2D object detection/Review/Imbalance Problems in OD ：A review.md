## Imbalance Problems in OD ：A review

+ foreground-to-background imbalance: 正负样本严重不平衡
+ Class Imbalance
+ Scale Imbalance
+ Spatial imbalance
+ Objective imbalance

 ![img](file:///C:\Users\ASUS\Documents\Tencent Files\3437713406\Image\C2C\{3069023C-4ED5-3362-5D9E-17EA3BE8F8BC}.png) 

##### Class Imbalance

当一个类被过度表达了，就是比其他的类拥有更多的数据

+ foreground-background class imbalance

  正负样本严重不平衡

  + Hard Sampling Methods
  + + random sampling in RCNN family
    + hard-example mining methods: bootstrapping
      + 使用一小批的负样本进行训练一个初始化模型，然后使用负样本当模型失败，一个新的分类器就训练好了 sg. Single Shot Detector
    + IOU based sampling
  + Limit search space
    + 1ROI
  + Soft Sampling Methods: 不像hard sampling，这里所有的样本都根据相对重要性来训练，不会被丢弃
    + constant coefficients for foreground and background classes in YOLO
    + Focal Loss & Gradient Harmoniziing Mechanism(GHM)
    + PrIme Sample Attention(PISA)
  + Sampling-free methods
    + Residual Objectness scores
      + 只考虑正样本
    + AP loss & DR loss
  + Generative Methods
    + Adversarial-Fast-RCNN model
    + Generate Images 生成图像的一种方法
    + Positive RoI Generator: 通过给定的IOU生成一批正的RoI

+ Foreground-Foreground Class Imbalance

  + 数据集本身收集的数据不平衡
    + 用GAN等生成网络生成数据
  + 数据集中batch的数据不平衡
    + 采用概率采样的方法

+ 