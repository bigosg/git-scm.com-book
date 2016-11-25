
Git学习笔记
=============
参考：https://git-scm.com/book/zh/v2

# 1. 起步
## 1.1 关于版本控制
* 本地版本控制系统
* 集中化的版本控制系统(CVCS)
  - CVS、Subversion 以及 Perforce。
* 分布式版本控制系统(DVCS)
  - Git、Mercurial、Bazaar 以及 Darcs。

## 1.2 Git 简史
设计目标:
 - 速度
 - 简单的设计
 - 对非线性开发模式的强力支持（允许成千上万个并行开发的分支）
 - 完全分布式
 - 有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）

## 1.3 Git 基础
与其他版本管理系统的差异：
- 直接记录快照，而非差异比较
- 近乎所有操作都是本地执行
- Git 保证完整性
- Git 一般只添加数据
- 三种状态
  - 已提交（committed）：表示数据已经安全的保存在本地数据库中。
  - 已修改（modified）：已修改表示修改了文件，但还没保存到数据库中。
  - 已暂存（staged）： 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。

## 1.4 命令行
- 命令行模式
  - 包含Git的所有命令
- GUI 模式
  - 只实现了 Git 所有功能的一个子集以降低操作难度

## 1.5 安装 Git
。。。

## 1.6 初次运行 Git 前的配置
- 通过 `git config`来设置控制 Git 外观和行为的配置变量。
  - `/etc/gitconfig` 针对所有用户
  - `~/.gitconfig 或 ~/.config/git/config` 针对当前用户
  - `.git/config` 目录 针对该仓库

  每一个级别覆盖上一级别的配置，所以 `.git/config` 的配置变量会覆盖 `/etc/gitconfig` 中的配置变量。
- 用户信息

  `$ git config --global user.name "John Doe"` ：用户名

  `$ git config --global user.email johndoe@example.com`： 邮箱

- 文本编辑器

  `$ git config --global core.editor emacs`

- 检查配置信息

  `git config --list`

## 1.7 获取帮助
  - `git help <verb>`

  - `git <verb> --help`

  - `man git-<verb>`

  - `git help config` :获得 config 命令的手册


## 1.8 总结
...

# 2. Git 基础
## 2.1 获取 Git 仓库
- 在现有目录中初始化仓库

  `$ git init`

  `$ git add *.c`

  `$ git add LICENSE`

  `$ git commit -m 'initial project version'`
- 克隆现有的仓库

  `$ git clone https://github.com/libgit2/libgit2`

  `$ git clone https://github.com/libgit2/libgit2 mylibgit`  自定义名称为mylibgit

## 2.2 记录每次更新到仓库
- 检查当前文件状态

  `$ git status`

- 跟踪新文件

  `$ git add README`

- <strong>暂存已修改文件</strong> （*需要重点注意*）

  `git add README` :可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等。

- 状态简览

  `git status -s` 或者 `git status --short`

- 忽略文件

  - 我们可以创建一个名为 .gitignore 的文件，列出要忽略的文件模式。

  - 文件 .gitignore 的格式规范如下：

    - 所有空行或者以 ＃ 开头的行都会被 Git 忽略。

    - 可以使用标准的 glob 模式匹配。

    - 匹配模式可以以（/）开头防止递归。

    - 匹配模式可以以（/）结尾指定目录。

    - 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。

    - https://github.com/github/gitignore :十分详细的针对数十种项目及语言的 .gitignore 文件列表

- 查看已暂存和未暂存的修改

  - 查看尚未暂存的文件更新了哪些部分：`git diff`

  - 要查看已暂存的将要添加到下次提交里的内容：`git diff --cached` 或者 `git diff --staged` (Git 1.6.1 及更高版本)

  - Git Diff 的插件版本
    - 使用`git difftool` 命令来用 Araxis ，emerge 或 vimdiff 等软件输出 diff 分析结果。
    - 使用 `git difftool --tool-help` 命令来看你的系统支持哪些 Git Diff 插件。

- 提交更新

  - `$ git commit` (这种方式会启动文本编辑器以便输入本次提交的说明。)

  - `使用 git config --global core.editor` 命令设定你喜欢的编辑软件。）
  - 你也可以在 commit 命令后添加 -m 选项，将提交信息与命令放在同一行:

   `$ git commit -m "Story 182: Fix benchmarks for speed"`

- 跳过使用暂存区域

   `$ git commit -a`

- 移除文件

  `$ rm PROJECTS.md`

  `$ git rm PROJECTS.md`

- 移动文件

  `$ git mv file_from file_to`

  运行 `git mv` 就相当于运行了下面三条命令：
  <pre><code>
  $ mv README.md README
  $ git rm README.md
  $ git add README
  </code></pre>

## 2.3 查看提交历史
- `git log` 会按提交时间列出所有的更新，最近的更新排在最上面。

- `git log -p -2` 用来显示每次提交的内容差异。 你也可以加上 -2 来仅显示最近两次提交

- `git log --stat` 看到每次提交的简略的统计信息

- `git log --pretty=oneline` 将每个提交放在一行显示，查看的提交数很大时非常有用。
  - 还有 `short`，`full` 和 `fuller` 可以用

- `git log --pretty=format:"%h - %an, %ar : %s"` 可以定制要显示的记录格式

- `git log --pretty=format:"%h %s" --graph` 这个选项添加了一些ASCII字符串来形象地展示你的分支、合并历史

- 限制输出长度

  `git log --since=2.weeks` 列出所有最近两周内的提交


## 2.4 撤消操作
- 撤销操作
  <pre><code>
  $ git commit -m 'initial commit'
  $ git add forgotten_file
  $ git commit --amend
  </code></pre>

  最终你只会有一个提交 - 第二次提交将代替第一次提交的结果。

- 取消暂存的文件

  `git reset HEAD CONTRIBUTING.md`

- 撤消对文件的修改

  `git checkout -- CONTRIBUTING.md`

  <strong>你需要知道 `git checkout -- [file]` 是一个危险的命令，这很重要。 你对那个文件做的任何修改都会消失 - 你只是拷贝了另一个文件来覆盖它。</strong>

## 2.5 远程仓库的使用
- 查看远程仓库

  `git remote`

  <pre><code>
  $ git remote -v
  origin	https://github.com/schacon/ticgit (fetch)
  origin	https://github.com/schacon/ticgit (push)
  </code></pre>

- 添加远程仓库
  <pre><code>
  $ git remote add pb https://github.com/paulboone/ticgit
  $ git remote -v
  origin	https://github.com/schacon/ticgit (fetch)
  origin	https://github.com/schacon/ticgit (push)
  pb	https://github.com/paulboone/ticgit (fetch)
  pb	https://github.com/paulboone/ticgit (push)  
  </pre></code>

  可以运行 `git fetch pb`：拉取 Paul 的仓库中有但你没有的信息。

- 从远程仓库中抓取与拉取

  `$ git fetch [remote-name]`

- 推送到远程仓库

  `$ git push origin master`

- 查看远程仓库

  `git remote show origin`

- 远程仓库的移除与重命名

  <pre><code>
  $ git remote rename pb paul
  $ git remote
  origin
  paul
  </code></pre>

  <pre><code>
  $ git remote rm paul
  $ git remote
  origin
  </code></pre>

## 2.6 打标签
- 列出标签

  `git tag`

  `it tag -l 'v1.8.5*'` 只对 1.8.5 系列感兴趣

- 创建标签
  - 附注标签

    `git tag -a v1.4 -m 'my version 1.4'`

    `git show` 看到标签信息与对应的提交信息

  - 轻量标签

    `git tag v1.4-lw`

  - 后期打标签
    `git tag -a v1.2 9fceb02`

- 共享标签

  `git push origin v1.5`

  `git push origin --tags` 这将会把所有不在远程仓库服务器上的标签全部传送到那里。

- 检出标签

  `git checkout -b version2 v2.0.0`


## 2.7 Git 别名

  <pre><code>
  $ git config --global alias.co checkout  
  $ git config --global alias.br branch
  $ git config --global alias.ci commit
  $ git config --global alias.st status
  </code></pre>

  `$ git config --global alias.unstage 'reset HEAD --'`：表示`git unstage fileA` 和 `git reset HEAD -- fileA`等价


  `git config --global alias.last 'log -1 HEAD'` 和`git last`等价

## 2.8 总结
...

# 3. Git 分支
## 3.1 分支简介

- 分支创建

  `$ git branch testing` 仅仅 创建 一个新分支，并不会自动切换到新分支中去。

  <strong>在 Git 中，`HEAD `是一个指针，指向当前所在的本地分支</strong>

- 分支切换

  `$ git checkout testing`

  <strong>分支切换会改变你工作目录中的文件</strong>

## 3.2 分支的新建与合并

- 新建分支

  `$ git checkout -b iss53`

  相当于两条命令：

  <pre><code>
  $ git branch iss53
  $ git checkout iss53
  </pre></code>

- 分支的合并

  <pre><code>
  $ git checkout master
  Switched to branch 'master'
  $ git merge iss53
  Merge made by the 'recursive' strategy.
  index.html |    1 +
  1 file changed, 1 insertion(+)
  </pre></code>

  删除已经完成的分支:`$ git branch -d iss53`

- 遇到冲突时的分支合并
  冲突完了之后使用:

   `git add` 命令来将其标记为冲突已解决。

   `git mergetool` 使用图形化工具来解决冲突

## 3.3 分支管理
- `git branch`  会得到当前所有分支的一个列表

- `git branch -v` 查看每一个分支的最后一次提交

- `--merged` 与 `--no-merged` 这两个有用的选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支。

- `git branch -d` 删除非当前无用分支

查看所有包含未合并工作的分支，可以运行 `git branch --no-merged`：

`$ git branch --no-merged`

  testing
这里显示了其他分支。 因为它包含了还未合并的工作，尝试使用` git branch -d `命令删除它时会失败：

<pre><code>
$ git branch -d testing
error: The branch 'testing' is not fully merged.
If you are sure you want to delete it, run 'git branch -D testing'.
</pre></code>
如果真的想要删除分支并丢掉那些工作，如同帮助信息里所指出的，可以使用 `-D` 选项强制删除它。

## 3.4 分支开发工作流

- 长期分支

  流水线（work silos）

- 特性分支

  特性分支是一种短期分支，它被用来实现单一特性或其相关工作。

## 3.5 远程分支

它们以 `(remote)/(branch)` 形式命名。


- 推送

- 跟踪分支

- 拉取

- 删除远程分支

## 3.6 变基
  在 Git 中整合来自不同分支的修改主要有两种方法：`merge` 以及 `rebase`。

- 变基的基本操作
  使用 `rebase` 命令将提交到某一分支上的所有修改都移至另一分支上。

  <pre><code>
  $ git checkout experiment
  $ git rebase master
  First, rewinding head to replay your work on top of it...
  Applying: added staged command
  </pre></code>

  <pre><code>
  $ git checkout master
  $ git merge experiment
  </pre></code>

  ![Alt text](https://git-scm.com/book/en/v2/book/03-git-branching/images/interesting-rebase-1.png)

  `$ git rebase --onto master server client` “取出 client 分支，找出处于 `client` 分支和 `server` 分支的共同祖先之后的修改，然后把它们在 `master` 分支上重演一遍”。

  <pre><code>
  $ git checkout master
  $ git merge client
  </pre></code>

  ![Alt text](https://git-scm.com/book/en/v2/book/03-git-branching/images/interesting-rebase-3.png)

  `$ git rebase master server`

  ![Alt](https://git-scm.com/book/en/v2/book/03-git-branching/images/interesting-rebase-4.png)

  <pre><code>
  $ git checkout master
  $ git merge server
  </pre></code>

  ![Alt](https://git-scm.com/book/en/v2/book/03-git-branching/images/interesting-rebase-5.png)


- 变基的风险

  <strong>不要对在你的仓库外有副本的分支执行变基。</strong>

- 用变基解决变基
  `git pull --rebase`

  <strong>需要精读</strong>

- 变基 vs. 合并

# 4. 服务器上的 Git
## 4.1 协议
- 本地协议

  `$ git clone file:///opt/git/project.git` 克隆一个本地版本库

- HTTP 协议
  - 智能（Smart） HTTP 协议
    - 优点
      - 可以使用用户名／密码授权
      - HTTP/S 协议被广泛使用，一般的企业防火墙都会允许这些端口的数据通过。
    - 缺点
      - 架设 HTTP/S 协议的服务端会比 SSH 协议的棘手一些。
  - 哑（Dumb） HTTP 协议

- SSH 协议
  - 优点
    - SSH 架设相对简单
    - 通过 SSH 访问是安全的
    - 与 HTTP/S 协议、Git 协议及本地协议一样，SSH 协议很高效，在传输前也会尽量压缩数据。
  - 缺点： 不能通过他实现匿名访问

- Git 协议
  - 优点：使用的网络传输协议里最快的。
  - 缺点：缺乏授权机制。

## 4.2 在服务器上搭建 Git
- 现有仓库导出为裸仓库

  `$ git clone --bare my_project my_project.git`

- 把裸仓库放到服务器上

  `$ scp -r my_project.git user@git.example.com:/opt/git`

- 小型安装
  - 提供访问权限（SSH 连接）
    - 给团队里的每个人创建账号，这种方法很直接但也很麻烦。
    - 在主机上建立一个 git 账户，让每个需要写权限的人发送一个 SSH 公钥，然后将其加入 git 账户的 ~/.ssh/authorized_keys 文件。
    - 让 SSH 服务器通过某个 LDAP 服务，或者其他已经设定好的集中授权机制，来进行授权。 只要每个用户可以获得主机的 shell 访问权限，任何 SSH 授权机制你都可视为是有效的。


## 4.3 生成 SSH 公钥

## 4.4 配置服务器

## 4.5 Git 守护进程

## 4.6 Smart HTTP
## 4.7 GitWeb
## 4.8 GitLab
## 4.9 第三方托管的选择
## 4.10 总结


# 5. 分布式 Git
## 5.1 分布式工作流程
- 集中式工作流

  一个中心集线器，或者说仓库，可以接受代码，所有人将自己的工作与之同步。 若干个开发者则作为节点——也就是中心仓库的消费者——并且与其进行同步。

- 集成管理者工作流 (感觉和github目前很像)
  - 项目维护者推送到主仓库。
  - 贡献者克隆此仓库，做出修改。
  - 贡献者将数据推送到自己的公开仓库。
  - 贡献者给维护者发送邮件，请求拉取自己的更新。
  - 维护者在自己本地的仓库中，将贡献者的仓库加为远程仓库并合并修改。
  - 维护者将合并后的修改推送到主仓库。

- 司令官与副官工作流

  这其实是多仓库工作流程的变种。 一般拥有数百位协作开发者的超大型项目才会用到这样的工作方式，例如著名的 Linux 内核项目。 被称为副官（lieutenant）的各个集成管理者分别负责集成项目中的特定部分。 所有这些副官头上还有一位称为司令官（dictator）的总集成管理者负责统筹。 司令官维护的仓库作为参考仓库，为所有协作者提供他们需要拉取的项目代码。

## 5.2 向一个项目贡献
- 提交准则
  - 首先，你不会想要把空白错误（根据 git help diff 的描述，结合下面给出的图片，空白错误是指行尾的空格、Tab 制表符，和行首空格后跟 Tab 制表符的行为）提交上去。
  - 尝试让每一个提交成为一个逻辑上的独立变更集。
  - 提交信息。 有一个创建优质提交信息的习惯会使 Git 的使用与协作容易的多。

- 私有小型团队
- 私有管理团队
- 派生的公开项目
- 通过邮件的公开项目

## 5.3 维护项目
- 在特性分支中工作
- 应用来自邮件的补丁
- 检出远程分支
- 确定引入了哪些东西
- 将贡献的工作整合进来
- 为发布打标签
- 生成一个构建号
- 准备一次发布
- 制作提交简报

## 5.4 总结

# 6. GitHub
## 6.1 账户的创建和配置
## 6.2 对项目做出贡献
## 6.3 维护项目
## 6.4 管理组织
## 6.5 脚本 GitHub
## 6.6 总结

# 7. Git 工具
## 7.1 选择修订版本
- 单个修订版本
- 简短的 SHA-1
- 分支引用
- 引用日志

  `git reflog`

  `$ git show HEAD@{5}`: 查看仓库中 HEAD 在五次前的所指向的提交

  `$ git show master@{yesterday}` :查看你的 master 分支在昨天的时候指向了哪个提交

- 祖先引用
- 提交区间
  - 双点
  - 多点
  - 三点

## 7.2 交互式暂存
<pre><code>
$ git add -i
           staged     unstaged path
  1:    unchanged        +0/-1 TODO
  2:    unchanged        +1/-1 index.html
  3:    unchanged        +5/-1 lib/simplegit.rb
*** Commands ***
  1: status     2: update      3: revert     4: add untracked
  5: patch      6: diff        7: quit       8: help
What now>
</pre></code>

- 暂存与取消暂存文件

  `What now> 2`

- 暂存补丁

## 7.3 储藏与清理
- 储藏工作

  `git stash 或 git stash save：` :想要切换分支，但是还不想要提交之前的工作；所以储藏修改。 将新的储藏推送到栈上。

  `git stash list`:查看储藏的东西

  `git stash apply`: 将刚刚储藏的工作重新应用

  `git stash apply stash@{2}。` :想要应用其中一个更旧的储藏，可以通过名字指定它。

- 创造性的储藏

- 从储藏创建一个分支

- 清理工作目录

  ` git clean`

## 7.4 签署工作
- GPG 介绍
- 签署标签
- 验证标签
- 每个人必须签署


## 7.5 搜索
- Git Grep
- Git 日志搜索

## 7.6 重写历史
- 修改最后一次提交

  `$ git commit --amend`
- 修改多个提交信息

  `$ git rebase -i HEAD~3`

- 重新排序提交

- 压缩提交

- 拆分提交

- 核武器级选项：filter-branch
  - 从每一个提交移除一个文件
  - 使一个子目录做为新的根目录
  - 全局修改邮箱地址

## 7.7 重置揭密

- 三棵树
- 工作流程
- 重置的作用
- 通过路径来重置
- 压缩
- 检出
- 总结

## 7.8 高级合并
- 合并冲突
- 撤消合并
- 其他类型的合并

## 7.9 Rerere

- `git rerere`

## 7.10 使用 Git 调试
- 文件标注

  `git blame`

- 二分查找

  `git bisect`

## 7.11 子模块

- 开始使用子模块

  <pre><code>
  $ git submodule add https://github.com/chaconinc/DbConnector
  Cloning into 'DbConnector'...
  remote: Counting objects: 11, done.
  remote: Compressing objects: 100% (10/10), done.
  remote: Total 11 (delta 0), reused 11 (delta 0)
  Unpacking objects: 100% (11/11), done.
  Checking connectivity... done.
  </pre></code>

- 克隆含有子模块的项目
- 在包含子模块的项目上工作
- 子模块技巧
- 子模块的问题

## 7.12 打包 （没网络的情况）

- 打包

  <pre><code>
  $ git bundle create repo.bundle HEAD master
  Counting objects: 6, done.
  Delta compression using up to 2 threads.
  Compressing objects: 100% (2/2), done.
  Writing objects: 100% (6/6), 441 bytes, done.
  Total 6 (delta 0), reused 0 (delta 0)
  </pre></code>

- 解包

  <pre><code>
  $ git clone repo.bundle repo
  Initialized empty Git repository in /private/tmp/bundle/repo/.git/
  $ cd repo
  $ git log --oneline
  9a466c5 second commit
  b1ec324 first commit
  </pre></code>

- 检查
  <pre><code>
  $ git bundle verify ../commits-bad.bundle
  error: Repository lacks these prerequisite commits:
  error: 7011d3d8fc200abe0ad561c011c3852a4b7bbe95 third commit - second repo
  </pre></code>

## 7.13 替换
## 7.14 凭证存储

- 底层实现
- 自定义凭证缓存

## 7.15 总结

# 8 自定义 Git
## 8.1 配置 Git

- 客户端基本配置

  `$ man git-config`

- Git 中的着色

  `$ git config --global color.ui false`

- 外部的合并与比较工具

  `P4Merge`

- 格式化与多余的空白字符

- 服务器端配置

## 8.2 Git 属性

- 二进制文件
  - 识别二进制文件
  - 比较二进制文件

- 关键字展开
- 导出版本库
- 合并策略

## 8.3 Git 钩子

- 安装一个钩子
- 客户端钩子
  - 提交工作流钩子
    - `pre-commit` 钩子在键入提交信息前运行。 它用于检查即将提交的快照
    - `prepare-commit-msg` 钩子在启动提交信息编辑器之前，默认信息被创建之后运行。 它允许你编辑提交者所看到的默认信息。
    - `commit-msg `钩子接收一个参数，此参数即上文提到的，存有当前提交信息的临时文件的路径。
    - ` post-commit `钩子在整个提交过程完成后运行。 它不接收任何参数，但你可以很容易地通过运行 git log -1 HEAD 来获得最后一次的提交信息。 该钩子一般用于通知之类的事情。
  - 电子邮件工作流钩子`git am`
    - ` applypatch-msg `。 它接收单个参数：包含请求合并信息的临时文件的名字。
    - ` pre-applypatch` 。 有些难以理解的是，它正好运行于应用补丁 之后，产生提交之前，所以你可以用它在提交前检查快照。
  - 其它钩子
    - `pre-rebase`钩子运行于变基之前，以非零值退出可以中止变基的过程。
    - `post-rewrite` 钩子被那些会替换提交记录的命令调用，比如 `git commit --amend` 和`git rebase`（不过不包括 git filter-branch）。
    - 在 git checkout 成功运行后，`post-checkout` 钩子会被调用。
    - 在 git merge 成功运行后，`post-merge `钩子会被调用。
    - `pre-push `钩子会在 git push 运行期间， 更新了远程引用但尚未传送对象时被调用。
  - 服务器端钩子
    - `pre-receive` 它从标准输入获取一系列被推送的引用。如果它以非零值退出，所有的推送内容都不会被接受。 你可以用这个钩子阻止对引用进行非快进（non-fast-forward）的更新，或者对该推送所修改的所有引用和文件进行访问控制。
    - `update `脚本和 `pre-receive` 脚本十分类似，不同之处在于它会为每一个准备更新的分支各运行一次。
    - `post-receive`挂钩在整个过程完结以后运行，可以用来更新其他系统服务或者通知用户。

## 8.4 使用强制策略的一个例子
## 8.5 总结

# 9. Git 与其他系统
## 9.1 作为客户端的 Git

- 作为客户端的 Git
  - Git 与 Subversion
    - `git svn`  

## 9.2 迁移到 Git
## 9.3 总结

# 10. Git 内部原理
## 10.1 底层命令和高层命令
## 10.2 Git 对象
## 10.3 Git 引用
## 10.4 包文件
## 10.5 引用规格
## 10.6 传输协议
## 10.7 维护与数据恢复
## 10.8 环境变量

# A1. 其它环境中的 Git
## A1.1 图形界面
## A1.2 Visual Studio 中的 Git
## A1.3 Eclipse 中的 Git
## A1.4 Bash 中的 Git
## A1.5 Zsh 中的 Git
## A1.6 Powershell 中的 Git
## A1.7 总结

# A2. 将 Git 嵌入你的应用
## A2.1 命令行 Git 方式
## A2.2 Libgit2
## A2.3 JGit

# A3. Git 命令
## A3.1 设置与配置
 - `git config`
 - `git help`

## A3.2 获取与创建项目
- `git help`
- `git clone`

## A3.3 快照基础
- `git add`
- `git status`
- `git diff`
- `git difftool`
- `git commit`
- `git rm`
- `git mv`
- `git clean`

## A3.4 分支与合并
- `git branch`
- `git checkout`
- `git merge`
- `git mergetool`
- `git log`
- `git stash`
- `git tag`

## A3.5 项目分享与更新
- `git fetch`
- `git pull`
- `git push`
- `git remote`
- `git archive`
- `git submodule
`
## A3.6 检查与比较
- `git show`
- `git shortlog`
- `git describe`

## A3.7 调试
- `git bisect`
- `git blame`
- `git grep`

## A3.8 补丁
- `git cherry-pick`
- `git rebase`
- `git revert`

## A3.9 邮件
- `git apply`
- `git am`
- `git format-patch`
- `git imap-send`
- `git send-email`
- `git request-pull`

## A3.10 外部系统
- `git svn`
- `git fast-import`

## A3.11 管理
- `git gc`
- `git fsck`
- `git reflog`
- `git filter-branch`

## A3.12 底层命令
