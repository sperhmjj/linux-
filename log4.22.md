> ​	今天来个好玩的，linux中修改指定用户密码不过期。

​	废话少说，let’s go!

​	这个操作跟shadow文件有密切关系，所以先介绍一下。

​	/etc/shadow 文件，用于存储 Linux 系统中用户的密码信息，又称为“影子文件”。

​	由于 /etc/passwd 文件允许所有用户读取，易导致用户密码泄露，因此 Linux 系统将用户的密码信息从 /etc/passwd 文件中分离出来，并单独放到了此文件中。

​	/etc/shadow 文件只有 root 用户拥有读权限，其他用户没有任何权限，这样就保证了用户密码的安全性。

​	首先找到shadow文件。

~~~perl
[root@bss-acct-ys-04 brudoa]# vi /etc/shadow
bsszk:$6$pvY5ywKH$GqHbZ2benbHE3MI5qucmfGiBjRqnOB9wXl4kTId5TiRfaAtdc9UNkMl0At.vfA1BLMUOsPKRAgIKMplNbMhIl.:18948:10:90:5:::
~~~

​	可以看到，这一条信息，就是我们需要动手的，但是不知道改哪里怎么办。

​	同 /etc/passwd 文件一样，文件中每行代表一个用户，同样使用 ":" 作为分隔符，不同之处在于，每行用户信息被划分为 9 个字段。每个字段的含义如下：

​	用户名：加密密码：最后一次修改时间：最小修改时间间隔：密码有效期：密码需要变更前的警告天数：密码过期后的宽限时间：账号失效时间：保留字段

将信息段拆分，如下：

~~~perl
bsszk:    //用户名
$6$pvY5ywKH$GqHbZ2benbHE3MI5qucmfGiBjRqnOB9wXl4kTId5TiRfaAtdc9UNkMl0At.vfA1BLMUOsPKRAgIKMplNbMhIl.:      //加密密码
18948:     //最后一次修改时间
10:            //最小修改时间间隔
90:            //密码有效期
5:             //密码需要变更前的警告天数
:              //密码过期后的宽限时间
:              //账号失效时间
~~~

​	至于为什么没第九字段，暂时涉及知识盲区了，日后有时间再回来巩固一下。

​	因此来看，我们只需要修改第五个字段，便可以了。

bsszk:6pvY5ywKH$GqHbZ2benbHE3MI5qucmfGiBjRqnOB9wXl4kTId5TiRfaAtdc9UNkMl0At.vfA1BLMUOsPKRAgIKMplNbMhIl.:18948:10:==90==:5:::

​	将他改为9999，便大功告成！

​	密码有效期9999天，也算是另一种意义的永久不过期了，毕竟9999天相当于27.3年，一辈子又有多少个27.3年呢？你说是吧！