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
  # 删除远程配置
  git remote rm origin
  ```

+ fatal:拒绝合并无关的历史：
  
  ```bash
  # 需要将远程仓库和本地仓库关联起来：
  git branch --set-upstream-to=origin/main main
  # 然后使用git pull整合远程仓库和本地仓库
  git pull --allow-unrelated-histories# 忽略版本不同造成的影响
  ```
