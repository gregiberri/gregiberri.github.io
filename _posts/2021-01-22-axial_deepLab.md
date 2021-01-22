---
layout: post
title: Axial-Deeplab - Stand-Alone Axial-Attention for Panoptic Segmentation
article: https://arxiv.org/abs/2003.07853
description: Token based transformer for semantic segmentation.
tags: ['semantic_segmentation', 'transformers']
---

Convolution success: translation equivariance to generalize the model for different positions, locality to reduce parameter number, but the latter makes long range relation modelling challenging.

Attention layers are computationally expensive,and local attention constrains the receptive field.

Axial attention: factorize the global 2D attention into 1D attentions. Global self attention is O(h^2*w^2), local attention is O(h*w*m^2) and axial attention is O(hwm) with m is the span to the height or width axis.

Position sensitivity: keys can also have information about the location: they used key-dependent positional bias in addition to the query dependent bias.

![function_0]({{ site.baseurl }}/images/axial_deeplab/function_0.png)

Axial-attention: the axial attention is a factorization of the global attention to height and width axis.

The whole axial attention with :

![position_sensitive_axial_attention]({{ site.baseurl }}/images/axial_deeplab/position_sensitive_axial_attention.png)

Including the position sensitivity, the axial-attention to the height axis with m span range:

![function_1]({{ site.baseurl }}/images/axial_deeplab/function_1.png)

![axial_attention_block]({{ site.baseurl }}/images/axial_deeplab/axial_attention_block.png)

The results with conv first layers and full attention:

![results]({{ site.baseurl }}/images/axial_deeplab/results.png)

![parameter_comparison]({{ site.baseurl }}/images/axial_deeplab/parameter_comparison.png)

