---
layout: post
title: Caffe + Fedora 21 + CUDA 7.5 开发环境配置总结
---
> 阅读须知：本文主要针对使用Caffe进行深度学习研究的用户，提供Fedora 21的系统开发环境配置说明，未使用Fedora 22/23的主要原因是CUDA 7.5的更新支持与对稳定性的需求，未使用CentOS 7的主要原因是虽相对稳定但缺乏较新的软件包（若为服务器端则仍建议使用CentOS 7）。除少数系统依赖的特性外，本文大多数配置亦可用于Ubuntu的配置参考。

# 1. 系统配置
<br>

## 系统更新
<br>

### 安装yum-axelget
yum-axelget是一款应用于yum的并行下载工具，可显著加快更新速度。

```sh
sudo yum install yum-axelget
```

### 全面更新

```sh
sudo yum update
```

更新后建议重启。
<br>

## 基础软件安装
<br>

### 安装fedy
[fedy](http://folkswithhats.org/)类似于ubuntu tweaks，可用于方便的安装与调整部分基础软件和系统设置，包括但不限于：

- Google Chrome
- Dropbox
- Sublime Text 3
- Archive formats
- Better font rendering
- SSD optimization

经过字体安装与设置后系统字体渲染将大为改善，其余UI设置可通过yum安装`gnome-tweak-tool`完成。

> 此处请安装sublime text，若未安装，请将后文的subl命令替换为gedit

![fedy](http://folkswithhats.org/assets//fedy.png)

```sh
sudo bash -c 'su -c "curl http://folkswithhats.org/fedy-installer -o fedy-installer && chmod +x fedy-installer && ./fedy-installer"'
```
<br>

### 基础开发工具与开发库
`Development Tools`与`Development Libraries`包含了众多fedora未预装的开发工具与开发库，包括`gcc`, `gcc-c++`, `make`等。

```sh
sudo yum groupinstall "Development Tools" "Development Libraries"
```
<br>

### 安装terminator
[terminator](http://gnometerminator.blogspot.com/p/introduction.html)基于python写就，可用于替代系统自带的终端gnome terminal，相较而言提供了更多配置项，界面更美观，功能更丰富实用。

```sh
sudo yum install terminator
```

安装后可在Preferences的keybindings中通过直接按键调整如下：

- `split_horiz` -> `Shift+Ctrl+D`
- `split_vert` -> `Shift+D`

如此可与OS X下的iterm2保持一致，方便的切分子屏。

<br>

### 安装zsh与prezto
[zsh](https://en.wikipedia.org/wiki/Z_shell)作为一款shell，可用于替代系统所自带的bash，即使是为了目录跳转的自动补全功能也值得一试。

[prezto](https://github.com/sorin-ionescu/prezto)为zsh提供了众多预置配置，与oh-my-zsh相比更加简洁而不繁复

```sh
sudo yum install zsh

git clone --recursive https://github.com/sorin-ionescu/prezto.git "${ZDOTDIR:-$HOME}/.zprezto"

ln -s ~/.zprezto/runcoms/zlogin ~/.zlogin
ln -s ~/.zprezto/runcoms/zlogout ~/.zlogout
ln -s ~/.zprezto/runcoms/zpreztorc ~/.zpreztorc
ln -s ~/.zprezto/runcoms/zprofile ~/.zprofile
ln -s ~/.zprezto/runcoms/zshenv ~/.zshenv
ln -s ~/.zprezto/runcoms/zshrc ~/.zshrc
```

安装后开启zsh，并切换当前user与root的默认shell为zsh

```sh
zsh
chsh -s /usr/bin/zsh
sudo chsh -s /usr/bin/zsh
```
<br>

### pyenv
[pyenv](https://github.com/yyuu/pyenv)是一款python环境管理工具，可用于方便的安装与管理众多python版本，包括流行的科学计算平台Anaconda

```sh
curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | zsh
```

安装后需要将如下语句加入shell配置文件

> 此处应为`~/.zshrc`而非`~/.bashrc`，可通过`ctrl+h`显示或在终端中使用`subl ~/.zshrc`打开

```sh
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

完成添加后重启终端，使用如下命令安装并设置默认python版本为Anaconda

> 之后可通过`pyenv versions`命令查看已安装与默认python版本，并通过`pyenv global [python-version]`命令切换

```sh
pyenv install anaconda-2.3.0
pyenv global anaconda-2.3.0
```

**Anaconda**提供了一个类似于`pip`的包管理工具`conda`

```sh
conda update conda    # 更新module
conda list    # 查看已安装module
conda install [module] # 安装新module
```

> 如conda所预置的module无法满足需求，可尝试使用`pip install`安装，且[anaconda.org](http://anaconda.org/)(原binstar.org)中提供了众多用户自建的module，如opencv等，可自行搜索安装。

<br>

## 可选软件安装
<br>

### 安装JDK[^6]
因为jetbrains系的多款IDE以及Android Studio均需要JDK运行，此处推荐JDK 1.7。

下载

```sh
cd ~/Downloads
wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-linux-x64.rpm"
```

安装

```sh
sudo yum localinstall jdk-7u79-linux-x64.rpm
```

当前JDK安装路径为`/usr/java/jdk1.7.0_79`，将以下语句加入`/etc/profile.d/java.sh`以设置环境变量:

```sh
export JDK_HOME=/usr/java/jdk1.7.0_79
export PATH=$JDK_HOME/bin:$PATH
export JAVA_HOME=/usr/java/jdk1.7.0_79
export PATH=$JAVA_HOME/bin:$PATH
```

保存后, 执行下列命令, 使环境变量立即生效

```sh
source /etc/profile.d/java.sh
```

<br>

### 安装redshift
[redshift](http://jonls.dk/redshift/)是一款与[f.lux](https://justgetflux.com/)类似的屏幕色温调节软件，对于视力保护有一定帮助。

```sh
sudo yum install redshift
sudo redshift
```
<br>

### 安装clang
Clang可以认为是GCC的替代品，可以用于编译C、C++、Objective-C和Objective-C++。其提供了更友好的报错信息，在有些方面比GCC更友好，同时其提供了一个代码静态分析器，可以用于分析代码中可能出现的bug和内存溢出问题。[^1]

```sh
sudo yum install clang clang-analyzer
```
<br>

### 安装remarkable
remarkable作为一款markdown编辑器，功能齐备，基本满足编辑需求，相比sublime text+markdown preview插件的方法更加直观。

> [http://remarkableapp.github.io/index.html](http://remarkableapp.github.io/index.html)

<br>

### 安装foxit reader
foxit reader的linux版本，为时隔六年后的最新更新，字体渲染明显好于Gnome Evince和Google Chrome，常用的标注与高亮功能一应俱全。

> [https://www.foxitsoftware.com/downloads](https://www.foxitsoftware.com/downloads)


----------

# 2. CUDA与显卡驱动安装
<br>

### 下载CUDA 7.5安装文件

> [https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)

建议存于`~/Downloads`文件夹，并赋予可执行属性

```sh
chmod +x ./cuda_7.5.18_linux.run
```
<br>

### 开启root user权限

```sh
sudo su
```
<br>

### 安装相应版本kernel
此处应确保在`sudo yum update`后进行，否则版本可能发生错误。

```sh
yum install gcc kernel-devel-$(uname -r)
```
<br>

### 禁用默认nouveau驱动[^2]

```sh
subl /etc/modprobe.d/blacklist.conf
echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf
```

> 之后在`/etc/sysconfig/grub`文件的`GRUB_CMDLINE_LINUX=`这一行添加`rd.driver.blacklist=nouveau`

激活，之后卸载nouveau驱动。

```sh
grub2-mkconfig -o /boot/grub2/grub.cfg
yum remove xorg-x11-drv-nouveau.x86_64
```
<br>

### 禁用图形界面启动

```sh
systemctl set-default multi-user.target
reboot
```
<br>

### 重启后进入终端环境，安装CUDA

```sh
sudo su
cd ~/Downloads
./cuda_7.5.18_linux.run
```

> 若安装发生kernel问题，执行以下命令后继续安装：

```sh
yum install kernel-devel-$(uname -r)

grub2-mkconfig -o /boot/grub2/grub.cfg
```
<br>

### 恢复图形界面启动并重启

```sh
systemctl set-default graphical.target
reboot
```
<br>

### 验证CUDA及驱动安装[^3]
重启后进入`/usr/local/cuda/samples`，, 执行下列命令来build samples：

```sh
sudo make all -j8
```

全部编译完成后， 进入 samples/bin/x86_64/linux/release, 运行deviceQuery

```sh
./deviceQuery
```

如果出现显卡信息， 则驱动及显卡安装成功，可在系统中启动NVIDIA X Server Settings查看显卡状态。

<br>

### 设置CUDA环境变量

创建文件`sudo subl /etc/profile.d/cuda.sh`，加入以下语句:

```sh
export PATH=$/usr/local/cuda-7.5/bin:$PATH
```

保存后, 执行下列命令, 使环境变量立即生效

```sh
source /etc/profile.d/cuda.sh
```

创建文件`sudo subl /etc/ld.so.conf.d/cuda.conf`，加入以下语句:

```sh
/usr/local/cuda/lib64
```
保存后，执行下列命令使之立刻生效

```sh
sudo ldconfig
```

----------



# 3. Caffe安装
<br>

### 下载Caffe最新版本

```sh
git clone https://github.com/BVLC/caffe.git
```
<br>

### 安装基础依赖库[^4]

```sh
sudo yum install protobuf-devel leveldb-devel snappy-devel opencv-devel boost-devel hdf5-devel

sudo yum install gflags-devel glog-devel lmdb-devel atlas-devel
```
<br>

### 安装pycaffe依赖[^3]
打开新的终端, 用`which python`和`which pip`确定使用的是anaconda-2.3.0提供的python环境，然后进入`caffe_root/python`, 执行下列命令


```sh
for req in $(cat requirements.txt); do pip install $req; done
```
<br>

### 安装OpenBLAS[^5]
Caffe的BLAS接口可选Atlas, Intel MKL与OpenBLAS，此处推荐OpenBLAS

```sh
git clone https://github.com/xianyi/OpenBLAS.git
cd OpenBLAS
make -j8
sudo make install
```

安装后路径位于`/opt/OpenBLAS`，之后设置OpenBLAS环境变量

创建文件`sudo subl /etc/ld.so.conf.d/openblas.conf`，加入以下语句:

```sh
/opt/OpenBLAS/lib
```

保存后，执行下列命令使之立刻生效

```sh
sudo ldconfig
```

<br>

### 安装cuDNN[^3]

下载

> [https://developer.nvidia.com/cudnn](https://developer.nvidia.com/cudnn)

安装

```sh
tar -zxvf cudnn-7.0-linux-x64-v3.0-prod.tgz
cd cuda
sudo cp include/cudnn.h /usr/local/cuda/include/
sudo cp lib64/lib* /usr/local/cuda/lib64/
```

更新软链接

```sh
cd /usr/local/cuda/lib64/
sudo rm -rf libcudnn.so libcudnn.so.7.0
sudo ln -s libcudnn.so.7.0.64 libcudnn.so.7.0
sudo ln -s libcudnn.so.7.0 libcudnn.so
```
<br>

### 安装OpenCV与Matlab

此处非必须，可参考[^3].

- 对于OpenCV，如果之后只用到pycaffe接口的话，在编译caffe与pycaffe完成后直接使用conda安装即可，若提前安装会出现openblas冲突等错误。

```sh
conda install -c http://conda.anaconda.org/menpo opencv3
```

- 对于Matlab，当前版本R2015b需要gcc 4.7，而fedora 21自带版本为gcc 4.9，需要安装所需版本matcaffe才能编译通过。

<br>

### make.config文件

完成如上配置之后，在caffe目录编辑make.config如下，请根据具体配置酌情修改。

```
USE_CUDNN := 1

# CPU_ONLY := 1

# CUSTOM_CXX := g++

CUDA_DIR := /usr/local/cuda

CUDA_ARCH := -gencode arch=compute_20,code=sm_20 \
		-gencode arch=compute_20,code=sm_21 \
		-gencode arch=compute_30,code=sm_30 \
		-gencode arch=compute_35,code=sm_35 \
		-gencode arch=compute_50,code=sm_50 \
		-gencode arch=compute_50,code=compute_50

BLAS := open
BLAS_INCLUDE := /opt/OpenBLAS/include
BLAS_LIB := /opt/OpenBLAS/lib

# MATLAB_DIR := /usr/local/MATLAB/R2015b/

ANACONDA_HOME := $(HOME)/.pyenv/versions/anaconda-2.3.0
PYTHON_INCLUDE := $(ANACONDA_HOME)/include \
		$(ANACONDA_HOME)/include/python2.7 \
		$(ANACONDA_HOME)/lib/python2.7/site-packages/numpy/core/include \
PYTHON_LIB := $(ANACONDA_HOME)/lib

WITH_PYTHON_LAYER := 1

INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include
LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib

BUILD_DIR := build
DISTRIBUTE_DIR := distribute

# DEBUG := 1

TEST_GPUID := 0

Q ?= @
```
<br>

### 修复libhdf5错误

```sh
cd /usr/lib64/
sudo ln -s libhdf5_hl.so.8.0.2 libhdf5_hl.so.10
sudo ln -s libhdf5.so.8.0.2 libhdf5.so.10
```
<br>

### 编译Caffe

```sh
cd ~/Documents/Library/caffe
make all -j8
make pycaffe
make test
make runtest
```

----------
[^1]: [用CentOS 7打造合适的科研环境](http://seisman.info/linux-environment-for-seismology-research.html)
[^2]: [Fedora21安装Nvidia的闭源驱动](http://binglispace.com/2015/02/21/fedora21-nvidia/)
[^3]: [Caffe + Ubuntu 12.04 64bit + CUDA 6.5 配置说明](https://gist.github.com/bearpaw/c38ef18ec45ba6548ec0)
[^4]: [RHEL / Fedora / CentOS Installation](http://caffe.berkeleyvision.org/install_yum.html)
[^5]: [ubuntu上Caffe使用OpenBLAS多线程加速](http://wxyblog.com/2015/08/27/openblas-with-caffe-on-ubuntu/)
[^6]: [How To Install Java on CentOS and Fedora](https://www.digitalocean.com/community/tutorials/how-to-install-java-on-centos-and-fedora)


