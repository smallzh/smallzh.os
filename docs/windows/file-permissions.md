# Windows文件权限

## 0x01 权限基础

Windows使用访问控制列表（ACL）管理文件权限。权限分为：
- **完全控制**: 所有权限
- **修改**: 读取、写入、执行、删除
- **读取和执行**: 运行程序、读取文件
- **列出文件夹内容**: 查看目录内容
- **读取**: 查看文件内容
- **写入**: 修改文件内容

## 0x02 权限查看

### 右键属性查看
1. 右键点击文件/文件夹
2. 选择"属性"
3. 切换到"安全"选项卡

### icacls命令查看
```cmd
# 查看文件权限
icacls file.txt

# 查看目录权限
icacls C:\folder

# 详细输出
icacls file.txt /T
```

## 0x03 icacls - 权限管理

### 基本语法
```cmd
icacls <文件或目录> [选项]
```

### 授予权限
```cmd
# 授予用户完全控制权
icacls file.txt /grant user:F

# 授予用户读取权限
icacls file.txt /grant user:R

# 授予用户修改权限
icacls file.txt /grant user:M

# 授予组读取和执行权限
icacls file.txt /grant group:RX

# 继承权限
icacls file.txt /inheritance:e
```

### 拒绝权限
```cmd
# 拒绝用户写入权限
icacls file.txt /deny user:W

# 拒绝用户读取权限
icacls file.txt /deny user:R

# 撤销拒绝
icacls file.txt /remove:d user
```

### 移除权限
```cmd
# 移除用户所有权限
icacls file.txt /remove user

# 移除组所有权限
icacls file.txt /remove group

# 移除所有用户权限
icacls file.txt /remove:g users
```

### 递归操作
```cmd
# 递归设置目录权限
icacls C:\folder /grant user:F /T

# 递归移除权限
icacls C:\folder /remove user /T
```

## 0x04 高级权限设置

### 继承权限
```cmd
# 禁用继承并保留已继承权限
icacls file.txt /inheritance:e

# 禁用继承并移除已继承权限
icacls file.txt /inheritance:d

# 启用继承
icacls file.txt /inheritance:e
```

### 替换权限
```cmd
# 替换所有权限
icacls file.txt /reset

# 替换子对象权限
icacls C:\folder /reset /T
```

### 保存和恢复权限
```cmd
# 保存权限到文件
icacls C:\folder /save permissions.txt /T

# 从文件恢复权限
icacls C:\folder /restore permissions.txt
```

## 0x05 takeown - 获取所有权

```cmd
# 获取文件所有权
takeown /f file.txt

# 获取目录所有权
takeown /f C:\folder /r /d y

# 指定用户获取所有权
takeown /f file.txt /u domain\user
```

## 0x06 权限应用场景

### 共享文件夹权限
```cmd
# 设置共享文件夹权限
icacls D:\Shared /grant users:RX /T
icacls D:\Shared /grant administrators:F /T
```

### Web服务器权限
```cmd
# IIS网站权限
icacls C:\inetpub\wwwroot /grant IIS_IUSRS:RX /T
icacls C:\inetpub\wwwroot /grant IUSR:RX /T
```

### 敏感文件权限
```cmd
# 配置文件权限
icacls config.xml /inheritance:d
icacls config.xml /grant administrators:F
icacls config.xml /grant system:F
```

## 参考

- [Windows文件权限指南](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/access-control)
- [icacls命令参考](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/icacls)
- [NTFS权限最佳实践](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn221981(v=ws.11))