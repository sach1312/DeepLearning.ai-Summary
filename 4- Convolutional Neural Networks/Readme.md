# Convolutional Neural Networks

This is the fourth course of the deep learning specialization at [Coursera](https://www.coursera.org/specializations/deep-learning) which is moderated by [DeepLearning.ai](http://deeplearning.ai/). The course is taught by Andrew Ng.

## Table of contents

* [Convolutional Neural Networks](#convolutional-neural-networks)
   * [Table of contents](#table-of-contents)
   * [Course summary](#course-summary)
   * [Foundations of CNNs](#foundations-of-cnns)
      * [Computer vision](#computer-vision)
      * [Edge detection example](#edge-detection-example)
      * [Padding](#padding)
      * [Strided convolution](#strided-convolution)
      * [Convolutions over volumes](#convolutions-over-volumes)
      * [One Layer of a Convolutional Network](#one-layer-of-a-convolutional-network)
      * [A simple convolution network example](#a-simple-convolution-network-example)
      * [Pooling layers](#pooling-layers)
      * [Convolutional neural network example](#convolutional-neural-network-example)
      * [Why convolutions?](#why-convolutions)
   * [Deep convolutional models: case studies](#deep-convolutional-models-case-studies)
      * [Why look at case studies?](#why-look-at-case-studies)
      * [Classic networks](#classic-networks)
      * [Residual Networks (ResNets)](#residual-networks-resnets)
      * [Why ResNets work](#why-resnets-work)
      * [Network in Network and 1×1 convolutions](#network-in-network-and-1-X-1-convolutions)
      * [Inception network motivation](#inception-network-motivation)
      * [Inception network (GoogleNet)](#inception-network-googlenet)
      * [Using Open-Source Implementation](#using-open-source-implementation)
      * [Transfer Learning](#transfer-learning)
      * [Data Augmentation](#data-augmentation)
      * [State of Computer Vision](#state-of-computer-vision)
   * [Object detection](#object-detection)
      * [Object Localization](#object-localization)
      * [Landmark Detection](#landmark-detection)
      * [Object Detection](#object-detection-1)
      * [Convolutional Implementation of Sliding Windows](#convolutional-implementation-of-sliding-windows)
      * [Bounding Box Predictions](#bounding-box-predictions)
      * [Intersection Over Union](#intersection-over-union)
      * [Non-max Suppression](#non-max-suppression)
      * [Anchor Boxes](#anchor-boxes)
      * [YOLO Algorithm](#yolo-algorithm)
      * [Region Proposals (R-CNN)](#region-proposals-r-cnn)
   * [Special applications: Face recognition &amp; Neural style transfer](#special-applications-face-recognition--neural-style-transfer)
      * [Face Recognition](#face-recognition)
         * [What is face recognition?](#what-is-face-recognition)
         * [One Shot Learning](#one-shot-learning)
         * [Siamese Network](#siamese-network)
         * [Triplet Loss](#triplet-loss)
         * [Face Verification and Binary Classification](#face-verification-and-binary-classification)
      * [Neural Style Transfer](#neural-style-transfer)
         * [What is neural style transfer?](#what-is-neural-style-transfer)
         * [What are deep ConvNets learning?](#what-are-deep-convnets-learning)
         * [Cost Function](#cost-function)
         * [Content Cost Function](#content-cost-function)
         * [Style Cost Function](#style-cost-function)
         * [1D and 3D Generalizations](#1d-and-3d-generalizations)
   * [Extras](#extras)
      * [Keras](#keras)

## Course summary

Here is the course summary as given on the course [link](https://www.coursera.org/learn/convolutional-neural-networks):

> This course will teach you how to build convolutional neural networks and apply it to image data. Thanks to deep learning, computer vision is working far better than just two years ago, and this is enabling numerous exciting applications ranging from safe autonomous driving, to accurate face recognition, to automatic reading of radiology images. 
>
> You will:
> - Understand how to build a convolutional neural network, including recent variations such as residual networks.
> - Know how to apply convolutional networks to visual detection and recognition tasks.
> - Know to use neural style transfer to generate art.
> - Be able to apply these algorithms to a variety of image, video, and other 2D or 3D data.
>
> This is the fourth course of the Deep Learning Specialization.



## Foundations of CNNs

> Learn to implement the foundational layers of CNNs (pooling, convolutions) and to stack them properly in a deep network to solve multi-class image classification problems.

### Computer vision

- Computer vision is one of the applications that are rapidly active thanks to deep learning.
- Some of the applications of computer vision that are using deep learning includes:
  - Self driving cars.
  - Face recognition.
- Deep learning is also enabling new types of art to be created.
- Rapid changes to computer vision are making new applications that weren't possible a few years ago.
- Computer vision deep leaning techniques are always evolving making a new architectures which can help us in other areas other than computer vision.
  - For example, Andrew Ng took some ideas of computer vision and applied it in speech recognition.
- Examples of a computer vision problems includes:
  - Image classification.
  - Object detection.
    - Detect object and localize them.
  - Neural style transfer
    - Changes the style of an image using another image.
- One of the challenges of computer vision problem that images can be so large and we want a fast and accurate algorithm to work with that.
  - For example, a `1000x1000` image will represent 3 million feature/input to the full connected neural network. If the following hidden layer contains 1000, then we will want to learn weights of the shape `[1000, 3 million]` which is 3 billion parameter only in the first layer and thats so computationally expensive!
- One of the solutions is to build this using **convolution layers** instead of the **fully connected layers**.

### Edge detection example

- The convolution operation is one of the fundamentals blocks of a CNN. One of the examples about convolution is the image edge detection operation.
- Early layers of CNN might detect edges then the middle layers will detect parts of objects and the later layers will put the these parts together to produce an output.
- In an image we can detect vertical edges, horizontal edges, or full edge detector.
- Vertical edge detection:
  - An example of convolution operation to detect vertical edges:
    - ![](Images/01.png)
  - In the last example a `6x6` matrix convolved with `3x3` filter/kernel gives us a `4x4` matrix.
  - If you make the convolution operation in TensorFlow you will find the function `tf.nn.conv2d`. In keras you will find `Conv2d` function.
  - The vertical edge detection filter will find a `3x3` place in an image where there are a bright region followed by a dark region.
  - If we applied this filter to a white region followed by a dark region, it should find the edges in between the two colors as a positive value. But if we applied the same filter to a dark region followed by a white region it will give us negative values. To solve this we can use the abs function to make it positive.
- Horizontal edge detection
  - Filter would be like this

    ```
    1	1	1
    0	0	0
    -1	-1	-1
    ```

- There are a lot of ways we can put number inside the horizontal or vertical edge detections. For example here are the vertical **Sobel** filter (The idea is taking care of the middle row):

  ```
  1	0	-1
  2	0	-2
  1	0	-1
  ```

- Also something called **Scharr** filter (The idea is taking great care of the middle row):

  ```
  3	0	-3
  10	0	-10
  3	0	-3
  ```

- What we learned in the deep learning is that we don't need to hand craft these numbers, we can treat them as weights and then learn them. It can learn horizontal, vertical, angled, or any edge type automatically rather than getting them by hand.

### Padding

- In order to to use deep neural networks we really need to use **paddings**.
- In the last section we saw that a `6x6` matrix convolved with `3x3` filter/kernel gives us a `4x4` matrix.
- To give it a general rule, if a matrix `nxn` is convolved with `fxf` filter/kernel give us `n-f+1,n-f+1` matrix. 
- The convolution operation shrinks the matrix if f>1.
- We want to apply convolution operation multiple times, but if the image shrinks we will lose a lot of data on this process. Also the edges pixels are used less than other pixels in an image.
- So the problems with convolutions are:
  - Shrinks output.
  - throwing away a lot of information that are in the edges.
- To solve these problems we can pad the input image before convolution by adding some rows and columns to it. We will call the padding amount `P` the number of row/columns that we will insert in top, bottom, left and right of the image.
- In almost all the cases the padding values are zeros.
- The general rule now,  if a matrix `nxn` is convolved with `fxf` filter/kernel and padding `p` give us `n+2p-f+1,n+2p-f+1` matrix. 
- If n = 6, f = 3, and p = 1 Then the output image will have `n+2p-f+1 = 6+2-3+1 = 6`. We maintain the size of the image.
- Same convolutions is a convolution with a pad so that output size is the same as the input size. Its given by the equation:

  ```
  P = (f-1) / 2
  ```

- In computer vision f is usually odd. Some of the reasons is that its have a center value.

### Strided convolution

- Strided convolution is another piece that are used in CNNs.

- We will call stride `S`.

- When we are making the convolution operation we used `S` to tell us the number of pixels we will jump when we are convolving filter/kernel. The last examples we described S was 1.

- Now the general rule are:
  -  if a matrix `nxn` is convolved with `fxf` filter/kernel and padding `p` and stride `s` it give us `(n+2p-f)/s + 1,(n+2p-f)/s + 1` matrix. 

- In case `(n+2p-f)/s + 1` is fraction we can take **floor** of this value.

- In math textbooks the conv operation is filpping the filter before using it. What we were doing is called cross-correlation operation but the state of art of deep learning is using this as conv operation.

- Same convolutions is a convolution with a padding so that output size is the same as the input size. Its given by the equation:

  ```
  p = (n*s - n + f - s) / 2
  When s = 1 ==> P = (f-1) / 2
  ```

### Convolutions over volumes

- We see how convolution works with 2D images, now lets see if we want to convolve 3D images (RGB image)
- We will convolve an image of height, width, # of channels with a filter of a height, width, same # of channels. Hint that the image number channels and the filter number of channels are the same.
- We can call this as stacked filters for each channel!
- Example:
  - Input image: `6x6x3`
  - Filter: `3x3x3`
  - Result image: `4x4x1`
  - In the last result p=0, s=1
- Hint the output here is only 2D.
- We can use multiple filters to detect multiple features or edges. Example.
  - Input image: `6x6x3`
  - 10 Filters: `3x3x3`
  - Result image: `4x4x10`
  - In the last result p=0, s=1

### One Layer of a Convolutional Network

- First we convolve some filters to a given input and then add a bias to each filter output and then get RELU of the result. Example:
  - Input image: `6x6x3`         `# a0`
  - 10 Filters: `3x3x3`         `#W1`
  - Result image: `4x4x10`     `#W1a0`
  - Add b (bias) with `10x1` will get us : `4x4x10` image      `#W1a0 + b`
  - Apply RELU will get us: `4x4x10` image                `#A1 = RELU(W1a0 + b)`
  - In the last result p=0, s=1
  - Hint number of parameters here are: `(3x3x3x10) + 10 = 280`
- The last example forms a layer in the CNN.
- Hint: no matter the size of the input, the number of the parameters is same if filter size is same. That makes it less prone to overfitting.
- Here are some notations we will use. If layer l is a conv layer:

  ```
  Hyperparameters
  f[l] = filter size
  p[l] = padding	# Default is zero
  s[l] = stride
  nc[l] = number of filters

  Input:  n[l-1] x n[l-1] x nc[l-1]	Or	 nH[l-1] x nW[l-1] x nc[l-1]
  Output: n[l] x n[l] x nc[l]	Or	 nH[l] x nW[l] x nc[l]
  Where n[l] = (n[l-1] + 2p[l] - f[l] / s[l]) + 1

  Each filter is: f[l] x f[l] x nc[l-1]

  Activations: a[l] is nH[l] x nW[l] x nc[l]
  		     A[l] is m x nH[l] x nW[l] x nc[l]   # In batch or minbatch training
  		     
  Weights: f[l] * f[l] * nc[l-1] * nc[l]
  bias:  (1, 1, 1, nc[l])
  ```

### A simple convolution network example

- Lets build a big example.
  - Input Image are:   `a0 = 39x39x3`
    - `n0 = 39` and `nc0 = 3`
  - First layer (Conv layer):
    - `f1 = 3`, `s1 = 1`, and `p1 = 0`
    - `number of filters = 10`
    - Then output are `a1 = 37x37x10`
      - `n1 = 37` and `nc1 = 10`
  - Second layer (Conv layer):
    - `f2 = 5`, `s2 = 2`, `p2 = 0`
    - `number of filters = 20`
    - The output are `a2 = 17x17x20`
      - `n2 = 17`, `nc2 = 20`
    - Hint shrinking goes much faster because the stride is 2
  - Third layer (Conv layer):
    - `f3 = 5`, `s3 = 2`, `p3 = 0`
    - `number of filters = 40`
    - The output are `a3 = 7x7x40`
      - `n3 = 7`, `nc3 = 40`
  - Forth layer (Fully connected Softmax)
    - `a3 = 7x7x40 = 1960`  as a vector..
- In the last example you seen that the image are getting smaller after each layer and thats the trend now.
- Types of layer in a convolutional network:
  - Convolution. 		`#Conv`
  - Pooling      `#Pool`
  - Fully connected     `#FC`

### Pooling layers

- Other than the conv layers, CNNs often uses pooling layers to reduce the size of the inputs, speed up computation, and to make some of the features it detects more robust.
- Max pooling example:
  - ![](Images/02.png)
  - This example has `f = 2`, `s = 2`, and `p = 0` hyperparameters
- The max pooling is saying, if the feature is detected anywhere in this filter then keep a high number. But the main reason why people are using pooling because its works well in practice and reduce computations.
- Max pooling has no parameters to learn.
- Example of Max pooling on 3D input:
  - Input: `4x4x10`
  - `Max pooling size = 2` and `stride = 2`
  - Output: `2x2x10`
- Average pooling is taking the averages of the values instead of taking the max values.
- Max pooling is used more often than average pooling in practice.
- If stride of pooling equals the size, it will then apply the effect of shrinking.
- Hyperparameters summary
  - f : filter size.
  - s : stride.
  - Padding are rarely uses here.
  - Max or average pooling.

### Convolutional neural network example

- Now we will deal with a full CNN example. This example is something like the ***LeNet-5*** that was invented by Yann Lecun.
  - Input Image are:   `a0 = 32x32x3`
    - `n0 = 32` and `nc0 = 3`
  - First layer (Conv layer):        `#Conv1`
    - `f1 = 5`, `s1 = 1`, and `p1 = 0`
    - `number of filters = 6`
    - Then output are `a1 = 28x28x6`
      - `n1 = 28` and `nc1 = 6`
    - Then apply (Max pooling):         `#Pool1`
      - `f1p = 2`, and `s1p = 2`
      - The output are `a1 = 14x14x6`
  - Second layer (Conv layer):   `#Conv2`
    - `f2 = 5`, `s2 = 1`, `p2 = 0`
    - `number of filters = 16`
    - The output are `a2 = 10x10x16`
      - `n2 = 10`, `nc2 = 16`
    - Then apply (Max pooling):         `#Pool2`
      - `f2p = 2`, and `s2p = 2`
      - The output are `a2 = 5x5x16`
  - Third layer (Fully connected)   `#FC3`
    - Number of neurons are 120
    - The output `a3 = 120 x 1` . 400 came from `5x5x16`
  - Forth layer (Fully connected)  `#FC4`
    - Number of neurons are 84
    - The output `a4 = 84 x 1` .
  - Fifth layer (Softmax)
    - Number of neurons is 10 if we need to identify for example the 10 digits.
- Hint a Conv1 and Pool1 is treated as one layer.
- Some statistics about the last example:
  - ![](Images/03.png)
- Hyperparameters are a lot. For choosing the value of each you should follow the guideline that we will discuss later or check the literature and takes some ideas and numbers from it.
- Usually the input size decreases over layers while the number of filters increases.
- A CNN usually consists of one or more convolution (Not just one as the shown examples) followed by a pooling.
- Fully connected layers has the most parameters in the network.
- To consider using these blocks together you should look at other working examples firsts to get some intuitions.

### Why convolutions?

- Two main advantages of Convs are:
  - Parameter sharing.
    - A feature detector (such as a vertical edge detector) that's useful in one part of the image is probably useful in another part of the image.
  - sparsity of connections.
    - In each layer, each output value depends only on a small number of inputs which makes it translation invariance.
- Putting it all together:
  - ![](Images/04.png)

## Deep convolutional models: case studies

> Learn about the practical tricks and methods used in deep CNNs straight from the research papers.

### Why look at case studies?

- We learned about Conv layer, pooling layer, and fully connected layers. It turns out that computer vision researchers spent the past few years on how to put these layers together.
- To get some intuitions you have to see the examples that has been made.
- Some neural networks architecture that works well in some tasks can also work well in other tasks.
- Here are some classical CNN networks:
  - **LeNet-5**
  - **AlexNet**
  - **VGG**
- The best CNN architecture that won the last ImageNet competition is called **ResNet** and it has 152 layers!
- There are also an architecture called **Inception** that was made by Google that are very useful to learn and apply to your tasks.
- Reading and trying the mentioned models can boost you and give you a lot of ideas to solve your task.
- Ideas may be useful in other domains, not just computer vision.

### Classic networks

- In this section we will talk about classic networks which are **LeNet-5**, **AlexNet**, and **VGG**.

- **LeNet-5**

  - The goal for this model was to identify handwritten digits in a `32x32x1` grayscale image. Here are the drawing of it:
  - ![](Images/05.png)
  - This model was published in 1998. The last layer wasn't using softmax back then.
  - It has 60k parameters. The softmax performs 10-way classfication. 
  - The dimensions of the image decreases as the number of channels increases.
  - `Conv ==> Pool ==> Conv ==> Pool ==> FC ==> FC ==> softmax` this type of arrangement is quite common. (note that FC just connects some nodes to some other nodes.)
  - In the original LeCun paper on LeNet, the activation function used in the paper was Sigmoid and Tanh. Modern implementation uses RELU in most of the cases.
  - Moreover, since the paper was written at a time when computation was not developed yet, it was common to have some filters look at specific channels of the input block. This was to conserve computational power, and is quite irrelevant today. 
  - The paper also used a sigmoid non-linearity after the pooling layer. 
  - [[LeCun et al., 1998. Gradient-based learning applied to document recognition]](http://ieeexplore.ieee.org/document/726791/?reload=true)

- **AlexNet**

  - Named after Alex Krizhevsky who was the first author of this paper. The other authors include Geoffrey Hinton and Ilya Sutskever.

  - The goal for the model was the ImageNet challenge which classifies images into 1000 classes. Here are the drawing of the model:

  - ![](Images/06.png)

  - Summary:

    - ```
      Conv => Max-pool => Conv => Max-pool => Conv => Conv => Conv => Max-pool ==> Flatten ==> FC ==> FC ==> Softmax
      ```

  - Similar to LeNet-5 but bigger.

  - Has 60 Million parameter compared to 60k parameter of LeNet-5.

  - It used the ReLU activation function.

  - The original paper contains Multiple GPUs and Local Response normalization (RN).

    - Multiple GPUs were used because the GPUs were not so fast back then. The layers were split across GPUs. 
    - Essentially, local RN just normalizes all values across all channels, while looking down from one block. Researchers proved that Local Response normalization doesn't help much so for now don't bother yourself for understanding or implementing it. 

  - This paper convinced the computer vision researchers that deep learning is so important.

  - [[Krizhevsky et al., 2012. ImageNet classification with deep convolutional neural networks]](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf)

- **VGG-16**

  - A modification for AlexNet.
  - Instead of having a lot of hyperparameters lets have some simpler network.
  - Focus on having only these blocks:
    - CONV = 3 X 3 filter, s = 1, same  
    - MAX-POOL = 2 X 2 , s = 2
  - Here are the architecture:
    - ![](Images/07.png)
  - Since the convolutions are 'same' i.e appropriate padding is done beforehand, the dimensions after a convolutional operation remains the same. 
  - This network is large even by modern standards. It has around 138 million parameters. However, it is also quite simple, as all the increments were relatively uniform in nature (doubling of number of filters, etc).
    - Most of the parameters are in the fully connected layers.
  - It has a total memory of 96MB per image for only forward propagation!
    - Most memory are in the earlier layers.
  - Number of filters increases from 64 to 128 to 256 to 512. 512 was made twice.
  - Pooling was the only one who is responsible for shrinking the dimensions.
  - There are another version called **VGG-19** which is a bigger version of this network. But most people uses the VGG-16 instead of the VGG-19 because it performs just as well. 
  - VGG paper is attractive it tries to make some rules regarding using CNNs.
  - [[Simonyan & Zisserman 2015. Very deep convolutional networks for large-scale image recognition]](https://arxiv.org/abs/1409.1556)

### Residual Networks (ResNets)

- Very, very deep NNs are difficult to train because of vanishing and exploding gradients problems.
- In this section we will learn about skip connection which makes you take the activation from one layer and suddenly feed it to another layer even much deeper in NN which allows you to train large NNs even with layers greater than 100.
- **Residual block**
  - ResNets are built out of Residual blocks.
  - ![](Images/08.png)
  - There are two connections, the 'main' and the 'shortcut/skip connection'. 
  - They add a shortcut/skip connection before the ReLU nonlinearity, so what passes into the ReLU block is not just `z[l+2]`, it is `z[l+2]+a[l]`.
  - The authors of this block find that you can train a deeper NNs using stacking this block.
  - [[He et al., 2015. Deep residual networks for image recognition]](https://arxiv.org/abs/1512.03385)
- **Residual Network**
  - Are a NN that consists of some Residual blocks. Any network without skip connections is a 'plain network'. 
  - ![](Images/09.png)
  - The above network has **5** residual blocks. 
  - These networks can go deeper without hurting the performance. In the normal NN - Plain networks - the theory tell us that if we go deeper we will get a better solution to our problem, but because of the vanishing and exploding gradients problems the performance of the network suffers as it goes deeper. Thanks to Residual Network we can go deeper as we want now.
  - ![](Images/10.png)
  - On the left is the normal NN and on the right are the ResNet. As you can see the performance of ResNet increases as the network goes deeper.
  - In some cases going deeper won't effect the performance and that depends on the problem on your hand.
  - Some people are trying to train 1000 layer now (which isn't used in practice), but the idea is this: even with a network that big, ResNets are still very effective. 
  - [He et al., 2015. Deep residual networks for image recognition]

### Why ResNets work

- We know that for a network to do well on a test set, the minimum requirement and assumption is for it to perform well on the training set. As ResNets perform well enough on even very deep networks, we can play this to our advantage.
- Let's see an example that illustrates why ResNet work.

  - We have a big NN as the following:

    - `X --> Big NN --> a[l]`

  - Let's add two layers to this network as a residual block:

    - `X --> Big NN --> a[l] --> Layer1 --> Layer2 --> a[l+2]`
    - And a`[l]` has a direct (skip) connection to `a[l+2]`

  - Suppose we are using RELU activations.

  - Then:

    - ```
      a[l+2] = g( z[l+2] + a[l] )  #a[2] comes from skip connection we just added
      	   = g( W[l+2] a[l+1] + b[l+2] + a[l] )
      ```

  - Then if we are using L2 regularization, `W[l+2]` will shrink. 

  - If `W[l+2]` goes to zero (and for the sake for argument `b[l+2]` goes to zero), then `a[l+2] = g( a[l] ) = a[l]`. Why `a[l]`? It's because `a[l]` is the output of a ReLU block (and is non-negative), so the ReLU return the non-negative value only. 

  - This show that identity function is easy for a residual block to learn, i.e. we got `a[l]=a[l+2]` even when we added two extra layers. 
  
  - Therefore, if we wish to train deeper NNs, but some layers actually don't possess much significance, ResNets ensure that the identity function is easily learnt and make the network behave as thought the two layers did not exist. Therefore, it makes it easier for us to train deeper networks. 

  - Another point to note: if I added two extra layers to a plain net, it is actually difficult for such a network to pick the right parameters to learn an identity function properly. It cannot be guaranteed that the performance of plain nets will not worsen in case more layers are added. 
  
 - In case of ResNets, we are guaranteed that worst case, the ResNet just learns the identity function; in the best case, it learns more. Therefore, we are guaranteed that in case more layers are added, performance will either remain the same/improve. 

  - Hint: dimensions of z[l+2] and a[l] have to be the same in resNets (this is why most ResNets use 'same' convolutions). In case they have different dimensions, we multiply by a matrix `Ws` to ensure equality of dimensions (which can be either learned or fixed).

    - `a[l+2] = g( z[l+2] + Ws * a[l] ) # The added Ws should make the dimensions equal`
    - `Ws` also can be a zero padding.

- Using a skip-connection helps the gradient to backpropagate and thus helps you to train deeper networks.

- Lets take a look at ResNet on images.

  - Here are the architecture of **ResNet-34**:
  - ![](Images/resNet.jpg)
  - All the 3x3 Conv are same Convs. 
  - Keep it simple in design of the network.
  - spatial size /2 => # filters x2
  - No FC layers, No dropout is used.
  - Two main types of blocks are used in a ResNet, depending mainly on whether the input/output dimensions are same or different. You are going to implement both of them.
  - The dotted lines is the case when the dimensions are different. To solve then they down-sample the input by 2 and then pad zeros to match the two dimensions. There's another trick which is called bottleneck which we will explore later.

- Useful concept (**Spectrum of Depth**):

  - ![](Images/12.png)
  - Taken from [icml.cc/2016/tutorials/icml2016_tutorial_deep_residual_networks_kaiminghe.pdf](icml.cc/2016/tutorials/icml2016_tutorial_deep_residual_networks_kaiminghe.pdf)

- Residual blocks types:

  - Identity block:
    - ![](Images/16.png)
    - Hint the conv is followed by a batch norm `BN` before `RELU`. Dimensions here are same.
    - This skip is over 2 layers. The skip connection can jump n connections where n>2
    - This drawing represents [Keras](https://keras.io/) layers.
  - The convolutional block:
    - ![](Images/17.png)
    - The conv can be bottleneck 1 x 1 conv

### Network in Network and 1 X 1 convolutions

- A 1 x 1 convolution  - We also call it Network in Network- is so useful in many CNN models.

- What does a 1 X 1 convolution do? Isn't it just multiplying by a number?

  - Let's first consider an example:
    - Input: `6x6x1`
    - Conv: `1x1x1` one filter.        `# The 1 x 1 Conv`
    - Output: `6x6x1`
      - In this example, yes. 1 x 1 just multiplies.
  - Another example:
    - Input: `6x6x32`
    - Conv: `1x1x32` 5 filters.     `# The 1 x 1 Conv`
    - Output: `6x6x5`
      - 1 x 1 makes a little more sense here.

- The Network in Network is proposed in [Lin et al., 2013. Network in network]

- It has been used in a lot of modern CNN implementations like ResNet and Inception models.

- A 1 x 1 convolution is useful when:

  - We want to shrink the number of channels. We also call this feature transformation.
    - Let's say we have an input of 28x28x192. Each filter is of size 1x1x192. Each of the 192 numbers is multiplied with the other 192 numbers, and we get one number for each of the 28x28 squares. This process is repeated for 32 filters, and we get 28x28x32 dimensional output. 
  - We will later see that by shrinking it we can save a lot of computations.
  - If we have specified the number of 1 x 1 Conv filters to be the same as the input number of channels then the output will contain the same number of channels. Then the 1 x 1 Conv will act like a non linearity and will learn non linearity operator. 

- Replace fully connected layers with 1 x 1 convolutions as Yann LeCun believes they are the same.

  - > In Convolutional Nets, there is no such thing as "fully-connected layers". There are only convolution layers with 1x1 convolution kernels and a full connection table. [Yann LeCun](https://www.facebook.com/yann.lecun/posts/10152820758292143) 

- [[Lin et al., 2013. Network in network]](https://arxiv.org/abs/1312.4400)

### Inception network motivation

- When you design a CNN you have to decide all the layers yourself. Will you pick a 3 x 3 Conv or 5 x 5 Conv or maybe a max pooling layer? You have so many choices.
- What **inception** tells us is, why not use all of them at once?
- **Inception module**, naive version:
  - ![](Images/13.png)
  - Hint: all convolutional and max-pool layers are 'same' here. This is to ensure dimensions are the same. 
  - Input to the inception module is 28 x 28 x 192 and the output is 28 x 28 x 256. 
  - Instead of manually picking our choices, we perform all the convolutions and pools that we might want. We will let the NN learn and decide which it want to use most.
  - [[Szegedy et al. 2014. Going deeper with convolutions]](https://arxiv.org/abs/1409.4842)
- The problem of computational cost in Inception model:
  - Let's just focus on 5 x 5 Conv that we have done in the last example.
  - There are 32 same filters of 5 x 5, and the inputs are 28 x 28 x 192.
  - Output should be 28 x 28 x 32.
  - The total number of multiplications needed here are:
    - Number of outputs * Filter size * Filter size * Input dimensions
    - Which equals: `28 * 28 * 32 * 5 * 5 * 192 = 120 Mil` 
    - 120 Mil multiply operation still a problem in the modern day computers.
  - Using a 1 x 1 convolution we can reduce 120 mil to just 12 mil. Let's see how.
- Using 1 X 1 convolution to reduce computational cost:
  - The new architecture are:
    - X0 shape is (28, 28, 192)
    - We then apply 16 (1 x 1 Convolution)
    - That produces X1 of shape (28, 28, 16)
      - Hint, we have reduced the dimensions here.
    - Then apply 32  (5 x 5 Convolution)
    - That produces X2 of shape (28, 28, 32)
  - Now lets calculate the number of multiplications:
    - For the first Conv: `28 * 28 * 16 * 1 * 1 * 192 = 2.5 Mil`
    - For the second Conv: `28 * 28 * 32 * 5 * 5 * 16 = 10 Mil`
    - So the total number are 12.5 Mil approx. which is so good compared to 120 Mil
- A 1 x 1 Conv here is called Bottleneck `BN`. Why? It takes a huge volume as input, shrinks it to an intermediate volume, and then performs further computations.
- It turns out that the 1 x 1 Conv won't hurt the performance.

### Inception network (GoogleNet)
- **Inception module**
  - ![](Images/14.png)
  - Why is there a 1x1 convolution after the maxpooling? It's because the maxpool outputs 192 channels, which is a lot. Therefore, we wish to reduce the number of channels. 
- The inception network consists of many inception modules concatenated on top of each other. 
- The research paper consists of some side branches that are added. What is the purpose of a side branch? It tries to make a prediction directly from a hidden layer, instead of computing a prediction only on the output. 
- Such a side branch has a regularizing effect. 
- Example of inception model in Keras:
  - ![](Images/inception_block1a.png)
- The name inception was taken from a *meme* image which was taken from **Inception movie**
- Here are the full model:
  - ![](Images/15.png)
- Since the development of the Inception module, the authors and the others have built another versions of this network. Like inception v2, v3, and v4. Also there is a network that has used the inception module and the ResNet together.
- [[Szegedy et al., 2014, Going Deeper with Convolutions]](https://arxiv.org/abs/1409.4842)

### Using Open-Source Implementation

- We have learned a lot of NNs and ConvNets architectures.
- It turns out that a lot of these NN are difficult to replicated. because there are some details that may not presented on its papers. There are some other reasons like:
  - Learning decay.
  - Parameter tuning.
- A lot of deep learning researchers are open-sourcing their code into Internet on sites like [Github](Github.com).
- Notes on using GitHub:
  - git clone <link>: will download contents of link to your local system, i.e clones repository to hard disk. 
- If you see a research paper and you want to build over it, the first thing you should do is to look for an open source implementation for this paper.
- One advantage of doing this is that you might download the network implementation along with its parameters/weights. The author might have used multiple GPUs and spent some weeks to reach this result and it's right in front of you after you download it. Transfer learning may also be easy to put into practice in such a case. 

### Transfer Learning

- If you are using a specific NN architecture that has been trained before, you can use this pretrained parameters/weights instead of random initialization to solve your problem.
- It can help you boost the performance of the NN.
- The pretrained models might have trained on a large datasets like ImageNet, Ms COCO, or pascal and took a lot of time to learn those parameters/weights with optimized hyperparameters. This can save you a lot of time.
- Let's see an example:
  - Let's say you have a cat classification problem which contains 3 classes: Tigger, Misty and neither.
  - You might not have a lot of data to train a NN on these images.
  - Andrew recommends to go online and download a good NN (implementation + weights), remove the softmax activation layer and put your own one (to classify into just Tigger/Misty/neither). Make the network learn only the new layer while other layer weights are fixed/frozen.
  - Frameworks have options to make the parameters frozen in some layers using `trainable = 0` or `freeze = 1`.
  - One of the tricks that can speed up your training, is to run the pretrained NN without final softmax layer and get an intermediate representation of your images and save them to disk. Then, use this representation to train a shallow NN network. This can save you the time needed to run an image through all the layers.
    - It's like converting your images into vectors.
- Another example:
  - What if you have a lot of pictures for your cats, i.e. what if your training set is big?
  - One thing you can do is to freeze few layers from the beginning of the pretrained network and learn the other weights in the network.
  - Another idea is to throw away the layers that aren't frozen and put your own layers there. Generally, as a rule of thumb, if you have a large amount of data, then you can train upon a large number of layers (and freeze a smaller number). 
- Another example:
  - If you have enough data, you can fine tune all the layers in your pretrained network. However, instead of randomly initialize the parameters, initialize them to the weights downloaded from open source implementation.
- Bottomline: unless you have exceptional amounts of data and computational power, transfer learning is almost always the preferred way to go. 

### Data Augmentation

- Not all deep NNs require insane data sizes, but computer vision problems **definitely** do. When it feels like there isn't enough data for CV problems, data augmentation is a very useful technique to apply. 
- Some data augmentation methods that are used for computer vision tasks includes:
  - Mirroring (use only if whatever we're trying to identify is preserved in the picture after mirroring).
  - Random cropping.
    - The issue with this technique is that you might take a wrong crop.
    - The solution is to make your crops reasonably large subsets of the actual image.
  - Rotation.
  - Shearing (distort by an angle i.e | | => / /).
  - Local warping.
  - Color shifting.
    - For example, we add to R, G, and B some distortions that will make the image identified as the same for the human but is different for the computer.
    - In practice, the added value are pulled from some probability distribution and these shifts are small.
    - Makes your algorithm more robust in changing colors in image, caused by something as simple as a sunlight angle change. 
    - There are an algorithm which is called ***PCA color augmentation*** that decides the shifts needed automatically, so that the overall tint of all the images remains the same.
- Implementing distortions during training:
  - One CPU thread (a thread is a process) is responsible for constantly loading images from the hard disk, while another thread implements training. Often, both threads run in parallel. 
- Data Augmentation has also some hyperparameters (how much colour shifting? how do I 'randomly' crop?). A good place to start is to find an open source data augmentation implementation and then use it, or fine tune these hyperparameters yourself.

### State of Computer Vision

- For a specific problem we may have either little data, or lots of data.
- Nowadays, speech recognition problems for example has a big amount of data, while image recognition (identifying what an image is) has a medium amount of data and object detection (putting a bounding box around a particular object, as part of a larger image) has a small amount of data. 
- If your problem has a large amount of data, researchers are tend to use:
  - Simpler algorithms.
  - Less hand engineering (i.e., they implement more hacks, or workarounds, essentially ensuring that all the features/architecture/components of the problem are well-designed).
- It can be thought of like this. There are two sources of knowledge for an ML problem:
  - data
  - hand engineering
- If you don't have one, people rely on the other, i.e., if you don't have that much data people tend to try more hand engineering for the problem, like choosing a more complex NN architecture. It makes no sense to rely on hand engineering when there's a lot of data out there. 
- Because we haven't got that much data in a lot of computer vision problems, it relies a lot on hand engineering.
- We will see that since the object detection problem has less data, more complex NN architectures/transfer learning techniques will be presented.
- Tips for doing well on benchmarks/winning competitions:
  - Ensembling.
    - Train several networks independently and average their outputs (NOT the weight). Merging down some classifiers.
    - After you decide the best architecture for your problem, initialize some of that randomly and train them independently.
    - This can give you a push by 2%
    - But this will slow down your production by the number of the ensembles. Also it takes more memory as it saves all the models in the memory.
    - People use this in competitions but few use this in a real production.
  - Multi-crop at test time.
    - Run classifier on multiple versions of test versions and average results.
    - There is a technique called 10-crop that uses this: take the image, and 4 different crops of it. Run through classifier. Then, take the mirrored image, and 4 different crops of it. Run through classifier. Eventually, 10 (1+4+1+4) versions of the same image run through the classifier. 
    - This can give you a better result in the production system. Moreover, memory is lesser compared to ensembling (but runtime is still long). 
  - Use open source code
    - Use architectures of networks published in literature.
    - Use open source implementations if possible.
    - Use pretrained models and fine-tune on your dataset.

## Object detection

> Learn how to apply your knowledge of CNNs to one of the toughest but hottest field of computer vision: Object detection.

### Object Localization

- Object detection is one of the areas in which deep learning is doing great in the past two years.

- What are localization and detection?

  - **Image Classification**: 
    - Classify an image to a specific class. The whole image represents one class. We don't want to know exactly where are the object. Usually only one object is presented in the middle of the image.
    - ![](Images/Classification.jpg)
  - **Classification with localization**:
    - Given an image, we want to first draw a bounding box around the object present in that image, and then classify the object within that box to a specific class. Usually only one object is presented in the middle of the image.
    - ![](Images/ClassificationLoc.jpg)
  - **Object detection**:
    - An image can contain more than one object with different classes. Given such an image, we first have to localize each object, and then classify the object as belong to specific classes. 
    - ![](Images/ObjectDetection.png)
  - **Semantic Segmentation**:
    - We want to label each pixel in the image with a category label. Semantic Segmentation doesn't differentiate instances, only care about pixels. It detects no objects just pixels.
    - If there are two objects of the same class is intersected, we won't be able to separate them.
    - ![](Images/SemanticSegmentation.png)
  - **Instance Segmentation**
    - This is like the full problem. Rather than we want to predict the bounding box, we want to know which pixel label but also distinguish them.
    - ![](Images/InstanceSegmentation.png)

- To implement image classification, we use a ConvNet with a Softmax layer attached to the end of it. This is referred to as the standard classification pipeline.

- To implement classification with localization, we again use a ConvNet with a softmax attached to the end of it. But instead of just outputting the classes to which the image can possibly belong (car/pedestrian/motorcycle/none etc.), four more numbers (`bx`, `by`, `bh`, and `bw`) are also produced as outputs of the softmax layer. 

- The purpose of these `b's` is to tell you the location of the class in the image: `bx` and `by` represent the co-ordinates of the centroid of the bounding box, `bh` and `bw` represent height and width respectively. 

- By convention, the top left corner is (0,0), and bottom right is (1,1).

- Defining the target label Y in classification with localization problem (assume that classes 1-4 are pedestrian/car/motorcycle/background):

  - ```
    Y = [
      		Pc				# Probability that an object is present =1 unless class 4 
      		bx				# Bounding box
      		by				# Bounding box
      		bh				# Bounding box
      		bw				# Bounding box
      		c1				# The classes (under the assumption that only one class is present)
      		c2
      		...
    ]
    ```

  - Example (Object is present):

    - ```
      Y = [
        		1		# Object of type car is present
        		0
        		0
        		100
        		100
        		0
        		1
        		0
      ]
      ```

  - Example (When object isn't present):

    - ```
      Y = [
        		0		# Object isn't present
        		?		# ? are all don't cares: we don't care about these values
        		?
        		?
        		?
        		?
        		?
        		?
      ]
      ```

- The loss function for the Y we have created (Example of squared error):

  - ```
    L(y',y) = {
      			(y1'-y1)^2 + (y2'-y2)^2 + ...           if y1 = 1, i.e. object is present
      			(y1'-y1)^2						if y1 = 0, i.e. no object is present 
    		}
    ```

  - Note that if no object is present, we only care about how accurately the network is predicting the probability of existence of an object. 
  - In practice, we could use logistic regression for `Pc` (probability of object being present), log likelihood loss for classes, and squared error for the bounding box.

### Landmark Detection

- In our previous example, we were concerned about `bx`,`by`,`bw` and `bh`. In some CV problems, we don't need to output all four; we just need to output the co-ordinates of important points/features present on the image. Such a problem is called **landmark detection**.

- For example, if you are working in a face recognition problem, you might want to locate some significant points on the face (like corners of the eyes, corners of the mouth, and corners of the nose and so on). This can help in a lot of further applications such as recognizing emotions, Snapchat filters, pose detection, outlining the skeleton a person, etc. 

- Y shape for the face recognition problem that needs to output 64 landmarks:

  - ```
    Y = [
      		THereIsAface				# Probability of whether a face is present (0 or 1)
      		l1x,
      		l1y,
      		....,
      		l64x,
      		l64y
    ]
    ```
- Hint: your labeled data must be consistent. If `l1x,l1y` is the left corner of left eye, then `l1x, l1y` must indicate the left corner of the left eye across **all** images.

### Object Detection

- We will use a ConvNet to solve the object detection problem, using a technique called the sliding windows detection algorithm.
- For example lets say we are working on car object detection.
- Firstly, we will build a training set of labelled closely-cropped car and non-car images. Then, we will train a ConvNet on this training set. The purpose of the ConvNet is to correctly identify a given closely-cropped image.
  - ![](Images/18.png)
- After we finish training of this ConvNet we will then use it with the sliding windows technique.
- Sliding windows detection algorithm:
  1. Decide a rectangle size. This is the window.
  2. Slide the window across the image repeatedly, such that the window covers the entire image. You can vary the horizontal and vertical stride.
  3. For each region enclosed by the window, feed the image into the ConvNet and decide if it is a car or not.
  4. Pick larger/smaller window sizes, and repeat the process from 2 to 3.
- The hope is that some window, somewhere, will output a 1. 
- Disadvantage of sliding window is the computational cost.
- In the era of machine learning before deep learning, people used a hand-engineered linear classifiers that classifies the object and then use the sliding window technique. The use of a linear classifier made it a cheap computation to perform (obviously, the same cannot be expected when a CNN is performing classification). 
- To solve this problem, we can implement the sliding windows with a ***Convolutional approach***.
- One other idea is to compress your deep learning model.

### Convolutional Implementation of Sliding Windows

- To understand convolutional implementation, we first need to understand how to turn FC layers into convolutional layers (predict image class from four classes):
  - ![](Images/19.png)
  - As you can see in the above image, we turned the FC layer into a Conv layer using a convolutional layer, with the width and height of the filter as the width and height of the input.
  - Does the spirit of the FC change? No. Consider the 5x5x16 to 1x1x400 transformation. Each of the 400 nodes is still a linear combination of the 5x5x16 in some fashion, so the basic implementation technique remains the same. 
- **Convolution implementation of sliding windows**:
  - First let's consider that the ConvNet you trained is like this (No FC, all are conv layers):
    - ![](Images/20.png)
  - Say now we have a 16 x 16 x 3 test image that we need to apply the sliding windows algorithm on. To normally implement these, we'd input a 14x14 image (the one represented in blue) to the previous network. Then, we'd slide the window and repeat.
  - With a stride of 2, we would have to perform this process **4** times in total, to get desired output. 
  - The convolution implementation will be as follows:
    - ![](Images/21.png)
  - We have to simply feed the image into the same ConvNet that we trained earlier. 
  - It's easy to see that the top-left cell of the result would represent the output we'd have gotten, if we had run the top-left 14x14x3 segment of the original image. Same goes for top-right, bottom-left and bottom-right cells of the output; they would be the outputs if those corresponding windows were run on the original image.
  - It's more efficient now because instead of running the code 4 times, we just run it once. 
  - Another example would be:
    - ![](Images/22.png)
  - This example has a total of 16 sliding windows that shares the computation together.
  - [[Sermanet et al., 2014, OverFeat: Integrated recognition, localization and detection using convolutional networks]](https://arxiv.org/abs/1312.6229)
- The weakness of the algorithm is that the position of the rectangle may not be so accurate. It may be that none of the rectangles match up with the exact object you want to recognize.
  - ![](Images/23.png)
  - In red is the rectangle we want. In blue, the output we get. Note that even the aspect ratios of the two boxes vary a little. 

### Bounding Box Predictions

- A better algorithm than the one described in the last section is the [YOLO algorithm](https://arxiv.org/abs/1506.02640).

- YOLO stands for *you only look once* and was developed back in 2015.

- Yolo Algorithm:

  - ![](Images/24.png)

  1. Let's say we have an image of 100 X 100.
  2. Place a 3 x 3 grid on the image. For smoother results you should use a finer grid (like 19 x 19 for the 100 x 100).
  3. Apply the classification and localization algorithm (discussed in a previous section) to each section of the grid. To assign coordinates, we will assume that the upper left corner of the grid cell is (0,0) and the bottom right corner is (1,1); the other co-ordinates are assigned accordingly. `bx` and `by` will represent the center point of the object (relative to the grid cell) while `bh` and `bw` will represent the height and width of the object as fractions of the height and width of the grid cell. 
     Note that `bx` and `by` must lie between 0 and 1, but `bh` and `bw` can be greater than 1. Also note that there are alternatives to this mode of parametrization. 
  4. Do everything at once with the convolution sliding window. If Y shape is 1 x 8 (Pc, bx, by, bh, bw, c1, c2, c3) as we discussed before then the output of the 100 x 100 image should be 3 x 3 x 8. Each of the 1 x 1 x 8 that makes up the 3 x 3 x 8 corresponds to one square of the grid. 
  5. If an object spans multiple grid cells, then you assign the object to the grid cell which contains the midpoint of the object. Therefore, only one grid will predict the occurrence of an object. 

- We have a problem if we have found more than one object in one grid cell.

- One of the best advantages that makes the YOLO algorithm popular is that it has a great speed and a Conv net implementation.

- How is YOLO different from other Object detectors?  YOLO uses a single CNN
  network for both classification and localizing the object using bounding boxes.

- In the next sections we will see some ideas that can make the YOLO algorithm better.

### Intersection Over Union

- Intersection Over Union is a function used to evaluate the object detection algorithm.
- It computes `size of intersection/size of the union`. More generally, *IoU* *is a measure of the overlap between two bounding boxes*.
- While IoU is a general measure for overlap between any two bounding boxes, it can be used a performance measure for object localization, if computed between the actual bounding box and the predicted bounding box. 
- For example:
  - ![](Images/25.png)
  - The red is the labeled output and the purple is the predicted output.
  - To compute Intersection Over Union we first compute the union area of the two rectangles which is "the first rectangle + second rectangle" Then compute the intersection area between these two rectangles.
  - Finally `IOU = intersection area / Union area`
- If `IOU >=0.5`, then the prediction is correct. 
- The best value of IoU is 1.
- The higher the IOU the better is the accuracy.

### Non-max Suppression

- One of the problems that the YOLO faces, is that it can detect an object multiple times.
- Non-max Suppression is a way to make sure that YOLO detects each object just once.
- For example:
  - ![](Images/26.png)
  - The grid cells surrounding the one containing the midpoint may report that the car is contained in them, and therefore each car has two or more detections - with multiple probabilities. 
- How Non-max suppression algorithm works:
  1. This algorithm is being described for only one class. Therefore, discard C1,C2,C3 etc so that output looks like `[Pc,bx,by,bh,bw]`.  
  2. Discard all boxes with Pc<=0.6.
  3. Out of all the grid cells that report the presence of an object, pick the box with the largest Pc. 
  4. Now consider the remaining grid cells. If the IoU of this grid cell, with the grid cell obtained in the previous step, is >0.5, then discard that box. (basically high IoU indicates that the boxes are overlapping).
  5. Repeat steps 3 and 4, starting with the box with the next highest probability - until all the boxes have either been chosen (as a prediction) or discarded.
  
- If there are multiple classes/object types `c` you want to detect, you should run the Non-max suppression `c` times, once for every output class.

### Anchor Boxes

- In YOLO, a grid cell can only detect one object. What if a grid cell wants to detect multiple objects?
  - ![](Images/27.png)
  - The midpoint of car and person are in the same grid cell (and are almost the same).
  - In practice this happens rarely.
- The idea of Anchor boxes helps us in solving this issue. Define two anchor boxes, one in landscape and one in perspective mode.
- If Y = `[Pc, bx, by, bh, bw, c1, c2, c3]` Then to use two anchor boxes like this:
  - Y = `[Pc, bx, by, bh, bw, c1, c2, c3, Pc, bx, by, bh, bw, c1, c2, c3]`. The first set of parameters correspond to the first anchor box, and the second to the second. We simply have repeated the one anchor Y.
  - The two anchor boxes you choose should be known as a shape:
    - ![](Images/28.png)
- So previously, each object in training image is assigned to grid cell that contains that object's midpoint.
- In this mode of implementation, we have defined two anchor boxes for each grid cell. Therefore, each object in the training image is assigned to a grid cell that contains object's midpoint, as well as to an anchor box for the grid cell with <u>highest IoU</u>, i.e. the encoding of each object is now (grid cell, anchor box). The anchor box is chosen based on higher IoU.
- Example of data:
  - ![](Images/29.png)
  - In this case, the car resembled anchor 2 more. Moreover, only a car is present in the grid cell- no object matches to anchor box 1. 
- This algorithm fails when:
  - we have more than two objects' midpoints in a grid cell (happens rarely, though).
  - multiple objects in the same grid cell all correspond to the same anchor box
- You may have two or more anchor boxes but you should know their shapes.
  - How do you choose the anchor boxes? People used to just choose them by hand- maybe five or ten anchor box shapes that span a variety of shapes, to cover the types of objects that you detect frequently.
  - You may also use a k-means algorithm on your dataset to specify the shapes of anchor boxes.
- Anchor boxes allow your algorithm to specialize, i.e. easily detects wider images or taller ones.

### YOLO Algorithm

- YOLO is a state-of-the-art object detection model that is fast and accurate.

- Let's sum up and introduce the whole YOLO algorithm, given an example.

- Suppose we need to do object detection for our autonomous driver system. It needs to identify three classes:

  1. Pedestrian.
  2. Car.
  3. Motorcycle.

- We decided to choose two anchor boxes, a taller one and a wide one (i.e. landscape and portrait). 

  - In practice, we may use five or more hand-made anchor boxes, or we may generate them using k-means.

- The output y would be either 3 x 3 x 2 x 8 or 3 x 3 x 16 in this case. Our labeled Y shape will be `[Ny, HeightOfGrid, WidthOfGrid, 16]`, where Ny is number of instances and each row (of size 16) is as follows:

  - `[Pc, bx, by, bh, bw, c1, c2, c3, Pc, bx, by, bh, bw, c1, c2, c3]`

  - An example:
    - ![](Images/30.png)
  - Shape of Y for one image should be `[HeightOfGrid, WidthOfGrid,16]`

- Train the labeled images on a ConvNet. You should receive an output of `[HeightOfGrid, WidthOfGrid,16]` for our case.

- To make predictions, run the ConvNet on an image and run Non-max suppression algorithm for each class you have. In our case, there are 3 classes.
  - This is what the algorithm looks like:
    - For each grid cell, get 2 predicted bounding boxes.
    - Get rid of low probability predictions.
    - For each class, use non-max prediction to generate final predictions (run the algorithm to detect presence of each class). 

  - You could get something like that:
    - ![](Images/31.png)
    - Total number of generated boxes are grid_width * grid_height * no_of_anchors = 3 x 3 x 2
  - By removing the low probability predictions you should have:
    - ![](Images/32.png)
  - Then get the best probability followed by the IOU filtering:
    - ![](Images/33.png)

- YOLO is not very good at detecting smaller objects.

- [YOLO9000 Better, faster, stronger](https://arxiv.org/abs/1612.08242)

  - Summary:

  - ```
    ________________________________________________________________________________________
    Layer (type)                     Output Shape          Param #     Connected to                
    ========================================================================================
    input_1 (InputLayer)             (None, 608, 608, 3)   0                                 
    ________________________________________________________________________________________
    conv2d_1 (Conv2D)                (None, 608, 608, 32)  864         input_1[0][0]         
    ________________________________________________________________________________________
    batch_normalization_1 (BatchNorm (None, 608, 608, 32)  128         conv2d_1[0][0]       
    ________________________________________________________________________________________
    leaky_re_lu_1 (LeakyReLU)        (None, 608, 608, 32)  0     batch_normalization_1[0][0] 
    ________________________________________________________________________________________
    max_pooling2d_1 (MaxPooling2D)   (None, 304, 304, 32)  0           leaky_re_lu_1[0][0]   
    ________________________________________________________________________________________
    conv2d_2 (Conv2D)                (None, 304, 304, 64)  18432       max_pooling2d_1[0][0] 
    ________________________________________________________________________________________
    batch_normalization_2 (BatchNorm (None, 304, 304, 64)  256         conv2d_2[0][0]       
    ________________________________________________________________________________________
    leaky_re_lu_2 (LeakyReLU)        (None, 304, 304, 64)  0     batch_normalization_2[0][0] 
    _______________________________________________________________________________________
    max_pooling2d_2 (MaxPooling2D)   (None, 152, 152, 64)  0           leaky_re_lu_2[0][0]   
    ________________________________________________________________________________________
    conv2d_3 (Conv2D)                (None, 152, 152, 128) 73728       max_pooling2d_2[0][0] 
    ________________________________________________________________________________________
    batch_normalization_3 (BatchNorm (None, 152, 152, 128) 512         conv2d_3[0][0]       
    ________________________________________________________________________________________
    leaky_re_lu_3 (LeakyReLU)        (None, 152, 152, 128) 0     batch_normalization_3[0][0] 
    ________________________________________________________________________________________
    conv2d_4 (Conv2D)                (None, 152, 152, 64)  8192        leaky_re_lu_3[0][0]   
    ________________________________________________________________________________________
    batch_normalization_4 (BatchNorm (None, 152, 152, 64)  256         conv2d_4[0][0]       
    ________________________________________________________________________________________
    leaky_re_lu_4 (LeakyReLU)        (None, 152, 152, 64)  0     batch_normalization_4[0][0] 
    ________________________________________________________________________________________
    conv2d_5 (Conv2D)                (None, 152, 152, 128) 73728       leaky_re_lu_4[0][0]   
    ________________________________________________________________________________________
    batch_normalization_5 (BatchNorm (None, 152, 152, 128) 512         conv2d_5[0][0]       
    ________________________________________________________________________________________
    leaky_re_lu_5 (LeakyReLU)        (None, 152, 152, 128) 0     batch_normalization_5[0][0] 
    ________________________________________________________________________________________
    max_pooling2d_3 (MaxPooling2D)   (None, 76, 76, 128)   0           leaky_re_lu_5[0][0]   
    ________________________________________________________________________________________
    conv2d_6 (Conv2D)                (None, 76, 76, 256)   294912      max_pooling2d_3[0][0] 
    _______________________________________________________________________________________
    batch_normalization_6 (BatchNorm (None, 76, 76, 256)   1024        conv2d_6[0][0]       
    ________________________________________________________________________________________
    leaky_re_lu_6 (LeakyReLU)        (None, 76, 76, 256)   0     batch_normalization_6[0][0] 
    _______________________________________________________________________________________
    conv2d_7 (Conv2D)                (None, 76, 76, 128)   32768       leaky_re_lu_6[0][0]   
    ________________________________________________________________________________________
    batch_normalization_7 (BatchNorm (None, 76, 76, 128)   512         conv2d_7[0][0]       
    _______________________________________________________________________________________
    leaky_re_lu_7 (LeakyReLU)        (None, 76, 76, 128)   0     batch_normalization_7[0][0] 
    ________________________________________________________________________________________
    conv2d_8 (Conv2D)                (None, 76, 76, 256)   294912      leaky_re_lu_7[0][0]   
    ________________________________________________________________________________________
    batch_normalization_8 (BatchNorm (None, 76, 76, 256)   1024        conv2d_8[0][0]       
    ________________________________________________________________________________________
    leaky_re_lu_8 (LeakyReLU)        (None, 76, 76, 256)   0     batch_normalization_8[0][0] 
    ________________________________________________________________________________________
    max_pooling2d_4 (MaxPooling2D)   (None, 38, 38, 256)   0           leaky_re_lu_8[0][0]   
    ________________________________________________________________________________________
    conv2d_9 (Conv2D)                (None, 38, 38, 512)   1179648     max_pooling2d_4[0][0] 
    ________________________________________________________________________________________
    batch_normalization_9 (BatchNorm (None, 38, 38, 512)   2048        conv2d_9[0][0]       
    ________________________________________________________________________________________
    leaky_re_lu_9 (LeakyReLU)        (None, 38, 38, 512)   0     batch_normalization_9[0][0] 
    ________________________________________________________________________________________
    conv2d_10 (Conv2D)               (None, 38, 38, 256)   131072      leaky_re_lu_9[0][0]   
    ________________________________________________________________________________________
    batch_normalization_10 (BatchNor (None, 38, 38, 256)   1024        conv2d_10[0][0]       
    ________________________________________________________________________________________
    leaky_re_lu_10 (LeakyReLU)       (None, 38, 38, 256)   0    batch_normalization_10[0][0]
    ________________________________________________________________________________________
    conv2d_11 (Conv2D)               (None, 38, 38, 512)   1179648    leaky_re_lu_10[0][0]   
    ________________________________________________________________________________________
    batch_normalization_11 (BatchNor (None, 38, 38, 512)   2048        conv2d_11[0][0]       
    ________________________________________________________________________________________
    leaky_re_lu_11 (LeakyReLU)       (None, 38, 38, 512)   0    batch_normalization_11[0][0]
    _______________________________________________________________________________________
    conv2d_12 (Conv2D)               (None, 38, 38, 256)   131072      leaky_re_lu_11[0][0] 
    ________________________________________________________________________________________
    batch_normalization_12 (BatchNor (None, 38, 38, 256)   1024        conv2d_12[0][0]       
    ________________________________________________________________________________________
    leaky_re_lu_12 (LeakyReLU)       (None, 38, 38, 256)   0   batch_normalization_12[0][0]
    ________________________________________________________________________________________
    conv2d_13 (Conv2D)               (None, 38, 38, 512)   1179648     leaky_re_lu_12[0][0] 
    ________________________________________________________________________________________
    batch_normalization_13 (BatchNor (None, 38, 38, 512)   2048        conv2d_13[0][0]       
    ________________________________________________________________________________________
    leaky_re_lu_13 (LeakyReLU)       (None, 38, 38, 512)   0    batch_normalization_13[0][0]
    ________________________________________________________________________________________
    max_pooling2d_5 (MaxPooling2D)   (None, 19, 19, 512)   0           leaky_re_lu_13[0][0] 
    _______________________________________________________________________________________
    conv2d_14 (Conv2D)               (None, 19, 19, 1024)  4718592     max_pooling2d_5[0][0] 
    ________________________________________________________________________________________
    batch_normalization_14 (BatchNor (None, 19, 19, 1024)  4096        conv2d_14[0][0]       
    ________________________________________________________________________________________
    leaky_re_lu_14 (LeakyReLU)       (None, 19, 19, 1024)  0    batch_normalization_14[0][0]
    ________________________________________________________________________________________
    conv2d_15 (Conv2D)               (None, 19, 19, 512)   524288      leaky_re_lu_14[0][0] 
    ________________________________________________________________________________________
    batch_normalization_15 (BatchNor (None, 19, 19, 512)   2048        conv2d_15[0][0]       
    ________________________________________________________________________________________
    leaky_re_lu_15 (LeakyReLU)       (None, 19, 19, 512)   0    batch_normalization_15[0][0]
    ________________________________________________________________________________________
    conv2d_16 (Conv2D)               (None, 19, 19, 1024)  4718592     leaky_re_lu_15[0][0] 
    ________________________________________________________________________________________
    batch_normalization_16 (BatchNor (None, 19, 19, 1024)  4096        conv2d_16[0][0]       
    ________________________________________________________________________________________
    leaky_re_lu_16 (LeakyReLU)       (None, 19, 19, 1024)  0    batch_normalization_16[0][0]
    ________________________________________________________________________________________
    conv2d_17 (Conv2D)               (None, 19, 19, 512)   524288      leaky_re_lu_16[0][0] 
    ________________________________________________________________________________________
    batch_normalization_17 (BatchNor (None, 19, 19, 512)   2048        conv2d_17[0][0]       
    ________________________________________________________________________________________
    leaky_re_lu_17 (LeakyReLU)       (None, 19, 19, 512)   0    batch_normalization_17[0][0]
    _______________________________________________________________________________________
    conv2d_18 (Conv2D)               (None, 19, 19, 1024)  4718592     leaky_re_lu_17[0][0] 
    ________________________________________________________________________________________
    batch_normalization_18 (BatchNor (None, 19, 19, 1024)  4096        conv2d_18[0][0]       
    ________________________________________________________________________________________
    leaky_re_lu_18 (LeakyReLU)       (None, 19, 19, 1024)  0    batch_normalization_18[0][0]
    ________________________________________________________________________________________
    conv2d_19 (Conv2D)               (None, 19, 19, 1024)  9437184     leaky_re_lu_18[0][0] 
    ________________________________________________________________________________________
    batch_normalization_19 (BatchNor (None, 19, 19, 1024)  4096        conv2d_19[0][0]       
    ________________________________________________________________________________________
    conv2d_21 (Conv2D)               (None, 38, 38, 64)    32768       leaky_re_lu_13[0][0]
    ________________________________________________________________________________________
    leaky_re_lu_19 (LeakyReLU)       (None, 19, 19, 1024)  0    batch_normalization_19[0][0]
    ________________________________________________________________________________________
    batch_normalization_21 (BatchNor (None, 38, 38, 64)    256         conv2d_21[0][0]       
    ________________________________________________________________________________________
    conv2d_20 (Conv2D)               (None, 19, 19, 1024)  9437184     leaky_re_lu_19[0][0]
    ________________________________________________________________________________________
    leaky_re_lu_21 (LeakyReLU)       (None, 38, 38, 64)    0    batch_normalization_21[0][0]
    ________________________________________________________________________________________
    batch_normalization_20 (BatchNor (None, 19, 19, 1024)  4096        conv2d_20[0][0]       
    ________________________________________________________________________________________
    space_to_depth_x2 (Lambda)       (None, 19, 19, 256)   0           leaky_re_lu_21[0][0] 
    ________________________________________________________________________________________
    leaky_re_lu_20 (LeakyReLU)       (None, 19, 19, 1024)  0    batch_normalization_20[0][0]
    ________________________________________________________________________________________
    concatenate_1 (Concatenate)      (None, 19, 19, 1280)  0         space_to_depth_x2[0][0] 
                                                                      leaky_re_lu_20[0][0] 
    ________________________________________________________________________________________
    conv2d_22 (Conv2D)               (None, 19, 19, 1024)  11796480    concatenate_1[0][0]   
    ________________________________________________________________________________________
    batch_normalization_22 (BatchNor (None, 19, 19, 1024)  4096        conv2d_22[0][0]       
    ________________________________________________________________________________________
    leaky_re_lu_22 (LeakyReLU)       (None, 19, 19, 1024)  0    batch_normalization_22[0][0]
    ________________________________________________________________________________________
    conv2d_23 (Conv2D)               (None, 19, 19, 425)   435625      leaky_re_lu_22[0][0] 
    ===============================================================================================
    Total params: 50,983,561
    Trainable params: 50,962,889
    Non-trainable params: 20,672
    _______________________________________________________________________________________________
    ```

- You can find implementations for YOLO here:

  - https://github.com/allanzelener/YAD2K
  - https://github.com/thtrieu/darkflow
  - https://pjreddie.com/darknet/yolo/

### Region Proposals (R-CNN)

- R-CNN is an algorithm that also makes an object detection.

- Yolo tells that its faster:

  - > Our model has several advantages over classifier-based systems. It looks at the whole image at test time so its predictions are informed by global context in the image. It also makes predictions with a single network evaluation unlike systems like R-CNN which require thousands for a single image. This makes it extremely fast, more than 1000x faster than R-CNN and 100x faster than Fast R-CNN. See our paper for more details on the full system.

- But one of the downsides of YOLO that it process a lot of areas where no objects are present.

- **R-CNN** stands for regions with Conv Nets.

- R-CNN tries to pick a few windows and run a Conv net (your confident classifier) on top of them.

- The algorithm R-CNN uses to pick windows is called a segmentation algorithm. Outputs something like this:

  - ![](Images/34.png)

- If for example the segmentation algorithm produces 2000 blob then we should run our classifier/CNN on top of these blobs.

- There has been a lot of work regarding R-CNN tries to make it faster:

  - R-CNN:
    - Propose regions. Classify proposed regions one at a time. Output label + bounding box.
    - Downside is that its slow.
    - [[Girshik et. al, 2013. Rich feature hierarchies for accurate object detection and semantic segmentation]](https://arxiv.org/abs/1311.2524)
  - Fast R-CNN:
    - Propose regions. Use convolution implementation of sliding windows to classify all the proposed regions.
    - [[Girshik, 2015. Fast R-CNN]](https://arxiv.org/abs/1504.08083)
  - Faster R-CNN:
    - Use convolutional network to propose regions.
    - [[Ren et. al, 2016. Faster R-CNN: Towards real-time object detection with region proposal networks]](https://arxiv.org/abs/1506.01497)
  - Mask R-CNN:
    - https://arxiv.org/abs/1703.06870

- Most of the implementation of faster R-CNN are still slower than YOLO.

- Andrew Ng thinks that the idea behind YOLO is better than R-CNN because you are able to do all the things in just one time instead of two times.

- Other algorithms that uses one shot to get the output includes **SSD** and **MultiBox**.

  - [[Wei Liu, et. al 2015 SSD: Single Shot MultiBox Detector]](https://arxiv.org/abs/1512.02325)

- **R-FCN** is similar to Faster R-CNN but more efficient.

  - [[Jifeng Dai, et. al 2016 R-FCN: Object Detection via Region-based Fully Convolutional Networks ]](https://arxiv.org/abs/1605.06409)

## Special applications: Face recognition & Neural style transfer

> Discover how CNNs can be applied to multiple fields, including art generation and face recognition. Implement your own algorithm to generate art and recognize faces!

### Face Recognition

#### What is face recognition?

- Face recognition system identifies a person's face. It can work on both images or videos.
- **<u>Liveness detection</u>** within a video face recognition system, prevents the network from identifying a face in an image (i.e., it would be able to distinguish the presence of a real human being, versus an image of them being shown to a camera). It can be learned by supervised deep learning using a dataset for live human versus non-live human. 
- However, we will stick to face recognition. 
- Face verification vs. face recognition:
  - Verification (1 : 1 problem):
    - Input: image, name/ID. 
    - Output: whether the input image is that of the claimed person
    - "is this the claimed person?"
  - Recognition (1 : k problem):
    - Has a database of K persons
    - Get an input image
    - Output ID if the image is any of the K persons (or not recognized)
    - "who is this person?"
- We can use a face verification system to make a face recognition system. 
- Since the accuracy of the recognition system depends on whether all K persons are correctly identified, its accuracy will be less than that of the verification system.
- Therefore, the accuracy of the verification system has to be high (around 99.9% or more) to be used accurately within a recognition system.

#### One Shot Learning

- One of the face recognition challenges is to solve one shot learning problem.
- One Shot Learning is when a recognition system is able to recognize a person after learning from one image. However, we know that deep learning usually doesn't work well with such a small amount of data. (Therefore, training a softmax, etc. is useless).
- Instead, we will learn a **similarity function**:
  - d( **img1**, **img2** ) = degree of difference between the two kimages.
  - We want d result to be low in case of images of the same person (i.e. similar images, low d).
  - We use tau T as a threshold for d:
    - If d( **img1**, **img2** ) <= T    Then the faces are the same.
    tau is a hyperparameter.
- Similarity function helps us solve the one shot learning problem. Also, it's robust to new inputs.

#### Siamese Network

- We will implement the similarity function using a type of NNs called Siamese Network, in which we can pass multiple inputs to two or more networks with the same architecture and parameters.
- Siamese network architecture is as below:
  - ![](Images/35.png)
  - We train a ConvNet so that it takes an image and outputs a vector of 128 numbers, which essentially represents the **encoding** of the input image. The encoding of image x1 is f(x1). 
  - How are the parameters of the network learned? The parameters are learned (i.e. the network learns to assign encoding, such that) such that the loss function is small if the given pair of images are of the same person, and large if they are of different people.
  - The loss function will be `d(x1, x2) = || f(x1) - f(x2) ||^2`
  - If `X1`, `X2` are the same person, we want d to be low. If they are different persons, we want d to be high.
  - [[Taigman et. al., 2014. DeepFace closing the gap to human level performance]](https://www.cv-foundation.org/openaccess/content_cvpr_2014/html/Taigman_DeepFace_Closing_the_2014_CVPR_paper.html)

#### Triplet Loss

- Triplet Loss is one of the loss functions that we can use to practically implement differences in a Siamese network.
- Our learning objective in the triplet loss function is to get the distance between an **Anchor** image and a **positive** or a **negative** image.
  - Positive means same person, while negative means different person.
- The triplet name came from that we are comparing an anchor A with a positive P and a negative N image (there are 3 images involved at a time).
- Therefore, we want:
  - Positive distance to be less than negative distance
  - `||f(A) - f(P)||^2  <= ||f(A) - f(N)||^2` or `d(A,P) <= d(A,N)`
  - Then
  - `||f(A) - f(P)||^2  - ||f(A) - f(N)||^2 <= 0`
  - Note that outputting the encoding as 0/same value for all the images (trivial o/p) also satisfies this condition. To make sure that the NN doesn't get away with such an output: 
  - `||f(A) - f(P)||^2  - ||f(A) - f(N)||^2 <= -alpha`
    - Alpha is a small number. Sometimes it's called the margin.
    - Having the margin also ensures that the quantities differ sufficiently from each other, instead of one being just greater than another. 
  - Then
  - `||f(A) - f(P)||^2  - ||f(A) - f(N)||^2 + alpha <= 0`
- Final loss function:
  - Given 3 images (A, P, N)
  - `L(A, P, N) = max (||f(A) - f(P)||^2  - ||f(A) - f(N)||^2 + alpha , 0)`
  - `J = Sum(L(A[i], P[i], N[i]) , i)` for all triplets of images. 
  - Note that if the first term of the max function is greater than 0, then it will be included in the loss function, and hence will end up being minimised along the way.
- Dataset should be big enough, because you need multiple images of the same person in your dataset; then only you can get some triplets out of your dataset. 
- Choosing the triplets A, P, N:
  - During training if A, P, N are chosen randomly (subject to A and P are the same and A and N aren't the same) then one of the problems this constraint is easily satisfied 
    - `d(A, P) + alpha <= d (A, N)` 
    - So the NN wont learn much
  - What we want to do is choose triplets that are **hard** to train on.
    - So for all the triplets we want this to be satisfied:
    - `d(A, P) + alpha <= d (A, N)`
    - This can be achieved by for example same poses!
    - Find more at the paper.
- Details are in this paper [[Schroff et al.,2015, FaceNet: A unified embedding for face recognition and clustering]](https://arxiv.org/abs/1503.03832)
- Commercial recognition systems are trained on a large datasets like 10/100 million images.
- There are a lot of pretrained models and parameters online for face recognition.

#### Face Verification and Binary Classification

- Triplet loss is one way to learn the parameters of a conv net for face recognition there's another way to learn these parameters as a straight binary classification problem.
- Learning the similarity function another way:
  - ![](Images/36.png)
  - The final layer is a sigmoid layer.
  - `Y' = wi * Sigmoid ( f(x(i)) - f(x(j)) ) + b` where the subtraction is the Manhattan distance between f(x(i)) and f(x(j))
  - Some other similarities can be Euclidean and Ki square similarity.
  - The NN here is Siamese means the top and bottom convs has the same parameters.
- The paper for this work: [[Taigman et. al., 2014. DeepFace closing the gap to human level performance]](https://www.cv-foundation.org/openaccess/content_cvpr_2014/html/Taigman_DeepFace_Closing_the_2014_CVPR_paper.html)
- A good performance/deployment trick:
  - Pre-compute all the images that you are using as a comparison to the vector f(x(j))
  - When a new image that needs to be compared, get its vector f(x(i)) then put it with all the pre computed vectors and pass it to the sigmoid function.
- This version works quite as well as the triplet loss function.
- Available implementations for face recognition using deep learning includes:
  - [Openface](https://cmusatyalab.github.io/openface/)
  - [FaceNet](https://github.com/davidsandberg/facenet)
  - [DeepFace](https://github.com/RiweiChen/DeepFace)

### Neural Style Transfer

#### What is neural style transfer?

- Neural style transfer is one of the application of Conv nets.
- Neural style transfer takes a content image `C` and a style image `S` and generates the content image `G` with the style of style image.
- ![](Images/37.png)
- In order to implement this you need to look at the features extracted by the Conv net at the shallower and deeper layers.
- It uses a previously trained convolutional network like VGG, and builds on top of that. The idea of using a network trained on a different task and applying it to a new task is called transfer learning.

#### What are deep ConvNets learning?

- Visualizing what a deep network is learning:
  - Given this AlexNet like Conv net:
    - ![](Images/38.png)
  - Pick a unit in layer l. Find the nine image patches that maximize the unit's activation. 
    - Notice that a hidden unit in layer one will see relatively small portion of NN, so if you plotted it it will match a small image in the shallower layers while it will get larger image in deeper layers.
  - Repeat for other units and layers.
  - It turns out that layer 1 are learning the low level representations like colors and edges.
- You will find out that each layer are learning more complex representations.
  - ![](Images/39.png)
- The first layer was created using the weights of the first layer. Other images are generated using the receptive field in the image that triggered the neuron to be max.
- [[Zeiler and Fergus., 2013, Visualizing and understanding convolutional networks]](https://arxiv.org/abs/1311.2901)
- A good explanation on how to get **receptive field** given a layer:
  - ![](Images/receptiveField.png)
  - From [A guide to receptive field arithmetic for Convolutional Neural Networks](https://medium.com/@nikasa1889/a-guide-to-receptive-field-arithmetic-for-convolutional-neural-networks-e0f514068807)

#### Cost Function

- We will define a cost function for the generated image that measures how good it is.
- Give a content image C, a style image S, and a generated image G:
  - `J(G) = alpha * J(C,G) + beta * J(S,G)`
  - `J(C, G)` measures how similar is the generated image to the Content image.
  - `J(S, G)` measures how similar is the generated image to the Style image.
  - alpha and beta are relative weighting to the similarity and these are hyperparameters.
- Find the generated image G:
  1. Initiate G randomly
     - For example G: 100 X 100 X 3
  2. Use gradient descent to minimize `J(G)`
     - `G = G - dG`  We compute the gradient image and use gradient decent to minimize the cost function.
- The iterations might be as following image:
  - To Generate this:
    - ![](Images/40.png)
  - You will go through this:
    - ![](Images/41.png)

#### Content Cost Function

- In the previous section we showed that we need a cost function for the content image and the style image to measure how similar is them to each other.
- Say you use hidden layer `l` to compute content cost. 
  - If we choose `l` to be small (like layer 1), we will force the network to get similar output to the original content image.
  - In practice `l` is not too shallow and not too deep but in the middle.
- Use pre-trained ConvNet. (E.g., VGG network)
- Let `a(c)[l]` and `a(G)[l]` be the activation of layer `l` on the images.
- If `a(c)[l]` and `a(G)[l]` are similar then they will have the same content
  - `J(C, G) at a layer l = 1/2 || a(c)[l] - a(G)[l] ||^2`

#### Style Cost Function

- Meaning of the ***style*** of an image:
  - Say you are using layer l's activation to measure ***style***.
  - Define style as correlation between **activations** across **channels**. 
    - That means given an activation like this:
      - ![](Images/42.png)
    - How correlate is the orange channel with the yellow channel?
    - Correlated means if a value appeared in a specific channel a specific value will appear too (Depends on each other).
    - Uncorrelated means if a value appeared in a specific channel doesn't mean that another value will appear (Not depend on each other)
  - The correlation tells you how a components might occur or not occur together in the same image.
- The correlation of style image channels should appear in the generated image channels.
- Style matrix (Gram matrix):
  - Let `a(l)[i, j, k]` be the activation at l with `(i=H, j=W, k=C)`
  - Also `G(l)(s)` is matrix of shape `nc(l) x nc(l)`
    - We call this matrix style matrix or Gram matrix.
    - In this matrix each cell will tell us how correlated is a channel to another channel.
  - To populate the matrix we use these equations to compute style matrix of the style image and the generated image.
    - ![](Images/43.png)
    - As it appears its the sum of the multiplication of each member in the matrix.
- To compute gram matrix efficiently:
  - Reshape activation from H X W X C to HW X C
  - Name the reshaped activation F.
  - `G[l] = F * F.T`
- Finally the cost function will be as following:
  - `J(S, G) at layer l = (1/ 2 * H * W * C) || G(l)(s) - G(l)(G) ||`
- And if you have used it from some layers
  - `J(S, G) = Sum (lamda[l]*J(S, G)[l], for all layers)`
- Steps to be made if you want to create a tensorflow model for neural style transfer:
  1. Create an Interactive Session.
  2. Load the content image.
  3. Load the style image
  4. Randomly initialize the image to be generated
  5. Load the VGG16 model
  6. Build the TensorFlow graph:
     - Run the content image through the VGG16 model and compute the content cost
     - Run the style image through the VGG16 model and compute the style cost
     - Compute the total cost
     - Define the optimizer and the learning rate
  7. Initialize the TensorFlow graph and run it for a large number of iterations, updating the generated image at every step.

#### 1D and 3D Generalizations

- So far we have used the Conv nets for images which are 2D.
- Conv nets can work with 1D and 3D data as well.
- An example of 1D convolution:
  - Input shape (14, 1)
  - Applying 16 filters with F = 5 , S = 1
  - Output shape will be 10 X 16
  - Applying 32 filters with F = 5, S = 1
  - Output shape will be 6 X 32
- The general equation `(N - F)/S + 1` can be applied here but here it gives a vector rather than a 2D matrix.
- 1D data comes from a lot of resources such as waves, sounds, heartbeat signals. 
- In most of the applications that uses 1D data we use Recurrent Neural Network RNN.
- 3D data also are available in some applications like CT scan:
  - ![](Images/44.png)
- Example of 3D convolution:
  - Input shape (14, 14,14, 1)
  - Applying 16 filters with F = 5 , S = 1
  - Output shape (10, 10, 10, 16)
  - Applying 32 filters with F = 5, S = 1
  - Output shape will be (6, 6, 6, 32)

## Extras

### Keras

- Keras is a high-level neural networks API (programming framework), written in Python and capable of running on top of several lower-level frameworks including TensorFlow, Theano, and CNTK.
- Keras was developed to enable deep learning engineers to build and experiment with different models very quickly.
- Just as TensorFlow is a higher-level framework than Python, Keras is an even higher-level framework and provides additional abstractions.
- Keras will work fine for many common models.
- Layers in Keras:
  - Dense (Fully connected layers).
    - A linear function followed by a non linear function.
  - Convolutional layer.
  - Pooling layer.
  - Normalisation layer.
    - A batch normalization layer.
  - Flatten layer
    - Flatten a matrix into vector.
  - Activation layer
    - Different activations include: relu, tanh, sigmoid, and softmax.
- To train and test a model in Keras there are four steps:
  1. Create the model.
  2. Compile the model by calling `model.compile(optimizer = "...", loss = "...", metrics = ["accuracy"])`
  3. Train the model on train data by calling `model.fit(x = ..., y = ..., epochs = ..., batch_size = ...)`
     - You can add a validation set while training too.
  4. Test the model on test data by calling `model.evaluate(x = ..., y = ...)`
- Summarize of step in Keras: Create->Compile->Fit/Train->Evaluate/Test
- `Model.summary()` gives a lot of useful informations regarding your model including each layers inputs, outputs, and number of parameters at each layer.
- To choose the Keras backend you should go to `$HOME/.keras/keras.json` and change the file to the desired backend like Theano or Tensorflow or whatever backend you want.
- After you create the model you can run it in a tensorflow session without compiling, training, and testing capabilities.
- You can save your model with `model_save` and load your model using `model_load ` This will save your whole trained model to disk with the trained weights.








<br><br>
<br><br>
These Notes were made by [Mahmoud Badry](mailto:mma18@fayoum.edu.eg) @2017
