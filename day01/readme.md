# vue 是什么
js构建用户界面 渐进式框架
特点：
  1 声明式开发  响应式：vue是响应式的，当数据发生变化时，视图会自动更新
     （命令式、声明式）
  2  数据驱动视图 （vue开发，不要想着去操作dom, 而是 修改状态，然后vue会自动更新视图）
  组件化：组件化是vue的核心思想，组件化的好处是可以将复杂的页面拆分成小的组件，然后通过组合这些组件来构建复杂的页面
  3 组件化开发 （自定义标签）

  
# vue 应用实例
mvvm （数据和视图绑定是双向的）
m 数据

v 视图

m数据改变 v视图刷新 视图view 操作 数据也会自动改变 


## mvc
传统 应用程序设计模式  以后端为主导
m model 模型 数据层
v view 视图 （前端网页）
c controller 控制器 （处理用户交互）

## 应用实例创建
```js
// Vue  application   b/s  c/s
    const { createApp } = Vue;
    // 创建应用实例 传入 配置对象 config
    const app = createApp({
      // data存储 实例的 数据  mvvm中的m   数据是直接挂载到 实例上
      data(){
        return {
          msg: 'hello world'
        }
      },
      // 实例的一个方法 会在 实例初始化完成后 自动触发
      mounted(){
        console.log(this, 'aaa');
      },
      // 存储实例方法  方法也是 直接挂载到 实例对象上
      methods: {
        changeMsg(){
            this.msg = '值改变了';
        }
      }
    })
    // 应用实例 挂载（渲染）到 视图 #app上 #app元素内部受了 app控制
    app.mount("#app")
```

# 模板 插值 {{}}
vue用于在视图上 渲染（输出） 字符串数据
特点：
 1 js环境
 2 变量函数 去实例上查找 
 3 用来在 模板输出 所以只支持 表达式  （式子  结果是 一个值   比如 运算符  变量  函数  字符串  数组  对象  短路 三目 等）
 4 js内置的全局变量 (白名单内部的 js全局变量可以使用)

```
  'Infinity,undefined,NaN,isFinite,isNaN,parseFloat,parseInt,decodeURI,' +
  'decodeURIComponent,encodeURI,encodeURIComponent,Math,Number,Date,Array,' +
  'Object,Boolean,String,RegExp,Map,Set,JSON,Intl,BigInt,console'
```
总结：
  1 本身自己就是一个值
  2 方法 调用后有返回值


# 指令  directive 
功能：
  将标签 某种状态 绑定到 实例某个数据上  （中间层 打通视图和 数据）
语法： （设计成了 html标签自定义属性语法）
注意：
  指令 值中（引号） 环境是js环境 (和 模板中环境一致)  ******** 
  ```html
    <标签 v-指令名="值"></标签>
  ```
vue设计了很多的指令 用来绑定 标签 各种状态

## 常用指令
+ v-text (不重要) 可以使用模板取代
+ v-html  输出html 编译富文本数据
+ v-show 将元素显示 隐藏状态 和 布尔值（true 显示 false 元素隐藏）
+ v-model 将 表单控件的value属性和 值进行双向绑定
  表单修饰符 （输入框举例）
  <标签 v-model.修饰符="值"/>
  .lazy  将双向绑定事件 由input触发  变成change触发
  .trim 去除 value开头结尾空格 再绑定给 变量
  .number 将input value 解析成number 赋值给变量 （规则同parseFloat 注意首字符就是非数字 变成普通v-model不再解析成number）
