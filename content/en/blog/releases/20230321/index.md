---
date: 2023-3-21
title: "【论文解析】复杂动力学的自适应、局部线性模型"
linkTitle: "【论文解析】复杂动力学的自适应、局部线性模型"
description: "该方法基于“即使是复杂的时间序列也是局部线性的”的简单思想：将动态数据转换为线性模型的参数化空间，并详细说明了该空间的层次聚类代表的动态类别"
author: Yuanyuan Li
Tags: ["NS-CI","AP-TS"]
---

# **复杂动力学的自适应、局部线性模型**

|    Title     | Adaptive, locally linear models of complex dynamics          |
| :----------: | ------------------------------------------------------------ |
|    Author    | Antonio C. Costa; Tosif Ahamed; Greg J. Stephens             |
| Affiliations | Department of Physics and Astronomy, Vrije Universiteit Amsterdam, 1081HV Amsterdam, The Netherlands; Biological Physics Theory Unit, Okinawa Institute of Science and Technology Graduate University, Okinawa 904-0495, Japan |
|    Emails    | [g.j.stephens@vu.nl](mailto:g.j.stephens@vu.nl)              |
|    Paper     | https://www.pnas.org/cgi/doi/10.1073/pnas.1813476116         |
|     Code     | https://github.com/AntonioCCosta/ local-linear-segmentation.git |

## 摘要

The dynamics of complex systems generally include high-dimensional, nonstationary, and nonlinear behavior, all of which pose fundamental challenges to quantitative understanding. To address these difficulties, we detail an approach based on local linear models within windows determined adaptively from data. While the dynamics within each window are simple, consisting of exponential decay, growth, and oscillations, the collection of local parameters across all windows provides a principled characterization of the full time series. To explore the resulting model space, we develop a likelihood-based hierarchical clustering, and we examine the eigenvalues of the linear dynamics. We demonstrate our analysis with the Lorenz system undergoing stable spiral dynamics and in the standard chaotic regime. Applied to the posture dynamics of the nematode *Caenorhabditis elegans*, our approach identifies fine-grained behavioral states and model dynamics which fluctuate about an instability boundary, and we detail a bifurcation in a transition from forward to backward crawling. We analyze whole-brain imaging in *C. elegans* and show that global brain dynamics is damped away from the instability boundary by a decrease in oxygen concentration. We provide additional evidence for such near-critical dynamics from the analysis of electrocorticography in monkey and the imaging of a neural population from mouse visual cortex at single-cell resolution.

## 背景

复杂系统的动力学通常包括高维、非平稳和非线性行为，这些性质对系统的定量分析提出了挑战。如何用足够简单的模型来提供实质性的可解释性以捕捉复杂系统动态的定量细节呢？

## 相关工作

由于数据数量和质量的显著增加以及计算能力的增长，一种方法是用从数据中提取的属性来拟合单一的全局动态模型。例如，深度神经网络等机器学习技术可以精确地表示复杂动力学并产生准确的预测，这些方法虽然功能强大，但对于简单的概念理解来说过于复杂。其他方法包括使用稀疏回归来找到一个控制非线性动力学系统的微分方程组，以及利用jPCA将动力学近似为具有斜对称耦合的线性模型。这类全局方法不能处理非平稳性（如当一个时间序列由一组随时间变化的不同动态组成）。局部方法将动力学分割成随时间变化的更简单的组件。例如，对人脑自我调节动态临界性的研究使用了局部向量自回归模型，利用局部时间小波分析发现了黑腹果蝇的行为基序。然而，这类方法中的局部窗口是从现象学上定义的，可能会混淆不同的动态行为。基于时间序列分割的方法包括变化点检测，旨在识别时间序列中的结构变化，但往往关注变化点的位置或预测，而不是潜在动态的表示。其他技术，如隐藏马尔可夫模型，假设全局动力学是由一组系统重新访问的底层动态状态组成的，而不提供底层动力学模式的参数化。最近，切换线性动力系统（LDSs）和自回归隐马尔可夫模型开发的目的是提供这样的参数化模型，但他们需要设置从出发点开始的中断次数，或者假设存在一组潜在的动态机制，并且系统在它们之间切换，这些模态的数量是模型的一个超参数，用来平衡模型的复杂性和准确性。。

## 创新点

结合简单的LDSs与基于似然的动态中断识别算法，通过对断点或状态数进行最小先验假设来构建可解释的、数据驱动的复杂动力学模型。在短窗口中用一阶LDSs来近似动态，并使用似然比检验来估计新添加的观测值在多大程度上符合相同的线性模型，从而自适应地确定局部窗口的大小。因此，全局动力学被参数化为在不同长度的窗口内的一组线性耦合。为了探索得到的模型空间，开发了一个基于似然的层次聚类，并检验了线性动力学的特征值。层次聚类的深度可以根据后验选择，这取决于聚类的解释。

## 算法框架

{{< imgproc 1 Fill "1051x808" >}}
算法框架
{{< /imgproc >}}

简单来说，该算法在短窗口中使用一阶LDSs（可扩展到更高阶）来近似一个给定的时间序列，对窗口对{k，k+1}进行迭代，使用对数似然比Λ*data*来评估窗口之间是否存在动态中断，并利用蒙特卡罗方法在无模型变化的零假设下构建似然比分布*P*null来评估Λ*data*的重要性。当Λ*data* > Λ*thresh*时，可以确定一个动态中断，在这种情况下，保存模型参数，并从中断位置开始一个新的建模过程。如果Λdata≤Λthresh，则没有识别出中断，移动到下一个窗口对{k+1，k+2}。该方法类似于用一组局部平坦块近似复杂形状的流形，并通过局部线性模型空间内的轨迹编码非线性时间序列，以捕获完整动力学的重要性质。

## 实验

**使用基于似然的相似性度量的层次聚类分析结果模型空间。局部线性、自适应分割的应用通常会产生大量的LDSs，通过耦合矩阵的特征值和通过基于似然的相似性度量的模型聚类来探索这个空间。**

1. 通过具有稳定螺旋动力学的Lorenz系统和标准的混沌Lorenz系统来演示和分析结果。在螺旋动力学中，模型在每个瓣之间有很大的分离，而瓣内的动力学非常相似。在混沌状态下，模型空间聚类首先将吸引子的两个叶分开，整个空间是复杂和参差的。对动力学的进一步了解反映在特征值的光谱在整个线性局部模型上的分布。在螺旋动力学中，两个峰反映了一对主复共轭特征值，它们对应于一个衰减振荡（Re(λ) < 0）。混沌状态下的光谱反映了行为的复杂性，许多模型沿着原点的一维不稳定流形显示出不稳定的动力学。

{{< imgproc 2 Fill "871x836" >}}
Lorenz系统和标准的混沌Lorenz系统
{{< /imgproc >}}

2. 应用于C. elegans（线虫）的姿态动力学，揭示了一个丰富的行为空间。结果表明该方法识别了围绕不稳定边界波动的细粒度行为状态和模型动力学，并详细描述了从向前爬行到向后爬行过渡的分岔。正向爬行在层次结构的顶层与其他行为分离。在更精细的尺度上，向前爬行会变成更快和更慢的模型，而转弯和逆转则隶属另一个分支。

{{< imgproc 3 Fill "870x873" >}}
线虫的姿态动力学
{{< /imgproc >}}

模型窗口的中值大小与半个线虫的体波的持续时间相似，这表明体波动力学提供了一个重要的运动控制的时间尺度。

{{< imgproc 4 Fill "1446x804" >}}
elegans的自发逆转明显为分岔
{{< /imgproc >}}

3. 分析了C. elegans（线虫）的全脑成像，结果表明，氧浓度的降低使整体大脑动力学远离了不稳定边界。在瘫痪的线虫神经成像中，在活跃的大脑状态下，特征值在不稳定边界上的广泛分布与行为动力学的复杂性是一致的。

{{< imgproc 5 Fill "1098x811" >}}
线虫的全脑成像
{{< /imgproc >}}

4.扩展到高维动力学的分析：非人类灵长类动物的皮质电图（ECoG）记录和小鼠视觉皮层中的数百个神经群成像。

{{< imgproc 6 Fill "1593x466" >}}
脑皮质图像以及神经群成像
{{< /imgproc >}}

## 总结

简单的线性模型是分析复杂时间序列的基础，该序列基于由数据自适应确定的短窗口的可解释动力学。一个单一模型的轨迹只能呈指数增长、衰减或振荡。通过用许多这样的模型来拼接全局动力学，本算法再现了非线性、多维和非平稳的行为，并使用局部耦合集参数化了完整的动力学。为了阐明模型的结果空间，构造了一种新的基于似然的不同度量的层次聚类，并研究了动态特征值的分布和稳定性。

自适应分割和层次聚类的结合可以显式地检查每个聚类内的模型的可变性。线性模型的简单性与统计方法的强大能力相结合为更深层次地理解复杂动力学提供了一条引人注目的途径。