---
layout:     post
title:      "GitHub+Jekyll 静态网页的个人博客"
subtitle:   " \"建立博客了！\""
date:       2018-11-17 01:00
author:     "TanJX"
comments: true
header-img: "img/first-lovecode.jpg"
tags:
    - 博客
---

# Github jekyll建立个人网页

基于github建立个人网页，好处就在于不需要购买服务器（贫穷限制我的思想），尤其对于我这样的学生。。。大二了，想瞎折腾也有了行动的能力。所以，参考教程建立了个人博客，一个只看自己心情的博客。

### 1.注册github账号
不得不说，从刚接触```github```这个全球最大的同性交友网站，很快我就喜欢上了这个网站，因为它真的**很强大**


### 2.建立仓库
要想成功通过github的```github pages```建立个人网页，你就需要将仓库名字命名为```username.github.io```，否则后面你会发现
无法使用```github pages```
![GitHub](/img/in_post/2018-11-17---GitHub.png)

### 3.GitHub Desktop
我没用大多数教程教的使用git来配置。因为毕竟以前没接触过，短时间学习还是很麻烦的事情，所以我选择了简单的```github desktop```。在这个里面我们可以直接将仓库clone到本地。在本地修改后直接通过这个软件```commit```后push上github。
![GitHub Desktop](/img/in_post/2018--11-17---GitHub Desktop.png)

### 4.主页配置
个人网页风格按自己喜好配置，如果你懂html,css,yml,js等等，你可以选择自己编写一个好看的网页风格。但是肯定很难。。。所以你可以自己选择上网下载模板，或者在github上```fork```一个喜欢的模板然后自己再去慢慢修改。
当然这种美化不过多说。因为我们这里暂时只用建立，所以从简的建立```index.html ```
```
Hello World!
```
保存后通过Github Desktop push进仓库。

### 5.购买域名
我是在[万网](https://wanwang.aliyun.com/)购买的域名。因为国家新规定，购买的域名凡是没有实名制的一律Serverhold了。所以购买后要记得实名认证。一般实名认证是比较快的，他说一天左右，其实用不了，我的一个小时就认证通过了。然后要配置dns解析。
![万网](/img/in_post/2018-11-17---wanwang.png)
![购买后的列表](/img/in_post/2018-11-17---yuming.png)
当然我这个是我买完并且解析已经弄好了的。第一次购买的话，上面会有让你解析的提示的，还是很显眼的。
![解析配置](/img/in_post/2018-11-17---dns.png)
比较需要注意的就是，这个框起来的，我们两个都选cmake，后面的一个是@,一个是www。这样配置完还是不够的，没有实名认证的话是会处于Serverhold状态。


### 6.将域名与自己的个人网页挂在一起
我们创建一个CMADE文件，在里面添加你刚刚购买的域名。这样就可以通过购买的域名访问自己的个人网页了。
→[我的博客](https://nolearnnogrowth.top/)

### 7.按照自己的想法美化吧