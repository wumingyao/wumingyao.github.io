---
layout:     post
title:      "时空数据挖掘"
subtitle:   "A Comprehensive Survey on STDM"
date:       2020-07-14
author:     "wumingyao"
header-img: "img/in-post/2020.07/14/bg.jpg"
tags: [交通预测,论文笔记,综述,时空数据挖掘]
categories: [论文笔记]
---

## 参考论文：
[《Deep Learning for Spatio-Temporal Data Mining: A Survey》](https://arxiv.org/abs/1906.04928)


## 主要内容
* [一、Abstract](#p1)
* [二、INTRODUCTION](#p2)
* [三、CATEGORIZATION OF SPATIO-TEMPORAL DATA](#p3)
* [四、FRAMEWORK](#p4)
* [五、DEEP LEARNING MODELS FOR ADDRESSING DIFFERENT STDM PROBLEMS](#p5)
* [六、APPLICATIONS](#p6)
* [七、OPEN PROBLEMS](#p7)
* [八、REFERENCES](#p8)
## 正文

###  <span id="p1">一、Abstract</span>
随着全球定位系统（GPS）、移动设备和遥感等各种定位技术的快速发展，时空数据变得越来越有用。从时空数据中挖掘有价值的知识对于许多现实世界的应用都是至关重要的，这些应用包括人类活动理解、智能交通、城市规划、公共安全、卫生保健和环境管理。随着时空数据集的数量、容量和分辨率的迅速增加，传统的数据挖掘方法，特别是基于统计的方法正在变得不堪重负。近年来，随着深度学习技术的发展，卷积神经网络（CNN）和递归神经网络（RNN）等深度学习模型由于其强大的时空层次特征学习能力，在各种机器学习任务中取得了相当大的成功，并得到了广泛的应用在各种时空数据挖掘任务中，如预测学习、表示学习、异常检测和分类等。在这篇文章中，我们提供了一个全面的调查，在STDM中应用深度学习技术的最新进展。我们首先对时空数据的类型进行了分类，并简要介绍了STDM中常用的深度学习模型。然后介绍了一个框架，展示了深度学习模型在STDM中应用的一般流程。接下来，我们根据ST数据的类型、数据挖掘任务和深度学习模型对现有文献进行分类，然后介绍了深度学习在不同领域的应用，包括交通、气候科学、人类流动、基于位置的社会网络、犯罪分析和神经科学。最后，总结了目前研究的局限性，并指出了未来的研究方向。

###  <span id="p2">二、INTRODUCTION</span>
在大数据时代，随着地图、虚拟地球仪、遥感影像、十年一度的人口普查和GPS轨道等时空数据集的日益丰富和重要性，时空数据挖掘（STDM）正变得越来越重要。
STDM在环境与气候（如风预报、降水预报）、公共安全（如犯罪预测）、智能交通（如交通流预测）、人类活动（如人体轨迹模式挖掘）等各个领域有着广泛的应用。
传统的数据挖掘技术通常用于处理事务数据或图形数据，但由于种种原因，在应用于时空数据集时往往表现不佳。
首先，ST数据通常嵌入在连续空间中，而事务和图形等经典数据集通常是离散的。第二，ST数据的模式通常同时具有空间和时间特性，这一特性更加复杂，传统的方法很难捕捉到数据的相关性。最后，传统的基于统计的数据挖掘方法的一个常见假设是数据样本是独立生成的。然而，在时空数据的分析中，由于ST数据往往具有高度的自相关性，关于样本独立性的假设通常并不成立。
与传统方法相比，STDM的深度学习模式具有以下优点。

1)自动特征表示学习

与传统的需要手工制作特征的机器学习方法显著不同，深度学习模型可以从原始ST数据中自动学习分层特征表示。在STDM中，数据的空间邻近性和长期时间相关性通常比较复杂，难以捕捉。利用CNN的多层卷积运算和RNN的递归结构，可以直接从原始数据中自动有效地学习ST数据中的空间邻近性和时间相关性。

2)强大的函数逼近能力

从理论上讲，深度学习可以逼近任何复杂的非线性函数，只要有足够的层次和神经网络，就可以拟合任何曲线。深度学习模型通常由多个层次组成，每一层都可以看作是一个简单而非线性的模块，具有池、辍学和激活功能，从而将一个层次的特征表示转化为更高层次、更抽象的表示。通过组合足够多的这样的转换，可以学习非常复杂的函数来用更复杂的ST数据执行更困难的STDM任务。

###  <span id="p3">三、CATEGORIZATION OF SPATIO-TEMPORAL DATA</span>
#### 3.1 Data Types

在不同的实际应用中，不同类型的ST数据在数据收集和表示方式上存在差异。不同的应用场景和ST数据类型导致不同类别的数据挖掘任务和问题公式。
不同的深度学习模型通常对ST数据的类型有不同的偏好，对输入数据格式有不同的要求。
例如，CNN模型用于处理图像类数据，而RNN通常用于处理序列数据。因此，首先总结ST数据的一般类型并正确表示它们是很重要的。
我们遵循并扩展了文献[4]中的分类方法，将ST数据分为以下类型：
事件数据(event data)、轨迹数据(trajectory data)、点参考数据(point reference data)、光栅数据(raster data)和视频(videos)。
![Figure 1](../../../../../../img/in-post/2020.07/14/Figure 1.jpg)
Figure 1: Illustration of event and trajectory data types.

##### 3.1.1 Event data

事件数据包括在点位置和时间发生的离散事件（例如，城市犯罪事件和交通网络中的交通事故事件）。
一个事件通常可以用一个点位置和时间来描述，这分别表示事件发生的地点和时间。
例如，犯罪事件可以描述为这样一个元组$(e_i,l_i,t_i)$，其中$e_i$是犯罪类型，$l_i$是犯罪发生的地点，$t_i$是它发生的时间。
图1(a)示出了事件数据的图示。它显示了由符号的不同形状表示的三种类型的事件。
ST事件数据在现实世界的应用中很常见，例如犯罪学（犯罪率和相关事件）、流行病学（疾病爆发事件）、交通（车祸）和社交网络（社会事件和趋势主题）。

##### 3.1.2 Trajectory data
轨迹是指物体随时间在空间中运动的轨迹。（例如，自行车旅行或出租车旅行的行进路线）。
轨迹数据通常由部署在移动物体上的传感器收集，这些传感器可以随着时间的推移定期传送目标的位置，例如出租车上的GPS。
图1(b)示出了两个轨迹的图示。每个轨迹通常可以描述为这样一个序列$\lbrace(l_1,t_1),(l_2,t_2)\cdots (l_n,t_n)\rbrace$，其中$l_i$是位置(例如纬度和经度)，$t_i$是移动物体经过该位置的时间。
随着移动应用和物联网技术的发展，人体轨迹、城市交通轨迹和基于位置的社交网络等轨迹数据变得无处不在。

##### 3.1.3 Point reference data
![Figure 2](../../../../../../img/in-post/2020.07/14/Figure 2.jpg)
Figure 2: Illustration of ST reference point data in two time stamps.

点参考数据由一组空间和时间上的移动参考点上的连续ST场的测量值组成，如温度、植被或人口。
例如，温度和湿度等气象数据通常是使用漂浮在太空中的气象气球来测量的，气球会连续记录天气观测结果。
点引用数据通常可以表示为一组元组，如下所示$\lbrace(r_1,l_1,t_1),(r_2,l_2,t_2)\cdots (r_n,l_n,t_n)\rbrace$。
每个元组$(r_i,l_i,t_i)$表示在时刻$t_i$时ST场的位置$l_i$处传感器$r_i$的测量。
图2示出在两个时间戳处的连续ST场中的点参考数据（例如海面温度）的示例。
它们由传感器在两个时间戳上的参考位置（如while圆圈所示）进行测量。请注意，温度传感器的位置会随时间变化。

##### 3.1.4 Raster data
![Figure 3](../../../../../../img/in-post/2020.07/14/Figure 3.jpg)
Figure 3:  Illustration of raster data collected from traffic flow sensors.

光栅数据是在空间的固定位置和固定的时间点记录的连续或离散ST场的测量值。点参考数据和光栅数据的主要区别在于，点参考数据的位置不断变化，而光栅数据的位置是固定的。测量ST场的位置和时间可以有规律地或不规则地分布。给定m个固定位置S={s1，s2，…sm}和n个时间戳T={t1，t2，…tn}，栅格数据可以表示为矩阵Rm×n，其中每个条目rij是时间戳tj在位置si处的测量值。栅格数据在现实世界的应用中也很常见，比如交通、气候科学和神经科学。例如，空气质量数据（如PM2.5）可以通过部署在城市固定位置的传感器采集，而在一个连续的时间段内采集的数据则构成空气质量栅格数据。
在神经科学中，功能磁共振成像或功能磁共振成像（fMRI）通过检测与血流相关的变化来测量大脑活动。
扫描的fMRI信号也构成了分析大脑活动和识别某些疾病的光栅数据。图3示出了交通网络的交通流栅格数据的示例。
每条道路都部署了一个交通传感器来收集实时交通流量数据。所有道路传感器全天（24小时）的交通流量数据形成栅格数据。

##### 3.1.5 Video
由一系列图像组成的视频也可以被视为ST数据的一种类型。
在空间域中，相邻像素通常具有相似的RGB值，因此呈现出很高的空间相关性。
在时域中，连续帧的图像变化平缓，具有很高的时间依赖性。
一个视频通常可以用三维张量来表示，其中一个维度代表时间t，另两个维度代表图像。
实际上，如果我们假设在每个像素上都有一个“传感器”，并且在每一帧“传感器”将收集RGB值，那么视频数据也可以被视为一种特殊的光栅数据。
基于深度学习的视频数据分析是近年来研究的热点，发表了大量的论文。
虽然我们将视频归为ST数据的一类，但我们主要从数据挖掘的角度对相关工作进行回顾，视频数据分析属于计算机视觉和模式识别的研究领域。
因此，在本次调查中，我们不涉及ST数据类型的视频。

#### 3.2 Data Instances and Representations
数据挖掘算法操作的基本数据单元称为数据实例。
对于经典的数据挖掘设置，数据实例通常可以表示为一组特征，其中有一个用于监督学习的标签，或者没有用于无监督学习的标签。
在ST数据挖掘场景中，对于不同的ST数据类型，存在不同类型的数据实例。
对于不同的数据实例，有几种类型的数据表示形式，这些数据表示用于为深度学习模型的进一步挖掘制定数据。
![Figure 4](../../../../../../img/in-post/2020.07/14/Figure 4.jpg)
Figure 4: Data instances and representations of different ST data types.

##### 3.2.1 Data Instances
一般而言，ST数据可概括为以下数据实例：如图4的左侧所示的点(Points)、轨迹(Trajectories)、时间序列(Time Series)、空间地图(Spatial Maps)和ST光栅(ST Raster)。
一个ST点可以表示为一个元组，它包含空间和时间信息以及观察到的一些附加特征，例如犯罪或交通事故的类型。
除了ST事件外，轨迹和ST点参考也可以形成点。例如，可以将一条轨迹分成若干个离散点，以计算在特定时间段内有多少轨迹经过特定区域。
在某些应用中，轨迹除了可以表示为点和轨迹外，还可以形成时间序列。
如果我们确定位置并计算穿过该位置的轨迹数，它就形成了一个时间序列数据。
空间地图的数据实例包含在每个时间戳的整个ST字段中的所有传感器的数据观测。
例如，在时间t时部署在高速公路上的所有环路传感器的交通速度读数构成空间地图数据。
ST光栅数据的数据实例包含跨越整个位置集和时间戳的测量值。也就是说，ST光栅由一组空间地图组成。

根据不同的应用和分析要求，可以从ST-raster中提取不同的数据实例作为时间序列、空间地图或ST-raster本身。
首先，我们可以将ST字段的特定ST网格上的测量视为某些时间序列挖掘任务的时间序列。
其次，对于每一个时间戳，ST光栅的测量值可以看作是一个空间地图。
第三，我们也可以将跨越所有地点和时间戳的所有测量作为一个整体进行分析。在这种情况下，ST光栅本身可以是一个数据实例。

##### 3.2.2 Data representations
对于上述五种类型的ST数据实例，通常使用四种类型的数据表示来表示它们作为各种深度学习模型、序列、图形、二维矩阵和三维张量的输入，如图4的右侧所示。
不同的深度学习模型需要不同类型的数据表示作为输入。因此，如何表示ST数据实例依赖于所研究的数据挖掘任务和所选择的深度学习模型。

轨迹(Trajectories)和时间序列(Time series) 都可以表示成序列(sequences)。
轨迹有时候也可以表示为一个矩阵，其二维是网格ST字段的行和列id。
矩阵的每个输入值表示轨迹是否穿过相应的网格区域。
这种数据表示通常用于促进CNN模型的使用[67]、[118]、[142]。
虽然图(graph)也可以表示为矩阵，但这里我们将图(graph)和图像(image)矩阵分为两种不同的数据表示形式。
这是因为图节点不像图像矩阵那样遵循欧几里德距离，因此处理图和图像矩阵的方法是完全不同的。
空间地图可以表示为图形(graphs)和矩阵，这取决于不同的应用。
例如，在城市交通流预测中，城市交通网络的交通数据可以表示为交通流图(traffic flow graph)[85]、[155]或小区域级交通流矩阵(traffic flow matrix)[121]、[137]。
栅格数据通常表示为二维矩阵或三维张量。对于矩阵，二维是位置和时间步。
对于张量，三维是行区域单元id、列区域id和时间戳。
与张量相比，矩阵是一种更简单的数据表示格式，但它会丢失位置之间的空间相关性信息。
它们都被广泛用于表示栅格数据。
例如，在风力预报中，通常将部署在不同地点的多个风速仪的风速时间序列数据合并为一个矩阵，
然后输入CNN或RNN模型进行未来风速预测[96]，[200]。
在神经科学中，一个人的功能磁共振成像数据是一系列扫描的功能磁共振脑图像，
因此可以用像视频一样的张量来表示。
许多研究将fMRI图像张量作为CNN模型的输入进行特征学习，
以检测大脑活动[66]、[76]和疾病诊断[116]、[158]。

#### 3.3 Preliminary of Deep Learning Models
在本小节中，我们简要介绍了几种广泛应用于STDM的深度学习模型，包括RBM、CNN、graphnn、RNN、LSTM、AE/SAE和Seq2Seq。

##### 3.3.1 Restricted Boltzmann Machines (RBM)
![Figure 5](../../../../../../img/in-post/2020.07/14/Figure 5.jpg)
Figure 5: Structure of the RBM model.

RBM是一个两层随机神经网络[53]，可用于降维、分类、特征学习和协同过滤。
如图5所示，RBM的第一层被称为可见层，或带有神经元节点$\lbrace v_1,v_2,\cdots,v_n \rbrace$的输入层,
第二层是带有神经元结点$\lbrace h_1,h_2,\cdots,h_m \rbrace$。
RBM中的所有节点通过无向权边$\lbrace w_{11},\cdots,w_{nm}\rbrace$跨层连接.RBM通常用于学习特征。

##### 3.3.2 CNN
![Figure 6](../../../../../../img/in-post/2020.07/14/Figure 6.jpg)
Figure 6: Structure of the CNN model.

卷积神经网络（CNN）是一类用于视觉图像分析的深度前馈人工神经网络。
一个典型的CNN模型通常包含以下层，如图6所示：输入层、卷积层、池层、完全连接层和输出层。
卷积层通过计算神经元权值与输入体积连接区域之间的标量积来确定连接到输入局部区域的神经元的输出。
然后，池层将简单地沿着给定输入的空间维度执行下采样，以减少参数的数量。
全连通层将一层中的每一个神经元连接到下一层的每一个神经元，学习最终的特征向量进行分类。
它在原理上与传统的多层感知器神经网络（MLP）相同。
与传统的MLPs相比，CNNs具有以下特点：神经元的三维体积、局部连通性和共享权值。
CNN是用来处理图像数据的。
由于其强大的空间相关性捕获能力，目前已被广泛应用于ST数据的挖掘，特别是空间地图和ST-raster。

##### 3.3.3 GraphCNN
![Figure 7](../../../../../../img/in-post/2020.07/14/Figure 7.jpg)
Figure 7: Structure of GraphCNN model.

CNN被设计用来处理在欧几里得空间中可以用规则网格表示的图像。
然而，在许多应用中，数据是从非欧几里德域（如图）生成的。
graphnn最近被广泛研究，将CNN推广到图结构数据[160]。
图7示出了graphnn模型的结构示意图。
图卷积操作将卷积变换应用于每个节点的邻居，然后进行池操作。
通过叠加多个图卷积层，每个节点的潜在嵌入可以包含来自多跳距离的邻居的更多信息。
在生成图中节点的潜在嵌入后，
既可以方便地将潜在嵌入量反馈给前馈网络以实现回归目标的节点分类，
也可以将所有节点嵌入量聚合起来表示整个图，然后进行图分类和回归。
由于它能够很好地捕捉节点间的相关性和节点的特征，
因此被广泛地应用于网络规模的交通流数据和脑网络数据等图结构的ST数据挖掘中。

##### 3.3.4 RNN&LSTM
![Figure 8](../../../../../../img/in-post/2020.07/14/Figure 8.jpg)
Figure 8: Structure of the RNN and LSTM models.

递归神经网络（RNN）是一类人工神经网络，节点之间的连接沿序列形成有向图。
RNN被设计用来识别序列特征并使用模式来预测下一个可能的场景。
它们广泛应用于语音识别和自然语言处理等领域。
图8（a）示出了RNN模型的一般结构，其中$X_t$是输入数据，a是网络的参数，
$h_t$是学习的隐藏状态。
我们可以看到上一个时间步骤$t−1$的输出（隐藏状态）被输入到下一个时间步骤t的神经网络中，
从而可以存储历史信息并将其传递给未来。

标准RNN的一个主要问题是由于梯度消失的问题，它只有短期记忆。
长短期记忆（LSTM）网络是递归神经网络的扩展，它能够学习输入数据的长期相关性。
LSTM使得RNN能够长时间地记住它们的输入，
是由于图8（b）的中间部分所示的特殊存储器单元。
LSTM单元由三个门组成：输入门、遗忘门和输出门。
这些门决定是否让新的输入进入（输入门），删除不重要的信息（忘记门），
还是让它影响当前时间步长的输出（输出门）。
RNN和LSTM都被广泛用于处理序列和时间严重的数据，以学习ST数据的时间依赖性。

##### 3.3.5 Seq2Seq
![Figure 9](../../../../../../img/in-post/2020.07/14/Figure 9.jpg)
Figure 9: Structure of Seq2Seq model.

序列到序列（Seq2Seq）模型旨在将固定长度的输入映射到固定长度的输出，
其中输入和输出的长度可能不同[138]。
它被广泛应用于机器翻译、语音识别和在线聊天机器人等各种自然语言处理任务。
虽然Seq2Seq最初是用来解决NLP任务的，但Seq2Seq是一个通用框架，
可以用于任何基于序列的问题。
如图9所示，Seq2Seq模型一般由3部分组成：编码器、中间（编码器）向量和解码器。
由于Seq2Seq模型能够很好地捕捉序列数据之间的相关性，
因此它被广泛地应用于ST数据具有很高时间相关性的ST预测任务中，
如城市人群流量数据和交通数据。

##### 3.3.6 Autoencoder (AE) and Stacked AE
![Figure 10](../../../../../../img/in-post/2020.07/14/Figure 10.jpg)
Figure 10: Structure of the one-layer AE model.

自动编码是一种高效的神经网络编码方法。
如图10所示，它具有编码器功能来创建隐藏层（或多层），
其中包含描述输入的代码。然后有一个解码器，它从隐藏层重建输入。
自动编码器通过学习数据中的相关性，在隐藏层或瓶颈层创建数据的压缩表示，
这可以看作是降维的一种方法。作为一种有效的无监督特征表示学习技术，
AE有助于实现各种下游数据挖掘和机器学习任务，如分类和聚类。
堆叠式自动编码器（SAE）是由多层稀疏自动编码器组成的神经网络，
每层的输出连接到连续层的输入[7]。

![Figure 11](../../../../../../img/in-post/2020.07/14/Figure 11.jpg)
Figure 11: Data representation for different DL models.

###  <span id="p4">四、FRAMEWORK</span>
![Figure 12](../../../../../../img/in-post/2020.07/14/Figure 12.jpg)
Figure 12: A general pipeline for using DL models for ST data mining.

在本节中，我们将介绍如何使用深度学习模型来解决STDM问题。
首先，我们将给出一个包含ST数据实例构造、ST数据表示、深度学习模型划分与设计的流水线框架，并最终解决问题。
接下来我们将详细介绍这些主要步骤。

图12显示了用于ST数据挖掘的深度学习模型的一般流程。
根据从各种位置传感器采集的原始ST数据，包括事件数据、轨迹数据、点参考数据和光栅数据，
首先构造数据实例进行数据存储。
如前所述，ST数据实例可以是点、时间序列、空间地图、轨迹和ST光栅。
为了将深度学习模型应用于各种挖掘任务，
需要将ST数据实例进一步表示为适合深度学习模型的特定数据格式。
ST数据实例可以表示为序列数据、2D矩阵、3D张量和图形。
然后针对不同的数据表示，采用不同的深度学习模型进行处理。
RNN和LSTM模型能够很好地处理具有短期或长期时间相关性的序列数据，
而CNN模型能够有效地捕捉类图像矩阵中的空间相关性。
结合RNN和CNN的混合模型可以捕获ST栅格数据的张量表示的空间和时间相关性。
最后，将所选的深度学习模型应用于预测、分类、表征学习等多种STDM任务。

#### 4.1 ST Data Preprocessing
ST数据预处理的目的是将ST数据实例表示为深度学习模型能够处理的适当的数据表示格式。
通常，深度学习模型的输入数据格式可以是向量、矩阵或张量，具体取决于不同的模型。
图11示出ST数据实例及其对应的数据表示。
我们可以看到，通常一种类型的ST数据实例对应于一种典型的数据表示。
轨迹和时间序列数据可以自然地表示为序列数据。空间地图数据可以用二维矩阵表示。
ST光栅可以表示为二维矩阵或三维张量。

然而，情况并非总是如此。例如，轨迹数据有时用矩阵表示，然后应用CNN模型更好地捕捉空间特征[24]、[67]、[103]、[117]、[150]。
首先将测量轨迹的ST场（如城市）划分为网格单元区域。然后将ST字段建模为一个矩阵，每个单元格区域代表一个条目。
如果轨迹经过单元格区域，则相应的输入值设置为1；否则设置为0。
这样，轨迹数据可以用矩阵表示，从而可以应用CNN。有时，空间地图用图形表示。
例如，部署在高速公路上的传感器通常被建模为一个图，其中节点是传感器，边缘表示两个相邻传感器之间的路段。
在这种情况下，graphnn模型通常用于处理传感器图数据并预测所有节点的未来流量（流量、速度等）[22]，[85]。
ST光栅数据可以表示为二维矩阵或三维张量，这取决于数据类型和应用。
例如，一系列功能磁共振成像脑图像数据可以表示为张量并输入到3D-CNN疾病分类模型中[78]，[116]，也可以通过提取大脑成对区域之间的时间序列相关性来表示为矩阵，以便进行脑活动分析[48]，[113]。

#### 4.2 Deep Learning Model Selection & Design
利用ST数据实例的数据表示，下一步是将它们输入到针对不同STDM任务选择或设计的深度学习模型中。
如图11的右侧所示，对于每种类型的数据表示有不同的深度学习模型选项。
序列数据可以作为模型的输入，包括RNN、LSTM、GRU、Seq2Seq、AE、混合模型等。
RNN、LSTM和GRU都是适合于序列数据预测的递归神经网络。
序列数据也可以用Seq2Seq模型进行处理。
例如，在多步骤业务量预测中，
通常应用由编码器层的一组LSTM单元和解码器层的一组LSTM单元组成的Seq2Seq模型来同时预测下几个时隙中的业务速度或流量[89]、[90]。
作为一种特征学习模型，AE或SAE可以用于不同的数据表示来学习低维特征编码。
序列数据也可以用AE或SAE编码为低维特征。
graphnn专门用来处理图数据，以捕捉相邻节点之间的空间相关性。
如果输入是一个单一的矩阵，通常采用CNN模型；
如果输入是一个矩阵序列，则可以根据所研究的问题应用RNN模型、convlsm模型和混合模型。
如果目标仅仅是特征学习，则可以应用AE和SAE模型。
对于张量数据，通常采用3D-CNN或3D-CNN与RNN模型相结合的方式进行处理。

Table 1 DIFFERENT DL MODELS FOR PROCESSING FOUR TYPES OF ST DATA.
![Table 1](../../../../../../img/in-post/2020.07/14/Table 1.jpg)

表一总结了使用深度学习模型处理不同类型的ST数据的工作。
如表所示，CNN、RNN及其变体（如graphnn和convlsm）是STDM中使用最广泛的两种深度学习模型。
CNN模型主要用于空间地图和ST栅格的处理。一些工作也使用CNN来处理轨迹数据，
但是目前还没有使用CNN进行时间序列数据学习的工作。
graphnn模型是专门为处理图形数据而设计的，它可以划分为空间地图。
包括LSTM和GRU在内的RNN模型可以广泛应用于轨道、时间序列和空间地图序列的处理。
convlsm可以看作是RNN和CNN的混合模型，通常用于处理空间地图。
AE和SDAE主要用于从时间序列、轨迹和空间地图中学习特征。
Seq2Seq模型一般是针对序列数据设计的，因此只用于处理时间序列和轨迹。
混合模型在STDM中也很常见。
例如，可以将CNN和RNN进行叠加，首先学习空间特征，然后捕捉历史ST数据之间的时间相关性。
混合模型可以设计成适合所有四种类型的数据表示。
其他模型，如网络嵌入[164]、多层感知器（MLP）[57]、[186]、生成对抗网(GAN)[49]、[93]、残差网[78]、[89]、深度强化学习[50]等也在最近的研究中得到了应用。

###  <span id="p5">五、DEEP LEARNING MODELS FOR ADDRESSING DIFFERENT STDM PROBLEMS</span>
![Figure 13](../../../../../../img/in-post/2020.07/14/Figure 13.jpg)
Figure 13:Distributions of the STDM problems addressed by deep learning models.

在这一部分，我们将对STDM问题进行分类，并介绍相应的深度学习模型来解决这些问题。图13示出了由深度学习模型解决的各种STDM问题的分布，包括预测、表示学习、检测、分类、推断/估计、推荐等。可以看出，研究的STDM问题中最大的一类是预测。70%以上的论文侧重于研究预测问题。这主要是因为准确的预测很大程度上依赖于高质量的特征，而深度学习模型在特征学习方面尤其强大。第二大类问题是表征学习，其目的是以无监督或半监督的方式学习各种ST数据的特征表示。深度学习模型也可用于其他STDM任务，包括分类、检测、推理/估计、推荐等。接下来我们将详细介绍STDM的主要问题，并总结相应的基于深度学习的解决方案。

#### 5.1 Predictive Learning
预测学习的基本目标是根据ST数据的历史数据预测未来的观测值。对于不同的应用，输入和输出变量都可以属于不同类型的ST数据实例，从而产生各种预测学习问题的公式。接下来，我们将介绍基于ST数据实例类型作为模型输入的预测问题。

##### 5.1.1 Points
通常在时间域或空间域中对点进行合并，以形成时间序列或空间地图，如犯罪[31]、[57]、[145]、[56]、交通事故[201]和社会事件[43]，因此可以应用深度学习模型。[145]采用ST-ResNet模型预测洛杉矶地区的犯罪分布。他们的模型包含两个阶段。首先，他们通过合并发生在同一时间段和城市区域的所有犯罪事件，将原始犯罪点数据转换为类似图像的犯罪热图。然后，他们采用残差卷积单元的层次结构，以犯罪热图为输入训练犯罪预测模型。同样，[57]建议使用GRU模型来预测一个城市的犯罪。[201]利用卷积长短期记忆（convlsm）神经网络模型研究了交通事故预测问题。他们还首先合并了交通事故的点数据，并将交通事故数在时空场中建模为三维张量。张量的每个条目（i，j，t）代表时隙t中网格单元（i，j）处的交通事故计数。将历史交通事故张量输入CovnLSTM进行预测。[43]提出了一个空间不完全多任务深度学习框架，有效地预测未来发生在不同地点的事件的子类型.

##### 5.1.2 Time series
在道路交通预测中，道路或高速公路上的交通流数据可以建模为时间序列。近年来，许多研究尝试了各种深度学习模型用于道路交通预测[104]、[136]、[191]。[104]首次利用堆叠式自动编码器从交通流时间序列数据中学习特征，用于路段级交通流预测。[136]将高速公路上的交通流数据视为时间序列，并提出基于先前的交通流观测结果，使用深度置信网络（DBNs）来预测未来的交通流。[126]研究了出租车需求预测问题，并将特定区域的出租车需求建模为时间序列。深度学习模式提出全连通层，从出租车需求的历史时间序列中学习特征，然后将特征与天气和社交媒体文本等其他上下文特征相结合，预测未来的需求。

RNN和LSTM被广泛应用于时间序列ST数据的预测。[90]集成LSTM和序列到序列模型来预测路段的交通速度。除了交通速度信息，他们的模型还考虑了其他外部特征，包括道路的地理结构、公共社会事件（如国庆活动）和在线人群出行信息查询。风速等天气变量通常也被建模为时间序列，然后RNN/LSTM模型应用于未来天气预报[14]、[17]、[55]、[97]、[124]、[179]。例如，[17]提出了概率风速预测的集合模型。该模型将小波阈值去噪（WTD）和自适应神经模糊推理系统（ANFIS）等传统风速预测模型与递归神经网络（RNN）相结合。在功能磁共振成像数据分析领域，功能磁共振时间序列数据通常用于研究功能性脑网络和疾病诊断。[34]建议直接从静息状态fMRI时间序列中使用LSTM模型对自闭症谱系障碍（ASD）和典型对照者进行分类。[59]开发了一个名为DCAE的深卷积自动编码器模型，用于无监督地从复杂、大规模的tfMRI时间序列中学习中高级特征。时间序列数据通常不包含空间信息，因此在基于深度学习的预测模型中没有明确考虑数据之间的空间相关性。

##### 5.1.3 Spatial maps
间地图通常可以表示为像图像一样的矩阵，因此适合用CNN模型进行预测学习[69]、[80]、[184]、[200]。[184]提出了一种基于CNN的预测模型来捕捉城市乌鸦流预测的空间特征。建立了一个实时人群流量预测系统UrbanFlow，以人群流动空间图为输入。为了预测搭车服务的供需关系，[69]提出了基于六边形的卷积神经网络（H-CNN），其输入和输出都是大量的局部六边形映射。与以往将城市区域划分为多个正方形网格的研究不同，他们提出将城市区域划分为各种规则的六边形网格，因为六边形分割具有明确的邻域定义、较小的边面积比和各向同性。一个监测点的风速数据可以建模为时间序列，而多个监测点的风速数据可以用空间地图表示。CNN模型也可用于同时预测多个站点的风速[200]。

在给定一系列空间地图的情况下，许多研究试图将CNN和RNN结合起来进行预测。[161]提出了一种卷积LSTM（convlsm），并将其应用于降水实时预报问题的端到端可训练模型。本文将CNN的卷积结构与LSTM相结合，在序列到序列学习框架下对时空序列进行预测。convlsm是一种序列到序列的预测模型，它的每一层都是一个convlsm单元，在输入到状态和状态到状态的转换中都有卷积结构。模型的输入和输出都是空间地图矩阵。在这项工作之后，许多研究尝试将convlsm应用于其他不同领域的空间地图预测任务[1]、[6]、[28]、[70]、[73]、[98]、[151]、[198]。[151]提出了一种新的用于深空时预测的跨城市迁移学习方法，称为RegionTrans。RegionTrans包含多个ConvLSTM层来捕捉隐藏在数据中的时空模式。[73]应用ConvlTM网络，利用多通道雷达数据预测降水。[198]提出了一种端到端的深度神经网络来预测按需移动（MOD）服务中的乘客上下车需求。基于卷积单元和convlsm单元的编解码框架被用于识别复杂特征，这些特征捕捉了城市乘客需求的时空影响和接送交互作用。将城市单元区域的旅客需求建模为空间地图，并用矩阵表示。同样，[1]提出了一个融合ConvLSTM层、标准LSTM层和卷积层的FCL网络模型，用于预测按需乘车服务下的乘客需求。[98]提出了一个统一的神经网络模块，称为注意人群流动机器（ACFM）。ACFM能够通过学习具有注意机制的时变数据的动态表示来推断人群流的演化。ACFM由两个渐进的convlsm单元和一个用于空间权重预测的卷积层组成。

其他一些模型也可用于预测空间地图，如Graphnn[8]、[22]、[92]、[144]、ResNet[146]、[183]、[185]和混合方法[49]、[109]、[177]。注：在本文中，我们认为空间地图既包含图像数据，也包含图形数据。虽然图也被表示为矩阵，但它们需要完全不同的技术，如graphnn或graphnn。在路网规模交通预测中，交通网络可以自然地建模为图，然后应用graphnn或graphnn。[85]提出将交通网络上的交通流建模为有向图上的扩散过程，并引入扩散卷积递归神经网络（DCRNN）进行交通预测。它在整个路网的交通流中同时考虑了空间和时间的依赖性。具体地说，DCRNN利用图上的双向随机游动来捕获空间相关性，并使用带定时采样的编解码器架构来捕获时间相关性。[155]提出了一种新的拓扑结构，称之为连接网络来对道路网络进行建模，并给出了交通流的传播模式。基于链接网络模型，设计了一种新的在线预测器，即图递归神经网络（GRNN），用于学习图中的传播模式。它根据从整个图形中收集的信息同时预测所有路段的交通流量。[144]引入了ST加权图（STWG）来表示稀疏时空数据。建立了可扩展的小尺度数据预测图。

##### 5.1.4 Trajectories
目前，基于轨迹数据表示的轨迹预测主要采用RNN和CNN两种深度学习模型。首先，轨迹可以表示为如图11所示的位置序列。在这种情况下，可以应用RNN和LSTM模型[38]、[64]、[77]、[88]、[135]、[163]、[165]。[163]提出了无碰撞LSTM，它扩展了经典LSTM，通过增加斥力池层来共享相邻行人的隐藏状态来预测人体轨迹。无碰撞LSTM可以根据行人过去的位置生成未来序列。[64]研究了城市人的流动性预测问题，该问题给出了一个人观察到的几步流动性，试图预测他/她下一步在城市中的去向。他们提出了一种基于RNN的深度序列学习模型，以有效地预测城市人类的流动性。[135]提出了一个名为DeepTransport的模型，从一组个体的GPS轨迹中预测步行、乘火车、乘公共汽车等交通方式。利用四个LSTM层构建了DeepTransport，以预测用户未来的运输方式。

轨迹也可以用矩阵表示。在这种情况下，CNN模型可用于更好地捕捉空间相关性[67]、[103]、[142]。[67]提出了一种基于CNN的表示语义轨迹和预测未来位置的方法。在语义轨迹中，每个访问过的位置都与一个语义意义相关联，比如家、工作、购物中心，等，他们将语义轨迹建模为语义和轨迹ID两个维度的矩阵，并将其输入到具有多个卷积层的CNN中，学习下一次访问语义位置预测的潜在特征。[103]将轨迹建模为二维图像，其中图像的每个像素表示轨迹中是否访问了相应的位置。然后采用多层卷积神经网络结合多尺度轨迹模式对出租车轨迹进行终点预测。将轨迹建模为像图像一样的矩阵也用于其他任务，如异常检测和推断[111]，[150]，稍后将详细介绍。

##### 5.1.5 ST raster
如前所述，ST光栅数据可以表示为二维为位置和时间的矩阵，或三维为单元区域ID、单元区域ID和时间的张量。在ST栅格数据预测中，通常采用2DCNN（矩阵）和3D-CNN（张量），有时还与RNN相结合。[188]提出了一个名为3D-SCN的多通道三维立方体连续卷积网络，用于从3D雷达数据中实时预报风暴的发生、增长和平流。[121]将连续时间段内道路多个位置的交通速度数据建模为ST栅格矩阵，然后输入到深度神经网络中进行交通流预测。[106]探索了与[121]相似的思想，用于大型交通网络的交通预测。[12] 提出了一种用于城市交通流预测的三维卷积神经网络。他们没有预测道路上的交通量，而是试图预测城市每个小区的车辆流量。因此，他们将连续时间段内的全市车辆流量数据建模为ST-rasters，并将其输入到所提出的3DCNN模型中。类似地，[131]将城市中不同时间段内乘客的流动事件建模为三维张量，然后利用3D-CNN模型预测乘客对交通的供求关系。注意，ST光栅图和空间图的主要区别在于ST光栅是多个时隙的合并ST场测量，而spatial map是仅在一个时隙中进行的ST场测量。因此，根据实际应用场景和数据分析的目的，同一类型的ST数据有时可以表示为空间地图和ST栅格。

#### 5.2 Fusing Multi-Sourced Data
除了正在研究的ST数据，通常还有一些其他类型的数据与ST数据高度相关。将这些数据与ST数据融合在一起通常可以提高各种STDM任务的性能。例如，城市交通流数据会受到天气、社会事件、节假日等外部因素的显著影响。最近的一些研究尝试将ST数据和其他类型的数据融合到一个深度学习架构中，以联合学习特征并捕捉它们之间的相关性[16]、[19]、[89]、[174]、[178]、[188]、[201]。在应用深度学习模型进行STDM的多源数据融合时，一般有两种方法：原始数据级融合和潜在特征级融合。

##### 5.2.1 Raw data-level fusion
对于原始数据级融合，首先将多源数据集成，然后输入到深度学习模型中进行特征学习。[201]利用卷积长短期记忆（convlsm）神经网络模型研究了交通事故预测问题。首先，将整个研究区域划分为网格单元。然后收集到交通量、道路状况、降雨量、温度和卫星图像等细粒度的城市和环境特征，并与每个网格单元匹配。以事故数和上述每个位置的外部特征作为模型输入，提出了一种预测未来时段内每个网格单元将发生的事故数的异方差convlsm模型。[19] 提出了ADAIN模型，该模型融合了监测站的城市空气质量信息和与空气质量密切相关的城市数据，包括poi、路网和气象数据，推断出一个城市的精细城市空气质量。ADAIN模型的框架如图15所示。首先从道路网、泊松分布、气象数据和城市空气质量指数等多源数据中手工提取特征。然后将所有特征融合在一起，分别输入到FNN和RNN模型中进行特征学习。

##### 5.2.2 Latent feature-level fusion
对于潜在特征级融合，首先将不同类型的原始特征输入到不同的深度学习模型中，然后使用一个潜在特征融合组件对不同类型的潜在特征进行融合。[89]提出了一种基于深度学习的方法ST-ResNet，该方法基于残差神经网络框架，对城市各个区域的人群流入和流出进行集体预测。如图16所示，STResNet处理两种类型的数据，城市中的ST人群流数据序列和包括天气和假日事件在内的外部特征。设计两个分量分别学习外部特征和人群流量数据特征的潜在特征，然后利用特征融合函数tanh将两类学习的潜在特征进行融合。[174]提出了一个深度多视角时空网络（DMVST-Net）框架，将多视角数据结合起来进行出租车需求预测。DMVST网络由三个视图组成：时间视图、空间视图和语义视图。CNN用于从空间角度学习特征，LSTM用于从时间角度学习特征，网络嵌入用于学习区域间的相关性。最后，应用全连通神经网络融合三种观点的潜在特征进行出租车需求预测。

#### 5.3 Attention
注意力是一种机制，它是为提高编解码器RNN在机器翻译上的性能而开发的[5]。编解码器RNN的一个主要缺陷是它将输入序列编码成固定长度的内部表示，这导致长输入序列的性能下降。为了解决这个问题，attention允许模型学习源序列中的哪些编码单词需要关注，以及在预测目标序列中每个单词的过程中关注的程度。虽然注意最初是在以词序数据为输入的机器翻译中提出的，但它实际上可以应用于任何类型的输入，如图像，这就是视觉注意。由于许多ST数据可以表示为序列数据（时间序列和轨迹）和类似图像的空间地图，因此还可以将注意力纳入深度学习模型，以提高各种STDM任务的性能[19]、[38]、[39]、[57]、[81]、[88]、[98]、[142]、[198]。

STDM中使用的神经注意机制一般可分为空间域注意[19]、[39]和时域注意[38]、[57]、[81]、[198]。一些作品同时使用了空间域和时间域的注意[88]，[98]，[142]。[39]提出了一种空间域的组合注意模型。它利用“软注意”和“硬连线”注意，以便将从本地邻居的轨迹信息映射到感兴趣的行人的未来位置。[57]提出了一种值得关注的分层递归网络模型DeepCrime，用于犯罪预测。该方法利用时域注意机制，捕捉从前一时间段学习到的犯罪模式之间的相关性，帮助预测未来的犯罪发生，并在不同的时间框架内自动为学习到的隐藏状态分配重要权重。在所提出的注意机制中，犯罪发生在过去时间段内的重要性是通过一个softmax函数推导一个归一化的重要性权重来估计的。[88]提出了一个多层次的注意网络，用于预测部署在不同地理空间位置的传感器产生的地理感测时间序列，以连续、协同地监测周围环境，如空气质量。具体而言，在第一级注意中，提出了一种由局部空间注意和全局空间注意组成的空间注意机制，以捕捉不同传感器时间序列之间的复杂空间相关性。在第二级注意中，采用时间注意来模拟时间序列中不同时间间隔之间的动态时间相关性。


###  <span id="p6">六、APPLICATIONS</span>
大量的ST数据来自于不同的应用领域，如交通、按需服务、气候和天气、人员流动、基于位置的社会网络（LBSN）、犯罪分析和神经科学。表二展示了上述应用领域的相关工作。可以看出，由于城市交通数据和人员流动数据的可用性不断增加，大部分工程属于交通和人员流动。在本节中，我们将描述深度学习技术在不同应用中的应用。

#### 6.1 Transportation
随着从各种传感器（如环路检测器、道路摄像机和GPS）收集的交通数据的可用性不断提高，迫切需要利用深度学习方法来学习交通数据之间复杂且高度非线性的时空相关性，以促进各种任务，如交通流预测[30]，[60]、[90]、[121]、[136]、[167]、交通事故检测[125]、[189]、[199]和交通拥堵预测[108]、[137]。这些与交通相关的ST数据通常包含交通速度、交通量或交通事件、区域路段位置和时间的信息。在不同的应用场景下，交通数据可以建模为时间序列、空间地图和ST栅格。例如，在道路网络规模的交通流预测中，从多个道路环路传感器收集的交通流数据可以建模为栅格矩阵，其中一个维度是传感器的位置，另一个维度是时间槽[106]。环路传感器也可以基于传感器部署的道路链路之间的连接作为传感器图进行连接，并且可以将道路网络的交通数据建模为图形空间地图，以便应用graphnn模型[85]，[175]。而在道路交通预测中，将每条道路上的历史交通流数据建模为一个时间序列，然后使用RNN或其他深度学习模型对单个道路进行交通预测[60]、[90]、[167]。

#### 6.2 On-Demand Service
近年来，由于手机的广泛使用，Uber、Mobike、滴滴、GoGoVan等各种点播服务日益普及。按需服务通过为人们提供他们想要的东西和地点，取代了传统的业务。许多按需服务产生大量的ST数据，这些数据涉及到客户的位置和所需的服务时间。例如，优步和滴滴分别是美国和中国最受欢迎的共享单车服务提供商。这两家公司都通过智能手机应用程序向用户提供出租车招呼、私家车招呼和社交共享服务。为了更好地满足顾客的需求，提高服务质量，一个关键的问题就是如何准确地预测不同地点、不同时间的服务需求和供给。深度学习方法在按需服务中的应用主要集中在预测需求和供给上。[1] 提出应用深度学习方法预测无码头自行车运动系统的供需分布。[92]提出了一个图形CNN模型来预测大规模自行车共享网络中的站点级小时需求。[126]、[174]提出用LSTM模型预测不同地区的出租车需求。[146]应用ResNet模型预测在线叫车服务的供需。研究中城市不同区域的历史需求供给通常被建模为空间地图或栅格张量，因此应用CNN、RNN和组合模型对未来进行预测。

#### 6.3 Climate & Weather
气候科学是对气候的科学研究，科学定义为一段时间内平均的天气状况。气象数据通常包含大气和海洋条件（例如，温度、压力、风流和湿度），这些数据由部署在固定或浮动位置的各种气候传感器收集。由于不同地区的气候资料往往具有很高的时空相关性，STDM技术被广泛应用于短期和长期天气预报。特别是随着深度学习技术的最新进展，许多研究尝试将深度学习模型用于分析各种天气和环境数据[79]、[129]，如空气质量推断[19]、[94]、降水预测[100]、[161]、风速预测[96]、[200]和极端天气检测[100]。与气候和天气相关的数据可以是空间地图（例如雷达反射率图像）[188]、时间序列（例如风速）[17]和事件（例如极端天气事件）[100]。[19] 提出了一种神经注意模型来预测不同监测站的城市空气质量数据。[100]建议在气候数据库中使用CNN模型来检测极端天气。CNN模型也可用于从遥感图像中估计降水量[100]。

#### 6.4 Human Mobility
随着移动设备的广泛应用，近年来与人类活动相关的地理定位数据集激增。大量的人类流动数据使我们能够定量研究个体和集体的人类流动模式，并生成能够捕捉和再现人类运动轨迹时空结构和规律的模型。在人口流动预测、人口流动预测等城市交通流预测等方面具有重要的应用价值。应用于人类移动数据的深度学习技术主要集中在人体轨迹数据挖掘上，如轨迹分类[36]、轨迹预测[38]、[64]、[163]、轨迹表示学习[82]、[170]、机动性模式挖掘[118]、以及从轨迹推断人类运输模式[24]、[42]。根据不同的应用场景和分析目的，可以将人的轨迹建模为不同类型的ST数据类型和数据表示，从而可以应用不同的深度学习模型。在人体轨迹数据挖掘中应用最广泛的模型是RNN和CNN模型，有时这两种模型结合起来以获取人体运动数据之间的空间和时间相关性。

#### 6.5 Location Based Social Network (LBSN)
Foursquare和Flickr等基于位置的社交网络是使用GPS功能定位用户并让用户通过移动设备广播他们的位置和其他内容的社交网络[196]。LBSN不仅意味着在现有的社交网络中添加一个位置，以便人们可以共享位置嵌入的信息，而且还包括一个新的社会结构，这个社会结构由个人在物理世界中的位置以及他们的位置标记的媒体内容组成。LBSN数据包含大量用户签入数据，这些数据由个人在给定时间戳上的即时位置组成。目前，对LBSN中用户生成的ST数据的分析已经采用了深度学习的方法，研究的任务包括下一次值机位置预测[67]、LBSN中的用户表示学习[164]、地理特征提取[26]和用户报到时间预测[165]。

#### 6.6 Crime Analysis
执法机构在许多城市存储有关已报告犯罪的信息，并将犯罪数据公开用于研究目的。犯罪事件数据通常包含犯罪类型（例如纵火、袭击、入室盗窃、抢劫、盗窃和故意破坏），以及犯罪的时间和地点。犯罪模式和执法政策对一个地区犯罪数量的影响可以利用这些数据进行研究，目的是减少犯罪[4]。由于发生在城市不同区域的犯罪通常具有很高的时空相关性，因此可以使用深度学习模型，以一个城市的犯罪账户热图为输入，捕捉这种复杂的相关性[31]、[57]、[145]。例如，[31]提出了一个基于CNN的时空犯罪网络来预测未来一天城市中各个区域的犯罪风险。[145]建议利用ST-ResNet模型来集体预测洛杉矶地区的犯罪分布。[57]开发了一个新的犯罪预测框架——深度犯罪，这是一个深度神经网络架构，它揭示了动态犯罪模式，并探索了犯罪与城市空间中其他普遍存在的数据之间不断演变的相互依赖关系。如前所述，犯罪数据是典型的ST事件数据，但通常通过合并时空域中的数据来表示为空间地图，以便将深度学习模型应用于分析。

#### 6.7 Neuroscience
近年来，脑成像技术已成为神经科学领域的一个热门话题。这些技术包括功能磁共振成像（fMRI）、脑电图（EEG）、脑磁图（MEG）和功能性近红外光谱（fNIRS）。这些技术测量的神经活动的空间和时间分辨率与其他技术有很大不同。功能磁共振成像（fMRI）从数百万个位置测量神经活动，而脑电数据仅从数十个位置测量。功能磁共振成像通常每两秒钟测量一次活动，而脑电图数据的时间分辨率通常是1毫秒。功能磁共振成像（fMRI）和脑电图（EEG）结合深度学习的方法，因其具有空间分辨率而被广泛应用于神经科学的研究[34]、[63]、[113]、[128]。如前所述，在神经科学中，深度学习模型主要是利用功能磁共振成像（fMRI）数据或EEG数据（如疾病分类[34]、脑功能网络分类[113]和脑激活分类[63]来完成分类任务。例如，长短期记忆网络（LSTM）用于识别自闭症谱系障碍（ASD）[34]，卷积神经网络（CNN）用于诊断遗忘性轻度认知障碍（aMCI）[113]，前馈神经网络（FNN）用于精神分裂症的分类[119]。


###  <span id="p7">七、 OPEN PROBLEMS</span>
#### 7.1 Interpretable models
目前针对STDM的深度学习模型大多被认为是缺乏可解释性的黑匣子。可解释性赋予深度学习模型以可理解的方式解释或呈现模型行为的能力，它是机器学习模型为了更好地服务于人和为社会带来利益而不可或缺的一部分[29]。考虑到ST数据的复杂数据类型和表示形式，与其他类型的数据（如图像和单词标记）相比，设计可解释的深度学习模型更具挑战性。虽然注意机制被用于提高模型的可解释性，如周期性和局部空间依赖性[19]，[57]，[88]，但是如何为STDM任务建立一个更具解释性的深度学习模型仍然是一个有待研究的问题。

#### 7.2 Deep learning model selection
对于给定的STDM任务，有时可以收集多种类型的相关ST数据，并且可以选择不同的数据表示形式。如何正确地选择ST数据表示和相应的深度学习模式，目前还没有很好的研究。例如，在交通流预测中，有些作品将每条道路的交通流数据建模为一个时间序列，以便使用RNN、DNN或SAE进行预测[104]、[136]；有些作品将多个路段的交通流数据建模为空间地图，以便应用CNN进行预测[184]；一些研究将路网的交通流数据建模为一个图，因此采用了GraphCNN[85]。如何正确地选择深度学习模型和ST数据的数据表示，以更好地解决所研究的STDM任务，缺乏更深入的研究。

#### 7.3 Fusing multi-modal ST datasets
在大数据时代，多模式ST数据集在神经影像学、气候科学和城市交通等领域的应用越来越广泛。例如，在神经成像中，fMRI和DTI都可以通过提供不同时空分辨率的不同技术来捕获大脑活动的成像数据[61]。如何利用深度学习模型将它们有效地融合在一起，更好地完成疾病分类和脑活动识别的任务，研究较少。一个城市的多式联运数据，包括出租车轨迹数据、共享单车出行数据、公共交通值机数据等，都能从不同角度反映城市人群流动性[30]。将它们融合在一起而不是分开分析，可以更全面地捕捉潜在的流动模式，并做出更准确的预测。虽然最近有人尝试将深度学习模型应用于不同城市人群流量数据中的知识传递[151]、[172]，但是如何将多模态ST数据集与深度学习模型相融合，仍然没有得到很好的研究，需要更多的研究关注。



###  <span id="p8">八、 REFERENCES</span>
[1] Y. Ai, Z. Li, M. Gan, Y. Zhang, D. Yu, W. Chen, and Y. Ju. A deep
learning approach on short-term spatiotemporal distribution forecasting
of dockless bike-sharing system. Neural Computing and Applications,
pages 1–13, 2018.

[2] A. Akbari Asanjan, T. Yang, K. Hsu, S. Sorooshian, J. Lin, and Q. Peng.
Short-term precipitation forecast based on the persiann system and
lstm recurrent neural networks. Journal of Geophysical Research:
Atmospheres, 123(22):12–543, 2018.

[3] A. Alahi, K. Goel, V. Ramanathan, A. Robicquet, L. Fei-Fei, and
S. Savarese. Social lstm: Human trajectory prediction in crowded
spaces. In Proceedings of the IEEE Conference on Computer Vision
and Pattern Recognition, pages 961–971, 2016.

[4] G. Atluri, A. Karpatne, and V. Kumar. Spatio-temporal data mining: A
survey of problems and methods. ACM Computing Surveys (CSUR),
51(4):83, 2018.

[5] D. Bahdanau, K. Cho, and Y. Bengio. Neural machine translation
by jointly learning to align and translate. In In Proceedings of
International Conference on Learning Representations 2015, 2015.

[6] J. Bao, P. Liu, and S. V. Ukkusuri. A spatiotemporal deep learning
approach for citywide short-term crash risk prediction with multisource data. Accident Analysis & Prevention, 122:239–254, 2019.

[7] Y. Bengio, P. Lamblin, D. Popovici, and H. Larochelle. Greedy layerwise training of deep networks. In In Proceedings of Advances in
Neural Information Processing Systems, 2006.

[8] D. Chai, L. Wang, and Q. Yang. Bike flow prediction with multigraph convolutional networks. In Proceedings of the 26th ACM
SIGSPATIAL International Conference on Advances in Geographic
Information Systems, pages 397–400. ACM, 2018.

[9] V. Chandola, R. R. Vatsavai, D. Kumar, and A. Ganguly. Analyzing
big spatial and big spatiotemporal data: a case study of methods and
applications. Big Data Analytics, 33(239), 2015.

[10] B. Chang, Y. Park, D. Park, S. Kim, and J. Kang. Content-aware
hierarchical point-of-interest embedding model for successive poi recommendation. In IJCAI, pages 3301–3307, 2018.

[11] A. Chattopadhyay, P. Hassanzadeh, and S. Pasha. A test case for
application of convolutional neural networks to spatio-temporal climate data: Re-identifying clustered weather patterns. arXiv preprint
arXiv:1811.04817, 2018.

[12] C. Chen, K. Li, S. G. Teo, G. Chen, X. Zou, X. Yang, R. C. Vijay,
J. Feng, and Z. Zeng. Exploiting spatio-temporal correlations with
multiple 3d convolutional neural networks for citywide vehicle flow
prediction. In 2018 IEEE International Conference on Data Mining
(ICDM), pages 893–898. IEEE, 2018.

[13] C. Chen, C. Liao, X. Xie, Y. Wang, and J. Zhao. Trip2vec: a deep
embedding approach for clustering and profiling taxi trip purposes.
Personal and Ubiquitous Computing, pages 1–14, 2018.

[14] M. Chen, J. M. Davis, C. Liu, Z. Sun, M. M. Zempila, and W. Gao.
Using deep recurrent neural network for direct beam solar irradiance
cloud screening. In Remote Sensing and Modeling of Ecosystems for
Sustainability XIV, volume 10405, page 1040503. International Society
for Optics and Photonics, 2017.

[15] M. Chen, X. Yu, and Y. Liu. Pcnn: Deep convolutional networks
for short-term traffic congestion prediction. IEEE Transactions on
Intelligent Transportation Systems, (99):1–10, 2018.

[16] Q. Chen, X. Song, H. Yamada, and R. Shibasaki. Learning deep
representation from big and heterogeneous data for traffic accident
inference. In AAAI, pages 338–344, 2016.

[17] L. Cheng, H. Zang, T. Ding, R. Sun, M. Wang, Z. Wei, and G. Sun.
Ensemble recurrent neural network based probabilistic wind speed
forecasting approach. Energies, 11(8):1958, 2018.

[18] T. Cheng, J. H. B. Anbaroglu, and G. Tanakaranond. Spatiotemporal
data mining. Handbookd of Regional Science, pages 1173–1193, 2014.

[19] W. Cheng, Y. Shen, Y. Zhu, and L. Huang. A neural attention model
for urban air quality inference: Learning the weights of monitoring
stations. In AAAI, 2018.

[20] K.-H. Chow, A. Hiranandani, Y. Zhang, and S.-H. G. Chan. Representation learning of pedestrian trajectories using actor-critic sequence-tosequence autoencoder. arXiv preprint arXiv:1811.08069, 2018.

[21] O. Costilla-Reyes, P. Scully, and K. B. Ozanyan. Deep neural networks
for learning spatio-temporal features from tomography sensors. IEEE
Transactions on Industrial Electronics, 65(1):645–653, 2018.

[22] Z. Cui, K. Henrickson, R. Ke, and Y. Wang. High-order graph
convolutional recurrent neural network: A deep learning framework
for network-scale traffic learning and forecasting. arXiv preprint
arXiv:1802.07007, 2018.

[23] Z. Cui, R. Ke, and Y. Wang. Deep stacked bidirectional and
unidirectional lstm recurrent neural network for network-wide traffic
speed prediction. In 6th International Workshop on Urban Computing
(UrbComp 2017), 2016.

[24] S. Dabiri and K. Heaslip. Inferring transportation modes from gps
trajectories using a convolutional neural network. Transportation
research part C: emerging technologies, 86:360–371, 2018.

[25] Z. Diao, X. Wang, D. Zhang, Y. Liu, K. Xie, and S. He. Dynamic
spatial-temporal graph convolutional neural networks for traffic forecasting. In In Proceedings of 33rd AAAI Conference on Artificial
Intelligence, 2019.

[26] D. Ding, M. Zhang, X. Pan, D. Wu, and P. Pu. Geographical
feature extraction for entities in location-based social networks. In
Proceedings of the 2018 World Wide Web Conference on World Wide
Web, pages 833–842. International World Wide Web Conferences
Steering Committee, 2018.

[27] M. F. Dixon, N. G. Polson, and V. O. Sokolov. Deep learning for
spatio-temporal modeling: Dynamic traffic flows and high frequency
trading. Applied Stochastic Models in Business and Industry, 2017.

[28] B. Du, H. Peng, S. Wang, M. Bhuiyan, L. Wang, Q. Gong, L. Liu,
and J. Li. Deep irregular convolutional residual lstm for urban
traffic passenger flows prediction. IEEE Transactions on Intelligent
Transportation Systems, 99:1–14, 2019.

[29] M. Du, N. Liu, and X. Hu. Techniques for interpretable machine
learning. arXiv:1808.00033 [cs.LG], 2018.

[30] S. Du, T. Li, X. Gong, Z. Yu, and S.-J. Horng. A hybrid method for
traffic flow forecasting using multimodal deep learning. arXiv preprint
arXiv:1803.02099, 2018.

[31] L. Duan, T. Hu, E. Cheng, J. Zhu, and C. Gao. Deep convolutional
neural networks for spatiotemporal crime prediction. In 2017 International Conference on Information and Knowledge Engineering (IKE),
pages 61–67, 2017.

[32] Y. Duan, Y. Lv, W. Kang, and Y. Zhao. A deep learning based
approach for traffic data imputation. In Intelligent Transportation
Systems (ITSC), 2014 IEEE 17th International Conference on, pages
912–917. IEEE, 2014.

[33] Y. Duan, Y. Lv, Y.-L. Liu, and F.-Y. Wang. An efficient realization of
deep learning for traffic data imputation. Transportation research part
C: emerging technologies, 72:168–181, 2016.

[34] N. C. Dvornek, P. Ventola, K. A. Pelphrey, and J. S. Duncan. Identifying autism from resting-state fmri using long short-term memory
networks. In International Workshop on Machine Learning in Medical
Imaging, pages 362–370. Springer, 2017.

[35] Y. Endo, K. Nishida, H. Toda, and H. Sawada. Predicting destinations
from partial trajectories using recurrent neural network. In Pacific-Asia
Conference on Knowledge Discovery and Data Mining, pages 160–172.
Springer, 2017.

[36] Y. Endo, H. Toda, K. Nishida, and J. Ikedo. Classifying spatial
trajectories using representation learning. International Journal of Data
Science and Analytics, 2(3-4):107–117, 2016.

[37] Z. Fan, X. Song, T. Xia, R. Jiang, R. Shibasaki, and R. Sakuramachi.
Online deep ensemble learning for predicting citywide human mobility. Proceedings of the ACM on Interactive, Mobile, Wearable and
Ubiquitous Technologies, 2(3):105, 2018.

[38] J. Feng, Y. Li, C. Zhang, F. Sun, F. Meng, A. Guo, and D. Jin. Deepmove: Predicting human mobility with attentional recurrent networks.
In Proceedings of the 2018 World Wide Web Conference on World Wide
Web, pages 1459–1468. International World Wide Web Conferences
Steering Committee, 2018.

[39] T. Fernando, S. Denman, S. Sridharan, and C. Fookes. Soft+ hardwired
attention: An lstm framework for human trajectory prediction and
abnormal event detection. Neural networks, 108:466–478, 2018.

[40] T. G. C. N. for Traffic Speed Prediction Considering External Factors.
Liang ge and hang li and junling liu and aoli zhou. In In Proceedings
of The 20th International Conference on Mobile Data Management,
2019.

[41] Q. Gao, G. Trajcevski, F. Zhou, K. Zhang, T. Zhong, and F. Zhang.
Trajectory-based social circle inference. In Proceedings of the 26th
ACM SIGSPATIAL International Conference on Advances in Geographic Information Systems, pages 369–378. ACM, 2018.

[42] Q. Gao, F. Zhou, K. Zhang, G. Trajcevski, X. Luo, and F. Zhang.
Identifying human mobility via trajectory embeddings. In Proceedings
of the 26th International Joint Conference on Artificial Intelligence,
pages 1689–1695. AAAI Press, 2017.

[43] Y. Gao, L. Zhao, L. Wu, Y. Ye, H. Xiong, and C. Yang. Incomplete label
multi-task deep learning for spatio-temporal event subtype forecasting.
2019.

[44] X. Geng, Y. Li, L. Wang, L. Zhang, Q. Yang, J. Ye, and Y. Liu. Spatiotemporal multi-graph convolution network for ride-hailing demand
forecasting. 2019.

[45] X. Geng, Y. Li, L. Wang, L. Zhang, J. Ye, Y. Liu, and Q. Yang. Spatiotemporal multi-graph convolution network for ride-hailing demand
forecasting. In In Proceedings of 33rd AAAI Conference on Artificial
Intelligence, 2019.

[46] S. Giffard-Roisin, M. Yang, G. Charpiat, B. Kegl, and C. Monteleoni. ´
Fused deep learning for hurricane forecast fused deep learning for
hurricane track forecast from reanalysis data. In Climate Informatics
Workshop Proceedings 2018, 2018.

[47] S. Guo, Y. Lin, N. Feng, C. Song, and H. Wan. Attention based spatialtemporal graph convolutional networks for traffic flow forecasting. In In
Proceedings of 33rd AAAI Conference on Artificial Intelligence, 2019.

[48] X. Guo, K. C. Dominick, A. A. Minai, H. Li, C. A. Erickson, and
L. J. Lu. Diagnosing autism spectrum disorder from brain restingstate functional connectivity patterns using a deep neural network with
a novel feature selection method. Frontiers in neuroscience, 11:460,
2017.

[49] A. Gupta, J. Johnson, L. Fei-Fei, S. Savarese, and A. Alahi. Social gan:
Socially acceptable trajectories with generative adversarial networks.
In IEEE Conference on Computer Vision and Pattern Recognition
(CVPR), number CONF, 2018.

[50] S. He and K. G. Shin. Spatio-temporal capsule-based reinforcement
learning for mobility-on-demand network coordination. In In proceeding of the 27th International conference on World Wide Web, 2019.

[51] Z. He, C.-Y. Chow, and J.-D. Zhang. Stcnn: A spatio-temporal
convolutional neural network for long-term traffic prediction. In In
Proceedings of The 20th International Conference on Mobile Data
Management, 2019.

[52] A. S. Heinsfeld, A. R. Franco, R. C. Craddock, A. Buchweitz, and
F. Meneguzzi. Identification of autism spectrum disorder using deep
learning and the abide dataset. NeuroImage: Clinical, 17:16–23, 2018.

[53] G. E. Hinton and R. R. Salakhutdinov. Reducing the dimensionality
of data with neural networks. Science, 313(5786):504–507, 2013.

[54] T. Horikawa and Y. Kamitani. Generic decoding of seen and imagined
objects using hierarchical visual features. Nature communications,
8:15037, 2017.

[55] M. Hossain, B. Rekabdar, S. J. Louis, and S. Dascalu. Forecasting
the weather of nevada: A deep learning approach. In Neural Networks
(IJCNN), 2015 International Joint Conference on, pages 1–6. IEEE,
2015.

[56] C. Huang, C. Zhang, J. Zhao, X. Wu, N. Chawla, and D. Yin. Mist:
A multiview and multimodal spatial-temporal learning framework for
citywide abnormal event forecasting. In In proceeding of the 27th
International conference on World Wide Web, 2019.

[57] C. Huang, J. Zhang, Y. Zheng, and N. V. Chawla. Deepcrime: Attentive
hierarchical recurrent networks for crime prediction. In Proceedings of
the 27th ACM International Conference on Information and Knowledge
Management, pages 1423–1432. ACM, 2018.

[58] H. Huang, X. Hu, J. Han, J. Lv, N. Liu, L. Guo, and T. Liu. Latent
source mining in fmri data via deep neural network. In Biomedical
Imaging (ISBI), 2016 IEEE 13th International Symposium on, pages
638–641. IEEE, 2016.

[59] H. Huang, X. Hu, Y. Zhao, M. Makkie, Q. Dong, S. Zhao, L. Guo, and
T. Liu. Modeling task fmri data via deep convolutional autoencoder.
IEEE transactions on medical imaging, 37(7):1551–1561, 2018.

[60] W. Huang, G. Song, H. Hong, and K. Xie. Deep architecture for traffic
flow prediction: Deep belief networks with multitask learning. IEEE
Trans. Intelligent Transportation Systems, 15(5):2191–2201, 2014.

[61] R. J. Huster, S. Debener, T. Eichele, and C. S. Herrmann. Methods
for simultaneous eeg-fmri: An introductory review. Journal of Neuroscience, 32(18):6053–6060, May 2012.

[62] A. Jain, A. R. Zamir, S. Savarese, and A. Saxena. Structural-rnn:
Deep learning on spatio-temporal graphs. In Proceedings of the IEEE
Conference on Computer Vision and Pattern Recognition, pages 5308–
5317, 2016.

[63] H. Jang, S. M. Plis, V. D. Calhoun, and J.-H. Lee. Task-specific
feature extraction and classification of fmri volumes using a deep
neural network initialized with a deep belief network: Evaluation using
sensorimotor tasks. NeuroImage, 145:314–328, 2017.

[64] R. Jiang, X. Song, Z. Fan, T. Xia, Q. Chen, Q. Chen, and R. Shibasaki.
Deep roi-based modeling for urban human mobility prediction. Proceedings of the ACM on Interactive, Mobile, Wearable and Ubiquitous
Technologies, 2(1):14, 2018.

[65] X. Jiang, E. N. de Souza, A. Pesaranghader, B. Hu, D. L. Silver, and
S. Matwin. Trajectorynet: An embedded gps trajectory representation
for point-based classification using recurrent neural networks. arXiv
preprint arXiv:1705.02636, 2017.

[66] M. Jin, T. Curran, and D. Cordes. Classification of amnestic mild
cognitive impairment using fmri. In Biomedical Imaging (ISBI), 2014
IEEE 11th International Symposium on, pages 29–32. IEEE, 2014.

[67] A. Karatzoglou, N. Schnell, and M. Beigl. A convolutional neural
network approach for modeling semantic trajectories and predicting
future locations. In International Conference on Artificial Neural
Networks, pages 61–72. Springer, 2018.

[68] J. Kawahara, C. J. Brown, S. P. Miller, B. G. Booth, V. Chau, R. E.
Grunau, J. G. Zwicker, and G. Hamarneh. Brainnetcnn: convolutional
neural networks for brain networks; towards predicting neurodevelopment. NeuroImage, 146:1038–1049, 2017.

[69] J. Ke, H. Yang, H. Zheng, X. Chen, Y. Jia, P. Gong, and J. Ye. Hexagonbased convolutional neural network for supply-demand forecasting of
ride-sourcing services. IEEE Transactions on Intelligent Transportation
Systems, 2018.

[70] J. Ke, H. Zheng, H. Yang, and X. M. Chen. Short-term forecasting of
passenger demand under on-demand ride services: A spatio-temporal
deep learning approach. Transportation Research Part C: Emerging
Technologies, 85:591–608, 2017.

[71] J. Kim, V. D. Calhoun, E. Shim, and J.-H. Lee. Deep neural network
with weight sparsity control and pre-training extracts hierarchical features and enhances classification performance: Evidence from wholebrain resting-state functional connectivity patterns of schizophrenia.
Neuroimage, 124:127–146, 2016.

[72] S. Kim, S. Ames, J. Lee, C. Zhang, A. C. Wilson, and D. Williams.
Resolution reconstruction of climate data with pixel recursive model.
In Data Mining Workshops (ICDMW), 2017 IEEE International Conference on, pages 313–321. IEEE, 2017.

[73] S. Kim, S. Hong, M. Joh, and S.-k. Song. Deeprain: Convlstm
network for precipitation prediction using multichannel radar data.
arXiv preprint arXiv:1711.02316, 2017.

[74] Z. Kira, W. Li, R. Allen, and A. R. Wagner. Leveraging deep learning
for spatio-temporal understanding of everyday environments. In IJCAI
Workshop on Deep Learning and Artificial Intelligence, 2016.

[75] S. Kisilevich, F. Mansmann, M. Nanni, and S. Rinzivillo. Spatiotemporal clustering: a survey. Data Mining and Knowledge Discovery
Handbook, Springer, 2015.

[76] J. Kleesiek, G. Urban, A. Hubert, D. Schwarz, K. Maier-Hein,
M. Bendszus, and A. Biller. Deep mri brain extraction: a 3d convolutional neural network for skull stripping. NeuroImage, 129:460–469,
2016.

[77] D. Kong and F. Wu. Hst-lstm: A hierarchical spatial-temporal longshort term memory network for location prediction. In IJCAI, pages
2341–2347, 2018.

[78] S. Korolev, A. Safiullin, M. Belyaev, and Y. Dodonova. Residual and
plain convolutional neural networks for 3d brain mri classification.
In Biomedical Imaging (ISBI 2017), 2017 IEEE 14th International
Symposium on, pages 835–838. IEEE, 2017.

[79] T. Kurth, S. Treichler, J. Romero, M. Mudigonda, N. Luehr, E. Phillips,
A. Mahesh, M. Matheson, J. Deslippe, M. Fatica, et al. Exascale deep
learning for climate analytics. In Proceedings of the International
Conference for High Performance Computing, Networking, Storage,
and Analysis, page 51. IEEE Press, 2018.

[80] D. Lee, S. Jung, Y. Cheon, D. Kim, and S. You. Forecasting
taxi demands with fully convolutional networks and temporal guided
embedding. 2018.

[81] R. Li, Y. Shen, and Y. Zhu. Next point-of-interest recommendation with
temporal and multi-level context attention. In 2018 IEEE International
Conference on Data Mining (ICDM), pages 1110–1115. IEEE, 2018.

[82] X. Li, K. Zhao, G. Cong, C. S. Jensen, and W. Wei. Deep representation
learning for trajectory similarity computation. In 2018 IEEE 34th
International Conference on Data Engineering (ICDE), pages 617–
628. IEEE, 2018.

[83] Y. Li, K. Fu, Z. Wang, C. Shahabi, J. Ye, and Y. Liu. Multi-task
representation learning for travel time estimation. In International
Conference on Knowledge Discovery and Data Mining,(KDD), 2018.

[84] Y. Li and B. Shuai. Origin and destination forecasting on dockless
shared bicycle in a hybrid deep-learning algorithms. Multimedia Tools
and Applications, pages 1–12, 2018.

[85] Y. Li, R. Yu, C. Shahabi, and Y. Liu. Diffusion convolutional recurrent
neural network: Data-driven traffic forecasting. 2018.

[86] Y. Li, Z. Zhu, D. Kong, M. Xu, and Y. Zhao. Learning heterogeneous
spatial-temporal representation for bike-sharing demand prediction. In
In Proceedings of 33rd AAAI Conference on Artificial Intelligence,
2019.

[87] Z. Li. Spatiotemporal pattern mining: Algorithms and applications.
Frequent Pattern Mining, 2014.

[88] Y. Liang, S. Ke, J. Zhang, X. Yi, and Y. Zheng. Geoman: Multi-level
attention networks for geo-sensory time series prediction. In IJCAI,
pages 3428–3434, 2018.

[89] B. Liao, J. Zhang, M. Cai, S. Tang, Y. Gao, C. Wu, S. Yang, W. Zhu,
Y. Guo, and F. Wu. Dest-resnet: A deep spatiotemporal residual
network for hotspot traffic speed prediction. In 2018 ACM Multimedia
Conference on Multimedia Conference, pages 1883–1891. ACM, 2018.

[90] B. Liao, J. Zhang, C. Wu, D. McIlwraith, T. Chen, S. Yang, Y. Guo,
and F. Wu. Deep sequence learning with auxiliary information for
traffic prediction. arXiv preprint arXiv:1806.07380, 2018.

[91] D. Liao, W. Liu, Y. Zhong, J. Li, and G. Wang. Predicting activity
and location with multi-task context aware recurrent neural network.
In Proceedings of the Twenty-Seventh International Joint Conference
on Artificial Intelligence (IJCAI-18), pages 3435–3441, 2018.

[92] L. Lin, Z. He, and S. Peeta. Predicting station-level hourly demand in a
large-scale bike-sharing network: A graph convolutional neural network
approach. Transportation Research Part C: Emerging Technologies,
97:258–276, 2018.

[93] Y. Lin, X. Dai, L. Li, and F.-Y. Wang. Pattern sensitive prediction
of traffic flow based on generative adversarial framework. IEEE
Transactions on Intelligent Transportation Systems, (99):1–6, 2018.

[94] Y. Lin, N. Mago, Y. Gao, Y. Li, Y.-Y. Chiang, C. Shahabi, and J. L.
Ambite. Exploiting spatiotemporal patterns for accurate air quality
forecasting using deep learning. In Proceedings of the 26th ACM
SIGSPATIAL International Conference on Advances in Geographic
Information Systems, pages 359–368. ACM, 2018.

[95] Z. Lin, J. Feng, Z. Lu, Y. Li, and D. Jin. Deepstn+: Contextaware spatial
temporal neural network for crowd flow prediction in metropolis. In In
Proceedings of 33rd AAAI Conference on Artificial Intelligence, 2019.

[96] H. Liu, X. Mi, and Y. Li. Smart deep learning based wind speed
prediction model using wavelet packet decomposition, convolutional
neural network and convolutional long short term memory network.
Energy Conversion and Management, 166:120–131, 2018.

[97] H. Liu, X. Mi, and Y. Li. Smart multi-step deep learning model
for wind speed forecasting based on variational mode decomposition,
singular spectrum analysis, lstm network and elm. Energy Conversion
and Management, 159:54–64, 2018.

[98] L. Liu, R. Zhang, J. Peng, G. Li, B. Du, and L. Lin. Attentive crowd
flow machines. arXiv preprint arXiv:1809.00101, 2018.

[99] Q. Liu, S. Wu, L. Wang, and T. Tan. Predicting the next location: A
recurrent model with spatial and temporal contexts. In AAAI, pages
194–200, 2016.

[100] Y. Liu, E. Racah, J. Correa, A. Khosrowshahi, D. Lavers, K. Kunkel,
M. Wehner, W. Collins, et al. Application of deep convolutional neural
networks for detecting extreme weather in climate datasets. arXiv
preprint arXiv:1605.01156, 2016.

[101] Y. Liu, Y. Wang, X. Yang, and L. Zhang. Short-term travel time
prediction by deep learning: A comparison of different lstm-dnn
models. In Intelligent Transportation Systems (ITSC), 2017 IEEE 20th
International Conference on, pages 1–8. IEEE, 2017.

[102] Z. Liu, Z. Li, K. Wu, and M. Li. Urban traffic prediction from mobility
data using deep learning. IEEE Network, 32(4):40–46, 2018.

[103] J. Lv, Q. Li, Q. Sun, and X. Wang. T-conv: A convolutional neural
network for multi-scale taxi trajectory prediction. In Big Data and
Smart Computing (BigComp), 2018 IEEE International Conference on,
pages 82–89. IEEE, 2018.

[104] Y. Lv, Y. Duan, W. Kang, Z. Li, F.-Y. Wang, et al. Traffic flow
prediction with big data: A deep learning approach. IEEE Trans.
Intelligent Transportation Systems, 16(2):865–873, 2015.

[105] Z. Lv, J. Xu, K. Zheng, H. Yin, P. Zhao, and X. Zhou. Lc-rnn: A deep
learning model for traffic speed prediction. In IJCAI, pages 3470–3476,
2018.

[106] X. Ma, Z. Dai, Z. He, J. Ma, Y. Wang, and Y. Wang. Learning
traffic as images: a deep convolutional neural network for large-scale
transportation network speed prediction. Sensors, 17(4):818, 2017.

[107] X. Ma, Y. Li, Z. Cui, and Y. Wang. Forecasting transportation network
speed using deep capsule networks with nested lstm models. arXiv
preprint arXiv:1811.04745, 2018.

[108] X. Ma, H. Yu, Y. Wang, and Y. Wang. Large-scale transportation
network congestion evolution prediction using deep learning theory.
PloS one, 10(3):e0119044, 2015.

[109] X. Ma, J. Zhang, B. Du, C. Ding, and L. Sun. Parallel architecture
of convolutional bi-directional lstm neural networks for networkwide metro ridership prediction. IEEE Transactions on Intelligent
Transportation Systems, 2018.

[110] A. H. Marblestone, G. Wayne, and K. P. Kording. Toward an integration of deep learning and neuroscience. Frontiers in computational
neuroscience, 10:94, 2016.

[111] H. Martin, D. Bucher, E. Suel, P. Zhao, F. Perez-Cruz, and M. Raubal.
Graph convolutional neural networks for human activity purpose imputation from gps-based trajectory data. 2018.

[112] J. D. Mazimpaka and S. Timpf. Trajectory data mining: A review
of methods and applications. Journal of Spatial Information Science,
(13):61–99, 2016.

[113] R. J. Meszlenyi, K. Buza, and Z. Vidny ´ anszky. Resting state fmri ´
functional connectivity-based classification using a convolutional neural network architecture. Frontiers in neuroinformatics, 11:61, 2017.

[114] H. Nguyen, L.-M. Kieu, T. Wen, and C. Cai. Deep learning methods
in transportation domain: a review. IET Intelligent Transport Systems,
12(9):998–1004, 2018.

[115] N. T. Nguyen, Y. Wang, H. Li, X. Liu, and Z. Han. Extracting typical
users’ moving patterns using deep learning. In Global Communications
Conference (GLOBECOM), 2012 IEEE, pages 5410–5414. IEEE, 2012.

[116] D. Nie, H. Zhang, E. Adeli, L. Liu, and D. Shen. 3d deep learning for
multi-modal imaging-guided survival time prediction of brain tumor
patients. In International Conference on Medical Image Computing
and Computer-Assisted Intervention, pages 212–220. Springer, 2016.

[117] X. Niu, Y. Zhu, and X. Zhang. Deepsense: A novel learning mechanism
for traffic prediction with taxi gps traces. In Global Communications
Conference (GLOBECOM), 2014 IEEE, pages 2745–2750. IEEE, 2014.

[118] X. Ouyang, C. Zhang, P. Zhou, and H. Jiang. Deepspace: An online
deep learning framework for mobile big data to understand human
mobility patterns. arXiv preprint arXiv:1610.07009, 2016.

[119] P. Patel, P. Aggarwal, and A. Gupta. Classification of schizophrenia
versus normal subjects using deep learning. In In Proceedings of the
Tenth Indian Conference on Computer Vision, Graphics and Image
Processing, 2016.

[120] S. M. Plis, D. R. Hjelm, R. Salakhutdinov, E. A. Allen, H. J. Bockholt,
J. D. Long, H. J. Johnson, J. S. Paulsen, J. A. Turner, and V. D.
Calhoun. Deep learning for neuroimaging: a validation study. Frontiers
in neuroscience, 8:229, 2014.

[121] N. G. Polson and V. O. Sokolov. Deep learning for short-term
traffic flow prediction. Transportation Research Part C: Emerging
Technologies, 79:1–17, 2017.

[122] Z. Qiu, T. Yao, and T. Mei. Learning spatio-temporal representation
with pseudo-3d residual networks. In 2017 IEEE International Conference on Computer Vision (ICCV), pages 5534–5542. IEEE, 2017.

[123] E. Racah, C. Beckham, T. Maharaj, S. E. Kahou, M. Prabhat,
and C. Pal. Extremeweather: A large-scale climate dataset for
semi-supervised detection, localization, and understanding of extreme
weather events. In Advances in Neural Information Processing Systems,
pages 3402–3413, 2017.

[124] S. Rasp and S. Lerch. Neural networks for post-processing ensemble
weather forecasts. arXiv preprint arXiv:1805.09091, 2018.

[125] H. Ren, Y. Song, J. Wang, Y. Hu, and J. Lei. A deep learning
approach to the citywide traffic accident risk prediction. In 2018 21st
International Conference on Intelligent Transportation Systems (ITSC),
pages 3346–3351. IEEE, 2018.

[126] F. Rodrigues, I. Markou, and F. C. Pereira. Combining time-series and
textual data for taxi demand prediction in event areas: a deep learning
approach. Information Fusion, 49:120–129, 2019.

[127] I. Roesch and T. Gunther. Visualization of neural network predictions ¨
for weather forecasting. In Computer Graphics Forum. Wiley Online
Library, 2017.

[128] S. Sarraf and G. Tofighi. Deep learning-based pipeline to recognize
alzheimer’s disease using fmri data. In Future Technologies Conference
(FTC), pages 816–820. IEEE, 2016.

[129] S. Scher. Toward data-driven weather and climate forecasting: Approximating a simple general circulation model with deep learning.
Geophysical Research Letters, 45(22):12–616, 2018.

[130] S. Shekhar, Z. Jiang, R. Y. Ali, E. Eftelioglu, X. Tang, V. M. V.
Gunturi, and X. Zhou. Spatiotemporal data mining: A computational
perspective. ISPRS International Journal of Geo-Information, 4:2306–
2338, 2015.

[131] B. Shen, X. Liang, Y. Ouyang, M. Liu, W. Zheng, and K. M.
Carley. Stepdeep: A novel spatial-temporal mobility event prediction
framework based on deep neural network. In Proceedings of the 24th
ACM SIGKDD International Conference on Knowledge Discovery &
Data Mining, pages 724–733. ACM, 2018.

[132] J. Shi, X. Zheng, Y. Li, Q. Zhang, and S. Ying. Multimodal neuroimaging feature learning with multimodal stacked deep polynomial networks
for diagnosis of alzheimer’s disease. IEEE journal of biomedical and
health informatics, 22(1):173–183, 2018.

[133] X. Shi, Z. Gao, L. Lausen, H. Wang, D.-Y. Yeung, W.-k. Wong, and W.-
c. Woo. Deep learning for precipitation nowcasting: A benchmark and
a new model. In Advances in Neural Information Processing Systems,
pages 5617–5627, 2017.

[134] Y. Shi, Y. Tian, Y. Wang, and T. Huang. Sequential deep trajectory
descriptor for action recognition with three-stream cnn. IEEE Transactions on Multimedia, 19(7):1510–1520, 2017.

[135] X. Song, H. Kanasugi, and R. Shibasaki. Deeptransport: Prediction and
simulation of human mobility and transportation mode at a citywide
level. In IJCAI, volume 16, pages 2618–2624, 2016.

[136] R. Soua, A. Koesdwiady, and F. Karray. Big-data-generated traffic
flow prediction using deep learning and dempster-shafer theory. In
Neural Networks (IJCNN), 2016 International Joint Conference on,
pages 3195–3202. IEEE, 2016.

[137] F. Sun, A. Dubey, and J. White. Dxnat-deep neural networks for
explaining non-recurring traffic congestion. In In Proceedings of IEEE
International Conference on Big Data, 2017.

[138] I. Sutskever, O. Vinyals, and Q. V. Le. Sequence to sequence learning
with neural networks. In Proceedings of the 27th International
Conference on Neural Information Processing Systems, 2014.

[139] Y. Tao, X. Gao, A. Ihler, K. Hsu, and S. Sorooshian. Deep neural networks for precipitation estimation from remotely sensed information.
In Evolutionary Computation (CEC), 2016 IEEE Congress on, pages
1349–1355. IEEE, 2016.

[140] G. W. Taylor, R. Fergus, Y. LeCun, and C. Bregler. Convolutional
learning of spatio-temporal features. In European conference on
computer vision, pages 140–153. Springer, 2010.

[141] D. Tran, L. Bourdev, R. Fergus, L. Torresani, and M. Paluri. Learning
spatiotemporal features with 3d convolutional networks. In Proceedings
of the IEEE international conference on computer vision, pages 4489–
4497, 2015.

[142] D. Varshneya and G. Srinivasaraghavan. Human trajectory prediction using spatially aware deep attention models. arXiv preprint
arXiv:1705.09436, 2017.

[143] R. R. Vatsavai, V. Chandola, S. Klasky, A. Ganguly, A. Stefanidis, and
S. Shekhar. Spatiotemporal data mining in the era of big spatial data:
algorithms and applications. In In SIGSPATIAL international workshop
on analytics for big geospatial data, 2012.

[144] B. Wang, X. Luo, F. Zhang, B. Yuan, A. L. Bertozzi, and P. J.
Brantingham. Graph-based deep modeling and real time forecasting of
sparse spatio-temporal data. arXiv preprint arXiv:1804.00684, 2018.

[145] B. Wang, D. Zhang, D. Zhang, P. J. Brantingham, and A. L.
Bertozzi. Deep learning for real time crime forecasting. arXiv preprint
arXiv:1707.03340, 2017.

[146] D. Wang, W. Cao, J. Li, and J. Ye. Deepsd: supply-demand prediction
for online car-hailing services using deep neural networks. In 2017
IEEE 33rd International Conference on Data Engineering (ICDE),
pages 243–254. IEEE, 2017.

[147] D. Wang, J. Zhang, W. Cao, J. Li, and Y. Zheng. When will you arrive?
estimating travel time based on deep neural networks. AAAI, 2018.

[148] H. Wang, G. Liu, J. Duan, and L. Zhang. Detecting transportation
modes using deep neural network. IEICE TRANSACTIONS on Information and Systems, 100(5):1132–1135, 2017.

[149] J. Wang, Q. Gu, J. Wu, G. Liu, and Z. Xiong. Traffic speed prediction
and congestion source exploration: A deep learning method. In Data
Mining (ICDM), 2016 IEEE 16th International Conference on, pages
499–508. IEEE, 2016.

[150] L. Wang, X. Geng, J. Ke, C. Peng, X. Ma, D. Zhang, and Q. Yang.
Ridesourcing car detection by transfer learning. arXiv preprint
arXiv:1705.08409, 2017.

[151] L. Wang, X. Geng, X. Ma, F. Liu, and Q. Yang. Crowd flow prediction
by deep spatio-temporal transfer learning. CoRR, abs/1802.00386,
2018.

[152] L. Wang, Y. Qiao, and X. Tang. Action recognition with trajectorypooled deep-convolutional descriptors. In Proceedings of the IEEE
conference on computer vision and pattern recognition, pages 4305–
4314, 2015.

[153] P. Wang, Y. Fu, J. Zhang, X. Li, and D. Lin. Learning urban community
structures: A collective embedding perspective with periodic spatialtemporal mobility graphs. ACM Transactions on Intelligent Systems
and Technology (TIST), 9(6):63, 2018.

[154] P. Wang, Z. Li, Y. Hou, and W. Li. Action recognition based on joint
trajectory maps using convolutional neural networks. In Proceedings
of the 2016 ACM on Multimedia Conference, pages 102–106. ACM,
2016.

[155] X. Wang, C. Chen, Y. Min, J. He, B. Yang, and Y. Zhang. Efficient
metropolitan traffic prediction based on graph recurrent neural network.
arXiv preprint arXiv:1811.00740, 2018.

[156] Y. Wang, M. Long, J. Wang, Z. Gao, and S. Y. Philip. Predrnn:
Recurrent neural networks for predictive learning using spatiotemporal
lstms. In Advances in Neural Information Processing Systems, pages
879–888, 2017.

[157] Y. Wang, D. Zhang, Y. Liu, B. Dai, and L. H. Lee. Enhancing
transportation systems via deep learning: A survey. Transportation
Research Part C: Emerging Technologies, 2018.

[158] D. Wen, Z. Wei, Y. Zhou, G. Li, X. Zhang, and W. Han. Deep learning
methods to process fmri data and their application in the diagnosis of
cognitive impairment: A brief overview and our opinion. Frontiers in
neuroinformatics, 12:23, 2018.

[159] H. Wu, Z. Chen, W. Sun, B. Zheng, and W. Wang. Modeling trajectories
with recurrent neural networks. IJCAI, 2017.

[160] Z. Wu, S. Pan, F. Chen, G. Long, and P. S. Y. Chengqi Zhang. A
comprehensive survey on graph neural networks. arXiv: 1901.00596v2,
2019.

[161] S. Xingjian, Z. Chen, H. Wang, D.-Y. Yeung, W.-K. Wong, and W.-c.
Woo. Convolutional lstm network: A machine learning approach for
precipitation nowcasting. In Advances in neural information processing
systems, pages 802–810, 2015.

[162] C. Xu, J. Ji, and P. Liu. The station-free sharing bike demand
forecasting with a deep learning approach and large-scale datasets.
Transportation Research Part C: Emerging Technologies, 95:47–60,
2018.

[163] K. Xu, Z. Qin, G. Wang, K. Huang, S. Ye, and H. Zhang. Collisionfree lstm for human trajectory prediction. In International Conference
on Multimedia Modeling, pages 106–116. Springer, 2018.

[164] C. Yang, M. Sun, W. X. Zhao, Z. Liu, and E. Y. Chang. A neural
network approach to jointly modeling social networks and mobile tra-
jectories. ACM Transactions on Information Systems (TOIS), 35(4):36,
2017.

[165] G. Yang, Y. Cai, and C. K. Reddy. Recurrent spatio-temporal point
process for check-in time prediction. In Proceedings of the 27th ACM
International Conference on Information and Knowledge Management,
pages 2203–2211. ACM, 2018.

[166] G. Yang, Y. Cai, and C. K. Reddy. Spatio-temporal check-in time
prediction with recurrent neural network based survival analysis. In
Proceedings of the International Joint Conference on Artificial Intelligence (IJCAI), 2018.

[167] H.-F. Yang, T. S. Dillon, and Y.-P. P. Chen. Optimized structure of the
traffic flow forecasting model with a deep learning approach. IEEE
transactions on neural networks and learning systems, 28(10):2371–
2381, 2017.

[168] J. Yang and C. Eickhoff. Unsupervised learning of parsimonious
general-purpose embeddings for user and location modeling. ACM
Transactions on Information Systems (TOIS), 36(3):32, 2018.

[169] D. Yao, C. Zhang, J. Huang, and J. Bi. Serm: A recurrent model for next
location prediction in semantic trajectories. In Proceedings of the 2017
ACM on Conference on Information and Knowledge Management,
pages 2411–2414. ACM, 2017.

[170] D. Yao, C. Zhang, Z. Zhu, Q. Hu, Z. Wang, J. Huang, and J. Bi.
Learning deep representation for trajectory clustering. Expert Systems,
35(2):e12252, 2018.

[171] D. Yao, C. Zhang, Z. Zhu, J. Huang, and J. Bi. Trajectory clustering
via deep representation learning. In Neural Networks (IJCNN), 2017
International Joint Conference on, pages 3880–3887. IEEE, 2017.

[172] H. Yao, Y. Liu, Y. Wei, X. Tang, and Z. Li. Learning from multiple
cities: A meta-learning approach for spatial-temporal prediction. In In
proceeding of the 27th International conference on World Wide Web,
2019.

[173] H. Yao, X. Tang, H. Wei, G. Zheng, and Z. Li. Revisiting spatialtemporal similarity: A deep learning framework for traffic prediction.
In In Proceedings of 33rd AAAI Conference on Artificial Intelligence,
2019.

[174] H. Yao, F. Wu, J. Ke, X. Tang, Y. Jia, S. Lu, P. Gong, and J. Ye. Deep
multi-view spatial-temporal network for taxi demand prediction. arXiv
preprint arXiv:1802.08714, 2018.

[175] B. Yu, H. Yin, and Z. Zhu. Spatio-temporal graph convolutional
networks: A deep learning framework for traffic forecasting.

[176] H. Yu, Z. Wu, S. Wang, Y. Wang, and X. Ma. Spatiotemporal recurrent
convolutional networks for traffic prediction in transportation networks.
Sensors, 17(7):1501, 2017.

[177] R. Yu, Y. Li, C. Shahabi, U. Demiryurek, and Y. Liu. Deep learning:
A generic approach for extreme condition traffic forecasting. In
Proceedings of the 2017 SIAM International Conference on Data
Mining, pages 777–785. SIAM, 2017.

[178] R. Yuecheng, X. Zhimian, Y. Ruibo, and M. Xu. Du-parking: Spatiotemporal big data tells you realtime parking availability. In Proceedings
of the 24th ACM SIGKDD International Conference on Knowledge
Discovery and Data Mining, pages 646–654. ACM, 2018.

[179] M. A. Zaytar and C. El Amrani. Sequence to sequence weather
forecasting with long short term memory recurrent neural networks.
Int J Comput Appl, 143(11), 2016.

[180] G. Zhan. A semantic sequential correlation based lstm model for next
poi recommendation. In In Proceedings of The 20th International
Conference on Mobile Data Management, 2019.

[181] H. Zhang, H. Wu, W. Sun, and B. Zheng. Deeptravel: a neural network
based travel time estimation model with auxiliary supervision. arXiv
preprint arXiv:1802.02147, 2018.

[182] J. Zhang, B. Cao, S. Xie, C.-T. Lu, P. S. Yu, and A. B. Ragin.
Identifying connectivity patterns for brain diseases via multi-sideview guided deep architectures. In Proceedings of the 2016 SIAM
International Conference on Data Mining, pages 36–44. SIAM, 2016.

[183] J. Zhang, Y. Zheng, and D. Qi. Deep spatio-temporal residual networks
for citywide crowd flows prediction. In AAAI, pages 1655–1661, 2017.

[184] J. Zhang, Y. Zheng, D. Qi, R. Li, and X. Yi. Dnn-based prediction
model for spatio-temporal data. In Proceedings of the 24th ACM
SIGSPATIAL International Conference on Advances in Geographic
Information Systems, page 92. ACM, 2016.

[185] J. Zhang, Y. Zheng, D. Qi, R. Li, X. Yi, and T. Li. Predicting citywide
crowd flows using deep spatio-temporal residual networks. Artificial
Intelligence, 259:147–166, 2018.

[186] P. Zhang, Y. Jia, J. Gao, W. Song, and H. K. Leung. Short-term rainfall
forecasting using multi-layer perceptron. IEEE Transactions on Big
Data, 2018.

[187] S. Zhang, G. Wu, J. P. Costeira, and J. M. Moura. Fcn-rlstm: Deep
spatio-temporal neural networks for vehicle counting in city cameras.
In Computer Vision (ICCV), 2017 IEEE International Conference on,
pages 3687–3696. IEEE, 2017.

[188] W. Zhang, L. Han, J. Sun, H. Guo, and J. Dai. Application of multichannel 3d-cube successive convolution network for convective storm
nowcasting. arXiv preprint arXiv:1702.04517, 2017.

[189] Z. Zhang, Q. He, J. Gao, and M. Ni. A deep learning approach
for detecting traffic accidents from social media data. Transportation
research part C: emerging technologies, 86:580–596, 2018.

[190] J. Zhao, J. Xu, R. Zhou, P. Zhao, C. Liu, and F. Zhu. On prediction of
user destination by sub-trajectory understanding: A deep learning based
approach. In Proceedings of the 27th ACM International Conference
on Information and Knowledge Management, pages 1413–1422. ACM,
2018.

[191] L. Zhao, Y. Song, M. Deng, and H. Li. Temporal graph convolutional
network for urban traffic flow prediction method. arXiv preprint
arXiv:1811.05320, 2018.

[192] P. Zhao, H. Zhu, Y. Liu, Z. Li, J. Xu, and V. S. Sheng. Where to
go next: A spatio-temporal lstm model for next poi recommendation.
arXiv preprint arXiv:1806.06671, 2018.

[193] S. Zhao, T. Zhao, I. King, and M. R. Lyu. Geo-teaser: Geo-temporal
sequential embedding rank for point-of-interest recommendation. In
Proceedings of the 26th international conference on world wide web
companion, pages 153–162. International World Wide Web Conferences Steering Committee, 2017.

[194] Y. Zhao, Q. Dong, S. Zhang, W. Zhang, H. Chen, X. Jiang, L. Guo,
X. Hu, J. Han, and T. Liu. Automatic recognition of fmri-derived
functional networks using 3-d convolutional neural networks. IEEE
Transactions on Biomedical Engineering, 65(9):1975–1984, 2018.

[195] S. Zheng, Y. Yue, and J. Hobbs. Generating long-term trajectories
using deep hierarchical networks. In Advances in Neural Information
Processing Systems, pages 1543–1551, 2016.

[196] Y. Zheng. Tutorial on location-based social networks. In In proceeding
of the 21st International conference on World Wide Web, 2012.

[197] F. Zhou, Q. Gao, G. Trajcevski, K. Zhang, T. Zhong, and F. Zhang.
Trajectory-user linking via variational autoencoder. In IJCAI, pages
3212–3218, 2018.

[198] X. Zhou, Y. Shen, Y. Zhu, and L. Huang. Predicting multi-step
citywide passenger demands using attention-based neural networks. In
Proceedings of the Eleventh ACM International Conference on Web
Search and Data Mining, pages 736–744. ACM, 2018.

[199] L. Zhu, F. Guo, R. Krishnan, and J. W. Polak. A deep learning
approach for traffic incident detection in urban networks. In 2018 21st
International Conference on Intelligent Transportation Systems (ITSC),
pages 1011–1016. IEEE, 2018.

[200] Q. Zhu, J. Chen, L. Zhu, X. Duan, and Y. Liu. Wind speed prediction
with spatio–temporal correlation: A deep learning approach. Energies,
11(4):705, 2018.

[201] Y. Zhuoning, Z. Xun, and Y. Tianbao. Hetero-convlstm: A deep learning approach to traffic accident prediction on heterogeneous spatiotemporal data. In Proceedings of the 24th ACM SIGKDD International
Conference on Knowledge Discovery & Data Mining, pages 984–992.
ACM, 2018.

[202] A. Zonoozi, J.-j. Kim, X.-L. Li, and G. Cong. Periodic-crn: A convolutional recurrent model for crowd density prediction with recurring
periodic patterns. In IJCAI, pages 3732–3738, 2018.
