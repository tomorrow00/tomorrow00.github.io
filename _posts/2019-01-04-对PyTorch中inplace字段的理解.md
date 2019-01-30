---
layout: post
title: 对PyTorch中inplace字段的理解
date: 2019-01-04 10:51:32 +0800
categories: 原创文章
tag: 深度学习框架
---

* content
{:toc}

例如torch.nn.ReLU(inplace=True)
inplace=True表示进行原地操作，对上一层传递下来的tensor直接进行修改，如x=x+3；
inplace=False表示新建一个变量存储操作结果，如y=x+3，x=y；
inplace=True可以节省运算内存，不用多存储变量。
