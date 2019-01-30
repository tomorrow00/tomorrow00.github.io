---
layout: post
title: PyTorch的自适应池化Adaptive Pooling
date: 2019-01-07 23:04:29 +0800
categories: 原创文章
tag: 深度学习框架
---

* content
{:toc}

自适应池化Adaptive Pooling是PyTorch含有的一种池化层，在PyTorch的中有六种形式：

> 自适应最大池化Adaptive Max Pooling：
torch.nn.AdaptiveMaxPool1d(output_size)
torch.nn.AdaptiveMaxPool2d(output_size)
torch.nn.AdaptiveMaxPool3d(output_size)

> 自适应平均池化Adaptive Average Pooling：
torch.nn.AdaptiveAvgPool1d(output_size)
torch.nn.AdaptiveAvgPool2d(output_size)
torch.nn.AdaptiveAvgPool3d(output_size)

具体可见[官方文档](https://pytorch.org/docs/stable/nn.html?highlight=adaptiveavgpool#torch.nn.AdaptiveMaxPool1d)。
其特殊性在于，输出Tensor的size都是output_size。
```
官方给出的例子：
>>> # target output size of 5x7
>>> m = nn.AdaptiveMaxPool2d((5,7))
>>> input = torch.randn(1, 64, 8, 9)
>>> output = m(input)
>>> output.size()
torch.Size([1, 64, 5, 7])

>>> # target output size of 7x7 (square)
>>> m = nn.AdaptiveMaxPool2d(7)
>>> input = torch.randn(1, 64, 10, 9)
>>> output = m(input)
>>> output.size()
torch.Size([1, 64, 7, 7])

>>> # target output size of 10x7
>>> m = nn.AdaptiveMaxPool2d((None, 7))
>>> input = torch.randn(1, 64, 10, 9)
>>> output = m(input)
>>> output.size()
torch.Size([1, 64, 10, 7])
```
这是因为，Adaptive Pooling的卷积核大小kernel_size由输入input_size和输出output_size决定：kernel_size=(input_size + output_size - 1) / output_size，padding=0，stride=1。

这样就保证了不管输入Tensor的size是多少，输出Tensor的size都是output_size。
