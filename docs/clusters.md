# 实验室集群

实验室提供了若干实验集群可供使用，详细列举如下。更详细的信息（如 IP 分配、物理位置、资产标签、序列号等）可在 NetBox 中查询。

## `gorgon`

* 服务器型号：Dell PowerEdge R730
* CPU：双路 Intel(R) Xeon(R) CPU E5-2670 v3 @ 2.30GHz (12C24T)
* 内存：128GB DDR3-2133
* 存储：2TB (`/home`) + 2TB (`/mnt/ssd`) 均为共享 NFS（来自 `gorgon0`）
* 网络：10Gbps (`gorgon0`) / 1Gbps （其他） Ethernet + 100Gbps Infiniband EDR (w/ OFED 4.3)
* GPU：NVIDIA Tesla V100 16GB (`gorgon[1-4]`) / 32GB (`gorgon[5-7]`)
* 数量：9 (`gorgon[0-9]`) + 4 (`gorgon[10-13]`)
* 管理员：黄可钊
* 用户管理：集群内共享
* 软件管理：Environment Modules (`/usr/local/Modules`) + Spack (`/opt/spack`)
* 系统：Ubuntu 16.04 (Xenial)
* 集群使用：SLURM（登录节点 `gorgon0`）

`Gorgon` 集群的前 9 台为实际业务集群，可通过 SLURM 直接提交作业使用；后 4 台为实验性集群，可通过独占方式向管理员申请使用权限。

目前 `gorgon` 的软件管理较为混乱，两套体系共存。用户可能需要提前测试可用性。

## `bic`

`bic` 的全称为 Brain-Inspired Computing。

* 服务器型号：Dell PowerEdge R730
* CPU：双路 Intel(R) Xeon(R) CPU E5-2680 v4 @ 2.40GHz (28C56T)
* 内存：256GB DDR4-2400
* 存储：15TB (`/home`) 共享 NFS（来自 `fermat`） + 24TB 本地 HDD + 6.4TB 本地 NVMe SSD (Intel DC P4618)
* 网络：1Gbps Ethernet + 100Gbps Infiniband EDR (w/ OFED 4.2)
* GPU：2 $\times$ NVIDIA GeFore 1080 (`bic0[1-4]`)
* 数量：9 (`bic0[1-9]`)
* 管理员：余博文
* 用户管理：LDAP（要求位于 `login` 组方可登录）
* 软件管理：Spack (`/spack-bic`)
* 系统：Ubuntu 18.04 (Bionic)
* 使用方式：SSH

目前 `bic` 整体专用于大数据相关实验，使用前必须与管理员协商确认。

## `nico`

`nico` 得名于 Nikola Tesla，取其姓代指此为 GPU 集群。

* 服务器型号：Supermicro SYS-4029GP-TRT
* CPU：双路 Intel(R) Xeon(R) Gold 5218 CPU @ 2.30GHz (32C64T)
* 内存：384GB DDR4-2666
* 存储：2.6TB (`/home`) 共享 NFS（来自 `nico4`） + 11TB (`/mnt/zoltan`) 共享 NFS（来自 `zoltan`）
* 网络：1Gbps Ethernet + 100Gbps Infiniband EDR (w/ OFED 5.3)
* GPU：
    * `nico[1-2]`: 8 $\times$ NVIDIA Tesla V100 32GB
    * `nico3`: 4 $\times$ NVIDIA Tesla V100 32GB + 4 $\times$ NVIDIA Tesla V100 16GB
    * `nico4`: NVIDIA Tesla P100 16GB + GeForce GTX 1080
* 数量：8 (`nico[1-4]`)
* 管理员：黄可钊
* 用户管理：LDAP
* 软件管理：Spack (`/opt/spack`)
* 系统：Debian 10 (Buster)
* 使用方式：SLURM（登录节点 `nico4`）

`nico` 是实验室主要的 GPU 集群，可用于大规模 GPU 实验。两台八卡机（`nico[1-2]`）位于 SLURM 的 `Big` 分区中，默认对用户不开放，需要向管理员进行申请。申请后，可使用 `srun -A priority -p Big` 的方式提交运行。

## 超算

超算集群为超算队成员专用，不对外开放。

* 服务器型号：Dell PowerEdge R720
* CPU：双路 Intel(R) Xeon(R) CPU E5-2699 v4 @ 2.20GHz
* 内存：384GB DDR3-1866
* 存储：1TB (`/home`) + 8TB (`/mnt/ssd`) 均为共享 NFS（来自 `e1`）
* 网络：1Gbps Ethernet + 100Gbps Infiniband EDR（w/ OFED 5.4，实际带宽约 50Gbps）
* GPU：2 $\times$ NVIDIA Tesla V100 32GB
* 数量：2 (`e[1-2]`)
* 管理员：翟明书
* 用户管理：集群内共享
* 软件管理：Spack (`/opt/spack`)
* 系统：Debian 11 (Bullseye)
* 使用方式：SSH

## AMD 服务器

实验室现有三台 AMD 服务器，其中 `yes` 和 `ja` 为 CPU 服务器，`octave` 为 GPU 服务器。

### `yes`

`yes` 取自 `AMD Yes!` 的口号。

* 服务器型号：Dell PowerEdge R7515
* CPU：单路 AMD EPYC 7742 64-Core Processor (64C128T)
* 内存：256GB DDR4-3200
* 存储：1TB (`/home`) 共享 NFS（来自 `ja`）
* 网络：1Gbps Ethernet
* 管理员：陈晟祺
* 用户管理：LDAP
* 软件管理：Spack (`/home/spack`)
* 系统：Debian 11 (Bullseye)
* 使用方式：SSH

### ja

`ja` 即为德语的 `yes`。

* 服务器型号：Supermicro AS-2023US-TR4
* CPU：双路 AMD EPYC 7742 64-Core Processor (128C256T)
* 内存：512GB DDR4-3200
* 存储：1TB (`/home`)
* 网络：1Gbps Ethernet
* 管理员：陈晟祺
* 用户管理：LDAP
* 软件管理：Spack (`/home/spack`)
* 系统：Debian 11 (Bullseye)
* 使用方式：SSH

### `octave`

`oct` 即为拉丁文中代表 8 的词缀，指此服务器可以安装 8 块 A100 GPU。

* 服务器型号：Supermicro AS-4124GS-TNR
* CPU：双路 AMD EPYC 7742 64-Core Processor (128C256T)
* 内存：512GB DDR4-2933
* 存储：2.2TB (`/home`) 本地存储
* 网络：1Gbps Ethernet
* GPU：5 $\times$ NVIDIA A00-PCIE-40GB
* 管理员：陈晟祺
* 用户管理：LDAP
* 软件管理：Spack (`/home/spack`)
* 系统：Debian 11 (Bullseye)
* 使用方式：SSH

### 新集群

新的 AMD 集群正在进一步采购中，预计将由四台与 `ja` 配置相同的服务器构成，并配置 Infiniband HDR 200Gbps 网卡。

## NVLink GPU 服务器

有两台服务器提供 NVLink 连接的 GPU：

### `zoltan`

* 服务器型号：Supermicro 型号不明
* CPU：双路 Intel(R) Xeon(R) Gold 6240 CPU @ 2.60GHz (36C72T)
* 内存：512GB DDR4-2933
* 存储：1TB (`/home`) 本地存储 + 11TB (`/mnt/data`) 共享存储（共享给 `nico` 集群）
* 网络：20Gbps Ethernet + 100Gbps Infiniband EDR (w/ OFED 5.4)
* GPU：4 $\times$ NVIDIA Tesla V100-SXM2 32GB
* 管理员：陈晟祺
* 用户管理：LDAP
* 软件管理：暂无
* 系统：Debian 11 (Bullseye)
* 使用方式：SSH

### `hanzo`

* 服务器型号：Dell PowerEdge C4130
* CPU：双路 Intel(R) Xeon(R) CPU E5-2620 v4 @ 2.10GHz (16C32T)
* 内存：256GB DDR4-2400
* 存储：15TB (`/home`) 共享 NFS（来自 `fermat`）
* 网络：1Gbps Ethernet + 100Gbps Infiniband EDR（未连接）
* GPU：4 $\times$ NVIDIA Tesla V100-SXM2 16GB
* 管理员：陈晟祺
* 用户管理：LDAP
* 软件管理：暂无
* 系统：Ubuntu 20.04 (Focal)
* 使用方式：SSH

## 其他集群/服务器

### `nvdimm`

此集群上装备有 NVDIMM 非易失内存（非 Intel Optane）。

* 服务器型号：Dell PowerEdge R740
* CPU：？
* 内存：？
* 存储：？
* 网络：1Gbps Ethernet
* 数量：2 (`nvdimm[1-2]`)
* 管理员：冯冠宇、郑立言
* 用户管理：本地
* 软件管理：暂无
* 系统：？
* 使用方式：SSH

### `lotus`

此集群原本为 `bic` 的一部分，故配置相同。

* 服务器型号：Dell PowerEdge R730
* CPU：双路 Intel(R) Xeon(R) CPU E5-2680 v4 @ 2.40GHz (28C56T)
* 内存：256GB DDR4-2400
* 存储：24TB 本地 HDD
* 网络：1Gbps Ethernet + 100Gbps Infiniband EDR（未连接）
* GPU：NVIDIA Tesla P100 16GB + NVIDIA Tesla V100 32GB
* 管理员：王豪杰、马子轩
* 用户管理：本地
* 软件管理：暂无
* 系统：Ubuntu 16.04 (Xenial)
* 使用方式：SSH

### `conv`

此集群原本也为 `bic` 的一部分，故配置相同。`conv` 取内卷之意。

* 服务器型号：Dell PowerEdge R730
* CPU：双路 Intel(R) Xeon(R) CPU E5-2680 v4 @ 2.40GHz (28C56T)
* 内存：256GB DDR4-2400
* 存储：14TB (`/home`) 共享 NFS（来自 `conv0`） + 24TB 本地 HDD
* 网络：1Gbps Ethernet + 100Gbps Infiniband EDR (w/o OFED)
* GPU：NVIDIA GeForce GTX 1080 (`conv0`) / NVIDIA Tesla P100 16GB (`conv[1-4]`)
* 数量：5 (`conv[0-4]`)
* 管理员：陈晟祺
* 用户管理：集群内 LDAP
* 软件管理：Spack (`/opt/spack/spack`)
* 系统：Debian 10 (Buster)
* 使用方式：SLURM（登录节点 `conv0`）

此集群目前为《高性能计算导论》课程专用（网络与 PACMAN 内网完全隔离），同时也提供给超算队使用。

### `diablo`

* 服务器型号：Dell PowerEdge R730
* CPU：双路 Intel(R) Xeon(R) Gold 6154 CPU @ 3.00GHz (36C72T)
* 内存：384GB DDR4-2666
* 存储：450GB (`rpool`) + 256GB (`tank`) 本地存储（ZFS）
* 网络：20Gbps Ethernet
* 管理员：陈晟祺、王邈
* 用户管理：本地，仅授权可访问
* 系统：Debian 11 (Bullseye)
* 使用方式：SSH

此为实验室网关，承载了所有的网络流量出入和大部分重要服务。

### `fermat`

* 服务器型号：Dell PowerEdge R730xd
* CPU：双路 Intel(R) Xeon(R) CPU E5-2620 v4 @ 2.10GHz (8C16T)
* 内存：128GB DDR4-2400
* 存储：15TB (`/vols/main`) 提供共享 NFS
* 网络：1Gbps Ethernet
* 管理员：？
* 用户管理：LDAP
* 系统：Ubuntu 16.04 (Xenial)
* 使用方式：SSH

`fermat` 为 `bic` 集群和 `hanzo` 等服务器提供了共享的 `/home` 存储，并为实验室提供 LDAP 服务。

### `poseidon`

`poseidon` 为已经损坏的共享存储，品牌为赛凡（CYPHY）。
