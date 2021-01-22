---
layout: post
title: The Devil is in the Decoder
article: https://arxiv.org/abs/1707.05847
submission: 2017 Jul
description: Analyzing the upsampling layers inside the decoders.
tags: ['foundation', 'neural_net', 'semantic_segmentation', 'instance_edge_detection', 'human_keypoints', 'depth_prediction', 'colorization', 'super-resolution', 'gans']
---

Different upsampling layers:
* Transposed convolution
* Decomposed transposed convolutions: Conv + Depth-To_Space  
![decomposed_transposed_convolutions]({{ site.baseurl }}/images/devil_is_in_the_decoder/decomposed_transposed_convolutions.png)
* Conv + Depth-To_Space
![depth_to_sparse]({{ site.baseurl }}/images/devil_is_in_the_decoder/depth_to_sparse.png)
* Bilinear upsampling + conv/separable conv
* Their proposition: bilinear additive upsampling: to overcome computation problems first bilinear upsampling, but add every N consecutive channel to reduce the folloving convolution’s conputation by N  
![bilinear_additive_upsampling]({{ site.baseurl }}/images/devil_is_in_the_decoder/bilinear_additive_upsampling.png)

They also checked the affect of resudual connection in upsampling. For that they used bilinear additive upsampling without convolution at the end.
![residual_connection]({{ site.baseurl }}/images/devil_is_in_the_decoder/residual_connection.png)

The computation complexities:

![computation_complexities]({{ site.baseurl }}/images/devil_is_in_the_decoder/computation_complexities.png)

The results w/o residual connections:
* For semantic segmentation, the depth-to-space transformation is the best upsampling method
* For instance edge detection and human keypoints estimation, the skip-layers are necessary to get good results
* For instance edge detection, the best performance is obtained by transposed, depth-to-space, and bilinear additive upsampling
* For human keypoints estimation, the hourglass network uses skip-layers by default and all types of upsampling layers are about as good
* For both depth prediction and colorization, all upsampling methods perform similarly, and the specific choice of upsampling matters little
* For super-resolution, networks with skiplayers are not possible because there are no encoder modules which high-resolution (and relatively low-semantic) features. Therefore, this problem has no skip-layer entries. Only all transposed variants perform well on this task; other layers do not
* GANs have no encoder and therefore it is not possible to have skip connections. Separable convolutions perform very poorly on this task
* no single upsampling scheme provides consistently good results across all the tasks.

The results w residual connections:
* adding residual connections is beneficial
* for semantic segmentation, depth prediction, colorization, and superresolution, adding residual connections results in consistently high performance across decoder types
* For instance edge detection, transposed convolutions, depthto-space, and bilinear additive upsampling work well when no skip connections are used
* The only task which is unaffected by adding residual connections is human keypoints estimation. This is because there are already residual connections over each hourglass in the stacked hourglass network

Decoder artifacts:
Transposed convolutions have uneven overlap: convolutions at different places operate on a different number of features. Transposed convolution and depth-to-space decoder suffers from from checkerboard artifacts. This artifact can not be found with bilinear additive upsampling.

![transposed_convolution_artifact]({{ site.baseurl }}/images/devil_is_in_the_decoder/transposed_convolution_artifact.png)

Adding residual connections in GAN-s solves the visible artifacts.