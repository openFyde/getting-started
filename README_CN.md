![openFyde Splash](https://fydeos.com/content/wp-content/uploads/2022/04/openfyde-splash-image.jpg, "openFyde Splash")

本文内容并**不是** [README.md](README.md) 的中文翻译版本，而是针对中国大陆地区网络环境，对流程进行了修改，与 [README.md](README.md) 有所不同。

<br>


## TL;DR: (以FAQ的形式)

<details>
  <summary>openFyde 是什么?</summary>
  <br>

  openFyde（总是表示为 `openFyde`，第一个字母 `o` 为小写，`F` 为大写）是燧炻创新（FydeOS 的创建者）发起的一项开源计划，目的是为 [Chromium OS](https://www.chromium.org/chromium-os) 提供另一种选择。通过 openFyde，你可以拥有一个更加开放和灵活的 Chromium OS。在燧炻创新，我们相信有更多的选择就有更大的可能。

  **简单来说：openFyde 是更开放、更自由的 Chromium OS ，并且把是否要使用 Google / FydeOS 网络服务的选项交给用户。**
</details>


<details>
  <summary>Chromium OS, Chrome OS, openFyde 和 FydeOS 之间有什么区别？</summary>
  <br>

  - Chromium OS 是一个开源项目，主要由开发者使用，其代码可供任何人查看、修改和构建。
  - Google Chrome OS 是 OEM 厂商在 Chromebook 上出货的 Google 产品，供一般消费者使用。
  - openFyde 类似于 Chromium OS，但它为开发者和用户提供了更多的选择和灵活性。openFyde 由燧炻创新公司发起，由 openFyde 作者维护。
  - FydeOS 类似于 Google Chrome OS，但由燧炻创新编写和维护。

  一些具体的区别：

  - 这些操作系统从根本上共享相同的代码库，但 Google Chrome OS 具有一些额外的固件功能，包括安全启动和轻松恢复，这需要相应的硬件更改，因此即使是 Chromium OS 构建出的也不能开箱即用。
  - Google Chrome OS / FydeOS for You 在经过特别优化的硬件上运行，以获得增强的性能和安全性。
  - Chromium OS / openFyde 默认不自动更新，而 Google Chrome OS / FydeOS 会无缝自动更新，以便用户拥有最新最好的功能和修复。
  - Google Chrome OS / FydeOS 包含一些专有的或带有商业许可的软件包，这些包不包含在 Chromium OS 项目中。
  - 由于上述原因，Google Chrome OS / FydeOS 支持 Android 子系统，而 Chromium OS / openFyde 不支持。
  - 同样由于上述原因，openFyde 和 FydeOS 共享大部分代码库，但专有的或带有商业许可的软件包不包含在 openFyde 中。但是，从长远来看，我们的目标是用开源的替代方案或我们自己的实现来替换那些闭源的软件包。
  - 新的和实验性的功能将在 openFyde 下开发，一旦稳定下来就会移植到 FydeOS。
  - Google Chrome OS 有一个绿色 / 黄色 / 红色的 logo；Chromium OS 有一个蓝色 / 更蓝 / 最蓝的 logo。 而 FydeOS 和 openFyde 的 logo 上没有图形，只有文字，但是有一个小小的设计彩蛋。


</details>

<details>
  <summary>到哪里去寻求帮助？</summary>
  <br>

  你不能在当前的这个仓库中提交 issue，因为它只起到介绍和指导的作用。我们欢迎你在其他 openFyde 仓库中提交 issue，如果:
   - 你已经阅读了整个 [Chromium OS Developer Guide](https://chromium.googlesource.com/chromiumos/docs/+/main/main/developer_guide.md) 以及下面的 README，在构建 openFyde 后依然遇到了问题。
   - 你认为 openFyde 的某些功能实现 / 代码片段 / ebuilds / 配置方案 / 脚本不能正常工作。
   - 你在 openFyde 中发现了安全相关的 bug 或漏洞。
   - 你认为 openFyde 公布的部分代码完全是垃圾，而且你有强有力的证据来支持你的结论。

  你的 issue 可能会被关闭，如果：
   - 你询问的是关于 Chromium OS / Chrome OS 的通用功能或错误：为此，请使用 [chromium-os-dev Google Group](https://groups.google.com/a/chromium.org/g/chromium-os-dev) 或向 [crbugs](https://bugs.chromium.org/) 提交 bug。
   - 你问的是关于第三方应用程序、非标准外设或特殊设置，此类对社区没有帮助的问题。
</details>



<br><br>


## 快速开始

### 必备条件

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


<br>

## 获取代码

### gerrit 及帐号权限

* 首先在 [https://account.fydeos.com](https://account.fydeos.com) 注册
  FydeOS 帐号，用于登录 [https://gerrit.openfyde.cn](https://gerrit.openfyde.cn)。
* 在 [https://gerrit.openfyde.cn/settings/#Profile](https://gerrit.openfyde.cn/settings/#Profile) 页面查看 Username，下文中以
  `<gerrit_user>` 代替。
* 在 [https://gerrit.openfyde.cn/settings/#HTTPCredentials](https://gerrit.openfyde.cn/settings/#HTTPCredentials) 生成并复制密码
* 在 `$HOME/.git-credentials` 加入以下内容，其中 `<gerrit_user>` 和 `<gerrit_password>` 用上一步获取到的 Username 和密码替换。

```txt
https://<gerrit_user>:<gerrit_password>@gerrit.openfyde.cn
```

* 然后配置使用密码验证 gerrit 权限

```shell
git config --global credential.helper store
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

首先创建目录，后续多数命令都在此目录 `$HOME/r102` 中执行：

```shell
mkdir $HOME/r102
cd $HOME/r102
```

配置此条环境变量以禁止工具自行从 Google 服务器尝试升级，由于网络问题可能会导致卡死及失败：

```shell
export DEPOT_TOOLS_UPDATE=0
```

配置 git 用户名和邮箱:

```shell
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```

执行 `repo init`；

```shell
repo init -u https://gerrit.openfyde.cn/chromium.googlesource.com/chromiumos/manifest \
          -b release-R102-14695.B \
          --repo-url=https://gerrit.openfyde.cn/chromium.googlesource.com/external/repo
```

下一步，引入 openfyde 的各个项目代码：

```shell
git clone https://gitee.com/openFyde/manifest.git openfyde/manifest -b r102-dev-gitee
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


### 同步 Chromium 依赖

在 `repo sync` 之后，openfyde/chromium/src 应该已经存在 chromium 源码。为了顺利编译 chromium，需要把 chromium 所需依赖同步到本地。

确认在 chromium 目录存在 `.gclient` 链接到上一步生成的 `dotgclient` 文件。

```shell
$ cd $HOME/r102/openfyde/chromium
$ readlink .gclient
../dotgclient/dotgclient
```

多数的依赖内容都已经镜像至 gerrit.openfyde.cn，可以直接通过 `gclient` 工具获取。由于
`openfyde/chromium/src/DEPS` 文件还有一部分内容由 [https: //chrome-infra-packages.appspot.com/](https://chrome-infra-packages.appspot.com/)提供，
另外还会在同步代码后、执行 `DEPS` 中指定的 hooks 命令过程中从 google storage
下载内容，网络不通的情况下无法访问到，暂时需要通过其他途径补全这部分内容，
openFyde 提供了预先打包的文件供下载。

```shell
wget https://packages.cdn.openfyde.cn/chromium/r102/cipd_deps_102.0.5005.90.tar.gz
tar xzf cipd_deps_102.0.5005.90.tar.gz
gclient sync --nohooks -vvv
```

此处看到 `__init__:cipd ensure -log-level ......` 时，命令可能会卡住，此时可以按下
ctrl+c 中止，所需内容已经由 `cipd_deps_102.0.5005.90.tar.gz` 提供。

接下来需要完成代码同步之后的 hooks。

```shell
wget https://packages.cdn.openfyde.cn/chromium/r102/hooks_bin_102.0.5005.90.tar.gz
tar xzf hooks_bin_102.0.5005.90.tar.gz
gclient runhooks -vvv
```

最后可能会看到 generate_location_tags 失败的信息，可以忽略，所需文件已经由 `hooks_bin_102.0.5005.90.tar.gz` 提供。


<br>

### 申请 Google 和 FydeOS 的 API Key

如果你想用你的 Google 账户登录 Chromium OS GUI，你需要申请 Google API Key，并在你构建的镜像中添加它们。

根据[这篇文档](http://www.chromium.org/developers/how-tos/api-keys)在 Google 的网站上申请 Google API。在获得 Client ID, Client Secret 和 API key 后，把它们放在 `~/.googleapikeys` 文件中，格式如下：

```
'google_api_key': 'your api key',
'google_default_client_id': 'your client id',
'google_default_client_secret': 'your client secret',
```

同样，如果你想使用由 https://account.fydeos.com 提供的 FydeOS 的在线账户和同步功能，你需要申请一个 openFyde 开发者 API Key，并将其放在同一个 `~/.googleapikeys` 文件中。目前是通过将你的 FydeOS 账户（如果你还没有，可以注册一个新的账户）详细信息发送邮件到 [dev-support@openfyde.io](mailto:dev-support@openfyde.io) 来申请，开发者支持团队会尽快处理你的申请。

一旦你有了 openFyde 开发者 API Key，你需要把它们加到 `~/.googleapikeys` 文件中，格式如下：

```
'fydeos_default_client_id' : 'your openFyde Developer API client id',
'fydeos_default_client_secret' : 'your openFyde Developer API client secret',
```

然后 Chromium OS 的构建脚本会自动从这个文件中读取必要的信息，你构建出的镜像将能够使用 Google 账户和 FydeOS 账户登录。


<br>

## 创建 chroot 编译环境

由于 `cros_sdk` 命令在创建和进入 chroot 编译环境过程中，会更新 chroot
内部的包，这个过程会由于网络问题无法编译某些包，导致命令失败，所以需要预先把编译过程中需要的文件准备好。openFyde
提供了 chroot 编译环境完整的 `.cache` 目录打包文件。

```shell
cd $HOME/r102
wget https://packages.cdn.openfyde.cn/distfiles/r102/r102_distfiles_cache.tar.gz
tar xzf r102_distfiles_cache.tar.gz
```

此时 `$HOME/r102/.cache` 目录中已经有了后续编译需要下载的各个源码包文件。

然后执行 `cros_sdk` 命令，通过 `--url` 指定 openFyde 提供的 sdk 包的链接。
创建并进入 chroot 内部。

```shell
cros_sdk --nouse-image --url=https://gs.cdn.openfyde.cn/chromiumos-sdk/cros-sdk-2022.04.11.135343.tar.xz
```

<br>

## 编译 amd64-openfyde/rpi4-openfyde

跟 Google 官方 [Developer Guide](https://chromium.googlesource.com/chromiumos/docs/+/release-R102-14695.B/developer_guide.md#Select-a-board) 步骤一致。

下面的命令中 `(inside)` 表示这条命令在 chroot 环境内部执行。进入 chroot 环境后，默认所在的目录是 `$HOME/trunk/src/scripts`。

```shell
(inside) export BOARD=amd64-openfyde # 对于 rpi4-openfyde 需要 export BOARD=rpi4-openfyde
(inside) setup_board --board=${BOARD}
```

之后需要手动为 chroot 环境编译输入法相关的包 `dev-libs/capnproto`。

```bash
(inside)
sudo emerge capnproto
```

接下来继续编译其他 packages 和 image。

```shell
(inside) ./build_packages --board=${BOARD} --nowithautotest --autosetgov --nouse_any_chrome
(inside) ./build_image --board=${BOARD} --noenable_rootfs_verification test
```

最后将 image 烧录到 U 盘:
```shell
(inside) cros flash usb:// ${BOARD}/latest
```
