---
layout:     post
title:      "Traffic Accident Dataset"
subtitle:   "A Countrywide Traffic Accident Dataset"
date:       2020-08-11
author:     "wumingyao"
header-img: "img/in-post/2020.08/11/bg.jpg"
tags: [论文笔记,数据集]
categories: [论文笔记]
---

## 主要内容
* [ABSTRACT](#p1)
* [一、INTRODUCTION](#p2)
* [二、TERMINOLOGY](#p3)
* [三、DATASET CREATION PROCESS](#p4)
* [四、US-ACCIDENTSDATASET ](#p5)
* [五、CONCLUSION AND FUTUREWORK ](#p6)

## 参考论文：
[《A Countrywide Traffic Accident Dataset》](https://arxiv.org/pdf/1906.05409)

## 正文

###  <span id="p1">ABSTRACT</span>
#### 目的(Objective)
目的是为了帮助研究者处理现有交通事故数据的缺点(小规模、老、私有)，并为研究者
提供一份大规模的公开的交通事故数据集，即US-Accidents。
#### 方法(Methods)：对研究的基本设计加以描述。
作者通过对数据的收集、整合和增强等数据处理过程，产生一份大规模的公开的交通事故数据集，即US-Accidents。
#### 结果(Results)
US-Accidents 当前包含225万条美国过去三年交通的案例，每个案例记录包含locatioin,time,
description,weather,period-of-day,points-of-interest等信息。
#### 结论(Conclusion)
该数据集发表论文，并且从这些数据中收集到的关于事故时空特征的一系列观察结果。
数据集可通过[链接](https://smoosavi.org/datasets/us_accidents)下载

###  <span id="p2">一、INTRODUCTION</span>
#### 研究背景和重要性(Background And Importance)
重要性：事故预测对优化公共运输，规划安全路线和经济高效地改善交通基础设施。

#### 该领域的科研空白/挑战(Description of knowledge gap or chanllenge)
对于事故分析和预测，有大量的工作聚焦于小数据集，即使有部分工作作用于大规模数据集上，
但这些数据集时私有的，还有部分公开的大规模数据集，但却很老。
#### 本文的研究课题(Topic Of Research Paper)
为了缓解这些挑战，作者提供了一份交通事故数据集US-Accident,这份数据集包含美国从2016-2019年
225万个交通事故案例。每个案例记录包含locatioin,time,description,weather,period-of-day,points-of-interest等信息。

#### 核心方法论和主要发现/结果(Highlight The Approach And Highlight Principal Findings/Result)
分析表明，大约40%的事故发生在高速公路（高速公路、州际公路等）内，约32%发生在当地公路（街道、大道等）或附近。
作者对事故与时间、兴趣点和天气条件的相关性提出了不同的见解。

贡献：

1) 提供了一份公开数据集。这份数据集包含美国从2016-2019年
225万个交通事故案例。每个案例记录包含locatioin,time,description,weather,period-of-day,points-of-interest等信息。

2) 一种用于收集、净化和增强的非确定性方法；需要为交通事故准备一个独特的大规模数据。

3) 通过分析事故热点位置、时间、天气以及与事故数据相关的兴趣点，收集到了各种见解；这些见解可直接用于城市规划、交通基础设施设计、交通管理和预测以及个性化保险等应用。
###  <span id="p3">二、TERMINOLOGY</span>
#### 定义1(Traffic Event)
使用$e=\langle lat,lng,time,type,desc \rangle$来定义一个交通事件$e$,
其中lat和lng是GPS的维度和经度，type是事件的类型，desc是事件的描述。交通事件
包括：accident,broken-vehicle,congestion,,event,
lane-blocked,flow-incident。

#### 定义2(Weather Observation Record)
使用$w=\langle lat,lng,time,tempreature,humidity,pressure,visiblity,
precip,rain,snow,fog,hail \rangle$来定义一个天气观察$w$。其中，precip代表
降水量。

#### 定义3(Point-of-Interest)
使用$p=\langle lat,lng,type \rangle$来定义兴趣点$p$,兴趣点p的type如表一所示。
![Table 1](../../../../../../img/in-post/2020.08/11/Table 1.jpg)

###  <span id="p4">三、DATASET CREATION PROCESS</span>
![Figure 1](../../../../../../img/in-post/2020.08/11/Figure 1.jpg)

#### 3.1 Traffic Data Collection
##### 3.1.1 Realtime Traffic Data Collection. 
使用两个实时数据提供器收集流数据，即"MapQuest Traffic"和”Microsoft Bing Map Traffic"。
从早上6点到晚上11点每隔90秒提取一次数据，从11点到6点每隔150秒提取一次数据。
从2016年2月到2019年3月，总共收集了227万次交通事故
；从MapQuest收集了170万次，从Bing54万次。

##### 3.1.2 Integration
数据一致性的集成移动跨两个源复制的案例并构建一个统一的数据集。
如果两个事件发生的直线距离小于250m，发生的时间间隔小于10min,
则认为两个事件是一个事件(被两个收集器同时收集到了)。
去除了24600(1%)的冗余数据。最终的数据包含225万条数据。
#### 3.2 Data Augmentation
##### 3.2.1 Augmenting with Reverse Geo-Coding. 
使用了一个工具来执行反向编码，
将地址、街道名称、相对方（左/右）、城市、县、州、国家和邮政编码转换成GPS。
这个过程是逐点地图匹配。

##### 3.2.2 Augmenting with Weather Data.
使用Weather Underground API来获取每个事故的天气信息。原始天气数据是从位于机场的1977个气象站收集的环游美国数据来源于观测记录的形式，其中每条记录包括温度、湿度、风速、压力、降水量（以毫米为单位）和条件。对于每个气象站，我们每天都会收集几条数据记录，每一条记录都会在测量的天气属性发生重大变化时报告。

##### 3.2.3 Augmenting with Points-Of-Interest.
兴趣点（POI）是地图上没有标注的位置、交通信号灯、交叉口等。

##### 3.2.4 Augmenting with Period-of-Day.
给定事故记录的起始时间，我们使用“TimeAndDate”API[22]将其标记为白天晚上。我们在四个不同的白昼上标记这个标签系统，即日出/日落、民用黄昏、航海黄昏和天文黄昏。请注意，这些系统是根据与水平面12相对应的位置来定义的.

###  <span id="p5">四、US-ACCIDENTSDATASET </span>
图3提供了数据集特征的更多详细信息。图3-（a）显示了交通事故的每日分布情况，在这两个星期中观察到的事故明显更多。根据图3的（b）和（c）部分，可以观察到工作日的小时分布为两个高峰（8点和下午5点），而周末的分布显示为一个高峰（下午1点）。图3（d）显示了大多数事故的发生地点位于交叉口或交叉口（交叉口、交通信号灯和停车场）附近。
MapQuest将更多的事故存储在区间内，而Bing的报道更多案例是交叉点。这显示了互补性这些行为是由四个数据集组成的。图3-（e）描述了从地图匹配结果（即街道名称）中提取的道路类型分布。在这里，我们注意到大约32%的事故发生在或附近的道路上（例如，街道林荫道和林荫大道），占国内高速公路（如高速公路、州际公路和州际公路）的40%。我们也注意到，BING报告的案件数量很高-快车道。终于日数据显示，大约73%的事故发生在天亮后（或当天）。
![Figure 2](../../../../../../img/in-post/2020.08/11/Figure 2.jpg)
![Figure 3](../../../../../../img/in-post/2020.08/11/Figure 3.jpg)
![Table 3](../../../../../../img/in-post/2020.08/11/Table 3.jpg)

#### 4.1 Applications of the Dataset
US-Accidents数据集可以用于一些应用中，例如实时交通事故预测、研究事故热点位置、
伤亡分析（提取事故原因和影响因素）或者研究降水或其他环境因素在交通事故上的影响。
![Table 4](../../../../../../img/in-post/2020.08/11/Table 4.jpg)

###  <span id="p6">五、CONCLUSION AND FUTUREWORK</span>
本文描述了美国事故，一个独特的，公开的机动车事故数据集，以及它的创建过程，包括几个重要步骤，如实时交通数据收集、数据集成，以及使用地图匹配、天气、一天中的时段和感兴趣点数据的多阶段数据扩充。据我们所知，美国意外地发现了这个规模的第一个全国范围的数据，其中包含了为三个国家收集到的22.5万个意外记录。从这个数据集中，我们可以得到与地点、时间、天气和兴趣点有关的各种观测结果意外韦伯里维塔斯-事故提供了未来研究合同事故的背景分析和预测在欧洲未来的工作中，我们计划使用这个数据集来进行实时交通事故预测。
