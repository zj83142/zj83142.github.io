---
title: Promise 对象
date: 2017-11-10 18:44:21
tags:
  - Promise
categories: 
  - Javascript
---

[Promise对象 - 阮一峰](http://javascript.ruanyifeng.com/advanced/promise.html#toc9)

Promises原本只是社区提出的一个构想，一些外部函数库率先实现了这个功能。ES2015正式发布（也就是ES6），其中promise被列为只是规范。因此目前JavaScript语言原生支持Promise对象。现在，promise已在浏览器中实现，在chrome32、Opera19、Firefox29、Safari8 和Microsoft Edge中，promise默认启用。promise 是抽象异步处理对象以及对其进行各种操作的组件。

### javascript 的异步执行

javascript语言的执行环境是单线程(single thread)。每次只能处理一件任务，如果有多个任务，就必须排队。一个执行完成，再执行后面一个任务。这种模式的好处是实现起来比较简单，执行环境相对简单。坏处是只要有一个任务耗时很长，后面的任务都必须排队等待，会拖延整个程序的执行。常见的浏览器无应用（假死），往往就是这样形成的。
<!-- more -->
javascript语言本身并不慢，慢的是读写外部数据，比如等待Ajax请求返回结果。这个时候，如果服务器迟迟没有相应。或者网络不通畅，就会导致脚本长时间停滞。

为了解决这个问题，javascript语言将任务的执行模式分为两种，同步(synchronous) 和异步(Asynchronous)。**同步模式**就是传统的做法，按顺序执行，这往往用于一些简单的、快速的、不涉及IO读写的操作。**异步模式**将任务分成两段，第一段代码包含对外部数据的请求，第二段代码被写成一个回调函数，包含了对外部数据的处理，第一段代码执行完，不是立刻执行第二段代码，而是讲程序的执行权交给第二个任务，等到外部数据返回了，再由系统通知执行第二段代码。所以，程序的执行顺序与任务的排列顺勋是不一致的，异步的。

#### 回调函数

回调函数是异步编程最基本的方法。假设有两个函数f1和f2, 后者必须等到前者执行完成，才能执行，这时可以考虑改写f1，把f2 写成发的回调函数。
```
function f1(callback) {
  // f1 的代码

  // f1 执行完成后，调用回调函数
  callback();
}
// 执行代码
f1(f2);
```
回调函数的优点是简单、容易理解和部署，缺点是不利于代码的阅读和维护，各个部分之间高度**耦合(Coupling)**,是的程序结构混乱、流程难以追踪（尤其是回调函数嵌套的情况），而且每个任务只能指定一个回调函数。

#### 事件监听
另一种思路是采用事件驱动模式，任务的执行不取决于代码的顺序。而取决于某个事件是否发生。
```
f1.on('done', f2);
function f1() {
  setTimeout(function() {
    // f1 的任务代码
    f1.trigger('done');
  }, 1000);
 
}
```
上面代码中，f1.trigger('done')表示，执行完成后，立即触发done事件，从而开始执行f2。这种方法的优点是比较容易理解，可以绑定多个事件，每个事件可以指定多个回调函数，而且可以**"去耦合(Decoupling)"**，有利于实现模块化。缺点是整个程序都编程事件驱动型，运行流程会变得很不清楚。

#### 发布/订阅
“事件”可以理解成“信号”，如果存在一个“信号中心”，某个任务执行完成，就向信号中心“发布(publish)”一个信号，其他任务可以向信号中心“订阅(subscribe)”这个信号,从而知道什么时候自己可以开始。这就叫做“发布/订阅模式(publish-subscribe oattern)”,又称“观察者模式(observer pattern)”。
这个模式有多种实现，下面采用Ben Alman 的Tiny Pub/Sub —— jquery的一个插件。
```
jQuery.subscribe('done', f2); // f2向信号中心订阅"done"信号

function f1() {
  setTimeout(function() {
    // f1 的代码任务
    jQuery.publish("done"); // f1 执行完后，想信号中心jquery 发布done信号，从而已发f2 的执行。
  }, 1000);
}
jQuery.unsubscribe('done', f2); // f2 执行完后，可以取消订阅(unsubscirbe)

```
这种方法的性质与“事件监听”类似，但是明显优于后者。因为我们可以通过查看“消息中心”，了解存在多少信号，每个信号有多少订阅者，从而监控程序的运行。

### 异步操作的流程控制
如果有多个异步操作，就存在一个流程控制的问题：确定操作执行的顺序，以后如何保证遵守这种顺序。
```
function async(arg, callback) {
  console.log('参数为' + arg + ', 1秒后返回结果');
  setTimeout(function() {
    callback(arg * 2);
  }, 1000);
}
```
上面代码的async函数是一个异步任务，非常耗时，每次执行需要1秒才能完成，然后在调用回调函数，如果有6个这样的异步任务，需要全部完成后，才能执行下一步的final函数。
```
function final(value) {
  console.log('完成: ', value);
}
```
#### 串行执行
我们可以编写一个流程控制函数，让它来控制异步任务，一个任务完成以后，再执行另一个，这就叫串行执行。
```
var items = [1, 2, 3, 4, 5, 6];
var results = [];
function series(item) {
  if(item) {
    async( item, function(result) {
      results.push(result);
      return series(items.shift());
    });
  } else {
    return final(results);
  }
}
series(items.shift());
```
上面代码中，函数series就是串行函数，它会依次执行异步任务，所有任务都完成后，才会执行final函数。items数组保存每一个异步任务的参数，results数组保存每一个异步任务的运行结果。

#### 并行执行
流程控制函数也可以是并行执行，即所有异步任务同时执行，等到全部完成以后，才执行final函数。
```
var items = [1, 2, 3, 4, 5, 6];
var results = [];
items.forEach(function(item) {
  async(item, funciton(result) {
    results.push(result);
    if(result.length == items.length) {
      final(results);
    }
  })
});
```
上面代码中，forEach方法会同事发起6个异步任务，等到他们全部完成以后，才会执行final函数。

并行执行的好处是效率较高，比起串行执行只能执行一个任务，比较节约事件。但是问题在于如果秉性的任务较多，很容易耗尽系统资源，拖慢运行速度。

#### 并行与串行的结合

所谓并行和串行的结合，就是设置一个门槛，每次最多只能并行执行n个异步任务，这样就避免了过分占用系统资源。
```
var items = [1, 2, 3, 4, 5, 6];
var results = [];
var running = 0;
var limit = 2;
function launcher() {
  while(running < limit && item.length > 0) {
    var item = items.shift();
    async(item, funciton(result) {
      results.push(result);
      running --;
      if(items.length > 0) {
        launcher();
      } else if(running == 0) {
        final(results);
      }
    });
    running ++;
  }
}
launcher();
```
上面代码中，最多只能同时运行两个异步任务。变量running 记录当前正在运行的任务数，只要低于门槛值，就再启动一个新的任务，如果等于0，就表示任务都执行完了，这时就执行final函数。

### promise对象

promise 对象是CommonJS工作组提出的一种规范，目的是为异步操作提供统一的接口。
首先，他是一个对象，与其他javascript的用法一样，其次，它起到代理作用(proxy)，充当异步操作与回调函数之间的中介。它使得异步操作具备同步操作的接口，使得程序具备正常的同步运行的流程，回调函数不必再一层一层嵌套。

简单说，每一个异步任务立即返回一个promise对象，由于是立即返回，所以可以采用同步操作的流程，这个promise对象有一个then方法，允许指定回调函数，在异步任务完成后调用。
```
(new Promise(f1).then(f2)); // 异步操作f1返回一个Promise对象，它的回调函数是f2

// 对于多层嵌套的回调函数尤其方便，采用Promises接口以后，程序流程变得非常清楚，十分易读。

// 传统写法
step1(function (value1) {
  step2(value1, function(value2) {
    step3(value2, function(value3) {
      step4(value3, function(value4) {
        // ...
      });
    });
  });
});

// Promises的写法
(new Promise(step1))
  .then(step2)
  .then(step3)
  .then(step4);
```
#### promise接口

promise接口的基本思想是，异步任务返回一个Promise对象。

promise对象只有三种状态
- 异步操作**未完成(pending)**
- 异步操作**已完成(resolved, 又称fulfilled)**
- 异步操作**失败(rejected)**

这三种状态的改变只有两种途径
- 异步操作从未完成 ——> 完成
- 异步操作从未完成 ——> 失败

这种改变只能发生一次，一旦当前状态改变为“已完成”或“失败”，就意味着不会再有新的状态变化了，因此Promise对象的最终结果只有两种
- 异步操作成功，Promise对象返回一个值，状态改变为resolved
- 异步操作失败，Promise对象抛出一个错误，状态改变为rejected

Promise 对象使用then方法添加回调函数。then方法可以接受两个回调函数，第一个是异步操作成功时(变为resolved状态)时的回调函数，第二个是异步操作失败（变为rejected）时的回调函数（可以省略）。
```
po.then(    // po 是一个Promise对象
  console.log,
  console.error
);

// then方法可以链式使用
po.then(step1)
  .then(step2)
  .then(step3)
  .then(
    console.log,
    console.error
  );

// 从同步的角度看，上面代码等同于下面的形式

try {
  var v1 = step1(po);
  var v2 = step2(v1);
  var v3 = step3(v2);
  console.log(v3);
} catch (error) {
  console.error(error);
}
```

#### promise对象的生成
ES6提供了原生的Promise构造函数，用来生成promise实例、
```
var promise = new Promise(function(resolve, reject) {
  // 异步操作的代码

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

promise构造函数接收一个函数作为参数，该函数的两个参数分别是resolve和reject。由javascript殷勤提供，不用自己部署。

resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从Pending变为Resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从Pending变为Rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

Promise实例生成以后，可以用then方法分别指定Resolved状态和Reject状态的回调函数。
```
po.then(function(value) {
  // success
}, function(value) {
  // failure
});
```

#### 用法辨析
Promise的用法，简单说就是一句话：使用then方法添加回调函数。但是，不同的写法有一些细微的差别，请看下面四种写法，它们的差别在哪里？
```
/ 写法一
doSomething().then(function () {
  return doSomethingElse();
});

// 写法二
doSomething().then(function () {
  doSomethingElse();
});

// 写法三
doSomething().then(doSomethingElse());

// 写法四
doSomething().then(doSomethingElse);
```

为了便于讲解，下面这四种写法都再用then方法接一个回调函数finalHandler。写法一的finalHandler回调函数的参数，是doSomethingElse函数的运行结果。
```
doSomething().then(function () {
  return doSomethingElse();
}).then(finalHandler);
```
写法二的finalHandler回调函数的参数是undefined。
```
doSomething().then(function () {
  doSomethingElse();
  return;
}).then(finalHandler);
```
写法三的finalHandler回调函数的参数，是doSomethingElse函数返回的回调函数的运行结果。
```
doSomething().then(doSomethingElse())
  .then(finalHandler);
```
写法四与写法一只有一个差别，那就是doSomethingElse会接收到doSomething()返回的结果。
```
doSomething().then(doSomethingElse)
  .then(finalHandler);
```

### promise 的应用

#### 加载图片
我们可以把图片的加载写成一个Promise对象。
```
var preloadImage = function (path) {
  return new Promise(function (resolve, reject) {
    var image = new Image();
    image.onload  = resolve;
    image.onerror = reject;
    image.src = path;
  });
};
```
#### Ajax操作
Ajax操作是典型的异步操作，传统上往往写成下面这样。
```
function search(term, onload, onerror) {
  var xhr, results, url;
  url = 'http://example.com/search?q=' + term;
  xhr = new XMLHttpRequest();
  xhr.open('GET', url, true);
  xhr.onload = function (e) {
    if (this.status === 200) {
      results = JSON.parse(this.responseText);
      onload(results);
    }
  };
  xhr.onerror = function (e) {
    onerror(e);
  };
  xhr.send();
}
search("Hello World", console.log, console.error);
```
如果使用Promise对象，就可以写成下面这样。
```
function search(term) {
  var url = 'http://example.com/search?q=' + term;
  var xhr = new XMLHttpRequest();
  var result;
  var p = new Promise(function (resolve, reject) {
    xhr.open('GET', url, true);
    xhr.onload = function (e) {
      if (this.status === 200) {
        result = JSON.parse(this.responseText);
        resolve(result);
      }
    };
    xhr.onerror = function (e) {
      reject(e);
    };
    xhr.send();
  });
  return p;
}
search("Hello World").then(console.log, console.error);
```
加载图片的例子，也可以用Ajax操作完成。
```
function imgLoad(url) {
  return new Promise(function(resolve, reject) {
    var request = new XMLHttpRequest();
    request.open('GET', url);
    request.responseType = 'blob';
    request.onload = function() {
      if (request.status === 200) {
        resolve(request.response);
      } else {
        reject(new Error('图片加载失败：' + request.statusText));
      }
    };
    request.onerror = function() {
      reject(new Error('发生网络错误'));
    };
    request.send();
  });
}
```