
---
title: "推文规范"
linkTitle: "推文规范"
date: 2023-02-10
author: Chao Wang (xiaofengxd@126.com)
description: >
  This is the first release.
Tags: ["ML-AL", "ML-ML", "ML-KA", "ML-GL", "ML-GM", "ML-AD"]
---

**推文规范**

这篇文章主要讲解推文的规范，主要说明如下：

1. 所有推文均用markdown编写，其语法简单，使用方便。具体语法可参见：https://markdown.com.cn/basic-syntax/。
2. 本地撰写是推荐使用Typora，这是一个markdown语言的多平台编辑器，可以实时渲染结果，其网站如下：https://typora.io/。
3. 所有推文表头必须包含以下信息：题目，链接题目（与题目相同即可，较长可用缩写），日期，作者（邮箱），简要描述，文章标签。

```
title: "The First Release"
linkTitle: "The First Release"
date: 2023-02-10
author: Chao Wang (xiaofengxd@126.com)
description: >
  这是关于表头撰写的一个例子。
Tags: ["ML-AL", "ML-ML", "ML-KA", "ML-GL", "ML-GM", "ML-AL"]
```
4. 文章标签主要按照内容分为四大类，每个大类下面包含多个小类，具体见如下表格。

| 大类      | 小类 |  标签形式     |
| :----:       |    :----:   |         :----: |
| Network Sciences      |    Hypergraphs    | NS-HG  |
| Network Sciences      |    Causal Inference    |  NS-CI |
| Network Sciences      |    Community Detection    |  NS-CD |
| Network Sciences      |    Network Robustness    |  NS-NB |
| Network Sciences      |   Influence Maximization      |  NS-IM |
| Network Sciences      |    Network Reconstruction    | NS-NR  |
| Machine Learning   |    Auto Learning     |    ML-AL  |
| Machine Learning   |    Meta Learning     |   ML-ML   |
| Machine Learning   |  Kinetic Analysis       |  ML-KA    |
| Machine Learning   |    Graph Learning     |   ML-GL   |
| Machine Learning   |   Generative Model      |  ML-GM    |
| Machine Learning   |     Adversarial Learning    |   ML-AD   |
| Optimization   |    Numerical Optmization     |   OP-NO   |
| Optimization   |    Expensive Optimization      |   OP-EO   |
| Optimization   |    Large-Scale Optimization     |   OP-LO   |
| Optimization   |   Combinatorial Optimization      |   OP-CO   |
| Optimization   |   Multi-objective Optimization      |   OP-MO   |
| Optimization   |    Multi-task/Transfer Optimization     |    OP-TO  |
| Applications   |    Scheduling     |   AP-SC   |
| Applications   |   Cyber security      |   AP-CS   |
| Applications   |   Time Series Analysis      |   AP-TS   |
| Applications   |    Black-box Applications      |   AP-BB   |
| Applications   |  Placement and Routing       |   AP-PR   |
| Applications   |   Bioinformatics and Life Sciences      |   AP-BL   |

5. 推文内容和格式不限，可自由发挥，只要与科研相关即可。

6. 提交规范：md文件以及图片等元文件统一放在一个文件夹中即可，文件夹命名为你的推文题目。

7. 为大家提供了一个模板，可参照【论文分享】用于社交网络中影响力最大化的多转换进化框架。