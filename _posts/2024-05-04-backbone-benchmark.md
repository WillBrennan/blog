---
layout: post
title: TensorRT Shootout - The Fastest ML Backbones for Computer Vision
tags: [machine-learning, tensorrt]
--- 

Fast and accurate models in computer-vision are critical for optimizing costs in cloud services and enabling compute on edge-devices. Often papers & libraries report GFLOPs & model size as a proxy metric for speed, but is that reliable? 

To put this to the test, we have benchmarked common computer-vision models on a [Nvidia 3090](https://www.nvidia.com/en-gb/geforce/graphics-cards/30-series/rtx-3090-3090ti/) using [TensorRT-10.0.1](https://docs.nvidia.com/deeplearning/tensorrt/release-notes/index.html#tensorrt-10)

The key findings are, 

✅ **Compute time is independent of model size and GFLOPs.**

✅ **The fastest model on a small image doesn't guarantee its the fastest on a large image**

✅ **Pick Mobilenet_v3_small for FP32 precision**

✅ **Pick ResNet-18 or Fast-SCNN for FP16 & INT8 precision**

✅ **FP16 is 35~300% faster than FP32** *(more consistent speed-ups at higher resolutions / batch-sizes)*

✅ **INT8 is 200~700% faster than FP32** *(its highly dependent on your models architecture)*

## Methodology 
The benchmark was performed with [ML Benchmark](https://github.com/WillBrennan/ml_benchmark) which is built on [trtexec](https://docs.nvidia.com/tao/tao-toolkit/text/trtexec_integration/index.html). The following `trtexec` flags were used, 

* `--cudaGraphs` - Further speed-up by removing CPU overhead in kernel launches
* `--noDataTransfer` - Disabling D2H & H2D transfers, reducing variation caused by the CPU.
* `--useSpinLock` - Spin-locking in benchmarking to avoid timing noise being caused by the OS.
* `--avgRuns=50` - Recording time over more runs to reduce variation (default=10)

The timing data was saved directly from `trteexec` with `--exportTimes`.



## Models
In addition to Resnet & VGG the following models were benchmarked: 
* [BiSeNet-V2](https://www.semanticscholar.org/paper/BiSeNet-V2%3A-Bilateral-Network-with-Guided-for-Yu-Gao/6142526105340bfbe17107032543bebcf93bf3ae)
* [FASSDNet-L2](https://www.semanticscholar.org/paper/FASSD-Net%3A-Fast-and-Accurate-Real-Time-Semantic-for-Rosas-Arias-Benitez-Garcia/d697a3004e2a7ec495018be77cb51a1c3b50a82e)
* [fast-scnn](https://www.semanticscholar.org/paper/Fast-SCNN%3A-Fast-Semantic-Segmentation-Network-Poudel-Liwicki/b2324651155468c9b6bef8a2e006272126d17608)
* [efficientnet_b0](https://www.semanticscholar.org/paper/EfficientNet%3A-Rethinking-Model-Scaling-for-Neural-Tan-Le/4f2eda8077dc7a69bb2b4e0a1a086cf054adb3f9)
* [MNASNET](https://arxiv.org/abs/1807.11626)
* [regnet_y](https://arxiv.org/abs/2003.13678)
* [shufflenet_v2](https://pytorch.org/vision/main/models/shufflenetv2.html).
 
 
Several models reporting to be faster & more accurate than Fast-SCNN were not benchmarked ([DAPSPNet](https://www.semanticscholar.org/paper/DAPSPNet%3A-Deep-Aggregation-Pyramid-Strip-Pooling-Wang-Xiong/ddb9b9f57d6940b1b91bac67f7fb9c8acacb1cf7)
, [DDRNet23_slim](https://www.semanticscholar.org/paper/Real-Time-Segmentation-of-Unstructured-Environments-Lin-Zhao/8cf38ea4b4b4977a330a5017a6e1f9fd9b9a2712)
, and [FasterSeg](https://www.semanticscholar.org/paper/FasterSeg%3A-Searching-for-Faster-Real-time-Semantic-Chen-Gong/a5bb1a662883d502ca3d8ddfa9321e8830860fd2)).

## Results
The graphs below plot FPS against resolution for models. All curves are non-linear, making it difficult to predict model compute time without benchmarking. Most of the backbones we are testing have similar compute time. 

Nvidia 3090 w/ FP16             |  Nvidia 3090 w/ FP16 & INT8
:-------------------------:|:-------------------------:
![](/blog/assets/img/tensorrt_benchmark/stats_fp16.png)  |  ![](/blog/assets/img/tensorrt_benchmark/stats_int8.png)

Below is the table of raw results, showing the number of milliseconds for a single inference with each model. It includes additional information on the accuracy, GFLOPs, and model size. We can see that the compute time is not correlated with the GFLOPs & model-size. 

![TensorRT Benchmark](/blog/assets/img/tensorrt_benchmark/tensorrt_benchmark.png)


