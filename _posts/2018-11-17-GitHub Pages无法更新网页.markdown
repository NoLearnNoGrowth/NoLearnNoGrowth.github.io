---
layout:     post
title:      "GitHub+Jekyll 静态网页的个人博客"
subtitle:   " \"建立博客了！\""
date:       2018-11-17 01:00
author:     "TanJX"
header-img: "img/first-lovecode.jpg"
tags:
    - 博客
---

## GitHub Pages 无法更新网页
我在美化自己博客的时候，发现自己update了文件，通过GitHub Desktop```push``` 上去了 ```repository```，但是在网页上没有更新。
我一开始以为是延迟没有在意，但是后来发现过了一晚上都没有更新。而且平时的延迟也不可能那么长。才发现自己的邮箱里被GitHub发了邮件

```
The page build failed for the `master` branch with the following error:

The tag `endif` on line 125 in `/_layouts/post.html` is not a recognized Liquid tag. For more information, see https://help.github.com/articles/page-build-failed-unknown-tag-error/.

For information on troubleshooting Jekyll see:

  https://help.github.com/articles/troubleshooting-jekyll-builds

If you have any questions you can contact us by replying to this email.
```

这是我的邮件原件。看起来是我在更新search功能时在/_layouts/post.html里的修改有语法错误，然而尴尬的是我并不懂这种语言，因为我没学过。我尝试着把它说的那个```endif```删掉了，但是又来了一封邮件，是说```endfor```也出错了。这就尴尬了。

所以我只能针对我这种不懂html的表示，我们只能通过GitHub Desktop的```history```来找到自己是在更新哪里时出了错。
![GitHub Desktop](/img/in_post/2018-11-17---1.png)

我选择了删除导致错误的那一部分更新的内容，删除后再一次push上去，这一次终于没有发给我邮件，很快我的网页也就成功的再次更新了。

现在想想，要是我能学学语法，没准就可以直接修改而无需删除了呢。