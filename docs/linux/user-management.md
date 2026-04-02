# Linux用户管理

## 0x01 用户基础

Linux用户分为三类：
- **超级用户（root）**: UID=0，拥有所有权限
- **系统用户**: UID=1-999，用于运行系统服务
- **普通用户**: UID=1000+，日常使用

用户信息存储在 `/etc/passwd`，密码存储在 `/etc/shadow`。

## 0x02 用户查看

### whoami - 当前用户
```bash
# 显示当前用户名
whoami

# 显示当前用户ID
id

# 显示当前用户详细信息
id username
```

### who - 登录用户
```bash
# 显示当前登录用户
who

# 显示详细登录信息
who -a

# 显示当前终端
who am i
```

### w - 用户活动
```bash
# 显示用户活动
w

# 显示特定用户
w username

# 显示简短信息
w -s
```

### last - 登录历史
```bash
# 显示最近登录
last

# 显示特定用户登录历史
last username

# 显示特定终端登录历史
last pts/0

# 显示最近N条记录
last -n 10
```

## 0x03 用户管理

### useradd - 创建用户
```bash
# 基本创建用户
sudo useradd username

# 创建用户并指定主目录
sudo useradd -m username

# 创建用户并指定shell
sudo useradd -s /bin/bash username

# 创建用户并指定UID
sudo useradd -u 1001 username

# 创建用户并指定组
sudo useradd -g groupname username

# 创建用户并添加到附加组
sudo useradd -G group1,group2 username

# 创建系统用户
sudo useradd -r username
```

### usermod - 修改用户
```bash
# 修改用户名
sudo usermod -l newname oldname

# 修改用户主目录
sudo usermod -d /new/home -m username

# 修改用户shell
sudo usermod -s /bin/zsh username

# 修改用户UID
sudo usermod -u 1002 username

# 添加用户到组
sudo usermod -aG groupname username

# 修改用户主组
sudo usermod -g groupname username

# 锁定用户
sudo usermod -L username

# 解锁用户
sudo usermod -U username

# 设置用户过期日期
sudo usermod -e 2024-12-31 username
```

### userdel - 删除用户
```bash
# 删除用户（保留主目录）
sudo userdel username

# 删除用户并删除主目录
sudo userdel -r username

# 强制删除用户
sudo userdel -f username
```

### passwd - 密码管理
```bash
# 修改当前用户密码
passwd

# 修改其他用户密码（需要root）
sudo passwd username

# 锁定用户密码
sudo passwd -l username

# 解锁用户密码
sudo passwd -u username

# 设置密码过期时间
sudo passwd -e username

# 显示密码状态
sudo passwd -S username
```

## 0x04 组管理

### groupadd - 创建组
```bash
# 创建组
sudo groupadd groupname

# 创建组并指定GID
sudo groupadd -g 1001 groupname

# 创建系统组
sudo groupadd -r groupname
```

### groupmod - 修改组
```bash
# 修改组名
sudo groupmod -n newname oldname

# 修改GID
sudo groupmod -g 1002 groupname
```

### groupdel - 删除组
```bash
# 删除组
sudo groupdel groupname
```

### groups - 用户组
```bash
# 显示当前用户所属组
groups

# 显示特定用户所属组
groups username
```

## 0x05 用户信息文件

### /etc/passwd - 用户信息
```bash
# 查看用户信息文件
cat /etc/passwd

# 格式：username:password:UID:GID:comment:home:shell
# 示例：user:x:1000:1000:User:/home/user:/bin/bash
```

### /etc/shadow - 密码信息
```bash
# 查看密码信息文件（需要root）
sudo cat /etc/shadow

# 格式：username:password:lastchange:min:max:warn:inactive:expire:reserved
```

### /etc/group - 组信息
```bash
# 查看组信息文件
cat /etc/group

# 格式：groupname:password:GID:members
```

## 0x06 用户切换

### su - 切换用户
```bash
# 切换到其他用户
su username

# 切换到root
su -

# 切换用户并执行命令
su -c "command" username

# 切换用户并保留环境
su -m username
```

### sudo - 以其他用户身份执行
```bash
# 以root身份执行命令
sudo command

# 以其他用户身份执行命令
sudo -u username command

# 以root身份执行交互式shell
sudo -i

# 以root身份执行登录shell
sudo -s

# 列出用户权限
sudo -l
```

## 0x07 用户监控

### lastlog - 最后登录
```bash
# 显示所有用户最后登录
lastlog

# 显示特定用户最后登录
lastlog -u username

# 显示最近登录的用户
lastlog -t 30
```

### faillog - 登录失败
```bash
# 显示登录失败记录
sudo faillog

# 显示特定用户登录失败
sudo faillog -u username

# 重置失败计数
sudo faillog -r username
```

## 参考

- [Linux用户管理指南](https://www.linux.com/training-tutorials/linux-user-management/)
- [useradd命令手册](https://man7.org/linux/man-pages/man8/useradd.8.html)
- [Linux用户和组管理](https://www.linux.com/training-tutorials/linux-users-and-groups/)