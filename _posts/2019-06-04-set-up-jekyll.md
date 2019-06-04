---
title: 关于个人博客的set up
layout: post
tags: jekyll
categories: 博客
---
以前很多人都用微博（好像没落了），技术人用csdn和博客园，好像big不够，怎么办，其实好几年前我也搭建过github的主页，但是很久没维护，故作废了。
  本文内容为个人博客及jekyll writer工具写博客的指导，本人很菜，各种百度。总结一下，基本原则是要坚持，coder那必须的遇到各种问题困难，迎难而上你就会变得更强。
  前人博客写教程质量参差不齐，很多人断章取义，百度给你的检索结果也很奇怪，每个人的设备不一样，网络和硬件也不同，软件环境可能差异很大。（本人用的实验室的机器：GTX1070 i7-8700 16GRAM Windows 10 +Java 1.8+Idea 2017（这些都是唬人的）正经的是Ruby ）
[![TIM图片20190604144004.png](https://i.loli.net/2019/06/04/5cf61252406c954955.png)](https://i.loli.net/2019/06/04/5cf61252406c954955.png)
看到这里很多时候不知道用哪个版本，我推荐你用低的，我从2.6一直装到2.3，好像对Windows不友好。DEVELOPMENT KIT这个也得有，不然你到时候启动就发现缺少好多东西。
下载Ruby DevKit DevKit-mingw64有64位或32位可以选择，下载完成后打开，会询问解压位置（假定：C:\RubyDevKit），解压。解压后用cd C:\RubyDevKit定位到该目录下，然后运行ruby dk.rb init,初始化成功后会在目录下看到一个新文件config.yml，打开之后，添加本机ruby的安装路径，如图，然后运行ruby dk.rb install 即可完成。

插播一个Git相关的知识：绑定远程仓库，方便提交：

git remote add origin git@github.com:username/username.github.io.git
1
介绍几个常用命令：

git add . //添加文件
git commit -m "commit-messages" //提交本地仓库
git push origin master //提交远程仓库
git pull //拉取远程文件，与以下命令类似
git branch temp //创建本地分支
git fetch origin master:temp
git merge master
--------------------- 
原文：https://blog.csdn.net/KNIGH_YUN/article/details/79774344 
DEVELOPMENT KIT 装好之后进行如上ruby dk.rb init,初始化等操作再来进行下面的安装。
Ruby安装完成它自带gem不用再麻烦了
安装jekyll：cmd命令行输入 gem install jekyll
这里Windows还是会报错
* 使用 bundle exec jekyll s 命令就可以运行了，如果提示没有安装 bundler ，就 gem install bundler 再 bundle install
* 可能还会提示没有安装其他组件，记下名称， gem install xxx 就可以了
* 来一句 jekyll --version如果能看到jekyll版本（我的是3.8.5，这是gem命令装的我们不知道下啥版本）到这一步基本上的工作已经完成了。百度了的博客呢，会介绍两种方向：一种是在本地生成静态网页，可以说就像本地测试，做个好看的界面再发布，不然我们怎么知道做的好不好看。这里呢，也不要cmd那个黑白框了，目录不好跳，直接在E盘右键Git here啊
[![TIM图片20190604150131.png](https://i.loli.net/2019/06/04/5cf61752bcff828399.png)](https://i.loli.net/2019/06/04/5cf61752bcff828399.png)
这个过程要过很久的，这也是博客教程的问题，有些过程依据机器的不同而需要等待的时间也不同。
等它执行完成我们会看到E盘出现了blog目录并且很多自带的文件生成了。再在这个目录里面右键Git here
[![TIM图片20190604150539.png](https://i.loli.net/2019/06/04/5cf61849cec9064987.png)](https://i.loli.net/2019/06/04/5cf61849cec9064987.png)
如图所示，localhost：4000就是你的博客页面了。
然后我们很多时候是copy的jekyll模板已经有这个东西了，我们想要的是往里面写博客内容想我这样的，那就是第二种方向了：
装jekyll writer 这个就是Windows的EXE毫无难度，打开之后首要任务链接上你的github.io
[![TIM图片20190604150920.png](https://i.loli.net/2019/06/04/5cf619781ea3879408.png)](https://i.loli.net/2019/06/04/5cf619781ea3879408.png)
点create new token  就会出现
[![TIM图片20190604151302.png](https://i.loli.net/2019/06/04/5cf61a0e0e17919606.png)](https://i.loli.net/2019/06/04/5cf61a0e0e17919606.png)
把生成的token帖回来就可以链接上你的github.io博客了
此处盗图一张：[![20180401093410604.jpg](https://i.loli.net/2019/06/04/5cf61a6d2592224816.jpg)](https://i.loli.net/2019/06/04/5cf61a6d2592224816.jpg)

写博客就无法避免上传图片，图床就是这么一个地方，就是一个网站，你发自己的图片上传到它的网站，然后它给你一个这个图片的链接，插入博客中就能显示图片了。

推荐一个知名的，七牛云https://portal.qiniu.com/，注册完实名认证后有一些优惠。 
还有一个神奇的网站：https://sm.ms/，也能用

然后在 jekyll writer中配置一下： 
--------------------- 



好了，大家也可以像我一样用自己的github写博客了。
特别鸣谢（CSDN博客）：https://blog.csdn.net/KNIGH_YUN/article/details/79774344