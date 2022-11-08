令牌：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

+ 从新与GitHub仓库建立联系，并可以提交代码：
  
  ```bash
  #克隆仓库到本地
  git clone git@github.com:Origin-yy/Note.git(ssh代码地址）
  #cd进仓库
  git init
  #与远程仓库建立链接
  git remote add origin git@github.com:Origin-yy/Note.git(ssh代码地址)
  #获取远程更新
  git fetch origin
  #把更新的内容合并到本地分支
  git merge origin/main
  #对代码进行一些修改
  git add .
  git commit -m "..."
  git push origin main
  ```

+ 远程origin已存在：
  
  ```bash
  #删除远程配置
  git remote rm origin
  ```

+ fatal:拒绝合并无关的历史：
  
  ```bash
  #需要将远程仓库和本地仓库关联起来：
  git branch --set-upstream-to=origin/master master
  #然后使用git pull整合远程仓库和本地仓库
  git pull --allow-unrelated-histories#忽略版本不同造成的影响
  ```
