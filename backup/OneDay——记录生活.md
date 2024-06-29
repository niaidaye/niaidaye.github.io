## 不一样的开始

​	想跟大家，聊聊这个博客的想法吧，其实从一开始读大学，就开始有想写，个人博客的想法，以前研究过，hexo、通过云服务+WordPress呀，等等但是由于，种种原因吧， 都没坚持✊下来，以前写的文章也没有保存下来，蛮可惜的😂

​	机缘巧合下，看见b站up主——[技术爬爬虾](https://space.bilibili.com/316183842)分享的[仅需一个Github账号，让文字在互联网中永生](https://www.bilibili.com/video/BV1GM4m1m7ZD/)视频，其中一句话引起我的共鸣“如果你想写一段文字，让100年后的人也能访问到，你会写在哪里？”。

​	我记得有**来自2300年前的“漂流瓶”** 的故事，是这样的：在战国时代一位叫公乘得和一位叫曼的人，在石头上刻下十九个字：

![公乘得守丘刻石](https://github.com/niaidaye/niaidaye.github.io/assets/27543204/03efb4e4-c1c4-4545-b346-b3579571dbbe#pic_center "监罟尤臣公乘得守丘，丌臼将曼敢谒后尗贤者")


简单翻译过来就是：监管捕鱼的罪臣公乘得在此看守陵墓，他的旧将曼敬告后来善良贤德的人。

​	在我看来，当时刻下这块石头的人，是为了告诉后人曾经发生的历史点滴，是“致两千多年后的你”。更是无意中促成古今的对话，就像“漂流瓶”一般，穿越时空传递到了两千多年后人们的眼前。

​	而且我也想将自己留存现实的痕迹，告诉给100年后的人们。

下面是Gmeek的快速开始，感兴趣的可以看看，我直接照搬官网里的教程啦

## 简介

[Gmeek](https://github.com/Meekdai/Gmeek) 一个博客框架，超轻量级个人博客模板，完全基于`Github Pages `、 `Github Issues` 和 `Github Actions`，可以称作`All in Github`。不需要本地部署，从搭建到写作，只需要18秒，2步搭建好博客，第3步就是写作。

## 一、安装

大致四步，

1. 创建仓库：点击[通过模板创建仓库](https://github.com/new?template_name=Gmeek-template&template_owner=Meekdai)，建议仓库名称为`XXX.github.io`，其中`XXX`为你的github用户名。

2. 启动Pages：在仓库的设置`Settings`中`Pages->Build and deployment->Source`下面选择`Github Actions`。

3. 开始写作：打开一篇issue，开始写作，并且**必须**添加一个`标签Label`（至少添加一个），再保存issue后会自动创建博客内容，片刻后可通过[https://XXX.github.io](https://xxx.github.io/) 访问（可进入Actions页面查看构建进度）。

4. 手动全局构建：

   通过Actions->build Gmeek->Run workflow->里面的按钮全局重新生成一次

   📝备注：这个步骤只有在修改`config.json` 文件或者出现奇怪问题的时候，需要执行。

   