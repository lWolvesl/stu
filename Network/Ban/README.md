# Ban

- 网络限速



## TC 命令

```shell
apt install iproute2
```

- 限制上行

```shell
sudo tc qdisc add dev eth0 root tbf rate 10mbit burst 10kb latency 50ms
```

- 限制下行

```shell
sudo tc qdisc add dev eth0 handle ffff: ingress
sudo tc filter add dev eth0 parent ffff: protocol ip prio 50 u32 match ip src 0.0.0.0/0 police rate 100mbit burst 10mbit drop flowid :1
```

- 清空设置

```shell
# 清除 egress (上行) 限速规则
sudo tc qdisc del dev eth0 root

# 清除 ingress (下行) 限速规则
sudo tc qdisc del dev eth0 ingress
```

- 注意，tc是命令，启用后存储在内存中，若需要持久性需要以下步骤

  - 1、创建一个新的配置文件，例如 /etc/tc-limits.sh，并使用文本编辑器打开该文件。

    ```shell
    sudo touch /etc/tc-limits.sh
    sudo nano /etc/tc-limits.sh
    ```

  - 2、在配置文件中添加 tc 设置命令，例如：

    ```shell
    #!/bin/bash
    
    # 设置下行带宽限制为 100 Mbps
    sudo tc qdisc add dev eth0 handle ffff: ingress
    sudo tc filter add dev eth0 parent ffff: protocol ip prio 50 \
        u32 match ip src 0.0.0.0/0 police rate 100mbit burst 10mbit drop flowid :1
    
    # 设置上行带宽限制为 10 Mbps
    sudo tc qdisc add dev eth0 root tbf rate 10mbit burst 10kb latency 50ms
    
    ```

    请注意，上述命令中的 eth0 是要设置限速的网络接口名称，可以根据实际情况替换为相应的接口名称。

  - 保存并关闭文件，然后赋予文件可执行权限(chmod +x)

  - 将配置文件添加到系统的启动脚本中，以便在系统启动时自动加载。

    ```shell
    sudo ln -s /etc/tc-limits.sh /etc/rc.d/rc.local
    ```

  以上步骤将会在每次系统启动时自动执行 /etc/tc-limits.sh 文件中的 tc 设置命令，从而在系统重启后依然保持 tc 设置的有效性。
