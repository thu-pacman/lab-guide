# 服务器初始化

## 命名与地址分配

服务器的命名遵循如下原则：

- 必须是合法的 NETBIOS、mDNS 和 DNS 名称
- 总长度不建议超过 8 个字符
- 如为集群，则最后若干位应该为数字编号（`[0-9]`），管理结点通常使用 `0`，其他结点从 `1` 开始

（新）服务器的 IP 地址分配遵循以下原则：

- 原则上使用 DHCP 绑定到网口 MAC，不要手工配置静态地址；如有特殊需求，应向管理员申请；
- 必须在 NetBox 上查询相应地址是否被占用；
- 每个集群可以使用一个单独的 `172.23.x.0/24` 子网（其中 $ 10 \le x \le 99$），建议分配如下：
    - 管理 / 登录结点：`172.23.x.0/16`，IPMI 使用 `172.23.x.100/16`
    - 工作结点：`172.23.x.[1-99]/16`，IPMI 使用 `172.23.x.[101-199]/16`
- 如果为独立服务器，可以在 `172.23.2.0/24` 开始的子网中分配地址：
    - 服务 IP：`172.23.2.y/16`，其中 $ 2 \le y \le 99$
    - IPMI IP：`172.23.2.y+100/16`
- 如果有 IB 网络，则 IPoIB 网络使用 `10.0.x.y/16` 的地址，其中 `x.y` 与以太网地址中一致

上述分配的主机名、网卡 MAC 地址、IP 地址等信息必须在 NetBox 系统中记录，并在 Zentyal 中进行 DHCP 绑定。

## BMC 配置

- 用户设置
    - 通常使用 root 作为用户名，密码不能与系统 root 密码一致
- 网络连接
    - 有条件使用独立网口，不使用 failover 配置
    - 原则上除关键服务器（如网关外）使用 DHCP 获取 IP，并根据实际情况配置主机名为 `$hostname-idrac` / `$hostname-bmc`，DNS 域名为 `lab.pacman-thu.org`
    - 如果无法自动获取 NTP 服务器，可设置为 172.23.0.1 或者 101.6.6.172 (ntp.tuna.tsinghua.edu.cn)

## BIOS 配置

- CPU 选项
    - 根据需要，确定启用或禁用超线程
    - 通常启用虚拟化（Intel VT-x 或者 AMD SVM）
    - 根据需要，启用或禁用 IOMMU
    - 对于 AMD CPU，注意 NPS (NUMA Per Socket) 的设置
- PCIe 选项
    - 启用 SR-IOV
    - 启用 Above 4G Decoding
    - 启用 Re-Size BAR
- 电源选项
    - 根据需要，调整电源策略（通常使用 Balanced，不建议选择 Power save 或者 Performance）
    - 根据需要，启用或者禁用 Turbo boost
    - 如果有 RAPL (Running Average Power Limit) 选项，启用
- 引导选项
    - 关闭 CSP，或者将所有相关项设置为 UEFI Only
    - 允许网络引导选项，可能包括 Network Boot、PXE Boot、HTTP Boot 等；如果要求选择网络适配器，通常只启用第一个
    - 引导顺序调整：PXE Boot > UEFI CD/DVD （通常是 BMC 模拟）> UEFI Hard Disk
