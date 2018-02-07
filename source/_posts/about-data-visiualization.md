---
title: 有关数据可视化学习
author: TigerFED
clearReading: true
autoThumbnailImage: false
thumbnailImagePosition: left
metaAlignment: left
coverSize: partial
coverMeta: in
comments: true
meta: true
actions: true
date: 2018-02-07 16:39:22
tags: [ data, d3, chart, visualization ]
keywords: [ data, d3, chart, visualization ]
thumbnailImage:
coverImage: https://tigerbrokers.github.io/assets/images/william-iven-22449.jpg
coverCaption:
gallery:
---
数据可视化（Data Visualization）和 信息可视化（Information Visualization）是两个相近的专业领域名词。狭义上的数据可视化指的是将数据用统计图表方式呈现，而信息可视化则是将非数字的信息进行可视化。前者用于传递信息，后者用于表现抽象或复杂的概念、技术和信息。而广义上的数据可视化则是数据可视化、信息可视化以及科学可视化等等多个领域的统称。
<!-- excerpt -->

# 概要
数据可视化主要旨在借助于图形化手段，清晰有效地传达与沟通信息。

> 数据可视化（Data Visualization）和 信息可视化（Information Visualization）是两个相近的专业领域名词。狭义上的数据可视化指的是将数据用统计图表方式呈现，而信息可视化则是将非数字的信息进行可视化。前者用于传递信息，后者用于表现抽象或复杂的概念、技术和信息。而广义上的数据可视化则是数据可视化、信息可视化以及科学可视化等等多个领域的统称。
> ——《数据可视化之美》

# 基础概念

1. 维度
2. 维度类型和转换

# 数据可视化技术栈
* 基础数学：三角函数、线性代数、几何算法
* 图形相关：canvas、svg、webgl、计算图形学、图论
* 工程算法：基础算法、统计算法、常用的布局算法
* 数据分析：数据清洗、统计学、数据建模
* 设计美学：设计原则、美学评判、颜色、交互、认知
* 可视化基础：可视化编码、可视分析、图形交互
* 可视化解决方案：图表的正确使用、常见的业务的可视化场景

# 基于Web的数据可视化技术
## 底层技术规范
1. SVG：可缩放矢量图形（Scalable Vector Graphics），是基于可扩展标记语言（标准通用标记语言的子集）用于描述二维矢量图形的一种图形格式。
2. Canvas 2D：Canvas 通过 JavaScript 来绘制 2D 图形，通过逐像素来进行渲染。
3. Canvas 3D WebGL：WebGL（Web Graphic Library）是一个 JavaScript API，用于在任何兼容的 Web 浏览器中渲染 3D 图形。WebGL 程序由用 JavaScript 编写的控制代码和用 OpenGL 着色语言（GLSL）编写的着色器代码构成，这种语言类似于 C 或 C++，可在 GPU 上执行。

比较流行的基础绘图库，基于 SVG 的有 snap.svg、rapheal.js 等，基于 Canvas 2D 的有 zrender、g 等，基于 WebGL 的有 three.js、SceneJS、PhiloGL 等，这些基础绘图库可以让上层封装更简单容易。

## Web数据可视化类库
* D3
* Highcharts
* ECharts

# 如何选择图表
![](/assets/images/15179928837522.jpg)

# 扩展
## 数据分析师技能
1. 用熟Excel
2. 数据可视化
    3. 图表选择
    4. 报表制作
    5. 信息图和BI系统
6. 分析思维的训练
    7. 金字塔原理
    8. 掌握思考框架：SMART, 5W2H, SWOT, 4P，六顶思考帽
9. 数据库学习
    1. SQL语言掌握
    2. MapReduce原理
3. 统计知识学习
    4. 均值、中位数、标准差、方差、概率
    5. 假设检验、显著性、总体和抽样
    6. 数据推导
7. 业务学习
    8. 用户行为
    9. 产品和运营相关，如行业指标：活跃用户数，洛用户率，留存率，流失率，传播系数等
1. Python/R语言学习
    2. 数据挖掘
    3. 爬虫
    4. 可视化报表

# 参考
https://antv.alipay.com/zh-cn/vis/blog/vis-introduce.html
https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE%E5%8F%AF%E8%A7%86%E5%8C%96
https://www.zhihu.com/question/19929609
http://www.cbdio.com/BigData/2017-06/02/content_5531235.htm

