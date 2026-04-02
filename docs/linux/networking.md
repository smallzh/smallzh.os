# Linux网络工具

## 0x01 网络连通性测试

### ping - 网络连通性测试
```bash
# 基本ping测试
ping google.com

# 指定次数
ping -c 5 google.com

# 指定间隔（秒）
ping -i 2 google.com

# 指定包大小
ping -s 1000 google.com

# 指定超时时间
ping -W 5 google.com

# 持续ping（按Ctrl+C停止）
ping -i 0.5 google.com
```

### traceroute - 路由跟踪
```bash
# 基本路由跟踪
traceroute google.com

# 指定最大跳数
traceroute -m 15 google.com

# 使用ICMP模式
traceroute -I google.com

# 指定端口
traceroute -p 80 google.com
```

### mtr - 网络诊断工具
```bash
# 基本网络诊断
mtr google.com

# 报告模式
mtr -r google.com

# 指定次数
mtr -c 10 google.com

# 显示IP地址
mtr -n google.com
```

## 0x02 网络配置查看

### ip - 网络配置
```bash
# 显示所有网络接口
ip addr

# 显示特定接口
ip addr show eth0

# 显示路由表
ip route

# 显示邻居表（ARP缓存）
ip neigh

# 添加IP地址
sudo ip addr add 192.168.1.100/24 dev eth0

# 删除IP地址
sudo ip addr del 192.168.1.100/24 dev eth0

# 启用接口
sudo ip link set eth0 up

# 禁用接口
sudo ip link set eth0 down
```

### ifconfig - 网络配置（旧版）
```bash
# 显示所有接口
ifconfig

# 显示特定接口
ifconfig eth0

# 配置IP地址
sudo ifconfig eth0 192.168.1.100 netmask 255.255.255.0

# 启用接口
sudo ifconfig eth0 up

# 禁用接口
sudo ifconfig eth0 down
```

### route - 路由表管理
```bash
# 显示路由表
route -n

# 添加默认网关
sudo route add default gw 192.168.1.1

# 添加静态路由
sudo route add -net 10.0.0.0/24 gw 192.168.1.1

# 删除路由
sudo route del -net 10.0.0.0/24
```

## 0x03 网络连接查看

### netstat - 网络统计
```bash
# 显示所有连接
netstat -a

# 显示TCP连接
netstat -at

# 显示UDP连接
netstat -au

# 显示监听端口
netstat -l

# 显示进程和PID
netstat -p

# 显示数字地址
netstat -n

# 显示统计信息
netstat -s

# 显示路由表
netstat -r
```

### ss - 套接字统计
```bash
# 显示所有连接
ss -a

# 显示TCP连接
ss -t

# 显示UDP连接
ss -u

# 显示监听端口
ss -l

# 显示进程信息
ss -p

# 显示数字地址
ss -n

# 过滤特定端口
ss -t sport = :80

# 过滤特定状态
ss -t state established
```

### lsof - 列出打开的文件
```bash
# 显示所有打开的网络文件
lsof -i

# 显示特定端口
lsof -i :80

# 显示特定协议
lsof -i tcp

# 显示特定IP的连接
lsof -i @192.168.1.100

# 显示特定用户的网络连接
lsof -i -u username
```

## 0x04 DNS工具

### nslookup - DNS查询
```bash
# 基本DNS查询
nslookup google.com

# 指定DNS服务器
nslookup google.com 8.8.8.8

# 查询MX记录
nslookup -type=MX google.com

# 查询NS记录
nslookup -type=NS google.com

# 交互模式
nslookup
> set type=A
> google.com
```

### dig - DNS查询
```bash
# 基本DNS查询
dig google.com

# 指定DNS服务器
dig @8.8.8.8 google.com

# 查询特定记录类型
dig google.com MX
dig google.com NS
dig google.com AAAA

# 简短输出
dig +short google.com

# 反向查询
dig -x 8.8.8.8
```

### host - DNS查询
```bash
# 基本DNS查询
host google.com

# 查询特定记录类型
host -t MX google.com
host -t NS google.com

# 指定DNS服务器
host google.com 8.8.8.8
```

## 0x05 数据传输工具

### curl - 网络数据传输
```bash
# 基本GET请求
curl https://example.com

# 保存到文件
curl -o file.html https://example.com

# 跟随重定向
curl -L https://example.com

# 显示响应头
curl -I https://example.com

# POST请求
curl -X POST -d "data" https://example.com

# 上传文件
curl -F "file=@file.txt" https://example.com/upload

# 设置请求头
curl -H "Content-Type: application/json" https://example.com

# 使用代理
curl -x http://proxy:8080 https://example.com

# 下载文件
curl -O https://example.com/file.zip
```

### wget - 文件下载
```bash
# 下载文件
wget https://example.com/file.zip

# 保存为特定名称
wget -O myfile.zip https://example.com/file.zip

# 后台下载
wget -b https://example.com/file.zip

# 继续中断的下载
wget -c https://example.com/file.zip

# 递归下载
wget -r https://example.com/

# 限制下载速度
wget --limit-rate=1m https://example.com/file.zip

# 下载并转换链接
wget -k -p https://example.com/
```

### scp - 安全复制
```bash
# 本地到远程
scp file.txt user@remote:/path/

# 远程到本地
scp user@remote:/path/file.txt .

# 递归复制目录
scp -r dir/ user@remote:/path/

# 指定端口
scp -P 2222 file.txt user@remote:/path/

# 保留属性
scp -p file.txt user@remote:/path/
```

### rsync - 远程同步
```bash
# 基本同步
rsync -avz source/ user@remote:/path/

# 本地同步
rsync -avz source/ /backup/

# 删除目标中多余的文件
rsync -avz --delete source/ user@remote:/path/

# 排除特定文件
rsync -avz --exclude="*.log" source/ user@remote:/path/

# 干运行（显示将要执行的操作）
rsync -avz --dry-run source/ user@remote:/path/

# 压缩传输
rsync -avz -e ssh source/ user@remote:/path/
```

## 0x06 网络安全工具

### nmap - 网络扫描
```bash
# 扫描主机
nmap 192.168.1.1

# 扫描端口范围
nmap -p 1-100 192.168.1.1

# 扫描常见端口
nmap -F 192.168.1.1

# 操作系统检测
nmap -O 192.168.1.1

# 服务版本检测
nmap -sV 192.168.1.1

# 扫描网络
nmap 192.168.1.0/24
```

### iptables - 防火墙
```bash
# 列出规则
sudo iptables -L

# 允许特定端口
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT

# 拒绝特定IP
sudo iptables -A INPUT -s 192.168.1.100 -j DROP

# 保存规则
sudo iptables-save > /etc/iptables/rules.v4

# 恢复规则
sudo iptables-restore < /etc/iptables/rules.v4
```

### netcat - 网络工具
```bash
# 监听端口
nc -l 8080

# 连接主机
nc 192.168.1.1 80

# 端口扫描
nc -zv 192.168.1.1 1-100

# 文件传输（发送）
nc -l 8080 < file.txt

# 文件传输（接收）
nc 192.168.1.1 8080 > file.txt
```

## 参考

- [Linux网络命令大全](https://www.linux.com/training-tutorials/linux-networking-commands/)
- [网络诊断工具](https://www.linux.com/training-tutorials/linux-network-troubleshooting-commands/)
- [curl使用指南](https://curl.se/docs/manual.html)