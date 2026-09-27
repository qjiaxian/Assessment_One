# M0-1 开发环境搭建与验收

## 1. 实验目的

本次 M0-1 任务主要用于从零搭建机器人项目所需的基本开发环境，为后续 ROS 2、Python、C/C++ 等相关开发任务做好准备。

主要完成以下内容：

- 安装并检查 Ubuntu 22.04 开发环境
- 配置 ROS 2 Humble
- 配置 Python 和 Python 虚拟环境工具
- 配置 C/C++ 编译环境
- 安装 VS Code 及相关开发工具
- 安装并配置 Git
- 创建个人 Git 仓库并完成基本使用
- 使用环境检查脚本对开发环境进行验收

## 2. 开发环境

| 项目 | 配置 |
|---|---|
| 操作系统 | Ubuntu 22.04.5 LTS |
| 虚拟机 | VMware |
| ROS 2 | Humble |
| Python | 3.10.12 |
| pip | 22.0.2 |
| uv | 0.12.19 |
| GCC | 11.4.0 |
| G++ | 11.4.0 |
| Make | 4.3 |
| CMake | 3.22.1 |
| VS Code | 1.139.1 |
| Git | 2.34.1 |

## 3. 环境搭建过程

### 3.1 ROS 2 Humble

#### 3.1.1 配置方法

本次开发环境使用 Ubuntu 22.04，并按照 ROS 2 Humble 官方安装文档完成 ROS 2 环境配置。

首先安装基础工具：
sudo apt install curl -y

随后配置 ROS 2 软件源，安装 ros2-apt-source，并更新软件源：
sudo apt update

安装 ROS 2 Humble Desktop：
sudo apt install ros-humble-desktop -y

安装完成后配置 ROS 2 环境：
source /opt/ros/humble/setup.bash

为了使每次打开终端都能够自动使用 ROS 2，将环境配置加入 ~/.bashrc：
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc

### 3.1.2 本机踩坑实录

问题：安装 ROS 软件源时报错

第一次尝试执行：
sudo apt install ros-apt-source -y
终端出现：
E: 无法定位软件包 ros-apt-source

通过询问GPT得知当前安装方式应使用 ROS 官方提供的 ros2-apt-source 软件源配置包，而不是直接安装 ros-apt-source。

解决方法：
按照 ROS 2 官方安装方式下载并安装 ros2-apt-source，然后重新更新软件源：
sudo apt update
之后系统能够正常识别 ROS 2 软件源。
随后执行：
sudo apt install ros-humble-desktop -y
最终成功完成 ROS 2 Humble 的安装。

### 3.2 Python 环境

#### 3.2.1 Python 和 pip

Ubuntu 22.04 中使用 Python 3.10 作为 Python 开发环境。

首先检查 Python 版本：
python3 --version
检查结果：
Python 3.10.12

由于系统初始环境中没有安装 pip，因此安装 pip：
sudo apt install python3-pip -y

安装完成后检查 pip 版本：
pip3 --version
检查结果：
pip 22.0.2

#### 3.2.2 安装 uv

为了方便后续 Python 虚拟环境的创建和管理，安装 uv。

执行：
curl -LsSf https://astral.sh/uv/install.sh | sh

安装完成后加载 uv 环境：
source $HOME/.local/bin/env

检查 uv 版本：
uv --version
检查结果：
uv 0.12.19

至此，Python、pip 和 uv 均已完成安装和版本检查。

#### 3.2.3 创建并激活 Python 虚拟环境

在个人项目 `Assessment_One` 目录下使用 uv 创建 Python 虚拟环境：
uv venv

创建成功后，激活虚拟环境：
source .venv/bin/activate
激活成功后，终端前出现：
(Assessment_One)

通过以下命令检查当前 Python：
which python
输出：
/home/qjiaxian/Assessment_One/.venv/bin/python
检查 Python 版本：
python --version
输出：
Python 3.10.12

说明 Python 虚拟环境已经成功创建并激活。

#### 3.2.4 本机踩坑记录

第一次运行 M0-1 环境检查时，出现：
[FAIL] pip 不可用

检查发现虚拟环境中的 Python 没有 ensurepip 模块：
python -m ensurepip --upgrade
出现：
No module named ensurepip

之后使用 uv 安装 pip：
uv pip install pip
检查发现 pip 已经安装到：
/home/qjiaxian/Assessment_One/.venv/
但终端中的 pip 命令仍然指向系统 pip。

使用：
which pip
确认最终 pip 位于：
/home/qjiaxian/Assessment_One/.venv/bin/pip

由于 Shell 缓存了之前的 pip 路径，执行：
hash -r
清除缓存后重新检查：
pip --version
最终得到：
pip 26.2.1 from /home/qjiaxian/Assessment_One/.venv/lib/python3.10/site-packages/pip

问题解决。

### 3.3 C/C++ 开发环境

#### 3.3.1 环境检查

为了满足后续 C/C++ 项目的编译和构建需求，对 GCC、G++、Make 和 CMake 进行了检查。

检查 GCC 版本：
gcc --version
检查结果：
gcc 11.4.0

检查 G++ 版本：
g++ --version
检查结果：
g++ 11.4.0

检查 Make 版本：
make --version
检查结果：
GNU Make 4.3

检查 CMake 版本：
cmake --version
检查结果：
cmake version 3.22.1

以上工具均能够正常使用，可以满足后续 C/C++ 程序的编译和项目构建需求。

#### 3.3.2 本机踩坑记录

本次 C/C++ 环境配置过程中没有遇到需要额外解决的报错。
通过版本检查确认 GCC、G++、Make 和 CMake 均已正常安装并可以使用。

### 3.4 VS Code

#### 3.4.1 安装与检查

使用 Snap 安装：
sudo snap install code --classic

安装完成后检查 VS Code 版本：
code --version
检查结果：
1.139.1

说明 VS Code 已经成功安装，可以正常启动和使用。

#### 3.4.2 插件检查

根据 M0-1 的开发需求配置了 Python、C/C++ 和远程开发相关插件。

已安装的主要插件包括：
Python
Pylance
Python Debugger
Python Environments
C/C++
Remote - SSH
Remote Explorer
Remote - SSH: Editing Configuration Files

其中 Python、C/C++ 和 Remote - SSH 均能够通过环境检查。

本机使用 VMware 运行 Ubuntu 22.04，并不是 WSL2 环境，因此没有安装 Remote - WSL 插件。

#### 3.4.3 本机踩坑记录

VS Code 初次使用时，部分中文字符显示异常，出现字符周围有方框的情况。

经过检查发现与 VS Code 的 Unicode Highlight 设置有关。调整：
Editor › Unicode Highlight: Non Basic ASCII
相关设置并重新启动 VS Code 后，中文显示恢复正常。

### 3.5 Git

#### 3.5.1 Git安装与检查

首先检查 Git：
git --version
得到：
git version 2.34.1

说明 Git 已经安装并可以正常使用。

#### 3.5.2 Git 用户信息配置

根据 M0-1 要求配置 Git 用户名和邮箱：
git config --global user.name "M0-1"
git config --global user.email "3326580952@qq.com"
检查配置：
git config --global user.name
git config --global user.email

结果为：
M0-1
3326580952@qq.com

Git 用户信息配置完成。

## 4. Git 仓库创建

为了保存后续 M0、M1 等任务的代码和实验记录，在 GitHub 上创建个人仓库：
Assessment_One

本地创建对应项目目录：
cd ~
mkdir Assessment_One
cd Assessment_One

初始化 Git 仓库：
git init

设置默认分支：
git branch -M main

按照任务要求建立目录结构：
Assessment_One
└── M0
    └── M0-1
        └── README.md

其中：
Assessment_One：整个个人项目仓库
M0：M0 模块
M0-1：M0 模块中的第一个任务
README.md：记录本次任务的环境配置、问题及验收结果

目前已经完成本地 Git 仓库和基本目录结构的创建。

GitHub 远程仓库已经创建，后续将完成本地仓库与 GitHub 远程仓库的连接和代码推送。

## 5. M0-1 环境验收

使用项目提供的 check_env.sh 对开发环境进行统一检查。

最终检查结果：
PASS=28
FAIL=0
WARN=4

最终结果：
环境验收: 通过（WARN 项请现场向考官说明）

4 项 WARN 分别为：
缺少 Remote - WSL 插件。由于本机使用 VMware Ubuntu 22.04，而不是 WSL2，因此不安装该插件。
当前尚未创建 SSH 公钥，后续根据任务要求配置 SSH 密钥。
known_hosts 当前为空，后续实际连接开发板或服务器后会产生记录。
当前检查的是老师提供的 22SmartCar 仓库，仓库结构中未发现顶层 M0/M1 模块目录。

以上 WARN 均不影响本次环境验收。

最终：
FAIL=0

因此 M0-1 开发环境验收通过。

## 6. 当前完成情况

## 7. 后续任务