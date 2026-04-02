# Linux系统信息

## 0x01 系统基本信息

### uname - 系统信息
```bash
# 显示所有系统信息
uname -a

# 显示内核名称
uname -s

# 显示内核版本
uname -r

# 显示主机名
uname -n

# 显示机器硬件名称
uname -m

# 显示操作系统名称
uname -o
```

### hostname - 主机名管理
```bash
# 显示主机名
hostname

# 设置主机名（临时）
sudo hostname new-hostname

# 永久设置主机名（编辑/etc/hostname）
sudo nano /etc/hostname
```

### lsb_release - 发行版信息
```bash
# 显示发行版信息
lsb_release -a

# 显示发行版描述
lsb_release -d

# 显示发行版版本
lsb_release -r
```

## 0x02 磁盘空间管理

### df - 磁盘使用情况
```bash
# 显示磁盘使用情况
df

# 人类可读格式
df -h

# 显示文件系统类型
df -T

# 显示特定文件系统
df /dev/sda1

# 显示inode使用情况
df -i
```

### du - 目录大小
```bash
# 显示当前目录大小
du

# 人类可读格式
du -h

# 显示总大小
du -sh

# 显示子目录大小
du -h --max-depth=1

# 排序显示
du -h | sort -hr
```

### lsblk - 块设备信息
```bash
# 列出块设备
lsblk

# 显示文件系统信息
lsblk -f

# 显示详细信息
lsblk -m

# 显示特定设备
lsblk /dev/sda
```

## 0x03 内存管理

### free - 内存使用情况
```bash
# 显示内存使用情况
free

# 人类可读格式
free -h

# 以MB为单位显示
free -m

# 以GB为单位显示
free -g

# 持续监控（每2秒更新）
free -h -s 2
```

### vmstat - 虚拟内存统计
```bash
# 显示虚拟内存统计
vmstat

# 每2秒更新一次，共5次
vmstat 2 5

# 显示详细统计
vmstat -d

# 显示内存统计
vmstat -s
```

## 0x04 进程管理

### ps - 进程状态
```bash
# 显示所有进程
ps aux

# 显示进程树
ps -ef --forest

# 显示特定用户的进程
ps -u username

# 显示特定进程
ps -p PID

# 自定义输出格式
ps -eo pid,ppid,cmd,%mem,%cpu
```

### top - 实时进程监控
```bash
# 启动top
top

# 按CPU使用率排序（运行中按P）
# 按内存使用率排序（运行中按M）
# 按进程ID排序（运行中按N）
# 杀死进程（运行中按k，然后输入PID）

# 批处理模式
top -b -n 1

# 显示特定用户的进程
top -u username
```

### htop - 增强版top
```bash
# 安装htop
sudo apt install htop

# 启动htop
htop

# 功能键：
# F1: 帮助
# F2: 设置
# F3: 搜索
# F4: 过滤
# F5: 树状显示
# F6: 排序
# F9: 杀死进程
# F10: 退出
```

## 0x05 系统日志

### journalctl - systemd日志
```bash
# 显示所有日志
journalctl

# 显示最近日志
journalctl -n 100

# 实时跟踪日志
journalctl -f

# 显示特定服务的日志
journalctl -u service-name

# 按时间过滤
journalctl --since "2024-01-01" --until "2024-01-02"

# 显示内核日志
journalctl -k
```

### dmesg - 内核日志
```bash
# 显示内核日志
dmesg

# 显示最近日志
dmesg | tail

# 实时跟踪
dmesg -w

# 显示时间戳
dmesg -T

# 显示特定级别的消息
dmesg -l err
```

## 0x06 系统性能监控

### iostat - I/O统计
```bash
# 显示I/O统计
iostat

# 每2秒更新一次，共5次
iostat 2 5

# 显示详细统计
iostat -x

# 显示特定设备
iostat -p sda
```

### sar - 系统活动报告
```bash
# 显示CPU使用情况
sar -u

# 显示内存使用情况
sar -r

# 显示I/O统计
sar -b

# 显示网络统计
sar -n DEV

# 每2秒采样一次，共5次
sar -u 2 5
```

## 参考

- [Linux系统管理命令](https://www.linux.com/training-tutorials/linux-system-monitoring-commands/)
- [性能监控工具](https://www.brendangregg.com/linuxperf.html)