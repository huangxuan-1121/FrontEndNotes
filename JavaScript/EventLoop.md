# EventLoop

JavaScript是单线程脚本语言，也就是说当一行代码执行时，必然不会同时执行另一行代码。

Event Loop（事件循环），指浏览器或node的一种解决JavaScript单线程运行时不会阻塞的一种机制的。

### 浏览器中的Event Loop

当我们执行JS代码时，实际上就是往执行栈中放入函数。

在JavaScript中，不同的任务源会被分配到不同的任务队列中，源任务被分为两种，分别是宏任务（MacroTask）和微任务（MicroTask）

宏任务包括script全部代码、setTimeout、setInterval、setImmediate、I/O，UI rendering。

微任务包括promise、MutationObserver、process.nextTick(Node独有)。

#### **宏任务和微任务的区别**

- 宏队列可以有多个，微任务队列只有一个,所以每创建一个新的setTimeout都是一个新的宏任务队列，执行完一个宏任务队列后，都要检查微任务队列。
- 一个事件循环后，微任务队列执行完了，再执行宏任务队列
- 一个事件循环中，在执行完一个宏队列之后，就会去check 微任务队列

#### 一个栗子：

```js
//测试版本 Google Chrome	89.0.4389.114
<script>
        console.log('script start')

        async function async1() {
            await async2()
            console.log('async1 end')
        }
        async function async2() {
            console.log('async2 end')
        }
        async1()

        setTimeout(function () {
            console.log('setTimeout')
        }, 0)

        new Promise(resolve => {
                console.log('Promise')
                resolve()
            })
            .then(function () {
                console.log('promise1')
            })
            .then(function () {
                console.log('promise2')
            })

        console.log('script end')
    </script>

/*输出结果：

script start
async2 end
Promise
script end
async1 end
promise1
promise2
setTimeout

*/
```

- 整体script作为第一个宏任务进入主线程
- 将console.log('script start')压入调用栈中，执行并***输出script start***，而后栈为空
- 将async1压入栈中，遇到 await async2()
- 将async2压入栈中，遇到 console.log('async2 end')，执行并***输出async2 end***。并且函数返回一个 `Promise`，然后把剩下的 `async1` 函数中的操作放到 then 回调函数中(异步操作，放入微任务队列中)。所以我们完全可以把 `await` 看成是**让出线程**的标志。
- 遇到setTimeout，放入新的宏任务队列中（第二个宏任务）
- 继续执行同步代码，***输出Promise***和***script end***，将`then`函数放入**微任务**队列中等待执行。
- 同步执行完成之后，检查**微任务**队列是否为空，然后按照先入先出规则，依次执行。
- 微任务不为空，先执行async1放在 then 回调函数中的内容，***输出async1 end*** ，然后执行promise的then函数 ***输出promise1***,此时then的回调函数返回undefinde，此时又有then的链式调用，又放入**微任务**队列中，再次***输出promise2***。
- 微任务队列为空，第一轮事件循环结束。
- 发现setTimeout宏任务，第二轮事件循环开始，***输出setTimeout***
- 检查微任务队列，为空，循环结束。

注意，并不是微任务快于宏任务。而是宏任务中包括了 `script` ，浏览器会**先执行一个宏任务**，接下来有异步代码的话才会先执行微任务。

Event Loop执行顺序：

- 首先执行同步代码，属于宏任务
- 当执行完所有同步代码后，执行栈为空，然后检查异步代码
- 执行所有微任务，即将异步事件添加到主线程的执行栈中
- 执行完所有微任务后，如有必要渲染页面则渲染页面
- 执行宏任务，开始下一轮Event Loop

简而言之，就是JS只有一个主线程，主线程执行完执行栈的任务后去检查异步的任务队列，如果异步事件触发，则将其加到主线程的执行栈。

ps:随处可见的例子，但一定要用自己的思路写一遍过程，加深理解Event Loop。

参考文章：

《[一篇搞定（Js异步、事件循环与消息队列、微任务与宏任务）](https://zhuanlan.zhihu.com/p/139967525)》

《[微任务、宏任务与Event-Loop](https://segmentfault.com/a/1190000016022069)》

《[从event loop到async await来了解事件循环机制](https://juejin.cn/post/6844903740667854861)》

《[从event loop到async await来了解事件循环机制](https://juejin.cn/post/6844903740667854861)》

