---
layout: post
title: "linux shell脚本攻略学习笔记1"
date: 2015-08-19 17:17:22 +0800
comments: true
categories: [linux,shell,bash] 
---


###1.1 简介
1. 打开终端会出现一个提示符，如`username@hostname$` 或者 `root@hostname#`，`$`表示普通用户，`#`表示超级用户
2. shell脚本通常以`#!`起始，如`#!/bin/bash`，`#!`读作shebang，`#`英语读作sharp或hash，`!`读作bang
3. 执行脚本的两种方式：
* `sh script.sh` 将脚本做为sh的命令行参数执行，脚本中的shebang行就不起作用了
* `./script.sh` 首先要运行```chmod a+x script.sh```添加可执行权限，会识别shebang行，在内部已```/bin/bash script.sh```执行该脚本
4. 在bash中，命令通过分号`;`或者换行来分隔，比如
	`$cmd1;cmd2` 
  等同于
	`$cmd1` 
	`$cmd2`
	
----------

###1.2 终端打印
####1.2.1 实战演练
1. echo 不带引号、单引号、双引号区别
2. printf  类似于C语言
####1.2.2补充内容
1. echo转义换行符`echo -e "1\t2\t3"`
2. 打印彩色输出`echo -e "\033[44;37;5m ME \033[0m COOL"`

----------

###1.3 玩转变量和环境变量
1. `pgrep <PID>` `cat /proc/<PID>/environ`
2. 变量赋值 `var=value`,**等号两边没有空格**
3. `export`命令用来设置环境变量，`export PATH=$JAVA_HOME/bin:$PATH`
4. 获取字符串长度`echo ${#var}`
5. 识别当前shell版本`echo $SHELL`或`echo $0`
6. 检查用户是否为超级用户，root用户的$UID是0
	
	```
	#!/bin/bash
	if [ $UID -ne 0 ]; then
	echo "Non root user. Please run as root."
	else
	echo "root user"
	fi
	```  
7. 修改bash提示字符串(`[root@vultr workspace]#`) ，修改`$PS1`变量

----------

###1.4 通过shell进行数学运算
利用let，(())，[]执行基本算数操作，expr和bc执行高级操作
1. let 
	变量名之前不需要添加`$`
	> val1=4
	 val2=3
	let result=val1+val2
	echo result
	
	自加自减
	> let val++
	let val--

2. []操作符

	> result=$[ val1 + val2 ]
3. (())操作符

	> result=$((val1+val2))
4. expr

	> result=expr val1+val2
	或
	result=$(expr val1+val2) 

以上只能用于整数运算，不支持浮点数。
1.  bc是一个数学运算的高级工具

	> ➜  ~ echo "4 * 132.23123" | bc
528.92492
➜  ~  no=34
➜  ~  result1=`echo "$no * 1.534" | bc`
➜  ~  echo $result1
52.156

2. 设定小数精度

	> ➜  ~  echo "scale=2;10/3" | bc
3.33

3. 进制转换
	```
	➜  ~  no=100
	➜  ~  echo "obase=2;$no" | bc
	1100100
	➜  ~  no=1100100
	➜  ~  echo "obase=10;ibase=2;$no" | bc
	100
	```
	
4. 计算平方以及平方根
	```
	➜  ~  echo "sqrt(100)" | bc
	10
	➜  ~  echo "10^10" | bc
	10000000000
	```
	

----------

###1.5文件描述符和重定向

1. 预备知识
* stdin(标准输入)  0
* stdout(标准输出) 1
* stderr(标准错误) 2
	
2. 自定义文件描述符
	

