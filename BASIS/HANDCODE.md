#1. 手写Promise
#2. 手写AJAX
```
function getURL(URL) {
    return new Promise(function (resolve, reject) {
        const req = new XMLHttpRequest();
        req.open('GET', URL, true);
        req.onload = function () {
          if (req.readystatechange === 4) {
            if (req.status > 200 && req.status < 400) {
              resolve(req.responseText);
            } else {
              reject(new Error(req.statusText));
            }
          }
        };
        req.onerror = function () {
            reject(new Error(req.statusText));
        };
        req.send();
    });
}

const URL = "http://httpbin.org/get";
getURL(URL).then(function onFulfilled(value){
    console.log(value);
}).catch(function onRejected(error){
    console.error(error);
});
```
#3. 手写JSONP
#4. 手写双向绑定