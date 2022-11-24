## ArchLinux安装

整体遵循**[Arch Linux 安装指引 - Y7n05h](https://mail.qq.com/cgi-bin/readtemplate?t=safety&check=false&gourl=https%3A%2F%2Fblog.y7n05h.dev%2FArchLinuxInstallationGuide%2F&subtemplate=gray&evil=0)**(国内用户可能无法访问)。
辅以[[Arch Linux 安装使用教程 - ArchTutorial - Arch Linux Studio](https://archlinuxstudio.github.io/ArchLinuxTutorial/#/)(国内用户可能无法访问)。

## ArchLinux使用

[Arch Linux 安装使用教程 - ArchTutorial - Arch Linux Studio](https://archlinuxstudio.github.io/ArchLinuxTutorial/#/?id=arch-linux-安装使用教程-archtutorial-arch-linux-studio)是一个非常详尽的Archlinux安装和使用教程，且简单易懂，同时提供了很多优秀文章的链接。阅读本文，收获颇丰。

## 常用命令

##Pacman包管理：存存储器，系统总线，主存，磁盘。

操作系统管理硬件。进程（处理器，主存，i/o设备），虚拟内存（主存，i/o设备），文件（i/o设备）。

并发运行：一个进程的指令和另一个进程的指令交错执行。这种交错执行的机制叫上下文切换，进程间切换由内核管理。

上下文：操作系统保持跟踪进程运行所需的所有状态信息。

内核：操作系统代码常驻主存的一部分，不是独立的进程。是系统全部进程所用代码和数据结构的集合。

栈位于用户虚拟地址空间顶部，编译器用它来实现函数调用，和堆一样，用户栈在程序执行期间可以动态地扩展和收缩 。 调用函数时，栈会增长；从一个函数返回时，栈会收缩 。

内核虚拟内存：程序无法读写，或调用内核代码定义的函数，需要调用内核来执行操作。

<img title="" src="file:///home/yuanye/图片/2022-10-06%2017-25-26%20的屏幕截图.png" alt="2022-10-06 17-25-26 的屏幕截图.png" width="309" data-align="center">

PC：程序计数器（寄存器），ALU（算数/逻辑单元）

```bash
sudo pacman -Syu                # 升级系统
sudo pacman -S package_name     # 安装软件包
sudo pacman -Syu package_name   # 升级系统并安装软件包，Arch Linux 不支持部分升级，建议用此命令先升级再安装
sudo pacman -Syyu               # 升级系统 yy标记强制刷新 u标记升级动作
sudo pacman -Ss package_name    # 搜索包含相关内容的软件包
sudo pacman -R package_name     # 删除软件包

sudo pacman -Rs package_name    # 删除软件包，及其所有没有被其他已安装软件包使用的依赖包
sudo pacman -Si package_name    # 从数据库中搜索软件包的信息
sudo pacman -Qdt                # 找出孤立包 Q为查询本地软件包数据库 d标记依赖包 t标记不需要的包 dt合并标记孤立包
sudo pacman -Rs $(pacman -Qtdq) # 删除孤立软件包
sudo pacman -U abc.pkg.tar.gz   # 安装下载的abc包，或新编译的本地abc包
sudo pacman -Fy                 # 更新命令查询文件列表数据库
sudo pacman -F xxx              # 当不知道某个命令属于哪个包时，用来查询某个xxx命令属于哪个包
sudo pacman -Sc                 # 清理没有安装的所有缓存包，和没有被使用的同步数据库
yay -S abc                # 安装abc包
yay -Ss abc | grep 已安装  # 搜索已安装且包含abc的包
yay -R 包名                # 删除软件包(不包括前后缀，版本号)
```
## 系统服务的操作与介绍

Linux 系统中运行着各种服务，你需要掌握查询，变更服务状态的方式。同时对创建服务最好也有大致的了解。这里讲述命令`systemctl`的用法。以 dhcpcd 为例

```bash
systemctl start dhcpcd          # 启动服务
systemctl stop dhcpcd           # 停止服务
systemctl restart dhcpcd        # 重启服务
systemctl reload dhcpcd         # 重新加载服务以及它的配置文件
systemctl status dhcpcd         # 查看服务状态
systemctl enable dhcpcd         # 设置开机启动服务
systemctl enable --now dhcpcd   # 设置服务为开机启动并立即启动这个单元:
systemctl disable dhcpcd        # 取消开机自动启动
systemctl daemon-reload dhcpcd  # 重新载入 systemd 配置 扫描新增或变更的服务单元 不会重新加载变更的配置 加载变更的配置用 reload
```

# 换源-阿里云

编辑文件`/etc/pacman.d/mirrorlist`：
```bash
sudo vim /etc/pacman.d/mirrorlist
```
在镜像源列表最顶端添加：
```bash
Server = http://mirrors.aliyun.com/archlinux/$repo/os/$arch
```
>参考：[阿里云开发者社区> 镜像站> arch存存储器，系统总线，主存，磁盘。
>
>操作系统管理硬件。进程（处理器，主存，i/o设备），虚拟内存（主存，i/o设备），文件（i/o设备）。
>
>并发运行：一个进程的指令和另一个进程的指令交错执行。这种交错执行的机制叫上下文切换，进程间切换由内核管理。
>
>上下文：操作系统保持跟踪进程运行所需的所有状态信息。
>
>内核：操作系统代码常驻主存的一部分，不是独立的进程。是系统全部进程所用代码和数据结构的集合。
>
>栈位于用户虚拟地址空间顶部，编译器用它来实现函数调用，和堆一样，用户栈在程序执行期间可以动态地扩展和收缩 。 调用函数时，栈会增长；从一个函数返回时，栈会收缩 。
>
>内核虚拟内存：程序无法读写，或调用内核代码定义的函数，需要调用内核来执行操作。
>
><img title="" src="file:///home/yuanye/图片/2022-10-06%2017-25-26%20的屏幕截图.png" alt="2022-10-06 17-25-26 的屏幕截图.png" width="309" data-align="center">
>
>PC：程序计数器（寄存器），ALU（算数/逻辑单元）linuxcn](https://developer.aliyun.com/mirror/archlinuxcn?spm=a2c6h.13651102.0.0.3e221b11Nc8UpY)
```bash
sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring
```
#### 3、安装yay
```bash
sudo pacman -S yay
#安装完成后再次更新
yay -Syyu && yay -Sys
```
### 常见问题及其解决
+ 使用yay命令时报错：
  ```bash
  搜索 AUR 时出错: response decoding failed: invalid character '<' looking for
  ```
  + 解决：
  ```bash
  yay --aururl "https://aur.archlinux.org" --save
  ```
  
+ Telegram无法登陆：
  + 解决：
  SETTING中添加地址，使用代理，port选择代理所使用端口号。
  
+ 连接显示器无法使用：

  + 解决：
    根据Linux 安装使用教程](https://archlinuxstudio.github.io/ArchLinuxTutorial/#/)中**显卡驱动**所描述进行即可。
  
+ VSCode无法唤出外部终端：
  + 解决：
    .vscode文件中设置启用外部终端，在settings.json文件中加入"terminal.external.linuxExec": "/usr/bin/konsole", ""内指要调用的终端具体位置，这里是/usr/bin/konsole。