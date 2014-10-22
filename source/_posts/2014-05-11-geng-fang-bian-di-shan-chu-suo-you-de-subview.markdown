---
layout: post
title: "更方便地删除所有的subView"
date: 2014-05-11 23:15:17 +0800
comments: true
categories: iOS
---

------

参考出处：http://stackoverflow.com/questions/2156015/remove-all-subviews

------

### 常用做法
通常我们移除一个`View`中的所有subView，都会使用遍历的方式去移除，代码如下：
```objective-c
    UIView *view = [[UIView alloc] initWithFrame:CGRectZero];
    UIButton *button = [UIButton buttonWithType:UIButtonTypeCustom];
    [view addSubview:button];
    UIImageView *imageview = [[UIImageView alloc] initWithFrame:CGRectZero];
    [view addSubview:imageview];
    NSLog(@"subview count is %ld", view.subviews.count);//输出2
    for (id subview in view.subviews) {
        [subview removeFromSuperview];
    }
    NSLog(@"subview count is %ld", view.subviews.count);//输出0

```

### 更方便的做法

在`iOS`上的写法是：
```objective-c
    UIView *view = [[UIView alloc] initWithFrame:CGRectZero];
    UIButton *button = [UIButton buttonWithType:UIButtonTypeCustom];
    [view addSubview:button];
    UIImageView *imageview = [[UIImageView alloc] initWithFrame:CGRectZero];
    [view addSubview:imageview];
    NSLog(@"subview count is %ld", view.subviews.count);//输出2
    // 使用makeObjectPerformSelector
    [[view subviews] makeObjectsPerformSelector:@selector(removeFromSuperview)];
    NSLog(@"subview count is %ld", view.subviews.count);//输出0

```

在`Mac`上的做法是：
```objective-c
[someNSView setSubviews:[NSArray array]];

```
