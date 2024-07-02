# Moodle in Kubeblocks（windows）

Moodle是一个免费的在线学习管理系统,它使教育工作者能够创建自己的私人网站,填充充满动态的课程内容,从而实现随时随地的学习扩展。

## 介绍

本文档将引导你快速入门windows下依托于KubeBlocks的数据库mysql来快速安装moodle并使用他

## 必要准备

- [Docker](https://docs.docker.com/get-docker/)：v20.10.5（runc ≥ v1.0.0-rc93）或更高版本；
- [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)：用于与 Kubernetes 集群进行交互；
- [kbcli](https://cn.kubeblocks.io/docs/preview/user-docs/installation/install-with-kbcli/install-kbcli)：用于 Playground 和 KubeBlocks 之间的交互。

## 数据库准备

确保 ApeCloud MySQL 引擎已启用

```shell
kbcli addon list
```

>NAME                           TYPE   STATUS     EXTRAS         AUTO-INSTALL   INSTALLABLE-SELECTOR
>...
>apecloud-mysql                 Helm   Enabled                   true
>...

执行以下命令以获取目标数据库的网络信息，特别注意这里的密码之后要用

```shell
kbcli cluster connect --show-example --show-password ${cluster-name}
```

-![](https://raw.githubusercontent.com/DWJ-Squirtle/blogimage/main/moodle%E5%9B%BE1.png)



执行 `port-forward`将mysql服务提供给主机，**注意这里因为之后本机还需要开启mysql服务，所以为了避免冲突需要将本地端口号修改成其他，这里修改为了3307**

```shell
kubectl port-forward service/mycluster-mysql 3307:3306
```

>Forwarding from 127.0.0.1:3307 -> 3306
>Forwarding from [::1]:3307 -> 3306

有关详细的数据库连接指南，请参考[连接数据库](https://cn.kubeblocks.io/docs/preview/user-docs/create-and-connect-databases/overview-on-connect-databases)。

## 安装moodle

在windows下使用moodle推荐使用xampp

### 1.下载安装xampp

去官网下载xampp并安装，这是网址[XAMPP Installers and Downloads for Apache Friends](https://www.apachefriends.org/zh_cn/index.html)

### 2.打开xampp开启Apache和mysql服务

-![](https://raw.githubusercontent.com/DWJ-Squirtle/blogimage/main/%E6%9C%80%E6%96%B09.png)

### 3.下载moodle

去moodle官网下载适合的文件，这是网址[Latest release | Moodle downloads](https://download.moodle.org/releases/latest/)

![](https://raw.githubusercontent.com/DWJ-Squirtle/blogimage/main/%E6%9C%80%E6%96%B010.png)

下载完成后将压缩包解压至xampp的下载目录中htdocs/下，如果都是默认配置的话会在C:\xampp\htdocs

-![](https://raw.githubusercontent.com/DWJ-Squirtle/blogimage/main/%E6%9C%80%E6%96%B011.png)

### 4.安装moodle

打开浏览器输入地址localhost/moodle之后按照下面的步骤安装moodle

#### 1.确定数据目录

![](https://raw.githubusercontent.com/DWJ-Squirtle/blogimage/main/%E6%9C%80%E6%96%B01.png)

#### 2.确定数据库驱动

-![](https://raw.githubusercontent.com/DWJ-Squirtle/blogimage/main/%E6%9C%80%E6%96%B02.png)

#### 3.填写数据库设置，需要用到之前kubeblocks的设置

![](https://raw.githubusercontent.com/DWJ-Squirtle/blogimage/main/%E6%9C%80%E6%96%B03.png)

#### 4.服务器检查

**PHP扩展的下载，在这一步中会出现一些服务器组件检查报错**

修复步骤如下：

1.使用文本编辑器打开 `php.ini` 文件,他在xampp/下，搜索并取消注释以下扩展行:

- `extension=zip`
- `extension=gd`
- `extension=intl`
- `extension=sodium`
- `max_input_vars`

2.取消`max_input_vars`之后将其修改为`max_input_vars=5000`

3.将 \xampp\php\libsodium.dll 库复制到 \xampp\apache\bin\libsodium.dll

![](https://raw.githubusercontent.com/DWJ-Squirtle/blogimage/main/%E6%9C%80%E6%96%B04.png)

#### 5.等待下载

之后就是一些下载，等待即可

![](https://raw.githubusercontent.com/DWJ-Squirtle/blogimage/main/%E6%9C%80%E6%96%B05.png)

#### 6.moodle基本设置

设置名称和邮件，即将完成啦

![](https://raw.githubusercontent.com/DWJ-Squirtle/blogimage/main/%E6%9C%80%E6%96%B07.png)

#### 7.大功告成

到这一步就完成了所有安装，你可以尽情使用moodle啦！！

![](https://raw.githubusercontent.com/DWJ-Squirtle/blogimage/main/%E6%9C%80%E6%96%B08.png)