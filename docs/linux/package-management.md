# Linux包管理

## 0x01 包管理基础

Linux包管理器负责：
- 安装、更新、删除软件包
- 解决依赖关系
- 管理软件源
- 版本控制

主要包管理器：
- **APT**: Debian/Ubuntu
- **YUM/DNF**: RHEL/CentOS/Fedora
- **Pacman**: Arch Linux
- **Zypper**: openSUSE

## 0x02 APT包管理

### 基本操作
```bash
# 更新软件源
sudo apt update

# 升级所有软件包
sudo apt upgrade

# 安装软件包
sudo apt install package_name

# 安装特定版本
sudo apt install package_name=version

# 删除软件包
sudo apt remove package_name

# 删除软件包并删除配置文件
sudo apt purge package_name

# 自动删除不需要的依赖
sudo apt autoremove
```

### 查询操作
```bash
# 搜索软件包
apt search keyword

# 显示软件包信息
apt show package_name

# 列出所有已安装软件包
apt list --installed

# 列出可升级软件包
apt list --upgradable

# 检查软件包是否安装
dpkg -l | grep package_name
```

### 高级操作
```bash
# 清理软件包缓存
sudo apt clean

# 清理旧版本软件包缓存
sudo apt autoclean

# 下载软件包但不安装
apt download package_name

# 安装本地软件包
sudo dpkg -i package.deb

# 修复依赖关系
sudo apt -f install

# 锁定软件包版本
sudo apt-mark hold package_name

# 解锁软件包版本
sudo apt-mark unhold package_name
```

### 软件源管理
```bash
# 添加软件源
sudo add-apt-repository ppa:repository/name

# 删除软件源
sudo add-apt-repository --remove ppa:repository/name

# 编辑软件源列表
sudo nano /etc/apt/sources.list

# 添加软件源（手动）
echo "deb http://example.com/ubuntu focal main" | sudo tee -a /etc/apt/sources.list

# 添加GPG密钥
wget -qO - https://example.com/key.gpg | sudo apt-key add -
```

## 0x03 YUM/DNF包管理

### YUM基本操作
```bash
# 更新软件源
sudo yum check-update

# 升级所有软件包
sudo yum update

# 安装软件包
sudo yum install package_name

# 删除软件包
sudo yum remove package_name

# 搜索软件包
yum search keyword

# 显示软件包信息
yum info package_name

# 列出所有已安装软件包
yum list installed

# 清理缓存
sudo yum clean all
```

### DNF基本操作（YUM的替代品）
```bash
# 更新软件源
sudo dnf check-update

# 升级所有软件包
sudo dnf upgrade

# 安装软件包
sudo dnf install package_name

# 删除软件包
sudo dnf remove package_name

# 搜索软件包
dnf search keyword

# 显示软件包信息
dnf info package_name

# 列出所有已安装软件包
dnf list installed

# 清理缓存
sudo dnf clean all
```

### 仓库管理
```bash
# 列出仓库
yum repolist

# 启用仓库
sudo yum-config-manager --enable repository

# 禁用仓库
sudo yum-config-manager --disable repository

# 添加仓库
sudo yum-config-manager --add-repo repository_url
```

## 0x04 Pacman包管理

### 基本操作
```bash
# 同步软件源
sudo pacman -Sy

# 升级所有软件包
sudo pacman -Syu

# 安装软件包
sudo pacman -S package_name

# 删除软件包
sudo pacman -R package_name

# 删除软件包并删除依赖
sudo pacman -Rs package_name

# 搜索软件包
pacman -Ss keyword

# 显示软件包信息
pacman -Si package_name

# 列出所有已安装软件包
pacman -Q
```

### 高级操作
```bash
# 清理软件包缓存
sudo pacman -Sc

# 下载软件包但不安装
pacman -Sw package_name

# 安装本地软件包
sudo pacman -U package.pkg.tar.zst

# 检查软件包依赖
pactree package_name

# 显示软件包文件
pacman -Ql package_name
```

## 0x05 Snap包管理

### 基本操作
```bash
# 安装Snap
sudo apt install snapd

# 安装Snap软件包
sudo snap install package_name

# 删除Snap软件包
sudo snap remove package_name

# 列出所有Snap软件包
snap list

# 搜索Snap软件包
snap find keyword

# 更新所有Snap软件包
sudo snap refresh

# 更新特定Snap软件包
sudo snap refresh package_name
```

## 0x06 Flatpak包管理

### 基本操作
```bash
# 安装Flatpak
sudo apt install flatpak

# 添加Flathub仓库
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# 安装Flatpak软件包
flatpak install flathub package_name

# 删除Flatpak软件包
flatpak uninstall package_name

# 列出所有Flatpak软件包
flatpak list

# 搜索Flatpak软件包
flatpak search keyword

# 更新所有Flatpak软件包
flatpak update
```

## 0x07 源码编译安装

### 基本步骤
```bash
# 1. 下载源码
wget https://example.com/source.tar.gz

# 2. 解压源码
tar -xzvf source.tar.gz

# 3. 进入源码目录
cd source/

# 4. 配置
./configure --prefix=/usr/local

# 5. 编译
make

# 6. 安装
sudo make install

# 7. 清理编译文件
make clean
```

### 高级配置
```bash
# 查看配置选项
./configure --help

# 指定安装路径
./configure --prefix=/opt/custom

# 启用/禁用功能
./configure --enable-feature --disable-feature

# 指定依赖库路径
./configure --with-library=/path/to/library

# 并行编译（加快速度）
make -j$(nproc)
```

## 0x08 软件包验证

### GPG验证
```bash
# 导入GPG密钥
wget -qO - https://example.com/key.gpg | sudo apt-key add -

# 验证软件包
gpg --verify package.sig package.deb

# 列出已导入密钥
apt-key list

# 删除密钥
sudo apt-key del key_id
```

### 校验和验证
```bash
# 计算MD5校验和
md5sum file

# 计算SHA256校验和
sha256sum file

# 验证校验和
echo "checksum  file" | sha256sum -c
```

## 参考

- [APT使用指南](https://wiki.debian.org/Apt)
- [YUM使用指南](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_basic_system_settings/using-yum-to-manage-software_configuring-basic-system-settings)
- [Pacman使用指南](https://wiki.archlinux.org/title/pacman)
- [Snap使用指南](https://snapcraft.io/docs)
- [Flatpak使用指南](https://docs.flatpak.org/en/latest/)