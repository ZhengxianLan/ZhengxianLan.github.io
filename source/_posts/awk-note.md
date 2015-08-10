---
title: awk-note
date: 2015-08-11 00:37:44
categories:
tags:
---

1. 在cli中用-v key=value给awk变量赋值，在awk程序内可以使用key来获取value值。
	不过，不能放在expression中/.../,只能在动作块{...}中获取
	而ARGV只能获取awk处理的数据文件即之后的值，而且，对于awk来说ARGV[1+]被认为是要处理的数据文件。所以，一般的变量应该使用k=v的方式植入
	例如：
	```awk
		awk -v test=capitek '{
			print test
			for(x in ARGV){
				print x,ARGV[x]
			}
		}' same lala
	```	
		output:
			capitek		通过-v key=value 以键值对的形式植入awk程序
			0	awk		固定值，ARGC也將這個計算在內，所以ARGC總是比我們在尾部傳入的參數多億個
			1	same	awk处理的数据文件
			2	lala 	数据文件后的参数
2. 在awk中只有数字和字符串
3. { printf("%-8s %6.2f\n",$1,$2*$3) }
	%-8s:	左对齐，宽8位的字符串
	%6.2f:	右对齐，宽6位，并保留2位小数
4. 多个模式
	使用 && || !
	$2 >= 4 || $3 >=20
	!( $2<4 || $3<20 )
5. 字符串连接操作: names=names $1 "" 通过在最后增加一个字符串就可以了.数字连接上一个字符串会强制转为字符串。而连接操作就像java里，只不过由+号改为空格符。
	由于字符串的连接没有显式的操作符，因此，当涉及到其他操作符的时候，最好用()包裹起来。比如
	{ print "abs($1) = " -$1  }
	上面看起来好像是连接操作，实际是进行了减法操作。若想进行字符串的连接操作，应该用下面的2种方式:
	{ print "abs($1) = " (-$1)  }
	{ print "abs($1) = ",-$1  }

6. print the last input line
		{ last = $0 }
	END	{ print last }
7. 控制流只能用于action内部
8. AWK这本书真的非常好。作者在第二章开头就说到，这一章节将会是对该语言的描述，势必会非常详细，建议读者迅速浏览，待到遇到具体问题的时候，再回来查阅。非常非常好的建议。反观，自己看犀牛6的时候，竟然，试图将那些非常详细，而很少用到的细节强记下来。真的非常不好。感谢awk，感谢伟大的作者！
9. BEGIN,END都必须跟action
10. 只有双方都是数字时才会进行数字比较，否则一方自动转为string，进行string比较。
11. 在字符类(character class,i.e.:[]),所有的字符表示它们的字面量，除了
	转义符:\
	头部的:^
	被字符包裹的连字符:-
12. 优先级，由高到低:
	*,+,?
	concatenation
	|
13. awk的转义序列，优先级由低到:
	\b	backspace
	\f	formfeed
	\n	newline (line feed)
	\r 	carriage return
	\t	tab
	\ddd	octal value ddd,where ddd is 1 to 3 digits between 0 and 7
	\c 	any other character c literally(e.g., \\ for backslash,\" for ")
15. 在awk中，优先级：||<&&<!
16. 范围模式(range pattern):
	pattern1,pattern2
	只要第一个模式匹配就会执行动作，在匹配到第二个模式之前会一直进行动作，终结于第二模式匹配的位置。若，一直无法匹配第二模式，那么会一直针对输入进行动作直至输入结束。
	demo:
		FNR==1,FNR==5 { print FILENAME ": " $0  }
		等价于
		FNR<=5 {print FILENAME ": " $0 }
17. Actions:
	expressions,with constants,variables,assignment,functions calls,etc
	print expression-list
	printf(format,expression-list)
	if(expression) statement
	if(expression) statement else statement
	while(expression) statement
	for (expression;expression,expression) statement
	for (variable in array) statement
	do statement while (expression)
	break
	continue
	next
	exit
	exit expression
	{ statement }
18. awk会为变量初始化默认值""或0
19. 获取倒数第二个field，$(NF-1)
20. 还可以动态添加字段，并且print
	BEGIN	{ FS=OFS="\t" }
		{ $5=1000*$3/$2;print }
21. 通常awk实现可能有每行不超过100个字段的限制
22. rand()返回一个随机数x，范围 0<=x<1
23. string的连接: 直接让他们以空格相连就可以了。
24. 正则表达式不仅可以使用双斜杠//的形式，也可以使用字符串。不过，注意在字符串内，转义需要多加反斜杠\.
		$0 ~ /(\+|-)[0-9]+/
	evaluate:
		$0 ~ "(\\+|-)[0-9]+"
25. awk提供了很多内建函数.需要注意的是，比如index('source','target'),下标从1开始。若搜索不到，返回0.
26. awk的gsub(r,s,t)，s中的&会被替换为r,thus:	PS这个在vi里同样适用哦，在sed里也能用哦
		gubs(/a/,"aba","banana")
	replaces banana by babanabanaba;so does
		gsubs(/a/,"&b&","banana")
27. 在使用双等号==进行比较时:
	若全为数字，那么按照数字比较
	若一方为字符串，那么按字符串比较
	若表示的数字超过计算机能解读的范围，那么按照字符串进行比较
28. 未初始化的变量，会默认被赋予初值 0 或者 ""。不存在的字段或已经明确置空的字段，仅有初值""，他们并非数字，但在需要时会被转为0.
29. 类型转换：
	number "" 数字转字符串
	string + 0 字符串转数字
	一个字符串的数字值为最前面的数字前缀，类似于javascript.parseInt('str')
30. awk中的if判断条件，若为0 或者 null 便为 false.
31. 内建的SUBSEP并非字面上的一个逗号‘,’,而是以\034的形式储存
32. awk默认是不支持多维数组的，不过可以使用一维数组模拟。
	对于多维数组的检测: if((i,j) in arr)...
	对于多维数组的loop: for(k in arr)...
33. 用户自定义函数，函数名称和左括号之间不允许又空格
34. 向用户自定义函数传变量实际是传了一个复制品，i.e.就是传值。不影响原来的参数本身.而传数组，则是传引用。大概可以认为数组为对象.
35. 在用户函数内，传入的参数为本地变量，他们仅有函数作用域。而其他非参数列表内的变量，拥有全局作用域，可被awk整个脚本区域内访问。
36. 输出
	print expression > file
	print expression >> file
	print expression | command
	close(filename),close(command)
		break coonnection between print and filename or command
	system(command)
		execute command;value is status return of command
37. printf(format,expr1,expr2...)
	each specification begins with a %,ends with a character that determines the conversion,and may include three modifiers:
		-	left-justfy expression in its field
		width	pad field to this width as needed;leading 0 pads with zeros
		.prec	maximum string width,or digits to right of decimal point
38.
	{ print $1,$2 > $3 } #把$1,$2都重定向到$3
	{print $1,($2) >$3 } #把$2重定向到$3
39. 在awk中使用shell命令通常需要被""包裹，而且，一般使用在管道，重定向，system()函数
	print message | "cat 1>&2"		# redirect cat to stderr
	system("echo '" message "' 1>&2")	# redirect cat to stderr	用这个比较保险哈
	print message > "/dev/tty"		# redirect cat to stderr
	"who"|getline
40. getline:
	while("who"|getline)
		n++
	---calculate the count of users

	"date"|getline d
	---pipes the output of the date command into the variable d,thus setting d to the current date.

	In all cases involving getline,you should be aware of the possibility of an error return if the file can't be accessed.Although it's appealing to write
	while(getline<"file")	# Dangerous

	that's an infinite loop if file doesn't exist,because with a nonexistent file getline returns -1,a nonzero value that represents true.The prefered way is:
	while (getline<"file">0) ... # safe

	Here the loop will be executed only when getline returns 1.
41. awk中的命令行参数，并不包括那些内建参数，而是在awk程序或者awk文件之后的参数，并且ARGV[0]为awk，所以ARGC总是多于实际参数.
42. 	在命令行的awk程序中 function 定义需要使用之前，貌似是脚本语言的通用原则
	awk程序以脚本形式存在，那么function可以放在最下面
43. awk脚本即使关键字错误也不会提示报错。所以，拼写一定要仔细。
44.
	"date"|getline date	# get today's date
	split(date,d," ")	# split date by " " into the array d
45. printf()输出到stdout
	sprintf()返回一个字符串
46. 处理多行数据。若数据以空行分隔，那么只需要BEGIN{ RS="" },为让输出更美观则：BEGIN{ RS="";ORS="\n\n" }
47. When RS is set to "",the field separator by default is any sequence of blanks and tabs,or newline.When FS is set to \n,only a newline acts as a field separator.
48. 或者使用range pattern.比如: /^doctor/,/^$/  只打印头衔是医生的数据。
	如果要除去头衔，只打印个人其他信息
		/^doctor/ {p=1;next}
		p==1
		/^$/ {p=0;next}
	其中next，表示让awk去读取下一行数据
49. 变量类型的自动转换。默认会首先尝试转为numberic.若需强制转string==>expr "".具体参考man awk