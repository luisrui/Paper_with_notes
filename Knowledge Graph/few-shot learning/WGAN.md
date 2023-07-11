# GAN for Zero-shot relational learning in KG

![](/Users/cairui/Library/Application%20Support/marktext/images/2023-04-26-11-34-03-image.png)

内容参考： https://zhuanlan.zhihu.com/p/112908641 

### 在zero-shot中KGC任务的定义

对于每一个给定的query tuple($e_1$, r), 我们拥有ground truth的尾节点和一个备选的集合$C_{(e_1, r)}$，我们的模型需要把集合中通过排序，排第一个的潜在节点和ground truth节点做匹配

根据zero-shot把关系分为两类 $R_s$为可见的关系，$R_u$为不可见的关系

根据可见的关系，构建训练集

$$
D_s= {(e_1, r_s, e_2, C_{(e_1, r_s)})}
$$

根据不可见关系，测试集为：

$$
D_s= {(e_1, r_u, e_2, C_{(e_1, r_s)})}
$$

在训练过程中，无论训练集还是测试集，尾节点必须和C的最高节点一致，最为最后的correctly recognized

![](/Users/cairui/Library/Application%20Support/marktext/images/2023-04-26-15-58-55-image.png)

### GAN

![](/Users/cairui/Library/Application%20Support/marktext/images/2023-04-26-16-18-33-image.png)

    其中生成器（Generator）将文本描述转化成能够反映关系在知识图谱中的语义信息的表示；判别器（Discriminator）将虚假数据（生成器生成的关系向量）区别于真实数据分布，也同时判别关系的类型。为了编码真实数据获取关系表示，作者训练了一个特征编码器（Feature Encoder），用于从KG embeddings中训练真实数据分布，它提前通过知识图谱数据训练好，并且在后续的对抗生成训练中参数固定。
