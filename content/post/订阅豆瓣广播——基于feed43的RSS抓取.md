---
title: 订阅豆瓣广播——基于feed43的RSS抓取
id: 1063
categories:
  - About互联网
date: 2014-02-04 19:33:40

---



# 相关介绍

Feed43：读作feed for free，用来给网页生成feed。Feed43建立于2006年，[archive.org](https://web.archive.org/web/*/http:/feed43.com)上在2006年也有记录，可见Feed43是个稳定、可长期使用的服务。更多信息参见Feed43官网：[http://feed43.com/](http://feed43.com/)

![](http://www.itoldme.net/wordpress/wp-content/uploads/2014/02/020414_1153_feed41.png)

# 使用方法

本文以我的豆瓣广播为例，地址：[http://www.douban.com/people/2743322/statuses](http://www.douban.com/people/2743322/statuses)

打开[http://feed43.com/](http://feed43.com/)，点击Create your own feed。

## 第一步. Step 1. Specify source page address (URL)

![](http://www.itoldme.net/wordpress/wp-content/uploads/2014/02/020414_1153_feed42.png)


输入所要生成网页的网址，"Encoding"要填写网页编码格式，一般可以省略，豆瓣广播为utf-8。点击"Reload"进入第二步。



## 第二步. Step 2. Define extraction rules

这是关键的一步，选择要抓取的东西。

![](http://www.itoldme.net/wordpress/wp-content/uploads/2014/02/020414_1153_feed43.png)



**Global Search Pattern**表示搜寻的范围，可以不填，表示搜寻整个网页。**Item (repeatable) Search Pattern** 里需要填入替换的规则，这里需要懂些html知识，如果不懂或者懒得找的话就继续看文章吧。具体写法可参见小众软件的教程[《FEED43 – 为没有 Feed 的网页生成 RSS 格式订阅源[教程]》](http://www.appinn.com/feed43/)。本文介绍豆瓣广播的规则的具体写法。


打开豆瓣广播网页的源文件，经观察发现，每条广播的动态都在名为status-item的div中。

![](http://www.itoldme.net/wordpress/wp-content/uploads/2014/02/020414_1153_feed44.png)

规则开头自然是`<div class="status-item"{}>`，然后填入表示替换内容的`｛％｝`，一堆div做结尾自然不好处理，还好每个div后带个`.html -->`，就用其作为结尾。整个规则是`<div class="status-item"{*}>｛％｝.html --> `，如图所示。然后点击Extract进入下一步。



## 第三步. Step 3. Define output format

这一步用于定义RSS输出格式，豆瓣广播根本就没有标题，所以就直接如图填写就可以了。

![](http://www.itoldme.net/wordpress/wp-content/uploads/2014/02/020414_1153_feed45.png)

其中的Item Link Template，直接填写豆瓣广播的地址是比较好的处理方法。点击Preview进入下一步。



## 第四步. Step 4. Get your RSS feed

这一步就可以获得输出的RSS地址，以及一些其他不重要的功能。

![](http://www.itoldme.net/wordpress/wp-content/uploads/2014/02/020414_1153_feed46.png)

# 注意事项

1.  如果豆瓣广播的网页代码更改，则提取规则也需要更改，虽然这种情况很少发生
2.  免费版的抓取周期为6小时，付费版最快为15分钟，参见[http://feed43.com/upgrade.html](http://feed43.com/upgrade.html)
3.  抓取是以非登录状态，所以一些只有登录后才能看到的东西就看不到了