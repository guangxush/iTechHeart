## CNN基本概念与计算方法
CNN也叫convnet，中文名称为卷积神经网络，是计算机视觉领域常用的一种深度学习模型。本博客主要介绍CNN的原理和计算方法，然后用Keras搭建两个简单的CNN模型。
![CNN结构.png](https://github.com/guangxush/iTechHeart/blob/master/image/CNN/CNN1.png)
上图是CNN的卷积操作，图中左侧是输入一个图像为7\*7的RGB图像，所以通道数是3，记为7\*7\*3，这里采用一个3*3的卷积核对图像进行卷积操作，卷积核的通道是3，记为3\*3\*3，滑动的步长为2，原来的图片对周围进行pad为1的操作，这样W0这个卷积核分别对三个通道的图片进行卷积，卷积完成之后相加，得到了Output Volume下面第一个图左上角为6的结果；
详细的计算方法是Input Volume第一个蓝色的图与Filter W0第一个红色图进行点乘（0\*-1+0\*1+0\*0+...以此类推）得到4，第二个蓝色的图与第二个蓝色图点乘得到0，第三个蓝色的图与第三个红色的图点乘得到1，最后加上bias的1得到6这个数字，其余的以此类推，得到Output Volume第一个绿色矩形框，下面用Filter W1进行同样的卷积操作，得到第二个绿色框。

卷积层输出大小计算方式：
- 输入图片大小 W×W
- Filter大小 F×F
- 步长 S
- padding的像素数 P
输出N = (W − F + 2P )/S+1

## 使用Keras搭建一个二维的卷积神经网络
我们使用keras对MNIST图像数据集进行二维卷积从操作，代码如下：
```python
    model = Sequential()
    model.add(Conv2D(32, kernel_size=(3, 3),
                     activation='relu',
                     input_shape=input_shape))
    model.add(Conv2D(64, (3, 3), activation='relu'))
    model.add(MaxPooling2D(pool_size=(2, 2)))
    model.add(Dropout(0.25))
    model.add(Flatten())
    model.add(Dense(128, activation='relu'))
    model.add(Dropout(0.5))
    model.add(Dense(num_classes, activation='softmax'))
    model.summary()
```
输入的shape是：28*28*1（黑白图片，通道为1，长宽都为28）

这里```Conv2D(32, kernel_size=(3, 3),
                     activation='relu',
                     input_shape=input_shape)```
使用2维卷积核，卷积核大小是3\*3，默认步长是1，所以conv2d_1输出的图片为32-3+1（原图长度-卷积核大小+1），周围没有padding，所以=26\*26；又因为有32个卷积核，所以是26\*26\*32, 参数Param的计算方式为（3\*3+1）\*32=320个，1为偏执；



第二层```Conv2D(64, (3, 3), activation='relu')```，64个卷积核，大小是3\*3\*64，根据上面的计算方法得到输出为24\*24\*64，参数为：（3\*3\*32+1）\*64=18496

第三层```MaxPooling2D(pool_size=(2, 2))```, 池化层，卷积核大小是2*2*1，对24\*24\*64的进行池化，得到12\*12\*64（原来的2*2图像变成了1\*1），这里默认步长是2，（官方解释：If None, it will default to `pool_size`.）

后面的```Flatten()```,是把12\*12\*64变成1维的=9216，剩下的全连接层不再赘述。

具体的参数和输出如下图：
![image.png](https://github.com/guangxush/iTechHeart/blob/master/image/CNN/CNN2.png)


## 使用Keras搭建一个一维的卷积神经网络
在NLP任务中，经常会使用到1维卷积，我们这里对一个二维数据集做1维卷积
代码如下：
```python
# 编码层
    encoded_1 = Conv1D(filters=16, kernel_size=2, padding='same', activation='relu', strides=1,
                       name='encoder_1')(data_input)
    encoded_2 = MaxPooling1D(pool_size=2, name='encoder_2')(encoded_1)
    encoder_3 = Flatten(name='encoder_3')(encoded_2)
    encoded_4 = Dense(10, activation='relu', name='encoded_4')(encoder_3)
    encoded_5 = Dense(encoding_dim, name='encoded_5')(encoded_4)
```
取了其中一部分代码
输入特征的维度是：
(105, 4, 1)
(44, 4, 1)
这里要注意对数据进行reshape, 因为Conv1D接收的输入是3维的。
详细的参数如下：
![image.png](https://github.com/guangxush/iTechHeart/blob/master/image/CNN/CNN3.png)

第一层卷积：``` Conv1D(filters=16, kernel_size=2, padding='same', activation='relu', strides=1,
                       name='encoder_1')```
卷积核大小是2*1，默认写成2，卷积核数量是16，步长为1，增加了padding，所以第一层得到4*16的大小。

第二层池化层，```MaxPooling1D(pool_size=2, name='encoder_2')```,得到2*16的，因为步长为2。
剩下的参数可以自行计算。

## 完整源码

[二维卷积代码](https://github.com/guangxush/AutoEncoders/blob/master/test.py)
[一维卷积代码](https://github.com/guangxush/AutoEncoders/blob/master/base_cnn.py)
