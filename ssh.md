## SSH

#### 基本用法
ssh -options [用户名]@[ip地址]

#### 配置key
1. 本地生成公钥私钥对 ssh-keygen -t <加密算法> -b <密钥长度> -C <注释，一般为邮箱地址>
    - e.g.: ssh-keygen -t rsa -b 2048 -C "wyh453255505@gmail.com"

    在本地~./ssh文件夹下会出现：
    - 公钥文件：id_<加密算法>.pub  例如    id_rsa.pub
    - 私钥文件：id_<加密算法>  例如    id_rsa

2. 本地公钥.pub文件内容，复制添加到远程主机的 ~/.ssh/authorized_keys文件中


#### options
- -p <port> 在指定的端口连接ssh，有时服务器端可能ssh默认端口被改变，不再是port22
- -C 压缩，当带宽比较小时候用，占用一些cpu
- -v 调试模式
- -N 登陆，不执行远程命令，仅仅转发端口
- -f 后台执行
- -t 不分配终端
- -L <local_port>:<host>:<host:port> 本地转发，正向代理, 对本机<local_port>的访问就相当于对remote_host的对应端口访问,通常来说，服务部署在远程，本机作为客户端。正向代理一般是客户端架设的。例如，在remote开启jupyter服务，本地用浏览器
打开jupyter。
  - ssh -L 8888:destiny_host:7777 username@bridge_host.  
     local_host----bridge_host----destiny_host  
      8888|______________________________|7777
  - ssh -L 8888:localhost:7777 username@bridge_host.    
  local_host----bridge_host  
   8888|___________|7777

- -R <wan_port>:localhost:<local_port> 远程转发，反向代理。多用于内网穿透，内网机器lan_host作为服务端，连接外网机器wan_host，并且将wan_port接收到的数据转发到本地或者本地可以连接到的一个host的对应端口。这样访问拥有公共ip的外网机的对应端口就可以访问到内网的对应端口，达到内网穿透的目的。反向代理一般是服务端架设的
 - ssh -R 8888:localhost:7777 username@wan_host
 - 对wan_host:8888的会被转发到localhost:7777，localhost相当于服务端，访问wan_host的为客户端连接

ssh完整option http://man.openbsd.org/ssh#T  
关于正向代理，反向代理 https://juejin.cn/post/6844903782556368910

#### ssh拷贝文件
scp -r username@192.168.0.1:/home/username/remotefile.txt ～/
