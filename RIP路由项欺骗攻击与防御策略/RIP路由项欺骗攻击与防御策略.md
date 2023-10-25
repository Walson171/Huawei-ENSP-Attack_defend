# RIP路由项欺骗攻击与防御策略

## 任务目的

掌握基于RIP路由项欺骗攻击过程与RIP源端鉴别的配置方法。

## 任务设备、设施

win10、华为eNSP、vmvare、win7

## 任务拓扑结构图

![image-20231025122720934](D:\作业\华为ENSP攻防\RIP路由项欺骗攻击与防御策略\img\1.png)

## 基本配置

路由器R1接口IP与RIP路由配置

```basic
<Huawei>sys
[Huawei]sys R1
[R1]undo info en
Info: Information center is disabled.
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
[R1]
```

路由器R2接口接口IP与RIP路由配置

```basic
<Huawei>sys
Enter system view, return user view with Ctrl+Z.
[Huawei]sys R2
[R2]undo info en
Info: Information center is disabled.
[R2]int g0/0/0
[R2-GigabitEthernet0/0/0]ip add 192.168.2.2 24
[R2-GigabitEthernet0/0/0]q
[R2]int g0/0/1
[R2-GigabitEthernet0/0/1]ip add 192.168.3.1 24
[R2-GigabitEthernet0/0/1]q
[R2]rip 2
[R2-rip-2]version 2
[R2-rip-2]network 192.168.2.0
[R2-rip-2]network 192.168.3.0
[R2-rip-2]q
[R2]
```

路由R3接口IP与RIP路由配置

```basic
<Huawei>sys
Enter system view, return user view with Ctrl+Z.
[Huawei]sys R3
[R3]undo info en
Info: Information center is disabled.
[R3]int g0/0/0
[R3-GigabitEthernet0/0/0]ip add 192.168.3.2 24
[R3-GigabitEthernet0/0/0]q
[R3]int g0/0/1
[R3-GigabitEthernet0/0/1]ip add 192.168.4.1 24
[R3-GigabitEthernet0/0/1]q
[R3]rip 3
[R3-rip-3]version 2
[R3-rip-3]network 192.168.3.0
[R3-rip-3]network 192.168.4.0
[R3-rip-3]q
[R3]
```

查看路由器R1路由表

![image-20231025123804312](D:\作业\华为ENSP攻防\RIP路由项欺骗攻击与防御策略\img\2.png)

## 入侵实战

网络拓扑

![image-20231025124303116](D:\作业\华为ENSP攻防\RIP路由项欺骗攻击与防御策略\img\3.png)

路由器R4接口IP与RIP路由配置

```basic
<Huawei>sys
Enter system view, return user view with Ctrl+Z.
[Huawei]sys R4
[R4]undo info en
Info: Information center is disabled.
[R4]int g0/0/0
[R4-GigabitEthernet0/0/0]ip add 192.168.2.3 24
[R4-GigabitEthernet0/0/0]q
[R4]int g0/0/1
[R4-GigabitEthernet0/0/1]ip add 192.168.4.1 24
[R4-GigabitEthernet0/0/1]q
[R4]rip 4
[R4-rip-4]version 2
[R4-rip-4]network 192.168.2.0
[R4-rip-4]network 192.168.4.0
[R4-rip-4]q
[R4]
```

R4伪造后查看R1路由表

![image-20231025124642906](D:\作业\华为ENSP攻防\RIP路由项欺骗攻击与防御策略\img\4.png)

R2路由表

![image-20231025124743637](D:\作业\华为ENSP攻防\RIP路由项欺骗攻击与防御策略\img\5.png)

查看tracert测试结果

![image-20231025125027829](D:\作业\华为ENSP攻防\RIP路由项欺骗攻击与防御策略\img\6.png)

## 防御策略

在路由器R1接口开启RIP路由项源端鉴别功能

```basic
[R1]int g0/0/1
[R1-GigabitEthernet0/0/1]rip version 2 multicast 
[R1-GigabitEthernet0/0/1]rip authentication-mode hmac-sha256 cipher huawei 100
[R1-GigabitEthernet0/0/1]q
```

在路由器R2接口开启RIP路由项端鉴别功能

```basic
[R2]int g0/0/0
[R2-GigabitEthernet0/0/0]rip version 2 multicast 
[R2-GigabitEthernet0/0/0]rip authentication-mode hmac-sha256 cipher huawei 100
[R2-GigabitEthernet0/0/0]q
[R2]int g0/0/1
[R2-GigabitEthernet0/0/1]rip version 2 multicast 
[R2-GigabitEthernet0/0/1]rip authentication-mode hmac-sha256 cipher huawei 100
[R2-GigabitEthernet0/0/1]q
[R2]
```

在路由器R3接口开启RIP路由项源端鉴别功能

```basic
[R3]int g0/0/0
[R3-GigabitEthernet0/0/0]rip version 2 multicast 
[R3-GigabitEthernet0/0/0]rip authentication-mode hmac-sha256 cipher huawei 100
[R3-GigabitEthernet0/0/0]q
[R3]
```

任务验证

查看AR1路由表

![image-20231025125916250](D:\作业\华为ENSP攻防\RIP路由项欺骗攻击与防御策略\img\7.png)

查看tracert结果

![image-20231025130023436](D:\作业\华为ENSP攻防\RIP路由项欺骗攻击与防御策略\img\8.png)

## 任务总结

1.在配置RIP路由项源端鉴别时，相邻路由器之间接口必须使用相同摘要算法(如Hmac-SHA256)、相同的共享密钥(密钥存储方式可以不同,如cipher或者plain)和相同的密钥标识符,否则不能建立RIP邻居关系。

2.对于交换机SW2而言,去往IP地址为192.168.4.1的目的地时可能通过GE0/0/1接口(客户机与Web服务器通信时去跟回走不同路径),也可能通过GE 0/0/3接口(客户机与Web服务器通信时去跟回走相同路径),由SW2端口映射表更新状态决定,无法人为指定。