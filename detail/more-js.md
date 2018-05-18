### http 和 https协议的区别

https 协议

1、https协议时基于ssl的http协议

2、https使用与http不同的端口进行通信

3、提供身份验证与加密通信的方法，被广泛应用于互联网安全敏感的通信

交互过程

1、客户端请求ssl连接，并将自己支持的加密规则发给服务器

2、服务器端将自己的身份信息以证书的方式发给客户端。证书里包含了网站地址、加密公钥以及证书的颁发机构等信息

3、获得证书后，客户端做以下工作：

    1）验证证书的合法性

    2）如果证书受信任，客户端会生成一串随机的密码，并用证书提供的公钥进行加密

    3）将加密好的随机数发给服务器

4、服务器获得客户端发的加密了的随机数，用自己的私钥进行解密，得到这个随机数，把这个随机数作为对称加密的密钥。

5、之后服务器与客户端就可以用随机数对各自的信息进行加密，解密。

http 与 https 的区别

1、https协议需要证书

2、https是明文传输，而https使用的是具有安全性的ssl加密传输协议

3、http默认端口为80 https 默认端口号是443

4、http是简单无状态，https是有ssl + http 组成的可进行加密传输、身份验证的䙳网络协议



### 2、移动端点击穿透

移动端的click事件300ms延迟是因为浏览器为了判断当前事件是否双击（double click）而在触发touchend事件后等待用户约300ms（双击事件可以缩放视图内容），若用户没有再次点击则默认触发click事件。

解决办法
通过设置meta标签

    <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,minimum-scale=1,user-scalable=no" 

    fastclick https://github.com/ftlabs/fastclick


### 3、javascript 设计者模式

#### 单例模式

确保只有一个实例

可以全局访问

```js
const singleton = function(name) {
    this.name = name
    this.instance = null
}

singleton.prototype.getName = function() {
    console.log(this.name)
}

singleton.getInstance = function(name) {
    if (!this.instance) { // 关键语句
        this.instance = new singleton(name)
    }
    return this.instance
}
//感觉跟动态原型方式创建对象有点像。
```
```js
//弹框层的实现
const createLoginLayer = function() {
    const div = document.createElement('div')
    div.innerHTML = '登入浮框'
    div.style.display = 'none'
    document.appendChild(div)
    return div
}
//解耦
const getSingle = function(fn) {
    //利用闭包来保存局部变量
    const result
    return function() {
        return result || result = fn.apply(this, arguments)
    }
}

const createSingleLoginLayer = getSingle(createLoginLayer)

document.getElementById('loginBtn').onclick = function() {
    createSingleLoginLayer()
}
```

####  策略模式

定义：根据不同参数可以命中不同的策略
在函数是一等公民的 JS 中，策略模式的使用常常隐藏在高阶函数中

```js
const strategy = {
    'S':function(salary){
        return salary * 4;
    },
    'A':function(salary){
        return salary * 3;
    },
    'B':function(salary){
        return salary * 2;
    }
}

const calculateBonus = function(level,salary){
    return strategy[level](salary);
}

```


```js
//策略模式变通
const S = function(salary){
    return salary * 4;
}
const A = function(salary){
    return salary * 3;
}
const B = function(salary){
    return salary * 2;
}

const calculateBonus = function(func,salary){
    return func(salary);
}


```

优点：

能减少大量的if语句

复用性好

```js


```










参考：https://github.com/MuYunyun/blog/tree/master/Basic%20Skill/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F