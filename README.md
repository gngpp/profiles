# Script profiles

- 卸载docker (只适合Debian系)
```shell
wget -qO- https://raw.githubusercontent.com/gngpp/profiles/master/uninstall_docker.sh | bash
```

- 安装Docker
```shell
curl -sSL https://get.daocloud.io/docker | sh
```

<details> <summary>lede-builder</summary>

- 构建lede OpenWrt docker环境镜像
 
```shell
docker run -it -v $PWD:/build ghcr.io/gngpp/lede-builder:latest
```

</details>

<details> <summary>openwrt-builder</summary>

- 构建官方 OpenWrt docker环境镜像
 
```shell
docker run -it -v $PWD:/build ghcr.io/gngpp/openwrt-builder:latest
```

</details>

<details> <summary>Proxmox</summary>

- proxmox ve开启网卡sriov

```shell
wget https://raw.githubusercontent.com/gngpp/profiles/master/proxmox/ethernet_sriov.sh && bash +x ethernet_sriov.sh 
```  

</details>

<details> <summary>Synology</summary>

- 群晖（7.1）交叉工具链编译ixgbevf,进入root用户
```shell

# 下载工具链
wget https://cndl.synology.cn/download/ToolChain/toolkit/7.1/base/base_env-7.1.txz
wget https://cndl.synology.cn/download/ToolChain/toolkit/7.1/apollolake/ds.apollolake-7.1.env.txz
wget https://cndl.synology.cn/download/ToolChain/toolkit/7.1/apollolake/ds.apollolake-7.1.dev.txz
wget https://cndl.synology.cn/download/ToolChain/toolchain/7.0-41890/Intel%20x86%20Linux%204.4.180%20%28Apollolake%29/apollolake-gcc750_glibc226_x86_64-GPL.txz

tar -xf base_env-7.1.txz && tar -xf apollolake/ds.apollolake-7.1.env.txz
tar -xf ds.apollolake-7.1.dev.txz && tar -xf apollolake-gcc750_glibc226_x86_64-GPL.txz

# 源码驱动
wget https://downloadmirror.intel.com/762486/ixgbevf-4.17.5.tar.gz
tar -xvzf ixgbevf-4.17.5.tar.gz

# 建立软链接
ln -s /volume2/homes/gngpp/build/usr/local/x86_64-pc-linux-gnu/x86_64-pc-linux-gnu/sys-root/usr/lib/modules/DSM-7.1/build /lib/modules/4.4.180+/build

# 编译驱动
pushd ixgbevf-4.17.5/src
../../bin/make ARCH=x86_64 CROSS_COMPILE=/volume2/homes/gngpp/build/x86_64-pc-linux-gnu/bin/x86_64-pc-linux-gnu-
popd

# 安装驱动
cp ixgbevf.ko /lib/modules/ixgbevf.ko
insmod /lib/modules/ixgbevf.ko

# 不出意外日志可以看到驱动了
dmesg

```

> 驱动放在/lib/modules下开机依旧不会自动加载，需加入启动计划任务脚本

```shell
insmod /lib/ixgbevf.ko
ip link set ethX up
```

</details>
