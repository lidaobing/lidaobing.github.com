---
layout: post
title: "[备忘] Jekyll FAQ"
tags: jekyll
---

纪录一下这次把 blog 迁移到 Jekyll 时遇到的问题

### 1. Jekyll 是一个静态页面生成器

首先要记住的是 Jekyll 是一个静态页面生成器，所以不会有用户系统，不会有权限控制，很多常规站点有的东西它都没有。
如果需要一些高级功能，优先的选择就是 javascript ＋ 外部服务，比如评论可以用 disqus, 搜索可以用 google search。

### 2. 文档位置

* [Jekyll Template Data](https://github.com/mojombo/jekyll/wiki/Template-Data): 主要是 site, page, paginator 的详细说明。
* [Liquid](https://github.com/shopify/liquid/wiki/liquid-for-designers): Jekyll 使用的模板系统是 liquid, 这个是 liquid 的使用手册。
* [Jekyll Sites](https://github.com/mojombo/jekyll/wiki/Sites): 范例站点，如果实在某个功能不知道如何实现，就可以在这页找找，也许有人实现过。

### 3. sitemap.xml, rss.xml, atom.xml

这个其实比较简单，因为 Jekyll 的模板系统支持 xml, 范例可以参考[这个站点的源码](https://github.com/lidaobing/lidaobing.github.com)，另外 Jekyll 系统的 post 只有一个实践，如果你更新了一个 blog, 并且想在这些文件中体现，你需要在你的 post 中引入一个新的变量，并在模板文件中体现，比如 post 的 头部如下所示:

{% raw %}

      ---
      layout: post
      title: 机房网段更改后，如何避免在机房逐台更改IP?
      lastmod: 2012-06-01
      ---

      some text

{% endraw %}

然后 site.xml 如下所示
{% raw %}

      ---
      layout: nil
      ---
      <?xml version='1.0' encoding='UTF-8'?>
      <urlset xmlns='http://www.sitemaps.org/schemas/sitemap/0.9'>
          {% for post in site.posts %}
          <url>
              <loc>{{ site.url }}{{ post.url }}</loc>
              {% if post.lastmod %}
              <lastmod>{{ post.lastmod | date_to_xmlschema }}</lastmod>
              {% else %}
              <lastmod>{{ post.date | date_to_xmlschema }}</lastmod>
              {% endif %}
          </url>
          {% endfor %}
      </urlset>

{% endraw %}

### 4. tag, category

其实这两个功能很类似，我的站点就只使用了 tag, 没有用 category。 tag 的使用比较简单，把头部加入如下的一行即可。

      tags: linux debian qterm

如果你的 tag 中可能含有空格，也可以使用

      tags: ["et al", "foo bar"]

#### 4.1 生成 tag 列表页

如果想偷懒，直接把 tag 链接到 google search 即可，但那样的结果不太准确，我的方法是分两步

1. 用 Rakefile 中生成所有 tag 的文件
2. 引入一个 tag 专用的 layout

生成 tag 文件的代码如下:

<script src="https://gist.github.com/2863678.js?file=gistfile1.rb">
</script>

tag 的 layout 文件

<script src="https://gist.github.com/2863687.js?file=gistfile1.html">
</script>


#### 4.2 生成 tag cloud

我个人对 tag cloud 没有兴趣，所有只做了 tag lists, 不过方法类似，同样也是用 Rakefile，范例如下

<script src="https://gist.github.com/2863697.js?file=gistfile1.rb">
</script>

然后在你的 sidebar 中 include 生成的文件即可。 category 也是类似。每次更新后运行 `rake build` 即可 (build 依赖前面的两个任务)。

### 5. PS

在 markdown 文件中 embed gist 文件时，记得把 </script> 放在第二行，比如

      <script src="https://gist.github.com/2863678.js?file=gistfile1.rb"></script>

会导致所有之后的文字不现实，改成如下形式后即可工作

      <script src="https://gist.github.com/2863678.js?file=gistfile1.rb">
      </script>





