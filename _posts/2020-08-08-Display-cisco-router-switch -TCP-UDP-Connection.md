Display cisco router/switch TCP/UDP Connection

1. Use **show ip sockets command**   to verify that the socket being used is opening correctly. If there is a local and remote endpoint, a connection is established with the ports indicated.![show_ip_sockets.png](https://github.com/HuangWendell/huangwendell.github.io/blob/master/img/show_ip_sockets.png?raw=true)



2. To display a concise description of TCP connection endpoints, use the **show** **tcp** **brief** command in user EXEC or privileged EXEC mode. (显示路由器/交换机TCP的connection的简要信息,类似linux netstat -an)![show_tcp_brief.jpg](https://github.com/HuangWendell/huangwendell.github.io/blob/master/img/show_tcp_brief.jpg?raw=true)

