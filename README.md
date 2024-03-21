## Welcome to my blog

> 「晚来天欲雪，能饮一杯无？」

### 博客介绍

写作配套：
- Obsidian + git 插件 ：markdown 写作，自动上传
- Hugo：静态网站构建工具
- Github  + Github Action ：源码托管，自动化编译
- Cloudflare Page：静态网站托管

部署流程:
- 在 obsidian 中编写文章
- 使用 obsidian git 插件一键上传源码（上传到 [github 博客源码仓库](https://github.com/goby-ao/yaofun.top) ）
- 触发 [github action](https://github.com/goby-ao/goby-ao.github.io) ：1）hugo 编译生成静态代码 ./public；2）推送 public 静态代码到 [github 静态文件仓库](https://github.com/goby-ao/goby-ao.github.io)
- cloudflare page 托管 [github 静态文件仓库](https://github.com/goby-ao/goby-ao.github.io)
