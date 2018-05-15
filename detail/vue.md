### vue 生命周期

beforeCreate
    在实例初始化之后，数据观测（data observer）和 event/watcher事件配置之前被调用

created
    在实例创建完成后被立即调用，在这一步，实例已完成一下配置：数据观测（data observer）,属性和方法运算，watch/event事件回调。然而，挂载阶段还没有开始，$el属性目前不可见

beforeMount
    在挂载开始之前被调用，相关的render函数首次被调用

mounted
    el被新创建的vm.\$el替换，并挂载到实例上去之后调用该钩子。
    如果root实例挂载了一个文档内元素，当mounted被调用时，vm.\$el也在文档内。
    mounted不曾诺所有子组件也一起挂载，可以用vm.$nextTick来替换mounted保证子组件挂载

beforeUpdate
    数据更新时调用，发生在虚拟DOM打补丁之前，适合在更新之前访问现有DOM，比如手动移除已经添加的事件监听器

updated
    由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。
    当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态。如果要相应状态改变，通常最好使用计算属性或 watcher 取而代之。
    注意: updated 不会承诺所有的子组件也都一起被重绘。如果你希望等到整个视图都重绘完毕，可以用 vm.$nextTick 替换掉 updated

beforeDestroy
    实例销毁之前调用。在这一步，实例仍然完全可用

destroyed
    Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

图示：

![vue 生命周期](../img/lifecycle.png)


### vue响应式原理

把一个普通的js对象传给vue实例的data选项，vue会遍历此对象属性，使用Object.defineProperty把这些属性全部转化为getter/setter，这些 getter/setter 对用户来说是不可见的，但是在内部它们让 Vue 追踪依赖，在属性被访问和修改时通知变化

每个组件实例都有相应的 watcher 实例对象，它会在组件渲染的过程中把属性记录为依赖，之后当依赖项的 setter 被调用时，会通知 watcher 重新计算，从而致使它关联的组件得以更新

图示：
![响应式](../img/data.png)





### 双向数据绑定

基于数据劫持的双向数据绑定

利用Object.defineProperty生成的Observer针对对象／对象的属性进行劫持，在属性发生变化后通知订阅者。

解析器Compile解析模板中的Directive（指令），收集指令所依赖的方法和数据，等待数据变化然后进行渲染。

Watcher属于Observer和Compile桥梁，它将接収到的Observer产生的数据变化，并根据Compile提供的指令进行视图渲染，使得数据变化促使视图变化。


Vue 不能监测以下变动

    利用数组索引直接设置一个项
    修改数组长度

### vue-router

全局守卫
    使用 router.beforeEach 注册一个全局前置守卫
    在 2.5.0+ 你可以用 router.beforeResolve 注册一个全局守卫。这和 router.beforeEach 类似，区别是在导航被确认之前，同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被调用
    你也可以注册全局后置钩子router.afterEach，然而和守卫不同的是，这些钩子不会接受 next 函数也不会改变导航本身

路由独享的守卫
    可以在路由配置上直接定义 beforeEnter 守卫

组件内的守卫
    beforeRouteEnter 不能获取组件实例 `this`,因为守卫在导航确认前被调用,因此即将登场的新组件还没被创建。
    beforeRouteUpdate (2.2 新增)
    beforeRouteLeave

完整导航解析

    - 导航被触发。
    - 在失活的组件里调用离开守卫。
    - 调用全局的 beforeEach 守卫。
    - 在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
    - 在路由配置里调用 beforeEnter。
    - 解析异步路由组件。
    - 在被激活的组件里调用 beforeRouteEnter。
    - 调用全局的 beforeResolve 守卫 (2.5+)。
    - 导航被确认。
    - 调用全局的 afterEach 钩子。
    - 触发 DOM 更新。
    - 用创建好的实例调用 beforeRouteEnter 守卫中传给 next 的回调函数。