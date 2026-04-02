# Linux基础命令

## 0x01 目录导航命令

### pwd - 显示当前工作目录
```bash
# 显示当前目录
pwd

# 显示逻辑路径（包含符号链接）
pwd -L

# 显示物理路径（解析符号链接）
pwd -P
```

### cd - 切换目录
```bash
# 切换到指定目录
cd /home/user

# 切换到用户主目录
cd ~
cd

# 切换到上级目录
cd ..

# 切换到上次访问的目录
cd -

# 切换到当前目录（无操作）
cd .
```

### ls - 列出目录内容
```bash
# 列出当前目录文件
ls

# 列出详细信息
ls -l

# 列出所有文件（包括隐藏文件）
ls -a

# 组合使用：详细列表包含隐藏文件
ls -la

# 按时间排序
ls -lt

# 递归列出子目录
ls -R

# 显示文件大小（人类可读）
ls -lh
```

## 0x02 文件操作命令

### touch - 创建空文件或更新时间戳
```bash
# 创建空文件
touch file.txt

# 创建多个文件
touch file1.txt file2.txt file3.txt

# 更新文件时间戳
touch existing_file.txt

# 设置特定时间戳
touch -t 202401011200 file.txt
```

### mkdir - 创建目录
```bash
# 创建单个目录
mkdir mydir

# 创建多级目录
mkdir -p parent/child/grandchild

# 设置权限创建目录
mkdir -m 755 mydir
```

### cp - 复制文件或目录
```bash
# 复制文件
cp file1.txt file2.txt

# 复制文件到目录
cp file.txt /backup/

# 递归复制目录
cp -r dir1/ dir2/

# 保留属性复制
cp -p file.txt backup/

# 交互式复制（覆盖前提示）
cp -i file.txt existing.txt

# 复制并显示进度
cp -v file.txt backup/
```

### mv - 移动或重命名文件
```bash
# 重命名文件
mv oldname.txt newname.txt

# 移动文件到目录
mv file.txt /backup/

# 移动目录
mv dir1/ /backup/

# 交互式移动（覆盖前提示）
mv -i file.txt existing.txt

# 不覆盖已存在文件
mv -n file.txt existing.txt
```

### rm - 删除文件或目录
```bash
# 删除文件
rm file.txt

# 交互式删除（确认提示）
rm -i file.txt

# 强制删除
rm -f file.txt

# 递归删除目录
rm -r dir/

# 递归强制删除目录
rm -rf dir/

# 删除前显示信息
rm -v file.txt
```

## 0x03 文件查看命令

### cat - 连接文件并打印到标准输出
```bash
# 查看文件内容
cat file.txt

# 显示行号
cat -n file.txt

# 显示非打印字符
cat -v file.txt

# 合并多个文件
cat file1.txt file2.txt > combined.txt
```

### less - 分页查看文件
```bash
# 分页查看文件
less file.txt

# 显示行号
less -N file.txt

# 搜索文本（在less中输入 /keyword）
less file.txt
```

### head - 显示文件开头
```bash
# 显示前10行
head file.txt

# 指定行数
head -n 20 file.txt

# 显示前5个字节
head -c 5 file.txt
```

### tail - 显示文件结尾
```bash
# 显示最后10行
tail file.txt

# 指定行数
tail -n 20 file.txt

# 实时追踪文件更新
tail -f /var/log/syslog

# 显示最后5个字节
tail -c 5 file.txt
```

## 0x04 文本搜索命令

### grep - 文本搜索
```bash
# 基本搜索
grep "pattern" file.txt

# 忽略大小写
grep -i "pattern" file.txt

# 显示行号
grep -n "pattern" file.txt

# 反向匹配（不匹配的行）
grep -v "pattern" file.txt

# 递归搜索目录
grep -r "pattern" /path/

# 显示匹配的文件名
grep -l "pattern" *.txt

# 使用正则表达式
grep -E "^[0-9]+" file.txt

# 统计匹配行数
grep -c "pattern" file.txt
```

### find - 文件搜索
```bash
# 按名称搜索
find /path -name "*.txt"

# 忽略大小写搜索
find /path -iname "*.txt"

# 按类型搜索（f=文件，d=目录）
find /path -type f
find /path -type d

# 按大小搜索
find /path -size +100M
find /path -size -1k

# 按时间搜索（7天内修改）
find /path -mtime -7

# 搜索并执行命令
find /path -name "*.txt" -exec ls -l {} \;

# 搜索并删除
find /path -name "*.tmp" -delete
```

## 0x05 文件权限命令

### chmod - 修改权限
```bash
# 数字模式设置权限
chmod 755 file.txt
chmod 644 file.txt

# 符号模式添加权限
chmod +x script.sh
chmod u+w file.txt
chmod g+r file.txt
chmod o-r file.txt

# 递归修改目录权限
chmod -R 755 directory/
```

### chown - 修改所有者
```bash
# 修改所有者
chown user file.txt

# 修改所有者和组
chown user:group file.txt

# 递归修改目录
chown -R user:group directory/
```

## 0x06 压缩解压命令

### tar - 打包工具
```bash
# 创建tar包
tar -cvf archive.tar files/

# 创建gzip压缩包
tar -czvf archive.tar.gz files/

# 创建bzip2压缩包
tar -cjvf archive.tar.bz2 files/

# 解压tar包
tar -xvf archive.tar

# 解压gzip压缩包
tar -xzvf archive.tar.gz

# 解压bzip2压缩包
tar -xjvf archive.tar.bz2

# 查看压缩包内容
tar -tvf archive.tar.gz
```

### zip/unzip - ZIP压缩
```bash
# 压缩文件
zip archive.zip file1.txt file2.txt

# 递归压缩目录
zip -r archive.zip directory/

# 解压文件
unzip archive.zip

# 解压到指定目录
unzip archive.zip -d /path/

# 查看压缩包内容
unzip -l archive.zip
```

### gzip/gunzip - GZIP压缩
```bash
# 压缩文件
gzip file.txt

# 解压文件
gunzip file.txt.gz

# 保留原文件压缩
gzip -k file.txt

# 指定压缩级别（1-9）
gzip -9 file.txt
```

## 参考

- [Linux命令手册](https://man7.org/linux/man-pages/)
- [Linux命令大全](https://www.linuxcool.com/)