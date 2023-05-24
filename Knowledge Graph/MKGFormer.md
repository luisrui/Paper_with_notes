# MKGFormer

争对下游任务：MNER MRE 

现在各种编码模型的问题：

+ 对于文本需要不同任务下各种预训练编码方式，对于图片需要各种任务下各种预训练编码方式，所以需要有一个统一的模型对各种任务都能编码(Architecture Universality)
+ 对于视觉信息中通常存在无关的噪声，而且使用目标检测方法得到的图片信息通常和语义信息不符合(Modality contradiction)

总体来说，对于图片信息采用ViT，对于文本信息采用BERT，在这些模型的最后几层上对多模态信息进行建模

![1684679723655](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1684679723655.png)

架构三个部分：T-encoder， V-encoder，M-encoder

#### T-encoder

找到语言文本中基本的语法和词汇信息

在Bert的前$L_T$层作为visual encoder用来提取图片信息

![1684682551168](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1684682551168.png)

$X_{wd}$ 是一个token sequence，$T_pos$是其position embedding

#### V-encoder

找到图片信息中的基本视觉信息

在ViT的前$L_v$层作为visual encoder用来提取图片信息

![1684682007087](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1684682007087.png)

把 o 张图片的patched embeddings连接起来，得到$X_{pc}$, 其对应的position embedding layer的特征为$V_{pos}$，之后经过$L_v$层的Transformer

#### M-encoder

对语言和文本信息进行融合建模

总体框架： PGI CAF

![1684683746051](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1684683746051.png)

#### PGI predix-guided interaction module

![1684683854897](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1684683854897.png)

在transformer传播中，把textual的训练过程中的keys和values传输到images上

![1684684000913](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1684684000913.png)

![1684684010512](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1684684010512.png)

第一块是自注意力 第二块是交互注意力

#### CAF correlation-aware fusion module

目的：减少噪声印象

在PGI模块输出visual vectors和textual vectors之后，先计算两个模态信息之间的相似性矩阵 $S = x^tx(^v)^T$

x^t 为textual vectors， x^v为visual vectors

![1684684655185](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1684684655185.png)

$Agg_i(x^v)$代表了对于第i个文本token的similarity-aware aggregated visual representation

![1684684768258](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1684684768258.png)

把这个相似度感知融合视觉表示矩阵作为一个信息传入到后面的FFN中