# Java复习纲要

## 第一章

### Java的特点
- **跨平台性**：基于Java虚拟机（JVM），一次编写，到处运行。
- **面向对象**：支持封装、继承、多态。
- **内存管理**：自动垃圾回收机制。
- **安全性**：通过类加载器和字节码验证，防止恶意代码。
- **多线程**：内置线程支持。
- **丰富的API库**：提供功能强大的类库。

### Java虚拟机
- **JVM职责**：
  - 加载、验证、解释和执行Java字节码。
  - 垃圾回收和内存管理。
  - 支持语言特性（如动态加载）。
- **JVM结构**：
  - 类加载子系统。
  - 内存区域：堆、方法区、栈、本地方法栈、程序计数器。
  - 执行引擎。

### 1.4 Java数组
- **创建、声明、初始化、引用**：
  ```java
  int[] arr = {1, 2, 3}; // 静态初始化
  int[] arr2 = new int[5]; // 动态初始化
  int[][] arr3 = new int[5][5]
   ```
- 遍历过程中发生的事情：
   * 按索引访问元素。
   * 循环中对元素进行操作可能会改变数组的值。
- 关键字：
   * ` break ` ：跳出当前循环。
   * ` continue ` ：跳过当前迭代，进入下一次循环。
- 多维数组：
   * 每一行的元素个数可以不同。

### 1.5.6 和 1.5.7 遍历数组时的关键字
- ` break ` 和 ` continue ` 的作用：

   ``` java
   for (int i = 0; i < arr.length; i++) {
         if (arr[i] == 5) break; // 提前退出循环
         if (arr[i] % 2 == 0) continue; // 跳过偶数
         System.out.println(arr[i]);
   }
   ```

## 第二章

### 2.2  类或对象
- 类的定义：
   * 属性和方法。
   * 修饰符（public，private，protected）
- 对象：
   * 通过类实例化
   * 访问成员变量和方法

#### 2.2.6 类的访问控制
- public：可被所有类访问。
- protected：仅在桐宝子类中可访问。
- private：仅在本类中可访问。
- 默认（包访问权限）：仅在同包中可访问。

### 2.3 对象初始化和回收
- 构造方法：
   * 无参和有参构造方法。
   * 特点：与类名相同，无返回值。
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
   * Java 内存回收机制：
      - 自动垃圾回收（GC）。
      - System.gc（）仅为建议，无法强制。
   * 变量赋值：
      - 若赋值：使用赋值值。
      - 若未赋值：
         * 数值类型：默认为0.
         * 布尔类型：默认false。
         * 引用类型：默认为null。

## 第三章

### 3.1 类的继承
- 子类和父类的关系：
   ``` java
   public class Parent {
       public void method() {
           System.out.println("Parent method");
       }
   }
   public class Child extends Parent {
       @Override
       public void method() {
           System.out.println("Child method");
       }
   }
   ```
- 测试类：
   ``` java
   public class Test {
       public static void main(String[] args) {
           Parent obj = new Child();
           obj.method(); // 输出: Child method
       }
   }
   ```

### 3.2 Object 类
- 相等与同一：
   * == 比较地址值。
   * equals 默认实现比较地址，可重写比较内容。

### 3.3 final类与方法
- final类不能被继承。
- final方法不能被重写。

### 3.4 抽象类
- 特点：
   * 不能实例化。
   * 包含抽象方法和非抽象方法。

## 第四章

### 4.1接口
- 定义接口：
   ``` java
   public interface MyInterface {
       int CONSTANT = 100; // 默认`public static final`
       void method(); // 默认`public abstract`
   }
   ```

- 实现接口：
   ``` java
   public class MyClass implements MyInterface {
       @Override
       public void method() {
           System.out.println("Implementing interface");
       }
   }
   ```

- 测试类：
   ``` java
   public class Test {
       public static void main(String[] args) {
           MyInterface obj = new MyClass();
           obj.method();
       }
   }
   ```

### 4.5 构造方法与多态
- 构造方法的调用顺序：
   * 父类构造方法 -> 子类构造方法。
- 多态：
   * 父类引用指向子类对象。

## 第五章

### 5.1 异常处理机制
- 简介：
   * 异常是程序执行过程中出现的错误。
   * 异常分为检查型（IOException）和非检查型（RuntimeException）。

### 5.2 异常处理
- 语句块：
   ``` java
   try {
       // 可能抛出异常的代码
   } catch (ExceptionType e) {
       // 异常处理
   } finally {
       // 始终执行
   }
   ```
- catch顺序：
   * 子类异常必须在父类异常之前。

### 5.3 输入输出流
- 键盘输入：
   ``` java
   Scanner scanner = new Scanner(System.in);
   int n = scanner.nextInt();
   ```
- 文件读写：
   ``` java
   try (BufferedReader br = new BufferedReader(new FileReader("file.txt"));
        BufferedWriter bw = new BufferedWriter(new FileWriter("output.txt"))) {
       String line;
       while ((line = br.readLine()) != null) {
           bw.write(line);
           bw.newLine();
       }
   } catch (IOException e) {
       e.printStackTrace();
   }

## 第六章
### 6.3 集合框架中的常用类
- HashSet：无序，不允许重复元素。
- Vector 和 ArrayList：
   * Vector：线程安全。
   * ArrayList：效率更高，非线程安全。
- LinkedList：双向链表结构。
- HashTable 和 HashMap：
   * HashTable：线程安全。
   * HashMap：非线程安全，允许null键值。

### 6.4 集合操作
- 遍历集合：
   ``` java
   for (String item : list) {
       System.out.println(item);
   }
   ```

## 第七章（浅看）
### Swing组件和容器
- 常见组件：
   * JButton、JLabel、JTextField。
- 事件处理：
   ``` java
   button.addActionListener(e -> System.out.println("Button clicked"));
   ```