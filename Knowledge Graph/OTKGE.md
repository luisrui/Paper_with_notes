# OTKGE

## Introduction

争对当前的KGE(knowledge graph embedding)主要集中于单模态知识图谱，而对于多模态知识图谱，由于不同模态的嵌入通常处于异构的空间中，直接融合可能会破坏内在分布结构，从而导致统一空间中的不一致和不全面的表示。 

### Optimal Transport Knowledge Graph Embeddings 

本文把多模态知识的聚合步骤看作是一个Optimal Transport的问题， 通过最小化不同分布之间的Wasserstein distance得到多模态融合的embedding和通过Wasserstein barycenter得到一个统一的表达

#### Optimal Transport Problem

假设有两组点X，Y，同时在平面上具有狄拉克分布$\delta()$ 这个问题的目标是得到X，Y的经验分布$\mu$, $\nu$

![1681107060167](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681107060167.png)

本文中求解的是p = 1, 2 时候的最优化Wasserstein distance长度

## Methodology

问题的输入是一个多模态知识图谱 G = ($\epsilon, \R, \tau$)

一个实体e拥有结构structural embedding $e_s$, linguistic embedding $e_I$ 和 visual embedding $e_v$

### Semantic matching

## Multi-modal Alignment

让多模态编码和结构编码相匹配

令语义空间 I和结构编码 S作为一个例子，采用OT alignment的策略

+ 估计语义编码$E_I$的分布$\mu$ 和结构编码$E_s$的分布$\nu$
+ 找到一个矩阵T，使得$T_{ij}$表示把$E_I$第i个特征维度转换为结构编码$E_s$的第j个特征维度的概率
+ 使用T计算barycenter，把语义编码映射到统一的空间S中，形成新的编码$\hat{E_I}$

![](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681222456963.png)

### Relational Transformations

转化头部的实体h：

![1681118144781](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1681118144781.png)