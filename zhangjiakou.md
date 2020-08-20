---
title:  张家口游记
heading: 张北草原天路游记-2020年8月22日
date: 2020-08-20T02:06:06.690Z
categories: ["code"]
tags: 
description:  zhangjiakou
---

行程：

- 北京（清河站）--张家口站，火车，29.5元。
- 张家口站-草原天路东段路口，租车，一天314。

清河站--张家口站--张北草原天路东路口--草原天路终点--张家口站--北京



ASCII Art：使用纯文本画流程图

ASCII字符流程图txt流程图

ascii流程图在线

Graph::Easy

纯文本流程图

ascii图

ASCIIFlow

Text 流程图

文本绘图

字符 作 图

流程图 字符画

Graph Easy 教程

无意中在一个python库中发现如下文本信息

![ascii流程图](https://gitee.com/smile365/blogimg/raw/master/sxy91/1597890929660.png)

瞬间被惊艳到了。

竟然还能用文本字符来画流程图

有时候写代码，一般的注释只能写文字性描述信息，有了纯文本流程图，以后就能把架构设计或者其他需要流程图表示的说明写在代码注释里了。或者还能把这个粘贴到makdown的文档里或者txt文档里，简直不要太方便。

好奇是怎么生成这样的纯文本字符流程图。

通过关键词“文本流程图”、“txt流程图”，“字符流程图”等一番搜索，终于知道了这叫“ascii字符流程图”。

通过[Graph::Easy](http://bloodgate.com/perl/graph/manual/index.html)这个工具即可生成上面的流程图。`Graph::Easy`是一个perl语言实现的软件包，他的功能是“用文本画图像”。

举个栗子🌰：

先在txt里写一段文字如下：

```
[ Bonn ] --> [ Koblenz ] --> [ Frankfurt ] --> [ Dresden ]

[ Koblenz ] --> [ Trier ] { origin: Koblenz; offset: 2, 2; }
  --> [ Frankfurt ]
```

用`Graph::Easy`可以转化成这样的文字
```
+------+     +---------+                   +-----------+     +---------+
| Bonn | --> | Koblenz | ----------------> | Frankfurt | --> | Dresden |
+------+     +---------+                   +-----------+     +---------+
               |                             ^
               |                             |
               |                             |
               |             +-------+       |
               +-----------> | Trier | ------+
                             +-------+
```


简直太方便了。


它怎么实现的呢

主要借助graphviz这个软件包，这是一个提供不同平台的二进制包，安装好以后可给其他任意语言调用，比如python、perl、c等。Graphviz是一个开源的图形可视化软件，通过DOT语言描述节点、边、属性、形状、箭头、颜色等信息，然后Graphviz即可生成图像。

灵活性非常高，[官网](https://graphviz.org/gallery/)有很多通过Graphviz生成图像的例子，简直无所不能。

![enter description here](https://gitee.com/smile365/blogimg/raw/master/sxy91/1597893101723.png)

`Graph::Easy`对`Graphviz`的DOT语言中流程图的部分进行了二次包装，提供了更简单的DSL语言，从此描述一个流程图像码文字一样简单。不用关心图像里面如何布局，箭头颜色、边从哪里画等等复杂的信息。


安装Graph::Easy的步骤如下：

- 首先需要安装 graphviz 软件包，可以在graphviz官网下载；mac用户可以 brew install graphviz；其他linux发行版参考官网。
- 安装perl；mac和linux用户可以略过；一般系统自带，没有的话和windows一起去perl官网查询如何安装; 据说windows下有傻瓜包activeperl；请自行搜索。
- 安装cpan; 这个是perl的软件包管理，类似npm, pip, apt-get; mac下直接在命令行输入 cpan 命令，一路next即可。其他系统参考cpan官网
- 安装Graph::Easy ;这一步就很容易了；在命令行输入cpan进入cpan shell；然后输入 install Graph::Easy即可。
使用

#### Graph::Easy的dsl语法

注释
```
# 用#号注释，#号后面有空格。空格通常没有什么影响，多个空字符会合并成一个，换行的空字符会忽略
# 空格和换行通常没有什么影响，多个空字符会合并成一个，换行的空字符会忽略
```




参考文档  
- [Graph::Easy的dsl语法](http://bloodgate.com/perl/graph/manual/syntax.html)
- [Graph::Easy文档译文](https://weishu.gitbooks.io/graph-easy-cn/content/)