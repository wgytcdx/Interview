<!--
 * @Author: your name
 * @Date: 2021-01-05 11:07:35
 * @LastEditTime: 2021-04-26 14:20:37
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /Interview Files/大纲/09-webpack.md
-->
#### 1. 什么是webpack

webpack是JavaScript应用程序的静态打包器. 当webpack处理应用程序的时候, 它会递归构建一个依赖关系图, 其中包含应用程序需要的每个模块, 然后将所有这些模块打包成一个或多个bundle. 

#### 2. webpack的4个核心概念

- 入口(entry)
- 输出(output)
- loader
- 插件(plugin)

#### 3. 如何利用 webpack 来优化前端性能

1. 优化一: vue-router路由懒加载

懒加载：也叫延迟加载，即在需要的时候进行加载，随用随载。

2. 打包后的js过大，将js打包多个文件

由于webpack打包后的js过大，以至于在加载资源时间过长。所以将文件打包成多个js文件，在需要的时候按需加载。

3. 去掉不必要的插件

4. 服务器缓存

参考:https://www.cnblogs.com/ssh-007/p/7944491.html

#### 4. webpack的简单原理

- 初始化参数：从配置文件和 Shell 语句中读取与合并参数，得出最终的参数；
- 开始编译：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译；
- 确定入口：根据配置中的 entry 找出所有的入口文件；
- 编译模块：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理；
- 完成模块编译：在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系；
- 输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会；
- 输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统。

#### 5. webpack、gulp、grunt的区别

三者都是前端构建工具, grunt和gulp在早期比较流行, 现在webpack比较主流, 不过一些轻量化任务还是会用gulp处理, 比如单独打包css文件等.

- grunt和gulp都是基于任务和流(Task, Stream)的, 类似jQuery, 找到一个(一类)文件, 对其做一系列链式操作, 更新流上的数据, 整条链式操作构成了一个任务, 多个任务就构成了整个web的构建流程.

- webpack是基于入口的. webpack会自动递归解析入口所需要加载的所有资源文件, 然后用不同的Loader来处理不同的文件, 用Plugin来扩展webpack功能. 













































