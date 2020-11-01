##  BFC (block formatting context)块格式化上下文调研

  ### MDN解释：

  一个块格式化上下文（block formatting context） 是Web页面的可视化CSS渲染出的一部分。它是块级盒布局出现的区域，也是浮动层元素进行交互的区域。
   一个块格式化上下文由以下之一创建：

  - 根元素或其它包含它的元素
  - 浮动元素 (元素的 float 不是 none)
  - 绝对定位元素 (元素具有 position 为 absolute 或 fixed)
  - 内联块 (元素具有 display: inline-block)
  - 表格单元格 (元素具有 display: table-cell，HTML表格单元格默认属性)
  - 表格标题 (元素具有 display: table-caption, HTML表格标题默认属性)
  - 具有overflow 且值不是 visible 的块元素，
  - display: flow-root ( 后面讲解 )
  - column-span: all 应当总是会创建一个新的格式化上下文，即便具有 column-span: all 的元素并不被包裹在一个多列容器中。
  - 一个块格式化上下文包括创建它的元素内部所有内容，除了被包含于创建新的块级格式化上下文的后代元素内的元素。

  块格式化上下文对于定位 (参见 float) 与清除浮动 (参见 clear) 很重要。定位和清除浮动的样式规则只适用于处于同一块格式化上下文内的元素。浮动不会影响其它块格式化上下文中元素的布局，并且清除浮动只能清除同一块格式化上下文中在它前面的元素的浮动。

  ### 关于BFC的定义：

  - BFC(Block formatting context)直译为"块级格式化上下文"。它**是一个独立的渲染区域**，只有**Block-level box**参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。
  
    我们常说的文档流其实分为定位流、浮动流和普通流三种。而**普通流其实就是指BFC中的FC**。
  
    **FC**是formatting context的首字母缩写，直译过来是格式化上下文，它**是页面中的一块渲染区域**，有一套渲染规则，决定了其**子元素如何布局，以及和其他元素之间的关系和作用。**
  
    BFC是一个独立的布局环境，其中的元素布局是不受外界的影响，并且在一个BFC中，块盒与行盒（行盒由一行中所有的内联元素所组成）都会垂直的沿着其父元素的边框排列。
    
    常见的FC有BFC、IFC（行级格式化上下文），还有GFC（网格布局格式化上下文）和FFC（自适应格式化上下文），这里就不再展开了。
  
    #### 通俗一点的方式解释:
  
    BFC 可以简单的理解为**某个元素的一个 CSS 属性**，只不过这个属性**不能被开发者显式的修改**，拥有这个属性的元素对内部元素和外部元素会表现出一些特性，这就是BFC。
    
  - **BFC概括：**可以在心中记住这么一个概念———**所谓的BFC就是css布局的一个概念，是一块区域，一个环境。** 

  - 顶级(top-level)元素，块级(block-level)元素，内联(inline)元素

    - **Top-level element [顶级元素]**  html, body, frameset ,他们的表现如block-element  属于高级块元素
    - **Block-level element [块级元素]** p, h1-h6, div, ul 
      -  高度宽度都是可以设置的。
      - 默认状态下每次都占据一整个行，后面的内容也必须再新起一行显示。当然非块级元素也可以通过css的display:block;将其更改成块级元素。此外还有个特殊的，float也具有此功能。
      - 块级元素能够独立存在, 一般的块级元素之间以换行分隔。块级元素是构成一个html的主要和关键元素, 而任意一个块级元素均可以用Box model来解释说明. 
    -  **Inline element** 【内联元素】: { a, br, em, img, li, span } 
      -  内联元素的高度宽度是不可以设置的，其宽度就是自身文字或者图片的宽度。我们常用到的<a>、<span>、<em>都属于内联元素。
      - 内联元素的显示特点就是像文本一样的显示，不会独自占据一个行。当然内联元素也能变成块级元素，那就是通过css的display:block;或float来实现。
      - 内联元素依附其他块级元素存在, 紧接于被联元素之间显示, 而不换行. 

  ### 触发条件或者说哪些元素会生成BFC：

  　　满足下列条件之一就可触发BFC

  　　【1】根元素，即HTML元素

  　　【2】float的值不为none

  　　【3】overflow的值不为visible

  　　【4】display的值为inline-block、table-cell、table-caption

  　　【5】position的值为absolute或fixed 

​      【6】display: flow-root

  ### BFC有哪些作用：

  - 自适应两栏布局
  - 可以阻止元素被浮动元素覆盖
  - 可以包含浮动元素——清除内部浮动
  - 分属于不同的BFC时可以阻止margin重叠(外边距折叠问题)

  ### BFC布局规则：

  ​	1.内部的Box会在垂直方向，一个接一个地放置。[demo](http://127.0.0.1:5500/demo/bfc.html)

  ​	2.Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠(外边距合并)。[demo](http://127.0.0.1:5500/demo/bfc2.html)

    - 在CSS当中，相邻的两个盒子（可能是兄弟关系也可能是祖先关系）的外边距可以结合成一个单独的外边距。这种合并外边距的方式被称为折叠，并且因而所结合成的外边距称为折叠外边距。

  ```
  折叠结果：
      1.   两个相邻的外边距都是正数时，折叠结果是它们两者之间较大的值。
      2.   两个相邻的外边距都是负数时，折叠结果是两者绝对值的较大值。
      3.   两个外边距一正一负时，折叠结果是两者的相加的和。
  ```

- **产生折叠的必备条件：margin必须是邻接的!**，而根据w3c规范，两个margin是邻接的必须满足以下条件：

  - 必须是处于常规文档流（非float和绝对定位）的块级盒子,并且处于同一个BFC当中。
  - 没有线盒，没有空隙（clearance，下面会讲到），没有padding和border将他们分隔开
  - 都属于垂直方向上相邻的外边距，可以是下面任意一种情况：
  - 元素的margin-top与其第一个常规文档流的子元素的margin-top
    - 元素的margin-bottom与其下一个常规文档流的兄弟元素的margin-top
  - height为auto的元素的margin-bottom与其最后一个常规文档流的子元素的margin-bottom
    - 高度为0并且最小高度也为0，不包含常规文档流的子元素，并且自身没有建立新的BFC的元素的margin-top和margin-bottom

  - **以上的条件意味着下列的规则:**
    - 创建了新的BFC的元素（例如浮动元素或者'overflow'值为'visible'以外的元素）与它的子元素的外边距不会折叠
    - 浮动元素不与任何元素的外边距产生折叠（包括其父元素和子元素）
  - 绝对定位元素不与任何元素的外边距产生折叠
    - inline-block元素不与任何元素的外边距产生折叠
  - 一个常规文档流元素的margin-bottom与它下一个常规文档流的兄弟元素的margin-top会产生折叠，除非它们之间存在间隙（clearance）。
    - 一个常规文档流元素的margin-top 与其第一个常规文档流的子元素的margin-top产生折叠，条件为父元素不包含 padding 和 border ，子元素不包含 clearance。
  - 一个 'height' 为 'auto' 并且 'min-height' 为 '0'的常规文档流元素的 margin-bottom 会与其最后一个常规文档流子元素的 margin-bottom 折叠，条件为父元素不包含 padding 和 border ，子元素的 margin-bottom 不与包含 clearance 的 margin-top 折叠。
    - 一个不包含border-top、border-bottom、padding-top、padding-bottom的常规文档流元素，并且其 'height' 为 0 或 'auto'， 'min-height' 为 '0'，其里面也不包含行盒(line box)，其自身的 margin-top 和 margin-bottom 会折叠。

  ​	3.每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。[demo](http://127.0.0.1:5500/demo/bfc3.html)

  ​	4.BFC的区域不会与float box重叠。[demo](http://127.0.0.1:5500/demo/bfc4.html)

  ​	5.BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。[demo](http://127.0.0.1:5500/demo/bfc5.html)

  ​	6.计算BFC的高度时，浮动元素也参与计算[demo](http://127.0.0.1:5500/demo/bfc6.html)