# js相关知识

1、 js隐式转换

基本数据类型（原始值）

undefined、null、boolean、string、number、symbol(es6)

引用数据类型（对象值）

object 

---
js中的隐式转换主要涉及的三种转换：

    1、将值转化为原始值 ToPrimitive()

    2、将值转化成数字 ToNumber()

    3、将值转化成字符串 ToString()

js内部ToPrimitive()操作 ToPrimitive(input,PreferredType)

    PreferredType为number

    1、如果输入的值已经是一个原始值，则直接返回它

    2、否则，如果输入的值是一个对象，则调用该对象的valueOf()方法，如果valueOf()方法的返回值是一个原始值，则返回这个原始值。

    3、否则，调用这个对象的toString()方法，如果toString()方法返回的是一个原始值，则返回这个原始值。

    4、否则，抛出TypeError异常。

    *PreferredType为string

    1、如果输入的值已经是一个原始值，则直接返回它

    2、否则，调用这个对象的toString()方法，如果toString()方法返回的是一个原始值，则返回这个原始值。

    3、否则，如果输入的值是一个对象，则调用该对象的valueOf()方法，如果valueOf()方法的返回值是一个原始值，则返回这个原始值。

    4、否则，抛出TypeError异常。

    *PreferredType不存在

    1、该对象为Date类型，则PreferredType被设置为String

    2、否则，PreferredType被设置为Number

```js
const a = {
    i: 1,
    toString: function () {
        return a.i++;
    }
}
if (a == 1 && a == 2 && a == 3) {
    console.log('hello world!');
}
```

2 数组常用方法

    会改变原数组的方法
    pop         删除末尾元素 返回删除的元素，删除为空时是undefined
    push        向后追加一个元素 返回新数组的长度
    unshift     向前追加一个元素 返回新数组的长度
    shift       删除第一个元素 返回删除的元素，删除为空时是undefined

    splice      数组修改最常用方法   返回一个数组 包含被删除的元素 
    reverse     翻转数组

    不会改变原数组的方法
    concat      合并数组 返回合并后的新数组（可以给用来做浅拷贝）
    slice       截取数组 返回一个从开始到结束（不包括结束）选择的数组的一部分浅拷贝到一个新数组对象 可以将伪数组转为真正数组

    some,every  判断数组元素是否通过函数测试，some 为true时跳出，every 若为false时则停止检测跳出
    reduce,map,filter,forEach 数组的一些迭代方法，不会修改原数组




参考[https://juejin.im/post/5a7172d9f265da3e3245cbca](https://juejin.im/post/5a7172d9f265da3e3245cbca)



