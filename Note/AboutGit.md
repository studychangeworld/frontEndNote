# About Git

## 一台电脑使用多个GitHub账户   

github使用SSH与客户端连接。将公钥保存至github，每次连接时SSH客户端发送本地私钥（默认~/.ssh/id_rsa）到服务端验证。
一台电脑如果不做配置，仍然会发生默认的私钥和服务端的公钥进行配对，验证自然就无法通过。
要实现多帐号下的SSH key切换在客户端做一些配置即可。

### 解决方案

1. 生成不同的私钥，公钥对。
```$xslt
ssh-keygen -t rsa -f ~/.ssh/id_rsa_second -C "second@mail.com"
```
2.  添加到SSH agent
```$xslt
ssh-keygen -t rsa -f ~/.ssh/id_rsa_second -C "second@mail.com"
```
该命令如果报错：Could not open a connection to your authentication agent.无法连接到ssh agent，可执行ssh-agent bash命令后再执行ssh-add命令。

3. 在~/.ssh目录创建config文件

```
# first.github (first@gmail.com)
Host github.com
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa

# second (second@gmail.com)
Host github-second
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa_second

```
Host随意即可，方便自己记忆，后续在添加remote是还需要用到
```$xslt
git remote add origin git@github-second:second/test.git
# 并非原来的git remote add origin git@github.com:second/test.git
```

### 进入clone下来的项目--->.git --->config--->"url = git@github-second:xxx/xxx.git"


###解决 idea无法同步“Could not read from remote repository的问题”
```$xslt
file|
    v-->settings|
                v-->Version Control|
                                   v-->Git
Here change SSH executable from Built-in into Native
```


注意：github根据配置文件的user.email来获取github帐号显示author信息，所以对于多帐号用户一定要记得将user.email改为相应的email(second@mail.com)。

##### 切换账户信息：
```$xslt
git config user.name
git config user.email

git config --global user.name "Your_username"
git config --global user.email "Your_email"

```
参考：
[http://stormzhang.com/other/2013/10/16/github-multiply-ssh-key/](http://stormzhang.com/other/2013/10/16/github-multiply-ssh-key//)    
[https://www.cnblogs.com/xjnotxj/p/5845574.html](https://www.cnblogs.com/xjnotxj/p/5845574.html)
----

##### 使用：
```
1、clone到本地

(1)原来的写法：
$ git clone git@github.com: one的用户名/learngit.git
(2)现在的写法：

$ git clone git@one.github.com: one的用户名/learngit.git

$ git clone git@two.github.com: two的用户名/learngit.git


2、记得给这个仓库设置局部的用户名和邮箱：

$ git config user.name "one_name" ; git config user.email "one_email"

$ git config user.name "two_name" ; git config user.email "two_email"

```

#### git】全局配置和单个仓库的用户名邮箱配置
Git全局配置和单个仓库的用户名邮箱配置

学习git的时候, 大家刚开始使用之前都配置了一个全局的用户名和邮箱

$ git config --global user.name "github's Name"

$ git config --global user.email "github@xx.com"

$ git config --list

 

如果你公司的项目是放在自建的gitlab上面, 如果你不进行配置用户名和邮箱的话, 则会使用全局的, 这个时候是错误的, 正确的做法是针对公司的项目, 在项目根目录下进行单独配置    

$ git config user.name "gitlab's Name"

$ git config user.email "gitlab@xx.com"

$ git config --list

 git config --list查看当前配置, 在当前项目下面查看的配置是全局配置+当前项目的配置, 使用的时候会优先使用当前项目的配置



### Github Git彻底删除历史提交记录的方法
1.git log命令获取提交的历史找到需要回滚到的提交点 

2.git reset –-hard commit_hash 

3.git push origin HEAD –-force







