# js相关知识

### 1、 js隐式转换

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
参考[https://juejin.im/post/5a7172d9f265da3e3245cbca](https://juejin.im/post/5a7172d9f265da3e3245cbca)
### 2、 字符串

常用方法 （不会改变原字符串）

match       当一个字符串与一个正则表达式匹配时， match()方法检索匹配项

split       使用指定的分隔符字符串将一个String对象分割成字符串数组，以将字符串分隔为子字符串

slice       提取一个字符串的一部分，并返回一新的字符串 包括begin不包括end  不会修改原数组

substr      返回一个字符串中从指定位置开始到指定字符数的字符     

replace     方法返回一个由替换值替换一些或所有匹配的模式后的新字符串。模式可以是一个字符串或者一个正则表达式, 替换值可以是一个字符串或者一个每次匹配都要调用的函数。


### 3、 数组

会改变原数组的方法

pop         删除末尾元素 返回删除的元素，删除为空时是undefined

push        向后追加一个元素 返回新数组的长度

unshift     向前追加一个元素 返回新数组的长度

shift       删除第一个元素 返回删除的元素，删除为空时是undefined

splice      数组修改最常用方法   返回一个数组 包含被删除的元素 

reverse     翻转数组

sort        排序

不会改变原数组的方法

concat      合并数组 返回合并后的新数组（可以给用来做浅拷贝）

slice       截取数组 返回一个从开始到结束（不包括结束）选择的数组的一部分浅拷贝到一个新数组对象 可以将伪数组转为真正数组

join    将数组转为字符串

some,every  判断数组元素是否通过函数测试，some 为true时跳出，every 若为false时则停止检测跳出

reduce,map,filter,forEach 数组的一些迭代方法，不会修改原数组

判断一个元素是不是数组

    arr instanceof  Array === true
    Object.prototype.toString.call(arr) === '[object Array]'
    Array.isArray(arr)

### 4、 原型

javascript 中 每个对象都有一个私有属性（称之为[[Prototype]]）,它指向它的原型对象（prototype）。该prototype对象又具有一个自己的prototype，层层向上直到一个对象的原型为null

构造函数、原型、实例的关系：每个构造函数都有一个原型对象，原型对象中都包含一个指向构造函数的指针（constructor）,而实例都包含一个指向原型对象的内部指针（\_\_proto\_\_）

基于原型链继承的思想：利用原型让一个引用类型继承另一个引用类型的属性和方法。


### 5、创建对象的方式

* 工厂模式
```js
function createObj(){
    var o = new Object();
    o.name = 'aaa';
    //XXXX
    return o;
}

```

* 构造函数模式
```js
function CreateObj(){
    this.name = 'aaa';
    this.sayName = function(){
        console.log(this.name);
    }
}
var newObj = new CreateObj();
```
缺点：

每个方法都要在实例上创建一遍

解决办法：将方法移到构造函数外面

* 原型模式

```js
function CreateObj(){}
CreateObj.prototype.name = 'aaa';
CreateObj.prototype.sayName = function(){
    console.log(this.name);
}
newObj = new CreateObj();
```
缺点：原型上引用类型的数据会被所有实例共享

* 组合使用构造函数模式和原型模式

```js
function CreateObj(){
    this.name = 'aaa';
}
CreateObj.prototype ={
    constructor:CreateObj,
    sayName: function(){
        console.log(this.name);
    }
}
```
实例属性都是在构造函数中定义，所有实例共享的属性constructor 和 方法在原型中定义。


* 动态原型方式

```js
function CreateObj(){
    this.name = 'aaa';
    if(typeof this.sayName !== 'function'){
        CreateObj.prototype.sayName = function(){
            console.log(this.name);
        }
    }
}

```
调用改构造函数,只在sayName方法不存在的时候才会执行判断分支，此后，原型已经完成初始化。
注意：不能使用对象字面量来修改原型，回切断现有实例和新原型之间的联系。

* 寄生构造函数

```js
function CreateObj(){
    var o = new Object();
    o.name = 'aaa';
    retrun o;
}
var newObj = new CreateObj();
```
感觉跟工厂模式区别不大，只不过多了一个new调用的过程。

* 稳妥构造函数模式
稳妥对象：没有公共属性，其方法也不引用this的对象
```js
function CreateObj(name){
    //创建返回的对象
    var o = new Object();
    //定义私有属性和方法

    o.sayName = function(){
        console.log(name)
    }
    return o;
}

```


### 6、继承

确定原型和实例的方法：instanceof    isPrototypeOf()


* 原型链继承

```js
function SuperType(){
    this.name = 'Super';
}
SuperType.prototype.sayName = function(){
    console.log(this.name)
}
function SubType(){}
SubType.prototype = new SuperType();
```
问题：
引用类型的原型属性会被所有实例共享。  

重写了子类的prototype 导致constructor 属性丢失，instanceof失去作用(将子类的constructor属性重新赋值)
>SubType.prototype.constructor = SubType;

无法向构造函数传递参数


* 借用构造函数(伪造对象或经典继承)

```js
function SuperType(){
    this.name = 'Super';
}
SuperType.prototype.sayName = function(){
    console.log(this.name)
}
//使用call 或 apply
function SubType(){
    SuperType.apply(this,arguments);
    this.say = function(){};
}
```
缺点：方法都在构造函数中定义，原型上函数无法复用


* 组合继承
使用原型链实现对原型属性和方法的继承，通过构造函数实现对实例属性的继承。
```js
function SuperType(){
    this.name = 'Super';
}
SuperType.prototype.sayName = function(){
    console.log(this.name)
}
//使用call 或 apply
function SubType(){
    SuperType.apply(this,arguments);
    this.name = 'bbb';
}
SubType.prototype = new SuperType();
SubType.prototype.constructor =  SubType;
```
避免原型链和借用构造函数的缺陷


* 原型式继承
借用原型可以基于已有的对象创建新对象
```js
function createObj(obj){
    function F(){}
    F.prototype = obj;
    return new F();
}

```
等同于 ES6中的Object.create();

在传入一个参数的情况下，Object.create() 和 Object() 方法的行为相同

* 寄生式继承
```js
function createNewObj(obj){
    var clone = Object(obj);
    clone.anotherFun = function(){

    }
    return clone;
}
```

缺点：使用寄生式继承来为对象添加函数，不能做到函数复用而降低效率。与构造函数类型

* 寄生组合式继承

```js
function SuperType(){
    this.name = 'aaa';
}
SuperType.prototype.sayName = function(){
    console.log(this.name);
}
function SubType(){
    SuperType.call(this);
}
SubType.prototype = new SuperType();
SubType.prototype.constructor = SubType;

```
缺点：父类构造函数被调用了两次


改进：
```js
function SuperType(){
    this.name = 'aaa';
}
SuperType.prototype.sayName = function(){
    console.log(this.name);
}
function SubType(){
    SuperType.call(this);
}
SubType.prototype = Object.create(SuperType.prototype);
SubType.prototype.constructor = SubType;

```

### 7、跨域

由于浏览器的同源策略（保证用户信息的安全，防止恶意的网站窃取数据），只要协议、域名、端口号不同就会产生跨域问题。

解决办法 jsonp,cors,nginx代理，window.name ,document.domain

### 8、cookie,localStorage,sessionStorage

cookie一般只有4k，localStorage,sessionStorage有5M

不同浏览器无法共享localStorage和sessionStorage中的信息

同一浏览器的相同域名和端口的不同页面间可以共享相同的 localStorage，但是不同页面间无法共享sessionStorage的信息。这里需要注意的是，页面仅指顶级窗口，如果一个页面包含多个iframe且他们属于同源页面，那么他们之间是可以共享sessionStorage的

多级域名共享cookie  设置cookie的domain为顶级域名 

cookie 读取
```js
function parseCookie(){
    var obj = {};
    document.cookie.split(';').forEach(function(item,index){
        obj[item.split('=')[0]] = item.split('=')[1];
    });
    return obj;
}
//设置cookie及失效日期
//1
document.cookie += ';'+name+'='+value+';expires='+date // date为GMT时间  toGMTString();
//2
document.cookie += ';'+name+'='+value+';max-age='+time // 失效的毫秒数  正数持久化cookie 0 删除cookie  负数 会话cookie窗口关闭，cookie便清除   
//IE 浏览器 不支持 max-age
```

localStorage,sessionStorage读取

getItem()

setItem

removeItem()

clear();//清除所有的

### 9、web缓存

数据库缓存、代理服务器缓存、CDN缓存、浏览器缓存


浏览器缓存

1、Cache-Control

常用值：max-age/s-maxage（只用于共享缓存（比如CDN缓存））/public/private/no-cache/no-store/must-revalidate

2、Expires 

缓存过期时间，用来指定资源到期的时间，是服务器端的具体的时间点。也就是说，Expires=max-age + 请求时间，需要和Last-modified结合使用

3、Last-modified

服务器端文件的最后修改时间，需要和cache-control共同使用，是检查服务器端资源是否更新的一种方式

当浏览器再次进行请求时，会向服务器传送If-Modified-Since报头，询问Last-Modified时间点之后资源是否被修改过。如果没有修改，则返回码为304，使用缓存；如果修改过，则再次去服务器请求资源，返回码和首次请求相同为200，资源为服务器最新资源

4、ETag

根据实体内容生成一段hash字符串，标识资源的状态，由服务端产生。浏览器会将这串字符串传回服务器，验证资源是否已经修改

使用缓存流程

![cache](../img/cache.png)


缓存分为强制缓存和协商缓存

1）浏览器在加载资源时，先根据这个资源的一些http header判断它是否命中强缓存，强缓存如果命中，浏览器直接从自己的缓存中读取资源，不会发请求到服务器。比如某个css文件，如果浏览器在加载它所在的网页时，这个css文件的缓存配置命中了强缓存，浏览器就直接从缓存中加载这个css，连请求都不会发送到网页所在服务器；

2）当强缓存没有命中的时候，浏览器一定会发送一个请求到服务器，通过服务器端依据资源的另外一些http header验证这个资源是否命中协商缓存，如果协商缓存命中，服务器会将这个请求返回，但是不会返回这个资源的数据，而是告诉客户端可以直接从缓存中加载这个资源，于是浏览器就又会从自己的缓存中去加载这个资源；

3）强缓存与协商缓存的共同点是：如果命中，都是从客户端缓存中加载资源，而不是从服务器加载资源数据；区别是：强缓存不发请求到服务器，协商缓存会发请求到服务器。

4）当协商缓存也没有命中的时候，浏览器直接从服务器加载资源数据。

参考：
https://segmentfault.com/a/1190000008377508

https://www.cnblogs.com/lyzg/p/5125934.html




### 10、事件循环 （event loop）

参考：https://zhuanlan.zhihu.com/p/33058983


### 11、js与native交互 JSBradge

参考：https://juejin.im/post/5abca877f265da238155b6bc


### 11、es6相关 

let,const 

    let不存在变量声明提升的现象。

promise

async,await


for in  和 for of的区别

for of 只能遍历数组  遍历value

for of要想遍历对象，需要使用Object.keys()

for in是ES5标准,for of是ES6标准

for in  可以遍历数组或对象 但是遍历的是key

```js
var obj = {
    a:1,
    b:2,
    c:3,
    d:4,
    e:5
}
for(var key of Object.keys(obj)){
    console.log(key + ':' + obj[key]);
}
```

扩展：

for的三个表达式是可选的

第二个表达式只要返回true 就会继续执行循环体

若三个表达式都省略，在循环体内部需要加上break语句，防止创建死循环



