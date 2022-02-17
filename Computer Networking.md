1. **一个 router 至少链接到2个 subnet ，因此至少有2个 IP addr，这不是对 IP addr 的浪费吗？**
    + 对 IP 来说，其寻址的对象是 subnet，而 subnet 其实就可以粗暴第看成一些由 routers 阻隔开的 islands

2. **下层协议要为上层协议提供服务，因此需要知道上层送来的包里装的究竟是哪个上层协议，所以在其 header 中需要加上协标识协议的字段**

3. **PPP 的帧起始和帧结束都是同样的标志，岂不是可能出现一种情况：第一帧传送到一半失败了，第二帧马上跟着到，接收方把第二帧的起始标志当成第一帧的结束标志，后面全乱套**

4. **Manchester 编码用跳变表示0和1，用不跳变表示无数据，这一做法可以让接收者不需要根据帧长度就可以知道何处截止**
	+ 然而 Manchester 编码需要双方同步时钟，因此每传送一阵都要在前面增加8个字节用于同步时钟

5. **Ethernet 在传送帧时，各帧间有一定空隙，因此只需要找帧的开始界定符，而不需要找结束界定符，也不需要用字节插入来保证透明传输**

6. **Web，HTTP，和 HTML 是什么关系？**
    + 三者是深深绑定在一起的，但并不是同一个概念
	+ Web 是应用的名字
	+ HTML 是一种超文本标记语言，用于描述网页文档
	+ HTTP 是应用层的传输协议，规定 Web 应用的 client 和 server 之间如何请求响应

7. **对网络协议栈的理解：**
	+ application layer 决定发送什么样的信息
	+ transport layer 决定怎么发送这些信息
	+ network layer 决定怎么把信息送到目的地
	+ linker layer 决定路径上的两个节点之间如何转送
	+ physical layer 是线路

8. **Ethernet 和局域网是什么关系？**
	+ 局域网是一种网络形式，具体可以理解为在此类网络中，所有主机之间不需要经过路由器就可以直接访问到
	+ Ethernet 是一种具体的组成局域网的技术，其他组成局域网的技术还有令牌环网和 ARCNET，但后两者都被淘汰了
	+ 由于目前组成局域网绝大多数都采用的是 Ethernet，所以 Ethernet 逐渐变成了局域网的代名词，二者经常被混用

9. **距离永远是影响通信速率的一大因素，不论是有线还是无线**
	+ 距离会使信号衰减，导致信噪比降低，而信噪比则是限制通信速率（或者说是载波频率）的一大因素

10. **有线网络的安装成本远远高于材料成本，因此很多建筑商在建楼之初就为每一间房子装了有线网络设施**

11. **IP address 是用来标记 host 在网络中的逻辑位置的，而 prot number 则是用来对外标记同一 host 上的不同进程的**
	+ port number 的出现，根本上还是源于多任务，如果系统只有一个进程运行，也就不需要什么 port number 了

12. **是不是所有介质的网络都需要用 model？**
	+ 应该是的，至少 dial-up，DSL 和 HFC 都需要
	+ model 就是一种 A/D、 D/A 转换工具，应该所有介质都是需要的

13. **Intranet 指的是用与 Internet 相同的协议的私人网络**

14. **end system = host**
	+ 在网络中的计算机，因为位于网络拓扑的外缘，所以被称为 end system
	+ 又因为它们容纳了使用网络的程序，所以被称为 host
		+ host 有根据其所容纳的网络应用的类型，被分为 server 和 client
	
15. **MAC 地址是不是专门为 Ethernet 提出的？**
	+ 是的，所谓 MAC 中的 Media Access Control，正是 Ethernet 中的概念，用以在同一 Ethernet 下通过区分不同主机来协调通信，也正因为如此， MAC 地址也被称为 Ethernet 地址
	+ 其他类型的局域网似乎用不到 MAC 地址，不过目前也怎么存在其他类型的局域网了

16. **transport layer 和 network layer 中都有 connection-oriented service 概念，但二者是不一样的：**
	+ transport layer：方法是利用重传和确认机制，是在 end systems 上实现的
	+ network layer：方法是建立一条 virtual circuit，是在 Internet core 中实现的

17. **Router 为了减小 forwarding table 的尺寸，就必须得将 IP address 按一定粒度分块，这就要求了 IP address 的分配一定是 hierarchical 的**
	+ 而 hierarchical 的不足之处在于，容易产生 hole，即块内的地址浪费
		+ 这种 hole，与内存分页机制中的 internal fragmentation 的本质是一样的，只要管理一片连续空间的策略是按一定粒度，那么 internal fragmentation 就一定难免存在

18. **MAC addr 是平坦的，而 IP addr 是有层级（hierarchical）的**
	+ MAC addr 是为网络而设，而 IP addr 是为网络的网络而设

19. **PPP，CSMA/CD 和 MAC 之间的关系是什么？**
	+ PPP 是点对点通信协议，用于用户和 ISP 之间的通信，与 CSMA/CD 和 MAC 之间没有关系，也不用 MAC 帧格式
	+ CSMA/CD 是利用广播信道的以太网的随机接入控制协议，MAC 对顶发送的帧格式，换句话说，在 Ethernet 中， CSMA/CD 决定由谁来发送，MAC 决定发什么样格式的数据

20. **CSMA 是载波监听，旨在预先避免碰撞，但实际情况下由于时延的存在，光载波监听是无法避免所有碰撞的，因此还需要加上 CD，即碰撞检测；CSMA 是预先避免，CD 是碰撞后补救，二者协作**

21. **使用交换机的以太网还需要 CSMA/CD 吗？**
	+ 由于 switch 的分组转发机制，不存在碰撞，于是似乎也就既不需要 CSMA，也不需要 CD 了

22. **有种针对 switch 的攻击方式是：**
	+ 先不断发送 MAC 地址变化的 frame，占满 switch 的内存，如此一来就无处存放实际存在的主机的 MAC 地址，于是所有 frame 都不得不退化成广播，然后就可以用 sniffer 捕获原本只发给某个主机的信息了
	+ 不过正常的 switch 是有防嗅探功能的

23. **什么是 baseband transmission？**
	+ 就是不把信号进行频谱搬移
	+ 正是因为 Ethernet 使用 baseband transmission，双方不知道对方的 baseband，所以需要在 Ethernet 帧的开头加上8字节的 preamble 用来协调频率

24. **The link layer is a combination of hardware and software - the place in the protocol stack where software meets hardware.**

25. **为什么transport layer 的校验用简单的校验和，而 link layer 的校验用更复杂的 CRC？**
	+ 这是因为 transport layer 由软件实现，慢且不便使用复杂算法过度占用 CPU 资源，而 link layer 的校验用硬件实现，快而且不占用 CPU
	+ 所谓软件实现，就是要编程让 CPU 去执行；所谓硬件实现，泛指让 CPU 以外的运算设备来实现，例如 fpga，显卡等，这些运算设备由于只专注于某一类运算，因而可以进行各种有针对性的优化，将效率和速率提升到很高
	+ 实际上，link layer 很多协议和功能都是直接用硬件实现的，比如差错校验，而其以上各层的协议则基本都是由软件实现的

26. + **link layer 为什么要封装成帧？**
	1. 上层协议都是要封装成帧的
	2. 若中途出现了差错，则需要重传的只是一帧而不是全部数据
	3. 在link layer，没有一直传送数据的需求，而且传送的数据来源不同，意义不同，这些信息都需要标注，"分割标注"，自然就形成了帧

27. **MAC 协议，MAC 地址与 MAC 帧是什么关系？**
	+ MAC = Media Access Control
	+ MAC 协议并不是一项具体的协议，其全称为媒体接入控制协议，是一类用于控制帧如何传入信道的协议，在广播信道的局域网和点对点信道中是不同的，它们统称 MAC 协议
	+ MAC 地址中的 MAC 与 MAC 协议中的 MAC 是同一个 "MAC"，即用于局域网中标记身份的地址，"MAC" 帧中的 "MAC" 也与之相同，对应传送的帧的格式

28. **link layer 和 transport layer 都可以有可靠传输和流量控制功能，颇为类似**
	+ 但实际上有很大区别：link layer 是相邻节点之间的可靠传输和流量控制，而 transport layer 这是端到端，相似但不同

29. **网络适配器、网卡、交换机、网桥、路由器、集线器这些设备分别是什么？**
	+ 网络适配器是代表主机作为网络接口的设备，MAC address 位于其上，网卡是网络适配器的简称
	+ 网桥主要用于连接两个 LAN，从而起到扩展的作用，其内部采用分组转发来实现过滤，集线器直接转发 bits，作用也是用于扩展 LAN，但不如网桥先进，集线器可以等效成一个"焊点"
		+ 网桥是先把整帧接收下来再转发，而集线器则是逐比特转发

30. **只有在 link layer 上数据才被当成一系列比特流，在其上面各层协议的视角中，数据都是包而不是流**

31. **OSI 模型只是参考而不是规范，某个协议能完成很多层的功能很正常，不必严格按照分层模型来对协议进行僵化的划分**

32. **广播信道的接入方式分为静态划分信道和动态接入两种**

33. **以集线器为中心的星形网与总线网在拓扑上没有区别，因为集线器只是进行简单的比特转发，而不进行碰撞检测**

34. **关于可靠传输**
	+ 可靠传输既可以交给网络，也可以交给终端机来实现，这是两种截然不同的思路
	+ 电信网之所以选择前者，是因为过去电话机功能简单
	+ 而 Internet 之所以用后者，是因为计算机终端功能足够强大，而且把可靠传输的任务给终端，好处是可以使网络基础设施大大简化，从而降低成本且更灵活，有助于大规模铺设，还有一个原因是技术进步使网络链路间的传输可靠性大大提升，没必要在传输中的每一条链路上都保证可靠传输
	+ 网络只是尽其所能尽力地把数据准确传输到，但并不保证，让终端机自行确认保证

35. **1983年 TCP/IP 成为 ARPANE 上的标准协议，使所有使用 TCP/IP 的计算机能够通信，于是1983年被认为是互联网的诞生时间**

36. **路由器将收到的 packets 放在内存中而不是磁盘上，为的是保证效率**

37. **ICANN = Internet Corporation for Assigned Names and Numbers**
	+ 其职能在于为 ISP 分配 IP 地址，以及维护 DNS root server
	+ 它还为 AS 分配ASN

38. **子网 subnet 和 局域网 LAN 并不是同一个概念**
	+ 前者是一种层次化组织因特网使其便于管理和寻址的方式，后者是一种接入网络的技术

39. **DHCP = Dynamic Host Configuration Protocol**
	+ 用来动态配置子网中的 IP 地址，优点在于自动化；缺点在于，当用户从一个局域网切换到另一个时，涉及到段考-重新分配的过程会导致TCP断开

40. **Classless Interdomain Routing 与 Classful 分别是什么？**
	+ Classless 和 classful 都是 IPv4 的地址分配方式之一，之所以称为 classless，是为了和 classful 相对，后者即所谓 A，B，C 三类网络
	+ 后者的缺陷在于， IP address 浪费严重
	+ CIDR 使得 IP addr 块的分配更为灵活，一方面延缓了 IP address 的耗尽，另一方面减小了 forwarding table 的长度（因为前几位就够用了）

41. **TCP 协议并不是只能基于 IP，尽管经常如此，它还可以基于 ATM**

42. **电话网络很复杂（理论上肯定比计算机网络复杂），因为它连接的终端是功简单的 dumb end-systems，所以网络不得不承担更多复杂性**

43. **router 是用来跨接 subnets 的，一个 subnet 相当于一个 "岛"，在同一个 subnet 内的通信不需要 router 的参与**
	+ IP 的寻址，是在 subnets 间的寻址，而对于 subnet 内，并不是通过 IP 寻址的，而是用 MAC 寻址

44. **virtual circuit 和 datagram network 是对立的**
	+ 它们都由 router 实现，也就是说 router 不为 datagram network 而专设

45. **在 virtual circuit 中，IP address 是不需要的，因为一旦 VC 建立，直接发带 VC number 的内容就可以了**

46. **所谓的 routing protocol，其实就是用来维护 routing table 的，routing table 是 router 的核心**

47. **IETF 和 IEEE 之间的区别是什么？**
	+ IETF 主要负责 TCP/IP 协议栈中网络层和传输层的协议，以及一部分应用层的协议
	+ IEEE 主要负责链路层和物理层协议（毕竟这两层涉及到电气）

48. **Manchester Coding 被用在什么地方？是不是所有 link layer 都使用了？**
	+ 至少在 Ethernet 中是十分常用的

49. **IP address 是只为 IP 协议而设的，而网络并不仅仅为 IP 协议而建**

50. **CDMA 和 MAC 有什么区别？**
	+ CDMA 是用编码来区分是谁发的，而 MAC 是用 MAC 地址来区分是发给谁的
	+ CDMA 是用来划分信道的，MAC 是用来标识目的地址的

51. **不同的链路层协议的 MTU 不一样，而传输层又不应受此影响，因此太长的数据必须分段，这一分段要么由网络层完成，要么由链路层完成，总之不能由传输层完成**

52. **NAT = Network Address Translation**
	+ NAT 由 NAT router 来实现
	+ NAT router 的 IP addr 由 DHCP 向 ISP 申请
	+ hosts 从 NAT router 申请 IP 地址
	+ NAT 的本质是充分利用 port number 资源，因为毕竟网络上通信的其实是进程之间而不是主机之间
	+ NAT 开发了 port number 资源，缓解了 IPv4 addr 不够用的窘境，但也只是轻微缓解了
	+ NAT 的优势在于可以灵活地接入上网设备
	+ NAT 的缺陷在于：
		+ 把 host-to-host 通信变成了 port-to-port 通信
		+ 引入了中介，违背直接通信之旨
		+ 使 router 更加复杂，例如修改 HTTP request 的 IP addr/port num，这一操作本应是 application layer 的事，而 router 的初衷只是打算工作在 network layer

53. **P2P app 之所以不能直接运行在 NAT 网络上，是因为 P2P 要求二者之间维持 TCP 连接，然而 NAT 把 host 的 IP 地址屏蔽了**
	+ UPnP 是解决这一弊端的补丁，其原理是建立端口映射，让 NAT router 称为一个 relay

53. **子网掩码本身是个32位数，前 x 位为全1，后 32-x 位为全0，用于与 IP 地址搭配，将后者区分为网络地址和网络内主机地址**

54. **HTTP 没有 ACK 这类东西**
	+ 也就是说 HTTP 没有可靠传输机制，应该说基于 TCP 的应用层协议都不需要可靠传输，因为 TCP 已经做了

55. **virtual circuit 中采用的 VC number 是 router 的局部变量，可以让 router 自己替换并设置，这是因为如果让它是全局变量，就会涉及到统一和同步的问题，需要大量信息交换**

56. **switch 相当于一个高级的 hub，它的作用是分发局域网中的包，由其内部的 MAC addr-interface number 来实现**
	+ switch 工作在 link layer，其工作的参考依据是 MAC addr 而非 IP addr

57. **在一个 switch-based Ethernet LAN 中，不存在 collision，因此不再需要 MAC protocol**
	+ switch 通过分组转发实现了隔离从而避免了 collision

58. **6个字节全为 1 的 MAC addr 是局域网内广播地址**

59. **奇校验是让 data+parity bit 中包含奇数个1，偶校验是让 data+parity bit 中包含偶数个1**
	+ 奇偶校验不是十分可靠，因为实际上 bit error 往往出现 in burst 而非 independently，因此它只是作为教材上的引例，实际不用它

60. **计算机网络协议体系结构有下述三种：**
	+ OSI 的七层模型，但 OSI 模型并未成功应用
	+ TCP/IP 的四层协议模型，实际上互联网通用
		+ TCP/IP 协议族不只是 TCP+IP，而是一个包含了数据链路层、网络层、传输层的四层协议系统
		+ TCP/IP 协议族又称“互联网协议套件(Internet Protocol Suite, IPS)”
	+ 教材中的五层模型，仅仅是为了便于讲述而设计，实际上并未应用

61. **协议是“水平的”对等通信，而服务则是“垂直的”不对等的调用**

62. 

