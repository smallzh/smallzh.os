# Linux进程管理

## 0x01 进程基础

进程是程序的运行实例。每个进程有：
- **PID**: 进程ID
- **PPID**: 父进程ID
- **UID/GID**: 运行用户/组
- **状态**: 运行、睡眠、停止、僵尸等
- **优先级**: nice值

## 0x02 进程查看

### ps - 进程状态
```bash
# BSD风格
ps aux

# System V风格
ps -ef

# 显示进程树
ps -ef --forest

# 显示特定用户进程
ps -u username

# 显示特定进程
ps -p PID

# 自定义输出
ps -eo pid,ppid,cmd,%cpu,%mem,stat

# 显示线程
ps -eLf
```

### top - 实时进程监控
```bash
# 启动top
top

# 交互命令：
# P - 按CPU排序
# M - 按内存排序
# N - 按PID排序
# T - 按时间排序
# k - 杀死进程
# r - 修改优先级
# q - 退出

# 批处理模式
top -b -n 1

# 显示特定用户
top -u username

# 显示特定进程
top -p PID

# 指定刷新间隔（秒）
top -d 2
```

### htop - 增强版top
```bash
# 安装
sudo apt install htop

# 启动
htop

# 功能键：
# F1: 帮助  F2: 设置  F3: 搜索
# F4: 过滤  F5: 树状  F6: 排序
# F9: 杀死  F10: 退出
```

### pstree - 进程树
```bash
# 显示进程树
pstree

# 显示PID
pstree -p

# 显示特定用户
pstree username

# 显示特定进程
pstree PID
```

## 0x03 进程控制

### kill - 发送信号
```bash
# 发送SIGTERM（优雅终止）
kill PID

# 发送SIGKILL（强制终止）
kill -9 PID

# 发送SIGHUP（重新加载配置）
kill -HUP PID

# 发送SIGSTOP（暂停进程）
kill -STOP PID

# 发送SIGCONT（继续进程）
kill -CONT PID

# 列出所有信号
kill -l
```

### killall - 按名称杀死
```bash
# 按名称杀死进程
killall process_name

# 强制杀死
killall -9 process_name

# 交互式杀死
killall -i process_name

# 杀死特定用户的进程
killall -u username process_name
```

### pkill - 按模式杀死
```bash
# 按模式杀死进程
pkill pattern

# 强制杀死
pkill -9 pattern

# 杀死特定用户的进程
pkill -u username pattern

# 杀死特定终端的进程
pkill -t pts/0
```

### xkill - 图形界面杀死
```bash
# 启动xkill（点击窗口杀死）
xkill

# 指定光标
xkill -cursor_name pirate
```

## 0x04 进程优先级

### nice - 启动时设置优先级
```bash
# 启动进程并设置nice值（-20到19，默认0）
nice -n 10 command

# 以最高优先级启动
nice -n -20 command

# 以最低优先级启动
nice -n 19 command
```

### renice - 修改运行进程优先级
```bash
# 修改进程优先级
renice -n 10 -p PID

# 修改用户所有进程优先级
renice -n 10 -u username

# 修改组所有进程优先级
renice -n 10 -g groupname
```

## 0x05 后台进程

### & - 后台运行
```bash
# 后台运行命令
command &

# 后台运行并输出PID
command & echo $!

# 后台运行并重定向输出
command > output.log 2>&1 &
```

### jobs - 作业控制
```bash
# 列出后台作业
jobs

# 列出作业并显示PID
jobs -l

# 将作业放到前台
fg %job_number

# 将作业放到后台
bg %job_number

# 杀死作业
kill %job_number
```

### nohup - 忽略挂断
```bash
# 忽略挂断运行命令
nohup command &

# 忽略挂断并重定向输出
nohup command > output.log 2>&1 &

# 忽略挂断运行脚本
nohup ./script.sh &
```

### disown - 从作业表移除
```bash
# 从作业表移除
disown %job_number

# 移除并防止SIGHUP
disown -h %job_number

# 移除所有作业
disown -a
```

## 0x06 进程监控工具

### pgrep - 查找进程
```bash
# 按名称查找进程
pgrep process_name

# 显示完整命令
pgrep -a process_name

# 显示特定用户的进程
pgrep -u username process_name

# 显示最新进程
pgrep -n process_name

# 显示最旧进程
pgrep -o process_name
```

### pidof - 获取PID
```bash
# 获取进程PID
pidof process_name

# 获取单个PID
pidof -s process_name

# 获取所有PID
pidof -o PID process_name
```

### lsof - 列出打开文件
```bash
# 显示所有打开文件
lsof

# 显示特定进程打开的文件
lsof -p PID

# 显示特定用户打开的文件
lsof -u username

# 显示网络连接
lsof -i

# 显示特定端口
lsof -i :80

# 显示特定协议
lsof -i tcp
```

## 0x07 系统负载

### uptime - 系统运行时间
```bash
# 显示系统运行时间和负载
uptime

# 显示简短格式
uptime -s
```

### w - 系统负载
```bash
# 显示系统负载和用户活动
w

# 显示简短格式
w -s
```

### load average 解释
```
1分钟、5分钟、15分钟平均负载
< 1.0: 系统空闲
= 1.0: 系统满载
> 1.0: 系统过载
```

## 0x08 进程调试

### strace - 系统调用跟踪
```bash
# 跟踪系统调用
strace command

# 跟踪运行中的进程
strace -p PID

# 跟踪特定系统调用
strace -e trace=open,read command

# 统计系统调用
strace -c command

# 跟踪并输出时间戳
strace -t command
```

### ltrace - 库函数跟踪
```bash
# 跟踪库函数调用
ltrace command

# 跟踪运行中的进程
ltrace -p PID

# 统计库函数调用
ltrace -c command
```

## 参考

- [Linux进程管理指南](https://www.linux.com/training-tutorials/linux-process-management/)
- [ps命令手册](https://man7.org/linux/man-pages/man1/ps.1.html)
- [top命令手册](https://man7.org/linux/man-pages/man1/top.1.html)