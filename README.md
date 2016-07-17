# notes
notes share from internet


##从AWS X1说起：为什么公有云也用四路服务器？
[Scale up能很好解决的问题，就没必要去Scale out。Google在2009年发布的《The Datacenter as a Computer》中很重要的一个观点：“强核理论”（编者注：强核与弱核是相对的。例如x86处理器相对ARM是强核，所以少有Google关于ARM的消息）。简单说来，在一个集群之中，单节点的性能越好，整个集群在通信上耗费的总体资源比例越低，整体利用率更好。比如，一个由较高性能服务器（单节点128核心）组成的集群，对比一个由较普通性能服务器（单节点4核心）组成的集群，集群总核心数量相同情况下，前者的总性能几乎是后者的10倍以上，后者在通信上效率也远不如前者。详见《The Datacenter as a Computer》33-36页](https://mp.weixin.qq.com/s?__biz=MzA3NTM0OTcyOA==&mid=2651310376&idx=1&sn=a5aa1e076155fb3907c39ce59f5a7729&scene=0&pass_ticket=6UJWfSVBUL98jDAy3ra%2BP5YUFnkwfpQMfQhWovUE3ps%3D#rd)


