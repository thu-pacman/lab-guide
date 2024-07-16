# 操作系统安装

## 硬盘分区

- **必须** 使用 GPT 分区表
- 推荐使用 **至少两块硬盘的硬件 RAID1** 作为系统盘，以保证可用性。
- 推荐使用 `fdisk`, `gdisk` 或者 `sgdisk` 创建分区，推荐的分区表为：
    - 1MB 的 BIOS boot 分区（类别为 `ef02`），无须格式化
    - （如果使用 ZFS 作为根文件系统）1GB 的 boot 分区（类别为 `8300`），格式化为 ext4，挂载到 `/boot`
    - 256MB 的 EFI 系统分区（类别为 `ef00`），格式化为 FAT32，挂载到 `/boot/efi`
    - 剩余空间均分给根分区（类别为 `8300`），格式化为 ext4，挂载到 `/`
    - 不推荐保留 SWAP 分区

## 系统安装

!!! success "请使用 Debian"

    除非有必要理由，请使用最新的 Debian 作为基本系统。不推荐使用 Ubuntu， **强烈不建议** 任何 RHEL 系发行版（如 Rocky Linux 、Alma Linux 等）。

1. 将服务器连接到网络，启动 BMC 的虚拟控制台，选择网络引导；
2. 在 iPXE 菜单中，选择 Debian，并勾上“Install Additional Firmware”后启动；
3. 在 Debian 安装器中的配置：
    1. 区域选择 Other -> Asia -> Shanghai；
    2. 语言/键盘等均选择 English；
    3. 网络配置选择 DHCP，此时应该可以自动识别出正确的主机名、域名等信息；
    4. 磁盘分区选择手动，按照上述要求进行分区（或者配置）；
    5. 创建管理用户 `pacman`；`root` 和 `pacman` 用户使用相同的密码（按规则分配并记录）；
    6. 安装基本系统，并在 `tasksel` 界面选择 SSH 服务器和标准系统工具， **不要选择** 图形界面或者 Web 服务器；
 4. 重启进入系统，测试 SSH 并进行下一步配置。

## 安装后配置

### 安装必要软件

使用 APT 安装以下软件：

```text
git
vim
wget
curl
rsync
build-essential
etckeeper
tmux
htop
iotop
fail2ban
fish
systemd-timesyncd
```

在集群的管理结点，增加安装 `clustershell`。

### 基本配置

在 APT 软件源中，启用 backports，并确保所有组件都已经启用，如：

```apt
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware 
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware 

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware 
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware 

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware 
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware 

deb https://mirrors.tuna.tsinghua.edu.cn/debian-security/ bookworm-security main contrib non-free non-free-firmware 
```

使用 TUNA NTP 进行时间同步：

```bash
sed -i 's/#NTP=/NTP=ntp.tuna.tsinghua.edu.cn/' /etc/systemd/timesyncd.conf
timedatectl set-ntp true
```

### 配置 LDAP 登录

TODO

### 安全增强

SSH 禁止密码登录（除 `pacman` 用户外）：

```bash
cat <<EOF > /etc/ssh/sshd_config.d/99-pacman.conf
PasswordAuthentication no
UsePAM yes
PermitRootLogin yes

Match User pacman root
    PasswordAuthentication yes

Match all
EOF
```

启用日志转发与收集：

```bash
apt install systemd-journal-remote
sed -i 's/# URL=/URL=172.23.0.1/' /etc/systemd/journal-upload.conf
systemctl enable --now systemd-journal-upload
```

TODO

### 安装附加软件

下列软件请根据具体需要进行安装。

#### Spack

TODO

#### ZFS

TODO

#### Docker

TODO

#### NVIDIA GPU 驱动

TODO

#### NVIDIA InfiniBand 驱动

TODO

#### SLURM

TODO

### 配置隔离环境

TODO

为这些用户启动额外 SSH Server：

```bash
cat <<EOF > /etc/ssh/sshd_config.d/98-naive.conf
Port 22222

Match LocalPort 22222
    PermitRootLogin no
    PasswordAuthentication no
    GatewayPorts no
    MaxAuthTries 1
    AllowGroups naive

Match all
EOF
```
