## git安装和命令

### 安装git

```
yum install -y git
```

### 创建git用户和邮箱

```
git config --global user.name "hjz"
git config --global user.email hjz@example.com
```



### 一、创建git用户

```shell
git config --global user.name "hjz"
```



### 二、创建用户邮箱

```shell
git config --global user.email hjz@example.com
```

- 注意git config命令的--global参数，用了这个参数，表示这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。



### 三、创建版本库

```shell
root@hdpserver1 data]# mkdir -p /data/learngit

[root@hdpserver1 data]# cd /data/learngit/
[root@hdpserver1 learngit]# git init
初始化空的 Git 版本库于 /data/learngit/.git/
[root@localhost data]# ls -a
.  ..  .git
```

- 可以发现当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。



### 四、添加文件到版本库，并提交

```shell
[root@hdpserver1 learngit]# git add file1.txt    #添加单个文件到 版本库的暂存区
[root@hdpserver1 learngit]# git add file2.txt file3.txt   #一次添加多个文件 版本库的暂存区
```

```shell
[root@hdpserver1 learngit]# git commit -m "add 3 files."   #将所有暂存区的文件提交版本库的master分支
```





### 五、撤消文件的修改，

撤消工作区的修改

```shell
[root@hdpserver1 learngit]# git checkout -- readme.txt
```

- 命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销

撤消暂存区的修改

```shell
[root@hdpserver1 learngit]# git reset HEAD readme.txt
重置后撤出暂存区的变更：
M	readme.txt
```



### 六、删除文件

先在工作区中删除

```
[root@hdpserver1 learngit]# rm -f readme.txt
```

再到暂存区中删除

```
[root@hdpserver1 learngit]# git rm readme.txt
```

再执行提交

```
[root@hdpserver1 learngit]# git commit -m "delete readme.txt"
```



### 七、查看工作区的状态

可以查看有哪些文件做了修改，（红色的表示被修改了，未执行git add）（绿色表示执行了git add但未执行git git commit）

```shell
[root@hdpserver1 learngit]# git status
# 位于分支 master
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#       修改：      readme.txt       #这个文件表示被修改过

```



### 八、查看文件修改内容

```shell

[root@hdpserver1 learngit]# git diff readme.txt

diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt    #这个表示是原始文件
+++ b/readme.txt    #这个表示被修改后的文件
@@ -1,2 +1,2 @@     #1，2表示第一个字段的第二行内容被修改了
-Git is a version control system.    #这是原始内容
+Git is a distributed version control system.  #这是被修改后的内容，可以和原始内容对比，确定被修改了哪些
```



### 九、修改仓库文件

```shell
[root@hdpserver1 learngit]# vim readme.txt   #文件修改后，确定没问题了，都需要执行下面两条命令
[root@hdpserver1 learngit]# git add file1.txt   
[root@hdpserver1 learngit]# git commit -m "modify 3 files."

```



### 十、Git查看版本历史修改记录

```shell
[root@hdpserver1 learngit]# git log      #查看版本历史修改记录
commit 06bb1548357a20dce968bf4b83d136f97ee9b805
Author: gaojf <gao.junfeng@aliyun.com>
Date:   Tue Aug 20 17:29:32 2019 +0800   #这是提交的时间
    append GPL     #这是注释信息，所以每次提交时最好注释清楚

commit fc57b4680dfa5b811daa93185e959c1b4b4bcf06
Author: gaojf <gao.junfeng@aliyun.com>
Date:   Tue Aug 20 17:16:15 2019 +0800
    add distributed

commit 61f5eb14e43aa7c90d878e9bcc1f7db22f997674
Author: gaojf <gao.junfeng@aliyun.com>
Date:   Tue Aug 20 11:26:41 2019 +0800
    wrote a readme fil
```

如果git log查看不到的（结果显示，第一个字段就是commit的ID号）

```shell 
[root@hdpserver1 learngit]# git reflog
083cedd HEAD@{0}: commit: add okokooo
98e1009 HEAD@{1}: pull: Merge made by the 'recursive' strategy.
a29010f HEAD@{2}: commit: test1.txt info
384c333 HEAD@{3}: checkout: moving from dev1 to dev
```





### 十一、版本回退

回退到上一个版本（HEAD后面加上一个“^”号）

```shell
[root@hdpserver1 learngit]# git reset --hard  HEAD^     
HEAD 现在位于 fc57b46 add distributed  #转换后的ID
```

回退到上上一个版本（HEAD后面加上两个“^”号）

```shell 
[root@hdpserver1 learngit]# git reset --hard  HEAD^ 
```

退回到指定版本（先通过git log查看，然后复制commit后面的ID号，只需要复制前面7-8位即可）

```shell
[root@hdpserver1 learngit]# git log      #先查看版本历史修改记录
commit 06bb1548357a20dce968bf4b83d136f97ee9b805  # ID
Author: gaojf <gao.junfeng@aliyun.com>
Date:   Tue Aug 20 17:29:32 2019 +0800   
    append GPL     

commit fc57b4680dfa5b811daa93185e959c1b4b4bcf06
Author: gaojf <gao.junfeng@aliyun.com>
Date:   Tue Aug 20 17:16:15 2019 +0800
    add distributed
    
[root@hdpserver1 learngit]# git reset --hard fc57b468
HEAD 现在位于 fc57b46 add fc57b4680d  #这里会提示转换后的ID
```



### 十二、创建一个新仓库 

```bash
git clone git@172.25.55.54:root/www.git      #在gitlab上先创建好项目www,再将其项目克隆下来
cd www 
touch README.md                 
git add README.md git commit -m "add README" 
git push -u origin master
```

### 十三、推送现有文件夹到gitlab

```bash
cd existing_folder               #进入现有文件夹
git init                         #初始化文件夹
git remote add origin git@172.25.55.54:root/www.git    #添加远程gitlab项目地址（需要先在gitlab上创建项目）
git add .                        #添加当前文件下的所有文件
git commit -m "Initial commit"   #提交
git push origin master           #上传到远程gitlab项目www.git中的mater分支

#如果提交远程报错可以使用下面命令解决
git push -f  进行解决错误
如果上述解决方式不管用也可以输入：git pull --rebase origin master 之后再进行git push 即可
```

### 十四、推送现有的 Git 仓库

```
cd existing_repo
git remote rename origin old-origin
git remote add origin git@172.25.55.54:root/www.git
git push -u origin --all
git push -u origin --tags
```









## git常用命令

### **一、本地操作：**

#### **1.其它**

```
git init：初始化本地库

git status：查看工作区、暂存区的状态

git add <file name>：将工作区的“新建/修改”添加到暂存区

git rm --cached <file name>：移除暂存区的修改

git commit <file name>：将暂存区的内容提交到本地库

tip：需要再编辑提交日志，比较麻烦，建议用下面带参数的提交方法

git commit -m "提交日志" <file name>：文件从暂存区到本地库
```

 

#### **2.日志**

```
git log：查看历史提交

tip：空格向下翻页，b向上翻页，q退出

git log --pretty=oneline：以漂亮的一行显示，包含全部哈希索引值

git log --oneline：以简洁的一行显示，包含简洁哈希索引值

git reflog：以简洁的一行显示，包含简洁哈希索引值，同时显示移动到某个历史版本所需的步数
```

 

#### **3.版本控制**

```
git reset --hard 简洁/完整哈希索引值：回到指定哈希值所对应的版本

git reset --hard HEAD：强制工作区、暂存区、本地库为当前HEAD指针所在的版本

git reset --hard HEAD^：后退一个版本　　

tip：一个^表示回退一个版本

git reset --hard HEAD~1：后退一个版本

tip：波浪线~后面的数字表示后退几个版本
```

 

#### **4.比较差异**

```
git diff：比较工作区和暂存区的**所有文件**差异

git diff <file name>：比较工作区和暂存区的**指定文件**的差异

git diff HEAD|HEAD^|HEAD~|哈希索引值 <file name>：比较工作区跟本地库的某个版本的**指定文件**的差异
```

 

#### **5.分支操作**

```
git branch -v：查看所有分支

git branch -d <分支名>：删除本地分支

git branch <分支名>：新建分支

git checkout <分支名>：切换分支

git merge <被合并分支名>：合并分支

tip：如master分支合并 hot_fix分支，那么当前必须处于master分支上，然后执行 git merge hot_fix 命令

tip2：合并出现冲突

　　　　①删除git自动标记符号，如<<<<<<< HEAD、>>>>>>>等

　　　　②修改到满意后，保存退出

　　　　③git add <file name>

　　　　④git commit -m "日志信息"，此时后面不要带文件名
```



### 二、本地库跟远程库交互 

```
git clone <远程库地址>：克隆远程库

　　功能：①完整的克隆远程库为本地库，②为本地库新建origin别名，③初始化本地库

git remote -v：查看远程库地址别名

git remote add <别名> <远程库地址>：新建远程库地址别名

git remote rm <别名>：删除本地中远程库别名

git push <别名> <分支名>：本地库某个分支推送到远程库，分支必须指定

git pull <别名> <分支名>：把远程库的修改拉取到本地

tip：该命令包括git fetch，git merge

git fetch <远程库别名> <远程库分支名>：抓取远程库的指定分支到本地，但没有合并

git merge <远程库别名/远程库分支名>：将抓取下来的远程的分支，跟当前所在分支进行合并

git fork：复制远程库

tip：一般是外面团队的开发人员fork本团队项目，然后进行开发，之后外面团队发起pull  request，然后本团队进行审核，如无问题本团队进行merge（合并）到团队自己的远程库，整个流程就是本团队跟外面团队的协同开发流程，Linux的团队开发成员即为这种工作方式。


```



## GitLabe搭建

### 一、部署社区版gitlab

1、 安装gitlab的依赖项

```
yum install -y curl openssh-server openssh-clients postfix cronie policycoreutils-python
```

 // 10.x以后开始依赖policycoreutils-python

### 二、 启动postfix，并设置为开机启动 

```
systemctl start postfix
systemctl enable postfix
```

### 三、设置防火墙 

```
setenforce 0 
sed -i '7d' /etc/selinux/config
sed -i '7i SELINUX=disabled' /etc/selinux/config

systemctl stop firewalld

systemctl disable firewalld
```

### 四、获取gitlab的rpm包

国内链接：

wget https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/gitlab-ce-12.1.6-ce.0.el7.x86_64.rpm

或者官方连接：

官方下载：https://packages.gitlab.com/gitlab/gitlab-ce/

### 五、安装rpm包

```bash
[root@localhost ~]# rpm -ivh gitlab-ce-12.1.6-ce.0.el7.x86_64.rpm
```

根据提示，继续执行指令配置gitlab。

### 六、修改配置文件

```bash
[root@localhost ~]# vim /etc/gitlab/gitlab.rb
```

将external_url变量的地址修改为gitlab所在centos的ip地址，也就是本机的IP地址 external_url 'http://172.25.55.54'  。最后执行：wq

```bash
[root@localhost ~]# gitlab-ctl reconfigure
[root@localhost ~]# gitlab-ctl restart
```

如果修改了配置文件，还需要重新加载配置内容。

```bash
[root@localhost ~]# head -1 /opt/gitlab/version-manifest.txt
```

查看gitlab版本  

### 七、访问Gitlab

```
输入本机IP地址即可访问     默认账户root 密码需要自己定义
```

### 九、Gitlab汉化WEB界面

项目地址：https://gitlab.com/xhang/gitlab

下载最新的汉化包：

git clone https://gitlab.com/xhang/gitlab.git

也可以指定版本：

git clone https://gitlab.com/xhang/gitlab.git -b  v12.1.6-zh

查看该汉化补丁的版本：

cat gitlab/VERSION

覆盖汉化：

将下载下来的汉化版目录下所有内容拷贝到gitlab指定路径下，cp前面加的“\”作用：覆盖时不提示

```
[root@localhost ~]# \cp -rf gitlab/* /opt/gitlab/embedded/service/gitlab-rails/
```

回到web界面，点击右上角的选项，选择Settings，然后选择右边菜单栏的Preferences，下滑找到Language，选择简体中文

