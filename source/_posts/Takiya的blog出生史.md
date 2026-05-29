---
title: Takiya的blog出生史
date: 2026-05-28 17:45:09
tags:
  - Hexo
  - 博客搭建
  - Mac
  - 踩坑
categories: 技术笔记
thumbnail: post-cover-home.jpg 
---

# 📖 从零到一：Mac 部署 Hexo 博客踩坑记

> 终于，我的博客上线了！  
> 这不仅仅是一个技术记录，更是一部“血泪史”。  
> 从安装环境到背景图不显示，从子模块坑到国内访问 401，我几乎把所有能踩的坑都踩了一遍。  
> 这篇文章将完整还原整个过程，希望能帮你节省几天时间。

#### 实际操作建议搭配AI，本文仅为大概流程，~~和犯错指南~~

## 🚀 为什么突然想搭博客？

其实很简单：那天和付Q聊了一会儿，他说叫我搞个博客。
<img src="{% asset_path FQ.jpg %}" width="200" alt="FQ" />
于是，在2026年2月26日的深夜，我打开了终端…… 



## 💻 第一步：本地环境准备

我用的 Mac，需要先装 Homebrew，再装 Node.js 和 Git  
<!-- <small>这是一句小字号的话，适合写补充说明或次要信息。</small> -->
<!--<span style="color: gray;">这是一句灰色文字，不显眼但可读。</span>-->

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install node git
```
<small>不得不说 Mac 装了 Homebrew 之后，安装东西一行命令就搞定了，简直不要太方便。</small>

---
然后全局安装 Hexo：

```bash
sudo npm install -g hexo-cli
```
<small> 但是可能会因为网络问题，需要选择国内镜像，不然会卡很久。</small>

---

初始化博客文件夹：

```bash
hexo init my-blog
cd my-blog
npm install
```
---
启动本地预览：

```bash
hexo server
```

打开 ```http://localhost:4000```，看到了 Hexo 默认的 Landscape 主题 —— 一切顺利，天真地以为后面也会这么顺利。

---
新建一篇博客：
终端输入：
```bash
code source/_posts/我的第一篇博客.md
```

<details>
  <summary>如果有vscode不能打开点这里</summary>

打开 VS Code：

```bash
open -a "Visual Studio Code"
```
   
在 VS Code 中按 Cmd + Shift + P 打开命令面板

输入 shell command，选择 Shell Command: Install 'code' command in PATH

</details>

在 VS Code 中新建的 Markdown 文件里，尝试写点东西。

写完后，再次启动本地预览：

```bash
hexo c && hexo g && hexo s
```

就可以在 ```http://localhost:4000```，看到你刚刚写的文章了。


## 🎨 第二步：选主题

你可以直接在 Hexo 官方的主题库 [Themes | Hexo](https://hexo.io/themes/) 里逛一逛。这里有400多个主题，大部分都提供了「Preview site」链接，可以实时预览效果。

<small>我相中了一个叫 **miccall** 的主题，风格简洁，可以自定义背景。  </small>
<small>但是，这个主题已经 **8年没更新了**！而我没在意。</small>

```bash
git clone 你选择主题在GitHub的地址
```
<small>GitHub中绿色code按钮下方的链接</small>

然后修改 ```_config.yml``` 中的 ```theme: 你的主题名```。

改完后，再次启动本地预览：

```bash
hexo c && hexo g && hexo s
```

就可以在 ```http://localhost:4000```，看到你的主题变了。


## 🖼️ 第三步：编辑主页内容

用 VS Code 打开主题的配置文件：

```bash
code themes/miccall/_config.yml
```
可以把里面代码复制给AI，让它解释每一行的意思，然后自己去修改内容

<small>如果有时候打开发现是空的，可能是没有进入博客文件夹。 </small>

---

主题模板里其实早就预留了 ```tumbnail``` 字段，但我一直用 ```home_cover``` 和 ```post_cover```，自然没效果。  
看主题的 ```index.ejs``` 才发现：

```ejs
<div class="page_title" style="background-image: url(<%= post.thumbnail %>); ... ">
```

所以只需要在文章 Front-Matter 里写：

```yaml
thumbnail: /images/post-cover-home.jpg
```


## 🌍 第四步：部署到 Vercel

#### 第1步：在 GitHub 上创建一个仓库

登录你的 GitHub 账号。

点击右上角的 + 号，选择 New repository。

Repository name 必须填写为 你的GitHub用户名.github.io。

比如你的用户名是 Takiya168，就填 Takiya168.github.io。

确保仓库是 Public (公开的)，然后点击 Create repository 创建。

#### 第2步：将本地博客代码推送到 GitHub
这个步骤是把你的博客源文件（包括主题、文章、配置）上传到 GitHub 仓库。

打开终端，确保你当前在博客根目录 (~/my-blog)。

初始化 Git 仓库（如果还没初始化过）：

```bash
git init
```
添加所有文件并提交：

```bash
git add .
git commit -m "第一次提交，我的Hexo博客"
```
关联远程仓库并推送：

```bash
git remote add origin https://github.com/你的用户名/你的用户名.github.io.git
git push -u origin main
```
如果默认分支是 master，则使用 git push -u origin master。完成后，你的所有代码就都到 GitHub 上了。

#### 第3步：在 Vercel 上导入项目
访问 Vercel 官网，使用 GitHub 账号登录。

登录后，点击 Add New，选择 Project。

Vercel 会自动列出你 GitHub 账号下的所有仓库，找到并点击你的博客仓库（你的用户名.github.io）旁的 Import 按钮。

在配置页面，Vercel 会自动检测到这是 Hexo 项目，并帮你填好构建命令。你什么都不用改，直接点击底部的 Deploy 按钮即可。

等待不到一分钟，部署就会完成。Vercel 会为你生成一个 .vercel.app 的域名，点击 Visit 就能立即访问。

---
##### 📌 如何更新博客？
无论你用了哪种部署方式，日后更新博客的流程都变得无比简单和统一：

在本地写好新文章 (hexo new "新文章标题")。

在终端依次执行：

```bash
git add .
git commit -m "发布了一篇新文章"
git push
```
就这样，你每次执行 git push 把改动推送到 GitHub 后，GitHub Actions 或 Vercel 就会自动完成后续所有的构建和部署工作，过1-2分钟刷新一下网页，新内容就上线了

---

但是当我部署上去之后发现主页图片依旧是原来主题默认的图片
反正GitHub里面，包括直接访问链接都是修改后的图片，但是博客里面就是死活不对
和deepseek搞了好几十个来回都没搞出来，于是我转战ChatGPT求助

虽然他没直接找出我的错误，但是在回答中提醒到我了

<img src="{% asset_path ChatGPT.jpg %}" width="600" alt="ChatGPT提示截图" />

一直没注意，被随机链接搞了好久

## 🌍 第五步：国内访问被墙，域名还太长了

用 GitHub 托管代码，Vercel 自动部署真的很香。  
但问题来了：** ```*.vercel.app``` 域名在国内经常被阻断**，需要 VPN 才能访问。

Cloudflare 代理
这是目前最主流、性价比最高的方案。它利用 Cloudflare 遍布全球的 CDN 网络来缓存和加速你的网站，同时还能帮你绑定自己的域名。

#### 1. 准备一个域名
首先，你需要准备一个属于自己的域名，将域名的 DNS 解析服务托管到 Cloudflare。

###### 1.1 购买域名：在任意域名注册商购买一个你喜欢的域名

我买的域名是阿里云，你也可以在 GoDaddy、Namecheap 等平台购买。
阿里云购买后需要注册局审核，大概需要1-2天时间（但实际只需要半个多小时），审核通过后才能正式使用。
腾讯云是审核之后才能购买。

**下面的内容还是建议跟着AI走，因为实际操作起来，很多细节还是需要自己摸索的，需要不断的问AI细节。**


###### 1.2 将DNS托管给Cloudflare：

注册并登录 Cloudflare。

在仪表板点击 “添加站点” ，输入你的域名，并选择 “Free” 计划。

Cloudflare 会提供给你两个新的 Nameserver (DNS服务器) 地址。

前往你购买域名的网站（如阿里云、GoDaddy等），在域名的DNS管理设置中，把 DNS 服务器改为 Cloudflare 给你的那两个地址。

这个修改通常需要1小时到24小时才能在全球生效。
（其实也还是比较快，一会儿就好了）

#### 2. 在 Vercel 上绑定自定义域名
登录你的 Vercel 控制台，进入你的博客项目。

点击顶部的 “Settings” -> “Domains”。

在输入框中填入你刚刚购买的域名（例如 yourdomain.com），然后点击 “Add” 按钮。

Vercel 会显示需要添加的DNS记录，先保持这个页面打开，不要关闭。

#### 3. 在 Cloudflare 配置加速和代理
回到 Cloudflare 控制台，点击你添加的站点，进入 “DNS” 设置页面。

点击 “添加记录”，添加以下两条 CNAME 记录：

记录 1 (用于根域名)

类型：CNAME  名称：@ (代表你的主域名 yourdomain.com)  目标（内容）：cname-china.vercel-dns.com

这里就是最关键的一步。不要填 cname.vercel-dns.com，而是要填 cname-china.vercel-dns.com。这个域名是 Vercel 专门为优化亚太地区访问准备的。

代理状态：开启（点亮橙色云朵图标）。

记录 2 (用于 www 子域名)

类型：CNAME  名称：www  目标（内容）：同样填 cname-china.vercel-dns.com

代理状态：开启。

#### 4. 最终检查与生效
添加完DNS记录后，回到 Vercel 的 Domains 设置页面。

稍等片刻，Vercel 就会验证你的DNS配置，并为你的域名自动签发免费的 SSL 证书（HTTPS）。

看到域名旁边出现绿色的 Valid 或 Success 状态，就表示大功告成了！

**具体详细的还是建议跟着AI走**

## 🌐 最终成果

- 博客地址：[https://www.takiya168.top/](https://www.takiya168.top/)
- GitHub 仓库：[https://github.com/Takiya168/Takiya168.github.io](https://github.com/Takiya168/Takiya168.github.io)

## 🎯 下一步计划
- 写更多技术文章
- 优化移动端样式

---

**感谢你读到这儿。**  
如果你也在搭建博客，希望这篇文章能让你少走一半弯路。  

**Happy Blogging!** 🚀
