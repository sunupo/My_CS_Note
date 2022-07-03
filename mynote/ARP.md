## 2.ARP协议

 

　　还记得数据链路层的以太网的协议中，每一个数据包都有一个MAC地址头么?我们知道每一块以太网卡都有一个MAC地址，这个地址是唯一的，那么IP包是如何知道这个MAC地址的?这就是ARP协议的工作。



　　ARP(地址解析)协议是一种解析协议，本来主机是完全不知道这个IP对应的是哪个主机的哪个接口，当主机要发送一个IP包的时候，会首先查一下自己的ARP高速缓存(就是一个IP-MAC地址对应表缓存)，如果查询的IP-MAC值对不存在，那么主机就向网络发送一个ARP协议广播包，这个广播包里面就有待查询的IP地址，而直接收到这份广播的包的所有主机都会查询自己的IP地址，如果收到广播包的某一个主机发现自己符合条件，那么就准备好一个包含自己的MAC地址的ARP包传送给发送ARP广播的主机，而广播主机拿到ARP包后会更新自己的ARP缓存(就是存放IP-MAC对应表的地方)。发送广播的主机就会用新的ARP缓存数据准备好数据链路层的的数据包发送工作。