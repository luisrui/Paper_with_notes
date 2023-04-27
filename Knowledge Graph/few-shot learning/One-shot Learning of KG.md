# One-shot Learning of KG

Problems:

+ A large portion of KG relations are actually long-tail( few instances)

+ real world KGs are often dynamic and evolving at any given moment

这篇文章预测tail entity $t_{true}$ 比其他$C_{h,r}$中的entities要高

#### Meta-Train:

在实验中设定采样训练任务的时候，每一个训练任务对应一种关系的一个元祖：

$$
T_r = ({D_r^{train}, D_r^{test}})--T_{meta-train}
$$

每一个关系在Dtrain中只有一个元祖$(h_0, r, t_0)$，每个$D_r^{test}$包含

$$
D_r^{test} = (h_i, r, t_i, C_{h_i, r})
$$

整体的loss function可以设计为：

$$
l_{\theta}(h_i, r, t_i,|C_{h_i, r}, D_r^{train}) 
$$

![](/Users/cairui/Library/Application%20Support/marktext/images/2023-04-26-23-08-26-image.png)

同时，抽出meta-train的一小部分关系作为<b>meta-validation</b>

#### Meta-Test:

在新的关系上做预测, 每一个新的关系都拥有一个样本在meta-training的supporting set中，并且和meta-train类似的测试集合

### Model

目的： 生成一个$M((h, t),(h', t'))$的相似性函数

##### Neighbor encoder：

用周围节点编码本身节点，加强representation

![](/Users/cairui/Library/Application%20Support/marktext/images/2023-04-26-23-50-07-image.png)

使用tanh作为激活函数
