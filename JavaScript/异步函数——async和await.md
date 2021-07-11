# 异步函数——async/await

ES8中的async/await旨在解决利用异步结构组织代码的问题。

### async函数

- async函数返回的是一个Promise对象

```javascript
async function test(){
    return '123';
}
const res = test();
console.log(res);
//控制台输出：
```

![image-20210404162540309](C:\Users\hx\Desktop\image-20210404162540309.png)

### await函数

- await必须要放在async函数中

- await返回的是Promise成功的值

- await的promise失败，就会抛出异常，需要try-catch捕获并处理

  ```javascript
  const p = new Promise((resolve,reject)=>{
    		resolve('成功！');  
  })
  async function test(){
      let res = await p;
      console.log(res);
  }
  test();//调用函数
  //控制台输出：成功！
  ```

  ```JavaScript
  //await的premise失败，需要用try-catch捕获
  const p = new Promise((resolve,reject)=>{
    		reject('失败！');  
  })
  async function test(){
      try{
          let res = await p;
      	console.log(res);
      } catch(e){
        console.log(e);  
      }
  }
  test();//调用函数
  //控制台输出：失败！
  ```

### async/await结合使用

```javascript
//
function request1(){
    return new Promise((resolve,reject)=>{
        //读取资源代码
    })
}
function request2(){
    return new Promise((resolve,reject)=>{
        //读取资源代码
    })
}
function request3(){
    return new Promise((resolve,reject)=>{
        //读取资源代码
    })
}
//以上三个函数返回结果都是Promise对象
//声明一个async函数
async function test(){
    //获取所读取到的内容
    const result1 = await request1();
    const result2 = await request2();
    const result3 = await request3();
    console.log(result1);
    console.log(result2);
    console.log(result3);
}
//调用
test();
```

从上可以看出async/await 的优势在于处理then链