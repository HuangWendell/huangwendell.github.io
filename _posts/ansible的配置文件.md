## ansible的配置文件

- 位置/etc/ansible/ansible.cfg
- [default]配置
  -  #inventory #标识主机列表配置文件
  - #host_key_checking = False #检查对应服务器的host_key,建议取消
  - #log_path = /var/log/ansible.log #日志文件位置，建议开启

ansible命令

ansible-doc ：显示模块帮助

