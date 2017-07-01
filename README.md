# The Overview of Modern OpenGL

国内图形学教材过老过旧，导致很多概念前后矛盾，解释不清，所以我在读了国外的一些材料后，写就此文，解释一些OpenGL的现代特性。

OpenGL is a cross-platform library for interfacing with programmable GPUs for the purpose of rendering real-time 3D graphics.

这是OpenGL的定义，同时OpenGL在这么多年的发展过程中，有了很多变种，在手机上的OpenGL被称之为OpenGL ES，在网页端的OpenGL被称之为WebGL。

这个世纪以来，还有一项技术的迅速发展，这个技术就是GPU。Modern OpenGL的核心技术之一，就是为OpenGL提供了底层并行加速机制。

目前主流的GPU编程语言有两个：1、NVIDIA的CUDA；2、开源社区的OpenCL。这两种工具，都使得程序员和GPU硬件的交互成为了可能。

但是以上两种工具面向的是GPGPU，也就是general purpose GPU，因此OpenGL并不能直接使用这两种工具。

OpenGL自己有一套机制用于并行计算，也就是所谓的shader，底层不同品牌的硬件直接支持了OpenGL这套机制。

OpenGL有不少工具包，其中比较常用的有：GLUT（提供跨平台的交互界面），GLEW（GL的扩展包）。

很多文章上手就解释所谓的The Graphics Pipeline，我个人认为这是不好的，同时很难理解，因为压根不明白为什么要引入这个pipeline，为什么要有不同的shader。

要读懂这套pipeline，首先要明白两件事情：1、计算机图形学原理；2、OpenGL的并行机制。

## 计算机图形学原理和OpenGL的并行机制

计算机图形学的核心，就是将构建的三维场景，转换成图片，使得图片具有三维效果。为了生成图片，需要有以下阶段：

构造三维场景——三维场景中的坐标转为二维图片中的坐标——根据相机位置和参数裁剪图片——将图片坐标转为像素坐标——对像素上色

整个过程中，每个过程都可以使用并行计算加速。但是整个过程又是串行的，必须先完成一个过程，接着继续完成一个过程。

因此我们对这个过程起名叫做pipeline，对于pipeline上每一个过程进行并行处理，处理器叫做shader。在走完这个pipeline后，整个图片也就渲染完成了。

因此我们看看pipeline上每一步完成了什么：

构建完三维场景后，我们需要渲染出一张图片：

1、vertex shader：输入顶点的三维坐标，输出顶点对应的二维坐标；

2、triangle assembly：将顶点连接成三角面片；

3、rasterization（光栅化）：将二维形状，转换为像素表示；

4、fragment shader：输出最终渲染的图片数据；

## Cinder

我很不喜欢从0开始写OpenGL，因为有太多讨厌的宏，以及丑陋的编程方式。

后来遇到了cinder，这个框架真的很棒，所以这个repository下的工作，也都是使用cinder来实现的。