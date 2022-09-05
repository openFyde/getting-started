![openFyde Splash](https://fydeos.io/content/wp-content/uploads/2022/04/openfyde-splash-image.jpg, "openFyde Splash")

<br>

[<img src="https://img.shields.io/badge/Join%20Telegram%20Group-FydeOS-yellowgreen.svg?style=popout-square&logo=telegram&colorA=870611&colorB=333333">](https://t.me/hi_fydeos)

<br>

## TL;DR: (in FAQ format)

<details>
  <summary>What's this, is this openFyde?</summary>
  <br>
  
  openFyde (always denotes as `openFyde`, with a first `o` in lowercase and `F` in capital letter) is an open-source initiative started by Fyde Innovations, the creator of FydeOS, with an aim to provide another approach to [Chromium OS](https://www.chromium.org/chromium-os). With openFyde you can have a more open and flexible Chromium OS. At Fyde Innovations we believe with more choices comes greater possibilities.
  
  **In a one-liner: openFyde is Chromium OS open-sourced instead of open-core, with or without Google.**
</details>


<details>
  <summary>What's the difference between Chromium OS, Chrome OS, openFyde and FydeOS?</summary>
  <br>
  
  - Chromium OS is an open-source project, used primarily by developers, with code that is available for anyone to checkout, modify, and build.
  - Google Chrome OS is the Google product that OEMs ship on Chromebooks for general consumer use.
  - openFyde OS is similar to Chromium OS, but it offers more choices and flexibilities for developers and users. openFyde is started by Fyde Innovations and maintained by the openFyde Authors.
  - FydeOS is similar to Google Chrome OS but produced and maintained by Fyde Innovations.
 
  Some specific differences:

  - These OS projects fundamentally share the same code base, but Google Chrome OS has some additional firmware features, including verified boot and easy recovery, which require corresponding hardware changes and thus also don't work out of the box in Chromium OS builds.
  - Google Chrome OS / "FydeOS for You" runs on specially optimised hardware in order to get enhanced performance and security.
  - Chromium OS / openFyde does not auto-update by default (so that changes you may have made to the code are not blown away), whereas Google Chrome OS / FydeOS seamlessly auto-updates so that users have the latest and greatest features and fixes.
  - Google Chrome OS / FydeOS includes some proprietary/commercial/licensed packages which are not included in the Chromium OS project.
  - In consequence of the above, Google Chrome OS / FydeOS supports Android subsystem, Chromium OS does not.
  - Also in consequence of the above, openFyde and FydeOS share most of the codebases but proprietary/commercial/licensed packages are not included in openFyde. However, we aim to replace those close-sourced packages with open alternatives or our own implementations in the long run.
  - New and experimental features will be developed under openFyde and port to FydeOS once stabilised.
  - Google Chrome OS has a green/yellow/red logo; Chromium OS has a blue/bluer/bluest logo. FydeOS and openFyde no longer have graphical logos, only textual ones but with a tiny signature twist.

</details>

<details>
  <summary>Where to get help?</summary>
  <br>
  
  You won't be able to open issues in this current repository, as it only serves introductory and guidance purposes. You are welcome to open issues for each of the other openFyde repositories if:
   - You've read the entire [Chromium OS Developer Guide](https://chromium.googlesource.com/chromiumos/docs/+/main/developer_guide.md) as well as the following README. After which you are attempting to build openFyde then have encountered problems
   - You believe certain implementations / constructs / ebuilds / config schemes / misc scripts of openFyde don't work as they should
   - You have discovered a security-related bug or vulnerability within openFyde
   - You believe parts of openFyde's published code are utter rubbish and you have strong evidence to back your conclusion

  Your issues will likely get closed if:
   - You are asking for generic features/bugs about Chromium OS / Chrome OS: for this please use [chromium-os-dev Google Group](https://groups.google.com/a/chromium.org/g/chromium-os-dev) or report bugs to [crbugs](https://bugs.chromium.org/)
   - You are asking about issues about a 3rd-party app, a non-standard peripheral device or a special setup that does not benefit the general community
</details>



<br><br>

## Table of contents (for cool kids)

<!-- TOC -->

- [Introduction](#introduction)
- [System requirement](#system-requirement)
- [Prepare the system](#prepare-the-system)
- [Get source code](#get-source-code)
- [Setup openFyde](#setup-openfyde)
- [Build openFyde for amd64](#build-openfyde-for-amd64)
- [Boot from the image](#boot-from-the-image)
- [More information](#more-information)
- [Last but not least](#last-but-not-least)
  <!-- /TOC -->

<br>

## Introduction

The following sections of this document describe how to build [openFyde](https://openfyde.io) and run it on x86 machines, from its source code and the board overlay provided by openFyde. Once you are familiar with how things work, you will be able to create your own board overlay and port openFyde to your hardware devices.


### Goal and non-goal of this project

* To provide an open-source codebase that everybody can use to build and improve openFyde on various platforms.
* To provide a path that some users/developers can port hardware drivers that openFyde/FydeOS does not currently have.
* To provide a platform for developers/groups/companies that are interested to use openFyde as the foundation of the OS of its hardware.
* This project does not aim to provide support for Chromium OS itself. If you find bugs and glitches, please report to [crbugs](https://bugs.chromium.org/p/chromium/issues/list); if you have further queries regarding Chromium OS, please revert to one of the official Chromium related [Google groups](https://www.chromium.org/developers/technical-discussion-groups).


### About the openFyde project

openFyde refers to a collection of projects and repositories working together to serve a unified goal.

Due to the sheer complexity of any Linux based operating system, openFyde is organised as many individual repositories with every single one serving a distinctive purpose. You will also need the entirety of Chromium OS source code to continue working on openFyde, as openFyde and Chromium OS basically share the same codebase. There are also quite a large number of dependencies all over the place. 

In consequence of the above, openFyde in GitHub is managed as an "organisation" with necessary repositories inside. This current repository does not contain any actual source code and only serves for introductory and guidance purposes.

As a downstream fork of Chromium OS, openFyde does not intend to be the rivalry of Chromium OS. We are open to contributing works back to the upstream and making Chromium OS a better operating system.

The code and document in openFyde are the result of works by the people of the openFyde team in Fyde Innovations. 



### Branches and tags in repositories within openFyde

#### branches

 - `main` or `master` - this branch can be used to build an openFyde image. It has been tested against our current release version. You are welcome to test it with future releases and send feedback and/or PRs.
 - `$release-dev` e.g. 'r102-dev' - this branch would be the develop branch for $release e.g. r102. If you are going to create PRs to
release 102, better to rebase it to r102-dev.

#### tags

 - When we do release a prebuilt image, the commit would be tagged with a release number corresponding to the repo manifest. For example, if the repo manifest release is `release-R102-14695.B`, then our release tag would be `r102`.
 - Often we will be doing more than one release for each repo manifest release number, so we will append a meaningful string to the tag name to identify such. For example: `r102-borealis_enabled`


### Naming conventions

It's worth mentioning some of the naming conventions that we have been adhering to since the creation of openFyde to clear out confusion and maintain consistency:

 - We use the dash `-` in strings to connect words/substrings with different realms or representations. Some examples:
     - `<prefix>-<version>-<suffix>`: `openFyde-14.1-prebuild`
     - `<release number>-<branch type>`: `r102-dev`
 - The underscore `_` is usually used in strings to connect words with spaces within the same realm or representation. Some examples:
     - `chromium_os-release-raspberry_pi`
     - `r102-borealis_enabled`
 - To name a bunch of items (files, classes, variables, branches, tags etc.), use a common prefix so they can be indexed and searched easily



### Typography conventions

Shell Commands are shown with different labels to indicate whether they apply to 

 - your build computer (the computer on which you're doing development)
 - the chroot (Chromium OS SDK) on your build computer
 - your Chromium OS computer (the device on which you run the images you build)


| Label     | Commands                                   |
| --------- | ------------------------------------------ |
| (outside) | on your build computer, outside the chroot |
| (inside)  | inside the chroot on your build computer   |


<br>

## System requirement

- A x86_64 system to perform the build. 64-bit hardware and OS are musts. The openFyde (and Chromium OS) is a very large project, building from the source from scratch usually takes hours to over 10 hours, depending on the system configuration.
   - CPU: we recommend using a 4-core or higher processor. The openFyde build process runs in parallel so more cores can help shorten build time dramatically.

   - Memory: we recommend at least 16GB, plus enough swap space because for the purpose of this project you will need to build Chromium from source code. Linking Chromium required between 8GB and 28GB of RAM as of March 2017, so you will run into massive swapping or OOM if you have less memory. However, if you are not building your own copy of Chromium, the RAM requirements will be substantially lower at a cost of losing some of the key features provided by this project.

   - Disk: at least 150GB of free space, 200GB or more is highly recommended. SSD could noticeably shorten the build time as there are many gigabytes of files that need to be written to and read from the disk.

   - Network: total source code downloading will be over 30GB. Fast and stable Internet access is going to be very helpful.

- A x86_64 Linux OS as your main workstation, it will be referred to as the *host OS* later in this doc. The openFyde build process utilises chroot to isolate the build environment from the host OS. So theoretically any modern Linux system should work. However, only limited Linux distros are tested by the Chromium OS team and the FydeOS team. Linux versions that are known to work:

   - Ubuntu Linux 18.04 LTS
   - Gentoo Linux
   - Arch Linux

- A non-root user account with sudo access. The build process should be run by this user, not the root user. The user needs to have _sudo_ access. For simplicity and convenience password-less sudo could be set for this user.


<br>

## Prepare the system


### Install necessary tools

Git and curl as the essential tools that need to be installed in the host OS, you will also need Python3 for most of the scripting work in the build process.

```bash
(outside)
sudo apt-get install git-core gitk git-gui curl lvm2 thin-provisioning-tools \
     python-pkg-resources python-virtualenv python-oauth2client xz-utils \
     python3.6

# If Python 3.5 is the default, switch it to Python 3.6.
python3 --version
# If above version says 3.5, you'll need to run:
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.5 1
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 2
sudo update-alternatives --config python3
```

This command also installs git's graphical front end (`git gui`) and revision history browser (`gitk`).



### Install Google depot_tools

The depot_tools is a package of useful scripts, provided by Google, to manage source code checkouts and code reviews. We need it to fetch the Chromium OS source code.

```bash
(outside)
$ sudo mkdir -p /usr/local/repo
$ sudo chmod 777 /usr/local/repo
$ cd /usr/local/repo
$ git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git

```

Then add the depot_tools directory to PATH and set up proper umask for the user who is going to perform the build. Add below lines to the file `~/.bash_profile` of that user. Or if you are using a different shell, handle that accordingly.

```bash
(outside)
export PATH=/usr/local/repo/depot_tools:$PATH
umask 022
```

Then re-login to your shell session to make the above changes take effect.




### Configure git

You should configure git now or it may complain in some operations later.

```bash
(outside)
$ git config --global user.email "you@email.address"
$ git config --global user.name "Your Name"
```


<br>

## Get source code

### Create directory structure

The directory structure described here is a recommendation based on the best practice in the Fyde Innovations team. You may host the files in a different way as you wish.

```bash
(outside)
# This is the directory to hold Chromium OS source code， aka cros-sdk
$ mkdir -p r102
```

If you are building a different release, make sure you use the actual directory name on your own system, the name here mentioned is just an example.



### Fetch Chromium OS repo

First, you need to find out the reference name of the release you would like to build, by visiting this page [https://chromium.googlesource.com/chromiumos/manifest.git](https://chromium.googlesource.com/chromiumos/manifest.git):

You will see a list of git commit IDs and its name in the form of `refs/heads/release-Rxx-xxxx.B`. That `release-Rxx-XXXX.B` link is what you need for fetching the code of that specific Chromium OS release. For example, [release-R102-14695.B](https://chromium.googlesource.com/chromiumos/manifest.git/+/refs/heads/release-R102-14695.B) for release r102.

Now run these commands to fetch the source code. Find and use a different release name if you would like to build a different release.

```bash
(outside)
$ cd r102

$ repo init -u https://chromium.googlesource.com/chromiumos/manifest.git --repo-url https://chromium.googlesource.com/external/repo.git -b release-R102-14695.B

```



### Request for Google and FydeOS API keys

If you would like to login into the Chromium OS GUI by using your Google account, you will need to request for Google API key and include them in the disk image you build. 

Apply for Google API on the Google website per [this document](http://www.chromium.org/developers/how-tos/api-keys). After acquiring the client ID, client secret and API key, put them in `~/.googleapikeys` file in the format as below:

```
'google_api_key': 'your api key',
'google_default_client_id': 'your client id',
'google_default_client_secret': 'your client secret',
```

Similarly, if you wish to utilise FydeOS online account and sync features provided by https://account.fydeos.com, you will need to apply for an openFyde Developer API key and include it in the same `~/.googleapikeys` file. At this moment the application is done manually by sending an email to [dev-support@openfyde.io](mailto:dev-support@openfyde.io) including your FydeOS account(you can sign up for a new one if you don't have it already) details, the team will process your application as soon as possible.

Once you have your openFyde Developer API key, you need to append them to the `~/.googleapikeys` file in the format as below:

```
'fydeos_default_client_id' : 'your openFyde Developer API client id',
'fydeos_default_client_secret' : 'your openFyde Developer API client secret',

```

Then the Chromium OS build script will read the necessary information from this file automatically, and the image you build will allow Google account as well as FydeOS account sign-in.


<br>

## Setup openFyde

Now fetch openFyde manifest, also create symlinks in the designated place to override the default one used by Chromium OS.

```bash
(outside)
$ cd r102
$ mkdir openfyde

$ git clone https://github.com/openFyde/manifest.git openfyde/manifest -b r102-dev

$ ln -snfr openfyde/manifest .repo/local_manifests
```

Now let us start syncing:

```bash
(outside)
# Raise this number if you have a fast internet connection
$ repo sync -j8

$ cd openfyde/chromium
$ gclient sync
```
Fetching of Chromium OS source code may take more than 30 minutes depending on your connection speed, around 10GB of data will need to be downloaded primarily from googlesource.com, it'd be helpful if you have a decent internet speed to reach google's server.

Once `gclient sync` is completed, the chromium source folder is now fully set up.


<br>

## Build openFyde for amd64
_and some development tips and tricks_

### Create the chroot

As mentioned above, a chroot environment will be used to run the actual build process and some other related tasks. To create the chroot environment, run the commands below:

```
(outside)
$ cd r102
$ cros_sdk
```

If you wish to build your own chromium and you have followed the steps to set it up, you need to specify it when entering the `cros_sdk` by:


```bash
(outside)
$ cd r102
$ cros_sdk --chrome-root /path/to/your/chromium #absolute path needed
```


It may take around 30 minutes depending on your internet connection speed and disk io speed. Once finished, it will enter the chroot. The shell prompt string looks like below so it is very easy to tell whether you are currently in the chroot or not.

```
(inside)
(release-R102-14695.B/(xxxxxx...)) <user>@<host> ~/trunk/src/scripts $
```

The chroot environment is located under the `r102/chroot` directory.

Let's exit from the chroot first as we need to do some customisation before moving on. Type `exit` or `ctrl + d` to exit from the chroot shell.

Usually, the chroot only needs to be created once and can be used to build a board many times or build different boards. It very rarely needs to be removed/re-created.



### Delete the chroot

If you would like to remove the chroot and re-create it from scratch, don't delete the `chroot` directory directly. As there could be directories from the host OS bind mounted in the chroot, an `rm chroot` command could actually remove files from your host OS undesirably.

The correct way to remove the chroot is by using the commands below:

```bash
(outside)
$ cd r102
$ cros_sdk --delete
```



### To enter the chroot again

Now we can enter the chroot.

```bash
(outside)
$ cd r102
$ cros_sdk
```



### Set password for the chronos user

The chronos user is used to log into the command line interface of Chromium OS, via ssh, local console or the shell in crosh interface. It is recommended that a password is set for this user so you can login as this user and also can do `sudo` in the Chromium OS command line, for advanced tasks.

To set a password for chronos user, run the command below:

```bash
(inside)
$ ./set_shared_user_password.sh
```

Type in a password when prompted. If you would like to change the password, simply run the command again.

The password is encrypted and saved in the file `/etc/shared_user_passwd.txt` in the chroot. You only need to set it once and it will be used for all the images you build unless you re-create the chroot.



### Setup board amd64-openfyde

In the Chromium OS terminology, a board refers to a class of computer platforms with distinct hardware configurations. The board will be used as a target in the process of building software packages and disk images for that specific computer platform.

There are many boards that exist in the Chromium OS codebase. They are either development platforms or actual retail hardware products running Chrome OS, such as Chromebooks you can buy from many vendors.

The Chromium OS project utilises the Portage package management system from Gentoo Linux. Each board lives in its own "overlay", which holds distinct build configuration, system configurations, collection of software packages, system services, disk image customisation etc. for that board.

In our case here, we created a board named "amd64-openfyde" and it refers to the x86_64 PCs. And we call the overlay "amd64-openfyde", all its files are hosted in (https//github.com/openFyde/overlay-amd64-openfyde)[https://github.com/openFyde/overlay-amd64-openfyde].

To build Chromium OS for a specific board, the first thing is to initialise the board from its overlay.

```bash
(inside)
$ setup_board --board=amd64-openfyde
```

Again, it may take 10 to over 30 minutes depending on the speed of your internet connection and disk i/o.

Once it's done, a directory structure for the "amd64-openfyde" board will be created under `/build/amd64-openfyde/` of the chroot.



### Re-initialise the board

It is usually not necessary to re-initialise the board as what you have already built will be lost, and you will have to spend hours to rebuild all packages from scratch. But if you really need to do so, just re-run the same setup_board command with the `---force` option.

```bash
(inside)
$ setup_board --board=amd64-openfyde --force
```

The `--force` option will remove the existing board directory `/build/amd64-openfyde` and re-create it from scratch.



### Build packages

#### Install package `dev-libs/capnproto`

Before starting to build packages, the package `dev-libs/capnproto` should be installed.

```bash
(inside)
sudo emerge capnproto
```

#### Build all packages
Now it is time to build all software packages for the amd64-openfyde board.

```bash
(inside)
$ ./build_packages --board=amd64-openfyde --nowithautotest --autosetgov --nouse_any_chrome
# Append "--nowithautotest" to speed up the build process by skipping some tests
```

It may take hours depending on your processor power, your memory size, your disk speed and the quality of your internet connection. Here are some examples for you to adjust your expectation: 

- On a decent machine with 4 cores 8 threads, 16GB memory, files on regular HDD, and 100Mb broadband, it takes about 5 to 6 hours for the command to finish.
- On a Workstation-grade server with AMD Threadripper 3990x CPU with 64-core 128-thread, 128GB memory and 300Mb broadband, it takes 44mins for the command to finish.


#### Things to note

- **What is happening now**

  The `build_packages` script acts as an entry point to initialise a series of processes aiming to compile all the necessary software packages from source code and build them together forming Chromium OS as a whole. During the process there are a few required dependencies will be fetched and cloned from GitHub, so please do ensure a decent internet connection to GitHub.


- **When interrupted**

  The build process is incremental. If it gets interrupted for any reason, you can always re-run the same `build_packages` command and it will resume the build instead of rebuilding from scratch.


- **Read the output**

  The `build_packages` command throws out a lot of information on the console. Fortunately, that information is very well organised.

  - <span style="color:red">Red text</span>: these are error messages and very likely will cause the build process to break.
  - <span style="color:green">Green text</span>: these are useful messages printed by the build script itself. They are useful when debugging problem.
  - White text: these are regular information that mostly is printed by the commands called in the build script. They provide more details about the build process and thus are also useful for debugging.


- **Read the logs**

  The `build_packages` script spends most of its airtime on running the `emerge` commands, to build, install and pack those hundreds of software packages required by the overlay. The `emerge` command is from the Portage system of Gentoo Linux.

  The `emerge` command saves the output of its building, installation and packing process into log files. These files are extremely useful if there is a failure when building packages. Those log files are located under the `/build/amd64-openfyde/tmp/portage/logs` directory of the chroot. They are plain text files so can be viewed right from your command-line interface.



### Build the disk image

After the build_packages command is finished successfully, you can start building the disk image.

```bash
(inside)
$ ./build_image --board=amd64-openfyde --noenable_rootfs_verification
# Append --noenable_rootfs_verification flag to enable root file system read/write on the built image
```

It may take 10 to 30 minutes, mainly depending on the speed of your disk. It will be much faster on SSD than on HDD.


### Find your image

After the command finished successfully, you will have disk images generated, saved under `/mnt/host/source/src/build/images/amd64-openfyde/` directory in the chroot, or `r102/src/build/images/amd64-openfyde` in the host OS. These two are the same directory, just bind mounted in the chroot.

Each time the build_image command gets invoked it will create a directory named similar to `R102-XXXX.XXX.<date time>-a1` under above directory. There is a symlink named `latest` under the above directory, it always points to the image directory of the last successful build.

The disk image is usually named `chromiumos_image.bin`, under the abovementioned directory. So full path to the latest image is

```
/mnt/host/source/src/build/images/amd64-openfyde/latest/chromiumos_image.bin
```

in the chroot, and

```
r102/src/build/images/amd64-openfyde/latest/chromiumos_image.bin
```

in the host OS.


<br>

## Boot from the image

We need to write the previously generated disk image onto a USB storage device (preferably a higher speed USB HDD), at least 8GB capacity is required.



### Write the disk image to a USB storage device

There are two usual ways to write the Chromium OS disk image to a USB storage device. You can copy the image out to another Windows/macOS/Linux system and write it using your favourite GUI/CLI application.

Another Chromium OS specific way is by using the `cros` command in the chroot.



### Write the image by using the `cros` command

First, plug the USB stick into the box used to build the image and has the chroot. Then run the command below:

```
(inside)
$ cros flash usb:// amd64-openfyde/latest
```

This asks to write the latest disk image to USB removable media. A list of USB removable media will be presented, with the index number prefixed. You can select which USB drive to write to by typing in the index number when prompted.



### Boot from the USB storage device

After the disk image is successfully written to the USB storage device, plug it into your PC and boot it as usual. After a few seconds, you will see a Chromium logo, momentarily it will boot into GUI mode and the first time setup screen (OOBE) will pop up for you to configure the system and sign in.



<br>

## More information

[Chromium OS Developer Guide](http://www.chromium.org/chromium-os/developer-guide). This is the official source of how to build Chromium OS

[The FydeOS website](https://fydeos.com), our home.

[FydeOS official Telegram group](https://t.me/hi_fydeos), to say hi and get help.



<br>

## Last but not least

openFyde and FydeOS are made possible by gentle souls with real ❤️
