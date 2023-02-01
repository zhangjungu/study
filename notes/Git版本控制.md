

# Git版本控制

## 一、版本控制介绍

#### 集中式

版本库是集中存放在中央服务器的，而干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器。

#### 分布式

分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库，这样，你工作的时候，就不需要联网了，因为版本库就在你自己的电脑上。既然每个人电脑上都有一个完整的版本库，那多个人如何协作呢？比方说你在自己电脑上改了文件A，你的同事也在他的电脑上改了文件A，这时，你们俩之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。

#### 分布式的优势

和集中式版本控制系统相比，分布式版本控制系统的安全性要高很多，因为每个人电脑里都有完整的版本库，某一个人的电脑坏掉了不要紧，随便从其他人那里复制一个就可以了。而集中式版本控制系统的中央服务器要是出了问题，所有人都没法干活了。

在实际使用分布式版本控制系统的时候，其实很少在两人之间的电脑上推送版本库的修改，因为可能你们俩不在一个局域网内，两台电脑互相访问不了，也可能今天你的同事病了，他的电脑压根没有开机。因此，分布式版本控制系统通常也有一台充当“中央服务器”的电脑，但这个服务器的作用仅仅是用来方便“交换”大家的修改，没有它大家也一样干活，只是交换修改不方便而已。



## 二、安装Git软件

#### windows平台

git for windows ( https://gitforwindows.org/ )

#### Mac OS 或 Linux/Unix平台

git ( http://git-scm.com/ )



## 三、Git相关操作

### 安装配置

#### 初始配置

配置文件为 `~/.gitconfig` ，执行任何Git配置命令后文件将自动创建。

第一个要配置的是你个人的用户名称和电子邮件地址。这两条配置很重要，每次 Git 提交时都会引用这两条信息，说明是谁提交了更新，所以会随更新内容一起被永久纳入历史记录：

```
git config --global user.email "xxxxxxxxxx@qq.com"
git config --global user.name "vayne"
```

#### 常用命令

1. 初始化新仓库 `git init`
2. 克隆代码 `git clone https://gitee.com/houdunwang/hdcms.git`
3. 克隆指定分支 `git clone -b dev git@gitee.com:houdunwang/hdcms.git`
4. 查看状态 `git status`
5. 提交单个文件 `git add index.php`
6. 提交所有文件 `git add -A`
7. 使用通配符提交 `git add *.js`
8. 提交到仓库中 `git commit -m '提示信息'`
9. 提交已经跟踪过的文件，不需要执行add `git commit -a -m '提交信息'`
10. 删除版本库与项目目录中的文件 `git rm index.php`
11. 只删除版本库中文件但保存项目目录中文件 `git rm --cached index.php`
12. 修改最后一次提交 `git commit --amend`



### 工作流

#### 基础流程

1. 首先克隆你的项目

   ```text
   git clone https://gitee.com/houdunwang/hdcms.git
   ```

2. 开始开发添加新文件hd.js，这时新的文件并没有被版本库管理，可以通过以下命令查看没有被管理的文件

   ```text
   git clean -n
   ```

3. 将所有文件提交到暂存区

   ```text
   git add .
   ```

   这时再通过 `clean` 命令查看会发现结果为空，即文件已经被版本库管理了

   ```text
   git clean -n
   ```

4. 不小心将工作区中的 hd.js 文件删除了，现在可以将暂存区中的hd.js恢复回来

   ```text
   git checkout hd.js
   ```

5. 完成工作后创建一个新的提交，并使用 -m 选项说明完成的工作

   ```text
   git commit -m '购物车开发'
   ```

6. 将代码提交到远程服务器，与同事或其他开发者分享代码

   ```text
   git push 
   ```



#### 分支流程

大部分情况下不会直接在master分支工作，我们应该保护这个分支是最终开发完成代码健康可交付运行的。

所以功能和缺陷(bug)修复都会新建分支完成，除了这个概念外与基本流程使用是一样的。

1. 新建支付功能开发分支

   ```text
   git branch pay
   ```

2. 切换到新分支开始开发，这里的工作内容与上面的基础流程是一样的

   ```text
   git checkout pay
   ```

3. 开发完成执行提交

   ```text
   git commit -m 'H5 支付功能'
   ```

4. 合并分支到master

   ```text
   切换到master分支
   git checkout master
   
   合并pay分支的代码
   git merge pay
   ```

5. 提交代码到master远程分支

   ```text
   git push
   ```



### 基本管理

#### 工作区管理

git clean命令用来从工作目录中删除所有没有跟踪（tracked）过的文件

1. `git clean -n`是一次clean的演习, 告诉你哪些文件会被删除
2. `git clean -f`删除当前目录下没有tracked过的文件，不会删除.gitignore指定的文件
3. `git clean -df`删除当前目录下没有被tracked过的文件和文件夹



#### 暂存区管理

1. 提交所有修改和新增的文件 `git add .`
2. 只提交修改文件不提交新文件 `git add -u`
3. 放弃没有提交的所有修改 `git checkout .`
4. 放弃指定文件的修改 `git checkout hd.js`
5. 查看暂存区文件列表 `git ls-files -s`
6. 查看暂存区文件内容 `git cat-file -p 6e9a94`



#### 版本库管理

使用reset恢复到历史提交点，重置暂存区与工作目录的内容。

1. 清空工作区和暂存区的改动 `git reset --hard`
2. 恢复前三个版本 `git reset --hard HEAD^^^`
3. 保留工作区的内容，把文件差异放进暂存区 `git reset --soft`
4. 恢复到指定提交版本（先通过 git log 查看版本号) `git reset --hard b7b73147ca8d6fc20e451d7b36`
5. 放弃已经add 暂存区的文件hd.js `git reset HEAD hd.js`



#### 分支管理

分支用于为项目增加新功能或修复Bug时使用。

1. 创建分支 `git branch dev`

2. 查看分支 `git branch`

3. 切换分支 `git checkout dev`

4. 创建并切换分支 `git checkout -b feature/bbs`

5. 合并dev分支到master

   ```text
   git checkout master
   git merge dev
   ```

6. 删除分支 `git branch -d dev`

7. 删除没有合并的分支`git branch -D dev`

8. 删除远程分支 `git push origin :dev`

9. 查看未合并的分支(切换到master) `git branch --no-merged`

10. 查看已经合并的分支(切换到master) `git branch --merged`



#### 日志查看

1. 查看日志 `git log`
2. 查看最近2次提交日志并显示文件差异 `git log -p -2`
3. 显示已修改的文件清单 `git log --name-only`
4. 显示新增、修改、删除的文件清单 `git log --name-status`
5. 一行显示并只显示SHA-1的前几个字符 `git log --oneline`



### 效率提升

#### 定义别名

通过创建命令别名可以减少命令输入量，有几种方式进行设置

##### 配置文件定义

修改配置文件 ~/.gitconfig 并添加以下命令别名配置段

```text
[alias]
	a = add .
	c = commit
	s = status
	l = log
	b = branch
```

现在可以使用 `git a` 实现 `git add .` 一样的效果了。

##### 系统配置定义

window用户可以修改`~/.bashrc` 或 `~/.bash_profile`文件。

mac/linux修改 `~/.zshrc` 文件中定义常用的别名指令，需要首先安装zsh命令行扩展

```text
alias gs="git status"
alias gc="git commit -m "
alias gl="git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit  "
alias gb="git branch"
alias ga="git add ."
alias go="git checkout"
```

命令行直接使用 `gs` 即可以实现 `git status` 一样的效果了。

> window 系统需要使用 git for window 中的 `Git Base` 软件



#### .gitignore

.gitignore用于定义忽略提交的文件

- 所有空行或者以注释符号 `＃` 开头的行都会被 Git 忽略。
- 匹配模式最后跟反斜杠（`/`）说明要忽略的是目录。
- 可以使用标准的 glob 模式匹配。

```text
.idea
/vendor
.env
/node_modules
/public/storage
*.txt
```



#### 冲突解决

不同分修改同一个文件或不同开发者修改同一个分支文件都可能造成冲突，造成无法提交代码。

1. 使用编辑器修改冲突的文件
2. 添加暂存 `git add .` 表示已经解决冲突
3. git commit 提交完成



#### Stashing

当你正在进行项目中某一部分的工作，里面的东西处于一个比较杂乱的状态，而你想转到其他分支上进行一些工作。问题是，你不想提交进行了一半的工作，否则以后你无法回到这个工作点。

"暂存" 可以获取你工作目录的中间状态——也就是你修改过的被追踪的文件和暂存的变更——并将它保存到一个未完结变更的堆栈中，随时可以重新应用。

1. 储藏工作 `git stash`
2. 查看储藏列表 `git stash list`
3. 应用最近的储藏 `git stash apply`
4. 应用更早的储藏 `git stash apply stash@{2}`
5. 删除储藏`git stash drop stash@{0}`
6. 应用并删除储藏 `git stash pop`



#### Tag

Git 也可以对某一时间点上的版本打上标签 ，用于发布软件版本如 v1.0

1. 添加标签 `git tag v1.0`
2. 列出标签 `git tag`
3. 推送标签 `git push --tags`
4. 删除标签 `git tag -d v1.0.1`
5. 删除远程标签 `git push origin :v1.0.1`



#### 打包发布

对mster分支代码生成压缩包供使用者下载使用，`--prefix` 指定目录名

```text
git archive master --prefix='hdcms/' --format=zip > hdcms.zip
```



### 远程仓库

下面是最热的`Github`进行讲解，使用`码云、codeing` 等国内仓库使用方式一致，就不在赘述了。



#### 创建仓库

使用ssh连接Github发送指令更加安全可靠，也可以免掉每次输入密码的困扰。

在命令行中输入以下代码（windows用户使用 Git Bash）

```text
ssh-keygen -t rsa
```

一直按回车键直到结束。系统会在`~/.ssh` 目录中生成 `id_rsa`和`id_rsa.pub`，即密钥`id_rsa`和公钥`id_rsa.pub`。

**向GitHub添加秘钥**

![1526219105062](https://houdunren.gitee.io/note/assets/img/1526219105062-4856466.7379665a.png)

点击 `New SSH key` 按钮，添加上面生成的 `id_rsa.pub` 公钥内容。



#### 关联远程

1. 创建本地库并完成初始提交

   ```text
   echo "# hd-xj" >> README.md
   git init
   git add README.md
   git commit -m "first commit"
   ```

2. 添加远程仓库

   ```text
   git remote add origin git@github.com:houdunwang/hd-xj.git
   ```

3. 查看远程库

   ```text
    git remote -v
   ```

4. 推送数据到远程仓库

   ```text
   git push -u origin master
   ```

5. 删除远程仓库关联

   ```text
   git remote rm origin
   ```

> 通过 clone 克隆的仓库，本地与远程已经自动关联，上面几步都可以省略。



#### pull

拉取远程主机某个分支的更新，再与本地的指定分支合并。

1. 拉取origin主机的ask分支与本地的master分支合并 `git pull origin ask:ask`
2. 拉取origin主机的ask分支与当前分支合并 `git pull origin ask`
3. 如果远程分支与当前本地分支同名直接执行 `git pull`



#### push

`git push`命令用于将本地分支的更新，推送到远程主机。它的格式与`git pull`命令相似。

1. 将当前分支推送到`origin`主机的对应分支(如果当前分支只有一个追踪分支 ，可省略主机名)

   ```text
   git push origin
   ```

2. 使用`-u`选项指定一个默认主机 ,这样以后就可以不加任何参数直播使用`git push`。

   ```text
   $ git push -u origin master
   ```

3. 删除远程`ask`分支 `git push origin --delete ask`

4. 本地ask分支关联远程分支并推送 `git push --set-upstream origin ask`



#### 多库提交

我可以将代码提交到多个远程版本库中，比如后盾人的 [课程代码 (opens new window)](https://gitee.com/houdunren/code)就提交到了Github与Gitee两个库中。

```text
# 增加一个远程库
git remote add github git@github.com:houdunwang/coding.git

# 提交到远程库
git push github
```

也可以创建命令一次提交到两个库(注：参考上面的命令设置章节)

```text
alias gp="git push & git push github"
```



### 自动部署

GitHub设置 `WebHook`

![1526276371437](https://houdunren.gitee.io/note/assets/img/1526276371437.88e7de39.png)





#### 同步脚本

项目中添加处理 webhook 的webhook.php文件内容如下，并提交到版本库。

```text
<?php
// GitHub Webhook Secret.
// GitHub项目 Settings/Webhooks 中的 Secret
$secret = "houdunren";

// Path to your respostory on your server.
// e.g. "/var/www/respostory"
// 项目地址
$path = "/www/wwwroot/xj.houdunren.com";

// Headers deliveried from GitHub
$signature = $_SERVER['HTTP_X_HUB_SIGNATURE'];

if ($signature) {
  $hash = "sha1=".hash_hmac('sha1', file_get_contents("php://input"), $secret);
  if (strcmp($signature, $hash) == 0) {
    echo shell_exec("cd {$path} && /usr/bin/git reset --hard origin/master && /usr/bin/git clean -f && /usr/bin/git pull 2>&1");
    exit();
  }
}

http_response_code(404);
?>
```



#### 站点配置

##### 创建站点

下面示例我使用的是 `宝塔` 主机面板。 ![1526280838031](https://houdunren.gitee.io/note/assets/img/1526280838031.9af2ade9.png)

现在服务器上生成了站点目录 `/www/wwwroot/xj.houdunren.com` ，因为目录中存在 `.user.ini` 文件（定义站点可以访问的目录权限），造成不能 `clone` 代码，将目录随意改名。



##### shell_exec

执行 `git pull` 指令需要使用 `shell_exec` 函数，删除shell_exec 禁用函数后重启PHP。

![1526281914667](https://houdunren.gitee.io/note/assets/img/1526281914667.8ec5d311.png)



#### clone

登录服务器并使用 https 协议 clone 项目代码

```text
ssh root@xj.houdunren.com -p 22
git clone https://github.com/houdunwang/xj.git xj.houdunren.com
```



#### 修改权限

```text
chown -R www .
chmod -R g+s .
sudo -u www git pull
```

现在向GitHub 推送代码后，服务器将自动执行代码拉取，自动部署功能设置完成了。



## 四、参考文档

1. 廖雪峰的官方网站（ https://www.liaoxuefeng.com/wiki/896043488029600/896202780297248 ）
2. 后盾人官方网站（ https://houdunren.gitee.io/note/git/git.html ）



本人仅作学习整理，非原创，感谢各位老师的无私分享