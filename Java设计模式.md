# Java���ģʽ(����)

ԭ���ߣ���̫�յĳ���Գ

ԭ��ַ��`https://zhuanlan.zhihu.com/p/93770973`


## һ�����ģʽ�ķ���

������˵���ģʽ��Ϊ�����ࣺ

��1��������ģʽ�������֣���������ģʽ�����󹤳�ģʽ������ģʽ��������ģʽ��ԭ��ģʽ��

��2���ṹ��ģʽ�������֣�������ģʽ��װ����ģʽ������ģʽ�����ģʽ���Ž�ģʽ�����ģʽ����Ԫģʽ��

��3����Ϊ��ģʽ����ʮһ�֣�����ģʽ��ģ�巽��ģʽ���۲���ģʽ��������ģʽ��������ģʽ������ģʽ������¼ģʽ��״̬ģʽ��������ģʽ���н���ģʽ��������ģʽ��

��ʵ�������ࣺ������ģʽ���̳߳�ģʽ����һ��ͼƬ����������һ�£�
![������ģʽ���̳߳�ģʽ](https://pic3.zhimg.com/80/v2-4428ce2fc4933559ace1e4a0fa2b329a_720w.jpg)

## �������ģʽ������ԭ��

### 1������ԭ��Open Close Principle

����ԭ�����˵**����չ���ţ����޸Ĺر�**���ڳ�����Ҫ������չ��ʱ�򣬲���ȥ�޸�ԭ�еĴ��룬ʵ��һ���Ȳ�ε�Ч��������һ�仰�������ǣ�Ϊ��ʹ�������չ�Ժã�����ά������������Ҫ�ﵽ������Ч����������Ҫʹ�ýӿںͳ����࣬����ľ�����������ǻ��ᵽ��㡣

### 2�����ϴ���ԭ��Liskov Substitution Principle��

���ϴ���ԭ��(Liskov Substitution Principle LSP)���������ƵĻ���ԭ��֮һ�� ���ϴ���ԭ����˵���κλ�����Գ��ֵĵط�������һ�����Գ��֡� LSP�Ǽ̳и��õĻ�ʯ��ֻ�е�����������滻�����࣬�����λ�Ĺ��ܲ��ܵ�Ӱ��ʱ������������������ã���������Ҳ�ܹ��ڻ���Ļ����������µ���Ϊ�����ϴ���ԭ���Ƕԡ���-�ա�ԭ��Ĳ��䡣ʵ�֡���-�ա�ԭ��Ĺؼ�������ǳ��󻯡�������������ļ̳й�ϵ���ǳ��󻯵ľ���ʵ�֣��������ϴ���ԭ���Ƕ�ʵ�ֳ��󻯵ľ��岽��Ĺ淶������ From Baidu �ٿ�

### 3��������תԭ��Dependence Inversion Principle��

����ǿ���ԭ��Ļ������������ݣ���Խӿڱ�̣������ڳ�����������ھ��塣

### 4���ӿڸ���ԭ��Interface Segregation Principle��

���ԭ�����˼�ǣ�ʹ�ö������Ľӿڣ���ʹ�õ����ӿ�Ҫ�á�����һ��������֮�����϶ȵ���˼����������ǿ�������ʵ���ģʽ����һ����������˼�룬�Ӵ�������ܹ�������Ϊ��������ά�����㡣���������ж�γ��֣�����������������ϡ�

### 5�������ط�������֪��ԭ�򣩣�Demeter Principle��

Ϊʲô������֪��ԭ�򣬾���˵��һ��ʵ��Ӧ�������ٵ�������ʵ��֮�䷢���໥���ã�ʹ��ϵͳ����ģ����Զ�����

### 6���ϳɸ���ԭ��Composite Reuse Principle��

ԭ���Ǿ���ʹ�úϳ�/�ۺϵķ�ʽ��������ʹ�ü̳С�

## ����Java��23�����ģʽ

����һ�鿪ʼ��������ϸ����Java��23�����ģʽ�ĸ��Ӧ�ó������������������ǵ��ص㼰���ģʽ��ԭ����з�����

### 1����������ģʽ��Factory Method��

��������ģʽ��Ϊ�������֣�

#### ��1������ͨ����ģʽ

���ǽ���һ�������࣬��ʵ����ͬһ�ӿڵ�һЩ�����ʵ���Ĵ��������ȿ��¹�ϵͼ��
![��ͨ����ģʽ��ϵͼ](https://pic4.zhimg.com/80/v2-204dc45a48954c6599e5738581cc8803_720w.jpg)

�������£������Ǿ�һ�������ʼ��Ͷ��ŵ����ӣ�

���ȣ��������ߵĹ�ͬ�ӿڣ�
```
public interface Sender { 
 public void Send(); 
}
```
��Σ�����ʵ���ࣺ
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
��󣬽������ࣺ
```
public class SendFactory { 
 
 public Sender produce(String type) { 
 if ("mail".equals(type)) { 
 return new MailSender(); 
 } else if ("sms".equals(type)) { 
 return new SmsSender(); 
 } else { 
 System.out.println("��������ȷ������!"); 
 return null; 
 } 
 } 
} 
```
�����������£�
```
public class FactoryTest { 
 
 public static void main(String[] args) { 
 SendFactory factory = new SendFactory(); 
 Sender sender = factory.produce("sms"); 
 sender.Send(); 
 } 
} 
```
�����this is sms sender!

#### (2)�����������ģʽ

��ģʽ�Ƕ���ͨ��������ģʽ�ĸĽ�������ͨ��������ģʽ�У�������ݵ��ַ�������������ȷ�������󣬶������������ģʽ���ṩ��������������ֱ𴴽����󡣹�ϵͼ��
![�����������ģʽ��ϵͼ](https://pic2.zhimg.com/80/v2-2ded2662725757dc8e47b9e9ebcf30e1_720w.jpg)
������Ĵ��������޸ģ��Ķ���SendFactory����У����£�
```
 public Sender produceMail(){ 

 return new MailSender(); 
 } 
 
 public Sender produceSms(){ 
 return new SmsSender(); 
 } 
 } 
```
���������£�
```
public class FactoryTest { 
 
 public static void main(String[] args) { 
 SendFactory factory = new SendFactory(); 
 Sender sender = factory.produceMail(); 
 sender.Send(); 
 } 
} 
```
�����this is mailsender!

#### ��3����̬��������ģʽ

������Ķ����������ģʽ��ķ�����Ϊ��̬�ģ�����Ҫ����ʵ����ֱ�ӵ��ü��ɡ�
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
�����this is mailsender!

������˵������ģʽ�ʺϣ����ǳ����˴����Ĳ�Ʒ��Ҫ���������Ҿ��й�ͬ�Ľӿ�ʱ������ͨ����������ģʽ���д����������ϵ�����ģʽ�У���һ�����������ַ������󣬲�����ȷ�������󣬵���������ڵڶ��֣�����Ҫʵ���������࣬���ԣ����������£����ǻ�ѡ�õ����֡�����̬��������ģʽ��

### 2�����󹤳�ģʽ��Abstract Factory��

��������ģʽ��һ��������ǣ���Ĵ������������࣬Ҳ����˵�������Ҫ��չ���򣬱���Թ���������޸ģ���Υ���˱հ�ԭ�����ԣ�����ƽǶȿ��ǣ���һ�������⣬��ν�������õ����󹤳�ģʽ��������������࣬����һ����Ҫ�����µĹ��ܣ�ֱ�������µĹ�����Ϳ����ˣ�����Ҫ�޸�֮ǰ�Ĵ��롣��Ϊ���󹤳���̫����⣬�����ȿ���ͼ��Ȼ��ͺʹ��룬�ͱȽ�������⡣
![���󹤳�ģʽ��ϵͼ](https://pic3.zhimg.com/80/v2-3ab976d1ca13c291b678f0a72c7693ae_720w.jpg)

�뿴���ӣ�
```
public interface Sender { 
 public void Send(); 
 } 
```
����ʵ���ࣺ
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
���������ࣺ
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
 ���ṩһ���ӿڣ�
 ```
 public interface Provider { 
 public Sender produce(); 
}
 ```
 �����ࣺ
 ```
 public class Test { 
 
 public static void main(String[] args) { 
 Provider provider = new SendMailFactory(); 
 Sender sender = provider.produce(); 
 sender.Send(); 
 } 
} 
```
��ʵ���ģʽ�ĺô����ǣ����������������һ�����ܣ�����ʱ��Ϣ����ֻ����һ��ʵ���࣬ʵ��Sender�ӿڣ�ͬʱ��һ�������࣬ʵ��Provider�ӿڣ���OK�ˣ�����ȥ�Ķ��ֳɵĴ��롣����������չ�ԽϺã�

### 3������ģʽ��Singleton��

����ģʽ�����ģʽ�����Ҳ��򵥵�һ�����ģʽ����֤���ڳ�����ֻ��һ��ʵ�����ڲ�����ȫ�ֵķ��ʵ���������Androidʵ��APP �������õ��� �˺���Ϣ������� ���ݿ����SQLiteOpenHelper���ȶ����õ�����ģʽ��������ģʽ�м����ô���

1. ĳЩ�ഴ���Ƚ�Ƶ��������һЩ���͵Ķ�������һ�ʺܴ��ϵͳ������

2. ʡȥ��new��������������ϵͳ�ڴ��ʹ��Ƶ�ʣ�����GCѹ����

3. ��Щ���罻�����ĺ��Ľ������棬�����Ž������̣����������Դ�������Ļ���ϵͳ��ȫ���ˡ�������һ�����ӳ����˶��˾��Աͬʱָ�ӣ��϶����ҳ�һ�ţ�������ֻ��ʹ�õ���ģʽ�����ܱ�֤���Ľ��׷��������������������̡��������һЩ���ӷ���һ�������ڿ���������Ӧ�õ���ģʽ��Ҫע��ĵ㡣

#### һ������
����ģʽ��Singleton������֤һ�������һ��ʵ�������ṩһ����������ȫ�ַ��ʵ�

#### �������ó���

1. Ӧ����ĳ��ʵ��������ҪƵ���ı����ʡ�

2. Ӧ����ÿ������ֻ�����һ��ʵ�������˺�ϵͳ�����ݿ�ϵͳ��

#### �������õ�ʹ�÷�ʽ

##### ��1������ʽ

�ŵ㣺�ӳټ��أ���Ҫ��ʱ���ȥ���أ�

ȱ�㣺 �̲߳���ȫ���ڶ��߳��к����׳��ֲ�ͬ����������������ݿ������е�Ƶ����д����ʱ��

����ʵ�����£�
```
public class Singleton { 
 
 /* ����˽�о�̬ʵ������ֹ�����ã��˴���ֵΪnull��Ŀ����ʵ���ӳټ��� */ 
 private static Singleton instance = null; 
 
 /* ˽�й��췽������ֹ��ʵ���� */ 
 private Singleton() { 
 } 
 
 /* 1:����ʽ����̬���̷���������ʵ�� */ 
 public static Singleton getInstance() { 
 if (instance == null) { 
 instance = new Singleton(); 
 } 
 return instance; 
 } 
}
```
##### ��2����ͬ����

�ŵ㣺������̲߳���ȫ�����⡣

ȱ�㣺Ч���е�ͣ�ÿ�ε���ʵ����Ҫ�ж�ͬ����

ע����AndroidԴ����ʹ�õĸõ��������У�InputMethodManager��AccessibilityManager�ȶ���ʹ�����ֵ���ģʽ��

����������£�
```
public static synchronized Singleton getInstance() { 
 if (instance == null) { 
 instance = new Singleton(); 
 } 
 return instance; 
 } 
 ```
 ��
 ```
  /*����synchronized������ÿ�ε���ʵ��ʱ�������**/ 
 public static Singleton getInstance() { 
 synchronized (Singleton.class) { 
 if (instance == null) { 
 instance = new Singleton(); 
 } 
 } 
 return instance; 
 } 
 ```
 ##### ��3��˫�ؼ�����

Ҫ�Ż���2������Ϊÿ�ε���ʵ����Ҫ�ж�ͬ���������⣬�ܶ��˶�ʹ�������һ��˫���ж�У��İ취��

�ŵ㣺�ڲ��������࣬��ȫ�Բ��ߵ�����»����ܺ��������е���ģʽ

ȱ�㣺��ͬƽ̨��������п��ܻ�������ذ�ȫ������

���䣺��androidͼ��Դ��ĿAndroid-Universal-Image-Loader ��https://github.com/nostra13/Android-Universal-Image-Loader����ʹ�õ������ַ�ʽ��
```
/*3.˫������:ֻ�ڵ�һ�γ�ʼ����ʱ�����ͬ����*/ 
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
 ���ַ���ò�ƺ������Ľ��������Ч�ʵ����⣬�������ڲ��������࣬��ȫ�Բ�̫�ߵ�������������У����ǣ����ַ���Ҳ�в��ҵĵط���������ǳ��������
 ```
 instance = new Singleton(); 
 ```
 ��JVM����Ĺ����л����ָ�����ŵ��Ż����̣���ͻᵼ�µ� instanceʵ���ϻ�û��ʼ�����Ϳ��ܱ��������ڴ�ռ䣬Ҳ����˵����� instance !=null ������û��ʼ��������������ͻᵼ�·��ص� instance �����������Բο���http://www.360doc.com/content/11/0810/12/1542811_139352888.shtml����

##### ��4���ڲ����ʵ��

�ŵ㣺�ӳټ��أ��̰߳�ȫ��java��class����ʱ����ģ���Ҳ�������ڴ����ġ��ڲ�����һ�ֺõ�ʵ�ַ�ʽ�������Ƽ�ʹ��һ�£�
```
public class SingletonInner { 
 private static class SingletonHolder { 
 private static SingletonInner instance = new SingletonInner(); 
 } 
 
 /** 
 * ˽�еĹ��캯�� 
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
##### ��5��ö�ٵķ���

�������Ϻܶ����Ƽ���һ������������ò��ʹ�õĲ��㷺����ҿ������ԣ�����������£�
```
public enum SingletonEnum { 
 /** 
 * 1.��Java1.5��ʼ֧��; 
 * 2.�޳��ṩ���л�����; 
 * 3.���Է�ֹ���ʵ��������ʹ����Ը��ӵ����л����߷��乥����ʱ��; 
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
ͨ������ģʽ��ѧϰ�������ǣ�

1. ����ģʽ��������򵥣����Ǿ���ʵ������������һ�����Ѷȡ�

2. synchronized�ؼ����������Ƕ������õ�ʱ��һ��Ҫ��ǡ���ĵط�ʹ�ã�ע����Ҫʹ�����Ķ���͹��̣������е�ʱ�򲢲������������������̶���Ҫ������

�����������ģʽ�����Ѿ������ˣ���β��������ͻȻ�뵽��һ�����⣬���ǲ�����ľ�̬������ʵ�ֵ���ģʽ��Ч����Ҳ�ǿ��еģ��˴�������ʲô��ͬ��

���ȣ���̬�಻��ʵ�ֽӿڡ�������ĽǶ�˵�ǿ��Եģ������������ƻ��˾�̬�ˡ���Ϊ�ӿ��в�������static���εķ��������Լ�ʹʵ����Ҳ�ǷǾ�̬�ģ�

��Σ��������Ա��ӳٳ�ʼ������̬��һ���ڵ�һ�μ����ǳ�ʼ����֮�����ӳټ��أ�����Ϊ��Щ��Ƚ��Ӵ������ӳټ����������������ܡ�

�ٴΣ���������Ա��̳У����ķ������Ա���д�����Ǿ�̬���ڲ���������static���޷�����д��

���һ�㣬������Ƚ����Ͼ���ʵ����ֻ��һ����ͨ��Java�ֻ࣬Ҫ���㵥���Ļ����������������������������ʵ��һЩ�������ܣ����Ǿ�̬�಻�С���������Щ�����У��������Կ������ߵ����𣬵��ǣ�����һ���潲�������������ʵ�ֵ��Ǹ�����ģʽ���ڲ�������һ����̬����ʵ�ֵģ����ԣ������кܴ�Ĺ�����ֻ�����ǿ�������Ĳ��治ͬ���ˡ�����˼��Ľ�ϣ�������ͳ������Ľ������������HashMap��������+������ʵ��һ������ʵ�����кܶ����鶼�����������ò�ͬ�ķ������������⣬�������ŵ�Ҳ��ȱ�㣬�������ķ����ǣ���ϸ����������ŵ㣬������õĽ�����⣡

����java��ҵ�������������Ѽǵ�˽����ʦ��ȡ��ϵ��ʽ���ཻ����

����л��ҵ��Ķ���ϣ����ҵ���ת���ղء�

������ 2019-11-26