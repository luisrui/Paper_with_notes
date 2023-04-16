## MMKG Construction and Application

对于MMKG：

+ Construction of MMKG:
  + from images to symbols : label images with symbols in KG
  + from symbols to images : ground symbols in KG to images
+ Application of MMKG:
  + In-MMKG applications: 处理MMKG本身的性能或整合的问题
  + Out -of-MMKG applications: MMKG处理的多模态问题

##### MMKG有两种表示形式：

+ (s,  r,  t)三元组： s对于t具有r这个关系

+ (s, p, o)三元组：  s具有p这个属性，属性值为o

主流： 

一种形式是把多模态数据作为attribute value，这种称为A-MMKG

![1681312413179](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681312413179.png)

一种形式是把多模态的数据作为实体， 这种被称为N-MMKG

![1681312647014](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681312647014.png)

## Multi-Modal Learning

### multi-modal representation

利用多模态互补学习特征表示

+ project the multiple modalities into a unified space
+ represent every single modal in its own vector space(satisfy certain constraints)

### multi-modal translation

把一个模态的实例转化为另一个模态的实例

### multi-modal alignment

找到不同模态之间的联系

### multi-modal Fusion

融合不同模态信息，实现预测

目前大部分使用attention机制去实现

### multi-modal co-learning

用其他模态的数据缓解某一模态数据不足的问题，用过校准调节的方式

### Contruction

