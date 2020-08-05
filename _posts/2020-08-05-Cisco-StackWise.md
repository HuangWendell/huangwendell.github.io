---
layout:     post   				    # 使用的布局（不需要改）
title:      cisco stackwise virtual技术简介 				# 标题 
subtitle:   cisco堆叠技术 #副标题
date:       2020-08-05 				# 时间
author:     WD 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 网络学习
    - cisco
---





# Cisco StackWise  Virtual

[toc]



# SVL
#stack-mac persistent timer 修改mac保持的时间，一段时间后会跟随active变动
#stack-mac update force 修改stack的mac地址

![ha](https://github.com/HuangWendell/huangwendell.github.io/blob/master/img/image-20200426163043533.png?raw=true)

# 状态查看命令

show redundancy

show switch

stackwise vitual link：transfer both data and  control traffic between the peer switches；
通过virtual link 传输的数据会被添加StackWise virtual header

ether-channel的负载分担算法：
SVL(config)#port-channel load-balance ?
dst-ip Dst IP Addrdst-mac Dst Mac Addr

```sequence
title:堆叠连接图
SW01-->SW02:forty 1/0/23
SW01-->SW02:forty 1/0/24
Note left of SW01: virtual link for 1/0/23,24
SW02-->SW01:for 1/0/12
Note right of SW02:virtual dual-active detection

```



# SVL配置步骤

1. 两台交换机都需要配置StackWise virtual domain

   ```text
   SV-1#conf t
   SV-1(config)#stackwise-virtual
   SV-1(config-stackwise-virtual)#domain 100
   ```

2. 配置 StackWise Virtual link.

   ```tex
   SV-1(config)#interface range fortyGigabitEthernet 1/0/23-24
   SV-1(config-if-range)#stackwise-virtual link 1
   note:会自动的生成portchannel，并使用internal 协议;
   ```

3. 配置 StackWise Virtual dual-active detection

   ```tex
   SV-1(config)#interface fortyGigabitEthernet 1/0/12
   SV-1(config-if)#stackwise-virtual dual-active-detection
   ```

4. 保存配置并重启

   ```tex
   SV-1#wr 
   SV-1#reload
   ```

# SVL interface numbering

​	show interface status查看接口状态

| <INTERFACE_TYPE> | <SWITCH_ID>           | /<MODULE> | /<PORT> |
| ---------------- | --------------------- | --------- | ------- |
| 接口类型         | 交换机id              | 槽位      | 接口    |
| for 1/0/1        | 40G 交换机1 0槽 1号口 |           |         |

# 重启StackWise Virtual domain and its members

| Commands                         | Description                             |
| -------------------------------- | --------------------------------------- |
| reload/redundancy reload shelf   | 重启整个stackWise virtual domain，2只SW |
| SV-1#redundancy force-switchover | 重启active 并转换为standby              |
| redundancy force-switchover [id] | 指定交换机id重启                        |
| SV-1#reload standby-cpu          | 重启standby switch                      |
| redundancy reload peer           | 重启standby switch                      |

# 其他问题

## stackwise HA

![高可用性](https://github.com/HuangWendell/huangwendell.github.io/blob/master/img/image-20200426163335759.png?raw=true)

## 配置同步

​	通过virtual-link同步配置，standby 的nvram被重写；

## Virtual switch priorities and switch preempt

默认行为为(通过switch priorities feature可以修改)：

1. 优先初始化的switch 为acitve
2. 同时初始化的，mac地址小的为active

交换机优先级和抢占：switch priorities and preemption

默认不抢占，手工切换主备，优先级越大越好 配置命令：# switch 1 priority 15



## SV失效场景：Failure scenarios

### Sctive switch failure的感知方法：

1. redundancy framework heartbeats sent across the sv virtual link
2. satckwise discovery protocol
3. cisco generic online diagnositics（GOLD）failure event
4. full statckwise virtual link down

一旦检查到active 失效，hot-standby 执行SSO，并将自己设置为acitve

sso切换过程展示

![](https://github.com/HuangWendell/huangwendell.github.io/blob/master/img/image-20200426165632674.png?raw=true)



### Hot-standby switch failure 感知

1. redundancy framework heartbeats sent across the sv virtual link

2. cisco generic online diagnositics（GOLD）failure event

3. full statckwise virtual link down situation

一旦hot-standby失效，active 交换机会在线模拟一个an online insertion and removal (OIR) 

event for the SV standby switch，期间流向standby的数据会有到影响，上行带宽会减半

### dual active

双主检测机制：

1. Fast Hello detection

   ```sequence
   standby->active: standby send recovery message
   Note right of active:received message and goes in to recovery mode and error disables its ports and its mgmt if.
   ```

2. Enhanced PAgP

```text
SV-1(config)#stackwise-virtual
SV-1(config-stackwise-virtual)#dual-active detection pagp trust channel-group 20
SV-1#show stackwise-virtual dual-active-detection pagp
```

以上两种方法可同时部署实施；











