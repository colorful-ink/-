[视频链接](https://www.bilibili.com/video/BV1rf4y1k7vE/?spm_id_from=333.788&vd_source=89cdbc52fca0b3fc1318b88e36456c76)

## 插件工具：

PolyBrush：3D 低模地形以及关卡设计的快捷编辑（类似于 unity 自带的 terrain）
ProBuilder：关卡原型、基础形状快速创建。
ProGrids：可配合 ProBuilder 使用,添加参考线以及移动吸附
Cinemachine:

## 管理类

### MouseManager

using UnityEngine;

public class SpriteOverlap : MonoBehaviour
{
public SpriteRenderer sprite1;
public SpriteRenderer sprite2;

    private void Update()
    {
        Texture2D texture1 = sprite1.sprite.texture;
        Texture2D texture2 = sprite2.sprite.texture;

        // 获取两个Sprite的边界框重叠部分的范围
        Bounds overlapBounds = sprite1.bounds.Intersect(sprite2.bounds);

        // 将边界框转换为纹理坐标
        Rect overlapRect = new Rect(
            (overlapBounds.min.x - sprite1.bounds.min.x) / sprite1.bounds.size.x * texture1.width,
            (overlapBounds.min.y - sprite1.bounds.min.y) / sprite1.bounds.size.y * texture1.height,
            overlapBounds.size.x / sprite1.bounds.size.x * texture1.width,
            overlapBounds.size.y / sprite1.bounds.size.y * texture1.height
        );

        // 获取重叠部分的非透明像素
        for (int x = (int)overlapRect.xMin; x < overlapRect.xMax; x++)
        {
            for (int y = (int)overlapRect.yMin; y < overlapRect.yMax; y++)
            {
                Color pixelColor1 = texture1.GetPixel(x, y);
                Color pixelColor2 = texture2.GetPixel(x, y);

                if (pixelColor1.a > 0 && pixelColor2.a > 0)
                {
                    Debug.Log("Non-transparent overlap pixel at: " + x + ", " + y);
                }
            }
        }
    }

}
