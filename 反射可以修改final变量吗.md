# ��������޸�final������ֵ��

���ǿ��Եģ�ֻ������ȡ�޸ĺ��ֵ�е����ѡ�

**���ȣ�����JVM�����Ż����������ڱ���ʱ�����Ż�����Ĳ��������ǲ�����������������ָ���ĺ������벢ȡ��ÿһ�����øú����ĵط����Ӷ���ʡÿ�ε��ú��������Ķ���ʱ�俪����**

���磺
```
public class A{
  private final String name="Bob";
  
  public String getName(){
    return name;
  }
  
  public static void main(String[] args){
    A a=new A();
    
    System.out.println(a.getName());
    //�������� Bob
    
    Field field=A.class.getDeclaredField("name");
    field.setAccessible(true);//�رհ�ȫ���
    field.set(a,"Apple");
    System.out.println(a.getName());
    //���������� Bob����ʵ����ֵ�Ѿ����ı��ˣ�ֻ�������������Ż��޷���ȡ
    
    System.out.println(field.get(a));
    //�������� Apple����Ϊ�˴�ֱ�ӵ��ñ��޸ĵĺ����
  }
}
```
A���Ǹ���ȡname�ĺ��������Ż����Ϊ
```
public String getName(){
  return "Bob";
}
```
ԭ�еı�����ʧ�ˣ���ȻҲ�޷���ȡ���޸ĺ��ֵ�������ҵ��������Ϳɻ�ȡ���޸ĺ��ֵ��
����֮�����ֱ��ʹ��a.name,��ȡ����ֵ����Bob��������Ϊ�����Ż�������ֻ��`System.out.println(field.get(a));`�ſ��Ի�ȡ���޸ĺ��ֵ��