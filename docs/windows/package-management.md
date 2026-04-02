# Windows包管理

## 0x01 包管理基础

Windows包管理器：
- **winget**: 微软官方包管理器
- **Chocolatey**: 社区包管理器
- **Scoop**: 命令行安装器
- **MSI/EXE**: 传统安装包
- **Microsoft Store**: 应用商店

## 0x02 winget包管理

### 基本操作
```cmd
# 搜索软件包
winget search package_name

# 安装软件包
winget install package_name

# 安装特定版本
winget install package_name --version version

# 删除软件包
winget uninstall package_name

# 更新软件包
winget upgrade package_name

# 更新所有软件包
winget upgrade --all

# 列出已安装软件包
winget list

# 显示软件包信息
winget show package_name
```

### 高级操作
```cmd
# 安装并接受协议
winget install package_name --accept-package-agreements --accept-source-agreements

# 安装到特定路径
winget install package_name --location "C:\Program Files\Package"

# 安装并跳过哈希验证
winget install package_name --ignore-security-hash

# 安装并强制覆盖
winget install package_name --force

# 导出已安装软件包列表
winget export -o packages.json

# 导入软件包列表
winget import -i packages.json
```

### 源管理
```cmd
# 列出源
winget source list

# 添加源
winget source add name url

# 删除源
winget source remove name

# 更新源
winget source update

# 重置源
winget source reset
```

## 0x03 Chocolatey包管理

### 安装Chocolatey
```cmd
# 以管理员身份运行PowerShell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

### 基本操作
```cmd
# 搜索软件包
choco search package_name

# 安装软件包
choco install package_name -y

# 安装特定版本
choco install package_name --version version -y

# 删除软件包
choco uninstall package_name -y

# 更新软件包
choco upgrade package_name -y

# 更新所有软件包
choco upgrade all -y

# 列出已安装软件包
choco list --local-only

# 显示软件包信息
choco info package_name
```

### 高级操作
```cmd
# 安装并忽略依赖
choco install package_name --ignore-dependencies -y

# 安装到特定路径
choco install package_name --install-directory "C:\Tools" -y

# 强制重新安装
choco install package_name --force -y

# 导出已安装软件包列表
choco export -o packages.config

# 导入软件包列表
choco install packages.config -y
```

### 包源管理
```cmd
# 列出源
choco source list

# 添加源
choco source add -n name -s url

# 删除源
choco source remove -n name

# 启用源
choco source enable -n name

# 禁用源
choco source disable -n name
```

## 0x04 Scoop包管理

### 安装Scoop
```cmd
# 以管理员身份运行PowerShell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
 irm get.scoop.sh | iex
```

### 基本操作
```cmd
# 搜索软件包
scoop search package_name

# 安装软件包
scoop install package_name

# 删除软件包
scoop uninstall package_name

# 更新软件包
scoop update package_name

# 更新所有软件包
scoop update *

# 列出已安装软件包
scoop list

# 显示软件包信息
scoop info package_name
```

### 高级操作
```cmd
# 添加bucket（软件源）
scoop bucket add bucket_name

# 列出bucket
scoop bucket list

# 删除bucket
scoop bucket rm bucket_name

# 清理缓存
scoop cache rm *

# 重置应用
scoop reset package_name
```

## 0x05 Microsoft Store

### PowerShell操作
```powershell
# 列出所有已安装应用
Get-AppxPackage

# 列出特定应用
Get-AppxPackage -Name "*package_name*"

# 安装应用（需要从Store获取PackageFamilyName）
Add-AppxPackage -Register "C:\path\to\AppxManifest.xml"

# 卸载应用
Get-AppxPackage -Name "package_name" | Remove-AppxPackage

# 重新安装所有应用
Get-AppxPackage -AllUsers | ForEach-Object {Add-AppxPackage -DisableDevelopmentMode -Register "$($_.InstallLocation)\AppXManifest.xml"}
```

### winget管理Store应用
```cmd
# 搜索Store应用
winget search --source msstore package_name

# 安装Store应用
winget install --source msstore package_name

# 列出Store应用
winget list --source msstore
```

## 0x06 MSI/EXE安装

### msiexec - MSI安装
```cmd
# 安装MSI
msiexec /i package.msi

# 静默安装
msiexec /i package.msi /quiet

# 无界面安装
msiexec /i package.msi /passive

# 指定安装路径
msiexec /i package.msi INSTALLDIR="C:\Program Files\Package"

# 修复安装
msiexec /f package.msi

# 卸载MSI
msiexec /x package.msi

# 卸载并删除日志
msiexec /x package.msi /quiet /log uninstall.log
```

### EXE安装
```cmd
# 交互式安装
package.exe

# 静默安装
package.exe /S

# 无界面安装
package.exe /silent

# 指定安装路径
package.exe /D=C:\Program Files\Package

# 查看安装参数
package.exe /?
```

## 0x07 软件包验证

### 文件校验
```powershell
# 计算文件哈希
Get-FileHash -Path "file.msi" -Algorithm SHA256

# 验证哈希
(Get-FileHash -Path "file.msi" -Algorithm SHA256).Hash -eq "expected_hash"
```

### 数字签名验证
```powershell
# 验证文件签名
Get-AuthenticodeSignature -FilePath "file.msi"

# 验证并显示详细信息
Get-AuthenticodeSignature -FilePath "file.msi" | Select-Object *
```

## 0x08 软件包管理最佳实践

### 版本控制
```cmd
# 导出软件包列表（winget）
winget export -o packages.json

# 导出软件包列表（choco）
choco export -o packages.config

# 定期更新软件包
winget upgrade --all
choco upgrade all -y
```

### 自动化安装
```cmd
# 批量安装（winget）
winget import -i packages.json

# 批量安装（choco）
choco install packages.config -y
```

## 参考

- [winget使用指南](https://docs.microsoft.com/en-us/windows/package-manager/winget/)
- [Chocolatey使用指南](https://docs.chocolatey.org/en-us/choco/commands/)
- [Scoop使用指南](https://github.com/ScoopInstaller/Scoop/wiki)
- [Microsoft Store管理](https://docs.microsoft.com/en-us/windows/application-management/apps-in-windows-10)