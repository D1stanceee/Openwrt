如何编译自己所需的 OpenWrt 固件
====

**以下命令可以作为搭建环境和编译的参考**

注意：
1. __不要__ 用 `root` 用户进行编译！！！
2. 国内用户编译前最好准备好梯子
3. 默认登陆IP 192.168.1.1, 用户名`root`，密码 `无`

编译命令如下:

1. 首先装好 Linux 系统，推荐较新版本的 Ubuntu LTS x64 ，  至少拥有 4GB RAM 和 30 GB 的可用磁盘空间并连上`国际互联网`

2. 然后搭建系统环境，命令行输入以下命令：

```bash
sudo apt update -y
sudo apt full-upgrade -y
```

```bash
sudo bash -c 'bash <(curl -s https://build-scripts.immortalwrt.eu.org/init_build_environment.sh)'
```

**如果你使用`root`用户执行了以上命令，那从此时开始，你必须使用`非root`权限用户进行后续操作**

3. 下载好源代码

3-1. OpenWrt 源码

```bash
git clone https://github.com/openwrt/openwrt.git openwrt
```

3-2. ImmortalWrt 源码

```bash
git clone https://github.com/immortalwrt/immortalwrt.git openwrt
```


然后进入`openwrt`目录（源码存在此目录，所以此目录即为`buildroot`目录）

```bash   
cd openwrt
```

4. 切换到特定分支

4-1.查看所有的分支

```bash
git branch -a
```

4-2.查看所有标签

```bash
git tag
```

4-3.使用OpenWrt xx分支

```bash
git checkout xx
```

5. 更新系统组件

```bash
./scripts/feeds update -a
./scripts/feeds install -a
```

6. 运行 `make menuconfig`入选单界面，选择CPU架构，型号，固件类型，所需插件及工具等，记得先`Save` 再退出

```bash   
make menuconfig
```

7. 下载源码文件到`buildroot`录下的`dl`目录

```bash
make download
```

8. 正式开始编译，建议编译前先运行`screen`命令守护进程，尤其是在`VPS`上编译时

```bash
make V=s
```

8-1. `-j $(nproc)` 后面是线程数，第一次编译推荐用单线程，即 `-j1`

```bash
make -j1 V=s
```

至此等待机器编译完成即可

-----

二次编译：

```bash
cd openwrt
git pull
./scripts/feeds update -a
./scripts/feeds install -a
make defconfig
make download
make V=s
```

如果需要重新配置：

```bash
rm -rf ./tmp && rm -rf .config
make menuconfig
make download
make V=s
```

-----

**编译完成后输出路径为：bin/targets**

-----
