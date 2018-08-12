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
#3. 手写JSONP

#4. 手写双向绑定
https://segmentfault.com/a/1190000011225943