---
date: 2023-04-04
title: "【论文解析】通过条件GAN生成多标签临床时间序列"
linkTitle: "【论文解析】通过条件GAN生成多标签临床时间序列"
description: "【论文解析】通过条件GAN生成多标签临床时间序列"
author: Shuijing Zhang
Tags: ["AP-BL"]
---

# 通过条件GAN生成多标签临床时间序列

|    Title     | Multi-Label Clinical Time-Series Generation via Conditional GAN |
| :----------: | ------------------------------------------------------------ |
|    Author    | Chang Lu, , Chandan K. Reddy,  Ping Wang,  Dong Nie, and Yue Ning |
| Affiliations | Member, IEEE, Senior Member, IEEE,                           |
|    Emails    | clu13@stevens.edu, pwang44@stevens.edu, yue.ning@stevens.edu |
|    Paper     | http://arxiv.org/abs/2204.04797                              |
|     Code     | https://github.com/LuChang-CS/MTGAN                          |

# 摘要

With wide applications of electronic health records (EHR), deep learning methods have been adopted to analyze EHR data on various tasks such as representation learning, clinical event prediction, and phenotyping. However, due to privacy constraints, limited access to EHR becomes a bottleneck for deep learning research. Recently, generative adversarial networks (GANs) have been successful in generating EHR data. However, there are still challenges in high-quality EHR generation, including generating time-series EHR and uncommon diseases given imbalanced datasets. In this work, we propose a Multi-label Time-series GAN (MTGAN) to generate EHR data and simultaneously improve the quality of uncommon disease generation. The generator of MTGAN uses a gated recurrent unit (GRU) with a smooth conditional matrix to generate sequences and uncommon diseases. The critic gives scores using Wasserstein distance to recognize real samples from synthetic samples by considering both data and temporal features. We also propose a training strategy to calculate temporal features for real data and stabilize GAN training. Furthermore, we design multiple statistical metrics and prediction tasks to evaluate the generated data. Experimental results demonstrate the quality of the synthetic data and the effectiveness of MTGAN in generating realistic sequential EHR data, especially for uncommon diseases



随着电子健康记录（EHR）的广泛应用，深度学习方法已被采用在表征学习、临床事件预测和表型等各种任务上分析EHR数据。然而，由于隐私限制，对EHR的有限访问成为深度学习研究的瓶颈。最近，生成对抗网络（GAN）已经成功地生成了EHR数据。然而，高质量的EHR生成仍然存在挑战，包括生成时间序列EHR和给定不平衡数据集的不常见疾病。在这项工作中，我们提出了一种多标签时间序列GAN（MTGAN）来生成EHR数据，同时提高不常见疾病生成的质量。MTGAN的发生器使用具有平滑条件矩阵的门控循环单元（GRU）来生成序列和不常见的疾病。评论家给分数使用瓦瑟斯坦距离识别真正的合成样品通过考虑样本数据和时间特性。我们也提出一个计算实际数据时序特性的培训策略,稳定GAN培训。此外,我们设计多个统计指标来评估和预测任务生成的数据。实验结果证明合成的质量数据和生成现实的顺序MTGAN EHR的有效性的数据,特别是对于罕见疾病。



# 研究背景

## 为什么要生成EHR数据？

EHR不仅记录了患者的关键临床信息，而且为研究者提供了宝贵的数据资源。研究者可以根据这些EHR数据，对病人和医学概念进行表示学习，预测诊断和死亡率等健康事件，临床笔记分析，隐私保护，以及表型分析等等。一方面，大多数EHR数据不公开，因为它们包含患者的敏感临床信息，如人口统计学特征和诊断。另一方面，一些公开的EHR数据集，包括MIMIC - III和eICU ，样本数量有限，可能不适合大规模深度学习。因此，有限的电子健康档案数据成为数据驱动医疗研究的瓶颈。

## 生成EHR数据的瓶颈？

最近,生成对抗网络(GANs)已成功地生成高质量的图像。相比传统的生成模型,如自编码器和变分自编码器,GANs能够生成更真实的数据。因此,GANs也被应用于生成EHR来缓解有限的数据问题。然而,当生成电子健康档案使用现有的GANs,还有几个挑战:

- 生成时间序列EHR数据。在电子健康档案数据,一个病人可以有多个访问。然而,大多数现有的GANs生成EHR,如medGAN , EMRWGAN, Smooth-GAN,和RDP-CGAN, 只能生成独立的访问,而不是时间序列数据。因为传统GANs专为图像生成只生成一个图像噪声的输入。虽然可以将生成的随机访问序列,这种方法不能保存时间信息披露患者级别（patient-level）的访问。图1显示了一个示例visit-level数据生成和patient-level数据生成。在图1 (a),它为只有一个访问生成诊断。图1中描述的(b)是理想的序列生成：两个相邻访问的诊断和相关类似，如高血压和心力衰竭。因此,如何生成多标记时间序列与时间相关性EHR仍然是一个挑战。

- 生成常见疾病。根据著名的公共EHR数据集MIMIC-III的统计数据，一些疾病经常被诊断出来，例如高血压和糖尿病，而其他一些疾病（例如结核病）则不太常见。虽然这些疾病不经常发生，但研究它们为患者提供更好的护理计划仍然很有价值，例如，分析发生模式以提高诊断预测的准确性。尽管现有的GAN能够生成时间序列EHR数据，但他们仍然难以学习不常见疾病的良好分布。 给定高度不平衡的 EHR 数据集的情况下，我们需要找到产生不常见疾病的有效方法，例如图1（b）中的阑尾炎，而不是只产生频繁的疾病，如图1（a）所示。

- 评估合成电子健康档案数据。正如我们提到的，EHR 数据集通常具有不平衡的疾病分布。然而，当将合成图像的传统评估指标（如Kullback-Leibler散度和Jensen-Shannon散度）应用于EHR数据时，它们并没有对低频率的疾病给予足够的关注。因此，当真实EHR数据和合成EHR数据的分布在高发疾病方面接近时，我们可能仍然会得到较低的差异。因此，仍然有必要探索适当的指标来评估合成EHR数据的质量，特别是对于不常见的疾病。

{{< imgproc 1 Fill "674x240" >}}
1
{{< /imgproc >}}

# 解决办法

针对上述三个问题，本文提出了三个解决方案，

- 用GRU生成时序数据。
- 提出一个平滑条件矩阵来产生不常见的疾病。
- 不仅仅使用JS散度，而是使用多种统计指标综合进行EHR数据的评估，设计了一个针对罕见疾病的标准化距离。

# 模型架构图

{{< imgproc 2 Fill "1059x548" >}}
2
{{< /imgproc >}}



首先介绍一下模型结构，GAN一般包含生成器和判别器，对于生成器来说，给定一个噪声向量z，首先通过解码噪声向量生成第一次就诊的疾病概率P1，其中σ是sigmoid函数。在有了第一次就诊的概率以后，我们使用门控循环单元GRU进行递归生成数据，上一个时间步生成的就诊概率以及隐层状态成为GRU的输入，然后隐层状态经过一层sigmoid函数生成下一个时间步的就诊概率。

{{< imgproc eq1 Fill "263x51" >}}
eq1
{{< /imgproc >}}

{{< imgproc eq2 Fill "341x93" >}}
eq2
{{< /imgproc >}}

为了生成不常见的疾病，该论文还应用了包含注意力方法的条件矩阵，将目标疾病广播为所有的访问的概率分布，具体来说，针对疾病的种类进行采样，生成目标疾病，然后Pt是生成器生成目标疾病的概率，我们应用一个注意力权重Wv与每个时间步目标疾病的概率Pt相乘，得到每个时间步的注意力分数Vt，然后对各个时间步的分数进行归一化得到最终的注意力分数scoret。，最后创建全零条件矩阵c，目标疾病分数被赋值为score,然后把score加到概率P上，最后为了保证概率不超过1，对修正后的概率进行裁剪。

{{< imgproc eq3 Fill "502x139" >}}
eq3
{{< /imgproc >}}

{{< imgproc eq4 Fill "354x57" >}}
eq4
{{< /imgproc >}}

对于判别器来说，判别器获得每次诊断的合成样本和时间特征序列（也就是隐状态ht），将他们对应拼接起来，然后用一个MLP层计算每次诊断的判别分数，最后，这个诊断时间序列的最后得分r是一个病人所有诊断的平均值。

{{< imgproc eq5 Fill "385x85" >}}
eq5
{{< /imgproc >}}

{{< imgproc eq6 Fill "491x138" >}}
eq6
{{< /imgproc >}}



# 模型算法

{{< imgproc 3 Fill "729x798" >}}
3
{{< /imgproc >}}

上图是模型的算法框架，首先从疾病种类中采样获得目标疾病，训练判别器时，对真实数据采样，用预训练GRU获得隐状态，然后获得生成器的合成概率序列和隐状态，对概率进行取整采样，获得合成样本，然后计算梯度惩罚项，最后利用损失函数LD来优化判别器。训练生成器时，就是对噪声进行采样，然后用GRU生成诊断概率以及隐状态，用损失函数LG来优化生成器。



# 实验结果

数据集采用了MIMIC3和MIMIC4数据集，这是一个大型、免费可用的数据库，包含来自贝斯以色列女执事医疗中心重症监护室的患者的健康相关数据。

## 合成数据集质量评估

论文中对比的方法有两种，一种是用于访问级别生成的GAN：medGAN，CTGAN，EMR-WGAN，RDP-CGAN。这些都用于EHR数据生成。另一种用于患者级别生成的GAN：WGAN-GP，TimeGAN，T-CGAN。

论文中使用了多种评估指标，第一个是生成的疾病类型GT，第二个是比较常用的JS散度，第三种是作者自己提出的归一化距离ND（如下所示）。第四个是生成所有疾病种类所需的样本数目。

{{< imgproc eq7 Fill "349x88" >}}
eq7 
{{< /imgproc >}}

{{< imgproc 4 Fill "1257x435" >}}
4
{{< /imgproc >}}

从图中可以看到，在生成的疾病类型GT上，MTCAN在两个数据集上的疾病数目都是最多的。在JS散度方面，MTGAN也是最小的，在ND评价指标上MTGAN也都超过了其他方法。说明MTGAN在生成罕见疾病上是优于其他算法的。在生成所有疾病种类所需的样本数目RN上，其他的方法需要的样本数目都在10^7以上。不过从参数的数目方面，本文的方法略大于其他方法的参数量。



## 下游任务

另外该论文还用合成数据进行了下游任务的实验，在诊断预测、心力衰竭、帕金森疾病预测三个方面。诊断预测是在给定先前 T 次就诊的情况下，它预测 T + 1 次就诊中患者的所有诊断。它是一个**多标签分类**；心力衰竭/帕金森氏病预测：在给定先前 T 次就诊的情况下，预测患者是否会在 T + 1 次就诊时**是否**被诊断为心力衰竭/帕金森氏病。这是一个**二元分类**。

首先使用了两种基于RNN的改进模型Dipole和GRAM来预测诊断。w/o synthetic是在真实数据集上预训练的结果，然后把两种RNN模型分别在几个时间序列生成GAN生成的数据上进行训练，看看获得的诊断、心脏病、帕金森诊断的样本多了多少。如下图所示，可以看到MTGAN的提升是最大的。

{{< imgproc 5 Fill "1215x348" >}}
5
{{< /imgproc >}}



论文还使用不同比例的真实数据与合成数据，用两种RNN模型进行训练，然后评估他们的F1值和AUC。从图中可以看出，这些GAN模型可以在一定程度上学习EHR数据中的有效疾病分布。但是，MTGAN可以生成比其他GAN更有益于下游任务的合成EHR数据，尤其是帕金森病预测。因此，所提出的MTGAN可以生成更准确的疾病分布和在促进下游任务方面更具优势的EHR数据。

{{< imgproc 6 Fill "1356x549" >}}
6
{{< /imgproc >}}

# 总结

这篇文章的贡献是：为了应对用GAN生成EHR的挑战，提出了MTGAN来生成罕见疾病的时间序列就诊记录。MTGAN可以记录时间信息，并通过平滑条件矩阵提高EHR中罕见疾病的生成质量。实验结果表明，由MTGAN生成的合成EHR数据不仅具有更好的统计特性，而且在提高多任务预测模型的性能，特别是预测罕见疾病方面，其性能优于最新的GAN模型。然而，这项工作中主要关注生成疾病的GAN模型，即多标签生成，它没有考虑EHR中的其他特征类型，如程序、药物或实验室测试。
