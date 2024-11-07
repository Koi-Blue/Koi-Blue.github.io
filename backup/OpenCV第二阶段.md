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

    // 等待用户按键
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
cv::Vec3b color = image.at<cv::Vec3b>(100, 100);  // BGR顺序
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