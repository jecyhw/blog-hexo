---
title: Scala case class学习
tags:
  - scala
  - case class
categories:
  - scala
abbrlink: 650286525
date: 2019-05-15 22:05:40
---
本文介绍了case class和以及case class在模式匹配中如何使用。

<!-- more -->

## case class介绍
样例类（case class）适合用于不可变的数据。它是一种特殊的类，能够被优化以用于模式匹配。

case class定义
``` scala
case class Book(name: String) {
  def printBookName(): Unit = {
    println(name)
  }
}

object BookTest {
  def main(args: Array[String]): Unit = {
    val book = Book("Java入门到放弃")
    book.printBookName()
  }
}
```

在实例化case class类时，不需要使用关键字New，case class类编译成class文件之后会自动生成apply方法，这个方法负责对象的创建。
通过[JD-GUI](https://github.com/java-decompiler/jd-gui)工具可以查看编译后的.class文件（有兴趣的可以自己看下）。
Scala自动为Book生成了apply静态方法，里面调用了Book$类的apply方法用来生成Book对象。

![](https://upload-images.jianshu.io/upload_images/14444552-4dac6df995accaf4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Book$类的截图

![](https://upload-images.jianshu.io/upload_images/14444552-4d763e6ba2fc13c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
case class类的参数都是可以直接访问的val（不能被修改），但是实际上编译成的class字节码会对book.name转成book.name()方法调用。如下图所示，name声明的时候是加了final关键字，并且生成了对应的name()方法。
![](https://upload-images.jianshu.io/upload_images/14444552-04d16c5ecf0d558f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
printBookName()方法中使用到的book.name实际上是调用的name()方法。
![](https://upload-images.jianshu.io/upload_images/14444552-438d3c7defa3316b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 模式匹配
模式匹配是检查某个值（value）是否匹配某一个模式的机制，一个成功的匹配同时会将匹配值解构为其组成部分。它是Java中的switch语句的升级版。

### 语法
一个模式匹配语句包括一个待匹配的值，match关键字，以及至少一个case语句。示例如下：

``` scala
def matchTest(x: Int): String = x match {
    case 1 => "one"
    case 2 => "two"
    case _ => "many"
  }
```

### case class的匹配

``` scala
abstract class Notification

case class Email(sender: String, title: String, body: String) extends Notification
case class SMS(caller: String, message: String) extends Notification
case class VoiceRecording(contactName: String, link: String) extends Notification

```

Notification 是一个虚基类，它有三个具体的子类Email, SMS和VoiceRecording，我们可以在这些Case Class类上使用模式匹配：

``` scala
def showNotification(notification: Notification): String = {
  notification match {
    case Email(email, title, _) =>
      s"You got an email from $email with title: $title"
    case SMS(number, message) =>
      s"You got an SMS from $number! Message: $message"
    case VoiceRecording(name, link) =>
      s"you received a Voice Recording from $name! Link: $link"
  }
}

val someSms = SMS("12345", "Are you there?")
val someVoiceRecording = VoiceRecording("Tom", "voicerecording.org/id/123")

println(showNotification(someSms))  // prints You got an SMS from 12345! Message: Are you there?
println(showNotification(someVoiceRecording))  // you received a Voice Recording from Tom! Click the link to hear it: voicerecording.org/id/123
```
---------

showNotification函数接受一个抽象类Notification对象作为输入参数，然后匹配其具体类型。（也就是判断它是一个Email，SMS，还是VoiceRecording）。在case Email(email, title, _ )中，对象的email和title属性在返回值中被使用，而body属性则被忽略，故使用_代替。
另外需要注意的一点是case Email(email, title, _)语句实际上是使用了Email提取器对象的unApply方法，这个方法也是Scala编译字节码的时候自动生成的，它会去提取匹配到的Email对象的sender和title属性填充到email和title属性上。
showNotification方法还可以等价写成下面这种形式，只匹配类型，而不使用Scala的提取器方式。

``` scala
def showNotification(notification: Notification): String = {
    notification match {
      case e: Email =>
        s"You got an email from ${e.sender} with title: ${e.title}"
      case s: SMS =>
        s"You got an SMS from ${s.caller}! Message: ${s.message}"
      case vr: VoiceRecording =>
        s"you received a Voice Recording from ${vr.contactName}! Link: ${vr.link}"
    }
  }
```

### 模式守卫

为了让匹配更加具体，可以使用模式守卫，也就是在模式后面加上if表达式。

``` scala
def showImportantNotification(notification: Notification, importantPeopleInfo: Seq[String]): String = {
  notification match {
    case Email(email, _, _) if importantPeopleInfo.contains(email) =>
      "You got an email from special someone!"
    case SMS(number, _) if importantPeopleInfo.contains(number) =>
      "You got an SMS from special someone!"
    case other =>
      showNotification(other) // nothing special, delegate to our original showNotification function
  }
}

val importantPeopleInfo = Seq("867-5309", "jenny@gmail.com")

val someSms = SMS("867-5309", "Are you there?")
val someVoiceRecording = VoiceRecording("Tom", "vr.org/id/123")
val importantEmail = Email("jenny@gmail.com", "Drinks tonight?", "I'm free after 5!")
val importantSms = SMS("867-5309", "I'm here! Where are you?")

println(showImportantNotification(someSms, importantPeopleInfo)) // You got an SMS from special someone!
println(showImportantNotification(someVoiceRecording, importantPeopleInfo)) //you received a Voice Recording from Tom! Link: vr.org/id/123
println(showImportantNotification(importantEmail, importantPeopleInfo)) // You got an email from special someone!
println(showImportantNotification(importantSms, importantPeopleInfo)) //You got an SMS from special someone!

```
---------

在case Email(email, _ , _ ) if importantPeopleInfo.contains(email)中，除了要求notification是Email类型外，还需要email在重要人物列表importantPeopleInfo中，才会匹配到该模式。


### 密封类

特质（trait）和类（class）可以用sealed标记为密封的，这意味着其所有子类都必须与之定义在相同文件中，从而保证所有子类型都是已知的。

``` scala
sealed abstract class Furniture
case class Couch() extends Furniture
case class Chair() extends Furniture

def findPlaceToSit(piece: Furniture): String = piece match {
  case a: Couch => "Lie on the couch"
  case b: Chair => "Sit on the chair"
}
```

## 参考资料

1. [案例类 https://docs.scala-lang.org/zh-cn/tour/case-classes.html](https://docs.scala-lang.org/zh-cn/tour/case-classes.html)
2. [模式匹配 https://docs.scala-lang.org/zh-cn/tour/pattern-matching.html](https://docs.scala-lang.org/zh-cn/tour/pattern-matching.html)
3. [https://blog.csdn.net/lovehuangjiaju/article/details/47176829](https://blog.csdn.net/lovehuangjiaju/article/details/47176829)

