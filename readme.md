# git操作

> 学习一下git操作

### 入门第一步：

```
// 假如使用“ssh加密的方式”来，传输本地git仓库和远程仓库的内容的话，就要确定本机与远程主机、远程仓库的关系

需要在本地生成ssh钥匙，分公钥、密钥。生成后密钥不需要理会，复制公钥信息，配置在“想要的远程主机的配置ssh上”（这里指的是github、gitee的配置ssh上）

// 步骤一：
控制台直接输入“ssh-keygen -t rsa -C "github/gitee上面绑定的邮箱"”
// 步骤二：
步骤一，会在当前用户根目录下生成“.ssh”目录，里面有id_rsa（密钥）、id_rsa.pub（公钥），打开复制公钥文件信息，配置到对应github/gitee的ssh配置项上 
```

  **配置截图：**

![image-20210803160149121](/Users/zhangyuewei/Library/Application Support/typora-user-images/image-20210803160149121.png)

### 建立本地与远程仓库的联系：

> 温馨提示：仓库一开始，要在官网手动建立。

```
// 方法一：
git remote add origin 远程仓库地址    // 这样子就能建立俩者间的关系，确立一个origin对应一个远程仓库地址，方便之后正常的fetch、pull、push

注意一点：git clone 远程仓库地址  // 不需要建立关系

// 查看本地远程库关联信息
git remote -v

// 有冲突，删除该远程库关联
git remote rm origin
```

### 正常操作下：

> 温馨提示：首次提交可能需要绑定下仓库上对应的名字、邮箱信息
>
> 设置提交代码时的用户信息：
>
> ```
> // 如果去掉 --global 参数只对当前仓库有效。
> git config --global user.name "runoob"
> git config --global user.email test@runoob.com
> ```

```
// 将工作区内容，添加到暂存区
git add . 

// 清空暂存区，提交到版本库（仓库）
git commit -m '提交说明'

“git commit -a” ：类似于操作了“git add .”,并提交到本地仓库，会提示要写“操作说明”；
“git commit -a” ：只对添加了索引的文件（即执行过“git add xxx”的文件）有效，因为没有对文件进行index索引编号，所以对于新添加的文件，直接“git commit -a”，不会将新文件追加到暂存区，也不会提交上去的；

// 拉去远程主机origin的远程仓库的xx分支内容，合并到本地（应该是合并到本地仓库版本，有冲突，就手动解决冲突）
git pull origin 远程仓库的xx分支  // 将远程主机origin的远程仓库xx分支拉取合并到本地分支
git pull origin 远程仓库的xx分支:xx分支  // 将远程主机origin的远程仓库xx分支拉取合并到指定分支
（同类型的还有fetch，但是这个命令只拉取，不合并。这里不作描述）

// （检查无误之后）提交“版本库的最新版本”到远程主机origin（这里的origin指代远程主机，这在一开始就有建立本地与远程主机的联系所形成的）的远程仓库的xx分支
// 注意：本地分支与远程分支名称必须要一致，才能提交成功。一一对应的关系
git push origin xxx分支   // 提交“xxx分支”到远程主机origin的“xxx分支”
git push origin xxx分支:xxx分支。 // 提交“指定xxx分支”到远程主机origin的“xxx分支”
git push -u origin xxx分支 //  提交“xxx分支”到远程主机origin的“xxx分支”，（“-u”指的是对于每个最新或成功推送的分支，添加上游【跟踪】引用，【一般】在首次推送的时候需要建立，之后就可以在对应的分支下直接“git push”来实现本地分支与远程仓库分支的一一对应关系传输）
git push -u origin xxx分支:xxx分支
```

### 分支信息：

```
每一个分支都有自己独立的目录树，新建分支会commit一份当前分支版本哭最新的内容作为自己的版本库的first commit

默认情况下，新建仓库，建立联系，本地的分支默认是master

不同分支可以合并数据
可以删除分支（本地、远程都可以）
```

### 分支管理：

```
git branch  // 查看所有本地分支
git branch -r // 查看所有远程分支
git branch -a // 查看所有分支

git branch xxx   // 创建xxx分支，但是不切换到该分支
git branch -d xx分支  // 删除本地xx分支
git push origin --delete [branchName]  // 删除远程仓库的分支
git branch --unset-upstream  // 假如本地有这个分支，远程删除了。可以通过该命令恢复被删除的远程分支
git branch | grep 'branchName' // 分支模糊查找
git fetch -p // 清理本地无效分支(远程已删除本地没删除的分支)，没有效果的原因是fetch的特性，使用“git pull -p”来清理本地无效分支

git checkout -b xxx  //创建并切换到xxx分支
git checkout xxx // 切换到xxx分支

// 合并分支
git merge
git pull = git fetch + git merge    // 解释
git merge xx分支   // 将xx分支内容合并到当前分支，会提示要写“操作说明”

```

**错误提交时**

> 回退版本库（当有“错误提交”的时候，会很庆幸懂得这个知识点）
>
> ```
> git reset  [HEAD]  // 命令用于回退版本，可以指定退回某一次提交的版本（HEAD可以看成指针一样的东西，这里命令指的是，回退到HEAD指定的版本去）
> // 不同修饰命令，带来的区别：
> git reset --soft 版本库ID   //仅仅只是撤销已提交的版本库，不会修改暂存区和工作区
> git reset --mixed 版本库ID  //仅仅只是撤销已提交的版本库和暂存区，不会修改工作区（不写修饰符，默认就是--mixed）
> git reset --hard 版本库ID   //彻底将工作区、暂存区和版本库记录恢复到指定的版本库
> ```



### 补充命令：

> 补充一些会用到的命令

```
// 查看配置信息
git config  // 配置命令的相关命令文档
git config --list // 查看配置信息

// 查看当前分支的状态
git status
分支状态：‘’=unmodified（未修改）;M=modified（修改）;A=added（添加）;D=deleted（删除）;R=renamed（重命名）;C=copied（复制）;U=updated but unmerged（更新但为合并）


// 查看提交到历史数据
git log
git log --oneline // 查看简介版本
（更多修饰命令，查看官网）

// 查看指定文件的修改记录
git blame xxx

// 删除暂存区指定文件（以下俩个命令，如果要删除多个文件使用空格分隔文件名称即可。）
git rm --cached <文件名> //将暂存区中的内容删除，工作区中对应的文件并不会受到影响。
git rm -f <文件名> // 不但将暂存区中的内容删除，并且工作区中对应的文件也会被删除。

// 清空暂存区
rm .git/index //暂存区实质是.git目录下的index文件，只要将此文件删除，那么就可以认为暂存区被清空。


// 差异对比（比较文件的不同）
git diff [file] // 显示暂存区与工作区的差异 
git diff --cached [file] // 显示暂存区与最近一次commit的差异 
git diff --staged [file] // 显示暂存区与最近一次commit的差异 
git diff [commit1]...[commit2] //比较俩次提交的差异



// 查看文件内容（可在合并有冲突的情况下，查看文件，有冲突标记）
cat xxx文件
// 编辑文件
vim xxx文件

// 在 Git 中，我们可以用 git add 要告诉 Git 文件冲突已经解决
```

> 一般情况下，多人开发，至少设置三个分支。如master分支作为项目最终发布版本分支；dev分支作为多人开发最终版本分支；zlw分支（以自己名字命名的分支）作为个人开发分支；



> 对于重要的分支，可以设置分支保护，不允许本地操作该分支进行上传到该远程（上游）分支；

