---
title: 有关Node.js多线程thread
date: 2013-09-15 00:00:00 Z
published: true
categories:
  - article
tags:
  - Node.js
layout: post
author: ijse
comments: true
---

Node.js 是单进程、无阻塞，异步事件驱动执行，相比阻塞式的程序，异步执行的特性使Node.js非常适合于网络编程，但是这却无法最大化利用多核处理器的优势。 node.js的作者Dahl的建议是启动多个程序进程，利用某些通信机制来协调各项任务。

后来，Node.js提供了Cluster模块，官方文档的描述是：
<!-- more -->

> A single instance of Node runs in a single thread. To take advantage of multi-core systems the user will sometimes want to launch a cluster of Node processes to handle the load.

其实底层是对`child_process.fork()`的封装，用Cluster可以让多个进程共同绑定一个端口号。

通常使用Cluster的时候，创建与服务器CPU术数相同的worker数量，这样可以有效利用CPU的多个核心，当前CPU核心数可以通过命令`os.cpus().length`获得。

但是Cluster的这种解决方案并不适用于密集CPU型任务，假如一个HTTP请求需要较长的CPU处理时间，那么它就会一直阻塞这个子进程，直到计算完成返回结果。若服务器CPU数为4，那么服务器最多支持4个任务同时运行，若4个任务都是CPU密集型计算，那么第5个请求只能等待了。所以，cluster并不能解决单线程模型的cpu密集计算带来的阻塞问题。

通常的解决方案是使用C/C++的addon方式，将CPU密集型部分改写成为C/C++实现，以此来实现多线程运行。这当然不会是一种很理想的情况，如果开发人员不了解C/C++，而且这调试起来也很不方便。

国外jorgechamorro写了一个库[tagg](https://github.com/xk/node-threads-a-gogo), 可以实现Node.js对线程的支持，后来，国内snoopy又将它进行了重写，发布了[tagg2](https://github.com/DoubleSpout/node-threads-a-gogo2), 并添加了更多特性，包括跨平台和对Node.js v0.10版本的支持，API使用起来也更简单方便。

通过使用tagg2, 将CPU密集计算部分改用tagg2去处理，这样便可以有效利用系统的多线程特性。
