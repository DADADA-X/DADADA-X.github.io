---
layout:     post
title:      "github page 搭建记录"
subtitle:   " \"Hello World, Hello Blog\""
date:       2020-04-27 15:00:00
author:     "DadaX"
header-img: "img/post-bg-2015.jpg"
tags:
    - 其他
---



> 我的个人主页：[DadaX Blog](https://dadada-x.github.io/)



## About Jekyll

**Jekyll** is a static site generator with built-in support for GitHub Pages.

**依赖：**ruby gem（mac用户安装Xcode 和 Command-Line Tools后就有了；**windows用户不建议安装jekyll**）

## 各文件含义

一个较简单的 Jekyll 项目的文件结构如下：

```text
.
├── _config.yml				# 全局配置文件
└── _includes					# layout中的公共部分，可以加载这部分到布局或文章中方便重用。\
│   ├── footer.html		# 在 layout 中通过 {%include footer.html%} 引入
│   ├── header.html
│   └── nav.html
├── _layouts					# 布局，存放页面模版。在yaml头信息种进行选择。（{{ content }}标签内容填充）
│   ├── default.html
│   ├── keynote.html
│   ├── page.html
│   └── post.html
├── _posts						# 文章，格式：YYYY-mm-dd-title.md。Jekyll会根据YAML头信息自动将它们进行转换
│   └── 2020-04-26-welcome-to-jekyll.markdown
├── _site							# Jekyll完成转换生成的页面，格式为：/YYYY/mm/dd/title.html
│   └── ……						# 其他一些css, image, favicon.icon都将被完全拷贝到生成的site中。
├── index.html				# 首页，任何一目录都会查找index.html以生成html。
├── about.md					# Github Pages 会将所有非下划线开头的路径中的 markdown 文件转为对应名字的网页。
├── README.md
└── ……								# js, css, img 等，是一些原生的资源文件
```

## Steps

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

## 遇到的问题

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

2. 网页模版含义：


* 网页模版：带 {`% if true %`} {`{ var | filter }`} {`% endif %`} 一类标记的html或md

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

  

## 参考

[Steve Losh Blog](https://stevelosh.com/blog/)

[Hux Blog](http://huxpro.github.io/)

[Jekyll + Github Pages 博客搭建入门](https://www.jianshu.com/p/9f198d5779e6)

[利用 GitHub Pages 快速搭建个人博客](https://www.jianshu.com/p/e68fba58f75c)