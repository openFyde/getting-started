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

### 帐号权限

 - 首先在 [https://account.fydeos.com](https://account.fydeos.com) 注册 FydeOS 帐号，用于登录 [https://gerrit.openfyde.cn](https://gerrit.openfyde.cn)。

 - 在 [https://gerrit.openfyde.cn/settings/#Profile](https://gerrit.openfyde.cn/settings/#Profile) 页面查看 Username，下文中以 `<user>` 代替。

 - 在 [https://gerrit.openfyde.cn/settings/#SSHKeys](https://gerrit.openfyde.cn/settings/#SSHKeys) 添加自己的 ssh 公钥，点击 `SAVE CHANGES` 保存后，在命令行执行 `ssh user@gerrit.openfyde.cn` 并确认有登录权限，应该看到输出如下信息后自动退出。

```txt
  ****    Welcome to Gerrit Code Review    ****

  Hi user, you have successfully connected over SSH.

  Unfortunately, interactive shells are disabled.
  To clone a hosted Git repository, use:

  git clone ssh://user@gerrit.openfyde.cn/REPOSITORY_NAME.git
```


### git 配置

为了避免克隆代码时可能遇到的网络问题，需要手工修改当前用户的 git 配置，使用 openFyde 项目提供的地址，替换 Google 提供的地址。
把下面的内容添加在 `$HOME/.gitconfig` 中，注意使用自己在 gerrit.openfyde.cn 的 username 替换下面内容的 `<user>`。

```ini
[url "<user>@gerrit.openfyde.cn:chromium.googlesource.com"]
  insteadOf = https://chromium.googlesource.com

[url "<user>@gerrit.openfyde.cn:android.googlesource.com"]
  insteadOf = https://android.googlesource.com

[url "<user>@gerrit.openfyde.cn:aomedia.googlesource.com"]
  insteadOf = https://aomedia.googlesource.com

[url "<user>@gerrit.openfyde.cn:boringssl.googlesource.com"]
  insteadOf = https://boringssl.googlesource.com

[url "<user>@gerrit.openfyde.cn:dawn.googlesource.com"]
  insteadOf = https://dawn.googlesource.com

[url "<user>@gerrit.openfyde.cn:pdfium.googlesource.com"]
  insteadOf = https://pdfium.googlesource.com

[url "<user>@gerrit.openfyde.cn:quiche.googlesource.com"]
  insteadOf = https://quiche.googlesource.com

[url "<user>@gerrit.openfyde.cn:skia.googlesource.com"]
  insteadOf = https://skia.googlesource.com

[url "<user>@gerrit.openfyde.cn:swiftshader.googlesource.com"]
  insteadOf = https://swiftshader.googlesource.com

[url "<user>@gerrit.openfyde.cn:webrtc.googlesource.com"]
  insteadOf = https://webrtc.googlesource.com
```


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

首先创建目录，后续多数命令都在此目录 `$HOME/r92` 中执行：

```shell
mkdir $HOME/r92
cd $HOME/r92
```

配置此条环境变量以禁止工具自行从 Google 服务器尝试升级，由于网络问题可能会导致卡死及失败：

```shell
export DEPOT_TOOLS_UPDATE=0
```

执行 `repo init`；如果之前 git 配置正确，以下命令会从部署在中国大陆地区的 gerrit.openfyde.cn 服务器同步代码：

```shell
repo init -u https://chromium.googlesource.com/chromiumos/manifest -b release-R92-13982.B
```

下一步，引入 openfyde 的各个项目代码：

```shell
mkdir openfyde

git clone https://gitee.com/openFyde/manifest.git openfyde/manifest -b r92

ln -snfr openfyde/manifest .repo/local_manifests
```

`openfyde/manifest` 中包含了 openfyde 的项目信息，通过 `.repo/local_manifests` 链接引入进来。

之后进行代码同步操作，可以视机器配置和服务端状态，调整`-j` 或  `--jobs-network` 参数。具体的参数请执行 `repo sync --help` 或 `repo help sync` 查看。

以下 `sync` 命令会从 gerrit.openfyde.cn 同步大量代码，耗时较久，请耐心等待：

```shell
repo sync
```

如果中途看到 `Connection to gerrit.openfyde.cn closed by remote host.` 不必中断命令，`repo` 会自动重试。

如果最终 `repo sync` 输出失败信息之后退出，可以稍后再次执行 `repo sync`，之前已下载到本地的内容不会重复下载。

`repo sync` 成功后，会看到 `repo sync has finished successfully.` 的信息。下一步准备 `chromium` 的代码依赖。

### Chromium 依赖

在 `repo sync` 之后，`openfyde/chromium/src` 应该已经存在 `chromium` 源码，并且处于 `92.0.4515.183` tag。
为了顺利编译 `chromium` ，需要使用把 `openfyde/chromium/src/DEPS` 文件中指定的内容下载到本地。

多数的依赖内容都已经镜像至 gerrit.openfyde.cn，可以直接通过 `gclient` 工具获取。由于 `openfyde/chromium/src/DEPS` 文件还有一部分内容由 [https://chrome-infra-packages.appspot.com/](https://chrome-infra-packages.appspot.com/) 提供，另外还会在同步代码后、执行 `DEPS` 中指定的 hooks 命令过程中从 google storage 下载内容，网络不通的情况下无法访问到，暂时需要通过其他途径补全这部分内容，openFyde 提供了预先打包的文件供下载。

```shell
cd openfyde/chromium
wget https://packages.cdn.openfyde.cn/chromium/r92/cipd_deps_92.0.4515.183.tar.gz
tar xzf cipd_deps_92.0.4515.183.tar.gz
gclient sync --nohooks -vvv
```

此处看到 `__init__:cipd ensure -log-level ......` 时，命令可能会卡住，此时可以 ctrl+c 中止，
所需内容已经由 `cipd_deps_92.0.4515.183.tar.gz` 提供。

接下来需要完成代码同步之后的 hooks。

```shell
wget https://packages.cdn.openfyde.cn/chromium/r92/hooks_bin_92.0.4515.183-r1.tar.gz
tar xzf hooks_bin_92.0.4515.183-r1.tar.gz
gclient runhooks -vvv
```

最后可能会看到 generate_location_tags 失败的信息，可以忽略，所需文件已经由 `hooks_bin_92.0.4515.183-r1.tar.gz` 提供。


## 创建 chroot 编译环境

由于 `cros_sdk` 命令在创建和进入 chroot 编译环境过程中，会更新 chroot 内部的包，这个过程会由于网络问题无法编译某些包，导致命令失败，所以需要预先把编译过程中需要的文件准备好。openFyde 提供了 chroot 编译环境完整的 `.cache` 目录打包文件。

```shell
cd $HOME/r92
wget https://packages.cdn.openfyde.cn/distfiles/r92_distfiles_cache_r1.tar.gz

tar xzf r92_distfiles_cache_r1.tar.gz
```

此时 `$HOME/r92/.cache` 目录中已经有了后续编译需要下载的各个源码包文件。

然后执行 `cros_sdk` 命令，通过 `--url` 指定 openFyde 提供的 sdk 包的链接。创建并进入 chroot 内部。

```shell
cros_sdk --nouse-image --url=https://gs.cdn.openfyde.cn/chromiumos-sdk/cros-sdk-2021.05.18.170403.tar.xz
```

等待命令执行完成后，已进入 chroot 内部。`$HOME/.gitconfig` 的配置会被自动复制到 chroot 环境的 HOME 目录。

由于在后面的编译过程中，还有可能从 gerrit.openfyde.cn 获取代码，所以此时还需要把 `$HOME/.ssh` 的文件复制进 chroot 环境内部。 另外由于编译时 emerge 命令不会读取 `$HOME/.gitconfig` 中的配置，所以需要把上述 `$HOME/.gitconfig` 中 `insteadOf` 配置内容复制到 chroot 环境中的 `/etc/gitconfig` 文件，如果 `/etc/gitconfig` 不存在，新建即可。

新建一个命令行窗口，在 chroot 外部执行命令，复制 ssh 公私钥到 chroot 内部。

```shell
cd $HOME/r92
cp $HOME/.ssh/id_rsa* chroot/home/$(whoami)/.ssh
```

然后回到之前已经处在 chroot 内部的窗口。

## 编译 amd64-openfyde/rpi4-openfyde

跟 Google 官方 [Developer Guide](https://chromium.googlesource.com/chromiumos/docs/+/HEAD/developer_guide.md#Select-a-board) 步骤一致。

下面的命令中 `(inside)` 表示这条命令在 chroot 环境内部执行。进入 chroot 环境后，默认所在的目录是 `$HOME/trunk/src/scripts`。

首先执行 `ssh user@gerrit.openfyde.cn` 查看结果，确保在 chroot 内部可以从 gerrit.openfyde.cn 获取代码。


```shell
(inside) export GERRIT_USERNAME=<user> # 把 <user> 替换为 gerrit 用户名
(inside) export BOARD=amd64-openfyde
(inside) setup_board --board=${BOARD}
(inside) ./build_packages --board=${BOARD} --nowithautotest --autosetgov
(inside) ./build_image --board=${BOARD} --noenable_rootfs_verification test
```

