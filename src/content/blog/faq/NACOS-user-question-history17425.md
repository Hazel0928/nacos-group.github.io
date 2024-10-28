---
id: "question-history-17425"
title: "通过helm安装nacos"
date: "2024-10-28"
category: "expertConsultation"
description: "通过Helm安装Nacos的步骤可以分为几个主要部分，包括选择运行模式（单机或集群）、是否使用MySQL作为存储、以及具体执行安装命令等。以下是详细的步骤说明：### 1. 确定Nacos运行模式根据您的需求，您需要决定是以`standalone`（单机）模式还是`cluster`（集群）模式部署N"
tags: ["Helm","Nacos","安装","指南","Kubernetes","应用"]
keywords: ["Helm","Nacos","安装","指南","Kubernetes","应用"]
---

## 用户问题 ： 
 通过helm安装nacos  

---------------
## 专家官方解答 ：

通过Helm安装Nacos的步骤可以分为几个主要部分，包括选择运行模式（单机或集群）、是否使用MySQL作为存储、以及具体执行安装命令等。以下是详细的步骤说明：

### 1. 确定Nacos运行模式
根据您的需求，您需要决定是以`standalone`（单机）模式还是`cluster`（集群）模式部署Nacos。
- **Standalone**模式适合测试或小型应用环境。
- **Cluster**模式适合生产环境，以提供高可用性和更好的性能。

### 2. 准备MySQL数据库（如果适用）
如果您计划在`standalone`模式下使用外部MySQL数据库来存储数据，则需要提前准备一个MySQL实例，并记录下其主机名/地址、端口、用户名、密码及数据库名称。请参考这里提供的数据库初始化脚本: [nacos-mysql.sql](https://github.com/alibaba/nacos/blob/develop/distribution/conf/nacos-mysql.sql)

### 3. 执行Helm安装命令
接下来，基于所选的运行模式和是否配置了MySQL，您可以按照下面的方式之一进行安装：
- **Standalone模式（嵌入式存储）**
  ```shell
  $ helm install <release-name> ./ --set global.mode=standalone
  ```
- **Standalone模式（MySQL存储）**
  ```shell
  $ helm install <release-name> ./ --set global.mode=standalone \
    --set nacos.storage.db.host=<mysql-host> \
    --set nacos.storage.db.name=<db-name> \
    --set nacos.storage.db.port=<port> \
    --set nacos.storage.db.username=<username>  \
    --set nacos.storage.db.password=<password>
  ```
- **Cluster模式**
  - 安装基础组件
    ```shell
    $ helm install <release-name> ./ --set global.mode=cluster
    ```
  - 调整副本数量至至少3个
    ```shell
    $ kubectl scale sts <release-name>-nacos --replicas=3
    ```

  > 注意：在集群模式下，可能还需要手动获取并配置各个Pod之间的集群信息，请参阅相关文档了解详细步骤。

### 4. 验证安装
安装完成后，可以通过列出所有Helm发布的版本来检查Nacos是否成功部署：
```shell
$ helm list
```
此外，也可以直接访问Nacos服务页面（通常是通过NodePort暴露出来的），确保其正常运行。

### 5. 清理
如果想要卸载Nacos及其相关的Kubernetes资源，可以使用以下命令：
```shell
$ helm uninstall <release-name>
```

以上就是利用Helm工具安装Nacos的具体过程。每一步都旨在让您能够根据自己的实际需求灵活地调整配置选项，从而搭建出最合适的Nacos环境。


<font color="#949494">---------------</font> 


## 参考链接 ：

* 专家经验：Nacos helm安装 
 
 * 专家经验：Nacos nacos 2.3.0全新安装也需要关闭双写吗？ 


 <font color="#949494">---------------</font> 
 


## <font color="#FF0000">答疑服务说明：</font> 

本内容经由技术专家审阅的用户问答的镜像生成，我们提供了<font color="#FF0000">专家智能答疑服务</font>,使用方法：
用法1： 在<font color="#FF0000">页面的右下的浮窗”专家答疑“</font>。
用法2： 点击[专家答疑页](https://answer.opensource.alibaba.com/docs/intro)（针对部分网站不支持插件嵌入的情况）
### 另：


有其他开源产品的使用问题？[点击访问阿里AI专家答疑服务](https://answer.opensource.alibaba.com/docs/intro)。
### 反馈
如问答有错漏，欢迎点：[差评](https://ai.nacos.io/user/feedbackByEnhancerGradePOJOID?enhancerGradePOJOId=17426)给我们反馈。
