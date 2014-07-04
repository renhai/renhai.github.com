---
layout: post
title: "开始使用Java Lambda表达式"
date: 2014-07-04 17:46:05 +0800
comments: true
categories: [java,lambda,java8] 
---

###简介
-----
Lambda表达式是Java SE 8中一个全新和重要的特性。Lambda表达式提供一种通过使用表达式来表述方法的方式。Lambda表达式和一个方法一样，拥有一个形参列表和一个参考这些形参的方法体（可以是一个表达式或者一个代码块）。

此外，Lambda表达式也增强了集合类库。Java SE 8为集合类库增加了两个批量数据操作相关的package：java.util.function和java.util.stream。stream类似于iterator，但是提供了更多的功能。总之，Lambda表达式和stream是自泛型和注解被添加到Java语言之后最大的一个改变。在这篇文章中，我们将要由浅入深的探索Lambda表达式和stream的强大。
<!--more-->

###前期准备
------
在开始使用lambda和stream之前，你需要确保安装Java 8。NetBeans和IntelliJ IDEA这些工具支持Java 8的特性，例如：lambda表达式，repeatable annotations， compact profiles等等。

以下提供了Java SE 8和NetBeans IDE 8的下载链接地址：

*   [Java Platform(JDK 8)](http://www.oracle.com/technetwork/java/javase/downloads/index.html)：从Oracle官网下载Java 8，也可以下载捆绑了NetBeans IDE 版本。

*   [NetBeans IDE 8](https://netbeans.org/downloads/index.html)：从NetBeans官网下载NetBeans IDE。

###Lambda表达式语法
------
lambda基本语法：
```
(parameters) ->expression
```
或者
```
 (parameters) ->{ statements; }
```
以下是Lambda表达式的几个例子：
```
() -> 5                           		// takes no value and returns 5
x -> 2 * x            					// takes a number and returns the result of doubling it
(x, y) -> x – y                     		// takes two numbers and returns their difference
(int x, int y) -> x + y      				// takes two integers and returns their sum
(String s) -> System.out.print(s) 		// takes a string and prints it to console without returning anything
```

###基本的Lambda表达式例子
-----
现在我们大概了解了什么是lambda表达式，让我们从一些简单的例子开始学习。在这个部分，我们将会看到lambda表达式如何影响我们编程的方式。假设我们有一个player的列表，程序猿经常使用的for循环在Java SE 8中可以转换成如下方式：
```
String[] atp = {"Rafael Nadal", "Novak Djokovic", "Stanislas Wawrinka", "David Ferrer", "Roger Federer", "Andy Murray", "Tomas Berdych", "Juan Martin Del Potro"};
List<String> players =  Arrays.asList(atp);
       
// Old looping
for (String player : players) {
     System.out.print(player + "; ");
}
       
// Using lambda expression and functional operations
players.forEach((player) -> System.out.print(player + "; "));
 
// Using double colon operator in Java 8
players.forEach(System.out::println);
```
正如你看到的，lambda表达式可以将我们的代码精简到一行。另外的一个例子是在用户图形界面应用中，匿名类可以被Lambda表达式替换。此外实现一个Runnable接口是也是如此：
```
// Using anonymous innerclass
btn.setOnAction(new EventHandler<ActionEvent>() {
              @Override
				public void handle(ActionEvent event) {
					System.out.println("Hello World!"); 
            }
        });
 
// Using lambda expression
btn.setOnAction(event -> System.out.println("Hello World!"));
 
Here is how we can write a Runnable using lambdas:

// Using anonymous innerclass 
new Thread(new Runnable() {
	@Override
	public void run() {
		System.out.println("Hello world !");
	}
}).start();
 
// Using lambda expression
new Thread(() -> System.out.println("Hello world !")).start();
 
// Using anonymous innerclass
Runnable race1 = new Runnable() {
	@Override
	public void run() {
		System.out.println("Hello world !");
	}
};
 
// Using lambda expression
Runnable race2 = () -> System.out.println("Hello world !");
 
// Run em!
race1.run();
race2.run();
```
Runnable使用lambda表达式之后可以将5行代码转换成1行，接下来，我们将在集合类排序中使用Lambda。

###使用Lambda对Collection排序
-----
在Java中，我们通常使用Comparator类来进行集合类排序。在接下来的例子中，我们将根据name，surname，name length和last name letter来对player list排序。首先我们使用之前的方式及匿名内部类的方式进行排序，然后我们使用lambda表达式的方式。

第一个例子，我们使用之前的方式，根据name排序，如下：
```
String[] players = {"Rafael Nadal", "Novak Djokovic", "Stanislas Wawrinka", "David Ferrer", "Roger Federer", "Andy Murray", "Tomas Berdych", "Juan Martin Del Potro", "Richard Gasquet", "John Isner"};
 
// Sort players by name using anonymous innerclass
Arrays.sort(players, new Comparator<String>() {
	@Override
	public int compare(String s1, String s2) {
		return (s1.compareTo(s2));
	}
});
```

使用lambda可以达到相同的效果：

```
// Sort players by name using lambda expression
Comparator<String> sortByName = (String s1, String s2) -> (s1.compareTo(s2));
 
Arrays.sort(players, sortByName);
// or this
Arrays.sort(players, (String s1, String s2) -> (s1.compareTo(s2)));
```

下面是根据其他字段的排序，和上面的例子一样，代码中使用了匿名内部类和lambda表达式两种方式：

```
// Sort players by surname using anonymous innerclass
Arrays.sort(players, new Comparator<String>() {
	@Override
	public int compare(String s1, String s2) {
		return (s1.substring(s1.indexOf(" ")).compareTo(s2.substring(s2.indexOf(" "))));
	}
});
 
// Sort players by surname using lambda expression
Comparator<String> sortBySurname = (String s1, String s2) -> (s1.substring(s1.indexOf(" ")).compareTo(s2.substring(s2.indexOf(" "))));
 
Arrays.sort(players, sortBySurname);
// or this
Arrays.sort(players, (String s1, String s2) -> (s1.substring(s1.indexOf(" ")).compareTo(s2.substring(s2.indexOf(" ")))));
 
// Sort players by name lenght using anonymous innerclass
Arrays.sort(players, new Comparator<String>() {
	@Override
	public int compare(String s1, String s2) {
		return (s1.length() - s2.length());
	}
});
 
// Sort players by name lenght using lambda expression
Comparator<String> sortByNameLenght = (String s1, String s2) -> (s1.length() - s2.length());
 
Arrays.sort(players, sortByNameLenght);
// or this
Arrays.sort(players, (String s1, String s2) -> (s1.length() - s2.length()));
 
// Sort players by last letter using anonymous innerclass
Arrays.sort(players, new Comparator<String>() {
	@Override
	public int compare(String s1, String s2) {
		return (s1.charAt(s1.length() - 1) - s2.charAt(s2.length() - 1));
	}
});
 
// Sort players by last letter using lambda expression
Comparator<String> sortByLastLetter = (String s1, String s2) -> (s1.charAt(s1.length() - 1) - s2.charAt(s2.length() - 1));
Arrays.sort(players, sortByLastLetter);
// or this
Arrays.sort(players, (String s1, String s2) -> (s1.charAt(s1.length() - 1) - s2.charAt(s2.length() - 1)));
```

就是这样，非常得简单明确。在下一节，我们将会探索更多的lambda表达式功能--同stream混合使用。

###混合使用Lambda和Stream
-----

Stream是对集合类的又一次包装，其中大量使用Lambda表达式。Stream支持多种操作，例如map，filter，limit，sorted，count，min，max，sum，collect等等。另外，Stream使用惰性计算方式，并不是读取全部数据，像getFirst()这种方法可以结束stream。在下一个例子中，我们将要探索lambda和stream能够做什么。我们创建了一个Person类，并向一个list中添加Person，在接下来的stream操作中将会使用这个list。Person类是一个简单的POJO:

```
public class Person {
 
private String firstName, lastName, job, gender;
private int salary, age;
 
public Person(String firstName, String lastName, String job, String gender, int age, int salary)       {
          this.firstName = firstName;
          this.lastName = lastName;
          this.gender = gender;
          this.age = age;
          this.job = job;
          this.salary = salary;
}
// Getter and Setter 
. . . . .   
}   
```

接下来，创建2个都包含Person对象的list:

```
            
        List<Person> javaProgrammers = new ArrayList<Person>() {
            {
                add(new Person("Elsdon", "Jaycob", "Java programmer", "male", 43, 2000));
                add(new Person("Tamsen", "Brittany", "Java programmer", "female", 23, 1500));
                add(new Person("Floyd", "Donny", "Java programmer", "male", 33, 1800));
                add(new Person("Sindy", "Jonie", "Java programmer", "female", 32, 1600));
                add(new Person("Vere", "Hervey", "Java programmer", "male", 22, 1200));
                add(new Person("Maude", "Jaimie", "Java programmer", "female", 27, 1900));
                add(new Person("Shawn", "Randall", "Java programmer", "male", 30, 2300));
                add(new Person("Jayden", "Corrina", "Java programmer", "female", 35, 1700));
                add(new Person("Palmer", "Dene", "Java programmer", "male", 33, 2000));
                add(new Person("Addison", "Pam", "Java programmer", "female", 34, 1300));
            }
        };
 
        List<Person> phpProgrammers = new ArrayList<Person>() {
            {
                add(new Person("Jarrod", "Pace", "PHP programmer", "male", 34, 1550));
                add(new Person("Clarette", "Cicely", "PHP programmer", "female", 23, 1200));
                add(new Person("Victor", "Channing", "PHP programmer", "male", 32, 1600));
                add(new Person("Tori", "Sheryl", "PHP programmer", "female", 21, 1000));
                add(new Person("Osborne", "Shad", "PHP programmer", "male", 32, 1100));
                add(new Person("Rosalind", "Layla", "PHP programmer", "female", 25, 1300));
                add(new Person("Fraser", "Hewie", "PHP programmer", "male", 36, 1100));
                add(new Person("Quinn", "Tamara", "PHP programmer", "female", 21, 1000));
                add(new Person("Alvin", "Lance", "PHP programmer", "male", 38, 1600));
                add(new Person("Evonne", "Shari", "PHP programmer", "female", 40, 1800));
            }
        };
```

现在我们使用forEach方法来遍历上面的list：

```
System.out.println("Show programmers names:");
javaProgrammers.forEach((p) -> System.out.printf("%s %s; ", p.getFirstName(), p.getLastName()));
phpProgrammers.forEach((p) -> System.out.printf("%s %s; ", p.getFirstName(), p.getLastName()));
```

下面我们使用同样的forEach方法将programmer的salary增加5%：

```
System.out.println("Increase salary by 5% to programmers:");
Consumer<Person> giveRaise = e -> e.setSalary(e.getSalary() / 100 * 5 + e.getSalary());
 
javaProgrammers.forEach(giveRaise);
phpProgrammers.forEach(giveRaise);
```

另外一个实用的方法是filter方法。为了展示如何使用filter，我们打印一下salary超过$1,400的PHP programmer：

```
System.out.println("Show PHP programmers that earn more than $1,400:")
phpProgrammers.stream()
          .filter((p) -> (p.getSalary() > 1400))
          .forEach((p) -> System.out.printf("%s %s; ", p.getFirstName(), p.getLastName()));
```

此外，我们可以预先定义filter，以便在接下来的操作中重用：

```
// Define some filters
Predicate<Person> ageFilter = (p) -> (p.getAge() > 25);
Predicate<Person> salaryFilter = (p) -> (p.getSalary() > 1400);
Predicate<Person> genderFilter = (p) -> ("female".equals(p.getGender()));
 
System.out.println("Show female PHP programmers that earn more than $1,400 and are older than 24 years:");
phpProgrammers.stream()
          .filter(ageFilter)
          .filter(salaryFilter)
          .filter(genderFilter)
          .forEach((p) -> System.out.printf("%s %s; ", p.getFirstName(), p.getLastName()));
 
// Reuse filters
System.out.println("Show female Java programmers older than 24 years:");
javaProgrammers.stream()
          .filter(ageFilter)
          .filter(genderFilter)
          .forEach((p) -> System.out.printf("%s %s; ", p.getFirstName(), p.getLastName()));
```

我们可以使用limit方法来限制结果数量：

```
System.out.println("Show first 3 Java programmers:");
javaProgrammers.stream()
          .limit(3)
          .forEach((p) -> System.out.printf("%s %s; ", p.getFirstName(), p.getLastName()));
 
System.out.println("Show first 3 female Java programmers:");
javaProgrammers.stream()
          .filter(genderFilter)
          .limit(3)
          .forEach((p) -> System.out.printf("%s %s; ", p.getFirstName(), p.getLastName()));
```

我们可以使用stream进行排序吗？当然可以。在下面的例子中，我们将根据name，salary对Java programmer进行排序，并将结果添加到一个list中，最后打印这个list:

```
System.out.println("Sort and show the first 5 Java programmers by name:");
List<Person> sortedJavaProgrammers = javaProgrammers
          .stream()
          .sorted((p, p2) -> (p.getFirstName().compareTo(p2.getFirstName())))
          .limit(5)
          .collect(toList());
 
sortedJavaProgrammers.forEach((p) -> System.out.printf("%s %s; %n", p.getFirstName(), p.getLastName()));
 
System.out.println("Sort and show Java programmers by salary:");
sortedJavaProgrammers = javaProgrammers
          .stream()
          .sorted((p, p2) -> (p.getSalary() - p2.getSalary()))
          .collect(toList());
 
sortedJavaProgrammers.forEach((p) -> System.out.printf("%s %s; %n", p.getFirstName(), p.getLastName()));
```

如果只对最低和最高salary感兴趣，我们可以先进行排序，然后取fist或者last，比这种方式更快的是使用min和max方法:

```
System.out.println("Get the lowest Java programmer salary:");
Person pers = javaProgrammers
          .stream()
          .min((p1, p2) -> (p1.getSalary() - p2.getSalary()))
          .get()
 
System.out.printf("Name: %s %s; Salary: $%,d.", pers.getFirstName(), pers.getLastName(), pers.getSalary())
 
System.out.println("Get the highest Java programmer salary:");
Person person = javaProgrammers
          .stream()
          .max((p, p2) -> (p.getSalary() - p2.getSalary()))
          .get()
 
System.out.printf("Name: %s %s; Salary: $%,d.", person.getFirstName(), person.getLastName(), person.getSalary())
```

通过上面的例子，我们已经看到了collect method是如何使用的。结合map method，我们使用collect method将结果转化成一个String，Set或者是TreeSet：

```
System.out.println("Get PHP programmers first name to String:");
String phpDevelopers = phpProgrammers
          .stream()
          .map(Person::getFirstName)
          .collect(joining(" ; "));    // this can be used as a token in further operations
 
System.out.println("Get Java programmers first name to Set:");
Set<String> javaDevFirstName = javaProgrammers
          .stream()
          .map(Person::getFirstName)
          .collect(toSet());
 
System.out.println("Get Java programmers last name to TreeSet:");
TreeSet<String> javaDevLastName = javaProgrammers
          .stream()
          .map(Person::getLastName)
          .collect(toCollection(TreeSet::new));
```

Stream也可以是并行的，例如：

```
System.out.println("Calculate total money spent for paying Java programmers:");
int totalSalary = javaProgrammers
          .parallelStream()
          .mapToInt(p -> p.getSalary())
          .sum();
```

为了获取stream中元素的各种统计数据，我们可以使用summaryStatistics method，然后调用getMax, getMin, getSum or getAverage这些方法：

```
//Get count, min, max, sum, and average for numbers
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
IntSummaryStatistics stats = numbers
          .stream()
          .mapToInt((x) -> x)
          .summaryStatistics();
 
System.out.println("Highest number in List : " + stats.getMax());
System.out.println("Lowest number in List : " + stats.getMin());
System.out.println("Sum of all numbers : " + stats.getSum());
System.out.println("Average of all numbers : " + stats.getAverage());
```

That's all, 希望大家喜欢。

###总结
-----

在这篇文章中，我们探讨了lambda表达式的多种使用方式，从简单的例子到复杂的例子，其中我们学习了如何在stream中使用lambda。此外，我们还学习了如何使用lambda表达式对collection进行排序。


[原文链接](http://www.jguru.com/article/core/start-using-java-lambda-expressions.html)

