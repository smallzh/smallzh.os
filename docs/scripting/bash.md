# Bash脚本编程

## 0x01 脚本基础

### 创建脚本
```bash
#!/bin/bash
# 这是一个注释
echo "Hello, World!"
```

### 运行脚本
```bash
# 添加执行权限
chmod +x script.sh

# 运行脚本
./script.sh

# 使用bash运行
bash script.sh

# 使用source运行（在当前shell执行）
source script.sh
```

## 0x02 变量

### 变量定义
```bash
# 定义变量（等号两边不能有空格）
name="John"
age=25
readonly pi=3.14159

# 使用变量
echo $name
echo ${name}

# 删除变量
unset name

# 环境变量
export PATH="/usr/local/bin:$PATH"
```

### 特殊变量
```bash
# 脚本参数
echo "脚本名: $0"
echo "第一个参数: $1"
echo "第二个参数: $2"
echo "参数个数: $#"
echo "所有参数: $*"
echo "所有参数(数组): $@"
echo "进程ID: $$"
echo "上个命令退出状态: $?"
```

### 字符串操作
```bash
# 字符串长度
echo ${#name}

# 字符串拼接
greeting="Hello, $name!"

# 子字符串
echo ${name:0:3}

# 字符串替换
echo ${name/John/Jane}

# 字符串删除
echo ${name#J}   # 删除开头的J
echo ${name%hn}  # 删除结尾的hn
```

### 数组
```bash
# 定义数组
fruits=("apple" "banana" "orange")

# 访问数组元素
echo ${fruits[0]}

# 数组长度
echo ${#fruits[@]}

# 所有元素
echo ${fruits[@]}

# 添加元素
fruits+=("grape")

# 删除元素
unset fruits[1]
```

## 0x03 条件判断

### if语句
```bash
# 基本if
if [ condition ]; then
    commands
fi

# if-else
if [ condition ]; then
    commands
else
    commands
fi

# if-elif-else
if [ condition1 ]; then
    commands
elif [ condition2 ]; then
    commands
else
    commands
fi
```

### 条件测试
```bash
# 文件测试
[ -f file.txt ]    # 文件存在
[ -d directory ]   # 目录存在
[ -r file.txt ]    # 可读
[ -w file.txt ]    # 可写
[ -x file.txt ]    # 可执行
[ -s file.txt ]    # 文件非空
[ -e file.txt ]    # 文件存在（包括目录）

# 字符串测试
[ -z "$str" ]      # 字符串为空
[ -n "$str" ]      # 字符串非空
[ "$str1" = "$str2" ]   # 字符串相等
[ "$str1" != "$str2" ]  # 字符串不相等

# 数值测试
[ $num1 -eq $num2 ]   # 等于
[ $num1 -ne $num2 ]   # 不等于
[ $num1 -lt $num2 ]   # 小于
[ $num1 -le $num2 ]   # 小于等于
[ $num1 -gt $num2 ]   # 大于
[ $num1 -ge $num2 ]   # 大于等于

# 逻辑测试
[ condition1 -a condition2 ]   # AND
[ condition1 -o condition2 ]   # OR
[ ! condition ]                # NOT
```

### case语句
```bash
case $variable in
    pattern1)
        commands
        ;;
    pattern2)
        commands
        ;;
    *)
        default_commands
        ;;
esac
```

## 0x04 循环

### for循环
```bash
# 列表循环
for item in item1 item2 item3; do
    echo $item
done

# 范围循环
for i in {1..10}; do
    echo $i
done

# C风格循环
for ((i=0; i<10; i++)); do
    echo $i
done

# 命令替换循环
for file in $(ls *.txt); do
    echo $file
done
```

### while循环
```bash
# 基本while
count=0
while [ $count -lt 10 ]; do
    echo $count
    ((count++))
done

# 读取文件
while read line; do
    echo $line
done < file.txt

# 无限循环
while true; do
    echo "Running..."
    sleep 1
done
```

### until循环
```bash
# until循环（条件为真时停止）
count=0
until [ $count -ge 10 ]; do
    echo $count
    ((count++))
done
```

### 循环控制
```bash
# break - 跳出循环
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        break
    fi
    echo $i
done

# continue - 跳过本次循环
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        continue
    fi
    echo $i
done
```

## 0x05 函数

### 函数定义
```bash
# 方法1
function_name() {
    commands
}

# 方法2
function function_name {
    commands
}
```

### 函数调用
```bash
# 定义函数
greet() {
    echo "Hello, $1!"
}

# 调用函数
greet "John"
```

### 函数参数
```bash
# 函数内部参数
process_args() {
    echo "第一个参数: $1"
    echo "参数个数: $#"
    echo "所有参数: $@"
}

process_args arg1 arg2 arg3
```

### 返回值
```bash
# 使用return（0-255）
check_status() {
    if [ condition ]; then
        return 0  # 成功
    else
        return 1  # 失败
    fi
}

# 使用echo返回结果
get_sum() {
    local sum=$(( $1 + $2 ))
    echo $sum
}

result=$(get_sum 5 3)
echo "和是: $result"
```

## 0x06 输入输出

### 读取输入
```bash
# 读取变量
read -p "请输入名字: " name
echo "你好, $name"

# 读取密码
read -s -p "请输入密码: " password

# 读取超时
read -t 5 -p "5秒内输入: " input

# 读取数组
read -a arr -p "输入多个值: "
echo ${arr[@]}
```

### 输出重定向
```bash
# 输出到文件
echo "内容" > file.txt

# 追加到文件
echo "内容" >> file.txt

# 错误重定向
command 2> error.log

# 同时重定向标准输出和错误
command > output.log 2>&1

# 丢弃输出
command > /dev/null 2>&1
```

### 管道
```bash
# 管道连接命令
ls -la | grep ".txt"

# 多重管道
cat file.txt | grep "pattern" | sort | uniq
```

## 0x07 调试

### 调试选项
```bash
# 显示执行的命令
bash -x script.sh

# 在脚本中启用调试
set -x  # 启用
set +x  # 禁用

# 显示未定义变量
set -u

# 命令失败时退出
set -e

# 管道失败时退出
set -o pipefail
```

### 调试技巧
```bash
# 打印变量
echo "DEBUG: variable=$variable"

# 打印执行位置
echo "DEBUG: 执行到第 $LINENO 行"

# 使用trap捕获错误
trap 'echo "错误发生在第 $LINENO 行"; exit 1' ERR
```

## 0x08 实用示例

### 备份脚本
```bash
#!/bin/bash

# 备份目录
SOURCE="/home/user/documents"
BACKUP="/backup"
DATE=$(date +%Y%m%d)

# 创建备份
tar -czf "$BACKUP/backup_$DATE.tar.gz" "$SOURCE"

# 检查结果
if [ $? -eq 0 ]; then
    echo "备份成功: backup_$DATE.tar.gz"
else
    echo "备份失败"
    exit 1
fi
```

### 监控脚本
```bash
#!/bin/bash

# 监控磁盘空间
THRESHOLD=90
USAGE=$(df / | tail -1 | awk '{print $5}' | sed 's/%//')

if [ $USAGE -gt $THRESHOLD ]; then
    echo "警告: 磁盘空间不足，使用率: $USAGE%"
    # 发送邮件或其他操作
fi
```

### 日志脚本
```bash
#!/bin/bash

# 日志函数
log() {
    local level=$1
    local message=$2
    local timestamp=$(date +"%Y-%m-%d %H:%M:%S")
    echo "[$timestamp] [$level] $message" >> script.log
}

# 使用日志
log "INFO" "脚本开始执行"
log "WARNING" "发生警告"
log "ERROR" "发生错误"
```

## 参考

- [Bash脚本指南](https://www.gnu.org/software/bash/manual/bash.html)
- [高级Bash脚本编程](https://tldp.org/LDP/abs/html/)
- [Bash脚本最佳实践](https://google.github.io/styleguide/shellguide.html)