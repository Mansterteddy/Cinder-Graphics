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



OpenGL调用API Functions发生在run time，而不是compile time。

GLEW支持了Modern OpenGL  API。

GLFW处理窗口部分，同时接收鼠标和键盘输入。

GLM是处理矩阵和向量的数学库。现在的OpenGL已经不支持矩阵运算了。



要使用shader的话，要build own small library，其中包含shader、program，用来load、compile和link shaders。

Shaders are little programs, made from GLSL code, that run on the GPU instead of the CPU.

在modern OpenGL中，shader是必需的。



Vertex Shaders

The main purpose of a vertex shader is to transform points (x, y, and z coordinates) into different points.

gl_Position是OpenGL定义的全局变量，用于保存vertex shader的输出，所有的vertex shader都必须设置gl_Position这个变量。



Fragment Shaders

The main purpose of a fragment shader is to calculate the color of each pixel that is drawn.

A fragment is basically a pixel, so fragment shaders means pixel shaders.



OpenGL中采用了和并行计算相同的机制

CPU写数据到GPU的存储单元，GPU处理好后，放在存储单元里，CPU去存储单元中，取出结果。



在OpenGL中，CPU将要处理的信息，发送给显卡上的存储单元，这块单元就被称为VBO。

VBOs (vertex buffer objects) are buffers of video memory - just a bunch of bytes containing any kind of binary data you want.

VBO可以放任何东西。

VAO要处理VBO的内容，VBO是不清楚自己保存了什么东西的，VAO通过程序，知道了在VBO中，有什么样的参数，随后将这些参数，传递到该去的地方。

VAOs (vertex array objects) are the link between the VBOs and the shader variables. VAOs describe what type of data is contained within a VBO, and which shader variables the data should be sent to.

Attribute variables can have a different value for each vertex. Uniform variables keep the same value for multiple vertices.

Uniforms can be accessed from any shader, but attributes must enter the vertex shader first, not the fragment shader. The vertex shader can pass the value into the fragment shader if necessary.

Textures are basically 2D images that you can apply to your 3D objects.

Displaying a 2D image on 3D geometry is the most common use.

The pixel width and height of a texture should be a power of two.

The strange thing about texture coordinates is that they are not in pixels. They range from zero to one, where (0, 0) is the bottom left and (1, 1) is the top right.

You can not just send a texture straight to a shader. First you bind the texture to a texture unit, the you send the index of the texture unit to the shader.

uniform 和 attribute的区别

uniform代表每一个顶点只有一种纹理 uniform可以被任何shader读取

attribute代表每一个顶点有多种纹理 attribute只能被vertex shader读取

The clip volume is a cube. Whatever is inside the clip volume appears on the screen, and anything outside the clip volume is not visible.