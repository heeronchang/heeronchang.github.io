---
layout: post
title:  "创建pods库"
date:   2015-09-10 15:09:54 +0800
categories: jekyll update
---

#### 概述
**创建自己到Pod库大致流程：在GitHub上创建一个repository并上传代码，然后创建podspec上传到CocoaPods**

#### 步骤
- 上传代码到Github

在GitHub上创建一个自己的库（这里记得选择一个MIT License），down到本地，把自己的项目放进去然后提交。<br/>
<img width="746" alt="2018-03-29 2 43 32" src="https://user-images.githubusercontent.com/12045500/38076063-0b644436-3367-11e8-9bea-054b8a342aa1.png">


- 创建podspec

打开终端进入工程目录，输入命令`pod spec create HEERONCategory` 创建`HEERONCategory.podspec`
创建成功如下：<br/>
<img width="351" alt="2018-03-29 2 53 16" src="https://user-images.githubusercontent.com/12045500/38076107-31c11c44-3367-11e8-8931-81d9a91488fa.png">


编辑`HEERONCategory.podspec`
```
Pod::Spec.new do |s|

  s.name         = "HEERONCategory"
  s.version      = "1.0.0"
  s.summary      = "A pod of HeeronChang named HEERONCategory."

  s.description  = <<-DESC
  this is a test pod named HEERONCategory of HeeronChang.
                   DESC

  s.homepage     = "https://github.com/heeronchang"
  s.license      = { :type => "MIT", :file => "FILE_LICENSE" }
  s.author             = { "Heeron Chang" => "heeronchang@outlook.com" }
  s.platform     = :ios, "8.0"

  s.source       = { :git => "https://github.com/heeronchang/HEERONCategory.git", :tag => "#{s.version}" }
  s.source_files  = "HEERONCategory", "HEERONCategory/**/*.{h,m}"

end
```
name 库名

version 版本

summary 库概要

description 对库做一些描述

license 许可证

source 项目的Github地址
source_files 要共享的代码 `HEERONCategory/**/*.{h,m}` 代表HEERONCategory下面的所有.h/.m文件（包括子文件夹`/**/*`代表递归文件夹）

终端输入`pod lib lint HEERONCategory.podspec --allow-warnings` 验证
验证通过如下：
<img width="255" alt="2018-03-29 3 08 09" src="https://user-images.githubusercontent.com/12045500/38076165-6357b560-3367-11e8-84f9-9722c04c69c9.png">


- 注册CocoaPods

终端输入`pod trunk register heeronchang@outlook.com 'HEERONCategory'`
进入邮箱验证：<br/>
<img width="713" alt="2018-03-29 3 10 58" src="https://user-images.githubusercontent.com/12045500/38076186-70f1c60c-3367-11e8-9b39-508cf3b8ed54.png">


然后输入`pod trunk me` 验证：<br/>
<img width="284" alt="2018-03-29 3 19 46" src="https://user-images.githubusercontent.com/12045500/38076197-773a160e-3367-11e8-9030-179e5650f47e.png">


- 上传代码到CocoaPods

输入`pod trunk push HEERONCategory.podspec --allow-warnings` 上传代码：<br/>

<img width="387" alt="2018-03-29 6 53 24" src="https://user-images.githubusercontent.com/12045500/38085422-0ccd61ec-3383-11e8-868d-5372b7aba963.png">


#### 遇到的问题
- 上传代码到CocoaPods的时候提示没有权限，库的所有者竟然是别人
解决方案一：创建库之前先搜一下是否已经存在和你要创建的库同名的库，免得最后上传代码到CocoaPods的时候出现没有权限的问题
解决方案二：暂无
- ERROR | [iOS] The `source_files` pattern did not match any file.
出现这个问题一般是` s.source_files  = "HEERONCategory", "HEERONCategory/**/*.{h,m}"` 这里的原因，查看自己的路径是否正确，然后看看要引用的文件是否提交到了GitHub
- Remote branch v0.0.1 not found in upstream origin
从远端拉不到v0.0.1branch，解决办法，首次发布时需要创建一个tag`v0.0.1`，记得`v`不能丢

#### 参考<br/>
[https://guides.cocoapods.org/](https://guides.cocoapods.org/)<br/>
[https://blog.csdn.net/potato512/article/details/51108813(子目录分层)](https://blog.csdn.net/potato512/article/details/51108813)<br/>
<http://www.cocoachina.com/ios/20161027/17869.html><br/>
<https://www.cnblogs.com/zhanglinfeng/p/6283178.html><br/>
<https://www.jianshu.com/p/283584683b0b><br/>
[https://blog.csdn.net/yst19910702/article/details/72677986 (私有pod库)](https://blog.csdn.net/yst19910702/article/details/72677986)<br/>
[http://www.cocoachina.com/ios/20150228/11206.html (私有pod库）](http://www.cocoachina.com/ios/20150228/11206.html)<br/>
[https://www.cnblogs.com/wsyuzx/p/7089564.html (私有库推荐)](https://www.cnblogs.com/wsyuzx/p/7089564.html)<br/>
[https://blog.csdn.net/blog_jihq/article/details/52614156 (私有库遇到的问题）](https://blog.csdn.net/blog_jihq/article/details/52614156 )<br/>
