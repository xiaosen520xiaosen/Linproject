回顾
```
v-bind:属性="值"  :属性
v-on:事件  简写 @事件  stop prevent capture self once
v-if  v-else-if v-else
v-for
:class
```

# mvvm原理
总结：
  利用数据劫持 结合发布订阅模式 做数据双向绑定

## 数据劫持

## mvvm原理  vue2
```js
const vm = new Vue({
  data: {
    msg: 'hello'
  },
  methods: {},
})
vm.$mount('#app')
```


原理：
  new Vue时，传入 data对象存储数据， Vue会立即遍历data,利用Object.defineProperty将所有属性 都劫持，转换成 getter和setter,每个实例创建 观察者实例(watcher  依赖或监听就是 data中的数据)，当setter触发，通知 watcher,触发回调，回调中 调用  触发render函数， render生成新的虚拟dom，不会立即操作真实dom,而是将新的虚拟dom和上一次保存在内存中的虚拟dom进行比较（diff算法），得到代价最小方案 更新真实dom


## vue3原理
  创建vue实例完成后，使用Proxy直接代理整个实例，数据作为实例属性，全部被劫持，转换成 getter和setter,每个实例创建 观察者实例(watcher  依赖或监听就是 data中的数据)，当setter触发，通知 watcher,触发回调，回调中 调用  触发render函数， render生成新的虚拟dom，不会立即操作真实dom,而是将新的虚拟dom和上一次保存在内存中的虚拟dom进行比较（diff算法），得到代价最小方案 更新真实dom


## 虚拟dom

概念：
  利用js对象语法结构 描述 真实 dom 树形结构
```html
<div id="box" class="container">
  <p class="op">这是p</p>
  <span>这是span</span>
  文本内容
</div>

```
指定使用js对象的语法 描述 上述 这个div标签

```js
const box = {
  tag: 'div',
  attrs: {
    id: 'box',
    class: 'container'
  },
  children: [
      {
        tag: 'p',
        attrs: {
          class: 'op'
        },
        children: ['这是p']
      },
      {
        tag:'span',
        children: ['这是span']
      },
      "文本内容"
  ]
}
```
vue为什么定义虚拟dom
场景：
  视图中 使用 v-for 循环 一个数组 （10个值），循环10个Li,  修改了数组  视图更新几次dom
答案是 10次dom 
严重问题：
  dom重新构建 耗性能 
怎么解决？
  首次渲染，先生成虚拟dom,虚拟dom节点，渲染成真实dom，节点 插入到页面上，  
  数据更新时，重新渲染虚拟dom,  将更新后路最新的虚拟dom和 上次保存在内存中虚拟dom,进行比较， 得到代价最小的方案 更新真实dom

虚拟dom 怎么比较？
diff算法： 优化虚拟dom 比较效率
  1 同层比较
  2 同层如果有key属性  则会同级同key比较

## 循环中 为什么要加key属性 （v-for循环一定要加key 且key是独一无二）
key属性：
  当前 元素 独一无二的 标记

### 循环中 为什么使用id做key和 下标做key的区别？
每次循环 下标会重新生成（变化）
每次循环  id不会重新生成（不变化）