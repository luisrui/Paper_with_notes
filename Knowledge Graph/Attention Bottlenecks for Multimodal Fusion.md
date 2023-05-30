## Attention Bottlenecks for Multimodal Fusion

![1684595520049](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1684595520049.png)

设计了一个新架构，限制了在latent units中交叉模态信息的流动，避免了二次标度代价

在普通的transformer架构中，一个token经过了LN MSA(multi-headed self attention)的过程，即

a transformer layer: $z^{l+1} = Transformer(z^{l})$
$$
y^l = MSA(LN(z^l)) + z^l \\
z^{l+1} = MLP(LN(y^l)) + y^l \\
MSA(x) = Attention(W^QX, W^KX, W^VX)
$$
在本文中，定义了一个多头交叉注意力机制Multi-headed Cross Attention, 输入是两个tensors， X为query， Y为Key & Value
$$
MCA(X, Y) = Attention(W^QX,W^KY,W^VY)
$$
![1684653316183](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1684653316183.png)

在多模态视频中，分解为图片模态和声音模态，在架构中添加中间的FSN(fusion bottleneck)

![1684655276843](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1684655276843.png)

![1684655283548](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1684655283548.png)

两个模态rgb和spec，只能在bottleneck里面交换信息

由于在多层transformer中，low-level的信息只是一些边角信息，到high-level才是具体的semantic concepts，一开始的边角信息不同模态的差异性很大，所以没有必要fusion

在本文中采用了层次设计，开始使用vanilla self-attention，每个模态单独训练，之后把latent tokens拼接在一起![1684655952813](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1684655952813.png) (前 Lf层)之后采用fusion with modality-specific parameters每个模态单独参数进行训练(后L-Lf层)

#### 实验部分

![1684659260129](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1684659260129.png)

主要突出一个Lf超参数的调节 可以看出来MBT比vanilla Fusion更加集中attention 区域更小更精细

