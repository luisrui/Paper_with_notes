# MKGFormer

争对下游任务：MNER MRE 

现在各种编码模型的问题：

+ 对于文本需要不同任务下各种预训练编码方式，对于图片需要各种任务下各种预训练编码方式，所以需要有一个统一的模型对各种任务都能编码(Architecture Universality)
+ 对于视觉信息中通常存在无关的噪声，而且使用目标检测方法得到的图片信息通常和语义信息不符合(Modality contradiction)