# OTKGE

## Introduction

争对当前的KGE(knowledge graph embedding)主要集中于单模态知识图谱，而对于多模态知识图谱，由于不同模态的嵌入通常处于异构的空间中，直接融合可能会破坏内在分布结构，从而导致统一空间中的不一致和不全面的表示。 

### Optimal Transport Knowledge Graph Embeddings 

本文把多模态知识的聚合步骤看作是一个Optimal Transport的问题， 通过最小化不同分布之间的Wasserstein distance得到多模态融合的embedding和通过Wasserstein barycenter得到一个统一的表达

#### Optimal Transport Problem

假设有两组点X，Y，同时在平面上具有狄拉克分布$\delta()$ 这个问题的文物