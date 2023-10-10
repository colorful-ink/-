### 迭代器

```C#
foreach(var i in collection){

}
//上述语句基于迭代器实现，即基于IEnumerable<T>(代表可枚举的集合，即可枚举类需实现该接口,其中的GetEnumerator方法返回一个IEnumerator对象) 和 IEnumerator<T>(代表集合的迭代器，其中)泛型接口，或非泛型接口IEnumerable和 IEnumerator。
```

##### 创建迭代器

返回类型是上述类型之一的函数，并在函数体中使用 yied return 语句。在同一方法中不能同时使用 return 语句和 yield return 语句。

##### 使用迭代器

#### Unity 中的协程(Coroutine)即是基于迭代器实现的

协程是使用 IEnumerator 返回类型并使用 yield return 语句声明的方法。
使用`yield return null;`或者`yield return 0；`的时候，会在下一帧继续执行；
返回的每个 IEnumerator 类型变量，可作为参数传入 StarCoroutine 方法中开启协程，并且每个 Ienumerator 变量对应一个上下文，即对应一个协程。

### 委托

##### 1.定义委托类型

委托类型`myMethodDelegate`，可以认为是一个 _指定了返回值、参数的方法 的变量_ 的类型

`public delegate string myMethodDelegate( int myInt );`

##### 2.定义委托对象

委托对象`myD`：指定了返回值、参数的方法 的变量

`myMethodDelegate myD;`

##### 3.绑定委托对象

```C#
public string myStringMethod ( int myInt )  {
         if ( myInt > 0 )
            return( "positive" );
         if ( myInt < 0 )
            return( "negative" );
         return ( "zero" );
      }

myD += myStringMethod;
```

##### 4.调用委托

`myD(1);`

#### 通用的预定义的委托类型

C#中预先定义的 指定了返回值、参数的方法 的变量 的模板

##### Func<Tresult>、Func<T1,Tresult>······Func<T1...T16,TResult>

#### C#内置的 event 和 Action<>————对委托的封装

`using System`

##### 1.创建事件

`public event Action<[T1[,T2]]> eventName`

##### 2.绑定(订阅)事件

`eventName += Fun`

##### 3.触发事件

`eventName?.Invoke([T1[,T2]])` || `eventName([T1[,T2]])`

##### 额外说明

- 对于事件 (event Action<T, T> variableChanged)，只有在定义该事件的类内部才能使用 += 添加事件处理程序或者 -= 移除事件处理程序。在外部代码中，只能使用 += 添加事件处理程序，而无法使用 -= 移除。

- 对于普通委托 (Action<T, T> variableChanged)，可以在任何地方使用 += 添加委托方法或者 -= 移除委托方法。

#### UnityAction & UnityEvent（unity 中对 c#委托、事件的封装）

**UnityEvent 只能在继承自 MonoBehaviour 的类中使用，不适用于普通的 C#类。**
所以在非 MonoBehaviour 派生类中，请使用 C#内置事件封装；
`using UnityEngine.Events`

##### 0.自定义事件类(默认 UnityEvent 是不传递参数的)

`public class CustomEvent : UnityEvent<[T1[,...T4]]> {}`

##### 1.创建事件实例

`UnityEvent eventName` || `CustomEvent eventName`

##### 2.绑定(订阅)事件

`在unity编辑器中通过Inspector窗口绑定`

##### 3.触发事件

`eventName.Invoke()` || `eventName.Invoke([T1[,...T4]])`

### 特性(attribute) 和 反射(reflection)

#### [特性](https://learn.microsoft.com/zh-cn/dotnet/csharp/advanced-topics/reflection-and-attributes/attribute-tutorial)

**以声明的方式将信息与代码相关联，可应用于类、结构、方法、构造函数等**
只能向特性构造函数传递以下简单类型/文本类型参数：bool, int, double, string, Type, enums, etc 和这些类型的数组。 不能使用表达式或变量。 可以使用任何位置参数或已命名参数。

##### 自定义特性

自定义特性只作元数据之用，并不会在执行的特性类中生成任何代码。（可配合反射机制进行一些检测）

```C#
//[AttributeUsage(AttributeTargets.Class | AttributeTargets.Struct)] 用于限制特性的应用范围
public class MySpecialAttribute : Attribute
{
}
```

##### 反射机制获取特性信息

##### 一些系统特性

`using System;`

- [Flags]: 将枚举视为位域（即一组标志,类似0001,0010,0100...）;使得枚举值之间可以有组合

```C#
   [Flags]
   enum MultiHue : short
   {
      //None = 0,
      Black = 1,
      Red = 2,
      Green = 4,
      Blue = 8
   };
   // 0 = 0b...0000 = None
   // 1 = 0b...0001 = Black
   // 2 = 0b...0010 = Red
   // 3 = 0b...0011 = Red | Black
   // ......
```

##### 一些 Unity 特性

###### 引擎

`Using UnityEngine`

- `[RuntimeInitializeOnLoadMethod[(loadType)]]`: 游戏加载后，将调用标记的方法。这是在调用 Awake 方法后进行的。

###### 编辑器

`using UnityEditor`

- `[CustomEditor]`:告知编辑器类该编辑器所针对的运行时类型。当您为组件创建自定义编辑器时，您需要将此属性添加到编辑器类上

### 单例模板

```C#
public interface ISingleton<T> where T : class
{
    T Instance { get; }
}

public class Singleton<T> : ISingleton<T> where T : class, new()
{
    private static readonly T instance = new T();

    protected Singleton()
    {
        // 私有构造函数
    }

    public static T Instance
    {
        get { return instance; }
    }
}
```

### C#中的异步模式

一般情况下，在涉及与磁盘或网络交互时，使用异步操作是较为常见的做法。这是因为读写磁盘或进行网络通信都可能涉及到较长的延迟和不确定的响应时间。

#### [基于任务（Task）](https://learn.microsoft.com/zh-cn/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)

基于 `System.Threading.Tasks` 命名空间中的 Task 和 Task<TResult> 类型，这些类型用于表示异步操作。异步方法的返回类型只能是 void、Task 或 Task<TResult>（具有 GetAwaiter 方法的任何其他类型）。
!!! 异步方法也可以使用 void 作为返回类型，但是不能在调用时使用 await。这种返回类型主要用于定义事件处理程序，因为事件处理程序要求返回类型为 void。异步事件处理程序通常用作异步程序的起始点。void 返回值一个作用：在 void 异步方法中包含一个 await Task 异步方法，从而能免除警告。
Task<TResult>类型变量，是立即返回的。 `Task<string> getStringTask = GetStringSync();`
但是在获取 Tresult 类型返回值时，是需要等待的。 `string contents = await getStringTask;`

### [类型测试运算符](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/type-testing-and-cast)

#### is

语法: `E is T`
(E 是返回一个值的表达式，T 是类型或类型参数的名称。 E 不得为匿名方法或 Lambda 表达式。)
返回值：

> `true`:
>
> > **E 运行时类型为 T** >> **表达式结果的运行时类型派生自类型 T、实现接口 T，或者存在从其到 T 的另一种隐式引用转换。(不会考虑用户定义的转换)**
> > 表达式结果的运行时类型是基础类型为 T 且 Nullable<T>.HasValue 为 true 的可为空值类型。
> > 存在从表达式结果的运行时类型到类型 T 的装箱或取消装箱转换。(不会考虑数值转换)
> > `false`:
> > 其他情况

#### as

as 运算符将表达式结果显式转换为给定的引用或可以为 null 值的类型。 如果无法进行转换，则 as 运算符返回 null。 与强制转换表达式 不同，as 运算符永远不会引发异常。
语法: `E as T`
常用：`E is T ? (T)(E) : (T)null`
as 运算符仅考虑引用、可以为 null、装箱和取消装箱转换。 不能使用 as 运算符执行用户定义的转换。 为此，请使用强制转换表达式。

#### 强制类型转换

语法: `(T)E`

#### typeof

语法：`typeof(T)`
返回获取编译时类类型的 System.Type 实例

#### (额外)GetType()

语法：`E.GetType()`
返回实例的运行时类型 System.Type 实例

#### nameof

语法：`nameof(E)`
用于获取一个标识符（变量、属性、方法等）的名称作为一个字符串。 只能用于编译时期可访问的标识符。

#### sizeof

`unsafe{}`
语法：`sizeof(T)`
返回类型所占内存的字节数

### 反射机制

`using System.Reflection;`

#### 常用反射类

##### Type

表示类型的类，可以获取类型的信息，如名称、命名空间、成员等。

##### MethodInfo

表示方法的类，可以获取方法的信息，并在运行时调用方法。

##### FieldInfo

表示字段的类，可以获取字段的信息，并在运行时访问和修改字段的值。

##### PropertyInfo

表示属性的类，可以获取属性的信息，并在运行时访问和修改属性的值。

### 元组

用例：

```C#
(double, int) t1 = (4.5, 3);
Console.WriteLine($"Tuple with elements {t1.Item1} and {t1.Item2}.");
// Output:
// Tuple with elements 4.5 and 3.

(double Sum, int Count) t2 = (4.5, 3);
Console.WriteLine($"Sum of {t2.Count} elements is {t2.Sum}.");
// Output:
// Sum of 3 elements is 4.5.

var t = (Sum: 4.5, Count: 3);
Console.WriteLine($"Sum of {t.Count} elements is {t.Sum}.");

//元组的析构
var t = ("post office", 3.6);
var (destination, distance) = t; | (string destination, double distance) = t;
Console.WriteLine($"Distance to {destination} is {distance} kilometers.");
// Output:
// Distance to post office is 3.6 kilometers.
```

### 扩展方法


### 定义属性的常用写法