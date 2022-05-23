## GitLabe搭建

 <br/>

## 一、部署社区版gitlab

1、 安装gitlab的依赖项

```
yum install -y curl openssh-server openssh-clients postfix cronie policycoreutils-python
```

 // 10.x以后开始依赖policycoreutils-python

 <br/>

## 二、 启动postfix，并设置为开机启动 

```
systemctl start postfix
systemctl enable postfix
```

 <br/>

## 三、设置防火墙 

```
setenforce 0 
sed -i '7d' /etc/selinux/config
sed -i '7i SELINUX=disabled' /etc/selinux/config

systemctl stop firewalld

systemctl disable firewalld
```

 <br/>

## 四、获取gitlab的rpm包

国内链接：

wget https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/gitlab-ce-12.1.6-ce.0.el7.x86_64.rpm

或者官方连接：

官方下载：https://packages.gitlab.com/gitlab/gitlab-ce/

 <br/>

## 五、安装rpm包

```bash
[root@localhost ~]# rpm -ivh gitlab-ce-12.1.6-ce.0.el7.x86_64.rpm
```

根据提示，继续执行指令配置gitlab。

 <br/>

## 六、修改配置文件

将external_url变量的地址，修改自己需要的域名，或者是自己本机的 IP ，如 external_url 'http://172.25.55.54' ,最后执行：wq
```bash
[root@localhost ~]# vim /etc/gitlab/gitlab.rb
```

如果修改了配置文件，还需要重新加载配置内容。
```bash
[root@localhost ~]# gitlab-ctl reconfigure
[root@localhost ~]# gitlab-ctl restart
```

查看gitlab版本
```bash
[root@localhost ~]# head -1 /opt/gitlab/version-manifest.txt   
```
  

 <br/>


## 七、访问Gitlab

```
输入本机IP地址即可访问     默认账户root 密码需要自己定义

```

获取密码
```bah
Notes:
Default admin account has been configured with following details:
Username: root
Password: You didn't opt-in to print initial root password to STDOUT.
Password stored to /etc/gitlab/initial_root_password. This file will be cleaned up in first reconfigure run after 24 hours.

```


 如访问502 可以看下是不是 80 和 8080 端口被占用了，如果端口被占用，可以参考: [8080 端口被占用,访问 502](#jump)


<br/>  
  

## 九、Gitlab汉化WEB界面

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


<br/>

<br/>

<br/>

<br/>

## 常见问题：
问题一：<span id="jump">8080端口被占用，访问502</span>

 ** 解决方案：**

修改 puma.rb 配置
```bash
vim /var/opt/gitlab/gitlab-rails/etc/puma.rb
```
```bash

bind 'unix:///var/opt/gitlab/gitlab-rails/sockets/gitlab.socket'

bind 'tcp://127.0.0.1:8092' ## 默认8080,修改为自己服务不冲突的端口,这里我改为8092

directory '/var/opt/gitlab/gitlab-rails/working'

修改gitlab.rb 配置

```bash
### Advanced settings
# puma['listen'] = '127.0.0.1'
puma['port'] = 8092 # 默认8080,去掉注释修改为同 `puma.rb` 中配置的端口:8092
# puma['socket'] = '/var/opt/gitlab/gitlab-rails/sockets/gitlab.socket'
# puma['somaxconn'] = 1024

```

生效配置，重启GitLab服务
```bash
gitlab-ctl reconfigu
gitlab-ctl restart
```

检查端口情况
```bash
netstat -tunlp | grep 8092
```
