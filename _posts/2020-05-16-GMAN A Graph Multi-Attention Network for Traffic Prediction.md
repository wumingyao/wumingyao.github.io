---
layout:     post
title:      "GMan论文阅读笔记"
subtitle:   "GMAN:AGraph Multi-Attention Network for Traffic Prediction"
date:       2020-05-16
author:     "wumingyao"
header-img: "img/in-post/2020.05/16/bg.jpg"
tags: [论文阅读笔记]
categories: [论文笔记]
---
## 参考论文：
[《GMAN:A Graph Multi-Attention Network for Traffic Prediction》](https://arxiv.org/pdf/1911.08415.pdf)

## 主要内容
* [Abstract](#p1)
* [Introduction](#p2)
* [RelatedWork](#p3)
* [Preliminaries](#p4)
* [GraphMulti-AttentionNetwork ](#p5)
* [Experiments](#p6)
* [ Conclusion](#p7)

## 正文

###  <span id="p1">Abstract</span>

由于交通系统的复杂性和许多影响因素的持续变化性，长期交通预测极具挑战性。本文研究了时空因素，提出了一种基于图的多注意力网络（GMAN），用于预测路网图中不同位置的时间步长的交通状况。 GMAN采用了编解码结构，其中编解码器都由多个时空注意块组成，以模拟时空因素对交通条件的影响。
在编码器和解码器之间，使用transform attention layer来转换被编码的交通特征，以生成未来时间步的序列表示作为解码器的输入。
transform attention机制模拟历史和未来时间步之间的直接关系，有效的缓解累计误差的问题。实验结果表明，GMAN在两个实际的交通量预测任务（交通量预测和交通速度预测）中都具有优越性。特别是，在提前1小时的预测中，GMAN在MAE度量方面的性能比最新的方法提高了4%。
    

###  <span id="p2">Introduction</span>
交通预测的目的是基于历史的观测数据来预测道路网上的未来交通情况。附近地区的交通状况预计会相互影响，为了捕捉这种空间相关性，卷积神经网络（CNN）被广泛应用。同时，一个地点的交通状况也与其历史观察相关,RNN网络广泛的应用到模型的时间相关性中。
最近的研究将交通预测描述为一个图形建模问题，因为交通条件限制在路网图形上。使用图卷积网络的研究实现了短期（提前5∼15分钟）交通预测的结果。然而，长期交通预测在文献中仍然缺乏令人满意的进展，主要是由于以下挑战。

1)复杂的时空相关性。
* 动态的空间相关性。如图1所示，随着时间的推移，路网中传感器之间的交通条件相关性发生了重大变化。如何动态选择相关传感器的数据来预测目标传感器在长期范围内的交通状况是一个具有挑战性的问题。
* 非线性时间相关性。传感器处的交通状况可能会突然发生剧烈变化（例如，由于事故），影响不同时间步之间的相关性。如何随时间的推移下，对非线性时间相关性进行自适应建模仍是一个挑战。

2)对误差传播的敏感性。在长期范围内，当对未来做进一步的预测时，每个时间步的小误差可能会放大。

![Figure 1](../../../../../../img/in-post/2020.05/16/Figure 1.jpg)
Figure1:Complexspatio-temporalcorrelations.
动态空间相关性：传感器1和2虽然在道路网络中很接近，但并不总是高度相关；
非线性时间相关性：传感器3在时间步t+l+1处的交通状况可能与远距离时间步（例如t-1）的交通状况更相关，而不是最近的时间步（例如t+l）。

为了解决上述挑战，作者提出了一个图多注意力网络(GMAN)来预测路网上的交通状况。GMAN遵循编码器解码器架构，其中编码器编码输入的交通特征，解码器预测输出序列。
在编码器和解码器之间添加一个transform attention层，以转换被编码的历史交通特征以生成未来表示。编码器和解码器都由一堆ST-Atteintion块组成。
每个ST注意块由一个空间注意机制来建模动态空间相关性，一个时间注意机制来建模非线性时间相关性，以及一个门控融合机制来自适应地融合空间和时间表示。
转换注意机制将历史和未来时间步之间的关系建模，以减轻错误传播的影响。

贡献如下：
* 作者分别提出时空注意机制来模拟动态时空线性相关。此外，还设计了门控融合，以自适应融合时空注意机制所提取的信息。
* 作者提出了一种转换注意机制，将历史交通特征转换为未来的表现形式。这种注意机制模拟了历史和未来时间步之间的直接关系，以缓解错误传播的问题。
* 作者在两个真实的交通数据集上评估了我们的图形多注意网络（GMAN），并在1小时前预测中观察到4%的改进和优于现有基线方法的容错能力。

### <span id="p3">RelatedWork</span>
在过去的几十年里，交通预测得到了广泛的研究。
深度学习方法（例如，长期短期记忆（LSTM））。与传统时间序列方法（例如，自回归综合移动平均（ARIMA））和机器学习模型（例如，支持向量回归（SVR））在捕获交通条件下的时间相关性方面k-最近邻（KNN）显示出更优越的性能。为了建立空间相关性模型，研究人员应用卷积神经网络（CNN）来捕获欧几里德空间中的依赖关系。
最近的研究提出了基于图的交通预测，并使用图卷积网络（GCN）对道路网络中的非欧几里德相关进行建模（Li等人。2018b；Lv等人。2018年）。这些图形化模型通过逐步的方法产生多个提前步骤的预测，并且可能遭受不同预测步骤之间的错误传播。

图的深度学习将神经网络推广到图的结构化数据是一个新兴的课题。一系列的研究将CNN推广到在谱或空间上建立任意图的模型透视图。另一个研究方向是图嵌入，它学习保留图形结构信息的顶点的低维表示，将WaveNet集成到GCN中进行时空建模。当它学习静态邻接矩阵时，该方法在捕获动态空间相关性方面面临困难。

注意机制因其在依赖关系建模中的高效性和灵活性而被广泛应用于各个领域。注意机制的核心思想是根据输入的数据自适应地关注最相关的特征。最近，研究人员将注意力机制应用于图形结构数据，以建立用于图形分类的空间相关性模型。将注意力机制扩展到图形时空数据预测。
### <span id="p4">Preliminaries</span>
将路网标记为加权有向图$G=(\nu,\varepsilon,A)$。其中，$\nu$代表道路网上的一组顶点$N=|\nu|$，例如传感器。$\varepsilon$表示这些顶点项链的边。$A\in \mathbb{R}^{N\times N}$
是相邻矩阵的权重。$A_{\nu_i,\nu_j}$表示顶点$\nu_i$和顶点$\nu_j$的相近度，通过路网的距离测量。

在时间步$t$的交通状况表示为在图$G$上的图信号$X_t=\mathbb{R}^{N\times C}$，其中$C$表示交通状况的指标数量。

问题研究：在给定$N$个点的过去的$P$个时间步的观察数据$\chi =(X_{t_1},X_{t_2},\cdots,X_{t_p}) \in \mathbb{R}^{P\times N\times C}$,目标是预测未来的$Q$个时间步的交通状况，标记为
$ \hat{Y} = (\hat{X}\_{t_{P+1}},\hat{X}\_{t_{P+2}},\cdots,\hat{X}\_{t_{P+Q}}) \in \mathbb{R}^{Q\times N\times C}$

###  <span id="p5">GraphMulti-AttentionNetwork</span>
![Figure 2](../../../../../../img/in-post/2020.05/16/Figure 2.jpg)
Figure 2: The framework of Graph multi-attention network (GMAN). 

图2展示了图多注意网络（GMAN）的框架，它有一个编码器和解码器结构。两者编码器和解码器包含有L个ST注意块（STAtt块）。
每一个ST注意块都与门控融合的时空注意机制有关。在编码器和解码器之间，向网络添加转换注意层，以将编码的特征转换为解码器。
作者还通过时空嵌入（STE）将图形结构和时间信息整合到多注意机制中。
此外，为了便于连接，所有层都产生D维输出。

#### Spatio-Temporal Embedding 
由于交通条件的演变受到基础道路网的限制，所以将路网信息纳入预测模型至关重要。为此，作者提出了一种空间嵌入的方法，将顶点编码成保留图结构信息的向量。
作者利用node2vec方法来学习顶点表示。此外，为了将预学习向量与整个模型共同训练，这些向量被输入到两层全连接神经网络中。
然后，我们得到空间嵌入，表示为$e^S_{v_i}\in \mathbb{R}^D$,其中$v_i\in\nu$ 

空间嵌入只需要提供静态的表示，不能代表路网中交通传感器之间的动态相关性。因此，我们进一步提出了一种时间嵌入，将每个时间步编码为一个向量。
具体来说，让一天有T个时间步。我们使用one-hot编码将每个时间步的星期几和时间编码为$\mathbb{R}^7$和$\mathbb{R}^T$，并将他们连接到向量$\mathbb{R}^{T+7}$。
接下来，作者应用两层全连接神经网络将时间特征转换为向量$\mathbb{R}^D$。在本模型中，作者嵌入了历史$P$和未来$Q$个时间步的时间特征，表示为$e^T_{t_j}\in \mathbb{R}^D$，其中$t_j=t_1,\cdots,t_P,\cdots,t_{P+Q}$。

#### ST-Attention Block
如图2(C)所示，ST-Attention 块包含一个空间注意力、一个时间注意力和门融合块。将第$l$块
的输入记为$H^{(l-1)}$，在时间步$t_j$向量$v_i$的隐藏状态记为$h_{v_i,t_j}^{(l-1)}$。
在第$l$块的空间注意力机制和时间注意力机制的输出分别记为$H_S^{(l)}$和$H_T^{(l)}$，其中在时间步$t_j$
向量$v_i$的隐藏状态分别记为$hs_{v_i,t_j}^{(l)}$和$ht_{v_i,t_j}^{(l)}$。在门融合后，获得第$l$块
的输出为$H^{(l)}$。

将非线性变化表示为：
$$f(x)=ReLU(xW+b)\tag{1}$$
其中$W,b$是可学习的参数，ReLU是激活函数。

Spatial Attention：道路的交通条件会受到其他路段的不同影响。这种影响是随时间高度动态的、有变化的。
为了去建模这些属性，作者开发了一个空间注意力机制去自适应捕捉道路网上的传感器间的相互关系。
其关键思想是在不同的时间步动态地为不同的顶点（例如，传感器）分配不同的权重，如图三所示。
对于在时间步$t_j$的顶点$v_i$,从所有顶点捕获一个权重总和：
$$hs_{v_i,t_j}^{(l)}=\sum_{v\in V}\alpha_{v_i,v}\cdot h_{v,t_j}^{(l-1)}\tag{2}$$
其中$V$是所有顶点的集合，$\alpha_{v_i,v}是顶点$v$对顶点$v_i$的重要性得分，并且注意力得分的总和
等于1，即$\sum_{v\in V}\alpha_{v_i,v}=1$。

![Figure 3](../../../../../../img/in-post/2020.05/16/Figure 3.jpg)
Figure 3: The spatial attention mechanism captures timevariant pair-wise correlations between vertices.

在一个特定的时间步，当前交通状况和路网结构都会影响传感器之间的相关性。例如，一条道路拥堵后会影响
它相邻路段的交通状况。因此，作者同时考虑交通特征和路网拓扑结构来学习注意力得分。
作者将隐藏状态与时空嵌入相连接，并采用标度点积方法计算顶点vi与v之间的相关性：
$$s_{v_i,v}=\frac{\langle h_{v_i,t_j}^{(l-1)}\quad||\ e_{v_i,t_j}\ ,\ h_{v,t_j}^{(l-1)}\quad||\ e_{v,t_j}\rangle}{\sqrt{2D}}\tag{3}$$

其中，$\|\|$代表拼接操作(concatenation)，$\langle\bullet,\bullet\rangle$表示点积操作，2D是$h_{v_i,t_j}^{(l-1)}\quad||\ e_{v_i,t_j}$
的维度。然后，$s_{v_i,v}$经过softmax函数标准化为：

$$\alpha_{v_i,v}=\frac{exp(s_{v_i},v)}{\sum_{v_r\in V}exp(s_{v_i},v_r)}\tag{4}$$

在注意力得分$\alpha_{v_i,v}$被获得后，隐藏状态就能通过等式2更新。

为了稳定学习过程，我们将空间注意机制扩展为多头注意机制。具体来说，作者将$K$个并行的注意力机制
与不同的可学习函数进行拼接：

$$s_{v_i,v}^{(k)}=\frac{\langle f_{s,1}^{(k)}(h_{v_i,t_j}^{(l-1)}\quad||\ e_{v_i,t_j}),f_{s,2}^{(k)}(h_{v,t_j}^(l-1)\quad||\ e_{v,t_j})\rangle}{\sqrt{d}}\tag{5}$$

$$\alpha_{v_i,v}^{(k)}=\frac{exp(s_{v_i,v}^{(k)})}{\sum_{v_r\in V}exp(s_{v_i,v_r}^{(k)})}\tag{6}$$

$$hs_{v_i,t_j}^{(l)}=\quad||_{k=1}^{K}\lbrace\sum_{v\in V}\alpha_{v_i,v}^{(k)}\cdot f_{s,3}^{(k)}(h_{v,t_j}^{(l-1)})\rbrace\tag{7}$$

其中，$f_{s,1}^{(k)}(\bullet)$，$f_{s,2}^{(k)}(\bullet)$，和$f_{s,3}^{(k)}(\bullet)$代表三个不同的在第$k$头注意力中的非线性投影，产生$d=D/K$维的输出。

当顶点N的数量很大的时候，就会耗费很大的内存和计算。为了解决这个问题，作者进一步提出了组空间注意力，包括组内空间注意和组间空间注意，如图4所示。

![Figure 4](../../../../../../img/in-post/2020.05/16/Figure 4.jpg)
Figure 4:  Group spatial attention computes both intra-group and inter-group attention to model spatial correlations.

作者随机地将N个顶点分成G个组，其中每组包含$M=N/G$个顶点。在每组中，作者通过方程5、6和7计算组内注意来模拟顶点之间的局部空间相关性，其中可学习地
参数是通过组间进行分享的。然后，作者在每个组中应用最大池方法来获得每个组的单个表示。接下来，作者计算组间的空间注意来模拟不同组之间的相关性，为每个组生成一个全局特征。
局部特征作为最终输出添加到相应的全局特征中。

Temporal Attention 一个地点的交通状况与其先前的观测值相关，并且相关度随时间的推移而非线性变化。
为了模拟这些性质，作者设计了一个时间注意机制，以自适应地模拟不同时间步之间的非线性相关性，如图5所示。
时间相关性受交通条件和相应时间上下文的影响。例如，早上高峰时段发生的拥堵可能会影响几个小时的交通。
因此，作者同时考虑交通特征和时间来衡量不同时间步之间的相关性。
具体地说，作者将隐藏状态与时空嵌入相连接，并采用多头方法计算注意得分。
对于顶点$v_i$,在时间步$t_j$和$t$的相关性被定义为：
$$u_{t_j,t}^{(k)}=\frac{\langle f_{t,1}^{(k)}(h_{v_i,t_j}^{(l-1)}\quad|| e_{v_i,t_j}),f_{t,2}^{(k)}(h_{v_i,t}^{(l-1)}\quad|| e_{v_i,t})\rangle}{\sqrt d}\tag{8}$$

$$\beta_{t_j,t}^{(k)}=\frac{exp(u_{t_j,t}^{(k)})}{\sum_{t_r\in\mathcal{N}_{t_j}}exp(u_{t_j,t_r}^{(k)})}\tag{9}$$

其中，$u_{t_j,t}^{(k)}$表示时间步$t_j$和$t$的相关性，$\beta_{t_j,t}^{(k)}$是第$k$个头的注意力得分，表示从时间步$t$到$t_j$的重要性。
$f_{s,1}^{(k)}(\bullet)$，$f_{s,2}^{(k)}(\bullet)$代表两种不同的可学习的转换器。$\mathcal{N}_{t_j}$代表时间步$t_j$之前的时间步集合，
例如，仅考虑早于目标步骤的时间步骤中的信息，以启用因果关系。
一旦注意力得分被获得，顶点$v_i$在时间步$t_j$的隐藏状态被更新为如下：

$$ht_{v_i,t_j}^{(l)}=\quad||_{k=1}^{K}\lbrace\sum_{t\in \mathcal{N_{t_j}}}\beta_{t_j,t}^{(k)}\cdot f_{t,3}^{(k)}(h_{v_i,t}^{(l-1)})\tag{10}$$

其中，$f_{s,3}^{(k)}(\bullet)$代表一个非线性投影。方程8、9和10中的可学习参数通过并行计算在所有顶点和时间步上共享。

![Figure 5](../../../../../../img/in-post/2020.05/16/Figure 5.jpg)
Figure 5: The temporal attention mechanism models the non-linear correlations between different time steps.

Gated Fusion 一个路段在一个特定时间步的交通条件与之前的观测值和其他路段的交通条件是相关联的。正如图2(c)所示，
作者设计了一个门融合机制来自适应地融合空间和时间特征。在第$l$个块中，空间和时间注意力机制的输出分别表示为$H_S^{(l)}$，$H_T^{(l)}$，
两者在编码器端的形状为$\mathbb{R}^{P\times N\times D}$,在解码器端的形状为$\mathbb{R}^{Q\times N\times D}$。
$H_S^{(l)}$和$H_T^{(l)}$被融合为:

$$H^{(l)}=z\bigodot H_S^{(l)}+(1-z)\bigodot H_T^{(l)}\tag{11}$$

$$z=\sigma(H_S^{(l)}W_{z,1}+H_T^{(l)}W_{z,2}+b_z)\tag{12}$$

其中$W_{z,1}\in\mathbb{R}^{D\times D}$，$W_{z,2}\in\mathbb{R}^{D\times D}$
以及$b_z\in\mathbb{R}^D$都是可学习的参数，$\bigodot$代表元素级别的乘，$\sigma$代表
sigmoid激活函数，$z$是门函数。门控融合机制自适应地控制每个顶点和时间步长的时空依赖流。

#### Transform Attention

![Figure 6](../../../../../../img/in-post/2020.05/16/Figure 6.jpg)
Figure 6: The transform attention mechanism models direct relationships between historical and future time steps.

为了缓解长时间范围内不同预测时间步长之间的误差传播效应，在编码器和解码器之间增加了一个变换注意层。
它将每个未来时间步和每个历史时间步之间的直接关系建模，以转换编码的编码特征以生成未来表示作为解码器的输入。
如图6所示，对于顶点$v_i$,预测时间步$t_j(t_j=t_{P+1},\cdots,t_{P+Q})$和历史时间步
$t(t=t1,\cdots,t_P)$的相关性通过一个时空嵌入进行测量：

$$\lambda_{t_j,t}^{(k)}=\frac{\langle f_{t_r,1}^{(k)}(e_{v_i,t_j}),f_{t_r,2}^{(k)}(e_{v_i,t})\rangle}{\sqrt d}\tag{13}$$


$$\gamma_{t_j,t}^{(k)}=\frac{exp(lambda_{t_j,t}^{(k)})}{\sum_{t_r=t_1}^{t_P}exp(\lambda_{t_j,t_r}^{(k)})}\tag{14}$$

利用注意力得分$\gamma_{t_j,t}^{(k)}$，通过自适应地选择所有历史P时间步骤中的相关特征，将编码的业务特征转换为解码器：

$$h_{v_i,t_j}^{(l)}=\quad||_{k=1}^{K}\lbrace\sum_{t=t_1}^{t_P}\gamma_{t_j,t}^{(k)}\dot f_{t_r,3}^{(k)}(h_{v_i,t}^{(l-1)})\rbrace\tag{15}$$

方程13、14和15可以在所有顶点和时间步上并行计算，共享可学习参数。

#### Encoder-Decoder

如图2(a)所示，GMAN是一个编码-解码架构。输入编码器之前，历史观察数据$\chi\in\mathbb{R}^{P\times N\times C}$
被转换到$H^{(0)}\in\mathbb{P\times N\times D}$用于全连接层。然后$H^{(0)}$和L个ST-Attention 块一起被喂给编码器，
并且产生一个输出$H^{(L)}\in\mathbb{R}^{P\times N\times D}$。
在编码器之后，一个转换层用于转换编码特征$H^{(L)}$来产生未来的序列特征$H^{L+1}\in\mathbb{R}^{Q\times N\times D}$。
接着，解码器叠加L个ST-Attention 块到$H^{(L+1)}$，并且产生的输出为$H^{(2L+1)}\in\mathbb{R}^{Q\times N\times D}$。
最后，全连接层产生Q个时间步的预测$\hat{Y}\in\mathbb{R}^{Q\times N\times C}$。

GMAN可以通过反向传播进行端到端的训练，方法是最小化预测值和真实值之间的平均绝对误差（MAE）：

$$\mathcal{L}(\theta)=\frac{1}{Q}\sum_{t=t_{P+1}}^{t_{P+Q}}\left| Y_t-\hat{Y}_t\right| \tag{16}$$

###  <span id="p6">Experiments</span>

#### Datasets

作者评估了GMAN在两个不同路网规模的交通预测任务中的性能：

(1) 厦门数据集的交通量预测（Wang等人，2017年），其中包含中国厦门市从2015年8月1日至2015年12月31日的95个交通传感器记录的5个月数据；

(2) PeMS数据集的交通速度预测（Li等人，2017年）。2018b），包含325个交通传感器从2017年1月1日到2017年6月30日在湾区记录的6个月数据。

两个数据集中传感器的分布如图7所示。

![Figure 7](../../../../../../img/in-post/2020.05/16/Figure 7.jpg)
Figure 7: Sensor distribution of Xiamen and PeMS datasets.

![Table 1](../../../../../../img/in-post/2020.05/16/Table 1.jpg)
Table 1: Performance comparison of different approaches for traffic prediction on Xiamen and PeMS datasets.

#### 数据预处理

在这两个数据集中，时间步表示5分钟，数据通过ZScore方法规范化。将70%的数据用于训练，10%用于验证，20%用于测试。
在构建路网图时，将每个交通传感器视为一个顶点，计算传感器之间的成对路网距离。然后，邻接矩阵定义为：

$$\mathcal{A}_{v_i,v_j}=\begin{cases}
exp(-\frac{d_{v_i,v_j}^{2}}{\sigma^2}), if\ exp(-\frac{d_{v_i,v_j}^2}{\sigma^2}\ge\epsilon) \\
0, otherwise
\end{cases}\tag{17}$$

其中，$d_{v_i,v_j}$是传感器$v_i$到传感器$v_j$的路网距离，$\sigma$是标准差，$\epsilon=0.1$是控制邻接矩阵\mathcal{A}的稀疏性的阈值。

#### Experimental Settings
Metrics：MAE、RMSE、MAPE

Hyperparameters：遵循先前的工作，作者使用P=12个历史时间点（1小时）来预测下一个Q=12个时间点（1小时）的交通状况。使用Adam优化器对模型进行训练，初始学习率为0.001。
在群体空间注意中，作者分别将厦门数据集中的顶点划分为G=19组和PeMS数据集中的G=37组。两个数据集上的交通状况数均为C=1。模型共有3个超参数，即注意块的数目L，注意头的数目K，以及每个注意头的维数d（每个层的通道d=K×d）
在验证集上调整这些参数，并观察在设置L=3、K=8和d=8（d=64）上的最佳性能。

基线：
(1)Arima     
(2)SVR     
(3)前馈神经网络FNN     
(4)FC-LSTM 它是一个在编码器和解码器中具有完全连接的LSTM层的序列到序列模型       
(5)STGCN    
(6)DCRNN扩散卷积递归神经网络    
(7)WaveNet

#### Experimental Results

Forecasting Performance Comparison   表1显示了两个数据集上提前15分钟（3步）、30分钟（6步）和1小时（12步）预测的不同方法的比较。我们观察到：
（1）深度学习方法优于传统的时间序列方法和机器学习模型，显示了深度神经网络对非线性交通数据建模的能力；
（2）在深度学习方法中，基于图的模型（包括STGCN、DCRNN、图波网和GMAN）通常优于FC-LSTM，指出路网信息对交通预测至关重要；
（3）GMAN具有最先进的预测性能，其优势在长期范围内（如提前1小时）更为明显。我们认为，长期交通量预测更有利于实际应用，例如，它允许运输机构有更多的时间根据预测采取行动优化交通。

