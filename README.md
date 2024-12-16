# 脚本介绍

此脚本用于监控VPS服务器上的网络流量，并根据设定的流量限制来决定，是否阻止除SSH以外的所有网络流量。

## 参数介绍

脚本接受6个参数，但是所有参数都是可选的，如果没有提供，脚本会使用默认值。

这6个参数分别是：

1. `LIMIT_GB`：流量限制，单位为GB。默认值1024。
2. `max_percentage` ：达到多少百分比进行限制，无需输入`%` 。默认值90。
3. `reset_day`：流量重置日，即每月的哪一天流量会被重置。默认值为1，即每月的第一天。
4. `CHECK_TYPE` ：流量检查类型，有4个选项，默认值4。
   - 1：只检查上传流量
   - 2：只检查下载流量
   - 3：检查上传和下载流量中的最大值
   - 4：检查上传和下载流量的总和
5. `ssh_port` ：SSH端口，默认值22。
6. `INTERFACE`：网络接口。默认自动检测，检测失败会提示。

## 举例

`bash traffic_monitor.sh 200 90 1 1 1022 eth0` :设置流量限制为200GB，使用90%进行限制，流量在每月的1号重置，只检查上传流量，SSH端口为1022，并指定网络接口为`eth0`。

## 运行命令

```
# 请将 traffic_monitor.sh 复制到/root目录，并切换到root用户执行下列命令

# 安装必须软件。iptables 是用于配置 Linux 内核防火墙的工具，bc 是一个任意精度的计算器语言，vnstat 是一个控制台下的网络流量监控工具
# Debian系安装命令
apt-get update && apt-get install -y iptables bc vnstat
# RedHat系安装命令
yum update && yum install -y iptables bc vnstat

# 添加可执行权限
chmod +x monitor_traffic.sh

# 试运行脚本
bash monitor_traffic.sh 200 90 1 1 1022 eth0

# 添加计划任务，每分钟运行一次脚本，并将脚本的输出重定向到 /root/脚本日志.txt 文件
(crontab -l ; echo "* * * * * /root/monitor_traffic.sh 190 3 > /root/脚本日志.txt")  | crontab -
```



> 此脚本是在“她说是晒黑的”基础上修改，[原文链接](https://www.nodeseek.com/post-127477-1)
>
> 修改点：额外添加两个参数，分别是“达到多少百分比进行限制”、“SSH端口”。
