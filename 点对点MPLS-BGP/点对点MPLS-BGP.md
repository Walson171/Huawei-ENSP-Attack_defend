# 点对点MPLS-BGP

## 实验拓扑

![1](D:\作业\华为ENSP攻防\点对点MPLS-BGP\img\1.png)

## 基本配置

#### 路由器接口IP配置

CE1

```basic
<Huawei>sys
Enter system view, return user view with Ctrl+Z.
[Huawei]sys CE1
[CE1]undo info en 
Info: Information center is disabled.
[CE1]user-int con 0 
[CE1-ui-console0]idle 0 0 
[CE1-ui-console0]q
[CE1]int g0/0/1
[CE1-GigabitEthernet0/0/1]ip add 192.168.1.1 24
[CE1-GigabitEthernet0/0/1]q
[CE1]int g0/0/0
[CE1-GigabitEthernet0/0/0]ip add 201.201.201.2 24
[CE1-GigabitEthernet0/0/0]q
[CE1]
```

CE2

```basic
<Huawei>sys
Enter system view, return user view with Ctrl+Z.
[Huawei]sys CE2
[CE2]undo info en 
Info: Information center is disabled.
[CE2]user-int con 0 
[CE2-ui-console0]idle 0 0 
[CE2-ui-console0]q
[CE2]int g0/0/1
[CE2-GigabitEthernet0/0/1]ip add 192.168.2.1 24 
[CE2-GigabitEthernet0/0/1]q
[CE2]int g0/0/0 
[CE2-GigabitEthernet0/0/0]ip add 202.202.202.2 24 
[CE2-GigabitEthernet0/0/0]
```

CE3

```basic
<Huawei>sys
Enter system view, return user view with Ctrl+Z.
[Huawei]sys CE3
[CE3]undo info en 
Info: Information center is disabled.
[CE3]user-int con 0
[CE3-ui-console0]idle 0 0 
[CE3-ui-console0]int g0/0/1
[CE3-GigabitEthernet0/0/1]ip add 192.168.1.1 24 
[CE3-GigabitEthernet0/0/1]q
[CE3]int g0/0/0
[CE3-GigabitEthernet0/0/0]ip add 203.203.203.2 24
[CE3-GigabitEthernet0/0/0]q
[CE3]
```

CE4

```basic
<Huawei>sys
Enter system view, return user view with Ctrl+Z.
[Huawei]sys CE4
[CE4]undo info en 
Info: Information center is disabled.
[CE4]user-int con 0 
[CE4-ui-console0]idle 0 0 
[CE4-ui-console0]q
[CE4]int g0/0/0
[CE4-GigabitEthernet0/0/0]ip add 204.204.204.2 24
[CE4-GigabitEthernet0/0/0]q
[CE4]int g0/0/1 
[CE4-GigabitEthernet0/0/1]ip add 192.168.2.1 24 
[CE4-GigabitEthernet0/0/1]q
[CE4]
```

PE1

```basic
<Huawei>sys
Enter system view, return user view with Ctrl+Z.
[Huawei]sys PE1
[PE1]undo info en 
Info: Information center is disabled.
[PE1]user-int con 0 
[PE1-ui-console0]idle 0 0 
[PE1-ui-console0]q
[PE1]int g0/0/1
[PE1-GigabitEthernet0/0/1]ip add 201.201.201.1 24 
[PE1-GigabitEthernet0/0/1]q
[PE1]int g0/0/0
[PE1-GigabitEthernet0/0/0]ip add 116.64.64.1 24
[PE1-GigabitEthernet0/0/0]q
[PE1]int g2/0/0
[PE1-GigabitEthernet2/0/0]ip add 203.203.203.1 24
[PE1-GigabitEthernet2/0/0]q
[PE1]int loopback 0
[PE1-LoopBack0]ip add 10.0.1.1 24 
[PE1-LoopBack0]
[PE1]
```

PE2

```basic
<Huawei>sys
Enter system view, return user view with Ctrl+Z.
[Huawei]sys PE2
[PE2]undo info en 
Info: Information center is disabled.
[PE2]user-int con 0
[PE2-ui-console0]idle 0 0
[PE2-ui-console0]q
[PE2]int g0/0/1
[PE2-GigabitEthernet0/0/1]ip add 202.202.202.1 24 
[PE2-GigabitEthernet0/0/1]q
[PE2]int g0/0/0
[PE2-GigabitEthernet0/0/0]ip add 118.16.16.2 24 
[PE2-GigabitEthernet0/0/0]q
[PE2]int g2/0/0
[PE2-GigabitEthernet2/0/0]ip add 204.204.204.1 24 
[PE2-GigabitEthernet2/0/0]q
[PE2]int loopback 0 
[PE2-LoopBack0]ip add 10.0.4.4 24 
[PE2-LoopBack0]
```

P1

```basic
<Huawei>sys
Enter system view, return user view with Ctrl+Z.
[Huawei]sys p1
[p1]undo info en 
Info: Information center is disabled.
[p1]user-int con 0 
[p1-ui-console0]idle 0 0 
[p1-ui-console0]q
[p1]int g0/0/0 
[p1-GigabitEthernet0/0/0]ip add 116.64.64.2 24 
[p1-GigabitEthernet0/0/0]q
[p1]int g0/0/1
[p1-GigabitEthernet0/0/1]ip add 117.32.32.1 24 
[p1-GigabitEthernet0/0/1]q
[p1]int loopback 0
[p1-LoopBack0]ip add 10.0.2.2 24 
[p1-LoopBack0]q
[p1]
```

P2

```basic
<Huawei>sys
Enter system view, return user view with Ctrl+Z.
[Huawei]sys P2
[P2]undo info en 
Info: Information center is disabled.
[P2]user-int con 0 
[P2-ui-console0]idle 0 0 
[P2-ui-console0]q
[P2]int g0/0/1
[P2-GigabitEthernet0/0/1]ip add 117.32.32.2 24 
[P2-GigabitEthernet0/0/1]q
[P2]int g0/0/0
[P2-GigabitEthernet0/0/0]ip add 118.16.16.1 24 
[P2-GigabitEthernet0/0/0]q
[P2]int loopback 0
[P2-LoopBack0]ip add 10.0.3.3 24 
[P2-LoopBack0]q
[P2]
```

### 配置运营商内网OSPF路由

PE1

```basic
[PE1]ospf router-id 10.0.1.1 
[PE1-ospf-1]area 0
[PE1-ospf-1-area-0.0.0.0]network 116.64.64.0 0.0.0.255
[PE1-ospf-1-area-0.0.0.0]network 10.0.1.1 0.0.0.0
[PE1-ospf-1-area-0.0.0.0]q
[PE1-ospf-1]q
[PE1]
```

PE2

```basic
[PE2]ospf router-id 10.0.4.4
[PE2-ospf-1]area 0
[PE2-ospf-1-area-0.0.0.0]network 118.16.16.0 0.0.0.255
[PE2-ospf-1-area-0.0.0.0]network 10.0.4.4 0.0.0.0
[PE2-ospf-1-area-0.0.0.0]q
[PE2-ospf-1]q
[PE2]
```

P1

```basic
[p1]ospf router-id 10.0.2.2 
[p1-ospf-1]area 0
[p1-ospf-1-area-0.0.0.0]network 116.64.64.0 0.0.0.255
[p1-ospf-1-area-0.0.0.0]network 117.32.32.0 0.0.0.255
[p1-ospf-1-area-0.0.0.0]network 10.0.2.2 0.0.0.0
[p1-ospf-1-area-0.0.0.0]q
[p1-ospf-1]q
[p1]
```

P2

```basic
[P2]ospf router-id 10.0.3.3
[P2-ospf-1]area 0
[P2-ospf-1-area-0.0.0.0]network 117.32.32.0 0.0.0.255
[P2-ospf-1-area-0.0.0.0]network 118.16.16.0 0.0.0.255
[P2-ospf-1-area-0.0.0.0]network 10.0.3.3 0.0.0.0 
[P2-ospf-1-area-0.0.0.0]q
[P2-ospf-1]q
[P2]
```

### OSPF路由配置完成后，在路由器P1和P2查看邻居关系

P1

![image-20231113105546346](D:\作业\华为ENSP攻防\点对点MPLS-BGP\img\2.png)

P2

![image-20231113105635714](D:\作业\华为ENSP攻防\点对点MPLS-BGP\img\3.png)

测试PE1的连通性

![image-20231113105806393](D:\作业\华为ENSP攻防\点对点MPLS-BGP\img\4.png)

### 配置PE1与PE2之间MP-BGP邻居关系

PE1与PE2通过Loopback0虚拟接口建立IBGP邻居关系

```basic
[PE1]bgp 500
[PE1-bgp]peer 10.0.4.4 as-number 500
[PE1-bgp]peer 10.0.4.4 connect-interface loopback 0
[PE1-bgp]
```

```basic
[PE2]bgp 500
[PE2-bgp]peer 10.0.1.1 as-number 500
[PE2-bgp]peer 10.0.1.1 conn	
[PE2-bgp]peer 10.0.1.1 connect-interface loopback 0
[PE2-bgp]
```

在AR1上查看BGP邻居关系

```basic
[PE1-bgp]disp bgp peer

 BGP local router ID : 201.201.201.1
 Local AS number : 500
 Total number of peers : 1		  Peers in established state : 1

Peer            V          AS  MsgRcvd  MsgSent  OutQ  Up/Down       State         Pre fRcv
10.0.4.4        4         500        3        5     0  00:01:14      Established          0
```

在PE1与PE2启用IPv4-Family子族VPNv4地址族，允许PE1与PE2之间交换VPNv4路由信息

```basic
[PE1-bgp]ipv4	
[PE1-bgp]ipv4-family vpnv4
[PE1-bgp-af-vpnv4]peer 10.0.4.4 en
[PE1-bgp-af-vpnv4]peer 10.0.4.4 advertise-community 
[PE1-bgp-af-vpnv4]q
[PE1-bgp]q
[PE1]
```

```basic
[PE2-bgp]ipv4-family vpnv4
[PE2-bgp-af-vpnv4]peer 10.0.1.1 en
[PE2-bgp-af-vpnv4]peer 10.0.1.1 advertise-community 
[PE2-bgp-af-vpnv4]q
[PE2-bgp]q
[PE2]
```

### 在处于LSP路径上的四个路由器上启用LDP标签自动分发协议

PE1

```basic
[PE1]mpls lsr-id 10.0.1.1 
[PE1]mpls
Info: Mpls starting, please wait... OK!
[PE1-mpls]mpls ldp
[PE1-mpls-ldp]q
[PE1]int g0/0/0
[PE1-GigabitEthernet0/0/0]mpls
[PE1-GigabitEthernet0/0/0]mpls ldp
[PE1-GigabitEthernet0/0/0]q
[PE1]
```

PE2

```basic
[PE2]mpls lsr-id 10.0.4.4 
[PE2]mpls
Info: Mpls starting, please wait... OK!
[PE2-mpls]mpls ldp
[PE2-mpls-ldp]q
[PE2]int g0/0/0
[PE2-GigabitEthernet0/0/0]mpls
[PE2-GigabitEthernet0/0/0]mpls ldp
[PE2-GigabitEthernet0/0/0]q
[PE2]
```

P1

```basic
[p1]mpls lsr-id 10.0.2.2
[p1]mpls
Info: Mpls starting, please wait... OK!
[p1-mpls]mpls ldp
[p1-mpls-ldp]q
[p1]int g0/0/0
[p1-GigabitEthernet0/0/0]mpls
[p1-GigabitEthernet0/0/0]mpls ldp
[p1-GigabitEthernet0/0/0]q
[p1]int g0/0/1
[p1-GigabitEthernet0/0/1]mpls
[p1-GigabitEthernet0/0/1]mpls ldp
[p1-GigabitEthernet0/0/1]q
[p1]
```

P2

```basic
[P2]mpls lsr-id 10.0.3.3
[P2]mpls 
Info: Mpls starting, please wait... OK!
[P2-mpls]mpls ldp 
[P2-mpls-ldp]q
[P2]int g0/0/0
[P2-GigabitEthernet0/0/0]mpls
[P2-GigabitEthernet0/0/0]mpls ldp
[P2-GigabitEthernet0/0/0]q
[P2]int g0/0/1
[P2-GigabitEthernet0/0/1]mpls
[P2-GigabitEthernet0/0/1]mpls ldp 
[P2-GigabitEthernet0/0/1]q
[P2]
```

### 验证LSP路径上四个路由器LDP会话状态

P1

![image-20231113112129690](D:\作业\华为ENSP攻防\点对点MPLS-BGP\img\5.png)

P2

![6](D:\作业\华为ENSP攻防\点对点MPLS-BGP\img\6.png)

#### 查看MPLS标签转发表和标签交换路径

在PE1查看MPLS标签转发表

![image-20231113112437747](D:\作业\华为ENSP攻防\点对点MPLS-BGP\img\7.png)

在PE2查看MPLS标签转发表

![image-20231113112545518](D:\作业\华为ENSP攻防\点对点MPLS-BGP\img\8.png)

在P1查看完整标签交换路径LSP表

![image-20231113112816296](D:\作业\华为ENSP攻防\点对点MPLS-BGP\img\9.png)

### 在路由器PE1创建A公司VPN实例，并与接口绑定

```basic
[PE1]ip vpn-instance vpn_company_A
[PE1-vpn-instance-vpn_company_A]ipv4-family
[PE1-vpn-instance-vpn_company_A-af-ipv4]route-distinguisher 100:1
[PE1-vpn-instance-vpn_company_A-af-ipv4]vpn-target 20:1 export-extcommunity 
 EVT Assignment result: 
Info: VPN-Target assignment is successful.	
[PE1-vpn-instance-vpn_company_A-af-ipv4]vpn-target 20:1 import-extcommunity 
 IVT Assignment result: 
Info: VPN-Target assignment is successful.


[PE1-vpn-instance-vpn_company_A-af-ipv4]q
[PE1-vpn-instance-vpn_company_A]q
[PE1]int g0/0/1
[PE1-GigabitEthernet0/0/1]ip binding vpn-instance vpn_company_A
Info: All IPv4 related configurations on this interface are removed!
Info: All IPv6 related configurations on this interface are removed!
[PE1-GigabitEthernet0/0/1]ip add 201.201.201.1 24
[PE1-GigabitEthernet0/0/1]q


[PE1]ping -vpn-instance vpn_company_A 201.201.201.2
  PING 201.201.201.2: 56  data bytes, press CTRL_C to break
    Reply from 201.201.201.2: bytes=56 Sequence=1 ttl=255 time=60 ms
    Reply from 201.201.201.2: bytes=56 Sequence=2 ttl=255 time=20 ms
    Reply from 201.201.201.2: bytes=56 Sequence=3 ttl=255 time=20 ms
    Reply from 201.201.201.2: bytes=56 Sequence=4 ttl=255 time=30 ms
    Reply from 201.201.201.2: bytes=56 Sequence=5 ttl=255 time=20 ms

  --- 201.201.201.2 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 20/30/60 ms
```

### 在PE1上创建B公司VPN实例并与接口绑定

```basic
[PE1]ip vpn-instance vpn_company_B
[PE1-vpn-instance-vpn_company_B]ipv4-family
[PE1-vpn-instance-vpn_company_B-af-ipv4]route-distinguisher 300:1
[PE1-vpn-instance-vpn_company_B-af-ipv4]vpn-target 20:2 both
 IVT Assignment result: 
Info: VPN-Target assignment is successful.
 EVT Assignment result: 
Info: VPN-Target assignment is successful.
[PE1-vpn-instance-vpn_company_B-af-ipv4]q
[PE1-vpn-instance-vpn_company_B]q
[PE1]int g2/0/0
[PE1-GigabitEthernet2/0/0]ip binding vpn-instance vpn_company_B
Info: All IPv4 related configurations on this interface are removed!
Info: All IPv6 related configurations on this interface are removed!
[PE1-GigabitEthernet2/0/0]ip add 203.203.203.1 24 
[PE1-GigabitEthernet2/0/0]q
[PE1]
```

### 在PE2上创建A公司VPN实例并与接口绑定

```basic
[PE2]ip vpn-instance vpn_company_A
[PE2-vpn-instance-vpn_company_A]ipv4-family	
[PE2-vpn-instance-vpn_company_A-af-ipv4]route-distinguisher 200:1
[PE2-vpn-instance-vpn_company_A-af-ipv4]vpn-target 20:1 both
 IVT Assignment result: 
Info: VPN-Target assignment is successful.
 EVT Assignment result: 
Info: VPN-Target assignment is successful.
[PE2-vpn-instance-vpn_company_A-af-ipv4]q
[PE2-vpn-instance-vpn_company_A]q
[PE2]int g0/0/1
[PE2-GigabitEthernet0/0/1]ip binding vpn-instance vpn_company_A
Info: All IPv4 related configurations on this interface are removed!
Info: All IPv6 related configurations on this interface are removed!
[PE2-GigabitEthernet0/0/1]ip add 202.202.202.1 24
[PE2-GigabitEthernet0/0/1]q
[PE2]
```

### 在PE2上创建B公司VPN实例并与接口进行绑定

```basic
[PE2]ip vpn-instance vpn_company_B
[PE2-vpn-instance-vpn_company_B]ipv4-family 
[PE2-vpn-instance-vpn_company_B-af-ipv4]route-distinguisher 400:1
[PE2-vpn-instance-vpn_company_B-af-ipv4]vpn-target 20:2 both
 IVT Assignment result: 
Info: VPN-Target assignment is successful.
 EVT Assignment result: 
Info: VPN-Target assignment is successful.
[PE2-vpn-instance-vpn_company_B-af-ipv4]q
[PE2-vpn-instance-vpn_company_B]q
[PE2]int g2/0/0	
[PE2-GigabitEthernet2/0/0]ip binding vpn-instance vpn_company_B
Info: All IPv4 related configurations on this interface are removed!
Info: All IPv6 related configurations on this interface are removed!
[PE2-GigabitEthernet2/0/0]ip add 204.204.204.1 24
[PE2-GigabitEthernet2/0/0]q
[PE2]
```

### CE1与PE1,CE2与PE2建立BGP邻居并通告公司A私网路由

CE1与PE1建立EBGP邻居关系

```
[CE1]bgp 100
[CE1-bgp]peer 201.201.201.1 as-number 500
[CE1-bgp]network 192.168.1.0
```

```basic
[PE1]bgp 500
[PE1-bgp]ipv4-family vpn-ins	
[PE1-bgp]ipv4-family vpn-instance vpn_company_A
[PE1-bgp-vpn_company_A]peer 201.201.201.2 as-number 100
[PE1-bgp-vpn_company_A]
```

```basic
[CE1-bgp]disp bgp peer

 BGP local router ID : 192.168.1.1
 Local AS number : 100
 Total number of peers : 1		  Peers in established state : 1

Peer            V          AS  MsgRcvd  MsgSent  OutQ  Up/Down       State          Pre fRcv
201.201.201.1   4          500       2        5     0  00:00:07      Established    0
[CE1-bgp]


[PE1]disp bgp peer

 BGP local router ID : 201.201.201.1
 Local AS number : 500
 Total number of peers : 1		  Peers in established state : 1

Peer            V          AS  MsgRcvd  MsgSent  OutQ  Up/Down       State          Pre fRcv
10.0.4.4        4          500      47       49     0   00:43:15     Established    0
[PE1]


[PE1]disp bgp vpnv4 vpn-instance vpn_company_A peer

 BGP local router ID : 201.201.201.1
 Local AS number : 500

 VPN-Instance vpn_company_A, Router ID 201.201.201.1:
 Total number of peers : 1		  Peers in established state : 1

Peer            V          AS  MsgRcvd  MsgSent  OutQ  Up/Down       State         Pre fRcv
201.201.201.2   4         100        4        3     0  00:01:56      Established          1
[PE1]
```

CE2与PE2建立EBGP邻居关系

```basic
[CE2]bgp 200
[CE2-bgp]peer 202.202.202.1 as-number 500
[CE2-bgp]network 192.168.2.0
[CE2-bgp]
```

```basic
[PE2]bgp 500
[PE2-bgp]ipv4-family vpn-instance vpn_company_A
[PE2-bgp-vpn_company_A]peer 202.202.202.2 as-number 200
[PE2-bgp-vpn_company_A]
```

```basic
[PE1]disp bgp vpnv4 vpn-instance vpn_company_A routing-table 

 BGP Local router ID is 201.201.201.1 
 Status codes: * - valid, > - best, d - damped,
               h - history,  i - internal, s - suppressed, S - Stale
               Origin : i - IGP, e - EGP, ? - incomplete


 VPN-Instance vpn_company_A, Router ID 201.201.201.1:

 Total Number of Routes: 2
      Network            NextHop        MED        LocPrf    PrefVal Path/Ogn

 *>   192.168.1.0        201.201.201.2   0                     0      100i
 *>i  192.168.2.0        10.0.4.4        0          100        0      200i
[PE1]


[PE2]disp bgp vpnv4 vpn-instance vpn_company_A routing-table 

 BGP Local router ID is 202.202.202.1 
 Status codes: * - valid, > - best, d - damped,
               h - history,  i - internal, s - suppressed, S - Stale
               Origin : i - IGP, e - EGP, ? - incomplete


 VPN-Instance vpn_company_A, Router ID 202.202.202.1:

 Total Number of Routes: 2
      Network            NextHop        MED        LocPrf    PrefVal Path/Ogn

 *>i  192.168.1.0        10.0.1.1        0          100        0      100i
 *>   192.168.2.0        202.202.202.2   0                     0      200i
[PE2]
```

### 相互通告私网络后 在PE1和PE2查看公司A基于MPLS VPN完整标注路径

![image-20231113120027599](D:\作业\华为ENSP攻防\点对点MPLS-BGP\img\10.png)

![image-20231113120110708](D:\作业\华为ENSP攻防\点对点MPLS-BGP\img\11.png)

## 任务验证

在主机1命令行输入ping 192.168.2.10 可以连通服务器1

![image-20231113134341524](D:\作业\华为ENSP攻防\点对点MPLS-BGP\img\12.png)

### CE3与PE1，CE4与PE2建立BGP邻居并通告公司B私网路由

CE3与PE1建立EBGP

```basic
[CE3]bgp 300
[CE3-bgp]peer 203.203.203.1 as-number 500
[CE3-bgp]network 192.168.1.0 
```

```basic
[PE1]bgp 500
[PE1-bgp]ipv4-family vpn-instance vpn_company_B
[PE1-bgp-vpn_company_B]peer 203.203.203.2 as-number 300
[PE1-bgp-vpn_company_B]
```

CE4与PE2建立EBGP邻居关系

```basic
[CE4]bgp 400
[CE4-bgp]peer 204.204.204.1 as-number 500
[CE4-bgp]network 192.168.2.0
[CE4-bgp]
```

```basic
[PE2]bgp 500
[PE2-bgp]ipv4-family vpn-instance vpn_company_B
[PE2-bgp-vpn_company_B]peer 204.204.204.2 as-number 400
[PE2-bgp-vpn_company_B]
```

在PE1和PE2查看vpn_company_B实例的VPNv4路由表

```basic
[PE1]disp bgp vpnv4 vpn-instance vpn_company_B routing-table

 BGP Local router ID is 116.64.64.1 
 Status codes: * - valid, > - best, d - damped,
               h - history,  i - internal, s - suppressed, S - Stale
               Origin : i - IGP, e - EGP, ? - incomplete


 VPN-Instance vpn_company_B, Router ID 116.64.64.1:

 Total Number of Routes: 2
      Network            NextHop        MED        LocPrf    PrefVal Path/Ogn

 *>   192.168.1.0        203.203.203.2   0                     0      300i
 *>i  192.168.2.0        10.0.4.4        0          100        0      400i
[PE1]
```

```basic
[PE2]disp bgp vpnv4 vpn-instance vpn_company_B routing-table

 BGP Local router ID is 118.16.16.2 
 Status codes: * - valid, > - best, d - damped,
               h - history,  i - internal, s - suppressed, S - Stale
               Origin : i - IGP, e - EGP, ? - incomplete


 VPN-Instance vpn_company_B, Router ID 118.16.16.2:

 Total Number of Routes: 2
      Network            NextHop        MED        LocPrf    PrefVal Path/Ogn

 *>i  192.168.1.0        10.0.1.1        0          100        0      300i
 *>   192.168.2.0        204.204.204.2   0                     0      400i
[PE2]
```

### 相互通告私网路由后，在PE1和PE2查看公司B基于MPLS VPN完整标签路径

![image-20231113135648670](D:\作业\华为ENSP攻防\点对点MPLS-BGP\img\13.png)

![image-20231113135748920](D:\作业\华为ENSP攻防\点对点MPLS-BGP\img\14.png)

## 任务验证

在主机2命令行输入命令ping 192.168.2.20,可以连通服务器2

![image-20231113135923769](D:\作业\华为ENSP攻防\点对点MPLS-BGP\img\15.png)

在主机2命令行输入命令ping 192.168.2.10，不可以连通服务器1

![image-20231113140117289](D:\作业\华为ENSP攻防\点对点MPLS-BGP\img\16.png)

## 防范策略

1.可在服务器上部署IIS(Internet Information Services,互联网信息服务)时选择https协议，通过SSL证书对Web站点数据加密

2.MPLS属于2.5层VPN，不能使用IPSec(第三层协议)保护MPLS报文安全性,即不可能有MPLS Over IPSec技术。

3.路由器只能保证数据包的完整性和来源的可靠性,不对其内部具体数据负责,也不负责弥补非路由器转发导致的安全漏洞

## 任务总结

1.对于所有企业而言，一般认为数据包在自身内网中传输是安全的，而在公网中传输是不安全的。MPLS优点是转发效率高，低延迟，但缺少安全协议，难以保证MPLS报文在公网中传输的安全性

2.IPSec(Internet Protocol Security,互联网安全协议)属于第三层协议，通过重新封装IP头部字段并实施加密以实现数据包在公网中传输的安全性。但MPLS属于2.5层隧道，路由器不会拆封MPLS报文的IP头部字段并查询IP路由表转发(MPLS报文IP头部目的地址为私网IP，即使拆封也无法在公网中投递),而是采用类似二层交换机的方式对MPLS报文标签查询转发,因此不能通过IPSec协议保证其在公网投递的安全性。