## 关于this

[TOC]



### 为什么要使用this？

this提供了一种隐式传递的方法来 **传递** 一个对象的引用

>  this既不指向函数自身，也不指向词法作用域。this实际上是在函数被调用时发生的绑定，它指向什么完全取决于函数在哪里**调用**（调用位置并不等于声明位置）。

### 调用位置

首先要理解**调用栈**和**调用位置**

调用栈：就是为了到达当前执行执行位置所调用的所有函数。

调用位置：就在当前执行函数的前一个调用

```js
function baz(){
    //当前调用栈：bar
    //因此当前调用位置是：全局作用域中
    console.log("baz");
    bar();	//-->bar的调用位置
}
function bar(){
    //当前调用栈：baz->bar
    //因此当前调用位置是：baz中
    console.log("bar");
    foo(); // -->foo的调用位置
}
function foo(){
    //当前调用栈：baz->bar->foo
    //因此当前调用位置是：bar中
    console.log("foo");
}
baz(); // -->bar的调用位置
```

### 绑定规则

##### 默认绑定

适用于**独立函数调用**或 无法运用其他绑定规则时使用

- 严格模式下：this会被绑定到undefined

- 非严格模式下：this默认绑定到全局模式

##### 隐式绑定

适用于调用位置**有上下文对象**，或者说是否被某个对象拥有或包含

```js
function foo(){
    console.log(this.a);
}
var obj = {
    a:2,
    foo:foo
};
obj.foo(); //2
```

隐式绑定规则：会把函数调用中的this绑定到这个上下文对象

缺点：

- 隐式丢失，被隐式绑定的函数会丢失绑定的对象，即应用默认规则
- 必须在一个对象内部包含一个指向函数的属性，并通过这个属性间接引用函数，从而把this间接（隐式)绑定到这个对象上。

##### 显式绑定

使用call(..)和apply(..)方法，它们的第一个参数是指定this的绑定对象，因为可以直接指定，所以我们称之为显式绑定。

```js
function foo(){
    console.log(this.a);
}
var obj = {
    a:2
};
foo.call(obj); //2
//通过foo.call(obj);可以在调用foo时强制把它的this绑定在obj上。
```

但是，显式绑定还是无法解决绑定丢失的问题

###### 硬绑定

**可以解决绑定丢失的问题**

实现：

```js
function bind(fn,obj){
    return function(){
        return fn.apply(obj,arguments);
    };
}
```

另：ES5还提供了Function.prototype.bind()方法

###### new绑定

```js
function foo(a){
    this.a = a;
}
var bar = new foo(2);
console.log(bar.a); // 2
```

##### 各类绑定的优先级

- 显式绑定优先级高于隐式绑定
- new绑定优先级高于隐式绑定

### 小结

​	1、如果想要判断一个运行中函数的this绑定，就需要找到这个函数的直接调用位置。而后通过顺序应用下面四条规则来判断this的绑定：

- 由new调用，绑定到新创建的对象
- 由call、apply指定，绑定到指定对象
- 由上下文对象调用，绑定到那个上下文对象
- 默认：严格模式下this绑定到undefined，非严格模式下this绑定到全局对象

2、如果想要安全的使用this的默认绑定规则，可以使用一个DMZ对象，如  n = object.create(null);  以保护全局对象

3、ES6中的箭头函数不会使用第一点中的四条绑定规则，而是根据当前的词法作用域来决定this，具体来说，箭头函数回继承外层函数调用的this绑定。

