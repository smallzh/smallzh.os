# Windows网络工具

## 0x01 网络连通性测试

### ping - 网络连通性测试
```cmd
# 基本ping测试
ping google.com

# 指定次数
ping -n 5 google.com

# 指定包大小
ping -l 1000 google.com

# 指定超时时间（毫秒）
ping -w 1000 google.com

# 持续ping
ping -t google.com

# 记录路由（最多9跳）
ping -r 9 google.com
```

### Test-Connection (PowerShell)
```powershell
# 基本连接测试
Test-Connection google.com

# 指定次数
Test-Connection google.com -Count 5

# 指定缓冲区大小
Test-Connection google.com -BufferSize 1000

# 持续测试
Test-Connection google.com -Continuous

# 显示详细信息
Test-Connection google.com -Detailed
```

### tracert - 路由跟踪
```cmd
# 基本路由跟踪
tracert google.com

# 指定最大跳数
tracert -h 15 google.com

# 不解析主机名
tracert -d google.com

# 指定超时时间
tracert -w 1000 google.com
```

### Test-NetConnection (PowerShell)
```powershell
# 测试到特定端口的连接
Test-NetConnection google.com -Port 80

# 测试到特定主机的连接
Test-NetConnection google.com

# 显示详细信息
Test-NetConnection google.com -InformationLevel Detailed

# 路由跟踪
Test-NetConnection google.com -TraceRoute
```

## 0x02 网络配置查看

### ipconfig - 网络配置
```cmd
# 显示基本配置
ipconfig

# 显示详细配置
ipconfig /all

# 释放IP地址
ipconfig /release

# 续订IP地址
ipconfig /renew

# 刷新DNS缓存
ipconfig /flushdns

# 显示DNS缓存
ipconfig /displaydns
```

### netsh - 网络配置
```cmd
# 显示网络接口
netsh interface show interface

# 显示IP配置
netsh interface ip show config

# 设置静态IP
netsh interface ip set address "Local Area Connection" static 192.168.1.100 255.255.255.0 192.168.1.1

# 设置DNS
netsh interface ip set dns "Local Area Connection" static 8.8.8.8

# 显示防火墙状态
netsh firewall show state

# 配置防火墙规则
netsh advfirewall firewall add rule name="Allow HTTP" dir=in action=allow protocol=TCP localport=80
```

### Get-NetAdapter (PowerShell)
```powershell
# 获取网络适配器信息
Get-NetAdapter

# 获取特定适配器
Get-NetAdapter -Name "Ethernet"

# 获取适配器状态
Get-NetAdapter | Select-Object Name, Status, LinkSpeed

# 启用适配器
Enable-NetAdapter -Name "Ethernet"

# 禁用适配器
Disable-NetAdapter -Name "Ethernet"
```

## 0x03 网络连接查看

### netstat - 网络统计
```cmd
# 显示所有连接
netstat -a

# 显示监听端口
netstat -an | findstr LISTENING

# 显示TCP连接
netstat -p tcp

# 显示UDP连接
netstat -p udp

# 显示进程ID
netstat -o

# 显示路由表
netstat -r

# 持续显示（每5秒）
netstat -n 5
```

### Get-NetTCPConnection (PowerShell)
```powershell
# 获取所有TCP连接
Get-NetTCPConnection

# 获取监听连接
Get-NetTCPConnection -State Listen

# 获取特定端口的连接
Get-NetTCPConnection -LocalPort 80

# 获取特定远程地址的连接
Get-NetTCPConnection -RemoteAddress 192.168.1.1

# 获取特定进程的连接
Get-NetTCPConnection -OwningProcess (Get-Process chrome).Id
```

### Get-NetUDPEndpoint (PowerShell)
```powershell
# 获取所有UDP端点
Get-NetUDPEndpoint

# 获取特定端口的端点
Get-NetUDPEndpoint -LocalPort 53

# 获取特定进程的端点
Get-NetUDPEndpoint -OwningProcess (Get-Process svchost).Id
```

## 0x04 DNS工具

### nslookup - DNS查询
```cmd
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

### Resolve-DnsName (PowerShell)
```powershell
# 基本DNS查询
Resolve-DnsName google.com

# 查询特定记录类型
Resolve-DnsName google.com -Type MX
Resolve-DnsName google.com -Type AAAA
Resolve-DnsName google.com -Type NS

# 指定DNS服务器
Resolve-DnsName google.com -Server 8.8.8.8

# 反向查询
Resolve-DnsName 8.8.8.8
```

## 0x05 数据传输工具

### Invoke-WebRequest (PowerShell)
```powershell
# 下载文件
Invoke-WebRequest -Uri "https://example.com/file.zip" -OutFile "file.zip"

# 获取网页内容
$response = Invoke-WebRequest -Uri "https://example.com"
$response.Content

# 发送POST请求
Invoke-WebRequest -Uri "https://example.com/api" -Method POST -Body @{key="value"}

# 设置请求头
$headers = @{"Content-Type" = "application/json"}
Invoke-WebRequest -Uri "https://example.com" -Headers $headers

# 使用代理
Invoke-WebRequest -Uri "https://example.com" -Proxy "http://proxy:8080"

# 保存cookie
$session = New-Object Microsoft.PowerShell.Commands.WebRequestSession
Invoke-WebRequest -Uri "https://example.com" -WebSession $session
```

### Invoke-RestMethod (PowerShell)
```powershell
# 获取JSON数据
$data = Invoke-RestMethod -Uri "https://api.example.com/data"
$data | ConvertTo-Json

# 发送POST请求
$body = @{
    name = "John"
    age = 30
} | ConvertTo-Json
Invoke-RestMethod -Uri "https://api.example.com/users" -Method POST -Body $body -ContentType "application/json"

# 使用认证
$cred = Get-Credential
Invoke-RestMethod -Uri "https://api.example.com/data" -Credential $cred
```

### BITS传输
```cmd
# 后台智能传输服务
bitsadmin /transfer myjob /download /priority normal https://example.com/file.zip C:\file.zip

# 上传文件
bitsadmin /transfer myjob /upload /priority normal C:\file.zip https://example.com/upload

# 查看传输状态
bitsadmin /list
bitsadmin /info myjob
```

## 0x06 远程连接工具

### mstsc - 远程桌面
```cmd
# 打开远程桌面连接
mstsc

# 连接到特定计算机
mstsc /v:192.168.1.100

# 指定分辨率
mstsc /v:192.168.1.100 /w:1920 /h:1080

# 全屏模式
mstsc /v:192.168.1.100 /f
```

### Enter-PSSession (PowerShell)
```powershell
# 远程PowerShell会话
Enter-PSSession -ComputerName SERVER01

# 使用凭据
$cred = Get-Credential
Enter-PSSession -ComputerName SERVER01 -Credential $cred

# 使用SSL
Enter-PSSession -ComputerName SERVER01 -UseSSL
```

### Invoke-Command (PowerShell)
```powershell
# 远程执行命令
Invoke-Command -ComputerName SERVER01 -ScriptBlock { Get-Process }

# 执行脚本文件
Invoke-Command -ComputerName SERVER01 -FilePath "C:\script.ps1"

# 并行执行多台计算机
Invoke-Command -ComputerName SERVER01, SERVER02 -ScriptBlock { Get-Service }

# 使用凭据
$cred = Get-Credential
Invoke-Command -ComputerName SERVER01 -Credential $cred -ScriptBlock { Get-Process }
```

## 0x07 网络安全工具

### netsh advfirewall - 防火墙
```cmd
# 显示防火墙状态
netsh advfirewall show allprofiles

# 启用防火墙
netsh advfirewall set allprofiles state on

# 禁用防火墙
netsh advfirewall set allprofiles state off

# 添加入站规则
netsh advfirewall firewall add rule name="Allow HTTP" dir=in action=allow protocol=TCP localport=80

# 添加出站规则
netsh advfirewall firewall add rule name="Block Outbound" dir=out action=block remoteip=192.168.1.100

# 删除规则
netsh advfirewall firewall delete rule name="Allow HTTP"
```

### Get-NetFirewallRule (PowerShell)
```powershell
# 获取所有防火墙规则
Get-NetFirewallRule

# 获取启用的规则
Get-NetFirewallRule -Enabled True

# 获取特定方向的规则
Get-NetFirewallRule -Direction Inbound

# 创建新规则
New-NetFirewallRule -DisplayName "Allow HTTP" -Direction Inbound -Protocol TCP -LocalPort 80 -Action Allow

# 删除规则
Remove-NetFirewallRule -DisplayName "Allow HTTP"
```

## 参考

- [Windows网络命令参考](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/network-shell-netsh)
- [PowerShell网络管理](https://docs.microsoft.com/en-us/powershell/module/netadapter/)
- [Windows防火墙配置](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-firewall/windows-firewall-with-advanced-security)