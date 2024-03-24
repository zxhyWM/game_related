#### 一、Git

git是一种分布式的版本控制软件

下载git到本地后，初始设置

##### 1、全局设置用户名和邮箱，之后每一次提交都会使用

```bash
git config --global user.name "用户名"

git config --global user.email "邮箱"
```

##### 2、查看更改user信息

```bash
~/.gitconfig
```

##### 3、git显示颜色配置

```bash
git config --global color.ui auto
```



#### 二、GitHub

##### 1、首先创建拥有一个账户

##### 2、创建SSH Key

设置SSH Key（非对称公钥加密），生成的公钥放到github的网站上，私钥放在自己的电脑上，每当将文件上传到github上时，服务器就会用公钥与你给出的私钥进行验证，验证用户操作

```bash
ssh-keygen -t rsa -C "注册github时使用的邮箱"
#提示输入密码，设置或者直接回车不设置
```

##### 3、查看SSH Key

```bash
cd ~/.ssh
ls
#带有.pub是公钥，没有的是对应的私钥
```

##### 4、将公钥上传到对应的GitHub账号中

设置--ssh。配置名字，注意，一个GitHub账号可以拥有多个公钥（可以实现多台设备对同一个github账户操作）

##### 5、验证绑定是否成功

```bash
ssh -T git@github.com
```

到此，github配置完成，后续可以进行操作

#### 三、github相关操作

##### 1、克隆远程仓库，本地选取文件夹位置

```bash
#全部
git clone "仓库ssh/https等链接"
#拉取指定分支
git clone -b "分支名称" "仓库地址"
```

##### 2、托管仓库/关联远程

要么将远程仓库拉取到本地，要么本地文件夹初始化作为本地仓库

```bash
git init
#关联本地仓库与github仓库
git remote add origin git@github.com:"用户名"/"仓库名".git#origin是远程仓库名称代号

#同步仓库到本地/拉取远程仓库
git fetch origin #合并前先询问
git pull origin #直接合并到本地
```

##### 3、提交文件

```bash
#文件先从工作区提交到缓存区
git add "具体的文件"#hello.py
git add .#工作区全部

#从暂存区提交到本地仓库，并写明备注最好
git commit -m "修改什么。。。"

#提交成功，可以查看提交日志，知道提交人、提交id（为了出错后可以回退）
git log

#从本地推送到远程仓库（前提是本地仓库要先与远程关联）
git push <origin>#推送本地仓库到远程仓库
```

##### 4、查看工作区和暂存区的差别

```bash
git diff 

#
git diff "commitid"
```

##### 5、移除文件

从暂存区域移除，下一次提交时，该文件就不再纳入版本管理了。 

如果要删除之前修改过或已经放到暂存区的文件，则必须使用强制删除选项 -f。

另外一种情况是，想让文件保留在磁盘，但是并不想让 Git 继续跟踪。 当你忘记添加 `.gitignore` 文件，不小心把一个很大的日志文件或一堆 `.a` 这样的编译生成文件添加到暂存区时，这一做法尤其有用。 为达到这一目的，使用 `--cached` 

```bash
git rm "文件名"
```

##### 6、工作区的状态

```bash
git status
```

##### 7、重命名文件

```bash
git mv file_from file_to
```

##### 8、撤消操作

```
#带有 --amend 选项的提交命令来重新提交，也就是用一个新的提交替换旧的提交
git commit --amend


```

