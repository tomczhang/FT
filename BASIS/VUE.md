# 1.双向绑定
* 发布/订阅模式
* 脏数据检测
* 数据绑定+发布/订阅模式

# 2.生命周期
* beforeCreated
* created
* beforeMounted
* mounted
* beforeUpdated
* updated
* beforeDestory
* destoryed

# 3. vueRouter的原理
前端路由分为Hash和History两种，Hash是在浏览url后面添加#和字符串，通过调用hashchange事件来完成前端路由，而History是通过window.history的pushState和replaceState API，来完成前端路由。

VueRouter既可以使用Hash的方式，也可以使用History的方式，其中Hash的方式是默认的。VueRouter是一个组件，它包括三个部分，router核心，router-link和router-view。
具体的原理是VueRouter把自己作为一个插件，在新建Vue实例的时候安装到了Vue上，然后通过Vue.mixin方法注册了一个beforeCreated的钩子，在之后所有Vue组件调用时都会使用这个Vue钩子函数，初始化router并定义相应的_route对象。当前端路由改变的时候，会根据地址匹配，得到一个对应的route, 然后将其赋值到vue._route上，因为在初始化的时候_route对象已经被Vue劫持了，所以Vue就能拿到路由变化的状态，从而触发Vue组件的渲染更新流程。具体的流程是取出当前的路由对象下对应的组件，然后交给router-view进行渲染。所以整个流程就是路由变化->匹配路由->设置vm._route的值->router-view开始渲染->找到合适的组件并渲染。

动态路由匹配、嵌套路由、编程式导航、命名视图、命名路由

# 4. 路由的算法
```
  <ul>
    <li><a href="#/">index</a></li>
    <li><a href="#/item">item</a></li>
    <li><a href="#/list">list</a></li>
  </ul>
  <br>
  <br>
  <div>头部</div>
  <h1 class='result'></h1>


    function Router() {
       // 路由储存
       this.routes = {};
       // 当前路由
       this.currentUrl = '';
    }
    Router.prototype = {
      // 路由处理
      route: function (path, callback) {
        this.routes[path] = callback || function(){};
      },
       // 页面刷新
      refresh: function () {
        // 当前的hash值
        this.currentUrl = location.hash.slice(1) || '/';
        // 执行hash值改变后相对应的回调函数
        this.routes[this.currentUrl]();
      },
      // 页面初始化
      init: function () {
        // 页面加载事件
        window.addEventListener('load', this.refresh.bind(this), false);
        // hash 值改变事件
        window.addEventListener('hashchange', this.refresh.bind(this), false);
      }
    }
   
    // 全局挂载
    window.Router = new Router();
    // 初始化
    window.Router.init();

    let obj = document.querySelector('.result');
    
    function changeConent (cnt) {
      obj.innerHTML = cnt
    }

    // 匹配路由做相应的操作
    Router.route('/', () => {
      changeConent("当前是首页");
    })

    Router.route('/item', () => {
      changeConent('当前是item页面');
    })

    Router.route('/list', () => {
      // ajax 的数据就可以这样去拼接
      setTimeout(() => {
        obj.innerHTML = '<h1 style="color: red">Hello World</h1>'
      }, 1000)
    })
    ```

# 5. Vue和React的区别
* 相同点：
  * 都有Virtual DOM
  * 都支持组件化，都支持API和文件两种组件声明方式
  * 都是数据驱动视图


不同点：
  * DOM diff的算法不一样
  * Vue是MVVM框架，双向绑定数据流，React是单向数据流，严格来说只针对MVC的V层
  * Vue有模版引擎，v-if, v-for
  * 生命周期不一样
  * 组件写法不一样，React JSX+inline js, Vue webpack+vue-loader

# 6. Nuxt
* Nuxt的优点：
  * 无需手动划分路由，它会根据pages文件夹下的目录生成路由
  * 无需考虑数据传输问题，会自动异步请求数据
  * 内置了webpack

* Nuxt的流程：
  * 实例化Nuxt()类
  * 实例对象会依次调用build, render, renderRoute, renderAndGetWindow, generate方法
  * build 运行generateRouteAndFiles方法，根据layout和page生成模版文件和路由配置，然后把解析好的内容放到.nuxt文件夹下; 运行buildFiles方法，生成dist文件
  * render render先处理一系列路径问题，然后调用renderRoute方法获得响应所需的内容并完成相应。
  * renderRoute 判断是否是服务器端渲染，是的化调用vue的bundleRender方法，将html内容渲染完毕再整体输出，如果不是的化，渲染一个div节点，交给客户端渲染
  * 最后使用renderAndGetWindow来判断生成的html是否正常，并发出通知

