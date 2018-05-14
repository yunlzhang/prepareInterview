### 1、双向数据绑定

基于数据劫持的双向数据绑定

利用Object.defineProperty生成的Observer针对对象／对象的属性进行劫持，在属性发生变化后通知订阅者。

解析器Compile解析模板中的Directive（指令），收集指令所依赖的方法和数据，等待数据变化然后进行渲染。

Watcher属于Observer和Compile桥梁，它将接収到的Observer产生的数据变化，并根据Compile提供的指令进行视图渲染，使得数据变化促使视图变化。


Vue 不能监测以下变动

    利用数组索引直接设置一个项
    修改数组长度