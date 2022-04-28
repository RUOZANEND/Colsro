---
title: 可能是Windows上最强的开源任务管理器：Process Hacker
categories: 
  - Tech
tags:
  - Windows
  - Process Hacker
  - taskmgr
date: 2020-03-03 13:12:00
updated: 2020-03-03 13:12:00
thumbnail: /imgs/process-hacker/cover.jpg
pslug: process-hacker
copyright: false
reward: false
---
**※建议从头读到尾流畅阅读。分块阅读将损失体验。**

稍微会玩点 Windows 的用户，都跟内置的任务管理器打过交道。

当然，今天不讲任务管理器，虽然经过微软百代改进，任务管理器变得更加人性化，界面更加好看，但是除此之外一无是处，真的是个彻头彻尾的垃圾。

<!--more-->

![ph01.jpg](/imgs/processhacker/ph01.jpg)
前些日子，V2EX 上有人说 TIM 电脑版会扫描硬盘，这个我还是第一次知道，就用 `Process Hacker` 挂着看，没遇到问题，就只是告诉了我曾经的同学。正好几天前，同学截图，TIM 的子进程 Q 盾 开始读磁盘，比较卡，我就向他推荐 `Process Hacker` 看看腾讯到底在扫什么东西。最终他并没有安装。

但很明显，任务管理器毛都看不出来 TIM 在读写什么。那么，有什么好点的任务管理器，能看到程序实时的读写情况？
![ph02.jpg](/imgs/processhacker/ph02.jpg)

如图所见，Process Hacker 2 是任务管理器的替代。

您可以在 https://processhacker.sourceforge.io/ 上下载它，它是 100% 开源，并且前几天还更新了的。具体可以下载源码自己编译一下。Release 是较早版本，够用，适用于大众的。

GitHub: https://github.com/processhacker 

这种垃圾又复古，又是彩虹色的一条条的界面，真的损第一印象。我刚做完程序界面，为啥会推荐这个程序？因为这个“颜色”，是有深意的。

![ph03.jpg](/imgs/processhacker/ph03.jpg)

当我关闭了一个应用程序后，这个任务管理器并不像 Taskmgr.exe，直接把这个进程剔除，而是显示红色并残留数秒，供你参考。

![ph04.jpg](/imgs/processhacker/ph04.jpg)

在选项里可以更改这些设置，比如更改高亮时间、新建进程的颜色与被结束进程的颜色。同时，别的色彩会标亮特殊进程，更好区分。

好，这个看上去也就一小功能啊，界面就这点改进还没汉化，为什么骗我下这个？

这里举个例子，我用 Bittorrent 下一个 BDRip，连到一个垃圾迅雷，就硬吸我血，上传猛增，下载没有，还屏蔽不了。不为这个人专门开防火墙，也不换程序的话，看样子很难解决了。

当然不是。

![ph05.jpg](/imgs/processhacker/ph05.jpg)

只要知道对方的 IP 地址，就是手到擒来，不说关闭通讯录，想干嘛就干嘛。

![ph06.jpg](/imgs/processhacker/ph06.jpg)

同样，腾讯 Q 盾也是不堪一击的，扫盘程序在扫描什么，都可以马上知道，不过这里还是推荐大家用沙盒安装国产程序。

当然，Taskmgr.exe 是不允许你禁用 CPU 0 的，但是 Process Hacker 可以拒绝程序使用所有 CPU。

![ph07.jpg](/imgs/processhacker/ph07.jpg)

说到占用，就不得不提现在 Taskmgr.exe 做的很好的“性能”对话框。那么在 Process Hacker 上面是怎么样的呢，一起看一下。

![ph08.jpg](/imgs/processhacker/ph08.jpg)
![ph09.jpg](/imgs/processhacker/ph09.jpg)

如图，Process Hacker 完全舍弃了美观来做硬核之事。右边也可以看出，鼠标指针悬浮在上面就可以看到针对进程的占用量统计，而不是整一个的。这样更加明细。

然后，如迅雷、百度云、aria2 这类下载应用，在你迅速拔下网线的时候，从几 M 开始，速度一直下到 1M，再下到 KB，最后短则几秒，长则十几秒的延迟，到底为什么会出现？这是因为套了个算法，而 Process Hacker 和 Taskmgr.exe 是绝对的实时，网线一拔直接归零。

另外，从图中也能轻松看出，全部是采用 MB/s 单位显示，非常实用。

![ph10.jpg](/imgs/processhacker/ph10.jpg)

所有操作选项

![ph11.jpg](/imgs/processhacker/ph11.jpg)

这里还有一些优秀功能，这里也一并介绍。下图是它的小托盘，左下角可以更改其监测内容，以占用率高低排序。

![ph12.jpg](/imgs/processhacker/ph12.jpg)

![ph13.jpg](/imgs/processhacker/ph13.jpg)

这个功能真的非常到位。有时候进程开太多，又搜索不到，比如 Chrome 怎么办？

![ph15.jpg](/imgs/processhacker/ph15.jpg)

Find Window 功能可以精确到进程中的每一个控件，长按图标然后鼠标拖动到你要监测的窗口里，马上就会帮你找到进程。右侧 X 图标是拖动后结束进程的意思，当软件无响应时，你不需要去 Taskmgr.exe 那里找无响应进程并结束（并且还有些许延迟）。直接一拖就能结束。

![ph14.jpg](/imgs/processhacker/ph14.jpg)

当然在属性窗口中还有诸多小插件（Plugin），比如 Comment 选项卡，能快速标记某个进程，重启也会保留此内容。还有，进程高亮功能也能是你快速在众多进程中找到那个该死的进程（Need-to-kill）。

![ph16.jpg](/imgs/processhacker/ph16.jpg)

选项卡一览

![ph17.jpg](/imgs/processhacker/ph17.jpg)

状态一览

![ph18.jpg](/imgs/processhacker/ph18.jpg)

我们看一下 Environment。

![ph19.jpg](/imgs/processhacker/ph19.jpg)

稍微会点电脑的编程中级高手都知道，这个 Environment 指的是系统全局变量或为环境。装过 Java 开发者多多少少都沾点过。通过这里，我们能欺骗程序或者作为伪沙盒，下面就是一个示例。

![ph20.jpg](/imgs/processhacker/ph20.jpg)

好，我们看看 Thread 选项卡。

![ph21.jpg](/imgs/processhacker/ph21.jpg)

很显而易见了。这里专门锁定这个程序的进程，并提供更详细的调试信息。

在很多的调试软件中，内存是必不可少的。这个当然也是。

![ph22.jpg](/imgs/processhacker/ph22.jpg)

还有很多实用的功能，这里就不一一列举了。当然，还有很多你可能用的到底功能，比如删除与添加服务（Service），我这里不再一一赘述了。既然你都看到了这里，也最好坚持看完吧，说不定你真的需要呢

添加服务

![ph23.jpg](/imgs/processhacker/ph23.jpg)

删除服务（有些服务真的用不到但是卸载不删，比如这个以及 Flash 中国版，Windows 服务里面删不掉，360 等虽然可以但是不推荐安装）

![ph24.jpg](/imgs/processhacker/ph24.jpg)

然后是非常非常非常非常非常有用的东西，进程上传到反病毒网站，快速检测危险。

![ph25.jpg](/imgs/processhacker/ph25.jpg)

![ph26.jpg](/imgs/processhacker/ph26.jpg)

如果你心动了，这里还提供一个功能，干掉 Windows 任务管理器 Taskmgr.exe，并以 Process Hacker 替代。当然，想用任务管理器的时候，Process Hacker 里面也是能够调用打开任务管理器的。所以可以放心用。

![ph27.jpg](/imgs/processhacker/ph27.jpg)

那么这就是图文的全部了。如果你是开发者，我认为这个软件是你不得不装的 - 除非你是浑水摸鱼或者经常摸鱼。

哦对了，你不喜欢微软雅黑？那爱什么字体换什么换什么字体，比如下面的。

![ph28.jpg](/imgs/processhacker/ph28.jpg)

> 本文转自酷安[@FlyfishMoe233](https://www.coolapk.com/feed/16922771?shareKey=MzUyNTBhYjEwNjBlNWU1ZGRmMjU~&shareUid=2969102)
> 已经原作者授权