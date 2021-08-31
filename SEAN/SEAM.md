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

 ++**Fully Convolutional Box head:**++
![](https://i.imgur.com/LWzZZgF.png)

- ${(x,y)}$: location on the feature map of candidate box after RoIAlign.
${B = (l,t,r,b)}$ : bounding box regression.\
- ${P = (x_0,y_0,x_1,y_1)}$ : proposal from RPN module.\
$(X,Y)$:corresponding location of $(x,y)$ on input image\
    \begin{gather*}
        X = x_0 + x\frac{W_p}{l} +\frac{W_p}{2l},
        Y = y_0 + y\frac{H_p}{l} +\frac{H_p}{2l},(l=7)
    \end{gather*}
- B-IoU combines the information of the porposal feature and the high-level semantic feature from regression branch.
- Multipliy IoU scores with classification scores to get final scores.
- For each proposal, the detected bounding box with hightest final scores will be chosen. 



 ++**Mask attention head**++
![](https://i.imgur.com/qHNQWuw.png)




**Supervised Attention Module**\
In order to weaken the non-object information and prevent the boundaries of the objects being blurred, we apply two attention modules:
- **channel attention module**
![](https://i.imgur.com/ArUdRGs.png)

    - We use a varient of SE block.
    - In differently, we add two convolutional layers with large kernel.(Kernel size =5).It helps the network to achive better discrimination for information of the foreground and background.
    - The detected box with hightest final score will be chosen. 


- **edge attention module**
![](https://i.imgur.com/MEQXKbz.png)
- Pixels on the boundaries of the instances are hard to segmentation .The features on these regions are easily blurred by the noise or other instances.
    - We add four convolution layer after position attention module to get the edge feature.
    - The features will element-wisely sum with on the mask head,which puts the two feature in an approximately equal data space.





**Loss Function**  
 \begin{gather*}
    {L = L_{box} + L_{seg}}\\
    {L_{box}(c,B,I) = \lambda_1L_{cls}(c,c^*) + \lambda_2L_{reg}(B,B^*)+\lambda_3L_{IoU}(I,I^*)}\\
    {L_{seg} = \lambda_4L_{mask}}+\lambda_5{edge}  
\end{gather*}
${L_{reg}}:$ varient of GIoU loss. 
\begin{gather*}
    L_{reg} = -ln(\frac{GIoU+1}{2})
\end{gather*}

## Experiment 
- Training detail
    - Dataset:COCO 2017(115K training image, 5k testing image )
    - batch size: 16(2 image per GPU)
    - learning rate:0.02
    - optimizer:SGD
    - weight decay and momentum:0.0001 and 0.9
    - image size: 1333*800
    - λ1 = λ2 = λ4 = λ5 = 1, λ3 = 0.5

- Result

    ![](https://i.imgur.com/p0Nb5IZ.png =80%x)
    ![](https://i.imgur.com/CGk2qjH.png =80%x)
    ![](https://i.imgur.com/kZfRmad.png =80%x)


- We extend the original mask head in Mask R-CNN by attention module.
    ![](https://i.imgur.com/rsiZ3gI.png =80%x)

- The comparison of runtime and parameter
    ![](https://i.imgur.com/tfWEw4T.png =80%x)
