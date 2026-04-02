# Linux文本处理

## 0x01 grep - 文本搜索

### 基本用法
```bash
# 基本搜索
grep "pattern" file.txt

# 忽略大小写
grep -i "pattern" file.txt

# 显示行号
grep -n "pattern" file.txt

# 反向匹配
grep -v "pattern" file.txt

# 统计匹配行数
grep -c "pattern" file.txt

# 显示匹配的文件名
grep -l "pattern" *.txt
```

### 高级用法
```bash
# 递归搜索目录
grep -r "pattern" /path/

# 使用正则表达式
grep -E "^[0-9]+" file.txt

# 显示匹配前后行
grep -A 3 "pattern" file.txt  # 显示匹配行后3行
grep -B 3 "pattern" file.txt  # 显示匹配行前3行
grep -C 3 "pattern" file.txt  # 显示匹配行前后各3行

# 只显示匹配部分
grep -o "pattern" file.txt

# 从多个文件搜索
grep "pattern" file1.txt file2.txt

# 排除特定文件
grep -r --exclude="*.log" "pattern" /path/
```

## 0x02 sed - 流编辑器

### 基本替换
```bash
# 基本替换
sed 's/old/new/' file.txt

# 全局替换
sed 's/old/new/g' file.txt

# 原地修改文件
sed -i 's/old/new/g' file.txt

# 备份原文件
sed -i.bak 's/old/new/g' file.txt

# 忽略大小写
sed 's/old/new/gi' file.txt
```

### 行操作
```bash
# 删除行
sed '2d' file.txt          # 删除第2行
sed '2,5d' file.txt        # 删除2-5行
sed '/pattern/d' file.txt  # 删除匹配行

# 插入行
sed '2i\new line' file.txt  # 在第2行前插入
sed '2a\new line' file.txt  # 在第2行后插入

# 替换行
sed '2c\new line' file.txt  # 替换第2行
```

### 高级用法
```bash
# 多条命令
sed -e 's/old1/new1/g' -e 's/old2/new2/g' file.txt

# 使用不同分隔符
sed 's|old|new|g' file.txt

# 匹配行范围
sed '/start/,/end/s/old/new/g' file.txt

# 仅打印匹配行
sed -n '/pattern/p' file.txt

# 打印特定行
sed -n '2,5p' file.txt
```

## 0x03 awk - 文本处理工具

### 基本用法
```bash
# 打印特定列
awk '{print $1}' file.txt        # 打印第1列
awk '{print $1, $3}' file.txt    # 打印第1和第3列
awk '{print $NF}' file.txt       # 打印最后一列

# 指定分隔符
awk -F: '{print $1}' /etc/passwd

# 打印行号
awk '{print NR, $0}' file.txt
```

### 条件处理
```bash
# 条件打印
awk '$1 > 100' file.txt          # 第1列大于100的行
awk '/pattern/' file.txt         # 匹配模式的行
awk '$1 == "value"' file.txt     # 第1列等于value的行

# 多条件
awk '$1 > 100 && $2 < 50' file.txt
```

### 内置变量
```bash
# NR - 当前行号
awk '{print NR, $0}' file.txt

# NF - 当前行字段数
awk '{print NF, $0}' file.txt

# FS - 输入字段分隔符
awk 'BEGIN{FS=":"} {print $1}' /etc/passwd

# OFS - 输出字段分隔符
awk 'BEGIN{OFS=" - "} {print $1, $2}' file.txt
```

### 高级用法
```bash
# 数学计算
awk '{sum += $1} END {print sum}' file.txt

# 条件求和
awk '$1 > 100 {sum += $1} END {print sum}' file.txt

# 格式化输出
awk '{printf "%-10s %5d\n", $1, $2}' file.txt

# 多文件处理
awk '{print FILENAME, $0}' file1.txt file2.txt

# 自定义函数
awk 'function add(a, b) {return a + b} {print add($1, $2)}' file.txt
```

## 0x04 cut - 列提取

### 基本用法
```bash
# 按字节提取
cut -b 1-5 file.txt

# 按字符提取
cut -c 1-10 file.txt

# 按字段提取
cut -f 1 file.txt

# 指定分隔符
cut -d: -f 1 /etc/passwd

# 提取多个字段
cut -f 1,3 file.txt

# 提取字段范围
cut -f 1-3 file.txt
```

### 高级用法
```bash
# 仅输出分隔符后的字段
cut -f 2- file.txt

# 互补选择
cut -f 1 --complement file.txt

# 不输出不包含分隔符的行
cut -f 1 -s file.txt
```

## 0x05 sort - 排序

### 基本用法
```bash
# 基本排序
sort file.txt

# 数字排序
sort -n file.txt

# 反向排序
sort -r file.txt

# 按特定字段排序
sort -k 2 file.txt

# 指定分隔符
sort -t: -k 3 /etc/passwd
```

### 高级用法
```bash
# 忽略大小写
sort -f file.txt

# 去重排序
sort -u file.txt

# 月份排序
sort -M file.txt

# 人类可读数字排序
sort -h file.txt

# 检查是否已排序
sort -c file.txt

# 合并已排序文件
sort -m sorted1.txt sorted2.txt
```

## 0x06 uniq - 去重

### 基本用法
```bash
# 去除连续重复行
uniq file.txt

# 统计重复次数
uniq -c file.txt

# 只显示重复行
uniq -d file.txt

# 只显示唯一行
uniq -u file.txt

# 忽略大小写
uniq -i file.txt

# 跳过前N个字段
uniq -f 2 file.txt
```

### 高级用法
```bash
# 指定比较字符数
uniq -w 5 file.txt

# 跳过前N个字符
uniq -s 3 file.txt

# 组合使用（先排序再去重）
sort file.txt | uniq -c | sort -nr
```

## 0x07 tr - 字符转换

### 基本用法
```bash
# 字符替换
echo "hello" | tr 'a-z' 'A-Z'

# 删除字符
echo "hello123" | tr -d '0-9'

# 压缩重复字符
echo "aaabbbccc" | tr -s 'a'

# 删除非字母数字字符
echo "hello@#$world" | tr -cd 'a-zA-Z0-9'
```

### 高级用法
```bash
# 多字符替换
echo "hello" | tr 'aeiou' '12345'

# 使用字符类
echo "hello123" | tr -d '[:digit:]'
echo "hello123" | tr -d '[:alpha:]'

# 补集操作
echo "hello123" | tr -c '0-9' '\n'
```

## 0x08 wc - 统计

### 基本用法
```bash
# 统计行数
wc -l file.txt

# 统计单词数
wc -w file.txt

# 统计字符数
wc -c file.txt

# 统计字节数
wc -m file.txt

# 显示所有统计
wc file.txt
```

### 高级用法
```bash
# 统计多个文件
wc *.txt

# 从标准输入统计
echo "hello world" | wc -w

# 统计目录中文件数
ls | wc -l
```

## 0x09 组合使用示例

### 日志分析
```bash
# 统计访问量最多的IP
cat access.log | awk '{print $1}' | sort | uniq -c | sort -nr | head -10

# 统计HTTP状态码
cat access.log | awk '{print $9}' | sort | uniq -c | sort -nr

# 统计特定时间段的日志
grep "2024-01-01" access.log | wc -l
```

### 系统管理
```bash
# 查找大文件
find / -type f -size +100M | xargs ls -lh | sort -k5 -hr

# 统计目录大小
du -sh * | sort -hr

# 监控日志文件
tail -f /var/log/syslog | grep --color "error"
```

### 数据处理
```bash
# CSV文件处理
awk -F, '{print $1, $3}' data.csv

# JSON数据处理
cat data.json | grep -o '"name":"[^"]*"' | cut -d'"' -f4

# 提取邮箱地址
grep -oE "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" file.txt
```

## 参考

- [grep使用指南](https://www.gnu.org/software/grep/manual/grep.html)
- [sed使用指南](https://www.gnu.org/software/sed/manual/sed.html)
- [awk使用指南](https://www.gnu.org/software/gawk/manual/gawk.html)
- [文本处理命令大全](https://www.linux.com/training-tutorials/linux-text-processing-commands/)