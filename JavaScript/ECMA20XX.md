# ES2020
1. Promise.allSettled  
2. String.prototype.matchAll  
3. globalThis  
4. 运算符  
  4.1 可选链式调用  
  4.2 空值运算
5. class  
	5.1 静态字段  
    5.2 私有变量
6. 语法规则
  
#### 1.Promise.allSettled与Promise.all
在之前版本中，使用Promise来解决异步的问题再好不过了。但是使用Promise.all有一个副作用，如果传入的其中一个对象失效，会导致其他对象一同失效。  
```
const p1 = new Promise((succ, err) => {
  setTimeout(() => {
    succ();
  }, 1000);
});

const p2 = new Promise((succ, err) => {
  setTimeout(() => {
    err();
  }, 500);
});
```

```
Promise.all([p1, p2])
  .then(function () {
    console.log(arguments)
  })
```
这是Promise.all的写法，显而易见这么写会报错,但是也有解决方法，不会的自行百度这里就不赘述了。  
  
  
##### 使用新特性-Promise.allSettled  
```
Promise.allSettled([p1, p2])
  .then(function () {
    console.log(arguments)
  })
```
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/80fbee3ff93146388280ddd1c180aa9f~tplv-k3u1fbpfcp-zoom-1.image)
可以看到有一个rejected，但并不会影响程序的执行  
支持情况  
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/010dfee75ee34e81a99f5773db4e5aef~tplv-k3u1fbpfcp-zoom-1.image)

#### 2.String.prototype.matchAll与String.prototype.match
match方法可在字符串内检索指定的值,或找到一个或多个正则表达式的匹配。
```
const str = 'hello world'

const reg = /l/

let res = str.match(reg)

console.log(res)
```
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3dba978fb180455ba665d73932d623b9~tplv-k3u1fbpfcp-zoom-1.image)

如果修改匹配规则进行全局匹配的话，返回的信息就比较简单了
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/afb1a701bf0d4eda95458207c5aa1013~tplv-k3u1fbpfcp-zoom-1.image)

##### matchAll用法  
由于其返回的是RegExpStringIterator对象，我们用```for of```处理下可以看到打印出了所有“l”的详细信息![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d06979b47ab64df5b7e187853d6cf693~tplv-k3u1fbpfcp-zoom-1.image)
支持情况![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/307f8d386ff640b1b37b13498bddbe77~tplv-k3u1fbpfcp-zoom-1.image)
#### globalThis
在web中，globalThis = window，在nodejs中，globalThis = global，对于```globalThis```没什么好说的，奉上连接[MDN-globalThis](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/globalThis)  
支持情况
![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/aa1ccd7c1c854588a519b1ca937eba71~tplv-k3u1fbpfcp-zoom-1.image)

#### 运算符
##### 可选链式调用  
```
  const obj = {}
  obj.a // => undefined
  obj.a.b // => Cannot read property 'b' of undefined
```
使用可选链式调用后
```
  const obj = {}
  obj.a // => undefined
  obj.a?.b // => 不会报错  如果前方数据为undefined或null 会终止程序的执行 
```
**⚠️    慎重使用，不利于我们查找错误  **  
支持情况
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/38d02106d99b42fbb6696d878bad21c9~tplv-k3u1fbpfcp-zoom-1.image)

##### 空值运算
假设有这么一个场景，我写了一个function，带有一个默认参数，如果传参，则返回参数，如果无参，则返回默认值 
```
function foo(a) {
  return a || 'hello world'
}
foo(123)	  // => 123
foo(0)		 // => 'hello world'
foo("")		 // => 'hello world'
foo(false)  // => 'hello world'
```
但是这么写会有个弊端，如果我们输入一些数字或字符串会正常返回，但是传入了像```0、false、""```这样的这个function就不太聪明了

使用空值运算来改写一下这个function
```
function foo(a) {
  return a ?? 'hello world'
}
```
尝试下上面的情况
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/64bb232b631846c081932b165feeca5c~tplv-k3u1fbpfcp-zoom-1.image)
注意⚠️，空值运算不支持```undefined、null```  
支持情况
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/585b29f1a6c34f3d9a91fbdfd69b1dfb~tplv-k3u1fbpfcp-zoom-1.image)
#### class
##### 静态字段  
静态字段这个我感觉没啥好说的[MDN-Static](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes/static)
##### 私有变量
在变量或函数前面添加一个符号#，可以将它们设为私有属性，只在类内部可用
```
class Foo {
  #num = 857
  init() {
    console.log(this.#num);
  }
}
new Foo().init() // => 857

let Poo = new Foo()
Poo.#num // => Uncaught SyntaxError: Private field '#num' must be declared in an enclosing class
```
#### 6.语法规则
##### 动态引入
```
import xxx from 'xx' 
xxx.yyy  // 之前的写法

import(xxx).then(function () {

})  // 全新的写法

// 举个栗子🌰
let sss = await import (xxx);
```
##### 抛异常 (非常遗憾现在市面上的浏览器还不支持这种语法，看看就得了)
```
let foo = (options = throw TypeError()) => {

}
```
##### tyr catch
```
try {
  
} catch () { // ES2020支持catch不写参数，但是不建议使用，因为会忽略掉错误信息
  
}
```


