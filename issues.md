Learn from the issues

# MTU misconfig
## MTU misconfig的表现是什么
Lose connection  


## MTU的作用
https://en.wikipedia.org/wiki/Maximum_transmission_unit

MTU - the largest size of protocol data unit(PDU) - not include the data link/physical layer header.    
L2MTU - Ethernet II - IEEE 802.3 define the MTU size is [46-1500]; Jumbo frame can be 9000, not the part of IEEE 802.3, but has fully supported by major vendors; - https://zh.wikipedia.org/wiki/%E5%B7%A8%E5%9E%8B%E5%B8%A7   
IPMTU - the length is defined by the total length in IP header, maxmize can be 2^16-1-20;  
MSS - the L4 MTU, TCP use it to define max payload can be sent in a tcp segmentation without fragment.  

why Ethernet II MTU size is 1500 - https://community.cisco.com/t5/other-network-architecture/why-the-mtu-size-is-1500/td-p/105418

Larger MTU reduce overhead, smaller MTU reduce network delay.  


```
if package size > L3MTU
  if don't fragement set(DF=1)
    drop package
  else
    do fragment according to L3MTU

send package
```

## 虚拟化场景有什么特别
Liunx bridge/OVS interface has it's own MTU configuration.  
需要查看tap，qbr，qvm，ply等interface是否都配置正确

## 诊断/解决的方法
Check link MTU：
- ifconfig DEV
- ip link show DEV

```
ping -M dont -s 90000 202.114.0.248
-M pmtudisc_opt
  Select Path MTU Discovery strategy.  pmtudisc_option may be either do (prohibit fragmentation, even local one, DF=1), 
  want (do PMTU discovery, fragment locally when packet size is large), or dont (do not set DF flag, DF=0).

default is auto - sent small package the DF=1, sent large package the DF=0.

-f for Winodws, not fragment.

# tcpdump -i bond0 -nnvv icmp
tcpdump: listening on bond0, link-type EN10MB (Ethernet), capture size 65535 bytes
16:45:03.369130 IP (tos 0x0, ttl 64, id 60047, offset 0, flags [+], proto ICMP (1), length 8884)
    192.168.0.217 > 202.114.0.248: ICMP echo request, id 29869, seq 1, length 8864
16:45:03.369138 IP (tos 0x0, ttl 64, id 60047, offset 8864, flags [none], proto ICMP (1), length 164)
    192.168.0.217 > 202.114.0.248: ip-proto-1
16:45:03.413705 IP (tos 0x0, ttl 42, id 3076, offset 0, flags [+], proto ICMP (1), length 1500)
    202.114.0.248 > 192.168.0.217: ICMP echo reply, id 29869, seq 1, length 1480
16:45:03.413712 IP (tos 0x0, ttl 42, id 3076, offset 1480, flags [+], proto ICMP (1), length 1500)
    202.114.0.248 > 192.168.0.217: ip-proto-1
16:45:03.413713 IP (tos 0x0, ttl 42, id 3076, offset 2960, flags [+], proto ICMP (1), length 1500)
    202.114.0.248 > 192.168.0.217: ip-proto-1
16:45:03.413714 IP (tos 0x0, ttl 42, id 3076, offset 4440, flags [+], proto ICMP (1), length 1500)
    202.114.0.248 > 192.168.0.217: ip-proto-1
16:45:03.413714 IP (tos 0x0, ttl 42, id 3076, offset 5920, flags [+], proto ICMP (1), length 1500)
    202.114.0.248 > 192.168.0.217: ip-proto-1
16:45:03.413754 IP (tos 0x0, ttl 42, id 3076, offset 7400, flags [+], proto ICMP (1), length 1500)
    202.114.0.248 > 192.168.0.217: ip-proto-1
16:45:03.413757 IP (tos 0x0, ttl 42, id 3076, offset 8880, flags [none], proto ICMP (1), length 148)
    202.114.0.248 > 192.168.0.217: ip-proto-1
--> data in request MUST returned in the echo reply package

# ping -s 10000 -c 1 202.114.0.248  -M do
PING 202.114.0.248 (202.114.0.248) 10000(10028) bytes of data.
ping: local error: Message too long, mtu=8888

--- 202.114.0.248 ping statistics ---
1 packets transmitted, 0 received, +1 errors, 100% packet loss, time 0ms
```

PMTUD - https://www.cisco.com/c/en/us/support/docs/ip/generic-routing-encapsulation-gre/25885-pmtud-ipfrag.html#anc6

Change MTU  
```
ifconfig ${Interface} mtu ${SIZE} up

# vi /etc/sysconfig/network-scripts/ifcfg-eth0
#add 
MTU="9000"

# service network restart
```
