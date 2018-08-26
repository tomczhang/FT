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
> webpack的原理
webpack打包出来的文件其实是立即执行函数，然后通过webpack自己定义的exports和require来实现模块化。对于代码分割，webpack使用jsonp的形式进行js异步加载。

# 3. babel
> 描述一下babel
* babel-core: ES6 Code
* babel-polyfill: ES6 API
* babel preset: 方案/提案
* bebel plugin:  提供一些单一功能

# 4. yarn/npm
> 描述包管理工具yarn/npm区别：
yarn有缓存，安全（每个安装包被执行前校验其完整性），可靠，lockfile来准确控制版本。
package-lock.json和package.json会有一系列问题。

