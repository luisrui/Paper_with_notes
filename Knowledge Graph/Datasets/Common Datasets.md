## Common Datasets

![1681477454328](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681477454328.png)

#### VisualSem (MMKG)  https://github.com/iacercalixto/visualsem

Covers a subset of Wikipedia and WorkNet entities

![1681477482919](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681477482919.png)

总结：

+ 89896个结点

+ 13中视觉关系

  ​	  *is-a*, *has-part*, *related-to*, *used-for*, *used-by*, *subject-of*, *receives-action*, *made-of*, *has-property*, *gloss-related*, *synonym*, *part-of*, and *located-at*. 

  对于图谱中关系分布不平衡，related to这种关系达到了82%

+ 1.5M个元组

+ 1.3M个注释，最多可以提供14种语言

+ 930k张关于结点的图片(938100, 一张图平均10.4个images)

a high-quality knowledge graph(KG) which includes nodes with

+ 多种语言解释
+ 多张解释性图片 
+ 视觉相关关系



#### WN9-IMG  https://github.com/thunlp/IKRL

在这篇文章中第一次构建： https://arxiv.org/pdf/1609.07028.pdf

母体数据集： WN18

The triple part of WN9-IMG is the subset of a classical KG dataset WN18 

![1681539378616](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681539378616.png)

总结：

+ 63225张从ImageNet选中的图片

+ 9种relations，6555个entity

选用了在WN18数据集中 含有图片的entity作为triples，分成了train test valid三组

调查:

给的文件里，图片的索引格式是 ImageNet的格式， 在使用中需要用AlexNet预训练获得embedding



#### FB-IMG  https://fileserver.ukp.informatik.tu-darmstadt.de/starsem18-multimodalKB/ 

第一次在这篇文章构建 https://download.visinf.tu-darmstadt.de/papers/2018-starsem-mousselly-sergieh-knowledge-preprint.pdf

母体数据集：FB15K-237  https://github.com/thunlp/OpenKE

在网站上存储的是VGG19或者VGG128编码的图片和用Glove编码的语义

总结：

+ 11757个entity， 对于每个entity从网络上爬取100张图片后用pagerank排序，取前十张图片补充数据集
+ 1131种关系
+ 107570张图片

调查：

有图片和文字的编码，但是索引到真实数据的方法还没找到



#### MarKG https://github.com/zjunlp/MKG_Analogy

在这篇文章构建  https://arxiv.org/pdf/2210.00312.pdf

母体数据集： wikidata

这个是主要针对类比推理任务的

总结：

+ 11292个entity
+ 192钟关系
+ 76424张图片