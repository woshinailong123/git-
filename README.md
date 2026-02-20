# git基本命令

### 下载代码

`git clone --recursive git@gitee.com:wang-66/diff-power.git --depth 10`

`--recursive`：**同时把仓库里的子模块（submodule）也一起克隆/初始化**。否则子模块目录会是空的或只含一个引用。

`--depth 10`：浅克隆，只拉取**最近 10 条提交历史**（减少下载量，加快速度）。

### 配置提交身份（全局）

登陆 GitHub 用户名
 `git config --global user.name "John Doe"`
 
 查看当前用户名
 `git config --global user.name`
 
 登陆 GitHub 的邮箱
 `git config --global user.email [johndoe@example.com](mailto:johndoe@example.com)`
 
 查看当前邮箱
 `git config --global user.email`

### 提交

`git commit -m "[planner]: add st boundary pre decider test"`

**作用**：把暂存区内容提交成一个新的 commit。

`-m`：直接写提交说明（message）。

**备注**：“添加评论”通常指写好 commit message，让别人知道你改了什么。

### 推送并建立上游关联

`git push --set-upstream origin happy/0817_test`

**作用**：把本地分支推到远端 `origin`，并设置该本地分支的**上游跟踪分支**为 `origin/happy/0817_test`。

好处：以后你在这个分支上可以直接 `git push` / `git pull` 不用再写远端和分支名。

### 子模块相关

`git submodule update --init --recursive`

`--init`：如果本地还没初始化过子模块，就初始化它（相当于帮你做了 `.gitmodules` 里的配置读取和建立）。

`--recursive`：如果子模块里面还有子模块，也一并更新。

### 查看状态

`git status`

### 创建并切到新分支

`git checkout -b happy/0817_test`

**作用**：创建一个新分支并立即切换过去。

等价于：

`git branch happy/0817_test`

`git checkout happy/0817_test`

**注意点**：

新分支默认从你当前所在分支的当前提交点分出来。

### 撤销最近一次提交，但保留改动

`git reset --soft HEAD^`

**作用**：把当前分支指针回退到上一个提交（`HEAD^`），**但保留改动在暂存区**。

`--soft` 的含义：

回退 commit 记录

不动工作区文件

不动暂存区内容（改动仍是 staged）

**适用场景**：

刚提交完发现 message 写错了 / 想把多个提交合并成一个 / 想改完再重新提交

### 强制推送

`git push -f`

**作用**：强制把本地分支的历史覆盖推到远端（force push）。

通常配合 `reset` / `rebase` 使用，因为这些操作会改写提交历史，普通 push 会被拒绝。

**强烈注意**：

如果这个远端分支别人也在用，你强推会让别人的历史对不上，可能造成麻烦。

更安全的写法是：`git push --force-with-lease`（会检查远端是否有你不知道的新提交

### 查看远端信息

`git remote -v`

**作用**：添加一个名叫 `origin` 的远端，并设置它的 URL。

**注意点**：

如果你已经有 `origin` 了，再 add 会报错（remote origin already exists）。

想替换 URL 用：`git remote set-url origin <new_url>`

###  拉取远端对象（浅）

`git fetch --depth 10`

**作用**：从远端拉取更新到本地（更新远端跟踪分支），但不合并到当前分支。

`--depth 10`：只拉最近 10 条历史（浅 fetch）。

**和 pull 的区别**

`fetch`：只下载 + 更新远端引用，不改你当前分支代码


`pull`：= `fetch` + `merge`（或 rebase），会改动你当前分支
