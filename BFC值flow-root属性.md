### flow-root属性：

-  元素容器会生成一个块盒子，并且块盒子里的内容是使用流布局。它总是为它建立一个新的块格式化上下文内容。 

 - 推荐一个查询兼容性网站：https://caniuse.com/

    目前兼容性：![img](D:\work\depository\float\img\flow.png)

   

    今天开始你可以使用`display:flow-root`。这周 Firefox 53 和 Chrome 58 两大主流浏览器在这周都发布相关消息：**支持`flow-root`属性值。**

   来看一个简单的示例，比如我们一个这样的结构：

   ```css
   <div class="wrapper">     <div class="floatElement">浮动元素</div>     <div class="floatElement">浮动元素</div> </div> 
   ```

   我们的 CSS 是这样的：

   ```css
   .floatElement{     float: left; /*或者 right*/ } 
   ```

   如果仅这样操作，都会造成容器`wrapper`的高度塌陷。以前我们都是通过`clearfix`的方案（最常用的吧）来解决：

   ```css
   .wrapper::after {     content:'';     display: table;     clear: both } 
   ```

   上面的解决方案都是老的，其实今天我们可以在`.wrapper`容器上这样使用就可以达到类似`clearfix`的效果：

   ```css
   .wrapper{     display: flow-root; } 
   ```

   虽然主流浏览器**Firefox 53+**、**Chrome 58+**和**Opera 45+**都支持`flow-root`属性(有关于浏览器对该属性的兼容性，可以通过 Caniuse.com 来查询)。但实际当中，我们必竟有很多业务需求是需要兼容一些低版本的。对于一位 CSS 的极度爱好者，总是喜欢在项目中不断的尝试使用一些新特性。为了更好对`flow-root`做降级处理，我们可以通过 CSS 的条件属性`@supports()`来做相应的处理。比如上面的代码我们可以这样使用：

   ```css
   .floatElement{     float: left; /*或者 right*/ } .wrapper::after {     content:'';    display: table;    clear: both } @supports(display:flow-root){     .wrapper{         display: flow-root;     }     .wrapper::after{         content:none;     }  } 
   ```

   当然你还可以把这样使用：

   ```css
   .floatElement{     float: left; /*或者 right*/ } @supports not (display:flow-root) {     .wrapper::after {         content: '';         display: table;         clear:both;     } } 
   ```

   - [demo](http://127.0.0.1:5500/demo/flow.html)

