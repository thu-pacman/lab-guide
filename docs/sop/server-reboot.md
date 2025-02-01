# 服务器重启流程

## 重启前

* 在 slurm 上 drain 掉该机器, 并检查 (或等待直到) 该机器上没有正在执行的任务
* (可选) 在对应运维通知渠道发送停机通知

## 重启后
### nfs 挂载

#### yes 集群

* 在 worker 节点上: `mount yes:/home /home`
* 在除 octave 外的所有节点上: `/mnt/octave/mount_octave.sh`

#### nico 集群

* 在 worker 节点上: `mount nico0:/home /home`
* 在 zoltan 以外的节点上: `mount zoltan:/mnt/data /mnt/zoltan`

#### 其它集群

TBD

### naive 环境初始化

* 在 yes / nico 集群的所有被重启的节点上 `/home/naive/prepare_all.sh`

### slurm 初始化

* 如有 gres 修改, 更新 slurm 控制节点的 gres.conf 和 slurm.conf
  * 在控制节点上 `systemctl restart slurmctld`
  * 在所有工作节点上 `systemctl restart slurmd`
* 如遇工作节点状态为 `IDLE*` 等非正常状态
  * 可能是 `/dev/nvidia*` 未被创建导致的, 在机器上执行 `nvidia-smi && systemctl restart slurmd` 即可

