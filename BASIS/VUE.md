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
