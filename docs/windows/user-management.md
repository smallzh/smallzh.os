# Windows用户管理

## 0x01 用户基础

Windows用户类型：
- **管理员**: 完全控制权限
- **标准用户**: 受限权限
- **来宾**: 最低权限

用户信息存储在SAM数据库（本地）或Active Directory（域）。

## 0x02 用户查看

### whoami - 当前用户
```cmd
# 显示当前用户名
whoami

# 显示用户SID
whoami /user

# 显示用户组
whoami /groups

# 显示用户权限
whoami /priv

# 显示用户UPN
whoami /upn
```

### net user - 用户信息
```cmd
# 列出所有用户
net user

# 显示特定用户信息
net user username

# 显示当前用户
net user %username%
```

### Get-LocalUser (PowerShell)
```powershell
# 获取所有本地用户
Get-LocalUser

# 获取特定用户
Get-LocalUser -Name "username"

# 获取启用的用户
Get-LocalUser | Where-Object Enabled -eq $true

# 获取用户详细信息
Get-LocalUser -Name "username" | Select-Object *
```

## 0x03 用户管理

### net user - 创建用户
```cmd
# 创建用户
net user username password /add

# 创建用户并设置全名
net user username password /add /fullname:"Full Name"

# 创建用户并设置描述
net user username password /add /comment:"User description"

# 创建用户并设置密码永不过期
net user username password /add /expires:never

# 创建用户并设置用户主目录
net user username password /add /homedir:\\server\share\username
```

### net user - 修改用户
```cmd
# 修改密码
net user username newpassword

# 禁用用户
net user username /active:no

# 启用用户
net user username /active:yes

# 设置密码过期时间
net user username /expires:12/31/2024

# 设置用户主目录
net user username /homedir:\\server\share\username

# 设置用户配置文件路径
net user username /profilepath:\\server\profiles\username
```

### net user - 删除用户
```cmd
# 删除用户
net user username /delete
```

### New-LocalUser (PowerShell)
```powershell
# 创建用户（交互式）
New-LocalUser -Name "username" -Password (Read-Host -AsSecureString)

# 创建用户（指定密码）
$password = ConvertTo-SecureString "password" -AsPlainText -Force
New-LocalUser -Name "username" -Password $password

# 创建用户并设置全名
New-LocalUser -Name "username" -FullName "Full Name" -Password $password

# 创建用户并设置描述
New-LocalUser -Name "username" -Description "User description" -Password $password

# 创建用户并禁用
New-LocalUser -Name "username" -Password $password -Disabled
```

### Set-LocalUser (PowerShell)
```powershell
# 修改用户密码
Set-LocalUser -Name "username" -Password (Read-Host -AsSecureString)

# 修改用户全名
Set-LocalUser -Name "username" -FullName "New Full Name"

# 修改用户描述
Set-LocalUser -Name "username" -Description "New description"

# 启用用户
Enable-LocalUser -Name "username"

# 禁用用户
Disable-LocalUser -Name "username"
```

### Remove-LocalUser (PowerShell)
```powershell
# 删除用户
Remove-LocalUser -Name "username"
```

## 0x04 组管理

### net localgroup - 组管理
```cmd
# 列出所有组
net localgroup

# 创建组
net localgroup groupname /add

# 删除组
net localgroup groupname /delete

# 添加用户到组
net localgroup groupname username /add

# 从组中移除用户
net localgroup groupname username /delete

# 列出组成员
net localgroup groupname
```

### Get-LocalGroup (PowerShell)
```powershell
# 获取所有本地组
Get-LocalGroup

# 获取特定组
Get-LocalGroup -Name "groupname"

# 获取组成员
Get-LocalGroupMember -Group "groupname"

# 获取用户所属组
Get-LocalGroup | Where-Object { (Get-LocalGroupMember -Group $_.Name).Name -match "username" }
```

### New-LocalGroup (PowerShell)
```powershell
# 创建组
New-LocalGroup -Name "groupname"

# 创建组并设置描述
New-LocalGroup -Name "groupname" -Description "Group description"
```

### Add-LocalGroupMember (PowerShell)
```powershell
# 添加用户到组
Add-LocalGroupMember -Group "groupname" -Member "username"

# 添加多个用户到组
Add-LocalGroupMember -Group "groupname" -Member "user1", "user2"
```

### Remove-LocalGroupMember (PowerShell)
```powershell
# 从组中移除用户
Remove-LocalGroupMember -Group "groupname" -Member "username"
```

## 0x05 用户配置文件

### 用户配置文件路径
```
C:\Users\username\
├── Desktop\
├── Documents\
├── Downloads\
├── AppData\
│   ├── Local\
│   ├── LocalLow\
│   └── Roaming\
```

### 查看用户配置文件
```cmd
# 列出用户配置文件
dir C:\Users

# 查看当前用户配置文件路径
echo %USERPROFILE%
```

### delprof - 删除用户配置文件
```cmd
# 删除未使用的配置文件
delprof /d:30

# 删除特定用户配置文件
delprof /c:computername /u:username
```

## 0x06 密码策略

### net accounts - 密码策略
```cmd
# 查看密码策略
net accounts

# 设置密码最小长度
net accounts /minpwlen:8

# 设置密码最长使用期限
net accounts /maxpwage:90

# 设置密码最短使用期限
net accounts /minpwage:1

# 设置密码历史记录
net accounts /uniquepw:5

# 设置账户锁定阈值
net accounts /lockoutthreshold:5

# 设置账户锁定持续时间
net accounts /lockoutduration:30

# 设置重置账户锁定计数器
net accounts /lockoutwindow:30
```

### secpol.msc - 本地安全策略
```cmd
# 打开本地安全策略
secpol.msc
```

## 0x07 用户权限分配

### secpol.msc - 用户权限分配
```cmd
# 打开本地安全策略
secpol.msc
# 导航到：本地策略 > 用户权限分配
```

### ntrights - 命令行权限分配
```cmd
# 分配权限（需要Resource Kit工具）
ntrights +r SeBackupPrivilege -u username

# 撤销权限
ntrights -r SeBackupPrivilege -u username
```

## 0x08 用户登录管理

### query user - 登录用户
```cmd
# 查询登录用户
query user

# 查询特定服务器登录用户
query user /server:SERVER01
```

### logoff - 注销用户
```cmd
# 注销用户
logoff session_id

# 注销特定服务器用户
logoff session_id /server:SERVER01
```

### tsdiscon - 断开远程桌面
```cmd
# 断开远程桌面会话
tsdiscon session_id
```

## 参考

- [Windows用户管理指南](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/manage-user-accounts)
- [PowerShell用户管理](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.localaccounts/)
- [Windows密码策略](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/password-policy)