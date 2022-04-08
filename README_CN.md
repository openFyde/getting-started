# 快速开始

## 必备条件

* Ubuntu Linux (version 18.04 - Bionic)
* x86_64 64 位系统
* 拥有`sudo` 权限的帐号
* 内存不小于 8G
* 硬盘剩余空间不小于 150G

### 安装 Python 和其他基础工具

目前需要的 Python 版本为 3.6 或更新版本。执行 `python -V` 确定 python 是所
需版本，如果当前 python 的版本低于 3.6，那么需要卸载旧版，安装 3.6 或更新
版本的 Python，或者使用 [pyenv](https://github.com/pyenv/pyenv)。

```shell
sudo apt-get uninstall <current python package lower than 3.6>
sudo apt-get install python3.9
```

然后安装其他基础工具

```shell
sudo add-apt-repository universe
sudo apt-get install git gitk git-gui curl xz-utils \
     python3-pkg-resources python3-virtualenv python3-oauth2client \
     lvm2 thin-provisioning-tools
```

## 获取代码

### 安装 depot_tools

获取 `depot_tools`

```shell
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
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

#### 同步 Chromium 依赖

在 `repo sync` 之后，openfyde/chromium/src 应该已经存在 chromium 源码。为
了顺利编译 chromium，需要把 chromium 所需依赖同步到本地。

```shell
cd openfyde/chromium
```

确认此目录下存在文件 `.gclient`（指向 `../dotgclient/dotgclient` 的符号链
接），否则请回到上一步检查 `repo sync` 是否成功。

然后执行 `gclient sync`，等待命令执行完成。

```shell
$ readlink .gclient
../dotgclient/dotgclient
$ gclient sync
```

## 创建 chroot 编译环境

执行 `cros_sdk` 命令，创建并进入 chroot 内部。

```shell
cros_sdk --nouse-image
```

## 编译 amd64-openfyde/rpi4-openfyde

跟 Google 官方 [Developer Guide](https://chromium.googlesource.com/chromiumos/docs/+/release-R96-14268.B/developer_guide.md#Select-a-board) 步骤一致。

下面的命令中 `(inside)` 表示这条命令在 chroot 环境内部执行。进入 chroot 环境后，默认所在的目录是 `$HOME/trunk/src/scripts`。

```shell
(inside) export BOARD=amd64-openfyde
(inside) setup_board --board=${BOARD}
(inside) ./build_packages --board=${BOARD} --nowithautotest --autosetgov
(inside) ./build_image --board=${BOARD} --noenable_rootfs_verification test
```
