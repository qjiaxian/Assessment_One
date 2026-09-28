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

## 5.远程登录与远程操作

为了满足后续机器人开发过程中远程连接开发板或服务器的需求，本次 M0-1 对 SSH 远程登录及相关文件传输工具进行了学习和配置。

### 5.1 SSH环境检查

首先检查 Ubuntu 中是否安装 SSH 客户端：
ssh -V
检查结果：
OpenSSH_8.9p1 Ubuntu-3ubuntu0.17
OpenSSL 3.0.2 15 Mar 2022

说明 SSH 客户端已经正常安装，可以用于后续远程登录。

### 5.2 SSH 密钥配置

为了后续实现更加方便的远程登录，生成 SSH 密钥对。

执行：
ssh-keygen -t ed25519 -C "3326580952@qq.com"
按照提示使用默认保存路径：
/home/qjiaxian/.ssh/id_ed25519
生成完成后，在 ~/.ssh/ 目录下得到 SSH 密钥文件。
其中：
id_ed25519
为私钥，应妥善保管，不能泄露。
id_ed25519.pub
为公钥，可以配置到远程服务器或开发板中。

通过以下命令查看公钥：
cat ~/.ssh/id_ed25519.pub

本机已经成功生成 SSH 密钥对。

### 5.3 SSH 远程登录基本方法

SSH 的基本登录格式为：
ssh 用户名@远程电脑IP
例如：
ssh user@192.168.1.100
其中：
user：远程电脑上的用户名
192.168.1.100：远程电脑的 IP 地址

实际使用时需要根据开发板或服务器提供的用户名和 IP 地址进行替换。
首次连接远程电脑时，SSH 可能会提示是否信任该主机。确认后，远程主机的信息会记录到：
~/.ssh/known_hosts

### 5.4 scp 文件传输

scp 可以通过 SSH 在本机和远程电脑之间传输文件。

本地文件上传到远程电脑：
scp 文件名 用户名@远程IP:远程路径
例如：
scp test.py user@192.168.1.100:~/ 

从远程电脑下载文件：
scp 用户名@远程IP:远程文件路径 本地路径
例如：
scp user@192.168.1.100:~/test.py ./

### 5.5 rsync 文件同步

rsync 主要用于目录或大量文件的同步，相比反复使用 scp 更适合后续项目开发。

基本使用格式：
rsync -av 本地目录/ 用户名@远程IP:远程目录/
例如：
rsync -av ./M0/ user@192.168.1.100:~/M0/

后续在机器人开发过程中，可以使用 rsync 将本地代码同步到开发板或服务器。

### 5.6 当前完成情况

### 5.1 SSH 环境检查

本机已安装并验证以下工具：
ssh -V
scp -V
ssh-keygen -V
rsync --version

检查结果：
SSH 客户端：已安装
scp：已安装
ssh-keygen：已安装
rsync：已安装

### 5.2 SSH Key 配置

使用 Ed25519 算法生成 SSH 密钥：
ssh-keygen -t ed25519 -C "3326580952@qq.com"
密钥文件：
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub

其中：
id_ed25519：私钥，只能保存在本机，不能泄露
id_ed25519.pub：公钥，可以配置到远程服务器

本机已经成功生成 SSH 公钥，并通过环境检查。

### 5.3 SSH 远程登录

获得开发板或服务器的 IP 地址、用户名和密码后，可以使用：
ssh 用户名@IP地址

例如：
ssh user@192.168.1.100

之后输入服务器密码即可登录。

目前由于尚未获得实际开发板/服务器的登录地址和账号，因此暂未进行真实远程登录验证。

### 5.4 scp 文件传输

使用 scp 可以在本机和远程服务器之间传输文件。

本地文件上传到服务器：
scp 文件 用户名@IP地址:远程路径
例如：
scp test.txt user@192.168.1.100:~/test/

从服务器下载文件：
scp 用户名@IP地址:远程文件 本地路径
例如：
scp user@192.168.1.100:~/test/test.txt ./

### 5.5 rsync 文件同步

rsync 适合进行目录或文件的同步，可以减少重复传输。

例如将本地目录同步到远程服务器：
rsync -av ./test/ 用户名@IP地址:~/test/

也可以使用 SSH 作为传输方式：
rsync -av -e ssh ./test/ 用户名@IP地址:~/test/

### 5.6 SSH Key 免密登录

将本机公钥配置到远程服务器后，可以实现免密码登录。

常用方法：
ssh-copy-id 用户名@IP地址
配置完成后：
ssh 用户名@IP地址
即可尝试使用 SSH Key 登录。

目前已经完成本机 SSH Key 的生成，待获得实际开发板/服务器账号后进行免密登录验证。

### 5.7 当前完成情况

目前已经完成：

SSH 客户端安装与检查
scp 工具安装与检查
rsync 工具安装与检查
SSH Key 生成
SSH 基本登录命令学习
scp 文件传输命令学习
rsync 文件同步命令学习
SSH Key 免密登录方法学习

## 6. M0-1 环境验收

使用项目提供的 check_env.sh 对开发环境进行统一检查。

最终检查结果：
PASS=29
FAIL=0
WARN=3

最终结果：
环境验收: 通过（WARN 项请现场向考官说明）

3 项 WARN 分别为：

缺少 Remote - WSL 插件
本机使用 VMware Ubuntu 22.04.5 LTS，并非 WSL2 环境，因此不需要使用 Remote - WSL 插件。

known_hosts 当前为空
本机已经完成 SSH 客户端、SSH Key、scp 和 rsync 的配置，但目前尚未获得实际开发板/服务器的登录信息，因此暂未进行真实 SSH 远程连接。

未发现 M0/M1 等模块目录
当前 check_env.sh 检查的是课程提供的 22SmartCar 仓库，而个人作业仓库为 Assessment_One。个人仓库已经建立 M0/M0-1/README.md 目录结构。
以上 WARN 均不影响本次环境验收。

最终：
FAIL=0

因此 M0-1 开发环境验收通过。

## 7.上传文件到GitHub

### 7.1 创建 GitHub 远程仓库

在 GitHub 创建个人仓库：
https://github.com/qjiaxian/Assessment_One

本地项目目录：
~/Assessment_One

### 7.2 添加远程仓库

在本地仓库中执行：
git remote add origin https://github.com/qjiaxian/Assessment_One.git

检查远程仓库：
git remote -v

确认：
origin  https://github.com/qjiaxian/Assessment_One.git (fetch)
origin  https://github.com/qjiaxian/Assessment_One.git (push)

### 7.3 配置 GitHub 网络代理

由于 Ubuntu 虚拟机无法直接访问 GitHub，本机使用 Windows 主机上的 Clash Verge 代理。
Windows VMware VMnet8 地址：
192.168.184.1

Clash Verge 代理端口：
7897

Git 配置代理：
git config --global http.proxy http://192.168.184.1:7897

检查：
git config --global --get http.proxy

### 7.4 推送到 GitHub

执行：
git push -u origin main

GitHub HTTPS 认证时：
Username：GitHub 用户名
Password：GitHub Fine-grained Personal Access Token

注意：GitHub 已不支持使用普通账户密码进行 Git HTTPS 操作。

本次推送成功：
To https://github.com/qjiaxian/Assessment_One.git
 * [new branch]      main -> main
分支 'main' 设置为跟踪来自 'origin/main' 的远程分支 'main'。

### 7.5 检查本地与 GitHub 是否同步

执行：
git status

最终结果：
位于分支 main
您的分支与上游分支 'origin/main' 一致。
无文件要提交，干净的工作区

### 7.6 本机踩坑实录

#### 7.6.1 Ubuntu 无法直接访问 GitHub

原始报错：
执行：
git push -u origin main
出现：
Failed to connect to github.com port 443
使用 curl 测试：
curl -I https://github.com
同样无法连接。

排查过程：
Windows 主机使用 Clash Verge，代理端口为：
7897
开始时 Clash 只监听：
127.0.0.1:7897
Ubuntu 虚拟机无法直接访问 Windows 的 localhost 代理。
在 Clash Verge 中开启：
局域网连接
然后确认 VMware VMnet8 地址：
192.168.184.1
Ubuntu 使用：
curl -I -x http://192.168.184.1:7897 https://github.com
最终得到：
HTTP/2 200
server: github.com
说明代理连接恢复正常。
Git 代理配置
git config --global http.proxy http://192.168.184.1:7897
随后 Git 可以正常访问 GitHub。

#### 7.6.2 GitHub HTTPS 认证失败

第一次执行：
git push -u origin main
使用 GitHub 登录密码进行认证，出现：
Password authentication is not supported for Git operations.
原因：
GitHub 的 HTTPS Git 操作不再使用普通账户密码进行认证。
解决方法：
创建 GitHub Fine-grained Personal Access Token，并限制：
Repository access：Only select repositories
Repository：qjiaxian/Assessment_One
Contents：Read and write
Metadata：Read-only
使用 Token 代替 GitHub 登录密码完成认证。

最终成功：
[new branch] main -> main

## 8. 当前完成情况

截至目前，M0-1 已完成以下内容：

- [x] Ubuntu 22.04.5 LTS 开发环境搭建
- [x] ROS 2 Humble 安装与配置
- [x] Python 3.10 环境配置
- [x] Python 虚拟环境 `.venv` 创建
- [x] uv 安装与使用
- [x] gcc / g++ / make / cmake 安装与检查
- [x] VS Code 安装及 Python、C/C++、Remote-SSH 等插件配置
- [x] Git 安装及用户名、邮箱配置
- [x] Git 本地仓库创建
- [x] `M0/M0-1/README.md` 创建与编写
- [x] Git 分阶段提交，已完成多次 commit
- [x] GitHub 个人仓库 `Assessment_One` 创建
- [x] 本地仓库连接 GitHub 远程仓库
- [x] GitHub 网络代理配置
- [x] GitHub Fine-grained Personal Access Token 配置
- [x] 项目成功推送至 GitHub
- [x] 本地 `main` 分支与 GitHub `origin/main` 同步
- [x] SSH、scp、rsync 工具安装与检查
- [x] SSH Ed25519 密钥生成
- [x] M0-1 环境检查

当前验收结果：

PASS=29
FAIL=0
WARN=3

## 9. 尚未完成
使用实际开发板或服务器进行 SSH 远程登录验证
 使用实际设备验证 scp 文件传输
 使用实际设备验证 rsync 文件同步
 使用实际设备验证 SSH Key 免密登录
 根据实际远程操作结果继续补充 README
 完成 M0-2、M0-3、M0-4 等后续任务
 