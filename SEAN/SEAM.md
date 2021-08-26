# Supervised Edge Attention Network for Accurate Image Instance Segmentation

## Abstract
- We propose a FCN box head and supervised edge attention module in mask head.
- Box head learns association between object features and detected bounding boxes.
- The edge attention module utilizes attention mechanism to hightlight object and suppress the background noise.

## Introduction
![](https://i.imgur.com/nuwsUii.png)
- Inorder to imporve the performance of bounding box regression. FCN is proposed to break the limitation of fixed anchor sizes and scales.

- The method predicts the classification  and regression pixel by pixel. 
    - The classification score obtained from  the box head shows no important interaction to the quality of the regression result.

- We propose a new branch ,"B-IoU" bases on the FCN box head.
    - Multiply the classification scores with IoU scores.

- In recently, attention mechanisms are increasingly used in segmentation network.
    - However, the availabe methods only focus on the correlation of internal features of the object and do not emphasize edge features.
    - We propose the edge attention modules to suppress useless information and hightlight boundary features.

## Approach

**Framework:**

![](https://i.imgur.com/hVIMGZb.png)
