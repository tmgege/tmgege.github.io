---
layout: post
title:  "Jekyll 语法"
date:   2015-07-23 17:48:54
categories: GitHub
excerpt: jekyll是一个静态网站生成器，通过标记语言（markdown或textile）和模板引擎liquid转换生成网页
---

* content
{:toc}

## Jekyll是什么？

jekyll是一个静态网站生成器，通过标记语言（markdown或textile）和模板引擎liquid转换生成网页.
你可以通过jekyll在GitHub上创建自己的博客或项目主页。

* [如何在github上搭建一个独立博客](http://cnfeat.com/2014/05/10/2014-05-11-how-to-build-a-blog/)
* [一步步在github上创建博客主页](http://www.pchou.info/web-build/2013/01/09/build-github-blog-page-06.html)

## 目录结构介绍

### _config.yml
jekyll的全局配置都在_config.yml文件中。请参考[配置介绍](http://jekyllrb.com/docs/configuration/)。

* [JYaml项目主页](http://jyaml.sourceforge.net)
* [JYaml入门](http://jyaml.sourceforge.net/tutorial.html)++++
* [YAML主页](http://www.yaml.org)

### _includes
对于网站的头部, 底部, 侧栏等公共部分, 为了维护方便, 我们可能想提取出来单独编写, 然后使用的时候包含进去即可.
这时我们可以把那些公共部分放在这个目录下.
使用时只需要引入即可.
<pre><code>{\% include filename \%}</code></pre>

### _layouts
对于网站的布局,我们一般会写成模板的形式,这样对于写实质性的内容时,比如文章,只需要专心写文章的内容, 然后加个标签指定用哪个模板即可. 对于内容,指定模板了模板后,我们可以称内容是模板的儿子.
为什么这样说呢? 因为这个模板时可以多层嵌套的, 内容实际上模板,只不过是叶子节点而已.

在模板中, 引入儿子的内容.
<pre><code>{ { content } }
</code></pre>
在儿子中,指定父节点模板

注意,必须在子节点的顶部.
<pre><code>---
layout: default
---
</code></pre>

### _posts

写的内容,比如博客,常放在这里面, 而且一般作为叶子节点.

### _data

也用于配置一些全局变量,不过数据比较多,所以放在这里。

比如这个网站如果是多人开发, 我们通常会在这里面定义一个 members.yml 文件.

文件内容为
<pre><code>- name: 袁小康
  github: tiankonguse
  oldnick : shen1000
  nick : skyyuan
</code></pre>
然后在模板中我们就可以通过下面语法使用这些数据了.
<pre><code>site.data.members
</code></pre>

### _site

jekyll 生成网站输出的地方, 一般需要在 .gitignore 中屏蔽掉这个目录.

### index.html

主页文件, 后缀有时也用 index.md.
位于_config.yml对应base配置的目录，该目录下的所有文件会对应在sites.pages里。

### 静态资源

对于其他静态资源, 可以直接放在根目录或任何其他目录, 然后路径和平常的网站一样,按路径来找链接中的文件。本人习惯于放static目录下


## 模板语法

### 头部定义

头部定义主要用于指定模板(layout)和定义一些变量, 比如 标题(title), 描述(description), 分类(category/categories), tags, 是否发布(published), 自定义变量.
<pre><code>---
layout:     post
title:      title
category: blog
description: description
published: true # default true
---
</code></pre>

### 全局根结点

* site （_config.yml中的配置信息，通过site.[具体配置项]来引用）
* page （具体page的页面信息，通过page.[具体项]来引用）
* content （在模板中用于引入子节点的内容）
* paginator （分页信息）

### site下的变量

* site.time 运行 jekyll 的时间
* site.pages 所有页面
* site.posts 所有文章
* site.related_posts 类似的10篇文章,默认最新的10篇文章,指定lsi为相似的文章
* site.static_files 没有被 jekyll 处理的文章,有属性 path, modified_time 和 extname.
* site.html_pages 所有的 html 页面
* site.collections 新功能,没使用过
* site.data _data 目录下的数据
* site.documents 所有 collections 里面的文档
* site.categories 所有的 categorie
* site.tags 所有的 tag
* site.[CONFIGURATION_DATA] 自定义变量

### page下的变量

* page.content 页面的内容
* page.title 标题
* page.excerpt 摘要
* page.url 链接
* page.date 时间
* page.id 唯一标示
* page.categories 分类
* page.tags 标签
* page.path 源代码位置
* page.next 下一篇文章
* page.previous 上一篇文章

### paginator下的变量

* paginator.per_page 每一页的数量
* paginator.posts 这一页的数量
* paginator.total_posts 所有文章的数量
* paginator.total_pages 总的页数
* paginator.page 当前页数
* paginator.previous_page 上一页的页数
* paginator.previous_page_path 上一页的路径
* paginator.next_page 下一页的页数
* paginator.next_page_path 下一页的路径

## liquid模板语法

[项目主页](https://github.com/shopify/liquid/wiki/liquid-for-designers)

### 字符转义
有时候想输出 { 了,怎么办,使用 \ 转义即可.
<pre><code>\{ => {
</code></pre>

### 输出变量
输出变量直接使用两个大括号括起来即可.
<pre><code>{ { page.title } }
</code></pre>

### 循环
<pre><code>{ % for post in site.posts % }
    <a href="http://blog/2014/11/10/jekyll-study/{ { post.url } }">{ { post.title } }</a>
{ % endfor % }
</code></pre>

### 自动生成摘要
<pre><code>{ % for post in site.posts % }
 	{ { post.url } } { { post.title } }
	{ { post.excerpt | remove: 'test' } }
{ % endfor % }
</code></pre>

### 删除指定文本
remove 可以删除变量中的指定内容
<pre><code>{ { post.url | remove: 'http' } }
</code></pre>

### 删除 html 标签
这个在摘要中很有用.
<pre><code>{ { post.excerpt | strip_html } }
</code></pre>

### 代码高亮
<pre><code>{ % highlight ruby linenos % }
\# some ruby code
{ % endhighlight % }
</code></pre>

### 数组的大小
<pre><code>{ { array | size } }
</code></pre>

### 赋值
<pre><code>{ % assign index = 1 % }
</code></pre>

### 格式化时间
<pre><code>{ { site.time | date_to_xmlschema } } 2008-11-07T13:07:54-08:00
{ { site.time | date_to_rfc822 } } Mon, 07 Nov 2008 13:07:54 -0800
{ { site.time | date_to_string } } 07 Nov 2008
{ { site.time | date_to_long_string } } 07 November 2008
</code></pre>

### 搜索指定key
<pre><code>#Select all the objects in an array where the key has the given value.
{ { site.members | where:"graduation_year","2014" } }
</code></pre>

### 排序
<pre><code>{ { site.pages | sort: 'title', 'last' } }
</code></pre>

### to json
<pre><code>{ { site.data.projects | jsonify } }
</code></pre>

### 序列化
把一个对象变成一个字符串
<pre><code>{ { page.tags | array_to_sentence_string } }
</code></pre>

### 单词的个数
<pre><code>{ { page.content | number_of_words } }
</code></pre>

### 指定个数
得到数组指定范围的结果集
<pre><code>{ % for post in site.posts limit:20 % }
</code></pre>

### 内容名字规范

对于博客,名字必须是 YEAR-MONTH-DAY-title.MARKUP 的格式.

比如
<pre><code>2015-07-23-jekyllGrammar.md
</code></pre>