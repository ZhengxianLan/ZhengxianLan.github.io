---
title: intelljIdea
date: 2015-09-24 15:20:20
categories: tool
tags: ['intelljIdea','java','tomcat']
---

# tips for using intellij idea

## 热部署
- 热部署：tomcat server--deployment-- 点击加号 --extenal source 指向 webapp 所在的目录。tomcat 需要以 debug 模式启动，启动后修改 java 文件后，Ctrl[+Shift]+F9 编译文件，直接就热部署了。也可以打个 devevm 的 jvm 补丁，直接就热部署。
- 注意：On ‘Update’ actions 和 On frame deactivation 都要选择“Updatre classes and resources”。还需要以 debug 模式启动。能获得热部署功能。但是还是不知如何将工程发布到 tomcat 安装目录，不过也没必要。
- 关于热部署, 在 web 目录下新建文件夹 META-INF，在其中新建一个文件 context.xml，内容为 <context reloadable="true"> </context>，配置部署时 2 个框都选择 update classes and resources，然后以 debug 模式启动 tomcat. 关于这个配置文件，可以参考 tomvmcat 自带的 docs，很详细。
 - 好奇怪。。。现在不加 context.xml 也能 hot swap。。。到底是怎么了。。。。不过不支持增加方法等结构性的改变。估计还得打补丁。改了类结构导致不能 reload，那么回退之，就可以继续 debug。reload
好郁闷，又没自动 swap，不过手动编译之后，倒是自动 swap 了。然后，页面若是 checkbox 数组，那么使用 request.getParameterValues("hobby");
- ** 远程调试 **  
	（前提：远端服务器上已经有了项目文件）, 关于这些，还是请参考 tomcat 的文档...
	- 本例假设为 tomcat6, 在 bin 目录中新建 setenv.bat, 内容如下：
	`SET CATALINA_OPTS=-server -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=5005
	address` 要和 intellj idea 内设置的一致，5005 为 idea 默认的远程调试端口，可更改。
	然后启动 tomcat, 可以看到 `Listening for transport dt_socket at address:5005`
	在 Linux 的话需要新建的是 setenv.sh. 并且等号右边的参数必须用双引号括起来。
	否则会报错：-Xdebug -Xnoagent. 找不到。
	其实这些都可以在 catalina.sh 内找到，大约在 256 行。若是懒得配那么多参数，直接 `./catalina.sh jpda run` 就好了，端口默认为 8000. 可根据情况自行修改。
	- 而在 intellj idea 这一端，在 configurations，新建一个 "Remote"，设置 host 和 port，设置要诊断的模块。就可以开工了。
	运行之，显示：Connected to the target VM, address: 'localhost:5005', transport: 'socket'。
	即表示成功。然后可以试试，在 server 端改下 jsp 之类的文件，然后随便改下 web.xml(譬如添加空行) 并保存，tomcat 会自动重新加载工程。

## 常用快捷键

keymaps       		|  作用
------------------- | ------------------------
F7 ,Shift + F7		|	在文件 diff 到时候，，跳转到不同到地方
Ctrl+Alt+S			|	配置服务器，，--application server ：配置整个平台的 server
Ctrl+J				|	查看更多快捷键用
Ctrl+Alt+F12 	  	|	在资源管理器中显示
Alt+(up/down arrows)|	在方法级别上跳转
Ctrl + Delete		|	Delete to word end
Ctrl + Backspace 	|	Delete to word start
Ctrl+Shift+V		|	智能粘贴板
Ctrl + F12 			|	File structure popup
Shift + F11 		|	Show bookmarks
Alt + Delete 		|	Safe Delete
Ctrl+Alt ←/→  		|	返回上次编辑的位置
Ctrl+Alt+B 			|	跳转到实现处，若用 myeclipse，按下 Ctrl+ 鼠标悬浮于方法上可选择 implemention
Ctrl+U				|	跳转到超类方法，这个和上面的 Ctrl+Alt+B 一结合，真是太好用啦，怪不得说 idea 是 program with pleasure
Ctrl+O 				|	只产生超类方法，而 Alt+insert 可以产生比较好的实现。
Ctrl+H 				|	类继承树
Ctrl+Shift+H 		|	方法继承树
Ctrl+Alt+H 			|	方法调用链. 按 (c^+b)/F4，光标会直接跳到 source，在此 tab 有个向下箭头，Alt=Autoscroll to source
方法前 /**+enter		|	就可以产生 Java 注释
Ctrl+Alt+F7    		|  项目内检索。eclipse 中则 (Ctrl+Shift+G)
Ctrl+Shift+Alt+u	| 用图表查看类的继承结构。 显示，Ctrl+ 鼠标左键拖动，Alt 显示放大镜

## 编码
- 显示中文。
	`OutputStream os = response.getOutputStream();`
    `os.write("中文".getBytes("UTF-8"));`
- console 栏或者工具按钮快捷键需要显示中文，就在 setting 中设置 appearance 中 Theme 下面设置微软雅黑等中文字样


## Linux
- 在使用 linux 恢复之后，启动时报错，各种 NullPointerException。检查之后发现时~/.Idea\* 文件夹的权限问题，不知怎么的变成来 root/root，使用 `sudo chown -R $(whoami) .Idea*`，之后即可正常启动
- 在 Linux 中，F7 被 tty 占用，只得改成 Ctrl+Alt+7，
- 编辑栈即 Ctrl+Alt+Left/Right，若在 vm 中是被 vm 占用了 Ctrl+Alt，编辑 vm 的 hot key 为 Ctrl+Alt+Shift 即可
- Ctrl+Alt+F12 也是 linux 系统热键。好吧，改！
- 有时候 Ctrl+Alt+F12，也会出现 cli，那么就试试 Ctrl+Alt+F8。

## etc
- resin4 难道不支持 jsp 的 el 表达式？原来是支持的，用了 resin-pro 就好了。果然社区版本的 el 表达式被阉割掉了。。。
- 点击右下角的 power mode，自动提示就不会出来了，注解的颜色也不会显眼了。好吧，千万不要为此而重装 idea，差点就重装了
- 在执行 junit 时，若希望执行指定的某个 testcase，就将光标停在其方法内，Ctrl+Shift+F10, 若想全部运行，就不放在任一 testcase 方法内，放在 setUp，tearDownd 都行，就是不能放在具体的 testcase 里。
- 若是 64 位系统，把快捷方式里的 idea.exe 换成 idea64.exe 就 ok 了。修改 bin 目录下的 dea64.exe.vmoptions 内部 -Xmx1024m 就可以让 idea 获取更多内存，默认的是 800m。目前设置为 2048，内存嘛就是应该尽量用完
- 在调试程序时查看任何表达式值的一个容易的方法就是在编辑器中选择文本（可以按几次 Ctrl-W 组合键更有效地执行这个操作）然后按 Alt-F8。这个不是选择某个变量，而是某个表达式
- 当你想用代码片断捕捉异常时，在编辑器里选中这个片断，按 Ctrl-Alt-T
- 悲剧呀，不知道改哪里了，idea 不能定位到 struts 的 xml 了, 原来直接在显示框下面就有 web flow diagram
- 在 spring 的 aop，point-cut 中，idea 的查找有点失灵，在下面能找到上面的变量，而在上面的找不到下面的，这个得注意，若 Alt+F7 没有，要试试 Ctrl+F
- idea 对正则编辑的支持度还是不错的，但是，检查就不敢恭维了，譬如 Pattern pattern = Pattern.compile("\\[(.*)\\]"); 竟然认为是错的。明明就是对的嘛
- 而且不支持多行的正则 replacement，此时就需要 sublime 华丽登场了。
- 对于国际化，idea 是直接一个键把对应的 value 全显示，但没有自动转 ascii 码。这个估计 idea 认为设置的时候就已经对应了其编码，方便开发人员查看吧。
- 有时候在 jsp 页面上的 js 函数并不能使用 F4 跳转到其定义的地方，此时 CTRL+B 好使
- 关联源码。和添加 jar 包一样。在 module
- 单元测试，可以让整个包或者整个工程批量运行
- 修改文件模板配置：*C:\Users\Njoy\.IntelliJIdea12\config\fileTemplates\includes*
- 为了与 eclipse 系兼容，可以将 web 目录更改为 *WebRoot* 或者 *WebContent*, 除了更改目录名还需要在 Project Structure--Modules--Web--Web Resource Directories 进行更改
- 使用 idea 导出 javadoc，需要加 2 个参数
	`-encoding utf-8` 	#源文件的编码格式
	`-charset utf-8`		#用于跨平台查看文档的字符集。因为中国大陆电脑默认是 GBK
- 鼠标滚轮设置字号。Ctrl+Alt+S-->Editor--> 勾选 change font size (Zoom) with Ctrl + MouseWheel
- 在 idea 中 xml 的问题。如果 xmlns 有问题，通常情况 `xsi:schemaLocation` 是没有找到对应文件。因此，只要为 xsi 在工程内找到对应文件即可。一般是在 jar 包内。并且，可以动用 Ctrl+Shift+N 查找 xsi 指定的文件，将文件目录复制。在 set manually xxx，或者在 Ctrl+Shift+S 内搜索 dtd 进行配置。
- 在 project 视图，F4 跳到编辑视图内。而 Ctrl+B 仅仅显示在编辑视图，光标依然在 project 视图内。
- 新建 java_module 可以自动产生 artifacts，而 j2ee module 不会。然后采用 debug 模式启动服务器
- 设置 jvm 参数:Setting--Compiler--Java Compiler
- 设置自动导入:Setting--Editor--Auto Import
- 在工具栏中，大概是在 build 的下面，那个运行按钮之前，有个下拉菜单，选择 Edit Configuration。在 default 中选择需要的服务器，然后在对话框的左上角点击加号 (+)，选择 local 类型的服务器
- 配置 C:\Users\happylife\.IntelliJIdea12\config
- 在 code completion 中设置 Case sensitive completion =None
- 设置 jvm 参数： Menu 找到 --Run--Edit configuration-- 设置参数 --apply- 