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

