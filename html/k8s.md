# 遇到的问题
### Killing container with id docker://log-collector:Need to kill Pod  
问题原因：  
    这个问题是Kubernetes偶发的BUG  
解决办法：    	
    kubectl delete pod xxxxxx --grace-period=0 --force  