---
layout: post
title: Visual Transformers - Token based image representation
article: https://arxiv.org/abs/2006.03677
submission: 2020 Jun
description: Token based transformer for semantic segmentation.
tags: ['semantic_segmentation', 'transformers']
---

Problem of convolutions: not all pixels are of the same importance (but they are treated like equals in conv), struggle with capturing long-range interactions, convolutions are bad for sparse high-level semantic features (low-level features are densely distributed, convolutions are apt for image processing early in nn, but as receptive field increases deeper in the network the number of potential patterns grows exponentially, semantic concepts become increasingly large)

Keep tokens and feature maps too: tokens: high-level semantics, feature maps: pixel-level details.

![architecture]({{ site.baseurl }}/images/visual_transformers/architecture.png){: class="center"}

Visual transformer: 
1. first use convolution to extract map representation
2. feed the feature maps to stacked visual transformers
    1. tokenizer extracts a small number of visual tokens from the feature map
![function_0]({{ site.baseurl }}/images/visual_transformers/function_0.png){: class="center"}  
to encode the position of the contributing pixels they use position encoding, which is recorded in the token coefficient A . First downsample it, than multiply it with a learnable weight to compress the positional info in A to a smaller encoding vector.  
![function_1]({{ site.baseurl }}/images/visual_transformers/function_1.png){: class="center"}  
The whole tokenizer and projector:  
![tokenizer]({{ site.baseurl }}/images/visual_transformers/tokenizer.png)  
    2. transformer captures the interaction between the visual tokens and computes the output tokens  
![function_2]({{ site.baseurl }}/images/visual_transformers/function_2.png)  
    3. projector fuses the output tokens back to the feature map
    
Q_T to decide the information a pixel needs from the visual tokens, K_T to encode the information each visual token contains, V_T to compute the values to be fused with the feature map

![function_3]({{ site.baseurl }}/images/visual_transformers/function_3.png)  

Dynamic tokens:  
they cascade visual transformers to capture more semantic concepts. First with a static W_A weith they extract a fixed set of semantic concepts, and after that they conpute the weight dynamically from the previous tokens, by splitting it into a and b. (tokenizer is eq(1))  The new tokens are the second half, and extraction of from the feature map with the first half and weight.

![function_4]({{ site.baseurl }}/images/visual_transformers/function_4.png)  

