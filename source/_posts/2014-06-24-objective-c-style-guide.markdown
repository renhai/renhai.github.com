---
layout: post
title: "Objective-C 代码规范指南"
date: 2014-06-24 17:33:06 +0800
comments: true
categories: [Objective-C,iOS]
styles: [data-table]
---

###背景介绍
***
Objective-C 是 C 语言的扩展，增加了动态类型和面对对象的特性。它被设计成具有易读易用的，支持复杂的面向对象设计的编程语言。它是 Mac OS X 以及 iPhone 的主要开发语言。

本指南主要基于 [Google Objective-C Style Guide](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml) 修改简化，作为 iOS Team 编码规范和 Code Review 指导。

<!--more-->

###留白和格式
***
####空格 vs. 制表符
只使用空格，一次缩进4个空格。

    通过 IDE 设置缩进空格：
	XCode：Preferences -> Text Editing -> Indentation
	Prefer indent using: Spaces
	Tab width: 4 spaces
	Indent width: 4 spaces
	
####方法声明和定义
+- 符号和返回类型之间须使用一个空格，参数列表中只有参数之间可以有空格。

方法应该像这样：

	- (void)doSomethingWithString:(NSString *)theString {
	...
	}

如果一行有非常多的参数，更好的方式是将每个参数单独拆成一行。如果使用多行，将每个参数前的冒号对齐。

	- (void)doSomethingWith:(GTMFoo *)theFoo
                   	   rect:(NSRect)theRect
                   interval:(float)theInterval {
  		...
	}
	
当第一个关键字比其它的短时，保证下一行至少有 4 个空格的缩进。这样可以使关键字垂直对齐，而不是使用冒号对齐：

	- (void)short:(GTMFoo *)theFoo
    	longKeyword:(NSRect)theRect
    	evenLongerKeyword:(float)theInterval {
  	...
	}
	
####方法调用

方法调用应尽量保持与方法声明的格式一致。

调用时所有参数应该在同一行：

	[myObject doFooWith:arg1 name:arg2 error:arg3];

或者每行一个参数，以冒号对齐：

	[myObject doFooWith:arg1
               	   name:arg2
                  error:arg3];

不要使用下面的缩进风格：
	
	[myObject doFooWith:arg1 name:arg2  // some lines with >1 arg
                  error:arg3];

	[myObject doFooWith:arg1
                   name:arg2 error:arg3];

	[myObject doFooWith:arg1
          	  name:arg2  // aligning keywords instead of 	colons
              error:arg3];

方法定义与方法声明一样，当关键字的长度不足以以冒号对齐时，下一行都要以四个空格进行缩进。

	[myObj short:arg1
    	longKeyword:arg2
    	evenLongerKeyword:arg3];
    	
    	
####@public 和 @private
 	
@public 和 @private 访问修饰符应该以一个空格缩进。

	@interface MyClass : NSObject {
 	 @public
  	 ...
 	 @private
  	 ...
	}
	@end

####异常

避免抛出异常。

#####block

取决于块的长度，下列都是合理的风格准则：

*	如果一行可以写完块，则没必要换行。
*	如果不得不换行，关括号应与块声明的第一个字符对齐。
*	如果块太长，比如超过 20 行，建议把它定义成一个局部变量，然后再使用该变量。
*	如果块不带参数，``{`` 之间无须空格。如果带有参数，``(`` 之间无须空格，但 ``) {`` 之间须有一个空格。




###命名
***	

命名方法基本遵守[Apple的官方标准](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)。

变量名使用驼峰命名法。

任何的类、类别、方法以及变量的名字中都使用全大写的首字母缩写，如URL，TIFF，EXIF等。

####类名

类名（以及类别、协议名）应首字母大写，并以驼峰格式分割单词。

应用层代码，使用项目约定的前缀。

当编写的代码希望在不同项目中复用是，应使用相同的前缀。

####类别名

类别名应该包含它所扩展的类的名字，在之后增加相关功能描述和“Additions”。
类别的文件名可以为“所扩展类名+类别功能描述”。
类名与包含类别名的括号之间，应该以一个空格分隔。

比如我们要基于 NSString 创建一个用于解析的类别，我们将把类别放在一个名为 GTMNSString+Parsing.h 的文件中。类别本身命名为 GTMStringParsingAdditions （是的，我们知道类别名和文件名不一样，但是这个文件中可能存在多个不同的与解析有关类别）。类别中的方法应该以 gtm_myCategoryMethodOnAString: 为前缀以避免命名冲突，因为 Objective-C 只有一个名字空间。如果代码不会分享出去，也不会运行在不同的地址空间中，方法名字就不那么重要了。

####文件名

文件名须反映出其实现了什么类 – 包括大小写。遵循你所参与项目的约定。

文件的扩展名应该如下：

扩展名 |描述
:-----:|:------------------------:
.h     |C/C++/Objective-C 的头文件
.m     |Ojbective-C 实现文件      
.mm    |Ojbective-C++ 的实现文件  
.cc    |纯 C++ 的实现文件         
.c     |纯 C 的实现文件 
<br />

类别的文件名应该包含被扩展的类名，如：“GTMNSString+Utils.h” 或 “GTMNSTextView+Autocomplete.h”。

####方法名

方法名应该以小写字母开头，并混合驼峰格式。每个具名参数也应该以小写字母开头。

方法名应尽量读起来就像句子，这表示你应该选择与方法名连在一起读起来通顺的参数名。

访问器方法应该与他们 要获取的 成员变量的名字一样，但不应该以get作为前缀。例如：

	- (id)getDelegate;  // AVOID
	- (id)delegate;     // GOOD

####变量名

变量名应该以小写字母开头，并使用驼峰格式。类的成员变量应该以下划线作为前缀。例如："myLocalVariable"、"_myInstanceVariable"。

####普通变量名

对于静态的属性（``int`` 或指针），不要使用匈牙利命名法。尽量为变量起一个描述性的名字。不要担心浪费列宽，因为让新的代码阅读者立即理解你的代码更重要。例如：

*	错误的命名：	

		int w;
		int nerr;
		int nCompConns;
		tix = [[NSMutableArray alloc] init];
		obj = [someObject object];
		p = [network port];

*	正确的命名：

		int numErrors;
		int numCompletedConnections;
		tickets = [[NSMutableArray alloc] init];
		userInfo = [someObject object];
		port = [network port];


####实例变量
实例变量应该混合大小写，并以下划线作为前缀，如 _usernameTextField。

####常量
常量名（如宏定义、枚举、静态局部变量等）应该以小写字母 k 开头，使用驼峰格式分隔单词，如：kInvalidHandle，kWritePerm。


###注释
***

尽管注释很重要，但最好的代码应该自成文档。与其给类型及变量起一个晦涩难懂的名字，再为它写注释，不如直接起一个有意义的名字。

当你写注释的时候，记得你是在给你的听众写，即下一个需要阅读你所写代码的贡献者。大方一点，下一个读代码的人可能就是你！

Objective-C 本来就是一个”啰嗦“的语言，除必要的注释外，在实现部分我们提倡尽量少注释儿依赖代码自解释，务必养成好的命名及代码习惯。

必要注释和迫不得已的注释需要遵循下列规范。


####文件注释
文件注释为必须添加的注释，因为当文件需要修改时，可能需要联系原作者。

可依赖 IDE 提供的自动添加功能，文件注释应包含如下项：

*	文件内容的简要描述
*	代码作者
*	版权信息声明（如：``Copyright 2008 Google Inc.``）
*	必要的话，加上许可证样板。为项目选择一个合适的授权样板（例如，``Apache 2.0, BSD, LGPL, GPL``）。

如果你对其他人的原始代码作出重大的修改，请把你自己的名字添加到作者里面。当另外一个代码贡献者对文件有问题时，他需要知道怎么联系你，这十分有用。


####声明部分的注释
每个接口、类别以及协议应辅以注释，以描述它的目的及与整个项目的关系。

####实现部分的注释
使用 | 来引用注释中的变量名及符号名而不是使用引号。

这会避免二义性，尤其是当符号是一个常用词汇，这使用语句读起来很糟糕。例如，对于符号 count ：

	// Sometimes we need |count| to be less than zero.
	
或者当引用已经包含引号的符号：

	// Remember to call |StringWithoutSpaces("foo bar baz")|
	
	
###Cocoa 和 Objective-C 特性
***

####成员变量应该是 @private
成员变量应该声明为 @private。

	@interface MyClass : NSObject {
 	 @private
  		id myInstanceVariable_;
	}
	// public accessors, setter takes ownership
	- (id)myInstanceVariable;
	- (void)setMyInstanceVariable:(id)theVar;
	@end


####明确指定构造函数
对于需要继承你的类的人来说，明确指定构造函数十分重要。这样他们就可以只重写一个构造函数（可能是几个）来保证他们的子类的构造函数会被调用。这也有助于将来别人调试你的类时，理解初始化代码的工作流程。

####重载指定构造函数
当你写子类的时候，如果需要 init… 方法，记得重载父类的指定构造函数。

如果你没有重载父类的指定构造函数，你的构造函数有时可能不会被调用，这会导致非常隐秘而且难以解决的 bug。

####重载 NSObject 的方法
如果重载了 NSObject 类的方法，强烈建议把它们放在 @implementation 内的起始处，这也是常见的操作方法。

通常适用（但不局限）于 init...，copyWithZone，以及 dealloc 方法。

dealloc 为第一个重载的方法， init... 方法应该放在一起，copyWithZone: 紧随其后。


####初始化
不要在 init 方法中，将成员变量初始化为 0 或者 nil, 对 objective-C 来说毫无必要。

####避免 +new
不要调用 NSObject 类方法 new``，也不要在子类中重载它。使用 ``alloc 和 init 方法创建并初始化对象。

现代的 Ojbective-C 代码通过调用 alloc 和 init 方法来创建并 retain 一个对象。由于类方法 new 很少使用，这使得有关内存分配的代码审查更困难。

####保持公共 API 简单
保持类简单；避免 “厨房水槽（kitchen-sink）” 式的 API。如果一个函数压根没必要公开，就不要这么做。用私有类别保证公共头文件整洁。

与 C++ 不同，Objective-C 没有方法来区分公共的方法和私有的方法 – 所有的方法都是公共的（译者注：这取决于 Objective-C 运行时的方法调用的消息机制）。因此，除非客户端的代码期望使用某个方法，不要把这个方法放进公共 API 中。尽可能的避免了你不希望被调用的方法却被调用到。这包括重载父类的方法。

对于内部实现所需要的方法，在实现的文件中定义一个类别，类别名位(private)，而不是把它们放进公有的头文件中。

	// GTMFoo.m
	#import "GTMFoo.h"

	@interface GTMFoo (private)
	- (NSString *)doSomethingWithDelegate;  // Declare private method
	@end

	@implementation GTMFoo(private)
	...
	- (NSString *)doSomethingWithDelegate {
  	// Implement this method
	}
	...
	@end


####\#import and #include
*	当包含一个使用 Objective-C、Objective-C++ 的头文件时，使用 #import 。
*	当包含一个使用标准 C、C++ 头文件时，使用 #include， 头文件应该使用 #define 保护。

####使用根框架
\#import 根框架而不是单独的零散文件

当你试图从框架（如 Cocoa 或者 Foundation）中包含若干零散的系统头文件时，实际上包含顶层根框架的话，编译器要做的工作更少。根框架通常已经经过预编译，加载更快。另外记得使用 #import 而不是 #include 来包含 Objective-C 的框架。

	#import <Foundation/Foundation.h>     // good

	#import <Foundation/NSArray.h>        // avoid
	#import <Foundation/NSString.h>
	...

####BOOL
将普通整形转换成 BOOL 时要小心。不要直接将 BOOL 值与 YES 进行比较。

Ojbective-C 中把 BOOL 定义成无符号字符型，这意味着 BOOL 类型的值远不止 YE(1)或 NO(0)。不要直接把整形转换成 BOOL。常见的错误包括将数组的大小、指针值及位运算的结果直接转换成 BOOL ，取决于整型结果的最后一个字节，很可能会产生一个 NO 值。当转换整形至 BOOL 时，使用三目操作符来返回 YES 或者 NO。（译者注：读者可以试一下任意的 256 的整数的转换结果，如 256、512 …）

你可以安全在 BOOL_Bool 以及 bool 之间转换（参见 C++ Std 4.7.4, 4.12 以及 C99 Std 6.3.1.2）。你不能安全在 BOOL 以及 Boolean 之间转换，因此请把 Boolean 当作一个普通整形，就像之前讨论的那样。但 Objective-C 的方法标识符中，只使用 BOOL。

对 BOOL 使用逻辑运算符（&&，|| 和 !）是合法的，返回值也可以安全地转换成BOOL，不需要使用三目操作符。

错误的用法：

	- (BOOL)isBold {
  		return [self fontTraits] & NSFontBoldTrait;
	}
	- (BOOL)isValid {
  		return [self stringValue];
	}

正确的用法：

	(BOOL)isBold {
  		return ([self fontTraits] & NSFontBoldTrait) ? YES : NO;
	}
	- (BOOL)isValid {
  		return [self stringValue] != nil;
	}
	- (BOOL)isEnabled {
  		return [self isValid] && [self isBold];
	}
同样，不要直接比较 YES/NO 和 BOOL 变量。不仅仅因为影响可读性，更重要的是结果可能与你想的不同。

错误的用法:

	BOOL great = [foo isGreat];
	if (great == YES)
  	// ...be great!

正确的用法：

	BOOL great = [foo isGreat];
	if (great)
  	// ...be great!

####属性（Property）
属性（Property）通常允许使用，但需要清楚的了解：属性（Property）是 Objective-C 2.0 的特性，会限制你的代码只能跑在 iPhone 和 Mac OS X 10.5 (Leopard) 及更高版本上。点引用只允许访问声明过的 @property。

####命名
属性所关联的实例变量的命名必须遵守以下划线作为前缀的规则。属性的名字应该与成员变量去掉下划线后缀的名字一模一样。

使用 @synthesize 指示符来正确地重命名属性。

	@interface MyClass : NSObject {
 	 @private
  		NSString *name_;
	}
	@property(copy, nonatomic) NSString *name;
	@end

	@implementation MyClass
	@synthesize name = name_;
	@end

####位置
属性的声明必须紧靠着类接口中的实例变量语句块。属性的定义必须在 @implementation 的类定义的最上方。他们的缩进与包含他们的 @interface 以及 @implementation 语句一样。  	  		
