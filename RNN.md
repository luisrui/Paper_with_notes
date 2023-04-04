# RNN

潜变量自回归模型：

$x_{t}$ 与 $x_{t-1}$,$h_{t}$ 相关

$h_{t}$ 与 $h_{t-1}$,$x_{t-1}$相关

#### 有隐状态的循环神经网络：

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668478848709.png" alt="1668478848709" style="zoom:50%;" />

输出层$o_{t}$ 与隐变量层$h_{t}$，输出层$x_{t-1}$相关
$$
h_{t} = \phi(W_{hh}h_{t-1} + W_{hx}X_{t-1} + b_{h})\\
o_{t} = \phi(W_{ho}h_{t} + b_{o})
$$
$O_{t}$  only related to $h_{t}$ and have no business with $x_{t}$

 $x_{t-1}$ is to update $h_{t}$

# LSTM

### Overall pipeline:

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1669086406207.png" alt="1669086406207" style="zoom:67%;" />

Input Gate:  控制外界的输入进入Memory Cell

Output Gate: 控制memory cell的输出能否输出到外界

Forget Gate:  控制Memory cell中的值是否格式化

四个输入： 三个门单元信号和一个外界输入， 一个输出

##### 和RNN的比较：

RNN 每个周期都会把Memory Cell格式化

对于四个输入：

+ 输入后接激活函数，一般选择sigmoid（->0~1）决定开关开和关的程度

把每一个LSTM的cell看作一个Neuron

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1669162563468.png" alt="1669162563468" style="zoom:50%;" />

在LSTM里面，四个input每一个对应一个weight和bias，LSTM训练的参数是普通网络的四倍

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1669163487866.png" alt="1669163487866" style="zoom:50%;" />

c 这个vector就是指这一整排LSTM的memory cell 的值，每个memory cell 里的值代表 c 的一个维度 

时间点t有输入$x^{t}$乘以线性变换矩阵得到Z，Z的四维分别对应每个LSTM单元的四个开关输入向量

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1669164222028.png" alt="1669164222028" style="zoom:50%;" />

对于每一个LSTM module的输入，不仅是时间点的输入x^t, 还是把“peephole"--cell的输出，还有cell乘以output gate的输入传入下一个LSTM module， 用这三个vector作transform去控制整个模型

#### RNN Training

BPTT：在时间维度上新的back propogation

RNN的error surface非常崎岖,导致训练非常不稳定

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1669167038659.png" alt="1669167038659" style="zoom:33%;" />

##### 解决方法：

应用LSTM，解决梯度消失问题（并不能解决梯度爆炸问题）

- 原始的RNN中，后一个时间点输入到memory cell的值会直接覆盖前一个时间点memory cell的值，这相当于把前一时间点的w对memory cell的影响给消除掉了，所以会容易产生gradient vanishing的问题。
- 而LSTM中，如果forget gate打开（即保留memory cell的值），memory cell值会是上一个时间点的memory cell的值加上现在 input的值。所以原来的 w 对 memory cell 造成的影响还保留着，没有被直接消除掉。所以训练的时候，可以给一个bias，使得forget gate在大多数时候都被开启

# VAE

## Auto-encoder

自监督学习的学习框架：

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1669176403236.png" alt="1669176403236" style="zoom: 67%;" />![1669176728048](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1669176728048.png)

用没有标注(no labels)的数据训练一个模型，做微调完成实现其他任务

Auto-encoder 被视为是一种自监督学习的方法：

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1669176728048.png" alt="1669176728048" style="zoom:50%;" />

在Reconstruction任务中，decoder要尽可能重建原图片对应向量

Encoder做的事情就是把高维输入转化为低维输出(dimension reduction)



































# Transformer

### self attention

Vector set as input: 

+ world vector : using one-hot encoding
+ graph: relationship graph

the output could be：

+ each vector has a label (POS tagging)
+ the whole sequence has a label (sentiment analysis)
+ model decides the number of labels itself(seq2seq)

##### sequence labeling

consider the whole sequence but not just focus on a single vector

using <b>self-attention</b> with whole information of the sequence and output the vector numbers same as the input

steps:

+ find the relevant vectors in a sequence, use a parameter to represent the  relevance

How to calculate the parameter $\alpha$?

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668547816522.png" alt="1668547816522" style="zoom:50%;" />

one way to compute: <b>dot-product</b>, Additive

+ 输入的两个向量分别乘以不同 的矩阵, Wq, Wk, 再作内积

+ 输入的两个向量乘以权重之后相加，经过一个激活函数和乘以新权重后输出

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668548418161.png" alt="1668548418161" style="zoom:50%;" />query 是第一个向量提供的，key是第二个向量提供，此时query * key就是1向量与2向量的关联性 $\alpha_{1,2}$(attention score) 因为query对应的权重矩阵Wq和key对应权重矩阵Wk不同，所以$\alpha_{1,2}$ 与 $\alpha_{2,1}$可能不同

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668548520341.png" alt="1668548520341" style="zoom:67%;" />
$$
b^{i} =\Sigma_{j}\alpha_{i,j}v^{j}
$$
其中$v^{j} = W^{v}a^{i}$ 

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668548770206.png" alt="1668548770206" style="zoom:25%;" />

每一个v都是由初始a向量乘以一个v权重矩阵得来，整个过程由三个矩阵，Wq，Wk，Wv

在self-attention的module中，每一个向量对应的attention b值都是被同时计算得出的

一个明显的分块函数形式（整个module就是一个线性变换）：

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668549634649.png" alt="1668549634649" style="zoom:50%;" /> 



<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668612562393.png" alt="1668612562393" style="zoom:50%;" />



#### Multi-head self attention

对于求出的qi, ki, vi, 在乘以不同的矩阵得到qi1,qi2,ki1,ki2,vi1,vi2, 1号下标作attention得到bi1, 2号下标作attention得到bi2

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668613836747.png" alt="1668613836747" style="zoom:25%;" />

把得到的bi1,bi2线性拼接，再乘以一个权重矩阵，的到最后的bi

#### Positional Encoding

There is no position information in former self-attention

Each position has a unique positional vector $e^{i}$

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668614425839.png" alt="1668614425839" style="zoom:33%;" />

把位置信息加在input vector上

#### Trancated self-attention

对于一句长度为L的句子，产生一个注意力矩阵需要计算L^2个单位，提出只关注一句话中一个小的范围（人为设定），应用self-attention module.



CNN可以理解为一种特殊的self-attention

self-attention比RNN的优点就是可以平行处理所有input

#### Sequence-to-sequence

 Input a sequence, output a sequence  The output length is determined by model. 

Pipeline:

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668617513745.png" alt="1668617513745" style="zoom:25%;" />

### Encoder：

##### Normal Encoder using self-attention module：

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668617613778.png" alt="1668617613778" style="zoom:20%;" />

#### Transformer Encoder:

应用了input + output的残差形式，在应用Layer Normalization(把输入的向量计算了mean和standard deviation，再做一个normalization)

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668617714158.png" alt="1668617714158" style="zoom: 25%;" />![1668617848874](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668617848874.png)

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668617879312.png" alt="1668617879312" style="zoom:25%;" />

在上图self-attention模块输出之后，再进入FC层，利用相似的处理过程输出标准化的encoding结果

### Decoder：

##### Autoregressive Decoder(AT):

special token：在encoder编码出的向量前加一个起始向量START

输出向量，向量中每个元素对应词库中字的分数，用softmax求出最大分数

每次输出一个词汇，再作为decoder的输入进行下一个训练(一个一个产生)

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668632105879.png" alt="1668632105879" style="zoom:33%;" />

### Transformer Decoder：

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668634491186.png" alt="1668634491186" style="zoom:33%;" />

在decoder中，multi-head attention加了一个masked：

##### masked self-attention：

在产生bi的时候只考虑当前和过去的信息（ai,ai-1,ai-2,...,a1）,而不是全部的信息

因为对于decoder而言，输入的是时间序列，整个过程是马尔科夫的

目前decoder无法判断什么时候停下，需要认为加入END向量 

##### Non-autoregressive decoder(NAT):

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668649029776.png" alt="1668649029776" style="zoom:33%;" />

NAT decoder 同时输入，同时输出

如何判断NAT decoder输出长度：

+ 训练一个分类器，使得encoder输入，输出一个长度代表decoder输出的长度
+ 大长度的START，输出的时候忽略END tokens之后的结果

NAT的优点：平行化，可控制的输出长度 NAT缺点：Multi-modality

##### Encoder 和Decoder之间的信息传递：

##### Cross Attention：

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668649494437.png" alt="1668649494437" style="zoom:50%;" />

<img src="C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1668657265211.png" alt="1668657265211" style="zoom:50%;" />

query来自decoder，encoder生成的编码向量再生成key和value，作multi-head attention之后得到新的value，作normalization之后传入FC中

### Training：

收集声音信号，经过encoder编译得到编码向量，输入decoder得到输出结果，作softmax之后与ground truth(读热词向量作loss)，得到cross entropy function之后，同理其他的输入也可以得到交叉熵损失，一整句话交叉熵损失求和，得到总体损失函数，可以优化损失函数

##### Teacher Forcing：

把ground truth正确答案作为decoder的输入

##### Copy Mechanism:

在语言中，重复的现象很常见，需要直接复制一段输入作为输出，不用做seq2seq