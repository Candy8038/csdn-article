> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.imooc.com](https://www.imooc.com/wiki/vue3zhihu/c2.html)

> 慕课网慕课教程第二章 你好 Typescript： 进入类型的世界涵盖海量编程基础技术教程，以图文图表的形式，把晦涩难懂的编程专业用语，以通俗易懂的方式呈现给用户。

安装 Typescript:

```
npm install -g typescript

```

使用 tsc 全局命令：

```
// 查看 tsc 版本
tsc -v
// 编译 ts 文件
tsc fileName.ts

```

Javascript 类型分类：

原始数据类型 - primitive values：

*   Boolean
*   Null
*   Undefined
*   Number
*   BigInt
*   String
*   Symbol

```
let isDone: boolean = false



let age: number = 10
let binaryNumber: number = 0b1111


let firstName: string = 'viking'
let message: string = `Hello, ${firstName}, age is ${age}`


let u: undefined = undefined
let n: null = null


let num: number = undefined

```

```
let notSure: any = 4
notSure = 'maybe it is a string'
notSure = 'boolean'

notSure.myName

notSure.getName()

```

```
let arrOfNumbers: number[] = [1, 2, 3, 4]


arrOfNumbers.push(3)
arrOfNumbers.push('abc')


let user: [string, number] = ['viking', 20]

user = ['molly', 20, true]

```

Duck Typing 概念：

> 如果某个东西长得像鸭子，像鸭子一样游泳，像鸭子一样嘎嘎叫，那它就可以被看成是一只鸭子。

```
interface Person {
  name: string;
  age: number;
}

let viking: Person ={
  name: 'viking',
  age: 20
}


interface Person {
    name: string;
    age?: number;
}
let viking: Person = {
    name: 'Viking'
}



interface Person {
  readonly id: number;
  name: string;
  age?: number;
}
viking.id = 9527


```

```
function add(x: number, y: number): number {
  return x + y
}

function add(x: number, y: number, z?: number): number {
  if (typeof z === 'number') {
    return x + y + z
  } else {
    return x + y
  }
}


const add2: (x: number, y: number, z?:number) => number = add


const sum = (x: number, y: number) => {
  return x + y
}
interface ISum {
  (x: number, y: number): number
}
const sum2: ISum = sum

```

```
let numberOrString: number | string 

numberOrString.length
numberOrString.toString()

```

```
function getLength(input: string | number): number {
  const str = input as string
  if (str.length) {
    return str.length
  } else {
    const number = input as number
    return number.toString().length
  }
}

```

```
function getLength2(input: string | number): number {
  if (typeof input === 'string') {
    return input.length
  } else {
    return input.toString().length
  }
}

```

面向对象编程的三大特点

*   **封装（Encapsulation）**：将对数据的操作细节隐藏起来，只暴露对外的接口。外界调用端不需要（也不可能）知道细节，就能通过对外提供的接口来访问该对象，
*   **继承（Inheritance）**：子类继承父类，子类除了拥有父类的所有特性外，还有一些更具体的特性。
*   **多态（Polymorphism）**：由继承而产生了相关的不同的类，对同一个方法可以有不同的响应。

```
class Animal {
  name: string;
  constructor(name: string) {
    this.name = name
  }
  run() {
    return `${this.name} is running`
  }
}
const snake = new Animal('lily')


class Dog extends Animal {
  bark() {
    return `${this.name} is barking`
  }
}

const xiaobao = new Dog('xiaobao')
console.log(xiaobao.run())
console.log(xiaobao.bark())


class Cat extends Animal {
  constructor(name) {
    super(name)
    console.log(this.name)
  }
  run() {
    return 'Meow, ' + super.run()
  }
}
const maomao = new Cat('maomao')
console.log(maomao.run())

```

*   **public** 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 public 的
*   **private** 修饰的属性或方法是私有的，不能在声明它的类的外部访问
*   **protected** 修饰的属性或方法是受保护的，它和 private 类似，区别是它在子类中也是允许被访问的

```
interface Radio {
  switchRadio(trigger: boolean): void;
}
class Car implements Radio {
  switchRadio(trigger) {
    return 123
  }
}
class Cellphone implements Radio {
  switchRadio() {
  }
}

interface Battery {
  checkBatteryStatus(): void;
}


class Cellphone implements Radio, Battery {
  switchRadio() {
  }
  checkBatteryStatus() {

  }
}

```

```
enum Direction {
  Up,
  Down,
  Left,
  Right,
}
console.log(Direction.Up)


console.log(Direction[0])


enum Direction {
  Up = 'UP',
  Down = 'DOWN',
  Left = 'LEFT',
  Right = 'RIGHT',
}
const value = 'UP'
if (value === Direction.Up) {
  console.log('go up!')
}

```

泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。

```
function echo(arg) {
  return arg
}
const result = echo(123)


function echo<T>(arg: T): T {
  return arg
}
const result = echo(123)


function swap<T, U>(tuple: [T, U]): [U, T] {
  return [tuple[1], tuple[0]]
}

const result = swap(['string', 123])


```

在函数内部使用泛型变量的时候，由于事先不知道它是哪种类型，所以不能随意的操作它的属性或方法

```
function echoWithArr<T>(arg: T): T {
  console.log(arg.length)
  return arg
}



interface IWithLength {
  length: number;
}
function echoWithLength<T extends IWithLength>(arg: T): T {
  console.log(arg.length)
  return arg
}

echoWithLength('str')
const result3 = echoWithLength({length: 10})
const result4 = echoWithLength([1, 2, 3])

```

```
class Queue {
  private data = [];
  push(item) {
    return this.data.push(item)
  }
  pop() {
    return this.data.shift()
  }
}

const queue = new Queue()
queue.push(1)
queue.push('str')
console.log(queue.pop().toFixed())
console.log(queue.pop().toFixed())



class Queue<T> {
  private data = [];
  push(item: T) {
    return this.data.push(item)
  }
  pop(): T {
    return this.data.shift()
  }
}
const queue = new Queue<number>()


interface KeyPair<T, U> {
  key: T;
  value: U;
}

let kp1: KeyPair<number, string> = { key: 1, value: "str"}
let kp2: KeyPair<string, number> = { key: "str", value: 123}

```

类型别名，就是给类型起一个别名，让它可以更方便的被重用。

```
let sum: (x: number, y: number) => number
const result = sum(1,2)
type PlusType = (x: number, y: number) => number
let sum2: PlusType


type StrOrNumber = string | number
let result2: StrOrNumber = '123'
result2 = 123


type Directions = 'Up' | 'Down' | 'Left' | 'Right'
let toWhere: Directions = 'Up'


```

```
interface IName  {
  name: string
}
type IPerson = IName & { age: number }
let person: IPerson = { name: 'hello', age: 12}

```

```
const a: Array<number> = [1,2,3]

const date: Date = new Date()
const reg = /abc/


Math.pow(2,2)



let body: HTMLElement = document.body

let allLis = document.querySelectorAll('li')


document.addEventListener('click', (e) => {
  e.preventDefault()
})

```

Typescript 还提供了一些功能性，帮助性的类型，这些类型，大家在 js 的世界是看不到的，这些类型叫做 utility types，提供一些简洁明快而且非常方便的功能。

```
interface IPerson {
  name: string
  age: number
}

let viking: IPerson = { name: 'viking', age: 20 }
type IPartial = Partial<IPerson>
let viking2: IPartial = { }



type IOmit = Omit<IPerson, 'name'>
let viking3: IOmit = { age: 20 }


```

**配置示例**

```
{
  "files": ["test.ts", "test2.d.ts"],
  "compilerOptions": {
    "outDir": "./output",
    "module": "ESNext",
    "target":"ES5",
    "declaration": true
  }

```