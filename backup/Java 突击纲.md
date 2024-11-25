[![java.png](https://i.postimg.cc/1XMsF5xz/java.png)](https://postimg.cc/kRV03Cgr)

> [!CAUTION]
> 非突击期末不建议看

> [!TIP]
> 全套突击代码上传至 [github](https://github.com/Koi-Blue/repo/tree/main/java_projects) ，可以在线访问

> [!NOTE]
> 可以通过 `git clone https://github.com/Koi-Blue/repo.git` 下载

> [!NOTE]
> 也可以通过 [百度网盘](https://pan.baidu.com/s/1YRWmjon139B0BA4S5HecxA?pwd=fish ) 下载
> 提取码：`fish`

# 第一章

## Java的特点
- **跨平台性**：基于Java虚拟机（JVM），一次编写，到处运行。
- **面向对象**：支持封装、继承、多态。
- **内存管理**：自动垃圾回收机制。
- **安全性**：通过类加载器和字节码验证，防止恶意代码。
- **多线程**：内置线程支持。
- **丰富的API库**：提供功能强大的类库。

## Java虚拟机
- **JVM职责**：
  - 加载、验证、解释和执行Java字节码。
  - 垃圾回收和内存管理。
  - 支持语言特性（如动态加载）。
- **JVM结构**：
  - 类加载子系统。
  - 内存区域：堆、方法区、栈、本地方法栈、程序计数器。
  - 执行引擎。

## 1.4 Java数组
- **创建、声明、初始化、引用**：
   ```java
   int[] arr = {1, 2, 3}; // 静态初始化
   String[] names = {"Alice", "Bob", "Charlie"};
 
   int[] arr2 = new int[5]; // 动态初始化
   int[][] arr3 = new int[5][5]

   // 数据类型[][] 数组名 = new 数据类型[行数][];
   int[][] jaggedArray = new int[2][];
   
   // 数组名[行号] = new 数据类型[列数];
   jaggedArray[0] = new int[3];
   jaggedArray[1] = new int[2];
   ```
   | 初始化方式          | 适用场景                             | 示例代码                                                 |
   |-------------------|----------------------------------|-----------------------------------------------------|
   | 静态初始化          | 已知具体值                          | `int[] arr = {1, 2, 3};`                           |
   | 动态初始化          | 未知具体值，但已知长度                | `int[] arr = new int[5];`                         |
   | 部分静态初始化      | 动态与静态结合                       | `int[] arr = new int[]{1, 2, 3};`                 |
   | 匿名数组初始化       | 临时使用数组或传参                   | `printArray(new int[]{1, 2, 3});`                 |
   | 静态二维数组初始化   | 初始化完整矩阵                      | `int[][] matrix = {{1, 2}, {3, 4}};`             |
   | 动态二维数组初始化   | 创建默认矩阵，元素为默认值            | `int[][] matrix = new int[2][3];`                |
   | 不规则二维数组初始化 | 初始化不规则的矩阵（行长度不同）        | `int[][] jaggedArray = new int[2][];`            |


- 遍历过程中发生的事情：
   * 按索引访问元素。
   * 循环中对元素进行操作可能会改变数组的值。
- 关键字：
   * **` break `** ：跳出当前循环。
   * **` continue `** ：跳过当前迭代，进入下一次循环。
- 多维数组：
   * 每一行的元素个数可以不同。

## 1.5.6 和 1.5.7 遍历数组时的关键字
- ` break ` 和 ` continue ` 的作用：

   ``` java
   for (int i = 0; i < arr.length; i++) {
         if (arr[i] == 5) break; // 提前退出循环
         if (arr[i] % 2 == 0) continue; // 跳过偶数
         System.out.println(arr[i]);
   }
   ```

---

# 第二章

## 2.2  类或对象
- 类的定义：
   * 属性和方法。
   * 修饰符（ ` public ` ， ` private ` ， ` protected ` ）
- 对象：
   * 通过类实例化
   * 访问成员变量和方法

### 2.2.6 类的访问控制
-  **` public `** ：可被所有类访问。
-  **` protected `** ：仅在桐宝子类中可访问。
-  **` private `** ：仅在本类中可访问。
- **默认**（包访问权限）：仅在同包中可访问。

## 2.3 对象初始化和回收
- **构造方法**：
   * 无参和有参构造方法。
   * **特点**：与类名相同，无返回值。
   ``` java
   public class MyClass {
          int value;
          public MyClass() {
              value = 0;
          }
          public MyClass(int value) {
              this.value = value;
          }
   }
   ```
   * **Java 内存回收机制**：
      - 自动垃圾回收（ ` GC ` ）。
      -  ` System.gc ` （）仅为建议，无法强制。
   * **变量赋值**：
      - 若赋值：使用赋值值。
      - 若未赋值：
         * 数值类型：默认为 ` 0 ` .
         * 布尔类型：默认 ` false ` 。
         * 引用类型：默认为 ` null ` 。

---

# 第三章

## 3.1 类的继承

### 子类和父类的关系
- 父类的属性和方法可以被子类继承。
- 子类可以重写父类的方法，也可以扩展新的属性和方法。

   ```java
   // 父类
   public class Parent {
       public String name = "Parent";
   
       public void method() {
           System.out.println("Parent method");
       }
   
       public void showName() {
           System.out.println("Name: " + name);
       }
   }
   
   // 子类
   public class Child extends Parent {
       public String name = "Child";
   
       @Override
       public void method() {
           System.out.println("Child method");
       }
   
       public void childOnlyMethod() {
           System.out.println("This is a child-only method");
       }
   }
   
   // 测试类
   public class Test {
       public static void main(String[] args) {
           // 父类引用指向子类对象（多态）
           Parent obj = new Child();
   
           obj.method();          // 输出: Child method
           obj.showName();        // 输出: Name: Parent
           // obj.childOnlyMethod(); // 编译报错：Parent类中无此方法
   
           // 如果想访问子类特有方法，需强制类型转换
           Child child = (Child) obj;
           child.childOnlyMethod(); // 输出: This is a child-only method
       }
   }
   ```
### 特性分析
- **重写**：子类可以提供自己的实现，父类方法被覆盖。
- **多态**：通过父类引用调用的方法，实际运行时决定于对象的实际类型。
- **字段隐藏**：父类和子类的同名属性不共享，多态不适用于字段。

## 3.2  ` Object ` 类
### 相等与同一
-  **` == `**  比较的是两个引用的内存地址。
-  **` equals `**  默认比较内存地址，但可以被重写用来比较对象内容。
   ``` java
   // 示例代码
   public class EqualsDemo {
       private String value;
   
       public EqualsDemo(String value) {
           this.value = value;
       }

       @Override
       public boolean equals(Object obj) {
           if (this == obj) return true; // 比较内存地址
           if (obj == null || getClass() != obj.getClass()) return false;
   
           EqualsDemo that = (EqualsDemo) obj;
           return value != null ? value.equals(that.value) : that.value == null;
       }

       public static void main(String[] args) {
           EqualsDemo obj1 = new EqualsDemo("Test");
           EqualsDemo obj2 = new EqualsDemo("Test");
           EqualsDemo obj3 = obj1;
   
           System.out.println(obj1 == obj2);        // false
           System.out.println(obj1.equals(obj2));  // true
           System.out.println(obj1 == obj3);       // true
       }
   }
   ```

## 3.3  ` final ` 类与方法
###  ` final ` 类
-  ` final ` 修饰的类无法被继承。
   ``` java
   public final class FinalClass {
       public void show() {
           System.out.println("This is a final class");
       }
   }
   // 无法继承
   // public class ChildClass extends FinalClass { } // 报错
   ```
###  ` final ` 方法
- 子类无法重写 ` final ` 方法。
   ``` java
      public class Parent {
       public final void finalMethod() {
           System.out.println("This is a final method");
       }
   }
   
   public class Child extends Parent {
       // @Override
       // public void finalMethod() { } // 报错
   }
   ```

## 3.4 抽象类
### 特点
- 抽象类可以包含抽象方法（无实现）和具体方法（有实现）。
- 无法直接实例化抽象类。
   ``` java
   // 抽象类
   public abstract class Animal {
       public abstract void makeSound(); // 抽象方法
   
       public void eat() {
           System.out.println("This animal is eating");
       }
   }
   
   // 具体类
   public class Dog extends Animal {
       @Override
       public void makeSound() {
           System.out.println("Woof Woof!");
       }
   }
   
   // 测试类
   public class Test {
       public static void main(String[] args) {
           Animal animal = new Dog();
           animal.makeSound(); // 输出: Woof Woof!
           animal.eat();       // 输出: This animal is eating
       }
   }
   ```

------------------------------------

# 第四章

## 4.1接口
### 定义接口：
- 接口中所有的变量默认为 **` public static final `** ，方法默认为 **` public abstract `** 。
   ``` java
   public interface MyInterface {
       int CONSTANT = 100; // 等价于 public static final int CONSTANT = 100;
       void method(); // 等价于 public abstract void method();
   }
   ```

### 实现接口：
- 类通过 **` implements `** 关键字实现接口，并必须实现接口的所有方法。
   ``` java
   public class MyClass implements MyInterface {
       @Override
       public void method() {
           System.out.println("Implementing interface");
       }
   }
   ```

### 多接口实现
- 一个类可以实现多个接口。
   ``` java
   public interface InterfaceA {
       void methodA();
   }
   
   public interface InterfaceB {
       void methodB();
   }
   
   public class MyMultiInterfaceClass implements InterfaceA, InterfaceB {
       @Override
       public void methodA() {
           System.out.println("Implementing methodA");
       }
   
       @Override
       public void methodB() {
           System.out.println("Implementing methodB");
       }
   }
   ```


- 测试类：
   ``` java
   public class Test {
       public static void main(String[] args) {
           MyInterface obj = new MyClass();
           obj.method(); // 输出: Implementing interface
       }
   }
   ```

## 4.5 构造方法与多态
- 构造方法的调用顺序：
   * 父类构造方法 -> 子类构造方法。
   ``` java
   public class Parent {
       public Parent() {
           System.out.println("Parent constructor");
       }
   }
   
   public class Child extends Parent {
       public Child() {
           System.out.println("Child constructor");
       }
   }
   
   public class Test {
       public static void main(String[] args) {
           Child child = new Child();
           // 输出：
           // Parent constructor
           // Child constructor
       }
   }
   ```

- 多态：
   * 父类引用指向子类对象，方法调用表现出多态性。
   ``` java
   public class Test {
       public static void main(String[] args) {
           Parent obj = new Child();
           obj.method(); // 输出: Child method
       }
   }
   ```

---

# 第五章


## 5.1 异常处理机制简介概述
### 异常的概念
- **异常**是程序运行时出现的错误情况。Java通过异常处理机制提供了一种优雅的错误处理方式。
- **异常分类**：
  1. **受检异常（Checked Exception）**：编译器强制要求处理的异常，例如`IOException`。
  2. **非受检异常（Unchecked Exception）**：运行时异常或错误，例如`NullPointerException`。
  3. **错误（Error）**：严重问题，通常由JVM抛出，程序无法处理，例如`OutOfMemoryError`。

### 异常处理的优势
- 提升程序的健壮性。
- 将错误与正常逻辑分离，代码可读性更高。
- 提供了一种结构化的错误恢复机制。

## 5.2 异常处理

###  ` try-catch-finally ` 结构
- **语法**：
  ```java
  try {
      // 可能抛出异常的代码
  } catch (ExceptionType1 e) {
      // 捕获ExceptionType1的异常
  } catch (ExceptionType2 e) {
      // 捕获ExceptionType2的异常
  } finally {
      // 总是会执行的代码块（如释放资源）
  }
   ```
### 示例代码
   ``` java
   public class ExceptionDemo {
       public static void main(String[] args) {
           try {
               int a = 10 / 0; // 可能抛出ArithmeticException
           } catch (ArithmeticException e) {
               System.out.println("Caught ArithmeticException: " + e.getMessage());
           } catch (Exception e) {
               System.out.println("Caught general exception: " + e.getMessage());
           } finally {
               System.out.println("This block always executes.");
           }
       }
   }
   ```
### 捕获多个异常的顺序
- 捕获顺序从具体异常到通用异常，因为Java采用从上到下匹配的方式。
- 示例：
   ``` java
   try {
       int[] arr = new int[2];
       System.out.println(arr[5]); // ArrayIndexOutOfBoundsException
   } catch (ArrayIndexOutOfBoundsException e) {
       System.out.println("Array index out of bounds!");
   } catch (Exception e) {
       System.out.println("General exception!");
   }
   ```
- 若将 ` Exception ` 写在最前面，具体异常永远不会被捕获，编译器会报错。

###  ` finally ` 的作用
- **确保资源释放**：例如关闭文件流、数据库连接。
- **总是执行**：即使在 ` try ` 或 ` catch ` 中有 ` return ` 或异常抛出。
**示例代码**
   ``` java
   public class FinallyDemo {
       public static void main(String[] args) {
           try {
               System.out.println("Inside try block");
               return;
           } catch (Exception e) {
               System.out.println("Inside catch block");
           } finally {
               System.out.println("Inside finally block");
           }
       }
   }
   // 输出:
   // Inside try block
   // Inside finally block
   ```

## 5.3 输入输出流
从键盘输入 ` n ` 个数
使用 ` Scanner ` 
   ``` java
      import java.util.Scanner;
   
   public class InputDemo {
       public static void main(String[] args) {
           Scanner scanner = new Scanner(System.in);
           System.out.println("Enter 5 numbers:");
           int[] numbers = new int[5];
   
           for (int i = 0; i < numbers.length; i++) {
               numbers[i] = scanner.nextInt();
           }
   
           System.out.println("You entered:");
           for (int number : numbers) {
               System.out.print(number + " ");
           }
       }
   }
   ```

使用 ` BufferedReader ` 
   ``` java
   import java.io.BufferedReader;
   import java.io.IOException;
   import java.io.InputStreamReader;
   
   public class BufferedReaderDemo {
       public static void main(String[] args) throws IOException {
           BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
           System.out.println("Enter 5 numbers separated by space:");
           String[] input = reader.readLine().split(" ");
           int[] numbers = new int[input.length];
   
           for (int i = 0; i < input.length; i++) {
               numbers[i] = Integer.parseInt(input[i]);
           }
   
           System.out.println("You entered:");
           for (int number : numbers) {
               System.out.print(number + " ");
           }
       }
   }
   ```

### 文件读写：
**从文件读写**
   ``` java
   import java.io.*;

      public class FileReadWriteDemo {
       public static void main(String[] args) {
           String inputFile = "input.txt";
           String outputFile = "output.txt";
   
           try (BufferedReader br = new BufferedReader(new FileReader(inputFile));
                BufferedWriter bw = new BufferedWriter(new FileWriter(outputFile))) {
   
               String line;
               while ((line = br.readLine()) != null) {
                   bw.write(line.toUpperCase());
                   bw.newLine();
               }
           } catch (IOException e) {
               System.out.println("Error: " + e.getMessage());
           }
       }
   }
   ```

---

# 第六章
## 6.3 集合框架中的常用类
-  **` HashSet `** ：无序，不允许重复元素。
-  **` Vector `**  和  ` ArrayList ` ：
   *  ` Vector ` ：线程安全。
   *  ` ArrayList ` ：效率更高，非线程安全。
-  **` LinkedList `** ：双向链表结构。
-  **` HashTable `**  和  **` HashMap `** ：
   *  ` HashTable ` ：线程安全。
   *  ` HashMap ` ：非线程安全，允许 ` null ` 键值。

## 6.4 集合操作
- 遍历集合：
   ``` java
   for (String item : list) {
       System.out.println(item);
   }
   ```

---

# 第七章（浅看）
##  ` Swing ` 组件和容器
- 常见组件：
   *  ` JButton ` 、 ` JLabel ` 、 ` JTextField ` 。
- 事件处理：
   ``` java
   button.addActionListener(e -> System.out.println("Button clicked"));
   ```