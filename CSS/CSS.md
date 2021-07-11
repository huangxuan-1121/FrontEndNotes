# CSS

**1、盒子模型：**

盒子模型，指用来装页面上元素的矩形区域，CSS盒子模型包括IE盒子模型和W3C盒子模型。

区别在于width、height，IE盒子模型中width包括margin、padding、content

W3C盒子模型中，width只包括content的大小

W3C盒子模型中，有三种

-box-sizing的属性有哪些：

- border-box：宽高应用到margin、padding、content
- padding-box:宽高应用到padding、content
- content-box:宽高只包括content

**2、link标签和@import url()标签的区别**

link标签属于html标签，没有兼容性，@import url属于CSS，需要IE5+

当使用js控制dom去改变样式时，只能使用link标签，因为@import不受dom控制

link除了可以加载css样式以外，还可以定义RSS，定义rel外部连接属性，连接外部资源

页面加载顺序问题：link会在页面加载的同时被加载，@import引用会在加载页面结束后再加载

**3、块级元素和行内元素**

块级元素：会独占一行

行内元素：会在水平方向上排列

其中行内只能包含文本和其他行内元素，块级元素可以包含行内元素和块级元素

在盒模型上，行内元素没有宽高，padding和margin在垂直方向上无效

常见的块级元素有：span、a、

**4、inline-block、inline和block的区别**

block是块级元素，独占一行

inline-block 行内块元素，可以设置宽高，有不换行的属性

inline 宽高无效，margin垂直方向无效，padding有效，前后无换行符

**5、为什么img是inline还可以设置宽高**

img是可替换元素，这类元素展现的效果不是由CSS样式控制的，CSS可以影响可替换元素的位置，但是不可以改变它们的内容，可替换元素有内置的宽高，就如同设置的inline-block一样。

**7、CSS选择器的权重**

!important 权重最大

(>)  内联样式 ( 写在html标签里的)

(>) ID选择器（#id）

(>) 类选择器（.class) = 伪类选择器（:hover等）= 属性选择器[type等]

(>)元素选择器(p、span等) = 伪元素选择器(:after\:before等)

(>)通用选择器（*）

(>)继承样式

原理：

![image-20210530155954714](C:\Users\hx\AppData\Roaming\Typora\typora-user-images\image-20210530155954714.png)

**8、外边距重叠**

规则：

- 相邻的外边距都为正数，取最大值
- 相邻的外边距都为负数，取绝对值最大的值
- 相邻的外边距一正一负，取二者之和

**9、说一下什么是BFC**

块级格式化上下文，是一个独立的块、独立的渲染区域；只有 block-level box 参与，它规定了 block-level box 内部的布局

触发原则：

- float的值不为none
- position的值不为relative或static
- display的值为：inline-block,table-cell,flex,inline-flex,table-caption
- overfloat的值不为visible

布局规则：

- 垂直方向上一个一个放
- 距离由margin决定
- 同一个bfc的元素相邻的会重叠，但是不会和浮动元素重叠
- 计算高度时要计算浮动元素的高度
- 容器内与容器外互不干扰

作用：

- 防止margin重叠
- 清除浮动

**10、伪类和伪元素**

伪类和伪元素都不会出现在源文件和文档树中，也就是说在html源文件中看不到伪类和伪元素。

伪类只是dom中一个元素的某种状态

伪类：同一个标签根据其不同的状态，有不同的样式；伪类用冒号表示，伪类分为静态伪类和动态伪类

- 静态伪类: 只能用于超链接的样式。如下：

1. `:link` 超链接点击之前
2. `:visited` 链接被访问过之后
    以上两种样式，只能用于超链接。

- 动态伪类：针对所有标签都适用的样式。如下：

1. `:hover` “悬停”：鼠标放到标签上的时候
2. `:active` “激活”： 鼠标点击标签，但是不松手时。
3. `:focus` 是某个标签获得焦点时的样式（比如某个输入框获得焦点）

超链接的四种状态：

a标签有4种伪类（即对应四种状态），要求背诵。如下：

1. `:link` “链接”：超链接点击之前
2. `:visited` “访问过的”：链接被访问过之后,不一定是本页面访问过，之前访问过，留在缓存里的都认为是访问过
3. `:hover` “悬停”：鼠标放到标签上的时候
4. `:active` “激活”： 鼠标点击标签，但是不松手时。

常见伪元素：

- :before 
- :after

**11、CSS中inline可以设置padding和margin吗？**

可以设置padding，但是margin在垂直方向上无效。

**12、如何清除浮动?**

当元素设置float时，会脱离文档流向左或向右浮动，这时会造成父元素的高度会塌陷，所以设置浮动完浮动后要给父元素清除浮动。

清除浮动的方法：

- BFC特性

  给父容器加上overfloat: hidden;形成BFC

  缺点：一旦子元素的大小超出父元素的大小会显示异常

- 使用带clear属性的空元素

  在浮动元素之后添加一个不浮动的空元素，这样父元素就必须要考虑子元素的高度。然后还需要在空元素添加一个样式：clear:both;

- 使用伪元素::after

  它会在父元素尾部自动创建一个子元素，原理和空元素一样，把它的height:0;clear:both;display:block;

  缺点：IE6不支持::after;

  ```css
  .father::after{
      height:0;
      display:block;
      clear:both;
      content:'';
  }
  ```

  

**13、元素隐藏的方法**

display:none;  隐藏后不占文档流的位置，资源会加载，dom可以访问

visibility:hidden;	隐藏后空间保留，资源加载，不能点击

opacity：0；	隐藏后空间保留，可以点击

**14、display:none; 和  visibility:hidden; 的区别**

display:none 

- 会引起回流和重绘；
- 隐藏后不占文档流的位置，资源会加载，dom可以访问；
- 子孙节点不可以显示

visibility:hidden 

- 不会引起回流，但会造成重绘；
- 隐藏后空间保留，资源加载，不能点击；
- 子孙节点可以显示

**15、rbga()和opacity的区别**

rgba()作用于元素的颜色和背景颜色

opacity作用于元素中的所有内容，并且带有继承性

**16、outline和border的区别**

outline是轮廓线，自带的；而且outline不占空间，不会像border一样影响元素的尺寸

**17、层叠上下文**

在考虑到多个元素会重合的情况下，W3C提出了重叠的概念，即当两个元素重合时，那个元素在前那个元素在后。

创建方法：position为relative、absolute、fixed的元素设置z-index

顺序：

- 底层的border、background、
- 负z-index
- 块级盒子
- float浮动盒子
- 内联盒子，inline/inline-block
- z-index:auto;或0
- 正z-index

![image-20210530171822582](C:\Users\hx\AppData\Roaming\Typora\typora-user-images\image-20210530171822582.png)

**18、水平居中**

- 利用块级元素撑满的特点，如果宽度已定，使用margin:0 auto;

- flex布局

  ```css
  display:flex;
  justify-content：center;
  ```

- 使用绝对定位absolute

  ```css
  position:absolute;
  left:50%;
  ```

- 利用行内块居中：父级元素设置为text-align:center;子元素display:inline-block;

**19、垂直居中**

- 元素无高度

  利用内边距，让块级文字包裹在padding中，实现垂直居中。

- 父元素高度确定的单行文本：

  使用行高的特性：height=line-height即可。

- 父元素高度确定的多行文本：

  利用vertical-align（只能内联元素）如果是div，可以设置为table和table-cell。

  ```css
  display：table-cell;
  vertical-align:center;
  ```

- 父元素高度未知：

- 使用绝对定位absolute

  position:absolute;

  top:50%;

- flex布局

  ```css
  display:flex;
  align-items：center;
  ```

  

**20、水平垂直居中**

- flex布局

```css
child{
    display:flex;
    align-items:center;
    justify-content:center;
}
```

**21、两栏布局**



**22、三栏布局**



**23、flex布局**



**24、css动画如何实现**



**25、transition和animation的区别**

animation

**26、img和background-image的区别**

img属于html标签，background-image属于css。
