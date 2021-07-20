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