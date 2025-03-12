# Residual Networks (ResNet) - Deep Learning - GeeksforGeeks
- URL: https://www.geeksforgeeks.org/residual-networks-resnet-deep-learning/
- Added At: 2025-03-12 03:05:16
- [Link To Text](2025-03-12-residual-networks-(resnet)---deep-learning---geeksforgeeks_raw.md)

## TL;DR
ResNet是一种深度学习架构，通过残差块和跳跃连接解决梯度消失问题，使得网络可以更深且训练更有效。它通过学习残差映射简化了学习过程，并在多个数据集上取得了优异的性能，对图像分类和目标检测等任务产生了重大影响。

## Summary
1. **Residual Networks (ResNet) 概述**
   - ResNet 是一种深度学习架构，由微软研究院在2015年提出，用于解决深度神经网络中的梯度消失或爆炸问题。
   - 通过引入残差块（Residual Blocks）和跳跃连接（skip connections）来构建网络，允许网络学习残差映射而非直接学习映射。

2. **ResNet 的原理**
   - 网络不是学习复杂的映射H(x)，而是学习残差F(x) = H(x) - x，这通常更小且更容易学习。
   - 网络通过跳跃连接将残差F(x)与输入x结合，形成H(x) = F(x) + x。

3. **跳跃连接的好处**
   - 如果任何层降低了网络性能，可以通过正则化跳过这些层。
   - 有助于训练非常深的神经网络，避免了梯度消失或爆炸的问题，使得训练更快，并减少了梯度变得过小的问题。

4. **ResNet 架构**
   - 使用34层的网络架构，灵感来源于VGG-19，并添加了跳跃连接。

5. **ResNet 实现**
   - 利用Tensorflow和Keras API，可以从头开始设计ResNet架构，包括残差块。
   - 使用CIFAR-10数据集进行训练和测试，包含60,000张32×32彩色图像，分为10个类别。

6. **ResNet 训练和测试**
   - 定义了学习率调整函数`lr_schedule`，根据训练周期逐步减小学习率。
   - 定义了`resnet_layer`函数，创建ResNet层，包括卷积、可选的批量归一化和激活函数。
   - 实现了ResNet V1和V2架构，并进行了训练和测试。

7. **ResNet 结果**
   - 在ImageNet数据集上，152层的ResNet比VGG19更深，但参数更少，错误率更低。
   - ResNet在ILSVRC 2015比赛中获胜，错误率仅为3.7%，在COCO目标检测数据集上也有显著改进。

8. **ResNet 的影响**
   - ResNet通过引入跳跃连接，解决了梯度消失问题，使得模型可以更深、更准确，改善了图像分类和目标检测等任务的性能。
