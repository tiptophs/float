##  BFC (block formatting context)块格式化上下文调研

  ### MDN解释：
  
   - [MDN上BFC介绍地址一](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)
   
   - [MDN上BFC介绍地址二](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flow_Layout/Intro_to_formatting_contexts)
   
   - **Block formatting contexts 块格式化上下文** 

     -  ```
          块格式化上下文（Block Formatting Context，BFC） 是Web页面的可视CSS渲染的一部分，是块盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域。

          下列方式会创建块格式化上下文：

            1. 根元素（<html>）
            2. 浮动元素（元素的 float 不是 none）
            3. 绝对定位元素（元素的 position 为 absolute 或 fixed）
            4. 行内块元素（元素的 display 为 inline-block）
            5. 表格单元格（元素的 display 为 table-cell，HTML表格单元格默认为该值）
            6. 表格标题（元素的 display 为 table-caption，HTML表格标题默认为该值）
            7. 匿名表格单元格元素（元素的 display 为 table、table-row、 table-row-group、table-header-group、table-footer-group（分别是HTML table、row、tbody、thead、tfoot 的默认属性）或 inline-table）
            8. overflow 值不为 visible 的块元素
            9. display 值为 flow-root 的元素
            10. contain 值为 layout、content 或 paint 的元素
            11. 弹性元素（display 为 flex 或 inline-flex 元素的直接子元素）
            12. 网格元素（display 为 grid 或 inline-grid 元素的直接子元素）
            多列容器（元素的 column-count 或 column-width 不为 auto，包括 column-count 为 1）
            13. column-span 为 all 的元素始终会创建一个新的BFC，即使该元素没有包裹在一个多列容器中（标准变更，Chrome bug）。
            块格式化上下文包含创建它的元素内部的所有内容.

          块格式化上下文对浮动定位（参见 float）与清除浮动（参见 clear）都很重要。
          
            1. 浮动定位和清除浮动时只会应用于同一个BFC内的元素。
            2. 浮动不会影响其它BFC中元素的布局，而清除浮动只能清除同一BFC中在它前面的元素的浮动。
            3. 外边距折叠（Margin collapsing）也只会发生在属于同一BFC的块级元素之间。
        ```

     -  ```
          文档最外层元素使用块布局规则或称为初始块格式上下文。这意味着<html>元素块中的每个元素都是按照正常流程遵循块和内联布局规则进行布局的。参与 BFC 的元素使用CSS框模型概述的规则，该模型定义了元素的边距、边框和填充如何与同一上下文中的其他块交互。
            
            <html> 元素不是唯一能够创建块格式上下文的元素。**默认为块布局的任何元素也会为其后代元素创建块格式上下文**。此外，还有一些CSS属性可以使元素创建一个BFC，即使默认情况下它不这样做。

            除了文档的根元素(<html>) 之外，还将在以下情况下创建一个新的BFC：

              1. 使用float 时其浮动的元素
              2. 绝对定位的元素 (包含 position: fixed 或position: absolute 或 positon: sticky)
              3. 使用以下属性的元素 display: inline-block
              4. 表格单元格或使用 display: table-cell, 包括使用 display: table-* 属性的所有表格单元格
              5. 表格标题或使用 display: table-caption 的元素
              6. 块级元素的 overflow 属性不为 visible
              7. 元素属性为 display: flow-root 或 display: flow-root list-item
              8. 元素属性为 contain: layout, content, 或 strict
              9. flex items
              10. 网格布局元素
              11. multicol containers
              12. 元素属性 column-span 设置为 all
            这很有用，因为新的BFC的行为与最外层的文档非常相似，它在主布局中创造了一个小布局。BFC包含其内部的所有内容，float 和 clear 仅适用于同一格式上下文中的项目，而页边距仅在同一格式上下文中的元素之间折叠。
        ```
  ### 关于BFC的定义：

  - BFC(Block formatting context)直译为"块级格式化上下文"。它**是一个独立的渲染区域**，只有**Block-level box**参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。
  
    我们常说的文档流其实分为定位流、浮动流和普通流三种。而**普通流其实就是指BFC中的FC**。
  
    **FC**是formatting context的首字母缩写，直译过来是格式化上下文，它**是页面中的一块渲染区域**，有一套渲染规则，决定了其**子元素如何布局，以及和其他元素之间的关系和作用。**
  
    BFC是一个独立的布局环境，其中的元素布局是不受外界的影响，并且在一个BFC中，块盒与行盒（行盒由一行中所有的内联元素所组成）都会垂直的沿着其父元素的边框排列。
    
    常见的FC有BFC、IFC（行级格式化上下文），还有GFC（网格布局格式化上下文）和FFC（自适应格式化上下文），这里就不再展开了。
  
    #### 通俗一点的方式解释:
  
    BFC 可以简单的理解为**某个元素的一个 CSS 属性**，只不过这个属性**不能被开发者显式的修改**，拥有这个属性的元素对内部元素和外部元素会表现出一些特性，这就是BFC。
    
  - **BFC概括：** 可以在心中记住这么一个概念———**所谓的BFC就是css布局的一个概念，是一块区域，一个环境。** 

  - 顶级(top-level)元素，块级(block-level)元素，内联(inline)元素

    - **Top-level element [顶级元素]**  html, body, frameset ,他们的表现如block-element  属于高级块元素
    - **Block-level element [块级元素]** p, h1-h6, div, ul 
      -  高度宽度都是可以设置的。
      - 默认状态下每次都占据一整个行，后面的内容也必须再新起一行显示。当然非块级元素也可以通过css的display:block;将其更改成块级元素。此外还有个特殊的，float也具有此功能。
      - 块级元素能够独立存在, 一般的块级元素之间以换行分隔。块级元素是构成一个html的主要和关键元素, 而任意一个块级元素均可以用Box model来解释说明. 
    -  **Inline element** 【内联元素】:  a, br, em, img, li, span 
      -  内联元素的高度宽度是不可以设置的，其宽度就是自身文字或者图片的宽度。我们常用到的`<a>、<span>、<em>`都属于内联元素。
      - 内联元素的显示特点就是像文本一样的显示，不会独自占据一个行。当然内联元素也能变成块级元素，那就是通过css的display:block;或float来实现。
      - 内联元素依附其他块级元素存在, 紧接于被联元素之间显示, 而不换行. 

  ### 触发条件或者说哪些元素会生成BFC：

​	满足下列条件之一就可触发BFC(常用)

  		【1】根元素，即HTML元素

  		【2】float的值不为none

  		【3】overflow的值不为visible

  		【4】display的值为inline-block、table-cell、table-caption

  		【5】position的值为absolute或fixed 
  		
  		【6】display: flow-root

  ### BFC有哪些作用：

  - 自适应两栏布局
  - 可以阻止元素被浮动元素覆盖
  - 可以包含浮动元素——清除内部浮动
  - 分属于不同的BFC时可以阻止margin重叠(外边距折叠问题)

  ### BFC布局规则：

- 内部的Box会在垂直方向，一个接一个地放置。[demo](http://127.0.0.1:5500/demo/bfc.html)

- Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠(外边距合并)。[demo](http://127.0.0.1:5500/demo/bfc2.html)

  - ```
    在CSS当中，相邻的两个盒子（可能是兄弟关系也可能是祖先关系）的外边距可以结合成一个单独的外边距。这种合并外边距的方式被称为折叠，并且因而所结合成的外边距称为折叠外边距。
    ```

  - ```
    折叠结果：
    1.   两个相邻的外边距都是正数时，折叠结果是它们两者之间较大的值。
    2.   两个相邻的外边距都是负数时，折叠结果是两者绝对值的较大值。
    3.   两个外边距一正一负时，折叠结果是两者的相加的和。
    ```

  - **产生折叠的必备条件：margin必须是邻接的!**，而根据w3c规范，两个margin是邻接的必须满足以下条件：

    - 必须是处于常规文档流（非float和绝对定位）的块级盒子,并且处于同一个BFC当中。

    - 没有线盒，没有空隙（clearance，下面会讲到），没有padding和border将他们分隔开

    - 都属于垂直方向上相邻的外边距，可以是下面任意一种情况：

      1. 元素的margin-top与其第一个常规文档流的子元素的margin-top

      2. 元素的margin-bottom与其下一个常规文档流的兄弟元素的margin-top

      3. height为auto的元素的margin-bottom与其最后一个常规文档流的子元素的margin-bottom

      4. 高度为0并且最小高度也为0，不包含常规文档流的子元素，并且自身没有建立新的BFC的元素的margin-top和margin-bottom

  - **以上的条件意味着下列的规则:**

    - 创建了新的BFC的元素（例如浮动元素或者'overflow'值为'visible'以外的元素）与它的子元素的外边距不会折叠

    - 浮动元素不与任何元素的外边距产生折叠（包括其父元素和子元素）

    - 绝对定位元素不与任何元素的外边距产生折叠

    - inline-block元素不与任何元素的外边距产生折叠

    - 一个常规文档流元素的margin-bottom与它下一个常规文档流的兄弟元素的margin-top会产生折叠，除非它们之间存在间隙（clearance）。

    - 一个常规文档流元素的margin-top 与其第一个常规文档流的子元素的margin-top产生折叠，条件为父元素不包含 padding 和 border ，子元素不包含 clearance。

    - 一个 'height' 为 'auto' 并且 'min-height' 为 '0'的常规文档流元素的 margin-bottom 会与其最后一个常规文档流子元素的 margin-bottom 折叠，条件为父元素不包含 padding 和 border ，子元素的 margin-bottom 不与包含 clearance 的 margin-top 折叠。

    - 一个不包含border-top、border-bottom、padding-top、padding-bottom的常规文档流元素，并且其 'height' 为 0 或 'auto'， 'min-height' 为 '0'，其里面也不包含行盒(line box)，其自身的 margin-top 和 margin-bottom 会折叠。

- 每个元素的margin 即box的左边， 与包含块border 即box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。[demo](http://127.0.0.1:5500/demo/bfc3.html)
- BFC的区域不会与float box重叠。[demo](http://127.0.0.1:5500/demo/bfc4.html)
- BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。[demo](http://127.0.0.1:5500/demo/bfc5.html)
- 计算BFC的高度时，浮动元素也参与计算[demo](http://127.0.0.1:5500/demo/bfc6.html)
