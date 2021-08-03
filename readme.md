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

// 拉去远程主机origin的远程仓库的xx分支内容，合并到本地（应该是合并到本地仓库版本，有冲突，就手动解决冲突）
git pull origin 远程仓库的xx分支  // 将远程主机origin的远程仓库xx分支拉取合并到本地分支
git pull origin 远程仓库的xx分支:xx分支  // 将远程主机origin的远程仓库xx分支拉取合并到指定分支
（同类型的还有fetch，但是这个命令只拉取，不合并。这里不作描述）

// （检查无误之后）提交“版本库的最新版本”到远程主机origin（这里的origin指代远程主机，这在一开始就有建立本地与远程主机的联系所形成的）的远程仓库的xx分支
git push -u origin xxx分支
git push -u origin xxx分支:xxx分支。 // 提交指定xx分支到远程主机origin的xxx分支
```

### 分支信息：

```
每一个分支都有自己独立的目录树，新建分支会commit一份当前分支版本哭最新的内容作为自己的版本库的first commit

默认情况下，新建仓库，建立联系，本地的分支默认是master

分支可以合并，也可以删除（本地、远程都可以）
```

### 分支管理：

```
git branch  // 查看所有分支

git branch xxx   // 创建xxx分支，但是不切换到该分支

git checkout -b xxx  //创建并切换到xxx分支

git checkout xxx // 切换到xxx分支
```



### 补充命令：

```
// 查看配置信息
git config  // 配置命令的相关命令文档
git config --list // 查看配置信息
```

