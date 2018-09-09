# 1. 数据类型
> 数据类型的分类
* 基本数据类型：Undefined、Null、Boolean、Number、String、Symbol
* 引用数据类型：Object、Array、RegExp、Date、Function

> 基本数据类型与引用数据类型的区别
* 基本数据类型按值传递，引用数据类型按引用传递值。
* 基本数据类型会保存在栈内存中，引用数据类型会保存在堆内存里，而引用类型的引用地址是存在于栈内存中的。栈内存和堆内存的区分与垃圾回收机制有关，这样使得程序在运行时占用内存最小。
* 基本数据类型的创建使用了基本包装类型，这就是为什么能对字符串调用方法的原因。基本包装类型在创建后立刻被销毁。

> 判断数据类型的方法
* typeof：主要用于检测基本数据类型。判断引用类型时只能区分object, undefined和function
* instaneof：主要用于引用数据类型的检测。判断同一个作用域下，一个对象/类是否是另外一个对象/类的实例。会一直递归到最终原型。基本数据类型没有父类型，都会返回false
* Object.prototype上的原生toString()：可以用与检测基本和引用数据类型。任何值上调用Object原生的toString方法，都会返回一个[object NativeConstructorName]格式的字符串。通过这种方法来判断变量的数据类型，例如`Object.prototype.toString.call(undefined);//”[object Undefined]”`

> undefined/null的异同
* Undefined表示一个变量最原始的状态，没有赋值的变量都是undefined。
* Null表示一个对象被人为重置为空对象。所以`typeof null = 'object'`但是它本身并不是以Object为原型建立的，所以`null instanceof Object`是false。
* 但是他们如果转换为boolean类型则都为false

# 2.对象拷贝

> 深浅拷贝的定义及实现方案

深/浅拷贝：引用类型是否使用同一个内存地址。

* 浅拷贝：包含拷贝原对象的引用和拷贝原对象的实例两种情况。
  * 拷贝原对象的引用：简单赋值
  * 拷贝原对象的实例：Object.assign(), [].concat(), [].slice()

* 深拷贝：
  * JSON.stringify/parse互转
  简单方便，问题是会修改constustor并且不会转换key为undefined的值，另外Map, Set, RegExp, Date, ArrayBuffer和Function的值都会丢失
  * JQuery extends
  不能解决循环引用，效率中等
  * lodash cloneDeep
  能解决循环引用，效率最高

参考文章：https://segmentfault.com/a/1190000002801042

# 3. 作用域

> 声明提升/函数提升

* 声明提升：在JavaScript中，函数声明与变量声明经常被JavaScript引擎隐式地提升到当前作用域的顶部。注意，赋值并不会被提升。
* 函数提升：
函数提升的优先级高于变量提升，且只有`function func1(){};`这样声明的函数才会有提升。

> var/let/const的区别：
* var：函数作用域。有变量提升，只能声明的变量可以提升，初始化的变量不会。
* let：块级作用域，声明变量，暂时性死区，变量不会提升。相同作用域不能重复定义，全局定义不会挂在window上。babel会用var来模拟实现let，从而导致会提升的错觉。let作为循环变量，每次都是一个新的变量。
* const：块级作用域，声明常量，不能改变，声明时必须初始化。相同作用域不能重复定义。不允许修改绑定，但是允许修改值，所以const创建对象之后可以修改该对象的值。const应该用在for...in/of循环上。


> 作用域链

当前作用域没有找到属性和方法，会向上层作用域中查找，直至全局函数，这种形式就是作用域链。

# 4. 函数
> 函数名词的定义

* 闭包：函数可以访问其声明时所在作用域内的变量，从而突破作用域的限制。
原理：原理和js的变量解析机制有关，js解析变量会沿着作用域链层层向上查找。每次调用一个函数时会创建相应的执行上下文，和作用链，作用链会赋值给内部属性[[scope]]，这样内层函数就可以拿到外层函数作用域的变量，即使外层函数的执行上下文已经被销毁，比如调用外层函数，返回内层函数，也可以通过内层函数拿到外层函数作用域的变量。
* this：调用函数的对象
* arguments：函数的实参对象，类数组对象，除了length之外没有数组的其他方法。使用arguments.callee指向当前的函数。
转数组：[].slice.call(arguments)，ES6可以Array.from(arguments)或者[...arguments]
* apply/call/bind：使用apply/call/bind来改变函数运行时的上下文。都定义在Function.prototype上，第一个参数是调用的函数内部this指向的对象，第二个及之后是要传入的函数参数，call和bind以值的形式传入，apply以数组的形式传入，call和apply返回调用函数的返回值，bind返回一个绑定函数。

> 实现一个bind方法
call() 方法在使用一个指定的 this 值和若干个指定的参数值的前提下调用某个函数或方法。
bind() 方法会创建一个新函数。当这个新函数被调用时，bind() 的第一个参数将作为它运行时的 this，之后的一序列参数将会在传递的实参前传入作为它的参数。
```
Function.prototype.bind = function (context) {
    var self = this;
    // 获取bind函数从第二个参数到最后一个参数
    var args = Array.prototype.slice.call(arguments, 1);
    return function () {
        // 这个时候的arguments是指bind返回的函数传入的参数
        var bindArgs = Array.prototype.slice.call(arguments);
        return self.apply(context, args.concat(bindArgs));
    }
}
```

> new一个对象的过程
* 创建一个空对象
* 设置这个对象的原型链
* 构造函数绑定这个对象，并运行
* 若构造函数返回引用类型，就返回这个引用类型的对象，否则返回新创建的对象


# 5. 事件

> 事件名词的定义

浏览器获得事件是先捕获后冒泡的。

* 事件冒泡：事件最开始以具体的元素接收，然后逐级向上传播到不具体的节点。target.addEventListener(eventName, callback)
* 事件捕获：事件最开始被不太具体的节点捕获，然后逐级向下传递给具体的节点。
target.addEventListener(eventName, callback, true)
* 事件委托：一个元素将事件响应委托给另外一个元素。
* 事件广播：elem.dispatchEvent(event)。

event.preventDefault(): 取消事件的默认行为
event.stopPropagation(): 阻止事件的进一步捕获或冒泡
使用document.createEvent()来模拟和创造自定义事件

# 6. 垃圾回收

> 垃圾回收算法与V8垃圾机制
常用的垃圾回收算法：1. 引用计次 2. 标记清除（从根节点开始遍历，V8使用）

V8的GC机制：
Yound Generation/Old Generation, To/From

> JS中出现内存泄漏的原因
1. 闭包
2. 没有切断Dom节点上的事件
3. 对象的相互引用
4. 过大的数组，无节制的循环

# 7. 浏览器进程

> 描述浏览器的进程与线程

浏览器内核包括渲染引擎和JS引擎，不过现在JS引擎越来越独立，所以浏览器内核主要表征渲染引擎。渲染引擎与JS引擎相互互斥。

![浏览器进程][browser]

# 8. 原型链与继承
> 原型与原型链
* 原型：每个**函数对象**都有一个prototype属性，这个属性本身是一个对象，用来包含所有实例共享的属性和方法，被成为原型对象/原型。每个原型会有一个constructor对象，指向原型所在的函数
* 原型链：每个**对象**都有一个[[Prototype]]属性，Chrome中可以使用`__proto__`访问，ES6使用Object.getPrototypeOf()访问。指向的是这个**对象的构造函数的原型**。当对象需要引用一个属性的时候，js引擎会首先在自身的属性表中查找，没有找到的话则在`__proto__`属性引用的对象属性表中继续查找，如此往复，直到找到或者`__proto__`指向null。这个引用链，就是原型链。

> 不同的继承方式及优缺

![继承方式](https://segmentfault.com/img/bVbd9t9?w=2162&h=990)

优缺：https://segmentfault.com/a/1190000015727237

> 写一个完美继承
```
function inheritPrototype(subType, superType){
    var prototype = Object.create(superType.prototype); // 创建了父类原型的浅复制
    prototype.constructor = subType;             // 修正原型的构造函数
    subType.prototype = prototype;               // 将子类的原型替换为这个原型
}

function SuperType(name){
    this.name = name;
    this.colors = ["red", "blue", "green"];
}

SuperType.prototype.sayName = function(){
    alert(this.name);
};

function SubType(name, age){
    SuperType.call(this, name);
    this.age = age;
}
// 核心：因为是对父类原型的复制，所以不包含父类的构造函数，也就不会调用两次父类的构造函数造成浪费
inheritPrototype(SubType, SuperType);
SubType.prototype.sayAge = function(){
    alert(this.age);
}
```

> ES6与ES5继承的异同

* ES6继承本质上是ES5继承的语法糖
* ES6继承中子类的构造函数的原型链指向父类的构造函数，ES5中使用的是构造函数复制，没有原型链指向。
* ES6子类实例的构建，基于父类实例，ES5中不是。

> super
子类在构造函数中必须调用父类的super方法，不然就会报错。这是因为子类自己的this对象，必须通过父类的构造函数创建。

> Object关于原型链的函数
// 图片


# 9. 设计模式

> 实现一个单例

核心：使用闭包保存实例对象，若实例对象存在，则直接返回实例对象。

```
class ProxysingletonCreateDiv {
  constructor(htmlStr) {
    return ProxysingletonCreateDiv.getInstance(htmlStr);
  }
  static getInstance(name) {
    if(!ProxysingletonCreateDiv.instance) {
      ProxysingletonCreateDiv.instance = new CreateDiv(name)
    }
    return ProxysingletonCreateDiv.instance;
  }
}
```

> 实现观察者

核心：用一个数组保存要通知的项，当一个对象变化后，逐一通知其他对象。
```
class publisher {
    constructor() {
        this.subscribers = {
            any: []
        }
    }
    subscribe(fn, type = 'any') {
        if (typeof this.subscribers[type] === 'undefined') {
            this.subscribers[type] = []
        }
        this.subscribers[type].push(fn)
    }
    unsubscribe(fn, type) {
        this.visitSubscribers('unsubscribe', fn, type)
    }
    publish(publication, type) {
        this.visitSubscribers('publish', publication, type)
    }
    visitSubscribers(action, arg, type = 'any') {
        this.subscribers[type].forEach((currentValue, index, array) => {
            if (action === 'publish') {
                currentValue(arg)
            } else if (action === 'unsubscribe') {
                if (currentValue === arg) {
                    this.subscribers[type].splice(index, 1)
                }
            }
        })
    }
}

let publish = new publisher();

```


# 10. ES6新特性
* 语法糖：class，箭头函数，对象属性的简写
* promise/async await/generator
* let/const
* 解构赋值
* for...of
* 原生模块化
* map/set
* symbol

> 箭头函数和普通函数的区别
普通函数的this指向最靠近的对象
箭头函数的this是定义时所在环境的this, 无法被改变，所以不能用作构造函数，不能new一个新对象，内部也没有arguments

# 11. 异步
> Promise的理解
Promise返回一个对象，你可以往这个对象上挂回调。它不能解决回调地狱，但可以通过链式调用，让异步操作逻辑更加清晰。promise对象有三种状态，pending、fulfilled和rejected，promise对象内部保存一个需要执行一段时间的异步操作，当异步操作执行结束后可以调用resolve或reject方法，来改变promise对象的状态，状态一旦改变就不能再变。new一个promise后可以通过then方法，指定resolved和rejected时的回调函数。
> Generator/yield/next
ES6新的一种异步回调方式，它实现了类似于同步编程的执行方式。yield是暂停标志，next是继续执行的执行器。
> async/await
是Generator函数的语法糖。有更好的语义和更广的适应性，内置执行器。
> 回调函数
> 基于事件的异步模型

