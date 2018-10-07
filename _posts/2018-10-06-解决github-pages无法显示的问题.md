---
layout:     post
title:      解决github page无法显示的问题
subtitle:
date:       2018-10-06
author:     up2wing
header-img: img/post-bg-desk.jpg
catalog: true
tags:
    - Blog
    - github page
---

# 问题
搞了个基于Jekyll框架的github pages博客，但是往post目录push了一篇后发现无法在github.io更新显示。
出现这种问题，一般是因为新增的文章格式不对，不符合Jekyll语法，无法解析，也就不能正常显示了。但是要知道具体是在哪儿出了问题，就需要搭建一个本地环境了。

# 问题解决
按照[Setting up your GitHub Pages site locally with Jekyll](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#platform-windows)，搭建个Jekyll环境。
## 准备
### 安装ruby
从[rubyinstaller.org](https://rubyinstaller.org/downloads/archives/)下载**Ruby+Devkit Installers**，而不是RubyInstallers，否则会出现各种ERROR，因为缺少了包，不若一步到位，给自己挖坑。如果遇到莫名的错误，回头看看是不是安装了Devkit。

### 配置ruby安装源
```
$ gem sources -r https://rubygems.org/
https://rubygems.org/ removed from sources

$ gem sources -a http://gems.ruby-china.com
http://gems.ruby-china.com added to sources
```

可能会遇到的错误：
```
Error fetching https://gems.ruby-china.org/:
SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (https://gems.ruby-china.org/specs.4.8.gz)
```
参考[certificate verify failed (https://gems.ruby-china.org/specs.4.8.gz) #5](https://github.com/ruby-china/rubygems-mirror/issues/5)，按照评论安装SSL证书。

淘宝的源已经停止维护，所以会出错：
```
$ gem sources -a https://ruby.taobao.org/
Error fetching https://ruby.taobao.org/:
        bad response Not Found 404 (https://ruby.taobao.org/specs.4.8.gz)
```

### 安装Bundler
```
$ gem install bundler
```

## 使用Bundler安装Jekyll
### 创建Gemfile
进入github page目录，创建Gemfile的文件，内容如下：
```
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
```
### 安装Jekyll
```
$ bundle install
```

### 构建
```
$ bundle exec jekyll serve
Configuration file: F:/github/up2wing.github.io/_config.yml
      Generating...
             Error: could not read file ***.md: invalid byte sequence in UTF-8

```
没有用utf-8编码，vim打开重新用utf-8格式保存下就可以了。
```
set fileencoding=UTF-8
```


## 可能遇到的问题
- ERROR 1
```
$ gem install jekyll bundler
ERROR:  Error installing jekyll:
        ERROR: Failed to build gem native extension.

    current directory: F:/ruby/Ruby23-x64/lib/ruby/gems/2.3.0/gems/http_parser.rb-0.6.0/ext/ruby_http_parser
F:/ruby/Ruby23-x64/bin/ruby.exe -r ./siteconf20181006-10140-f0hea4.rb extconf.rb
creating Makefile

current directory: F:/ruby/Ruby23-x64/lib/ruby/gems/2.3.0/gems/http_parser.rb-0.6.0/ext/ruby_http_parser
make "DESTDIR=" clean

current directory: F:/ruby/Ruby23-x64/lib/ruby/gems/2.3.0/gems/http_parser.rb-0.6.0/ext/ruby_http_parser
make "DESTDIR="
generating ruby_http_parser-x64-mingw32.def
make: *** No rule to make target '/F/ruby/Ruby23-x64/include/ruby-2.3.0/ruby.h', needed by 'ruby_http_parser.o'.  Stop.

make failed, exit code 2

Gem files will remain installed in F:/ruby/Ruby23-x64/lib/ruby/gems/2.3.0/gems/http_parser.rb-0.6.0 for inspection.
Results logged to F:/ruby/Ruby23-x64/lib/ruby/gems/2.3.0/extensions/x64-mingw32/2.3.0/http_parser.rb-0.6.0/gem_make.out
Building native extensions.  This could take a while...
Successfully installed bundler-1.16.6
Parsing documentation for bundler-1.16.6
Done installing documentation for bundler after 8 seconds
1 gem installed
```
参考[Ruby-generated makefile doesn't run](https://stackoverflow.com/questions/17739607/ruby-generated-makefile-doesnt-run)，将MIGW从path环境变量中删除。


- ERROR 2
```
$ gem install jekyll bundler
ERROR:  Error installing jekyll:
        The 'http_parser.rb' native gem requires installed build tools.

Please update your PATH to include build tools or download the DevKit
from 'http://rubyinstaller.org/downloads' and follow the instructions
at 'http://github.com/oneclick/rubyinstaller/wiki/Development-Kit'
Successfully installed bundler-1.16.6
Parsing documentation for bundler-1.16.6
Done installing documentation for bundler after 8 seconds
1 gem installed
```


