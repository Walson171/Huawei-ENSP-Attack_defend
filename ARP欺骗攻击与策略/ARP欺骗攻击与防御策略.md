# ARP欺骗攻击与防御策略

## 环境拓扑

![1](D:\作业\华为ENSP攻防\ARP欺骗攻击与策略\imag\1.png)

## 实验步骤

#### 配置AR2

```basic
<Huawei>sys
[Huawei]sys AR2
[AR2]undo info en
[AR2]int g0/0/0
[AR2-GigabitEthernet0/0/0]ip add 192.168.1.254 255.255.255.0
[AR2-GigabitEthernet0/0/0]q
[AR2]int g0/0/1
[AR2-GigabitEthernet0/0/1]ip add 192.168.2.254 255.255.255.0
[AR2-GigabitEthernet0/0/1]q
[AR2]
```

#### ping各PC之间的情况

![2](D:\作业\华为ENSP攻防\ARP欺骗攻击与策略\imag\2.png)

![3](D:\作业\华为ENSP攻防\ARP欺骗攻击与策略\imag\3.png)

#### 查看路由器的mac和IP地址绑定情况(arp表)

![4](D:\作业\华为ENSP攻防\ARP欺骗攻击与策略\imag\4.png)

#### 修改pc3 IP地址为pc1 IP地址，并查看arp信息，清除arp

##### 1修改ip

![5](D:\作业\华为ENSP攻防\ARP欺骗攻击与策略\imag\5.png)

##### 2清除arp信息

arp -d 

![6](D:\作业\华为ENSP攻防\ARP欺骗攻击与策略\imag\6.png)

#### 使用PC3 ping PC4 查看AR1的arp表项

##### 1.使用PC3 ping PC4

![7](D:\作业\华为ENSP攻防\ARP欺骗攻击与策略\imag\7.png)

##### 2.AR1的arp表项

![8](D:\作业\华为ENSP攻防\ARP欺骗攻击与策略\imag\8.png)

#### PC3 IP地址重新修改为192.168.1.3,然后让PC4 ping 192.168.1.1

![image-20231018103127848](D:\作业\华为ENSP攻防\ARP欺骗攻击与策略\imag\9.png)

## 防御策略

##### 在AR2绑定网关与MAC地址映射关系

```basic
<AR2>sys
[AR2]user-bind static ip-address 192.168.1.1 MAC-address 5489-9851-2565
Info: 1 static user-bind item(s) added.
```

##### 在交换机SW3开启动态arp监测

```basic
<Huawei>sys
Enter system view, return user view with Ctrl+Z.
[Huawei]int g0/0/3
[Huawei-GigabitEthernet0/0/3]arp anti-attack check user-bind enable
```

##### 验证

将pc3的IP地址改为pc1后 无法ping通

![image-20231018104829939](D:\作业\华为ENSP攻防\ARP欺骗攻击与策略\imag\11.png)

![12](D:\作业\华为ENSP攻防\ARP欺骗攻击与策略\imag\12.png)

##### 查看AR2的arp表 

![image-20231018105124700](D:\作业\华为ENSP攻防\ARP欺骗攻击与策略\imag\13.png)

## 任务总结

1.交换机可以只绑定IP与ＭＡＣ地址关系，即执行如下操作

user-bind static ip-address 192.168.1.1 MAC-address 5489-9851-2565

2.为防范ARP攻击，假如管理员没有开启动态ARP监测(DAI)功能,在客户机上可以自己手动绑定网关IP与MAC地址关系以避免遭受攻击

3.ARP欺骗劫持不属于病毒木马，不能通过安装防病毒软件达到防御效果