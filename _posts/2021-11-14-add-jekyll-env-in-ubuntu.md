---
layout: post
title: "Linux下创建jekyll环境"
categories: linux blog
tags: jekyll blog github pages
---

## 1. 环境准备
1. 安装ruby
```
sudo apt install ruby
sudo apt-get install ruby-dev
```   
2. 安装bundle
```
sudo gem install bundler
# 切换到文件目录, 注意可先切换软件源，避免下载速度慢。
bundle install
```   
3. 安装jekylly
```
gem install jekyll
```
## 2. 测试结果
切换到可运行文件夹：  
```
bundle exec jekyll serve
```

## 3. 切换镜像源
* 外部源更改   

```
gem sources -l
gem sources --remove https://rubygems.org/
gem sources -a http://gems.ruby-china.com/
```

* 更改bundle install的单个源，更改Gemfile.   
 
```
# frozen_string_literal: true

source "https://gems.ruby-china.com/"

gemspec
```
