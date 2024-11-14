# 快速参考手册（QRH）

本页记载了在发生特定情况时的参考操作步骤，供运维人员参考。

## 系统异常

### 无法启动

TODO

### 无法关机

TODO

## 网络异常

TODO

## SLURM 异常

## 任务异常

1. 确认对应结点正常：`sinfo -R <name>` 无输出，否则使用结点异常或离线 QRH。
2. 确认错误信息：
    * 如为 `couldn't chdir to ...`，则说明目标机器上无此目录，可能原因包括：
        * 如果报错目录是用户 home，检查 NFS 是否挂载；
        * 如果报错目录是 `/sys/fs/cgroups` 等，确认隔离环境是否正确配置；
        * 如果报错目录为其他非共享目录，确认用户意图；
    * 如为 `Invalid account or account/partition combination specified`，则说明用户指定了错误的账户或者分区；
        * 确认用户在正确的集群上提交任务；
        * 确认用户指定的账户和分区是否正确；
        * 确认用户的账户是否有相应分区的权限；

## 结点异常（drain 或者 down）

1. 确认结点状态：DRAIN 或者 DOWN，没有星号，否则转向结点掉线 QRH。
2. 查看具体原因：`scontrol show node <name>` 以及 `sinfo -R <name>`：
    * 如为 `Node unexpectedly rebooted`，则确认重启原因，并必须执行 [服务器重启流程](server-reboot.md)。
    * 如为 `Kill task failed`，则需要在对应结点确认是否有残留进程；并查看系统日志确认原因。如果必要，则告知用户避免某些操作。
    * 如为其他原因，请执行结点异常 QRH。
3. 使用 `scontrol update nodename=<name> state=resume` 恢复结点状态。

## 结点离线（`down*`）

1. 确认结点状态：`DOWN*`，有星号，否则转向结点异常 QRH。
2. 确认对应结点否能能访问：
    * 如果不能，则修理此问题；
3. 登录相应结点，检查 slurm 服务状态：
    * 运行 `systemctl status slurmd`，确认服务是否正常运行：
    * 如果服务没有运行，运行 `journalctl -u slurmd`，以及查看日志 `/var/log/slurmd.log` 确认具体错误。
        * 如果报告 `/dev/nvidia*` 未能找到，则运行 `modprobe nvidia` 以及 `systemctl restart slurmd`；
        * 如果报告 `/sys/fs/cgroups/freezer` 等未找到，很有可能是用户隔离环境没有正确配置（如某个用户启用了隔离，但是没有挂载相应文件夹，需要重新运行对应的 prepare 脚本）。
4. 确保 slurm 服务在运行，检查结点是否能够访问控制结点：
   * 运行 `ping <controller>`，确认网络是否正常；
   * 运行 `scontrol ping`，确认 slurm 是否正常。
