### flex弹性布局笔记
- 元素不会在主轴方向被拉伸，但是可以被压缩。
- 元素会被在交叉轴(副轴)方向拉伸来填充交叉轴大小。
- 默认情况下，元素呈线形排列，并且把自己的大小作为主轴上的大小。
display：flex；的元素的子元素称为flex items,flex items具有flex属性
>flex: [flex-grow] [flex-shrink] [flex-basis]

### 关于页面布局的理解
body元素只是一个定义了width的block盒子，所以需要内容撑起他的高度，否则默认height为零
### 响应式布局的等比例缩放
#### 使用flex布局实现大盒子（zoom-box）及其内部元素的响应式等比例缩放：
- 如何控制元素响应式等比例缩放：
1. 通过body的width来定义zoom-box的宽度，使得zoom-box的宽度能不借助js的情况下，对窗口宽度的变化做出响应；
2. 自定义属性`--ratio`(css规范)或`data-ratio`(Vue规范)，来记录zoom-box的固定比例。
3. 通过js绑定`resize`窗口事件，使高度响应变化：`zoom-box.style.height = box.offsetWidth * ratio + 'px';`

- 如何控制元素及其内部元素响应式等比例缩放：
1. 在以上基础上，将zoom-box定义为弹性盒子(flex)
2. 定义多个弹性盒子(flex)column来划分zoom-box的纵向，从而形成一个可控制的二维结构。
3. 将内容放入一个个column中，通过flex属性来控制比例

***一定不要定义内部元素的大小，而是通过事先精确计算比例，调整columns、flex-items的flex值来调整实际比例效果***

#### 使用' transform: scale(); '实现响应式等比例缩放
- 何时进行缩放？缩放比例如何计算？
  - 这些问题难以拆分成简单的实现方法来解决。