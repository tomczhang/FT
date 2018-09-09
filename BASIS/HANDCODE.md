#1. 手写Promise
```
/**
 * 1. Promise是一个构造函数，接受一个函数作为参数，这个函数函数又有两个参数，一个是成功后调用，一个是失败后调用
 * 2. 异步设置状态
 * 3. 链式调用
 * 
 */
class Promise {
  constructor (executor) {
    this.state = 'pending';
    this.value = '';
    this.fulfulledArr = [];
    this.rejectedArr = [];
    this.resolve = this.resolve.bind(this);
    this.reject = this.reject.bind(this);
    this.then = this.then.bind(this);
    try {
      executor(this.resolve, this.reject);
    } catch (e){
      console.log('e', e)
      this.reject();
    }
    
  }

  resolve (value) {
    if (this.state === 'pending') {
      this.value = value;
      this.fulfulledArr.forEach(element => {
        element(this.value);
      });
      this.state = 'fulfilled';
    }
  }

  reject () {
    if (this.state === 'pending') {
      this.value = value;
      this.state = 'rejected';
    }
  }

  then (onfulfilled, onrejected) {
    let promise2;
    if (this.state === 'fulfilled') {
      let x = onfulfilled(this.value);
      return promise2 = new Promise(function(resolve, reject) {
        resolve(x);
      })
      
    }
    if (this.state === 'rejected') {
      onrejected(this.value);
    }
    if (this.state === 'pending') {
      let self = this;
      return promise2 = new Promise(function(resolve, reject) {
        self.fulfulledArr.push(function() {
          let x = onfulfilled(self.value)
          resolve(x);
        });
        self.rejectedArr.push(onrejected);
      })
    }
  }
}

const test = new Promise(((resolve, reject) => {
  console.log('AAA');
  setTimeout(()=> {
    resolve(1000);
  }, 3000)
}));
// test.reject();

test.then(function(data1) {
  console.log('hey', data1);
  return data1 + 5;
})
.then(function (data2){
  console.log('hey2', data2 + 1);
})
console.log('herhe')
console.log(new Promise(((resolve, reject) => {})))
```
#2. 手写AJAX
```
var xhr = createXHR();
xhr.onreadystatechange = function(){
    if (xhr.readyState == 4){ //每次readyState改变都检查一次，仅当4时才真正作用
            //如果状态码>200且<300，或者为304,输出响应文本
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
            alert(xhr.responseText);
        } else {
            alert("Request was unsuccessful: " + xhr.status);
        }
    }
};
xhr.open("get", "example.txt", true); //添加onreadystatechange事件后使用open()方法
xhr.send(null);

```

#3. 手写双向绑定
```
var obj={}
Object.defineProperty(obj,'txt',{
    get:function(){
        return obj
    },
    set:function(newValue){
        document.getElementById('txt').value = newValue
        document.getElementById('show-txt').innerHTML = newValue
    }
})
document.addEventListener('keyup',function(e){
    obj.txt = e.target.value
})
```
#4. 一个数组中其他值都出现了两次，只有一个值出现了一次，找出这个值
* Hash
* 异或
```
var test = [1,2,2,3,3,4,4];
function getSingle () {
  var map = {};
  for(let i =0;i< test.length;i++) {
    const item = test[i];
    if (!map[item]) {
      map[item] = 1;
    } else {
      map[item]++;
    }
  }
  for(let item in map) {
    console.log(map[item])
    if(map[item] === 1) {
      return item;
    }
  }
} 

```
# 5. isNaN实现
NaN number类型，和自身不相等

Number.isNaN = function (value) {
    return typeof value === 'number' && isNaN(value);//es6下面已经这样实现
}

# 6. 手写DeepClone
```
function deepClone(obj) {
  if (obj === null || typeof obj !== 'object') return obj;
  if (obj instanceof Boolean) return new Boolean(obj.valueOf());
  if (obj instanceof Number) return new Number(obj.valueOf());
  if (obj instanceof String) return new String(obj.valueOf());
  if (obj instanceof RegExp) return new RegExp(obj.valueOf());
  if (obj instanceof Date) return new Date(obj.valueOf());
  var cpObj = obj instanceof Array ? [] : {};
  for (var key in obj) cpObj[key] = deepClone(obj[key]);
  return cpObj;
}
```

# 7. 函数防抖/截流
```
let debounce = function (fn, delay) {
  const self = this;
  return function (args) {
    clearTimeout(fn.id);
    fn.id = setTimeout(function () {
      fn.apply(self, args);
    }, delay);
  }
}
let throttle = function (fn, delay) {
  const self = this;
  let last;
  return function (args) {
    let now = Date.now();
    if (last && now < last + dalay) {
      clearTimeout(fn.id);
      fn.id = setTimeout(function () {
        fn.apply(self, args);
      }, delay);
    } else {
      last = now;
      fn.apply(self, args);
    }
  }
}
```
