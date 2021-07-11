JS数据类型判断

- typeof

  返回一个表示数据类型的字符串，返回结果包括number、string、boolean、object、undefined。（typeof null === "object"  true）

- instanceof

  可知道某个对象的具体类型，判断左操作数对象的原型上是否有右边这个构造函数的prototype属性，也就是说是否是某个构造函数的实例。

  instanceof只适用于对象，不适用原始类型的值

- constructor

  可得知某个实例对象是哪个构造函数产生的。

  但constructor属性易变，不可信赖

- Object.prototype.toString

  toString是Object原型对象上的一个方法，该方法默认返回其调用者的具体类型，更严格的讲，返回的是 toString运行时this指向的对象类型

  ```
  Object.prototype.toString.call(new Function()); // [object Function]
  ```

  

