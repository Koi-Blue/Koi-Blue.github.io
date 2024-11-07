# 第二阶段：OpenCV 基础

## 1. OpenCV 安装与配置

### 目标：
- 学会在Linux上安装和配置OpenCV。

### 内容：
- 安装OpenCV库（使用包管理器或从源码编译）
- 配置CMake以使用OpenCV

### 实践：
#### 1.1 安装 OpenCV
- 在Linux下，OpenCV可以通过两种主要方式安装：使用包管理器（如APT，YUM等）或者从源码编译。

##### 方式一：使用包管理器安装（以Ubuntu为例）
- 在Ubuntu系统上，可以通过APT包管理器快速安装OpenCV。

```bash
sudo apt update
sudo apt install libopencv-dev
```
##### 方式二：从源码编译
``` bash
# 安装依赖项
sudo apt update
sudo apt install build-essential cmake git pkg-config libjpeg-dev libpng-dev libtiff-dev libjasper-dev libeigen3-dev

# 下载OpenCV源码
git clone https://github.com/opencv/opencv.git
cd opencv
mkdir build
cd build

# 配置CMake并编译
cmake ..
make -j4
sudo make install
```

#### 1.2 配置CMake以使用OpenCV
- 在CMake项目中，配置OpenCV需要设置OpenCV_DIR并通过 ` find_package ` 来找到OpenCV。假设我们已经安装了OpenCV库：
CMakeLists.txt :
``` cmake
cmake_minimum_required(VERSION 3.10)
project(OpenCVExample)

# 设置C++标准
set(CMAKE_CXX_STANDARD 11)

# 设置OpenCV的路径（根据安装路径调整）
find_package(OpenCV REQUIRED)

# 添加可执行文件
add_executable(OpenCVExample main.cpp)

# 链接OpenCV库
target_link_libraries(OpenCVExample ${OpenCV_LIBS})
```

## 2. 图像读取与显示
### 目标 :
- 学会使用OpenCV读取、显示和保存图像。

### 内容：
- ` cv::imread ` 和 ` cv::imshow ` 函数
- 图像格式和颜色空间

### 实践：
#### 2.1 读取并显示图像
- 使用 ` cv::imread ` 读取图像，并使用 ` cv::imshow ` 显示图像。
``` cpp
#include <opencv2/opencv.hpp>
#include <iostream>

int main() {
    // 读取图像（默认以BGR格式读取）
    cv::Mat image = cv::imread("example.jpg");

    // 检查图像是否成功读取
    if (image.empty()) {
        std::cerr << "Could not open or find the image!" << std::endl;
        return -1;
    }

    // 显示图像
    cv::imshow("Display Image", image);

    // 0为等待用户按键，也可以创建延时，你们懂
    cv::waitKey(0);

    // 销毁所有窗口
    cv::destroyAllWindows();

    return 0;
}
```
##### 解释：

- ` cv::imread("example.jpg") `：读取指定路径的图像文件，返回一个 ` cv::Mat ` 对象（图像数据）。
-  ` cv::imshow("Display Image", image) ` ：显示图像窗口， ` "Display Image" ` 为窗口名称。
-  ` cv::waitKey(0) ` ：等待按键事件， ` 0 ` 表示无限等待直到有按键按下。
-  ` cv::destroyAllWindows() ` ：关闭所有 ` OpenCV ` 创建的窗口。

#### 2.2 将图像保存到文件
- 使用 ` cv::imwrite ` 将图像保存到文件中。
``` cpp
cv::imwrite("output.jpg", image);
```
## 图像处理基础
### 目标：
- 掌握基本的图像处理技术。
### 内容：

- 图像通道操作
- 颜色空间转换
- 图像缩放和旋转
- 像素操作

### 实践：
#### 3.1 将彩色图像转换为灰度图像
 - ` OpenCV ` 提供了多种颜色空间转换函数。使用 ` cv::cvtColor ` 可以将彩色图像转换为灰度图像。
``` cpp
cv::Mat grayImage;
cv::cvtColor(image, grayImage, cv::COLOR_BGR2GRAY);
cv::imshow("Gray Image", grayImage);
cv::waitKey(0);
```
##### 解释：
- ` cv::cvtColor(image, grayImage, cv::COLOR_BGR2GRAY) ` ：将输入图像从BGR色彩空间转换为灰度图像。 ` cv::COLOR_BGR2GRAY ` 是转换标志。

#### 3.2 图像缩放和旋转
- 缩放图像：使用 ` cv::resize ` 来缩放图像。
- 旋转图像：通过仿射变换来旋转图像。
```cpp
// 缩放图像
cv::Mat resizedImage;
cv::resize(image, resizedImage, cv::Size(300, 300));  // 将图像缩放为300x300大小

// 显示缩放后的图像
cv::imshow("Resized Image", resizedImage);

// 旋转图像
cv::Mat rotatedImage;
cv::Point2f center(resizedImage.cols / 2, resizedImage.rows / 2);  // 旋转中心
cv::Mat rotationMatrix = cv::getRotationMatrix2D(center, 45, 1);  // 旋转45度，缩放因子为1
cv::warpAffine(resizedImage, rotatedImage, rotationMatrix, resizedImage.size());  // 执行旋转变换

// 显示旋转后的图像
cv::imshow("Rotated Image", rotatedImage);
cv::waitKey(0);
```
##### 解释：
 - ` cv::resize ` ：用于调整图像大小， ` cv::Size(300, 300) ` 表示缩放到300x300的尺寸。
 - ` cv::getRotationMatrix2D(center, angle, scale) ` ：生成一个旋转矩阵， ` center ` 是旋转中心， ` angle ` 是旋转角度， ` scale ` 是缩放因子。
-  ` cv::warpAffine ` ：使用仿射变换矩阵对图像进行旋转。

### 3.3 像素操作
 - ` OpenCV ` 允许直接访问和修改图像的像素值。通过 ` cv::Mat ` 对象可以访问图像的每个像素。
``` cpp
// 访问图像的第(100,100)个像素
cv::Vec3b color = image.at<cv::Vec3b>(100, 100);  // BGR顺序，这很细节
std::cout << "Pixel at (100, 100): " 
          << "Blue: " << (int)color[0] << " "
          << "Green: " << (int)color[1] << " "
          << "Red: " << (int)color[2] << std::endl;

// 修改图像的某个像素值
image.at<cv::Vec3b>(100, 100) = cv::Vec3b(0, 255, 0);  // 将该像素修改为绿色
cv::imshow("Modified Image", image);
cv::waitKey(0);
```
##### 解释：
-  ` image.at<cv::Vec3b>(100, 100) ` ：访问图像的 ` (100,100) ` 像素，返回一个 ` cv::Vec3b ` 对象，其中包含该像素的BGR值。
-  ` image.at<cv::Vec3b>(100, 100) = cv::Vec3b(0, 255, 0) ` ：将该像素的值修改为绿色。


## 拓展
> 关于我们会用到的编程思想：面向对象编程。

## 面向对象编程与C++中的面向对象

## 1. 面向对象编程（OOP）的概述

### 什么是面向对象编程？
面向对象编程（Object-Oriented Programming，简称OOP）是一种程序设计范式，它通过**对象**来组织程序结构。每个对象代表了程序中的一个实体，它将数据和操作这些数据的方法结合在一起，形成了“类”的概念。

在面向对象编程中，程序是由对象和对象之间的交互组成的，核心思想是：
- **封装（Encapsulation）**：将数据（属性）和操作数据的方法（函数）打包成一个单位，称为对象。通过封装，我们可以隐藏实现细节，仅暴露必要的接口。
- **继承（Inheritance）**：通过继承，可以创建一个新类（子类）从现有的类（父类）派生，继承父类的属性和方法。继承促进了代码的重用性。
- **多态（Polymorphism）**：允许通过一个接口来操作不同类型的对象，运行时决定实际执行的方法，增强了代码的灵活性和可扩展性。
- **抽象（Abstraction）**：通过创建抽象类或接口，定义对象的公共特征，同时隐藏具体实现。抽象帮助我们简化问题，并专注于核心功能。

### 面向对象编程的优点：
1. **代码重用性**：继承允许子类重用父类的代码，减少了重复代码。
2. **可维护性**：封装和抽象使得对象的内部实现与外部使用分离，增加了代码的可维护性。
3. **灵活性**：多态性使得代码更加灵活，可以通过相同的接口调用不同的实现。

## 2. C++中的面向对象编程

C++是一种支持面向对象编程的语言，它通过类和对象来实现OOP的概念。

### 2.1 类与对象

- **类（Class）**是对象的蓝图或模板。它定义了对象的属性（数据成员）和行为（成员函数）。
- **对象（Object）**是类的实例，它拥有类定义的属性和方法。

#### 类的定义和对象的创建：

```cpp
#include <iostream>
using namespace std;

// 定义一个类
class Car {
private:
    string brand;
    int year;

public:
    // 构造函数：用于创建对象时初始化属性
    Car(string b, int y) : brand(b), year(y) {}

    // 成员函数：获取品牌信息
    void displayInfo() {
        cout << "Brand: " << brand << ", Year: " << year << endl;
    }

    // 设定品牌
    void setBrand(string b) {
        brand = b;
    }

    // 获取品牌
    string getBrand() {
        return brand;
    }
};

int main() {
    // 创建对象
    Car myCar("Toyota", 2020);
    myCar.displayInfo();

    // 修改品牌
    myCar.setBrand("Honda");
    cout << "Updated Brand: " << myCar.getBrand() << endl;

    return 0;
}
```
#### 解释：
-  ` class Car ` ：定义了一个 ` Car ` 类，类包含 ` brand ` 和 ` year ` 两个属性， ` displayInfo ` 、 ` setBrand ` 和 ` getBrand ` 是其成员函数。
-  ` Car myCar("Toyota", 2020); ` ：创建了一个 ` Car ` 类型的对象 ` myCar ` ，并通过构造函数初始化了 ` brand ` 和 ` year ` 属性。

### 2.2 封装
- 封装是面向对象的一个重要概念。通过封装，类可以将数据（成员变量）和操作数据的函数（成员函数）组合起来，同时隐藏不必要的细节，外部代码只能通过公开的接口访问对象的数据。
``` cpp
class BankAccount {
private:
    double balance;

public:
    // 构造函数：初始化账户余额
    BankAccount(double initial_balance) : balance(initial_balance) {}

    // 存款操作
    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    // 取款操作
    bool withdraw(double amount) {
        if (amount > 0 && balance >= amount) {
            balance -= amount;
            return true;
        }
        return false;
    }

    // 获取账户余额
    double getBalance() {
        return balance;
    }
};
```
#### 解释：
-  ` private ` 访问修饰符：表示 ` balance ` 是私有成员，只能在类的内部访问。
-  ` deposit ` 、 ` withdraw ` 和 ` getBalance ` 是公开的成员函数，它们提供了对账户余额的操作接口。

### 2.3 继承
- 继承使得我们能够创建一个新的类（子类），它继承父类的属性和方法。子类可以扩展或修改父类的行为。
``` cpp
class Vehicle {
public:
    string brand;

    Vehicle(string b) : brand(b) {}

    void honk() {
        cout << "Beep! Beep!" << endl;
    }
};

// 子类继承自Vehicle类
class Car : public Vehicle {
public:
    int doors;

    Car(string b, int d) : Vehicle(b), doors(d) {}

    void displayInfo() {
        cout << "Brand: " << brand << ", Doors: " << doors << endl;
    }
};

int main() {
    Car myCar("Toyota", 4);
    myCar.honk();
    myCar.displayInfo();

    return 0;
}
```
#### 解释：
-  ` class Vehicle ` ：父类，包含了一个 ` brand ` 属性和一个 ` honk ` 方法。
-  ` class Car : public Vehicle ` ： ` Car ` 类继承自 ` Vehicle ` 类，继承了 ` brand ` 属性和 ` honk ` 方法，并增加了 ` doors ` 属性和 ` displayInfo ` 方法。

### 2.4 多态
- 多态是指同一接口可以作用于不同的对象，并产生不同的结果。在C++中，使用虚函数来实现多态。
``` cpp
class Shape {
public:
    virtual void draw() {
        cout << "Drawing a shape" << endl;
    }
};

class Circle : public Shape {
public:
    void draw() override {
        cout << "Drawing a circle" << endl;
    }
};

class Square : public Shape {
public:
    void draw() override {
        cout << "Drawing a square" << endl;
    }
};

int main() {
    Shape* shape1 = new Circle();
    Shape* shape2 = new Square();

    shape1->draw();
    shape2->draw(); 

    delete shape1;
    delete shape2;

    return 0;
}
```
#### 解释：
-  ` virtual void draw() ` ： ` Shape ` 类中的虚函数，允许子类重写它。
-  ` override ` 关键字：在子类中重写父类的虚函数，保证子类提供具体的实现。