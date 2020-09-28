## 基本数据类型 
* [String](#String)
* [Boolean](#Boolean)
* [Number](#Number)
* [Array](#Array)
* [Tuple](#Tuple)
* [Enum](#Enum)
* [null和undefined](#nullundefined)
* [Symbol](#Symbol)
* [void](#void)
* [Never](#Never)
* [Any](#Any)

1. <span id="String">**String**</span>  
字符串类型可以用单引号，双引号，也可以用模板字符串
```ts
let name: string = '余笙学长';
let girlFriends: string = "富婆们";
let say: string = `我有一个富婆,她的年龄${60}多`;
```
2. <span id="Boolean">**Boolean**</span>  
同js的Boolean类型,只有true和false
```ts
let isHaveGirlfriend: boolean = false;
```
3. <span id="Number">**Number**</span>  
ts中的数字和js一样，都是浮点数，这些浮点数的类型是number，除了支持十进制和十六进制，还支持二进制和八进制
```ts
let myMoney: number = 5;
// 科学计数法表示
let fupoMoney: number = 1.99714E13;
// 官方栗子🌰
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
// ES6 中的二进制表示法
let binaryLiteral: number = 0b1010;
// ES6 中的八进制表示法
let octalLiteral: number = 0o744;
let notANumber: number = NaN;
let infinityNumber: number = Infinity;
```
4. <span id="Array">**Array**</span>  
数组定义需要声明数组里元素的类型,string只能装字符串,number只能装数字,如果数据结构过于复杂可以用any,但是不建议  
数组有两种定义方法  
* 第一种
```ts
let fupos: string[] = ['翠花', '小芳', '铁蛋'];
```
* 第二种 泛型语法
```ts
let fuposMoney: Array<number> = [17327185421871234, 38912123497910273, 9248913412373657264];
```
5. <span id="Tuple">**Tuple**</span>
元组类似一个数组，但是和数组的区别是，元组有固定的长度和类型(编译后其实就是一个数组)  
    1.  长度固定
    2. 有类型，类型可以不统一
```ts
let fupo01: [string, number] = ['翠花', 59]; // 名字，年龄 
let fupo02: [string, number, boolean] = ['小芳', 62, true]; // 名字，年龄，是否爱我
let fupo03: [number, number] = [64, 9248913412373657264]; // 年龄， 资产
```
6. <span id="Enum">**Enum**</span>  
枚举类型是互相对应的  
```ts
enum Hobbies {
  driveGoodCar,
  drivePlan,
  fupo,
  lolita
}
```
编译后的代码  
```ts
var Hobbies;
(function (Hobbies) {
    Hobbies[Hobbies["driveGoodCar"] = 0] = "driveGoodCar";
    Hobbies[Hobbies["drivePlan"] = 1] = "drivePlan";
    Hobbies[Hobbies["fupo"] = 2] = "fupo";
    Hobbies[Hobbies["lolita"] = 3] = "lolita";
})(Hobbies || (Hobbies = {}));
```
* 常数枚举
```ts
const enum Colors { Red, Yellow, Blue }
console.log(Colors.Red, Colors.Yellow, Colors.Blue)
```
编译后
```ts
console.log(0 /* Red */, 1 /* Yellow */, 2 /* Blue */);
```
使用任意类型的几种情况:  
  1. 使用了第三方库没有类型定义  
  2. 类型转换  
  3. 数据结构太复杂
```ts
// 使用感叹号  ！  进行强行断言，意思是告诉ts，我知道这个的类型你就别操心了
let root: HTMLElement | null = document.getElementById('root')
root!.style.color = 'red'
```


7. <span id="nullundefined">**null和undefined**</span>  
其他类型的子类型
如果想将null和undefined赋值给其他的类型，修改strictNullChecks为false即可
```ts
let str: string = null;
let num: number = undefined;
```
* null  
空
* undefined  
未定义

8. <span id="Symbol">**symbol**</span>  
```ts
const sym = Symbol();
let obj = {
  [sym]: "semlinker",
};

console.log(obj[sym]); // semlinker 
```
9. <span id="void">**void**</span>  
void表示  空的  没有  
下面的代码表示     这个函数没有返回值
```ts
function greeting(name:string):void {
    console.log(name)
}
greeting('余笙学长')
```

11. <span id='Never'>**never**</span>  
永远不 表示永远也不可能出现的值  
其他类型的子类型 不会出现的值
```ts
function createError(msg:string):never {
  console.log(123)
  throw new Error('error')  
  // 出现never表示这个函数永远不能正常结束，在ide中可以看到这句话下面是不可执行的状态的，因为抛出异常导致下面的代码无法继续执行
  console.log(456)
}
```
出现死循环
```ts
function sum():never {
  while (true) {
    console.log('hello');
  }
  console.log('end point');
}
```
12. <span id="Any">**Any**</span>  
ts中所有类型都是any类型的子类型,任何类型都可以被归为any,也就是说any类型十四系统的顶级类型(全局超级类型)  
```ts
let myAny:any = 666;
myAny = '字符串';
myAny = true;
myAny = [1,2,3];
// ...........
```
ts也有一个外号叫anyscript,就是代码中无脑any类型,这违背了ts的初衷,因此,ts3.0引入了unkonw类型

## 类型推论
如果定义变量的时候没有给定类型，ts会猜测这个变量的类型
```ts
let num = 2 // ts猜num为number类型
num = false // 将num再次赋值为boolean类型会报错！

let num1; // 这么写ts就不知道是什么类型了，它会将num1设置为any类型
num1 = 2   // 不报错
num1 = false  // 不报错
num1 = 'haha'  // 不报错
```

## 包装对象
Java中叫装箱和拆箱  
自动在基本类型和对象类型之间切换  
基本类型上没有方法，会在内部完成装箱操作（基本类型→对象类型）
```ts
let str1: string = 'YushengSenior';
let str = str1.toLocaleLowerCase();

// 内部原理

let str11 = new String(str1)
str11.toLocaleLowerCase()
```

## 联合类型
提供可选项，为其中之一

```ts
let a: string | number;  // a的类型可以为string或者number

a = 5   // 当a为number，就可以调用number的方法
a.toFixed()

a = 'hello' // 当a为字符串，就可以调用字符串的方法
a.toString()
```
## 类型断言
不是强制类型转化，智能转换联合类型中存在的
可以用来手动指定一个值的类型
语法：<类型>值
```ts
function person(say:string|number):number { // 返回length为数字类型
  return (<string>say).length
}
```
值as类型
```ts
function person(say:string|number):number {
  return (say as string).length
}
```
## 函数定义
```ts
function hello(name:string):void {
  console.log('helllo'+name)
}
hello('二狗子')
```

### 函数表达式
```ts
// 对函数进行约束
type getUserName = (firstName: string, lastName: string) => string;
// type 类型别名明 = （参数） => 返回值
let uername: getUserName = function (firstName: string, lastName: string): string {
  return firstName + lastName;
}
```
### 可选参数
```ts
// 可选参数

function print(name: string, local?: string, age?: number): void {
  console.log('我是' + name, '我来自' + local, '我已经' + age)
}
// print('二狗子','cn',20)
// print('张三')
```

### 默认参数
```ts
// 默认参数
function ajax(url: string, method: string = 'GET'): any {
  console.log(url, method)
}
// ajax('/user/center')
// ajax('/home', 'POST')
```
### 剩余参数
```ts
function sum(...numbers: Array<number>): number {
  // console.log(numbers);
  // numbers=>[1,2,3,4,5,6,7]
  return numbers.reduce((accu, item) => accu + item, 0)
}

let res = sum(1, 2, 3, 4, 5, 6, 7)
```
### 函数重载
```ts
function sum1(a:number, b:number):void;
function sum1(a:string, b:string):void;
function sum1(a:any, b:any):void {
  console.log(a+b)
}
// sum1(1,2)
// sum1('1','2')
```

## 继承
访问修饰符  
1. public 自己、自己的子类、其他类都可以访问
2. protected 受保护的 自己和自己的子类可以访问 其他类不可以访问
3. private 私有的 只能自己访问，自己的子类、其他类无法访问
