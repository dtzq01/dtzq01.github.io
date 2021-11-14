---
layout: post
title: "Linux下创建jekyll环境"
categories: linux ssl
---

## 环境准备
1. 安装ruby
```
sudo apt install ruby
sudo apt-get install ruby-dev
```   
2. 安装bundle
```
gem install bundler
bundle install
```   
3. 安装jekylly
```
gem install jekyll
```
## 测试结果
切换到可运行文件夹：  
```
bundle exec jekyll serve
```