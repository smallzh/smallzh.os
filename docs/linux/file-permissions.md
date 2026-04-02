# Linux文件权限

## 0x01 权限基础

Linux文件权限分为三类：
- **所有者（User）**: 文件创建者
- **组（Group）**: 文件所属组
- **其他（Other）**: 其他用户

每种权限类型：
- **读（r）**: 读取文件内容或列出目录
- **写（w）**: 修改文件或目录中创建/删除文件
- **执行（x）**: 执行文件或进入目录

## 0x02 权限查看

### ls -l 查看权限
```bash
# 查看文件详细信息
ls -l file.txt
# 输出示例：-rw-r--r-- 1 user group 1024 Jan 1 12:00 file.txt

# 查看目录详细信息
ls -ld directory/
# 输出示例：drwxr-xr-x 2 user group 4096 Jan 1 12:00 directory/

# 递归查看目录权限
ls -lR directory/
```

权限字符串解析：
- 第1位：文件类型（-=普通文件，d=目录，l=符号链接）
- 第2-4位：所有者权限（rwx）
- 第5-7位：组权限（rwx）
- 第8-10位：其他用户权限（rwx）

## 0x03 chmod - 修改权限

### 数字模式
```bash
# 权限数字表示：
# r=4, w=2, x=1
# 组合：rwx=7, rw-=6, r-x=5, r--=4

# 设置权限为755（rwxr-xr-x）
chmod 755 file.txt

# 设置权限为644（rw-r--r--）
chmod 644 file.txt

# 设置权限为700（rwx------）
chmod 700 private.key

# 递归设置目录权限
chmod -R 755 directory/
```

### 符号模式
```bash
# 添加权限
chmod +x script.sh          # 添加执行权限
chmod u+w file.txt          # 所有者添加写权限
chmod g+r file.txt          # 组添加读权限
chmod o-r file.txt          # 其他用户移除读权限

# 设置权限
chmod u=rwx file.txt        # 所有者设置为rwx
chmod g=rx file.txt         # 组设置为r-x
chmod o=r file.txt          # 其他用户设置为r--

# 组合操作
chmod u+x,g+x,o+x script.sh  # 所有用户添加执行权限
chmod go-rwx private.txt     # 组和其他用户移除所有权限
```

### 特殊权限
```bash
# 设置SUID（Set User ID）
chmod u+s executable

# 设置SGID（Set Group ID）
chmod g+s directory

# 设置粘滞位（Sticky Bit）
chmod +t directory

# 数字模式设置特殊权限
chmod 4755 executable        # SUID
chmod 2755 directory         # SGID
chmod 1755 directory         # 粘滞位
```

## 0x04 chown - 修改所有者

### 基本用法
```bash
# 修改所有者
chown user file.txt

# 修改所有者和组
chown user:group file.txt

# 只修改组
chown :group file.txt

# 递归修改目录
chown -R user:group directory/

# 修改符号链接指向的文件
chown -h user symlink
```

### 参考其他文件的所有权
```bash
# 参考其他文件的所有权
chown --reference=ref_file.txt target_file.txt
```

## 0x05 chgrp - 修改组

```bash
# 修改文件组
chgrp group file.txt

# 递归修改目录组
chgrp -R group directory/

# 参考其他文件的组
chgrp --reference=ref_file.txt target_file.txt
```

## 0x06 权限检查

### 查看文件权限
```bash
# 查看文件权限
stat file.txt

# 查看文件类型
file file.txt

# 检查文件是否可执行
test -x file.txt && echo "Executable" || echo "Not executable"
```

### 查看目录权限
```bash
# 查看目录权限
stat directory/

# 检查目录是否可进入
test -d directory/ && echo "Is directory"
```

## 0x07 ACL（访问控制列表）

### 设置ACL
```bash
# 设置用户ACL
setfacl -m u:user:rwx file.txt

# 设置组ACL
setfacl -m g:group:rx file.txt

# 设置默认ACL（目录）
setfacl -d -m u:user:rwx directory/

# 递归设置ACL
setfacl -R -m u:user:rwx directory/
```

### 查看ACL
```bash
# 查看文件ACL
getfacl file.txt

# 查看目录ACL
getfacl directory/
```

### 删除ACL
```bash
# 删除特定用户ACL
setfacl -x u:user file.txt

# 删除所有ACL
setfacl -b file.txt
```

## 0x08 权限应用场景

### Web服务器权限
```bash
# Web文件权限（推荐）
chmod 644 *.html
chmod 755 cgi-bin/

# Web目录权限
chmod 755 /var/www/html/
chown -R www-data:www-data /var/www/html/
```

### 脚本权限
```bash
# 脚本执行权限
chmod +x script.sh

# 脚本目录权限
chmod 755 scripts/
```

### 敏感文件权限
```bash
# SSH密钥权限
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
chmod 700 ~/.ssh/

# 配置文件权限
chmod 600 /etc/shadow
chmod 644 /etc/passwd
```

## 参考

- [Linux文件权限详解](https://www.linux.com/training-tutorials/understanding-linux-file-permissions/)
- [chmod命令手册](https://man7.org/linux/man-pages/man1/chmod.1.html)
- [ACL使用指南](https://wiki.archlinux.org/title/Access_Control_Lists)