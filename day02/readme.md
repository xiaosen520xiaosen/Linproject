
## v-bind:属性  指令  v-bind指令
功能： 将 标签普通属性 和 动态值绑定（编译后还是 原来属性）
```html
<标签 v-bind:属性="值"></标签>
<标签 v-bind:id="a"></标签>
<!-- 编译后  id属性 -->
```

简写：
  <标签 :属性="值"></标签>
  <标签 :id="a"></标签>

## v-on:事件  指令  v-on指令
功能： 事件绑定（编译后还是 原来事件）
为什么 要 v-on绑定事件：
    v-on作为 vue指令，绑定函数，找 实例方法

```html
<标签 v-on:事件="方法"></标签>
<标签 v-on:click="方法"></标签>

<!-- 简写 -->
<标签 @事件="方法"></标签>
```




语法：

### 直接在行内 修改数据 （特别不推荐）
```html
 <button v-on:click="msg='值改变了'">按钮</button>
```
### v-on绑定方法
+ 不加括号绑定  （方法 作为事件函数存在）
```html
<button @click="handleClick">按钮</button>
```
```js
const app = createApp({
  methods： {
    handleClick(){}
  }
})
```

+ 加括号绑定  本质上 不再是事件函数 事件发生的时候调用而已
```html
<button @click="handleClick2('这是我传入的值')">按钮2</button>
<!-- 编译时
  <button @click="() => {
    handleClick2('这是我传入的值')
  }">按钮2</button>
-->
```

```js
          handleClick2(msg2){
            this.msg2 = msg2;
          }
```

## 获取事件源
```js
e.stopPropagation()
e.prevent()
e.target
```

如何获取
+ 绑定时  不加括号
  事件函数的第一个参数就是事件对象
+ 绑定时 加括号
  vue定义 全局变量 $event事件触发时 作为实参传入即可
```html
<button @click="fn(1, $event)">按钮</button>
```

## 事件修饰符
语法：
```html
<标签 @事件.修饰符="方法"></标签>
```

.stop  取消冒泡行为
.prevent  阻止默认事件
.self   自己触发  （后代无法冒泡触发）
.capture   捕获阶段提前触发
.once  只触发一次

# 条件渲染
+ 单分支
v-if
显示效果同 v-show 绑定布尔值 true 元素显示 false 元素隐藏

不同点：
  false时， v-show控制 display: none 元素隐藏， v-if直接从节点移除该元素

使用场景：
  v-show具有更高切换性能，适用于 切换频率高的场景 如 tab选项卡
  v-if 具有更高的 初始加载性能 （false）， 比如 侧边弹出层
+ v-else 结合 v-if一起使用
注意：  v-else绑定元素必须是 v-if绑定元素的下一个兄弟

+ v-else-if
```html
<div class="box" v-if="activeIndex%3===0">11</div>
      <div class="box2" v-else-if="activeIndex%3===1">22</div>
      <div class="box3" v-else>33</div>
```
v-if v-else-if v-else必须是连续的兄弟 中间不能包含其他标签，


## 面试题
v-if和v-show异同

# 列表渲染 v-for
+ 基础循环
```html
      <!-- 
        基础循环，声明循环值的变量   
        注意：循环时声明的变量 作用域 只能在 v-for指令 绑定元素的内部使用
       -->
      <ul>
        <li v-for="item in arr">
            {{item}}
        </li>
      </ul>
```
+ 声明循环下标
```html
      <ul>
        <li v-for="(item, index) in arr">

          {{item}}
            -->
          {{index}}
        </li>
       </ul>
```

注意：
  复杂循环，循环嵌套循环，如果在内层循环中 要使用外层循环声明的变量 内外层变量不能重名

# vue对于v-bind:class扩展
+ 扩展 绑定数组  数组每个值 编译成 一个类
```html
<div :class="[a, 'box2']">11</div>
<!-- <div class="box box2"></div> -->
```
```js
      const app = createApp({
        data() {
          return {
            a: 'box',
            isActive: true
          };
        },
      });
```
+ 绑定对象
 将对象每个属性编译成一个类，是否有这个类取决于 属性值是否为 true
```html
<div :class="{a: true, b: 1===1, active: isActive, c: 1===3}">22</div>
<!-- <div class="a b active"></div> -->
```
```js
      const app = createApp({
        data() {
          return {
            a: 'box',
            isActive: true
          };
        },
      });
```

+ 将对象 嵌套在数组中
将数组中每个值，和对象每个属性都编译成一个类（属性要为true）
```html
<div :class="[a, 'box2', {a: true, b: 1===1, active: isActive, c: 1===3}]">33</div>
<!-- 
  <div class="box box2 a b active"></div>
 -->
```
```js
 const app = createApp({
        data() {
          return {
            a: 'box',
            isActive: true
          };
        },
      });
```
# v-bind:style 扩展
绑定对象
扩展了内联样式 style语法，可以绑定表达式
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
        <input type="number" v-model="wd">
        <div :style="{
          width: wd+'px',
          height: 100+400+'px',
          backgroundColor: 'red'
        }"></div>
    </div>
    <script src="./vue.global.js"></script>
    <script>
      const { createApp } = Vue;
      const app = createApp({
        data() {
          return {
            wd:  100
          };
        },
      });
      app.mount("#app");
    </script>
  </body>
</html>

```
