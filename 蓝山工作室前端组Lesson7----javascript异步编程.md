# 蓝山工作室前端组Lesson7----javascript异步编程



JavaScript 的执行模型基于**单线程**，这意味着它在同一时间段内只能处理一个任务，就像一个人在厨房做饭，一次只能专注完成一个步骤。



如果采用同步方式处理所有操作，当遇到耗时任务（比如从服务器获取数据、读取本地文件、等待定时器结束）时，主线程必须暂停当前工作，等待耗时任务完成后才能继续下一步。这种情况下，前端页面会出现明显的卡顿 —— 用户点击按钮无响应、页面无法滚动、动画停止播放，因为主线程被耗时任务 “卡住” 了，无法处理用户交互和 UI 渲染等关键工作。



异步编程的核心目的，就是打破这种阻塞状态。它允许耗时任务脱离主线程在后台执行，主线程则可以继续处理优先级更高的任务（比如响应用户点击、更新页面元素）。当后台的耗时任务完成后，会通过特定机制（回调函数、Promise、async/await 等）通知主线程，主线程再在空闲时处理该任务的结果。



## 同步与异步



---



### 同步



JavaScript中的同步（Synchronous）指的是代码按照顺序逐行执行的方式。在同步代码中，每一行代码的执行都必须等待前一行代码执行完毕才能继续执行下一行代码，这意味着代码的执行是阻塞的，直到当前行的任务完成后才会执行下一行代码。



在同步代码中，如果遇到一个耗时的操作（如网络请求、文件读取、复杂计算等），代码会一直等待该操作完成后才会继续执行下一行代码。这可能导致页面或应用程序的阻塞，因为在等待耗时操作完成期间，JavaScript执行引擎无法执行其他任务，界面可能会冻结或不响应。



```javascript

console.log("开始");

console.log("执行任务1");

console.log("执行任务2");

console.log("结束");

```



在执行同步代码时，如果遇到一个耗时的操作，例如一个循环计算耗费较长的时间，那么代码会一直停留在这个循环中，直到循环计算完成才会继续执行后续代码。



同步代码的执行方式相对简单，因为代码按照顺序执行，开发者可以更容易地理解和控制代码的执行流程。然而，对于需要处理大量数据或耗时操作的情况，同步代码可能会导致性能问题和用户体验下降，因为页面或应用程序可能会在等待操作完成时出现卡顿或无响应的情况。



为了解决同步代码的阻塞问题，JavaScript提供了异步编程的机制，可以使用回调函数、Promise、async/await等方式来处理耗时操作，使得代码在等待操作完成期间可以执行其他任务，从而提高响应性和并发性。



### 异步



```javascript

console.log("开始")；

setTimeout(function(){

   consle.log("异步操作完成")

},2000);

console.log（"结束"）

```



在这个示例中，使用了`setTimeout`函数来模拟一个异步操作，它会在2秒后触发回调函数。在执行过程中，代码不会等待`setTimeout`



```javascript

开始

结束

异步操作完成

```



在执行异步代码时，当遇到一个耗时的操作（如网络请求、文件读写、定时器等），代码会将该操作委托给其他机制处理，并立即继续执行后续代码。当操作完成后，会触发一个回调函数或返回一个Promise对象，以通知代码操作的完成。



-



<img title="" src="https://cdn.jsdelivr.net/gh/qinyso/pic-bed/20251125204653270.png" alt="">

## 任务队列与事件循环



---



### 宏任务



宏任务是指由 JavaScript 主线程执行的任务，它包括但不限于以下情况：



* **浏览器事件**（如 click、mouseover 等）
  
  

* **定时器任务**（如 setTimeout 和 setInterval）
  
  

* **页面渲染**（如 回流或重绘）
  
  

* **事件回调**（如 I/O、点击事件等）
  
  

* **网络请求** （如 XMLHttpRequest 和 fetch 等）

  示例1：用事件监听创建宏任务

  ```javascxript

  const button=document.querySelector("button");

  button.addEventListener("click",()=>{

      console.log("Button clicked");

  });

  console.log("Waiting for button click...");

  ```



    示例2： 使用定时器创建宏任务



```html

<!DOCTYPE html>

<html lang="en">

<head>

<body>

  <button id="btn">Click me</button>  

</body>

<script>

console.log("start");

setTimeout(()=>{

  console.log("In Timeout");

},2000);

console.log("end");

const button=document.querySelector('#btn');

button.addEventListener('click',()=>{

  console.log("Button Clicked")

});

console.log("Waiting for Button onclick")

</script>

</html>

```



示例3：页面渲染



```javascript

console.log("Start");



// 修改页面样式

document.body.style.backgroundColor = "red";



console.log("End"); 

```



> 总结： 宏任务的使用广泛，包括定时器任务、网络请求、事件监听器等。理解宏任务的概念和用法可以帮助我们正确处理 JavaScript 中的异步操作，并合理安排任务的执行顺序，以提高应用的性能和用户体验。



### 微任务



微任务是 JavaScript 事件循环中优先级**高于宏任务**的异步任务类型，用于处理需要**尽快执行**的异步操作（通常是 Promise 相关回调），其执行时机严格遵循 “同步代码执行完毕后立即清空微任务队列” 的规则。



* Promise 的回调函数

* Async/Await 函数

* MutationObserver 的回调函数

* process.nextTick（Node.js 环境下）
  
  

```html

<!DOCTYPE html>

<html lang="en">

<head>

<body>



</body>

<script>

console.log("同步代码");

setTimeout(()=>{

  console.log('宏任务(setTimeout)');

},0)

Promise.resolve().then(()=>{

  console.log('微任务(promise)');

});

</script>

</html>

```



### 事件循环



事件循环是一个持续运行的循环，它不断地从任务队列中获取任务并执行。事件循环的运行过程如下：



1. 从宏任务队列中取出一个任务执行。

2. 执行过程中，如果遇到微任务，将其添加到微任务队列中。

3. 当前宏任务执行完毕后，检查微任务队列，依次执行所有微任务。

4. 检查是否需要渲染页面的更新。

5. 执行下一个宏任务（从宏任务队列中取出）。

6. 重复上述步骤，循环执行。
   
   

<img title="" src="https://cdn.jsdelivr.net/gh/qinyso/pic-bed/20251127163543362.png" alt="">

这个过程保证了任务的执行顺序和异步代码的管理。通过将任务按照宏任务和微任务的方式添加到队列中，并通过事件循环的机制来执行任务，JavaScript能够实现高效的异步编程和处理。



需要注意的是，微任务在每个宏任务执行完毕后立即执行，而不是等待所有的宏任务执行完毕。这意味着微任务能够在同一轮事件循环中快速执行，而不需要等待下一轮事件循环。



任务队列和事件循环机制是 JavaScript 异步编程的核心概念，理解它们能够帮助开发者更好地处理异步代码的执行顺序和控制程序的流程。





### 回调函数



回调函数是一种传递给异步函数的函数，在异步操作完成后被调用。通过将回调函数作为参数传递给异步函数，可以在异步操作完成后执行相应的处理逻辑。



#### 1.定时器回调函数



在使用`setTimeout`或`setInterval`函数创建定时器时，可以传递一个回调函数作为参数，用于指定需要在一定时间后执行的操作。



```javascript

setTimeout(function(){

    console.log("定时器回调函数")

},1000);

```



#### 2.事件处理回调函数



在处理DOM事件或其他异步事件时，通常需要指定一个回调函数来处理事件触发时的操作。



```javascript

document.addEventListener("click", function () {

  console.log("点击事件回调函数");

});

```



#### .AJAX回调函数



在进行异步请求（如通过XMLHttpRequest或fetch发送请求）时，可以指定回调函数来处理响应结果。



```javascript

fetch("https://api.example.com/data")

  .then(function (response) {

    return response.json();

  })

  .then(function (data) {

    console.log("AJAX回调函数", data);

  });

```



### 回调地狱



回调地狱（Callback Hell）是指在异步编程中，多个嵌套的回调函数形成的复杂和难以维护的代码结构。当有多个异步操作需要依次执行，并且后一个操作依赖于前一个操作的结果时，代码会出现深层嵌套的回调函数，导致代码可读性差、难以理解和调试。



以下是一个典型的回调地狱示例：



```javascript

asyncFunc1(function (result1) {

  asyncFunc2(result1, function (result2) {

    asyncFunc3(result2, function (result3) {

      asyncFunc4(result3, function (result4) {

        // 更多回调函数...

      });

    });

  });

});

```



这种嵌套回调的代码结构使得代码难以理解，可读性差，同时也增加了出错的可能性。当需要处理大量的异步操作时，回调地狱会导致代码的可维护性和可扩展性变差。



### Promise



在 JavaScript 中，Promise 是一种用于处理异步操作的对象。它表示一个异步操作的最终完成或失败，并提供了一种更优雅的方式来处理异步代码。



创建 Promise 对象可以使用 Promise 构造函数，它接受一个执行器函数作为参数。执行器函数会立即执行，通常包含异步操作，并决定 Promise 是成功还是失败。执行器函数带有两个参数，它们是 resolve 和 reject 函数，用于将 Promise 标记为成功或失败。



```javascript

new Promise((resolve, reject) => {

    console.log("Promise start")

    resolve('success')

})

console.log("End")

```



我们可以直接实例化



```js

const myPromise = new Promise(resolve => {

    setTimeout(() => {

        console.log('执行完成');

        resolve('win');

    }, 0);

});



myPromise.then(() => {

    console.log('Promise成功后的回调');

});

```



也可以使用返回实例函数的方式接收(推荐)

```javascript
const setPromise=()=>{
    return new Promise(resolve=>{
        setTimeout(()=>{
           resolve('在蓝山学习真是太有趣了')
        },1000)
    })
 }
 const myPromise=setPromise()
 myPromise.then(res=>{
    console.log(res)
 }) 
```

Promise的构造函数接收一个参数(函数)，该函数接收两个参数：

* resolve：异步操作执行成功时的回调函数
* reject：异步操作执行失败时的回调函数

### then()方法的链式调用

在 JavaScript 的 Promise 中，`then` 方法用于处理 Promise 成功的情况，并支持链式调用。通过链式调用 `then` 方法，可以在一个 Promise 的成功回调函数中返回另一个 Promise，从而形成一个 Promise 链。

在链式调用中，每个 `then` 方法都会返回一个新的 Promise 对象，该对象可以用于注册下一个 `then` 方法的回调函数。这样，可以将多个异步操作链接在一起，并按顺序处理它们的结果。

下面是一个典型的示例，演示了 `then` 方法的链式调用：

```javascript
asyncFunc1()
  .then((result1) => {
    console.log("操作 1 成功:", result1); 
    return asyncFunc2(result1);
  })
  .then((result2) => {
    console.log("操作 2 成功:", result2);
    return asyncFunc3(result2);
  })
  .then((result3) => {
    console.log("操作 3 成功:", result3); 
  })
  .catch((error) => {
    console.log("操作失败:", error);
  });
```

在上述示例中，`asyncFunc1`、`asyncFunc2`、`asyncFunc3` 是三个异步函数，它们返回 Promise 对象。通过链式调用 `then` 方法，每个操作的结果都会传递给下一个操作。如果任何一个操作失败，则会跳过后续的 `then` 方法，直接执行 `catch` 方法中的错误处理逻辑。

需要注意的是，在链式调用中，每个 `then` 方法都可以返回一个新的 Promise 对象，从而允许进一步的链式调用。这使得代码更具可读性和可维护性，避免了回调地狱的问题。

此外，`then` 方法还可以接受两个参数，分别是成功回调函数和失败回调函数。如果只关心成功的情况，也可以省略失败回调函数。

### catch()的用法

`catch` 方法用于处理 Promise 失败的情况。它是 `then` 方法的一个特殊形式，用于注册 Promise 的错误处理回调函数。

`catch` 方法接受一个回调函数作为参数，该回调函数会在 Promise 被拒绝（rejected）时被调用，并接收拒绝的原因（通常是一个 Error 对象）作为参数。通过 `catch` 方法，可以集中处理 Promise 链中的错误，避免在每个 `then` 方法中都编写错误处理逻辑。

下面是一个简单的示例，演示了 `catch` 方法的使用：

```javascript
asyncFunc()
  .then((result) => {
    console.log("操作成功:", result);
  })
  .catch((error) => {
    console.log("操作失败:", error);
  });


```

上述示例中，`asyncFunc` 是一个返回 Promise 对象的异步函数。通过 `then` 方法注册了成功回调函数，通过 `catch` 方法注册了错误处理回调函数。如果 Promise 成功，则会执行成功回调函数；如果 Promise 失败，则会执行错误处理回调函数。

需要注意的是，`catch` 方法只会捕获最近的 Promise 链中的错误。如果在链式调用中的某个 `then` 方法中抛出了错误，但没有使用 `catch` 方法进行处理，那么错误将会被传递到下一个 `catch` 方法中。因此，建议在 Promise 链的末尾使用一个最终的 `catch` 方法，以确保能够处理所有可能的错误。

除了使用 `catch` 方法，还可以在 `then` 方法中的第二个参数中指定失败回调函数来处理 Promise 的失败情况。这种方式与 `catch` 方法类似，但它只处理当前 `then` 方法的失败，不会捕获后续 `then` 方法中的错误。

### all()方法

`Promise.all` 是 JavaScript 中 Promise 的一个静态方法，用于处理多个 Promise 对象，并在所有 Promise 都成功完成时返回一个新的 Promise 对象。如果其中任何一个 Promise 失败，则返回的 Promise 会立即被拒绝，并传递失败的原因。

`Promise.all` 方法接受一个 Promise 数组作为参数，并返回一个新的 Promise。返回的 Promise 将在所有的 Promise 都成功解决后解决，并将解决值作为一个数组传递。数组中的值的顺序与输入的 Promise 数组中的顺序一致。

下面是一个示例，演示了 `Promise.all` 方法的使用：



```javascript
const promise1 = asyncFunc1();
const promise2 = asyncFunc2();
const promise3 = asyncFunc3();
Promise.all([promise1, promise2, promise3])
  .then((results) => {
    console.log("所有操作都成功:", results);
  })
  .catch((error) => {
    console.log("至少有一个操作失败:", error);
  });

```

在上述示例中，`promise1`、`promise2`、`promise3` 是三个 Promise 对象，它们分别表示三个异步操作。通过 `Promise.all` 方法将它们作为数组传递，并使用 `then` 方法注册成功回调函数。当所有的 Promise 都成功解决时，成功回调函数将被调用，并接收一个包含所有结果的数组。

如果其中任何一个 Promise 失败，则返回的 Promise 会立即被拒绝，并传递失败的原因。在上述示例中，如果任何一个 Promise 失败，`catch` 方法中的错误处理回调函数将被调用。

需要注意的是，传递给 `Promise.all` 的 Promise 数组中的 Promise 可以是异步操作的返回值，也可以是已经解决的（已完成或已拒绝）Promise。如果传递一个空数组给 `Promise.all`，则返回的 Promise 将立即解决为一个空数组。

### race()方法

`Promise.race` 是 JavaScript 中 Promise 的一个静态方法，用于处理多个 Promise 对象，并在其中任意一个 Promise 完成（无论是成功还是失败）时返回一个新的 Promise 对象。返回的 Promise 将与第一个完成的 Promise 具有相同的状态和结果。

`Promise.race` 方法接受一个 Promise 数组作为参数，并返回一个新的 Promise。返回的 Promise 将与第一个完成的 Promise 具有相同的状态和结果。

下面是一个示例，演示了 `Promise.race` 方法的使用：



```javascript
const promise1 = asyncFunc1();
const promise2 = asyncFunc2();
const promise3 = asyncFunc3();
Promise.race([promise1, promise2, promise3])
  .then((result) => {
    console.log("第一个完成的操作:", result);
  })
  .catch((error) => {
    console.log("所有操作都失败:", error);
  });

```

在上述示例中，`promise1`、`promise2`、`promise3` 是三个 Promise 对象，它们分别表示三个异步操作。通过 `Promise.race` 方法将它们作为数组传递，并使用 `then` 方法注册成功回调函数。当其中任意一个 Promise 完成时（无论是成功还是失败），成功回调函数将被调用，并接收第一个完成的 Promise 的结果。

如果其中任何一个 Promise 失败，返回的 Promise 也会被拒绝，并传递失败的原因。在上述示例中，如果所有的 Promise 都失败，则 `catch` 方法中的错误处理回调函数将被调用。

需要注意的是，`Promise.race` 方法只关注第一个完成的 Promise，而忽略其他 Promise 的状态和结果。一旦第一个 Promise 完成，返回的 Promise 将立即完成，而不会等待其他 Promise 的完成。

### Generator*

JavaScript Generator（生成器）是一种特殊类型的函数，它可以通过多次返回值的方式来生成一系列的值。Generator 函数使用 `function*` 语法进行定义，并使用 `yield` 关键字来产生（yield）值。

下面是一个简单的示例，展示了 Generator 函数的定义和使用：

```javascript
function* numberGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

const generator = numberGenerator();
console.log(generator.next()); // { value: 1, done: false }
console.log(generator.next()); // { value: 2, done: false }
console.log(generator.next()); // { value: 3, done: false }
console.log(generator.next()); // { value: undefined, done: true }

```

在上面的示例中，`numberGenerator` 是一个 Generator 函数，通过使用 `function*` 语法进行定义。函数体内使用 `yield` 关键字来产生一系列的值。每次调用 `yield` 语句时，函数会返回一个对象，其中 `value` 属性表示生成的值，而 `done` 属性表示是否已经生成完所有的值。

通过调用 `numberGenerator` 函数，可以获得一个 Generator 对象 `generator`。然后，可以使用 `generator.next()` 方法来获取生成器生成的值。每次调用 `next()` 方法，生成器都会从上一个 yield 语句处继续执行，直到遇到下一个 yield 语句或函数结束。

当生成器生成完所有的值后，再次调用 `next()` 方法时，会返回一个值为 `undefined` 的对象，并将 `done` 属性设为 `true`，表示生成器已经完成。

除了使用 `next()` 方法来遍历生成器生成的值之外，还可以使用 `for...of` 循环来遍历生成器。这样可以更方便地获取生成器生成的所有值。

Generator 函数还支持通过 `yield*` 语法来委托给另一个 Generator 函数。这样可以将多个 Generator 函数组合在一起，形成一个更复杂的生成器。

终极操作： async
-----------

---

在 JavaScript 中，`async/await` 是一种用于处理异步操作的语法糖（syntactic sugar）。它建立在 Promise 的基础上，提供了一种更简洁、更直观的方式来编写异步代码。

`async/await` 结合使用两个关键字：`async` 和 `await`。

1. `async` 关键字用于修饰函数，将其标记为异步函数。异步函数内部可以使用 `await` 关键字来等待（await）一个 Promise 对象的解决（resolve）或拒绝（reject）。
2. `await` 关键字用于等待一个 Promise 对象的解决或拒绝，并暂停异步函数的执行，直到 Promise 对象完成。

下面是一个示例，演示了 `async/await` 的使用：

```javascript
function delay(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

async function asyncFunc() {
  console.log("开始");

  await delay(2000);
  console.log("等待 2 秒");

  await delay(1000);
  console.log("又等待 1 秒");

  console.log("完成");
}

asyncFunc();

```

在上述示例中，`delay` 函数返回一个 Promise 对象，用于模拟一个异步操作。`asyncFunc` 是一个异步函数，使用 `async` 关键字进行修饰。在函数内部，通过使用 `await` 关键字等待 `delay` 函数返回的 Promise 对象。

当遇到 `await` 关键字时，`asyncFunc` 函数会暂停执行，直到等待的 Promise 对象解决或拒绝。在等待期间，控制权会返回给调用者，允许其他操作或函数继续执行。

在上述示例中，`asyncFunc` 函数会依次等待 2 秒和 1 秒，然后打印出一系列的日志。由于使用了 `await` 关键字，这些操作看起来像是同步的，代码的执行顺序更直观、易读。

需要注意的是，`await` 关键字只能在异步函数（使用 `async` 关键字修饰的函数）内部使用。在非异步函数中使用 `await` 关键字会导致语法错误。

作业： 给出打印的顺序

```javascript
console.log("桂花鸭围月季红");

setTimeout(() => {
  console.log("陈皮牛肉干");
  new Promise((resolve) => {
    console.log("红椒米熏鸡");
    resolve();
  }).then(() => {
    console.log("八宝菠菜炝鱼片");
  });
}, 0);

new Promise((resolve) => {
  console.log("黄瓜烤去皮虾");
  resolve();
});

Promise.resolve().then(() => {
  console.log("果藕");
  Promise.resolve().then(() => {
    console.log("红烧鱼翅");
  });
  setTimeout(() => {
    console.log("清汤燕菜");
  }, 0);
});

console.log("口蘑白菜卷");

Promise.resolve().then(() => {
  Promise.resolve().then(() => {
    console.log("炸鸡腿");
    Promise.resolve().then(() => {
      console.log("鸡油兰笋");
    });
  });
});

setTimeout(() => {
  console.log("樟茶去骨鸭");
  console.log("烧四素");
}, 0);

Promise.resolve().then(() => {
  console.log("核桃酪");
});

```


