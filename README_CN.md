# 快速开始 (WIP)

## 必备条件

* Ubuntu Linux (version 18.04 - Bionic)
* x86_64 64 位系统
* 拥有`sudo` 权限的帐号
* 内存不小于 8G
* 硬盘剩余空间不小于 150G

### 安装 Python 和其他基础工具

目前需要的 Python 版本为 3.6 或更新版本。执行 `python -V` 确定 Python 是所需版本，如果当前
Python 的版本低于 3.6，那么需要卸载旧版，安装 3.6 或更新版本的 Python，或者使用
[pyenv](https://github.com/pyenv/pyenv)。

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

### gerrit 帐号权限

### 帐号权限

* 首先在 [https://account.fydeos.com](https://account.fydeos.com) 注册
  FydeOS 帐号，用于登录 [https://gerrit.openfyde.cn](https://gerrit.openfyde.cn)。
* 在 [https://gerrit.openfyde.cn/settings/#Profile](https://gerrit.openfyde.cn/settings/#Profile) 页面查看 Username，下文中以
  `<gerrit_user>` 代替。
* 在 [https://gerrit.openfyde.cn/settings/#HTTPCredentials](https://gerrit.openfyde.cn/settings/#HTTPCredentials) 生成并复制密码
* 在 `$HOME/git-credentials` 加入以下内容，其中 `<gerrit_user>` 和 `<gerrit_password>` 用上一步获取到的 Username 和密码替换。

```txt
https://<gerrit_user>:<gerrit_password>@gerrit.openfyde.cn
```

执行 `git ls-remote https://gerrit.openfyde.cn/chromium.googlesource.com/chromiumos/manifest.git` 验证是否已经正确配置 gerrit.openfyde.cn 鉴权信息。

### 安装 depot_tools

获取 `depot_tools`

```shell
git clone https://gerrit.openfyde.cn/chromium.googlesource.com/chromium/tools/depot_tools.git
cd depot_tools
git checkout e121d14b12412e95ac833cfd31602b674499ea25
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
repo init -u https://gerrit.openfyde.cn/chromium.googlesource.com/chromiumos/manifest \
          -b release-R96-14268.B \
          --repo-url=https://gerrit.openfyde.cn/chromium.googlesource.com/external/repo
```

下一步，引入 openfyde 的各个项目代码：

```shell
mkdir openfyde
git clone https://gitee.com/openFyde/manifest.git openfyde/manifest -b r96_v14.1_dev_gitee
ln -snfr openfyde/manifest .repo/local_manifests
```

`openfyde/manifest` 中包含了 openfyde 的项目信息，通过 `.repo/local_manifests` 链接引入进来。

之后进行代码同步操作，可以视机器配置和服务端状态，调整`-j` 或  `--jobs-network` 参数。具体的参数请执行 `repo sync --help` 或 `repo help sync` 查看。

以下 `sync` 命令会从 gerrit.openfyde.cn 同步大量代码，耗时较久，请耐心等待：

```shell
repo sync
```

如果中途看到 `Connection to * closed by remote host.` 不必中断命令，`repo` 会自动重试。

如果最终 `repo sync` 输出失败信息之后退出，可以稍后再次执行 `repo sync`，之前已下载到本地的内容不会重复下载。

`repo sync` 成功后，会看到 `repo sync has finished successfully.` 的信息。

#### 同步 Chromium 依赖

在 `repo sync` 之后，openfyde/chromium/src 应该已经存在 chromium 源码。为了顺利编译 chromium，需要把 chromium 所需依赖同步到本地。

确认在 chromium 目录存在 `.gclient` 链接到上一步生成的 `dotgclient` 文件。

```shell
$ cd $HOME/r96/openfyde/chromium
$ readlink .gclient
../dotgclient/dotgclient
```

多数的依赖内容都已经镜像至 gerrit.openfyde.cn，可以直接通过 `gclient` 工具获取。由于
`openfyde/chromium/src/DEPS` 文件还有一部分内容由 [https: //chrome-infra-packages.appspot.com/](https://chrome-infra-packages.appspot.com/)提供，
另外还会在同步代码后、执行 `DEPS` 中指定的 hooks 命令过程中从 google storage
下载内容，网络不通的情况下无法访问到，暂时需要通过其他途径补全这部分内容，
openFyde 提供了预先打包的文件供下载。

```shell
wget https://packages.cdn.openfyde.cn/chromium/r96/cipd_deps_96.0.4664.202.tar.gz
tar xzf cipd_deps_96.0.4664.202.tar.gz
gclient sync --nohooks -vvv
```

此处看到 `__init__:cipd ensure -log-level ......` 时，命令可能会卡住，此时可以按下
ctrl+c 中止，所需内容已经由 `cipd_deps_96.0.4664.202.tar.gz` 提供。

接下来需要完成代码同步之后的 hooks。

```shell
wget https://packages.cdn.openfyde.cn/chromium/r96/hooks_bin_96.0.4664.202.tar.gz
tar xzf hooks_bin_96.0.4664.202.tar.gz
gclient runhooks -vvv
```

最后可能会看到 generate_location_tags 失败的信息，可以忽略，所需文件已经由
`hooks_bin_96.0.4664.202.tar.gz` 提供。

## 创建 chroot 编译环境

由于 `cros_sdk` 命令在创建和进入 chroot 编译环境过程中，会更新 chroot
内部的包，这个过程会由于网络问题无法编译某些包，导致命令失败，所以需要预先把编译过程中需要的文件准备好。openFyde
提供了 chroot 编译环境完整的 `.cache` 目录打包文件。

```shell
cd $HOME/r96
wget https://packages.cdn.openfyde.cn/distfiles/r96/r96_distfiles_cache.tar.gz
tar xzf r96_distfiles_cache.tar.gz
```

此时 `$HOME/r96/.cache` 目录中已经有了后续编译需要下载的各个源码包文件。

然后执行 `cros_sdk` 命令，通过 `--url` 指定 openFyde 提供的 sdk 包的链接。
创建并进入 chroot 内部。

```shell
cros_sdk --nouse-image --url=https://gs.cdn.openfyde.cn/chromiumos-sdk/cros-sdk-2021.10.05.002450.tar.xz
```

TODO: 验证编译过程中需要从 gerrit.openfyde.cn 获取代码的包。

由于在后面的编译过程中，还有可能从 gerrit.openfyde.cn 获取代码，所以此时还需要把 `$HOME/.ssh` 的文件复制进 chroot 环境内部。

新建一个命令行窗口，在 chroot 外部执行命令，复制 ssh 公私钥到 chroot 内部。

```shell
cd $HOME/r96
cp $HOME/.ssh/id_rsa* chroot/home/$(whoami)/.ssh
```

然后回到之前已经处在 chroot 内部的窗口。

## 编译 amd64-openfyde/rpi4-openfyde

跟 Google 官方 [Developer Guide](https://chromium.googlesource.com/chromiumos/docs/+/release-R96-14268.B/developer_guide.md#Select-a-board) 步骤一致。

下面的命令中 `(inside)` 表示这条命令在 chroot 环境内部执行。进入 chroot 环境后，默认所在的目录是 `$HOME/trunk/src/scripts`。

```shell
(inside) export BOARD=amd64-openfyde
(inside) setup_board --board=${BOARD}
(inside) ./build_packages --board=${BOARD} --nowithautotest --autosetgov
(inside) ./build_image --board=${BOARD} --noenable_rootfs_verification test
```
