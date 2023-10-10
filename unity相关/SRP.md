自定义渲染管线

# URP

`using UnityEngine.Rendering.Universal;`

### 三大画面效果利器：Shader（Material）、RendererFeature（RendererData）、Post-Processing（Volume&Camera）
#### Shader
- 使用ShaderGraph或者ShaderLab、HSLS编写
- 控制特定材质的几何体的单个顶点的渲染行为
#### RendererFeature
- 使用C#，继承ScriptableRendererFeature类编写，需要使用继承自ScriptableRenderPass的类编写通道。
- 控制所有的顶点着色器或者片元着色器的渲染行为(干涉整体行为)
- 可以将场景信息传递给shader（使用ShaderData）
#### Post-Processing
- 直接使用预定义的后处理效果
- 官方给出的常用的对RendererFeature的实现

## RendererFeature
基于CommandBuffer（https://docs.unity.cn/cn/2021.3/Manual/srp-creating-simple-render-loop.html）

### 步骤

1. 创建继承自 ScriptableRendererFeature 的 CustomRendererFeature；
2. (一般在 CustomRendererFeature 类中)声明并实例化继承自 ScriptableRenderPass 的 CustomPass 类（在该类内部实现具体操作）；(通过构造函数来实现实例化中初始化的过程)
3. 将实例化的 CustomPass 类添加进渲染器队列中(renderer.EnqueuePass(CustomPass 实例))。
4. 在 URP Data 资源上，将自定义的 RendererFeature 添加进游戏中。

### [ScriptableRendererFeature 类中的方法说明](https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@16.0/api/UnityEngine.Rendering.Universal.ScriptableRendererFeature.html#methods)

- `public override void Create() {}`

  一般用于 Pass 的实例化。在第一次加载、enable 或 disable 这个 rendererFeature、改变 Inspector 窗口的属性时调用。
  举例：

  ```C#
  public override void Create()
  {
    createdMaterial = CoreUtils.CreateEngineMaterial(shader);
    customPass = new CustomRenderPass();
  }
  ```

- `piblic override void AddRenderPasses(ScriptableRenderer renderer, ref RenderingData renderingData) {}`

  renderer：用于将实例化的 Pass 添加进渲染队列：renderer.EnqueuePass(CustomPass 实例);
  配置 Pass（可根据 RenderingData 来配置不同条件下的 Pass）

- `protected override void Dispose(bool disposing) {}`

  实现关闭特性时销毁相关资源，举例：

  ```C#
  protected override void Dispose(bool disposing)
  {
    CoreUtils.Destroy(createdMaterial);
  }
  ```

### [ScriptableRenderPass 类中的常用方法、属性说明](https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@16.0/api/UnityEngine.Rendering.Universal.ScriptableRenderPass.html)

#### methods

- **`public override void Execute(ScriptableRenderContext context, ref RenderingData renderingData) {}`**
  定义这个 Pass 做什么，什么时候执行。每帧在渲染事件发生时调用

  参数：

  > [context](https://docs.unity.cn/cn/2021.3/ScriptReference/Rendering.ScriptableRenderContext.html): 用作 C# 渲染管线代码与 Unity 的低级图形代码之间的接口。用于构建渲染命令列表。<a href= "#contex">相关</a>
  >>

  > [renderingData](https://docs.unity.cn/Packages/com.unity.render-pipelines.universal@14.0/api/UnityEngine.Rendering.Universal.RenderingData.html):
  >> 常用：
  >> `renderingData.cameraData.camera`
  >> `renderingData.lightData.visibleLights`

#### properties

- `renderPassEvent`
  指定通道在哪个阶段执行，例：`renderPassEvent = RenderPassEvent.BeforeRenferingPosrProcessing；`

### 在渲染通道的执行函数中，常用方法

#### ScriptableRenderContext 类  <p id = "contex"></p>
ScriptableRenderContext 类用作 C# 渲染管线代码与 Unity 的低级图形代码之间的接口。
##### [CommandBuffer](https://docs.unity.cn/cn/2021.3/ScriptReference/Rendering.CommandBuffer.html) 
###### 使用CommandBuffer
public override void Execute(ScriptableRenderContext context, ref RenderingData renderingData)
{

1. `CommandBuffer cmd = CommandBufferPool.Get(name: "BuffName");`
   创建新的命令缓冲区（因为命令是在之后按顺序执行的，所以需要先保存在缓冲区中）
2. 在该命令缓冲区中添加渲染命令
   比如：`cmd.DrawMesh(_mesh, Matrix4x4.identity, _material);`
3. `context.ExecuteCommandBuffer(cmd);`
   使用 ScriptableRenderContext.ExecuteCommandBuffer 将 CommandBuffers 传递到 ScriptableRenderContext
4. `CommandBufferPool.Release(cmd);`
   在执行完成后，将缓冲区池中的该命令占用空间释放。

}

##### 使用contex直接API调用
[参考:在可编程渲染管线中调度和执行渲染命令](https://docs.unity.cn/cn/2021.3/Manual/srp-using-scriptable-render-context.html)

#### RendererData类



### 与 Shader 的交互
#### 通过CommandBuffer设置全局属性（cmd.SetGlobal~()）
#### 通过Shader设置全局属性（Shader.SetGlobal~()）
#### 通过Material获取、设置单一属性（material.Get~()，material.Set~()）


### [CommadBuffer 类实例 `cmd` 中常用的方法](https://docs.unity.cn/cn/2021.3/ScriptReference/Rendering.CommandBuffer.html)

#### 在脚本中通过 shader 创建材质

`Material CoreUtils.CreateEngineMaterial(Shader shader);`


### 渲染过程中可能涉及的纹理以及获取方法

*在Universal Render Pipeline (URP) 中，你可以使用ScriptableRenderContext结构中的BeginRenderPass和EndRenderPass方法，通过定义渲染目标纹理来获取和使用这些渲染相关纹理*

cmd.getTemporaryRT && cmd.ReleaseTemporaryRT

- 屏幕纹理
- 深度纹理
- GBuffer纹理
- 法线纹理
- 光照纹理
- 阴影纹理
- 渲染目标纹理

<h4 id="appendix_01">附录一 官方案例</h4>

```C#
using UnityEngine;
using UnityEngine.Rendering;
using UnityEngine.Rendering.Universal;

public class LensFlareRendererFeature : ScriptableRendererFeature
{
    class LensFlarePass : ScriptableRenderPass
    {
        private Material _material;
        private Mesh _mesh;

        public LensFlarePass(Material material, Mesh mesh)
        {
            _material = material;
            _mesh = mesh;
        }

        public override void Execute(ScriptableRenderContext context,
            ref RenderingData renderingData)
        {
            CommandBuffer cmd = CommandBufferPool.Get(name: "LensFlarePass");
            // Get the Camera data from the renderingData argument.
            Camera camera = renderingData.cameraData.camera;
            // Set the projection matrix so that Unity draws the quad in screen space.
            cmd.SetViewProjectionMatrices(Matrix4x4.identity, Matrix4x4.identity);
            // Add the scale variable, use the Camera aspect ratio for the y coordinate
            Vector3 scale = new Vector3(1, camera.aspect, 1);

            // Draw a quad for each Light, at the screen space position of the Light.
            foreach (VisibleLight visibleLight in renderingData.lightData.visibleLights)
            {
                Light light = visibleLight.light;
                // Convert the position of each Light from world to viewport point.
                Vector3 position =
                    camera.WorldToViewportPoint(light.transform.position) * 2 - Vector3.one;
                // Set the z coordinate of the quads to 0 so that Uniy draws them on the same
                // plane.
                position.z = 0;
                // Change the Matrix4x4 argument in the cmd.DrawMesh method to use
                // the position and the scale variables.
                cmd.DrawMesh(_mesh, Matrix4x4.TRS(position, Quaternion.identity, scale),
                    _material, 0, 0);
            }
            context.ExecuteCommandBuffer(cmd);
            CommandBufferPool.Release(cmd);
        }
    }

    private LensFlarePass _lensFlarePass;
    public Material material;
    public Mesh mesh;

    public override void Create()
    {
        _lensFlarePass = new LensFlarePass(material, mesh);
        // Draw the lens flare effect after the skybox.
        _lensFlarePass.renderPassEvent = RenderPassEvent.AfterRenderingSkybox;
    }

    public override void AddRenderPasses(ScriptableRenderer renderer, ref RenderingData renderingData)
    {
        if (material != null && mesh != null)
        {
            renderer.EnqueuePass(_lensFlarePass);
        }
    }
}
```

<h4 id="appendix_02">附录二 URP无光照Shader模板</h4>

```ShaderLab
// This shader fills the mesh shape with a color predefined in the code.
Shader "Example/URPUnlitShaderBasic"
{
    // The properties block of the Unity shader. In this example this block is empty
    // because the output color is predefined in the fragment shader code.
    Properties
    { }

    // The SubShader block containing the Shader code.
    SubShader
    {
        // SubShader Tags define when and under which conditions a SubShader block or
        // a pass is executed.
        Tags { "RenderType" = "Opaque" "RenderPipeline" = "UniversalPipeline" }

        Pass
        {
            // The HLSL code block. Unity SRP uses the HLSL language.
            HLSLPROGRAM
            // This line defines the name of the vertex shader.
            #pragma vertex vert
            // This line defines the name of the fragment shader.
            #pragma fragment frag

            // The Core.hlsl file contains definitions of frequently used HLSL
            // macros and functions, and also contains #include references to other
            // HLSL files (for example, Common.hlsl, SpaceTransforms.hlsl, etc.).
            #include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Core.hlsl"

            // The structure definition defines which variables it contains.
            // This example uses the Attributes structure as an input structure in
            // the vertex shader.
            struct Attributes
            {
                // The positionOS variable contains the vertex positions in object
                // space.
                float4 positionOS   : POSITION;
            };

            struct Varyings
            {
                // The positions in this struct must have the SV_POSITION semantic.
                float4 positionHCS  : SV_POSITION;
            };

            // The vertex shader definition with properties defined in the Varyings
            // structure. The type of the vert function must match the type (struct)
            // that it returns.
            Varyings vert(Attributes IN)
            {
                // Declaring the output object (OUT) with the Varyings struct.
                Varyings OUT;
                // The TransformObjectToHClip function transforms vertex positions
                // from object space to homogenous clip space.
                OUT.positionHCS = TransformObjectToHClip(IN.positionOS.xyz);
                // Returning the output.
                return OUT;
            }

            // The fragment shader definition.
            half4 frag() : SV_Target
            {
                // Defining the color variable and returning it.
                half4 customColor = half4(0.5, 0, 0, 1);
                return customColor;
            }
            ENDHLSL
        }
    }
}
```
