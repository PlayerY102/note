# Java_Note

## 基本数据类型

* 整形
  * byte 一字节
  * short 两字节
  * char 两字节 无符号
  * int 四字节
  * long 八字节
* 浮点
  * float 四字节
  * double 八字节

## 运算符

* 混合运算
  * (byte)10+'a' 为int型变量

## 类

* 成员变量
  * 在整个类都有效，与前后关系无关
* 方法
* 成员变量的操作只能在方法中
* 声明变量时可以初始化，操作只能在方法中

## 对象

* 用类声明的变量被称为对象
* 和基本数据类型不同，在类声明对象后，还必须创建对象
* 创建一个对象时，也称给出了这个类的一个实例

* 构造方法
  * 默认构造
    * 方法体中没有语句
  * 自定义
    * 构造方法没有类型

    ``` java
    class Point{
        int x,y;
        Point(){
            x=1;
            y=1;
        }
        point(int a,int b){
            x=a;
            y=b;
        }
    }
    ```

* new
  * Point a=new point();

* static
  * 在类被加载时加载入内存

* pacakage
  * 指明源文件的类所在的包

* import
  * 写在pacakage和类定义中间

  * 使用不在一个包的类

* 权限
  * public:可以被调用

  * private:不能被调用

  * 友好类:可以被同一包的调用
  
* 对象数组

  ```JAVA
    Student [] stu=new Student[10];
    student[0]=new Student();
  ```

* super
  * 调用被子类隐藏的成员变量和方法
  * 父类构造函数不被继承，默认开头super()

* 上转型对象

  * 父类使用子类创建的实例

  * 使用被继承或者重写的方法

  * 使用被继承或隐藏的成员变量

  * 新增的无法使用

* final
  * final类不能被继承，不能有子类
  * final方法不能被隐藏
  * final成员变量时常量，不能被修改

* abstract
  * 只有abstract类能有abstract方法，不能new一个abstract类

  * abstract方法只允许声明不允许实现（没有方法体），不能用final，static修饰

  * abstract类的子类必须重写父类的abstract方法，

## 接口

* interface
  * interface 接口名字

  * 接口体中包含常量的声明和抽象方法

  * 常量的访问权限一定都是public，而且时static

  * 抽象方法的访问权限一定是public

* implements
  * public class 名字 implements 接口

* 接口可当作方法参数

## 内部类与异常类

* java 支持在一个类中定义另一个类，这样的类称为内部类
  * 内部类可以用外嵌类的成员和方法
  
  * 外嵌类可以通过内部类的对象调用内部类的变量方法

  * 内部类只供它的外嵌类使用

  * 可以用static修饰内部类

  * 非内部类不能是static

* 匿名类

  ```java
    new Bank(){
        匿名类类体
    };
  ```

  * 也可以是接口的匿名类

* 异常类
  * 为Exception类的子类

  * e.getMessage(),e.printStackTrace(),e.toString();

  * try-catch

    ```java
    try{

    }
    catch(Exception e){
    }
    ```

  * throw throws
    * throws 可能抛出的异常类 加在方法后面，表示这个方法可能抛出的异常
    * throw抛出异常

  * assert bool:string; bool为flase时中断，输出stirng

  * finally{}
    * 在try-catch语句后面，try catch return 也执行，System.exit(0)不执行
