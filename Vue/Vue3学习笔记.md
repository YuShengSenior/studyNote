[vue3官网](https://v3.vuejs.org/)
[vue3编译器](https://vue-next-template-explorer.netlify.app/)  
[vue-cli](https://cli.vuejs.org/)

diff算法优化  
  * vue2中是全量比较
  * vue3中是静态标记（PatchFlag）  
    静态标记会创建一个flag，diff算法对比时只会对比有标记的，无标记的会忽略掉
    dom结构
  ```html
  <div>
    <p>'你好'</p>
    <p>'你好'</p>
    <p>'你好'</p>
    <p>{{msg}}</p>
  </div>
  ```
  编译后  
  ```ts
  import { createVNode as _createVNode, toDisplayString as _toDisplayString, openBlock as _openBlock, createBlock as _createBlock } from "vue"

export function render(_ctx, _cache, $props, $setup, $data, $options) {
  return (_openBlock(), _createBlock("div", null, [
    _createVNode("p", null, "'你好'"),
    _createVNode("p", null, "'你好'"),
    _createVNode("p", null, "'你好'"),
    _createVNode("p", null, _toDisplayString(_ctx.msg), 1 /* TEXT */)
  ]))
}
  ```
  * 静态提升（hoistStatic）  
  把不需要更新的元素放到外面只创建一次，使用的时候去复用  
提升前
```js
export function render(_ctx, _cache, $props, $setup, $data, $options) {
  return (_openBlock(), _createBlock("div", null, [
    _createVNode("p", null, "'你好'"),
    _createVNode("p", null, "'你好'"),
    _createVNode("p", null, "'你好'"),
    _createVNode("p", null, _toDisplayString(_ctx.msg), 1 /* TEXT */)
  ]))
}
```
提升后
```js
const _hoisted_1 = /*#__PURE__*/_createVNode("p", null, "'你好'", -1 /* HOISTED */)
const _hoisted_2 = /*#__PURE__*/_createVNode("p", null, "'你好'", -1 /* HOISTED */)
const _hoisted_3 = /*#__PURE__*/_createVNode("p", null, "'你好'", -1 /* HOISTED */)

export function render(_ctx, _cache, $props, $setup, $data, $options) {
  return (_openBlock(), _createBlock("div", null, [
    _hoisted_1,
    _hoisted_2,
    _hoisted_3,
    _createVNode("p", null, _toDisplayString(_ctx.msg), 1 /* TEXT */)
  ]))
}
```
  * 时间侦听器缓存(cacheHandlers) 
  


ref只能监听简单类型的数据  
reactive 可以监听复杂类型的数据 例如：数组、对象  
组合api  
composition api 和option api  
composition api本质  注入  
setup执行时机  
组件的  
* setup  
* beforeCreate 组件刚刚被创建出来，组建的data和methods还没初始化好 
* Created 组件刚刚被创建出来，data和methods已经初始化好

setup注意点  
1. 执行setup的时候还没有执行created生命周期，在setup函数中不能使用data和methods
2. 不能在setup不能使用data和methods，为了避免错误使用，setup中的this修改成undefined
3. setup函数只能是同步的，不能是异步的

# reactive

* Vue3中提供的实现响应式数据的方法  
vue2.x是通过defineProperty实现  
vue3通过ES6的proxy实现的  
reactive参数必须是对象（json/arr） 
如果传入了其他对象，视图不会更新，需要重新赋值  
# ref
 本质：reactive  
 传递一个值之后，ref底层会自动将ref转换成reaactive  
 ref(18) => reactive({value:18})  
 如果通过ref创建数据，使用的时候不用通过.value获取数据

 template如何判断是否为ref数据？  
 通过__v_ref来判断  
 如果这个私有的属性，并且取值为true  
 isRef、isReactive =>boolean

ref，reactive都是递归监听的，数据量大会很耗性能

# 单层监听
* shallowReactive
* shallowRef
* triggerRef
    shallowRef创建的数据监听.value的变化
    