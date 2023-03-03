### 1. I copied the code given by the wiki to install.sh and ran it. The following is the content of my terminal running window:

```bash
> cat install.sh
#!/bin/bash 
cd ~/
echo "Installing dependencies..."
sudo pacman -Sy wget git gcc gdb make pkg-config protobuf protobuf-c zlib glew glm libpng glu mesa openal libogg alure mpg123 fluidsynth libvorbis vorbis-tools box2d dumb sdl2 freetype2 libffi libx11 libxrandr libxinerama jre-openjdk jdk-openjdk pkg-config rapidjson yaml-cpp boost grpc pugixml zenity kdialog
echo "Downloading Enigma..."
git clone git https://github.com/enigma-dev/enigma-dev.git
cd enigma-dev
echo "Downloading easy startup script..."
wget https://pastebin.com/raw/aBAU4j3C -O start.sh
sed -i -e 's/\r$//' start.sh
echo "Correcting permissions..."
chmod +x start.sh
chmod +x install.sh
echo "Installing..."
./install.sh
echo "Rebuilding compiler..."
make clean
make
echo "Done, to start Enigma just run ~/enigma-dev/start.sh"%                                    > ./install.sh
Installing dependencies...
[sudo] password for origin::: Synchronizing package database...
  core is already the latest version
  extra is already the latest version
  community is already the latest version
  multilib is already the latest version
  archlinuxcn is already the latest version
  WARNING: wget-1.21.3-1 is already up to date -- reinstall
  WARNING: git-2.39.2-1 is already up to date -- reinstall
  WARNING: gcc-12.2.1-2 is already up to date -- reinstall
  WARNING: gdb-13.1-1 is already up to date -- reinstall
  WARNING: make-4.4.1-1 is already up to date -- reinstall
  WARNING: pkgconf-1.8.0-1 is already up to date -- reinstall
  WARNING: protobuf-21.12-1 is already up to date -- reinstall
  WARNING: protobuf-c-1.4.1-1 is already up to date -- reinstall
  WARNING: zlib-1:1.2.13-2 is already up to date -- reinstall
  WARNING: glew-2.2.0-6 is already up to date -- reinstall
  WARNING: glm-0.9.9.8-1 is already up to date -- reinstall
  WARNING: libpng-1.6.39-1 is already up to date -- reinstall
  WARNING: glu-9.0.2-3 is already up to date -- reinstall
  WARNING: mesa-22.3.6-1 is already up to date -- reinstall
  WARNING: openal-1.23.0-1 is already up to date -- reinstall
  WARNING: libogg-1.3.5-1 is already up to date -- reinstall
  WARNING: alure-1.2-9 is already up to date -- reinstall
  WARNING: mpg123-1.31.2-2 is already up to date -- reinstall
  WARNING: fluidsynth-2.3.1-2 is already up to date -- reinstall
  WARNING: libvorbis-1.3.7-3 is already up to date -- reinstall
  WARNING: vorbis-tools-1.4.2-3 is already up to date -- reinstall
  WARNING: box2d-2.4.1-1 is already up to date -- reinstall
  WARNING: dumb-2.0.3-3 is already up to date -- reinstall
  WARNING: sdl2-2.26.3-1 is already up to date -- reinstall
  WARNING: freetype2-2.13.0-1 is already up to date -- reinstall
  WARNING: libffi-3.4.4-1 is already up to date -- reinstall
  WARNING: libx11-1.8.4-1 is already up to date -- reinstall
  WARNING: libxrandr-1.5.3-1 is already up to date -- reinstall
  WARNING: libxinerama-1.1.5-1 is already up to date -- reinstall
  WARNING: jre-openjdk-19.0.2.u7-2 is already up to date -- reinstall
  WARNING: jdk-openjdk-19.0.2.u7-2 is already up to date -- reinstall
  WARNING: rapidjson-1.1.0-5 is already up to date -- reinstall
  WARNING: yaml-cpp-0.7.0-2 is already up to date -- reinstall
  WARNING: boost-1.81.0-3 is already up to date -- reinstall
  WARNING: grpc-1.51.1-4 is already up to date -- reinstall
  WARNING: pugixml-1.13-1 is already up to date -- reinstall
  WARNING: zenity-3.44.0-2 is already up to date -- reinstall
  WARNING: kdialog-22.12.3-1 is already up to date -- reinstalling is resolving dependencies...
Looking for package conflicts...

package (38) alure-1.2-9  boost-1.81.0-3  box2d-2.4.1-1  dumb-2.0.3-3  fluidsynth-2.3.1-2
            freetype2-2.13.0-1  gcc-12.2.1-2  gdb-13.1-1  git-2.39.2-1  glew-2.2.0-6
            glm-0.9.9.8-1  glu-9.0.2-3  grpc-1.51.1-4  jdk-openjdk-19.0.2.u7-2
            jre-openjdk-19.0.2.u7-2  kdialog-22.12.3-1  libffi-3.4.4-1  libogg-1.3.5-1
            libpng-1.6.39-1  libvorbis-1.3.7-3  libx11-1.8.4-1  libxinerama-1.1.5-1
            libxrandr-1.5.3-1  make-4.4.1-1  mesa-22.3.6-1  mpg123-1.31.2-2  openal-1.23.0-1
            pkgconf-1.8.0-1  protobuf-21.12-1  protobuf-c-1.4.1-1  pugixml-1.13-1
            rapidjson-1.1.0-5  sdl2-2.26.3-1  vorbis-tools-1.4.2-3  wget-1.21.3-1
            yaml-cpp-0.7.0-2  zenity-3.44.0-2  zlib-1:1.2.13-2

Total installation size: 851.43 MiB
Net update size: 0.00 MiB

:: Do you want to install? [Y/n]
(38/38) Checking keys in keyring [#################################] 100%
(38/38) Checking package integrity [################################] 100%
(38/38) Loading package file [################################] 100%
(38/38) Checking for file conflicts [################################] 100%
(38/38) Checking available storage space [################################] 100%
:: Processing package changes...
( 1/38) Reinstalling zlib [#################################] 100%
(2/38) Reinstalling libffi [#################################] 100%
( 3/38) Reinstalling wget [################################] 100%
(4/38) Reinstalling git [################################] 100%
(5/38) Reinstalling gcc [################################] 100%
(6/38) Reinstalling gdb [################################] 100%
( 7/38) Reinstalling make [################################] 100%
(8/38) Reinstalling pkgconf [#################################] 100%
(9/38) Reinstalling protobuf [#################################] 100%
(10/38) Reinstalling protobuf-c [#################################] 100%
(11/38) Reinstalling libx11 [#################################] 100%
(12/38) Reinstalling mesa [#################################] 100%
(13/38) Reinstalling libpng [################################] 100%
(14/38) Reinstalling freetype2 [#################################] 100%
(15/38) Reinstalling libxrandr [#################################] 100%
(16/38) Reinstalling glu [################################] 100%
(17/38) reinstalling glew [################################] 100%
(18/38) Reinstalling glm [################################] 100%
(19/38) Reinstalling openal [################################] 100%
(20/38) Reinstalling libogg [#################################] 100%
(21/38) reinstalling alure [################################] 100%
(22/38) Reinstalling mpg123 [#################################] 100%
(23/38) Reinstalling sdl2 [#################################] 100%
(24/38) Reinstalling libvorbis [#################################] 100%
(25/38) Reinstalling fluidsynth [#################################] 100%
(26/38) Reinstalling vorbis-tools [#################################] 100%
(27/38) Reinstalling box2d [#################################] 100%
(28/38) Reinstalling dumb [################################] 100%
(29/38) Reinstalling libxinerama [#################################] 100%
(30/38) Reinstalling jre-openjdk [#################################] 100%
(31/38) Reinstalling jdk-openjdk [#################################] 100%
(32/38) Reinstalling rapidjson [#################################] 100%
(33/38) Reinstalling yaml-cpp [#################################] 100%
(34/38) Reinstalling boost [################################] 100%
(35/38) Reinstalling grpc [#################################] 100%
(36/38) Reinstalling pugixml [#################################] 100%
(37/38) Reinstalling zenity [#################################] 100%
(38/38) Reinstalling kdialog [#################################] 100%
:: running post-transaction hook function...
(1/7) Creating system user accounts...
(2/7) Reloading system manager configuration...
(3/7) Arming ConditionNeedsUpdate...
(4/7) Refreshing PackageKit...
(5/7) Updating icon theme caches...
(6/7) Updating the info directory file...
(7/7) Updating the desktop file MIME type cache...
Downloading Enigma...
Fatal error: repository 'git' does not exist Downloading easy startup script...
--2023-03-03 10:51:42-- https://pastebin.com/raw/aBAU4j3C
Loaded CA certificate "/etc/ssl/certs/ca-certificates.crt"
Resolving host pastebin.com (pastebin.com)... 172.67.34.170, 104.20.68.143, 104.20.67.143, ...
Connecting pastebin.com (pastebin.com)|172.67.34.170|:443... connected. HTTP request sent, waiting for response... 200 OK
Length: unspecified [text/plain]
Saving to: "start.sh"

start.sh                    [ <=>                            ]      93  --.-KB/s  用时 0s      

2023-03-03 10:51:43 (1.10 MB/s) - “start.sh” 已保存 [93]

Correcting permissions...
chmod: cannot access 'install.sh': No such file or directory Installing...
./install.sh: line 15: ./install.sh: No such file or directory Rebuilding compiler...
make: *** No rule to make target 'clean'. stop. make: *** No target specified and makefile not found. stop. Done, to start Enigma just run ~/enigma-dev/start.sh

~/Code                                                                  took 1m 22s at 10:51:43
> 

```

### 2. Use the command directly in the terminal:

```bash
curl https://gist.githubusercontent.com/HitCoder9768/1ba09a6ebc096aa863af9c4f1ac553cb/raw/16b5f482793c5a3c55716fc381aa6af784f114c0/arch_install.sh | bash
```

The terminal window looks like this:

```bash
> curl https://gist.githubusercontent.com/HitCoder9768/1ba09a6ebc096aa863af9c4f1ac553cb/raw/16b5f482793c5a3c55716fc381aa6af784f114c0/arch_install.sh | bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   888  100   888    0     0    196      0  0:00:04  0:00:04 --:--:--   196
Installing dependencies...
[sudo] password for origin::: Synchronizing package database...
Error: Failed to sync all databases (unable to lock database) Downloading Enigma...
Cloning into 'enigma-dev'...
Fatal error: Failed to connect to github.com: github.com[0: 20.205.243.166]: errno=Connection timed out
bash: line 9: cd: enigma-dev: No such file or directory Downloading easy startup script...
--2023-03-03 10:52:57-- https://pastebin.com/raw/aBAU4j3C
Loaded CA certificate "/etc/ssl/certs/ca-certificates.crt"
Resolving host pastebin.com (pastebin.com)... 104.20.67.143, 104.20.68.143, 172.67.34.170, ...
Connecting pastebin.com (pastebin.com)|104.20.67.143|:443... connected. HTTP request sent, waiting for response... 200 OK
Length: unspecified [text/plain]
Saving to: "start.sh"

start.sh [ <=> ] 93 --.-KB/s takes 0s

2023-03-03 10:52:58 (1.05 MB/s) - "start.sh" saved [93]

Correcting permissions...
chmod: cannot access 'install.sh': No such file or directory Installing...
bash: line 17: ./install.sh: No such file or directory Rebuilding compiler...
make: *** No rule to make target 'clean'. stop. make: *** No target specified and makefile not found. stop. Done, to start Enigma just run ~/enigma-dev/start.sh

~                                                                       took 3m 40s at 10:52:58
> 

```

### 3. by yay

```bash
> yay -S enigma
[sudo] password for origin: WARNING: enigma-1.04-7 is already up-to-date -- reinstalling resolving dependencies...
Looking for package conflicts...

Package (1) enigma-1.04-7

Total installation size: 0.06 MiB
Net update size: 0.00 MiB

:: Do you want to install? [Y/n]
(1/1) Checking keys in keyring [################################] 100%
(1/1) Checking package integrity [#################################] 100%
(1/1) Loading package file [#################################] 100%
(1/1) Checking for file conflicts [#################################] 100%
(1/1) Checking available storage space [#################################] 100%
:: Processing package changes...
(1/1) Reinstalling enigma [################################] 100%
:: running post-transaction hook function...
(1/2) Arming Condition Needs Update...
(2/2) Refreshing PackageKit...
> yay -S enigma
WARNING: enigma-1.04-7 is already up-to-date -- reinstalling is resolving dependencies...
Looking for package conflicts...

Package (1) enigma-1.04-7

Total installation size: 0.06 MiB
Net update size: 0.00 MiB

:: Do you want to install? [Y/n]
(1/1) Checking keys in keyring [################################] 100%
(1/1) Checking package integrity [#################################] 100%
(1/1) Loading package file [#################################] 100%
(1/1) Checking for file conflicts [#################################] 100%
(1/1) Checking available storage space [#################################] 100%
:: Processing package changes...
(1/1) Reinstalling enigma [################################] 100%
:: running post-transaction hook function...
(1/2) Arming Condition Needs Update...
(2/2) Refreshing PackageKit...
> yay -S enigma-dev-git
:: Checking for conflicts...
:: Checking for internal conflicts...
[Aur:1] enigma-dev-git-4771.c6d773671-1

   1 enigma-dev-git (build files already exist)
==> Clean builds of which packages? ==> [N] None [A] All [Ab] Aborted [I] Installed [No] Not Installed or (1 2 3, 1-3, ^4)
==>
:: PKGBUILD is up to date, skipped (1/0): enigma-dev-git
   1 enigma-dev-git (build files already exist)
==> What differences are shown? ==> [N] None [A] All [Ab] Aborted [I] Installed [No] Not Installed or (1 2 3, 1-3, ^4)
==>
:: (1/1) Parsing SRCINFO: enigma-dev-git
==> Creating package: enigma-dev-git 4771.c6d773671-1 (Fri Mar 03 10:48:58 2023)
==> Get source code...
   -> find enigma
   -> find emake
   -> find enigma-dev.desktop
   -> Upgrading enigma-dev git repository...
Fatal error: Unable to access 'https://github.com/enigma-dev/enigma-dev.git/': HTTP/2 stream 1 was not closed cleanly before end of the underlying stream
==> WARNING: Failed to upgrade enigma-dev git repository -> logo.png found
==> Verifying source file using sha256sums...
     enigma ... via emake ... via enigma-dev.desktop ... via enigma-dev ... logo.png ... via ==> Creating package: enigma-dev-git 4771. c6d773671-1 (Friday, March 03, 2023 10:50:59)
==> Checking runtime dependencies...
==> Checking compile-time dependencies ==> Getting source code...
  -> 找到 enigma
  -> 找到 emake
  -> 找到 enigma-dev.desktop
  -> 正在升级 enigma-dev git 仓库...
致命错误：无法访问 'https://github.com/enigma-dev/enigma-dev.git/'：Failed to connect to github.com port 443 after 129977 ms: Couldn't connect to server
==> 警告： 升级 enigma-dev git 仓库失败  -> 找到 logo.png
==> 正在验证 source 文件，使用sha256sums...
    enigma ... 通过    emake ... 通过    enigma-dev.desktop ... 通过    enigma-dev ... 已跳过    logo.png ... 通过==> 正在删除现存的 $srcdir/ 目录...
==> 正在释放源码...
  -> 正在建立 enigma-dev git 仓库的拷贝...
正克隆到 'enigma-dev'...
完成。==> 正在开始 pkgver()...
==> 已升级版本：enigma-dev-git 4772.ab1e99c9a-1
==> 源代码已就绪。==> 正在创建软件包：enigma-dev-git 4772.ab1e99c9a-1 (2023年03月03日 星期五 10时53分10秒)
==> 正在检查运行时依赖关系...
==> 正在检查编译时依赖关系==> 警告： 使用现存的 $srcdir/ 树==> 正在开始 pkgver()...
==> 正在删除现存的 $pkgdir/ 目录...
==> 正在开始 build()...
==> Installing LateralGM...
Attempting to download jna.jar to /home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/plugins/shared/jna.jar from https://enigma-dev.org/bin/jna.jar 
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 1689k  100 1689k    0     0   128k      0  0:00:13  0:00:13 --:--:--  217k
Attempting to download enigma.jar to /home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/plugins/enigma.jar from https://github.com/enigma-dev/lgmplugin/releases/download//enigma.jar 
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100     9  100     9    0     0     14      0 --:--:-- --:--:-- --:--:--    14
Attempting to download lateralgm.jar to /home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/lateralgm.jar from https://github.com/IsmAvatar/LateralGM/releases/download/v1.8.234/lateralgm.jar 
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:--  0:02:10 --:--:--     0
curl: (28) Failed to connect to github.com port 443 after 130176 ms: Couldn't connect to server
==> Compiling Enigma...
make -C CompilerSource/clean
make[1]: Enter the directory "/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/CompilerSource"
rm -rf ../libcompileEGMf.so .eobjs
make[1]: Leaving directory "/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/CompilerSource"
make -C CommandLine/emake/clean
make[1]: Enter the directory "/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/CommandLine/emake"
rm -rf ../../emake .eobjs
make[1]: Leaving directory "/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/CommandLine/emake"
make -C CommandLine/libEGM/clean
make[1]: Enter the directory "/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/CommandLine/libEGM"
rm -rf ../../libEGM.so .eobjs
make[1]: Leaving directory "/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/CommandLine/libEGM"
make -C CommandLine/testing/clean
make[1]: Enter the directory "/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/CommandLine/testing"
rm -rf ../../test-runner.eobjs
make[1]: Leaving directory "/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/CommandLine/testing"
make -C shared/clean
make[1]: Enter the directory "/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/shared"
rm -rf ../libENIGMAShared.so .eobjs
make[1]: Leaving directory "/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/shared"
make -C shared/protos/clean
make[1]: Enter the directory "/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/shared/protos"
rm -rf ../../libProtocols.so.eobjs
make[1]: Leaving directory "/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/shared/protos"
make -C CommandLine/gm2egm/clean
make[1]: Enter the directory "/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/CommandLine/gm2egm"
rm -rf ../../gm2egm.eobjs
make[1]: Leaving directory "/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/CommandLine/gm2egm"
make -C shared/protos/
make[1]: Enter the directory "/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/shared/protos"
mkdir -p .eobjs
protoc -I. --cpp_out=.eobjs Action.proto
protoc -I. --cpp_out=.eobjs Background.proto
protoc -I. --cpp_out=.eobjs compiler.proto
protoc -I. --cpp_out=.eobjs EventDescriptor.proto
protoc -I. --cpp_out=.eobjs Font.proto
protoc -I. --cpp_out=.eobjs GameInformation.proto
GameInformation.proto:4:1: warning: Import options.proto is unused.
protoc -I. --cpp_out=.eobjs game.proto
protoc -I. --cpp_out=.eobjs Include.proto
protoc -I. --cpp_out=.eobjs Object.proto
protoc -I. --cpp_out=.eobjs options.proto
protoc -I. --cpp_out=.eobjs Path.proto
protoc -I. --cpp_out=.eobjs project.proto
protoc -I. --cpp_out=.eobjs Room.proto
protoc -I. --cpp_out=.eobjs Script.proto
Script.proto:4:1: warning: Import options.proto is unused.
protoc -I. --cpp_out=.eobjs server.proto
protoc -I. --cpp_out=.eobjs Settings.proto
protoc -I. --cpp_out=.eobjs Shader.proto
Shader.proto:4:1: warning: Import options.proto is unused.
protoc -I. --cpp_out=.eobjs Sound.proto
protoc -I. --cpp_out=.eobjs Sprite.proto
protoc -I. --cpp_out=.eobjs Timeline.proto
protoc -I. --cpp_out=.eobjs treenode.proto
protoc -I. --grpc_out=.eobjs --plugin=protoc-gen-grpc=/usr/bin/grpc_cpp_plugin server.proto
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/Action.pb.o .eobjs/Action.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/Background.pb.o .eobjs/Background.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/compiler.pb.o .eobjs/compiler.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/EventDescriptor.pb.o .eobjs/EventDescriptor.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/Font.pb.o .eobjs/Font.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/GameInformation.pb.o .eobjs/GameInformation.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/game.pb.o .eobjs/game.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/Include.pb.o .eobjs/Include.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/Object.pb.o .eobjs/Object.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/options.pb.o .eobjs/options.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/Path.pb.o .eobjs/Path.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/project.pb.o .eobjs/project.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/Room.pb.o .eobjs/Room.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/Script.pb.o .eobjs/Script.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/server.pb.o .eobjs/server.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/Settings.pb.o .eobjs/Settings.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/Shader.pb.o .eobjs/Shader.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/Sound.pb.o .eobjs/Sound.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/Sprite.pb.o .eobjs/Sprite.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/Timeline.pb.o .eobjs/Timeline.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/treenode.pb.o .eobjs/treenode.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -MMD -MP -c -o .eobjs/server.grpc.pb.o .eobjs/server.grpc.pb.cc
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -D_WIN32_WINNT=0x0600 -I.eobjs -fPIC -o ../../libProtocols.so .eobjs/Action.pb.o .eobjs/Background.pb.o .eobjs/compiler.pb.o .eobjs/EventDescriptor.pb.o .eobjs/Font.pb.o .eobjs/GameInformation.pb.o .eobjs/game.pb.o .eobjs/Include.pb.o .eobjs/Object.pb.o .eobjs/options.pb.o .eobjs/Path.pb.o .eobjs/project.pb.o .eobjs/Room.pb.o .eobjs/Script.pb.o .eobjs/server.pb.o .eobjs/Settings.pb.o .eobjs/Shader.pb.o .eobjs/Sound.pb.o .eobjs/Sprite.pb.o .eobjs/Timeline.pb.o .eobjs/treenode.pb.o .eobjs/server.grpc.pb.o -g -lgrpc++ -shared -lprotobuf
make[1]: Leaving directory "/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/shared/protos"
make -C shared/
make[1]: Enter the directory "/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/shared"
mkdir -p .eobjs
mkdir -p .eobjs/shared/ProtoYaml/
mkdir -p .eobjs/shared/event_reader/
mkdir -p .eobjs/shared/eyaml/
mkdir -p .eobjs/shared/libpng-util/
mkdir -p .eobjs/shared/rectpacker/
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I../CompilerSource -I./protos/.eobjs -I.  -MMD -c -o .eobjs/shared/event_reader/egm_events.o event_reader/egm_events.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I../CompilerSource -I./protos/.eobjs -I.  -MMD -c -o .eobjs/shared/event_reader/event_parser.o event_reader/event_parser.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I../CompilerSource -I./protos/.eobjs -I.  -MMD -c -o .eobjs/shared/eyaml/eyaml.o eyaml/eyaml.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I../CompilerSource -I./protos/.eobjs -I.  -MMD -c -o .eobjs/shared/libpng-util/libpng-util.o libpng-util/libpng-util.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I../CompilerSource -I./protos/.eobjs -I.  -MMD -c -o .eobjs/shared/rectpacker/rectpack.o rectpacker/rectpack.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I../CompilerSource -I./protos/.eobjs -I.  -MMD -c -o .eobjs/shared/ProtoYaml/proto-yaml.o ProtoYaml/proto-yaml.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I../CompilerSource -I./protos/.eobjs -I.  -o ../libENIGMAShared.so .eobjs/shared/event_reader/egm_events.o .eobjs/shared/event_reader/event_parser.o .eobjs/shared/eyaml/eyaml.o .eobjs/shared/libpng-util/libpng-util.o .eobjs/shared/rectpacker/rectpack.o .eobjs/shared/ProtoYaml/proto-yaml.o -g -shared -lpng -lyaml-cpp -L../ -lProtocols -lprotobuf
make[1]: 离开目录“/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/shared”
make -C CompilerSource
make[1]: 进入目录“/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/CompilerSource”
mkdir -p .eobjs
mkdir -p .eobjs/JDI/src/API/
mkdir -p .eobjs/JDI/src/General/
mkdir -p .eobjs/JDI/src/Parser/
mkdir -p .eobjs/JDI/src/Parser/handlers/
mkdir -p .eobjs/JDI/src/Parser/readers/
mkdir -p .eobjs/JDI/src/Storage/
mkdir -p .eobjs/JDI/src/System/
mkdir -p .eobjs/JDI/test/
mkdir -p .eobjs/backend/
mkdir -p .eobjs/compiler/
mkdir -p .eobjs/compiler/components/
mkdir -p .eobjs/gcc_interface/
mkdir -p .eobjs/general/
mkdir -p .eobjs/languages/
mkdir -p .eobjs/parser/
mkdir -p .eobjs/settings-parse/
mkdir -p .eobjs/syntax/
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/frontend.o frontend.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/main.o main.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/settings.o settings.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/backend/GameData.o backend/GameData.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/backend/ideprint.o backend/ideprint.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/backend/JavaCallbacks.o backend/JavaCallbacks.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/compiler/compile_common.o compiler/compile_common.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/compiler/compile.o compiler/compile.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/compiler/jdi_utility.o compiler/jdi_utility.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/compiler/components/module_write_backgrounds.o compiler/components/module_write_backgrounds.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/compiler/components/module_write_fonts.o compiler/components/module_write_fonts.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/compiler/components/module_write_paths.o compiler/components/module_write_paths.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/compiler/components/module_write_sounds.o compiler/components/module_write_sounds.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/compiler/components/module_write_sprites.o compiler/components/module_write_sprites.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/compiler/components/parse_and_link.o compiler/components/parse_and_link.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/compiler/components/parse_secondary.o compiler/components/parse_secondary.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/compiler/components/write_defragged_events.o compiler/components/write_defragged_events.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/compiler/components/write_font_info.o compiler/components/write_font_info.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/compiler/components/write_globals.o compiler/components/write_globals.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/compiler/components/write_object_access.o compiler/components/write_object_access.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/compiler/components/write_object_data.o compiler/components/write_object_data.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/compiler/components/write_room_data.o compiler/components/write_room_data.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/compiler/components/write_shader_data.o compiler/components/write_shader_data.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/gcc_interface/gcc_backend.o gcc_interface/gcc_backend.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/general/bettersystem.o general/bettersystem.cpp
general/bettersystem.cpp: 在函数‘int e_execp(const char*, std::string)’中:
general/bettersystem.cpp:527:32: 警告：ISO C++ does not allow ‘?:’ with omitted middle operand [-Wpedantic]
  527 |       path += getenv("PATH") ? : "";
      |                                ^
general/bettersystem.cpp:527:30: 警告：ISO C++ forbids omitting the middle term of a ‘?:’ expression [-Wpedantic]
  527 |       path += getenv("PATH") ? : "";
      |               ~~~~~~~~~~~~~~~^~~~~~
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/general/darray.o general/darray.cpp
general/darray.cpp: 在函数‘void post_access_watch(unsigned int, unsigned int, unsigned int)’中:
general/darray.cpp:6:56: 警告：unused parameter ‘all’ [-Wunused-parameter]
    6 | void post_access_watch(unsigned i,unsigned sz,unsigned all)
      |                                               ~~~~~~~~~^~~
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/general/macro_integration.o general/macro_integration.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/general/string.o general/string.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/API/AST.o JDI/src/API/AST.cpp
JDI/src/API/AST.cpp: 在成员函数‘jdip::AST_Node* jdip::AST_Builder::parse_expression(jdi::AST*, jdip::token_t&, int)’中:
JDI/src/API/AST.cpp:259:64: 警告：implicitly-declared ‘jdi::full_type& jdi::full_type::operator=(const jdi::full_type&)’ is deprecated [-Wdeprecated-copy]
  259 |         ann->alloc_type = cparse->read_type(token, search_scope);
      |                                                                ^
In file included from ./JDI/src/Storage/references.h:209,
                 from ./JDI/src/Storage/full_type.h:32,
                 from ./JDI/src/Storage/arg_key.h:29,
                 from JDI/src/API/AST.h:35,
                 from JDI/src/API/AST.cpp:23:
./JDI/src/Storage/full_type.h:70:5: 附注：because ‘jdi::full_type’ has user-provided ‘jdi::full_type::full_type(const jdi::full_type&)’
   70 |     full_type(const full_type&); ///< Copy constructor. Makes a copy, so slowish.
      |     ^~~~~~~~~
JDI/src/API/AST.cpp: 在成员函数‘virtual jdi::full_type jdip::AST_Node_Subscript::coerce(const jdi::error_context&) const’中:
JDI/src/API/AST.cpp:1169:31: 警告：implicitly-declared ‘jdi::full_type& jdi::full_type::operator=(const jdi::full_type&)’ is deprecated [-Wdeprecated-copy]
 1169 |       res = index->coerce(errc);
      |                               ^
./JDI/src/Storage/full_type.h:70:5: 附注：because ‘jdi::full_type’ has user-provided ‘jdi::full_type::full_type(const jdi::full_type&)’
   70 |     full_type(const full_type&); ///< Copy constructor. Makes a copy, so slowish.
      |     ^~~~~~~~~
JDI/src/API/AST.cpp: 在构造函数‘jdip::AST_Node_sizeof::AST_Node_sizeof(jdip::AST_Node*, bool)’中:
JDI/src/API/AST.cpp:1236:96: 警告：enum constant in boolean context [-Wint-in-bool-context]
 1236 | AST_Node_sizeof(AST_Node* param, bool n): AST_Node_Unary(param, str_sizeof, AT_SIZEOF), negate(n) {}
      |                                                                             ^~~~~~~~~

JDI/src/API/AST.cpp: 在构造函数‘jdip::AST_Node_Cast::AST_Node_Cast(jdip::AST_Node*, const jdi::full_type&, cast_modes)’中:
JDI/src/API/AST.cpp:1237:121: 警告：enum constant in boolean context [-Wint-in-bool-context]
 1237 | am, const full_type& ft, cast_modes cmode): AST_Node_Unary(param, str_cast, AT_CAST), cast_mode(cmode) { cast_type.copy(ft); }
      |                                                                             ^~~~~~~

JDI/src/API/AST.cpp: 在构造函数‘jdip::AST_Node_Cast::AST_Node_Cast(jdip::AST_Node*, jdi::full_type&, cast_modes)’中:
JDI/src/API/AST.cpp:1238:115: 警告：enum constant in boolean context [-Wint-in-bool-context]
 1238 | e* param, full_type& ft, cast_modes cmode): AST_Node_Unary(param, str_cast, AT_CAST), cast_mode(cmode) { cast_type.swap(ft); }
      |                                                                             ^~~~~~~

JDI/src/API/AST.cpp: 在构造函数‘jdip::AST_Node_Cast::AST_Node_Cast(jdip::AST_Node*)’中:
JDI/src/API/AST.cpp:1239:82: 警告：enum constant in boolean context [-Wint-in-bool-context]
 1239 | _Node_Cast::AST_Node_Cast(AST_Node* param): AST_Node_Unary(param, str_cast, AT_CAST) {}
      |                                                                             ^~~~~~~

JDI/src/API/AST.cpp: 在构造函数‘jdip::AST_Node_delete::AST_Node_delete(jdip::AST_Node*, bool)’中:
JDI/src/API/AST.cpp:1247:114: 警告：enum constant in boolean context [-Wint-in-bool-context]
 1247 | T_Node* param, bool arr): AST_Node_Unary(param, arr? "delete" : "delete[]", AT_DELETE), array(arr) {}
      |                                                                            ^~~~~~~~~

JDI/src/API/AST.cpp: In member function 'jdip::AST_Node* jdip::AST_Builder::parse_binary_or_unary_post(jdi::AST*, jdip::token_t&, jdip::AST_Node*, int)':
JDI/src/API/AST.cpp:490:9: Warning: this statement may fall through [-Wimplicit-fallthrough=]
   490 | }
       | ^
JDI/src/API/AST.cpp:493:7: Note: here
   493 | case TT_OPERATOR: case_TT_OPERATOR: {
       | ^~~~
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/API/AST_Export.o JDI/src/API/AST_Export.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/API/AST_operator.o JDI/src/API/AST_operator.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/API/context.o JDI/src/API/context.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/API/error_reporting.o JDI/src/API/error_reporting.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/API/jdi.o JDI/src/API/jdi.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/API/lexer_interface.o JDI/src/API/lexer_interface.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/API/user_tokens.o JDI/src/API/user_tokens.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/General/debug_macros.o JDI/src/General/debug_macros.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/General/llreader.o JDI/src/General/llreader.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/General/parse_basics.o JDI/src/General/parse_basics.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/General/svg_simple.o JDI/src/General/svg_simple.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/Parser/base.o JDI/src/Parser/base.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/Parser/context_parser.o JDI/src/Parser/context_parser.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/Parser/handlers/handle_class.o JDI/src/Parser/handlers/handle_class.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/Parser/handlers/handle_declarators.o JDI/src/Parser/handlers/handle_declarators.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/Parser/handlers/handle_enum.o JDI/src/Parser/handlers/handle_enum.cpp
JDI/src/Parser/handlers/handle_enum.cpp: in member function‘jdi::definition_enum* jdip::context_parser::handle_enum(jdi::definition_scope*, jdip::token_t&, int)’中:
JDI/src/Parser/handlers/handle_enum.cpp:116:46: warning：implicitly-declared ‘constexpr jdi::value& jdi::value::operator=(const jdi::value&)’ is deprecated [-Wdeprecated-copy]
  116 |         this_value = value(++this_value.val.i);
      |                                              ^
In file included from ./JDI/src/Storage/arg_key.h:30,
                 from ./JDI/src/API/AST.h:35,
                 from JDI/src/Parser/handlers/handle_enum.cpp:23:
./JDI/src/Storage/value.h:67:5: notes：because ‘jdi::value’ has user-provided ‘jdi::value::value(const jdi::value&)’
   67 |     value(const value& v); ///< Copy a value. Handles allocation issues.
      |     ^~~~~
JDI/src/Parser/handlers/handle_enum.cpp:119:22: warning：implicitly-declared ‘constexpr jdi::value& jdi::value::operator=(const jdi::value&)’ is deprecated [-Wdeprecated-copy]
  119 |         this_value = v;
      |                      ^
./JDI/src/Storage/value.h:67:5: notes：because ‘jdi::value’ has user-provided ‘jdi::value::value(const jdi::value&)’
   67 |     value(const value& v); ///< Copy a value. Handles allocation issues.
      |     ^~~~~
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/Parser/handlers/handle_friend.o JDI/src/Parser/handlers/handle_friend.cpp
g++ -std=c++17 -Wall -Wextra -Wpedantic -g -I. -fPIC -I./JDI/src -I../shared -I../shared/libpng-util -I../shared/protos/.eobjs  -I../shared  -MMD -c -o .eobjs/JDI/src/Parser/handlers/handle_function_impl.o JDI/src/Parser/handlers/handle_function_impl.cpp
JDI/src/Parser/handlers/handle_function_impl.cpp:65:66: fatal error: opening dependency file .eobjs/JDI/src/Parser/handlers/handle_function_impl.d: No such file or directory 65 | void (*delete_constructor_initializers)( void* impl) = do_nothing;
       | ^
Compilation interrupted. make[1]: *** [../Default.mk:37:.eobjs/JDI/src/Parser/handlers/handle_function_impl.o] Error 1
make[1]: Leaving directory "/home/origin/.cache/yay/enigma-dev-git/src/enigma-dev/CompilerSource"
make: *** [Makefile:10:ENIGMA] Error 2
==> Error: An error occurred in build(). giving up...
  -> Error generating: enigma-dev-git

~                                                                      took 11m 21s at 10:59:18
> 

```

