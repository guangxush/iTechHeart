## 服务器使用

### ssh远程连接

ssh -p 21122 shgx@xxx.xxx.xxx.xxx
ssh shgx@xxx.xxx.xxx.xxx:21122 这个是211
ssh shgx@xxx.xxx.xxx.xxx:22

### 远程桌面连接
xrdp shgx@xxx.xxx.xxx.xxx:13389 (实际是211)
ssh xxx.xxx.xxx.xxx -p 21122

### 远程传输文件
FileZilla的sftp://xxx.xxx.xxx.xxx:21122   13389
scp -P 21122 -r shgx@xxx.xxx.xxx.xxx:/data/shgx/models /Users/guangxush/Pycharm/models

### 常用的命令

1. Pycharm 环境配置

```text
PYTHONUNBUFFERED 1
LD_LIBRARY_PATH /usr/local/cuda/lib64
CUDA_VISIBLE_DEVICES 2(这个数字填写你使用的GPU编号)
```

2. GPU使用情况

  ```
    nvidia-smi
  ```
  使用命令```watch -n 1  nvidia-smi```(使用watch是为了隔一秒查看一次)
  ![GPU使用情况](https://github.com/guangxush/iTechHeart/blob/master/image/Server/RPC11.png)
  
3. 使用gpu运行程序
  - 命令行方式使用：前面要加CUDA_VISIBLE_DEVICES=2，选择上面的一块空闲CPU，如果不加的话会把所有的GPU全占了（上图是把GPU全占了，正常情况下红框里出现的GPU就是有人使用，没有出现在红框的编号可以使用，一般是0-3共4块）
     
     ```
     CUDA_VISIBLE_DEVICES=2 python cab_embedding.py  train.txt test.txt
     CUDA_VISIBLE_DEVICES=0 python mnist-tf.py 
     ```
     
  - 代码的方式使用GPU
     在程序中加入代码：
      
      ```text
      import os
      os.environ['CUDA_VISIBLE_DEVICES'] = '1'
      ```
      
      后面的数字表示GPU的编号，这时运行的时候输入python main.py即可 
4. 屏幕使用(让你在后台使用，下次在登录服务器的时候也能看到程序的执行情况)
```
  screen -S GX 新建屏幕
  screen -ls 显示所有的屏幕
  screen -r GX 进入查看结果
  ctrl＋D 杀死屏幕
  screen -S GX -X quit 强制杀死屏幕
```

5. nohup使用（也可以在后台运行程序，程序输出的结果默认保存在nohup.out文件）  
```
nohup python main.py &
```

6. vim 编辑
 ``` 
 i 插入
 ESC + :wq 保存退出
 ```
  
7. 文件压缩
```
zip 压缩为zip格式的文件
unzip 解压文件
```

8. 在新机器上面运行
```
  ssh shgx@192.168.1.211 
```

9. 新机器上面安装软件 
```
pip install pandas --user
```

10. 安装TensorFlow
```
pip install tensorflow --user --upgrade(升级)
pip list 查看所有安装
pip install   -U  tensorflow-gpu --user  升级
```

11. 服务器之间文件拷贝
```
  scp taxi_predict.zip shgx@10.60.103.248:./
```

12. 复制文件
```
  cp test1/1.txt test2/
```

13. 把文件拷贝到新机器上面
```
  scp -P 21122 -r shgx@原地址 目标地址
  scp -r taxi_predict shgx@192.168.1.***:/home/shgx
```

14. 重命名文件和文件名,将A重命名为B
```
mv A B 
```
13. 安装Python3
```
下载Python3源码 wget https://www.python.org/ftp/python/3.6.0/Python-3.6.0.tgz
解压 tar -xvf Python-3.6.0.tgz
创建安装路径 mkdir /home/shgx/python3
编译 ./configure --prefix=/home/shgx/python3
安装 先 make 然后 make install
建立软连接 ln -s /home/shgx/python3/bin/python3 /usr/bin/python3
检查python版本 python -V 显示python-3.6.0 如果不成功，见本页23
```
14. 文件夹移动剪切
```
mv Python-3.6.0  /home/shgx
```
15. 文件重命名
```
mv Python-3.6.0/ python3.6 重命名
rename a.txt b.txt
```
16. 安装的Python3 下面安装软件包
```
python3 -m pip --default-timeout=100 install 
pip3 install tensorflow-gpu=1.2
```
17. Linux 显示文件的一部分
```
head -n 10 a.txt
tail -n 10 a.txt
```
18. 删除zip文件
```
rm -f *.zip
```
19. git
```
git rm -r --cached .移除本地git commit
git log 查看日志 git resert --hard commit_id回到之前的版本
git reset –hard id 撤销版本 git reset id不对代码撤销
撤销git add: git reset HEAD
撤销git commit: git revert HEAD /上上次 HEAD^
```
20. 文件压缩
```
zip -r xxx.zip ./*
unzip filename.zip
```
21. 文件大小
```
du -h --max-depth=1
```
22. 输出环境变量
```
echo $PATH
```
23. 安装python3.6环境变量配置
```text
在/home/shgx目录下ls -a 查看.bashrc
然后vim .bashrc
添加自己的路径
export PATH="/home/shgx/Python3.6/bin/:$PATH"
保存之后source .bashrc
查看路径which  python3
```
