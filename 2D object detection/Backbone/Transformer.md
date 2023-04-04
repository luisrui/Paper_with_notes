# Transformer 精读论文

 编码器-解码器的架构

![1680364302483](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680364302483.png)

左边编码器，右边解码器 

编码器输出作为解码器输入

编码器： 重复六次的编码块，每个编码块中有单独两个sub-layers 

+ 多头自注意力机制
+ MLP

每次加在一起，输出为LayerNorm(x + Sublayer(x))

#### Layer Norm

解码器：

### Scaled Dot-Profuct Attention

![1680408022815](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680408022815.png)

输入的query和key长度等长，长度$d_{k}$， values长度$d_v$

![1680407755169](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680407755169.png)

和普通的注意力机制相比，多除了一个$\sqrt{d_k}$

两个长度比较长的向量点积出来的值会很大，在softmax中很靠近1，在梯度下降时比较小，所以要做一个normalization

#### Masked

有一个特殊的masked multi-head attention, 用来防止解码器看到一整个时序输入，只能在t时刻看到t时刻对应时序输入及之前的输入

在masked module中，把$q_t$以及后面的值乘以一个非常大的负权重， softmax之后会变成0，使得他们不影响之后的output运算 

## Multi-head Self-Attention Mechanism

![1680408116672](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680408116672.png)

把value，key，query放入线性层(投影到低维)，在做一个scaled dot-product attention，做H次后拼接到一起，最后经过线性层回归到原来的大小规模

 ![1680408337439](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680408337439.png)

## transformer 如何使用attention

在编码器中，传入的三叉戟箭头是KQV, 但是KQV其实在transformer中是一个东西，称为自注意力机制，在编码器中，如果不考虑投影，多头的情况，输出就是输入的一个加权和

在解码器中，output embedding的输入首先经过的是一个masked attention，因为他不允许看某一时刻当前及之后的输入

在masked之后还有一个多头注意力机制，此时k，v是编码器输出后的输入，query是解码器masked attention的输入。此时这个注意力机制的作用是根据解码器的输入，挑选编码器中输出的和query相似度较大的东西（挑选我想要的东西）

## FeedForward

就是MLP，不过是对每一个词做一个多层感知机输出(一个输入里有多少个向量，就有多少个单独的mlp)

![1680409153983](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680409153983.png)

x为一个512的向量，MLP把他投射成2048，再投影成512

![1680409267213](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680409267213.png)

## Embedding

输出的token，要用网络映射成向量，在输出时向量还除了一个$\sqrt{d_{model}}$ ,便于再positional encoding中计算

## Positional Encoding

原因：Attention不会有时序信息，只是一整个句子的加权和

要在输入中加入时序信息

用周期不一样的三角函数值来计算每一个位置。生成一个位置信息向量，和嵌入层相加，输入到注意力机制中

