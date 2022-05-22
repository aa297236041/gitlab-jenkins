## 安装依赖软件 JDK
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
## 启动 jenkins
 ```bash
 [root@localhost jenkins]# java -jar jenkins.war     #执行java -jar jenkins.war 即可前台启动
 ```
  有以下输出，表示启动成功
 
 ![](./media/jenkins启动成功.png)
 
 
 ## 解锁jenkins
 
 ![](./media/解锁jenkins.png)

## 安装配置(选择自定义配置)

![](./media/图片1.png)
![](./media/图片2.png)

    如果在安装插件的途中觉得安装忒慢，可以尝试更换他的镜像源，因为Jenkins默认使用的是国外镜像源所以会有些慢，这里我们可以更改为清华镜像源
    ```bash
    [root@Cengtos7-4 .jenkins]# sed -i 's/http:\/\/www.google.com\//http:\/\/www.baidu.com\//g' /root/.jenkins/updates/default.json
    [root@Cengtos7-4 .jenkins]# sed -i 's#http://updates.jenkins-ci.org/download/#https://mirrors.tuna.tsinghua.edu.cn/jenkins/#g' /root/.jenkins/updates/default.json
    ```
  创建用户后，默认下一步  
    
 ## 全局设置
 ![](./media/图片5.png)
 ![](./media/图片6.png)
 ![](./media/图片7.png)
 ![](./media/图片8.png)
 ![](./media/图片9.png)
 ![](./media/图片10.png)
 ![](./media/图片11.png)
 
  下载 maven [https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi)
  
 ![](./media/图片12.png)
 ![](./media/图片13.png)
 ![](./media/图片14.png)
 ![](./media/图片15.png)
 ![](./media/图片16.png)
 ![](./media/图片17.png)
 ![](./media/图片18.png)
 ![](./media/图片19.png)
 
 
 # 
## 到此 jenkins 部署完成


