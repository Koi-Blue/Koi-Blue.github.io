# 第三阶段：中级图像处理

## 1. 图像滤波

### 目标：
- 学会使用 ` OpenCV ` 进行图像滤波。

### 内容：
- 平滑滤波（均值滤波、高斯滤波）
- 边缘检测（ ` Sobel ` 、 ` Canny ` ）
- 形态学操作（腐蚀、膨胀）

### 实践：
#### 1.1 平滑滤波
- 平滑滤波可以有效去除图像中的噪声，常见的平滑滤波方法有均值滤波和高斯滤波。

##### 均值滤波（ ` Average Filtering ` ）
- 均值滤波通过对每个像素点周围邻域的像素值取平均来进行平滑，能够去除一些噪声。

```cpp
#include <opencv2/opencv.hpp>
#include <iostream>

int main() {
    // 读取图像
    cv::Mat image = cv::imread("example.jpg");
    if (image.empty()) {
        std::cerr << "Could not open or find the image!" << std::endl;
        return -1;
    }

    // 均值滤波
    cv::Mat avgFiltered;
    cv::blur(image, avgFiltered, cv::Size(5, 5));  // 使用5x5的滤波器

    // 显示结果
    cv::imshow("Original Image", image);
    cv::imshow("Average Filtered Image", avgFiltered);
    cv::waitKey(0);
    return 0;
}
```
##### 解释：
-  ` cv::blur(image, avgFiltered, cv::Size(5, 5)) ` ：使用一个大小为 ` 5x5 ` 的窗口对图像进行均值滤波。
-  ` cv::imshow ` ：显示原始图像和经过滤波处理后的图像。
##### 高斯滤波（ ` Gaussian Filtering ` ）
- 高斯滤波器使用高斯分布对每个像素周围的像素加权平均，能够更好地保留图像的边缘信息。

``` cpp
cv::Mat gaussianFiltered;
cv::GaussianBlur(image, gaussianFiltered, cv::Size(5, 5), 1.5);  // 使用5x5的高斯核
```
##### 解释：
-  ` cv::GaussianBlur(image, gaussianFiltered, cv::Size(5, 5), 1.5) ` ： ` 1.5 ` 是高斯核的标准差，控制平滑的程度。
#### 1.2 边缘检测
- 边缘检测是图像处理中重要的操作之一，用于提取图像中的边缘信息。

#####  ` Sobel `  边缘检测
-  ` Sobel ` 算子用于检测图像中的边缘，通常应用于水平方向和垂直方向。

``` cpp
cv::Mat sobelX, sobelY, sobelCombined;
cv::Sobel(image, sobelX, CV_64F, 1, 0, 3);  // 水平方向Sobel
cv::Sobel(image, sobelY, CV_64F, 0, 1, 3);  // 垂直方向Sobel

// 合并X方向和Y方向的边缘
cv::magnitude(sobelX, sobelY, sobelCombined);

cv::imshow("Sobel Edge Detection", sobelCombined);
cv::waitKey(0);
```
##### 解释：
-  ` cv::Sobel(image, sobelX, CV_64F, 1, 0, 3) ` ：对图像应用 ` Sobel ` 滤波器， ` 1 ` 表示在水平方向计算导数， ` 0 ` 表示不计算垂直方向， ` 3 ` 表示使用的 ` Sobel ` 算子大小。
-  ` cv::magnitude(sobelX, sobelY, sobelCombined) ` ：合并 ` X ` 和 ` Y ` 方向的边缘检测结果。
#####  ` Canny ` 边缘检测
-  ` Canny ` 边缘检测是一种更为精确的边缘检测方法。

``` cpp
cv::Mat cannyEdges;
cv::Canny(image, cannyEdges, 100, 200);  // 阈值分别为100和200
cv::imshow("Canny Edge Detection", cannyEdges);
cv::waitKey(0);
```
##### 解释：
-  ` cv::Canny(image, cannyEdges, 100, 200) ` ： ` 100 ` 和 ` 200 ` 是 ` Canny ` 边缘检测的低阈值和高阈值。
#### 1.3 形态学操作
- 形态学操作主要包括腐蚀和膨胀，用于处理图像中的结构元素，常用于去除噪声和连接物体。

##### 腐蚀（ ` Erosion ` ）
- 腐蚀操作用于减小图像中物体的区域，通常用于去除小的白色噪点。

``` cpp

cv::Mat eroded;
cv::Mat kernel = cv::Mat::ones(3, 3, CV_8U);  // 3x3大小的结构元素
cv::erode(image, eroded, kernel);
cv::imshow("Eroded Image", eroded);
cv::waitKey(0);
```
##### 解释：
-  ` cv::erode(image, eroded, kernel) ` ：腐蚀操作，使用一个 ` 3x3 ` 的结构元素 ` （kernel） ` 对图像进行腐蚀。
##### 膨胀（ ` Dilation ` ）
- 膨胀操作用于扩展图像中物体的区域。

``` cpp
cv::Mat dilated;
cv::dilate(image, dilated, kernel);
cv::imshow("Dilated Image", dilated);
cv::waitKey(0);
```
##### 解释：
-  ` cv::dilate(image, dilated, kernel) ` ：膨胀操作，同样使用一个 ` 3x3 ` 的结构元素进行膨胀。
## 2. 特征检测
> 本部分特征检测先了解，需要线下聊
### 目标：
- 学会使用 ` OpenCV ` 进行特征检测和匹配。

### 内容：
- 角点检测（ ` Harris ` 角点检测）
-  ` SIFT ` 和 ` SURF ` 特征提取
- 特征匹配
### 实践：
#### 2.1 角点检测（ ` Harris ` 角点检测）
-  ` Harris ` 角点检测算法可以检测到图像中的角点，这些点通常是图像中特征丰富的区域。

``` cpp
cv::Mat gray, cornerStrength, cornerImage;
cv::cvtColor(image, gray, cv::COLOR_BGR2GRAY);
cv::cornerHarris(gray, cornerStrength, 3, 3, 0.04);

// 归一化结果
cv::normalize(cornerStrength, cornerStrength, 0, 255, cv::NORM_MINMAX, CV_32FC1);

// 标记角点
cornerImage = image.clone();
for (int i = 0; i < cornerStrength.rows; i++) {
    for (int j = 0; j < cornerStrength.cols; j++) {
        if (cornerStrength.at<float>(i, j) > 100) {
            circle(cornerImage, cv::Point(j, i), 5, cv::Scalar(0, 0, 255), 2);
        }
    }
}

cv::imshow("Harris Corner Detection", cornerImage);
cv::waitKey(0);
```
##### 解释：
-  ` cv::cornerHarris(gray, cornerStrength, 3, 3, 0.04) ` ：执行Harris角点检测，3是窗口大小，0.04是 ` Harris ` 检测器的自由参数。
-  ` cv::normalize(cornerStrength, cornerStrength, 0, 255, cv::NORM_MINMAX, CV_32FC1) ` ：归一化角点响应值，使其适合显示。
#### 2.2  ` SIFT ` 特征提取
-  ` SIFT ` （尺度不变特征变换）用于提取图像中的局部特征，它对图像的旋转、缩放和光照变化具有不变性。

``` cpp
#include <opencv2/xfeatures2d.hpp>

cv::Ptr<cv::xfeatures2d::SIFT> sift = cv::xfeatures2d::SIFT::create();
std::vector<cv::KeyPoint> keypoints;
cv::Mat descriptors;
sift->detectAndCompute(image, cv::Mat(), keypoints, descriptors);

cv::Mat output;
cv::drawKeypoints(image, keypoints, output);
cv::imshow("SIFT Features", output);
cv::waitKey(0);
```
##### 解释：
-  ` cv::xfeatures2d::SIFT::create() ` ：创建一个SIFT对象。
-  ` sift->detectAndCompute() ` ：提取图像的特征点（ ` keypoints ` ）和描述子（ ` descriptors ` ）。
-  ` cv::drawKeypoints(image, keypoints, output) ` ：将特征点绘制在图像上，显示提取的特征。
#### 2.3 特征匹配
- 特征匹配是将不同图像中相似的特征点进行匹配，常用的方法有暴力匹配（ ` Brute Force Matcher ` ）和 ` FLANN ` 匹配。

``` cpp
cv::Ptr<cv::xfeatures2d::SIFT> sift = cv::xfeatures2d::SIFT::create();
cv::Mat descriptors1, descriptors2;
std::vector<cv::KeyPoint> keypoints1, keypoints2;

// 提取特征点和描述子
sift->detectAndCompute(image1, cv::Mat(), keypoints1, descriptors1);
sift->detectAndCompute(image2, cv::Mat(), keypoints2, descriptors2);

// 暴力匹配
cv::BFMatcher matcher(cv::NORM_L2, true);
std::vector<cv::DMatch> matches;
matcher.match(descriptors1, descriptors2, matches);

// 绘制匹配结果
cv::Mat imgMatches;
cv::drawMatches(image1, keypoints1, image2, keypoints2, matches, imgMatches);
cv::imshow("Feature Matching", imgMatches);
cv::waitKey(0);
```
##### 解释：
-  ` cv::BFMatcher ` ：暴力匹配器， ` cv::NORM_L2 ` 表示使用欧氏距离进行匹配。
-  ` cv::drawMatches ` ：绘制匹配的特征点。