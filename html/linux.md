# 关于Linux主机报错(Resource temporarily unavailable)的解决办法  

### 现象描述:  
主机登陆或者切换用户时报错资源暂时不可用(Resource temporarily unavailable)。 
``` 
[root@zyserver ~]# su - oracle  
-bash: fork: retry: Resource temporarily unavailable  
-bash: fork: retry: Resource temporarily unavailable  
-bash: fork: retry: Resource temporarily unavailable  
-bash: fork: retry: Resource temporarily unavailable  
-bash: fork: retry: Resource temporarily unavailable  
-bash: fork: Resource temporarily unavailable  
```

### 解决方法：  
这是由于主机上用户打开的线程太多，超过了系统内核参数设置。   
需要调整主机上的参数。  
编辑以下文件/etc/security/limits.conf,调整以下参数即可.  

```
oracle soft nproc  65536  
oracle hard nproc  65536  
oracle soft nofile 65536  
orale hard  nofile 65536  
```

另外，还需要将以下文件/etc/security/limits.d/90-nproc.conf的参数也适当增大。  
-bash-4.1$ more etc/security/limits.d/90-nproc.conf  
```
# Default limit for number of user's processes to prevent
# accidental fork bombs.
# See rhbz #432903 for reasoning.

* soft         nproc            65536
```  
### 查看java进程开启的线程数量  
`ps -ef|grep java|awk '{ print $2 }'|xargs -I {} top -Hp {} -b -n 1 >> 1.log`  

### 查看指定pid的进程开启的线程数量  
```
top -Hp ${pid} -b -n 1 | grep -i threading  
# 例如：top -Hp 32091 -b -n 1 | grep -i threads  
```  
### 根据指定列排序 
 
```
#根据第三列进行降序排列，-k3代表第三列，-rn代表降序
sort -k3 -rn
```  
### pstree安装  
```
#ubuntu/debian
apt-get install psmisc
#centos
yum install psmisc  
```  
### Linux强制踢人命令  
```
Linux中root用户可以T掉其他登录的用户，包括登录的用户，也可以T掉自己。

用who命令查看登录的用户

[admin@localhost ~]$ who
admin    pts/2        2011-06-13 16:22 (192.168.0.89)
admin    pts/3        2011-06-13 09:59 (192.168.0.109)
root     pts/4        2011-06-13 15:44 (192.168.0.109)
admin    pts/5        2011-06-13 16:22 (192.168.0.212)

以上是登录系统的相关用户已经登录的时间ip等。



踢人命令：pkill -kill -t tty

例如我现在想踢出192.168.0.89这个用户则：pkill -kill -t pts/2即可
```  

