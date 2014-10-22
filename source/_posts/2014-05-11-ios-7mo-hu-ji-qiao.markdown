---
layout: post
title: "iOS 7模糊技巧"
date: 2014-05-11 21:05:22 +0800
comments: true
categories: 翻译
---

原文出处：http://damir.me/ios7-blurring-techniques

------

你可能想在iOS 7上实现模糊的view，那么应该要怎么做呢？


## 静态模糊（blur）

要知道这并不是一种新的技术。它在iOS7之前的版本就可以使用，但是有限制的。主要的做法就是获取一个`view`的快照（即做成一张图片），然后把图片模糊化。

在iOS 6下获取快照使用`renderInContext:`方法，但它耗时很长，幸运的是，在iOS 7下在`view`里新增加了一个叫`drawViewHierarchyInRect:afterScreenUpdates:`的方法，它获得的效果是一样的。它能很快地获取一个`view`的快照。有多快？这里和`renderInContext:`方法对比一下。

![comparison-image](http://f.cl.ly/items/323c000h013V2f3R2p3b/Screen%20Shot%202013-09-16%20at%2001.12.37%20.png)

它比`renderInContext:`方法快了15倍。现在我们能获得一个快照了，那我们怎么做才能获得模糊的效果呢？`Apple`公司也帮我们封装好了方法。你可以通过`Apple`公司创建好的`UIImageEffects`这个`category`来获得模糊的效果。（找到`Sample Code`>`and search for ImageEffects`）

---

## 例子

```objective-c

// YourUIView.m (UIView)
#import "UIImage+ImageEffects.h"

-(UIImage *)blurredSnapshot
{
    // 创建图片的上下文
    UIGraphicsBeginImageContextWithOptions(self.bounds.size, NO, self.window.screen.scale);

    // 这个就是新的 API
    [self drawViewHierarchyInRect:self.frame afterScreenUpdates:NO];

    // 获取快照
    UIImage *snapshotImage = UIGraphicsGetImageFromCurrentImageContext();

    // 使用Apple的 UIImageEffect category 把模糊效果应用到图片中
    UIImage *blurredSnapshotImage = [snapshotImage applyLightEffect];

    // 或者使用 "UIImage+ImageEffects.h" 中的其它效果
    // UIImage *blurredSnapshotImage = [snapshotImage applyDarkEffect];
    // UIImage *blurredSnapshotImage = [snapshotImage applyExtraLightEffect];

    // 清理一下
    UIGraphicsEndImageContext();

    return blurredSnapshotImage;
}

```

提示：Make it a `UIView` category if you plan to blur th `sh**` out of your application.（不懂sh**是什么，干脆直接上原话好了）

---

## 实时模糊（blur）

你怎么才能做到像系统的控制中心或通知中心的那种实时模糊呢？`Apple`（经常但不总是）使用我们不能使用的自定义私有API来做的。

---

## 聪明的小技巧

`Andy Matuschak`指出，`Apple`是通过使用了一些小技巧，把静态的图片做到或者说是模拟到实时模糊的效果。这种小技巧的做法只是在旧的设备上(iPhone 5之前)使用的，在iPhone 5 上的模糊效果似乎是实时的，没有任何技巧。

然而，让我们拿通知中心（iOS 7上的）作为一个例子。他（`Andy Matuschak`）的意思是使用了技巧（我猜的），那就是获得了一个快照并从起点开始模糊它，并把它设置为通知中心的背景图。当用户拉下通知中心时，它通过增加背景图高度来模拟毛玻璃模糊效果。这给用户的印象就是实时的模糊效果。

---

## UIToolbar

最后同样重要的是，你可以更好地利用`UIToolbar` 这个类（而不是`UIView`）。确保你设置了`barStyle`属性为`UIBarStyleBlack`，`translucent`属性为`YES`。
你随意地调整`barTintColor`就行了！这样将会给你一个半透明实时模糊的view。

有没有一个更加优雅和简洁的解决办法呢？还真没有，`UIToolbar`只能当成一个toolbar使用。但是如果你真的想实时模糊`UIView`的而`Apple`又不提供这个API的话，它可能是唯一的方法了。它当然是不会被拒绝的。


