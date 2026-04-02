# Windows系统信息

## 0x01 系统基本信息

### systeminfo - 系统详细信息
```cmd
# 显示完整系统信息
systeminfo

# 显示特定信息
systeminfo | findstr "OS"
systeminfo | findstr "Memory"
systeminfo | findstr "Processor"

# 远程获取系统信息
systeminfo /s remote-pc /u domain\user /p password
```

### hostname - 主机名
```cmd
# 显示计算机名
hostname

# 设置计算机名（需要管理员权限）
wmic computersystem where name="%computername%" call rename name="NewName"
```

### ver - 操作系统版本
```cmd
# 显示操作系统版本
ver
```

### winver - Windows版本（图形界面）
```cmd
# 打开Windows版本对话框
winver
```

## 0x02 磁盘空间管理

### dir - 显示目录信息
```cmd
# 显示目录大小
dir /s C:\

# 显示特定类型文件
dir *.txt /s

# 按大小排序
dir /o:s

# 显示隐藏文件
dir /a:h
```

### chkdsk - 磁盘检查
```cmd
# 检查磁盘
chkdsk C:

# 修复磁盘错误
chkdsk C: /f

# 扫描并修复坏扇区
chkdsk C: /r

# 强制卸载卷
chkdsk C: /x
```

### diskpart - 磁盘分区工具
```cmd
# 启动diskpart
diskpart

# 常用命令：
list disk
list volume
select disk 0
select volume 1
detail disk
detail volume
```

## 0x03 内存管理

### systeminfo 查看内存
```cmd
# 查看物理内存
systeminfo | findstr "Memory"

# 查看可用内存
wmic OS get FreePhysicalMemory

# 查看总内存
wmic computersystem get TotalPhysicalMemory
```

### performance monitor
```cmd
# 打开性能监视器
perfmon

# 命令行性能监视
typeperf "\Memory\Available MBytes"
```

## 0x04 进程管理

### tasklist - 进程列表
```cmd
# 显示所有进程
tasklist

# 显示详细信息
tasklist /v

# 显示特定进程
tasklist /fi "imagename eq chrome.exe"

# 显示内存使用大于100MB的进程
tasklist /fi "memusage gt 102400"

# 显示特定用户的进程
tasklist /fi "username eq domain\user"
```

### taskkill - 结束进程
```cmd
# 按名称结束进程
taskkill /im notepad.exe

# 按PID结束进程
taskkill /pid 1234

# 强制结束进程
taskkill /f /im notepad.exe

# 结束进程树
taskkill /f /t /im notepad.exe

# 结束远程计算机进程
taskkill /s remote-pc /im notepad.exe
```

### wmic - 进程管理
```cmd
# 列出进程
wmic process list brief

# 显示特定进程
wmic process where name="chrome.exe" get processid,commandline

# 创建进程
wmic process call create "notepad.exe"

# 结束进程
wmic process where processid=1234 delete
```

## 0x05 服务管理

### sc - 服务控制
```cmd
# 查询服务状态
sc query

# 查询特定服务
sc query wuauserv

# 启动服务
sc start wuauserv

# 停止服务
sc stop wuauserv

# 配置服务启动类型
sc config wuauserv start= auto
```

### net start/stop - 服务管理
```cmd
# 启动服务
net start wuauserv

# 停止服务
net stop wuauserv

# 列出所有服务
net start
```

### PowerShell服务管理
```powershell
# 获取所有服务
Get-Service

# 获取特定服务
Get-Service -Name wuauserv

# 启动服务
Start-Service -Name wuauserv

# 停止服务
Stop-Service -Name wuauserv

# 设置服务启动类型
Set-Service -Name wuauserv -StartupType Automatic
```

## 0x06 系统日志

### eventvwr - 事件查看器
```cmd
# 打开事件查看器
eventvwr

# 命令行查看事件
wevtutil qe System /c:10 /rd:true /f:text
```

### PowerShell日志查看
```powershell
# 获取系统日志
Get-EventLog -LogName System -Newest 10

# 获取应用程序日志
Get-EventLog -LogName Application -Newest 10

# 获取安全日志
Get-EventLog -LogName Security -Newest 10

# 按事件ID过滤
Get-EventLog -LogName System -InstanceId 7036
```

## 0x07 系统性能监控

### perfmon - 性能监视器
```cmd
# 打开性能监视器
perfmon

# 命令行性能监视
typeperf "\Processor(_Total)\% Processor Time"
typeperf "\Memory\Available MBytes"
typeperf "\PhysicalDisk(_Total)\% Disk Time"
```

### resmon - 资源监视器
```cmd
# 打开资源监视器
resmon
```

### PowerShell性能监控
```powershell
# CPU使用率
Get-Counter '\Processor(_Total)\% Processor Time'

# 内存使用
Get-Counter '\Memory\Available MBytes'

# 磁盘使用
Get-Counter '\PhysicalDisk(_Total)\% Disk Time'
```

## 参考

- [Windows系统管理命令](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/windows-commands)
- [PowerShell管理命令](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/)