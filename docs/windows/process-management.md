# Windows进程管理

## 0x01 进程基础

Windows进程类型：
- **应用程序进程**: 用户启动的应用程序
- **后台进程**: 系统服务和后台任务
- **系统进程**: Windows核心进程
- **会话进程**: 每个用户会话的进程

## 0x02 进程查看

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

# 显示特定状态的进程
tasklist /fi "status eq running"

# 显示特定PID的进程
tasklist /fi "pid eq 1234"

# 显示服务进程
tasklist /svc

# 显示模块信息
tasklist /m

# 显示特定模块的进程
tasklist /m kernel32.dll
```

### Get-Process (PowerShell)
```powershell
# 获取所有进程
Get-Process

# 获取特定进程
Get-Process -Name chrome

# 获取特定PID的进程
Get-Process -Id 1234

# 获取进程详细信息
Get-Process | Select-Object Name, Id, CPU, WorkingSet, Path

# 按CPU使用率排序
Get-Process | Sort-Object CPU -Descending

# 按内存使用排序
Get-Process | Sort-Object WorkingSet -Descending

# 获取进程和模块
Get-Process -Name chrome | Select-Object -ExpandProperty Modules

# 获取进程启动信息
Get-Process -Name chrome | Select-Object StartTime, TotalProcessorTime
```

### wmic - 进程管理
```cmd
# 列出所有进程
wmic process list brief

# 列出详细进程信息
wmic process list full

# 显示特定进程
wmic process where name="chrome.exe" get processid,commandline

# 显示进程树
wmic process get name,parentprocessid,processid

# 显示进程启动的命令行
wmic process get name,commandline
```

## 0x03 进程控制

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
taskkill /s SERVER01 /im notepad.exe

# 结束特定用户的进程
taskkill /fi "username eq domain\user" /im notepad.exe

# 结束特定会话的进程
taskkill /fi "sessionname eq console" /im notepad.exe
```

### Stop-Process (PowerShell)
```powershell
# 按名称结束进程
Stop-Process -Name notepad

# 按PID结束进程
Stop-Process -Id 1234

# 强制结束进程
Stop-Process -Name notepad -Force

# 结束多个进程
Stop-Process -Name notepad, chrome

# 获取并结束进程
Get-Process -Name notepad | Stop-Process
```

### wmic - 进程控制
```cmd
# 创建进程
wmic process call create "notepad.exe"

# 结束进程
wmic process where processid=1234 delete

# 修改进程优先级
wmic process where processid=1234 call setpriority 32
```

## 0x04 进程优先级

### wmic - 修改优先级
```cmd
# 设置进程优先级
wmic process where processid=1234 call setpriority 64

# 优先级值：
# 64: 低
# 16384: 低于正常
# 32: 正常
# 32768: 高于正常
# 128: 高
# 256: 实时
```

### PowerShell - 修改优先级
```powershell
# 获取进程优先级
(Get-Process -Name notepad).PriorityClass

# 设置进程优先级
(Get-Process -Name notepad).PriorityClass = "High"

# 设置优先级（数值）
(Get-Process -Name notepad).PriorityClass = 128

# 获取所有优先级值
[System.Diagnostics.ProcessPriorityClass]::GetValues([System.Diagnostics.ProcessPriorityClass])
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

# 暂停服务
sc pause wuauserv

# 恢复服务
sc continue wuauserv

# 配置服务启动类型
sc config wuauserv start= auto

# 删除服务
sc delete wuauserv

# 创建服务
sc create servicename binPath= "C:\path\to\service.exe"
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

### Get-Service (PowerShell)
```powershell
# 获取所有服务
Get-Service

# 获取特定服务
Get-Service -Name wuauserv

# 获取运行中的服务
Get-Service | Where-Object Status -eq "Running"

# 获取停止的服务
Get-Service | Where-Object Status -eq "Stopped"

# 获取服务依赖关系
Get-Service -Name wuauserv | Select-Object -ExpandProperty ServicesDependedOn

# 获取依赖此服务的服务
Get-Service -Name wuauserv | Select-Object -ExpandProperty DependentServices
```

### Start-Service (PowerShell)
```powershell
# 启动服务
Start-Service -Name wuauserv

# 启动多个服务
Start-Service -Name wuauserv, bits
```

### Stop-Service (PowerShell)
```powershell
# 停止服务
Stop-Service -Name wuauserv

# 强制停止服务
Stop-Service -Name wuauserv -Force

# 停止多个服务
Stop-Service -Name wuauserv, bits
```

### Restart-Service (PowerShell)
```powershell
# 重启服务
Restart-Service -Name wuauserv

# 强制重启服务
Restart-Service -Name wuauserv -Force
```

### Set-Service (PowerShell)
```powershell
# 设置服务启动类型
Set-Service -Name wuauserv -StartupType Automatic

# 设置服务启动类型（手动）
Set-Service -Name wuauserv -StartupType Manual

# 设置服务启动类型（禁用）
Set-Service -Name wuauserv -StartupType Disabled

# 设置服务描述
Set-Service -Name wuauserv -Description "Windows Update Service"
```

## 0x06 进程监控

### perfmon - 性能监视器
```cmd
# 打开性能监视器
perfmon

# 命令行性能监视
typeperf "\Process(chrome)\% Processor Time"
typeperf "\Process(chrome)\Working Set"
```

### resmon - 资源监视器
```cmd
# 打开资源监视器
resmon
```

### Get-Counter (PowerShell)
```powershell
# 获取进程CPU使用率
Get-Counter "\Process(chrome)\% Processor Time"

# 获取进程内存使用
Get-Counter "\Process(chrome)\Working Set"

# 获取所有进程CPU使用率
Get-Counter "\Process(*)\% Processor Time" | Select-Object -ExpandProperty CounterSamples | Sort-Object CookedValue -Descending | Select-Object -First 10
```

## 0x07 进程调试

### procdump - 进程转储
```cmd
# 创建进程转储
procdump -ma chrome.exe

# 创建特定PID转储
procdump -ma 1234

# 创建多次转储
procdump -n 3 chrome.exe

# 创建转储当CPU超过80%
procdump -c 80 chrome.exe
```

### Debug-Process (PowerShell)
```powershell
# 附加调试器
Debug-Process -Name notepad

# 使用现有调试器
Debug-Process -Name notepad -Debugger "C:\debugger\cdb.exe"
```

## 参考

- [Windows进程管理指南](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/tasklist)
- [PowerShell进程管理](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/)
- [Windows服务管理](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/sc-config)