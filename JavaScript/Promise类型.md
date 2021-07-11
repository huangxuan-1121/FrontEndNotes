# Promise类型

ES6新增的引用类型Promise，解决异步编程。可以通过new实例化创建，创建时需要传入**执行器函数**作为参数。

异步：操作之间没什么关系，可以同时进行多个操作。如ajax请求。

同步：同时只能做一件事。

promise（期约），是一个有状态的对象，期约的状态代表期约是否完成，其可能存在如下3种状态之一：

1. 待定(pending)：表示尚未开始或正在执行中，初始化。
2. 兑现(fulfilled,有时也称为解决，resolved)：表示已经成功完成。
3. 拒绝(rejected)：表示没有成功完成。

待定是Promise的初始状态，可以通过此状态转换成兑现（fulfilled）状态或拒绝状态，且此转换是不可逆的。也不能保证Promise会脱离某种状态，所以在设置Promise的状态时必须要具有恰当的行为。

那么问题来，如何控制Promise的状态？

通过执行器函数控制Promise的状态。

由于Promise的状态是私有的，只能在内部进行操作，内部操作在Promise的执行器函数中完成。执行器函数主要有两项职责：初始化Promise的异步行为和控制期约的最终转换。其中，控制期约状态转换是通过调用它的两个函数参数实现，这两个函数通常命名为resolve()和reject()。调用resolve()会切换为兑现状态，调用reject()会切换为拒绝状态，但有时也会抛出错误。

但是，无论resolve()或reject()谁被调用，状态转换都不可撤销。继续修改状态会失败。

```javascript
let p = new Promise((resolve,reject)=>{
    resolve();
    reject(); //没效果
});
```

**Promise.resolve()** 通过调用Promise.resolve()方法可以实例化一个解决的Promise

```js
let p1 = Promise.resolve();
let p2 = new Promise((resolve,reject)=>resolve());
```

传给Promise.resolve()的第一个参数意味着解决Promise的值。有且只有一个，多余的参数会被忽略。

**Promise.reject**() 通过调用Promise.resolve()方法可以实例化一个拒绝的Promise并抛出一个错误，并且这个错误不能用cry/catch捕捉到，只能通过拒绝处理程序捕获。

```js
let p1 = Promise.reject();
let p2 = new Promise((resolve,reject)=>reject());
```

拒绝Promise的理由就是传给Promise.reject()的第一个参数，这个参数也会传给后续的拒绝程序。有且只有一个，多余的参数会被忽略。

#### **Promise** 的实例方法

Promise的实例方法是连接外部同步代码和内部异步代码的桥梁，

2. Promise.prototype.then()

   Promise.prototype.then()是为Promise添加处理程序的主要方法。then()方法最多添加两个参数：onResolved处理程序和onRejected处理程序。这两个参数都是可选的，如果提供，会在Promise分别进入“resolve”和“reject”状态时执行。

   ```js
   //使用then方法
   let p = new Promise((resolve,reject)=>{
       setTimeout(()=>{
           resolve('一些数据');
           //reject('错误');
       },1000);
   });
   //调用then方法
   p.then((value)=>{
       console.log(value);
   },(reason)=>{
       console.err(reason);
   });
   //控制台输出：一些数据
   ```

   

3. Promise.prototype.catch()

   Promise.prototype.catch()方法用于给Promise添加拒绝处理程序（用于指定Promise对象失败的回调）。此方法只接收一个参数：onRejected处理程序，语法糖，调用它相当于调用Promise.prototype.then(null,onRejected)。

   ```js
   //使用catch方法
   let p = new Promise((resolve,reject)=>{
       setTimeout(()=>{
           //resolve('一些数据');
           reject('错误');
       },1000);
   });
   //调用catch方法
   p.catch((reason)=>{
       console.err(reason);
   });
   //控制台输出：错误期约合成
   ```

#### Promise合成  

Promise类提供两个将多个Promise实例组合成一个Promise的静态方法：Promise.all()和Promise.race()。

1. Promise.all()

   Promise.all()静态方法创建的Promise会在一组Promise全部解决后再解决。此方法接收一个可迭代的对象，一般为数组，返回一个新Promise。

   ```js
   let p1 = new Promise((resolve,reject)=>{});
   let p2 = new Promise((resolve,reject)=>{});
   let p3 = new Promise((resolve,reject)=>{});
   //使用Promise.all.()，将以上三个Promise对象合成一个
   let p = new Promise.all([p1,p2,p3]);
   p.then(data=>{
       //p1,p2,p3都成功，才会调用p的then方法
   }).catch(reason=>{
       //只有有一个失败，就会调用p的catch方法
   })
   //可以理解为p1&&p2&&p3
   ```

   

2. Promise.race()

   Promise.race()静态方法返回一个包装好的Promise，是一组集合中最先解决或拒绝Promise的镜像。此方法接收一个可迭代的对象，一般为数组，返回一个新Promise。

   ```js
   //应用场景：给某个异步请求设置超时时间，并且在超时后执行相应的操作。
   //1、请求某资源
   function requestSome(Src){
       return new Promise((resolve,reject)=>{
           //获取资源操作
       });
   }
   function time(){
       return new Promise((resolve,reject)=>{
           setTimeout(()=>{
               reject('请求超时');
           },5000);
       });
   }
   Promise.race([requestSome('xx/xx'),time()]).then(data=>{
     //如果5s后，requestSome('xx/xx')里的资源请求成功，则会执行  
   }).catch(reason=>{
       //否则，超时
   })
   //也就是说，只要Promise.race()的第一个参数的Promise成功了，就不会走第二个参数的Promise；
   //看谁执行的快then就回调谁的结果。可以理解为p1||p2||p3
   ```

#### 手写Promise

```js
const PENDING='pending'
const RESOLVED='resolved'
const REJECTED='rejected'

function MyPromise(fn){
    const that =this
    that.state=PENDING
    that.value=null
    that.resolvedCallbacks=[]
    that.rejectedCallbacks=[]
    //待完善resolve和reject
    //待完善执行fn
}
```

- 首先创建了三个常量用于表示状态

- 在函数体内部首先创建了常量that，因为代码可能会异步执行，用于获取正确的 this 对象
- 起始Promise状态为pending
- value用于保存resolve或者reject中传的值
- resolvedCallbacks和rejectedCallbacks用于保存then中的回调，因为当执行完Promise时状态能还是等待中，这时候应该把then中的回调保存起来用于状态改变时使用