---
layout: post
title: "Unicode 的换行符号 e280a8 导致 JS 报错"
date: 2014-05-14 21:52:40 +0800
comments: true
categories: 疑难杂症
---

### 出现的现象

项目是使用 [phoneGap（现称 Cordova ）][1] 框架，`JS` 端 从 `Navite` 端获取到处理数据后，其实是段可执行的 `JS` 代码，当要执行它时，报 **“SyntaxError: Unexpected token ILLEGAL”** 的错误，意思是不合法的记号，代码无法再执行下去，使用肉眼在 `Linux` 下看到是空白行，在 `Windows` 下看到的是一些特殊字符， [phoneGap][1] 的关键代码为


```javascript

// 获取到 Navite 端的处理结果
var msg = prompt("", "gap_poll:");
if (msg) {
    setTimeout(function() {
        try {
            // 用 eval 执行它
            var t = eval(""+msg);
        }catch (e) {
            // catch 到错误是 SyntaxError: Unexpected token ILLEGAL
            console.log("JSCallbackPolling: Message from Server: " + msg);
            console.log("JSCallbackPolling Error: "+e);
        }
    }, 1);
}
      
```

### 问题分析

怀疑就是此特殊字符引起的 `JS` 语法错误，经服务器的同学查证，发现此字符的 `UTF-8` 的编码是 `e280a8`，这货是什么呢？ `Google` 后发现它的 `Unicode` 编码是 `\u2028`，正是 `Unicode` 下的换行符号。此符号对于 `JS` 下执行 `eval` 函数时出错，造成语法错误。由于出现此问题的是线上环境，当时服务器同学已经把数据库有问题的那个用户的字段给修改好了，因此我决定使用代码来模拟一下，模拟的思路是：

> * 用 `Java` 来创建一个包含 `Unicode` 编码的换行符号 `\u2028` 的字符串；> 
> * 再转成 `UTF-8` 编码的字符串；
> * 回传给 `JS` 端

代码如下：

```java

//Unicode编码的字符中，意义是”HelloWorld,*HelloWorld”,*代表\u2028,它代表Unicode下的换行符
String nicknameUnicode = "\u0048\u0065\u006C\u006C\u006F World\u2028,\u0048\u0065\u006C\u006C\u006F World";
String nicknameUTF8 = null; 
try {
    //转成utf-8,这里就包括Unicode的换行符号，返回给js会报错
    nicknameUTF8 = new String(nicknameUnicode.getBytes("UTF-8"), "UTF-8");
} catch (UnsupportedEncodingException e) {
    e.printStackTrace();
}
     
// 把这个 nicknameUTF-8传给JS端，代码非关键，就不贴了

```

果然，`JS` 端同样报了 **“SyntaxError: Unexpected token ILLEGAL”** 的错误


### 如何解决
#### 方式一：Java端解决

既然我们知道是 `Unicode` 的换行符号 `\u2028` 引起的，那就对症下药，把这个换行符号去掉就好了，因为项目用的是 `Java`，因此使用 `Java` 的方法去掉，其它语言应该也是一样的，只不过写法不同而已，发现这个换行符号用 `trim()` 方法是没法去除的，但可以用正则表达式去掉，代码如下：

```java
     
//正则替换换行符号，假如\u2028放在首尾，用trim()方法，也是替换不了的
nicknameUTF8 = nicknameUTF8.replaceAll("\\s*", "");

```

好了，一切正常了。

#### 方式二：JS 端解决
当从 `prompt` 函数拿到处理结果后，判断一下，如果存在这样的字符，则把它替换为空，再去执行 `eval` 函数，代码如下：

```javascript

// 获取到 Navite 端的处理结果
var msg = prompt("", "gap_poll:");
var lineBreakCharReg = /\u2028|\u2029/g;
// 如果存在换行符号，则替换为空。
if (lineBreakCharReg.test(msg)) {
	msg = msg.replace(lineBreakCharReg, "");}
if (msg) {
    setTimeout(function() {
        try {
            // 用 eval 执行它
            var t = eval(""+msg);
        }catch (e) {
            // catch 到错误是 SyntaxError: Unexpected token ILLEGAL
            console.log("JSCallbackPolling: Message from Server: " + msg);
            console.log("JSCallbackPolling Error: "+e);
        }
    }, 1);
}
      
```


### 小小总结

>* `JS` 从服务端或者客户端获取到结果时，特别要注意是否包括换行符号，包括 `Unicode` 和 `UTF-8` 等的编码的换行符号，因为会引起 `JS` 的语法问题；
>* 那么其它的编码格式下的换行符号会不会也引起 `JS` 的语法问题呢？这里没有再进一步测试，有兴趣的可以试一下，告诉我，^_^；
>* 正则 `\s` 可去掉 `Unicode` 和 `UTF-8` 的换行符号，其它的编码换行我猜也可以的。
>* 后来经验证，\u2029也会导致这样的问题，这个是 `Unicode` 下的段换行符号。
### 参考的资料

[List of Unicode characters that should be filtered in output? --stackoverflow][2]

[1]: http://cordova.apache.org/
[2]: http://stackoverflow.com/questions/10556875/list-of-unicode-characters-that-should-be-filtered-in-output


