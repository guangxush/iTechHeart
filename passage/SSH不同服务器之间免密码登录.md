### 方法一：

- ```ssh-keygen -t rsa -f hello```建立公钥

- ```cd ~/.ssh cat hello.pub```查看公钥并复制到对方服务器.ssh下方的authority中

- 目标主机之间的~/.ssh目录下面新建authorized_keys文件

- 将复制的公钥粘贴追加到目标主机的authorized_keys文件即可

(注意：将目标主机下的.ssh权限为700:```chmod 700 -R .ssh```，authorized_keys设为600:```chmod 600 .ssh/accio .ssh/authorized_keys```)

- ```ssh -i hello user@192.168.1.211```即可免密码连接

### 方法二：

- 原机器生成秘钥```ssh-keygen -t hello```

- 复制到新机器上```ssh-copy-id -i ~/.ssh/hello.pub root@192.168.1.211```

- 直接免密码登录即可```ssh root@192.168.1.211```

### 动手实践：

1. 进入本地ServerA的usera账户下的.ssh目录

2. 本地设置公钥：``` ssh-keygen -t rsa -f accio```(accio为私钥名称)

3. ```ls -la .ssh``` 此时本地.ssh目录下会产生 accio(私钥) accio.pub（公钥）

4. ```cat accio.pub``` 查看公钥

5.将公钥上传到serverB并以userb登录:```ssh-copy-id userb@ServerBip```

6.此时在查看ServerB中userb的.ssh目录，会发现usera的公钥文件内容已经复制到accio目录下面```cat accio.pub```；同时也追加到了```.ssh/authorized_keys``` 文件中，内容是一致的

7.设置userb中.ssh权限为700，```chmod 700 -R .ssh```，authorized_keys的权限为600 ```chmod 600 .ssh/accio .ssh/authorized_keys```;并查看```ls -la .ssh```

8.使用 免密登录```ssh -i accio userb@ServerBip```即可

9.也可以通过```cat ~/.ssh/id_rsa.pub | ssh -p 22 user@host ‘cat >> ~/.ssh/authorized_keys```的方式复制公钥

### 实现原理：
![图片1.png](https://github.com/guangxush/iTechHeart/blob/master/image/Passage/ssh.png)
