# css 相关知识

### 盒模型
    
    - IE盒模型
    
        box width = content width + padding left + padding right + border left + border right

        box height = content height + padding top + padding bottom + border top + border bottom   

        ![IE](https://www.ibm.com/developerworks/cn/web/1310_shatao_quirks/image005.jpg)
    - 标准盒模型

        box width = content width

        box height = content height
        
        ![standard](https://www.ibm.com/developerworks/cn/web/1310_shatao_quirks/image007.jpg)


### 重绘和回流

重绘 ： 当页面中元素样式的改变并不影响它在文档中的位置时，如（color,background,visibility等），浏览器会将新样式赋给新元素并重新绘制它，这个过程称为重绘

回流：当render tree(dom)中部分或全部元素的尺寸、结构、或某些属性发生变化时,浏览器重新渲染部分或全部文档的过程称为回流。

回流比重绘的消耗性能要大

回流必将引起重绘，重绘不一定引起回流。


### bfc

特点：

内部的Box会在垂直方向，从顶部开始一个接一个地放置。

Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生叠加

每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。

BFC的区域不会与float box叠加。

BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然。

计算BFC的高度时，浮动元素也参与计算


创建bfc条件：

float 不为none

positon属性为absolute或fixed

display值为 table-cell table-caption inline-block flex inline-flex

overflow不为visible
