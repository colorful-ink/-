## 简介
一个由 @凉鞋的笔记 开发的Unity游戏框架
[b站视频链接](https://www.bilibili.com/video/BV1tV4y157k1/?spm_id_from=333.788&vd_source=89cdbc52fca0b3fc1318b88e36456c76)
[官网链接](https://qframework.cn/qf)

## 理解
- MVC模型
- 通过命令模式来独立出实现响应类逻辑功能的代码
- 通过事件机制来决定何时更新View层
- 通过IOC(Inversion of Control)容器，实现对类的实例的管理，保证一个类指对应一个实例，并且可被灵活访问，用于代替单例模式。即在IOC容器中注册类来实例化一个类，并保证这个类在IOC容器中只能有一个实例。
- 规则类逻辑适合写在一个对象里(继承自System类的类)
- Command负责数据的 增 删 改，Query 负责数据的 查

#