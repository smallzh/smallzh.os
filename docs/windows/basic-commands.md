# Windows基础命令

## 0x01 目录导航命令

### cd - 切换目录
```cmd
# 切换到指定目录
cd C:\Users

# 切换到上级目录
cd ..

# 切换到根目录
cd \

# 切换到其他驱动器
cd /d D:\Projects

# 显示当前目录
cd
```

### dir - 列出目录内容
```cmd
# 列出当前目录
dir

# 列出详细信息
dir /a

# 按时间排序
dir /o:d

# 递归列出
dir /s

# 显示文件大小
dir /s /a
```

### mkdir - 创建目录
```cmd
# 创建单个目录
mkdir mydir

# 创建多级目录
mkdir parent\child\grandchild
```

### rmdir - 删除目录
```cmd
# 删除空目录
rmdir mydir

# 递归删除目录
rmdir /s mydir

# 强制删除
rmdir /s /q mydir
```

## 0x02 文件操作命令

### copy - 复制文件
```cmd
# 复制文件
copy file1.txt file2.txt

# 复制到目录
copy file.txt D:\backup\

# 合并文件
copy file1.txt + file2.txt combined.txt

# 复制所有文件
copy *.* D:\backup\
```

### move - 移动文件
```cmd
# 移动文件
move file.txt D:\backup\

# 重命名文件
move oldname.txt newname.txt

# 移动多个文件
move *.txt D:\backup\
```

### del - 删除文件
```cmd
# 删除文件
del file.txt

# 删除多个文件
del *.txt

# 强制删除只读文件
del /f file.txt

# 递归删除
del /s *.tmp

# 安静模式（不提示）
del /q *.tmp
```

### ren - 重命名文件
```cmd
# 重命名文件
ren oldname.txt newname.txt

# 重命名多个文件
ren *.txt *.bak
```

## 0x03 文件查看命令

### type - 显示文件内容
```cmd
# 显示文件内容
type file.txt

# 显示多个文件
type file1.txt file2.txt
```

### more - 分页显示
```cmd
# 分页显示文件
more file.txt

# 逐屏显示
type file.txt | more
```

### find - 查找文本
```cmd
# 查找文本
find "keyword" file.txt

# 显示行号
find /n "keyword" file.txt

# 统计行数
find /c "keyword" file.txt
```

### findstr - 查找字符串
```cmd
# 基本搜索
findstr "pattern" file.txt

# 忽略大小写
findstr /i "pattern" file.txt

# 递归搜索
findstr /s "pattern" *.txt

# 正则表达式
findstr /r "^[0-9]" file.txt

# 显示行号
findstr /n "pattern" file.txt
```

## 0x04 系统信息命令

### echo - 显示消息
```cmd
# 显示消息
echo Hello World

# 显示环境变量
echo %PATH%
echo %USERNAME%

# 关闭回显
@echo off

# 开启回显
echo on
```

### set - 设置环境变量
```cmd
# 显示所有环境变量
set

# 设置环境变量
set MY_VAR=hello

# 显示特定变量
set MY_VAR

# 追加到PATH
set PATH=%PATH%;C:\new\path
```

### date/time - 显示日期时间
```cmd
# 显示日期
date /t

# 显示时间
time /t

# 设置日期
date 01-01-2024

# 设置时间
time 12:00:00
```

## 0x05 网络基础命令

### ping - 网络连通性测试
```cmd
# 基本ping
ping google.com

# 指定次数
ping -n 5 google.com

# 持续ping
ping -t google.com

# 指定超时
ping -w 1000 google.com
```

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
```

### netstat - 网络统计
```cmd
# 显示所有连接
netstat -a

# 显示路由表
netstat -r

# 显示统计信息
netstat -s

# 显示PID和程序名
netstat -b

# 持续显示
netstat -n 5
```

## 0x06 批处理基础

### 基本语法
```batch
@echo off
echo This is a batch file
echo Current directory: %CD%
echo Current date: %DATE%
echo Current time: %TIME%
pause
```

### 变量使用
```batch
@echo off
set NAME=John
set AGE=25
echo Hello %NAME%, you are %AGE% years old
pause
```

### 条件判断
```batch
@echo off
set /p INPUT=Enter yes or no: 
if "%INPUT%"=="yes" (
    echo You entered yes
) else (
    echo You entered something else
)
pause
```

### 循环
```batch
@echo off
for %%i in (1 2 3 4 5) do (
    echo Number: %%i
)

for /f "tokens=*" %%i in ('dir /b') do (
    echo File: %%i
)
pause
```

## 参考

- [Windows命令参考](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/windows-commands)
- [CMD命令大全](https://ss64.com/nt/)