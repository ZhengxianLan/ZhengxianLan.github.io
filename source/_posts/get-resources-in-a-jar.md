---
layout: post
title: Get resources in a jar
categories: java
tags: jar
date: 2014-10-19 22:48
description: get resources in a jar
---

我们常常在代码中读取一些资源文件 (比如图片，音乐，文本等等)。在单独运行的时候这些简单的处理当然不会有问题。
但是，如果我们把代码打成一个 jar 包以后，即使将资源文件一并打包，这些东西也找不出来了
这个 jar 包内的目录为：

```
edu/hxraid/Resource.class
resource/res.txt
```

```java
// 源代码 3：
package edu.hxraid;
import java.io.*;
public class Resource {public void getResource() throws IOException{
		// 返回读取指定资源的输入流
		InputStream is=this.getClass().getResourceAsStream("/resource/res.txt");
		BufferedReader br=new BufferedReader(new InputStreamReader(is));
		String s="";
		while((s=br.readLine())!=null)
			System.out.println(s);
    }
}
```

我们将 java 工程下 /bin 目录中的 edu/hxraid/Resource.class 和资源文件 resource/res.txt 一并打包进 ResourceJar.jar 中，
不管 jar 包在系统的任何目录下，调用 jar 包中的 Resource 类都可以获得 jar 包中的 res.txt 资源，再也不会找不到 res.txt 文件了。

转自： [【解惑】深入 jar 包：从 jar 包中读取资源文件](http://hxraid.iteye.com/blog/483115) 作者:Heart.X.Raid
