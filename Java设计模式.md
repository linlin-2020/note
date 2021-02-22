# Java设计模式(搬运)

原作者：外太空的程序猿

原地址：`https://zhuanlan.zhihu.com/p/93770973`


## 一、设计模式的分类

总体来说设计模式分为三大类：

（1）创建型模式，共五种：工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式。

（2）结构型模式，共七种：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式。

（3）行为型模式，共十一种：策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。

其实还有两类：并发型模式和线程池模式。用一个图片来整体描述一下：
![并发型模式和线程池模式](https://pic3.zhimg.com/80/v2-4428ce2fc4933559ace1e4a0fa2b329a_720w.jpg)

## 二、设计模式的六大原则

### 1、开闭原则（Open Close Principle

开闭原则就是说**对扩展开放，对修改关闭**。在程序需要进行拓展的时候，不能去修改原有的代码，实现一个热插拔的效果。所以一句话概括就是：为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，我们需要使用接口和抽象类，后面的具体设计中我们会提到这点。

### 2、里氏代换原则（Liskov Substitution Principle）

里氏代换原则(Liskov Substitution Principle LSP)面向对象设计的基本原则之一。 里氏代换原则中说，任何基类可以出现的地方，子类一定可以出现。 LSP是继承复用的基石，只有当衍生类可以替换掉基类，软件单位的功能不受到影响时，基类才能真正被复用，而衍生类也能够在基类的基础上增加新的行为。里氏代换原则是对“开-闭”原则的补充。实现“开-闭”原则的关键步骤就是抽象化。而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对实现抽象化的具体步骤的规范。—— From Baidu 百科

### 3、依赖倒转原则（Dependence Inversion Principle）

这个是开闭原则的基础，具体内容：真对接口编程，依赖于抽象而不依赖于具体。

### 4、接口隔离原则（Interface Segregation Principle）

这个原则的意思是：使用多个隔离的接口，比使用单个接口要好。还是一个降低类之间的耦合度的意思，从这儿我们看出，其实设计模式就是一个软件的设计思想，从大型软件架构出发，为了升级和维护方便。所以上文中多次出现：降低依赖，降低耦合。

### 5、迪米特法则（最少知道原则）（Demeter Principle）

为什么叫最少知道原则，就是说：一个实体应当尽量少的与其他实体之间发生相互作用，使得系统功能模块相对独立。

### 6、合成复用原则（Composite Reuse Principle）

原则是尽量使用合成/聚合的方式，而不是使用继承。

## 三、Java的23中设计模式

从这一块开始，我们详细介绍Java中23种设计模式的概念，应用场景等情况，并结合他们的特点及设计模式的原则进行分析。

### 1、工厂方法模式（Factory Method）

工厂方法模式分为以下三种：

#### （1）、普通工厂模式

就是建立一个工厂类，对实现了同一接口的一些类进行实例的创建。首先看下关系图：
![普通工厂模式关系图](https://pic4.zhimg.com/80/v2-204dc45a48954c6599e5738581cc8803_720w.jpg)

举例如下：（我们举一个发送邮件和短信的例子）

首先，创建二者的共同接口：
```
public interface Sender { 
 public void Send(); 
}
```
其次，创建实现类：
```
public class MailSender implements Sender { 
 @Override 
 public void Send() { 
 System.out.println("this is mailsender!"); 
 } 
}
public class SmsSender implements Sender { 
 
 @Override 
 public void Send() { 
 System.out.println("this is sms sender!"); 
 } 
}
```
最后，建工厂类：
```
public class SendFactory { 
 
 public Sender produce(String type) { 
 if ("mail".equals(type)) { 
 return new MailSender(); 
 } else if ("sms".equals(type)) { 
 return new SmsSender(); 
 } else { 
 System.out.println("请输入正确的类型!"); 
 return null; 
 } 
 } 
} 
```
我们来测试下：
```
public class FactoryTest { 
 
 public static void main(String[] args) { 
 SendFactory factory = new SendFactory(); 
 Sender sender = factory.produce("sms"); 
 sender.Send(); 
 } 
} 
```
输出：this is sms sender!

#### (2)多个工厂方法模式

该模式是对普通工厂方法模式的改进，在普通工厂方法模式中，如果传递的字符串出错，则不能正确创建对象，而多个工厂方法模式是提供多个工厂方法，分别创建对象。关系图：
![多个工厂方法模式关系图](https://pic2.zhimg.com/80/v2-2ded2662725757dc8e47b9e9ebcf30e1_720w.jpg)
将上面的代码做下修改，改动下SendFactory类就行，如下：
```
 public Sender produceMail(){ 

 return new MailSender(); 
 } 
 
 public Sender produceSms(){ 
 return new SmsSender(); 
 } 
 } 
```
测试类如下：
```
public class FactoryTest { 
 
 public static void main(String[] args) { 
 SendFactory factory = new SendFactory(); 
 Sender sender = factory.produceMail(); 
 sender.Send(); 
 } 
} 
```
输出：this is mailsender!

#### （3）静态工厂方法模式

将上面的多个工厂方法模式里的方法置为静态的，不需要创建实例，直接调用即可。
```
public class SendFactory { 
 
 public static Sender produceMail(){ 
 return new MailSender(); 
 } 
 
 public static Sender produceSms(){ 
 return new SmsSender(); 
 } 
} 
public class FactoryTest { 
 
 public static void main(String[] args) { 
 Sender sender = SendFactory.produceMail(); 
 sender.Send(); 
 } 
} 
```
输出：this is mailsender!

总体来说，工厂模式适合：凡是出现了大量的产品需要创建，并且具有共同的接口时，可以通过工厂方法模式进行创建。在以上的三种模式中，第一种如果传入的字符串有误，不能正确创建对象，第三种相对于第二种，不需要实例化工厂类，所以，大多数情况下，我们会选用第三种——静态工厂方法模式。

### 2、抽象工厂模式（Abstract Factory）

工厂方法模式有一个问题就是，类的创建依赖工厂类，也就是说，如果想要拓展程序，必须对工厂类进行修改，这违背了闭包原则，所以，从设计角度考虑，有一定的问题，如何解决？就用到抽象工厂模式，创建多个工厂类，这样一旦需要增加新的功能，直接增加新的工厂类就可以了，不需要修改之前的代码。因为抽象工厂不太好理解，我们先看看图，然后就和代码，就比较容易理解。
![抽象工厂模式关系图](https://pic3.zhimg.com/80/v2-3ab976d1ca13c291b678f0a72c7693ae_720w.jpg)

请看例子：
```
public interface Sender { 
 public void Send(); 
 } 
```
两个实现类：
```
public class MailSender implements Sender { 
 @Override 
 public void Send() { 
 System.out.println("this is mailsender!"); 
 } 
}
public class SmsSender implements Sender { 
 
 @Override 
 public void Send() { 
 System.out.println("this is sms sender!"); 
 } 
}
```
两个工厂类：
```
public class SendMailFactory implements Provider { 
 
 @Override 
 public Sender produce(){ 
 return new MailSender(); 
 } 
 } 
 public class SendSmsFactory implements Provider{ 
 
 @Override 
 public Sender produce() { 
 return new SmsSender(); 
 } 
 } 
 ```
 在提供一个接口：
 ```
 public interface Provider { 
 public Sender produce(); 
}
 ```
 测试类：
 ```
 public class Test { 
 
 public static void main(String[] args) { 
 Provider provider = new SendMailFactory(); 
 Sender sender = provider.produce(); 
 sender.Send(); 
 } 
} 
```
其实这个模式的好处就是，如果你现在想增加一个功能：发及时信息，则只需做一个实现类，实现Sender接口，同时做一个工厂类，实现Provider接口，就OK了，无需去改动现成的代码。这样做，拓展性较好！

### 3、单例模式（Singleton）

单例模式是设计模式中最常见也最简单的一种设计模式，保证了在程序中只有一个实例存在并且能全局的访问到。比如在Android实际APP 开发中用到的 账号信息对象管理， 数据库对象（SQLiteOpenHelper）等都会用到单例模式。这样的模式有几个好处：

1. 某些类创建比较频繁，对于一些大型的对象，这是一笔很大的系统开销。

2. 省去了new操作符，降低了系统内存的使用频率，减轻GC压力。

3. 有些类如交易所的核心交易引擎，控制着交易流程，如果该类可以创建多个的话，系统完全乱了。（比如一个军队出现了多个司令员同时指挥，肯定会乱成一团），所以只有使用单例模式，才能保证核心交易服务器独立控制整个流程。下面针对一些例子分析一下我们在开发过程中应用单例模式需要注意的点。

#### 一、作用
单例模式（Singleton）：保证一个类仅有一个实例，并提供一个访问它的全局访问点

#### 二、适用场景

1. 应用中某个实例对象需要频繁的被访问。

2. 应用中每次启动只会存在一个实例。如账号系统，数据库系统。

#### 三、常用的使用方式

##### （1）懒汉式

优点：延迟加载（需要的时候才去加载）

缺点： 线程不安全，在多线程中很容易出现不同步的情况，如在数据库对象进行的频繁读写操作时。

具体实现如下：
```
public class Singleton { 
 
 /* 持有私有静态实例，防止被引用，此处赋值为null，目的是实现延迟加载 */ 
 private static Singleton instance = null; 
 
 /* 私有构造方法，防止被实例化 */ 
 private Singleton() { 
 } 
 
 /* 1:懒汉式，静态工程方法，创建实例 */ 
 public static Singleton getInstance() { 
 if (instance == null) { 
 instance = new Singleton(); 
 } 
 return instance; 
 } 
}
```
##### （2）加同步锁

优点：解决了线程不安全的问题。

缺点：效率有点低，每次调用实例都要判断同步锁

注：在Android源码中使用的该单例方法有：InputMethodManager，AccessibilityManager等都是使用这种单例模式。

具体代码如下：
```
public static synchronized Singleton getInstance() { 
 if (instance == null) { 
 instance = new Singleton(); 
 } 
 return instance; 
 } 
 ```
 或
 ```
  /*加上synchronized，但是每次调用实例时都会加载**/ 
 public static Singleton getInstance() { 
 synchronized (Singleton.class) { 
 if (instance == null) { 
 instance = new Singleton(); 
 } 
 } 
 return instance; 
 } 
 ```
 ##### （3）双重检验锁

要优化（2）中因为每次调用实例都要判断同步锁的问题，很多人都使用下面的一种双重判断校验的办法。

优点：在并发量不多，安全性不高的情况下或许能很完美运行单例模式

缺点：不同平台编译过程中可能会存在严重安全隐患。

补充：在android图像开源项目Android-Universal-Image-Loader （https://github.com/nostra13/Android-Universal-Image-Loader）中使用的是这种方式。
```
/*3.双重锁定:只在第一次初始化的时候加上同步锁*/ 
 public static Singleton getInstance() { 
 if (instance == null) { 
 synchronized (Singleton.class) { 
 if (instance == null) { 
 instance = new Singleton(); 
 } 
 } 
 } 
 return instance; 
 }
 ```
 这种方法貌似很完美的解决了上述效率的问题，它或许在并发量不多，安全性不太高的情况能完美运行，但是，这种方法也有不幸的地方。问题就是出现在这句
 ```
 instance = new Singleton(); 
 ```
 在JVM编译的过程中会出现指令重排的优化过程，这就会导致当 instance实际上还没初始化，就可能被分配了内存空间，也就是说会出现 instance !=null 但是又没初始化的情况，这样就会导致返回的 instance 不完整（可以参考：http://www.360doc.com/content/11/0810/12/1542811_139352888.shtml）。

##### （4）内部类的实现

优点：延迟加载，线程安全（java中class加载时互斥的），也减少了内存消耗。内部类是一种好的实现方式，可以推荐使用一下：
```
public class SingletonInner { 
 private static class SingletonHolder { 
 private static SingletonInner instance = new SingletonInner(); 
 } 
 
 /** 
 * 私有的构造函数 
 */ 
 private SingletonInner() { 
 
 } 
 
 public static SingletonInner getInstance() { 
 return SingletonHolder.instance; 
 } 
 
 protected void method() { 
 System.out.println("SingletonInner"); 
 } 
} 
```
##### （5）枚举的方法

这是网上很多人推荐的一种做法，但是貌似使用的不广泛，大家可以试试，具体代码如下：
```
public enum SingletonEnum { 
 /** 
 * 1.从Java1.5开始支持; 
 * 2.无偿提供序列化机制; 
 * 3.绝对防止多次实例化，即使在面对复杂的序列化或者反射攻击的时候; 
 */ 
 
 instance; 
 
 private String others; 
 
 SingletonEnum() { 
 
 } 
 
 public void method() { 
 System.out.println("SingletonEnum"); 
 } 
 
 public String getOthers() { 
 return others; 
 } 
 
 public void setOthers(String others) { 
 this.others = others; 
 } 
} 
```
通过单例模式的学习告诉我们：

1. 单例模式理解起来简单，但是具体实现起来还是有一定的难度。

2. synchronized关键字锁定的是对象，在用的时候，一定要在恰当的地方使用（注意需要使用锁的对象和过程，可能有的时候并不是整个对象及整个过程都需要锁）。

到这儿，单例模式基本已经讲完了，结尾处，笔者突然想到另一个问题，就是采用类的静态方法，实现单例模式的效果，也是可行的，此处二者有什么不同？

首先，静态类不能实现接口。（从类的角度说是可以的，但是那样就破坏了静态了。因为接口中不允许有static修饰的方法，所以即使实现了也是非静态的）

其次，单例可以被延迟初始化，静态类一般在第一次加载是初始化。之所以延迟加载，是因为有些类比较庞大，所以延迟加载有助于提升性能。

再次，单例类可以被继承，他的方法可以被覆写。但是静态类内部方法都是static，无法被覆写。

最后一点，单例类比较灵活，毕竟从实现上只是一个普通的Java类，只要满足单例的基本需求，你可以在里面随心所欲的实现一些其它功能，但是静态类不行。从上面这些概括中，基本可以看出二者的区别，但是，从另一方面讲，我们上面最后实现的那个单例模式，内部就是用一个静态类来实现的，所以，二者有很大的关联，只是我们考虑问题的层面不同罢了。两种思想的结合，才能造就出完美的解决方案，就像HashMap采用数组+链表来实现一样，其实生活中很多事情都是这样，单用不同的方法来处理问题，总是有优点也有缺点，最完美的方法是，结合各个方法的优点，才能最好的解决问题！

想在java行业有所建树的朋友记得私聊老师获取联系方式互相交流。

最后感谢大家的阅读，希望大家点赞转发收藏。

发布于 2019-11-26