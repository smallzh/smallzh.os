# PowerShell脚本编程

## 0x01 脚本基础

### 创建脚本
```powershell
# 这是一个注释
Write-Host "Hello, World!"
```

### 运行脚本
```powershell
# 设置执行策略
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

# 运行脚本
.\script.ps1

# 使用PowerShell运行
powershell -File script.ps1

# 使用Invoke-Expression
Get-Content script.ps1 | Invoke-Expression
```

## 0x02 变量

### 变量定义
```powershell
# 定义变量
$name = "John"
$age = 25
$pi = 3.14159

# 强制类型
[int]$number = 42
[string]$text = "Hello"
[bool]$flag = $true

# 常量
Set-Variable -Name pi -Value 3.14159 -Option Constant

# 使用变量
Write-Host $name
Write-Host ${name}
```

### 特殊变量
```powershell
# 自动变量
$args          # 函数参数
$PSItem        # 当前对象（$_）
$Error         # 错误数组
$NULL          # 空值
$TRUE          # 真
$FALSE         # 假
$PWD           # 当前目录
$HOME          # 用户主目录
$PID           # 进程ID
$PSVersionTable # PowerShell版本
```

### 字符串操作
```powershell
# 字符串长度
$name.Length

# 字符串拼接
$greeting = "Hello, $name!"

# 子字符串
$name.Substring(0, 3)

# 字符串替换
$name -replace "John", "Jane"

# 字符串分割
"apple,banana,orange" -split ","

# 字符串连接
"apple", "banana", "orange" -join ", "

# 字符串格式化
"Name: {0}, Age: {1}" -f $name, $age
```

### 数组
```powershell
# 定义数组
$fruits = @("apple", "banana", "orange")

# 访问数组元素
$fruits[0]

# 数组长度
$fruits.Length
$fruits.Count

# 添加元素
$fruits += "grape"

# 删除元素
$fruits = $fruits | Where-Object { $_ -ne "banana" }

# 数组切片
$fruits[0..2]
```

### 哈希表
```powershell
# 定义哈希表
$person = @{
    Name = "John"
    Age = 25
    City = "New York"
}

# 访问元素
$person.Name
$person["Name"]

# 添加元素
$person.Email = "john@example.com"

# 删除元素
$person.Remove("Age")

# 遍历哈希表
$person.GetEnumerator() | ForEach-Object {
    Write-Host "$($_.Key): $($_.Value)"
}
```

## 0x03 条件判断

### if语句
```powershell
# 基本if
if ($condition) {
    # 命令
}

# if-else
if ($condition) {
    # 命令
} else {
    # 命令
}

# if-elseif-else
if ($condition1) {
    # 命令
} elseif ($condition2) {
    # 命令
} else {
    # 命令
}
```

### 条件测试
```powershell
# 比较运算符
$a -eq $b     # 等于
$a -ne $b     # 不等于
$a -gt $b     # 大于
$a -ge $b     # 大于等于
$a -lt $b     # 小于
$a -le $b     # 小于等于

# 字符串比较
$str -like "*pattern*"     # 模式匹配
$str -notlike "*pattern*"  # 非模式匹配
$str -match "regex"        # 正则表达式匹配
$str -notmatch "regex"     # 非正则表达式匹配
$str -contains "value"     # 包含
$str -notcontains "value"  # 不包含

# 文件测试
Test-Path file.txt         # 文件存在
Test-Path directory -PathType Container  # 目录存在
Test-Path file.txt -PathType Leaf        # 文件存在

# 逻辑运算符
$a -and $b    # AND
$a -or $b     # OR
-not $a       # NOT
```

### switch语句
```powershell
switch ($value) {
    "value1" {
        # 命令
    }
    "value2" {
        # 命令
    }
    default {
        # 默认命令
    }
}

# 正则表达式switch
switch -Regex ($text) {
    "^\d+$" {
        Write-Host "数字"
    }
    "^[a-zA-Z]+$" {
        Write-Host "字母"
    }
    default {
        Write-Host "其他"
    }
}
```

## 0x04 循环

### for循环
```powershell
# 标准for循环
for ($i = 0; $i -lt 10; $i++) {
    Write-Host $i
}
```

### foreach循环
```powershell
# 数组循环
$fruits = @("apple", "banana", "orange")
foreach ($fruit in $fruits) {
    Write-Host $fruit
}

# 使用ForEach-Object
$fruits | ForEach-Object {
    Write-Host $_
}

# 简写
$fruits | % { Write-Host $_ }
```

### while循环
```powershell
# 基本while
$count = 0
while ($count -lt 10) {
    Write-Host $count
    $count++
}

# do-while
$count = 0
do {
    Write-Host $count
    $count++
} while ($count -lt 10)

# do-until
$count = 0
do {
    Write-Host $count
    $count++
} until ($count -ge 10)
```

### 循环控制
```powershell
# break - 跳出循环
for ($i = 0; $i -lt 10; $i++) {
    if ($i -eq 5) {
        break
    }
    Write-Host $i
}

# continue - 跳过本次循环
for ($i = 0; $i -lt 10; $i++) {
    if ($i -eq 5) {
        continue
    }
    Write-Host $i
}
```

## 0x05 函数

### 函数定义
```powershell
# 方法1
function Get-Sum {
    param($a, $b)
    return $a + $b
}

# 方法2
function Get-Sum($a, $b) {
    return $a + $b
}

# 高级函数
function Get-Sum {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory=$true)]
        [int]$a,
        
        [Parameter(Mandatory=$true)]
        [int]$b
    )
    
    return $a + $b
}
```

### 函数调用
```powershell
# 调用函数
$result = Get-Sum -a 5 -b 3
Write-Host "和是: $result"

# 使用位置参数
$result = Get-Sum 5 3
```

### 参数验证
```powershell
function Test-Range {
    param(
        [ValidateRange(1, 100)]
        [int]$number
    )
    Write-Host "数字: $number"
}

function Test-Pattern {
    param(
        [ValidatePattern("^\d+$")]
        [string]$text
    )
    Write-Host "文本: $text"
}
```

### 返回值
```powershell
# 使用return
function Get-Status {
    if ($condition) {
        return $true
    } else {
        return $false
    }
}

# 使用Write-Output（默认）
function Get-Data {
    Write-Output "数据"
}

# 使用Write-Host（仅显示）
function Show-Message {
    Write-Host "消息"
}
```

## 0x06 输入输出

### 读取输入
```powershell
# 读取变量
$name = Read-Host "请输入名字"
Write-Host "你好, $name"

# 读取密码
$securePassword = Read-Host "请输入密码" -AsSecureString

# 读取选择
$choice = Read-Host "请选择 (Y/N)"
```

### 输出方法
```powershell
# Write-Host - 直接输出
Write-Host "消息" -ForegroundColor Green

# Write-Output - 输出到管道
Write-Output "数据"

# Write-Verbose - 详细输出
Write-Verbose "详细信息"

# Write-Debug - 调试输出
Write-Debug "调试信息"

# Write-Warning - 警告
Write-Warning "警告信息"

# Write-Error - 错误
Write-Error "错误信息"
```

### 文件操作
```powershell
# 读取文件
$content = Get-Content file.txt

# 写入文件
Set-Content -Path file.txt -Value "内容"

# 追加到文件
Add-Content -Path file.txt -Value "新内容"

# 读取CSV
$data = Import-Csv data.csv

# 写入CSV
$data | Export-Csv data.csv -NoTypeInformation

# 读取JSON
$data = Get-Content data.json | ConvertFrom-Json

# 写入JSON
$data | ConvertTo-Json | Set-Content data.json
```

## 0x07 错误处理

### try-catch-finally
```powershell
try {
    # 可能出错的代码
    $result = 1 / 0
} catch {
    # 错误处理
    Write-Host "发生错误: $_"
} finally {
    # 总是执行
    Write-Host "清理操作"
}
```

### 错误变量
```powershell
# 查看最近错误
$Error[0]

# 查看所有错误
$Error

# 清除错误
$Error.Clear()
```

### 错误偏好
```powershell
# 设置错误偏好
$ErrorActionPreference = "Stop"  # 遇到错误停止
$ErrorActionPreference = "Continue"  # 遇到错误继续
$ErrorActionPreference = "SilentlyContinue"  # 静默继续

# 临时设置
Get-Process -Name "nonexistent" -ErrorAction SilentlyContinue
```

## 0x08 调试

### 调试命令
```powershell
# 设置断点
Set-PSBreakpoint -Script script.ps1 -Line 10

# 列出断点
Get-PSBreakpoint

# 删除断点
Remove-PSBreakpoint -Id 1

# 进入调试模式
Set-PSDebug -Step

# 退出调试模式
Set-PSDebug -Off
```

### 调试技巧
```powershell
# 打印变量
Write-Debug "变量值: $variable"

# 使用Trace-Command
Trace-Command -Name ParameterBinding -Expression { Get-Process } -PSHost
```

## 0x09 实用示例

### 系统信息脚本
```powershell
# 获取系统信息
function Get-SystemInfo {
    $os = Get-CimInstance Win32_OperatingSystem
    $cpu = Get-CimInstance Win32_Processor
    $memory = Get-CimInstance Win32_PhysicalMemory | Measure-Object -Property Capacity -Sum
    
    [PSCustomObject]@{
        ComputerName = $env:COMPUTERNAME
        OS = $os.Caption
        Version = $os.Version
        CPU = $cpu.Name
        MemoryGB = [math]::Round($memory.Sum / 1GB, 2)
    }
}

Get-SystemInfo
```

### 服务监控脚本
```powershell
# 监控服务状态
function Monitor-Services {
    param(
        [string[]]$Services = @("wuauserv", "bits")
    )
    
    foreach ($service in $Services) {
        $status = Get-Service -Name $service
        if ($status.Status -ne "Running") {
            Write-Warning "服务 $service 未运行"
            # 尝试启动服务
            Start-Service -Name $service -ErrorAction SilentlyContinue
        }
    }
}

Monitor-Services
```

### 日志脚本
```powershell
# 日志函数
function Write-Log {
    param(
        [string]$Message,
        [string]$Level = "INFO"
    )
    
    $timestamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
    $logMessage = "[$timestamp] [$Level] $Message"
    
    Add-Content -Path "script.log" -Value $logMessage
    Write-Host $logMessage
}

# 使用日志
Write-Log "脚本开始执行"
Write-Log "发生警告" -Level "WARNING"
Write-Log "发生错误" -Level "ERROR"
```

## 参考

- [PowerShell官方文档](https://docs.microsoft.com/en-us/powershell/)
- [PowerShell脚本指南](https://docs.microsoft.com/en-us/powershell/scripting/overview)
- [PowerShell最佳实践](https://docs.microsoft.com/en-us/powershell/scripting/learn/ps101/)