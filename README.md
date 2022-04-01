# 快速开始

## 必备条件

* Ubuntu Linux (version 18.04 - Bionic)
* x86_64 64 位系统
* 拥有`sudo` 权限的帐号
* 内存不小于 8G
* 硬盘剩余空间不小于 150G

### 安装基础工具

```shell
sudo apt-get install git-core gitk git-gui curl lvm2 thin-provisioning-tools \
     python-pkg-resources python-virtualenv python-oauth2client xz-utils \
     python3.6
```

执行 `python3 --version` 确定 python3 版本，如果当前 python3 的默认版本是 3.5，那么需要切换到 3.6。

```shell
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.5 1
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 2
sudo update-alternatives --config python3
```

## 获取代码

### 安装 depot_tools

获取 `depot_tools`

```shell
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
cd depot_tools
git checkout 650f853ced2d5f740eb6e4a80edbf0d09351d10e
```

然后把 `depot_tools` 目录加入到 `PATH` 变量中，并且 `depot_tools` 的路径要放在前部。

```shell
export PATH=/path/to/depot_tools:$PATH
```

建议把上述语句加到 `~/.bashrc` 或 `~/.zshrc` 中。


### 同步代码

首先创建目录，后续多数命令都在此目录 `$HOME/r96` 中执行：

```shell
mkdir $HOME/r96
cd $HOME/r96
```

配置此条环境变量以禁止工具自行从 Google 服务器尝试升级，由于网络问题可能会导致卡死及失败：

```shell
export DEPOT_TOOLS_UPDATE=0
```

执行 `repo init`；

```shell
repo init -u https://chromium.googlesource.com/chromiumos/manifest -b release-R96-14268.B
```

下一步，引入 openfyde 的各个项目代码：

```shell
mkdir openfyde

git clone https://github.com/openFyde/manifest.git openfyde/manifest -b r96_v14.1_dev

ln -snfr openfyde/manifest .repo/local_manifests
```

`openfyde/manifest` 中包含了 openfyde 的项目信息，通过 `.repo/local_manifests` 链接引入进来。

之后进行代码同步操作，可以视机器配置和服务端状态，调整`-j` 或  `--jobs-network` 参数。具体的参数请执行 `repo sync --help` 或 `repo help sync` 查看。

以下 `sync` 命令会 同步大量代码，耗时较久，请耐心等待：

```shell
repo sync
```

如果中途看到 `Connection to * closed by remote host.` 不必中断命令，`repo` 会自动重试。

如果最终 `repo sync` 输出失败信息之后退出，可以稍后再次执行 `repo sync`，之前已下载到本地的内容不会重复下载。

`repo sync` 成功后，会看到 `repo sync has finished successfully.` 的信息。


## 创建 chroot 编译环境

执行 `cros_sdk` 命令，通过 `--url` 指定 openFyde 提供的 sdk 包的链接。创建并进入 chroot 内部。

```shell
cros_sdk --nouse-image
```

## 编译 amd64-openfyde/rpi4-openfyde

跟 Google 官方 [Developer Guide](https://chromium.googlesource.com/chromiumos/docs/+/HEAD/developer_guide.md#Select-a-board) 步骤一致。

下面的命令中 `(inside)` 表示这条命令在 chroot 环境内部执行。进入 chroot 环境后，默认所在的目录是 `$HOME/trunk/src/scripts`。


```shell
(inside) export BOARD=amd64-openfyde
(inside) setup_board --board=${BOARD}
(inside) ./build_packages --board=${BOARD} --nowithautotest --autosetgov
(inside) ./build_image --board=${BOARD} --noenable_rootfs_verification test
```

