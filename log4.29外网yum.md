真是绝了，配置外网yum居然如此简单！

这个问题还困扰我长达一个月，真是晕~

~~~perl
[root@localhost yum.repos.d]# cd /etc/yum.repos.d/
[root@localhost yum.repos.d]# ls
CentOS-Base.repo  CentOS-Debuginfo.repo  CentOS-Media.repo    CentOS-Vault.repo
CentOS-CR.repo    CentOS-fasttrack.repo  CentOS-Sources.repo
[root@localhost yum.repos.d]# mkdir repo_backup            //新建一个备份的文件夹repo_backup
[root@localhost yum.repos.d]# mv  *.repo  repo_backup/     //把当前目录的所有文件，都放到repo_backup
[root@localhost yum.repos.d]# ls
repo_backup
~~~

完成这一步，我们就可以找到阿里云镜像，下载并放进该目录下。

http://mirrors.aliyun.com/repo/Centos-7.repo 阿里云网络仓库镜像

因为我通过跳板机连到的别的设备，所有直接上传是上传不了的，最远就只能到达跳板机，所有还需要用scp从跳板机把文件传输到最终的测试机。

~~~perl
跳板机
[root@p5b0409d8 yum.repos.d]# ls         //确认文件所在目录
bak  Centos-7.repo
[root@p5b0409d8 yum.repos.d]# scp /etc/yum.repos.d/Centos-7.repo root@91.4.9.103:/etc/yum.repos.d/
root@91.4.9.103's password: 
Centos-7.repo                                     100% 2523     1.8MB/s   00:00    
~~~

这里发现，已经看到Centos-7.repo这个文件了，这就是阿里云的镜像
到这一步，，已经算是做好了外网yum的基本配置了，但是现在安装软件，还是会报错，显示无法安装
因为我们还需要配置==最重要的DNS！！！==

~~~perl
[root@localhost yum.repos.d]# vi /etc/resolv.conf
[root@localhost yum.repos.d]# cat /etc/resolv.conf
nameserver 114.114.114.114
nameserver 8.8.8.8
[root@localhost yum.repos.d]# 
~~~

这样便是配置好了外网yum

最重要的还是得配置DNS不然之前做的一切都没有用！！！                                                  