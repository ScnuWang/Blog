Github+docsify +Typora 免费构建自己的文档网站

### 一、工具简介

[docsify](https://docsify.js.org/#/) ：可以直接将Markdown文件解析到网站上面去，启动后，上传的Markdown文件，不需要重启服务器。

​	优点：

	> - 无需构建，写完文档直接发布
	> - 容易使用并且轻量 (~19kB gzipped)
	> - 智能的全文搜索
	> - 提供多套主题
	> - 丰富的 API
	> - 支持 Emoji
	> - 兼容 IE10+
	> - 支持 SSR ([example](https://github.com/docsifyjs/docsify-ssr-demo))

[Typora](https://www.typora.io/) ：是我最喜欢的一个Markdown文本编辑器

### 二、搭建步骤

使用docsify搭建文档网站步骤：

1. 安装npm : 可通过nvm安装

   >nvm 安装：curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
   >
   >查看nvm的版本说明安装成功 : nvm -v 
   >
   >安装指定版本的nodejs: nvm install  version

2. 安装docsify-cli : npm i docsify-cli -g 

3. 初始化项目：docsify init ./docs 

   >初始化成功后，可以看到 `./docs` 目录下创建的几个文件
   >
   >- `index.html` 入口文件
   >- `README.md` 会做为主页内容渲染
   >- `.nojekyll` 用于阻止 GitHub Pages 会忽略掉下划线开头的文件
   >
   >直接编辑 `docs/README.md` 就能更新网站内容，当然也可以[写多个页面](https://docsify.js.org/#/zh-cn/more-pages)。

4. 本地预览网站: docsify serve docs 

   >运行一个本地服务器通过 `docsify serve` 可以方便的预览效果，而且提供 LiveReload 功能，可以让实时的预览。默认访问 [http://localhost:3000](http://localhost:3000/) 。 

### 三、部署

​	[部署](https://docsify.js.org/#/zh-cn/deploy)

​	如果要绑定自己的域名的话：在自己的域名服务商的域名解析页面添加一条记录：

​	![1530859461954](_media/20180706144542.png)

​	GitHub在验证通过之后，会自动在项目中添加一个CNAME文件

### 四、示例网站

​	[官方中文文档](https://docsify.js.org/#/zh-cn/)

