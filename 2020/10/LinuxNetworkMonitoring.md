# 在 Linux 下监控带宽使用情况
2020/10/08

## 背景
* 估算某跨平台程序（<del>Minecraft: Java Edition</del>）的 **流量** 消耗情况
* 我在使用某 Linux 发行版
* 因此我需要能够在 Linux 下实时测量某程序带宽占用的情况的程序

## 查询资料
https://askubuntu.com/questions/2411/how-do-i-find-out-which-process-is-eating-up-my-bandwidth

## 比较

这些软件都依赖于 `libpcap` ，因此“特殊权限”等同于“使用 libpcap 嗅探某网络接口的所有数据包的权限”。
| 软件 | 在 Debian 源内 | 区分 PID | 区分连接 | 统计总流量 | 实时速率 | 需要特殊权限 |
| --- | ---- | ---- | ---- | ---- | ---- | ----- |
| nethogs | Y | Y | Y | Y（按 M 键切换） | Y | Y |
| iftop   | Y | N | Y | Y | Y | Y |
| ntopng  | Y | N | Y | Y | Y | Y |

这些软件/方式 **不** 依赖 `libpcap`。
* `lsof -ni | grep -v 'localhost'`，可以查看连接（一切皆文件）
* `netstat` 和 `ss`，可指定的参数多，Debian 默认不安装前者。
* `iptraf-ng`，甚至有 CLI 和 WebUI！
* `iptables -n -v`，甚至能够看到有多少包/字节被防火墙拒绝 <del>（真的有人会用这种方法吗……）</del>
