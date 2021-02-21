---
layout: post
title: A Principled Approach for Learning Task Similarity in Multitask Learning
article: https://arxiv.org/abs/1903.09109
submission: 2019 Mar
description: The writers of this work derived the upper bound of multitask generalization error, and the proposing a training algorithm for Adversarial Multitask Neural Networks to learn the task relation coefficients and neural network parameters iteratively.
tags: [multitask_learing, task_similarity, adversarial_multitask_neural_networks]
---

Understanding the task relationship plays key role in designing good MTL: determines which <i>inductive bias</i> should be involved in the learning procedure.
They derive a new algorithm to train the Adversarial Multitask Neural Network

<br/><br/>
<strong>Preliminaries</strong>  
* we have a set of T tasks $$\left\{\hat{\mathcal{D}}_{t}\right\}_{t=1}^{T}$$ 
* we have labelling functions $$f_{t}: \mathcal{X} \rightarrow \mathcal{Y} \text { for }\left\{\left(\mathcal{D}_{t}, f_{t}\right)\right\}_{t=1}^{T}$$
* goal is to find T hypotheses: $$h_{1}, \ldots, h_{T}$$
* average expected error, where 
$$R_{t}\left(h_{t}\right) \equiv R_{t}\left(h_{t}, f_{t}\right)=\mathbb{E}_{\mathbf{x} \sim \mathcal{D}_{t}} \ell\left(h_{t}(\mathbf{x}), f_{t}(\mathbf{x})\right)$$:  
<p align=center>$$\frac{1}{T} \sum_{t=1}^{T} R_{t}\left(h_{t}\right)$$</p>
* for each task we want to minimize the empirical loss for each task. the weighted empirical error w.r.t. 
the hypothesis <i>h</i>: 
<p align=center>$$\hat{R}_{\alpha_{t}}(h)=\sum_{i=1}^{T} \boldsymbol{\alpha}_{t, i} \hat{R}_{i}(h)$$</p>
 
<br/><br/>
<strong>Similarity measures</strong>  
They considered 2 similarity measurement: 
* $$\mathcal{H}$$-divergence
* Wasserstein distance

<br/><br/>
<strong>Upper bounds for generalization error</strong>
For both distance measurements the upper bound for the generalization error looks the following: 
<p align=center>$$\begin{array}{c}
\frac{1}{T} \sum_{t=1}^{T} R_{t}\left(h_{t}\right) \leq \underbrace{\frac{1}{T} \sum_{t=1}^{T} \hat{R}_{\alpha_{t}}\left(h_{t}\right)}_{\text {Weighted empirical loss }}+\underbrace{C_{1} \sum_{t=1}^{T}\left(\sqrt{\sum_{i=1}^{T} \frac{\alpha_{t, i}^{2}}{\beta_{i}}}\right)}_{\text {Coefficient regularization }} \\
+\underbrace{\frac{1}{T} \sum_{t=1}^{T} \sum_{i=1}^{T} \alpha_{t, i} d_{distance}\left(\hat{\mathcal{D}}_{t}, \hat{\mathcal{D}}_{i}\right)}_{\text {Empirical distribution distance }}+\underbrace{C_{2}+\frac{1}{T} \sum_{t=1}^{T} \sum_{i=1}^{T} \boldsymbol{\alpha}_{t, i} \lambda_{t, i}}_{\text {Complexity \& optimal expected loss }}
\end{array}$$</p>
* the empirical loss and empirical distribution controls the task relation coefficients $$\boldsymbol{\alpha}_{1}, \ldots, \boldsymbol{\alpha}_{T}$$
* for a given task $$t$$ if a task $$i$$ has small empirical distance $$d_{distance}(\hat{\mathcal{D}}_{t}, \hat{\mathcal{D}}_{i})$$
and small empirical loss $$\hat{R}_{i}(h_{t})$$ then task $$i$$ is similar to task $$t$$
* coefficient regularization term prevents the relation coefficients locating only on $$\boldsymbol{\alpha}_{t, t}$$
* the upper bound of generalization error shows that not only the weighted empirical loss, but also the divergence should be minimized

<br/><br/>
<strong>Adversarial Multitask neural Network</strong>
* three types of parameters $$\boldsymbol{\theta}^{f}, \boldsymbol{\theta}^{d} \text { and } \boldsymbol{\theta}^{h}$$
for the feature extractor, adversarial loss, and task loss respectively
![framework]({{ site.baseurl }}/images/learning_task_similarity_in_multitask_learing/framework.png)

The network works the following way: 

1.for fixed $$\alpha_{1}, \ldots, \alpha_{T}$$ we minimize the empirical loss and the empirical distance. The latter is the same as maximizing the adversarial loss 
$$\hat{E}_{t, i}\left(\boldsymbol{\theta}^{f}, \boldsymbol{\theta}_{t, i}^{d}\right)$$
<p align=center>$$\min _{\boldsymbol{\theta}^{f}, \boldsymbol{\theta}_{1}^{h}, \ldots, \boldsymbol{\theta}_{t}^{h}} \max _{\boldsymbol{\theta}_{t, i}^{d}} \sum_{t=1}^{T} \hat{R}_{\boldsymbol{\alpha}_{t}}\left(\boldsymbol{\theta}^{f}, \boldsymbol{\theta}_{t}^{h}\right)+\rho \sum_{i, t=1}^{T} \boldsymbol{\alpha}_{t, i} \hat{E}_{t, i}\left(\boldsymbol{\theta}^{f}, \boldsymbol{\theta}_{t, i}^{d}\right)$$</p>
2.update the relation coefficients
 <p align=center>$$\begin{aligned}
\min _{\boldsymbol{\alpha}_{1}, \ldots, \boldsymbol{\alpha}_{T}} \sum_{t=1}^{T} \hat{R}_{\boldsymbol{\alpha}_{t}}\left(\boldsymbol{\theta}^{f}, \boldsymbol{\theta}_{t}^{h}\right)+\kappa_{1} \sum_{t=1}^{T}\left\|\boldsymbol{\alpha}_{t}\right\|_{2} \\
+\kappa_{2} \sum_{i, t=1}^{T} \boldsymbol{\alpha}_{t, i} \hat{d}_{t, i}\left(\boldsymbol{\theta}^{f}, \boldsymbol{\theta}_{t, i}^{d}\right) \\
\text { s.t. } \quad\left\|\boldsymbol{\alpha}_{t}\right\|_{1}=1, \quad \boldsymbol{\alpha}_{t, i} \geq 0 \quad \forall t, i
\end{aligned}$$</p>

The whole algorithm: 
![algorithm]({{ site.baseurl }}/images/learning_task_similarity_in_multitask_learing/algorithm.png)

<br/><br/>
<strong>Results</strong>
![results]({{ site.baseurl }}/images/learning_task_similarity_in_multitask_learing/results.png)
The network not only learned solving the multitask problem, but also the task relation coefficients.
![task_relation_coefficients]({{ site.baseurl }}/images/learning_task_similarity_in_multitask_learing/task_relation_coefficients.png) 