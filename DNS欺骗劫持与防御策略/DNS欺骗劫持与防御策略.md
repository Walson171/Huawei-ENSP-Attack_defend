# DNS欺骗劫持与防御策略

## 环境拓扑

![image-20231018110554207](D:\作业\华为ENSP攻防\DNS欺骗劫持与防御策略\img\1.png)

## 基本配置

#### 接口IP与默认路由配置

R1

```basic
<Huawei>sys
[Huawei]sys R1
[R1]undo info en
[R1]int g0/0/0
[R1-GigabitEthernet0/0/0]ip add 192.168.1.1 24
[R1-GigabitEthernet0/0/0]q
[R1]int g0/0/1
[R1-GigabitEthernet0/0/1]ip add 192.168.2.1 24
[R1-GigabitEthernet0/0/1]q
[R1]rip 1
[R1-rip-1]version 2
[R1-rip-1]network 192.168.1.0
[R1-rip-1]network 192.168.2.0
[R1-rip-1]q
[R1]ip route-static 0.0.0.0 0.0.0.0 192.168.2.2
```

R2

```basic
<Huawei>sys
[Huawei]sys R2
[R2]undo info en
[R2]int g0/0/0
[R2-GigabitEthernet0/0/0]ip add 192.168.2.2 24
[R2-GigabitEthernet0/0/0]q
[R2]int g0/0/1
[R2-GigabitEthernet0/0/1]ip add 172.16.1.1 24 
[R2-GigabitEthernet0/0/1]q
[R2]int s2/0/0
[R2-Serial2/0/0]ip add 202.116.64.1 2
[R2-Serial2/0/0]q
[R2]rip 1
[R2-rip-1]version 2 
[R2-rip-1]network 192.168.2.0
[R2-rip-1]network 192.16.0.0
[R2-rip-1]q
[R2]ip route-static 0.0.0.0 0.0.0.0 serial 2/0/0
```

R3

```basic
<Huawei>sys
[Huawei]sys R3
[R3]undo info en
[R3]int s2/0/0
[R3-Serial2/0/0]ip add 202.116.64.2 24
[R3-Serial2/0/0]q
[R3]int g0/0/0
[R3-GigabitEthernet0/0/0]ip add 203.203.100.1 24
[R3-GigabitEthernet0/0/0]q
[R3]
```

#### 路由器R2 Rasy-IP 配置

```basic
[R2]acl 2000
[R2-acl-basic-2000]rule permit source 192.168.1.0 0.0.0.255
[R2-acl-basic-2000]rule permit source 192.168.2.0 0.0.0.255
[R2-acl-basic-2000]rule permit source 172.16.1.0 0.0.0.255
[R2-acl-basic-2000]q
[R2]int s2/0/0
[R2-Serial2/0/0]nat outbound 2000
[R2-Serial2/0/0]q
[R2]
```

#### 配置R2路由器DHCP服务 给技术部和工程部主机分配IP地址

```basic
[R2]dhcp enable 
[R2]ip pool jishu
[R2-ip-pool-jishu]network 192.168.1.0 mask 24
[R2-ip-pool-jishu]gateway-list 192.168.1.1 
[R2-ip-pool-jishu]dns-list 192.168.2.254
[R2-ip-pool-jishu]excluded-ip-address 192.168.1.2 192.168.1.9
[R2-ip-pool-jishu]q

[R2]ip pool gongcheng
[R2-ip-pool-gongcheng]network 172.16.1.0 mask 24
[R2-ip-pool-gongcheng]gateway-list 172.16.1.1
[R2-ip-pool-gongcheng]dns-list 192.168.2.254
[R2-ip-pool-gongcheng]excluded-ip-address 172.16.1.2 172.16.1.9
[R2-ip-pool-gongcheng]q

[R2]int g0/0/0
[R2-GigabitEthernet0/0/0]dhcp select global
[R2-GigabitEthernet0/0/0]q
[R2]
```

```basic
[R1]dhcp enable 
[R1]int g0/0/0
[R1-GigabitEthernet0/0/0]dhcp select relay
[R1-GigabitEthernet0/0/0]dhcp relay server-ip 192.168.2.2
[R1-GigabitEthernet0/0/0]q
[R1]
```

#### 配置百度服务器HttpServer

![image-20231018113218970](D:\作业\华为ENSP攻防\DNS欺骗劫持与防御策略\img\2.png)

#### 配置DNS Server

![image-20231018113328822](D:\作业\华为ENSP攻防\DNS欺骗劫持与防御策略\img\3.png)

#### 基本配置验证

技术部主机PC2 ping DNS服务器server1

![image-20231018113759609](D:\作业\华为ENSP攻防\DNS欺骗劫持与防御策略\img\4.png)

技术部主机PC2 ping baidu.com

![image-20231018113908230](D:\作业\华为ENSP攻防\DNS欺骗劫持与防御策略\img\5.png)

## 入侵实战

#### 网络拓扑结构图

![image-20231018114853921](D:\作业\华为ENSP攻防\DNS欺骗劫持与防御策略\img\6.png)

#### 伪造DHCP服务器R4配置

```basic
<Huawei>sys
[Huawei]sys R4
[R4]undo info en
[R4]int g0/0/0
[R4-GigabitEthernet0/0/0]ip add 192.168.1.9 24
[R4-GigabitEthernet0/0/0]q
[R4]dhcp enable 
[R4]ip pool forget
[R4-ip-pool-forget]network 192.168.1.0 mask 24
[R4-ip-pool-forget]gateway-list 192.168.1.1
[R4-ip-pool-forget]dns-list 192.168.1.8
[R4-ip-pool-forget]q
[R4]int g0/0/0
[R4-GigabitEthernet0/0/0]dhcp select global
[R4-GigabitEthernet0/0/0]q
[R4]
```

#### 伪造DHCP服务器配置

![image-20231018115739459](D:\作业\华为ENSP攻防\DNS欺骗劫持与防御策略\img\7.png)

#### arp欺骗

将pc3的IP地址改为pc2的IP地址去ping  baidu.com

![image-20231018120352131](D:\作业\华为ENSP攻防\DNS欺骗劫持与防御策略\img\8.png)

## 防御策略

#### 1.查看路由器R1的GE0/0/0接口(网关接口)MAC地址

![image-20231018221640687](D:\作业\华为ENSP攻防\DNS欺骗劫持与防御策略\img\9.png)

#### 2.在交换机SW1上绑定网关与MAC地址映射关系

```basic
<Huawei>sys
Enter system view, return user view with Ctrl+Z.
[Huawei]sys SW1
[SW1]undo info en
[SW1]user-bind static ip-address 192.168.1.1 MAC-address 00e0-fc81-6ae8
[SW1]user-bind static ip-address 192.168.1.1 MAC-address 00e0-fc81-6ae8 int g0/0/3 vlan 1
[SW1]vlan 1
[SW1-vlan1]arp anti-attack check user-bind enable 
[SW1-vlan1]q
[SW1]
```

## 任务总结

1.DNS欺骗劫持事件仅发生在局域网内。在IP规划时可通过可变长子网(VLSM)将一个网段划分成多个子网,限制广播域范围以减少此类攻击事件发生

2.DNS欺骗不属于病毒木马,不能通过安装防病毒软件避免此类攻击

3.计算机DNS缓存表不会立刻刷新,需等待一段时长.如需手动刷新,命令为ipconfig /flushdns
