# 快速搭建手册

## 一、基础软件安装
Gradle (4.9)  
MySQL (5.5+)  
JDK (1.8.0_141)
Zookeeper (3.4.9)
Linkis（0.9.1), 必装Spark引擎。[如何安装Linkis](https://github.com/WeBankFinTech/Linkis)  
DataSphereStudio (0.6.0) 可选. 如果你想使用工作流，必装DataSphereStudio [如何安装DataSphereStudio?](https://github.com/WeBankFinTech/DataSphereStudio)

## 二、安装包下载
[下载](https://github.com/WeBankFinTech/Qualitis/releases)

## 三、编译(二进制包跳过)
```
gradle clean distZip
```

## 四、部署
### 4.1 解压安装包
##### zip包
```
unzip qualitis-{version}.zip
```

##### tar包
```
tar -zxvf qualitis-{VERSION}.tar.gz
```

### 4.2 连接MySQL，插入初始数据。
```
mysql -u {USERNAME} -p {PASSWORD} -h {IP} --default-character-set=utf8
source conf/database/init.sql
```

### 4.3 修改配置文件
```
vim conf/application-dev.yml
```
修改以下配置：
```
## 数据库配置
spring.datasource.username=
spring.datasource.password=
spring.datasource.url=

## 数据库配置，和以上一致
task.persistence.username=
task.persistence.password=
task.persistence.address=

## zk配置
zk.address=
```
并打开HA开关
```
vim conf/application.yml
```
开启HA：
```
ha.enable=true
```

### 4.4 启动系统
```
dos2unix bin/*
sh bin/start.sh
```

## 五、登录
### 5.1 登录验证
浏览器中输入localhost:8090, 出现一下页面说明启动成功  
![登录验证图片](../../../images/zh_CN/ch1/登录.png)  
输入  
用户名: admin  
密码: admin  

### 5.2 系统配置填写
点击系统配置 -> 参数配置，并新增集群
填入以下配置信息：  
集群名称(Hadoop集群名称)  
集群类型
Linkis地址  
Linkis Token(Linkis Token配置方式请查看文档：[接入Linkis文档](接入Linkis文档.md))  

可参考以下例子：
![](../../../images/zh_CN/ch1/规则配置样例.png)

---

Tips:

Qualitis会将异常数据保存在数据库当中，保存的数据库名称可以在系统设置中配置，如下图所示：
![](../../../images/zh_CN/ch1/异常数据数据库配置.png)
如图所示，Qualitis提供${USERNAME}来作为用户名替换的表达式，不同的用户跑出来的异常数据，保存在各自的数据库中。

---

工作流已默认开启，工作流需要安装[DataSphereStudio](https://github.com/WeBankFinTech/DataSphereStudio)。  
开启工作流不影响Qualitis正常使用。
如需关闭，可修改配置文件
```
vim conf/application.yml
```
并修改workflow.enable=false

---

Debug模式已默认开启，默认端口是8091。
在源码中，你可以通过在'build.gradle'文件中删除以下代码，关闭Debug模式。
```
-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=8091
```
如果是在编译好的代码中，可以通过编辑'bin/qualitis'文件，来关闭Debug模式。
