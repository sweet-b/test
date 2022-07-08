### 版本控制工具

- 集中式版本控制工具
  - CVS、**SVN(Subversion)**、VSS……        SVN 企业常用

- 分布式版本控制工具
  - **Git**、Mercurial、Bazaar、Darcs……     Git 

### 一、相关概念

#### 1、理解工作区、版本库、暂存区概念

- **工作区(Working Directory)**：就是你电脑本地硬盘目录，一般是项目当前目录

- **版本库(Repository)**：工作区有个隐藏目录.git，它就是Git的本地版本库

- **暂存区(stage)**：一般存放在"git目录"下的index文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）

- **分支（Branch****）**：Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD

  

#### 2、用法

- git add  将工作区添加道暂存区

- git commit  将暂存区提交到本地库

  

#### 3、提交Git版本库分两步执行

- **第一步** 用“git add”把文件纳入Git管理，实际是把本地文件修改添加到暂存区

- **第二步** 用“git commit”提交更改，实际上就是把暂存区的所有内容提交到当前分支

  - 因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以commit就是往master分支上提交更改。
  - 可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。一旦提交完后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的。即：nothing to commit (working directory clean)。

- **其他操作**

  ①用“git diff HEAD -- filename”命令可以查看工作区和暂存区里面最新版本的区别。

  ②新建过撤销未add： git checkout -- 文件名

  ③撤销已add未commit：先git reset [HEAD] 文件名，再 git checkout -- 文件名

  ④撤销已add已commit：git reset --hard HEAD^    回退一个版本

### 二、Git本地库实战

- 常用命令预览：

  | 命令名称                                                     | 命令作用                 |
  | ------------------------------------------------------------ | ------------------------ |
  | git init                                                     | 初始化本地库             |
  | git config --global user.name 用户名       我们也可以不设置 --global | 设置用户签名的用户名部分 |
  | git config --global user.email 邮箱         为单个项目设置用户名和邮箱 | 设置用户签名的邮箱部分   |
  | git status                                                   | 查看本地库状态           |
  | git add 文件名                                               | 添加到暂存区             |
  | git commit -m "日志信息" 文件名                              | 提交到本地库             |
  | git reflog                                                   | 查看历史记录             |
  | git reset --hard 版本号                                      | 版本穿梭                 |

#### 2.1 实战（初始化版本库）

-  要使用Git对我们的代码进行版本控制，首先需要获得Git仓库，获取Git仓库通常有**两种方式**：
  - 在**本地初始化一个Git仓库**
  - 从远程仓库克隆
- 本地初始化操作**步骤**：
  - 创建目录（用作本地版本库）,例如：D:\DevRepository\GitRepository\oa，oa表示办公自动化项目名称
  - 当前目录打开Git Bash窗口，初始化仓库     命令：**git init**
  - 查看当前目录产生 **.git** 隐藏文件夹

#### 2.2  实战(新建\提交\状态)

 	0.  新建文件：
      - 命令：touch a.txt
      - 命令：vim a.txt

1. 查看文件状态
   - 命令：  **git status**
     - On branch master ：表示主分支
     - Untracked files：表示未跟踪状态
   - 说明：   Git工作目录下的文件状态信息
     -  Untracked 未跟踪（未被纳入版本控制）
     -  Tracked 已跟踪（被纳入版本控制）
     -  Unmodified 未修改状态
     -  Modified 已修改状态
     -  Staged 已暂存状态
   -  这些文件的状态会随着我们执行Git的命令发生变化
     - 红色表示新建文件或者新修改的文件,都在工作区.
     - 绿色表示文件在暂存区
     - 新建的文件在工作区，需要添加到暂存区并提交到仓库区
2. 添加到暂存区
   - **git add** <文件名称>
     - 只是增加到栈空间（index文件）中，还没有添加到本地库中。初始化时没有这个index文件。这还是一个新文件，需要将栈空间文件提交到本地仓库。
   -   **git add .**      添加项目中所有文件
   -   **git add hello.txt**       添加未存在文件会出错：fatal: pathspec 'hello.txt' did not match any files
3. 撤销暂存区的文件
   -   **git reset ** <**文件名称**>
   -  撤销后，查看文件状态（git status）文件由绿色变为红色
4. 将暂存区文件提交到本地库
   -   **git commit**     执行命令时需要填写提交日志，进入编辑模式
   -   **git commit –m “注释内容”**   
     -  直接用-m参数指定日志内容，推荐
     - commit 会生成一条版本记录，add只是添加暂存区，不会生成版本记录，建议多次add后，一次性commit，避免每次add都commit产生版本信息爆炸。
   -  **git commit -am "注释内容"**
     - 代码编辑完成后即可进行 add 和 commit 操作
     - 提示：添加和提交合并命令
   - git commit -m用于提交暂存区的文件，git commit -am用于提交跟踪过的文件。

#### 2.3 实战(查看日志)

-  **git log**
-  **git log a.txt**    
  - 查看文件日志(查看所有日志或某个文件日志)
  - q退出
-  **git log --pretty=oneline**    如果日志很多,可以在一行显示
-  **git reflog**        查看历史操作

#### 2.4  实战(回退\穿梭\撤销)

- 回退到历史版本
  - 命令
    - git reset --hard HEAD^       一次回退一个版本，一个^代表一个版本数量
    - git reset --hard HEAD~n      回退n次操作
- 版本穿梭
  - 命令
    - git reflog a.txt   查看历史操作
    - git reset   --hard 版本号   回到最新的版本
-  撤销：
  - 未add，未commit
    - vim修改文件，没有add和commit，进行撤销
    - 命令： git checkout -- a.txt   撤销修改(还原原来的文件)
  - 已add，未commit
    -  vim 修改文件，添加add，但没提交commit，进行撤销
    - 命令：
      -  git add a.txt
      -  **git reset**      软回退    查看文件内容：cat a.txt     查看日志：git reflog a.txt

#### 2.5 实战(删除)

​	①  手动拷贝图片java.jpg到工作空间目录，并查看目录列表：ls -l

​	②  添加：git add java.jpg

​	③  提交：git commit -m "新建图片" java.jpg

​	④  删除图片：rm java.jpg

​	⑤  添加：git add java.jpg

​	⑥  提交：git commit -m "新建图片" java.jpg

​	⑦  回退：git reset --hard HEAD^

​	⑧  文件不是被删除了吗？怎么又回来啦！呵呵…

​	⑨  处处留痕：git reflog



## Git使用记录



#### 一、设置全局用户签名

- 安装完成后，在任意的文件目录下，右键都可以开打Git的命令行窗口——Git Bash Here
- Git是分布式版本控制系统，所以需要填写用户名和邮箱作为一个标识——即：用户签名
-  说明
  - 签名的作用是区分不同操作者身份。用户的签名信息在每一个版本的提交信息中能够看到，以此确认本次提交是谁做的。
  - 这里设置用户签名和将来登录GitHub（或其他代码托管中心）的账号没有任何关系。

- 命令：

  ```
  git config --global user.name "用户名"
  git config --global user.email "用户邮箱"
  
  --global 表示全局属性，所有的git项目都会共用属性
  查看配置信息：git config --list
  ```

#### 二、使用ssh进行克隆 

#### 步骤：

- 打开git bash.exe  输入ssh-keygen -t rsa -C "1608576814@qq.com" 

  使用你的邮箱用ssh-keygen命令创建密码对。

- 打开github或者gitlab账户，找到SSH Keys 进行设置

  将剪贴板内容粘贴到内容框中，title可以用默认的邮箱名字，最后点击add

  这就代表这个用户被远程仓库所承认了，接下来就可以克隆仓库了

- 可以先选择一个空文件夹用来储存克隆下来的项目，然后鼠标右键选择git bash here，然后输入命令 git clone + 自己Git库的地址

  ```
  git clone  git@gitlab.code-nav.cn:root/lovefinder-frontend.git
  git clone  git@gitlab.code-nav.cn:root/lovefinder-backend.git
  
  ```

- 完成克隆，检查代码

#### 创建SSH的目的：

> 创建SSH  KEY(这个作用是来识别你的电脑，相当于人的身份证号)，在你的c盘用户目录下面看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：$ ssh-keygen -t rsa -C "你的邮箱地址"，由于这个Key也不是用于军事目的，所以也无需设置密码。
> 如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。