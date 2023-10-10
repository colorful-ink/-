## 三大Shader编程语言（CG/HLSL/GLSL）与 Unity的ShaderLab
### 建议学习HLSL和ShaderLab，因为CG已淘汰，Unity可将HLSL交叉编译为优化的GLSL。
在开发过程中，通常首先会通过 ShaderLab 来创建和调试着色器，利用其简洁和可视化的特性来实现各种渲染效果。如果你对 ShaderLab 的抽象程度不够满意，或者需要更精确和底层的控制，那么可以在 ShaderLab 中嵌入 HLSL 代码来扩展其功能。

## [HLSL](https://learn.microsoft.com/zh-cn/windows/win32/direct3dhlsl/dx-graphics-hlsl-reference)
### 语法
#### 变量
`[Storage_Class] [Type_Modifier] Type Name[Index] [: Semantic] [: Packoffset] [: Register]; [Annotations] [= Initial_Value]`
类似C语言，但是支持额外的数据类型
**额外的类型:**
- 矢量：例：float3，float4，vector<float,4>
- 矩阵 例： int1x1, float3x3 , matrix matrix <float, 2, 2> fMatrix = { 0.0f, 0.1, 2.1f, 2.2f };
- 
- 采样器 : DX版本不同有差异 DX10：
- 纹理
- 缓冲区
- 结构
- 用户自定义
- 数组
- state对象

####