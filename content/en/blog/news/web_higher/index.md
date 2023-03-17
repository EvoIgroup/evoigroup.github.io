---
date: 2023-03-13
title: "Higher-order Knowledge Transfer for Dynamic Community Detection with Great Changes"
linkTitle: "One paper accepted by IEEE TEVC"
author: Huixin Ma, Kai Wu, Handing Wang, and Jing Liu
description: 'Recently, we have one paper entitle "Higher-order Knowledge Transfer for Dynamic Community Detection with Great Changes" accepted by the IEEE TEVC.'
Tags: ["OP-TO"]
---

|    Title     | Higher-order Knowledge Transfer for Dynamic Community Detection with Great Changes |
| :----------: | ------------------------------------------------------------ |
|    Author    | Huixin Ma; Kai Wu; Handing Wang; Jing Liu                    |
| Affiliations | Xidian University                                            |
|    Emails    | 948181384@qq.com; kwu@xidian.edu.cn; hdwang@xidian.edu.cn; neouma@mail.xidian.edu.cn |
|    Paper     | https://doi.org/10.48550/arXiv.2211.15043                    |



## **Abstract**


Network structure evolves with time in the real world, and the discovery of changing communities in dynamic networks is an important research topic that poses challenging tasks. Most existing methods assume that no significant change occurs; namely, the difference between adjacent snapshots is slight. However, great change exists in the real world usually. The great change in the network will result in the community detection algorithms are difficulty obtaining valuable information from the previous snapshot, leading to negative transfer for the next time steps. This paper focuses on dynamic community detection with substantial changes by integrating higher-order knowledge from the previous snapshots to aid the subsequent snapshots. Moreover, to improve search efficiency, a higher-order knowledge transfer strategy is designed to determine first-order and higher-order knowledge by detecting the similarity of the adjacency matrix of snapshots. In this way, our proposal can keep the advantages of previous community detection results and transfer them to the next task. We conduct the experiments on four real-world networks, including the networks with great or minor changes. Experimental results in the low-similarity datasets demonstrate that higher-order knowledge is more valuable than first-order knowledge when the network changes significantly and keeps the advantage even if handling the high-similarity datasets. Our proposal can also guide other dynamic optimization problems with great changes.    


## **摘要**


在现实世界中，网络结构会随着时间的变化而变化。发现动态网络中不断变化的社区是一个具有挑战性的重要研究课题。大多数现有的方法都假设网络结构不会发生明显的变化；即，相邻快照之间的差异很小。然而，在现实世界中，网络大扰动的情况是存在的。网络的巨大变化将导致社区检测算法难以从之前的快照中获取有价值的信息，从而导致下一个时间步检测社团时会发生知识的负转移。本文通过集成来自先前快照的高阶知识来帮助后续快照，重点关注具有重大变化的动态社区检测。此外，为了提高搜索效率，本文设计了一种高阶知识转移策略，通过检测快照邻接矩阵的相似性来确定算法要使用一阶知识还是高阶知识。这样，我们的建议就可以保留以前的社区检测结果的优势，并将它们转移到下一个任务中。我们在四个真实世界的网络上进行实验，包括有大或小变化的网络。在低相似度数据集上的实验结果表明，当网络发生显著变化时，高阶知识比一阶知识更有价值，即使处理高相似度数据集我们的算法也能保持优势。此外，我们的建议还可以指导其他变化巨大的动态优化问题。  


## **一、创新点**

  

本文首先用实验证明了当网络变化的程度对普通的动态社团检测算法的影响是非常大的，尤其是依赖知识迁移的动态社团检测算法。以2014年Francesco Folino和Clara Pizzuti提出的DYNMOGA算法为例，本文在有巨大改变的网络上和改变微小的网络上分别做了测试，结果如下图所示。

{{< imgproc net Fill "474x373" >}}
网络变化影响图
{{< /imgproc >}}

   蓝线表示在网络变化较小的时候算法的性能，红线表示在巨大的网络变化下DYNMOGA的性能。可以看到，网络一旦经历大的扰动，DYNMOGA不能保证会有好的性能。造成这种现象的原因是原算法会使用t-1时刻的知识，来帮助检测t时刻的社团。当网络变化小的时候，有用的知识居多，因此也就能保证性能，但是当网络变化大的时候，再利用t-1时刻的知识就会发生负迁移的情况。为了解决网络变化大时候的负迁移问题，本文提出了利用多目标优化和高阶知识迁移的方法（HoKT），来检测动态社团结构，用这种方法同时也可以降低一阶知识带来的负迁移。总结一下，本文的贡献点主要有以下三个：

1.在动态网络中，经常会发生巨大的变化。大量的负迁移降低了当前的社区检测算法的性能，因为它们假设相邻的快照具有很高的相似性。本文是第一个重点研究变化巨大的动态社团检测问题的论文，并给出了一个简单而有效的解决方法。
	
2.我们提出了高阶知识转移策略，通过测量两个相邻的社区快照之间的重叠程度，来减少负迁移，在发生较大变化时增强正迁移。
	
3.该策略对其他变化较大的动态优化问题也有很好的参考。我们可以考虑如何利用早期时间戳的结果来实现更高阶的知识转移，从而平衡负转移和正转移的影响。



## **二、背景**



### **2.1 动态社团检测**

   在社交网络中，每个用户都可以当成一个节点，用户之间的关注，联系就形成了节点之间的边，这些联系和用户就形成了一个巨大的关系网络，在这样的网络中，有的用户之间联系会较为紧密，而有的用户之间联系会比较稀疏，我们可以将其中联系紧密的用户划分为一个社团。社团检测的目的就是检测出这样的社团：内部节点有很紧密的链接，而两个社团之间的链接比较稀疏。那么动态社团检测是什么呢？我们都知道，网络是动态变化的，捕捉出每个时间步的社团结构，就是动态社团检测问题。社团检测在现实生活中是很值得研究的一个课题，它将网络上有逻辑共有关系的一批节点聚集在了一起，比如，节点代表消费者，在一个社团中有大部分消费者都买过同一本书，那么，在此社团中的其他用户都有可能买这本书，我们就可以把这本书推荐给其他的节点。本文主要解决的就是如何在网络动态变化的情况下，高效的找到每一时刻网络的社团结构问题。



### **2.2 多目标优化**

本文是将动态社团检测建模为一个多目标优化问题来解决的，因此，这部分先简要介绍一下多目标优化算法。
	
现如今有许多多目标优化算法，例如NSGAII，MOEA/D等，这两个算法在这两个论文里有详细的讲解：“A fast and elitist multiobjective genetic algorithm: NSGA-II“ 、”MOEA/D: A multiobjective evolutionary algorithm based on decomposition“，感兴趣的同学可以看看。多目标优化问题通常会给定一个可行解空间*S*，和*m*个目标函数*f*1,*f*2,...,*fm*。多目标优化的目的是找到解x*，使满足下式

{{< imgproc multi Fill "210x70" >}}
多目标优化公式
{{< /imgproc >}}

其中，*gi*(*x*）和*hi*(*x*)是不等式约束和等式约束。通常，这些目标函数是相互矛盾的，优化其中一个目标函数会损害其他优化函数，因此，多目标优化问题的解是一组解，其中每个解对其他解都有独特的优势。基于多目标问题的特性，社团检测经常需要保证当前时间步社团结构预测的足够精确，同时还要最小化连续两个时间步长的社团结构的距离。所以，现在有很多研究把动态社团检测模拟为一个多目标优化算法来解决。



### **2.3 知识迁移**

知识迁移是一种学习对另一种学习的影响，假设我们已经学习过钢琴，那么我们在学习钢琴时的乐理知识，也可以应用到我们学习其他乐器的过程中。也即，将原有的认知经验应用到本质特征相同的一类事物中去。那么，知识迁移的条件就是，两种学习之间有一定的关系，并且，学习者对这一关系的理解和领悟也格外重要。如果学习者对关系的理解不充分，那么这种知识迁移就会有负迁移的可能。比如，学习钢琴时的乐理知识可以应用到学习其他乐器中，但是钢琴的指法在学习其他乐器时就不一定有用，严重时还可能影响我们学习其他乐器。
	
现在，很多关于动态社团检测的研究都假设动态网络处于一个较为稳定的环境下，即，两个连续时间点的网络结构不会发生很大的改变，那么，前一个时间点检测出的社团结构对于后一个时间点来说是可以应用的，因此，他们利用了前一个时间点的知识，来更好更快的检测后一个时间点的社团结构。
	
但是，这个假设一定成立吗？相邻时间步的知识一定对此时是最有用的吗？如果说网络发生了巨大的变化怎么办？我们抱着对这个假设怀疑的态度，经过对一系列网络结构相似度的计算和验证发现，第一，网络是有可能发生重大变化的；第二，在有些网络上，可能高阶的知识会更加有用；第三，使用高阶知识在某些时刻会减少一阶知识带来的负迁移问题。本文的动机也就由此而来。



## **三、算法**

​		

前面讲过，我们将此问题模拟为一个基于高阶知识迁移的多目标优化问题，所以在这一部分，我们首先介绍多目标优化问题的两个目标函数，其次我们介绍如何进行高阶知识迁移以及整个算法的框架。



### **3.1 目标函数**



#### **3.1.1 模块度Q**

本文使用的第一个目标函数是模块度*Q*，是目前常用的一种衡量网络社区结构强度的方法。社区划分的质量越高对应的模块度*Q*越大。模块度的大小定义为社区内部的总边数和网络中总边数的比例减去一个期望值，该期望值是将网络设定为随机网络时同样的社区分配所形成的社区内部的总边数和网络中总边数的比例的大小，其公式如下所示：

{{< imgproc volumn Fill "110x38" >}}
模块度公式
{{< /imgproc >}}

其中，*ls*是社区内连接顶点的边数，*ds*是社团中节点的度之和，*m*是*t*时刻网络中的边数。



#### **3.1.2 HoNMI**

在讲HoNMI之前，我们要先介绍一下NMI这个目标函数。NMI全称Normalized mutual information，翻译过来就是标准化互信息，在我理解，NMI实际上就是衡量两个不同网络的社团结构的相似度的一个指标，它的公式我放在下面，具体可以去看原文，或者在“An evolutionary multi-objective approach for community discovery in dynamic networks”这篇论文里也有更详细的介绍。

{{< imgproc NMI Fill "268x74" >}}
NMI公式
{{< /imgproc >}}

在许多研究中，NMI经常被设定为一个目标函数，以尽量减少不同社区结构之间的差异。NMI也是传统动态社团检测文章中经常被使用的一个目标函数，它尽可能的减少相邻两个时间步的社团结构的差异。在使用NMI的文章中存在这样一个假设：两个连续时间步长之间的网络不会剧烈变化，进化应该呈现为一个平稳的过渡。然而，这种假设并没有考虑到集群可能发生剧烈变化的情况。
	
为了解决NMI带来的一系列负迁移等问题，本文提出了一个高阶NMI，全称Higher-order normalized mutual information(HoNMI)，这也是本文的重点了。我们将提出的HoNMI作为新的目标函数来使用。HoNMI 有以下两个优点：1)采用多数投票原则消除了最后一个快照的负传递效应，2)充分利用了其他高度相似的快照的社区标签信息。HoNMI公式如下：

{{< imgproc HoNMI Fill "257x21" >}}
HoNMI公式
{{< /imgproc >}}

其中，*w* (*i*)是指时间步长*i* (*i*=1,...,*t*-1)的权重。NMI(CRt,CRi)表示第t个时间段和第i个时间段的社团相似性。诶！原来的目标函数只计算了t时间和t-1时间的互信息，而HoNMI考虑了t时间和1到*t*-1时刻所有的网络的NMI，并且给他们加权和！这不就利用了高阶网络的信息吗！
	
那我们当然也不能一股脑将t时刻之前所有的网络信息都使用到，第一是因为网络它是有大波动的情况在的，但是我们也不能忘了网络没有大扰动的情况。第二是我们还需要考虑应用哪些知识我们的性能才会更好。那如何选择有用的知识呢？我们合理的将高阶知识和一阶知识结合起来应用。具体步骤看下一个部分，我们来介绍一下这个算法的具体框架流程。



### **3.2 算法框架**

​		

下图是本文的一个算法框架图，前文说过，我们将一阶知识和高阶知识结合起来应用，那么如何结合呢？首先，我们要判断，是否网络发生了大的扰动，我们计算相邻两个时间步的网络相似度r，并定义一个参数sigma，如果相似度r大于定义的参数sigma，则判定没有发生大扰动，那么毫无疑问，一阶知识是最有用的，我们就使用NMI作为目标函数来进行优化。相反，如果相似度r小于定义的参数sigma，我们判定发生了大的扰动，此时使用HoNMI来进行社团检测。最后就是经典的进化算法操作，交叉变异和选择。总而言之，本文创新部分就主要是下图标蓝的区域，也就是进行了一个不同的目标函数选取的操作。

{{< imgproc HoKT Fill "991x968" >}}
HoKT流程图
{{< /imgproc >}}



## **四、实验**



> 实验部分，我们主要针对四个数据集来进行了实验。这部分，我们首先介绍本文的参数设置，然后对结果进行一个展示。



### **4.1 参数设置**

{{< imgproc setting Fill "335x144" >}}
参数设置
{{< /imgproc >}}

上面这个表格是本文的参数设置，可以看到，本文主要有五个参数，种群大小，代数，交叉概率，突变概率和知识阶数。前四个就是经典进化算法的参数，这里不做详细的阐述。知识阶数是本文算法里应用的一个参数，表示我们最高使用前三阶的知识来做知识迁移。



### **4.2 实验结果及分析**

下表是在Cell Phone Call数据集上的实验结果，衡量指标主要选取NMI和F1-score。NMI越高表示检测出的社团和真实的结构越相近，F1-score是精度和查全率的调和平均值，F1-score越大说明我们实验结果越好。可以看到，HoKT对于DYNMOGA和其他知识迁移问题来说都获得了更好的解。表中最后一列表示我们使用的权重和知识的阶数。
	
在t=1,2时，HoKT没有使用前面的知识，等于使用了静态的社团检测方法去检测社团，我们从表中也可以看出，结果是比较差的，而在第三步，HoKT使用了二阶知识，这时HoKT就展现了其优势，得到了最好的NMI和F1-score。权重的设置和阶数的选择和网络相似度也有很大的关系，原文中展示了网络相似度的热力图，发现相似度高的网路可以提供更优质有用的知识，在这里我们展示其中一个数据集作为例子，其他结果具体可以参考原文。
	
下面第二张图展示的是CELL PHONE CALL网络每一时间点的网络相似度。颜色越深表明相似度越高，颜色越浅表明相似度越低。t=1和t=3时刻的网络相似度r13=0.7506，而r23=0.7600（看下图二）。这两个r值之间的差异很明显，且r23大于r13，因此，t=2时的权重被设置为0.8，而t=1时的权重设置为0.2。对于其他网络时间节点，都可以按照网络相似度去选择权重。在原文中也有详细的解释。

{{< imgproc result Fill "691x207" >}}
cell phone call结果图
{{< /imgproc >}}

{{< imgproc similar Fill "1923x1027" >}}
cell phone call网络相似度
{{< /imgproc >}}

## **总结**

​		

本文关注了在网络发生巨大扰动下的社团检测问题，提出了一种HoNMI，去利用高阶知识，来平衡不同时间步之间发生的负迁移情况。我认为，本文提出的高阶知识迁移思想实际上是可以利用到其他的动态优化问题中的，而不是只局限于当前的社团检测问题。但是这个算法还有一个缺陷：权重*w*还需要用户手动设置，未来还可以改进成一个自适应算法，来获得更好的*w*。
	
此外，我认为，本文是对原来算法的假设持有怀疑态度然后经过一系列实验验证去做的改善，那往后我们在看任何论文的时候也可以都可以抱着怀疑的态度去看，以此来获得更好的创新点和idea。
