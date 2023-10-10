

## 学习建议（来自ChatGPT）
要学习URP渲染管线，可以按照以下步骤进行：

1. 熟悉Unity的渲染管线的概念和流程。了解渲染管线的基本模块，例如：顶点处理、几何处理、纹理采样、片元着色等，这对于理解URP的渲染管线是非常有帮助的。

2. 熟悉Universal Render Pipeline的基本概念，例如Lighting、Shader、Asset等。建议在官方文档的基础概念部分进行学习。

3. 了解URP的漫反射和镜面反射光照模型以及自带的SRP Batcher优化工具。这些对于优化场景性能是非常重要的。

4. 学习URP的shader的编写，熟悉shader编写的基础语法，并学习URP渲染管线中定义的一些特有的变量或函数。可以自己编写一些简单的Shader，并对着文档学习URP模板。

5. 理解渲染管线中的一些优化技术，例如视锥剔除、批处理、Mipmap、图集等。这些技术可以在URP中得到更好的体现，理解这些技术的使用对于优化场景性能非常有帮助。

6. 掌握URP的一些高级技巧，例如GPU Instancing、SRP Batcher等。这些技巧对于大规模的场景优化，例如场景宏观壹体化等，非常有帮助。

以上就是一般学习URP渲染管线的思路，建议可以参考官方文档进行深入的学习。
### 重点
>基本模块
> 1. 顶点处理
> 2. 几何处理
> 3. 纹理采样
> 4. 片元着色
> ...
>
>基本概念
> 1. Lighting
> 2. Shader
> 3. Asset
> ...
>
>光照模型、性能优化工具、Shader的编写等

## 学习笔记
### 重要提醒
1. 将项目从一个渲染管线切换到另一个渲染管线可能很困难，因为不同的渲染管线使用不同的着色器输出，并且有不同的特性。所以在创建项目时就应决定使用什么渲染管线。
2. 内置渲染管线是Unity默认提供的渲染管线，主要面向低端设备和移动平台，目标是提供良好的性能和可移植性。它的渲染流程是固定的，难以自定义，但是开发者可以通过编写Shader在小范围内进行一些个性化的渲染。
通用渲染管线（URP）是Unity的轻量级渲染管线，主要面向跨平台的2D和3D游戏开发，提供了一些新的Lighting、Shader和Rendering等基本部分，相比于内置渲染管线在对性能的要求上做了很大的优化，在简单场景中性能表现非常优秀，但在细节较多、场景较复杂（比如大型开放世界）时性能表现不如HDRP。
高清渲染管线（HDRP）是Unity新推出的高保真游戏渲染管线，主要面向PC和主机平台的AAA级开发，提供了许多高级视觉实现技术，例如全局光照、高级材料、真实时阴影等，但相对于URP和内置渲染管线在性能上要求更高。
自定义渲染管线是根据游戏项目的特定需求，开发者自行编写的一套渲染管线，它可以让开发者从底层实现自己的渲染过程，可以对渲染流程和贴图管线进行完全的控制，非常灵活，但开发成本和难度相对较高。
实现自定义渲染管线需要开发者熟练掌握计算机图形学相关的知识，包括顶点着色、片元着色、光照、阴影、纹理等，同时需要了解计算机图形学中的各种算法和流程，例如深度测试、剪裁、三角剖分、法线计算、贴图过滤等等，这些都需要有较高的数学和计算机基础。所以相对于其他渲染管线来说，自定义渲染管线难度更高，需要相应的专业基础和经验。（来自chatGPT）
### **学习建议**
学习URP（Universal Render Pipeline）主要需要学习以下几个模块：
1. **Rendering Module**：渲染模块是URP的核心组件，它包含渲染管线（Pipeline）和渲染插件（Renderer Feature）等。渲染模块是URP的基础，它提供了许多可自定义的渲染管线功能和工具。
2. **Shader Graph**：Shader Graph是Unity提供的一个可视化着色器编辑器，它允许开发人员通过拖放节点来创建复杂的着色器。当使用URP时，Shader Graph被用于创建自定义Shader，并与渲染管线集成，实现高品质的渲染效果。
3. **Post-Processing**：Post-Processing是一种后期处理技术，可以用于对已渲染的图像进行处理，从而提高渲染效果。URP集成了Unity内置的Post-Processing Stack v2，它提供了众多可自定义的后期处理效果，如景深、运动模糊、边缘检测等。
4. **Lighting Module**：光照模块是URP的另一个重要模块，它负责处理Unity中的光照和阴影。URP提供了许多可自定义的光照功能和技术，如实时光照、高动态范围（HDR）光照、阴影范围控制等。
5. **URP Asset**：URP Asset是URP的配置文件，它可以用于自定义渲染管线和渲染效果。通过针对具体项目需求定制URP Asset，开发人员可以优化渲染效果和性能。
以上这些是URP的主要模块，学习时可以根据需求逐一学习。另外，许多细节和技术点都有详细的文档介绍和示例工程，可以通过官方文档和论坛等途径学习。
（来自ChatGPT）

### [URP](https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@12.1/manual/light-component.html)
#### 如何使用URP
创建URP资源，并且在项目图形设置中分配该资源。
##### 两种资源模板
*URP Asset(with 2D Renderer)*：主要用于2D场景的渲染，它专注于处理2D Sprite、UI元素等视觉元素，也提供一些2D特有的渲染选项（例如2D Lights、2D Shadows）。
*URP Asset(with Universal Renderer)*：主要用于3D或者混合2D/3D场景的渲染，它提供了更全面的渲染选项，可以处理更广泛的情况，比如PBR材质、后处理等。

#### 可配置设置
- 常规(General)
- 质量(Quality)
- 照明(Lighting)
- 阴影(Shadows)
- 后处理(Post-processing)
- 高级(Advanced)
- 自适应性能(Adaptve Performance)
##### 常规
控制核心部分
- 深度纹理(Depth Texture)
- 不透明度纹理(Opaque Texture)
- 不透明度缩减采样(Opaque Downsampling)
- 地形洞(Terrain Holes)
##### 质量
- HDR(高动态范围)
- MSAA(多采样抗锯齿)
- 渲染缩放(Render Scale)
##### 照明
- 主光照(Main Light)
- 投射阴影(Cast Shadow)
- 阴影分辨率(Shadow Resolution)
- 额外光照(Additional Lights)
- 每个对象的限制(Per Object Limit)
##### 阴影
- 距离(Distance)
- 层级(Cascades)
- 软阴影(Soft Shadows)
##### 后处理
- 分级模式(Grading Mode)
- 查找纹理大小(LUT Size)[LUT:look-up texture]
##### 高级
##### 自适应性能

#### 渲染(Rendering)
使用通用渲染器或2D渲染器
使用配套的着色器的着色模型，并使用相机定义渲染范围
使用URP资源定义渲染管线的选项和自定义设置

![渲染循环图示](https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@8.2/manual/Images/Graphics/Rendering_Flowchart.png '渲染循环')
#### 光照(Lighting)
- 直接光照和间接光照
- 实时光照和烘焙光照(两者同时使用称为混合光照)
- 全局光照(对直接和间接光照进行建模以提供逼真光照效果的一组技术)
##### 光源(Light sources)
###### 光源组件
(根据不同的渲染管线有不同的，有不同的属性。)
#### 着色器和材质(Shader & Material)
##### 常用着色器
- 复杂光照 (Complex Lit)

- 光照 (Lit)
- 简单光照 (Simple Lit)
- 烘焙光照 (Baked Lit)
- 无光照 (Unlit)

- 地形光照 (Terrain Lit)

- 粒子光照 (Particles Lit)
- 粒子简单光照 (Particles Simple Lit)
- 粒子无光照 (Particles Unlit)

- SpeedTree
- 贴花 (Decal)
- Autodesk 交互 (Autodesk Interactive)
- Autodesk 交互透明 (Autodesk Interactive Transparent)
- Autodesk 交互遮罩 (Autodesk Interactive Masked)

### Shader Graph