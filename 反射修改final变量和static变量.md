# 反射可以修改final变量的值吗？

答案是可以的，只不过获取修改后的值有点困难。

**首先，存在JVM内联优化。编译器在编译时会有优化代码的操作，就是插入内联函数，即将指定的函数插入并取代每一处调用该函数的地方，从而节省每次调用函数带来的额外时间开销。**

比如：
```
public class A{
  private final String name="Bob";
  
  public String getName(){
    return name;
  }
  
  public static void main(String[] args){
    A a=new A();
    
    System.out.println(a.getName());
    //输出结果是 Bob
    
    Field field=A.class.getDeclaredField("name");
    field.setAccessible(true);//关闭安全检查
    field.set(a,"Apple");
    System.out.println(a.getName());
    //输出结果还是 Bob，其实变量值已经被改变了，只不过经过内联优化无法获取
    
    System.out.println(field.get(a));
    //输出结果是 Apple，因为此处直接调用被修改的后变量
  }
}
```
A的那个获取name的函数经过优化后变为
```
public String getName(){
  return "Bob";
}
```
原有的变量消失了，自然也无法获取到修改后的值。所以找到变量，就可获取到修改后的值。
除此之外如果直接使用a.name,获取到的值还是Bob，还是因为内联优化。所以只有`System.out.println(field.get(a));`才可以获取到修改后的值。

# 反射获取static静态变量

已知静态变量是随着类的加载而初始化的，所以值跟类的实例化对象无关。故，反射获取修改都跟对象无关。
```
public class A{
  public static String NAME="呵呵";
  
  public static void main(String[] args){
    Field field=A.class.getDeclaredField("NAME");
    field.setAccessible(true);
    Object o=field.get(null);
    System.out.println("修改前 "+o);
    //输出：修改前 呵呵
    field.set(null,"哈哈");
    System.out.println("修改后 "+A.NAME);
    //输出：修改后 哈哈
  }
}
```