`static` 是 Java 中一个非常重要的关键字，它用于指示某个成员属于类本身，而不是类的某个实例。通过 `static` 声明的成员可以在没有创建对象的情况下访问。下面将详细展示 `static` 的多种用法。

- 静态变量：属于类的所有实例共享，不依赖于实例化。
- 静态方法：属于类的所有实例共享，不能访问实例成员。
- 静态代码块：用于初始化静态变量，类加载时执行一次。
- 静态内部类：属于外部类本身，而非外部类的实例。
- 静态导入：允许导入类的静态成员，简化调用。

### 1. 静态变量
静态变量是属于类本身的，而不是某个特定对象。所有类的实例共享同一个静态变量。
**示例**
``` java
class Counter {
    static int count = 0;  // 静态变量
    
    // 构造方法
    Counter() {
        count++;  // 每创建一个对象，静态变量 count 增加
    }
    
    void displayCount() {
        System.out.println("Count: " + count);
    }
}

public class Main {
    public static void main(String[] args) {
        Counter c1 = new Counter();
        Counter c2 = new Counter();
        c1.displayCount();  // 输出: Count: 2
        c2.displayCount();  // 输出: Count: 2
    }
}
```
在这个例子中，`count` 是一个静态变量，不管创建多少个 `Counter` 对象，所有对象共享这个变量。它会随每个对象的创建而递增。

### 2. 静态方法
静态方法属于类本身，而不是类的某个对象。静态方法只能访问静态变量和静态方法，不能访问实例变量和实例方法。
**示例**
``` java
class MathUtils {
    static int square(int x) {  // 静态方法
        return x * x;
    }
    
    static int cube(int x) {  // 静态方法
        return x * x * x;
    }
}

public class Main {
    public static void main(String[] args) {
        int result = MathUtils.square(5);
        System.out.println("Square of 5: " + result);  // 输出: Square of 5: 25
        
        result = MathUtils.cube(3);
        System.out.println("Cube of 3: " + result);  // 输出: Cube of 3: 27
    }
}
```
`square` 和 `cube` 是静态方法，可以直接通过类名调用，而不需要实例化 `MathUtils` 类。

### 3. 静态代码块
静态代码块在类加载时执行一次，常用于初始化静态变量。静态代码块在类的构造器之前运行，且只会执行一次。
**示例**
``` java
class MyClass {
    static int value;
    
    static {
        value = 10;  // 静态代码块初始化静态变量
        System.out.println("Static block is executed.");
    }
    
    void displayValue() {
        System.out.println("Value: " + value);
    }
}

public class Main {
    public static void main(String[] args) {
        MyClass obj1 = new MyClass();
        MyClass obj2 = new MyClass();
    }
}
```
输出：
```
Static block is executed.
Static block is executed.
```
静态代码块只在类加载时执行一次，即使创建多个实例，也只会执行一次。

### 4. 静态内部类
静态内部类是属于外部类的，而不是外部类的实例。静态内部类可以直接访问外部类的静态成员，但不能访问外部类的实例成员。
**示例**
``` java
class Outer {
    static int staticVar = 10;
    
    static class Inner {
        void display() {
            System.out.println("Static variable: " + staticVar);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Outer.Inner inner = new Outer.Inner();
        inner.display();  // 输出: Static variable: 10
    }
}
```
`Inner` 类是 `Outer` 类的静态内部类，它可以访问 `Outer` 类的静态变量 `staticVar`。

### 5. 静态导入
Java 允许使用 `static import` 语法来直接导入类的静态成员，使得在代码中可以直接使用静态成员，而不需要使用类名。
**示例**
``` java
import static java.lang.Math.*;  // 导入所有 Math 类的静态方法

public class Main {
    public static void main(String[] args) {
        double result = sqrt(25);  // 直接使用 sqrt 方法
        System.out.println("Square root of 25: " + result);  // 输出: Square root of 25: 5.0
    }
}
```
通过 `static import`，我们可以直接调用 `Math` 类中的静态方法（如 `sqrt`）而无需写出类名。

### 6. 静态方法与实例方法的区别
静态方法和实例方法的一个重要区别是，静态方法可以在没有创建对象的情况下调用，而实例方法则必须通过对象来调用。
**示例**
``` java
class MyClass {
    static void staticMethod() {
        System.out.println("Static method called");
    }
    
    void instanceMethod() {
        System.out.println("Instance method called");
    }
}

public class Main {
    public static void main(String[] args) {
        MyClass.staticMethod();  // 静态方法无需创建对象
        MyClass obj = new MyClass();
        obj.instanceMethod();  // 实例方法必须通过对象来调用
    }
}
```