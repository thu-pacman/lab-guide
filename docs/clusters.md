# 实验室集群

实验室提供了若干实验集群可供使用，详细列举如下。更详细的信息（如 IP 分配、物理位置、资产标签、序列号等）可在 NetBox 中查询。

## `jumper`

此为 `diablo` 上用于提供实验室内网访问的跳板机，不应用用于任何其他用途，尤其是存放较大文件。 **用户在此的文件随时可能不加通知地被清理。**

端口转发：`166.111.68.163:2222` -> `jumper:22`。用户可以使用 LDAP 账号或者 `jumper` 上的本地帐号登录。

## `bic`

`bic` 的全称为 Brain-Inspired Computing。

* 服务器型号：Dell PowerEdge R730
* CPU：双路 Intel(R) Xeon(R) CPU E5-2680 v4 @ 2.40GHz (28C56T)
* 内存：256GB DDR4-2400
* 存储：15TB (`/home`) 共享 NFS（来自 `fermat`） + 24TB 本地 HDD + 6.4TB 本地 NVMe SSD (Intel DC P4618)
* 网络：1Gbps Ethernet + 100Gbps Infiniband EDR (w/ OFED 4.2)
* GPU：2 $\times$ NVIDIA GeFore 1080 (`bic0[1-4]`)
* 数量：9 (`bic0[1-9]`)
* 管理员：蔡严正
* 用户管理：LDAP（要求位于 `login` 组方可登录）
* 软件管理：Spack (`/spack-bic`)
* 系统：Ubuntu 18.04 (Bionic)
* 使用方式：SSH

目前 `bic` 整体专用于大数据相关实验，使用前必须与管理员协商确认。

## `nico`

`nico` 得名于 Nikola Tesla，取其姓代指此为 GPU 集群。

* 工作节点
    * 服务器型号：Supermicro SYS-4029GP-TRT
    * CPU：双路 Intel(R) Xeon(R) Gold 5218 CPU @ 2.30GHz (32C64T)
    * 数量：4 (`nico[1-4]`)
    * GPU：
        * `nico[1-2]`: 8 $\times$ NVIDIA Tesla V100 32GB
        * `nico3`: 4 $\times$ NVIDIA Tesla V100 32GB + 4 $\times$ NVIDIA Tesla V100 16GB
        * `nico4`: 3 $\times$ NVIDIA Tesla V100 32GB + 5 $\times$ NVIDIA Tesla V100 16GB
    * 网络：1Gbps Ethernet + 100Gbps Infiniband EDR (w/ OFED 23.04)
    * 内存：384GB DDR4-2666
* NVLink GPU 节点：
    * 服务器型号：Inspur 型号不明
    * CPU：双路 Intel(R) Xeon(R) Gold 6240 CPU @ 2.60GHz (36C72T)
    * 内存：512GB DDR4-2933
    * 网络：20Gbps Ethernet + 100Gbps Infiniband EDR (w/ OFED 23.04)
    * GPU：8 $\times$ NVIDIA Tesla V100-SXM2 32GB
* 管理节点
    * 服务器型号：Supermicro SYS-2029U-TR4T
    * CPU：双路 Intel(R) Xeon(R) Gold 6252 CPU @ 2.10GHz (48C96T)
    * 网络：20Gbps Ethernet + 100Gbps Infiniband EDR (w/ OFED 23.04)
    * 内存：384GB DDR4-2933
* 存储：13.5TB (`/home`) 共享 NFS（来自 `nico0`） + 11TB (`/mnt/zoltan`) 共享 NFS（来自 `zoltan`）
* 客服：黄可钊
* 用户管理：LDAP
* 软件管理：Spack (`/opt/spack`)
* 系统：Debian 12 (Bookworm)
* 使用方式：SLURM（登录节点 `nico0`）

`nico` 是实验室用于分布式计算实验的 GPU 集群，可进行至多 32 GPU 规模的实验。进行大规模实验需使用 `Big` 分区，进行长时间任务（如训练大型深度神经网络）需使用 `Long` 分区，这两个分区默认对用户不开放，需要时向管理员进行申请。普通队列中的机器数目会根据使用情况由管理员进行动态调整。__禁止私自从 nico 集群中拆走 GPU，否则会受到天谴。__

## `ja`

请注意不要和下面的 `ja` 服务器混淆。其中 `ja` 是德语的 `yes`，均取自于 `AMD Yes!` 的口号。

* 服务器型号：
    * `yes`: Dell PowerEdge R7515 (2U)
    * `ja[1-4]`: Supermicro AS-2024US-TRT (2U)
    * `octave, twills`: Supermicro AS-4124GS-TNR (4U)
* CPU：
    * `yes`: 1 $\times$ AMD EPYC 7742 64-Core Processor (64C128T)
    * `ja[1-4], octave`: 2 $\times$ AMD EPYC 7742 64-Core Processor (128C256T)
    * `twills`: 2 $\times$ AMD EPYC 7453 28-Core Processor (56C112T)
* GPU:
    * `ja[1-4]`: 1 $\times$ NVIDIA Tesla V100 16GB
    * `octave`: 8 $\times$ NVIDIA A100 40GB PCIe
    * `twills`: 2 $\times$ NVIDIA A10 24GB PCIe，另有其他请自行查看
* 内存：256GB DDR4-3200 (`yes, twills`) / 512GB DDR4-3200 (`ja[1-4]`) / 512GB DDDR4-2933 (`octave`)
* 存储：
    * 共享：8TB (`/home`) 共享 NFS（来自 `yes` 的 ZFS RAIDZ1）
    * `octave`: 16TB 本地私有 ZFS 存储（`/data`）
* 网络：
    * `yes`： 20Gbps Ethernet
    * `ja[1-4]`：1Gbps Ethernet + 200Gbps Infiniband HDR (w/ OFED 23.04)
    * `twills`: 1Gbps Ethernet + 2 $\times$ 100Gbps Infiniband EDR (w/ OFED 23.04)
* 数量：6 (`ja[1-4]`, `octave`, `twills`)
* 管理员：何家傲
* 用户管理：LDAP
* 软件管理：Spack (`/opt/spack`)
* 系统：Debian 12 (Bookworm)
* 使用方式：SLURM（登录节点 `yes`）

## 超算

超算集群为超算队成员专用，不对外开放。

* 服务器型号：Inspur NF5280M6
* CPU：双路 Intel(R) Xeon(R) Platinum 8358 CPU
* 内存：512GB DDR4-3200
* 存储：180GB (`rpool`) + 4TB (`tank`) 均为共享 NFS（来自 `e1`）
* 网络：1Gbps Ethernet + 200Gbps Infiniband HDR（w/ OFED 23.04）
* GPU：3 $\times$ NVIDIA A100 80GB PCIe (1 on `e1`, 2 on `e3`)
* 数量：4 (`e[1-4]`)
* 管理员：张闰清
* 用户管理：集群内共享
* 软件管理：Spack (`/opt/spack`)
* 系统：Debian 12 (Bookworm)
* 使用方式：SSH

## 实验性服务器及建设中的集群

### `ja`

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

### `impreza0`

`impreza` 意为 intel's impressive advance

* 服务器型号: Dell R750
* CPU: 双路 Intel Xeon Gold 6338 (64C128T)
* 内存: 512GB DDR4-3200
* 网络：1Gbps Ethernet
* GPU：2 $\times$ AMD Instinct MI100
* 存储：120GB (`/`) + 480GB (`/home`)
* 管理员：于纪平
* 用户管理：LDAP
* 系统：Debian 12 (Bookworm)
* 使用方式：SSH


### `octave2`

* 服务器型号: Supermicro AS-4124GS-TNR (4U)
* CPU: 2 $\times$ AMD EPYC 7763 64-Core Processor (128C256T)
* 内存: 512GB DDR4-3200
* 网络：1Gbps Ethernet + 2 $\times$ 100Gbps Infiniband EDR (w/ OFED 23.04)
* GPU：AMD Instinct MI250
* 存储：512GB (`/`) + Intel P4618 6.4TB NVMe SSD 若干
* 管理员：陈晟祺
* 用户管理：LDAP
* 系统：Debian 12 (Bookworm)
* 使用方式：SSH

该服务器有若干实验性硬件（如 AMD GPU），使用前请与管理员确认。

### `da`

`da` 是俄语的 `yes`，同样取自于 `AMD Yes!` 的口号。此外，也指“大数据”的“大”字。

* 服务器型号：Dell PowerEdge R7525
* CPU：2 $\times$ AMD EPYC 7763 64-Core Processor (128C256T)
* 内存：384GB DDR4-3200
* 存储：每机 2 $\times$ 960GB SSD (RAID1) + 2 $\times$ 3.84TB NVMe U.2 SSD
* 网络：每机 2 $\times$ 25Gbps Ethernet (LACP) + 2 $\times$ 200Gbps Infiniband HDR (MCX755106AS-HEAT 双口网卡)
* GPU：每机 2 $\times$ NVIDIA Tesla T4 16GB PCIe
* 数量：4 (`da[1-4]`)
* 管理员：建设中
* 用户管理：建设中
* 软件管理：建设中
* 系统：Debian 12 (Bookworm)
* 使用方式：SSH

## NVLink GPU 服务器

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
* 系统：Ubuntu 22.04 (Jammy)
* 使用方式：SSH

## 其他集群/服务器

### `nvdimm`

<del>此集群上装备有 NVDIMM 非易失内存（非 Intel Optane）。目前已损坏。</del>

此集群已被拆散挪作他用。

* 服务器型号：Dell PowerEdge R740
* CPU：Intel(R) Xeon(R) Gold 6126 CPU @ 2.60GHz (24C48T)
* 内存：576GB DDR4-2666
* 存储：多个 NVDIMM 非易失内存，容量从 700G 至 1.8T
* 网络：1Gbps Ethernet
* 数量：2 (`nvdimm[1-2]`)
* 管理员：暂无
* 用户管理：本地
* 软件管理：暂无
* 系统：Debian 12 (Bookworm)
* 使用方式：SSH

### `lotus`

此集群原本为 `bic` 的一部分，故配置相同。

* 服务器型号：Dell PowerEdge R730
* CPU：双路 Intel(R) Xeon(R) CPU E5-2680 v4 @ 2.40GHz (28C56T)
* 内存：256GB DDR4-2400
* 存储：24TB 本地 HDD
* 网络：1Gbps Ethernet + 100Gbps Infiniband EDR（未连接）
* GPU：NVIDIA Tesla A100 40GB + NVIDIA Tesla V100 32GB
* 管理员：郑立言
* 用户管理：本地
* 软件管理：Spack (`/home/spack/spack/share/spack/`)
* 系统：Ubuntu 16.04 (Xenial)
* 使用方式：SSH

<!-- ### `xavier`

此机器原本为 `nova`。

* 服务器型号：SuperMicro 不明 (4U)
* CPU：单路 AMD EPYC 7282 16-Core Processor
* 内存：112GB DDR4-2400
* 网络：1Gbps Ethernet
* 加速卡：Xilinx FPGA
* 管理员：师天麾
* 用户管理：LDAP
* 软件管理：无
* 系统：Debian 12 (Bookworm)
* 使用方式：SSH, 使用前请先联系管理员 -->

### `conv`

此集群原本也为 `bic` 的一部分，故配置相同。`conv` 取内卷之意。

* 服务器型号：Dell PowerEdge R730
* CPU：双路 Intel(R) Xeon(R) CPU E5-2680 v4 @ 2.40GHz (28C56T)
* 内存：256GB DDR4-2400
* 存储：14TB (`/home`) 共享 NFS（来自 `conv0`） + 24TB 本地 HDD
* 网络：1Gbps Ethernet + 100Gbps Infiniband EDR (w/o OFED)
* GPU：NVIDIA GeForce GTX 1080 (`conv0`) / NVIDIA Tesla P100 16GB (`conv[1-4]`)
* 数量：5 (`conv[0-4]`)
* 管理员：翟明书
* 用户管理：集群内 LDAP
* 软件管理：Spack (`/opt/spack/spack`)
* 系统：Debian 12 (Bookworm)
* 使用方式：SLURM（登录节点 `conv0`）

此集群目前为《高性能计算导论》课程专用（网络与 PACMAN 内网完全隔离），不提供对外服务。

### `diablo`

* 服务器型号：Dell PowerEdge R740
* CPU：双路 Intel(R) Xeon(R) Gold 6154 CPU @ 3.00GHz (36C72T)
* 内存：384GB DDR4-2666
* 存储：450GB (`rpool`) + 256GB (`tank`) 本地存储（ZFS）
* 网络：20Gbps Ethernet
* 管理员：陈晟祺、王邈
* 用户管理：本地，仅授权可访问
* 系统：Debian 12 (Bookworm)
* 使用方式：SSH

此为东主楼网关，承载了所有的网络流量出入和大部分重要服务。

### `diablo`

* 服务器型号：Dell PowerEdge R740
* CPU：Intel(R) Xeon(R) Gold 6126 CPU @ 2.60GHz (24C48T)
* 内存：192GB DDR4-2666
* 网络：2 $\times$ 10Gbps Ethernet (LACP) + 2 $\times$ 100Gbps Infiniband EDR (N/C)
* 管理员：陈晟祺、王邈
* 用户管理：本地，仅授权可访问
* 系统：Debian 12 (Bookworm)
* 使用方式：SSH

此为自强科技楼机房网关，承载了所有的网络流量出入。

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

### `gorgon`

__该集群已下线停用，仅保留 gorgon0 以提供旧数据的访问。__

* 服务器型号：Dell PowerEdge R730
* CPU：双路 Intel(R) Xeon(R) CPU E5-2670 v3 @ 2.30GHz (12C, HT disabled)
* 内存：128GB DDR3-2133
* 存储：2TB (`/home`) + 2TB (`/mnt/ssd`) 均为共享 NFS（来自 `gorgon0`）
* 网络：10Gbps (`gorgon0`) / 1Gbps （其他） Ethernet + 100Gbps Infiniband EDR (w/ OFED 4.3)
* 端口转发：`166.111.68.163:3330` -> `gorgon0:22`
* GPU：NVIDIA Tesla V100 16GB (`gorgon[1]`) / A100 80GB (`gorgon[3-6]`)
* 数量：9 (`gorgon[0-9]`) + 4 (`gorgon[10-13]`)
* 管理员：黄可钊
* 用户管理：集群内共享
* 软件管理：Environment Modules (`/usr/local/Modules`) + Spack (`/opt/spack`)
* 系统：Ubuntu 16.04 (Xenial)
* 集群使用：SLURM（登录节点 `gorgon0`）

`gorgon` 集群的前 9 台为实际业务集群（`gorgon[2,7]` 已损坏），可通过 SLURM 直接提交作业使用；后 4 台为实验性集群，可通过独占方式向管理员申请使用权限。

目前 `gorgon` 的软件管理较为混乱，两套体系共存。用户可能需要提前测试可用性。
