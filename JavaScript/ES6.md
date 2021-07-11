# ES6

## **变量提升与var、let和const**

- 变量提升

  使用var声明的变量会被提升到作用域的顶部

  提上存在的原因就是为了解决函数间相互调用的情况

  ```js
  function t1(){
      t2()
  }
  function t2(){
      t1()
  }
  t1();
  //如果不存在提升这个情况就无法实现上述代码，因为不可能存在t1在t2前面，然后t2又在t1前面
  ```

  

- let

  - let声明的变量只在let命令所在的代码块内有效
  - 不存在变量提升
  - 存在`暂时性死区`，只要块级作用域存在let命令，它所声明的变量就“绑定”这个区域，不受外部影响
  - 不允许重复声明，即在相同的作用域内，重复声明一个变量

- const

  - const声明的是常量，一旦声明，常量的值就不能改变
  - 其他特性与let相似或一致

  **小结：**

  - 函数提升优于变量提升，函数提升会把整个函数挪到作用域顶部，变量提升只会把`声明`挪到作用域顶部
  - var存在变量提升，所以可以在声明前使用，let和const因为暂时性死区的原因，不能在声明前使用
  - var在全局作用于先声明后会挂载在`window`上，let和const不会
  - let和const的作用基本一致，但是后者声明的变量不能再赋值

**2、原型继承和class继承**



**3、模块化**



**4、Proxy**



**5、map、filter、reduce**



## 数组的扩展

### Array.from()

-- 用于将类数组转换为真正的数组

```js
function foo(){
	let args = Array.from(arguments);
	//...
}
```

### Arryy.of()

-- 用于将一组值转换为数组

```js
Array.of(3,11,4,5,66); //[3,11,4,5,66]
Array.of(3); //[ , , ,]
```

### 数组实例的copyWithin()

-- 数组实例的copyWithin()方法，在当前数组内部，将指定位置的成员负责到其他位置，会覆盖原有成员，然后返回当前数组，也就是说，使用这个方法，会改变当前数组。

```js
Array.prototype.copyWithin(target,start=0,end=this.length);
```

它接受三个参数：

- target（必需）：从该位置开始替换数据。 

- start（可选）：从该位置开始读取数据，默认为0。如果为负值，表示倒数。 

- end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示倒数。

### 数组实例的find()和findIndex()

-- find()方法，找出第一个符合条件的数组成员，它的参数是一个回调函数，所有数组成员依次执行该回调函数，指导找出第一个返回值为true的成员。没有符合条件的成员则返回undefined

```js
[1,4,-4,10].find((n) => n<0); //-5
```

-- findIndex()方法，返回第一个符合条件的成员，如果都不符合则返回-1（find()方法是返回undefined）

### 数组的实例fill()

-- fill()方法使用给定值，填充一个数组

```js
['a','b','c'].fill(7); 
//[7,7,7]
new Array(3).fill(7);
//[7,7,7]

['a','b','c'].fill(7,1,2);
//['a',7,'c']
```

### 数组实例的entries(),keys()和values()

ES6提供三个新的方法 --entries(),keys()和values()用于遍历数组。它们都返回一个遍历对象，可以用for...of循环遍历，区别是keys()是对键名的遍历，values()是对键值的遍历，entries()是键值对的遍历。

```js
for(let index of ['a','b'].keys()){
	console.log(index);
}
//0
//1
for(let elem of ['a','b'].values()){
	console.log(elem);
}
//a
//b
for(let [index,elem] of ['a','b'].entries()){
	console.log(index,elem);
}
//0 "a"
//1 "b"
```

### 数组实例的includes()

-- 该方法返回一个布尔值，表示某个数组是否存在包含给定的值，与字符串的includes()类似。

该方法的第二个参数表示搜索的起始位置，默认为0，若为负数，则表示倒数的位置

```js
[1,2,3].includes(2);  //true
[1,2,3].includes(3,3); //false
```

## 函数的扩展

### 扩展运算符

含义：用于将一个数组转换为用逗号分隔的参数序列

```js
console.log(...[1,2,3]);
//1 2 3

```

### 箭头函数

#### 使用注意点：

- 在箭头函数中，this指向是固定的，它总是指向函数所在的对象
- 箭头函数没有arguments对象，但是可以用Rest代替
- 不可以当作构造函数
- 不可以使用yield命令

#### 函数绑定

箭头函数可以绑定this对象，减少了显示绑定this对象的写法（call、bind、apply)，但是箭头函数不适合所有场合，于是ES7提出‘函数绑定’运算符，来取代call、bind、apply。

函数绑定运算符是并排的两个双冒号(::),该运算符左边是一个对象，右边是一个函数。该运算符会自动把左边的对象作为上下文环境（即this对象）绑定到右边的函数上面。

## 对象的扩展

#### Object.is()

Object.is()用来比较两个值是否严格相等，它与严格比较运算符（===）的行为基本一致。

```js
Object.is('foo','foo');
//true
Object.is({},{});
//false
```

![image-20210604144041043](C:\Users\hx\AppData\Roaming\Typora\typora-user-images\image-20210604144041043.png)

#### Object.assign()

Object.assign()用来将源对象的所有可枚举属性，**复制**到目标对象（target）。它至少需要两 个对象作为参数，第一个参数是目标对象，后面的参数都是源对象。只要有一个参数不是对象，就会抛出 TypeError错误。

![image-20210604144353460](C:\Users\hx\AppData\Roaming\Typora\typora-user-images\image-20210604144353460.png)

##### 用处：

- 为对象添加属性

  ![image-20210604144451885](C:\Users\hx\AppData\Roaming\Typora\typora-user-images\image-20210604144451885.png)

- 为对象添加方法

- 克隆对象
- 合并多个对象
- 为属性指定默认值

#### ES6一共有6种方法可以遍历对象的属性：

![image-20210604144722488](C:\Users\hx\AppData\Roaming\Typora\typora-user-images\image-20210604144722488.png)

## Symbol

ES6引入了新的原始数据类型Symbol，表示独一无二的值，它是js语言中的第七种数据类型，前六种是：Number、null、Boolean、String、Object、undefined

它由Symbol函数产生

```js
let s = Symbol();
typeof s; //symbol
```

注意，	`Symbol`函数前不能使用`new `命令，否则会报错。这是因为生成的Symbol是一个原始类型的值，**不是对象**。也就是说，由于Symbol值不是对象，所以不能添加属性。基本上，它是一种**类似于字符串的数据类型**。

## Proxy概述	 p177

Proxy用于修改某些操作的默认行为，等同为在语言层面做出修改，即对编程语言进行修改。

用来自定义**对象**中的操作

```js
let p = new Proxy(target,handler);
```

![image-20210604150850821](C:\Users\hx\AppData\Roaming\Typora\typora-user-images\image-20210604150850821.png)

`target`代表需要添加代理的对象，`handler `用来定义对象中的操作，比如用来自定义`set`和`get`函数。

接下来我们通过 `Proxy` 来实现一个数据响应式

```js
let onWatch = (obj, setBind, getLogger) => {
  let handler = {
    get(target, property, receiver) {
      getLogger(target, property)
      return Reflect.get(target, property, receiver)
    },
    set(target, property, value, receiver) {
      setBind(value, property)
      return Reflect.set(target, property, value)
    }
  }
  return new Proxy(obj, handler)
}

let obj = { a: 1 }
let p = onWatch(
  obj,
  (v, property) => {
    console.log(`监听到属性${property}改变为${v}`)
  },
  (target, property) => {
    console.log(`'${property}' = ${target[property]}`)
  }
)
p.a = 2 // 监听到属性a改变
p.a // 'a' = 2
```

在上述代码中，我们通过自定义 `set` 和 `get` 函数的方式，在原本的逻辑中插入了我们的函数逻辑，实现了在对对象任何属性进行读写时发出通知。

当然这是简单版的响应式实现，如果需要实现一个 Vue 中的响应式，需要我们在 `get` 中收集依赖，在 `set` 派发更新，之所以 Vue3.0 要使用 `Proxy`替换原本的 API 原因在于 `Proxy` 无需一层层递归为每个属性添加代理，一次即可完成以上操作，性能上更好，并且原本的实现有一些数据更新不能监听到，但是 `Proxy` 可以完美监听到任何方式的数据改变，唯一缺陷可能就是浏览器的兼容性不好了。

## Set和Map数据结构   	P218

### Map

es6提供新的数据结构，用于保存键值对，并且任何类型的值都可以作为键值

#### Map和Object的区别

- map可以保存任何类型的键值，object只能以字符串或者Symbols作为键值
- map的键值对个数可以用size属性直接获取，object只能循环计算
- object都有自己的原型，原型链上的键名有可能和你自己再对象上设置的键名重复

#### Map的属性

get(key)、set(ket,val)、size() has(key) delete(key) clear()

#### Map的遍历方法

key()  value()  entries()  forEach()

forEach栗子

```js
let mp = new Map([['a',1],['b',2]]);
for(let [key,value] of mp){
    console.log(key,value)l
}
```

#### Map和其他类型互换

- 转换数组：[...mp]

- 转换对象：

  ```js
  let o = {}；
  let mp = new Map();
  for(let [key,value] of mp){
      o[key] = value;
  }
  console.log(o);
  ```

### Set

ES6提供了新的数据结构Set，它类似于数组，但是成员的值都必须是唯一的，没有重复的值。

![image-20210604161248906](C:\Users\hx\AppData\Roaming\Typora\typora-user-images\image-20210604161248906.png)

`map` 作用是生成一个新数组，遍历原数组，将每个元素拿出来做一些变换然后放入到新的数组中。

```js
[1, 2, 3].map(v => v + 1) // -> [2, 3, 4]
```

另外 `map` 的回调函数接受三个参数，分别是当前索引元素，索引，原数组

```js
['1','2','3'].map(parseInt)
```

- 第一轮遍历 `parseInt('1', 0) -> 1`
- 第二轮遍历 `parseInt('2', 1) -> NaN`
- 第三轮遍历 `parseInt('3', 2) -> NaN`

`filter` 的作用也是生成一个新数组，在遍历数组的时候将返回值为 `true`的元素放入新数组，我们可以利用这个函数删除一些不需要的元素

```js
let array = [1, 2, 4, 6]
let newArray = array.filter(item => item !== 6)
console.log(newArray) // [1, 2, 4]
```

和 `map` 一样，`filter` 的回调函数也接受三个参数，用处也相同。

最后我们来讲解 `reduce` 这块的内容，同时也是最难理解的一块内容。`reduce` 可以将数组中的元素通过回调函数最终转换为一个值。

如果我们想实现一个功能将函数里的元素全部相加得到一个值，可能会这样写代码

```js
const arr = [1, 2, 3]
let total = 0
for (let i = 0; i < arr.length; i++) {
  total += arr[i]
}
console.log(total) //6 
```

但是如果我们使用 `reduce` 的话就可以将遍历部分的代码优化为一行代码

```js
const arr = [1, 2, 3]
const sum = arr.reduce((acc, current) => acc + current, 0)
console.log(sum)
```

对于 `reduce` 来说，它接受两个参数，分别是回调函数和初始值，接下来我们来分解上述代码中 `reduce` 的过程

- 首先初始值为 `0`，该值会在执行第一次回调函数时作为第一个参数传入
- 回调函数接受四个参数，分别为累计值、当前元素、当前索引、原数组，后三者想必大家都可以明白作用，这里着重分析第一个参数
- 在一次执行回调函数时，当前值和初始值相加得出结果 `1`，该结果会在第二次执行回调函数时当做第一个参数传入
- 所以在第二次执行回调函数时，相加的值就分别是 `1` 和 `2`，以此类推，循环结束后得到结果 `6`

想必通过以上的解析大家应该明白 `reduce` 是如何通过回调函数将所有元素最终转换为一个值的，当然 `reduce` 还可以实现很多功能，接下来我们就通过 `reduce` 来实现 `map` 函数

```js
const arr = [1, 2, 3]
const mapArray = arr.map(value => value * 2)
const reduceArray = arr.reduce((acc, current) => {
  acc.push(current * 2)
  return acc
}, [])
console.log(mapArray, reduceArray) // [2, 4, 6]
```

## Generator 函数

