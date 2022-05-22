## 安装 Jenkins：
1、安装依赖软件 JDK
  ```bash
  yum install java-1.8.0-openjdk* -y
  ```
  使用 war 包安装 Jenkins（自行去官网下载 [https://www.jenkins.io/download/](https://www.jenkins.io/download/)
  
  ```bash
[root@localhost local]# mkdir jenkins    #创建jenkins目录
[root@localhost local]# cd jenkins/  #进入安装目录
[root@localhost jenkins]# rz  #上传安装包
jenkins.war
  ```
启动 jenkins
 ```bash
 [root@localhost jenkins]# java -jar jenkins.war     #执行java -jar jenkins.war 即可前台启动
 ```
 有以下输出，表示启动成功
 
