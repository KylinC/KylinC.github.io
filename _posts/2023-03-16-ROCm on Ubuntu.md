---
dlayout:    post
title:      Ubuntu22.04+RX7900xtx 安装 AMD ROCm 4.5.2
subtitle:   安装先前版本的 ROCm 方案
date:       2023-3-16
author:     Kylin
header-img: img/ubuntu.jpg
catalog: true
tags:
    - Linux
---



[TOC]

### Ubuntu22.04+RX7900xtx 安装 AMD ROCm 4.5.2

#### Configuration：

i9-13900K+RX7900xtx+Ubuntu 22.04+ROCm 4.5.2

#### amdgpu-install

We going to use a terminal. So open one. Create a directory to work in:

```bash
mkdir ~/ROCm && cd ~/ROCm
```

Update, then download the `.deb` file for `amdgpu-install`, and install it ([link gotten from AMD](https://docs.amd.com/bundle/ROCm-Installation-Guide-v5.2.1/page/How_to_Install_ROCm.html#_How_to_Install)):

```bash
sudo apt update
wget https://repo.radeon.com/amdgpu-install/5.4/ubuntu/jammy/amdgpu-install_5.4.50400-1_all.deb
sudo apt install ./amdgpu-install_5.4.50400-1_all.deb
```

> 只需要下载最新版的，rocm可以配置之前的版本：https://docs.amd.com/bundle/ROCm-Installation-Guide-v5.1/page/How_to_Install_ROCm.html

We now have to edit `amdgpu-install`:

```bash
sudo gedit /usr/bin/amdgpu-install 
```

- Pop!_OS is not listed as supported by `amdgpu-install`, so we add it: Search for `ubuntu`, and add `|pop` to the list (`|` reads "or").

- Search for `linux-modules-extra` and replace the entire function `debian_build_package_list()` with, on one line,

  ```
  function debian_build_package_list() { echo 'empty function'; }
  ```

- Save and quit

#### python3

安装之后的编译相关

```
sudo add-apt-repository --yes ppa:deadsnakes/ppa
sudo apt-get update
sudo apt install --yes python3.8
```

> 不要试图卸载python，这样会导致ubuntu图形界面崩溃

#### ROCm Repositories and Package Editing

Next, we add the desired ROCm repository. The link is relative to the ROCm version to be installed, [so look up the base URL here](https://docs.amd.com/bundle/ROCm-Installation-Guide-v5.2.1/page/How_to_Install_ROCm.html#_Selecting_Base_URLs).

```bash
echo 'deb [arch=amd64] https://repo.radeon.com/rocm/apt/4.5.2 ubuntu main' | sudo tee /etc/apt/sources.list.d/rocm.list
sudo apt update
```

The download the `.deb` file for the ROCm package:

```bash
sudo apt download rocm-llvm4.5.2
```

##### Edit Package 1/2

We need to edit this package before it will install. So we unpack, unpack and edit (by `<tab>` I mean press `Tab` to autocomplete):

```bash
ar x rocm-llvm<tab>
tar xf control.tar.xz
gedit control
```

In `control`, edit the `Depends` line to

```bash
Depends: python3, libc6, libstdc++6|libstdc++8, libstdc++-5-dev|libstdc++-7-dev|libstdc++-10-dev, libgcc-5-dev|libgcc-7-dev|libgcc-10-dev, rocm-core4.5.2
```

I.e., modified `python` to `python3` , add `|libstdc++-10-dev` and `|libgcc-10-dev` in the appropriate positions.

Then repack:

```bash
tar c postinst prerm control | xz -c > control.tar.xz
ar rcs rocm-llvm<tab> debian-binary control.tar.xz data.tar.xz
```

Great! Now, we install the possible dependencies we just added, and `rocm-core4.5.2`:

```bash
sudo apt install libstdc++-10-dev libgcc-10-dev rocm-core4.5.2
```

Now the downloaded package installed for me with

```bash
sudo dpkg -i rocm-llvm<tab>
```

Super.



##### Edit Package 2/2

Now we download, identically edit, and repack another package, `openmp-extras4.5.2`:

```bash
mkdir openmp && cd openmp
apt download openmp-extras4.5.2
ar x openmp<tab>
tar xf control.tar.xz
gedit control
```

Edit `Depends` line, add `|libstdc++-10-dev` and `|libgcc-10-dev` as before. Save, close, repack, install a dependency, and then the package with:

```bash
tar c control | xz -c > control.tar.xz
ar rcs openmp<tab> debian-binary control.tar.xz data.tar.xz
sudo apt install rocm-device-libs4.5.2
sudo dpkg -i openmp<tab>
```



#### Install ROCm

Install ROCm with the usecases you need. Mine are ROCm and HIP. See `amdgpu-install --help` for options.

```bash
sudo amdgpu-install --rocmrelease=4.5.2 --usecase="rocm,dkms,graphics,opencl,hip,hiplibsdk"
```

Now a final piece of setup: Add your user to the `render` and `video` groups:

```bash
sudo usermod -a -G render $LOGNAME
sudo usermod -a -G video $LOGNAME
```

and reboot.

Once rebooted, check that ROCm is loaded with

```bash
rocminfo
```



Finally, thx to the author of https://askubuntu.com/questions/1429376/how-can-i-install-amd-rocm-5-on-ubuntu-22-04
