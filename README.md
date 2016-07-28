# notes
notes share from internet


##从AWS X1说起：为什么公有云也用四路服务器？
[Scale up能很好解决的问题，就没必要去Scale out。Google在2009年发布的《The Datacenter as a Computer》中很重要的一个观点：“强核理论”（编者注：强核与弱核是相对的。例如x86处理器相对ARM是强核，所以少有Google关于ARM的消息）。简单说来，在一个集群之中，单节点的性能越好，整个集群在通信上耗费的总体资源比例越低，整体利用率更好。比如，一个由较高性能服务器（单节点128核心）组成的集群，对比一个由较普通性能服务器（单节点4核心）组成的集群，集群总核心数量相同情况下，前者的总性能几乎是后者的10倍以上，后者在通信上效率也远不如前者。详见《The Datacenter as a Computer》33-36页](https://mp.weixin.qq.com/s?__biz=MzA3NTM0OTcyOA==&mid=2651310376&idx=1&sn=a5aa1e076155fb3907c39ce59f5a7729&scene=0&pass_ticket=6UJWfSVBUL98jDAy3ra%2BP5YUFnkwfpQMfQhWovUE3ps%3D#rd)

##三层交换与路由
[从硬件上说，三层交换机是通过交换芯片转发数据的，交换芯片是带有三层转发能力的，也就是路由的功能。路由器则是通过CPU转发的，所有的报文的重新计算和转发任务是在CPU的计算下完成的。三层交换使用NP作为转发实现，报文路由通过CPU计算出来之后，下发到NP，后续报文即根据三层转发表进行路由（FIB？）；路由器CPU功能强，计算能力强，可以快速响应路由的变化。路由器转发报文都需要经过CPU实现，报文上送耗时较NP大。](https://www.zhihu.com/question/20843778)

##169.254.0.0地址的作用
[169.254.0.0/16地址是Automatic Private IP Addressing, 用于在使用DHCP获取不到地址的情况下，自动给本机分配的一个随机地址（冲突怎么办？）。由/etc/sysconfig/network NOZEROCONF 配置项控制是否启用](http://serverfault.com/questions/132657/where-route-to-169-254-0-0-comes-from)
