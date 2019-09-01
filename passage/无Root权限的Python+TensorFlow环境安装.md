### 一、248机器Python3源码安装
1.  ssh之后到根目录（例如```/home/shgx```）[shgx为用户名]，下载指定版本的Python3源码：```wget https://www.python.org/ftp/python/3.5.6/Python-3.5.6.tgz```
2. 解压文件```tar -xvf Python-3.5.6.tgz```
3. 在根目录下（```/home/shgx```）创建如下路径（```mkdir -p bin/Python3```）,使用```pwd```检查Python3下的路径为```/home/shgx/bin/Python3```
4. 打开刚刚解压的文件目录```cd /home/shgx/Python-3.5.6/```（下面的步骤5.6.7都在此目录下执行）
5. 执行```./configure --prefix=/home/shgx/bin/Python3```进行检测
6. 执行```make```进行编译
7. 执行```make install``` 进行安装
8. 打开```cd /home/shgx/bin/Python3/bin```执行```python3```以及```pip3```或者```/pip3```查看是否运行成功，
8. 成功之后在根目录下（```/home/shgx```）创建软链接
```ln -s /home/shgx/bin/Python3/bin/python3 python3```
9. 这样在你的根目录下就可以执行```python3```了

### 二、248机器安装GPU-TenSorFlow
1. 可以通过上面创建软连接的方式同样创建pip3的软连接，或者进入直接调用目录```/home/shgx/bin/Python3/bin/pip3```进行安装
2. 安装GPU版本的TensorFlow 
``` /home/shgx/bin/Python3/bin/pip3  install tensorflow-gpu==1.2```
3. 安装Keras```/home/shgx/bin/Python3/bin/pip3  install keras==2.1.5```
4. 以此类推...可以安装其他库

### 三、 211机器源码安装TensorFlow
1. 确认安装了Python3(我的安装路径为```/home/shgx/Python3.5```)以及```numpy、wheel ```
2. 查看CUDA版本,并记录版本为```7.1.2```
```
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
#define CUDNN_MAJOR 7
#define CUDNN_MINOR 1
#define CUDNN_PATCHLEVEL 2
--
#define CUDNN_VERSION    (CUDNN_MAJOR * 1000 + CUDNN_MINOR * 100 + CUDNN_PATCHLEVEL)
#include "driver_types.h"
```
3.在本地目录下创建临时文件夹如：```mkdir -p tmp/tensorflow_pkg```
4. 下载tensorflow源码```git clone https://github.com/tensorflow/tensorflow```
5. 以上是准备工作，下面开始安装，首先进入tensorflow文件夹```cd tensorflow```，切换分支```git checkout r1.6```
6. 执行```./configure ```进行检测,configure成功后，依然还在tensorflow的根目录下输入以下命令（Yes or No可根据你的需要进行选择，其中需要填写CUDA版本）
```
[shgx@deep-worker1 tensorflow]$ ./configure
You have bazel 0.11.1- (@non-git) installed.
Please specify the location of python. [Default is /usr/bin/python]: /home/shgx/Python3.5/bin/python3


Found possible Python library paths:
  /home/shgx/Python3.5/lib/python3.5/site-packages
Please input the desired Python library path to use.  Default is [/home/shgx/Python3.5/lib/python3.5/site-packages]

Do you wish to build TensorFlow with jemalloc as malloc support? [Y/n]: 
jemalloc as malloc support will be enabled for TensorFlow.

Do you wish to build TensorFlow with Google Cloud Platform support? [Y/n]: n
No Google Cloud Platform support will be enabled for TensorFlow.

Do you wish to build TensorFlow with Hadoop File System support? [Y/n]: n
No Hadoop File System support will be enabled for TensorFlow.

Do you wish to build TensorFlow with Amazon S3 File System support? [Y/n]: n
No Amazon S3 File System support will be enabled for TensorFlow.

Do you wish to build TensorFlow with Apache Kafka Platform support? [y/N]: 
No Apache Kafka Platform support will be enabled for TensorFlow.

Do you wish to build TensorFlow with XLA JIT support? [y/N]: 
No XLA JIT support will be enabled for TensorFlow.

Do you wish to build TensorFlow with GDR support? [y/N]: 
No GDR support will be enabled for TensorFlow.

Do you wish to build TensorFlow with VERBS support? [y/N]: 
No VERBS support will be enabled for TensorFlow.

Do you wish to build TensorFlow with OpenCL SYCL support? [y/N]: 
No OpenCL SYCL support will be enabled for TensorFlow.

Do you wish to build TensorFlow with CUDA support? [y/N]: y
CUDA support will be enabled for TensorFlow.

Please specify the CUDA SDK version you want to use, e.g. 7.0. [Leave empty to default to CUDA 9.0]: 9.1


Please specify the location where CUDA 9.1 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: 


Please specify the cuDNN version you want to use. [Leave empty to default to cuDNN 7.0]: 


Please specify the location where cuDNN 7 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]:7.1.2


Please specify the cuDNN version you want to use. [Leave empty to default to cuDNN 7.0]: 


Please specify the location where cuDNN 7 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]:


Do you wish to build TensorFlow with TensorRT support? [y/N]: 
No TensorRT support will be enabled for TensorFlow.

Please specify a list of comma-separated Cuda compute capabilities you want to build with.
You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.
Please note that each additional compute capability significantly increases your build time and binary size. [Default is: 6.1,6.1,6.1,6.1]


Do you want to use clang as CUDA compiler? [y/N]: 
nvcc will be used as CUDA compiler.

Please specify which gcc should be used by nvcc as the host compiler. [Default is /usr/bin/gcc]: 


Do you wish to build TensorFlow with MPI support? [y/N]: 
No MPI support will be enabled for TensorFlow.

Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native]: 


Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]: 
Not configuring the WORKSPACE for Android builds.

Preconfigured Bazel build configs. You can use any of the below by adding "--config=<>" to your build command. See tools/bazel.rc for more details.
	--config=mkl         	# Build with MKL support.
	--config=monolithic  	# Config for mostly static monolithic build.
	--config=tensorrt    	# Build with TensorRT support.
Configuration finished
```
7. configure成功后，依然还在tensorflow的根目录下输入以下命令(大约20分钟)
```
bazel build --config=opt --config=cuda tensorflow/tools/pip_package:build_pip_package
```
8. 之后执行：
```
bazel-bin/tensorflow/tools/pip_package/build_pip_package /home/shgx/tmp/tensorflow_pkg
```
9. 最后```cd /tmp/tensorflow_pkg```切换到```/tmp/tensorflow_pkg```目录下，使用pip3进行安装```pip3 install tensorflow-1.6.0-cp35-cp35m-linux_x86_64.whl```（具体名字和你的python版本有关）
### 四、GPU使用
1. 使用```nvidia-smi```查看GPU使用情况
2. 选择一块GPU进行实验```CUDA_VISIBLE_DEVICES=0 python3 xxx.py```