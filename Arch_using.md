## ArchLinux安装

整体遵循**[Arch Linux 安装指引 - Y7n05h](https://mail.qq.com/cgi-bin/readtemplate?t=safety&check=false&gourl=https%3A%2F%2Fblog.y7n05h.dev%2FArchLinuxInstallationGuide%2F&subtemplate=gray&evil=0)**(国内用户可能无法访问)。
辅以[[Arch Linux 安装使用教程 - ArchTutorial - Arch Linux Studio](https://archlinuxstudio.github.io/ArchLinuxTutorial/#/)(国内用户可能无法访问)。

## ArchLinux使用

[Arch Linux 安装使用教程 - ArchTutorial - Arch Linux Studio](https://archlinuxstudio.github.io/ArchLinuxTutorial/#/?id=arch-linux-安装使用教程-archtutorial-arch-linux-studio)是一个非常详尽的Archlinux安装和使用教程，且简单易懂，同时提供了很多优秀文章的链接。阅读本文，收获颇丰。

## 常用命令

##Pacman包管理：

```bash
sudo pacman -Syu                # 升级系统
sudo pacman -S package_name     # 安装软件包
sudo pacman -Syu package_name   # 升级系统并安装软件包，Arch Linux 不支持部分升级，建议用此命令先升级再安装
sudo pacman -Syyu               # 升级系统 yy标记强制刷新 u标记升级动作
sudo pacman -Ss package_name    # 搜索包含相关内容的软件包
sudo pacman -R package_name     # 删除软件包
pacman -Qi package_name         # 查看软件包信息            
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

## 换源-阿里云

编辑文件`/etc/pacman.d/mirrorlist`：

```bash
sudo vim /etc/pacman.d/mirrorlist
```
在镜像源列表最顶端添加：
```bash
Server = http://mirrors.aliyun.com/archlinux/$repo/os/$arch
```
>参考：[阿里云开发者社区> 镜像站> archlinuxcn](https://developer.aliyun.com/mirror/archlinuxcn?spm=a2c6h.13651102.0.0.3e221b11Nc8UpY)
>
```bash
sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring
```
## 安装yay

```bash
sudo pacman -S yay
#安装完成后再次更新
yay -Syyu && yay -Sys
```
## 常见问题及其解决

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
    根据[Linux 安装使用教程](https://archlinuxstudio.github.io/ArchLinuxTutorial/#/)中**显卡驱动**所描述进行即可。
  
+ VSCode无法唤出外部终端：
  + 解决：
    `.vscode`文件中设置启用外部终端，在`settings.json`文件中加入"terminal.external.linuxExec": "/usr/bin/konsole", ""内指要调用的终端bin/konsole。
  
+ 在升级系统（syu）时，出现以下内容：

  ```bash
  错误：python-markdown: 来自 "Caleb Maclennan <alerque@archlinux.org>" 的签名是勉强信任的
  :: 文件 /var/cache/pacman/pkg/python-markdown-3.3.6-1-any.pkg.tar.zst 已损坏 (无效或已损坏的软件包 (PGP 签名)).
  打算删除吗？ [Y/n] 
  错误：trash-cli: 来自 "Alexander Epaneshnikov <alex19ep@archlinux.org>" 的签名是勉强信任的
  :: 文件 /var/cache/pacman/pkg/trash-cli-0.21.10.24-1-any.pkg.tar.zst 已损坏 (无效或已损坏的软件包 (PGP 签名)).
  打算删除吗？ [Y/n] 
  错误：无法提交处理 (无效或已损坏的软件包)
  发生错误，没有软件包被更新。
  ```

  + 解决：

    终端输入：`sudo pacman-key --init && sudo pacman-key --populate && sudo pacman -Syyu`

    可能的方法：`sudo pacman -S archlinux-keyring`

## 使用Docker

1. 安装：
    ```bash
    sudo pacman -S docker # 安装Docker -Ss搜索Docker软件包
    sudo systemctl enable docker.service # 开启Docker开机自启动服务
    sudo systemctl start docker.service  # 启动Docker服务
    # 安装好docker后自动建立了docker组，不需要自己添加docker组，只需要把当前工作用户加入docker组即可
    sudo gpasswd -a $USER docker # 把工作用户加入Docker组，避免使用root账号工作
    #重启系统生效
    sudo systemctl disable docker.service # 关闭开机自启动服务
    ```

2. 获取镜像：

   ```bash
   docker pull archlinux  # 下载镜像
   docker image ls        # 列出镜像列表
   docker ps -a           # 列出容器列表
   docker run -t -i archlinux /bin/bash # 启动镜像
   docker stop [contaionerID]           # 终止镜像
   docker stop $(docker ps -aq)         # 停止所有容器
   docker rmi $(docker images -q)       # 删除所有镜像
   docker container rm [contaionerID]   # 删除一个处于终止状态的容器
   ```

3. 配置镜像：

   启动镜像，配置初始开发环境，安装了一些包（可能有些没有用），

   ```bash
   docker run -t -i archlinux /bin/bash # 启动镜像
   # 在docker内输入以下内容
   sed -i '1i Server = http://mirrors.aliyun.com/archlinux/$repo/os/$arch' /etc/pacman.d/mirrorlist \
       && sed -i '1i Server = https://mirrors.tencent.com/archlinux/$repo/os/$arch' /etc/pacman.d/mirrorlist \
       && sed -i '$i [archlinuxcn]' /etc/pacman.conf \
       && sed -i '$i SigLevel = TrustAll' /etc/pacman.conf \
       && sed -i '$i Server = https://repo.archlinuxcn.org/$arch' /etc/pacman.conf \
       && sed -i -r 's/^NoExtract\s*=\s*.*/# \0/g' /etc/pacman.conf \
       && pacman -Syyu --noconfirm \
       && pacman -Sy --noconfirm archlinuxcn-keyring && pacman -Su --noconfirm\
       && pacman -Syy --noconfirm git vim neovim zsh oh-my-zsh-git jdk-openjdk jdk8-openjdk jdk11-openjdk \
       maven yay zsh python3 go nodejs npm yarn tmux python2 zsh-autosuggestions zsh-syntax-highlighting \
       zsh-theme-powerlevel10k ranger python-pip python-neovim wl-clipboard fzf ripgrep man-db \
       gcc clang base-devel wqy-zenhei noto-fonts-cjk wget unzip thefuck \
       && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
       && pacman -Scc --noconfirm \
       && rm -rf /var/lib/pacman/sync/* /var/cache/pacman/pkg/* \
       && echo "" > /var/log/pacman.log
   ```

   精简版本：

   ```bash
   sed -i '1i Server = http://mirrors.aliyun.com/archlinux/$repo/os/$arch' /etc/pacman.d/mirrorlist \
       && sed -i '1i Server = https://mirrors.tencent.com/archlinux/$repo/os/$arch' /etc/pacman.d/mirrorlist \
       && sed -i '$i [archlinuxcn]' /etc/pacman.conf \
       && sed -i '$i SigLevel = TrustAll' /etc/pacman.conf \
       && sed -i '$i Server = https://repo.archlinuxcn.org/$arch' /etc/pacman.conf \
       && sed -i -r 's/^NoExtract\s*=\s*.*/# \0/g' /etc/pacman.conf \
       && pacman -Syyu --noconfirm \
       && pacman -Sy --noconfirm archlinuxcn-keyring && pacman -Su --noconfirm\
       && pacman -Syy --noconfirm git vim neovim \
       maven yay go npm yarn tmux \
       ranger python-pip python-neovim wl-clipboard fzf ripgrep man-db \
       gcc clang base-devel wqy-zenhei noto-fonts-cjk wget unzip thefuck \
       && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
       && pacman -Scc --noconfirm \
       && rm -rf /var/lib/pacman/sync/* /var/cache/pacman/pkg/* \
       && echo "" > /var/log/pacman.log
   ```

   

4. 配置vscode：

   下载插件：Remote Development Pack（包含Remote-Containers)，Docker。打开插件，在CONTAINERS中右键Attach Visual Studio Code，在vscode中运行了镜像，重新安装一些扩展，当作一个新的archlinux一样使用。

5. 容器和本地间的文件传输：

   ```bash
   docker ps -a   # 获得容器ID
   docker cp 本地文件路径 ID全称:容器路径 # 本地文件复制到容器
   docker cp ID全称:容器文件路径 本地路径 # 容器文件复制到本地
   ```


## Typora+PicGo+Github图床

整体遵循：https://juejin.cn/post/6844904137407086600#heading-8

1. 创建GIthub仓库并创建Token并复制
2. 下载PicGo（app）并配置，包括时间戳命名，选择github图床，仓库名，分支用main，设定Token
3. 下载并配置node.js
4. 配置Typora并测试图床

### 编译安装从 GitHub 下载的源码

1. 执行以下命令，生成 Makefile 文件：

    ``` ./autogen.sh ``` 

2. 如果下载的源代码中已经包含了 Makefile 文件，则可以跳过此步骤。

3. 执行以下命令，配置编译选项：

   ``` ./configure ``` 

   configure 脚本会检查系统环境和依赖库，并生成 Makefile 文件。可以通过指定不同的选项来定制编译过程。

   例如，可以使用 --prefix 选项指定安装目录，使用 --enable-shared 选项生成共享库等。

4. 执行以下命令，开始编译： 

    ``` make ``` 

    这个命令会编译源代码，并生成可执行文件或共享库等。 

5. 执行以下命令，安装编译好的文件：

6.  ``` sudo make install ``` 

    这个命令会将编译好的文件安装到系统中，通常是 /usr/local 目录。 如果在执行 configure 或 make 命令时遇到了错误，可以根据错误信息进行调整。通常情况下，错误信息会提示缺少依赖库或者系统环境不兼容等问题。
