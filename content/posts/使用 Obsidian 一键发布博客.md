---
date: 2024-03-21
title: 使用 Obsidian 一键发布博客
tags:
  - 折腾
  - obsidian
---

> 种豆南山下，草盛豆苗稀。  
> 晨兴理荒秽，带月荷锄归。  
> 道狭草木长，夕露沾我衣。  
> 衣沾不足惜，但使愿无违。

博客荒废好久了！之前也陆陆续续使用过一些博客程序，要么发布部署有些麻烦，要么使用第三方服务不太稳定，一段时间就停止维护了。

偶然的机会看到文章： [obsidian 配合 hugo、cloudflare：让发布博客简单到不可思议](https://lillianwho.com/posts/obsidian-hugo-cloudflare/)，心动入坑，花了近一周的时间研究折腾，终于把基于 obsidian 的博客写作与发布工作流搭建起来了，现在可以愉快的一键发布啦。

从 0 到 1 折腾踩坑，整个过程好似「**晨兴理荒秽，带月荷锄归**」，心累，年纪再大点肯定折腾不动的，怕麻烦的劝退。

## 博客方案
折腾前我整理出自己的博客需求如下：
- 简单快捷：使用 Obsidian 写完，一键发布
- 稳定靠谱：减少第三方依赖，尽量使用靠谱的第三方服务，例如 github
- 图片顺畅加载：github 图床国内访问有点难

参考一些大佬的配置，最终的选型如下：
- Obsidian + git 插件 ：markdown 写作，git 插件自动上传 github
- Hugo：静态网站构建工具
- Github  + Github Action ：源码托管，自动化编译（*emmm，github 应该不会倒闭*）
- Cloudflare Page：静态网站托管 （也可以直接用 github page 托管）
- Picgo + jsdelivr：基于 github 的图床 、cdn 加速

本站源码与主题：
- [GitHub - goby-ao/yaofun.top: my blog](https://github.com/goby-ao/yaofun.top/)
- [GitHub - rhazdon/hugo-theme-hello-friend-ng](https://github.com/rhazdon/hugo-theme-hello-friend-ng)

参考与致谢：
- [折腾 :: 木木木木木](https://immmmm.com/tags/%E6%8A%98%E8%85%BE/)
- [Lillian Who](https://lillianwho.com/)

## 折腾记录 

### 0 准备工作
- 能科学上网
- github 账号、熟悉基本命令
- cloudflare 账号
- 个人域名
### 1 本地初始化 hugo 博客

**1.1 hugo 安装**

windows 上安装 hugo 需要先安装 go，在 macOS 上安装 hugo 比较简单可以自动安装所需依赖。
```bash
brew update
brew install hugo
```

**1.2 hugo 博客初始化**
```bash
hugo new site yaofun.top
```
例如 /home/yao/blog 下执行命令，会生成 yaofun.top 子目录： /home/yao/blog/yaofun.top

**1.3 安装 hugo 主题**

我安装的是 hello-friend-ng 主题：
```bash
$ git submodule add https://github.com/rhazdon/hugo-theme-hello-friend-ng.git themes/hello-friend-ng
```

**1.4 本地运行与调试**

拷贝 theme 下的 exmapleSite 下的 content 目录和 .toml 配置到 hugo 根目录（/home/yao/blog/yaofun.top）

执行 hugo server 即可用 http://localhost:1313 查看预览效果，修改静态代码后它会自动重新编译构建。

你可以调试 hugo 的配置，折腾成自己满意的效果。

### 2 自动化发布与部署

本地博客搭建完毕后，可以开始配置自动化部署。

**2.1 博客及源码上传到 github 仓库**
1. github 新建一个仓库 ，git clone 到本地
2. 把博客源码（`/home/yao/blog/yaofun.top/*` ）拷贝到本地 git 仓库里面
3. 通过 git push 到 github

至此可以通过 git 把源码推送到 github 仓库

**2.2 博客源码自动化编译部署**
1. 新建一个仓库，存放 hugo 生成的博客静态文件（这一步也可以用同仓库，不同分支）
2. 配置 github action [github action](https://github.com/goby-ao/goby-ao.github.io) 实现：
	1. hugo 编译生成静态代码 ./public；
	2. 推送 ./public 静态代码到刚才新建的 [github 静态文件仓库](https://github.com/goby-ao/goby-ao.github.io)

**2.3 cloudflare page 托管静态网站**
1. cloudflare 在 workers & pages 里面新建一个 page，指向静态代码的仓库
2. build 里面什么都不用配置，因为 github action 已经完成了 hugo 编译
3. 配置自定义域名


至此完成基本的自动化发布流程：

本地 -> github 源码库 -> github 静态仓库 -> cloudflare page 托管

### 3 图床与图床加速
文本没问题了，还差一个图片如何展示，并且国内也能顺畅展示的问题。

**3.1 picgo + github 图床**
下载 picgo ，设置 github 图床，此步略过不表

**3.2 图床加速**
picgo 配置自定义域名，例如我的是：https://cdn.jsdelivr.net/gh/goby-ao/picgo@main
配置规则：github 用户名，仓库名，分支要换成自己的。默认图片在仓库的 img 目录。

### 4  obsidian 配置
以上工作做好之后，就可以配置 obsidian 了，实现拼图的最后一步 - 使用 obsidian 一键发布。
1. obsidian 创建新的 vault ，地址为 hugo 源码目录
2. 安装 obsidian 插件 *git*：在 obsidian 中一键 commit 、push 代码
3. 安装 obsidian 插件 *Image Auto Upload*：实现贴图自动上传图床，生成链接
4. 安装 obsidian 插件 *Templater* ：配置博客模板 header

```yml
---
date: <% tp.file.creation_date("YYYY-MM-DD") %>
title: <% tp.file.title %>
tags: ['<% tp.system.suggester(item => item, Object.keys(app.metadataCache.getTags()).map(x => x.replace("#", ""))) %>']
---
```

5. 安装 obsidian 插件 *quickadd* ：更便捷的创建博客文件

### 5 折腾美化

**5.1 图片排版**

参考：[Hugo 多图排版这样来：：木木木木木](https://immmmm.com/about-images-gird/) 在 ` themes/hello-friend-ng/assets/scss/_single.scss  ` 里面加入以下 css 代码：

```css
.post-content p:has(> img:nth-child(2)){column-count:2;column-gap:8px;margin:6px 0;}
.post-content p:has(> img:nth-child(3)){column-count:3;}
.post-content p:has(> img:nth-child(4)){column-count:4;}
.post-content p:has(> img:nth-child(5)){column-count:5;}
.post-content p:has(> img:nth-child(6)){column-count:4;}
.post-content p:has(> img:nth-child(2)) img{display:inherit;}
.post-content p:has(> img:nth-child(6)) img{margin-bottom:8px;}
```

**5.2 图片灯箱效果**

图片点击后灯箱效果，使用：[ViewImage.js - 2kb minimalist lightbox.](https://tokinx.github.io/ViewImage/)

1. 下载 `view-image.min.js` 到 static/js/ 下
2. 在 themes/hello-friend-ng/layouts/partials/javascript.html 下加入代码：

```js
<script src="/js/view-image.min.js"></script>
<script>
    window.ViewImage && ViewImage.init('.content img');
</script>
```

### 6 其他问题

**中文字数统计不准**

在 hugo.toml 里加入一行：hasCJKLanguage = true


## 为什么要写博客 ？

督促自己思考，沉淀，进步。在互联网上有一片属于自己的自留地。
> People Die, but Long Live GitHub
------------

*本文使用 obsidian 一键发布*