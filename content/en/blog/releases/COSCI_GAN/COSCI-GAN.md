---
date: 2023-3-12
title: "【论文分享】Generating multivariate time series with COmmon Source CoordInated GAN (COSCI-GAN)"
linkTitle: "【论文分享】Generating multivariate time series with COmmon Source CoordInated GAN (COSCI-GAN)"
description: "【论文分享】Generating multivariate time series with COmmon Source CoordInated GAN (COSCI-GAN)"
author: Beining Zhang(zhangbeiningxidian@qq.com)
Tags: ["AP-TS"]
---

# **Title**

|    Title     | Generating multivariate time series with COmmon Source CoordInated GAN (COSCI-GAN) |
| :----------: | ------------------------------------------------------------ |
|    Author    | Ali Seyfi ，Jean-Francois Rajotte ，Raymond T. Ng            |
| Affiliations | Department of Computer Science University of British Columbia ，Data Science Institute University of British Columbia |
|    Emails    | aliseyfi@cs.ubc.ca，jfraj@mail.ubc.ca，rng@cs.ubc.ca         |
|    Paper     | https://arxiv.org/abs/2205.13741                             |
|     Code     | https://github.com/aliseyfi75/COSCI-GAN                      |
## **摘要**  
Generating multivariate time series is a promising approach for sharing sensitive data in many medical, financial, and IoT applications. A common type of multivariate time series originates from a single source such as the biometric measurements from a medical patient. This leads to complex dynamical patterns between individual time series that are hard to learn by typical generation models such as GANs.There is valuable information in those patterns that machine learning models can use to better classify, predict or perform other downstream tasks. We propose a novel framework that takes time series’ common origin into account and favors channel/feature relationships preservation. The two key points of our method are: 1) the individual time series are generated from a common point in latent space and 2) a central discriminator favors the preservation of inter-channel/feature dynamics.We demonstrate empirically that our method helps preserve channel/feature correlations and that our synthetic data performs very well in downstream tasks with medical and financial data.   

## **创新点**  
    这是第一项分析如何生成多变量时间序列的研究，其中单个通道生成源自共同噪声，同时通过中央鉴别器强制保留通道间相关性。

• COSCI-GAN 在脑电图 (EEG) 眼睛状态时间序列数据集的下游任务中优于最先进的算法。

• COSCI-GAN 结果在保存EEG 数据集的统计特性方面优于最先进的算法。



## **框架**   
COSCI-GAN的模型分为三个部分：
1.单通道生成器
2.单通道鉴别器
3.中央鉴别器

在通道 GAN 中，生成器负责生成逼真的时间序列，鉴别器负责区分真实和合成的时间序列。中央鉴别器负责强制所有给定实例生成的时间序列与真实多元时间序列具有相同的相关性。同时生成所有通道需要生成模型学习所有时间序列的联合分布，这是一项需要大量数据和时间的艰巨任务。相比之下，学习单个通道的边缘分布是一项简单得多的任务。因此，与单个多通道生成器-鉴别器对相比，使用通道 GAN 的主要目的是帮助每个单独的时间序列生成器从其自己通道的数据分布中合成更准确的时间序列。通过包括中央鉴别器，文章的目标是尽可能地加强通道之间的现实相关性。

{{< imgproc COSCI_strucet Fill "734x321" >}}
COSCI-GAN结构图 
{{< /imgproc >}}

### 算法（Algorithm）  
让文章的多元时间序列的维度为 N ∗ L ∗ C，其中 N 是数据集中的实例数，L 是每个时间序列的长度，C 是通道数。有C对生成器-鉴别器，或通道GAN。所有生成器都被馈送一个共享的噪声向量以开始生成过程。通道 GAN 中的每个生成器都会合成一个TS，生成的TS和对应通道的真实TS都会传递给它们配对的判别器，判别器判断生成的TS是否与真实TS属于同一分布。

{{< imgproc suanfa Fill "873x429" >}}
算法流程图
{{< /imgproc >}}
如前所述，中央鉴别器的作用是保持通道间相关性。由所有通道生成器合成的 TS 将被连接为一个 MTS 并馈送到中央鉴别器，其目的是确定 MTS 是真实的还是假的。文章假设这将惩罚通道之间不切实际的（不）相关模式，并且文章在补充材料中提供了一些证据表明，与真实数据相比，其他最先进 (SOTA) 方法通常会夸大甚至产生不切实际的相关性相关性。

### 训练过程（Training）
在训练期间，通道 GAN 中的鉴别器（从现在起文章将其称为通道鉴别器）、它们的配对生成器和中央鉴别器将进行三人游戏。给定通道 GAN 结合中央鉴别器的三人目标是：
{{< imgproc loss Fill "805x117" >}}
loss
{{< /imgproc >}}

其中
    （1）Gi,θi是第i个发生器，它的参数为θi，
    （2）Di是第i个信道鉴别器，参数为  i，
    （3）CDα是中央鉴别器
    （4）Pdata是实时时间序列的分布，xi是时间序列x的第i个通道，Gj≠i是Gi,θi的优化步骤中具有固定参数的所有其他生成器，γ是一个超参数，用于控制在保持通道之间的相关性与在每个通道内生成更好质量的信号之间的权衡，z是从Pz分布采样的共享噪声向量。

信道GAN有许多可能的网络类型选择。文章在补充材料中表明，与基于多层感知器（MLP）的网络相比，基于长短期记忆（LSTM）的网络生成的信号质量更高。文章研究了用于中央鉴别器的基于LSTM和MLP的网络。尽管基于LSTM的网络在信道GAN中的性能优于基于MLP的网络，但基于MLP网络在中央鉴别器中的性能更好。文章假设，如果中央鉴别器太强大，结果的质量将较低，因为发生器将努力使信号更加相关，而牺牲实际的个体TS。下图是基于MLP的中央鉴别器的结构。它由三个LinearLeakyReLU Dropout（LLD）模块、一个线性模块和一个S形函数组成。
{{< imgproc LLD Fill "878x261" >}}
 LLD
{{< /imgproc >}}


## **实证评估:**
#### 1.玩具正弦数据集：多样性与相关性保持
为了能够研究 COSCI-GAN 的经验行为，特别是中央鉴别器的影响，文章需要完全控制数据集的性质，尤其是文章所需任务的基本事实。因此，文章模拟了三个具有两个通道的“玩具医疗”数据集，并将它们用作“真实”数据集来生成合成数据。对于所有这些数据集，文章假设每个实例对应于不同的患者，并且每个患者产生两个通道（c1 和 c2）的测量值。为了使数据集更真实一些，文章还假设有两种类型的患者（pt1 和 pt2），如“健康”与“状况”。
文章通过两个标准评估了COSCI-GAN的行为，特别是中央鉴别器：
（1） 多样性：通过比较真实数据集中患者信号的振幅分布（双峰高斯分布）和使用Wasserstein距离（WD）生成的样本的振幅分布来测量多样性。文章采用WDs平均值（AWD）来汇总所有渠道。较低的AWD表示与真实分布更接近。
（2） 相关性保存：要求每种患者类型的通道振幅应尽可能彼此相等。文章测量了所有信号中通道1和2的振幅，并验证了它们的相似性。文章将相关性度量定义为2D平面上映射的振幅（通道1与通道2）与斜率为1的直线之间的平均欧氏距离（AED）。AED越低，相关性越强。


{{< imgproc result1 Fill "659x225" >}}
 result1 
{{< /imgproc >}}
如表1所示，多样性和相关性保存之间存在权衡。也就是说，当文章使用CD时，相关性得到了更好的保留，但以多样性为代价（生成的时间序列的幅度边际分布和玩具简单正弦时间序列的振幅边际分布之间的相似性）。相反，不使用CD将使生成的样本分布更接近真实数据，但生成的信道相关性较小。

CD的强度由参数γ控制，当文章进行调整超参数γ的实验时，文章观察到γ有一个稳定的范围。文章最终将γ设置为5，这为本文中提出的所有实验提供了稳定的结果，无论数据集是玩具医疗数据集还是稍后讨论的真实数据集。

#### 2.sine模拟数据集基于特征的相关性分析

catch22特征集已被引入，以捕获不同时间序列数据挖掘任务中常见的22个非经典时间序列特征。使用该特征集，文章评估了在合成时间序列数据生成中如何保持捕获22个特征之间的相关性。换句话说，如果在两个通道之间的真实数据集中有一对强相关，文章希望在合成数据集中保留这一对。类似地，如果两个特征在真实数据集中不相关，那么它们在合成数据集中应该保持不相关。
  图3显示了简单正弦数据集的两个通道之间的成对相关性的三个热图。为了简化热图，文章删除了真实数据集中7个不变的特征，并保留了其余15个不同的特征。左侧热图显示了真实数据集的15个特征的成对相关性。中心热图是由COSCI-GAN生成的合成数据集的热图，没有CD，而右侧热图是COSCI-GAN生成的具有CD的热图。显然，右侧热图与左侧热图更接近。中心热图显示，如果没有CD，15个特征的几乎所有相关关系都被破坏了。
{{< imgproc result2 Fill "682x236" >}}
 result2
{{< /imgproc >}}

#### 3.EEG眼状态数据集和下游分类
文章选择了一个14通道EEG眼睛状态数据集来测量COSCI-GAN对真实信号的有效性。为了衡量COSCI-GAN分类的有效性，文章使用了对真训练和对假测试的方法，以及对假训练和对真测试的相反方法。文章在包含两个通道的数据集上测量了基于LSTM的分类器的准确度，表3显示了该比较的结果。文章比较了使用CD和不使用CD生成合成数据时分类器的准确性。再次，CD为下游分类任务带来了重要价值。
{{< imgproc result3 Fill "800x136" >}}
 result3 
{{< /imgproc >}}

#### 4.与EEG分类现有方法比较
在最后的实验中，我们将COSCI-GAN与两种最先进的（SOTA）方法进行了比较：TimeGAN和之前讨论的最新傅里叶流（Fourier Flows），在这些论文中，下游任务是预测时间序列的下一个时间点，即预测。由于我们的重点是分类，我们使用TimeGAN和Fourier Flows代码生成了我们在前一个实验中选择的五个EEG通道。如图5所示，COSCI-GAN具有最佳的分类精度，并且COSCI-GAN和其他两种方法的结果之间存在统计显著差异。
{{< imgproc result41 Fill "511x435" >}}
result41 
{{< /imgproc >}}

{{< imgproc result42 Fill "335x225" >}}
 result42 
{{< /imgproc >}}

## **总结**
在本文中，我们介绍了 COSCI-GAN，这是一种用于多变量时间序列生成的新型框架，可提供更多相关通道。通过保留通道之间的相关性，COSCIGAN 能够生成与实时时间序列更相似的时间序列，并在下游分类任务中取得比其他最先进方法更好的性能。文章已经证明我们的框架与从公共来源生成 MTS 相关，我们认为它特别适合基于人类的生物测量。在我们的实验中，我们从来没有性能限制，但是我们预见到，COSCI-GAN 不会扩展到非常多的通道，因为每个通道都需要一个专用的 GAN。然而，这个问题并不是 COSCI-GAN 方法独有的。这是 COSCI-GAN 方法的“计算资源”限制，而对于基线和许多其他方法来说，这是一个根本上难以解决的问题。此外，COSCI-GAN 具有可并行化的优势，这使其速度更快。在另一个极端，我们已经证明 COSCI-GAN 在两个通道上没有竞争力，它的性能通常比简单的基线差。





