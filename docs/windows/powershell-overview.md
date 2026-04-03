# PowerShell概述

## 0x01 Windows自带PowerShell版本

### Windows 10中的PowerShell
- Windows 10默认安装Windows PowerShell 5.1
- 位置：`$env:SystemRoot\System32\WindowsPowerShell\v1.0\powershell.exe`
- 这是基于.NET Framework的版本
- 版本号可通过`$PSVersionTable.PSVersion`查询，显示为5.1.x

### Windows 11中的PowerShell
- Windows 11同样默认安装Windows PowerShell 5.1
- 同样位于`$env:SystemRoot\System32\WindowsPowerShell\v1.0\powershell.exe`
- 但Windows 11同时预装了PowerShell 7作为可选功能
- 可以通过"设置" -> "应用" -> "可选功能" -> "添加功能"来安装PowerShell 7

### 两个版本的共存
- Windows PowerShell 5.1（ legado版本）: `powershell.exe`
- PowerShell 7（跨平台版本）: `pwsh.exe`
- 两者可以并行运行，互不影响
- 建议在新项目中使用PowerShell 7，除非有特定兼容性需求

## 0x02 升级到PowerShell 7的指导

### 为什么升级到PowerShell 7
- 跨平台支持（Windows、macOS、Linux）
- 性能改进和新功能
- 更好的错误信息和调试体验
- 新增的运算符和语法特性
- 主动维护和更新，而Windows PowerShell 5.1仅接收安全更新

### 安装方法选择（个人环境）

#### 推荐方法：使用WinGet（Windows Package Manager）
1. 以管理员身份打开PowerShell或命令提示符
2. 搜索可用的PowerShell版本：
   ```powershell
   winget search --id Microsoft.PowerShell
   ```
3. 安装最新稳定版本：
   ```powershell
   winget install --id Microsoft.PowerShell --source winget
   ```
4. 安装预览版本（如果需要）：
   ```powershell
   winget install --id Microsoft.PowerShell.Preview --source winget
   ```

#### 使用ZIP包安装（灵活部署）
1. 下载ZIP存档：
    - [PowerShell-7.6.0-win-x64.zip](https://github.com/PowerShell/PowerShell/releases/download/v7.6.0/PowerShell-7.6.0-win-x64.zip)
    - [PowerShell-7.6.0-win-arm64.zip](https://github.com/PowerShell/PowerShell/releases/download/v7.6.0/PowerShell-7.6.0-win-arm64.zip)
2. 解压到目标目录（建议：`$Env:ProgramFiles\PowerShell\7`）
3. 手动将目录添加到PATH环境变量
4. 手动创建开始菜单快捷方式

#### 作为.NET全局工具安装
1. 确保已安装.NET Core SDK
2. 执行安装命令：
    ```powershell
    dotnet tool install --global PowerShell
   ```
3. 新的shell会话中即可使用`pwsh`命令

#### 从Microsoft Store安装
1. 打开Microsoft Store应用
2. 搜索"PowerShell"
3. 选择由Microsoft发布的PowerShell应用进行安装
4. 注意：Microsoft Store版本有一些限制，不支持某些系统级配置

### 验证安装
安装完成后，验证PowerShell 7已正确安装：
```powershell
pwsh -版本
# 应显示类似：7.6.0

# 或者在PowerShell 7会话中
$PSVersionTable.PSVersion
```

### 并排安装多个版本
如果需要安装多个PowerShell 7版本（例如稳定版和预览版）：
1. 使用不同的安装方法（如一个用WinGet，一个用ZIP）
2. 或者使用ZIP方法手动安装到不同目录
3. 确保每个版本的安装目录不同（如7和7-preview）
4. 手动管理PATH环境变量和开始菜单快捷方式

### 升级现有PowerShell 7
如果已经安装了PowerShell 7并想升级到更新版本：
1. 使用最初的安装方法进行升级：
    - 如果是用WinGet安装的：`winget upgrade --id Microsoft.PowerShell`
    - 如果是用ZIP安装的：下载新ZIP并替换旧文件
2. PowerShell的预览版本可以与非预览版本并行安装
3. 较新的预览版本会替换现有的以前的预览版本

### 卸载PowerShell 7
根据安装方法选择相应的卸载方式：
- WinGet安装：`winget uninstall --id Microsoft.PowerShell`
- MSI包安装：通过"程序和功能"控制面板卸载
- ZIP包安装：删除解压缩的文件夹
- Microsoft Store安装：在开始菜单中右键点击PowerShell 7选择"卸载"
- .NET全局工具：`dotnet tool uninstall --global PowerShell`

## 0x03 使用建议

### 开发和学习建议
- 对于新项目和学习，优先使用PowerShell 7
- 只有在必须与旧Windows PowerShell模块兼容时才使用Windows PowerShell 5.1
- 利用PowerShell 7的跨平台特性在不同环境中测试脚本

### 个人使用建议
- 通过Microsoft Update启用自动更新以获取安全补丁（如果使用支持此功能的安装方法）
- 测试现有脚本在PowerShell 7中的兼容性
- 考虑使用兼容性特性运行必要的Windows PowerShell模块

### 资源和进一步学习
- 官方文档：https://learn.microsoft.com/zh-cn/powershell/
- PowerShell GitHub仓库：https://github.com/PowerShell/PowerShell
- PowerShell社区和论坛
- 各种在线教程和视频课程