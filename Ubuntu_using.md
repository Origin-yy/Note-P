### 一、删除Ubuntu系统分区
1. `win+R`输入`diskpart`进入命令行的diskpart。
2. `list disk`，展示磁盘。
3. `select disk 0`选中装有Ubuntu系统的磁盘。
4. `list partition`查看分区。
> 类型为**未知**的为非Windows的分区，要删除它们，类型为**保留**、**主要**、**恢复**的为Windows使用的分区，类型为**系统**的为EFI系统分区，用于启动系统。
5. `select partition num`选中要删除的分区（num为要删除的分区标号）。
6. `delete partition override`删除步骤5中所选分区。
7. 重复步骤5-6删除所有类型为未知的分区。
### 二、删除Ubuntu系统启动项
1. 使用第一部分的1-4步，查看磁盘分区。
2. `select partition num`选中类型为**系统**的分区（num为分区标号）。
3. `assign letter = p`分配盘符。此时**此电脑**中会多一个p盘。
4. 以管理员身份运行记事本，选择**文件**，打开**p盘**，进入**EFI**文件夹，选中**Ubuntu**文件夹，右键删除它。
5. 最后在终端输入`remove letter=p`删除p盘。

==至此，你的Ubuntu系统已经彻底删除。==
### 三、安装Ubuntu
#### 制作Ubuntu启动盘。
1. 准备一个大于4G的U盘，删除里面的所有文件用作启动盘。
2. Ubuntu官网下载你要安装的Ubuntu版本的镜像文件，这里以Ubuntu 22.04 LTS为例。
3. UltralSO官网下载安装UltralSO，点击试用即可，打开后在窗口左下角的**本地目录**打开存有Ubuntu镜像文件的目录，在右边选中该镜像文件。
4. 工具栏里点击**启动**，点击**写入硬盘镜像**。硬盘驱动器选择你的U盘，写入方式为USB-HDD+，点击写入。等待写入完成。
#### 安装Ubuntu
1. 关机重启，进入BIOS（自行搜索），找到USB启动，打开它，并将USB启动下班给放在最上面，保存并退出。（不同机型界面不同）。
2. 充启，进入选择界面，选择U盘打开（第一个），进入Ubuntu安装界面
