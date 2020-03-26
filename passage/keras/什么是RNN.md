---
typora-copy-images-to: ../../image/Keras
typora-root-url: ../keras
---

## 什么是RNN

RNN (Recurrent Neural Network)是一种应用于序列建模的神经网络。这里每一层不仅输出给下一层，同时还输出一个隐含状态，给当前层在处理下一个样本时使用。

![image-20200326150459410](/../../image/Keras/RNN.png)

循环神经网络的这种结构使得它适合处理前后有依赖关系数据样本。