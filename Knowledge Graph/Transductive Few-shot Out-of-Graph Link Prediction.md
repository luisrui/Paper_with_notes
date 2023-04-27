##  Transductive Few-shot Out-of-Graph Link Prediction

## Graph Extrapolation Networks

Few-shot on Nodes

![截屏2023-04-21 下午10.08.11](C:\Users\ASUS\Documents\Tencent Files\3437713406\FileRecv\截屏2023-04-21 下午10.08.11.png)                            

###### Graph Extrapolation Networks(GENs)

Meta-Train two GNNs to predict the links between seen-to-unseen , and unseen-to-unseen entities

#### Few-Shot Out-of-Graph Link Prediction

![1682149108939](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1682149108939.png)

用已知的关系去预测位置的实体，在未知的实体之间预测关系

#### Learning Objective 

对于不可见的实体，根据外推已有知识图谱的知识，去学一个distribution。这个任务可以转化为最大化含有不可见元组的置信度分数:$s(e_h, r, e_t^{'})$

![1682152242564](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1682152242564.png)

每一个任务拥有一个对应于不可见实体的分布，一共有N个实体。把关联元组的实体分成两组，一组为supporting set $S_i$, 一组为query set $Q_i$

![img](file:///C:\Users\ASUS\Documents\Tencent Files\3437713406\Image\C2C\{F8F9D803-034D-70EC-CA14-5F2FAEFA712F}.png)

![img](file:///C:\Users\ASUS\Documents\Tencent Files\3437713406\Image\C2C\{070B16CA-47D6-BDD0-E15A-2CAB78105990}.png)

k是few shot的个数，Mi是和不可见实体相关的元组个数

元学习的目标就是：最大化基于Qset的元组分数：

![1682243807022](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1682243807022.png)

###### K-shot out-of-graph(OOG) link prediction

### inductive GEN:

$f_{\theta}$ 用GNN-based一个元学习模组输出不可见实体的表征

![1682250963858](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1682250963858.png)

$W_r$是一个meta-learned的矩阵，$C_{r,e}$是关系和实体对的特征表示的拼接

###  Transductive Meta-Learning of GENs

inductive的策略没有考虑不可见实体之间的关系

新增加一层GEN层，考虑了不可见实体之间的关系                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                ![1682251534585](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1682251534585.png)

$\Phi$是不可见实体的编码

### Stochastic Inference（这个地方基本没看懂，是一个分裂成正态分布的操作）

通过学习一个不可见实体编码的分布，随机编码这些不可见的实体

### Loss Function

![1682255488674](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1682255488674.png)

