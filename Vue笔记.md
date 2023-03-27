## Vue@2
#### 使用VueCli项目文件夹的参考结构
>build
>>index.js [webpack配置文件]
>
>mock [存储模拟数据，mockjs实现]
>mode_modules [项目依赖文件夹]
>public [经常防止一些静态文件，并且编译时不会被打包]
>>index.html
>
>src
>>api [涉及请求相关,封装与后端交互的函数]
>>assets [放置共享的静态文件，会被编译打包]
>>components [放置非路由组件、全局组件]
>>icons [svg矢量图、icons]
>>layout [放置布局相关组件]
>>>Header
>>>Footer
>>
>>router [放置路由组件]
>>store [Vuex相关js]
>>style [样式相关]
>>utils [自定义工具包,工具性js]
>>views/pages [放置路由组件]
## Vue@3