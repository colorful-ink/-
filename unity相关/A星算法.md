```C#
using System.Collections.Generic;
using UnityEngine;

// A*节点类，包含格子坐标和节点的启发值
public class AStarNode
{
    public int x;
    public int y;
    public float f;

    public AStarNode(int x, int y, float f)
    {
        this.x = x;
        this.y = y;
        this.f = f;
    }
}

// A*搜索
public class AStarSearch
{
    private int[,] map;
    private int width;
    private int height;
    private List<AStarNode> openList;
    private List<AStarNode> closeList;

    public AStarSearch(int[,] map)
    {
        this.map = map;
        this.width = map.GetLength(0);
        this.height = map.GetLength(1);
    }

    // 计算启发值
    private float Heuristic(int x1, int y1, int x2, int y2)
    {
        return Mathf.Sqrt(Mathf.Pow(x1 - x2, 2) + Mathf.Pow(y1 - y2, 2));
    }

    // 检查节点是否在地图中并且可以通过
    private bool IsValidNode(int x, int y)
    {
        return x >= 0 && x < width && y >= 0 && y < height && map[x, y] == 0;
    }

    // 检查节点是否已经在开启列表或者关闭列表中
    private bool IsInList(List<AStarNode> list, int x, int y)
    {
        foreach (AStarNode node in list)
        {
            if (node.x == x && node.y == y)
            {
                return true;
            }
        }
        return false;
    }

    // 取得下一步可以移动到的节点
    private List<AStarNode> GetNeighbors(AStarNode node)
    {
        List<AStarNode> neighbors = new List<AStarNode>();
        for (int x = node.x - 1; x <= node.x + 1; ++x)
        {
            for (int y = node.y - 1; y <= node.y + 1; ++y)
            {
                if (IsValidNode(x, y) && (x == node.x || y == node.y))
                {
                    float f = node.f + Heuristic(node.x, node.y, x, y);
                    neighbors.Add(new AStarNode(x, y, f));
                }
            }
        }
        return neighbors;
    }

    // 查找路径
    public List<Vector2Int> FindPath(Vector2Int start, Vector2Int end)
    {
        // 初始化开启列表和关闭列表
        openList = new List<AStarNode>();
        closeList = new List<AStarNode>();

        // 把起点放入开启列表
        AStarNode startNode = new AStarNode(start.x, start.y, 0);
        openList.Add(startNode);

        // 搜索直到找到目标或者没有可以搜索的节点
        while (openList.Count > 0)
        {
            // 取出开启列表中启发值最小的节点
            AStarNode currentNode = openList[0];
            foreach (AStarNode node in openList)
            {
                if (node.f < currentNode.f)
                {
                    currentNode = node;
                }
            }

            // 把当前节点从开启列表中移除，放入关闭列表
            openList.Remove(currentNode);
            closeList.Add(currentNode);

            if (currentNode.x == end.x && currentNode.y == end.y)
            {
                // 找到路径
                List<Vector2Int> path = new List<Vector2Int>();
                while (currentNode.x != start.x || currentNode.y != start.y)
                {
                    path.Add(new Vector2Int(currentNode.x, currentNode.y));
                    foreach (AStarNode node in closeList)
                    {
                        if (node.x == currentNode.x && node.y == currentNode.y)
                        {
                            currentNode = node;
                            break;
                        }
                    }
                }
                path.Reverse();
                return path;
            }

            // 取得当前节点可以到达的邻居节点
            List<AStarNode> neighbors = GetNeighbors(currentNode);

            foreach (AStarNode node in neighbors)
            {
                // 如果邻居节点已经在关闭列表中，则跳过
                if (IsInList(closeList, node.x, node.y))
                {
                    continue;
                }

                // 计算邻居节点的启发值
                node.f += Heuristic(node.x, node.y, end.x, end.y);

                // 如果邻居节点不在开启列表中，则加入列表
                if (!IsInList(openList, node.x, node.y))
{
openList.Add(node);
}
// 否则更新邻居节点的启发值
else
{
foreach (AStarNode openNode in openList)
{
if (openNode.x == node.x && openNode.y == node.y && openNode.f > node.f)
{
openNode.f = node.f;
break;
}
}
}
}
}
    // 没有找到路径
    return null;
}
}
```


使用方法：

1. 定义一个二维整数数组表示地图，其中 0 表示可以通行，1 表示不可通行。
2. 创建一个 `AStarSearch` 类的对象。
3. 调用 `FindPath` 方法，传入起点和终点坐标，得到一条从起点到终点的可行路径。如果找不到路径，返回 null。

示例代码：

```c#
int[,] map = new int[,] {
    {0, 0, 0, 0, 0},
    {0, 1, 1, 0, 0},
    {0, 0, 0, 0, 0},
    {0, 0, 1, 1, 0},
    {0, 0, 0, 0, 0}
};

AStarSearch search = new AStarSearch(map);
List<Vector2Int> path = search.FindPath(new Vector2Int(0, 0), new Vector2Int(4, 4));
if (path != null)
{
    foreach (Vector2Int pos in path)
    {
        Debug.Log(pos.x.ToString() + "," + pos.y.ToString());
    }
}
else
{
    Debug.Log("Path not found.");
}
```