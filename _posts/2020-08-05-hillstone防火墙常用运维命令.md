---
layout:     post   				    # 使用的布局（不需要改）
title:      hillstone防火墙常用运维命令 				# 标题 
subtitle:   山石常用运维命令 #副标题
date:       2020-08-05 				# 时间
author:     WD 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 网络学习
---

# hillstone防火墙常用运维命令

1. HA traffic Feature： 

   HA traffic 针对在非对称路由环境中部署的HA设备启用，以确保会话的出战和入站在同一个设备上处理。

   用户可通过以下步骤在设备上配置HA traffic功能：

   1. 将两台HA设备配置为HA Peer组网模式；

   2. 启用HA traffic功能：

​			启用ha traffic：

​			ha traffic enable

2. 查看HA配置命令：

| show ha cluster                                              | 查看ha cluster配置                         |
| ------------------------------------------------------------ | ------------------------------------------ |
| show ha group {conf \| group-id}                             | 查看ha组配置信息                           |
| show ha link status                                          | 查看ha连接配置状态                         |
| show ha sync state {pki \| dns \|　dhcp \| vpn \| ntp\| flow \| scpvn \| l2tp \| route \| igmp} | 查看各个模块的ha同步状态                   |
| show ha sync state all                                       | 查看ha同步的总体同步状态及各模块的同步状态 |
| show ha traffic                                              | 查看ha traffic 状态                        |
| show ha sync statistic {pki \| dns \|　dhcp \| vpn \| ntp\| flow \| scpvn \| l2tp \| route \| igmp} | 查看ha同步统计信息                         |
| show ha sync rdo session config                              | 查看ha会话自动同步功能是否开启             |
| show ha protocol statiscitc                                  | 显示接收和发送ha协议的统计信息             |
| show session {sync \| unsync}                                | 显示已同步或未同步的ha会话信息             |
| show ha flow [[slot *slot-number*]  \| [cpu *cpu-number* statistics | 显示ha统计信息                             |
|                                                              |                                            |

3. 其他执行性质的命令

   1. exec ha master switch-over 任何模式下执行主备手动切换

   2. ha analysis-data multicast    备份设备的数据统计数据

   3. show ha apm state        查看统计数据备份状态