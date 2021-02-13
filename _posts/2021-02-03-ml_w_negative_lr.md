---
layout: post
title: Meta-Learing with Negative Learning Rate
article: https://arxiv.org/abs/2102.00940
submission: 2021 Feb
description: Recent empirical studies show, that the inner loop is unnecessary (learning rate=0) in meta-learning phase. This work shows on MAML, that the optimal learning rate for training the inner loop is always negative for the meta-learning phase, and positive for the adaptation phase.
tags: [meta_learing]
---

<strong>Problem Formulation:</strong>  
* there is a distribution of tasks $$\tau$$ with distribution of data points $$\mathcal{D}^{\tau}$$ and loss function $$\mathcal{L}^\tau$$
* meta-training: 
    * outer loop: goal is to minimize the loss function in respect to the parameter $$\omega$$: 
        <p align=center>$$\mathcal{L}^{\text {meta }}(\boldsymbol{\omega})=\underset{\tau}{\mathbb{E}} \underset{\mathcal{D}_{t}^{\tau}}{\mathbb{E}} \underset{\mathcal{D}_{v}^{\tau}}{\mathbb{E}} \mathcal{L}^{\tau}\left(\boldsymbol{\theta}^{\tau}\left(\boldsymbol{\omega}, \mathcal{D}_{t}^{\tau}\right) ; \mathcal{D}_{v}^{\tau}\right)$$</p>
    * parameter vector $$\theta$$ is class specific and depends on the meta-parameters $$\omega$$ and the training data $$\mathcal{D}_{t}$$
    * inner loop: computation of $$\theta$$, in maml gained with stochastic gradient steps: 
        <p align=center>$$\boldsymbol{\theta}^{(i)}(\boldsymbol{\omega})=\boldsymbol{\omega}-\left.\frac{\alpha_{t}}{n_{t}} \sum_{j=1}^{n_{t}} \frac{\partial \mathcal{L}^{(i)}}{\partial \theta}\right|_{\boldsymbol{\omega} ; \mathcal{D}_{j}^{(i)}}$$</p>
    * if the learning rate $$\alpha_{t}$$ is 0, then the parameters are not adapted during the meta-training: $$\theta(\omega)=\omega$$
* the meta-testing:
    * adaptation: during meta-testing, a new task is given, and the parameters $$\theta$$ are learned: 
        <p align=center>$$\boldsymbol{\theta}(\boldsymbol{\omega})=\boldsymbol{\omega}-\left.\frac{\alpha_{r}}{n_{r}} \sum_{j=1}^{n_{r}} \frac{\partial \mathcal{L}}{\partial \theta}\right|_{\boldsymbol{\omega} ; \mathcal{D}_{j}}$$</p>

<br/><br/>
<strong>Mixed Linear Regression</strong>  
They considered MAML with mixed linear regression: each task is characterized by a different linear function and a model is evaluated by the mean squared error loss
Generative model form: $$y = x^{T}w + z$$

<br/><br/>
<strong>Overparametrization case: $$p > n_{v}m$$</strong>  
* The optimal learning rate for the meta-testing adaptation step $$\alpha_{r}$$ is either $$0$$ or positive
* The meta-learning inner loop learning rate $$\alpha_{t}$$ has a unique negative absolute minimum
* However, learning of the meta parameters $$\omega$$ preformed by the outer loop, we are using the exact solution to the linear problem, it is unclear whether the inner loop should push the parameters towards higher or lower loss
![overparam]({{ site.baseurl }}/images/ml_w_negative_lr/overparam.png)
 
<br/><br/>
<strong>Underparametrization case: $$p < n_{v}m$$</strong>  
* The optimal learning rate for the meta-testing adaptation step $$\alpha_{r}$$ is either $$0$$ or positive
* The meta-learning inner loop learning rate is not straightforward to determine, but the results suggest, that performance is always better for negative $$\alpha_{t}$$-s around zero   
![underparam]({{ site.baseurl }}/images/ml_w_negative_lr/underparam.png)

<br/><br/>
<strong>Nonlinear Regression</strong>  
* 2 layer neural network with large width (for overparametrization) and ReLU activations applied to a quadratic function: $$y=\left(\mathbf{w}^{T} \mathbf{x}+b\right)^{2}+z$$
* performed better with negatice $$\alpha_{t}$$
![nonlinear]({{ site.baseurl }}/images/ml_w_negative_lr/nonlinear.png)