# 1. 事件循环
>事件循环：Js引擎用来协调事务的一种方式。

* 同一个上下文中，总的执行顺序为同步代码->微任务->宏任务
* 浏览器按照一个MacroTask任务，所有MicroTask的顺序运行，Node按照六个阶段的顺序运行，并在每个阶段后面都会运行MicroTask队列
* 同个MicroTask队列下process.tick()会优于Promise


> MacroTask: script(整体代码), setTimeout, setInterval, setImmediate（node独有）, I/O, UI rendering
MicroTask: process.nextTick（node独有）, Promises, Object.observe(废弃), MutationObserver

Node EventLoop的六个阶段：
timers：执行setTimeout() 和 setInterval()中到期的callback。
I/O callbacks：上一轮循环中有少数的I/Ocallback会被延迟到这一轮的这一阶段执行
idle, prepare：队列的移动，仅内部使用
poll：最为重要的阶段，执行I/O callback，在适当的条件下会阻塞在这个阶段
check：执行setImmediate的callback
close callbacks：执行close事件的callback，例如socket.on("close",func)

# 2. Express/koa/egg
Express由Application, Request, Response, Router组成，它是一个由路由和中间件组合而成的Web框架。一个Express应用就是在调用不同的框架，中间件之间是顺序执行的。
Koa由Application, Request, Response, Context组成，它比express更小，没有绑定任何中间件。它的中间件模型是一个洋葱圈模型，每个请求都会在一个中间件中执行两次，可以非常方便的实现后置处理逻辑
egg是基于Koa的，它的好处是约定优先，它规定了项目的结构和写法，是一个企业级的应用框架。另外，它提供了很多官方的插件，可以省却选择的苦恼。

# 3. Node的特点
* 异步IO
* 事件驱动
* 跨平台
* 单线程单进程
NodeJs的单线程是指只有一个JS引擎在主线程上运行，其他异步IO和事件相关的线程通过libuv来实现内部线程池和调度。libuv中存在EventLoop，通过EventLoop来实现多线程的效果。
进程是资源容器，线程是执行单位。Node默认是单进程的，这样不利于多核处理，所以使用child_process可以实现一个master主进程，多个worker子进程的多进程模式。cluster意为集成，一方面集成了child_process.fork创建node子进程的方式，一方面集成了创建多个子进程后，自动控制负载均衡的方式。

# 4. Node的应用场景
* IO密集型
* 长链接应用

# 5. XSS/CSRF
XSS: 跨站脚本攻击。往web页面中插入恶意的script代码，当用户浏览该页面时，嵌入其中的script代码会执行，从而达到恶意攻击用户的目的。先攻击含有漏洞的服务器，诱使受害者打开受到攻击的服务器Url，受害者在web中打开Url, 脚本运行。防御措施：html编码，过滤

CSRF：跨站请求伪造。攻击者诱使用户访问它的页面，在他的页面中，伪造了一个访问其他网站的请求，利用用户已经登陆的状态，从而达到模拟用户进行操作的行为。防御措施：referer, 每次提交时添加token




