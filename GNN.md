# Graph Neural Networks

+ 什么数据可以表示一张图
+ 图和别的数据有什么不一样的地方
+ 构建一个GNN pipeline
+ 提供一个GNN 的playground

### 什么是图

 图表示一组实体(模式)之间的关系(边)

+ 顶点 vertex embedding
+ 边 edge embedding
+ 总体 global embedding

有向图，无向图

储存高效，顺序无关(零阶矩阵中可以任意打乱位置)

### Images as graphs

对于一张分辨率为244 * 244 * 3的图片来说，每一个像素都是一个点，如果像素是邻接关系就练成一条边

![1680850012402](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680850012402.png)

### Text as graphs

Graphs -> are -> all -> around -> us

每一个词就是一个顶点，把时序当作有向边

![1680853329342](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680853329342.png)

## What types of problems have graph structured data?

##### graph-level task

对于图结构分类

##### Node-level task

判断顶点的属性

预测每个结点在图中的身份(predict the identity or role of each node within a graph)

##### Edge-level task

判断边的属性

语义分割，关系检测



## GNN

  **A GNN is an optimizable transformation on all attributes of the graph (nodes, edges, global-context) that preserves graph symmetries (permutation invariances)** 

是一种Graph2Graph的网络

example:

![1680855632190](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680855632190.png)

对于一个图的全局向量，定点向量和边向量，分别经过一个MLP

#### Pooling

如果对于某个顶点没有对应的向量编码，采取pooling方法

把该点对应的所有连接边的编码，还有全局向量的编码加在一起，得到该点的编码(如果这些向量的维度一致)

如果这些向量的维度不一致，采取投影的方法



如果对于某条边没有对应的向量编码，可以把该边对应的两个顶点的编码向量，和全局向量相加得到近似编码



如果只有顶点的特征，没有全局的特征，可以把所有的顶点向量相加，进入全局的MLP输出层得到全局的输出

### GNN Pipeline

![1680856402716](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680856402716.png)

原始的架构，对于图信息通过GNN的各种变换层(上述的三个MLP)得到变换后的图，在进入全连接网络得到三种信息的预测输出，但是对于三种信息的更新是直接通过MLP做的，没有考虑图的结构，需要优化

#### Passing messages between parts of the graph

信息传递

把一个顶点和邻接的顶点的所有向量求和，再放入Transform 函数中得到新的graph(聚合 + 更新)，和图片卷积类似  

![1680876768953](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680876768953.png)

在顶点处，把“1近邻”的顶点信息汇聚一起

#### Learning edge representations

![1680877525111](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680877525111.png)

先把顶点的信息给边，让边做更新，再把更新过的边的信息传给顶点，让顶点做更新(如果维度不一致则映射)

也有直接用Weave Layer这样的交错式更新的方法来实现顶点和边之间的信息交换:

![1680877665878](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680877665878.png)

### Global Representations

当图片比较稀疏的时候，从一个顶点到另一个顶点需要传递的成本较大，全局embedding可以用来解决这样的问题： master node or context vector

抽象的连接了所有顶点和边的一个embedding，再传递信息的时候作为中间项，再最后会汇聚所有顶点和边的信息对全局编码进行更新

![1680878160856](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1680878160856.png)

### Inductive biases 归纳偏置

GNN的假设： 保持图的对称性

### GNN Module的抽象解释性分析：

Cited from *Multimodel Learning with graphs* from Nature Machine Intelligence

+ Neighborhood aggregation

![1681042171469](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681042171469.png)

​	根据不同GNN中聚合函数的使用，将GNN分为两类： convolutional & message-passing

todo: 分析GCN的架构

##### Neural Message-passing

![1681043024849](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681043024849.png)

$\phi$是一个aggregator, $\psi$是一个可学的函数，比如MLP

+ Node Update

![1681042289986](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681042289986.png)

​     根据现有的结点状态和聚合后的结点表示，去更新结点

+ Representation transformation

  ![1681042433362](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681042433362.png)

   

​     把已经学到的结点表达转化成根据下游任务需要的embedding形式(参考labels生成的)

# GAT

#### a graph attentional layer

a set of node features

模型的输入是一组结点特征，输出是一组新的结点特征(潜在输出不同的特征维度)

输入结点特征$h_i$, $h_j$
$$
e_{ij} = a(Wh_i, Wh_j)
$$
$e_{ij}$用来计算node j 对于node i的重要性程度

同时，文章中采用mask attention机制，在计算$e_{ij}$的时候，只考虑i周围的一些邻居结点(通常来讲就是一阶临域)

![1681564204632](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681564204632.png)

在原始的GAT原文中，注意力机制是一个线性层和LeakyRelu非线性层 negative input slope

![1681566673665](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681566673665.png)

||是连接矩阵符号

最后把关于node i周围的点的注意力乘以权重乘以编码求和，用一个非线性函数激活，得到新的改结点编码：![1681570277109](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681570277109.png)

实际上会使用多头注意力机制进行整合得到一个综合的embedding，最后输出的向量长度是K * F'的长度(k

个注意力机制，F’为原来每一个注意力机制应该编码的长度)，所以采用平均方法输出一个编码：![1681570937568](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681570937568.png)















