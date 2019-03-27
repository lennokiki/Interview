# 前端面试常见问题汇总1

## CSS

### 1. 简单介绍一下盒子模型，标准盒模型和非标准盒模型有什么区别？

  * 所有的HTML元素可以看做盒子，在css里面主要用来做布局。盒模型本质上是一个盒子，从外到内它包括：外边距(margin)、边框(border)、内边距(padding)、内容(content)。标准的盒模型内容区域的宽高就是真正的宽高，非标准的盒模型border + padding + content的宽高 才是真正设置的宽高


### 2. 如何将标准盒子模型变为非标准盒子模型 -> box-sizing: border-box； box-sizing还有哪几个属性值，分别是什么？

  * content-box: 默认值，即标准的盒子模型
  * inherit: 规定应从父元素继承 box-sizing 属性的值。


### 3. css常用选择器有哪些,选择器的优先级的顺序是怎样的？

  * id选择器(#)、
  * class选择器(.)、
  * 标签选择器(div、h1、span...)、
  * 相邻选择器(h1 + p)
  * 子代选择器(div > p)
  * 后台选择器(div p)
  * 通配符选择器(*)
  * 属性选择器(a[rel = "out"])
  * 伪类选择器(a:hover、 a:nth-child(1))
  * !important > 行内样式 > id > class > 标签


### 4. h1标签渲染的颜色是什么？

  ```
  # html

  <h1 class="green red">

  # xxx.css

  .red { color: red; }
  .green { color: green; } 
  ```
 
  * green绿色，对于一个元素有多个class属性，当class属性内有相同的属性值的时候，覆盖的规则和class在标签内出现的先后顺序无关，覆盖规则参考权重优先级，相同情况下，css文件内后面定义的会覆盖前者


### 5. position有哪几个属性值？relative 和 absolute 分别相对于谁进行定位？

  * static， 默认值，没有定位，元素出现在正常的文档流中
  * fixed, 固定定位，相对于浏览器的窗口进行定位
  * relative, 相对定位，相对于元素自身在当前文档流中的位置进行定位
  * absolute, 绝对定位，相对于 static 以外的第一个祖先元素进行定位
  * inherit, 规定从父元素继承 position 属性的值


### 6. display里面 block 和 inline 的区别？

#### block元素的特点：
  * ①处于常规流中时，如果width未设置，会自动填充满父级容器
  * ②可以应用margin/padding
  * ③在不设置高度的情况下会扩展高度以包含常规流中的子元素
  * ④处于常规流中布局时在前后元素之间，独占一个水平空间

#### inline元素特点： 
  * ①水平方向上会根据direction一次布局
  * ②不会在元素的前后进行换行
  * ③受white-space控制
  * ④margin 和 padding在竖直方向上无效，水平方向上有效
  * ⑤width 和 height 属性对非替换的行内元素无效（如span、a），宽度由元素内容决定
  * ⑥非替换行内元素的行高由line-height决定，替换的行内元素（img、input)行框高由height、margin、padding、border决定
  * ⑦浮动或者绝对定位或者固定定位的时候会转换为block


### 7. 什么是bfc规则（Block formatting contexts)？

#### w3c规范中的定义：
  * 浮动元素和绝对定位元素，非块级盒子的块级容器（例如inline-block, table-cells, 和table-captions） 以及overflow值不为“visible”的块级
  盒子，都会为他们的内容创建新的BFC(块级格式上下文)
  * 在BFC中，盒子从顶端开始垂直地一个接一个地排列，两个盒子之间的垂直的间隙是由他们的margin值所决定的。在一个BFC中，两个相邻的块级盒子的垂直外边距会发生折叠。
#### 通俗的理解：
  * 可以将BFC理解为一个概念，它是一个独立的布局环境，它是由零碎的规范来展示和约束我们所编写的布局。可以将它理解为一个箱子，箱子里面的物品的摆放是不受外界所影响的。在一个BFC中，所有的块盒和行盒都会垂直的沿着其父元素的外边框排列。
  * 什么情况下会产生折叠？
  * ①必须是同一个BFC下，位于常规文档流中的块级盒子。
  * ②折叠只会发生在垂直方向上

  * ③父元素的margin-top 与 其常规文档流中的第一个子元素的margin-top, 并且父级元素不设置border padding-top 且两者之间不存在间隔
  * ④兄弟元素之间，上一个元素的margin-bottom 和 下一个元素的margin-top
  * ⑤没有明确设置高度的父元素的margin-bottom 与 其常规文档流中最后一个子元素的 margin-bottom
  * 相邻元素折叠的结果？
  * ①两个外边距都是正数时，折叠结果是它们两者之间较大的值
  * ②两个外边距都是负数时，折叠结果是两者绝对值较大的值
  * ③两个外边距是一正一负，折叠结果是两者相加的和
#### 举几个布局中常遇到的问题来说明：
##### 1. 子级元素应用margin-top造成父级元素塌陷
  ```
    # html

    <header>
        <p class="title">Title BFC</p>
    </header>

    # css

    .title {
      margin-top: 20px;
    }
  ```
  * header中的p标签设置了margin-top之后，页面上并没有改变距离header顶部的margin-top，反而是header的产生了距离顶部的20px。
  * 产生的原因，css在盒模型中规定
  ```
  In this specification, the expression collapsing margins means that adjoining margins (no non-empty content, padding or border areas or clearance separate them) of two or more boxes (which may be next to one another or nested) combine to form a single margin. 所有毗邻的两个或更多盒元素的margin将会合并为一个margin共享之。毗邻的定义为：同级或者嵌套的盒元素，并且它们之间没有非空内容、Padding或Border分隔。

  # 原文：https://blog.csdn.net/hahhahahaa/article/details/80676873 
  ```
  * 毗邻的意思就是接壤，挨着的。因为嵌套的父元素和第一个子元素也属于毗邻，也会应用margin共享的规则，所以父级元素header的margin-top： 0 和子元素p的 margin-top: 20px 最终合并为了20px，并且两个元素共享，最终产生了上述的表现。
  * 如何解决呢？ 对父元素添加透明border 或者 padding-top, 或者为父元素创建一个新的bfc。

##### 2. 利用BFC消除外部浮动对内部元素影响
  ```
    # html

    <div class="box">
      <img class="img" src="..."/>
      <p class="info">xxxxxxxx</p>
    </div>

    # css
    .box {
      width: 200px;
      float: left;
    }
    .img {
      width: 100px;
      height: 100px;
      float: left
    }
  ```
  * 当p标签内的文字过多时就会发生常见的图文混排布局，解决方法就是为p标签创建一个新的bfc上下文
  
  ```
  # css

  .info {
    overflow: hidden;
  }
  ```

  * 这里还有一个bfc的特性，计算bfc高度的时候，浮动元素也参与计算。对于float的元素它是脱离当前文档流的即不在占位，当父级元素没有设置固定高度 或者 也没有创建一个新的bfc的时候，当子级只有一个元素并且该元素设置了float定位，那么父级的元素的高度是无法被撑开的，所以这个例子里在box盒子上也添加了一个float，这样内部浮动的元素也会参与到box高度的计算中，box的高度就会做到自适应。

### 8. 什么是圣杯布局？实现一个圣杯布局？实现的原理是什么？

  * 圣杯布局是将内容分为左中右三部分，左右盒子宽度固定，中间盒子宽度自适应
  ```
  # html

  <div class="container">
    <div class="left"> left </div>
    <div class="middle"> middle </div>
    <div class="right"> right </div>
  </div>

  # css

  .container {
    padding: 0 200px;
    overflow: hidden;
  }
  .left, .middle, .right {
    float: left;
    position: relative;
    height: 300px;
  }
  .middle {
    width: 100%;
    background: green;
  }
  .left {
    width: 200px;
    background: navy;
    margin-left: -100%; // 将left盒子上移 并且 左移一个middle的宽度 此时left的左边框和middle的左边框相邻
    left: -200px; // 将left盒子在左移200px 正好占据container padding-left的位置
  }
  right {
    width: 200px;
    background: fuchsia;
    margin-left: -200px; // 将right盒子上移 此时 right盒子的右边框和middle的右边框相邻
    left: 200px; // 将right盒子在右移200px 正好占据container padding-right的位置
  }

  ```
  
  * 原理就是利用当浮动的元素使用了margin-left为负值，并且负值的距离大于或者等于了自身的宽度时，这个浮动的元素就不再占据空间位置，会浮动到当前上下文中的最顶部，按照先后顺序，后者覆盖前者。同时负值还会产生位移的效果，再结合相对定位，正好将左右两个盒子补充到父级元素的左右padding中，从而实现中间盒子宽度的自由伸缩的布局效果

### 9. flex布局如何实现水平垂直居中一个元素？justify-content都有哪些属性值？

  ```
    # html

    <div class="box">
      <div>123</div>
    </div>

    # css

    .box {
      display: flex;
      align-items: center; // 使子元素垂直居中
      jutsify-content: center; // 使子元素水平居中
    }

  ```

  * flex-start, 交叉轴的起点对齐 （默认）
  * flex-end, 交叉轴的终点对齐 
  * center, 交叉轴的中点对齐
  * space-between, 两端对齐，项目之间的间隔都相等
  * space-around, 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍

### 10. link 和 @import 的区别？

  * link属于HTML标签，而@import是CSS提供的; 
  * 页面被加载的时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载;
  * @import只在IE5以上才能识别，而link是HTML标签，无兼容问题; 
  * link方式的样式的权重 高于@import的权重

### 11. 常用的清除浮动有哪几种方式？

  * 父级元素添加 overflow: hidden;
  * 父级元素设置 float: left；
  * 在浮动元素的后面添加一个空div, 并设置属性clear: both;
  * 父级元素使用伪类进行清除
  ```
    .clear {
      zoom: 1;
    }
    .clear:after {
      content: "";
      display: table;
      clear: both;
    }
  ```
