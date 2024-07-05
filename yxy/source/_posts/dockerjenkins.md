---
title: docker安装jenkins
date: 2022-03-21 19:36:26
tags:
---

明天写拉
<!--- https://juejin.cn/post/6967243012199940110 --->

docker pull jenkins
docker run xxx

## **一、目的**

本文介绍如何在本地搭建一个基于 Docker 和 Jenkins 的自动化部署系统，并通过 GitHub 的 Webhook 实现代码的自动化部署。

## **二、准备工作**

首先，我们需要安装 Docker。根据操作系统的不同，安装方式有所不同：

- 对于 macOS，请[點擊下載](https://link.juejin.cn/?target=https%3A%2F%2Fdocs.docker.com%2Fdocker-for-mac%2Finstall%2F)。
- 对于 Windows，请[點擊下載](https://link.juejin.cn/?target=https%3A%2F%2Fdocs.docker.com%2Fdocker-for-windows%2Finstall%2F)。
- 对于 Linux，请使用以下命令安装：

```bash
curl -fsSL get.docker.com -o get-docker.sh
sudo sh get-docker.sh --mirror Aliyun
```

安装完成后，通过输入 `docker -v` 命令来验证是否成功安装。

```bash
Docker version 20.10.5, build xxxxx
```

接下来，我们安装 Jenkins。首先搜索 Jenkins 镜像并拉取稳定運行的版本。

```bash
docker search jenkins
docker pull jenkinsci/blueocean
```

确认镜像拉取成功后，使用以下命令启动 Jenkins 容器：

```bash
docker run -d --name docker-jenkins -p 8008:8080 -p 50000:50000 jenkinsci/blueocean
```

启动成功后，你将会收到该容器人唯一的假的 ID。

打开浏览器，访问本地的 8008 端口，你将看到 Jenkins 的解锁页面。

### **获取初始化密码**

接下来，我们需要获取 Jenkins 的初始化密码。进入容器内部并查看密码：

```bash
docker exec -it docker-jenkins bash
cat /var/jenkins_home/secrets/initialAdminPassword
```

输入控制台返回的密码，即可进入配置页面。选择安装推荐的插件，如果有安装失败的插件，重试即可。

### **创建用户**

安装完成后，输入相关信息创建用户。创建完成后，继续点击下一步。系统将会重启，重启完成后使用刚创建的用户登录。接下来进行配置环节。

## **三、配置**

接下来，我们对 GitHub 和 Jenkins 进行配置。

### **在容器内生成 SSH 公钥**

首先，我们需要在 Jenkins 容器内生成 SSH 公钥：

```bash
ssh-keygen -t rsa -C test@test.com
```

然后，将公钥添加到 GitHub 上。在 GitHub 中的个人设置中找到 SSH and GPG keys，添加新的 SSH key，填入公钥即可。

### **添加私钥到 Jenkins**

接下来，我们需要将生成的私钥添加到 Jenkins 中。在 Jenkins 中的系统管理中，添加凭证。

- 类型选择 SSH Username with private key。
- 描述和用户名随意填写。

### **创建任务**

在 Jenkins 中创建一个新任务，选择构建一个自由风格的软件项目。配置源码管理为 Git，输入远程仓库地址，并选择之前添加的凭证。指定分支为 master。

### **尝试项目构建**

手动点击构建项目，查看日志。如果配置正确，可以看到构建完成的状态为 SUCCESS。

### **自动化配置**

完成自动化部署主要通过 GitHub Webhook。GitHub Webhook 通知 Jenkins 进行构建。在 GitHub 上生成一个 Personal access token，并拷贝生成的 token。

### **配置 GitHub**

在 GitHub 仓库设置中，添加 Webhook。

- Payload URL 填入 Jenkins 容器运行端口映射出去的公网域名 + `/github-webhook/`。
- Content type 选择 application/json。
- Secret 填入之前生成的 token。

### **配置 Jenkins**

首先，我们需要在 Jenkins 中创建一个凭证：

- 类型选择 Secret text。

然后对该凭证进行配置：

- 在系统配置中，配置 GitHub，覆盖 Hook URL 并输入 Webhook 地址，并选中创建的凭证。

配置完成后，当代码仓库更新后，Jenkins 将会自动进行构建。

### **打包项目**

项目代码需要打包后才能正确使用，接下来在 Jenkins 端进行代码构建操作。

首先，安装必要的插件 NodeJS 和 Publish Over SSH。

### **配置全局 NodeJS 插件**：

- 在全局工具配置中，新增 NodeJS。

### **配置项目中的 NodeJS 构建环境**：

- 在项目配置中，选择 NodeJS 环境。
- 在构建操作中，执行 shell 安装依赖項并打包。

```bash
echo "hello world"
npm install
npm run build
cd dist
tar zcvf dist.tar.gz ./*
```

### **发送到服务器**

全局配置 Publish Over SSH 插件：

- 在系统配置中，配置 SSH 服务器。

### **项目中配置**：

- 在项目配置中，配置 SSH Server 的 Transfers。

```bash
Source files: 文件发送的相对路径
Remove prefix: 要去掉的前缀
Remote directory: 远端服务器目录
Exec command: 发送成功后在服务器进行的操作，如解压、删除多余文件等
```

脚本示例：

```bash
cd 文件发送的目录
tar zxvf dist.tar.gz
rm -rf dist.tar.gz
```

至此，你已经完成了本地 Docker + Jenkins 的自动化部署系统的搭建。你可以尝试推送代码，验证自动化构建部署的流程是否成功。
