赋权这玩意儿，在运维中很常用，一般可以用来实现几个功能。

1、提高某个用户的权限（可以理解为暂时将其赋予root权限，sudo su实现）

2、锁定某个用户切换权限（!/bin/su，禁止切换用户）

3、免密切换登陆（NOPASSWD，切换用户无需再次输入密码）

~~~pwel
赋权
echo 'aiuap ALL=(ALL) NOPASSWD: ALL,!/bin/su' >/etc/sudoers.d/diy_sudo

字段意思：
echo >> 输出
'' >> 里面的内容就是输出内容
>/etc/sudoers.d/diy_sudo >> 就是输出到的指定文件路径

内容意思：
'aiuap ALL=(ALL) NOPASSWD: ALL,!/bin/su'
aiuap >> 用户名
ALL=(ALL) NOPASSWD: ALL >> 免密登陆
!/bin/su >> 禁止切换，不写这段，就是可以允许切换，看具体情况决定要不要用

下面看个例子吧
[root@localhost ~]# cat /etc/sudoers.d/diy_sudo 
aiuap ALL=(ALL) NOPASSWD: ALL,!/bin/su
hmj ALL=(ALL) NOPASSWD: ALL
hmjj ALL=(ALL) NOPASSWD: ALL,!/bin/su

这里已经设置了三条赋权，其中，hmj是没有后面,!/bin/su这段的，因此可以理解为，hmj用户允许切换。

[root@localhost ~]# su hmj
[hmj@localhost root]$ cat /etc/shadow
cat: /etc/shadow: Permission denied         //注意，这里普通用户是没有权限查看/etc/shadow文件的
[hmj@localhost root]$ sudo su               //这里便是赋权
[root@localhost ~]# cat /etc/shadow         //再次进入/etc/shadow，发现可以进入，所以便证明赋权成功了
hmj:$6$dqA32r76$eH5Me3RYuHNnwHkD6ercopI3Q0rzDq.ljW9bpGc4HAD.Y4sJgJT67WmCCq85O.KaZXJahUFYsv/hwCfjoEPPq.:19110:0:99999:7:::
hmjj:$6$wmPdNHRe$XujXOaqkAi5d0F9ior3LBegG19k5jbr861FRlMDGohTJ2AJdrc3bgkPeg7kfXshoTYtjQuGxmZoduw/2oKg4k1:19110:0:99999:7:::

验证一下!/bin/su字段，因为hmj没有写这条，所以是允许切换用户的
[hmj@localhost root]$ su hmjj
Password: 
[hmjj@localhost root]$ 

而hmjj写入了!/bin/su，因此是禁止切换
[hmjj@localhost root]$ sudo su
Sorry, user hmjj is not allowed to execute '/bin/su' as root on localhost.localdomain.       //切换用户失败       
[hmjj@localhost root]$ 
~~~

