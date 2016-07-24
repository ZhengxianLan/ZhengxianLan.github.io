---
title: event-loop
layout: post
date: 2015-10-23 13:23:38
categories: ['javascript','nodejs']
tags: ['eventloop']
---

一直对 eventloop 好奇
以前尝试写个玩具监控文件夹变动，最初是用 java 开个线程不断遍历，效率低，cpu 高
知道 java7 才提供个 nio，有 file system notify api
但是，不明白为什么 event loop 就不消耗 cpu
看了 [how would you impletement a basic event loop](http://stackoverflow.com/questions/658403/how-would-you-implement-a-basic-event-loop)

```c++
void App::exec() {for(;;) {vector<Waitable> waitables;
        waitables.push_back(m_networkSocket);
        waitables.push_back(m_xConnection);
        waitables.push_back(m_globalTimer);
        Waitable* whatHappened = System::waitOnAll(waitables);
        switch(whatHappened) {case &m_networkSocket: readAndDispatchNetworkEvent(); break;
            case &m_xConnection: readAndDispatchGuiEvent(); break;
            case &m_globalTimer: readAndDispatchTimerEvent(); break;}
    }
}
```

大致意思就是对要监控的事件进行注册，并交由操作系统事件管理机制托管，然后等待事件发生，此时，event loop 是阻塞的。
在等待 event occurs 之前，loop 其实是被休眠的，block 的。相应的 process 也是 idle 的。
当 event occurs，loop 会被唤醒，处理完毕，进入下一轮循环。


[另有 libuv 的解释](http://nikhilm.github.io/uvbook/basics.html#event-loops)
对于系统来说，线程是最小运行单位，libuv 和操作系统会跑个后台线程轮巡任务和执行其他任务，但这都不需要我们关心，我们只需认为 nodejs 是单线程就可以
[更为详细的解释](http://docs.libuv.org/en/v1.x/design.html#the-i-o-loop)
