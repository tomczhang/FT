1. Webpack打包出文件的结构
2. vue减少vendor.js的大小
3. 模块化
5. babel
6. yarn/npm

# 1.前端模块化

> 描述一下前端模块化
前端复杂的代码环境，多个script之间互相污染全局变量，按需加载等需求，产生了前端模块化。
* CommonJS: 用同步require的方法加载依赖项。Nodejs和browserify都用的这种方法。
* AMD/CMD: CommonJS在浏览器的异步加载环境中有点问题，所以出现了异步加载模块。AMD提前执行，CMD延迟执行。requirejs和seajs。
* ES6 Module: 使用import/export来控制模块，思想是尽量静态化，在编译时就确定模块的依赖关系和输入输出, 实质不同于Commmonjs输出值的拷贝，变成了输出值的引用。<srcipt type="module">
* Webpack: 当前最流行的模块打包工具。
  * 将每个静态资源都当成一个模块，能处理所有的静态资源；
  * 能处理流行的模块规范
  * 可以异步加载，代码拆分
  * 带有插件系统，可以完成很多强大的功能

# 2. Webpack模块化原理

