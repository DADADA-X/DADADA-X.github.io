---
layout:     post
title:      "github page 搭建记录"
subtitle:   " \"Hello World, Hello Blog\""
date:       2020-04-27 15:00:00
author:     "DadaX"
header-img: "img/post-bg-2015.jpg"
---



> 我的个人主页：[DadaX Blog](https://dadada-x.github.io/)



### About Jekyll

**Jekyll** is a static site generator with built-in support for GitHub Pages.

**依赖：**ruby gem（mac用户安装Xcode 和 Command-Line Tools后就有了；**windows用户不建议安装jekyll**）

### 第〇步 安装jekyll

```python
brew install ruby
ruby -v		# 检查已有版本
gem install jekyll bundler
```

### 第一步 fork模版

我用的是[Hux blog 的模版](https://huangxuan.me/)，这是[仓库地址](https://github.com/Huxpro/huxblog-boilerplate)，这是[中文教程](https://github.com/Huxpro/huxpro.github.io/blob/master/README.zh.md)。

进入仓库，点击fork，再回到自己的仓库，进入刚刚fork的项目，点击setting，将仓库名字改为： `<username>.github.io`,`<username>`就是你的github用户名。

将该仓库clone到本地。

然后在命令行输入`jekyll serve`，就可以启动一个本地服务器，根据给出的地址，就可以查看主题（没有文章）。

### 第二步 开始自定义

* 修改`_config.yml`，自定变量，会自动绑定到`site`对象上。

### 第三步 write-posts

markdown的格式放在这里`_posts/`

文章的文件名遵循下面的格式：

```
年-月-日-标题.md
```

文章内容顶部必须有下面的 YAML 头信息：

```
---
layout:     post
title:      "Hello 2015"
subtitle:   "Hello World, Hello Blog"
date:       2015-01-29 12:00:00
author:     "Hux"
header-img: "img/post-bg-2015.jpg"
tags:
    - Life
---
```

### 各文件含义

* `_layouts` （存放页面模板，md或html文件的内容会填充模板）

* `_sass`（存放样式表）

* `_includes` （可以复用在其它页面被include的html页面）

* `_posts`（博客文章页面）

* `assets`（原生的资源文件）

* - `js`
  - `css`
  - `image`

* `_config.yml` （全局配置文件）

* `index.html, index.md, README.md` （首页index.html优先级最高，如果没有index，默认启用README.md文件）

* `自定义文件和目录`



一个较简单的 Jekyll 项目的文件结构如下：

```text
.
├── _config.yml
├── _posts
│   └── 2020-04-26-welcome-to-jekyll.markdown
├── about.md
├── readme.md
├── _layouts
│   ├── default.html
│   ├── post.html
│   └── page.html
└── _includes
    ├── footer.html
    └── header.html
```

- `/_config.yml`：配置数据。
- `_posts`：文章。格式：`YYYY-mm-dd-title.md`（如果这些文件中包含YAML头信息部分，Jekyll 就会自动将它们进行转换。）
- `about.md`
- `readme.md`
- `_layouts`：布局。是文章外部的模版，可在yaml头信息种进行选择。（标签 `{{ content }}` 可以将content插入页面中。）
- `_includes`：通常是 layout 中抽取出的公共部分，可以加载这部分到布局或文章中方便重用。在 layout 中通过如 {`%include footer.html %`} 引入
- `_site`：Jekyll 完成转换，就会将生成的页面放在这里（默认）。格式为：`/YYYY/mm/dd/title.html`，其他一些css, image, favicon.icon都将被完全靠背到生成的site中。
- `index.html`：任何一目录都会查找索引页用以生成html。

* 其它页：如 `about.md`。Github Pages 会将所有非下划线开头的路径中的 markdown 文件转为对应名字的网页。
* 网页模板：带 {`% if true %`} {`{ var | filter }`} {`% endif %`} 一类标记的html或md

---

* 变量 `{{ variable }}` 被嵌入在页面中，会在静态页面生成的时候被替换成具体的数值。
* 常用的全局变量对象有：`site` 和 `page`。
  * `site`对象对应的就是网站范围，自定义变量放在`_config.yml`中。
  * `page`对象对应的是单个页面，自定义变量放在每个页面的最开头。

* 条件判断语句

  ```html
  {% if site.title == 'Awesome Shoes' %}     
     These shoes are awesome! 
  {% endif %}  
  ```

* 循环迭代

  ```html
  {% for product in collection.products %}    
   {{ product.title }} 
  {% endfor %}
  ```

  

### 问题

1. **安装`jekyll`**

   * `brew install ruby`，会先让`update brewhome`，但是因为网速等原因没有反应，【之后记得升级一下（已升级）】直接`ctrl+C`可以取消更新，直接安装。

   * `gem install jekyll bundler`，这里是装了jekyll和bunlder，jekyll安装失败，报错：

     ```
     ERROR:  Error installing jekyll:
     	ERROR: Failed to build gem native extension.
     
         current directory: /Library/Ruby/Gems/2.3.0/gems/http_parser.rb-0.6.0/ext/ruby_http_parser
     /System/Library/Frameworks/Ruby.framework/Versions/2.3/usr/bin/ruby -r ./siteconf20200408-12416-1sihhf9.rb extconf.rb
     mkmf.rb can't find header files for ruby at /System/Library/Frameworks/Ruby.framework/Versions/2.3/usr/lib/ruby/include/ruby.h
     
     extconf failed, exit code 1
     ```

     首先根据[jekyll官网](https://jekyllrb.com/docs/installation/macos/): Jekyll requires **Ruby > 2.4.0**. 这里的ruby是2.3，所以首先更新ruby

     ```python
     # 下载rvm, Ruby Version Manager
     curl -L https://get.rvm.io | bash -s stable
       
     # 载入 RVM 环境（新开 Termal 就不用这么做了，会自动重新载入的）
     source ~/.rvm/scripts/rvm
     
     rvm -v	# 检查是否安装正确
     
     '''再用rvm安装ruby环境'''
     rvm autolibs read-only	# 先运行这个，免得他更系统【错，不用改，不是更系统，只是装一些相关的环境。默认为 4, enable, enabled - Install missing package manager，可以reset】
     rvm install 2.6.3	# 安装2.6.3版本
     
     ruby -v # 查看版本，已经是新的了，继续gem install，安装成功
     ```



### 参考

[Steve Losh Blog](https://stevelosh.com/blog/)

[Hux Blog](http://huxpro.github.io/)

[Jekyll + Github Pages 博客搭建入门](https://www.jianshu.com/p/9f198d5779e6)

[利用 GitHub Pages 快速搭建个人博客](https://www.jianshu.com/p/e68fba58f75c)