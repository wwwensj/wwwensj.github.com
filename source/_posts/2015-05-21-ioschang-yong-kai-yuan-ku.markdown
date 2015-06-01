---
layout: post
title: "iOS常用开源库"
date: 2015-05-21 10:44:02 +0800
comments: true
categories: iOS
---
这里记录的都是一些比较好的 `iOS` 开源库，涉及各个方面的内容，感觉这些开源的作者。此文章将**持续更新**

#网络请求
1、**[AFNetworking][1]** 网络请求的封装，相信不用多说，开发过`iOS`的都应该会知道；

2、**[YTKNetwork][2]** `iOS`大牛唐巧出口，官方介绍:YTKNetwork 是猿题库 iOS 研发团队基于 AFNetworking 封装的 iOS 网络库，其实现了一套 High Level 的 API，提供了更高层次的网络访问抽象；


#效果类
1、**[ScoreBoard][3]** 数字滚动变化，圆形进度条；

2、**[UIBarButtonItem-Badge][11]** `UIBarButtonItem` 和 `UIButton` 添加气泡显示；

3、**[CXPhotoBrowser][14]** 一个图片浏览的库；

4、**[MJPhotoBrowser][15]** 一个图片浏览的库；

5、**[MWPhotoBrowser][16]** 一个图片浏览的库；

#UITableView
1、**[UITableView-FDTemplateLayoutCell][4]** 优化UITableViewCell高度计算，以比较简单的方式，支持`iOS6.0+`,并且性能得到不少的提升，需要了解原理可看这篇[博文][5]

#UITabBar
1、**[RDVTabBarController][19]** 一个方便设置`tabbar`的类；


#图片加载
1、**[SDWebImage][6]** 这个相信也不用多说，图片网络请求封装，之前也有相关文章与其它库进行对比，性能不错；


#响应式编码
1、**[ReactiveCocoa][20]** 是由Github 开源的一个应用于iOS和OS X开发的新框架。RAC具有函数式编程和响应式编程的特性。它主要吸取了.Net的 Reactive Extensions的设计和实现。文章介绍在[猛戳这里][21]，还有[这个][22];

#效率类
1、**[Masonry][24]** 能让你用代码更快速地编写 `Constraint`,使用说明看**[这里][23]**；

#开发工具、脚本
1、**[KZBootstrap][7]** 一个`iOS`项目引导程序；

2、**[capimage][10]**一个使用python的PIL库写的脚本，用于将一个普通的图片去除重复部分生成一个可伸缩的图片（resizable image），这样使得图片资源的使用更加灵活和节省空间；

#账号接入
1、**[STTwitter][8]** 一个封装了Twitter官方REST Api的库，目前Twitter官方本身也有自身的`iOS SDK`,通过`Fabric`客户端可以进行配置安装；

#支付接入
1、**[PayPal-iOS-SDK][12]** `PayPal-iOS-SDK`接入的官方示例；


#扩展
1、**[NSObject-ObjectMap][18]** 是一个能将`JSON`或`XML`等格式的数据直接转换成想要的对象;

#完整项目
1、**[Coding-iOS][17]** 一个完整的`iOS`客户端源码，更新频繁，涉及的东西比较多，值得学习；

#问题类
1、**[ColorDifferenceDemo][9]** Xcode自带取色器与`UIColor`有色差的解决办法；

#其它
1、**[MNPP][13]** 一键安装一个高性能的`Web Server`,`MNPP`是Mac + Nginx + Percona + PHP 的组合简称；





[1]: https://github.com/AFNetworking/AFNetworking
[2]: https://github.com/yuantiku/YTKNetwork
[3]: https://github.com/SeniorZhai/ScoreBoard
[4]: https://github.com/forkingdog/UITableView-FDTemplateLayoutCell
[5]: http://blog.sunnyxx.com/2015/05/17/cell-height-calculation/
[6]: https://github.com/rs/SDWebImage
[7]: https://github.com/krzysztofzablocki/KZBootstrap
[8]: https://github.com/nst/STTwitter
[9]: https://github.com/Xummer/ColorDifferenceDemo
[10]: https://github.com/kejinlu/capimage
[11]: https://github.com/mikeMTOL/UIBarButtonItem-Badge
[12]:https://github.com/paypal/PayPal-iOS-SDK
[13]:  https://github.com/jyr/MNPP
[14]: https://github.com/ChrisXu1221/CXPhotoBrowser
[15]: https://github.com/azxfire/MJPhotoBrowser
[16]: https://github.com/mwaterfall/MWPhotoBrowser
[17]: https://coding.net/u/coding/p/Coding-iOS/git
[18]: https://github.com/uacaps/NSObject-ObjectMap
[19]: https://github.com/robbdimitrov/RDVTabBarController
[20]: https://github.com/ReactiveCocoa/ReactiveCocoa
[21]: http://www.devtang.com/blog/2014/02/11/reactivecocoa-introduction/
[22]: http://limboy.me/ios/2013/12/27/reactivecocoa-2.html
[23]: http://adad184.com/2014/09/28/use-masonry-to-quick-solve-autolayout/
[24]: https://github.com/SnapKit/Masonry