---
title: "[KDD 2020] Short reviews on 5 Papers of interests"
author: yoonkij
date: 2020-09-01 09:00:00 +0800
categories: [Research, Paper Review]
tags: [kdd, recsys]
render_with_liquid: true
---

이번 글에서는 KDD 2020에 개제된 논문 중 제목, Abstract, Introduction을 읽고 관심있는 논문을 5편 추려보았습니다. 따라서 자세한 설명은 담겨있지 않으며, 재밌는 논문 발굴 정도로 사용하시면 될 듯 합니다. 아래 논문들과 관련있는 다른 논문 알고 계시면 언제든지 추천 받습니다!(개인적 선호도는 별 3개 만점입니다.)

[KDD 2020 Accepted Papers](https://www.kdd.org/kdd2020/accepted-papers)

### On Sampled Metrics for Item Recommendation

Walid Krichene, Steffen RendleGoogle Research
(**Best Paper Award)**

[Paper](https://dl.acm.org/doi/pdf/10.1145/3394486.3403226)

**Abstract**

The task of item recommendation requires ranking a large catalogue of items given a context. Item recommendation algorithms are evaluated using ranking metrics that depend on the positions of relevant items. To speed up the computation of metrics, recent work often uses sampled metrics where only a smaller set of random items and the relevant items are ranked. **`This paper investigates sampled metrics in more detail and shows that they are inconsistent with their exact version`**, in the sense that they do not persist relative statements, e.g.,recommender A is better than B, not even in expectation. Moreover, the smaller the sampling size, the less difference there is between metrics, and for very small sampling size, all metrics collapse to the AUC metric. **`We show that it is possible to improve the quality of the sampled metrics by applying a correction`,** obtained by minimizing different criteria such as bias or mean squared error. We conclude with an empirical evaluation of the naive sampled metrics and their corrected variants. To summarize, **`our work suggests that sampling should be avoided for metric calculation`**, however if an experimental study needs to sample, the proposed corrections can improve the quality of the estimate.

**Short Reviews**

이 논문은 추천 모델 평가에 많이 사용되는 Sampled Metric, 전체 Negative items이 아닌 Sampled Negative itmes (보통 100, 1000)과 Positive Item 간의 Ranking을 평가하는 방식의 문제를 지적합니다. 큰 Item 수에 기인하는 Sorting 연산 등의 overhead를 줄이고자 많은 논문에서 Sampled Metrics을 사용합니다. 하지만 이 논문에 의하면, Sampled Metric은 전체를 사용한 True Metric에서의 모델간 우위와 일관성을 보이지 못하며, high bias-low variance를 보입니다. 따라서, 되도록 True Metric의 활용할 것을 말하며, Sample Metric을 사용해야 할 경우, 함께 제안한 Unbiased, Minimizing Bias 등의 Correction을 통해 True metric과 일관성 있는 결과로 수정해야 함을 주장합니다.

Steffen Rendle이 최근 발표한 Evaluation 관련 논문들에 연장선인 듯 보이며, 실제로 sampled metric을 사용하는 유명 논문들이 많다는 것을 고려하면 꽤나 임팩트 있어 보이고 Best Paper Award를 받은 이유도 그러하지 않나 싶습니다. 작년 RecSys 2019에서도 그렇고 Best Paper에 추천 Evaluation에 관해 되짚어보는 논문들이 선정되었네요.

참고 [1]: Evaluation Metrics for Item Recommendation under Sampling, Steffen Rendle, arxiv, 2019.참고 [2]: Neural Collaborative Filtering vs. Matrix Factorization Revisited, Steffen Rendle, Walid Krichene, Li Zhang, John Anderson, arxiv, 2020.

**개인적 관심도:** ⭐️⭐️⭐️

---

### Counterfactual Evaluation of Slate Recommendations with Sequential Reward Interactions

James McInerney, Brian Brost, Praveen Chandar, Rishabh Mehrotra, Ben Carterette
Netflix, Spotify

[Paper](https://dl.acm.org/doi/pdf/10.1145/3394486.3403229)

**Abstract**

Users of music streaming, video streaming, news recommendation, and e-commerce services often engage with content in a sequential manner. Providing and evaluating good sequences of recommendations is therefore a central problem for these services. Prior reweighting-based counterfactual evaluation methods either suffer from high variance or make strong independence assumptions about rewards. **`We propose a new counterfactual estimator that allows for sequential interactions in the rewards with lower variance in an asymptotically unbiased manner.`** Our method uses graphical assumptions about the causal relationships of the slate to reweight the rewards in the logging policy in a way that approximates the expected sum of rewards under the target policy. Extensive experiments in simulation and on a live recommender system show that our approach outperforms existing methods in terms of bias and data efficiency for the sequential track recommendations problem.

**Short Reviews**

이 논문은 음악 스트리밍, 뉴스 추천, 온라인 쇼핑 등과 같이 사용자와 추천 시스템이 실시간으로 소통하는 Online 환경을 가정하고, 이를 Multi-armed bandit framework로 부릅니다. Counterfactual Evaluation은 "What if"라는 키워드로 설명되는데, "새로운 시스템이 현재 시스템을 대신했다면 어떤 성능을 보였을까?"라는 질문에 답을 하는 past-oriented 평가방식이며, 전통적인 offline보다 현실에 가까운 환경으로 많은 관심을 받고 있습니다.

가장 기본적이며 많이 언급되는 Counterfactual Evaluation 방법은 Inverse Propensity Score (IPS)입니다. IPS는 True performance의 unbiased estimator이긴 하나 action space (추천 아이템 수)가 커짐에 따라 큰 Variance를 보이는 단점이 있습니다. 문제 해결을 위해 Sequence 간의 독립성을 부여하는 등의 가정을 추가하기도 하지만 이는 Online 환경의 Sequence간의 연관성을 무너뜨리는 한계를 가집니다. 예를 들어, 처음 보여진 좋은 Reward가 큰 추천은 그 이후 추천과 그에 따른 Reward에 영향을 미칩니다.

이 논문에서는 Reward Interaction Inverse Propensity Score (RIPS)라는 새로운 estimator를 제안합니다. Sequence간의 연관성을 Estimator에 도입하면서도 IPS의 한계인 큰 Variance를 방지하여 기존 다른 방법들에 비해 True Performance를 정확하게 예측하여 추천 시스템의 올바른 평가를 가능하게 합니다.    개인적으로 관심있게 보는 키워드인 "*counterfactual*"에 대한 논문으로 기존 offline evaluation에서 벗어난 새로운 혹은 제대로 된 (실제 추천과 일관성을 지닌) 평가가 큰 화두가 아닌가 싶습니다.

**개인적 관심도:** ⭐️⭐️⭐️

---

### Attribute-based Propensity for Unbiased Learning in Recommender Systems: Algorithm and Case Studies

Zhen Qin, Suming J. Chen, Donald Metzler, Yongwoo Noh, Jingzheng Qin, Xuanhui Wang
Google LLC

[Paper](https://dl.acm.org/doi/pdf/10.1145/3394486.3403285)

**Abstract**

Many modern recommender systems train their models based on a large amount of implicit user feedback data. Due to the inherent bias in this data (e.g., position bias), learning from it directly can lead to suboptimal models. Recently, unbiased learning was proposed to address such problems by leveraging counterfactual techniques like inverse propensity weighting (IPW). In these methods, propensity scores estimation is usually limited to item’s display position in a single user interface (UI).In this paper, **`we generalize the traditional position bias model to an attribute-based propensity framework.`** Our methods estimate propensity scores based on offline data and allow propensity estimation across a broad range of implicit feedback scenarios, e.g.,feedback beyond recommender system UI. We demonstrate this by applying this framework to three real-world large-scale recommender systems in Google Drive that serve millions of users. Foreach system, we conduct both offline and online evaluation. **`Our results show that the proposed framework is able to significantly improve upon strong production baselines`** across a diverse range of recommendation item types (documents, people-document pairs,and queries), UI layouts (horizontal, vertical, and grid layouts), and underlying learning algorithms (gradient boosted decision trees and neural networks), all without the need to intervene and degrade the user experience. The proposed models have been deployed in the production systems with ease since no serving infrastructure change is needed.

**Short Reviews**

이 논문은 Counterfactual inference 사용한 unbiased recommender learning에 대해 얘기합니다. 흔히 사용되는 (offline) implicit feedback data는 bias를 갖고 있습니다 (e.g. position, popularity bias). Unbiasedness를 위해 Counterfactual learning의 Inverse Propensity Score or Weighting (IPS, IPW)를 활용하곤 합니다. 이 방법의 핵심은 Propensity score를 구하는 것입니다. 이 논문에서는 Attribute-based propensity score를 계산합니다. 여기서 attribute란 사용자가 사용한 device (mobile or web), feedback source (추천 UI or 별도의 검색창) 등을 말합니다. 이 Propensity를 사용한 결과 Real-world case (Google Drive)에서 CTR의 향상을 가져왔다고 합니다.

**개인적 관심도:** ⭐️⭐️

---

### GPT-GNN: Generative Pre-Training of Graph Neural Networks

Ziniu Hu, Yuxiao Dong, Kuansan Wang, Kai-Wei Chang, Yizhou Sun
UCLA, Microsoft Research

[Paper](https://dl.acm.org/doi/pdf/10.1145/3394486.3403237) [Code](https://github.com/acbull/GPT-GNN)

**Abstract**

Graph neural networks (GNNs) have been demonstrated to be powerful in modeling graph-structured data. However, **`training GNNs usually requires abundant task-specific labeled data`**, which is often arduously expensive to obtain. One effective way to reduce the labeling effort is to pre-train an expressive GNN model on un-labeled data with self-supervision and then transfer the learned model to downstream tasks with only a few labels. **`In this paper, we present the GPT-GNN∗framework to initialize GNNs by generative pre-training`.** GPT-GNN introduces a self-supervised attributed graph generation task to pre-train a GNN so that it can capture the structural and semantic properties of the graph. We factorize the likelihood of the graph generation into two components: 1) Attribute Generation and 2) Edge Generation. By modeling both components, GPT-GNN captures the inherent dependency between node attributes and graph structure during the generative process. **`Comprehensive experiments on the billion-scale Open Academic Graph and Amazon recommendation data demonstrate that GPT-GNN significantly outperforms state-of-the-art GNN models without pre-training by up to 9.1% across various downstream tasks.`**

**Short Reviews**

GNN이 여러 분야에서 성공적인 성과를 보이는 반면, GNN을 학습 시키는 것은 쉽지 않습니다. 그 이유 중 하나는 학습에 task-specific label이 필요하다는 점입니다. 각 Node의 혹은 Graph Label을 할당하는 작업은 아주 쉽지 않으며 애써 학습시킨 모델을 다른 task에 적용하는 것 또한 쉽지 않습니다. 이 논문에서는 GNN을 Generative하게 Self-supervised로 학습시키는 GPT-GNN (Generative Pre-Training Graph Neural Networks)를 제안합니다. 1) Attributed graph generation task를 정의하여 Node의 특징과 Graph 구조를 잘 파악할 수 있도록 하며, 2) 이를 활용하여 GPT-GNN을 학습시켰고, 3) 1억 7900만 개 노드의 Open Academic Graph와 1억 1300개 노드의 Amazon review 데이터에 각각 Pre-training한 후, Downstream task에 fine tuning한 결과, 기존 GNN 모델보다 더 우수한 성능을 보였습니다.

개인적으로 생각만 하고 실제로 연구, 구현에는 옮기지 못한 주제로 GNN을 Pretrain하는 것입니다. GNN은 Transformer류의 모델처럼 깊게 쌓는다고 성능이 더 좋아지지 않는게 일반적으로 알려진 한계인데요, 자세히 살펴봐야겠지만 이 논문은 다량의 데이터를 얼마나 큰 GNN으로 학습한 것인지 살펴봐야겠습니다. 또한, 이 논문에서도 비슷하게 적용했지만 추천 또한 Bipartite Graph의 Link-prediction으로 볼 수 있기에 Pretrained RecSys로 적용이 가능하지 않을까? 하고 상상해봅니다.

**개인적 관심도:** ⭐️⭐️

---

## GCC: Graph Contrastive Coding for Graph Neural Network Pre-Training

Jiezhong Qiu, Qibin Chen, Yuxiao Dong, Jing Zhang, Hongxia Yang, Ming Ding, Kuansan Wang, Jie Tang
Tsinghua University, Microsoft Research, Remin University, Alibaba Group

[Paper](https://dl.acm.org/doi/pdf/10.1145/3394486.3403168) [Code](https://github.com/THUDM/GCC)

**Abstract**

Graph representation learning has emerged as a powerful technique for addressing real-world problems. Various downstream graph learning tasks have benefited from its recent developments, such as node classification, similarity search, and graph classification.However, prior arts on graph representation learning focus on do-main specific problems and train a dedicated model for each graph dataset, which is usually non-transferable to out-of-domain data. In-spired by the recent advances in pre-training from natural language processing and computer vision, **`we design Graph Contrastive Cod-ing (GCC)1—a self-supervised graph neural network pre-training framework—to capture the universal network topological proper-ties across multiple networks.`** We design GCC’s **`pre-training task as subgraph instance discrimination in and across networks and leverage contrastive learning`** to empower graph neural networks to learn the intrinsic and transferable structural representations.We conduct extensive experiments on three graph learning tasks and ten graph datasets. The results show that GCC pre-trained on a collection of diverse datasets can achieve competitive or better performance to its task-specific and trained-from-scratch counter-parts. This suggests that the pre-training and fine-tuning paradigm presents great potential for graph representation learning.

**Short Reviews**

여러 분야에서 성공적인 성과를 보이는 GNN은 대부분 특정 데이터셋에 한정된 representation learning을 합니다. 따라서, 데이터셋 A에 학습한 GNN을 B에 적용할 수 없습니다. *"그래프 구조적 패턴의 표현은 그래프 종류에 무관하게 Universal하다"* 라는 가설을 세우고, 임의의 (서로 다른 데이터로부터 온 그래프를) 같은 공간에 맵핑하여 GNN이 그래프 구조를 distinct, universal, transferable representation으로 만들 수 있도록 합니다. 이를 위해, Constrastive Learning 아이디어를 도입합니다. 간단히 말해, 특정 노드의 Subgraph와 다른 구조를 가진 노드의 Subgraph를 GNN (논문에서는 GIN 사용)으로 encode 할 경우, 두 거리가 멀어지도록 학습합니다. Pre-training 후 Node, Graph Classification task에 fine-tuning 한 결과 유의미한 성능 향상을 가져왔습니다.

위 논문과 더불어 Pretraining GNN에 대한 논문으로 공교롭게 동시에 KDD 2020에 개제됐네요.    같은 이유로 관심이 있습니다.

**개인적 관심도:** ⭐️⭐️

### Others

그 외에 논문은 제목 List-up을 하며 글을 마치겠습니다.

On Sampling Top-K Recommendation Evaluation, ⭐️⭐️ (Best Paper와 제목과 Topic이 비슷하네요! 실제로 Steffen Rendle의 Arxiv 논문이 연구 시작점이라고 합니다.)

A Framework for Recommending Accurate and Diverse Items Using Bayesian Graph Convolutional Neural Networks. ⭐️

An Efficient Neighborhood-based Interaction Model for Recommendation on Heterogeneous Graph. ⭐️

Evaluating Conversational Recommender Systems via User Simulation. ⭐️

Temporal-Contextual Recommendation in Real-Time. **(Best Paper)**⭐️

Identifying Homeless Youth At-Risk of Substance Use Disorder: Data-Driven Insights for Policymakers ⭐️