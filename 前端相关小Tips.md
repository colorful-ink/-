#vue打包后的index.html为什么打不开？
```java
dist文件是需要放在服务器上运行的，资源默认放在根目录下。index.html中css和js文件的引用路径是绝对路径。
```