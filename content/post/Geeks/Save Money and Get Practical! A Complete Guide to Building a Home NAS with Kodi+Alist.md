---
layout:   page
title:       "省钱又实用！使用Kodi+Alist搭建家庭NAS完全指南"
subtitle:    ""
description: ""
date:        2023-04-16
author:   "aiqiu"
#image:       "https://golearning.oss-cn-shanghai.aliyuncs.com/202304161638319.png"
categories:  ["Geeks"]
summary:     "在本文中，我们介绍了使用Kodi+Alist搭建家庭NAS的完全指南。我们首先解释了为什么需要使用家庭NAS，并阐述了Kodi+Alist组合的优缺点。然后，我们简要介绍了Kodi和Alist，以及如何配置和安装它们。最后，我们演示了如何将Kodi连接到Alist，以便在家庭网络中访问和共享云存储中的文件。使用Kodi+Alist搭建家庭NAS是一种省钱又实用的方法，可以帮助家庭用户在家中轻松存储和共享文件，同时还可以作为媒体中心，播放家庭娱乐内容。"
published: true
tags:
    - NAS 
    - Kodi
    - Alist
---


## **引言**

### **为什么要使用家庭 NAS？**

家庭 NAS 是一个可以让你在家中存储和共享文件的设备，既省钱又实用。使用 Kodi+Alist 的组合，可以搭建一个功能强大的 NAS 系统，可以用来存储照片、视频、音乐和其他文件，并在家庭网络中共享这些文件。此外，这个系统还可以作为媒体中心，播放家庭娱乐内容，如电影、电视节目和音乐。

### **本文适合的阅读人群**

本文适合想要搭建家庭 NAS 系统，尤其是在家中需要存储和共享文件，并且希望使用省钱又实用的设备的人群。

### **搭建所需设备**

- 一台可以运行 Kodi 和 Alist 的设备，如 Raspberry Pi 或者普通的台式机，或者一个带有 OpenWrt 的软路由。

### **与传统 NAS 相比的优缺点**

与传统的 NAS 相比，Kodi+Alist 的组合有一些优缺点。优点包括价格更加实惠，可以自定义配置，可以随时访问和分享文件。缺点则包括依赖于云服务，可能会受到网络速度和稳定性的影响。

## **什么是 Kodi 和 Alist？**

### **Kodi**

Kodi 是一款开源媒体播放器软件，可以在多种平台上使用，如 Windows、Mac、Linux 和 Android 等。它支持各种格式的音频和视频文件，并且可以通过插件和附加组件扩展其功能。

### **Alist**

Alist 是一款云存储服务，可以让你在云端存储和管理文件，并且可以通过网络访问这些文件。在官方文档中可以看到，Alist 可以支持很多常用的云盘：

![https://golearning.oss-cn-shanghai.aliyuncs.com/202304161514460.png](https://golearning.oss-cn-shanghai.aliyuncs.com/202304161514460.png)

## **如何配置和安装Kodi和Alist**

> 这是kodi和alist集成的基本结构
>

![https://golearning.oss-cn-shanghai.aliyuncs.com/202304161530462.png](https://golearning.oss-cn-shanghai.aliyuncs.com/202304161530462.png)

首先，从Kodi的官方网站（**[https://kodi.tv/download](https://kodi.tv/download)**）下载适合你设备的版本。例如，你可以下载适用于你的 Mac 和投影仪的 Kodi 版本。

在完成 Kodi 的安装后，我们需要安装 Alist。

### **Alist 安装方法**

### OpenWrt 安装包安装

如果你是 OpenWrt 用户，可以直接在系统的软件包管理中搜索 Alist 并进行下载安装。

![https://golearning.oss-cn-shanghai.aliyuncs.com/202304161545961.png](https://golearning.oss-cn-shanghai.aliyuncs.com/202304161545961.png)

安装完成后，你可以在“网络存储”位置找到 Alist 的管理界面。

![https://golearning.oss-cn-shanghai.aliyuncs.com/202304161546642.png](https://golearning.oss-cn-shanghai.aliyuncs.com/202304161546642.png)

### Docker 部署

如果你使用 Docker 进行部署，你可以使用官方提供的 Docker 部署命令。

```
docker run -d --restart=always -v /etc/alist:/opt/alist/data -p 5244:5244 -e PUID=0 -e PGID=0 -e UMASK=022 --name="alist" xhofe/alist:latest
```

在完成部署后，使用以下命令获取 admin 的登录密码：

```
docker exec -it alist ./alist admin
```

### **Kodi 设置**

在完成 Alist 的安装后，我们可以使用 Kodi 来连接 Alist。

进入文件区 -> 选择添加视频 -> 点击浏览 ->添加网络位置

![https://golearning.oss-cn-shanghai.aliyuncs.com/202304161556448.png](https://golearning.oss-cn-shanghai.aliyuncs.com/202304161556448.png)

添加一个新的网络位置。如果你的地址不支持 HTTPS，一定要选择 HTTP 的 WebDAV。

![https://golearning.oss-cn-shanghai.aliyuncs.com/202304161605596.png](https://golearning.oss-cn-shanghai.aliyuncs.com/202304161605596.png)

点击确定后，添加成功！在共享中就可以看到刚刚添加的 Alist 地址。

![https://golearning.oss-cn-shanghai.aliyuncs.com/202304161606446.png](https://golearning.oss-cn-shanghai.aliyuncs.com/202304161606446.png)

回到最外层菜单，使用我们新增的这个Alist地址，便可以访问到你放在云盘里的电影文件了。

## **总结**

在本文中，我们介绍了使用Kodi+Alist搭建家庭NAS的完全指南。我们首先解释了为什么需要使用家庭NAS，并阐述了Kodi+Alist组合的优缺点。然后，我们简要介绍了Kodi和Alist，以及如何配置和安装它们。最后，我们演示了如何将Kodi连接到Alist，以便在家庭网络中访问和共享云存储中的文件。使用Kodi+Alist搭建家庭NAS是一种省钱又实用的方法，可以帮助家庭用户在家中轻松存储和共享文件，同时还可以作为媒体中心，播放家庭娱乐内容。