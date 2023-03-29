令牌：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

远程仓库指github仓库（repositoris），本地仓库指自己被git管理的文件夹（含有.git文件夹）。

+ 重新与远程仓库建立联系，并可以提交代码（自己远程仓库有代码 , 本地无代码，比如重装系统或者换电脑）：
  
+ 如果只是本地没有了仓库，在与远程仓库建立链接时提示”远程origin已存在"，可以直接commit+push。
  
  ```bash
  # 克隆远程仓库到本地仓库
  git clone git@github.com:username/repository_name.git(远程仓库地址ssh）
  # cd进本地仓库
  cd 仓库
  # 初始化git
  git init
  # 与远程仓库建立链接（关联远程仓库，远程仓库名字叫origin）
  git remote add origin git@github.com:username/repository_name.git(远程仓库地址ssh)
  # 获取远程更新
  git fetch origin
  # 把更新的内容合并到本地分支
  git merge origin/main
  # 对代码进行一些修改
  git add .
  git commit -m "..."
  git push origin main
  ```

+  创建一个新的仓库：
  
  ```bash
  # 首先在github上创建一个空仓库（之后其实就可以看到教程了）
  
  # 创建一个README.md文件，并写入"# test"
  echo "# test" >> README.md 
  # 初始化.git文件夹
  git init
  # 将刚刚创建的README.md文件加入git管理
  git add README.md 
  # 创建一个提交（即刚刚README.md文件的变化）
  git commit -m "first commit" 
  # 创建分支main作为默认分支
  git branch -M main
  # 与远程仓库建立连接，远程仓库的名字默认是origin（可换）
  git remote add origin git@github.com:username/repository_name.git（远程仓库地址ssh）
  # 将提交推送到远程仓库（-u是指第一次提交）
  git push -u origin main
  ```
  
+ 把本地的仓库提交到一个新建立的远程仓库（即远程仓库为空，本地已经有了一个包含.git文件的仓库，希望不改变内容，将本地仓库推送到远程并关联）

+ ```bash
  # 与远程仓库建立连接（关联）
  git remote add origin git@github.com:username/repository_name.git（远程仓库地址ssh）
  # 与远程仓库建立连接，远程仓库的名字默认是origin（可换）
  git branch -M main
  # 将提交推送到远程仓库（-u是指第一次提交）
  git push -u origin main
  ```

+ 远程origin已存在：
  
  ```bash
  # 查看远程配置
  git remote -v
  # 删除远程配置
  git remote rm [远程仓库名]
  ```
  
+ fatal:拒绝合并无关的历史：
  
  ```bash
  # 需要将远程仓库和本地仓库关联起来：
  git branch --set-upstream-to=origin/main main
  # 然后使用git pull整合远程仓库和本地仓库
  git pull --allow-unrelated-histories# 忽略版本不同造成的影响
  ```

如何给开源社区提交PR

假设官方GitHub名叫[officialName]，你的GitHub名叫[yourName]，GitHub仓库名字叫[repository]。

1. 网页操作：在官方GitHub仓库fork一个相同的仓库到你的GitHub.

2. 在你的仓库复制代码地址[your code SSH/HTTPS]，在官方仓库复制代码地址[official code SSH/HTTPS]，然后

   ```bash
   # 克隆你GitHub仓库到本地
   git clone [your code SSH/HTTPS]
   # 添加官方仓库地址
   git remote add upstream [official code SSH/HTTPS]
   # 检查仓库的远程信息
   git remote -v
   # 远程信息输出应为：
   origin  [your code SSH/HTTPS] (fetch)
   origin  [your code SSH/HTTPS] (push)
   upstream        [official code SSH/HTTPS] (fetch)
   upstream        [official code SSH/HTTPS] (push)
   ```

   fetch和push分别代表你在fetch（拉取）和push（推送）的GitHub仓库的地址，origin代表你的GitHub仓库，upstream代表官方的GitHub仓库。

3. ```bash
   # 从upstream中获取最新的代码下载到本地，但是不会自动合并到本地分支中。一般和下面第二个命令一起使用。
   git fetch upstream
   # 将upstream的master分支合并到当前本地分支中，如果有冲突需要手动解决（本地分支的代码就与upstream的master分支保持同步）
   git merge upstream/master
   # 创建并切换到名为fixBug的新分支
   git checkout -b fixBug
   # 将当前分支fixBug推送到远程仓库origin上，并将本地分支和远程分支关联起来。
   git push -u origin fixBug
   ```

4. 之后，你可以在fixBug分支上进行一些修改，然后通过一下步骤将修改好的代码push到GitHub仓库的对应分支。

   ```bash
   # 查看修改了那些文件
   git status
   # 查看修改内容
   git diff
   # add 提交
   git add .
   # commit 提交（如果官方仓需要Signed-off-by检查的就带账号邮箱信息）
   git commit -m "xxxxx" -s
   # push到远程仓
   git push origin
   ```

5. 此时，你的GitHub仓库的fixBug分支的代码已经发生了变化，在Pull requests界面你可以找到提交PR的方法，然后就就可以按照社区要去提供代码了。对于本地的代码，一旦你创建分支之后，master和fixBug就是两份代码了，你在fixBug的修改并不会影响到master的的代码。你可以通过`git checkout [分支名]`来切换到你想要的分支。你可以创建多个分支来确保工作的顺利进行。

6. 其他有用的命令

   ```bash
   # 分支操作：
   # 查看所有分支
   git branch（*代表当前所在分支）
   # 删除分支
   git branch -d [branchName](-d换为-D强制删除)
   # 更改分支名
   git branch -m [oldName] [newName]
   
   # 从远程仓库upstream拉取代码（获取最新的代码，但不会将其合并到本地）
   git fetch upstream
   # 从远程仓库origin拉取代码（获取最新的代码，但不会将其合并到本地）
   git fetch origin
   # 将本地分支重置为upstream仓库的版本
   git reset --hard upstream/<upstream_branch_name>
   # 将本地分支重置为oorigin仓库的版本
   git reset --hard origin/<origin_branch_name>
   # 将本地仓库的更改推送到origin（-f选项强制推送更改，这将覆盖origin仓库中的所有更改）
   git push -f origin <local_branch_name>
   
   # 删除本地未跟踪的目录和文件（d和f分别指目录和文件）
   git clean -df
   # 从origin指定的(自己的）GitHub仓库拉取代码
   git fetch origin
   ```
   
   
