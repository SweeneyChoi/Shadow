# Shadow

使用阴影贴图（shadow mapping）以及扩展的高级算法Omnidirectional Shadow Maps实现实时动态阴影

## 依赖
* glfw3.lib 推荐在[官方网站](http://www.glfw.org/download.html)下载源代码，然后自行编译。本项目编译使用的是CMake和Visual Studio 2015.
* GLAD 打开GLAD的[在线服务](http://glad.dav1d.de/)可轻松配置。本项目使用OpenGL 4.3.
* stb_image.h 是[Sean Barrett](https://github.com/nothings)的一个非常流行的单头文件图像加载库，可以在[这里](https://github.com/nothings/stb/blob/master/stb_image.h)下载。本项目使用其来加载纹理图片。
* GLM 一个只有头文件的库，不用链接和编译。可以在它们的[网站](http://glm.g-truc.net/0.9.5/index.html)上下载。本项目使用其作为数学库。
* Assimp 一个非常流行的模型导入库，可以在[下载页面](http://assimp.org/main_downloads.html)选择相应的版本，自行使用CMake 和 Visual Studio 2015编译。

## 思路

渲染一个点P处的片元，需要决定它是否在阴影中。先得使用T把P变换到光源的坐标空间里。既然点P是从光的透视图中看到的，它的z坐标就对应于它的深度。使用点P在光源的坐标空间的坐标，可以索引深度贴图，来获得从光的视角中最近的可见深度。这样即可判断一个点是否在阴影中。

## 步骤
1. 首先，以光源视角渲染深度贴图，普通阴影算法只用将深度贴图设置为2D纹理就行了，而适合于OSM算法的点光源则需要一个立方体贴图的深度贴图

2. 像往常一样渲染场景，使用生成的深度贴图来计算片元是否在阴影之中

## 改进

1. 使用**阴影偏移（shadow bias）**技巧来解决**阴影失真(Shadow Acne)**，即明显的线条样式。
2. 使用一个简单的**PCF**，从纹理像素四周对深度贴图采样，然后把结果平均起来，使其产生柔和阴影，使它们出现更少的锯齿块和硬边。

## 效果	

- 阴影映射

![阴影映射](https://github.com/SweeneyChoi/ShadowMap/blob/master/images/shadow.png)

- 万向阴影贴图技术

![OSM](https://github.com/SweeneyChoi/ShadowMap/blob/master/images/pointShadow.png)


