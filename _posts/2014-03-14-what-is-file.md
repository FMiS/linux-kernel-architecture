---
layout:    post
title:     Unix文件系统
category:  基础
description: Unix文件系统的一些概念...
tags: 文件
---

Unix操作系统的设计集中反映在其文件系统上，文件系统有几个重要的特点。

### 文件 ###

Unix文件是以字节序列组成的信息载体，内核不解释文件的内容，很多编程库实现了更高级的抽象，这些库中的程序必须依靠内核提供的系统调用。从用户来看，文件被组织在一个树结构的命名空间中。除了叶节点以外，树的所有节点都表示目录名，目录节点包含下面的文件以及目录的所有信息。文件或目录名由除了『/』和空字符串『\0』以外的任何ASCII字符序列组成[^1]。大多数文件系统对文件名的长度都有限制。

[^1]: 一些操作系统允许以多种字符表示文件名，例如Unicode。

Unix的每个进程都有一个当前工作目录，它属于进程执行上下文（*execution context*），标识出进程所用的当前目录。进程使用路径名标识一个特定的文件。

### 硬链接和软链接 ###

包含在目录中的文件名就是一个文件的硬链接，或简称链接。在同一目录或不同的目录中，同一个文件可以有几个链接，因此对应几个文件名。硬链接有两个方面限制：

1. 不允许用户给目录创建硬链接。因为这可能把目录树变为环形图，从而就不可能通过名字定位一个文件。
2. 只有在同一文件系统中的文件之间才能创建链接。这带来比较大的限制，因为现代Unix系统可能包含了多种文件系统，这些文件系统位于不同的磁盘和/或分区，用户也许无法知道它们之间的物理划分。

为了克服这些限制，引入了软链接，也称符号链接。符号链接是短文件，这些文件包含有另一个文件的任意一个路径名。路径名可以指向位于任意一个文件系统的任意文件或目录，甚至可以指向一个不存在的文件。

### 文件类型 ###

Unix文件可以是：

1. 普通文件
2. 目录
3. 符号链接
4. 面向块的设备
5. 面向字符的设备
6. 管道和命名管道
7. 套接字（socket）

Unix系统的基本设计思想是万物皆为文件，但也有特殊情况，例如网络设备不可能是文件形式。