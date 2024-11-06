> 声明：仅作为今天交流内容参考
## 0 构建CMakeLists.txt文件
- 先声明 cmake 版本及项目名称
- 查找OpenCV包
``` CMakeLists.txt
find_package(OpenCV REQUIRED)
```
- 包含OpenCV头文件
```
include_directories(${OpenCV_INCLUDE_DIRS})
```
- 添加可执行文件
- 链接OpenCV库
```
target_link_libraries(ImageToGray ${OpenCV_LIBS})
```

## 1 主要功能的C++实现
### 1.1 导入opencv头文件
``` cpp
#include <opencv2/opencv.hpp>
```
### 1.2 主函数
``` cpp
int main(int argc, char** argv) 
```
- ``` main ``` 函数是C++程序的入口点。
- ``` argc ``` 参数表示传递给程序的命令行参数的数量。
- ``` argv ``` 参数是一个字符指针数组，包含了所有命令行参数。

### 1.3 参数检查
``` cpp
if (argc != 2) {
    std::cout << "Usage: ./ImageToGray <image_path>\n";
    return -1;
}
```
- 检查命令行参数的数量是否正确。如果参数数量不是2（包括程序名本身），则输出使用说明并返回错误码 -1。

### 1.4 图像读取
``` cpp
cv::Mat image = cv::imread(argv[1], cv::IMREAD_COLOR);
if (image.empty()) {
    std::cout << "Could not open or find the image\n";
    return -1;
}
```
- ``` cv::imread ``` 函数用于从文件中读取图像。``` argv[1] ``` 是图像文件的路径，``` cv::IMREAD_COLOR ``` 表示以彩色模式读取图像。
- ``` image.empty() ``` 检查图像是否成功加载。如果图像未找到或无法打开，则输出错误信息并返回错误码 ``` -1 ```。

### 1.5 图像转换为灰度
``` cpp
cv::Mat gray_image;
cvtColor(image, gray_image, cv::COLOR_BGR2GRAY);
```
- ``` cv::Mat ``` 是OpenCV中用于存储图像数据的类。
- ``` gray_image ``` 是用来存储灰度图像的变量。
- ``` cvtColor ``` 函数用于颜色空间转换，将彩色图像转换为灰度图像。``` cv::COLOR_BGR2GRAY ``` 指定了从BGR颜色空间到灰度颜色空间的转换。

### 1.6 显示图像
``` cpp
cv::namedWindow("Original Image", cv::WINDOW_NORMAL);
cv::imshow("Original Image", image);

cv::namedWindow("Gray Image", cv::WINDOW_NORMAL);
cv::imshow("Gray Image", gray_image);
```
- ``` cv::namedWindow ``` 函数创建一个窗口，用于显示图像。第一个参数是窗口的名称，第二个参数是窗口的类型，``` cv::WINDOW_NORMAL ``` 表示窗口大小可以调整。
- ``` cv::imshow ``` 函数在指定的窗口中显示图像。第一个参数是窗口的名称，第二个参数是要显示的图像。

### 1.7 等待用户输入
``` cpp
cv::waitKey(0);
```
- ``` cv::waitKey ``` 函数等待用户按键事件。参数 0 表示无限期等待，直到用户按下任意键。如果传入一个正整数（如 5000），则表示等待指定毫秒数。该操作用于把图像留在窗口上。

## 注意
- 编译之后得到可执行文件，在命令行窗口运行的时候需要``` ./可执行文件名称 图片路径 。
- 亦可将路径写入cpp文件中，直接读取文件路径。
- : )