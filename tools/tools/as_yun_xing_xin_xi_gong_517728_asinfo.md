# As 运行信息工具(asinfo)

asinfo它提供Aerospike集群指挥和控制功能。这包括的能力改变Aerospike运行时服务器配置参数。

>通过asinfo命令更改配置不会持久化到配置文件中。如果需要持久性改变，则需要手动添加到配置文件中。


## 用法
这个asinfo命名默认会安装在/usr/bin/asinfo，使用语法格式

>```asinfo [-h HOST] [-p PORT] [-v VALUE] [-l]```

| Option | Default | Description|
| -- | -- | -- |
| -h | localhost | IP Address or FQDN of the target Aerospike server. |
| -p | 3000 | 	Service port of the target Aerospike server. |
| -l | disabled | Replaced semicolons ';' in with line breaks in the response. |
| -v |  | Command to send to the target server. If not provided returns a default set of results. See Commands |


Example:
```
$ asinfo -v "namespaces"
requested value  namespaces
value is user_profile;test;bar
```

## Aerospike's Telnet Port
aerospike还提供Telnet服务通常配置为3003端口。这个服务提供为asinfo相同的功能，

Example:
```
$ telnet 127.0.0.1 3003
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
namespaces
user_profile;test;bar
```