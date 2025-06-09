# 实验室服务

实验室运行了一系列的网络服务，为各位同学提供便利。

## 运行中

### NetBox 设备管理

* 地址：<https://pacman.cs.tsinghua.edu.cn/netbox/>
* 服务器：`diablo`
* 认证：LDAP（`sudo` 组用户可编辑，`ops` 组可管理）

[NetBox](https://netbox.readthedocs.io/en/stable/) 是开源的设备管理系统，用于维护机房的各类设备详情（包括机器位置、编号）、网络配置（IP 分配、连接关系）、设备库存（如 GPU、网卡）等。

在对机房设备进行更改后，必须通知管理员修改 NetBox 中的相应信息，以保持记录和实际情况的一致性。

### Grafana 监控系统

* 地址：<https://pacman.cs.tsinghua.edu.cn/grafana/>
* 服务器：`diablo`
* 认证：LDAP（仅有查看权限）

为了监控实验室各类设备和服务，实验室搭建了一套 Grafana 系统，目前提供以下监控：

* 机房用电：Power Dashboard
* `nico`, `gorgon` 和 `conv` 的 SLURM 使用：SLURM Dashboard
* `nico`, `gorgon`, `bic` 和 `conv` 的 IB 使用：Infiniband Dashboard
* 交换机网络流量：Switch Dashboard
* 打印机耗材：Printers Overview
* 某些服务器（需使用 Telegraf 显式接入）的系统整体情况：Telegraf: system dashboard

## 个人主页

* 地址：<https://pacman.cs.tsinghua.edu.cn/~username/>（需要替换 `username` 为自己 LDAP 用户名）
* 服务器：`fermat`、`web`
* 认证：LDAP / SSH（仅可编辑自己的主页）

实验室为每个 LDAP 用户提供了个人主页服务。只需要将内容（仅支持静态页面）复制到 `fermat` 的 `~/public_html/` 目录下，即可被同步至网页服务器发布（同步间隔约为 10 分钟）。需要注意必须有 `index.html` 才能被浏览器显示，并且不支持列目录功能。

## PACMAN Guide

* 地址：<https://pacman.cs.tsinghua.edu.cn/guide/>
* 服务器：`web`

为了使新进入实验室的同学能够更好地使用各类资源，特编写了这本手册。

## 实验性

!!! warning "实验性服务"

    此类服务不对运行情况、数据安全作任何保证。重要数据请及时备份，涉密数据请勿使用。

### PanLeaf （实验性）

* 地址：<http://panleaf.pacman-thu.org>（仅限内网访问）
* 服务器：`fermat`
* 认证：LDAP，原来由 xl 维护的 ShareLatex 的用户可使用原有邮箱和密码登录
* 维护者：laekov

PanLeaf 是一个扩展版的 OverLeaf，除多人同时在线编辑 LaTeX 项目外，还支持了 pandoc markdown 语法，欢迎试用。（详见项目主页：<https://github.com/laekov/panleaf>）

## 已下线

!!! note "已下线服务"

    以下服务已不再运行，相关数据可能已经被删除或者只读归档。

### 会议室预订系统

* 地址：<https://pacman.cs.tsinghua.edu.cn/room/>
* 服务器：`diablo`
* 认证：LDAP（仅可编辑自己的活动）

为了更好地管理 9-320 会议室的使用，防止冲突，使用开源软件 [MRBS](https://mrbs.sourceforge.io/) 搭建了一套会议室预订系统。
任何合法的 LDAP 用户都可以创建日程，包括单次和重复日程。

为了方便不具有 LDAP 账号的用户，此系统具有一个公共账号，用户名为 `room`，密码与 PACMAN 的 WiFi 口令相同。
