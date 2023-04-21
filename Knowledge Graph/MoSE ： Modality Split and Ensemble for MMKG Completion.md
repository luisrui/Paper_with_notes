## MoSE ： Modality Split and Ensemble for MMKG Completion

潜在问题：

TCR（tight coupling relation）

Implicit TCR: 把一个结点的多模态信息都融合成一个编码，然后根据编码学一种同一个关系表征

+ TransAE，RMSE 忽略了模态的信息对于最后的结果是否有关

Explicit TCR: 不通过融合，直接根据多模态的不同信息生成一种关系表征 

+ DKRL， IKRL 不同模态之间语义关系的矛盾没有被消除

现存MKGC方法通常对于关系编码分享同样的模态关系的矛盾：

###### 多模态中一种模态之间的关系通常在另一种模态中表示不出来

![1681821036359](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681821036359.png)

###### 模态之间的差异通常被忽略

## Decoupled TCR and modality-split MKG

![1681872438456](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681872438456.png)

Encoder: ComplEX作为KGC Decoder，ViT和RSME结合来编码视觉信息，BERT来编码文本信息

这篇文章的启发是：

单图片给结点的信息是高度相关的，但是文字提供的信息对于一类实体都相关，所以在KGC任务中，文字提供的信息比图片更加丰富

同时这篇文章提出，多图片信息可能会提供side information，增强图片这个模态对于任务的帮助

